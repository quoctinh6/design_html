# 03. KIẾN TRÚC HOẠT ẢNH (`imskyleen/animate-ui`), CHỐNG VĂNG CHỮ & THANG `Z-INDEX` (`03_ANIMATION_AND_ZINDEX.md`)

Tài liệu này hướng dẫn cách sử dụng bộ skill **[`imskyleen/animate-ui`](https://github.com/imskyleen/animate-ui)** (`npx skills add https://github.com/imskyleen/animate-ui`) để thiết kế chuyển động tinh tế, kiểm soát phạm vi ảnh hưởng của từng hoạt ảnh, chống hiện tượng chữ bị văng/dồn khi phần tử co giãn, và chuẩn hóa `z-index` toàn trang.

---

## 1. Hệ Thống Motion Tokens Chuẩn (`imskyleen/animate-ui`)

Mọi dự án khai báo bộ token chuyển động thống nhất trong `:root` của `css/styles.css`:

```css
:root {
  /* Đường cong gia tốc (Easing Curves) */
  --ease-out: cubic-bezier(0.22, 1, 0.36, 1);         /* Xuất hiện / Phản hồi nhanh, dừng êm */
  --ease-in-out: cubic-bezier(0.65, 0, 0.35, 1);      /* Co giãn khung / Di chuyển phần tử trên màn hình */
  --ease-drawer: cubic-bezier(0.32, 0.72, 0, 1);      /* Mở ngăn kéo / Popup trượt kiểu iOS */
  --ease-enter: cubic-bezier(0.22, 1, 0.36, 1);       /* Nội dung mới trượt vào khi đổi tab (260ms) */
  --ease-leave: cubic-bezier(0.4, 0, 1, 1);           /* Nội dung cũ mờ đi trước khi đổi DOM (130ms) */

  /* Thời lượng & Quy mô biến đổi (Duration, Scale, Blur, Shift) */
  --dur-press: 120ms;
  --dur-fast: 220ms;
  --dur-med: 520ms;
  --scale-press: 0.97;
  --scale-choice: 0.985;
  --blur-icon: 2px;
  --blur-panel: 5px;
  --shift-tabs: 12px;
}
```

---

## 2. Quy Tắc Bắt Buộc: Chống Văng & Dồn Chữ Khi Khung Thay Đổi Kích Thước (`Anti-Text-Scattering Law`)

Một trong những lỗi phổ biến nhất trên web là khi một phần tử đang chạy hoạt ảnh thay đổi chiều rộng (`width` / `flex-basis` / `grid-template-columns`), đoạn văn bản bên trong bị trình duyệt tính toán lại dòng liên tục ở từng khung hình (`reflow per frame`), làm chữ bị dồn cục, nhảy dòng hoặc văng ra ngoài khung.

**Để triệt tiêu hoàn toàn hiện tượng này, mọi Agent bắt buộc áp dụng 4 kỹ thuật sau:**

### 2.1. Kỹ thuật "Ẩn Tức Thì -> Đợi Khung Mở Xong -> Hiện Chữ Mượt" (Dành cho Accordion Ngang / Thẻ Co Giãn)
- Khi người dùng bấm vào một thẻ co giãn (ví dụ `.spine-card` hoặc панель ngang):
  1. **Bước 1 (`0ms` — Tức thì):** Ẩn ngay lập tức toàn bộ khối chữ của thẻ vừa đóng và thẻ chuẩn bị mở (`opacity: 0; visibility: hidden; transition: none;`).
  2. **Bước 2 (`0ms -> 520ms`):** Khung thẻ thực hiện hoạt ảnh co giãn chiều rộng (`transition: flex 520ms var(--ease-in-out)`). Trong suốt thời gian này, khối chữ bên trong được khóa `min-width` cố định (ví dụ `min-width: 320px`) và đang ẩn nên không bao giờ bị ép dòng.
  3. **Bước 3 (`Sau 530ms` — Khi khung đã dừng hẳn):** Bật class `.is-text-ready` để chữ hiện ra êm ái (`opacity: 0 -> 1`, `translateY(6px -> 0)` trong `240ms`).

### 2.2. Kỹ thuật Khóa Chiều Rộng Hộp Chú Thích (`width: max-content`) Trên Dải Ảnh Co Giãn
- Đối với các dải ảnh co giãn tỷ lệ `flex` (như `.showcase-band` có nhiều tấm ảnh co giãn qua lại):
  - Hộp chữ đè trên ảnh **KHÔNG BAO GIỜ** được đặt `left: 20px; right: 20px;` (vì khi tấm ảnh thu hẹp, khoảng cách giữa `left` và `right` nhỏ lại sẽ bóp nát dòng chữ).
  - **Giải pháp chuẩn:** Đặt `left: 20px; right: auto; width: max-content; max-width: calc(100% - 40px);` và ẩn hộp chữ tức thì khi tấm ảnh không còn ở trạng thái `.is-active`.

### 2.3. Kỹ thuật Hoán Đổi Nội Dung Hai Pha (`Two-Phase Blur & Shift Swap`) Cho Bảng Kết Quả / Bộ Lọc
- Khi người dùng bấm nút lọc làm thay đổi nội dung một bảng kết quả:
  - **Pha Thoát (`Leave` — `130ms`):** Đưa bảng kết quả về `opacity: 0`, `filter: blur(5px)`, `transform: translate3d(-12px, 0, 0) scale(0.985)`.
  - **Pha Cập Nhật DOM (`Tại mốc 130ms`):** Thay đổi nội dung text (`textContent`) khi phần tử đang ẩn hoàn toàn (`opacity: 0`), nhờ đó người dùng không thấy chữ giật cục.
  - **Pha Xuất Hiện (`Enter` — `260ms`):** Đưa bảng kết quả từ `translate3d(+12px, 0, 0)` về `translate3d(0, 0, 0) scale(1)` và `filter: blur(0px)`.

### 2.4. Không Lạm Dụng Scroll-Reveal Lên Văn Bản Đọc (`No Gratuitous Text Entrance`)
- Theo quy tắc `animate-ui`: Không gắn hiệu ứng trượt/mờ khi cuộn trang (`scroll-reveal`) lên từng đoạn văn `<p>`, tiêu đề `h2`, `h3` hay các ô thông tin tĩnh, vừa tránh cảm giác trang tải chậm vừa giữ văn bản luôn sắc nét ngay khi cuộn tới.

---

## 3. Mẫu Bảng Thống Kê Phạm Vi Ảnh Hưởng Của Hoạt Ảnh (`ANIMATION.md`)

Trong mỗi dự án, Agent cần lập file `ANIMATION.md` thống kê rõ từng hoạt ảnh theo cấu trúc 4 cột sau:

| Tên Hoạt Ảnh & Thành Phần | Thông Số Kỹ Thuật (`Duration / Easing / Scale`) | Phần Tử Bị Ảnh Hưởng Bên Trong (`Internal Children`) | Phần Tử Bị Ảnh Hưởng Bên Ngoài (`External Siblings / Layout`) |
| :--- | :--- | :--- | :--- |
| **1. Hoán đổi bảng kết quả bộ lọc** | Leave `130ms` (`blur(5px)`, `-12px`) -> Enter `260ms` (`+12px -> 0`) | Tiêu đề kết quả, mô tả, các ô thông số và nút CTA bên trong bảng | Không đẩy chiều cao các section bên dưới nhờ giữ khung ổn định |
| **2. Mở rộng thẻ hồ sơ ngang (`.spine-card`)** | Khung `flex` `520ms var(--ease-in-out)`; Chữ đợi `530ms` mới hiện `240ms` | Nhãn dọc ẩn đi; nội dung chi tiết đợi khung mở xong mới hiện (`min-width: 320px`) | Các thẻ anh em trong cùng `.spine-deck` thu hẹp đồng bộ; không ảnh hưởng section ngoài |
| **3. Dải ảnh tiêu điểm (`.showcase-panel`)** | Khung `flex` `420ms var(--ease-out)`; Chữ đợi `420ms` mới hiện `220ms` | Ảnh `object-fit: cover`; hộp chú thích khóa `width: max-content` | Các tấm ảnh anh em trong dải `.showcase-band` co giãn tỷ lệ đối ứng |
| **4. Ngăn kéo bộ lọc di động & Menu Mobile** | Mở `320ms var(--ease-drawer)`; Đóng `200ms` | Các nút chọn và thanh tiêu đề đóng/mở bên trong popup | Lớp nền mờ `.backdrop` che phủ phía sau; khóa cuộn nền bằng `overscroll-behavior: contain` |
| **5. Popup phóng to ảnh (`Fancybox v5 Local`)** | Panzoom gốc (`contentClick: 'toggleMax'`, `wheel: 'zoom'`, `maxScale: 3`); Nút `:active` `scale(0.95)` | `.fancybox__content` và `.fancybox-image` biến đổi mượt qua bộ máy Panzoom; các nút `.f-button` đảo cực màu khi hover | Tạm dừng các bộ đếm tự động chuyển ảnh nền khi `html.with-fancybox` đang bật |

---

## 4. Thang `z-index` 9 Tầng Thống Nhất Toàn Hệ Thống

Để đảm bảo không bao giờ xảy ra lỗi phần tử này đè lên phần tử kia khi cuộn trang hoặc mở popup, toàn bộ dự án phải sử dụng đúng 9 biến `--z-*` sau trong `:root`:

```css
:root {
  --z-bg-media: 1;          /* Tầng 1: Ảnh/video nền trong Hero hoặc dải trang trí */
  --z-bg-scrim: 2;          /* Tầng 2: Lớp phủ tối (Scrim) trên ảnh nền & dải nền Footer */
  --z-card-overlay: 5;      /* Tầng 3: Hộp chữ trên Hero, nhãn chú thích trên ảnh, thẻ nổi nội bộ */
  --z-floating-dock: 80;    /* Tầng 4: Nút công cụ nổi bên lề màn hình di động (nằm dưới Header) */
  --z-site-header: 90;      /* Tầng 5: Thanh điều hướng cố định đầu trang (Sticky Header) */
  --z-drawer-backdrop: 110; /* Tầng 6: Lớp nền mờ của Popup / Drawer công cụ (che trên Header) */
  --z-drawer-panel: 120;    /* Tầng 7: Khung nội dung Popup / Drawer công cụ trên di động */
  --z-mobile-nav: 200;      /* Tầng 8: Menu điều hướng toàn màn hình trên di động */
  --z-lightbox: 1000;       /* Tầng 9: Hộp đèn xem ảnh phóng to toàn màn hình (Fancybox v5 Local .fancybox__container) */
}
```
