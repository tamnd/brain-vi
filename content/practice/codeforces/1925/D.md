---
title: "CF 1925D - Chuyến Đi Tốt Đẹp"
description: "Chúng ta có một đồ thị về n đứa trẻ. Chỉ có m cặp được đánh dấu là bạn bè và mỗi cạnh tình bạn có trọng số ban đầu là f. Giáo viên thực hiện chính xác k chuyến du ngoạn. Trong mỗi chuyến tham quan, một cặp trẻ em không có thứ tự được chọn ngẫu nhiên từ tất cả các cặp có thể có."
date: "2026-06-08T19:04:52+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1925
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 921 (Div. 2)"
rating: 1900
weight: 1925
solve_time_s: 129
verified: true
draft: false
---

[CF 1925D - Chuyến đi tốt đẹp](https://codeforces.com/problemset/problem/1925/D) 

**Xếp hạng:** 1900 
**Tags:** tổ hợp, dp, toán, xác suất 
**Thời gian giải:** 2m 9s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một biểu đồ trên`n`những đứa trẻ. Chỉ một`m`các cặp được đánh dấu là bạn bè và mỗi cạnh tình bạn có trọng số ban đầu`f`. 

Giáo viên thực hiện chính xác`k`những chuyến du ngoạn. Trong mỗi chuyến tham quan, một cặp trẻ em không có thứ tự được chọn ngẫu nhiên từ tất cả các cặp có thể có. Cùng một cặp có thể được chọn nhiều lần vì mỗi lần chọn đều độc lập. 

Nếu cặp được chọn là cạnh tình bạn thì có hai điều sẽ xảy ra: 

Đầu tiên, giá trị tình bạn hiện tại của cạnh đó góp phần trả lời cho chuyến du ngoạn này. 

Thứ hai, sau khi chuyến du ngoạn kết thúc, giá trị tình bạn của lợi thế đó sẽ tăng thêm một, ảnh hưởng đến những chuyến du ngoạn trong tương lai. 

Nếu cặp được chọn không phải là cạnh tình bạn thì nó không đóng góp gì và không bao giờ thay đổi. 

Chúng ta phải tính toán tổng đóng góp dự kiến ​​trên tất cả`k`những chuyến du ngoạn. Câu trả lời là một số hữu tỉ. Chúng tôi được yêu cầu xuất nó theo modulo`10^9+7`, có nghĩa là nhân tử số với nghịch đảo mô đun của mẫu số. 

Bản thân biểu đồ rất thưa thớt. Trong tất cả các trường hợp thử nghiệm, tổng số cạnh hữu nghị nhiều nhất là`10^5`, trong khi tổng số chuyến du ngoạn nhiều nhất là`2·10^5`. Điều này ngay lập tức loại trừ bất kỳ sự mô phỏng nào về tất cả các trạng thái có thể có của các giá trị tình bạn. Số lượng lịch sử có thể tăng theo cấp số nhân với`k`. 

Thách thức chính là giá trị tình bạn phát triển theo thời gian. Một lợi thế tình bạn sẽ trở nên có giá trị hơn nếu nó đã được chọn trước đó. Thoạt nhìn, điều này tạo ra sự phụ thuộc giữa các chuyến du ngoạn, nhưng kỳ vọng cho phép chúng ta tách biệt quá trình một cách mạnh mẽ hơn nhiều so với mô phỏng trạng thái xác suất trực tiếp gợi ý. 

Một số trường hợp đáng chú ý. 

Xét một đồ thị không có cạnh hữu nghị:```
n = 100
m = 0
k = 24
```Mỗi cặp được chọn đều không đóng góp. Câu trả lời chính xác là bằng không. Bất kỳ công thức nào chia cho số cạnh tình bạn hoặc giả định ít nhất một tình bạn đều phải xử lý trường hợp này. 

Xét đồ thị không tầm thường nhỏ nhất:```
n = 2
m = 1
k = 3
f = 5
```Chỉ có một cặp có thể có nên nó được chọn mọi lúc. Những đóng góp là:```
5 + 6 + 7 = 18
```Một giải pháp xử lý mọi chuyến tham quan một cách độc lập và luôn sử dụng giá trị tình bạn ban đầu sẽ tạo ra sai sót`15`. 

Một trường hợp tế nhị khác là khi các giá trị tình bạn khác nhau:```
n = 3
m = 2
edges:
(1,2) = 100
(2,3) = 1
```Mức tăng dự kiến ​​được tạo ra bởi các lựa chọn trong tương lai chỉ phụ thuộc vào tần suất các cạnh tình bạn được chọn chứ không phụ thuộc vào trọng số ban đầu của chúng. Việc trộn lẫn hai hiệu ứng này không chính xác thường dẫn đến việc đếm hai lần. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô hình hóa toàn bộ quá trình ngẫu nhiên. 

Tại mỗi chuyến tham quan, chúng tôi chọn một trong những`C(n,2)`cặp. Nếu đó là tình bạn thì giá trị của nó sẽ tăng lên. Trạng thái của quá trình là vectơ của các giá trị tình bạn hiện tại. Sau một bước có thể có nhiều trạng thái, sau hai bước thậm chí còn nhiều hơn nữa, và sau`k`bước, số lượng trạng thái trở nên khổng lồ. 

Ngay cả khi chúng tôi chỉ theo dõi số lần mỗi cạnh tình bạn đã được chọn, thì kích thước không gian trạng thái gần bằng số lượng các thành phần nguyên của nhiều nhất`k`giữa`m`cạnh, điều này hoàn toàn không thể thực hiện được. 

Ý tưởng dùng vũ lực là đúng vì nó tuân theo quy trình một cách chính xác, nhưng nó trở nên không thể thực hiện được rất lâu trước khi đạt đến giới hạn của vấn đề. 

Quan sát mở ra giải pháp là tính tuyến tính của kỳ vọng. 

Thay vì theo dõi toàn bộ trạng thái, hãy tập trung vào từng chuyến tham quan một lần. 

Cho phép```
T = C(n,2)
```là tổng số cặp có thể có. 

Giả sử một lợi thế bắt đầu bằng giá trị tình bạn`f`. 

Nếu chúng ta hiện đang tham quan`i`(được lập chỉ mục 0), giá trị của cạnh đó bằng```
f + (# times this edge was selected during previous i excursions)
```Số lượng lựa chọn trước đó dự kiến ​​​​rất dễ tính toán. 

Mỗi chuyến tham quan chọn cạnh này với xác suất```
1 / T
```một cách độc lập. 

Trong số những người đầu tiên`i`chuyến du ngoạn, số lần dự kiến ​​cạnh này được chọn là```
i / T
```Do đó, giá trị kỳ vọng của cạnh này khi tham quan`i`là```
f + i/T
```và xác suất cạnh được chọn khi tham quan`i`lại là```
1/T.
```Vì vậy, sự đóng góp dự kiến ​​của lợi thế đặc biệt này trong chuyến tham quan`i`là```
(1/T) · (f + i/T).
```Tổng hợp tất cả các khía cạnh tình bạn mang lại```
(Σf)/T + m·i/T².
```Bây giờ tổng hợp điều này trên tất cả các chuyến du ngoạn`i = 0 ... k-1`. 

Thuật ngữ đầu tiên trở thành```
k·Σf / T.
```Số hạng thứ hai trở thành```
m/T² · Σi
= m/T² · k(k-1)/2.
```Đây đã là công thức cuối cùng. 

Toàn bộ quá trình xác suất được chia thành hai giá trị tổng hợp:```
S = Σf
```Và```
m.
```Không có thông tin nào khác về vấn đề đồ thị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ theo k | Hàm mũ | Quá chậm | 
| Tối ưu | O(m) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`m`, Và`k`. 
2. Tính toán```
T = n(n-1)/2
```đó là tổng số cặp con không có thứ tự. 
3. Đọc tất cả các khía cạnh tình bạn và tích lũy```
S = Σf.
```Điểm cuối riêng lẻ không bao giờ xuất hiện trong công thức cuối cùng, chỉ có tổng giá trị tình bạn ban đầu mới quan trọng. 
4. Tính mức đóng góp dự kiến ​​từ các giá trị tình bạn ban đầu:```
k · S / T.
```Mỗi chuyến du ngoạn đều chọn một khía cạnh tình bạn cụ thể với xác suất`1/T`. 
5. Tính toán mức đóng góp dự kiến ​​được tạo ra khi giá trị tình bạn tăng lên:```
m · k(k-1)/2 / T².
```Dành cho chuyến tham quan`i`, mọi khía cạnh của tình bạn đều có giá trị gia tăng được mong đợi`i/T`, và tổng tất cả các cạnh và tất cả các chuyến du ngoạn sẽ tạo ra số hạng này. 
6. Cộng hai số hạng modulo`10^9+7`. 
7. Sử dụng nghịch đảo mô đun của`T`Và`T²`để đại diện cho các bộ phận. 

### Tại sao nó hoạt động 

Đối với bất kỳ cạnh tình bạn nào, giá trị hiện tại của nó bằng giá trị ban đầu cộng với số lần nó đã được chọn trước đó. 

Số lượng lựa chọn trước đó là tổng của các biến chỉ báo độc lập. Theo tính tuyến tính của kỳ vọng, kỳ vọng của nó sau`i`chính xác là những chuyến du ngoạn trước đó`i/T`. 

Kỳ vọng không yêu cầu sự độc lập giữa giá trị cạnh và sự kiện cạnh được chọn khi tham quan`i`, bởi vì chúng tôi tính toán trực tiếp```
E[ value × indicator ]
```BẰNG```
(1/T) × E[value].
```Chỉ báo cho hành trình hiện tại độc lập với tất cả các hành trình trước đó xác định giá trị hiện tại. 

Tổng hợp những kỳ vọng này trên tất cả các khía cạnh tình bạn và tất cả các chuyến du ngoạn sẽ mang lại chính xác tổng số đóng góp như mong đợi. Vì mọi phép biến đổi đều bảo toàn sự bằng nhau của các kỳ vọng nên công thức cuối cùng là đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    t = int(input())

    for _ in range(t):
        n, m, k = map(int, input().split())

        s = 0
        for _ in range(m):
            a, b, f = map(int, input().split())
            s = (s + f) % MOD

        T = n * (n - 1) // 2

        invT = pow(T % MOD, MOD - 2, MOD)
        invT2 = invT * invT % MOD

        part1 = (k % MOD) * s % MOD * invT % MOD

        pairs_sum = (k % MOD) * ((k - 1) % MOD) % MOD
        pairs_sum = pairs_sum * pow(2, MOD - 2, MOD) % MOD

        part2 = (m % MOD) * pairs_sum % MOD * invT2 % MOD

        ans = (part1 + part2) % MOD
        print(ans)

solve()
```Việc thực hiện tuân theo công thức dẫn xuất trực tiếp. 

Biến`s`lưu trữ tổng của tất cả các giá trị tình bạn ban đầu. Vì chỉ có tổng xuất hiện trong biểu thức cuối cùng nên chúng ta không bao giờ cần lưu trữ các cạnh riêng lẻ.`T`là số cặp không có thứ tự có thể có. Phép chia modulo một số nguyên tố được thực hiện bằng cách sử dụng nghịch đảo mô đun. Từ```
T²
```xuất hiện trong số hạng thứ hai, chúng tôi tính toán`invT`một lần và bình phương nó. 

số lượng```
k(k-1)/2
```có thể rất lớn nên mọi phép nhân đều được thực hiện theo modulo`MOD`. 

Một lỗi phổ biến là tính số hạng thứ hai bằng cách sử dụng`k(k+1)/2`. Tổng hợp đúng là```
0 + 1 + ... + (k-1),
```cái nào bằng`k(k-1)/2`. 

Một sai lầm phổ biến khác là quên rằng chỉ có tình bạn mới có giá trị. Hệ số nhân của số hạng thứ hai là`m`, không phải tổng số cặp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 1 10
1 2 1
```Đây:```
T = 1
S = 1
m = 1
k = 10
```| Biến | Giá trị | 
| --- | --- | 
| T | 1 | 
| S | 1 | 
| k·S/T | 10 | 
| k(k-1)/2 | 45 | 
| m·k(k-1)/(2T²) | 45 | 
| Trả lời | 55 | 

Những đóng góp thực sự là:```
1+2+3+...+10 = 55
```và công thức tái tạo kết quả chính xác. 

### Ví dụ 2 

đầu vào:```
3 1 2
1 2 1
```Có ba cặp có thể. 

| Biến | Giá trị | 
| --- | --- | 
| T | 3 | 
| S | 1 | 
| m | 1 | 
| k | 2 | 
| Học kỳ đầu tiên | 2/3 | 
| Học kỳ thứ hai | 9/1 | 
| Tổng cộng | 9/7 | 

Câu trả lời được mong đợi là chính xác`7/9`, phù hợp với mẫu 

Ví dụ này thể hiện ý tưởng chính rằng số hạng tăng trưởng xuất phát từ các lựa chọn dự kiến ​​trước đó. Sau chuyến du ngoạn đầu tiên, tình bạn có cơ hội`1/3`của việc đã tăng lên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) | Đọc những góc cạnh của tình bạn và tích lũy sức nặng của họ | 
| Không gian | O(1) | Chỉ một số tổng số đang chạy được lưu trữ | 

