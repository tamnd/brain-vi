---
title: "CF 977C - Ít hơn hoặc bằng"
description: "Chúng ta được cung cấp một danh sách các số nguyên và được yêu cầu xây dựng một giá trị $x$ trong khoảng từ 1 đến $10^9$ sao cho chính xác các phần tử $k$ của danh sách nhỏ hơn hoặc bằng $x$. Nếu không có giá trị như vậy tồn tại, chúng ta phải xuất $-1$."
date: "2026-06-17T01:26:45+07:00"
tags: ["codeforces", "competitive-programming", "sortings"]
categories: ["algorithms"]
codeforces_contest: 977
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 479 (Div. 3)"
rating: 1200
weight: 977
solve_time_s: 93
verified: true
draft: false
---

[CF 977C - Ít hơn hoặc bằng](https://codeforces.com/problemset/problem/977/C) 

**Đánh giá:** 1200 
**Thẻ:** sắp xếp 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên và được yêu cầu xây dựng một giá trị$x$giữa 1 và$10^9$chính xác như vậy$k$các phần tử của danh sách nhỏ hơn hoặc bằng$x$. Nếu không có giá trị như vậy tồn tại, chúng ta phải xuất ra$-1$. 

Một cách hữu ích để suy nghĩ về điều này là$x$xác định một ngưỡng trên trục số. BẰNG$x$tăng lên, số phần tử$\le x$chỉ tăng chứ không bao giờ giảm. Chúng tôi đang cố gắng tìm một điểm mà số đếm này trở nên chính xác$k$. 

Kích thước đầu vào cho phép lên đến$2 \cdot 10^5$những con số. Bất kỳ cách tiếp cận nào kiểm tra mọi ứng viên$x$trực tiếp trong phạm vi$[1, 10^9]$là không thể, vì điều đó sẽ đòi hỏi tới$10^9$hoạt động. Ngay cả việc kiểm tra từng phần tử mảng nhiều lần một cách đơn giản cũng sẽ dẫn đến khoảng$O(n^2)$làm việc trong trường hợp xấu nhất, vượt xa thời hạn. 

Trường hợp cạnh khóa xuất hiện khi nhiều giá trị trong mảng xung quanh ranh giới bằng nhau. Nếu chúng ta chọn một$x$bằng một giá trị nào đó trong mảng, số phần tử$\le x$có thể nhảy nhiều hơn một cùng một lúc. Ví dụ, nếu mảng là$[3, 3, 3]$Và$k = 2$, không có$x$mang lại chính xác hai phần tử$\le x$, vì số đếm nhảy từ 0 (đối với$x < 3$) trực tiếp đến 3 (đối với$x \ge 3$). 

Một trường hợp tế nhị khác là$k = 0$. Điều này đòi hỏi một$x$nhỏ hơn tất cả các phần tử Từ$x \ge 1$, điều này chỉ có thể thực hiện được nếu tất cả các giá trị mảng lớn hơn 1 và thậm chí sau đó$x = 1$có thể hoạt động hoặc không tùy thuộc vào việc có phần tử nào bằng 1 hay không. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là thử mọi số nguyên có thể$x$từ 1 đến$10^9$và với mỗi cái hãy đếm xem có bao nhiêu phần tử mảng$\le x$. Điều này đúng vì nó khớp trực tiếp với điều kiện chúng ta muốn, nhưng nó thực hiện$10^9$kiểm tra ứng viên, mỗi chi phí$O(n)$, dẫn đến$O(n \cdot 10^9)$, điều đó hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là hàm “số phần tử$\le x$" chỉ thay đổi ở các giá trị có trong mảng. Giữa hai giá trị mảng riêng biệt liên tiếp, số đếm không đổi. Điều này có nghĩa là chúng ta chỉ cần xem xét các ngưỡng gần các giá trị được sắp xếp của mảng. 

Sau khi sắp xếp mảng, chúng ta có thể tính toán xem độ dài tiền tố bằng$k$. Nếu tiền tố như vậy tồn tại, bất kỳ giá trị nào$x$giữa$k$-giá trị nhỏ nhất và ngay dưới giá trị$(k+1)$-giá trị nhỏ nhất sẽ hoạt động. Một công trình xây dựng sạch sẽ phải đảm bảo$k$-phần tử nhỏ nhất và đảm bảo nó không vô tình bao gồm phần tử$(k+1)$-thứ 1 

Trường hợp thất bại duy nhất là khi$k$-th và$(k+1)$-các phần tử thứ đều bằng nhau, bởi vì khi đó bất kỳ phần tử nào$x$bao gồm một bản bao gồm tất cả các bản sao giống nhau, khiến không thể tách biệt chính xác$k$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot 10^9)$|$O(1)$| Quá chậm | 
| Tối ưu (sắp xếp) |$O(n \log n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng theo thứ tự không giảm. Điều này cho phép chúng tôi suy luận trực tiếp về số lượng tiền tố. 
2. Nếu$k = 0$, chúng tôi muốn một giá trị nhỏ hơn tất cả các phần tử. Chúng tôi kiểm tra phần tử nhỏ nhất. Nếu là 1 thì không hợp lệ$x \ge 1$tồn tại, vì vậy chúng tôi xuất ra$-1$. Nếu không, chúng tôi xuất ra 1, vì nó đảm bảo không có phần tử nào$\le x$. 
3. Nếu$k = n$, chúng tôi muốn tất cả các phần tử là$\le x$. Phần tử lớn nhất luôn hoạt động, vì vậy chúng tôi xuất ra giá trị mảng tối đa. 
4. Ngược lại, hãy xem xét$k$-phần tử nhỏ nhất thứ (chỉ số$k-1$trong lập chỉ mục dựa trên 0). 
5. Kiểm tra$(k+1)$-phần tử nhỏ nhất thứ (chỉ số$k$). Nếu nó bằng$k$-phần tử thứ, sau đó bất kỳ$x$bao gồm$k$-th cũng bao gồm$(k+1)$-th, vì vậy chúng tôi không thể đạt được chính xác$k$. Trong trường hợp này, đầu ra$-1$. 
6. Nếu chúng khác nhau, hãy xuất ra$k$-th phần tử là câu trả lời. Điều này đảm bảo chính xác$k$các yếu tố là$\le x$. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, số phần tử$\le x$là một hàm bước chỉ tăng ở các giá trị mảng khác nhau. Lựa chọn$x$bằng với$k$-giá trị nhỏ nhất đảm bảo ít nhất$k$các phần tử được bao gồm. Nếu giá trị khác biệt tiếp theo lớn hơn hoàn toàn thì không có phần tử bổ sung nào được nhập cho đến sau khoảng cách đó, do đó số đếm vẫn chính xác$k$. Nếu các bản sao thu hẹp khoảng cách, kích thước bước sẽ trở nên quá lớn, khiến cho sự bình đẳng chính xác là không thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))
a.sort()

