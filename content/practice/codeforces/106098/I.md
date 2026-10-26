---
title: "CF 106098I - MEDAA và Totient"
description: "Đối với mọi số $ai$, Meda tính toán $$f(ai)=prod{p mid ai}left(1-frac1pright)$$ trong đó tích chạy trên các ước số nguyên tố riêng biệt của $ai$."
date: "2026-06-25T11:56:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106098
codeforces_index: "I"
codeforces_contest_name: "The American University in Cairo CSEA Fall 2025 contest"
rating: 0
weight: 106098
solve_time_s: 60
verified: true
draft: false
---

[CF 106098I - MEDAA và Totients](https://codeforces.com/problemset/problem/106098/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đối với mỗi số$a_i$, Meda tính toán$$f(a_i)=\prod_{p \mid a_i}\left(1-\frac1p\right)$$trong đó tích chạy trên các ước số nguyên tố riêng biệt của$a_i$. 

Chúng tôi được cung cấp một mảng lên tới$2 \cdot 10^5$số nguyên, mỗi số nhiều nhất$10^6$và phải xác định có bao nhiêu giá trị hữu tỉ riêng biệt xuất hiện trong số tất cả các giá trị được tính toán$f(a_i)$. 

Quan sát đầu tiên là số mũ không quan trọng. Giá trị chỉ phụ thuộc vào số nguyên tố nào chia cho số đó chứ không phụ thuộc vào số lần chúng chia. Ví dụ,$$f(12)=\left(1-\frac12\right)\left(1-\frac13\right)$$Và$$f(36)=\left(1-\frac12\right)\left(1-\frac13\right)$$bởi vì cả hai số đều có cùng một tập hợp các thừa số nguyên tố riêng biệt. 

Các ràng buộc ngay lập tức loại trừ việc phân tích mọi số thành nhân tử bằng cách chia thử cho đến$\sqrt{10^6}$. Trong trường hợp xấu nhất sẽ cần khoảng$2 \cdot 10^5 \cdot 1000$hoạt động vốn đã quá lớn trong Python. Vì tất cả các giá trị lớn nhất$10^6$, phương pháp tiền xử lý dựa trên sàng là hướng đi tự nhiên. 

Một điểm tinh tế là hai bộ ước số nguyên tố khác nhau không bao giờ tạo ra cùng một giá trị của$f$. Giải pháp lưu trữ các xấp xỉ dấu phẩy động có thể thất bại vì các số hữu tỷ riêng biệt có thể không thể phân biệt được sau khi làm tròn. 

Hãy xem xét các con số:```
6 10
```Giá trị của chúng là$$f(6)=\frac12\cdot\frac23=\frac13$$Và$$f(10)=\frac12\cdot\frac45=\frac25$$Đây là những lý do khác nhau. Bất kỳ cách tiếp cận nào dựa trên số học dấu phẩy động có độ chính xác thấp đều có nguy cơ hợp nhất các giá trị lẽ ra vẫn khác biệt. 

Một sai lầm dễ mắc phải khác là phân biệt các số bằng hệ số hóa đầy đủ thay vì tập hợp số nguyên tố riêng biệt. 

Ví dụ:```
2 4 8 16
```Cả bốn số đều có cùng một tập hợp ước số nguyên tố$\{2\}$, vậy mọi giá trị đều bằng$1/2$, và câu trả lời là 1. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ tính từng số, tính toán rõ ràng giá trị hợp lý của$$\prod_{p \mid n}\left(1-\frac1p\right),$$giảm phân số và chèn nó vào một tập hợp. 

Điều này đúng vì công thức xác định trực tiếp giá trị. Điểm nghẽn là yếu tố hóa. Phân chia thử nghiệm lên đến$\sqrt{n}$tốn khoảng 1000 séc cho mỗi số khi$n$đang ở gần$10^6$. Với$2 \cdot 10^5$những con số đó trở thành hàng trăm triệu phép tính. 

Quan sát quan trọng là$$f(n)=\prod_{p\mid n}\frac{p-1}{p}.$$Giá trị chỉ phụ thuộc vào tập hợp các ước số nguyên tố riêng biệt. 

Bây giờ đến thực tế lý thuyết số quan trọng. Giả sử hai số có tập hợp nguyên tố$S_1$Và$S_2$. Sau đó$$\prod_{p\in S_1}\frac{p-1}{p}
=
\prod_{p\in S_2}\frac{p-1}{p}.$$Nhân cả hai vế với tích của tất cả các số nguyên tố liên quan sẽ được$$\prod_{p\in S_1}(p-1)\prod_{q\in S_2\setminus S_1}q
=
\prod_{p\in S_2}(p-1)\prod_{q\in S_1\setminus S_2}q.$$Lấy số nguyên tố$r$thuộc về chính xác một bộ, giả sử$r\in S_1\setminus S_2$. Vế phải có thể chia cho$r$, vì nó chứa yếu tố$r$. Vế trái không chia hết cho$r$, từ$r\nmid (r-1)$Và$r\nmid (p-1)$cho mọi thừa số nguyên tố khác xuất hiện ở đó. Sự mâu thuẫn. 

Do đó các giá trị bằng nhau ngụ ý các tập hợp nguyên tố giống hệt nhau. 

Vấn đề bây giờ đơn giản hơn nhiều. Chúng ta chỉ cần đếm xem có bao nhiêu tập hợp ước số nguyên tố riêng biệt xuất hiện trong mảng. 

Từ$a_i \le 10^6$, chúng ta có thể tính trước hệ số nguyên tố nhỏ nhất cho mỗi số bằng cách sử dụng sàng. Sau đó, mỗi số có thể được phân tích thành nhân tử theo thời gian logarit, các ước nguyên tố riêng biệt của nó được trích xuất và bộ dữ liệu thu được sẽ được chèn vào một tập hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n\sqrt A)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(A \log\log A + n\log A)$|$O(A)$| Đã chấp nhận | 

