---
title: "CF 102868G - Trắng"
description: "Nhiệm vụ yêu cầu chúng tôi trả lời nhiều truy vấn trong phạm vi. Đối với mỗi khoảng đầu vào có thể, chúng ta cần tìm số x bên trong khoảng đó có mã id được tạo lớn nhất. Mã được tạo ra là x phi(x), trong đó phi(x) thu được bằng cách thay thế mọi chữ số của x bằng phần bù của nó thành 9."
date: "2026-07-25T13:31:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102868
codeforces_index: "G"
codeforces_contest_name: "2020 UTPC Fall Puzzle Contest"
rating: 0
weight: 102868
solve_time_s: 57
verified: true
draft: false
---

[CF 102868G - Trắng](https://codeforces.com/problemset/problem/102868/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Nhiệm vụ yêu cầu chúng tôi trả lời nhiều truy vấn trong phạm vi. Đối với mỗi khoảng đầu vào có thể, chúng ta cần tìm số`x`bên trong khoảng đó có mã id được tạo lớn nhất. Mã được tạo ra là`x * phi(x)`, Ở đâu`phi(x)`thu được bằng cách thay thế mọi chữ số của`x`với phần bù của nó là 9. Các số 0 đứng đầu được tạo bởi thao tác này sẽ bị bỏ qua, nhưng điều đó không làm thay đổi giá trị số. 

Việc đơn giản hóa khóa được ẩn bên trong thao tác chữ số. Nếu như`x`có chính xác`k`chữ số, thay thế từng chữ số`d`qua`9-d`tạo ra giá trị tương tự như phép trừ`x`từ`k`số có chữ số chỉ gồm số chín. Ví dụ: đối với số có ba chữ số,`phi(x) = 999 - x`. Việc loại bỏ các số 0 đứng đầu không ảnh hưởng đến đẳng thức này vì các số 0 đứng đầu không đóng góp vào giá trị. 

Đầu vào chứa tối đa 1000 khoảng và mọi điểm cuối đều có nhiều nhất`10^9`. Một giải pháp kiểm tra mọi số trong mỗi khoảng thời gian có thể cần tới khoảng`10^12`những đánh giá vượt xa những gì có thể. Chúng ta cần sử dụng dạng toán học của hàm thay vì lặp qua phạm vi. 

Hàm cho độ dài chữ số cố định là:`f(x) = x * (10^k - 1 - x)`Việc mở rộng nó sẽ tạo ra một parabol mở hướng xuống dưới. Cực đại của nó là ở giữa khoảng thời gian từ`0`ĐẾN`10^k - 1`, vì vậy chúng ta chỉ cần kiểm tra các vị trí gần điểm giữa đó và các ranh giới của khoảng truy vấn. 

Các trường hợp phức tạp là do sự thay đổi độ dài chữ số. Một con số như`99`thuộc nhóm có hai chữ số, trong đó`phi(99)=0`, Nhưng`100`thuộc nhóm có ba chữ số, trong đó`phi(100)=899`. Một giải pháp giả định toàn bộ truy vấn sử dụng một giá trị là`k`có thể thất bại. 

Ví dụ:```
Input
1
99 100

Output
89900
```Câu trả lời đến từ`x=100`, bởi vì`100 * 899 = 89900`. Chỉ nhìn vào số có hai chữ số sẽ trả về số 0 không chính xác. 

Một trường hợp cạnh khác là khi khoảng chỉ chứa một số.```
Input
1
5 5

Output
20
```Việc thực hiện bất cẩn chỉ kiểm tra phần giữa của phạm vi có thể bỏ lỡ câu trả lời vì không còn chỗ để di chuyển về điểm giữa. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ cố gắng hết sức có thể`x`TRONG`[l,r]`, tính toán`phi(x)`từng chữ số và giữ lại tích lớn nhất. Điều này đúng vì mọi ứng viên hợp lệ đều được kiểm tra. Tuy nhiên, một khoảng có thể chứa gần một tỷ giá trị và có thể có 1000 truy vấn. Trường hợp xấu nhất sẽ cần khoảng`10^12`đánh giá sản phẩm, điều đó là không thể. 

Quan sát hữu ích là các số có cùng số chữ số có cùng cơ số bù. Nếu như`x`có`k`thì các chữ số`phi(x)=10^k-1-x`, do đó tích trở thành:`x * (10^k-1-x)`Đây là một parabol. Một parabol mở xuống sẽ đạt cực đại tại đỉnh. Đối với khoảng truy vấn, giá trị tốt nhất bên trong đoạn có độ dài chữ số phải là đỉnh nếu nó nằm bên trong đoạn đó hoặc một trong các điểm cuối của đoạn. 

Từ`x`nhiều nhất là`10^9`, chỉ có mười một nhóm độ dài chữ số có thể xem xét, từ một chữ số đến mười chữ số. Đối với mỗi nhóm, chúng tôi giao nó với phạm vi truy vấn và kiểm tra một số ứng cử viên có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(r-l+1) mỗi truy vấn | O(1) | Quá chậm | 
| Tối ưu | O(10) mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi khoảng truy vấn`[l,r]`, xem xét mọi độ dài chữ số có thể`k`từ 1 đến 10. Khoảng các số có chính xác`k`chữ số là`[10^(k-1), 10^k-1]`, với giới hạn dưới được coi là 1 cho`k=1`. 
2. Giao khoảng độ dài chữ số này với`[l,r]`. Nếu giao điểm trống thì không có ứng cử viên nào có độ dài chữ số này. 
3. Tính toán`m = (10^k - 1) / 2`, đó là vị trí mà`x * (10^k - 1 - x)`được tối đa hóa. Ứng cử viên tốt nhất trong phân khúc hiện tại nằm trong ranh giới của phân khúc và hai số nguyên gần nhất với`m`. 
4. Đánh giá sản phẩm cho mọi ứng viên còn lại trong phân khúc và cập nhật mức tối đa toàn cầu. 

Tại sao nó hoạt động: Với mỗi độ dài chữ số cố định, hàm tích là một hàm bậc hai lõm. Một phương trình bậc hai lõm không thể có điểm bên trong tốt hơn đỉnh của nó và nếu đỉnh nằm ngoài khoảng cho phép thì điểm tốt nhất phải là điểm cuối gần nhất. Chúng tôi kiểm tra chính xác những khả năng đó cho từng độ dài chữ số, vì vậy mọi số tối ưu có thể đều được đề cập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_query(l, r):
    ans = 0
    pow10 = 1

    for k in range(1, 11):
        low = 1 if k == 1 else pow10
        high = pow10 * 10 - 1

        left = max(l, low)
        right = min(r, high)

        if left <= right:
            limit = high
            middle_floor = limit // 2
            middle_ceil = middle_floor + (limit % 2)

            candidates = [
                left,
                right,
                middle_floor,
                middle_ceil
            ]

            for x in candidates:
                if left <= x <= right:
                    ans = max(ans, x * (limit - x))

        pow10 *= 10

    return ans

def main():
    t = int(input())
    out = []
    for _ in range(t):
        l, r = map(int, input().split())
        out.append(str(solve_query(l, r)))
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Vòng lặp kết thúc`k`xử lý mọi chiều dài chữ số có thể. Biến`pow10`cửa hàng`10^(k-1)`trước khi nó được cập nhật cho lần lặp tiếp theo, do đó phạm vi chữ số hiện tại có thể được xây dựng mà không cần tính lũy thừa lặp lại. 

Với mỗi độ dài chữ số,`limit`là`10^k-1`, số chín được sử dụng trong công thức cho`phi(x)`. Danh sách ứng cử viên chứa hai đầu của khoảng hợp lệ và hai giá trị ở giữa. Việc kiểm tra cả hai giá trị ở giữa sẽ tránh được các vấn đề do phép chia số nguyên gây ra khi đỉnh nằm giữa hai số nguyên. 

Phép nhân được thực hiện bằng cách sử dụng các số nguyên Python, vì vậy tích lớn nhất có thể có, xấp xỉ`9 * 10^18`, được xử lý an toàn. Thứ tự của các thao tác cũng tránh được việc làm tròn dấu phẩy động. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Khoảng đầu vào:`[1,7]`| k | Phạm vi hợp lệ | Ứng viên đã được kiểm tra | Sản phẩm tốt nhất | 
| --- | --- | --- | --- | 
| 1 | [1,7] | 1, 7, 4, 5 | 20 | 

Giữa phạm vi một chữ số là khoảng 4,5, vì vậy việc kiểm tra 4 và 5 sẽ tìm thấy mức tối ưu. Câu trả lời là`4 * 5 = 20`. 

### Mẫu 2 

Khoảng đầu vào:`[8,14]`| k | Phạm vi hợp lệ | Ứng viên đã được kiểm tra | Sản phẩm tốt nhất | 
| --- | --- | --- | --- | 
| 1 | [8,9] | 8, 9, 4, 5 | 8 | 
| 2 | [10,14] | 10, 14, 49, 50 | 1190 | 

Phần một chữ số không thể đánh bại phần hai chữ số. Trong phân đoạn có hai chữ số, giá trị khả dụng gần nhất với 49,5 không nằm trong truy vấn, vì vậy điểm tốt nhất là ranh giới trên`14`, cho`14 * 85 = 1190`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(10t) | Mỗi truy vấn kiểm tra mười độ dài chữ số có thể có và số lượng ứng cử viên không đổi. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ. | 

Số lượng thao tác tối đa là khoảng vài chục nghìn vì chỉ có 1000 truy vấn. Điều này dễ dàng phù hợp với các giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_query(l, r):
    ans = 0
    pow10 = 1

    for k in range(1, 11):
        low = 1 if k == 1 else pow10
        high = pow10 * 10 - 1
        left = max(l, low)
        right = min(r, high)

        if left <= right:
            middle_floor = high // 2
            middle_ceil = middle_floor + (high % 2)

            for x in (left, right, middle_floor, middle_ceil):
                if left <= x <= right:
                    ans = max(ans, x * (high - x))

        pow10 *= 10

    return ans

def run(inp: str) -> str:
    data = inp.strip().split()
    if not data:
        return ""
    t = int(data[0])
    idx = 1
    res = []
    for _ in range(t):
        l = int(data[idx])
        r = int(data[idx + 1])
        idx += 2
        res.append(str(solve_query(l, r)))
    return "\n".join(res)

assert run("""3
1 7
7 10
8 14
""") == """20
890
1190""", "samples"

assert run("""1
1 1
""") == "8", "minimum input"

assert run("""1
5 5
""") == "20", "single value interval"

assert run("""1
99 100
""") == "89900", "digit boundary"

assert run("""1
1000000000 1000000000
""") == "8999999999000000000", "maximum value"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[1,7]`|`20`| Điểm giữa bên trong một khoảng nhỏ | 
|`[1,1]`|`8`| Giá trị nhỏ nhất có thể | 
|`[5,5]`|`20`| Truy vấn một phần tử | 
|`[99,100]`|`89900`| Vượt qua ranh giới có độ dài chữ số | 
|`[1000000000,1000000000]`|`8999999999000000000`| Đầu vào được phép lớn nhất và phép nhân lớn | 

## Vỏ cạnh 

Đối với khoảng thời gian`[99,100]`, thuật toán kiểm tra cả hai nhóm chữ số. Nhóm hai chữ số sử dụng`99 - x`, chỉ cho số 0 tại`x=99`. Nhóm ba chữ số sử dụng`999 - x`, Và`x=100`cho`899`, sản xuất`89900`. Vì cả hai nhóm được xem xét độc lập nên không thể bỏ qua việc chuyển đổi chữ số. 

Đối với khoảng thời gian`[5,5]`, giao điểm của nhóm một chữ số là một điểm duy nhất. Danh sách ứng cử viên chứa cả hai ranh giới, vì vậy`x=5`được kiểm tra trực tiếp. Giá trị tính toán là`5 * (9-5) = 20`. 

Đối với giá trị đầu vào lớn nhất`1000000000`, thuật toán đặt nó vào nhóm mười chữ số. Số chẵn chín là`9999999999`, Vì thế`phi(1000000000)=8999999999`. Sản phẩm phù hợp với kiểu số nguyên của Python và được đánh giá không bị tràn.
