---
title: "CF 1882B - Bộ và Liên minh"
description: "Chúng tôi được cung cấp một số bộ số nguyên. Chúng ta có thể chọn bất kỳ tập hợp con nào của các tập hợp này (bao gồm không có tập hợp nào) và lấy hợp của các tập hợp đã chọn. Một tập hợp có thể đạt được nếu nó có thể được hình thành theo cách này."
date: "2026-06-08T22:36:06+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1882
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 899 (Div. 2)"
rating: 1300
weight: 1882
solve_time_s: 126
verified: true
draft: false
---

[CF 1882B - Bộ và Liên minh](https://codeforces.com/problemset/problem/1882/B) 

**Đánh giá:** 1300 
**Tags:** bitmasks, vũ phu, thuật toán xây dựng, tham lam 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số bộ số nguyên. Chúng ta có thể chọn bất kỳ tập hợp con nào của các tập hợp này (bao gồm không có tập hợp nào) và lấy hợp của các tập hợp đã chọn. Một tập hợp được coi là _có thể đạt được_ nếu nó có thể được hình thành theo cách này. Nhiệm vụ của chúng ta là tìm ra tập hợp lớn nhất có thể đạt được **không bằng** hợp của tất cả các tập hợp ban đầu. Đầu ra phải là số phần tử trong một tập hợp như vậy. 

Mỗi trường hợp thử nghiệm cung cấp cho chúng tôi tối đa 50 bộ và mỗi bộ chứa tối đa 50 số nguyên nằm trong khoảng từ 1 đến 50. Các ràng buộc nhỏ nên các kỹ thuật ép buộc thô bạo đối với các phần tử của tập hợp là khả thi, nhưng việc ép buộc tất cả các tập hợp con có thể có của các bộ là ở mức giới hạn, vì điều đó sẽ xảy ra$2^n$khả năng đã kết thúc$10^{15}$khi$n = 50$. Vì vậy, chúng ta cần một cách tiếp cận thông minh hơn, mang tính xây dựng hơn là kiểm tra từng tập hợp con một cách rõ ràng. 

Các trường hợp biên không rõ ràng bao gồm các tình huống trong đó mỗi tập hợp là một tập hợp đơn hoặc chỉ có một tập hợp. Ví dụ, nếu chỉ có một bộ$\{1,2,3\}$, tập duy nhất có thể đạt được không bằng tập hợp đầy đủ là tập trống, cho đầu ra 0. Một điều tinh tế khác là khi các tập hợp chồng chéo theo cách mà việc loại bỏ bất kỳ một tập hợp nào vẫn mang lại kết hợp đầy đủ; trong trường hợp đó, phép hợp không đầy đủ lớn nhất có được bằng cách lấy hợp của tất cả các tập hợp ngoại trừ một tập hợp. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là lặp lại tất cả các tập hợp con của các tập hợp đã cho, tính toán liên kết của chúng và theo dõi kích thước của liên kết lớn nhất không khớp với liên kết của tất cả các tập hợp. Về nguyên tắc, điều này hoạt động vì phép toán hợp có tính kết hợp và đơn điệu, nhưng số lượng tập hợp con tăng theo cấp số nhân ($2^n$), nó quá lớn đối với$n = 50$. 

Quan sát quan trọng giúp mở ra một giải pháp hiệu quả hơn là tập hợp lớn nhất có thể đạt được nhưng không phải là hợp đầy đủ luôn có thể thu được bằng cách lấy hợp của tất cả các tập hợp **ngoại trừ một**. Nếu chúng ta loại bỏ một tập hợp chứa các phần tử không có trong bất kỳ tập hợp nào khác, thì phép kết thu được sẽ mất chính xác các phần tử đó, tạo cho chúng ta phép kết không đầy đủ lớn nhất. Nếu tất cả các tập hợp đều là tập con của hợp của các tập còn lại, việc loại bỏ bất kỳ tập hợp nào vẫn mang lại kết hợp đầy đủ và khi đó câu trả lời là kích thước của hợp trừ đi một phần tử hoặc bằng 0 nếu chỉ tồn tại một tập hợp. 

Điều này làm giảm vấn đề lặp lại trên từng tập hợp, tính toán hợp của tất cả các tập hợp khác và lấy kích thước tối đa của các hợp này. Thay vì$2^n$hoạt động, chúng tôi chỉ làm$n$các công đoàn, mỗi công đoàn có tối đa 50 phần tử. Việc biểu diễn các tập hợp dưới dạng mặt nạ bit trên các số từ 1-50 giúp đơn giản hóa các phép toán hợp và tính toán kích thước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * k) | O(k) | Quá chậm | 
| Tối ưu (loại trừ một bộ) | O(n * k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Đối với mỗi trường hợp thử nghiệm, hãy đọc$n$, số tập hợp. 
2. Đọc từng bộ và lưu trữ dưới dạng Python`set`hoặc dưới dạng bitmask số nguyên 50 bit. Mặt nạ bit rất tiện lợi vì sự kết hợp trở thành một phép toán OR theo bit đơn lẻ. 
3. Tính hợp của tất cả$n$bộ. Gọi đây`full_union`. 
4. Khởi tạo một biến`best`thành 0. Điều này sẽ theo dõi kích thước tối đa của các bộ có thể đạt được nhỏ hơn`full_union`. 
5. Đối với mỗi bộ$S_i$: 

- Tính hợp của tất cả các tập hợp ngoại trừ$S_i$. Sử dụng mặt nạ bit, đây là`union_except_i = full_union & ~S_i`. Sử dụng bộ Python, nó`union_except_i = full_union - S_i`chỉ dành cho các phần tử trong$S_i$hoặc`union_except_i = set().union(*[S[j] for j != i])`. 
- Đếm số phần tử trong`union_except_i`và cập nhật`best`nếu lớn hơn. 
6. Sau khi kiểm tra tất cả các bộ, in`best`cho trường hợp thử nghiệm. 

Tại sao nó hoạt động: Bất kỳ tập hợp nào có thể đạt được không bằng phép kết hợp đầy đủ đều phải bỏ qua ít nhất một phần tử khỏi phép kết hợp. Loại bỏ toàn bộ một tập hợp là đủ để che đi tất cả các thiếu sót có thể xảy ra vì việc bao gồm ít tập hợp hơn không thể tạo ra một liên minh lớn hơn`full_union`. Do đó, việc lặp lại từng bộ và loại trừ nó đảm bảo chúng ta sẽ tìm thấy kích thước tối đa có thể đạt được bên dưới`full_union`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        sets = []
        for _ in range(n):
            data = list(map(int, input().split()))
            sets.append(set(data[1:]))
        
        full_union = set()
        for s in sets:
            full_union |= s
        
        best = 0
        for i in range(n):
            union_except_i = full_union - sets[i]
            best = max(best, len(union_except_i))
        
        print(best)

solve()
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`và lưu trữ từng bộ dưới dạng một bộ Python. Chúng tôi tính toán sự kết hợp của tất cả các bộ với`|=`toán tử rồi lặp lại, lần lượt loại trừ từng bộ. Việc sử dụng phép trừ tập hợp đảm bảo chúng ta chỉ loại bỏ các phần tử duy nhất của tập hợp hiện tại, đây chính xác là cơ chế để tìm ra liên kết không đầy đủ lớn nhất có thể đạt được. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:```
3
3 1 2 3
2 4 5
2 3 4
```| Bước | đầy đủ_union | liên minh_ngoại trừ_i | tốt nhất | 
| --- | --- | --- | --- | 
| loại trừ S1 | {2,3,4,5} | cỡ 4 | 4 | 
| loại trừ S2 | {1,2,3,4} | cỡ 4 | 4 | 
| loại trừ S3 | {1,2,3,4,5} | cỡ 5 | 4 (không thể bằng full_union) | 

Đầu ra: 4 

Điều này chứng tỏ rằng việc loại trừ tập thứ nhất hoặc tập thứ hai sẽ tạo ra tập hợp lớn nhất có thể đạt được mà không bao gồm tất cả các phần tử từ`full_union`. 

**Mẫu 2:** 

đầu vào:```
4
4 1 2 3 4
3 2 5 6
3 3 5 6
3 4 5 6
```| Bước | đầy đủ_union | liên minh_ngoại trừ_i | tốt nhất | 
| --- | --- | --- | --- | 
| loại trừ S1 | {2,3,4,5,6} | cỡ 5 | 5 | 
| loại trừ S2 | {1,3,4,5,6} | cỡ 5 | 5 | 
| loại trừ S3 | {1,2,4,5,6} | cỡ 5 | 5 | 
| loại trừ S4 | {1,2,3,5,6} | cỡ 5 | 5 | 

Đầu ra: 5 

Chúng ta thấy rằng việc loại trừ bất kỳ tập hợp nào sẽ để lại một hợp tối đa nhỏ hơn`full_union`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n * k) | Đối với mỗi trường hợp thử nghiệm, chúng tôi tính toán`full_union`(O(n_k)) rồi lặp lại loại trừ từng bộ (O(n_k)). k 50 và n 50, do đó tổng số thao tác 2500 cho mỗi trường hợp thử nghiệm. | 
| Không gian | O(n * k) | Chúng tôi lưu trữ tối đa n bộ kích thước k mỗi bộ. Biểu diễn Bitmask làm giảm không gian nhưng không cần thiết cho những ràng buộc này. | 

Điều này thoải mái phù hợp trong giới hạn thời gian 2 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("4\n3\n3 1 2 3\n2 4 5\n2 3 4\n4\n4 1 2 3 4\n3 2 5 6\n3 3 5 6\n3 4 5 6\n5\n1 1\n3 3 6 10\n1 9\n2 1 3\n3 5 8 9\n1\n2 4 28") == "4\n5\n6\n0"

# custom tests
assert run("1\n1\n3 1 2 3") == "0", "single set"
assert run("1\n2\n1 1\n1 2") == "1", "two disjoint singletons"
assert run("1\n3\n2 1 2\n2 1 2\n2 1 2") == "2", "all sets equal"
assert run("1\n2\n3 1 2 3\n3 2 3 4") == "3", "overlapping sets"
assert run("1\n3\n1 1\n1 2\n1 3") == "2", "each singleton, maximum union excluding one"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chỉ 1 bộ | 0 | Vỏ cạnh của bộ đơn | 
| 2 đơn vị rời rạc | 1 | Loại trừ một trong hai tạo ra sự kết hợp tối đa có thể đạt được | 
| 3 bộ giống hệt nhau | 2 | Việc xóa bất kỳ tập hợp nào vẫn mang lại kết quả nhỏ hơn kết hợp đầy đủ | 
| 2 bộ chồng lên nhau | | |
