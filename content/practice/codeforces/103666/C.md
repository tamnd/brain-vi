---
title: "CF 103666C - \u041c\u0430\u0440\u0441\u0438\u0430\u043d\u0441\u043a\u0438\u0435 \u043d\u043e\u043b\u0438\u043a\u0438"
description: "Chúng ta đang làm việc trong một hệ thống số vị trí có cơ số $k$, trong đó các số được viết bằng các chữ số từ $0$ đến $k-1$. Một số được gọi là “đủ tròn” nếu khi viết dưới dạng cơ số $k$, biểu diễn của nó kết thúc bằng ít nhất $n$ chữ số 0."
date: "2026-07-02T21:31:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103666
codeforces_index: "C"
codeforces_contest_name: "\u0422\u0443\u0440\u043d\u0438\u0440 \u0410\u0440\u0445\u0438\u043c\u0435\u0434\u0430 2016"
rating: 0
weight: 103666
solve_time_s: 49
verified: true
draft: false
---

[CF 103666C - \u041c\u0430\u0440\u0441\u0438\u0430\u043d\u0441\u043a\u0438\u0435 \u043d\u043e\u043b\u0438\u043a\u0438](https://codeforces.com/problemset/problem/103666/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang làm việc trong một hệ thống số vị trí có cơ số$k$, trong đó các số được viết bằng các chữ số từ$0$ĐẾN$k-1$. Một số được gọi là đủ tròn nếu khi viết dưới dạng cơ số$k$, biểu diễn của nó kết thúc bằng ít nhất$n$không có chữ số. Nói cách khác, số đó chia hết cho$k^n$. 

Nhiệm vụ là liệt kê tất cả các số nguyên dương có thuộc tính này theo thứ tự tăng dần và trả về$i$-thứ một trong biểu diễn thập phân thông thường. 

Từ những hạn chế,$k$có thể lớn như$10^9$,$n$có thể lên tới$100$, Và$i$có thể lên tới$10^9$. Câu trả lời được đảm bảo phù hợp với$10^{18}$, có nghĩa là chúng ta có thể sử dụng logic số học 64-bit một cách an toàn và tránh những lo ngại tràn vượt quá giới hạn đó. 

Một cách giải thích ngây thơ sẽ là lặp qua các số tự nhiên, chuyển đổi từng số thành cơ số$k$, đếm các số 0 ở cuối và chọn những số đủ điều kiện. Điều này ngay lập tức thất bại vì việc kiểm tra một số tốn kém$O(\log_k x)$và chúng tôi có thể cần tới$10^9$số hợp lệ, nghĩa là chúng tôi sẽ mô phỏng tối đa$10^9$ứng viên, vượt xa giới hạn khả thi. 

Trường hợp cạnh tinh tế xuất hiện khi$n = 0$. Khi đó mọi số đều hợp lệ và câu trả lời đơn giản là$i$. Bất kỳ giải pháp nào thực thi sai tính chia hết cho$k^n$như một điều kiện nghiêm ngặt ngay cả khi$n=0$phải cẩn thận tránh việc buộc phải chia bằng$1$-giống như logic có thể làm sai lệch việc lập chỉ mục. 

Một trường hợp cạnh khác là khi$k = 2$Và$n$là lớn. Mặc dù$k^n$phát triển nhanh, bài toán không yêu cầu ta liệt kê số lượng lớn; thay vào đó, chúng ta phải suy luận một cách có cấu trúc về cách các số có số 0 ở cuối hoạt động trong cơ số$k$. 

## Phương pháp tiếp cận 

Quan sát quan trọng là một số có ít nhất$n$số 0 ở cuối trong cơ sở$k$khi và chỉ khi nó chia hết cho$k^n$. Điều này biến vấn đề thành một câu hỏi cấp số cộng đơn giản: chúng ta được yêu cầu liệt kê tất cả các bội số dương của$k^n$. 

Vậy dãy số làm tròn đủ là:$$k^n,\; 2k^n,\; 3k^n,\; \dots$$các$i$-con số đó chính xác là:$$i \cdot k^n$$Cách tiếp cận bạo lực sẽ kiểm tra rõ ràng từng số nguyên, chuyển đổi nó thành cơ sở$k$và kiểm tra các số 0 ở cuối. Điều này hoạt động về mặt khái niệm vì việc chuyển đổi cơ sở rất đơn giản, nhưng nó không thành công về mặt tính toán vì mật độ của các số hợp lệ là$1/k^n$, vì vậy chúng tôi vẫn sẽ quét tới$i \cdot k^n$ứng viên trong trường hợp xấu nhất. 

Cách tiếp cận được tối ưu hóa bỏ qua hoàn toàn việc biểu diễn và dựa vào sự tương đương giữa các số 0 ở cuối trong cơ sở$k$và khả năng chia hết cho$k^n$. Một khi sự tương đương này được thiết lập, bài toán sẽ giảm xuống việc tính lũy thừa và phép nhân. 

Khó khăn kỹ thuật duy nhất là tính toán$k^n$một cách an toàn. Từ$n \le 100$, lũy thừa bằng cách bình phương lặp lại là đủ và ổn định trong số học 64 bit do đảm bảo rằng câu trả lời cuối cùng không vượt quá$10^{18}$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(i \cdot \log_k(i k^n))$|$O(1)$| Quá chậm | 
| Tối ưu |$O(\log n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

## 1. Giải thích điều kiện theo cơ sở$k$Một số kết thúc bằng$n$số không trong cơ sở$k$nghĩa là nó chia hết cho$k^n$. Điều này chuyển đổi một điều kiện dựa trên chữ số thành một điều kiện chia hết số học thuần túy. 

## 2. Tính toán$k^n$Chúng tôi tính toán$p = k^n$sử dụng lũy ​​thừa nhanh. Bước này là cần thiết vì phép nhân trực tiếp$n$thời gian có thể tràn hoặc không hiệu quả nếu$n$đã lớn hơn. 

## 3. Xây dựng dãy 

Tất cả các số hợp lệ tạo thành một cấp số cộng:$$p, 2p, 3p, \dots$$Vì vậy$i$-số hợp lệ chỉ đơn giản là$i \cdot p$. 

## 4. Xuất kết quả 

Trở lại$i \cdot k^n$như câu trả lời ở dạng thập phân. 

### Tại sao nó hoạt động 

Bất biến quan trọng là mọi số có ít nhất$n$số 0 ở cuối trong cơ sở$k$chính xác là bội số của$k^n$và mọi bội số của$k^n$có ít nhất$n$số 0 ở cuối. Đây là hệ quả trực tiếp của biểu diễn vị trí: mỗi số 0 ở cuối tương ứng với một hệ số của$k$. Do đó, việc sắp xếp các số bằng cách tăng giá trị sẽ bảo toàn thứ tự của bội số và$i$-số hợp lệ thứ chính xác là$i$- bội số thứ của$k^n$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def pow_fast(a, e):
    res = 1
    while e > 0:
        if e & 1:
            res *= a
        a *= a
        e >>= 1
    return res

def solve():
    k = int(input())
    n = int(input())
    i = int(input())

    p = pow_fast(k, n)
    print(i * p)

if __name__ == "__main__":
    solve()
```Việc thực hiện lần đầu tiên đọc$k$,$n$, Và$i$. Nó tính toán$k^n$sử dụng phép lũy thừa nhị phân, liên tục bình phương cơ số và nhân nó thành kết quả khi bit số mũ hiện tại được đặt. Cuối cùng, nó nhân kết quả với$i$, trực tiếp sản xuất$i$- bội số thứ của$k^n$. 

Một điểm tinh tế là chúng tôi không bao giờ kiểm tra rõ ràng các số 0 ở cuối hoặc thực hiện chuyển đổi cơ số. Toàn bộ điều kiện dựa trên chữ số được hấp thụ vào lũy thừa. 

## Ví dụ đã hoạt động 

Hãy xem xét$k = 10$,$n = 2$,$i = 3$. 

| Bước | Giá trị | 
| --- | --- | 
| Tính toán$10^2$| 100 | 
| Trình tự | 100, 200, 300,... | 
| Yếu tố thứ 3 | 300 | 

Điều này xác nhận rằng các số kết thúc bằng hai số 0 trong cơ số 10 chính xác là bội số của 100 và việc lập chỉ mục là trực tiếp. 

Bây giờ hãy xem xét$k = 2$,$n = 3$,$i = 5$. 

| Bước | Giá trị | 
| --- | --- | 
| Tính toán$2^3$| 8 | 
| Trình tự | 8, 16, 24, 32, 40, ... | 
| Yếu tố thứ 5 | 40 | 

Ví dụ này chứng tỏ rằng cơ số không quan trọng trong dạng số học cuối cùng; chỉ một$k^n$vấn đề. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log n)$| lũy thừa nhị phân cho$k^n$cộng với phép nhân theo thời gian không đổi | 
| Không gian |$O(1)$| chỉ một vài biến số nguyên được sử dụng | 

Các ràng buộc cho phép lên đến$n = 100$, vì vậy lũy thừa có hiệu quả là hằng số thời gian trong thực tế. Kết quả bị ràng buộc của$10^{18}$đảm bảo số nguyên Python vẫn có thể quản lý được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def pow_fast(a, e):
        res = 1
        while e > 0:
            if e & 1:
                res *= a
            a *= a
            e >>= 1
        return res

    k = int(sys.stdin.readline())
    n = int(sys.stdin.readline())
    i = int(sys.stdin.readline())

    p = pow_fast(k, n)
    return str(i * p)

# basic cases
assert run("10\n2\n3\n") == "300"
assert run("2\n3\n5\n") == "40"

# n = 0 case
assert run("7\n0\n10\n") == "10"

# small base
assert run("3\n1\n4\n") == "12"

# large i, small exponent
assert run("10\n1\n1000000\n") == "10000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10,2,3 | 300 | số 0 ở cuối thập phân tiêu chuẩn | 
| 2,3,5 | 40 | hành vi cơ sở không thập phân | 
| 7,0,10 | 10 | trường hợp cạnh n = 0 | 
| 3,1,4 | 12 | số mũ tối thiểu khác 0 | 
| 10,1,1000000 | 10000000 | mở rộng chỉ số lớn | 

## Vỏ cạnh 

Khi nào$n = 0$, mọi số đều hợp lệ vì mọi số nguyên đều có ít nhất 0 số 0 ở cuối trong bất kỳ cơ số nào. Công thức vẫn hoạt động vì$k^0 = 1$, vì vậy câu trả lời trở thành$i \cdot 1 = i$, phù hợp với trình tự dự kiến. 

Khi$k = 2$Và$n$lớn, nói$n = 60$, chúng tôi tính toán$2^{60}$, vẫn nằm trong phạm vi 64-bit. Dãy số trở thành bội số của$2^{60}$, và$i$-th hạng chỉ đơn giản là một số nguyên tỷ lệ. Thuật toán không bao giờ phụ thuộc trực tiếp vào cấu trúc biểu diễn nhị phân, do đó không có nguy cơ hiểu sai các số 0 ở cuối. 

Khi$i = 1$, đáp án chính xác đấy$k^n$. Đây là số hợp lệ nhỏ nhất và thuật toán trả về số đó một cách chính xác mà không cần điều chỉnh từng cái một vì việc lập chỉ mục bắt đầu từ bội số đầu tiên.
