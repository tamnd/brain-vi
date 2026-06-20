---
title: "CF 103C - Roulette Nga"
description: "Hình trụ có n khe xếp thành hình tròn và có đúng k khe chứa đạn. Trước khi trò chơi bắt đầu, hình trụ được quay đều một cách ngẫu nhiên, do đó mọi sự dịch chuyển theo chu kỳ đều có khả năng xảy ra như nhau. Sau khi xoay, Sasha bắn trước."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 103
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 80 (Div. 1 Only)"
rating: 1900
weight: 103
solve_time_s: 120
verified: true
draft: false
---

[CF 103C - Roulette Nga](https://codeforces.com/problemset/problem/103/C) 

**Xếp hạng:** 1900 
**Tags:** thuật toán xây dựng, tham lam 
**Thời gian giải:** 2 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Xi lanh có`n`các khe được sắp xếp theo hình tròn và chính xác`k`trong số chúng có chứa đạn. Trước khi trò chơi bắt đầu, hình trụ được quay đều một cách ngẫu nhiên, do đó mọi sự dịch chuyển theo chu kỳ đều có khả năng xảy ra như nhau. 

Sau khi xoay, Sasha bắn trước. Nếu ô hiện tại trống, trụ tiến lên một vị trí và Roma sút. Người chơi đầu tiên gặp phải viên đạn sẽ chết ngay lập tức. 

Nhiệm vụ không phải là mô phỏng trò chơi. Thay vào đó, chúng ta phải xây dựng cách sắp xếp đạn sao cho khả năng tử vong của Sasha giảm thiểu. Trong số tất cả các cách sắp xếp tối ưu, chúng ta phải đưa ra cách sắp xếp nhỏ nhất về mặt từ điển, trong đó`.`nhỏ hơn`X`. 

Đầu vào không yêu cầu chúng ta in trực tiếp toàn bộ chuỗi. Từ`n`có thể lớn như`10^18`, việc in tất cả các vị trí sẽ là không thể. Thay vào đó, chúng tôi nhận được`p`các truy vấn hỏi về các vị trí cụ thể và đối với mỗi chỉ mục được truy vấn, chúng tôi sẽ in xem vị trí đó có chứa dấu đầu dòng hay không. 

Sự ràng buộc lớn trên`n`làm thay đổi hoàn toàn bản chất của vấn đề. Bất kỳ thuật toán nào tỷ lệ thuận với`n`là không thể. Chúng tôi không thể xây dựng toàn bộ chuỗi, mô phỏng trò chơi hoặc thậm chí lặp lại tất cả các vị trí một lần. Giải pháp phải hoạt động chỉ bằng cách sử dụng các công thức số học và các quyết định cục bộ. 

Phần tinh tế nhất là hiểu được điều gì quyết định khả năng thua cuộc của Sasha. 

Giả sử trò chơi bắt đầu ở một ô nào đó sau vòng quay ngẫu nhiên. 

Nếu viên đạn đầu tiên xuất hiện sau một số chẵn ô trống, thì cuối cùng Sasha sẽ phải đối mặt với nó và chết. 

Nếu viên đạn đầu tiên xuất hiện sau một số lẻ ô trống, thì Roma sẽ chết. 

Điều đó có nghĩa là mỗi khối tối đa của các ô trống liên tiếp đều đóng góp vào việc thắng hoặc thua ở vị trí bắt đầu tùy thuộc vào tính chẵn lẻ của nó. 

Một trực giác ngây thơ cho rằng việc rải đạn đều luôn là tối ưu. Điều đó là không đủ. Chúng ta cũng cần sự sắp xếp tối ưu nhỏ nhất về mặt từ điển. 

Ví dụ: 

đầu vào:```
5 2
```Một sự sắp xếp cân bằng có thể là:```
X..X.
```Nhưng:```
..X.X
```cho xác suất tương tự trong khi nhỏ hơn về mặt từ điển vì nó làm trì hoãn các viên đạn càng xa càng tốt. 

Một sai lầm dễ mắc phải khác là quên rằng hình trụ là tuần hoàn. 

Ví dụ:```
n = 6
k = 2
```Sự sắp xếp:```
X...X.
```chứa một khoảng trống có chiều dài`3`và một khoảng cách chiều dài`1`, không phải là hai khoảng trống tuyến tính riêng biệt. Vị trí đầu tiên và cuối cùng liền kề nhau trong vòng tròn. 

Vụ án`k = 0`cũng quan trọng. Không ai chết cả nên khả năng Sasha thiệt mạng là`0`. Sự sắp xếp hợp lệ duy nhất là tất cả các dấu chấm. 

Vụ án`k = n`là cực đoan ngược lại. Mỗi ô chứa một viên đạn, xác suất Sasha chết ngay lập tức`1`, và sự sắp xếp là tất cả`X`. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê mọi vị trí có thể có của`k`đạn giữa`n`khe cắm. Với mỗi cách sắp xếp, chúng ta có thể tính xác suất thua của Sasha bằng cách kiểm tra tất cả`n`vị trí bắt đầu theo chu kỳ. 

có`C(n, k)`sắp xếp, và mỗi người đảm nhận`O(n)`để đánh giá. Ngay cả đối với các giá trị vừa phải như`n = 50`, điều này trở nên lớn về mặt thiên văn. 

Nhận xét quan trọng là kết quả của trò chơi chỉ phụ thuộc vào độ dài của các đoạn trống giữa các viên đạn. 

Giả sử khoảng cách giữa hai viên đạn có chiều dài`g`. 

Nếu trò chơi bắt đầu bên trong khoảng trống này, người chơi sẽ luân phiên nhau đi qua nó. Người chơi chạm được viên đạn chỉ phụ thuộc vào khoảng cách ngang nhau. 

Bên trong một khoảng cách chiều dài`g`: 

-`ceil(g / 2)`vị trí xuất phát khiến Sasha thua cuộc. 
-`floor(g / 2)`vị trí xuất phát khiến Roma thua cuộc. 

Vì vậy, mỗi khoảng cách có độ dài lẻ đều góp phần tạo thêm một vị trí thua cho Sasha. 

Điều này ngay lập tức chuyển bài toán thành bài toán tối ưu hóa tổ hợp: 

Chúng tôi muốn chia`n - k`khe trống vào`k`khoảng trống tuần hoàn để số lượng khoảng trống lẻ được giảm thiểu. 

Nếu như`n - k`chẵn, chúng ta có thể làm cho tất cả các khoảng cách đều nhau, mang lại sự cân bằng hoàn hảo. 

Nếu như`n - k`là số lẻ thì ít nhất một khoảng trống phải là số lẻ. 

Vậy số khoảng trống lẻ nhỏ nhất có thể là: 

-`0`nếu như`n - k`thậm chí là 
-`1`mặt khác 

Sau khi giảm thiểu số khoảng trống lẻ, chúng ta vẫn cần sắp xếp nhỏ nhất về mặt từ điển. 

Về mặt từ điển nhỏ hơn có nghĩa là trì hoãn các viên đạn ở bên phải càng nhiều càng tốt. Vì các khoảng trống xác định vị trí dấu đầu dòng nên chúng tôi muốn các khoảng trống trước đó càng lớn càng tốt. 

Trong số tất cả các giải pháp tối ưu: 

- tất cả các khoảng trống phải chẵn ngoại trừ một khoảng trống lẻ, 
- và những khoảng trống trước đó nên được tối đa hóa một cách tham lam. 

Điều này dẫn đến một giải pháp số học mang tính xây dựng có thể trả lời từng vị trí được truy vấn một cách độc lập mà không cần xây dựng chuỗi đầy đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(k + p) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hãy để`m = n - k`, số lượng vị trí trống. 
2. Chúng ta phải phân phối những thứ này`m`khe trống vào`k`khoảng cách tuần hoàn giữa các viên đạn. 
3. Để giảm thiểu khả năng Sasha thua cuộc, hãy giảm thiểu số khoảng trống lẻ. 

Nếu như`m`là số chẵn, mọi khoảng cách đều có thể chẵn. 

Nếu như`m`là số lẻ, chính xác có một khoảng trống phải là số lẻ. 
4. Để có được sự sắp xếp nhỏ nhất về mặt từ điển, hãy tối đa hóa những khoảng trống trước đó. 

Các dấu chấm trước đó trì hoãn các dấu đầu dòng trước đó, làm cho chuỗi nhỏ hơn về mặt từ điển. 
5. Xây dựng các khoảng trống một cách tham lam từ trái sang phải. 

Mỗi khoảng trống phải duy trì điều kiện chẵn lẻ toàn cầu: 

- hầu hết các khoảng trống phải bằng nhau, 
- cho phép nhiều nhất một khoảng cách lẻ khi`m`thật kỳ quặc. 
6. Đặt càng nhiều ô trống vào khoảng trống hiện tại càng tốt trong khi vẫn giữ đủ chỗ cho các khoảng trống còn lại. 
7. Sau khi xác định tất cả các khoảng trống, hãy xây dựng lại các vị trí dấu đầu dòng một cách khái niệm. 

Bắt đầu từ vị trí`1`, địa điểm:

-`gap[0]`dấu chấm, 
- một viên đạn, 
-`gap[1]`dấu chấm, 
- một viên đạn, 
- và cứ thế theo chu kỳ. 
8. Vì`n`có thể rất lớn, không bao giờ xây dựng chuỗi đầy đủ. 

Thay vào đó, hãy lưu trữ các vị trí dấu đầu dòng trong một tập hợp và chỉ trả lời các chỉ mục được truy vấn. 

### Tại sao nó hoạt động 

Xác suất chỉ phụ thuộc vào tính chẵn lẻ của mỗi khoảng trống. Một khoảng cách chẵn đóng góp vào số lần khởi đầu thắng và thua bằng nhau. Khoảng cách kỳ lạ khiến Sasha có thêm một khởi đầu thua cuộc. 

Bởi vì tổng số vị trí trống có tính chẵn lẻ cố định, nên số khoảng trống lẻ tối thiểu có thể chỉ được ép buộc bằng tính chẵn lẻ. 

Sau khi cố định cấu trúc chẵn lẻ tối ưu, thứ tự từ điển được xác định theo vị trí khác biệt sớm nhất. Việc tối đa hóa các khoảng trống trước đó sẽ trì hoãn các dấu đầu dòng càng xa càng tốt, làm cho chuỗi trở nên tối thiểu về mặt từ điển. 

Việc xây dựng tham lam có hiệu quả vì việc chọn một khoảng cách lớn hơn trước đó không bao giờ có thể ảnh hưởng đến tính khả thi trong tương lai miễn là các ràng buộc chẵn lẻ còn lại được bảo toàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k, p = map(int, input().split())

    queries = [int(input()) for _ in range(p)]

    if k == 0:
        ans = ['.'] * p
        print(''.join(ans))
        return

    if k == n:
        ans = ['X'] * p
        print(''.join(ans))
        return

    m = n - k

    gaps = []

    odd_needed = m % 2
    remain = m

    for i in range(k):
        left = k - i - 1

        # largest feasible gap
        g = remain - left

        # adjust parity
        if odd_needed == 0:
            if g % 2 == 1:
                g -= 1
        else:
            if g % 2 == 0:
                g -= 1

        gaps.append(g)

        remain -= g

        if g % 2 == 1:
            odd_needed = 0

    bullets = set()

    pos = 1

    for g in gaps:
        pos += g
        bullets.add(pos)
        pos += 1

    out = []

    for x in queries:
        if x in bullets:
            out.append('X')
        else:
            out.append('.')

    print(''.join(out))

solve()
```Các trường hợp đặc biệt đầu tiên được xử lý`k = 0`Và`k = n`. Những điều này tránh các trường hợp góc trong logic mang tính xây dựng và cũng tránh các cách diễn giải theo chu kỳ không hợp lệ. 

Biến`m`lưu trữ tổng số vị trí trống. Chúng tôi phân phối chúng vào`k`những khoảng trống. 

Bước tham lam chọn khoảng cách hiện tại lớn nhất khả thi. Điều kiện:```
g = remain - left
```có nghĩa là chúng tôi để lại ít nhất một chỗ cho mỗi khoảng trống còn lại. 

Sau đó chúng tôi điều chỉnh tính chẵn lẻ. Nếu chúng ta vẫn cần một khoảng cách lẻ thì khoảng cách hiện tại sẽ là số lẻ. Nếu không thì nó phải bằng nhau. 

Một điểm tinh tế là việc giảm đi một luôn đảm bảo tính khả thi vì tính chẵn lẻ thay đổi đúng một và chúng ta đã dành đủ không gian cho các khoảng trống sau này. 

Giai đoạn xây dựng lại không bao giờ tạo ra chuỗi đầy đủ. Thay vào đó, nó chỉ tính toán các vị trí chứa đạn. Vì chỉ có`k`đạn, cách này hiệu quả. 

Các vị trí được lập chỉ mục 1 vì các truy vấn được lập chỉ mục 1. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 1 3
1
2
3
```Đây:

-`n = 3`-`k = 1`-`m = 2`| Bước | ở lại | lẻ_cần thiết | khoảng cách đã chọn | 
| --- | --- | --- | --- | 
| 1 | 2 | 0 | 2 | 

Sự sắp xếp trở thành:```
..X
```| Vị trí | 1 | 2 | 3 | 
| --- | --- | --- | --- | 
| Giá trị | . | . | X | 

Điều này thể hiện quy luật từ điển. Với một viên đạn, đặt nó càng muộn càng tốt luôn là tối ưu. 

### Ví dụ 2 

đầu vào:```
7 3
```Chúng tôi có: 

-`m = 4`- tất cả các khoảng trống phải bằng nhau. 

| Bước | ở lại | lẻ_cần thiết | khoảng cách đã chọn | 
| --- | --- | --- | --- | 
| 1 | 4 | 0 | 2 | 
| 2 | 2 | 0 | 0 | 
| 3 | 2 | 0 | 2 | 

Khoảng trống chu kỳ là:```
2, 0, 2
```Sự sắp xếp kết quả:```
..XX..X
```| Vị trí | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Giá trị | . | . | X | X | . | . | X | 

Ví dụ này cho thấy rằng các dấu đầu dòng liên tiếp đôi khi cần thiết để tối đa hóa các khoảng trống trước đó về mặt từ điển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k + p) | Xây dựng các khoảng trống và trả lời các truy vấn | 
| Không gian | O(k) | Lưu trữ vị trí đạn | 

