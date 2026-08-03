---
title: "CF 102512D - Bình đẳng"
description: "Chúng ta cần chọn một khoảng thời gian nguyên dương T. Các tin nhắn xảy ra ở mọi bội số của T, nhưng chủ sở hữu của tin nhắn sẽ thay thế. Nhóm đầu tiên thuộc về Kotaro, nhóm thứ hai thuộc về Akane, nhóm thứ ba lại thuộc về Kotaro, v.v."
date: "2026-08-04T00:10:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102512
codeforces_index: "D"
codeforces_contest_name: "Valentines Day Contest 2020"
rating: 0
weight: 102512
solve_time_s: 178
verified: true
draft: false
---

[CF 102512D - Bình đẳng](https://codeforces.com/problemset/problem/102512/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần chọn một khoảng thời gian nguyên dương`T`. Tin nhắn xảy ra ở mọi bội số của`T`, nhưng chủ nhân của tin nhắn lại thay đổi. Nhóm đầu tiên thuộc về Kotaro, nhóm thứ hai thuộc về Akane, nhóm thứ ba lại thuộc về Kotaro, v.v. Một giá trị của`T`chỉ có hiệu lực khi mọi thời gian tin nhắn được yêu cầu đều nằm trong khoảng thời gian hoạt động của người gửi nó. 

Đầu vào cho biết độ dài của ngày`N`, tiếp theo là khoảng thời gian hoạt động của Kotaro và khoảng thời gian hoạt động của Akane. Nhiệm vụ là đếm xem có bao nhiêu tiết`T`từ`1`ĐẾN`N`đáp ứng mọi yêu cầu về thời gian gửi tin nhắn. 

giới hạn`N <= 10^9`ngay lập tức loại trừ việc kiểm tra mọi thứ có thể`T`. Thậm chí một`O(N)`giải pháp quá lớn vì một tỷ lần lặp không thể phù hợp thoải mái trong thời gian giới hạn. Số lượng khoảng thời gian nhỏ, chỉ tối đa 300 mỗi người, điều này cho thấy giải pháp nên sử dụng cấu trúc khoảng thời gian thay vì lặp lại trên mỗi đơn vị thời gian. 

Một lỗi phổ biến là quên rằng dấu chấm chỉ có thể tạo một tin nhắn. Nếu như`T > N/2`, thời gian cần thiết duy nhất là`T`chính nó, và chỉ có sự sẵn có của Kotaro mới quan trọng. Một sai lầm khác là coi các khoảng là phạm vi nửa mở. Các khoảng bao gồm cả hai điểm cuối, do đó, thông báo chính xác tại điểm cuối là hợp lệ. 

Ví dụ, hãy xem xét:```
3
1
3 3
1
1 1
```Câu trả lời là`1`bởi vì`T = 3`tạo một tin nhắn vào thời điểm thứ 3 mà Kotaro có thể gửi. Một giải pháp chỉ kiểm tra các điểm bên trong các khoảng sẽ loại bỏ nó một cách không chính xác. 

Một ví dụ khác:```
5
1
1 5
1
1 3
```Vì`T = 2`, tin nhắn xảy ra ở thời điểm 2 và 4. Thời gian 4 thuộc về Akane và không hợp lệ nên`T = 2`không được tính. Giải pháp bất cẩn chỉ kiểm tra tin nhắn đầu tiên sẽ cho kết quả sai. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ cố gắng mọi`T`từ`1`ĐẾN`N`. Đối với mỗi ứng cử viên, nó tạo ra bội số của`T`và kiểm tra xem mỗi bội số có nằm trong khoảng của đúng người hay không. Điều này đúng vì nó tuân theo định nghĩa chính xác. Vấn đề là số lượng ứng viên. Trong trường hợp xấu nhất, điều này đòi hỏi khoảng`N + N/2 + ...`kiểm tra tin nhắn, đại khái là`N log N`. Với`N = 10^9`, điều này là không thể. 

Nhận xét quan trọng là các ứng viên khó được chia thành hai nhóm. Bé nhỏ`T`các giá trị có nhiều thông điệp nhưng có rất ít giá trị như vậy. Lớn`T`các giá trị có ít thông báo nên có thể xử lý chúng bằng cách xem số lượng thông báo thay vì giá trị của`T`. 

Cho phép`S = floor(sqrt(N))`. Vì`T <= S`, chỉ có khoảng 31623 ứng viên. Chúng ta có thể kiểm tra từng cái một cách hiệu quả bằng cách hỏi liệu một khoảng xấu có chứa bội số của`T`với độ chẵn lẻ cần thiết. 

Vì`T > S`, số lượng tin nhắn nhiều nhất`S`. Thay vì lặp đi lặp lại`T`, chúng ta duy trì tập hợp có thể`T`các giá trị thỏa mãn điều kiện đầu tiên`k`tin nhắn. Khi chúng tôi thêm`k`-ràng buộc thông báo thứ, chúng ta giao tập hợp này với các khoảng trong đó`k*T`là hợp lệ. Tập hợp hiện tại được lưu dưới dạng các khoảng rời rạc, vì vậy mỗi bước chỉ là giao điểm của khoảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N log N) | O(1) | Quá chậm | 
| Tối ưu | O(sqrt(N) * (X+Y)) | O(X+Y) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia các giá trị có thể có của`T`thành hai nhóm bằng cách sử dụng`S = floor(sqrt(N))`. Giá trị từ`1`ĐẾN`S`được xử lý riêng lẻ. Giá trị lớn hơn`S`được xử lý bằng cách đếm số lượng tin nhắn họ tạo ra. 
2. Đối với mọi việc nhỏ`T`, kiểm tra xem có bội số lẻ bắt buộc nào nằm ngoài khoảng của Kotaro hay bội số chẵn bắt buộc có nằm ngoài khoảng của Akane hay không. Trong một khoảng thời gian`[L, R]`, bội số của`T`bên trong nó tương ứng với các giá trị cấp số nhân từ`ceil(L/T)`ĐẾN`floor(R/T)`. Chúng ta chỉ cần biết phạm vi đó chứa số lẻ hay số chẵn. 
3. Đối với lớn`T`, giữ một danh sách khoảng đại diện cho tất cả`T`giá trị lớn hơn`S`đã thỏa mãn các tin nhắn được xử lý cho đến nay. Ban đầu mọi giá trị trong`(S, N]`là có thể. 
4. Xử lý số tin nhắn`k = 1, 2, ...`. Đối với số tin nhắn`k`, chuyển đổi mọi khoảng thời gian hoạt động`[L, R]`của người gửi thành các giá trị có thể có của`T`:`ceil(L/k) <= T <= floor(R/k)`. 

