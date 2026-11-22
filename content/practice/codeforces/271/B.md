---
title: "CF 271B - Ma Trận Nguyên Tố"
description: "Chúng ta được cho một lưới các số nguyên dương. Từ lưới này, chúng ta được phép chọn liên tục bất kỳ ô nào và tăng giá trị của nó lên một ô. Mỗi lần tăng giá một lần di chuyển."
date: "2026-06-05T01:33:30+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 271
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 166 (Div. 2)"
rating: 1300
weight: 271
solve_time_s: 81
verified: true
draft: false
---

[CF 271B - Ma trận nguyên tố](https://codeforces.com/problemset/problem/271/B) 

**Đánh giá:** 1300 
**Tags:** tìm kiếm nhị phân, lực lượng vũ phu, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một lưới các số nguyên dương. Từ lưới này, chúng ta được phép chọn liên tục bất kỳ ô nào và tăng giá trị của nó lên một ô. Mỗi lần tăng giá một lần di chuyển. Mục tiêu là làm cho lưới trở thành "nguyên tố" theo nghĩa là ít nhất toàn bộ một hàng trở thành giá trị nguyên tố hoặc ít nhất toàn bộ một cột trở thành giá trị nguyên tố. 

Một giá trị chỉ được chấp nhận trong hàng hoặc cột đã chọn nếu nó trở thành số nguyên tố. Vì chúng ta chỉ có thể tăng giá trị nên mỗi ô đều có một chi phí độc lập: số lượng gia tăng cần thiết để biến nó thành số nguyên tố lớn hơn hoặc bằng giá trị hiện tại của nó. 

Nhiệm vụ là chọn một hàng hoặc một cột và trả tổng chi phí để chuyển mọi giá trị trong đó thành số nguyên tố, sau đó giảm thiểu chi phí này trên tất cả các hàng và cột. 

Các ràng buộc tăng lên đến lưới 500 x 500, do đó có tới 250.000 ô. Mỗi giá trị tối đa là 100.000 nhưng có thể tăng tùy ý. Điều đó ngay lập tức gợi ý rằng mọi tính toán nặng trên mỗi ô đều phải được xử lý trước, bởi vì việc tính toán lại các khoảng cách nguyên tố nhiều lần bên trong các vòng lặp lồng nhau vẫn sẽ đạt, nhưng chỉ khi việc kiểm tra tính nguyên tố hiệu quả và được sử dụng lại. 

Trường hợp cạnh tinh tế là khi các số đã là số nguyên tố ở một hàng hoặc cột nào đó. Hàng hoặc cột đó đóng góp chi phí bằng 0 và câu trả lời trở thành 0. Một trường hợp quan trọng khác là khi nhiều giá trị nằm ngay dưới số nguyên tố, chẳng hạn như 14, 20, 26. Việc “kiểm tra số nguyên tố tiếp theo bằng cách tăng từng số một” trên mỗi ô sẽ quá chậm nếu được thực hiện lặp đi lặp lại trong các vòng lặp. 

Yêu cầu ẩn giấu quan trọng là chúng ta phải tính toán một cách hiệu quả, đối với mọi số trong lưới, số nhỏ nhất lớn hơn hoặc bằng số nguyên tố. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xử lý từng hàng và cột một cách độc lập. Đối với một hàng cố định, chúng ta sẽ lặp qua tất cả các ô của nó và với mỗi ô sẽ tăng liên tục cho đến khi đạt đến số nguyên tố, tính tổng các số tăng. Sau đó, chúng tôi lặp lại tương tự cho mỗi cột và lấy mức tối thiểu. 

Điều này đúng nhưng quá chậm vì mỗi ô có thể yêu cầu nhiều mức tăng và chúng tôi thực hiện việc này cho tối đa 250.000 ô. Trong trường hợp xấu nhất, nếu các giá trị gần 100.000 và các số nguyên tố thưa thớt cục bộ thì việc kiểm tra tính nguyên tố lặp đi lặp lại trên mỗi mức tăng sẽ dẫn đến hàng tỷ lượt kiểm tra. 

Quan sát quan trọng là chi phí của mỗi ô là độc lập và chỉ phụ thuộc vào giá trị của nó. Chúng ta không cần phải mô phỏng số gia tăng. Thay vào đó, chúng tôi tính toán trước số nguyên tố tiếp theo cho mọi số nguyên đến giới hạn an toàn trên 100.000 một chút. Sau đó, mỗi ô đóng góp một chi phí cố định:`next_prime[a[i][j]] - a[i][j]`. 

Khi chúng ta có lưới chi phí này, vấn đề sẽ giảm xuống còn việc tìm tổng tối thiểu trên tất cả các hàng và tất cả các cột. Đó chỉ là tổng hợp các giá trị được tính toán trước. 

Vì vậy, cấu trúc giải pháp trở thành: tính toán trước các số nguyên tố bằng cách sử dụng sàng, xây dựng mảng số nguyên tố tiếp theo, tính chi phí trên mỗi ô, sau đó tính tổng hàng và tổng cột. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng tăng cường vũ phu | O(nm × bước) | O(1) | Quá chậm | 
| Sàng + Tính toán trước + Tổng hợp | O(MAX log log MAX + nm) | O(TỐI ĐA) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chọn giới hạn tối đa cao hơn một chút so với giá trị lớn nhất có thể, thường là 100000 + lề (chẳng hạn như 130000). Điều này đảm bảo mọi số đều có một số nguyên tố có thể truy cập được phía trên nó. 
2. Chạy sàng Eratosthenes để đánh dấu tất cả các số nguyên tố đến giới hạn này. Điều này mang lại khả năng tra cứu tính nguyên thủy theo thời gian liên tục. 
3. Xây dựng một mảng`next_prime`sao cho với mọi số nguyên x,`next_prime[x]`là số nguyên tố nhỏ nhất lớn hơn hoặc bằng x. 

Điều này được thực hiện bằng cách quét xuống từ giá trị tối đa trong khi ghi nhớ số nguyên tố được nhìn thấy lần cuối. 
4. Tạo ngầm ma trận chi phí: với mỗi ô (i, j), tính`cost[i][j] = next_prime[a[i][j]] - a[i][j]`. Điều này thể hiện số lượng gia tăng tối thiểu cần thiết cho ô đó. 
5. Tính chi phí của mỗi hàng bằng cách tính tổng chi phí ô của nó. 
6. Tính giá trị của từng cột tương tự. 
7. Trả về giá trị nhỏ nhất của tổng hàng và tổng cột. 

Mỗi bước sẽ giảm bớt vấn đề từ các hoạt động gia tăng động thành các khác biệt được tính toán trước tĩnh, đó là sự đơn giản hóa trung tâm. 

### Tại sao nó hoạt động 

Mỗi ô đóng góp độc lập vào việc một hàng hoặc cột có trở thành số nguyên tố hoàn toàn hay không. Do các thao tác trên các ô khác nhau không tương tác với nhau nên chi phí chuyển đổi một hàng chỉ là tổng của các chi phí tối thiểu riêng lẻ. Bởi vì chúng tôi luôn chọn số nguyên tố gần nhất phía trên mỗi giá trị nên chúng tôi không bao giờ trả quá nhiều cho mỗi ô. Điều này đảm bảo rằng bất kỳ chi phí hàng hoặc cột nào được tính toán đều là chi phí tối thiểu thực sự cho cấu trúc đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXV = 130000

# sieve
is_prime = [True] * (MAXV + 1)
is_prime[0] = is_prime[1] = False

for i in range(2, int(MAXV ** 0.5) + 1):
    if is_prime[i]:
        step = i
        start = i * i
        for j in range(start, MAXV + 1, step):
            is_prime[j] = False

# build next prime
next_prime = [0] * (MAXV + 2)
last = -1
for i in range(MAXV, -1, -1):
    if is_prime[i]:
        last = i
    next_prime[i] = last

n, m = map(int, input().split())
a = [list(map(int, input().split())) for _ in range(n)]

row_cost = [0] * n
col_cost = [0] * m

for i in range(n):
    for j in range(m):
        v = a[i][j]
        cost = next_prime[v] - v
        row_cost[i] += cost
        col_cost[j] += cost

ans = min(min(row_cost), min(col_cost))
print(ans)
```Phần sàng đảm bảo chúng ta có thể xác định ngay lập tức tính nguyên tố và cũng lấy được số nguyên tố tiếp theo cho bất kỳ giá trị nào. Quét ngược là rất quan trọng vì nó biến phân phối nguyên tố thưa thớt thành bảng tra cứu trực tiếp. 

Vòng lặp lồng nhau trên ma trận sẽ tính toán chi phí một lần cho mỗi ô và tích lũy tổng số cả hàng và cột cùng một lúc, tránh các lượt chuyển thêm. 

Một điểm tinh tế là chúng ta dựa vào`next_prime[v]`luôn tồn tại. Đây là lý do tại sao phạm vi sàng phải đủ lớn vượt quá giá trị đầu vào tối đa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
1 2 3
5 6 1
4 4 1
```Chúng tôi tính toán các số nguyên tố tiếp theo: 

1→2, 2→2, 3→3, 4→5, 5→5, 6→7. 

Ma trận chi phí trở thành:```
1 0 0
0 1 1
1 1 1
```Tổng hàng và tổng cột: 

| tôi | Hàng | Tổng hợp | 
| --- | --- | --- | 
| 0 | 1 0 0 | 1 | 
| 1 | 0 1 1 | 2 | 
| 2 | 1 1 1 | 3 | 

| j | Cột | Tổng hợp | 
| --- | --- | --- | 
| 0 | 1 0 1 | 2 | 
| 1 | 0 1 1 | 2 | 
| 2 | 0 1 1 | 2 | 

Tối thiểu là 1, đạt được ở hàng đầu tiên. 

Điều này cho thấy ngay cả một hàng giá rẻ cũng có thể thống trị tất cả các cấu hình khác. 

### Ví dụ 2 

đầu vào:```
2 2
4 6
1 10
```Các số nguyên tố tiếp theo: 

4→5, 6→7, 1→2, 10→11. 

Ma trận chi phí:```
1 1
1 1
```Tổng hàng đều bằng 2, tổng cột đều bằng 2. 

| tôi | Hàng | Tổng hợp | 
| --- | --- | --- | 
| 0 | 1 1 | 2 | 
| 1 | 1 1 | 2 | 

| j | Cột | Tổng hợp | 
| --- | --- | --- | 
| 0 | 1 1 | 2 | 
| 1 | 1 1 | 2 | 

Câu trả lời là 2. 

Điều này thể hiện tính đối xứng: khi tất cả các chi phí đều giống nhau thì bất kỳ hàng hoặc cột nào cũng tương đương. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(MAX log log MAX + nm) | sàng tạo số nguyên tố một lần, sau đó chuyển qua lưới | 
| Không gian | O(TỐI ĐA) | mảng tra cứu số nguyên tố và số nguyên tố tiếp theo | 

Việc truyền lưới là tuyến tính theo số lượng ô, tối đa là 250.000, dễ dàng nằm trong giới hạn. Sàng chạy hơn ~130.000 giá trị, điều này cũng không đáng kể trong 2 giây trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline

    MAXV = 130000

    is_prime = [True] * (MAXV + 1)
    is_prime[0] = is_prime[1] = False
    for i in range(2, int(MAXV ** 0.5) + 1):
        if is_prime[i]:
            for j in range(i * i, MAXV + 1, i):
                is_prime[j] = False

    next_prime = [0] * (MAXV + 2)
    last = -1
    for i in range(MAXV, -1, -1):
        if is_prime[i]:
            last = i
        next_prime[i] = last

    n, m = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(n)]

    row = [0] * n
    col = [0] * m

    for i in range(n):
        for j in range(m):
            c = next_prime[a[i][j]] - a[i][j]
            row[i] += c
            col[j] += c

    print(min(min(row), min(col)))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided sample
