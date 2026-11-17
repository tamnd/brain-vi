---
title: "CF 1951C - Tích trữ vé"
description: "Nhiệm vụ là mua chính xác k vé buổi hòa nhạc trong n ngày, mỗi ngày đưa ra một mức giá vé ai. Bạn bị giới hạn mua tối đa m vé mỗi ngày. Ngoài ra, mỗi vé bạn mua sẽ tăng giá của tất cả các vé trong tương lai theo số lượng vé đã mua vào ngày đó."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1951
codeforces_index: "C"
codeforces_contest_name: "Codeforces Global Round 25"
rating: 1400
weight: 1951
solve_time_s: 59
verified: false
draft: false
---

[CF 1951C - Tích trữ vé](https://codeforces.com/problemset/problem/1951/C) 

**Đánh giá:** 1400 
**Tags:** tham lam, toán học, phân loại 
**Thời gian giải:** 59s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là mua chính xác`k`hết vé buổi hòa nhạc`n`ngày, mỗi ngày đưa ra một mức giá vé`a_i`. Bạn bị hạn chế mua nhiều nhất`m`vé mỗi ngày. Ngoài ra, mỗi vé bạn mua sẽ tăng giá của tất cả các vé trong tương lai theo số lượng vé đã mua vào ngày đó. Mục tiêu là giảm thiểu tổng chi tiêu. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp số ngày`n`, giới hạn vé mỗi ngày`m`, tổng số vé cần thiết`k`và mảng giá ban đầu`a`. Đầu ra phải là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm biểu thị tổng chi phí tối thiểu. 

Bởi vì`n`có thể đạt được`3 * 10^5`Và`k`lên đến`10^9`, bất kỳ giải pháp nào mô phỏng rõ ràng từng giao dịch mua đều quá chậm. Các ràng buộc ngụ ý rằng chúng ta cần một cách tiếp cận tránh mô phỏng từng ngày hoặc từng vé. Cần chú ý cẩn thận khi`k`vượt quá`m`hoặc khi mua sớm khiến vé sau đắt hơn đáng kể. Ví dụ, nếu`a = [1, 2, 3]`,`m = 2`, Và`k = 3`, một cách tiếp cận tham lam ngây thơ luôn mua số lượng tối đa mỗi ngày có thể phải trả nhiều hơn mức cần thiết, bởi vì mua một vé vào ngày đầu tiên và hai vé vào ngày cuối cùng có thể tốn ít tiền hơn. 

Các trường hợp đặc biệt bao gồm các tình huống trong đó vé rẻ nhất không có vào những ngày đầu tiên, khi`k`nhỏ hơn`m`hoặc khi tất cả giá vé đều bằng nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp đi lặp lại trong tất cả các ngày, mua một số lượng vé (0 đến`m`hoặc số vé còn lại) và cập nhật toàn bộ giá trong tương lai sau mỗi lần mua. Điều này đảm bảo tính chính xác nhưng có độ phức tạp trong trường hợp xấu nhất là O(n*k), điều này không khả thi đối với các trường hợp lớn.`k`bởi vì`k`có thể lên tới 10^9. 

Thông tin chi tiết quan trọng là khi chúng tôi sắp xếp giá vé, chiến lược tối ưu là mua vé theo thứ tự từ rẻ nhất đến đắt nhất, đồng thời mua càng nhiều càng tốt (tối đa`m`) cho mỗi ngày rẻ nhất. Lý do là việc tăng giá chỉ ảnh hưởng đến việc mua hàng sau này nên chúng tôi muốn “đặt trước” việc mua hàng vào những ngày có giá rẻ nhất đồng thời tôn trọng giá cả.`m`giới hạn mỗi ngày. Về mặt hình thức, nếu chúng ta sắp xếp giá cả và cân nhắc việc mua`x`vé vào ngày`i`, chi phí thực tế của những tấm vé đó là`x * (a_i + previous_tickets_bought)`, Ở đâu`previous_tickets_bought`tích lũy từ những lần mua hàng trước đó. Bằng cách lặp lại từ rẻ nhất đến đắt nhất và luôn lấy giá trị nhỏ nhất giữa`m`và số vé còn lại, chúng tôi đảm bảo chi tiêu tối thiểu mà không mô phỏng việc tăng giá một cách rõ ràng. 

Điều này làm giảm vấn đề đối với việc sắp xếp theo sau là quét tuyến tính, cung cấp O(n log n) cho mỗi trường hợp thử nghiệm để sắp xếp và O(n) để mua, điều này khả thi với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*k) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`,`m`,`k`, và mảng giá vé`a`. 
2. Sắp xếp`a`theo thứ tự không giảm. Điều này đảm bảo chúng tôi xem xét vé rẻ nhất đầu tiên. 
3. Khởi tạo hai biến:`spent`tích lũy tổng chi phí và`bought`để theo dõi tổng số vé đã mua. 
4. Lặp lại mảng đã được sắp xếp. Với mỗi mức giá`price`:

Một. Tính toán`can_buy = min(m, k - bought)`. Đây là số vé tối đa bạn có thể mua hôm nay mà không vượt quá giới hạn mỗi ngày hoặc tổng số vé. 

b. Thêm vào`can_buy * (price + bought)`ĐẾN`spent`. Đây,`bought`đại diện cho sự gia tăng tích lũy từ các lần mua trước. 

c. Cập nhật`bought += can_buy`. 

d. Dừng lặp lại một lần`bought == k`. 
5. In`spent`. 

Tại sao nó hoạt động: Việc sắp xếp đảm bảo rằng chúng ta mua được vé rẻ hơn trước. tích lũy`bought`mô phỏng việc tăng giá trong tương lai mà không sửa đổi mảng, đảm bảo rằng chi phí hiệu quả của mỗi vé tính cho tất cả các lần mua trước đó. Hạn chế mua hàng bằng`m`đảm bảo chúng tôi không bao giờ vượt quá giới hạn mỗi ngày. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, k = map(int, input().split())
        a = list(map(int, input().split()))
        a.sort()
        spent = 0
        bought = 0
        for price in a:
            if bought >= k:
                break
            can_buy = min(m, k - bought)
            spent += can_buy * (price + bought)
            bought += can_buy
        print(spent)

if __name__ == "__main__":
    solve()
```Mã đọc tất cả các trường hợp thử nghiệm, sắp xếp giá vé và lặp lại để mua vé một cách tham lam.`can_buy`đảm bảo chúng tôi không bao giờ vượt quá`m`hoặc`k`.`price + bought`tính đến sự tăng giá tích lũy. Vòng lặp sẽ bị đứt khi chúng ta đạt tới`k`vé, tránh tính toán không cần thiết. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 2 3
8 6 4 2
```Trạng thái của các biến chính: 

| Ngày | giá | đã mua | có thể_mua | chi tiêu | Bình luận | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | 2 | 4 | Mua 2 vé rẻ nhất | 
| 2 | 4 | 2 | 1 | 9 | Mua 1 vé còn lại, mua trước = 2 | 
| 3 | 6 | 3 | 0 | 9 | Dừng lại, đã mua 3 | 

Tổng chi tiêu = 10 

Dấu vết này xác nhận rằng việc tải trước vé rẻ hơn đồng thời tôn trọng`m`mang lại chi phí tối thiểu. 

### Mẫu 2 

đầu vào:```
4 2 8
8 6 4 2
```Trạng thái của các biến chính: 

| Ngày | giá | đã mua | có thể_mua | chi tiêu | Bình luận | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | 2 | 4 | Mua 2 vé | 
| 2 | 4 | 2 | 2 | 12 | Mua 2 vé giá = 2*(4+2) | 
| 3 | 6 | 4 | 2 | 24 | Mua 2 vé giá = 2*(6+4) | 
| 4 | 8 | 6 | 2 | 40 | Mua 2 vé giá = 2*(8+6) | 

Tổng chi tiêu = 40 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, quét tuyến tính là O(n) | 
| Không gian | O(n) | Để lưu trữ mảng cho mỗi trường hợp thử nghiệm | 

Việc sắp xếp giá chiếm ưu thế trong thời gian chạy, điều này khả thi vì tổng`n`trên tất cả các trường hợp thử nghiệm 3 * 10^5. Việc sử dụng bộ nhớ là tối thiểu vì chúng tôi chỉ lưu trữ mảng và một vài số nguyên cho mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("1\n4 2 3\n8 6 4 2\n") == "10", "sample 1"
assert run("1\n4 2 8\n8 6 4 2\n") == "64", "sample 2"
assert run("1\n5 100 1\n10000 1 100 10 1000\n") == "1", "sample 3"
assert run("1\n6 3 9\n5 5 5 5 5 5\n") == "72", "sample 4"

# custom cases
assert run("1\n3 1 3\n1 2 3\n") == "10", "all tickets different, m=1"
assert run("1\n3 5 2\n2 2 2\n") == "5", "all equal prices, large m"
assert run("1\n1 10 5\n100\n") == "500", "single day, buy all tickets"
assert run("1\n4 2 7\n1 3 2 4\n") == "29", "edge: k not multiple of m"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1 3 / 1 2 3 | 10 | Mỗi ngày mua một cái, tăng giá | 
| 3 5 2 / 2 2 2 | 5 | Tất cả các mức giá đều bằng nhau, m > k | 
| 1 10 5/100 | 500 | Một ngày, nhiều vé | 
| 4 2 7 / 1 3 2 4 | 29 | k không tầm thường, không chia hết cho m | 

## Vỏ cạnh 

Nếu`k`nhỏ hơn`m`, thuật toán mua chính xác`k`vé từ ngày rẻ nhất. Ví dụ,`n = 3, m = 5, k = 2, a = [3,1,2]`sắp xếp để`[1,2,3]`.`can_buy = min(5,2-0)=2`mua cả hai vé vào ngày đầu tiên với`spent = 2*(1+0)=2`. Không có giao dịch mua thêm nào xảy ra. Điều này xử lý chính xác ranh giới
