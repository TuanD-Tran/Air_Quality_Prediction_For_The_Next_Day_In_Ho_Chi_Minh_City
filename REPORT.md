# **BÁO CÁO DỰ ÁN - DỰ ĐOÁN CHẤT LƯỢNG KHÔNG KHÍ CHO NGÀY HÔM SAU Ở TP. HỒ CHÍ MINH**
**Mini project môn Nhập môn Khoa học Dữ liệu**  

**Nhóm 6:**
  - 24280113 - Trần Đình Tuấn - Nhóm trưởng
  - 24280011 - Phạm Minh Duy
  - 24280057 - Lê Viết Khánh Đạt
  - 24280078 - Phạm Minh Khoa
  - 24280098 - Lê Phạm Thế Phú
  - 24280112 - Trần Phan Nhật Trường

**Thời gian thực hiện:** Tháng 06/2026 - Tháng 07/2026  

**Ngày hoàn thành báo cáo:** 26/06/2026

---
<div style="text-align: justify;">
  
## **1. PHÂN CÔNG CÔNG VIỆC & ĐÓNG GÓP CỦA CÁC THÀNH VIÊN**

| MSSV | Họ Tên | Đóng Góp | Tỷ Lệ Đóng Góp |
| :---: | :---: | :--- | :---: |
| 24280113 | **Trần Đình Tuấn** | LightGBM, Dashboard, Phân công nhiệm vụ, Điều hành, Tổng hợp, Điều chỉnh | % |
| 24280011 | **Phạm Minh Duy** | XGBoost, Bài toán Classification, API, Viết README.md | % |
| 24280057 | **Lê Viết Khánh Đạt** | Ridge Regression (Baseline), SHAP Values | % |
| 24280078 | **Phạm Minh Khoa** | SARIMA, SHAP Values | % |
| 24280098 | **Lê Phạm Thế Phú** | Random Forest Regressor, API, Đánh giá Best Model, Viết báo cáo | % |
| 24280112 | **Trần Phan Nhật Trường** | Decision Tree Regressor, Bài toán Classification | % |

---

## **2. CẤU TRÚC MÃ NGUỒN DỰ ÁN**
Dự án được tổ chức theo một cấu trúc thư mục rõ ràng, hỗ trợ quy trình CRISP-DM từ thu thập dữ liệu đến triển khai mô hình:

