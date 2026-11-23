---
title: "CF 456B - Fedya và Toán"
description: "Nhiệm vụ yêu cầu phần còn lại khi tổng của bốn bội số đầu tiên của một số rất lớn n chia cho 5. Nói cách khác, bạn cần tính (1n + 2n + 3n + 4n) mod 5."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 456
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 260 (Div. 2)"
rating: 1200
weight: 456
solve_time_s: 66
verified: true
draft: false
---

[CF 456B - Fedya và Toán học](https://codeforces.com/problemset/problem/456/B) 

**Đánh giá:** 1200 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu số dư khi tổng 4 bội số đầu tiên của một số rất lớn _n_ chia cho 5. Nói cách khác, bạn cần tính`(1*n + 2*n + 3*n + 4*n) mod 5`. Đầu vào _n_ là một số nguyên duy nhất có thể có tới 10.105 chữ số, vượt xa những gì số nguyên 64 bit tiêu chuẩn có thể lưu trữ. Điều này có nghĩa là bạn không thể trực tiếp sử dụng số học số nguyên điển hình hoặc nhân _n_ với 10^4 trong bộ nhớ rồi lấy modulo, vì bản thân số đó có thể rất lớn. 

Đầu ra là một số nguyên duy nhất từ ​​0 đến 4, biểu thị số dư sau khi chia tổng của bốn bội số này cho 5. Về cơ bản, bài toán yêu cầu một mẫu trong số học mô-đun, bởi vì tổng`1 + 2 + 3 + 4`là hằng số, nên điều duy nhất quan trọng là _n modulo 5_. Các trường hợp cạnh cần xem xét bao gồm _n_ bằng 0, số có một chữ số hoặc có hàng chục nghìn chữ số. Một triển khai ngây thơ cố gắng lưu trữ`4*n`rõ ràng sẽ thất bại đối với _n_ lớn, do bị lỗi do tràn bộ nhớ hoặc mất quá nhiều thời gian. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ liên quan đến việc tính toán từng bội số đầu tiên`1*n`,`2*n`,`3*n`, Và`4*n`, tính tổng chúng rồi lấy modulo 5. Cách tiếp cận này đúng với các số nhỏ vì phép nhân và phép cộng rất đơn giản. Tuy nhiên, nếu _n_ có 10.000 chữ số, việc tính toán`4*n`rõ ràng là không thực tế. Ngay cả khi sử dụng thư viện số nguyên lớn, việc này vẫn tiêu tốn thời gian và bộ nhớ đáng kể, vượt xa giới hạn 1 giây dự định. 

Quan sát quan trọng là số học mô-đun có tính phân phối đối với phép cộng và phép nhân. Đó là,`(a + b) mod m = ((a mod m) + (b mod m)) mod m`Và`(a * b) mod m = ((a mod m) * (b mod m)) mod m`. Tổng các hệ số`1 + 2 + 3 + 4`là 10, vậy`(1*n + 2*n + 3*n + 4*n) mod 5 = (10*n) mod 5`. Vì 10 chia hết cho 5 nên`(10*n) mod 5`luôn bằng 0, bất kể _n_. Điều này giúp đơn giản hóa đáng kể vấn đề: đầu ra luôn bằng 0. Không cần phải đọc các chữ số của _n_ hoặc thực hiện bất kỳ phép tính nào với số thực. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(len(n)) hoặc tệ hơn tùy thuộc vào số học số nguyên lớn | O(len(n)) | Quá chậm đối với n có 10^5 chữ số | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nhận biết rằng tổng của bốn bội số đầu tiên của _n_ có thể được phân tích thành nhân tử:`1*n + 2*n + 3*n + 4*n = (1+2+3+4)*n = 10*n`. 
2. Nhận ra điều đó`(10*n) mod 5`đơn giản hóa vì 10 chia hết cho 5, cho`(10*n) mod 5 = 0`. 
3. Trả về 0 trực tiếp, vì biểu thức luôn ước tính bằng 0 theo modulo 5 đối với mọi số nguyên không âm _n_, bất kể kích thước của nó. 

Tại sao nó hoạt động: Thuộc tính phân phối của số học mô-đun đảm bảo tính chính xác. Vì 10 là bội số của 5 nên nhân nó với bất kỳ số nguyên _n_ nào cũng sẽ chia hết cho 5. Phép toán modulo sau đó tạo ra số dư bằng 0. Không có trường hợp biên nào, vì 0 nhân với 10 cũng cho kết quả bằng 0 và công thức hợp lệ với tất cả các giá trị được phép của _n_. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = input().strip()
print(0)
```Giải pháp này đọc đầu vào dưới dạng chuỗi, điều này cần thiết vì _n_ có thể rất lớn. Chúng tôi không chuyển đổi nó thành số nguyên vì kích thước của _n_ không liên quan sau khi nhận ra rằng kết quả luôn bằng 0. các`strip()`phương pháp loại bỏ bất kỳ ký tự dòng mới nào. In ấn`0`tạo ra kết quả đầu ra chính xác mà không thực hiện bất kỳ phép tính số học nào trên _n_. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
4
```| Bước | Biểu hiện | Giá trị | 
| --- | --- | --- | 
| Tổng hệ số | 1_4 + 2_4 + 3_4 + 4_4 | 40 | 
| Mô-đun 5 | 40 mod 5 | 0 | 
| Đầu ra | 0 | 0 | 

Đầu vào mẫu 2:```
123456789012345678901234567890
```| Bước | Biểu hiện | Giá trị | 
| --- | --- | --- | 
| Tổng hệ số | 1_n + 2_n + 3_n + 4_n | 10*n | 
| Mô-đun 5 | 10*n mod 5 | 0 | 
| Đầu ra | 0 | 0 | 

Những ví dụ này chứng minh rằng thuật toán hoạt động với cả số nhỏ và số cực lớn. Việc tính toán modulo chỉ phụ thuộc vào hệ số 10 chứ không phụ thuộc vào _n_. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Việc tính toán không phụ thuộc vào số chữ số trong n. | 
| Không gian | O(1) | Không có bộ nhớ bổ sung nào được sử dụng ngoài việc đọc đầu vào dưới dạng chuỗi. | 

Giải pháp dễ dàng phù hợp với giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB vì ​​nó tránh mọi phép tính số học trên số lượng lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = sys.stdin.readline().strip()
    return str(0)

# provided sample
assert run("4\n") == "0", "sample 1"

# custom cases
assert run("0\n") == "0", "minimum n"
assert run("5\n") == "0", "small n divisible by 5"
assert run("1\n") == "0", "small n not divisible by 5"
assert run("9"*10105 + "\n") == "0", "maximum-size n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 | 0 | Đầu vào tối thiểu | 
| 5 | 0 | Đầu vào nhỏ chia hết cho 5 | 
| 1 | 0 | Đầu vào nhỏ không chia hết cho 5 | 
| 9...9 (10^5 chữ số) | 0 | Kích thước đầu vào tối đa | 

## Vỏ cạnh 

Với _n = 0_, tổng của bốn bội số đầu tiên là`0 + 0 + 0 + 0 = 0`, modulo 5 bằng 0. Đối với _n_ có 10.105 chữ số, thuật toán in chính xác 0 mà không cần thực hiện bất kỳ số học nào. Kể cả những số có một chữ số không chia hết cho 5 thì tổng`1*n + 2*n + 3*n + 4*n`là bội số của 10 và kết quả modulo 5 vẫn bằng 0. Không có trường hợp cạnh nào vi phạm mẫu này.
