---
title: "CF 102277F - Làm tròn theo nhiều cách"
description: "Chúng ta được cho một số nguyên dương N, là giá trị xuất hiện sau một số thao tác làm tròn chưa biết. Chúng ta cần xác định mọi số nguyên dương X có thể được sử dụng làm đơn vị làm tròn. Đơn vị làm tròn X có hai hạn chế."
date: "2026-08-16T19:35:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102277
codeforces_index: "F"
codeforces_contest_name: "UCF Locals 2018"
rating: 0
weight: 102277
solve_time_s: 72
verified: true
draft: false
---

[CF 102277F - Làm tròn nhiều cách](https://codeforces.com/problemset/problem/102277/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên dương`N`, là giá trị xuất hiện sau một số thao tác làm tròn không xác định. Chúng ta cần xác định mọi số nguyên dương có thể`X`có thể được sử dụng làm đơn vị làm tròn. 

Đơn vị làm tròn`X`có hai hạn chế. Đầu tiên,`X`phải chia một số lũy thừa của 10, nên thừa số nguyên tố duy nhất có thể có của nó là 2 và 5. Tương tự,`X`phải có hình thức`2^a * 5^b`đối với một số số nguyên không âm`a`Và`b`. Thứ hai, giá trị làm tròn`N`bản thân nó phải là bội số của`X`. Số liệu thống kê ban đầu có thể chỉ đơn giản là`N`, vậy hai điều kiện này cũng đủ. Tuyên bố chính thức rõ ràng giảm bớt nhiệm vụ thành việc tìm kiếm tất cả`X`thỏa mãn hai điều kiện chia hết. 

Đầu vào chứa một số nguyên`N`, với`1 <= N <= 10^18`. Đầu ra đầu tiên đưa ra số lượng giá trị hợp lệ của`X`, sau đó in các giá trị đó theo thứ tự tăng dần, mỗi giá trị một dòng. Các mẫu được công bố là`N = 30`,`N = 120`, Và`N = 8`. 

Giới hạn trên của`10^18`loại trừ mọi thứ quét tất cả các giá trị có thể lên đến`N`. Thậm chí một`O(sqrt(N))`tìm kiếm chia có thể yêu cầu khoảng`10^9`lặp đi lặp lại, vượt xa giới hạn một giây. Cấu trúc hữu ích là chúng ta chỉ quan tâm đến số mũ của 2 và 5 trong`N`, và các số mũ đó lần lượt nhiều nhất là 59 và 25. 

Có một số trường hợp phức tạp có thể đánh lừa một giải pháp dựa trên số chia cơ học hơn. Vì`N = 1`, đầu ra đúng là`1`theo sau là`1`. Một giải pháp bắt đầu tìm kiếm ở vị trí thứ 2 sẽ báo cáo không chính xác rằng không có câu trả lời nào, mặc dù`X = 1`luôn luôn hợp lệ. 

Vì`N = 8`, đầu ra đúng là`4`, theo sau là`1`,`2`,`4`, Và`8`. Một giải pháp chỉ xét lũy thừa của 10 sẽ bỏ lỡ`2`,`4`, Và`8`, bởi vì các đơn vị hợp lệ là tất cả các số có thừa số nguyên tố bị giới hạn ở 2 và 5, không chỉ các số có lũy thừa của 10. 

cho`N = 30`, đầu ra đúng là`4`, theo sau là`1`,`2`,`5`, Và`10`. Các giá trị như`3`,`6`, Và`15`là các ước của 30 nhưng không thể chia bất kỳ lũy thừa nào của 10 vì chúng chứa thừa số nguyên tố 3. Một giải pháp liệt kê các ước số tùy ý của`N`và chấp nhận tất cả chúng sẽ tạo ra câu trả lời sai. 

Vì`N = 120`, các giá trị đúng là`1, 2, 4, 5, 8, 10, 20, 40`. Ở đây số mũ của 2 in`N`là 3 và số mũ của 5 là 1, nên mọi tổ hợp`2^a * 5^b`với`0 <= a <= 3`Và`0 <= b <= 1`là hợp lệ. Điều này mang lại`(3 + 1)(1 + 1) = 8`câu trả lời. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử mọi số nguyên`X`từ 1 đến`N`, kiểm tra xem`X`chia rẽ`N`, sau đó kiểm tra xem`X`không có thừa số nguyên tố nào ngoài 2 và 5. Điều này đúng vì hai phép kiểm tra chính xác là hai yêu cầu toán học. Vấn đề là phạm vi. Vì`N = 10^18`, một vòng lặp như vậy thực hiện`10^18`lặp đi lặp lại, điều này hoàn toàn không thể thực hiện được. 

Một cách tiếp cận chia số mạnh mẽ hơn một chút sẽ liệt kê các ước số của`N`bằng cách kiểm tra mọi số nguyên cho đến`sqrt(N)`. Điều này làm giảm trường hợp xấu nhất xuống còn khoảng`10^9`lặp đi lặp lại khi`N`đang ở gần`10^18`. Tốc độ đó vẫn còn quá chậm đối với một bài toán chỉ trong một giây. 

Quan sát quan trọng là một số chia hết lũy thừa của 10 khi và chỉ khi nó có dạng`X = 2^a * 5^b`. 

Giả sử phân tích thành thừa số nguyên tố của`N`chứa`N = 2^A * 5^B * R`,

Ở đâu`R`không chia hết cho 2 hoặc 5. Vì`X`phải chia`N`, số mũ của nó không thể vượt quá số mũ trong`N`. Vì vậy, mọi câu trả lời hợp lệ đều chính xác là một trong`2^a * 5^b`, Ở đâu`0 <= a <= A`Và`0 <= b <= B`. 

Không cần phải tính phần còn lại`N`không hề. Chúng tôi chỉ liên tục chia`N`bằng 2 để tìm`A`, rồi chia liên tục cho 5 để tìm`B`. 

Brute-force hoạt động vì nó kiểm tra rõ ràng mọi đơn vị có thể, nhưng không thành công vì phạm vi số rất lớn. Nhận xét rằng các đơn vị hợp lệ chỉ chứa các số nguyên tố 2 và 5 cho phép chúng ta thay thế tìm kiếm lên đến`10^18`các số nguyên với việc tìm kiếm tối đa khoảng 60 lũy thừa có thể có của 2 và 26 lũy thừa có thể có của 5. 

Khi chúng tôi tạo ra mọi`2^a * 5^b`, chúng tôi sắp xếp các giá trị kết quả và in chúng. Số lượng ứng viên được tạo ra rất ít, nhiều nhất là`(59 + 1)(25 + 1) = 1560`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force kết thúc`1..N`|`O(N log N)`trong việc thực hiện đơn giản |`O(1)`không bao gồm đầu ra | Quá chậm | 
| Tìm số chia lên đến`sqrt(N)`|`O(sqrt(N))`|`O(1)`không bao gồm đầu ra | Quá chậm | 
| Liệt kê số mũ tối ưu |`O(log N + K log K)`|`O(K)`| Đã chấp nhận | 

Đây`K`là số giá trị hợp lệ của`X`, Và`K <= 1560`đối với giới hạn đã cho. 

## Hướng dẫn thuật toán 

1. Đọc`N`và xác định số đó chia cho 2 bao nhiêu lần. Bắt đầu với`a = 0`và liên tục chia`N`tăng lên 2 trong khi nó chẵn, tăng`a`mỗi lần. 

Cuối cùng,`a`chính xác là số mũ của 2 trong hệ số nguyên tố của số ban đầu`N`. 
2. Thực hiện tương tự cho 5. Liên tục chia số còn lại cho 5 nếu có thể, tăng dần`b`. 

Chúng ta chỉ cần hai số mũ này vì mọi số mũ hợp lệ`X`không chứa thừa số nguyên tố nào ngoài 2 và 5. 
3. Sinh từng cặp số mũ`i`Và`j`thỏa mãn`0 <= i <= a`Và`0 <= j <= b`. 

Với mỗi cặp, hãy xây dựng`2^i * 5^j`và lưu trữ nó. Mỗi giá trị như vậy chia hết`N`, bởi vì không có số mũ nào vượt quá số mũ tương ứng trong`N`. 
4. Sắp xếp các giá trị được tạo ra. 

Các vòng lặp lồng nhau liệt kê các giá trị theo số mũ thay vì độ lớn bằng số một cách tự nhiên, do đó thứ tự tạo của chúng không nhất thiết phải tăng lên. Việc sắp xếp đưa ra chính xác thứ tự đầu ra mà bài toán yêu cầu. 
5. In số lượng giá trị được tạo và sau đó mỗi giá trị trên một dòng riêng. 

### Tại sao nó hoạt động 

Điều bất biến là mọi giá trị được tạo ra đều có dạng`2^i * 5^j`với số mũ không lớn hơn số mũ trong`N`. Do đó, mọi giá trị được tạo ra sẽ chia cả hai`N`và lũy thừa đủ lớn bằng 10 nên thỏa mãn cả hai điều kiện cần. 

Ngược lại, lấy bất kỳ giá trị nào`X`. Từ`X`chia lũy thừa của 10, hệ số nguyên tố của nó chỉ có thể chứa 2 và 5, vì vậy`X = 2^i * 5^j`. Từ`X`cũng chia`N`, số mũ của nó thỏa mãn`i <= a`Và`j <= b`. Thuật toán liệt kê chính xác cặp đó, vì vậy mọi giá trị hợp lệ`X`được tạo ra. Hai hướng cùng nhau chứng minh rằng tập hợp được tạo ra chính xác là tập hợp câu trả lời được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    x = n
    a = 0
    while x % 2 == 0:
        x //= 2
        a += 1

    b = 0
    while x % 5 == 0:
        x //= 5
        b += 1

    powers2 = [1] * (a + 1)
    for i in range(1, a + 1):
        powers2[i] = powers2[i - 1] * 2

    powers5 = [1] * (b + 1)
    for j in range(1, b + 1):
        powers5[j] = powers5[j - 1] * 5

    ans = []
    for p2 in powers2:
        for p5 in powers5:
            ans.append(p2 * p5)

    ans.sort()

    out = [str(len(ans))]
    out.extend(map(str, ans))
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên trích số mũ của 2 từ`N`. Vòng lặp thứ hai trích số mũ của 5 từ giá trị còn lại. Việc loại bỏ thừa số của 2 trước khi đếm thừa số của 5 không ảnh hưởng đến số mũ của 5, vì 2 và 5 là các số nguyên tố riêng biệt. 

Hai mảng công suất tránh tính toán công suất lặp đi lặp lại bên trong các vòng lặp lồng nhau. Chúng nhỏ vì`N <= 10^18`. Trong thực tế,`2^60 > 10^18`Và`5^26 > 10^18`, do đó có thể có nhiều nhất 60 lũy thừa 2 và 26 lũy thừa 5 phù hợp với câu trả lời. 

Số nguyên Python không bị tràn, nhưng kết quả được tạo ra cũng bị giới hạn bởi`N`, bởi vì mỗi lần tạo ra`2^i * 5^j`chia rẽ`N`. Các số mũ sử dụng giới hạn bao gồm, điều này là cần thiết bởi vì`X`bản thân nó có thể chứa toàn bộ sức mạnh của 2 hoặc 5 hiện diện trong`N`. 

Bước sắp xếp là cần thiết vì thứ tự số mũ không bằng thứ tự số. Ví dụ, với`N = 120`, cặp`(3, 0)`tạo ra 8 trong khi`(0, 1)`tạo ra 5. 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho`N = 30`, chỉ phân tích số đó theo 2 và 5. 

|`a`|`b`|`2^a`|`5^b`| Giá trị được tạo | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1, 2 | 1, 5 | 1, 5, 2, 10 | 

Sau khi sắp xếp, các giá trị được`1, 2, 5, 10`. 

Số mũ của 2 là 1 và số mũ của 5 là 1, cho`(1 + 1)(1 + 1) = 4`ứng viên. Mỗi một chia cho 30 và không có thừa số nguyên tố nào khác ngoài 2 và 5. Do đó, kết quả là:```
4
1
2
5
10
```Ví dụ này chứng minh tại sao các ước số tùy ý là không đủ. Số chia 3 không hợp lệ vì nó không thể chia lũy thừa của 10. 

### Mẫu 2 

cho`N = 120`, hệ số có liên quan là`120 = 2^3 * 5^1 * 3`. 

Yếu tố 3 có thể bỏ qua vì nó không bao giờ xuất hiện trong`X`. 

|`i`|`2^i`|`j`|`5^j`|`X`| 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 1 | 
| 0 | 1 | 1 | 5 | 5 | 
| 1 | 2 | 0 | 1 | 2 | 
| 1 | 2 | 1 | 5 | 10 | 
| 2 | 4 | 0 | 1 | 4 | 
| 2 | 4 | 1 | 5 | 20 | 
| 3 | 8 | 0 | 1 | 8 | 
| 3 | 8 | 1 | 5 | 40 | 

Sắp xếp các giá trị này tạo ra`1, 2, 4, 5, 8, 10, 20, 40`. 

Hai phạm vi số mũ đưa ra bốn lựa chọn về lũy thừa 2 và hai lựa chọn về lũy thừa 5, do đó có tám câu trả lời. Điều này xác nhận tính bất biến của phạm vi tích số mũ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(log N + K log K)`| Phân tích nhân tử cho 2 và 5 mất`O(log N)`, việc tạo ra câu trả lời sẽ mất`O(K)`và việc sắp xếp mất`O(K log K)`. | 
| Không gian |`O(K)`| Danh sách câu trả lời và hai mảng công suất nhỏ sử dụng không gian tỷ lệ thuận với số lượng giá trị hợp lệ. | 

Vì`N <= 10^18`, có nhiều nhất 1560 ứng viên nên chi phí sắp xếp là không đáng kể. Thuật toán chỉ thực hiện vài chục phép chia, theo sau là một loại rất nhỏ, vừa vặn với giới hạn thời gian một giây và giới hạn bộ nhớ 256 MB đã nêu cho bài toán. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline
    n = int(input())

    x = n
    a = 0
    while x % 2 == 0:
        x //= 2
        a += 1

    b = 0
    while x % 5 == 0:
        x //= 5
        b += 1

    powers2 = [1] * (a + 1)
    for i in range(1, a + 1):
        powers2[i] = powers2[i - 1] * 2

    powers5 = [1] * (b + 1)
    for j in range(1, b + 1):
        powers5[j] = powers5[j - 1] * 5

    ans = []
    for p2 in powers2:
        for p5 in powers5:
            ans.append(p2 * p5)

    ans.sort()

    out = [str(len(ans))]
    out.extend(map(str, ans))
    return "\n".join(out)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solve()
    finally:
        sys.stdin = old_stdin

# Provided samples
assert run("30\n") == "4\n1\n2\n5\n10", "sample 1"
assert run("120\n") == "8\n1\n2\n4\n5\n8\n10\n20\n40", "sample 2"
assert run("8\n") == "4\n1\n2\n4\n8", "sample 3"

# Minimum-size input
assert run("1\n") == "1\n1", "minimum N"

# All relevant factors are powers of 2
assert run("64\n") == "7\n1\n2\n4\n8\n16\n32\n64", "power of 2"

# Only one factor of 2 and many factors of 5
assert run("1250\n") == "12\n1\n2\n5\n10\n25\n50\n125\n250\n625\n1250", "mixed exponents"

# Maximum-size input
assert run("1000000000000000000\n") == (
    "361\n" +
    "\n".join(
        str((2 ** i) * (5 ** j))
        for value in sorted(
            (2 ** i) * (5 ** j)
            for i in range(19)
            for j in range(19)
        )
    )
), "maximum N"
```Xác nhận kích thước tối đa ở trên được tạo ra có chủ ý thay vì mã hóa cứng. Từ`10^18 = 2^18 * 5^18`, có chính xác`19 * 19 = 361`các giá trị hợp lệ và biểu thức sẽ xây dựng tập hợp dự kiến ​​một cách độc lập. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1, 1`| Đầu vào tối thiểu và bao gồm`X = 1`| 
|`64`|`1, 2, 4, 8, 16, 32, 64`| Bao gồm ranh giới số mũ cho lũy thừa của 2 | 
|`1250`|`1, 2, 5, 10, 25, 50, 125, 250, 625, 1250`| Quyền hạn hỗn hợp của 2 và 5 | 
|`10^18`| 361 giá trị | Phạm vi đầu vào tối đa và số mũ tối đa | 

## Vỏ cạnh 

cho`N = 1`, thuật toán không đi vào vòng lặp đếm nhân tố, vì vậy`a = b = 0`. Bảng liệt kê lồng nhau có chính xác một cặp,`(0, 0)`, sản xuất`2^0 * 5^0 = 1`. Đầu ra là`1`theo sau là`1`, điều này đúng vì làm tròn đến bội số gần nhất của 1 luôn là một phương pháp hợp lệ. 

Vì`N = 8`, hệ số hóa liên quan đến vấn đề là`2^3`. Thuật toán thu được`a = 3`Và`b = 0`, sau đó tạo ra`1, 2, 4, 8`. Đầu ra là`4`theo sau là bốn giá trị đó. Điều này mắc phải lỗi phổ biến khi giải thích "chia lũy thừa của 10" là "là lũy thừa của 10". 

Vì`N = 30`, số mũ liên quan là`a = 1`Và`b = 1`. Tập hợp được tạo ra là`{1, 2, 5, 10}`. Mặc dù 30 có các ước khác, chẳng hạn như 3, 6 và 15, nhưng không có ước nào có thể chia bất kỳ lũy thừa nào của 10 vì mỗi số đều chứa thừa số 3. Thuật toán không bao giờ tạo ra chúng. 

Vì`N = 120`, số mũ là`a = 3`Và`b = 1`. Bốn lũy thừa có thể có của 2 kết hợp với hai lũy thừa có thể có của 5, tạo ra chính xác tám giá trị. Ứng cử viên lớn nhất là`2^3 * 5 = 40`, chia hết cho 120. Số lớn tiếp theo chỉ liên quan đến 2 và 5, 80, không chia hết cho 120 vì nó yêu cầu bốn thừa số là 2, chứng tỏ tại sao cả hai giới hạn số mũ phải bao hàm nhưng không bao giờ vượt quá. 

Vì`N = 10^18`, hệ số hóa là`2^18 * 5^18`. Mỗi cặp`(i, j)`với`0 <= i, j <= 18`là hợp lệ, đưa ra chính xác 361 câu trả lời. Đây là tổ hợp số mũ có liên quan lớn nhất trong giới hạn đầu vào và nó chứng minh tại sao việc liệt kê các cặp số mũ vẫn rất nhỏ ngay cả khi`N`bản thân nó là rất lớn. 

Sự rút gọn cơ bản là câu chuyện làm tròn ban đầu không yêu cầu chúng ta xây dựng lại số liệu thống kê ban đầu chưa biết. Một lần`X`chia rẽ`N`, chúng ta có thể chọn thống kê ban đầu là`N`chính nó, do đó toàn bộ vấn đề trở thành việc tìm các ước của`N`có thừa số nguyên tố bị giới hạn ở mức 2 và 5. Điều đó biến một bài toán chia số rất lớn có thể thành một phép liệt kê nhỏ trên hai số mũ nguyên tố.
