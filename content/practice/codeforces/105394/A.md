---
title: "CF 105394A - Cuộc tấn công của người ngoài hành tinh 2"
description: "Chúng ta được cung cấp một mạng xã hội gồm những người nơi tình bạn tạo thành một đồ thị vô hướng. Người ngoài hành tinh không thể bắt cóc cá nhân một cách độc lập."
date: "2026-06-23T04:57:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105394
codeforces_index: "A"
codeforces_contest_name: "2024-2025 ICPC German Collegiate Programming Contest (GCPC 2024)"
rating: 0
weight: 105394
solve_time_s: 44
verified: true
draft: false
---

[CF 105394A - Cuộc tấn công của người ngoài hành tinh 2](https://codeforces.com/problemset/problem/105394/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mạng xã hội gồm những người nơi tình bạn tạo thành một đồ thị vô hướng. Người ngoài hành tinh không thể bắt cóc cá nhân một cách độc lập. Nếu chọn bất kỳ người nào trong nhóm bạn được kết nối, họ phải đưa toàn bộ nhóm được kết nối cùng lúc để không ai bị phát hiện. 

Nhiệm vụ là xác định sức chứa tối thiểu cần thiết trên một con tàu vũ trụ để có thể hoàn thành tất cả các vụ bắt cóc cần thiết. Vì mỗi bộ phận được kết nối phải được xem xét tổng thể nên một chuyến đi có thể chở toàn bộ một bộ phận và con tàu phải có khả năng lắp vừa bộ phận lớn nhất đó. 

Đầu vào mô tả một biểu đồ có tối đa 200.000 người và 200.000 bạn bè. Thang đo này loại trừ mọi cách tiếp cận bậc hai, chẳng hạn như kiểm tra liên tục khả năng tiếp cận từ mọi nút bằng cách sử dụng các đường truyền mới. Bất kỳ lời giải đúng nào cũng phải xử lý đồ thị theo thời gian tuyến tính, thường là O(n + m), vì ngay cả O(n log n) cũng có thể chấp nhận được nhưng không cần thiết. 

Một trường hợp thất bại tinh tế xuất hiện khi tình bạn thưa thớt hoặc hoàn toàn vắng bóng. Nếu không có cạnh nào, mọi nút đều bị cô lập và mỗi nút tạo thành một thành phần có kích thước 1, vì vậy câu trả lời là 1. Một sai lầm ngây thơ là giả sử có ít nhất một cạnh tồn tại và chỉ bắt đầu theo dõi thành phần từ danh sách kề, điều này sẽ bỏ sót hoàn toàn các nút bị cô lập. 

Một trường hợp cạnh khác là khi đồ thị được kết nối đầy đủ. Trong trường hợp đó, câu trả lời là n, vì một thành phần duy nhất chứa tất cả mọi người. Bất kỳ giải pháp nào đếm cạnh không chính xác hoặc giả sử có nhiều thành phần tồn tại sẽ đánh giá thấp công suất cần thiết. 

## Phương pháp tiếp cận 

Một cách trực tiếp để giải quyết vấn đề là coi mỗi người là điểm bắt đầu và chạy duyệt đồ thị, chẳng hạn như tìm kiếm theo chiều sâu hoặc tìm kiếm theo chiều rộng, bất cứ khi nào chúng ta gặp một nút chưa được truy cập. Mỗi lần truyền tải sẽ phát hiện ra một thành phần được kết nối và chúng tôi đếm xem có bao nhiêu nút trong thành phần đó. Chúng tôi theo dõi kích thước tối đa trên tất cả các thành phần. 

Ý tưởng bạo lực này là đúng vì nó mô phỏng rõ ràng quy tắc: nhóm lực lượng tình bạn và khả năng kết nối xác định tư cách thành viên nhóm. Chi phí đến từ việc duyệt qua nhiều lần trên một biểu đồ lớn. Trong trường hợp xấu nhất, nếu chúng ta bất cẩn và không đánh dấu chính xác các nút đã truy cập, chúng ta có thể khám phá lại các phần lớn của biểu đồ nhiều lần, dẫn đến hành vi O(nm). Ngay cả khi theo dõi lượt truy cập chính xác, chúng ta vẫn cần duyệt qua mọi nút và cạnh một lần, điều này sẽ trở thành O(n + m), điều này là ổn. 

Cái nhìn sâu sắc quan trọng là vấn đề không yêu cầu chúng tôi mô phỏng nhiều chuyến đi hoặc bất kỳ quy trình động nào. Nó chỉ yêu cầu kích thước của thành phần được kết nối lớn nhất trong biểu đồ vô hướng. Khi chúng tôi nhận ra rằng mỗi lần bắt cóc tương ứng chính xác với một thành phần được kết nối, toàn bộ vấn đề sẽ giảm xuống việc tính toán kích thước thành phần và lấy mức tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Thăm dò ngây thơ lặp đi lặp lại | O(nm) | O(n + m) | Quá chậm | 
| Các thành phần được kết nối DFS/BFS | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Xây dựng biểu đồ danh sách kề của các cặp tình bạn. Điều này cho phép truyền tải hiệu quả hàng xóm của bất kỳ người nào mà không cần quét liên tục tất cả các cạnh. 
2. Duy trì mảng đã truy cập được khởi tạo thành false cho tất cả mọi người. Điều này đảm bảo mỗi nút được xử lý chính xác một lần trên tất cả các lần truyền tải. 
3. Lặp lại tất cả những người từ 1 đến n. Đối với mỗi người, nếu họ chưa được truy cập, hãy bắt đầu duyệt từ nút đó. 
4. Trong quá trình truyền tải, hãy sử dụng ngăn xếp hoặc hàng đợi để khám phá tất cả các nút có thể truy cập trong cùng một thành phần được kết nối. Đánh dấu từng nút đã truy cập ngay khi phát hiện ra để tránh phải truy cập lại. 
5. Đếm xem có bao nhiêu nút được truy cập trong quá trình truyền tải này. Số lượng này thể hiện kích thước của thành phần được kết nối hiện tại. 
6. Cập nhật câu trả lời với kích thước thành phần tối đa gặp phải cho đến nay. 
7. Sau khi xử lý tất cả các nút, xuất giá trị tối đa. 

Lý do quy trình này hoạt động là vì mỗi lần truyền tải khớp chính xác với định nghĩa của thành phần được kết nối trong biểu đồ vô hướng. Khi một nút được đánh dấu đã truy cập, nó không thể thuộc về một thành phần khác, vì vậy mỗi nút đóng góp chính xác vào một kích thước thành phần. Do đó, mức tối đa trên các kích thước này là sức chứa nhỏ nhất của tàu có thể xử lý bất kỳ nhóm bắt cóc cần thiết nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    adj = [[] for _ in range(n + 1)]

    for _ in range(m):
        a, b = map(int, input().split())
        adj[a].append(b)
        adj[b].append(a)

    visited = [False] * (n + 1)
    best = 1

    for i in range(1, n + 1):
        if visited[i]:
            continue

        stack = [i]
        visited[i] = True
        size = 0

        while stack:
            v = stack.pop()
            size += 1

            for to in adj[v]:
                if not visited[to]:
                    visited[to] = True
                    stack.append(to)

        if size > best:
            best = size

    print(best)

if __name__ == "__main__":
    solve()
```Việc xây dựng danh sách kề đảm bảo chúng ta có thể vượt qua tình bạn một cách hiệu quả. Mảng đã truy cập đảm bảo mỗi nút được xử lý một lần, ngăn chặn việc đếm lặp lại. Ngăn xếp DFS tránh các vấn đề về độ sâu đệ quy trong Python đối với các đồ thị lớn. Biến`best`lưu trữ thành phần được kết nối lớn nhất gặp phải. 

Một chi tiết triển khai tinh tế là đánh dấu các nút được truy cập vào thời điểm đẩy thay vì thời gian bật. Điều này ngăn cản việc chèn nhiều nút giống nhau vào ngăn xếp, điều này sẽ làm tăng thời gian chạy trong các biểu đồ dày đặc. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đồ thị đầu vào có năm nút với các cạnh tạo thành các thành phần {1,2,3} và {4,5}. 

| Nút bắt đầu | Ngăn xếp | Đã truy cập bộ | Kích thước thành phần | 
| --- | --- | --- | --- | 
| 1 | [1] → [] | {1,2,3} | 3 | 
| 2 | bỏ qua | đã ghé thăm | - | 
| 3 | bỏ qua | đã ghé thăm | - | 
| 4 | [4] → [] | {4,5} | 2 | 
| 5 | bỏ qua | đã ghé thăm | - | 

Kích thước thành phần lớn nhất là 3, trở thành dung lượng cần thiết. 

### Mẫu 2 

Không có tình bạn, mọi nút đều bị cô lập. 

| Nút | Hành động | Kích thước thành phần | 
| --- | --- | --- | 
| 1 | DFS mới | 1 | 
| 2 | DFS mới | 1 | 
| 3 | DFS mới | 1 | 

Tối đa là 1, nghĩa là tàu chỉ cần chỗ cho một người trong mỗi chuyến đi. 

Điều này xác nhận rằng các nút bị cô lập được xử lý chính xác và thuật toán không giả định sự hiện diện của các cạnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi nút được truy cập một lần và mỗi cạnh được đi qua nhiều nhất hai lần trong đồ thị vô hướng | 
| Không gian | O(n + m) | Danh sách kề cộng với mảng đã truy cập và ngăn xếp truyền tải | 

Các ràng buộc cho phép lên tới 200.000 nút và cạnh, đồng thời xử lý tuyến tính phù hợp thoải mái trong giới hạn thời gian trong Python khi được triển khai lặp đi lặp lại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sysio

    out = sysio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("5 3\n1 2\n2 3\n4 5\n") == "3", "sample 1"
assert run("3 0\n") == "1", "sample 2"

# custom cases
assert run("1 0\n") == "1", "single node"
assert run("4 3\n1 2\n2 3\n3 4\n") == "4", "fully connected chain"
assert run("6 3\n1 2\n3 4\n5 6\n") == "2", "multiple equal components"
assert run("5 4\n1 2\n2 3\n3 1\n4 5\n") == "3", "cycle plus pair"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- |
 | 1 nút, không có cạnh | 1 | minimal graph handling |
 | chuỗi 1-2-3-4 | 4 | thành phần lớn duy nhất | 
| cặp rời rạc | 2 | nhiều thành phần | 
| chu kỳ + cặp | 3 | cycle correctness |

 ## Vỏ cạnh 

Biểu đồ không có cạnh là trường hợp dễ xảy ra nhất khi triển khai không chính xác. Đối với đầu vào`3 0`, danh sách kề trống cho tất cả các nút. Thuật toán vẫn lặp lại trên mọi nút, bắt đầu DFS có kích thước 1 cho mỗi nút. Tối đa vẫn là 1, điều này đúng. 

Biểu đồ được kết nối đầy đủ sẽ hiển thị các lỗi trong đó việc truyền tải bị giới hạn không chính xác ở các lân cận ngay lập tức hoặc khi việc đánh dấu đã truy cập bị trì hoãn. Đối với đầu vào`4 6`với tất cả các cạnh có thể, bắt đầu từ nút 1 dẫn đến việc truyền tải cuối cùng đến tất cả các nút, tạo ra kích thước 4. Bất kỳ việc đánh dấu sớm hoặc lặp lại hàng xóm không chính xác nào sẽ đánh giá thấp kích thước này. 

Biểu đồ chuỗi kiểm tra mức độ lan truyền của khả năng tiếp cận. TRONG`1-2-3-4`, bắt đầu từ 1 cuối cùng phải đến 4 thông qua các nút trung gian. DFS dựa trên ngăn xếp đảm bảo quá trình đóng bắc cầu được khám phá đầy đủ, do đó kích thước thành phần sẽ trở thành 4 thay vì 1 hoặc 2, điều này sẽ xảy ra nếu chỉ tính các lân cận trực tiếp.
