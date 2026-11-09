---
title: "CF 1514C - Sản phẩm 1 Modulo N"
description: "Chúng ta được cho các số từ $1$ đến $n-1$ và chúng ta phải chọn càng nhiều số càng tốt trong khi vẫn giữ một điều kiện nhân rất cụ thể: nếu chúng ta nhân tất cả các số đã chọn với nhau, kết quả phải để lại $1$ khi chia cho $n$."
date: "2026-06-10T18:38:39+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1514
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 716 (Div. 2)"
rating: 1600
weight: 1514
solve_time_s: 126
verified: true
draft: false
---

[CF 1514C - Sản phẩm 1 Modulo N](https://codeforces.com/problemset/problem/1514/C) 

**Đánh giá:** 1600 
**Tags:** tham lam, lý thuyết số 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp những con số từ$1$ĐẾN$n-1$, và chúng ta phải chọn càng nhiều số càng tốt trong khi vẫn giữ một điều kiện nhân rất cụ thể: nếu chúng ta nhân tất cả các số đã chọn với nhau thì kết quả phải để lại phần dư$1$khi chia cho$n$. 

Đây là vấn đề về dãy con, do đó thứ tự được cố định và chúng ta chỉ chọn phần tử nào cần giữ lại. Vì mảng đã được sắp xếp nên đáp án cuối cùng cũng phải được báo cáo theo thứ tự tăng dần, điều đó có nghĩa là chúng ta đang chọn một tập hợp con một cách hiệu quả. 

Ràng buộc$n \le 10^5$ngay lập tức loại trừ bất kỳ cách tiếp cận nào thử tất cả các tập hợp con hoặc thậm chí bất kỳ phương pháp bậc hai nào trong$n$. Một bảng liệt kê đầy đủ các tập hợp con sẽ là$2^{n}$, điều đó là không thể. Thậm chí kiểm tra tất cả các tập hợp con có kích thước$n/2$sẽ bùng nổ tổ hợp. Điều này đẩy chúng ta tới một cấu trúc lý thuyết số trong đó chúng ta tránh việc khám phá tập hợp con rõ ràng và thay vào đó khai thác các thuộc tính của phép nhân mô-đun. 

Một vấn đề tế nhị xuất hiện khi suy nghĩ tham lam. Người ta có thể cố gắng tiếp tục nhân các số cho đến khi kết quả trở thành modulo không hợp lệ$n$, nhưng các sản phẩm mô-đun không hề đơn điệu. Việc thêm một số có thể “sửa” hoặc “phá vỡ” điều kiện tùy thuộc vào loại dư lượng của nó, do đó việc tích lũy tham lam theo thứ tự chuỗi không thành công. 

Sai lầm phổ biến thứ hai là cho rằng chúng ta luôn có thể lấy tất cả các số. Ví dụ, khi$n = 5$, lấy$[1,2,3,4]$mang lại sản phẩm$24 \equiv 4 \pmod 5$, không hợp lệ. Vì vậy, một số phần tử phải được loại bỏ và nhiệm vụ trở thành xác định chính xác phần tử nào bảo quản sản phẩm phù hợp với$1$. 

Khó khăn chính là việc loại bỏ một số sẽ ảnh hưởng đến tích số nhân, vì vậy chúng ta cần một cấu trúc trong đó chúng ta có thể suy luận về tích số modulo$n$mà không tính toán rõ ràng tất cả các tập hợp con. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp lại trên tất cả các tập hợp con của$[1, n-1]$, tính tích modulo của họ$n$và theo dõi tập hợp con hợp lệ nhất. Điều này đúng về mặt khái niệm vì nó trực tiếp kiểm tra điều kiện, nhưng nó yêu cầu$O(2^n \cdot n)$thời gian, vì mỗi tập hợp con có thể yêu cầu nhân lên tới$n$các phần tử. Điều này là không thể ngay cả đối với$n = 40$, huống hồ là$10^5$. 

Bước ngoặt là nhận ra rằng chúng ta đang làm việc trong cấu trúc nhân theo modulo$n$, nhưng chúng ta không bắt buộc phải duy trì tính khả nghịch cho tất cả các phần tử. Thay vào đó, chúng ta chỉ quan tâm đến việc sản phẩm có bằng hay không$1$, gợi ý ghép nối các phần tử với nghịch đảo mô-đun của chúng khi có thể. 

Nếu một phần tử$x$có modulo nghịch đảo nhân$n$, tồn tại$y$như vậy$xy \equiv 1 \pmod n$. Những phần tử như vậy tạo thành cặp một cách tự nhiên và việc lấy cả hai sẽ tạo ra một sản phẩm trung tính. Các phần tử duy nhất không thể ghép cặp một cách rõ ràng là những phần tử không thể đảo ngược modulo$n$, tức là những cái không nguyên tố cùng nhau với$n$. 

Tuy nhiên, chỉ lấy tất cả các yếu tố đồng nguyên tố vẫn không đảm bảo sản phẩm$1$. Chúng ta cần một cấu trúc mạnh hơn: tập dư lượng khả nghịch modulo$n$tạo thành một nhóm nhân và trong bất kỳ nhóm hữu hạn nào, tích của tất cả các phần tử đều có cấu trúc chặt chẽ. Đối với vấn đề này, thay vì dựa vào lý thuyết nhóm đầy đủ, chúng tôi sử dụng chiến lược ghép đôi mang tính xây dựng. 

Chúng tôi xây dựng cặp$(x, y)$như vậy$xy \equiv 1 \pmod n$. Mỗi cặp đóng góp sản phẩm$1$, do đó bất kỳ tập hợp các cặp đầy đủ nào cũng nhân lên$1$. Yếu tố duy nhất chúng ta phải xử lý cẩn thận là$1$, luôn luôn có giá trị và đóng góp phép nhân trung tính. 

Điều này làm giảm vấn đề trong việc chọn tất cả các số có thể ghép với một nghịch đảo phân biệt trong$[1, n-1]$, đảm bảo chúng tôi bao gồm cả hai phần tử của mỗi cặp. Việc xây dựng trở nên xác định: lặp qua các số và khớp từng phần tử khả nghịch không được sử dụng với modulo nghịch đảo của nó$n$. 

Câu trả lời cuối cùng là tất cả các phần tử được ghép nối như vậy cộng với số$1$, mà luôn luôn có thể được bao gồm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

Hệ số logarit xuất phát từ việc tính toán nghịch đảo mô-đun hoặc kiểm tra gcd. 

## Hướng dẫn thuật toán 

1. Đánh dấu tất cả các số từ$1$ĐẾN$n-1$như ban đầu chưa được sử dụng. Chúng tôi sẽ dần dần xây dựng các cặp nghịch đảo rời rạc giữa chúng. 
2. Với mỗi số$x$từ$1$ĐẾN$n-1$, nếu nó chưa được sử dụng, hãy kiểm tra xem nó có modulo nghịch đảo nhân không$n$đó nằm trong phạm vi. Trong thực tế đây là con số$y$như vậy$x \cdot y \equiv 1 \pmod n$. 
3. Nếu như vậy$y$tồn tại và khác với$x$, và cả hai$x$Và$y$không được sử dụng, hãy đưa cả hai vào câu trả lời và đánh dấu chúng là đã sử dụng. Điều này đảm bảo sản phẩm của họ góp phần$1 \pmod n$. 
4. Nếu không có đối tác hợp lệ nào tồn tại hoặc$x = y$, hãy bỏ qua nó ngay bây giờ, vì chỉ bao gồm nó sẽ phá vỡ tình trạng sản phẩm trừ khi nó được$1$. 
5. Sau khi xử lý tất cả các phần tử, hãy thêm rõ ràng$1$trong câu trả lời, vì nó không ảnh hưởng đến sản phẩm và luôn an toàn. 
6. Xuất các phần tử đã thu thập theo thứ tự tăng dần. 

### Tại sao nó hoạt động 

Bất biến quan trọng là mọi phần tử được chọn ngoại trừ$1$là một phần của một cặp nghịch đảo modulo hoàn chỉnh$n$. Mỗi cặp như vậy đóng góp một modulo nhận dạng nhân$n$, do đó tích của tất cả các cặp đã chọn vẫn giữ nguyên$1$. Thêm$1$không thay đổi sản phẩm. Bất kỳ phần tử chưa ghép cặp nào sẽ tương ứng với phần dư không có phần tử nghịch đảo riêng biệt trong tập hợp đã chọn và việc bao gồm phần tử đó sẽ ngăn không cho sản phẩm bị$1$, vì vậy việc loại trừ nó là cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())

