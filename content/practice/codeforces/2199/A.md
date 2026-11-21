---
title: "CF 2199A - Trò chơi"
description: "Alice và Bob chơi một trò chơi bài ba vòng trong đó mỗi vòng có điểm độc lập từ 0 đến k tối đa. Hai vòng đầu tiên đã diễn ra, Alice và Bob đạt điểm a1, b1 ở vòng một và a2, b2 ở vòng hai."
date: "2026-06-07T20:22:56+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 2199
codeforces_index: "A"
codeforces_contest_name: "Kotlin Heroes: Episode 14"
rating: 1000
weight: 2199
solve_time_s: 196
verified: true
draft: false
---

[CF 2199A - Trò chơi](https://codeforces.com/problemset/problem/2199/A) 

**Đánh giá:** 1000 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 3 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Alice và Bob chơi một trò chơi bài ba vòng trong đó mỗi vòng có điểm số độc lập giữa`0`và tối đa`k`. Hai hiệp đầu tiên đã diễn ra và Alice và Bob đã ghi điểm`a1, b1`ở vòng một và`a2, b2`ở vòng hai. Vòng thứ ba vẫn chưa diễn ra nhưng điểm số phải khác nhau và mỗi bên đều nằm trong`[0, k]`. Mục tiêu là để xác định xem Bob có kịch bản nào ở vòng thứ ba cho phép anh ta thắng trò chơi hay không, hay Alice thắng bất kể kết quả của vòng thứ ba như thế nào. 

Chiến thắng được xác định chủ yếu bằng tổng số điểm trong cả ba vòng. Nếu tổng số điểm bằng nhau, người chơi nào thắng nhiều ván hơn trong số ba ván sẽ thắng. Điều này có nghĩa là vòng thứ ba có khả năng lật đổ người chiến thắng nếu chênh lệch điểm đủ nhỏ hoặc nếu Alice và Bob có số trận thắng ở vòng bằng nhau cho đến nay. 

Những hạn chế là nhỏ:`k`nhiều nhất là 50 và có tới`10^4`trường hợp thử nghiệm. Điều này làm cho việc mô phỏng toàn diện trở nên khả thi vì vòng thứ ba chỉ có tối đa 51 khả năng cho mỗi người chơi và mỗi trường hợp thử nghiệm có thể được đánh giá độc lập. Các trường hợp có lợi thế không rõ ràng bao gồm các tình huống trong đó Alice dẫn trước đúng một điểm tổng cộng hoặc đã thắng sít sao cả hai hiệp, bởi vì những thao tác điểm số nhỏ ở hiệp ba có thể thay đổi người chiến thắng hoặc người hòa. 

Ví dụ, nếu`k=5`,`a1=3, b1=4`,`a2=2, b2=1`, Bob vẫn có thể thắng khi ghi được 5 điểm ở vòng ba trong khi Alice ghi được 0 điểm, mặc dù Alice đã thắng ở vòng thứ hai. Việc thực hiện bất cẩn có thể chỉ so sánh tổng điểm và bỏ qua các ván thắng, đưa ra phán quyết sai khi tổng điểm hòa. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ là bạo lực: liệt kê mọi cặp điểm có thể`(a3, b3)`cho vòng thứ ba nơi`0 ≤ a3, b3 ≤ k`Và`a3 ≠ b3`, sau đó tính tổng điểm và đếm số vòng thắng cho từng kịch bản. Nếu Bob thắng trong bất kỳ sự kết hợp nào, xuất ra`YES`; nếu không thì,`NO`. Điều này hoạt động vì`k`nhỏ, nhưng trong trường hợp xấu nhất, đây là O(k^2) cho mỗi trường hợp thử nghiệm, điều này phù hợp với`k=50`Và`t=10^4`nhưng có thể được tối ưu hóa. 

Quan sát quan trọng là chỉ có điểm số cực cao mới quan trọng. Cơ hội tốt nhất để Bob giành chiến thắng là tối đa hóa số điểm của anh ấy ở vòng thứ ba trong khi giảm thiểu số điểm của Alice. Ngược lại, Alice thắng bất kể ngay cả trong trường hợp tối ưu của Bob, cô ấy vẫn có tổng điểm cao hơn hoặc không thể thua người hòa ở các hiệp thắng. Điều này làm giảm vấn đề thành một kiểm tra đơn giản: tính chênh lệch điểm tối đa mà Bob có thể tạo ra ở vòng thứ ba (`b3 - a3 = k - 0 = k`), thêm nó vào vị trí dẫn đầu (hoặc thâm hụt) hiện tại của anh ấy và so sánh tổng số và số vòng thắng. Nếu Bob không thể vượt qua Alice về tổng điểm hoặc không thể thắng thêm vòng ngay cả khi có delta tối đa, Alice sẽ thắng vô điều kiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k^2) cho mỗi trường hợp thử nghiệm | O(1) | Được chấp nhận nhưng có thể đơn giản hóa | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`k`và điểm số`a1, b1`Và`a2, b2`. 
3. Tính tổng số điểm hiện tại của Alice (`totalA = a1 + a2`) và Bob (`totalB = b1 + b2`). 
4. Đếm số vòng Alice thắng (`roundA`) và Bob đã thắng (`roundB`) trong hai vòng đầu tiên bằng cách so sánh`a1 > b1`Và`a2 > b2`. 
5. Tính mức chênh lệch điểm tối đa mà Bob có thể đạt được ở vòng ba, đó là`k - 0 = k`. 
6. Kiểm tra xem Bob có thể vượt qua tổng số điểm của Alice hay không bằng cách thêm`k`ĐẾN`totalB`và giả sử Alice đạt 0 điểm ở vòng ba. Nếu như`totalB + k > totalA`, đầu ra`YES`. 
7. Nếu tổng số điểm có thể hòa sau hiệp thứ ba, hãy kiểm tra xem hiệp đó thắng. Bob phải có khả năng giành được nhiều ván thắng hơn Alice. Nếu chênh lệch thắng ở vòng đấu quá lớn khiến Bob không thể vượt qua trong một vòng, hãy xuất ra`NO`. 
8. Nếu cả tổng số lẫn số vòng đều không cho phép Bob thắng, hãy xuất ra`NO`. 

