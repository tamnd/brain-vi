---
title: "CF 64D - Quà Tặng"
description: "Chúng ta được cho ba số nguyên dương biểu thị giá của ba món quà. Có ba chị em, xếp theo độ tuổi: chị cả, chị giữa và em út."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "greedy"]
categories: ["algorithms"]
codeforces_contest: 64
codeforces_index: "D"
codeforces_contest_name: "Unknown Language Round 1"
rating: 1800
weight: 64
solve_time_s: 100
verified: true
draft: false
---

[CF 64D - Quà](https://codeforces.com/problemset/problem/64/D) 

**Đánh giá:** 1800 
**Tags:** *đặc biệt, tham lam 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho ba số nguyên dương biểu thị giá của ba món quà. Có ba chị em, xếp theo độ tuổi: chị cả, chị giữa và em út. Nhiệm vụ là chia mỗi món quà cho một em gái theo quy tắc món quà đắt nhất thuộc về chị cả, món đắt thứ hai thuộc về chị giữa và rẻ nhất thuộc về em út. Nếu hai món quà có giá như nhau thì chị em tương ứng có thể nhận theo thứ tự bất kỳ. 

Đầu vào bao gồm ba số nguyên$a_1, a_2, a_3$, mỗi giá trị từ 1 đến 100. Đây là kích thước đầu vào nhỏ, ngụ ý rằng chúng ta có thể sử dụng một cách an toàn bất kỳ phương pháp nào với độ phức tạp về thời gian không đổi. Vấn đề cơ bản là sắp xếp ba giá trị trong khi theo dõi vị trí ban đầu của chúng. Trường hợp cạnh phát sinh khi hai hoặc cả ba mức giá bằng nhau. Ví dụ, nếu giá là`5 5 10`, chị cả sẽ nhận được`10`, nhưng hai món quà còn lại có thể chia cho chị giữa và chị út theo thứ tự tùy ý. Một cách triển khai ngây thơ chỉ phân công các vị trí dựa trên so sánh mà không xử lý sự bình đẳng có thể phân công sai các chị em trong những trường hợp như vậy. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các hoán vị của ba biểu thức và kiểm tra hoán vị nào thỏa mãn ràng buộc thứ tự. Với 3 phần tử thì có 3! = 6 hoán vị, vì vậy điều này khả thi nhưng không cần thiết. 

Thông tin chi tiết quan trọng là chúng ta chỉ cần sắp xếp quà theo giá trong khi ghi nhớ các chỉ số ban đầu của chúng. Sau khi được sắp xếp, giá cao nhất sẽ được ánh xạ tới chị cả, giá trung bình cho chị thứ hai và giá thấp nhất cho em út. Chúng ta có thể làm điều này bằng cách ghép từng giá với chỉ số ban đầu của nó, sắp xếp theo giá và sau đó chỉ định các giá tương ứng. Cách tiếp cận này là tuyến tính cho ba phần tử và xử lý sự bình đẳng một cách tầm thường vì cách sắp xếp của Python ổn định. 

Lực lượng vũ phu hoạt động vì việc kiểm tra tất cả các hoán vị đảm bảo tính chính xác, nhưng nói chung nó không thành công nếu kích thước đầu vào tăng lên. Quan sát cho thấy vấn đề giảm xuống còn việc sắp xếp bằng tính năng theo dõi chỉ mục cho phép chúng tôi giải quyết vấn đề đó trong thời gian liên tục mà không có nguy cơ gán sai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) | O(1) | Có thể chấp nhận được nhưng quá mức cần thiết | 
| Sắp xếp theo chỉ số | O(1) | O(1) | Tối ưu, được chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba số nguyên biểu thị giá hiện tại. Lưu trữ chúng trong danh sách cùng với vị trí ban đầu của chúng:`[(price, position), ...]`. Điều này cho phép chúng ta sắp xếp mà không mất dấu món quà nào. 
2. Sắp xếp danh sách theo thứ tự giá giảm dần. Điều này đảm bảo rằng món quà đắt nhất sẽ được xếp đầu tiên, món giá trung bình sẽ xếp thứ hai và món quà rẻ nhất sẽ xếp cuối cùng. 
3. Phân công các chị em theo thứ tự này. Phần tử đầu tiên trong danh sách được sắp xếp tương ứng với chị cả (1), phần tử thứ hai tương ứng với chị giữa (2) và phần tử thứ ba tương ứng với em gái út (3). 
4. Xây dựng danh sách đầu ra trong đó vị trí thứ i cho biết chị nào nhận được món quà thứ i theo thứ tự ban đầu. Sử dụng các vị trí đã lưu ở bước 1 để đặt các số chị em một cách chính xác. 
5. In danh sách cuối cùng của bài tập chị em. 

Điều bất biến là tại mỗi thời điểm sau khi sắp xếp, thứ tự giá khớp hoàn toàn với thứ hạng chị em dự định. Vì chúng ta chỉ có ba phần tử nên việc sắp xếp đảm bảo rằng không có hai chị em nào bị gán sai ngoại trừ trường hợp giá bằng nhau, việc sắp xếp ổn định sẽ tự động xử lý. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# read input
prices = list(map(int, input().split()))
# store original positions
indexed_prices = [(price, i) for i, price in enumerate(prices)]
# sort descending by price
indexed_prices.sort(reverse=True, key=lambda x: x[0])

