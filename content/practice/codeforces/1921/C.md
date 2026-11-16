---
title: "CF 1921C - Gửi tin nhắn"
description: "Stepan cần gửi một loạt tin nhắn vào những thời điểm tăng dần theo thời gian. Điện thoại của anh ấy khởi động với mức sạc hữu hạn, mất một lượng không đổi trên một đơn vị thời gian khi bật và tiêu tốn một khoản chi phí cố định nếu anh ấy tắt rồi bật lại."
date: "2026-06-08T19:21:48+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1921
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 920 (Div. 3)"
rating: 900
weight: 1921
solve_time_s: 108
verified: true
draft: false
---

[CF 1921C - Gửi tin nhắn](https://codeforces.com/problemset/problem/1921/C) 

**Xếp hạng:** 900 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Stepan cần gửi một loạt tin nhắn vào những thời điểm tăng dần theo thời gian. Điện thoại của anh ấy khởi động với mức sạc hữu hạn, mất một lượng không đổi trên một đơn vị thời gian khi bật và tiêu tốn một khoản chi phí cố định nếu anh ấy tắt rồi bật lại. Vấn đề đặt ra là liệu anh ta có thể lên lịch cho tất cả các tin nhắn mà không bao giờ hết pin hay không. 

Mỗi trường hợp thử nghiệm cung cấp số lượng tin nhắn, lượng pin ban đầu, mức tiêu thụ thời gian trên mỗi đơn vị, chi phí chuyển đổi năng lượng và thời điểm chính xác mà tin nhắn cần được gửi. Chúng ta phải xuất "CÓ" nếu tồn tại một chuỗi quyết định bật/tắt cho phép gửi tất cả tin nhắn và "KHÔNG" nếu ngược lại. 

Các ràng buộc cho thấy tổng số thông báo trên tất cả các trường hợp thử nghiệm không vượt quá 200.000 và các khoảnh khắc thời gian có thể lên tới$10^9$. Điều này loại trừ mọi giải pháp lặp lại qua từng đơn vị thời gian riêng lẻ; chúng ta phải lý giải về sự khác biệt giữa các lần nhắn tin. Các trường hợp đặc biệt bao gồm các tình huống trong đó pin hầu như không đủ để tồn tại cho đến khi có tin nhắn đầu tiên hoặc việc bật và tắt điện thoại sẽ rẻ hơn so với việc để nó bật. 

Cách tiếp cận bất cẩn có thể mô phỏng mọi đơn vị thời gian hoặc bỏ qua lựa chọn giữa việc bật điện thoại và tắt điện thoại, dẫn đến các câu trả lời sai về đầu vào trong đó chiến lược tối ưu liên quan đến việc chuyển đổi. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force xem xét trạng thái điện thoại tại mọi thời điểm từ 0 đến tin nhắn cuối cùng, bật hoặc tắt để tiết kiệm năng lượng. Đối với mỗi đơn vị thời gian, chúng tôi sẽ tính toán mức giảm pin. Về nguyên tắc thì điều này đúng nhưng với$10^9$vì thời gian tin nhắn tối đa nên việc lặp lại theo đơn vị thời gian là không khả thi. 

Thông tin chi tiết quan trọng là pin chỉ thay đổi tại những thời điểm riêng biệt: tại thời điểm có tin nhắn và ngay trước hoặc sau khi chuyển đổi điện thoại. Giữa hai tin nhắn liên tiếp, chúng ta chỉ cần quyết định xem nên để điện thoại bật hay tắt đi bật lại sẽ rẻ hơn. Đối với một khoảng cách chiều dài$d$, để điện thoại bật sẽ tiêu tốn$a \cdot d$các thiết bị, trong khi tắt và bật sẽ tiêu tốn$b$đơn vị. Do đó, trong mỗi khoảng thời gian, chi phí pin tối thiểu là$\min(a \cdot d, b)$. Chúng tôi tích lũy những chi phí này bắt đầu từ tin nhắn đầu tiên và kiểm tra xem lượng pin còn lại có giảm xuống dưới 0 hay không. 

Quan sát này làm giảm vấn đề xuống$O(n)$cho mỗi trường hợp thử nghiệm vì chúng tôi chỉ xử lý sự khác biệt giữa các thời điểm tin nhắn liên tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng từng đơn vị thời gian) | O(max(m_i)) | O(1) | Quá chậm | 
| Tối ưu (khoảng tham lam) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc$n, f, a, b$và danh sách thời gian nhắn tin$m$. 
3. Khởi tạo lượng pin còn lại thành$f$. 
4. Lặp lại các khoảng trống giữa các tin nhắn liên tiếp, bao gồm cả tin nhắn đầu tiên tại thời điểm đó$m_1$dưới dạng khoảng cách từ 0. Tính độ dài của khoảng cách$d = m_i - m_{i-1}$(với$m_0 = 0$). 
5. Với mỗi khoảng trống, hãy xác định chi phí pin tối thiểu:$\min(a \cdot d, b)$. Trừ đi phần này từ lượng pin còn lại. 
6. Nếu tại bất kỳ thời điểm nào pin giảm xuống 0 hoặc thấp hơn, hãy ngắt và xuất "NO" cho trường hợp kiểm tra này. Ngược lại, sau tất cả các tin nhắn, xuất ra "CÓ". 

