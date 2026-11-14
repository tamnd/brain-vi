---
title: "CF 1777A - Mọi người đều thích mảng tốt!"
description: "Chúng ta được yêu cầu chuyển đổi một mảng các số nguyên thành một “mảng tốt”, được định nghĩa là một mảng trong đó không có hai phần tử liên tiếp nào có cùng tính chẵn lẻ. Tính chẵn lẻ đơn giản có nghĩa là một số là số lẻ hay số chẵn."
date: "2026-06-09T11:38:28+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1777
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 845 (Div. 2) and ByteRace 2023"
rating: 800
weight: 1777
solve_time_s: 93
verified: true
draft: false
---

[CF 1777A - Mọi người đều thích mảng tốt!](https://codeforces.com/problemset/problem/1777/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu chuyển đổi một mảng các số nguyên thành một “mảng tốt”, được định nghĩa là một mảng trong đó không có hai phần tử liên tiếp nào có cùng tính chẵn lẻ. Tính chẵn lẻ đơn giản có nghĩa là một số là số lẻ hay số chẵn. Vì vậy, trong một mảng tốt, số lẻ phải theo sau là số chẵn và số chẵn phải theo sau là số lẻ. Mảng có kích thước một là khá tốt vì không có phần tử liên tiếp. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp một mảng có độ dài lên tới 100, với các phần tử lớn bằng$10^9$. Đầu ra của mỗi trường hợp thử nghiệm là số lượng thao tác tối thiểu cần thiết để biến mảng thành một mảng tốt. Thao tác được phép là chọn hai số liền kề có cùng số chẵn lẻ, loại bỏ chúng và thay thế bằng tích của chúng. 

Các ràng buộc gợi ý rằng một cách đơn giản$O(n^2)$cách tiếp cận này là khả thi, bởi vì$n \le 100$Và$t \le 500$, vì vậy trong trường hợp xấu nhất chúng ta có thể thực hiện đại khái$500 \cdot 100^2 = 5 \cdot 10^6$hoạt động trong một mô phỏng đơn giản. Tuy nhiên, chúng ta có thể giải quyết vấn đề nhanh hơn bằng cách suy luận về mẫu chẵn lẻ thay vì mô phỏng phép nhân một cách rõ ràng. 

Trường hợp cạnh khóa là khi nhiều số chẵn lẻ giống nhau xuất hiện liên tiếp. Ví dụ, mảng`[2, 4, 6, 1]`yêu cầu hai thao tác: kết hợp`2*4 = 8`và sau đó`8*6 = 48`để đạt được`[48, 1]`. Một phép kiểm tra ngây thơ chỉ xem xét cặp xung đột đầu tiên có thể tính thiếu các phép toán. Một trường hợp cạnh khác là một mảng đã xen kẽ tính chẵn lẻ, chẳng hạn như`[1, 2, 3, 4]`, không yêu cầu thao tác nào. 

## Phương pháp tiếp cận 

Phương pháp brute-force là mô phỏng hoạt động: quét mảng, tìm các phần tử liền kề có cùng chẵn lẻ, thực hiện phép nhân và tiếp tục cho đến khi không còn cặp nào như vậy tồn tại. Cách tiếp cận này đúng vì mỗi thao tác làm giảm nghiêm ngặt số lượng các cặp liền kề có cùng chẵn lẻ, do đó cuối cùng nó sẽ chấm dứt. Nhược điểm của nó là thực hiện thao tác mảng lặp đi lặp lại và có thể không hiệu quả đối với các lần chạy chẵn lẻ liên tiếp. Đối với mảng có độ dài 100, điều này có thể chấp nhận được nhưng cồng kềnh. 

Cái nhìn sâu sắc quan trọng cho một cách tiếp cận tối ưu là chúng ta thực sự không cần phải theo dõi các con số. Nhân hai số không làm thay đổi tính chẵn lẻ của chúng, vì vậy thông tin liên quan duy nhất là chuỗi chẵn lẻ. Điều này làm giảm vấn đề đếm độ dài của các chuỗi số liên tiếp có cùng số chẵn lẻ. Mỗi lần chạy dài$k$đóng góp chính xác$k-1$hoạt động để hợp nhất nó thành một phần tử duy nhất. Tổng hợp$k-1$trên tất cả các lần chạy chẵn lẻ giống nhau trong mảng sẽ cho số lượng hoạt động tối thiểu. Quan sát này biến vấn đề thành một lần quét tuyến tính đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Có thể chấp nhận được nhưng không cần thiết | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Đối với mỗi trường hợp kiểm thử, hãy đọc độ dài của mảng và chính mảng đó. 
3. Khởi tạo bộ đếm cho các phép toán về 0. 
4. Bắt đầu quét mảng từ phần tử thứ hai đến cuối. 
5. Theo dõi độ dài của chuỗi số liên tiếp hiện tại có cùng số chẵn lẻ. Bắt đầu với độ dài chạy 1. 
6. Đối với mỗi phần tử, so sánh tính chẵn lẻ của nó với phần tử trước đó. Nếu nó giống nhau, hãy tăng thời lượng chạy. Nếu khác thì thêm`run_length - 1`đến bộ đếm hoạt động và đặt lại độ dài chạy thành 1. 
7. Sau vòng lặp, thêm`run_length - 1`cho lần chạy cuối cùng. 
8. Xuất ra tổng số thao tác cho test case. 

Tại sao nó hoạt động: Điều bất biến là bất kỳ chuỗi số liên tiếp nào có cùng số chẵn lẻ đều có thể được rút gọn thành một số một cách chính xác.`length-1`hoạt động, không phụ thuộc vào thứ tự. Điều này đảm bảo rằng sau khi xử lý tất cả các lần chạy, mảng sẽ hoạt động tốt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    operations = 0
    run_length = 1
    for i in range(1, n):
        if a[i] % 2 == a[i-1] % 2:
            run_length += 1
        else:
            operations += run_length - 1
            run_length = 1
    operations += run_length - 1
    print(operations)
```Mã đọc tất cả đầu vào một cách hiệu quả bằng cách sử dụng I/O nhanh. Nó khởi tạo bộ đếm độ dài chạy thành 1 vì một phần tử đơn lẻ bắt đầu chạy một cách tầm thường. Mỗi lần tính chẵn lẻ thay đổi, lần chạy trước đó sẽ được tính và bộ đếm được đặt lại. Cuối cùng, chúng tôi thêm các thao tác cho lần chạy cuối cùng. 

## Ví dụ đã hoạt động 

Đầu vào mẫu`[1, 7, 11, 2, 13]`: 

| tôi | một [tôi] | chẵn lẻ | run_length | hoạt động | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | lẻ | 1 | 0 | 
| 1 | 7 | lẻ | 2 | 0 | 
| 2 | 11 | lẻ | 3 | 0 | 
| 3 | 2 | thậm chí | đặt lại | 3-1=2 | 
| 4 | 13 | lẻ | 1 | 2 | 

Tổng số thao tác = 2, khớp với kết quả dự kiến. 

Đầu vào mẫu`[1, 2, 3, 4]`: 

| tôi | một [tôi] | chẵn lẻ | run_length | hoạt động | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | lẻ | 1 | 0 | 
| 1 | 2 | thậm chí | đặt lại | 1-1=0 | 
| 2 | 3 | lẻ | đặt lại | 1-1=0 | 
| 3 | 4 | thậm chí | đặt lại | 1-1=0 | 

Tổng số thao tác = 0, đã tốt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Quét tuyến tính qua mảng đủ | 
| Không gian | O(1) thêm | Chỉ có bộ đếm được lưu trữ | 

Giải pháp phù hợp thoải mái với các ràng buộc, vì$n \le 100$Và$t \le 500$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        operations = 0
        run_length = 1
        for i in range(1, n):
            if a[i] % 2 == a[i-1] % 2:
                run_length += 1
            else:
                operations += run_length - 1
                run_length = 1
        operations += run_length - 1
        output.append(str(operations))
    return "\n".join(output)

# provided samples
assert run("3\n5\n1 7 11 2 13\n4\n1 2 3 4\n6\n1 1 1 2 2 3\n") == "2\n0\n3", "samples"

# custom cases
assert run("1\n1\n42\n") == "0", "single element"
assert run("1\n5\n2 2 2 2 2\n") == "4", "all even"
assert run("1\n6\n1 3 5 7 9 11\n") == "5", "all odd"
assert run("1\n4\n1 2 4 3\n") == "1", "mixed small run"
assert run("1\n3\n2 2 1\n") == "1", "short run at start"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n42`| 0 | Mảng một phần tử khá tốt | 
|`1\n5\n2 2 2 2 2`| 4 | Chuỗi số chẵn dài | 
|`1\n6\n1 3 5 7 9 11`| 5 | Chuỗi số lẻ dài | 
|`1\n4\n1 2 4 3`| 1 | Chạy nhỏ ở giữa | 
|`1\n3\n2 2 1`| 1 | Chạy ngắn lúc đầu | 

## Vỏ cạnh 

Mảng có độ dài 1 được xử lý chính xác: độ dài chạy không bao giờ tăng, vì vậy`run_length - 1 = 0`. Mảng có tính chẵn lẻ xen kẽ không tăng`run_length`vượt quá 1, mang lại hoạt động bằng không. Các lần chạy có tính chẵn lẻ giống hệt nhau ở đầu, giữa hoặc cuối đều được ghi lại chính xác vì thuật toán thêm lần chạy cuối cùng sau khi kết thúc vòng lặp. Ví dụ,`[2,2,1]`kết quả là`run_length = 2`tại chỉ mục 1, sau đó tính chẵn lẻ thay đổi ở chỉ mục 2, thêm`2-1=1`hoạt động, w_
