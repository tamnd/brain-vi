---
title: "CF 105043C - \u041c\u0430\u0440\u0441\u0438\u0430\u043d\u0441\u043a\u0438\u0435 \u0447\u0438\u0441\u043b\u0430"
description: "Chúng ta được cho một số nguyên cố định a và một khoảng [l, r]. Một số x trong khoảng đó được coi là tương thích với a nếu gcd(a, x) = 1. Nhiệm vụ là đếm xem có bao nhiêu số nguyên trong khoảng đó cùng nguyên tố với a. Hạn chế nổi bật nhất là l và r có thể lớn bằng 10^18."
date: "2026-06-28T01:31:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105043
codeforces_index: "C"
codeforces_contest_name: "\u0424\u0438\u043d\u0430\u043b \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b \u041d\u0422\u041e: \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0446\u0438\u043e\u043d\u043d\u0430\u044f \u0431\u0435\u0437\u043e\u043f\u0430\u0441\u043d\u043e\u0441\u0442\u044c. \u0421\u0435\u043a\u0446\u0438\u044f - \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0430"
rating: 0
weight: 105043
solve_time_s: 72
verified: true
draft: false
---

[CF 105043C - \u041c\u0430\u0440\u0441\u0438\u0430\u043d\u0441\u043a\u0438\u0435 \u0447\u0438\u0441\u043b\u0430](https://codeforces.com/problemset/problem/105043/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên cố định`a`và một khoảng`[l, r]`. một con số`x`từ khoảng đó được coi là tương thích với`a`nếu như`gcd(a, x) = 1`. Nhiệm vụ là đếm xem có bao nhiêu số nguyên nguyên tố cùng nhau trong khoảng`a`. 

Hạn chế nổi bật nhất là`l`Và`r`có thể lớn như`10^18`. Mặc dù các điểm cuối của khoảng là rất lớn nhưng giá trị của`a`nhiều nhất là`10^7`. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ thuật toán nào phụ thuộc vào kích thước của khoảng đều không thể thực hiện được. Thậm chí lặp đi lặp lại trong một khoảng thời gian`10^9`đã là quá chậm rồi, trong khi khoảng thời gian ở đây có thể lớn hơn rất nhiều. 

Mặt khác, phân tích một số đến`10^7`là không tốn kém. Phân chia thử nghiệm lên đến`√a`yêu cầu tối đa khoảng 3162 lần lặp, rất nhỏ. Điều này cho thấy rằng nghiệm chỉ nên phụ thuộc vào các thừa số nguyên tố của`a`, không phải trên độ dài khoảng. 

Một sai lầm dễ mắc phải là đếm bội số của mọi thừa số nguyên tố một cách độc lập mà không sửa các phần trùng lặp. 

Ví dụ,```
a = 6
l = 1
r = 6
```Câu trả lời đúng là`2`, vì chỉ`1`Và`5`là nguyên tố cùng nhau với`6`. Đơn giản chỉ cần trừ bội số của`2`và bội số của`3`cho`6 - 3 - 2 = 1`, vì số`6`được trừ hai lần. 

Một sai lầm phổ biến khác là sử dụng mọi thừa số nguyên tố với bội số. 

Ví dụ,```
a = 12
l = 1
r = 12
```Hệ số nguyên tố là`2² · 3`, nhưng chỉ có các thừa số nguyên tố phân biệt`{2, 3}`vấn đề. Liệu một số có chia sẻ một hệ số của`2`với`12`không phụ thuộc vào bao nhiêu lần`2`xuất hiện trong phép nhân hóa. 

Trường hợp cạnh cuối cùng xảy ra khi`a = 1`.```
a = 1
l = 10
r = 20
```Mọi số nguyên đều nguyên tố cùng nhau`1`, vậy câu trả lời là`11`. Việc triển khai luôn thực hiện loại trừ bao hàm mà không kiểm tra xem có thực sự có thừa số nguyên tố hay không có thể tạo ra số 0 không chính xác. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất sẽ kiểm tra mọi số nguyên trong khoảng. Đối với mỗi số`x`, tính toán`gcd(a, x)`và tăng câu trả lời nếu gcd bằng`1`. Điều này rõ ràng là đúng vì nó kiểm tra chính xác thuộc tính được yêu cầu. 

Thật không may, cách tiếp cận này phụ thuộc vào độ dài khoảng thời gian. Trong trường hợp xấu nhất khoảng chứa gần như`10^18`những con số, khiến cho ngay cả một lần vượt qua cũng hoàn toàn không thể. 

Quan sát quan trọng là một số không nguyên tố cùng nhau với`a`chính xác khi nó chia hết cho ít nhất một thừa số nguyên tố riêng biệt của`a`. Khi đã biết các ước nguyên tố riêng biệt, bài toán sẽ trở thành đếm các số trong khoảng chia hết cho ít nhất một trong các số nguyên tố đó. 

Đếm bội số của một số trong một khoảng thật dễ dàng. Khó khăn là một số có thể chia hết cho nhiều thừa số nguyên tố cùng một lúc, gây ra việc tính hai lần. Đây chính xác là tình huống áp dụng nguyên tắc bao gồm-loại trừ. 

Từ`a ≤ 10^7`, nó chỉ có thể có một số lượng nhỏ các thừa số nguyên tố khác nhau. Tích của chín số nguyên tố đầu tiên đã vượt quá`10^7`, vậy có nhiều nhất 8 ước số nguyên tố khác nhau. Điều này có nghĩa là có nhiều nhất`2^8 = 256`tập hợp con, làm cho việc loại trừ bao gồm cực kỳ nhanh chóng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((r - l + 1) log a) | O(1) | Quá chậm | 
| Tối ưu | O(√a + 2^k · k) | O(k) | Đã chấp nhận | 

Đây,`k`là số các thừa số nguyên tố phân biệt của`a`. 

## Hướng dẫn thuật toán 

1. Phân tích nhân tử`a`sử dụng phép chia thử và chỉ lưu trữ các ước nguyên tố riêng biệt của nó. 
2. Hãy để`total = r - l + 1`, số các số nguyên trong khoảng. 
3. Liệt kê mọi tập con khác rỗng của các ước nguyên tố riêng biệt. 
4. Với mỗi tập hợp con, hãy tính tích các số nguyên tố của nó. Bởi vì tất cả các thừa số đều là các số nguyên tố phân biệt nên tích này đơn giản là bội số chung nhỏ nhất của chúng. 
5. Đếm xem có bao nhiêu số`[l, r]`có thể chia hết cho tích này bằng cách sử dụng```
r // product - (l - 1) // product
```6. Nếu tập hợp con chứa số lẻ các số nguyên tố, hãy cộng số này vào tổng số bao gồm-loại trừ. Nếu nó chứa một số số nguyên tố chẵn thì hãy trừ nó đi. 
7. Tổng bao gồm-loại trừ bằng số số nguyên có chung ít nhất một thừa số nguyên tố với`a`. 
8. Trừ giá trị này khỏi`total`. Kết quả chính xác là số số nguyên nguyên tố cùng nhau`a`. 

### Tại sao nó hoạt động 

Mọi số nguyên trong khoảng đều không chia sẻ thừa số nguyên tố nào với`a`hoặc chia sẻ ít nhất một. Loại trừ bao gồm đếm chính xác những số nguyên chia hết cho ít nhất một ước số nguyên tố riêng biệt của`a`, hiệu chỉnh cho mọi sự trùng lặp giữa các điều kiện chia hết. Vì một số nguyên tố cùng nhau`a`chính xác là khi nó bị loại khỏi liên kết này, việc trừ kích thước liên kết khỏi kích thước khoảng sẽ cho câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a, l, r = map(int, input().split())

primes = []
x = a
d = 2
while d * d <= x:
    if x % d == 0:
        primes.append(d)
        while x % d == 0:
            x //= d
    d += 1 if d == 2 else 2
if x > 1:
    primes.append(x)

bad = 0
k = len(primes)

for mask in range(1, 1 << k):
    prod = 1
    bits = 0
    for i in range(k):
        if (mask >> i) & 1:
            prod *= primes[i]
            bits += 1
    cnt = r // prod - (l - 1) // prod
    if bits & 1:
        bad += cnt
    else:
        bad -= cnt

print((r - l + 1) - bad)
```Phần đầu tiên phân tích thành nhân tử`a`và chỉ lưu trữ các ước số nguyên tố riêng biệt. Bên trong`while`vòng lặp loại bỏ mọi lần xuất hiện của số nguyên tố hiện tại để các yếu tố lặp lại không bao giờ xuất hiện trong danh sách. 

Phần tiếp theo lặp qua mọi tập hợp con không trống của các số nguyên tố này bằng cách sử dụng mặt nạ bit. Mỗi mặt nạ đại diện duy nhất cho một tập hợp con. Tích tập hợp con được xây dựng bằng cách nhân các số nguyên tố đã chọn với nhau. 

Việc đếm khoảng sử dụng công thức chuẩn để đếm bội số trong một khoảng đóng. Vì số nguyên Python có độ chính xác tùy ý nên tích và điểm cuối khoảng lên tới`10^18`được xử lý một cách an toàn. 

Cuối cùng, các tập con có kích thước lẻ được thêm vào và các tập con có kích thước chẵn được trừ đi theo nguyên tắc bao gồm-loại trừ. Số lượng kết quả của các số không nguyên tố cùng nhau được loại bỏ khỏi kích thước khoảng để có được câu trả lời. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 4 8
```Các thừa số nguyên tố riêng biệt là`{5}`. 

| Bước | Tập hợp con hiện tại | Sản phẩm | Bội số trong khoảng | Tổng số bao gồm-loại trừ | 
| --- | --- | --- | --- | --- | 
| 1 | {5} | 5 | 1 | 1 | 

Khoảng chứa`5`những con số. Một trong số chúng có thể chia được cho`5`, vậy 4 số nguyên tố cùng nhau`5`. 

Trả lời:`4`. 

### Mẫu 2 

đầu vào:```
3 1 20
```Các thừa số nguyên tố riêng biệt là`{3}`. 

| Bước | Tập hợp con hiện tại | Sản phẩm | Bội số trong khoảng | Tổng số bao gồm-loại trừ | 
| --- | --- | --- | --- | --- | 
| 1 | {3} | 3 | 6 | 6 | 

Khoảng thời gian có`20`số và`6`được chia cho`3`. Phần còn lại`14`là nguyên tố cùng nhau với`3`. 

Trả lời:`14`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(√a + 2^k · k) | Phân tích nhân tử cộng với bao gồm-loại trừ trên tất cả các tập hợp con | 
| Không gian | O(k) | Lưu trữ các ước số nguyên tố riêng biệt | 

Từ`a ≤ 10^7`, phép chia thử là cực kỳ nhỏ. Số lượng các thừa số nguyên tố phân biệt nhiều nhất là 8, vì vậy nhiều nhất`255`tập hợp con không trống được xử lý. Thời gian chạy thực sự không đổi đối với kích thước khoảng, cho phép các khoảng đạt tới`10^18`. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    a, l, r = map(int, input().split())

    primes = []
    x = a
    d = 2
    while d * d <= x:
        if x % d == 0:
            primes.append(d)
            while x % d == 0:
                x //= d
        d += 1 if d == 2 else 2
    if x > 1:
        primes.append(x)

    bad = 0
    k = len(primes)

    for mask in range(1, 1 << k):
        prod = 1
        bits = 0
        for i in range(k):
            if (mask >> i) & 1:
                prod *= primes[i]
                bits += 1
        cnt = r // prod - (l - 1) // prod
        if bits & 1:
            bad += cnt
        else:
            bad -= cnt

    print((r - l + 1) - bad)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    global input
    input = sys.stdin.readline
    out = io.StringIO()
    old = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old
    return out.getvalue()

assert run("5 4 8\n") == "4\n", "sample 1"
assert run("3 1 20\n") == "14\n", "sample 2"
assert run("1 1 1\n") == "1\n", "minimum case"
assert run("12 1 12\n") == "4\n", "repeated prime factors"
assert run("6 6 6\n") == "0\n", "single non-coprime value"
assert run("2 999999999999999999 1000000000000000000\n") == "1\n", "large boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`1`| Không có thừa số nguyên tố | 
|`12 1 12`|`4`| Các thừa số nguyên tố trùng lặp được xử lý chính xác | 
|`6 6 6`|`0`| Khoảng phần tử đơn | 
|`2 999999999999999999 1000000000000000000`|`1`| Điểm cuối và ranh giới khoảng rất lớn | 

## Vỏ cạnh 

Khi nào`a = 1`, không có ước số nguyên tố. Coi như```
1 10 20
```Danh sách nguyên tố trống, do đó vòng lặp loại trừ bao gồm không thực hiện phép lặp nào và số lượng các số không nguyên tố cùng nhau vẫn bằng 0. Khoảng chứa`11`số, do đó thuật toán trả về`11`, điều này đúng vì mọi số nguyên đều nguyên tố cùng nhau với`1`. 

Khi`a`chứa các thừa số nguyên tố lặp lại, chỉ các số nguyên tố riêng biệt được lưu trữ.```
12 1 12
```Bước phân tích nhân tử tạo ra`{2, 3}`thay vì`{2, 2, 3}`. Bao gồm-loại trừ đếm số chia hết cho`2`hoặc`3`, cho`8`, và trừ đi`12`lá`4`, cụ thể là`1`,`5`,`7`, Và`11`. 

Khi khoảng chỉ chứa một số, các công thức đếm tương tự vẫn hoạt động.```
6 6 6
```Kích thước khoảng là`1`. Bao gồm-loại trừ thấy rằng`6`chia hết cho cả hai`2`Và`3`, nên nó thuộc tập các số bị cấm. Câu trả lời cuối cùng là`0`, đúng như mong đợi. 

Khi các điểm cuối của khoảng là cực kỳ lớn,```
2 999999999999999999 1000000000000000000
```thuật toán không bao giờ lặp lại trong khoảng thời gian. Nó tính số số nguyên chẵn bằng phép chia số nguyên, trừ đi số đó từ độ dài khoảng của`2`, và trả về một cách chính xác`1`. Thời gian chạy chỉ phụ thuộc vào hệ số của`2`, không phải về kích thước của khoảng.
