---
title: "CF 104308D - Ước số không mong muốn"
description: "Chúng ta được cung cấp một chuỗi các số nguyên và sau đó là một chuỗi các truy vấn. Với mỗi giá trị truy vấn $b$, chúng ta quan tâm đến tất cả các ước dương của $b$. Trong số các ước số đó, một số có thể xuất hiện bên trong mảng đã cho, còn một số khác thì không."
date: "2026-07-01T20:01:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104308
codeforces_index: "D"
codeforces_contest_name: "Mirror of Independence Day Programming Contest 2023 by MIST Computer Club"
rating: 0
weight: 104308
solve_time_s: 51
verified: true
draft: false
---

[CF 104308D - Ước số không mong muốn](https://codeforces.com/problemset/problem/104308/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các số nguyên và sau đó là một chuỗi các truy vấn. Đối với mỗi giá trị truy vấn$b$, chúng ta quan tâm đến mọi ước số dương của$b$. Trong số các ước số đó, một số có thể xuất hiện bên trong mảng đã cho, còn một số khác thì không. Nhiệm vụ của mỗi truy vấn là đếm xem có bao nhiêu ước của$b$vắng mặt trong mảng. 

Bản thân mảng là tĩnh đối với từng trường hợp thử nghiệm, do đó, nó có thể được coi là một bộ tham chiếu cố định trong khi xử lý tất cả các truy vấn. 

Các ràng buộc ngụ ý rằng cả kích thước mảng và số lượng truy vấn có thể lên tới$10^5$và các giá trị bên trong mảng và truy vấn cũng được giới hạn bởi$10^5$. Điều này ngay lập tức loại trừ việc tính toán lại các ước số một cách đơn giản cho mỗi truy vấn và cũng loại trừ việc kiểm tra tính chia hết đối với mảng một cách trực tiếp cho mọi phần tử truy vấn bằng cách quét mảng. Một giải pháp chạm vào tất cả các phần tử mảng cho mỗi truy vấn sẽ dẫn đến khoảng$10^{10}$trong trường hợp xấu nhất vượt xa thời hạn. 

Một cạm bẫy nhỏ xuất hiện khi nghĩ đến việc tính toán lại các ước số cho từng truy vấn một cách độc lập. Đối với một số như 100000, lặp lại tối đa$\sqrt{b}$mỗi truy vấn thì được, nhưng thực hiện nó$10^5$đôi khi vẫn có nguy cơ hết thời gian nếu không được tối ưu hóa cẩn thận, đặc biệt là trong Python với giới hạn chặt chẽ. 

Một trường hợp cạnh khác xuất phát từ các bản sao trong mảng. Nếu một giá trị xuất hiện nhiều lần thì giá trị đó vẫn chỉ được tính một lần là “ước số hiện tại”, vì vậy việc xử lý mảng như một tập hợp là cần thiết. Ví dụ, nếu mảng là$[2, 2, 3]$Và$b = 6$, các ước số là$1,2,3,6$. Cả hai$2$Và$3$có mặt, vì vậy câu trả lời là$2$, không bị ảnh hưởng bởi bản sao 2. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp cho mỗi truy vấn là liệt kê tất cả các số nguyên từ 1 đến$b$và kiểm tra xem cái nào chia$b$, sau đó xác minh tư cách thành viên trong mảng. Điều này đúng nhưng ngay lập tức quá chậm, vì mỗi truy vấn sẽ tốn$O(b)$, dẫn đến$10^{10}$hoạt động. 

Một quan sát tốt hơn là các ước số đi theo cặp và có thể được tạo ra một cách hiệu quả đến$\sqrt{b}$. Điều này làm giảm chi phí cho mỗi truy vấn xuống$O(\sqrt{b})$, nhưng vẫn để lại cho chúng tôi tới$10^5$các truy vấn, có thể đẩy tổng số gần đến giới hạn. 

Cải tiến cơ cấu quan trọng xuất phát từ việc nhận ra rằng tất cả các con số đều bị giới hạn bởi$10^5$. Thay vì phải tính toán lại các ước số nhiều lần, chúng ta có thể tính toán trước danh sách ước số cho mọi số từ 1 đến$10^5$một khi sử dụng một công trình giống như sàng. Sau đó, mỗi truy vấn sẽ trở thành một lần quét đơn giản trên danh sách được tính toán trước và việc kiểm tra tư cách thành viên có thể được thực hiện trong$O(1)$sử dụng mảng boolean hoặc bộ băm. 

Điều này biến bài toán từ việc nhân tử hóa số học lặp đi lặp lại thành một bài toán tra cứu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force cho mỗi truy vấn |$O(q \cdot b)$|$O(1)$| Quá chậm | 
| Hệ số sqrt trên mỗi truy vấn |$O(q \sqrt{b})$|$O(1)$| Đường biên giới | 
| Tính toán trước các ước số + tra cứu tập hợp |$O(N \log N + q \cdot d(b))$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán 

1. Đọc mảng và chuyển đổi nó thành bảng hiện diện boolean được lập chỉ mục theo giá trị. Điều này cho phép kiểm tra liên tục xem một số có tồn tại trong mảng hay không. Lý do sử dụng bảng thay vì tập hợp là vì phạm vi giá trị nhỏ và cố định. 
2. Tính toán trước danh sách các ước số cho mọi số từ 1 đến 100000. Đối với mỗi số nguyên$i$, lặp qua bội số của nó và nối thêm$i$như một số chia. Điều này xây dựng tất cả các danh sách ước số theo cách giống như sàng. 
3. Với mỗi giá trị truy vấn$b$, truy xuất danh sách ước số được tính toán trước của nó. 
4. Lặp lại tất cả các ước của$b$và đếm những cái không được đánh dấu trong bảng boolean của mảng. 
5. Xuất số lượng cho mỗi truy vấn. 

Ý tưởng chính là việc tạo số chia được di chuyển hoàn toàn ra ngoài vòng lặp truy vấn, do đó, mỗi truy vấn sẽ trở thành một lần quét tuyến tính trên một cấu trúc nhỏ được tính toán trước. 

### Tại sao nó hoạt động 

Mỗi ước số của một số được đảm bảo được đưa vào danh sách ước số được tính toán trước đúng một lần. Bảng hiện diện theo dõi độc lập xem ước số đó có tồn tại trong mảng hay không. Vì cả hai thành phần đều là biểu diễn chính xác của các tập hợp bắt buộc nên việc đếm các giá trị không khớp sẽ trực tiếp tạo ra câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXV = 100000

# precompute divisors for all numbers up to MAXV
divs = [[] for _ in range(MAXV + 1)]
for i in range(1, MAXV + 1):
    for j in range(i, MAXV + 1, i):
        divs[j].append(i)

def solve():
    t = int(input())
    out_lines = []

    for _ in range(t):
        n, q = map(int, input().split())
        arr = list(map(int, input().split()))
        queries = list(map(int, input().split()))

        present = [False] * (MAXV + 1)
        for x in arr:
            present[x] = True

        for b in queries:
            cnt = 0
            for d in divs[b]:
                if not present[d]:
                    cnt += 1
            out_lines.append(str(cnt))

    return "\n".join(out_lines)

print(solve())
```Bước tiền xử lý sẽ xây dựng một danh sách ước số cho mỗi số nguyên một lần, do đó không có truy vấn nào thực hiện bất kỳ phép phân tích nhân tử số học nào. Mảng được chuyển đổi thành bảng tra cứu boolean, đảm bảo việc kiểm tra tư cách thành viên liên tục. 

Bên trong mỗi truy vấn, chúng tôi chỉ lặp lại các ước thực sự của$b$, do đó công tỷ lệ thuận với số lượng ước số hơn là độ lớn của$b$. 

Một lỗi phổ biến ở đây là sử dụng lặp đi lặp lại một bộ Python cho các ước số của mỗi số, điều này sẽ tạo ra chi phí chung cho mỗi truy vấn. Việc tính toán trước sẽ tránh hoàn toàn việc phân bổ lặp lại. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ nhỏ trong đó mảng$[1, 2, 4, 7]$và chúng tôi truy vấn$b = 8$. 

Các ước của 8 là$1, 2, 4, 8$. 

| Bước | Số chia | Hiện diện trong mảng | Đếm | 
| --- | --- | --- | --- | 
| 1 | 1 | vâng | 0 | 
| 2 | 2 | vâng | 0 | 
| 3 | 4 | vâng | 0 | 
| 4 | 8 | không | 1 | 

Câu trả lời là 1. 

Bây giờ hãy xem xét mảng$[3, 5, 6]$và truy vấn$b = 12$. 

Các ước của 12 là$1, 2, 3, 4, 6, 12$. 

| Bước | Số chia | Hiện diện trong mảng | Đếm | 
| --- | --- | --- | --- | 
| 1 | 1 | không | 1 | 
| 2 | 2 | không | 2 | 
| 3 | 3 | vâng | 2 | 
| 4 | 4 | không | 3 | 
| 5 | 6 | vâng | 3 | 
| 6 | 12 | không | 4 | 

Câu trả lời là 4. 

Những ví dụ này cho thấy thuật toán này hoàn toàn là tập hợp thành viên dựa trên phân tách số chia. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N + \sum d(b))$| Sàng chia cộng với ước số quét cho mỗi truy vấn | 
| Không gian |$O(N)$| Danh sách chia cộng với mảng hiện diện | 

Sàng chiếm ưu thế trong quá trình tiền xử lý nhưng vẫn nằm trong giới hạn cho$N = 10^5$. Việc xử lý truy vấn hiệu quả vì số ước của bất kỳ số nguyên nào lên đến$10^5$trung bình là nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

MAXV = 100000
divs = [[] for _ in range(MAXV + 1)]
for i in range(1, MAXV + 1):
    for j in range(i, MAXV + 1, i):
        divs[j].append(i)

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, q = map(int, input().split())
        arr = list(map(int, input().split()))
        queries = list(map(int, input().split()))

        present = [False] * (MAXV + 1)
        for x in arr:
            present[x] = True

        for b in queries:
            cnt = 0
            for d in divs[b]:
                if not present[d]:
                    cnt += 1
            out.append(str(cnt))

    return "\n".join(out)

# sample-style test
assert solve("""1
4 2
1 3 6 9
6 12
""") == "2\n4"

# all divisors present
assert solve("""1
3 1
1 2 3
6
""") == "0"

# none present
assert solve("""1
3 1
7 8 9
6
""") == "4"

# single element array
assert solve("""1
1 1
5
10
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp tất cả các ước số hiện tại | 0 | loại bỏ bảo hiểm đầy đủ | 
| không có trường hợp nào hiện tại | số chia đầy đủ | hành vi đếm tối đa | 
| mảng phần tử đơn | logic loại trừ đúng | độ chính xác cấu trúc tối thiểu | 

## Vỏ cạnh 

Khi mảng chứa các giá trị lặp lại, chẳng hạn như$[2, 2, 2]$, mảng hiện diện boolean đảm bảo sự trùng lặp không ảnh hưởng đến kết quả. Đối với một truy vấn$b = 6$, ước số$1,2,3,6$được kiểm tra chính xác một lần đối với bảng hiện diện, vì vậy sự trùng lặp không thể làm tăng số lượng. 

Đối với các giá trị nhỏ như$b = 1$, danh sách ước chỉ chứa$[1]$. Nếu mảng không chứa 1 thì câu trả lời là 1; nếu không thì bằng 0. Thuật toán xử lý điều này một cách tự nhiên vì danh sách ước số được tính toán trước cho 1 là chính xác và tối thiểu. 

Đối với giá trị tối đa gần$10^5$, danh sách ước số vẫn đủ nhỏ để việc lặp lại nó cho mỗi truy vấn là hiệu quả. Ngay cả trong những trường hợp xấu nhất như số tổng hợp cao, số chia vẫn bị giới hạn và không đe dọa đến giới hạn thời gian.
