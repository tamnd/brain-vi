---
title: "CF 1080B - Margarite và món quà tuyệt vời nhất"
description: "Chúng ta được cung cấp một chuỗi vô hạn rất lớn trong đó mỗi vị trí có một giá trị xác định chỉ dựa trên chỉ số của nó. Giá trị đổi dấu và tăng độ lớn một cách tuyến tính: vị trí 1 đóng góp −1, vị trí 2 đóng góp +2, vị trí 3 đóng góp −3, v.v."
date: "2026-06-15T06:23:45+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1080
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 524 (Div. 2)"
rating: 900
weight: 1080
solve_time_s: 158
verified: true
draft: false
---

[CF 1080B - Margarite và món quà tuyệt vời nhất](https://codeforces.com/problemset/problem/1080/B) 

**Xếp hạng:** 900 
**Thẻ:** toán 
**Thời gian giải:** 2 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi vô hạn rất lớn trong đó mỗi vị trí có một giá trị xác định chỉ dựa trên chỉ số của nó. Giá trị đổi dấu và tăng độ lớn một cách tuyến tính: vị trí 1 đóng góp −1, vị trí 2 đóng góp +2, vị trí 3 đóng góp −3, v.v. Nói chung, phần tử ở vị trí$i$là$i$nhân với một dấu hiệu lật mỗi bước. 

Mỗi truy vấn yêu cầu tổng giá trị trong một phân đoạn liền kề$[l, r]$. Vì có thể có tới một nghìn truy vấn và chỉ mục lên tới một tỷ, nên chúng tôi không thể xây dựng mảng hoặc lặp trực tiếp qua các phạm vi. Bất kỳ giải pháp nào chạm vào mọi phần tử trong phạm vi truy vấn sẽ giảm xuống$10^9$hoạt động trong trường hợp xấu nhất, không thể thực hiện được trong thời hạn. 

Thách thức chính là mặc dù chuỗi lớn nhưng cấu trúc của nó cực kỳ đều đặn, điều này cho thấy tồn tại một tổng tiền tố dạng đóng. 

Trường hợp cạnh tinh vi xuất hiện khi phạm vi chỉ trải dài ở các vị trí lẻ hoặc chỉ chẵn. Ví dụ, hãy xem xét$[1, 1]$,$[2, 2]$, hoặc$[1, 2]$. Việc triển khai đơn giản có thể xử lý sai sự thay đổi dấu hiệu nếu nó dựa vào mô phỏng gia tăng mà không lập chỉ mục cẩn thận. Một dạng lỗi khác là lỗi từng lỗi một trong mẫu dấu hiệu: liệu$i$chẵn là dương hoặc âm phải duy trì nhất quán trên cả tính toán tiền tố và phạm vi. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force sẽ tính toán từng truy vấn một cách độc lập bằng cách lặp từ$l$ĐẾN$r$và tổng hợp$a_i$. Điều này đúng vì nó tuân theo định nghĩa trực tiếp. Tuy nhiên, mỗi truy vấn có thể kéo dài tới$10^9$các phần tử, do đó, ngay cả một truy vấn trong trường hợp xấu nhất cũng đã quá lớn và có tối đa$10^3$truy vấn cách tiếp cận này là hoàn toàn không khả thi. 

Cấu trúc của trình tự là quan sát chính. Các giá trị thay thế dấu trong khi tăng tuyến tính về độ lớn. Điều này gợi ý việc chia chuỗi thành các vị trí chẵn và lẻ, trong đó mỗi chuỗi con trở thành một cấp số cộng đơn giản với dấu nhất quán. 

Vị trí lẻ đóng góp giá trị âm: tại vị trí$2k-1$, giá trị là$-(2k-1)$. Các vị trí chẵn đóng góp giá trị tích cực: tại vị trí$2k$, giá trị là$+2k$. Mỗi dãy con này có một công thức đã biết cho tổng tiền tố của cấp số cộng, cho phép chúng ta tính tổng lên tới$n$trong thời gian không đổi. Khi đã có tổng tiền tố, mỗi truy vấn sẽ giảm xuống thành phép trừ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(r − l + 1) mỗi truy vấn | O(1) | Quá chậm | 
| Công thức tiền tố tối ưu | O(1) mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Viết lại chuỗi bằng cách tách các đóng góp khỏi các chỉ số lẻ và chẵn. Điều này quan trọng vì mỗi phần trở thành một cấp số cộng tiêu chuẩn thay vì một mẫu xen kẽ. 
2. Suy ra tổng tiền tố thành chỉ số$n$. Chúng tôi tính toán có bao nhiêu số hạng chẵn và lẻ tồn tại tối đa$n$, vì số lượng của chúng hơi khác nhau tùy thuộc vào tính chẵn lẻ. 
3. Tính tổng các chỉ số chẵn. Ngay cả các vị trí cũng$2, 4, 6, \dots, 2k$. Tổng của chúng là:$$2(1 + 2 + \dots + k) = k(k+1)$$Ở đâu$k = \lfloor n/2 \rfloor$. 
4. Tính tổng các chỉ số lẻ. Các vị trí lẻ là$1, 3, 5, \dots, (2k-1)$. Đóng góp của họ là tiêu cực:$$-(1 + 3 + 5 + \dots + (2k-1)) = -k^2$$Ở đâu$k = \lceil n/2 \rceil$. 
5. Kết hợp cả hai phần để có được tổng tiền tố$S(n)$. Sau đó mỗi truy vấn$[l, r]$được trả lời là:$$S(r) - S(l-1)$$Lý do điều này hoạt động là vì chuỗi phân tách thành hai chuỗi số học độc lập với các chỉ số rời rạc. Tổng tiền tố tôn trọng tính tuyến tính, vì vậy chúng ta có thể tính tổng từng chuỗi con riêng biệt và kết hợp lại chúng mà không cần tương tác. 

### Tại sao nó hoạt động 

Bất biến chính là mọi chỉ số đều thuộc về chính xác một trong hai tập hợp rời nhau: chẵn hoặc lẻ. Trên mỗi tập hợp, hàm giá trị trở thành một cấp số cộng thuần túy với bước cố định và dấu cố định. Vì tổng tiền tố là tuyến tính trên các tập hợp rời rạc nên tổng tiền tố chính xác là tổng của hai tổng tiền tố độc lập. Điều này đảm bảo rằng việc trừ các giá trị tiền tố sẽ tách biệt chính xác mọi phân đoạn$[l, r]$không có lỗi tính toán lại hoặc chồng chéo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def pref(n):
    if n <= 0:
        return 0

    k_even = n // 2
    even_sum = k_even * (k_even + 1)

    k_odd = (n + 1) // 2
    odd_sum = k_odd * k_odd

    return even_sum - odd_sum

q = int(input())
for _ in range(q):
    l, r = map(int, input().split())
    print(pref(r) - pref(l - 1))
```chức năng`pref(n)`tính tổng của chuỗi từ chỉ số 1 đến n bằng cách chia thành các phần đóng góp chẵn và lẻ. Đối với các chỉ số chẵn, chúng tôi sử dụng công thức tính tổng của chỉ số đầu tiên$k$số chẵn. Đối với các chỉ số lẻ, chúng tôi tính tổng của chỉ số đầu tiên$k$số lẻ và phủ định nó. 

Mỗi truy vấn sử dụng thủ thuật trừ tiền tố tiêu chuẩn, an toàn vì hàm tiền tố được xác định rõ ràng cho$n = 0$thông qua trường hợp cơ sở. 

Một lỗi phổ biến ở đây là nhầm lẫn số lượng vị trí chẵn và lẻ. Một điều nữa là quên rằng các vị trí lẻ đóng góp các giá trị âm, vì vậy dấu phải được áp dụng sau phép tính tổng chứ không phải trước đó. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu```
1 3
2 5
```Chúng tôi tính toán các giá trị tiền tố từng bước. 

| n | k_even | chẵn_sum | k_lẻ | lẻ_sum | trước(n) | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 1 | 1 | -1 | 
| 3 | 1 | 2 | 2 | 4 | -2 | 
| 5 | 2 | 6 | 3 | 9 | -3 | 

Bây giờ chúng tôi tính toán các truy vấn. 

### Truy vấn 1: [1, 3] 

| bước | giá trị | 
| --- | --- | 
| trước(3) | -2 | 
| trước(0) | 0 | 
| kết quả | -2 | 

Điều này xác nhận rằng phép trừ tiền tố sẽ cô lập phân đoạn một cách chính xác. 

### Truy vấn 2: [2, 5] 

| bước | giá trị | 
| --- | --- | 
| trước(5) | -3 | 
| trước(1) | -1 | 
| kết quả | -2 | 

Điều này cho thấy rằng ngay cả khi phân đoạn bắt đầu ở giữa mẫu, việc phân tách tiền tố vẫn hoạt động vì nó nắm bắt được đầy đủ các đóng góp về cấu trúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q) | Mỗi truy vấn sử dụng các công thức số học có thời gian không đổi | 
| Không gian | O(1) | Chỉ một số số nguyên được lưu trữ bất kể kích thước đầu vào | 

Các ràng buộc cho phép lên đến$10^3$truy vấn có giá trị lên tới$10^9$và đánh giá theo thời gian không đổi cho mỗi truy vấn phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def pref(n):
        if n <= 0:
            return 0
        k_even = n // 2
        even_sum = k_even * (k_even + 1)
        k_odd = (n + 1) // 2
        odd_sum = k_odd * k_odd
        return even_sum - odd_sum

    q = int(input())
    out = []
    for _ in range(q):
        l, r = map(int, input().split())
        out.append(str(pref(r) - pref(l - 1)))
    return "\n".join(out)

# provided samples
assert run("""5
1 3
2 5
5 5
4 4
2 3
""") == """-2
-2
-5
4
-1"""

# minimum range
assert run("""1
1 1
""") == "-1"

# single even index
assert run("""1
2 2
""") == "2"

# large symmetric range
assert run("""1
1 10
""") == str(sum(i if i % 2 == 0 else -i for i in range(1, 11)))

# mixed ranges
assert run("""2
1 2
3 6
""") == """1
-2"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | -1 | độ chính xác chỉ số nhỏ nhất | 
| 2 2 | 2 | xử lý dấu vị trí chẵn | 
| 1 10 | tổng tính toán | thỏa thuận với định nghĩa | 
| phạm vi hỗn hợp | - | tính chính xác của tiền tố chung | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi phạm vi bắt đầu từ 1, vì logic tiền tố phụ thuộc vào việc xử lý$l - 1 = 0$. Ví dụ, trong$[1, 1]$, việc tính toán trở thành$S(1) - S(0)$. Việc triển khai trả về 0 một cách rõ ràng cho$S(0)$, đảm bảo tính chính xác mà không cần truy vấn cách viết hoa đặc biệt. 

Một trường hợp cạnh khác là khi$l = r$. Thuật toán giảm xuống còn một lần tra cứu giá trị thông qua phép trừ các tiền tố liền kề và các công thức số học vẫn hoạt động chính xác vì cả đóng góp chẵn và lẻ đều liên tục qua các ranh giới. 

Trường hợp thứ ba là khi$l$Và$r$có tính chẵn lẻ khác nhau, chẳng hạn như$[2, 3]$. Ở đây, đoạn này chuyển từ số hạng chẵn dương sang số hạng lẻ âm. Hàm tiền tố không quan tâm đến ranh giới này vì cả hai đóng góp đều đã được mã hóa toàn cầu trong$S(n)$.
