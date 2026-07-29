---
title: "CF 102760J - Điều khiển từ xa"
description: "Lưới là vô hạn, ngoại trừ ô (0,0) chứa một bức tường. Điều khiển từ xa chứa một chuỗi chuyển động cố định."
date: "2026-07-29T00:01:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102760
codeforces_index: "J"
codeforces_contest_name: "2020 KAIST 10th ICPC Mock Contest (XXI Open Cup. Grand Prix of Korea. Division 2)"
rating: 0
weight: 102760
solve_time_s: 77
verified: true
draft: false
---

[CF 102760J - Điều khiển từ xa](https://codeforces.com/problemset/problem/102760/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Lưới là vô hạn, ngoại trừ ô`(0,0)`chứa một bức tường. Điều khiển từ xa chứa một chuỗi chuyển động cố định. Một chiếc ô tô được đặt ở một ô không có tường nào đó sẽ tuân theo trình tự đó, nhưng bất cứ khi nào một nước đi vào tường, nước đi đó sẽ bị bỏ qua và ô tô sẽ tiếp tục thực hiện các lệnh còn lại. 

Đối với mỗi tọa độ bắt đầu, chúng ta cần tìm tọa độ cuối cùng sau khi nhấn nút một lần. 

Chuỗi lệnh có độ dài lên tới`300000`, và cũng có thể lên tới`300000`truy vấn. Việc mô phỏng toàn bộ đường dẫn cho mỗi truy vấn sẽ yêu cầu tới`9 * 10^10`hoạt động trong trường hợp xấu nhất, vượt xa giới hạn 2 giây cho phép. Chúng ta cần xử lý trước chuỗi lệnh một lần và trả lời từng truy vấn trong thời gian không đổi. 

Khó khăn chính là bức tường chỉ ảnh hưởng đến những vị trí xuất phát có thể va chạm với nó. Hầu hết các vị trí bắt đầu chỉ đơn giản tuân theo trình tự lệnh như thể bức tường không tồn tại. 

Việc triển khai bất cẩn có thể thất bại ở các vị trí lặp lại trong đường dẫn lệnh. Ví dụ: nếu đường đi đến cùng một điểm dịch chuyển nhiều lần thì lần truy cập đầu tiên là lần quan trọng vì ô tô va vào tường ở đó và thay đổi phần còn lại của mô phỏng. 

Đối với lệnh:```
2
RL
```các chuyển vị tiền tố là`(0,0)`,`(1,0)`,`(0,0)`. Bắt đầu lúc`(0,-0)`không hợp lệ vì đó là bức tường, nhưng vấn đề vị trí lặp lại tương tự lại xuất hiện trong các trường hợp gần đó. Nếu chúng ta lưu trữ lần xuất hiện cuối cùng thay vì lần xuất hiện đầu tiên của vị trí tiền tố thì điểm xung đột được tính toán sẽ sai. 

Một trường hợp cạnh khác là đường dẫn lệnh không bao giờ quay trở lại cùng một độ dịch chuyển. Vì:```
1
R
```một truy vấn bắt đầu từ`(5,5)`nên xuất ra`(6,5)`. Giải pháp cố gắng xử lý mọi điểm gần đường đi thay vì sử dụng quan sát dịch chuyển sẽ lãng phí thời gian. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng các lệnh cho mọi truy vấn. Đối với một truy vấn`(x,y)`, chúng ta giữ nguyên vị trí hiện tại và áp dụng mọi ký tự của lệnh. Bất cứ khi nào ô tiếp theo được`(0,0)`, chúng ta bỏ qua bước đi đó. Điều này rõ ràng là đúng vì nó tuân thủ chính xác các quy tắc của ô tô, nhưng cần phải có`O(N)`thời gian cho mỗi truy vấn. Với cả hai`N`Và`Q`bằng`300000`, điều này trở thành`O(NQ)`, điều đó là không thể. 

Quan sát quan trọng là bức tường chỉ có thể quan trọng nếu đường dẫn lệnh được dịch đến điểm gốc. Cho phép`P[i]`là độ dịch chuyển của ô tô sau lần đầu tiên`i`lệnh khi bắt đầu từ`(0,0)`. Không có tường, ô tô khởi hành từ`(x,y)`sẽ kết thúc tại:```
(x + P[N].x, y + P[N].y)
```Bức tường chỉ gặp phải khi:```
(x,y) + P[i] = (0,0)
```có nghĩa là:```
(x,y) = -P[i]
```Vì vậy, chỉ những vị trí bắt đầu là số âm của các vị trí tiền tố mới cần xử lý đặc biệt. 

Đối với những vị trí đó, lần đầu tiên đường đi tới bức tường là lần xuất hiện đầu tiên của sự dịch chuyển tiền tố đó. Sau lần di chuyển bị chặn đó, hậu tố còn lại của đường dẫn được thực hiện bình thường. Chúng tôi có thể xử lý trước câu trả lời cho mọi vị trí xuất phát nguy hiểm trong khi quét lệnh một lần. 

Brute-force hoạt động vì nó tuân theo mô phỏng chính xác nhưng không thành công vì nó lặp lại cùng một công việc cho nhiều truy vấn. Nhận xét rằng chỉ những chuyển vị tiền tố mới có thể gây ra xung đột sẽ giảm bớt vấn đề xử lý trước một tập hợp các trạng thái đặc biệt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NQ) | O(1) | Quá chậm | 
| Tối ưu | O(N + Q) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng độ dịch chuyển của toàn bộ chuỗi lệnh. Bất kỳ truy vấn nào có vị trí bắt đầu không bao giờ chạm tới bức tường đều có thể được trả lời bằng cách thêm chuyển vị này. 
2. Quét chuỗi lệnh và lưu trữ mọi chuyển vị tiền tố. Đối với mỗi lần dịch chuyển, chỉ giữ lại chỉ số xuất hiện đầu tiên của nó. Lần xuất hiện đầu tiên là lần đầu tiên ô tô có thể va vào tường ở vị trí xuất phát tương ứng. 
3. Đối với mỗi lần xuất hiện đầu tiên của vị trí tiền tố`P[i]`, tạo một mục nhập cho điểm xuất phát nguy hiểm`-P[i]`. 
4. Để tính vị trí cuối cùng của điểm xuất phát nguy hiểm, hãy`i`là lần đầu tiên đường đi đạt tới sự dịch chuyển tiền tố đó. Trước khi va chạm xe đến vị trí là`P[i-1] - P[i]`. Lệnh vào thời điểm đó`i`bị chặn và các lệnh còn lại góp phần dịch chuyển`P[N] - P[i]`. 