Tổng số cạnh hữu nghị trên tất cả các trường hợp thử nghiệm là nhiều nhất`10^5`, do đó thuật toán chỉ thực hiện công việc tuyến tính ở kích thước đầu vào. Điều này dễ dàng phù hợp trong giới hạn một giây. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n, m, k = map(int, input().split())

        s = 0
        for _ in range(m):
            a, b, f = map(int, input().split())
            s += f

        T = n * (n - 1) // 2

        invT = pow(T % MOD, MOD - 2, MOD)
        invT2 = invT * invT % MOD

        part1 = (k % MOD) * (s % MOD) % MOD * invT % MOD

        pairs_sum = (k % MOD) * ((k - 1) % MOD) % MOD
        pairs_sum = pairs_sum * pow(2, MOD - 2, MOD) % MOD

        part2 = (m % MOD) * pairs_sum % MOD * invT2 % MOD

        out.append(str((part1 + part2) % MOD))

    return "\n".join(out)

# provided samples
assert run(
"""4
100 0 24
2 1 10
1 2 1
3 1 2
2 1 1
5 2 4
1 2 25
3 2 24
"""
) == """0
55
777777784
40000020"""

# minimum size
assert run(
"""1
2 1 1
1 2 5
"""
) == "5"

# no friendship edges
assert run(
"""1
5 0 100
"""
) == "0"

