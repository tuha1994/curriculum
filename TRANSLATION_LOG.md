# TRANSLATION_LOG — Sổ theo dõi bản dịch

## Phạm vi

Hiện chỉ theo dõi các bài học trong thư mục:

- `intermediate_html_css`

Không theo dõi các khóa hoặc thư mục khác cho đến khi người học yêu cầu mở rộng phạm vi.

## Cách ghi

- `Đường dẫn bài`: đường dẫn tương đối từ root repo curriculum.
- `Commit SHA gốc`: commit của repo curriculum gốc mà bản dịch đã đối chiếu.
- `Ngày kiểm tra`: ngày người học hoặc agent kiểm tra bản dịch.
- `Trạng thái`: ví dụ `chưa dịch`, `bản nháp`, `đã kiểm tra`, `cần cập nhật`.
- `Ghi chú cần duyệt`: thuật ngữ hoặc câu chưa chắc cần người học xem lại.

## Các bài đã dịch

| Đường dẫn bài | Commit SHA gốc | Ngày kiểm tra | Trạng thái | Ghi chú cần duyệt |
|---|---|---|---|---|
| `intermediate_html_css/intermediate_html_concepts/introduction.md` | `ef7d9a4a8206c60050ffa0efb893316220826021` | 2026-10-20 | `đã kiểm tra code và cấu trúc, chờ kiểm tra hiển thị` | Hai URL bên ngoài cần giữ nguyên: MDN HTML elements và CSS Cheat Sheet. Có khối `<div class="lesson-content__panel" markdown="1">` cần giữ nguyên. Không có điểm chưa chắc cần người học duyệt. |

## Quy tắc cập nhật

1. Chỉ thêm bài thuộc `intermediate_html_css`.
2. Ghi Git commit SHA thật của bản gốc, không dùng SHA giả hoặc chỉ ghi ngày.
3. Khi upstream thay đổi, xem diff trước rồi chỉ cập nhật phần tiếng Việt tương ứng.
4. Sau khi kiểm tra lại code, HTML, URL, lệnh và nội dung hiển thị, cập nhật SHA và trạng thái.
5. Nếu có điểm chưa chắc, ghi ngắn gọn để người học duyệt; không biến ghi chú thành nội dung bài học.
6. Người học tự chạy quy trình này bằng tay.
