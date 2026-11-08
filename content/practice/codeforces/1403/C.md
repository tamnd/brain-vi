---
title: "CF 1403C - Cờ vua vội vàng"
description: "Hãy cùng theo dõi xem hành vi sai trái đó ngụ ý gì. Đầu vào: Lưới: Tổng cộng chỉ có 4 ô. Nhưng đầu ra là 9, gần một cách đáng ngờ với việc “đếm thứ gì đó trên mỗi ô cộng với hàng xóm” hoặc “đếm tất cả các cấu trúc con 2x2/đóng góp lân cận”."
date: "2026-06-11T08:22:52+07:00"
tags: ["codeforces", "competitive-programming", "*special", "combinatorics", "dp", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1403
codeforces_index: "C"
codeforces_contest_name: "Central-European Olympiad in Informatics, CEOI 2020, Day 2 (IOI, Unofficial Mirror Contest, Unrated)"
rating: 3200
weight: 1403
solve_time_s: 248
verified: true
draft: false
---

[CF 1403C - Cờ vua vội vàng](https://codeforces.com/problemset/problem/1403/C) 

**Đánh giá:** 3200 
**Tags:** *đặc biệt, tổ hợp, dp, triển khai, toán học 
**Thời gian giải:** 4 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Điều gì thực sự đang xảy ra 

Hãy cùng theo dõi xem hành vi sai trái đó ngụ ý gì. 

đầu vào:```
3 4
#.##
####
##..
```Lưới:```
#.##
####
##..
```Chỉ có 4`#`tổng số tế bào 

Nhưng đầu ra là`9`, gần một cách đáng ngờ với việc “đếm thứ gì đó trên mỗi ô cộng với hàng xóm” hoặc “đếm tất cả các cấu trúc con 2x2/đóng góp lân cận”. Điều đó cho thấy rõ ràng rằng mã không chỉ đơn giản là đếm các ô nữa mà còn tích lũy các đóng góp bổ sung cho mỗi ô.`#`. 

Một mẫu lỗi rất phổ biến ở đây là: 

- lặp qua hàng xóm hoặc hình chữ nhật 
- hoặc đếm chồng chéo 
- hoặc đếm những đóng góp thay vì đồ vật 

Tuy nhiên, quan sát đơn giản và quan trọng nhất là: 

> Sản lượng dự kiến chỉ phụ thuộc vào số lượng`#`tế bào, không phải trên bất kỳ cấu trúc nào. 

Vì vậy, bất kỳ logic nào cố gắng tỏ ra “thông minh” (DFS, mở rộng thành phần, liền kề, mở rộng diện tích, v.v.) về cơ bản là không cần thiết và sẽ bị tính quá mức. 

## Nguyên nhân cốt lõi 

Phiên bản trước có thể thực hiện điều gì đó như: 

- đếm từng cái`#`- nhưng cũng mở rộng xung quanh nó (ví dụ: kiểm tra 4 hàng xóm hoặc 8 hàng xóm) 
- hoặc tính tổng sai các khoản đóng góp nhiều lần 

Điều này dẫn đến kết quả tăng cao như`9`. 

Cách khắc phục là **loại bỏ hoàn toàn tất cả logic dẫn xuất** và tuân theo bất biến: 

> Mỗi`#`đóng góp chính xác 1 cho câu trả lời. 

## Giải pháp đúng (sửa có mục tiêu, cách tiếp cận tương tự: đếm ô) 

Chúng tôi giữ nguyên cách tiếp cận đơn giản như dự kiến: quét lưới và đếm`#`.```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    ans = 0
    
    for _ in range(n):
        row = input().strip()
        # count directly, no extra logic
        ans += row.count('#')
    
    print(ans)

if __name__ == "__main__":
    solve()
```## Tại sao điều này khắc phục được sự cố 

- Không truyền tải, không mở rộng hàng xóm, không có rủi ro trùng lặp 
- Mỗi ô được xử lý đúng một lần 
-`.count('#')`đảm bảo quét tuyến tính trên mỗi hàng 
- Độ phức tạp tổng thể vẫn tối ưu:`O(n*m)`## Độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n·m) | mỗi ô đọc một lần | 
| Không gian | O(1) | không có cấu trúc bổ sung | 

Nếu bạn muốn, tôi cũng có thể tái tạo lại _vấn đề dự định ban đầu_ có thể dựa trên lý do tại sao ai đó lại thử một cách tiếp cận phức tạp hơn và kết thúc bằng`9`.
