---
title: "CF 1248A - Điểm nguyên"
description: "Chúng ta có hai họ đường thẳng trên mặt phẳng. Họ thứ nhất bao gồm các dòng có dạng $y = x + pi$, và họ thứ hai bao gồm các dòng có dạng $y = -x + qj$. Mọi $pi$ đều khác biệt trong nhóm của chính nó và mọi $qj$ đều khác biệt trong nhóm riêng của nó."
date: "2026-06-13T20:48:09+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 1248
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 594 (Div. 2)"
rating: 1000
weight: 1248
solve_time_s: 217
verified: true
draft: false
---

[CF 1248A - Điểm nguyên](https://codeforces.com/problemset/problem/1248/A) 

**Đánh giá:** 1000 
**Thẻ:** hình học, toán học 
**Thời gian giải:** 3 phút 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai họ đường thẳng trên mặt phẳng. Họ đầu tiên bao gồm các dòng có dạng$y = x + p_i$và họ thứ hai bao gồm các dòng có dạng$y = -x + q_j$. Mọi$p_i$là khác biệt trong nhóm riêng của nó, và mọi$q_j$khác biệt trong nhóm riêng của nó. 

Mỗi đường thẳng từ họ thứ nhất cắt mọi đường thẳng từ họ thứ hai tại đúng một điểm. Nhiệm vụ là đếm xem có bao nhiêu giao điểm có cả hai tọa độ là số nguyên. 

Một quan sát quan trọng là chúng ta không được yêu cầu tính toán tọa độ hoặc kiểm tra các giao điểm về mặt hình học theo nghĩa tổng quát. Chúng ta chỉ cần quyết định, cho mỗi cặp$(p_i, q_j)$, điểm giao nhau của chúng có nằm trên tọa độ nguyên hay không và đếm xem có bao nhiêu cặp như vậy thỏa mãn điều kiện này. 

Các ràng buộc cho thấy rõ rằng việc sử dụng vũ lực đối với tất cả các cặp là không khả thi. Với$n, m$lên tới$10^5$cho mỗi trường hợp thử nghiệm và tổng số tiền lên tới$10^5$, MỘT$O(nm)$cách tiếp cận sẽ yêu cầu lên tới$10^{10}$hoạt động trong trường hợp xấu nhất, vượt xa giới hạn 2 giây. Điều này ngay lập tức buộc chúng ta phải hướng tới một$O(n + m)$hoặc$O(n \log n)$giải pháp phong cách cho mỗi bài kiểm tra. 

Một điểm tinh tế thường gây ra sai lầm là hiểu sai ý nghĩa của “điểm giao nhau số nguyên” trong cách thiết lập này. Giao luôn tồn tại và duy nhất nhưng không phải lúc nào cũng tích phân mặc dù tất cả các hệ số đều là số nguyên. Một vấn đề khác là tính hai lần hoặc cố gắng tính toán tọa độ một cách rõ ràng cho tất cả các cặp, điều này là không cần thiết. 

Một ví dụ cạm bẫy ngây thơ là: 

đầu vào:```
n = 2, p = [0, 1]
m = 2, q = [0, 2]
```Giao điểm của$y = x + p$Và$y = -x + q$xảy ra vào lúc:$$x = \frac{q - p}{2}, \quad y = \frac{p + q}{2}$$Mặc dù tất cả đầu vào đều là số nguyên nhưng kết quả chỉ tích phân khi cả hai$q - p$Và$p + q$đều chẵn. Nhiều giải pháp ngây thơ cho rằng tất cả các giao điểm đều là số nguyên hoặc quên điều kiện chẵn lẻ. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là lặp lại từng cặp$(p_i, q_j)$, tính điểm giao nhau và kiểm tra xem cả hai tọa độ có phải là số nguyên hay không. Việc này rất đơn giản: giải phương trình, tính toán$x = (q_j - p_i)/2$,$y = (q_j + p_i)/2$và xác minh khả năng chia hết cho 2. Mặc dù đúng nhưng điều này yêu cầu$O(nm)$thời gian cho mỗi trường hợp thử nghiệm, điều này trở nên không khả thi ngay cả đối với kích thước đầu vào vừa phải. 

Cấu trúc của công thức cho thấy một điều kiện đơn giản hơn nhiều. Giao điểm là nguyên khi và chỉ khi cả hai$q_j - p_i$Và$q_j + p_i$đều chẵn. Hai điều kiện này tương đương với việc yêu cầu$p_i$Và$q_j$có cùng độ ngang bằng. Nếu như$p_i$là số chẵn và$q_j$thế thì kỳ quặc$p_i + q_j$là số lẻ và không thể tạo ra số nguyên$y$-điều phối. Vấn đề tương tự xảy ra nếu tính chẵn lẻ bị đảo ngược. 

Vì vậy, toàn bộ vấn đề giảm xuống việc đếm xem có bao nhiêu cặp có giá trị chẵn lẻ phù hợp. Chúng tôi chỉ cần chia cả hai mảng thành số chẵn và số lẻ rồi nhân các nhóm tương ứng. 

Cho phép$E_p, O_p$là số chẵn và số lẻ$p_i$, và tương tự$E_q, O_q$vì$q_j$. Vậy thì câu trả lời là:$$E_p \cdot E_q + O_p \cdot O_q$$Brute-force hoạt động vì nó kiểm tra rõ ràng mọi giao điểm hình học, nhưng nó không thành công vì nó liên tục tính toán lại cùng một điều kiện chẵn lẻ. Nhận xét rằng tính tích phân chỉ phụ thuộc vào tính chẵn lẻ sẽ biến bài toán hình học thành bài toán đếm trên hai nhóm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nm)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n + m)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng$p$và đếm xem có bao nhiêu giá trị chẵn và bao nhiêu giá trị lẻ. Điều này phân tách tất cả các dòng của biểu mẫu$y = x + p_i$thành hai lớp dựa trên tính chẵn lẻ, điều này xác định đầy đủ cách chúng tương tác với họ thứ hai. 
2. Đọc mảng$q$và tương tự đếm các giá trị chẵn và lẻ. Điều này phản ánh sự phân loại tương tự cho các dòng$y = -x + q_j$. 
3. Tính số cặp hợp lệ trong đó cả hai giá trị đều chẵn, góp phần$E_p \cdot E_q$giao điểm có tọa độ nguyên. 
4. Tính số cặp hợp lệ trong đó cả hai giá trị đều là số lẻ, góp phần$O_p \cdot O_q$giao lộ hợp lệ. 
5. Tính tổng hai phần đóng góp này và đưa ra kết quả. 

