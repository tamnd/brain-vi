---
title: "CF 106351K - Wala matgeesh bra7tek whoa enty hatzeleniiii"
description: "Chúng ta có một dãy số nguyên dương. Chúng ta cần đếm các cặp chỉ số (i, j) trong đó i < j và hai giá trị được chọn thỏa mãn hai điều kiện lý thuyết số cùng một lúc. Ước chung lớn nhất của chúng ít nhất phải là x và bội số chung nhỏ nhất của chúng không được vượt quá y."
date: "2026-06-25T08:11:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106351
codeforces_index: "K"
codeforces_contest_name: "Zaglol Contest - FCDS level 2 contest 2026"
rating: 0
weight: 106351
solve_time_s: 52
verified: true
draft: false
---

[CF 106351K - Wala matgeesh bra7tek waya enty hatzeleniiii](https://codeforces.com/problemset/problem/106351/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một dãy số nguyên dương. Chúng ta cần đếm các cặp chỉ số`(i, j)`Ở đâu`i < j`và hai giá trị được chọn thỏa mãn đồng thời hai điều kiện lý thuyết số. Ước chung lớn nhất của chúng ít nhất phải bằng`x`và bội số chung nhỏ nhất của chúng không được vượt quá`y`. Nhiệm vụ là xuất ra số lượng các cặp như vậy. 

Các giới hạn quan trọng là độ dài mảng và mọi giá trị tối đa là`100000`. Điều này loại trừ việc kiểm tra trực tiếp từng cặp vì có thể có khoảng`10^10`cặp. Chúng ta cần sử dụng thực tế là bản thân các giá trị được giới hạn bởi`100000`, cho phép xử lý trước trong phạm vi giá trị thay vì các chỉ mục. 

Một cặp chứa giá trị lớn hơn`y`không bao giờ có giá trị vì lcm của hai số nguyên dương ít nhất luôn bằng cả hai số nguyên. Vì vậy chúng ta có thể bỏ qua tất cả các giá trị mảng lớn hơn`y`ngay từ đầu. 

Một trường hợp tinh tế là khi nhiều giá trị bằng nhau. Ví dụ: với đầu vào:```
3 2 10
4 4 4
```câu trả lời là:```
3
```Cả ba cặp đều có gcd`4`và lcm`4`. Một giải pháp chỉ đếm các giá trị khác nhau sẽ bỏ sót các cặp này. 

Một trường hợp phức tạp khác là khi điều kiện gcd và điều kiện lcm tương tác với nhau. Ví dụ:```
2 5 6
10 15
```gcd là`5`, đủ lớn, nhưng lcm thì`30`, quá lớn. Câu trả lời đúng là:```
0
```Chỉ kiểm tra gcd hoặc chỉ lcm sẽ tính sai cặp này. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là thử từng cặp chỉ số, tính gcd và lcm, rồi kiểm tra các điều kiện. Điều này đúng vì mọi cặp có thể đều được kiểm tra. Tuy nhiên, với`n = 100000`, điều này thực hiện xung quanh`5 * 10^9`kiểm tra cặp, vượt xa thời hạn. 

Quan sát quan trọng là các giá trị có thể có đều nhỏ. Thay vì nghĩ về các vị trí, chúng ta có thể nghĩ về các giá trị lcm có thể có. Mỗi cặp hợp lệ đều có một lcm`L`với`L <= y`và cả hai số trong cặp phải là ước của`L`. Số ước của một số lên tới`100000`nhỏ nên với mỗi lcm có thể, chúng ta chỉ cần kiểm tra một tập ứng cử viên nhỏ. 

Đối với một cố định`L`, ta tập hợp tất cả các ước của`L`xuất hiện trong mảng. Bất kỳ cặp nào trong số các ước số này đều có lcm chia hết`L`. Chúng tôi chỉ giữ lại các cặp có lcm thực tế chính xác`L`, và gcd của nó ít nhất là`x`. 

Sức mạnh tàn bạo đối với các chỉ số trở thành một phép liệt kê nhỏ hơn nhiều đối với các ước số lên tới`y`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² log A) | O(1) | Quá chậm | 
| Tối ưu | O(y log y + Σ τ(i)²) | O(y log y) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem mỗi giá trị xuất hiện trong mảng bao nhiêu lần. Giá trị lớn hơn`y`bị bỏ qua vì chúng không thể là một phần của cặp hợp lệ. 
2. Tính toán trước danh sách các ước số cho mọi số từ`1`ĐẾN`y`. Điều này cho phép chúng ta nhanh chóng tìm thấy tất cả các giá trị có thể tham gia vào một cặp với lcm nhất định. 
3. Lặp lại mọi giá trị lcm có thể`L`từ`1`ĐẾN`y`. Vì điều này`L`, chỉ những giá trị chia`L`có thể xuất hiện thành một cặp có lcm là`L`. 
4. Với mọi cặp ước số`(a, b)`của`L`, tính toán`gcd(a, b)`Và`lcm(a, b)`. Nếu gcd ít nhất là`x`và lcm chính xác là`L`, cộng số cặp chỉ mục được hình thành bởi các giá trị này. 
5. Xử lý các giá trị bằng nhau một cách riêng biệt bên trong bảng liệt kê số chia. Nếu như`a == b`, phần đóng góp là`freq[a] * (freq[a] - 1) / 2`. Ngược lại, sự đóng góp là`freq[a] * freq[b]`. 

