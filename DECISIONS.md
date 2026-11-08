# DECISIONS — Vì sao chọn X

> Ghi lại mỗi quyết định quan trọng để sau này (hoặc AI agent ở phiên mới) không hỏi lại và không vô tình làm ngược lại.
> Quyết định mới nhất để ở trên cùng. Không xóa quyết định cũ, nếu đổi ý thì thêm quyết định mới và đánh dấu cái cũ là "Đã thay thế".

## Mẫu

```
### D-XXX — Tiêu đề ngắn
- Ngày: YYYY-MM-DD
- Trạng thái: Đang áp dụng / Đã thay thế bởi D-YYY
- Quyết định: ...
- Lý do: ...
- Đã cân nhắc nhưng loại bỏ: ...
- Hệ quả / điều cần nhớ: ...
```

---

### D-013 — Giao diện song ngữ Việt - Anh, mặc định tiếng Việt
- Ngày: 2026-11-08
- Trạng thái: Đang áp dụng
- Quyết định: Trang có hai phiên bản ngôn ngữ: `/` là tiếng Việt (mặc định) và `/en` là tiếng Anh. Có nút chuyển "VI / EN" ở đầu trang, chỉ dùng chữ (không cờ, không emoji). Trang `/admin` chỉ dùng tiếng Việt.
- Lý do: Tên trang là tiếng Anh nhưng người xem chủ yếu là bạn bè người Việt, đồng thời vẫn muốn người nói tiếng Anh đọc hiểu được. Dùng hai URL riêng vì Facebook đọc thẻ chia sẻ theo từng URL, nên mỗi ngôn ngữ có tiêu đề và mô tả riêng.
- Đã cân nhắc nhưng loại bỏ:
  - Một URL, nút chuyển lưu bằng cookie: thẻ chia sẻ chỉ có được một ngôn ngữ.
  - Tự nhận diện ngôn ngữ trình duyệt rồi chuyển hướng: trái với yêu cầu mặc định luôn là tiếng Việt.
  - Chỉ một ngôn ngữ.
- Hệ quả / điều cần nhớ:
  - Cái gì dịch, cái gì không:
    - Dịch: nhãn giao diện, nhãn truy cập (aria-label) của player, dòng giới thiệu, mô tả khi chia sẻ link, định dạng ngày.
    - Không dịch: tên trang "Hear Me Singing", tên bài hát, ghi chú từng bài (hiện nguyên văn ở cả hai bản).
  - Chuỗi giao diện đặt trong file từ điển `vi` / `en`, không viết cứng trong component. Hai route dùng chung một component nhận tham số `lang`.
  - Thay đổi `site_settings` so với D-010: `intro` tách thành `intro_vi` (bắt buộc) và `intro_en` (không bắt buộc); `og_description` tách thành `og_description_vi` và `og_description_en`. Trường nào của tiếng Anh để trống thì dùng bản tiếng Việt.
  - Ngày hiển thị theo ngôn ngữ: tiếng Việt `12.10.2026`, tiếng Anh `12 Oct 2026`.
  - Mỗi bản ngôn ngữ phải có `<html lang>` đúng, thẻ `hreflang` trỏ qua lại, và `og:locale` tương ứng (`vi_VN`, `en_US`).
  - Link gắn lên Facebook là bản tiếng Việt (`/`). Facebook cache từng URL riêng nên phải làm mới bằng Sharing Debugger cho cả `/` và `/en`.
  - Trình phát nhạc phải nằm trong layout dùng chung của hai route, để bấm chuyển ngôn ngữ không làm ngắt bài đang phát.

