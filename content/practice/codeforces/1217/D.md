---
title: "CF 1217D - Viền màu"
description: "Chúng ta được cung cấp một đồ thị có hướng trong đó mỗi cạnh được cố định trước và chúng ta phải gán một màu (nhãn số nguyên) cho mỗi cạnh. Ràng buộc không phải về các đỉnh mà là về các chu trình có hướng: nếu bạn nhìn vào tất cả các cạnh của một màu, chúng không được chứa bất kỳ chu trình có hướng nào."
date: "2026-06-15T18:50:02+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1217
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 72 (Rated for Div. 2)"
rating: 2100
weight: 1217
solve_time_s: 161
verified: true
draft: false
---

[CF 1217D - Tô màu các cạnh](https://codeforces.com/problemset/problem/1217/D) 

**Xếp hạng:** 2100 
**Tags:** thuật toán xây dựng, dfs và tương tự, đồ thị 
**Thời gian giải:** 2m 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị có hướng trong đó mỗi cạnh được cố định trước và chúng ta phải gán một màu (nhãn số nguyên) cho mỗi cạnh. Ràng buộc không phải về các đỉnh mà là về các chu trình có hướng: nếu bạn nhìn vào tất cả các cạnh của một màu, chúng không được chứa bất kỳ chu trình có hướng nào. 

Nhiệm vụ là giảm thiểu số lượng màu được sử dụng trong khi vẫn đảm bảo rằng mọi đồ thị con đơn sắc đều không theo chu kỳ. 

Một cách hữu ích để diễn giải lại yêu cầu là mỗi lớp màu phải tạo thành một Đồ thị tuần hoàn có hướng. Chúng tôi đang phân tách tập cạnh thành số lượng tối thiểu các đồ thị con có hướng không theo chu kỳ một cách hiệu quả. 

Các ràng buộc đủ nhỏ để cấu trúc O(nm) hoặc O(m log m) là ổn, vì cả n và m đều lớn nhất là 5000. Điều này cho thấy rằng chúng ta có thể duyệt đồ thị, DFS lặp lại hoặc xây dựng tham lam trên các cạnh mà không phải lo lắng về độ chặt tiệm cận ngoài bậc hai. 

Một ý tưởng ngây thơ là gán màu tùy ý hoặc bằng cách nhóm các cạnh, nhưng các chu kỳ ngay lập tức phá vỡ những nỗ lực đó. Ví dụ: trong tam giác có hướng 1 → 2 → 3 → 1, nếu tất cả các cạnh có cùng màu thì chúng ta đã vi phạm điều kiện. Điều này cho thấy rằng ngay cả các thành phần nhỏ được kết nối mạnh mẽ cũng buộc phải có nhiều màu sắc. 

Một trường hợp lỗi tinh vi xuất hiện khi các cạnh được xử lý mà không tôn trọng sự phụ thuộc. Ví dụ: trong biểu đồ trong đó các cạnh được sắp xếp theo thứ tự tùy ý, việc gán màu một cách tham lam mà không theo dõi khả năng tiếp cận có thể tạo ra tình huống trong đó cạnh sau trong DFS chia sẻ màu với các cạnh phía trước, tạo ra một chu kỳ đơn sắc. Một cách tiếp cận đúng phải đảm bảo rằng bất kỳ cạnh nào tham gia vào cấu trúc cạnh sau DFS đều được phân tách bằng màu sắc với các cạnh của cây DFS đứng trước nó. 

Thông tin chi tiết về cấu trúc quan trọng là câu trả lời được kết nối chặt chẽ với độ sâu của các cạnh sau trong cây DFS. 

## Phương pháp tiếp cận 

Hướng mạnh mẽ sẽ là thử tất cả các phép gán màu có thể có cho các cạnh, tăng k từ 1 trở lên và kiểm tra xem mỗi màu có hợp lệ hay không. Kiểm tra tính hợp lệ yêu cầu xác minh rằng mỗi lớp màu là không theo chu kỳ, bản thân lớp này yêu cầu phát hiện chu kỳ cho mỗi màu, thường là O(n + m). Ngay cả khi chúng ta giới hạn k ở m, điều này sẽ trở thành tổ hợp và phát triển dưới dạng k^m theo cách hiểu tồi tệ nhất, điều này không khả thi. 

Chúng ta có thể đơn giản hóa không gian tìm kiếm bằng cách lưu ý rằng chúng ta không thực sự cần phải thử các bài tập. Thay vào đó, chúng ta có thể tạo màu một cách trực tiếp. 

Quan sát trung tâm là các chu trình trong đồ thị có hướng được biểu thị bằng các cạnh sau của DFS. Trong DFS, khi chúng ta duyệt một cạnh tới nút hiện đang hoạt động, cạnh đó sẽ đóng một chu trình. Nếu chúng tôi đảm bảo rằng các cạnh như vậy nhận được màu lớn hơn nghiêm ngặt so với độ sâu của cây DFS nơi chúng xuất hiện, thì chúng tôi sẽ ngăn chúng chia sẻ màu với tất cả các cạnh tham gia vào cùng cấu trúc ngăn xếp DFS. 

Điều này dẫn đến một ý tưởng mang tính xây dựng: duy trì trạng thái truy cập DFS và gán màu dựa trên việc một cạnh tiến lên trong cây DFS hay lùi về nút hoạt động. Các cạnh chuyển tiếp có thể chia sẻ màu 1 một cách an toàn vì chúng không thể tạo thành một chu trình có hướng với nhau nếu chúng tôn trọng thứ tự DFS. Các cạnh sau phải được gán một màu khác, vì mỗi cạnh sau biểu thị sự vi phạm phụ thuộc liên quan đến thứ tự DFS. 

Một sàng lọc cẩn thận cho thấy rằng chúng ta chỉ cần hai màu. Nếu đồ thị không có chu trình thì chỉ cần một màu là đủ. Nếu có ít nhất một cạnh sau, hai màu là đủ để tách cây/cạnh tiến khỏi cạnh sau theo cách ngăn chặn chu kỳ đơn sắc. 

Vấn đề giảm xuống còn việc phát hiện xem biểu đồ có chứa chu trình được định hướng có thể truy cập được trong DFS hay không và ghi nhãn các cạnh tương ứng.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n + m) | Quá chậm | 
| Màu sắc mang tính xây dựng dựa trên DFS | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi thực hiện DFS trên biểu đồ có hướng trong khi theo dõi trạng thái nút. 

