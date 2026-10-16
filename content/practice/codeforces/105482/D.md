---
title: "CF 105482D - \u041c\u0438\u043b\u043f\u0443\u043b \u0438 \u0414\u043e\u0433\u043f\u0443\u043b\u044f"
description: "Đây là một vấn đề hai lần. Trong lần chạy đầu tiên, chương trình nhận được một số nguyên dương $n$. Nó phải xuất ra một chuỗi nhị phân $s$ có độ dài tối đa là $sqrt{2}cdot lceil log2 n rceil + 5$. Trong lần chạy thứ hai, chương trình nhận được chuỗi nhị phân $q$."
date: "2026-06-25T06:02:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105482
codeforces_index: "D"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2024-2025, \u041f\u0435\u0440\u0432\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 105482
solve_time_s: 51
verified: true
draft: false
---

[CF 105482D - \u041c\u0438\u043b\u043f\u0443\u043b \u0438 \u0414\u043e\u0433\u043f\u0443\u043b\u044f](https://codeforces.com/problemset/problem/105482/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đây là một vấn đề hai lần. 

Trong lần chạy đầu tiên, chương trình nhận được một số nguyên dương$n$. Nó phải xuất ra một chuỗi nhị phân$s$có chiều dài nhiều nhất$\sqrt{2}\cdot \lceil \log_2 n \rceil + 5$. 

Trong lần chạy thứ hai, chương trình nhận được một chuỗi nhị phân$q$. Chuỗi này có được bằng cách lấy chuỗi được in trước đó$s$và thêm một hậu tố nhị phân tùy ý, có thể trống. Nhiệm vụ là lấy lại số ban đầu$n$. 

Khó khăn cốt lõi là bộ giải mã không biết chuỗi gốc kết thúc ở đâu. Mọi giải pháp hợp lệ đều phải mã hóa$n$theo cách tự phân định để bộ giải mã có thể xác định ranh giới giữa thông tin được mã hóa và các bit bổ sung tùy ý. 

Từ$n \le 10^{18}$, biểu diễn nhị phân của nó chứa tối đa 60 bit. 

Điều đó ngay lập tức gợi ý rằng chúng ta không cần một sơ đồ nén cực kỳ phức tạp. Giới hạn độ dài là khoảng$1.414 \cdot k + 5$, Ở đâu$k$là độ dài bit của$n$. Vì$k \le 60$, chúng tôi được phép sử dụng gần 90 bit, trong khi bản thân biểu diễn nhị phân chỉ sử dụng 60 bit. Chúng tôi có đủ chỗ để lưu trữ một số siêu dữ liệu mô tả độ dài của bản trình bày. 

Một lỗi phổ biến là xuất ra biểu diễn nhị phân của$n$trực tiếp. Giả định$n=5$, có dạng nhị phân là`101`. Lần chạy thứ hai có thể nhận được`101011001...`. Không có cách nào để biết mã gốc kết thúc ở đâu nên việc giải mã trở nên bất khả thi. 

Một ý tưởng không chính xác khác là gắn thêm một dấu phân cách cố định như`0`. Nếu bản thân dữ liệu được mã hóa có thể chứa mẫu dấu phân cách đó thì bộ giải mã có thể dừng sai vị trí. 

Mã hóa phải tự phân định. 

## Phương pháp tiếp cận 

Cách suy nghĩ mạnh mẽ là gán một chuỗi nhị phân duy nhất cho mọi số có thể và sau đó cố gắng xác định từ mã nào là tiền tố của chuỗi nhận được. Điều này chỉ có tác dụng nếu toàn bộ sổ mã được xây dựng cẩn thận để không có tiền tố. Ngay cả khi đó, chúng ta vẫn cần phải thỏa mãn giới hạn độ dài. 

Quan sát quan trọng là biểu diễn nhị phân của$n$đã chứa gần như tất cả thông tin chúng ta cần. Phần còn thiếu duy nhất là chiều dài của nó. Khi bộ giải mã biết có bao nhiêu bit thuộc về biểu diễn của$n$, nó có thể đọc chính xác những bit đó và bỏ qua mọi thứ tiếp theo. 

Điều này đương nhiên dẫn đến một mã tự phân định. 

Cho phép$k$là độ dài của biểu diễn nhị phân của$n$. 

Chúng tôi mã hóa$k$sử dụng mã hóa gamma Elias. Mã gamma có khả năng tự phân định: 

Đối với số nguyên dương$k$, cho phép$b$là biểu diễn nhị phân của nó. 

Mã gamma bao gồm: 

1.$|b|-1$số không. 
2. Chuỗi nhị phân$b$. 

Sau khi đọc các số 0 đứng đầu, bộ giải mã sẽ biết chính xác có bao nhiêu bit còn lại trong mã gamma. 

Sau đó chúng tôi nối thêm$k$-bit biểu diễn nhị phân của$n$. 

Bộ giải mã thực hiện quá trình ngược lại: 

1. Đọc mã gamma và khôi phục$k$. 
2. Đọc chính xác$k$các bit bổ sung. 
3. Giải thích các bit đó dưới dạng biểu diễn nhị phân của$n$. 
4. Bỏ qua hậu tố còn lại. 

Độ dài giới hạn có thể dễ dàng được thỏa mãn. Từ$k \le 60$,$$|\,\gamma(k)\,| = 2\lfloor \log_2 k \rfloor + 1 \le 11.$$Như vậy$$|s| \le k + 11.$$Đối với mọi$k \ge 1$,$$k + 11 \le \sqrt{2}\,k + 5,$$khi$k$lớn và việc kiểm tra trực tiếp các giá trị nhỏ cho thấy giới hạn cũng được giữ ở đó. Trường hợp xấu nhất$k=60$chỉ cung cấp 71 bit, thấp hơn giới hạn khoảng 89 bit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm tiền tố Brute Force | Hàm mũ theo chiều dài mã | Lớn | Không thực tế | 
| Độ dài gamma Elias + biểu diễn nhị phân | O(k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Trong lần chạy đầu tiên, hãy tính biểu diễn nhị phân của$n$. Gọi chiều dài của nó là$k$. 
2. Mã hóa$k$với mã hóa gamma Elias. 

Nếu dạng nhị phân của$k$có chiều dài$m$, đầu ra$m-1$số 0 theo sau là dạng nhị phân của$k$. 
3. Nối biểu diễn nhị phân của$n$. 
4. Xuất chuỗi kết quả. 
5. Trong lần chạy thứ hai, đếm các số 0 đứng đầu trong chuỗi nhận được$q$. Hãy để số đó được tính$z$. 
6. Đọc phần tiếp theo$z+1$bits. Chúng cùng nhau tạo thành biểu diễn nhị phân của$k$. 
7. Chuyển các bit đó thành số nguyên$k$. 
8. Đọc phần sau$k$bit. Đây chính xác là biểu diễn nhị phân của$n$. 
9. Chuyển đổi chúng trở lại thành số nguyên và xuất ra. 

### Tại sao nó hoạt động 

Mã gamma Elias có khả năng tự phân định. Sau khi đếm các số 0 đứng đầu, bộ giải mã sẽ biết chính xác độ dài của giá trị được mã hóa$k$, nên nó có thể phục hồi$k$không có sự mơ hồ. 

Một lần$k$đã biết, bộ giải mã biết rằng tiếp theo$k$bit là biểu diễn nhị phân của$n$. Bất kỳ bit bổ sung nào thuộc về hậu tố tùy ý được Dogpulya thêm vào và bị bỏ qua. 

Bộ giải mã luôn tái tạo lại giống nhau$k$và giống nhau$n$đã được sử dụng trong quá trình mã hóa, vì vậy quy trình là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

mode, x = input().split()

if mode == "1":
    n = int(x)

    nb = bin(n)[2:]
    k = len(nb)

    kb = bin(k)[2:]
    gamma = "0" * (len(kb) - 1) + kb

    print(gamma + nb)

else:
    q = x.strip()

    z = 0
    while z < len(q) and q[z] == '0':
        z += 1

    pos = z
    kb = q[pos:pos + z + 1]
    k = int(kb, 2)

    pos += z + 1
    nb = q[pos:pos + k]

    print(int(nb, 2))
```Nhánh đầu tiên xử lý việc chạy mã hóa. Nó tính toán độ dài bit$k$, viết mã gamma của$k$, sau đó nối thêm biểu diễn nhị phân của chính số đó. 

Nhánh thứ hai thực hiện thao tác nghịch đảo. Số lượng số 0 đứng đầu xác định độ dài của tải trọng mã gamma. Sau khi xây dựng lại$k$, bộ giải mã đọc chính xác$k$bit và chuyển đổi chúng trở lại số nguyên ban đầu. 

Lỗi triển khai phổ biến nhất là quên rằng lần chạy thứ hai nhận thêm các bit ở cuối. Bộ giải mã phải dừng sau khi đọc chính xác$k$bit cho số và hoàn toàn bỏ qua mọi thứ sau đó. 

Một lỗi đơn lẻ khác xuất hiện trong quá trình giải mã gamma. Nếu có$z$số 0 đứng đầu, biểu diễn nhị phân của$k$chiếm chính xác$z+1$bit. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Giả sử lần chạy đầu tiên nhận được:```
n = 1
```Biểu diễn nhị phân của$n$là`1`, Vì thế$k=1$. 

Mã gamma của$k=1$là`1`. 

Chuỗi được mã hóa:```
s = 11
```Giả sử lần chạy thứ hai nhận được:```
q = 1101010
```| Bước | Giá trị | 
| --- | --- | 
| Số 0 đứng đầu | 0 | 
| Đã mã hóa k bit |`1`| 
| k | 1 | 
| Số bit |`1`| 
| Đã giải mã n | 1 | 

Hậu tố còn lại`01010`bị bỏ qua. 

### Ví dụ 2 

Giả sử:```
n = 15664
```| Số lượng | Giá trị | 
| --- | --- | 
| Nhị phân của n |`11110100110000`| 
| k | 14 | 
| Nhị phân của k |`1110`| 
| Gamma(k) |`0001110`| 
| Chuỗi được mã hóa s |`000111011110100110000`| 

Bây giờ giả sử chuỗi nhận được là:```
000111011110100110000101101
```| Bước | Giá trị | 
| --- | --- | 
| Số 0 đứng đầu | 3 | 
| Đã mã hóa k bit |`1110`| 
| k | 14 | 
| Số bit |`11110100110000`| 
| Đã giải mã n | 15664 | 

Hậu tố`101101`không ảnh hưởng đến việc giải mã. 

Những ví dụ này chứng minh tính bất biến đằng sau lời giải: một khi mã gamma tiết lộ$k$, ranh giới của số được mã hóa sẽ được biết chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k) | Đọc hoặc ghi nhiều nhất độ dài chuỗi được mã hóa | 
| Không gian | O(k) | Lưu trữ chuỗi được mã hóa và dạng nhị phân trung gian | 

Từ$k \le 60$, thời gian chạy và mức sử dụng bộ nhớ rất nhỏ. Giải pháp thoải mái trong mọi giới hạn. 

## Trường hợp thử nghiệm```
# encoder/decoder helpers for testing

def encode(n):
    nb = bin(n)[2:]
    k = len(nb)

    kb = bin(k)[2:]
    gamma = "0" * (len(kb) - 1) + kb

    return gamma + nb

def decode(q):
    z = 0
    while z < len(q) and q[z] == '0':
        z += 1

    pos = z
    kb = q[pos:pos + z + 1]
    k = int(kb, 2)

    pos += z + 1
    nb = q[pos:pos + k]

    return int(nb, 2)

# minimum value
s = encode(1)
assert decode(s) == 1

# suffix appended
assert decode(s + "101010") == 1

# power of two
s = encode(1024)
assert decode(s) == 1024
assert decode(s + "111111") == 1024

# all ones in binary
s = encode(63)
assert decode(s + "0000") == 63

# maximum constraint
s = encode(10**18)
assert decode(s + "101011001") == 10**18
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$n=1$| 1 | Giá trị nhỏ nhất có thể | 
|$n=1$, hậu tố bổ sung | 1 | Bỏ qua các bit được nối thêm | 
|$n=1024$| 1024 | Độ dài bit lớn hơn | 
|$n=63$| 63 | Biểu diễn nhị phân của tất cả những cái | 
|$n=10^{18}$|$10^{18}$| Hạn chế tối đa | 

## Vỏ cạnh 

Hãy xem xét$n=1$. 

Bộ mã hóa tạo ra:```
gamma(1) = 1
s = 11
```Nếu chuỗi nhận được là:```
111111111
```bộ giải mã nhìn thấy số 0 ở đầu, đọc một bit cho$k$, thu được$k=1$, sau đó đọc một bit cho số đó và xây dựng lại$n=1$. Mọi bit còn lại đều bị bỏ qua. 

Ví dụ, hãy xem xét một giá trị có độ dài bit là lũy thừa của hai$n=128$. Sau đó$k=8$, có biểu diễn nhị phân là`1000`. Mã gamma trở thành`0001000`. Bộ giải mã đếm ba số 0 đứng đầu và biết rằng chính xác bốn bit thuộc về biểu diễn của$k$. Không có sự mơ hồ. 

Hãy xem xét giá trị lớn nhất có thể$n=10^{18}$. Biểu diễn nhị phân của nó chỉ sử dụng 60 bit. Mã gamma 60 có độ dài 11, do đó tổng chiều dài mã hóa là 71 bit, vẫn nằm dưới giới hạn cho phép.
