### Giới thiệu

Bạn đã đi đến cuối phạm vi của khóa học này và hẳn có thể cảm thấy khá tự tin về kỹ năng Rails của mình. Điều đó không có nghĩa là bạn phải cảm thấy mình đã hiểu mọi thứ, nhưng bạn có thể xây dựng một ứng dụng Rails với những chức năng thực sự hữu ích trong thế giới thực. Có thể ứng dụng sẽ chưa đẹp lắm, nhưng điều đó sẽ đến trong vài khóa học tiếp theo.

Đây chưa phải khóa học cuối cùng trong chương trình, và bạn vẫn còn một chặng đường khá dài trước khi đạt đến điểm "full stack", nhưng đây là toàn bộ phần Rails mà chúng ta sẽ đi qua, và như vậy cũng đã khá xa rồi. Dù đã tiến bộ đến đâu, bạn vẫn cần tiếp tục xây dựng sản phẩm, đặt câu hỏi và tìm hiểu sâu hơn về cả Rails lẫn các best practice của kỹ nghệ phần mềm nói chung.

Đến lúc này, bạn đã có những khối xây dựng cơ bản của Rails, nhưng việc học không bao giờ kết thúc. Sáu tháng đầu tiên đi làm với vai trò Rails developer sẽ bao gồm việc học ít nhất lượng kiến thức tương đương những gì bạn vừa học. Mục tiêu của khóa học này là đưa bạn đến vị trí có thể bắt đầu nghĩ nghiêm túc về việc tìm công việc đầu tiên.

### Bước tiếp theo

Bài học cuối này không được thiết kế để dạy rõ ràng thêm kiến thức mới, mà chỉ hướng bạn đi đúng hướng nếu muốn đào sâu hơn trước khi tiếp tục. Hoặc có thể bạn sẽ đánh dấu trang này rồi quay lại khi muốn biết nên đi tiếp thế nào trên hành trình Rails của mình.

Rails rất sâu... có rất nhiều thứ diễn ra phía sau và sẽ mất khá lâu để thực sự hiểu mọi thứ hoạt động thế nào cũng như cách sử dụng tốt nhất. Cách tốt nhất để tìm hiểu là làm việc cho một công ty sử dụng Rails và được trả lương trong khi học. Nếu đó không phải lựa chọn khả thi, tự xây dựng sản phẩm và đọc tài liệu trong thời gian rảnh cũng là một phương án thay thế tốt. Mỗi ngày đều có thông tin mới được tạo ra trong các bài blog, bài đăng Stack Overflow và podcast.

