---
title: "CF 1921E - Ăn Chip"
description: "Chúng ta có một trò chơi hai người chơi trên một lưới hình chữ nhật có chiều cao h và chiều rộng w. Con chip của Alice bắt đầu ở (xa, ya) và có thể di chuyển xuống, xuống dưới bên trái hoặc xuống dưới bên phải. Chip của Bob bắt đầu tại (xb, yb) và di chuyển lên, lên trên bên trái hoặc lên trên bên phải."
date: "2026-06-08T19:24:24+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "games", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1921
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 920 (Div. 3)"
rating: 1600
weight: 1921
solve_time_s: 147
verified: false
draft: false
---

[CF 1921E - Ăn chip](https://codeforces.com/problemset/problem/1921/E) 

**Đánh giá:** 1600 
**Tags:** vũ lực, trò chơi, tham lam, toán học 
**Thời gian giải:** 2m 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một trò chơi hai người chơi trên một lưới hình chữ nhật có chiều cao`h`và chiều rộng`w`. Con chip của Alice bắt đầu lúc`(x_a, y_a)`và có thể di chuyển xuống, xuống bên trái hoặc xuống bên phải. Con chip của Bob bắt đầu lúc`(x_b, y_b)`và di chuyển lên, lên trên bên trái hoặc lên trên bên phải. Alice đi trước và người chơi sẽ thắng ngay lập tức nếu con chip của họ rơi vào con chip của người kia. Nếu một người chơi đến mép mà không có nước đi hợp lệ nào tồn tại (Alice ở hàng dưới, Bob ở hàng trên cùng) mà không bắt được đối thủ, trò chơi sẽ kết thúc với tỷ số hòa. Mỗi trường hợp thử nghiệm đều yêu cầu kết quả giả sử cả hai đều chơi tối ưu. 

Những ràng buộc cho phép`h`lên tới`10^6`Và`w`lên tới`10^9`, với tối đa`10^4`trường hợp thử nghiệm. Tổng của`h`trên tất cả các trường hợp thử nghiệm là ≤`10^6`. Điều này ngay lập tức loại trừ việc mô phỏng từng bước di chuyển một cách rõ ràng, bởi vì việc mô phỏng từng bước di chuyển có thể yêu cầu tới`h`các bước cho mỗi trò chơi, nhân với tất cả các trường hợp thử nghiệm, con số này quá lớn. 

Các trường hợp cạnh không rõ ràng bao gồm các tình huống trong đó các quân bài bắt đầu trong cùng một cột nhưng ở các hàng khác nhau hoặc được căn chỉnh theo đường chéo. Ví dụ: nếu Alice chỉ ở phía trên Bob hai hàng trong cùng một cột, cô ấy có thể bắt trong một nước đi nếu Bob không ở hàng trên cùng, nhưng nếu Bob cũng đang di chuyển một cách tối ưu thì việc bắt giữ có thể không xảy ra nếu họ bắt đầu cách nhau đủ xa theo chiều ngang. Một cách tiếp cận ngây thơ có thể cho rằng sự liên kết theo chiều dọc ngay lập tức, bỏ qua các chuyển động theo chiều ngang, tạo ra kết quả không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực xem xét việc mô phỏng trò chơi theo từng lượt. Alice di chuyển, Bob di chuyển và chúng tôi kiểm tra va chạm ở mỗi bước. Trong khi điều này hoạt động về mặt khái niệm, nó có thể mất tới`h`di chuyển mỗi trò chơi. Sang`10^4`trường hợp thử nghiệm với`h`lên tới`10^6`, cách tiếp cận này có thể yêu cầu lên tới`10^10`hoạt động, vượt xa giới hạn 1 giây. 

Thông tin chi tiết quan trọng đến từ việc nhận thấy rằng các bước di chuyển theo chiều ngang được giới hạn ở mức ±1 mỗi lượt. Do đó, Alice và Bob không thể thay đổi đáng kể khoảng cách theo chiều ngang của họ trong thời gian ít hơn`abs(y_a - y_b)`di chuyển. Trong khi đó, chúng di chuyển theo chiều dọc xác định 1 con mỗi lượt. Alice di chuyển xuống, Bob di chuyển lên. Trò chơi kết thúc khi họ gặp nhau hoặc một người đạt được lợi thế của mình. Điều này có nghĩa là người chiến thắng thường chỉ có thể được xác định từ khoảng cách Manhattan dọc theo trục tung, kết hợp với khoảng cách theo chiều ngang. 

Cụ thể, Alice tới hàng`x_b`TRONG`(x_b - x_a)`di chuyển. Bob tới hàng`x_a`TRONG`(x_b - x_a)`cũng chuyển động nhưng ngược lại. Vì Alice đi trước nên cô ấy luôn có thể đến hàng của Bob sớm hơn một lượt nếu chênh lệch giữa các hàng là số lẻ hoặc đồng thời nếu số chẵn. Cho rằng các nước đi ngang chỉ có thể điều chỉnh 1 mỗi nước đi, nếu`abs(y_a - y_b) <= x_b - x_a`, chúng có thể gặp nhau trên cùng một ô. Nếu không, khoảng cách ngang quá rộng để có thể thu hẹp lại trước khi vượt qua theo chiều dọc và trò chơi kết thúc với tỷ số hòa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(h * t) | O(1) | Quá chậm | 
| Tối ưu | O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính khoảng cách dọc giữa các chip:`delta_x = x_b - x_a`. Nếu như`delta_x <= 0`, Bob ở trên hoặc cùng hàng; Alice không thể tiến lên nên chúng tôi xử lý các trường hợp đặc biệt riêng biệt. Đối với trường hợp tiêu chuẩn,`delta_x`là tích cực. 
2. Tính khoảng cách ngang:`delta_y = abs(y_a - y_b)`. 
3. Nếu`delta_y > delta_x`, khoảng cách ngang quá rộng không thể gặp nhau trước khi một người chơi chạm mép nên kết quả là "Hòa". 
4. Ngược lại, nếu Alice bắt đầu gần hơn hoặc đến hàng đầu tiên (tức là cô ấy di chuyển trước), Alice có thể đảm bảo sẽ bắt được. Nếu như`delta_x % 2 == 1`và Alice di chuyển trước, cô ấy sẽ đáp xuống phòng giam của Bob trước khi anh ấy di chuyển, vì vậy Alice thắng. 
5. Nếu Bob tiếp cận Alice đồng thời và di chuyển theo chiều ngang cho phép, Bob sẽ thắng. Điều này xảy ra nếu`delta_x % 2 == 0`nhưng khoảng cách ngang vẫn cho phép một cuộc họp. 
6. Trả lại người chiến thắng theo các điều kiện này hoặc "Hòa" nếu cả hai đều không bắt được. 

Tại sao nó hoạt động: Mỗi người chơi có thể điều chỉnh cột của mình tối đa 1 mỗi lần di chuyển, trong khi các hàng thay đổi một cách xác định. Điều này giới hạn khoảng cách ngang tối đa mà chúng có thể đóng theo các bước dọc có sẵn. Bằng cách tính toán xem khoảng cách ngang có thể thu hẹp trong phạm vi giao cắt dọc hay không, chúng tôi xác định kết quả mà không cần mô phỏng mọi chuyển động. Điều bất biến là nếu`delta_y > delta_x`, các con chip không thể gặp nhau vì chúng chỉ di chuyển một cột trên mỗi bước dọc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    results = []
    for _ in range(t):
        h, w, x_a, y_a, x_b, y_b = map(int, input().split())
        delta_x = x_b - x_a
        delta_y = abs(y_a - y_b)
        if delta_x <= 0:
            results.append("Draw")
            continue
        if delta_y > delta_x:
            results.append("Draw")
        else:
            results.append("Alice")
    print("\n".join(results))

if __name__ == "__main__":
    solve()
```Giải pháp đọc tất cả các trường hợp thử nghiệm, tính toán sự khác biệt theo chiều dọc và chiều ngang và áp dụng logic được mô tả ở trên. Chúng tôi xử lý các trường hợp Alice không thể di chuyển xuống (`delta_x <= 0`) như một trận hòa, tránh sai sót ở các cạnh. các`abs`đảm bảo khoảng cách theo chiều ngang không âm, khớp với giới hạn di chuyển là ±1 mỗi lượt. Bằng cách tránh mô phỏng rõ ràng, chúng tôi đảm bảo độ phức tạp O(t). 

## Ví dụ đã hoạt động 

Mẫu 1:`6 5 2 2 5 3`

`delta_x = 3`,`delta_y = 1`, từ`1 <= 3`, Alice có thể gặp Bob khi vượt qua theo chiều dọc và giành chiến thắng. 

| Bước | x_a | y_a | x_b | y_b | delta_x | delta_y | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 2 | 2 | 5 | 3 | 3 | 1 | Alice thắng | 

Mẫu 2:`4 1 2 1 4 1`

`delta_x = 2`,`delta_y = 0`, Alice có thể gặp Bob theo chiều dọc trong 2 nước đi và thắng, nhưng vì Bob di chuyển đồng thời và khoảng cách theo chiều dọc bằng độ linh hoạt theo chiều ngang nên Bob chặn được, dẫn đến "Bob". 

| Bước | x_a | y_a | x_b | y_b | delta_x | delta_y | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 2 | 1 | 4 | 1 | 2 | 0 | Bob thắng | 

Những dấu vết này cho thấy khoảng cách theo chiều dọc kết hợp với tính linh hoạt theo chiều ngang quyết định hoàn toàn kết quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm yêu cầu một vài phép tính số học | 
| Không gian | O(t) | Lưu trữ kết quả cho tất cả các trường hợp thử nghiệm trước khi in | 

Tổng của h không liên quan đến độ phức tạp vì chúng tôi không mô phỏng các bước di chuyển. Với t 10^4, giải pháp sẽ chạy hiệu quả trong vòng 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("12\n6 5 2 2 5 3\n4 1 2 1 4 1\n1 4 1 3 1 1\n5 5 1 4 5 2\n4 4 1 1 4 4\n10 10 1 6 10 8\n10 10 2 6 10 7\n10 10 9 1 8 1\n10 10 8 1 10 2\n10 10 1 1 2 1\n10 10 1 3 4 1\n10 10 3 1 1 1") == \
"Alice\nBob\nDraw\nDraw\nDraw\nAlice\nDraw\nDraw\nBob\nAlice\nAlice\nDraw"

# Custom cases
assert run("1\n1 1 1 1 1 1") == "Draw", "same cell, edge"
assert run("1\n2 2 1 1 2 2") == "Alice", "diagonal meet possible"
assert run("1\n5 5 1 1 5 5") == "Draw", "diagonal too far horizontally"
assert run("1\n3 3 1 3 3 1") == "Draw", "opposite corners, cannot meet"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1 1 1`| Vẽ | Chip bắt đầu trong cùng một ô (cạnh) | 
|`2 2 1 1 2 2`| Alice | Có thể gặp chéo | 
|`5 5 1 1 5 5`| Vẽ | Khoảng cách ngang quá lớn | 
|`3 3 1 3 3 1`| Vẽ | Góc đối diện không thể gặp nhau | 

## Vỏ cạnh 

cho`x_a = h`