``` text
Air_Quality_Prediction_For_The_Next_Day_In_Ho_Chi_Minh_City/
│
│
├── data/                          # Dữ liệu thô (Raw Data)
│   ├── air_quality_historical.csv # Chuỗi thời gian lịch sử AQI (2022-2026, 1253 ngày)
│   ├── city_info.csv              # Thông tin thành phố, tọa độ, dân số
│   ├── data_dictionary.csv        # Từ điển dữ liệu, mô tả các biến
│   └── README.md                  # Hướng dẫn chi tiết về dataset
│
├── notebooks/                     # Jupyter Notebooks (Quy trình từng bước)
│   ├── 00_notebooks_overall.ipynb # Tổng quan toàn bộ pipeline & kết nối các notebook
│   ├── 01_eda.ipynb               # Exploratory Data Analysis (EDA) - phân phối, xu hướng, mùa vụ
│   ├── 02_preprocessing.ipynb     # Tiền xử lý & Feature Engineering - lag, rolling, sin/cos
│   ├── 03_model_regression.ipynb  # Xây dựng mô hình Regression (Ridge, DT, RF, XGB, LGBM, SARIMA)
│   ├── 04_model_classification.ipynb # Xây dựng mô hình Classification (RF, XGB, LightGBM)
│   └── 05_shap_explainability.ipynb  # Giải thích mô hình bằng SHAP (Summary, Beeswarm, Waterfall, Dependence)
│
├── models/                        # Mô hình đã huấn luyện (Serialized Models)
│   ├── best_regressor.pkl         # Best Regressor Model (Ridge Regression, R²=0.7491)
│   ├── best_classifier.pkl        # Best Classifier Model (LightGBM Tuned, Macro F1=0.4973)
│   ├── ridge_regressor.pkl        # Ridge Regression baseline model
│   ├── dt_regressor.pkl           # Decision Tree Regressor
│   ├── rf_regressor.pkl           # Random Forest Regressor
│   ├── xgb_regressor.pkl          # XGBoost Regressor
│   ├── lgbm_regressor.pkl         # LightGBM Regressor
│   ├── sarima_model.pkl           # SARIMA Model (Classical Statistics baseline)
│   ├── ensemble_dict.pkl          # Ensemble dict chứa tất cả mô hình cho so sánh
│   ├── scaler.pkl                 # RobustScaler cho chuẩn hóa dữ liệu
│   ├── feature_cols.pkl           # Danh sách tên và thứ tự các feature (40 features)
│   └── metadata.json              # Thông tin metadata về best_regressor 
│
├── figures/                       # Các biểu đồ minh họa (30 biểu đồ)
│   │
│   ├── eda/                       # Biểu đồ từ phân tích EDA (9 files)
│   │   ├── aqi_dist_hist.png             # Phân phối Histogram AQI - phân tích xu hướng trung tâm
│   │   ├── aqi_dist_pie.png              # Pie Chart phân bố theo 6 mức độ AQI
│   │   ├── aqi_monthly_dist.png          # Phân bố AQI theo tháng
│   │   ├── aqi_yearly_comparison.png     # So sánh AQI qua các năm (2022-2026)
│   │   ├── aqi_daily_monthly_heatmap.png # Heatmap AQI theo ngày-tháng
│   │   ├── aqi_rolling_mean.png          # Đường trung bình động
│   │   ├── aqi_time_series_decomposition.png # Phân tích thành phần - Observed, Trend, Seasonal, Residual
│   │   ├── meteo_aqi_corr_matrix.png     # Ma trận tương quan - AQI vs 13 biến input
│   │   └── monitoring_stations_map.html  # Bản đồ Folium các trạm quan sát
│   │
│   ├── models/                    # Biểu đồ từ mô hình (20 files)
│   │   ├── all_models_compare.png           # So sánh R², MAE, RMSE, MAPE của tất cả mô hình
│   │   ├── all_models_actual_predicted.png  # Actual vs Predicted Scatter plot tất cả mô hình
│   │   ├── all_models_mae_by_month.png      # MAE phân tích theo tháng
│   │   ├── all_models_scatter_residual.png  # Residual plot phân tích sai số
│   │   ├── ridge_coefficients.png           # Top 20 hệ số Ridge - Feature Importance
│   │   ├── dt_feature_importance.png        # Feature Importance Decision Tree
│   │   ├── rf_feature_importance.png        # Feature Importance Random Forest
│   │   ├── xgb_feature_importance.png       # Feature Importance XGBoost
│   │   ├── lgbm_feature_importance.png      # Feature Importance LightGBM
│   │   ├── sarima_aqi.png                   # Dự đoán SARIMA vs Actual AQI
│   │   ├── sarima_aqi_rolling_forecast.png  # SARIMA rolling forecast kỳ vọng
│   │   ├── sarima_compare.png               # So sánh SARIMA vs ML Models
│   │   ├── lgbm_xgb_aqi_compare.png         # So sánh LightGBM vs XGBoost
│   │   ├── all_model_classifier_compare.png # So sánh Classification metrics - Accuracy, Precision, Recall, F1
│   │   ├── imbalanced_data.png              # Phân bố lớp mất cân bằng - 76.5% Moderate, 18.6% Sensitive, ...
│   │   ├── confusion_matrix.png             # Confusion Matrix LightGBM Classifier
│   │   ├── SHAP_summary_plot.png            # SHAP Bar Chart - Top 20 features quan trọng nhất
│   │   ├── SHAP_beeswarm_plot.png           # SHAP Beeswarm Plot - chiều hướng tác động của features
│   │   ├── SHAP_waterfall_plot.png          # SHAP Waterfall - giải thích 1 dự đoán cụ thể (AQI cao nhất)
│   │   └── SHAP_dependence_plot.png         # SHAP Dependence - tương tác pm2_5 vs is_dry_season
│   │
│   └── preprocessing/             # Biểu đồ từ tiền xử lý (1 file)
│       └── boxplot_check_outliers.png  # Kiểm tra outliers trước xử lý - biểu đồ hộp 13 biến
│
├── outputs/                       # Dữ liệu đã xử lý & Kết quả dự đoán (9 files)
│   ├── data_processed.csv              # Dữ liệu sau xử lý
│   ├── X_train.csv                     # Features tập Train
│   ├── y_train.csv                     # Target tập Train
│   ├── X_test.csv                      # Features tập Test
│   ├── y_test.csv                      # Target tập Test
│   ├── regression_results.csv          # Kết quả dự đoán Regression
│   ├── classification_results.csv      # Kết quả phân loại Classification
│   ├── predictions_log.csv             # Nhật ký các lần dự đoán hàng ngày từ predict.py
│   └── latest_prediction.json          # Dự đoán mới nhất (cho API/Dashboard) - JSON format
│
├── dashboard.py                   # Ứng dụng Dashboard Dash (4 tab: EDA, Model, SHAP, Predictions)
├── predict.py                     # Script dự đoán tự động hàng ngày (chạy 22:00 hàng ngày, gọi Open-Meteo API)
├── Bảng Kế Hoạch.docx             # Bảng kế hoạch chi tiết - phân công, timeline 4 giai đoạn
├── README.md                      # Hướng dẫn chung về dự án - Cách chạy, cấu hình, dependencies
├── REPORT.md                      # Báo cáo chi tiết 11 phần (File hiện tại - tài liệu toàn diện)
├── LICENSE                        # Giấy phép dự án (MIT hoặc tương tự)
└── requirements.txt               # Danh sách Python packages cần thiết
```

