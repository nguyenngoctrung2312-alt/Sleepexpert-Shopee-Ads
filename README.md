# Shopee Ads BI — Web App (1 file HTML, publish qua GitHub Pages)

## 🆕 Giao diện v2 (07/2026) — "Aurora Glass" + Trợ lý Ads AI

Bản `index.html` hiện tại là giao diện xây mới hoàn toàn, giữ nguyên 100%
tính năng và dữ liệu Supabase cũ (không cần chạy lại SQL, không cần upload
lại dữ liệu). Bản cũ được giữ tại `index_backup_2026-07-06.html`.

**Mới về giao diện & animation:**
- Nền "aurora" gradient chuyển động + lưới hạt particle toàn app.
- Card kính mờ (glassmorphism), hover nâng nhẹ + viền phát sáng; Dark mode
  là mặc định (vẫn đổi được bằng nút 🌙/☀️).
- Trang đăng nhập hero 2 cột với orb ánh sáng chuyển động.
- Số KPI đếm tăng dần khi hiện; card fade-in khi cuộn tới; hiệu ứng chuyển
  trang mượt; biểu đồ vẽ animation; sidebar mới có icon SVG, nav active
  gradient; có nav ngang cho mobile.
- Font chữ Be Vietnam Pro (Google Fonts).
- Tôn trọng cài đặt "reduced motion" của hệ điều hành (tự tắt animation).

**Mới: Trợ lý Ads AI (nút 🤖 góc phải dưới, hiện sau khi đăng nhập):**
- Chatbot chạy HOÀN TOÀN trên trình duyệt, không cần API key, miễn phí,
  dữ liệu không gửi ra ngoài — bot đọc thẳng Supabase của bạn
  (`ads_daily_reports` 90 ngày gần nhất + Kế hoạch tháng mới nhất).
- Hiểu câu hỏi tiếng Việt có/không dấu: tổng quan, ROAS/ACOS, chi phí,
  doanh số, ngày tốt/tệ nhất, so sánh tuần này vs tuần trước, kế hoạch vs
  thực tế (kèm cảnh báo tốc độ chi nhanh/chậm so tiến độ tháng), gợi ý
  tối ưu theo chuẩn ngành.
- Có chip câu hỏi gợi ý sẵn, chỉ cần bấm.

**Cách publish bản mới:** chỉ cần upload lại `index.html` lên repo GitHub
(như Bước 3 bên dưới). Không có thay đổi SQL.

---

Web App quản lý & phân tích dữ liệu **Shopee Ads** — Upload Excel/CSV →
Đồng bộ Supabase → Dashboard BI. Toàn bộ Frontend chỉ gồm **1 file
`index.html`** (không cần cài đặt, không cần build) + **1 file SQL** để tạo
Database trên Supabase.

## Bộ file

| File | Vai trò | Có cần đưa lên GitHub Pages không? |
|---|---|---|
| `index.html` | Toàn bộ Web App (giao diện + logic). Chỉ cần sửa 2 dòng CONFIG rồi dùng. | **Bắt buộc** — đây là file duy nhất GitHub Pages thực sự chạy. |
| `.nojekyll` | File rỗng, báo GitHub bỏ qua bước build Jekyll (tránh lỗi deploy). | **Bắt buộc** — upload cùng `index.html`. |
| `supabase_schema.sql` | Chạy 1 lần trên Supabase để tạo Database (bảng, view, bảo mật RLS). | Không bắt buộc, chỉ để lưu tham khảo/tái tạo Database sau này. |
| `README.md` | File hướng dẫn này. | Không bắt buộc. |

Kiến trúc: **Frontend tĩnh (HTML/JS)** gọi thẳng **Supabase** (Postgres +
Auth) qua `supabase-js` — không có server trung gian, nên có thể host miễn
phí bằng GitHub Pages.

---

## BƯỚC 1 — Tạo Database trên Supabase

1. Vào https://supabase.com → **Start your project** → đăng nhập bằng GitHub.
2. **New project** → đặt tên (vd: `shopee-ads-bi`), đặt mật khẩu Database,
   chọn Region gần nhất (vd: Singapore) → chờ 1-2 phút khởi tạo.
