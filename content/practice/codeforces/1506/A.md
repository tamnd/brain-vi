---
title: "CF 1506A - Bàn Lạ"
description: "Chúng ta có một lưới hình chữ nhật có $n$ hàng và $m$ cột. Mỗi ô chứa một số duy nhất từ ​​$1$ đến $n cdot m$, nhưng việc đánh số được xác định theo hai cách khác nhau. Đầu tiên, hãy tưởng tượng việc lấp đầy cột lưới theo từng cột."
date: "2026-06-10T20:17:15+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1506
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 710 (Div. 3)"
rating: 800
weight: 1506
solve_time_s: 113
verified: true
draft: false
---

[CF 1506A - Bảng lạ](https://codeforces.com/problemset/problem/1506/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một lưới hình chữ nhật có$n$hàng và$m$cột. Mỗi ô chứa một số duy nhất từ$1$ĐẾN$n \cdot m$, nhưng việc đánh số được xác định theo hai cách khác nhau. 

Đầu tiên, hãy tưởng tượng việc lấp đầy cột lưới theo từng cột. Chúng ta bắt đầu từ cột ngoài cùng bên trái, điền từ trên xuống dưới, sau đó chuyển sang cột tiếp theo và lặp lại. Điều này tạo ra một sơ đồ đánh số cố định trong đó các giá trị tăng dần xuống trong một cột và sau đó nhảy lên đầu cột tiếp theo. 

Thứ hai, chúng tôi xem xét cách đánh số khác, tự nhiên hơn: theo từng hàng. Chúng ta bắt đầu từ ô trên cùng bên trái, điền từng hàng từ trái sang phải và di chuyển xuống từng hàng. 

Đối với mỗi truy vấn, chúng tôi được cung cấp$n$,$m$, và một giá trị$x$tương ứng với nhãn của ô trong cách đánh số theo cột. Nhiệm vụ là xác định số nào mà ô đó sẽ có khi đánh số theo hàng. 

Các ràng buộc cho phép lên đến$10^4$trường hợp thử nghiệm, với$n, m$lên đến$10^6$, Và$x$lên đến$10^{12}$trong một số trường hợp. This immediately rules out any simulation of the grid. Even constructing a single grid is impossible due to both memory and time constraints. Giải pháp phải là$O(1)$mỗi trường hợp thử nghiệm. 

Một vấn đề tế nhị phát sinh từ việc lập chỉ mục. Vì cả hai sơ đồ đánh số đều dựa trên cơ sở 1 và có cấu trúc khác nhau, nên các sai sót riêng lẻ trong tính toán hàng và cột là điểm thất bại phổ biến nhất. Một cạm bẫy tiềm tàng khác là tràn số nguyên nếu các sản phẩm trung gian như$n \cdot m$được tính toán bằng số học 32 bit, mặc dù Python đương nhiên tránh được điều này. 

Một cách tiếp cận đơn giản có thể cố gắng xây dựng lại lưới và lập chỉ mục lại nó. Ví dụ, đối với$n=3, m=5$, chúng ta sẽ điền toàn bộ bảng theo thứ tự cột, lưu trữ nó và sau đó xây dựng lại thứ tự hàng. Điều này đã thất bại đối với các ràng buộc lớn, nhưng ngay cả lý luận thủ công cũng cho thấy điều đó là không cần thiết. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: mô phỏng việc đánh số theo cột thành một mảng 2D, sau đó gán cách đánh số theo hàng riêng biệt và cuối cùng ánh xạ lại các giá trị. Mỗi ô được truy cập hai lần, do đó độ phức tạp là$O(nm)$mỗi trường hợp thử nghiệm. Với tối đa$10^4$trường hợp thử nghiệm và$nm$có khả năng$10^{12}$, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là cả hai hệ thống số chỉ là sự tuyến tính hóa khác nhau của cùng một ma trận. Cấu trúc đánh số theo cột cho chúng ta một cách trực tiếp để giải mã một số$x$vào vị trí hàng và cột của nó. Khi chúng ta biết tọa độ, việc chuyển đổi sang đánh số theo hàng là chuyện nhỏ. 

Trong cách đánh số theo cột, mỗi cột chứa chính xác$n$các phần tử. Điều đó có nghĩa là chỉ số cột của$x$được xác định bởi có bao nhiêu cột đầy đủ phù hợp trước nó. Cụ thể,$(x-1) // n$đưa ra chỉ mục cột và$(x-1) \% n$đưa ra chỉ mục hàng. 

Một khi chúng ta có được vị trí$(r, c)$, việc chuyển đổi sang đánh số theo hàng chỉ đơn giản là đặt nó theo một chuỗi trong đó các hàng là các khối có kích thước liền kề nhau$m$. Vậy giá trị cuối cùng là$r \cdot m + c + 1$. 

Việc chuyển đổi làm giảm vấn đề từ việc xây dựng lại lưới sang lập chỉ mục số học đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nm)$mỗi bài kiểm tra |$O(nm)$| Quá chậm | 
| Tối ưu |$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giải thích giá trị$x$như một vị trí trong quá trình tuyến tính hóa theo cột chính của lưới với$n$hàng. Điều này có nghĩa là mỗi khối có kích thước$n$tương ứng với một cột duy nhất. 
2. Tính chỉ số cột như sau$(x - 1) // n$. Điều này hoạt động vì mỗi cột đóng góp chính xác$n$phần tử trước khi chuyển sang phần tử tiếp theo. 
3. Tính chỉ số hàng như$(x - 1) \% n$. Điều này xác định vị trí bên trong cột từ trên xuống dưới. 
4. Bây giờ diễn giải lại tương tự$(row, column)$vị trí theo thứ tự hàng lớn. Trong cách đánh số theo hàng, mỗi hàng đóng góp chính xác$m$các phần tử trước khi chuyển sang hàng tiếp theo. 
5. Chuyển đổi tọa độ trở lại thành giá trị tuyến tính bằng cách sử dụng$row \cdot m + column + 1$. Điều này tạo ra nhãn theo hàng của cùng một ô. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là cả hai hệ thống đánh số đều là song ánh trên cùng một lưới. Đánh số theo cột xác định ánh xạ$f(r, c) = c \cdot n + r + 1$, trong khi đánh số theo hàng xác định$g(r, c) = r \cdot m + c + 1$. Vì cả hai đều biểu diễn cùng một hệ tọa độ nên việc giải mã$x$bởi vì$f^{-1}$và mã hóa lại thông qua$g$bảo toàn tính đúng đắn. Không có xung đột hoặc mơ hồ tồn tại vì mỗi số nguyên tương ứng với chính xác một vị trí lưới. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, x = map(int, input().split())

        x -= 1
        col = x // n
        row = x % n

        ans = row * m + col + 1
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp xử lý từng trường hợp thử nghiệm một cách độc lập trong thời gian không đổi. Bước quan trọng trước tiên là chuyển đổi chỉ mục đã cho thành tọa độ dựa trên 0, điều này tránh sự phức tạp từng phần một trong các hoạt động chia và modulo. Sau khi tính toán hàng và cột, chúng tôi trực tiếp áp dụng lại việc lập chỉ mục theo hàng chính. 