**Ý nghĩa chi tiết cấu trúc:**

- **Thư mục `data/`** - Chứa dữ liệu thô từ Open-Meteo API, cần thiết cho mọi phân tích sau
- **Thư mục `notebooks/`** - Pipeline toàn bộ từ A-Z: EDA → Preprocessing → Modeling → Explainability:
  - Mỗi notebook là một bước trong quy trình CRISP-DM
  - File `00_notebooks_overall.ipynb` tóm tắt tổng quan
- **Thư mục `models/`** - Lưu trữ các mô hình đã huấn luyện và các artifact cần thiết:
  - `best_regressor.pkl` (Ridge) & `best_classifier.pkl` (LightGBM Tuned) là mô hình sản xuất
  - Các mô hình khác lưu để so sánh và đánh giá
  - `metadata.json` chứa các chỉ số hiệu năng chính cho tường thuật
- **Thư mục `figures/`** - Tổ chức biểu đồ theo 3 giai đoạn:
  - `eda/` - 9 biểu đồ phân tích dữ liệu thô (phân phối, chuỗi thời gian, tương quan)
  - `preprocessing/` - 1 biểu đồ kiểm tra outliers
  - `models/` - 20 biểu đồ so sánh mô hình, SHAP explainability
- **Thư mục `outputs/`** - Kết quả trung gian:
  - Train/Test sets để kiểm toán lại kết quả
  - `predictions_log.csv` để theo dõi độ chính xác theo thời gian
  - `latest_prediction.json` để phục vụ API real-time
- **File `dashboard.py`** - Ứng dụng Dash web tương tác (4 tab EDA/Model/SHAP/Predictions)
- **File `predict.py`** - Cron job tự động dự đoán mỗi ngày 22:00
- **File `Bảng Kế Hoạch.docx`** - Tài liệu quản lý dự án (phân công, timeline, deliverables)
- **File `README.md`** - Hướng dẫn nhanh setup và chạy dự án
- **File `REPORT.md`** - Báo cáo toàn diện 11 phần (tài liệu chính thức cuối cùng)

## **3. TỔNG QUAN & ĐẶT VẤN ĐỀ**

