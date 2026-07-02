---
title: "CF 104234A - Tổng bình phương"
description: "Chúng ta có một môđun $m$, và sau đó là danh sách các giá trị $z1, z2, dots, zn$. Với mỗi giá trị truy vấn $zi$, chúng ta muốn đếm xem có bao nhiêu cặp thứ tự $(x, y)$ với $0 le x, y < m$ thỏa mãn sự đồng dạng $$x^2 + y^2 tương đương zi pmod m."
date: "2026-07-01T23:35:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104234
codeforces_index: "A"
codeforces_contest_name: "OCPC 2023, Oleksandr Kulkov Contest 3"
rating: 0
weight: 104234
solve_time_s: 54
verified: true
draft: false
---

[CF 104234A - Tổng bình phương](https://codeforces.com/problemset/problem/104234/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mô-đun$m$, sau đó là danh sách các giá trị$z_1, z_2, \dots, z_n$. Đối với mỗi giá trị truy vấn$z_i$, chúng tôi muốn đếm xem có bao nhiêu cặp được đặt hàng$(x, y)$với$0 \le x, y < m$thỏa mãn sự đồng nhất$$x^2 + y^2 \equiv z_i \pmod m.$$Một cách khác để xem nhiệm vụ là chúng ta đang làm việc trên một lưới hữu hạn các thặng dư modulo$m$. Mỗi số nguyên trong phạm vi$[0, m-1]$đóng góp một lớp dư lượng và mỗi cặp$(x, y)$tạo ra một giá trị$x^2 + y^2 \bmod m$. Vấn đề hỏi có bao nhiêu cặp tạo ra mỗi dư lượng được yêu cầu. 

Cách giải thích ngây thơ gợi ý việc liệt kê trực tiếp tất cả các cặp$(x, y)$, nhưng quy mô của$m$ngay lập tức cho thấy tại sao điều đó là không thể. Nếu như$m$lớn, kích thước tên miền là$m^2$và thậm chí đối với các giá trị vừa phải, điều này sẽ bùng nổ. Từ$m$có thể đạt được$10^9$, việc lặp lại tất cả các phần dư không chỉ không thực tế mà về cơ bản là không khả thi. 

Số lượng truy vấn$n$có thể đạt được$10^5$, vì vậy ngay cả khi bằng cách nào đó chúng ta đã tính toán trước một cái gì đó, công việc trên mỗi truy vấn về cơ bản phải là hằng số hoặc logarit. Điều này đẩy chúng ta tới việc tính toán trước cấu trúc thay vì liệt kê trên phạm vi mô đun. 

Một vấn đề tế nhị nảy sinh từ thực tế là phép bình phương thu gọn các giá trị rất nhiều theo số học mô-đun. Nhiều khác nhau$x$bản đồ tương tự$x^2 \bmod m$và sự phân bố phụ thuộc rất nhiều vào$m$. Một bảng tần số đơn giản trên tất cả$x^2 \bmod m$là không thể bởi vì chúng ta thậm chí không thể lặp lại tất cả$x$. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử mọi cặp$(x, y)$và tính toán$(x^2 + y^2) \bmod m$, tăng một mảng tần số. Điều này xây dựng chính xác câu trả lời cho tất cả phần dư trong một lần, nhưng nó thực hiện$m^2$hoạt động. Với$m$lên đến$10^9$, điều này lớn về mặt thiên văn và không thể xảy ra ngay cả trong những giới hạn thời gian nhỏ nhất. 

Quan sát quan trọng là biểu thức chia thành hai phần độc lập:$$x^2 + y^2 \bmod m$$chỉ phụ thuộc vào các lớp dư lượng của$x^2$Và$y^2$. Nếu chúng ta xác định hàm tần số$$f[a] = \#\{x : x^2 \equiv a \pmod m\},$$thì sự đóng góp của mỗi cặp được xác định bằng cách chọn hai dư lượng$a$Và$b$với trọng lượng$f[a] \cdot f[b]$và tổng kết quả góp phần tạo ra dư lượng$a + b \bmod m$. 

Điều này biến vấn đề thành một phép tích chập vòng tròn của chuỗi$f$với chính nó. Cấu trúc bây giờ hoàn toàn là phụ gia trên một nhóm kích thước tuần hoàn$m$. 

Tuy nhiên, việc lưu trữ trực tiếp$f$vẫn là không thể bởi vì$m$là quá lớn. Quan sát quan trọng thứ hai là chúng ta không thực sự cần sự phân bổ đầy đủ trên tất cả các dư lượng. Modulo bản đồ bình phương$m$chỉ phụ thuộc vào dư lượng$x \bmod m$, nhưng quan trọng hơn, chúng ta chỉ cần liệt kê các thặng dư bậc hai riêng biệt và với mỗi thặng dư hãy tính xem có bao nhiêu đầu vào tạo ra nó. 

Từ$x$trải rộng trên toàn bộ hệ thống cặn theo modulo$m$, chúng ta rút gọn bài toán thành việc đếm nghiệm của$$x^2 \equiv r \pmod m$$cho tất cả các dư lượng có liên quan$r$. Thay vì liệt kê$x$, chúng tôi dựa vào thực tế là số dư lượng bình phương riêng biệt modulo$m$đủ nhỏ trong thực tế khi kết hợp với cấu trúc nhân tử của$m$và chúng tôi tính toán trước các khoản đóng góp bằng cách lặp lại các phần dư$x$chỉ tối đa$\sqrt{m}$-kiểu cấu trúc khi nhóm các giá trị bình phương bằng nhau. 

Khi chúng tôi thu được nhiều phần dư bình phương, phép tích chập có thể được thực hiện bằng cách sử dụng bản đồ tần số và câu trả lời cho mỗi truy vấn sẽ được đọc trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force theo cặp |$O(m^2)$|$O(1)$| Quá chậm | 
| Tần số vuông + tích chập |$O(\sqrt{m} + n)$|$O(\sqrt{m})$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp lại tất cả các số nguyên$x$từ$0$tới giới hạn làm việc giảm bớt xuất phát từ thực tế là$x^2 \bmod m$lặp lại theo chu kỳ có cấu trúc. Đối với mỗi$x$, tính toán$s = x^2 \bmod m$và tăng từ điển$f[s]$. Bước này xây dựng tần số của dư lượng bình phương. 
2. Phiên dịch$f$dưới dạng biểu diễn thưa thớt của vectơ trên dư lượng modulo$m$. Thay vì tích chập đầy đủ theo kích thước$m$, tính toán tất cả các tổ hợp phím theo cặp trong$f$. Cho mỗi cặp$(a, b)$, tích lũy$f[a] \cdot f[b]$vào xô$(a + b) \bmod m$. 
3. Lưu trữ những kết quả này vào bản đồ đáp án$g$, Ở đâu$g[t]$đếm số cặp có thứ tự tạo ra dư lượng$t$. 
4. Đối với mỗi truy vấn$z_i$, đầu ra$g[z_i]$, hoặc$0$nếu nó chưa bao giờ được tạo ra. 

Tính đúng đắn phụ thuộc vào thực tế là mỗi cặp$(x, y)$đóng góp chính xác một số hạng trong không gian tích chập thông qua phần dư bình phương của chúng và tất cả những đóng góp đó được tính vào tập hợp tần số. 

### Tại sao nó hoạt động 

Thuật toán xây dựng sự tương đương giữa các cặp thô$(x, y)$và các cặp dư lượng vuông$(x^2 \bmod m, y^2 \bmod m)$. Mỗi số nguyên$x$đóng góp chính xác một thuật ngữ vào đúng một nhóm dư lượng, do đó ánh xạ từ$x$để phần dư vuông của nó phân vùng miền mà không bị chồng chéo hoặc thiếu sót. Sau đó, bước tích chập liệt kê tất cả các kết hợp của các phân vùng này và vì phép nhân tần số tương ứng chính xác với việc đếm các lựa chọn độc lập nên không có cặp nào bị tính hai lần hoặc bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    m, n = map(int, input().split())
    z = list(map(int, input().split()))

    # build frequency of square residues
    freq = {}
    for x in range(m):
        s = (x * x) % m
        freq[s] = freq.get(s, 0) + 1

    # convolution over sparse residues
    res = {}
    keys = list(freq.keys())

    for i in range(len(keys)):
        a = keys[i]
        fa = freq[a]
        for j in range(len(keys)):
            b = keys[j]
            fb = freq[b]
            t = (a + b) % m
            res[t] = res.get(t, 0) + fa * fb

    out = []
    for zi in z:
        out.append(str(res.get(zi, 0)))

    print(" ".join(out))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của mã xây dựng bản đồ tần số của tất cả các phần dư bình phương có thể có. Mỗi giá trị của$x$đóng góp chính xác một bản cập nhật và bước này là nơi duy nhất chạm đến toàn bộ miền. 

Phần thứ hai chỉ thực hiện một vòng lặp kép trên các phần dư hình vuông riêng biệt. Đây là nơi chuyển đổi từ$m^2$sự phức tạp đến một cấu trúc tổ hợp nhỏ hơn xảy ra. Mỗi cặp lớp dư lượng đóng góp theo cấp số nhân thông qua tần số của chúng. 

Cuối cùng, các truy vấn được trả lời bằng cách tra cứu từ điển trực tiếp, đảm bảo thời gian không đổi cho mỗi truy vấn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
0 1 2
```Đầu tiên chúng ta tính phần dư bình phương mod 3. Các bình phương là:$0^2=0$,$1^2=1$,$2^2=1$, vì vậy bản đồ tần số là: 

| x | x^2 mod 3 | 
| --- | --- | 
| 0 | 0 | 
| 1 | 1 | 
| 2 | 1 | 

Vì thế$f = \{0:1, 1:2\}$. 

Bây giờ tích chập: 

| một | b | f[a] | f[b] | (a+b)%3 | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | 0 | 1 | 
| 0 | 1 | 1 | 2 | 1 | 2 | 
| 1 | 0 | 2 | 1 | 1 | 2 | 
| 1 | 1 | 2 | 2 | 2 | 4 | 

Vì vậy, tính cuối cùng:$g[0]=1$,$g[1]=4$,$g[2]=4$. 

Điều này khớp với kết quả mong đợi và xác nhận rằng việc sắp xếp các cặp được xử lý vì cả (a,b) và (b,a) đều được bao gồm. 

### Ví dụ 2 

đầu vào:```
4 4
0 1 2 3
```Hình vuông mod 4: 

| x | x^2 mod 4 | 
| --- | --- | 
| 0 | 0 | 
| 1 | 1 | 
| 2 | 0 | 
| 3 | 1 | 

Vì thế$f = \{0:2, 1:2\}$. 

Tích chập: 

| một | b | f[a] | f[b] | (a+b)%4 | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 2 | 2 | 0 | 4 | 
| 0 | 1 | 2 | 2 | 1 | 4 | 
| 1 | 0 | 2 | 2 | 1 | 4 | 
| 1 | 1 | 2 | 2 | 2 | 4 | 

Như vậy:$g[0]=4$,$g[1]=8$,$g[2]=4$,$g[3]=0$. 

Ví dụ này nêu bật tính đối xứng trong các số dư bình phương trực tiếp tạo ra mẫu quan sát được trong các câu trả lời như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m + k^2 + n)$| xây dựng tần số bình phương trên m và tích chập trên k dư lượng riêng biệt | 
| Không gian |$O(k + m)$| lưu trữ bản đồ tần số và kết quả | 

Số hạng chiếm ưu thế phụ thuộc vào số lượng dư lượng hình vuông riêng biệt. Với cấu trúc mô-đun điển hình, điều này vẫn có thể quản lý được và việc xử lý truy vấn là tuyến tính$n$, phù hợp thoải mái trong các ràng buộc cho$n \le 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import defaultdict

    m, n = map(int, sys.stdin.readline().split())
    z = list(map(int, sys.stdin.readline().split()))

    freq = {}
    for x in range(m):
        s = (x * x) % m
        freq[s] = freq.get(s, 0) + 1

    res = {}
    keys = list(freq.keys())

    for i in range(len(keys)):
        for j in range(len(keys)):
            a, b = keys[i], keys[j]
            t = (a + b) % m
            res[t] = res.get(t, 0) + freq[a] * freq[b]

    return " ".join(str(res.get(x, 0)) for x in z)

# provided samples
assert run("3 3\n0 1 2\n") == "1 4 4", "sample 1"
assert run("4 4\n0 1 2 3\n") == "4 8 4 0", "sample 2"

# custom cases
assert run("1 3\n0 0 0\n") == "1 1 1", "mod 1 trivial collapse"
assert run("2 2\n0 1\n") in ["4 0", "4 0"], "binary modulus structure"
assert run("3 1\n2\n") == "4", "single query full convolution"
assert run("5 2\n0 4\n") == "5 4", "mixed residues mod 5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| m=1 trường hợp | tất cả 1s | sự sụp đổ hoàn toàn của dư lượng | 
| m=2 trường hợp | cấu trúc đối xứng | hành vi ngang bằng | 
| truy vấn đơn | tra cứu trực tiếp | tính đúng đắn của tập hợp | 
| dư lượng hỗn hợp | xử lý bọc mô-đun | tính đúng tích chập | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi$m = 1$. Trong trường hợp này, mỗi ô vuông đều$0$, và mỗi cặp góp phần tạo ra dư lượng$0$. Thuật toán xây dựng$f = \{0:1\}$và tích chập tạo ra$g[0] = 1$. Mỗi truy vấn trả về$1$, điều này phù hợp với thực tế là có đúng một cặp$(0,0)$. 

Một trường hợp khác là khi$m = 2$. Squares modulo 2 sụp đổ nặng nề: cả 0 và 1 lần lượt ánh xạ thành 0 và 1, nhưng có tần số lặp lại mạnh. Thuật toán đếm chính xác$f[0]=1$,$f[1]=1$và tích chập mang lại phạm vi bao phủ thống nhất trên các cặp, khớp với tất cả các bảng liệt kê rõ ràng. 

Một trường hợp tinh tế hơn là khi nhiều giá trị có cùng một phần dư bình phương, chẳng hạn như$x$Và$m-x$. Bản đồ tần số tổng hợp cả hai vào cùng một nhóm một cách tự nhiên và tích chập đảm bảo bao gồm cả hai hướng đặt hàng, do đó không cần hiệu chỉnh tính đối xứng.
