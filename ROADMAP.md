# ROADMAP — Chúng ta đang build gì

> File này là "bản đồ". AI agent đọc file này đầu tiên để hiểu mục tiêu và các bước.
> Khi xong một mục, đổi `[ ]` thành `[x]`. Khi thay đổi hướng đi, ghi lý do vào `DECISIONS.md`.

## Mục tiêu

Một website cá nhân tên **Hear Me Singing** để trưng bày các bài hát tôi tự thu âm (ballad, indie, jazz).
Link website sẽ được gắn lên trang Facebook của tôi để người khác vào nghe.

- Người xem: không cần đăng nhập, chỉ cần mở link là nghe được. Phần lớn mở từ điện thoại (trình duyệt trong app Facebook).
- Ngôn ngữ: song ngữ Việt - Anh. Mặc định tiếng Việt, người xem chuyển sang tiếng Anh bằng nút ở đầu trang.
- Chủ trang (tôi): đăng nhập để upload, sửa, xóa bài hát, và chỉnh tên trang, dòng giới thiệu, ảnh chia sẻ lên Facebook, tất cả mà không cần sửa code hay deploy lại.

## Tech stack

- Next.js (App Router) + Tailwind CSS
- Supabase: Storage (public bucket `songs`), Database (bảng `songs`), Auth (một tài khoản admin)
- Deploy: Vercel

## Cấu trúc trang

| Route | Ai vào được | Chức năng |
|-------|-------------|-----------|
| `/` | Mọi người | Danh sách bài hát + trình phát nhạc (tiếng Việt, mặc định) |
| `/en` | Mọi người | Giống `/` nhưng giao diện tiếng Anh |
| `/admin` | Chỉ tôi (đăng nhập) | Quản lý bài hát (upload MP3, tên bài, ngày thu, ghi chú, sửa, xóa) và Thông tin trang (tên trang, giới thiệu, ảnh bìa, thông tin chia sẻ Facebook) |

## Dữ liệu

### Bảng `songs` (mỗi bài hát một dòng)

| Trường | Ghi chú |
|--------|---------|
| `id` | Khóa chính |
| `title` | Tên bài, hiện ở danh sách |
| `recorded_at` | Ngày thu, hiện bên phải mỗi dòng |
| `note` | Ghi chú ngắn, không bắt buộc |
| `audio_path` | Đường dẫn file MP3 hoặc M4A trong bucket `songs` (tên file lưu là ID ngẫu nhiên giữ nguyên đuôi `.mp3` / `.m4a`, không dùng tên bài) |
| `created_at` | Thời điểm tạo |

### Bảng `site_settings` (chỉ có đúng MỘT dòng)

| Trường | Hiện ở đâu | Bắt buộc |
|--------|-----------|----------|
| `site_title` | Tên trang, chữ serif lớn ở đầu trang | Có |
| `intro_vi` | 1-2 dòng giới thiệu dưới tên trang (bản tiếng Việt) | Có |
| `intro_en` | Dòng giới thiệu bản tiếng Anh. Để trống thì trang `/en` dùng `intro_vi` | Không (nên có) |
| `cover_image_path` | Ảnh bìa nhỏ vuông ở đầu trang. Chỉ hiện khi có ảnh | Không |
| `og_title` | Tiêu đề thẻ xem trước khi chia sẻ link (Facebook, Zalo, Messenger). Để trống thì dùng `site_title` | Không |
| `og_description_vi` | Mô tả trong thẻ xem trước của link `/`. Để trống thì dùng `intro_vi` | Không |
| `og_description_en` | Mô tả trong thẻ xem trước của link `/en`. Để trống thì dùng `intro_en`, rồi đến `intro_vi` | Không |
| `og_image_path` | Ảnh chia sẻ, tỷ lệ ngang ~1200×630px | Không (nên có) |

### Storage
- Bucket `songs` (public): file MP3 và M4A. Cấu hình `allowed_mime_types` (`audio/mpeg`, `audio/mp4`, `audio/x-m4a`, `audio/m4a`) và `file_size_limit` 15MB ngay trên Supabase
- Bucket `site-assets` (public): ảnh bìa và ảnh chia sẻ

### Phân quyền
Cả hai bảng và cả hai bucket: ai cũng ĐỌC được, chỉ tài khoản admin được GHI (bằng RLS và storage policy).

