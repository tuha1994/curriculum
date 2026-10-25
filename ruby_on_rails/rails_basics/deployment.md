### Giới thiệu

Trước khi tiếp tục hành trình phát triển web, chúng ta cần giải quyết một nhiệm vụ quan trọng: giới thiệu thành quả của mình với thế giới.

Dù là để chia sẻ sản phẩm với bạn bè, tạo hồ sơ năng lực cho các nhà tuyển dụng tương lai hay khởi động một hoạt động kinh doanh trực tuyến, chúng ta cần lưu trữ ứng dụng ở đâu đó để người khác có thể công khai truy cập trên internet.

Trong bài học này, chúng ta sẽ học cách triển khai ứng dụng lên một nhà cung cấp dịch vụ lưu trữ, cho phép chạy, xây dựng và vận hành các ứng dụng web trên đám mây.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài học.

- Nhà cung cấp dịch vụ lưu trữ là gì.
- Sự khác nhau giữa trang tĩnh và trang động.
- PaaS là gì và chúng hoạt động như thế nào.
- Cách triển khai lên một nhà cung cấp PaaS.
- Cách khắc phục các vấn đề triển khai thường gặp.

### Nhà cung cấp dịch vụ lưu trữ là gì?

Nhà cung cấp dịch vụ lưu trữ giống như chủ nhà cho thuê máy chủ. Họ sở hữu các máy chủ và cho khách hàng thuê không gian trên đó; khách hàng có thể dùng không gian này để lưu trữ website và giúp bất kỳ ai trên web cũng có thể truy cập.

Trước đây trong chương trình học, bạn đã từng sử dụng nhà cung cấp dịch vụ lưu trữ khi triển khai các dự án lên Github Pages. GitHub Pages rất phù hợp để lưu trữ miễn phí các trang web **tĩnh**, nhưng chúng ta không thể dùng nó để lưu trữ các ứng dụng Ruby on Rails **động**. Chúng ta sẽ cần một giải pháp mạnh hơn.

#### Trang tĩnh và trang động

Website tĩnh bao gồm các trang HTML được viết sẵn. Chúng là “tĩnh” vì mọi người truy cập đều nhìn thấy cùng một nội dung. Để xây dựng trang tĩnh, bạn chỉ cần HTML, CSS và JavaScript.

Ngược lại, website động có thể thay đổi nội dung dựa trên người đang truy cập. X (trước đây có tên là Twitter) là một ví dụ điển hình; mỗi người dùng nhìn thấy nội dung khác nhau trên bảng tin trang chủ tùy theo những tài khoản họ theo dõi. Để xây dựng trang động, bạn vẫn cần HTML, CSS và JS. Ngoài ra, bạn cần một ngôn ngữ phía máy chủ như Ruby và một cơ sở dữ liệu.

Những công nghệ bổ sung này khiến chúng ta không thể dùng GitHub Pages để lưu trữ ứng dụng Ruby on Rails. GitHub Pages không thể chạy mã Ruby và không cung cấp dịch vụ cơ sở dữ liệu mà chúng ta có thể sử dụng.

