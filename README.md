# BỘ KHUNG HƯỚNG DẪN ĐA AGENT XÂY DỰNG WEBSITE (`design_demo`)

Thư mục này chứa bộ quy trình chuẩn (**Project-Agnostic Blueprint**) dành cho **Lead Agent** và các **Subagent** khi thiết kế và lập trình bất kỳ dự án website mới nào.

> **Nguyên tắc cốt lõi:**
> - **Không rập khuôn màu sắc hay bố cục cũ:** Mỗi dự án mới phải rút hướng nghệ thuật (`Art Direction`) mới phù hợp với ngành nghề, thương hiệu và tệp khách hàng của dự án đó.
> - **Giữ kỷ luật kỹ thuật tuyệt đối:** Mọi dự án đều phải tuân thủ quy tắc chống văng/dồn chữ khi hoạt ảnh chạy, thang `z-index` không chồng chéo, tương thích hoàn hảo trên thiết bị thật (PC, iPad, iPhone/Android, Điện thoại gập) và vượt qua bộ kiểm tra tự động `0 FAIL, 0 WARN`.

---

## Danh Mục Tài Liệu Trong `design_demo/`

| STT | File Tài Liệu | Mục Đích & Nội Dung Chính |
| :--- | :--- | :--- |
| **01** | [`01_AGENT_WORKFLOW.md`](01_AGENT_WORKFLOW.md) | **Quy trình 5 Giai đoạn & Điều phối Đa Agent:** Cách kết hợp `taste-skill` + `impeccable` + `imskyleen/animate-ui` + `emilkowalski/skill`, chốt `DESIGN.md` & `SITEMAP.md` trước khi tách các Subagent chạy song song. |
| **02** | [`02_DESIGN_SYSTEM_TEMPLATE.md`](02_DESIGN_SYSTEM_TEMPLATE.md) | **Khung Thiết Kế Mở (`Open Design System`) & Custom Fancybox v5 Local:** Phương pháp gợi mở bảng màu theo ngữ nghĩa, ma trận trạng thái nút bấm (`Idle / Hover / Active / Focus`), nhịp điệu nền cho `h1`/`h2`, Typography và yêu cầu bắt buộc custom thiết kế lại hệ thống nút/thanh công cụ của Fancybox v5 Local (`--f-button-*`). |
| **03** | [`03_ANIMATION_AND_ZINDEX.md`](03_ANIMATION_AND_ZINDEX.md) | **Kiến Trúc Hoạt Ảnh & Thang `z-index` Thống Nhất:** Ứng dụng `imskyleen/animate-ui`, quy tắc bắt buộc **Chống Văng / Dồn Chữ Khi Khung Co Giãn**, bảng kê phần tử bị ảnh hưởng trong/ngoài và thang `z-index` 9 tầng. |
| **04** | [`04_REAL_DEVICE_AND_FOLDABLE.md`](04_REAL_DEVICE_AND_FOLDABLE.md) | **Tối Ưu Thiết Bị Thật (iPhone, Android, iPad & Điện Thoại Gập):** Ứng dụng bộ skill của `emilkowalski` (`mobile-native`, `break-ui`, `emil-design-eng`, `apple-design`) để xử lý khác biệt giữa giả lập PC và trình duyệt di động thật. |

---

## Thứ Tự Đọc Dành Cho Agent Khi Bắt Đầu Dự Án Mới

1. **Lead Agent (Kiến trúc sư trưởng):**
   - Đọc [`01_AGENT_WORKFLOW.md`](01_AGENT_WORKFLOW.md) -> [`02_DESIGN_SYSTEM_TEMPLATE.md`](02_DESIGN_SYSTEM_TEMPLATE.md) -> [`03_ANIMATION_AND_ZINDEX.md`](03_ANIMATION_AND_ZINDEX.md).
   - Khởi tạo `DESIGN.md`, `SITEMAP.md`, `ANIMATION.md`, và `css/styles.css` nền tảng cho dự án mới.
2. **Các Subagent (Triển khai Sitemap & Đa thiết bị):**
   - Đọc `DESIGN.md`, `SITEMAP.md`, [`03_ANIMATION_AND_ZINDEX.md`](03_ANIMATION_AND_ZINDEX.md) và [`04_REAL_DEVICE_AND_FOLDABLE.md`](04_REAL_DEVICE_AND_FOLDABLE.md).
   - Xây dựng từng trang độc lập và tối ưu cho từng dải thiết bị được phân công.
3. **QA & Real-Device Agent (Kiểm định cuối):**
   - Áp dụng [`04_REAL_DEVICE_AND_FOLDABLE.md`](04_REAL_DEVICE_AND_FOLDABLE.md) để chạy `lint.mjs`, `inspect.mjs` và kiểm thử khắc nghiệt (`break-ui`) trên các khung nhìn thực tế.