Tại sao nó hoạt động: Điều bất biến là chiến thắng vô điều kiện của Alice chỉ xảy ra nếu ngay cả khi Bob nỗ lực tối đa ở vòng thứ ba, cô ấy vẫn có tổng điểm cao hơn hoặc thắng người hòa theo từng hiệp. Điều này đảm bảo tính chính xác vì chúng ta chỉ cần xem xét kịch bản tính điểm cực đoan cho Bob, tránh việc liệt kê không cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    k = int(input())
    a1, b1 = map(int, input().split())
    a2, b2 = map(int, input().split())
    
    totalA = a1 + a2
    totalB = b1 + b2
    
    roundsA = (a1 > b1) + (a2 > b2)
    roundsB = (b1 > a1) + (b2 > a2)
    
    # Maximum Bob can score and minimum Alice can score in round 3
    maxBob = totalB + k
    minAlice = totalA + 0
    
    if maxBob > minAlice:
        print("YES")
    elif maxBob == minAlice:
        # Check if Bob can win the tie-breaker (round wins)
        # Bob can only win one additional round in third round
        if roundsB + 1 > roundsA:
            print("YES")
        else:
            print("NO")
    else:
        print("NO")
```Mã tính toán tổng số tiền thắng trong hai vòng đầu tiên. Sau đó, nó mô phỏng vòng thứ ba có khả năng xảy ra cao nhất để kiểm tra xem Bob có thể thắng theo tổng điểm hay theo số vòng hay không. Sử dụng so sánh trực tiếp sẽ tránh được việc liệt kê đầy đủ, đơn giản hóa logic trong khi vẫn duy trì tính chính xác. Các trường hợp biên như`a3=0, b3=k`vốn đã được xử lý vì chúng tôi giả định điều tốt nhất cho Bob. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:`k=6`,`a1=2, b1=3`,`a2=1, b2=4`| tổngA | tổngB | vòngA | vòngB | maxBob | phútAlice | Quyết định | 
| --- | --- | --- | --- | --- | --- | --- | 
| 3 | 7 | 0 | 2 | 13 | 3 | CÓ | 

Bob có thể ghi được 6 điểm ở vòng 3 và Alice ghi được 0. TotalB = 7 + 6 = 13 > 3, vậy Bob có kịch bản thắng. 

**Ví dụ 2** 

đầu vào:`k=3`,`a1=3, b1=1`,`a2=3, b2=1`| tổngA | tổngB | vòngA | vòngB | maxBob | phútAlice | Quyết định | 
| --- | --- | --- | --- | --- | --- | --- | 
| 6 | 2 | 2 | 0 | 5 | 6 | KHÔNG | 

Ngay cả khi Bob đạt 3 điểm ở vòng 3 và Alice đạt 0 điểm thì tổng B = 5 < 6. Alice thắng vô điều kiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm được xử lý trong thời gian không đổi vì chúng tôi chỉ tính tổng và so sánh | 
| Không gian | O(1) | Chỉ một số biến cố định được sử dụng cho mỗi trường hợp thử nghiệm | 

Được cho`t ≤ 10^4`, thuật toán này dễ dàng chạy trong giới hạn 2 giây và chiếm dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        k = int(input())
        a1, b1 = map(int, input().split())
        a2, b2 = map(int, input().split())
        totalA = a1 + a2
        totalB = b1 + b2
        roundsA = (a1 > b1) + (a2 > b2)
        roundsB = (b1 > a1) + (b2 > a2)
        maxBob = totalB + k
        minAlice = totalA + 0
        if maxBob > minAlice:
            output.append("YES")
        elif maxBob == minAlice:
            output.append("YES" if roundsB + 1 > roundsA else "NO")
        else:
            output.append("NO")
    return "\n".join(output)

# Provided samples
assert run("5\n6\n2 3\n1 4\n5\n3 1\n3 1\n3\n3 1\n3 1\n10\n0 1\n10 0\n4\n3 1\n3 1\n") == "YES\nYES\nNO\nYES\nNO"

# Custom cases
assert run("1\n1\n0 1\n0 1\n") == "YES" # Bob can tie in totals but win last round
assert run("1\n50\n50 0\n50 0\n") == "NO" # Alice already guaranteed win
assert run("1\n10\n5 5\n5 5\n") == "YES" # Close totals, Bob can surpass
```
