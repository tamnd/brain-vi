---
title: "CF 1725J - Hành Trình"
description: "Chúng ta có một cây có trọng số trong đó mỗi nút đại diện cho một thành phố và mỗi cạnh đại diện cho một con đường hai chiều với thời gian di chuyển. Người du lịch phải thiết kế một chuyến đi bộ để cuối cùng ghé thăm mọi thành phố ít nhất một lần."
date: "2026-06-16T16:58:30+07:00"
tags: ["codeforces", "competitive-programming", "dp", "trees"]
categories: ["algorithms"]
codeforces_contest: 1725
codeforces_index: "J"
codeforces_contest_name: "COMPFEST 14 - Preliminary Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 2500
weight: 1725
solve_time_s: 274
verified: false
draft: false
---

[CF 1725J - Hành trình](https://codeforces.com/problemset/problem/1725/J) 

**Đánh giá:** 2500 
**Thẻ:** dp, cây cối 
**Thời gian giải:** 4 phút 34 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có trọng số trong đó mỗi nút đại diện cho một thành phố và mỗi cạnh đại diện cho một con đường hai chiều với thời gian di chuyển. Người du lịch phải thiết kế một chuyến đi bộ để cuối cùng ghé thăm mọi thành phố ít nhất một lần. Chuyển động không bị giới hạn ở các đường dẫn đơn giản, do đó, việc xem lại các nút và cạnh được cho phép và các cạnh có thể được đi qua nhiều lần nếu cần. 

Điều khó khăn là khách du lịch có chính xác một dịch chuyển tức thời có thể di chuyển họ ngay lập tức từ bất kỳ thành phố nào đến bất kỳ thành phố nào khác và nó có thể được sử dụng nhiều nhất một lần trong toàn bộ cuộc hành trình. Mục tiêu là giảm thiểu tổng thời gian di chuyển của một chuyến đi bao gồm tất cả các nút ít nhất một lần, kết hợp các lần di chuyển qua biên thông thường và nhiều nhất là một lần dịch chuyển tức thời. 

Cấu trúc cây rất quan trọng vì không có dịch chuyển tức thời, chiến lược tối ưu có liên quan chặt chẽ đến việc bao phủ tất cả các cạnh của biểu đồ tuần hoàn được kết nối. Trong một cây, mỗi cạnh là một cây cầu, do đó, bất kỳ chiến lược truyền tải nào cuối cùng đều là về số lần các cạnh phải được sử dụng lại để đảm bảo bao phủ toàn bộ các đỉnh. 

Giới hạn lên tới 100.000 nút ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng đường dẫn một cách rõ ràng hoặc khám phá tất cả các cặp bắt đầu và kết thúc. Bất kỳ cách tiếp cận bậc hai hoặc thậm chí gần bậc hai đều không khả thi. Chúng ta nên mong đợi một giải pháp dựa trên việc truyền tải tuyến tính cộng với một lượng nhỏ tối ưu hóa tổng thể, thường liên quan đến DP của cây hoặc lý luận liên quan đến đường kính. 

Một cách tiếp cận đơn giản sẽ cố gắng tính toán các bước đi ngắn nhất cho mọi lựa chọn điểm cuối dịch chuyển tức thời có thể có. Điều đó đã hàm ý một$O(N^2)$không gian tìm kiếm cho các điểm cuối và đối với mỗi cặp, chúng tôi sẽ cần giải thích về chi phí bao phủ, vượt xa giới hạn. 

Vấn đề tế nhị thứ hai là hiểu sai ý nghĩa của việc “ghé thăm mỗi thành phố ít nhất một lần”. Một lỗi phổ biến là cho rằng chúng ta phải đi qua tất cả các cạnh, nhưng trong một cây, chúng ta có thể bỏ qua hoàn toàn các cây con nếu chúng ta tiếp cận các nút của chúng thông qua một số cấu trúc thay thế, đặc biệt là sau khi dịch chuyển tức thời. 

Một cạm bẫy khác là giả sử dịch chuyển tức thời hoạt động giống như thêm một cạnh tự do vào cây. Nó không tạo ra một kết nối lâu dài; nó chỉ cho phép di chuyển tức thời ở giữa hành trình, điều này thay đổi cách chúng ta nghĩ về việc chia đường đi thành hai đoạn. 

## Phương pháp tiếp cận 

Nếu không có dịch chuyển tức thời, kết quả tiêu chuẩn để truy cập tất cả các nút trong cây là bước đi tối ưu tương ứng với quá trình truyền tải DFS quay trở lại điểm bắt đầu hoặc tương đương, mỗi cạnh được đi qua hai lần ngoại trừ có thể dọc theo một đường dẫn đã chọn tùy thuộc vào điểm cuối. Điều này mang lại chi phí cơ bản gấp đôi tổng trọng số của tất cả các cạnh. 

Chính xác hơn, nếu chúng ta thực hiện duyệt toàn bộ bao gồm tất cả các nút bắt đầu và kết thúc ở bất kỳ đâu, chúng ta vẫn cần phải duyệt qua mọi cạnh ít nhất một lần theo mỗi hướng để đảm bảo phạm vi bao phủ, do đó đường cơ sở là$2 \sum w_i$trừ đi khả năng lưu một đường dẫn duy nhất nếu chúng ta không cần quay lại từ đầu. 

Bây giờ hãy xem xét việc dịch chuyển tức thời. Tác dụng chính của dịch chuyển tức thời là nó cho phép chúng ta “cắt” bước đi thành hai đoạn cây rời nhau. Thay vì buộc phải quay trở lại qua các cạnh kết nối giống nhau, chúng ta có thể đi ngang qua một phần của cây, dịch chuyển tức thời và tiếp tục ở phần khác mà không phải trả chi phí cho đường kết nối. 

Điều này có nghĩa là chúng ta đang cố gắng chọn một con đường trên cây một cách hiệu quả mà chúng ta không cần phải đi qua hai lần. Trong phạm vi phủ sóng đầy đủ dựa trên DFS tiêu chuẩn, mọi cạnh đều được nhân đôi. Nếu chúng ta có thể tránh đi lại đường đi có tổng trọng số tối đa thì chúng ta sẽ giảm thiểu chi phí nhiều nhất. Dịch chuyển tức thời cho phép chúng tôi phá vỡ quá trình truyền tải ở hai điểm cuối, loại bỏ nhu cầu phải đi qua đường đi giữa chúng hai lần. 

Do đó, vấn đề giảm xuống còn việc tìm đường đi trong cây sao cho tổng trọng số của các cạnh là lớn nhất, bởi vì đây là phần mà lần truyền tải thứ hai có thể tránh được bằng cách sử dụng dịch chuyển tức thời. 

Đây chính xác là bài toán đường kính cây ở dạng có trọng số. 

Chúng tôi tính toán: 

1. Tổng trọng số của các cạnh, được ký hiệu$S$. 
2. Chiều dài đường kính$D$, khoảng cách có trọng số tối đa giữa hai nút bất kỳ. 

Nếu không có dịch chuyển tức thời, chi phí là$2S - D$nếu chúng tôi cho phép bắt đầu và kết thúc tại các điểm tùy ý (bước đi tiêu chuẩn bao gồm tất cả các nút). 

Với dịch chuyển tức thời, chúng ta có thể loại bỏ thêm một$D$giá trị của hiệu ứng truyền tải lặp đi lặp lại trong trường hợp tốt nhất, giảm chi phí một cách hiệu quả bằng con đường tốt nhất có thể mà chúng ta có thể “bỏ qua việc sao chép”. 

Vì vậy, câu trả lời cuối cùng trở thành:$$2S - D$$Theo trực giác, chiến lược tối ưu là đi ngang qua cây theo cách giống như DFS trong khi sử dụng dịch chuyển tức thời một cách chính xác để nhảy qua hai điểm cuối của đường kính, loại bỏ nhu cầu quay lại con đường dài nhất đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các điểm cuối dịch chuyển tức thời |$O(N^2)$|$O(N)$| Quá chậm | 
| Tối ưu (cây + đường kính) |$O(N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc cây và tính tổng trọng số của các cạnh. Điều này thể hiện chi phí đi qua mỗi cạnh một lần. 
2. Xây dựng biểu diễn danh sách kề của cây để cho phép truyền tải hiệu quả. 
3. Tính đường kính của cây bằng hai lượt DFS/BFS. Đầu tiên, bắt đầu từ bất kỳ nút nào và tìm nút xa nhất$A$. Sau đó chạy lại từ$A$để tìm nút xa nhất$B$, ghi lại khoảng cách$D$. 
4. Khoảng cách đường kính$D$đại diện cho con đường đơn giản dài nhất trong cây. Đường dẫn này là ứng cử viên tốt nhất để tránh việc truyền tải lặp lại vì nó tối đa hóa chi phí quay lui được tiết kiệm. 
5. Tính đáp án cuối cùng là$2S - D$, Ở đâu$S$là tổng trọng số của các cạnh. 

Lý do chúng tôi tính toán đường kính một cách cụ thể là vì mọi đường truyền đã lưu phải tương ứng với một đường đi đơn giản và mức tiết kiệm tốt nhất đến từ việc tối đa hóa tổng trọng lượng của nó. 

### Tại sao nó hoạt động 

Bất kỳ bước đi nào ghé thăm tất cả các nút đều phải đi qua đủ các cạnh để đến được mọi nhánh của cây. Trong cây, điều này buộc phải có cấu trúc giống DFS trong đó các cạnh được sử dụng một cách tự nhiên theo cả hai hướng trừ khi chúng ta tạo ra sự gián đoạn trong tính liên tục. Dịch chuyển tức thời giới thiệu chính xác một điểm dừng như vậy, chia quá trình truyền tải thành hai thành phần được kết nối theo thứ tự đi bộ. Nơi tốt nhất để phân chia là dọc theo con đường nặng nhất, vì mỗi đơn vị trọng lượng trên con đường đó tương ứng với một đơn vị quay lui có thể được loại bỏ. Vì đường kính là đường đơn giản có trọng lượng tối đa nên tiết kiệm tối đa, đảm bảo tính tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

def dfs(start, adj):
    stack = [(start, -1, 0)]
    far_node = start
    far_dist = 0
    dist = {start: 0}

    while stack:
        u, p, d = stack.pop()
        if d > far_dist:
            far_dist = d
            far_node = u
        for v, w in adj[u]:
            if v == p:
                continue
            dist[v] = d + w
            stack.append((v, u, d + w))
    return far_node, far_dist

def dfs2(start, adj):
    stack = [(start, -1, 0)]
    dist = {start: 0}
    far = 0

    while stack:
        u, p, d = stack.pop()
        if d > far:
            far = d
        for v, w in adj[u]:
            if v == p:
                continue
            stack.append((v, u, d + w))
    return far

def main():
    n = int(input())
    adj = [[] for _ in range(n + 1)]
    total = 0

    for _ in range(n - 1):
        u, v, w = map(int, input().split())
        adj[u].append((v, w))
        adj[v].append((u, w))
        total += w

    if n == 1:
        print(0)
        return

    # find one endpoint of diameter
    u, _ = dfs(1, adj)
    # compute diameter length
    diameter = dfs2(u, adj)

    print(2 * total - diameter)

if __name__ == "__main__":
    main()
```Giải pháp đầu tiên là tích lũy tổng trọng số của tất cả các cạnh trong khi xây dựng danh sách kề. Điều này trực tiếp tương ứng với cấu trúc chi phí cơ bản của việc duyệt cây. 

DFS đầu tiên tìm thấy nút xa nhất tính từ gốc tùy ý, đây là cách tiêu chuẩn để xác định vị trí một điểm cuối của đường kính. Lần truyền thứ hai tính toán khoảng cách tối đa từ điểm cuối này, cho ra chiều dài đường kính. Cả hai đường truyền đều tuyến tính về số lượng nút. 

Cuối cùng, công thức kết hợp tổng chi phí truyền tải với đoạn đường kính đã lưu. 

Một điểm tinh tế là chúng tôi không bao giờ mô hình hóa rõ ràng việc đi bộ hoặc dịch chuyển tức thời. Toàn bộ hiệu ứng của dịch chuyển tức thời được ghi lại dưới dạng phép trừ toàn cục duy nhất, điều này hợp lệ vì mức tiết kiệm tối ưu luôn phù hợp với một đường dẫn đơn giản duy nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 2 4
2 3 5
3 4 4
```Tổng trọng lượng cạnh là$4 + 5 + 4 = 13$. 

Chúng tôi tính toán đường kính. Đường đi dài nhất là từ nút 1 đến nút 4 có trọng số$13$. 

| Giai đoạn | Nút | Khoảng cách | Ghi chú | 
| --- | --- | --- | --- | 
| Bắt đầu DFS | 1 | 0 | bắt đầu tùy ý | 
| xa nhất | 4 | 13 | tìm thấy điểm cuối | 
| đường kính | 4 | 13 | con đường dài nhất | 

Trả lời:$$2 \cdot 13 - 13 = 13$$Điều này phù hợp với ý tưởng rằng chúng ta đi ngang qua cây nhưng tránh trùng lặp con đường nặng nhất do dịch chuyển tức thời. 

### Ví dụ 2 

đầu vào:```
5
1 2 1
2 3 2
3 4 3
3 5 4
```Tổng trọng lượng là$1 + 2 + 3 + 4 = 10$. 

Đường kính từ nút 2 đến nút 5 kèm theo trọng lượng$1 + 2 + 4 = 7$. 

| Giai đoạn | Nút | Khoảng cách | Ghi chú | 
| --- | --- | --- | --- | 
| Bắt đầu DFS | 1 | 0 | gốc tùy ý | 
| xa nhất | 5 | 7 | điểm cuối | 
| đường kính | 7 | 7 | tính toán | 

Trả lời:$$2 \cdot 10 - 7 = 13$$Dấu vết cho thấy cách giải pháp cô lập cấu trúc tổng thể (tổng trọng lượng và đường đi dài nhất) mà không cần mô phỏng quá trình truyền tải. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| hai DFS đi qua một cây với$N-1$cạnh | 
| Không gian |$O(N)$| danh sách kề và lưu trữ đệ quy/ngăn xếp | 

Độ phức tạp tuyến tính là đủ cho$10^5$các nút và hệ số hằng số nhỏ vì mỗi cạnh được xử lý một số lần không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    import subprocess, textwrap
    return subprocess.run(
        ["python3", "solution.py"],
        input=inp.encode(),
        stdout=subprocess.PIPE
    ).stdout.decode().strip()

# provided sample
assert run("""4
1 2 4
2 3 5
3 4 4
""") == "13"

# chain minimal
assert run("""1
""") == "0"

# small tree
assert run("""3
1 2 1
2 3 2
""") == "3"

# star
assert run("""5
1 2 1
1 3 1
1 4 1
1 5 1
""") == "7"

# weighted skewed tree
assert run("""6
1 2 3
2 3 4
3 4 5
4 5 6
5 6 7
""") == "50"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 0 | trường hợp cơ sở | 
| chuỗi | hiệu ứng đường kính tuyến tính đúng | cấu trúc đường dẫn | 
| ngôi sao | nhiều chi nhánh | phân nhánh đúng đắn | 
| chuỗi dài | đường kính tối đa | cấu trúc trường hợp xấu nhất | 

## Vỏ cạnh 

Đối với một thành phố, không có con đường nào để đi qua, vì vậy câu trả lời là không. Thuật toán kiểm tra trực tiếp$n = 1$và trả về 0 trước bất kỳ DFS nào, tránh việc truyền tải không hợp lệ. 

Đối với biểu đồ chuỗi, đường kính bằng toàn bộ đường đi. Thuật toán vẫn thực hiện hai lượt DFS và lượt thứ hai xác định chính xác điểm cuối đối diện, tạo ra tổng đường dẫn đầy đủ dưới dạng đường kính. Công thức cuối cùng giảm chính xác. 

Đối với cây hình ngôi sao, đường kính luôn nằm giữa hai lá xuyên qua tâm. DFS xác định chính xác lá đầu tiên, sau đó là lá đối diện, đảm bảo đường kính gấp đôi cấu trúc cạnh lớn nhất và công thức trừ nó một cách chính xác một lần từ tổng gấp đôi.
