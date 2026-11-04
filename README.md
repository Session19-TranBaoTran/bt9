# BÀI LÀM SESSION 18 – QUẢN LÝ MẬT KHẨU AN TOÀN

## Phần 1. Báo cáo phân tích kiến trúc an ninh mật khẩu

### 1. Vì sao lưu mật khẩu trên Google Chrome nguy hiểm khi máy tính nhiễm Infostealer?

Google Chrome có chức năng lưu mật khẩu để người dùng không cần nhập lại mỗi khi đăng nhập. Tuy nhiên, nếu máy tính bị nhiễm mã độc Infostealer thì những thông tin này có thể bị đánh cắp.

Chrome lưu thông tin đăng nhập trong tệp có tên Login Data trên máy tính. Mã độc có thể tìm kiếm tệp này và lấy dữ liệu đăng nhập. Ngoài ra, mã độc còn có thể tìm cách lấy thông tin giải mã được lưu trên hệ thống.

Đối với lập trình viên, điều này rất nguy hiểm vì trình duyệt thường lưu nhiều tài khoản quan trọng như GitHub, AWS, Steamworks, Firebase và các dịch vụ thanh toán.

Nếu hacker lấy được những thông tin này, họ có thể đăng nhập trái phép, chiếm quyền truy cập vào dự án, đánh cắp mã nguồn hoặc sử dụng tài khoản dịch vụ để gây thiệt hại tài chính.

Vì vậy, không nên chỉ dựa vào chức năng lưu mật khẩu mặc định của trình duyệt để bảo vệ các tài khoản quan trọng của công ty.

### 2. Kiến trúc Zero-Knowledge và Client-side Decryption

Zero-Knowledge là kiến trúc bảo mật trong đó nhà cung cấp dịch vụ không thể biết nội dung bí mật mà người dùng lưu trữ.

Đối với Bitwarden, dữ liệu được mã hóa ngay trên thiết bị của người dùng trước khi đồng bộ lên máy chủ. Quá trình này được gọi là Client-side Encryption.

Cơ chế hoạt động như sau:

1. Người dùng tạo Master Password để bảo vệ két sắt.
2. Bitwarden sử dụng cơ chế dẫn xuất khóa để tạo khóa mã hóa từ thông tin bí mật của người dùng.
3. Dữ liệu trong két sắt được mã hóa ngay trên thiết bị.
4. Dữ liệu đã mã hóa mới được gửi lên máy chủ Bitwarden.
5. Khi người dùng đăng nhập, thiết bị tải dữ liệu về và giải mã ngay trên máy khách.

Client-side Decryption có nghĩa là quá trình giải mã diễn ra trên thiết bị của người dùng chứ không phải trên máy chủ của nhà cung cấp.

Nếu máy chủ Bitwarden bị hacker tấn công, dữ liệu két sắt của PixelForge Studio vẫn được bảo vệ bởi lớp mã hóa AES-256. Hacker có thể lấy được dữ liệu đã mã hóa nhưng không thể đọc trực tiếp mật khẩu nếu không có khóa giải mã phù hợp.

Tuy nhiên, điều này không có nghĩa là an toàn tuyệt đối. Nếu Master Password yếu, thiết bị bị nhiễm mã độc hoặc khóa giải mã bị đánh cắp thì dữ liệu vẫn có nguy cơ bị lộ.

## Phần 2. Thiết kế chính sách và kịch bản chuyển đổi

### 3. Phương pháp tạo Master Passphrase đạt chuẩn

Master Passphrase là mật khẩu chính dùng để mở khóa két sắt Bitwarden. Đây là mật khẩu rất quan trọng vì nó bảo vệ toàn bộ tài khoản được lưu trữ bên trong.

Để tạo Master Passphrase an toàn, cần đáp ứng các yêu cầu sau:

