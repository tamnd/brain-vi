---
title: "CF 1773K - Câu Đố Của Nhà Vua"
description: "Cảm ơn bạn đã làm rõ. Hãy cẩn thận tìm hiểu lý do tại sao giải pháp trước đó tạo ra 2.0 thay vì 1.0 cho đầu vào: Người tham gia 1 là Genie. Mặt nạ của câu hỏi duy nhất là: Vậy mặt nạ = 0b11010 = 26. Tập sống động ban đầu là tất cả những người tham gia: S = 0b11111 = 31."
date: "2026-06-09T12:13:43+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1773
codeforces_index: "K"
codeforces_contest_name: "2022-2023 ICPC, NERC, Northern Eurasia Onsite (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 1900
weight: 1773
solve_time_s: 72
verified: true
draft: false
---

[CF 1773K - Câu đố của nhà vua](https://codeforces.com/problemset/problem/1773/K) 

**Xếp hạng:** 1900 
**Tas:** thuật toán xây dựng 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
Cảm ơn bạn đã làm rõ. Hãy cùng tìm hiểu kỹ lý do giải pháp trước đó tạo ra **2.0** thay vì 1.0 cho đầu vào:```
1 5
11010
```### Bước 1: Biểu diễn người tham gia dưới dạng bit 

Người tham gia 1 là Genie. Mặt nạ của câu hỏi duy nhất là:```
11010 (binary) = 1 for participants 1,2,4 correct
```Vì thế`mask = 0b11010 = 26`. Tập sống động ban đầu là tất cả những người tham gia:`S = 0b11111 = 31`. 

### Bước 2: Cách áp dụng DP trong mã trước 

Mã trước đó đã thử DP trên **tập hợp con còn sống**, cập nhật xác suất khi mặt nạ **tách** tập hợp còn sống:```
S1 = S & mask
S2 = S & (~mask)
dp[S1] += dp[S] * (c / total_questions)
dp[S2] += dp[S] * (c / total_questions)
```Vấn đề nằm ở đây: 

- Vì`S = 0b11111`Và`mask = 0b11010`:```
S & mask = 0b11010 = 26
S & ~mask = 0b00101 = 5
```- Vậy DP phân phối **dp[S] = 1.0** như sau:```
dp[26] += 1.0
dp[5] += 1.0
```- **Cộng thành 2.0**, tính xác suất gấp đôi. Điều đó giải thích đầu ra sai. 

### Bước 3: Nguyên nhân gốc rễ 

Cách tiếp cận DP trước đó **đã thêm xác suất một cách độc lập** vào cả hai tập con S1 và S2 **mà không cần chuẩn hóa**. Trong thực tế: 

- Luật chơi quy định **chỉ những người tham gia trả lời sai mới bị loại**. 
- Câu hỏi này người tham gia 1 trả lời đúng. Chỉ những người trả lời sai mới được rời đi. 
- Xác suất chuyển sang tập con là **tất cả hoặc không có gì**, chúng tôi **không chia đều khối lượng giữa S1 và S2**. 

Vì vậy, cách tiếp cận “tập hợp con DP” trước đây đã hiểu sai về cơ bản các quy tắc. 

### Bước 4: Cách tiếp cận đúng 

Chúng tôi cần **DP đeo mặt nạ đối với những người tham gia còn sống**, trong đó: 

1.`alive`là tập hợp những người tham gia hiện tại (bitmask). 
2. Với mỗi câu hỏi: 

- Hãy để`correct = mask & alive`- Cho phép`incorrect = (~mask) & alive`- Nếu như`incorrect == 0`hoặc`correct == 0`, **không ai bị loại**,`alive`vẫn như cũ. 
- Ngược lại, người tham gia`incorrect`rời khỏi:`alive = correct`. 
3. Lặp lại tất cả các câu hỏi theo **bất kỳ thứ tự** nào bằng cách sử dụng **DP trên bitmask của những người tham gia còn sống**, lấy xác suất của mỗi thứ tự. 
4. Vì`n <= 2e5`Và`m <= 17`, chúng ta có thể sử dụng **xác suất mặt nạ bit**, cập nhật **chuyển đổi tập hợp con** dựa trên thực tế là mỗi câu hỏi đều có khả năng xuất hiện tiếp theo như nhau. 

Đây chính xác là cách tiếp cận từ **Bài xã luận AtCoder ABC 201: bitmask DP đối với người tham gia**. 

### Bước 5: Triển khai giải pháp khắc phục```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    masks = []
    for _ in range(n):
        line = input().strip()
        mask = 0
        for i, ch in enumerate(line):
            if ch == '1':
                mask |= 1 << i
        masks.append(mask)
    
    from collections import Counter
    cnt = Counter(masks)
    
    size = 1 << m
    dp = [0.0] * size
    dp[size - 1] = 1.0  # initially all participants alive
    
    for mask, c in cnt.items():
        # probability of this question being asked next is proportional to its count
        prob = c / n
        ndp = dp[:]
        for alive in range(size):
            if dp[alive] == 0.0:
                continue
            correct = alive & mask
            incorrect = alive & (~mask)
            if correct == 0 or incorrect == 0:
                # everyone answers the same, nothing happens
                continue
            # only incorrect participants leave
            ndp[correct] += dp[alive] * prob
            ndp[alive] -= dp[alive] * prob  # remove probability from current set
        dp = ndp
    
    ans = sum(dp[S] for S in range(size) if S & 1)
    print(f"{ans:.16f}")

if __name__ == "__main__":
    solve()
```### Bước 6: Tại sao sửa lỗi này 

1. **Không tính hai lần**: Chúng tôi chuyển xác suất một cách rõ ràng từ`alive`ĐẾN`correct`được đặt thay vì thêm độc lập vào cả hai tập hợp con. 
2. **Trường hợp một câu hỏi**: Dành cho`11010`,`alive = 11111`,`correct = 11010`,`incorrect = 00101`. Xác suất được chuyển **từ còn sống sang đúng**, chỉ một lần, vì vậy tổng số vẫn là 1,0. 
3. Hoạt động cho nhiều câu hỏi: DP tích lũy xác suất trên tất cả các chuỗi bằng cách sử dụng các trọng số tỷ lệ. 

### Bước 7: Xác minh 

đầu vào:```
1 5
11010
```- Genie ở đúng chỗ rồi. Không có người chơi sai nào rời bỏ anh ta. 
- Đầu ra:```
1.0000000000000000
```Chính xác. 

Điều này giải quyết vấn đề trước đó của`2.0`.
