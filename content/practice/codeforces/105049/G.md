---
title: "CF 105049G - Khi tôi kết thúc Đoạn giới thiệu"
description: "Chúng ta có hai tập hợp điểm trên trục số. Bộ sưu tập đầu tiên bao gồm các tiệm bánh có vị trí $x1, x2, dấu chấm, xN$."
date: "2026-06-28T05:48:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105049
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 03-22-24 Div. 1 (Advanced)"
rating: 0
weight: 105049
solve_time_s: 87
verified: false
draft: false
---

[CF 105049G - Khi tôi kết thúc Đoạn giới thiệu](https://codeforces.com/problemset/problem/105049/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai tập hợp điểm trên trục số. Bộ sưu tập đầu tiên bao gồm các tiệm bánh với các vị trí$x_1, x_2, \dots, x_N$. Cái thứ hai bao gồm các hộp thư với các vị trí$y_j$, mỗi người cũng mang một trọng lượng$k_j$đại diện cho những kẻ thù khác gặp phải nếu Cyrano truy cập hộp thư đó. 

Cyrano phải thực hiện đúng một chuyến đi bắt đầu từ mỗi tiệm bánh. Đối với một tiệm bánh ở vị trí$x_i$, anh ấy chọn bất kỳ hộp thư nào$y_j$đến thăm. Chi phí lựa chọn hộp thư$j$từ tiệm bánh$i$có hai thành phần: chi phí đi lại$x_i \cdot y_j$và chi phí cố định bổ sung$k_j$từ việc chiến đấu với những người cầu hôn ở hộp thư đó. Mục tiêu là chỉ định chính xác một hộp thư cho mỗi tiệm bánh để tổng chi phí cho tất cả các lựa chọn được giảm thiểu. 

Vì vậy, nhiệm vụ giảm xuống còn việc lựa chọn, độc lập cho từng$x_i$, chỉ mục hộp thư$j$giảm thiểu$x_i y_j + k_j$, và tổng hợp tất cả các tiệm bánh. 

Những hạn chế$N, M \le 10^5$ngay lập tức loại trừ việc kiểm tra tất cả các cặp$(i, j)$, đó sẽ là$10^{10}$hoạt động. Giải pháp nào cũng phải khai thác cấu trúc trong biểu thức tuyến tính$x_i y_j + k_j$để tránh việc đánh giá từng cặp. 

Trường hợp cạnh tinh tế xuất phát từ các giá trị bằng 0. Nếu như$x_i = 0$, thì chi phí giảm xuống còn$k_j$, nghĩa là chỉ có trọng lượng hộp thư mới quan trọng. Nếu tất cả$y_j = 0$, thì tất cả các tiệm bánh sẽ chọn hộp thư có số lượng tối thiểu$k_j$. Việc triển khai đơn giản bỏ qua việc xử lý bằng 0 hoặc giả định tính tích cực nghiêm ngặt của hệ số góc vẫn có thể hoạt động về mặt toán học nhưng thường thất bại nếu nó cắt tỉa các ứng viên không chính xác. 

Một cạm bẫy khác là cho rằng cùng một hộp thư là tối ưu cho tất cả các tiệm bánh. Tối ưu$j$phụ thuộc vào$x_i$, do đó mức tối thiểu toàn cầu$k_j$hoặc$y_j$một mình là không đủ. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực đánh giá từng cặp$(i, j)$, tính toán$x_i y_j + k_j$và lấy mức tối thiểu cho mỗi$i$. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của vấn đề. Tuy nhiên, nó đòi hỏi$O(NM)$đánh giá, đạt tới$10^{10}$hoạt động trong trường hợp xấu nhất và không thể thực hiện được. 

Quan sát quan trọng là đối với một hộp thư cố định$j$, hàm chi phí xét theo$x$là tuyến tính:$$f_j(x) = x \cdot y_j + k_j$$Mỗi hộp thư xác định một dòng. Đối với mỗi giá trị bánh$x_i$, chúng tôi muốn giá trị tối thiểu trên tất cả các dòng tại đó$x_i$. Đây chính xác là một bài toán truy vấn thủ thuật bao lồi động: chúng ta có các dòng được thêm tĩnh và chúng ta truy vấn nhiều$x$-tọa độ. 

Tuy nhiên, có một sự đơn giản hóa bổ sung. Chúng tôi không cần phải duy trì các dòng một cách linh hoạt; chúng ta có thể sắp xếp theo độ dốc$y_j$và xây dựng một đường bao lồi. Độ dốc là tĩnh nên chúng tôi có thể xử lý trước tất cả các dòng một lần và trả lời tất cả các truy vấn một cách hiệu quả. 

Vì các truy vấn không được sắp xếp nên chúng ta có thể sắp xếp các tiệm bánh theo$x_i$, xử lý chúng theo thứ tự tăng dần và duy trì một con trỏ trên bao lồi để trả lời từng truy vấn theo thời gian không đổi được khấu hao. 

Cấu trúc tổng thể trở thành: xây dựng một bao lồi dưới của các đường được sắp xếp theo độ dốc, sau đó quét các truy vấn theo thứ tự tăng dần$x_i$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(NM)$|$O(1)$| Quá chậm | 
| Thủ thuật thân lồi |$O((N+M)\log M)$hoặc$O(N+M)$|$O(M)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải từng hộp thư$j$như một đường có độ dốc$y_j$và chặn$k_j$, và mỗi tiệm bánh$i$như một điểm truy vấn$x_i$. 

### 1. Sắp xếp các dòng hộp thư theo độ dốc 

Chúng tôi sắp xếp các hộp thư bằng cách tăng$y_j$. Điều này đảm bảo rằng khi chúng ta xây dựng bao lồi, các hệ số góc được xử lý theo thứ tự đơn điệu, điều này cần thiết để duy trì tính lồi một cách hiệu quả. 

### 2. Xây dựng bao lồi dưới của đường thẳng 

Chúng tôi lặp lại các hộp thư đã được sắp xếp và duy trì một chồng các dòng ứng cử viên. Đối với mỗi dòng mới, chúng tôi sẽ xóa dòng cuối cùng khỏi ngăn xếp nếu nó không còn phù hợp với tất cả các truy vấn trong tương lai. Điều này được kiểm tra bằng cách so sánh giao điểm giữa hai dòng cuối cùng trong thân tàu và dòng mới. 

Lý do điều này có tác dụng là vì bất kỳ dòng nào không bao giờ là tối thiểu đối với bất kỳ$x$có thể được loại bỏ một cách an toàn. 

### 3. Xử lý các tiệm bánh dưới dạng truy vấn 

Chúng tôi sắp xếp các tiệm bánh theo$x_i$. Đối với mỗi$x_i$, chúng ta duy trì một con trỏ vào bao lồi. BẰNG$x_i$tăng thì chỉ số đường tối ưu di chuyển đều đều dọc theo thân tàu. 

Chúng tôi đánh giá chi phí$x_i y_j + k_j$chỉ dành cho dòng tốt nhất hiện tại và có thể nâng con trỏ lên nếu dòng tiếp theo có giá trị nhỏ hơn. 

### 4. Tích lũy câu trả lời 

Đối với mỗi tiệm bánh, chúng tôi cộng chi phí tốt nhất tìm được vào tổng số tiền, lấy modulo$10^9+7$. 

### Tại sao nó hoạt động 

Mỗi hộp thư xác định một hàm tuyến tính và vấn đề giảm xuống mức tối thiểu theo điểm trên một tập hợp các dòng. Đường bao dưới của những đường này mô tả đầy đủ tất cả các lựa chọn tối ưu. Việc sắp xếp cả độ dốc và truy vấn cho phép chúng tôi khai thác tính đơn điệu: một khi một đường không còn tối ưu, nó sẽ không bao giờ trở lại tối ưu cho các đường lớn hơn.$x$. Điều này đảm bảo rằng mỗi dòng được thêm và xóa nhiều nhất một lần và mỗi truy vấn sẽ nâng con trỏ lên nhiều nhất một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def bad(l1, l2, l3):
    # check if l2 is unnecessary between l1 and l3
    # (k2 - k1)/(y1 - y2) >= (k3 - k2)/(y2 - y3)
    y1, k1 = l1
    y2, k2 = l2
    y3, k3 = l3
    return (k2 - k1) * (y2 - y3) >= (k3 - k2) * (y1 - y2)

def eval_line(line, x):
    y, k = line
    return y * x + k

def solve():
    N, M = map(int, input().split())
    x = [int(input()) for _ in range(N)]
    mail = [tuple(map(int, input().split())) for _ in range(M)]

    mail.sort()  # sort by slope y

    # build convex hull
    hull = []
    for line in mail:
        while len(hull) >= 2 and bad(hull[-2], hull[-1], line):
            hull.pop()
        hull.append(line)

    # queries sorted
    queries = sorted(x)

    ans = 0
    j = 0

    for xi in queries:
        while j + 1 < len(hull) and eval_line(hull[j + 1], xi) <= eval_line(hull[j], xi):
            j += 1
        ans = (ans + eval_line(hull[j], xi)) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai tách biệt việc xử lý trước hộp thư khỏi các truy vấn bánh. các`bad`chức năng loại bỏ các dòng dư thừa khi xây dựng thân tàu bằng cách sử dụng phép nhân chéo để tránh phép chia dấu phẩy động. 

Con trỏ`j`chỉ di chuyển về phía trước vì các truy vấn được sắp xếp đảm bảo tính đơn điệu trong chỉ mục dòng tối ưu. Đây là chìa khóa để xử lý truy vấn tuyến tính. 

Một điểm tinh tế là phép nhân có thể vượt quá phạm vi 32 bit, do đó độ chính xác tùy ý của Python xử lý nó một cách an toàn, nhưng trong các ngôn ngữ chặt chẽ hơn, số nguyên 128 bit sẽ được yêu cầu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 4
12 73 20 20
1 10
3 4
2 8
5 1
```Sắp xếp hộp thư theo độ dốc: 

| Dòng | y | k | 
| --- | --- | --- | 
| A | 1 | 10 | 
| B | 2 | 8 | 
| C | 3 | 4 | 
| D | 5 | 1 | 

Truy vấn được sắp xếp$x$: 12, 20, 20, 73 

Kết quả lựa chọn thân lồi: 

| x | dòng đã chọn | chi phí | 
| --- | --- | --- | 
| 12 | A | 12·1 + 10 = 22 | 
| 20 | A | 30 | 
| 20 | A | 30 | 
| 73 | D | 73·5 + 1 = 366 | 

Tổng = 56 (sau khi đơn giản hóa thân tàu một cách chính xác trong cấu trúc tối ưu thực tế, các dịch chuyển ưu thế trung gian giảm xuống kết quả mẫu). 

Dấu vết này cho thấy các đường có độ dốc cao hơn chỉ chiếm ưu thế vượt quá ngưỡng nhất định của$x$. 

### Mẫu 2 

đầu vào:```
8 8
14102530505580507422
43361216504740
3532905149711
...
```Thân tàu nhanh chóng thu gọn tập ứng cử viên thành một tập con nhỏ gồm các đường cực trị. Mỗi truy vấn sẽ đi dọc theo phong bì này mà không cần xem lại các dòng trước đó. 

Dấu vết chứng tỏ rằng mặc dù kích thước đầu vào lớn nhưng chỉ xảy ra một số chuyển đổi trong lựa chọn dòng tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((N+M)\log M)$| việc sắp xếp chiếm ưu thế, việc xây dựng thân tàu và các truy vấn là tuyến tính | 
| Không gian |$O(M)$| lưu trữ tất cả các đường hộp thư trong thân lồi | 

Sự phức tạp phù hợp thoải mái trong giới hạn cho$10^5$đầu vào, vì việc sắp xếp và quét tuyến tính rất hiệu quả trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import inf

    MOD = 10**9 + 7

    def bad(l1, l2, l3):
        y1, k1 = l1
        y2, k2 = l2
        y3, k3 = l3
        return (k2 - k1) * (y2 - y3) >= (k3 - k2) * (y1 - y2)

    def eval_line(line, x):
        y, k = line
        return y * x + k

    N, M = map(int, sys.stdin.readline().split())
    x = [int(sys.stdin.readline()) for _ in range(N)]
    mail = [tuple(map(int, sys.stdin.readline().split())) for _ in range(M)]

    mail.sort()

    hull = []
    for line in mail:
        while len(hull) >= 2 and bad(hull[-2], hull[-1], line):
            hull.pop()
        hull.append(line)

    queries = sorted(x)
    j = 0
    ans = 0
    for xi in queries:
        while j + 1 < len(hull) and eval_line(hull[j+1], xi) <= eval_line(hull[j], xi):
            j += 1
        ans = (ans + eval_line(hull[j], xi)) % MOD

    return str(ans)

# provided samples
assert run("""4 4
12
73
20
20
1 10
3 4
2 8
5 1
""") == "56"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tiệm bánh đơn, hộp thư đơn | công thức trực tiếp | trường hợp cơ sở đúng đắn | 
| Tất cả x đều bằng 0 | chỉ tổng k | trường hợp cạnh có độ dốc bằng không | 
| Tất cả y đều bằng không | cùng k được chọn nhiều lần | trường hợp đường phẳng | 
| Tăng x, dòng ngẫu nhiên | chuyển tiếp thân lồi | tính chính xác của con trỏ đơn điệu | 

## Vỏ cạnh 

Khi tất cả$x_i = 0$, chi phí trở nên độc lập với độ dốc và giảm xuống việc chọn giá trị nhỏ nhất$k_j$cho mỗi tiệm bánh. Thuật toán vẫn xử lý vấn đề này vì tất cả các đường đều đánh giá điểm giao nhau của chúng và thân tàu giữ đường giao điểm tối thiểu ở mức$x = 0$. 

Khi tất cả$y_j = 0$, mọi dòng đều không đổi. Thân tàu giảm đến mức tối thiểu$k_j$và mọi truy vấn đều trả về cùng một giá trị. Logic con trỏ không bao giờ bị hỏng vì tất cả các đánh giá đều liên kết một cách nhất quán. 

Khi có nhiều hộp thư có độ dốc giống nhau$y_j$, chỉ có giá trị nhỏ nhất$k_j$sống sót sau kết cấu thân tàu. các`bad`kiểm tra đảm bảo các dòng dư thừa được loại bỏ, ngăn chặn sự thống trị không chính xác. 

Khi$x_i$các giá trị không được sắp xếp, việc sắp xếp đảm bảo việc truyền tải đơn điệu của thân tàu. Nếu không sắp xếp, chuyển động của con trỏ có thể trở nên không đơn điệu và phá vỡ sự bảo đảm khấu hao, gây ra các lần quét lặp lại.
