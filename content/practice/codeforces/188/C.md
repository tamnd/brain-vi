---
title: "CF 188C - LCM"
description: "Chúng ta có hai số nguyên dương và cần tính bội số chung nhỏ nhất của chúng. Bội số chung nhỏ nhất, thường được viết tắt là LCM, là số nguyên dương nhỏ nhất có thể chia hết cho cả hai số mà không để lại phần dư."
date: "2026-06-04T23:19:02+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 188
codeforces_index: "C"
codeforces_contest_name: "Surprise Language Round 6"
rating: 1400
weight: 188
solve_time_s: 182
verified: true
draft: false
---

[CF 188C - LCM](https://codeforces.com/problemset/problem/188/C) 

**Đánh giá:** 1400 
**Tags:** *đặc biệt, thực hiện, toán học 
**Thời gian giải:** 3m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai số nguyên dương và cần tính bội số chung nhỏ nhất của chúng. Bội số chung nhỏ nhất, thường được viết tắt là LCM, là số nguyên dương nhỏ nhất có thể chia hết cho cả hai số mà không để lại phần dư. 

Ví dụ: nếu các số là 10 và 42 thì 210 chia hết cho cả 10 và 42 và không có số dương nhỏ hơn nào có thuộc tính đó. 

Những hạn chế là rất nhỏ. Cả hai con số nhiều nhất là 1000, vì vậy ngay cả những phương pháp khá kém hiệu quả cũng sẽ kết thúc nhanh chóng. Điều đó nói lên rằng, bài toán về cơ bản là một bài toán và có một công thức chuẩn để tính ra đáp án ngay lập tức. Vì đầu vào chỉ chứa hai số nên thời gian chạy thực tế là không đổi. 

Một lỗi phổ biến là nhân trực tiếp các số và cho rằng tích luôn là LCM. 

Coi như:```
Input:
6 8
```Câu trả lời đúng là:```
24
```Việc thực hiện bất cẩn có thể dẫn đến:```
48
```vì 6 × 8 = 48. Tích chia hết cho cả hai số, nhưng nó không phải là giá trị nhỏ nhất. 

Một trường hợp tinh vi khác xảy ra khi một số đã chia hết cho số kia.```
Input:
5 10
```Câu trả lời đúng là:```
10
```Số lớn hơn đã là bội số chung của cả hai giá trị. Việc trả lại sản phẩm 50 sẽ không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là vũ lực. Bắt đầu từ số lớn hơn trong hai số, liên tục kiểm tra các số nguyên liên tiếp cho đến khi tìm được số nguyên chia hết cho cả hai số đầu vào. Số đầu tiên như vậy là LCM. 

Điều này có hiệu quả vì định nghĩa của LCM chính xác là "bội số chung dương nhỏ nhất". Cuối cùng, bội số chung phải xuất hiện nên việc tìm kiếm kết thúc. 

Mặc dù những hạn chế là rất nhỏ nhưng sức mạnh vũ phu vẫn có thể làm những việc không cần thiết. Trong trường hợp xấu nhất, chúng ta có thể kiểm tra nhiều số ứng viên trước khi đưa ra câu trả lời. 

Quan sát chính là mối quan hệ giữa ước chung lớn nhất (GCD) và bội số chung nhỏ nhất:$$\text{LCM}(a,b)\times\text{GCD}(a,b)=a\times b$$Công thức này hoạt động vì GCD đại diện cho hệ số được tính hai lần khi nhân hai số với nhau. Chia cho GCD sẽ loại bỏ sự trùng lặp đó. 

Khi chúng tôi tính toán GCD bằng thuật toán Euclid, LCM sẽ ngay lập tức như sau:$$\text{LCM}(a,b)=\frac{a\times b}{\text{GCD}(a,b)}$$Thuật toán của Euclid chạy cực kỳ nhanh, khiến phương pháp này vừa tinh tế vừa hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(LCM(a,b)) | O(1) | Được chấp nhận cho những ràng buộc này, nhưng không hiệu quả | 
| Tối ưu | O(log(min(a,b))) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`a`Và`b`. 
2. Tính toán`g = gcd(a, b)`bằng thuật toán Euclid. 

Thuật toán Euclid liên tục thay thế`(a, b)`với`(b, a % b)`cho đến khi giá trị thứ hai trở thành 0. Giá trị khác 0 còn lại là GCD. 
3. Tính bội số chung nhỏ nhất bằng cách sử dụng:$$\text{lcm}=\frac{a\times b}{g}$$Phép chia phải được thực hiện bởi GCD vì các thừa số chung được tính hai lần trong tích. 
4. Xuất LCM thu được. 

### Tại sao nó hoạt động 

Tính đúng đắn xuất phát từ nhận dạng$$\text{LCM}(a,b)\times\text{GCD}(a,b)=a\times b.$$GCD chứa chính xác các thừa số được chia sẻ bởi cả hai số. Khi nhân`a`Và`b`, những yếu tố được chia sẻ đó xuất hiện hai lần. Chia cho GCD sẽ loại bỏ một bản sao của mỗi thừa số chung, để lại số nhỏ nhất vẫn chứa tất cả các thừa số nguyên tố mà cả hai đầu vào yêu cầu. Con số đó chính xác là bội số chung nhỏ nhất. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

a, b = map(int, input().split())

print(a * b // gcd(a, b))
```Giải pháp đọc hai số nguyên và tính ước số chung lớn nhất của chúng bằng cách sử dụng công cụ tích hợp sẵn của Python`gcd`chức năng. 

Công thức`a * b // gcd(a, b)`trực tiếp thực hiện mối quan hệ toán học giữa GCD và LCM. Phép chia số nguyên được sử dụng vì kết quả được đảm bảo là số nguyên. 

Một chi tiết triển khai đáng ghi nhớ là thứ tự thực hiện. Phép nhân phải xảy ra trước phép chia. Trong các ngôn ngữ có loại số nguyên có chiều rộng cố định, điều này có thể gây tràn, nhưng số nguyên Python tự động tăng lên, do đó không có rủi ro ở đây. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 42
```| Bước | một | b | gcd | lcm | 
| --- | --- | --- | --- | --- | 
| Đọc đầu vào | 10 | 42 | - | - | 
| Tính GCD | 10 | 42 | 2 | - | 
| Tính LCM | 10 | 42 | 2 | 210 | 

Đầu ra:```
210
```Ví dụ này cho thấy trường hợp tiêu chuẩn trong đó các số có chung một thừa số. GCD là 2, do đó chia tích 420 cho 2 sẽ được bội số chung nhỏ nhất, 210. 

### Ví dụ 2 

đầu vào:```
5 10
```| Bước | một | b | gcd | lcm | 
| --- | --- | --- | --- | --- | 
| Đọc đầu vào | 5 | 10 | - | - | 
| Tính GCD | 5 | 10 | 5 | - | 
| Tính LCM | 5 | 10 | 5 | 10 | 

Đầu ra:```
10
```Dấu vết này cho thấy trường hợp một số chia cho số kia. LCM đơn giản là giá trị lớn hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log(min(a, b))) | Thuật toán Euclid tính GCD theo thời gian logarit | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Với đầu vào không lớn hơn 1000, giải pháp này về cơ bản chạy ngay lập tức và sử dụng bộ nhớ không đáng kể. Nó dễ dàng phù hợp trong giới hạn nhất định. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io
from math import gcd

def solve():
    a, b = map(int, input().split())
    print(a * b // gcd(a, b))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    global input
    input = sys.stdin.readline

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return out

# provided sample
assert run("10 42\n") == "210\n", "sample 1"

# custom cases
assert run("1 1\n") == "1\n", "minimum values"
assert run("5 10\n") == "10\n", "one divides the other"
assert run("6 8\n") == "24\n", "shared factors"
assert run("1000 1000\n") == "1000\n", "maximum equal values"
assert run("999 1000\n") == "999000\n", "coprime near upper bound"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Đầu vào hợp lệ tối thiểu | 
|`5 10`|`10`| Một số chia hết cho số kia | 
|`6 8`|`24`| Xử lý đúng các yếu tố chung | 
|`1000 1000`|`1000`| Giá trị bằng nhau ở giới hạn trên | 
|`999 1000`|`999000`| Số nguyên tố gần giới hạn tối đa | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
5 10
```Thuật toán tính toán:```
gcd(5, 10) = 5
lcm = (5 × 10) / 5 = 10
```Đầu ra là:```
10
```Điều này xử lý chính xác tình huống trong đó một số đã là bội số của số kia. 

Bây giờ hãy xem xét:```
6 8
```Thuật toán tính toán:```
gcd(6, 8) = 2
lcm = (6 × 8) / 2 = 24
```Đầu ra là:```
24
```Điều này xác nhận rằng việc chia cho GCD sẽ loại bỏ các thừa số chung trùng lặp khỏi sản phẩm. 

Cuối cùng, hãy xem xét các số bằng nhau:```
1000 1000
```Việc tính toán trở thành:```
gcd(1000, 1000) = 1000
lcm = (1000 × 1000) / 1000 = 1000
```Kết quả chính xác là số ban đầu, là bội số chung nhỏ nhất chính xác khi cả hai đầu vào giống hệt nhau.
