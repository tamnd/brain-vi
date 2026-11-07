---
title: "CF 136A - Quà tặng"
description: "Chúng ta có một kịch bản về bữa tiệc trong đó Petya mời n người bạn, mỗi người trong số họ tặng đúng một món quà cho một người bạn khác. Danh sách đầu vào theo thứ tự của từng người bạn, người bạn mà họ đã tặng quà."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 136
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 97 (Div. 2)"
rating: 800
weight: 136
solve_time_s: 73
verified: true
draft: false
---

[CF 136A - Quà](https://codeforces.com/problemset/problem/136/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa ra một kịch bản về bữa tiệc trong đó Petya mời _n_ người bạn, mỗi người trong số họ tặng chính xác một món quà cho một người bạn khác. Danh sách đầu vào theo thứ tự của từng người bạn, người bạn mà họ đã tặng quà. Nhiệm vụ của chúng ta là đảo ngược ánh xạ này: đối với mỗi người bạn, chúng ta muốn biết ai đã tặng quà cho họ. 

Về hình thức, đầu vào bao gồm một số nguyên _n_ và một mảng`p`có độ dài _n_, ở đâu`p[i]`là số người bạn nhận được quà từ bạn`i + 1`. Đầu ra là một mảng`q`có độ dài _n_ ở đâu`q[i]`là người bạn đã tặng quà cho bạn`i + 1`. 

Ràng buộc`1 ≤ n ≤ 100`nhỏ, điều đó có nghĩa là ngay cả giải pháp O(n²) cũng đủ nhanh. Tuy nhiên, tính đúng đắn còn tinh tế hơn tính hiệu quả. Mỗi người bạn tặng và nhận đúng một món quà, vậy mảng`p`đại diện cho hoán vị của các số từ 1 đến _n_. Điều này đảm bảo rằng ánh xạ nghịch đảo tồn tại. 

Một trường hợp thường gặp là khi một người bạn tự tặng quà cho mình. Ví dụ, nếu`n = 3`Và`p = [1, 2, 3]`, sau đó mỗi người bạn sẽ nhận được món quà của riêng mình. Kết quả đúng cũng phải phản ánh rằng mỗi người bạn đã tự tặng một món quà cho mình:`q = [1, 2, 3]`. Một cách tiếp cận ngây thơ cho rằng không tự tặng có thể tạo ra ánh xạ không chính xác hoặc không xử lý được các chỉ số đúng cách. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực lặp đi lặp lại trên tất cả bạn bè của mỗi người nhận. Dành cho bạn bè`i`, chúng tôi tìm kiếm thông qua`p`để tìm chỉ số`j`Ở đâu`p[j] = i`. Điều này hoạt động vì mỗi giá trị trong`p`là duy nhất, đảm bảo sự phù hợp. Độ phức tạp về thời gian là O(n2), có thể chấp nhận được đối với`n ≤ 100`. Phương pháp này về mặt khái niệm đơn giản nhưng không hiệu quả đối với các mảng lớn hơn. 

Thông tin chi tiết quan trọng để tối ưu hóa đến từ việc nhận ra rằng`p`là một hoán vị. Hoán vị có thể được đảo ngược trực tiếp: nếu bạn`i`tặng quà cho bạn bè`p[i]`, rồi bạn ơi`p[i]`đã nhận được một món quà từ một người bạn`i`. Chúng ta có thể tạo một mảng`q`có kích thước _n_ và gán`q[p[i] - 1] = i + 1`cho tất cả`i`. Cách tiếp cận này là O(n) và chỉ yêu cầu một lần duyệt qua mảng, loại bỏ các vòng lặp lồng nhau không cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Được chấp nhận cho vấn đề này, không hiệu quả với n | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng bạn bè`n`và mảng`p`đại diện cho người mà mỗi người bạn tặng quà. 
2. Khởi tạo một mảng`q`chiều dài`n`để lưu trữ ánh xạ nghịch đảo. 
3. Lặp lại tất cả bạn bè`i`từ 0 đến n - 1. 
4. Cho mỗi người bạn`i`, giao phó`q[p[i] - 1] = i + 1`. Bước này đặt người tặng`i + 1`tại chỉ số tương ứng với người nhận`p[i] - 1`. 
5. Sau khi đổ đầy`q`, in nó dưới dạng số nguyên được phân tách bằng dấu cách. 

Thuật toán này hoạt động vì mỗi người bạn tặng đúng một món quà và nhận đúng một món quà. Nhiệm vụ`q[p[i] - 1] = i + 1`đảm bảo rằng mọi vị trí trong`q`được điền một lần và ánh xạ chính xác người nhận tới người tặng của họ. Tính duy nhất của hoán vị đảm bảo không xảy ra việc ghi đè. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
p = list(map(int, input().split()))

q = [0] * n
for i in range(n):
    q[p[i] - 1] = i + 1

print(*q)
```Chúng tôi đọc số lượng bạn bè và mảng hoán vị bằng cách sử dụng I/O nhanh. Mảng`q`được khởi tạo bằng số 0 để lưu trữ ánh xạ nghịch đảo. Vòng lặp chỉ định người gửi chính xác cho mỗi người nhận bằng cách sử dụng chỉ mục dựa trên 1. Cuối cùng, chúng ta in kết quả khi giải nén để phù hợp với định dạng đầu ra được yêu cầu. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
4
2 3 4 1
```Chúng tôi khởi tạo`q = [0, 0, 0, 0]`. 

| tôi | p[i] | q sau khi phân công | 
| --- | --- | --- | 
| 0 | 2 | [0, 1, 0, 0] | 
| 1 | 3 | [0, 1, 2, 0] | 
| 2 | 4 | [0, 1, 2, 3] | 
| 3 | 1 | [4, 1, 2, 3] | 

Đầu ra là`4 1 2 3`. Mỗi giá trị tại chỉ mục`i`chỉ ra chính xác ai đã tặng quà cho bạn bè`i + 1`. 

Một đầu vào khác:```
3
1 3 2
```| tôi | p[i] | q sau khi phân công | 
| --- | --- | --- | 
| 0 | 1 | [1, 0, 0] | 
| 1 | 3 | [1, 0, 2] | 
| 2 | 2 | [1, 3, 2] | 

Đầu ra:`1 3 2`, ánh xạ chính xác người tặng tới người nhận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Truyền đơn qua mảng có độ dài n để xây dựng hoán vị nghịch đảo | 
| Không gian | O(n) | Mảng`q`có độ dài n để lưu trữ kết quả | 

Với n 100, thuật toán này cực kỳ nhanh và tiết kiệm bộ nhớ. Giải pháp O(n) dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    p = list(map(int, input().split()))
    q = [0] * n
    for i in range(n):
        q[p[i] - 1] = i + 1
    return ' '.join(map(str, q))

# Provided samples
assert run("4\n2 3 4 1\n") == "4 1 2 3", "sample 1"
assert run("3\n1 3 2\n") == "1 3 2", "sample 2"

# Custom cases
assert run("1\n1\n") == "1", "single friend self-gift"
assert run("5\n1 2 3 4 5\n") == "1 2 3 4 5", "all self-gifts"
assert run("5\n5 4 3 2 1\n") == "5 4 3 2 1", "reversed permutation"
assert run("6\n2 1 4 3 6 5\n") == "2 1 4 3 6 5", "pair swaps"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1 | 1 | Người bạn độc thân tự tặng quà | 
| 5\n1 2 3 4 5 | 1 2 3 4 5 | Tất cả bạn bè đều tặng quà cho mình | 
| 5\n5 4 3 2 1 | 5 4 3 2 1 | Hoán vị đảo ngược hoàn toàn | 
| 6\n2 1 4 3 6 5 | 2 1 4 3 6 5 | Nhiều giao dịch hoán đổi, đảm bảo ánh xạ chính xác | 

## Vỏ cạnh 

Đối với một người bạn, hãy nhập`1\n1`, thuật toán khởi tạo`q = [0]`và phân công`q[0] = 1`, tạo ra sản lượng`1`. Thuật toán xử lý chính xác ranh giới tối thiểu. 

Để tự tặng, hãy nhập`5\n1 2 3 4 5`, mỗi`q[i]`được giao`i + 1`, tạo ra mảng giống như đầu ra. Không có lỗi riêng lẻ nào xảy ra vì chúng tôi điều chỉnh cẩn thận việc lập chỉ mục dựa trên 0 trong Python so với số bạn bè dựa trên 1. 

Đối với các hoán vị đảo ngược hoặc hoán đổi, phép gán trực tiếp`q[p[i] - 1] = i + 1`ánh xạ người tặng tới người nhận một cách đáng tin cậy mà không cần tìm kiếm, xác nhận tính chính xác của tất cả các hoán vị.
