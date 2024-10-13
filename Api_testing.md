# 🛡️ **PortSwigger Learning Path Notes** 🛡️

## 📚 Module: API testing
- **Ngày học**: [12/10/2024]

### 🔑 **Key Concepts**
1. **Khái niệm chính 1**: 
- Để bắt đầu kiểm thử API, cần xác định các điểm cuối (endpoints) để tìm hiểu bề mặt tấn công. Sau khi xác định, cần tìm cách tương tác với chúng, bao gồm dữ liệu đầu vào, phương thức HTTP hỗ trợ, định dạng yêu cầu, giới hạn tần suất và cơ chế xác thực.

2. **Khái niệm chính 2**:  
- Tài liệu API giúp nhà phát triển hiểu và tích hợp API, có thể ở dạng đọc được bởi con người hoặc máy móc (JSON, XML). Nếu tài liệu công khai, nên bắt đầu khảo sát từ đó. Nếu không, có thể tìm tài liệu bằng cách duyệt ứng dụng sử dụng API, dùng Burp Scanner để tìm các endpoint như /api, /swagger/index.html, hoặc /openapi.json, và kiểm tra các đường dẫn cơ sở liên quan. Bạn có thể dùng Burp Scanner để quét tài liệu API dạng JSON, YAML, OpenAPI. OpenAPI Parser BApp cũng hữu ích cho việc phân tích. Các công cụ như Postman và SoapUI hỗ trợ kiểm thử các endpoint từ tài liệu.

3. **Khái niệm chính 3**:
- Để xác định endpoint API, bạn có thể duyệt ứng dụng sử dụng API và tìm mẫu URL như /api/. Burp Scanner giúp thu thập endpoint, và JS Link Finder BApp hỗ trợ tìm endpoint trong JavaScript. Sau khi xác định, bạn dùng Burp Repeater và Intruder để tương tác với các endpoint, kiểm tra cách API phản hồi khi thay đổi phương thức HTTP và loại dữ liệu.

- Bạn cần kiểm tra các phương thức HTTP khác nhau (GET, POST, DELETE) để phát hiện thêm chức năng. Ngoài ra, thay đổi loại dữ liệu (Content-Type) trong yêu cầu có thể tiết lộ thông tin hữu ích hoặc khai thác lỗi bảo mật.

4. **Khái niệm chính 4**:

- Để xác định endpoint API, bạn có thể duyệt ứng dụng sử dụng API và tìm mẫu URL như /api/. Burp Scanner giúp thu thập endpoint, và JS Link Finder BApp hỗ trợ tìm endpoint trong JavaScript. Sau khi xác định, bạn dùng Burp Repeater và Intruder để tương tác với các endpoint, kiểm tra cách API phản hồi khi thay đổi phương thức HTTP và loại dữ liệu.
- Bạn cần kiểm tra các phương thức HTTP khác nhau (GET, POST, DELETE) để phát hiện thêm chức năng. Ngoài ra, thay đổi loại dữ liệu (Content-Type) ( Content type converter BApp) trong yêu cầu có thể tiết lộ thông tin hữu ích hoặc khai thác lỗi bảo mật.
- Sử dụng Burp Intruder để tìm endpoint ẩn bằng cách thay đổi các phần của endpoint đã biết với các từ khóa phổ biến, kết hợp wordlists chứa thuật ngữ ngành và từ khóa ứng dụng.

- Bạn có thể tìm tham số ẩn trong API bằng các công cụ như:

    - Burp Intruder: Thay thế hoặc thêm từ khóa vào tham số đã biết để phát hiện chức năng ẩn.
    - Param Miner BApp: Đoán tự động tên tham số ẩn, giúp khám phá thông tin như chế độ gỡ lỗi hoặc token.
    - Content Discovery Tool: Tìm nội dung không được liên kết trực tiếp, có thể bao gồm tham số bổ sung.

5. **Khái niệm chính 5**:
- Mass assignment xảy ra khi tham số yêu cầu được tự động gán vào đối tượng nội bộ, tạo ra các tham số ẩn như isAdmin. Bạn có thể kiểm tra lỗ hổng này bằng cách gửi các yêu cầu chỉnh sửa tham số ẩn, kiểm tra xem tham số có bị gán mà không qua xác thực không. Nếu thành công, kẻ tấn công có thể chiếm được quyền truy cập không hợp lệ, chẳng hạn như quyền admin.

6. **Khái niệm chính 6**: Server-side parameter pollution
- Server-side parameter pollution (SSPP) xảy ra khi hacker chèn các tham số vào yêu cầu gửi đến API phía server để thao tác hoặc vượt qua các kiểm tra bảo mật.
- Truncating query strings (Cắt ngắn chuỗi truy vấn):
Hacker dùng ký tự `#` để cắt ngắn yêu cầu phía server.
Ví dụ:
Yêu cầu bình thường:
`GET /userSearch?name=peter&back=/home`
Yêu cầu chèn #:
`GET /userSearch?name=peter%23foo&back=/home`
Kết quả: `GET /users/search?name=peter#foo&publicProfile=true`, bỏ qua `publicProfile=true` và có thể hiển thị thông tin không công khai.
- Injecting invalid parameters (Chèn tham số không hợp lệ):
Hacker dùng & để chèn thêm tham số không hợp lệ vào yêu cầu.
Ví dụ:
`GET /userSearch?name=peter%26foo=xyz&back=/home`
Kết quả: `GET /users/search?name=peter&foo=xyz&publicProfile=true`. Nếu tham số foo được chấp nhận, có thể tiết lộ thêm thông tin.
- Injecting valid parameters (Chèn tham số hợp lệ):
Hacker tìm các tham số hợp lệ để chèn vào.
Ví dụ:
`GET /userSearch?name=peter%26email=foo&back=/home`
Kết quả: `GET /users/search?name=peter&email=foo&publicProfile=true`. Thử thêm tham số hợp lệ như email có thể gây ra thay đổi đáng chú ý trong phản hồi của server.
- Overriding existing parameters (Ghi đè tham số hiện có):
Hacker chèn lại cùng một tham số với giá trị khác để ghi đè.
Ví dụ:
`GET /userSearch?name=peter%26name=carlos&back=/home`
Kết quả: `GET /users/search?name=peter&name=carlos&publicProfile=true`. Ứng dụng có thể xử lý `name=carlos` và ghi đè `name=peter`, dẫn đến khai thác như đăng nhập với tài khoản khác.

