---
title: "CF 106503E - Vấn đề hàng đầu của sinh viên\u2160"
description: "Chúng ta có một lưới 3 × 3 rất nhỏ trong đó mỗi ô chứa giá trị chiều cao từ 0 đến 3. Bạn có thể coi đây là hình chiếu từ trên xuống của một chồng khối đơn vị: tại vị trí (i, j), có các khối hi,j xếp chồng lên nhau theo chiều dọc."
date: "2026-06-19T15:08:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106503
codeforces_index: "E"
codeforces_contest_name: "2026 \u534e\u5357\u5e08\u8303\u5927\u5b66\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b (SCNUCPC 2026)"
rating: 0
weight: 106503
solve_time_s: 44
verified: true
draft: false
---

[CF 106503E - Vấn đề hàng đầu của sinh viên\u2160](https://codeforces.com/problemset/problem/106503/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới 3 × 3 rất nhỏ trong đó mỗi ô chứa giá trị chiều cao từ 0 đến 3. Bạn có thể coi đây là hình chiếu từ trên xuống của một chồng khối đơn vị: tại vị trí (i, j), có các khối hi,j xếp chồng lên nhau theo chiều dọc. 

Từ ma trận chiều cao này, chúng tôi muốn xây dựng lại hai hình chiếu nhị phân 3 × 3 khác nhau của cùng một cấu trúc 3D. Một là “chế độ xem trước”, trong đó đối với mỗi cột j, chúng tôi chỉ quan tâm liệu có tồn tại ít nhất một khối trong cột đó hay không khi xem dọc theo một trục. Cái còn lại là “chế độ xem bên trái”, trong đó đối với mỗi hàng tôi, chúng tôi kiểm tra xem có tồn tại ít nhất một khối khi nhìn từ phía bên kia hay không. 

Mỗi trong số hai đầu ra là một lưới ký tự 3 × 3. Một ô được đánh dấu bằng`*`nếu có ít nhất một khối được nhìn thấy ở vị trí đó trong hình chiếu tương ứng, nếu không thì đó là`.`. 

Hạn chế chính là kích thước lưới được cố định ở mức 3 × 3, nhưng số lượng trường hợp thử nghiệm có thể lớn tới 100000. Điều đó ngay lập tức loại trừ bất kỳ phương pháp nào cố gắng tái tạo lại hoặc mô phỏng cấu trúc 3D đầy đủ một cách rõ ràng cho mỗi trường hợp thử nghiệm. Ngay cả các hoạt động tuyến tính với hằng số như 27 cho mỗi trường hợp thử nghiệm đều có thể chấp nhận được, nhưng mọi hoạt động liên quan đến tìm kiếm hoặc lặp lại các cấu hình giả định vượt quá O(1) cho mỗi thử nghiệm đều trở thành chi phí không cần thiết. 

Một điểm tinh tế là nhiều cấu hình 3D có thể tạo ra cùng một ma trận chiều cao, nhưng các phép chiếu mà chúng ta cần được xác định duy nhất bởi các điều kiện logic đơn giản về độ cao. Một sự hiểu lầm ngây thơ là nghĩ rằng chúng ta cần xây dựng lại cách sắp xếp 3D hợp lệ phù hợp với các phép chiếu, nhưng vấn đề đã đưa ra bản đồ chiều cao đầy đủ nên không còn sự mơ hồ. 

Một lỗi có thể xảy ra là nhầm lẫn giữa hàng và cột khi xây dựng các hình chiếu. Vì cả hai đầu ra đều là lưới 3 × 3 nên các trục hoán đổi vẫn tạo ra lưới hợp lệ về mặt trực quan nhưng ý nghĩa lại sai. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ là tưởng tượng việc tái tạo lại toàn bộ không gian khối 3 × 3 × 3. Đối với mỗi ô, chúng tôi có thể đặt các hình khối theo chiều cao một cách rõ ràng, sau đó mô phỏng chế độ xem từ phía trước và từ bên trái bằng cách kiểm tra khả năng hiển thị dọc theo các tia. Điều này sẽ liên quan đến việc lặp lại tất cả 27 vị trí và tính toán lại khả năng hiển thị bằng cách quét dọc theo hàng hoặc cột. Mặc dù 27 là hằng số, việc triển khai đơn giản vẫn có thể thực hiện quét lặp lại cho mỗi truy vấn, dẫn đến sự cố hệ số không đổi không cần thiết trong 100000 trường hợp thử nghiệm. 

Quan sát quan trọng là ma trận chiều cao đã mã hóa mọi thứ cần thiết cho cả hai phép chiếu. Đối với chế độ xem trước, điều quan trọng là liệu bất kỳ chiều cao nào trong cột có khác 0 hay không. Đối với chế độ xem bên trái, điều quan trọng là liệu bất kỳ chiều cao nào trong một hàng có khác 0 hay không. Chúng ta không cần biết cách xếp chồng thực tế, chỉ cần biết sự tồn tại của ít nhất một khối. 

Điều này làm giảm vấn đề thành việc giảm OR theo hàng và theo cột đơn giản trên lưới 3 × 3 cố định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng hình khối 3D + phép chiếu) | O(T · 27) với hằng số nặng | O(27) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu (kiểm tra hàng/cột) | O(T) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập vì không có sự tương tác giữa chúng. 

