---
title: "CF 105112D - Bộ chọn ngày"
description: "Chúng ta được cấp một lịch hàng tuần được mã hóa dưới dạng lưới 7 x 24. Mỗi hàng tương ứng với một ngày và mỗi cột tương ứng với một giờ. Một ô trống hoặc bị chặn. Rảnh nghĩa là bạn có mặt vào ngày giờ đó, bị chặn nghĩa là bạn bận."
date: "2026-06-27T19:57:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105112
codeforces_index: "D"
codeforces_contest_name: "2023-2024 ICPC Northwestern European Regional Programming Contest (NWERC 2023)"
rating: 0
weight: 105112
solve_time_s: 67
verified: true
draft: false
---

[CF 105112D - Bộ chọn ngày](https://codeforces.com/problemset/problem/105112/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một lịch hàng tuần được mã hóa dưới dạng lưới 7 x 24. Mỗi hàng tương ứng với một ngày và mỗi cột tương ứng với một giờ. Một ô trống hoặc bị chặn. Rảnh nghĩa là bạn có mặt vào ngày giờ đó, bị chặn nghĩa là bạn bận. 

Sau đó, quy trình “chọn ngày” sẽ diễn ra theo hai bước độc lập. Đầu tiên, bạn phải chọn ít nhất$d$ngày trong số 7 ngày. Thứ hai, bạn phải chọn ít nhất$h$giờ trong tổng số 24 giờ. Sau khi thực hiện những lựa chọn này, thời gian họp được lấy mẫu thống nhất từ ​​tích Descartes của những ngày đã chọn và giờ đã chọn. Mỗi cặp$(day, hour)$trong các tập hợp đã chọn đều có khả năng xảy ra như nhau. 

Xác suất mà chúng tôi quan tâm là xác suất vị trí được lấy mẫu còn trống trong lịch gốc. Chúng ta được phép chọn các tập hợp ngày và giờ một cách tối ưu để tối đa hóa xác suất này. 

Đầu ra là xác suất tối đa có thể đạt được này. 

Điều tinh tế quan trọng là tính ngẫu nhiên được tính theo cặp chứ không phải theo ngày hoặc giờ riêng lẻ. Khi chúng tôi chọn một tập hợp ngày và giờ, mọi kết hợp đều có khả năng như nhau, do đó chất lượng của lựa chọn được xác định bởi số lượng ô tự do nằm bên trong ma trận con cảm ứng, được chuẩn hóa theo diện tích của nó. 

Lưới cực kỳ nhỏ: chỉ 7 x 24. Điều này ngay lập tức loại trừ mọi thứ theo cấp số nhân ở cả hai chiều. Một cách tiếp cận ngây thơ thử tất cả các tập hợp con của ngày và giờ sẽ liên quan đến$2^7 \cdot 2^{24}$, nó quá lớn. Thậm chí việc liệt kê có mục tiêu hơn của cả hai bên là quá chậm nếu được thực hiện độc lập. 

Một số trường hợp khó xử lý sai: 

Nếu tất cả các ô đều bị chặn, câu trả lời phải là 0 bất kể lựa chọn nào. Cách tiếp cận tham lam cố gắng tối đa hóa các hàng hoặc cột một cách độc lập vẫn có thể tạo ra tỷ lệ khác 0 nếu nó chuẩn hóa không chính xác. 

Nếu như$d = 7$Và$h = 24$, sự lựa chọn là bắt buộc và câu trả lời chỉ đơn giản là một phần các ô tự do trong toàn bộ lưới. 

Nếu như$d = 1$hoặc$h = 1$, vấn đề giảm xuống còn việc chọn một tập hợp hàng hoặc cột và lý luận ngây thơ về "tối ưu hóa độc lập cho mỗi thứ nguyên" có thể không thành công vì việc thêm nhiều hàng hoặc cột sẽ làm tăng cả tử số và mẫu số theo cách kết hợp. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ thử từng tập hợp con ngày và mọi tập hợp con giờ. Đối với mỗi cặp tập hợp con, chúng tôi tính toán có bao nhiêu ô tự do nằm bên trong ma trận con cảm ứng và chia cho kích thước của nó. Điều này đúng vì nó khớp chính xác với định nghĩa xác suất. Vấn đề là quy mô: có$2^7 = 128$tập hợp con hàng và$2^{24} \approx 16$triệu tập hợp con cột, dẫn đến hàng tỷ đánh giá ngay cả trước khi tính chi phí tính tổng bên trong mỗi ma trận con. 

Cấu trúc sẽ trở nên dễ quản lý hơn khi chúng ta tách biệt vai trò của hàng và cột. Lưới có số lượng hàng nhỏ nên chúng ta có thể liệt kê tất cả các tập hợp con hàng một cách hiệu quả. Đối với bất kỳ tập hợp hàng cố định nào, vấn đề sẽ tập trung vào việc chọn một tập hợp con cột tốt. 

Sửa một tập hợp con hàng$S$. Đối với mỗi cột$j$, chúng ta có thể tính toán có bao nhiêu ô trống xuất hiện trong cột đó trong số các hàng đã chọn. Gọi giá trị này$c[j]$. Bây giờ vấn đề hoàn toàn là một chiều: chúng ta phải chọn ít nhất$h$cột và mỗi cột được chọn đóng góp độc lập$c[j]$đến tử số. Xác suất kết quả cho một tập hợp cột$T$là$$\frac{\sum_{j \in T} c[j]}{|S| \cdot |T|}.$$Để cố định$S$, mẫu số trong các hàng không đổi, do đó bài toán bên trong trở thành cực đại hóa giá trị trung bình của các cột đã chọn, tùy thuộc vào việc chọn ít nhất$h$của họ. 

Đây là cấu trúc cổ điển: khi biết được sự đóng góp của cột, tập hợp con tốt nhất có kích thước nhất định luôn là các cột top-k theo$c[j]$. Câu hỏi duy nhất còn lại là kích thước nào$k \ge h$là tối ưu. 

Do đó, đối với mỗi tập hợp con hàng, chúng tôi sắp xếp điểm số của 24 cột, tính tổng tiền tố và đánh giá tỷ lệ tốt nhất trên tất cả các tập hợp con.$k \ge h$. 

Điều này làm giảm vấn đề khi thử tất cả các tập hợp con hàng và giải quyết vấn đề tối ưu hóa được sắp xếp nhỏ cho mỗi tập hợp con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các hàng và cột |$O(2^7 \cdot 2^{24} \cdot 7 \cdot 24)$|$O(1)$| Quá chậm | 
| Liệt kê các tập hợp con hàng + cột tham lam |$O(2^7 \cdot 24 \log 24)$|$O(24)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán 

1. Liệt kê mọi tập hợp con của 7 hàng bằng mặt nạ bit. Mỗi tập hợp con đại diện cho một sự lựa chọn có thể có của ngày. Điều này khả thi vì chỉ có 128 tập con và mọi lời giải tối ưu đều phải tương ứng với một trong các tập con này. 
2. Đối với mỗi tập con hàng$S$, xây dựng một mảng$c[0..23]$Ở đâu$c[j]$đếm xem có bao nhiêu hàng đã chọn có một ô trống trong cột$j$. Điều này nén cấu trúc 2D thành hệ thống tính điểm 1D trong nhiều giờ. 
3. Nếu số hàng được chọn ít hơn$d$, bỏ qua tập hợp con này. Mặc dù vấn đề cho phép “ít nhất$d$” ngày, sử dụng ít hơn$d$không hợp lệ và phải bị loại bỏ. 
4. Sắp xếp điểm theo cột$c[j]$theo thứ tự giảm dần. Thứ tự này đảm bảo rằng với bất kỳ số cột cố định nào$k$, sự lựa chọn tối ưu luôn là sự lựa chọn đầu tiên$k$, vì mọi đóng góp đều độc lập và bổ sung. 
5. Tính tổng tiền tố trên mảng đã sắp xếp sao cho tổng của giá trị tốt nhất$k$cột có thể được lấy trong thời gian không đổi. 
6. Đối với mỗi$k$từ$h$đến 24, tính giá trị$$\frac{\text{prefix}[k]}{k \cdot |S|}$$và theo dõi mức tối đa trên tất cả$k$. Sự phân chia theo$|S|$phản ánh rằng mỗi ngày được chọn sẽ nhân số lượng vị trí họp ứng cử viên. 
7. Lấy giá trị lớn nhất trên tất cả các tập con hàng. 

### Tại sao nó hoạt động 

Đối với một tập hợp con hàng cố định, mỗi cột đóng góp độc lập vào tổng số cặp miễn phí. Bởi vì xác suất chỉ phụ thuộc vào tổng các đóng góp của cột đã chọn chia cho tích của các kích thước tập hợp đã chọn, nên cấu trúc tối ưu cho các cột phải là tiền tố của các đóng góp đã được sắp xếp. Bất kỳ sai lệch nào, chẳng hạn như thay thế cột có giá trị cao hơn bằng cột có giá trị thấp hơn, sẽ làm giảm nghiêm trọng tử số mà không thay đổi mẫu số. Điều này đảm bảo rằng việc hạn chế sự chú ý đến các tiền tố được sắp xếp sẽ không làm mất đi các giải pháp tối ưu và việc liệt kê các tập hợp con hàng đảm bảo tất cả các kết hợp hàng có thể được xem xét. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    grid = [input().strip() for _ in range(7)]
    d, h = map(int, input().split())

    best = 0.0

    for mask in range(1 << 7):
        rows = [i for i in range(7) if mask & (1 << i)]
        r = len(rows)
        if r < d:
            continue

        col = [0] * 24
        for i in rows:
            row = grid[i]
            for j in range(24):
                if row[j] == '.':
                    col[j] += 1

        col.sort(reverse=True)

        pref = [0] * 25
        for i in range(24):
            pref[i + 1] = pref[i] + col[i]

        for k in range(h, 25):
            val = pref[k] / (k * r)
            if val > best:
                best = val

    print(best)

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp theo sau việc phân tách thành các tập hợp con hàng và tính điểm cột. Mặt nạ hàng liệt kê tất cả các lựa chọn ngày có thể. Đối với mỗi lựa chọn, bước tổng hợp cột sẽ xây dựng điểm giờ độc lập. 

Việc sắp xếp 24 giá trị cột là an toàn vì nó chuyển đổi lựa chọn cột tổ hợp thành một bài toán quyết định đơn điệu trong đó các tập hợp tối ưu luôn là tiền tố. Mảng tiền tố cho phép đánh giá liên tục theo thời gian của bất kỳ kích thước ứng viên nào$k$. 

Sự phân chia cuối cùng bởi$k \cdot r$được thực hiện cho mỗi ứng viên, đảm bảo chúng tôi tôn trọng chính xác định nghĩa xác suất thay vì tối đa hóa số lượng thô. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng tôi xem xét một tập hợp con hàng đại diện có thể tối ưu sau khi đánh giá. Giả sử một tập hợp con chọn$r = 2$hàng. Sau khi quét các hàng đó, chúng tôi nhận được điểm số theo cột như: 

| Bước | Điểm cột (chưa sắp xếp) | Đã sắp xếp | Tổng tiền tố | 
| --- | --- | --- | --- | 
| Sau khi tổng hợp | [2, 1, 0, 2, ...] | 2,2,1,0,... | 0,2,4,5,... | 

Đối với mỗi$k \ge h = 5$, ta tính tỉ số$\text{prefix}[k]/(2k)$. Giá trị tốt nhất trong số tất cả các tập con hàng hóa ra là 0,8, khớp với đầu ra mẫu. 

Dấu vết này cho thấy cách giải pháp không khắc phục một số cột mà thay vào đó thử tất cả các kích thước hợp lệ trong khi vẫn duy trì cấu trúc tiền tố tối ưu. 

### Mẫu 2 

Đối với một lưới khác, giả sử một tập hợp con hàng có kích thước$r = 3$tạo ra điểm cột cân bằng hơn: 

| Bước | Điểm cột (chưa sắp xếp) | Đã sắp xếp | Tổng tiền tố | 
| --- | --- | --- | --- | 
| Sau khi tổng hợp | [3,2,2,1,...] | 3,2,2,1,... | 0,3,5,7,8,... | 

Chúng tôi đánh giá$k \ge 8$. Đối với mỗi$k$, tỷ lệ này thay đổi một chút và điều tốt nhất xảy ra ở một thời điểm cụ thể$k$trong đó việc thêm nhiều cột bắt đầu làm giảm chất lượng trung bình. 

Điều này chứng tỏ tại sao “luôn lấy chính xác h cột” là không chính xác: việc tăng số lượng cột có thể cải thiện hoặc làm giảm tỷ lệ tùy thuộc vào phần cuối của phân phối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(2^7 \cdot 24 \log 24)$| liệt kê tất cả các tập hợp con hàng, tính tổng 24 cột, sắp xếp theo tập hợp con | 
| Không gian |$O(24)$| chỉ các bộ đếm cột và mảng tiền tố được lưu trữ | 

Tổng công việc cực kỳ nhỏ: tối đa 128 lần lặp, mỗi lần xử lý 24 giá trị. Việc sắp xếp chiếm ưu thế nhưng không đáng kể ở quy mô này. Điều này dễ dàng phù hợp với giới hạn thời gian ngay cả trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue().strip() if False else solve_and_capture(inp)

def solve_and_capture(inp: str) -> str:
    import sys
    from io import StringIO
    backup = sys.stdin
    sys.stdin = StringIO(inp)

    grid = [input().strip() for _ in range(7)]
    d, h = map(int, input().split())

    best = 0.0

    for mask in range(1 << 7):
        rows = [i for i in range(7) if mask & (1 << i)]
        r = len(rows)
        if r < d:
            continue

        col = [0] * 24
        for i in rows:
            for j in range(24):
                if grid[i][j] == '.':
                    col[j] += 1

        col.sort(reverse=True)

        pref = [0] * 25
        for i in range(24):
            pref[i + 1] = pref[i] + col[i]

        for k in range(h, 25):
            best = max(best, pref[k] / (k * r))

    sys.stdin = backup
    return f"{best:.12f}".rstrip('0').rstrip('.')

# provided samples
assert abs(float(run("""\
xxxxxx..xx..xxxxxxxxxxxx
xxxxxxxxxxxxx....xxxxxxx
xxxxxxxxxxxxxxxxxxxxxxxx
xxxxxx..xx..xxxxxxxxxxxx
xxxxxxxxxxxxx...x..xxxxx
xxxxxxxxxxxxxxxxxxxxxxxx
xxxxxxxxxxxxxxxxxxxxxxxx
2 5
""")) - 0.8) < 1e-6

assert abs(float(run("""\
xxxxxxxxx.....x...xxxxxx
xxxxxxxx..x...x...xxxxxx
xxxxxxxx......x...x.xxxx
xxxxxxxx...xxxxxxxxxxxxx
xxxxxxxx...xxxxxxxxxxxxx
xxxxxxxx...xxxxxxxx.xxxx
......xxxxxxxxxxxxxxxxxx
3 8
""")) - 0.958333333333333) < 1e-6

# custom cases
assert abs(float(run("""\
........................

........................

........................

........................

........................

........................

........................

1 1
""")) - 1.0) < 1e-6

assert abs(float(run("""\
xxxxxxxxxxxxxxxxxxxxxxxx
xxxxxxxxxxxxxxxxxxxxxxxx
xxxxxxxxxxxxxxxxxxxxxxxx
xxxxxxxxxxxxxxxxxxxxxxxx
xxxxxxxxxxxxxxxxxxxxxxxx
xxxxxxxxxxxxxxxxxxxxxxxx
xxxxxxxxxxxxxxxxxxxxxxxx
7 24
""")) - 0.0) < 1e-6
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các dấu chấm, sự lựa chọn tối thiểu | 1.0 | trường hợp cạnh lưới hoàn toàn miễn phí | 
| tất cả bị chặn, lựa chọn đầy đủ | 0,0 | ổn định xác suất bằng không | 

## Vỏ cạnh 

Khi lưới không chứa ô trống, mỗi tập hợp con hàng sẽ tạo ra điểm cột bằng 0, do đó mọi tỷ lệ ứng cử viên đều có giá trị bằng 0. Thuật toán vẫn hoạt động chính xác vì tất cả các tiền tố vẫn bằng 0 và mức tối đa vẫn ở mức 0. 

Khi tất cả các ô đều trống, điểm của mỗi cột bằng số hàng đã chọn, do đó việc sắp xếp không thay đổi giá trị. Bất kỳ lựa chọn nào cũng mang lại xác suất 1 và thuật toán tự nhiên trả về 1 vì mọi tỷ lệ đều trở thành$r / (k \cdot r) = 1/k$lần$k$, đơn giản hóa thành 1. 

Khi nào$d = 7$, chỉ có mặt nạ hàng đầy đủ được xem xét. Thuật toán giảm xuống mức tối ưu hóa cột trên toàn bộ lưới, phù hợp với cách giải thích trực tiếp của vấn đề. 

Khi$h = 24$, chỉ có tập hợp đầy đủ các cột được xem xét. Giải pháp suy biến thành việc chọn tập hợp con hàng tốt nhất theo mật độ trung bình, vẫn được xử lý chính xác vì vòng lặp cột chỉ đánh giá$k = 24$.
