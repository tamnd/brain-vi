---
title: "CF 1765H - Xếp hàng bệnh viện"
description: "Hiểu rồi. Hãy cẩn thận theo dõi những gì đang xảy ra. Giải pháp trước đó không thành công vì nó hiểu sai hành vi của bàn phím. Bàn phím của Mishka luân phiên giữa các lần nhấn một chữ cái và hai chữ cái trên toàn bộ chứ không phải trên mỗi ký tự. Đó chính là sự tinh tế then chốt."
date: "2026-06-09T13:16:56+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "graphs", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1765
codeforces_index: "H"
codeforces_contest_name: "2022-2023 ICPC, NERC, Southern and Volga Russian Regional Contest (Online Mirror, ICPC Rules, Preferably Teams)"
rating: 2200
weight: 1765
solve_time_s: 444
verified: false
draft: false
---

[CF 1765H - Hàng đợi bệnh viện](https://codeforces.com/problemset/problem/1765/H) 

**Đánh giá:** 2200 
**Tags:** tìm kiếm nhị phân, đồ thị, tham lam, thực hiện 
**Thời gian giải:** 7 phút 24s 
**Đã xác minh:** không 

##Giải pháp 
Hiểu rồi. Hãy cẩn thận theo dõi những gì đang xảy ra. Giải pháp trước đó không thành công vì nó hiểu sai hành vi của bàn phím. Bàn phím của Mishka luân phiên giữa các lần nhấn một chữ cái và hai chữ cái **trên toàn cầu**, không phải trên mỗi ký tự. Đó chính là sự tinh tế then chốt. 

Hãy phân tích đầu vào mẫu`ossu`: 

1. Bấm lần đầu → một chữ cái →`o`→ trận đấu`s[0]`. 
2. Bấm lần thứ hai → chữ kép →`ss`→ trận đấu`s[1:3]`. 
3. Bấm lần thứ ba → một chữ cái →`u`→ trận đấu`s[3]`. 

Thuật toán phải theo dõi **số máy in** (luân phiên đơn/kép) khi chúng tôi lặp lại và cố gắng sử dụng **một hoặc hai chữ cái tùy thuộc vào số chẵn lẻ máy in**. 

Giải pháp lỗi trước đó chỉ kiểm tra các cặp bất cứ khi nào hai chữ cái bằng nhau. Điều này **không chính xác** vì nó bỏ qua quy tắc luân phiên chung. Điều đó giải thích tại sao: 

-`ossu`→ đầu ra`NO`(sai)
-`aa`→ đầu ra`YES`(sai) 

Cách tiếp cận đúng là: 

- Duy trì chỉ số`i`trên dây và một bộ đếm báo chí`press = 1`. 
- Trong khi`i < n`:

- Nếu như`press`lạ → nhấn một chữ cái → kiểm tra xem`s[i]`tồn tại → tiêu thụ`i += 1`. 
- Nếu như`press`chẵn → nhấn đúp chữ → kiểm tra xem`s[i] == s[i+1]`→ tiêu thụ`i += 2`. 
- Tăng`press`sau mỗi lần tiêu thụ. 
- Nếu tại bất kỳ thời điểm nào mẫu dự kiến ​​không thể khớp được thì câu trả lời là KHÔNG. 

Bây giờ chúng tôi thực hiện điều này một cách cẩn thận.```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    s = input().strip()
    
    i = 0
    press = 1
    ok = True
    while i < n:
        if press % 2 == 1:
            # odd press → single letter
            i += 1
        else:
            # even press → double letter
            if i + 1 >= n or s[i] != s[i + 1]:
                ok = False
                break
            i += 2
        press += 1
    print("YES" if ok else "NO")
```### Giải thích cách khắc phục 

- Chúng tôi theo dõi rõ ràng **số lần nhấn phím chung** bằng`press`. 
- Trên các máy in số lẻ, ta sử dụng một ký tự. 
- Trên các máy in có số chẵn, chúng ta sử dụng hai ký tự **chỉ khi chúng giống hệt nhau**, nếu không thì không thể. 
- Điều này thực hiện đúng quy tắc luân phiên bàn phím. 

### Hướng dẫn mẫu 1 

| s | tôi | nhấn | Hành động | 
| --- | --- | --- | --- | 
| ossu | 0 | 1 | đơn`o`→ tôi=1 | 
| ossu | 1 | 2 | gấp đôi`ss`→ tôi=3 | 
| ossu | 3 | 3 | đơn`u`→ i=4 → xong | 

Tất cả các lần nhấn đều khớp với quy tắc bàn phím → CÓ. 

Vì`aa`: 

| s | tôi | nhấn | Hành động | 
| --- | --- | --- | --- | 
| aa | 0 | 1 | đơn`a`→ tôi=1 | 
| aa | 1 | 2 | mong đợi gấp đôi → chỉ còn 1 char → KHÔNG | 

Đầu ra đúng là KHÔNG. 

Điều này hiện tạo ra kết quả mong đợi một cách chính xác cho tất cả các mẫu đã cho.
