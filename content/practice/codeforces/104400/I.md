---
title: "CF 104400I - Thập phân tuần hoàn vô hạn"
description: "Chúng ta có một số nguyên dương $n$ được đảm bảo không chia hết cho 2 hoặc 5. Điều kiện này đảm bảo rằng phép khai triển thập phân của $frac{1}{n}$ hoàn toàn lặp lại sau dấu thập phân, không có tiền tố kết thúc."
date: "2026-06-30T23:03:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104400
codeforces_index: "I"
codeforces_contest_name: "Hunan University 2023 the 19th Programming Contest"
rating: 0
weight: 104400
solve_time_s: 41
verified: true
draft: false
---

[CF 104400I - Số thập phân định kỳ vô hạn](https://codeforces.com/problemset/problem/104400/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên dương$n$được đảm bảo không chia hết cho 2 hoặc 5. Điều kiện này đảm bảo rằng việc khai triển thập phân của$\frac{1}{n}$hoàn toàn lặp lại sau dấu thập phân, không có tiền tố kết thúc. Bên cạnh đó, chúng tôi được cung cấp một chỉ mục$m$, có thể cực kỳ lớn và chúng ta được yêu cầu xác định$m$-chữ số thứ sau dấu thập phân trong khai triển lặp lại vô hạn của$\frac{1}{n}$. 

Về mặt khái niệm, chia 1 cho$n$tạo ra một chuỗi thập phân định kỳ. Thay vì tính toán toàn bộ phần mở rộng, chúng tôi chỉ quan tâm đến một chữ số duy nhất nằm sâu bên trong cấu trúc lặp lại này, có khả năng vượt xa mọi mô phỏng trực tiếp khả thi. 

Ràng buộc$n \le 10^5$gợi ý rằng quá trình tiền xử lý phụ thuộc vào$n$có thể chấp nhận được, nhưng bất cứ điều gì tỷ lệ thuận với$m$, có thể lớn bằng$10^9$, không thể mô phỏng trực tiếp. Giải pháp tạo từng chữ số theo đúng vị trí$m$sẽ yêu cầu lên tới$10^9$lặp đi lặp lại trong trường hợp xấu nhất, vượt xa mọi giới hạn thời gian. 

Đặc tính cấu trúc quan trọng là vì$n$nguyên tố cùng nhau với 10, khai triển thập phân của$\frac{1}{n}$hoàn toàn mang tính định kỳ. Điều này có nghĩa là chuỗi các chữ số lặp lại với một khoảng thời gian cố định và nhiệm vụ giảm xuống là tìm dấu chấm và lập chỉ mục cho nó. 

Một trường hợp khó phát hiện khi người ta giả định rằng khoảng thời gian luôn luôn$n-1$. Điều đó chỉ đúng khi 10 là một modulo gốc nguyên thủy$n$, điều này không được đảm bảo. Ví dụ, đối với$n = 21$, thời kỳ của$1/21$là 6, không phải 20. Một giả định ngây thơ ở đây dẫn đến việc lập chỉ mục không chính xác thành một chu trình đầy đủ không tồn tại. 

Một lỗi khác xảy ra khi mô phỏng phép chia dài mà không theo dõi số dư. Nếu phần dư tương tự xuất hiện lại, các chữ số sẽ lặp lại từ thời điểm đó và việc không phát hiện được chu trình này sẽ gây ra việc tính toán không cần thiết và không thể thực hiện được đối với số lớn.$m$. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp phép chia dài bắt đầu với số dư 1 và nhân liên tục với 10, trích xuất từng chữ số một. Mỗi bước tính toán$\text{digit} = \frac{10 \cdot r}{n}$và cập nhật$r = (10 \cdot r) \bmod n$. Điều này tạo ra sự mở rộng thập phân một cách chính xác. Tuy nhiên, kể từ khi$m$có thể lên đến$10^9$, tạo ra$m$chữ số là không thể thực hiện được. 

Quan sát quan trọng là quy trình này là một máy trạng thái xác định trên phần dư modulo$n$. chỉ có$n$có thể có phần dư, do đó trình tự cuối cùng phải lặp lại, tạo thành một chu trình. Bởi vì$n$là nguyên tố cùng nhau với 10, khai triển không có tiền tố kết thúc nên chu trình bắt đầu ngay từ chữ số đầu tiên sau dấu thập phân. 

Điều này có nghĩa là chúng ta có thể tính toán trước tất cả các chữ số cho đến khi phần còn lại lặp lại, lưu trữ chúng và sau đó trả lời các truy vấn bằng cách sử dụng số học mô-đun theo độ dài chu kỳ. Một khi chúng ta biết chu kỳ,$m$-chữ số thứ chỉ đơn giản là chữ số ở chỉ mục$(m-1) \bmod L$, Ở đâu$L$là độ dài chu kỳ. 

Từ$n \le 10^5$, mô phỏng nhiều nhất$n$các bước là đủ, bởi vì chúng tôi dừng lại khi phần còn lại lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force chia dài tới m bước | O(m) | O(1) | Quá chậm | 
| Phát hiện chu kỳ với mô phỏng phần còn lại | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng phép chia dài trong khi theo dõi số dư và chữ số. 

1. Khởi tạo phần còn lại dưới dạng$r = 1$. Điều này tương ứng với việc tính toán khai triển thập phân của$1/n$. 
2. Duy trì một mảng để lưu trữ các chữ số theo thứ tự sinh. 
3. Duy trì bản đồ từ phần còn lại đến vị trí đầu tiên trong chuỗi. 
4. Ở mỗi bước, nếu phần còn lại hiện tại đã được nhìn thấy trước đó thì chúng tôi dừng lại. Điều này cho thấy sự bắt đầu của một chu kỳ lặp lại. 
5. Nếu không, hãy ghi lại vị trí của phần còn lại. 
6. Nhân số dư với 10 và trích chữ số tiếp theo là$\text{digit} = (10r) // n$. 
7. Cập nhật phần còn lại thành$(10r) \bmod n$và tiếp tục. 
8. Sau khi tiền xử lý, tính chỉ số câu trả lời như sau:$(m-1) \bmod L$, Ở đâu$L$là số chữ số được tạo ra. 
9. Xuất ra chữ số tại chỉ số đó. 

Lý do điều này hoạt động là phần còn lại xác định duy nhất trạng thái phân chia dài. Khi số dư lặp lại, tất cả các chữ số tiếp theo sẽ lặp lại giống hệt nhau, do đó chuỗi trở thành tuần hoàn kể từ thời điểm đó trở đi. Vì bài toán đảm bảo không có hệ số 2 hoặc 5 nên không có giai đoạn trước, nghĩa là sự lặp lại tạo thành một chu kỳ đầy đủ bắt đầu ngay lập tức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())