3. Menu trái → **SQL Editor** → **New query** → dán **toàn bộ nội dung file
   `supabase_schema.sql`** → bấm **Run**.
4. Kiểm tra: menu **Table Editor** phải thấy 2 bảng `ads_campaign_reports`,
   `upload_history`.
5. Menu **Project Settings** (biểu tượng bánh răng) → **API** → copy 2 giá trị:
   - **Project URL**
   - **anon public** key
6. Menu **Authentication** → **Users** → **Add user** → tạo email/mật khẩu
   để đăng nhập App (có thể tạo nhiều user cho các thành viên khác).

## BƯỚC 2 — Cấu hình Supabase trong `index.html` (đã điền sẵn)

File `index.html` trong bộ này **đã được điền sẵn** Project URL và anon key
của project Supabase của bạn (khoảng dòng 117-118) — mở app lên là dùng
được ngay, không cần nhập gì thêm.

```js
const SUPABASE_URL = 'https://fphiqjdwnbegdhtkuhvb.supabase.co'
const SUPABASE_ANON_KEY = 'eyJhbGciOiJI...' // anon public key
```

Nếu sau này bạn **tạo project Supabase khác** hoặc **regenerate key**, chỉ
cần mở `index.html` bằng Notepad/VS Code, sửa lại 2 dòng trên bằng giá trị
mới lấy tại Project Settings → API → Lưu file.

> `anon public key` là key công khai, an toàn khi để trong file tĩnh —
> dữ liệu đã được bảo vệ bằng Row Level Security (RLS) trong
> `supabase_schema.sql`, chỉ user đã đăng nhập mới đọc/ghi được.

## BƯỚC 3 — Publish lên GitHub Pages để chia sẻ

1. Tạo repository mới trên https://github.com (vd: `shopee-ads-bi`). Có thể
   để **Public** vì không còn chứa key bí mật nào (chỉ có anon key công khai).
2. Upload file lên repo — **bắt buộc phải có `index.html` và `.nojekyll`**
   (2 file còn lại `supabase_schema.sql`, `README.md` tuỳ chọn, để tham
   khảo sau này):
   - Cách nhanh: vào repo trên GitHub → **Add file** → **Upload files** →
     kéo thả các file (nhớ chọn cả `.nojekyll`, đây là file ẩn nên phải
     bật hiện file ẩn trong File Explorer/Finder trước khi kéo-thả) →
     **Commit changes**.
   - Hoặc dùng Git:
     ```bash
     git init
     git add .
     git commit -m "Init Shopee Ads BI"
     git branch -M main
     git remote add origin https://github.com/<username>/shopee-ads-bi.git
     git push -u origin main
     ```
3. Bật GitHub Pages: vào repo → **Settings** → **Pages** (menu trái) →
   mục **Build and deployment** → **Source**: chọn **Deploy from a branch**
   → **Branch**: chọn `main` / `root` → **Save**.
4. Chờ 1-2 phút, GitHub hiển thị link dạng:
   `https://<username>.github.io/shopee-ads-bi/`
   → Đây là link Web App để chia sẻ, dùng trên mọi thiết bị có trình duyệt.

### Xử lý lỗi khi deploy GitHub Pages

**Lỗi `Error: Deployment failed, try again later.`** hoặc mục **"All
deployments"** hiện dấu ❌ đỏ ở `github-pages` — đây là lỗi hạ tầng/build
của GitHub, **không liên quan đến file dữ liệu hay code trong app**. Xử lý
theo thứ tự sau, làm hết cả 4 bước nếu bước trước chưa hết lỗi:

1. **Thêm file `.nojekyll` vào repo (nguyên nhân phổ biến nhất)**: mặc định
   GitHub Pages build site bằng Jekyll, đôi khi gây lỗi build không rõ ràng
   với site HTML/JS thuần như bộ này. Bộ 4 file đã có sẵn file rỗng
   `.nojekyll` — nhớ **upload cả file này lên repo cùng `index.html`** (ở
   GitHub web UI, bật hiện file ẩn hoặc kéo-thả cả file `.nojekyll` vào
   cùng lúc). File này báo cho GitHub bỏ qua bước build Jekyll, serve file
   tĩnh trực tiếp.
