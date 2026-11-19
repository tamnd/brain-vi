---
title: "CF 2102D - Hoán đổi bộ tứ"
description: "Chúng ta có một hoán vị và một thao tác được phép duy nhất trên bốn vị trí liên tiếp: $$[ai,a{i+1},a{i+2},a{i+3}] rightarrow [a{i+2},a{i+3},ai,a{i+1}]$$ Được xem theo cách khác, phần tử ở vị trí $i$ hoán đổi với $i+2$ và phần tử ở vị trí $i+1$ hoán đổi với $i+3$."
date: "2026-06-09T03:57:27+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2102
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1024 (Div. 2)"
rating: 1800
weight: 2102
solve_time_s: 136
verified: false
draft: false
---

[CF 2102D - Hoán đổi bộ tứ](https://codeforces.com/problemset/problem/2102/D) 

**Đánh giá:** 1800 
**Tags:** cấu trúc dữ liệu, tham lam, sắp xếp 
**Thời gian giải:** 2m 16s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hoán vị và một phép toán được phép duy nhất trên bốn vị trí liên tiếp:$$[a_i,a_{i+1},a_{i+2},a_{i+3}] \rightarrow [a_{i+2},a_{i+3},a_i,a_{i+1}]$$Nhìn khác đi, phần tử ở vị trí$i$hoán đổi với$i+2$và phần tử ở vị trí$i+1$hoán đổi với$i+3$. 

Nhiệm vụ không phải là quyết định xem hoán vị mục tiêu có thể truy cập được hay không. Chúng ta phải tìm hoán vị nhỏ nhất về mặt từ điển có thể đạt được sau khi áp dụng thao tác này bao nhiêu lần. 

Tổng chiều dài của tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$. Bất kỳ giải pháp nào mô phỏng nhiều lần các thao tác hoặc tìm kiếm trong không gian trạng thái đều không thể thực hiện được. Thậm chí$O(n^2)$mỗi trường hợp thử nghiệm sẽ quá chậm trong trường hợp xấu nhất. Chúng ta cần thứ gì đó xung quanh$O(n \log n)$. 

Quan sát tinh tế đầu tiên là mọi phần tử luôn ở các vị trí có cùng tính chẵn lẻ. Phần tử bắt đầu ở vị trí 1 chỉ có thể di chuyển đến vị trí 3, 5, 7, v.v. Phần tử bắt đầu ở vị trí 2 chỉ có thể di chuyển đến vị trí 4, 6, 8, v.v. 

Một sai lầm phổ biến là kết luận rằng chúng ta có thể sắp xếp đơn giản các phần tử ở vị trí lẻ và sắp xếp các phần tử ở vị trí chẵn một cách độc lập. 

Coi như:```
n = 4
3 2 1 4
```Vị trí lẻ chứa`[3,1]`, các vị trí chẵn chứa`[2,4]`. 

Sắp xếp từng nhóm sẽ cho:```
1 2 3 4
```nhưng cấu hình này không thể truy cập được. Hoạt động duy nhất tạo ra:```
1 4 3 2
```Thành phần còn thiếu là ràng buộc chẵn lẻ đối với các hoán vị được áp dụng bên trong hai nhóm. 

Một trường hợp dễ bỏ sót khác xuất hiện khi các nhóm chẵn và lẻ được sắp xếp yêu cầu các phép chẵn lẻ hoán vị không khớp nhau. Sau đó, việc hợp nhất được sắp xếp đầy đủ là không thể truy cập được và chúng tôi phải cố tình làm cho một nhóm tệ hơn một chút trong khi vẫn giữ câu trả lời ở mức tối thiểu về mặt từ điển. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ coi mỗi hoán vị là một nút biểu đồ và mỗi thao tác là một cạnh. Tìm kiếm theo chiều rộng cuối cùng sẽ tìm thấy trạng thái có thể tiếp cận nhỏ nhất về mặt từ điển. 

Điều này đúng vì mọi hoán vị có thể tiếp cận sẽ được khám phá. Thật không may là không gian trạng thái chứa$n!$hoán vị. Ngay cả đối với$n=10$, điều này đã hoàn toàn không thể thực hiện được. 

Để có được thứ gì đó nhanh hơn, chúng ta cần hiểu thao tác thực sự làm gì. 

Lấy dãy con của các phần tử hiện đang ở vị trí lẻ và gọi nó là$O$. Lấy dãy con ở vị trí chẵn và gọi nó là$E$. 

Mỗi thao tác thực hiện một lần hoán đổi liền kề bên trong$O$và một trao đổi liền kề bên trong$E$đồng thời. Vì các hoán đổi liền kề tạo ra toàn bộ nhóm đối xứng, nên chúng ta có thể sắp xếp lại các phần tử vị trí lẻ một cách tùy ý và các phần tử vị trí chẵn một cách tùy ý. 

Tuy nhiên, mỗi thao tác đóng góp chính xác một chuyển vị cho cả hai nhóm. Tính chẵn lẻ của hoán vị áp dụng cho$O$và tính chẵn lẻ của hoán vị áp dụng cho$E$luôn thay đổi cùng nhau. 

Điều đó có nghĩa là bất biến sau đây đúng: 

Hoán vị áp dụng cho vị trí lẻ và hoán vị áp dụng cho vị trí chẵn phải có cùng tính chẵn lẻ. 

Điều này hóa ra là hạn chế duy nhất. Bất kỳ cặp sắp xếp lại nào có tính chẵn lẻ đều có thể truy cập được. 

Bây giờ bài toán trở thành tổ hợp thuần túy. 

Ứng cử viên nhỏ nhất về mặt từ điển có được bằng cách sắp xếp các phần tử vị trí lẻ và các phần tử vị trí chẵn một cách riêng biệt, sau đó hợp nhất chúng lại vào vị trí của chúng. 

Nếu các hoán vị chẵn lẻ cần thiết để đạt được các thứ tự được sắp xếp đó bằng nhau thì ứng viên này có thể truy cập được và là tối ưu. 

Nếu các điểm chẵn lẻ khác nhau, ứng viên sẽ không thể tiếp cận được. Chúng ta phải thay đổi tính chẵn lẻ của chính xác một nhóm. Cách rẻ nhất là hoán đổi hai phần tử cuối cùng của nhóm lẻ đã sắp xếp hoặc hoán đổi hai phần tử cuối cùng của nhóm chẵn đã sắp xếp. Vì các nhóm được sắp xếp nên việc thay đổi hai phần tử cuối cùng sẽ gây ra thiệt hại nhỏ nhất về mặt từ điển. Chúng tôi xây dựng cả hai khả năng và chọn hoán vị kết quả nhỏ hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n!)$|$O(n!)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Trích xuất các phần tử ở vị trí lẻ thành mảng`odd`và các phần tử ở vị trí chẵn thành mảng`even`. 
2. Tạo`odd_sorted`Và`even_sorted`bằng cách sắp xếp các mảng đó. 
3. Tính tính chẵn lẻ của hoán vị biến đổi`odd`vào trong`odd_sorted`. 
4. Tính tính chẵn lẻ của hoán vị biến đổi`even`vào trong`even_sorted`. 
5. Nếu hai số chẵn lẻ bằng nhau thì hợp nhất`odd_sorted`Và`even_sorted`trở lại vị trí xen kẽ và xuất kết quả. 
6. Nếu không, hãy tạo hai câu trả lời dự kiến. 

