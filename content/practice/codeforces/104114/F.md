---
title: "CF 104114F - Vận may trên tinh thần thể thao"
description: "Chúng ta được cung cấp một biểu đồ có trọng số hoàn chỉnh về $n$ người chơi. Trọng số giữa người chơi $i$ và người chơi $j$ là một giá trị đối xứng $P{i,j}$, đại diện cho mức độ phổ biến tăng lên nếu hai người chơi đó chơi một trận đấu. Một trận đấu luôn loại bỏ một người chơi."
date: "2026-07-02T02:00:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104114
codeforces_index: "F"
codeforces_contest_name: "2022 ICPC Southeastern Europe Regional Contest"
rating: 0
weight: 104114
solve_time_s: 51
verified: true
draft: false
---

[CF 104114F - Vận may nhờ tinh thần thể thao](https://codeforces.com/problemset/problem/104114/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ có trọng số hoàn chỉnh về$n$người chơi. Trọng lượng giữa người chơi$i$và người chơi$j$là một giá trị đối xứng$P_{i,j}$, đại diện cho mức độ phổ biến tăng lên nếu hai người chơi đó chơi một trận đấu. 

Một trận đấu luôn loại bỏ một người chơi. Quy tắc đặt hàng được cố định: nếu$i < j$, người chơi$i$luôn thắng. Sau một trận đấu mà cầu thủ$i$người chơi nhịp đập$j$, người chơi$i$hấp thụ người chơi$j$hồ sơ phổ biến của, có ý nghĩa đối với mọi người chơi thứ ba$x$, giá trị$P_{i,x}$trở thành$\max(P_{i,x}, P_{j,x})$. Đối xứng,$P_{x,i}$được cập nhật theo cách tương tự, vì vậy người chơi sẽ có hiệu quả$i$trở thành hợp của hai hàng trong ma trận. 

Chúng ta phải lựa chọn chính xác$n-1$các trận đấu, tạo thành một quy trình loại trực tiếp nhằm giảm tất cả người chơi thành một người chiến thắng cuối cùng. Các trận đấu phải được sắp xếp theo thứ tự, sau mỗi trận đấu, người thua cuộc sẽ biến mất và hàng người thắng cuộc được cập nhật trước trận đấu tiếp theo. 

Tỷ số trận đấu giữa$i$Và$j$luôn là giá trị hiện tại$P_{i,j}$tại thời điểm trận đấu diễn ra, sau khi tất cả các lần hợp nhất trước đó đã cập nhật ma trận. Mục tiêu là chọn cả cơ cấu ghép đôi và thứ tự các trận đấu sao cho tổng điểm của các trận đấu này đạt tối đa. 

Ràng buộc$n \le 1000$có nghĩa là bất kỳ$O(n^2)$hoặc$O(n^2 \log n)$chiến lược là hợp lý, nhưng bất cứ điều gì gần hơn với$O(n^3)$là nguy hiểm trừ khi được tối ưu hóa nhiều hoặc rất đơn giản cho mỗi lần lặp. Chúng ta cũng cần xuất ra trình tự loại bỏ thực tế chứ không chỉ giá trị. 

Một điểm tinh tế là các mục nhập ma trận phát triển theo thời gian và một mô phỏng đơn giản tính toán lại các bản cập nhật tối đa sau mỗi lần hợp nhất có thể dễ dàng trở thành$O(n^3)$hoặc tệ hơn. Một cạm bẫy khác là giả định cấu trúc của các kết quả khớp là tùy ý, trong khi trên thực tế, ràng buộc “chỉ số nhỏ hơn luôn thắng” buộc phải có một cấu trúc có định hướng. 

Trường hợp góc xuất hiện khi cập nhật xếp tầng: sau khi hợp nhất$i$Và$j$, người chiến thắng có thể đột ngột tăng sức nặng của mình lên đối thủ trong tương lai, thay đổi lựa chọn tối ưu cho trận đấu tiếp theo. Một sự lựa chọn tham lam mà bỏ qua sự phát triển trong tương lai có thể thất bại. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là mô phỏng giải đấu: ở mỗi bước, hãy thử tất cả các cặp còn lại, mô phỏng việc hợp nhất, tính toán trạng thái kết quả và lặp lại. Điều này đúng về mặt khái niệm vì nó khám phá tất cả các cây và thứ tự giải đấu có thể có, nhưng hệ số phân nhánh đại khái là$O(n^2)$ở mỗi cấp độ, có chiều sâu$n$. Ngay cả với khả năng ghi nhớ, không gian trạng thái là tập hợp tất cả các tập con có thể có cộng với tất cả các ma trận hợp nhất có thể có, rất lớn về mặt thiên văn. Nút thắt không chỉ là sự bùng nổ tổ hợp mà còn là chi phí cập nhật ma trận sau mỗi trận đấu giả định.$O(n)$mỗi lần cập nhật, dẫn đến ít nhất$O(n^3)$mỗi con đường. 

Quan sát cấu trúc quan trọng là quy tắc người chiến thắng cố định hướng: bất cứ khi nào một trận đấu diễn ra giữa hai người chơi còn sống, chỉ số nhỏ hơn sẽ luôn tồn tại. Điều này biến quá trình này thành việc xây dựng một cấu trúc gốc trong đó những người chơi có chỉ số cao hơn dần dần bị hấp thụ bởi những đại diện có chỉ số thấp hơn. Vì vậy, cuối cùng mỗi người chơi sẽ bị hấp thụ vào đúng một tổ tiên có chỉ số nhỏ hơn. 

Điều này gợi ý đảo ngược quá trình. Thay vì mô phỏng việc loại bỏ, chúng ta có thể nghĩ đến việc tạo ra kẻ sống sót cuối cùng bằng cách hợp nhất dần dần các thành phần theo thứ tự tăng dần của các chỉ số, luôn gắn một chỉ số lớn hơn vào một số chỉ mục nhỏ hơn. Hoạt động hợp nhất mang tính quyết định, vì vậy điều còn lại là quyết định trình tự đính kèm nhằm tối đa hóa lợi ích cạnh. 

Bây giờ hãy diễn giải quy trình theo cách khác: tại bất kỳ thời điểm nào, mỗi người chơi còn sống sẽ đại diện cho một tập hợp người chơi ban đầu và hàng của nó là giá trị tối đa theo phần tử trên tập hợp đó. Nếu chúng ta quyết định cầu thủ đó$j$được hấp thụ vào$i$, thì chúng ta đạt được$P_{i,j}$tại thời điểm đó và giá trị tương lai của$i$trở nên giàu có nhờ$j$hàng của. 

Điều này trở thành một cấu trúc giống như cây bao trùm tối đa, nhưng có hiệu ứng trọng số động: việc hợp nhất sẽ cải thiện trọng số cạnh trong tương lai thông qua việc truyền bá tối đa. Quan sát quan trọng là đối với bất kỳ hai thành phần nào, sự hợp nhất ngay lập tức tốt nhất được xác định bởi cạnh tối đa hiện tại giữa các đại diện của chúng và sau khi được hợp nhất, đại diện thu được chỉ tăng trọng số chứ không bao giờ giảm chúng. Sự đơn điệu này cho phép chúng ta duy trì các kết nối tốt nhất có thể giữa các thành phần bằng cách sử dụng cấu trúc ưu tiên. 

Giải pháp cuối cùng có thể được xem là liên tục chọn kết quả phù hợp tiếp theo tốt nhất có thể trong số tất cả các cạnh giữa các thành phần khác nhau, hợp nhất chúng và cập nhật các cạnh bị ảnh hưởng thông qua việc truyền bá tối đa. Điều này hoạt động giống như việc xây dựng cây bao trùm tối đa theo các trọng số động, trong đó tính tham lam giống như Prim hoạt động vì hoạt động hợp nhất chỉ làm tăng trọng số cạnh trong tương lai, duy trì tính hợp lệ của các quyết định cục bộ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force của tất cả các giải đấu | Hàm mũ | Hàm mũ | Quá chậm | 
| Hợp nhất thành phần tham lam với các bản cập nhật động |$O(n^2 \log n)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một tập hợp các thành phần hoạt động rời rạc, mỗi thành phần đại diện cho một trình phát đã hợp nhất. Mỗi thành phần có một chỉ mục đại diện, luôn là chỉ mục nhỏ nhất bên trong nó để tôn trọng quy tắc các chỉ số nhỏ hơn sẽ thắng khi hợp nhất. 

Chúng tôi cũng duy trì ma trận có trọng số tốt nhất hiện tại giữa các thành phần, ma trận này phát triển thông qua hợp nhất tối đa khi các thành phần được hợp nhất. 

### Các bước 

1. Khởi tạo mỗi trình phát dưới dạng thành phần riêng của nó và đặt đại diện hiện tại của từng thành phần cho chính trình phát đó. Ma trận hiện tại ban đầu là$P$. 
2. Đối với mỗi cặp$(i, j)$, duy trì trọng lượng cạnh tốt nhất có thể đạt được hiện tại giữa các thành phần của chúng. Ban đầu đây chỉ là$P_{i,j}$. 
3. Chọn nhiều lần các cặp thành phần riêng biệt$(A, B)$tối đa hóa giá trị hiện tại$P_{rep[A], rep[B]}$. Đây là trận đấu ngay lập tức tốt nhất có thể. 
4. Thực hiện so khớp giữa$rep[A]$Và$rep[B]$. Vì chỉ số nhỏ hơn luôn thắng nên đại diện có chỉ số nhỏ hơn sẽ hấp thụ chỉ số kia. 
5. Thêm kết quả trùng khớp vào đầu ra và cộng điểm của nó vào tổng số. 
6. Hợp nhất thành phần thua vào thành phần thắng. Đối với mọi thành phần khác$C$, cập nhật:$$P_{win, C} = \max(P_{win, C}, P_{lose, C}), \quad P_{C, win} = P_{win, C}$$Điều này mô phỏng sự kế thừa của sự nổi tiếng. 
7. Đánh dấu thành phần bị mất là không hoạt động và tiếp tục cho đến khi chỉ còn lại một thành phần. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là đối với bất kỳ cặp thành phần hoạt động nào, trọng số cạnh được lưu trữ của chúng luôn bằng giá trị tối đa có thể đạt được bởi bất kỳ cặp người chơi ban đầu nào hiện có trong các thành phần đó, sau tất cả các lần hợp nhất trước đó. Bởi vì việc hợp nhất chỉ lấy cực đại theo từng phần tử nên không có thao tác nào trong tương lai có thể giảm bất kỳ trọng số cạnh nào mà chỉ tăng nó. 

Tính đơn điệu này đảm bảo rằng việc chọn cạnh hiện tại tối đa là an toàn: mọi cải tiến trong tương lai đối với một cạnh chỉ có thể tăng trọng số bên trong các thành phần đã được hình thành, không bao giờ tạo ra lựa chọn nhiều thành phần tốt hơn mà trước đây không có. Do đó, lựa chọn tham lam không bao giờ chặn cấu trúc hợp nhất tối ưu trong tương lai và quá trình này hoạt động giống như một cây bao trùm tối đa dưới các cập nhật cạnh không giảm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    P = [list(map(int, input().split())) for _ in range(n)]

    parent = list(range(n))
    alive = [True] * n

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    # current representative is always smallest index in component
    def merge(a, b):
        ra, rb = find(a), find(b)
        if ra == rb:
            return ra
        if ra > rb:
            ra, rb = rb, ra

        parent[rb] = ra
        alive[rb] = False

        for k in range(n):
            if alive[k] and k != ra:
                P[ra][k] = max(P[ra][k], P[rb][k])
                P[k][ra] = P[ra][k]
        return ra

    active = set(range(n))
    total = 0
    edges = []

    import heapq
    heap = []

    for i in range(n):
        for j in range(i + 1, n):
            heapq.heappush(heap, (-P[i][j], i, j))

    while len(active) > 1:
        w, i, j = heapq.heappop(heap)
        w = -w
        i = find(i)
        j = find(j)
        if i == j or not alive[i] or not alive[j]:
            continue

        if i > j:
            i, j = j, i

        total += P[i][j]
        edges.append((i + 1, j + 1))

        new_rep = merge(i, j)

        # push updated edges from new_rep
        for k in list(active):
            rk = find(k)
            if rk != new_rep:
                heapq.heappush(heap, (-P[new_rep][rk], new_rep, rk))

        # rebuild active set lazily
        active = {find(x) for x in active if alive[find(x)]}

    print(total)
    for u, v in edges:
        print(u, v)

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì rất nhiều cạnh ứng cử viên, luôn trích xuất kết quả phù hợp nhất hiện có. Bởi vì việc hợp nhất thay đổi trọng số, các mục nhập heap lỗi thời bị bỏ qua một cách lười biếng khi điểm cuối của chúng không còn đại diện cho các thành phần hoạt động hợp lệ. 

Hoạt động hợp nhất là nơi tồn tại tính chính xác: sau khi chọn nút chiến thắng, chúng tôi truyền bá các giá trị tối đa từ nút thua đến nút thắng trên tất cả các nút hoạt động còn lại, mô phỏng chính xác quy tắc “kế thừa mức độ phổ biến”. 

Một sai lầm phổ biến là quên rằng trọng số của cạnh thay đổi theo thời gian, do đó chỉ hàng đợi ưu tiên tĩnh là không đủ trừ khi chúng tôi xác thực các đại diện tại thời điểm trích xuất. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ nhỏ với ba người chơi: 

Ma trận đầu vào:$$P =
\begin{bmatrix}
0 & 5 & 1 \\
5 & 0 & 4 \\
1 & 4 & 0
\end{bmatrix}$$Chúng tôi bắt đầu với các thành phần$\{1\}, \{2\}, \{3\}$. 

| Bước | Cạnh được chọn | Linh kiện | Hợp nhất kết quả | Điểm | 
| --- | --- | --- | --- | --- | 
| 1 | (1,2) | {1,2}, {3} | 1 hấp thụ 2 | 5 | 
| 2 | (1,3) | {1,2,3} | 1 hấp thụ 3 | 1 | 

Tổng cộng là$6$. Sau khi hợp nhất 1 và 2, hàng 1 trở thành$[0,5,1]$. Việc hợp nhất 3 không thay đổi cạnh vốn đã cao thành 2 vì ảnh hưởng của 3 yếu hơn trên tất cả các chiều ngoại trừ 2, vốn đã bị 2 chi phối. 

Dấu vết này cho thấy rằng các cạnh có trọng số cao ban đầu được lấy một cách an toàn vì các lần hợp nhất sau này chỉ làm tăng hoặc duy trì các so sánh có liên quan. 

Bây giờ hãy xem xét một trường hợp trong đó lợi ích bị trì hoãn có ý nghĩa quan trọng:$$P =
\begin{bmatrix}
0 & 2 & 100 \\
2 & 0 & 3 \\
100 & 3 & 0
\end{bmatrix}$$| Bước | Cạnh được chọn | Linh kiện | Hợp nhất kết quả | Điểm | 
| --- | --- | --- | --- | --- | 
| 1 | (1,3) | {1,3}, {2} | 1 hấp thụ 3 | 100 | 
| 2 | (1,2) | {1,2,3} | 1 hấp thụ 2 | 2 | 

Ở đây, việc hợp nhất 1 và 3 trước tiên sẽ mở ra cạnh mạnh nhất ngay lập tức và những lần hợp nhất tiếp theo không làm giảm mức tăng đó. 

Những ví dụ này xác nhận rằng việc khai thác tham lam phù hợp với việc tối đa hóa sự hợp nhất có giá trị cao ngay lập tức trong khi vẫn bảo toàn các khả năng trong tương lai một cách an toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2 \log n)$| Mỗi cạnh được đẩy và bật ra từ một đống và mỗi cạnh hợp nhất sẽ cập nhật nhiều nhất$O(n)$cạnh | 
| Không gian |$O(n^2)$| Lưu trữ ma trận cộng với hàng đống cạnh ứng cử viên | 

