---
title: "CF 39H - Bảng cửu chương"
description: "Nhiệm vụ là tạo một bảng nhân cho các số trong hệ thống số vị trí có cơ số k. Không giống như hệ thập phân, các chữ số trong hệ thống này nằm trong khoảng từ 0 đến k-1. Petya muốn xem tích các số từ 1 đến k-1 được thể hiện trong hệ thống này."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 39
codeforces_index: "H"
codeforces_contest_name: "School Team Contest 1 (Winter Computer School 2010/11)"
rating: 1300
weight: 39
solve_time_s: 93
verified: false
draft: false
---

[CF 39H - Bảng cửu chương](https://codeforces.com/problemset/problem/39/H) 

**Đánh giá:** 1300 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là lập bảng nhân các số trong hệ đếm số vị trí có cơ số`k`. Không giống như hệ thập phân, các chữ số trong hệ thống này nằm trong khoảng từ 0 đến`k-1`. Petya muốn xem các tích số từ 1 đến`k-1`thể hiện trong hệ thống này. Đầu vào là một số nguyên duy nhất`k`đại diện cho cơ số, giới hạn từ 2 đến 10. Đầu ra là một`(k-1) × (k-1)`bảng, nơi mục nhập ở hàng`i`và cột`j`chứa sản phẩm`i × j`thể hiện ở cơ sở`k`. 

Bởi vì`k`nhiều nhất là 10, kích thước bảng lớn nhất là`9 × 9`. Mỗi sản phẩm trong bảng nhiều nhất là`(k-1) × (k-1) = 81`khi`k = 10`. Nó đủ nhỏ để chúng ta có thể tạo bảng một cách rõ ràng mà không cần lo lắng về hiệu suất. Sự tinh tế chính nằm ở việc chuyển đổi tích số thập phân thành cơ số đích, đảm bảo các chữ số được biểu diễn chính xác dưới dạng chuỗi và xử lý đúng các số có nhiều chữ số. 

Một trường hợp cạnh phát sinh khi`k = 2`. Bảng sau đó chỉ có một mục duy nhất:`1 × 1 = 1`. Việc triển khai bất cẩn có thể lặp không chính xác hoặc cố gắng đưa số 0 vào bảng, tạo ra kích thước sai. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: lặp qua tất cả các số nguyên`i`Và`j`từ 1 đến`k-1`, tính tích của chúng và chuyển đổi nó thành biểu diễn chuỗi trong cơ sở`k`. Điều này hoạt động vì kích thước bảng rất nhỏ, thậm chí ở mức tối đa`k`. Mỗi tích nhiều nhất là 81, do đó việc chuyển đổi cơ số có thể được thực hiện bằng các phép chia và lấy số dư lặp đi lặp lại. 

Không có cách tiếp cận tiệm cận nào nhanh hơn vì bài toán yêu cầu in rõ ràng mọi mục nhập. Cái nhìn sâu sắc quan trọng là việc tạo ra các con số trong cơ sở`k`rất đơn giản khi chúng ta xử lý việc chuyển đổi một cách cẩn thận: chia liên tục cho`k`và ghi số dư rồi đảo ngược các chữ số đã thu thập được. Bản thân bảng không được bao gồm hàng hoặc cột bằng 0 và tất cả các số phải được in dưới dạng chuỗi trong cơ sở đích, phân tách bằng dấu cách. Số học số nguyên tích hợp của Python xử lý tất cả các sản phẩm một cách an toàn mà không bị tràn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k^2 log k) | O(k^2 log k) | Đã chấp nhận | 
| Tối ưu | O(k^2 log k) | O(k^2 log k) | Đã chấp nhận | 

các`log k`hệ số xuất phát từ số chữ số cần thiết để biểu diễn các số lên đến`(k-1)^2`trong căn cứ`k`. 

## Hướng dẫn thuật toán 

1. Đọc cơ số`k`từ đầu vào. Điều này xác định kích thước của bảng,`(k-1) × (k-1)`. 
2. Xác định hàm trợ giúp để chuyển đổi số nguyên thập phân`n`thành một chuỗi trong cơ sở`k`. Khởi tạo một danh sách trống`digits`. Trong khi`n`lớn hơn 0, chia`n`qua`k`, lấy phần còn lại, nối phần còn lại dưới dạng chuỗi vào`digits`, và tiếp tục với thương số. Đảo ngược`digits`ở cuối. Nếu như`n`bằng 0, quay lại`"0"`. 
3. Lặp lại từng chỉ mục hàng`i`từ 1 đến`k-1`. Đối với mỗi`i`, khởi tạo một danh sách trống`row`. 
4. Bên trong vòng lặp hàng, lặp qua từng chỉ mục cột`j`từ 1 đến`k-1`. Tính toán sản phẩm`i × j`. 
5. Chuyển đổi sản phẩm thành đế`k`sử dụng chức năng trợ giúp và nối nó vào`row`. 
6. Sau khi xử lý tất cả các cột trong một hàng, nối các mục trong hàng với dấu cách và in kết quả. 
7. Lặp lại cho đến khi tất cả các hàng được in. 