### D-012 — Tên trang: "Hear Me Singing"
- Ngày: 2026-11-08
- Trạng thái: Đang áp dụng
- Quyết định: Tên trang là **Hear Me Singing** (3 chữ, viết hoa chữ cái đầu mỗi chữ). Dùng làm `site_title`, thẻ `<title>`, và là giá trị dự phòng cho `og_title` khi chia sẻ link.
- Lý do: Nghe như một lời mời trực tiếp ("hãy nghe tôi hát"), đúng nội dung trang là các bài hát tự thu âm, dễ nhớ và dễ đọc to.
- Đã cân nhắc nhưng loại bỏ: Hát thầm, Ngân nga, Late Takes, Quiet Hours, Lamplight, Rough Cuts, Margin Notes và một số tên khác.
- Hệ quả / điều cần nhớ:
  - Luôn viết đúng "Hear Me Singing", không viết hoa toàn bộ và không viết liền.
  - Tên là tiếng Anh, giữ nguyên ở cả hai phiên bản ngôn ngữ (không dịch). Việc chọn ngôn ngữ giao diện đã giải quyết ở D-013.
  - Nên đặt tên trang Facebook trùng hoặc gần với tên này để người xem nhận ra ngay.
  - Chữ serif lớn ở đầu trang: trên điện thoại màn hình hẹp, tên có thể xuống dòng, cần kiểm tra giao diện.
  - Tên miền chưa chốt. Nếu mua miền riêng, tên không dấu và không khoảng trắng, ví dụ `hearmesinging` (kiểm tra còn trống chưa).

### D-011 — Cho phép upload MP3 và M4A
- Ngày: 2026-11-08
- Trạng thái: Đang áp dụng (thay thế D-003)
- Quyết định: Trang admin chấp nhận file `.mp3` và `.m4a`. Giới hạn tối đa 15MB mỗi file. Các định dạng khác (WAV, FLAC, OGG...) bị từ chối kèm thông báo: "Chỉ nhận MP3 hoặc M4A, tối đa 15MB."
- Lý do: M4A (AAC) là định dạng mặc định của Voice Memos trên iPhone và nhiều ứng dụng thu âm trên điện thoại. Cho phép M4A giúp khỏi phải chuyển đổi, trong khi vẫn nhẹ và nghe được trên mọi thiết bị.
- Đã cân nhắc nhưng loại bỏ:
  - Chỉ nhận MP3: đơn giản nhất nhưng phải tự chuyển file M4A trước khi upload.
  - Nhận thêm WAV: file 30-50MB mỗi bài, trang tải chậm và tốn băng thông.
  - Nhận thêm OGG / FLAC: Safari trên iPhone hỗ trợ kém, người xem từ Facebook trên iPhone có thể không nghe được.
- Hệ quả / điều cần nhớ:
  - Kiểm tra định dạng ở HAI nơi: giao diện admin (thông báo lỗi dễ hiểu) và cấu hình bucket `songs` trên Supabase (`allowed_mime_types` và `file_size_limit`), vì kiểm tra ở giao diện không ngăn được người gọi API trực tiếp.
  - MIME type của M4A không nhất quán giữa các thiết bị (`audio/mp4`, `audio/x-m4a`, `audio/m4a`). Bucket phải cho phép cả các giá trị này, cùng `audio/mpeg` cho MP3.
  - Khi upload phải đặt `contentType` đúng cho file. Nếu sai, Supabase có thể phục vụ file dưới dạng `application/octet-stream` và Safari không phát được.
  - Tên file lưu trữ là ID ngẫu nhiên giữ nguyên đuôi gốc (`<id>.mp3` hoặc `<id>.m4a`). Tên bài gốc nằm trong bảng `songs`.
  - Khuyến nghị bitrate 128-192 kbps cho cả hai định dạng (khoảng 3-7MB mỗi bài).
  - Phải test phát CẢ HAI định dạng trên iPhone Safari và trình duyệt trong app Facebook.

