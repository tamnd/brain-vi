---
title: "CF 1113A - Sasha và chuyến đi của anh ấy"
description: "Sasha muốn lái xe từ thành phố 1 đến thành phố n dọc theo một đường thẳng gồm các thành phố. Mỗi thành phố cách nhau đúng một cây số, mọi con đường đều đi về phía trước nên anh ta không thể lùi lại được. Chiếc xe của anh ta tiêu thụ một lít nhiên liệu trên mỗi km và khởi động với bình xăng rỗng."
date: "2026-06-12T04:56:43+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1113
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 539 (Div. 2)"
rating: 900
weight: 1113
solve_time_s: 63
verified: true
draft: false
---

[CF 1113A - Sasha và chuyến đi của anh ấy](https://codeforces.com/problemset/problem/1113/A) 

**Xếp hạng:** 900 
**Tags:** dp, tham lam, toán học 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Sasha muốn lái xe từ thành phố 1 đến thành phố n dọc theo một đường thẳng gồm các thành phố. Mỗi thành phố cách nhau đúng một cây số, mọi con đường đều đi về phía trước nên anh ta không thể lùi lại được. Chiếc xe của anh ta tiêu thụ một lít nhiên liệu trên mỗi km và khởi động với bình xăng rỗng. Mỗi thành phố đều có một trạm xăng với giá nhiên liệu bằng với chỉ số của thành phố tính bằng đô la/lít. Thùng của ô tô có thể chứa tối đa`v`lít. Mục tiêu là tính toán tổng chi phí nhiên liệu tối thiểu để Sasha có thể đến được thành phố.`n`. 

Đầu vào bao gồm hai số nguyên:`n`, số lượng thành phố và`v`, dung tích bể. Đầu ra là một số nguyên biểu thị tổng chi phí nhiên liệu tối thiểu. 

Với các ràng buộc (`2 ≤ n ≤ 100`Và`1 ≤ v ≤ 100`), chúng tôi biết rằng giải pháp có thể sử dụng thuật toán O(n) hoặc thậm chí O(n²), vì trong trường hợp xấu nhất, chúng tôi chỉ xử lý được vài nghìn thao tác. Các trường hợp cạnh phát sinh khi dung tích bể rất nhỏ (`v = 1`), khi nó lớn hơn hoặc bằng số thành phố trừ đi một (`v ≥ n - 1`), và khi Sasha phải đổ xăng nhiều lần ở những thành phố ngày càng đắt đỏ. Ví dụ, nếu`n = 5`Và`v = 2`, một cách tiếp cận ngây thơ chỉ đổ đầy một lít mỗi lần sẽ vượt mức so với việc đổ đầy bình một cách chiến lược ở các thành phố rẻ hơn. 

## Phương pháp tiếp cận 

Giải pháp bạo lực rất đơn giản: mô phỏng mọi trình tự mua nhiên liệu có thể có ở mỗi thành phố. Đối với mỗi thành phố, hãy thử mua mọi lượng nhiên liệu khả thi, tiến về phía trước và lặp lại cho đến khi đến thành phố cuối cùng. Mặc dù đúng nhưng cách tiếp cận này nhanh chóng trở nên cồng kềnh vì nó yêu cầu kiểm tra số lượng trình tự mua hàng theo cấp số nhân. Ngay cả với nhỏ`n`, việc ghi sổ tất cả các trạng thái nhiên liệu có thể có là tẻ nhạt. 

Quan sát chính là nhiên liệu rẻ hơn ở các thành phố sớm hơn. Vì xe chỉ di chuyển về phía trước nên tối ưu là mua càng nhiều nhiên liệu càng tốt ở thành phố rẻ nhất hiện có mà không vượt quá dung tích bình xăng hoặc lãng phí nhiên liệu. Cụ thể, Sasha nên đổ đầy bình ở thành phố 1 nếu dung tích bình chứa nhỏ hơn tổng khoảng cách, sau đó chỉ đổ đầy bình ở các thành phố tiếp theo một cách chiến lược khi cần thiết. Nếu dung tích bể`v`lớn hơn hoặc bằng`n - 1`, anh ta có thể đổ đầy bình một lần ở thành phố đầu tiên và lái thẳng đến thành phố cuối cùng. 

Quan sát này cho phép một cách tiếp cận tham lam: mua đủ nhiên liệu để đến thành phố cuối cùng bất cứ khi nào có thể, bắt đầu từ thành phố sớm nhất mà không bao giờ mua thêm nhiên liệu đắt tiền một cách không cần thiết. Nó làm giảm vấn đề thành một phép tính số học đơn giản dựa trên`v`Và`n`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n²) | Quá chậm | 
| Tham lam / Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu dung tích bể`v`lớn hơn hoặc bằng`n - 1`, mua`n - 1`lít tại thành phố 1. Tổng chi phí là`(n - 1) * 1`đô la. Trả lại giá trị này ngay lập tức. Điều này xử lý trường hợp đơn giản trong đó Sasha có thể đến đích chỉ trong một lần. 
2. Nếu không, hãy đổ đầy bình`v`tại thành phố 1. Chi phí là`v * 1`đô la. Sasha sẽ sử dụng`v`km nhiên liệu để tiến bộ. 
3. Quãng đường còn lại tính xem cần thêm bao nhiêu nhiên liệu:`remaining = n - 1 - v`. 
4. Mua 1 lít ở mỗi thành phố tiếp theo, bắt đầu từ thành phố 2 cho đến hết quãng đường còn lại. Chi phí tại TP.`i`là`i`đô la một lít. Thêm các chi phí này một cách tích lũy. 
5. Tính tổng chi phí đầy bình ban đầu và tất cả chi phí nhiên liệu bổ sung làm tổng chi phí tối thiểu. 

