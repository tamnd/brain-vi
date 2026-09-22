---
title: "CF 104786B - Trò chơi John và cái cây"
description: "Chúng ta được cho một cây có (N) nút. John muốn chọn các cặp nút rời nhau, với hạn chế là một cặp chỉ hợp lệ nếu khoảng cách giữa hai nút dọc theo cây là chẵn."
date: "2026-06-28T14:36:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104786
codeforces_index: "B"
codeforces_contest_name: "FIICode2023Round1"
rating: 0
weight: 104786
solve_time_s: 81
verified: true
draft: false
---

[CF 104786B - Trò chơi John và cái cây](https://codeforces.com/problemset/problem/104786/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với\(N\)nút. John muốn chọn các cặp nút rời nhau, với hạn chế là một cặp chỉ hợp lệ nếu khoảng cách giữa hai nút dọc theo cây là chẵn. Mỗi nút có thể xuất hiện trong nhiều nhất một cặp đã chọn và mục tiêu là sử dụng mỗi nút chính xác một lần trong một số cặp. 

Câu hỏi đơn giản là liệu một cặp đôi hoàn hảo có tồn tại theo quy tắc này hay không. 

Ràng buộc\(N \le 5 \cdot 10^5\)loại trừ bất cứ điều gì cố gắng kiểm tra các cặp một cách rõ ràng hoặc tìm kiếm các kết quả khớp. Việc kiểm tra bậc hai trên tất cả các cặp nút sẽ yêu cầu khoảng\(10^{11}\)trong trường hợp xấu nhất vượt xa thời hạn. Ngay cả các phương pháp tiếp cận dựa trên khối hoặc dựa trên dòng chảy cũng không cần thiết và sẽ quá chậm hoặc quá mức cần thiết đối với những gì hóa ra lại là quan sát cấu trúc về cây cối. 

Một vấn đề tế nhị sẽ xuất hiện nếu một người cố gắng lý luận cục bộ. Ví dụ, thật thú vị khi nghĩ rằng vì chúng ta có thể ghép các nút ở khoảng cách đều nhau, nên chúng ta có thể cần phải xây dựng cẩn thận các đường dẫn hoặc khớp các nút một cách tham lam theo khoảng cách. Điều đó không thành công vì khoảng cách không độc lập với mỗi cặp; việc ghép nối một nút sẽ thay đổi tính khả dụng của các nút khác. 

Hãy xem xét một đường dẫn đơn giản gồm ba nút:```
1 - 2 - 3
```Các cặp khoảng cách chẵn hợp lệ là (1,3). Nút 2 không thể ghép nối với một trong hai điểm cuối, vì vậy câu trả lời đúng là KHÔNG. Một nỗ lực tham lam ngây thơ có thể ghép (1,2) hoặc (2,3), nhưng những cặp đó không hợp lệ vì khoảng cách của chúng là 1. 

Một ví dụ khác:```
1 - 2 - 3 - 4
```Ở đây (1,3) và (2,4) là hợp lệ, do đó tồn tại một cặp đầy đủ. Bất kỳ phương pháp nào không thừa nhận cấu trúc chẵn lẻ toàn cầu sẽ gặp khó khăn trong việc phân biệt các trường hợp này một cách nhất quán. 

## Phương pháp tiếp cận 

Quan sát quan trọng là trong một cây, tính chẵn lẻ của khoảng cách hoàn toàn được xác định bằng cách tô màu hai bên. Nếu chúng ta root cây ở bất kỳ đâu, mọi nút đều có độ sâu và tính chẵn lẻ của khoảng cách giữa hai nút bằng XOR của độ tương đương độ sâu của chúng. Điều này có nghĩa là hai nút có khoảng cách chẵn khi và chỉ khi chúng nằm ở cùng một phía của phần chia đôi. 

Vì vậy, vấn đề không còn là về hình học của các đường dẫn mà trở thành việc nhóm các nút theo độ sâu chẵn lẻ. Mỗi cặp hợp lệ phải được hình thành bên trong một trong hai nhóm này. 

Một cách tiếp cận bạo lực sẽ tính toán các đường đi ngắn nhất cho tất cả các cặp hoặc kiểm tra mọi cặp nút có thể có, xác minh các ràng buộc và cố gắng xây dựng một kết hợp hoàn hảo. Ngay cả khi chúng tôi giả định một quy trình khớp thông minh, cấu trúc vẫn quá mức cần thiết vì hạn chế duy nhất là “cùng một nhóm chẵn lẻ”. Nút thắt không phải là tính đúng đắn mà là sự bùng nổ tổ hợp: các quyết định ghép đôi tăng lên theo giai đoạn. 

Một khi chúng ta quy vấn đề về việc tô màu hai bên, câu hỏi còn lại là liệu mỗi lớp màu có thể được phân chia hoàn toàn thành từng cặp hay không. Một tập hợp có thể được phân chia thành các cặp rời nhau khi và chỉ khi kích thước của nó là số chẵn. Vì các cặp không bao giờ giao nhau giữa các lớp màu nên mỗi bên phải có số lượng chẵn một cách độc lập. 

Điều này làm giảm toàn bộ vấn đề về cây thành một màu DFS duy nhất và hai kiểm tra tính chẵn lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Tìm kiếm ghép đôi vũ phu | hàm mũ | O(N) | Quá chậm | 
| Màu lưỡng cực BFS/DFS + kiểm tra tính chẵn lẻ | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Gốc cây tại bất kỳ nút nào, để thuận tiện cho nút 1. Chạy DFS hoặc BFS để tính toán độ sâu chẵn lẻ cho mỗi nút. Điều này gán mỗi nút vào một trong hai nhóm, tùy thuộc vào khoảng cách của nó với gốc là chẵn hay lẻ. 

2. Duy trì hai bộ đếm, một bộ đếm cho mỗi nhóm chẵn lẻ. 

3. Trong khi duyệt cây, tăng bộ đếm tương ứng với tính chẵn lẻ của mỗi nút. 

4. Sau khi quá trình truyền tải kết thúc, hãy kiểm tra xem cả hai bộ đếm có chẵn không. Nếu một trong hai bộ đếm là số lẻ, hãy trả về NO ngay lập tức vì nhóm đó không thể chia hoàn toàn thành từng cặp. 

5. Nếu cả hai bộ đếm đều chẵn, trả về CÓ vì chúng ta có thể ghép các nút tùy ý trong mỗi nhóm. 

Bước ẩn quan trọng là nhận ra rằng một khi các nút được phân chia theo tính chẵn lẻ thì sẽ không còn ràng buộc về cấu trúc nào nữa. Bất kỳ hai nút nào trong cùng một lớp chẵn lẻ đều có khoảng cách bằng nhau trong cây, vì vậy mọi cặp trong một lớp đều hợp lệ. 

### Tại sao nó hoạt động 

Một cây có tính chất lưỡng cực, vì vậy mỗi cạnh kết nối các nút chẵn lẻ đối diện trong một quá trình truyền tải có gốc. Điều này ngụ ý rằng tính chẵn lẻ của độ dài đường dẫn giữa hai nút chỉ phụ thuộc vào việc độ sâu của chúng có chia sẻ cùng tính chẵn lẻ hay không. Do đó, mối quan hệ ghép đôi được phép trở thành “cùng màu trong màu lưỡng cực”. 

Bên trong mỗi lớp màu, biểu đồ hạn chế cảm ứng đã hoàn tất: mọi cặp đều hợp lệ. Do đó, điều kiện duy nhất để ghép đôi hoàn hảo là mỗi lớp có số nút chẵn, vì việc ghép đôi sẽ giảm số lượng đi hai mỗi lần và không được phép ghép nối giữa các lớp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    g = [[] for _ in range(n + 1)]
    
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)
    
    color = [-1] * (n + 1)
    stack = [(1, 0)]
    color[1] = 0
    
    cnt = [0, 0]
    
    while stack:
        u, c = stack.pop()
        color[u] = c
        cnt[c] += 1
        
        for v in g[u]:
            if color[v] == -1:
                stack.append((v, c ^ 1))
    
    if cnt[0] % 2 == 0 and cnt[1] % 2 == 0:
        print("YES")
    else:
        print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp xây dựng danh sách cây kề cận và thực hiện DFS lặp để tránh các vấn đề về độ sâu đệ quy được đưa ra\(N\)lên đến\(5 \cdot 10^5\). Việc tô màu sử dụng thao tác lật XOR đơn giản để thay thế tính chẵn lẻ giữa cha mẹ và con cái. 

