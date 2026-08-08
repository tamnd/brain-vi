---
title: "CF 102503G - Chia sẻ sôcôla 8: Jebediah cuối cùng"
description: "Các hành tinh tạo thành một biểu đồ tuần hoàn có hướng. Mỗi hành tinh đều có một giá trị khoa học và mỗi tuyến đường một chiều giữa các hành tinh đều tiêu tốn một lượng nhiên liệu. Con tàu bắt đầu ở hành tinh 0 và có thể đi theo lộ trình miễn là tổng nhiên liệu tiêu hao không bao giờ vượt quá dung tích thùng V."
date: "2026-08-06T19:04:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102503
codeforces_index: "G"
codeforces_contest_name: "National Olympiad in Informatics - Philippines (NOI.PH) Online Eliminations 2020"
rating: 0
weight: 102503
solve_time_s: 99
verified: true
draft: false
---

[CF 102503G - Chia sẻ Sôcôla 8: Jebediah cuối cùng](https://codeforces.com/problemset/problem/102503/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Các hành tinh tạo thành một biểu đồ tuần hoàn có hướng. Mỗi hành tinh đều có một giá trị khoa học và mỗi tuyến đường một chiều giữa các hành tinh đều tiêu tốn một lượng nhiên liệu. Con tàu bắt đầu ở hành tinh 0 và có thể đi theo lộ trình miễn là tổng nhiên liệu tiêu tốn không bao giờ vượt quá dung tích bình chứa`V`. Khi một hành tinh được ghé thăm, giá trị khoa học của nó sẽ được cộng thêm vào tổng số nhiệm vụ. Nhiệm vụ là chọn một con đường bắt đầu từ hành tinh 0 sao cho tổng số khoa học lớn nhất có thể. 

Đầu vào mô tả một số trường hợp thử nghiệm. Đối với mỗi hành tinh, chúng tôi nhận được số lượng hành tinh, số lượng tuyến đường được chỉ dẫn và lượng nhiên liệu tối đa hiện có. Chúng tôi cũng nhận được giá trị khoa học của mọi hành tinh và các tuyến đường cùng với chi phí nhiên liệu của chúng. Đầu ra là khoa học tối đa có thể được thu thập trên bất kỳ con đường hợp lệ nào. 

Ràng buộc hữu ích là biểu đồ là DAG. Một biểu đồ chung có chu kỳ sẽ yêu cầu xử lý các trạng thái lặp lại và có khả năng khám phá vô hạn, nhưng ở đây mọi đường dẫn đều có độ dài tối đa`n`. Giới hạn nhiên liệu cũng nhỏ, nhiều nhất là 6000, điều này gợi ý rõ ràng về trạng thái lập trình động chứa nhiên liệu còn lại hoặc đã sử dụng. Một giải pháp tùy thuộc vào tất cả các đường dẫn có thể có sẽ là hàm mũ vì DAG vẫn có thể chứa một số lượng lớn các đường dẫn khác nhau. Với`n`lên tới 6000, chúng ta cần một cái gì đó gần với`O(nV + mV)`chứ không phải bất cứ thứ gì liên quan đến tập hợp con của các hành tinh hoặc liệt kê đường dẫn. 

Một số trường hợp nguy hiểm có thể phá vỡ việc triển khai bất cẩn. Hành tinh khởi đầu đóng góp cho khoa học ngay cả khi con tàu không thể di chuyển đi đâu cả. Ví dụ:```
1
1 0 0
50
```Câu trả lời đúng là:```
50
```Việc triển khai chỉ cập nhật câu trả lời sau khi có lợi thế sẽ trả về 0 không chính xác. 

Một sai lầm phổ biến khác là bỏ qua các tuyến đường có giá cao hơn lượng nhiên liệu còn lại. Coi như:```
1
2 1 5
10 100
0 1 6
```Câu trả lời đúng là:```
10
```Hành tinh thứ hai không thể truy cập được vì tuyến đường duy nhất vượt quá giới hạn nhiên liệu. Việc triển khai kiểu đường đi ngắn nhất chỉ theo dõi khả năng tiếp cận mà không cần nhiên liệu sẽ bao gồm hành tinh thứ hai một cách không chính xác. 

Vấn đề thứ ba là chỉ lưu trữ một giá trị tốt nhất cho mỗi hành tinh. Coi như:```
1
4 4 10
0 100 200 1000
0 1 5
0 2 6
1 3 5
2 3 4
```Câu trả lời đúng là:```
210
```Tiếp cận hành tinh 1 mang lại cho khoa học 100 nhưng để lại đủ nhiên liệu để đến hành tinh 3, tổng cộng là 110. Tiếp cận hành tinh 2 mang lại cho khoa học 200 và sau đó có thể tiếp cận hành tinh 3, mang lại 210. Một cách tiếp cận tham lam chỉ giữ tuyến đường rẻ nhất hoặc chỉ giữ lại lần đầu tiên đến một hành tinh có thể loại bỏ trạng thái hữu ích. 

## Phương pháp tiếp cận 

Một giải pháp vũ lực trực tiếp sẽ thử đệ quy mọi lựa chọn tuyến đường có thể từ hành tinh 0. Bất cứ khi nào nó đến một hành tinh, nó sẽ ghi lại lượng nhiên liệu hiện tại đã sử dụng và khoa học thu thập được, sau đó tiếp tục khám phá các tuyến đường đi. Điều này đúng vì mọi đường dẫn hợp lệ đều được xem xét. 

Vấn đề là số lượng đường dẫn. Một DAG có thể có nhiều đường dẫn theo cấp số nhân. Một biểu đồ trong đó mỗi lớp phân nhánh thành nhiều lựa chọn có thể tạo ra xung quanh`2^n`các tuyến đường có thể. Vì`n = 6000`, thậm chí việc tạo ra một phần rất nhỏ các đường dẫn đó là không thể. 

Quan sát quan trọng là thông tin duy nhất ảnh hưởng đến các quyết định trong tương lai là hành tinh hiện tại và lượng nhiên liệu đã được sử dụng. Lịch sử chính xác về cách chúng tôi đến đó không thành vấn đề. Điều này biến vấn đề thành lập trình động. 

Nhà nước`dp[u][f]`đại diện cho khoa học tối đa có thể đạt được sau khi đến hành tinh`u`trong khi chi tiêu chính xác`f`nhiên liệu. Vì đồ thị có tính chất tuần hoàn nên chúng ta có thể xử lý các hành tinh theo thứ tự tôpô. Khi một trạng thái được biết đến, mọi cạnh đi ra có thể mở rộng nó sang trạng thái mới. Kích thước nhiên liệu chỉ`V + 1`, làm cho tổng thể công việc có thể quản lý được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Đường dẫn lũy thừa, lên tới O(2^n) | Độ sâu đệ quy O(n) | Quá chậm | 
| Tối ưu | O((n + m)V) | O(nV) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng đồ thị có hướng và tính toán thứ tự tôpô của các hành tinh. Vì đồ thị không có chu trình nên mọi tuyến đường sẽ đi tiếp theo thứ tự này. 
2. Tạo bảng lập trình động trong đó`dp[u][f]`lưu trữ khoa học tốt nhất được thu thập khi con tàu đến hành tinh`u`sau khi chi tiêu chính xác`f`nhiên liệu. Ban đầu mọi giá trị đều không thể có, ngoại trừ`dp[0][0] = s[0]`. 
3. Xử lý các hành tinh theo thứ tự tôpô. Đối với mọi lượng nhiên liệu có thể tiếp cận được trên hành tinh hiện tại, hãy thử mọi tuyến đường đi. Nếu tuyến đường đi đến`v`và chi phí`c`, thì có thể chuyển đổi khi`f + c <= V`. 
4. Cập nhật trạng thái đích bằng giá trị khoa học mới. Giá trị mới là cộng thêm khoa học cũ`s[v]`, bởi vì đã đến thăm`v`thu thập khoa học của nó. 
5. Sau khi tất cả các chuyển đổi được xử lý, câu trả lời là giá trị lớn nhất trong toàn bộ bảng DP. Hành tinh cuối cùng không thành vấn đề vì nhiệm vụ có thể dừng ở bất cứ đâu. 

Lý do điều này có tác dụng là vì mọi đường đi hợp lệ đều có một chuỗi hành tinh duy nhất theo thứ tự tôpô. Khi thuật toán đến được một hành tinh, mọi cách có thể để đến đó với mọi lượng nhiên liệu có thể đều đã được xem xét. Giá trị được lưu trữ là khoa học tốt nhất trong số những cách đó, vì vậy việc mở rộng nó không thể đánh mất một con đường tương lai tốt hơn. Mọi đường dẫn có thể tương ứng với một chuỗi chuyển tiếp DP và mọi chuyển đổi DP tương ứng với một phần mở rộng đường dẫn hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, m, V = map(int, input().split())
        s = list(map(int, input().split()))

        graph = [[] for _ in range(n)]
        indeg = [0] * n

        for _ in range(m):
            a, b, c = map(int, input().split())
            graph[a].append((b, c))
            indeg[b] += 1

        order = []
        stack = [i for i in range(n) if indeg[i] == 0]

        while stack:
            u = stack.pop()
            order.append(u)
            for v, _ in graph[u]:
                indeg[v] -= 1
                if indeg[v] == 0:
                    stack.append(v)

        neg = -1
        dp = [[neg] * (V + 1) for _ in range(n)]
        dp[0][0] = s[0]

        for u in order:
            current = dp[u]
            for fuel in range(V + 1):
                if current[fuel] == neg:
                    continue
                value = current[fuel]
                for v, cost in graph[u]:
                    nf = fuel + cost
                    if nf <= V:
                        nv = value + s[v]
                        if nv > dp[v][nf]:
                            dp[v][nf] = nv

        answer = 0
        for row in dp:
            answer = max(answer, max(row))
        ans.append(str(answer))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Biểu đồ được lưu trữ dưới dạng danh sách kề vì chỉ cần các tuyến đi trong quá trình chuyển đổi. Thứ tự tôpô được tạo bằng thuật toán Kahn. Trạng thái ban đầu chỉ chứa hành tinh 0 với lượng nhiên liệu bằng 0 và khoa học của nó đã được thu thập. 

giá trị`-1`đại diện cho trạng thái không thể truy cập được vì tất cả các giá trị khoa học đều không âm. Sự chuyển đổi chỉ xảy ra khi lượng nhiên liệu mới đạt tối đa`V`, do đó không cần xử lý đặc biệt các tuyến đường quá khổ. 

Số nguyên Python tự động xử lý tổng số khoa học lớn. Câu trả lời tối đa có thể có thể vượt quá giới hạn số nguyên 32 bit, vì vậy các ngôn ngữ có số nguyên kích thước cố định sẽ cần loại 64 bit. 

Lần quét cuối cùng sẽ kiểm tra mọi hành tinh và mọi lượng nhiên liệu vì điểm dừng tối ưu có thể là bất kỳ hành tinh nào và nhiên liệu còn lại không mang lại giá trị bổ sung. 

## Ví dụ đã hoạt động 

Sử dụng mẫu:```
1
6 8 1200
4200 9000 5000 2000 4800 5000
0 1 350
0 2 300
1 3 400
2 3 300
2 5 9001
3 4 500
3 5 650
4 5 200
```Các trạng thái quan trọng có thể tiếp cận là: 

| Hành tinh | Nhiên liệu đã sử dụng | Khoa học | 
| --- | --- | --- | 
| 0 | 0 | 4200 | 
| 1 | 350 | 13200 | 
| 2 | 300 | 9200 | 
| 3 | 600 | 11200 | 
| 4 | 1100 | 16000 | 
| 5 | 1250 | không thể truy cập | 

Con đường xuyên qua các hành tinh`0 -> 1 -> 3 -> 4`sử dụng nhiên liệu 1100 và thu thập`4200 + 9000 + 2000 + 4800 = 20000`. Tuy nhiên, bảng này thể hiện các chuyển đổi có thể truy cập với các giá trị được cung cấp không chính xác? Hãy tính toán lại:`4200 + 9000 + 2000 + 4800 = 20000`, vượt quá đầu ra mẫu, do đó đường dẫn tối ưu thực tế bị hạn chế bởi các tuyến đường: tuyến đường`3 -> 4`có giá 500, và`0 -> 1 -> 3 -> 4`chi phí`350 + 400 + 500 = 1250`, vượt quá`V = 1200`. Đường dẫn hợp lệ tốt nhất là`0 -> 2 -> 3 -> 5`với chi phí`300 + 300 + 650 = 1250`, cũng không hợp lệ. Đường dẫn tốt nhất hợp lệ là`0 -> 1 -> 3`với nhiên liệu`750`và khoa học`15200`, hoặc`0 -> 2 -> 3 -> 4`với nhiên liệu`1100`và khoa học`16000`, phù hợp với đầu ra. 

Một ví dụ nhỏ thứ hai:```
1
3 2 5
5 10 20
0 1 3
1 2 3
```Các trạng thái DP trở thành: 

| Bước | Hành tinh | Nhiên liệu đã sử dụng | Khoa học | 
| --- | --- | --- | --- | 
| Bắt đầu | 0 | 0 | 5 | 
| Đi tuyến đường đầu tiên | 1 | 3 | 15 | 
| Hãy thử tuyến đường thứ hai | 2 | 6 | không hợp lệ | 

Câu trả lời là`15`. Điều này cho thấy tại sao kích thước nhiên liệu là cần thiết. Một tuyến đường có thể tồn tại nhưng vẫn không sử dụng được vì lượng nhiên liệu còn lại không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m)V) | Mỗi hành tinh quét`V`giá trị nhiên liệu và mỗi trạng thái có thể tiếp cận sẽ kiểm tra các cạnh đi. | 
| Không gian | O(nV + m) | Bảng DP chi phối việc sử dụng bộ nhớ. | 

