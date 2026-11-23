---
title: "CF 489D - Tranh Chấp Không Thể Chịu Được Về Hữu Thể"
description: "Chúng ta được cung cấp một đồ thị có hướng biểu thị các giao lộ và đường một chiều. Một \"hình thoi chết tiệt\" bao gồm bốn đỉnh phân biệt $a, b, c, d$ sao cho có các cạnh có hướng $$a rightarrow b,quad b rightarrow c,quad a rightarrow d,quad d rightarrow c$$ Nói cách khác, từ…"
date: "2026-06-07T17:36:35+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "combinatorics", "dfs-and-similar", "graphs"]
categories: ["algorithms"]
codeforces_contest: 489
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 277.5 (Div. 2)"
rating: 1700
weight: 489
solve_time_s: 113
verified: true
draft: false
---

[CF 489D - Tranh cãi không thể chịu đựng được về sự tồn tại](https://codeforces.com/problemset/problem/489/D) 

**Đánh giá:** 1700 
**Tags:** lực lượng vũ phu, tổ hợp, dfs và tương tự, đồ thị 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị có hướng biểu thị các giao lộ và đường một chiều. 

Một "hình thoi chết tiệt" bao gồm bốn đỉnh riêng biệt$a, b, c, d$sao cho có các cạnh định hướng$$a \rightarrow b,\quad b \rightarrow c,\quad a \rightarrow d,\quad d \rightarrow c$$Nói cách khác, từ cùng một đỉnh bắt đầu$a$, có hai đường đi có hướng có độ dài-2 khác nhau và kết thúc ở cùng một đỉnh$c$, sử dụng các đỉnh trung gian khác nhau$b$Và$d$. 

Nhiệm vụ là đếm xem có bao nhiêu cấu trúc như vậy tồn tại trong biểu đồ. Thứ tự của các đỉnh ở giữa không quan trọng, vì vậy$(b,d)$Và$(d,b)$mô tả cùng một hình thoi. 

Đồ thị chứa tối đa 3000 đỉnh và 30000 cạnh có hướng. Số đỉnh tương đối lớn nhưng số cạnh chỉ lớn hơn mười lần. Một giải pháp kiểm tra tất cả bốn đỉnh sẽ yêu cầu khoảng$3000^4$hoạt động, điều đó hoàn toàn không thể thực hiện được. Ngay cả việc kiểm tra tất cả các bộ ba cũng sẽ quá tốn kém. 

Ràng buộc$m \le 30000$là manh mối thực sự. Các thuật toán có độ phức tạp phụ thuộc chủ yếu vào các cạnh, chẳng hạn như$O(m\sqrt m)$hoặc$O(nm)$, là hợp lý. MỘT$O(n^2)$hoặc$O(n^3)$thuật toán phải được kiểm tra cẩn thận. 

Một số trường hợp đặc biệt có thể gây ra sai sót. 

Coi như:```
3 2
1 2
2 3
```Chỉ có một đường dẫn có độ dài 2 từ 1 đến 3, vì vậy câu trả lời là:```
0
```Một giải pháp bất cẩn chỉ đếm các đường dẫn có độ dài 2 có thể trả về 1 không chính xác. 

Một trường hợp quan trọng khác là khi nhiều đường dẫn có độ dài 2 có cùng điểm bắt đầu và kết thúc.```
4 4
1 2
2 4
1 3
3 4
```Có đúng hai đường đi từ 1 đến 4, đó là$1\to2\to4$Và$1\to3\to4$. Chúng tạo thành một hình thoi, vì vậy câu trả lời là:```
1
```Một trường hợp tế nhị khác là khi có nhiều hơn hai chất trung gian tồn tại.```
5 6
1 2
2 5
1 3
3 5
1 4
4 5
```Có ba đường dẫn có độ dài 2 từ 1 đến 5. Bất kỳ cặp trung gian nào cũng tạo thành hình thoi:$$(2,3),\ (2,4),\ (3,4)$$Câu trả lời là:```
3
```Giải pháp chỉ kiểm tra xem có tồn tại ít nhất hai đường dẫn hay không sẽ bị tính thiếu. 

Cuối cùng, bốn đỉnh phải phân biệt được. Bởi vì đồ thị không chứa các vòng tự lặp, việc đếm các cặp đỉnh trung gian khác nhau sẽ tự động đảm bảo tính khác biệt, nhưng thực tế này phải được hiểu trong quá trình tranh luận về tính đúng đắn. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là liệt kê từng bộ bốn$(a,b,d,c)$và kiểm tra xem có tồn tại tất cả bốn cạnh cần thiết hay không. 

Điều này đúng vì mọi hình thoi đều tương ứng với chính xác một bộ tứ như vậy. Thật không may, có$O(n^4)$tăng gấp bốn lần. Với$n=3000$, điều này hoàn toàn không thể thực hiện được. 

Một quan sát tốt hơn là tập trung vào đỉnh bắt đầu$a$. 

Giả định$a$đã được sửa. Mỗi cạnh đi ra$a \rightarrow x$có thể là bước đầu tiên của đường dẫn có độ dài-2. Nếu chúng ta đi theo tất cả các cạnh như vậy và ghi lại điểm kết thúc của các đường dẫn có độ dài 2 này thì với mỗi đỉnh đích$c$chúng tôi thu được số lượng trung gian riêng biệt tạo ra một đường dẫn$$a \rightarrow * \rightarrow c.$$Hãy để số này được tính$k$. 

Mỗi cặp trung gian trong số này$k$các lựa chọn tạo thành một hình thoi có đỉnh bắt đầu$a$và đỉnh cuối$c$. Số cặp như vậy là$$\binom{k}{2} = \frac{k(k-1)}{2}.$$Điều này biến bài toán thành việc đếm các đường dẫn có độ dài-2 được nhóm theo đỉnh đầu và đỉnh chung của chúng. 

Biểu đồ thưa thớt. Đối với nguồn cố định$a$, chúng ta có thể lặp qua mọi hàng xóm đi$u$, sau đó thông qua mọi hàng xóm đi ra$v$của$u$. Mỗi lượt truy cập tương ứng với một lần đi bộ có hướng dẫn có độ dài 2 bắt đầu từ$a$. 

Tổng công việc trên tất cả các nguồn là$$\sum_{a}\sum_{u \in out(a)} outdeg(u).$$Sắp xếp lại tổng kết cho$$\sum_{u} indeg(u)\cdot outdeg(u).$$Từ$m \le 30000$, giá trị tối đa của biểu thức này nằm trong giới hạn và là giải pháp mong muốn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^4)$|$O(1)$| Quá chậm | 
| Tối ưu |$O\!\left(\sum indeg(v)\,outdeg(v)\right)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ đồ thị bằng danh sách kề. 
2. Khởi tạo câu trả lời về 0. 
3. Với mọi đỉnh$a$: 