- Có độ dài tối thiểu 20 ký tự.
- Nên sử dụng từ 5 đến 6 từ ngẫu nhiên.
- Không sử dụng tên, ngày sinh hoặc thông tin cá nhân.
- Có thể sử dụng dấu gạch ngang hoặc ký tự đặc biệt để ngăn cách các từ.
- Không sử dụng lại mật khẩu đã từng dùng ở tài khoản khác.
- Không chia sẻ Master Passphrase cho người khác.

Ví dụ:

```
MayBay!RungThong7?CaPheXanh
```

Đây chỉ là ví dụ minh họa, không nên sử dụng làm mật khẩu thật. Khi tạo mật khẩu thực tế, nên chọn các từ ngẫu nhiên độc lập với nhau.

Một Passphrase dài và gồm nhiều từ ngẫu nhiên sẽ khó bị đoán hơn mật khẩu ngắn hoặc có quy luật quen thuộc. Ngoài ra, cần bật xác thực hai yếu tố (2FA) cho tài khoản Bitwarden để tăng thêm lớp bảo vệ.

### 4. Bảng cấu hình Generator trong Bitwarden

Để quản lý hơn 40 tài khoản của PixelForge Studio, nhóm nên thống nhất sử dụng Password Generator của Bitwarden để tạo mật khẩu ngẫu nhiên riêng cho từng tài khoản.

| Tiêu chí                    | Cấu hình |
| --------------------------- | -------- |
| Loại mật khẩu               | Password |
| Độ dài                      | 32 ký tự |
| Chữ in hoa (A-Z)            | Bật      |
| Chữ thường (a-z)            | Bật      |
| Chữ số (0-9)                | Bật      |
| Ký tự đặc biệt              | Bật      |
| Số chữ số tối thiểu         | 1        |
| Số ký tự đặc biệt tối thiểu | 1        |
| Tránh ký tự dễ nhầm lẫn     | Bật      |

Quy định áp dụng:

Mỗi tài khoản phải có một mật khẩu riêng, không sử dụng chung mật khẩu giữa GitHub, Steamworks, AWS, Firebase hoặc các dịch vụ khác.

Nếu một dịch vụ có giới hạn ký tự hoặc không hỗ trợ một số ký tự đặc biệt thì cần điều chỉnh theo yêu cầu của dịch vụ đó, nhưng vẫn phải giữ mật khẩu dài và ngẫu nhiên nhất có thể.

Sau khi tạo mật khẩu, cần lưu trực tiếp vào đúng mục tài khoản trong Bitwarden, không ghi ra giấy hoặc gửi qua tin nhắn nhóm.

### 5. Vì sao Auto-fill của Bitwarden chống Phishing tốt hơn con người?

Khi người dùng truy cập một trang web giả mạo, họ có thể không nhận ra sự khác biệt giữa tên miền thật và tên miền giả.

Ví dụ:

- `github.com` là tên miền thật.
- `githúb.com` là tên miền giả mạo.
- `steampowered.xyz` không phải tên miền chính thức của Steam.

Bitwarden có thể kiểm tra địa chỉ website hiện tại với địa chỉ đã lưu trong mục đăng nhập của người dùng.

Nếu tên miền không khớp, Bitwarden sẽ không tự động đề xuất hoặc điền thông tin đăng nhập như trên website chính thức.

Điều này giúp hạn chế việc người dùng vô tình nhập mật khẩu vào trang web giả mạo. Trong khi đó, con người có thể dễ bị đánh lừa bởi giao diện website được làm giống trang thật.

Để tăng độ an toàn, nhóm nên cấu hình phương thức nhận diện tên miền phù hợp, ưu tiên Base domain hoặc Exact cho những tài khoản quan trọng. Không nên tự động điền mật khẩu vào một trang web nếu chưa kiểm tra địa chỉ.

### 6. Quy trình lưu trữ Khóa Cấp cứu (Emergency Kit)

PixelForge Studio cần có phương án khôi phục quyền truy cập nếu Trưởng phòng Kỹ thuật gặp sự cố, mất thiết bị hoặc không thể đăng nhập.

