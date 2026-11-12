---
title: "CF 1615A - Thu hẹp khoảng cách"
description: "Chúng ta có một dãy tháp, mỗi tháp có một số khối nhất định xếp chồng lên nhau theo chiều dọc. Mục tiêu là làm cho chiều cao của tất cả các tòa tháp càng gần nhau càng tốt. Vào bất kỳ ngày nào, chúng ta có thể lấy một khối từ một tòa tháp và đặt nó lên một tòa tháp khác."
date: "2026-06-10T06:38:08+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1615
codeforces_index: "A"
codeforces_contest_name: "Codeforces Global Round 18"
rating: 800
weight: 1615
solve_time_s: 71
verified: true
draft: false
---

[CF 1615A - Thu hẹp khoảng cách](https://codeforces.com/problemset/problem/1615/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một dãy tháp, mỗi tháp có một số khối nhất định xếp chồng lên nhau theo chiều dọc. Mục tiêu là làm cho chiều cao của tất cả các tòa tháp càng gần nhau càng tốt. Vào bất kỳ ngày nào, chúng ta có thể lấy một khối từ một tòa tháp và đặt nó lên một tòa tháp khác. Sự sắp xếp xấu xí chính là sự khác biệt giữa tòa tháp cao nhất và thấp nhất. Chúng ta muốn xác định độ xấu nhỏ nhất có thể mà chúng ta có thể đạt được sau bất kỳ số lần di chuyển khối nào. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi nhận được số lượng tháp`n`và chiều cao của những tòa tháp đó. Đầu ra phải là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm biểu thị độ xấu tối thiểu có thể đạt được. 

Các hạn chế ở mức vừa phải: tối đa 100 tòa tháp và 1000 trường hợp thử nghiệm. Chiều cao tháp riêng lẻ có thể lên tới$10^7$. Từ`n`nhỏ, một thuật toán có dạng bậc hai`n`là khả thi, nhưng giải pháp tuyến tính được ưu tiên hơn. Các trường hợp cạnh bao gồm các tòa tháp đã bằng nhau, các tòa tháp chỉ có một tòa nhà rất cao hoặc rất thấp và các trường hợp tổng số khối không chia đều cho`n`. Một cách tiếp cận đơn giản là di chuyển liên tục từng khối một sẽ đúng nhưng sẽ chậm một cách không cần thiết. Ví dụ, nếu các tòa tháp`[1, 2, 3, 1, 5]`, không phải lúc nào chúng ta cũng đạt được độ cao bằng nhau và độ xấu tối thiểu cuối cùng sẽ là`1`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng di chuyển liên tục các khối từ tòa tháp cao nhất đến tòa tháp ngắn nhất cho đến khi không thể cải thiện được. Mặc dù điều này cuối cùng sẽ hội tụ nhưng nó có thể cần tới$O(n \cdot \text{max}(a_i))$hoạt động không thể thực hiện được khi`a_i`có thể đạt được$10^7$. Chúng ta cần một cách tiếp cận thông minh hơn. 

Quan sát quan trọng là các khối di chuyển chỉ phân phối lại tổng số khối trên các tháp. Cho phép`total = sum(a)`. Nếu như`total`chia đều cho`n`, chúng ta có thể làm cho tất cả các tòa tháp bằng nhau, đạt được độ xấu bằng 0. Nếu`total`không chia đều thì một số tháp phải có`floor(total/n)`khối và một số phải có`ceil(total/n)`khối. Sự khác biệt giữa hai đại lượng này luôn là`1`, đó là độ xấu tối thiểu có thể đạt được. 

Quan sát này cho phép chúng tôi tính toán kết quả theo thời gian không đổi cho mỗi trường hợp thử nghiệm: kiểm tra khả năng chia hết của tổng cho số tháp và đầu ra`0`hoặc`1`. Không cần mô phỏng chuyển động khối thực tế. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * max(a_i)) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`và mảng độ cao của tháp`a`. 
3. Tính tổng chiều cao của tất cả các tòa tháp. 
4. Tính phép chia số nguyên`avg = total // n`. 
5. Tính số dư`r = total % n`. Nếu như`r == 0`, tổng chia đều nên độ xấu tối thiểu là`0`. Nếu không, một số tòa tháp chắc chắn sẽ cao hơn những tòa tháp khác một khối, tạo ra độ xấu tối thiểu`1`. 
6. Xuất ra độ xấu được tính toán cho từng trường hợp thử nghiệm. 