### **3.1. Bối Cảnh & Động Lực**
Hiện nay, chất lượng không khí đang là một trong những thách thức môi trường trọng tâm tại TP. Hồ Chí Minh. Dữ liệu quan trắc trong giai đoạn 2022-2026 cho thấy chỉ số AQI (Air Quality Index) trung bình của thành phố đạt mức 81.6, thuộc nhóm Moderate (Trung bình) theo thang đo US. Tuy nhiên, con số trung bình này chưa phản ánh đầy đủ tính chất biến động phức tạp và mức độ rủi ro thực tế đối với sức khỏe cộng đồng. Cụ thể:

- **Tần suất ô nhiễm cao:** Thành phố ghi nhận nhiều ngày có chỉ số AQI vượt ngưỡng 100, rơi vào mức cảnh báo Unhealthy for Sensitive Groups, gây hại trực tiếp cho các nhóm nhạy cảm như trẻ em, người cao tuổi và bệnh nhân hô hấp.

- **Tính mùa vụ khắc nghiệt:** Tình trạng ô nhiễm thường đạt đỉnh vào mùa khô (từ tháng 12 đến tháng 3 năm sau) với các mốc AQI có thể vượt xa ngưỡng 150.

- **Sự kiện ô nhiễm cực đoan:** Điển hình là giai đoạn từ cuối năm 2024 đến giữa năm 2025, thành phố đã trải qua một đợt suy giảm chất lượng không khí nghiêm trọng, đẩy chỉ số AQI trung bình trượt 30 ngày (30-day rolling mean) lên gần mốc 140.

Từ những biến động phức tạp và khó lường trên, việc xây dựng một hệ thống **dự báo chất lượng không khí trước một ngày** trở thành một nhu cầu cấp thiết. Dự án được thực hiện nhằm giải quyết ba động lực cốt lõi:

- **Hỗ trợ người dân chủ động bảo vệ sức khỏe:** Cung cấp bản tin dự báo sớm giúp cư dân thiết lập kế hoạch sinh hoạt phù hợp, chủ động phòng tránh và hạn chế tối đa các hoạt động ngoài trời vào những ngày ô nhiễm nặng.

- **Cung cấp cơ sở cho cơ quan quản lý:** Hệ thống đóng vai trò như một công cụ tham khảo khoa học, giúp các cơ quan y tế và môi trường kịp thời hoạch định chiến lược ứng phó và phát đi cảnh báo rủi ro đến cộng đồng.

- **Nâng cao nhận thức cộng đồng:** Cụ thể hóa các số liệu kỹ thuật thành các mức cảnh báo trực quan giúp công chúng dễ dàng tiếp cận, hiểu rõ tác động của môi trường và hình thành thói quen theo dõi chất lượng không khí hàng ngày.

> Nhằm giải quyết triệt để bài toán thực tiễn nêu trên, dự án đã xác định các mục tiêu hành động cụ thể và thiết lập một phạm vi nghiên cứu nghiêm ngặt về cả không gian lẫn thời gian.

### **3.2. Mục Tiêu & Phạm Vi Dự Án**

#### **3.2.1. Mục tiêu chiến lược**
Dự án được định hướng triển khai toàn diện qua các mục tiêu kỹ thuật sau:

- **Khám phá dữ liệu chuyên sâu:** Thực hiện EDA trên bộ dữ liệu chất lượng không khí và phân tích tính mùa vụ bằng phương pháp phân tách chuỗi thời gian (Time Series Decomposition).

- **Xây dựng hệ thống dự báo:** Huấn luyện, tối ưu hóa và so sánh hiệu suất của nhiều mô hình Machine Learning khác nhau để dự đoán chính xác chỉ số không khí của ngày hôm sau.

- **Minh bạch hóa mô hình:** Ứng dụng lý thuyết SHAP Values để giải thích cơ chế ra quyết định của mô hình, trả lời tường tận câu hỏi về nguyên nhân gây biến động chỉ số.

- **Tự động hóa luồng dữ liệu:** Tích hợp API thời gian thực để thu thập dữ liệu khí tượng/môi trường cuối ngày và tự động đưa ra dự báo cho ngày tiếp theo.

