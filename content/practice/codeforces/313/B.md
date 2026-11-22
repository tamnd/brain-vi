---
title: "CF 313B - Ilya và Truy vấn"
description: "Chúng ta được cấp một chuỗi chỉ gồm các ký tự \".\" và \"\". Nhiệm vụ là trả lời nhiều truy vấn một cách hiệu quả. Mỗi truy vấn hỏi, trong một chuỗi con từ vị trí l đến r, có bao nhiêu lần một ký tự được theo sau bởi một ký tự giống hệt nhau."
date: "2026-06-06T00:53:03+07:00"
tags: ["codeforces", "competitive-programming", "dp", "implementation"]
categories: ["algorithms"]
codeforces_contest: 313
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 186 (Div. 2)"
rating: 1100
weight: 313
solve_time_s: 78
verified: true
draft: false
---

[CF 313B - Ilya và Truy vấn](https://codeforces.com/problemset/problem/313/B) 

**Đánh giá:** 1100 
**Tags:** dp, triển khai 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi chỉ gồm các ký tự "." Và "#". Nhiệm vụ là trả lời nhiều truy vấn một cách hiệu quả. Mỗi truy vấn hỏi, trong một chuỗi con từ vị trí _l_ đến _r_, có bao nhiêu lần một ký tự được theo sau bởi một ký tự giống hệt nhau. Ví dụ: trong chuỗi con “..#..”, các cặp ký tự giống nhau liên tiếp nằm ở vị trí 1-2 và 4-5. Truy vấn muốn đếm các vị trí như vậy. 

Chuỗi đầu vào có thể dài tới 100.000 ký tự và số lượng truy vấn cũng có thể lên tới 100.000. Điều này có nghĩa là một cách tiếp cận đơn giản quét từng chuỗi con cho mỗi truy vấn sẽ yêu cầu tới 10^10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn có thể chấp nhận được trong khung thời gian 2 giây. Do đó, giải pháp phải tránh việc quét lặp lại các chuỗi con chồng chéo. 

Các trường hợp cạnh không rõ ràng bao gồm các chuỗi có tất cả các ký tự giống hệt nhau, các ký tự xen kẽ hoặc các truy vấn trải rộng trên toàn bộ chuỗi. Ví dụ: nếu chuỗi là "######" và truy vấn là từ 1 đến 6 thì câu trả lời đúng là 5, vì mỗi ký tự trong số năm ký tự đầu tiên tạo thành một cặp với ký tự lân cận của nó. Cách tiếp cận bất cẩn chỉ đếm các ký tự duy nhất hoặc quên cặp cuối cùng sẽ tạo ra kết quả sai. Tương tự, đối với một chuỗi như ".#.#.#", mọi truy vấn dài hơn một ký tự có thể trả về 0 nếu không có ký tự liên tiếp nào khớp và cách tiếp cận đơn giản phải xử lý ranh giới một cách chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: đối với mỗi truy vấn, lặp lại từ chỉ mục bắt đầu _l_ đến _r_−1, kiểm tra xem ký tự hiện tại có bằng ký tự tiếp theo hay không và tăng bộ đếm. Điều này hoạt động chính xác nhưng độ phức tạp của nó là O(m·n) trong trường hợp xấu nhất. Với các ràng buộc n, m ≤ 10^5, điều này có thể dẫn đến khoảng 10^10 thao tác, điều này là không thể thực hiện được. 

Quan sát quan trọng là thông tin chúng ta cần - số lượng ký tự bằng nhau liên tiếp - có thể được tính toán trước cho toàn bộ chuỗi theo thời gian tuyến tính. Xác định một mảng`prefix`như vậy`prefix[i]`là số cặp ký tự giống nhau liên tiếp từ đầu chuỗi đến chỉ mục`i`. Sau đó, đối với truy vấn (l, r), câu trả lời đơn giản là`prefix[r-1] - prefix[l-1]`. Điều này hoạt động vì`prefix[r-1]`đếm tất cả các cặp đến vị trí r-1, và`prefix[l-1]`đếm tất cả các cặp trước vị trí l, do đó sự khác biệt của chúng cho biết số cặp trong phạm vi truy vấn. Điều này làm giảm việc xử lý truy vấn xuống còn O(1) cho mỗi truy vấn, dẫn đến độ phức tạp tổng cộng là O(n + m), có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·m) | O(1) | Quá chậm | 
| Tiền tố Tổng | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng tổng tiền tố có độ dài n+1, với tất cả các giá trị được đặt thành 0. Mảng này sẽ lưu trữ số lượng tích lũy của các ký tự giống hệt nhau liên tiếp. 
2. Lặp lại chuỗi từ chỉ số 1 đến n−1. Tại mỗi vị trí i, kiểm tra xem ký tự hiện tại có`s[i]`bằng với ký tự trước đó`s[i-1]`. Nếu vậy, hãy tăng số lượng:`prefix[i] = prefix[i-1] + 1`. Ngược lại, đặt`prefix[i] = prefix[i-1]`. 
3. Với mỗi truy vấn được xác định bởi l và r, hãy tính câu trả lời là`prefix[r-1] - prefix[l-1]`. Điều này cho biết số vị trí tôi sao cho`s[i] == s[i+1]`cho tôi trong [l, r-1]. 
4. Xuất ra câu trả lời được tính toán cho mỗi truy vấn. 