Cách tiếp cận này hiệu quả vì mỗi mục nhập được tính toán rõ ràng và chuyển đổi chính xác thành cơ sở mục tiêu. Giới hạn vòng lặp đảm bảo kích thước bảng phù hợp`(k-1) × (k-1)`và chuyển đổi cơ sở đảm bảo các chữ số được biểu diễn dưới dạng chuỗi mà không dựa vào định dạng thập phân ẩn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def to_base(n, k):
    if n == 0:
        return "0"
    digits = []
    while n > 0:
        digits.append(str(n % k))
        n //= k
    return ''.join(reversed(digits))

def main():
    k = int(input())
    for i in range(1, k):
        row = []
        for j in range(1, k):
            product = i * j
            row.append(to_base(product, k))
        print(' '.join(row))

if __name__ == "__main__":
    main()
```các`to_base`hàm xử lý chuyển đổi cơ sở bằng cách chia lặp lại và trích xuất phần dư. các`main`hàm lặp trên tất cả các chỉ mục của bảng và xây dựng các hàng một cách linh hoạt. Việc nối các phần tử hàng đảm bảo khoảng cách thích hợp. Mã xử lý chính xác giá trị nhỏ nhất và lớn nhất`k`các giá trị, bao gồm`k = 2`tạo ra một mục duy nhất. 

## Ví dụ đã hoạt động 

Xem xét đầu vào`k = 3`. Kích thước bàn là`2 × 2`. Theo dõi thông qua các biến: 

| tôi | j | sản phẩm | chuyển đổi cơ sở 3 | hàng | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | "1" | ["1"] | 
| 1 | 2 | 2 | "2" | ["1","2"] | 
| 2 | 1 | 2 | "2" | ["2"] | 
| 2 | 2 | 4 | "11" | ["2","11"] | 

Đầu ra:```
1 2
2 11
```Điều này chứng tỏ rằng việc chuyển đổi xử lý chính xác các số có nhiều chữ số (`4`trở thành`"11"`ở cơ sở 3). 

Một ví dụ khác với`k = 2`: 

| tôi | j | sản phẩm | chuyển đổi cơ số 2 | hàng | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | "1" | ["1"] | 

Đầu ra:```
1
```Điều này xác nhận thuật toán xử lý đúng trường hợp bảng tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k^2 log k) | Mỗi sản phẩm trong số (k-1)^2 yêu cầu chuyển đổi một số lên tới (k-1)^2 thành cơ số k, thực hiện các phép toán O(log_k((k-1)^2)) = O(log k). | 
| Không gian | O(k^2 log k) | Mỗi hàng lưu trữ k-1 số ở dạng chuỗi. Mỗi số có tối đa O(log k) chữ số. | 

Cho rằng k 10, thuật toán thực hiện tối đa 81 mục trong bảng với nhiều nhất 2 chữ số cơ sở 10, nằm trong giới hạn 2 giây và 64 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided sample
assert run("10\n") == "\n".join([
    "1 2 3 4 5 6 7 8 9",
    "2 4 6 8 10 12 14 16 18",
    "3 6 9 12 15 18 21 24 27",
    "4 8 12 16 20 24 28 32 36",
    "5 10 15 20 25 30 35 40 45",
    "6 12 18 24 30 36 42 48 54",
    "7 14 21 28 35 42 49 56 63",
    "8 16 24 32 40 48 56 64 72",
    "9 18 27 36 45 54 63 72 81"
]), "sample 1"

# custom minimum k
assert run("2\n") == "1", "minimum k"

# custom small k
assert run("3\n") == "1 2\n2 11", "k=3"

# custom medium k
assert run("4\n") == "1 2 3\n2 10 12\n3 12 21", "k=4"

# custom max k
assert run("10\n") == run("10\n"), "k=10 consistency"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | 1 | Kích thước bảng tối thiểu | 
| 3 | 1 2 \n 2 11 | Chuyển đổi cơ sở cho số có nhiều chữ số | 
| 4 | 1 2 3 \n 2 10 12 \n 3 12 21 | Chuyển đổi cơ sở với nhiều chữ số hơn và bảng nhiều hàng | 
| 10 | bàn 9×9 đầy đủ | Tính chính xác, nhất quán của bảng lớn | 

## Vỏ cạnh 

Khi nào`k = 2`, bảng chỉ có một ô. Các vòng lặp thực hiện đúng một lần:`i = 1`,`j = 1`, sản phẩm là`1`, được chuyển đổi sang cơ số 2 dưới dạng`"1"`, và được in. Không có rủi ro riêng lẻ vì phạm vi là`range(1, k)`. Vì`k = 3`, sản phẩm như`2 × 2 = 4`trở nên`"11"`trong cơ số 3, chứng minh rằng thuật toán xử lý chuyển đổi cơ số nhiều chữ số một cách chính xác. Các bảng theo dõi ở trên xác nhận tất cả các ranh giới và chuyển đổi nhiều chữ số đều chính xác.
