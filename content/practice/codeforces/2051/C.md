---
title: "CF 2051C - Chuẩn bị cho kỳ thi"
description: "Có thể có $n$ câu hỏi thi. Mỗi phiên bản bài kiểm tra có sẵn chứa tất cả các câu hỏi ngoại trừ một câu hỏi. Giá trị $ai$ cho chúng ta biết câu hỏi nào bị bỏ qua trong phiên bản $i$-th. Monocarp biết một bộ câu hỏi $k$."
date: "2026-06-08T08:40:17+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2051
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 995 (Div. 3)"
rating: 1000
weight: 2051
solve_time_s: 119
verified: true
draft: false
---

[CF 2051C - Chuẩn bị cho kỳ thi](https://codeforces.com/problemset/problem/2051/C) 

**Đánh giá:** 1000 
**Tas:** thuật toán xây dựng, triển khai 
**Thời gian giải:** 1 phút 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

có$n$các câu hỏi thi có thể. Mỗi phiên bản bài kiểm tra có sẵn chứa tất cả các câu hỏi ngoại trừ một câu hỏi. giá trị$a_i$cho chúng ta biết câu hỏi nào bị bỏ qua trong$i$-phiên bản thứ. 

Monocarp biết một bộ$k$câu hỏi. Đối với mỗi phiên bản đề thi, chúng ta phải quyết định xem mọi câu hỏi xuất hiện trong phiên bản đó có nằm trong số những câu hỏi mà em biết hay không. 

Đầu ra là một chuỗi nhị phân có độ dài$m$. các$i$-ký tự thứ là`1`nếu Monocarp vượt qua phiên bản bài kiểm tra bỏ qua câu hỏi$a_i$, nếu không thì`0`. 

Những hạn chế là lớn. Tổng của tất cả$n$đạt đến các giá trị trên các trường hợp thử nghiệm$3 \cdot 10^5$, do đó, bất kỳ thuật toán nào kiểm tra rõ ràng tất cả$n-1$các câu hỏi bên trong mỗi phiên bản thi sẽ yêu cầu$O(nm)$hoạt động và sẽ rất chậm. Chúng ta cần một giải pháp xử lý từng trường hợp thử nghiệm về cơ bản theo thời gian tuyến tính. 

Một quan sát tinh tế là mỗi phiên bản bài thi khác với bộ câu hỏi đầy đủ bởi đúng một câu hỏi còn thiếu. Điều đó hạn chế đáng kể số lượng các tình huống có thể xảy ra. 

Hãy xem xét trường hợp Edge sau:```
n = 5
known = {1,2,3}
```Có 2 câu hỏi chưa biết là câu 4 và câu 5. Phiên bản thi nào cũng chỉ bỏ một câu hỏi. Ngay cả khi câu hỏi bị bỏ qua là câu 4 thì câu hỏi số 5 vẫn xuất hiện trong bài thi và Monocarp không hề biết. Mọi câu trả lời đều phải`0`. 

Một trường hợp quan trọng khác là:```
n = 5
known = {1,2,3,4}
```Bây giờ có đúng một câu hỏi chưa biết, đó là câu 5. Phiên bản thi duy nhất mà Monocarp có thể vượt qua là phiên bản bỏ câu hỏi 5. Mọi phiên bản khác vẫn có câu hỏi 5. 

Cuối cùng, nếu Monocarp biết tất cả các câu hỏi, anh ấy sẽ vượt qua mọi phiên bản. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ xây dựng từng phiên bản bài thi và xác minh xem tất cả các phiên bản đó có$n-1$câu hỏi thuộc tập hợp đã biết. Vì có$m$phiên bản, chi phí này$O(nm)$. Với$n$Và$m$đều đạt tới hàng trăm nghìn trên đầu vào, điều này là không khả thi. 

Nhận xét quan trọng là chỉ có số lượng câu hỏi chưa biết mới quan trọng. 

Cho phép$$U = n-k$$là số lượng câu hỏi mà Monocarp không biết. 

Nếu như$U \ge 2$, mỗi phiên bản đề thi chỉ bỏ sót một câu hỏi nên trong đề thi còn lại ít nhất một câu hỏi chưa biết. Vượt qua là không thể. 

Nếu như$U = 1$, đặt câu hỏi chưa biết duy nhất là$x$. Monocarp vượt qua chính xác những phiên bản thi bỏ qua$x$. 

Nếu như$U = 0$, Monocarp biết mọi thứ và vượt qua mọi phiên bản. 

Điều này làm giảm toàn bộ vấn đề xuống việc xác định có bao nhiêu câu hỏi chưa biết và trong một trường hợp đặc biệt là tìm ra câu hỏi chưa biết duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nm)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n+m)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc bộ câu hỏi đã biết. 
2. Tính số câu hỏi chưa biết:$$U = n-k$$3. Nếu$U = 0$, xuất ra một chuỗi$m$vì mọi phiên bản bài thi đều có thể vượt qua. 
4. Nếu$U \ge 2$, xuất ra một chuỗi$m$số không vì mỗi phiên bản thi vẫn chứa ít nhất một câu hỏi chưa biết. 
5. Nếu$U = 1$, tìm câu hỏi chưa biết duy nhất$x$. 
6. Với mọi giá trị$a_i$: 

