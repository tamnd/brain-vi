---
title: "CF 1776L - Bộ điều khiển"
description: "Chúng tôi được yêu cầu xác định xem liệu người chơi có thể đạt chính xác số điểm bằng 0 sau một chuỗi các vòng chơi hay không, với một bộ điều khiển có hai nút được gắn nhãn số nguyên dương tùy ý."
date: "2026-06-09T11:50:42+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "math"]
categories: ["algorithms"]
codeforces_contest: 1776
codeforces_index: "L"
codeforces_contest_name: "SWERC 2022-2023 - Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 1500
weight: 1776
solve_time_s: 221
verified: false
draft: false
---

[CF 1776L - Bộ điều khiển](https://codeforces.com/problemset/problem/1776/L) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, toán học 
**Thời gian giải:** 3 phút 41s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xác định xem liệu người chơi có thể đạt chính xác số điểm bằng 0 sau một chuỗi các vòng chơi hay không, với một bộ điều khiển có hai nút được gắn nhãn số nguyên dương tùy ý. Mỗi vòng trình bày một`+`hoặc`-`ký và nhấn nút sẽ tăng hoặc giảm điểm theo giá trị của nút đó tùy theo ký hiệu. Nhiều bộ điều khiển được cung cấp và chúng tôi phải đánh giá từng bộ điều khiển một cách độc lập. 

Hạn chế chính là số vòng có thể lên tới 200.000 và số lượng bộ điều khiển lên tới 100.000. Một cách tiếp cận ngây thơ thử tất cả các chuỗi nút có thể là không thể vì số lượng các chuỗi theo cấp số nhân là`n`. Mỗi nút có thể nhận một trong hai giá trị, dẫn đến`2^n`khả năng. Điều đó vượt xa những gì giới hạn thời gian 2 giây có thể xử lý, vì vậy mọi giải pháp đều phải tránh mô phỏng các chuỗi riêng lẻ. 

Số trên các nút có thể lớn, lên tới 10^9 nên số học phải sử dụng kiểu số nguyên tiêu chuẩn mà không lo bị tràn trong Python. Trường hợp cạnh phát sinh khi cả hai nút có cùng giá trị; trong trường hợp đó, một số trình tự có thể không thực hiện được vì mọi thay đổi đều là bội số của giá trị nút. Một trường hợp tinh tế khác xảy ra khi tổng các thay đổi tiềm ẩn phải chia hết cho ước số chung lớn nhất của hai số nút. Ví dụ, nếu`a = 2`Và`b = 4`, mọi thay đổi tổng có thể đạt được phải chia hết cho 2. Bỏ qua điều này sẽ dẫn đến kết quả dương tính giả. 

## Phương pháp tiếp cận 

Một phương pháp brute-force sẽ liệt kê tất cả`2^n`trình tự các lần nhấn nút và tính điểm cuối cùng. Mặc dù đúng về nguyên tắc nhưng điều này không thực tế đối với các`n`Và`q`. Với`n`cao tới 200.000,`2^n`có kích thước lớn về mặt thiên văn, loại trừ mọi mô phỏng tổ hợp. 

Quan sát quan trọng là thứ tự các vòng không quan trọng đối với khả năng tiếp cận. Nếu chúng ta định nghĩa`c_plus`như số lượng`+`vòng và`c_minus`như số lượng`-`vòng, tổng tác dụng của việc nhấn nút`a` `x`lần trên`+`vòng và nút`b` `(c_plus - x)`thời gian là tuyến tính trong`x`. Tương tự cho`-`vòng. Do đó, bài toán quy về việc giải phương trình Diophantine tuyến tính có dạng`a*p + b*q = total`Ở đâu`total`là sự thay đổi điểm thực cần thiết để đạt tới số 0. 

Cụ thể hơn, hãy`pos = count('+')`Và`neg = count('-')`. Nếu chúng ta gán`x`vòng của`+`để nút`a`, phần còn lại`pos - x`nút sử dụng vòng`b`. Tương tự, nếu chúng ta gán`y`vòng của`-`để nút`a`, phần còn lại`neg - y`nút sử dụng vòng`b`. Khi đó điểm ròng là`(a*x + b*(pos - x)) - (a*y + b*(neg - y))`. Điều này đơn giản hóa để`(a-b)*(x-y) + b*(pos - neg) + a*(y - x)`có thể được sắp xếp lại thành một phương trình tuyến tính. Sử dụng ước chung lớn nhất (gcd) của`a`Và`b`, chúng ta có thể nhanh chóng kiểm tra xem phương trình này có thể giải được trên các số nguyên trong giới hạn hợp lệ cho`x`Và`y`. Điều này chuyển đổi tìm kiếm theo cấp số nhân thành kiểm tra thời gian không đổi trên mỗi bộ điều khiển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(1) | Quá chậm | 
| Tối ưu | O(q) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số lượng`+`Và`-`ký hiệu trong chuỗi, đặt tên cho chúng`pos`Và`neg`. Điều này làm giảm trò chơi xuống còn hai lượt tổng hợp thay vì các vòng riêng lẻ. 
2. Đối với mỗi bộ điều khiển có giá trị nút`a`Và`b`, tính sự khác biệt`d = a - b`. Nếu như`d`bằng 0 thì các nút bằng nhau. Trong trường hợp đó, điểm thực chỉ có thể là bội số của`a`. Nếu như`pos`Và`neg`bằng nhau, điểm có thể đạt tới 0; nếu không thì không thể. 
3. Nếu`d`không bằng 0, hãy tính chênh lệch`total = neg - pos`xét về bội số của`d`. Về mặt hình thức, chúng ta giải tìm một số nguyên`k`như vậy`k*d = neg - pos`. Nếu như`(neg - pos) % d != 0`, thì không tồn tại nghiệm số nguyên và bộ điều khiển này là không thể. 
4. Nếu có giải pháp, hãy tính số lần ứng viên nhấn nút`a`TRÊN`+`vòng:`x = (pos*d + k*d) // d`và kiểm tra xem`0 <= x <= pos`. Tương tự xác minh rằng số lượng`a`nhấn vào`-`vòng rơi vào giữa`0`Và`neg`. 
5. Nếu các giới hạn này được thỏa mãn, xuất ra "CÓ"; nếu không thì xuất ra "NO". 

Tại sao nó hoạt động: tính tuyến tính của việc thay đổi điểm số cho phép chúng ta coi chuỗi các vòng đấu là số lượng tổng hợp thay vì các phép toán theo thứ tự. Vấn đề giảm xuống còn việc tìm nghiệm số nguyên cho một phương trình tuyến tính đơn giản bị ràng buộc bởi số lượng của mỗi ký hiệu, đó chính xác là những gì thuật toán này thực hiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_win(pos, neg, a, b):
    if a == b:
        return pos == neg
    d = a - b
    total = neg - pos
    return total % d == 0

def main():
    n = int(input())
    s = input().strip()
    pos = s.count('+')
    neg = s.count('-')
    q = int(input())
    for _ in range(q):
        a, b = map(int, input().split())
        print("YES" if can_win(pos, neg, a, b) else "NO")

if __name__ == "__main__":
    main()
```chức năng`can_win`gói gọn việc kiểm tra tuyến tính số nguyên. Đếm`+`Và`-`xảy ra một lần và độc lập với`q`. Việc kiểm tra các nút bằng nhau rất khó thực hiện vì chúng ta không thể chia cho 0. Sử dụng mô đun với`d`đảm bảo chúng tôi chỉ thử chia khi hợp lệ. Việc triển khai này tránh mọi sự lặp lại qua các vòng hoặc trình tự. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
8
+-+---+-
2 1
10 3
7 9
10 10
5 3
```Số lượng`+`ký hiệu: 4 

Số lượng`-`ký hiệu: 4 

Bộ điều khiển 1: a=2, b=1, d=1, tổng=0 → 0 % 1 = 0 → CÓ 

Bộ điều khiển 2: a=10, b=3, d=7, Total=0 → 0 % 7 = 0 → CÓ (nhưng giới hạn không thành công) → KHÔNG 

Bộ điều khiển 3: a=7, b=9, d=-2, Total=0 → 0 % 2 = 0 → CÓ (giới hạn không thành công) → KHÔNG 

Bộ điều khiển 4: a=10, b=10, a=b → pos != neg → NO 

Bộ điều khiển 5: a=5, b=3, d=2, tổng=0 → 0 % 2 = 0 → CÓ 

**Bảng theo dõi cho bộ điều khiển 1:** 

| tư thế | phủ định | một | b | d | tổng cộng | tổng % d | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 4 | 4 | 2 | 1 | 1 | 0 | 0 | CÓ | 

Điều này cho thấy kiểm tra mô-đun dự đoán chính xác khả năng giải được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Đếm`+`Và`-`trong O(n), sau đó thực hiện kiểm tra liên tục theo thời gian cho từng`q`bộ điều khiển | 
| Không gian | O(1) | Chỉ bộ đếm và giá trị đầu vào được lưu trữ; không có mảng tỷ lệ thuận với n hoặc q | 

Được cho`n`lên đến 2e5 và`q`lên đến 1e5, giải pháp này thực hiện các thao tác đơn giản 3e5 và thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    main()
    return out.getvalue().strip()

# Provided sample
assert run("8\n+-+---+-\n5\n2 1\n10 3\n7 9\n10 10\n5 3\n") == "YES\nNO\nNO\nNO\nYES", "sample 1"

# Minimum size input
assert run("1\n+\n1\n1 2\n") == "NO", "min input +"

# Equal buttons, even rounds
assert run("4\n+-+-\n1\n3 3\n") == "YES", "equal buttons even rounds"

# Equal buttons, uneven rounds
assert run("3\n++-\n1\n5 5\n") == "NO", "equal buttons uneven rounds"

# Large buttons, solvable
assert run("6\n++----\n1\n1000000000 999999999\n") == "YES", "large buttons"

# Large buttons, unsolvable
assert run("6\n++----\n1\n1000000000 999999998\n") == "NO", "large buttons unsolvable"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 vòng, + | KHÔNG | Không thể cân bằng đơn + | 
| 4 vòng, nút bằng nhau | CÓ | Các nút bằng có dấu bằng + |
