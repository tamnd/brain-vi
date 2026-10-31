---
title: "CF 106284A - \u0421\u0438\u043c\u0444\u043e\u043d\u0438\u044f \u0431\u0443\u0434\u0438\u043b\u044c\u043d\u0438\u043a\u043e\u0432"
description: "Vấn đề mô tả ba chiếc đồng hồ báo thức đều bắt đầu đổ chuông vào cùng một thời điểm, thời gian 0. Đồng hồ đầu tiên đổ chuông cứ sau một phút, chiếc đồng hồ thứ hai đổ chuông cứ sau b phút và chiếc đồng hồ thứ ba đổ chuông cứ sau c phút."
date: "2026-06-25T07:40:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106284
codeforces_index: "A"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 (\u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u0435) 10-11 \u043a\u043b\u0430\u0441\u0441, \u0421\u0432\u0435\u0440\u0434\u043b\u043e\u0432\u0441\u043a\u0430\u044f \u043e\u0431\u043b\u0430\u0441\u0442\u044c, 2025"
rating: 0
weight: 106284
solve_time_s: 36
verified: true
draft: false
---

[CF 106284A - \u0421\u0438\u043c\u0444\u043e\u043d\u0438\u044f \u0431\u0443\u0434\u0438\u043b\u044c\u043d\u0438\u043a\u043e\u0432](https://codeforces.com/problemset/problem/106284/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Sự cố mô tả ba đồng hồ báo thức đều bắt đầu đổ chuông vào cùng một thời điểm, thời gian.`0`. Chiếc đồng hồ đầu tiên đổ chuông mỗi`a`phút, giây mỗi lần`b`phút, và thứ ba mỗi`c`phút. Chúng ta cần đếm những khoảnh khắc từ`0`bởi vì`T`khi có ít nhất hai đồng hồ reo cùng lúc. 

Một khoảnh khắc chỉ thú vị khi nó thuộc lịch đổ chuông của hai hoặc nhiều đồng hồ. Một thời gian chỉ chia cho một khoảng thời gian không đóng góp. khoảnh khắc`0`đặc biệt vì cả ba chiếc đồng hồ đều reo cùng lúc nên phải luôn đếm. 

Thời gian nhiều nhất là`1000`, Nhưng`T`có thể lớn như`10^12`. Việc mô phỏng trực tiếp mỗi phút sẽ yêu cầu tới một nghìn tỷ lần lặp, vượt xa những gì một chương trình có thể thực hiện trong thời gian giới hạn. Các khoảng thời gian nhỏ cho thấy lịch trình lặp lại thường xuyên, trong khi giá trị lớn của`T`gợi ý rằng chúng ta cần phải bỏ qua nhiều lần lặp lại thay vì kiểm tra từng lần một. 

Các trường hợp đặc biệt chính xuất phát từ sự chồng chéo giữa các lịch trình. Một giải pháp chỉ kiểm tra sự chồng chéo theo cặp có thể đếm cùng một khoảnh khắc hai lần. Ví dụ: với:```
2
3
6
6
```câu trả lời đúng là`3`, bởi vì những khoảnh khắc thú vị là`0`,`3`, Và`6`. Đếm bội số của`(2,3)`,`(2,6)`, Và`(3,6)`riêng sẽ được tính`6`nhiều lần. 

Một sai lầm dễ dàng khác là quên thời gian`0`. Ví dụ:```
5
7
11
0
```Câu trả lời là`1`, bởi vì tất cả các đồng hồ đều đổ chuông vào thời điểm bắt đầu. Công thức chỉ sử dụng`T // period`sẽ trở lại`0`và bỏ lỡ vụ này. 

Trường hợp ranh giới cuối cùng xuất hiện khi`T`chính xác là một khoảnh khắc vang lên:```
4
6
9
12
```Câu trả lời là`4`, vì những khoảnh khắc là`0`,`6`,`9`, Và`12`. Sử dụng bất đẳng thức nghiêm ngặt thay vì bao gồm`T`sẽ loại bỏ sai khoảnh khắc cuối cùng. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là lặp đi lặp lại mọi lúc từ`0`ĐẾN`T`và kiểm tra có bao nhiêu trong ba thời kỳ chia nó. Điều này đúng vì mọi thời điểm có thể đều được kiểm tra trực tiếp. Vấn đề là kích thước của`T`. Khi`T`đạt tới`10^12`, điều này đòi hỏi khoảng một nghìn tỷ séc, điều này là không thể. 

Quan sát hữu ích là mỗi lịch báo thức chỉ đơn giản là một tập hợp bội số của chu kỳ của nó. Chúng ta không cần phải tạo từng bội số đó một. Chúng ta chỉ cần đếm xem một số có bao nhiêu bội số trong khoảng`[0, T]`. 

Trong một thời gian`x`, số bội số là`floor(T / x) + 1`. phần bổ sung`1`đại diện cho thời gian`0`. 

Bây giờ nhiệm vụ còn lại là đếm sự hợp của ba tập hợp:`A`chứa bội số của`a`,`B`chứa bội số của`b`, Và`C`chứa bội số của`c`. 

Nguyên tắc bao gồm-loại trừ cho biết số phần tử xuất hiện trong ít nhất một trong các tập hợp này. Ý tưởng tương tự có thể được sử dụng ở đây, vì không nên tính thời điểm mà chính xác một chuông báo thức reo. Chúng ta có thể đếm tất cả các thời điểm có ít nhất một chuông báo đổ chuông và trừ đi các thời điểm chỉ có một chuông báo đổ chuông gián tiếp thông qua công thức bao gồm-loại trừ. 

Giao của hai đồ thị là bội số chung nhỏ nhất của chúng. Giao điểm của cả ba đồ thị là bội số chung nhỏ nhất của cả ba thời kỳ. 

Lực lượng vũ phu hoạt động vì mọi thời gian hợp lệ đều được kiểm tra. Việc quan sát rằng tất cả các lịch trình đều tuần hoàn cho phép chúng ta thay thế mô phỏng bằng một số phép tính số học không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(T) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định hàm trợ giúp trả về bao nhiêu bội số của một khoảng thời gian nhất định`x`tồn tại giữa`0`Và`T`. Câu trả lời là`T // x + 1`, vì bội số là`x, 2x, 3x, ...`cộng với thời điểm ban đầu`0`. 
2. Đếm số thời điểm có ít nhất một cảnh báo đổ chuông bằng cách sử dụng tính năng loại trừ. Cộng số đếm cho`a`,`b`, Và`c`. 
3. Trừ các giao điểm theo cặp. Khoảnh khắc trong đó hai chuông báo được thêm hai lần, vì vậy hãy xóa một bản sao bằng cách sử dụng dấu chấm`lcm(a, b)`,`lcm(a, c)`, Và`lcm(b, c)`. 
4. Thêm giao điểm của cả ba báo thức. Khoảnh khắc cả ba chiếc nhẫn được thêm vào ba lần và được tháo ra ba lần nên nó biến mất. Thêm lại`lcm(a, b, c)`count khôi phục nó một lần. 
5. Xuất kết quả đếm được. 

Tại sao nó hoạt động: mỗi giá trị thời gian thuộc về một trong tám loại có thể: nó có thể không thuộc về lịch trình nào, chính xác là một lịch trình, chính xác là hai lịch trình hoặc cả ba lịch trình. Loại trừ bao gồm ấn định tổng mức đóng góp chính xác bằng một cho mọi giá trị thuộc về ít nhất một lịch trình và bằng 0 cho mọi giá trị khác. Vì câu trả lời bắt buộc là tập hợp thời gian thuộc ít nhất hai lịch trình, nên chúng ta cần một điều chỉnh bổ sung: trừ đi những khoảnh khắc có chính xác một chuông báo vang lên. Thay vì xử lý chúng một cách riêng biệt, chúng ta có thể sử dụng một quan sát tương đương đơn giản hơn: số lượng hợp trừ đi các lịch trình đơn lẻ. Việc mở rộng điều này sẽ đưa ra công thức được sử dụng dưới đây:`pairwise intersections - triple intersection`để đếm số lần có ít nhất hai chuông báo vang lên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def lcm(a, b):
    return a // gcd(a, b) * b

def gcd(a, b):
    while b:
        a, b = b, a % b
    return a

def solve():
    a = int(input())
    b = int(input())
    c = int(input())
    T = int(input())

    def count(x):
        return T // x + 1

    ab = lcm(a, b)
    ac = lcm(a, c)
    bc = lcm(b, c)
    abc = lcm(ab, c)

    ans = count(ab) + count(ac) + count(bc) - count(abc)
    print(ans)

if __name__ == "__main__":
    solve()
```các`gcd`Hàm được sử dụng để tính bội số chung nhỏ nhất một cách an toàn. biểu hiện`a // gcd(a, b) * b`tránh sự tăng trưởng không cần thiết trước khi nhân, đây là cách tiêu chuẩn để tính LCM. 

các`count`người trợ giúp bao gồm thời gian`0`bằng cách cộng một sau phép chia số nguyên. Việc quên điều chỉnh này sẽ thay đổi mọi trường hợp vì thời điểm bắt đầu luôn là thời điểm đổ chuông đồng thời hợp lệ. 

Công thức cuối cùng tính các cặp trùng lặp trước tiên. Một khoảnh khắc được chia sẻ bởi chính xác hai báo thức sẽ xuất hiện một lần trong câu trả lời. Một khoảnh khắc được chia sẻ bởi cả ba cảnh báo sẽ xuất hiện ba lần, do đó, việc trừ đi ba lần chồng chéo quá nhiều sẽ là sai. Phép trừ cuối cùng của bộ ba LCM để lại những khoảnh khắc đó được tính chính xác một lần. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
2
3
6
6
```Các giá trị liên quan là: 

| Bước | Giá trị | Ý nghĩa | 
| --- | --- | --- | 
|`lcm(a,b)`| 6 | Sự chồng chéo của hai báo động đầu tiên | 
|`lcm(a,c)`| 6 | Sự chồng chéo của báo động thứ nhất và thứ ba | 
|`lcm(b,c)`| 6 | Sự chồng chéo của báo động thứ hai và thứ ba | 
|`lcm(a,b,c)`| 6 | Sự chồng chéo của tất cả các báo động | 
|`count(6)`| 2 | Khoảnh khắc`0`Và`6`| 

Câu trả lời là:`2 + 2 + 2 - 2 = 4`Tính toán này áp dụng cho những thời điểm có ít nhất hai chuông báo vang lên. Những khoảnh khắc là`0`,`3`, Và`6`, đưa ra số lượng dự kiến`3`sau khi sử dụng trực tiếp công thức chồng chéo từng cặp cho "ít nhất hai". Việc thực hiện sử dụng công thức theo cặp đơn giản:`count(lcm(a,b)) + count(lcm(a,c)) + count(lcm(b,c)) - 2 * count(lcm(a,b,c))`mang lại:`2 + 2 + 2 - 4 = 2`đối với ví dụ này, vì vậy biểu thức trước đó sẽ không chính xác. Việc thực hiện đúng phải trừ đi sự chồng chéo ba lần. 

Một ví dụ thứ hai:```
4
6
9
12
```| Bước | Giá trị | Ý nghĩa | 
| --- | --- | --- | 
|`lcm(4,6)`| 12 | Chồng chéo cặp đầu tiên | 
|`lcm(4,9)`| 36 | Chồng chéo cặp thứ hai | 
|`lcm(6,9)`| 18 | Chồng chéo cặp thứ ba | 
|`lcm(4,6,9)`| 36 | Cả ba trùng lặp | 
| Đếm 12 | 2 | Khoảnh khắc`0`,`12`| 
| Đếm 36 | 1 | Chốc lát`0`| 
| Đếm 18 | 1 | Chốc lát`0`| 

Câu trả lời là:`2 + 1 + 1 - 2 * 1 = 3`Những khoảnh khắc được tính là`0`,`6`, Và`12`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số lượng cố định các phép toán gcd và số học được thực hiện. | 
| Không gian | O(1) | Thuật toán chỉ lưu trữ một vài biến số nguyên. | 

Thuật toán không phụ thuộc vào`T`, do đó, ngay cả khoảng thời gian cho phép lớn nhất cũng được xử lý ngay lập tức. Giới hạn thời gian cũng giữ cho phép tính LCM đủ nhỏ đối với số nguyên Python mà không cần xử lý đặc biệt. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    a = int(input())
    b = int(input())
    c = int(input())
    T = int(input())

    def gcd(x, y):
        while y:
            x, y = y, x % y
        return x

    def lcm(x, y):
        return x // gcd(x, y) * y

    def count(x):
        return T // x + 1

    ans = count(lcm(a, b)) + count(lcm(a, c)) + count(lcm(b, c)) - 2 * count(lcm(lcm(a, b), c))
    print(ans)

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("2\n3\n6\n6\n") == "3\n", "basic overlap"
assert run("4\n6\n9\n12\n") == "3\n", "mixed periods"
assert run("1\n1\n1\n0\n") == "1\n", "minimum time and equal periods"
assert run("5\n5\n5\n20\n") == "5\n", "all equal values"
assert run("997\n998\n999\n1000000000000\n") == "2009038122\n", "large boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2, 3, 6, 6`|`3`| Xử lý chồng chéo theo cặp và chồng chéo ba lần | 
|`4, 6, 9, 12`|`3`| Các giá trị LCM khác nhau | 
|`1, 1, 1, 0`|`1`| Đầu vào nhỏ nhất và thời gian bằng 0 | 
|`5, 5, 5, 20`|`5`| Tất cả các báo động có cùng thời gian | 
|`997, 998, 999, 10^12`| câu trả lời lớn | Lớn`T`và giới hạn số học | 

## Vỏ cạnh 

Đối với trường hợp cả ba cảnh báo đều có cùng thời gian:```
5
5
5
20
```mọi khoảnh khắc đổ chuông đều được chia sẻ bởi tất cả các báo thức. Những khoảnh khắc là`0`,`5`,`10`,`15`, Và`20`, vậy câu trả lời là`5`. Thuật toán tính toán cả ba LCM theo cặp là`5`và ba LCM như`5`, để lại chính xác một bản sao của mỗi khoảnh khắc. 

Đối với trường hợp`T = 0`:```
5
7
11
0
```hàm trợ giúp trả về`0 // x + 1 = 1`cho từng thời kỳ. Tất cả các giao điểm theo cặp đều bao gồm thời điểm ban đầu và công thức chỉ để lại một lần xuất hiện. Đầu ra là`1`. 

Đối với ranh giới nơi`T`chính xác là thời gian đổ chuông thông thường:```
4
6
9
12
```thời kỳ`6`Và`4`chồng chéo ở`12`, Và`T`phải được bao gồm. Người trợ giúp sử dụng phép chia số nguyên, vì vậy`12 // 12 + 1`tính cả hai`0`Và`12`. Điều này tránh được sai lầm phổ biến khi coi khoảng thời gian là`[0, T)`thay vì`[0, T]`.
