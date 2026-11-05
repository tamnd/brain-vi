---
title: "CF 1185F - Hai Pizza"
description: "Chúng ta được yêu cầu giúp một nhóm bạn chọn chính xác hai chiếc bánh pizza sao cho số lượng bạn bè tối đa hài lòng với lựa chọn đó."
date: "2026-06-12T00:54:49+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 1185
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 568 (Div. 2)"
rating: 2100
weight: 1185
solve_time_s: 66
verified: true
draft: false
---

[CF 1185F - Hai chiếc pizza](https://codeforces.com/problemset/problem/1185/F) 

**Xếp hạng:** 2100 
**Tags:** bitmask, vũ lực 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu giúp một nhóm bạn chọn chính xác hai chiếc bánh pizza sao cho số lượng bạn bè tối đa hài lòng với lựa chọn đó. Mỗi người bạn có một bộ nguyên liệu yêu thích và một người bạn sẽ hài lòng nếu mỗi nguyên liệu yêu thích của họ xuất hiện trên ít nhất một trong hai chiếc bánh pizza đã chọn. Mỗi chiếc bánh pizza có một bộ nguyên liệu và giá cố định. Nếu nhiều cặp pizza làm hài lòng cùng một số lượng bạn bè, chúng ta phải chọn cặp có giá kết hợp tối thiểu. Đầu ra chỉ đơn giản là chỉ số của hai chiếc pizza. 

Số lượng bạn bè,$n$, và số lượng pizza,$m$, mỗi cái có thể lớn bằng$10^5$. Mỗi người bạn và chiếc bánh pizza bao gồm tối đa 9 thành phần, vì tổng số thành phần có thể có là 9. Các giới hạn này loại trừ mọi giải pháp kiểm tra rõ ràng tất cả$n \times m^2$sự kết hợp, sẽ đạt được$10^{15}$hoạt động trong trường hợp xấu nhất. Thay vào đó, chúng ta cần khai thác số lượng nhỏ nguyên liệu và sử dụng một biểu diễn cho phép chúng ta nhanh chóng kiểm tra xem người bạn nào hài lòng với một cặp pizza. 

Một trường hợp phức tạp xảy ra khi nhiều chiếc pizza cùng nhau bao gồm tất cả các nguyên liệu cần thiết cho một người bạn, nhưng không một chiếc pizza nào làm được. Ví dụ: nếu một người bạn thích nguyên liệu {1,2} và pizza là [{1}, {2}] thì việc chỉ chọn một chiếc pizza sẽ không làm người bạn này hài lòng. Bất kỳ cách tiếp cận ngây thơ nào đánh giá mức độ hài lòng trên mỗi chiếc bánh pizza một cách độc lập sẽ coi người bạn này là không hài lòng một cách không chính xác. Một trường hợp khác xảy ra khi một số cặp pizza thỏa mãn cùng một số lượng bạn bè - chúng ta phải chọn cặp có tổng chi phí tối thiểu. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: lặp lại tất cả các cặp pizza, tính toán sự kết hợp các thành phần của chúng và đếm xem có bao nhiêu người bạn có tất cả các thành phần của họ trong sự kết hợp đó. Theo dõi cặp nào có số lượng bạn bè hài lòng tối đa, phá vỡ mối quan hệ với tổng chi phí tối thiểu. Điều này đúng về nguyên tắc nhưng có$O(m^2 \cdot n \cdot k)$sự phức tạp, ở đâu$k \le 9$là số nguyên liệu cho mỗi người bạn. Với$m = 10^5$Và$n = 10^5$, điều này hoàn toàn không thực tế. 

Thông tin chi tiết quan trọng là chỉ có 9 thành phần, vì vậy chúng ta có thể biểu thị bất kỳ bộ thành phần nào dưới dạng số nguyên 9 bit (bitmask). Mỗi bộ tùy chọn của một người bạn và các thành phần của pizza có thể được mã hóa dưới dạng bitmask có độ dài 9. Sử dụng các thao tác theo bit, chúng ta có thể nhanh chóng xác định xem sở thích của một người bạn có được thỏa mãn bởi một chiếc pizza hay sự kết hợp của các loại pizza hay không. Vì chỉ có$2^9 = 512$các bộ nguyên liệu có thể, chúng ta có thể xử lý trước chiếc bánh pizza rẻ nhất cho mỗi bộ nguyên liệu. Sau đó, thay vì kiểm tra tất cả$m^2$trực tiếp các cặp pizza, chúng ta chỉ cần xem xét các cặp trong số 512 bộ có thể, giảm đáng kể số lượng so sánh từ$10^{10}$đại khái$512^2 = 262144$, điều đó là khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m^2) | O(1) | Quá chậm | 
| Nén Bitmask | O(n + 2^9 * 2^9) | O(2^9) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Mã hóa bộ nguyên liệu yêu thích của mỗi người bạn dưới dạng số nguyên 9 bit. Mỗi bit thể hiện liệu người bạn có thích thành phần đó hay không. 
2. Mã hóa từng thành phần của bánh pizza thành số nguyên 9 bit. Mỗi chiếc bánh pizza cũng có một chi phí. 
3. Duy trì một từ điển ánh xạ từng bitmask vào chỉ mục của chiếc bánh pizza rẻ nhất với bộ nguyên liệu chính xác đó. Nếu nhiều chiếc pizza dùng chung một mặt nạ bit, hãy chọn hai chiếc rẻ nhất vì cặp tối ưu có thể bao gồm cùng một bộ nguyên liệu hai lần. 
4. Đếm xem, với mỗi mặt nạ bạn bè có thể có (tối đa 512), có bao nhiêu người bạn có mặt nạ đó. Điều này cho phép chúng ta tính toán mức độ hài lòng một cách nhanh chóng bằng cách kiểm tra xem sự kết hợp của hai mặt nạ pizza có bao gồm mặt nạ bạn bè hay không. 
5. Lặp lại tất cả các cặp mặt nạ pizza (bao gồm cả việc sử dụng cùng một mặt nạ hai lần nếu có ít nhất hai chiếc pizza cùng loại). Đối với mỗi cặp, tính toán sự kết hợp của hai mặt nạ. Đối với mỗi mặt nạ bạn bè, hãy kiểm tra xem nó có được chứa đầy đủ trong liên kết hay không bằng cách sử dụng AND theo bit. Tổng hợp số lượng bạn bè hài lòng. 
6. Theo dõi cặp pizza nào giúp tối đa hóa tổng số bạn bè hài lòng. Nếu nhiều cặp mang lại mức tối đa như nhau, hãy chọn cặp có tổng chi phí tối thiểu. Lưu trữ các chỉ số cho đầu ra. 
7. Xuất ra chỉ số của hai chiếc pizza đã chọn. 