2. **Kiểm tra file nằm đúng ở gốc repo (root)**, không nằm trong thư mục
   con. Vào tab **Code** của repo, phải thấy `index.html` ngay ngoài cùng,
   không phải trong một folder con nào.
3. **Kiểm tra Source đang chọn "Deploy from a branch"**: **Settings →
   Pages → Build and deployment → Source** → chọn **Deploy from a branch**
   → **Branch**: `main` / `root` → **Save**.
4. **Nếu repo đang Private + tài khoản GitHub Free**: chuyển sang **Public**
   tại **Settings → General → Danger Zone → Change repository visibility**
   — GitHub Pages (kể cả 2 cách trên) đều không chạy được với repo Private
   trên gói Free. An toàn vì 4 file trong bộ này không chứa secret nào.
5. **Xem log lỗi chi tiết**: vào tab **Actions** hoặc bấm vào dòng deploy bị
   lỗi trong **"All deployments"** → xem dòng báo lỗi cụ thể để biết chính
   xác nguyên nhân nếu 4 bước trên chưa hết.

Sau khi sửa, vào lại **Settings → Pages**, đợi 1-2 phút và refresh — hoặc
tạo thêm 1 commit nhỏ bất kỳ (vd: sửa README) để kích hoạt build lại.

## BƯỚC 4 — Sử dụng

1. Mở link GitHub Pages → đăng nhập bằng email/mật khẩu đã tạo ở Bước 1.6.
2. Vào **Upload dữ liệu** → nhập tên Shop → kéo-thả file Excel/CSV đúng định
   dạng báo cáo Chiến dịch/Từ khóa-Vị trí Shopee → xem kết quả kiểm tra →
   bấm **Đồng bộ lên Supabase**.
3. Vào **Dashboard** → xem KPI, biểu đồ, lọc theo Ngày/Tuần/Tháng/Quý/Năm/
   Shop/Campaign/Sản phẩm.
4. Upload file tháng mới bất cứ lúc nào — dữ liệu trùng sẽ tự **cập nhật**,
   dữ liệu mới sẽ tự **thêm**, không bao giờ tạo trùng (xem cơ chế bên dưới).

---

## Cách hoạt động (tóm tắt)

- **Upload → Database**: `index.html` đọc file bằng SheetJS/PapaParse ngay
  trên trình duyệt, đối chiếu đủ 36 cột chuẩn Shopee, báo lỗi rõ dòng/cột
  nếu sai định dạng, rồi gửi thẳng lên Supabase qua `supabase-js`.
- **Chống trùng dữ liệu**: mỗi dòng có mã định danh (`row_key`) tính từ
  Shop + Tháng/Năm + Campaign + Sản phẩm + Nội dung + Vị trí + Từ khóa +
  Ngày bắt đầu. Trùng mã → **UPDATE**; chưa có → **INSERT**. Xem chi tiết
  trong `supabase_schema.sql`.
- **Dashboard**: đọc từ view `v_ads_computed` (đã tính sẵn CPC, CPM), tổng
  hợp KPI/biểu đồ ngay trên trình duyệt.
- **Đăng nhập**: dùng Supabase Auth (email/mật khẩu), dữ liệu được khoá bằng
  Row Level Security — chỉ user đã đăng nhập mới truy cập được. App **không**
  tự nhảy về Dashboard khi đang thao tác ở trang khác — chỉ chuyển trang khi
  bạn thật sự Đăng nhập/Đăng xuất (trước đây có lỗi: Supabase tự làm mới
  phiên đăng nhập ngầm định kỳ khiến app hiểu nhầm là vừa đăng nhập lại và
  tự kéo về Dashboard, làm mất trang đang sửa — đã sửa).
- **Biểu đồ đường (line chart)**: số liệu hiển thị **trực tiếp trên đường**,
  không cần rê chuột vào mới thấy (dùng `chartjs-plugin-datalabels`).