- Đầu ra`1`nếu như$a_i = x$. 
- Đầu ra`0`nếu không thì. 

Nguyên nhân là do phiên bản thi bỏ sót$x$là phiên bản duy nhất loại bỏ câu hỏi duy nhất mà Monocarp không biết. 

### Tại sao nó hoạt động 

Giả sử có ít nhất hai câu hỏi chưa biết. Vì mỗi phiên bản bài thi chỉ loại bỏ một câu hỏi nên bài thi luôn tồn tại ít nhất một câu hỏi chưa biết. Vượt qua là không thể. 

Giả sử có chính xác một câu hỏi chưa biết$x$. Phiên bản thi nào cũng không bỏ sót$x$vẫn chứa$x$, thế là Monocarp thất bại. Phiên bản bỏ qua$x$chỉ chứa những câu hỏi đã biết nên Monocarp đã vượt qua. 

Giả sử không có câu hỏi nào chưa biết. Mọi câu hỏi trong mọi phiên bản đề thi đều biết nên Monocarp luôn đậu. 

Ba trường hợp này bao gồm tất cả các khả năng, do đó thuật toán đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, m, k = map(int, input().split())
        a = list(map(int, input().split()))
        q = list(map(int, input().split()))

        known = set(q)
        missing_cnt = n - k

        if missing_cnt == 0:
            ans.append("1" * m)
            continue

        if missing_cnt >= 2:
            ans.append("0" * m)
            continue

        unknown = -1
        for x in range(1, n + 1):
            if x not in known:
                unknown = x
                break

        res = []
        for x in a:
            res.append('1' if x == unknown else '0')

        ans.append(''.join(res))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo phân tích ba trường hợp trực tiếp. 

Bước không cần thiết duy nhất là tìm ra câu hỏi chưa biết duy nhất khi$n-k=1$. Một bộ băm mang lại$O(1)$kiểm tra thành viên trung bình, do đó quét từ 1 đến$n$tìm thấy nó trong thời gian tuyến tính. 