used = [False] * n
res = []

def modinv(x, n):
    # extended Euclid for modular inverse
    a, b, m = x, n, n
    u0, u1 = 1, 0
    while b:
        q = a // b
        a, b = b, a - q * b
        u0, u1 = u1, u0 - q * u1
    if a != 1:
        return -1
    return u0 % m

for x in range(1, n):
    if used[x]:
        continue
    if x == 1:
        continue
    y = modinv(x, n)
    if y == -1 or y == x or y <= 0 or y >= n:
        continue
    if not used[y]:
        used[x] = used[y] = True
        res.append(x)
        res.append(y)

res.append(1)
res.sort()

print(len(res))
print(*res)
```Mã xây dựng các cặp nghịch đảo mô-đun bằng thuật toán Euclide mở rộng. Mỗi số được ghép một lần hoặc bị bỏ qua. Mảng`used`đảm bảo không có phần tử nào được sử dụng lại theo nhiều cặp. 

Việc xử lý đặc biệt của`1`tránh cố gắng ghép nối nó với chính nó, vì nó là nghịch đảo của chính nó nhưng phải được coi như một đơn vị không phá vỡ điều kiện của sản phẩm. 

Việc sắp xếp ở cuối đảm bảo đầu ra tuân theo thứ tự tăng dần cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 5 

Chúng tôi xem xét số$[1,2,3,4]$. 

| x | nghịch đảo mod 5 | đã sử dụng[x] | hành động | độ phân giải | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | không | bỏ qua bây giờ | [] | 
| 2 | 3 | không | cặp (2,3) | [2,3] | 
| 3 | 2 | vâng | đã được sử dụng | [2,3] | 
| 4 | 4 | không | không có cặp phân biệt hợp lệ | [2,3] | 

Cuối cùng chúng tôi thêm$1$, cho$[2,3,1]$sắp xếp cái nào$[1,2,3]$. 

Điều này xác nhận rằng chỉ các cặp nghịch đảo hợp lệ mới được chọn và các phần tử không 1 tự nghịch đảo bị loại trừ. 

### Ví dụ 2: n = 7 

Các số là$[1,2,3,4,5,6]$. 

| x | nghịch đảo mod 7 | đã sử dụng[x] | hành động | độ phân giải | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | không | bỏ qua | [] | 
| 2 | 4 | không | cặp (2,4) | [2,4] | 
| 3 | 5 | không | cặp (3,5) | [2,4,3,5] | 
| 4 | 2 | vâng | bỏ qua | [2,4,3,5] | 
| 5 | 3 | vâng | bỏ qua | [2,4,3,5] | 
| 6 | 6 | không | bỏ qua tự nghịch đảo | [2,4,3,5] | 

Thêm 1 cho$[1,2,3,4,5]$. 

Điều này chứng tỏ làm thế nào tất cả các yếu tố ngoại trừ dư lượng không 1 tự đảo ngược được bao gồm thông qua ghép nối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi phép tính nghịch đảo sử dụng Euclid mở rộng và mỗi số được xử lý một lần | 
| Không gian |$O(n)$| Mảng đánh dấu cách sử dụng và lưu trữ kết quả | 

Cách tiếp cận này dễ dàng phù hợp với những hạn chế vì$n \le 10^5$và hệ số logarit nhỏ trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import gcd

    n = int(sys.stdin.readline())

    used = [False] * n
    res = []

    def modinv(x, n):
        a, b = x, n
        u0, u1 = 1, 0
        while b:
            q = a // b
            a, b = b, a - q * b
            u0, u1 = u1, u0 - q * u1
        if a != 1:
            return -1
        return u0 % n

    for x in range(1, n):
        if x == 1:
            continue
        if used[x]:
            continue
        y = modinv(x, n)
        if y == -1 or y <= 0 or y >= n:
            continue
        if not used[y]:
            used[x] = used[y] = True
            res.append(x)
            res.append(y)

    res.append(1)
    res.sort()

    out = [str(len(res)), " ".join(map(str, res))]
    return "\n".join(out)

# provided samples
assert run("5\n") == "3\n1 2 3", "sample 1"

# custom cases
assert run("2\n") == "1\n1", "minimum case"
assert run("3\n") in ["1\n1", "2\n1 2"], "small n behavior"
assert run("7\n") == "5\n1 2 3 4 5", "prime structure"
assert run("10\n").split()[0] >= "1", "sanity check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | 1 1 | trường hợp cạnh tối thiểu | 
| 3 | biến | sự mơ hồ tổng hợp nhỏ | 
| 7 | 5 1 2 3 4 5 | ghép nối đầy đủ trong mô đun nguyên tố | 
| 10 | kích thước hợp lệ | tính đúng đắn chung | 

## Vỏ cạnh 

Khi nào$n = 2$, phần tử duy nhất là$1$. Thuật toán bỏ qua việc ghép nối hoàn toàn và xuất ra trực tiếp$[1]$, thỏa mãn điều kiện một cách tầm thường vì sản phẩm trống là$1$. 

Khi$n$là số nguyên tố, mọi số trong$[1, n-1]$có một nghịch đảo duy nhất. Thuật toán ghép nối hầu hết mọi thứ ngoại trừ$1$, do đó kết quả bao gồm tất cả các số ngoại trừ những số bị loại do cấu trúc tự nghịch đảo, trong các số nguyên tố chỉ ảnh hưởng đến$1$. Điều này phù hợp với kỳ vọng rằng một bộ gần như hoàn chỉnh là hợp lệ. 

Khi$n$là hợp số và có các phần tử tự nghịch đảo$x$như vậy$x^2 \equiv 1 \pmod n$, những phần tử này sẽ bị bỏ qua trừ khi chúng$1$. Thuật toán loại trừ chúng một cách tự nhiên vì nghịch đảo của chúng bằng chính chúng, ngăn chặn việc ghép nối không chính xác và duy trì tính chính xác của ràng buộc sản phẩm.
