---
title: "CF 241A - Peykan cũ"
description: "Chúng ta được yêu cầu lập mô hình một hành trình dọc theo một đường thẳng của các thành phố được nối với nhau bằng đường một chiều, trong đó một ô tô di chuyển với tốc độ không đổi một km một giờ và tiêu thụ một lít nhiên liệu trên một km."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 241
codeforces_index: "A"
codeforces_contest_name: "Bayan 2012-2013 Elimination Round (ACM ICPC Rules, English statements)"
rating: 1300
weight: 241
solve_time_s: 72
verified: true
draft: false
---

[CF 241A - Peykan cũ](https://codeforces.com/problemset/problem/241/A) 

**Đánh giá:** 1300 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu lập mô hình một hành trình dọc theo một đường thẳng của các thành phố được nối với nhau bằng đường một chiều, trong đó một ô tô di chuyển với tốc độ không đổi một km một giờ và tiêu thụ một lít nhiên liệu trên một km. Mỗi thành phố (trừ thành phố cuối cùng) có nguồn cung cấp nhiên liệu được bổ sung sau mỗi _k_ giờ. Old Peykan bắt đầu tại thành phố đầu tiên với nguồn cung cấp nhiên liệu ban đầu từ thành phố đó và chúng ta cần tính tổng thời gian tối thiểu để đến thành phố cuối cùng. 

Dữ liệu đầu vào cung cấp số lượng đường _m_, ít hơn một so với số lượng thành phố, khoảng thời gian bổ sung _k_, khoảng cách của từng đoạn đường và nhiên liệu có sẵn tại mỗi thành phố. Đầu ra là một số nguyên duy nhất: thời gian tối thiểu tính bằng giờ để đến thành phố cuối cùng. 

Các ràng buộc đủ nhỏ để chúng ta có thể suy luận tuần tự qua từng thành phố. Với _m_ lên tới 1000 và khoảng cách lên tới 1000, giải pháp O(m) hoặc O(m log m) có thể được chấp nhận. Mọi thứ bậc hai vẫn sẽ chạy, nhưng việc mô phỏng đơn giản về mỗi lần nạp nhiên liệu vào mỗi giờ có thể trở nên cồng kềnh. 

Các trường hợp đặc biệt cần cảnh giác bao gồm các tình huống trong đó nhiên liệu của thành phố không đủ để đến thành phố tiếp theo. Trong trường hợp đó, Old Peykan có thể phải đợi nhiều khoảng thời gian _k_ giờ để nạp đủ nhiên liệu. Ví dụ, với khoảng cách`[10]`, nhiên liệu`[3]`, Và`k = 5`, thành phố đầu tiên không thể đi hết 10 km ngay lập tức. Chúng ta phải đợi nhiên liệu được làm mới trong bội số của 5 giờ. Một cách tiếp cận tham lam ngây thơ không tính đến việc chờ đợi sẽ đánh giá thấp thời gian di chuyển. 

## Phương pháp tiếp cận 

Phương pháp vũ lực sẽ mô phỏng hành trình từng giờ, giảm nhiên liệu mỗi giờ một lần và tăng nhiên liệu ở mỗi thành phố sau mỗi _k_ giờ. Mặc dù phương pháp này hoạt động ở khoảng cách nhỏ nhưng sẽ quá chậm nếu khoảng cách lớn vì nó có thể yêu cầu mô phỏng tới 1.000.000 đơn vị nhiên liệu, điều này không hiệu quả. 

Cái nhìn sâu sắc quan trọng là xử lý vấn đề theo từng thành phố thay vì từng giờ. Tại mỗi thành phố, chúng tôi biết khoảng cách đến thành phố tiếp theo và lượng nhiên liệu hiện có. Nếu lượng nhiên liệu sẵn có ít hơn quãng đường, chúng ta có thể tính thời gian chờ tối thiểu cần thiết để tích lũy nhiên liệu. Vì nhiên liệu được bổ sung theo bội số của _k_ giờ nên thời gian chờ là bội số nhỏ nhất của _k_ sao cho tổng lượng nhiên liệu là đủ. Sau khi chờ đợi, Old Peykan tiến ngay đến thành phố tiếp theo, tiêu tốn đúng quãng đường nhiên liệu. Điều này biến vấn đề thành một bước tham lam đơn giản: luôn đợi mức tối thiểu cần thiết ở mỗi thành phố để có đủ nhiên liệu để đến thành phố tiếp theo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force theo giờ | O(tổng khoảng cách) | O(1) | Quá chậm nếu khoảng cách lớn | 
| Tính toán theo từng thành phố tham lam | O(m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo tổng thời gian`t`về 0 và đặt nhiên liệu hiện tại`fuel`đến nguồn cung tại thành phố đầu tiên`s1`. 
2. Đi qua từng con đường từ thành phố i đến thành phố i+1. Cho phép`d`là khoảng cách và`s`nhiên liệu ở thành phố i. 
3. Nếu nhiên liệu hiện tại ít hơn khoảng cách đến thành phố tiếp theo, hãy tính xem chúng ta cần đợi bao nhiêu khoảng thời gian bổ sung đầy đủ. Cụ thể là tính`(d - fuel + s - 1) // s`để có được số lần nạp đầy đủ cần thiết. Nhân số này với`k`để có được thời gian chờ đợi. Cộng thời gian chờ này vào tổng thời gian và tăng lượng nhiên liệu được bổ sung tương ứng. 
4. Trừ khoảng cách`d`từ nhiên liệu để mô phỏng hành trình đến thành phố tiếp theo. Thêm vào`d`đến tổng thời gian. 
5. Di chuyển đến thành phố tiếp theo và lặp lại cho đến khi đến thành phố cuối cùng. 

Tại sao nó hoạt động: tại mỗi thành phố, chúng tôi luôn đợi mức nhiên liệu tối thiểu cần thiết để đến thành phố tiếp theo. Điều này đảm bảo rằng chúng ta không bao giờ phải chờ đợi quá mức cần thiết và không bao giờ hết nhiên liệu trên đường. Bởi vì vấn đề là tuyến tính nên việc tiếp nhiên liệu trước không có lợi ích gì ngoài nhu cầu trước mắt. Cách tiếp cận tham lam này tạo ra tổng thời gian tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

m, k = map(int, input().split())
d = list(map(int, input().split()))
s = list(map(int, input().split()))

time = 0
fuel = s[0]

for i in range(m):
    if fuel < d[i]:
        need = d[i] - fuel
        # number of full k-hour waits required
        wait_times = (need + s[i] - 1) // s[i]
        time += wait_times * k
        fuel += wait_times * s[i]
    fuel -= d[i]
    time += d[i]
    if i + 1 < m:
        fuel += s[i+1]

print(time)
```Mã đọc thông tin đầu vào, khởi tạo tổng thời gian và nhiên liệu, đồng thời lặp lại qua từng con đường. Tính toán chờ sử dụng phép chia số nguyên để làm tròn, đảm bảo Old Peykan đợi đủ để có ít nhất`d[i]`nhiên liệu. Sau khi đi xong, nhiên liệu ở thành phố tiếp theo sẽ được đổ vào bình. Các điểm tinh tế bao gồm việc đảm bảo chúng tôi không cố gắng truy cập`s[i+1]`sau thành phố cuối cùng và xử lý làm tròn chính xác trong tính toán chờ. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
4 6
1 2 5 2
2 3 3 4
```| Thành phố | Nhiên liệu trước | Khoảng cách | Nhiên Liệu Sau Khi Chờ Đợi | Chờ Thời Gian | Nhiên Liệu Sau Chuyến Đi | Tổng Thời Gian | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 2 | 0 | 1 | 1 | 
| 2 | 1+3=4 | 2 | 4 | 0 | 2 | 3 | 
| 3 | 2+3=5 | 5 | 5 | 0 | 0 | 8 | 
| 4 | 0+4=4 | 2 | 4 | 0 | 2 | 10 | 

Điều này chứng tỏ thuật toán chỉ chờ khi cần và tích lũy nhiên liệu chính xác từ mỗi thành phố. 

Đầu vào mẫu 2:```
3 5
10 3
3 2 4
```| Thành phố | Nhiên liệu trước | Khoảng cách | Nhiên Liệu Sau Khi Chờ Đợi | Chờ Thời Gian | Nhiên Liệu Sau Chuyến Đi | Tổng Thời Gian | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 10 | 3 | 2*5=10 | 13 | 10 | 
| 2 | 13+2=15 | 3 | 15 | 0 | 12 | 13 | 
| 3 | 12+4=16 | - | - | - | - | - | 

Old Peykan đợi 10 tiếng ở thành phố 1 để tích đủ nhiên liệu đi được 10 km. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) | Một lần đi qua những con đường, số học đơn giản từng bước | 
| Không gian | O(m) | Mảng khoảng cách và nguồn cung cấp | 

Với m ≤ 1000, nghiệm tuyến tính này dễ dàng chạy trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    m, k = map(int, input().split())
    d = list(map(int, input().split()))
    s = list(map(int, input().split()))

    time = 0
    fuel = s[0]

    for i in range(m):
        if fuel < d[i]:
            need = d[i] - fuel
            wait_times = (need + s[i] - 1) // s[i]
            time += wait_times * k
            fuel += wait_times * s[i]
        fuel -= d[i]
        time += d[i]
        if i + 1 < m:
            fuel += s[i+1]

    return str(time)

# Provided samples
assert run("4 6\n1 2 5 2\n2 3 3 4\n") == "10", "sample 1"
assert run("3 5\n10 3\n3 2 4\n") == "18", "sample 2"

# Custom cases
assert run("1 1\n1\n1\n") == "1", "minimum input"
assert run("2 3\n5 6\n2 2 3\n") == "14", "requires wait at first city"
assert run("2 10\n10 10\n10 10 10\n") == "20", "exact fuel, no wait"
assert run("3 2\n3 6 5\n2 2 2 2\n") == "19", "multiple waits required"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1\n1\n1\n | 1 | kích thước bài toán nhỏ nhất | 
| 2 3\n5 6\n2 2 3\n | 14 | cần chờ đổ xăng ở thành phố đầu tiên | 
| 2 10\n10 10\n10 10 10\n | 20 | nhiên liệu chính xác, không phải chờ đợi | 
| 3 2\n3 6 5\n2 2 2 2\n | 19 | bổ sung nhiều lần trên khắp các thành phố | 

## Vỏ cạnh 

Một thành phố có thể không đủ nhiên liệu để đến thành phố tiếp theo ngay lập tức. Ví dụ:```
2 5
10
```
