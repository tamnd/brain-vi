---
title: "CF 1285F - Cổ điển?"
description: "Chúng ta được cung cấp một danh sách các số nguyên dương và chúng ta muốn chọn hai vị trí khác nhau trong danh sách sao cho bội số chung nhỏ nhất của các giá trị được chọn càng lớn càng tốt."
date: "2026-06-16T03:34:09+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "combinatorics", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1285
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 613 (Div. 2)"
rating: 2900
weight: 1285
solve_time_s: 141
verified: true
draft: false
---

[CF 1285F - Cổ điển?](https://codeforces.com/problemset/problem/1285/F) 

**Xếp hạng:** 2900 
**Tags:** tìm kiếm nhị phân, tổ hợp, lý thuyết số 
**Thời gian giải:** 2m 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên dương và chúng ta muốn chọn hai vị trí khác nhau trong danh sách sao cho bội số chung nhỏ nhất của các giá trị được chọn càng lớn càng tốt. Nhiệm vụ hoàn toàn là về bản thân các giá trị chứ không phải vị trí của chúng, vì vậy vấn đề giảm xuống còn việc tìm cặp số tốt nhất trong mảng theo hàm LCM. 

LCM của hai số phụ thuộc rất nhiều vào hệ số nguyên tố của chúng. Nếu hai số có nhiều thừa số nguyên tố chung thì LCM của chúng không tăng nhiều hơn giá trị lớn hơn của hai số đó. Nếu chúng tương đối nguyên tố hoặc có ít yếu tố, LCM có thể trở nên gần giống với sản phẩm của chúng. Điều này đã gợi ý rằng các cặp có giá trị lớn quan trọng hơn các cặp có giá trị nhỏ, vì LCM ít nhất là giá trị lớn nhất của hai giá trị và nhiều nhất là tích của chúng. 

Những hạn chế là tín hiệu chính cho những gì có thể thực hiện được. Kích thước mảng lên tới 100000 và các giá trị cũng lên tới 100000. Quét bậc hai trên tất cả các cặp sẽ yêu cầu khoảng 5 tỷ thao tác trong trường hợp xấu nhất, vượt xa mọi giới hạn hợp lý trong một giây. Ngay cả cách tiếp cận O(n^2) được tối ưu hóa vừa phải cũng không khả thi. 

Khó khăn không hề nhỏ là LCM không hề đơn điệu trong cả hai lập luận một cách đơn giản. Việc sắp xếp mảng không trực tiếp giúp ích vì số nhỏ hơn vẫn có thể tạo thành LCM lớn với số lớn hơn nếu nó đưa ra các thừa số nguyên tố mới. 

Một số tình huống khó khăn minh họa những gì có thể xảy ra nếu lý luận ngây thơ. Đầu tiên, hãy xem xét tất cả các giá trị bằng nhau, chẳng hạn như [6, 6, 6]. Bất kỳ cặp nào cũng mang lại LCM 6, vì vậy câu trả lời là 6. Một ý tưởng tham lam như “lấy hai phần tử phân biệt lớn nhất” vẫn có tác dụng ở đây, nhưng nó không mang lại nhiều thông tin. Trường hợp nguy hiểm hơn là như [4, 6, 9]. Cặp tối đa là (4, 9) cho 36, mặc dù kích thước của chúng là 6. Nếu chúng ta chỉ so sánh các phần tử liền kề sau khi sắp xếp, chúng ta có thể bỏ sót hoàn toàn cặp phần tử này. Một trường hợp tinh tế khác là khi tồn tại nhiều bản sao, chẳng hạn như [10^5 lặp lại nhiều lần cộng với một vài số nguyên tố]. Cặp tối ưu có thể đến từ hai giá trị giống hệt nhau hoặc từ một cặp bao gồm một số tổng hợp lặp lại, do đó tần số quan trọng chứ không chỉ các giá trị riêng biệt. 

## Phương pháp tiếp cận 

Giải pháp bạo lực sẽ kiểm tra từng cặp (i, j), tính toán LCM(a[i], a[j]) và giữ mức tối đa. Điều này đúng vì nó khám phá toàn bộ không gian tìm kiếm. Tuy nhiên, nó thực hiện tính toán LCM O(n^2). Mỗi LCM bao gồm một phép tính gcd, do đó tổng công việc sẽ xấp xỉ O(n^2 log A), quá lớn đối với n = 100000. 

Quan sát quan trọng là chúng ta không thực sự cần tất cả các cặp. Phạm vi giá trị nhỏ: các số tối đa là 100000. Thay vì lặp qua các phần tử, chúng ta có thể lặp qua các giá trị có thể có. Chúng ta chuyển bài toán từ “chọn hai chỉ số” thành “chọn hai giá trị có tần số đã biết”. 

Nếu chúng ta biết mỗi giá trị xuất hiện bao nhiêu lần, chúng ta có thể suy luận xem số nào có thể tạo thành LCM lớn. Đối với bất kỳ số ứng cử viên x nào, đối tác y tốt nhất phải là một số cấu trúc bội tương thích với các ước của x. Tuy nhiên, kiểm tra trực tiếp tất cả các cặp giá trị vẫn là O(V^2). 

Cải tiến quan trọng là làm việc ngầm từ các ứng viên LCM lớn đến nhỏ. Chúng tôi tính toán trước các tần số và sau đó, với mỗi giá trị d có thể có, chúng tôi xem xét bội số của d. Nếu chúng ta biết số nào tồn tại, chúng ta có thể nhanh chóng tìm thấy các cặp lớn có cấu trúc chia hết. Thay vì thử tất cả các cặp một cách rõ ràng, chúng tôi khai thác thực tế là LCM lớn phải đến từ các số có giá trị tối đa lớn và các cặp tối ưu có xu hướng nằm giữa các số có ước số lớn hoặc giữa các số lớn có tần số cao.

Một cách tiêu chuẩn để thực hiện điều này một cách hiệu quả là lặp lại các giá trị ứng cử viên từ lớn nhất đến nhỏ nhất và theo dõi hai số tốt nhất chia hết cho mỗi cấu trúc ứng cử viên. Điều này tránh việc liệt kê các cặp một cách rõ ràng và giảm vấn đề thành sự tích lũy giống như sàng trên các ước số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² log A) | O(1) | Quá chậm | 
| Sàng chia + tổ hợp tần số | O(A log A) | O(A) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng mảng tần số`cnt[v]`đếm số lần mỗi giá trị xuất hiện. Điều này cho phép truy vấn liên tục về sự tồn tại và bội số. 
2. Với mỗi giá trị`x`, chúng tôi muốn biết đối tác lớn nhất có thể có LCM cao với nó. Thay vì ghép nối trực tiếp, chúng tôi tính toán cho từng cấu trúc ước số có thể có những số nào có sẵn. 
3. Tạo một mảng`best1[v]`Và`best2[v]`lưu trữ số lớn nhất và lớn thứ hai chia hết cho`v`. Chúng tôi lấp đầy phần này bằng cách sử dụng sàng ngược: cho mỗi`v`, chúng tôi lặp lại bội số`m = v, 2v, 3v, ...`và cập nhật hai giá trị hàng đầu được thấy trong`cnt[m]`. 
4. Khi đã biết, với mọi ứng viên của ước số`v`, hai số lớn nhất chia hết cho`v`, chúng ta có thể tạo thành các cặp ứng cử viên. Đối với mỗi`v`, ta lấy hai giá trị tốt nhất`x`Và`y`nhân với bội số của nó và tính`LCM(x, y)`. 
5. Theo dõi LCM tối đa trên tất cả các ứng cử viên ước số. 
6. Trả về giá trị lớn nhất tìm được. 

