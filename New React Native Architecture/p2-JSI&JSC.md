# JSI and JSC

*Được công bố lần đầu tiên vào năm 2018, kiến ​​trúc lại React Native là một nỗ lực lớn mà Facebook đã thực hiện để giải quyết một số vấn đề lâu dài của giải pháp di động đa nền tảng này.*

*Trong loạt bài này, chúng tôi sẽ giới thiệu tổng quan về các yếu tố chính tạo nên cấu trúc mới của React Native. Chúng tôi sẽ tránh hiển thị mã, để giữ cho phần giải thích này dễ tiếp cận nhất có thể và sẽ chia sẻ sự phấn khích của chúng tôi về việc triển khai mới này.*

Trong bài đăng thứ hai này, chúng ta sẽ đi sâu vào cách React Native sử dụng mã bạn viết và cách kiến ​​trúc lại thay đổi nó.

Do bản chất của JavaScript, nhóm React Native phải dựa vào một công cụ để diễn giải nó để nó có thể chạy trong một ứng dụng di động gốc. Trong kiến ​​trúc hiện tại, nhóm đã chọn sử dụng trực tiếp JavaScriptCore (JSC), theo quy tắc hướng dẫn iOS của Apple “sử dụng khung WebKit thích hợp và WebKit JavaScript” (JSC là khung được WebKit sử dụng).

Để nâng cao yếu tố này, ( khối thứ hai của kiến ​​trúc React Native hiện tại ), họ đã quyết định tách JavaScript được đóng gói và rút gọn đúng cách được tạo ra khỏi mã đã viết và công cụ sẽ “tiêu hóa nó”. Điều này có thể thực hiện được bằng cách giới thiệu một phần tử thứ ba giữa hai phần tử này, được gọi một cách rõ ràng là Giao diện JavaScript (JSI).

JSI không phải là một phần của React Native - nó là một lớp hợp nhất, nhẹ, có mục đích chung cho (về mặt lý thuyết) bất kỳ công cụ JavaScript nào. Tuy nhiên, khi được đưa vào câu đố rộng hơn của kiến ​​trúc mới, nó cho phép một vài cải tiến thực sự quan trọng.

Cách đầu tiên khá trực quan - có khả năng, JSC hiện có thể được hoán đổi dễ dàng hơn cho các động cơ khác (hoặc các phiên bản mới hơn của JSC, như gần đây đã xảy ra trong RN 0,59). Các tùy chọn khác mà bạn có thể biết là ChakraCore của Microsoft và V8 của Google.

Cải tiến thứ hai — được cho là nền tảng của toàn bộ kiến ​​trúc lại — là “bằng cách sử dụng JSI, JavaScript có thể giữ tham chiếu đến các Đối tượng máy chủ C ++ và gọi các phương thức trên chúng”. Điều này có nghĩa là cuối cùng, chúng tôi đang giải quyết vấn đề cốt lõi được giải thích trong bài viết trước : hai lĩnh vực JavaScript và Native sẽ thực sự nhận thức được nhau và sẽ không cần phải tuần tự hóa thành JSON các thông báo để chuyển qua , xóa bỏ mọi tắc nghẽn trên cầu (chúng ta sẽ tìm hiểu kỹ hơn điều này trong bài viết tiếp theo).

Đây là một thay đổi rất thú vị vì C ++ từ lâu đã là một trong số ít cách để chia sẻ mã giữa Android và iOS mà không cần dựa vào JavaScript; Mã gốc của Android được viết bằng C \ C ++ (Java và Kotlin được “dịch xuống” thông qua Giao diện gốc Java) và tương tự iOS hỗ trợ nó theo mặc định (Objective-C không gì khác hơn là một tập hợp chính xác của C).

Vì vậy, bước tái kiến ​​trúc này của React Native vừa cho phép những thay đổi lớn đối với cấu trúc hiện tại vừa mở ra cánh cửa để viết nhiều C ++ hơn cho các ứng dụng của bạn, đồng thời cho phép các phương pháp tiếp cận trường nâu dễ dàng hơn.

Nếu chúng tôi thay thế khối thứ hai của kiến ​​trúc bằng khối mới của nó ( bạn có thể tìm thấy đồ thị đầy đủ trong bài viết đầu tiên ), thay đổi sẽ như sau:

![alt](new-2.webp)

Điều này kết thúc phần thứ hai của cuộc khám phá của chúng tôi về kiến ​​trúc lại. Trong vài tuần tới, chúng tôi sẽ phát hành nhiều bài đăng hơn đi sâu vào các khối khác. Trong thời gian chờ đợi, hãy nhớ chia sẻ bài viết này với các nhà phát triển đồng nghiệp của bạn hoặc liên hệ với các câu hỏi tiếp theo trên Twitter (DM đang mở).

Như bạn có thể tưởng tượng, những thay đổi này mở ra cánh cửa cho nhiều cải tiến hơn trong khối tiếp theo và chúng tôi hy vọng chúng khơi dậy sự phấn khích về cách chúng sẽ tác động đến cơ sở mã của bạn mà không yêu cầu bất kỳ viết lại nào.


