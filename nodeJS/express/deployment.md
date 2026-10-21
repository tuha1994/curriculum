### Giới thiệu

Trước khi tiếp tục hành trình phát triển web, chúng tôi phải giải quyết một nhiệm vụ quan trọng - giới thiệu công việc khó khăn của chúng tôi với thế giới.

Cho dù đó là để chia sẻ những sáng tạo của chúng tôi với bạn bè, tạo danh mục đầu tư cho các nhà tuyển dụng trong tương lai hay khởi động một doanh nghiệp trực tuyến, chúng tôi cần lưu trữ ứng dụng của mình ở nơi mà những người khác có thể truy cập công khai trên internet.

Trong bài học này, chúng ta sẽ tìm hiểu cách triển khai ứng dụng của mình cho nhà cung cấp dịch vụ lưu trữ, cho phép chúng ta chạy, xây dựng và vận hành các ứng dụng web của mình trên đám mây.

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Hiểu nhà cung cấp dịch vụ lưu trữ là gì.
- Hiểu được sự khác biệt giữa trang web tĩnh và trang web động.
- Hiểu PaaS là gì và cách chúng hoạt động.
- Biết cách triển khai tới nhà cung cấp PaaS.
- Biết cách khắc phục các sự cố triển khai thường gặp.

### Nhà cung cấp dịch vụ lưu trữ là gì?

Các nhà cung cấp dịch vụ lưu trữ giống như chủ sở hữu máy chủ. Họ sở hữu máy chủ và cho khách hàng thuê không gian trên đó, sau đó họ có thể sử dụng không gian đó để lưu trữ trang web của mình và giúp mọi người trên web có thể truy cập chúng.

Bạn đã có một số kinh nghiệm sử dụng nhà cung cấp dịch vụ lưu trữ khi triển khai dự án lên GitHub Pages trong các phần trước của chương trình học. GitHub Pages rất phù hợp để lưu trữ miễn phí các trang web **tĩnh**, nhưng không thể dùng để lưu trữ các ứng dụng Node **động**. Chúng ta sẽ cần một giải pháp mạnh mẽ hơn.

### Trang web tĩnh và trang web động

Các trang web tĩnh bao gồm các trang HTML được viết sẵn. Chúng "tĩnh" vì mọi người truy cập sẽ thấy nội dung giống nhau. Để xây dựng trang web tĩnh, bạn chỉ cần HTML, CSS và JavaScript.

Mặt khác, trang web động là những trang web có thể thay đổi nội dung dựa trên người dùng đang truy cập chúng. X (trước đây gọi là Twitter) là một ví dụ điển hình; mỗi người dùng sẽ thấy nội dung khác nhau trên nguồn cấp dữ liệu trang chủ của họ dựa trên người họ theo dõi. Để xây dựng trang web động, bạn vẫn cần HTML, CSS và JS. Nhưng ngoài ra, bạn cần một ứng dụng phía máy chủ và cơ sở dữ liệu.

Công nghệ bổ sung này cấm chúng tôi sử dụng GitHub Pages để lưu trữ các ứng dụng Node của mình. Các trang GitHub không thể chạy các ứng dụng Node.js và không có dịch vụ cơ sở dữ liệu mà chúng tôi có thể sử dụng. Tương tự, các nhà cung cấp máy chủ như Netlify và Vercel mà bạn có thể đã sử dụng trong khóa học React, không có khả năng tương tự để chạy các máy chủ và cơ sở dữ liệu Node.js của chúng tôi. Chúng không phải là công cụ phù hợp cho phần hỗ trợ của chúng tôi.

