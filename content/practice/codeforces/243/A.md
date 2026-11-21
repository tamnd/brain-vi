---
title: "CF 243A - Chức năng hoàn toàn mới"
description: "Chúng ta được cho một dãy số nguyên không âm. Đối với bất kỳ mảng con liền kề nào của chuỗi này, chúng ta có thể tính OR theo bit của nó. Bài toán yêu cầu số lượng giá trị riêng biệt thu được từ tất cả các mảng con như vậy."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks"]
categories: ["algorithms"]
codeforces_contest: 243
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 150 (Div. 1)"
rating: 1600
weight: 243
solve_time_s: 87
verified: true
draft: false
---

[CF 243A - Chức năng hoàn toàn mới](https://codeforces.com/problemset/problem/243/A) 

**Đánh giá:** 1600 
**Thẻ:** bitmask 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên không âm. Đối với bất kỳ mảng con liền kề nào của chuỗi này, chúng ta có thể tính OR theo bit của nó. Bài toán yêu cầu số lượng giá trị riêng biệt thu được từ tất cả các mảng con như vậy. 

Đầu vào cung cấp độ dài chuỗi,$n$, và chính trình tự đó. Đầu ra là một số nguyên duy nhất, đếm các kết quả OR duy nhất cho tất cả các mảng con. Ví dụ, nếu mảng là`[1, 2, 0]`, các mảng con là`[1]`,`[1, 2]`,`[1, 2, 0]`,`[2]`,`[2, 0]`,`[0]`. Tính toán HOẶC cho mỗi sản lượng`[1, 3, 3, 2, 2, 0]`, chứa 4 số phân biệt:`0, 1, 2, 3`. 

Được cho$n$có thể lớn như$10^5$và giá trị có thể đạt tới$10^6$, bất kỳ giải pháp nào tính toán rõ ràng OR cho mỗi mảng con trong một vòng lặp lồng nhau sẽ liên quan đến$O(n^2)$hoạt động quá chậm. Điều này thúc đẩy chúng tôi hướng tới các giải pháp nén kết quả trung gian hoặc khai thác các thuộc tính của OR theo bit. 

Các trường hợp cạnh có thể gây khó khăn cho một giải pháp đơn giản bao gồm các mảng toàn số 0, mảng có các phần tử lặp lại và mảng có các phần tử thêm dần các bit mới. Ví dụ,`[0, 0, 0]`sẽ mang lại chính xác một giá trị riêng biệt,`0`. Cách tiếp cận vòng lặp kép ngây thơ có thể đếm nhầm hoặc xử lý sai các kết quả OR lặp lại. 

## Phương pháp tiếp cận 

Phương pháp brute-force tính toán OR cho mọi mảng con có thể bằng cách sử dụng hai vòng lặp lồng nhau. Vòng lặp bên trong tích lũy kết quả OR của các phần tử từ`l`ĐẾN`r`và thêm nó vào một bộ. Điều này có tác dụng vì OR có tính kết hợp và giao hoán. Tổng số mảng con là$n(n+1)/2$, đại khái$5 \times 10^9$hoạt động cho$n = 10^5$, quá chậm so với giới hạn 2 giây. 

Quan sát quan trọng để tối ưu hóa là nếu chúng ta di chuyển từ trái sang phải, tập hợp các kết quả OR có thể có kết thúc ở mỗi chỉ mục chỉ tăng lên khi các bit mới xuất hiện. Thay vì lưu trữ kết quả OR cho tất cả các mảng con, chúng tôi duy trì một tập hợp các kết quả OR cho các mảng con kết thúc ở phần tử trước đó và tính toán các kết quả mới bằng cách OR-ing từng mảng con với phần tử hiện tại. Chúng tôi cũng bao gồm chính phần tử hiện tại dưới dạng một mảng con mới bắt đầu từ vị trí này. Các bản sao tự nhiên sụp đổ trong tập hợp. Điều này làm giảm đáng kể số lượng thao tác vì mỗi kết quả OR giới thiệu tối đa 20 bit mới (vì các giá trị là ≤$10^6$), giữ kích thước đặt cho mỗi vị trí ở mức nhỏ. Sử dụng tập hợp toàn cục để thu thập tất cả các kết quả sẽ đưa ra câu trả lời cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n²) | Quá chậm | 
| HOẶC tuyên truyền | O(n * B) trong đó B 20 | O(n * B) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một tập hợp trống`all_or`để lưu trữ tất cả các kết quả OR riêng biệt trên các mảng con. 
2. Khởi tạo một tập hợp trống`current_or`để lưu trữ kết quả OR của mảng con kết thúc ở phần tử trước đó. 
3. Lặp lại từng phần tử trong mảng: 