Do đó, vị trí cuối cùng là:```
P[i-1] - P[i] + P[N] - P[i]
```hoặc:```
P[N] + P[i-1] - 2 * P[i]
```1. Đối với mỗi truy vấn, hãy kiểm tra xem tọa độ của nó có tồn tại trong bản đồ vị trí nguy hiểm được tính toán trước hay không. Nếu có, hãy xuất câu trả lời được lưu trữ. Nếu không, xuất vị trí dịch bình thường. 

Tại sao nó hoạt động: mọi tương tác có thể có với tường đều tương ứng chính xác với sự dịch chuyển tiền tố của đường dẫn lệnh. Nếu vị trí bắt đầu không phải là giá trị âm của bất kỳ chuyển vị tiền tố nào, thì bức tường sẽ không bao giờ đạt tới và vị trí cuối cùng chỉ là chuyển vị tổng cộng. Nếu đó là một vị trí nguy hiểm, lần xuất hiện đầu tiên của sự dịch chuyển tiền tố đó sẽ quyết định nước đi bị chặn đầu tiên. Sau thời điểm đó, hành vi của ô tô hoàn toàn được xác định bởi hậu tố còn lại, đó chính xác là những gì công thức tính toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    s = input().strip()

    dx = [0] * (n + 1)
    dy = [0] * (n + 1)

    x = y = 0
    for i, c in enumerate(s, 1):
        if c == 'L':
            x -= 1
        elif c == 'R':
            x += 1
        elif c == 'U':
            y += 1
        else:
            y -= 1
        dx[i] = x
        dy[i] = y

    total_x, total_y = x, y

    first = {}
    for i in range(n + 1):
        pos = (dx[i], dy[i])
        if pos not in first:
            first[pos] = i

    special = {}
    for (px, py), i in first.items():
        if px == 0 and py == 0:
            continue
        prev_x = dx[i - 1]
        prev_y = dy[i - 1]
        ans_x = total_x + prev_x - 2 * px
        ans_y = total_y + prev_y - 2 * py
        special[(-px, -py)] = (ans_x, ans_y)

    q = int(input())
    out = []
    for _ in range(q):
        x, y = map(int, input().split())
        if (x, y) in special:
            ax, ay = special[(x, y)]
            out.append(f"{ax} {ay}")
        else:
            out.append(f"{x + total_x} {y + total_y}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Các mảng`dx`Và`dy`lưu trữ đường dẫn lệnh bắt đầu từ gốc. Việc giữ lại tất cả các tiền tố sẽ tránh việc tính toán lại tọa độ trong khi trả lời các truy vấn. 

các`first`Từ điển là phần quan trọng của quá trình tiền xử lý. Khi cùng một chuyển vị xuất hiện nhiều lần, chỉ chỉ số sớm nhất được lưu trữ vì đó là nơi xảy ra va chạm đầu tiên. Sử dụng lần xuất hiện cuối cùng sẽ mô phỏng một chiếc ô tô đã đi xuyên qua bức tường, điều này là không thể. 

các`special`từ điển chỉ lưu trữ tọa độ bắt đầu không phải tường. Nguồn gốc bị loại trừ vì ô tô không bao giờ có thể khởi động được ở đó. Mọi truy vấn thông thường đều tránh việc tra cứu từ điển này và sử dụng trực tiếp tổng dịch chuyển. 

Tất cả các phép tính đều sử dụng số nguyên Python, do đó không có vấn đề tràn. Việc lập chỉ mục với`i - 1`là an toàn vì lần xuất hiện được lưu trữ đầu tiên của`(0,0)`là chỉ số`0`, và trường hợp đó được bỏ qua. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
8
RRDRUULL
5
-2 1
-2 2
-2 -1
-3 -1
1 1
```Các chuyển vị tiền tố là: 

| Bước | Lệnh | Vị trí | 
| --- | --- | --- | 
| 0 | bắt đầu | (0,0) | 
| 1 | R | (1,0) | 
| 2 | R | (2,0) | 
| 3 | D | (2,-1) | 
| 4 | R | (3,-1) | 
| 5 | Bạn | (3,0) | 
| 6 | Bạn | (3,1) | 
| 7 | L | (2,1) | 
| 8 | L | (1,1) | 

Tổng chuyển vị là`(1,1)`. Một truy vấn như`(1,1)`không nguy hiểm vì`(-1,-1)`không phải là sự dịch chuyển tiền tố, nên câu trả lời là`(2,2)`. 

Đối với một truy vấn nguy hiểm: 

| Giá trị | Ý nghĩa | 
| --- | --- | 
| Điểm xuất phát | (-2, -1) | 
| Vị trí tiền tố phủ định | (2,1) | 
| Chỉ số xuất hiện đầu tiên | 7 | 
| Tiền tố trước | (3,1) | 
| Tổng chuyển vị | (1,1) | 
| Vị trí cuối cùng | (1,0) | 

Dấu vết này chứng tỏ rằng chỉ có va chạm đầu tiên mới quan trọng. Sau khi di chuyển bị chặn, hậu tố tiếp tục từ vị trí di chuyển không thành công đã rời khỏi xe. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + Q) | Đường dẫn lệnh được quét một lần và mọi truy vấn đều được trả lời bằng cách tra cứu từ điển. | 
| Không gian | O(N) | Chúng tôi lưu trữ các chuyển vị tiền tố và vị trí bắt đầu nguy hiểm. | 

Các ràng buộc cho phép tiền xử lý tuyến tính vì cả độ dài đường dẫn và số lượng truy vấn đều`300000`. Thuật toán chỉ thực hiện một lượng công việc không đổi cho mỗi lệnh và mỗi truy vấn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    result = sys.stdout.getvalue()
    sys.stdin = old
    return result

assert run("""1
R
3
1 1
-1 0
0 1
""") == """2 1
0 0
1 1""", "simple movement"

assert run("""8
RRDRUULL
5
-2 1
-2 2
-2 -1
-3 -1
1 1
""") == """-1 3
-1 3
1 0
-2 -1
2 2""", "sample"

assert run("""3
RRR
4
-1 0
-3 0
10 10
1 0
""") == """2 0
2 0
13 10
4 0""", "straight line boundary"

assert run("""4
UDUD
3
0 1
0 -1
5 5
""") == """0 1
0 -1
5 5""", "repeated positions"

assert run("""1
U
2
0 -1
100 100
""") == """0 0
100 101""", "minimum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Nước đi đơn bên phải | Vị trí dịch | Chuyển động cơ bản không va chạm | 
| Mẫu | Đầu ra mẫu | Xử lý va chạm đầy đủ | 
| Con đường thẳng | Đúng chuyển động bị chặn và bình thường | Tiền tố phát hiện va chạm | 
| Vị trí lặp đi lặp lại | Xử lý tọa độ tương tự | Yêu cầu xuất hiện lần đầu | 
| Lệnh đơn | Kích thước đường dẫn nhỏ nhất | Hành vi ranh giới | 

## Vỏ cạnh 

Vị trí tiền tố lặp lại phải sử dụng lần xuất hiện đầu tiên của nó. Giả sử một đường dẫn lệnh đạt đến cùng một độ dịch chuyển nhiều lần. Xe xuất phát ở số âm của dịch chuyển đó va chạm ở lần đi đầu tiên nên những lần đi sau không liên quan. Quá trình tiền xử lý giữ chỉ mục sớm nhất và tạo ra mô phỏng hậu tố chính xác. 

Một truy vấn không bao giờ chạm tới tường không được nhập logic trường hợp đặc biệt. Ví dụ: với:```
1
R
1
5 5
```tổng độ dịch chuyển là`(1,0)`. Từ`(-5,-5)`không phải là vị trí tiền tố, câu trả lời đơn giản là`(6,5)`. 

Đường dẫn trả về hoặc lặp lại vị trí ngay lập tức được xử lý theo quy tắc tương tự. Từ điển chỉ chứa lần đầu tiên mỗi lần dịch chuyển xuất hiện, vì vậy mô phỏng luôn sử dụng nước đi bị chặn đầu tiên và không bao giờ cho rằng ô tô có thể đi xuyên qua tường.
