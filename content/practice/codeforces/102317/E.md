---
title: "CF 102317E - Tìm kiếm từ lặp"
description: "Chúng tôi có một số câu đố tìm kiếm từ độc lập. Mỗi câu đố bao gồm một lưới r × c gồm các chữ cái viết hoa và một danh sách các từ. Một từ được hình thành bằng cách bắt đầu từ một ô lưới và liên tục di chuyển theo đúng một trong bốn hướng chính, phải, trái, xuống hoặc lên."
date: "2026-08-16T18:54:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102317
codeforces_index: "E"
codeforces_contest_name: "UCF Locals 2016"
rating: 0
weight: 102317
solve_time_s: 301
verified: true
draft: false
---

[CF 102317E - Tìm kiếm từ lặp lại](https://codeforces.com/problemset/problem/102317/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 1 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một số câu đố tìm kiếm từ độc lập. Mỗi câu đố bao gồm một`r × c`lưới các chữ cái viết hoa và một danh sách các từ. Một từ được hình thành bằng cách bắt đầu từ một ô lưới và liên tục di chuyển theo đúng một trong bốn hướng chính, phải, trái, xuống hoặc lên. Lưới có tính tuần hoàn, do đó việc di chuyển qua cạnh phải sẽ tiếp tục ở cạnh trái, việc di chuyển qua phần dưới cùng sẽ tiếp tục ở phần trên cùng và cách bao bọc tương tự cũng được áp dụng cho hai hướng còn lại. Do đó, một từ có thể sử dụng cùng một ô vật lý nhiều lần. 

Đối với mỗi từ được yêu cầu, chúng tôi phải báo cáo hàng và cột của chữ cái đầu tiên và hướng mà phần còn lại của từ đó tiếp tục. Đầu vào đảm bảo rằng mỗi từ xuất hiện chính xác một lần và không có từ nào là palindrome, do đó không có sự mơ hồ giữa hai hướng đối diện của cùng một chuỗi. 

Lưới có từ 3 đến 12 hàng và từ 3 đến 20 cột, trong khi mỗi từ có từ 3 đến 100 ký tự. Những kích thước này là nhỏ có chủ ý. Ngay cả việc kiểm tra từng ô, từng hướng chính, từng ký tự của một từ cũng chỉ là so sánh vài triệu ký tự cho một câu đố điển hình. Không cần đến những máy móc nối chuỗi phức tạp như Aho-Corasick hoặc các cấu trúc hậu tố. Mối quan tâm triển khai chính là tính chính xác xung quanh ranh giới tuần hoàn, bởi vì một từ có thể tiếp tục đi qua một cạnh và có thể bao bọc nhiều lần khi độ dài của nó vượt quá kích thước hàng hoặc cột. 

Trường hợp cạnh đầu tiên bao quanh một ranh giới nằm ngang. Coi như```
3 3
ABC
DEF
GHI
1
BCA
```Từ bắt đầu ở hàng 1, cột 2 và tiếp tục sang phải. Sau đó`C`, quá trình tìm kiếm kết thúc ở`A`, vậy vị trí đúng là hàng 1, cột 2 hướng sang phải. Việc triển khai dừng khi cột đạt tới 3 sẽ từ chối nó một cách không chính xác. 

Trường hợp cạnh thứ hai được gói nhiều lần. Coi như```
3 3
ABC
DEF
GHI
1
BCABC
```Từ bắt đầu ở hàng 1, cột 2 và sang phải. Các ký tự của nó là`B C A B C`, do đó việc tìm kiếm vượt qua ranh giới bên phải hai lần. Việc triển khai bất cẩn chỉ xử lý một gói sẽ thất bại trong trường hợp này. 

Trường hợp cạnh thứ ba là một từ sử dụng lại cùng một ô. Với```
3 3
ABC
DEF
GHI
1
BCABCABC
```từ lại bắt đầu ở hàng 1, cột 2 và di chuyển sang phải. Không có lệnh cấm truy cập lại một ô, vì vậy tìm kiếm dựa trên tập hợp đã truy cập sẽ giải quyết được một vấn đề khác và có thể từ chối một lần xuất hiện hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi ô bắt đầu có thể và mọi hướng chính có thể. Khi một ứng cử viên được chọn, hãy so sánh từng ký tự của từ trong khi di chuyển theo hướng đó. Bởi vì lưới có tính tuần hoàn nên hàng và cột tiếp theo được lấy bằng số học mô-đun. Nếu tất cả các ký tự đều khớp thì ứng cử viên là câu trả lời duy nhất được đảm bảo bởi dữ liệu đầu vào. 

Phương pháp vũ phu này đã đủ cho các ràng buộc thực tế. Trong trường hợp xấu nhất, đối với một từ chúng tôi kiểm tra`r × c`vị trí bắt đầu, bốn hướng và tối đa 100 ký tự. Với kích thước lưới tối đa là`12 × 20`, đó là nhiều nhất`12 × 20 × 4 × 100 = 96,000`so sánh ký tự mỗi từ. Ngay cả với vài chục từ, tổng số vẫn nằm trong giới hạn một giây. 

Quan sát hữu ích là lưới rất nhỏ và quy tắc chuyển động chỉ có bốn khả năng. Không có sự phân nhánh trong quá trình kiểm tra ứng viên. Khi ô bắt đầu và hướng được cố định, toàn bộ chuỗi ô sẽ được xác định. Điều đó có nghĩa là một lần quét xác định đơn giản sẽ mang lại cho chúng tôi cả độ chính xác và hiệu suất có thể dự đoán được. 

Chúng ta có thể làm cho quá trình triển khai trở nên rõ ràng hơn bằng cách chỉ kiểm tra các ô chứa ký tự đầu tiên của từ, sau đó kiểm tra bốn hướng từ các ô đó. Chúng tôi cũng chuẩn hóa mọi nước đi bằng số học modulo. Ví dụ: di chuyển sang trái từ cột`0`trở thành`(0 - 1) % c`, trong Python tự động tạo ra`c - 1`. Điều này trực tiếp mô hình lưới tuần hoàn. 

Phương pháp brute-force hoạt động vì mọi lần xuất hiện hợp lệ đều có chính xác một ô bắt đầu và một hướng. Không cần phải tìm kiếm đường dẫn hoặc duy trì trạng thái đã truy cập. Quan sát tương tự là điều giữ cho việc triển khai ở mức nhỏ và độ phức tạp tuyến tính về số lượng ký tự ứng cử viên được kiểm tra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(s · r · c · L)`|`O(1)`ngoài đầu vào | Đã chấp nhận | 
| Tối ưu |`O(s · r · c · L)`|`O(1)`ngoài đầu vào | Đã chấp nhận | 

Đây`s`là số từ và`L`là độ dài từ tối đa. Hàng thứ hai thể hiện phiên bản thực tế của cùng một thuật toán tiệm cận, với việc kiểm tra ký tự đầu tiên và lập chỉ mục mô-đun được sử dụng để tránh những công việc không cần thiết. 

## Hướng dẫn thuật toán 

1. Đọc số lượng câu đố và đối với mỗi câu đố, hãy đọc kích thước và lưới chữ cái. Lưu trữ lưới dưới dạng danh sách các chuỗi để truy cập`grid[row][column]`là thời gian không đổi. 
2. Đối với mỗi từ, hãy quét tất cả`r × c`tế bào. Một ô chỉ có thể là điểm bắt đầu nếu chữ cái của nó bằng ký tự đầu tiên của từ, vì vậy hãy bỏ qua ngay tất cả các ô khác. 
3. Đối với mỗi ô có thể bắt đầu, hãy thử bốn hướng`(0, 1)`,`(0, -1)`,`(1, 0)`, Và`(-1, 0)`. Chúng đại diện cho phải, trái, xuống và lên tương ứng. 
4. Đối với hướng đã chọn, hãy kiểm tra từng từ một ký tự. Vị trí của nhân vật`k`là`row = (start_row + dr * k) % r`Và`column = (start_col + dc * k) % c`. 

Lập chỉ mục mô-đun là chìa khóa cho phần vòng lặp của vấn đề. Nó xử lý cả việc vượt qua một cạnh và vượt qua cùng một cạnh nhiều lần mà không có trường hợp đặc biệt nào. 
5. Nếu mọi ký tự đều khớp, hãy ghi lại hàng bắt đầu, cột bắt đầu và hướng. Bài toán đảm bảo tính duy nhất nên ứng cử viên thành công đầu tiên là câu trả lời bắt buộc. 
6. Xuất kết quả cho từ đó và tiếp tục với từ tiếp theo. Việc tìm kiếm độc lập với mỗi từ nên không cần phải chuyển thông tin từ tìm kiếm này sang tìm kiếm khác. 

Tính bất biến rất đơn giản: bất cứ khi nào chúng ta kiểm tra một ứng cử viên`(start_row, start_col, direction)`, sau khi xử lý lần đầu`k`ký tự, mỗi ký tự đã được kiểm tra chính xác là ký tự gặp phải khi đi theo hướng đó từ ô bắt đầu cho số lần di chuyển theo chu kỳ tương ứng. Nếu tất cả`L`so sánh thành công, từ hoàn chỉnh xuất hiện ở đó. Vì mọi ô bắt đầu có thể và mọi hướng có thể đều được kiểm tra nên không thể bỏ sót một lần xuất hiện hợp lệ, đồng thời đảm bảo tính duy nhất ngăn cản thuật toán chọn một phương án thay thế sai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    directions = [
        (0, 1, "RIGHT"),
        (0, -1, "LEFT"),
        (1, 0, "DOWN"),
        (-1, 0, "UP"),
    ]

    out = []

    for _ in range(t):
        r, c = map(int, input().split())
        grid = [input().strip() for _ in range(r)]

        s = int(input())

        for _ in range(s):
            word = input().strip()
            found = False

            for sr in range(r):
                if found:
                    break

                for sc in range(c):
                    if grid[sr][sc] != word[0]:
                        continue

                    for dr, dc, name in directions:
                        ok = True

                        for k in range(1, len(word)):
                            nr = (sr + dr * k) % r
                            nc = (sc + dc * k) % c

                            if grid[nr][nc] != word[k]:
                                ok = False
                                break

                        if ok:
                            out.append(f"{sr + 1} {sc + 1} {name}")
                            found = True
                            break

                    if found:
                        break

        # Separate consecutive puzzles if required by the judge format.
        # The original input guarantees every requested word has one answer.

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```các`directions`mảng chứa bốn chuyển động được phép và tên đầu ra của chúng. Việc giữ các vùng đồng bằng hàng và cột cùng với hướng văn bản sẽ ngăn thứ tự tìm kiếm bị ngắt kết nối với đầu ra. 

Việc tìm kiếm bắt đầu ở mọi ô nhưng ngay lập tức loại bỏ các ô có ký tự khác với`word[0]`. Đây là một sự tối ưu hóa nhỏ nhưng quan trọng hơn là nó làm cho ý nghĩa của ứng viên trở nên rõ ràng: mọi ứng viên được kiểm tra đều thực sự bắt đầu bằng chữ cái đầu tiên của từ. 

Vòng lặp bắt đầu lúc`k = 1`bởi vì`k = 0`đã được biết là phù hợp. biểu hiện`(sr + dr * k) % r`kết thúc hàng, trong khi`(sc + dc * k) % c`bao bọc cột. Không có điều kiện biên riêng biệt và điều này đặc biệt hữu ích cho các từ dài hơn kích thước lưới. 

Các tọa độ được chuyển đổi từ các chỉ số Python dựa trên 0 sang tọa độ câu đố dựa trên một khi được in. Không cần xử lý số nguyên đặc biệt vì phép nhân lớn nhất chỉ`100 × 12`hoặc`100 × 20`. 

Mã dừng ngay khi tìm thấy sự xuất hiện duy nhất. các`found`cờ phá vỡ các vòng lặp lồng nhau một cách rõ ràng và ngăn chặn việc vô tình báo cáo ứng cử viên thứ hai. 

## Ví dụ đã hoạt động 

Vì lời nhắc được cung cấp không bao gồm đầu vào và đầu ra mẫu ban đầu nên các dấu vết sau đây sử dụng hai câu đố được xây dựng hợp lệ. 

Đối với ví dụ đầu tiên, hãy xem xét:```
1
3 3
ABC
DEF
GHI
2
BCA
IHG
```Từ đầu tiên bắt đầu ở hàng 1, cột 2 và sang phải. Cái thứ hai bắt đầu ở hàng 3, cột 3 và sang trái. 

| Lời | Bắt đầu | Hướng | Ký tự được kiểm tra | Kết quả | 
| --- | --- | --- | --- | --- | 
|`BCA`|`(1,2)`| QUYỀN |`B C A`| Trận đấu | 
|`IHG`|`(3,3)`| TRÁI |`I H G`| Trận đấu | 

Tìm kiếm đầu tiên đến cột 3 sau khi đọc`C`, sau đó`(1 + 0, 3 + 1) % 3`kết thúc ở cột 1, đưa ra`A`. Từ thứ hai diễn ra bình thường từ phải sang trái. Ví dụ này xác nhận rằng cùng một công thức mô-đun xử lý cả chuyển động thông thường và sự bao bọc. 

Đối với ví dụ thứ hai, sử dụng:```
1
3 3
ABC
DEF
GHI
2
BCABC
FED
```Dấu vết là: 

| Lời | Bắt đầu | Hướng | Vị trí đã ghé thăm | Kết quả | 
| --- | --- | --- | --- | --- | 
|`BCABC`|`(1,2)`| QUYỀN |`(1,2),(1,3),(1,1),(1,2),(1,3)`| Trận đấu | 
|`FED`|`(2,3)`| TRÁI |`(2,3),(2,2),(2,1)`| Trận đấu | 

Từ đầu tiên dài hơn số cột, do đó tìm kiếm sẽ truy cập lại các ô trong cùng một hàng. Điều này chứng tỏ tại sao thuật toán không được dừng lại sau một lần duyệt hết một hàng. Vấn đề cho phép độ dài từ tùy ý lên tới 100, vì vậy việc gói lặp lại là một phần của tìm kiếm thông thường. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(s · r · c · L)`| Đối với mỗi từ, nhiều nhất`r · c`bắt đầu, bốn hướng, và`L`kiểm tra ký tự | 
| Không gian |`O(r · c)`| Lưới được lưu trữ rõ ràng; bản thân tìm kiếm sử dụng không gian bổ sung liên tục | 

Với`r ≤ 12`,`c ≤ 20`, Và`L ≤ 100`, một từ yêu cầu tối đa khoảng 96.000 so sánh ký tự trước khi xem xét tối ưu hóa ký tự đầu tiên. Lưới quá nhỏ nên việc tìm kiếm toàn diện đơn giản dễ dàng phù hợp với giới hạn của cuộc thi và bộ nhớ tìm kiếm bổ sung liên tục giúp việc triển khai trở nên nhẹ nhàng. 

## Trường hợp thử nghiệm```python
# The helper below mirrors the submitted solution while making it callable
# from assertions.

import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        t = int(sys.stdin.readline())
        directions = [
            (0, 1, "RIGHT"),
            (0, -1, "LEFT"),
            (1, 0, "DOWN"),
            (-1, 0, "UP"),
        ]

        out = []

        for _ in range(t):
            r, c = map(int, sys.stdin.readline().split())
            grid = [sys.stdin.readline().strip() for _ in range(r)]
            s = int(sys.stdin.readline())

            for _ in range(s):
                word = sys.stdin.readline().strip()
                found = False

                for sr in range(r):
                    if found:
                        break

                    for sc in range(c):
                        if grid[sr][sc] != word[0]:
                            continue

                        for dr, dc, name in directions:
                            ok = True

                            for k in range(1, len(word)):
                                nr = (sr + dr * k) % r
                                nc = (sc + dc * k) % c

                                if grid[nr][nc] != word[k]:
                                    ok = False
                                    break

                            if ok:
                                out.append(f"{sr + 1} {sc + 1} {name}")
                                found = True
                                break

                        if found:
                            break

        sys.stdout.write("\n".join(out))
        return sys.stdout.getvalue()

    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Sample-style case 1: horizontal wrapping and reverse horizontal search.
assert run(
    """1
3 3
ABC
DEF
GHI
2
BCA
IHG
"""
) == """1 2 RIGHT
3 3 LEFT""", "wrapping directions"

# Sample-style case 2: a word wraps more than once.
assert run(
    """1
3 3
ABC
DEF
GHI
2
BCABC
FED
"""
) == """1 2 RIGHT
2 3 LEFT""", "multiple wrapping"

# Minimum-size grid and all-equal letters.
assert run(
    """1
3 3
AAA
AAA
AAA
1
AAAAAA
"""
) == """1 1 RIGHT""", "minimum grid and repeated cells"

# Boundary case: vertical wrapping.
assert run(
    """1
3 3
ABC
DEF
GHI
1
ADGAD
"""
) == """1 1 DOWN""", "vertical wrapping"

# Long word that repeatedly traverses a row.
assert run(
    """1
3 4
ABCD
EFGH
IJKL
1
DABCDABCD
"""
) == """1 4 RIGHT""", "repeated horizontal wrapping"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`ABC / DEF / GHI`, từ`BCA`,`IHG`|`1 2 RIGHT`,`3 3 LEFT`| Gói ngang và hướng ngược lại | 
|`ABC / DEF / GHI`, từ`BCABC`,`FED`|`1 2 RIGHT`,`2 3 LEFT`| Nhiều hơn một lần bọc và di chuyển ngược thông thường | 
| Tất cả`A`lưới, từ`AAAAAA`|`1 1 RIGHT`| Tái sử dụng các ô giống nhau và lưới nhỏ nhất | 
|`ABC / DEF / GHI`, từ`ADGAD`|`1 1 DOWN`| Gói dọc | 
|`ABCD / EFGH / IJKL`, từ`DABCDABCD`|`1 4 RIGHT`| Một từ dài hơn hàng và lặp đi lặp lại theo chu kỳ | 

## Vỏ cạnh 

Đối với ranh giới ngang, sử dụng```
1
3 3
ABC
DEF
GHI
1
BCA
```Thuật toán xem xét`(1,2)`làm ô bắt đầu vì nó chứa`B`. Để viết đúng hướng, ký tự tiếp theo ở cột 3, cho biết`C`. Ký tự thứ ba là tại`(2 + 1) % 3 = 0`trong lập chỉ mục dựa trên số không, đưa ra`A`. Đầu ra là`1 2 RIGHT`. Không cần chi nhánh có ranh giới cụ thể. 

Để gói lặp lại, hãy sử dụng```
1
3 3
ABC
DEF
GHI
1
BCABC
```Bắt đầu lúc`(1,2)`và di chuyển sang phải sẽ cho chuỗi`B,C,A,B,C`. Biểu thức mô-đun tiếp tục tuần hoàn qua các cột`2,3,1,2,3`trong ký hiệu dựa trên một. Đầu ra vẫn còn`1 2 RIGHT`. 

Để tái sử dụng ô, hãy sử dụng```
1
3 3
AAA
AAA
AAA
1
AAAAAA
```Mỗi vị trí được truy cập đều chứa`A`, do đó từ khớp ngay từ ô đầu tiên. Thuật toán không duy trì mảng đã truy cập vì việc xem lại ngăn xếp được cho phép rõ ràng. Đầu ra là`1 1 RIGHT`. 

Để gói theo chiều dọc, sử dụng```
1
3 3
ABC
DEF
GHI
1
ADGAD
```Bắt đầu từ hàng 1, cột 1 và di chuyển xuống dưới sẽ có`A`,`D`,`G`, sau đó kết thúc trở lại`A`, và cuối cùng`D`. Đầu ra là`1 1 DOWN`. Điều này xác nhận rằng cả hàng và cột đều phải được xử lý theo chu kỳ. 

Đối với một từ dài hơn một hàng hoàn chỉnh, hãy sử dụng```
1
3 4
ABCD
EFGH
IJKL
1
DABCDABCD
```Chữ cái đầu tiên ở hàng 1, cột 4. Di chuyển sang phải sẽ tạo ra`D,A,B,C,D,A,B,C,D`, vượt qua ranh giới hai lần. Thuật toán xử lý tất cả các chuyển đổi này thông qua số học modulo, do đó kết quả là`1 4 RIGHT`.