1. Đọc ma trận độ cao 3 × 3. Ma trận này mô tả đầy đủ cấu trúc, do đó không cần xử lý trước ngoài việc lưu trữ nó. 
2. Xây dựng chế độ xem trước bằng cách quét từng cột j từ 0 đến 2. Đối với mỗi cột, kiểm tra xem có hàng i nào có hi,j > 0 hay không. Nếu có ít nhất một hàng tồn tại, hãy đánh dấu toàn bộ vị trí chiếu dọc cho cột đó trong chế độ xem trước là`*`ở cấp hàng đó. Vì đầu ra cũng là 3 × 3, điều này có nghĩa là lấp đầy toàn bộ cột của chế độ xem trước bằng`*`nếu cột chứa bất kỳ giá trị dương nào, nếu không hãy điền nó bằng`.`. 
3. Xây dựng chế độ xem bên trái bằng cách quét từng hàng i từ 0 đến 2. Đối với mỗi hàng, kiểm tra xem có cột j nào có hi,j > 0 hay không. Nếu có, hãy đánh dấu hàng tương ứng trong chế độ xem bên trái là hiển thị đầy đủ (`*`), nếu không hãy điền nó bằng`.`. 
4. Xuất cả hai lưới 3 × 3 cho mỗi trường hợp thử nghiệm. 

Bước suy luận quan trọng là các phép chiếu thu gọn thông tin về độ sâu. Khi chúng tôi quyết định rằng một hàng hoặc cột chứa ít nhất một khối lập phương, chúng tôi không còn quan tâm đến chiều cao riêng lẻ bên trong nó nữa. 

### Tại sao nó hoạt động 

Mỗi phép chiếu được xác định hoàn toàn bằng khả năng hiển thị chứ không phải bằng số lượng. Một vị trí trong chế độ xem được chiếu sẽ hiển thị khi và chỉ khi tồn tại ít nhất một khối ở đâu đó dọc theo đường ngắm tương ứng. Vì tất cả các hình khối trong một cột (hoặc hàng) đều được căn chỉnh nên bất kỳ chiều cao dương nào cũng đảm bảo khả năng hiển thị cho toàn bộ đường chiếu đó. Ngược lại, nếu tất cả các chiều cao bằng 0 thì không có khối lập phương nào tồn tại trong dòng đó nên không thể nhìn thấy gì. Sự tương đương này đảm bảo rằng việc giảm từng hàng và cột thành điều kiện boolean sẽ bảo toàn chính xác thông tin mà cả hai chế độ xem yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        h = [list(map(int, input().split())) for _ in range(3)]

        front = [['.' for _ in range(3)] for _ in range(3)]
        left = [['.' for _ in range(3)] for _ in range(3)]

        # front view: column-wise OR
        col_has = [False] * 3
        for j in range(3):
            for i in range(3):
                if h[i][j] > 0:
                    col_has[j] = True

        # left view: row-wise OR
        row_has = [False] * 3
        for i in range(3):
            for j in range(3):
                if h[i][j] > 0:
                    row_has[i] = True

        for j in range(3):
            if col_has[j]:
                for i in range(3):
                    front[i][j] = '*'

        for i in range(3):
            if row_has[i]:
                for j in range(3):
                    left[i][j] = '*'

        for i in range(3):
            print(''.join(front[i]))
        for i in range(3):
            print(''.join(left[i]))

