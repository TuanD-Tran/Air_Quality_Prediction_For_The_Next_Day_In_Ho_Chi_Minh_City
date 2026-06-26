# **Dự Đoán Chất Lượng Không Khí Cho Ngày Hôm Sau Ở TP.HCM**
Mini project môn Nhập môn Khoa học Dữ liệu

Nhóm 6:
  - 24280113 - Trần Đình Tuấn - Nhóm trưởng
  - 24280011 - Phạm Minh Duy
  - 24280057 - Lê Viết Khánh Đạt
  - 24280078 - Phạm Minh Khoa
  - 24280098 - Lê Phạm Thế Phú
  - 24280112 - Trần Phan Nhật Trường

---

## **I. Tổng Quan**
Dự án này xây dựng một hệ thống dự đoán chỉ số AQI (Air Quality Index) cho Thành phố Hồ Chí Minh vào ngày tiếp theo bằng các phương pháp học máy. Mục tiêu chính là hỗ trợ nhận diện xu hướng ô nhiễm không khí và cung cấp cảnh báo sớm cho người dân cũng như cơ quan quản lý môi trường.

Project gồm 3 thành phần chính:
- Phân tích và tiền xử lý dữ liệu.
- Huấn luyện và đánh giá các mô hình dự đoán.
- Trực quan hóa kết quả thông qua dashboard và biểu đồ minh họa.

---

## **II. Mục Tiêu Dự Án**
- Xây dựng pipeline xử lý dữ liệu chuỗi thời gian phù hợp với bài toán dự báo AQI.
- Tạo các đặc trưng hữu ích như lag features, rolling statistics và feature theo thời gian.
- So sánh hiệu năng của nhiều mô hình hồi quy và phân loại.
- Đánh giá mô hình bằng các chỉ số MAE, RMSE, R² và MAPE.
- Cung cấp một hệ thống có thể chạy được local và có thể mở rộng về sau.

---

## **III. Dữ Liệu Sử Dụng**
Dự án sử dụng dữ liệu lịch sử về chất lượng không khí và các biến liên quan để huấn luyện mô hình dự đoán AQI.

Các file dữ liệu chính:
- [data/air_quality_historical.csv](data/air_quality_historical.csv) - dữ liệu lịch sử về AQI và các chỉ số môi trường.
- [data/city_info.csv](data/city_info.csv) - thông tin về thành phố và các thuộc tính liên quan.
- [data/data_dictionary.csv](data/data_dictionary.csv) - bảng mô tả các cột dữ liệu.

---

## **IV. Quy Trình Thực Hiện**
### **1. Khám phá dữ liệu (EDA)**
Sau khi phân tích dữ liệu, nhóm đã quan sát được xu hướng biến động của AQI theo thời gian, mức độ phân bố và mối liên hệ giữa các biến môi trường.

Một số hình ảnh minh họa từ quá trình EDA:

![Biểu đồ phân phối biến AQI](figures/eda/aqi_dist_hist.png)

![Biểu đồ phân tích theo thời gian](figures/eda/aqi_time_series_decomposition.png)

![Ma trận tương quan giữa các biến](figures/eda/meteo_aqi_corr_matrix.png)

### **2. Tiền xử lý và Feature Engineering**
Các bước chính bao gồm:
- Xử lý giá trị thiếu.
- Tạo target cho bài toán dự đoán ngày tiếp theo.
- Tạo các feature như lag, rolling mean/std/min/max.
- Thêm các feature thời gian như tháng, ngày trong tuần, mùa.

### **3. Huấn luyện mô hình**
Nhóm đã thử nghiệm nhiều mô hình khác nhau cho bài toán hồi quy và phân loại, bao gồm:
- Ridge Regression
- Decision Tree
- Random Forest
- LightGBM
- XGBoost
- SARIMA
- Các mô hình phân loại để chuyển AQI thành các nhãn mức độ ô nhiễm

### **4. Đánh giá mô hình**
Các chỉ số chính được sử dụng để so sánh là:
- MAE
- RMSE
- R²
- MAPE

---