### D-010 — Thêm bảng `site_settings` để admin tự sửa thông tin trang
- Ngày: 2026-11-08
- Trạng thái: Đang áp dụng
- Quyết định: Tạo bảng `site_settings` (chỉ một dòng) trong Supabase chứa tên trang, dòng giới thiệu, ảnh bìa nhỏ (tùy chọn), và thông tin Open Graph (tiêu đề, mô tả, ảnh chia sẻ). Trang `/admin` có thêm mục "Thông tin trang" để sửa các trường này. Ảnh lưu ở bucket public riêng `site-assets`.
- Lý do: Nếu viết cứng trong code thì mỗi lần đổi tên trang, câu giới thiệu hay ảnh chia sẻ đều phải nhờ AI sửa code và deploy lại.
- Đã cân nhắc nhưng loại bỏ:
  - Viết cứng trong code: đơn giản hơn lúc đầu nhưng bất tiện lâu dài.
  - Đưa chung vào bảng `songs`: sai bản chất, đây là cấu hình của cả trang chứ không phải của một bài.
- Hệ quả / điều cần nhớ:
  - Chỉ có đúng một dòng trong `site_settings`. Admin chỉ sửa dòng đó, không tạo thêm.
  - Open Graph phải lấy từ `site_settings` qua `generateMetadata`, và phải có giá trị dự phòng: `og_title` trống thì dùng `site_title`, `og_description` trống thì dùng `intro`. (Hai trường `intro` và `og_description` sau đó được tách theo ngôn ngữ, xem D-013.)
  - Facebook cache thẻ xem trước: sau khi đổi ảnh hoặc tiêu đề, phải dùng Sharing Debugger để làm mới (xem `BUGS.md`).
  - Ảnh bìa là tùy chọn: không có ảnh thì không hiện gì, không để khung trống.
  - Ở Giai đoạn 1, dùng một object cấu hình tạm trong code có cùng hình dạng với `site_settings` để chuyển sang Supabase ở Giai đoạn 3 không phải sửa giao diện.

### D-009 — Trình phát nhạc tự viết, không dùng player mặc định
- Ngày: 2026-11-08
- Trạng thái: Đang áp dụng
- Quyết định: Dùng thẻ `<audio>` ẩn và tự viết giao diện điều khiển (play/pause, tiến trình, thời gian).
- Lý do: Player mặc định của trình duyệt không thể tùy biến, sẽ phá vỡ phong cách tối giản.
- Hệ quả: Phải tự xử lý tua, chuyển bài, trạng thái đang phát. Cần test kỹ trên iOS Safari.

### D-008 — Phân quyền ở phía Supabase, không chỉ ẩn nút trên giao diện
- Ngày: 2026-11-08
- Trạng thái: Đang áp dụng
- Quyết định: Dùng Row Level Security cho bảng `songs` và storage policy cho bucket `songs`. Ai cũng được đọc, chỉ tài khoản admin được ghi.
- Lý do: Ẩn nút upload trên UI không ngăn được người khác gọi API trực tiếp.
- Hệ quả: Sau khi làm xong admin, phải thử ghi dữ liệu khi chưa đăng nhập để chắc chắn bị chặn.

### D-007 — Gắn link lên Facebook, không nhúng website vào trang Facebook
- Ngày: 2026-11-08
- Trạng thái: Đang áp dụng
- Quyết định: Đặt link website vào phần giới thiệu / nút của trang Facebook. Thêm Open Graph metadata để link hiện đẹp khi chia sẻ.
- Lý do: Trang Facebook không nhúng trực tiếp được website bên ngoài.
- Hệ quả: Người xem thường mở bằng trình duyệt trong app Facebook, nên ưu tiên mobile và phải test trong đó.

### D-006 — Phong cách thiết kế: tối giản kiểu mục lục đĩa nhạc / tạp chí in
- Ngày: 2026-11-08
- Trạng thái: Đang áp dụng
- Quyết định: Một cột hẹp, căn trái, font serif Newsreader + DM Sans, bảng màu 3-4 mã hex cố định, danh sách dạng dòng kẻ.
- Lý do: Hợp với ballad / indie / jazz, và tránh giao diện "AI-generated" mặc định (gradient tím, Inter, card bóng đổ).
- Đã cân nhắc nhưng loại bỏ: Landing page kiểu SaaS; phong cách vintage cassette (quá nhiều chi tiết trang trí so với yêu cầu tối giản).
- Hệ quả: Mọi thay đổi UI phải tuân theo danh sách "Tuyệt đối tránh" trong `ROADMAP.md`.

