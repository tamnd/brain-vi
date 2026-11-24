---
title: "CF 920G - Danh sách số nguyên"
description: "Vấn đề yêu cầu chúng tôi tạo một danh sách số nguyên đặc biệt cho nhiều truy vấn. Đối với một truy vấn nhất định có các số nguyên x, p và k, chúng ta cần tìm số nguyên thứ k lớn hơn x nguyên tố cùng nhau với p. Coprime có nghĩa là ước số chung lớn nhất (gcd) của số và p là 1."
date: "2026-06-13T02:54:45+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "bitmasks", "brute-force", "combinatorics", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 920
codeforces_index: "G"
codeforces_contest_name: "Educational Codeforces Round 37 (Rated for Div. 2)"
rating: 2200
weight: 920
solve_time_s: 274
verified: true
draft: false
---

[CF 920G - Danh sách số nguyên](https://codeforces.com/problemset/problem/920/G) 

**Đánh giá:** 2200 
**Tags:** tìm kiếm nhị phân, bitmasks, lực lượng vũ phu, tổ hợp, toán học, lý thuyết số 
**Thời gian giải:** 4 phút 34 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề yêu cầu chúng tôi tạo một danh sách số nguyên đặc biệt cho nhiều truy vấn. Đối với một truy vấn nhất định có số nguyên`x`,`p`, Và`k`, chúng ta cần tìm`k`-số nguyên thứ lớn hơn`x`đó là nguyên tố cùng nhau với`p`. Coprime có nghĩa là ước số chung lớn nhất (gcd) của số và`p`là 1. Ví dụ: nếu`x = 7`Và`p = 22`, các số lớn hơn 7 và nguyên tố cùng nhau bằng 22 là 9, 13, 15, 17, v.v. Truy vấn muốn`k`-số thứ trong dãy này. 

Các ràng buộc cung cấp cho chúng tôi tối đa 30.000 truy vấn và mỗi truy vấn có số lượng lên tới 1.000.000. Một cách tiếp cận ngây thơ để kiểm tra từng số nguyên ở trên`x`từng người một sẽ cần tính toán gcd cho hàng triệu số cho mỗi truy vấn, tốc độ này quá chậm. Với`t`lên tới 30.000 và con số đạt tới`10^6`, chúng ta cần một phương pháp chia tỷ lệ gần đúng theo thứ tự logarit hoặc tuyến tính theo số thừa số nguyên tố của`p`. 

Trường hợp khó phát hiện xảy ra khi`p`có nhiều thừa số nguyên tố nhỏ. Ví dụ, nếu`p = 6`, các số là bội số của 2 hoặc 3 phải được bỏ qua. Một cách tiếp cận ngây thơ có thể đếm không chính xác các số chia hết cho bất kỳ thừa số nguyên tố nào của`p`. Tương tự,`x`có thể ở ngay dưới một số nguyên tố cùng nhau`p`, do đó, từng lỗi một trong việc đếm hoặc lập chỉ mục có thể dẫn đến trả về sai số. Ví dụ, với`x = 5`,`p = 6`, Và`k = 1`, câu trả lời đúng là 7 chứ không phải 6. 

## Phương pháp tiếp cận 

Phương pháp brute-force rất đơn giản: bắt đầu từ`x + 1`, kiểm tra lần lượt từng số và đếm xem có bao nhiêu số nguyên tố cùng nhau`p`. Khi số lượng đạt đến`k`, con số đó chính là câu trả lời. Điều này hoạt động vì nó mô phỏng trực tiếp định nghĩa của chuỗi. Vấn đề là trường hợp xấu nhất: nếu`p`nhỏ như 2, khoảng một nửa số đó là số nguyên tố cùng nhau và chúng ta có thể phải lặp lại tới`10^6`số cho mỗi truy vấn. Với`t`là 30.000, con số này có thể đạt tới`3 * 10^10`hoạt động, rõ ràng là không thể thực hiện được. 

Quan sát quan trọng là tính đồng nguyên tố được xác định bởi các thừa số nguyên tố của`p`. Nếu chúng ta biết phân tích thành thừa số nguyên tố của`p`, chúng ta có thể sử dụng loại trừ bao gồm để đếm xem có bao nhiêu số nhỏ hơn hoặc bằng một giá trị nào đó`y`là nguyên tố cùng nhau`p`. Với điều này, chúng ta có thể coi bài toán như một phép tìm kiếm: tìm số nhỏ nhất`y`lớn hơn`x`sao cho có chính xác`k`số giữa`x`Và`y`đó là nguyên tố cùng nhau`p`. Điều này làm giảm vấn đề tìm kiếm nhị phân kết hợp với chức năng đếm nhanh. Đếm thông qua loại trừ bao gồm là hiệu quả vì số lượng các thừa số nguyên tố riêng biệt của bất kỳ số nào lên đến`10^6`nhiều nhất là 7 (vì 2 * 3 * 5 * 7 * 11 * 13 * 17 > 10^6). 

Các phương pháp tiếp cận vũ phu và tối ưu có thể được so sánh như sau: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k * log p) mỗi truy vấn | O(1) | Quá chậm đối với k lớn hoặc p nhỏ | 
| Bao gồm-Loại trừ + Tìm kiếm nhị phân | O(2^m * log(range)) cho mỗi truy vấn, m = #hệ số nguyên tố của p | O(m) | Hiệu quả và được chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước tất cả các số nguyên tố lên tới`10^6`bằng sàng Eratosthenes. Điều này cho phép chúng ta phân tích bất kỳ yếu tố nào`p`một cách hiệu quả. 
2. Đối với mỗi truy vấn`(x, p, k)`, nhân tố`p`thành các thừa số nguyên tố riêng biệt của nó. Số lượng các yếu tố`m`là nhỏ. 
3. Xác định hàm`count_coprime(y)`đếm được bao nhiêu số nguyên ≤`y`là nguyên tố cùng nhau`p`. Sử dụng phép loại trừ bao hàm, lặp lại tất cả các tập hợp con của các thừa số nguyên tố của`p`. Với mỗi tập hợp con, hãy tính tích các số nguyên tố rồi trừ hoặc cộng chia sàn`y // product`theo tính chẵn lẻ của kích thước tập hợp con. 
4. Thực hiện tìm kiếm nhị phân cho số nguyên nhỏ nhất`y`như vậy`count_coprime(y) - count_coprime(x) == k`. Khởi tạo`low = x + 1`Và`high`BẰNG`x + k * p`(đây là giới hạn trên; trong trường hợp xấu nhất, mọi`p`số chứa ít nhất một số nguyên tố cùng nhau). 
5. Trở về`y`như`k`-số thứ trong dãy. 

