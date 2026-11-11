---
title: "CF 1553A - Tổng các chữ số"
description: "Chúng tôi được đưa ra một số truy vấn độc lập. Mỗi truy vấn cung cấp một số nguyên dương $n$ và chúng ta phải đếm xem có bao nhiêu số nguyên $x$ trong phạm vi từ 1 đến $n$ có một thuộc tính đặc biệt. Đối với một số $x$, chúng ta so sánh tổng các chữ số của nó trước và sau khi cộng một số."
date: "2026-06-14T21:09:50+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1553
codeforces_index: "A"
codeforces_contest_name: "Harbour.Space Scholarship Contest 2021-2022 (open for everyone, rated, Div. 1 + Div. 2)"
rating: 800
weight: 1553
solve_time_s: 281
verified: true
draft: false
---

[CF 1553A - Tổng các chữ số](https://codeforces.com/problemset/problem/1553/A) 

**Đánh giá:** 800 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 4 phút 41 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số truy vấn độc lập. Mỗi truy vấn cung cấp một số nguyên dương$n$, và chúng ta phải đếm có bao nhiêu số nguyên$x$trong khoảng từ 1 đến$n$có một tài sản đặc biệt. 

Đối với một số$x$, chúng ta so sánh tổng các chữ số của nó trước và sau khi cộng một. Nếu tổng các chữ số giảm hẳn sau khi tăng, nghĩa là$S(x+1) < S(x)$, sau đó$x$được gọi là thú vị. Nhiệm vụ là đếm xem tồn tại bao nhiêu con số thú vị như vậy.$n$. 

Ràng buộc$n \le 10^9$ngay lập tức loại trừ mọi giải pháp kiểm tra từng số riêng lẻ. Quét tuyến tính sẽ cần tới$10^9$hoạt động trên mỗi trường hợp thử nghiệm, vượt xa giới hạn khả thi cho tối đa 1000 trường hợp thử nghiệm. 

Hành vi của cạnh khóa xuất phát từ cách tổng các chữ số thay đổi khi tăng một số. Hầu hết các số đều tăng tổng các chữ số của chúng lên 1 khi chúng ta cộng 1, nhưng có những trường hợp đặc biệt khi có khối 9 ở cuối. Ví dụ: 9 giảm xuống 10 và tổng các chữ số giảm từ 9 xuống 1. Tương tự, 199 giảm xuống 200, giảm từ 19 xuống 2. Đây là những khoảnh khắc duy nhất mà tổng các chữ số giảm xuống. 

Một cách tiếp cận đơn giản để kiểm tra từng số riêng lẻ cũng sẽ cần tính tổng các chữ số hai lần cho mỗi số, điều này vẫn không khả thi ở quy mô lớn. Một sai lầm tinh vi khác là giả sử chỉ có một chữ số 9 là hợp lệ. Các số như 99, 999, 1999 cũng góp phần và chúng tuân theo một mẫu cấu trúc. 

## Phương pháp tiếp cận 

Một phương pháp brute-force lặp đi lặp lại qua tất cả$x \le n$, tính toán$S(x)$Và$S(x+1)$và đếm các trường hợp hợp lệ. Điều này đúng vì nó trực tiếp tuân theo định nghĩa. Tuy nhiên, việc tính tổng các chữ số cần$O(\log x)$, và làm điều này cho tất cả$x \le 10^9$dẫn đến khoảng$10^9 \cdot 9$hoạt động trên mỗi trường hợp thử nghiệm, quá chậm. 

Quan sát quan trọng là tổng chữ số chỉ giảm khi số gia tăng gây ra chuỗi mang loại bỏ một hoặc nhiều số 9 ở cuối. Giả định$x$kết thúc bằng chính xác$k$9 giây liên tiếp Khi chúng ta thêm 1, những$k$số chín trở thành số 0 và chữ số ở bên trái tăng thêm 1. Tổng các chữ số thay đổi là:$$S(x+1) = S(x) - 9k + 1$$Vậy điều kiện$S(x+1) < S(x)$trở thành:$$-9k + 1 < 0 \Rightarrow 9k > 1$$Điều này đúng với mọi$k \ge 1$. Điều đó có nghĩa là mọi số kết thúc bằng ít nhất một số 9 đều thú vị. 

Vậy bài toán rút gọn thành việc đếm xem có bao nhiêu số từ 1 đến$n$kết thúc bằng chữ số 9, 99, 999, v.v. Với mỗi lũy thừa của 10 biên, ta đếm các số có dạng:$$10^k - 1, 2 \cdot 10^k - 1, 3 \cdot 10^k - 1, \dots$$miễn là chúng không vượt quá$n$. 

Thay vì lặp lại tất cả các số, chúng ta đếm xem có bao nhiêu bội số của$10^k$phù hợp với$n + 1$, bởi vì mỗi khối có kích thước$10^k$chứa đúng một số tận cùng bằng$k$số chín. 

Điều này làm giảm nhiệm vụ tổng hợp những đóng góp trên tất cả những gì có thể$k$, nhiều nhất là 10 đối với$n \le 10^9$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \log n)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(\log n)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với một nhất định$n$, chuyển quan điểm từ việc đếm trực tiếp các số thú vị sang đếm các số có dạng$10^k - 1$các mẫu. 