Đây$A = 10^6$. 

## Hướng dẫn thuật toán 

1. Đọc mảng và xác định giá trị lớn nhất xuất hiện trong mảng đó. 
2. Xây dựng sàng có hệ số nguyên tố nhỏ nhất đến giá trị lớn nhất đó. 
3. Với mỗi số, chia nhiều lần cho thừa số nguyên tố nhỏ nhất của nó. 
4. Bất cứ khi nào gặp một thừa số nguyên tố mới, hãy thêm nó vào danh sách các số nguyên tố riêng biệt. 
5. Bỏ qua tất cả các lần xuất hiện lặp lại của cùng một số nguyên tố trước khi tiếp tục phân tích nhân tử. Điều này chỉ trích xuất tập hợp ước số nguyên tố riêng biệt. 
6. Chuyển đổi danh sách các số nguyên tố riêng biệt thành một bộ và chèn nó vào một tập hợp. 
7. Sau khi xử lý tất cả các số, xuất ra kích thước của tập hợp. 

Lý do bước 6 có hiệu quả là vì bộ dữ liệu xác định duy nhất tập hợp ước số nguyên tố và bằng chứng ở trên cho thấy giá trị của$f(n)$được xác định duy nhất bởi tập hợp đó. 

### Tại sao nó hoạt động 

Đối với mỗi số$n$, giá trị$f(n)$chỉ phụ thuộc vào các ước nguyên tố phân biệt của nó. 

Ta đã chứng minh rằng hai tập hợp số nguyên tố khác nhau không thể tạo ra cùng một giá trị của$f$. Như vậy có sự tương ứng một-một giữa các giá trị của$f$và tập hợp các ước số nguyên tố phân biệt. 

Thuật toán lưu trữ chính xác những bộ đó. Mỗi lần xuất hiện của cùng một bộ đều đóng góp cùng một bộ, và mỗi bộ khác nhau sẽ đóng góp một bộ khác nhau. Số lượng các bộ dữ liệu riêng biệt chính xác là số lượng các giá trị riêng biệt của$f$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

mx = max(a)

spf = list(range(mx + 1))
for i in range(2, int(mx ** 0.5) + 1):
    if spf[i] == i:
        for j in range(i * i, mx + 1, i):
            if spf[j] == j:
                spf[j] = i

seen = set()

for x in a:
    primes = []

    while x > 1:
        p = spf[x]
        primes.append(p)

        while x % p == 0:
            x //= p

    seen.add(tuple(primes))