### Admin sửa được những gì

| Nhóm | Việc làm được |
|------|---------------|
| Bài hát | Upload / thay file MP3 hoặc M4A (tối đa 15MB), sửa tên, ngày thu, ghi chú, xóa bài |
| Thông tin trang | Sửa tên trang, dòng giới thiệu (tiếng Việt và tiếng Anh), ảnh bìa, tiêu đề / mô tả (hai ngôn ngữ) / ảnh khi chia sẻ |

## Phong cách thiết kế

Cảm giác: tĩnh, ấm, riêng tư, như nghe nhạc một mình vào buổi tối. Tối giản kiểu mục lục đĩa nhạc / trang tạp chí in.

- Một cột hẹp (max-width ~640px), căn lề trái, nhiều khoảng trắng
- Font tiêu đề: Newsreader (serif). Font nội dung: DM Sans. Bắt buộc nạp subset `vietnamese`
- Màu (sáng): nền `#F5F3EE`, chữ `#1A1A1A`, chữ phụ `#8A8680`, nhấn `#8C3B2A`
- Màu (tối): nền `#151412`, chữ `#EAE6DE`, nhấn `#D08A6E`
- Danh sách bài hát: dòng kẻ mảnh 1px, KHÔNG dùng card
- Màu nhấn chỉ dùng cho bài đang phát và hover

### Tuyệt đối tránh
- Gradient, glassmorphism, blur
- Emoji hoặc icon trang trí
- Card đổ bóng, bo góc lớn
- Nền tím/xanh, font Inter/Roboto
- Section kiểu "Features", "Why choose us", "Welcome to..."
- Nội dung placeholder (Song Title 1, lorem ipsum)
- Animation hoa mỹ khi cuộn trang

## Các giai đoạn

Làm lần lượt, dừng sau mỗi giai đoạn để tôi xem và nhận xét.

### Giai đoạn 1 — Khung trang chính
- [ ] Khởi tạo project Next.js + Tailwind
- [ ] Nạp font Newsreader + DM Sans (có subset vietnamese)
- [ ] Thiết lập biến màu (sáng + tối)
- [ ] Thiết lập song ngữ: route `/` (tiếng Việt) và `/en` (tiếng Anh) dùng chung một component nhận tham số `lang`. Chuỗi giao diện đặt trong file từ điển `vi` / `en`, không viết cứng trong component
- [ ] Nút chuyển ngôn ngữ "VI / EN" ở đầu trang, chỉ dùng chữ (ngôn ngữ đang chọn dùng màu chữ chính, ngôn ngữ còn lại màu xám)
- [ ] Ngày hiển thị theo ngôn ngữ (`12.10.2026` cho tiếng Việt, `12 Oct 2026` cho tiếng Anh)
- [ ] Phần đầu trang: tên trang "Hear Me Singing" + 1-2 dòng giới thiệu. Đọc từ một object cấu hình có cùng hình dạng với `site_settings` (tạm viết cứng trong code) để sau này đổi sang đọc từ Supabase rất dễ
- [ ] Danh sách bài hát dạng mục lục (dùng dữ liệu mẫu thật, không dùng placeholder)

### Giai đoạn 2 — Trình phát nhạc
- [ ] Thanh player cố định ở đáy (~56px)
- [ ] Nút play/pause SVG nét mảnh, tên bài, thanh tiến trình 2px, thời gian
- [ ] Nhãn truy cập (aria-label) của các nút lấy từ từ điển theo ngôn ngữ
- [ ] Đặt player trong layout dùng chung của `/` và `/en`, để chuyển ngôn ngữ không làm ngắt bài đang phát
- [ ] Bấm vào thanh tiến trình để tua
- [ ] Tự chuyển bài tiếp theo khi hết bài
- [ ] Chỉ phát một bài tại một thời điểm
- [ ] `preload="none"`, tự viết điều khiển trên thẻ `<audio>` ẩn

