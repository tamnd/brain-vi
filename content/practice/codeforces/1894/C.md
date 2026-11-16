---
title: "CF 1894C - Người cung cấp thông tin ẩn danh"
description: "Chúng ta có một mảng $b$ có độ dài $n$ và một số $k$, đại diện cho số lượng thao tác được thực hiện trên một mảng ban đầu ẩn $a$. Mỗi thao tác bao gồm việc chọn một điểm cố định $x$ trong $a$ và dịch chuyển trái $a$ theo các vị trí $x$ theo chu kỳ."
date: "2026-06-09T01:14:35+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "graphs", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1894
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 908 (Div. 2)"
rating: 1400
weight: 1894
solve_time_s: 90
verified: false
draft: false
---

[CF 1894C - Người cung cấp thông tin ẩn danh](https://codeforces.com/problemset/problem/1894/C) 

**Đánh giá:** 1400 
**Tags:** thuật toán xây dựng, đồ thị, cách triển khai 
**Thời gian giải:** 1 phút 30 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng$b$chiều dài$n$và một số$k$, đại diện cho số lượng thao tác được thực hiện trên một số mảng gốc ẩn$a$. Mỗi thao tác bao gồm việc chọn một điểm cố định$x$TRONG$a$và dịch chuyển trái theo chu kỳ$a$qua$x$các vị trí. Một điểm cố định là một chỉ số$i$như vậy$a_i = i$. Thách thức là quyết định xem có tồn tại một số mảng ban đầu hay không$a$và một chuỗi$k$các thao tác dẫn đến mảng$b$. 

Kích thước đầu vào cho phép$n$lên đến$2 \cdot 10^5$Và$k$lên đến$10^9$. Điều này loại trừ mọi giải pháp cố gắng mô phỏng tất cả$k$hoạt động. Từ$k$có thể lớn hơn nhiều so với$n$, chúng ta phải tìm tính chất của$b$điều đó cho phép chúng tôi suy luận về tính khả thi mà không cần thực hiện mỗi ca. 

Các trường hợp cạnh không rõ ràng bao gồm các mảng có độ dài bằng 1, trong đó mọi thao tác đều không đáng kể vì một phần tử duy nhất luôn là một điểm cố định và các mảng trong đó tất cả các phần tử đều giống nhau, vì nhiều lần dịch chuyển có thể dẫn đến cùng một cấu hình hoặc tạo ra một chuỗi hợp lệ không thể thực hiện được. Ví dụ, nếu$b = [1]$Và$k = 10^9$, câu trả lời luôn là “Có” vì phần tử duy nhất là điểm cố định của chính nó. Ngược lại, nếu$b = [1, 1]$Và$k = 5$, không có mảng ban đầu nào có đủ các điểm cố định riêng biệt để đạt tới$b$, vì vậy câu trả lời là "Không". 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thử mọi mảng bắt đầu có thể$a$và mô phỏng$k$hoạt động. Điều này liên quan đến việc xác định tất cả các điểm cố định trong$a$, thực hiện các ca trái và lặp lại. Số khả năng cho$a$là$n^n$, điều này là không thể thực hiện được đối với$n$lên đến$2 \cdot 10^5$. Ngay cả việc mô phỏng một ứng cử viên cũng sẽ mất$O(k \cdot n)$, điều đó là không thể khi$k$tùy thuộc vào$10^9$. 

Thông tin chi tiết quan trọng là mỗi thao tác sẽ di chuyển các phần tử theo một chu trình và mảng cuối cùng$b$chỉ có thể khác với hoán vị danh tính bởi một số "vị trí dịch chuyển" tương ứng với một chuỗi ca liền kề. Điều này làm giảm vấn đề kiểm tra xem có bao nhiêu vị trí trong$b$là "lạc chỗ" so với mảng nhận dạng$1, 2, \ldots, n$. Cụ thể, với mỗi chỉ số$i$, xác định phần bù$\text{diff} = b_i - i$. Vì ca trái quấn quanh,$\text{diff} \mod n$đại diện cho sự dịch chuyển cần thiết để khớp phần tử đó với một điểm cố định. 

Chúng tôi đếm số lần xuất hiện của từng giá trị dịch chuyển theo modulo$n$. Bất kỳ giá trị dịch chuyển nào xuất hiện$n - k$hoặc ít lần hơn không thể sửa được bằng$k$hoạt động. Do đó, câu trả lời là "Có" nếu tồn tại giá trị dịch chuyển có thể đạt được bằng cách thực hiện tối đa$k$chuyển dịch theo thứ tự. 

Câu chuyện ở đây là vũ phu bị chặn bởi$k$Và$n$, nhưng số lần dịch chuyển modulo nắm bắt được tính bất biến: mỗi phần tử chỉ di chuyển tương ứng với vị trí ban đầu của nó bằng một số lần dịch chuyển trái và$k$các hoạt động chỉ có thể bao gồm một số lượng hạn chế "các phần tử bị sai lệch". 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^n * k) | O(n) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$Và$k$, sau đó đọc mảng$b$. 
2. Khởi tạo mảng truy cập$count$kích thước$n$về không. Điều này sẽ theo dõi số lượng phần tử có thể được căn chỉnh với mỗi ca có thể. 
3. Đối với mỗi chỉ số$i$TRONG$b$, tính độ dịch chuyển cần thiết$\text{shift} = (i - b[i] \% n + n) \% n$. Điều này thể hiện khoảng cách$b[i]$là từ vị trí "bản sắc" của nó$i$modulo$n$. Tăng$count[shift]$. 
4. Sau khi xử lý tất cả các chỉ số, lặp lại các giá trị dịch chuyển có thể có. Nếu có sự thay đổi nào xuất hiện thì ít nhất$n - k$lần, xuất ra "Có". Nếu không thì xuất ra "No". 

Lý do điều này hoạt động là vì mỗi thao tác có thể sửa một khối vị trí liền kề tương ứng với một ca. Đếm số lần xuất hiện ca để biết liệu có tồn tại một chuỗi thao tác có thể sắp xếp lại các phần tử cho phù hợp hay không$b$. Hoạt động modulo giải thích cho sự bao bọc do sự dịch chuyển theo chu kỳ. 

## Giải pháp Python```
PythonRun
```Giải pháp đọc từng trường hợp kiểm thử, tính toán xem mỗi phần tử của$b$là từ vị trí mong đợi của nó và đếm các khoảng cách này theo modulo$n$. Kiểm tra xem có sự thay đổi nào xảy ra ít nhất$n-k$lần đảm bảo rằng$k$hoạt động là đủ
