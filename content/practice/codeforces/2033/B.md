---
title: "CF 2033B - Sakurako và Nước"
description: "Chúng ta có một lưới các số nguyên $n nhân n$, trong đó mỗi ô biểu thị một chiều cao. Giá trị âm đại diện cho các ô “xấu” mà chúng ta muốn loại bỏ bằng cách tăng giá trị."
date: "2026-06-08T11:41:17+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2033
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 981 (Div. 3)"
rating: 900
weight: 2033
solve_time_s: 72
verified: false
draft: false
---

[CF 2033B - Sakurako và Nước](https://codeforces.com/problemset/problem/2033/B) 

**Xếp hạng:** 900 
**Tags:** vũ lực, thuật toán mang tính xây dựng, lòng tham 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times n$lưới các số nguyên, trong đó mỗi ô đại diện cho một chiều cao. Giá trị âm đại diện cho các ô “xấu” mà chúng ta muốn loại bỏ bằng cách tăng giá trị. 

Thao tác duy nhất được phép thực hiện rất cụ thể: chúng tôi chọn một đoạn đường chéo của ma trận con vuông và tăng mọi ô trên đường chéo đó lên 1. Mỗi thao tác ảnh hưởng đến chính xác một đoạn đường chéo chính bên trong một số hình vuông đã chọn. 

Mục tiêu là chuyển đổi lưới sao cho không có giá trị nào còn âm, sử dụng số lượng tối thiểu các phép toán tăng theo đường chéo như vậy. 

Chi tiết quan trọng là mỗi thao tác thực hiện dọc theo đường chéo chứ không phải hàng hoặc cột. Điều đó ngay lập tức gợi ý rằng các ô được ghép dọc theo các đường chéo được lập chỉ mục bởi$i - j$. Mỗi đường chéo có thể được xử lý độc lập vì không có phép toán nào giao nhau giữa các đường chéo. 

Cho rằng$n \le 500$mỗi trường hợp thử nghiệm và tổng số$n$qua các bài kiểm tra là nhỏ, một$O(n^2)$hoặc$O(n^2 \log n)$cách tiếp cận là đủ. Bất cứ điều gì tệ hơn, chẳng hạn như tính toán lại mỗi thao tác hoặc mô phỏng tham lam trên các ma trận phụ, sẽ quá chậm hoặc không chính xác. 

Trường hợp cạnh tinh tế phát sinh khi nhiều ô âm tồn tại trên cùng một đường chéo nhưng ở các vị trí khác nhau. Một kẻ tham lam ngây thơ sửa từng ô một cách độc lập sẽ tính toán quá mức các phép toán, bởi vì một mức tăng theo đường chéo có thể sửa được nhiều giá trị âm cùng một lúc. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liên tục quét lưới, tìm ô âm và áp dụng thao tác bao phủ ô đó. Điều này có vẻ hợp lý vì mỗi thao tác sẽ sửa được ít nhất một ô bị lỗi. Tuy nhiên, điều này không thành công vì một thao tác có thể sửa nhiều ô dọc theo cùng một đường chéo và mô phỏng đơn giản không nắm bắt được sự khớp nối đó một cách hiệu quả. Trong trường hợp xấu nhất, việc quét liên tục lưới sau mỗi thao tác sẽ dẫn đến$O(n^3)$hoặc hành vi tồi tệ hơn. 

Cái nhìn sâu sắc quan trọng là các hoạt động hoạt động độc lập dọc theo các đường chéo được lập chỉ mục bởi$d = i - j$. Trong mỗi đường chéo, chúng ta chỉ quan tâm đến việc cần bao nhiêu mức tăng để nâng tất cả các giá trị âm lên 0. Vì mỗi thao tác làm tăng một đoạn đường chéo liền kề, nên chiến lược tối ưu là “đẩy” số gia dọc theo đường chéo một cách tham lam bất cứ khi nào chúng ta gặp phải mức thâm hụt so với các thao tác được áp dụng trước đó. 

Điều này làm giảm vấn đề xử lý từng đường chéo một cách độc lập trong thời gian tuyến tính và tích lũy số lượng tăng dần cần thiết để đảm bảo tất cả các giá trị trở thành không âm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n^3)$|$O(n^2)$| Quá chậm | 
| Chế biến tham lam theo đường chéo |$O(n^2)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi khai thác thực tế là tất cả các ô có cùng$i - j$nằm trên một đường chéo duy nhất. 

1. Nhóm tất cả các ô theo chỉ số đường chéo$d = i - j$. Mỗi đường chéo được xử lý độc lập vì các phép toán không bao giờ trộn lẫn các đường chéo. 

Lý do mang tính cấu trúc: mọi thao tác được phép đều nằm hoàn toàn trong một đoạn đường chéo. 
2. Đối với mỗi đường chéo, duyệt theo thứ tự tự nhiên từ trên cùng bên trái đến dưới cùng bên phải (chỉ số hàng tăng dần). 
3. Duy trì một biến`carry`, biểu thị tổng số tiền tăng đã được áp dụng cho đường chéo này từ các hoạt động trước đó ảnh hưởng đến các vị trí trước đó. 
4. Tại mỗi ô, tính giá trị hiệu dụng của nó sau các thao tác trước đó:`a[i][j] + carry`. 
5. Nếu giá trị này âm, chúng ta phải áp dụng đủ các thao tác bắt đầu từ vị trí này để đưa nó về 0. Số thao tác cần thực hiện là`need = -value`. 
6. Thêm`need`cho câu trả lời và tăng`carry`qua`need`, vì những thao tác đó ảnh hưởng đến phần còn lại của đoạn đường chéo. 
7. Tiếp tục cho đến hết đường chéo. 

Sau khi xử lý tất cả các đường chéo, các phép toán tích lũy sẽ tạo thành số lượng yêu cầu tối thiểu. 

### Tại sao nó hoạt động 

Bất biến là tại
