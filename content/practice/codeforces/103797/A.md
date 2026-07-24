---
title: "CF 103797A - Kẻ thù cố vấn"
description: "Chúng ta được cung cấp một tập hợp các quy tắc ưu tiên giữa những người được đặt tên, trong đó mỗi quy tắc nói rằng một người phải xuất hiện trước người khác theo thứ tự hợp lệ. Mỗi tên chỉ là một chuỗi và mọi quy tắc đều được chuyển từ điều kiện tiên quyết đến điều kiện phụ thuộc."
date: "2026-07-02T08:47:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103797
codeforces_index: "A"
codeforces_contest_name: "IME++ Starters Try-outs 2022"
rating: 0
weight: 103797
solve_time_s: 50
verified: true
draft: false
---

[CF 103797A - Kẻ thù cố vấn](https://codeforces.com/problemset/problem/103797/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các quy tắc ưu tiên giữa những người được đặt tên, trong đó mỗi quy tắc nói rằng một người phải xuất hiện trước người khác theo thứ tự hợp lệ. Mỗi tên chỉ là một chuỗi và mọi quy tắc đều được chuyển từ điều kiện tiên quyết đến điều kiện phụ thuộc. 

Nhiệm vụ là xác định liệu có thể sắp xếp tất cả các tên liên quan theo thứ tự nào đó sao cho mọi quy tắc đều được tôn trọng đồng thời hay không. Nếu thứ tự như vậy tồn tại, chúng tôi sẽ đưa ra phán quyết tích cực, nếu không, chúng tôi sẽ báo cáo rằng điều đó là không thể. 

Về mặt khái niệm, mỗi tên là một nút trong đồ thị có hướng và mỗi quy tắc là một cạnh có hướng. Câu hỏi đặt ra là liệu đồ thị có hướng này có chấp nhận thứ tự tuyến tính nhất quán với tất cả các cạnh hay không. 

Các ràng buộc cho phép tối đa 100.000 quan hệ và mỗi tên có thể dài tối đa 30 ký tự. Kích thước này ngay lập tức gợi ý rằng mọi suy luận bậc hai trên tất cả các cặp nút là không thể. Ngay cả việc xây dựng một ma trận kề cận dày đặc cũng không thể thực hiện được cả về bộ nhớ và thời gian. Cần phải duyệt đồ thị tuyến tính hoặc gần tuyến tính, điều này hướng mạnh đến việc phát hiện chu kỳ đồ thị hoặc sắp xếp cấu trúc liên kết. 

Một vấn đề tế nhị quan trọng là các nút không được cung cấp dưới dạng số nguyên. Chúng ta phải ánh xạ các chuỗi tùy ý tới các ID số nguyên nhỏ gọn một cách hiệu quả. Một trường hợp khác là các ràng buộc trùng lặp hoặc dư thừa, điều này sẽ không ảnh hưởng đến tính chính xác. Cuối cùng, các vòng lặp tự là không thể thực hiện được bằng câu lệnh, nhưng phải phát hiện các chu kỳ có độ dài lớn hơn một. 

Một cách tiếp cận ngây thơ cố gắng hoán vị các nút hoặc kiểm tra tất cả các thứ tự có thể có sẽ bùng nổ về mặt tổ hợp ngay cả đối với một số lượng nhỏ các tên duy nhất, vì số lượng hoán vị tăng theo giai thừa. 

## Phương pháp tiếp cận 

Chiến lược brute-force trực tiếp sẽ cố gắng tạo ra thứ tự của tất cả các tên riêng biệt và xác minh xem tất cả các quy tắc ưu tiên có được thỏa mãn hay không. Ngay cả khi chúng tôi sửa tập hợp các nút và kiểm tra tất cả các hoán vị, độ phức tạp sẽ trở thành giai thừa trong số lượng tên duy nhất. Với tối đa 100.000 ràng buộc, số lượng tên riêng biệt vẫn có thể đủ lớn để phương pháp này trở nên hoàn toàn không khả thi. 

Một ý tưởng ngây thơ khác là liên tục loại bỏ các nút không có cạnh đầu vào, nhưng không xử lý đồ thị cẩn thận, điều này sẽ thoái hóa thành việc quét lặp đi lặp lại tất cả các cạnh để tính toán lại các bậc, dẫn đến hành vi bậc hai trong trường hợp xấu nhất. 

Cấu trúc của bài toán là kiểm tra tính nhất quán của đồ thị có hướng. Cái nhìn sâu sắc chính xác là một thứ tự hợp lệ tồn tại khi và chỉ khi đồ thị có hướng là không theo chu kỳ. Việc phát hiện xem đồ thị có hướng có chu trình hay không có thể được thực hiện một cách hiệu quả bằng cách sử dụng tìm kiếm theo chiều sâu với theo dõi trạng thái đệ quy hoặc thuật toán của Kahn để sắp xếp cấu trúc liên kết. 

Thuật toán của Kahn ở đây đặc biệt tự nhiên vì nó mô phỏng trực tiếp việc loại bỏ nhiều lần các nút không còn điều kiện tiên quyết nào. Nếu tại một thời điểm nào đó không có nút nào tồn tại nhưng vẫn còn các nút chưa được xử lý thì một chu trình phải tồn tại. 

Chúng tôi giảm bớt vấn đề trong việc xây dựng biểu đồ, tính toán mức độ và thực hiện quy trình loại bỏ giống như BFS. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Sắp xếp tôpô (Kahn) | O(M + N) | O(M + N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi chuyển đổi tất cả các tên chuỗi thành các mã định danh số nguyên để các thao tác trên biểu đồ có hiệu quả. Mỗi tên duy nhất được gán một chỉ mục khi gặp lần đầu tiên. 

Sau đó, chúng tôi xây dựng một danh sách kề cận có hướng biểu thị tất cả các ràng buộc về mức độ ưu tiên và tính toán mức độ cho mỗi nút. 

Sau đó, chúng tôi liên tục chọn các nút có mức độ bằng 0, nghĩa là chúng hiện không có điều kiện tiên quyết nào chưa được đáp ứng. Các nút này được thêm vào hàng đợi xử lý.

Chúng tôi xử lý từng nút trong hàng đợi này và đối với mỗi cạnh đi ra, chúng tôi giảm mức độ của nút đích. Nếu mức độ đó bằng 0, chúng tôi sẽ thêm nút đích vào hàng đợi. 

Cuối cùng, chúng tôi kiểm tra xem liệu chúng tôi có thể xử lý tất cả các nút hay không. Nếu có, biểu đồ có tính tuần hoàn và tồn tại một thứ tự hợp lệ. Nếu không, phải có một chu kỳ ngăn cản sự tiến bộ hơn nữa. 

Tại sao nó hoạt động dựa trên tính bất biến rằng bất kỳ nút nào có mức độ bằng 0 đều an toàn để đặt tiếp theo theo thứ tự hợp lệ, vì không có gì phụ thuộc vào việc nó bị trì hoãn. Nếu một chu trình tồn tại, mỗi nút trong chu trình luôn có ít nhất một cạnh đến từ bên trong chu trình, do đó không có nút nào trong chu trình đó có thể đạt đến mức 0. Điều này ngăn thuật toán tiêu thụ tất cả các nút. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict, deque

def solve():
    m = int(input())
    
    adj = defaultdict(list)
    indeg = defaultdict(int)
    id_map = {}
    idx = 0

    def get_id(x):
        nonlocal idx
        if x not in id_map:
            id_map[x] = idx
            idx += 1
        return id_map[x]

    for _ in range(m):
        a, b = input().split()
        u = get_id(a)
        v = get_id(b)
        adj[u].append(v)
        indeg[v] += 1
        if u not in indeg:
            indeg[u] = indeg[u]

    n = idx
    q = deque()

    for i in range(n):
        if indeg[i] == 0:
            q.append(i)

    processed = 0

    while q:
        u = q.popleft()
        processed += 1
        for v in adj[u]:
            indeg[v] -= 1
            if indeg[v] == 0:
                q.append(v)

    if processed == n:
        print("I disagree with the advisor")
    else:
        print("No more comedians++")

if __name__ == "__main__":
    solve()
```Đầu tiên, mã này xây dựng ánh xạ từ chuỗi sang số nguyên để việc lưu trữ biểu đồ trở thành dựa trên mảng thay vì dựa trên chuỗi. Điều này rất cần thiết vì các thao tác chuỗi trực tiếp bên trong việc truyền tải biểu đồ sẽ tạo ra chi phí không cần thiết. 

Danh sách kề lưu trữ tất cả các ràng buộc có hướng, trong khi từ điển mức độ theo dõi xem mỗi nút vẫn còn bao nhiêu điều kiện tiên quyết. Các nút không xuất hiện dưới dạng đích được khởi tạo ngầm với mức độ bằng 0. 

Hàng đợi được khởi tạo với tất cả các nút không có cạnh vào. Vòng lặp BFS sau đó mô phỏng lặp đi lặp lại bước tiếp theo hợp lệ theo thứ tự tôpô. Mỗi lần chúng tôi loại bỏ một nút, chúng tôi sẽ giảm mức độ của các nút lân cận, có khả năng mở khóa các nút mới. 

Sự so sánh cuối cùng giữa các nút được xử lý và các nút tổng là bước phát hiện chu trình. Nếu bất kỳ nút nào không bao giờ được xử lý thì nó phải là một phần hoặc bị chặn bởi một chu trình. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
JEFF POTATO
JEFF MARK
```Chúng tôi ánh xạ JEFF, POTATO, MARK thành id 0, 1, 2. Mức độ bắt đầu là POTATO:1, MARK:1, JEFF:0. 

| Bước | Xếp hàng | Đã xử lý | Thay đổi mức độ | 
| --- | --- | --- | --- | 
| ban đầu | [JEFF] | 0 | KHOAI TÂY=1, MARK=1 | 
| lấy JEFF | [] | 1 | KHOAI TÂY=0, MARK=0 | 
| đẩy mới bằng 0 | [KHOAI TÂY, MARK] | 1 | cả hai đều được mở khóa | 
| lấy KHOAI TÂY | [ĐÁNH DẤU] | 2 | không có tác dụng đi ra | 
| lấy MARK | [] | 3 | xong | 

Tất cả các nút đều được xử lý, vì vậy biểu đồ có tính tuần hoàn. 

Đầu ra:```
I disagree with the advisor
```Dấu vết này cho thấy cách loại bỏ các điều kiện tiên quyết sẽ dần dần mở khóa các nút phụ thuộc cho đến khi tất cả đều có thể truy cập được. 

### Ví dụ 2 

đầu vào:```
PETER PARKER
PARKER WAYNE
WAYNE PETER
```Điều này tạo thành một chu kỳ giữa ba nút. 

| Bước | Xếp hàng | Đã xử lý | Thay đổi mức độ | 
| --- | --- | --- | --- | 
| ban đầu | [] | 0 | đều có bằng cấp 1 | 
| dừng lại | [] | 0 | không có nút độ bằng 0 | 

Không nút nào có thể bắt đầu quá trình vì mọi nút đều phụ thuộc vào nút khác trong chu trình. 

Đầu ra:```
No more comedians++
```Điều này thể hiện chế độ lỗi chính: các chu kỳ ngăn chặn bất kỳ điểm vào nào có mức độ bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M + N) | Mỗi cạnh được xử lý một lần, mỗi nút được xếp vào hàng đợi một lần | 
| Không gian | O(M + N) | danh sách kề và lưu trữ mức độ cộng với ánh xạ | 

Giải pháp có quy mô tuyến tính theo số lượng quan hệ, phù hợp thoải mái trong giới hạn lên tới 100.000 cạnh. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import defaultdict, deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    def solve():
        m = int(input())
        adj = defaultdict(list)
        indeg = defaultdict(int)
        id_map = {}
        idx = 0

        def get_id(x):
            nonlocal idx
            if x not in id_map:
                id_map[x] = idx
                idx += 1
            return id_map[x]

        for _ in range(m):
            a, b = input().split()
            u = get_id(a)
            v = get_id(b)
            adj[u].append(v)
            indeg[v] += 1
            if u not in indeg:
                indeg[u] = indeg[u]

        n = idx
        q = deque()
        for i in range(n):
            if indeg[i] == 0:
                q.append(i)

        processed = 0
        while q:
            u = q.popleft()
            processed += 1
            for v in adj[u]:
                indeg[v] -= 1
                if indeg[v] == 0:
                    q.append(v)

        return "I disagree with the advisor" if processed == n else "No more comedians++"

    return solve()

# provided samples
assert run("2\nJEFF POTATO\nJEFF MARK\n") == "I disagree with the advisor"
assert run("3\nPETER PARKER\nPARKER WAYNE\nWAYNE PETER\n") == "No more comedians++"

# custom cases
assert run("1\nA B\n") == "I disagree with the advisor", "single edge"
assert run("2\nA B\nB C\n") == "I disagree with the advisor", "chain"
assert run("3\nA B\nB A\nC D\n") == "No more comedians++", "cycle + separate component"
assert run("0\n") == "I disagree with the advisor", "empty graph"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạnh đơn | hợp lệ | DAG tối thiểu | 
| chuỗi | hợp lệ | đặt hàng nhiều bước | 
| chu trình + thành phần | không hợp lệ | độ chính xác phát hiện chu kỳ | 
| đồ thị trống | hợp lệ | điều kiện biên | 

## Vỏ cạnh 

Trường hợp một cạnh là khi các ràng buộc hình thành nhiều thành phần bị ngắt kết nối. Thuật toán xử lý việc này một cách tự nhiên vì tất cả các nút có mức độ bằng 0 trên tất cả các thành phần đều được xếp vào hàng đợi ban đầu. Mỗi thành phần được xử lý độc lập mà không bị can thiệp. 

Một trường hợp khác là một chu trình thuần túy không có cạnh nào đến từ bên ngoài. Đối với đầu vào:```
A B
B C
C A
```tất cả các nút bắt đầu bằng cấp 1, vì vậy hàng đợi vẫn trống. Thuật toán ngay lập tức trả về lỗi do không thể thực hiện được tiến trình nào, xác định chính xác chu trình. 

Trường hợp cuối cùng là khi đồ thị lớn nhưng thưa thớt, chẳng hạn như một chuỗi dài 100.000 nút. Mỗi nút được xử lý chính xác một lần và mỗi cạnh được nới lỏng chính xác một lần, do đó hàng đợi không bao giờ phát triển vượt quá kích thước không đổi nhỏ so với cấu trúc, đảm bảo hiệu suất tuyến tính.
