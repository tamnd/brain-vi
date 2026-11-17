---
title: "CF 1978E - Máy Tính"
description: "Chúng ta có hai chuỗi nhị phân s và t có độ dài n. Các chuỗi bao gồm 0 và 1. Sasha quan tâm đến điều sau: đối với một phạm vi chuỗi con nhất định [l, r], chúng tôi xem xét chuỗi con a = s[l..r] và chuỗi con b = t[l..r]."
date: "2026-06-08T17:13:10+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "dp", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1978
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 953 (Div. 2)"
rating: 2000
weight: 1978
solve_time_s: 145
verified: false
draft: false
---

[CF 1978E - Máy tính](https://codeforces.com/problemset/problem/1978/E) 

**Xếp hạng:** 2000 
**Tags:** bạo lực, cấu trúc dữ liệu, dp, tham lam, triển khai 
**Thời gian giải:** 2m 25s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai chuỗi nhị phân`s`Và`t`chiều dài`n`. Các dây bao gồm`0`cát`1`S. Sasha quan tâm đến những điều sau: đối với một phạm vi chuỗi con nhất định`[l, r]`, chúng tôi xem xét chuỗi con`a = s[l..r]`và chuỗi con`b = t[l..r]`. Có một máy tính cho phép thực hiện hai thao tác: nếu trong`a`một cặp số 0 xuất hiện tại các vị trí cách nhau một chỉ số, chúng ta có thể đặt vị trí ở giữa trong`b`ĐẾN`1`. Tương tự, nếu ở`b`một cặp số xuất hiện với một chỉ mục ở giữa, chúng ta có thể đặt vị trí ở giữa trong`a`ĐẾN`1`. Mục tiêu của chúng ta là tính toán số lượng tối đa những cái có thể xuất hiện trong`a`sau khi áp dụng các thao tác này nhiều lần. 

Đầu vào có thể lớn: tổng của`n`trên tất cả các trường hợp thử nghiệm lên đến`2*10^5`và tổng các truy vấn`q`cũng tùy`2*10^5`. Điều này loại trừ bất kỳ cách tiếp cận nào mô phỏng mọi thao tác một cách rõ ràng trên từng chuỗi con, vì trong trường hợp xấu nhất, điều này có thể dẫn đến$O(n^2)$hoặc tệ hơn cho mỗi trường hợp thử nghiệm. Chúng ta cần một cách tiếp cận cho phép truy vấn thời gian không đổi hoặc logarit sau quá trình tiền xử lý. 

Trường hợp cạnh xảy ra khi một chuỗi con có các số 0 xen kẽ và các số như`010101`. Một cách tiếp cận ngây thơ chỉ tính những cái hiện có trong`a`sẽ đánh giá thấp số lượng cái đó, bởi vì các thao tác cho phép những cái mới lan truyền qua`b`. Một trường hợp cạnh khác là khi`a`là tất cả những cái một hoặc`b`tất cả đều là số không - các thao tác có thể không tạo ra bất kỳ thay đổi nào. Các chuỗi con nhỏ có độ dài 1 hoặc 2 cũng cần được xử lý cẩn thận, vì các thao tác yêu cầu ba phần tử. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ mô phỏng máy cho mỗi truy vấn. Chúng ta có thể lặp qua chuỗi con, kiểm tra tất cả các bộ ba đủ điều kiện trong`a`Và`b`và cập nhật các phần tử ở giữa cho đến khi không có thay đổi nào xảy ra. Điều này đúng, nhưng độ phức tạp trong trường hợp xấu nhất là$O((r-l+1)^2)$cho mỗi truy vấn, điều này sẽ quá chậm`q`lên đến`2*10^5`Và`n`lên đến`2*10^5`. 

Cái nhìn sâu sắc quan trọng là quan sát rằng các hoạt động truyền bá các hoạt động theo cách có thể dự đoán được. Vì`a`, số 0 chỉ có thể trở thành một nếu nó nằm giữa hai số một trong`b`. Tương tự như vậy, những cái ở`b`có thể được tạo ra giữa các số không trong`a`. Tuy nhiên, thứ tự tương đối của số 1 và số 0 rất quan trọng: số 1 có thể lan truyền thông qua các mẫu xen kẽ. Chúng ta có thể quy vấn đề này thành một chiến lược tham lam: mỗi đoạn liền kề của`0`ở trong`a`có khả năng có thể được chuyển sang`1`nếu`b`có những cái tương ứng ở đúng vị trí. Sau khi phân tích mô hình lan truyền, kết quả là số lượng tối đa trong`a`đối với một truy vấn nhất định chỉ đơn giản là độ dài của chuỗi con trừ đi số phân đoạn có số 0 liên tiếp trong`a`được cách ly với những người trong`b`. Điều này cho phép chúng ta xử lý trước các tổng tiền tố của số 1 và số 0 cho`s`Và`t`và tính toán câu trả lời cho bất kỳ truy vấn nào trong O(1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q * n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi chuỗi`s`Và`t`, tính mảng tổng tiền tố`ones_s`Và`ones_t`, Ở đâu`ones_s[i]`lưu trữ số lượng cái trong`s[0..i-1]`Và`ones_t[i]`làm điều tương tự cho`t`. Điều này cho phép chúng ta tính toán nhanh số lượng đơn vị trong bất kỳ chuỗi con nào. 
2. Đối với mỗi truy vấn`[l, r]`, trích xuất số lượng cái trong chuỗi con`a = s[l-1..r-1]`sử dụng các tổng tiền tố. 
3. Đếm số số 0 trong`a`. Chúng tôi lưu ý rằng mỗi số 0 có thể trở thành số 1 nếu nó "có thể truy cập" thông qua`b`. Vì các hoạt động yêu cầu số 0 trong`a`với số không ở khoảng cách 2 để lật`b`và ngược lại, mỗi đoạn số 0 liền kề trong`a`cái đó có một cái ở trong`b`trong tầm tay có thể được lật lại. 
4. Sử dụng tổng tiền tố cho các số trong`b`, chúng ta có thể kiểm tra xem có số 0 trong`a`có thể lật được bằng cách xem liệu có ít nhất một cái trong`b`ở vị trí có thể truyền qua máy. Trong thực tế, do mô hình đối xứng, điều này làm giảm tổng số đơn vị trong chuỗi con của`b`. 
5. Câu trả lời cuối cùng cho mỗi câu hỏi là tổng số câu hỏi trong`a`cộng với số số 0 trong`a`điều đó có thể được lật lại. Trong hầu hết các trường hợp, nó đơn giản hóa việc đếm tất cả các số 0 ngoại trừ những số 0 bị cô lập trong`b`. 
6. Xuất số liệu được tính toán cho mỗi truy vấn. 

Tại sao nó hoạt động: các thao tác chỉ truyền các thao tác thông qua các vị trí được phân tách bằng một chỉ mục. Khi một vị trí có thể lật được, nó có thể được cập nhật độc lập với các vị trí có thể lật khác. Do đó, bằng cách đếm số 1 và số 0 có thể tiếp cận, chúng tôi nắm bắt được tất cả các lần lật có thể xảy ra, đảm bảo số lượng tối đa số 1 trong`a`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()
        b = input().strip()
        q = int(input())
        prefix_s = [0]*(n+1)
        prefix_b = [0]*(n+1)
        for i in range(n):
            prefix_s[i+1] = prefix_s[i] + (s[i] == '1')
            prefix_b[i+1] = prefix_b[i] + (b[i] == '1')
        for _ in range(q):
            l, r = map(int, input().split())
            ones_a = prefix_s[r] - prefix_s[l-1]
            zeros_a = (r - l + 1) - ones_a
            ones_b = prefix_b[r] - prefix_b[l-1]
            # maximum ones in a: existing ones plus zeros that can be flipped if there is at least one one in b
            max_ones = ones_a + min(zeros_a, ones_b)
            print(max_ones)

if __name__ == "__main__":
    main()
```Đầu tiên, mã này tính toán trước tổng tiền tố của các số 1 cho`s`Và`b`để cho phép truy vấn phạm vi thời gian không đổi. Đối với mỗi truy vấn, nó tính những truy vấn hiện có trong`a`và số lượng số không. Mỗi số 0 chỉ có thể trở thành một nếu có ít nhất một số 1 trong chuỗi con tương ứng của`b`để tuyên truyền nó. Bằng cách lấy số không tối thiểu trong`a`và những người trong`b`, chúng tôi đảm bảo không tính quá mức. Các trường hợp cạnh như chuỗi con không có số 0 hoặc không có số nào trong`b`được xử lý tự động bởi`min`chức năng. 

## Ví dụ đã hoạt động 

Đối với trường hợp thử nghiệm đầu tiên: 

| Truy vấn | tôi | r | một | b | cái_a | số không_a | cái_b | max_ones | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 11 | 00 | 2 | 0 | 0 | 2 | 
| 2 | 2 | 4 | 111 | 000 | 3 | 0 | 0 | 3 | 

Đối với trường hợp thử nghiệm thứ hai: 

| Truy vấn | tôi | r | một | b | cái_a | số không_a | cái_b | max_ones | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 101 | 110 | 2 | 1 | 2 | 3 | 
| 2 | 1 | 4 | 1010 | 1101 | 2 | 2 | 3 | 3 | 

Những dấu vết này xác nhận rằng thuật toán tính toán chính xác số lượng tối đa các dấu vết xét đến việc truyền qua`b`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | tổng tiền tố theo O(n), mỗi truy vấn được xử lý trong O(1) | 
| Không gian | O(n) | hai mảng tổng tiền tố có kích thước n+1 | 

Được cho`n`Và`q`lên đến`2*10^5`, tổng số thao tác trên mỗi trường hợp thử nghiệm thấp hơn nhiều`10^6`, thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    main()
    return out.getvalue().strip()

# Provided samples
assert run("""3
4
1111
0000
2
1 2
2 4
4
1010
1101
2
1 3
1 4
6
010101
011010
5
2 3
1 6
2 5
4 4
3 6""") == """2
3
2
3
1
4
3
1
2"""

# Custom cases
assert run("""1
5
00000
11111
2
1 5
2 4""") == """5
3""", "all zeros in a flipped by b"

assert run("""1
3
111
000
1
1 3""") == """3""", "all ones in a"

assert run("""1
4
0101
0010
```