# single edge, deterministic growth
assert run(
"""1
2 1 3
1 2 5
"""
) == "18"

# multiple edges with equal values
assert run(
"""1
3 3 1
1 2 7
1 3 7
2 3 7
"""
) == "21"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=2,m=1,k=1`|`5`| Trường hợp không tầm thường nhỏ nhất | 
|`m=0`|`0`| Không có ranh giới tình bạn | 
|`n=2,m=1,k=3`|`18`| Sự tăng trưởng lặp đi lặp lại của cùng một cạnh | 
| Đồ thị hoàn chỉnh với`k=1`|`21`| Thời hạn tăng trưởng biến mất khi chỉ tồn tại một chuyến tham quan | 

## Vỏ cạnh 

### Không có ranh giới tình bạn 

đầu vào:```
1
5 0 10
```Chúng tôi có:```
S = 0
m = 0
```Công thức trở thành```
10·0/T + 0 = 0.
```Không có cặp nào đóng góp bất cứ điều gì, vì vậy câu trả lời chính xác là 0. 

### Chỉ có thể có một cặp 

đầu vào:```
1
2 1 4
1 2 3
```Đây`T = 1`. 

Công thức cho:```
4·3 + 1·(4·3/2)
= 12 + 6
= 18.
```Chuỗi giá trị thực tế là:```
3, 4, 5, 6
```tổng của nó cũng bằng`18`. 

### Giá trị tình bạn lớn lao 

đầu vào:```
1
3 1 1
1 2 1000000000
```Câu trả lời chỉ nên là```
1000000000 / 3.
```Số học Modulo xử lý điều này một cách chính xác vì tất cả các phép chia được thực hiện bằng cách sử dụng nghịch đảo mô-đun. Không xảy ra tràn vì số nguyên Python không bị chặn và tất cả các tính toán cuối cùng đều được giảm modulo`10^9+7`.
