# Phân tích Hài lòng Khách hàng trong ngành Hàng không

## Giới thiệu dự án

Dự án này tập trung vào việc áp dụng các kỹ thuật khai phá dữ liệu để khám phá các yếu tố chính ảnh hưởng đến trải nghiệm và mức độ hài lòng của hành khách trong ngành hàng không [Giới thiệu]. Mục tiêu là đưa ra các nhận định thực tiễn, giúp các hãng hàng không cải thiện chất lượng dịch vụ và nâng cao trải nghiệm khách hàng.

## Mục tiêu chính

*   Xác định các yếu tố có tác động lớn nhất đến trải nghiệm chuyến bay và đánh giá của hành khách [Giới thiệu].
*   Phân khúc khách hàng dựa trên các đặc điểm và mức độ hài lòng để hiểu rõ hơn về các nhóm khách hàng khác nhau [51, 531, 532].
*   Xây dựng các mô hình dự đoán khả năng hài lòng của khách hàng, hỗ trợ hãng hàng không chủ động can thiệp và cải thiện dịch vụ [51, 531, 552].

## Dữ liệu

Dữ liệu được sử dụng là bộ dữ liệu "Airline Passenger Satisfaction" thu thập từ Kaggle [Giới thiệu].

*   **Số lượng mẫu:** 103,904 hành khách [22, 35, 51].
*   **Số lượng thuộc tính:** 25 thuộc tính ban đầu [22, 35, 51].
*   **Mô tả thuộc tính chính:**
    *   **Thông tin cá nhân:**
        *   `Gender`: Giới tính (Male/Female) [1, 10, 100].
        *   `Age`: Tuổi của hành khách (liên tục) [1, 11].
        *   `Customer Type`: Loại khách hàng (Loyal/Disloyal Customer), phản ánh mức độ gắn bó [1, 10, 100].
    *   **Thông tin chuyến bay:**
        *   `Type of Travel`: Mục đích chuyến bay (Business/Personal Travel) [2, 11, 101].
        *   `Class`: Hạng vé (Business, Eco, Eco Plus), ảnh hưởng trực tiếp đến dịch vụ và kỳ vọng [2, 12, 101].
        *   `Flight Distance`: Khoảng cách chuyến bay (km), tác động đến trải nghiệm dài/ngắn [2, 12, 102].
        *   `Departure Delay in Minutes`: Thời gian trễ khởi hành (phút) [8, 17, 107].
        *   `Arrival Delay in Minutes`: Thời gian trễ đến (phút) [8, 17, 107].
    *   **Đánh giá dịch vụ (thang điểm 1-5):**
        *   `Inflight wifi service`: Dịch vụ Wi-Fi trên máy bay [3, 13, 102].
        *   `Online booking`: Mức độ dễ dàng đặt vé trực tuyến [4, 14, 103].
        *   `Seat comfort`: Sự thoải mái của ghế ngồi [6, 15, 105].
        *   `Food and drink`: Chất lượng đồ ăn và thức uống [5, 14, 104].
        *   `Inflight entertainment`: Các lựa chọn giải trí trên chuyến bay [6, 16, 105].
        *   `On-board service`: Dịch vụ tiếp viên [7, 17, 107].
        *   `Leg room service`: Không gian để chân [7, 16, 106].
        *   `Baggage handling`: Xử lý hành lý [7, 17, 106].
        *   `Checkin service`: Dịch vụ làm thủ tục tại sân bay [7, 17, 106].
        *   `Cleanliness`: Độ sạch sẽ của máy bay [8, 17, 107].
        *   `Gate location`: Vị trí cổng lên máy bay [4, 14, 103].
        *   `Departure/Arrival time convenient`: Sự thuận tiện về thời gian bay [3, 13, 103].
    *   **Biến mục tiêu:** `satisfaction` (satisfied / neutral or dissatisfied) [Giới thiệu, 14, 354].

## Phương pháp luận

Quá trình phân tích được chia thành các bước chính sau:

### 1. Tiền xử lý dữ liệu (Data Preprocessing)

Giai đoạn tiền xử lý đóng vai trò quan trọng, vì chất lượng dữ liệu đầu vào quyết định chất lượng của kết quả phân tích [19, 21, 35, 204].

