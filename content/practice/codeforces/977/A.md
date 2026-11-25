---
title: "CF 977A - Phép trừ sai"
description: "Nhiệm vụ này mô tả một cách rất cụ thể để giảm lặp đi lặp lại một số nguyên dương. Thay vì chỉ trừ một số theo nghĩa số học thông thường, phép tính phụ thuộc vào chữ số cuối cùng của số."
date: "2026-06-17T01:26:22+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 977
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 479 (Div. 3)"
rating: 800
weight: 977
solve_time_s: 71
verified: true
draft: false
---

[CF 977A - Phép trừ sai](https://codeforces.com/problemset/problem/977/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ này mô tả một cách rất cụ thể để giảm lặp đi lặp lại một số nguyên dương. Thay vì chỉ trừ một số theo nghĩa số học thông thường, phép tính phụ thuộc vào chữ số cuối cùng của số. Nếu số kết thúc bằng một chữ số khác 0 thì số đó hoạt động bình thường và giảm đi một. Nếu nó kết thúc bằng 0, thì số đó không giảm giá trị trực tiếp mà thay vào đó giảm hoàn toàn chữ số cuối cùng, tương đương với việc chia cho mười. 

Chúng ta được cho một số nguyên ban đầu n và một số phép tính k. Mỗi thao tác biến đổi số theo quy tắc trên và mục tiêu là xác định giá trị cuối cùng sau khi áp dụng chính xác k phép biến đổi đó. 

Ràng buộc n 10^9 ngụ ý số có tối đa 10 chữ số. Số lượng thao tác k nhiều nhất là 50, cực kỳ nhỏ. Điều này ngay lập tức loại trừ mọi nhu cầu tối ưu hóa ngoài mô phỏng đơn giản, vì ngay cả việc quét tuyến tính qua k bước cũng không đáng kể. 

Một sai lầm ngây thơ là hiểu phép toán luôn trừ đi một. Ví dụ: bắt đầu từ 100, một cách tiếp cận đơn giản sẽ tạo ra 99 sau một bước, nhưng thao tác đúng sẽ loại bỏ số 0 ở cuối, tạo ra 10. Một lỗi tinh vi khác là liên tục chuyển đổi giữa chuỗi và số nguyên không chính xác hoặc quên rằng việc xóa một chữ số sẽ thay đổi hoàn toàn độ lớn của số, điều này ảnh hưởng đến các bước tiếp theo. 

Các trường hợp đặc biệt xuất hiện khi số chứa nhiều số 0 ở cuối. Ví dụ: bắt đầu từ 1000 và áp dụng một thao tác sẽ cho kết quả là 100 chứ không phải 999. Một trường hợp đặc biệt khác là khi việc loại bỏ các số 0 nhiều lần sẽ nhanh chóng làm giảm số lượng chữ số, làm thay đổi đáng kể hành vi của các phép tính trong tương lai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực áp dụng trực tiếp quy tắc k lần. Mỗi bước kiểm tra chữ số cuối cùng và trừ một hoặc chia cho mười. Vì k tối đa là 50 nên cách tiếp cận này đã tối ưu trong thực tế và không cần tối ưu hóa thêm nữa. 

Quan sát quan trọng là không có cấu trúc ẩn nào ngoài thao tác chữ số cục bộ. Mỗi thao tác chỉ phụ thuộc vào chữ số cuối cùng hiện tại và sau mỗi phép biến đổi, số đó sẽ thay đổi theo cách duy trì tính chính xác cho bước tiếp theo. Không cần tính toán trước hoặc các phím tắt toán học vì bản thân quá trình này đã bị giới hạn bởi một k nhỏ. 

Do đó, giải pháp tối ưu chỉ đơn giản là mô phỏng quá trình chính xác như mô tả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(k) | O(1) | Đã chấp nhận | 
| Mô phỏng tối ưu | O(k) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên n và k từ đầu vào. Giá trị của n là số làm việc hiện tại và k là số lần chuyển đổi sẽ được áp dụng. 
2. Lặp lại quy trình sau chính xác k lần, vì mỗi lần lặp thể hiện một ứng dụng của phép toán Tanya. 
3. Kiểm tra chữ số cuối cùng của n bằng cách sử dụng n % 10. Điều này cô lập vị trí đơn vị mà không ảnh hưởng đến phần còn lại của số. 
4. Nếu chữ số cuối cùng khác 0 thì n trừ đi một. Điều này tương ứng với mức giảm bình thường trong biểu diễn thập phân. 
5. Nếu chữ số cuối cùng bằng 0, hãy chia n cho 10 bằng phép chia số nguyên. Điều này loại bỏ hoàn toàn chữ số cuối cùng, thu hẹp số chữ số. 
6. Sau khi hoàn thành tất cả k lần lặp, xuất ra giá trị kết quả của n. 

### Tại sao nó hoạt động

Ở mỗi bước, thuật toán phản ánh quy tắc biến đổi chính xác được áp dụng cho trạng thái hiện tại của số. Thao tác chỉ phụ thuộc vào chữ số cuối cùng và sau mỗi lần chuyển đổi, số kết quả sẽ trở thành trạng thái hợp lệ mới cho thao tác tiếp theo. Vì mỗi bước là độc lập và được xác định đầy đủ bởi giá trị hiện tại nên không cần có lịch sử bổ sung. Trình tự các phép biến đổi mang tính xác định, do đó việc mô phỏng chúng theo thứ tự đảm bảo giá trị cuối cùng khớp với quy trình được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())

