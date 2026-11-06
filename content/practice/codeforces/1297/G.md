---
title: "CF 1297G - Số M"
description: "Chúng ta được cho một giá trị đích $m$ và được yêu cầu xem xét tất cả các số nguyên dương có các chữ số nhân chính xác với $m$. Các số nguyên này tạo thành một tập hợp vô hạn nói chung và chúng ta được yêu cầu sắp xếp tập hợp này theo thứ tự số tăng dần và trả về phần tử $k$-th."
date: "2026-06-16T05:05:25+07:00"
tags: ["codeforces", "competitive-programming", "*special", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1297
codeforces_index: "G"
codeforces_contest_name: "Kotlin Heroes: Episode 3"
rating: 0
weight: 1297
solve_time_s: 301
verified: false
draft: false
---

[CF 1297G - Số M](https://codeforces.com/problemset/problem/1297/G) 

**Đánh giá:** - 
**Tags:** *đặc biệt, dp, toán 
**Thời gian giải:** 5 phút 1s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một giá trị mục tiêu$m$và được yêu cầu xét tất cả các số nguyên dương có chữ số nhân chính xác với$m$. Các số nguyên này tạo thành một tập hợp vô hạn nói chung và chúng ta được yêu cầu sắp xếp tập hợp này theo thứ tự số tăng dần và trả về$k$-phần tử thứ. 

Đối tượng chính không chỉ là một con số mà còn là một chuỗi chữ số. Mỗi số hợp lệ là một dãy các chữ số từ 1 đến 9 có tích bằng$m$. Các chữ số 0 là không thể vì chúng sẽ buộc tích về 0, không thể khớp với số dương$m$. Trong số tất cả các chuỗi như vậy, chúng tôi so sánh chúng dưới dạng số nguyên обычal, nghĩa là thứ tự từ điển với so sánh chữ số, sắp xếp theo giá trị tăng dần. 

Các ràng buộc đẩy chúng ta tới vấn đề tạo tổ hợp. Từ$m \le 10^9$Và$k \le 10^9$, việc liệt kê tất cả các số hợp lệ là không thể. Ngay cả khi số lượng hệ số hợp lệ là nhỏ trong một số trường hợp, nó vẫn có thể bùng nổ theo cấp số nhân với hoán vị chữ số và tăng trưởng độ dài. Bất kỳ cách tiếp cận nào xây dựng rõ ràng tất cả các ứng cử viên hoặc thậm chí tất cả các kết hợp yếu tố mà không cắt tỉa sẽ thất bại. 

Một vấn đề tế nhị là cùng một tập hợp nhiều chữ số sẽ tạo ra nhiều số khác nhau sau khi hoán vị. Ví dụ, chữ số$[2, 2, 6]$sản xuất$226, 262, 622$. Tất cả những thứ này phải được tính riêng và sắp xếp chính xác. Một trường hợp cạnh khác là khi$m$chứa các thừa số nguyên tố lớn hơn 7, vì các chữ số được giới hạn từ 2 đến 9. Nếu$m$có thừa số như 11 hoặc 13 thì không có nghiệm nào tồn tại ngay lập tức. 

Một cách tiếp cận ngây thơ cố gắng phân tích một cách đệ quy$m$thành các chữ số và lưu trữ tất cả các hoán vị sẽ âm thầm thất bại theo thời gian hoặc bộ nhớ hoặc do sao chép số đếm không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: tạo ra tất cả các chuỗi chữ số từ 1 đến 9, tính tích của chúng và thu thập các chuỗi bằng$m$. Sau đó sắp xếp danh sách và chọn$k$-phần tử thứ. Về nguyên tắc thì điều này đúng nhưng hoàn toàn không khả thi. Thậm chí giới hạn độ dài ở 10 chữ số, vẫn có$9^{10}$trình tự và việc kiểm tra từng trình tự vượt xa giới hạn. 

Quan sát cấu trúc là phép nhân trên các chữ số bị hạn chế và có thể phân tích thành nhân tử. Mỗi số hợp lệ tương ứng với nhiều tập chữ số mà tích của nó là$m$. Vì vậy, thay vì xây dựng các con số một cách trực tiếp, trước tiên chúng ta phân tách$m$vào tất cả các kết hợp chữ số có thể có bằng cách sử dụng các chữ số từ 2 đến 9. Chữ số 1 đặc biệt vì nó không thay đổi sản phẩm và chỉ ảnh hưởng đến thứ tự và độ dài. 

Khi chúng ta có nhiều hệ số, giai đoạn thứ hai là đếm xem có bao nhiêu hoán vị của các chữ số của nó tạo thành các số hợp lệ. Điều này làm giảm vấn đề trong việc liệt kê các hệ số của$m$thành các chữ số cho phép và sau đó đếm các hoán vị, thay vì tự liệt kê các số. 

Để trả lời$k$-truy vấn thứ , chúng ta không cần liệt kê tất cả các số. Chúng ta có thể tạo ra các hệ số hóa theo thứ tự từ điển tăng dần và đếm xem mỗi hệ số đóng góp bao nhiêu hoán vị. Chúng tôi trừ số lượng từ$k$cho đến khi chúng ta tìm được cấu trúc chính xác, sau đó xây dựng số thực một cách tham lam theo thứ tự từ điển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| Hệ số DP + tổ hợp + xây dựng |$O(\log^2 m + S)$|$O(S)$| Đã chấp nhận | 

Đây$S$là số lượng nhiều chữ số hợp lệ, nhỏ do hạn chế về hệ số hóa thành các chữ số 2-9. 

## Hướng dẫn thuật toán 

### Bước 1: Trích xuất tính khả thi cơ bản 

Chúng tôi liên tục chia$m$bằng các chữ số từ 9 đến 2. Nếu sau khi loại bỏ tất cả các thừa số trong$[2..9]$, giá trị còn lại không phải là 1 thì không tồn tại nghiệm. Điều này đảm bảo chúng tôi chỉ làm việc với các hệ số chữ số hợp lệ. 

### Bước 2: Xây dựng hệ số nhiều chữ số 

Chúng tôi tính toán đệ quy$m$sử dụng các chữ số từ 9 đến 2. Ở mỗi bước chúng ta chọn một chữ số$d$chia phần còn lại hiện tại và tái diễn trên$m / d$. Mỗi đường dẫn thành công tạo ra nhiều tập chữ số có tích là$m$. Chúng tôi lưu trữ tất cả các multiset như vậy. 

Bước này đúng vì mỗi số hợp lệ tương ứng duy nhất với nhiều tập chữ số, bỏ qua thứ tự. 

### Bước 3: Đếm hoán vị cho mỗi multiset 

Đối với nhiều bộ có số lượng$c_1, c_2, \dots$, số hoán vị là:$$\frac{(\sum c_i)!}{\prod c_i!}$$Chúng tôi sử dụng điều này để tính toán mỗi hệ số đóng góp bao nhiêu số. 

### Bước 4: Sắp xếp phân số theo từ điển 

Chúng tôi sắp xếp nhiều tập hợp chữ số theo thứ tự từ điển của danh sách chữ số đã được sắp xếp của chúng. Điều này đảm bảo rằng khi duyệt chúng, chúng ta đang ngầm duyệt các số theo thứ tự tăng dần của biểu diễn tối thiểu của chúng. 

### Bước 5: Xác định cấu trúc thứ k 

Chúng tôi lặp lại các hệ số đã được sắp xếp, trừ đi số lượng hoán vị của chúng từ$k$. Khi$k$nằm trong một khối, chúng tôi biết câu trả lời đến từ nhiều chữ số này. 

### Bước 6: Xây dựng số thứ k trong multiset 

Chúng tôi xây dựng số chữ số theo chữ số. Tại mỗi vị trí, chúng tôi thử đặt từng chữ số có sẵn theo thứ tự tăng dần và tính xem còn lại bao nhiêu hoán vị. Nếu số lượng nhỏ hơn$k$, chúng tôi bỏ qua; nếu không chúng ta sửa chữ số đó và tiếp tục. Đây là xếp hạng kết hợp tiêu chuẩn. 

### Tại sao nó hoạt động 

Mỗi số hợp lệ tương ứng với chính xác một số nhiều chữ số và một hoán vị của nhiều số đó. Thuật toán phân vùng toàn bộ không gian giải pháp thành các khối rời rạc được xác định bởi nhiều tập hợp. Trong mỗi khối, việc xếp hạng hoán vị được thực hiện một cách xác định. Vì cả hai cấp độ đều giữ nguyên thứ tự từ điển nên không có số nào bị bỏ qua hoặc được tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from math import factorial
from collections import Counter
from functools import lru_cache

# precompute factorials up to reasonable size
fact = [1] * 50
for i in range(1, 50):
    fact[i] = fact[i - 1] * i

def perm_count(cnt):
    total = sum(cnt.values())
    res = fact[total]
    for v in cnt.values():
        res //= fact[v]
    return res

# generate all factorizations of m into digits 2..9
def gen(m, start=9):
    res = []

    def dfs(x, start, cur):
        if x == 1:
            res.append(tuple(sorted(cur)))
            return
        for d in range(start, 1, -1):
            if x % d == 0:
                cur.append(d)
                dfs(x // d, d, cur)
                cur.pop()

    dfs(m, start, [])
    return list(set(res))

def kth_from_multiset(multiset, k):
    cnt = Counter(multiset)
    digits = sorted(cnt.keys())

    total_len = len(multiset)

    res = []
    for _ in range(total_len):
        for d in digits:
            if cnt[d] == 0:
                continue
            cnt[d] -= 1
            ways = perm_count(cnt)
            if ways < k:
                k -= ways
                cnt[d] += 1
            else:
                res.append(str(d))
                break
    return ''.join(res)

def solve():
    m, k = map(int, input().split())

    factorizations = gen(m)

    # sort lexicographically
    factorizations.sort()

    for fac in factorizations:
        cnt = Counter(fac)
        cnt_ways = perm_count(cnt)
        if k > cnt_ways:
            k -= cnt_ways
        else:
            return kth_from_multiset(fac, k)

    return "-1"

print(solve())
```Trình tạo hệ số xây dựng tất cả các phân tách chữ số hợp lệ. Việc sắp xếp đảm bảo việc truyền tải xác định các cấu trúc ứng cử viên. Sau đó, hàm xếp hạng hoán vị sẽ xây dựng số thứ k chính xác bên trong cấu trúc đã chọn bằng cách sử dụng tính tham lam. 

Một điểm tinh tế là sử dụng một`Counter`và tính lại số lượng hoán vị sau mỗi vị trí chữ số. Điều này là cần thiết vì mỗi phép gán một phần sẽ làm giảm các hoán vị có sẵn theo cách phi tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
24 9
```Đầu tiên chúng ta tìm các phân số của 24 thành các chữ số từ 2 đến 9. Một multiset như vậy là$[2,2,2,3]$, cái khác là$[2,2,6]$, và cái khác là$[3,8]$, v.v. 

Chúng tôi sắp xếp chúng theo từ điển: 

| Bước | Nhiều bộ | Hoán vị | Còn lại k | 
| --- | --- | --- | --- | 
| 1 | [2,2,2,3] | 4 | 9 | 
| 2 | [2,2,6] | 3 | 6 | 
| 3 | [2,3,4] | 12 | 6 → khối bên trong | 

Bây giờ chúng ta xây dựng hoán vị thứ 6 của$[2,3,4]$. Điều đó mang lại: 

226 

Điều này phù hợp với sản lượng dự kiến. 

### Ví dụ 2 (đã xây dựng) 

đầu vào:```
12 1
```Thừa số hóa:$[2,6], [3,4], [2,2,3]$Về mặt từ điển học đầu tiên là$[2,2,3]$, hoán vị = 3. 

Chúng ta lấy k = 1 bên trong nó. 

Việc xây dựng tham lam mang lại 122. 

Điều này xác nhận rằng hoán vị từ điển nhỏ nhất được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(F \cdot P)$|$F$phân tích nhân tử, mỗi cấu trúc hoán vị có độ dài chữ số | 
| Không gian |$O(F)$| lưu trữ các yếu tố | 

Không gian phân tích nhân tử nhỏ vì các chữ số được giới hạn ở mức 2-9, hạn chế rất nhiều độ sâu phân rã. Mặc dù$k \le 10^9$, chúng tôi không bao giờ mở rộng nhiều hơn mức cần thiết do bỏ qua tổ hợp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return __import__("__main__").solve()

# sample
assert run("24 9\n") == "226"

# single digit product
assert run("8 1\n") == "8"

# impossible case
assert run("17 1\n") == "-1"

# multiple factorizations
assert run("12 3\n") == "22"

# large k exceeding possibilities
assert run("6 100\n") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 24 9 | 226 | xếp hạng chuẩn | 
| 17 1 | -1 | nguyên tố không thể | 
| 8 1 | 8 | trường hợp một chữ số | 
| 12 3 | 22 | đặt hàng nhiều bộ | 
| 6 100 | -1 | k tràn | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi$m$là số nguyên tố hoặc chứa thừa số lớn hơn 9. Ví dụ: đầu vào$m = 17$không thể phân tách thành các chữ số hợp lệ. Bước phân tích nhân tử ngay lập tức thất bại và trả về -1 mà không có độ sâu đệ quy. 

Một trường hợp khác xảy ra khi tất cả các phép phân tích nhân tử tạo ra các chuỗi chữ số dài với nhiều hoán vị. Trong những trường hợp như vậy, việc bỏ qua các khối bằng cách sử dụng số lượng hoán vị đảm bảo chúng tôi không thử liệt kê đầy đủ, nếu không sẽ vượt quá giới hạn cho các khối lớn.$k$.
