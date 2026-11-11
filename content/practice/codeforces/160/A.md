---
title: "CF 160A - Cặp song sinh"
description: "Chúng ta được cấp một bộ sưu tập các đồng xu, mỗi đồng xu có một giá trị nguyên dương. Nhiệm vụ là chọn một tập hợp con của những đồng tiền này sao cho tổng giá trị của những đồng tiền mà chúng ta đã chọn lớn hơn tổng giá trị của những đồng tiền còn lại cho người kia."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 160
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 111 (Div. 2)"
rating: 900
weight: 160
solve_time_s: 75
verified: true
draft: false
---

[CF 160A - Cặp song sinh](https://codeforces.com/problemset/problem/160/A) 

**Xếp hạng:** 900 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một bộ sưu tập các đồng xu, mỗi đồng xu có một giá trị nguyên dương. Nhiệm vụ là chọn một tập hợp con của những đồng tiền này sao cho tổng giá trị của những đồng tiền mà chúng ta đã chọn lớn hơn tổng giá trị của những đồng tiền còn lại cho người kia. Chúng tôi cũng phải giảm thiểu số lượng xu lấy để đạt được mục tiêu này. 

Đầu vào cung cấp số lượng xu,$n$và giá trị của các đồng tiền dưới dạng một dãy số nguyên. Đầu ra là một số nguyên duy nhất: số lượng xu tối thiểu cần thiết để vượt quá tổng số xu còn lại. Vì giá trị của mỗi đồng xu nằm trong khoảng từ 1 đến 100 và$n$tối đa là 100, tất cả các phép tính đều phù hợp thoải mái trong số học số nguyên tiêu chuẩn. Điều này có nghĩa là chúng ta có thể tự do tính tổng các giá trị hoặc sắp xếp mảng mà không phải lo lắng về các thao tác tràn hoặc tốn kém. 

Các trường hợp khó khăn có thể gặp phải cách tiếp cận ngây thơ bao gồm các tình huống trong đó tất cả các đồng xu đều có giá trị như nhau hoặc khi một đồng xu đã lớn hơn một nửa tổng số tiền. Ví dụ, với đầu vào`4\n1 1 1 1`, bất kỳ nỗ lực lấy ít hơn 3 xu đều dẫn đến kết quả hòa hoặc thua. Một trường hợp tinh tế khác là khi một đồng tiền có giá trị bằng tổng của tất cả các đồng tiền khác, chẳng hạn như`2\n3 3`; ở đây chúng ta phải lấy cả hai đồng tiền để thỏa mãn sự bất đẳng thức nghiêm ngặt. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là xem xét tất cả các tập hợp con có thể có của các đồng xu, tính tổng của chúng và kiểm tra xem mỗi tập hợp con có tổng lớn hơn tổng các đồng xu còn lại hay không. Điều này đúng nhưng không hiệu quả: đối với$n$tiền xu, có$2^n$tập hợp con. Thậm chí tại$n = 20$, điều này sẽ yêu cầu hơn một triệu lần kiểm tra và tối đa$n = 100$, điều đó là không thể thực hiện được. 

Quan sát quan trọng giúp đơn giản hóa vấn đề là thứ tự chúng ta chọn đồng xu rất quan trọng và những đồng tiền lớn hơn sẽ đóng góp nhiều hơn vào tổng số tiền một cách nhanh chóng. Do đó, chúng ta có thể sắp xếp các đồng xu theo thứ tự giảm dần và chọn những đồng xu bắt đầu từ số lớn nhất. Ở mỗi bước, chúng tôi tích lũy số tiền của chúng cho đến khi vượt quá một nửa tổng số tiền. Điều này đảm bảo số lượng xu cần thiết nhỏ nhất vì việc lấy những đồng xu nhỏ hơn trước sẽ cần nhiều xu hơn để vượt qua số tiền còn lại. 

Cách tiếp cận tối ưu thúc đẩy việc sắp xếp và tích lũy tham lam đơn giản, tránh nhu cầu kiểm tra tất cả các tập hợp con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số xu,$n$và mảng các giá trị tiền xu. Chúng ta cần danh sách đầy đủ để tính tổng và thực hiện sắp xếp. 
2. Tính tổng giá trị của tất cả các đồng xu. Điều này là cần thiết để biết ngưỡng chúng ta cần vượt qua. 
3. Sắp xếp các đồng xu theo thứ tự giảm dần. Điều này đảm bảo rằng mỗi đồng xu chúng tôi chọn đóng góp giá trị tối đa có thể, giảm thiểu số lượng xu cần thiết. 
4. Khởi tạo hai biến: một để theo dõi tổng tích lũy của các đồng tiền đã chọn và một để đếm xem chúng tôi đã lấy được bao nhiêu đồng. 
5. Lặp lại các đồng xu đã được sắp xếp, cộng giá trị của từng đồng xu vào tổng tích lũy và tăng số lượng xu. 
6. Sau khi thêm từng đồng xu, hãy kiểm tra xem số tiền tích lũy có vượt quá một nửa tổng số tiền hay không. Nếu đúng như vậy, hãy dừng việc lặp lại và trả về số xu đã lấy. 
7. Xuất ra số xu tối thiểu. Điều này thỏa mãn điều kiện là tổng của chúng tôi lớn hơn tổng số tiền còn lại. 

Điều bất biến ở đây là tại bất kỳ thời điểm nào trong vòng lặp, tổng tích lũy là số tiền lớn nhất có thể đạt được với số xu đó. Bằng cách chọn những đồng tiền lớn nhất trước tiên, chúng tôi đảm bảo rằng ngay khi số tiền tích lũy vượt quá số tiền còn lại, không tập hợp con nào nhỏ hơn có thể đạt được điều này với ít tiền hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
coins = list(map(int, input().split()))

total_sum = sum(coins)
coins.sort(reverse=True)

accum = 0
count = 0

for coin in coins:
    accum += coin
    count += 1
    if accum > total_sum - accum:
        print(count)
        break
```Đầu tiên chúng ta đọc số xu và danh sách các giá trị. các`sum`hàm tính tổng một cách hiệu quả. Sắp xếp theo thứ tự giảm dần đảm bảo việc lựa chọn các đồng tiền lớn nhất trước tiên sẽ được tham lam. Trong quá trình lặp lại, chúng tôi tích lũy tổng và đếm đồng tiền. Điều kiện dừng`accum > total_sum - accum`trực tiếp thực hiện yêu cầu bất bình đẳng nghiêm ngặt. sử dụng`break`đảm bảo rằng chúng ta sẽ dừng ngay lập tức ngay khi đạt được điều kiện. 

## Ví dụ đã hoạt động 

**Mẫu 1:**`2\n3 3`| đồng xu | tích lũy | tổng_tổng - tích lũy | đếm | 
| --- | --- | --- | --- | 
| 3 | 3 | 3 | 1 | 
| 3 | 6 | 0 | 2 | 

Đồng xu đầu tiên không đủ vì`3`không lớn hơn`3`. Lấy cả hai đồng xu sẽ mang lại`6 > 0`, vì vậy đầu ra là`2`. 

**Mẫu 2:**`4\n1 2 2 2`| đồng xu | tích lũy | tổng_tổng - tích lũy | đếm | 
| --- | --- | --- | --- | 
| 2 | 2 | 5 | 1 | 
| 2 | 4 | 3 | 2 | 

Lấy hai đồng tiền lớn nhất sẽ mang lại`4 > 3`, vì vậy đầu ra là`2`. Điều này cho thấy cách tiếp cận tham lam đảm bảo số lượng xu tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp các đồng tiền chiếm ưu thế; lặp lại để tích lũy tổng là O(n) | 
| Không gian | O(n) | Lưu trữ mảng tiền xu | 

Với$n \leq 100$, việc sắp xếp và tích lũy là không đáng kể trong khoảng thời gian 2 giây. Mức sử dụng bộ nhớ thấp hơn nhiều so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    coins = list(map(int, input().split()))
    total_sum = sum(coins)
    coins.sort(reverse=True)
    accum = 0
    count = 0
    for coin in coins:
        accum += coin
        count += 1
        if accum > total_sum - accum:
            return str(count)

# provided samples
assert run("2\n3 3\n") == "2", "sample 1"
assert run("4\n1 2 2 2\n") == "2", "sample 2"

# custom cases
assert run("1\n10\n") == "1", "single coin"
assert run("5\n1 1 1 1 1\n") == "3", "all equal small coins"
assert run("3\n2 5 7\n") == "2", "large coin plus another"
assert run("6\n10 10 10 1 1 1\n") == "2", "take two largest"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n10 | 1 | Vỏ đồng xu đơn | 
| 5\n1 1 1 1 1 | 3 | Số tối thiểu vượt quá một nửa khi tất cả các đồng xu bằng nhau | 
| 3\n2 5 7 | 2 | Kết hợp lựa chọn đồng xu lớn và nhỏ | 
| 6\n10 10 10 1 1 1 | 2 | Tham lam chọn đúng đồng tiền lớn nhất | 

## Vỏ cạnh 

cho`2\n3 3`, lặp lại các đồng tiền đã được sắp xếp sẽ mang lại tổng tích lũy là`3`sau đồng xu đầu tiên, không vượt quá`3`. Thêm đồng xu thứ hai sẽ mang lại`6 > 0`, do đó thuật toán xuất ra chính xác`2`. Vì`5\n1 1 1 1 1`, thuật toán chọn ba đồng tiền để tiếp cận`3 > 2`, cho thấy nó xử lý tất cả các đồng tiền bằng nhau một cách chính xác. Điều kiện dừng đảm bảo rằng bất đẳng thức nghiêm ngặt luôn được thỏa mãn, ngay cả trong các kịch bản ràng buộc hoặc biên.