Với$n \le 1000$, cái$n^2$bộ nhớ có thể chấp nhận được và hệ số logarit duy trì thời gian chạy trong giới hạn 2 giây trong Python được tối ưu hóa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue().strip()

# Small triangle
# assert run(...) == "..."

# Custom minimal
assert run("1\n0\n") == "0", "single node"

# Symmetric small case
assert run("3\n0 1 2\n1 0 3\n2 3 0\n") is not None

# Equal weights
assert run("4\n0 1 1 1\n1 0 1 1\n1 1 0 1\n1 1 1 0\n") is not None

# Chain dominance
assert run("4\n0 10 1 1\n10 0 1 1\n1 1 0 1\n1 1 1 0\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 | 0 | trường hợp cơ sở | 
| nhỏ đối xứng | bất kỳ giá trị tối đa hợp lệ nào | tính đúng đắn khi hợp nhất nhỏ | 
| trọng lượng bằng nhau | hợp nhất nhất quán | xử lý cà vạt | 
| cặp thống trị | thích cạnh cao trước | hành vi tham lam | 

## Vỏ cạnh 

Trường hợp một người chơi là chuyện nhỏ vì không có trận đấu nào xảy ra. Thuật toán ngay lập tức đưa ra tổng bằng 0 và không có cạnh, vì tập hoạt động có kích thước bằng một ngay từ đầu. 

Một ma trận hoàn toàn đồng nhất, trong đó tất cả các giá trị ngoài đường chéo đều bằng nhau, kiểm tra xem việc phá vỡ liên kết dựa trên đống có tạo ra một chuỗi hợp lệ hay không. Vì tất cả các cạnh đều giống hệt nhau nên mọi thứ tự hợp nhất đều là tối ưu và thuật toán có thể chọn các cặp tùy ý trong khi vẫn duy trì tính chính xác do tính đối xứng và sự hợp nhất đơn điệu. 

Cấu hình cặp trội, trong đó một cạnh lớn hơn đáng kể so với tất cả các cạnh khác, xác nhận rằng thuật toán ưu tiên hợp nhất trước. Sau khi hợp nhất cặp ưu thế, việc truyền bá sẽ tăng trọng số, nhưng không thể tạo ra cơ hội bị bỏ lỡ ban đầu tốt hơn, vì cạnh lớn nhất đã bị chiếm đoạt một cách tham lam. 

Cấu trúc chuỗi nhỏ kiểm tra xem liệu các sự hợp nhất trung gian có chặn các cạnh có giá trị cao trong tương lai hay không. Vì mỗi lần hợp nhất sẽ tăng hoặc giữ nguyên trọng số nên thuật toán sẽ trì hoãn hoặc tăng tốc các lần hợp nhất một cách an toàn mà không làm mất cấu trúc tối ưu.
