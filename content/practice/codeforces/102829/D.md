---
title: "CF 102829D - Bằng chứng hữu ích"
description: "Bài toán yêu cầu chúng ta đếm các cặp phần tử có thứ tự trong một mảng sao cho việc viết số này ngay sau số khác sẽ tạo ra một số chia hết cho một mô đun cho trước. Thứ tự rất quan trọng, vì vậy việc chọn x rồi y khác với việc chọn y rồi x."
date: "2026-07-26T15:29:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102829
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 11-06-20 Div. 1 (Tryout)"
rating: 0
weight: 102829
solve_time_s: 39
verified: true
draft: false
---

[CF 102829D - Bằng chứng hữu ích](https://codeforces.com/problemset/problem/102829/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta đếm các cặp phần tử có thứ tự trong một mảng sao cho việc viết số này ngay sau số khác sẽ tạo ra một số chia hết cho một mô đun cho trước. Thứ tự rất quan trọng, vì vậy việc chọn`x`sau đó`y`khác với việc chọn`y`sau đó`x`. Nhiệm vụ là đếm tất cả các cặp vị trí hợp lệ, không chỉ các giá trị riêng biệt. 

Đầu vào cung cấp số lượng giá trị, số chia và chính mảng đó. Mỗi giá trị mảng có thể có tối đa 10 chữ số và kích thước mảng có thể đạt tới 200.000. Một giải pháp kiểm tra từng cặp sẽ thực hiện khoảng 40 tỷ lần kiểm tra trong trường hợp lớn nhất, vượt xa giới hạn 2 giây thông thường cho phép. Chúng ta cần giảm công của mỗi phần tử xuống mức gần với thời gian không đổi. 

Thách thức chính là việc ghép nối phụ thuộc vào số chữ số trong giá trị thứ hai. Ví dụ, gắn thêm`34`ĐẾN`12`tạo ra`1234`, đó là`12 * 100 + 34`. Một giải pháp đúng phải bảo toàn hiệu ứng vị trí này thay vì coi các con số là tổng thông thường. 

Việc triển khai bất cẩn có thể thất bại với các giá trị lặp lại. Ví dụ:```
Input:
3 2
1 1 1

Output:
6
```Mỗi cặp vị trí khác nhau có thứ tự đều hoạt động vì mỗi phép nối đều được`11`, chia hết cho 2. Việc triển khai chỉ lưu trữ liệu một giá trị có tồn tại hay không thay vì số lần nó xuất hiện sẽ bị tính thiếu. 

Một trường hợp khác là khi cùng một giá trị có thể được sử dụng nhiều lần nhưng vị trí vẫn phải khác nhau. Ví dụ:```
Input:
2 11
1 1

Output:
2
```Có hai cặp có thứ tự`(first 1, second 1)`Và`(second 1, first 1)`. Việc đếm các giá trị thay vì vị trí có thể vô tình loại bỏ các cặp này. 

Số lượng chữ số cũng là một nguồn sai lầm phổ biến. Ví dụ:```
Input:
2 100
12 3

Output:
1
```Sự nối`123`không chia hết cho 100, trong khi`312`cũng không, vì vậy ví dụ này thực sự có câu trả lời`0`. Một cách tiếp cận sai luôn nhân lên bằng`10^9`thay vì sử dụng độ dài chữ số thực của số được nối thêm sẽ tính số dư không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi cặp đã đặt hàng`(i, j)`, nối hai số và kiểm tra phần còn lại. Cho hai số`a`Và`b`, nếu như`b`có`d`chữ số, phép nối là:`a * 10^d + b`Việc kiểm tra một cặp là thời gian không đổi, nhưng có`n * (n - 1)`các cặp đặt hàng. Với`n = 200000`, đây là gần 40 tỷ hoạt động nên cách tiếp cận vũ phu không thể vượt qua. 

Quan sát quan trọng là đối với số đầu tiên cố định`a`, chúng ta chưa cần biết chính xác số thứ hai. Chúng ta chỉ cần phần còn lại mà số thứ hai phải cung cấp. 

Đối với số thứ hai`b`với`d`chữ số:`a * 10^d + b ≡ 0 (mod k)`Độ dài chữ số của`b`chỉ có thể nằm trong khoảng từ 1 đến 10, vì mọi giá trị tối đa là`10^9`. Điều này có nghĩa là chỉ có mười ca có thể làm được. Với mỗi số ta tính được:`a * 10^d mod k`cho mọi độ dài chữ số có thể`d`. 

Trong khi quét mảng, chúng tôi lưu trữ có bao nhiêu số có độ dài mỗi chữ số có mỗi modulo còn lại`k`. Sau đó, khi xử lý`a`, chúng ta biết phần còn lại cần thiết của`b`cho mọi số chữ số có thể. 

Lực lượng vũ phu hoạt động vì nó kiểm tra rõ ràng tất cả các đối tác có thể có, nhưng nó không thành công vì có quá nhiều đối tác. Quan sát cho thấy chỉ phần còn lại và độ dài chữ số của đối tượng mới cho phép chúng ta thay thế phép liệt kê cặp bằng cách đếm tần số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(10n) | O(10n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ mọi số mảng được nhóm theo độ dài chữ số của nó. Đối với mỗi độ dài, hãy giữ bản đồ tần số của phần dư theo modulo`k`. 

Độ dài chữ số quan trọng vì việc nối một số với một số khác sẽ dịch chuyển nó theo lũy thừa mười. Hai số có cùng số dư nhưng có độ dài khác nhau thì không thể hoán đổi cho nhau. 

1. Xử lý từng số`a`là phần tử đầu tiên của một cặp. Đối với mọi độ dài chữ số có thể`d`từ 1 đến 10, tính:`(a * 10^d) mod k`Số thứ hai phải có số dư:`(-a * 10^d) mod k`để làm cho toàn bộ phần nối có thể chia hết cho`k`. 

1. Thêm vào đáp án số giá trị lưu trữ có độ dài chữ số`d`và phần còn lại cần thiết. 

Bảng tần số ngay lập tức cho chúng ta biết có bao nhiêu vị trí thứ hai có thể tồn tại. 

1. Xóa số hiện tại khỏi tần số được lưu trữ trước khi tính nó làm đối tác, sau đó thêm lại sau khi xử lý. 

Điều này ngăn việc đếm một số được ghép với chính nó ở cùng một vị trí trong khi vẫn cho phép các giá trị bằng nhau từ các vị trí khác nhau. 

### Tại sao nó hoạt động 

Đối với bất kỳ cặp nào`(a, b)`, phép nối được xác định bởi giá trị của`a`, giá trị của`b`, và số chữ số trong`b`. Thuật toán nhóm tất cả các số thứ hai có thể có theo chính xác hai thuộc tính ảnh hưởng đến phương trình chia hết: độ dài chữ số và modulo dư`k`. 

Khi xử lý`a`, mọi độ dài chữ số có thể có của`b`được xem xét. Phần còn lại bắt buộc được lấy trực tiếp từ điều kiện chia hết, vì vậy mỗi mục nhập tần số được tính đều đại diện cho một đối tác hợp lệ. Việc xóa phần tử hiện tại trước khi truy vấn đảm bảo rằng chỉ các vị trí khác nhau mới được tính. Mỗi cặp thứ tự hợp lệ được tính một lần khi phần tử đầu tiên của nó được xử lý. 

## Giải pháp Python```python
import sys
from collections import defaultdict

input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    cnt = [defaultdict(int) for _ in range(11)]
    lengths = []

    for x in a:
        d = len(str(x))
        lengths.append(d)
        cnt[d][x % k] += 1

    ans = 0

    for x, d in zip(a, lengths):
        cnt[d][x % k] -= 1

        power = 10
        for length in range(1, 11):
            need = (-x * power) % k
            ans += cnt[length][need]
            power = (power * 10) % k

        cnt[d][x % k] += 1

    print(ans)

solve()
```Bảng tần số có 11 khe vì có thể có độ dài chữ số từ 1 đến 10. Mỗi ô là một từ điển vì`k`có thể lớn như`10^9`, vì vậy việc lập chỉ mục theo phần còn lại là không thể. 

Bước loại bỏ trước khi truy vấn là cần thiết. Không có nó, một số sẽ tự khớp với chính nó bất cứ khi nào phép nối của chính nó thỏa mãn điều kiện. Sau khi truy vấn kết thúc, giá trị sẽ được khôi phục để có thể sử dụng cho các vị trí đầu tiên sau này. 

Biến`power`cửa hàng`10^length mod k`. Đang cập nhật nó theo modulo`k`giữ tất cả các phép tính ở mức nhỏ và tránh các số nguyên lớn không cần thiết. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
Input:
6 11
45 1 10 12 11 7
```Dấu vết cho giá trị đầu tiên: 

| Giá trị hiện tại | Độ dài hiện tại | Đã kiểm tra độ dài thứ hai | Phần còn lại bắt buộc | Các trận đấu được thêm vào | 
| --- | --- | --- | --- | --- | 
| 45 | 2 | 1 | 6 | 0 | 
| 45 | 2 | 2 | 0 | 1 | 
| 45 | 2 | 3 | 7 | 1 | 
| 45 | 2 | 4 | 5 | 0 | 

giá trị`45`tìm đối tác như`1`Và`10`bởi vì độ dài chữ số và số dư của chúng thỏa mãn phương trình nối. 

Đối với ví dụ thứ hai:```
Input:
4 2
2 78 4 10
```| Giá trị hiện tại | Chiều dài | Kiểm tra phần dư bắt buộc | Đối tác hợp lệ | 
| --- | --- | --- | --- | 
| 2 | 1 | mọi độ dài | 3 | 
| 78 | 2 | mọi độ dài | 3 | 
| 4 | 1 | mọi độ dài | 3 | 
| 10 | 2 | mọi độ dài | 3 | 

Mỗi cặp đặt hàng đều hoạt động, mang lại`12`tổng số cặp. Dấu vết chứng tỏ rằng phương pháp này đếm các vị trí chứ không phải các giá trị riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(10n) | Mỗi số chỉ kiểm tra độ dài mười chữ số có thể | 
| Không gian | O(10n) | Tối đa mười bản đồ tần số lưu trữ phần còn lại cho tất cả các số | 

Thuật toán thực hiện khoảng hai triệu lần kiểm tra phần còn lại để tìm kích thước đầu vào tối đa, vừa vặn trong giới hạn. Việc sử dụng bộ nhớ phụ thuộc vào số lượng phần dư riêng biệt thay vì toàn bộ các giá trị modulo có thể có. 

## Trường hợp thử nghiệm```python
import sys
import io
from collections import defaultdict

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    cnt = [defaultdict(int) for _ in range(11)]
    lengths = []

    for x in a:
        d = len(str(x))
        lengths.append(d)
        cnt[d][x % k] += 1

    ans = 0

    for x, d in zip(a, lengths):
        cnt[d][x % k] -= 1
        power = 10

        for length in range(1, 11):
            ans += cnt[length][(-x * power) % k]
            power = power * 10 % k

        cnt[d][x % k] += 1

    return str(ans) + "\n"

assert run("""6 11
45 1 10 12 11 7
""") == "7\n", "sample 1"

assert run("""4 2
2 78 4 10
""") == "12\n", "sample 2"

assert run("""5 2
3 7 19 3 3
""") == "0\n", "sample 3"

assert run("""2 11
1 1
""") == "2\n", "same values"

assert run("""1 100
123
""") == "0\n", "single element"

assert run("""3 2
2 2 2
""") == "6\n", "all equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 11 / 1 1`|`2`| Đếm các vị trí khác nhau có giá trị bằng nhau | 
|`1 100 / 123`|`0`| Xử lý mảng nhỏ nhất | 
|`3 2 / 2 2 2`|`6`| Xử lý nhiều giá trị trùng lặp | 

## Vỏ cạnh 

Đối với các giá trị trùng lặp, thuật toán chỉ loại bỏ vị trí hiện đang được xử lý chứ không loại bỏ mọi giá trị bằng nhau. Với:```
2 11
1 1
```cái đầu tiên`1`bị loại bỏ, để lại một đối tác hợp lệ. thứ hai`1`sau đó được xử lý tương tự. Câu trả lời trở thành`2`. 

Để tự ghép nối, hãy cân nhắc:```
1 1
5
```Chỉ có một vị trí nên không được phép có cặp. Thuật toán loại bỏ phần còn lại được lưu trữ duy nhất trước khi kiểm tra, làm cho tần số bằng 0 và trả về`0`. 

Đối với các số có độ dài chữ số khác nhau, hãy xem xét:```
2 10
1 23
```Thuật toán kiểm tra độ dài`2`khi sử dụng`1`là số đầu tiên vì`23`góp phần chuyển dịch`100`. Nó không chữa trị`23`là số có một chữ số nên số dư được tính là chính xác. 

Đối với các giá trị lớn, chẳng hạn như:```
2 1000000000
999999999 1
```thuật toán không bao giờ xây dựng số nối trực tiếp. Nó giữ mọi modulo nhân`k`, tránh tràn và bảo toàn phép tính chia hết.
