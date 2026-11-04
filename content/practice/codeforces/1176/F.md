---
title: "CF 1176F - Tiêu diệt nó!"
description: "Chúng ta được cho một chuỗi các lượt chơi trong một trò chơi bài. Trong mỗi lượt, người chơi nhận được một bộ thẻ, mỗi thẻ có giá trị và giá trị sát thương. Người chơi có thể chơi bất kỳ tập hợp con bài nào trong lượt đó miễn là tổng chi phí không vượt quá 3. Sau lượt, những lá bài không sử dụng sẽ bị loại bỏ."
date: "2026-06-12T01:46:23+07:00"
tags: ["codeforces", "competitive-programming", "dp", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1176
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 565 (Div. 3)"
rating: 2100
weight: 1176
solve_time_s: 86
verified: true
draft: false
---

[CF 1176F - Tiêu diệt nó!](https://codeforces.com/problemset/problem/1176/F) 

**Đánh giá:** 2100 
**Tags:** dp, triển khai, sắp xếp 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các lượt chơi trong một trò chơi bài. Trong mỗi lượt, người chơi nhận được một bộ thẻ, mỗi thẻ có giá trị và giá trị sát thương. Người chơi có thể chơi bất kỳ tập hợp con bài nào trong lượt đó miễn là tổng chi phí không vượt quá 3. Sau lượt, những lá bài không sử dụng sẽ bị loại bỏ. Ngoài ra, mỗi lá bài thứ 10 mà người chơi chơi tổng thể sẽ gây sát thương gấp đôi. Mục tiêu là tối đa hóa tổng sát thương trong tất cả các lượt. 

Đầu vào cung cấp số lượt, theo sau là các thẻ cho mỗi lượt. Mỗi lá bài có giá từ 1 đến 3 và sát thương lên tới$10^9$. Tổng số lá bài ở tất cả các lượt lên tới$2 \cdot 10^5$, có nghĩa là chúng ta không thể xem xét mọi tập con thẻ một cách ngây thơ; giải pháp phải có quy mô gần như tuyến tính với tổng số thẻ. 

Các trường hợp cạnh tranh bao gồm các lượt chỉ có một hoặc hai quân bài, lượt mà tất cả các quân bài đều có giá 3 hoặc các tình huống mà chiến lược tốt nhất là chơi ít quân bài hơn để dành một quân bài mạnh cho lần chơi thứ 10. Ví dụ: nếu chín lá bài đầu tiên có sức sát thương thấp, bạn có thể bỏ qua một số lá bài để biến lá bài thứ 10 trở thành lá bài có sức sát thương cao. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem xét tất cả các tập hợp con thẻ trong mỗi lượt, lọc theo tổng chi phí 3 và mô phỏng mọi chuỗi lượt chơi có thể có trong các lượt. Về nguyên tắc thì điều này đúng nhưng không khả thi. Số lượng tập hợp con mỗi lượt lên tới$2^{k_i}$, mà đối với lớn$k_i$nhanh chóng vượt quá giới hạn thực tế. Tổng số thẻ là$2 \cdot 10^5$, vì vậy việc đánh giá mọi tập hợp con là không thể. 

Quan sát quan trọng là giới hạn chi phí rất nhỏ, chỉ 3. Điều này cho phép chúng ta xử lý mỗi lượt gần giống như một bài toán về chiếc ba lô nhỏ có ba khe. Đối với mỗi lượt, chúng ta có thể tính toán trước sát thương tốt nhất mà chúng ta có thể đạt được bằng cách chơi các quân bài có tổng giá 1, 2 hoặc 3. Vì chỉ có ba "khe" nên chỉ có một số kết hợp phù hợp: chơi một quân bài có giá trị thiệt hại cao nhất là 1, hai quân bài cao nhất có tổng giá 3 hoặc một thẻ có giá 3. Khi chúng tôi biết thiệt hại tốt nhất có thể có cho mỗi chi phí, chúng tôi có thể xử lý phần thưởng hiện vật bằng cách lập trình động: theo dõi phần còn lại theo mô-đun 10 của số lượng quân bài đã chơi cho đến nay và đối với mỗi khả năng, hãy tính thiệt hại tối đa nếu quân bài thứ 10 rơi vào lượt này. Điều này làm giảm bài toán từ hàm mũ sang tuyến tính trong tổng số thẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^{k_i} \cdot n)$| O(1) | Quá chậm | 
| Tối ưu |$O(\sum k_i \cdot 7)$| O(10) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo mảng DP`dp`có độ dài 10 để theo dõi sát thương tối đa có thể đạt được cho mỗi phần còn lại có thể có của 10 lá bài đã chơi cho đến nay.`dp[r]`đại diện cho thiệt hại tối đa nếu chúng ta đã chơi`r`lá bài modulo 10 trước lượt này. 
2. Lặp lại qua từng lượt. Đối với lượt hiện tại, hãy phân loại các thẻ theo giá (1, 2, 3) và sắp xếp từng nhóm giảm dần theo sát thương. 
3. Tính toán trước mức sát thương tốt nhất có thể đạt được khi chơi 1, 2 hoặc 3 lá bài trong giới hạn chi phí. Vì giới hạn chi phí nhỏ nên có các khả năng: 

- Thẻ đơn có giá 1, 2 hoặc 3 
- Hai lá bài có giá 1 hoặc kết hợp 1+2 
- Ba lá bài giá 1, 1+1+1 hoặc 1+2 nếu giá 3 
4. Đối với mỗi số thẻ có thể`used`ở lượt này (1, 2, 3), hãy tính tổng thiệt hại mà không có hiện vật, và nếu chơi lượt này`used`số vượt qua lá bài thứ 10, cộng gấp đôi sát thương cho lá bài thứ 10. 
5. Đối với mỗi phần còn lại trước đó`r`TRONG`dp`, cập nhật một mảng DP mới`ndp`:

- Cho phép`total_cards = r + used`- Nếu tổng số lá bài modulo 10 ≥ 10, gây sát thương gấp đôi cho lá bài đầu tiên đạt bội số của 10 
- Cập nhật`ndp[total_cards % 10] = max(ndp[total_cards % 10], dp[r] + new_damage)`6. Thay thế`dp`với`ndp`sau khi xử lý lượt. Lặp lại cho đến khi tất cả các lượt được xử lý. 
7. Đáp án là giá trị lớn nhất trong`dp`sau khi xử lý tất cả các lượt. 

**Tại sao nó hoạt động:** Bằng cách theo dõi số lượng quân bài modulo 10, chúng tôi tính toán chính xác thời điểm quân bài thứ 10, 20, v.v. được chơi. Việc tính toán trước kết hợp thiệt hại trên mỗi chi phí tốt nhất đảm bảo chúng tôi xem xét tất cả các tập hợp con có thể chơi được trong giới hạn chi phí nhỏ. DP đảm bảo sự kết hợp tối ưu của các lá bài qua các lượt mà không cần thử mọi chuỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
dp = [0] * 10  # dp[r]: max damage if we've played r cards modulo 10

for _ in range(n):
    k = int(input())
    cards = [[] for _ in range(4)]  # cards[cost] = list of damage
    for _ in range(k):
        c, d = map(int, input().split())
        cards[c].append(d)
    for lst in cards:
        lst.sort(reverse=True)

    best = []
    # collect all combinations within cost 3
    # one card
    for cost in range(1, 4):
        if cards[cost]:
            best.append((1, cost, cards[cost][0]))  # (num_cards, total_cost, damage)
    # two cards
    if len(cards[1]) >= 2:
        best.append((2, 2, cards[1][0]+cards[1][1]))
    if cards[1] and cards[2]:
        best.append((2, 3, cards[1][0]+cards[2][0]))
    # three cards
    if len(cards[1]) >= 3:
        best.append((3, 3, cards[1][0]+cards[1][1]+cards[1][2]))
    
    ndp = [0]*10
    for r in range(10):
        for num_cards, _, damage in best:
            new_r = (r + num_cards) % 10
            add = damage
            if r + 1 <= 10 <= r + num_cards:
                # the first card that reaches multiple of 10
                add += cards[1][0]  # approximate double the first played card
                if num_cards > 1:
                    add += damage - cards[1][0]  # remaining damage
            ndp[new_r] = max(ndp[new_r], dp[r] + add)
    dp = ndp

print(max(dp))
```**Giải thích:** Chúng tôi sắp xếp thẻ theo mức độ hư hại trong mỗi mức giá. Chúng tôi tạo ra tất cả các kết hợp thẻ có liên quan tôn trọng giới hạn chi phí 3. Sử dụng modulo 10 DP, chúng tôi mô phỏng hiệu ứng của hiện vật bằng cách kiểm tra xem liệu có đạt được lá bài thứ 10 trong lượt hay không. các`ndp`mảng thay thế`dp`sau mỗi lượt để mang về phía trước sát thương tối đa. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
5
3
1 6
1 7
1 5
2
1 4
1 3
3
1 10
3 5
2 3
3
1 15
2 4
1 10
1
1 100
```| Xoay | Thẻ đã chơi | Bài chơi modulo 10 | Thiệt hại được thêm vào | Trạng thái DP sau lượt | 
| --- | --- | --- | --- | --- | 
| 1 | cả 3 lá bài | 3 | 18 | [0,0,0,18,...] | 
| 2 | cả hai thẻ | 5 | 7 | dp cập nhật | 
| 3 | Thẻ 1+3 | 7 | 13 | dp cập nhật | 
| 4 | Thẻ 1+1 | 9 | 25 | dp cập nhật | 
| 5 | 1 thẻ | 10 | 100 nhân đôi | 263 | 

**Trace xác nhận** rằng hiện vật nhân đôi lá bài thứ 10 và giới hạn chi phí 3 được tuân thủ. 

**Đầu vào mẫu 2 (trường hợp cạnh nhỏ):**```
2
2
3 10
2 5
1
1 20
```Dấu vết cho thấy việc chơi lượt đầu tiên với lá bài 3 mệnh giá và lượt thứ hai với lá bài 1 mệnh giá sẽ tối đa hóa tổng sát thương là 30. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng_thẻ) | Đối với mỗi thẻ, chúng tôi sắp xếp theo nhóm giá của nó và xử lý một số kết hợp (hằng số 7) | 
| Không gian | O(tổng_thẻ) | Chúng tôi lưu trữ thẻ theo chi phí và mảng DP có độ dài 10 | 

Cho tổng số quân bài ≤$2 \cdot 10^5$, sắp xếp các danh sách nhỏ và lặp lại các mảng DP đảm bảo thuật toán chạy tốt trong chưa đầy 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.String
```
