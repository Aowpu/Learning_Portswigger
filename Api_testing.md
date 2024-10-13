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


### 🚀 **Vulnerabilities Covered**
- **Lỗ hổng 1**: Index listing khi xóa bớt phần của dường dẫn.
    - Khi xóa từng phần của đường dẫn api dẫn tới khi còn đường dẫn cuối thì ra chức năng của admin.
- **Lỗ hổng 2**: Thay đổi được Http method
    - Thay đổi phương thức HTTP (đi kèm nội dung và header) để chỉnh sửa yêu cầu từ GET sang PATCH chỉnh sửa được giá.
- **Lỗ hổng 3**: Lỗ hổng Mass Assignment xảy ra khi tham số từ phía người dùng được ánh xạ trực tiếp vào các thuộc tính nội bộ mà không qua kiểm tra.
    - Request ban đầu gửi đi không trường `discount` tuy nhiên ở một request khác lại phát hiện trường này nên thay vào request ban đầu và thay đổi `discount`
- **Lỗ hổng 4**:


### 🔨 **Tools/Techniques Used**
1. **OpenAPI Parser BApp**: Để phân tích tài liệu OpenAPI.
2. **JS Link Finder BApp**: Tìm endpoint trong JavaScript.
3. **Content type converter BApp**: Tự động chuyển đổi dữ liệu được gửi trong các yêu cầu giữa XML và JSON.
4. **Param Miner BApp**: Đoán tự động tên tham số ẩn.
5. **Content Discovery Tool**: Tìm nội dung không được liên kết trực tiếp, có thể bao gồm tham số bổ sung.
6. ****:

### 🎯 **Key Takeaways**
- **Điểm chính 1**: Bảo mật API document .
- **Điểm chính 2**: Đảm bảo tài liệu của bạn được cập nhật để những người kiểm tra hợp pháp có thể thấy toàn bộ bề mặt tấn công của API.
- **Điểm chính 3**: Áp dụng danh sách cho phép các phương thức HTTP được phép.
- **Điểm chính 4**: Sử dụng thông báo lỗi chung để tránh cung cấp thông tin có thể hữu ích cho kẻ tấn công.
- **Điểm chính 5**: Sử dụng các biện pháp bảo vệ trên tất cả các phiên bản API của bạn, không chỉ phiên bản sản xuất hiện tại.

### 💡 **Additional Notes**
- Ghi chú bổ sung nếu có.

### 📄 **Useful Resources**
- [Tài liệu tham khảo 1](#)
- [Tài liệu tham khảo 2](#)

---

## 📚 Module: [Tên Module]
- **Ngày học**: [Ngày/tháng/năm]

### 🔑 **Key Concepts**
1. **Khái niệm chính 1**: Mô tả ngắn gọn.
2. **Khái niệm chính 2**: Mô tả ngắn gọn.
3. **Khái niệm chính 3**: Mô tả ngắn gọn.

### 🚀 **Vulnerabilities Covered**
- **Lỗ hổng 1**: Mô tả lỗ hổng và cách khai thác.
- **Lỗ hổng 2**: Mô tả lỗ hổng và cách khai thác.
- **Lỗ hổng 3**: Mô tả lỗ hổng và cách khai thác.

### 🔨 **Tools/Techniques Used**
1. **Tool 1**: Cách sử dụng.
2. **Tool 2**: Cách sử dụng.
3. **Kỹ thuật 1**: Mô tả chi tiết.

### 🎯 **Key Takeaways**
- **Điểm chính 1**: Tóm tắt kiến thức quan trọng.
- **Điểm chính 2**: Tóm tắt kiến thức quan trọng.
- **Điểm chính 3**: Tóm tắt kiến thức quan trọng.

### 💡 **Additional Notes**
- Ghi chú bổ sung nếu có.

### 📄 **Useful Resources**
- [Tài liệu tham khảo 1](#)
- [Tài liệu tham khảo 2](#)