for _ in range(k):
    if n % 10 == 0:
        n //= 10
    else:
        n -= 1

print(n)
```Giải pháp đọc số ban đầu và lặp lại áp dụng các phép biến đổi k. điều kiện`n % 10 == 0`trực tiếp thực hiện quy tắc về số 0 ở cuối. Phép chia số nguyên`//= 10`loại bỏ chữ số cuối cùng trong trường hợp đó, trong khi phép trừ xử lý tất cả các trường hợp khác. 

Vòng lặp chạy chính xác k lần, đảm bảo tính chính xác mà không cần xử lý quá mức. Vì k nhỏ nên không cần tối ưu hóa thêm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
512 4
```| Bước | Hiện tại | Chữ số cuối cùng | Hoạt động | Tiếp theo | 
| --- | --- | --- | --- | --- | 
| 1 | 512 | 2 | trừ 1 | 511 | 
| 2 | 511 | 1 | trừ 1 | 510 | 
| 3 | 510 | 0 | chia cho 10 | 51 | 
| 4 | 51 | 1 | trừ 1 | 50 | 

Kết quả cuối cùng là 50. 

Dấu vết này cho thấy một số 0 ở cuối gây ra sự thay đổi cấu trúc của số thay vì giảm đơn giản như thế nào, đó là lý do tại sao cần phải có logic dựa trên chữ số. 

### Ví dụ 2 

đầu vào:```
1000 3
```| Bước | Hiện tại | Chữ số cuối cùng | Hoạt động | Tiếp theo | 
| --- | --- | --- | --- | --- | 
| 1 | 1000 | 0 | chia cho 10 | 100 | 
| 2 | 100 | 0 | chia cho 10 | 10 | 
| 3 | 10 | 0 | chia cho 10 | 1 | 

Kết quả cuối cùng là 1 

Ví dụ này nêu bật việc loại bỏ chữ số lặp đi lặp lại, cho thấy số này co lại nhanh như thế nào khi các số 0 ở cuối chiếm ưu thế. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k) | Mỗi thao tác là một lần kiểm tra và cập nhật liên tục được áp dụng k lần | 
| Không gian | O(1) | Chỉ có một biến số nguyên duy nhất được cập nhật tại chỗ | 

Các ràng buộc đảm bảo k ≤ 50, do đó thời gian chạy thực tế là không đổi. Ngay cả trong trường hợp xấu nhất, giải pháp thực hiện tối đa 50 phép tính số học đơn giản, điều này không đáng kể trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    n, k = map(int, sys.stdin.readline().split())
    for _ in range(k):
        if n % 10 == 0:
            n //= 10
        else:
            n -= 1
    print(n)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

# provided sample
assert run("512 4\n") == "50\n", "sample 1"

# minimum case
assert run("10 1\n") == "1\n", "single division"

# all non-zero digits
assert run("999 3\n") == "996\n", "simple decrements"

# multiple trailing zeros
assert run("1000 2\n") == "10\n", "repeated digit removal"

# boundary small number
assert run("2 1\n") == "1\n", "small decrement"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10 1 | 1 | loại bỏ một chữ số | 
| 999 3 | 996 | chuỗi giảm thông thường | 
| 1000 2 | 10 | lặp đi lặp lại không tước | 
| 2 1 | 1 | trường hợp giảm tối thiểu | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi số kết thúc bằng nhiều số 0. Đối với đầu vào`1000`với k = 2, phép toán đầu tiên giảm nó xuống 100 theo phép chia và phép toán thứ hai giảm nó xuống 10 theo phép chia khác. Thuật toán xử lý việc này một cách tự nhiên vì mỗi lần lặp sẽ đánh giá lại chữ số cuối cùng sau khi cấu trúc thay đổi. 

Một trường hợp đặc biệt khác là khi số sớm trở thành một chữ số trong quy trình. Ví dụ: bắt đầu từ 10, thao tác đầu tiên tạo ra 1 và các thao tác tiếp theo tiếp tục trừ hoặc chia một cách an toàn. Vòng lặp không giả định độ dài chữ số cố định, vì vậy nó vẫn chính xác khi số co lại một cách linh hoạt. 

Trường hợp cạnh cuối cùng là khi n ở ngay trên lũy thừa mười ranh giới, chẳng hạn như 1001. Chuỗi phép trừ nhanh chóng tạo ra một số 0 ở cuối, sau đó phép chia xảy ra. Vì mỗi bước luôn kiểm tra trạng thái hiện tại nên quá trình chuyển đổi giữa phép trừ và phép chia luôn được xử lý chính xác mà không cần logic trường hợp đặc biệt.