7. **Khái niệm chính 7**: Testing for server-side parameter pollution in REST paths
- SSPP trong RESTful API xảy ra khi kẻ tấn công sử dụng chuỗi traversal (vd: `GET /edit_profile.php?name=peter%2f..%2fadmin`) để thay đổi tham số trong đường dẫn URL, từ đó có thể truy cập tài nguyên bị hạn chế nếu API không xử lý đúng cách.
8. **Khái niệm chính 8**: Testing for server-side parameter pollution in structured data formats
- SSPP trong dữ liệu có cấu trúc (JSON/XML) cho phép kẻ tấn công chèn các tham số không mong muốn vào cấu trúc dữ liệu. Nếu máy chủ không xác thực đúng cách, dữ liệu độc hại có thể thay đổi hành vi của API, như nâng quyền truy cập của người dùng.

- Burp cung cấp các công cụ như Burp Scanner và Backslash Powered Scanner BApp để tự động phát hiện các lỗ hổng SSPP, nhưng vẫn cần kiểm tra thủ công các đầu vào đáng ngờ để xác nhận chúng là lỗ hổng.
- 
### 🚀 **Vulnerabilities Covered**
- **Lỗ hổng 1**: Index listing khi xóa bớt phần của dường dẫn.
    - Khi xóa từng phần của đường dẫn api dẫn tới khi còn đường dẫn cuối thì ra chức năng của admin.
- **Lỗ hổng 2**: Thay đổi được Http method
    - Thay đổi phương thức HTTP (đi kèm nội dung và header) để chỉnh sửa yêu cầu từ GET sang PATCH chỉnh sửa được giá.
- **Lỗ hổng 3**: Lỗ hổng Mass Assignment xảy ra khi tham số từ phía người dùng được ánh xạ trực tiếp vào các thuộc tính nội bộ mà không qua kiểm tra.
    - Request ban đầu gửi đi không trường `discount` tuy nhiên ở một request khác lại phát hiện trường này nên thay vào request ban đầu và thay đổi `discount`
- **Lỗ hổng 4**: Server-Side Parameter Pollution (SSPP) cho phép hacker chèn hoặc ghi đè tham số trong yêu cầu phía server.
    - Thêm tham số mới vào chuỗi truy vấn để lấy token đặt lại mật khẩu của admin. Sử dụng token này để đặt lại mật khẩu và truy cập tài khoản admin.
    - Ví dụ: `username=admin` thì sửa thành `username=admin%26field=x%23` nhờ việc thêm dấu `#` in ra lỗi nên biết có tham số ẩn là `field` và từ đó đọc code js beiets rằng cso mã token và dùng tham số phía `field` để đọc

### 🔨 **Tools/Techniques Used**
1. **OpenAPI Parser BApp**: Để phân tích tài liệu OpenAPI.
2. **JS Link Finder BApp**: Tìm endpoint trong JavaScript.
3. **Content type converter BApp**: Tự động chuyển đổi dữ liệu được gửi trong các yêu cầu giữa XML và JSON.
4. **Param Miner BApp**: Đoán tự động tên tham số ẩn.
5. **Content Discovery Tool**: Tìm nội dung không được liên kết trực tiếp, có thể bao gồm tham số bổ sung.
6. **Backslash Powered Scanner BApp**: tìm ra các lỗ hổng Server-Side Parameter Pollution. Công cụ này phân loại đầu vào thành ba loại: nhàm chán (boring), thú vị (interesting) và có khả năng bị tấn công (vulnerable)

### 🎯 **Key Takeaways**
- **Điểm chính 1**: Bảo mật API document .
- **Điểm chính 2**: Đảm bảo tài liệu của bạn được cập nhật để những người kiểm tra hợp pháp có thể thấy toàn bộ bề mặt tấn công của API.
- **Điểm chính 3**: Áp dụng danh sách cho phép các phương thức HTTP được phép.
- **Điểm chính 4**: Sử dụng thông báo lỗi chung để tránh cung cấp thông tin có thể hữu ích cho kẻ tấn công.
- **Điểm chính 5**: Sử dụng các biện pháp bảo vệ trên tất cả các phiên bản API của bạn, không chỉ phiên bản sản xuất hiện tại.
- **Điểm chính 6**: Để ngăn chặn lỗ hổng Server-Side Parameter Pollution (SSPP), cần sử dụng allowlist cho ký tự hợp lệ, mã hóa đúng cách đầu vào và kiểm tra định dạng, cấu trúc dữ liệu đầu vào trước khi xử lý trên server.

### 📄 **Useful Resources**
- [Tài liệu tham khảo 1](https://portswigger.net/web-security/learning-paths/api-testing)


---