Một bước đầu tiên tốt là đọc lại [Rails Guides](http://guides.rubyonrails.org/index.html). Có một số phần chúng ta đã bỏ qua và một số guide chưa kịp học (đặc biệt là phần "đào sâu hơn"). Cụ thể, các phần sau có thể sẽ thú vị:

#### Bảo mật

Nếu bắt đầu làm việc trên các ứng dụng hướng tới công chúng nhiều hơn, bảo mật sẽ trở nên quan trọng hơn. Hãy xem [Rails Guide về Security](http://guides.rubyonrails.org/security.html) để biết thêm thông tin.

#### Rails task

Rails task là một cách đặc biệt để chạy các script có thể sử dụng ứng dụng của bạn. Bạn đã quen với một số task như `$ rails db:migrate`, nhưng cũng có thể tự tạo task của mình. Website này dùng một Rails task để nạp curriculum từ repo GitHub. Hãy xem [Rails Guide về Command Line và bin/rails](http://guides.rubyonrails.org/command_line.html) để biết thêm thông tin.

#### Caching

Caching giúp ứng dụng nhanh hơn bằng cách giảm số lần gọi database. Hãy xem [Rails Guide về Caching](http://guides.rubyonrails.org/caching_with_rails.html) để tìm hiểu thêm.

### Sách

Dưới đây là một vài cuốn sách sẽ giúp bạn tìm hiểu sâu hơn:

- [The Rails 5 Way](https://leanpub.com/tr5w) của Obie Fernandez là một hướng dẫn toàn diện về framework.
- [Agile Web Development with Rails 6](https://pragprog.com/titles/rails6/agile-web-development-with-rails-6/) là một hướng dẫn chuyên sâu kết hợp với tutorial, đồng thời áp dụng các phương pháp Agile trong suốt quá trình.
- [Rails Antipatterns](http://www.amazon.com/Rails-AntiPatterns-Refactoring-Addison-Wesley-Professional/dp/0321604814) là một hướng dẫn hay về best practice.

### Tài nguyên khác

- [RailsCasts của Ryan Bates](http://railscasts.com/) vô cùng hữu ích, giúp bạn thực sự thấy cách triển khai nhiều gem hoặc practice khác nhau.
- [GoRails](https://gorails.com/) là một lựa chọn thay thế cho RailsCasts.
- [RailsApps Project](https://tutorials.railsapps.org/) cung cấp rất nhiều tutorial hay, hướng dẫn bạn xây dựng các tình huống thực tế như tích hợp Devise và CanCan vào app hoặc xây dựng backend thanh toán.
- [Blog cộng đồng RubyFlow](http://www.rubyflow.com/) thỉnh thoảng sẽ có những nội dung thú vị xuất hiện.

### Đóng góp

Bạn biết nhiều hơn mình nghĩ đấy. Hãy nhớ lúc chúng ta nói rằng bạn nên tiếp tục xây dựng sản phẩm? Website này hoàn toàn mã nguồn mở và cần sự giúp đỡ của bạn để cải thiện. Chúng ta có một nhóm riêng gồm các học viên hiện tại và cựu học viên, những người giúp thêm tính năng và đọc soát curriculum. Điều tuyệt vời nhất là mọi thứ hoàn toàn công khai và miễn phí, nên bạn có thể theo dõi hoặc tham gia ở mức độ phù hợp với mình.

Đây là một cách tuyệt vời để bắt đầu học về phương pháp phát triển Agile và bắt đầu làm những công việc phát triển có ý nghĩa trong một môi trường thân thiện, hỗ trợ lẫn nhau. Vì vậy, hãy xem [kênh chung trên máy chủ Discord của chúng ta](https://discordapp.com/channels/505093832157691914/505093832157691916) để biết chúng ta đang làm gì. Chúng tôi rất mong bạn tham gia!

### Kết luận

Một lần nữa, tôi muốn nhấn mạnh rằng việc học thực sự không bao giờ dừng lại, nhưng bạn cũng đã đi được một chặng đường dài. Hãy tiếp tục đồng hành trong vài khóa học tiếp theo; bạn sẽ dùng nền tảng vững chắc này để xây dựng những sản phẩm rất thú vị. Trong khóa HTML/CSS, bạn sẽ học cách khiến ứng dụng Rails trông như những website thực sự thay vì chỉ là các khối văn bản. Trong khóa JavaScript, bạn sẽ học cách làm cho các web app hằng ngày nhanh hơn nhiều và năng động hơn trong trình duyệt.

Vẫn còn rất nhiều điều phải học, nhưng mọi thứ sẽ ngày càng hay hơn. Hãy kiên trì!

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích tới nội dung liên quan. Đây không phải phần bắt buộc, nên hãy xem như tài liệu bổ trợ.

- [How I Learned Ruby on Rails](https://medium.com/how-i-learned-ruby-rails/e08c94e2a51e) của Richardson Dackam
- [Danh sách các tutorial Rails](https://tutorials.railsapps.org/rails-tutorial) của Daniel Kehoe thuộc RailsApps
- Danh sách một số [tutorial Rails dài hơn của Jumpstart Lab](https://web.archive.org/web/20240928083941/http://tutorials.jumpstartlab.com/)
- [Khóa học Rails trực tuyến của University of Texas](http://schneems.com/ut-rails) (chưa hoàn chỉnh/lỗi thời?)
- [Rails Cheat Sheet](http://pragtob.github.io/rails-beginner-cheatsheet/index.html) của Tobias Pfeiffer
- [Danh sách Rails Best Practices](http://rails-bestpractices.com/)
- [Tutorial xây dựng CMS đơn giản bằng Sinatra](http://www.sitepoint.com/a-simple-content-management-system-in-sinatra/)
- [Vì sao không nên hard-code secret token của ứng dụng trong production](http://daniel.fone.net.nz/blog/2013/05/20/a-better-way-to-manage-the-rails-secret-token/)
- [How I Test của Ryan Bates](http://railscasts.com/episodes/275-how-i-test)
- [Vì sao nên dùng các method `%Q`](http://stackoverflow.com/questions/10144543/what-is-the-use-case-for-rubys-q-q-quoting-methods)
- [Loạt video Build 10 Apps của Web-Crunch](https://www.youtube.com/watch?v=4ABesTeDKmQ&list=PL01nNIgQ4uxNkDZNMON-TrzDVNIk3cOz4)
- [Mackenzie Child: 12 app trong 12 tuần](https://medium.com/ruby-on-rails/how-i-finally-learned-rails-95e9b832675b#.mw99m5wat)
- [Video giới thiệu về API](https://www.youtube.com/watch?v=oBW_VNg4qD0)