May mắn thay, nhiều nhà cung cấp dịch vụ lưu trữ cung cấp mọi thứ chúng tôi cần. Chúng bao gồm từ các nhà cung cấp đám mây lớn và phức tạp như [AWS](https://aws.amazon.com/), [Google Cloud](https://cloud.google.com/) và [Microsoft Azure](https://azure.microsoft.com/) cho đến các nhà cung cấp nền tảng dịch vụ (PaaS) thân thiện với người mới bắt đầu hơn như [Railway](https://railway.app/) và [Render](https://render.com/). Chúng ta sẽ tập trung và sử dụng những nhà cung cấp sau này trong bài học này.

### PaaS là gì?

Nền tảng dưới dạng Dịch vụ là một loại nhà cung cấp dịch vụ lưu trữ cụ thể. Điều quan trọng nhất cần biết về chúng là chúng dễ sử dụng hơn và dễ tiếp cận hơn với người mới bắt đầu so với các nhà cung cấp dịch vụ lưu trữ khác. Họ quản lý nhiều chi tiết thực tế ở cấp độ thấp bằng cơ sở hạ tầng máy chủ cơ bản, cho phép chúng tôi, với tư cách là nhà phát triển, tập trung nhiều thời gian hơn vào việc xây dựng các ứng dụng của mình thay vì định cấu hình và quản lý các máy chủ mà chúng chạy trên đó.

Lấy phép ẩn dụ chủ nhà mà chúng ta đã sử dụng trước đó xa hơn một chút, nền tảng PaaS giống như có một chủ nhà đảm nhiệm tất cả các tiện ích, bảo trì tòa nhà và an ninh. Trong khi đó, bạn, nhà phát triển, tập trung vào việc trang trí nội thất, trang trí và sinh hoạt trong không gian.

Đó là một mô hình cực kỳ mạnh mẽ và hoàn hảo cho chúng tôi hiện nay. Bằng cách sử dụng nhà cung cấp PaaS để triển khai, chúng ta có thể tập trung vào việc học và thành thạo Node mà không cần mất nhiều thời gian để học kiến ​​thức chuyên môn cần thiết để tự quản lý và bảo trì máy chủ.

Chúng tôi sẽ cung cấp danh sách các nhà cung cấp PaaS được đề xuất ở phần sau của bài học. Trước tiên, chúng ta hãy khám phá ở cấp độ cao hơn cách các nhà cung cấp PaaS hoạt động.

### Dịch vụ PaaS hoạt động như thế nào?

Các nhà cung cấp PaaS hoạt động bằng cách cung cấp cho bạn quyền truy cập dễ dàng vào một số tài nguyên mà bất kỳ ứng dụng Node nào cũng không thể thiếu để hoạt động trên web.

#### Phiên bản

Điều đầu tiên và quan trọng nhất mà các nhà cung cấp PaaS cung cấp cho bạn là các "máy tính" ảo được gọi là phiên bản chạy ứng dụng của bạn. Về cơ bản, một phiên bản có nghĩa là một phiên bản duy nhất của ứng dụng của bạn đang chạy cùng một lúc. Điều đó giống như có một máy tính chạy ứng dụng của bạn giống như bạn làm trên Localhost. Nhiều phiên bản giống như có nhiều bản sao ứng dụng của bạn chạy đồng thời, cho phép bạn xử lý nhiều lưu lượng truy cập hơn.

Đối với hầu hết các ứng dụng của bạn, một phiên bản là đủ. Bạn có thể hỗ trợ nhiều lưu lượng truy cập chỉ bằng một phiên bản duy nhất. Nhiều nhà cung cấp PaaS mà chúng tôi sẽ giới thiệu ở phần sau của bài học này sẽ cung cấp miễn phí cho bạn nhà cung cấp đầu tiên.

<div class="lesson-note" markdown="1">

#### Phiên bản máy chủ và phiên bản cơ sở dữ liệu

Người học có thể giữ phiên bản máy chủ và phiên bản cơ sở dữ liệu của họ trên một PaaS hoặc sử dụng các PaaS riêng biệt nếu cần. Khi bạn sử dụng các gói trả phí, nó thậm chí có thể giảm chi phí lưu trữ!

</div>

#### Cơ sở dữ liệu

Điều quan trọng thứ hai mà các nhà cung cấp PaaS cung cấp cho bạn là cơ sở dữ liệu. Chúng giúp bạn dễ dàng tạo cơ sở dữ liệu mới cho từng ứng dụng bằng cách thực hiện tất cả việc thiết lập và cấu hình cho bạn.

Nhiều nhà cung cấp thậm chí còn quản lý cơ sở dữ liệu cho bạn bằng cách thiết lập sao lưu tự động, đảm bảo cơ sở dữ liệu được cập nhật liên tục với các bản vá bảo mật quan trọng mới nhất và bảo trì liên tục giúp cơ sở dữ liệu của bạn luôn hoạt động trơn tru.

Sự an tâm mà điều này mang lại cho bạn không thể bị phóng đại. Bạn không bao giờ muốn rơi vào tình trạng phải thức dậy lúc 4 giờ sáng với nhiều cảnh báo. Cơ sở dữ liệu của bạn đang gặp sự cố do một số bản vá bảo mật mà bạn quên áp dụng và không có bản sao lưu nào để dự phòng.

Nhiều dịch vụ PaaS đi kèm với cơ sở dữ liệu SQL.

Hiện tại, chúng tôi khuyến khích bạn thử tìm hiểu cách triển khai dự án bảng tin nhỏ của mình chỉ với những gì bạn học được trong bài học này.

#### Tên miền

Các nhà cung cấp PaaS sẽ cung cấp cho bạn một tên miền ngẫu nhiên khi bạn triển khai lần đầu. Trong trường hợp của Heroku, nó thường giống như "buổi chiều-mùa thu-4209". Nếu bạn muốn truy cập ứng dụng, bạn có thể truy cập trực tiếp vào `http://afternoon-falls-4209.herokuapp.com` để xem ứng dụng của bạn trực tiếp trên web với tất cả vinh quang của nó.

Tên miền sẽ luôn là của bạn khi sử dụng nhà cung cấp PaaS. Họ cấp cho mỗi ứng dụng một tên miền duy nhất là của bạn miễn là ứng dụng của bạn tồn tại trên nền tảng của họ.

Trong thế giới thực, bạn sẽ muốn liên kết nó với miền tùy chỉnh của riêng mình, ví dụ: `http://mycooldomain.com`. Điều đáng nói là bạn không cần tên miền tùy chỉnh cho các dự án danh mục đầu tư mà bạn sẽ xây dựng trong khóa học này. Các miền ngẫu nhiên mà nhà cung cấp PaaS cung cấp cho bạn sẽ đủ tốt. Tuy nhiên, nếu bạn muốn đánh lừa ứng dụng của mình bằng một tên miền tùy chỉnh, trước tiên bạn cần mua miền từ nhà đăng ký như [Porkbun](https://porkbun.com/) hoặc [NameSilo](https://www.namesilo.com/).

Để tìm miền mới, hãy thử sử dụng [Domainr](https://domainr.com/).

Khi bạn có tên miền, bạn cần trỏ nó đến dự án của mình. Nhà cung cấp bạn đang sử dụng sẽ có tài liệu đầy đủ về cách sử dụng tên miền tùy chỉnh trên nền tảng của họ.

### Các dịch vụ PaaS được chúng tôi đề xuất

Chọn nhà cung cấp PaaS đã từng là một quyết định đơn giản. Heroku có cấp độ miễn phí cung cấp cho bạn mọi thứ cần thiết để lưu trữ bao nhiêu ứng dụng nhỏ tùy thích, nhưng rất tiếc là họ đã ngừng cung cấp cấp độ này vào năm 2022.

May mắn thay, vẫn còn rất nhiều lựa chọn tuyệt vời khác ngoài kia. Nhược điểm là tất cả chúng đều có số lượng miễn phí rất hạn chế. Vì lý do này và để thu hút càng nhiều người học càng tốt, chúng tôi sẽ đề xuất nhiều lựa chọn thay vì chỉ một.

Bạn có thể lưu trữ miễn phí hầu hết các dự án của mình bằng cách sử dụng kết hợp các nhà cung cấp khác nhau mà chúng tôi đề xuất, nhưng bạn sẽ có nhiều việc phải làm hơn một chút. Bạn sẽ phải tạo một tài khoản và làm quen với một số nhà cung cấp khác nhau.

Nếu bạn có thể trả tiền cho việc lưu trữ, mọi thứ sẽ đơn giản hơn. Nó sẽ mang lại cho bạn sự thoải mái khi chọn một nhà cung cấp mà bạn có thể tìm hiểu sâu và bạn sẽ có thể quản lý tất cả ứng dụng của mình từ một nơi.

Dù hoàn cảnh của bạn là gì, chúng tôi đều có thể hỗ trợ bạn. Dưới đây là các nhà cung cấp PaaS mà chúng tôi đề xuất.

---

#### Railway.app

Có thể triển khai cả máy chủ và cơ sở dữ liệu.

- Railway có quá trình triển khai thuận lợi. Bạn liên kết tới kho GitHub của dự án của bạn.
- Trả tiền cho những gì bạn sử dụng mô hình.
- $5 một tháng là đủ để lưu trữ bốn ứng dụng.

##### Railway.app: Gói miễn phí

- Bạn nhận được khoản trợ cấp miễn phí một lần trị giá $5 khi dùng thử miễn phí và các ứng dụng không bao giờ chuyển sang chế độ ngủ khi không hoạt động.
- Sau 30 ngày trôi qua hoặc bạn đã sử dụng hết khoản trợ cấp $5, Rail sẽ đưa bạn trở lại bản dùng thử giới hạn của họ, nơi bạn chỉ có thể triển khai cơ sở dữ liệu.

##### Railway.app: Liên kết

- [Trang chủ Railway](https://railway.app/)
- [Tài liệu Railway](https://docs.railway.app/)

---

#### Render

Có thể triển khai cả máy chủ và cơ sở dữ liệu.

- Render cho phép bạn triển khai bằng cách sử dụng "Blueprints", liên kết tới kho lưu trữ GitHub của dự án của bạn.
- 750 giờ miễn phí đủ để lưu trữ một vài ứng dụng mà không phải trả bất kỳ khoản nào. Tuy nhiên, cơ sở dữ liệu riêng biệt trên Render và cơ sở dữ liệu có thông số kỹ thuật thấp nhất có giá 7 USD mỗi cơ sở dữ liệu.
- 21 USD một tháng là đủ để lưu trữ ba ứng dụng vì cơ sở dữ liệu của mỗi ứng dụng sẽ có giá 7 USD.

##### Render: Gói miễn phí

- 750 giờ sử dụng miễn phí mỗi tháng.
- Các ứng dụng sẽ tự động chuyển sang chế độ ngủ sau 15 phút không hoạt động, vì vậy 750 giờ miễn phí sẽ đủ để lưu trữ một vài ứng dụng trong cả tháng.
- Bạn chỉ có thể có một cơ sở dữ liệu miễn phí đang hoạt động tại một thời điểm, cơ sở dữ liệu này sẽ hết hạn sau 30 ngày kể từ ngày tạo.

##### Render: Liên kết

- [Render trang chủ](https://render.com/)
- [Tài liệu Render](https://render.com/docs/)
- [Hướng dẫn: Chính thức bắt đầu với hướng dẫn Node/Express trên Render](https://render.com/docs/deploy-node-express-app)

---

#### Neon

Chỉ có thể triển khai cơ sở dữ liệu.

- 24/7 cho cơ sở dữ liệu chính của bạn.
- Thêm 20 giờ phân nhánh cơ sở dữ liệu.
- Khôi phục tại thời điểm (24 giờ)
- Không cần thẻ tín dụng.

##### Neon: Gói miễn phí

- 10 dự án
- 0,5 GiB dung lượng lưu trữ cho mỗi dự án
- 24/7 cho máy tính chính của bạn
- Không cần thẻ tín dụng

##### Neon: Liên kết

- [Trang chủ neon](https://neon.tech/)
- [Tài liệu về Neon](https://neon.tech/docs/introduction)
- [Hướng dẫn: Kết nối ứng dụng Node.js với Neon](https://neon.tech/docs/guides/node)

---

#### Aiven

Chỉ có thể triển khai cơ sở dữ liệu.

- 24/7 cho tất cả các dịch vụ cơ sở dữ liệu.
- Tính sẵn sàng cao và sao lưu tự động.
- Phục hồi tại thời điểm (thay đổi tùy theo dịch vụ).
- Không cần thẻ tín dụng.

##### Aiven: Gói miễn phí

- Dung lượng lưu trữ 5 GiB.
- 24/7 cho tất cả các dịch vụ cơ sở dữ liệu.
- Một cơ sở dữ liệu miễn phí cho mọi dịch vụ bao gồm PostgreSQL, MySQL và Redis.
- Không cần thẻ tín dụng.

##### Aiven: Liên kết

- [Trang chủ Aiven](https://aiven.io/)
- [Tài liệu Aiven](https://aiven.io/docs/get-started)
- [Hướng dẫn: Kết nối ứng dụng Node.js với Aiven](https://aiven.io/docs/products/postgresql/howto/connect-node)

<div class="lesson-note lesson-note--tip" markdown="1">

#### Giữ bí mật của bạn an toàn!

Hướng dẫn này cung cấp mẫu để định cấu hình kết nối cơ sở dữ liệu. Vui lòng không lưu trữ thông tin xác thực trực tiếp trong mã của bạn. Tham khảo phần [Biến môi trường](https://www.theodinproject.com/lessons/nodejs-environment-variables) để biết các phương pháp hay nhất.

</div>

---

### Triển khai gỡ lỗi và khắc phục sự cố

Lỗi là một phần không thể tránh khỏi trong quá trình phát triển phần mềm. Họ đặc biệt có thói quen xuất hiện khi triển khai sang môi trường mới như nhà cung cấp dịch vụ lưu trữ. Khi điều này xảy ra, điều quan trọng là đừng hoảng sợ và làm theo quy trình gỡ lỗi từng bước một cách bình tĩnh.

Trong hầu hết các trường hợp, bạn sẽ gặp phải lỗi mà hàng nghìn nhà phát triển đã gặp phải trước đây. Những lỗi này được ghi chép đầy đủ và thường có giải pháp mà bạn có thể tìm thấy bằng tìm kiếm trên Google.

Có hai giai đoạn của quá trình triển khai mà bạn có nhiều khả năng gặp phải sự cố nhất. Đây là trong quá trình triển khai và ngay sau đó.

#### Khả năng tương thích phiên bản Node

Tùy thuộc vào nhà cung cấp máy chủ bạn sử dụng, phiên bản Node được hỗ trợ và phiên bản được chọn mặc định có thể khác nhau. Bạn có thể tham khảo tài liệu của nhà cung cấp để biết thêm thông tin về những gì được hỗ trợ và lựa chọn, đồng thời tùy thuộc vào tính năng bạn sử dụng trong mã của mình, bạn có thể cần [chỉ định phiên bản Node nào mà dự án của bạn tương thích trong `package.json`](https://docs.npmjs.com/cli/v10/configuring-npm/package-json#engines).

#### Đang triển khai

Nếu bạn gặp lỗi khi triển khai, điều đầu tiên cần làm là kiểm tra nhật ký bản dựng. Việc tìm kiếm nhật ký xây dựng phải dễ dàng; đó là luồng đầu ra mà bạn sẽ thấy sau khi bắt đầu triển khai mới.

Cuộn qua các nhật ký này và tìm điểm mà quá trình triển khai gặp phải lỗi. Nó phải nổi bật so với phần còn lại của đầu ra và thường trông giống như dấu vết ngăn xếp mà bạn đã thấy với JavaScript/Node. Đầu ra lỗi sẽ cho bạn biết chính xác điều gì đã xảy ra.

Nếu bạn không nhận ra lỗi hoặc nguyên nhân có thể gây ra lỗi đó, bước tiếp theo của bạn sẽ là sao chép và dán lỗi đó vào công cụ tìm kiếm trực tuyến yêu thích của bạn. Bạn có thể sẽ tìm thấy một bài viết về Stack Overflow kèm theo giải pháp. Bạn có thể nhận hỗ trợ trên máy chủ Discord của chúng tôi nếu việc tìm kiếm không dẫn đến kết quả gì.

Hầu hết các lỗi bạn gặp phải trong giai đoạn này sẽ liên quan đến việc thiết lập ứng dụng đúng cách với những gì nhà cung cấp dịch vụ lưu trữ của bạn cần. Kiểm tra kỹ hướng dẫn triển khai cho nhà cung cấp dịch vụ lưu trữ của bạn luôn là nơi tốt để bắt đầu. Rất dễ bỏ sót một bước hoặc gõ nhầm nội dung nào đó.

#### Sau khi triển khai

Bạn vừa triển khai thành công ứng dụng của mình; mọi thứ đang diễn ra theo cách của bạn và đây sẽ là một ngày tuyệt vời! Tuy nhiên, khi bạn truy cập ứng dụng của mình... bạn sẽ được chào đón với trang 500 đáng sợ.

Không có gì khiến một nhà phát triển hoảng sợ như một trang 500. Nó có thể có nghĩa là bất cứ điều gì. Các trang lỗi trong quá trình sản xuất được cố tình mơ hồ để cho người dùng biết đã xảy ra sự cố mà không cần sử dụng nhiều thuật ngữ kỹ thuật phức tạp. Một lý do quan trọng khác là ngăn chặn kẻ tấn công sử dụng lỗi trong hệ thống của bạn để làm lợi thế cho chúng.

Bạn có sẵn một số công cụ để giúp bạn chẩn đoán vấn đề. Đầu tiên là nhật ký ứng dụng.

Nhật ký ứng dụng là đầu ra của ứng dụng của bạn khi nó đang chạy. Nó cho bạn biết điều gì đang xảy ra với ứng dụng của bạn khi nó đang diễn ra. Tất cả các yêu cầu đến và truy vấn cơ sở dữ liệu đều được ghi lại và bạn có thể thấy chúng được ghi lại trong thời gian thực.

Vì vậy, nếu gặp lỗi 500, bạn có thể mở nhật ký và theo dõi kỹ khi làm mới trang trên ứng dụng của mình trong trình duyệt để tái tạo lỗi. Điều này sẽ cho bạn biết chính xác vấn đề là gì hoặc tạo ra một số khách hàng tiềm năng mà bạn có thể sử dụng để tìm hiểu thêm.

#### Tiến xa hơn với các công cụ khắc phục sự cố

Khi ứng dụng của bạn phát triển, bạn sẽ muốn các công cụ theo dõi lỗi của mình trở nên phức tạp hơn. Ví dụ, bạn có thể sử dụng các dịch vụ như [Sentry](https://sentry.io/) để theo dõi và giám sát lỗi bằng giao diện trực quan, dễ sử dụng, đồng thời nhận thông báo khi lỗi xảy ra.

Các dịch vụ này sẽ cung cấp cho bạn thêm thông tin về lỗi và yêu cầu gây ra lỗi đó, giúp bạn tiết kiệm rất nhiều thời gian. Tuy nhiên, việc thiết lập và sử dụng các dịch vụ này nằm ngoài phạm vi của bài học này. Bạn có thể hoàn thành tốt công việc với nhật ký cho một số ứng dụng đầu tiên của mình.

#### Một mẹo cuối cùng

Nếu có sự cố xảy ra trong lần triển khai mới nhất của bạn sau khi triển khai thành công trước đây, hãy quay lại phiên bản hoạt động gần đây nhất để xác định những thay đổi bạn đã thực hiện và từ từ giới thiệu lại những thay đổi đó nếu cần.

Đây sẽ là lúc các kỹ năng Git mà bạn đã học sẽ bắt đầu phát huy tác dụng và giúp bạn tiết kiệm rất nhiều thời gian. Bạn sẽ có thể sử dụng `git log` để xem lịch sử các thay đổi mới nhất của mình và `git checkout` để nhanh chóng hoàn nguyên về phiên bản hoạt động trước đó.

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Triển khai [Dự án Bảng tin Mini](https://www.theodinproject.com/lessons/node-path-nodejs-mini-message-board) của bạn cho một trong những nhà cung cấp dịch vụ lưu trữ mà chúng tôi đã đề cập. Bất kỳ tùy chọn miễn phí nào cũng sẽ hoạt động cho mục đích chương trình giảng dạy, vì vậy việc bạn chọn tùy chọn nào cho dự án của mình không quan trọng. Điều quan trọng cần rút ra từ lần triển khai đầu tiên này là tích lũy kinh nghiệm triển khai. Đừng lo lắng nếu bạn không hiểu mọi thứ đang xảy ra. Điều đó sẽ đến với thời gian.
   - Sử dụng một trong các hướng dẫn triển khai được liên kết dành cho nhà cung cấp PaaS của bạn để giúp bạn trong suốt quá trình.
   - Nếu bạn gặp sự cố khi triển khai, hãy xem phần [Gỡ lỗi và khắc phục sự cố triển khai](#debugging-and-troubleshooting-deployments) để biết một số mẹo.

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Nó không bắt buộc, vì vậy hãy xem xét nó bổ sung.

- [free-for.dev](https://free-for.dev/). Đây là một kho phần mềm khổng lồ như SaaS, PaaS, IaaS, v.v. và các dịch vụ khác với các cấp độ nhà phát triển miễn phí.