1. Xây dựng danh sách kề cho biểu đồ, đồng thời lưu trữ chỉ số của từng cạnh để chúng ta có thể gán màu theo thứ tự đầu vào. Điều này là cần thiết vì đầu ra được gắn với thứ tự cạnh ban đầu. 
2. Duy trì mảng trạng thái truy cập cho các nút có ba trạng thái: chưa được truy cập, đang hoạt động trong ngăn xếp đệ quy và đã được xử lý đầy đủ. Điều này cho phép chúng tôi phát hiện các cạnh phía sau một cách chính xác. 
3. Bắt đầu DFS từ mọi nút chưa được truy cập. Biểu đồ có thể bị ngắt kết nối nên chúng ta phải bao gồm tất cả các thành phần. 
4. Khi khám phá một cạnh u → v, chúng tôi phân loại nó dựa trên trạng thái của v. Nếu v không được thăm, chúng tôi coi nó như một cạnh cây DFS và tiếp tục DFS. 
5. Nếu v hiện đang hoạt động, cạnh u → v là cạnh sau, nghĩa là nó đóng một chu trình có hướng. Chúng tôi gán màu cạnh này 2. 
6. Nếu v đã được xử lý đầy đủ, cạnh đó là cạnh tiến hoặc cạnh chéo. Nó không thể tạo chu trình trong ngữ cảnh ngăn xếp DFS, vì vậy nó được gán màu 1. 
7. Việc hoàn thành DFS đánh dấu một nút là đã được xử lý đầy đủ. 

Câu trả lời cuối cùng sử dụng k = 2 nếu gặp bất kỳ cạnh sau nào, nếu không thì k = 1. 

### Tại sao nó hoạt động 

Thứ tự DFS tạo ra thứ tự một phần trên các cạnh không phải là cạnh sau. Bất kỳ cạnh nào không phải là cạnh sau sẽ tôn trọng thứ tự này và không thể tham gia vào chu trình được định hướng trong cùng một lớp màu. Các cạnh sau chính xác là những cạnh vi phạm trật tự này bằng cách trỏ vào ngăn xếp đệ quy đang hoạt động, điều này đảm bảo chúng chịu trách nhiệm đóng các chu trình. Bằng cách tách chúng thành một màu riêng biệt, chúng tôi phá vỡ mọi chu trình đơn sắc tiềm năng, vì mỗi chu trình có hướng phải chứa ít nhất một cạnh sau trong bất kỳ quá trình truyền tải DFS nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

n, m = map(int, input().split())

g = [[] for _ in range(n)]
edges = []

for i in range(m):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    g[u].append((v, i))
    edges.append((u, v))

color = [1] * m
state = [0] * n  # 0 = unvisited, 1 = active, 2 = done

has_cycle = False

def dfs(u):
    global has_cycle
    state[u] = 1
    for v, idx in g[u]:
        if state[v] == 0:
            dfs(v)
        elif state[v] == 1:
            color[idx] = 2
            has_cycle = True
        else:
            color[idx] = 1
    state[u] = 2

for i in range(n):
    if state[i] == 0:
        dfs(i)

k = 2 if has_cycle else 1
print(k)
print(*color)
```Việc triển khai trực tiếp mã hóa phân loại DFS. Chi tiết quan trọng là màu sắc của cạnh được quyết định tại thời điểm chúng tôi kiểm tra một cạnh, dựa trên việc đích đến của nó nằm trong ngăn xếp đệ quy hay đã được xử lý. Việc phát hiện ngăn xếp đệ quy được xử lý bởi mảng trạng thái, trong đó giá trị 1 có nghĩa là hoạt động. 

K cuối cùng phụ thuộc vào việc chúng ta có từng gặp phải cạnh sau hay không. Nếu không tồn tại, biểu đồ đã là DAG và tất cả các cạnh có thể chia sẻ màu 1 một cách an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 5
1 2
1 3
3 4
2 4
1 4
```Tất cả các cạnh tạo thành một DAG, do đó không có cạnh sau nào xuất hiện. 