- **Trực quan hóa thành phẩm:** Xây dựng ứng dụng Dashboard tương tác thông qua Dash/Plotly với cấu trúc hệ thống các biểu đồ trực quan.

- **Đóng gói chuẩn hóa khoa học:** Trình bày toàn bộ kết quả thực nghiệm dưới dạng hệ thống báo cáo, các notebook Jupyter và các script Python hoàn chỉnh.

#### **3.2.2. Phạm vi nghiên cứu**

- **Không gian (Địa điểm):** `Khu vực TP. Hồ Chí Minh (tọa độ 10.82°N, 106.63°E)` với quy mô dân số tiếp cận đạt khoảng 14 triệu người vào năm 2026.

- **Thời gian (Giai đoạn):** Từ ngày 01/08/2022 đến ngày 18/02/2026, tương đương 1.298 ngày quan trắc liên tục với tần suất dữ liệu theo ngày.

- **Đối tượng dự báo (Biến mục tiêu):** Chỉ số chất lượng không khí US AQI của ngày t+1 (ngày hôm sau).

> Trong các thành phần cấu thành phạm vi nghiên cứu, việc lựa chọn thước đo để định hình biến mục tiêu đóng vai trò quyết định đến độ chính xác và tính thực tiễn của toàn bộ các mô hình học máy phía sau.

### **3.3. Lý Do Lựa Chọn Biến Mục Tiêu: Tại Sao Lại Là US AQI?**
Thay vì sử dụng chỉ số European AQI hoặc dự đoán độc lập nồng độ các chất thô như PM2.5, dự án quyết định chọn `US AQI của ngày t+1` làm biến mục tiêu cốt lõi vì các lý do khoa học sau:

1. **Tính tổng hợp toàn diện:** `US AQI` không đo lường một chất riêng lẻ mà là chỉ số đại diện tối đa được tính toán từ 6 tác nhân ô nhiễm chính bao gồm PM2.5, PM10, O₃, NO₂, SO₂, và CO. Dự đoán chỉ số này đồng nghĩa với việc bắt được bức tranh toàn cảnh về mức độ ô nhiễm của ngày mà không cần phải xây dựng nhiều mô hình rời rạc cho từng biến thô.

2. **Độ phổ biến và tính kiểm chứng cao:** Ở Việt Nam, hầu hết các ứng dụng và trạm quan trắc quốc tế uy tín (như IQAir, AQICN) đều sử dụng `US AQI` làm chuẩn định dạng công bố. Việc chọn thang đo này giúp kết quả của mô hình dễ dàng đối chiếu, so sánh và kiểm chứng với dữ liệu thực tế.

3. **Thang đo trực quan, hướng đến người dùng cuối:** So với việc đưa ra thông số kỹ thuật thuần túy như nồng độ bụi mịn đạt `34.2 µg/m³` (vốn khó hiểu với người xem), `US AQI` quy đổi dữ liệu về thang điểm chuẩn từ 0 đến 300+ chia làm 6 mức phân tách rõ ràng (Good, Moderate, Unhealthy...). Người dùng nhìn vào có thể lập tức hiểu ngay mức độ nguy hại để tự động đưa ra hành động bảo vệ sức khỏe.

4. **Hỗ trợ tối ưu cho bài toán Phân loại (Classification):** Việc sở hữu các ngưỡng phân cấp rõ ràng của cấu trúc `US AQI` tạo tiền đề hoàn hảo để nhóm xây dựng song song bài toán Classification. Mô hình phân loại này sẽ chuyển hóa các dự báo định lượng thành các mức cảnh báo rủi ro thực tế phục vụ cộng đồng.

> Khi mục tiêu dự báo đã được làm rõ dựa trên nền tảng dữ liệu `US AQI`, nhóm đã thiết lập một chiến lược thử nghiệm đa dạng bao gồm cả mô hình thống kê cổ điển lẫn các thuật toán học máy tiên tiến nhằm tìm ra giải pháp tối ưu nhất.