May mắn là nhiều nhà cung cấp dịch vụ lưu trữ có cung cấp mọi thứ chúng ta cần. Họ bao gồm các nhà cung cấp đám mây lớn và phức tạp như [AWS](https://aws.amazon.com/), [Google Cloud](https://cloud.google.com/) và [Microsoft Azure](https://azure.microsoft.com/), cũng như các nhà cung cấp platform as a service (PaaS) thân thiện hơn với người mới bắt đầu như [Heroku](https://www.heroku.com/), [Railway](https://railway.app/) và [Fly.io](https://fly.io/). Trong bài học này, chúng ta sẽ tập trung vào và sử dụng nhóm nhà cung cấp sau.

### PaaS là gì?

Platform as a Service là một loại nhà cung cấp dịch vụ lưu trữ cụ thể. Điều quan trọng nhất cần biết là chúng dễ sử dụng và dễ tiếp cận với người mới bắt đầu hơn nhiều so với các nhà cung cấp khác. Chúng quản lý nhiều chi tiết cấp thấp rắc rối của hạ tầng máy chủ bên dưới, nhờ đó chúng ta, những nhà phát triển, có thể tập trung nhiều thời gian hơn vào việc xây dựng ứng dụng thay vì cấu hình và quản lý các máy chủ chạy ứng dụng.

Mở rộng thêm phép ẩn dụ về chủ nhà, một nền tảng PaaS giống như việc có một chủ nhà lo liệu toàn bộ tiện ích, bảo trì tòa nhà và an ninh, trong khi bạn, nhà phát triển, tập trung vào việc sắp xếp nội thất, trang trí và sinh hoạt trong không gian đó.

Đây là một mô hình cực kỳ mạnh mẽ và rất phù hợp với chúng ta lúc này. Khi dùng nhà cung cấp PaaS để triển khai, chúng ta có thể tập trung học và thành thạo Ruby on Rails mà không phải rẽ hướng quá xa để học những kiến thức chuyên biệt cần thiết cho việc tự quản lý và bảo trì máy chủ.

Chúng tôi sẽ cung cấp danh sách các nhà cung cấp PaaS được đề xuất ở phần sau của bài học. Trước hết, hãy tìm hiểu ở mức khái quát cách các nhà cung cấp PaaS hoạt động.

### Dịch vụ PaaS hoạt động như thế nào?

Nhà cung cấp PaaS hoạt động bằng cách cho bạn dễ dàng truy cập một số tài nguyên mà mọi ứng dụng Rails đều không thể thiếu để hoạt động trên web.

#### Instance

Điều đầu tiên và quan trọng nhất mà các nhà cung cấp PaaS cung cấp là những “máy tính” ảo gọi là instance, nơi chạy ứng dụng của bạn. Về cơ bản, một instance nghĩa là một bản ứng dụng của bạn đang chạy tại một thời điểm. Nó giống như việc một máy tính duy nhất chạy ứng dụng, như cách bạn chạy trên Localhost. Nhiều instance giống như có nhiều bản sao ứng dụng chạy đồng thời, cho phép bạn xử lý nhiều lưu lượng truy cập hơn.

Điểm hay của Rails là bạn luôn có thể khởi chạy thêm instance cho ứng dụng nếu có quá nhiều lưu lượng truy cập và người dùng phải chờ yêu cầu của họ được xử lý.

Với phần lớn ứng dụng, một instance là đủ. Bạn có thể hỗ trợ lượng truy cập lớn chỉ bằng một instance duy nhất. Nhiều nhà cung cấp PaaS mà chúng tôi đề xuất ở phần sau của bài học sẽ tặng miễn phí instance đầu tiên.

#### Cơ sở dữ liệu

Điều quan trọng thứ hai mà các nhà cung cấp PaaS cung cấp là cơ sở dữ liệu. Họ giúp bạn dễ dàng khởi tạo một cơ sở dữ liệu mới cho từng ứng dụng bằng cách thực hiện toàn bộ việc thiết lập và cấu hình.

Nhiều nhà cung cấp thậm chí còn quản lý cơ sở dữ liệu giúp bạn bằng cách thiết lập bản sao lưu tự động, bảo đảm cơ sở dữ liệu luôn được cập nhật các bản vá bảo mật quan trọng mới nhất và thực hiện việc bảo trì liên tục để cơ sở dữ liệu hoạt động ổn định.

Sự yên tâm mà điều này mang lại không thể xem nhẹ. Bạn chắc chắn không muốn rơi vào cảnh 4 giờ sáng phải thức dậy vì hàng loạt cảnh báo: cơ sở dữ liệu gặp trục trặc do bạn quên áp dụng một bản vá bảo mật, trong khi cũng không có bản sao lưu để khôi phục.

Với Rails, chúng ta sẽ dùng PostgreSQL, một cơ sở dữ liệu mã nguồn mở phổ biến. Một số nhà cung cấp PaaS như Heroku sẽ tự động tạo cơ sở dữ liệu PostgreSQL cho ứng dụng khi chúng ta triển khai lần đầu. Những nhà cung cấp khác như Fly.io sẽ yêu cầu thêm vài bước thủ công, nhưng vẫn tốt hơn nhiều so với việc phải tự thiết lập cơ sở dữ liệu từ đầu.

#### Tên miền

Khi triển khai lần đầu, nhà cung cấp PaaS sẽ cấp cho bạn một tên miền ngẫu nhiên. Với Heroku, đó có thể là một cái tên mang hơi hướng thiền như “afternoon-falls-4209”. Nếu muốn truy cập ứng dụng, bạn có thể đi thẳng đến `http://afternoon-falls-4209.herokuapp.com` để xem ứng dụng của mình trực tuyến.

Tên miền đó sẽ luôn thuộc về bạn trên nhà cung cấp PaaS. Họ cấp cho mỗi ứng dụng một tên miền duy nhất, thuộc về bạn chừng nào ứng dụng còn tồn tại trên nền tảng của họ.

Trong thực tế, bạn sẽ muốn liên kết ứng dụng với tên miền tùy chỉnh của riêng mình, chẳng hạn `http://mycooldomain.com`. Cần lưu ý rằng bạn không cần tên miền tùy chỉnh cho các dự án hồ sơ năng lực sẽ xây dựng trong khóa học này. Các tên miền ngẫu nhiên mà nhà cung cấp PaaS cấp là đủ dùng. Nhưng nếu muốn trang bị cho ứng dụng một tên miền tùy chỉnh, trước tiên bạn cần mua tên miền từ một nhà đăng ký như [GoDaddy](https://www.godaddy.com/) hoặc [IWantMyName](https://iwantmyname.com/).

Để tìm một tên miền mới, hãy thử dùng [Domainr](https://domainr.com/).

Sau khi có tên miền, bạn cần trỏ nó đến dự án. Nhà cung cấp bạn đang sử dụng sẽ có tài liệu đầy đủ về cách dùng tên miền tùy chỉnh trên nền tảng của họ.

### Các dịch vụ PaaS được đề xuất

Trước đây, chọn một nhà cung cấp PaaS là quyết định đơn giản. Heroku từng có gói miễn phí cung cấp mọi thứ cần thiết để lưu trữ bao nhiêu ứng dụng nhỏ tùy thích. Đáng tiếc là họ đã ngừng gói miễn phí vào năm 2022.

May mắn là hiện vẫn còn rất nhiều lựa chọn tốt khác. Nhược điểm là tất cả đều có gói miễn phí rất hạn chế. Vì lý do này, và để phục vụ được càng nhiều học viên càng tốt, chúng tôi sẽ đề xuất nhiều lựa chọn thay vì chỉ một nhà cung cấp.

Bạn có thể lưu trữ miễn phí phần lớn dự án bằng cách kết hợp các nhà cung cấp khác nhau mà chúng tôi đề xuất. Tuy nhiên, bạn sẽ phải tự làm nhiều việc hơn một chút: tạo tài khoản và làm quen với vài nhà cung cấp khác nhau.

Nếu có thể trả phí lưu trữ, mọi thứ sẽ đơn giản hơn. Bạn có thể thoải mái chọn một nhà cung cấp để tìm hiểu sâu, đồng thời quản lý mọi ứng dụng ở một nơi.

Dù hoàn cảnh của bạn thế nào, chúng tôi đều có lựa chọn phù hợp. Sau đây là các nhà cung cấp PaaS chúng tôi đề xuất.

---

#### Render

- Render cho phép triển khai bằng “Blueprints”, liên kết với kho GitHub của dự án.
- 750 giờ miễn phí là đủ để lưu trữ vài ứng dụng mà không phải trả phí. Tuy nhiên, cơ sở dữ liệu trên Render là dịch vụ riêng, và cơ sở dữ liệu cấu hình thấp nhất có giá $7 mỗi cái.
- $21 mỗi tháng là đủ để lưu trữ ba ứng dụng vì cơ sở dữ liệu của mỗi ứng dụng có giá $7.

##### Render: Gói miễn phí

- 750 giờ sử dụng miễn phí mỗi tháng.
- Ứng dụng sẽ tự động chuyển sang trạng thái ngủ sau 15 phút không hoạt động, vì vậy 750 giờ miễn phí đủ để lưu trữ vài ứng dụng trong cả tháng.
- Mỗi thời điểm bạn chỉ có thể có một cơ sở dữ liệu miễn phí đang hoạt động; cơ sở dữ liệu này hết hạn 30 ngày sau khi được tạo.

##### Render: Liên kết

- [Trang chủ Render](https://render.com/)
- [Tài liệu Render](https://render.com/docs/)
- [Hướng dẫn: Hướng dẫn chính thức bắt đầu với Rails trên Render](https://render.com/docs/deploy-rails)
- [Hướng dẫn: Video GoRails triển khai ứng dụng Rails lên Render](https://www.youtube.com/watch?v=QJ4BcTQlKC0)

---

#### Koyeb

Có thể triển khai cả máy chủ lẫn cơ sở dữ liệu.

- Dễ dàng triển khai bằng cách Git push.
- Dễ dàng chạy web app, API và worker.
- SSL và 10 tên miền tùy chỉnh miễn phí.

##### Koyeb: Gói miễn phí

- Một dịch vụ Web miễn phí.
- Một cơ sở dữ liệu Postgres miễn phí (50 giờ).
- Không yêu cầu thẻ tín dụng để bắt đầu.
- Tính phí theo mức sử dụng, tính theo từng giây, khi dùng vượt mức miễn phí.

##### Koyeb: Liên kết

- [Trang chủ Koyeb](https://www.koyeb.com/)
- [Tài liệu Koyeb](https://www.koyeb.com/docs)
- [Hướng dẫn: Hướng dẫn chính thức bắt đầu với Ruby on Rails trên Koyeb](https://www.koyeb.com/docs/deploy/ruby-on-rails)

---

#### Fly.io

- Fly.io sử dụng công cụ CLI để triển khai.
- Có các gói trả phí, trong đó tài nguyên được tính theo mức sử dụng với mức giá cạnh tranh. Hiện không còn cung cấp mức tài nguyên miễn phí.
- Fly.io tính phí $0.15/GB RootFS cho các machine bị dừng trong 30 ngày.

##### Fly.io: Gói Hobby

- Fly.io không còn cung cấp khoản tín dụng dùng thử miễn phí $5 hoặc gói Hobby $5/tháng. Tuy nhiên, các tài nguyên miễn phí trước đây thuộc các gói Hobby (nay đã ngừng), Launch và Scale vẫn được duy trì cho những tổ chức đã dùng các gói này trước khi chúng bị ngừng.
- Người dùng mới phải đăng ký bằng thẻ tín dụng và sẽ bị tính phí ngay dựa trên mức tiêu thụ tài nguyên.

##### Fly.io: Liên kết

- [Trang chủ Fly.io](https://fly.io/)
- [Tài liệu Fly.io](https://fly.io/docs/)
- [Hướng dẫn: Hướng dẫn chính thức bắt đầu với Rails trên Fly.io](https://fly.io/docs/rails/getting-started/)
- [Hướng dẫn: Video GoRails triển khai ứng dụng Rails lên Fly.io](https://www.youtube.com/watch?v=6Zp9y8nF5rE&)

---

#### Railway.app

- Railway có quy trình triển khai đơn giản. Bạn liên kết với kho GitHub của dự án.
- Mô hình trả phí theo mức sử dụng.
- $5 mỗi tháng có thể đủ để lưu trữ bốn ứng dụng.

##### Railway.app: Dùng thử miễn phí

- Tặng một lần $5 tín dụng, hết hạn sau 30 ngày.
- Số dư còn lại sẽ được chuyển sang gói trả phí nếu bạn quyết định nâng cấp.
- Ứng dụng phức tạp hơn, có nhiều lưu lượng hơn, có thể dùng hết tín dụng miễn phí trước khi đủ 30 ngày.
- Ứng dụng không bao giờ tự chuyển sang trạng thái ngủ khi không hoạt động.

##### Railway.app: Liên kết

- [Trang chủ Railway](https://railway.app/)
- [Tài liệu Railway](https://docs.railway.app/)
- [Hướng dẫn: Video triển khai ứng dụng Rails lên Railway](https://www.youtube.com/watch?v=__1TadBZZUg)

---

#### Heroku

- Heroku có quy trình triển khai đơn giản nhờ kết hợp công cụ CLI được tài liệu hóa đầy đủ với Git.
- Heroku là một nền tảng rất trưởng thành đã tồn tại hơn một thập kỷ. Hầu như mọi vấn đề bạn gặp phải đều có lời giải được ghi lại trên Stack Overflow hoặc nơi khác trên internet.
- Gói Eco $5 mỗi tháng cung cấp 1000 giờ miễn phí mỗi tháng cho tất cả ứng dụng. Ứng dụng tự động chuyển sang trạng thái ngủ sau 30 phút không hoạt động, vì vậy 1000 giờ sẽ đủ cho cả tháng đối với một số dự án hồ sơ năng lực.
- Cơ sở dữ liệu PostgreSQL cấp thấp nhất của Heroku có giá $5 mỗi tháng, tức mỗi ứng dụng thực tế tốn $5 để lưu trữ.
- $20 mỗi tháng là đủ để lưu trữ ba ứng dụng. Gói Eco $5 cho 1000 giờ máy chủ + thêm $5 cho mỗi ứng dụng.

##### Heroku: Gói miễn phí

- N/A

##### Heroku: Liên kết

- [Trang chủ Heroku](https://www.heroku.com/)
- [Tài liệu Heroku](https://devcenter.heroku.com/)
- [Hướng dẫn: Hướng dẫn Rails chính thức cho Heroku](https://devcenter.heroku.com/articles/getting-started-with-rails7)
- [Hướng dẫn: Video GoRails triển khai ứng dụng Rails lên Heroku](https://www.youtube.com/watch?v=bQ2wXZf9irA)

---

#### Aiven

Chỉ có thể triển khai cơ sở dữ liệu.

- Hoạt động 24/7 cho mọi dịch vụ cơ sở dữ liệu.
- Tính sẵn sàng cao và sao lưu tự động.
- Khôi phục tại một thời điểm cụ thể (tùy dịch vụ).
- Không yêu cầu thẻ tín dụng.

##### Aiven: Gói miễn phí

- 5 GiB dung lượng lưu trữ.
- Hoạt động 24/7 cho mọi dịch vụ cơ sở dữ liệu.
- Một cơ sở dữ liệu miễn phí cho mỗi dịch vụ, bao gồm PostgreSQL, MySQL và Redis.
- Không yêu cầu thẻ tín dụng.

##### Aiven: Liên kết

- [Trang chủ Aiven](https://aiven.io/)
- [Tài liệu Aiven](https://aiven.io/docs/get-started)

---

#### Neon

Chỉ có thể triển khai cơ sở dữ liệu.

- Cơ sở dữ liệu chính hoạt động 24/7.
- Thêm 20 giờ phân nhánh cơ sở dữ liệu.
- Khôi phục tại một thời điểm cụ thể (24 giờ).
- Không yêu cầu thẻ tín dụng.

##### Neon: Gói miễn phí

- 0.5 GiB dung lượng lưu trữ.
- Tài nguyên compute chính hoạt động 24/7.
- Không yêu cầu thẻ tín dụng.

##### Neon: Liên kết

- [Trang chủ Neon](https://neon.tech/)
- [Tài liệu Neon](https://neon.tech/docs/introduction)
- [Hướng dẫn: Kết nối ứng dụng Ruby on Rails với Neon](https://neon.tech/docs/guides/ruby-on-rails)

---

### Gỡ lỗi và khắc phục sự cố khi triển khai

Lỗi là một phần không thể tránh khỏi của quá trình phát triển phần mềm. Chúng đặc biệt hay xuất hiện khi triển khai lên một môi trường mới như nhà cung cấp dịch vụ lưu trữ. Khi đó, điều quan trọng là không hoảng loạn mà hãy bình tĩnh làm theo quy trình gỡ lỗi từng bước.

Trong hầu hết trường hợp, bạn sẽ gặp những lỗi mà hàng nghìn nhà phát triển đã từng gặp. Những lỗi này thường được tài liệu hóa đầy đủ và bạn thường có thể tìm thấy cách giải quyết bằng một chút khả năng tìm kiếm trên Google.

Có hai giai đoạn của quá trình triển khai mà bạn dễ gặp vấn đề nhất: trong khi triển khai và ngay sau đó.

#### Trong khi triển khai

Nếu gặp lỗi trong lúc triển khai, việc đầu tiên cần làm là kiểm tra build log. Bạn sẽ dễ tìm thấy build log; đó là luồng đầu ra xuất hiện sau khi bắt đầu một lần triển khai mới.

Cuộn qua các log và tìm thời điểm quá trình triển khai gặp lỗi. Lỗi thường nổi bật so với phần còn lại của đầu ra và thường có dạng giống các stack trace bạn đã thấy với Ruby. Nội dung lỗi sẽ cho bạn biết chính xác chuyện gì đã xảy ra.

Nếu không nhận ra lỗi hoặc nguyên nhân có thể gây ra nó, bước tiếp theo là sao chép và dán lỗi vào công cụ tìm kiếm trực tuyến yêu thích. Rất có thể bạn sẽ tìm thấy một bài đăng trên Stack Overflow có lời giải. Nếu tìm kiếm không đưa đến kết luận rõ ràng, bạn có thể nhận hỗ trợ trên máy chủ Discord của chúng tôi.

Phần lớn lỗi bạn gặp ở giai đoạn này liên quan đến việc thiết lập ứng dụng đúng theo yêu cầu của nhà cung cấp dịch vụ lưu trữ. Kiểm tra lại hướng dẫn triển khai dành cho nhà cung cấp của bạn luôn là điểm khởi đầu tốt. Rất dễ bỏ sót một bước hoặc gõ sai thứ gì đó.

#### Sau khi triển khai

Bạn vừa triển khai ứng dụng thành công; mọi việc đang diễn ra thuận lợi và đây sẽ là một ngày tuyệt vời! Nhưng rồi bạn truy cập ứng dụng... và được chào đón bằng trang 500 đáng sợ.

Không gì khiến nhà phát triển hoảng sợ như trang 500. Nó có thể có nghĩa gần như bất cứ điều gì. Các trang lỗi trong môi trường production được cố ý viết mơ hồ để cho người dùng biết có vấn đề xảy ra mà không bị choáng ngợp bởi thuật ngữ kỹ thuật. Một lý do quan trọng khác là ngăn kẻ tấn công lợi dụng lỗi trong hệ thống của bạn.

Tuy vậy, bạn có một số công cụ để chẩn đoán vấn đề. Công cụ đầu tiên là application log.

Application log là đầu ra của ứng dụng trong khi nó đang chạy. Nó cho biết điều gì đang xảy ra với ứng dụng ngay tại thời điểm đó. Tất cả request đến và truy vấn cơ sở dữ liệu đều được ghi lại, và bạn có thể theo dõi chúng theo thời gian thực.

Vì vậy, nếu nhận lỗi 500, bạn có thể mở log và theo dõi thật kỹ trong khi tải lại trang ứng dụng trên trình duyệt để tái hiện lỗi. Việc này sẽ cho biết chính xác vấn đề là gì hoặc tạo ra những manh mối để bạn tiếp tục tìm hiểu.

Nếu log cho biết ứng dụng bị crash trước cả khi khởi động và không cung cấp thêm thông tin, công cụ tiếp theo cần dùng là Rails console. Rails console khởi động ứng dụng, nên chỉ riêng việc mở nó trong môi trường production cũng có thể tạo ra thông báo lỗi rõ ràng hơn, cho biết chính xác vì sao ứng dụng bị crash và phần nào gây ra sự cố.

Một nguyên nhân phổ biến của lỗi 500 trong Rails sau khi triển khai là quên migrate cơ sở dữ liệu. Nếu gặp lỗi 500 và các thay đổi mới nhất có migration cơ sở dữ liệu mới, hãy chạy `rails db:migrate` trên cơ sở dữ liệu production để ứng dụng hoạt động trở lại.

#### Tìm hiểu thêm về các công cụ khắc phục sự cố

Khi ứng dụng phát triển, bạn sẽ muốn dùng các công cụ theo dõi lỗi chuyên nghiệp hơn. Ví dụ, bạn có thể dùng các dịch vụ như [Sentry](https://sentry.io/) để theo dõi và giám sát lỗi bằng giao diện hiện đại, dễ sử dụng, đồng thời nhận thông báo khi lỗi xảy ra.

Những dịch vụ này cung cấp thêm thông tin về lỗi và request gây ra lỗi, giúp bạn tiết kiệm rất nhiều thời gian. Tuy nhiên, thiết lập và sử dụng các dịch vụ này nằm ngoài phạm vi bài học. Với vài ứng dụng đầu tiên, log và Rails console là quá đủ để bạn xử lý.

#### Một mẹo cuối cùng

Nếu bản triển khai mới nhất bị hỏng sau khi những lần triển khai trước đó đều thành công, hãy quay lại phiên bản gần nhất còn hoạt động để xác định những thay đổi đã thực hiện, rồi từ từ đưa các thay đổi đó trở lại nếu cần.

Đây là lúc các kỹ năng Git bạn đang học thực sự phát huy tác dụng và giúp bạn tiết kiệm rất nhiều thời gian. Bạn có thể dùng `git log` để xem lịch sử các thay đổi mới nhất và dùng `git checkout` để nhanh chóng quay lại một phiên bản trước đó còn hoạt động.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Triển khai [dự án Store App](https://www.theodinproject.com/lessons/ruby-on-rails-store-app) lên một trong các nhà cung cấp dịch vụ lưu trữ đã đề cập. Bất kỳ lựa chọn miễn phí nào cũng phù hợp cho mục đích học tập, nên bạn chọn nhà cung cấp nào cho các dự án của mình cũng không quan trọng. Điều quan trọng rút ra từ lần triển khai đầu tiên này là có được kinh nghiệm triển khai. Đừng lo nếu bạn chưa hiểu mọi thứ đang diễn ra. Theo thời gian, bạn sẽ hiểu.
   - Dùng một trong các hướng dẫn triển khai được liên kết của nhà cung cấp PaaS để hỗ trợ quá trình thực hiện.
   - Nếu gặp khó khăn khi triển khai, hãy xem phần [Gỡ lỗi và Khắc phục sự cố khi triển khai](#debugging-and-troubleshooting-deployments) để tham khảo một số mẹo.

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, nên hãy xem nó như tài liệu bổ sung.

- [free-for.dev](https://free-for.dev/). Đây là một kho lưu trữ khổng lồ liệt kê các phần mềm như SaaS, PaaS, IaaS, v.v. cùng những dịch vụ khác có gói miễn phí cho nhà phát triển.