*   **Xử lý giá trị thiếu (Handling Missing Values):**
    *   **Vấn đề:** Chỉ có 310 giá trị thiếu trên tổng số 103,904 mẫu (chiếm khoảng 0.3%) ở cột `Arrival Delay in Minutes` [22, 36, 39, 50, 298, 342].
    *   **Kỹ thuật:** Điền giá trị thiếu bằng trung vị (median) cho các cột dạng số [21, 22, 35, 36, 39]. Trung vị được chọn vì ít nhạy cảm với các giá trị ngoại lai hoặc phân phối lệch hơn so với giá trị trung bình (mean) [22, 36].
*   **Mã hóa thuộc tính phân loại (Encoding Categorical Attributes):**
    *   **Kỹ thuật:** Sử dụng `LabelEncoder` để chuyển đổi các thuộc tính phân loại (`Gender`, `Customer Type`, `Type of Travel`, `Class`) và biến mục tiêu `satisfaction` (sang 0 và 1) thành định dạng số mà các thuật toán học máy có thể xử lý [41, 392, 473, 551].
*   **Chuẩn hóa dữ liệu (Standardization):**
    *   **Cơ sở lý thuyết:** Nhiều thuật toán khai phá dữ liệu (ví dụ: K-Means, Logistic Regression) hoạt động hiệu quả nhất khi các đặc trưng nằm trên cùng một thang đo. Nếu không chuẩn hóa, các đặc trưng có giá trị lớn hơn có thể lấn át các đặc trưng khác [23, 37].
    *   **Kỹ thuật:** Áp dụng chuẩn hóa Z-score (StandardScaler) để biến đổi dữ liệu sao cho các cột có giá trị trung bình bằng 0 và độ lệch chuẩn bằng 1 [23, 24, 38, 169, 569].
*   **Biến đổi dữ liệu (Data Transformation):**
    *   **Vấn đề:** Một số biến như `Flight Distance`, `Departure Delay in Minutes`, `Arrival Delay in Minutes` có phân phối bị lệch phải nặng [26, 27, 40, 343, 546].
    *   **Kỹ thuật:** Sử dụng phép biến đổi logarit (log1p) để "kéo" các giá trị lớn lại gần nhau, làm cho phân phối trở nên đối xứng và gần chuẩn hơn [25-28].
*   **Xử lý ngoại lai (Outlier Treatment):**
    *   **Vấn đề:** Các cột như `Departure Delay in Minutes` và `Arrival Delay in Minutes` có tỷ lệ ngoại lai cao (hơn 13%), có thể làm sai lệch các phép tính thống kê và ảnh hưởng tiêu cực đến mô hình [30, 43, 416, 547].
    *   **Kỹ thuật:** Áp dụng phương pháp giới hạn giá trị (capping) theo Interquartile Range (IQR) để giảm thiểu ảnh hưởng của các trường hợp cực đoan mà không loại bỏ hoàn toàn dữ liệu [29-31, 43, 44, 549].
*   **Lựa chọn đặc trưng và giảm chiều (Feature Selection & Dimensionality Reduction):**
    *   **Mục tiêu:** Loại bỏ nhiễu, giảm độ phức tạp, và cải thiện hiệu suất mô hình [31, 45].
    *   **Kỹ thuật:** Kết hợp nhiều phương pháp (F-test, Mutual Information, Random Forest Importance, RFE) để chọn ra 23 đặc trưng tốt nhất [32, 46, 47, 61-63, 394, 396, 398, 400]. Sau đó, áp dụng PCA để giảm chiều, giữ lại 90% phương sai, giúp đơn giản hóa dữ liệu cho gom cụm và trực quan hóa [32, 33, 46-48, 64, 65, 403-408, 570].

### 2. Khai phá dữ liệu (Data Mining)

Các kỹ thuật khai phá dữ liệu chính được áp dụng:

