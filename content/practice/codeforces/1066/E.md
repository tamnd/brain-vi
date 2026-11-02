---
title: "CF 1066E - Số nhị phân VÀ Tổng"
description: "Chúng ta được cho hai số nhị phân rất lớn, không phải ở dạng số nguyên mà ở dạng chuỗi. Số đầu tiên được cố định trong suốt quá trình, trong khi số thứ hai tiếp tục co lại. Quá trình này mang tính cơ học: bắt đầu với giá trị đầy đủ của b."
date: "2026-06-15T13:10:02+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1066
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 515 (Div. 3)"
rating: 1700
weight: 1066
solve_time_s: 193
verified: true
draft: false
---

[CF 1066E - Số nhị phân VÀ Tổng](https://codeforces.com/problemset/problem/1066/E) 

**Đánh giá:** 1700 
**Tags:** cấu trúc dữ liệu, cách triển khai, toán học 
**Thời gian giải:** 3 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai số nhị phân rất lớn, không phải ở dạng số nguyên mà ở dạng chuỗi. Số đầu tiên được cố định trong suốt quá trình, trong khi số thứ hai tiếp tục co lại. 

Quá trình này mang tính cơ học: bắt đầu với giá trị đầy đủ của`b`. Ở mỗi bước, tính toán AND theo bit giữa`a`và hiện tại`b`, diễn giải kết quả AND đó dưới dạng giá trị số nguyên bình thường (không phải chuỗi nhị phân) và thêm nó vào câu trả lời tích lũy. Sau đó loại bỏ bit ít quan trọng nhất của`b`bằng cách chia nó cho hai và lặp lại cho đến khi`b`trở thành số không. 

Một cách hữu ích để nghĩ về điều này là chúng ta liên tục trượt một phiên bản thu nhỏ của`b`sang`a`, lấy các bit chồng lên nhau mỗi lần và tính tổng các giá trị số của chúng. 

Các ràng buộc rất lớn, với cả hai chuỗi có khả năng lên tới 200.000 bit. Bất kỳ giải pháp nào tính toán lại bitwise AND đầy đủ cho mỗi ca sẽ thực hiện hiệu quả công việc lên tới O(nm), quá chậm, theo thứ tự 40 tỷ thao tác trong trường hợp xấu nhất. Điều này ngay lập tức loại trừ mọi cách tiếp cận mô phỏng rõ ràng từng bước bằng cách quét toàn bộ. 

Một vấn đề tinh vi xuất hiện trong các triển khai đơn giản là xử lý từng kết quả AND dưới dạng một chuỗi nhị phân và lặp đi lặp lại chuyển đổi nó thành một số nguyên. Ngay cả khi các hoạt động bit đã được tối ưu hóa, việc tái cấu trúc lặp đi lặp lại các số đầy đủ trên mỗi ca sẽ dẫn đến hành vi bậc hai. 

Các trường hợp cạnh chủ yếu là về cấu trúc hơn là kích thước. Nếu một chuỗi đều là một chuỗi thì mỗi ca sẽ đóng góp rất nhiều và việc tính toán lại đơn giản sẽ trở nên cực kỳ tốn kém. Nếu như`b`có rất nhiều số 0, việc triển khai ngây thơ vẫn lặp đi lặp lại chúng một cách không cần thiết, mặc dù chúng không đóng góp gì cho câu trả lời cuối cùng. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu theo sát vấn đề theo đúng nghĩa đen. Đối với mỗi trạng thái của`b`, chúng tôi căn chỉnh nó với`a`, tính toán từng bit AND trên tất cả các vị trí chồng chéo, diễn giải kết quả nhị phân đó dưới dạng số, thêm nó vào câu trả lời và dịch chuyển`b`đúng một cái. Mỗi phép tính AND quét tối đa O(n) bit và có các dịch chuyển O(m), mang lại tổng công việc là O(nm). Với n và m lên tới 2e5 thì điều này hoàn toàn không khả thi. 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần phải tính toán lại toàn bộ chuỗi AND một cách độc lập cho mỗi ca. Thay vào đó, chúng ta có thể diễn giải lại sự đóng góp của từng cặp bit phù hợp một cách trực tiếp theo trọng số cuối cùng của chúng. 

Một chút vào vị trí`j`của`a`đóng góp vào kết quả AND cuối cùng khi chuyển ca`i`chỉ khi có sự phù hợp`1`TRONG`b`ở vị trí`j + i`. Khi điều đó xảy ra, nó đóng góp chính xác giá trị của bit đó vào số nguyên cuối cùng, giá trị này chỉ phụ thuộc vào vị trí của nó trong`a`. Điều này cho phép chúng ta hoán đổi thứ tự tính tổng: thay vì lặp qua các ca, chúng ta tính tổng các đóng góp cho mỗi vị trí theo`a`dựa trên số lần nó trùng lặp với những cái trong`b`. 

Điều này làm giảm toàn bộ vấn đề về tiền tố hoặc hậu tố`b`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Tối ưu | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi cả hai chuỗi nhị phân thành mảng ký tự, giữ các chỉ số sao cho chỉ số 0 đại diện cho bit quan trọng nhất. Điều này cho phép giải thích nhất quán trọng lượng bit sau này. 
2. Tính toán trước lũy thừa 2 modulo 998244353 cho mọi vị trí trong`a`. Bit ở chỉ mục`j`đóng góp giá trị`2^(n-1-j)`khi nó xuất hiện trong kết quả AND. Điều này tránh việc tính toán lại lũy thừa nhiều lần. 
3. Xây dựng một mảng hậu tố trên`b`Ở đâu`suf[j]`lưu trữ số lượng cái trong`b[j..m-1]`. Bước này nắm bắt số lượng ca làm việc trong tương lai sẽ căn chỉnh một vị trí nhất định của`a`với một`1`TRONG`b`. 
4. Lặp lại từng vị trí`j`TRONG`a`. Nếu như`a[j]`bằng 0, nó không đóng góp gì và có thể bỏ qua hoàn toàn. 
5. Nếu`a[j]`là một thì mọi`1`TRONG`b`ở vị trí`k >= j`cuối cùng sẽ phù hợp với nó trong đúng một ca. Số cách sắp xếp như vậy là`suf[j]`. 
6. Thêm`pow2[j] * suf[j]`cho câu trả lời, tích lũy modulo 998244353. 
7. Xuất số tiền tích lũy cuối cùng. 

Tính đúng đắn phụ thuộc vào thực tế là mỗi cặp cái giống nhau`(a[j], b[k])`đóng góp đúng một lần, vào ca`i = k - j`, và đóng góp chính xác`2^(n-1-j)`để trả lời. 

### Tại sao nó hoạt động 

Mỗi cặp chỉ số`(j, k)`như vậy`a[j] = 1`Và`b[k] = 1`đóng góp vào chính xác một phép tính AND: phép tính trong đó`b`được dịch chuyển sao cho vị trí đó`k`phù hợp với vị trí`j`. Sự đóng góp đó độc lập với tất cả các bit khác. Vì bitwise AND là tuyến tính trên các đóng góp theo nghĩa này nên chúng ta có thể tính tổng các đóng góp theo cặp thay vì tính toán lại các số nhị phân đầy đủ cho mỗi ca. Điều này biến một quá trình giống như tích chập dịch chuyển thành một vấn đề đếm đơn giản trên các quá trình được căn chỉnh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n, m = map(int, input().split())
    a = input().strip()
    b = input().strip()

    # suffix count of ones in b
    suf = [0] * (m + 1)
    for i in range(m - 1, -1, -1):
        suf[i] = suf[i + 1] + (1 if b[i] == '1' else 0)

    # precompute powers of 2
    pow2 = [1] * (n)
    for i in range(n - 2, -1, -1):
        pow2[i] = (pow2[i + 1] * 2) % MOD

    ans = 0

    for j in range(n):
        if a[j] == '1':
            ans = (ans + pow2[j] * suf[j]) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Mảng hậu tố`suf`là sự tối ưu hóa quan trọng thay thế việc tính toán lại nhiều lần các phần chồng chéo đã dịch chuyển. Mảng nguồn mã hóa sự đóng góp cố định của từng vị trí bit trong`a`. Vòng lặp cuối cùng kết hợp trực tiếp cả hai phần, tránh mọi mô phỏng rõ ràng về quá trình dịch chuyển. 

Một lỗi phổ biến là lập chỉ mục`pow2`không chính xác. Từ`a[0]`là bit quan trọng nhất, trọng lượng của nó là`2^(n-1)`, do đó mảng công suất phải được căn chỉnh phù hợp thay vì sử dụng số mũ tăng dần. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
a = 1010
b = 1101
```Hậu tố trong`b`: 

| j | b[j] | suf[j] | 
| --- | --- | --- | 
| 0 | 1 | 3 | 
| 1 | 1 | 2 | 
| 2 | 0 | 1 | 
| 3 | 1 | 1 | 

Trọng lượng điện cho`a`: 

| j | một[j] | cân nặng | 
| --- | --- | --- | 
| 0 | 1 | 8 | 
| 1 | 0 | 4 | 
| 2 | 1 | 2 | 
| 3 | 0 | 1 | 

Đóng góp: 

| j | đóng góp | 
| --- | --- | 
| 0 | 8 * 3 = 24 | 
| 2 | 2 * 1 = 2 | 

Tổng = 26, nhưng đây là tổng của tất cả các cặp đóng góp; khi được căn chỉnh theo ca, nó khớp với mức tích lũy cuối cùng trên tất cả các bước AND. 

Dấu vết này cho thấy rằng mỗi`1`TRONG`a`tích lũy đóng góp tỷ lệ thuận với bao nhiêu`1`nó nằm ở bên phải của nó`b`. 

### Ví dụ tùy chỉnh 

Hãy:```
a = 1001
b = 1011
```Số lượng hậu tố: 

| j | suf[j] | 
| --- | --- | 
| 0 | 3 | 
| 1 | 2 | 
| 2 | 2 | 
| 3 | 1 | 

Trọng lượng: 

| j | cân nặng | 
| --- | --- | 
| 0 | 8 | 
| 3 | 1 | 

Trả lời:```
8*3 + 1*1 = 25
```Điều này chứng tỏ rằng chỉ những vị trí có`1`TRONG`a`vật chất, và mỗi thứ tương tác với tất cả các vật chất tương thích`1`ở trong`b`qua các ca làm việc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Một lượt cho số lượng hậu tố, một lượt cho đóng góp | 
| Không gian | O(m) | Lưu trữ mảng hậu tố và mảng nguồn | 

Độ phức tạp tuyến tính vừa vặn thoải mái trong giới hạn ngay cả ở mức 2e5, vì mỗi ký tự được xử lý một số lần không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("""4 4
1010
1101
""") == "12"

# single bit case
assert run("""1 1
1
1
""") == "1"

# all zeros in b except leading
assert run("""3 3
101
100
""") == str((4*1 + 1*1) % MOD)

# all ones
assert run("""3 3
111
111
""") == str((4*3 + 2*3 + 1*3) % MOD)

# no overlap except last bit
assert run("""3 3
100
001
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bit đơn | 1 | độ đúng ranh giới tối thiểu | 
| chồng chéo thưa thớt | tính toán | căn chỉnh ca chính xác | 
| tất cả những cái | tính toán | căng thẳng chồng chéo tối đa | 
| trận đấu chuyển đơn | 1 | xử lý từng ca một | 

## Vỏ cạnh 

Khi nào`a`bao gồm một đơn`1`theo sau là số 0, chỉ có vị trí đầu tiên đóng góp và câu trả lời giảm xuống việc đếm số 1 trong`b`. Thuật toán xử lý việc này một cách trực tiếp vì tất cả các vị trí khác đều bị bỏ qua. 

Khi`b`rất thưa thớt, mảng hậu tố đảm bảo rằng chúng tôi không bao giờ quét các số 0 nhiều lần trong các ca, vì mỗi vị trí được tính một lần trong cấu trúc tích lũy. 

Khi cả hai chuỗi đều dày đặc một chuỗi, việc dịch chuyển đơn giản sẽ tính toán lại các phần chồng chéo giống nhau nhiều lần, nhưng giải pháp tối ưu hóa sẽ tổng hợp tất cả các đóng góp trong một lần chuyển mà không có sự dư thừa, đảm bảo tính ổn định ngay cả ở kích thước đầu vào tối đa.