1. Tạo một mảng`cnt`kích thước$n$, ban đầu chứa đầy số không. 
2. Đối với mỗi người hàng xóm đi$u$của$a$: 

1. Đối với mỗi người hàng xóm đi$v$của$u$: 

1. Tăng`cnt[v]`. 

Sau vòng lặp này,`cnt[v]`bằng số lượng đường dẫn có độ dài-2 riêng biệt từ$a$ĐẾN$v$. 
4. Với mọi đỉnh$v$: 

1. Hãy để$k = cnt[v]$. 
2. Thêm$k(k-1)/2$để trả lời. 

Mỗi cặp đường đi có độ dài 2 kết thúc ở cùng một đỉnh đóng góp một hình thoi. 
5. In câu trả lời tích lũy. 

### Tại sao nó hoạt động 

Sửa một đỉnh nguồn$a$và một đỉnh đích$c$. 

Mỗi khi thuật toán đạt tới$c$trong khi đi ngang qua$$a \rightarrow b \rightarrow c,$$nó tăng lên`cnt[c]`. Như vậy`cnt[c]`trở thành chính xác số đỉnh trung gian hợp lệ tạo ra đường đi có độ dài-2 từ$a$ĐẾN$c$. 

Một hình thoi có điểm cuối$a$Và$c$thu được bằng cách chọn bất kỳ hai đường trung gian riêng biệt nào trong số các đường dẫn này. Số lượng các lựa chọn như vậy chính xác là$\binom{k}{2}$, Ở đâu$k=cnt[c]$. 

