# ETL Process with Google Colab

## 1. Giới thiệu dự án

Dự án này trình bày một quy trình ETL (Extract, Transform, Load) hoàn chỉnh được thực hiện trên Google Colab. Mục tiêu là trích xuất dữ liệu từ một nguồn công cộng, làm sạch và biến đổi dữ liệu, sau đó tải dữ liệu đã xử lý vào một định dạng sạch sẽ, sẵn sàng cho việc phân tích hoặc sử dụng tiếp theo. Dự án này tập trung vào việc tự động hóa các bước ETL, đảm bảo tính nhất quán và hiệu quả trong việc xử lý dữ liệu.

## 2. Dữ liệu đã sử dụng

Dữ liệu được sử dụng trong dự án này được trích xuất từ một API công cộng cung cấp thông tin về **[Mô tả ngắn gọn về loại dữ liệu, ví dụ: "tỷ giá hối đoái", "thông tin chứng khoán", "dữ liệu thời tiết", "dữ liệu Covid-19" - dựa trên nội dung Colab của bạn]**.

Nguồn dữ liệu cụ thể là: `[Đường dẫn API hoặc nguồn dữ liệu đã sử dụng trong Colab của bạn, ví dụ: https://api.exchangerate-api.com/v4/latest/USD]`

Dữ liệu ban đầu có thể chứa các giá trị bị thiếu, định dạng không nhất quán hoặc các bất thường khác cần được xử lý trước khi sử dụng.

## 3. Mô tả chi tiết các bước và quyết định

### 3.1. Extract (Trích xuất dữ liệu)

**Mô tả:** Bước này liên quan đến việc thu thập dữ liệu từ nguồn bên ngoài. Trong trường hợp này, chúng tôi đã sử dụng thư viện `requests` của Python để thực hiện một yêu cầu HTTP GET đến API đã cung cấp.

**Quyết định:**
* **Sử dụng thư viện `requests`:** Đây là một thư viện phổ biến và mạnh mẽ trong Python để xử lý các yêu cầu HTTP, cho phép chúng ta dễ dàng tương tác với các API RESTful.
* **Truy cập trực tiếp API:** Thay vì tải xuống một file tĩnh, việc truy cập trực tiếp API đảm bảo chúng ta luôn có được dữ liệu mới nhất.
* **Xử lý phản hồi JSON:** Dữ liệu từ API được trả về dưới định dạng JSON, và chúng tôi đã sử dụng phương thức `.json()` của đối tượng phản hồi để dễ dàng chuyển đổi nó thành một đối tượng Python (dictionary).

### 3.2. Transform (Biến đổi dữ liệu)

**Mô tả:** Bước này tập trung vào việc làm sạch, chuẩn hóa và cấu trúc lại dữ liệu đã trích xuất để nó phù hợp với các yêu cầu phân tích. Các hoạt động chính bao gồm: chuyển đổi dữ liệu thành DataFrame, xử lý các giá trị bị thiếu, đổi tên cột, và định dạng lại dữ liệu nếu cần.

**Quyết định:**
* **Chuyển đổi thành Pandas DataFrame:** Pandas DataFrame cung cấp các công cụ mạnh mẽ và linh hoạt để thao tác và phân tích dữ liệu dạng bảng. Điều này giúp việc làm sạch và biến đổi dữ liệu trở nên dễ dàng và hiệu quả hơn.
* **Xử lý các giá trị thiếu (Missing Values):**
    * **[Quyết định cụ thể của bạn về cách xử lý missing values, ví dụ: "Chúng tôi đã quyết định điền các giá trị thiếu bằng 0 hoặc giá trị trung bình" HOẶC "Chúng tôi đã loại bỏ các hàng có giá trị thiếu nếu chúng chiếm một phần nhỏ trong tổng số dữ liệu và không ảnh hưởng đến tính đại diện của dữ liệu." - Dựa trên logic của bạn trong Colab]**
    * **Lý do:** [Giải thích lý do cho quyết định đó, ví dụ: "Việc điền giá trị 0 là phù hợp cho dữ liệu định lượng khi giá trị thiếu thực sự có nghĩa là không có dữ liệu." HOẶC "Việc loại bỏ các hàng bị thiếu giúp duy trì tính toàn vẹn của dữ liệu và tránh làm sai lệch kết quả phân tích."]
* **Đổi tên cột (Renaming Columns):**
    * **[Quyết định cụ thể của bạn về việc đổi tên cột - Dựa trên logic của bạn trong Colab]**
    * **Lý do:** Việc đổi tên cột thành các tên rõ ràng và dễ hiểu giúp cải thiện khả năng đọc và sử dụng dữ liệu, đặc biệt khi làm việc với nhiều người hoặc trong các dự án lớn.
* **Chuyển đổi kiểu dữ liệu (Data Type Conversion):**
    * **[Quyết định cụ thể của bạn về việc chuyển đổi kiểu dữ liệu, ví dụ: "Chuyển đổi cột 'Giá' thành kiểu số thực (float)"]**
    * **Lý do:** Đảm bảo rằng dữ liệu có kiểu dữ liệu phù hợp là rất quan trọng cho các phép toán số học chính xác và để tránh lỗi trong quá trình phân tích sau này.

### 3.3. Load (Tải dữ liệu)

**Mô tả:** Bước cuối cùng là tải dữ liệu đã được làm sạch và biến đổi vào một đích đến. Trong dự án này, chúng tôi đã lưu DataFrame đã xử lý dưới dạng tệp CSV.

**Quyết định:**
* **Lưu dưới dạng tệp CSV:** CSV là một định dạng file văn bản đơn giản, phổ biến và dễ dàng tương thích với hầu hết các công cụ phân tích và cơ sở dữ liệu.
* **Không bao gồm chỉ mục (index=False):** Việc không bao gồm chỉ mục của DataFrame khi lưu vào CSV giúp giữ cho file sạch sẽ hơn và tránh tạo ra một cột chỉ mục không cần thiết trong dữ liệu cuối cùng.
* **[Nếu bạn lưu vào Google Sheets hoặc Google Drive, hãy mô tả ở đây và giải thích lý do]**
