# [Xây Dựng Giải Pháp Business Intelligence Trên Nền Tảng Đám Mây Microsoft Azure Kết Hợp ELT Động](https://github.com/trannhatnguyen2/K20406C_BoKho)

## Member of group

### **`BoKho`**

| student_id | class   | full_name        | role   |
| ---------- | ------- | ---------------- | ------ |
| K204061440 | K20406T | Tran Nhat Nguyen | Leader |
| K204061446 | K20406C | Man Dac Sang     | Member |

# 📕 Table of contents

<!--ts-->

- 🛠️ [Requirements](#️-requirements)
- 🧙‍♂️ [Data Source](#-data-source)
- 🚀 [Solution](#-solution)
- 🌊 [Building Data Lake](#-building-data-lake)
- 🧱 [Building Data Warehouse](#-building-data-warehouse)
- 📊 [Result](#️-result)
- 📂 [Files](#-files)
<!--te-->

 <br />

# 🛠️ Requirements

Các doanh nghiệp đang sử dụng nhiều hệ thống khác nhau và dữ liệu được phân tán ở nhiều nguồn và được định dạng ở các loại tệp khác nhau. Điều này dẫn đến việc nhập và lưu trữ dữ liệu gặp khó khăn, vì thế nếu gặp sai lệch có thể dẫn đến nhiều hậu quả xấu như mất tính nhất quán, tạo ra chi phí không cần thiết và ảnh hưởng đến quá trình ra quyết định của doanh nghiệp.

# 🧙‍♂️ Data Source

Dữ liệu được lấy từ Kaggle để thực nghiệm. Chia làm 3 nguồn khác nhau:

<p align="center">
<img src="./img/datasource.png" width=80% height=80%>

<p align="center">
    Data Sources
</p>

1. `Databases`: ghi nhận hoạt động bán hàng trên sàn thương mại điện tử tại Brazil về các đơn hàng

<p align="center">
<img src="./img/datasource.png" width=80% height=80%>

<p align="center">
    ERD model
</p>

2. `Accounting Systems`: ghi nhận và quản lý thông tin thanh toán của khách hàng về các đơn hàng
3. `Web Services`: bình luận khách hàng về sản phẩm và dịch vụ

# 🚀 Solution

<p align="center">
<img src="https://github.com/trannhatnguyen2/BI_BoKho/blob/main/img/BI_Process.png" width=100% height=100%>

<p align="center">
    BI Solution
</p>

- Step 1: Xác định các nguồn dữ liệu và loại định dạng tệp của từng nguồn
- Step 2: Trích xuất dữ liệu vào vùng chứa `rawdata` bằng Python Script; thực hiện quy trình ELT động vào vùng chứa `curated` với mục đích lưu trữ, đồng thời những dữ liệu cần cho mục đích phân tích được tải vào Azure SQL Server
- Step 3: Thực hiện quy trình ETL dữ liệu vào Data Warehouse bằng Data Factory
- Step 4: Trực quan hóa dữ liệu với Power BI

# 🌊 Building Data Lake

## Container

Công cụ sử dụng để tạo các vùng chứa dữ liệu trên nền tảng Azure là Blob Storage

<p align="center">
<img src="https://github.com/trannhatnguyen2/BI_BoKho/blob/main/img/DataWarehouse_StarSchema.png" width=70% height=70%>

<p align="center">
    Containers
</p>

### **rawdata**

Một bản sao chính xác của dữ liệu từ các nguồn và được sắp xếp theo thư mục có tổ chức

./RAWDATA
├── .accounting_systems/ <- Accounting System Source
│ ├── Payment_2018_01.csv
│ ├── Payment_2018_02.csv  
│ └── Payment_2018_03.csv
│
├── .databases/ <- Databases Source
│ ├── Customer_2018_01.csv
│ ├── Customer_2018_02.csv  
│ ├── Customer_2018_03.csv
│ ├── Order_2018_01.csv  
│ ├── Order_2018_02.csv
│ |── Order_2018_03.csv  
| ├── OrderItem_2018_01.csv
│ |── OrderItem_2018_02.csv
│ └── OrderItem_2018_03.csv  
│
├── .web_services/ <- Web Services Source
│ ├── Review_2018_01.zip  
│ ├── Review_2018_01.zip  
│ └── Review_2018_01.zip

### **staging**

Được sử dụng để giải nén tất cả các tệp nén để chuẩn bị cho quá trình nhập dữ liệu vào `curated`

### **curated**

./CURATED
├── .EXTERNAL/  
│ ├── .Review/
│ ├── .2018/
│ ├── .01/  
│ └── Review_2018_01.json  
│ ├── .02/
│ └── Review_2018_02.json  
│ ├── .03/  
│ └── Review_2018_03.json  
│
├── .INTERNAL/
│ ├── .Accounting/
│ ├── .Payment/
│ ├── .2018/
│ ├── .01/  
│ └── Payment_2018_01.csv  
│ ├── .02/
│ └── Payment_2018_02.csv  
│ ├── .03/  
│ └── Payment_2018_03.csv
│ ├── .Sales/
│ ├── .Customer/
│ ├── .2018/
│ ├── .01/  
│ └── Customer_2018_01.csv  
│ ├── .02/
│ └── Customer_2018_02.csv  
│ ├── .03/  
│ └── Customer_2018_03.csv
│ ├── .Order/
│ ├── .2018/
│ ├── .01/  
│ └── Order_2018_01.csv  
│ ├── .02/
│ └── Order_2018_02.csv  
│ ├── .03/  
│ └── Order_2018_03.csv
│ ├── .OrderItem/
│ ├── .2018/
│ ├── .01/  
│ └── OrderItemm_2018_01.csv  
│ ├── .02/
│ └── OrderItemm_2018_02.csv  
│ ├── .03/  
│ └── OrderItemm_2018_03.csv

# 🧱 Building Data Warehouse
