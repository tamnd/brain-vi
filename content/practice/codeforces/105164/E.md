---
title: "CF 105164E - Đánh giá biểu thức tuyến tính"
description: "Chúng ta được cung cấp một quy tắc tuyến tính đơn giản để biến đổi một số nguyên đầu vào $x$ thành một giá trị đầu ra $ax + b$. Nhiệm vụ là áp dụng quy tắc này nhiều lần cho các giá trị liên tiếp của $x$, bắt đầu từ 1 đến $k$, và in tất cả kết quả đầu ra theo thứ tự."
date: "2026-06-27T10:44:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105164
codeforces_index: "E"
codeforces_contest_name: "2024 ICPC Gran Premio de Mexico 1ra Fecha"
rating: 0
weight: 105164
solve_time_s: 65
verified: false
draft: false
---

[CF 105164E - Đánh giá biểu thức tuyến tính](https://codeforces.com/problemset/problem/105164/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một quy tắc tuyến tính đơn giản để biến đổi một số nguyên đầu vào$x$thành giá trị đầu ra$ax + b$. Nhiệm vụ là áp dụng quy tắc này nhiều lần cho các giá trị liên tiếp của$x$, bắt đầu từ 1 đến$k$và in tất cả kết quả đầu ra theo thứ tự. 

Nói một cách cụ thể hơn, hãy tưởng tượng một cái máy lấy một số, nhân nó với$a$, sau đó thêm$b$. Chúng tôi cung cấp cho máy này các đầu vào 1, 2, 3, cho đến tận$k$và chúng tôi ghi lại từng kết quả dưới dạng một chuỗi. Đầu ra cuối cùng chỉ đơn giản là danh sách các giá trị được chuyển đổi. 

Các ràng buộc cực kỳ nhỏ, với tất cả các tham số được giới hạn bởi 100. Điều này có nghĩa là mọi tính toán hợp lý cho mỗi thuật ngữ đều đã đủ nhanh và ngay cả mô phỏng đơn giản nhất cũng chạy trong thời gian không đáng kể. Không cần tối ưu hóa ngoài việc đánh giá trực tiếp. 

Không có trường hợp cạnh ẩn phức tạp nào trong chính cấu trúc đầu vào, nhưng một lỗi phổ biến là đọc sai phần bắt đầu của trình tự. Trình tự phải bắt đầu lúc$x = 1$, không$x = 0$. Nếu ai đó bắt đầu sai từ 0, số hạng đầu tiên sẽ trở thành$b$, điều này không hợp lệ trừ khi có ý định rõ ràng. 

Một sai lầm tiềm ẩn khác là cố gắng tỏ ra quá thông minh và rút ra một dãy dạng đóng mà không nhận ra rằng mỗi số hạng là độc lập. Vì mỗi số hạng chỉ phụ thuộc vào chính nó$x$, không có sự lặp lại hoặc tương tác giữa các điều khoản. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là tính toán từng số hạng một cách độc lập. Đối với mỗi$x$từ 1 đến$k$, chúng tôi đánh giá$ax + b$và in kết quả ngay lập tức. Điều này có hiệu quả vì biểu thức là tuyến tính và không phụ thuộc vào các tính toán trước đó. 

Cách giải thích thô bạo có thể đề xuất xây dựng danh sách các giá trị rồi in chúng, nhưng ngay cả điều đó cũng đã tối ưu trong cài đặt này. Tổng số thao tác là$k$và mỗi phép toán là một phép tính số học có thời gian không đổi. 

Quan sát quan trọng là vấn đề không có sự phụ thuộc trạng thái. Mỗi đầu ra được xác định đầy đủ bởi chỉ số của nó$x$, do đó việc lưu trữ các kết quả trung gian hoặc cố gắng cập nhật gia tăng sẽ không mang lại lợi ích gì. Bất kỳ tối ưu hóa nào ngoài vòng lặp đơn giản sẽ chỉ tăng thêm độ phức tạp mà không làm giảm thời gian chạy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (đánh giá trực tiếp) | O(k) | O(1) | Đã chấp nhận | 
| Tối ưu (cùng ý tưởng) | O(k) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán từng thuật ngữ một bằng cách sử dụng định nghĩa của biểu thức. 

1. Đọc số nguyên$a$,$b$, Và$k$. Những điều này xác định phép biến đổi tuyến tính và số lượng thuật ngữ chúng ta phải tạo ra. 
2. Lặp lại$x$từ 1 đến$k$. Mỗi giá trị của$x$tương ứng với một số hạng trong dãy. 
3. Đối với mỗi$x$, tính toán$ax + b$. Đây là đánh giá trực tiếp của biểu thức đại số đã cho. 
4. Lưu trữ hoặc in ngay giá trị tính toán. Vì thứ tự sản lượng rất quan trọng nên chúng tôi tiếp tục tăng$x$. 
5. Phân tách các giá trị bằng dấu cách trên một dòng. 

Việc tính toán bên trong vòng lặp là thời gian không đổi, do đó bản thân vòng lặp sẽ xác định đầy đủ thời gian chạy. 

### Tại sao nó hoạt động 

Mỗi thuật ngữ ở đầu ra được xác định độc lập bằng cách thay thế một giá trị cụ thể của$x$vào cùng một công thức. Không có sự phụ thuộc giữa các số hạng nên việc đánh giá chúng một cách tuần tự hoặc độc lập sẽ mang lại kết quả giống hệt nhau. Vòng lặp chỉ đơn giản liệt kê miền của$x$và áp dụng cùng một chức năng xác định cho từng phần tử. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a, b, k = map(int, input().split())

res = []
for x in range(1, k + 1):
    res.append(str(a * x + b))

print(" ".join(res))
```Giải pháp đọc ba số nguyên và sau đó xây dựng chuỗi bằng cách lặp lại tất cả các giá trị hợp lệ của$x$. Mỗi giá trị được tính toán sẽ được chuyển đổi thành chuỗi ngay lập tức để tránh chuyển đổi lặp lại trong quá trình nối. Đầu ra cuối cùng được in dưới dạng một dòng cách nhau bằng dấu cách. 

Một chi tiết triển khai tinh tế đang sử dụng`range(1, k + 1)`thay vì bắt đầu từ 0. Bắt đầu từ 0 sẽ tạo ra sai số$b$là thuật ngữ đầu tiên, không khớp với định nghĩa chuỗi được yêu cầu. 

Sử dụng danh sách và`" ".join(...)`đảm bảo rằng định dạng đầu ra nhất quán và tránh chi phí in lặp lại bên trong vòng lặp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 1 5
```Chúng tôi tính toán$x + 1$cho mỗi$x$. 

| x | Tính toán | Kết quả | 
| --- | --- | --- | 
| 1 | 1*1+1 | 2 | 
| 2 | 1*2 + 1 | 3 | 
| 3 | 1*3 + 1 | 4 | 
| 4 | 1*4 + 1 | 5 | 
| 5 | 1*5 + 1 | 6 | 

Đầu ra:```
2 3 4 5 6
```Điều này xác nhận ánh xạ trực tiếp từ chỉ mục đến giá trị và cho thấy mỗi thuật ngữ là độc lập. 

### Ví dụ 2 

đầu vào:```
1 10 7
```Chúng tôi tính toán$x + 10$cho mỗi$x$. 

| x | Tính toán | Kết quả | 
| --- | --- | --- | 
| 1 | 1*1 + 10 | 11 | 
| 2 | 1*2 + 10 | 12 | 
| 3 | 1*3 + 10 | 13 | 
| 4 | 1*4 + 10 | 14 | 
| 5 | 1*5 + 10 | 15 | 
| 6 | 1*6 + 10 | 16 | 
| 7 | 1*7 + 10 | 17 | 

Đầu ra:```
11 12 13 14 15 16 17
```Điều này chứng tỏ sự dịch chuyển thống nhất được áp dụng bởi$b$, ảnh hưởng như nhau đến tất cả các thuật ngữ mà không thay đổi khoảng cách của chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k) | Một lần lặp cho mỗi giá trị của$x$, mỗi lần thực hiện số học theo thời gian không đổi | 
| Không gian | O(1) | Chỉ có một danh sách nhỏ về kích thước$k$(hoặc đầu ra được phát trực tuyến nếu được tối ưu hóa hơn nữa) | 