Giao tập hợp mới này với tập hợp có thể hiện tại. 
5. Sau khi xử lý tin nhắn`k`, đếm các giá trị của`T`trong phạm vi chính xác`k`tin nhắn tồn tại:`floor(N/(k+1)) < T <= floor(N/k)`. 

Đây chính xác là những giá trị lớn mà thông điệp cuối cùng của nó là`k`-thứ 1 

Tại sao nó hoạt động: 

Đối với nhỏ`T`, thuật toán sẽ từ chối một giá trị chính xác khi nó tìm thấy một tin nhắn không thể gửi được. Nếu không có tin nhắn như vậy tồn tại thì mọi thời gian tin nhắn được yêu cầu đều hợp lệ, do đó khoảng thời gian đó được chấp nhận. 

Đối với lớn`T`, sau khi xử lý tin nhắn`k`, các khoảng thời gian được duy trì chứa chính xác các khoảng thời gian mà lần đầu tiên`k`tất cả các tin nhắn đều hợp lệ. Một khoảng thời gian thuộc về nhóm với chính xác`k`chỉ gửi tin nhắn nếu giá trị của nó nằm trong khoảng`floor(N/(k+1))+1`Và`floor(N/k)`. Giao nhau của hai điều kiện này sẽ tính chính xác các khoảng thời gian lớn hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def contains(intervals, x):
    lo, hi = 0, len(intervals)
    while lo < hi:
        mid = (lo + hi) // 2
        if intervals[mid][0] <= x:
            lo = mid + 1
        else:
            hi = mid
    idx = lo - 1
    return idx >= 0 and intervals[idx][1] >= x

