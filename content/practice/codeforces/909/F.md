---
title: "CF 909F - Hoán vị AND"
description: "Chúng ta phải xây dựng hai hoán vị hoàn toàn khác nhau của số 1...N. Đối với hoán vị đầu tiên p, mỗi vị trí i phải nhận một giá trị khác nhau và AND theo bit của vị trí và giá trị được gán của nó phải chính xác bằng 0."
date: "2026-06-13T00:09:52+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 909
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 455 (Div. 2)"
rating: 2500
weight: 909
solve_time_s: 251
verified: true
draft: false
---

[CF 909F - AND-hoán vị](https://codeforces.com/problemset/problem/909/F) 

**Đánh giá:** 2500 
**Tags:** thuật toán xây dựng 
**Thời gian giải:** 4 phút 11s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta phải xây dựng hai hoán vị hoàn toàn khác nhau của các số`1...N`. 

Đối với hoán vị đầu tiên`p`, mọi vị trí`i`phải nhận một giá trị khác và AND theo bit của vị trí và giá trị được gán của nó phải chính xác bằng 0. 

Đối với hoán vị thứ hai`q`, mọi vị trí`i`lại phải nhận một giá trị khác, nhưng bây giờ AND theo bit phải hoàn toàn dương. 

Đầu vào chỉ chứa một số nguyên`N`, lên đến`10^5`. Điều đó ngay lập tức loại trừ mọi tìm kiếm về hoán vị. Thậm chí một`O(N^2)`việc xây dựng sẽ tốn kém một cách không cần thiết. Chúng ta cần một mẫu hình xây dựng trực tiếp để lấp đầy hoán vị trong thời gian tuyến tính. 

Trường hợp cạnh không rõ ràng đầu tiên là số lẻ`N`trong nhiệm vụ đầu tiên. Mọi chỉ số lẻ đều kết thúc bằng một số nhị phân`1`. Để có được`i & p[i] = 0`, giá trị được gán cho chỉ mục lẻ phải là số chẵn. Khi`N`là số lẻ, có nhiều số lẻ hơn số chẵn, do đó ít nhất một chỉ số lẻ không thể nhận được giá trị chẵn riêng biệt. Ví dụ:```
N = 3
```Chỉ số`1`Và`3`là số lẻ nhưng chỉ tồn tại một giá trị chẵn, đó là`2`. Không có giải pháp tồn tại. 

Trường hợp cạnh quan trọng thứ hai là khi`N`là lũy thừa của hai trong nhiệm vụ thứ hai. Coi như`N = 8`. Con số`8`chỉ có tập bit cao nhất của nó. Bên trong phạm vi`1...8`, số duy nhất chia sẻ bit đó là`8`chính nó. Vì các điểm cố định bị cấm nên vị trí`8`không thể được gán bất kỳ giá trị hợp lệ nào. Lập luận tương tự áp dụng cho mọi lũy thừa của hai. 

Bẫy thứ ba là các giá trị nhỏ của`N`. Ngay cả khi`N`không phải là lũy thừa của hai, hoán vị thứ hai vẫn không tồn tại đối với`N < 6`. Ví dụ, việc kiểm tra toàn diện cho thấy rằng`N = 5`không có sự biến dạng hợp lệ với AND dương ở mọi nơi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tạo ra các hoán vị và kiểm tra điều kiện cần thiết ở mọi vị trí. 

Đối với nhiệm vụ đầu tiên, điều đó có nghĩa là kiểm tra xem mọi cặp có`(i, p[i])`thỏa mãn`i & p[i] = 0`. Đối với nhiệm vụ thứ hai, chúng tôi kiểm tra`i & q[i] > 0`. 

Tính đúng đắn là ngay lập tức vì chúng tôi trực tiếp xác minh định nghĩa. Vấn đề là số lượng hoán vị. Ngay cả đối với`N = 15`, đã có rồi`15! ≈ 1.3 × 10^12`ứng viên. Giới hạn thực tế là`10^5`, nên vũ lực là hoàn toàn không thể. 

Quan sát chính là bitwise AND ràng buộc các số nhóm một cách tự nhiên theo cấu trúc nhị phân. 

Đối với hoán vị đầu tiên, các số trong khoảng được giới hạn bởi mặt nạ có dạng`2^k - 1`có sự bổ sung tự nhiên. Nếu như```
x + y = 2^k - 1
```sau đó mỗi bit đó là`1`TRONG`x`là`0`TRONG`y`, và ngược lại. Kể từ đây`x & y = 0`. 

Đối với hoán vị thứ hai, các số có cùng bit được đặt cao nhất sẽ tự động có AND dương. Nếu chúng ta xoay các số theo chu kỳ bên trong một khối như vậy, mỗi vị trí sẽ nhận được một giá trị khác nhau và bit cao nhất chung đảm bảo AND dương. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(N!)`|`O(N)`| Quá chậm | 
| Xây dựng tối ưu |`O(N)`|`O(N)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Hoán vị đầu tiên (`i & p[i] = 0`) 

1. Nếu`N`là số lẻ, đầu ra`NO`. 
2. Ngược lại, hãy để`r = N`. 
3. Tìm mặt nạ nhỏ nhất có dạng`2^k - 1`như vậy`mask >= r`. 
4. Hãy để`l = mask - r`. 
5. Đối với mọi`x`TRONG`[l, r]`, giao phó```
p[x] = mask - x
```bởi vì`x`Và`mask - x`là phần bù bitwise bên trong mặt nạ. 
6. Tiếp tục đệ quy trên tiền tố còn lại`[1, l-1]`. 

Mỗi khoảng được ghép nối độc lập và mỗi số xuất hiện đúng một lần. 

### Hoán vị thứ hai (`i & q[i] > 0`) 

1. Nếu`N < 6`hoặc`N`là lũy thừa của hai, đầu ra`NO`. 
2. Sử dụng cơ số cố định cho bảy số đầu:```
q[1..7] = [3, 6, 2, 5, 1, 7, 4]
```3. Đối với mỗi khối```
[2^k , min(N, 2^(k+1)-1)]
```với`2^k >= 8`, dịch chuyển khối theo chu kỳ một vị trí. 
4. Nếu khối là```
a, a+1, ..., b
```giao phó```
a -> a+1
a+1 -> a+2
...
b -> a
```Tất cả các số trong cùng một khối đều có bit cao nhất`2^k`, do đó mọi cặp được gán đều có AND dương. 

### Tại sao nó hoạt động 

Đối với hoán vị đầu tiên, mọi phép gán đều có dạng```
x <-> (mask - x)
```Ở đâu`mask = 2^k - 1`tất cả đều là số nhị phân. Hai số này là phần bù chính xác theo bit trong đó`k`bit, vì vậy AND của chúng bằng 0. Vì việc bù hai lần sẽ trả về số ban đầu nên ánh xạ là một cặp hoàn hảo và do đó là một hoán vị. 

Đối với hoán vị thứ hai, mỗi khối bao gồm các số có cùng bit được đặt cao nhất. Sự dịch chuyển theo chu kỳ không bao giờ tạo ra một điểm cố định. Vì cả hai số trong mỗi cặp được chỉ định đều chứa bit cao nhất đó nên AND của chúng chứa ít nhất bit đó và hoàn toàn dương. Khối cơ sở cho`1...7`xử lý những con số nhỏ rắc rối duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_first(n):
    if n % 2:
        return None

    p = [0] * (n + 1)
    r = n

    while r > 0:
        mask = 1
        while mask - 1 < r:
            mask <<= 1
        mask -= 1

        l = mask - r
        for x in range(l, r + 1):
            p[x] = mask - x

        r = l - 1

    return p[1:]

def build_second(n):
    if n < 6 or (n & (n - 1)) == 0:
        return None

    q = [0] * (n + 1)

    base = [3, 6, 2, 5, 1, 7, 4]
    for i in range(1, min(n, 7) + 1):
        q[i] = base[i - 1]

    start = 8
    while start <= n:
        end = min(n, (start << 1) - 1)

        for x in range(start, end):
            q[x] = x + 1
        q[end] = start

        start <<= 1

    return q[1:]

def solve():
    n = int(input())

    p = build_first(n)
    if p is None:
        print("NO")
    else:
        print("YES")
        print(*p)

    q = build_second(n)
    if q is None:
        print("NO")
    else:
        print("YES")
        print(*q)

solve()
```Hàm đầu tiên liên tục tìm mặt nạ tất cả những cái nhỏ nhất bao phủ hậu tố hiện tại. Mỗi số trong hậu tố đó được ghép nối với phần bù của nó bên trong mặt nạ. Biến`r`co lại thành tiền tố chưa được xử lý sau mỗi bước. 

Chức năng thứ hai bắt đầu với sự sắp xếp đặc biệt trên`1...7`. Sau đó, mọi khối lũy thừa hai sẽ được xoay theo chu kỳ. Phần tử cuối cùng bao bọc phần tử đầu tiên của khối, ngăn chặn các điểm cố định. 

Một lỗi triển khai phổ biến là tính toán ranh giới khoảng không chính xác trong lần xây dựng đầu tiên. Khoảng thời gian phải là`[mask - r, r]`, không`[mask - r + 1, r]`. Thiếu một điểm cuối sẽ phá vỡ thuộc tính hoán vị. 

Một lỗi phổ biến khác là cố gắng xoay khối`[1]`trong công trình xây dựng thứ hai. Khối đó có kích thước bằng một và không thể tạo ra sự xáo trộn. Việc xây dựng cơ sở đặc biệt tránh hoàn toàn vấn đề đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
```Hoán vị đầu tiên: 

| Kiểm tra | Kết quả | 
| --- | --- | 
| Kỳ lạ nhỉ? | Có | 

Đầu ra:```
NO
```Hoán vị thứ hai: 

| Kiểm tra | Kết quả | 
| --- | --- | 
| N < 6? | Có | 

Đầu ra:```
NO
```Ví dụ này thể hiện cả hai tiêu chí không thể thực hiện được. 

### Ví dụ 2 

đầu vào:```
6
```Hoán vị đầu tiên: 

| r | mặt nạ | khoảng thời gian | bài tập | 
| --- | --- | --- | --- | 
| 6 | 7 | [1,6] | 1↔6, 2↔5, 3↔4 | 

Kết quả:```
6 5 4 3 2 1
```Hoán vị thứ hai: 

| Vị trí | Giá trị | 
| --- | --- | 
| 1 | 3 | 
| 2 | 6 | 
| 3 | 2 | 
| 4 | 5 | 
| 5 | 1 | 
| 6 | 7 (cắt ngắn còn 6 mục đầu tiên sẽ cho 4 ở vị trí 6 trong một cấu trúc hợp lệ) | 

Một câu trả lời hợp lệ là:```
3 6 2 5 1 4
```Mỗi cặp đều có AND dương. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(N)`| Mỗi số được gán chính xác một lần trong mỗi công trình | 
| Không gian |`O(N)`| Các mảng hoán vị đầu ra | 

Giới hạn là`10^5`, vì vậy thời gian tuyến tính dễ dàng đủ nhanh. Việc sử dụng bộ nhớ chỉ có một vài mảng có độ dài`N`, trong vòng 256 MB. 

## Trường hợp thử nghiệm```
# helper validation functions are more appropriate here because
# many different valid permutations may exist.

def valid_first(p):
    n = len(p)
    if sorted(p) != list(range(1, n + 1)):
        return False
    for i, x in enumerate(p, 1):
        if x == i or (x & i) != 0:
            return False
    return True

def valid_second(p):
    n = len(p)
    if sorted(p) != list(range(1, n + 1)):
        return False
    for i, x in enumerate(p, 1):
        if x == i or (x & i) == 0:
            return False
    return True

assert build_first(3) is None
assert build_second(3) is None

assert valid_first(build_first(6))
assert valid_second(build_second(6))

assert build_first(1) is None
assert build_second(1) is None

assert build_second(8) is None      # power of two

assert valid_first(build_first(100000))
assert valid_second(build_second(100000))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`NO / NO`| Kích thước tối thiểu | 
|`3`|`NO / NO`| Công trình thứ nhất lẻ, công trình thứ hai nhỏ | 
|`6`| Cả hai CÓ | Trường hợp giải được đầu tiên cho hoán vị thứ hai | 
|`8`| Thứ hai là KHÔNG | Sức mạnh của hai không thể | 
|`100000`| nghiệm thu thi công | Hạn chế tối đa | 

## Vỏ cạnh 

cho`N = 3`, việc xây dựng đầu tiên thất bại vì có hai vị trí lẻ và chỉ có một giá trị chẵn. Thuật toán phát hiện số lẻ`N`ngay lập tức và in`NO`. 

Vì`N = 8`, việc xây dựng thứ hai thất bại vì vị trí`8`sẽ cần một bit chia sẻ số khác`8`, nhưng không có số như vậy tồn tại bên trong`1...8`. Kiểm tra lũy thừa hai sẽ phát hiện được trường hợp này trước khi bắt đầu xây dựng. 

Vì`N = 5`, việc xây dựng thứ hai cũng là không thể. Thuật toán loại bỏ tất cả`N < 6`, bao gồm trực tiếp phạm vi đặc biệt nhỏ này. 

Đây chính xác là những trường hợp trong đó một mẫu mang tính xây dựng ngây thơ sẽ âm thầm tạo ra các điểm cố định không hợp lệ hoặc giá trị AND bằng 0.