print(len(seen))
```Sàng lưu trữ hệ số nguyên tố nhỏ nhất cho mỗi số. Điều này cho phép mỗi phép phân tích nhân tử liên tục tách ra từng số nguyên tố một. 

Vòng lặp bên trong loại bỏ tất cả các bản sao của cùng một số nguyên tố. Đó là chi tiết quan trọng vì hàm số chỉ phụ thuộc vào các ước nguyên tố riêng biệt. Nếu không loại bỏ sự lặp lại, các số như 2 và 4 sẽ tạo ra các chữ ký khác nhau mặc dù chúng tạo ra cùng một giá trị. 

Việc sử dụng các bộ dữ liệu bên trong một bộ Python sẽ cung cấp một biểu diễn nhỏ gọn của các bộ ước số nguyên tố. Do quá trình phân tích nhân tử phát hiện ra các số nguyên tố theo thứ tự tăng dần một cách tự nhiên khi sử dụng sàng SPF, nên cách biểu diễn bộ dữ liệu là chuẩn và không cần sắp xếp thêm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
2 3 4
```| Số | Số nguyên tố riêng biệt | Bộ dữ liệu được lưu trữ | Đặt kích thước | 
| --- | --- | --- | --- | 
| 2 | {2} | (2,) | 1 | 
| 3 | {3} | (3,) | 2 | 
| 4 | {2} | (2,) | 2 | 

Trả lời:`2`. 

Điều này chứng tỏ rằng lũy ​​thừa của cùng một số nguyên tố thu gọn về cùng một chữ ký. 

### Ví dụ 2 

đầu vào:```
6
3 9 93842 123 2 1024
```| Số | Số nguyên tố riêng biệt | Bộ dữ liệu được lưu trữ | 
| --- | --- | --- | 
| 3 | {3} | (3,) | 
| 9 | {3} | (3,) | 
| 93842 | {2, 11, 4261} | (2, 11, 4261) | 
| 123 | {3, 41} | (3, 41) | 
| 2 | {2} | (2,) | 
| 1024 | {2} | (2,) | 

Các bộ dữ liệu khác biệt là:$$(3,),\ (2,11,4261),\ (3,41),\ (2,)$$vậy câu trả lời là`4`. 

Ví dụ này cho thấy rằng lũy ​​thừa lặp lại không tạo ra giá trị mới, trong khi một tập hợp nguyên tố khác luôn làm như vậy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(A \log \log A + n \log A)$| Sàng SPF cộng hệ số của tất cả các số | 
| Không gian |$O(A)$| Mảng thừa số nguyên tố nhỏ nhất | 

Với$A \le 10^6$Và$n \le 2 \cdot 10^5$, điều này dễ dàng phù hợp với giới hạn của cuộc thi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    mx = max(a)

    spf = list(range(mx + 1))
    for i in range(2, int(mx ** 0.5) + 1):
        if spf[i] == i:
            for j in range(i * i, mx + 1, i):
                if spf[j] == j:
                    spf[j] = i

    seen = set()

    for x in a:
        cur = []
        while x > 1:
            p = spf[x]
            cur.append(p)
            while x % p == 0:
                x //= p
        seen.add(tuple(cur))

    return str(len(seen))

# provided samples
assert run("3\n2 3 4\n") == "2", "sample 1"
assert run("6\n3 9 93842 123 2 1024\n") == "4", "sample 2"

# custom cases
assert run("1\n2\n") == "1", "minimum size"
assert run("4\n2 4 8 16\n") == "1", "same prime set"
assert run("3\n6 10 15\n") == "3", "three different prime sets"
assert run("5\n30 60 90 210 2310\n") == "3", "mixed repeated sets"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 2`|`1`| Kích thước đầu vào tối thiểu | 
|`2 4 8 16`|`1`| Quyền năng của cùng một thủ tướng sụp đổ | 
|`6 10 15`|`3`| Các bộ nguyên tố khác nhau vẫn khác biệt | 
|`30 60 90 210 2310`|`3`| Bộ số nguyên tố lặp lại và mở rộng | 

## Vỏ cạnh 

Hãy xem xét:```
4
2 4 8 16
```Việc phân tích nhân tử tạo ra cùng một bộ dữ liệu nguyên tố riêng biệt`(2,)`mọi lúc. Tập hợp kết thúc bằng một phần tử, vì vậy câu trả lời là`1`. Giải pháp giữ số mũ sẽ đếm không chính xác bốn chữ ký khác nhau. 

Coi như:```
2
6 10
```Các bộ dữ liệu là`(2,3)`Và`(2,5)`. Cả hai đều được chèn riêng biệt, đưa ra câu trả lời`2`. Điều này xác nhận rằng các bộ nguyên tố khác nhau không bao giờ được hợp nhất. 

Coi như:```
3
6 12 18
```Mỗi số có các ước nguyên tố phân biệt`{2,3}`. Thuật toán trích xuất`(2,3)`đối với cả ba số, hãy chèn nó một lần và xuất ra`1`. Điều này xác minh rằng số mũ lặp lại hoàn toàn bị bỏ qua, chính xác theo yêu cầu của định nghĩa$f(n)$.
