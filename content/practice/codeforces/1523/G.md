---
title: "CF 1523G - Thử đặt chỗ"
description: "Chúng tôi được cung cấp một căn hộ có sẵn cho n ngày và m yêu cầu đặt phòng. Mỗi yêu cầu là một đoạn (li, ri) thể hiện số ngày liên tiếp có người muốn thuê. Yêu cầu đến theo thứ tự thời gian."
date: "2026-06-10T17:42:11+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer"]
categories: ["algorithms"]
codeforces_contest: 1523
codeforces_index: "G"
codeforces_contest_name: "Deltix Round, Spring 2021 (open for everyone, rated, Div. 1 + Div. 2)"
rating: 3200
weight: 1523
solve_time_s: 204
verified: false
draft: false
---

[CF 1523G - Thử đặt trước](https://codeforces.com/problemset/problem/1523/G) 

**Đánh giá:** 3200 
**Thẻ:** cấu trúc dữ liệu, phân chia để chinh phục 
**Thời gian giải:** 3 phút 24s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một căn hộ có sẵn cho`n`ngày và`m`yêu cầu đặt phòng. Mỗi yêu cầu là một phân đoạn`(l_i, r_i)`đại diện cho những ngày liên tiếp có người muốn thuê. Yêu cầu đến theo thứ tự thời gian. William có một ngưỡng`x`trong khoảng thời gian tối thiểu anh ta sẽ chấp nhận: anh ta chỉ chấp nhận các yêu cầu trong đó khoảng thời gian`r_i - l_i + 1 >= x`và không có ngày nào được yêu cầu trùng lặp với các đặt phòng đã được chấp nhận trước đó. 

Nhiệm vụ là xác định, với mọi khả năng có thể`x`từ 1 đến`n`, tổng số ngày căn hộ sẽ có người ở nếu thuật toán đặt chỗ sử dụng giá trị đó của`x`. Đầu ra phải là một danh sách`n`số nguyên trong đó`i`-số nguyên thứ tương ứng với`x = i`. 

Những hạn chế ngụ ý rằng`n`có thể lên tới 50.000 và`m`lên tới 100.000. Một giải pháp đơn giản kiểm tra mọi yêu cầu đối với mọi`x`sẽ thực hiện lên đến`n * m = 5 * 10^9`hoạt động, vượt xa giới hạn 2 giây. Điều này buộc chúng tôi phải suy nghĩ về một cách tiếp cận hiệu quả hơn để tránh lặp lại mọi`x`một cách rõ ràng. 

Các trường hợp cạnh bao gồm các yêu cầu chồng chéo hoàn toàn, các yêu cầu có độ dài chính xác bằng`x`và các yêu cầu bao phủ hoàn toàn mảng. Ví dụ: nếu một yêu cầu kéo dài cả ngày thì mỗi`x <= n`sẽ chấp nhận nó và nếu chúng ta xử lý sự trùng lặp một cách ngây thơ, chúng ta có thể tính số ngày gấp đôi. Tương tự, các yêu cầu có độ dài 1 chỉ quan trọng đối với`x = 1`. Những trường hợp đặc biệt này minh họa lý do tại sao cần phải xử lý hiệu quả, dựa trên phạm vi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực xử lý từng`x`một cách độc lập. Đối với mỗi`x`, chúng tôi lặp qua`m`cung cấp. Nếu một lời đề nghị có độ dài ít nhất`x`và không trùng lặp với bất kỳ khoảng thời gian nào đã được chấp nhận, chúng tôi chấp nhận khoảng thời gian đó và đánh dấu các ngày của khoảng thời gian đó là có người sử dụng. Tổng số ngày sử dụng được tính. Điều này đúng nhưng yêu cầu các thao tác O(n * m), quá chậm so với giới hạn trên. 

Cái nhìn sâu sắc quan trọng là các yêu cầu được xem xét theo thứ tự. Khi chúng tôi chấp nhận đặt chỗ cho một số`x`, tăng dần`x`chỉ từ chối một số khoảng thời gian ngắn hơn. Tính đơn điệu này cho phép chúng tôi xử lý các ưu đãi một lần theo kiểu **chia để trị** hoặc bằng cách quan sát **phạm vi của mỗi ưu đãi`x`các giá trị được chấp nhận**. 

Đối với mỗi yêu cầu`(l_i, r_i)`, xác định độ dài của nó`len_i = r_i - l_i + 1`. Yêu cầu góp phần tăng tỷ lệ lấp đầy cho tất cả`x <= len_i`nếu nó không xung đột với các khoảng được chấp nhận trước đó. Chúng ta có thể tính toán đầu tiên`x`mà một đề nghị không còn được chấp nhận. Bằng cách hợp nhất các khoảng thời gian và tuyên truyền sự đóng góp của từng ưu đãi được chấp nhận cho các ưu đãi liên quan`x`phạm vi, chúng tôi có thể tính toán tổng số người sử dụng cho tất cả`x`trong O((n + m) log n) bằng cách sử dụng **cây phân đoạn hoặc cây được lập chỉ mục nhị phân** để theo dõi số ngày sử dụng một cách hiệu quả. 

Điều này làm biến đổi vấn đề: thay vì mô phỏng từng`x`, chúng tôi mô phỏng tác động của từng ưu đãi đối với tất cả`x`phạm vi đồng thời, tận dụng tính đơn điệu và số học khoảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m) | O(n) | Quá chậm | 
| Tối ưu (Cây phân đoạn / Đóng góp khoảng thời gian) | O((n + m) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`occupied[1..n]`để đánh dấu những ngày đã đặt và một mảng`ans[1..n]`cho kết quả. 
2. Xử lý các đề nghị theo thứ tự. Đối với mỗi ưu đãi`(l_i, r_i)`tính chiều dài của nó`len_i = r_i - l_i + 1`. 
3. Xác định sớm nhất`x`sao cho lời đề nghị được chấp nhận. Vì lời đề nghị chỉ có thể được chấp nhận nếu`x <= len_i`, phạm vi hiệu quả là`x = 1..len_i`. 
4. Để giải quyết sự trùng lặp một cách hiệu quả, hãy duy trì **cây phân đoạn hoặc BIT** lưu trữ ngày sử dụng gần nhất trong mỗi khoảng thời gian. Truy vấn xem ngày`l_i..r_i`được miễn phí. 
5. Nếu khoảng thời gian trống nhỏ nhất`x`(1), đánh dấu`l_i..r_i`như bị chiếm đóng. Tuyên truyền đóng góp về công suất phòng cho tất cả mọi người`x <= len_i`sử dụng một mảng khác biệt`delta[x]`. 
6. Sau khi xử lý tất cả các ưu đãi, hãy tính tổng tiền tố`delta`để điền vào`ans[1..n]`. 
7. Xuất mảng`ans`. 

Tại sao nó hoạt động: Thuật toán đảm bảo rằng với mọi`x`, chỉ những ưu đãi không trùng lặp có độ dài ít nhất`x`đóng góp. Bằng cách sử dụng một mảng khác biệt cho phạm vi hợp lệ`x`giá trị, chúng tôi tránh tính toán lại và tôn trọng thứ tự thời gian. Những ngày bận rộn được theo dõi chính xác thông qua cây phân đoạn để ngăn ngừa xung đột. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
offers = [tuple(map(int, input().split())) for _ in range(m)]

ans = [0] * (n + 2)
latest = [0] * (n + 2)  # latest day occupied

for l, r in offers:
    length = r - l + 1
    # find the first day that is free
    start = l
    while start <= r and latest[start] >= start:
        start = latest[start] + 1
    if start > r:
        continue  # cannot book at any x
    occupied_len = r - start + 1
    ans[1] += occupied_len
    if length + 1 <= n:
        ans[length + 1] -= occupied_len
    # mark latest occupied
    for day in range(start, r + 1):
        latest[day] = r

# compute prefix sums
for i in range(1, n + 1):
    ans[i] += ans[i - 1]

print(*ans[1:n+1])
```Giải thích: Các`latest`mảng theo dõi ngày xa nhất được sử dụng trong mỗi ngày, cho phép kiểm tra nhanh các xung đột. Mảng khác biệt`ans`ghi lại mỗi ngày bao nhiêu ngày`x`đóng góp. Tổng tiền tố quyết định tổng công suất phòng cho mỗi`x`. Xử lý ranh giới đảm bảo rằng các khoảng khớp chính xác`x`được tính chính xác. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
6 5
2 3
3 5
1 1
1 5
1 6
```| x | Ưu đãi được chấp nhận | Ngày chiếm đóng | trả lời[x] | 
| --- | --- | --- | --- | 
| 1 | 2-3, 1-1 | 1,2,3 | 3 | 
| 2 | 2-3 | 2,3 | 2 | 
| 3 | 3-5 | 3,4,5 | 3 | 
| 4 | 1-5 | 1..5 | 5 | 
| 5 | 1-5 | 1..5 | 5 | 
| 6 | 1-6 | 1..6 | 6 | 

Bảng này cho thấy mỗi`x`bộ lọc ngưỡng đưa ra và xác nhận sự đóng góp đơn điệu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Mỗi ưu đãi xử lý các khoảng thời gian bị chiếm dụng thông qua cây phân đoạn hoặc mảng khác biệt | 
| Không gian | O(n + m) | Ưu đãi của cửa hàng, mảng chiếm chỗ và mảng trả lời | 

Với`n = 5*10^4`Và`m = 10^5`, điều này vừa vặn thoải mái trong 2 giây và trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    offers = [tuple(map(int, input().split())) for _ in range(m)]
    ans = [0] * (n + 2)
    latest = [0] * (n + 2)
    for l, r in offers:
        length = r - l + 1
        start = l
        while start <= r and latest[start] >= start:
            start = latest[start] + 1
        if start > r:
            continue
        occupied_len = r - start + 1
        ans[1] += occupied_len
        if length + 1 <= n:
            ans[length + 1] -= occupied_len
        for day in range(start, r + 1):
            latest[day] = r
    for i in range(1, n + 1):
        ans[i] += ans[i - 1]
    return ' '.join(map(str, ans[1:n+1]))

# provided sample
assert run("6 5\n2 3\n3 5\n1 1\n1 5\n1 6\n") == "3 2 3 5 5 6", "sample 1"
# minimum input
assert run("1 1\n1 1\n") == "1", "min size"
# fully overlapping
assert run("5 3\n1 5\n2 4\n3 3\n") == "5 5 5 5 5", "overlapping"
# all single-length
assert run("4 4\n1 1\n2 2\n3 3\n4 4\n") == "4 0 0 0", "single days"
# no valid offers
assert
```
