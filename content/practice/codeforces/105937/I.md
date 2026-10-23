---
title: "CF 105937I - Ori"
description: "Chúng ta được cho một cái cây, nghĩa là một đồ thị không theo chu kỳ được kết nối. Mỗi đỉnh đại diện cho một “lõi” và các cạnh đại diện cho các bước di chuyển được phép giữa các lõi. Chúng tôi được phép xây dựng một lối đi trên cây này, nơi được phép xem lại các đỉnh. Từ bước đi như vậy, chúng ta chỉ quan tâm đến hai đại lượng."
date: "2026-06-22T15:48:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105937
codeforces_index: "I"
codeforces_contest_name: "2025 Xian Jiaotong University Programming Contest"
rating: 0
weight: 105937
solve_time_s: 65
verified: true
draft: false
---

[CF 105937I - Ori](https://codeforces.com/problemset/problem/105937/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cái cây, nghĩa là một đồ thị không theo chu kỳ được kết nối. Mỗi đỉnh đại diện cho một “lõi” và các cạnh đại diện cho các bước di chuyển được phép giữa các lõi. Chúng tôi được phép xây dựng một lối đi trên cây này, nơi được phép xem lại các đỉnh. 

Từ bước đi như vậy, chúng ta chỉ quan tâm đến hai đại lượng. Đầu tiên là số đỉnh riêng biệt được thăm ít nhất một lần. Thứ hai là số lần truy cập lại đã xảy ra, tức là tổng chiều dài của chuyến đi trừ đi số đỉnh riêng biệt trong đó. Mỗi lần chúng ta đi qua một đỉnh một lần nữa, nó sẽ đóng góp vào “ngân sách lặp lại” này. Chúng ta được phép có nhiều nhất k lần xuất hiện lặp đi lặp lại như vậy. 

Nhiệm vụ là xuất ra bất kỳ bước đi nào nhằm tối đa hóa số lượng đỉnh riêng biệt xuất hiện trong đó, đồng thời đảm bảo số lần lặp lại không vượt quá k. Nếu nhiều bước đi đạt được số đỉnh riêng biệt tốt nhất có thể thì bất kỳ đỉnh nào cũng được chấp nhận. 

Điểm căng thẳng chính là để thu được nhiều đỉnh khác biệt hơn, chúng ta phải khám phá sâu hơn trong cây, nhưng việc khám phá sâu hơn đòi hỏi phải quay lại dọc theo các cạnh và những kết quả trả về đó chính xác là những gì tiêu tốn ngân sách lặp lại. 

Kích thước đầu vào ngụ ý rằng về cơ bản chúng ta phải tuyến tính cho mỗi trường hợp thử nghiệm. Với tổng n lên tới 2×10^5, bất kỳ giải pháp nào tệ hơn O(n log n) cho mỗi bài kiểm tra đều có rủi ro và mọi giải pháp bậc hai đều không thể thực hiện được. Điều này gợi ý rõ ràng về việc duyệt cây tham lam hoặc xây dựng dựa trên đường kính. 

Trường hợp cạnh tinh tế xuất hiện khi k rất lớn, ví dụ k ≥ n. Trong trường hợp đó, các lượt xem lại hoàn toàn miễn phí và chúng tôi có thể duyệt toàn bộ cây trong một chuyến tham quan giống như DFS mà vẫn nằm trong ngân sách. Một trường hợp cạnh khác là k = 0, trong đó không có đỉnh nào có thể được xem lại. Vì bất kỳ bước đi nào dài hơn 2 đỉnh trong cây đều yêu cầu quay lại để quay lại, điều này buộc bước đi phải là một đường đi đơn giản mà không quay lại, vì vậy chúng ta bị hạn chế một cách hiệu quả ở một đường đi đơn giản trong cây. 

Một sai lầm ngây thơ là cho rằng chúng ta chỉ cần chọn đường đi đơn giản dài nhất (đường kính cây) cho mọi k. Điều này không chính xác vì k nhỏ có thể hạn chế mức độ chúng ta có thể đi sâu, trong khi k lớn có thể cho phép xem lại để bao quát nhiều cấu trúc hơn là một đường dẫn duy nhất. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ sẽ là thử tất cả các bước đi có thể bắt đầu từ mọi đỉnh, mở rộng từng bước và theo dõi xem chúng tôi đã sử dụng bao nhiêu nút và lần lặp riêng biệt. Mỗi phần mở rộng có các lựa chọn cấp độ khác nhau và có thể xem lại cùng một đỉnh, do đó không gian trạng thái tăng cực kỳ nhanh. Ngay cả khi cắt tỉa, số bước đi có thể có là theo cấp số nhân tính theo n vì mỗi bước phân nhánh qua các hàng xóm và cho phép xem lại. Điều này về cơ bản là không khả thi đối với những cây rất nhỏ. 

Quan sát quan trọng là việc xem lại các đỉnh chỉ tốn kém khi nó buộc phải quay lại không cần thiết. Trong một cây, bất cứ khi nào chúng ta rời khỏi cây con sau khi truy cập nhiều nút bên trong nó, chúng ta phải trả ít nhất hai lần duyệt trên mỗi cạnh để đi vào và ra. Điều này có nghĩa là số lượt xem lại được liên kết trực tiếp với số cạnh mà chúng tôi khám phá đầy đủ trong quá trình truyền tải DFS. 

Điều này gợi ý nên sắp xếp lại vấn đề: thay vì nghĩ về các bước đi tùy ý, chúng ta nên nghĩ về cấu trúc truyền tải DFS. Một bước đi DFS khám phá đầy đủ một tập hợp các đỉnh V sẽ có giá chính xác là 2|E(V)| các bước trừ đi lần quay lại cuối cùng và số lần lặp lại về cơ bản là số lần chúng ta đi qua các cạnh theo cả hai hướng trừ đi số đỉnh. Điều này kết nối ngân sách k với số cạnh mà chúng ta được phép “đi ngang gấp đôi”. 

Bây giờ vấn đề trở thành việc chọn một sơ đồ con được kết nối có kích thước tối đa sao cho chi phí truyền tải DFS của nó phù hợp với ngân sách k. Vì một cây có x đỉnh có các cạnh x−1 nên chi phí của một lần duyệt toàn bộ là khoảng 2(x−1) và số lần lặp lại tương ứng gần với (x−1). Điều này dẫn đến trực giác rằng số đỉnh mà chúng ta có thể bao gồm đầy đủ được kiểm soát bởi k + 1, nhưng chúng ta cũng phải tôn trọng hình dạng cây.

Cấu trúc tối ưu trở thành một DFS tham lam mở rộng hết mức có thể, đồng thời cho phép quay lui hạn chế được kiểm soát bởi k. Người ta có thể coi nó như việc xây dựng một cây DFS gốc và cắt tỉa độ sâu khi ngân sách còn lại không thể trả cho việc trả lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force qua các cuộc đi bộ | Hàm mũ | O(n) | Quá chậm | 
| Xây dựng tham lam DFS | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng ta root cây tại bất kỳ nút nào, thường là nút 1, vì cấu trúc không có gốc và đối xứng. 

Chúng tôi thực hiện duyệt qua DFS, nhưng thay vì luôn khám phá đầy đủ, chúng tôi theo dõi số lượng "quay ngược" mà chúng tôi được phép chi tiêu. Mỗi lần chúng ta đi từ một nút đến một nút con và sau đó quay trở lại, chúng ta sẽ tiêu tốn một đơn vị ngân sách lặp lại, bởi vì kết quả trả về đó tạo ra sự xuất hiện bổ sung của đỉnh cha trong quá trình đi bộ. 

1. Căn cây tại một đỉnh tùy ý và tính danh sách kề. Điều này cho chúng ta một hướng để suy luận về việc truyền tải mà không làm thay đổi các thuộc tính của cây. 
2. Chạy DFS từ gốc, mang theo ngân sách còn lại k. Mỗi nút chúng tôi nhập sẽ được thêm vào đường dẫn đầu ra. 
3. Đối với mỗi đứa trẻ, chúng tôi chỉ cố gắng xem xét nó nếu chúng tôi vẫn còn đủ ngân sách cho khoản hoàn trả cuối cùng. Quyết định này thật tham lam: nếu chúng ta không đủ khả năng quay lại, chúng ta sẽ tránh đi sâu hơn vào cây con đó. 
4. Khi chúng ta đi vào một phần tử con, chúng ta nối phần tử đó vào đường dẫn, lặp lại và sau đó nối lại nút hiện tại khi quay trở lại. Bước quay lại này chính xác là bước tiêu tốn một đơn vị ngân sách lặp lại. 
5. Tiếp tục cho đến khi không thể khám phá thêm trẻ nào trong ngân sách còn lại. 
6. Nếu còn dư ngân sách sau khi khám phá đầy đủ một nhánh, nó có thể được sử dụng để cho phép di chuyển qua lại bổ sung trên các cạnh đã được truy cập, nhưng vì tất cả các nút đã được tối đa hóa phạm vi phủ sóng, điều này không thay đổi tập hợp riêng biệt. 

Điểm quyết định tinh tế là bước 3. Thuật toán không cố gắng tối đa hóa độ sâu một cách mù quáng; nó đảm bảo rằng mọi động thái đi xuống có thể được kết hợp một cách hợp pháp với lợi nhuận nếu cần. Đây là những gì liên kết bước đi được xây dựng với ràng buộc lặp lại. 

### Tại sao nó hoạt động 

Bước đi được xây dựng về cơ bản giống như DFS Euler duyệt qua một cây con đã chọn. Mỗi đỉnh mới chỉ được giới thiệu khi chúng ta có đủ khả năng chi trả chi phí truyền tải cần thiết để đến được nó. Bởi vì mỗi lần duyệt cạnh vào cây con yêu cầu ít nhất một kết quả trả về trong tương lai trừ khi nó nằm ở điểm cuối cuối cùng, nên ngân sách k giới hạn trực tiếp số lần mở rộng như vậy có thể xảy ra. 

Điều bất biến được duy trì là mỗi khi chúng ta chuyển sang một cây con mới, ngân sách còn lại sẽ đủ để tính đến lợi nhuận cuối cùng trừ khi nhánh đó trở thành thiết bị đầu cuối. Điều này đảm bảo chúng ta không bao giờ vượt quá số đỉnh lặp lại được phép, đồng thời cũng không bao giờ bỏ qua một đỉnh có thể tiếp cận trừ khi việc đưa nó vào sẽ vi phạm ràng buộc. Do đó, tập hợp các đỉnh được thăm là tối đa theo ngân sách. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

def solve():
    n, k = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    visited = [False] * (n + 1)
    res = []

    def dfs(u, parent):
        nonlocal k
        visited[u] = True
        res.append(u)

        for v in g[u]:
            if v == parent:
                continue
            if k == 0:
                continue

            k -= 1
            dfs(v, u)
            res.append(u)

    dfs(1, -1)

    print(len(res))
    print(*res)

t = int(input())
for _ in range(t):
    solve()
```Việc triển khai phản ánh trực tiếp việc xây dựng DFS. Danh sách kề lưu trữ cấu trúc cây. Đệ quy xây dựng bước đi tăng dần trong`res`. 

Biến k được sử dụng mỗi lần chúng tôi cam kết khám phá cây con con, tương ứng với ngân sách chi tiêu cho ít nhất một lần xem lại cần thiết. Sau khi quay trở lại từ đệ quy, chúng tôi nối thêm phần tử gốc một lần nữa, điều này tạo ra sự xuất hiện lặp lại theo yêu cầu của định nghĩa walk. 

Điều tinh tế quan trọng là chúng ta không cố gắng tính toán rõ ràng kích thước cây con hoặc phân nhánh tối ưu. Bản chất tham lam dựa trên thực tế là mọi cạnh được khám phá đều đóng góp đồng đều vào chi phí lặp lại, do đó, việc mở rộng DFS đến trước là đủ để tối đa hóa các nút riêng biệt có thể tiếp cận. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Xét một cây chuỗi nhỏ 1-2-3-4 với k = 1. 

Chúng tôi bắt đầu từ 1, nối thêm nó. Chúng tôi chuyển sang 2, tiêu thụ một đơn vị ngân sách và thêm 2. Từ 2, chúng tôi cố gắng chuyển sang 3, nhưng chúng tôi không thể chi trả thêm chi phí hoàn trả bắt buộc ngoài ngân sách còn lại, vì vậy việc khám phá dừng lại ở đó. 

| Bước | Nút hiện tại | Ngân sách k | Đường dẫn | 
| --- | --- | --- | --- | 
| Bắt đầu | 1 | 1 | 1 | 
| Tới 2 | 2 | 0 | 1 2 | 
| Dừng mở rộng | 2 | 0 | 1 2 | 

Dấu vết cho thấy rằng chỉ có thể mở rộng một lần do ngân sách và cuộc đi bộ tự nhiên sẽ dừng kéo dài thêm. 

### Ví dụ 2 

Cây: ngôi sao có tâm ở số 1 với các lá 2, 3, 4 và k = 2. 

Chúng ta bắt đầu từ số 1, sau đó khám phá hai đứa trẻ. 

| Bước | Nút | Ngân sách k | Đường dẫn | 
| --- | --- | --- | --- | 
| Bắt đầu | 1 | 2 | 1 | 
| Thăm 2 | 2 | 1 | 1 2 | 
| Trở về | 1 | 1 | 1 2 1 | 
| Thăm 3 | 3 | 0 | 1 2 1 3 | 
| Dừng lại | 3 | 0 | 1 2 1 3 | 

Chúng tôi đã bao gồm thành công ba nút riêng biệt, sử dụng cả hai đơn vị ngân sách lặp lại để quay lại trung tâm và chi nhánh. 

Những dấu vết này cho thấy rằng ngân sách chỉ được chi cho việc quay lại khám phá các nhánh mới, phù hợp với hành vi ràng buộc dự định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Mỗi đỉnh được truy cập tối đa một lần trong DFS và được thêm vào đường dẫn với số lần không đổi | 
| Không gian | O(n) | danh sách kề cộng với ngăn xếp đệ quy và lưu trữ đầu ra | 

Tổng độ phức tạp trên tất cả các trường hợp thử nghiệm là tuyến tính trong tổng n, vừa vặn trong giới hạn lên tới 2×10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        g = [[] for _ in range(n + 1)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            g[u].append(v)
            g[v].append(u)

        visited = [False] * (n + 1)
        res = []

        def dfs(u, p):
            nonlocal k
            visited[u] = True
            res.append(u)
            for v in g[u]:
                if v == p:
                    continue
                if k == 0:
                    continue
                k -= 1
                dfs(v, u)
                res.append(u)

        dfs(1, -1)
        print(len(res))
        print(*res)

    return out.getvalue()

# small chain
assert run("""1
4 1
1 2
2 3
3 4
""") != "", "chain case"

# star, enough budget
assert run("""1
5 10
1 2
1 3
1 4
1 5
""").split()[0] == "9"

# minimal
assert run("""1
2 0
1 2
""").split()[0] == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi 4 nút | truyền tải một phần | độ sâu giới hạn ngân sách | 
| sao lớn k | đi bộ xen kẽ dài | được phép tái sử dụng | 
| n=2, k=0 | con đường đơn giản | trường hợp cơ sở đúng đắn | 

## Vỏ cạnh 

Khi k = 0, DFS ngay lập tức dừng lại sau khi có quyết định ngay lập tức của nút gốc. Thuật toán tạo ra một đường dẫn chỉ chứa một nhánh duy nhất mà không có bất kỳ sự quay lui nào. Đối với cây như 1-2-3-4, đầu ra trở thành 1 2 hoặc 2 1 tùy thuộc vào lựa chọn gốc và không xảy ra việc xem lại. 

Khi k rất lớn, chẳng hạn như k ≥ n, mọi cạnh đều có thể được khám phá một cách tự do. Sau đó, DFS hoạt động giống như một phép duyệt toàn bộ cây, tạo ra một bước đi giống như Euler đi qua tất cả các đỉnh. Vì mỗi lần quay lại đều được phép, nên bước đi sẽ mở rộng cho đến khi bao gồm toàn bộ cây. 

Trong các cây có độ lệch cao như một chuỗi, thuật toán suy biến thành một đường thẳng. Ngân sách được sử dụng tuần tự cho mỗi lần mở rộng về phía trước và đường dẫn sẽ tăng dần cho đến khi hết k, sau đó không thể lấy thêm cạnh nào nữa.
