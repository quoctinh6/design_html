# 02. KHUNG THIẾT KẾ MỞ & HỆ THỐNG TOKEN MÀU ĐỘNG (`02_DESIGN_SYSTEM_TEMPLATE.md`)

Tài liệu này hướng dẫn các Agent cách xây dựng `DESIGN.md` cho một dự án mới bằng bộ skill **[`taste-skill`](https://github.com/Leonxlnx/taste-skill)** và **[`impeccable`](https://github.com/pbakaus/impeccable)** **mà không bị rập khuôn theo màu sắc hay phong cách của các dự án cũ**.

---

## 1. Cách Gợi Mở Bảng Màu Mới Cho Từng Dự Án (Không Theo Lối Mòn)

Thay vì dùng lại một bộ mã màu cố định, Agent phải chọn **Hướng cảm xúc màu sắc (`Palette Mood`)** dựa trên lĩnh vực của khách hàng, sau đó ánh xạ vào **8 Token Ngữ Nghĩa (`Semantic Tokens`)**:

### 1.1. Gợi Ý Các Hướng Bảng Màu Mở (Để Agent Tự Do Sáng Tạo)
- **Hướng A — Editorial Warmth (Ấn phẩm biên tập / Văn hóa / Kiến trúc / Giáo dục):**
  - Nền giấy ấm (Alabaster / Warm Ivory / Parchment), bề mặt phụ màu đá trầm hoặc cát nhạt, chữ màu than chì sâu, điểm nhấn màu đất nung (Terracotta), đỏ son (Vermilion) hoặc vàng hổ phách.
- **Hướng B — Botanical & Organic Craft (Ẩm thực / Nông nghiệp xanh / Bất động sản nghỉ dưỡng / Sức khỏe):**
  - Nền kem lanh (Linen / Oat), bề mặt phụ màu xô thơm nhạt (Soft Sage / Clay), khối nhấn màu xanh rừng sâu (Deep Forest / Cypress) và điểm nhấn đồng ấm (Warm Ochre / Copper).
- **Hướng C — High-Precision Studio (Công nghệ / SaaS / Kỹ thuật / Thiết kế công nghiệp):**
  - Nền xám thạch anh sáng (Cool Alabaster / Zinc Light) hoặc chế độ tối (Obsidian / Carbon), cấu trúc phân tách bằng bước chuyển sắc độ lạnh, điểm nhấn xanh cô-ban, cam tín hiệu (Signal Orange) hoặc xanh lục bảo sắc nét.
- **Hướng D — Monochrome Luxury & Accent (Thời trang / Nghệ thuật / Nội thất cao cấp / Studio sáng tạo):**
  - Hệ sắc độ đơn sắc giàu chiều sâu (95% diện tích là sự đối lập giữa nền ngà/trắng khói và mực đen ấm), chỉ dùng duy nhất `< 5%` diện tích cho 1 màu điểm nhấn (Burgundy, Champagne Gold, hoặc Klein Blue).

### 1.2. Kiến Trúc 8 Token Màu Ngữ Nghĩa Bắt Buộc Trong `:root`
Dù chọn bất kỳ bảng màu nào ở trên, Agent luôn chuẩn hóa vào hệ biến CSS sau để dễ dàng phối hợp giữa các Subagent:

```css
:root {
  /* 1. Nhóm Nền Sáng Chủ Đạo (Chiếm 70%–80% diện tích toàn trang) */
  --bg: /* Mã màu nền chính của dự án */;
  --surface: /* Mã màu bề mặt phụ (lệch 4%–8% độ sáng so với --bg để phân tách khối không cần viền 1px) */;

  /* 2. Nhóm Văn Bản Trên Nền Sáng */
  --ink: /* Màu chữ chính (Độ tương phản >= 10:1 trên --bg) */;
  --muted: /* Màu chữ phụ / chú thích (Độ tương phản >= 4.8:1 trên --bg) */;

  /* 3. Nhóm Khối Nền Đậm (Tạo nhịp điệu thị giác gián đoạn giữa các chương) */
  --field-primary: /* Màu khối thương hiệu chủ đạo */;
  --field-primary-deep: /* Sắc độ trầm hơn của --field-primary (dùng cho thẻ xếp lớp / accordion) */;
  --field-contrast: /* Màu khối đối trọng phụ (dùng cho ô số liệu hoặc dải trích dẫn) */;
  --field-ink: /* Màu khối tối sâu dùng cho Footer hoặc vùng CTA cuối trang */;

  /* 4. Nhóm Điểm Nhấn Tương Tác (< 5% diện tích màn hình) */
  --accent: /* Màu điểm nhấn dành cho nút CTA chính, trạng thái Hover & Focus */;
  --accent-ink: /* Màu chữ đặt trên nền --accent (Bắt buộc đạt tương phản >= 7:1) */;
}
```

---

## 2. Quy Tắc Thiết Kế Trạng Thái Nút Bấm (`Polarity-Swap Button Matrix`)

Mọi dự án phải định nghĩa rõ 4 trạng thái (`Idle`, `Hover`, `Active`, `Focus-Visible`) cho từng nhóm nút bấm trước khi code HTML:

1. **Nguyên tắc Đảo Cực Màu (`Polarity Swap`) khi Hover:**
   - Khi di chuột vào nút (`.btn:hover`), không chỉ làm mờ (`opacity: 0.8`) mà phải **đảo cặp màu nền – màu chữ** sang màu điểm nhấn (`--accent` / `--field-primary`) kết hợp dịch chuyển nhẹ `transform: translateY(-2px)` (chỉ kích hoạt trong `@media (hover: hover) and (pointer: fine)`).
2. **Nguyên tắc Phản Hồi Vật Lý khi Nhấn (`:active`):**
   - Khi nhấn chuột hoặc chạm tay (`.btn:active`), nút thu nhẹ về `transform: translateY(0) scale(0.97)` trong `120ms` để tạo cảm giác phím bấm vật lý.
3. **Nguyên tắc Phân Tách Không Dùng Viền 1px (`No-Line Separation`):**
   - Phân biệt nút bấm, thẻ nội dung và các ô bộ lọc bằng **sự chênh lệch tông nền** (`--surface` trên `--bg`, hoặc `--bg` trên `--surface`) thay vì lạm dụng `border: 1px solid` hay `box-shadow` mờ nhạt.

| Loại Nút / Tương Tác | Trạng Thái Nghỉ (`Idle`) | Trạng Thái Di Chuột (`:hover` trên PC) | Trạng Thái Nhấn (`:active`) | Trạng Thái Bàn Phím (`:focus-visible`) |
| :--- | :--- | :--- | :--- | :--- |
| **1. Nút Hành Động Chính (`.btn-primary`)** | Nền `--field-primary` · Chữ `--bg` | Nền `--accent` · Chữ `--accent-ink` · `translateY(-2px)` | `scale(0.97)` | `outline: 2px solid var(--field-primary)` |
| **2. Nút Chuyển Đổi Nổi Bật (`.btn-accent`)** | Nền `--accent` · Chữ `--accent-ink` | Nền `--bg` hoặc `--ink` · Chữ tương phản · `translateY(-2px)` | `scale(0.97)` | `outline: 2px solid var(--accent)` |
| **3. Nút Lọc / Tab Chọn (`.btn-secondary` / `[aria-pressed]`)** | Nền `--surface` · Chữ `--ink` | Nền `--ink` · Chữ `--accent` · `scale(0.985)` khi chọn | `scale(0.97)` | `outline: 2px solid var(--field-primary)` |

---

## 3. Ma Trận Nền Đặt Tiêu Đề `h1` & `h2` (`Surface Field Matrix`)

Để trang web có chiều sâu biên tập và không bị đơn điệu, `DESIGN.md` của mỗi dự án cần quy định rõ các loại nền hợp lệ nơi thẻ `h1` và `h2` được phép xuất hiện:

- **Đối với `h1` (Tiêu đề mở màn trang):**
  1. **Trên ảnh/video mở màn có lớp phủ tối cục bộ (`Scrim`):** Đảm bảo độ tương phản chữ `h1` đạt tối thiểu `10:1`.
  2. **Trên khối nền thương hiệu đặc (`--field-primary` hoặc `--bg`):** Dành cho các trang con cần sự rõ ràng, trực diện.
- **Đối với `h2` (Tiêu đề từng chương):**
  - Luân phiên nhịp nhàng giữa **Nền Sáng Chính (`--bg`)**, **Nền Bề Mặt Phụ (`--surface`)**, và **Khối Nền Đậm (`--field-primary` / `--field-ink`)** để người đọc cảm nhận rõ ranh giới từng chương mà không cần đường kẻ ngang `<hr>`.

---

## 4. Quy Tắc Typography & Bảo Toàn Tiêu Đề 2 Dòng

1. **Tự chủ ফন্ট chữ (`Self-Hosted Local Fonts`):**
   - Nhúng trực tiếp file `.woff2` từ thư mục `assets/fonts/fonts.css`, không phụ thuộc CDN bên ngoài để tránh nhảy chữ (`FOUT/CLS`) và vượt qua kiểm tra `lint.mjs`.
2. **Thang chữ ít cấp độ nhưng phân hóa rõ (`Few Sizes, Far Apart`):**
   - Sử dụng `clamp(min, vw, max)` cho `--step-display` (`h1`), `--step-head` (`h2`), `--step-title` (`h3`), `--step-lead`, `--step-body`, `--step-small`.
3. **Cam kết Tiêu đề Tối đa 2 Dòng (`Two-Line Headline Rule`):**
   - Mọi tiêu đề `h1`, `h2`, `h3` cần được biên tập cô đọng, súc tích kèm `text-wrap: balance` để không bao giờ bị rớt quá 2 dòng trên cả 3 khung nhìn `1440px`, `768px`, `390px`.
   - Đối với các danh từ ghép riêng ở cuối câu (ví dụ tên quốc gia, tên thương hiệu 2 âm tiết), gắn `&nbsp;` giữa 2 âm tiết để tránh bị rớt 1 chữ lẻ xuống dòng mới.

---

## 5. Chiến Lược Lựa Chọn Thư Viện JS Cho Tính Năng UI/UX & Quy Tắc Custom Skinning Đồng Bộ (Tránh Tự Dev Gây Lỗi)

### 5.1. Triết Lý "Không Tự Dev Lại Từ Đầu Các Tính Năng UI/UX Phức Tạp"
- **Nguyên nhân:** Các tính năng tương tác người dùng như Lightbox, Panzoom (phóng to/kéo di chuyển), Carousel/Slider (vuốt chạm có quán tính), Dropdown tìm kiếm đa mục, Toast thông báo, Accordion phức tạp... đòi hỏi xử lý rất nhiều bài toán hóc búa về bounding box, con lăn chuột (`wheel delta`), sự kiện cảm ứng chạm (`touch gestures`), khóa cuộn body (`scroll lock`) và chuyển động đa khung hình.
- **Hậu quả nếu tự dev tay:** Thường xuyên phát sinh lỗi mất kiểm soát: cuộn qua trái rồi qua phải thì không ấn phóng to ảnh được nữa, kẹt cử chỉ zoom, đơ nút bấm, giật lag trên di động thật.
- **Giải pháp bắt buộc:** **Khảo sát và chọn ngay thư viện JS tiêu chuẩn công nghiệp (đã qua hàng triệu người dùng kiểm thử), tải về lưu trữ cục bộ (`assets/vendor/`) và chỉ viết code custom skinning giao diện** theo đúng bảng màu & token của dự án. Không tự dev lại bánh xe để tiết kiệm thời gian và đảm bảo độ ổn định 100%.

### 5.2. Điển Hình Bắt Buộc: Dùng Thư Viện Fancybox v5 Local & Custom Thiết Kế Lại Giao Diện Popup
Mọi dự án website có hình ảnh nội dung, tư liệu hoặc thư viện ảnh **BẮT BUỘC** sử dụng thư viện **Fancybox v5 (`@fancyapps/ui` v5) lưu trữ cục bộ (`assets/vendor/fancybox/fancybox.css` + `assets/vendor/fancybox/fancybox.umd.js`)** và **phải thiết kế lại (custom) toàn bộ hệ thống nút bấm, thanh công cụ, bộ đếm, chú thích và ảnh thu nhỏ** theo đúng bảng màu & ngôn ngữ hình học của `DESIGN.md` (tuyệt đối không để giao diện mặc định của thư viện):

#### Yêu Cầu Tùy Biến Thiết Kế (`Custom UI Skinning`) Bằng Biến CSS Chính Thức `--f-button-*`
- **Đồng bộ Bảng màu & Độ bo góc (`border-radius`) với Hệ thống Nút Bấm của Dự án:**
  - Gán các biến `--f-button-*` trên `.fancybox__toolbar` và `.fancybox__nav` theo đúng Token Ngữ Nghĩa của dự án (`--field-primary`, `--accent`, `--accent-ink`, `--bg`, `--ink`).
  - Áp dụng **Đảo Cực Màu (`Polarity Swap`) khi `:hover`** và **Phản Hồi Vật Lý `scale(0.95)` khi `:active`** cho mọi nút điều hướng (`<`, `>`) và các nút trên thanh công cụ (`zoomIn`, `zoomOut`, `toggle1to1`, `slideshow`, `fullscreen`, `thumbs`, `close`).
  - Tách riêng màu nhấn cho nút Đóng (`[data-fancybox-close]`) ở góc phải trên và đồng bộ font chữ hiển thị (`--font-display`, `--font-text`) cho bộ đếm `.fancybox__infobar` cùng hộp chú thích `.fancybox__caption`.

```css
.fancybox__container {
  --fancybox-zIndex: var(--z-lightbox);
  --fancybox-bg: /* Lớp nền tối sâu đồng bộ --field-ink của dự án */;
  --fancybox-color: var(--bg);
  --fancybox-hover-color: var(--ink);
  --f-spinner-color-2: var(--accent);
  z-index: var(--z-lightbox) !important;
  font-family: var(--font-text);
}

.fancybox__toolbar,
.fancybox__nav {
  --f-button-width: 46px;
  --f-button-height: 46px;
  --f-button-border: 0;
  --f-button-border-radius: /* Đồng bộ bo góc với nút .btn của dự án */;
  --f-button-color: var(--bg);
  --f-button-hover-color: var(--accent-ink);
  --f-button-bg: var(--field-primary);
  --f-button-hover-bg: var(--accent);
  --f-button-active-bg: var(--field-contrast);
  --f-button-svg-width: 21px;
  --f-button-svg-height: 21px;
  --f-button-svg-stroke-width: 2;
  --f-button-next-pos: 20px;
  --f-button-prev-pos: 20px;
  --f-progress-color: var(--accent);
  text-shadow: none; /* taste-skill-ignore */
}

.fancybox__toolbar .f-button:active:not([disabled]),
.fancybox__nav .f-button:active:not([disabled]) {
  transform: scale(0.95);
}

.fancybox__toolbar [data-fancybox-close] {
  --f-button-bg: var(--field-contrast);
  --f-button-color: var(--bg);
  --f-button-hover-bg: var(--accent);
  --f-button-hover-color: var(--accent-ink);
}
```

### 5.2. Cấu Hình Chuẩn 5 Thao Tác Chuột & Cảm Ứng Gốc Của Fancybox v5 (`Fancybox.bind`)
```javascript
window.Fancybox.bind('[data-fancybox="page-gallery"]', {
  loop: true,
  dragToClose: false,
  contentClick: 'toggleMax', // 1. Ấn chuột lần 1 phóng to, ấn lần 2 thu nhỏ về 1x
  contentDblClick: false,
  wheel: 'zoom',             // 2. Cuộn con lăn chuột để phóng to / thu nhỏ mượt
  Images: {
    zoom: true,
    Panzoom: { maxScale: 3 } // 3. Ở 1x kéo trái/phải chuyển ảnh; ở >1x giữ chuột kéo pan xem các góc ảnh
  },
  Thumbs: {
    type: 'classic',
    showOnStart: false
  },
  Toolbar: {
    display: {
      left: ['infobar'],
      middle: ['zoomIn', 'zoomOut', 'toggle1to1'],
      right: ['slideshow', 'fullscreen', 'thumbs', 'close']
    }
  }
});
```

---

## 6. Quy Tắc Chuẩn Hóa Hệ Thống Biểu Mẫu (`Form Reset & Class Contract`) — Chống Lỗi Input Mặc Định Trình Duyệt

Khi nhiều Subagent cùng xây dựng các trang con hoặc thêm trang mới sau này, nếu một trang dùng `.member-form-field` trong khi `css/styles.css` chỉ định nghĩa `.member-form-row` (hoặc dùng `.matcher-chip-row` thay vì `.matcher-option-grid`), trình duyệt sẽ rơi về giao diện `<input>` / `<textarea>` mặc định thô cứng và làm vỡ bố cục. Vì vậy, `css/styles.css` **BẮT BUỘC** phải áp dụng quy tắc bảo vệ kép sau:

1. **Định nghĩa CSS bao phủ trực tiếp mọi thẻ `input, select, textarea` bên trong `form` và hỗ trợ đồng thời cả 2 tên class (`.member-form-row`, `.member-form-field`):**
   ```css
   .member-form-row,
   .member-form-field {
     display: flex;
     flex-direction: column;
     gap: 6px;
   }

   .member-form input:not([type="checkbox"]):not([type="radio"]),
   .member-form select,
   .member-form textarea {
     width: 100%;
     min-height: 48px;
     padding: 12px 14px;
     background-color: var(--bg);
     color: var(--ink);
     border: 0;
     border-radius: 0px;
     font-family: var(--font-text);
     font-size: var(--step-body);
   }
   ```
2. **Bắt buộc chạy cả `lint.mjs` VÀ `inspect.mjs` (render thực tế bằng trình duyệt headless) sau mỗi lần thêm/sửa trang:**
   - Công cụ `lint.mjs` của `taste-skill` chỉ quét mã nguồn tĩnh (regex text, inline style), **không phát hiện được lỗi lệch tên class CSS**.
   - Do đó, mọi Agent sau khi sửa HTML/CSS bắt buộc phải chạy `inspect.mjs` và mở ảnh chụp `.taste-skill/inspect-*/desktop-page-*.jpg` để kiểm tra trực quan các biểu mẫu, nút bấm và khoảng cách trước khi deploy.

