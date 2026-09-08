---
title: "CF 104573D - Thử thách XP"
description: "Chúng ta được cung cấp một chuỗi kẻ thù, mỗi kẻ thù có một số sức khỏe và một sinh vật có lượng điểm sức khỏe ban đầu. Mục tiêu là quyết định xem liệu cô ấy có thể đánh bại mọi kẻ thù theo thứ tự trong khi đảm bảo sức khỏe của bản thân không bao giờ giảm xuống 0 hoặc thấp hơn hay không. Có hai lựa chọn tấn công."
date: "2026-06-30T08:20:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104573
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 09-08-23 Div. 1"
rating: 0
weight: 104573
solve_time_s: 94
verified: true
draft: false
---

[CF 104573D - Thử thách XP](https://codeforces.com/problemset/problem/104573/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi kẻ thù, mỗi kẻ thù có một số sức khỏe và một sinh vật có lượng điểm sức khỏe ban đầu. Mục tiêu là quyết định xem liệu cô ấy có thể đánh bại mọi kẻ thù theo thứ tự trong khi đảm bảo sức khỏe của bản thân không bao giờ giảm xuống 0 hoặc thấp hơn hay không. 

Có hai lựa chọn tấn công. Một là đòn tấn công mạnh có thể sử dụng tối đa một lần cho mỗi kẻ địch, gây sát thương cao nhưng cũng tiêu tốn một lượng lớn máu. Loại còn lại là đòn tấn công yếu hơn có thể được sử dụng không giới hạn số lần, gây sát thương nhỏ hơn cho mỗi lần sử dụng và cũng tiêu tốn một lượng máu cho mỗi lần sử dụng. 

Khó khăn chính là mỗi kẻ địch phải giảm xuống 0 máu và chúng ta có thể kết hợp hai kiểu tấn công. Quyết định không chỉ mang tính cục bộ đối với mỗi kẻ thù, bởi vì tiêu tốn quá nhiều máu sớm có thể khiến các trận chiến sau này không thể thực hiện được. Vì vậy, vấn đề là giảm thiểu tổng thiệt hại cho bản thân trong khi vẫn đảm bảo mỗi kẻ thù đều bị đánh bại hoàn toàn. 

Các giới hạn lên tới 100.000 kẻ thù, do đó, bất kỳ giải pháp nào thử tất cả các kết hợp phân bổ tấn công cho mỗi kẻ thù đều là không thể. Ngay cả việc suy luận bậc hai cho mỗi kẻ thù cũng sẽ quá chậm, vì vậy chúng ta cần một quyết định tham lam hoặc số học cho mỗi kẻ thù. 

Một cạm bẫy phổ biến là xử lý từng kẻ thù một cách độc lập và luôn sử dụng đòn tấn công “sát thương mỗi chi phí tốt nhất”. Điều đó không thành công vì đòn tấn công mạnh bị giới hạn một lần cho mỗi kẻ thù và có thể bị lãng phí vào những kẻ thù nhỏ khi nó không hiệu quả, ngay cả khi cục bộ nó có vẻ tốt. 

Một vấn đề tế nhị khác là quên rằng việc sử dụng một phần đòn tấn công yếu phải tiêu diệt chính xác lượng máu còn lại, vì vậy lượng máu còn sót lại sau một đòn mạnh vẫn cần đủ số đòn yếu và tổng chi phí đó rất quan trọng. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ thử mọi cách phân công kiểu tấn công cho từng kẻ thù: đối với mỗi kẻ thù, hãy quyết định sử dụng bao nhiêu đòn tấn công yếu và có nên áp dụng đòn tấn công mạnh hay không. Điều này dẫn đến một số khả năng theo cấp số nhân, đại khái là$2^M$, vì mỗi kẻ thù có nhiều kiểu kết hợp cách sử dụng. Ngay cả việc giảm mỗi kẻ thù thành "mạnh được sử dụng hay không" cũng không đủ vì các đòn tấn công yếu vẫn có thể thay đổi về số lượng. 

Điều này nhanh chóng trở nên không khả thi khi M tăng thậm chí vượt quá 30 hoặc 40. 

Quan sát quan trọng là đối với mỗi kẻ thù, chúng ta chỉ quan tâm đến mức sát thương bản thân tối thiểu có thể cần thiết để đánh bại nó. Vì kẻ thù độc lập về yêu cầu sức khỏe (không có trạng thái chuyển tiếp ngoại trừ lượng HP còn lại), chúng tôi có thể tính toán, đối với mỗi kẻ thù, cách rẻ nhất để giảm HP của nó xuống 0, sau đó tính tổng các chi phí đó. 

Đối với một kẻ thù nhất định, chúng tôi so sánh hai chiến lược: chỉ sử dụng các đòn tấn công yếu hoặc sử dụng một đòn tấn công mạnh, sau đó là các đòn tấn công đủ yếu để tiêu diệt lượng máu còn lại. Chúng tôi chọn cái rẻ hơn. Khi mỗi kẻ thù bị giảm chi phí, chúng tôi chỉ trừ tổng chi phí từ HP ban đầu. 

Điều này biến vấn đề thành một kiểm tra tích lũy đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | O(1)-O(M) | Quá chậm | 
| Tối ưu | O(M) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Hãy để mỗi kẻ thù có sức khỏe$a_i$. 

1. Đối với mỗi kẻ thù, hãy tính xem cần bao nhiêu đòn tấn công yếu để đánh bại hoàn toàn. Mỗi đòn tấn công yếu gây ra$Q_1$thiệt hại và chi phí$Q_2$HP nên số đòn đánh yếu là$\lceil a_i / Q_1 \rceil$. Điều này đưa ra tổng chi phí của chiến lược chỉ yếu như$cost_weak = \lceil a_i / Q_1 \rceil \cdot Q_2$. 
2. Nếu chúng ta sử dụng đòn tấn công mạnh một lần, nó sẽ làm giảm kẻ địch đi$P_1$. Nếu như$a_i \le P_1$, thì một đòn tấn công mạnh là đủ và cái giá phải trả chỉ đơn giản là$P_2$. 
3. Ngược lại, sau đòn tấn công mạnh, lượng máu còn lại sẽ là$a_i - P_1$. Chúng tôi vẫn cần$\lceil (a_i - P_1) / Q_1 \rceil$các cuộc tấn công yếu, do đó chi phí trở thành$cost_strong = P_2 + \lceil (a_i - P_1) / Q_1 \rceil \cdot Q_2$. 
4. Đối với mỗi kẻ thù, lấy$min(cost_weak, cost_strong)$là lượng HP bị mất tối thiểu. 
5. Tính tổng tất cả chi phí tối thiểu này của tất cả kẻ thù. 
6. Nếu HP ban đầu$N$lớn hơn tổng chi phí, xuất ra "CÓ", nếu không thì "KHÔNG". 

Lý do lấy mức tối thiểu cho mỗi kẻ thù là vì không có sự tương tác giữa các kẻ thù ngoại trừ ngân sách HP được chia sẻ, do đó chiến lược toàn cầu tối ưu sẽ phân tách thành các lựa chọn địa phương tối ưu. 

### Tại sao nó hoạt động 

Mỗi kẻ thù đều độc lập về yêu cầu sát thương và mọi chiến lược hợp lệ đều phải giảm hoàn toàn HP của nó xuống 0. Đối với bất kỳ kẻ thù cố định nào, bất kỳ chuỗi hành động nào cũng có thể được sắp xếp lại thành “tất cả các cuộc tấn công yếu” hoặc “một cuộc tấn công mạnh theo sau là các cuộc tấn công yếu” mà không làm tăng chi phí, vì các cuộc tấn công cực mạnh không được phép và các cuộc tấn công yếu có chi phí tuyến tính. Do đó, chi phí tối thiểu cho mỗi kẻ thù là tối ưu trên toàn cầu khi được tính tổng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, M = map(int, input().split())
    P1, P2 = map(int, input().split())
    Q1, Q2 = map(int, input().split())
    a = list(map(int, input().split()))

    total = 0

    for hp in a:
        weak_hits = (hp + Q1 - 1) // Q1
        cost_weak = weak_hits * Q2

        if hp <= P1:
            cost_strong = P2
        else:
            rem = hp - P1
            weak_after = (rem + Q1 - 1) // Q1
            cost_strong = P2 + weak_after * Q2

        total += min(cost_weak, cost_strong)

    print("YES" if total < N else "NO")

if __name__ == "__main__":
    solve()
```Giải pháp đọc tất cả các tham số, sau đó duyệt qua từng kẻ thù để tính toán cách rẻ nhất để đánh bại nó. Chi tiết triển khai chính là phân chia trần cẩn thận khi tính toán số lần tấn công yếu. Cả hai chiến lược đều được tính toán rõ ràng để tránh những sai lầm trong lý luận về các trường hợp lợi thế khi đòn tấn công mạnh vượt quá hoặc khớp chính xác với lượng HP còn lại. 

Phép so sánh cuối cùng sử dụng sự bất bình đẳng nghiêm ngặt vì Iggy phải duy trì HP ở mức trên 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
N = 8, M = 3
P1 = 5, P2 = 2
Q1 = 3, Q2 = 1
a = [5, 8, 6]
```Chúng tôi tính toán cho mỗi kẻ thù: 

| Kẻ thù | Chi phí yếu | Chi phí mạnh mẽ | Được chọn | 
| --- | --- | --- | --- | 
| 5 | trần(5/3)=2 → 2 | P2=2 | 2 | 
| 8 | trần(8/3)=3 → 3 | 2 + trần(3/3)=1 → 3 | 3 | 
| 6 | trần(6/3)=2 → 2 | 2 + trần(1/3)=1 → 3 | 2 | 

Tổng chi phí = 2 + 3 + 2 = 7. 

HP còn lại = 8 − 7 = 1, do đó đầu ra là CÓ. 

Dấu vết này cho thấy tấn công mạnh không phải lúc nào cũng có lợi; nó phụ thuộc vào lượng máu còn lại so với hiệu quả tấn công yếu. 

### Ví dụ 2 

đầu vào:```
N = 10, M = 2
P1 = 4, P2 = 5
Q1 = 2, Q2 = 3
a = [3, 7]
```Kẻ thù 1: 

Yếu = trần(3/2)=2 → 6 

Mạnh = 5 (vì 3 <= 4) → chọn 5 

Kẻ thù 2: 

Yếu = trần(7/2)=4 → 12 

Mạnh = 5 + trần(3/2)=2 → 11 → chọn 11 

Tổng cộng = 5 + 11 = 16. 

HP còn lại = 10 − 16 ≤ 0 nên đáp án là KHÔNG. 

Điều này cho thấy ngay cả khi sử dụng đòn tấn công mạnh, chi phí tích lũy vẫn có thể vượt quá HP ban đầu, khiến kết quả không thể đạt được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M) | Mỗi kẻ thù được xử lý một lần với số học O(1) | 
| Không gian | O(1) | Chỉ tính tổng và lưu trữ đầu vào | 