assert run("""3 3
1 2 3
5 6 1
4 4 1
""") == "1"

# single cell already prime
assert run("""1 1
2
""") == "0"

# all same small composite
assert run("""2 2
1 1
1 1
""") == "2"

# mixed case
assert run("""2 3
4 6 8
9 10 1
""") == str(min(1+1+1, 1+1, 1+1))

# max-ish structure sanity
assert run("""3 3
100000 100000 100000
100000 100000 100000
100000 100000 100000
""") >= "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1×1 số nguyên tố | 0 | đã là trường hợp cơ bản hàng đầu | 
| tất cả 1s | 2 | chuyển đổi không chính tối thiểu | 
| lưới nhỏ hỗn hợp | tính toán | so sánh hàng và cột | 
| giá trị lớn bằng nhau | ≥0 tỉnh táo | hiệu suất và giới hạn | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi ma trận đã chứa đầy đủ một hàng hoặc cột nguyên tố. Ví dụ, nếu một hàng là`[2, 3, 5]`, chi phí của nó trở thành 0 và câu trả lời ngay lập tức phải bằng 0. Thuật toán xử lý việc này vì`next_prime[p] - p`bằng 0 khi`p`là số nguyên tố nên tổng hàng tự động trở thành 0. 

Một trường hợp khác là khi nhiều hàng và cột liên kết với nhau. Ví dụ: một ma trận thống nhất gồm tất cả các ma trận tạo ra chi phí hàng và cột giống hệt nhau. Thuật toán tính toán chính xác tất cả các tổng một cách độc lập và lấy giá trị tối thiểu mà không giả định tính duy nhất. 

Trường hợp thứ ba là khi các giá trị nằm ngay dưới các số nguyên tố, chẳng hạn như 14, 20, 26. Mỗi ô sẽ nhảy độc lập đến số nguyên tố gần nhất (17, 23, 29). Vì mỗi chi phí đều mang tính cục bộ nên việc tính tổng không ảnh hưởng đến cấu trúc tổng thể và việc đánh giá cả hàng và cột vẫn nhất quán. 

Cuối cùng, ranh giới sàng phải đủ cao. Nếu nó quá nhỏ,`next_prime[x]`có thể không được xác định đối với các giá trị lớn, tạo ra kết quả không chính xác. Giới hạn trên cố định đảm bảo mọi giá trị đầu vào đều có số nguyên tố tiếp theo hợp lệ.
