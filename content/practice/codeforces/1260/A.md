---
title: "CF 1260A - Sưởi ấm"
description: "Chúng tôi được cấp một ngôi nhà có n phòng. Đối với mỗi phòng, chúng ta biết hai số: ci, số lượng bộ tản nhiệt tối đa mà chúng ta có thể lắp đặt và sumi, tổng số bộ phận tản nhiệt cần thiết để sưởi ấm căn phòng đó một cách đầy đủ."
date: "2026-06-11T20:43:04+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1260
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 77 (Rated for Div. 2)"
rating: 1000
weight: 1260
solve_time_s: 142
verified: true
draft: false
---

[CF 1260A - Hệ thống sưởi](https://codeforces.com/problemset/problem/1260/A) 

**Đánh giá:** 1000 
**Thẻ:** toán 
**Thời gian giải:** 2m 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một ngôi nhà với`n`phòng. Với mỗi phòng, chúng ta biết hai số:`c_i`, số lượng bộ tản nhiệt tối đa mà chúng tôi có thể lắp đặt, và`sum_i`, tổng số bộ phận tản nhiệt cần thiết để sưởi ấm căn phòng đó một cách thỏa đáng. Mỗi bộ tản nhiệt có thể có số phần tùy ý và giá của một bộ tản nhiệt có`k`phần là`k^2`. Mục tiêu của chúng tôi là chỉ định các phần cho bộ tản nhiệt sao cho tổng số phần trong phòng ít nhất là`sum_i`, chúng tôi không vượt quá`c_i`bộ tản nhiệt và tổng chi phí được giảm thiểu. 

Những hạn chế chính là`n`có thể lên tới 1000 và mỗi phòng có thể cần tới 10.000 phần với tối đa 10.000 bộ tản nhiệt. Nó đủ nhỏ để cho phép`O(n)`hoạt động cho mỗi phòng, nhưng bất cứ điều gì bậc hai trong`sum_i`hoặc`c_i`có thể đạt tới 10^8 thao tác, quá chậm. Trường hợp cạnh xảy ra khi`c_i`nhỏ hơn nhiều so với`sum_i`hoặc lớn hơn nhiều. Ví dụ: nếu một căn phòng yêu cầu 100 phần nhưng chúng ta chỉ có thể đặt 1 bộ tản nhiệt thì chúng ta phải đặt tất cả 100 phần vào bộ tản nhiệt đó. Ngược lại, nếu một căn phòng cần 1 phần và chúng ta có thể đặt 10.000 bộ tản nhiệt thì nên đặt 1 phần trong một bộ tản nhiệt. 

Việc triển khai bất cẩn có thể cố gắng lặp lại tất cả các phân bổ có thể có của các phần trên bộ bức xạ. Ví dụ, đối với`c_i = 2`Và`sum_i = 6`, liệt kê`[6,0],[5,1],[4,2]…`hoạt động với số lượng nhỏ, nhưng nếu`sum_i`Và`c_i`là 10.000, cách tiếp cận này sẽ chậm một cách vô vọng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực ngây thơ sẽ thử mọi sự kết hợp có thể của`k`bộ tản nhiệt và phân công các phần theo mọi cách có thể. Điều này đúng vì nó bao gồm mọi phân bổ hợp lệ, nhưng số lượng kết hợp tăng theo cấp số nhân. Vì`c_i = 10,000`Và`sum_i = 10,000`, thậm chí việc liệt kê các phân vùng 10.000 thành 10.000 phần là không thể. Điều này chứng tỏ rằng vũ lực là không thể thực hiện được. 

Quan sát quan trọng là hàm chi phí`k^2`là lồi. Độ lồi có nghĩa là tổng bình phương được giảm thiểu khi các giá trị bằng nhau nhất có thể. Nói cách khác, đối với một số lượng bộ tản nhiệt nhất định, chi phí tối thiểu sẽ xảy ra khi chúng ta phân bổ đều các phần, chỉ định`floor(sum_i/c_i)`đến từng bộ tản nhiệt và lan rộng phần còn lại (`sum_i % c_i`) trên một số bộ tản nhiệt bằng cách thêm một phần bổ sung. Đây là một thủ thuật cổ điển có hàm lồi: để giảm thiểu tổng bình phương có tổng cố định, các số phải cân bằng nhất có thể. 

Khi chúng tôi biết số lượng bộ tản nhiệt cần sử dụng, việc tính toán chi phí rất đơn giản. Chúng tôi tính toán`x = sum_i // c_i`là số phần cơ bản trên mỗi bộ tản nhiệt,`r = sum_i % c_i`làm các phần còn lại, sau đó tính chi phí như`(c_i - r) * x^2 + r * (x + 1)^2`. Công thức này cân bằng các bộ tản nhiệt và tính đến mọi phần còn sót lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((sum_i chọn c_i)) | O(c_i) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`, số lượng phòng 
2. Với mỗi phòng, hãy đọc`c_i`Và`sum_i`. 
3. Xác định số lượng bộ tản nhiệt mà chúng ta sẽ thực sự sử dụng. Đây là`min(c_i, sum_i)`, bởi vì chúng ta không thể sử dụng nhiều bộ bức xạ hơn mức tối đa cho phép hoặc tổng số phần (bộ bức xạ có 0 phần là vô nghĩa). 
4. Tính số phần cơ bản trên mỗi bộ bức xạ như`x = sum_i // num_radiators`. 
5. Tính số dư`r = sum_i % num_radiators`. Đây là những phần bổ sung không thể phân bổ đều. 
6. Tính tổng chi phí:`(num_radiators - r) * x^2 + r * (x + 1)^2`. Công thức này gán`x`các phần cho hầu hết các bộ tản nhiệt và phân phối phần còn lại cho mỗi bộ tản nhiệt`r`bộ tản nhiệt. 
7. Xuất chi phí cho từng phòng. 

Tại sao nó hoạt động: Điều bất biến là tổng các phần được chỉ định bằng`sum_i`và số lượng bộ tản nhiệt không vượt quá`c_i`. Tính lồi đảm bảo rằng việc phân bố các phần càng đồng đều càng tốt sẽ giảm thiểu tổng bình phương. Bất kỳ sự phân phối nào khác mà một số bộ tản nhiệt có nhiều phần hơn đáng kể so với những bộ tản nhiệt khác đều làm tăng tổng chi phí. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
for _ in range(n):
    c_i, sum_i = map(int, input().split())
    num_radiators = min(c_i, sum_i)
    x = sum_i // num_radiators
    r = sum_i % num_radiators
    cost = (num_radiators - r) * x * x + r * (x + 1) * (x + 1)
    print(cost)
```Giải pháp đầu tiên là đọc số lượng phòng. Đối với mỗi phòng, nó tính toán số lượng bộ tản nhiệt sẽ sử dụng và chia đều các phần cần thiết. Phần còn lại được phân phối một cái cho mỗi bộ tản nhiệt, đảm bảo tất cả các bộ tản nhiệt đều có tiết diện nguyên. Nhân và bình phương theo thứ tự này sẽ tránh được lỗi dấu phẩy động. sử dụng`min(c_i, sum_i)`xử lý các trường hợp cạnh trong đó số lượng bộ tản nhiệt vượt quá số lượng phần. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
4
1 10000
10000 1
2 6
4 6
```| Phòng | c_i | tổng_i | num_radiators | x | r | Tính toán chi phí | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 10000 | 1 | 10000 | 0 | 1*10000^2 | 100000000 | 
| 2 | 10000 | 1 | 1 | 1 | 0 | 1*1^2 | 1 | 
| 3 | 2 | 6 | 2 | 3 | 0 | 2*3^2 | 18 | 
| 4 | 4 | 6 | 4 | 1 | 2 | 2_1^2 + 2_2^2 | 10 | 

Điều này khẳng định phần còn lại được phân bổ chính xác và tổng số phần luôn đáp ứng`sum_i`. 

**Đầu vào tùy chỉnh 1:**```
1
5 3
```| Phòng | c_i | tổng_i | num_radiators | x | r | Tính toán chi phí | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 3 | 3 | 1 | 0 | 3*1^2 | 3 | 

Điều này kiểm tra rằng khi`c_i > sum_i`, chúng tôi chỉ sử dụng`sum_i`bộ tản nhiệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phòng được xử lý độc lập với một số phép tính số học không đổi. | 
| Không gian | O(1) | Chúng tôi chỉ lưu trữ các biến tạm thời cho mỗi phòng, không cần mảng lớn. | 

Với`n <= 1000`, thuật toán thực hiện tối đa 1000 lần lặp, mỗi lần có các phép toán không đổi. Mức sử dụng bộ nhớ không đáng kể, thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    # solution
    n = int(input())
    for _ in range(n):
        c_i, sum_i = map(int, input().split())
        num_radiators = min(c_i, sum_i)
        x = sum_i // num_radiators
        r = sum_i % num_radiators
        cost = (num_radiators - r) * x * x + r * (x + 1) * (x + 1)
        print(cost)
    return out.getvalue().strip()

# provided samples
assert run("4\n1 10000\n10000 1\n2 6\n4 6\n") == "100000000\n1\n18\n10"

# custom cases
assert run("1\n5 3\n") == "3", "c_i > sum_i"
assert run("2\n1 1\n10 100\n") == "1\n1000", "single radiator vs many radiators"
assert run("1\n3 8\n") == "22", "unequal division"
assert run("1\n10000 10000\n") == "10000", "all ones"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n5 3 | 3 | Khi bộ tản nhiệt tối đa vượt quá phần | 
| 2\n1 1\n10 100 | 1\n1000 | Kết hợp bộ tản nhiệt đơn và nhiều | 
| 1\n3 8 | 22 | Phân bố phần không đồng đều | 
| 1\n10000 10000 | 10000 | Mỗi bộ tản nhiệt có đúng 1 phần | 

## Vỏ cạnh 

Nếu`c_i = 1`, chúng ta phải đặt tất cả các phần vào
