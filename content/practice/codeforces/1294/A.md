---
title: "CF 1294A - Thu thập tiền xu"
description: "Chúng ta được yêu cầu quyết định xem liệu một số đồng xu nhất định có thể được chia cho ba chị em để cuối cùng họ có tổng số xu như nhau hay không. Mỗi chị em đã có sẵn một số đồng xu: Alice có a, Barbara có b và Cerene có c."
date: "2026-06-11T18:40:03+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1294
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 615 (Div. 3)"
rating: 800
weight: 1294
solve_time_s: 109
verified: true
draft: false
---

[CF 1294A - Thu thập tiền xu](https://codeforces.com/problemset/problem/1294/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu quyết định xem liệu một số đồng xu nhất định có thể được chia cho ba chị em để cuối cùng họ có tổng số xu như nhau hay không. Mỗi chị em đã có sẵn một số đồng xu: Alice có`a`, Barbara có`b`, và Cerene có`c`. Polycarp mang lại`n`những đồng xu thừa, và anh ta có thể đưa cho mỗi em gái bất kỳ số không âm nào của những đồng xu này. Câu hỏi đặt ra là liệu có cách nào để phân phối tất cả`n`tiền xu vậy đó`a + A = b + B = c + C`, Ở đâu`A + B + C = n`Và`A, B, C ≥ 0`. 

Mỗi trường hợp kiểm thử là độc lập và số lượng có thể lớn tới 10^8, với tối đa 10^4 trường hợp kiểm thử. Điều này cho chúng ta biết rằng bất kỳ giải pháp nào lặp lại tất cả các đợt phân phối tiền có thể có đều quá chậm. Chúng tôi cần một cách tiếp cận O(1) cho mỗi trường hợp thử nghiệm. 

Các trường hợp đặc biệt cần chú ý bao gồm các tình huống trong đó một chị em đã có nhiều xu hơn những người khác hoặc tổng số xu cần thiết để cân bằng chúng không chia đều. Ví dụ: nếu hai chị em có 1, 2, 3 đồng xu và`n = 3`, một cách tiếp cận ngây thơ chỉ đơn giản là cộng từng đồng xu một có thể cho rằng điều đó là có thể một cách không chính xác, nhưng tính toán cẩn thận cho thấy rằng bạn không thể phân phối 3 đồng xu để làm cho tất cả các tổng bằng nhau vì khoảng cách lớn nhất là 2 và bạn không thể chia đều phần còn lại. 

## Phương pháp tiếp cận 

Một phương pháp vũ phu sẽ thử mọi sự kết hợp của`(A, B, C)`như vậy`A + B + C = n`. Về nguyên tắc, điều này sẽ có hiệu quả vì chúng ta chỉ cần kiểm tra các nghiệm số nguyên, nhưng số khả năng sẽ tăng theo cấp bậc ba với`n`. Cho rằng`n`có thể là 10^8, điều này rõ ràng là không khả thi. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải thử tất cả các bản phân phối. Cho phép`max_coins`là số xu lớn nhất mà bất kỳ chị em nào hiện có. Để cân bằng họ, ít nhất chúng ta phải đưa hai chị em kia lên ngang bằng.`max_coins`. Gọi tổng số xu cần thiết để đạt được mức đó là`(max_coins - a) + (max_coins - b) + (max_coins - c)`. Nếu con số này vượt quá`n`, điều đó là không thể. 

Nếu ta có đủ xu thì số xu còn lại sau khi cân bằng là`max_coins`phải chia hết cho 3. Điều này là do bất kỳ đồng tiền thừa nào vượt quá đường cơ sở phải được phân bổ đều để duy trì sự bình đẳng giữa cả ba chị em. Kiểm tra hai bước này - đầu tiên là đảm bảo chúng tôi có đủ tiền để tiếp cận`max_coins`, sau đó kiểm tra khả năng chia hết cho 3 - chuyển bài toán thành một phép tính đơn giản theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, hãy xác định số xu tối đa mà chị em nào hiện có:`max_coins = max(a, b, c)`. Đây là mục tiêu tối thiểu mà chúng ta phải đạt được đối với tất cả chị em. 
2. Tính tổng số xu cần thiết để đưa mỗi chị em lên`max_coins`:`needed = (max_coins - a) + (max_coins - b) + (max_coins - c)`. Điều này giải thích cho đường cơ sở cân bằng cả ba. 
3. Kiểm tra xem Polycarp có mang lại xu không (`n`) là đủ: nếu`needed > n`, xuất ra "KHÔNG". Việc phân phối là không thể. 
4. Nếu đủ số xu thì tính số xu còn lại:`extra = n - needed`. Những thứ này phải chia hết cho 3 để chia đều cho ba chị em. Nếu như`extra % 3 == 0`, xuất ra "CÓ"; nếu không thì xuất ra "NO". 

**Tại sao nó hoạt động:** Điều bất biến là sau bước 2, tất cả các chị em đã được đưa đến ít nhất`max_coins`. Bất kỳ đồng xu còn sót lại nào cũng phải chia hết cho ba để duy trì sự bình đẳng. Không có ràng buộc nào khác, vì vậy việc kiểm tra này đảm bảo tính chính xác. Không cần phải lặp lại hoặc phân phối phức tạp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, b, c, n = map(int, input().split())
    max_coins = max(a, b, c)
    needed = (max_coins - a) + (max_coins - b) + (max_coins - c)
    if n < needed:
        print("NO")
    else:
        extra = n - needed
        print("YES" if extra % 3 == 0 else "NO")
```Mã đọc nhiều trường hợp thử nghiệm một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`. các`max`function quickly finds the highest starting coin count. We compute the coins required to balance everyone and check for feasibility and divisibility in two simple conditions. Using integer arithmetic ensures no rounding errors, and there are no off-by-one issues because all calculations are exact.

 ## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**`5 3 2 8`| một | b | c | n | max_coin | cần thiết | thêm | Quyết định | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 5 | 3 | 2 | 8 | 5 | 5 | 3 | CÓ | 

Giải thích: Đưa Barbara từ 3→5 và Cerene từ 2→5.`needed = 5`. Còn lại`n - needed = 3`có thể chia đều để cho mỗi chị em thêm một đồng xu, đạt được sự bình đẳng. 

**Đầu vào mẫu 2:**`3 2 1 100000000`| một | b | c | n | max_coin | cần thiết | thêm | Quyết định | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 3 | 2 | 1 | 100000000 | 3 | 3 | 99999997 | KHÔNG | 

Giải thích: Chúng tôi đưa mọi người đến 3.`needed = 3`.`extra = 99999997`không chia hết cho 3 nên không thể chia đều cho 3 chị em. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ một vài phép tính số học cho mỗi trường hợp, không lặp lại`n`| 
| Không gian | O(1) | Chỉ sử dụng một số biến số nguyên | 

Được cho`t ≤ 10^4`, thuật toán này dễ dàng chạy trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        a, b, c, n = map(int, input().split())
        max_coins = max(a, b, c)
        needed = (max_coins - a) + (max_coins - b) + (max_coins - c)
        if n < needed:
            print("NO")
        else:
            extra = n - needed
            print("YES" if extra % 3 == 0 else "NO")
    return output.getvalue().strip()

# Provided samples
assert run("5\n5 3 2 8\n100 101 102 105\n3 2 1 100000000\n10 20 15 14\n101 101 101 3\n") == \
"YES\nYES\nNO\nNO\nYES", "sample 1"

# Custom cases
assert run("1\n1 1 1 0\n") == "YES", "all equal, no extra coins"
assert run("1\n1 2 3 3\n") == "YES", "small numbers, divisible extra"
assert run("1\n1 2 3 2\n") == "NO", "small numbers, insufficient extra"
assert run("1\n100000000 100000000 100000000 3\n") == "YES", "max input, small extra divisible"
assert run("1\n1 1 1 2\n") == "NO", "all equal, extra not divisible"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 0 | CÓ | Không có thêm xu, đã bằng nhau | 
| 1 2 3 3 | CÓ | Đủ tiền để cân bằng và chia hết | 
| 1 2 3 2 | KHÔNG | Không đủ tiền để cân bằng | 
| 100000000 100000000 100000000 3 | CÓ | Xử lý các giá trị đầu vào tối đa | 
| 1 1 1 2 | KHÔNG | Đồng xu bổ sung không chia hết cho 3 | 

## Vỏ cạnh 

Khi tất cả chị em đều đã có số tiền như nhau, chẳng hạn như`a = b = c = 1`Và`n = 0`,`needed = 0`Và`extra = 0`. Việc kiểm tra tính chia hết đã thành công nên kết quả đầu ra là "CÓ". Nếu như`n = 2`thay vì,`extra = 2`không chia hết cho 3 nên đầu ra là "NO". Điều này chứng tỏ thuật toán xử lý chính xác các trường hợp không cần phân phối hoặc số tiền còn sót lại không chia được. 

Khi các đồng tiền cực kỳ mất cân bằng, chẳng hạn như`a = 1, b = 2, c = 100000000`và `n