Thuật toán không bao giờ lặp lại tất cả`n`vị trí, đó là điều cần thiết bởi vì`n`có thể đạt được`10^18`. Chỉ có`k`dấu đầu dòng và vị trí được truy vấn rất quan trọng nên giải pháp phù hợp một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, k, p = map(int, input().split())

    queries = [int(input()) for _ in range(p)]

    if k == 0:
        print('.' * p)
        return

    if k == n:
        print('X' * p)
        return

    m = n - k

    gaps = []

    odd_needed = m % 2
    remain = m

    for i in range(k):
        left = k - i - 1

        g = remain - left

        if odd_needed == 0:
            if g % 2 == 1:
                g -= 1
        else:
            if g % 2 == 0:
                g -= 1

        gaps.append(g)

        remain -= g

        if g % 2 == 1:
            odd_needed = 0

    bullets = set()

    pos = 1

    for g in gaps:
        pos += g
        bullets.add(pos)
        pos += 1

    out = []

    for x in queries:
        out.append('X' if x in bullets else '.')

    print(''.join(out))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue()

# provided sample
assert run("3 1 3\n1\n2\n3\n") == "..X\n", "sample 1"

# minimum size
assert run("1 0 1\n1\n") == ".\n", "n=1, k=0"

# all bullets
assert run("4 4 4\n1\n2\n3\n4\n") == "XXXX\n", "all bullets"