1. Hoán đổi hai phần tử cuối cùng của`odd_sorted`, sau đó hợp nhất. 
2. Hoán đổi hai phần tử cuối cùng của`even_sorted`, sau đó hợp nhất. 
7. Xuất ra ứng viên nhỏ hơn về mặt từ điển. 

Việc hoán đổi hai phần tử cuối cùng sẽ làm đảo lộn tính chẵn lẻ hoán vị của chính xác một nhóm. Vì sự không khớp chẵn lẻ chính xác là một bit nên một trong hai ứng cử viên đều có thể truy cập được. 

### Tại sao nó hoạt động 

Mỗi thao tác thực hiện một chuyển vị bên trong dãy con vị trí lẻ và một chuyển vị bên trong dãy con vị trí chẵn. Do đó, tính chẵn lẻ của hai hoán vị cảm ứng luôn bằng nhau. 

Các chuyển vị liền kề tạo ra tất cả các hoán vị, vì vậy bất biến duy nhất là đẳng thức chẵn lẻ này. 

Việc sắp xếp cả hai nhóm riêng biệt sẽ tạo ra sự sắp xếp nhỏ nhất về mặt từ điển trong số tất cả các hoán vị tôn trọng tính chẵn lẻ của vị trí. Nếu điều kiện chẵn lẻ của nó đã được thỏa mãn thì không có câu trả lời nào có thể đạt được tốt hơn. 

