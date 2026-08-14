---
title: "CF 102302G - Trò chơi xếp chồng bên trái"
description: "Chúng ta có ba chồng xếp từ trái sang phải, chứa các viên đá a, b và c. Ở mỗi lượt, người chơi hiện tại chọn một ngăn xếp không trống và loại bỏ từ 1 đến m viên đá."
date: "2026-08-13T07:44:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102302
codeforces_index: "G"
codeforces_contest_name: "2019 USP-ICMC"
rating: 0
weight: 102302
solve_time_s: 221
verified: true
draft: false
---

[CF 102302G - Trò chơi xếp chồng bên trái](https://codeforces.com/problemset/problem/102302/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có ba ngăn xếp được sắp xếp từ trái sang phải, chứa`a`,`b`, Và`c`đá. Ở mỗi lượt, người chơi hiện tại chọn một ngăn xếp không trống và loại bỏ giữa`1`Và`m`đá. Hạn chế duy nhất là loại bỏ tảng đá cuối cùng của ngăn xếp: ngăn xếp chỉ có thể trở nên trống khi mọi ngăn xếp bên trái của nó đều trống. 

Ngăn xếp đầu tiên không có ngăn xếp nào ở bên trái nên nó luôn có thể được làm trống. Ngăn xếp thứ hai chỉ có thể được làm trống sau khi ngăn xếp thứ nhất biến mất và ngăn xếp thứ ba chỉ có thể được làm trống sau khi cả hai ngăn xếp trước đó đã biến mất. Người chơi nào loại bỏ được viên đá cuối cùng khỏi toàn bộ vị trí sẽ thắng, vì vậy đây là một trò chơi công bằng khi chơi bình thường. Chúng ta cần xác định xem vị trí ban đầu là Tomaz thắng hay Tomaz thua khi chơi tối ưu. 

Tất cả bốn giá trị đầu vào đều có thể đạt tới (10^{18}). Điều đó ngay lập tức loại trừ bất kỳ chương trình động nào về số lượng đá và thậm chí không thể mô phỏng tỷ lệ thuận với số vòng quay. Lời giải chỉ phải phụ thuộc vào các tính chất số học như phần dư modulo`m + 1`, với thời gian chạy không đổi hoặc logarit. 

Có hai hiệu ứng ranh giới rất dễ bị xử lý sai. Đầu tiên, ngăn xếp sau không thể bị làm trống trong khi ngăn xếp trước đó không trống. Ví dụ, với`m = 1`và đầu vào`1 1 2`, Tomaz không thể loại bỏ một viên đá khỏi ngăn xếp thứ hai ở nước đi đầu tiên, vì ngăn xếp đầu tiên vẫn còn chứa một viên đá. Một giải pháp coi ba ngăn xếp là trò chơi trừ độc lập sẽ làm cho trạng thái trò chơi bị sai. 

Trường hợp cạnh thứ hai là ngăn xếp đầu tiên luôn có thể được làm trống. Với`m = 3`và đầu vào`3 1 1 1`, Tomaz loại bỏ tảng đá duy nhất khỏi ngăn xếp đầu tiên, Danftito sau đó bị buộc vào ngăn xếp thứ hai, và Tomaz lấy tảng đá cuối cùng từ ngăn xếp thứ ba. Câu trả lời đúng là`Tomaz`. Một giải pháp áp dụng hạn chế "không thể để trống khi có thứ gì đó ở bên trái" cho ngăn xếp đầu tiên sẽ từ chối nước đi đầu tiên thắng một cách không chính xác. 

Trường hợp tinh tế thứ ba xảy ra khi một ngăn xếp đủ nhỏ để có thể làm trống trong một lần di chuyển. Ví dụ, với`m = 3`, một ngăn xếp chứa`1`,`2`, hoặc`3`đá có sự di chuyển trực tiếp về 0, trong khi một ngăn xếp chứa`4`không. Do đó, công thức tối ưu có cách xử lý riêng cho các giá trị nhiều nhất là`m`. 

## Phương pháp tiếp cận 

Một giải pháp đệ quy trực tiếp tuân theo định nghĩa của trò chơi. Từ một tiểu bang`(a,b,c)`, thử mọi số đá hợp pháp để loại bỏ khỏi mỗi ngăn xếp, xác định đệ quy xem vị trí kết quả có thắng hay không và tuyên bố vị trí hiện tại thắng nếu có ít nhất một nước đi đến vị trí thua. Điều này đúng vì mỗi nước đi đều làm giảm tổng số đá, do đó đồ thị trò chơi là hữu hạn. 

Vấn đề là số lượng trạng thái và bước di chuyển. Một bang có thể có tới`3m`các nước đi hợp pháp và một trò chơi có thể chứa tới`a + b + c`rẽ. Với các giá trị lớn bằng (10^{18}), ngay cả việc truyền tải tuyến tính giả định cũng đã vượt xa giới hạn. Cây trò chơi đệ quy tệ hơn theo cấp số nhân. 

Quan sát quan trọng là mặc dù vẫn chưa thể làm trống một ngăn xếp, nhưng việc trừ một viên đá khỏi nó tương đương với việc chơi một trò chơi trừ thông thường trên một viên đá ít hơn. Đối với một trò chơi trừ duy nhất mà chúng tôi có thể loại bỏ`1`bởi vì`m`đá, giá trị Grundy của một đống kích thước`x`là`x mod (m + 1)`. Vì vậy, trong khi một ngăn xếp bị khóa bởi một ngăn xếp không trống ở bên trái của nó, một chồng`x`đá hoạt động giống như một đống trừ kích thước`x - 1`, mang lại giá trị Grundy`(x - 1) mod (m + 1)`. 

Điều này biến ngăn xếp bị khóa bên phải thành vị trí Nim bình thường. Sự kiện đặc biệt duy nhất là khi ngăn xếp hiện đang hoạt động ở ngoài cùng bên trái đủ nhỏ để có thể trống trong một lần di chuyển. Tại thời điểm đó, trò chơi sẽ thay đổi từ phiên bản bị khóa của hậu tố sang phiên bản được mở khóa hoàn toàn của hậu tố. Vì chỉ có ba ngăn xếp nên chúng ta có thể xử lý quá trình chuyển đổi này một cách rõ ràng. 

Lý do tương tự trước tiên có thể giải quyết trò chơi hai ngăn và sau đó sử dụng kết quả đó làm trạng thái hậu tố cho trò chơi ba ngăn. Đối với ngăn xếp đầu tiên đủ lớn, nó không thể bị làm trống trong một nước đi, vì vậy trò chơi hiện tại chỉ đơn giản là XOR của giá trị trò chơi trừ của ngăn xếp đầu tiên và giá trị hậu tố bị khóa. Đối với ngăn xếp đầu tiên nhỏ, có sẵn một bước di chuyển về 0 và kết quả thay thế theo tính chẵn lẻ của ngăn xếp đầu tiên đó, tùy thuộc vào việc hậu tố đã mở khóa là thắng hay thua. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong`a+b+c`| số mũ trong`a+b+c`với ghi nhớ | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hãy để`k = m + 1`. Một trò chơi trừ thông thường mà người ta có thể loại bỏ`1`bởi vì`m`đá có giá trị Grundy`x mod k`. 
2. Đầu tiên giải quyết vị trí hai chồng`(b,c)`, bởi vì nó sẽ là hậu tố đã được mở khóa của trò chơi ba ngăn ban đầu. 
3. Trong khi`b`không trống, ngăn xếp`c`không thể làm trống được. Do đó giá trị bị khóa của nó là`(c - 1) mod k`. Nếu như`b > m`, ngăn xếp`b`cũng không thể bị làm trống trong một lần di chuyển, vì vậy hai ngăn xếp hoạt động như trò chơi trừ độc lập với các giá trị`b mod k`Và`(c - 1) mod k`. Vị thế bị mất chính xác khi XOR của họ bằng 0. 
4. Nếu`b <= m`, ngăn xếp`b`có thể được làm trống ngay lập tức. Nếu như`(c - 1) mod k`là khác 0, hậu tố bị khóa có giá trị Nim khác 0, do đó vị trí hiện tại đang thắng vì người chơi có thể sửa đổi hậu tố thành giá trị Nim bằng 0. 
5. Hộp đựng hai ngăn còn lại có`(c - 1) mod k = 0`. Hậu tố bị khóa khi đó là một vị trí thua cuộc. Làm trống`b`chuyển trực tiếp đến trò chơi một ngăn xếp thông thường`(c)`. Vị trí một ngăn xếp đó sẽ mất chính xác khi`c mod k = 0`. 
6. Do đó, khi`b <= m`Và`(c - 1) mod k = 0`, vị trí hai ngăn xếp xen kẽ với tính chẵn lẻ của`b`. Nếu như`(c mod k) != 0`, hậu tố đã được mở khóa là chiến thắng, thật kỳ lạ`b`đưa ra một vị trí thua cuộc. Nếu như`(c mod k) == 0`, hậu tố đã mở khóa sẽ bị mất, vì vậy ngay cả`b`đưa ra một vị trí thua cuộc. 
7. Bây giờ hãy tính giá trị Grundy bị khóa của hậu tố ban đầu`(b,c)`:`locked = ((b - 1) mod k) XOR ((c - 1) mod k)`. 
Điều này hợp lệ vì cả hai đều không`b`cũng không`c`có thể bị làm trống trong khi`a`là tích cực. 
8. Nếu`a > m`, ngăn xếp đầu tiên không thể bị làm trống trong một lần di chuyển. Do đó, toàn bộ vị trí là một tổng rời rạc thông thường của trò chơi trừ trên`a`và hậu tố bị khóa. Tomaz thắng chính xác khi`a mod k XOR locked != 0`. 
9. Nếu`a <= m`Và`locked != 0`, Tomaz có thể thực hiện nước đi thắng lợi bên trong hậu tố bị khóa, do đó thế cờ đang thắng. 
10. Nếu`a <= m`Và`locked == 0`, mọi chuyển động thay đổi`b`hoặc`c`đi đến vị trí chiến thắng. Câu hỏi duy nhất còn lại là điều gì sẽ xảy ra khi ngăn xếp đầu tiên giảm xuống 0. Nước đi đó đạt đến vị trí hai ngăn xếp đã được tính toán`(b,c)`. Nếu hậu tố đó thắng, các bang có`a = 1,2,3,...`luân phiên bắt đầu với trạng thái thua ở số lẻ`a`. Nếu hậu tố bị mất, chúng sẽ luân phiên bắt đầu với trạng thái mất ở mức chẵn`a`. 

### Tại sao nó hoạt động 

Điều bất biến là một ngăn xếp bị cấm trở thành trống sẽ hoạt động giống hệt như một trò chơi trừ sau khi loại bỏ một tảng đá khái niệm khỏi nó. Giá trị pháp lý của nó là`x -> x-d`vì`1 <= d <= min(m,x-1)`, đây chính xác là trò chơi trừ thông thường trên`x-1`đá. Do đó, hậu tố bị khóa có giá trị Nim tiêu chuẩn thu được bằng cách XOR các phần dư đã dịch chuyển này. 

Nước đi duy nhất không được mô hình Nim bị khóa này thể hiện là nước đi làm trống ngăn xếp ngoài cùng bên trái hiện tại. Một động thái như vậy sẽ thay đổi quy tắc của hậu tố vì ngăn xếp tiếp theo sẽ có thể mở khóa được. Đối với hai ngăn xếp, chúng tôi tính toán trực tiếp hậu tố đã mở khóa này, sau đó sử dụng nó làm kết quả cuối cùng của ngăn xếp đầu tiên. Vì ngăn xếp nhỏ đầu tiên có thể di chuyển đến mọi kích thước dương nhỏ hơn và cũng có thể di chuyển về 0, trạng thái thắng và thua của nó thay thế nhau bằng tính chẵn lẻ. Đối với ngăn xếp đầu tiên lớn, không thể truy cập được số 0 trong một lần di chuyển, do đó áp dụng đặc tính XOR thông thường. Những trường hợp này bao gồm mọi động thái hợp pháp, đưa ra kết quả chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def two_stack_wins(m, b, c):
    k = m + 1

    # While b > 0, c cannot be emptied.
    locked_c = (c - 1) % k

    if b > m:
        return ((b % k) ^ locked_c) != 0

    # b can be emptied immediately.
    if locked_c != 0:
        return True

    # If b is emptied, the remaining game is a normal
    # one-stack subtraction game on c.
    suffix_wins = (c % k) != 0

    # With locked suffix of Grundy value 0, outcomes alternate
    # as b goes through 1, 2, ..., m.
    if suffix_wins:
        # Losing for odd b.
        return (b % 2) == 0
    else:
        # Losing for even b.
        return (b % 2) == 1

def solve():
    m, a, b, c = map(int, input().split())
    k = m + 1

    # Outcome of the suffix after the first stack has been emptied.
    suffix_wins = two_stack_wins(m, b, c)

    # While a > 0, both later stacks are locked against becoming empty.
    locked = ((b - 1) % k) ^ ((c - 1) % k)

    if a > m:
        # The first stack cannot reach zero in one move, so the
        # position is an ordinary disjoint sum.
        first_value = a % k
        wins = (first_value ^ locked) != 0
    else:
        if locked != 0:
            wins = True
        else:
            # locked suffix is a P-position. The only exceptional
            # transition is a -> 0, which reaches the unlocked suffix.
            if suffix_wins:
                # P for odd a.
                wins = (a % 2) == 0
            else:
                # P for even a.
                wins = (a % 2) == 1

    print("Tomaz" if wins else "Danftito")

if __name__ == "__main__":
    solve()
```các`two_stack_wins`hàm thực hiện phân tích hậu tố đệ quy từ hướng dẫn. biểu thức`(c - 1) % k`là giá trị trò chơi trừ đã dịch chuyển của`c`trong khi ngăn xếp`b`vẫn không trống rỗng. 

Khi`b > m`, ngăn xếp thứ hai không thể bị làm trống trong một lần di chuyển, do đó không thể thực hiện được chuyển đổi đặc biệt nào. Trạng thái chỉ là XOR của hai trò chơi trừ bị khóa. Khi`b <= m`, quá trình chuyển đổi về 0 phải được xử lý riêng. 

Hàm chính áp dụng chính xác ý tưởng tương tự ở cấp độ trước đó. giá trị`locked`là XOR của các giá trị đã dịch chuyển của`b`Và`c`. Nếu như`a > m`, không thể đạt tới 0 từ ngăn xếp đầu tiên, vì vậy kiểm tra XOR thông thường là đủ. Nếu như`a <= m`, quá trình chuyển đổi về 0 tồn tại và kết quả phụ thuộc vào hậu tố đã được mở khóa đã được tính toán. 

Số nguyên Python có độ chính xác tùy ý, do đó giới hạn (10^{18}) không yêu cầu xử lý tràn đặc biệt. biểu thức`m + 1`cũng an toàn và các phép toán modulo được thực hiện trước khi so sánh XOR. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là:```
3 1 1 1
```Đây`m = 3`, Vì thế`k = 4`. 

| Biến | Giá trị | 
| --- | --- | 
|`m`| 3 | 
|`k`| 4 | 
|`a`| 1 | 
|`b`| 1 | 
|`c`| 1 | 
|`(b-1)%k`| 0 | 
|`(c-1)%k`| 0 | 
|`locked`| 0 | 

Đối với hậu tố hai ngăn xếp`(1,1)`, ngăn xếp`b`nhỏ và giá trị bị khóa của`c`là số không. Làm trống`b`rời khỏi trò chơi một chồng với`c = 1`, đó là chiến thắng. Do đó, hậu tố hai ngăn xếp sẽ bị mất. 

Bản gốc`a = 1`cũng nhỏ và`locked = 0`. Vì hậu tố bị mất nên lẻ`a`mang lại một vị trí chiến thắng. 

Đầu ra là:```
Tomaz
```Trò chơi chiến thắng thực tế chính xác là trình tự trực quan: Tomaz làm trống ngăn xếp đầu tiên, Danftito làm trống ngăn xếp thứ hai và Tomaz làm trống ngăn xếp thứ ba. 

### Dấu vết tùy chỉnh 

Hãy xem xét:```
1 2 1 1
```Đây`m = 1`, Vì thế`k = 2`. 

| Biến | Giá trị | 
| --- | --- | 
|`m`| 1 | 
|`k`| 2 | 
|`a`| 2 | 
|`b`| 1 | 
|`c`| 1 | 
|`(b-1)%k`| 0 | 
|`(c-1)%k`| 0 | 
|`locked`| 0 | 
|`a%k`| 0 | 
| Kết quả cuối cùng | P | 

Từ`a = 2 > m`, ngăn xếp đầu tiên không thể bị làm trống trong một lần di chuyển. Hậu tố bị khóa có giá trị bằng 0 và ngăn xếp đầu tiên có giá trị`2 % 2 = 0`. XOR của họ bằng 0, vì vậy vị thế đang thua. 

Tomaz chỉ có thể loại bỏ một viên đá khỏi ngăn xếp đầu tiên, để lại`(1,1,1)`, chiến thắng thuộc về Danftito. Điều này xác nhận trường hợp XOR ngăn xếp lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Một số lượng không đổi các phép toán modulo, XOR, so sánh và chẵn lẻ được thực hiện. | 
| Không gian | O(1) | Chỉ có một số biến số nguyên cố định được lưu trữ. | 

Giá trị đầu vào có thể lớn bằng (10^{18}), nhưng thuật toán không bao giờ lặp lại quá độ lớn của chúng. Nó giảm mọi ngăn xếp có liên quan xuống một modulo dư`m + 1`và kiểm tra số lượng trường hợp không đổi, do đó nó dễ dàng phù hợp với giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve_case(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        m, a, b, c = map(int, input().split())
        k = m + 1

        def two_stack_wins(m, b, c):
            k = m + 1
            locked_c = (c - 1) % k

            if b > m:
                return ((b % k) ^ locked_c) != 0

            if locked_c != 0:
                return True

            suffix_wins = (c % k) != 0

            if suffix_wins:
                return (b % 2) == 0
            return (b % 2) == 1

        suffix_wins = two_stack_wins(m, b, c)
        locked = ((b - 1) % k) ^ ((c - 1) % k)

        if a > m:
            wins = ((a % k) ^ locked) != 0
        else:
            if locked != 0:
                wins = True
            elif suffix_wins:
                wins = (a % 2) == 0
            else:
                wins = (a % 2) == 1

        print("Tomaz" if wins else "Danftito")
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
assert solve_case("3 1 1 1\n") == "Tomaz\n", "sample 1"

# Minimum-size parameters, same state as the sample with m = 1.
assert solve_case("1 1 1 1\n") == "Tomaz\n", "minimum values"

# Large first stack whose residue makes the whole position losing.
assert solve_case("1 2 1 1\n") == "Danftito\n", "large-stack XOR boundary"

# All stacks equal, with a modulus boundary.
assert solve_case("3 4 4 4\n") == "Danftito\n", "all equal"

# Maximum-size values, exercising arbitrary-precision arithmetic.
assert solve_case(
    "1000000000000000000 1000000000000000000 "
    "1000000000000000000 1000000000000000000\n"
) == "Tomaz\n", "maximum-size values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 1 1 1`|`Tomaz`| Cung cấp mẫu và mở khóa ngăn xếp nhỏ | 
|`1 1 1 1`|`Tomaz`| tối thiểu`m`và kích thước ngăn xếp tối thiểu | 
|`1 2 1 1`|`Danftito`| Ngăn xếp đầu tiên lớn và không có ranh giới XOR | 
|`3 4 4 4`|`Danftito`| Ngăn xếp và giá trị bằng nhau chính xác tại`m+1`| 
|`10^18 10^18 10^18 10^18`|`Tomaz`| Độ lớn đầu vào tối đa và xử lý số nguyên Python | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng đầu tiên là ngăn xếp đầu tiên có thể luôn trống. Vì`3 1 1 1`,`a = 1 <= m`, do đó Tomaz có thể di chuyển trực tiếp đến`(0,1,1)`. Hậu tố hai ngăn xếp`(1,1)`đang thua vì Danftito phải làm trống ngăn thứ hai, sau đó Tomaz lấy viên đá cuối cùng. Thuật toán tính toán`locked = 0`, thấy hậu tố bị mất và sử dụng số lẻ`a`để phân loại trạng thái ban đầu là chiến thắng. 

Trường hợp cạnh thứ hai là ngăn xếp sau chưa thể làm trống được. Coi như`1 1 2 1`. Đây`k = 2`và giá trị bị khóa của hậu tố là`((2-1) mod 2) XOR ((1-1) mod 2) = 1`. Vì giá trị bị khóa khác 0 nên vị trí hiện tại sẽ thắng ngay. Người chơi có thể chơi bên trong hậu tố bị khóa mà không làm trống ngăn xếp thứ hai một cách bất hợp pháp. điều trị`b`như một đống thông thường sẽ bỏ lỡ hạn chế này. 

Trường hợp cạnh thứ ba là ngăn xếp đầu tiên lớn hơn`m`. Vì`1 2 1 1`, ngăn xếp đầu tiên chứa hai viên đá trong khi chỉ có thể loại bỏ một viên mỗi lần di chuyển, vì vậy nó không thể về 0 ngay lập tức. Hậu tố bị khóa có giá trị bằng 0 và`a mod 2 = 0`, cho tổng XOR bằng không. Tomaz chưa chuyển sang thế thua nên đáp án là`Danftito`. Đây chính xác là tình huống mà phép phân rã Nim thông thường lại có hiệu lực. 

Trường hợp cạnh cuối cùng là một ngăn xếp chính xác tại một ranh giới mô đun. Với`m = 3`, một đống`4`có giá trị trò chơi trừ`4 mod 4 = 0`, trong khi một đống bị khóa`4`có giá trị`(4-1) mod 4 = 3`. Việc nhầm lẫn hai biểu thức này là một lỗi cổ điển. Thuật toán sử dụng`x % (m+1)`chỉ dành cho một ngăn xếp có thể bị làm trống và`(x-1) % (m+1)`đối với một ngăn xếp hiện bị cấm làm trống, phù hợp với các bước di chuyển hợp pháp thực tế trong từng giai đoạn.
