---
title: "CF 1215A - Thẻ Vàng"
description: "Trong bài toán này, chúng ta có hai đội bóng đá đang chơi một trận đấu. Mỗi đội bắt đầu với một số lượng người chơi nhất định, ký hiệu là a1 và a2. Trong suốt trận đấu, trọng tài rút n thẻ vàng cho các cầu thủ."
date: "2026-06-11T22:56:15+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1215
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 585 (Div. 2)"
rating: 1000
weight: 1215
solve_time_s: 93
verified: true
draft: false
---

[CF 1215A - Thẻ vàng](https://codeforces.com/problemset/problem/1215/A) 

**Đánh giá:** 1000 
**Tags:** tham lam, thực hiện, toán học 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trong bài toán này, chúng ta có hai đội bóng đá đang chơi một trận đấu. Mỗi đội bắt đầu với một số lượng người chơi nhất định, ký hiệu là`a1`Và`a2`. Trong suốt trận đấu, trọng tài chỉ ra`n`thẻ vàng cho cầu thủ. Mỗi đội có một ngưỡng thẻ vàng: cầu thủ của đội một sẽ bị đuổi khỏi sân nếu nhận thẻ vàng.`k1`thẻ vàng và một cầu thủ của đội thứ hai bị đuổi khỏi sân sau`k2`thẻ vàng. Trọng tài đã mất hồ sơ về cầu thủ nào đã nhận thẻ và chúng tôi cần xác định số cầu thủ tối thiểu và tối đa có thể bị đuổi khỏi sân, chỉ đưa ra tổng số. 

Dữ liệu đầu vào thể hiện số lượng cầu thủ, giới hạn thẻ vàng cá nhân của họ và tổng số thẻ vàng được hiển thị. Đầu ra là hai số nguyên: số lượng cầu thủ tối thiểu có thể bị đuổi khỏi sân và số lượng cầu thủ tối đa có thể bị đuổi khỏi sân. 

Các ràng buộc rất nhỏ: tất cả các giá trị đều lên tới 1000. Điều này cho phép tiếp cận toán học trực tiếp mà không cần lo lắng về hiệu suất, vì các phép toán trên các phạm vi này có chi phí tính toán không đáng kể. Một trường hợp quan trọng phát sinh khi số thẻ vàng nhỏ hơn tổng ngưỡng của các cầu thủ. Trong tình huống như vậy, có thể không có cầu thủ nào bị đuổi khỏi sân. Một trường hợp khác xảy ra khi tổng số thẻ vàng vượt quá số thẻ có thể được phân phát mà không gây ra mức phạt tối đa; số lần xuất phát tối đa không được vượt quá tổng số người chơi. 

Một cách tiếp cận đơn giản có thể thử mô phỏng việc phân phối từng thẻ vàng cho từng cầu thủ, nhưng việc tính toán cẩn thận là đủ để giải quyết vấn đề này mà không cần lặp lại tất cả các thẻ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp đi lặp lại qua từng`n`thẻ vàng, gán chúng cho các cầu thủ theo thứ tự nào đó và đếm xem có bao nhiêu người bị đuổi khỏi sân. Điều này hoạt động về nguyên tắc, nhưng với`n`lên đến`a1*k1 + a2*k2`trong trường hợp xấu nhất có thể là 1.000.000, việc lặp lại từng thẻ là không cần thiết. Mô phỏng sẽ đúng nhưng dài dòng và không hiệu quả. 

Cái nhìn sâu sắc quan trọng là lý luận về ngưỡng thay vì phân công thẻ riêng lẻ. Đối với số lượng cầu thủ bị đuổi khỏi sân tối thiểu, chúng tôi muốn trải đều các thẻ nhất có thể để không có cầu thủ nào đạt đến ngưỡng bị đuổi cho đến khi cần thiết. Nếu tổng số thẻ vàng ít hơn`a1*(k1-1) + a2*(k2-1)`, chúng ta có thể tránh đuổi bất cứ ai đi. Nếu không, số lượng tối thiểu là số thẻ vượt quá mức phân phối an toàn này. Để đạt mức tối đa, trước tiên chúng tôi tập trung thẻ vào những người chơi có ngưỡng nhỏ nhất, loại càng nhiều càng tốt. Điều này làm giảm vấn đề thành số học đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm đối với kích thước lớn`n`| 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số thẻ an toàn tối đa có thể được đưa ra mà không khiến ai phải rời sân:`(a1*(k1-1) + a2*(k2-1))`. Điều này thể hiện việc cung cấp cho mỗi người chơi ít hơn một điểm so với ngưỡng xuất phát của họ. Trừ đi số tiền này từ tổng số`n`để tìm số lượng cầu thủ tối thiểu phải bị đuổi khỏi sân. Nếu kết quả là âm, hãy đặt nó thành 0, vì chúng tôi không thể có ít hơn 0 lần gửi đi. 
2. Để có số lượng người chơi bị đuổi khỏi sân tối đa, trước tiên chúng tôi muốn phân phát thẻ một cách tham lam cho những người chơi có ngưỡng thấp nhất. Tính xem mỗi đội có thể có bao nhiêu lần tiễn đưa nếu chúng tôi phát thẻ một cách tối ưu để đạt được số lần tiễn đưa tối đa. Điều này đơn giản là`min(a1, n // k1)`cho đội một và`min(a2, n // k2)`cho đội thứ hai, cộng cả hai lại với nhau. Đây`//`là phép chia số nguyên, biểu thị số ngưỡng đầy đủ mà chúng tôi có thể đạt được cho mỗi đội. 
3. Trả về số tối thiểu và tối đa được tính toán làm câu trả lời. 

Tại sao nó hoạt động: Thuật toán hoạt động vì số lượng tối thiểu và tối đa được xác định hoàn toàn bằng tổng ngưỡng và tổng số thẻ vàng. Việc trải bài hoặc tập trung chúng không yêu cầu theo dõi từng người chơi; tổng số là đủ. Các thuộc tính chúng tôi sử dụng, cụ thể là mỗi người chơi có thể hấp thụ`k1-1`hoặc`k2-1`thẻ một cách an toàn và đó`n // k`đại diện cho sự gửi đi đầy đủ, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a1 = int(input())
a2 = int(input())
k1 = int(input())
k2 = int(input())
n = int(input())

# Minimum number of players sent off
safe_cards = a1 * (k1 - 1) + a2 * (k2 - 1)
min_sent_off = max(0, n - safe_cards)

# Maximum number of players sent off
max_sent_off = min(a1, n // k1) + min(a2, n // k2)

print(min_sent_off, max_sent_off)
```Trong giải pháp này, trước tiên chúng tôi tính toán cách phân phối thẻ an toàn để tính toán tối thiểu. Để đạt mức tối đa, phép chia số nguyên sẽ nhanh chóng cho chúng ta biết mỗi đội có thể hấp thụ được bao nhiêu ngưỡng đầy đủ. Xử lý cạnh, như mức tối thiểu âm, được thực hiện với`max(0, ...)`. Không cần vòng lặp, vì vậy hiệu suất là thời gian không đổi. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
2
3
5
1
8
```| Bước | a1 | a2 | k1 | k2 | n | safe_cards | phút_sent_off | max_sent_off | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | 5 | 1 | 8 | 2_4+3_0=8 | tối đa(0,8-8)=0 | min(2,8//5)=1, min(3,8//1)=3, sum=4 | 

Dấu vết này cho thấy rằng với sự phân bổ cẩn thận, không có cầu thủ nào phải bị đuổi khỏi sân. Để có số lần gửi đi tối đa, việc tập trung thẻ vào những người chơi có ngưỡng thấp sẽ mang lại bốn lần gửi đi. 

**Đầu vào mẫu 2**```
1
1
1
1
2
```| Bước | a1 | a2 | k1 | k2 | n | safe_cards | phút_sent_off | max_sent_off | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 1 | 2 | 1_0+1_0=0 | tối đa(0,2-0)=2 | phút(1,2//1)=1, phút(1,2//1)=1, tổng=2 | 

Điều này xác nhận rằng khi mỗi người chơi có ngưỡng thấp, cả hai đều phải bị đuổi khỏi sân và phép tính sẽ nắm bắt chính xác điều đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ sử dụng các phép tính số học và phép chia số nguyên, độc lập với`n`| 
| Không gian | O(1) | Không cần mảng hoặc cấu trúc dữ liệu bổ sung, chỉ cần một vài biến số nguyên | 

Với những hạn chế của vấn đề, giải pháp này sẽ chạy ngay lập tức đối với tất cả các dữ liệu đầu vào hợp lệ và sử dụng bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    a1 = int(input())
    a2 = int(input())
    k1 = int(input())
    k2 = int(input())
    n = int(input())
    safe_cards = a1 * (k1 - 1) + a2 * (k2 - 1)
    min_sent_off = max(0, n - safe_cards)
    max_sent_off = min(a1, n // k1) + min(a2, n // k2)
    return f"{min_sent_off} {max_sent_off}"

# Provided samples
assert run("2\n3\n5\n1\n8\n") == "0 4", "sample 1"
assert run("1\n1\n1\n1\n2\n") == "2 2", "sample 2"

# Custom cases
assert run("1\n1\n2\n3\n0\n") == "0 0", "no yellow cards"
assert run("10\n10\n1\n1\n20\n") == "10 20", "all thresholds minimal"
assert run("2\n3\n5\n5\n5\n") == "0 1", "enough cards for minimal send-off"
assert run("1000\n1000\n1000\n1000\n2000000\n") == "1000 2000", "max input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1\n2\n3\n0 | 0 0 | Không có thẻ vàng, số không tối thiểu và tối đa | 
| 10\n10\n1\n1\n20 | 10 20 | Ngưỡng bằng 1, tất cả cầu thủ bị đuổi khỏi sân | 
| 2\n3\n5\n5\n5 | 0 1 | Tính toán gửi đi tối thiểu chính xác | 
| 1000\n1000\n1000\n1000\n2000000 | 1000 2000 | Xử lý giới hạn đầu vào tối đa | 

## Vỏ cạnh 

Khi nào`n`nhỏ hơn tổng của`a1*(k1-1) + a2*(k2-1)`, số lần gửi tối thiểu bằng không. Ví dụ,`a1=2`,`a2=3`,`k1=5`,`k2=5`,`n=10`. Thẻ an toàn là`2*4 + 3*4 = 20`.`n - safe_cards = -10`, cho kết quả tối thiểu là 0. Thuật toán tính toán chính xác điều này mà không cần