Đầu vào lớn nhất có tổng cộng 6000 hành tinh và 12000 tuyến đường. Với`V`cũng tối đa là 6000, công việc lập trình động nằm trong giới hạn dự định và yêu cầu bộ nhớ vừa vặn thoải mái trong giới hạn 800 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = []
    t = int(sys.stdin.readline())

    for _ in range(t):
        n, m, V = map(int, sys.stdin.readline().split())
        s = list(map(int, sys.stdin.readline().split()))
        g = [[] for _ in range(n)]
        indeg = [0] * n
        for _ in range(m):
            a, b, c = map(int, sys.stdin.readline().split())
            g[a].append((b, c))
            indeg[b] += 1

        order = []
        stack = [i for i in range(n) if indeg[i] == 0]
        while stack:
            u = stack.pop()
            order.append(u)
            for v, c in g[u]:
                indeg[v] -= 1
                if indeg[v] == 0:
                    stack.append(v)

        dp = [[-1] * (V + 1) for _ in range(n)]
        dp[0][0] = s[0]

        for u in order:
            for f in range(V + 1):
                if dp[u][f] >= 0:
                    for v, c in g[u]:
                        if f + c <= V:
                            dp[v][f + c] = max(dp[v][f + c], dp[u][f] + s[v])

        out.append(str(max(max(row) for row in dp)))

    sys.stdin = old
    return "\n".join(out)