các quầy`cnt[0]`Và`cnt[1]`tích lũy kích thước của mỗi bên lưỡng cực. Điều kiện cuối cùng trực tiếp thực hiện yêu cầu rằng mỗi bên phải có khả năng ghép đôi hoàn hảo trong nội bộ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 
đầu vào:```
3
2 1
3 2
```Chúng tôi root ở mức 1. 

| Nút | Phụ huynh | Chẵn lẻ (màu) | cnt[0] | cnt[1] | 
|------|--------|-------|--------|--------| 
| 1 | - | 0 | 1 | 0 | 
| 2 | 1 | 1 | 1 | 1 | 
| 3 | 2 | 0 | 2 | 1 | 

Số lượng cuối cùng là\(cnt[0]=2\),\(cnt[1]=1\). Một nhóm có kích thước lẻ nên không thể ghép đôi được. 

Điều này phù hợp với thực tế là nút 2 bị mắc kẹt vì nó nằm trong lớp chẵn lẻ đơn. 

Đầu ra:```
NO
```### Ví dụ 2 
đầu vào:```
6
4 2
6 5
3 5
5 1
4 5
```Gốc ở 1. 

| Nút | Chẵn lẻ | cnt[0] | cnt[1] | 
|------|--------|--------|--------| 
| 1 | 0 | 1 | 0 | 
| 5 | 1 | 1 | 1 | 
| 6 | 0 | 2 | 1 | 
| 3 | 0 | 3 | 1 | 
| 4 | 0 | 4 | 1 | 
| 2 | 1 | 4 | 2 | 

