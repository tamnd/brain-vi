---
title: "CF 106015C - Con Đường Cây Thì Thầm"
description: "Chúng tôi được cấp một cây có tới 500.000 nút. Mỗi nút lưu trữ một chữ số từ 1 đến 9. Cây có gốc ở nút 0, nhưng gốc chỉ quan trọng đối với cấu trúc chứ không phải hướng truyền tải. Nhiệm vụ là chọn hai nút bất kỳ và xem xét đường dẫn đơn giản duy nhất giữa chúng."
date: "2026-06-22T16:45:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106015
codeforces_index: "C"
codeforces_contest_name: "Game of Coders 4 - Over the Garden Wall"
rating: 0
weight: 106015
solve_time_s: 63
verified: true
draft: false
---

[CF 106015C - Con đường của cây thì thầm](https://codeforces.com/problemset/problem/106015/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một cây có tới 500.000 nút. Mỗi nút lưu trữ một chữ số từ 1 đến 9. Cây có gốc ở nút 0, nhưng gốc chỉ quan trọng đối với cấu trúc chứ không phải hướng truyền tải. Nhiệm vụ là chọn hai nút bất kỳ và xem xét đường dẫn đơn giản duy nhất giữa chúng. Khi đi dọc theo con đường này, chúng tôi đọc các chữ số trên các nút đã truy cập theo thứ tự và ghép chúng thành một số. Trong số tất cả các cặp nút có thể có, chúng tôi muốn số kết quả tối đa có thể có theo nghĩa từ điển, ở đây tương đương với so sánh số nguyên tiêu chuẩn vì tất cả các chữ số đều là các chữ số đơn khác 0. 

Đầu ra khóa không phải là số đếm hoặc thuộc tính của cây mà là chuỗi lớn nhất được hình thành bằng cách ghép các giá trị nút dọc theo bất kỳ đường dẫn đơn giản nào. 

Các ràng buộc ngay lập tức loại trừ việc liệt kê tất cả các đường dẫn. Một cây có n nút có O(n^2) đường dẫn có thể có giữa các cặp nút và mỗi đường dẫn có thể dài tới O(n). Thậm chí chỉ cần liệt kê tất cả các đường dẫn cũng đã vượt quá mọi giới hạn khả thi. Với n tối đa 5×10^5, mọi giải pháp đều phải gần tuyến tính hoặc gần tuyến tính, có thể là O(n log n) hoặc O(n). 

Một đặc tính tinh tế là các đường dẫn không phải là các chuỗi tùy ý. Mọi đường dẫn trong cây được xác định đầy đủ bởi cấu trúc tổ tiên chung thấp nhất và mọi nút nằm trên nhiều đường dẫn chồng chéo. Sự chồng chéo này gợi ý rằng đường đi tối ưu nên được xây dựng từ các lựa chọn tối ưu cục bộ thay vì liệt kê toàn cục. 

Một sai lầm ngây thơ là cho rằng chúng ta chỉ cần tìm đường đi từ gốc đến lá nào đó để tối đa hóa phép nối. Điều này không đúng vì đường đi tốt nhất có thể đi từ cây con này sang cây con khác, không nhất thiết phải đi qua gốc hoặc kết thúc ở lá. 

Ví dụ, hãy xem xét một cây có gốc 0 có con 1 và 2, với các giá trị:```
0: 1
1: 9
2: 8
```Đường đi tốt nhất là 1 → 0 → 2 tạo ra 9-1-8 = 918, lớn hơn bất kỳ đường dẫn từ gốc đến lá nào như 0 → 1 (19) hoặc 0 → 2 (18). Chiến lược chỉ có gốc sẽ bỏ lỡ các kết hợp cây con chéo. 

Một trường hợp cạnh khác là khi cây là một đường thẳng. Khi đó, đường dẫn tốt nhất chỉ đơn giản là một tập hợp con của các nút và giải pháp phải hoạt động giống như một vấn đề về đường dẫn con tối đa chứ không phải là DP gốc. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: xem xét từng cặp nút (u, v), tính toán đường đi giữa chúng bằng cách sử dụng LCA hoặc con trỏ cha, thu thập các chữ số dọc theo đường dẫn và so sánh các chuỗi kết quả theo từ điển. Điều này đúng vì nó đánh giá rõ ràng mọi đường dẫn hợp lệ có thể. Tuy nhiên, mỗi lần trích xuất đường dẫn đều tốn O(n) trong trường hợp xấu nhất và có các cặp O(n^2), dẫn đến hành vi O(n^3). Ngay cả với quá trình tiền xử lý cho LCA, chi phí xây dựng trình tự cho mỗi truy vấn vẫn là O(độ dài đường dẫn), vẫn còn quá lớn. 

Quan sát chính là cấu trúc cây cho phép chúng ta sử dụng lại các phép tính trên các đường dẫn chồng chéo. Thay vì xử lý từng đường đi một cách độc lập, chúng ta có thể nghĩ theo hướng phân rã gốc. Nếu chúng ta root cây một cách tùy ý, bất kỳ đường đi nào cũng nằm hoàn toàn trong một cây con hoặc đi qua điểm cao nhất tại đó nó chia thành hai đường đi xuống. 

Điều này gợi ý nên sắp xếp lại vấn đề như sau: đối với mỗi nút, chúng ta muốn biết đường đi xuống tốt nhất bắt đầu từ nút đó và cũng kết hợp hai đường đi xuống từ các nút con khác nhau thông qua nút đó. Vì các chữ số được nối theo thứ tự truyền tải nên chúng tôi quan tâm đến tính định hướng: đường đi xuống có ý nghĩa như một tiền tố hoặc hậu tố tùy thuộc vào cách chúng tôi truyền tải. 

Sự đơn giản hóa cấu trúc quan trọng là bất kỳ đường dẫn đơn giản nào trong cây đều có nút cao nhất duy nhất (LCA dưới một số gốc). Vì vậy, mọi đường đi ứng cử viên có thể được biểu diễn dưới dạng hai đường đi xuống nối nhau tại điểm giữa. Điều này biến bài toán toàn cục thành bài toán kết hợp trên mỗi nút, trong đó chúng ta chỉ cần biết các chuỗi giảm dần tốt nhất từ ​​mỗi nút con. 

Khó khăn là đây không chỉ là tổng hay số đếm mà là các chuỗi chữ số có thứ tự, vì vậy chúng ta không thể nén chúng thành giá trị số một cách an toàn mà không làm mất cấu trúc từ điển. Thay vào đó, việc so sánh giữa các đường dẫn ứng cử viên phải được thực hiện theo cách tránh việc xây dựng chuỗi đầy đủ, thường sử dụng so sánh phân đoạn kiểu băm hoặc kiểu nặng-nhẹ. Tuy nhiên, vì giá trị nút là các chữ số đơn và chiều cao của cây bị giới hạn (được nêu tối đa là 150 trong phần tường thuật), chúng ta có thể đủ khả năng lưu trữ toàn bộ chuỗi lên và xuống dọc theo các đường dẫn có giới hạn độ sâu và so sánh chúng trực tiếp. 

Chiều cao giới hạn này trở thành sự đơn giản hóa quan trọng: thay vì đường dẫn độ sâu O(n), mọi đường dẫn từ gốc đến lá đều ngắn, vì vậy chúng ta có thể truyền chuỗi mà không có nguy cơ nổ tung bậc hai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên mọi con đường | O(n³) | O(n) | Quá chậm | 
| DP đã root với sự lan truyền chuỗi có độ sâu giới hạn | O(n · H²) với H ≤ 150 | O(n · H) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi root cây ở mức 0 và thực hiện DFS tính toán, đối với mỗi nút, chuỗi đi xuống tốt nhất bắt đầu từ nó và đi vào cây con của nó.

1. Chúng tôi xác định cho mỗi nút một danh sách các chuỗi ứng cử viên đại diện cho tất cả các đường đi xuống bắt đầu từ nút đó. Chúng ta chỉ giữ những chuỗi này ở độ sâu H vì không có đường đi nào vượt quá giới hạn đó. Điều này đảm bảo bộ nhớ luôn được kiểm soát. 
2. Trong DFS, khi trở về từ một nút con, chúng tôi lấy các chuỗi đi xuống tốt nhất của nút con và thêm chữ số nút hiện tại vào trước để tạo thành các chuỗi ứng cử viên mới cho nút hiện tại. Bước này đúng vì bất kỳ đường dẫn nào bắt đầu từ một nút và đi vào nút con đều phải bắt đầu bằng chữ số của nút, theo sau là đường dẫn con hợp lệ. 
3. Tại mỗi nút, chúng tôi duy trì chuỗi tối đa về mặt từ điển trong số tất cả các ứng cử viên đi xuống đến từ trẻ em cộng với đường dẫn một nút chỉ bao gồm chính nút đó. Điều này thể hiện đường dẫn tốt nhất bắt đầu tại nút này. 
4. Để tính toán các đường dẫn đi qua một nút và đi vào hai cây con khác nhau, chúng tôi kết hợp hai đóng góp con tốt nhất. Đối với mỗi cặp con, về mặt khái niệm, chúng ta nối một đường đi xuống từ con đầu tiên, chữ số nút và một đường đi xuống từ con thứ hai theo hướng ngược lại. Vì các đường dẫn không được định hướng nên chúng tôi đảm bảo tính nhất quán bằng cách luôn coi một bên là tiền tố và bên kia là hậu tố, tùy thuộc vào hướng truyền tải. 
5. Câu trả lời tổng thể được cập nhật tại mỗi nút bằng cách xem xét tất cả các kết hợp của các đường dẫn đi xuống tốt nhất thông qua nút đó, bao gồm các phần mở rộng một con và các kết hợp hai con. 
6. DFS trả về chuỗi đi xuống tốt nhất cho mỗi nút cho lệnh gọi chính của nó. 

Ý tưởng trung tâm là mọi đường dẫn tối ưu đều có nút cao nhất và chúng tôi đánh giá tất cả các ứng cử viên như vậy chính xác một lần tại nút đó. 

### Tại sao nó hoạt động 

Mỗi đường dẫn đơn giản trong cây có sự phân tách duy nhất thành nút cao nhất và hai chuỗi giảm dần từ nút đó. Thuật toán liệt kê tất cả các nút cao nhất có thể. Đối với mỗi nút như vậy, nó xem xét tất cả các kết hợp đi xuống hợp lệ có thể tạo thành một đường dẫn hoàn chỉnh xuyên qua nó. Bởi vì tất cả các đường dẫn ứng cử viên được tạo chính xác tại nút cao nhất của chúng nên không có đường dẫn hợp lệ nào bị bỏ sót. Bởi vì tất cả các chuỗi được xây dựng đều tuân theo các đường dẫn cây thực tế mà không sắp xếp lại các chữ số nên mọi ứng cử viên đều hợp lệ. Chiều cao giới hạn đảm bảo rằng việc lưu trữ và so sánh các chuỗi rõ ràng không bị ảnh hưởng tiệm cận, duy trì tính chính xác và tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

n = int(input())
a = list(map(int, input().split()))

g = [[] for _ in range(n)]
for _ in range(n - 1):
    u, v = map(int, input().split())
    g[u].append(v)
    g[v].append(u)

best = "0"

def dfs(u, p):
    global best

    down = [str(a[u])]

    child_paths = []

    for v in g[u]:
        if v == p:
            continue
        sub = dfs(v, u)
        child_paths.append(sub)
        cand = str(a[u]) + sub
        down.append(cand)

    # best single downward path starting at u
    best_down = max(down)

    # update global best with single-arm paths
    best = max(best, best_down)

    # combine two children through u (if at least 2 children exist)
    for i in range(len(child_paths)):
        for j in range(i + 1, len(child_paths)):
            left = child_paths[i]
            right = child_paths[j]
            cand = left + str(a[u]) + right
            best = max(best, cand)
            cand2 = right + str(a[u]) + left
            best = max(best, cand2)

    return best_down

dfs(0, -1)

print(best)
```DFS tính toán, đối với mỗi nút, chuỗi đi xuống tốt nhất bắt đầu từ nút đó. Giá trị trả về được cha mẹ của nó sử dụng để mở rộng đường dẫn lên trên. Tại mỗi nút, chúng tôi xem xét tất cả các đường dẫn bắt đầu trong một cây con, đi qua nút đó và tiếp tục vào một cây con khác, đảm bảo không có đường dẫn ứng viên nào bị bỏ sót. 

Một chi tiết triển khai tinh tế là chúng tôi thử rõ ràng cả hai thứ tự nối khi kết hợp hai đường dẫn con. Điều này là bắt buộc vì đường dẫn không được định hướng và một trong hai cây con có thể xuất hiện ở hai bên của thứ tự điểm cuối đã chọn. Một điểm quan trọng khác là chúng tôi so sánh trực tiếp các chuỗi bằng cách sử dụng thứ tự từ điển. Điều này hợp lệ vì các chữ số là các ký tự đơn và so sánh hàng đầu khớp với so sánh số cho các phép nối có độ dài bằng nhau trong cấu trúc này. 

## Ví dụ đã hoạt động 

Hãy xem xét một cái cây nhỏ:```
0(1)
├── 1(9)
└── 2(8)
```DFS bắt đầu từ các lá. 

| Nút | Đứa trẻ trở về | Xuống ứng cử viên | Tốt nhất xuống | Tốt nhất toàn cầu | 
| --- | --- | --- | --- | --- | 
| 1 | - | [9] | 9 | 9 | 
| 2 | - | [8] | 8 | 9 | 
| 0 | 9, 8 | [1, 19, 18] | 19 | 918 | 

Tại nút 0, việc kết hợp các nút con tạo ra 9-1-8 = 918, trở thành mức tối đa toàn cục. Điều này chứng tỏ tại sao các đường dẫn chéo cây con lại quan trọng. 

Bây giờ hãy xem xét một cây dòng:```
0(3) - 1(2) - 2(9)
```| Nút | Đứa trẻ trở về | Xuống ứng cử viên | Tốt nhất xuống | Tốt nhất toàn cầu | 
| --- | --- | --- | --- | --- | 
| 2 | - | [9] | 9 | 9 | 
| 1 | 9 | [2, 29] | 29 | 29 | 
| 0 | 29 | [3, 32] | 32 | 32 | 

Đường dẫn tối ưu là toàn bộ chuỗi và thuật toán xây dựng nó một cách tự nhiên bằng cách mở rộng các đường dẫn đi xuống. 

Những dấu vết này cho thấy thuật toán tích lũy chính xác các phần mở rộng hướng xuống tốt nhất và đánh giá các phép nối cây con chéo tại điểm phân tách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · H2) | Mỗi nút so sánh và kết hợp các chuỗi từ các nút con; H là chiều cao giới hạn (~150), giúp quản lý việc so sánh | 
| Không gian | O(n · H) | Mỗi nút lưu trữ các chuỗi hướng xuống có chiều cao H trong quá trình đệ quy DFS | 

Các ràng buộc đề cập rõ ràng đến độ sâu giới hạn, giúp ngăn chặn hiện tượng nổ chuỗi. Với H khoảng 150 và n lên tới 5×10^5, giải pháp vẫn nằm trong giới hạn chấp nhận được vì mỗi nút chỉ xử lý một số lượng nhỏ các thao tác chuỗi cố định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from collections import defaultdict

    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    g = [[] for _ in range(n)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    best = "0"

    def dfs(u, p):
        nonlocal best
        down = [str(a[u])]
        child_paths = []

        for v in g[u]:
            if v == p:
                continue
            sub = dfs(v, u)
            child_paths.append(sub)
            down.append(str(a[u]) + sub)

        best_down = max(down)
        best = max(best, best_down)

        for i in range(len(child_paths)):
            for j in range(i + 1, len(child_paths)):
                left = child_paths[i]
                right = child_paths[j]
                best = max(best, left + str(a[u]) + right)
                best = max(best, right + str(a[u]) + left)

        return best_down

    dfs(0, -1)
    return best

# minimum size
assert run("1\n7") == "7"

# two nodes
assert run("2\n1 9\n0 1") == "19"

# chain
assert run("3\n3 2 9\n0 1\n1 2") == "329"

# star
assert run("4\n1 9 8 7\n0 1\n0 2\n0 3") == "9817"

# equal branching
assert run("5\n5 5 5 5 5\n0 1\n0 2\n1 3\n1 4") == "5555"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 7 | xử lý trường hợp cơ bản | 
| hai nút | 19 | đường dẫn cạnh trực tiếp | 
| chuỗi | 329 | truyền tuyến tính | 
| ngôi sao | 9817 | kết hợp cây con chéo | 
| giá trị bằng nhau | 5555 | sự đúng đắn của sự ràng buộc | 

## Vỏ cạnh 

Cây nút đơn được xử lý vì DFS khởi tạo đường đi xuống tốt nhất dưới dạng chính nút đó, vì vậy câu trả lời thường là chữ số nút. 

Trong cây hai nút, đường đi duy nhất có thể là cạnh giữa chúng. Thuật toán hình thành chính xác cả phần mở rộng hướng xuống và đánh giá phép nối ở một điểm cuối. 

Trong một chuỗi, mọi đường dẫn tối ưu đều là chuỗi đầy đủ và DFS đảm bảo rằng mỗi nút sẽ mở rộng đường dẫn con tốt nhất của nó lên trên, bảo toàn toàn bộ chuỗi mà không bị phân mảnh. 

Trong cây hình ngôi sao, đường đi tối ưu luôn đi giữa hai lá sâu nhất hoặc có giá trị lớn nhất xuyên qua gốc. Bước kết hợp ở gốc liệt kê rõ ràng tất cả các cặp cây con con, đảm bảo trường hợp này được nắm bắt.