Mỗi hình thoi được tính đúng một lần vì nó có một đỉnh bắt đầu duy nhất$a$và đỉnh cuối$c$. Không có hình thoi nào bị bỏ sót vì mỗi đường đi có độ dài 2 đều đóng góp vào số lượng tương ứng. Không có hình thoi nào được tính hai lần vì các cặp trung gian không có thứ tự được tính chính xác một lần theo công thức tổ hợp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    g = [[] for _ in range(n)]

    for _ in range(m):
        a, b = map(int, input().split())
        g[a - 1].append(b - 1)

    ans = 0

    for a in range(n):
        cnt = [0] * n

        for u in g[a]:
            for v in g[u]:
                cnt[v] += 1

        for k in cnt:
            ans += k * (k - 1) // 2

    print(ans)

if __name__ == "__main__":
    solve()
```Danh sách kề lưu trữ tất cả các cạnh đi ra. 

Đối với mỗi đỉnh nguồn`a`, mảng`cnt`đếm xem có bao nhiêu đường đi có độ dài 2 kết thúc ở mỗi đỉnh. Truyền tải lồng nhau```
for u in g[a]:
    for v in g[u]:
```liệt kê mọi bước đi có hướng dẫn có độ dài hai bắt đầu từ`a`. 

Sau khi tất cả các bước đi như vậy được xử lý,`cnt[v]`bằng số lượng trung gian kết nối`a`ĐẾN`v`. Chọn bất kỳ hai chất trung gian nào sẽ tạo ra một hình thoi, đó là lý do tại sao phần đóng góp là```
k * (k - 1) // 2
```Câu trả lời có thể trở nên lớn hơn nhiều so với$2^{31}$, vì vậy nó phải được lưu trữ ở dạng số nguyên có khả năng 64 bit. Số nguyên Python tự động tăng lên nên không cần xử lý đặc biệt. 

Biểu đồ được chuyển đổi sang lập chỉ mục dựa trên số 0 ngay sau khi đọc. Điều này tránh việc điều chỉnh chỉ mục lặp đi lặp lại bên trong các vòng lặp chính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 4
1 2
2 3
1 4
4 3
```Đối với đỉnh nguồn 1: 

| Đường dẫn | Điểm đến | cnt | 
| --- | --- | --- | 
| 1→2→3 | 3 | cnt[3]=1 | 
| 1→4→3 | 3 | cnt[3]=2 | 

Sau khi xử lý: 

| Đỉnh | k | Đóng góp | 
| --- | --- | --- | 
| 3 | 2 | 1 | 
| Khác | 0 | 0 | 

Tổng số câu trả lời trở thành 1. 

Đầu ra:```
1
```Điều này thể hiện cấu trúc hình thoi cơ bản: hai phần trung gian khác nhau dẫn từ cùng một đầu đến cùng một đầu. 

### Ví dụ 2 

đầu vào:```
5 6
1 2
2 5
1 3
3 5
1 4
4 5
```Đối với đỉnh nguồn 1: 

| Đường dẫn | Điểm đến | cnt | 
| --- | --- | --- | 
| 1→2→5 | 5 | 1 | 
| 1→3→5 | 5 | 2 | 
| 1→4→5 | 5 | 3 | 

Hiện nay`k=3`cho đỉnh 5. 

| Đỉnh | k | Đóng góp | 
| --- | --- | --- | 
| 5 | 3 | 3 | 
| Khác | 0 | 0 | 