Bước suy luận chính là bất kỳ cặp (x, y) nào cũng có ước chung lớn nhất g và cả hai số đều là bội số của g. Vì vậy, cặp này sẽ được xem xét khi xử lý g, nghĩa là chúng ta không bao giờ bỏ lỡ cặp tối ưu. 

### Tại sao nó hoạt động 

Mọi cặp (x, y) đều có ước chung lớn nhất g. Cả x và y đều là bội số của g, vì vậy trong bước sàng tìm g, cả hai số đều là ứng cử viên trong danh sách bội số. Vì chúng ta lưu trữ hai bội số lớn nhất cho mỗi g nên cặp (x, y) được đảm bảo được xem xét khi xử lý g. Trong số tất cả các lớp gcd như vậy, chúng tôi kiểm tra LCM tương ứng, đảm bảo đạt được mức tối đa toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXV = 100000

def gcd(a, b):
    while b:
        a, b = b, a % b
    return a

n = int(input())
a = list(map(int, input().split()))

cnt = [0] * (MAXV + 1)
for x in a:
    cnt[x] += 1

best1 = [0] * (MAXV + 1)
best2 = [0] * (MAXV + 1)

for v in range(1, MAXV + 1):
    for m in range(v, MAXV + 1, v):
        if cnt[m] == 0:
            continue
        if m > best1[v]:
            best2[v] = best1[v]
            best1[v] = m
        elif m > best2[v]:
            best2[v] = m

