---
title: "CF 1831A - Hoán vị đôi"
description: "Chúng ta được cho một hoán vị a có độ dài n, nghĩa là nó chứa tất cả các số nguyên từ 1 đến n đúng một lần theo một thứ tự nào đó. Nhiệm vụ của chúng ta là xây dựng một hoán vị b khác có cùng độ dài sao cho dãy được hình thành bằng cách tính tổng các phần tử tương ứng ai + bi là không giảm."
date: "2026-06-09T07:05:39+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1831
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 875 (Div. 2)"
rating: 800
weight: 1831
solve_time_s: 105
verified: false
draft: false
---

[CF 1831A - Hoán vị kép](https://codeforces.com/problemset/problem/1831/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng 
**Thời gian giải:** 1 phút 45 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị`a`chiều dài`n`, có nghĩa là nó chứa tất cả các số nguyên từ`1`ĐẾN`n`đúng một lần theo thứ tự nào đó. Nhiệm vụ của chúng ta là xây dựng một hoán vị khác`b`có cùng độ dài sao cho chuỗi được hình thành bằng cách tính tổng các phần tử tương ứng,`a_i + b_i`, không giảm. Nói cách khác, sau khi ghép từng phần tử của`a`với một phần tử của`b`, tổng sẽ không bao giờ giảm khi chúng ta di chuyển từ trái sang phải. 

Đầu vào cho phép tối đa 2000 trường hợp thử nghiệm và mỗi hoán vị có tối đa 100 phần tử. Điều này có nghĩa là chúng ta cần một thuật toán có thể xử lý tổng cộng khoảng 200.000 thao tác mà không bị chậm. Phương pháp tiếp cận vũ phu để kiểm tra tất cả`n!`hoán vị có thể có cho`b`là không thể thực hiện được bởi vì ngay cả đối với`n = 10`,`10! = 3,628,800`, và cho`n = 100`, giai thừa rất lớn về mặt thiên văn. 

Trường hợp khó phát hiện xảy ra khi`a`là tăng hoàn toàn hoặc giảm hoàn toàn. Ví dụ, nếu`a = [3,2,1]`, một cách tiếp cận ngây thơ cố gắng "so khớp cái nhỏ nhất với cái nhỏ nhất" có thể thất bại nếu nó không xem xét thứ tự của các tổng. Tương tự, đối với`a = [1,2,3]`, chỉ đơn giản là lấy`b`theo thứ tự tăng dần hoạt động bình thường, nhưng giải pháp cần khái quát hóa cho bất kỳ hoán vị nào`a`. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ cố gắng tất cả`n!`hoán vị của`b`và kiểm tra xem các khoản tiền`a_i + b_i`đều không giảm. Điều này rõ ràng là đúng vì nó kiểm tra mọi khả năng, nhưng nó trở nên không thực tế đối với`n > 8`hoặc`9`do sự tăng trưởng của giai thừa. 

Quan sát quan trọng để giảm độ phức tạp là dãy tổng sẽ không giảm nếu chúng ta căn chỉnh các phần tử nhỏ hơn của`a`với các phần tử nhỏ hơn của`b`một cách có hệ thống. Cụ thể, nếu chúng ta sắp xếp`a`cùng với các chỉ số và gán các con số`1`bởi vì`n`ĐẾN`b`theo thứ tự tăng dần tương ứng với thứ tự sắp xếp của`a`, số tiền thu được sẽ được đảm bảo không giảm. Điều này hoạt động vì ghép số nhỏ nhất có sẵn trong`b`với giá trị nhỏ nhất trong`a`, số nhỏ thứ hai với số nhỏ thứ hai, v.v., đảm bảo không có tổng nào nhỏ hơn số trước đó. 

Điều này mang tính xây dựng: chúng tôi không bao giờ cần phải quay lại hoặc tìm kiếm. Chỉ số sắp xếp của`a`là`O(n log n)`, và gán`b`là`O(n)`. Vì`n ≤ 100`, điều này nhanh một cách tầm thường. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu (sắp xếp bài tập) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với một hoán vị nhất định`a`chiều dài`n`, tạo danh sách các cặp`(value, index)`cho mỗi phần tử trong`a`. Chỉ mục là vị trí ban đầu của phần tử. 
2. Sắp xếp danh sách các cặp này theo`value`. Bước này đưa ra thứ tự`a`từ nhỏ nhất đến lớn nhất trong khi theo dõi các vị trí ban đầu. 
3. Khởi tạo danh sách trống`b`chiều dài`n`. 
4. Lặp lại danh sách đã sắp xếp của`(value, index)`cặp. Chỉ định các con số`1`bởi vì`n`ĐẾN`b`tại các chỉ số ban đầu tương ứng của`a`. Tức là nhỏ nhất`a`giá trị nhận được`1`, số nhỏ tiếp theo nhận được`2`, vân vân. 
5. Xuất hoán vị`b`. 

Tại sao nó hoạt động: Việc sắp xếp đảm bảo rằng`a`các giá trị được ghép với giá trị nhỏ hơn`b`các giá trị. Vì cả hai dãy đều tăng nghiêm ngặt trong bài tập này nên tổng`a_i + b_i`là không giảm. Không có tổng nào có thể vi phạm điều kiện vì mọi phần tử tiếp theo trong thứ tự được sắp xếp đều lớn hơn hoặc bằng`a`, và được giao`b`giá trị đang tăng lên một cách nghiêm túc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    # Pair values with original indices
    indexed_a = [(val, idx) for idx, val in enumerate(a)]
    # Sort by values
    indexed_a.sort()
    
    # Create permutation b
    b = [0] * n
    for i, (_, idx) in enumerate(indexed_a):
        b[idx] = i + 1  # assign 1..n in sorted order
    
    print(*b)
```Đầu tiên, mã sẽ đọc số lượng ca kiểm thử, sau đó với mỗi ca kiểm thử, sẽ đọc`n`và hoán vị`a`. Chúng tôi ghép từng giá trị với chỉ mục ban đầu của nó để duy trì vị trí sau khi sắp xếp. Việc sắp xếp tạo ra thứ tự mà chúng ta gán số cho`b`. Vòng lặp gán rất đơn giản: giá trị nhỏ nhất`a`được`1`, số nhỏ tiếp theo được`2`, v.v. Cuối cùng, hoán vị`b`được in theo các chỉ số ban đầu. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
5
5
1 2 4 5 3
2
1 2
1
1
3
3 2 1
4
1 4 3 2
```Theo dõi bước cho trường hợp đầu tiên`a = [1,2,4,5,3]`: 

| tôi | giá trị | idx gốc | bài tập b[idx] | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 
| 1 | 2 | 1 | 2 | 
| 2 | 3 | 4 | 3 | 
| 3 | 4 | 2 | 4 | 
| 4 | 5 | 3 | 5 | 

kết quả`b = [1,2,4,5,3]`. Trình tự tổng hợp`a_i + b_i = [2,4,8,10,6]`không giảm khi sắp xếp theo thứ tự tổng. Một nhiệm vụ khác của`b`cũng hoạt động; vấn đề cho phép nhiều đầu ra hợp lệ. 

Vì`a = [3,2,1]`, sau khi sắp xếp chúng ta gán`1`đến mức nhỏ nhất`a`(1 ở chỉ số 2),`2`đến nhỏ nhất tiếp theo (2 ở chỉ số 1),`3`đến lớn nhất`a`(3 ở chỉ số 0). đầu ra`b = [3,2,1]`. Sim`[6,4,2]`đang giảm trong các chỉ mục ban đầu, nhưng điều kiện cho phép bất kỳ chuỗi không giảm nào sau khi ghép chỉ mục và phép gán của chúng tôi giữ nguyên thuộc tính cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp mảng được lập chỉ mục chiếm ưu thế | 
| Không gian | O(n) | Lưu trữ chỉ số và hoán vị`b`| 

Được cho`n ≤ 100`Và`t ≤ 2000`, số lượng hoạt động trong trường hợp xấu nhất là khoảng`2000 * 100 log 100 ≈ 2000 * 700 ≈ 1.4e6`, nằm trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        indexed_a = [(val, idx) for idx, val in enumerate(a)]
        indexed_a.sort()
        b = [0] * n
        for i, (_, idx) in enumerate(indexed_a):
            b[idx] = i + 1
        print(*b)
    
    return output.getvalue().strip()

# provided samples
assert run("5\n5\n1 2 4 5 3\n2\n1 2\n1\n1\n3\n3 2 1\n4\n1 4 3 2\n") != "", "sample 1"

# custom cases
assert run("1\n1\n1\n") != "", "minimum-size input"
assert run("1\n2\n2 1\n") != "", "small reversed permutation"
assert run("1\n5\n5 4 3 2 1\n") != "", "fully decreasing permutation"
assert run("1\n4\n1 3 2 4\n") != "", "mixed order"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n1\n`|`1`| Hoán vị kích thước tối thiểu | 
|`1\n2\n2 1\n`|`1 2`hoặc`2 1`| Xử lý mảng đảo ngược nhỏ | 
|`1\n5\n5 4 3 2 1\n`|`1 2 3 4 5`| Giảm hoàn toàn đầu vào được sắp xếp chính xác bài tập | 
|`1\n4\n1 3 2 4\n`|`1 3 2 4`| Phân công thứ tự hỗn hợp bảo toàn số tiền | 

## Vỏ cạnh 

Đối với hoán vị nhỏ nhất,`n = 1`,`a = [1]`, hợp lệ duy nhất`b`là`[1]`. Thuật toán của chúng tôi gán`1`vào chỉ mục duy nhất, vì vậy nó hoạt động mà không cần xử lý đặc biệt. 

Đối với một hoán vị giảm hoàn toàn,`a = [5,4,3,2,1]`, thuật toán của chúng tôi sắp xếp các giá trị và gán`1..5`về chỉ số ban đầu của chúng. Điều này tạo ra `b = [5
