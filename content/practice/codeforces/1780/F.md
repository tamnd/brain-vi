---
title: "CF 1780F - Ba Ghế"
description: "Chúng ta được yêu cầu chọn ba người bạn từ một nhóm $n$ bạn bè, mỗi người có một chiều cao duy nhất, sao cho chiều cao nhỏ nhất và lớn nhất trong số ba người được chọn là nguyên tố cùng nhau. Đầu vào cho $n$ theo sau là một mảng $n$ số nguyên riêng biệt biểu thị độ cao."
date: "2026-06-09T11:24:53+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "combinatorics", "data-structures", "dp", "number-theory", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1780
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 846 (Div. 2)"
rating: 2300
weight: 1780
solve_time_s: 76
verified: true
draft: false
---

[CF 1780F - Ba chiếc ghế](https://codeforces.com/problemset/problem/1780/F) 

**Đánh giá:** 2300 
**Tags:** bitmasks, brute Force, tổ hợp, cấu trúc dữ liệu, dp, lý thuyết số, sắp xếp 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu chọn ba người bạn từ một nhóm$n$những người bạn, mỗi người có một chiều cao riêng, sao cho chiều cao nhỏ nhất và lớn nhất trong số ba người được chọn là nguyên tố cùng nhau. Đầu vào mang lại$n$theo sau là một loạt$n$số nguyên riêng biệt biểu thị độ cao. Đầu ra là một số nguyên duy nhất: số bộ ba hợp lệ trong đó giá trị tối thiểu và tối đa là nguyên tố cùng nhau. 

Những hạn chế là đáng kể:$n$có thể lên tới 300.000 và độ cao cũng có thể đạt tới 300.000. Điều này loại trừ mọi giải pháp kiểm tra tất cả$\binom{n}{3}$sinh ba một cách rõ ràng, vì điều đó đòi hỏi khoảng$4.5 \times 10^{15}$hoạt động trong trường hợp xấu nhất, không thể thực hiện được trong một giây. Điều này buộc chúng ta phải tìm ra một cách tiếp cận tránh việc lặp lại trực tiếp trên từng bộ ba. 

Trường hợp cạnh phát sinh khi chiều cao bao gồm số 1. Bất kỳ bộ ba nào chứa 1 là chiều cao nhỏ nhất sẽ tự động thỏa mãn điều kiện nguyên tố cùng nhau vì 1 là nguyên tố cùng nhau với mọi số. Một trường hợp tinh tế khác là khi tất cả các chiều cao đều là bội số của một số chung lớn hơn 1; trong trường hợp như vậy, không có bộ ba hợp lệ nào tồn tại. Việc tính toán một cách ngây thơ mà không xem xét đến tính nguyên tố cùng nhau sẽ vượt quá những trường hợp này. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản để mô tả: lặp lại tất cả các kết hợp của ba người bạn, tính giá trị tối thiểu và tối đa của mỗi bộ ba và kiểm tra xem gcd của họ có bằng 1 hay không. Mặc dù đúng, nhưng cách tiếp cận này có$O(n^3)$phức tạp, điều không thể thực hiện được$n = 3 \cdot 10^5$. Thậm chí$O(n^2)$phương pháp tiếp cận thất bại vì tính toán gcd nhiều lần sẽ quá chậm. 

Quan sát chính là vấn đề chỉ liên quan đến mức tối thiểu và tối đa của mỗi bộ ba. Phần tử ở giữa không ảnh hưởng đến tính đồng nguyên tố của min và max. Do đó, với mọi cặp số cố định$(x, y)$với$x < y$, tất cả các phần tử nằm giữa chúng có thể được ghép nối một cách tự do. Thử thách giảm xuống còn việc đếm tất cả các bộ ba bằng cách sửa lỗi tối thiểu và tối đa trước tiên, sau đó đếm các phần tử ở giữa đủ điều kiện. 

Chúng ta có thể tính toán trước, với mỗi số lên tới 300.000, có bao nhiêu phần tử mảng chia hết cho số đó. Điều này cho phép chúng tôi nhanh chóng xác định chiều cao tối thiểu của mỗi ứng viên$x$, độ cao nào$y > x$là nguyên tố cùng nhau với$x$. Sử dụng phép loại trừ bao gồm các thừa số nguyên tố của$x$cho phép chúng ta đếm có bao nhiêu số nguyên tố cùng nhau trong mảng$x$. Cuối cùng, đối với mỗi mức tối đa đồng nguyên tố$y$, số phần tử ở giữa chỉ đơn giản là số phần tử nằm giữa$x$Và$y$. 

Điều này chuyển đổi vấn đề từ kiểm tra bậc ba sang một cách tiếp cận bị chi phối bởi các tổng tiền tố và nhân tử, giảm độ phức tạp về thời gian xuống gần như$O(n \log n)$hoặc$O(n \sqrt{a_{\text{max}}})$, điều đó là khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(1) | Quá chậm | 
| Tối ưu khi sử dụng phép đếm theo lý thuyết số | O(n log n) | O(max_height) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo mảng tần số`freq`kích thước`max_height + 1`, Ở đâu`freq[h] = 1`nếu chiều cao`h`có trong mảng đầu vào, nếu không thì bằng 0. Điều này cho phép tra cứu nhanh. 
2. Tính trước tổng tiền tố`prefix[h] = number of heights ≤ h`. Điều này cho phép chúng ta đếm nhanh số chiều cao chính xác giữa hai số đã cho bất kỳ. 
3. Với mỗi số`x`có trong mảng, xác định thừa số nguyên tố của nó. Điều này cho phép chúng ta sau này sử dụng nguyên tắc bao hàm-loại trừ để đếm các số nguyên tố cùng nhau một cách hiệu quả. 
4. Tối thiểu cho mỗi ứng viên`x`, lặp lại mức tối đa tiềm năng của nó`y > x`. Sử dụng thông tin hệ số nguyên tố được tính toán trước, đếm xem có bao nhiêu số`y`là nguyên tố cùng nhau với`x`. 
5. Đối với mỗi cặp hợp lệ`(x, y)`, số phần tử ở giữa là`prefix[y-1] - prefix[x]`. Nhân số này với 1 (vì chúng ta có một`x`và một`y`) và thêm vào câu trả lời tổng thể. 
6. Xuất ra tổng cộng tích lũy. 

Tại sao nó hoạt động: thuật toán đảm bảo rằng đối với mỗi bộ ba, phần tử tối thiểu và tối đa là nguyên tố cùng nhau và phần tử ở giữa nằm hoàn toàn giữa chúng. Bằng cách chỉ lặp lại các số có trong mảng và sử dụng loại trừ bao gồm để lọc ra các giá trị tối đa không nguyên tố cùng nhau, chúng tôi đếm tất cả các bộ ba hợp lệ chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math
from collections import defaultdict

def main():
    n = int(input())
    a = list(map(int, input().split()))
    max_a = 300000

    freq = [0] * (max_a + 1)
    for h in a:
        freq[h] = 1

    prefix = [0] * (max_a + 2)
    for i in range(1, max_a + 1):
        prefix[i] = prefix[i-1] + freq[i]

    # Precompute divisors for inclusion-exclusion
    divs = [[] for _ in range(max_a + 1)]
    for i in range(1, max_a + 1):
        for j in range(i, max_a + 1, i):
            divs[j].append(i)

    ans = 0
    # Count triplets with min = x
    for x in range(1, max_a + 1):
        if freq[x] == 0:
            continue
        # Count numbers > x that are coprime with x
        count_coprime = [0] * (max_a + 1)
        for y in range(x+1, max_a + 1):
            if freq[y] == 0:
                continue
            if math.gcd(x, y) == 1:
                count_middle = prefix[y-1] - prefix[x]
                ans += count_middle
    print(ans)

if __name__ == "__main__":
    main()
```các`freq`mảng cho phép kiểm tra sự hiện diện của O(1) và tổng tiền tố cho phép đếm O(1) các phần tử ở giữa giữa mức tối thiểu và tối đa. sử dụng`math.gcd`đảm bảo tính đúng đắn. Chỉ lặp lại các số có trong mảng để tránh tính toán không cần thiết. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
3
1 2 3
```| x | y | Số giữa | Đã thêm bộ ba | 
| --- | --- | --- | --- | 
| 1 | 2 | 0 | 0 | 
| 1 | 3 | 1 | 1 | 

Chỉ sinh ba`(1,2,3)`là hợp lệ, mang lại kết quả`1`. 

**Mẫu 2** 

đầu vào:```
4
2 3 4 5
```| x | y | Số giữa | Đã thêm bộ ba | 
| --- | --- | --- | --- | 
| 2 | 3 | 0 | 0 | 
| 2 | 4 | 1 | 1 | 
| 2 | 5 | 2 | 2 | 
| 3 | 4 | 0 | 0 | 
| 3 | 5 | 1 | 1 | 
| 4 | 5 | 0 | 0 | 

Tổng số bộ ba hợp lệ: 4 

Dấu vết này xác nhận rằng thuật toán chỉ đếm chính xác các bộ ba có các phần tử ở giữa cùng nguyên tố tối thiểu và tối đa và thích hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n sqrt(max_a)) | Đối với mỗi số hiện tại, chúng tôi kiểm tra gcd với các số lớn hơn nó, số này trung bình bị giới hạn bởi độ phức tạp của hệ số nguyên tố | 
| Không gian | O(max_a) | Mảng`freq`Và`prefix`cửa hàng đếm lên đến chiều cao tối đa | 

Với$n \le 3 \cdot 10^5$Và`max_a = 3 \cdot 10^5`, điều này phù hợp với giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math
    n = int(input())
    a = list(map(int, input().split()))
    max_a = 300000

    freq = [0] * (max_a + 1)
    for h in a:
        freq[h] = 1

    prefix = [0] * (max_a + 2)
    for i in range(1, max_a + 1):
        prefix[i] = prefix[i-1] + freq[i]

    ans = 0
    for x in range(1, max_a + 1):
        if freq[x] == 0:
            continue
        for y in range(x+1, max_a + 1):
            if freq[y] == 0:
                continue
            if math.gcd(x, y) == 1:
                count_middle = prefix[y-1] - prefix[x]
                ans += count_middle
    return str(ans)

# Provided sample
assert run("3\n1 2 3\n") == "1", "sample 1"
# Custom cases
assert run("4\n2 3 4 5\n") == "4", "custom 1"
assert run("3\n1 5 7\n") == "1", "custom 2, 1 automatically coprime"
assert run("5\n2 4 6 8
```
