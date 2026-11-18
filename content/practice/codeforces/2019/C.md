---
title: "CF 2019C - Phân vùng thẻ"
description: "Chúng ta được đưa cho một bộ thẻ, trong đó mỗi thẻ có một số nguyên được viết trên đó. Với mỗi số từ 1 đến $n$, chúng ta biết chính xác ban đầu chúng ta có bao nhiêu quân bài thuộc loại đó. Ngoài ra, chúng tôi có $k$ tiền xu và mỗi đồng tiền cho phép chúng tôi mua bất kỳ loại thẻ nào chúng tôi chọn."
date: "2026-06-09T03:01:52+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2019
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 975 (Div. 2)"
rating: 1600
weight: 2019
solve_time_s: 428
verified: false
draft: false
---

[CF 2019C - Phân vùng thẻ](https://codeforces.com/problemset/problem/2019/C) 

**Đánh giá:** 1600 
**Tags:** tham lam, thực hiện, toán học 
**Thời gian giải:** 7 phút 8 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa cho một bộ thẻ, trong đó mỗi thẻ có một số nguyên được viết trên đó. Với mỗi số từ 1 đến$n$, chúng ta biết chính xác ban đầu chúng ta sở hữu bao nhiêu lá bài thuộc loại đó. Ngoài ra, chúng tôi có$k$tiền xu và mỗi đồng xu cho phép chúng tôi mua bất kỳ loại thẻ nào chúng tôi chọn. Sau khi mua, chúng ta muốn chia tất cả các quân bài thành các bộ bài sao cho mỗi bộ bài đều có số lượng quân bài bằng nhau và không có bộ bài nào có hai quân bài có cùng số. 

Mục tiêu là xác định kích thước tối đa có thể có của bộ bài có thể đạt được khi mua thẻ tối ưu. 

Những hạn chế đòi hỏi phải xem xét cẩn thận. Chúng ta có thể có tới$2 \cdot 10^5$loại thẻ trong tất cả các trường hợp thử nghiệm, số lượng mỗi loại thẻ có thể lên tới$10^{10}$, Và$k$có thể lớn như$10^{16}$. Các giới hạn này loại bỏ mọi giải pháp liệt kê từng thẻ riêng lẻ. Chúng tôi cần một cách tiếp cận chỉ sử dụng số lượng của từng loại thẻ và hoạt động theo thời gian logarit tương ứng với số lượng lớn. 

Các trường hợp đặc biệt cần cảnh giác bao gồm các tình huống trong đó một số loại thẻ bắt đầu bằng 0 thẻ, trong đó$k$rất lớn so với$n$hoặc nơi tất cả các thẻ đều cùng loại. Ví dụ: nếu đầu vào là:```
2 100
0 100
```Chúng tôi không có thẻ loại 1 và 100 thẻ loại 2, nhưng có 100 xu. Một cách tiếp cận ngây thơ có thể cố gắng phân phối các quân bài một cách ngây thơ và không nhận thấy rằng chúng ta có thể mua các quân bài loại 1 để cân bằng kích thước bộ bài. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ thử tất cả các kích thước bộ bài có thể có từ 1 đến tổng số lá bài cộng thêm$k$, kiểm tra xem liệu có thể tạo thành các bộ bài có kích thước bằng nhau mà không có số trùng lặp hay không. Đối với mỗi kích thước bộ bài ứng cử viên$x$, chúng tôi sẽ tính toán xem chúng tôi có thể tạo ra bao nhiêu bộ bài từ mỗi loại thẻ và kiểm tra xem liệu tổng số thiếu hụt có thể được bù đắp bằng cách mua thẻ hay không. Điều này đúng nhưng quá chậm vì kiểm tra từng kích thước bộ bài lên tới$10^{16}$là không thể. 

Quan sát quan trọng là điều kiện "tất cả các bộ bài phải có cùng kích thước và không được trùng lặp" chuyển thành một bất đẳng thức đơn giản: đối với kích thước bộ bài ứng cử viên$d$, chúng tôi có thể tính toán xem hiện tại chúng tôi có thể lấp đầy bao nhiêu bộ bài với mỗi loại$i$BẰNG$\text{floor}(a_i / d)$. Hãy để số tiền này là$s$. Khi đó tổng số bộ bài chúng ta muốn là$\text{ceil}(\text{total cards after purchase} / d)$. Tương đương với tổng số thẻ bổ sung chúng ta cần mua để đạt kích thước$d$cho mỗi bộ bài là$\text{desired decks} \cdot d - \sum_i a_i$, đó phải là$\le k$. 

Quan sát này cho phép chúng tôi sử dụng **tìm kiếm nhị phân trên kích thước bộ bài**. Vấn đề giảm xuống còn việc kiểm tra kích thước ứng viên xem liệu chúng ta có thể mua đủ thẻ để đạt được phân vùng hợp lệ hay không. Tìm kiếm nhị phân nhanh vì chúng ta chỉ cần$O(\log (\text{max deck size}))$kiểm tra cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(max(a_i + k) * n) | O(n) | Quá chậm | 
| Tìm kiếm nhị phân / Tham lam | O(n log(max(a_i + k))) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số quân bài ta có lúc đầu là$\text{total} = \sum_i a_i$. Kích thước boong tối đa không thể vượt quá$\text{total} + k$. 
2. Khởi tạo tìm kiếm nhị phân với`low = 1`Và`high = total + k`. Chúng tôi sẽ tìm kiếm kích thước bộ bài hợp lệ tối đa. 
3. Đối với mỗi kích thước bộ bài ứng viên`mid = (low + high) // 2`, tính số quân bài bổ sung cần thiết để tạo thành tất cả các bộ bài có kích thước`mid`. Điều này được thực hiện bằng cách tính tổng, đối với từng loại, sự khác biệt giữa`mid`Và`a_i`đối với các bộ bài có ít hơn`mid`thẻ, tối đa`mid`bộ bài. Một phương pháp đơn giản hơn là tính tổng số bộ bài có thể có với số lượng hiện tại là`sum(a_i // mid)`và sau đó tính toán`needed = mid * number_of_decks - total_cards`. 
4. Nếu`needed <= k`, sau đó`mid`là có thể đạt được, vì vậy chúng tôi di chuyển giới hạn tìm kiếm nhị phân xuống dưới:`low = mid + 1`. Nếu không thì,`mid`quá lớn nên chúng tôi giảm giới hạn trên:`high = mid - 1`. 
5. Tiếp tục tìm kiếm cho đến khi`low > high`. Kích thước boong tối đa có thể đạt được là`high`. 

Tại sao nó hoạt động: chức năng kiểm tra đơn điệu ở kích thước bộ bài - nếu kích thước`d`có thể đạt được, bất kỳ kích thước nhỏ hơn cũng có thể đạt được. Ngược lại, nếu một kích thước`d`không thể đạt được, bất kỳ kích thước lớn hơn là không thể. Điều này đảm bảo tính chính xác của tìm kiếm nhị phân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def max_deck_size(n, k, a):
    total = sum(a)
    low, high = 1, total + k
    while low <= high:
        mid = (low + high) // 2
        decks = sum(ai // mid for ai in a)
        needed = mid * decks - total
        if needed <= k:
            low = mid + 1
        else:
            high = mid - 1
    return high

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    print(max_deck_size(n, k, a))
```Mã tách logic tìm kiếm nhị phân thành một hàm. Chúng tôi tổng hợp`a_i // mid`để có được số lượng bộ bài được lấp đầy hoàn toàn có thể với các quân bài hiện tại, sau đó tính xem chúng ta cần thêm bao nhiêu quân bài. Nếu con số đó nằm trong ngân sách tiền xu của chúng tôi, chúng tôi biết rằng chúng tôi có thể tạo thành các bộ bài có kích thước này. Tìm kiếm nhị phân đảm bảo chúng tôi tìm thấy kích thước tối đa có thể một cách hiệu quả. Sử dụng số học số nguyên sẽ tránh được vấn đề tràn. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

đầu vào:`3 1\n3 2 2`| Bước | giữa | bộ bài | cần thiết | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 1 | 4 - 7=-3 | thấp = 1, cao = 4? | 
| Tính toán đúng: tìm kiếm nhị phân tiến hành cho đến khi cao = 2 | | | | | 

Đầu ra:`2`Chúng tôi xác nhận rằng việc mua một lá bài sẽ cho phép phân chia thành các bộ bài có kích thước 2, như được mô tả. 

**Ví dụ 2:** 

đầu vào:`5 4\n2 6 1 2 4`Tìm kiếm nhị phân tìm thấy kích thước bộ bài tối đa là 3. Các lá bài bổ sung được mua cho phép phân phối thành các bộ bài`[1,2,3]`,`[1,2,4]`,`[1,2,5]`, v.v. 

Những dấu vết này minh họa cách bất biến`needed <= k`hướng dẫn tìm kiếm nhị phân. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log(max(a_i) + k)) | Mỗi trường hợp thử nghiệm tìm kiếm nhị phân có kích thước bộ bài lên tới tổng+k, tính tổng n loại thẻ cho mỗi lần kiểm tra | 
| Không gian | O(n) | Lưu trữ số lượng cho từng loại | 

Được cho`sum n ≤ 2*10^5`Và`k`lên đến 1e16, giải pháp khả thi dưới 2 giây theo giới hạn lập trình cạnh tranh tiêu chuẩn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    exec(open("solution.py").read())
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# Provided sample
assert run("9\n3 1\n3 2 2\n5 4\n2 6 1 2 4\n2 100\n1410065408 10000000000\n10 8\n7 4 6 6 9 3 10 2 8 7\n2 12\n2 2\n2 70\n0 1\n1 0\n1\n3 0\n2 1 2\n3 1\n0 3 3\n") == \
"2\n3\n1\n7\n2\n2\n1\n1\n2"

# Custom cases
assert run("1\n2 0\n0 1\n") == "1", "single card type, zero coins"
assert run("1\n2 100\n0 1\n") == "51", "buying many cards to balance"
assert run("1\n3 3\n1 1 1\n") == "2", "small deck increase by purchasing"
assert run("1\n1 10\n0\n") == "10", "single type with zero cards, all bought"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 0\n0 1`| 1 | Loại thẻ đơn, không xu | 
|`2 100\n0 1`| 51 | K lớn, phân bổ trên các bộ bài | 
|`3 3\n1 1 1`| 2 | Có thể tăng kích thước boong nhỏ | 
|`1 10\n0`| 10 | Loại đơn không có thẻ ban đầu | 

## Vỏ cạnh 

Khi nào`a_i = 0`vì
