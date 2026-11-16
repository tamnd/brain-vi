---
title: "CF 1940B - Ba Mảng"
description: "Chúng ta được cho ba mảng số nguyên. Nhiệm vụ là tìm số bộ ba (i, j, k) sao cho phần tử a[i] của mảng thứ nhất nhỏ hơn hoặc bằng phần tử b[j] của mảng thứ hai và phần tử b[j] của mảng thứ hai nhỏ hơn hoặc bằng phần tử của mảng thứ ba…"
date: "2026-06-08T17:48:16+07:00"
tags: ["codeforces", "competitive-programming", "*special", "constructive-algorithms", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1940
codeforces_index: "B"
codeforces_contest_name: "XVIII Open Olympiad in Informatics - Final Stage, Day 2 (Unrated, Online Mirror, IOI rules)"
rating: 0
weight: 1940
solve_time_s: 83
verified: true
draft: false
---

[CF 1940B - Ba mảng](https://codeforces.com/problemset/problem/1940/B) 

**Đánh giá:** - 
**Thẻ:** *thuật toán đặc biệt, mang tính xây dựng, triển khai, sắp xếp 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho ba mảng số nguyên. Nhiệm vụ là tìm số bộ ba`(i, j, k)`sao cho phần tử của mảng đầu tiên`a[i]`nhỏ hơn hoặc bằng phần tử của mảng thứ hai`b[j]`và phần tử của mảng thứ hai`b[j]`nhỏ hơn hoặc bằng phần tử của mảng thứ ba`c[k]`. Các chỉ số`i`,`j`, Và`k`độc lập; chúng tôi chỉ quan tâm đến các mối quan hệ số. 

Đầu vào cung cấp ba mảng theo thứ tự. Đầu ra là một số nguyên duy nhất, số bộ ba hợp lệ. 

Các ràng buộc là mỗi mảng có thể có tối đa`10^5`các phần tử và giá trị có thể lên tới`10^9`. Điều này ngụ ý rằng một vòng lặp lồng ba mạnh mẽ sẽ yêu cầu tới`10^15`hoạt động là không thể thực hiện được. Bất kỳ giải pháp nào tệ nhất đều phải gần tuyến tính. 

Một điểm tinh tế là các giá trị có thể lặp lại. Ví dụ, nếu`a = [1, 1]`,`b = [1]`,`c = [1, 1]`, tổng số không chỉ đơn giản là số giá trị duy nhất mà là tích của bội số: ở đây hai`1`ở trong`a`mỗi cặp với một`1`TRONG`b`, và đó`b`cặp với hai`1`ở trong`c`, cho`2 * 1 * 2 = 4`bộ ba hợp lệ. Một giải pháp ngây thơ chỉ đếm các số duy nhất sẽ thất bại ở đây. 

Một trường hợp khác là khi một số mảng trống hoặc chứa các phần tử lớn hơn mức tối thiểu của mảng tiếp theo. Ví dụ, nếu`a = [5]`,`b = [3]`,`c = [4]`, không thể có bộ ba và một giải pháp bất cẩn có thể vô tình tính đến điều gì đó. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ là lặp lại từng phần tử trong mảng đầu tiên, đối với mỗi phần tử lặp lại trên tất cả các phần tử trong mảng thứ hai và đối với mỗi phần tử lặp lại trên tất cả các phần tử trong mảng thứ ba. Đối với mỗi sự kết hợp, chúng tôi kiểm tra xem`a[i] <= b[j] <= c[k]`và tăng một bộ đếm nếu vậy. Điều này hoạt động chính xác nhưng đòi hỏi`O(n * m * l)`các hoạt động có thể đạt được`10^15`đối với mảng có kích thước tối đa, vượt xa giới hạn khả thi. 

Thông tin chi tiết quan trọng để tối ưu hóa là các mảng độc lập nhưng chỉ có thứ tự tương đối của chúng mới quan trọng. Nếu chúng ta sắp xếp các mảng thì với mỗi phần tử`b[j]`ở mảng giữa, số lượng`a[i]`thỏa mãn`a[i] <= b[j]`đơn giản là số phần tử trong`a`nhỏ hơn hoặc bằng`b[j]`. Tương tự, số lượng`c[k]`thỏa mãn`b[j] <= c[k]`là số phần tử trong`c`lớn hơn hoặc bằng`b[j]`. Điều này cho phép chúng ta sử dụng tìm kiếm nhị phân để đếm chúng một cách hiệu quả cho mỗi`b[j]`. 

Chúng tôi giảm độ phức tạp từ khối xuống`O(n log n + m log n + m log l + l log l)`bằng cách sắp xếp đầu tiên`a`Và`c`, sau đó lặp đi lặp lại`b`và sử dụng`bisect_right`TRÊN`a`Và`bisect_left`TRÊN`c`. Điều này khả thi đối với`10^5`các phần tử vì sắp xếp là`O(n log n)`và mỗi tìm kiếm nhị phân là`O(log n)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m * l) | O(1) | Quá chậm | 
| Tối ưu | O(n log n + m log n + m log l + l log l) | O(n + m + l) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng`a`theo thứ tự không giảm. Điều này cho phép chúng tôi nhanh chóng đếm các phần tử nhỏ hơn hoặc bằng mục tiêu bằng tìm kiếm nhị phân. 
2. Sắp xếp mảng`c`theo thứ tự không giảm. Điều này cho phép chúng tôi nhanh chóng đếm các phần tử lớn hơn hoặc bằng mục tiêu bằng cách sử dụng tìm kiếm nhị phân. 
3. Khởi tạo bộ đếm`total = 0`để tích lũy các bộ ba hợp lệ. 
4. Lặp qua từng phần tử`b_j`trong mảng`b`. Đối với mỗi`b_j`, tìm số phần tử trong`a`đó là`<= b_j`. sử dụng`bisect_right(a, b_j)`, chúng tôi nhận được chính xác số lượng này. 
5. Tương tự, với mỗi`b_j`, tìm số phần tử trong`c`đó là`>= b_j`. sử dụng`len(c) - bisect_left(c, b_j)`đưa ra số lượng này. 
6. Nhân số đếm từ bước 4 và 5 để có được số bộ ba cho trường hợp cụ thể này`b_j`và thêm nó vào`total`. 
7. Sau khi xử lý xong tất cả`b_j`, in tổng số. 

### Tại sao nó hoạt động 

Việc sắp xếp các mảng đảm bảo rằng tất cả các phần tử nhỏ hơn hoặc bằng một ngưỡng đều liền kề nhau và tất cả các phần tử lớn hơn hoặc bằng một ngưỡng đều liền kề nhau. sử dụng`bisect_right`Và`bisect_left`khai thác tài sản này. Bất biến chính là với mỗi`b_j`, số lượng chúng tôi tính toán là chính xác và độc lập; nhân chúng cho tất cả các kết hợp thỏa mãn các ràng buộc. Không có bộ ba nào bị bỏ sót và không có bộ ba không hợp lệ nào được tính vì tìm kiếm nhị phân đảm bảo việc so sánh giữ đúng từng phần tử. 

## Giải pháp Python```python
import sys
import bisect
input = sys.stdin.readline

def solve():
    n, m, l = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    c = list(map(int, input().split()))

    a.sort()
    c.sort()

    total = 0
    for bj in b:
        count_a = bisect.bisect_right(a, bj)
        count_c = len(c) - bisect.bisect_left(c, bj)
        total += count_a * count_c

    print(total)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên đọc độ dài và giá trị của mảng. Sắp xếp`a`Và`c`đảm bảo rằng`bisect`cuộc gọi hoạt động chính xác. Đối với mỗi`b_j`,`bisect_right(a, bj)`đếm tất cả hợp lệ`a[i]`Và`len(c) - bisect_left(c, bj)`đếm tất cả hợp lệ`c[k]`. Nhân các số đếm này cho tất cả các bộ ba trong đó`b_j`là phần tử ở giữa. các`total`bộ đếm tích lũy câu trả lời cuối cùng. Chi tiết triển khai quan trọng là chọn`bisect_right`vì`a`Và`bisect_left`vì`c`tôn trọng`<=`Và`>=`sự bất bình đẳng. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
3 2 3
1 3 5
2 3
3 4 6
```| b_j | đếm_a (≤ b_j) | đếm_c ( ≥ b_j) | sinh ba cho b_j | 
| --- | --- | --- | --- | 
| 2 | 1 | 3 | 3 | 
| 3 | 2 | 3 | 6 | 

Tổng cộng = 9. 

Ở đây, bảng hiển thị cách mỗi phần tử ở giữa đóng góp nhiều bộ ba bằng cách kết hợp số lượng phần tử hợp lệ ở bên trái và bên phải. 

**Đầu vào mẫu 2**```
2 1 2
1 1
1
1 1
```| b_j | đếm_a | đếm_c | sinh ba | 
| --- | --- | --- | --- | 
| 1 | 2 | 2 | 4 | 

Thuật toán xử lý chính xác các bản sao. Mỗi`a[i]`Và`c[k]`được xem xét độc lập, cho ra 4 bộ ba thay vì chỉ đếm nhầm 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + l log l + m log n + m log l) | Sắp xếp`a`Và`c`và tìm kiếm nhị phân cho mỗi`b_j`| 
| Không gian | O(n + m + l) | Lưu trữ mảng đầu vào | 

Được cho`n, m, l ≤ 10^5`, thuật toán thực hiện xung quanh`10^6`các thao tác sắp xếp và`10^5 * log 10^5 ≈ 5*10^5`hoạt động tìm kiếm, trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io
import bisect

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m, l = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    c = list(map(int, input().split()))

    a.sort()
    c.sort()

    total = 0
    for bj in b:
        count_a = bisect.bisect_right(a, bj)
        count_c = len(c) - bisect.bisect_left(c, bj)
        total += count_a * count_c

    return str(total)

# Provided samples
assert run("3 2 3\n1 3 5\n2 3\n3 4 6\n") == "9", "sample 1"
assert run("2 1 2\n1 1\n1\n1 1\n") == "4", "sample 2"

# Custom test cases
assert run("1 1 1\n1\n1\n1\n") == "1",
```
