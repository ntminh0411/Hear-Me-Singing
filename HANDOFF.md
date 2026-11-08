# HANDOFF — Đang làm dở ở đâu

> File này cập nhật vào CUỐI MỖI PHIÊN làm việc. Phiên mới bắt đầu bằng việc đọc file này.
> Viết như thể người đọc không nhớ gì về phiên trước.
> AI agent: cuối phiên, hãy ghi đè nội dung bên dưới bằng trạng thái mới nhất. Lịch sử cũ để ở mục "Nhật ký".

---

**Cập nhật lần cuối:** 2026-11-08
**Giai đoạn hiện tại:** Chưa bắt đầu code (đang chuẩn bị, xem `ROADMAP.md`)

## Đã xong

- Chốt hướng đi: Supabase + Next.js + Vercel, một website hai route (`/` và `/admin`)
- Chốt phong cách thiết kế tối giản (xem `DECISIONS.md` D-006 và `ROADMAP.md`)
- Soạn prompt khởi đầu và 4 file tài liệu này
- Chốt giao diện song ngữ Việt - Anh, mặc định tiếng Việt: `/` và `/en` (`DECISIONS.md` D-013)
- Chốt tên trang: **Hear Me Singing** (`DECISIONS.md` D-012)
- Chốt định dạng upload: chấp nhận `.mp3` và `.m4a`, tối đa 15MB (`DECISIONS.md` D-011, thay thế D-003)
- Bổ sung bảng `site_settings` (tên trang, giới thiệu, ảnh bìa, thông tin Open Graph) để admin tự sửa, ghi vào `ROADMAP.md` (mục Dữ liệu) và `DECISIONS.md` (D-010)

## Đang làm dở

_(Không có)_

## Việc tiếp theo (theo thứ tự)

1. Khởi tạo project Next.js + Tailwind
2. Nạp font, thiết lập biến màu sáng/tối
3. Làm Giai đoạn 1: khung trang chính + danh sách bài hát bằng dữ liệu mẫu thật. Tên trang / giới thiệu đọc từ object cấu hình tạm có hình dạng giống `site_settings` (xem D-010, D-013), có sẵn route `/` và `/en`, từ điển `vi` / `en` và nút "VI / EN"
4. Dừng lại để tôi xem trước khi sang Giai đoạn 2

## Câu hỏi đang chờ tôi trả lời

- Tên miền: mua miền riêng (ví dụ `hearmesinging.com`) hay dùng địa chỉ miễn phí `.vercel.app`?
- Dòng giới thiệu ngắn (tôi tự viết) bằng tiếng Việt (bắt buộc) và tiếng Anh (nên có)?
- Ảnh chia sẻ Facebook: ảnh tôi chụp hay ảnh chữ trên nền kem? Mô tả ngắn khi chia sẻ link?
- Có dùng ảnh bìa nhỏ ở đầu trang ngay từ đầu không?
- Danh sách tên bài hát thật để dùng làm dữ liệu mẫu?
- Mặc định sáng hay tối?

## Cách chạy dự án

_(Điền sau khi khởi tạo project)_

```
npm install
npm run dev
```

## Biến môi trường cần có

_(Chỉ ghi TÊN biến, không ghi giá trị. Giá trị nằm trong `.env.local` và trên Vercel)_

- `NEXT_PUBLIC_SUPABASE_URL`
- `NEXT_PUBLIC_SUPABASE_ANON_KEY`

## Trạng thái bên ngoài

- Supabase project: chưa tạo
- Vercel project: chưa tạo
- Bảng `songs`, `site_settings`, bucket `songs`, `site-assets`: chưa tạo
- Repo GitHub: chưa tạo
- Tên miền: chưa chọn
- Trang Facebook: chưa kiểm tra tên "Hear Me Singing" có bị trùng không

## Lưu ý cho phiên sau

- Làm từng giai đoạn nhỏ, dừng lại sau mỗi giai đoạn để tôi nhận xét.
- Không tự thêm tính năng ngoài `ROADMAP.md`.
- Không thay đổi phong cách thiết kế nếu tôi chưa yêu cầu.
- Giao diện song ngữ: `/` tiếng Việt (mặc định), `/en` tiếng Anh (D-013). Không viết cứng chuỗi giao diện trong component, luôn lấy từ từ điển. Player đặt trong layout chung để chuyển ngôn ngữ không ngắt nhạc.
- Tên trang là "Hear Me Singing" (D-012), giữ đúng cách viết hoa này ở mọi nơi.
- Upload chỉ nhận MP3 và M4A, tối đa 15MB. Kiểm tra ở cả giao diện lẫn cấu hình bucket Supabase (xem D-011).

---

## Nhật ký các phiên

| Ngày | Đã làm | Ghi chú |
|------|--------|---------|
| 2026-11-08 | Lên kế hoạch, chốt thiết kế, tạo tài liệu, thêm `site_settings`, chốt định dạng upload MP3 + M4A, chốt tên trang Hear Me Singing, chốt giao diện song ngữ Việt - Anh | Chưa có code |
