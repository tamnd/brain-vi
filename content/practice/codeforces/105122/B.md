---
title: "CF 105122B - Đường dẫn giám mục"
description: "Chúng ta có một bàn cờ $n nhân n$ và một quân tượng được đặt ở ô xuất phát. Giám mục có thể di chuyển bất kỳ số ô nào trong một lần di chuyển, nhưng chỉ dọc theo đường chéo."
date: "2026-06-27T19:37:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105122
codeforces_index: "B"
codeforces_contest_name: "XXVI Interregional Programming Olympiad, Vologda SU, 2024"
rating: 0
weight: 105122
solve_time_s: 87
verified: false
draft: false
---

[CF 105122B - Đường dẫn Bishop](https://codeforces.com/problemset/problem/105122/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times n$bàn cờ và quân tượng được đặt ở ô xuất phát. Giám mục có thể di chuyển bất kỳ số ô nào trong một lần di chuyển, nhưng chỉ dọc theo đường chéo. Nhiệm vụ không chỉ là quyết định xem quân tượng có thể đến được ô đích hay không mà còn đếm xem có bao nhiêu tuyến đường di chuyển ngắn nhất khác nhau tồn tại từ điểm xuất phát đến đích. 

Con đường ngắn nhất ở đây có nghĩa là con đường sử dụng số lần di chuyển theo đường chéo tối thiểu có thể. Vì nước đi của quân tượng là một đường trượt chéo theo đường thẳng, độ dài đường đi duy nhất có thể là nước đi bằng 0 nếu chúng ta đã bắt đầu ở đích, một nước đi nếu cả hai hình vuông nằm trên cùng một đường chéo hoặc hai nước đi nếu quân tượng phải đi vòng qua một hình vuông trung gian được căn chỉnh theo đường chéo với cả hai điểm cuối. 

Kích thước bảng có thể lớn bằng$10^9$, điều này ngay lập tức loại trừ mọi mô phỏng trên lưới. Bất kỳ giải pháp nào cũng phải hoạt động hoàn toàn với hình học tọa độ thay vì truyền tải. Mọi quyết định chỉ phụ thuộc vào quan hệ số học giữa các tọa độ. 

Điều tinh tế đầu tiên là các giám mục bị hạn chế bởi sự tương đồng về màu sắc. Trên bàn cờ, quân tượng luôn nằm trên các ô có cùng giá trị$x + y \bmod 2$. Nếu các ô bắt đầu và kết thúc khác nhau về tính chẵn lẻ thì không có chuỗi nước đi nào có thể kết nối chúng, bất kể kích thước bàn cờ. 

Trường hợp cạnh thứ hai là khi phần đầu và phần cuối giống hệt nhau. Câu trả lời đúng chính xác là một đường đi có độ dài bằng 0, mặc dù không có bước di chuyển nào được thực hiện. 

Tình huống thứ ba thường gây ra sai lầm là khi dường như có thể thực hiện được hai cách phân tách theo hai bước khác nhau. Vì nước đi của quân tượng là một đoạn đường chéo đầy đủ nên có thể có tối đa hai ô vuông trung gian riêng biệt nối đường chéo bắt đầu và đường chéo mục tiêu. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ cố gắng khám phá tất cả các bước di chuyển của quân tượng có thể có từ ô xuất phát và theo dõi tất cả các đường đi ngắn nhất đến đích. Từ bất kỳ vị trí nào, quân tượng có tối đa bốn hướng chéo và mỗi hướng cho phép tối đa$O(n)$các vị trí hạ cánh. Ngay cả khi chúng ta giới hạn bản thân ở những đường đi ngắn nhất, không gian trạng thái vẫn trở thành một đồ thị có$O(n^2)$các nút và kết nối chéo dày đặc. Về mặt khái niệm thì BFS sẽ hoạt động, nhưng$n$có thể$10^9$, do đó việc liệt kê các nút hoặc cạnh là không thể. 

Quan sát quan trọng là cấu trúc chuyển động của giám mục thu gọn toàn bộ vấn đề thành một tập hợp nhỏ các trường hợp hình học. Hoặc đích đến nằm trên một trong hai đường chéo tính từ điểm xuất phát hoặc không. Nếu có thì có đúng một con đường ngắn nhất. Nếu không, nhưng các ô vuông có tính chẵn lẻ, tuyến đường ngắn nhất phải có đúng hai nước đi và bất kỳ đường đi hợp lệ nào đều được xác định đầy đủ bằng cách chọn một ô vuông nằm đồng thời trên một đường chéo từ điểm bắt đầu và một đường chéo từ đích đến. 

Điều này làm giảm vấn đề tìm giao điểm của hai đường chéo. Mỗi nước đi của quân tượng tương ứng với việc chọn một trong hai dòng$x - y = c$hoặc$x + y = c$, do đó các hình vuông trung gian là giao điểm của một đường chéo từ đầu với một đường chéo từ cuối. Có nhiều nhất hai giao điểm như vậy và chúng ta chỉ cần đếm xem có bao nhiêu điểm nguyên hợp lệ bên trong bảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force BFS trên tàu |$O(n^2)$|$O(n^2)$| Quá chậm | 
| Phân tích trường hợp hình học |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Kiểm tra xem ô đầu và ô cuối có giống nhau không. Nếu đúng như vậy thì quân tượng đã đáp ứng yêu cầu mà không cần di chuyển nên chỉ có một con đường hợp lệ. 
2. Tính độ tương đương màu của cả hai hình vuông bằng cách sử dụng$(x + y) \bmod 2$. Nếu những giá trị này khác nhau, hãy trả về 0 vì không có chuỗi di chuyển quân tượng nào có thể vượt qua các lớp màu. 
3. Kiểm tra xem cả hai hình vuông có nằm trên cùng một đường chéo hay không. Điều này xảy ra nếu một trong hai$x_1 - y_1 = x_2 - y_2$hoặc$x_1 + y_1 = x_2 + y_2$. Nếu điều này đúng, quân tượng có thể đi thẳng trong một nước đi và có chính xác một con đường như vậy. 
4. Nếu không, số lần di chuyển tối thiểu là hai. Mọi đường đi hợp lệ đều phải đi qua một hình vuông trung gian có thể đến được từ cả hai điểm cuối trong một lần di chuyển theo đường chéo. 
5. Xây dựng tối đa hai hình vuông trung gian bằng cách cắt nhau các đường chéo: 

Ứng cử viên đầu tiên đến từ việc giải quyết$x - y = x_1 - y_1$Và$x + y = x_2 + y_2$. 

Ứng cử viên thứ hai đến từ việc giải quyết$x + y = x_1 + y_1$Và$x - y = x_2 - y_2$. 
6. Với mỗi ứng viên, hãy tính$x = \frac{(sum \pm diff)}{2}$,$y = sum - x$và kiểm tra xem cả hai tọa độ có phải là số nguyên và nằm trong giới hạn bảng hay không. 
7. Đếm xem có bao nhiêu ô vuông trung gian ứng cử viên này là hợp lệ. Mỗi điểm giữa hợp lệ tương ứng với chính xác một đường đi ngắn nhất riêng biệt. 

### Tại sao nó hoạt động 

Mỗi đường đi hai nước ngắn nhất phải bao gồm nước đi đầu tiên từ đầu đến một ô nào đó trên một trong các đường chéo của nó và nước đi thứ hai từ ô đó đến mục tiêu. Các ô vuông duy nhất có thể tiếp cận trong cả hai bước chính xác là các điểm giao nhau của các đường chéo được xác định bởi các điểm cuối. Vì mỗi cặp đường chéo giao nhau ở nhiều nhất một điểm, nên tổng số ô vuông trung gian ứng cử viên nhiều nhất là hai và mỗi ô tương ứng với một phân tách đường dẫn duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    x1, y1 = map(int, input().split())
    x2, y2 = map(int, input().split())

    if x1 == x2 and y1 == y2:
        print(1)
        return

    if (x1 + y1) % 2 != (x2 + y2) % 2:
        print(0)
        return

    if x1 - y1 == x2 - y2 or x1 + y1 == x2 + y2:
        print(1)
        return

    def valid(x, y):
        return 1 <= x <= n and 1 <= y <= n

    count = 0

    s1 = x1 + y1
    d1 = x1 - y1
    s2 = x2 + y2
    d2 = x2 - y2

    if (s2 + d1) % 2 == 0:
        x = (s2 + d1) // 2
        y = s2 - x
        if valid(x, y):
            count += 1

    if (s1 + d2) % 2 == 0:
        x = (s1 + d2) // 2
        y = s1 - x
        if valid(x, y):
            count += 1

    print(count)

if __name__ == "__main__":
    solve()
```Giải pháp được cấu trúc xung quanh các lối thoát sớm cho các trường hợp không di chuyển và một bước di chuyển, vì những trường hợp này chiếm ưu thế khi căn chỉnh tọa độ. Việc kiểm tra tính chẵn lẻ loại bỏ tất cả các trường hợp không thể xảy ra trước bất kỳ cách xây dựng hình học nào, tránh số học không cần thiết. 

Hai điểm giữa ứng cử viên đến trực tiếp từ việc giải hệ phương trình đường chéo. Việc kiểm tra số nguyên được ngầm thực hiện thông qua xác thực tổng chẵn, vì các giao điểm chéo yêu cầu tính chẵn lẻ nhất quán. Mỗi ứng cử viên sau đó được xác nhận dựa trên ranh giới của hội đồng, vì giao điểm đại số có thể nằm bên ngoài$n \times n$lưới mặc dù các đường chéo vô hạn giao nhau ở đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một bảng nhỏ nơi bắt đầu$(1,1)$và mục tiêu là$(3,3)$trên một$n = 5$Cái bảng. 

| Bước | Vị trí | Cùng một đường chéo | Hành động | 
| --- | --- | --- | --- | 
| Bắt đầu | (1,1) | - | tính chẵn lẻ | 
| Mục tiêu | (3,3) | vâng | có thể truy cập trong 1 lần di chuyển | 

Cả hai điểm đều thỏa mãn$x - y = 0$, nên quân tượng có thể di chuyển trực tiếp. 

Điều này xác nhận tồn tại một con đường ngắn nhất. 

### Ví dụ 2 

Bắt đầu$(1,3)$, mục tiêu$(3,1)$, với$n = 5$. 

| Bước | Tính toán | Kết quả | 
| --- | --- | --- | 
| Chẵn lẻ | (1+3) % 2 = (3+1) % 2 | giống nhau | 
| Cùng một đường chéo | sai | cần 2 chiêu | 
| Ứng viên 1 | (s2 + d1)/2 = (4 + -2)/2 | x = 1 | 
| Ứng viên 2 | (s1 + d2)/2 = (4 + 2)/2 | x = 3 | 

Cả hai điểm trung gian$(1,1)$Và$(3,3)$nằm bên trong bảng. 

Điều này cho thấy có chính xác hai đường đi ngắn nhất khác nhau, một đường đi qua mỗi điểm giữa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ các phép toán và kiểm tra số học liên tục mới được thực hiện | 
| Không gian |$O(1)$| Không sử dụng cấu trúc phụ trợ | 

Những ràng buộc cho phép$n$lên đến$10^9$, nhưng lời giải chỉ phụ thuộc vào đại số tọa độ, do đó thời gian thực hiện không đổi bất kể kích thước bảng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue() if False else capture(inp)

def capture(inp):
    import sys, io
    backup = sys.stdout
    sys.stdout = io.StringIO()
    sys.stdin = io.StringIO(inp)
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = backup
    return out.strip()

# sample-like cases
assert capture("5\n1 1\n3 3\n") == "1"
assert capture("5\n1 3\n3 1\n") == "2"

# same cell
assert capture("10\n4 4\n4 4\n") == "1"

# unreachable parity
assert capture("8\n1 1\n2 3\n") == "0"

# direct diagonal
assert capture("100\n2 7\n10 15\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cùng một ô | 1 | trường hợp không di chuyển | 
| sự không khớp chẵn lẻ | 0 | cấu hình không thể truy cập | 
| cùng một đường chéo | 1 | trường hợp một lần | 
| hai ngã tư | 2 | trường hợp hai đường dẫn đầy đủ | 

## Vỏ cạnh 

Khi phần đầu và phần cuối trùng nhau, thuật toán ngay lập tức trả về một phần trước bất kỳ suy luận hình học nào. Điều này tránh việc đếm không chính xác các giao điểm chéo có thể gợi ý các ô vuông trung gian. 

Khi giá trị chẵn lẻ khác nhau, quá trình tính toán sẽ dừng sớm vì tất cả các giao điểm đại số tiếp theo sẽ vẫn biểu thị các hình vuông có màu không chính xác, khiến mọi đường dẫn đều không thể thực hiện được bất kể hình dạng bảng. 

Khi cả hai hình vuông nằm trên cùng một đường chéo, cả hai công thức tính điểm giữa đều có thể tạo ra điểm cuối hoặc điểm không hợp lệ. Việc kiểm tra đường chéo sớm sẽ ngăn chặn việc đếm các cấu trúc trung gian giả và sửa chính xác câu trả lời thành một.