### **3.4. Định Hướng Phương Pháp Học Máy & Các Mô Hình Triển Khai**

Để đảm bảo tính khách quan và tìm ra kiến trúc có khả năng xử lý tốt nhất dữ liệu chuỗi thời gian dạng bảng (Tabular Time Series), dự án tiến hành triển khai thử nghiệm đồng loạt 6 mô hình thuộc các thuật toán khác nhau:

| Hệ thống mô hình | Kiến trúc thuật toán | Lý do lựa chọn thực nghiệm |
| :--- | :--- | :--- |
| **LightGBM** | Học máy nâng cao (Gradient Boosting) | Tốc độ huấn luyện cực nhanh, độ chính xác cao, tối ưu bộ nhớ và có khả năng xử lý tốt dữ liệu Tabular Time Series chứa nhiễu hoặc outliers |
| **XGBoost** | Học máy nâng cao (Gradient Boosting) | Thuật toán mạnh mẽ, phổ biến trong các bài toán dự báo học thuật, có độ chính xác cao và hỗ trợ tích hợp sâu với thư viện giải thích mô hình SHAP |
| **Random Forest** | Học máy dạng tập hợp (Ensemble Learning) | Đảm bảo tính ổn định cao nhờ cơ chế trung bình hóa các cây quyết định độc lập, có khả năng chống quá khớp (overfitting) và cung cấp bảng đo Feature Importance trực quan |
| **Decision Tree** | Học máy cơ bản (Tree-based Model) | Đóng vai trò làm Baseline cơ sở cho nhóm mô hình cấu trúc cây nhằm đánh giá mức độ cải tiến hiệu năng của các thuật toán dạng tập hợp phức tạp hơn |
| **Ridge Regression** | Học máy tuyến tính (Linear Model) | Đóng vai trò làm Baseline cơ sở cho toàn bộ bài toán Hồi quy, giúp đánh giá nhanh mối quan hệ tuyến tính tổng quát trước khi áp dụng các mô hình phi tuyến |
| **SARIMA** | Thống kê truyền thống (Time Series Cổ điển) | Mô hình chuẩn mực trong phân tích chuỗi thời gian có yếu tố mùa vụ, được sử dụng làm đối chứng để chứng minh sự vượt trội của Machine Learning khi kết hợp với Kỹ thuật đặc trưng (Feature Engineering) |

> **Giả thuyết mô hình tối ưu (Dự đoán ban đầu):** Dựa trên đặc tính dữ liệu chuỗi thời gian phân cấp phức tạp, mô hình thuộc nhóm Gradient Boosting (LightGBM hoặc XGBoost) được kỳ vọng sẽ đạt hiệu năng cao nhất và trở thành Best Model của dự án.

---

## **4. KẾ HOẠCH TRIỂN KHAI & THỰC HIỆN DỰ ÁN**
Dự án được tổ chức theo quy trình chuẩn của một bài toán Khoa học Dữ liệu, chia làm 4 giai đoạn cụ thể với mốc thời gian rõ ràng:

### **Giai Đoạn 1: Thu Thập Dữ Liệu và Khám Phá (EDA)**
**Mục tiêu:** Hiểu rõ đặc tính, cấu trúc và phân phối của bộ dữ liệu không khí tại TP.HCM  
**Thời hạn:** 07/06/2026

| Hoạt động | Nội dung chi tiết | Phụ trách chính |
| :--- | :--- | :--- |
| **Thu thập & Đánh giá** | Tải dataset 1.298 ngày (01/08/2022 - 18/02/2026) từ Kaggle, đọc từ điển dữ liệu, kiểm tra missing values | Tuấn (Hiểu rõ dữ liệu để lên kế hoạch và phân công) |
| **Phân tích Phân phối** | Vẽ biểu đồ Histogram và Pie chart cho US AQI | Khoa |
| **Tương quan & Bản đồ** | Vẽ heatmap tương quan giữa các chất ô nhiễm và bản đồ trạm quan trắc bằng Folium | Phú, Duy |
| **Phân tích Chuỗi thời gian** | Trực quan hóa Time Series Decomposition, tìm ra quy luật mùa khô/mưa | Tuấn |