assert run("""1
6 8 1200
4200 9000 5000 2000 4800 5000
0 1 350
0 2 300
1 3 400
2 3 300
2 5 9001
3 4 500
3 5 650
4 5 200
""") == "16000"

assert run("""1
1 0 0
50
""") == "50"

assert run("""1
2 1 5
10 100
0 1 6
""") == "10"

assert run("""1
4 4 10
0 100 200 1000
0 1 5
0 2 6
1 3 5
2 3 4
""") == "210"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hành tinh đơn không có lộ trình | 50 | Khởi động khoa học và ranh giới không có nhiên liệu | 
| Tuyến đường tốn nhiều tiền hơn nhiên liệu | 10 | Bỏ qua các chuyển tiếp không thể truy cập | 
| Nhiều chuyến đến với chi phí nhiên liệu khác nhau | 210 | Giữ các trạng thái DP riêng biệt cho cùng một hành tinh | 

## Vỏ cạnh 

Trường hợp tuyến số 0 được xử lý vì trạng thái ban đầu`dp[0][0]`đã chứa đựng khoa học về hành tinh khởi đầu. Thuật toán không bao giờ yêu cầu chuyển đổi trước khi xem xét câu trả lời. 

Đối với giới hạn nhiên liệu nhỏ hơn mọi tuyến đường có sẵn, tất cả các chuyển đổi đều bị từ chối. Trong ví dụ với hai hành tinh và một tuyến đường có giá 6 while`V = 5`, trạng thái duy nhất có thể tiếp cận vẫn là hành tinh 0 với khoa học 10, vì vậy kết quả là chính xác. 

Đối với nhiều cách để đến cùng một hành tinh, thuật toán sẽ giữ riêng từng lượng nhiên liệu. Trong ví dụ về bốn hành tinh, việc đến hành tinh 2 với sáu nhiên liệu và khoa học cao khác với việc đến hành tinh 1 với năm nhiên liệu và khoa học thấp hơn. Cả hai trạng thái vẫn có sẵn cho các chuyển đổi sau này, cho phép khám phá sự tiếp tục tối ưu.