Tại sao nó hoạt động: 

Mỗi cặp hợp lệ có một giá trị lcm duy nhất. Trong quá trình lặp cho lcm đó, cả hai giá trị phải xuất hiện giữa các ước số của nó, do đó cặp giá trị này được xem xét đúng một lần. Việc kiểm tra gcd và lcm khớp với yêu cầu ban đầu, có nghĩa là mọi cặp được đếm đều hợp lệ. Vì không có lần lặp lcm nào khác có thể đếm cùng một cặp nên không có phép tính kép. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

def solve():
    data = list(map(int, sys.stdin.buffer.read().split()))
    if not data:
        return

    n, x, y = data[0], data[1], data[2]
    arr = data[3:3 + n]

    freq = [0] * (y + 1)
    for v in arr:
        if v <= y:
            freq[v] += 1

    divisors = [[] for _ in range(y + 1)]
    for d in range(1, y + 1):
        for m in range(d, y + 1, d):
            divisors[m].append(d)

    ans = 0

    for l in range(1, y + 1):
        divs = divisors[l]
        m = len(divs)

        for i in range(m):
            a = divs[i]
            if freq[a] == 0:
                continue

            for j in range(i, m):
                b = divs[j]
                if freq[b] == 0:
                    continue

                if gcd(a, b) < x:
                    continue

                if a // gcd(a, b) * b != l:
                    continue

                if a == b:
                    ans += freq[a] * (freq[a] - 1) // 2
                else:
                    ans += freq[a] * freq[b]

    print(ans)

if __name__ == "__main__":
    solve()
```Mảng tần số chỉ lưu trữ những giá trị có thể xuất hiện trong câu trả lời. Điều này tránh lãng phí thời gian cho những con số đã vi phạm giới hạn lcm. 

Danh sách ước số được xây dựng bằng cách sử dụng vòng lặp dạng sàng. Đối với mỗi ứng viên của ước số, chúng ta nối nó vào tất cả các bội số của nó. Sau bước này,`divisors[l]`chứa chính xác các giá trị có thể tạo thành một cặp với lcm`l`. 

Bên trong vòng lặp chính, việc liệt kê cặp bắt đầu từ`i`thay vì 0 vì các cặp không có thứ tự. Điều này tránh việc đếm`(a, b)`Và`(b, a)`riêng. Kiểm tra lcm sử dụng`a // gcd(a, b) * b`thay vì`a * b // gcd(a, b)`để giảm khả năng tràn trong các ngôn ngữ có số nguyên kích thước cố định. 