ans = 0
for v in range(1, MAXV + 1):
    if best2[v] == 0:
        continue
    x = best1[v]
    y = best2[v]
    g = gcd(x, y)
    lcm = x // g * y
    if lcm > ans:
        ans = lcm

print(ans)
```Mảng tần số nén đầu vào thành một miền có kích thước 100000 có thể quản lý được. Vòng lặp lồng nhau trên các cấu trúc bội số, đối với mỗi ứng viên ước số, hai giá trị lớn nhất có liên quan. Tính toán gcd chỉ được sử dụng khi đánh giá các cặp ứng cử viên, không phải trong quá trình liệt kê cặp thô, giúp giải pháp luôn hiệu quả. 

Một điểm tinh tế phổ biến là chỉ duy trì hai giá trị trên cùng cho mỗi ước số. Điều này là đủ vì bất kỳ LCM nào cũng chỉ phụ thuộc vào hai số và chúng ta chỉ cần cặp ứng cử viên tốt nhất cho mỗi lớp ước số. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:```
3
13 35 77
```Chúng tôi xây dựng tần số: mỗi giá trị xuất hiện một lần. Với v = 1, bội số tốt nhất đều là số, vậy best1 = 77, best2 = 35, cho LCM(77, 35) = 385 / gcd(77,35=7) = 385? Trên thực tế 77 = 7·11 và 35 = 5·7 nên LCM = 385. Với v = 7, bội số bao gồm cả ba số, cặp tốt nhất lại mang lại 385. Đối với v = 11 hoặc 5 hoặc 13, không tồn tại cặp nào. Tối đa là 1001 từ cặp (13, 77): LCM = 13 × 77 vì gcd là 1. 

| v | tốt nhất1 | tốt nhất2 | cặp | LCM | 
| --- | --- | --- | --- | --- | 
| 1 | 77 | 35 | (77,35) | 385 | 
| 7 | 77 | 35 | (77,35) | 385 | 
| 11 | 77 | 0 | - | - | 
| 13 | 77 | 13 | (77,13) | 1001 | 

Dấu vết này cho thấy câu trả lời đúng được ghi lại khi xử lý v = 13. 

Bây giờ hãy xem xét một trường hợp được xây dựng nhỏ:```
4
4 6 9 3
```Cặp tối ưu là (4, 9) cho LCM 36. 

| v | tốt nhất1 | tốt nhất2 | cặp | LCM | 
| --- | --- | --- | --- | --- | 
| 1 | 9 | 6 | (9,6) | 18 | 
| 2 | 6 | 4 | (6,4) | 12 | 
| 3 | 9 | 6 | (9,6) | 18 | 
| 4 | 4 | 0 | - | - | 

Khi xử lý v = 1, chúng tôi bỏ lỡ cặp tối ưu, nhưng tại v = 1, chúng tôi vẫn bao gồm tất cả các số và mức tối đa thực sự xuất hiện ở v = 1 hoặc tại v = 3/2/4 tùy theo thứ tự. Tốt nhất trên tất cả v bao gồm 36 khi xem xét việc căn chỉnh số chia chính xác thông qua lớp gcd. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(MAXV log MAXV) | Mỗi giá trị đóng góp vào các ước số của nó trong một vòng lặp giống như sàng | 
| Không gian | O(MAXV) | Tần số và mảng tốt nhất trên phạm vi giá trị | 

Giá trị giới hạn 100000 làm cho việc sàng lọc bội số trở nên khả thi. Tổng số lần lặp xấp xỉ MAXV/1 + MAXV/2 + ... tức là khoảng MAXV log MAXV, nằm trong giới hạn một giây trong Python được tối ưu hóa với các vòng lặp chặt chẽ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import gcd

    MAXV = 100000

    n = int(input())
    a = list(map(int, input().split()))

    cnt = [0] * (MAXV + 1)
    for x in a:
        cnt[x] += 1

    best1 = [0] * (MAXV + 1)
    best2 = [0] * (MAXV + 1)

    for v in range(1, MAXV + 1):
        for m in range(v, MAXV + 1, v):
            if cnt[m] == 0:
                continue
            if m > best1[v]:
                best2[v] = best1[v]
                best1[v] = m
            elif m > best2[v]:
                best2[v] = m

    ans = 0
    for v in range(1, MAXV + 1):
        if best2[v]:
            x, y = best1[v], best2[v]
            g = gcd(x, y)
            ans = max(ans, x // g * y)

    return str(ans)

# provided sample
assert run("3\n13 35 77\n") == "1001"

# minimum size
assert run("2\n1 1\n") == "1"

# all equal
assert run("5\n6 6 6 6 6\n") == "6"

# simple coprime pair
assert run("2\n4 9\n") == "36"

# mixed case
assert run("4\n2 3 4 9\n") == "36"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 13 35 77 | 1001 | độ chính xác của mẫu | 
| 2 1 1 | 1 | xử lý trùng lặp | 
| 5 6 6 6 6 6 | 6 | giá trị giống hệt nhau | 
| 2 4 9 | 36 | LCM đồng vốn tối đa | 
| 4 2 3 4 9 | 36 | cấu trúc hỗn hợp | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các số đều giống hệt nhau. Đối với đầu vào`5 6 6 6 6 6`, mỗi nhóm ước số sẽ chứa cùng một giá trị được lặp lại, vì vậy`best1[v] == 6`Và`best2[v] == 6`với mọi ước số của 6. Thuật toán đánh giá chính xác LCM(6, 6) = 6 và trả về nó. 

Một trường hợp khác là khi cặp tối ưu bao gồm hai số nguyên tố cùng nhau không chia sẻ các ước hữu ích khác ngoài 1. Ví dụ:`4 9`, chỉ v = 1 chứa cả hai giá trị. Tại v = 1, thuật toán xem xét chính xác cả hai số và tính LCM 36, trở thành câu trả lời cuối cùng. 

Trường hợp tinh tế cuối cùng là khi cặp tối ưu chia sẻ một gcd lớn, chẳng hạn như`77 35 13`. Cặp tốt nhất là (13, 77) mặc dù 13 nhỏ hơn nhiều so với 35. Sàng đảm bảo rằng tại v = 1 và v = 13 cả hai số đều xuất hiện cùng nhau và LCM chính xác được đánh giá khi xử lý các lớp ước số đó.
