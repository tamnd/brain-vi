---
title: "CF 1571J - Hai Đường Sắt"
description: "Vấn đề không phải là một lỗi triển khai nhỏ. Toàn bộ cách tiếp cận thuật toán về cơ bản là sai. Hãy theo dõi mẫu bác bỏ ngay lập tức: Thuật toán đề xuất nhìn thấy ít nhất một '1', sau đó chỉ kiểm tra ký tự đầu tiên và cuối cùng: - s[0] = ')' → cần lật -…"
date: "2026-06-10T11:33:46+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 1571
codeforces_index: "J"
codeforces_contest_name: "Kotlin Heroes: Episode 8"
rating: 3000
weight: 1571
solve_time_s: 562
verified: false
draft: false
---

[CF 1571J - Hai tuyến đường sắt](https://codeforces.com/problemset/problem/1571/J) 

**Đánh giá:** 3000 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 9m 22s 
**Đã xác minh:** không 

##Giải pháp 
Vấn đề không phải là một lỗi triển khai nhỏ. Toàn bộ cách tiếp cận thuật toán về cơ bản là sai. 

Hãy theo dõi mẫu ngay lập tức bác bỏ nó:```
n = 6
s = ))(()(
a = 101
```Thuật toán đề xuất nhìn thấy ít nhất một`'1'`, sau đó chỉ kiểm tra ký tự đầu tiên và cuối cùng: 

-`s[0] = ')'`→ cần lật 
-`s[5] = '('`→ cần lật 

Nó trở lại`2`. 

Nhưng câu trả lời đúng là`4`. 

Tại sao? Bởi vì các ràng buộc là về **mọi cửa sổ có độ dài-4 có bit tương ứng trong`a`là 1**. 

Đây: 

- Cửa sổ`[0..3]`phải là một chuỗi dấu ngoặc thông thường. 
- Cửa sổ`[2..5]`phải là một chuỗi dấu ngoặc thông thường. 

Những cửa sổ đó chồng lên nhau. Tính cách bên trong của họ rất quan trọng. Việc sửa ký tự đầu tiên và ký tự cuối cùng không đảm bảo cửa sổ nào hợp lệ. 

Trên thực tế, toàn bộ tuyên bố của bài xã luận rằng "chỉ ký tự đầu tiên và ký tự cuối cùng là quan trọng" là sai. 

Giải pháp thực sự cho CF 1571E sử dụng lập trình động trên cửa sổ trượt 4 vị trí. Vì mỗi ràng buộc chỉ liên quan đến 4 ký tự liên tiếp nên chúng ta có thể xử lý chuỗi từ trái sang phải trong khi vẫn giữ 3 ký tự được gán cuối cùng làm trạng thái. 

Đối với mọi vị trí, chúng tôi quyết định xem ký tự cuối cùng có phải là`'('`hoặc`')'`. Chi phí là 0 nếu chúng ta giữ ký tự gốc, 1 nếu chúng ta lật nó. 

Bất cứ khi nào chúng tôi đã gán 4 ký tự liên tiếp, chúng tôi sẽ kiểm tra bit ràng buộc tương ứng: 

- Nếu`a[i] = '1'`, khối 4 ký tự phải là một trong hai chuỗi dấu ngoặc thông thường có độ dài 4 hợp lệ: 

-`(())`-`()()`- Nếu như`a[i] = '0'`, không có hạn chế nào được áp đặt. 

Điều này mang lại cho DP chỉ$2^3 = 8$tiểu bang. 

Một giải pháp đúng là:```python
import sys
input = sys.stdin.readline

INF = 10 ** 18

VALID = {
    0b1100,  # (())
    0b1010,  # ()()
}

t = int(input())

for _ in range(t):
    n = int(input())
    s = input().strip()
    a = input().strip()

    dp = {}

    # initialize first character
    for b in range(2):
        cost = (s[0] != ('(' if b else ')'))
        dp[(1, b)] = cost

    for i in range(1, n):
        ndp = {}

        for (length, mask), cur_cost in dp.items():
            for b in range(2):
                add = (s[i] != ('(' if b else ')'))

                if length < 3:
                    nmask = (mask << 1) | b
                    key = (length + 1, nmask)
                    old = ndp.get(key, INF)
                    if cur_cost + add < old:
                        ndp[key] = cur_cost + add
                else:
                    block = ((mask << 1) | b)

                    start = i - 3

                    if a[start] == '1' and block not in VALID:
                        continue

                    nmask = block & 0b111
                    key = (3, nmask)

                    old = ndp.get(key, INF)
                    if cur_cost + add < old:
                        ndp[key] = cur_cost + add

        dp = ndp

    ans = min(dp.values(), default=INF)
    print(-1 if ans == INF else ans)
```Cái này chạy vào$O(n \cdot 8 \cdot 2)$cho mỗi trường hợp thử nghiệm, tuyến tính trong tổng kích thước đầu vào và dễ dàng phù hợp với các ràng buộc.
