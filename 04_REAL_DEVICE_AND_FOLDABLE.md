# 04. TỐI ƯU HÓA TRÊN THIẾT BỊ THẬT (IPHONE, ANDROID, IPAD & ĐIỆN THOẠI GẬP) VỚI BỘ SKILL `EMILKOWALSKI` (`04_REAL_DEVICE_AND_FOLDABLE.md`)

Tài liệu này tổng hợp quy trình kiểm thử và tối ưu giao diện trên thiết bị di động thật dựa trên bộ skill của **Emil Kowalski** ([`https://github.com/emilkowalski/skill`](https://github.com/emilkowalski/skill) — cài đặt trên máy mới bằng `npx skills add https://github.com/emilkowalski/skill`):
- **`mobile-native`** ([`https://github.com/emilkowalski/skill`](https://github.com/emilkowalski/skill))
- **`break-ui`** ([`https://github.com/emilkowalski/skill`](https://github.com/emilkowalski/skill))
- **`emil-design-eng`** ([`https://github.com/emilkowalski/skill`](https://github.com/emilkowalski/skill))
- **`animate` & `animation-performance`** ([`https://github.com/emilkowalski/skill`](https://github.com/emilkowalski/skill))
- **`apple-design`** ([`https://github.com/emilkowalski/skill`](https://github.com/emilkowalski/skill))

---

## 1. Tại Sao Giả Lập Trên PC Thì Đẹp Nhưng Mở Trên Điện Thoại Thật Lại Lỗi?

Trình duyệt giả lập trên máy tính (DevTools Responsive Mode) chỉ thu nhỏ khung nhìn của trình duyệt Desktop, trong khi **Chrome Android** và **Safari iOS** trên điện thoại thật có cơ chế dựng hình khác biệt hoàn toàn ở 5 điểm:

1. **Cơ chế tính tràn ngang (`Viewport Overflow`):**
   - Trên Safari iOS và Chrome Android, nếu chỉ đặt `overflow-x: hidden` ở thẻ `body` mà không đặt ở `html`, bất kỳ phần tử con nào rộng hơn màn hình (như một dòng chữ lớn ở footer hoặc một cột Grid thiếu `min-width: 0`) sẽ đẩy rộng toàn bộ `documentElement`, tạo ra một vệt trống dọc bên phải màn hình và cho phép người dùng vuốt lệch trang sang ngang.
2. **Thanh địa chỉ co giãn động (`Dynamic Browser Chrome`):**
   - Đơn vị `100vh` truyền thống tính theo chiều cao khi thanh địa chỉ đã ẩn, khiến khi mới vào trang (thanh địa chỉ đang mở), phần đáy của Hero hoặc nút bấm trong menu bị che khuất.
3. **Trạng thái `:hover` bị dính (`Sticky Hover on Touch`):**
   - Khi chạm tay vào một nút hoặc thẻ trên điện thoại, trình duyệt di động giả lập sự kiện `:hover` và giữ nguyên trạng thái đó cho tới khi người dùng chạm vào chỗ khác, làm giao diện trông như bị kẹt.
4. **Phông chữ hệ thống & Tự động phóng to ô nhập liệu (`iOS Auto-Zoom`):**
   - Safari iOS tự động phóng to toàn trang nếu bất kỳ thẻ `<input>`, `<select>`, `<textarea>` nào có `font-size < 16px`.
5. **Độ trễ chạm & Menu chọn chữ mặc định (`Touch Callout & Tap Highlight`):**
   - Khi nhấn giữ hoặc bấm nhanh liên tiếp vào các nút tương tác trên di động, trình duyệt hiện khung nháy xanh (`tap-highlight`) hoặc bật menu copy văn bản của hệ điều hành.

---

## 2. Bộ 8 Quy Tắc CSS/HTML Bắt Buộc Cho Mọi Thiết Bị Thật

### Quy tắc 1: Khóa tràn ngang kép trên cả `html`, `body` và các vùng Full-Bleed
```css
html,
body {
  overflow-x: hidden;
  overflow-x: clip;
  width: 100%;
  max-width: 100%;
  -webkit-text-size-adjust: 100%;
  -webkit-tap-highlight-color: transparent;
}
```

### Quy tắc 2: Bảo vệ mọi cột CSS Grid / Flexbox bằng `min-width: 0`
```css
/* Bắt buộc trên mọi cột trực thuộc lưới 12 cột để chữ dài tự động xuống dòng, không đẩy vỡ khung */
.grid-12 > * {
  min-width: 0;
}
```
Đối với các chuỗi liên kết dài (email, URL, mã hồ sơ), luôn kết hợp `max-width: 100%; overflow-wrap: anywhere;` và cỡ chữ `clamp()` có sàn nhỏ phù hợp màn hình `320px`.

### Quy tắc 3: Khai báo Thẻ Meta Viewport Chuẩn Tai Thỏ (`Safe Area Insets`)
Trong `<head>` của mọi trang HTML:
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover, interactive-widget=resizes-content">
<meta name="theme-color" content="#0A1628">
```
Và bù lề vùng an toàn (`env(safe-area-inset-*)`) cho các thành phần sát mép màn hình (Header, Menu di động, Nút nổi, Popup, Footer).

### Quy tắc 4: Dùng `100svh` / `100dvh` Thay Cho `100vh`
```css
.hero-viewport {
  min-height: 100vh;
  min-height: 100svh;
}

.mobile-nav-sheet {
  height: 100vh;
  height: 100dvh;
  overscroll-behavior: contain;
}
```

### Quy tắc 5: Cô Lập `:hover` Chỉ Dành Cho Thiết Bị Có Chuột Thật
```css
@media (hover: hover) and (pointer: fine) {
  .btn:hover {
    transform: translateY(-2px);
  }
  .card:hover {
    transform: translateY(-4px);
  }
}
```

### Quy tắc 6: Tối Ưu Vùng Chạm Vật Lý (`>= 44px–48px`) & Chống Nháy Chạm
```css
.btn,
button,
[role="button"] {
  min-height: 44px;
  touch-action: manipulation;
  user-select: none;
  -webkit-user-select: none;
  -webkit-touch-callout: none;
}
```

### Quy tắc 7: Chống Tự Động Zoom Khi Nhập Form Trên iPhone (`font-size >= 16px`)
```css
input,
select,
textarea {
  font-size: max(16px, 1rem);
}
```

### Quy tắc 8: Tối Ưu Nút Công Cụ Thu Gọn Trên Màn Hình Nhỏ (`Floating Trigger + Compact Popup`)
- Khi một bộ công cụ có nhiều nút lọc + bảng kết quả nằm trên màn hình `< 1024px`:
  - Lúc người dùng cuộn xuống vùng bảng kết quả, thu gọn bảng nút lọc thành một **nút icon nhỏ gọn neo bên mép trái** (`width: 46px; height: 46px; z-index: var(--z-floating-dock)`).
  - Khi bấm vào nút icon đó, mở ra một **khung Popup nhỏ gọn vừa khít nội dung** (`height: auto; max-height: calc(100svh - 108px); width: min(350px, calc(100vw - 28px)); z-index: var(--z-drawer-panel)`), tuyệt đối không kéo dài hết chiều dọc màn hình gây trống trải.

---

## 3. Ma Trận 5 Điểm Gãy Responsive (Bao Gồm Cả iPad & Điện Thoại Gập)

| Thiết Bị Mục Tiêu | Media Query Chuẩn | Quy Tắc Bố Cục Trọng Tâm |
| :--- | :--- | :--- |
| **1. Desktop / Laptop** | Mặc định (`> 1024px`) | Lưới 12 cột đầy đủ; các dải ngang (`.showcase-band`, `.spine-deck`) mở rộng theo chiều ngang. |
| **2. iPad / Máy tính bảng** | `@media (max-width: 1024px)` | Chuyển thanh điều hướng sang `.mobile-menu-toggle`; các khối 3–4 cột rút xuống 2 cột (`span 6`); bật cơ chế thu gọn bộ lọc sang nút neo trái khi cuộn. |
| **3. Điện thoại Gập — Mở Màn Hình Chính (`Foldable Unfolded`)** | `@media (min-width: 641px) and (max-width: 900px)` | Tối ưu cho màn hình tỷ lệ gần vuông (*Galaxy Z Fold Unfolded `673px–768px`, Pixel Fold*): giữ lưới 2 cột cho các ô nút lọc/thông số và chia tỷ lệ `1.35fr 1fr` cho khối ảnh + số liệu để tận dụng chiều ngang. |
| **4. Điện thoại Di động Chuẩn (`iPhone / Android`)** | `@media (max-width: 640px)` | Chuyển các dải ảnh ngang (`.showcase-band`) sang **xếp chồng dọc (`flex-direction: column; width: 100%`)** để tấm ảnh đang mở luôn đạt 100% chiều rộng và chữ chú thích không bao giờ bị ép dọc; các cột nội dung trải rộng `1 / 13`. |
| **5. Điện thoại Gập — Màn Hình Phụ Khi Gập (`Foldable Cover`)** | `@media (max-width: 359px)` | Tối ưu cho màn hình ngoài siêu hẹp (`280px–344px`): giảm `--margin: 12px`, hạ sàn `clamp()` của tiêu đề, và đưa toàn bộ lưới 2 cột phụ về `grid-template-columns: 1fr`. |

---

## 4. Quy Trình Phá Thử Giao Diện (`break-ui`) Trước Khi Nghiệm Thu

Trước khi kết thúc dự án, Agent phải kiểm tra giao diện qua 4 bài thử khắc nghiệt của skill `break-ui`:
1. **Thử nghiệm co kéo cửa sổ liên tục (`Live Resize Stress Test`):** Kéo trình duyệt từ `1440px -> 768px -> 390px -> 320px` để đảm bảo không có chữ nào bị dồn cục hoặc tràn khỏi khung.
2. **Thử nghiệm chuỗi văn bản cực trị (`Worst-Case Copy`):** Kiểm tra các tiêu đề có từ ghép dài, địa chỉ email dài hoặc tên đơn vị dài xem có bị rớt 1 chữ lẻ xuống dòng cuối hay tràn ngang không.
3. **Thử nghiệm bấm liên tiếp tốc độ cao (`Rapid Interaction Interrupt`):** Bấm chuyển tab / mở thẻ liên tục 5 lần trong 1 giây để đảm bảo bộ đếm thời gian (`clearTimeout`) hủy sạch luồng cũ, không làm chữ nhấp nháy.
4. **Chạy kiểm định tự động `lint.mjs` và `inspect.mjs`:** Đảm bảo `0 FAIL, 0 WARN` trên toàn bộ các trang.