Tại sao nó hoạt động: Mảng tiền tố duy trì một bất biến`prefix[i]`luôn bằng số cặp ký tự bằng nhau liên tiếp từ đầu chuỗi đến vị trí i. Bằng cách trừ giá trị tiền tố tại l−1 khỏi giá trị tiền tố tại r−1, chúng tôi tách riêng số lượng cặp nằm hoàn toàn trong phạm vi truy vấn. Điều này tránh việc tính hai lần và đảm bảo tính chính xác cho tất cả các ranh giới truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
n = len(s)
prefix = [0] * (n + 1)

for i in range(1, n):
    prefix[i] = prefix[i-1] + (1 if s[i] == s[i-1] else 0)

m = int(input())
for _ in range(m):
    l, r = map(int, input().split())
    print(prefix[r-1] - prefix[l-1])
```Phần đầu tiên đọc chuỗi và khởi tạo mảng tổng tiền tố. Lặp lại từ 1 đến n−1 đảm bảo chúng ta so sánh từng ký tự với hàng xóm bên trái của nó. Đối với các truy vấn, trừ`prefix[l-1]`từ`prefix[r-1]`tính toán chính xác việc lập chỉ mục dựa trên 1 trong vấn đề khi sử dụng mảng dựa trên 0 trong Python. Các lỗi xảy ra có thể tránh được bằng cách căn chỉnh cẩn thận chỉ mục mảng với sơ đồ lập chỉ mục của bài toán. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
s = "......"
queries = [(3,4), (2,3), (1,6), (2,6)]
```| tôi | s[i] | tiền tố[i] | 
| --- | --- | --- | 
| 0 | . | 0 | 
| 1 | . | 1 | 
| 2 | . | 2 | 
| 3 | . | 3 | 
| 4 | . | 4 | 
| 5 | . | 5 | 

Truy vấn tính toán: 

- (3,4): tiền tố[3] - tiền tố[2] = 3 - 2 = 1 
- (2,3): tiền tố[2] - tiền tố[1] = 2 - 1 = 1 
- (1,6): tiền tố[5] - tiền tố[0] = 5 - 0 = 5 
- (2,6): tiền tố[5] - tiền tố[1] = 5 - 1 = 4 

Đầu vào mẫu 2:```
s = ".#.#"
queries = [(1,2), (1,4), (2,4)]
```| tôi | s[i] | tiền tố[i] | 
| --- | --- | --- | 
| 0 | . | 0 | 
| 1 | # | 0 | 
| 2 | . | 0 | 
| 3 | # | 0 | 

Truy vấn tính toán: 

- (1,2): 0 - 0 = 0 
- (1,4): 0 - 0 = 0 
- (2,4): 0 - 0 = 0 

Điều này thể hiện việc xử lý chính xác khi không có ký tự liên tiếp nào khớp và xác nhận rằng các ranh giới được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | O(n) để xây dựng mảng tiền tố, O(1) mỗi truy vấn cho m truy vấn | 
| Không gian | O(n) | Mảng tiền tố có độ dài n+1 | 

Cho n, m ≤ 10^5, điều này dẫn đến tối đa 2·10^5 thao tác cho tiền tố và 10^5 thao tác cho truy vấn, trong giới hạn thời gian 2 giây. Mức sử dụng bộ nhớ khoảng 0,8 MB cho mảng tiền tố, không đáng kể so với 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    s = input().strip()
    n = len(s)
    prefix = [0] * (n + 1)
    for i in range(1, n):
        prefix[i] = prefix[i-1] + (1 if s[i] == s[i-1] else 0)
    m = int(input())
    for _ in range(m):
        l, r = map(int, input().split())
        print(prefix[r-1] - prefix[l-1])
    return output.getvalue().strip()

# Provided samples
assert run("......\n4\n3 4\n2 3\n1 6\n2 6\n") == "1\n1\n5\n4", "sample 1"
assert run(".#.#\n3\n1 2\n1 4\n2 4\n") == "0\n0\n0", "sample 2"

# Custom cases
assert run("##\n1\n1 2\n") == "1", "two identical chars"
assert run(".#\n1\n1 2\n") == "0", "two different chars"
assert run("######\n2\n1 6\n2 5\n") == "5\n3", "all equal, multiple queries"
assert run(".#.#.#.#\n3\n1 8\n1 2\n7 8\n") == "0\n0\n0", "alternating pattern"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "##", 1 2 | 1 | đầu vào có kích thước tối thiểu với các ký tự giống hệt nhau | 
| ".#", 1 2 | 0 | đầu vào có kích thước tối thiểu với các ký tự khác nhau | 
| "######", 1 6 và 2 5 | 5, 3 | nhiều truy vấn trên chuỗi thống nhất | 
| ".#.#.#.#", 1 8, v.v. | 0,0,0 | mẫu xen kẽ, xác nhận việc xử lý số 0 chính xác | 

## Vỏ cạnh 

Đối với một chuỗi có độ dài 2, mảng tiền tố
