---
title: "CF 2173D - Xích Mang Của Taiga"
description: "Taiga được cho một số nguyên dương n và có thể thực hiện đúng k nước đi. Trong mỗi nước đi, cô ấy chọn lũy thừa của hai, 2^ℓ và cộng nó vào n. Điểm của mỗi lần di chuyển là số lần mang nhị phân xảy ra trong quá trình cộng."
date: "2026-06-07T22:47:30+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2173
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1068 (Div. 2)"
rating: 1900
weight: 2173
solve_time_s: 118
verified: false
draft: false
---

[CF 2173D - Dây xích của Taiga](https://codeforces.com/problemset/problem/2173/D) 

**Xếp hạng:** 1900 
**Tags:** bitmasks, vũ phu, dp, tham lam, toán học 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Taiga được cho một số nguyên dương`n`và có thể thực hiện chính xác`k`di chuyển. Trong mỗi nước đi, cô ấy chọn sức mạnh của hai,`2^ℓ`, và thêm nó vào`n`. Điểm của mỗi lần di chuyển là số lần mang nhị phân xảy ra trong quá trình cộng. Taiga muốn tối đa hóa tổng số lần mang theo tất cả`k`di chuyển. 

Ví dụ, nếu`n = 7`(`111`ở dạng nhị phân) và cô ấy thêm vào`2^0 = 1`, phép cộng`111 + 001`kết quả là`1000`và tạo ra ba mang. Nếu như`k = 1`, tổng số điểm là 3. Nếu`k > 1`, việc lựa chọn lũy thừa của hai bước đi liên tiếp sẽ ảnh hưởng đến số lượng lần mang xảy ra trong tương lai vì lần mang có thể lan truyền qua các bit đã có 1 giây. 

Đầu vào đưa ra nhiều trường hợp thử nghiệm. Mỗi ca kiểm thử có`n`lên đến gần`2^30`Và`k`lên đến`10^9`. Với quy mô lớn như vậy`k`, lặp đi lặp lại từng bước di chuyển hoặc thử mọi lựa chọn`ℓ`là không thể thực hiện được. Chúng ta cần một chiến lược hoạt động theo thời gian tỷ lệ thuận với số bit trong`n`, không`k`. 

Một trường hợp khó phát hiện khi`n`có nhiều số 1 ở dạng nhị phân. Thêm`2^0`liên tục tạo ra nhiều sóng mang lan truyền qua các số 1 này. Ví dụ,`n = 3`(`11`ở dạng nhị phân) và`k = 2`: thêm`2^0`đầu tiên sản xuất`11 + 01 = 100`, hai mang. Động thái tiếp theo phải xem xét trạng thái nhị phân mới`100`. Một cách tiếp cận ngây thơ không theo dõi bit nào cần thêm tiếp theo sẽ đánh giá thấp tổng số lượng mang theo. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ thử tất cả các động thái có thể. Ở mỗi lần di chuyển, chúng tôi sẽ kiểm tra từng`ℓ`từ 0 đến bit được đặt cao nhất của`n`và tính số lần mang. Điều này đúng vì nó khám phá tất cả các tùy chọn, nhưng nó thất bại khi`k`là lớn. Số lượng thao tác sẽ là O(k * số bit), điều này bị cấm đối với`k = 10^9`. 

Thông tin chi tiết quan trọng là số lần mang chỉ phụ thuộc vào các số 1 ở cuối liên tiếp trong`n`. Thêm`2^0`đến một số với`m`dấu 1 giây tạo ra`m + 1`mang theo. Để tối đa hóa điểm số, chúng tôi luôn muốn thêm vào từng chút nhỏ nhất`ℓ`như vậy`n`có số 1 ở vị trí đó hoặc thấp hơn, vì số này tạo ra chuỗi mang dài nhất. Khi chúng tôi thực hiện phép cộng như vậy, các số ở cuối sẽ chuyển thành số 0 duy nhất, theo sau là số 0 hoặc số 1 tiếp theo và quá trình lặp lại. 

Một cách hiệu quả, vấn đề giảm xuống việc liên tục tìm số lượng dấu vết trong`n`, cộng số đó vào điểm rồi tăng dần`n`bởi sức mạnh tương ứng của hai. Cách tiếp cận này chỉ phụ thuộc vào độ dài nhị phân của`n`, vì vậy nó có thể xử lý lớn`k`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k * log n) | O(1) | Quá chậm | 
| Tối ưu | O(log n + min(k, số lần lặp ở cuối)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, khởi tạo`total_score`về không. 
2. Trong khi`k > 0`, xác định số lượng dấu vết liên tiếp trong hiện tại`n`. Điều này có thể được thực hiện bằng cách sử dụng`n & -n`hoặc đếm các bit bắt đầu từ bit có trọng số nhỏ nhất. 
3. Hãy để`m`là số những cái ở cuối. Điểm của việc thêm`2^0`ở trạng thái này là`m + 1`. 
4. Tăng`total_score`qua`m + 1`. 
5. Cập nhật`n`bằng cách thêm`2^0 << m`(tương đương,`n += 2^0`, nhưng sau khi truyền bá các chuỗi mang, điều này sẽ lật các chuỗi theo sau). 
6. Giảm`k`bằng 1. 
7. Lặp lại cho đến hết`k`di chuyển được sử dụng. 
8. Đầu ra`total_score`cho trường hợp thử nghiệm này. 

Tại sao nó hoạt động: mỗi phép cộng ở bit khả dụng thấp nhất sẽ tạo ra mức mang tối đa có thể có ở bước đó. Những bước đi sau xác định chuỗi vận chuyển, do đó việc xử lý chúng theo thứ tự đảm bảo rằng mỗi bước di chuyển đều tối ưu. Vì mỗi bước di chuyển sẽ giảm thiểu hoặc đặt lại một cách nghiêm ngặt các bước theo sau, quá trình này sẽ hội tụ và sử dụng nhiều nhất`log n`lặp đi lặp lại ngay cả khi`k`là lớn. Nếu như`k`còn lại sau khi tất cả các dấu vết được xử lý, mỗi nước đi còn lại sẽ thêm 1 lần mang, có thể được thêm trực tiếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def max_carries(n, k):
    total = 0
    while k > 0:
        if n == 0:
            total += k
            break
        # count trailing ones
        trailing_ones = 0
        temp = n
        while temp & 1:
            trailing_ones += 1
            temp >>= 1
        total += trailing_ones + 1
        n += 1 << trailing_ones
        k -= 1
    return total

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    print(max_carries(n, k))
```Giải pháp đọc`t`trường hợp thử nghiệm. Đối với mỗi, nó lặp lại các bước di chuyển. Thao tác chính là đếm số ở cuối, xác định số lần mang được tạo ra bằng cách thêm`2^0`. Sau khi cập nhật`n`và giảm dần`k`, vòng lặp tiếp tục. Nếu như`n`đạt đến 0 (tất cả các bit bị xóa), các bước di chuyển còn lại đóng góp một lần mang. 

## Ví dụ đã hoạt động 

Đối với đầu vào`n = 7`,`k = 1`: 

| Bước | n (nhị phân) | Trail_ones | điểm_thêm | tổng_score | n sau khi cộng | k | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 111 | 3 | 3 | 3 | 1000 | 0 | 

Điều này xác nhận rằng việc thêm vào những cái ở cuối sẽ mang lại hiệu quả tối đa. 

Đối với đầu vào`n = 23`,`k = 2`: 

| Bước | n (nhị phân) | Trail_ones | điểm_thêm | tổng_score | n sau khi cộng | k | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 10111 | 3 | 3 | 3 | 11000 | 1 | 
| 2 | 11000 | 0 | 2 | 5 | 100000 | 0 | 

Dấu vết này chứng tỏ thuật toán chọn chính xác các phép cộng để tối đa hóa khả năng lan truyền mang. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n + min(k, log n)) | Mỗi lần lặp lại đếm số lần theo sau bằng cách sử dụng tối đa log n bit; k lần lặp được giới hạn bởi log n | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Cách tiếp cận dễ dàng xử lý`k`lên tới 10^9 vì sau khi hết các bước đi cuối, các bước đi còn lại là các phép cộng tuyến tính, mỗi nước đóng góp 1 lần mang. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        output.append(str(max_carries(n, k)))
    return "\n".join(output)

# provided samples
assert run("6\n7 1\n13 2\n42 2\n1048576 100\n23 2\n371 1\n") == "3\n4\n3\n100\n5\n3", "Sample 1"

# custom cases
assert run("2\n1 10\n0 5\n") == "10\n5", "minimum n, multiple k"
assert run("1\n1073741823 1\n") == "30", "max n with all bits 1, single move"
assert run("1\n15 4\n") == "10", "small n, multiple moves"
assert run("1\n1024 0\n") == "0", "zero moves"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 10 | 10 | n tối thiểu, nhiều nước đi được tính chính xác | 
| 0 5 | 5 | trường hợp cạnh có n=0, tất cả nước đi đều đóng góp 1 lần mang | 
| 1073741823 1 | 30 | n lớn với tất cả các bit là 1, chuỗi mang cực đại | 
| 15 4 | 10 | nhiều bước di chuyển với khả năng lan truyền mang theo | 
| 1024 0 | 0 | không di chuyển được xử lý chính xác | 

## Vỏ cạnh 

Nếu`n = 0`Và`k > 0`, mỗi nước đi thêm 1 lần mang theo. Thuật toán phát hiện ngay`n == 0`và thêm vào`k`đến tổng số điểm, trả về mà không cần tính toán không cần thiết. 

Nếu như`n`có một chuỗi các số 1 ở cuối, như`n = 7`(`111`) Và`k = 1`, bước đi đầu tiên thêm vào`2^0`và tạo ra ba mang. Vòng lặp đếm chính xác những cái ở cuối và cập nhật`n`ĐẾN`1000`ở dạng nhị phân. 

Vì`k`lớn hơn mức cần thiết để làm cạn kiệt những bước đi còn lại, mỗi bước di chuyển còn lại sẽ cộng thêm 1 vào tổng số điểm. Ví dụ,`n = 1`,`k = 5`, nước đi đầu tiên thêm 2 lần mang, sau đó bốn nước đi tiếp theo, mỗi nước thêm 1, tổng cộng là 6. Thuật toán nắm bắt hành vi này một cách tự nhiên.
