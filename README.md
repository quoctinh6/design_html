# BỘ KHUNG HƯỚNG DẪN ĐA AGENT XÂY DỰNG WEBSITE (`design_demo`)

Thư mục này chứa bộ quy trình chuẩn (**Project-Agnostic Blueprint**) dành cho **Lead Agent** và các **Subagent** khi thiết kế và lập trình bất kỳ dự án website mới nào.

> **Nguyên tắc cốt lõi:**
> - **Xác định phong cách thiết kế riêng biệt (Không rập khuôn):** Mỗi dự án mới phải nghiên cứu ngành nghề để rút ra hướng nghệ thuật (`Art Direction`), phong cách hình ảnh, bảng màu mở và cặp font chữ độc bản.
> - **Khảo sát & chọn thư viện JS chuẩn (Tránh tự dev lại từ đầu gây lỗi):** Bắt buộc chọn thư viện chuyên dụng chuẩn công nghiệp cho các tính năng UI/UX phức tạp (Lightbox, Slider, Dropdown, Toast...), tải về local và custom skinning đồng bộ Design System thay vì tự code tay chắp vá.
> - **Giữ kỷ luật kỹ thuật tuyệt đối:** Mọi dự án đều phải tuân thủ quy tắc chống văng/dồn chữ khi hoạt ảnh chạy, thang `z-index` không chồng chéo, tương thích hoàn hảo trên thiết bị thật (PC, iPad, iPhone/Android, Điện thoại gập) và vượt qua bộ kiểm tra tự động `0 FAIL, 0 WARN`.

---

## Danh Mục Tài Liệu Trong `design_demo/`

| STT | File Tài Liệu | Mục Đích & Nội Dung Chính |
| :--- | :--- | :--- |
| **01** | [`01_AGENT_WORKFLOW.md`](01_AGENT_WORKFLOW.md) | **Quy trình 6 Giai đoạn & Điều phối Đa Agent:**<br>• *Giai đoạn 1:* Xác định phong cách thiết kế (`Art Direction`), Archetype, Palette Mood & quy hoạch `SITEMAP.md` (kèm Trang Pháp Lý độc bản).<br>• *Giai đoạn 2:* Khảo sát & xác định thư viện JS tiêu chuẩn cho các tính năng UI/UX để tránh tự dev gây lỗi.<br>• *Giai đoạn 3:* Kiến trúc hoạt ảnh, chống văng chữ & thang z-index.<br>• *Giai đoạn 4:* Chốt `DESIGN.md`, `SITEMAP.md` & CSS nền tảng.<br>• *Giai đoạn 5:* Phân tách Subagent chạy song song theo Sitemap.<br>• *Giai đoạn 6:* Tối ưu thiết bị thật, kiểm định tự động (`lint`/`inspect`) & rà soát pháp lý Việt Nam. |
| **02** | [`02_DESIGN_SYSTEM_TEMPLATE.md`](02_DESIGN_SYSTEM_TEMPLATE.md) | **Khung Thiết Kế Mở (`Open Design System`) & Custom Thư Viện UI/UX:** Phương pháp gợi mở bảng màu theo ngữ nghĩa, ma trận trạng thái nút bấm (`Idle / Hover / Active / Focus`), nhịp điệu nền cho `h1`/`h2`, Typography, chuẩn hóa class biểu mẫu (`.member-form-row` / `.member-form-field`) và yêu cầu bắt buộc custom thiết kế lại hệ thống nút/thanh công cụ của thư viện JS (như `--f-button-*` của Fancybox v5 Local). |
| **03** | [`03_ANIMATION_AND_ZINDEX.md`](03_ANIMATION_AND_ZINDEX.md) | **Kiến Trúc Hoạt Ảnh & Thang `z-index` Thống Nhất:** Ứng dụng `imskyleen/animate-ui`, quy tắc bắt buộc **Chống Văng / Dồn Chữ Khi Khung Co Giãn**, bảng kê phần tử bị ảnh hưởng trong/ngoài và thang `z-index` 9 tầng. |
| **04** | [`04_REAL_DEVICE_AND_FOLDABLE.md`](04_REAL_DEVICE_AND_FOLDABLE.md) | **Tối Ưu Thiết Bị Thật (iPhone, Android, iPad & Điện Thoại Gập):** Ứng dụng bộ skill của `emilkowalski` (`mobile-native`, `break-ui`, `emil-design-eng`, `apple-design`) để xử lý khác biệt giữa giả lập PC và trình duyệt di động thật. |
| **05** | [`05_VN_LEGAL_COMPLIANCE.md`](05_VN_LEGAL_COMPLIANCE.md) | **Thiết Kế & Kiểm Định Tuân Thủ Pháp Luật Việt Nam Ngay Từ Sitemap:** Tham khảo [`mhfed/vn-law-check`](https://github.com/mhfed/vn-law-check) làm khung nền kết hợp **bắt buộc chủ động tra cứu trực tuyến các Luật/Nghị định/Thông tư mới nhất**, đưa các trang pháp lý bắt buộc vào `SITEMAP.md` ngay từ đầu và thiết kế theo **Layout Văn kiện Pháp chế độc bản** (cấm dùng lại layout của các trang nội dung cũ). |

---

## Thứ Tự Đọc Dành Cho Agent Khi Bắt Đầu Dự Án Mới

1. **Lead Agent (Kiến trúc sư trưởng):**
   - Đọc [`01_AGENT_WORKFLOW.md`](01_AGENT_WORKFLOW.md) -> [`02_DESIGN_SYSTEM_TEMPLATE.md`](02_DESIGN_SYSTEM_TEMPLATE.md) -> [`03_ANIMATION_AND_ZINDEX.md`](03_ANIMATION_AND_ZINDEX.md) -> [`05_VN_LEGAL_COMPLIANCE.md`](05_VN_LEGAL_COMPLIANCE.md).
   - Xác định phong cách thiết kế, lựa chọn thư viện JS chuẩn (tải vào `assets/vendor/`), khởi tạo `DESIGN.md`, `SITEMAP.md`, `ANIMATION.md`, và `css/styles.css` nền tảng cho dự án mới.
2. **Các Subagent (Triển khai Sitemap & Đa thiết bị):**
   - Đọc `DESIGN.md`, `SITEMAP.md`, [`03_ANIMATION_AND_ZINDEX.md`](03_ANIMATION_AND_ZINDEX.md), [`04_REAL_DEVICE_AND_FOLDABLE.md`](04_REAL_DEVICE_AND_FOLDABLE.md) và [`05_VN_LEGAL_COMPLIANCE.md`](05_VN_LEGAL_COMPLIANCE.md).
   - Xây dựng từng trang độc lập, sử dụng thư viện JS đã chọn (không tự chế lại), đúng chuẩn pháp lý Việt Nam và tối ưu cho từng dải thiết bị được phân công.
3. **QA & Real-Device Agent (Kiểm định cuối):**
   - Áp dụng [`04_REAL_DEVICE_AND_FOLDABLE.md`](04_REAL_DEVICE_AND_FOLDABLE.md) và [`05_VN_LEGAL_COMPLIANCE.md`](05_VN_LEGAL_COMPLIANCE.md) để chạy `lint.mjs`, `inspect.mjs`, kiểm thử khắc nghiệt (`break-ui`) và rà soát pháp lý trước khi bàn giao.
