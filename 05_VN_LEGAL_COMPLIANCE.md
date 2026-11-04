# 05. THIẾT KẾ & KIỂM ĐỊNH TUÂN THỦ PHÁP LUẬT VIỆT NAM (`05_VN_LEGAL_COMPLIANCE.md`)

Tài liệu này quy định quy trình bắt buộc dành cho mọi Agent khi thiết kế giao diện (`DESIGN.md`, `SITEMAP.md`), xây dựng biểu mẫu, chân trang (`Footer`) và nội dung website tại thị trường Việt Nam.

---

## 1. Nguyên Tắc Kết Hợp Đa Nguồn: Tham Khảo `mhfed/vn-law-check` + Chủ Động Tra Cứu Luật Mới Nhất

> **CẢNH BÁO QUAN TRỌNG — KHÔNG PHỤ THUỘC MỘT CHIỀU (`No Single-Source Trap`):**
> - Agent **sử dụng [`https://github.com/mhfed/vn-law-check`](https://github.com/mhfed/vn-law-check) làm bộ khung rà soát nền tảng (`Baseline Checklist`)**, **NHƯNG TUYỆT ĐỐI KHÔNG coi đó là nguồn duy nhất**.
> - Hệ thống văn bản quy phạm pháp luật Việt Nam (Luật của Quốc hội, Nghị định của Chính phủ, Thông tư của các Bộ ngành) được ban hành, sửa đổi và có hiệu lực mới liên tục theo từng giai đoạn.
> - Vì vậy, ở mỗi dự án mới, Agent **BẮT BUỘC phải kết hợp cả 2 bước**:
>   1. **Bước A — Đối chiếu khung nền từ [`https://github.com/mhfed/vn-law-check`](https://github.com/mhfed/vn-law-check):** Quét phân loại website (`00-triage.md`) và kiểm tra các nhóm quy định cốt lõi (Bảo vệ dữ liệu cá nhân, Thương mại điện tử, An ninh mạng, Bảo vệ quyền lợi người tiêu dùng, Quảng cáo, Giao dịch điện tử, Minh bạch AI).
>   2. **Bước B — Chủ động tra cứu trực tuyến (`Web Search`) các Luật, Nghị định, Thông tư mới nhất vừa ban hành tính đến thời điểm hiện tại:** Tra cứu trên các cơ sở dữ liệu pháp luật chính thống (`vbpl.vn`, `congbao.chinhphu.vn`, `chinhphu.vn`, `thuvienphapluat.vn`, `online.gov.vn`) theo đúng **ngành nghề đặc thù của dự án** (ví dụ: Y tế/Dược phẩm, Bất động sản, Tài chính/Ngân hàng, Giáo dục, Thực phẩm, Thương mại điện tử, Hiệp hội/Tổ chức...) để cập nhật các quy định mới nhất vào thiết kế.

---

## 2. Quy Hoạch Các Trang Pháp Lý Ngay Từ Lúc Lập `SITEMAP.md` & Thiết Kế Layout Độc Bản (Cấm Dùng Lại Layout Trang Cũ)

> **NGUYÊN TẮC KIẾN TRÚC BẮT BUỘC NGAY TỪ GIAI ĐOẠN 1 (`SITEMAP.md` & `DESIGN.md`):**
> 1. **Đưa các Trang Pháp Lý vào `SITEMAP.md` ngay từ đầu (Không chắp vá về sau):**
>    - Ngay khi phân loại website ở Giai đoạn 1, Lead Agent phải liệt kê đầy đủ các trang pháp lý bắt buộc vào sơ đồ `SITEMAP.md` và thanh điều hướng chân trang (`Footer`):
>      - **Website tổ chức / doanh nghiệp / dịch vụ có biểu mẫu liên hệ hoặc đăng ký:** Bắt buộc có trang **`chinh-sach-bao-mat.html` (*Quy chế Bảo vệ Dữ liệu Cá nhân & Pháp lý*)**.
>      - **Website Thương mại điện tử / Bán hàng / Đặt dịch vụ trực tuyến:** Bắt buộc quy hoạch đầy đủ các trang hoặc phân khu văn kiện: *Quy chế Bảo vệ Dữ liệu Cá nhân*, *Điều khoản giao dịch chung*, *Chính sách thanh toán*, *Chính sách vận chuyển & giao nhận*, *Chính sách đổi trả / hoàn tiền & giải quyết khiếu nại*.
> 2. **Thiết Kế Layout Văn Kiện Pháp Chế Độc Bản (`Bespoke Legal Charter & Dossier Layout`) — Cấm Dùng Lại Layout Trang Nội Dung Cũ:**
>    - Trang pháp lý **TUYỆT ĐỐI KHÔNG ĐƯỢC sao chép (`copy-paste`) lại cấu trúc layout của các trang giới thiệu, sự kiện hay liên hệ** (như `subpage-masthead` + `tonal-blocks-grid`).
>    - Ngay từ Giai đoạn 3 (khi viết `DESIGN.md` và `css/styles.css`), Lead Agent phải thiết kế riêng hệ thống class CSS độc bản cho Trang Pháp Lý theo **Kiến trúc Văn kiện Hiến chương & Hồ sơ Tuân thủ (`Legal Charter & PDPL Dossier`)** gồm 4 vùng chuyên biệt:
>      1. **Masthead Văn Kiện & Hồ Sơ Đóng Dấu (`.legal-charter-hero` + `.legal-dossier-stamp`):** Đặt tiêu đề `h1` trên nền giấy sáng trang trọng kèm cột bên phải là **Hồ sơ Pháp chế đóng dấu hiệu lực** (ghi rõ Bên Kiểm soát và Xử lý Dữ liệu, Cơ quan chủ quản/Mã số thuế, Căn cứ Luật/Nghị định mới nhất và Đầu mối Bảo vệ dữ liệu cá nhân).
>      2. **Băng Thông Số Thời Hạn Pháp Định (`.legal-sla-band`):** Hiển thị trực quan bằng chữ số lớn (`tabular-nums`) các mốc thời gian cam kết phản hồi pháp định (ví dụ theo Điều 5 Nghị định 356/2025/NĐ-CP: `2 Ngày` xác nhận tiếp nhận, `10 Ngày` cung cấp/đính chính, `15 Ngày` rút đồng ý/hạn chế, `20 Ngày` xóa dữ liệu).
>      3. **Không Gian Đọc Văn Kiện 2 Cột (`.legal-workspace-section`):**
>         - **Cột trái (`.legal-index-rail`):** Mục lục văn kiện neo cố định (`position: sticky`), tự động sáng (`.is-active`) theo vị trí cuộn trang.
>         - **Cột phải (`.legal-clauses-stream`):** Mở đầu bằng **Bảng Kiểm Toán Dữ Liệu Thực Tế (`.legal-audit-matrix`)** đối chiếu trực tiếp mã nguồn website, theo sau là các thẻ **Điều khoản Pháp chế (`.legal-clause-card`)** có gắn nhãn số hiệu điều luật.
>      4. **Trạm Tương Tác Thực Hiện Quyền Chủ Thể Dữ Liệu (`[data-ui="pdpl-rights-console"]`):** Cho phép người dùng bấm chọn quyền cần thực hiện (*Xem & sao lục*, *Đính chính*, *Rút lại sự đồng ý*, *Yêu cầu xóa dữ liệu*) để tự động điền căn cứ pháp lý, thời hạn giải quyết và gửi yêu cầu trực tiếp.

### 2.3. Bốn Quy Tắc Kỹ Thuật Bắt Buộc Khi Code Trang Pháp Lý (Để Đạt `0 FAIL, 0 WARN` Khi Chạy `lint.mjs` & `inspect.mjs`)
1. **Đặt thẻ tiêu đề (`h1`, `h2`, `h3`) làm phần tử con đầu tiên của khối (Chống lỗi `[eyebrow-above-heading]`):**
   - Trong `.legal-hero-main`, đặt `<h1>` lên đầu tiên rồi mới đặt đoạn dẫn `.legal-hero-lead` và nhãn `.legal-doc-badge` ở bên dưới.
   - Trong mỗi thẻ `.legal-sla-card` và `.legal-clause-card`, đặt `<h3>` làm phần tử con đầu tiên, sau đó mới đến `.legal-sla-days` hoặc thanh nhãn `.legal-clause-top` (`.legal-clause-pill` + `.legal-clause-law-ref`).
2. **Không dùng số thứ tự có số 0 đứng đầu (`01`, `02`, `03`) (Chống lỗi `[numbered-step]`):**
   - Dùng chữ số tự nhiên (`2 Ngày`, `10 Ngày`, `15 Ngày`, `20 Ngày`) và số La Mã (`Điều I`, `Điều II`, `Điều III`...) cho các điều khoản pháp chế.
3. **Chỉ gắn `data-ui="pdpl-rights-console"` lên khung tương tác `.matcher-console` (Chống lỗi `[ui-wrapper]`):**
   - Không gắn `data-ui` lên thẻ `<section class="legal-rights-section">` bao ngoài.
4. **Cỡ chữ nhãn pháp lý tối thiểu `var(--step-small)` (`>= 13.5px`) (Chống lỗi `[small-text]` & `[micro-label]`):**
   - Mọi nhãn `.legal-doc-badge`, `.legal-dossier-status`, `.legal-sla-ref`, `.legal-status-tag`, `.legal-clause-pill`, `.legal-clause-law-ref` đều dùng `font-size: var(--step-small)` và không bật `text-transform: uppercase` cỡ nhỏ.

---

## 3. Sáu Hạng Mục Thiết Kế Giao Diện (`UI/UX Legal By Design`) Bắt Buộc Đúng Luật Việt Nam

Ngay từ khi thiết kế `DESIGN.md` và lập trình HTML/JS, các Agent phải đưa các thành phần pháp lý sau vào bố cục một cách tinh tế, đồng bộ với ngôn ngữ thiết kế của trang:

### 3.1. Biểu Mẫu Thu Thập Dữ Liệu Cá Nhân (Luật Bảo Vệ Dữ Liệu Cá Nhân `91/2025/QH15` & Nghị Định Hướng Dẫn Hiện Hành)
Mọi biểu mẫu (`<form>`) thu thập Họ tên, Số điện thoại, Email, Đơn vị công tác, Địa chỉ, Hồ sơ đăng ký... phải tuân thủ:
1. **Sự đồng ý rõ ràng, tự nguyện (`Explicit Opt-in Consent`):**
   - Phải có hộp kiểm (`<input type="checkbox" required>`) hoặc cơ chế xác nhận chủ động với nội dung rõ ràng (ví dụ: *"Tôi đồng ý để [Tên Đơn Vị] xử lý thông tin liên hệ nhằm mục đích tư vấn và phản hồi theo Chính sách bảo vệ dữ liệu cá nhân"*).
   - **CẤM tích sẵn (`checked` mặc định)** trên hộp kiểm đồng ý.
   - **CẤM sử dụng câu ép buộc thụ động:** *"Bằng việc tiếp tục sử dụng website này, bạn mặc định đồng ý..."* (theo Luật Bảo vệ dữ liệu cá nhân, sự im lặng hoặc không phản hồi của chủ thể dữ liệu không được coi là sự đồng ý).
2. **Minh bạch mục đích thu thập:**
   - Ngay tại khu vực biểu mẫu, phải ghi rõ dữ liệu được thu thập để làm gì, đơn vị nào tiếp nhận xử lý, thời gian phản hồi dự kiến và liên kết tới Chính sách bảo mật / Bảo vệ dữ liệu cá nhân.

### 3.2. Thông Tin Pháp Nhân & Định Danh Tại Chân Trang (`Footer`) (Luật Thương Mại Điện Tử `122/2025` & Nghị Định TMĐT Hiện Hành)
Chân trang (`<footer>`) của mọi website doanh nghiệp, tổ chức, cửa hàng trực tuyến tại Việt Nam phải thiết kế vùng hiển thị trang trọng, rõ ràng cho các thông tin bắt buộc sau:
1. **Tên đầy đủ của cơ quan, tổ chức, doanh nghiệp chủ quản** (và cơ quan trực thuộc nếu có).
2. **Số Giấy chứng nhận đăng ký doanh nghiệp / Mã số thuế / Quyết định thành lập** (kèm ngày cấp, nơi cấp).
3. **Địa chỉ trụ sở chính, Số điện thoại và Email liên hệ chính thức.**
4. **Họ tên người chịu trách nhiệm quản lý nội dung** (hoặc Ban phụ trách đối với cổng thông tin tổ chức).
5. **Biểu tượng & Liên kết xác thực của Bộ Công Thương (`online.gov.vn`):**
   - *"Đã thông báo Bộ Công Thương"* (biểu tượng màu xanh) đối với website TMĐT bán hàng.
   - *"Đã đăng ký Bộ Công Thương"* (biểu tượng màu đỏ) đối với sàn giao dịch TMĐT / nền tảng cung cấp dịch vụ TMĐT.

### 3.3. Minh Bạch Giá Cả, Điều Khoản & Bảo Vệ Người Tiêu Dùng (Luật Bảo Vệ Quyền Lợi Người Tiêu Dùng)
Đối với website có hiển thị giá sản phẩm, gói dịch vụ, biểu phí hội viên hoặc đặt lịch:
1. Ghi rõ giá niêm yết bằng **Đồng Việt Nam (`VNĐ` / `đ`)** (tuân thủ quy định hạn chế niêm yết giá bằng ngoại tệ trên lãnh thổ Việt Nam của Ngân hàng Nhà nước).
2. Ghi rõ giá đã bao gồm hay chưa bao gồm thuế GTGT (`VAT`), phí vận chuyển và các phụ phí khác (nếu có).
3. Cung cấp liên kết dễ tìm thấy tới: **Điều khoản sử dụng / Quy chế hoạt động**, **Chính sách thanh toán**, **Chính sách vận chuyển & giao nhận**, **Chính sách đổi trả / hoàn tiền**, và **Cơ chế tiếp nhận, giải quyết khiếu nại**.

### 3.4. Nội Dung Quảng Cáo & Từ Ngữ Khẳng Định Tuyệt Đối (Luật Quảng Cáo Sửa Đổi `75/2025` & Nghị Định Hướng Dẫn)
1. **Không tự ý dùng các từ ngữ mang tính so sánh nhất / tuyệt đối** như *"Số 1"*, *"Đầu tiên"*, *"Tốt nhất"*, *"Duy nhất"*, *"100% cam kết khỏi..."* trong tiêu đề `h1`, `h2` hoặc đoạn giới thiệu nếu khách hàng không có văn bản chứng nhận hợp pháp hoặc tài liệu kiểm chứng được pháp luật công nhận.
2. **Đối với ngành nghề kinh doanh có điều kiện** (Y tế, Thực phẩm bảo vệ sức khỏe, Mỹ phẩm, Giáo dục, Tài chính, Bất động sản): bắt buộc tra cứu thông tư mới nhất của Bộ quản lý chuyên ngành để chèn đầy đủ dòng khuyến cáo bắt buộc (ví dụ: *"Thực phẩm này không phải là thuốc và không có tác dụng thay thế thuốc chữa bệnh"*).

### 3.5. Minh Bạch Trí Tuệ Nhân Tạo (Luật Trí Tuệ Nhân Tạo `134/2025` & Quy Định Mới Nhất)
1. Nếu trên giao diện website có tích hợp **Trợ lý ảo / Chatbot AI**, công cụ tư vấn tự động hoặc hình ảnh/văn bản do AI tạo sinh, phải thiết kế nhãn thông báo rõ ràng để người dùng nhận biết mình đang tương tác với hệ thống AI.
2. Không sử dụng AI để tạo đánh giá giả mạo (`Fake Reviews`) của khách hàng trên trang.

### 3.6. Theo Dõi Dữ Liệu Bên Thứ Ba (`Cookies / Analytics / Pixels`) & An Ninh Mạng (Luật An Ninh Mạng `116/2025`)
1. Nếu website có nhúng mã theo dõi bên thứ ba (`Google Analytics`, `Meta Pixel`, `TikTok Pixel`, `Hotjar`...), phải thông báo trong Chính sách bảo mật và cung cấp cơ chế để người dùng đồng ý hoặc từ chối theo dõi không thiết yếu.
2. Mọi biểu mẫu gửi dữ liệu phải hoạt động trên giao thức mã hóa `HTTPS` và không làm lộ thông tin nhạy cảm của người dùng trên thanh địa chỉ URL (`method="GET"`).

---

## 4. Quy Trình 3 Bước Kiểm Định Pháp Lý Trước Khi Bàn Giao (`Legal QA Gate`)

Trước khi kết thúc Giai đoạn 5 và bàn giao sản phẩm, **QA Agent** hoặc **Lead Agent** phải thực hiện:

```bash
# Bước 1: Tham khảo bộ quét tự động của mhfed/vn-law-check (nếu cần quét nhanh mã nguồn)
git clone --depth=1 https://github.com/mhfed/vn-law-check /tmp/vn-law-check
bash /tmp/vn-law-check/scripts/scan.sh .
```

- **Bước 2: Chủ động tìm kiếm trực tuyến (`search_web`) các quy định mới nhất vừa ban hành:**
  - Kiểm tra ngày cập nhật của `sources/legal-references.md` trong repo `mhfed/vn-law-check`.
  - Thực hiện truy vấn tìm kiếm các Nghị định, Thông tư mới nhất của năm hiện tại liên quan trực tiếp đến loại hình website và ngành nghề của dự án.
- **Bước 3: Xử lý triệt để toàn bộ cảnh báo mức `🔴 Cần xử lý ngay` và `🟡 Nên cải thiện`** ngay trên giao diện HTML/JS trước khi deploy lên máy chủ.