### Giai đoạn 3 — Kết nối Supabase
- [ ] Tạo project Supabase, bucket `songs` và bucket `site-assets` (đều public)
- [ ] Tạo bảng `songs` và bảng `site_settings` (một dòng, điền sẵn `site_title` = "Hear Me Singing", `intro_vi`, `intro_en` và nội dung ban đầu)
- [ ] Row Level Security + storage policy: ai cũng đọc được, chỉ admin được ghi
- [ ] Cấu hình bucket `songs`: chỉ cho phép MIME audio của MP3 và M4A, giới hạn 15MB
- [ ] Trang chủ lấy danh sách bài từ `songs`, tên trang / giới thiệu / ảnh bìa từ `site_settings`

### Giai đoạn 4 — Trang /admin
- [ ] Đăng nhập (một tài khoản)
- [ ] Form upload (kéo thả hoặc chọn file), nhập tên bài / ngày thu / ghi chú
- [ ] Kiểm tra định dạng và dung lượng ở giao diện: chỉ nhận `.mp3` và `.m4a`, tối đa 15MB. File sai thì hiện: "Chỉ nhận MP3 hoặc M4A, tối đa 15MB."
- [ ] Khi upload đặt đúng `contentType` (`audio/mpeg` cho MP3, `audio/mp4` cho M4A) và đặt tên file lưu bằng ID ngẫu nhiên giữ nguyên đuôi gốc
- [ ] Kiểm tra: upload thử file WAV / OGG / file quá 15MB phải bị từ chối (cả ở giao diện lẫn khi gọi API trực tiếp)
- [ ] Sửa và xóa bài
- [ ] Mục "Thông tin trang": form sửa tên trang, giới thiệu (`intro_vi`, `intro_en`), ảnh bìa (tùy chọn), tiêu đề chia sẻ, mô tả chia sẻ (`og_description_vi`, `og_description_en`), ảnh chia sẻ
- [ ] Giao diện trang admin chỉ dùng tiếng Việt
- [ ] Upload ảnh vào bucket `site-assets`, có xem trước ảnh và gợi ý kích thước (ảnh chia sẻ ~1200×630px)
- [ ] Thông báo rõ ràng khi thành công hoặc lỗi
- [ ] Kiểm tra: người chưa đăng nhập KHÔNG thể ghi (thử trực tiếp bằng API, không chỉ ẩn nút)

### Giai đoạn 5 — Hoàn thiện và deploy
- [ ] Open Graph metadata lấy từ `site_settings` theo từng ngôn ngữ (dùng `generateMetadata` của Next.js, truyền `lang` vào), có giá trị dự phòng khi trường để trống
- [ ] Mỗi bản ngôn ngữ có `<html lang>` đúng, thẻ `hreflang` trỏ qua lại giữa `/` và `/en`, và `og:locale` tương ứng (`vi_VN`, `en_US`)
- [ ] Đảm bảo đổi thông tin ở admin thì metadata được làm mới (revalidate), không bị kẹt cache cũ
- [ ] Kiểm tra thẻ xem trước bằng Sharing Debugger của Facebook, cho cả `/` và `/en`
- [ ] Dark mode
- [ ] Test trên điện thoại thật, kể cả trình duyệt trong app Facebook
- [ ] Test phát CẢ file MP3 và file M4A trên iPhone Safari, Android Chrome và trình duyệt trong app Facebook
- [ ] Test cả `/` và `/en`: không còn chuỗi tiếng Việt lọt vào bản tiếng Anh, bố cục không vỡ, bấm chuyển ngôn ngữ không làm ngắt nhạc đang phát
- [ ] Deploy lên Vercel
- [ ] Gắn link vào trang Facebook

## Ý tưởng cho sau này (chưa làm)

- Ảnh bìa riêng cho từng bài
- Ẩn/hiện bài (bản nháp) và kéo thả đổi thứ tự
- Link riêng cho từng bài để chia sẻ
- Lời bài hát / hợp âm hiển thị cùng bài
- Thống kê lượt nghe đơn giản

## Ngoài phạm vi

- Bình luận, like, đăng nhập cho người xem
- Nhận WAV / FLAC / OGG và tự động chuyển đổi định dạng
- Tự nhận diện ngôn ngữ trình duyệt để chuyển hướng (mặc định luôn là tiếng Việt)
- Dịch tên bài hát và ghi chú từng bài (giữ nguyên bản)
- Giao diện admin song ngữ (admin chỉ dùng tiếng Việt)
- Tải nhạc xuống, bán nhạc
