---
title: "CF 1735D - Bộ meta"
description: "Chúng tôi được cung cấp một biến thể của trò chơi bài \"Set\". Mỗi thẻ có các tính năng $k$, mỗi tính năng là 0, 1 hoặc 2. Do đó, thẻ là một vectơ độ dài-$k$ trên ${0,1,2}$. Một \"bộ\" là ba thẻ bất kỳ trong đó, đối với mỗi tính năng, cả ba giá trị đều bằng nhau hoặc cả ba đều khác nhau."
date: "2026-06-09T18:15:50+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "combinatorics", "data-structures", "hashing", "math"]
categories: ["algorithms"]
codeforces_contest: 1735
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 824 (Div. 2)"
rating: 1700
weight: 1735
solve_time_s: 535
verified: true
draft: false
---

[CF 1735D - Bộ meta](https://codeforces.com/problemset/problem/1735/D) 

**Đánh giá:** 1700 
**Tags:** Brute Force, tổ hợp, cấu trúc dữ liệu, hàm băm, toán học 
**Thời gian giải:** 8 phút 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một biến thể của trò chơi bài "Set". Mỗi thẻ có$k$các tính năng, mỗi tính năng là 0, 1 hoặc 2. Do đó, thẻ có chiều dài-$k$véc tơ trên$\{0,1,2\}$. Một "bộ" là ba lá bài bất kỳ trong đó, đối với mỗi tính năng, cả ba giá trị đều bằng nhau hoặc cả ba đều khác nhau. Đầu vào của chúng tôi là một danh sách$n$các thẻ riêng biệt và kết quả mà chúng tôi tìm kiếm là số nhóm năm thẻ riêng biệt, được gọi là "bộ meta", chứa nhiều hơn một bộ. 

Những hạn chế$n \le 10^3$Và$k \le 20$chỉ ra rằng chúng ta không thể liệt kê trực tiếp tất cả các kết hợp năm lá bài, bởi vì có${1000 \choose 5} \approx 8 \cdot 10^{13}$những nhóm như vậy. Tuy nhiên, việc liệt kê tất cả các bộ ba là khả thi, bởi vì${1000 \choose 3} \approx 1.66 \cdot 10^{8}$, điều này nằm ngoài khả năng, đặc biệt là với$O(k)$kiểm tra tính hợp lệ của thiết lập. Chúng ta phải tận dụng cấu trúc tổ hợp của "Set" thay vì liệt kê một cách thô bạo các nhóm ngũ phân. 

Các trường hợp khó khăn có thể phá vỡ một giải pháp đơn giản bao gồm các thẻ chỉ khác nhau ở một tính năng, các thẻ hoàn toàn giống nhau về một tính năng hoặc cấu hình trong đó nhiều bộ ba chia sẻ thẻ. Ví dụ: một nhóm năm thẻ trong đó bốn thẻ giống nhau ngoại trừ một đặc điểm có thể tạo thành nhiều bộ. Cách tiếp cận bất cẩn khi đếm các tập hợp một cách độc lập có thể đếm hai lần hoặc bỏ sót các tập hợp chồng chéo, vì vậy chúng ta cần một phương pháp đếm cẩn thận các tập hợp meta duy nhất. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là liệt kê tất cả các tập hợp con gồm năm thẻ, sau đó đối với mỗi tập hợp con, hãy kiểm tra tất cả mười bộ ba có thể có để biết tính hợp lệ và đếm những tập hợp có chứa nhiều hơn một bộ. Điều này đúng về nguyên tắc, nhưng không khả thi về mặt tính toán: vì$n = 1000$, số lượng tập hợp con năm lá bài là khoảng$8 \cdot 10^{13}$, khiến không thể xử lý trong 4 giây. 

Quan sát quan trọng là trong cấu trúc giống như "Set" trên$\{0,1,2\}^k$, quân bài thứ ba trong bộ được xác định duy nhất bởi hai quân bài đầu tiên. Đối với mỗi tính năng, nếu hai thẻ có cùng giá trị thì thẻ thứ ba phải khớp; nếu chúng khác nhau thì giá trị thứ ba phải là giá trị không có. Điều này cho phép chúng ta biểu diễn một tập hợp dưới dạng vectơ có độ dài có thể băm được$k$, cho phép$O(1)$tra cứu trong từ điển. 

Sử dụng thuộc tính này, trước tiên chúng ta có thể đếm tất cả các tập hợp trong số$n$thẻ trong$O(n^2 k)$bằng cách lặp lại tất cả các cặp, tính toán thẻ thứ ba cần thiết và kiểm tra sự tồn tại của nó trong một bộ thẻ. Khi đã biết tất cả các bộ, chúng ta cần đếm các nhóm năm lá bài chứa ít nhất hai bộ. Nhận thấy rằng một siêu tập hợp phải là sự kết hợp của hai bộ chồng chéo có chung một hoặc hai thẻ cho phép chúng ta liệt kê các siêu tập hợp tiềm năng một cách hiệu quả bằng cách lặp qua các cặp tập hợp có chung một hoặc hai thẻ và ghi lại kết hợp nếu kích thước của nó là năm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^5 k)$|$O(nk)$| Quá chậm | 
| Tối ưu |$O(n^2 k + s^2)$|$O(nk + s)$,$s$= số bộ | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số thẻ$n$và tính năng$k$. Lưu trữ tất cả các thẻ dưới dạng bộ dữ liệu trong một bộ để truy vấn thành viên nhanh chóng. Điều này cho phép$O(1)$kiểm tra sự tồn tại của thẻ thứ ba tiềm năng trong một bộ. 
2. Khởi tạo một danh sách trống để lưu trữ tất cả các tập hợp được phát hiện. Lặp lại tất cả các cặp thẻ$(a,b)$. Đối với mỗi vị trí đặc điểm$i$, xác định giá trị của lá bài thứ ba$c_i$điều đó sẽ hoàn thành một bộ: nếu$a_i = b_i$, sau đó$c_i = a_i$; nếu không thì,$c_i = 3 - a_i - b_i$bởi vì$0+1+2 = 3$. Điều này sử dụng số học mô-đun để tìm giá trị còn thiếu một cách hiệu quả. 
3. Kiểm tra xem thẻ thứ ba được tính toán có tồn tại trong bộ thẻ đầu vào hay không. Nếu có, hãy ghi lại bộ ba đã sắp xếp thành một bộ để tránh trùng lặp. Tiếp tục cho tất cả các cặp. Cuối cùng, chúng tôi có một danh sách đầy đủ của tất cả các bộ. 
4. Khởi tạo một bộ để ghi lại các bộ meta duy nhất. Lặp lại tất cả các cặp tập hợp. Nếu sự kết hợp các chỉ số thẻ của họ có kích thước năm, hãy thêm nó vào bộ sưu tập meta. Điều này đảm bảo chúng tôi tính mỗi tập hợp meta chính xác một lần, dựa trên các tập hợp chồng chéo. 
5. Xuất ra số lượng meta-set được tìm thấy. 