## Kế hoạch tháng (Media Plan) — chỉnh sửa trực tiếp, tự tính lại, tự lưu

Mục **📅 Kế hoạch tháng** dùng để nạp file kế hoạch quảng cáo dạng
`MediaPlan_ThangX_20XX.xlsx` (vd `MediaPlan_Thang7_2026.xlsx`) và biến nó
thành một kế hoạch **sống** ngay trên Web, không chỉ là bản xem tĩnh:

- **Đầy đủ chi tiết**: sau khi Upload, trang "Xem chi tiết" hiển thị Giả
  định/Benchmark (ngân sách, CPC/CTR/CR/ATC/AOV theo sản phẩm, trọng số theo
  Giai đoạn Sale/Pre-heat/Sustain/Normal), bảng phân bổ ngân sách **31 ngày**,
  và bảng phân bổ **theo khung giờ** cho ngày Big Sale/ngày thường.
- **Sửa trực tiếp**: mọi ô đều là input/select — đổi ngân sách, đổi Giai đoạn
  của 1 ngày cụ thể, đổi benchmark CPC/CTR/CR..., đổi tỷ lệ % ngân sách theo
  khung giờ... Sửa xong bấm Tab/click ra ngoài ô là xong, không cần nút Lưu.
- **Tự động tính lại**: các cột kết quả (Ngân sách/ngày, GMV kỳ vọng, Đơn
  hàng kỳ vọng, ROAS, CPC/CTR/CR bình quân...) dùng đúng công thức của file
  gốc (đã đối chiếu khớp từng đồng với file Excel mẫu) để tính lại ngay khi
  sửa bất kỳ thông số nào — giống cơ chế công thức trong Excel.
- **Tự động lưu**: khoảng 900ms sau lần sửa cuối, toàn bộ kế hoạch được lưu
  lên Supabase (trạng thái lưu hiển thị góc phải khung tiêu đề: "Có thay đổi
  chưa lưu" → "Đang lưu..." → "Đã lưu lúc ..."). Nếu lưu lỗi (vd mất mạng),
  sẽ có nút "Thử lại".
- File Excel gốc vẫn được lưu nguyên vẹn trong Supabase Storage (nút "⬇️ File
  gốc") để đối chiếu/tải về đầy đủ 14 sheet bất cứ lúc nào.
- **Bảng 31 ngày — sửa 2 chiều Trọng số ⇄ Tổng ngân sách**: sửa ô Trọng số thì
  Tổng ngân sách/ngày tự tính lại (như cũ), và ngược lại — gõ thẳng số tiền
  vào ô "Tổng NS (sửa được)" thì Trọng số tự suy ngược ra đúng giá trị đó.
- **Tỉ lệ % ngân sách theo Sản phẩm cho từng ngày**: mỗi cột sản phẩm trong
  bảng 31 ngày có 1 ô Tỉ lệ % (mặc định chia theo đúng tỷ lệ Ngân sách tháng
  đã cấu hình cho từng sản phẩm) — sửa riêng 1 ngày để lệch tỷ lệ so với mức
  mặc định (vd dồn 70% ngân sách hôm đó cho 1 sản phẩm), có nút ↺ để đặt lại
  tự động; không ảnh hưởng tỷ lệ của các ngày khác.
- **Lượt hiển thị / Lượt click**: đã bổ sung đầy đủ ở mọi bảng có số liệu ads
  (Tổng quan Kế hoạch, Mục tiêu theo Sản phẩm, Phân bổ theo Giai đoạn, bảng 31
  ngày, Kế hoạch vs Thực tế, và cả Dashboard/Phân tích ads).
- **Tỉ lệ % áp dụng cho cả tháng**: 2 ô Tỉ lệ % ngay trên tiêu đề cột NS của
  mỗi sản phẩm (trong bảng 31 ngày) — sửa 1 ô sẽ áp dụng tỉ lệ mới cho TẤT CẢ
  các ngày cùng lúc, có nút "↺ tất cả" để đặt lại mặc định.
- **🔒 Khóa ngày**: mỗi ngày trong bảng 31 ngày có 1 ô tick "Khóa" — tick vào
  sẽ CHỐT CỐ ĐỊNH Tổng ngân sách + Tỉ lệ hiện tại của ngày đó; từ lúc này mọi
  thao tác hàng loạt khác (sửa % Ngân sách theo Giai đoạn, sửa Tỉ lệ áp cho cả
  tháng...) sẽ **bỏ qua ngày đã khóa**, chỉ chia lại phần ngân sách còn lại
  cho các ngày chưa khóa. Bạn vẫn có thể tự sửa Tổng NS của 1 ngày đã khóa bất
  cứ lúc nào (số mới sẽ thay cho số cũ).
- **🎯 Mục tiêu / 💰 Thực tế sau điều chỉnh**: ô "Mục tiêu" là Tổng ngân sách
  tháng bạn muốn (sửa được, tự chia lại theo đúng tỷ lệ giữa các sản phẩm). Ô
  "Thực tế sau điều chỉnh" **tự động cộng lại** đúng cột "Tổng NS" của bảng 31
  ngày bên dưới (kể cả các ngày đã Khóa) — nếu các ngày đã Khóa/nhập tay cộng
  lại VƯỢT hoặc CHƯA DÙNG HẾT so với Mục tiêu, hệ thống sẽ hiện cảnh báo màu đỏ
  (vượt)/vàng (còn dư) thay vì âm thầm "ép" khớp số, để bạn biết chính xác và
  tự điều chỉnh lại các ngày cho phù hợp.

**Nếu bạn đã tạo Database trước khi có tính năng này**: mở lại Supabase → SQL
Editor → chạy lại toàn bộ `supabase_schema.sql` **một lần nữa** (an toàn,
không mất dữ liệu cũ) để bổ sung 3 cột mới (`assumptions`, `daily_plan`,
`hourly_plan`) cho bảng `media_plans`. Các Kế hoạch đã lưu từ trước sẽ hiện
cảnh báo "phiên bản cũ" — chỉ cần Upload lại đúng file Excel gốc để nâng cấp.

## Phân tích ads (theo ngày thật) — so sánh hiệu quả từng ngày + đối chiếu Kế hoạch

Mục **📈 Phân tích ads** dùng file báo cáo Ads có cột **Ngày** thật (khác với
Upload dữ liệu ở mục trên vốn chỉ có độ chi tiết theo Tháng), ví dụ file
`Data ad ThángX.xlsx`: giữ nguyên 36 cột chuẩn báo cáo Chiến dịch/Từ khóa-Vị
trí Shopee, chỉ thêm 1 cột **Ngày** ở đầu. Mỗi tháng chỉ cần dán thêm dữ liệu
mới xuống dưới file gốc rồi Upload lại — hệ thống tự thêm mới/không trùng lặp
(lưu vào bảng riêng `ads_daily_reports`, không lẫn với dữ liệu Upload theo
tháng ở mục trên).

Sau khi Upload, chọn Shop/Tháng/Năm rồi bấm "🔄 Xem phân tích" để thấy:

- **KPI tổng quan + biểu đồ xu hướng theo ngày** (Chi phí, Doanh số, ROAS).
- **Bảng so sánh hiệu quả giữa các ngày**: mỗi dòng 1 ngày với đầy đủ
  Chi phí/Doanh số/ROAS/ACOS/CTR/CR/CPC/CPA/Đơn hàng, kèm % thay đổi ROAS so
  với ngày liền trước và **lời khuyên tự động** dựa theo chuẩn ngành (vd
  "ROAS thấp — cân nhắc loại bớt từ khóa/vị trí kém hiệu quả"). Cuối bảng có
  **hàng TOTAL**: Chi phí/Doanh số/Lượt hiển thị/Lượt click/Đơn hàng là
  **TỔNG** cả kỳ, các chỉ số còn lại (ROAS/ACOS/CTR/CR/CPC/CPA) là **TRUNG
  BÌNH** cả kỳ.
- **📊 Tóm tắt & xu hướng nhanh** (ngay dưới bảng so sánh ngày): 6 thẻ số liệu
  gọn (Tổng Chi phí, Tổng Doanh số, ROAS trung bình, Tổng Đơn hàng, CTR trung
  bình, CPC trung bình) + 1 biểu đồ kết hợp cột (Chi phí/Doanh số) và đường
  (ROAS) theo ngày, giúp nhìn tổng quan nhanh mà không cần cuộn lên đầu trang.
- **📅 Phân bổ ngân sách theo tuần: Kế hoạch vs Thực tế**: gộp Kế hoạch tháng
  đã lưu theo từng tuần lịch (Thứ 2 → Chủ nhật), so với Chi phí/Doanh số thực
  tế đã chạy trong tuần đó để biết **% đạt mục tiêu tuần** (≥95% xanh, 70–95%
  vàng, &lt;70% đỏ) — không có cột "ngân sách live/chi phí live" (bảng plan
  gốc không dùng loại hình quảng cáo Live).
- **Kế hoạch vs Thực tế theo ngày**: đối chiếu với Kế hoạch tháng (mục 📅) đã
  lưu cho cùng Shop/Tháng/Năm. Mỗi ngày có 1 ô **ROAS tham chiếu** để tự nhập
  — nhập vào, hệ thống tự tính ra kịch bản "kỳ vọng" cho CPC/CTR/CR/ATC/CPA/
  Đơn hàng/GMV (giữ nguyên CPC/CTR/ATC/AOV theo chuẩn Giai đoạn trong Kế
  hoạch, áp đúng trên Ngân sách **thực chi** của ngày đó — không phải ngân
  sách kế hoạch, để so sánh đúng "táo với táo") và hiển thị song song với số
  liệu Thực tế đã đạt. Giá trị ROAS tham chiếu được lưu riêng theo Shop/Ngày
  (bảng `ads_daily_roas_ref`), không ảnh hưởng tới Kế hoạch tháng gốc.

**Nếu bạn đã tạo Database trước khi có tính năng này**: chạy lại toàn bộ
`supabase_schema.sql` một lần nữa trong Supabase SQL Editor (an toàn, không
mất dữ liệu cũ) để tạo 2 bảng mới `ads_daily_reports` và `ads_daily_roas_ref`.

## Đổi màu thương hiệu (Logo)

Khi có Logo công ty, mở `index.html`, tìm khối `tailwind.config` ở đầu file
(mục `colors.brand`) và đổi các mã màu `#EE4D2D...` sang bộ màu thương hiệu
— toàn bộ giao diện (nút, biểu đồ, số liệu nổi bật) sẽ tự cập nhật theo.

## Giới hạn dữ liệu hiện tại (sẽ cập nhật khi có thêm Format Data)

- File nguồn hiện tại (`Data base.xlsx`) có độ chi tiết theo **Tháng**
  (không có số liệu theo từng ngày). Bộ lọc Tháng/Quý/Năm chính xác 100%;
  bộ lọc Ngày/Tuần dùng "Ngày bắt đầu" chiến dịch làm mốc tham chiếu.
- Chỉ số **Add To Cart** chưa có trong báo cáo Chiến dịch/Từ khóa-Vị trí —
  hiển thị "—" trên Dashboard, sẽ có dữ liệu khi bạn cung cấp báo cáo Shopee
  khác chứa chỉ số này (chỉ cần mở rộng, không phá vỡ cấu trúc hiện tại).
- Cột **Shop** không có trong file gốc — người dùng khai báo tên Shop ngay
  lúc Upload để hỗ trợ quản lý nhiều Shop.

## Chạy thử ở máy local (tuỳ chọn, không bắt buộc)

Có thể mở trực tiếp `index.html` bằng cách double-click (một số trình duyệt
có thể chặn gọi API do chính sách bảo mật file `file://`). Nếu gặp lỗi, chạy
một server tĩnh đơn giản rồi mở `http://localhost:8000`:

```bash
python -m http.server 8000
# hoặc: npx serve .
```

Cách khuyến nghị vẫn là dùng link GitHub Pages ở Bước 3 — ổn định và chia sẻ
được cho cả team.
