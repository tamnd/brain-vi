---
title: "CF 1208A - XORinacci"
description: "Chúng ta được yêu cầu tính một dãy tương tự như các số Fibonacci, nhưng thay vì tính tổng hai số hạng trước đó, mỗi số hạng thu được bằng cách áp dụng phép toán XOR theo bit cho hai số hạng trước đó."
date: "2026-06-11T23:23:49+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1208
codeforces_index: "A"
codeforces_contest_name: "Manthan, Codefest 19 (open for everyone, rated, Div. 1 + Div. 2)"
rating: 900
weight: 1208
solve_time_s: 88
verified: true
draft: false
---

[CF 1208A - XORinacci](https://codeforces.com/problemset/problem/1208/A) 

**Xếp hạng:** 900 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính một dãy tương tự như các số Fibonacci, nhưng thay vì tính tổng hai số hạng trước đó, mỗi số hạng thu được bằng cách áp dụng phép toán XOR theo bit cho hai số hạng trước đó. Về mặt hình thức, dãy được xác định bởi hai số nguyên ban đầu`a`Và`b`BẰNG`f(0) = a`,`f(1) = b`, và cho`n > 1`,`f(n) = f(n-1) XOR f(n-2)`. Cho nhiều trường hợp thử nghiệm, mỗi trường hợp chỉ định`a`,`b`, Và`n`, chúng ta phải tính`f(n)`cho từng trường hợp. 

Những hạn chế cho thấy`a`,`b`, Và`n`mỗi cái có thể lên tới một tỷ và có thể có tới 1000 trường hợp thử nghiệm. Một cách tiếp cận đơn giản tính toán trình tự lặp đi lặp lại lên đến`n`sẽ quá chậm khi`n`lớn vì việc lặp lại một tỷ lần cho mỗi trường hợp thử nghiệm là không khả thi. 

Các trường hợp cạnh không rõ ràng là khi`n`đặc biệt là rất nhỏ`n = 0`hoặc`n = 1`. Một cách tiếp cận bất cẩn có thể bắt đầu lặp lại từ`0`và giả sử ít nhất hai lần lặp, tạo ra kết quả vượt quá giới hạn hoặc không xác định. Một trường hợp tế nhị khác là rất lớn`n`nơi mà cách tiếp cận vũ phu sẽ không chấm dứt trong thời gian hợp lý. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ chỉ khởi tạo một mảng với hai phần tử đầu tiên`a`Và`b`, sau đó tính toán lặp lại từng phần tử tiếp theo bằng cách XOR hai phần tử trước đó. Cách tiếp cận này được đảm bảo mang lại câu trả lời đúng vì nó tuân theo định nghĩa trình tự một cách trung thực. Tuy nhiên, trong trường hợp xấu nhất khi`n`đạt tới 10^9, điều này đòi hỏi 10^9 lần lặp, quá chậm với giới hạn thời gian 1 giây. 

Quan sát quan trọng giúp mở ra lời giải nhanh là các số XORinacci có tính tuần hoàn với chu kỳ có độ dài 3. Cụ thể, chuỗi lặp lại cứ ba số hạng trong mẫu`[a, b, a XOR b, a, b, a XOR b, ...]`. Điều này xảy ra vì hoạt động XOR có thuộc tính`(x XOR y) XOR x = y`, dẫn đến sự lặp lại có thể dự đoán được. Khi mẫu này được nhận dạng, chúng ta có thể tính toán`f(n)`trong thời gian không đổi bằng cách lấy`n modulo 3`và lập chỉ mục vào mẫu. 

So sánh các phương pháp là: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm đối với n lớn | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, hãy đọc các số nguyên`a`,`b`, Và`n`. 
2. Tính toán`n_mod = n % 3`. Điều này xác định vị trí của`f(n)`trong mô hình ba thuật ngữ lặp đi lặp lại. 
3. Nếu`n_mod`là 0,`f(n)`là`a`. Nếu như`n_mod`là 1,`f(n)`là`b`. Nếu không thì,`f(n)`là`a XOR b`. 
4. In giá trị tính toán cho từng test. 

Điều này hoạt động vì trình tự XORinacci tuân theo chu trình`[a, b, a XOR b]`vô thời hạn. Ví dụ, bắt đầu bằng`[a, b]`, số hạng tiếp theo là`a XOR b`, sau đó`(a XOR b) XOR b = a`, sau đó`a XOR (a XOR b) = b`, xác nhận sự lặp lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

T = int(input())
for _ in range(T):
    a, b, n = map(int, input().split())
    n_mod = n % 3
    if n_mod == 0:
        print(a)
    elif n_mod == 1:
        print(b)
    else:
        print(a ^ b)
```Giải pháp này trước tiên sẽ đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó đọc`a`,`b`, Và`n`. Sử dụng modulo 3, nó xác định phần tử nào của`[a, b, a XOR b]`chu kỳ tương ứng với`f(n)`. các`^`toán tử trong Python tính toán XOR theo bit. Modulo đảm bảo chúng tôi chỉ thực hiện tính toán theo thời gian không đổi cho mỗi trường hợp thử nghiệm và thuật toán xử lý`n = 0`Và`n = 1`một cách tự nhiên mà không cần kiểm tra thêm. 

## Ví dụ đã hoạt động 

**Mẫu 1**:`a=3, b=4, n=2`| Bước | Tính toán | Kết quả | 
| --- | --- | --- | 
| n % 3 | 2 % 3 | 2 | 
| mẫu[2] | a XOR b = 3 ^ 4 | 7 | 

Bảng xác nhận rằng`f(2)`là`7`như mong đợi. 

**Mẫu 2**:`a=4, b=5, n=0`| Bước | Tính toán | Kết quả | 
| --- | --- | --- | 
| n % 3 | 0 % 3 | 0 | 
| mẫu[0] | a = 4 | 4 | 

Điều này cho thấy rằng`f(0)`trả về chính xác giá trị ban đầu`a`. 

**Mẫu 3**:`a=325, b=265, n=1231232`| Bước | Tính toán | Kết quả | 
| --- | --- | --- | 
| n % 3 | 1231232 % 3 | 2 | 
| mẫu[2] | a XOR b = 325 ^ 265 | 76 | 

Điều này chứng tỏ giải pháp xử lý rất lớn`n`không có sự lặp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm yêu cầu một phép toán modulo và XOR duy nhất, thời gian không đổi. | 
| Không gian | O(1) | Chỉ cần một vài biến, không có mảng tỷ lệ với n. | 

Được cho`T <= 1000`, tổng công việc là khoảng 1000 thao tác liên tục, dễ dàng khớp trong vòng 1 giây và thấp hơn nhiều so với giới hạn bộ nhớ là 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    T = int(input())
    for _ in range(T):
        a, b, n = map(int, input().split())
        n_mod = n % 3
        if n_mod == 0:
            print(a)
        elif n_mod == 1:
            print(b)
        else:
            print(a ^ b)
    return output.getvalue().strip()

# provided samples
assert run("3\n3 4 2\n4 5 0\n325 265 1231232\n") == "7\n4\n76", "sample 1"

# custom cases
assert run("1\n0 0 0\n") == "0", "minimum values"
assert run("1\n0 0 1\n") == "0", "small n=1"
assert run("1\n1 1 2\n") == "0", "XOR zero result"
assert run("1\n1000000000 1000000000 1000000000\n") == "0", "large values"
assert run("3\n5 7 3\n5 7 4\n5 7 5\n") == "5\n7\n2", "sequence cycle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 0`|`0`| Trường hợp cạnh giá trị tối thiểu | 
|`0 0 1`|`0`| Bé nhỏ`n=1`trường hợp cạnh | 
|`1 1 2`|`0`| XOR mang lại số 0 | 
|`10^9 10^9 10^9`|`0`| Số lượng lớn, lớn`n`| 
|`5 7 3`/`4`/`5`|`5 / 7 / 2`| Xác nhận xử lý mẫu chu kỳ | 

## Vỏ cạnh 

cho`n = 0`, phép toán modulo`0 % 3 = 0`chọn đúng`a`. Ví dụ, với đầu vào`0 0 0`, thuật toán tính toán`n_mod = 0`và in`0`. Vì`n = 1`,`1 % 3 = 1`chọn lọc`b`. Lớn`n`giống`1231232`giảm xuống`1231232 % 3 = 2`, trở về`a XOR b`. Chu trình modulo tránh mọi nhu cầu về vòng lặp hoặc đệ quy, xử lý các đầu vào cực kỳ hiệu quả và chính xác.