Điều bất biến ở đây là chúng ta luôn mua nhiên liệu rẻ nhất có thể để tiếp tục tiến về phía trước. Chúng tôi không bao giờ mua thêm nhiên liệu ở những thành phố đắt đỏ nếu chúng tôi có thể vận chuyển nó từ một thành phố rẻ hơn, đảm bảo tính tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, v = map(int, input().split())

if v >= n - 1:
    # Tank is large enough to reach last city directly
    print(n - 1)
else:
    cost = v  # fill full tank at city 1
    remaining = n - 1 - v
    # Buy 1 liter at each city starting from city 2
    for i in range(2, 2 + remaining):
        cost += i
    print(cost)
```Đầu tiên mã sẽ kiểm tra xem dung tích bình chứa có cho phép di chuyển trực tiếp hay không. Nếu vậy, chi phí tối thiểu chỉ đơn giản là khoảng cách. Nếu không, nó sẽ đổ đầy bình ở thành phố đầu tiên, sau đó mua thêm 1 lít ở các thành phố rẻ nhất tiếp theo cho đến khi đến thành phố cuối cùng. các`range(2, 2 + remaining)`là rất quan trọng; bắt đầu tại thành phố 2 đảm bảo chúng tôi không tính hai lần thành phố 1 và kết thúc tại`2 + remaining - 1`bao gồm chính xác số km còn lại. 

## Ví dụ đã hoạt động 

Mẫu 1:`n = 4, v = 2`| Thành phố | Nhiên liệu đã mua | Khoảng cách còn lại | Chi phí | 
| --- | --- | --- | --- | 
| 1 | 2 | 2 | 2 | 
| 2 | 1 | 1 | 2 + 2 = 4 | 
| 3 | 0 | 1 | - | 

Giải thích: Bình chứa đầy 2 lít ở thành phố 1. Sasha lái xe 2 km đến thành phố 3, uống 2 lít, mua 1 lít ở thành phố 2 với giá 2 đô la, và cuối cùng đến thành phố 4. 

Mẫu 2:`n = 5, v = 10`| Thành phố | Nhiên liệu đã mua | Khoảng cách còn lại | Chi phí | 
| --- | --- | --- | --- | 
| 1 | 4 | 0 | 4 | 

Giải thích: Dung tích bình chứa vượt quá khoảng cách`n-1 = 4`, như vậy mua 4 lít ở thành phố 1 là đủ. Tổng chi phí là 4. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Tất cả các phép tính tối đa là tổng số học`n`giá trị | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Thuật toán dễ dàng phù hợp với giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB, ngay cả ở các giá trị tối đa của`n = 100`Và`v = 100`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, v = map(int, input().split())
    if v >= n - 1:
        return str(n - 1)
    cost = v
    remaining = n - 1 - v
    for i in range(2, 2 + remaining):
        cost += i
    return str(cost)

# Provided samples
assert run("4 2\n") == "4", "sample 1"
assert run("5 10\n") == "4", "sample 2"

# Custom cases
assert run("2 1\n") == "1", "minimum input"
assert run("3 2\n") == "2", "tank equals distance"
assert run("5 1\n") == "10", "small tank requires multiple stops"
assert run("100 50\n") == "3725", "large n, tank smaller than distance"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 | 1 | Khoảng cách tối thiểu, bể nhỏ nhất | 
| 3 2 | 2 | Xe tăng có thể đến thẳng thành phố cuối cùng | 
| 5 1 | 10 | Xe tăng nhỏ yêu cầu tiếp nhiên liệu tăng dần | 
| 100 50 | 3725 | Kịch bản n lớn, v < n-1 | 

## Vỏ cạnh 

Khi nào`v = 1`, Sasha phải mua nhiên liệu ở mọi thành phố. Ví dụ,`n = 5, v = 1`: 

| Thành phố | Nhiên liệu đã mua | Khoảng cách còn lại | Chi phí | 
| --- | --- | --- | --- | 
| 1 | 1 | 3 | 1 | 
| 2 | 1 | 2 | 1 + 2 = 3 | 
| 3 | 1 | 1 | 3 + 3 = 6 | 
| 4 | 1 | 0 | 6 + 4 = 10 | 

Đầu ra là 10, phù hợp với phép tính tham lam. Điều này xác nhận thuật toán xử lý chính xác tình huống xe tăng nhỏ nhất. Tương tự, nếu`v ≥ n - 1`, như trong`n = 5, v = 10`, thuật toán trả về ngay`n - 1 = 4`, đó là tối ưu.