*   **Khai thác Luật kết hợp (Association Rules - Apriori):**
    *   **Mục tiêu:** Tìm ra các bộ dịch vụ hoặc đặc điểm thường xuyên xuất hiện cùng nhau (frequent itemsets) hoặc có mối quan hệ phụ thuộc (association rules) [51, 53, 59, 217, 254].
    *   **Quy trình:**
        1.  **Chuyển đổi dữ liệu:** Mỗi khách hàng được xem là một "giao dịch", các đặc điểm của khách hàng (như dịch vụ được đánh giá tốt `rating >= 4`, nhóm tuổi, loại khách hàng, mục đích chuyến đi, trạng thái hài lòng/trễ chuyến) được xem là các "item" trong giao dịch [52, 53, 75-79, 119, 421-431].
        2.  **Áp dụng Apriori:** Sử dụng thuật toán Apriori với ngưỡng `min_support = 0.05` và `min_confidence = 0.6` để tìm các tập phổ biến (4145 frequent itemsets) và sinh ra các luật kết hợp (4321 rules) [55, 59, 65, 68, 81, 84-89, 173, 174, 219, 256, 257, 436-440].
    *   **Insight chính:** Phát hiện các mối quan hệ quan trọng giữa các đặc điểm dịch vụ và nhân khẩu học, bao gồm "nghịch lý" khách hàng trung thành nhưng không hài lòng [57, 65, 69, 80, 177, 193].
*   **Gom cụm (Clustering - K-Means, Hierarchical Clustering):**
    *   **Mục tiêu:** Phân khúc khách hàng thành các nhóm có đặc điểm tương đồng mà không cần nhãn định trước (unsupervised learning) [51, 147, 220, 531, 532].
    *   **Xác định số cụm tối ưu (k):** Sử dụng kết hợp phương pháp Elbow và Silhouette Score, cả hai đều đề xuất `k = 2` là tối ưu [68, 171-179, 531, 535-538, 546, 571-575]. Silhouette Score được ưu tiên hơn vì nó đo lường trực tiếp chất lượng của việc phân cụm [538, 575].
    *   **Thực hiện Clustering:**
        *   **K-Means:** Phân chia 103,904 khách hàng thành 2 cụm [68, 181, 531, 539, 576].
        *   **Hierarchical Clustering (Agglomerative):** Được áp dụng trên một mẫu đại diện (10,000 khách hàng) do kích thước dữ liệu lớn để xây dựng cấu trúc phân cấp, sau đó sử dụng K-Means làm mô hình proxy để gán nhãn cụm cho toàn bộ dữ liệu [70, 154, 183-186, 531, 539, 540, 542, 577, 578]. Hierarchical Clustering được đánh giá là thuật toán tốt nhất trong báo cáo [163, 191, 543].
    *   **Phân tích cụm:** Hai cụm chính được xác định: "Loyal Supporters" (55.6% khách hàng, trung thành và hài lòng) và "Dissatisfied Economy" (44.4% khách hàng, không hài lòng) [161-165, 193-197, 544-548].
*   **Phân loại (Classification - Decision Tree, Random Forest, Naive Bayes, Logistic Regression):**
    *   **Mục tiêu:** Xây dựng mô hình dự đoán `satisfaction` (satisfied / neutral or dissatisfied) dựa trên các đặc trưng khác (supervised learning) [Giới thiệu, 3, 55, 179, 271].
    *   **Chia dữ liệu:** Dữ liệu được chia thành tập huấn luyện (80%) và tập kiểm tra (20%) [44-47, 184, 475-477, 560].
    *   **Mô hình được xây dựng:**
        *   **Decision Tree:** Mô hình "hộp trắng", dễ hiểu và giải thích, giới hạn độ sâu để tránh overfitting [133, 224, 271, 477, 553].
        *   **Naive Bayes:** Thuật toán nhanh, hiệu quả, đóng vai trò baseline [50, 134, 175, 274, 480, 555].
        *   **Random Forest:** Phương pháp học tổ hợp (ensemble method), xây dựng nhiều cây quyết định, giảm overfitting và đạt độ chính xác cao [135, 149, 186, 284, 482, 505, 556, 557, 562].
        *   **Logistic Regression:** Mô hình phân loại tuyến tính, cung cấp xác suất dự đoán và trọng số đặc trưng [137, 153, 287, 487, 509, 558, 559].
    *   **Đánh giá mô hình:** Sử dụng các chỉ số Accuracy, Precision, Recall, F1-Score và Confusion Matrix [9-18, 50, 133-153, 156, 157, 160-530, 562-564].
    *   **Kết quả:** Random Forest là mô hình tối ưu nhất với độ chính xác 94.82% [78-80, 149, 158, 159, 186, 196, 505, 516, 517, 532, 562]. Các đặc trưng quan trọng nhất trong việc dự đoán mức độ hài lòng là `Online boarding` và `Inflight wifi service` [37, 80, 119, 188, 189, 485, 490, 565].

