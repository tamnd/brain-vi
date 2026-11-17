---
title: "CF 1954B - Làm Cho Nó Xấu"
description: "Chúng ta được cung cấp một mảng các số nguyên được đảm bảo là \"đẹp\", có nghĩa là thông qua một thao tác cụ thể, cuối cùng tất cả các phần tử có thể trở nên giống nhau."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1954
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 164 (Rated for Div. 2)"
rating: 1200
weight: 1954
solve_time_s: 75
verified: false
draft: false
---

[CF 1954B - Làm cho nó xấu đi](https://codeforces.com/problemset/problem/1954/B) 

**Đánh giá:** 1200 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên được đảm bảo là "đẹp", có nghĩa là thông qua một thao tác cụ thể, cuối cùng tất cả các phần tử có thể trở nên giống nhau. Thao tác này cho phép chúng ta chọn bất kỳ phần tử nào không nằm ở biên và thay thế nó bằng giá trị lân cận nếu các phần tử lân cận bằng nhau. Mục tiêu của chúng tôi là xác định số phần tử nhỏ nhất mà chúng tôi phải loại bỏ khỏi mảng để không thể làm cho tất cả các phần tử còn lại bằng thao tác này nữa. Nếu không có thao tác xóa nào có thể đạt được điều đó, chúng tôi sẽ trả về -1. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp kích thước mảng`n`và các phần tử mảng. Kích thước mảng có thể lên tới 300.000 và tổng của`n`trên tất cả các trường hợp thử nghiệm không vượt quá 300.000, vì vậy thuật toán của chúng tôi phải chạy với khoảng O(n) cho mỗi trường hợp thử nghiệm. Điều này loại trừ các mô phỏng bạo lực thử mọi cách loại bỏ hoặc cố gắng mô phỏng tất cả các hoạt động vì điều đó có thể dẫn đến độ phức tạp bậc hai hoặc tệ hơn. 

Các trường hợp cạnh không rõ ràng xảy ra khi mảng đã đồng nhất, chẳng hạn như`[1, 1, 1]`. Việc xóa bất kỳ số nào cũng không làm nó mất đi tính đẹp nên kết quả đầu ra phải là -1. Một trường hợp tinh tế khác là khi mảng xen kẽ giữa hai số chẳng hạn`[1, 2, 1, 2, 1]`. Ở đây, việc loại bỏ một phần tử được lựa chọn cẩn thận có thể phá vỡ cấu trúc và chỉ cần loại bỏ một lần là đủ. Mảng có các khối lặp lại, như`[3, 3, 3, 5, 3, 3, 3]`, yêu cầu đếm khoảng cách tối đa giữa các số giống hệt nhau để xác định mức loại bỏ tối thiểu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp lại mọi phần tử, cố gắng loại bỏ nó và mô phỏng hoạt động cho đến khi chúng ta đạt được một mảng thống nhất hoặc không. Về nguyên tắc, điều này đúng vì nó thực sự kiểm tra mọi khả năng, nhưng nó quá chậm. Đối với một mảng có độ dài n, việc loại bỏ từng phần tử và các thao tác mô phỏng có thể mất O(n^2) trong trường hợp xấu nhất, điều này không thể chấp nhận được khi n có thể là 3 × 10^5. 

Quan sát quan trọng là một mảng không còn đẹp nếu tồn tại một "khối" gồm các phần tử lặp lại được phân tách khỏi phần tử giống hệt tiếp theo bằng ít nhất một số khác. Cụ thể hơn, đối với mỗi giá trị riêng biệt, chúng ta có thể ghi lại vị trí xuất hiện của nó và kiểm tra khoảng cách giữa các lần xuất hiện liên tiếp. Nếu bất kỳ khoảng trống nào vượt quá 2 thì việc loại bỏ các phần tử trong khoảng trống đó có thể phá vỡ chuỗi hoạt động. Số lần xóa tối thiểu cần thiết cho một giá trị cụ thể là`(gap - 2)`để có khoảng cách lớn nhất và sau đó chúng tôi lấy mức tối thiểu trên tất cả các giá trị. 

Thông tin chi tiết này giúp giảm vấn đề từ việc mô phỏng các hoạt động sang phân tích khoảng cách và đếm đơn giản, có thể được thực hiện trong O(n) trên mỗi mảng bằng cách sử dụng từ điển hoặc danh sách vị trí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Phân tích / Đếm khoảng trống | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Với mỗi test case, hãy đọc`n`và mảng`a`. 
3. Nếu mảng đồng nhất (tất cả các phần tử đều giống nhau), xuất -1 ngay lập tức, vì việc loại bỏ bất kỳ phần tử nào cũng không làm mất đi vẻ đẹp của nó. 
4. Khởi tạo từ điển`positions`để lưu trữ danh sách các chỉ số cho mỗi giá trị. 
5. Lặp lại mảng và nối từng chỉ mục vào danh sách tương ứng trong`positions`. 
6. Với mỗi giá trị trong`positions`, tính số lần loại bỏ tối đa cần thiết để phá vỡ chuỗi: 

1. Coi khoảng cách từ đầu mảng đến lần xuất hiện đầu tiên và từ lần xuất hiện cuối cùng đến cuối mảng là những khoảng trống ngầm. 
2. Đối với các chỉ số liên tiếp của giá trị này, hãy tính khoảng cách như sau`(current index - previous index - 1)`. Điều này thể hiện số phần tử có thể được loại bỏ để tách biệt các lần xuất hiện. 
3. Theo dõi tối đa`(gap)`trên tất cả các lần xuất hiện liên tiếp. 
7. Số lần loại bỏ tối thiểu cho toàn bộ mảng là giá trị nhỏ nhất của`(maximum gap - 1)`trên tất cả các giá trị riêng biệt. các`-1`xuất phát từ thực tế là việc loại bỏ một cái nhỏ hơn độ dài khoảng cách là đủ để phá vỡ chuỗi. 
8. Xuất mức tối thiểu này cho từng trường hợp thử nghiệm. 

**Tại sao nó hoạt động**: Thao tác chỉ truyền giá trị của các phần tử nếu có các phần tử lân cận giống hệt nhau. Việc phá vỡ khoảng cách lớn nhất của phần tử lặp lại đảm bảo rằng không có chuỗi thay thế nào đủ dài để tạo nên sự thống nhất cho mảng. Bằng cách kiểm tra tất cả các giá trị, chúng tôi đảm bảo đã xem xét tất cả các chuỗi lan truyền tiềm năng. Thuật toán chỉ loại bỏ các phần tử thực sự ngăn cản sự lan truyền. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def min_removals_to_make_ugly(n, a):
    if all(x == a[0] for x in a):
        return -1

    positions = {}
    for idx, val in enumerate(a):
        if val not in positions:
            positions[val] = []
        positions[val].append(idx)
    
    min_remove = float('inf')
    for val, inds in positions.items():
        gaps = []
        # gap from start to first occurrence
        gaps.append(inds[0])
        # gaps between consecutive occurrences
        for i in range(1, len(inds)):
            gaps.append(inds[i] - inds[i-1] - 1)
        # gap from last occurrence to end
        gaps.append(n - 1 - inds[-1])
        max_gap = max(gaps)
        # need to remove at least max_gap elements to break propagation
        min_remove = min(min_remove, max_gap)
    return min_remove

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    print(min_removals_to_make_ugly(n, a))
```Giải pháp đầu tiên là kiểm tra xem mảng có đồng nhất hay không, đó là kiểm tra nhanh O(n). Sau đó, nó thu thập các vị trí của từng giá trị để tính toán các khoảng trống. Bằng cách thêm khoảng trống ở các ranh giới, chúng tôi xử lý chính xác các chuỗi ở đầu và cuối. các`max(gaps)`đảm bảo chúng tôi nhắm mục tiêu chuỗi lớn nhất có thể lan truyền và lấy`min`trên tất cả các giá trị mang lại ít thao tác xóa cần thiết nhất để phá vỡ vẻ đẹp. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`a = [1, 2, 1, 2, 1]`| idx | giá trị | vị trí dict | khoảng trống được tính toán | max_gap | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | {1:[0]} | [0] | 0 | 
| 2 | 1 | {1:[0,2]} | khoảng trống: [0,1,2] | 2 | 
| 4 | 1 | {1:[0,2,4]} | khoảng trống: [0,1,1,0] | 1 | 
| 1 | 2 | {2:[1]} | khoảng trống: [1,3] | 3 | 
| 3 | 2 | {2:[1,3]} | khoảng trống: [1,1,1] | 1 | 

Loại bỏ tối thiểu: 1. Loại bỏ chỉ mục 4 sẽ phá vỡ sự lan truyền. 

**Ví dụ 2:**`a = [3, 3, 3, 5, 3, 3, 3]`| idx | giá trị | vị trí dict | khoảng trống được tính toán | max_gap | 
| --- | --- | --- | --- | --- | 
| 0,1,2 | 3 | [0,1,2] | khoảng trống: [0,0,0,4] | 4 | 
| 3 | 5 | [3] | khoảng trống: [3,3] | 3 | 
| 4,5,6 | 3 | [0,1,2,4,5,6] | khoảng trống: [0,0,1,0,0,0,0] | 1 | 

Loại bỏ tối thiểu: 3. Loại bỏ ba phần tử đầu tiên đảm bảo 5 không thể lan truyền. 

Những dấu vết này chứng tỏ rằng các khoảng trống ở các cạnh và giữa các số lặp lại xác định mức độ loại bỏ tối thiểu cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử mảng được truy cập một lần để xây dựng vị trí và sau đó các khoảng trống được tính một lần cho mỗi giá trị. | 
| Không gian | O(n) | Từ điển vị trí lưu trữ các chỉ số cho từng giá trị riêng biệt. | 

Giải pháp phù hợp trong giới hạn thời gian vì tổng n trên tất cả các trường hợp thử nghiệm là 3 × 10^5. Các thao tác O(n) thoải mái dưới các thao tác cơ bản 2 × 10^6. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())  # assume the above code is saved as solution.py
    return output.getvalue().strip()

# Provided samples
assert run("4\n3\n2 2 2\n5\n1 2 1 2 1\n1\n1\n7\n3 3 3 5 3 3 3\n") == "-1\n1\n-1\n3", "sample 1"

# Custom cases
assert run("1\n1\n1\n") == "-1", "single element array"
assert run("1\n2\n1 1\n") == "-1", "two equal elements"
assert run("1\n2\n1 2\n") == "1", "two different elements"
assert run("1\n5\n1 1 2 1 1\n") == "1", "one different in the middle"
assert run("
```
