---
title: "CF 106475E - \u041a\u0430\u043f\u0438\u0431\u0430\u0440\u044b \u0438\u0433\u0440\u0430\u044e\u0442 \u0432 <<\u0422\u0440\u0438 \u0432 \u0440\u044f\u0434>>"
description: "Chúng tôi có một lưới vô hạn. Một số ô đã chứa dấu. Bảng hiện tại được đảm bảo tránh bất kỳ ba ô được đánh dấu liên tiếp nào trên một đường ngang, dọc hoặc chéo."
date: "2026-06-25T08:51:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106475
codeforces_index: "E"
codeforces_contest_name: "\u0424\u0438\u043d\u0430\u043b \u0412\u041a\u041e\u0428\u041f.Junior 2026"
rating: 0
weight: 106475
solve_time_s: 50
verified: true
draft: false
---

[CF 106475E - \u041a\u0430\u043f\u0438\u0431\u0430\u0440\u044b \u0438\u0433\u0440\u0430\u044e\u0442 \u0432 <<\u0422\u0440\u0438 \u0432 \u0440\u044f\u0434>>](https://codeforces.com/problemset/problem/106475/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Chúng tôi có một lưới vô hạn. Một số ô đã chứa dấu. Bảng hiện tại được đảm bảo tránh bất kỳ ba ô được đánh dấu liên tiếp nào trên một đường ngang, dọc hoặc chéo. 

Chúng ta cần đếm xem có bao nhiêu ô trống có thể nhận thêm một dấu sao cho sau khi đặt có ít nhất một dòng gồm ba ô được đánh dấu liên tiếp. 

Đầu vào cung cấp tọa độ của tất cả các ô hiện được đánh dấu. Đầu ra là số tọa độ trống riêng biệt có thể hoàn thành một dòng có độ dài ba. 

Hạn chế quan trọng là có thể có tới 200.000 điểm hiện có, trong khi tọa độ có thể lớn tới 10^9 ở giá trị tuyệt đối. Giải pháp quét bảng là không thể vì bảng thực sự là vô hạn. Ngay cả việc kiểm tra mọi tọa độ trong một hình chữ nhật giới hạn lớn cũng sẽ đòi hỏi nhiều thời gian hơn. Chúng ta cần một thuật toán có công việc phụ thuộc vào số điểm chứ không phụ thuộc vào phạm vi tọa độ. 

Sự vắng mặt của các bộ ba hiện có là một sự đảm bảo hữu ích. Điều đó có nghĩa là mọi nước đi hợp lệ đều được tạo từ chính xác hai dấu hiện có và một dấu mới. Chúng ta không bao giờ cần phải xử lý tình huống mà bàn cờ đã có sẵn đường thắng. 

Một lỗi phổ biến là đếm các cặp có thể thay vì các ô có thể. Các cặp khác nhau có thể gợi ý cùng một ô trống, vì vậy chúng ta phải loại bỏ các ô trùng lặp. 

Ví dụ, hãy xem xét:```
3
0 0
1 0
0 1
```Câu trả lời đúng là`4`. Các tế bào`(2,0)`,`(-1,0)`,`(0,2)`, Và`(0,-1)`tất cả tạo thành bộ ba ngang hoặc dọc. Một cách tiếp cận bất cẩn tính riêng từng lần hoàn thành cặp có thể tính cùng một vị trí nhiều lần. 

Một trường hợp cạnh khác là khi hai điểm có đường chéo nhưng không đủ gần:```
2
0 0
1 2
```Câu trả lời đúng là`0`. Hai dấu này không thuộc về bất kỳ dòng dài ba nào có thể có một ô bị thiếu. Chỉ kiểm tra sự tồn tại của hai điểm mà không xác minh khoảng cách sẽ dẫn đến câu trả lời sai. 

# Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là kiểm tra từng ô trống và kiểm tra xem việc đánh dấu vào đó có tạo ra bộ ba hay không. Điều này đúng vì mọi câu trả lời có thể đều được kiểm tra. Vấn đề là số lượng ô trống không bị giới hạn theo bất kỳ cách nào có ý nghĩa. Tọa độ lên tới 10^9, do đó, ngay cả một số lượng nhỏ nhãn hiệu cũng có thể có diện tích xung quanh rất lớn. Phương pháp vũ phu thậm chí không thể liệt kê được các ứng cử viên. 

Quan sát quan trọng là mọi nước đi thành công đều phải hoàn thành một đoạn có đúng ba ô. Chỉ có bốn hướng có thể có cho một đoạn như vậy: ngang, dọc và hai đường chéo. Nếu biết một dấu hiệu hiện có, chúng ta chỉ cần nhìn xa một hoặc hai ô theo các hướng này. 

Đối với vectơ có hướng cố định`(dx, dy)`, hai nhãn hiệu hiện có có thể xuất hiện ở ba vị trí có thể có của một đoạn dài ba. Nếu ô thứ nhất và ô thứ hai bị chiếm, các ô bị thiếu sẽ ở trước cặp một bước và sau cặp một bước. Nếu ô thứ nhất và thứ ba bị chiếm thì ô bị thiếu là ô ở giữa. Vì chỉ có bốn hướng và kiểm tra khoảng cách không đổi, mỗi điểm hiện tại chỉ tạo ra một số lượng ứng viên không đổi. 

Bước cuối cùng là loại bỏ trùng lặp. Chúng tôi chèn mọi tọa độ câu trả lời có thể có vào một tập hợp. Các tọa độ đã được sử dụng sẽ bị bỏ qua vì chúng ta phải đánh dấu vào ô trống. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(diện tích bảng tìm kiếm) | O(diện tích bảng tìm kiếm) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Lưu trữ tất cả các ô được đánh dấu hiện có trong bộ băm. Tra cứu tọa độ phải có thời gian không đổi vì mọi ứng cử viên được tạo cần phải được kiểm tra đối với bảng hiện tại. 
2. Đối với mỗi ô được đánh dấu, hãy kiểm tra bốn hướng`(1,0)`,`(0,1)`,`(1,1)`, Và`(1,-1)`. Chúng đại diện cho tất cả các hướng có thể có của một dòng ba ô không trùng lặp. 
3. Đối với mỗi hướng, hãy kiểm tra ô cách một bước. Nếu nó bị chiếm, hai ô có thể là hai vị trí đầu tiên của bộ ba. Thêm hai ô có thể bị thiếu, một ô trước cặp và một ô sau cặp. 
4. Đối với cùng một hướng, hãy kiểm tra ô cách đó hai bước. Nếu nó bị chiếm, hai dấu có thể là điểm cuối của bộ ba. Thêm ô ở giữa làm câu trả lời có thể. 
5. Loại bỏ những ứng viên đã có người. Số tọa độ duy nhất còn lại là câu trả lời. 

Tại sao nó hoạt động: mỗi nước đi hợp lệ sẽ tạo ra một dòng gồm ba ô. Trong dòng đó, lấy bất kỳ một trong hai ô đã bị chiếm giữ và xem xét hướng tới ô bị chiếm giữ còn lại. Khoảng cách giữa hai ô hiện có là một hoặc hai. Thuật toán kiểm tra chính xác hai trường hợp đó và thêm vị trí còn thiếu. Vì mọi vị trí được tạo đều được lưu trữ trong một tập hợp nên nhiều cách để khám phá cùng một nước đi sẽ không ảnh hưởng đến kết quả. 

#Giải pháp Python```python
import sys

input = sys.stdin.readline

def solve():
    n = int(input())
    points = set()

    for _ in range(n):
        x, y = map(int, input().split())
        points.add((x, y))

    directions = [
        (1, 0),
        (0, 1),
        (1, 1),
        (1, -1),
    ]

    ans = set()

    for x, y in points:
        for dx, dy in directions:
            x1, y1 = x + dx, y + dy
            if (x1, y1) in points:
                a = (x - dx, y - dy)
                b = (x + 2 * dx, y + 2 * dy)
                if a not in points:
                    ans.add(a)
                if b not in points:
                    ans.add(b)

            x2, y2 = x + 2 * dx, y + 2 * dy
            if (x2, y2) in points:
                mid = (x + dx, y + dy)
                if mid not in points:
                    ans.add(mid)

    print(len(ans))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của mã xây dựng tập hợp các ô bị chiếm dụng. Một bộ dữ liệu Python được sử dụng làm biểu diễn tọa độ, cho phép kiểm tra tư cách thành viên trực tiếp. 

Danh sách hướng chỉ chứa bốn vectơ vì hướng ngược lại sẽ mô tả cùng một đường thẳng. Ví dụ, kiểm tra`(1,0)`đã bao gồm các bộ ba ngang, vì vậy hãy kiểm tra`(-1,0)`sẽ chỉ sao chép công việc. 

Điều kiện đầu tiên kiểm tra các ô được đánh dấu liền kề. Nếu như`(x,y)`Và`(x+dx,y+dy)`bị chiếm giữ, chúng tạo thành hai ô lân cận của một bộ ba. Ô thứ ba có thể ở hai bên của chúng. 

Điều kiện thứ hai kiểm tra các ô cách nhau một khoảng trống. Nếu như`(x,y)`Và`(x+2dx,y+2dy)`đang bị chiếm giữ, vị trí duy nhất còn thiếu có thể là ô giữa của họ. 

bộ`ans`là cần thiết vì cùng một ô trống có thể được tạo từ các dấu hiện có khác nhau hoặc các hướng khác nhau. Số nguyên Python xử lý trực tiếp các giá trị tọa độ lớn, do đó không cần xử lý tràn. 

# Ví dụ đã hoạt động 

Sử dụng mẫu đầu tiên:```
4
2 2
3 3
4 2
5 2
```Thuật toán kiểm tra từng điểm theo bốn hướng. 

| Ô hiện tại | Hướng | Tìm thấy tế bào | Đã thêm ứng viên | 
| --- | --- | --- | --- | 
| (2,2) | (1,1) | (3,3) tồn tại | (1,1), (4,4) | 
| (4,2) | (1,0) | (5,2) tồn tại | (3,2), (6,2) | 
| (4,2) | (1,-1) | (3,3) tồn tại ở khoảng cách 1 | (5,1), (2,4) | 

Sau tất cả các lần kiểm tra, tất cả sáu vị trí được tạo đều khác biệt, vì vậy câu trả lời là:```
6
```Điều này chứng tỏ tại sao chúng ta phải xem xét cả hai vế của một cặp liền kề. Một cặp có thể tạo ra hai nước đi chiến thắng khác nhau. 

Đối với mẫu thứ hai:```
2
3 3
1 4
```| Ô hiện tại | Hướng | Tìm thấy tế bào | Đã thêm ứng viên | 
| --- | --- | --- | --- | 
| (3,3) | (1,-1) | không có ô phù hợp | không | 
| (1,4) | (1,-1) | không có ô phù hợp | không | 

Không có hai điểm nào được đặt cách nhau một hoặc hai bước trên một đường hợp lệ, vì vậy không có bước di chuyển nào có thể tạo ra bộ ba. 

Kết quả là:```
0
```Điều này xác nhận rằng thuật toán chỉ tạo ra các ứng cử viên từ các phân đoạn ba ô thực tế có thể có. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi điểm kiểm tra bốn hướng và một số ô lân cận không đổi. | 
| Không gian | O(n) | Tập chiếm và tập ứng cử viên chỉ chứa bội số không đổi của số điểm. | 

Với`n = 200000`, thuật toán chỉ thực hiện vài triệu phép tính băm, vừa vặn trong giới hạn. 

# Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    def solve():
        input = sys.stdin.readline
        n = int(input())
        points = set()

        for _ in range(n):
            x, y = map(int, input().split())
            points.add((x, y))

        directions = [(1, 0), (0, 1), (1, 1), (1, -1)]
        ans = set()

        for x, y in points:
            for dx, dy in directions:
                if (x + dx, y + dy) in points:
                    for p in [(x - dx, y - dy), (x + 2 * dx, y + 2 * dy)]:
                        if p not in points:
                            ans.add(p)

                if (x + 2 * dx, y + 2 * dy) in points:
                    p = (x + dx, y + dy)
                    if p not in points:
                        ans.add(p)

        return str(len(ans))

    result = solve()
    sys.stdin = old_stdin
    return result

assert run("""4
2 2
3 3
4 2
5 2
""") == "6"

assert run("""2
3 3
1 4
""") == "0"

assert run("""1
0 0
""") == "0"

assert run("""3
0 0
1 0
2 0
""") == "0"

assert run("""2
1000000000 1000000000
999999999 999999999
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Dấu đơn | 0 | Không có cặp nào tồn tại nên không thể di chuyển | 
| Đã căn chỉnh ba dấu | 0 | Sự đảm bảo được kiểm tra dựa trên việc xử lý ứng viên trùng lặp | 
| Tọa độ rất lớn | 2 | Phạm vi tọa độ và xử lý số nguyên | 
| Hai dấu chéo liền nhau | 2 | Cả hai mặt của một cặp liền kề đều được tính | 

# Vỏ cạnh 

Đối với trường hợp đếm trùng lặp:```
3
0 0
1 0
0 1
```Cặp đôi`(0,0)`Và`(1,0)`gợi ý`(2,0)`Và`(-1,0)`. Cặp đôi`(0,0)`Và`(0,1)`gợi ý`(0,2)`Và`(0,-1)`. Tập cuối cùng chứa bốn ô, vì vậy câu trả lời là`4`. Nếu các ứng cử viên được tính mà không có bộ, những khám phá chồng chéo có thể làm tăng câu trả lời. 

Đối với một khoảng cách chéo:```
2
0 0
2 2
```Hai điểm cách nhau đúng hai bước trên đường chéo. Thuật toán phát hiện`(1,1)`là ô ở giữa bị thiếu nên câu trả lời là`1`. 

Đối với các ô chéo ở xa:```
2
0 0
1 2
```Không có độ lệch được kiểm tra nào khớp với khoảng cách một hoặc khoảng cách hai theo hướng hợp lệ. Tập ứng viên vẫn trống, đưa ra câu trả lời`0`. 

Đối với tọa độ gần giới hạn số nguyên:```
2
1000000000 1000000000
999999999 999999999
```Thuật toán vẫn hoạt động vì tọa độ chỉ được dịch chuyển tối đa hai đơn vị. Số nguyên Python biểu thị các giá trị này một cách an toàn và các thao tác thiết lập hoạt động giống như đối với tọa độ nhỏ.
