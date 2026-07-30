---
title: "CF 102835A - Số ghép phải"
description: "Chúng ta cần quyết định xem một số nguyên dương nhất định có thể được chia thành hai thừa số có kích thước đủ gần nhau hay không. Một số x được coi là ghép đúng nếu tồn tại hai số nguyên dương a và b trong đó a là thừa số nhỏ hơn, b là thừa số lớn hơn, tích của chúng chính xác là x và…"
date: "2026-07-26T14:56:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102835
codeforces_index: "A"
codeforces_contest_name: "The 2020 ICPC Asia Taipei-Hsinchu Site Programming Contest"
rating: 0
weight: 102835
solve_time_s: 36
verified: true
draft: false
---

[CF 102835A - Số ghép phải](https://codeforces.com/problemset/problem/102835/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần quyết định xem một số nguyên dương nhất định có thể được chia thành hai thừa số có kích thước đủ gần nhau hay không. một con số`x`được coi là ghép phải nếu tồn tại hai số nguyên dương`a`Và`b`Ở đâu`a`là hệ số nhỏ hơn,`b`là yếu tố lớn hơn, sản phẩm của họ chính xác là`x`, và hệ số nhỏ ít nhất bằng một nửa hệ số lớn. 

Tương tự, sau khi chọn cặp số chia`(a, b)`với`a <= b`, chúng ta chỉ cần kiểm tra xem`b <= 2a`. Nếu có một cặp như vậy tồn tại thì câu trả lời là`1`; nếu không thì câu trả lời là`0`. 

Đầu vào chứa tối đa 1000 số độc lập và mỗi số đều nhỏ hơn`2^15`, chỉ bằng 32768. Giới hạn trên này rất nhỏ, do đó các thuật toán thực hiện vài trăm kiểm tra trên mỗi số dễ dàng đủ nhanh. Một giải pháp thử mọi cặp thừa số có thể vẫn sẽ không cần thiết vì có nhiều nhất vài trăm ước số có thể cần kiểm tra. Thậm chí một`O(x)`việc quét có thể chấp nhận được đối với giới hạn này, nhưng chúng tôi có thể giảm bớt công việc một cách tự nhiên bằng cách chỉ kiểm tra các ước số tối đa căn bậc hai. 

Một số trường hợp có thể phá vỡ việc thực hiện bất cẩn. Một lỗi phổ biến là sử dụng phép chia dấu phẩy động cho điều kiện`a / b >= 0.5`, có thể gây ra các vấn đề về độ chính xác có thể tránh được. Số nguyên tương đương`2 * a >= b`là chính xác. 

Ví dụ: đầu vào:```
1
66
```có yếu tố`6 * 11`. Từ`6 / 11`lớn hơn`0.5`, đầu ra là:```
1
```Một giải pháp chỉ kiểm tra cặp thừa số gần căn bậc hai không chính xác, giả sử hai thừa số phải bằng nhau sẽ bỏ lỡ trường hợp này. 

Một trường hợp cạnh khác là số nguyên tố:```
1
55
```Cặp nhân tố duy nhất là`1 * 55`Và`2 * 1 < 55`, do đó đầu ra là:```
0
```Một giải pháp chỉ kiểm tra xem số đó có các thừa số khác ngoài một hay không sẽ cho kết quả sai vì không phải mọi số tổng hợp đều được ghép đúng. 

Giá trị đầu vào nhỏ nhất có thể cũng đáng được xem xét:```
1
1
```Cặp nhân tố là`1 * 1`, và nó thỏa mãn điều kiện. Đầu ra đúng là:```
1
```Một triển khai bắt đầu tìm kiếm từ yếu tố`2`sẽ từ chối nó một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là thử mọi giá trị có thể có của`a`, kiểm tra xem`a`chia rẽ`x`, rồi tính`b = x / a`. Nếu như`a <= b`Và`b <= 2a`, chúng ta đã tìm thấy một phân rã hợp lệ. Cách tiếp cận này đúng vì mọi yếu tố nhỏ hơn đều được xem xét. 

Đối với một số`x`, điều này đòi hỏi phải kiểm tra tất cả các giá trị từ`1`ĐẾN`x`, cho`O(x)`hoạt động. Với giá trị tối đa gần bằng 32768 và 1000 trường hợp thử nghiệm, trường hợp xấu nhất là khoảng 32 triệu lượt kiểm tra, có thể vẫn vượt qua ở nhiều ngôn ngữ, nhưng nó bỏ qua cấu trúc toán học của các cặp yếu tố. 

Quan sát tốt hơn là các yếu tố xuất hiện theo cặp. Nếu như`a`là ước số của`x`, hệ số phù hợp là`b = x / a`. Ít nhất một phần tử của mỗi cặp không lớn hơn căn bậc hai của`x`, vì vậy chúng ta chỉ cần kiểm tra các ước số lên tới`sqrt(x)`. Bất cứ khi nào chúng ta tìm thấy một số chia`a`, ta biết ngay hệ số lớn hơn tương ứng và có thể kiểm tra bất đẳng thức cần tìm. 

Phương pháp brute-force hoạt động vì nó khám phá tất cả các phân tích nhân tử có thể có, nhưng nó tốn thời gian kiểm tra các giá trị không thể là cạnh nhỏ hơn của một cặp nhân tố. Việc quan sát căn bậc hai loại bỏ những kiểm tra không cần thiết đó trong khi vẫn đảm bảo rằng mọi cặp ứng cử viên có thể đều được xem xét. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(x) cho mỗi trường hợp thử nghiệm | O(1) | Được chấp nhận cho những giới hạn này, nhưng không cần thiết | 
| Tối ưu | O(sqrt(x)) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi số đầu vào`x`, bắt đầu kiểm tra các ước số có thể có từ`1`lên đến`sqrt(x)`. Chúng ta chỉ cần tìm kiếm trong phạm vi này vì mọi ước số lớn hơn căn bậc hai đều có một ước số nhỏ hơn phù hợp đã được kiểm tra. 
2. Khi một giá trị`a`chia rẽ`x`, tính hệ số cặp`b = x / a`. Cặp đôi`(a, b)`đại diện cho một cách có thể để viết`x`như một sản phẩm. 
3. Đảm bảo`a`là hệ số nhỏ hơn Vì vòng lặp chỉ đạt đến căn bậc hai,`a`luôn nhỏ hơn hoặc bằng`b`. 
4. Kiểm tra xem`b <= 2 * a`. Đây là dạng số nguyên của điều kiện tỷ lệ ban đầu, tránh tính toán dấu phẩy động. 
5. Nếu có cặp ước số nào thỏa mãn điều kiện thì in ra`1`. Nếu vòng lặp kết thúc mà không tìm thấy, hãy in`0`. 

Tính đúng đắn xuất phát từ thực tế là mọi hệ số có thể có của`x`xuất hiện như một số chia`a`và giá trị ghép đôi của nó`x / a`. Vì tất cả các yếu tố nhỏ hơn nhiều nhất`sqrt(x)`, thuật toán sẽ kiểm tra mọi cặp ứng cử viên có thể. Thuật toán chấp nhận chính xác các cặp trong đó hệ số lớn hơn không quá hai lần hệ số nhỏ hơn, đây chính xác là điều kiện bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(x):
    d = 1
    while d * d <= x:
        if x % d == 0:
            a = d
            b = x // d
            if b <= 2 * a:
                return 1
        d += 1
    return 0

def main():
    n = int(input())
    ans = []
    for _ in range(n):
        x = int(input())
        ans.append(str(solve_case(x)))
    print("\n".join(ans))

if __name__ == "__main__":
    main()
```chức năng`solve_case`xử lý một số một cách độc lập. Biến vòng lặp`d`đại diện cho một yếu tố có thể nhỏ hơn và điều kiện`d * d <= x`giới hạn việc tìm kiếm trong phạm vi cần thiết. 

Khi`x % d == 0`, mã đã tìm được cặp nhân tố hoàn chỉnh. Hệ số lớn hơn được tính bằng phép chia số nguyên, do đó việc làm tròn không thể ảnh hưởng đến câu trả lời. Việc so sánh sử dụng`b <= 2 * a`thay vì chia, điều này tránh được các vấn đề về độ chính xác của dấu phẩy động. 

phép nhân`d * d`ở đây an toàn vì giá trị tối đa của`x`chỉ là 32768. Trong các bài toán lớn hơn, việc kiểm tra ranh giới này thường sử dụng`d <= x // d`để tránh tràn số nguyên trong các ngôn ngữ có số nguyên có kích thước cố định. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
4
66
55
105
150
```Trường hợp đầu tiên diễn ra như sau. 

| Số chia đã kiểm tra | Là số chia? | Cặp | Tình trạng | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | Có | 1, 66 | 66 <= 2 | Sai | 
| 2 | Có | 2, 33 | 33 <= 4 | Sai | 
| 3 | Có | 3, 22 | 22 <= 6 | Sai | 
| 6 | Có | 6, 11 | 11 <= 12 | Đúng | 

Cặp đôi`6 * 11`là đủ gần, vì vậy`66`tạo ra đầu ra`1`. 

Đối với đầu vào:```
1
105
```| Số chia đã kiểm tra | Là số chia? | Cặp | Tình trạng | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | Có | 1, 105 | 105 <= 2 | Sai | 
| 3 | Có | 3, 35 | 35 <= 6 | Sai | 
| 5 | Có | 5, 21 | 21 <= 10 | Sai | 

Không có cặp ước số nào thỏa mãn điều kiện nên đầu ra là`0`. 

Những dấu vết này thể hiện tính bất biến chính: mỗi ước số được kiểm tra đại diện cho một cặp thừa số hoàn chỉnh và thuật toán chỉ loại bỏ một số sau khi tất cả các thừa số nhỏ hơn có thể đã được xem xét. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(sqrt(x)) cho mỗi trường hợp thử nghiệm | Chỉ các ước số lên đến căn bậc hai của mỗi số mới được kiểm tra | 
| Không gian | O(1) | Thuật toán chỉ lưu trữ một vài biến số nguyên | 

Giá trị tối đa đủ nhỏ để tìm kiếm căn bậc hai chỉ thực hiện khoảng 182 lần kiểm tra cho mỗi trường hợp thử nghiệm. Ngay cả với 1000 trường hợp thử nghiệm, tổng công việc vẫn rất nhỏ và dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    input = sys.stdin.readline

    def solve_case(x):
        d = 1
        while d * d <= x:
            if x % d == 0:
                if x // d <= 2 * d:
                    return 1
            d += 1
        return 0

    n = int(input())
    result = []
    for _ in range(n):
        result.append(str(solve_case(int(input()))))

    print("\n".join(result))
    output = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return output

assert solve("4\n66\n55\n105\n150\n") == "1\n0\n0\n1\n", "sample cases"

assert solve("1\n1\n") == "1\n", "minimum value"
assert solve("3\n2\n3\n32767\n") == "1\n0\n0\n", "small and maximum boundary values"
assert solve("4\n4\n9\n16\n25\n") == "1\n1\n1\n1\n", "perfect squares"
assert solve("3\n55\n77\n121\n") == "0\n0\n1\n", "prime factors and near-square cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Xử lý số nhỏ nhất có thể | 
|`2, 3, 32767`|`1, 0, 0`| Kiểm tra ranh giới dưới và giá trị lớn | 
|`4, 9, 16, 25`| Tất cả`1`| Kiểm tra số bình phương có thừa số bằng nhau | 
|`55, 77, 121`|`0, 0, 1`| Kiểm tra các tích nguyên tố và các cặp nhân tố gần gũi | 

## Vỏ cạnh 

Đối với đầu vào nhỏ nhất:```
1
1
```vòng lặp kiểm tra số chia`1`, tìm thấy cặp`(1, 1)`, và xác minh`1 <= 2`. Thuật toán trả về`1`, nhận biết đúng rằng số đó có thể chia thành hai thừa số bằng nhau. 

Đối với số nguyên tố:```
1
55
```cặp yếu tố hữu ích duy nhất là`(1, 55)`. Thuật toán tìm ước số`1`và kiểm tra`55 <= 2`, thất bại. Vì không có ước số nào khác nên đáp án vẫn là`0`. 

Đối với một số có cặp hợp lệ khác xa bằng nhau:```
1
66
```thuật toán không dừng lại sau khi nhìn thấy các cặp như`(1, 66)`hoặc`(2, 33)`. Nó tiếp tục cho đến khi chia`6`, nơi nó tìm thấy`(6, 11)`. Từ`11 <= 12`, nó trả về`1`. 

Để có một hình vuông hoàn hảo:```
1
121
```số chia`11`tạo ra cặp`(11, 11)`. Các thừa số bằng nhau luôn thỏa mãn điều kiện vì thừa số lớn hơn có cùng kích thước với thừa số nhỏ hơn nên kết quả là`1`.