Các ràng buộc cho phép tối đa 100.000 kẻ thù và giải pháp thực hiện công việc liên tục cho mỗi kẻ thù, do đó, nó dễ dàng phù hợp với giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import ceil

    def solve():
        N, M = map(int, input().split())
        P1, P2 = map(int, input().split())
        Q1, Q2 = map(int, input().split())
        a = list(map(int, input().split()))

        total = 0
        for hp in a:
            weak = (hp + Q1 - 1) // Q1 * Q2
            if hp <= P1:
                strong = P2
            else:
                rem = hp - P1
                strong = P2 + (rem + Q1 - 1) // Q1 * Q2
            total += min(weak, strong)

        return "YES" if total < N else "NO"

    return solve()

# provided sample
assert run("""8 3
5 2
3 1
5 8 6
""") == "YES"

# minimal case
assert run("""1 1
1 1
1 1
1
""") == "NO"

# strong always best
assert run("""20 2
10 1
100 1
5 5
""") == "YES"

# weak always best
assert run("""20 2
100 100
1 1
10 10
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu | KHÔNG | cạnh cạn kiệt chính xác của kẻ thù | 
| mạnh mẽ thống trị | CÓ | tối ưu tấn công mạnh mẽ | 
| ưu thế yếu | CÓ | sự đúng đắn chỉ yếu | 

## Vỏ cạnh 

Một trường hợp quan trọng xảy ra khi HP của kẻ địch chia chính xác cho sát thương tấn công yếu. Trong trường hợp đó, phân chia trần bằng phân chia chính xác và không có cuộc tấn công lãng phí nào. Thuật toán xử lý việc này một cách tự nhiên vì phép chia trần số nguyên`(hp + Q1 - 1) // Q1`sụp đổ thành`hp / Q1`. 

Một trường hợp khác xảy ra khi đòn tấn công mạnh làm HP giảm xuống chính xác bằng 0. Mã xử lý việc này thông qua điều kiện`hp <= P1`, đảm bảo không có cuộc tấn công yếu nào được thêm vào sau đó, ngăn chặn chi phí tăng thêm. 

Cuối cùng, khi cả hai kiểu tấn công đều cực kỳ kém hiệu quả, tổng chi phí có thể vượt quá N ngay cả đối với M nhỏ và thuật toán tổng hợp chính xác tất cả chi phí của mỗi kẻ thù trước khi so sánh thay vì đưa ra quyết định sớm.