### **Giai Đoạn 2: Tiền Xử Lý & Feature Engineering**
**Mục tiêu:** Làm sạch dữ liệu và tạo ra các đặc trưng mới để tăng sức mạnh cho mô hình  
**Thời hạn:** 07/06/2026

| Hoạt động | Nội dung chi tiết | Phụ trách chính |
| :--- | :--- | :--- |
| **Xử lý Nhiễu** | Nội suy (interpolate) hoặc loại bỏ (drop) các giá trị khuyết thiếu và xử lý ngoại lai (outliers) | Duy |
| **Trích xuất Đặc trưng** | Tạo Lag features (t-1 đến t-14) và Rolling features (cửa sổ 3, 7, 14, 30 ngày) | Phú |
| **Đặc trưng Thời gian** | Tạo các biến sai phân, biến nhị phân (`is_weekend`), mã hóa tuần hoàn Sin/Cos | Khoa, Đạt |
| **Chia tập & Chuẩn hóa** | Chia Train/Test tỉ lệ 80/20 (bắt buộc không dùng shuffle) và chuẩn hóa bằng RobustScaler | Đạt, Trường |

**Kết quả:** Tạo ra 40 features sau engineering, tập dữ liệu sẵn sàng cho việc huấn luyện.

### **Giai Đoạn 3: Mô Hình Machine Learning & SHAP**
**Mục tiêu:** Xây dựng, tinh chỉnh các mô hình dự đoán (Hồi quy & Phân loại), đánh giá hiệu suất chi tiết và ứng dụng SHAP để giải thích mô hình  
**Thời hạn:** 17/06/2026

#### **1. Bài Toán Regression - Dự Đoán Giá Trị AQI**
**Tất cả các mô hình đều được đánh giá chung bằng 5 metrics: MAE, MSE, RMSE, $R^2$, MAPE.**

| Hạng mục | Chi tiết công việc | Phụ trách |
| :--- | :--- | :--- |
| **Huấn luyện Mô hình** | • Train Ridge Regression (Baseline) <br> • Train Decision Tree Regressor <br> • Train Random Forest Regressor <br> • Train XGBoost <br> • Train LightGBM <br> • Kết hợp XGBoost và LightGBM <br> • Train SARIMA (Mô hình thống kê) | Đạt, Trường, Phú, Duy, Tuấn, Khoa |
| **So sánh Tổng hợp & Phân tích SARIMA** | • Vẽ biểu đồ so sánh hiệu suất giữa các mô hình <br> • Phân tích SARIMA vs ML: So sánh RMSE/MAE <br> • Viết kết luận: Đánh giá khả năng ML vượt trội SARIMA nhờ lag/rolling features | Đạt, Khoa |
| **Đánh giá Best Model** | • Tính các chỉ số tổng quát trên tập Test/Validation, lập bảng so sánh <br> • Lọc ra 2-3 mô hình tốt nhất để vẽ: <br>&nbsp;&nbsp; - *Actual vs Predicted* theo thời gian <br>&nbsp;&nbsp; - *Scatter + đường lý tưởng* $y=x$. <br>&nbsp;&nbsp; - *Residual (Histogram) distribution* <br>&nbsp;&nbsp; - *MAE theo từng tháng* (tìm tháng sai số nhiều nhất) <br> • **Output:** Lưu `best_regressor` + `metadata.json` (tên model, metrics) bằng `joblib` | Phú, Tuấn |

#### **2. Bài Toán Classification - Dự Đoán Mức AQI**
**Bài toán phân loại nhằm chia chất lượng không khí theo các mức độ.**

