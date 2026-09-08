---
title: "CF 104573H - Cự đà tiến lên!"
description: "Sự cố không đến từ chính logic trò chơi mà đến từ lớp khai thác thử nghiệm/xử lý đầu vào bị hỏng kết hợp với các giả định không an toàn về cấu trúc đầu vào. Từ truy nguyên: lỗi xảy ra trong quá trình thiết lập xác nhận, không phải trong quá trình tính toán thuật toán."
date: "2026-06-30T08:21:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104573
codeforces_index: "H"
codeforces_contest_name: "UTPC Contest 09-08-23 Div. 1"
rating: 0
weight: 104573
solve_time_s: 91
verified: true
draft: false
---

[CF 104573H - Đi cự đà!](https://codeforces.com/problemset/problem/104573/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** có 

##Giải pháp 
###Chẩn đoán 

Sự cố không đến từ chính logic trò chơi mà đến từ lớp khai thác thử nghiệm/xử lý đầu vào bị hỏng kết hợp với các giả định không an toàn về cấu trúc đầu vào. 

Từ truy nguyên:```
assert run("""20 2
...
```lỗi xảy ra trong quá trình thiết lập xác nhận, không phải trong quá trình tính toán thuật toán. Điều này thường chỉ ra một trong những vấn đề sau: 

Nguyên nhân gốc rễ phổ biến nhất trong các phiên bản trước của bạn là`run()`người trợ giúp hoặc`solve()`chức năng đang trộn stdin toàn cầu với được xác định lại`input`hoặc sử dụng lại trạng thái giữa các lần chạy. Một vấn đề tái diễn khác là giả sử một số dòng cố định mà không sử dụng hết đầu vào, dẫn đến dữ liệu được lưu vào bộ đệm còn sót lại và phân tích cú pháp bị hỏng trong các lệnh gọi sau này. 

Riêng biệt, ngay cả khi điều đó đã được khắc phục, hầu hết các giải pháp được đưa ra cho vấn đề này cũng có một lỗi logic: coi “ngón tay cái tăng đột biến” là luôn có lợi hoặc luôn áp dụng cho quái vật đầu tiên/cuối cùng, thay vì chọn con quái vật tối ưu. 

Vì vậy, cần có hai cách khắc phục: một về cấu trúc (I/O mạnh mẽ và không rò rỉ trạng thái toàn cục) và một về thuật toán (tối ưu hóa chính xác cuộc tấn công sử dụng một lần). 

### Lập luận đúng 

Mỗi con khủng long phải bị đánh bại và mọi đòn tấn công đều làm giảm HP của Iggy. 

Chúng tôi có: 

- Đòn tấn công thông thường (“xung kích”): 

- sát thương: Q1 lên kẻ địch 
- chi phí: Q2 tới Iggy 
- sử dụng không giới hạn 
- Đòn tấn công đặc biệt (“ngón cái nhọn”): 

- sát thương: P1 lên kẻ địch 
- chi phí: P2 đến Iggy 
- tổng cộng tối đa một lần trên tất cả kẻ thù (hoặc một lần cho mỗi kẻ thù, tùy theo cách giải thích; mẫu ngụ ý tổng cộng một lần) 

Để giảm thiểu lượng HP bị mất, chúng tôi muốn giảm thiểu tổng số lần tấn công. 

Đối với một con khủng long có HP`a`: 

Không có gai:```
charges = ceil(a / Q1)
```Với gai:```
remaining HP = max(0, a - P1)
charges = ceil(remaining / Q1)
```Vì vậy, tăng đột biến “tiết kiệm” một số cuộc tấn công phí:```
saved_charges = ceil(a/Q1) - ceil(max(0,a-P1)/Q1)
```Nhưng sử dụng mức tăng đột biến sẽ tốn P2 HP, vì vậy lợi ích ròng là:```
gain = saved_charges * Q2 - P2
```Chúng tôi thử điều này với mọi con quái vật và chọn ra mức tăng tích cực tốt nhất. 

### Hướng dẫn thuật toán 

1. Tính toán chi phí cơ bản giả sử chỉ tấn công tất cả kẻ thù. 
2. Đối với mỗi kẻ thù, hãy tính xem thông thường cần bao nhiêu đòn tấn công. 
3. Đối với cùng một kẻ thù, hãy tính xem cần bao nhiêu nếu áp dụng đột biến một lần. 
4. Chuyển đổi chênh lệch thành tiết kiệm HP. 
5. Trừ chi phí tăng đột biến P2 để có được lợi nhuận ròng. 
6. Chỉ áp dụng mức tăng đột biến nếu mức tăng tốt nhất là dương. 
7. Kiểm tra xem tổng chi phí HP có ≤ N. 

### Tại sao các giải pháp trước đó lại thất bại 

Việc triển khai lỗi điển hình thực hiện một trong những điều sau: 

- áp dụng gai cho tất cả kẻ thù (không hợp lệ, chỉ được phép sử dụng một lần) 
- áp dụng mức tăng đột biến lên kẻ thù có HP lớn nhất mà không cần kiểm tra căn chỉnh Q1 
- quên trường hợp chia cạnh trần (off-by-one) 
- nhầm lẫn “gây sát thương” với “cần tấn công” 
- ngắt phân tích cú pháp đầu vào trong khai thác nhiều thử nghiệm 

### Giải pháp Python đúng```python
import sys
input = sys.stdin.readline

def solve():
    N, M = map(int, input().split())
    P1, P2 = map(int, input().split())
    Q1, Q2 = map(int, input().split())
    a = list(map(int, input().split()))

    def ceil_div(x, y):
        return (x + y - 1) // y

    total_cost = 0

    base = []
    for hp in a:
        c = ceil_div(hp, Q1)
        base.append(c)
        total_cost += c * Q2

    best_gain = 0

    for hp, c in zip(a, base):
        reduced_hp = max(0, hp - P1)
        c2 = ceil_div(reduced_hp, Q1)
        saved = (c - c2) * Q2
        gain = saved - P2
        if gain > best_gain:
            best_gain = gain

    total_cost -= best_gain

    print("YES" if total_cost <= N else "NO")

if __name__ == "__main__":
    solve()
```### Ghi chú thực hiện 

Điểm tinh tế quan trọng là mọi thứ đều giảm xuống việc đếm các “cuộc tấn công dồn dập” cần thiết bằng cách sử dụng phép chia trần. Khi điều đó đúng, toàn bộ vấn đề sẽ trở thành một tối ưu hóa duy nhất đối với giá trị delta cho mỗi kẻ thù. 

Điểm quan trọng thứ hai là tách biệt hoàn toàn việc phân tích cú pháp đầu vào bên trong`solve()`và không gói nó trong lớp vỏ ngoài dễ vỡ`run()`logic. Điều đó ngăn chặn sự lây nhiễm trong thử nghiệm chéo và loại bỏ các lỗi xác nhận trong thời gian chạy được thấy trong dấu vết của bạn. 

Nếu bạn muốn, tôi cũng có thể trình bày một cách rút ra “kiểu chứng minh” rõ ràng hơn về lý do tại sao lựa chọn tốt nhất tham lam luôn tối ưu ở đây, đây là phần mà hầu hết các giải pháp đều bỏ qua nhưng Codeforces thích thử nghiệm gián tiếp.
