---
title: "CF 1919D - 01 Cây"
description: "Chúng ta được cho khoảng cách từ gốc đến các lá của cây nhị phân, được liệt kê theo thứ tự lá DFS. Mỗi đỉnh trong có đúng hai con. Một cạnh đi ra có trọng số 0 và cạnh kia có trọng số 1."
date: "2026-06-08T19:34:16+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "dsu", "greedy", "sortings", "trees"]
categories: ["algorithms"]
codeforces_contest: 1919
codeforces_index: "D"
codeforces_contest_name: "Hello 2024"
rating: 2100
weight: 1919
solve_time_s: 46
verified: false
draft: false
---

[CF 1919D - 01 Cây](https://codeforces.com/problemset/problem/1919/D) 

**Xếp hạng:** 2100 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, dsu, tham lam, sắp xếp, cây 
**Thời gian giải:** 46s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho khoảng cách từ gốc đến các lá của cây nhị phân, được liệt kê theo thứ tự lá DFS. 

Mỗi đỉnh trong có đúng hai con. Một cạnh đi ra có trọng lượng`0`và cái kia có trọng lượng`1`. Hình dạng thực sự của cây vẫn chưa được biết, chỉ biết khoảng cách của lá. 

Nhiệm vụ là quyết định xem một số cây như vậy có thể tạo ra mảng đã cho hay không. 

Tổng số lá trên tất cả các trường hợp thử nghiệm nhiều nhất là`2 · 10^5`, do đó mọi nghiệm bậc hai trong`n`ngay lập tức bị loại trừ. Thậm chí`O(n√n)`sẽ không thoải mái. Chúng ta cần một cái gì đó gần tuyến tính hoặc`O(n log n)`. 

Phần khó khăn là thứ tự DFS áp đặt một cấu trúc mạnh mẽ. Các lá thuộc cùng một cây con chiếm một đoạn liền kề. Một nỗ lực xây dựng lại ngây thơ có thể dễ dàng đưa ra những lựa chọn có giá trị cục bộ mà sau này không thể thực hiện được. 

Một trường hợp cạnh quan trọng là giá trị tối thiểu. 

đầu vào:```
2
0 0
```Đầu ra:```
NO
```Hai chiếc lá không thể có khoảng cách`0`. Mỗi cặp anh chị em khác nhau đúng một điểm vì một cạnh con có trọng số`0`và cái kia có trọng lượng`1`. Bản thân gốc tương ứng với một khoảng cách duy nhất`0`, do đó, một mảng hợp lệ phải chứa chính xác một số 0. 

Một trường hợp tế nhị khác là:```
3
0 2 1
```Đầu ra:```
NO
```giá trị`2`cần một số cấu trúc lân cận mà cuối cùng có thể đóng vai trò là khoảng cách gốc của nó`1`. Mặc dù một`1`tồn tại trong mảng, thứ tự DFS sẽ ngăn cản việc sử dụng nó một cách chính xác. 

Ví dụ dễ bỏ sót thứ ba là:```
3
0 1 2
```Đầu ra:```
YES
```Các giá trị tăng đều đều, nhưng điều này vẫn có thể thực hiện được. Chỉ nhìn vào các yếu tố liền kề là không đủ. Chúng ta cần suy luận về các giá trị nhỏ hơn gần nhất còn lại sau khi hợp nhất đệ quy. 

## Phương pháp tiếp cận 

Một quan điểm bạo lực là đảo ngược việc xây dựng cây. 

Nếu hai lá là anh em thì khoảng cách của chúng cách nhau đúng một. Khi chúng ta hợp nhất chúng vào cha mẹ, khoảng cách của cha mẹ sẽ trở thành giá trị nhỏ hơn trong hai giá trị. Chúng ta có thể liên tục tìm kiếm một cặp liền kề hợp lệ, hợp nhất nó và tiếp tục cho đến khi còn lại một giá trị. 

Điều này phản ánh chính xác cấu trúc cây, vì vậy nó đúng. Vấn đề là có thể có nhiều sự lựa chọn. Khám phá tất cả các đơn đặt hàng hợp nhất là theo cấp số nhân. Ngay cả việc mô phỏng một cách tham lam việc hợp nhất với các bản cập nhật đắt tiền cũng nhanh chóng trở thành phương trình bậc hai. 

Quan sát quan trọng là chúng ta thực sự không cần thực hiện việc hợp nhất. 

Hãy xem xét một giá trị`x > 0`. Ở đâu đó trong quá trình ngược lại, nó phải được ghép nối với một nút có giá trị`x - 1`. Theo thứ tự DFS hiện tại, đối tác phải xuất hiện ở một bên của nó. Nếu chúng ta tìm giá trị nhỏ hơn gần nhất ở bên trái và bên phải thì ít nhất một trong số chúng phải bằng`x - 1`. Nếu không thì không có cách nào để tạo cha mẹ theo yêu cầu của lá này. 

Điều này biến vấn đề cây thành vấn đề mảng. 

Đối với mọi vị trí`i`:

- Nếu như`a[i] = 0`, nó là lá có khoảng cách gốc duy nhất. 
- Nếu như`a[i] > 0`, thì trong số các giá trị nhỏ hơn gần nhất ở bên trái và bên phải, ít nhất một giá trị phải chính xác`a[i] - 1`. 

Các phần tử nhỏ hơn gần nhất có thể được tính toán theo thời gian tuyến tính với các ngăn xếp đơn điệu. 

Kết quả là một`O(n)`kiểm tra từng trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng hợp nhất vũ phu | Hàm mũ hoặc O(n²) tùy thuộc vào việc triển khai | O(n) | Quá chậm | 
| Xác thực ngăn xếp đơn điệu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem có bao nhiêu số 0 xuất hiện trong mảng. 

Một cây hợp lệ phải chứa chính xác một giá trị`0`. Nếu số đếm không phải là một, hãy trả lời`NO`. 
2. Tính số st gần nhất
