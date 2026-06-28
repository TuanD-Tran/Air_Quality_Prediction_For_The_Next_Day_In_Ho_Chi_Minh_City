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

---

## **4. KẾ HOẠCH TRIỂN KHAI & THỰC HIỆN DỰ ÁN**

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