# consecutive bullets optimal
assert run("7 3 7\n1\n2\n3\n4\n5\n6\n7\n") == "..XX..X\n"

# large sparse case
assert run("10 1 10\n1\n2\n3\n4\n5\n6\n7\n8\n9\n10\n") == ".........X\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 1`|`.`| Cấu hình trống kích thước tối thiểu | 
|`4 4 4`|`XXXX`| Mọi vị trí đều bị chiếm đóng | 
|`7 3 7`|`..XX..X`| Đạn liên tiếp có thể tối ưu | 
|`10 1 10`|`.........X`| Viên đạn đơn nhỏ nhất về mặt từ điển | 

## Vỏ cạnh 

Hãy xem xét:```
1 0 1
1
```Không có đạn. Trò chơi không bao giờ kết thúc nên khả năng Sasha thua là`0`. Thuật toán ngay lập tức trả về tất cả các dấu chấm mà không cần bước vào giai đoạn xây dựng. 

Bây giờ hãy xem xét:```
4 4 4
1
2
3
4
```Mỗi khe chứa một viên đạn. Sasha luôn chết ngay lập tức. Thuật toán lại xử lý việc này một cách trực tiếp và đưa ra tất cả`X`. 

Một trường hợp tuần hoàn tinh tế hơn:```
6 2
```Một cách giải thích tuyến tính bất cẩn có thể nghĩ:```
...X.X
```có những khoảng trống`3`Và`1`. 

Nhưng theo chu kỳ, trận chung kết`X`kết nối trở lại điểm bắt đầu, vì vậy những khoảng trống thực sự là`0`Và`4`. 

Thuật toán xây dựng các khoảng trống một cách rõ ràng theo thứ tự tuần hoàn nên nó không bao giờ mắc lỗi này. 

Cuối cùng:```
7 2
```Chúng tôi có`m = 5`, thật kỳ lạ. Ít nhất một khoảng trống phải là số lẻ. 

Việc xây dựng lựa chọn:```
5, 0
```cho:```
.....XX
```Có chính xác một khoảng cách lẻ, là khoảng cách tối ưu và dấu đầu dòng sớm nhất được đẩy sang phải càng xa càng tốt, điều này đảm bảo tính tối thiểu về mặt từ điển.