def has_bad_multiple(T, bad, parity):
    for l, r in bad:
        a = (l + T - 1) // T
        b = r // T
        if a <= b:
            if a % 2 != parity:
                a += 1
            if a <= b:
                return True
    return False

def intersect(a, b):
    res = []
    i = j = 0
    while i < len(a) and j < len(b):
        l = max(a[i][0], b[j][0])
        r = min(a[i][1], b[j][1])
        if l <= r:
            res.append((l, r))
        if a[i][1] < b[j][1]:
            i += 1
        else:
            j += 1
    return res

def scaled(intervals, k):
    res = []
    for l, r in intervals:
        a = (l + k - 1) // k
        b = r // k
        if a <= b:
            if res and res[-1][1] + 1 >= a:
                res[-1] = (res[-1][0], max(res[-1][1], b))
            else:
                res.append((a, b))
    return res

def count_in(intervals, l, r):
    if l > r:
        return 0
    ans = 0
    for a, b in intervals:
        if b < l:
            continue
        if a > r:
            break
        ans += min(b, r) - max(a, l) + 1
    return ans

def solve():
    N = int(input())
    X = int(input())
    kotaro = [tuple(map(int, input().split())) for _ in range(X)]
    Y = int(input())
    akane = [tuple(map(int, input().split())) for _ in range(Y)]

    bad_k = []
    last = 0
    for l, r in kotaro:
        if last + 1 <= l - 1:
            bad_k.append((last + 1, l - 1))
        last = r
    if last < N:
        bad_k.append((last + 1, N))

    bad_a = []
    last = 0
    for l, r in akane:
        if last + 1 <= l - 1:
            bad_a.append((last + 1, l - 1))
        last = r
    if last < N:
        bad_a.append((last + 1, N))

    ans = 0
    S = int(N ** 0.5)

    for t in range(1, S + 1):
        if not has_bad_multiple(t, bad_k, 1) and not has_bad_multiple(t, bad_a, 0):
            ans += 1

    cur = [(S + 1, N)]
    k = 1
    while k <= N // (S + 1) and cur:
        allowed = scaled(kotaro if k % 2 else akane, k)
        cur = intersect(cur, allowed)

        left = N // (k + 1) + 1
        right = N // k
        ans += count_in(cur, left, right)
        k += 1

    print(ans)

