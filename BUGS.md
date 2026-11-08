# BUGS — Những gì đã hỏng trước đây

> Mục đích: không mất công debug lại cùng một lỗi. Mỗi khi sửa xong một lỗi mất hơn ~10 phút, ghi vào đây.
> AI agent: trước khi debug một lỗi mới, hãy đọc file này xem đã từng gặp chưa.
> Lỗi mới nhất để ở trên cùng.

## Mẫu

```
### B-XXX — Mô tả ngắn lỗi
- Ngày: YYYY-MM-DD
- Trạng thái: Đã sửa / Đang mở / Tạm né
- Triệu chứng: Thấy gì xảy ra? (kèm thông báo lỗi nguyên văn nếu có)
- Chỗ xảy ra: Trang nào, thiết bị / trình duyệt nào, file nào
- Nguyên nhân gốc: Vì sao lỗi xảy ra
- Cách sửa: Đã làm gì (nêu file / dòng liên quan)
- Cách tránh lặp lại: Quy tắc rút ra
```

---

## Lỗi đã gặp

_(Chưa có lỗi nào. Thêm vào đây khi gặp.)_

---

## Điểm dễ gặp lỗi (chưa xảy ra, chỉ là cần để ý)

Những chỗ dự án kiểu này hay gặp vấn đề. Khi thực sự gặp, chuyển thành mục `B-XXX` ở trên.

- **Tự phát nhạc trên điện thoại**: iOS Safari và trình duyệt trong app Facebook chặn audio tự phát. Âm thanh chỉ phát được sau khi người dùng chạm vào nút play.
- **Audio không tua được**: thường do file hoặc server không hỗ trợ range request. Cần test tua trên bản deploy thật chứ không chỉ ở máy local.
- **Mất dấu tiếng Việt**: quên nạp subset `vietnamese` của Google Fonts, chữ có dấu sẽ rơi sang font khác.
- **RLS chặn nhầm**: bật Row Level Security mà quên viết policy cho quyền đọc, trang chính sẽ trống dù database có dữ liệu.
- **Bucket không public**: file upload được nhưng URL trả về 400/403, người xem không nghe được.
- **Upload file quá lớn**: Supabase và Vercel đều có giới hạn dung lượng. Upload thẳng từ trình duyệt lên Supabase Storage, đừng đi qua API route của Vercel.
- **Tên file có dấu hoặc khoảng trắng**: dễ gây lỗi URL. Nên đặt tên file lưu trữ bằng ID ngẫu nhiên, giữ tên bài gốc trong database.
- **Biến môi trường**: quên khai báo trên Vercel khiến bản deploy lỗi dù local chạy bình thường. Không bao giờ để service role key trong code phía client.
- **Link chia sẻ Facebook không hiện ảnh / tiêu đề**: Facebook cache Open Graph. Dùng công cụ Sharing Debugger của Facebook để làm mới.
- **Đổi thông tin ở admin nhưng thẻ chia sẻ vẫn cũ**: có thể do Next.js cache metadata của trang. Cần revalidate sau khi lưu `site_settings`.
- **Ảnh chia sẻ không hiện**: `og:image` phải là URL tuyệt đối, công khai và truy cập được không cần đăng nhập. Bucket `site-assets` phải là public. Ảnh quá nặng hoặc sai tỷ lệ (nên ~1200×630px) cũng khiến Facebook bỏ qua hoặc cắt xấu.
- **`site_settings` có nhiều hơn một dòng**: trang chủ có thể đọc nhầm dòng. Luôn đọc đúng một dòng cố định và không cho admin tạo thêm.
- **Ảnh bìa để trống mà vẫn chừa khung**: kiểm tra giao diện khi `cover_image_path` rỗng.
- **File M4A upload được nhưng Safari không phát**: thường do `contentType` sai, Supabase phục vụ file dưới dạng `application/octet-stream`. Khi upload phải đặt `audio/mp4` cho M4A và `audio/mpeg` cho MP3.
- **Bucket từ chối file M4A hợp lệ**: MIME type của M4A không nhất quán giữa thiết bị (`audio/mp4`, `audio/x-m4a`, `audio/m4a`). Nếu `allowed_mime_types` thiếu một giá trị, file thu từ một số máy sẽ bị chặn.
- **M4A là ALAC (lossless) thay vì AAC**: iPhone có tùy chọn ghi Voice Memos chất lượng không nén. File to hơn nhiều và có thể vượt giới hạn 15MB. Đặt Voice Memos về chế độ nén, hoặc xuất lại ở 128-192 kbps.
- **Chỉ kiểm tra đuôi file**: đổi tên `.wav` thành `.mp3` vẫn qua được kiểm tra đuôi. Kiểm tra thêm MIME type, và ràng buộc ở phía bucket Supabase chứ không chỉ ở giao diện.
- **Chuyển ngôn ngữ làm ngắt nhạc đang phát**: nếu player nằm trong từng trang riêng, chuyển giữa `/` và `/en` sẽ tải lại player. Đặt player trong layout dùng chung của hai route.
- **Chuỗi tiếng Việt lọt vào bản `/en`**: do viết cứng chữ trong component thay vì lấy từ từ điển. Kiểm tra cả nhãn truy cập (aria-label), thông báo trống, định dạng ngày.
- **`<html lang>` luôn là `vi` kể cả ở `/en`**: ảnh hưởng đến trình đọc màn hình và công cụ tìm kiếm. Đặt `lang` theo route.
- **Thẻ chia sẻ của `/en` giống hệt `/`**: quên truyền `lang` vào `generateMetadata`.
- **Facebook cache theo từng URL**: đổi thông tin ở admin xong phải làm mới Sharing Debugger cho cả `/` và `/en`.
- **`intro_en` để trống làm trang `/en` hiện ô trống**: phải có giá trị dự phòng là `intro_vi`.
- **Chữ tiếng Anh và tiếng Việt dài ngắn khác nhau**: làm vỡ bố cục dòng bài hát hoặc thanh player trên màn hình hẹp. Test cả hai bản trên điện thoại.
- **Tên file có đuôi viết hoa**: `.MP3`, `.M4A` phải được chấp nhận. So sánh đuôi file sau khi chuyển về chữ thường.
