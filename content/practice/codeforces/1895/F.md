---
title: "CF 1895F - Mảng ưa thích"
description: "Chúng tôi đang đếm các mảng có độ dài n bao gồm các số nguyên không âm. Các giá trị liền kề có thể di chuyển tối đa k theo một trong hai hướng. Một mảng được coi là ưa thích nếu có ít nhất một phần tử thuộc khoảng $$[x, x+k-1].$$ Nhiệm vụ là đếm tất cả các mảng như vậy theo modulo $10^9+7$."
date: "2026-06-08T21:46:31+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "math", "matrices"]
categories: ["algorithms"]
codeforces_contest: 1895
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 157 (Rated for Div. 2)"
rating: 2600
weight: 1895
solve_time_s: 139
verified: true
draft: false
---

[CF 1895F - Mảng ưa thích](https://codeforces.com/problemset/problem/1895/F) 

**Đánh giá:** 2600 
**Tags:** tổ hợp, dp, toán, ma trận 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang đếm các mảng có chiều dài`n`gồm các số nguyên không âm. Các giá trị liền kề có thể di chuyển nhiều nhất`k`theo một trong hai hướng. 

Một mảng được coi là ưa thích nếu có ít nhất một phần tử thuộc khoảng$$[x, x+k-1].$$Nhiệm vụ là đếm tất cả các mảng như vậy theo modulo$10^9+7$. 

Điều đầu tiên có vẻ bất thường là các giá trị mảng ở trên không bị giới hạn. Lập trình động trực tiếp trên các giá trị là không thể vì các giá trị có thể lớn tùy ý. 

Các ràng buộc giải thích cấu trúc thực sự nằm ở đâu. Cả hai`n`Và`k`có thể đạt được$10^9$, do đó, mọi điều tuyến tính trong một trong hai tham số đều không thể thực hiện được ngay lập tức. Tham số nhỏ duy nhất là`x`, nhiều nhất là`40`. Bất kỳ lời giải nào được chấp nhận đều phải quy bài toán về không gian trạng thái tùy thuộc vào`x`và sau đó sử dụng lũy ​​thừa ma trận để xử lý độ dài lớn. 

Một điểm tinh tế là tập hợp tất cả các mảng chỉ thỏa mãn điều kiện kề là vô hạn, bởi vì các giá trị có thể dịch chuyển lên trên mà không bị giới hạn. Yêu cầu một phần tử nào đó nằm trong$[x,x+k-1]$là những gì làm cho câu trả lời hữu hạn. 

Một sai lầm dễ mắc phải khác là thử đếm trực tiếp các mảng chứa giá trị từ khoảng mục tiêu. Khoảng thời gian có thể được truy cập nhiều lần, do đó việc loại trừ bao gồm các vị trí trở nên lộn xộn. 

Coi như:```
n = 2, x = 1, k = 1
```Các mảng hợp lệ là:```
[0,1]
[1,0]
[1,1]
[1,2]
[2,1]
```Có 5 người trong số họ. Việc đếm riêng các vị trí "lần truy cập đầu tiên" sẽ tính gấp đôi một số mảng. 

Cái bẫy thứ hai xuất hiện khi`x = 0`.```
n = 1, x = 0, k = 3
```Mỗi giá trị trong`{0,1,2}`đã đáp ứng được yêu cầu nên câu trả lời là`3`. 

Bất kỳ công thức nào trừ các mảng hoàn toàn bên dưới`x`phải xử lý chính xác thực tế là không có giá trị nào bên dưới`0`. 

## Phương pháp tiếp cận 

Đầu tiên chúng ta hãy tưởng tượng một giải pháp vũ phu. 

Giả sử các giá trị bị giới hạn ở một phạm vi hữu hạn nào đó. Chúng ta có thể liệt kê mọi mảng và kiểm tra xem các khác biệt liền kề có nằm trong phạm vi không`k`và liệu có ít nhất một phần tử thuộc khoảng mục tiêu hay không. Điều này rõ ràng là đúng, nhưng ngay cả đối với những phạm vi nhỏ, số lượng mảng vẫn tăng theo cấp số nhân khi$M^n$. Với$n$lên đến$10^9$, cách tiếp cận như vậy là vô vọng. 

Quan sát chính xuất phát từ việc xem xét giá trị tối thiểu của mảng. 

Lấy mảng bất kỳ thỏa mãn điều kiện kề. Đặt giá trị nhỏ nhất của nó là`m`. 

Nếu chúng ta trừ`m`từ mọi yếu tố, mọi sự khác biệt vẫn không thay đổi. Mảng dịch chuyển có giá trị nhỏ nhất`0`. 

Bây giờ hãy xem thứ tự tăng dần giữa các phần tử liên tiếp. Mỗi bước có thể là số nguyên bất kỳ trong`[-k,k]`, đưa ra chính xác`2k+1`khả năng. 

Khi giá trị tối thiểu`m`và trình tự bước được cố định, toàn bộ mảng được xác định duy nhất. 

Điều này gợi ý việc đếm mảng theo mức tối thiểu của chúng. 

Một mảng rất lạ mắt nếu giá trị tối thiểu của nó nhiều nhất là$x+k-1$và không phải tất cả các phần tử đều ở mức dưới đây`x`. 

Thật vậy, nếu mức tối thiểu vượt quá$x+k-1$, thì mọi phần tử đều vượt quá$x+k-1$, do đó khoảng mục tiêu không bao giờ đạt được. 

Ngược lại, nếu mức tối thiểu là nhiều nhất$x+k-1$, khi đó một số phần tử bằng mức tối thiểu và do đó nằm trong$[0,x+k-1]$. Cách duy nhất để bỏ lỡ khoảng mục tiêu là khi mọi phần tử thực sự ở dưới`x`. 

Điều này biến câu trả lời thành$$(\text{arrays with minimum } \le x+k-1) - (\text{arrays entirely in } [0,x-1]).$$Số lượng đầu tiên trở nên cực kỳ đơn giản. 

Chọn giá trị tối thiểu. Nó có thể là bất kỳ số nguyên nào từ`0`ĐẾN`x+k-1`, cho`x+k`sự lựa chọn. 

Sau khi ấn định mức tối thiểu, mỗi`n-1`sự khác biệt có thể được lựa chọn độc lập từ`[-k,k]`, cho$$(2k+1)^{n-1}$$khả năng. 

Kể từ đây$$A=(x+k)(2k+1)^{n-1}.$$Đại lượng thứ hai chỉ liên quan đến các giá trị trong`[0,x-1]`. Từ`x ≤ 40`, chúng ta có thể thực hiện DP trên các trạng thái này. 

Cho phép$$dp_i[j]$$là số độ dài-`i`mảng kết thúc bằng giá trị`j`, trong đó tất cả các giá trị đều ở bên trong`[0,x-1]`. 

Sự chuyển tiếp là$$dp_i[j] = \sum_{|j-t|\le k} dp_{i-1}[t].$$Đây là một phép biến đổi tuyến tính trên tối đa 40 trạng thái, vì vậy phép lũy thừa ma trận sẽ tính kết quả rất lớn.`n`. 

Câu trả lời là$$(x+k)(2k+1)^{n-1} - \text{count\_below\_x}.$$Điều này mang lại một$O(x^3\log n)$giải pháp. 

Ý tưởng cơ bản chính xác là ý tưởng được sử dụng trong các giải pháp chính thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu |$O(x^3\log n)$|$O(x^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán$$T=(x+k)(2k+1)^{n-1}.$$Điều này đếm tất cả các mảng có giá trị tối thiểu thuộc về`[0, x+k-1]`. 
2. Đếm các mảng có mọi giá trị nằm trong`[0,x-1]`. 

Hãy để các trạng thái là các giá trị`0,1,...,x-1`. 
3. Xây dựng một`x × x`ma trận chuyển tiếp`M`.`M[i][j] = 1`nếu như`|i-j| ≤ k`, nếu không thì`0`. 

Điều này khớp chính xác với ràng buộc kề. 
4. Tạo vector hàng đầu tiên$$V=[1,1,\dots,1].$$Mỗi giá trị trong`[0,x-1]`có thể xuất hiện dưới dạng phần tử đầu tiên. 
5. Tính toán$$V' = V \cdot M^{\,n-1}.$$Phép lũy thừa ma trận làm giảm số mũ lớn thành$O(\log n)$. 
6. Tính tổng tất cả các mục của`V'`. 

Đây là số mảng có giá trị không bao giờ rời khỏi`[0,x-1]`. 
7. Trừ số lượng đó khỏi`T`. 
8. Xuất kết quả theo modulo$10^9+7$. 

### Tại sao nó hoạt động 

Mỗi mảng thỏa mãn điều kiện kề đều có một giá trị nhỏ nhất duy nhất. 

Việc cố định mức tối thiểu và chuỗi các khác biệt liên tiếp sẽ xác định duy nhất toàn bộ mảng. Vì mọi sự khác biệt đều có chính xác`2k+1`khả năng, số lượng mảng có mức tối thiểu cố định bằng$(2k+1)^{n-1}$. 

Giá trị tối thiểu có thể nhận bất kỳ giá trị nào từ`0`ĐẾN`x+k-1`, mang lại$(x+k)(2k+1)^{n-1}$mảng có mức tối thiểu không vượt quá$x+k-1$. 

Trong số đó, các mảng duy nhất không đạt được điều kiện ưa thích là những mảng có giá trị hoàn toàn thấp hơn`x`. Những mảng đó được tính bằng ma trận DP. 

Hai bộ này khác nhau chính xác ở tập hợp các mảng ưa thích, do đó phép trừ sẽ tạo ra câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000007

def mat_mul(A, B):
    n = len(A)
    m = len(B[0])
    p = len(B)

    C = [[0] * m for _ in range(n)]

    for i in range(n):
        for k in range(p):
            if A[i][k] == 0:
                continue
            aik = A[i][k]
            for j in range(m):
                C[i][j] = (C[i][j] + aik * B[k][j]) % MOD

    return C

def mat_pow(M, e):
    n = len(M)

    R = [[0] * n for _ in range(n)]
    for i in range(n):
        R[i][i] = 1

    while e:
        if e & 1:
            R = mat_mul(R, M)
        M = mat_mul(M, M)
        e >>= 1

    return R

def solve_case(n, x, k):
    total = (x + k) % MOD
    total = total * pow(2 * k + 1, n - 1, MOD) % MOD

    if x == 0:
        below = 0
    else:
        M = [[0] * x for _ in range(x)]

        for i in range(x):
            for j in range(x):
                if abs(i - j) <= k:
                    M[i][j] = 1

        P = mat_pow(M, n - 1)

        vec = [[1] * x]
        res = mat_mul(vec, P)

        below = sum(res[0]) % MOD

    return (total - below) % MOD

def main():
    t = int(input())
    ans = []

    for _ in range(t):
        n, x, k = map(int, input().split())
        ans.append(str(solve_case(n, x, k)))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    main()
```Việc tính toán tự nhiên chia thành hai phần độc lập. 

Phần đầu tiên đếm các mảng có mức tối thiểu tối đa là`x+k-1`. Đó chỉ là phép lũy thừa mô-đun. 

Phần thứ hai đếm các mảng hoàn toàn bên dưới`x`. Từ`x ≤ 40`, không gian trạng thái rất nhỏ. Ma trận chuyển tiếp chứa chính xác các bước di chuyển hợp lệ giữa các giá trị. 

Vectơ ban đầu đều là số 1 vì bất kỳ giá trị nào bên dưới`x`có thể được chọn làm phần tử đầu tiên. nhân với$M^{n-1}$áp dụng quá trình chuyển đổi`n-1`lần. 

Một trường hợp ranh giới tinh tế là`x = 0`. Khi đó không có trạng thái nào trong DP "dưới x". Số lượng mảng hoàn toàn bên dưới`x`đơn giản là bằng không. 

Một chi tiết nữa đó là`n`có thể bằng`1`. lũy thừa ma trận với số mũ`0`trả về chính xác ma trận nhận dạng, do đó mã sẽ xử lý trường hợp này một cách tự nhiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 1, x = 4, k = 25
```Đầu tiên tính toán số lượng lớn. 

| Số lượng | Giá trị | 
| --- | --- | 
| x+k | 29 | 
| (2k+1)^(n-1) | 1 | 
| tổng cộng | 29 | 

Bây giờ hãy đếm các mảng hoàn toàn bên dưới`4`. 

| Giá trị | Yếu tố đầu tiên có thể? | 
| --- | --- | 
| 0 | Có | 
| 1 | Có | 
| 2 | Có | 
| 3 | Có | 

Vì vậy: 

| Số lượng | Giá trị | 
| --- | --- | 
| bên dưới | 4 | 
| trả lời | 29 - 4 = 25 | 

Đầu ra:```
25
```Ví dụ này cho thấy tại sao phép trừ là cần thiết. Công thức đầu tiên tính tất cả các cực tiểu từ`0`ĐẾN`28`, nhưng tối thiểu`0..3`tương ứng với các mảng không bao giờ chạm vào khoảng mục tiêu. 

### Ví dụ 2 

đầu vào:```
n = 3, x = 0, k = 1
```| Số lượng | Giá trị | 
| --- | --- | 
| x+k | 1 | 
| (2k+1)^(n-1) | 3^2 = 9 | 
| tổng cộng | 9 | 

Từ`x=0`, không có giá trị nào dưới đây`x`. 

| Số lượng | Giá trị | 
| --- | --- | 
| bên dưới | 0 | 
| trả lời | 9 | 

Đầu ra:```
9
```Điều này thể hiện trường hợp đặc biệt trong đó mọi mức tối thiểu hợp lệ đều nằm trong khoảng mục tiêu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(x^3 \log n)$| Phép lũy thừa ma trận trên`x × x`ma trận | 
| Không gian |$O(x^2)$| Lưu trữ ma trận | 

Từ`x ≤ 40`, kích thước ma trận không bao giờ vượt quá 40. Thậm chí$40^3$phép nhân ma trận nhỏ và chỉ$O(\log n)$trong số đó là cần thiết. Giải pháp dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    MOD = 1000000007

    def mat_mul(A, B):
        n = len(A)
        m = len(B[0])
        p = len(B)

        C = [[0] * m for _ in range(n)]

        for i in range(n):
            for k in range(p):
                if A[i][k] == 0:
                    continue
                for j in range(m):
                    C[i][j] = (C[i][j] + A[i][k] * B[k][j]) % MOD
        return C

    def mat_pow(M, e):
        n = len(M)
        R = [[0] * n for _ in range(n)]
        for i in range(n):
            R[i][i] = 1

        while e:
            if e & 1:
                R = mat_mul(R, M)
            M = mat_mul(M, M)
            e >>= 1
        return R

    data = io.StringIO(inp)
    t = int(data.readline())
    out = []

    for _ in range(t):
        n, x, k = map(int, data.readline().split())

        total = (x + k) % MOD
        total = total * pow(2 * k + 1, n - 1, MOD) % MOD

        if x == 0:
            below = 0
        else:
            M = [[0] * x for _ in range(x)]
            for i in range(x):
                for j in range(x):
                    if abs(i - j) <= k:
                        M[i][j] = 1

            P = mat_pow(M, n - 1)
            res = mat_mul([[1] * x], P)
            below = sum(res[0]) % MOD

        out.append(str((total - below) % MOD))

    return "\n".join(out)

# provided sample
assert run(
"""4
3 0 1
1 4 25
4 7 2
1000000000 40 1000000000
"""
) == """9
25
582
514035484"""

# minimum size
assert run("1\n1 0 1\n") == "1"

# x = 0 boundary
assert run("1\n1 0 3\n") == "3"

# single-element interval starting above zero
assert run("1\n1 1 1\n") == "1"

# all values below x excluded
assert run("1\n1 2 1\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 1`|`1`| Ví dụ nhỏ nhất có thể | 
|`1 0 3`|`3`|`x=0`xử lý đặc biệt | 
|`1 1 1`|`1`| Giá trị mục tiêu duy nhất | 
|`1 2 1`|`1`| Phép trừ chính xác các giá trị dưới đây`x`| 

## Vỏ cạnh 

Hãy xem xét:```
1
1 0 3
```Khoảng mục tiêu là`[0,2]`. Mỗi mảng hợp lệ có một phần tử và phần tử đó phải là một trong`0,1,2`. Công thức cho$$(0+3)\cdot 1 = 3.$$Không có mảng nào hoàn toàn bên dưới`0`, do đó số hạng trừ bằng 0. Thuật toán trả về`3`. 

Bây giờ hãy xem xét:```
1
1 4 25
```Công thức đầu tiên tính tất cả các cực tiểu từ`0`bởi vì`28`, cho`29`. Trong đó, giá trị`0,1,2,3`không bao giờ đạt được khoảng thời gian mục tiêu`[4,28]`. Ma trận DP đếm chính xác bốn mảng đó và câu trả lời trở thành`25`. 

Cuối cùng, hãy xem xét:```
1
2 1 1
```Khoảng thời gian là`[1,1]`. Công thức cho$$2 \cdot 3 = 6.$$DP bên dưới`x`chỉ có trạng thái`0`và có chính xác một mảng có độ dài-2 ở đó, cụ thể là`[0,0]`. Trừ nó mang lại`5`, phù hợp với phép liệt kê trực tiếp. Điều này xác nhận rằng ma trận DP loại bỏ chính xác các mảng không cầu kỳ và không có gì khác.