| Bước | Cạnh | Trạng thái của v | Loại | Màu sắc | 
| --- | --- | --- | --- | --- | 
| 1 | 1→2 | chưa được thăm | cây | 1 | 
| 2 | 1→3 | chưa được thăm | cây | 1 | 
| 3 | 3→4 | chưa được thăm | cây | 1 | 
| 4 | 2→4 | xong | chuyển tiếp | 1 | 
| 5 | 1→4 | xong | chuyển tiếp | 1 | 

Không có chu kỳ nào được phát hiện nên k = 1. 

Dấu vết này xác nhận rằng trong DAG, tất cả các cạnh vẫn an toàn trong một lớp màu duy nhất. 

### Ví dụ 2 

Hãy xem xét một biểu đồ:```
3 3
1 2
2 3
3 1
```Đây là một chu kỳ có định hướng. 

| Bước | Cạnh | Trạng thái của v | Loại | Màu sắc | 
| --- | --- | --- | --- | --- | 
| 1 | 1→2 | chưa được thăm | cây | 1 | 
| 2 | 2→3 | chưa được thăm | cây | 1 | 
| 3 | 3→1 | hoạt động | trở lại | 2 | 

Ở đây cạnh cuối cùng sẽ đóng chu trình và phải được tách ra. 

Điều này xác nhận rằng mọi chu kỳ có hướng buộc ít nhất một cạnh phải có màu thứ hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi nút và cạnh được xử lý một lần trong quá trình truyền tải DFS | 
| Không gian | O(n + m) | Danh sách kề cộng với ngăn xếp đệ quy và mảng trạng thái | 

Kích thước đồ thị đủ nhỏ để việc truyền tải tuyến tính dễ dàng nằm trong giới hạn. Ngay cả trong trường hợp xấu nhất là các mẫu cạnh dày đặc lên tới 5000 cạnh, DFS vẫn chạy thoải mái theo thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    g = [[] for _ in range(n)]
    color = [1] * m
    state = [0] * n

    edges = []
    for i in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append((v, i))
        edges.append((u, v))

    sys.setrecursionlimit(10**7)
    has_cycle = False

    def dfs(u):
        nonlocal has_cycle
        state[u] = 1
        for v, idx in g[u]:
            if state[v] == 0:
                dfs(v)
            elif state[v] == 1:
                color[idx] = 2
                has_cycle = True
            else:
                color[idx] = 1
        state[u] = 2

    for i in range(n):
        if state[i] == 0:
            dfs(i)

    k = 2 if has_cycle else 1
    return str(k) + "\n" + " ".join(map(str, color))

# provided sample
assert run("""4 5
1 2
1 3
3 4
2 4
1 4
""") == "1\n1 1 1 1 1"

# simple cycle
assert run("""3 3
1 2
2 3
3 1
""") == "2\n1 1 2"

# chain (DAG)
assert run("""5 4
1 2
2 3
3 4
4 5
""") == "1\n1 1 1 1"

# mixed graph
assert run("""4 4
1 2
2 3
3 1
3 4
""") in ["2\n1 1 2 1", "2\n1 1 2 1"]

# single cycle edge case structure
assert run("""3 3
1 3
3 2
2 1
""")[0] == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi DAG | 1 màu | tính khả thi một màu | 
| 3 chu kỳ | 2 màu | phát hiện chu kỳ | 
| đồ thị hỗn hợp | 2 màu | cấu trúc tuần hoàn một phần | 
| chu kỳ ngược lại | 2 màu | định hướng chu kỳ tùy ý | 

## Vỏ cạnh 

Trường hợp cạnh khóa là một biểu đồ đã là DAG. Trong những trường hợp như vậy, không có cạnh lùi ngăn xếp đệ quy nào xuất hiện, do đó thuật toán gán tất cả các cạnh có màu 1. Điều này phù hợp với câu trả lời tối ưu vì DAG không có chu kỳ theo định nghĩa. 

Một trường hợp cạnh khác là đồ thị trong đó tồn tại các chu trình nhưng được phân tách bằng cấu trúc, chẳng hạn như hai chu trình rời nhau. DFS sẽ phát hiện các cạnh sau trong cả hai thành phần, nhưng vẫn chỉ yêu cầu màu 2 trên toàn cầu. Việc phân tách có hiệu quả vì mọi cạnh sau đều được tách biệt thành lớp thứ hai bất kể nó xuất hiện ở đâu. 

Trường hợp tinh vi cuối cùng là khi các chu trình được hình thành thông qua các cạnh chéo thay vì các cạnh sau ngay lập tức theo một số thứ tự DFS. Ngay cả khi đó, bất kỳ chu trình có hướng nào cũng phải chứa ít nhất một cạnh sau trong phân loại DFS, do đó thuật toán vẫn xác định ít nhất một cạnh yêu cầu màu thứ hai, đảm bảo tính chính xác.
