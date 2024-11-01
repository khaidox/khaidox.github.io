---
created: 2023-12-01
title: Tracking people in real time
layout: post
tags: [jetson,deeplearning,machinelearning]
category: [Jetson Nano]
author: khaido
comments: true
---

> This is a note on my past study progress

Hiện thực, tối ưu và kiểm nghiệm module truy vết con người trên môi trường thiết bị nhúng NVIDIA Jetson Nano Developer Kit.


# 1. Giải pháp

![dl1](https://khaidox.github.io/assets/img/deeplearning/1.png)

# 2. Xây dựng bộ phát hiện đối tượng người

a. Phương pháp máy học (HOG + SVM + Sliding Windown + Non-Maximum Suppression)

b. Phương pháp mạng học sâu (YOLO)

Từ đó, thực nghiệm lựa chọn phương pháp cho độ chính xác cao và tốc độ nhanh.

## 1. Chuẩn bị tập dữ liệu
Tập dữ liệu người dành cho việc phát hiện đối tượng người. Sử dụng bộ dữ liệu Pascal VOC 2012 và 2007 gồm 20 lớp sau đó thực hiện trích xuất lấy mỗi lớp người.
![dl2](https://khaidox.github.io/assets/img/deeplearning/2.png)

## 2. Phương pháp máy học (HOG + SVM + Sliding Windown + Non-Maximum Suppression)
![dl3](https://khaidox.github.io/assets/img/deeplearning/3.png)

Có 5 bước để xây dựng một vector `HOG` cho hình ảnh:
- Tiền xử lý ảnh đầu vào.
- Tính toán độ dốc (gradient) của ảnh.
- Tính histogram của các ô (cell) 8x8.
- Chuẩn hóa khối (block) 16x16.
- Tính vector đặc trưng HOG.

`SVM` là một thuật toán học có giám sát được sử dụng phổ biến trong các bài toán phân lớp (classification) dựa trên lý thuyết học thống kê, được đề xuất bởi Vapnik (1998). Ý tưởng chính của SVM là xây dựng một siêu phẳng (hyperplane) để phân tách các điểm dữ liệu. Siêu phẳng này sẽ chia không gian thành các miền khác nhau và mỗi miền sẽ chứa một loại dữ liệu.

`Sliding windows` là một khu vực hình chữ nhật có chiều rộng và chiều cao cố định và trượt qua một hình ảnh. Đối với mỗi cửa sổ này, chúng ta sẽ lấy vùng cửa sổ và áp dụng trình phân loại hình ảnh để xác định xem cửa sổ có đối tượng mà cần quan tâm hay không. Kỹ thuật đơn giản này đóng một vai trò cực kỳ quan trọng trong việc phát hiện đối tượng và phân loại hình ảnh.

Sau khi thực hiện phương pháp HOG kết hợp với SVM và kỹ thuật cửa sổ trượt cho ra kết quả bước ban đầu phát hiện được đối tượng là con người nhưng vấn đề với cách làm này là chúng phát hiện nhiều hộp giới hạn xung quanh các đối tượng con người trong ảnh. Do đó, nhiệm vụ cần xác định một hộp giới hạn trong nhiều hộp giới hạn xung quanh đối tượng con người

### Nhân xét:
Thực hiện đánh giá kết quả việc sử dụng phương pháp máy học đã trình bày ở mục trên bằng cách sử dụng 500 tấm ảnh được chọn ngẫu nhiên từ tập dữ liệu test. Với 500 ảnh có tổng 1261 đối tượng. Sau khi thực hiện thực nghiệm bộ phát hiện đối tượng người đã trình bày trên thì được kết quả như sau:

Số lượng đối tượng phát hiện được: 100/1261 đối tượng

Thời gian thực thi: 13619 giây

Việc sử dụng trích xuất đặc trưng HOG, phân loại bằng thuật toán SVM sau đó thực hiện phát hiện và vẽ hộp giới hạn cho đối tượng con người cho kết quả có độ chính xác thấp. Độ chính xác thực nghiệm chỉ đạt 7.9%. Các đối tượng người bị ảnh hưởng bởi ánh sáng hoặc các đối tượng chồng lên nhau thì phương pháp này không thật sự hiệu quả. Thời gian xử lý là chậm, việc xử lý một ảnh bằng phương pháp trên để phát hiện đối tượng người mất 27.24 giây /ảnh


## 3. Phát hiện con người sử dụng phương pháp mạng học sâu
Dưới đây là sơ đồ tổng quát cho bài toán phát hiện đối tượng là con người sử dụng phương pháp mạng học sâu cụ thể là mạng học sâu YOLO.

![dl3](https://khaidox.github.io/assets/img/deeplearning/4.png)

YOLO là một giải pháp cho bài toán nhận dạng đối tượng dựa trên mạng tích chập neuron được đề xuất bởi Joseph Redmon và các cộng sự lần đầu tiên vào năm 2015. Không giống với các phương pháp khác, YOLO xem bài toán nhận dạng đối tượng như là một bài toán hồi quy nghĩa là vị trí của các đối tượng và lớp của chúng được tính toán bằng cách đưa giá trị các pixel của ảnh qua một mạng neuron duy nhất. YOLO có rất nhiều điểm mạnh so với các phương pháp truyền thống. YOLO là một mạng học sâu kết hợp giữa convolutional và fully connected layers. Ý tưởng cốt lõi của YOLO là thay vì sử dụng các vùng đề xuất để rút trích đặc trưng thì YOLO sử dụng các thông tin cục bộ từ dữ liệu huấn luyện để học các đặc trưng cần quan tâm bằng cách chia ảnh dữ liệu đầu vào thành lưới (grid view) SxS = 7x7 để khai thác đặc trưng trên lưới. YOLO sẽ dự đoán xem trong mỗi ô xem liệu có đối tượng mà điểm trung tâm rơi vào ô đó và dự đoán điểm trung tâm, kích thước của đối tượng đó và xác suất đối tượng đó là đối tượng nào trong các đối tượng cần xác định. Mỗi ô này có trách nhiệm dự đoán 2 hộp (B = 2) bao quanh. Mỗi 1 hộp mô tả hình chữ nhật bao quanh một đối tượng. Giả sử, đang cần huấn luyện YOLO nhận dạng C = 20 đối tượng khác nhau. Sau khi qua các layers, image input sẽ được biến đổi thành 1 tensor kích thước S x S x (B * 5 + C) = 7x7x30. Có thể hiểu là mỗi box sẽ có 30 tham số. Tham số 1, 2, 3, 4 lần lượt là x_center, y_center, width, height của box. Tham số thứ 5 là xác suất đối tượng. Các tham số còn lại đại diện cho lớp đối tượng.


![dl3](https://khaidox.github.io/assets/img/deeplearning/5.png)

### Nhân xét:
Cũng sử dụng 500 tấm ảnh đã sử dụng để đánh giá thực nghiệm phát hiện đối tượng bằng phương pháp máy học cho việc đánh giá hiệu quả việc sử dụng mạng học sâu YOLOv3-tiny

Số lượng đối tượng phát hiện được: 771/1261 đối tượng

Thời gian thực thi: 232 giây

Phát hiện đối tượng con người sử dụng mạng học sâu mô hình YOLOv3-tiny cho ra kết quả chính xác hơn so với phương pháp đã trình bày mục 2.5. Tỉ lệ độ chính xác thực nghiệm đạt được 61.14%. Tốc độ xử lý được cải thiện hơn.

## 4. Tiến hành sử dụng thư viện TensorRT để tăng tốc cho việc suy luận.
![dl3](https://khaidox.github.io/assets/img/deeplearning/6.png)

Từ kết quả so sánh hiệu suất trên chúng ta đo được với mẫu thử 500 ảnh. Thấy rằng trên GPU tốc độ xử lý trung bình cho mỗi ảnh là 0.094 giây /1 ảnh. Còn nếu sử dụng GPU với TensorRT thì mất 0.032 giây/1 ảnh nhanh gấp gần 3 lần so với chỉ sử dụng GPU để thực hiện suy luận trên ảnh đầu vào.

# 2. Xây dựng bộ theo dõi đối tượng người

Tiếp theo là quá trình xử lý để theo vết nhằm tìm ra đường chuyển động của đối tượng
![dl3](https://khaidox.github.io/assets/img/deeplearning/7.png)

# 3. Thuật toán truy vết đối tượng người

![dl3](https://khaidox.github.io/assets/img/deeplearning/8.png)