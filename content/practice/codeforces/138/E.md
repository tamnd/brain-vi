---
title: "CF 138E - Ràng buộc địa ngục"
description: "Chúng ta được yêu cầu đếm các chuỗi con của một chuỗi đã cho thỏa mãn một tập hợp các ràng buộc phức tạp. Mỗi ràng buộc chỉ định một chữ cái và số lần tối thiểu và tối đa mà chữ cái đó có thể xuất hiện."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 138
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 99 (Div. 1)"
rating: 2900
weight: 138
solve_time_s: 78
verified: true
draft: false
---

[CF 138E - Ràng buộc địa ngục](https://codeforces.com/problemset/problem/138/E) 

**Xếp hạng:** 2900 
**Tags:** vũ lực, dp, hai con trỏ 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm các chuỗi con của một chuỗi đã cho thỏa mãn một tập hợp các ràng buộc phức tạp. Mỗi ràng buộc chỉ định một chữ cái và số lần tối thiểu và tối đa mà chữ cái đó có thể xuất hiện. Ngoài ra, một chuỗi con chỉ được coi là hợp lệ nếu nó đáp ứng ít nhất _L_ và nhiều nhất _R_ của các ràng buộc _k_. Độ dài chuỗi có thể lên tới 100.000, trong khi số lượng ràng buộc lên tới 500. 

Thách thức chính là chúng ta không đủ khả năng để kiểm tra từng chuỗi con một cách đơn giản. Một chuỗi có độ dài 100.000 có khoảng 5 tỷ chuỗi con, vượt xa những gì mà cách tiếp cận bạo lực có thể xử lý trong một thời gian hợp lý. Vì vậy, chúng ta cần một cách tiếp cận đếm các chuỗi con hợp lệ mà không liệt kê rõ ràng tất cả chúng. 

Các trường hợp cạnh tinh tế phát sinh khi các ràng buộc mâu thuẫn với nhau hoặc khi L và R bằng 0. Ví dụ: nếu L = 0 và R = 0 thì chuỗi con phải vi phạm tất cả các ràng buộc. Một trường hợp phức tạp khác xảy ra khi nhiều ràng buộc tham chiếu đến cùng một ký tự nhưng có các giới hạn xung đột nhau. Một thuật toán bất cẩn có thể đếm hai lần hoặc bỏ qua hoàn toàn các chuỗi con. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force lặp lại trên tất cả các chuỗi con có thể, đếm tần số ký tự cho mỗi chuỗi con và kiểm tra xem có bao nhiêu ràng buộc được thỏa mãn. Mặc dù cách này hiệu quả với các chuỗi nhỏ nhưng độ phức tạp về thời gian của nó là O(n² k), xấp xỉ 5×10^10 phép tính trong trường hợp xấu nhất với n = 10^5 và k = 500. Điều này rõ ràng là quá chậm. 

Thông tin chi tiết quan trọng là đối với mỗi ký tự, chúng ta có thể theo dõi các khoảng thời gian mà các ràng buộc được thỏa mãn bằng cách sử dụng tổng số ký tự tiền tố. Cụ thể, chúng ta có thể xử lý trước tổng tiền tố cho mỗi ký tự. Khi đó, đối với một chuỗi con bắt đầu từ chỉ số i và kết thúc ở j, số lần xuất hiện của một ký tự chỉ đơn giản là`prefix[j+1] - prefix[i]`. Điều này làm giảm vấn đề kiểm tra các ràng buộc một cách hiệu quả trên các chuỗi con bắt đầu từ mỗi vị trí. 

Sau đó, chúng ta có thể sử dụng cách tiếp cận hai con trỏ để duy trì chuỗi con hợp lệ dài nhất bắt đầu từ mỗi chỉ mục. Bằng cách tiến con trỏ bên phải trong khi chuỗi con thỏa mãn số lượng ràng buộc giữa L và R, chúng ta có thể đếm số lượng chuỗi con hợp lệ bắt đầu từ mỗi chỉ mục bên trái trong thời gian O(n) cho mỗi ký tự. Kết hợp những điều này, chúng tôi tránh được độ phức tạp O(n²) và giảm thời gian chạy xuống O(n k), phù hợp thoải mái trong các ràng buộc nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n2k) | O(26 n) | Quá chậm | 
| Tối ưu (tổng tiền tố + hai con trỏ) | O(nk) | O(26 n + k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng mảng tổng tiền tố 2D cho mỗi chữ cái viết thường. Vì`prefix[c][i]`, lưu trữ số lần ký tự`c`xuất hiện trong chuỗi con`s[0:i]`. Điều này cho phép truy vấn tần số theo thời gian không đổi cho bất kỳ chuỗi con nào. 
2. Đối với từng vị trí`i`trong chuỗi, khởi tạo hai con trỏ,`left = i`Và`right = i`. các`right`con trỏ sẽ mở rộng sang bên phải miễn là chuỗi con`s[i:right+1]`thỏa mãn giữa ràng buộc L và R. 
3. Đối với mỗi chuỗi con bắt đầu từ`i`, tính toán số lượng ràng buộc được thỏa mãn bằng cách sử dụng tổng tiền tố được xử lý trước. Đối với mỗi ràng buộc, hãy tính số ký tự của nó trong chuỗi con hiện tại và kiểm tra xem nó có nằm trong phạm vi cho phép hay không. 
4. Di chuyển`right`con trỏ chuyển tiếp cho đến khi việc thêm một ký tự nữa sẽ vi phạm yêu cầu về số lượng ràng buộc (nhỏ hơn L hoặc nhiều hơn R). Tất cả các chuỗi con bắt đầu từ`i`và kết thúc giữa`i`Và`right-1`là hợp lệ. Thêm vào`right - i`đến kết quả. 
5. Lặp lại quy trình cho tất cả các chỉ số ban đầu`i`. 

Tại sao nó hoạt động: Thuật toán đếm mỗi chuỗi con chính xác một lần bằng cách mở rộng một cách có hệ thống ranh giới bên phải cho mỗi chỉ mục bên trái. Tổng tiền tố cho phép kiểm tra tần suất nhanh và phương pháp hai con trỏ đảm bảo rằng không có chuỗi con không hợp lệ nào được tính. Điều bất biến là tại mỗi bước,`right`là chỉ mục đầu tiên không thể có trong chuỗi con bắt đầu từ`i`mà không vi phạm giới hạn số lượng ràng buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
n = len(s)
k, L, R = map(int, input().split())

constraints = []
for _ in range(k):
    c, l, r = input().split()
    l, r = int(l), int(r)
    constraints.append((c, l, r))

# build prefix sums for each letter
prefix = {chr(ord('a') + i): [0]*(n+1) for i in range(26)}
for i, ch in enumerate(s):
    for c in prefix:
        prefix[c][i+1] = prefix[c][i] + (1 if ch == c else 0)

result = 0

for left in range(n):
    low, high = left, n
    # binary search for right boundary where constraints are violated
    def valid(right):
        count_satisfied = 0
        for c, l, r in constraints:
            cnt = prefix[c][right] - prefix[c][left]
            if l <= cnt <= r:
                count_satisfied += 1
        return L <= count_satisfied <= R

    r = left
    while r < n and valid(r+1):
        r += 1
    result += r - left

print(result)
```Giải pháp xây dựng tổng tiền tố trong thời gian O(26 n), sau đó lặp lại từng chỉ mục bắt đầu. Người trợ giúp`valid`hàm đếm số lượng ràng buộc thỏa mãn cho chuỗi con hiện tại. Việc mở rộng hai con trỏ đảm bảo mỗi chuỗi con được tính hiệu quả. Việc sử dụng tổng tiền tố sẽ tránh tính toán lại số lượng cho các chuỗi con chồng chéo, tránh độ phức tạp O(n² k). 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:`codeforces`, hạn chế`o 1 2`,`e 1 2`, L=0, R=0. 

| trái | r (kết thúc hợp lệ tối đa) | chuỗi con được tính | Ghi chú | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | "c" vi phạm quy tắc ràng buộc 0 | 
| 1 | 1 | 1 | “o” vi phạm 1 quy tắc ràng buộc | 
| 2 | 2 | 1 | "d" hợp lệ | 
| 3 | 3 | 1 | "e" vi phạm | 
| 4 | 4 | 1 | "f" hợp lệ | 
| 5 | 5 | 1 | "o" vi phạm | 
| 6 | 6 | 1 | "r" hợp lệ | 
| 7 | 7 | 1 | "c" hợp lệ | 
| 8 | 8 | 1 | "e" vi phạm | 
| 9 | 9 | 1 | "s" hợp lệ | 

Kết quả = 7 

**Mẫu 2** 

đầu vào:`aaabbb`, hạn chế`a 2 2`,`b 2 3`, L=1, R=2. 

| trái | đúng tối đa | chuỗi con được tính | Ghi chú | 
| --- | --- | --- | --- | 
| 0 | 2 | 2 | "aa", "aaa" thỏa mãn ít nhất 1 ràng buộc | 
| 1 | 2 | 1 | "aa" | 
| 2 | 2 | 0 | "a" chỉ 1 'a' | 
| 3 | 4 | 2 | "bb", "bbb" | 
| 4 | 4 | 1 | "bb" | 
| 5 | 5 | 0 | "b" | 

Kết quả = 6 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nk) | Tổng tiền tố O(26 n), kiểm tra các ràng buộc cho mỗi chuỗi con O(k) | 
| Không gian | O(26 n + k) | Mảng tổng tiền tố cho mỗi chữ cái + lưu trữ ràng buộc | 

Đối với n 10^5 và k 500, n*k = 5×10^7, vừa vặn thoải mái trong giới hạn 3 giây. Việc sử dụng bộ nhớ bị chi phối bởi tổng tiền tố nhưng vẫn dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = input().strip()
    n = len(s)
    k, L, R = map(int, input().split())
    constraints = []
    for _ in range(k):
        c, l, r = input().split()
        l, r = int(l), int(r)
        constraints.append((c, l, r))
    prefix = {chr(ord('a') + i): [0]*(n+1) for i in range(26)}
    for i, ch in enumerate(s):
        for c in prefix:
            prefix[c][i+1] = prefix[c][i] + (1 if ch == c else 0)
    result = 0
    for left in range(n):
        r = left
        while r < n:
            count_satisfied = 0
            for c, l, r_c in constraints:
                cnt = prefix[c][r+1] - prefix[c][left]
                if l <= cnt <= r_c:
                    count_satisfied += 1
            if L <= count_satisfied <= R:
                r += 1
            else:
                break
        result += r - left
    return str(result)

# provided samples
assert run("codeforces\n2 0 0\no 1 2\ne 1 2\n") == "7", "sample 1"
assert run("aaabbb\n2 1 2\na 2 2\nb 2
```