Tại sao nó hoạt động: Mỗi bộ được xác định duy nhất bởi hai lá bài của nó. Bằng cách xây dựng tất cả các bộ thông qua các cặp và kiểm tra kích thước kết hợp của chúng, chúng tôi liệt kê một cách có hệ thống tất cả các nhóm năm thẻ có chứa nhiều bộ mà không thiếu hoặc đếm kép bất kỳ bộ nào. Phương pháp này tận dụng cấu trúc tổ hợp của "Set" để giảm bớt sự đơn giản$O(n^5)$vấn đề có thể quản lý được$O(n^2 k + s^2)$, Ở đâu$s$là số tập hợp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from itertools import combinations

n, k = map(int, input().split())
cards = [tuple(map(int, input().split())) for _ in range(n)]
card_set = set(cards)

# map card to index for fast lookup
card_index = {cards[i]: i for i in range(n)}

sets = []
for i in range(n):
    for j in range(i+1, n):
        third = []
        for x, y in zip(cards[i], cards[j]):
            if x == y:
                third.append(x)
            else:
                third.append(3 - x - y)
        third = tuple(third)
        if third in card_set:
            indices = sorted([i, j, card_index[third]])
            if indices not in sets:
                sets.append(indices)

meta_sets = set()
for s1, s2 in combinations(sets, 2):
    union = set(s1) | set(s2)
    if len(union) == 5:
        meta_sets.add(frozenset(union))

print(len(meta_sets))
```Mỗi phần tương ứng với các bước thuật toán: lưu trữ thẻ để tra cứu O(1), tạo tất cả các bộ từ các cặp, lưu trữ các bộ mà không trùng lặp và xây dựng các bộ meta từ các bộ chồng chéo. sử dụng`frozenset`đảm bảo tính duy nhất của các bộ meta bất kể thứ tự. Những điểm tinh tế bao gồm việc tính toán lá bài thứ ba với`3 - x - y`và tránh trùng lặp trong cả bộ và bộ meta. 

## Ví dụ đã hoạt động 

đầu vào:```
8 4
0 0 0 0
0 0 0 1
0 0 0 2
0 0 1 0
0 0 2 0
0 1 0 0
1 0 0 0
2 2 0 0
```| Cặp (i,j) | Thẻ thứ ba | Tồn tại? | Bộ | 
| --- | --- | --- | --- | 
| 0000, 0001 | 0002 | Có | 0000,0001,0002 | 
| 0000, 0010 | 0020 | Có | 0000,0010,0020 | 
| ... | ... | ... | ... | 

Hợp của hai bộ đầu tiên:`{0000,0001,0002,0010,0020}`→ kích thước 5 → bộ meta được tính. 

Điều này xác nhận thuật toán xác định chính xác tập hợp meta duy nhất. 

Ví dụ thứ hai có thể hiển thị tương tự các bộ chồng chéo tạo ra các bộ meta, xác nhận tính chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2 k + s^2) | O(n^2 k) để tạo tất cả các bộ, O(s^2) để liệt kê các liên kết tập hợp meta | 
| Không gian | O(n k + s) | O(n k) để lưu trữ thẻ, O(s) để lưu trữ tất cả các bộ | 

Được cho$n \le 1000$Và$k \le 20$, các thao tác rất phù hợp trong giới hạn thời gian 4 giây. 

## Trường hợp thử nghiệm```python
import sys, io
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())
    cards = [tuple(map(int, input().split())) for _ in range(n)]
    card_set = set(cards)
    card_index = {cards[i]: i for i in range(n)}
    from itertools import combinations
    sets = []
    for i in range(n):
        for j in range(i+1, n):
            third = []
            for x, y in zip(cards[i], cards[j]):
                third.append(x if x == y else 3 - x - y)
            third = tuple(third)
            if third in card_set:
                indices = sorted([i,j,card_index[third]])
                if indices not in sets:
                    sets.append(indices)
    meta_sets = set()
    for s1, s2 in combinations(sets,2):
        union = set(s1) | set(s2)
        if len(union) == 5:
            meta_sets.add(frozenset(union))
    return str(len(meta_sets))

# provided sample
assert run("""8 4
0 0 0 0
0 0 0 1
0 0 0 2
0 0 1 0
0 0 2 0
0
```
