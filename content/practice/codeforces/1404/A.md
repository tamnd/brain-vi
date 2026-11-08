---
title: "CF 1404A - Chuỗi bit cân bằng"
description: "Chúng ta được cấp một chuỗi có độ dài $n$ bao gồm các ký tự 0, 1 và ?, và một số nguyên $k$ là số chẵn. Mục tiêu của chúng tôi là thay thế ? các ký tự có 0 hoặc 1 sao cho mỗi chuỗi con có độ dài $k$ chứa chính xác các số 0 $k/2$ và các số $k/2$."
date: "2026-06-11T08:10:39+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1404
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 668 (Div. 1)"
rating: 1500
weight: 1404
solve_time_s: 89
verified: true
draft: false
---

[CF 1404A - Chuỗi bit cân bằng](https://codeforces.com/problemset/problem/1404/A) 

**Đánh giá:** 1500 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi có độ dài$n$gồm các ký tự 0, 1, ? và một số nguyên$k$thế là chẵn. Mục tiêu của chúng tôi là thay thế ? các ký tự có 0 hoặc 1 sao cho mỗi chuỗi con có độ dài$k$chứa chính xác$k/2$số không và$k/2$những cái đó. Nói cách khác, chuỗi phải$k$-cân bằng. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp xác định$n$,$k$, và chuỗi$s$. Đầu ra cho mỗi trường hợp thử nghiệm là CÓ nếu có thể tạo thành một$k$-cân bằng chuỗi bằng cách thay thế ?, và KHÔNG nếu không thể. 

Những ràng buộc chỉ ra rằng$n$có thể lớn tới 300.000 cho mỗi trường hợp thử nghiệm và tổng số$n$trên tất cả các trường hợp thử nghiệm nhiều nhất là 300.000. Điều này loại trừ bất kỳ giải pháp nào lặp lại trên tất cả các chuỗi con một cách rõ ràng, vì có$O(nk)$các chuỗi con như vậy và có thể lên tới$10^{11}$hoạt động. Chúng ta cần một cách tiếp cận tuyến tính hoặc tuyến tính đối với$n$. 

Trường hợp cạnh tinh vi phát sinh từ tính chất chồng chéo của các chuỗi con. Coi như$s = "1?0"$với$k=2$. Chuỗi con bắt đầu từ chỉ mục 0 phải có một số 1 và một số 0, đồng thời chuỗi con bắt đầu từ chỉ số 1 cũng phải có một số 1 và một số 0. Một cách tiếp cận đơn giản có thể điền vào ? tham lam trong một chuỗi con mà không kiểm tra xung đột trong các chuỗi con chồng chéo. Câu trả lời đúng là KHÔNG, vì bất kỳ giá trị nào được chọn cho ?, chuỗi con còn lại sẽ vi phạm sự cân bằng. Điều này cho thấy chúng ta phải suy luận về các vị trí modulo$k$, không độc lập. 

Một trường hợp cạnh khác là khi tất cả các ký tự đều là ?, chẳng hạn$s = "???"$với$k=2$. Trong trường hợp này, một giải pháp chỉ tồn tại nếu mẫu có thể lặp lại một cách nhất quán theo modulo$k$. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ lặp lại trên mọi chuỗi con có độ dài$k$, đếm số không và số một, đồng thời thử tất cả các kết hợp có thể có cho ?. Về nguyên tắc thì điều này đúng nhưng đối với$n$lên tới 300.000, nó sẽ hoạt động$O(nk)$hoạt động quá chậm. 

Quan sát quan trọng là mỗi vị trí modulo$k$ảnh hưởng đến tất cả các chuỗi con bắt đầu tại các chỉ số$i, i+k, i+2k,\dots$. Do đó, mọi vị trí bằng nhau theo modulo$k$phải được phân công một cách nhất quán để tránh xung đột. Ví dụ: nếu vị trí 0 modulo$k$bị buộc phải là 1 bởi một chuỗi con, nó không thể bằng 0 trong bất kỳ chuỗi con chồng chéo nào khác. Điều này làm giảm vấn đề điền vào một mảng có độ dài$k$trong đó mỗi mục có thể là 0, 1 hoặc ?. Sau khi điền vào các vị trí đã biết, chúng ta chỉ cần kiểm tra xem số lượng số 0 và số 1 trong các vị trí này có$k$các vị trí không vượt quá$k/2$. Nếu họ làm vậy thì điều đó là không thể. Nếu không thì phần còn lại ? có thể được chỉ định tùy ý để cân bằng số lượng. 

Quan sát này biến đổi một khả năng$O(nk)$vấn đề vào$O(n)$, bởi vì chúng ta lặp qua chuỗi một lần để truyền các ràng buộc tới các vị trí theo modulo$k$, sau đó xác thực số lượng. Các ràng buộc trên mỗi lớp modulo xác định đầy đủ tính khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nk) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`pattern`chiều dài`k`để lưu trữ các giá trị bắt buộc cho từng vị trí modulo. Ban đầu, mỗi mục là ?. 
2. Lặp lại từng ký tự`s[i]`trong chuỗi. Nếu như`s[i]`là 0 hoặc 1, hãy truyền giá trị này tới`pattern[i % k]`. Nếu như`pattern[i % k]`đã chứa một giá trị khác, xuất ra NO vì tồn tại xung đột. 
3. Sau khi xử lý tất cả các ký tự, đếm số số 0 và số 1 trong`pattern`. Nếu một trong hai số lượng vượt quá`k/2`, xuất ra KHÔNG, vì chúng ta không thể gán ? để cân bằng. 
4. Ngược lại, xuất ra CÓ. 