### D-005 — Một website, hai route (`/` và `/admin`)
- Ngày: 2026-11-08
- Trạng thái: Đang áp dụng
- Quyết định: Trang công khai ở `/`, trang quản lý có đăng nhập ở `/admin`, cùng một project và cùng một database.
- Lý do: Upload ở admin thì bài hát tự xuất hiện ở trang chính, không cần deploy lại.
- Đã cân nhắc nhưng loại bỏ: Gộp thành một trang có nút Upload hiện khi đăng nhập (khó kiểm soát hơn với người mới).

### D-004 — Không dùng Google Drive / Dropbox để phát nhạc
- Ngày: 2026-11-08
- Trạng thái: Đang áp dụng
- Quyết định: Không dùng link chia sẻ của Google Drive hoặc Dropbox làm nguồn audio.
- Lý do: Hay bị giới hạn lượt truy cập và lỗi khi stream.

### D-003 — Định dạng audio: MP3 128-192 kbps
- Ngày: 2026-11-08
- Trạng thái: Đã thay thế bởi D-011 (nay nhận cả MP3 và M4A). Phần lý do về việc tránh WAV vẫn còn đúng.
- Quyết định: Chuyển file sang MP3 128-192 kbps trước khi upload (khoảng 3-5MB mỗi bài).
- Lý do: WAV rất nặng, trang tải chậm và tốn băng thông, nhất là với người xem trên điện thoại.
- Hệ quả: Nên ghi chú ngay trong trang admin về định dạng được nhận và đặt giới hạn dung lượng file (xem D-011).

### D-002 — Tech stack: Next.js + Tailwind + Vercel
- Ngày: 2026-11-08
- Trạng thái: Đang áp dụng
- Quyết định: Next.js (App Router), Tailwind CSS, deploy trên Vercel.
- Lý do: AI agent viết code cho bộ này rất chuẩn, deploy đơn giản, có gói miễn phí.

### D-001 — Lưu file audio và dữ liệu bằng Supabase
- Ngày: 2026-11-08
- Trạng thái: Đang áp dụng
- Quyết định: Supabase Storage (public bucket) cho file MP3, Supabase Database cho danh sách bài, Supabase Auth cho đăng nhập admin.
- Lý do: Một dịch vụ lo đủ ba việc (lưu file, dữ liệu, đăng nhập), ai cũng nghe được, tôi tự upload được.
- Đã cân nhắc nhưng loại bỏ:
  - Để file trong thư mục `public/` của project: phải deploy lại mỗi lần có bài mới, repo phình to.
  - Cloudflare R2: free tier tốt và không tính phí băng thông, nhưng phải tự ghép thêm database và auth.
  - SoundCloud / YouTube embed: phụ thuộc giao diện player của họ, không giữ được phong cách riêng.
- Hệ quả: Nếu sau này lượng nghe lớn, có thể chuyển file audio sang Cloudflare R2 để tiết kiệm băng thông.

---

## Câu hỏi còn mở

- [ ] Ảnh chia sẻ Facebook: dùng ảnh tôi chụp hay tạo ảnh chữ serif trên nền kem đúng bảng màu của trang?
- [ ] Có dùng ảnh bìa nhỏ ở đầu trang ngay từ đầu không, hay để trống đến khi có ảnh?
- [ ] Chủ đề mặc định: sáng (`#F5F3EE`) hay tối (`#151412`)? (đang để hệ thống tự theo thiết bị)
- [x] Tên trang: Hear Me Singing (đã chốt, xem D-012)
- [ ] Tên miền: mua miền riêng (ví dụ `hearmesinging.com`) hay dùng địa chỉ miễn phí của Vercel (`.vercel.app`)?
- [x] Ngôn ngữ giao diện: song ngữ Việt - Anh, mặc định tiếng Việt (đã chốt, xem D-013)
- [ ] Có bài nào là cover bài của người khác không? Nếu có, cân nhắc vấn đề bản quyền khi chia sẻ công khai.