Số đếm cuối cùng:\(cnt[0]=4\),\(cnt[1]=2\), cả hai đều chẵn. 

Chúng ta có thể ghép cặp trong mỗi nhóm và vì mỗi cặp chẵn lẻ có khoảng cách chẵn nên tồn tại một cặp đầy đủ. 

Đầu ra:```
YES
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(N) | Mỗi nút và cạnh được xử lý một lần trong quá trình truyền tải DFS | 
| Không gian | O(N) | Danh sách kề và mảng màu lưu trữ thông tin tuyến tính | 

Độ phức tạp tuyến tính là đủ cho\(N \le 5 \cdot 10^5\)và việc sử dụng bộ nhớ dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def solve():
        n = int(input())
        g = [[] for _ in range(n + 1)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            g[u].append(v)
            g[v].append(u)

        color = [-1] * (n + 1)
        stack = [(1, 0)]
        color[1] = 0
        cnt = [0, 0]

        while stack:
            u, c = stack.pop()
            color[u] = c
            cnt[c] += 1
            for v in g[u]:
                if color[v] == -1:
                    stack.append((v, c ^ 1))

        return "YES" if cnt[0] % 2 == 0 and cnt[1] % 2 == 0 else "NO"

    return solve()

# provided samples
assert run("3\n2 1\n3 2\n") == "NO"
assert run("6\n4 2\n6 5\n3 5\n5 1\n4 5\n") == "YES"

# single node
assert run("1\n") == "NO"

# simple even path
assert run("4\n1 2\n2 3\n3 4\n") == "YES"

# star tree (impossible)
assert run("5\n1 2\n1 3\n1 4\n1 5\n") == "NO"

# balanced binary-like
assert run("7\n1 2\n1 3\n2 4\n2 5\n3 6\n3 7\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| 1 nút | KHÔNG | trường hợp cạnh tối thiểu | 
| đường dẫn 4 dòng | CÓ | ghép nối chuỗi chẵn | 
| cây sao | KHÔNG | mất cân bằng chẵn lẻ bị lệch | 
| cây nhị phân đầy đủ | KHÔNG | sự không khớp chẵn lẻ không tầm thường | 

## Vỏ cạnh 

cho\(N=1\), DFS chỉ định một nút duy nhất cho một lớp chẵn lẻ có kích thước 1. Vì việc ghép nối yêu cầu các nhóm có kích thước chẵn nên thuật toán trả về NO ngay lập tức. 

Đối với đường dẫn gồm bốn nút\(1-2-3-4\), việc tô màu tạo ra hai nút trong mỗi lớp chẵn lẻ. Cả hai số đếm đều chẵn và thuật toán xuất ra CÓ. Điều này khớp với cặp hợp lệ (1,3) và (2,4), xác nhận rằng không tồn tại ràng buộc cấu trúc nào ngoài tính chẵn lẻ. 

Đối với cây hình ngôi sao, tâm có một chẵn lẻ và tất cả các lá đều có một chẵn lẻ đối diện. Nếu số lượng lá là số lẻ, một lớp sẽ có kích thước lẻ và thuật toán sẽ loại bỏ trường hợp đó một cách chính xác, vì một lá sẽ vẫn không thể so sánh được bất kể chiến lược ghép nối.