## Kết quả và Phân tích chính

Dự án đã mang lại nhiều insights quan trọng:

*   **Tổng quan hành vi khách hàng:** Hơn 56% khách hàng không hài lòng [297, 341]. Khách hàng hạng Business có tỷ lệ hài lòng cao nhất (gần 70%), trong khi hạng Eco và Eco Plus có tỷ lệ không hài lòng rất cao (hơn 80%) [109, 125, 314, 315, 579]. Khách hàng đi công tác (`Business travel`) thường hài lòng hơn khách đi cá nhân (`Personal Travel`) [110, 323, 580].
*   **Chênh lệch đánh giá dịch vụ:** `Online boarding` có sự chênh lệch đánh giá lớn nhất giữa nhóm hài lòng và không hài lòng (1.41 điểm), tiếp theo là `Inflight entertainment` và `Inflight wifi service` [110, 326, 327, 580]. Điều này cho thấy các dịch vụ số hóa có vai trò quan trọng [76].
*   **Nghịch lý Khách hàng Trung thành - Hài lòng:** Khoảng 81.8% khách hàng là trung thành, nhưng 56.3% trong số họ lại không hài lòng [57, 65, 69, 80, 177, 193, 434]. Đây là một "báo động đỏ" về chất lượng dịch vụ ngay cả với phân khúc khách hàng cốt lõi [69, 80, 177].
*   **Phân khúc khách hàng (Clustering):** Đã phân chia khách hàng thành 2 nhóm chính:
    *   **Cụm 0: "Loyal Supporters" (55.6% khách hàng):** Nhóm khách hàng trung thành (86.4%) với mức độ hài lòng khá cao (65.2%). Họ chủ yếu bay công tác (75.2%) và các chuyến dài hơn (trung bình 1366km). Đây là "xương sống" của hãng [161-163, 193-197, 544, 547-549].
    *   **Cụm 1: "Dissatisfied Economy" (44.4% khách hàng):** Nhóm khách hàng có mức độ hài lòng rất thấp (chỉ 16.0%), thường bay các chuyến ngắn hơn (trung bình 960km). Mặc dù tỷ lệ trung thành vẫn tương đối cao (75.9%), đây là nhóm có nguy cơ rời bỏ cao nhất [165, 166, 193-197, 545, 547, 550, 551].
*   **Yếu tố tác động đến hài lòng (Classification):** `Online boarding` (check-in trực tuyến) và `Inflight wifi service` là hai yếu tố có ảnh hưởng mạnh mẽ nhất đến sự hài lòng của khách hàng, cho thấy sự tiện lợi và trải nghiệm số hóa là "chìa khóa vàng" [37, 76, 80, 119, 188, 189, 485, 490, 565, 566].

## Khuyến nghị

Dựa trên các insights thu được, chúng tôi đề xuất các hành động sau để cải thiện chất lượng dịch vụ và giữ chân khách hàng:

1.  **Ưu tiên cải thiện các dịch vụ số hóa:** Đầu tư mạnh vào hệ thống check-in/boarding kỹ thuật số và nâng cấp chất lượng Wi-Fi trên máy bay, đặc biệt cho khách hàng đi công tác. Đây là những "điểm chạm" quan trọng ảnh hưởng lớn đến hài lòng [81, 189, 565].
2.  **Duy trì và nâng tầm trải nghiệm cho nhóm "Loyal Supporters" (Cụm 0):** Tập trung vào việc duy trì chất lượng dịch vụ cao và cá nhân hóa trải nghiệm. Nhóm này là nguồn doanh thu ổn định và "xương sống" của hãng [82, 164, 197, 549].
3.  **Hành động khẩn cấp cho nhóm "Dissatisfied Economy" (Cụm 1):** Cải thiện dịch vụ cơ bản và triển khai các chương trình phục hồi lòng tin để tránh mất khách hàng. Nhóm này đại diện cho rủi ro lớn nhất [82, 166, 197, 551].
4.  **Giải quyết "Nghịch lý Trung thành - Không hài lòng":** Cần phân tích sâu hơn nguyên nhân khiến khách hàng trung thành vẫn không hài lòng và có các biện pháp cải thiện dịch vụ kịp thời để chuyển họ thành "người hâm mộ thực sự" [57, 65, 70, 80, 81, 177, 193].
5.  **Tối ưu hóa phân khúc khách hàng công tác:** Nhóm khách hàng đi công tác chiếm ưu thế và có xu hướng trung thành cao; cần tập trung chiến lược và các gói dịch vụ phù hợp cho phân khúc này [66, 67, 82, 175].

