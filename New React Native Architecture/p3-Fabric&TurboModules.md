# Fabric and TurboModules

*Được công bố lần đầu tiên vào năm 2018, kiến ​​trúc lại React Native là một nỗ lực lớn mà Facebook đã thực hiện để giải quyết một số vấn đề lâu dài của giải pháp di động đa nền tảng này.*

*Trong loạt bài này, chúng tôi sẽ giới thiệu tổng quan về các yếu tố chính tạo nên cấu trúc mới của React Native. Chúng tôi sẽ tránh hiển thị mã, để giữ cho phần giải thích này dễ tiếp cận nhất có thể và sẽ chia sẻ sự phấn khích của chúng tôi về việc triển khai mới này.*

Trong bài đăng này, chúng ta sẽ đi sâu vào phần “cơ bản” của kiến ​​trúc lại, kiến ​​trúc mà mọi nhà phát triển React Native có lẽ đã nghe nói về: Fabric và TurboModules.

Như chúng ta đã khám phá trước đây, React mới cho phép khái niệm xếp hàng và JSI cho phép mã JavaScript nhận thức được mã Native “ở phía bên kia”.

Để dễ tiếp cận và dễ hiểu, chúng tôi sẽ “đơn giản hóa” khối cầu của kiến ​​trúc cũ là gì:

![alt](old-3.webp)

Nhóm yếu tố này về cơ bản chịu trách nhiệm về hai hành vi khác nhau: xác định giao diện người dùng sẽ trông như thế nào (thông qua Shadow Tree) và quản lý phía gốc (thông qua Native Modules). Như đã đề cập, những thông tin liên lạc này xảy ra thông qua các thông báo JSON không đồng bộ được gửi và gửi qua lại, qua một kênh giao tiếp, như bạn có thể mong đợi, có thể bị tắc nghẽn và dẫn đến trải nghiệm không tối ưu.

Nhóm Facebook đã quyết định chia cầu nối khổng lồ này thành hai tác nhân riêng biệt: Fabric, là cấu trúc lại của trình quản lý giao diện người dùng và TurboModules, là triển khai “thế hệ mới” của tương tác với phía gốc.

![alt](new-3.webp)

Fabric nhằm mục đích hiện đại hóa lớp kết xuất của React Native. Trong triển khai hiện tại, tất cả các hoạt động giao diện người dùng được xử lý bởi một loạt các “bước” xuyên cầu (React -> Native -> Shadow Tree -> Native UI). Tuy nhiên, việc triển khai mới cho phép người quản lý giao diện người dùng tạo Shadow Tree trực tiếp trong C ++, điều này làm tăng đáng kể sự nhanh chóng của quá trình bằng cách giảm số lượng "bước nhảy" giữa các vùng. Về cơ bản, điều này cải thiện đáng kể khả năng phản hồi của Giao diện người dùng.

Ngoài ra, bằng cách sử dụng JSI, Fabric hiển thị các hoạt động giao diện người dùng với JavaScript dưới dạng các chức năng: Shadow Tree mới (xác định những gì thực sự hiển thị trên màn hình) được chia sẻ giữa hai lĩnh vực, cho phép tương tác trực tiếp từ cả hai đầu.

Và, nếu đó chưa phải là một cải tiến lớn, thì kiểm soát trực tiếp này từ phía JavaScript cho phép có các hàng đợi ưu tiên từ React mới (được đề cập trong bài viết đầu tiên) cho các hoạt động giao diện người dùng, để có các thực thi đồng bộ chọn tham gia nơi nó có lợi cho hiệu suất Nền tảng này sẽ cho phép cải thiện các cạm bẫy phổ biến như danh sách, điều hướng và xử lý cử chỉ.

Trong quá trình triển khai hiện tại, các Mô-đun Gốc được sử dụng bởi mã JavaScript (ví dụ: Bluetooth) cần được khởi tạo khi ứng dụng được mở — ngay cả khi chúng không được sử dụng — vì “sự không nhận biết” giữa các lĩnh vực được đề cập trong bài viết đầu tiên . Cách tiếp cận TurboModules mới cho phép mã JavaScript chỉ tải từng mô-đun khi nó thực sự cần thiết và giữ tham chiếu trực tiếp đến nó, có nghĩa là không cần phải giao tiếp bằng cách sử dụng các thông báo JSON theo lô trên cầu cũ. Điều này sẽ cải thiện đáng kể thời gian khởi động cho các ứng dụng có nhiều Mô-đun gốc, cùng với giao tiếp trực tiếp được đề cập trong các bài viết khác.

Tất cả các thay đổi đối với khối thứ ba, như đã đề cập, dựa vào Giao diện JavaScript được khám phá trong bài viết trước . Nếu chúng tôi muốn tóm tắt kiến ​​trúc của nhóm Facebook mới trông như thế nào cho đến bước này, nó sẽ trông giống như sau:

![alt](new-5.webp)

Phần này kết thúc phần thứ ba của chuyến khám phá kiến ​​trúc lại của chúng tôi. Tuần tới, chúng tôi sẽ phát hành bài cuối cùng trong loạt bài này. Trong thời gian chờ đợi, hãy nhớ chia sẻ bài viết này với các nhà phát triển đồng nghiệp của bạn hoặc liên hệ với các câu hỏi tiếp theo trên Twitter (DM đang mở).

