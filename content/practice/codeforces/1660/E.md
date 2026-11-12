---
title: "CF 1660E - Ma trận và sự dịch chuyển"
description: "Chúng ta được cung cấp một ma trận vuông nhị phân và chúng ta được phép sắp xếp lại nó bằng cách dịch chuyển các hàng và cột theo chu kỳ. Những thay đổi này không thay đổi giá trị, chúng chỉ xoay vị trí, vì vậy điều quan trọng cuối cùng là cách chúng ta căn chỉnh ma trận trước khi bắt đầu trả tiền để lật bit."
date: "2026-06-10T03:03:38+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1660
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 780 (Div. 3)"
rating: 1600
weight: 1660
solve_time_s: 78
verified: true
draft: false
---

[CF 1660E - Ma trận và sự thay đổi](https://codeforces.com/problemset/problem/1660/E) 

**Đánh giá:** 1600 
**Tags:** vũ lực, thuật toán xây dựng, tham lam, thực hiện 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một ma trận vuông nhị phân và chúng ta được phép sắp xếp lại nó bằng cách dịch chuyển các hàng và cột theo chu kỳ. Những thay đổi này không thay đổi giá trị, chúng chỉ xoay vị trí, vì vậy điều quan trọng cuối cùng là cách chúng ta căn chỉnh ma trận trước khi bắt đầu trả tiền để lật bit. 

Sau khi chọn bất kỳ chuỗi xoay hàng và cột nào, chúng ta được phép lật từng ô riêng lẻ (chuyển 0 thành 1 hoặc 1 thành 0) và mỗi lần lật tốn một đơn vị. Mục tiêu là chuyển đổi ma trận thành ma trận đơn vị, nghĩa là mọi ô chéo phải bằng 1 và mọi ô ngoài đường chéo phải bằng 0. 

Khó khăn chính là chúng ta không buộc phải cố định đường chéo ở vị trí ban đầu. Bằng cách dịch chuyển các hàng và cột theo chu kỳ, chúng ta chọn được sự dịch chuyển theo chu kỳ của chỉ số hàng và sự dịch chuyển theo chu kỳ của chỉ mục cột một cách hiệu quả. Điều này có nghĩa là đường chéo mà chúng ta đang nhắm mục tiêu không cố định nhưng có thể được “di chuyển” so với ma trận ban đầu. 

Mỗi trường hợp thử nghiệm là độc lập. Kích thước ma trận có thể lên tới 2000 và tổng số ô trong các thử nghiệm lên tới 4 triệu, vì vậy mọi giải pháp đều phải tránh tính toán lại khối hoặc lặp đi lặp lại nhiều lần cho mỗi căn chỉnh. 

Một cách tiếp cận đơn giản sẽ thử tất cả các dịch chuyển hàng và tất cả các dịch chuyển cột, đưa ra sự sắp xếp O(n^2). Đối với mỗi lần căn chỉnh, chúng tôi sẽ tính toán số lần lật cần thiết để tạo thành đường chéo và mọi thứ khác bằng 0. Việc này sẽ tốn O(n^3), quá chậm. 

Trường hợp cạnh tinh tế xuất hiện khi ma trận đã có cấu trúc gần với đường chéo dưới một số dịch chuyển nhưng không có dưới các dịch chuyển khác. Ví dụ, trong một ma trận giống như hoán vị, chỉ có một sự sắp xếp theo chu kỳ cụ thể sẽ tạo ra một đường chéo hoàn hảo. Một cách tiếp cận bất cẩn chỉ giả định việc dịch chuyển hàng hoặc chỉ cột là quan trọng sẽ thất bại vì cả hai chiều tương tác. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua các thao tác dịch chuyển thì vấn đề rất đơn giản: chúng ta phải lật từng ô không khớp so với số nhận dạng, tức là chỉ đếm sự khác biệt với ma trận nhận dạng. Điều phức tạp là trước tiên chúng ta có thể xoay các hàng và cột theo chu kỳ. 

Chiến lược brute-force là thử mọi cặp dịch chuyển theo chu kỳ có thể có, một cho hàng và một cho cột. Sau khi áp dụng một cặp dịch chuyển, chúng tôi kiểm tra từng ô và đếm xem cần bao nhiêu lần lật để tạo nên ma trận đơn nhất. Vì có n lựa chọn cho dịch chuyển hàng và n lựa chọn cho dịch chuyển cột và mỗi đánh giá có chi phí O(n^2), nên tổng độ phức tạp sẽ trở thành O(n^4) hoặc O(n^3) tùy thuộc vào mức độ cẩn thận của chúng tôi khi sử dụng lại các phép tính. Dù bằng cách nào, điều này vượt xa giới hạn cho n lên tới 2000. 

Thông tin chi tiết quan trọng là các ca chỉ thay đổi chỉ mục lại các hàng và cột theo chu kỳ. Thay vì nghĩ đến việc di chuyển ma trận, chúng ta có thể nghĩ đến việc chọn đường chéo mục tiêu. Một đường chéo trong ma trận dịch chuyển theo chu kỳ tương ứng với các cặp chỉ số (i, j) trong đó j - i là hằng số modulo n. Vì vậy, mỗi cấu hình ca tương ứng với việc chọn độ lệch đường chéo. 

Khi chúng tôi sửa độ lệch đường chéo d, mọi ô (i, j) được phân loại là trên đường chéo nếu j - i ≡ d (mod n) hoặc ngoài đường chéo. Đối với một d cố định, chúng ta có thể tính chi phí trực tiếp: các ô chéo phải trở thành 1, các ô ngoài đường chéo phải trở thành 0. Chi phí chỉ là tính những điểm không khớp. 

Vì vậy, thay vì thử n^2 cặp dịch chuyển, chúng ta chỉ cần thử n độ lệch đường chéo có thể có. Mỗi lần đánh giá có chi phí O(n^2), cho ra O(n^3). Điều này vẫn còn quá lớn trong trường hợp xấu nhất. 

Chúng tôi giảm hơn nữa bằng cách quan sát thấy rằng mỗi ô đóng góp độc lập cho mỗi lựa chọn theo đường chéo. Nếu A[i][j] bằng 1, nó sẽ hỗ trợ chính xác một độ lệch đường chéo (khi j - i khớp với độ lệch đó). Đối với tất cả các giá trị bù trừ khác, đó là một ô ngoài đường chéo nên bằng 0, do đó, nó sẽ bị phạt nếu có giá trị 1 ở đó. Tương tự, số 0 sẽ bị phạt nếu nó nằm trên đường chéo đã chọn.

Chúng tôi có thể tính toán trước các khoản đóng góp cho mỗi phần bù trong O(n^2): đối với mỗi ô, chúng tôi cập nhật hai cấu trúc tổng hợp, một cấu trúc đếm xem có bao nhiêu số 1 rơi trên mỗi đường chéo và ngầm dẫn đến chi phí ngoài đường chéo. Điều này cho phép tính toán mức bù tốt nhất trong tổng số O(n^2) cho mỗi trường hợp thử nghiệm. 

Điều này làm giảm vấn đề từ tìm kiếm theo vị trí đến tổng hợp các đóng góp cho mỗi lớp đường chéo mô-đun. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Sự thay đổi vũ phu | O(n^4) | O(n^2) | Quá chậm | 
| Kiểm tra trực tiếp tất cả các đường chéo | O(n^3) | O(n^2) | Quá chậm | 
| Tổng hợp đóng góp theo đường chéo | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi điều chỉnh lại vấn đề bằng cách chọn độ lệch đường chéo để giảm thiểu chi phí không khớp. 

1. Tính toán, với mỗi độ lệch đường chéo d có thể có, có bao nhiêu đơn vị rơi trên đường chéo đó. Chúng ta định nghĩa lớp đường chéo theo (j - i mod n). Bước này là cần thiết vì chỉ các ô trên đường chéo đã chọn mới bắt buộc phải bằng 1. 
2. Với mỗi offset d, chúng ta tính số ô đường chéo, chính xác là n. Vậy các ô ngoài đường chéo là n^2 - n. 
3. Đối với d cố định, chi phí được tính như sau: tất cả các vị trí đường chéo chứa 0 phải được lật thành 1 và tất cả các vị trí ngoài đường chéo chứa 1 phải được lật thành 0. Điều này chia chi phí thành hai phần độc lập. 
4. Với mỗi d, hãy tính chi phí bằng cách sử dụng các số đếm được tính toán trước: 

đường chéo không khớp là (n - one_on_diagonal[d]), 

sự không khớp ngoài đường chéo là (tổng_ones - one_on_diagonal[d]). 
5. Tổng chi phí trở thành: 

chi phí[d] = (n - những cái_on_diagonal[d]) + (tổng_ones - những cái_on_diagonal[d]). 
6. Lặp lại tất cả d và lấy chi phí tối thiểu. 

Lý do điều này có tác dụng là vì sự dịch chuyển theo chu kỳ không thay đổi những ô nào có cùng giá trị (j - i mod n). Họ chỉ xoay đường chéo nào được coi là đường chéo chính. Do đó, mọi cấu hình dịch chuyển hợp lệ đều tương ứng chính xác với việc chọn một trong các lớp đường chéo này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t_line = input().strip()
    while t_line == "":
        t_line = input().strip()
    t = int(t_line)

    for _ in range(t):
        line = input().strip()
        while line == "":
            line = input().strip()
        n = int(line)

        grid = []
        total_ones = 0

        for _ in range(n):
            row = input().strip()
            grid.append(row)
            total_ones += row.count('1')

        diag_ones = [0] * n

        for i in range(n):
            row = grid[i]
            for j, ch in enumerate(row):
                if ch == '1':
                    diag_ones[(j - i) % n] += 1

        ans = 10**18

        for d in range(n):
            ones_on_diag = diag_ones[d]
            cost = (n - ones_on_diag) + (total_ones - ones_on_diag)
            ans = min(ans, cost)

        print(ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã tổng hợp tất cả các cái trên mỗi lớp đường chéo bằng cách sử dụng sai phân mô-đun (j - i). Điều này mã hóa hiệu ứng của tất cả các dịch chuyển hàng và cột theo chu kỳ có thể có. Sau đó, nó đánh giá từng lựa chọn đường chéo bằng cách tính toán số lần điều chỉnh cần thiết bên trong và bên ngoài đường chéo. 

Một điểm tinh tế là chúng tôi không bao giờ mô phỏng rõ ràng sự thay đổi. Việc lập chỉ mục modulo đã nắm bắt được tất cả các sắp xếp theo chu kỳ có thể có. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
010
011
100
```Chúng tôi tính toán tổng số đóng góp và đóng góp theo đường chéo. 

| tôi | j | A[i][j] | (j - tôi mod 3) | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 
| 1 | 1 | 1 | 0 | 
| 1 | 2 | 1 | 1 | 
| 2 | 0 | 1 | 1 | 

Vậy diag_ones = [1, 3, 0], tổng_ones = 4. 

Bây giờ đánh giá chi phí: 

Với d = 1, đường chéo có 3 đơn vị, do đó lật chéo = 3 - 3 = 0, lật chéo = 4 - 3 = 1, tổng = 1. 

Đối với các giá trị d khác, chi phí sẽ lớn hơn. 

Điều này xác nhận rằng sự căn chỉnh tốt nhất tương ứng với sự dịch chuyển sao cho đường chéo loại 1 trở thành đường chéo chính. 

### Ví dụ 2 

đầu vào:```
n = 2
10
10
```Tất cả đều ở cột 0. 

| tôi | j | A[i][j] | (j - tôi mod 2) | 
| --- | --- | --- | --- | 
| 0 | 0 | 1 | 0 | 
| 1 | 0 | 1 | 1 | 

Vậy diag_ones = [1, 1], Total_ones = 2. 

Đối với cả hai giá trị d, chi phí = (2 - 1) + (2 - 1) = 2. 

Điều này cho thấy tính đối xứng: không có sự dịch chuyển nào sẽ cải thiện sự căn chỉnh và cả hai đường chéo đều có giá trị như nhau nhưng vẫn yêu cầu hai lần lật. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) mỗi lần kiểm tra | Mỗi ô đóng góp vào một lớp đường chéo, sau đó chúng ta quét n đường chéo | 
| Không gian | O(n) | Chỉ các bộ đếm đường chéo được lưu trữ | 

Tổng độ phức tạp trên tất cả các trường hợp thử nghiệm được giới hạn bởi 4 triệu ô, vì vậy giải pháp này hoạt động thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        t_line = input().strip()
        while t_line == "":
            t_line = input().strip()
        t = int(t_line)

        out = []
        for _ in range(t):
            line = input().strip()
            while line == "":
                line = input().strip()
            n = int(line)

            grid = []
            total_ones = 0
            for _ in range(n):
                row = input().strip()
                grid.append(row)
                total_ones += row.count('1')

            diag = [0] * n
            for i in range(n):
                for j, ch in enumerate(grid[i]):
                    if ch == '1':
                        diag[(j - i) % n] += 1

            ans = 10**18
            for d in range(n):
                ones = diag[d]
                ans = min(ans, (n - ones) + (total_ones - ones))

            out.append(str(ans))

        return "\n".join(out)

    return solve()

# provided samples
assert run("""
4

3
010
011
100

5
00010
00001
10000
01000
00100

2
10
10

4
1111
1011
1111
1111
""".strip()) == """1
0
2
11"""

# custom cases
assert run("""
1
1
0
""".strip()) == "1", "single cell flip"

assert run("""
1
2
00
00
""".strip()) == "2", "all zeros must create one 1 on diagonal"

assert run("""
1
3
111
111
111
""".strip()) == "6", "dense matrix requires clearing off-diagonal ones"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 không | 1 | trường hợp lật tối thiểu | 
| tất cả số không 2x2 | 2 | phải tạo đường chéo | 
| tất cả những cái 3x3 | 6 | chi phí dọn dẹp ngoài đường chéo | 

## Vỏ cạnh 

Ma trận 1x1 tối thiểu không chứa tự do về cấu trúc. Yêu cầu duy nhất là làm cho một ô bằng 1, vì vậy nếu nó bắt đầu bằng 0 thì cần phải lật một lần. Thuật toán xử lý vấn đề này vì chỉ có một lớp đường chéo và việc tính toán giảm xuống mức số lượng không khớp trực tiếp. 

Một ma trận chứa toàn số 0 không có lợi thế về căn chỉnh đường chéo. Mọi lựa chọn theo đường chéo đều tạo ra chi phí như nhau, vì tất cả các vị trí trên đường chéo đều bằng 0 và phải được lật lại. Thuật toán phản ánh điều này vì diag_ones[d] bằng 0 với mọi d và chi phí không đổi. 

Một ma trận chứa đầy những cái đó nhấn mạnh hình phạt ngoài đường chéo. Ngay cả khi đường chéo được chọn để tối đa hóa sự căn chỉnh, tất cả các đường chéo phải được lật đi. Công thức nắm bắt điều này trực tiếp thông qua Total_ones - diag_ones[d], đảm bảo tích lũy chính xác các hiệu chỉnh ngoài đường chéo.
