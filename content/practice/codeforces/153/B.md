---
title: "CF 153B - Ký hiệu nhị phân"
description: "Chúng ta được cấp một số nguyên dương duy nhất và phải in biểu diễn của nó ở cơ số 2. Nói cách khác, chúng ta muốn biểu thị số đó dưới dạng lũy ​​thừa của hai và xuất ra chuỗi bit tương ứng."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 153
codeforces_index: "B"
codeforces_contest_name: "Surprise Language Round 5"
rating: 1800
weight: 153
solve_time_s: 89
verified: true
draft: false
---

[CF 153B - Ký hiệu nhị phân](https://codeforces.com/problemset/problem/153/B) 

**Đánh giá:** 1800 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một số nguyên dương duy nhất và phải in biểu diễn của nó ở cơ số 2. Nói cách khác, chúng ta muốn biểu thị số đó dưới dạng lũy thừa của hai và xuất ra chuỗi bit tương ứng. 

Ví dụ: số thập phân 13 bằng:$13 = 1\cdot2^3 + 1\cdot2^2 + 0\cdot2^1 + 1\cdot2^0$vậy ký hiệu nhị phân của nó là`1101`. 

Giới hạn là rất nhỏ. Giá trị của`n`nhiều nhất là một triệu, có nghĩa là biểu diễn nhị phân của nó chứa tối đa 20 bit vì:$2^{20}=1048576$Bất kỳ thuật toán hợp lý nào cũng dễ dàng phù hợp trong thời hạn. Ngay cả một cách tiếp cận liên tục chia số cho hai cũng chỉ thực hiện được khoảng 20 lần lặp. 

Phần khó khăn không phải là hiệu suất mà là tính chính xác trong việc định dạng. 

Một lỗi phổ biến là in các bit theo thứ tự ngược lại. Khi chúng tôi liên tục chia cho hai, chúng tôi thu được các bit từ ít quan trọng nhất đến quan trọng nhất. Nếu chúng ta in chúng ngay lập tức, câu trả lời sẽ bị đảo ngược. 

Ví dụ: 

đầu vào:```
5
```Phần còn lại xuất hiện theo thứ tự sau: 

| Hiện tại | n % 2 | 

|---|---|---| 

| 5 | 1 | 

| 2 | 0 | 

| 1 | 1 | 

Nếu chúng ta in chúng trực tiếp, chúng ta sẽ nhận được`101`từ dưới lên trên chứ không phải từ trên xuống dưới. 

Một sai lầm khác là tạo ra các số 0 đứng đầu. Ký hiệu nhị phân phải bắt đầu bằng bit được đặt cao nhất. 

Ví dụ: 

đầu vào:```
1
```Đầu ra đúng:```
1
```Việc triển khai chiều rộng cố định bất cẩn có thể in một cái gì đó như`00000001`, điều này không được phép. 

Trường hợp cạnh thứ ba đang xử lý chính xác số nhỏ nhất. Nếu điều kiện vòng lặp được viết không chính xác, chương trình có thể tạo ra một chuỗi trống cho`n = 1`. 

đầu vào:```
1
```Đầu ra đúng:```
1
```Thuật toán phải đảm bảo rằng ít nhất một bit được in. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là mô phỏng định nghĩa của ký hiệu nhị phân. Mỗi chữ số nhị phân cho biết một lũy thừa nhất định của hai có tham gia vào quá trình phân tách số hay không. 

Một cách tiếp cận bạo lực kiểm tra lũy thừa của hai từ lớn đến nhỏ. Đối với mỗi vị trí, chúng tôi xác định xem công suất đó có phù hợp với giá trị còn lại hay không. Nếu có, chúng tôi xuất ra`1`và trừ nó. Nếu không, chúng tôi xuất ra`0`. 

Ví dụ, đối với`13`:$13 = 8 + 4 + 0 + 1$vì vậy các bit trở thành`1101`. 

Phương pháp này đúng vì ký hiệu nhị phân chính xác là sự phân rã thành lũy thừa của hai. Vấn đề là trước tiên chúng ta cần xác định quyền lực liên quan lớn nhất. Nếu được triển khai một cách ngây thơ bằng cách tính lũy thừa lặp đi lặp lại hoặc quét nhiều vị trí không cần thiết, mã sẽ trở nên vụng về đối với một tác vụ đơn giản như vậy. 

Một quan sát rõ ràng hơn đến từ cách các số nhị phân được hình thành. Bit có trọng số nhỏ nhất bằng số dư khi chia cho hai.$n = 2q + r,\quad r\in\{0,1\}$Phần còn lại`r`chính xác là chữ số nhị phân hiện tại. Sau khi giải nén xong, chúng ta chia số cho hai và tiếp tục tính thương. 

Ví dụ, bắt đầu từ`13`: 

| n | n % 2 | n // 2 | 
| --- | --- | --- | 
| 13 | 1 | 6 | 
| 6 | 0 | 3 | 
| 3 | 1 | 1 | 
| 1 | 1 | 0 | 

Các bit xuất hiện theo thứ tự ngược lại:`1011`. Đảo ngược chúng mang lại`1101`. 

Cách tiếp cận này là tối ưu vì mỗi lần lặp sẽ loại bỏ một chữ số nhị phân. Vì một số lên tới một triệu chỉ có khoảng 20 bit nên thuật toán kết thúc gần như ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(log n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(log n) | O(log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`. 
2. Tạo danh sách trống`bits`để lưu trữ các chữ số nhị phân. 
3. Trong khi`n > 0`, hãy làm như sau: 

1. Tính toán`n % 2`. 
2. Nối phần còn lại này vào`bits`. 
3. Thay thế`n`với`n // 2`. 

Phần còn lại cho chữ số nhị phân có ý nghĩa thấp nhất hiện tại. Phép chia số nguyên sẽ loại bỏ chữ số đó và dịch số sang phải một bit. 
4. Đảo ngược danh sách`bits`. 

Các chữ số được tạo từ ít quan trọng nhất đến quan trọng nhất, do đó việc đảo ngược sẽ khôi phục lại thứ tự thích hợp. 
5. Nối các chữ số thành một chuỗi và in kết quả. 

### Tại sao nó hoạt động 

Ở mỗi lần lặp, thuật toán duy trì tính bất biến là số ban đầu có thể được xây dựng lại từ phần dư được thu thập và giá trị hiện tại của`n`. 

Giả sử giá trị hiện tại là`n`. Chúng tôi viết:$n = 2\left\lfloor\frac{n}{2}\right\rfloor + (n\bmod 2)$Phần còn lại luôn luôn là`0`hoặc`1`, khớp chính xác với một chữ số nhị phân. Sau khi ghi lại chữ số này, chúng ta tiếp tục với thương số. Lặp lại quá trình này sẽ trích xuất mọi chữ số nhị phân từ phải sang trái. Đảo ngược các chữ số đã thu thập sẽ khôi phục ký hiệu nhị phân từ trái sang phải tiêu chuẩn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

bits = []

while n > 0:
    bits.append(str(n % 2))
    n //= 2

bits.reverse()

print("".join(bits))
```Chương trình bắt đầu bằng cách đọc số nguyên từ đầu vào tiêu chuẩn. 

Danh sách`bits`lưu trữ các chữ số nhị phân dưới dạng chuỗi. Storing them as strings immediately avoids an extra conversion step during output construction.

 Bên trong vòng lặp,`n % 2`trích xuất bit có ý nghĩa nhỏ nhất hiện tại. Sau đó`n //= 2`loại bỏ bit đó khỏi số. Vòng lặp tiếp tục cho đến khi số trở thành số 0. 

Thứ tự của các hoạt động quan trọng. Ta phải ghi số dư trước khi chia. Đảo ngược hai dòng này sẽ làm mất bit hiện tại. 

Sau vòng lặp, các chữ số sẽ bị đảo ngược vì chúng được thu thập từ phải sang trái. Cuối cùng,`"".join(bits)`xây dựng đầu ra một cách hiệu quả. 

điều kiện`while n > 0`hoạt động chính xác vì vấn đề đảm bảo`n ≥ 1`. Nếu cho phép bằng 0, chúng ta sẽ cần xử lý đặc biệt vì vòng lặp sẽ không bao giờ thực thi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
```| Hiện tại | n % 2 | bit sau khi nối thêm | 
| --- | --- | --- | 
| 5 | 1 | [1] | 
| 2 | 0 | [1, 0] | 
| 1 | 1 | [1, 0, 1] | 

Sau khi đảo ngược: 

| bit | 
| --- | 
| [1, 0, 1] | 

Đầu ra:```
101
```Ví dụ này cho thấy các chữ số được tạo tự nhiên từ ít quan trọng nhất đến quan trọng nhất. Đảo ngược khôi phục lại thứ tự đúng. 

### Ví dụ 2 

đầu vào:```
13
```| Hiện tại | n % 2 | bit sau khi nối thêm | 
| --- | --- | --- | 
| 13 | 1 | [1] | 
| 6 | 0 | [1, 0] | 
| 3 | 1 | [1, 0, 1] | 
| 1 | 1 | [1, 0, 1, 1] | 

Sau khi đảo ngược: 

| bit | 
| --- | 
| [1, 1, 0, 1] | 

Đầu ra:```
1101
```Dấu vết này thể hiện sự phân rã thành lũy thừa của hai:$13 = 1\cdot2^3 + 1\cdot2^2 + 0\cdot2^1 + 1\cdot2^0$Thuật toán trích xuất chính xác các hệ số này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | Mỗi lần lặp chia số đó cho 2 | 
| Không gian | O(log n) | Biểu diễn nhị phân chứa các chữ số O(log n) | 

Từ`n ≤ 10^6`, vòng lặp chạy tối đa khoảng 20 lần. Cả thời gian chạy và mức sử dụng bộ nhớ đều rất nhỏ so với giới hạn của cuộc thi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    import sys
    input = sys.stdin.readline

    n = int(input())

    bits = []

    while n > 0:
        bits.append(str(n % 2))
        n //= 2

    bits.reverse()

    print("".join(bits))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("5\n") == "101\n", "sample 1"

# minimum value
assert run("1\n") == "1\n", "minimum input"

# power of two
assert run("8\n") == "1000\n", "single leading 1"

# alternating bits
assert run("10\n") == "1010\n", "checks ordering of bits"

# maximum constraint neighborhood
assert run("1000000\n") == bin(1000000)[2:] + "\n", "large input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Đầu vào hợp lệ nhỏ nhất | 
|`8`|`1000`| Xử lý đúng quyền hạn của hai | 
|`10`|`1010`| Các chữ số phải xuất hiện theo đúng thứ tự | 
|`1000000`|`11110100001001000000`| Đầu vào lớn gần giới hạn ràng buộc | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể: 

đầu vào:```
1
```Dấu vết thực hiện: 

| Hiện tại | n % 2 | bit | 
| --- | --- | --- | 
| 1 | 1 | [1] | 

Sau khi chia,`n`trở thành`0`, do đó vòng lặp dừng lại. Việc đảo ngược danh sách một phần tử không thay đổi gì và kết quả đầu ra sẽ trở thành:```
1
```Điều này xác nhận thuật toán không bao giờ tạo ra một chuỗi rỗng. 

Bây giờ hãy xem xét lũy thừa của hai: 

đầu vào:```
8
```Dấu vết thực hiện: 

| Hiện tại | n % 2 | bit | 
| --- | --- | --- | 
| 8 | 0 | [0] | 
| 4 | 0 | [0, 0] | 
| 2 | 0 | [0, 0, 0] | 
| 1 | 1 | [0, 0, 0, 1] | 

Sau khi đảo ngược: 

| bit | 
| --- | 
| [1, 0, 0, 0] | 

Đầu ra:```
1000
```Điều này xác minh rằng các số 0 đứng đầu không được đưa vào. Việc biểu diễn bắt đầu chính xác ở bit được đặt cao nhất. 

Cuối cùng, hãy xem xét một số có các bit thay thế: 

đầu vào:```
10
```Dấu vết thực hiện: 

| Hiện tại | n % 2 | bit | 
| --- | --- | --- | 
| 10 | 0 | [0] | 
| 5 | 1 | [0, 1] | 
| 2 | 0 | [0, 1, 0] | 
| 1 | 1 | [0, 1, 0, 1] | 

Đảo ngược sản xuất`1010`. 

Điều này mắc phải lỗi phổ biến là in phần dư ngay lập tức mà không đảo ngược chúng.