Thuật toán hoạt động vì hoạt động di chuyển khối bảo toàn tổng số khối. Các tòa tháp cao nhất và ngắn nhất chỉ có thể khác nhau nhiều nhất là một nếu tổng số tiền không thể chia đều. Bất biến này đảm bảo rằng việc tính toán của chúng ta về`0`hoặc`1`luôn đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    total = sum(a)
    if total % n == 0:
        print(0)
    else:
        print(1)
```Giải pháp đầu tiên đọc tất cả đầu vào một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`. Tổng hợp mảng`a`đưa ra tổng số khối. Kiểm tra khả năng chia hết cho`n`trực tiếp mang lại sự xấu xí tối thiểu có thể. Séc`total % n == 0`xử lý chính xác cả số tiền nhỏ và số tiền lớn và không cần bộ nhớ bổ sung ngoài mảng đầu vào. 

## Ví dụ đã hoạt động 

Theo dõi đầu vào`[3, 10, 10, 10]`: 

| Bước | một | tổng cộng | tổng cộng % n | sự xấu xí | 
| --- | --- | --- | --- | --- | 
| tính tổng | [10, 10, 10] | 30 | 0 | 0 | 

Tất cả các tòa tháp đều bình đẳng; đầu ra là`0`. 

Theo dõi đầu vào`[5, 1, 2, 3, 1, 5]`: 

| Bước | một | tổng cộng | tổng cộng % n | sự xấu xí | 
| --- | --- | --- | --- | --- | 
| tính tổng | [1, 2, 3, 1, 5] | 12 | 12% 5 = 2 | 1 | 

tổng`12`chia cho`5`lá còn lại`2`. Một số tháp phải`2`khối cao hơn những khối khác, vì vậy độ xấu tối thiểu là`1`. 

Những dấu vết này chứng tỏ sự bất biến rằng tổng xác định sự khác biệt tối thiểu có thể đạt được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Cần tính tổng mảng một lần cho mỗi test | 
| Không gian | O(n) cho mỗi trường hợp thử nghiệm | Lưu trữ chiều cao tháp | 

Được cho`n <= 100`Và`t <= 1000`, thời gian trong trường hợp xấu nhất là khoảng 100.000 thao tác, trong giới hạn thời gian 2 giây. Việc sử dụng bộ nhớ là không đáng kể ở quy mô này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        print(0 if sum(a) % n == 0 else 1)
    return out.getvalue().strip()

# provided samples
assert run("3\n3\n10 10 10\n4\n3 2 1 2\n5\n1 2 3 1 5\n") == "0\n0\n1", "sample 1"

# custom cases
assert run("2\n2\n1 2\n3\n2 2 2\n") == "1\n0", "minimum size inputs"
assert run("1\n4\n10000000 10000000 10000000 10000001\n") == "1", "large numbers"
assert run("1\n5\n5 5 5 5 5\n") == "0", "all equal values"
assert run("1\n3\n1 1 2\n") == "1", "boundary conditions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2\n2\n1 2\n3\n2 2 2\n`|`1\n0`| mảng nhỏ nhất, tổng không đều/chẵn | 
|`1\n4\n10000000 10000000 10000000 10000001\n`|`1`| số lượng lớn gần giới hạn | 
|`1\n5\n5 5 5 5 5\n`|`0`| tất cả các tháp đều bằng nhau | 
|`1\n3\n1 1 2\n`|`1`| tổng không chia hết, chênh lệch tối thiểu | 

## Vỏ cạnh 

Đối với một trường hợp cần thao tác duy nhất,`[1, 2]`, tổng là`3`. Chia cho`2`lá còn lại`1`. Đầu ra của thuật toán`1`, xác định chính xác rằng một tòa tháp phải cao hơn tháp kia. Đối với trường hợp đã cân bằng`[2, 2, 2]`, tổng là`6`, chia hết cho`3`, vậy đầu ra là`0`. Cả hai trường hợp đặc biệt đều chứng minh rằng thuật toán xử lý chính xác cả tổng chia hết và không chia hết mà không cần chuyển động khối rõ ràng.