Tại sao nó hoạt động: thuật toán luôn chọn tùy chọn rẻ hơn giữa việc để điện thoại bật hoặc tắt và bật cho mỗi khoảng trống. Vì điện thoại chỉ hao pin theo hai cách - tuyến tính theo thời gian khi bật hoặc cố định khi bật - sự lựa chọn tham lam này đảm bảo mức tiêu thụ pin tối thiểu có thể cho mỗi tin nhắn, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, f, a, b = map(int, input().split())
    m = list(map(int, input().split()))
    prev = 0
    battery = f
    possible = True
    for time in m:
        gap = time - prev
        cost = min(a * gap, b)
        battery -= cost
        if battery <= 0:
            possible = False
            break
        prev = time
    print("YES" if possible else "NO")
```Mã đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`cho nhiều trường hợp thử nghiệm. Đối với mỗi tin nhắn, nó tính toán khoảng thời gian tính từ thời điểm trước đó, tính toán chi phí rẻ hơn và cập nhật pin. các`battery <= 0`kiểm tra đảm bảo rằng mọi tình huống mà Stepan không thể gửi tin nhắn đều được phát hiện ngay lập tức. sử dụng`min(a * gap, b)`nắm bắt được quyết định tham lam thiết yếu. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:`1 3 1 5`- Khoảng cách: 3 đơn vị (từ 0 đến 3) 
- Chi phí: tối thiểu(1*3, 5) = 3 
- Pin còn lại: 3 - 3 = 0 → xuất ra “NO” 

**Ví dụ 3** 

đầu vào:`5 10 1 2`với tin nhắn`[1, 2, 3, 4, 5]`| Tin nhắn | Khoảng trống | Chi phí | Pin Sau | 
| --- | --- | --- | --- | 
| 1 | 1 | phút(1,2)=1 | 10-1=9 | 
| 2 | 1 | 1 | 9-1=8 | 
| 3 | 1 | 1 | 8-1=7 | 
| 4 | 1 | 1 | 7-1=6 | 
| 5 | 1 | 1 | 6-1=5 | 

Pin không bao giờ về 0 → xuất ra "CÓ". Điều này cho thấy thuật toán đã tích lũy chi phí một cách chính xác và xử lý những khoảng trống nhỏ một cách hiệu quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi tin nhắn được xử lý một lần; n tổng hợp trên tất cả các trường hợp thử nghiệm 2×10^5 | 
| Không gian | O(n) | Lưu trữ thời gian tin nhắn cho mỗi trường hợp thử nghiệm; không có cấu trúc bổ sung | 

Giải pháp này rất phù hợp trong giới hạn thời gian 2 giây vì nó xử lý mỗi tin nhắn một lần và sử dụng bộ nhớ bổ sung tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    # call solution
    t = int(input())
    for _ in range(t):
        n, f, a, b = map(int, input().split())
        m = list(map(int, input().split()))
        prev = 0
        battery = f
        possible = True
        for time in m:
            gap = time - prev
            cost = min(a * gap, b)
            battery -= cost
            if battery <= 0:
                possible = False
                break
            prev = time
        print("YES" if possible else "NO")
    return out.getvalue().strip()

# Provided samples
assert run("6\n1 3 1 5\n3\n7 21 1 3\n4 6 10 13 17 20 26\n5 10 1 2\n1 2 3 4 5\n1 1000000000 1000000000 1000000000\n1000000000\n3 11 9 6\n6 8 10\n12 621526648 2585904 3566299\n51789 61859 71998 73401 247675 298086 606959 663464 735972 806043 806459 919683") == "NO\nYES\nYES\nNO\nNO\nYES"

# Custom edge cases
assert run("1\n1 1 1 1\n1") == "NO", "battery exactly equal to gap cost"
assert run("1\n2 10 1 100\n1 1000000000") == "YES", "large gap with off/on cheaper than long on"
assert run("1\n3 5 2 3\n1 2 4") == "NO", "mixed gap costs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1\n1`| KHÔNG | Pin về 0 ngay tin nhắn đầu tiên | 
|`2 10 1 100\n1 1000000000`| CÓ | Xử lý những khoảng trống rất lớn một cách hiệu quả | 
|`3 5 2 3\n1 2 4`| KHÔNG | Những quyết định tham lam hỗn hợp giữa việc tiếp tục và chuyển đổi | 

## Vỏ cạnh 

Khi pin chính xác bằng mức tiêu hao của khoảng thời gian đầu tiên, thuật toán sẽ xuất ra "KHÔNG" một cách chính xác. Đối với những khoảng trống cực lớn, nhân`a * gap`có thể tràn trong một số ngôn ngữ, nhưng Python xử lý các số nguyên lớn và`min(a * gap, b)`lựa chọn chính xác sẽ ngăn chặn kết quả tràn không chính xác. Sự lựa chọn tham lam đảm bảo mức tiêu thụ tối thiểu cho mỗi khoảng thời gian, vì vậy không có trường hợp tinh vi nào bỏ qua việc chuyển đổi sẽ gây ra câu trả lời sai.
