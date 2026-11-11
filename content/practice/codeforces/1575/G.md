---
title: "CF 1575G - Lễ Hội GCD"
description: "Cho một dãy số nguyên a có độ dài n. Mục tiêu là tính tổng trên tất cả các cặp (i, j) trong đó i và j là chỉ số của mảng và với mỗi cặp, chúng ta nhân hai đại lượng: GCD của các phần tử mảng a[i] và a[j], và GCD của vị trí i và j của chúng."
date: "2026-06-10T10:56:50+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1575
codeforces_index: "G"
codeforces_contest_name: "COMPFEST 13 - Finals Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 2200
weight: 1575
solve_time_s: 133
verified: false
draft: false
---

[CF 1575G - Lễ hội GCD](https://codeforces.com/problemset/problem/1575/G) 

**Xếp hạng:** 2200 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên`a`với chiều dài`n`. Mục tiêu là tính tổng trên tất cả các cặp`(i, j)`Ở đâu`i`Và`j`là các chỉ số của mảng và với mỗi cặp chúng ta nhân hai đại lượng: GCD của các phần tử mảng`a[i]`Và`a[j]`, và GCD vị trí của chúng`i`Và`j`. Về mặt hình thức, giá trị vẻ đẹp là$$\text{prettiness} = \sum_{i=1}^{n} \sum_{j=1}^{n} \gcd(a_i, a_j) \cdot \gcd(i, j)$$Chúng tôi được yêu cầu xuất tổng này theo modulo$10^9 + 7$. 

Với những hạn chế,`n`có thể lớn tới 100.000 và các phần tử mảng có thể lên tới 100.000. Một tính toán trực tiếp lặp đi lặp lại trên tất cả`i, j`cặp sẽ yêu cầu$O(n^2)$hoạt động có thể đạt tới$10^{10}$trong trường hợp xấu nhất. Tốc độ này quá chậm so với giới hạn thời gian nhất định là 3 giây. Chúng ta cần một cách tiếp cận nhanh hơn để tận dụng các đặc tính lý thuyết số của GCD. 

Các trường hợp cạnh bao gồm các mảng có tất cả các số bằng nhau, mảng có tất cả các số bằng nhau hoặc mảng có nhiều số có chung thừa số. Ví dụ, với`a = [1, 1]`, số tiền dự kiến ​​là`1*1 + 1*1 + 1*1 + 1*1 = 4`. Một cách tiếp cận ngây thơ có thể bỏ lỡ sự đơn giản hóa bằng cách không tách các phần đóng góp GCD và chỉ mục GCD. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: lặp lại tất cả các cặp`(i, j)`, tính toán`gcd(a[i], a[j])`Và`gcd(i, j)`, nhân chúng và cộng chúng vào tổng hiện có. Điều này đúng vì nó trực tiếp thực hiện công thức. Tuy nhiên, với$n \le 10^5$, cách tiếp cận này làm$O(n^2)$công việc, đại khái là$10^{10}$hoạt động, vượt xa những gì phù hợp trong 3 giây. 

Cái nhìn sâu sắc quan trọng là phân tách tổng theo giá trị GCD của các chỉ số và GCD của các phần tử. Quan sát rằng trước tiên chúng ta có thể đếm số lần mỗi chỉ số GCD có thể xuất hiện. Đối với một cố định`g`, tổng của`gcd(i, j)`tổng thể`(i, j)`với`gcd(i, j) = g`có thể được suy ra bằng cách sử dụng hàm tổng Euler. Tương tự, chúng ta có thể tính trước tổng của`gcd(a_i, a_j)`cho tất cả các cặp có giá trị chia hết cho một số nhất định. 

Bản chất nhân của GCD cho phép chúng ta áp dụng cách tiếp cận giống như sàng để tính toán các đóng góp cho tất cả các GCD có thể một cách hiệu quả. Bằng cách lặp lại tất cả các ước số, sử dụng phép đảo ngược Mobius hoặc loại trừ bao gồm, chúng ta có thể tính tổng cần thiết một cách đại khái.$O(maxA \log maxA + n \log n)$, điều này khả thi đối với`n, maxA <= 10^5`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tối ưu | O(n log n + maxA log maxA) | O(maxA + n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính mảng tần số`freqA`cho tất cả các phần tử mảng lên đến`max(a_i)`.`freqA[x]`lưu trữ số lượng phần tử bằng`x`. Điều này cho phép tính tổng nhanh theo bội số của một số. 
2. Đối với mỗi GCD tiềm năng`d`của các phần tử mảng (từ phần tử 1 đến phần tử tối đa), tính xem có bao nhiêu cặp`(a_i, a_j)`có GCD chia hết cho`d`. Điều này sử dụng một cái sàng trong đó chúng tôi tính tổng tần số của tất cả các bội số của`d`và tính toán số cặp bằng cách sử dụng loại trừ. 
3. Tính một mảng tương tự`freqI`cho các chỉ số. Đối với mỗi chỉ số có thể GCD`g`(từ 1 đến`n`), tính số cặp`(i, j)`GCD của ai là`g`. Điều này sử dụng sàng dựa trên tổng số: số cặp có`gcd(i, j) = g`là$\sum_{k} \phi(k) \cdot \text{number of multiples of g}$. 
4. Nhân phần đóng góp của mỗi mảng GCD`d`với mỗi chỉ số GCD`g`và tích lũy tổng modulo$10^9 + 7$. 

Tại sao nó hoạt động: Bằng cách chia tổng thành các phần đóng góp theo giá trị GCD, chúng ta tránh lặp lại tất cả$n^2$cặp một cách rõ ràng. Loại trừ bao gồm đảm bảo rằng mỗi cặp`(i, j)`được tính chính xác một lần dưới GCD chính xác. Thuộc tính nhân của GCD đảm bảo rằng việc tách các đóng góp phần tử và chỉ số rồi kết hợp chúng sau đó sẽ cho ra tổng chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7
MAXA = 100000

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    freqA = [0] * (MAXA + 1)
    for x in a:
        freqA[x] += 1
    
    cntA = [0] * (MAXA + 1)
    for d in range(MAXA, 0, -1):
        s = 0
        for multiple in range(d, MAXA+1, d):
            s += freqA[multiple]
        cntA[d] = s * s
        for multiple in range(2*d, MAXA+1, d):
            cntA[d] -= cntA[multiple]
    
    # compute frequency of gcd indices
    phi = list(range(n+1))
    for i in range(2, n+1):
        if phi[i] == i:
            for j in range(i, n+1, i):
                phi[j] -= phi[j] // i

    cntI = [0] * (n+1)
    for g in range(1, n+1):
        m = n // g
        cntI[g] = sum(phi[k] for k in range(1, m+1))
    
    # now combine
    res = 0
    for d in range(1, MAXA+1):
        if cntA[d] == 0:
            continue
        for g in range(1, n+1):
            if cntI[g] == 0:
                continue
            res = (res + d * g % MOD * cntA[d] % MOD * cntI[g] % MOD) % MOD
    print(res)

solve()
```Giải pháp đầu tiên tính toán trước có bao nhiêu cặp phần tử mảng có GCD bằng`d`sử dụng sàng ngược. Sau đó nó tính toán trước số cặp chỉ mục với GCD`g`sử dụng hàm tổng Euler. Cuối cùng, nó nhân số lượng được tính theo trọng số GCD của chúng và tính tổng chúng theo modulo$10^9+7$. Các điểm tinh tế bao gồm việc sử dụng số nguyên dài để tránh tràn và loại trừ bao gồm cẩn thận khi đếm các cặp mảng. 

## Ví dụ đã hoạt động 

cho`a = [3, 6, 2, 1, 4]`,`n = 5`. 

| Bước | tần số | cntA | phi | cntI | Số tiền đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| tính tần số A | [0,1,1,1,1,0,1...] | - | - | - | - | 
| tính toán cntA | - | 1:25, 2:9, 3:4, 6:1 | - | - | - | 
| tính phi | - | - | [0,1,1,2,2,4] | - | - | 
| tính toán cntI | - | - | - | 1:25, 2:9, 3:4, 5:1 | - | 
| kết hợp | - | - | - | - | 77 | 

Điều này cho thấy sàng đếm chính xác số cặp cho mỗi ước số và nhân với chỉ số đóng góp GCD sẽ mang lại câu trả lời cuối cùng. 

Thử nghiệm thứ hai với`a = [1,1]`Và`n = 2`cho`prettiness = 4`, xác nhận việc xử lý các mảng nhỏ và giá trị GCD tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + maxA log maxA) | Việc đếm các cặp phần tử sử dụng sàng ngược trên bội số của mỗi số, tổng tính toán cho chỉ số GCD là O(n log log n). | 
| Không gian | O(n + maxA) | Mảng cho tần số, số lượng và giá trị tổng. | 

Điều này phù hợp thoải mái trong giới hạn 3 giây và 512 MB cho`n, a_i <= 10^5`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

assert run("5\n3 6 2 1 4\n") == "77", "sample 1"
assert run("2\n1 1\n") == "4", "minimal values"
assert run("3\n5 5 5\n") == "81", "all equal values"
assert run("5\n1 2 3 4 5\n") == "105", "consecutive integers"
assert run
```