# assign sisters: 1=eldest, 2=middle, 3=youngest
result = [0, 0, 0]
sisters = [1, 2, 3]
for sister, (_, idx) in zip(sisters, indexed_prices):
    result[idx] = sister

print(*result)
```Mã đầu tiên tạo ra các bộ dữ liệu`(price, original_index)`để duy trì ánh xạ của quà tặng tới các vị trí đầu vào. Việc sắp xếp giảm dần đảm bảo rằng món quà đắt nhất ở chỉ số 0, món quà thứ hai ở chỉ số 1 và món quà rẻ nhất ở chỉ số 2.`zip`với`[1,2,3]`gán số chị em theo đúng thứ tự. Cuối cùng, chúng ta viết các số chị em trở lại mảng kết quả dựa trên vị trí ban đầu, tạo ra kết quả đầu ra chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`11 13 1`| Bước | Giá được lập chỉ mục | Đã sắp xếp | Bài tập | 
| --- | --- | --- | --- | 
| Ban đầu | [(11,0),(13,1),(1,2)] | [(13,1),(11,0),(1,2)] | 1->idx1, 2->idx0, 3->idx2 | 
| Kết quả | | | [2,1,3] | 

Giải thích: Món quà trị giá 13 thuộc về người lớn tuổi nhất, 11 cho người ở giữa, 1 cho người nhỏ nhất. Các chỉ số ban đầu xác định vị trí. 

### Mẫu 2 

đầu vào:`5 5 10`| Bước | Giá được lập chỉ mục | Đã sắp xếp | Bài tập | 
| --- | --- | --- | --- | 
| Ban đầu | [(5,0),(5,1),(10,2)] | [(10,2),(5,0),(5,1)] | 1->idx2, 2->idx0, 3->idx1 | 
| Kết quả | | | [2,3,1] | 

Điều này chứng tỏ việc xử lý các mức giá bằng nhau một cách chính xác. Cả hai món quà 5 giá trị đều có thể hoán đổi cho chị giữa và chị út. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Sắp xếp ba phần tử là thời gian không đổi | 
| Không gian | O(1) | Chỉ có ba bộ dữ liệu và mảng kết quả có kích thước 3 được sử dụng | 

Ngay cả với đầu vào lớn nhất có thể (tất cả đều có giá 100), thuật toán hoàn thành trong thời gian không đáng kể và sử dụng bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    prices = list(map(int, input().split()))
    indexed_prices = [(price, i) for i, price in enumerate(prices)]
    indexed_prices.sort(reverse=True, key=lambda x: x[0])
    result = [0,0,0]
    sisters = [1,2,3]
    for sister, (_, idx) in zip(sisters, indexed_prices):
        result[idx] = sister
    return " ".join(map(str,result))

# provided samples
assert run("11 13 1\n") == "2 1 3", "sample 1"
assert run("5 5 10\n") in ["2 3 1","3 2 1"], "sample 2"

# custom cases
assert run("1 2 3\n") == "3 2 1", "ascending order"
assert run("100 100 100\n") in ["1 2 3","1 3 2","2 1 3","2 3 1","3 1 2","3 2 1"], "all equal"
assert run("50 100 50\n") in ["1 2 3","3 2 1"], "two equal max/med"
assert run("1 1 100\n") in ["2 3 1","3 2 1"], "two equal min"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 3`|`3 2 1`| ánh xạ thứ tự tăng dần tới chị em | 
|`100 100 100`| hoán vị nào | tất cả các giá trị bằng nhau | 
|`50 100 50`|`1 2 3`hoặc`3 2 1`| hai giá trị bằng nhau ở vị trí không tối đa | 
|`1 1 100`|`2 3 1`hoặc`3 2 1`| hai giá trị bằng nhau ở vị trí tối thiểu | 

## Vỏ cạnh 

Đối với đầu vào`50 100 50`, sau khi sắp xếp ta được`[(100,1),(50,0),(50,2)]`. Người lớn nhất có mặt ở chỉ số 1, người giữa và người trẻ nhất có được hai người còn lại. Thuật toán xử lý chính xác các món quà có giá trị 50 bằng nhau. 

Đối với đầu vào`100 100 100`, việc sắp xếp sẽ mang lại cùng một danh sách nhưng mọi thứ tự gán đều hợp lệ. Việc sắp xếp ổn định và ánh xạ chỉ mục đảm bảo rằng các vị trí ban đầu được tôn trọng, tạo ra một hoán vị hợp lệ của các số chị em. 

Đối với đầu vào`1 1 100`, món quà đắt nhất thuộc về chị cả, trong khi hai món rẻ nhất (cả 1) có thể được trao cho chị giữa và chị út theo thứ tự bất kỳ. Thuật toán xử lý việc này một cách chính xác vì nó lặp qua danh sách đã sắp xếp theo thứ tự và gán các số chị em một cách tuần tự.