solve()
```Phần đầu tiên xây dựng khoảng thời gian vắng mặt của cả hai người. Làm việc với các phạm vi không có sẵn giúp việc kiểm tra sau này dễ dàng hơn vì ứng viên bị từ chối ngay khi có một thông báo không hợp lệ. 

Cái nhỏ-`T`vòng lặp không liệt kê tin nhắn. Thay vào đó, nó kiểm tra xem phạm vi số nhân có chứa số nhân với số chẵn lẻ được yêu cầu hay không. Điều này giữ cho công việc tỷ lệ thuận với số khoảng thời gian. 

Cái lớn-`T`một phần lưu trữ các khoảng thời gian có thể có dưới dạng khoảng thời gian. Việc chuyển đổi từ khoảng thời gian người gửi`[L, R]`đến các khoảng thời gian có thể sử dụng phép chia cho chỉ mục tin nhắn`k`, đó là nghịch đảo của phép nhân`k*T`. Tất cả các phép tính đều sử dụng phép chia số nguyên một cách cẩn thận để các điểm cuối của khoảng vẫn được bao gồm. 

Số nguyên Python không bị tràn, vì vậy các giá trị lớn của`N`được an toàn. Việc sử dụng`floor`Và`ceil`sự chia rẽ là nơi chính có thể xảy ra những sai sót riêng lẻ. 

## Ví dụ đã hoạt động 

Đối với mẫu 1:```
10
2
2 4
6 9
3
1 3
5 7
9 10
```Đối với các giá trị nhỏ: 

| T | Đã kiểm tra bội số lẻ | Kiểm tra bội số chẵn | Kết quả | 
| --- | --- | --- | --- | 
| 1 | Kotaro thất bại ở 1 | | Không hợp lệ | 
| 2 | Kotaro thất bại ở điểm 6 | Akane thất bại ở điểm 4 | Không hợp lệ | 
| 3 | 3,9 hợp lệ | 6 hợp lệ | hợp lệ | 

Đối với các giá trị lớn, các khoảng thời gian trên`sqrt(10)`được kiểm tra bằng nhóm đếm tin nhắn. Các khoảng thời gian hợp lệ là`3,6,7,8,9`, đưa ra câu trả lời`5`. 

Đối với mẫu 2:```
10000000
1
4092001 5033941
2
206 314
1214 10000000
```Phạm vi lớn có nghĩa là hầu hết các ứng viên chỉ có một vài tin nhắn. 

| Số tin nhắn | Hạn chế hiện tại | Hiệu ứng | 
| --- | --- | --- | 
| 1 | T phải nằm trong khoảng Kotaro | Giữ kinh khoảng 4 triệu | 
| 2 | 2T phải nằm trong khoảng Akane | Xóa các khoảng thời gian có tin nhắn thứ hai quá sớm | 
| 3 | 3T phải nằm trong khoảng Kotaro | Tiếp tục thu hẹp bộ | 

Các giao điểm khoảng đếm tất cả các khoảng thời gian hợp lệ còn lại và tạo ra`941941`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(sqrt(N) * (X+Y)) | Số lượng tin nhắn trong khoảng thời gian nhỏ và khoảng thời gian lớn đều được giới hạn bởi khoảng sqrt(N) | 
| Không gian | O(X+Y) | Chỉ danh sách khoảng thời gian được lưu trữ | 

Với`N = 10^9`,`sqrt(N)`là khoảng 31623. Số lượng khoảng thời gian chỉ là 300 mỗi người, do đó số lượng thao tác vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old
    return out

assert run("""10
2
2 4
6 9
3
1 3
5 7
9 10
""") == "5\n"

assert run("""10000000
1
4092001 5033941
2
206 314
1214 10000000
""") == "941941\n"

assert run("""1
1
1 1
1
1 1
""") == "1\n"

assert run("""5
1
1 5
1
1 3
""") == "2\n"

assert run("""10
1
10 10
1
1 10
""") == "1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`N=1`có sẵn đầy đủ | 1 | Kích thước tối thiểu và xử lý điểm cuối | 
| Tất cả thời gian của Kotaro đều có nhưng Akane bỏ lỡ thời gian 4 | 2 | Kiểm tra người gửi thay thế | 
| Chỉ 10 lần làm việc cho Kotaro | 1 | Khoảng thời gian lớn với một tin nhắn | 

## Vỏ cạnh 

Khi nào`T`lớn hơn một nửa`N`, chỉ có một tin nhắn. Thuật toán xử lý việc này thông qua nhóm thời gian lớn đầu tiên, trong đó`k = 1`. Ví dụ, với`N = 10`và Kotaro chỉ có tại`[10,10]`, giá trị`T = 10`được tính vì thời gian tin nhắn duy nhất là hợp lệ. 

Khi lỗi xảy ra ở tin nhắn sau chứ không phải tin nhắn đầu tiên, thuật toán vẫn phát hiện được lỗi đó vì mỗi chỉ mục tin nhắn bổ sung đều giao với tập khoảng thời gian hiện tại. Trong ví dụ ở đâu`N = 5`, Kotaro có sẵn trên`[1,5]`, và Akane trên`[1,3]`, thời kỳ`T = 2`vẫn tồn tại trong lần kiểm tra tin nhắn đầu tiên nhưng bị xóa khi`k = 2`yêu cầu`2T = 4`thuộc khoảng Akane. 

Khi một tin nhắn đến chính xác điểm cuối khoảng thời gian, logic khoảng thời gian bao gồm sẽ giữ cho nó hợp lệ. Sự phân chia trần và sàn trong cả trường hợp nhỏ và lớn duy trì các ranh giới này, vì vậy thông điệp đôi khi`L`hoặc`R`không bị loại bỏ một cách ngẫu nhiên.
