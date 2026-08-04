---
title: "CF 102556F - Phòng chat Riana và Fiber"
description: "Mỗi người dùng có thể được biểu thị bằng năm sinh của họ, bởi vì tất cả người dùng sinh cùng năm đều tự động có cùng độ tuổi. Mối liên hệ bổ sung duy nhất đến từ sinh nhật “năm nhanh”, ngày từ ngày 1 tháng 1 đến ngày 28 tháng 2."
date: "2026-08-04T09:10:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102556
codeforces_index: "F"
codeforces_contest_name: "2020 Ateneo de Manila University DISCS PrO HS Division"
rating: 0
weight: 102556
solve_time_s: 63
verified: true
draft: false
---

[CF 102556F - Phòng trò chuyện Riana và Fiber](https://codeforces.com/problemset/problem/102556/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi người dùng có thể được biểu thị bằng năm sinh của họ, bởi vì tất cả người dùng sinh cùng năm đều tự động có cùng độ tuổi. Mối liên hệ bổ sung duy nhất đến từ sinh nhật “năm nhanh”, ngày từ ngày 1 tháng 1 đến ngày 28 tháng 2. Một người dùng năm nhanh sinh năm`y`kết nối năm`y`với năm`y-1`. 

Việc đóng cửa chuyển tiếp của các kết nối này xác định các nhóm tuổi cuối cùng. Riana muốn mọi người dùng hiện tại và bản thân cô đều thuộc một nhóm tuổi sau khi thêm càng ít người dùng mới càng tốt. 

Đầu vào chứa người dùng hiện tại và ngày sinh đã đăng ký của Riana. Kết quả đầu ra yêu cầu số ngày sinh nhật bổ sung tối thiểu để tạo ra một nhóm tuổi được kết nối, theo sau là bất kỳ ngày sinh nhật hợp lệ nào để thêm vào. 

Với tối đa`100000`người dùng, việc kiểm tra từng cặp người là không thể. Một giải pháp bậc hai sẽ thực hiện khoảng`10^10`sự so sánh vượt quá giới hạn. Cấu trúc hữu ích là các năm tạo thành một đường thẳng, do đó vấn đề trở thành việc kết nối các cạnh bị thiếu trên một đường dẫn. 

Một sai lầm phổ biến là so sánh ngày sinh chính xác. Các quy tắc chỉ phụ thuộc vào nhóm năm và kết nối năm nhanh. Một sai lầm nữa là quên mất chính Riana. Năm của cô ấy phải được đưa vào mặc dù cô ấy không nằm trong số các thành viên trò chuyện hiện có. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ xây dựng biểu đồ giữa tất cả người dùng, thêm một cạnh bất cứ khi nào hai người dùng được coi là cùng độ tuổi, sau đó liên tục thêm người dùng cho đến khi biểu đồ được kết nối. Điều này đúng vì định nghĩa về độ tuổi chính xác là sự kết nối theo những quy tắc đó. Tuy nhiên, việc so sánh tất cả các cặp là quá tốn kém và biểu đồ có thể chứa`100001`đỉnh. 

Quan sát quan trọng là mỗi năm người dùng là một nút trên một trục số. Sinh nhật nhanh trong năm`y`chỉ tạo ra ranh giới giữa`y`Và`y-1`. Chúng ta chỉ cần biết cạnh năm liền kề nào đã tồn tại. 

Nếu năm nhỏ nhất và lớn nhất có liên quan là`L`Và`R`, mọi cạnh bị thiếu giữa`y-1`Và`y`bên trong khoảng này phải được tạo ra. Thêm người dùng nhanh trong năm`y`chính xác là hoạt động tạo ra cạnh đó. Vì mỗi người dùng được thêm vào chỉ có thể tạo một kết nối trong năm liền kề nên mọi cạnh bị thiếu là không thể tránh khỏi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(N2) | Quá chậm | 
| Tối ưu | O(N + phạm vi năm) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích ngày sinh nhật của Riana và mọi ngày sinh nhật hiện có. Lưu trữ những năm xuất hiện và đánh dấu mỗi năm có ít nhất một năm sinh nhật nhanh. 
2. Bao gồm năm sinh của Riana trong số những năm có liên quan. Nhóm tuổi cuối cùng cũng phải có cô ấy. 
3. Tìm số năm tối thiểu và tối đa có liên quan. Mọi kết nối cần thiết đều nằm giữa hai năm này. 
4. Hàng năm`y`từ`min_year + 1`ĐẾN`max_year`, kiểm tra xem đã có người dùng năm nhanh sinh năm chưa`y`. Nếu không, hãy thêm người dùng mới sinh ngày 1 tháng 1 năm`y`. 
5. Xuất ra tất cả các ngày sinh nhật được thêm vào. 

Tại sao nó hoạt động: Biểu đồ tuổi là một đường dẫn qua nhiều năm. Các cạnh duy nhất có thể là giữa các năm liên tiếp. Người dùng năm nhanh hiện tại cung cấp một số lợi thế này. Bất kỳ cạnh nào bị thiếu sẽ chia đường dẫn thành hai phần bị ngắt kết nối, do đó ít nhất một người dùng mới phải thêm nó vào. Thuật toán thêm chính xác một người dùng cho mỗi cạnh bị thiếu, điều này vừa đủ vừa cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def parse_date(s):
    y, m, d = map(int, s.split("-"))
    return y, m, d

def format_date(y, m=1, d=1):
    if y < 10000:
        return f"{y:04d}-{m:02d}-{d:02d}"
    return f"{y:05d}-{m:02d}-{d:02d}"

def solve():
    n = int(input())
    ry, rm, rd = parse_date(input().strip())

    years = {ry}
    fast = set()

    for _ in range(n):
        y, m, d = parse_date(input().strip())
        years.add(y)
        if m == 1 or (m == 2 and d <= 28):
            fast.add(y)

    lo = min(years)
    hi = max(years)

    ans = []
    for y in range(lo + 1, hi + 1):
        if y not in fast:
            ans.append(format_date(y))

    print(len(ans))
    for x in ans:
        print(x)

if __name__ == "__main__":
    solve()
```Trình phân tích cú pháp bỏ qua độ rộng định dạng chính xác của năm vì việc chia theo`-`đưa ra năm số trực tiếp. Điều kiện năm nhanh chỉ được kiểm tra vào ngày 28 tháng 1 và tháng 2 hoặc sớm hơn. 

Vòng lặp bắt đầu lúc`lo + 1`bởi vì một người dùng nhanh trong năm`y`kết nối`y`ĐẾN`y - 1`. Bắt đầu từ`lo`sẽ tạo ra một kết nối không cần thiết ngoài khoảng thời gian cần thiết. Sử dụng ngày 1 tháng 1 rất thuận tiện vì nó luôn là ngày hợp lệ trong năm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + Y) |`N`sinh nhật được đọc và`Y`là khoảng cách giữa năm tối thiểu và tối đa | 
| Không gian | O(N) | Các bộ lưu trữ các năm liên quan và các kết nối năm nhanh hiện có | 

Phạm vi năm nhiều nhất là`99999 - 1000`, do đó quá trình quét tuyến tính nhỏ. Giải pháp dễ dàng phù hợp với các ràng buộc.