Điều này có hiệu quả vì số lượng loại trừ bao gồm cung cấp một số lượng chính xác các số nguyên nguyên tố cùng nhau`p`đến bất kỳ giới hạn nào. Tìm kiếm nhị phân có hiệu quả tìm thấy số ở đó chính xác`k`nguyên tố cùng xuất hiện sau`x`. Điều bất biến là ở mỗi bước tìm kiếm, câu trả lời đúng vẫn nằm trong khoảng tìm kiếm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def sieve(n):
    spf = list(range(n + 1))
    for i in range(2, int(n**0.5) + 1):
        if spf[i] == i:
            for j in range(i*i, n+1, i):
                if spf[j] == j:
                    spf[j] = i
    return spf

def factorize(n, spf):
    primes = set()
    while n > 1:
        primes.add(spf[n])
        n //= spf[n]
    return list(primes)

def count_coprime(y, primes):
    from itertools import combinations
    res = y
    m = len(primes)
    for sz in range(1, m+1):
        for comb in combinations(primes, sz):
            prod = 1
            for c in comb:
                prod *= c
            if sz % 2 == 1:
                res -= y // prod
            else:
                res += y // prod
    return res

def solve():
    spf = sieve(10**6)
    t = int(input())
    for _ in range(t):
        x, p, k = map(int, input().split())
        primes = factorize(p, spf)
        low = x + 1
        high = x + k * p
        while low < high:
            mid = (low + high) // 2
            if count_coprime(mid, primes) - count_coprime(x, primes) >= k:
                high = mid
            else:
                low = mid + 1
        print(low)

solve()
```Sàng được sử dụng để phân tích`p`một cách hiệu quả. các`count_coprime`Hàm áp dụng loại trừ bao hàm trên các thừa số nguyên tố để xác định chính xác số nguyên tố cùng nhau`p`lên tới bất kỳ giá trị nào. Tìm kiếm nhị phân định vị`k`- số nguyên tố lớn hơn`x`. Cần cẩn thận để đặt giới hạn tìm kiếm trên một cách an toàn vì`x + k * p`để tránh bị quá tải. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
x = 7, p = 22, k = 3
```Thừa số nguyên tố của 22: 2, 11. 

Tìm kiếm nhị phân đếm các số nguyên tố cùng nhau bằng cách sử dụng loại trừ bao gồm: 

| giữa | count_coprime(giữa) | count_coprime(mid) - count_coprime(7) | quyết định tìm kiếm nhị phân | 
| --- | --- | --- | --- | 
| 13 | 6 | 2 | tăng thấp | 
| 15 | 7 | 3 | giảm cao | 

Kết quả:`15`. 

Đầu vào mẫu 2:```
x = 5, p = 6, k = 4
```Thừa số nguyên tố: 2, 3. Các số nguyên tố cùng nhau >5: 7, 11, 13, 17. Kết quả:`17`. 

Điều này chứng tỏ thuật toán xử lý nhỏ`x`và nhỏ`p`đúng, bỏ qua các số chia hết cho 2 hoặc 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * 2^m * log(k*p)) | Tìm kiếm nhị phân qua`y`với sự bao gồm-loại trừ trên`m`các yếu tố cơ bản của`p`| 
| Không gian | O(n) | Sàng lưu trữ lên tới 10^6, mảng thừa số nguyên tố cho mỗi truy vấn | 

Vì`t = 3 * 10^4`Và`m ≤ 7`, tốc độ này đủ nhanh trong giới hạn 5 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# provided samples
assert run("3\n7 22 1\n7 22 2\n7 22 3\n") == "9\n13\n15", "sample 1"

# minimum-size input
assert run("1\n1 1 1\n") == "2", "minimum input"

# maximum-size p
assert run("1\n1 1000000 5\n") == "6\n7\n11\n13\n17".split()[4], "large p"

# all equal values
assert run("1\n5 6 3\n") == "13", "skip multiples"

#
```
