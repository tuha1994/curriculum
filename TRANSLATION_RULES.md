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
- Dấu định dạng Markdown như `*...*`, `**...**`, `_..._` và `__...__`.

## Định dạng Markdown phải được giữ nguyên

- Giữ nguyên số lượng, loại và vị trí logic của các cặp dấu emphasis (nhấn mạnh) Markdown.
- Được dịch phần chữ nằm bên trong dấu emphasis, nhưng không được xóa, thêm hoặc đổi cặp dấu.
- Giữ nguyên các định dạng Markdown khác như heading, danh sách, blockquote, link, ảnh và bảng.
- Không biến text được nhấn mạnh thành text thường.
- Ví dụ: `*lot*` có thể dịch thành `*rất nhiều*`, nhưng không được viết thành `rất nhiều`.
- Nếu dấu `*`, `_` hoặc backtick nằm trong code hoặc định danh kỹ thuật, áp dụng quy tắc giữ nguyên code, không coi đó là văn xuôi.

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

## Quy trình cập nhật từ upstream

Thực hiện bằng tay, chỉ trong phạm vi `intermediate_html_css`:

1. Kiểm tra trạng thái làm việc và bảo đảm bản dịch hiện tại đã được lưu ở một mốc có thể quay lui.
2. Ghi lại commit SHA đang được dùng trong `TRANSLATION_LOG.md`.
3. Cập nhật thông tin từ remote `upstream` theo quy trình Git mà người học đã duyệt.
4. Chỉ xem thay đổi trong các đường dẫn thuộc `intermediate_html_css`.
5. Với mỗi bài có thay đổi, xem diff bản gốc trước khi sửa bản dịch.
6. Cập nhật chỉ phần tiếng Việt tương ứng với phần văn xuôi đã thay đổi. Không thay thế toàn bộ file bài dịch bằng file upstream.
7. Giữ nguyên code, comment trong code, URL, lệnh, đường dẫn, tag HTML, thuộc tính HTML và các khối bao quanh bài học.
8. Nếu có xung đột, dừng ở bài đó, giữ riêng phần bản dịch và phần upstream để đối chiếu; không dùng thao tác ghi đè hàng loạt.
9. Chạy kiểm tra vùng code và kiểm tra thủ công URL, lệnh, đường dẫn, HTML và ngữ nghĩa bản dịch.
10. Xem bài đã cập nhật trên web local.
11. Chỉ sau khi mọi kiểm tra đạt mới cập nhật commit SHA, ngày, trạng thái và ghi chú trong `TRANSLATION_LOG.md`.
12. Nếu kiểm tra thất bại, hoàn tác bài đang xử lý về mốc trước cập nhật, rồi khôi phục log về SHA cũ.

Không tự động hóa bằng hook, CI, dịch nền hoặc job định kỳ. Không mở rộng sang thư mục khác nếu chưa được người học chốt.

## Checklist dịch ngay trước khi học

Dùng checklist này cho từng bài, theo đúng thứ tự:

1. Xác định đường dẫn bài và kiểm tra bài thuộc `intermediate_html_css`.
2. Đọc commit SHA gốc trong `TRANSLATION_LOG.md`.
3. Đọc bài gốc, `GLOSSARY.md` và các bài liên quan được bài liên kết.
4. Dịch chỉ văn xuôi, tiêu đề, heading và text hiển thị.
5. Giữ nguyên code, comment trong code, lệnh, đường dẫn, URL, tag và thuộc tính HTML.
6. Ghi thuật ngữ hoặc câu chưa chắc ở cuối bài để người học duyệt.
7. Chạy công cụ kiểm tra bất biến code.
8. Đối chiếu thủ công URL, lệnh, đường dẫn, HTML và ngữ nghĩa.
9. Mở đúng bài trên web local và kiểm tra nội dung, heading, liên kết, ghi chú và code.
10. Chỉ khi đạt mới cập nhật trạng thái trong `TRANSLATION_LOG.md` và bắt đầu học.

Nếu bất kỳ bước nào không đạt, dừng học bài đó, ghi trạng thái phù hợp và quay lui bản dịch về mốc an toàn.