### Tại sao nó hoạt động 

Giao điểm của$y = x + p$Và$y = -x + q$là:$$x = \frac{q - p}{2}, \quad y = \frac{p + q}{2}$$Cả hai tọa độ đều là số nguyên khi$p + q$chẵn, điều này xảy ra khi và chỉ khi$p$Và$q$chia sẻ cùng một sự bình đẳng. Vì mỗi cặp hoạt động độc lập và chỉ phụ thuộc vào tính chẵn lẻ, nên việc nhóm các giá trị theo tính chẵn lẻ sẽ bảo toàn tất cả thông tin cần thiết và đảm bảo tính chính xác. 

## Giải pháp Python```
PythonRun
```Giải pháp xử lý từng trường hợp thử nghiệm một cách độc lập. Chi tiết triển khai chính là tách số lượng chẵn lẻ trong một lần chuyển qua từng mảng, tránh mọi lần lặp lồng nhau. Bước nhân cuối cùng trực tiếp mã hóa việc ghép cặp tổ hợp của các lớp chẵn lẻ phù hợp. 

Một lỗi phổ biến là tính toán lại các điều kiện giao lộ một cách rõ ràng hoặc quên rằng cả hai tọa độ phải đồng thời là số nguyên. Một cách khác là trộn lẫn điều kiện và đếm các cặp chẵn lẻ không khớp thay vì các cặp chẵn lẻ khớp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```

```| Bước | Tập | Op | phương trình | Ối | Đóng góp | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 0 | 0 | 0 | 0 | 0 | 0 | 
| Sau p | 1 | 2 | 0 | 0 | 0 | 0 | 
| Sau q | 1 | 2 | 1 | 1 | 1_1 + 2_1 | 3 | 

Bảng này cho thấy việc nhóm chẵn lẻ làm giảm vấn đề thành việc đếm đơn giản như thế nào. Câu trả lời cuối cùng khớp với số cặp chẵn lẻ hợp lệ. 

### Ví dụ 2 

đầu vào:```

```| Bước | Tập | Op | phương trình | Ối | Đóng góp | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 0 | 0 | 0 | 0 | 0 | 0 | 
| Sau p | 1 | 0 | 0 | 0 | 0 | 0 | 
| Sau q | 1 | 0 | 0 | 1 | 1_0 + 0_1 | 0 | 

Điều này chứng tỏ trường hợp tất cả các giao điểm đều thất bại do không khớp chẵn lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$| Mỗi mảng được quét một lần cho mỗi trường hợp thử nghiệm | 
| Không gian |$O(1)$| Chỉ có bốn bộ đếm được sử dụng bất kể kích thước đầu vào | 

Tổng kích thước đầu vào trên các trường hợp thử nghiệm là$10^5$, do đó việc quét tuyến tính cho mỗi trường hợp kiểm thử vẫn dễ dàng thực hiện trong giới hạn thời gian. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều | 6 | ghép đôi đầy đủ giữa các sự kiện | 
| kỳ quặc | 4 | đối xứng cho các tập hợp lẻ | 
| chẵn lẻ hỗn hợp | 2 | tách chính xác các nhóm chẵn lẻ | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các giá trị trong cả hai mảng có cùng tính chẵn lẻ. Ví dụ, nếu tất cả$p_i$và tất cả$q_j$chẵn, mỗi cặp đóng góp một giao điểm số nguyên hợp lệ. Thuật toán tính$n \cdot m$bởi vì cả hai số lẻ đều bằng 0 và số chẵn chiếm ưu thế, khớp với tích Descartes đầy đủ. 

Một trường hợp khác là sự không khớp chẵn lẻ hoàn toàn, chẳng hạn như tất cả$p_i$là đồng đều và tất cả$q_j$trở nên kỳ quặc. Công thức lực giao nhau$p + q$luôn luôn lẻ, nên không có cặp nào đóng góp. Thuật toán tính toán chính xác$E_p \cdot E_q = 0$Và$O_p \cdot O_q = 0$, cho kết quả bằng không. 

Trường hợp tinh tế cuối cùng là kích thước đầu vào tối thiểu trong đó$n = m = 1$. Nếu như$p_1 = q_1$, câu trả lời chỉ phụ thuộc vào đẳng thức chẵn lẻ. Thuật toán giảm chính xác thành một so sánh duy nhất về tính chẵn lẻ, tránh mọi vấn đề về cách viết hoa đặc biệt hoặc lỗi riêng lẻ.