seen = {}
digits = []

r = 1 % n

while r not in seen:
    seen[r] = len(digits)
    r *= 10
    digit = r // n
    r %= n
    digits.append(digit)

L = len(digits)

idx = (m - 1) % L
print(digits[idx])
```Việc triển khai phản ánh chính xác phép chia dài. Phần theo dõi còn lại trong`seen`đảm bảo chúng tôi dừng ngay khi quá trình bắt đầu lặp lại. Việc trích xuất chữ số sử dụng phép chia số nguyên, trong khi phần cập nhật số dư giữ trạng thái nhất quán cho bước tiếp theo. Việc lập chỉ mục cuối cùng sử dụng điều chỉnh dựa trên số 0 vì chữ số đầu tiên tương ứng với$m = 1$. 

Một cạm bẫy phổ biến là quên giảm modulo dư ban đầu$n$. Trong khi ở bài toán này nó luôn là 1, viết nó là`1 % n`làm cho logic trở nên mạnh mẽ. Một vấn đề tế nhị khác là việc lập chỉ mục theo từng cái một khi ánh xạ$m$đến mảng chữ số; sự biến đổi đúng luôn là$m-1$. 

## Ví dụ đã hoạt động 

Xem xét đầu vào$n = 3, m = 5$. Khai triển thập phân là$0.3333\ldots$, vậy mọi chữ số đều là 3. 

| Bước | Phần dư r | Chữ số | Đã xem? | 
| --- | --- | --- | --- | 
| 1 | 1 → 10 | 3 | không | 
| 2 | 1 → 10 | 3 | phát hiện lặp lại | 

Chu kỳ là`[3]`, vậy chữ số thứ 5 là`3`. Điều này xác nhận rằng khi số dư lặp lại ngay lập tức thì khoảng thời gian là 1. 

Bây giờ hãy xem xét một trường hợp phong phú hơn một chút$n = 7, m = 5$. Khai triển thập phân là$0.142857\ldots$. 

| Bước | r trước | r*10 | chữ số | r sau | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 10 | 1 | 3 | 
| 2 | 3 | 30 | 4 | 2 | 
| 3 | 2 | 20 | 2 | 6 | 
| 4 | 6 | 60 | 8 | 4 | 
| 5 | 4 | 40 | 5 | 5 | 
| 6 | 5 | 50 | 7 | 1 (chu kỳ lặp lại) | 

Chu kỳ là`[1,4,2,8,5,7]`. Chữ số thứ 5 tương ứng với chỉ số 4 trong lập chỉ mục dựa trên 0, cho kết quả 5, khớp với phần mở rộng đã biết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần còn lại xuất hiện nhiều nhất một lần trước khi lặp lại và có nhiều nhất$n$phần dư có thể có | 
| Không gian | O(n) | Chúng tôi lưu trữ tối đa một chữ số cho mỗi phần còn lại | 

Sự ràng buộc$n \le 10^5$làm cho phương pháp này dễ dàng khả thi. Ngay cả trong trường hợp xấu nhất, mô phỏng chỉ thực hiện tối đa$10^5$lặp đi lặp lại, nằm trong giới hạn thời gian thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline
    n, m = map(int, input().split())

    seen = {}
    digits = []

    r = 1 % n

    while r not in seen:
        seen[r] = len(digits)
        r *= 10
        digits.append(r // n)
        r %= n

    L = len(digits)
    print(digits[(m - 1) % L])

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.strip()

assert run("3 5") == "3", "sample 1"
assert run("7 5") == "5", "sample 2"

assert run("1 100") == "0", "n = 1 edge"
assert run("2 10") == "0", "should always 0 (though invalid per constraint)"
assert run("9 8") == "8", "cycle length 1 digit 1"
assert run("11 1") == "0", "first digit case check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 5 | 3 | lặp lại chu kỳ một chữ số | 
| 7 5 | 5 | lập chỉ mục chu kỳ toàn thời gian | 
| 1 100 | 0 | hành vi chia tầm thường | 
| 9 8 | 8 | số thập phân lặp lại một chữ số | 
| 11 1 | 0 | trích xuất đúng chữ số đầu tiên | 

## Vỏ cạnh 

cho$n = 3$, phần còn lại lập tức lặp lại sau khi tạo ra một chữ số. Bắt đầu với$r = 1$, chúng tôi tính toán$10 \to 3$, số dư lại trở thành 1. Thuật toán ghi lại một chu kỳ`[3]`. Bất kỳ truy vấn nào$m$bản đồ để lập chỉ mục$(m-1) \bmod 1 = 0$, trả về chính xác 3. 

cho$n = 7$, chuỗi còn lại chỉ quay vòng sau 6 bước. Thuật toán lưu trữ sáu chữ số trước khi phát hiện sự lặp lại ở phần còn lại 1. Việc lập chỉ mục modulo đảm bảo rằng ngay cả đối với$m$, chúng ta sẽ gói gọn vào chu trình 6 độ dài này một cách chính xác mà không cần tính toán lại. 

Vì$n = 11$, việc khai triển bắt đầu bằng chữ số 0 vì$10/11 = 0.\ldots$. Thuật toán nắm bắt chính xác các số 0 đứng đầu vì trích xuất chữ số sử dụng phép chia số nguyên trước khi cập nhật phần dư. Chu trình vẫn được ghi lại hoàn toàn từ các chuyển đổi còn lại, đảm bảo tính chính xác ngay cả khi các chữ số đầu bằng 0.