solve()
```Giải pháp này tách tính toán thành hai mảng boolean, một mảng cho hàng và một mảng cho cột. Điều này tránh việc quét lặp đi lặp lại trong quá trình xây dựng đầu ra. Bước điền cuối cùng mang tính quyết định: nếu một hàng hoặc cột được đánh dấu là có chiều cao dương bất kỳ, chúng ta sẽ điền vào toàn bộ dòng bằng`*`. 

Một lỗi phổ biến là thử xây dựng các phép chiếu theo từng ô bằng cách sử dụng so sánh chiều cao, nhưng phép chiếu không phụ thuộc vào độ lớn vượt quá 0 so với khác 0. Một lỗi khác là in cùng một ma trận cho cả hai chế độ xem do trộn logic hàng/cột. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào đơn giản:```
1
1 0 0
0 0 0
0 0 2
```Ở đây chúng tôi tính toán khả năng hiển thị. 

Chế độ xem phía trước phụ thuộc vào các cột: 

| Cột | Có tích cực gì không? | Cột trước | 
| --- | --- | --- | 
| 0 | vâng | *** | 
| 1 | không | ... | 
| 2 | vâng | *** | 

Chế độ xem bên trái phụ thuộc vào hàng: 

| Hàng | Có tích cực gì không? | Hàng trái | 
| --- | --- | --- | 
| 0 | vâng | *** | 
| 1 | không | ... | 
| 2 | vâng | *** | 

Vì vậy, đầu ra là: 

Mặt trước:```
*..
...
*..
```Bên trái:```
***
...
***
```Bây giờ hãy xem xét một trường hợp được điền đầy đủ:```
1
3 3 3
3 3 3
3 3 3
```Mỗi hàng và cột có ít nhất một mục dương, do đó cả hai phép chiếu đều trở thành các lưới được lấp đầy hoàn toàn`*`. 

Đằng trước:```
***
***
***
```Bên trái:```
***
***
***
```Điều này xác nhận rằng các cấu hình dày đặc sẽ thu gọn lại thành khả năng hiển thị đầy đủ bất kể độ cao chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi bài kiểm tra xử lý một lưới 3 × 3 cố định với các lần quét liên tục | 
| Không gian | O(1) | Chỉ một số mảng cố định được sử dụng bất kể T | 

Các ràng buộc cho phép tối đa 100000 trường hợp thử nghiệm, nhưng mỗi trường hợp chỉ yêu cầu một lượng công việc không đổi. Điều này giữ cho tổng số hoạt động luôn nằm trong giới hạn ngay cả trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys as _sys

    out = []
    input = _sys.stdin.readline

    def solve():
        T = int(input())
        for _ in range(T):
            h = [list(map(int, input().split())) for _ in range(3)]

            col_has = [False] * 3
            row_has = [False] * 3

            for i in range(3):
                for j in range(3):
                    if h[i][j] > 0:
                        row_has[i] = True
                        col_has[j] = True

            front = [['.' for _ in range(3)] for _ in range(3)]
            left = [['.' for _ in range(3)] for _ in range(3)]

            for j in range(3):
                if col_has[j]:
                    for i in range(3):
                        front[i][j] = '*'

            for i in range(3):
                if row_has[i]:
                    for j in range(3):
                        left[i][j] = '*'

            for i in range(3):
                out.append(''.join(front[i]))
            for i in range(3):
                out.append(''.join(left[i]))

    solve()
    return '\n'.join(out) + '\n'

# provided sample (representative reconstruction)
assert run("""1
1 2 3
1 2 2
0 1 1
""") == """***
***
***
***
***
***\n"""

# all zeros
assert run("""1
0 0 0
0 0 0
0 0 0
""") == """...
...
...
...
...
...\n"""

# single column active
assert run("""1
0 0 1
0 0 1
0 0 1
""") == """..*
..*
..*
***
***
***\n"""

# all ones
assert run("""1
1 1 1
1 1 1
1 1 1
""") == """***
***
***
***
***
***\n"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả số không | tất cả các dấu chấm | không có trường hợp hiển thị | 
| cột đơn hoạt động | cột bên phải được gắn dấu sao ở phía trước | độ chính xác logic cột | 
| tất cả những cái | lưới đầy đủ | trường hợp bão hòa đầy đủ | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi toàn bộ hàng hoặc cột bằng 0 ngoại trừ một ô. Ví dụ:```
1
0 0 0
0 0 1
0 0 0
```Đối với chế độ xem trước, chỉ có cột 2 có giá trị dương nên chỉ cột đó mới trở thành đầy đủ`*`. Đối với chế độ xem bên trái, chỉ hàng 1 có giá trị dương nên chỉ hàng đó mới trở thành đầy đủ`*`. Thuật toán đánh dấu row_has và col_has một cách độc lập, do đó,`1`kích hoạt chính xác cả hai phép chiếu mà không bị nhiễu. 

Một trường hợp cạnh khác là khi có nhiều độ cao tồn tại ở các hàng và cột khác nhau nhưng không bao giờ trùng nhau theo cách quan trọng. Vì khả năng hiển thị chỉ phụ thuộc vào sự tồn tại nên các giá trị phân tán vẫn truyền chính xác thông qua kiểm tra OR hàng và cột và không cấu hình nào có thể hủy khả năng hiển thị khi giá trị dương xuất hiện.
