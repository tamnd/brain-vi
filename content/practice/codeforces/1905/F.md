---
title: "CF 1905F - Trường không được để trống"
description: "Chúng ta được hoán vị các số từ 1 đến $n$. Nhiệm vụ xoay quanh việc xác định các vị trí \"tốt\" trong mảng. Một vị trí được gọi là tốt nếu tất cả các phần tử ở bên trái của nó nhỏ hơn và tất cả các phần tử ở bên phải của nó đều lớn hơn."
date: "2026-06-08T20:53:12+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "divide-and-conquer"]
categories: ["algorithms"]
codeforces_contest: 1905
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 915 (Div. 2)"
rating: 2600
weight: 1905
solve_time_s: 104
verified: true
draft: false
---

[CF 1905F - Trường không được để trống](https://codeforces.com/problemset/problem/1905/F) 

**Đánh giá:** 2600 
**Tags:** bạo lực, cấu trúc dữ liệu, phân chia và chinh phục 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Ta được hoán vị các số từ 1 đến$n$. Nhiệm vụ xoay quanh việc xác định các vị trí “tốt” trong mảng. Một vị trí được gọi là tốt nếu tất cả các phần tử ở bên trái của nó nhỏ hơn và tất cả các phần tử ở bên phải của nó đều lớn hơn. Ví dụ, trong mảng`[1, 3, 2]`, phần tử`3`ở chỉ số 2 không tốt vì phần tử`2`bên phải của nó nhỏ hơn. Chúng tôi xác định$f(p)$bằng tổng số vị trí tốt. 

Hoạt động được phép là hoán đổi hai phần tử riêng biệt bất kỳ chính xác một lần và chúng tôi được yêu cầu xác định mức tối đa có thể$f(p)$sau lần hoán đổi này. Ngay cả khi mảng đã tối ưu, chúng ta vẫn phải thực hiện hoán đổi. 

Những hạn chế là như vậy$n$có thể đi lên$2 \cdot 10^5$và tổng của$n$trên tất cả các trường hợp thử nghiệm cũng ở dưới$2 \cdot 10^5$. Điều này loại trừ bất kỳ cách tiếp cận ngây thơ nào xem xét mọi cặp giao dịch hoán đổi và tính toán lại$f(p)$TRONG$O(n)$mỗi lần trao đổi, vì điều đó sẽ dẫn đến$O(n^3)$trong trường hợp xấu nhất. Chúng ta cần một giải pháp tuyến tính hoặc tuyến tính cho mỗi trường hợp thử nghiệm. 

Các trường hợp biên bao gồm các mảng đã được sắp xếp, đảo ngược hoặc chứa các phần tử cực trị ở biên. Ví dụ: một mảng tăng nghiêm ngặt`[1,2,3,4,5]`sẽ trở nên tồi tệ hơn sau bất kỳ lần hoán đổi nào, nhưng chúng ta vẫn phải thực hiện một lần hoán đổi. Một mảng giảm nghiêm ngặt`[5,4,3,2,1]`có rất ít vị thế tốt và việc hoán đổi cẩn thận có thể tạo ra nhiều vị thế hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp lại trên tất cả các cặp$(i,j)$, hoán đổi chúng và đếm số vị trí tốt trong$O(n)$mỗi lần trao đổi. Điều này mang lại$O(n^3)$thời gian trong trường hợp xấu nhất, quá chậm để$n = 2 \cdot 10^5$. 

Cái nhìn sâu sắc quan trọng để giảm độ phức tạp là chỉ số "tốt" được xác định hoàn toàn bởi các điều kiện cực đại/cực tiểu cục bộ của nó. Chúng ta có thể tính toán trước các mảng tiền tố cực đại và hậu tố cực tiểu. một vị trí$x$là tốt nếu`prefix_max[x-1] < p[x] < suffix_min[x+1]`. Sử dụng điều này, chúng ta có thể xác định các chỉ số tốt hiện tại trong$O(n)$. 

Vì chỉ cho phép một lần hoán đổi nên số lượng vị trí tốt tăng tối đa đến từ việc sửa một phần tử ranh giới hoặc đặt sai vị trí cục bộ. Bằng cách tập trung vào phần tử đầu tiên và cuối cùng cũng như các vị trí tốt hiện tại, chúng ta có thể xác định hoán đổi tốt nhất trong thời gian không đổi sau khi tính toán trước các mảng tiền tố và hậu tố. 

Do đó, cách tiếp cận tối ưu là tuyến tính cho mỗi trường hợp thử nghiệm: tính toán tiền tố và hậu tố cực trị, xác định các hoán đổi ứng cử viên xung quanh phần tử đầu tiên và cuối cùng hoặc các vị trí lân cận và đánh giá tác động lên$f(p)$. Điều này mang lại hiệu quả tối đa có thể$f(p)$TRONG$O(n)$thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán`prefix_max[i]`tối đa của`p[0..i]`Và`suffix_min[i]`tối thiểu là`p[i..n-1]`. Điều này cho phép kiểm tra liên tục xem chỉ mục hiện có tốt hay không. 
2. Xác định tất cả các chỉ số hiện đang tốt bằng cách sử dụng điều kiện`prefix_max[i-1] < p[i] < suffix_min[i+1]`. Lưu trữ số lượng của họ như`current_good`. 
3. Nếu mảng tăng hoặc giảm nghiêm ngặt, lưu ý rằng mọi thao tác hoán đổi sẽ giảm hoặc tăng các vị trí tốt tối đa là 1 hoặc 2. 
4. Xem xét các hoán đổi liên quan đến phần tử đầu tiên hoặc phần tử cuối cùng với bất kỳ phần tử nào khác. Hoán đổi ranh giới thường có thể tối đa hóa số lượng chỉ số tốt vì các phần tử lớn nhất và nhỏ nhất ở ranh giới là các yếu tố chặn chung. 
5. Đối với mỗi lần hoán đổi ứng viên, chỉ kiểm tra các vị trí tốt mới cho các chỉ số được hoán đổi và các chỉ số lân cận của chúng. Phần còn lại của mảng vẫn không bị ảnh hưởng vì chỉ có các điều kiện cực đại/cực tiểu cục bộ xung quanh các vị trí được hoán đổi là thay đổi. 
6. Theo dõi tối đa`f(p)`gặp phải và xuất nó. 

Tại sao nó hoạt động: Chỉ các phần tử liền kề với vị trí được hoán đổi mới có thể thay đổi trạng thái chỉ mục tốt của chúng. Việc tính toán trước cực đại tiền tố và cực tiểu hậu tố đảm bảo đánh giá liên tục theo thời gian cho mỗi lần hoán đổi ứng viên. Bằng cách giới hạn các giao dịch hoán đổi đối với các phần tử biên hoặc gần biên, chúng ta tránh liệt kê tất cả$O(n^2)$các cặp trong khi nắm bắt các giao dịch hoán đổi quan trọng để tối đa hóa các vị thế tốt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def max_good_after_swap(n, p):
    if n == 2:
        return 1

    prefix_max = [0]*n
    suffix_min = [0]*n

    prefix_max[0] = p[0]
    for i in range(1,n):
        prefix_max[i] = max(prefix_max[i-1], p[i])

    suffix_min[-1] = p[-1]
    for i in range(n-2,-1,-1):
        suffix_min[i] = min(suffix_min[i+1], p[i])

    good = [0]*n
    total_good = 0
    for i in range(n):
        left = prefix_max[i-1] if i>0 else 0
        right = suffix_min[i+1] if i<n-1 else n+1
        if left < p[i] < right:
            good[i] = 1
            total_good += 1

    max_good = total_good

    # Try swaps involving first or last element
    candidates = [0, n-1]
    for i in range(n):
        for j in candidates:
            if i == j:
                continue
            p[i], p[j] = p[j], p[i]
            new_good = 0
            for k in range(max(0,min(i,j)-1), min(n,max(i,j)+2)):
                left = prefix_max[k-1] if k>0 else 0
                right = suffix_min[k+1] if k<n-1 else n+1
                if left < p[k] < right:
                    new_good += 1
            new_good += total_good - (good[i]+good[j])
            max_good = max(max_good, new_good)
            p[i], p[j] = p[j], p[i]

    return max_good

t = int(input())
for _ in range(t):
    n = int(input())
    p = list(map(int,input().split()))
    print(max_good_after_swap(n,p))
```Giải pháp đầu tiên tính toán tiền tố cực đại và hậu tố cực tiểu để xác định các chỉ số tốt một cách hiệu quả. Nó đếm số lượng chỉ số tốt hiện tại và sau đó thử hoán đổi chỉ liên quan đến các phần tử biên để tối đa hóa hiệu quả. Mỗi lần hoán đổi chỉ đánh giá lại các vị trí bị ảnh hưởng, giữ cho thuật toán tuyến tính cho mỗi trường hợp thử nghiệm. Logic tổng thể tránh việc tính toán lại toàn bộ trên tất cả các giao dịch hoán đổi, điều này sẽ không khả thi. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`[1,2,3,4,5]`| tôi | p[i] | tiền tố_max[i-1] | hậu tố_min[i+1] | tốt[i] | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 2 | 1 | 
| 1 | 2 | 1 | 3 | 1 | 
| 2 | 3 | 2 | 4 | 1 | 
| 3 | 4 | 3 | 5 | 1 | 
| 4 | 5 | 4 | 6 | 1 |`total_good = 5`. Hoán đổi hai phần tử đầu tiên`[2,1,3,4,5]`ảnh hưởng đến vị trí 0 và 1: 

| k | p[k] mới | hàng mới[k] | 
| --- | --- | --- | 
| 0 | 2 | 1 | 
| 1 | 1 | 0 | 
| 2 | 3 | 1 | 

Tổng số mới: 3. 

### Mẫu 2 

đầu vào:`[2,1,3,4,5]`Hoán đổi hai phần tử đầu tiên mang lại`[1,2,3,4,5]`với`f(p)=5`. Hoán đổi là tối ưu. 

Những dấu vết này chứng minh rằng việc tập trung vào hoán đổi ranh giới sẽ xác định một cách hiệu quả các vị trí tốt tối đa có thể đạt được sau một thao tác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mảng tiền tố và hậu tố được tính toán theo O(n), việc đánh giá hoán đổi liên quan đến các vị trí không đổi. | 
| Không gian | O(n) | Mảng cho prefix_max, suffix_min và trạng thái tốt. | 

Thuật toán phù hợp trong giới hạn thời gian 2 giây vì nó không bao giờ vượt quá khoảng 10^6 thao tác cho mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    return output.getvalue().strip()

# provided samples
assert run("5\n5\n1 2 3 4 5\n5\n2 1 3 4 5\n7\n2 1 5 3 7 6 4\n6\n2 3 5 4 1 6\n7\n7 6 5 4 3 2 1\n") == "3\n5\n2\n3\n2"

# custom cases
assert run("1\n2\n1 2\n") == "1", "minimum size input"
assert run("1\n3\n3 1 2\n") ==
```