Không có vấn đề tràn số nguyên nào tồn tại vì chúng tôi chỉ thao tác các chỉ số và số lượng. Tổng công việc trên tất cả các ca kiểm thử được giới hạn bởi tổng của tất cả$n$, thỏa mãn giới hạn bài toán. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
a = [1,2,3,4]
known = [1,3,4]
```| Số lượng | Giá trị | 
| --- | --- | 
| n | 4 | 
| k | 3 | 
| Câu hỏi chưa biết | {2} | 
| Duy nhất chưa biết | 2 | 

Bây giờ hãy đánh giá mọi phiên bản bài kiểm tra. 

| Thiếu câu hỏi$a_i$| Vượt qua? | 
| --- | --- | 
| 1 | Không | 
| 2 | Có | 
| 3 | Không | 
| 4 | Không | 

Đầu ra:```
0100
```Điều này chứng tỏ trường hợp$n-k=1$. 

### Ví dụ 2 

đầu vào:```
n = 5
known = [1,3,4]
```| Số lượng | Giá trị | 
| --- | --- | 
| n | 5 | 
| k | 3 | 
| Câu hỏi chưa biết | {2,5} | 
| Số lượng ẩn số | 2 | 

Vì có ít nhất hai câu hỏi chưa biết nên mỗi phiên bản thi vẫn chứa một câu hỏi chưa biết sau khi chỉ xóa một câu hỏi. 

| Phiên bản thi | Vượt qua? | 
| --- | --- | 
| Bất kỳ phiên bản nào | Không | 

Đầu ra:```
0000
```Điều này chứng tỏ trường hợp$n-k \ge 2$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n+m)$mỗi trường hợp thử nghiệm | Xây dựng bộ, tìm ẩn số duy nhất nếu cần, tạo câu trả lời | 
| Không gian |$O(k)$| Lưu trữ các câu hỏi đã biết | 

Bởi vì tổng của tất cả$n$trên đầu vào nhiều nhất là$3 \cdot 10^5$, tổng thời gian chạy vẫn tuyến tính ở kích thước đầu vào và vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def solve():
        input = sys.stdin.readline
        t = int(input())
        out = []

        for _ in range(t):
            n, m, k = map(int, input().split())
            a = list(map(int, input().split()))
            q = list(map(int, input().split()))

            known = set(q)

            if k == n:
                out.append("1" * m)
                continue

            if n - k >= 2:
                out.append("0" * m)
                continue

            unknown = next(x for x in range(1, n + 1) if x not in known)
            out.append("".join('1' if x == unknown else '0' for x in a))

        return "\n".join(out)

    return solve()

# provided sample
assert run(
"""4
4 4 3
1 2 3 4
1 3 4
5 4 3
1 2 3 4
1 3 4
4 4 4
1 2 3 4
1 2 3 4
2 2 1
1 2
2
"""
) == "0100\n0000\n1111\n10"

# minimum size
assert run(
"""1
2 2 2
1 2
1 2
"""
) == "11"

# exactly one unknown
assert run(
"""1
5 5 4
1 2 3 4 5
1 2 3 4
"""
) == "00001"

# two unknowns
assert run(
"""1
5 5 3
1 2 3 4 5
1 2 3
"""
) == "00000"

# unknown question appears first
assert run(
"""1
4 4 3
1 2 3 4
2 3 4
"""
) == "1000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$n=2,k=2$|`11`| Tất cả các câu hỏi đã biết | 
| Một câu hỏi chưa biết |`00001`| Chính xác là một phiên bản khá | 
| Hai câu hỏi chưa biết |`00000`| Không có phiên bản nào có thể thành công | 
| Câu hỏi chưa biết là 1 |`1000`| Xử lý giá trị biên | 

## Vỏ cạnh 

Hãy xem xét:```
1
4 4 4
1 2 3 4
1 2 3 4
```Monocarp biết mọi câu hỏi. Thuật toán đi vào$n-k=0$nhánh và đầu ra`1111`. Mọi phiên bản thi đều có thể vượt qua vì mọi câu hỏi xuất hiện đều được biết đến. 

Coi như:```
1
5 5 4
1 2 3 4 5
1 2 3 4
```Câu hỏi 5 là câu hỏi duy nhất chưa biết. Thuật toán tìm`unknown = 5`và chỉ đánh dấu phiên bản có`a_i = 5`như có thể vượt qua. Đầu ra là`00001`. 

Coi như:```
1
5 5 3
1 2 3 4 5
1 2 3
```Câu hỏi 4 và 5 đều không rõ. Thuật toán phát hiện$n-k=2$và ngay lập tức xuất ra`00000`. Việc xóa một câu hỏi không thể loại bỏ đồng thời cả hai câu hỏi chưa biết khỏi bài kiểm tra, vì vậy mọi phiên bản đều thất bại.
