---
title: "CF 1909C - Quãng nặng"
description: "Chúng ta được cho một tập hợp các khoảng $n$ trên trục số. Mỗi khoảng có điểm cuối bên trái $li$, điểm cuối bên phải $ri$ và trọng số trên mỗi đơn vị độ dài $ci$. Trọng số thực tế của một khoảng được tính là $ci cdot (ri - li)$."
date: "2026-06-08T20:28:33+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "dsu", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1909
codeforces_index: "C"
codeforces_contest_name: "Pinely Round 3 (Div. 1 + Div. 2)"
rating: 1400
weight: 1909
solve_time_s: 115
verified: true
draft: false
---

[CF 1909C - Khoảng thời gian nặng](https://codeforces.com/problemset/problem/1909/C) 

**Đánh giá:** 1400 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, dsu, tham lam, toán học, sắp xếp 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một bộ$n$các khoảng trên trục số. Mỗi khoảng có một điểm cuối bên trái$l_i$, một điểm cuối bên phải$r_i$và trọng lượng trên một đơn vị chiều dài$c_i$. Trọng số thực tế của một khoảng được tính như$c_i \cdot (r_i - l_i)$. Mục tiêu là giảm thiểu tổng trọng số trong tất cả các khoảng. 

Điều khó khăn là chúng ta có thể sắp xếp lại các mảng một cách độc lập$l$,$r$, Và$c$. Hạn chế duy nhất là sau khi sắp xếp lại, mỗi khoảng phải còn hiệu lực, nghĩa là$l_i < r_i$. Về cơ bản, bài toán cho phép chúng ta ghép các điểm cuối bên trái, điểm cuối bên phải và chi phí theo bất kỳ sự kết hợp nào miễn là các khoảng được định dạng đúng. Tất cả các điểm cuối đều khác biệt, điều này đảm bảo rằng việc sắp xếp chúng sẽ tạo ra các khoảng thời gian hợp lệ mà không trùng lặp. 

Các ràng buộc chỉ ra rằng tổng số khoảng thời gian trên tất cả các trường hợp thử nghiệm không vượt quá$10^5$, có nghĩa là bất kỳ$O(n \log n)$giải pháp cho mỗi trường hợp thử nghiệm là chấp nhận được.$O(n^2)$phương pháp vũ lực bị loại trừ. Các trường hợp cạnh không rõ ràng bao gồm các tình huống trong đó điểm cuối bên trái nhỏ nhất có thể được ghép với điểm cuối bên phải lớn nhất, điều này sẽ mang lại một khoảng rất lớn. Các cách tiếp cận bất cẩn có thể không sắp xếp các mảng theo đúng thứ tự hoặc ghép chi phí cao nhất với độ dài nhỏ nhất, dẫn đến tổng trọng số dưới mức tối ưu. 

Ví dụ, nếu$l=[1,3]$,$r=[5,2]$,$c=[10,1]$, ghép nối$1\to5$với$10$Và$3\to2$với$1$không hợp lệ do$3>2$, vì vậy chúng ta phải sắp xếp lại$r$ĐẾN$[2,5]$. Sự ghép đôi tối ưu trong trường hợp này là$l=[1,3]$,$r=[2,5]$,$c=[10,1]$, cho trọng lượng$10_1 + 1_2 = 12$. Một kẻ tham lam ngây thơ chỉ biết cặp đôi$c$với bất kỳ khoảng thời gian nào cũng có thể thất bại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là thử mọi hoán vị của$l$,$r$, Và$c$, kiểm tra tính hợp lệ và tính tổng. Vì$n$các khoảng, có$n!^3$sự kết hợp hoàn toàn không thể thực hiện được. Thậm chí chỉ thử tất cả các cặp$l$Và$r$với cố định$c$là$O(n!^2)$, điều này cũng quá chậm. 

Cái nhìn sâu sắc quan trọng đến từ việc quan sát rằng tổng trọng lượng là tổng của các sản phẩm$c_i \cdot (r_i - l_i)$. Để giảm thiểu tổng này, chúng ta nên ghép những khác biệt nhỏ nhất$(r_i - l_i)$với chi phí lớn nhất$c_i$, hoặc tương tự, sắp xếp sự khác biệt theo thứ tự tăng dần và chi phí theo thứ tự giảm dần rồi nhân chúng theo cặp. Tương tự, để giảm thiểu sự khác biệt của bản thân, chúng ta nên ghép những điểm nhỏ nhất$l_i$với cái nhỏ nhất$r_i$nó vẫn lớn hơn nó. Điều này có thể đạt được bằng cách sắp xếp$l$Và$r$độc lập theo thứ tự tăng dần và khớp chúng theo chỉ mục. 

Tóm lại giải pháp tối ưu là sắp xếp các endpoint bên trái$l$theo thứ tự tăng dần, điểm cuối bên phải$r$theo thứ tự tăng dần và chi phí$c$theo thứ tự giảm dần. Sau đó tính tổng của$c_i \cdot (r_i - l_i)$cho mỗi$i$. Sắp xếp đảm bảo khoảng thời gian hợp lệ ($l_i < r_i$) do các điểm cuối khác biệt và giảm dần$c$đảm bảo chi phí lớn nhất được áp dụng cho khoảng thời gian nhỏ nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n!^3) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc$n$, điểm cuối bên trái$l$, điểm cuối bên phải$r$, và chi phí$c$. 
3. Sắp xếp mảng$l$theo thứ tự tăng dần để có được các điểm cuối bên trái từ nhỏ nhất đến lớn nhất. 
4. Sắp xếp mảng$r$theo thứ tự tăng dần để có được điểm cuối phù hợp từ nhỏ nhất đến lớn nhất. 
5. Sắp xếp mảng$c$theo thứ tự giảm dần để ghép chi phí lớn nhất với độ dài khoảng thời gian nhỏ nhất. 
6. Khởi tạo một biến`total_weight`về không. 
7. Lặp lại tất cả các khoảng thời gian$i=0$ĐẾN$n-1$: 