## **V. Kết quả đạt được**
Theo thông tin lưu trong [models/metadata.json](models/metadata.json), mô hình được chọn làm mô hình chính hiện tại là Ridge Regression với các chỉ số đánh giá như sau:

| Metric | Giá trị |
| --- | ---: |
| MAE | 8.27 |
| RMSE | 10.78 |
| R² | 0.75 |
| MAPE | 8.86 |

Mô hình này được huấn luyện với 40 feature đầu vào và mục tiêu là dự đoán giá trị AQI ngày tiếp theo.

### **Minh họa kết quả mô hình**

![So sánh hiệu suất các mô hình](figures/models/all_models_compare.png)

![Thực tế so với dự đoán của mô hình](figures/models/all_models_actual_predicted.png)

![Biểu đồ SHAP](figures/models/SHAP_summary_plot.png)

---

## **VI. Cấu Trúc Dự Án**
```text
Air_Quality_Prediction_For_The_Next_Day_In_Ho_Chi_Minh_City/
├── data/                
├── figures/             
├── models/              
├── notebooks/           
├── outputs/             
├── dashboard.py        
├── predict.py          
├── requirements.txt     
├── README.md
├── REPORT.md    
└── LICENSE
```

### **Chú thích các nhánh chính**
- data/: Chứa dữ liệu đầu vào cho dự án.
- figures/: Lưu các biểu đồ EDA, kết quả mô hình và SHAP.
- models/: Lưu mô hình huấn luyện và metadata để dashboard sử dụng.
- notebooks/: Chứa toàn bộ quy trình phân tích và thử nghiệm mô hình.
- outputs/: Lưu tập dữ liệu đã xử lý cùng kết quả đánh giá.
- dashboard.py: Triển khai dashboard trực quan hóa.
- predict.py: Chạy dự đoán AQI cho ngày tiếp theo.

---

## **VII. Hướng Dẫn Chạy Dự Án**
### **1. Cài đặt môi trường**
```bash
python -m venv .venv
source .venv/bin/activate   # macOS/Linux
.venv\Scripts\activate      # Windows
pip install -r requirements.txt
```

### **2. Chạy notebook**
Mở các file notebook trong thư mục [notebooks](notebooks) để xem toàn bộ quy trình phân tích, tiền xử lý và huấn luyện mô hình.

### **3. Chạy script dự đoán**
```bash
python predict.py
```
Script này sẽ:
- Gọi dữ liệu hiện tại từ API.
- Tạo feature cho ngày tiếp theo.
- Dự đoán AQI.
- Phân loại mức độ ô nhiễm.
- Lưu kết quả vào thư mục [outputs](outputs).

### **4. Chạy dashboard**
```bash
python dashboard.py
```
Sau đó mở trình duyệt tại địa chỉ:
```text
http://localhost:8050
```

---

## **VIII. Tính Năng Nổi Bật**
- Dự đoán AQI cho ngày tiếp theo.
- Phân loại mức ô nhiễm không khí thành các lớp rõ ràng.
- Trực quan hóa dữ liệu và kết quả mô hình bằng biểu đồ.
- Có thể mở rộng thêm dữ liệu thời tiết và các biến ngoại cảnh để cải thiện độ chính xác.

---

## **IV. Hướng Phát Triển Tiếp Theo**
- Bổ sung dữ liệu thời tiết để tăng độ chính xác.
- Thử nghiệm thêm các mô hình thời gian như SARIMA, LSTM hoặc Transformer.
- Tối ưu tham số mô hình và xây dựng hệ thống cảnh báo tự động.
- Kết nối dashboard với dữ liệu cập nhật theo thời gian thực.

---

## **X. Tài Liệu Tham Khảo**
- Dataset chất lượng không khí TP.HCM.
- Các thư viện hỗ trợ: pandas, scikit-learn, xgboost, lightgbm, plotly, dash, shap.

---

## **XI. License**
Dự án này được sử dụng cho mục đích học tập và minh họa trong môn Nhập môn Khoa học Dữ liệu - Nhóm 6 - 24KDL