Khi điều kiện chẵn lẻ không thành công, chúng ta phải thay đổi tính chẵn lẻ của một nhóm. Mọi thay đổi chẵn lẻ đều yêu cầu một hoán vị lẻ. Bắt đầu từ một mảng được sắp xếp, hoán vị lẻ nhỏ nhất về mặt từ điển có được bằng cách hoán đổi hai phần tử cuối cùng của nó. Làm như vậy sẽ thay đổi câu trả lời càng muộn càng tốt, giúp giảm thiểu thiệt hại về mặt từ điển. So sánh sửa đổi nhóm lẻ và sửa đổi nhóm chẵn mang lại hoán vị tốt nhất có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def permutation_parity(original, target_sorted):
    pos = {v: i for i, v in enumerate(target_sorted)}
    p = [pos[x] for x in original]

    n = len(p)
    vis = [False] * n
    parity = 0

    for i in range(n):
        if vis[i]:
            continue

        cur = i
        length = 0

        while not vis[cur]:
            vis[cur] = True
            cur = p[cur]
            length += 1

        parity ^= (length - 1) & 1

    return parity

def build(odd_vals, even_vals, n):
    res = [0] * n
    oi = 0
    ei = 0

    for i in range(n):
        if i % 2 == 0:
            res[i] = odd_vals[oi]
            oi += 1
        else:
            res[i] = even_vals[ei]
            ei += 1

    return res

t = int(input())

for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))

    odd = a[::2]
    even = a[1::2]

    odd_sorted = sorted(odd)
    even_sorted = sorted(even)

    p_odd = permutation_parity(odd, odd_sorted)
    p_even = permutation_parity(even, even_sorted)

    if p_odd == p_even:
        ans = build(odd_sorted, even_sorted, n)
    else:
        cand1_odd = odd_sorted[:]
        cand1_odd[-1], cand1_odd[-2] = cand1_odd[-2], cand1_odd[-1]
        cand1 = build(cand1_odd, even_sorted, n)

        cand2_even = even_sorted[:]
        cand2_even[-1], cand2_even[-2] = cand2_even[-2], cand2_even[-1]
        cand2 = build(odd_sorted, cand2_even, n)

        ans = min(cand1, cand2)

    print(*ans)
```Giải pháp bắt đầu bằng cách chia hoán vị theo vị trí chẵn lẻ. Đây là sự phân rã tự nhiên vì các phép toán không bao giờ di chuyển một phần tử giữa các vị trí chẵn và lẻ. 

Tính toán chẵn lẻ sử dụng phân rã chu trình. Nếu một hoán vị bao gồm các chu trình có độ dài$c_1,c_2,\dots$, tính chẵn lẻ của nó bằng$$(c_1-1)+(c_2-1)+\cdots \pmod 2.$$Điều này cho phép chúng ta tính toán tính chẵn lẻ trong thời gian tuyến tính. 

Bước xây dựng lại xen kẽ các phần tử từ danh sách vị trí lẻ và danh sách vị trí chẵn. Vì Python sử dụng tính năng lập chỉ mục dựa trên số 0 nên các chỉ mục`0,2,4,...`tương ứng với các vị trí lẻ trong bài toán ban đầu. 

Phần tế nhị nhất là trường hợp sửa lỗi chẵn lẻ. Hoán đổi hai phần tử cuối cùng của một nhóm được sắp xếp sẽ thay đổi tính chẵn lẻ của nó trong khi gây ra mức tăng từ điển nhỏ nhất có thể. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
3 4 1 2
```Nhóm lẻ và nhóm chẵn: 

| Số lượng | Giá trị | 
| --- | --- | 
| lẻ | [3, 1] | 
| thậm chí | [4, 2] | 
| lẻ_sắp xếp | [1, 3] | 
| chẵn_sắp xếp | [2, 4] | 
| chẵn lẻ lẻ | 1 | 
| chẵn lẻ | 1 | 

Sự tương đương khớp nhau. 

Kết quả gộp: 

| Vị trí | Giá trị | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 3 | 
| 4 | 4 | 

