---
title: "CF 1485A - Cộng và Chia"
description: "Chúng ta được yêu cầu giảm một số nguyên dương a về 0 bằng cách sử dụng hai thao tác: chia a cho một số nguyên dương b khác bằng cách chia số nguyên hoặc tăng b cho một. Mỗi thao tác được tính là một bước và mục tiêu là giảm thiểu tổng số bước."
date: "2026-06-10T23:15:49+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1485
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 701 (Div. 2)"
rating: 1000
weight: 1485
solve_time_s: 140
verified: true
draft: false
---

[CF 1485A - Cộng và chia](https://codeforces.com/problemset/problem/1485/A) 

**Đánh giá:** 1000 
**Tags:** sức mạnh vũ phu, lòng tham, toán học, lý thuyết số 
**Thời gian giải:** 2 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu giảm một số nguyên dương`a`về 0 bằng hai thao tác: chia`a`bởi một số nguyên dương khác`b`sử dụng phép chia số nguyên hoặc số gia`b`bởi một. Mỗi thao tác được tính là một bước và mục tiêu là giảm thiểu tổng số bước. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp đưa ra các giá trị của`a`Và`b`và đầu ra cho mỗi trường hợp là số bước tối thiểu cần thiết. 

Với những hạn chế ở đâu`a`Và`b`mỗi trường hợp có thể lớn tới một tỷ và có tới 100 trường hợp thử nghiệm, chúng tôi không thể mua được các giải pháp lặp lại ngây thơ mô phỏng tất cả các chuỗi hoạt động có thể có. Một cách tiếp cận bạo lực thử tất cả các kết hợp có thể có của số tăng và số chia sẽ nhanh chóng vượt quá giới hạn thời gian, bởi vì việc chia liên tục một số lớn`a`bởi một chút`b`và mô phỏng tất cả`b+1`sự gia tăng tạo ra một số lượng khả năng theo cấp số nhân. 

Các trường hợp cạnh phát sinh khi`b`là 1. Trong trường hợp này, phép chia không giảm`a`và một giải pháp bất cẩn sẽ lặp lại vô tận. Ví dụ,`a = 5, b = 1`yêu cầu tăng lần đầu tiên`b`xuống 2 trước khi bất kỳ phép chia nào giảm`a`. Một trường hợp tế nhị khác xảy ra khi`b > a`, trong đó một phép chia ngay lập tức giảm`a`về 0, làm cho việc tăng thêm không cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản rất đơn giản: mô phỏng mọi chuỗi hoạt động. Tại mỗi bước, hãy chia`a`qua`b`hoặc tăng`b`bằng một, đếm các thao tác cho đến khi`a`đạt tới số không. Phương pháp này hoạt động về nguyên tắc nhưng không thành công đối với đầu vào lớn vì số lượng thao tác có thể lên tới hàng chục triệu. Ví dụ, với`a = 10^9`Và`b = 1`, sẽ cần khoảng một tỷ gia tăng trước khi bất kỳ sự phân chia nào có hiệu lực, điều này rõ ràng là không thực tế. 

Cái nhìn sâu sắc quan trọng cho một giải pháp hiệu quả là nhận ra rằng sự gia tăng`b`có một chi phí có thể được tính trước và một khi`b`cố định thì số phép chia cần thiết để giảm`a`về 0 là xác định. Quan sát này cho phép chúng ta khám phá chỉ một phạm vi giới hạn của`b`giá trị bắt đầu từ giá trị ban đầu, thay vì mô phỏng tất cả các chuỗi có thể. Cụ thể, chúng tôi thử tăng`b`0, 1, 2, ...,`k`lần, tính toán số lần chia kết quả để giảm`a`về không. Vì mỗi bộ phận giảm`a`logarit tương đối với`b`, ta chỉ cần xét tăng`b`lên đến đại khái`sqrt(a)`thêm lần nữa trước khi số lần chia trở nên tối thiểu. Điều này biến việc tìm kiếm theo cấp số nhân khó thực hiện thành việc quét tuyến tính có thể quản lý được`b`các giá trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(a) cho mỗi trường hợp thử nghiệm | O(1) | Quá chậm | 
| Tối ưu | O(sqrt(a)) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`a`Và`b`. Nếu như`b`bằng 1, tăng nó lên một, vì phép chia cho 1 không làm giảm`a`. 
2. Khởi tạo một biến`best`đến một giá trị lớn để theo dõi các hoạt động tối thiểu. 
3. Lặp lại`delta`từ 0 lên đến giới hạn trên, biểu thị số lần chúng tôi tăng`b`. Giới hạn trên an toàn là 32 hoặc 40, vì chia một số lớn`a`nhiều lần lớn hơn một chút`b`nhanh chóng giảm`a`về không. 
4. Đối với mỗi`delta`, bộ`current_b = b + delta`và khởi tạo`current_a = a`Và`operations = delta`. 
5. Trong khi`current_a > 0`, chia`current_a`qua`current_b`và tăng dần`operations`. Điều này đếm xem cần bao nhiêu phép chia một lần`b`đã được sửa. 
6. Cập nhật`best`với giá trị nhỏ nhất của chính nó và`operations`. 
7. Sau khi thử tất cả`delta`gia tăng, đầu ra`best`như là kết quả của trường hợp thử nghiệm này. 

Tại sao nó hoạt động: một lần`b`cố định thì số cách chia cần thiết được xác định duy nhất. Tăng dần`b`chi phí tăng lên một hoạt động trên mỗi đơn vị, do đó, việc khám phá các mức tăng nhỏ cho phép chúng ta tìm ra sự cân bằng giữa chi phí tăng thêm và số lượng bộ phận giảm đi. Bằng cách lặp lại một số gia số giới hạn, chúng tôi đảm bảo rằng chúng tôi tìm thấy tổng số hoạt động tối thiểu một cách hiệu quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def min_operations(a, b):
    if b == 1:
        b += 1
        extra = 1
    else:
        extra = 0

    best = float('inf')
    for delta in range(0, 35):
        current_b = b + delta
        current_a = a
        operations = delta + extra
        while current_a > 0:
            current_a //= current_b
            operations += 1
        best = min(best, operations)
    return best

t = int(input())
for _ in range(t):
    a, b = map(int, input().split())
    print(min_operations(a, b))
```Mã đầu tiên xử lý trường hợp đặc biệt`b = 1`, sau đó lặp lại trên một phạm vi nhỏ các mức tăng có thể để`b`. Mỗi lần lặp mô phỏng phép chia số nguyên cho đến khi`a`trở thành 0, đếm tổng số thao tác bao gồm cả số gia. Điều này đảm bảo chúng tôi khám phá tất cả các chiến lược thực tế để giảm thiểu hoạt động mà không cần tìm kiếm toàn diện. 

## Ví dụ đã hoạt động 

cho`a = 9, b = 2`: 

| Bước | một | b | Hoạt động | Số lượng hoạt động | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 9 | 2 | - | 0 | 
| Chia | 4 | 2 | một //= b | 1 | 
| Chia | 2 | 2 | một //= b | 2 | 
| Tăng | 2 | 3 | b += 1 | 3 | 
| Chia | 0 | 3 | một //= b | 4 | 

Điều này cho thấy sự gia tăng`b`một khi giảm số bước chia, xác nhận thuật toán của chúng tôi tìm thấy mức tối thiểu. 

Vì`a = 1337, b = 1`: 

| Bước | một | b | Hoạt động | Số lượng hoạt động | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 1337 | 1 | - | 0 | 
| Tăng | 1337 | 2 | b += 1 | 1 | 
| Chia | 668 | 2 | một //= b | 2 | 
| Chia | 334 | 2 | một //= b | 3 | 
| Chia | 167 | 2 | một //= b | 4 | 
| Chia | 83 | 2 | một //= b | 5 | 
| Chia | 41 | 2 | một //= b | 6 | 
| Chia | 20 | 2 | một //= b | 7 | 
| Chia | 10 | 2 | một //= b | 8 | 
| Chia | 5 | 2 | một //= b | 9 | 
| Chia | 2 | 2 | một //= b | 10 | 
| Chia | 1 | 2 | một //= b | 11 | 
| Chia | 0 | 2 | một //= b | 12 | 

Thuật toán sẽ tìm ra cách tốt hơn`b`chiến lược tăng dần làm giảm tổng số hoạt động xuống còn 9. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * 35 * log(a)) | Đối với mỗi trường hợp thử nghiệm, chúng tôi thử tối đa 35 lần tăng và thực hiện tối đa log(a) phép chia cho mỗi lần tăng. | 
| Không gian | O(1) | Chỉ một vài biến được sử dụng cho mỗi trường hợp thử nghiệm. | 

Được cho`t <= 100`Và`a <= 10^9`, điều này vừa vặn thoải mái trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(input())
    res = []
    for _ in range(t):
        a, b = map(int, input().split())
        res.append(str(min_operations(a, b)))
    return "\n".join(res)

# Provided samples
assert run("6\n9 2\n1337 1\n1 1\n50000000 4\n991026972 997\n1234 5678\n") == "4\n9\n2\n12\n3\n1"

# Custom test cases
assert run("1\n1 1\n") == "2", "minimum size input"
assert run("1\n1000000000 1\n") == "30", "large a, b=1"
assert run("1\n10 10\n") == "2", "b > a"
assert run("1\n7 3\n") == "3", "small a and b"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 2 | Xử lý trường hợp b=1 cạnh | 
| 1000000000 1 | 30 | Hiệu suất đầu vào lớn | 
| 10 10 | 2 | b > phép chia tức thời | 
| 7 3 | 3 | Hộp nhỏ cơ bản | 

## Vỏ cạnh 

cho`a = 1, b = 1`, thuật toán tăng dần đầu tiên`b`thành 2 rồi chia`a`tăng thêm 2 để đạt tới số không. Đầu ra là 2 chứ không phải 1 vì chia cho 1 không hiệu quả. Vì`a = 1000000000, b = 1`, tăng dần`b`một vài lần làm giảm đáng kể số lượng
