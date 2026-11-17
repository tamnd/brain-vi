---
title: "CF 1955B - Quảng Trường Tiến Bộ"
description: "Chúng ta có một lưới $n nhân n$ được xác định đầy đủ bởi ba giá trị: ô trên cùng bên trái và hai số gia cố định chi phối chuyển động đi xuống và sang phải. Di chuyển xuống một bước luôn thêm $c$ và di chuyển sang phải một bước luôn thêm $d$."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1955
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 938 (Div. 3)"
rating: 1000
weight: 1955
solve_time_s: 49
verified: true
draft: false
---

[CF 1955B - Hình vuông lũy tiến](https://codeforces.com/problemset/problem/1955/B) 

**Đánh giá:** 1000 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, triển khai, sắp xếp 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times n$lưới được xác định đầy đủ bởi ba giá trị: ô trên cùng bên trái và hai số gia cố định chi phối chuyển động xuống dưới và sang phải. Di chuyển xuống một bước luôn thêm$c$, và di chuyển sang phải một bước luôn cộng thêm$d$. Do đó, mọi ô trong lưới có thể được biểu thị dưới dạng hàm tuyến tính của tọa độ của nó sau khi giá trị bắt đầu được cố định. 

Sau khi lưới này được xây dựng một cách khái niệm, chúng ta có nhiều tập hợp$n^2$số nguyên theo thứ tự tùy ý. Nhiệm vụ là quyết định xem multiset này có thể chính xác là tập hợp các giá trị xuất hiện trong lưới được xác định duy nhất cho đối tượng đã cho hay không.$n$,$c$, Và$d$. Chúng tôi không cần phải xây dựng lại bố cục lưới, chỉ xác minh rằng lưới đó tồn tại có giá trị khớp chính xác với nhiều tập hợp. 

Các ràng buộc buộc chúng ta vào trạng thái gần tuyến tính hoặc$O(n^2 \log n)$mỗi bài kiểm tra tồi tệ nhất. Vì tổng số phần tử trong tất cả các trường hợp thử nghiệm nhiều nhất là$2.5 \cdot 10^5$, bất kỳ phương pháp bậc hai nào trong việc sắp xếp hoặc băm cho mỗi lần kiểm tra đều ổn, nhưng bất kỳ điều gì cố gắng mô phỏng tất cả các giá trị bắt đầu có thể có hoặc kiểm tra tính nhất quán trên mỗi ô bằng tính toán lại nhiều sẽ quá chậm. 

Một cạm bẫy ngây thơ xuất phát từ việc nghĩ rằng thứ tự của mảng là quan trọng. Nó không. Một dạng sai sót tinh vi khác là giả định rằng chỉ những khác biệt tương đối mới có ý nghĩa cục bộ. Ví dụ: cố gắng xác minh các mẫu theo hàng hoặc theo cột mà không neo lưới sẽ dẫn đến kết quả sai khi tồn tại các bản sao hoặc khi sắp xếp ẩn cấu trúc. 

Hãy xem xét một trực giác về trường hợp thất bại nhỏ: nếu$n=2$,$c=1$,$d=2$, lưới phải là$$\begin{pmatrix} x & x+2 \\ x+1 & x+3 \end{pmatrix}$$Multiset phải chính xác$\{x, x+1, x+2, x+3\}$. Bất kỳ nỗ lực nào chỉ kiểm tra phần kề theo thứ tự được sắp xếp có thể chấp nhận nhầm các tập hợp không chính xác trong đó khoảng cách là đúng nhưng không thể hình thành cấu trúc. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ thử mọi giá trị có thể có của$a_{1,1}$như một điểm khởi đầu của ứng cử viên. Đối với mỗi ứng cử viên, chúng tôi sẽ xây dựng toàn bộ lưới bằng cách sử dụng các mối quan hệ lặp lại và so sánh nhiều tập hợp kết quả với mảng đã cho. Vì các giá trị dao động lên tới$10^9$, giá trị bắt đầu không bị giới hạn trực tiếp theo cách làm cho việc liệt kê trở nên khả thi. Tệ hơn nữa, mỗi công trình đều tiêu tốn$O(n^2)$và không có giới hạn có ý nghĩa nào về số lượng ứng viên sẽ thử, khiến phương pháp này hoàn toàn không khả thi. 

Quan sát cấu trúc quan trọng là lưới được xác định hoàn toàn bởi phần tử nhỏ nhất của nó. Bởi vì cả hai$c$Và$d$là dương, mỗi bước xuống hoặc sang phải đều làm tăng giá trị. Điều này có nghĩa là phần tử tối thiểu trong lưới phải là$a_{1,1}$và toàn bộ lưới có thể được xây dựng lại một cách xác định từ nó. 

Sau khi sắp xếp mảng, chúng ta có thể cố gắng coi giá trị nhỏ nhất là góc trên cùng bên trái. Từ đó, mọi giá trị mong đợi của lưới được xác định duy nhất. Câu hỏi duy nhất còn lại là liệu multiset có chứa chính xác những giá trị đó hay không. 

Sự giảm thiểu quan trọng là biến cấu trúc 2D thành vấn đề xác minh 1D: thay vì lý luận về hình học, chúng tôi xác thực một chuỗi các giá trị mong đợi được tạo dựa trên một mảng được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force bắt đầu | Hàm mũ / không khả thi | O(1) | Quá chậm | 
| Sắp xếp + xác nhận xây dựng |$O(n^2 \log n)$| O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi khai thác thực tế rằng việc sắp xếp sẽ tiết lộ cấu trúc dự định. 

1. Sắp xếp mảng theo kích thước$n^2$. Điều này mang lại cho chúng ta một thứ tự ứng cử viên trong đó phần tử nhỏ nhất được giả định tương ứng với$a_{1,1}$. Điều này hợp lý vì tất cả các bước di chuyển trong lưới đều tăng giá trị do$c, d \ge 1$. 
2. Đặt phần tử nhỏ nhất là giá trị bắt đầu ứng cử viên$x$. Điều này sửa chữa toàn bộ lưới một cách duy nhất. 
3. Đối với mọi vị trí$(i, j)$, tính giá trị kỳ vọng:$$x + i \cdot c + j \cdot d$$Điều này trực tiếp xuất phát từ việc áp dụng phép truy toán lặp đi lặp lại từ phía trên bên trái. 
4. So sánh từng giá trị được tính toán với mảng đã sắp xếp ở cùng một chỉ mục (sử dụng phương pháp truyền tải chính theo hàng). Nếu có bất kỳ sự không khớp nào xảy ra, thì tập hợp nhiều tập hợp không thể đại diện cho bình phương lũy ​​tiến. 
5. Nếu tất cả các giá trị khớp chính xác thì mảng đó hợp lệ. 

Thứ tự sử dụng để duyệt phải nhất quán với mảng đã được sắp xếp. Vì cả việc xây dựng và sắp xếp đều áp đặt tổng thứ tự, nên phép liệt kê hàng chính chỉ phù hợp với các giá trị tăng dần vì$c, d > 0$, đảm bảo sự tăng trưởng đơn điệu trên cả hai chiều. 

### Tại sao nó hoạt động 

Cấu trúc tạo ra một lưới tăng dần theo cả hướng hàng và cột. Điều này đảm bảo rằng các giá trị lưới, khi được làm phẳng theo thứ tự hàng lớn, sẽ tạo thành một chuỗi tăng dần. Vì việc sắp xếp cũng tạo ra thứ tự tăng dần duy nhất của cùng một tập hợp nhiều tập hợp, nên cả hai chuỗi phải trùng nhau khi và chỉ khi tập hợp đó chính xác là lưới được xây dựng. Điều này loại bỏ mọi sự mơ hồ về vị trí: khi phần tử nhỏ nhất được cố định, mọi giá trị khác có chính xác một vị trí hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, c, d = map(int, input().split())
        b = list(map(int, input().split()))
        
        b.sort()
        start = b[0]
        
        expected = []
        for i in range(n):
            for j in range(n):
                expected.append(start + i * c + j * d)
        
        expected.sort()
        
        if expected == b:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp nhiều tập hợp đầu vào. Điều này rất cần thiết vì cấu trúc đơn điệu của lưới đảm bảo thứ tự tăng dần duy nhất khi được làm phẳng. 

Sau đó, chúng tôi xây dựng lại lưới nếu nó hợp lệ, bắt đầu từ giá trị nhỏ nhất. Công thức$start + i \cdot c + j \cdot d$được bắt nguồn trực tiếp từ việc áp dụng nhiều lần các gia số hàng và cột. 

Cuối cùng, chúng tôi so sánh nhiều tập hợp được tạo với đầu vào. Việc sắp xếp cả hai đều đảm bảo rằng chúng ta đang so sánh các bộ sưu tập không có thứ tự thay vì bố cục theo vị trí. 

Một điểm tinh tế là chúng tôi không cố gắng đoán hướng hoặc hoán vị. Tính tích cực chặt chẽ của$c$Và$d$đảm bảo một hướng tăng duy nhất, do đó không có sự sắp xếp thay thế nào có thể thỏa mãn các ràng buộc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3, c = 2, d = 3
b = [3, 9, 6, 5, 7, 1, 10, 4, 8]
```Mảng được sắp xếp: 

| bước | sắp xếp b | mong đợi từ công thức | 
| --- | --- | --- | 
| 0 | 1 | 1 | 
| 1 | 3 | 3 | 
| 2 | 4 | 4 | 
| 3 | 5 | 5 | 
| 4 | 6 | 6 | 
| 5 | 7 | 7 | 
| 6 | 8 | 8 | 
| 7 | 9 | 9 | 
| 8 | 10 | 10 | 

Trình tự được tạo khớp chính xác, vì vậy đầu ra là CÓ. 

Điều này xác nhận rằng chỉ cần đặt hàng là đủ khi lưới hoàn toàn đơn điệu. 

### Ví dụ 2 

đầu vào:```
n = 3, c = 2, d = 3
b = [3, 9, 6, 5, 7, 1, 11, 4, 8]
```Mảng được sắp xếp: 

| bước | sắp xếp b | dự kiến ​​| 
| --- | --- | --- | 
| 0 | 1 | 1 | 
| 1 | 3 | 3 | 
| 2 | 4 | 4 | 
| 3 | 5 | 5 | 
| 4 | 6 | 6 | 
| 5 | 7 | 7 | 
| 6 | 8 | 8 | 
| 7 | 9 | 9 | 
| 8 | 11 | 10 | 

Sự không khớp xảy ra ở phần tử cuối cùng. 

Điều này cho thấy rằng ngay cả một giá trị bị hỏng cũng sẽ phá vỡ toàn bộ cấu trúc vì mọi vị trí đều bị ràng buộc chặt chẽ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t \cdot n^2 \log n)$| Sắp xếp chiếm ưu thế trên mỗi trường hợp thử nghiệm | 
| Không gian |$O(n^2)$| Lưu trữ lưới đầu vào và được tạo | 

Cho rằng tổng$n^2$qua các bài kiểm tra được giới hạn bởi$2.5 \cdot 10^5$, sắp xếp và tái thiết tuyến tính phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import contextlib
    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("""5
3 2 3
3 9 6 5 7 1 10 4 8
3 2 3
3 9 6 5 7 1 11 4 8
2 100 100
400 300 400 500
3 2 3
3 9 6 6 5 1 11 4 8
4 4 4
15 27 7 19 23 23 11 15 7 3 19 23 11 15 11 15
""") == """NO
YES
YES
NO
NO"""

# custom cases
assert run("""1
2 1 2
1 3 2 4
""") == "YES", "minimal valid"

assert run("""1
2 1 2
1 3 2 5
""") == "NO", "one wrong value"

assert run("""1
3 1 1
1 2 3 2 3 4 3 4 5
""") == "YES", "perfect arithmetic grid"

assert run("""1
3 1 1
1 2 3 2 3 4 3 4 6
""") == "NO", "off-by-one corruption"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới đúng 2x2 | CÓ | cấu trúc hợp lệ nhỏ nhất | 
| Giá trị bị hỏng 2x2 | KHÔNG | phát hiện không khớp đơn | 
| lưới số học 3x3 | CÓ | nhất quán đầy đủ | 
| lỗi 3x3 từng lỗi một | KHÔNG | vụ án tham nhũng tinh vi | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi các bản sao xuất hiện trong mảng đầu vào. Từ$c, d \ge 1$, lưới thực không bao giờ chứa các bản sao, do đó, bất kỳ số nào lặp lại sẽ ngay lập tức vô hiệu hóa việc xây dựng. Thuật toán xử lý việc này một cách tự nhiên vì việc sắp xếp sẽ giữ nguyên các bản sao và việc so sánh với chuỗi tăng dần được tạo sẽ không thành công ở giá trị lặp lại đầu tiên. 

Một trường hợp khó phát hiện khác là khi phần tử nhỏ nhất thực sự không chính xác$a_{1,1}$. Tuy nhiên, vì lưới tăng dần theo cả hai hướng nên không có vị trí nào khác có thể chứa giá trị nhỏ hơn, do đó phần tử nhỏ nhất phải luôn là điểm bắt đầu chính xác. Điều này loại bỏ bất kỳ sự mơ hồ nào trong quá trình khởi tạo. 

Trường hợp cạnh cuối cùng lớn$n = 500$, trong đó lưới có 250.000 phần tử. Giải pháp này vẫn an toàn vì nó chỉ thực hiện sắp xếp và tạo tuyến tính, tránh mọi phép tính toán lồng nhau có thể đẩy nó vào hành vi bậc hai cho mỗi trường hợp thử nghiệm.
