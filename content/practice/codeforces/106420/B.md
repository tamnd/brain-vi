---
title: "CF 106420B - Trình thử nghiệm Beta"
description: "Bài toán có thể được hiểu là việc khám phá khả năng kết nối trong một hệ thống các phòng được định hướng. Mỗi phòng đại diện cho một nút trong biểu đồ và mỗi lỗ thông một chiều đại diện cho một cạnh được định hướng từ nút này sang nút khác."
date: "2026-06-20T03:46:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106420
codeforces_index: "B"
codeforces_contest_name: "UTPC Contest 3-11-26 (Beginner)"
rating: 0
weight: 106420
solve_time_s: 61
verified: true
draft: false
---

[CF 106420B - Trình thử nghiệm Beta](https://codeforces.com/problemset/problem/106420/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán có thể được hiểu là việc khám phá khả năng kết nối trong một hệ thống các phòng được định hướng. Mỗi phòng đại diện cho một nút trong biểu đồ và mỗi lỗ thông một chiều đại diện cho một cạnh được định hướng từ nút này sang nút khác. Bắt đầu từ phòng 1, chúng ta được yêu cầu xác định có bao nhiêu phòng riêng biệt cuối cùng có thể được ghé thăm bằng cách liên tục đi theo những kết nối được định hướng này. 

Do đó, đầu vào mô tả một đồ thị có hướng: các phòng là các đỉnh và các lỗ thông là các cạnh. Đầu ra là một số duy nhất, kích thước của tập hợp các đỉnh có thể tiếp cận bắt đầu từ đỉnh 1. 

Từ góc độ ràng buộc, đây là cài đặt truyền tải đồ thị tuyến tính hoặc gần tuyến tính cổ điển. Ngay cả khi không có giới hạn rõ ràng, các bài toán biểu đồ Codeforces ở dạng này thường cho phép tối đa 10^5 hoặc 2 × 10^5 đỉnh và cạnh. Điều đó ngay lập tức loại trừ mọi thứ bậc hai như kiểm tra khả năng tiếp cận giữa tất cả các cặp nút. Một giải pháp cố gắng tính toán lại khả năng tiếp cận từ đầu cho mọi nút sẽ đạt đến O(n(n + m)), điều này không khả thi trong các giới hạn thông thường. 

Một trường hợp phức tạp xuất phát từ chu kỳ và các thành phần bị ngắt kết nối. Việc truyền tải đệ quy đơn giản mà không theo dõi các nút đã truy cập có thể lặp lại mãi mãi. Ví dụ: nếu phòng 1 dẫn đến phòng 2 và phòng 2 dẫn trở lại phòng 1, việc truyền tải sẽ luân phiên vô tận trừ khi chúng ta đánh dấu rõ ràng các nút đã truy cập. 

Một trường hợp góc khác là khi phòng 1 không có cạnh hướng ra ngoài. Trong trường hợp đó, câu trả lời đúng là 1, vì chúng ta chỉ có thể ở lại phòng xuất phát. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là liên tục mở rộng từ nhóm phòng có thể tiếp cận hiện tại. Chúng ta có thể bắt đầu từ phòng 1, sau đó thêm tất cả các phòng có thể truy cập trực tiếp, sau đó từ những phòng đó thêm hàng xóm của họ, v.v., tính toán lại đường biên cho đến khi không có phòng mới nào xuất hiện. Nếu được triển khai kém, thao tác này sẽ liên tục quét tất cả các cạnh để kiểm tra xem nút bắt đầu của chúng có nằm trong tập có thể truy cập hay không. Trong trường hợp xấu nhất, mỗi đường chuyền mở rộng xử lý các cạnh O(m) và có thể có O(n) đường chuyền như vậy, dẫn đến hành vi O(nm). 

Cấu trúc của vấn đề gợi ý một cái nhìn rõ ràng hơn: khi một căn phòng có thể truy cập được, nó chỉ nên được xử lý một lần và khi chúng ta đã khám phá các lỗ thông hơi đi ra của nó, chúng ta không bao giờ cần phải xem lại chúng. Đây chính xác là cài đặt cho việc truyền tải biểu đồ đánh dấu các nút là đã truy cập khi được phát hiện lần đầu. Tìm kiếm theo chiều sâu hoặc tìm kiếm theo chiều rộng đều thực thi thuộc tính “truy cập một lần” này một cách tự nhiên và đảm bảo mỗi cạnh được duyệt qua nhiều nhất một lần. 

Điều này làm giảm việc quét lặp lại các cạnh thành một lần duyệt qua danh sách kề, mang lại độ phức tạp tuyến tính trong kích thước của biểu đồ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mở rộng lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Truyền tải DFS/BFS | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lập mô hình các phòng dưới dạng danh sách lân cận, trong đó mỗi phòng lưu trữ tất cả các phòng có thể truy cập trực tiếp từ đó. 

1. Xây dựng danh sách kề từ đầu vào. Mỗi lỗ thông hơi có hướng từ u đến v được lưu trữ trong danh sách của u. Điều này cho phép liệt kê hiệu quả các kết nối đi mà không cần quét các cạnh không liên quan. 
2. Tạo một mảng đã truy cập có kích thước n + 1, khởi tạo là false. Điều này ngăn cản việc xem lại các phòng và rất cần thiết để tránh các vòng lặp vô hạn trong biểu đồ tuần hoàn. 
3. Bắt đầu di chuyển từ phòng 1. Đánh dấu phòng 1 là đã ghé thăm ngay trước khi khám phá hàng xóm. Điều này đảm bảo chúng tôi không bao giờ xếp hàng hoặc tái sử dụng nó nữa. 
4. Sử dụng ngăn xếp hoặc hàng đợi để quản lý hoạt động khám phá. Bật một phòng và lặp lại tất cả các phòng lân cận của nó. Bất kỳ hàng xóm nào chưa được truy cập đều được đánh dấu đã truy cập và được thêm vào cấu trúc để xử lý sau. 
5. Tiếp tục cho đến khi không còn chỗ trống để xử lý. Tại thời điểm này, tất cả các nút có thể truy cập đã được phát hiện chính xác một lần. 
6. Đếm xem có bao nhiêu mục trong mảng đã truy cập là đúng. Số lượng này là số lượng phòng riêng biệt có thể tiếp cận.

Sự lựa chọn giữa ngăn xếp và hàng đợi không ảnh hưởng đến tính chính xác. DFS khám phá các đường dẫn sâu hơn trước tiên, trong khi BFS khám phá từng cấp độ, nhưng cả hai đều đảm bảo phạm vi bao phủ hoàn toàn về khả năng tiếp cận vì chúng mở rộng một cách có hệ thống dọc theo tất cả các cạnh đi ra của mỗi nút được phát hiện. 

### Tại sao nó hoạt động 

Điều bất biến chính là mọi phòng được đánh dấu là đã ghé thăm đều có thể truy cập được từ phòng 1 và cuối cùng mọi phòng có thể truy cập sẽ được đánh dấu là đã ghé thăm. Chúng tôi duy trì điều này bằng cách chỉ đánh dấu một nút đã truy cập khi lần đầu tiên chúng tôi phát hiện ra nó thông qua một cạnh được định hướng hợp lệ từ một nút đã có thể truy cập được. Vì chúng tôi không bao giờ truy cập lại các nút nên mỗi nút được xử lý nhiều nhất một lần và cuối cùng tất cả các cạnh đi ra từ các nút có thể truy cập đều được khám phá. Điều này đảm bảo rằng không có nút nào có thể truy cập bị bỏ sót và không có nút nào không thể truy cập được đưa vào không chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    adj = [[] for _ in range(n + 1)]
    
    for _ in range(m):
        u, v = map(int, input().split())
        adj[u].append(v)
    
    visited = [False] * (n + 1)
    stack = [1]
    visited[1] = True
    count = 0
    
    while stack:
        u = stack.pop()
        count += 1
        
        for v in adj[u]:
            if not visited[v]:
                visited[v] = True
                stack.append(v)
    
    print(count)

if __name__ == "__main__":
    solve()
```Việc xây dựng danh sách kề là chuyển trực tiếp dữ liệu đầu vào thành biểu diễn đồ thị. Mảng đã truy cập rất quan trọng để ngăn việc truy cập lại các nút và nó được cập nhật tại thời điểm nút được phát hiện thay vì khi nó được bật lên, điều này tránh được các mục trùng lặp trong ngăn xếp. 

DFS dựa trên ngăn xếp đảm bảo chúng tôi duyệt qua từng khu vực có thể truy cập được kết nối một cách hoàn chỉnh. Bộ đếm tăng chính xác một lần cho mỗi nút khi nó được xử lý, khớp với bất biến mà mỗi nút được bật lên một lần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một đồ thị nhỏ trong đó 1 dẫn đến 2 và 3, và 2 dẫn đến 4. 

Trạng thái ban đầu: 

| Bước | Ngăn xếp | Đã truy cập | Đếm | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | [1] | {1} | 0 | Bắt đầu từ nút 1 | 
| 2 | [] | {1} | 1 | Pop 1 | 
| 3 | [2, 3] | {1,2,3} | 1 | Mở rộng hàng xóm của 1 | 
| 4 | [2] | {1,2,3} | 2 | Pop 3 | 
| 5 | [] | {1,2,3} | 3 | Pop 2 | 
| 6 | [4] | {1,2,3,4} | 3 | Mở rộng hàng xóm của 2 | 
| 7 | [] | {1,2,3,4} | 4 | Pop 4 | 

Câu trả lời cuối cùng là 4, nghĩa là tất cả các nút đều có thể truy cập được từ 1. 

Dấu vết này cho thấy cách ngăn xếp khám phá tất cả các nhánh mà không bị trùng lặp và cách truy cập ngăn chặn việc xử lý lại các nút. 

### Ví dụ 2 

Một biểu đồ bị ngắt kết nối trong đó 1 chỉ kết nối với 2 và 3 bị cô lập. 

| Bước | Ngăn xếp | Đã truy cập | Đếm | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | [1] | {1} | 0 | Bắt đầu | 
| 2 | [] | {1} | 1 | Pop 1 | 
| 3 | [2] | {1,2} | 1 | Thăm hàng xóm 2 | 
| 4 | [] | {1,2} | 2 | Pop 2 | 

Nút 3 không bao giờ xuất hiện trong quá trình truyền tải nên nó không được tính. 

Điều này chứng tỏ rằng thuật toán chỉ giới hạn chính xác ở thành phần có thể truy cập được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi nút được truy cập một lần và mỗi cạnh được quét một lần trong quá trình truyền tải kề | 
| Không gian | O(n + m) | Danh sách kề lưu trữ tất cả các cạnh và mảng đã truy cập lưu trữ trạng thái nút | 

Độ phức tạp tuyến tính phù hợp với các ràng buộc điển hình đối với các vấn đề về khả năng tiếp cận biểu đồ trên Codeforces, trong đó n và m có thể lớn nhưng vẫn yêu cầu truyền tải hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue() if False else capture(inp)

def capture(inp: str) -> str:
    import sys, io
    backup_in = sys.stdin
    backup_out = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = backup_in
    sys.stdout = backup_out
    return out.strip()

# sample-like tests
assert capture("4 3\n1 2\n2 3\n1 4\n") == "4"
assert capture("3 1\n1 2\n") == "2"

# minimum case
assert capture("1 0\n") == "1"

# cycle case
assert capture("3 3\n1 2\n2 3\n3 1\n") == "3"

# disconnected graph
assert capture("5 2\n1 2\n2 3\n") == "3"

# star graph
assert capture("6 5\n1 2\n1 3\n1 4\n1 5\n1 6\n") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nút, không có cạnh | 1 | xử lý đồ thị tối thiểu | 
| chu kỳ | đếm đầy đủ | an toàn chu kỳ với đã truy cập | 
| ngắt kết nối | khả năng tiếp cận một phần | tính đúng đắn của hạn chế đối với thành phần | 
| ngôi sao | n | truyền tải nút mức độ cao | 

## Vỏ cạnh 

Một trường hợp tự lặp như`1 -> 1`được xử lý rõ ràng vì nút 1 được đánh dấu đã truy cập trước khi bắt đầu khám phá. Khi xử lý danh sách kề của nó, nút lân cận đã được truy cập nên nó bị bỏ qua và quá trình truyền tải kết thúc sau khi chỉ đếm nút 1. 

Một chu trình thuần túy như`1 -> 2 -> 3 -> 1`chứng minh tại sao mảng được truy cập là cần thiết. Nếu không có nó, DFS sẽ tiếp tục truy cập lại các nút vô thời hạn. Với nó, mỗi nút được chèn vào ngăn xếp một lần, được bật lên một lần và bị bỏ qua sau đó nếu gặp lại. 

Biểu đồ trong đó nút 1 không có cạnh đi ra sẽ được xử lý ngay lập tức: ngăn xếp bắt đầu bằng`[1]`, chúng tôi bật nó lên, tăng số lượng một lần và không tìm thấy hàng xóm nào, dẫn đến kết quả là 1.