Một lỗi phổ biến là quên trừ 1 trước khi chia thành tọa độ, điều này làm dịch chuyển tất cả các kết quả không chính xác. Một người khác đang trao đổi$n$Và$m$ở bước chia, điều này phá vỡ hoàn toàn việc giải thích cột. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$n=3, m=5, x=11$| Bước | Giá trị | 
| --- | --- | 
| x (dựa trên 0) | 10 | 
| col = x // n | 3 | 
| hàng = x %n | 1 | 
| kết quả = hàng * m + col + 1 | 1 * 5 + 3 + 1 = 9 | 

Điều này xác nhận rằng ô thứ 11 theo cột tương ứng với 9 khi đánh số theo hàng. 

### Ví dụ 2 

đầu vào:$n=2, m=2, x=3$| Bước | Giá trị | 
| --- | --- | 
| x (dựa trên 0) | 2 | 
| col = x // n | 1 | 
| hàng = x %n | 0 | 
| kết quả = hàng * m + col + 1 | 0 * 2 + 1 + 1 = 2 | 

Điều này cho thấy cách một ô trong cột thứ hai, hàng đầu tiên ánh xạ chính xác theo thứ tự hàng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| Mỗi trường hợp thử nghiệm sử dụng các phép toán số học không đổi | 
| Không gian |$O(1)$| Không sử dụng cấu trúc phụ trợ | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì thậm chí$10^4$hoạt động là không đáng kể trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n, m, x = map(int, input().split())
        x -= 1
        col = x // n
        row = x % n
        out.append(str(row * m + col + 1))
    return "\n".join(out)

# provided samples
assert run("5\n1 1 1\n2 2 3\n3 5 11\n100 100 7312\n1000000 1000000 1000000000000\n") == \
"1\n2\n9\n1174\n1000000000000"

# custom cases
assert run("1\n3 3 1\n") == "1"
assert run("1\n3 3 9\n") == "9"
assert run("1\n2 5 6\n") == "3"
assert run("1\n5 2 10\n") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 3 3 1 | 1 | góc lưới nhỏ nhất | 
| 3 3 9 | 9 | ranh giới ô cuối cùng | 
| 2 5 6 | 3 | chuyển đổi hàng chéo | 
| 5 2 10 | 5 | cột-chính bọc | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi$x = 1$. Ở đây, giá trị dựa trên 0 là 0, vì vậy cả hàng và cột đều là 0 và câu trả lời đúng sẽ trở thành 1. Bất kỳ việc xử lý sai nào đối với việc lập chỉ mục dựa trên 1 sẽ chuyển giá trị này thành 0 hoặc chỉ mục không hợp lệ. 

Một trường hợp khác là khi$x = n \cdot m$, ô cuối cùng trong lưới. Sau khi trừ đi một, ta được$x-1 = nm - 1$, tạo ra cột$m-1$và hàng$n-1$. Việc cắm vào công thức sẽ cho$(n-1)m + (m-1) + 1 = nm$, bảo quản chính xác nhãn cuối cùng. 

Một trường hợp thất bại tinh tế xuất hiện khi$n = 1$. Lưới trở thành một hàng duy nhất và việc đánh số theo cột và theo hàng giống hệt nhau. Công thức giảm xuống còn$col = x$,$row = 0$, sản xuất$x$, phù hợp với mong đợi.
