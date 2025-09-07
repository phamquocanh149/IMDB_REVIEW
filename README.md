# 🎬 IMDB Sentiment Classification

## 📌 Giới thiệu
Trong thời đại **bùng nổ dữ liệu** hiện nay, các nền tảng trực tuyến (như Amazon, Netflix, IMDB) nhận được hàng triệu đánh giá và bình luận từ người dùng mỗi ngày.  
Việc **phân tích cảm xúc (Sentiment Analysis)** – xác định xem một đánh giá mang tính **tích cực (Positive)** hay **tiêu cực (Negative)** – có ý nghĩa rất quan trọng trong:  

- 📈 **Kinh doanh**: giúp doanh nghiệp nắm bắt ý kiến khách hàng, cải thiện sản phẩm/dịch vụ.  
- 🎬 **Giải trí**: hỗ trợ người dùng nhanh chóng nhận diện chất lượng phim/sách.  
- 🤖 **Trí tuệ nhân tạo**: là một trong những bài toán nền tảng của **Xử lý ngôn ngữ tự nhiên (NLP)**.  

Tập dữ liệu **IMDB Reviews** gồm **50.000 đánh giá phim** (25k train + 25k test), đã được gán nhãn cân bằng (positive/negative). Đây là một trong những benchmark nổi tiếng cho bài toán **binary sentiment classification**.  

Mục tiêu của dự án:  
- So sánh hiệu quả của các phương pháp từ truyền thống (**Bag of Words, TF-IDF**) đến hiện đại hơn (**LSTM, BERT**).  
- Đánh giá ưu – nhược điểm của từng mô hình.  
- Từ đó rút ra bài học: với dữ liệu và mục tiêu khác nhau, nên chọn mô hình nào để tối ưu.  

---

## 🔹 1. Bag of Words (BoW) + Logistic Regression
- Biểu diễn văn bản bằng **CountVectorizer (BoW)**.  
- Huấn luyện với **Logistic Regression**.  
- ✅ Kết quả:  
  - **Accuracy train ≈ 0.91, test ≈ 0.85**  
  - Mô hình đơn giản nhưng hiệu quả, xử lý nhanh.  

---

## 🔹 2. TF-IDF + Logistic Regression
- Biểu diễn văn bản bằng **TF-IDF**, nhấn mạnh những từ đặc trưng trong từng review.  
- Huấn luyện với **Logistic Regression**.  
- ✅ Kết quả:  
  - **Accuracy train ≈ 0.93, test ≈ 0.88**  
  - Cải thiện hơn BoW do giảm ảnh hưởng của những từ xuất hiện quá thường xuyên.  

---

## 🔹 3. LSTM (Long Short-Term Memory)
- Sử dụng **Embedding Layer + LSTM** để học ngữ cảnh và thứ tự từ trong câu.  
- ✅ Kết quả:  
  - **Accuracy train ≈ 0.99, test ≈ 0.82** → có hiện tượng **overfitting**.  
  - Mô hình hiểu được **chuỗi ngữ nghĩa**, ví dụ:  
    - “This movie is not bad” → Tích cực (Positive).  
    - BoW/TF-IDF khó nhận ra do chỉ đếm từ, còn LSTM hiểu ngữ cảnh phủ định.  
- Có thể cải thiện bằng **dropout, early stopping, regularization**.  

---

## 🔹 4. BERT (Bidirectional Encoder Representations from Transformers)
- Sử dụng **pretrained BERT base model** (fine-tune trên IMDB).  
- ✅ Kết quả:  
  - **Accuracy train ≈ 0.98, test ≈ 0.92**.  
  - Hiểu được ngữ cảnh dài và phức tạp, hạn chế overfitting hơn LSTM.  

---

## 📊 So sánh nhanh
| Phương pháp              | Accuracy (Train) | Accuracy (Test) | Ưu điểm | Hạn chế |
|---------------------------|------------------|-----------------|---------|---------|
| BoW + Logistic Reg.      | ~0.91            | ~0.85           | Nhanh, dễ triển khai | Không hiểu ngữ cảnh |
| TF-IDF + Logistic Reg.   | ~0.93            | ~0.88           | Hiệu quả hơn BoW | Vẫn mất ngữ cảnh |
| LSTM                     | 0.99             | ~0.82           | Hiểu ngữ nghĩa, chuỗi | Dễ overfit, cần GPU |
| BERT                     | 0.98             | ~0.92           | Hiểu ngữ cảnh tốt | Training chậm, nặng |

---

## 📌 Kết luận
- **BoW và TF-IDF với Logistic Regression**: phù hợp khi cần mô hình **nhẹ, nhanh**, kết quả ổn định (85–88% test). TF-IDF tỏ ra hiệu quả hơn nhờ biểu diễn đặc trưng tốt hơn.  
- **LSTM**: cho thấy khả năng học ngữ cảnh nhưng dễ bị **overfitting** nếu không có regularization. Mặc dù train accuracy rất cao (99%), nhưng test chỉ đạt ~82%, kém hơn cả TF-IDF.  
- **BERT**: đạt **kết quả tốt nhất (≈92% test)** nhờ kiến trúc transformer, tuy nhiên chi phí tính toán lớn hơn nhiều.  
- Tùy mục tiêu ứng dụng:  
  - Nếu cần **triển khai nhanh, dữ liệu nhỏ** → TF-IDF + Logistic Regression.  
  - Nếu muốn **khai thác ngữ cảnh sâu hơn, dữ liệu lớn, có GPU** → BERT là lựa chọn hợp lý.  
  - LSTM có thể là bước đệm học tập, nhưng khó cạnh tranh với TF-IDF và BERT trong bài toán này.  

