# TRANSLATION RULES — Quy tắc dịch

## Mục tiêu

Tạo bản dịch tiếng Việt dễ học, đúng bối cảnh The Odin Project và có thể hiển thị trên web local mà không làm hỏng nội dung kỹ thuật.

## Quy tắc bắt buộc

1. Chỉ dịch văn xuôi và phần chữ hiển thị cho người học.
2. Đọc bài đang dịch, `GLOSSARY.md` và các bài liên quan được bài đó liên kết trước khi dịch.
3. Dịch tiêu đề và heading sang tiếng Việt. Ở lần xuất hiện đầu tiên trong mỗi bài, giữ thuật ngữ tiếng Anh trong ngoặc và giải thích ngắn khi cần.
4. Giữ nguyên code, kể cả comment, output mẫu và nội dung trong code block.
5. Giữ nguyên lệnh terminal, đường dẫn, tên file, tên biến, tên hàm, tên API, tên package và các định danh kỹ thuật.
6. Giữ nguyên URL, địa chỉ liên kết, anchor, tham chiếu file và nội dung thuộc tính HTML.
7. Giữ nguyên tag HTML và cấu trúc HTML. Chỉ dịch text node hiển thị nếu đó là văn xuôi.
8. Không sửa các khối bao quanh bài học, ví dụ `<div class="lesson-note" markdown="1">`.
9. Nếu có nhiều cách hiểu, không tự đoán. Giữ thuật ngữ hoặc câu tiếng Anh cần thiết và ghi phần cần duyệt ở cuối file bài dịch.
10. Không viết lời giải cho bài tập của Odin.
11. Không dịch lại cả bài khi upstream chỉ thay đổi một phần. Chỉ cập nhật phần tiếng Việt tương ứng sau khi đối chiếu diff.

## Những phần không được thay đổi

- Fenced code block, indented code block và khối `<pre><code>`.
- Code inline và comment trong code.
- Lệnh, đường dẫn, URL, tên file, tên biến, tên hàm, tên API, tên package và định danh kỹ thuật.
- Tag HTML, thuộc tính HTML, `href`, `id`, `class`, `alt`, `title` và anchor.
- Cấu trúc Markdown/HTML cần cho web render, bao gồm các khối ghi chú của bài học.

## Kiểm tra trước khi chấp nhận bản dịch

- Đối chiếu bản gốc và bản dịch.
- So sánh các vùng code theo thứ tự.
- Kiểm tra URL, lệnh, đường dẫn, tag và thuộc tính HTML.
- Kiểm tra thuật ngữ lần đầu trong bài và đối chiếu `GLOSSARY.md`.
- Đọc các ghi chú cần duyệt ở cuối bài.
- Xem bài trên web local trước khi bắt đầu học.

## Ghi chú cần người học duyệt

Đặt ở cuối file bài dịch, dưới một heading rõ ràng như `## Ghi chú cần duyệt`. Không trộn ghi chú vào văn xuôi của bài. Ghi thuật ngữ hoặc câu gốc cần xem xét, cách hiểu tạm thời và lý do chưa chắc chắn.

## Theo dõi phiên bản

Sổ `TRANSLATION_LOG.md` ghi đường dẫn bài, Git commit SHA của bản gốc, ngày kiểm tra, trạng thái và ghi chú. Chỉ ghi các thư mục bài học người học đang học. Người học tự chạy quy trình cập nhật bằng tay.