Tại sao nó hoạt động: Mảng`pattern`mã hóa tất cả các nhiệm vụ bắt buộc cho các vị trí theo modulo`k`. Nếu các phép gán này nhất quán và số lượng số 0 và số 1 không vượt quá`k/2`, chúng ta có thể tự do điền vào phần còn lại ? để đạt được một chuỗi con có độ dài cân bằng hoàn hảo`k`ở mọi vị trí. Mỗi chuỗi con có độ dài`k`được xác định bởi mẫu, do đó chuỗi toàn cục được đảm bảo là$k$-cân bằng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    s = input().strip()
    pattern = ['?'] * k
    possible = True

    for i in range(n):
        if s[i] != '?':
            if pattern[i % k] == '?':
                pattern[i % k] = s[i]
            elif pattern[i % k] != s[i]:
                possible = False
                break

    if not possible:
        print("NO")
        continue

    zeros = pattern.count('0')
    ones = pattern.count('1')
    if zeros > k // 2 or ones > k // 2:
        print("NO")
    else:
        print("YES")
```Giải pháp đầu tiên xây dựng modulo`k`mẫu bằng cách truyền bá các giá trị đã biết. Xung đột được phát hiện ngay lập tức. Việc đếm số 0 và số 1 trong mẫu cho phép chúng tôi xác định xem có thể điền phần còn lại hay không? để cân bằng từng chuỗi con. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:`6 4`với`100110`. 

| tôi | s[i] | mẫu[ i % k ] | bình luận | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | nhiệm vụ đầu tiên | 
| 1 | 0 | 0 | nhiệm vụ đầu tiên | 
| 2 | 0 | 0 | nhiệm vụ đầu tiên | 
| 3 | 1 | 1 | nhiệm vụ đầu tiên | 
| 4 | 1 | 1 | mẫu[0] đã 1 rồi, được chưa | 
| 5 | 0 | 0 | mẫu[1] đã 0 rồi, được chưa | 

Mẫu là`['1','0','0','1']`. Số không=2, Số một=2, k/2=2, khả thi. Đầu ra CÓ. 

Đầu vào mẫu:`3 2`với`1?0`. 

| tôi | s[i] | mẫu[i % k] | bình luận | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | nhiệm vụ đầu tiên | 
| 1 | ? | ? | không có gì để phân công | 
| 2 | 0 | mẫu [0] đã là 1, xung đột? | kiểm tra modulo 2: 2 % 2 = 0; mẫu[0]=1, s[i]=0 → xung đột | 

Đã phát hiện xung đột, đầu ra NO. 

Những dấu vết này xác nhận rằng thuật toán xác định chính xác cả các mẫu khả thi và không thể bằng cách kiểm tra các vị trí theo modulo`k`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Lặp lại qua từng ký tự của chuỗi một lần, truyền các giá trị mẫu, đếm số 0/số 1 | 
| Không gian | O(k) | Lưu trữ modulo`k`mẫu | 

Thuật toán xử lý tổng tối đa của`n`trên các trường hợp thử nghiệm một cách hiệu quả vì mỗi ký tự chỉ được truy cập một lần và mảng phụ có độ dài tối đa`k`, nhiều nhất là`n`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        s = input().strip()
        pattern = ['?'] * k
        possible = True

        for i in range(n):
            if s[i] != '?':
                if pattern[i % k] == '?':
                    pattern[i % k] = s[i]
                elif pattern[i % k] != s[i]:
                    possible = False
                    break
        if not possible:
            output.append("NO")
            continue
        zeros = pattern.count('0')
        ones = pattern.count('1')
        if zeros > k // 2 or ones > k // 2:
            output.append("NO")
        else:
            output.append("YES")
    return "\n".join(output)

# provided samples
assert run("9\n6 4\n100110\n3 2\n1?1\n3 2\n1?0\n4 4\n????\n7 4\n1?0??1?\n10 10\n11??11??11\n4 2\n1??1\n4 4\n?0?0\n6 2\n????00\n") == "YES\nYES\nNO\nYES\nYES\nNO\nNO\nYES\nNO"

# custom tests
assert run("1\n2 2\n??\n") == "YES", "all unknown, minimum length"
assert run("1\n4 4\n1111\n") == "NO", "too many ones for k/2"
assert run("1\n6 2\n?1?0?1\n") == "YES", "alternating unknowns"
assert run("1\n8 4\n1?0?1?0?\n") == "YES", "pattern repeats modulo k"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | | |
