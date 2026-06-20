---
title: "CF 104A - Xì dách"
description: "Chúng ta được đưa ra một kịch bản blackjack đơn giản hóa trong đó lá bài đầu tiên được cố định: quân bài bích, đóng góp 10 điểm. Người chơi muốn tổng của lá bài này và lá bài thứ hai bằng một số n nhất định, nằm trong khoảng từ 1 đến 25."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 104
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 80 (Div. 2 Only)"
rating: 800
weight: 104
solve_time_s: 74
verified: true
draft: false
---

[CF 104A - Xì dách](https://codeforces.com/problemset/problem/104/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa ra một kịch bản blackjack đơn giản hóa trong đó lá bài đầu tiên được cố định: quân bài bích, đóng góp 10 điểm. Người chơi muốn tổng của lá bài này và lá bài thứ hai bằng một số nhất định`n`, nằm trong khoảng từ 1 đến 25. Nhiệm vụ là xác định xem có bao nhiêu quân bài thứ hai khác nhau sẽ tạo ra tổng số chính xác như vậy. 

Các quân bài có các giá trị điểm sau: 2 đến 10 có giá trị theo mệnh giá của chúng, quân jack, quân hậu và quân vua có giá trị là 10 và quân Át có thể là 1 hoặc 11. Mỗi giá trị có bốn quân bài, mỗi quân một chất, ngoại trừ quân hậu của quân bích đã được lấy và không thể sử dụng lại. 

Những ràng buộc của vấn đề là nhỏ:`n`tăng lên 25 và kích thước bộ bài được cố định ở mức 52 quân bài. Điều này có nghĩa là bất kỳ giải pháp nào lặp lại qua các giá trị thẻ và bộ quần áo một vài lần sẽ chạy thoải mái trong giới hạn 2 giây. Chi tiết quan trọng không rõ ràng là xử lý quân Át một cách chính xác: chúng có thể được tính là 1 hoặc 11, và 10 điểm của quân hậu kết hợp với lựa chọn đó để đạt được`n`. Một sự tinh tế khác là nếu`n`nhỏ hơn hoặc bằng 10 thì quân Át được tính là 11 sẽ vượt quá nên phải cân nhắc kỹ. Ví dụ, nếu`n = 11`, quân bài thứ hai có thể là quân Át được tính là 1, nhưng quân Át là 11 sẽ vượt quá`n`. Bỏ qua điều này sẽ tạo ra số đếm không chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản sẽ lặp qua tất cả 51 lá bài còn lại, tính tổng giá trị của chúng với 10 điểm của quân hậu và đếm những lá bài phù hợp.`n`. Điều này có thể đúng nhưng có phần không phù hợp. Với kích thước đầu vào nhỏ, ngay cả việc kiểm tra toàn diện như vậy cũng có hiệu quả, nhưng việc suy luận cẩn thận cho phép tính toán trực tiếp dựa trên giá trị quân bài và bộ bài. 

Cái nhìn sâu sắc quan trọng là suy nghĩ về giá trị cần thiết cho thẻ thứ hai thay vì liệt kê từng thẻ. Vì thẻ đầu tiên đóng góp 10 điểm nên thẻ thứ hai phải cung cấp chính xác`n - 10`điểm. Sau đó, chúng tôi ánh xạ mục tiêu này vào tập hợp các giá trị quân bài có thể có: 2 đến 10, quân jack, quân hậu, quân vua (tất cả 10 điểm) và quân át (1 hoặc 11). Mỗi giá trị số từ 2 đến 10 có bốn chất, mỗi quân bài 10 điểm có bốn chất (trừ quân Hậu) và quân Át có bốn chất. Việc kiểm tra các kết hợp này sẽ trực tiếp tạo ra số đếm mà không cần lặp qua từng thẻ riêng lẻ. Điều này tránh vòng lặp không cần thiết trong khi vẫn trực quan. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(51) | O(1) | Có thể chấp nhận do kích thước boong nhỏ | 
| Tối ưu | O(1) | O(1) | Được chấp nhận và sạch sẽ | 

## Hướng dẫn thuật toán 

1. Đọc`n`, tổng mục tiêu bao gồm cả lá bài đầu tiên. 
2. Lá bài đầu tiên là quân bài bích nên giá trị của nó là 10. Tính giá trị còn lại cần thiết từ lá bài thứ hai:`needed = n - 10`. 
3. Nếu`needed`nhỏ hơn 1 hoặc lớn hơn 11, không có lá bài nào có thể đáp ứng yêu cầu, do đó kết quả là 0. Điều này xử lý các trường hợp cạnh mà tổng mục tiêu là không thể theo quy tắc blackjack. 
4. Nếu`needed`chính xác là 10, quân bài thứ hai có thể là quân bài 10 điểm bất kỳ. Thông thường, có 16 lá bài mười điểm (bốn chục, bốn quân, bốn quân hậu, bốn quân vua). Vì quân Hậu đã được sử dụng nên trừ 1 để được 15. 
5. Nếu`needed`chính xác là 11, chỉ có quân Át được tính là 11 tác phẩm và có 4 quân Át. 
6. Đối với tất cả các giá trị khác từ 1 đến 9, có chính xác 4 lá bài tương ứng với giá trị số đó, mỗi lá bài cho một bộ. 
7. In số đã tính. 

Tính chính xác đến từ việc ánh xạ đầy đủ các điểm cần thiết vào tập hợp các giá trị quân bài có thể có, xem xét các giá trị đặc biệt (10 điểm cho quân bài mặt và giá trị kép của quân Át) và quân hậu đã được sử dụng. Mỗi bước xử lý tất cả các kết quả số có thể xảy ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
needed = n - 10

if needed < 1 or needed > 11:
    print(0)
elif needed == 10:
    # tens, jacks, queens, kings: 16 total, minus the queen of spades
    print(15)
elif needed == 11:
    # only aces counted as 11
    print(4)
else:
    # 2-9 and ace as 1
    print(4)
```Mã bắt đầu bằng cách tính toán sự khác biệt giữa tổng mục tiêu và giá trị của quân hậu. Các nhánh có điều kiện xử lý các giá trị không thể, thẻ 10 điểm (trừ quân hậu), quân Át là 11 và thẻ số. Không có vòng lặp vì ánh xạ là trực tiếp, tránh sai sót trong việc đếm các bộ quần áo. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
12
```| Biến | Giá trị | 
| --- | --- | 
| n | 12 | 
| cần thiết | 2 | 
| Đầu ra | 4 | 

Giải thích: Để đạt được 12, lá bài thứ hai phải có giá trị 2 điểm. Có bốn twos (trái tim, kim cương, câu lạc bộ, thuổng). Mỗi cái đều hợp lệ. 

### Đầu vào mẫu 2```
20
```| Biến | Giá trị | 
| --- | --- | 
| n | 20 | 
| cần thiết | 10 | 
| Đầu ra | 15 | 

Giải thích: Lá bài thứ hai phải là 10 điểm. Số hàng chục, số jack, quân hậu và quân vua. Tổng cộng có 16 quân bài, nhưng quân hậu đã được chơi, còn lại 15 quân. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ kiểm tra số học và có điều kiện; không có vòng lặp trên thẻ | 
| Không gian | O(1) | Không gian không đổi cho một vài biến số nguyên | 

Với kích thước sàn cố định và nhỏ`n`giá trị, giải pháp này có hiệu quả tức thời và sử dụng bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline())
    needed = n - 10
    if needed < 1 or needed > 11:
        return "0"
    elif needed == 10:
        return "15"
    elif needed == 11:
        return "4"
    else:
        return "4"

# Provided samples
assert run("12\n") == "4", "sample 1"
assert run("20\n") == "15", "sample 2"
assert run("10\n") == "0", "sample 3"

# Custom cases
assert run("1\n") == "0", "minimum impossible"
assert run("11\n") == "4", "ace counted as 1"
assert run("21\n") == "4", "ace counted as 11"
assert run("15\n") == "4", "numeric card in range 2-9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | n quá thấp để đạt 10 điểm | 
| 11 | 4 | quân Át được tính là 1 | 
| 21 | 4 | quân Át được tính là 11 | 
| 15 | 4 | thẻ số trong phạm vi 2-9 | 

## Vỏ cạnh 

cho`n = 1`,`needed = -9`. Vì không có thẻ nào có thể trừ điểm nên kết quả đầu ra chính xác là 0. Đối với`n = 21`,`needed = 11`. Chỉ quân Át được tính là 11 là đủ; cả bốn bộ đều hợp lệ, xác nhận việc xử lý quân Át chính xác. Vì`n = 20`,`needed = 10`. Việc trừ quân Hậu sẽ đảm bảo chúng ta không đếm quá nhiều quân bài 10 điểm. Mỗi điều này thể hiện thuật toán xử lý chính xác các điều kiện biên và giá trị thẻ đặc biệt.
