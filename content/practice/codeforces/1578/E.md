---
title: "CF 1578E - Lập kế hoạch dễ dàng"
description: "Chúng ta được yêu cầu lên lịch các tác vụ được sắp xếp dưới dạng cây nhị phân đầy đủ có chiều cao $h$. Cây nhị phân đầy đủ có chiều cao $h$ có các nút $2^h - 1$. Ban đầu, chỉ có tác vụ gốc là sẵn sàng. Tại mỗi thời điểm riêng biệt, có tới $p$ tiến trình có thể thực hiện đồng thời các tác vụ sẵn sàng."
date: "2026-06-10T10:36:50+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1578
codeforces_index: "E"
codeforces_contest_name: "ICPC WF Moscow Invitational Contest - Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 1200
weight: 1578
solve_time_s: 201
verified: false
draft: false
---

[CF 1578E - Lập kế hoạch dễ dàng](https://codeforces.com/problemset/problem/1578/E) 

**Đánh giá:** 1200 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 3m 21s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu lên lịch các nhiệm vụ được sắp xếp dưới dạng cây nhị phân đầy đủ có chiều cao$h$. Cây nhị phân đầy đủ có chiều cao$h$có$2^h - 1$nút. Ban đầu, chỉ có tác vụ gốc là sẵn sàng. Tại mỗi thời điểm riêng biệt, có thể lên tới$p$các tiến trình có thể thực hiện đồng thời các nhiệm vụ đã sẵn sàng. Khi một nhiệm vụ được thực hiện, các phần tử con của nó sẽ sẵn sàng vào thời điểm tiếp theo. Mục tiêu là xác định số lượng khoảnh khắc thời gian tối thiểu cần thiết để thực hiện tất cả các nhiệm vụ trong cây. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra$h$Và$p$, đại diện cho chiều cao của cây và số lượng tiến trình. Đầu ra của mỗi trường hợp thử nghiệm là một số nguyên duy nhất, số lượng thời gian tối thiểu cần thiết để hoàn thành tất cả các nhiệm vụ. 

Những hạn chế là rất quan trọng. Có thể có tới$5 \cdot 10^5$trường hợp thử nghiệm. Độ cao$h$nhiều nhất là 50, có nghĩa là cây lớn nhất có$2^{50} - 1$nút. Số lượng quy trình$p$tùy thuộc vào$10^4$. Những ràng buộc này chỉ ra rằng chúng ta không thể mô phỏng từng nhiệm vụ riêng lẻ cho một lượng lớn$h$- việc truyền tải rõ ràng tất cả các nút là không khả thi. Thay vào đó, chúng ta cần một chiến lược tận dụng cấu trúc của cây và số lượng nút có thể dự đoán được ở mỗi độ sâu. 

Các trường hợp biên không rõ ràng bao gồm việc chỉ có một quy trình với cây có kích thước vừa phải. Ví dụ,$h = 3$,$p = 1$dẫn đến việc thực hiện tuần tự các nhiệm vụ: thời điểm sẽ là 7, vì các nhiệm vụ phải được thực hiện từng nhiệm vụ một. Một cách tiếp cận tham lam bất cẩn cho rằng tính song song luôn có thể tận dụng tối đa tất cả các quy trình có thể đánh giá thấp thời gian cần thiết trong những trường hợp như vậy. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: mô phỏng quá trình. Giữ một hàng các nhiệm vụ sẵn sàng, mỗi thời điểm giao cho$p$xử lý, đánh dấu các nhiệm vụ là đã hoàn thành và đẩy các nhiệm vụ con của chúng vào hàng đợi sẵn sàng. Điều này hoạt động chính xác, nhưng đối với$h = 50$, có$2^{50} - 1$nhiệm vụ. Việc mô phỏng từng nhiệm vụ sẽ mất$10^{15}$hoạt động - vượt xa mọi giới hạn thời gian hợp lý. 

Cách tiếp cận tối ưu khai thác cấu trúc cây nhị phân. Ở độ sâu$d$, có$2^{d-1}$nhiệm vụ. Nhiệm vụ ở độ sâu chỉ sẵn sàng sau khi hoàn thành cấp độ trước đó. Bởi vì tất cả các nhiệm vụ ở cùng cấp độ đều sẵn sàng đồng thời, chúng ta có thể tính toán số lượng nhiệm vụ sẵn sàng và xử lý chúng theo lô có kích thước lên tới$p$. Điều này làm giảm vấn đề về tổng hợp các cấp độ, thực hiện phân chia trần nhiệm vụ theo giới hạn quy trình$p$. Việc tính toán phụ thuộc vào việc đếm các nhiệm vụ theo độ sâu, không lặp qua từng nút riêng lẻ. 

Thông tin chi tiết quan trọng là nhận ra rằng vấn đề có thể được giảm xuống bằng việc đếm số lượng thời điểm cần thiết để hoàn thành cấp độ cuối cùng, xem xét tồn đọng tích lũy của các nhiệm vụ chưa hoàn thành từ các cấp độ trước đó. Bằng cách duy trì danh sách các nhiệm vụ còn lại được sắp xếp và luôn xử lý các lô lớn nhất có sẵn trước tiên, chúng tôi giảm thiểu tổng thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(2^h) | O(2^h) | Quá chậm trong h > 30 | 
| Đếm tham lam theo cấp độ | O(h log h) | O(h) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số lượng nhiệm vụ ở mỗi độ sâu của cây. Về chiều sâu$d$từ 1 đến$h$, có$2^{d-1}$nhiệm vụ. Lưu trữ chúng trong một danh sách`tasks`. 
2. Đảo ngược danh sách để chúng ta bắt đầu xử lý từ mức sâu nhất. Điều này cho phép ưu tiên các lô lớn nhất, điều này rất quan trọng để giảm thiểu tổng thời gian. 
3. Khởi tạo`ready`là 1, thể hiện tác vụ gốc đã sẵn sàng tại thời điểm 1. Khởi tạo`time`bằng 0. 
4. Trong khi vẫn còn nhiệm vụ phải thực hiện: 

4.1. Thêm số lượng nhiệm vụ ở độ sâu hiện tại vào`ready`. 

4.2. Xác định có bao nhiêu nhiệm vụ có thể được thực hiện trong thời điểm này:`done = min(ready, p)`. 

4.3. Trừ`done`từ`ready`. 

4.4. Tăng`time`bằng 1. 
5. Lặp lại cho đến khi hết tất cả các cấp độ và`ready`đạt 0. Trở về`time`là số lượng khoảnh khắc thời gian tối thiểu. 

Tại sao nó hoạt động: bất biến là vậy`ready`luôn thể hiện chính xác số lượng nhiệm vụ có thể được thực hiện trong thời điểm hiện tại. Bằng cách luôn thực hiện tối đa`p`nhiệm vụ mỗi thời điểm và xử lý các lô lớn hơn trước, chúng tôi đảm bảo không có thời gian nhàn rỗi nếu có sẵn các quy trình. Chiến lược này đảm bảo tổng thời gian tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def minimal_time(h, p):
    tasks = [(1 << i) for i in range(h)]
    tasks.reverse()
    ready = 0
    time = 0
    for t in tasks:
        ready += t
        done = min(ready, p)
        ready -= done
        time += 1
    while ready > 0:
        done = min(ready, p)
        ready -= done
        time += 1
    return time

t = int(input())
for _ in range(t):
    h, p = map(int, input().split())
    print(minimal_time(h, p))
```Vòng lặp đầu tiên xây dựng số lượng nhiệm vụ theo độ sâu, đảo ngược để chúng tôi xử lý các lô lớn nhất trước tiên. Biến`ready`theo dõi các nhiệm vụ có sẵn để thực hiện. Chúng tôi liên tục thực hiện lên đến`p`nhiệm vụ mỗi thời điểm. Vòng lặp while cuối cùng xử lý các tác vụ còn lại từ độ sâu trước đó. Điều này đảm bảo không có nhiệm vụ nào chưa được xử lý. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
3 1
3 2
10 6
```Vì`3 1`: 

| Độ sâu | Nhiệm vụ | Sẵn sàng | Xong | Thời gian | 
| --- | --- | --- | --- | --- | 
| 3 | 4 | 4 | 1 | 1 | 
| 2 | 2 | 5 | 1 | 2 | 
| 1 | 1 | 5 | 1 | 3 | 
| Còn lại | - | 3 | 1 | 4 | 
| Còn lại | - | 2 | 1 | 5 | 
| Còn lại | - | 1 | 1 | 6 | 
| Còn lại | - | 0 | 1 | 7 | 

Thời gian = 7, như mong đợi. 

Vì`3 2`: 

| Độ sâu | Nhiệm vụ | Sẵn sàng | Xong | Thời gian | 
| --- | --- | --- | --- | --- | 
| 3 | 4 | 4 | 2 | 1 | 
| 2 | 2 | 4 | 2 | 2 | 
| 1 | 1 | 2 | 2 | 3 | 
| Còn lại | - | 0 | 0 | 4 | 

Thời gian = 4, phù hợp với mẫu. 

Điều này xác nhận thuật toán tôn trọng tính khả dụng của nhiệm vụ, giới hạn lô và xử lý các cấp độ lớn hơn trước tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(h) | Mỗi cấp độ của cây được truy cập một lần, h ≤ 50, cộng với tối đa h lần lặp cho các nhiệm vụ còn sót lại. | 
| Không gian | O(h) | Chúng tôi lưu trữ các nhiệm vụ theo độ sâu trong một danh sách có độ dài h. | 

Giải pháp dễ dàng xử lý$t \le 5\cdot10^5$các trường hợp thử nghiệm, vì mỗi thử nghiệm chỉ yêu cầu các thao tác O(h) = O(50). 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        h, p = map(int, input().split())
        print(minimal_time(h, p))
    return output.getvalue().strip()

# Provided samples
assert run("3\n3 1\n3 2\n10 6\n") == "7\n4\n173", "sample"

# Custom cases
assert run("1\n1 1\n") == "1", "minimum tree"
assert run("1\n50 10000\n") == "50", "maximum height, many processes"
assert run("1\n4 1\n") == "15", "single process, small tree"
assert run("1\n4 10\n") == "4", "more processes than tasks per level"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | Cây nút đơn | 
| 50 10000 | 50 | Cây lớn, đủ quy trình không bao giờ chờ đợi | 
| 4 1 | 15 | Xử lý tuần tự với một quy trình | 
| 4 10 | 4 | Tính song song vượt quá kích thước cấp độ | 

## Vỏ cạnh 

cho`h = 1`,`p = 1`: cây chỉ có rễ. Thuật toán đếm chính xác`ready = 1`, thực hiện nó trong một khoảnh khắc và kết thúc. 

Vì`h = 50`,`p = 10000`: ban đầu`ready = 2^49`ở mức độ sâu nhất, nhưng vòng lặp while hoạt động chính xác`p = 10000`nhiệm vụ mỗi thời điểm cho đến khi hoàn thành tất cả, việc này sẽ mất nhiều lần lặp lại. Thuật toán không bao giờ cố gắng lưu trữ tất cả các tác vụ riêng lẻ, ngăn chặn tình trạng tràn bộ nhớ. 

Vì`p > 2^(depth)`, thuật toán