| Hạng mục | Chi tiết công việc | Phụ trách |
| :--- | :--- | :--- |
| **Xử lý Dữ liệu & Huấn luyện** | • Phân tích lý do mất cân bằng dữ liệu (nhãn Moderate chiếm 76.5%) và giải thích lý do cần dùng `class_weight='balanced'` <br> • Train Random Forest (`class_weight="balanced"`) <br> • Train XGBoost (`scale_pos_weight` cho imbalanced data) <br> • Train LightGBM Classifier | Duy, Trường |
| **Đánh giá & So sánh** | • Đánh giá qua các metrics: Accuracy, F1-score (weighted), Classification Report <br> • Vẽ Confusion Matrix cho Best Classifier | Duy, Trường |
| **Hành động Thực tiễn** | • Trình bày ý nghĩa thực tiễn: Good/Moderate/Unhealthy $\rightarrow$ Đề xuất hành động tương ứng <br> • **Output:** Lưu `best_classifier.pkl` | Duy, Trường |

#### **3. SHAP Values - Giải Thích Mô Hình Hồi Quy**
**Yêu cầu tiên quyết: Phải hoàn thành file `best_regressor.pkl` ở phần Regression.**

| Hạng mục | Chi tiết công việc | Phụ trách |
| :--- | :--- | :--- |
| **Cài đặt & Trực quan hóa** | • Cài đặt thư viện (`pip install shap`) <br> • Tính toán SHAP values cho Best Model <br> • Vẽ **SHAP Summary Plot** (tìm các top features quan trọng nhất) <br> • Vẽ **SHAP Waterfall Plot** (giải thích cho 1 dự đoán cụ thể) | Đạt, Khoa |
| **Phân tích Insight** | • Phân tích feature nào có tác động đẩy giá trị AQI lên/xuống nhiều nhất <br> • Viết nhận xét chuyên sâu (VD: Biến `pm2_5_lag1` đóng vai trò quan trọng hơn các biến chỉ số khác như thế nào) | Đạt, Khoa |

### **Giai Đoạn 4: API, Dashboard & Báo Cáo**
**Mục tiêu:** Tự động hóa hệ thống dự đoán và trực quan hóa kết quả cho người dùng cuối  
**Thời hạn:** 26/06/2026

| Hoạt động | Nội dung chi tiết | Phụ trách chính |
| :--- | :--- | :--- |
| **Tích hợp API Tự động** | Tích hợp Open-Meteo API. Pipeline chạy lúc 22:00 mỗi ngày để lấy data và dự báo cho ngày mai | Duy, Phú |
| **Xây dựng Dashboard** | Thiết kế ứng dụng Dash/Plotly với 6 KPI cards và biểu đồ tương tác (Gauge, Heatmap, Line chart...) | Tuấn |
| **Kiểm thử Thực tế** | Lưu dự đoán hàng ngày vào `predictions_log.csv`. Kiểm thử pipeline từ 20/06/2026 đến 01/07/2026 | Duy, Phú |
| **Hoàn thiện Văn bản** | Soát lỗi code, viết README.md, dọn dẹp Jupyter Notebook và hoàn thành báo cáo tổng kết | Cả nhóm |

---

## **5. MÔ TẢ DỮ LIỆU (GIAI ĐOẠN 1)**

---

## **6. KHÁM PHÁ DỮ LIỆU (EXPLORATORY DATA ANALYSIS - EDA) (GIAI ĐOẠN 1)**

---

## **7. TIỀN XỬ LÝ & KỸ THUẬT ĐẶC TRƯNG (PREPROCESSING & FEATURE ENGINEERING) (GIAI ĐOẠN 2)**

---

## **8. XÂY DỰNG & ĐÁNH GIÁ MÔ HÌNH (GIAI ĐOẠN 3)**

---

## **9. GIẢI THÍCH MÔ HÌNH (SHAP VALUES) (GIAI ĐOẠN 3)**

---

## **10. TRIỂN KHAI THỰC TẾ (API & DASHBOARD) (GIAI ĐOẠN 4)**

---

## **11. TỔNG KẾT & BÀI HỌC**

---

**Ngày hoàn thành báo cáo:** 26 Tháng 06 Năm 2026  
**Nhóm thực hiện:** Nhóm 6 - Nhập môn Khoa học Dữ liệu - 24KDL

</div>

---
