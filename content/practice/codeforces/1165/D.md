---
title: "CF 1165D - Hầu hết tất cả các ước số"
description: "Chúng ta được cấp một tập hợp nhiều tập được cho là đại diện cho hầu hết tất cả các ước số của một số nguyên $x$ chưa biết. “Hầu hết tất cả” ở đây có ý nghĩa chính xác: danh sách chứa mọi ước số của $x$ ngoại trừ chính $1$ và $x$."
date: "2026-06-13T08:47:49+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1165
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 560 (Div. 3)"
rating: 1600
weight: 1165
solve_time_s: 389
verified: true
draft: false
---

[CF 1165D - Hầu hết tất cả các ước số](https://codeforces.com/problemset/problem/1165/D) 

**Đánh giá:** 1600 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 6 phút 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp đa số được cho là đại diện cho hầu hết tất cả các ước số của một số nguyên chưa biết$x$. “Hầu hết tất cả” ở đây có ý nghĩa chính xác: danh sách chứa mọi ước số của$x$ngoại trừ$1$Và$x$chính nó. Nhiệm vụ của chúng tôi là tái cấu trúc nhỏ nhất có thể$x$có thể tạo chính xác danh sách ước số này hoặc xác định rằng không tồn tại số nguyên như vậy. 

Khó khăn chính là chúng ta không được cung cấp$x$trực tiếp, thậm chí cả tập hợp ước số đầy đủ. Chúng tôi chỉ nhìn thấy các ước số bên trong, phải được đóng lại khi ghép nối: nếu$d \mid x$, sau đó$\frac{x}{d} \mid x$và cả hai sẽ xuất hiện trong danh sách trừ khi chúng là 1 hoặc$x$. 

Các hạn chế là nhỏ:$n \le 300$mỗi truy vấn và$t \le 25$. Điều này loại trừ mọi nhu cầu tối ưu hóa nặng hoặc cấu trúc dữ liệu nâng cao. Chúng ta có thể đủ khả năng sắp xếp, kiểm tra từng cặp và thậm chí$O(n^2)$xác nhận cho mỗi ứng viên mà không cần quan tâm. 

Một vấn đề tế nhị nảy sinh từ tính đối xứng. Một nỗ lực ngây thơ có thể cho rằng việc nhân các giá trị nhỏ nhất và lớn nhất sẽ cho kết quả$x$. Điều này không thành công khi cấu trúc số chia không nhất quán hoặc không đầy đủ. Một trường hợp thất bại khác là khi các số trùng lặp hoặc các ước số bổ sung bị thiếu làm hỏng việc ghép nối dự kiến. 

Ví dụ: nếu đầu vào là:```
3
2 3 4
```một sự tái thiết bất cẩn có thể đoán được$x = 12$, nhưng 12 có các ước {2, 3, 4, 6} nên danh sách không đầy đủ và không nhất quán. 

Một trường hợp cạnh khác là khi chỉ có một ước số được đưa ra:```
1
2
```Điều này gợi ý$x$có thể là 4, vì ước số không tầm thường duy nhất của nó là 2. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là thử tất cả các ứng cử viên có thể cho$x$. Vì mọi số đã cho đều phải chia hết$x$, giới hạn trên tự nhiên của$x$là tích của phần tử nhỏ nhất và lớn nhất trong danh sách hoặc thậm chí là cấu trúc LCM của chúng. Một chiến lược ngây thơ sẽ là liệt kê tất cả các ứng cử viên cho đến$10^6$và kiểm tra xem bộ ước số của chúng có khớp với đầu vào hay không sau khi loại bỏ 1 và$x$. Điều này nhanh chóng trở nên không khả thi vì việc tạo ước số cho mỗi ứng viên sẽ tốn kém$O(\sqrt{x})$và lặp đi lặp lại trên nhiều ứng cử viên dẫn đến chi phí trong trường hợp xấu nhất vượt xa giới hạn. 

Quan sát quan trọng là tập hợp ước số đầy đủ của$x$phải bao gồm các cặp$(d, x/d)$. Vì chúng ta có tất cả các ước trừ điểm cuối nên chúng ta có thể xây dựng lại$x$bằng cách ghép nối các phần tử nhỏ nhất và lớn nhất và kiểm tra tính nhất quán. Nếu danh sách đúng, ước số nhỏ nhất phải ghép với số lớn nhất, số nhỏ thứ hai với số lớn thứ hai, v.v., tất cả đều tạo ra cùng một sản phẩm$x$. 

Điều này làm giảm vấn đề sắp xếp và xác minh một giá trị ứng viên duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(10^6 \sqrt{x})$|$O(1)$| Quá chậm | 
| Kiểm tra ghép nối |$O(n \log n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp danh sách ước đã cho. Việc sắp xếp là cần thiết để chúng ta có thể so khớp nhỏ nhất với lớn nhất một cách đáng tin cậy. 
2. Tính giá trị ứng viên$x$là tích của phần tử nhỏ nhất và phần tử lớn nhất. 
3. Đối với mọi chỉ số$i$, xác minh rằng$d[i] \cdot d[n-1-i] = x$. 
4. Nếu bất kỳ cặp nào không đáp ứng điều kiện này, danh sách không thể đến từ một số hợp lệ, do đó xuất ra -1. 
5. Nếu tất cả các cặp khớp nhau, xuất ra$x$. 

Kiểm tra ghép nối là hạn chế trung tâm: mọi ước số bên dưới$\sqrt{x}$phải tương ứng với phần bổ sung duy nhất ở trên$\sqrt{x}$, và tích của chúng phải bằng$x$. 

### Tại sao nó hoạt động 

Tập hợp ước số của bất kỳ số nguyên nào đều đối xứng khi nhân thành$x$. Việc sắp xếp thể hiện sự đối xứng này dưới dạng các vị trí được phản chiếu. Nếu đầu vào đúng, mỗi phần tử nhỏ nhất phải ghép duy nhất với phần tử lớn nhất tạo ra cùng một sản phẩm$x$. Bất kỳ sai lệch nào đều phá vỡ thuộc tính đóng của số chia, nghĩa là không có số nguyên nào có thể tạo ra danh sách. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        d = list(map(int, input().split()))
        d.sort()

        x = d[0] * d[-1]

        ok = True
        for i in range(n // 2):
            if d[i] * d[n - 1 - i] != x:
                ok = False
                break

        if ok:
            print(x)
        else:
            print(-1)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp danh sách ước số sao cho phần bù được căn chỉnh đối xứng. Ứng viên$x$có nguồn gốc từ cặp ngoài cùng, vì các bộ ước số hợp lệ phải thỏa mãn$d_1 \cdot d_k = x$. 

Sau đó, vòng lặp sẽ xác minh tính bất biến của mỗi cặp được phản chiếu. Việc thoát sớm sẽ ngăn chặn việc tính toán không cần thiết khi phát hiện thấy sự không nhất quán. 

Một sai lầm phổ biến là quên rằng mọi cặp đều phải cho ra cùng một sản phẩm, không chỉ sản phẩm đầu tiên và sản phẩm cuối cùng. Một cách khác là giả định tính duy nhất của hệ số hóa mà không kiểm tra tính nhất quán trên tất cả các cặp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8
8 2 12 6 4 24 16 3
```Đã sắp xếp:```
2 3 4 6 8 12 16 24
```| tôi | d[i] | d[n-1-i] | sản phẩm | 
| --- | --- | --- | --- | 
| 0 | 2 | 24 | 48 | 
| 1 | 3 | 16 | 48 | 
| 2 | 4 | 12 | 48 | 
| 3 | 6 | 8 | 48 | 

Tất cả các sản phẩm phù hợp, vì vậy$x = 48$. 

Điều này xác nhận cấu trúc của một mạng chia số hợp lệ trong đó mỗi phần tử kết hợp chặt chẽ với nhau. 

### Ví dụ 2 

đầu vào:```
1
2
```Đã sắp xếp:```
2
```Chỉ có một số chia. Cách giải thích nhất quán duy nhất là$x = 4$, vì danh sách ước của 4 không bao gồm 1 và 4 là {2}. Thuật toán tạo ra$x = 2 \cdot 2 = 4$, điều đó đúng. 

Trường hợp này cho thấy danh sách một phần tử vẫn phù hợp với quy tắc ghép nối đối xứng như thế nào, coi phần tử ở giữa là ghép nối với chính nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế; kiểm tra cặp là tuyến tính | 
| Không gian |$O(1)$| Chỉ lưu trữ danh sách đầu vào | 

Các ràng buộc cho phép tối đa 300 phần tử cho mỗi lần kiểm tra, do đó, việc sắp xếp và xác minh tuyến tính có thể dễ dàng đủ nhanh ngay cả trên 25 truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        d = list(map(int, input().split()))
        d.sort()
        x = d[0] * d[-1]
        ok = True
        for i in range(n // 2):
            if d[i] * d[-1 - i] != x:
                ok = False
                break
        out.append(str(x if ok else -1))
    return "\n".join(out)

# provided samples
assert run("2\n8\n8 2 12 6 4 24 16 3\n1\n2\n") == "48\n4"

# custom cases
assert run("1\n3\n2 3 4\n") == "-1"
assert run("1\n2\n3 9\n") == "27"
assert run("1\n4\n2 4 3 6\n") == "12"
assert run("1\n1\n100\n") == "10000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 2 3 4 | -1 | đối xứng chia số không hợp lệ | 
| 3 9 | 27 | tái thiết chia đơn | 
| 2 4 3 6 | 12 | trường hợp ghép nối hoàn hảo | 
| 100 | 10000 | trường hợp cạnh đơn phần tử | 

## Vỏ cạnh 

Một trường hợp phức tạp là khi tất cả các ước số có cấu trúc ghép nối bằng nhau nhưng không đạt được tính nhất quán toàn cục. Ví dụ:```
3
2 3 4
```Sắp xếp mang lại`2, 3, 4`, sản xuất ứng viên$x = 8$. Việc kiểm tra ghép nối không thành công ngay lập tức vì$3 \cdot 3 \neq 8$. Thuật toán từ chối nó một cách chính xác. 

Một trường hợp khác là đầu vào tối thiểu:```
1
k
```Ở đây thuật toán xử lý$x = k^2$, vì số chia duy nhất phải ghép với chính nó. Điều này phù hợp với yêu cầu danh sách loại trừ 1 và$x$, để lại một ước số nội duy nhất trong các trường hợp suy biến.