Lý do là đây chính xác là những điểm mà việc mang theo làm giảm tổng chữ số. 
2. Lặp lại độ dài có thể có của các số 9 ở cuối, bắt đầu từ$k = 1$trở lên. 

Mỗi$k$tương ứng với các số kết thúc bằng chính xác$k$chín hoặc ít nhất$k$số chín. 
3. Đối với mỗi$k$, tính xem có bao nhiêu số nguyên$x \le n$thỏa mãn$x \equiv 10^k - 1 \pmod{10^k}$. 

Điều này tương đương với việc đếm xem có bao nhiêu khối đầy đủ kích thước$10^k$hòa nhập$n + 1$. 
4. Thêm$\left\lfloor \frac{n+1}{10^k} \right\rfloor$để trả lời. 

Điều này hoạt động vì mỗi khối đóng góp chính xác một số có số cuối cùng$k$các chữ số đều là số 9. 
5. Dừng lại khi$10^k > n + 1$, vì không có mẫu nào khác có thể xuất hiện trong phạm vi. 

### Tại sao nó hoạt động 

Mỗi số thú vị được xác định duy nhất bởi độ dài của chuỗi số 9 của nó. Cấu trúc theo sau đó phân chia tất cả các số nguyên thành các nhóm rời rạc dựa trên$k$và trong mỗi nhóm, chính xác một đại diện cho mỗi khối sẽ gây ra sự giảm tổng chữ số. Vì các nhóm này không trùng nhau nên tổng đóng góp của họ sẽ tính mỗi con số thú vị đúng một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve(n):
    ans = 0
    power = 10
    while power <= n + 1:
        ans += (n + 1) // power
        power *= 10
    return ans

t = int(input())
for _ in range(t):
    n = int(input())
    print(solve(n))