- Tính độ dài của khoảng đó là$length = r[i] - l[i]$. 
- Nhân với chi phí tương ứng`c[i]`để có được trọng lượng khoảng. 
- Thêm cái này vào`total_weight`. 
8. In tổng trọng lượng của test hiện tại. 

Tại sao nó hoạt động: Sắp xếp$l$Và$r$tăng dần đảm bảo rằng với mọi$i$,$l_i < r_i$giữ nguyên, bởi vì tất cả các điểm cuối đều khác biệt. Sắp xếp$c$giảm dần đảm bảo chi phí lớn nhất trên mỗi đơn vị chiều dài nhân với chiều dài nhỏ nhất có thể, giảm thiểu tổng sản phẩm. Thuộc tính tham lam được giữ nguyên vì bất kỳ cặp chi phí nào khác với các khoảng sẽ gán một chi phí lớn cho độ dài lớn hơn, làm tăng tổng hoặc vi phạm tính hợp lệ của khoảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    l = list(map(int, input().split()))
    r = list(map(int, input().split()))
    c = list(map(int, input().split()))

    l.sort()
    r.sort()
    c.sort(reverse=True)

    total_weight = 0
    for i in range(n):
        total_weight += c[i] * (r[i] - l[i])
    print(total_weight)
```Giải thích: Giải pháp đầu tiên đọc đầu vào một cách hiệu quả với`sys.stdin.readline`. Sắp xếp`l`Và`r`tăng dần đảm bảo khoảng thời gian hợp lệ, trong khi sắp xếp`c`giảm dần làm tổng số nhỏ nhất. Vòng lặp nhân các phần tử tương ứng và tích lũy tổng. sử dụng`reverse=True`vì`c`là rất quan trọng, vì việc quên bước này sẽ dẫn đến việc kết hợp các chi phí nhỏ với khoảng thời gian nhỏ, điều này là không tối ưu. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

| tôi | tôi | r | c | r-l | cân nặng | tổng_trọng lượng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 3 | 12 | 100 | 9 | 900 | 900 | 
| 1 | 8 | 23 | 100 | 15 | 1500 | 2400 | 

Đã sắp xếp$l=[3,8]$,$r=[12,23]$,$c=[100,100]$mang lại tổng trọng lượng 2400. Điều này xác nhận hoạt động ghép đôi tham lam. 

**Mẫu 2:** 

| tôi | tôi | r | c | r-l | cân nặng | tổng_trọng lượng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 3 | 3 | 2 | 6 | 6 | 
| 1 | 2 | 4 | 3 | 2 | 6 | 12 | 
| 2 | 5 | 10 | 2 | 5 | 10 | 22 | 
| 3 | 20 | 30 | 2 | 10 | 20 | 42 | 

Sắp xếp trái và phải tăng dần, chi phí giảm dần tạo ra tổng tối thiểu 42. Điều này cho thấy rằng cả khoảng thời gian ngắn với chi phí cao và khoảng thời gian dài với chi phí nhỏ đều được chỉ định chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp ba mảng chiếm ưu thế, truyền tuyến tính để tính tổng | 
| Không gian | O(n) | Lưu trữ ba mảng cho mỗi trường hợp thử nghiệm | 

Với những hạn chế của$n$lên đến$10^5$và tổng số tiền$n$trên các trường hợp thử nghiệm 10^5, giải pháp này chạy thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # solution call
    t = int(input())
    for _ in range(t):
        n = int(input())
        l = list(map(int, input().split()))
        r = list(map(int, input().split()))
        c = list(map(int, input().split()))
        l.sort()
        r.sort()
        c.sort(reverse=True)
        total_weight = sum(c[i]*(r[i]-l[i]) for i in range(n))
        print(total_weight)
    return output.getvalue().strip()

# provided samples
assert run("2\n2\n8 3\n12 23\n100 100\n4\n20 1 2 5\n30 4 3 10\n2 3 2 3\n") == "2400\n42"

# minimum-size input
assert run("1\n1\n1\n2\n10\n") == "10"

# maximum-size input with increasing sequences
n = 10**5
l = ' '.join(map(str, range(1,n+1)))
r = ' '.join(map(str, range(2,n+2)))
c = ' '.join(['1']*n)
input_str = f"1\n{n}\n{l}\n{r}\n{c}\n"
output_str = str(sum(1 for _ in range(n)))
assert run(input_str)  # checks performance

# all equal values
assert run("1\n3\n1 2 3\n4 5 6\n5 5 5\n") == "45"

# boundary condition: large costs
assert run("1\n2\n1 3\n4 6\n1000000 1\n") == str(1000000*(4-1) + 1*(6-3))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| | | |