if k == 0:
    if a[0] == 1:
        print(-1)
    else:
        print(1)
elif k == n:
    print(a[-1])
else:
    x = a[k - 1]
    if a[k] == x:
        print(-1)
    else:
        print(x)
```Bước sắp xếp sắp xếp các giá trị sao cho các vị trí tiền tố tương ứng với số phần tử nhỏ hơn hoặc bằng một ngưỡng. Việc xử lý$k = 0$là riêng biệt bởi vì chúng ta không thể chọn$x = 0$, vì vậy chúng tôi phải đảm bảo giá trị đã chọn nằm trong miền được phép. 

Kiểm tra quan trọng là so sánh$a[k-1]$Và$a[k]$. Nếu chúng bằng nhau thì bất kỳ ngưỡng nào bao gồm$k$-phần tử thứ nhất thiết phải bao gồm nhiều hơn$k$các yếu tố, phá vỡ yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7 4
3 7 5 1 10 3 20
```Mảng được sắp xếp:$[1, 3, 3, 5, 7, 10, 20]$chúng tôi muốn$k = 4$, vì vậy chúng ta xem xét phần tử thứ 4 (chỉ số 3), là 5. 

| Bước | giá trị thứ k | (k+1)-giá trị | Quyết định | 
| --- | --- | --- | --- | 
| Sau khi sắp xếp | 5 | 7 | khoảng cách hợp lệ | 