Được cho$k \leq 100$, thời gian chạy không đáng kể ngay cả trong các ngôn ngữ được thông dịch. Giải pháp tốt trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    a, b, k = map(int, input().split())
    res = []
    for x in range(1, k + 1):
        res.append(str(a * x + b))
    return " ".join(res)

# provided samples
assert run("1 1 5") == "2 3 4 5 6", "sample 1"
assert run("1 10 7") == "11 12 13 14 15 16 17", "sample 2"
assert run("3 1 4") == "4 7 10 13", "sample 3"

# custom cases
assert run("2 0 1") == "2", "minimum k"
assert run("1 0 5") == "1 2 3 4 5", "simple identity progression"
assert run("5 5 3") == "10 15 20", "uniform step with offset"
assert run("100 100 2") == "200 300", "max coefficient small k"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 0 1 | 2 | Độ dài chuỗi tối thiểu | 
| 1 0 5 | 1 2 3 4 5 | Cấp số cộng thuần túy không có offset | 
| 5 5 3 | 10 15 20 | Kết hợp độ dốc và độ chính xác đánh chặn | 
| 100 100 2 | 200 300 | Xử lý hệ số tối đa | 

## Vỏ cạnh 

Trường hợp một cạnh là khi$k = 1$. Trong trường hợp này, vòng lặp chạy đúng một lần và tạo ra một giá trị duy nhất$a \cdot 1 + b$. Thuật toán xử lý việc này một cách tự nhiên vì phạm vi chỉ bao gồm điểm bắt đầu. 

Một trường hợp cạnh khác là khi$b = 0$. Dãy số trở thành một cấp số cộng thuần túy bắt đầu từ$a$. Thuật toán vẫn áp dụng cùng một công thức cho mỗi chỉ mục, tạo ra tỷ lệ chính xác mà không cần xử lý đặc biệt. 

Trường hợp cạnh cuối cùng là khi cả hai$a$Và$b$đang ở giá trị cực đại của chúng. Ngay cả khi đó, mỗi phép tính đều nằm trong giới hạn số nguyên an toàn trong Python và không phát sinh vấn đề tràn hoặc độ chính xác.
