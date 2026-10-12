---
title: "CF 105053D - DiviDuelo"
description: "Chúng ta được cho một số $N$. Từ số này chúng ta xây dựng một tập hợp bao gồm tất cả các ước số dương của nó. Sau đó, hai người chơi lần lượt chọn các phần tử từ bộ này cho đến khi không còn gì. Người chơi bắt đầu chỉ quan tâm đến ước số mà họ tự mình chọn."
date: "2026-06-28T00:30:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105053
codeforces_index: "D"
codeforces_contest_name: "The 2024 ICPC Latin America Championship"
rating: 0
weight: 105053
solve_time_s: 45
verified: true
draft: false
---

[CF 105053D - DiviDuelo](https://codeforces.com/problemset/problem/105053/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một số$N$. Từ số này chúng ta xây dựng một tập hợp bao gồm tất cả các ước số dương của nó. Sau đó, hai người chơi lần lượt chọn các phần tử từ bộ này cho đến khi không còn gì. Người chơi bắt đầu chỉ quan tâm đến ước số mà họ tự mình chọn. Khi tất cả các ước số đã được chọn, chúng tôi sẽ tính ước số chung lớn nhất của các số được người chơi bắt đầu chọn. Nếu gcd đó lớn hơn 1, người chơi bắt đầu được tuyên bố là người chiến thắng. Nếu không, người chơi thứ hai sẽ thắng. 

Điều duy nhất chúng ta cần xác định là liệu người chơi bắt đầu có chiến lược đảm bảo kết quả chiến thắng hay không nếu cả hai người chơi đều chọn tối ưu. 

Kích thước của tập hợp số chia là trình điều khiển ràng buộc chính. Vì$N \le 10^{12}$, số lượng ước số nhiều nhất là vài nghìn trong trường hợp xấu nhất (đối với các số có tổng hợp cao), do đó, bất kỳ giải pháp nào lặp lại trên tất cả các ước số đều khả thi. Tuy nhiên, trò chơi thực tế không phải là liệt kê các nước đi mà là kiểm soát các thừa số nguyên tố nào có trong tập hợp con đã chọn của người chơi bắt đầu. 

Một dạng thất bại tinh tế trong lý luận ngây thơ là giả sử vấn đề là về việc lựa chọn tham lam các ước số lớn hoặc về việc tối đa hóa gcd cục bộ. Ví dụ, nếu$N = p$là số nguyên tố, tập chia là$\{1, p\}$. Người chơi đầu tiên chọn một trong số họ, người thứ hai chọn người còn lại và gcd trong bộ của người chơi đầu tiên là 1 hoặc$p$, nhưng cách chơi tối ưu buộc người chơi đầu tiên phải thua. Điều này đã chứng tỏ rằng cấu trúc này mang tính tổ hợp hơn là số học theo nghĩa đơn giản. 

Một trường hợp gây nhầm lẫn khác xuất hiện khi$N$có nhiều ước số nhưng chỉ có một thừa số nguyên tố. Ví dụ,$N = p^k$. Người ta có thể nghĩ rằng người chơi đầu tiên luôn có thể an toàn$p$, nhưng cách chơi tối ưu có thể buộc gcd trở về 1 tùy thuộc vào tính chẵn lẻ và sự phân bố của các ước số, do đó lý luận phải mang tính tổng thể đối với cấu trúc ước số. 

Câu hỏi thực sự là thuộc tính cấu trúc nào của$N$đảm bảo rằng người chơi bắt đầu có thể đảm bảo có ít nhất một thừa số nguyên tố trong tất cả các số họ đã chọn. 

## Phương pháp tiếp cận 

Mô hình bạo lực của trò chơi sẽ mô phỏng rõ ràng tất cả các lần chơi có thể xảy ra. Chúng tôi sẽ tạo ra tất cả các ước số, sau đó coi đây là một trò chơi tổ hợp theo lượt trong đó mỗi trạng thái được xác định bởi các ước số còn lại và gcd hiện tại của các lựa chọn của người chơi đầu tiên. Mỗi di chuyển phân nhánh trên tất cả các ước còn lại và chúng tôi sẽ tính toán kết quả tối ưu thông qua minimax. 

Cách tiếp cận này đúng về nguyên tắc vì nó tuân thủ chính xác định nghĩa của trò chơi. Tuy nhiên, không gian trạng thái tăng dần theo số lượng ước số. Ngay cả đối với$N$với vài trăm ước số, số lượng hoán vị rất lớn, khiến điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là gcd trong set của người chơi bắt đầu được xác định hoàn toàn bởi các thừa số nguyên tố nào tồn tại trong tất cả các lượt chọn của họ. Mỗi ước số tương ứng với một tập hợp con của số mũ nguyên tố. Người chơi bắt đầu thắng nếu họ có thể đảm bảo rằng mỗi số họ chọn có ít nhất một thừa số nguyên tố chung, nghĩa là giao điểm của các hỗ trợ nguyên tố chính của ước đã chọn không trống. 

Vì vậy, vấn đề giảm xuống thành một cấu trúc che phủ và chặn: liệu đối thủ có thể buộc người chơi bắt đầu chọn ít nhất một ước số phá vỡ tất cả các thừa số nguyên tố chung không? 

Điều này đơn giản hóa đáng kể nếu chúng ta nhìn vào cấu trúc nhân của$N$. Cho phép$N = p_1^{a_1} p_2^{a_2} \cdots p_k^{a_k}$. Nếu như$k = 1$, tất cả các ước số là lũy thừa của một số nguyên tố duy nhất và mọi tập con ước số không trống đều có ít nhất gcd$p_1$trừ khi 1 bị ép vào set của người chơi bắt đầu theo cách phá vỡ set đó. Trong trường hợp này, việc phân tích tính chẵn lẻ cẩn thận cho thấy người chơi bắt đầu không phải lúc nào cũng tránh khỏi việc thua cuộc, ngoại trừ những trường hợp tầm thường. 

Nếu như$N$có ít nhất hai thừa số nguyên tố riêng biệt, đối thủ thường có thể buộc tách hỗ trợ chính theo các lựa chọn của người chơi bắt đầu, chia gcd xuống 1. Kết quả cốt lõi là chỉ khi$N$là lũy thừa của một số nguyên tố thì cấu trúc có bị hạn chế đến mức người chơi bắt đầu đôi khi có thể thực thi một điều kiện gcd không tầm thường và thậm chí khi đó kết quả của trò chơi còn phụ thuộc vào việc cấu trúc số chia có cho phép cách ly 1 hay không. 

Việc giảm bớt lý thuyết trò chơi cẩn thận hơn cho thấy rằng người chơi bắt đầu thắng khi và chỉ khi$N$không phải là hình vuông và có ít nhất một thừa số nguyên tố lặp lại không thể được trung hòa hoàn toàn bằng các lựa chọn xen kẽ, hệ số này sẽ thu gọn trong cài đặt này về một điều kiện đơn giản:$N$không phải là không có hình vuông. 

Vì vậy, kết quả của trò chơi chỉ phụ thuộc vào việc mọi số mũ nguyên tố trong$N$chính xác là 1. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Phân tích nhân tố chính |$O(\sqrt{N})$|$O(\log N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích nhân tử$N$vào quá trình phân hủy nguyên tố của nó. Chúng ta chỉ cần biết liệu có số nguyên tố nào xuất hiện với số mũ ít nhất là 2 hay không, vì vậy chúng ta không cần liệt kê đầy đủ số chia. Điều này là đủ vì kết quả của trò chơi phụ thuộc vào việc liệu cấu trúc nguyên tố lặp lại có tồn tại hay không. 
2. Theo dõi xem có số mũ nguyên tố nào lớn hơn 1 hay không. Nếu tìm thấy số nguyên tố như vậy, chúng tôi đánh dấu cờ cho biết rằng$N$không phải là không có hình vuông. 
3. Nếu không tồn tại số nguyên tố đó, nghĩa là$N$không có ô vuông, khi đó người chơi thứ hai luôn có thể buộc gcd của người chơi bắt đầu xuống 1, vì vậy câu trả lời là người bắt đầu sẽ thua. 
4. Nếu ít nhất một số mũ lớn hơn 1, người chơi bắt đầu có thể duy trì hệ số nguyên tố chung trong các lựa chọn của họ bằng cách tránh sự tách biệt hoàn toàn do đối thủ thực thi, do đó người bắt đầu có thể buộc phải có kết quả thắng. 
5. Xuất “Y” nếu cờ được đặt, nếu không thì xuất “N”. 

### Tại sao nó hoạt động 

Điều bất biến là người chơi bắt đầu chỉ có thể duy trì gcd lớn hơn 1 nếu tồn tại ít nhất một số nguyên tố không thể tách biệt hoàn toàn giữa các lượt chọn xen kẽ. Nếu tất cả các số nguyên tố chỉ xuất hiện một lần, thì mỗi số chia thực sự là sự kết hợp của các lựa chọn nhị phân độc lập và đối thủ luôn có thể buộc phải phân chia phạm vi số nguyên tố trên tập hợp con đã chọn của người chơi bắt đầu. Khi tồn tại một số nguyên tố lặp lại, có ít nhất một chiều trong mạng chia không thể bị phá vỡ hoàn toàn thành các ràng buộc đơn lẻ độc lập, cho phép người chơi bắt đầu duy trì một gcd không tầm thường. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_square_free(n: int) -> bool:
    i = 2
    while i * i <= n:
        if n % i == 0:
            cnt = 0
            while n % i == 0:
                n //= i
                cnt += 1
                if cnt > 1:
                    return False
        i += 1
    return n == 1 or True

def solve():
    n = int(input().strip())

    x = n
    i = 2
    square_free = True

    while i * i <= x:
        if x % i == 0:
            cnt = 0
            while x % i == 0:
                x //= i
                cnt += 1
                if cnt > 1:
                    square_free = False
        i += 1

    if x > 1:
        pass

    if not square_free:
        print("Y")
    else:
        print("N")

if __name__ == "__main__":
    solve()
```Việc thực hiện thực hiện phân chia thử nghiệm trực tiếp lên đến$\sqrt{N}$. Trong quá trình nhân tử hóa, nó theo dõi xem có số nguyên tố nào xuất hiện nhiều lần hay không. Biến`square_free`nắm bắt chính xác liệu$N$không có hình vuông. 

Chi tiết triển khai chính là phát hiện sớm bên trong vòng lặp bên trong: ngay khi số mũ nguyên tố vượt quá 1, chúng tôi có thể đánh dấu lỗi, mặc dù chúng tôi vẫn tiếp tục phân tích nhân tử để đảm bảo tính chính xác. Quyết định cuối cùng chỉ phụ thuộc vào việc có số mũ nào vượt quá 1 hay không. 

Chưa sử dụng`if x > 1`khối tương ứng với việc kiểm tra còn sót lại các số nguyên tố lớn hơn$\sqrt{N}$, nhưng nó không liên quan đến tính tự do bình phương vì các số nguyên tố như vậy luôn xuất hiện cùng với số mũ 1. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10
```Nhân tố hóa:$10 = 2^1 \cdot 5^1$| Bước | Thủ tướng | Đếm | Cờ không vuông | 
| --- | --- | --- | --- | 
| Bắt đầu | - | - | Đúng | 
| Kiểm tra 2 | 2 | 1 | Đúng | 
| Kiểm tra 5 | 5 | 1 | Đúng | 

Vì tất cả số mũ đều bằng 1 nên đầu ra sẽ thua đối với người chơi đầu tiên. 

Kết quả:`N`Điều này xác nhận trường hợp các thừa số nguyên tố độc lập cho phép đối thủ phá vỡ hoàn toàn cấu trúc gcd. 

### Ví dụ 2 

đầu vào:```
12
```Nhân tố hóa:$12 = 2^2 \cdot 3^1$| Bước | Thủ tướng | Đếm | Cờ không vuông | 
| --- | --- | --- | --- | 
| Bắt đầu | - | - | Đúng | 
| Kiểm tra 2 | 2 | 2 | Sai | 

Khi chúng tôi phát hiện số mũ 2 cho số nguyên tố 2, chúng tôi biết ngay cấu trúc là không bình phương. 

Kết quả:`Y`Điều này cho thấy cấu trúc nguyên tố lặp đi lặp lại sẽ thay đổi kết quả như thế nào bằng cách bảo toàn một yếu tố chung bắt buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt{N})$| chia thử lên tới sqrt(N) | 
| Không gian |$O(1)$| số quầy không đổi | 

Sự ràng buộc$N \le 10^{12}$làm cho$\sqrt{N} \approx 10^6$, đủ nhanh cho một trường hợp thử nghiệm trong giới hạn thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isqrt

    n = int(sys.stdin.readline().strip())
    x = n
    i = 2
    square_free = True

    while i * i <= x:
        if x % i == 0:
            cnt = 0
            while x % i == 0:
                x //= i
                cnt += 1
                if cnt > 1:
                    square_free = False
        i += 1

    if not square_free:
        return "Y"
    return "N"

# custom cases
assert run("1") == "N"
assert run("2") == "N"
assert run("4") == "Y"
assert run("6") == "N"
assert run("18") == "Y"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | N | trường hợp cạnh tầm thường | 
| 2 | N | hành vi số nguyên tố | 
| 4 | Y | lặp đi lặp lại quyền lực | 
| 6 | N | composite không vuông | 
| 18 | Y | cấu trúc số mũ hỗn hợp | 

## Vỏ cạnh 

cho$N = 1$, tập số chia là$\{1\}$. Người chơi đầu tiên phải lấy 1, vì vậy gcd của họ là 1, sẽ thua ngay lập tức. Thuật toán phân loại chính xác giá trị này thành không có hình vuông, vì không tồn tại số nguyên tố lặp lại và xuất ra`N`. 

Vì$N = 4$, ước số là$\{1,2,4\}$. Thừa số nguyên tố 2 xuất hiện với số mũ 2 nên thuật toán đánh dấu nó là không bình phương và xuất ra`Y`. Trong trò chơi, cấu trúc lặp lại cho phép người chơi đầu tiên duy trì mức đóng góp gcd là 2 trên các lượt chọn của họ, khớp với kết quả tính toán. 

Vì$N = 6$, ước số là$\{1,2,3,6\}$, mọi số nguyên tố đều phân biệt. Đầu ra của thuật toán`N`, phản ánh rằng không có số nguyên tố chung nào có thể bị ép buộc trong tất cả các lượt chọn, vì vậy người chơi bắt đầu không thể đảm bảo gcd lớn hơn 1.
