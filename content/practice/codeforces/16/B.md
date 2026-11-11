---
title: "CF 16B - Trộm và diêm"
description: "Chúng ta có một tên trộm có thể mang đúng n hộp diêm. Trong kho có m container. Mỗi hộp diêm i có hộp diêm a_i và mỗi hộp diêm trong hộp chứa đó chứa b_i hộp diêm."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 16
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 16 (Div. 2 Only)"
rating: 900
weight: 16
solve_time_s: 73
verified: true
draft: false
---
[CF 16B - Kẻ trộm và diêm](https://codeforces.com/problemset/problem/16/B) 

**Xếp hạng:** 900 
**Tags:** tham lam, thực hiện, sắp xếp 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tên trộm có thể mang chính xác`n`hộp diêm. Trong kho có`m`thùng chứa. Mỗi thùng chứa`i`có`a_i`hộp diêm và mỗi hộp diêm trong hộp đó chứa`b_i`trận đấu. Mục tiêu là để đón`n`hộp diêm sao cho tổng số que diêm là lớn nhất. 

Đầu vào là hai số nguyên`n`Và`m`theo sau là`m`cặp`(a_i, b_i)`. Đầu ra là một số nguyên duy nhất: tổng số trận đấu tối đa mà tên trộm có thể lấy. 

Những hạn chế đang nói lên điều đó.`n`có thể lớn bằng 2·10^8. Điều này rất quan trọng đối với việc lặp lại các hộp diêm riêng lẻ, do đó, bất kỳ giải pháp nào kiểm tra từng hộp diêm riêng lẻ sẽ quá chậm.`m`nhiều nhất là 20 nên số lượng container rất ít. Điều đó gợi ý rằng việc phân loại hoặc lựa chọn tham lam đối với các vùng chứa là khả thi.`a_i`có thể lớn nên số lượng hộp diêm trong một thùng chứa có thể vượt quá`n`.`b_i`nhỏ, từ 1 đến 10, vì vậy các kết quả phù hợp trên mỗi hộp rất dễ xử lý với các số nguyên thông thường. 

Các hộp đựng ở cạnh cần chú ý bao gồm các hộp đựng có nhiều hộp diêm hơn mức mà tên trộm có thể mang theo. Ví dụ, nếu`n = 5`và một container có`a_i = 10`hộp với`b_i = 3`diêm, tên trộm chỉ lấy được 5 que diêm. Việc thực hiện bất cẩn sẽ bổ sung thêm tất cả`10*3 = 30`các trận đấu sẽ sai. Một trường hợp cạnh khác là khi`n`lớn hơn tổng số hộp diêm trong tất cả các thùng chứa. Trong trường hợp đó, tên trộm chỉ nên lấy đi mọi thứ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp lại qua mọi tổ hợp hộp diêm có thể có từ các thùng chứa, tổng hợp tổng số kết quả phù hợp và chọn ra kết quả tốt nhất. Với`m`lên tới 20, mỗi hộp có tới 10^8 hộp diêm, số lượng kết hợp là vô cùng lớn. Ngay cả khi chúng ta chỉ coi các thùng chứa là các đơn vị nguyên vẹn thì số lượng các tập hợp con vẫn là`2^m = 2^20 ≈ 1 million`, điều này khả thi đối với các tập hợp con nhưng không cần thiết ở đây. Đi xuống từng hộp diêm riêng lẻ rõ ràng là không thể bởi vì`n`có thể là 2·10^8. 

Nhận xét quan trọng là vấn đề có tính tham lam: để tối đa hóa số lượng que diêm, trước tiên tên trộm phải lấy hộp diêm có số lớn nhất.`b_i`(diêm mỗi hộp). Thứ tự của các container không liên quan ngoại trừ`b_i`. Sau khi các container được sắp xếp theo`b_i`Đi xuống, tên trộm lấy càng nhiều hộp càng tốt từ thùng đầu tiên, rồi đến thùng tiếp theo, cho đến khi đầy ba lô. Điều này có hiệu quả vì mỗi hộp diêm không thể chia được và các hộp có nhiều kết quả trùng khớp hơn luôn tốt hơn những hộp có ít kết quả hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^m * m) | O(m) | Quá chậm | 
| Tham lam/Sắp xếp theo`b_i`| O(m log m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`(công suất) và`m`(số lượng container). Khởi tạo`total_matches = 0`. 
2. Đọc từng thùng chứa`(a_i, b_i)`và lưu trữ dưới dạng danh sách các bộ dữ liệu. 
3. Sắp xếp danh sách container theo thứ tự giảm dần`b_i`. Việc sắp xếp đảm bảo chúng tôi luôn xem xét các hộp có giá trị nhất trước tiên. 
4. Lặp lại các vùng chứa đã được sắp xếp: 

một. Cho phép`take = min(a_i, n)`. Đây là số hộp chúng ta có thể lấy từ thùng này mà không vượt quá sức chứa. 

b. Thêm vào`take * b_i`ĐẾN`total_matches`. 

c. Giảm bớt`n`qua`take`. Nếu như`n`trở thành 0, ngắt vòng lặp vì ba lô đã đầy. 
5. In`total_matches`. 

Tại sao nó hoạt động: Ở mỗi bước, trước tiên chúng tôi sẽ lấy các hộp diêm có sẵn có số kết quả trùng khớp cao nhất trên mỗi hộp. Vì hộp diêm không thể chia được nên không có lợi ích gì khi để hộp có giá trị cao cho hộp có giá trị thấp hơn. Sự lựa chọn tham lam là an toàn và bằng cách lặp đi lặp lại cho đến khi`n`các hộp được lấy, chúng tôi đảm bảo rằng tổng số trận đấu được tối đa hóa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
containers = []

for _ in range(m):
    a, b = map(int, input().split())
    containers.append((b, a))  # store as (matches per box, number of boxes)

# Sort by matches per box descending
containers.sort(reverse=True)

total_matches = 0
for b, a in containers:
    if n == 0:
        break
    take = min(a, n)
    total_matches += take * b
    n -= take

print(total_matches)
```Chúng tôi lưu trữ`(b, a)`thay vì`(a, b)`bởi vì việc phân loại dễ dàng hơn`b`giảm dần. Trong quá trình lặp lại,`take = min(a, n)`đảm bảo chúng tôi không bao giờ vượt quá năng lực. Vòng lặp kết thúc sớm nếu ba lô đầy. Những chi tiết này ngăn ngừa sự cố tràn hoặc lỗi từng cái một. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
7 3
5 10
2 5
3 6
```Theo dõi từng bước: 

| Vùng chứa (b, a) | n trước | lấy | Total_matches sau | n sau | 
| --- | --- | --- | --- | --- | 
| (10, 5) | 7 | 5 | 50 | 2 | 
| (6, 3) | 2 | 2 | 50 + 12 = 62 | 0 | 
| (5, 2) | 0 | 0 | 62 | 0 | 

Giải thích: Thùng thứ nhất giàu nhất, lấy 5 thùng. Thùng tiếp theo chỉ còn 2 hộp đựng vừa. Tổng số trận đấu = 62. 

Một trường hợp thử nghiệm khác: 

đầu vào:```
10 2
3 10
5 20
```| Vùng chứa (b, a) | n trước | lấy | Total_matches sau | n sau | 
| --- | --- | --- | --- | --- | 
| (20, 5) | 10 | 5 | 100 | 5 | 
| (10, 3) | 5 | 3 | 100 + 30 = 130 | 2 | 

Dung lượng còn lại (2) nhiều hơn bất kỳ hộp nào còn lại nên chúng tôi dừng lại. Tổng số trận đấu = 130. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log m) | Sắp xếp`m`container chiếm ưu thế; lần lặp là O(m) | 
| Không gian | O(m) | Lưu trữ danh sách container | 

Được cho`m ≤ 20`, việc sắp xếp là không đáng kể và thuật toán dễ dàng phù hợp với giới hạn thời gian 0 giây và giới hạn bộ nhớ 64 MB. Xử lý`n`tối đa 2·10^8 hoạt động vì chúng tôi không bao giờ lặp lại trên mỗi hộp diêm, chỉ trên mỗi vùng chứa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    n, m = map(int, input().split())
    containers = []
    for _ in range(m):
        a, b = map(int, input().split())
        containers.append((b, a))
    containers.sort(reverse=True)
    total_matches = 0
    for b, a in containers:
        if n == 0:
            break
        take = min(a, n)
        total_matches += take * b
        n -= take
    return str(total_matches)

# Provided sample
assert run("7 3\n5 10\n2 5\n3 6\n") == "62", "sample 1"

# Minimum input
assert run("1 1\n1 1\n") == "1", "minimum input"

# Large n, all containers smaller
assert run("10 2\n3 5\n2 10\n") == "35", "capacity larger than total boxes"

# All boxes same b
assert run("5 2\n3 7\n4 7\n") == "35", "all boxes same value"

# Single rich container exceeds n
assert run("4 1\n10 100\n") == "400", "single container larger than capacity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1\n1 1 | 1 | trường hợp cạnh đầu vào tối thiểu | 
| 10 2\n3 5\n2 10 | 35 | công suất lớn hơn tổng số hộp | 
| 5 2\n3 7\n4 7 | 35 | tất cả các hộp có giá trị bằng nhau | 
| 4 1\n10 100 | 400 | container lớn hơn dung tích chỉ lấy n hộp | 

## Vỏ cạnh 

Trường hợp thùng giàu nhất có nhiều hộp hơn`n`:```
n = 4, m = 1
10 100
```

`take = min(10, 4) = 4`,`total_matches = 4*100 = 400`. Đúng, không tính quá mức. 

Trường hợp tổng số hộp < n:```
n = 10, m = 2
3 5
2
```