1. Bắt đầu một hiệp mới`next_or`. 
2. Với mỗi giá trị trong`current_or`, tính OR với phần tử hiện tại và thêm nó vào`next_or`. Điều này truyền bá các OR của mảng con trước đó. 
3. Thêm chính phần tử hiện tại vào`next_or`để tính đến các mảng con bắt đầu từ chỉ mục này. 
4. Đặt`current_or = next_or`cho lần lặp tiếp theo. 
5. Hợp nhất`current_or`vào trong`all_or`để tích lũy kết quả toàn cầu. 
4. Sau khi xử lý tất cả các phần tử, kích thước của`all_or`là số kết quả OR khác biệt. 

Tại sao nó hoạt động: Bộ`current_or`luôn chứa tất cả các giá trị OR riêng biệt của các mảng con kết thúc ở vị trí hiện tại. Mọi mảng con có thể đều được xem xét vì chúng tôi mở rộng tất cả các mảng con trước đó và cũng bắt đầu một mảng con mới ở chỉ mục hiện tại. Việc sử dụng các bộ đảm bảo rằng các bản sao được loại bỏ một cách tự động và việc truyền bá đảm bảo không bỏ sót tổ hợp OR nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

all_or = set()
current_or = set()

for num in a:
    next_or = {num}
    for val in current_or:
        next_or.add(val | num)
    current_or = next_or
    all_or.update(current_or)

print(len(all_or))
```Giải pháp bắt đầu bằng cách đọc đầu vào một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`. Chúng tôi duy trì hai bộ,`current_or`cho các mảng con kết thúc ở phần tử hiện tại và`all_or`cho tất cả các kết quả OR duy nhất. Ở mỗi lần lặp, chúng tôi truyền bá các kết quả OR trước đó và bao gồm phần tử mới. Cuối cùng, chúng tôi in kích thước của tập hợp toàn cầu. 

## Ví dụ đã hoạt động 

**Ví dụ 1**:`3`yếu tố`[1, 2, 0]`| tôi | số | current_or (trước) | next_or (sau OR) | all_or (tích lũy) | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | {} | {1} | {1} | 
| 1 | 2 | {1} | {1 | 2=3,2} → {2,3} | 
| 2 | 0 | {2,3} | {2 | 0=2, 3 | 

Số cuối cùng: 4 

**Ví dụ 2**:`4`yếu tố`[1,1,1,1]`| tôi | số | current_or (trước) | next_or (sau OR) | all_or (tích lũy) | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | {} | {1} | {1} | 
| 1 | 1 | {1} | {1 | 1=1,1} → {1} | 
| 2 | 1 | {1} | {1 | 1=1,1} → {1} | 
| 3 | 1 | {1} | {1 | 1=1,1} → {1} | 

Số cuối cùng: 1 

Dấu vết xác nhận rằng các phần tử lặp lại không đưa ra các giá trị OR dư thừa và các phần tử mới sẽ mở rộng đúng các mảng con hiện có. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * B) | Mỗi phần tử mảng có thể tạo tối đa 20 giá trị OR mới (B 20 bit cho số 10⁶) và chúng tôi xử lý n phần tử | 
| Không gian | O(n * B) | Mỗi bộ`current_or`Và`all_or`chứa tối đa n * B phần tử tích lũy | 

Với n 10⁵ và B ≤ 20, tổng số phép tính là khoảng 2×10⁶, nằm trong giới hạn 2 giây. Việc sử dụng bộ nhớ cũng khiêm tốn và vừa vặn thoải mái ở mức 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    all_or = set()
    current_or = set()
    for num in a:
        next_or = {num}
        for val in current_or:
            next_or.add(val | num)
        current_or = next_or
        all_or.update(current_or)
    return str(len(all_or))

# Provided samples
assert run("3\n1 2 0\n") == "4", "sample 1"

# Custom cases
assert run("3\n0 0 0\n") == "1", "all zeros"
assert run("4\n1 1 1 1\n") == "1", "all equal"
assert run("1\n42\n") == "1", "single element"
assert run("5\n1 2 4 8 16\n") == "15", "powers of two"
assert run("5\n1 3 7 15 31\n") == "15", "cumulative ORs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 0`| 1 | Tất cả các số 0 tạo ra một OR | 
|`1 1 1 1`| 1 | Các phần tử lặp lại không tạo ra giá trị OR mới | 
|`42`| 1 | Độ chính xác của mảng phần tử đơn | 
|`1 2 4 8 16`| 15 | OR lũy thừa của hai tạo ra tất cả các kết hợp | 
|`1 3 7 15 31`| 15 | HOẶC sự lan truyền nắm bắt các kết hợp tích lũy | 

## Vỏ cạnh 

Đối với mảng`[0,0,0]`, thuật toán bắt đầu bằng`current_or = {}`. Ở phần tử đầu tiên,`next_or = {0}`, Và`all_or = {0}`. Mỗi phần tử tiếp theo lan truyền`0|0 = 0`, không thay đổi`all_or`. Đầu ra cuối cùng là`1`. Điều này xác nhận rằng các số 0 lặp lại không làm tăng số lượng. 

Vì`[1,1,1,1]`, mỗi bước truyền OR dưới dạng `{