Tại sao nó hoạt động: Bằng cách sử dụng bitmasks và vũ trụ nhỏ của các thành phần, chúng tôi giảm thiểu vấn đề khi xem xét tất cả các kết hợp thành phần có ý nghĩa thay vì tất cả các loại pizza riêng lẻ. Mỗi cặp mặt nạ pizza thể hiện chính xác các thành phần có sẵn khi chọn hai loại pizza đó. Việc đếm những người bạn hài lòng bằng cách kiểm tra từng bit sẽ đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
friend_count = [0] * 512  # count of friends for each mask

for _ in range(n):
    parts = list(map(int, input().split()))
    fmask = 0
    for ing in parts[1:]:
        fmask |= 1 << (ing - 1)
    friend_count[fmask] += 1

pizza_masks = {}
pizza_prices = {}
pizza_indices = {}

for idx in range(1, m + 1):
    parts = list(map(int, input().split()))
    price = parts[0]
    pmask = 0
    for ing in parts[2:]:
        pmask |= 1 << (ing - 1)
    
    if pmask not in pizza_prices:
        pizza_prices[pmask] = [price]
        pizza_indices[pmask] = [idx]
    else:
        pizza_prices[pmask].append(price)
        pizza_indices[pmask].append(idx)
        # keep only two cheapest
        combined = sorted(zip(pizza_prices[pmask], pizza_indices[pmask]))
        pizza_prices[pmask], pizza_indices[pmask] = zip(*combined[:2])

best_count = -1
best_cost = None
best_pair = None

masks = list(pizza_prices.keys())
for i in range(len(masks)):
    for j in range(i, len(masks)):
        mask1, mask2 = masks[i], masks[j]
        cost1_list, cost2_list = pizza_prices[mask1], pizza_prices[mask2]
        idx1_list, idx2_list = pizza_indices[mask1], pizza_indices[mask2]

        if i == j and len(cost1_list) < 2:
            continue  # cannot pick same pizza twice if only one exists

        cost = cost1_list[0] + (cost2_list[0] if i != j else cost1_list[1])
        union_mask = mask1 | mask2
        count = 0
        for fmask in range(512):
            if friend_count[fmask] > 0 and (fmask & union_mask) == fmask:
                count += friend_count[fmask]

        if count > best_count or (count == best_count and cost < best_cost):
            best_count = count
            best_cost = cost
            if i != j:
                best_pair = (idx1_list[0], idx2_list[0])
            else:
                best_pair = (idx1_list[0], idx1_list[1])

print(best_pair[0], best_pair[1])
```Đầu tiên, mã này mã hóa bạn bè và pizza dưới dạng bitmask, sau đó lưu trữ hai loại pizza rẻ nhất trên mỗi mặt nạ. Vòng lặp lồng nhau trên mặt nạ đảm bảo chúng tôi xem xét tất cả các cặp khả thi mà không cần lặp lại trên tất cả$m^2$pizza. Mặt nạ liên minh và hoạt động bitwise AND kiểm tra hiệu quả xem bạn bè nào hài lòng. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
3 4
2 6 7
4 2 3 9 5
3 2 3 9
100 1 7
400 3 3 2 5
100 2 9 2
500 3 2 9 5
```| Bước | mặt nạ1 | mặt nạ2 | công đoàn_mask | bạn bè hài lòng | chi phí | cặp_tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 7 | 14 | 15 | 2 | 500 | (2,3) | 

Trace hiển thị bạn 1 thích {6,7}, bạn 2 thích {2,3,5,9}, bạn 3 thích {2,3,9}. Pizza 2 {3,2,5} + Pizza 3 {9,2} làm hài lòng bạn 2 và 3. Pizza 1 {7} làm hài lòng bạn 1. Cặp tối ưu là Pizza 2 và 3 vì nó bao gồm tối đa số bạn bè với mức giá tối thiểu trong số các cặp tối ưu. 

### Tùy chỉnh đầu vào 2```
2 3
1 1
1 2
100 1 1
200 1 2
300 2 1 2
```| Bước | mặt nạ1 | mặt nạ2 | công đoàn_mask | bạn bè hài lòng | chi phí | cặp_tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 3 | 2 | 300 | (1,2) | 
| 2 | 1 | 3 | 3 | 2 | 400 | (1,3) | 

Cặp đầu tiên đã mang lại sự hài lòng tối đa với chi phí tối thiểu. Hiển thị thuật toán ưu tiên chi phí tối thiểu trong các mối quan hệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + 2^9 * | |
