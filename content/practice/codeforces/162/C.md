---
title: "CF 162C - Phân tích thành thừa số nguyên tố"
description: "Chúng ta được yêu cầu lấy một số nguyên dương và biểu thị nó dưới dạng tích của các số nguyên tố, hiển thị số lần nó xuất hiện trong phân tích nhân tử cho mỗi số nguyên tố. Ví dụ: số 245 có thể được biểu thị bằng 5 nhân 7 hai lần, do đó kết quả sẽ là 577."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 162
codeforces_index: "C"
codeforces_contest_name: "VK Cup 2012 Wild-card Round 1"
rating: 1800
weight: 162
solve_time_s: 76
verified: true
draft: false
---

[CF 162C - Phân tích thành thừa số nguyên tố](https://codeforces.com/problemset/problem/162/C) 

**Đánh giá:** 1800 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu lấy một số nguyên dương và biểu thị nó dưới dạng tích của các số nguyên tố, hiển thị số lần nó xuất hiện trong phân tích nhân tử cho mỗi số nguyên tố. Ví dụ: số 245 có thể được biểu thị bằng 5 nhân 7 hai lần, do đó kết quả đầu ra sẽ là 5_7_7. Đầu vào là một số nguyên n, giới hạn từ 2 đến 10000. Đầu ra là các thừa số nguyên tố theo thứ tự không giảm, với bội số được phản ánh bằng cách in lặp lại từng thừa số. 

Giới hạn trên của n là 10000 cho phép chúng ta xem xét các thuật toán thực hiện tối đa khoảng 10^6 thao tác mà không đạt đến giới hạn thời gian, do đó, cách tiếp cận phân chia thử nghiệm đơn giản là khả thi. Tuy nhiên, việc triển khai bất cẩn có thể sai với các trường hợp biên nhỏ như bản thân số nguyên tố, số là bình phương hoàn hảo của số nguyên tố hoặc số có nhiều thừa số nhỏ lặp lại. Ví dụ: n = 13 sẽ xuất ra 13, không tạo ra hệ số trống và n = 16 sẽ xuất ra 2_2_2_2, không chỉ 2_8 hoặc 4*4, vì chúng tôi chỉ yêu cầu thừa số nguyên tố. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là lặp lại từ 2 đến n và liên tục chia n cho bất kỳ số nào chia đều cho n. Mỗi phép chia tương ứng với một lần xuất hiện của yếu tố đó. Phương pháp brute-force này hoạt động vì cuối cùng chúng ta thử mọi số nguyên cho đến n, vì vậy chúng ta sẽ tìm ra tất cả các thừa số nguyên tố. Với n = 10000, trong trường hợp xấu nhất, điều này có thể yêu cầu 10000 lần lặp và mỗi lần lặp có thể bao gồm các phép chia lặp đi lặp lại, dẫn đến độ phức tạp khoảng O(n). Điều này có thể chấp nhận được với n ≤ 10000, nhưng chúng ta có thể làm tốt hơn. 

Điểm mấu chốt là chúng ta chỉ cần kiểm tra các ước số đến căn bậc hai của n. Bất kỳ thừa số nào lớn hơn √n đều phải ghép với thừa số nhỏ hơn, vì vậy khi chúng ta đã chia tất cả các thừa số nhỏ hơn thì bản thân n còn lại phải là số nguyên tố. Điều này làm giảm đáng kể số lần lặp lại, biến cách tiếp cận O(n) tiềm năng thành O(√n), nhanh hơn nhiều và vẫn dễ thực hiện với n 10000. 

Cách tiếp cận tối ưu kết hợp phép chia thử nghiệm lên đến √n với việc xử lý mọi số nguyên tố còn lại sau đó. Bằng cách in ngay từng thừa số khi tìm thấy, chúng ta sẽ duy trì được thứ tự không giảm theo yêu cầu một cách tự nhiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Được chấp nhận với n ≤ 10000 | 
| Tối ưu (√n phép chia thử) | O(√n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên n từ đầu vào. Đây là con số chúng ta cần phân tích thành nhân tử. 
2. Khởi tạo biến d thành 2, đại diện cho ước số ứng viên hiện tại. Chúng ta bắt đầu với số 2 vì đây là số nguyên tố nhỏ nhất. 
3. Khi d*d ≤ n, kiểm tra xem n có chia hết cho d không. Nếu n % d == 0, in d (hoặc lưu trữ nó) và chia n cho d. Lặp lại bước này cho đến khi n không còn chia hết cho d. Điều này nắm bắt tất cả các lũy thừa của d trong hệ số hóa. 
4. Tăng d lên 1 và lặp lại bước 3. Chúng ta chỉ cần tăng đến √n vì bất kỳ thừa số nào lớn hơn √n sẽ có thừa số nhỏ hơn tương ứng đã được chia. 
5. Sau vòng lặp, nếu n > 1, in n vì đây là thừa số nguyên tố còn lại. Điều này xử lý trường hợp n chính nó là số nguyên tố hoặc còn lại một thừa số nguyên tố lớn. 

Tại sao nó hoạt động: Ở mỗi bước, chúng tôi chia ra thừa số nguyên tố nhỏ nhất có sẵn. Điều bất biến là n luôn là phần còn lại của số chưa được phân tích thành nhân tử và tất cả các số nguyên tố nhỏ hơn d đã được chia hết. Điều này đảm bảo rằng chúng ta không bỏ sót bất kỳ yếu tố nào và các yếu tố đó được xuất ra theo thứ tự không giảm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
d = 2
factors = []

while d * d <= n:
    while n % d == 0:
        factors.append(str(d))
        n //= d
    d += 1

if n > 1:
    factors.append(str(n))

print('*'.join(factors))
```Mã tuân theo thuật toán từng bước. Chúng tôi duy trì một danh sách các yếu tố dưới dạng chuỗi để thuận tiện cho việc nối với '*'. Vòng lặp while bên trong đảm bảo tất cả lũy thừa của mỗi số nguyên tố đều được nắm bắt. Việc tăng d đảm bảo chúng tôi kiểm tra tất cả các ước số có thể lên đến √n. Cuối cùng, việc thêm n nếu nó lớn hơn 1 sẽ bao gồm mọi số nguyên tố còn lại lớn hơn √n. Một lỗi phổ biến là quên bước kiểm tra cuối cùng này, điều này sẽ cho kết quả không chính xác đối với số nguyên tố hoặc số có thừa số nguyên tố lớn. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1: 245 

| Bước | n | d | yếu tố | 
| --- | --- | --- | --- | 
| ban đầu | 245 | 2 | [] | 
| chia cho 2? | 245 | 2 | [] | 
| tăng d | 245 | 3 | [] | 
| tăng d | 245 | 4 | [] | 
| tăng d | 245 | 5 | [] | 
| chia cho 5 | 49 | 5 | ['5'] | 
| tăng d | 49 | 6 | ['5'] | 
| ... | 49 | 7 | ['5'] | 
| chia cho 7 | 7 | 7 | ['5', '7'] | 
| chia cho 7 | 1 | 7 | ['5', '7', '7'] | 
| kết thúc vòng lặp | 1 | 8 | ['5', '7', '7'] | 

Đầu ra: 5_7_7. Dấu vết cho thấy tất cả các yếu tố được ghi lại theo thứ tự không giảm và các yếu tố lặp lại xuất hiện đúng số lần. 

Đầu vào mẫu 2: 13 

| Bước | n | d | yếu tố | 
| --- | --- | --- | --- | 
| ban đầu | 13 | 2 | [] | 
| chia cho 2? | 13 | 2 | [] | 
| tăng d | 13 | 3 | [] | 
| tăng d | 13 | 4 | [] | 
| kết thúc vòng lặp | 13 | 5 | [] | 
| n còn lại > 1 | 13 | 5 | ['13'] | 

Đầu ra: 13. Thuật toán xử lý chính xác một số nguyên tố bằng cách thêm nó vào cuối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(√n) | Vòng lặp bên ngoài lặp tới √n, vòng lặp bên trong chia lũy thừa của mỗi số nguyên tố. Với n 10000, đây là vài trăm lần lặp. | 
| Không gian | O(log n) | Chúng tôi lưu trữ danh sách các yếu tố. Số thừa số tối đa là O(log n) vì thừa số nguyên tố nhỏ nhất là 2 nên số lần 2 chia hết n là log2(n). | 

Độ phức tạp này nằm trong giới hạn 3 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    d = 2
    factors = []

    while d * d <= n:
        while n % d == 0:
            factors.append(str(d))
            n //= d
        d += 1

    if n > 1:
        factors.append(str(n))

    return '*'.join(factors)

# Provided sample
assert run("245\n") == "5*7*7", "sample 1"

# Custom cases
assert run("16\n") == "2*2*2*2", "all 2s"
assert run("13\n") == "13", "prime number"
assert run("10000\n") == "2*2*2*2*5*5*5*5", "mixed small primes"
assert run("9973\n") == "9973", "large prime"
assert run("72\n") == "2*2*2*3*3", "small composite with multiple primes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 16 | 2_2_2*2 | thừa số nguyên tố nhỏ lặp lại | 
| 13 | 13 | xử lý số nguyên tố | 
| 10000 | 2_2_2_2_5_5_5*5 | tổ hợp các số nguyên tố lặp lại | 
| 9973 | 9973 | số nguyên tố lớn | 
| 72 | 2_2_2_3_3 | nhiều số nguyên tố nhỏ khác nhau | 

## Vỏ cạnh 

Đối với số nguyên tố như 13, các ước số kiểm tra vòng lặp lên tới √13 sẽ không chia hết bất cứ thứ gì. Sau vòng lặp, n vẫn là 13, vì vậy việc thêm nó vào sẽ đảm bảo số nguyên tố được lấy chính xác. Đối với lũy thừa của một số nguyên tố như 16, vòng lặp bên trong chia 2 bốn lần, chứng tỏ rằng các thừa số lặp lại được tính chính xác. Đối với một số có cả số nguyên tố nhỏ và số nguyên tố lớn như 10000, thuật toán sẽ nắm bắt chính xác nhiều lần xuất hiện của 2 và 5 mà không bỏ sót bất kỳ yếu tố nào. Phương pháp này tránh được những lỗi thường gặp như dừng sau thừa số đầu tiên hoặc quên xử lý các số nguyên tố còn lại trên √n.