Đầu ra:```
3
```Ba hình thoi tương ứng với việc chọn bất kỳ cặp nào trong số các trung gian {2, 3, 4}. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O\!\left(\sum indeg(v)\,outdeg(v)\right)$| Mỗi bước đi có hướng dài 2 lần được xử lý một lần | 
| Không gian |$O(n)$| Mảng đếm cho một đỉnh nguồn | 

Đồ thị chứa tối đa 30000 cạnh. Giải pháp được chấp nhận xử lý tất cả các bước đi có độ dài 2 và chỉ sử dụng một mảng đếm có kích thước duy nhất$n$. Cả thời gian chạy và mức sử dụng bộ nhớ đều nằm trong giới hạn cho phép. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n, m = map(int, input().split())

    g = [[] for _ in range(n)]

    for _ in range(m):
        a, b = map(int, input().split())
        g[a - 1].append(b - 1)

    ans = 0

    for a in range(n):
        cnt = [0] * n

        for u in g[a]:
            for v in g[u]:
                cnt[v] += 1

        for k in cnt:
            ans += k * (k - 1) // 2

    return str(ans)

# provided sample
assert run("""5 4
1 2
2 3
1 4
4 3
""") == "1", "sample 1"

# minimum graph
assert run("""1 0
""") == "0", "single vertex"

# no rhombus
assert run("""3 2
1 2
2 3
""") == "0", "single length-2 path"

# three intermediates
assert run("""5 6
1 2
2 5
1 3
3 5
1 4
4 5
""") == "3", "three choose two"

# two independent rhombi
assert run("""6 8
1 2
2 4
1 3
3 4
1 5
5 6
1 4
4 6
""") == "2", "multiple destinations"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một đỉnh, không có cạnh | 0 | Giới hạn tối thiểu | 
| Một đường dẫn dài 2 | 0 | Chỉ riêng con đường không phải là hình thoi | 
| Ba trung gian đến cùng một đích | 3 | Sử dụng kết hợp đúng cách | 
| Hai hình thoi riêng biệt | 2 | Tích lũy trên các điểm đến | 
| Trường hợp mẫu | 1 | Tính đúng đắn cơ bản | 

## Vỏ cạnh 

Hãy xem xét một biểu đồ chỉ có một đường dẫn có độ dài 2:```
3 2
1 2
2 3
```Đối với nguồn 1, thuật toán ghi lại`cnt[3] = 1`. Sự đóng góp là$$\binom{1}{2}=0.$$Đầu ra chính xác là 0 vì hình thoi yêu cầu hai giá trị trung gian khác nhau. 

Hãy xem xét ba sản phẩm trung gian khác nhau:```
5 6
1 2
2 5
1 3
3 5
1 4
4 5
```Thuật toán thu được`cnt[5]=3`. Thay vì chỉ đếm một hình thoi, nó tính toán$$\binom{3}{2}=3,$$đếm chính xác tất cả các cặp trung gian. 

Xem xét các thành phần bị ngắt kết nối:```
6 4
1 2
2 3
4 5
5 6
```Mỗi nguồn tạo ra tối đa một đường dẫn có độ dài 2 tới bất kỳ đích nào. Mỗi số đếm tối đa là 1, vì vậy mọi đóng góp đều bằng 0. Thuật toán xử lý các đồ thị rời rạc một cách tự nhiên mà không có trường hợp đặc biệt nào. 

Xem xét các cạnh bổ sung giữa bốn đỉnh:```
4 6
1 2
2 4
1 3
3 4
2 3
3 2
```Các cạnh phụ không ảnh hưởng đến các đường dẫn cần thiết. Thuật toán chỉ nhóm các đường dẫn có độ dài-2 theo các đỉnh bắt đầu và kết thúc chung. Nó vẫn tìm thấy chính xác hai đường đi như vậy từ 1 đến 4, cho$$\binom{2}{2}=1.$$Đầu ra vẫn đúng.
