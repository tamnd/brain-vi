---
title: "CF 106189F - Tetris cổ điển: Tính điểm"
description: "Chúng tôi được cấp nhật ký ghi điểm của một người chơi Tetris. Mỗi mục nhật ký cho chúng ta biết rằng người chơi đã xóa chính xác 1, 2, 3 hoặc 4 dòng cùng một lúc, được biểu thị bằng các chuỗi đơn, đôi, ba và tetris. Người chơi bắt đầu ở cấp độ 18."
date: "2026-06-25T06:48:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106189
codeforces_index: "F"
codeforces_contest_name: "ICPC Central Russia Regional Contest, 2025"
rating: 0
weight: 106189
solve_time_s: 39
verified: true
draft: false
---

[CF 106189F - Tetris cổ điển: Tính điểm](https://codeforces.com/problemset/problem/106189/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp nhật ký ghi điểm của một người chơi Tetris. Mỗi mục nhật ký cho chúng ta biết rằng người chơi đã xóa chính xác 1, 2, 3 hoặc 4 dòng cùng một lúc, được biểu thị bằng các chuỗi`single`,`double`,`triple`, Và`tetris`. 

Người chơi bắt đầu ở cấp độ 18. Điểm được trao cho một lần hoàn thành phụ thuộc vào cấp độ hiện tại. 

Ở cấp độ 18: 

- đơn = 760 điểm 
- gấp đôi = 1900 điểm 
- gấp ba = 5700 điểm 
- tetris = 22800 điểm 

Mức độ tăng lên khi tổng số dòng bị xóa đạt đến ngưỡng nhất định. 

Sự chuyển đổi đầu tiên thật đặc biệt. Người chơi vẫn ở cấp 18 cho đến khi tích lũy được ít nhất 130 đường đã xóa. Khi tổng số đạt 130 trở lên, người chơi sẽ chuyển sang cấp 19. 

Sau đó, cứ thêm 10 dòng bị xóa sẽ tăng cấp thêm một. Đạt 140 dòng là cấp 20, đạt 150 dòng là cấp 21, v.v. 

Mỗi cấp độ trên 18 sẽ tăng giá trị tính điểm bằng cách: 

- +40 cho một đĩa đơn 
- +100 cho gấp đôi 
- +300 cho gấp ba 
- +1200 cho trò chơi tetris 

Nhiệm vụ là xử lý nhật ký, tính điểm cuối cùng và xuất ra cả điểm và cấp độ cuối cùng. 

Số lượng hành động có thể lớn như$10^5$. Điều này ngay lập tức loại trừ bất kỳ mô phỏng nào lặp đi lặp lại qua số lượng dòng hoặc phạm vi cấp độ cho mọi hành động. Chúng ta cần một giải pháp có công việc tỷ lệ thuận với số lượng mục nhật ký. MỘT$O(N)$quét dễ dàng đủ nhanh. 

Một chi tiết tinh tế là cấp độ được sử dụng để chấm điểm một hành động là cấp độ trước khi thêm các dòng mới được xóa từ hành động đó. Nếu một nước đi xóa đủ số dòng để vượt qua một ngưỡng, nước đi đó vẫn ghi điểm theo cấp độ trước đó và chỉ những nước đi trong tương lai mới sử dụng cấp độ cao hơn. 

Hãy xem xét ví dụ này:```
129 cleared lines so far
next action = single
```Đĩa đơn vẫn phải được tính là đĩa đơn cấp 18. Sau hành động, tổng số sẽ là 130 và người chơi tiến lên cấp 19. 

Một lỗi phổ biến là cập nhật số dòng trước, tính toán cấp độ mới và sau đó tính điểm cho hành động. Điều đó sẽ trao thưởng không chính xác điểm cấp 19 cho hành động thực sự kích hoạt quá trình chuyển đổi. 

Một trường hợp khác là một khoảng trống lớn vượt qua nhiều ngưỡng cùng một lúc.```
128 lines
next action = tetris
```Người chơi nhảy tổng cộng từ 128 đến 132 dòng. Hành động vẫn được tính điểm ở cấp 18. Cấp độ đó chỉ trở thành 19 sau đó. 

## Phương pháp tiếp cận 

Một mô phỏng đơn giản xử lý từng hành động một. 

Đối với mỗi hành động, hãy xác định cấp độ hiện tại từ số dòng đã xóa, tra cứu điểm cho hành động đó ở cấp độ đó, thêm số dòng đã xóa tương ứng và tiếp tục. 

Ngay cả một phiên bản mạnh mẽ cũng đã hiệu quả vì chỉ có$10^5$hành động. Sự kém hiệu quả tiềm tàng duy nhất sẽ là việc liên tục tính toán lại cấp độ bằng cách lặp qua các ngưỡng. Nếu chúng tôi quét ngưỡng mọi lúc, chi phí sẽ tăng lên một cách không cần thiết. 

Quan sát chính là mức độ chỉ phụ thuộc vào số dòng được xóa trước hành động hiện tại. Có một công thức trực tiếp. 

Nếu xóa được ít hơn 130 dòng thì cấp độ là 18. 

Mặt khác:$$\text{level} = 19 + \left\lfloor \frac{\text{lines}-130}{10} \right\rfloor$$Khi chúng ta có thể tính toán mức hiện tại trong thời gian không đổi, mọi hành động cũng được xử lý trong thời gian không đổi. 

Để ghi điểm, hãy$$d = \text{level} - 18$$Sau đó, giá trị điểm sẽ trở thành:$$760 + 40d$$cho một người duy nhất,$$1900 + 100d$$cho một đôi,$$5700 + 300d$$cho một bộ ba,$$22800 + 1200d$$cho một trò chơi tetris. 

Toàn bộ vấn đề giảm xuống chỉ còn một lần vượt qua nhật ký. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng ngưỡng Brute Force | O(N) | O(1) | Đã chấp nhận | 
| Tính toán cấp độ trực tiếp tối ưu | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`lines = 0`Và`score = 0`. 
2. Đối với mỗi hành động tính điểm, hãy xác định cấp độ hiện tại từ số dòng đã xóa. 

Nếu như`lines < 130`, cấp độ là 18. Nếu không thì sử dụng:$$19 + (lines - 130) // 10$$3. Tính toán`d = level - 18`. 

Điều này cho chúng ta biết số lần tăng cấp độ đã xảy ra kể từ cấp độ bắt đầu. 
4. Thưởng điểm cho hành động hiện tại theo cấp độ hiện tại. 

Hành động này phải được tính điểm trước khi thêm các dòng mới bị xóa. 
5. Thêm số dòng đã xóa tương ứng:`single -> 1`

`double -> 2`

`triple -> 3`

`tetris -> 4`6. Tiếp tục cho đến khi tất cả các mục nhật ký được xử lý. 
7. Sau khi xử lý toàn bộ nhật ký, hãy tính mức cuối cùng từ tổng số dòng đã xóa cuối cùng bằng cách sử dụng cùng một công thức. 
8. Xuất ra điểm cuối cùng và cấp độ cuối cùng. 

### Tại sao nó hoạt động 

Tại mọi thời điểm, cấp độ trò chơi chỉ được xác định bằng số dòng được xóa trước hành động hiện tại. Quy tắc tính điểm phụ thuộc vào cấp độ đó chứ không phụ thuộc vào số dòng sau hành động. 

Thuật toán duy trì chính xác bất biến này. Trước khi xử lý mỗi mục nhật ký,`lines`lưu trữ số dòng đã xóa trước đó. Mức hiện tại được tính từ giá trị đó, hành động được tính điểm tương ứng và chỉ khi đó các dòng mới bị xóa mới được thêm vào. 

Bởi vì mọi hành động đều được tính điểm bằng cùng một cấp độ mà trò chơi thực sẽ sử dụng và do quá trình chuyển đổi cấp độ được tính trực tiếp từ ngưỡng chính thức nên điểm tích lũy và cấp độ cuối cùng là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def get_level(lines):
    if lines < 130:
        return 18
    return 19 + (lines - 130) // 10

def solve():
    n = int(input())

    score = 0
    lines = 0

    for _ in range(n):
        action = input().strip()

        level = get_level(lines)
        d = level - 18

        if action == "single":
            score += 760 + 40 * d
            lines += 1
        elif action == "double":
            score += 1900 + 100 * d
            lines += 2
        elif action == "triple":
            score += 5700 + 300 * d
            lines += 3
        else:  # tetris
            score += 22800 + 1200 * d
            lines += 4

    final_level = get_level(lines)
    print(score, final_level)

solve()
```chức năng`get_level`mã hóa trực tiếp các quy tắc chuyển đổi cấp độ. Giữ logic này ở một nơi sẽ tránh được sự mâu thuẫn giữa việc tính điểm và tính toán ở cấp độ cuối cùng. 

Chi tiết thực hiện quan trọng là thứ tự các hoạt động. Mức hiện tại được tính toán trước khi cập nhật số dòng. Điều này khớp với hành vi của trò chơi khi một nước đi vượt qua một ngưỡng. 

Các giá trị điểm được thể hiện thông qua các giá trị cấp cơ sở-18 cộng với mức tăng phụ thuộc vào cấp độ. Điều này tránh việc lưu trữ một bảng lớn và phản ánh quy tắc được mô tả trong câu lệnh. 

Số nguyên Python tự động xử lý các giá trị lớn, do đó không có vấn đề tràn ngay cả khi mọi hành động đều là tetris. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
single
double
triple
tetris
```| Hành động | Dòng Trước | Cấp độ | Điểm đạt được | Dòng Sau | Tổng số điểm | 
| --- | --- | --- | --- | --- | --- | 
| độc thân | 0 | 18 | 760 | 1 | 760 | 
| đôi | 1 | 18 | 1900 | 3 | 2660 | 
| gấp ba | 3 | 18 | 5700 | 6 | 8360 | 
| tetris | 6 | 18 | 22800 | 10 | 31160 | 

Cấp độ cuối cùng = 18. 

Đầu ra:```
31160 18
```Dấu vết này cho thấy không có ngưỡng nào bị vượt qua nên mọi hành động đều sử dụng cấp độ bắt đầu. 

### Ví dụ 2 

Ví dụ được xây dựng:```
2
tetris
tetris
```Giả sử người chơi đã có 128 dòng bị xóa trước hai hành động này. Chúng tôi tập trung vào hành vi vượt ngưỡng. 

| Hành động | Dòng Trước | Cấp độ | Điểm đạt được | Dòng Sau | 
| --- | --- | --- | --- | --- | 
| tetris | 128 | 18 | 22800 | 132 | 
| tetris | 132 | 19 | 24000 | 136 | 

Tetris đầu tiên vượt qua 130 dòng nhưng vẫn được tính điểm ở cấp độ 18. Chỉ hành động tiếp theo mới nhận được điểm cấp độ 19. 

Điều này xác nhận bất biến khóa được thuật toán sử dụng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi mục nhật ký được xử lý một lần | 
| Không gian | O(1) | Chỉ có một vài quầy được lưu trữ | 

Với nhiều nhất$10^5$hành động, một lần quét tuyến tính dễ dàng phù hợp trong giới hạn thời gian. Việc sử dụng bộ nhớ không đổi bất kể kích thước đầu vào. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    input_data = io.StringIO(inp)
    output_data = io.StringIO()

    def input():
        return input_data.readline()

    n = int(input())

    def get_level(lines):
        if lines < 130:
            return 18
        return 19 + (lines - 130) // 10

    score = 0
    lines = 0

    for _ in range(n):
        action = input().strip()

        level = get_level(lines)
        d = level - 18

        if action == "single":
            score += 760 + 40 * d
            lines += 1
        elif action == "double":
            score += 1900 + 100 * d
            lines += 2
        elif action == "triple":
            score += 5700 + 300 * d
            lines += 3
        else:
            score += 22800 + 1200 * d
            lines += 4

    output_data.write(f"{score} {get_level(lines)}")
    return output_data.getvalue()

# provided samples
assert run("4\nsingle\ndouble\ntriple\ntetris\n") == "31160 18"
assert run("2\nsingle\nsingle\n") == "1520 18"

# minimum input
assert run("1\nsingle\n") == "760 18"

# exact first threshold
assert run("130\n" + "single\n" * 130) == "98800 19"

# threshold crossed by last action
assert run("33\n" + "tetris\n" * 33) == "752400 19"

# all equal values
assert run("3\ntetris\ntetris\ntetris\n") == "68400 18"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một đĩa đơn | 760 18 | Kích thước tối thiểu | 
| 130 đĩa đơn | 98800 19 | Chuyển chính xác lên cấp 19 | 
| 33 tetris | 752400 19 | Vượt ngưỡng qua nhiều dòng rõ ràng | 
| Ba tetrises | 68400 18 | Hành động giống hệt nhau lặp đi lặp lại | 

## Vỏ cạnh 

Xét trường hợp vượt ngưỡng:```
33
tetris
tetris
...
tetris
```Sau 32 lần tetris, người chơi đã vượt qua 128 dòng và vẫn ở cấp độ 18. Tetris thứ 33 được tính điểm ở cấp độ 18, mang lại 22800 điểm. Chỉ sau khi thêm bốn dòng đã xóa, tổng số mới trở thành 132, nâng cấp người chơi lên cấp 19. 

Một giải pháp cập nhật số dòng trước khi ghi điểm sẽ trao nhầm 24000 điểm cho trò chơi xếp hình cuối cùng đó. 

Bây giờ hãy xem xét ngưỡng chính xác:```
130
single
single
...
single
```Đĩa đơn thứ 130 được tính điểm trong khi người chơi vẫn còn 129 đường bị xóa nên sử dụng tính điểm cấp 18. Sau khi thêm dòng, tổng số sẽ chính xác là 130 và cấp độ cuối cùng trở thành 19. Thuật toán tính toán cấp độ trước khi cập nhật`lines`, do đó nó xử lý ranh giới này một cách chính xác. 

Cuối cùng, hãy xem xét một trò chơi không bao giờ đạt tới 130 dòng:```
2
single
double
```Tổng số dòng chỉ là 3. Mọi hành động được tính điểm ở cấp 18 và cấp cuối cùng vẫn là 18. Công thức nằm chính xác trong phạm vi đặc biệt trước khi chuyển đổi cho tất cả các tính toán.