## Hạn chế của dự án

Dự án này có một số hạn chế cần lưu ý:

*   **Chất lượng cụm:** Chỉ số Silhouette Score (khoảng 0.21) cho thấy các cụm có thể chưa được phân tách hoàn hảo, ảnh hưởng đến độ rõ ràng của từng phân khúc khách hàng [90, 197, 198].
*   **Giả định trong Feature Engineering:** Một số đặc trưng tổng hợp (`Price_Sensitivity`, `Satisfaction_Risk`) được xây dựng dựa trên các giả định, cần có dữ liệu thực tế để xác thực [91, 198].
*   **Tính giải thích của mô hình:** Random Forest, dù chính xác cao, vẫn là mô hình "hộp đen", khó giải thích sâu sắc quy trình dự đoán so với Decision Tree hay Logistic Regression [91, 198].
*   **Dữ liệu tĩnh:** Các phân tích và mô hình dựa trên một tập dữ liệu tại một thời điểm. Trong thực tế, dữ liệu liên tục thay đổi, đòi hỏi giải pháp linh hoạt hơn [91, 198].

## Công nghệ sử dụng

*   **Python**
*   **Thư viện xử lý dữ liệu:** `pandas`, `numpy` [293, 418, 477].
*   **Thư viện trực quan hóa:** `matplotlib`, `seaborn` [293, 418, 477].
*   **Thư viện học máy:** `scikit-learn` (StandardScaler, LabelEncoder, KMeans, AgglomerativeClustering, PCA, DecisionTreeClassifier, GaussianNB, RandomForestClassifier, LogisticRegression, SelectKBest, RFE, f_classif, mutual_info_classif, train_test_split, cross_val_score, StratifiedKFold, classification_report, confusion_matrix, accuracy_score, silhouette_score, silhouette_samples) [293, 294, 418, 477, 480, 482, 486, 495, 573].
*   **Thư viện khai phá luật kết hợp:** `mlxtend` (TransactionEncoder, apriori, association_rules) [72, 418].
*   **Thư viện thống kê:** `scipy.stats` (chi2_contingency, ttest_ind, pearsonr, skew, kurtosis) [294, 306, 315, 318, 319, 330, 343, 352, 389].
*   **Thư viện đồ thị:** `networkx` [99, 455].
*   **Thư viện phân cấp:** `scipy.cluster.hierarchy` (linkage, dendrogram) [294].

## Cách chạy dự án

1.  **Cài đặt các thư viện cần thiết:**
    ```bash
    pip install pandas numpy matplotlib seaborn scikit-learn mlxtend networkx scipy
    ```
2.  **Clone repository:**
    ```bash
    git clone <URL_repository>
    cd <tên_thư_mục>
    ```
3.  **Tải dữ liệu:** Đảm bảo file `dataset_clean_customer.csv` có sẵn trong thư mục dự án hoặc cập nhật đường dẫn trong mã nguồn. (Hiện tại đường dẫn đã được cung cấp trực tiếp trong mã nguồn là `https://github.com/papoibe/data-mining-flight/releases/download/v2.0/dataset_clean_customer.csv`) [294].
4.  **Chạy Jupyter Notebook hoặc script Python:** Mở và chạy các file `.ipynb` hoặc `.py` theo thứ tự các bước phân tích (Tiền xử lý, Khai phá luật kết hợp, Gom cụm, Phân loại).