Vì 5 và 7 khác nhau nên chúng tôi xuất ra 5. Bất kỳ giá trị nào từ 5 đến 6 cũng có tác dụng, vì vậy 5 là hợp lệ. 

Điều này xác nhận rằng thuật toán tìm thấy chính xác ngưỡng trong đó bao gồm chính xác bốn phần tử. 

### Ví dụ 2 

đầu vào:```
5 2
1 2 2 2 3
```Mảng được sắp xếp:$[1, 2, 2, 2, 3]$Ta lấy phần tử thứ 2 (chỉ số 1) là 2. Phần tử thứ 3 cũng là 2. 

| Bước | giá trị thứ k | (k+1)-giá trị | Quyết định | 
| --- | --- | --- | --- | 
| Sau khi sắp xếp | 2 | 2 | không hợp lệ | 

Vì chúng bằng nhau nên bất kỳ$x \ge 2$bao gồm ít nhất 3 phần tử, vì vậy việc đạt được chính xác 2 phần tử là không thể. Đầu ra là$-1$. 

Điều này cho thấy trường hợp lỗi khối trùng lặp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế; tất cả các lần kiểm tra đều có thời gian không đổi | 
| Không gian |$O(1)$| Sắp xếp tại chỗ ngoài mảng đầu vào | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các phần tử, do đó$O(n \log n)$giải pháp thoải mái phù hợp trong thời hạn. Việc sử dụng bộ nhớ là tuyến tính ở kích thước đầu vào, cũng an toàn dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()

    if k == 0:
        if a[0] == 1:
            return "-1"
        return "1"
    elif k == n:
        return str(a[-1])
    else:
        x = a[k - 1]
        if a[k] == x:
            return "-1"
        return str(x)

# provided sample
assert run("7 4\n3 7 5 1 10 3 20") == "5"

# all equal impossible case
assert run("5 2\n2 2 2 2 2") == "-1"

# k = 0 valid
assert run("3 0\n5 6 7") == "1"

# k = n case
assert run("4 4\n1 2 3 4") == "4"

# boundary duplicate-safe
assert run("6 3\n1 2 2 3 4 5") in {"2"}

# minimum size
assert run("1 1\n10") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | -1 | thất bại sụp đổ trùng lặp | 
| k = 0 | 1 | xử lý ngưỡng nhỏ nhất | 
| k = n | tối đa | trường hợp bao gồm đầy đủ | 
| kích thước tối thiểu | phần tử | độ chính xác của phần tử đơn | 

## Vỏ cạnh 

Khi tất cả các phần tử đều bằng nhau và$k$không phải là 0 hoặc n, thuật toán sẽ bác bỏ trường hợp đó một cách chính xác vì kiểm tra sự bằng nhau giữa$a[k-1]$Và$a[k]$thất bại ngay lập tức. Ví dụ, đầu vào$[5, 5, 5]$với$k = 1$dẫn đến$a[0] = 5$Và$a[1] = 5$, do đó không tồn tại ngưỡng hợp lệ. 

Vì$k = 0$, thuật toán đảm bảo chúng ta không bao giờ chọn giá trị dưới 1. Nếu phần tử nhỏ nhất là 3, thì đầu ra 1 mang lại 0 phần tử$\le x$. Nếu phần tử nhỏ nhất là 1 thì không hợp lệ$x$tồn tại trong phạm vi cho phép, vì vậy chúng tôi xuất ra một cách chính xác$-1$. 

Đối với ranh giới$k = n$, việc sắp xếp đảm bảo phần tử cuối cùng là phần tử lớn nhất, vì vậy việc chọn phần tử đó bao gồm tất cả các phần tử mà không cần đếm quá mức, ngay cả với các phần tử trùng lặp.