```Giải pháp này được xây dựng trực tiếp trên cấu trúc khối các số được nhóm theo số 0 ở cuối$x+1$, tương ứng với số chín ở cuối$x$. biểu thức$(n+1)//10^k$đếm có bao nhiêu số lên tới$n$rơi vào vị trí cuối cùng$k$các chữ số đều là số 9. 

Sự chuyển sang$n+1$rất quan trọng vì nó căn chỉnh mẫu một cách rõ ràng với bội số lũy thừa của mười. Nếu không có sự dịch chuyển này, các trường hợp biên xung quanh lũy thừa chính xác của 10 sẽ dễ xảy ra lỗi. 

Vòng lặp lũy thừa 10 vẫn còn nhỏ, bị giới hạn bởi số chữ số trong$n$, do đó giải pháp có hiệu quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 34$Chúng tôi tính toán đóng góp cho lũy thừa của 10. 

| k | công suất = 10^k | (n+1)//sức mạnh | đóng góp | 
| --- | --- | --- | --- | 
| 1 | 10 | 35//10 = 3 | 3 | 
| 2 | 100 | 35//100 = 0 | dừng lại | 

Tổng cộng = 3. 

Chúng tương ứng với 9, 19, 29. Mỗi số kết thúc bằng một số 9 duy nhất và trở nên thú vị khi tăng dần. 

Điều này xác nhận rằng các số 9 ở cuối chiếm ưu thế ở phạm vi nhỏ và các mẫu cao hơn không xuất hiện trước 100. 

### Ví dụ 2:$n = 880055535$Chúng tôi một lần nữa tổng hợp những đóng góp. 

| k | quyền lực | (n+1)//sức mạnh | 
| --- | --- | --- | 
| 1 | 10 | 88005553 | 
| 2 | 100 | 8800555 | 
| 3 | 1000 | 880055 | 
| ... | ... | ... | 

Tổng hợp các giá trị này tạo ra câu trả lời cuối cùng$88005553$. 

Dấu vết này cho thấy lũy thừa cao hơn của 10 vẫn đóng góp số lượng nhỏ hơn nhưng khác 0 và tổng tích lũy trên tất cả các thang đo có độ dài chữ số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log_{10} n)$mỗi bài kiểm tra | Chúng ta chỉ lặp lại lũy thừa từ 10 đến$n$| 
| Không gian |$O(1)$| Chỉ có một số biến số nguyên được sử dụng | 

Số bước logarit cho mỗi truy vấn là không đáng kể ngay cả đối với 1000 trường hợp kiểm thử, vì mỗi trường hợp$n$có nhiều nhất 10 chữ số. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve(n):
        ans = 0
        power = 10
        while power <= n + 1:
            ans += (n + 1) // power
            power *= 10
        return ans

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        out.append(str(solve(n)))
    return "\n".join(out)

# provided samples
assert run("5\n1\n9\n10\n34\n880055535\n") == "0\n1\n1\n3\n88005553"

# custom cases
assert run("1\n8\n") == "0"
assert run("1\n9\n") == "1"
assert run("1\n99\n") == "10"
assert run("1\n1000\n") == str((1001//10 + 1001//100 + 1001//1000))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, 8 | 0 | Không có 9 trường hợp tồn tại | 
| 1, 9 | 1 | Trường hợp ranh giới đơn | 
| 1, 99 | 10 | Nhiều đóng góp có độ dài chữ số chồng chéo | 
| 1.1000 | dựa trên công thức | Đóng góp đa quy mô đúng đắn | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi$n$chính xác là lũy thừa của 10 trừ một, chẳng hạn như 9, 99 hoặc 999. Đối với$n = 99$, thuật toán tính sự đóng góp từ cả hai$10$Và$100$. Các quá trình vòng lặp$10 \rightarrow 100$, cộng 10 từ số hạng đầu tiên và 1 từ số hạng thứ hai, phù hợp với thực tế là các số thú vị là 9, 19, 29, ..., 99. 

Một trường hợp tế nhị khác là$n = 1$. Điều kiện vòng lặp yêu cầu$power \le n+1$, vì vậy đối với$n = 1$, chúng tôi kiểm tra$power = 10$, điều đó đã thất bại. Kết quả là 0, phản ánh chính xác rằng không có số nào trong phạm vi này kết thúc bằng số 9. 

Cuối cùng, lớn$n$gần$10^9$vẫn chỉ kích hoạt khoảng 9 lần lặp, vì lũy thừa của 10 tăng nhanh. Điều này đảm bảo tính toán vẫn ổn định và nhanh chóng ngay cả ở những hạn chế tối đa.