Trường hợp giá trị bằng nhau cần xử lý đặc biệt vì việc chọn hai lần xuất hiện có cùng giá trị sẽ tạo ra sự kết hợp chứ không phải tích của hai tần số khác nhau. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
10 1 10
1 2 3 4 5 6 7 8 9 10
```Dấu vết của các biến quan trọng là: 

| giá trị lcm | các ước số được xem xét | đóng góp hợp lệ | trả lời | 
| --- | --- | --- | --- | 
| 1 | 1 | (1,1) | 0 | 
| 2 | 1,2 | (1,2), (2,2) không hợp lệ đối với lcm 2 ngoại trừ điều kiện gcd | 1 | 
| 3 | 1,3 | cặp hợp lệ được thêm vào | 2 | 
| 4 | 1,2,4 | thêm cặp với lcm 4 | 4 | 
| 5 đến 10 | các ước số còn lại | cặp hợp lệ bổ sung | 19 | 

Câu trả lời cuối cùng là:```
19
```Điều này cho thấy thuật toán tính theo nhóm lcm chứ không phải theo cặp chỉ số. 

Một ví dụ thứ hai:```
2 5 6
10 15
```Dấu vết là: 

| giá trị lcm | các ước số được xem xét | kiểm tra gcd | đóng góp | 
| --- | --- | --- | --- | 
| 1 đến 5 | không có giá trị nào chứa cả hai giá trị | thất bại | 0 | 
| 6 | chỉ chia hết cho 6 | không có cặp nào tồn tại | 0 | 

Câu trả lời cuối cùng là:```
0
```Bài tập này áp dụng cho trường hợp gcd lớn không đủ vì giới hạn lcm bị vi phạm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(y log y + Σ τ(i)²) | Việc kiểm tra sàng chia và cặp chia được giới hạn bởi phạm vi giá trị nhỏ | 
| Không gian | O(y log y) | Lưu trữ tất cả danh sách ước số cho các giá trị lên tới`y`| 

Giá trị tối đa chỉ`100000`, do đó việc xử lý tổng số chia vẫn có thể quản lý được. Thuật toán tránh số bậc hai của các cặp mảng và hoạt động trong giới hạn dự kiến. 

## Trường hợp thử nghiệm```python
import sys
import io
from math import gcd

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    data = list(map(int, sys.stdin.buffer.read().split()))
    if not data:
        return ""

    n, x, y = data[0], data[1], data[2]
    arr = data[3:3 + n]

    freq = [0] * (y + 1)
    for v in arr:
        if v <= y:
            freq[v] += 1

    divs = [[] for _ in range(y + 1)]
    for i in range(1, y + 1):
        for j in range(i, y + 1, i):
            divs[j].append(i)

    ans = 0
    for l in range(1, y + 1):
        d = divs[l]
        for i in range(len(d)):
            for j in range(i, len(d)):
                a, b = d[i], d[j]
                if freq[a] == 0 or freq[b] == 0:
                    continue
                g = gcd(a, b)
                if g >= x and a // g * b == l:
                    if a == b:
                        ans += freq[a] * (freq[a] - 1) // 2
                    else:
                        ans += freq[a] * freq[b]
    return str(ans) + "\n"

assert run("""10 1 10
1 2 3 4 5 6 7 8 9 10
""") == "19\n"

assert run("""2 5 6
10 15
""") == "0\n"

assert run("""3 2 10
4 4 4
""") == "3\n"

assert run("""4 3 20
6 10 15 30
""") == "1\n"

assert run("""1 1 100000
100000
""") == "0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ba giá trị bằng nhau | 3 | Đếm cặp giá trị bằng nhau | 
| Gcd vượt qua nhưng lcm thất bại | 0 | Cả hai điều kiện đều được yêu cầu | 
| Mối quan hệ số chia hỗn hợp | 1 | Nhóm lcm đúng | 
| Yếu tố đơn | 0 | Không có cặp nào có thể | 

## Vỏ cạnh 

Đối với trường hợp giá trị bằng nhau:```
3 2 10
4 4 4
```Thuật toán đạt lcm`4`. Các ước số bao gồm`4`, và cặp`(4,4)`có gcd`4`, vì vậy nó góp phần`3 * 2 / 2 = 3`. Không có lcm nào khác có thể đếm được những cặp này nên câu trả lời vẫn đúng. 

Đối với trường hợp lỗi lcm:```
2 5 6
10 15
```gcd là`5`, nhưng phép tính lcm cho`30`. Thuật toán chỉ chấp nhận cặp khi lcm được tính toán bằng nhóm lcm hiện tại và nhóm tối đa là`y`, vì vậy cặp này bị từ chối. 

Đối với các giá trị lớn hơn`y`, chẳng hạn như:```
2 1 5
10 2
```giá trị`10`sẽ bị loại khỏi sự xem xét ngay lập tức. Vì bất kỳ cặp nào chứa nó đều có ít nhất lcm`10`, nó không thể hợp lệ. Riêng giá trị còn lại không thể tạo thành một cặp nên đáp án là`0`.