Đầu ra:```
1 2 3 4
```Điều này chứng tỏ trường hợp dễ dàng có thể tiếp cận được việc hợp nhất được sắp xếp đầy đủ. 

### Mẫu 2 

đầu vào:```
5
5 4 3 1 2
```Nhóm lẻ và nhóm chẵn: 

| Số lượng | Giá trị | 
| --- | --- | 
| lẻ | [5, 3, 2] | 
| thậm chí | [4, 1] | 
| lẻ_sắp xếp | [2, 3, 5] | 
| chẵn_sắp xếp | [1, 4] | 
| chẵn lẻ lẻ | 1 | 
| chẵn lẻ | 1 | 

Sự tương đương lại phù hợp một lần nữa. 

Kết quả gộp: 

| Vị trí | Giá trị | 
| --- | --- | 
| 1 | 2 | 
| 2 | 1 | 
| 3 | 3 | 
| 4 | 4 | 
| 5 | 5 | 

Đầu ra:```
2 1 3 4 5
```Dấu vết cho thấy rằng câu trả lời không thu được bằng cách sắp xếp hoán vị trên toàn cầu. Chúng ta chỉ có thể sắp xếp lại các phần tử bên trong các lớp chẵn lẻ vị trí của chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp nhóm chẵn và lẻ chiếm ưu thế | 
| Không gian |$O(n)$| Mảng được sử dụng cho các nhóm và tái thiết | 

Tổng của tất cả$n$giá trị nhiều nhất là$2 \cdot 10^5$, Vì thế$O(n \log n)$thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve(inp: str) -> str:
    # paste solution here
    return ""

# provided samples
assert solve(
"""3
4
3 4 1 2
5
5 4 3 1 2
10
10 9 8 7 6 5 4 3 2 1
"""
) == (
"""1 2 3 4
2 1 3 4 5
2 1 4 3 6 5 8 7 10 9
"""
)

# minimum size
assert solve(
"""1
4
1 2 3 4
"""
) == (
"""1 2 3 4
"""
)

# already optimal
assert solve(
"""1
5
2 1 3 4 5
"""
) == (
"""2 1 3 4 5
"""
)

# parity-fix case
assert solve(
"""1
4
3 2 1 4
"""
) == (
"""1 4 3 2
"""
)

# larger boundary-style pattern
assert solve(
"""1
6
6 5 4 3 2 1
"""
) == (
"""2 1 4 3 6 5
"""
)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 3 4`|`1 2 3 4`| Đã hoán vị tối ưu | 
|`2 1 3 4 5`| cùng một đầu ra | Không có thay đổi không cần thiết | 
|`3 2 1 4`|`1 4 3 2`| Nhánh sửa lỗi chẵn lẻ | 
|`6 5 4 3 2 1`|`2 1 4 3 6 5`| Vỏ có chiều dài đều lớn hơn | 

## Vỏ cạnh 

Hãy xem xét:```
4
3 2 1 4
```Vị trí lẻ chứa`[3,1]`, các vị trí chẵn chứa`[2,4]`. 

Sắp xếp cả hai nhóm cho:```
odd_sorted = [1,3]
even_sorted = [2,4]
```Các hoán vị chẵn lẻ được yêu cầu khác nhau, vì vậy sự sắp xếp hợp nhất này không thể truy cập được. 

Thuật toán hoán đổi hai phần tử cuối cùng của một nhóm được sắp xếp. Ứng viên tốt hơn sẽ trở thành:```
1 4 3 2
```đó chính xác là mức tối ưu có thể đạt được. 

Bây giờ hãy xem xét:```
5
5 4 3 1 2
```Các nhóm chẵn và lẻ được sắp xếp đã có tính chẵn lẻ phù hợp. Không cần chỉnh sửa. Các đầu ra thuật toán:```
2 1 3 4 5
```đó là từ điển nhỏ nhất trong số tất cả các hoán vị có thể tiếp cận. 

Cuối cùng, hãy xem xét trường hợp một nhóm chỉ có hai phần tử:```
4
4 1 3 2
```Hoạt động sửa lỗi chẵn lẻ sẽ hoán đổi hai phần tử đó nếu cần thiết. Thuật toán không bao giờ truy cập vào vị trí ngoài phạm vi vì mỗi nhóm có độ dài ít nhất là hai khi$n \ge 4$, chính xác như được đảm bảo bởi các ràng buộc.