Nhóm có thể thực hiện quy trình sau:

Bước 1: Chuẩn bị thông tin khôi phục.

Trưởng phòng Kỹ thuật cần chuẩn bị thông tin tài khoản Bitwarden, các mã khôi phục 2FA và thông tin cần thiết để khôi phục quyền truy cập.

Bước 2: Lưu trữ an toàn.

Lưu bản Emergency Kit và mã khôi phục trong nơi lưu trữ ngoại tuyến có khóa bảo vệ. Có thể in ra giấy và cất trong két an toàn của công ty.

Không lưu bản Emergency Kit ở thư mục GitHub công khai, email cá nhân hoặc nhóm chat.

Bước 3: Thiết lập người liên hệ khẩn cấp.

Sử dụng tính năng Emergency Access của Bitwarden để chỉ định Trưởng nhóm làm người liên hệ khẩn cấp đáng tin cậy.

Thiết lập thời gian chờ phù hợp, ví dụ 24 giờ. Khi có sự cố, Trưởng nhóm có thể gửi yêu cầu truy cập. Nếu Trưởng phòng Kỹ thuật không phản hồi trong thời gian chờ, quyền truy cập sẽ được cấp theo cấu hình đã thiết lập.

Bước 4: Kiểm tra và cập nhật.

Định kỳ kiểm tra thông tin khôi phục, người được chỉ định và mã khôi phục 2FA. Khi có thay đổi nhân sự, cần cập nhật lại người có quyền truy cập.

Lưu ý: Emergency Kit là bản lưu thông tin cần thiết để hỗ trợ khôi phục tài khoản, không nên nhầm với tính năng Emergency Access. Emergency Access là cơ chế cho phép người liên hệ đáng tin cậy yêu cầu quyền truy cập vào két sắt theo quy trình đã thiết lập.

## Phần 3. Kế hoạch triển khai tại PixelForge Studio

Để chuyển đổi toàn bộ hơn 40 tài khoản sang Bitwarden, nhóm nên thực hiện theo các bước:

1. Thống kê toàn bộ tài khoản đang sử dụng và xác định người chịu trách nhiệm quản lý từng tài khoản.
2. Tạo tài khoản Bitwarden cho các thành viên và thiết lập Master Passphrase riêng.
3. Bật xác thực hai yếu tố cho tài khoản Bitwarden.
4. Tạo mật khẩu mới cho hơn 40 tài khoản bằng Generator, không tiếp tục sử dụng mật khẩu cũ.
5. Lưu từng tài khoản vào đúng mục trong két sắt Bitwarden.
6. Phân quyền truy cập theo nhiệm vụ của từng thành viên, không chia sẻ toàn bộ mật khẩu cho tất cả mọi người.
7. Tắt chức năng lưu mật khẩu của Chrome sau khi đã chuyển đổi và kiểm tra lại dữ liệu.
8. Kiểm tra định kỳ để đảm bảo các tài khoản đều có mật khẩu riêng và không còn sử dụng mật khẩu yếu.

## Kết luận

Qua bài tập này, em hiểu được tầm quan trọng của việc quản lý mật khẩu trong một dự án phần mềm. Việc sử dụng chung một mật khẩu cho nhiều tài khoản có thể khiến hacker dễ dàng chiếm quyền truy cập khi một dịch vụ bị rò rỉ dữ liệu.

Sử dụng Bitwarden với kiến trúc Zero-Knowledge, mã hóa AES-256, Master Passphrase mạnh và xác thực hai yếu tố sẽ giúp bảo vệ thông tin đăng nhập tốt hơn.

Ngoài ra, mỗi tài khoản cần có một mật khẩu riêng, được tạo ngẫu nhiên và lưu trữ trong két sắt số. Công ty cũng cần chuẩn bị phương án khôi phục khẩn cấp để tránh mất quyền truy cập khi nhân sự phụ trách gặp sự cố.