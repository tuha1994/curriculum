### Giới thiệu

Trong dự án này, chúng ta sẽ xây dựng một phiên bản rút gọn của [Google Drive](https://www.google.com/drive/) (hoặc bất kỳ dịch vụ lưu trữ cá nhân nào khác).

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Thiết lập một dự án mới bằng Express và Prisma. Cài đặt tất cả dependency cần thiết, bao gồm cả Passport, vân vân.
1. Thiết lập cơ chế xác thực dựa trên session bằng Passport.js. Sử dụng [thư viện Prisma session store](https://github.com/kleydon/prisma-session-store#readme) để lưu các session trong cơ sở dữ liệu.
1. Thêm một form để người dùng đã xác thực có thể tải file lên. Trước mắt, hãy lưu file trong hệ thống file của bạn. Bạn sẽ cần tích hợp [middleware multer](https://github.com/expressjs/multer). Chúng ta sẽ tải các file này lên sau khi mọi tính năng khác đã hoạt động.
1. Thêm các folder. Người dùng có thể CRUD folder và tải file lên chúng. Hãy thiết lập các route và những tương tác cần thiết với cơ sở dữ liệu cho việc này.
1. Thêm một route để xem thông tin cụ thể của file, chẳng hạn như tên, kích thước và thời điểm tải lên. Cần có một nút tải xuống để cho phép người dùng tải file về.
1. Cuối cùng, thêm logic để tải file lên. Bạn có thể lưu file trong cơ sở dữ liệu, nhưng nên sử dụng một dịch vụ lưu trữ đám mây cho trường hợp sử dụng này. Bạn có thể dùng [Cloudinary](https://cloudinary.com/) hoặc [Supabase storage](https://supabase.com/docs/guides/storage). Khi một file được tải lên, hãy lưu URL của file trong cơ sở dữ liệu.
1. Hãy xác thực các file của bạn! Cách bạn thực hiện việc này tùy thuộc vào bạn. Bạn có thể giới hạn một số loại file được tải lên và/hoặc hạn chế các file quá lớn.

#### Điểm cộng

- Thêm chức năng chia sẻ folder. Khi người dùng muốn chia sẻ một folder (và toàn bộ nội dung bên trong), họ cần có một form để chỉ định thời hạn, chẳng hạn như 1d, 10d, v.v. Chức năng này phải tạo ra một liên kết có thể chia sẻ với bất kỳ ai (kể cả người dùng chưa xác thực). Ví dụ, liên kết có thể có định dạng sau: `https://yourapp.com/share/c758c495-0705-44c6-8bab-6635fd12cf81`

</div>