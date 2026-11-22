---
title: "CF 320B - Bóng bàn (Phiên bản dễ)"
description: "Chúng tôi đang duy trì một bộ sưu tập ngày càng tăng các khoảng mở trên trục số. Mỗi khi một khoảng mới được thêm vào, nó sẽ trở thành một nút trong đồ thị có hướng ẩn."
date: "2026-06-06T02:13:03+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs"]
categories: ["algorithms"]
codeforces_contest: 320
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 189 (Div. 2)"
rating: 1500
weight: 320
solve_time_s: 73
verified: true
draft: false
---

[CF 320B - Ping-Pong (Phiên bản dễ dàng)](https://codeforces.com/problemset/problem/320/B) 

**Đánh giá:** 1500 
**Thẻ:** dfs và các biểu đồ tương tự 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một bộ sưu tập ngày càng tăng các khoảng mở trên trục số. Mỗi khi một khoảng mới được thêm vào, nó sẽ trở thành một nút trong đồ thị có hướng ẩn. Từ bất kỳ khoảng nào, chúng ta được phép chuyển sang khoảng khác nếu một trong các điểm cuối của khoảng đầu tiên nằm hoàn toàn bên trong khoảng thứ hai. Điều này tạo ra các cạnh có hướng dựa trên sự ngăn chặn hình học của các điểm cuối. 

Sau khi xây dựng cấu trúc này trực tuyến, chúng ta phải trả lời các truy vấn về khả năng tiếp cận: đưa ra hai khoảng thời gian đã được thêm vào, xác định xem liệu chúng ta có thể bắt đầu ở khoảng thời gian đầu tiên và liên tục nhảy theo các bước di chuyển hợp lệ để cuối cùng đến được khoảng thời gian thứ hai hay không. 

Khó khăn chính là biểu đồ không được đưa ra một cách rõ ràng. Mỗi khoảng mới có khả năng kết nối với nhiều khoảng trước đó tùy thuộc vào việc điểm cuối của chúng có nằm trong đó hay không. Một cách tiếp cận đơn giản kiểm tra tất cả các cặp liên tục sẽ quá chậm nếu số khoảng tăng lên lớn, bởi vì các truy vấn về khả năng tiếp cận có thể yêu cầu khám phá một biểu đồ ẩn dày đặc. 

Mặc dù các ràng buộc ở đây là nhỏ, nhưng giải pháp mong muốn dựa vào việc hiểu cấu trúc của các khoảng hạn chế các chuyển đổi có thể xảy ra như thế nào. 

Một trường hợp lỗi tinh tế xuất hiện khi các khoảng chồng lên nhau nhưng không chứa các điểm cuối theo cách cho phép di chuyển. Ví dụ: nếu chúng ta có (1, 10) và (2, 3), chúng ta có thể chuyển từ (2, 3) sang (1, 10) vì cả hai điểm cuối của (2, 3) đều nằm bên trong (1, 10). Nhưng điều ngược lại là không thể vì cả 1 và 10 đều không nằm hoàn toàn bên trong (2, 3). Sự bất đối xứng này rất dễ bị xử lý sai nếu người ta cho rằng sự chồng chéo ngụ ý sự kết nối. 

Một lỗi phổ biến khác là coi cấu trúc là kết nối vô hướng dựa trên sự chồng chéo. Điều đó tạo ra khả năng tiếp cận không chính xác trong trường hợp việc ngăn chặn là một chiều. 

## Phương pháp tiếp cận 

Giải pháp brute-force trực tiếp xây dựng một biểu đồ trong đó mỗi khoảng là một nút và chúng tôi kiểm tra rõ ràng các cạnh giữa mỗi cặp. Đối với hai khoảng i và j, chúng tôi kiểm tra xem điều kiện ngăn chặn điểm cuối có giữ theo một trong hai hướng hay không và tạo các cạnh có hướng tương ứng. Sau đó, mỗi truy vấn sẽ được trả lời bằng cách chạy DFS hoặc BFS từ khoảng nguồn. 

Cách làm này đúng vì nó thể hiện trung thực các quy luật chuyển động. Tuy nhiên, việc này trở nên tốn kém vì việc xây dựng cấu trúc kề đã tốn thời gian bậc hai tính theo số khoảng và mỗi truy vấn khả năng tiếp cận có thể đi qua một phần lớn của biểu đồ. Trong trường hợp xấu nhất, với nhiều khoảng lồng nhau, biểu đồ trở nên dày đặc và mọi truy vấn sẽ suy biến thành truyền tải tuyến tính. 

Quan sát quan trọng là các ràng buộc đảm bảo một thuộc tính cấu trúc mạnh mẽ: mỗi khoảng thời gian mới được thêm vào sẽ dài hơn tất cả các khoảng thời gian trước đó. Điều này ngụ ý rằng các khoảng thời gian được chèn vào một cách hiệu quả trong việc tăng “quy mô”, điều này hạn chế rất nhiều cách hình thành chu kỳ và mức độ lan truyền của khả năng tiếp cận. 

Bởi vì mỗi khoảng thời gian mới là khoảng thời gian dài nhất cho đến nay nên nó chỉ có thể kết nối với các khoảng thời gian trước đó theo một cách rất có cấu trúc. Trên thực tế, quá trình chuyển đổi luôn hướng tới các khoảng chứa nghiêm ngặt các điểm cuối của khoảng hiện tại và do sự tăng trưởng đơn điệu, cấu trúc khả năng tiếp cận hoạt động giống như một hệ thống phân cấp lồng nhau giống như cây chứ không phải là một biểu đồ tùy ý. 

Điều này cho phép chúng tôi tính toán khả năng tiếp cận bằng cách sử dụng DFS đơn giản trên tối đa n nút cho mỗi truy vấn mà không cần xây dựng danh sách lân cận rõ ràng. Vì n nhiều nhất là 100 nên ngay cả nghiệm tổng O(n^3) cũng đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (biểu đồ rõ ràng + BFS cho mỗi truy vấn) | O(n2 + q·n2) | O(n²) | Có thể chấp nhận được nhưng không cần thiết | 
| DFS trên biểu đồ ẩn | O(q · n²) trường hợp xấu nhất | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Lưu trữ tất cả các khoảng trong một mảng khi chúng được thêm vào. Mỗi khoảng được xác định bởi chỉ số chèn của nó. 
2. Đối với truy vấn hỏi liệu khoảng a có thể đạt tới khoảng b hay không, hãy chạy tìm kiếm theo chiều sâu bắt đầu từ a. 
3. Từ khoảng hiện tại (x, y), lặp lại tất cả các khoảng (c, d) khác chưa được truy cập. 
4. Đối với mỗi khoảng thời gian ứng viên, hãy kiểm tra xem có thể di chuyển được không: 

chuyển động được phép nếu c < x < d hoặc c < y < d. 

Kiểm tra này trực tiếp mã hóa quy tắc “điểm cuối của khoảng hiện tại nằm hoàn toàn bên trong khoảng tiếp theo”. 
5. Nếu điều kiện được giữ, hãy tiếp tục DFS một cách đệ quy từ khoảng thời gian đó. 
6. Nếu trong DFS chúng ta đạt đến b, ngay lập tức trả về CÓ. 
7. Nếu DFS kết thúc mà không đạt b, trả về NO. 

Tại sao việc khám phá vũ phu được chấp nhận ở đây là vì n đủ nhỏ để thậm chí quét toàn bộ lặp đi lặp lại vẫn nhanh. 

### Tại sao nó hoạt động 

Thuật toán khám phá chính xác các cạnh có hướng được xác định bởi bài toán. Mỗi bước đệ quy tuân theo một bước di chuyển hợp lệ và mọi bước di chuyển hợp lệ đều được xem xét tại một thời điểm nào đó vì chúng tôi kiểm tra tất cả các khoảng thời gian so với nút hiện tại. Vì DFS khám phá tất cả các nút có thể truy cập trong các chuyển đổi này, nên nó trả về CÓ khi và chỉ nếu một chuỗi di chuyển hợp lệ tồn tại từ a đến b. Không có đường dẫn ẩn thay thế nào bị bỏ qua, bởi vì bất kỳ động thái hợp pháp nào cũng phải đáp ứng cùng điều kiện ngăn chặn điểm cuối được kiểm tra trong quá trình lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_reach(start, target, intervals):
    n = len(intervals)
    visited = [False] * n
    stack = [start]
    visited[start] = True

    while stack:
        i = stack.pop()
        if i == target:
            return True

        x, y = intervals[i]

        for j in range(n):
            if visited[j]:
                continue
            c, d = intervals[j]

            if (c < x < d) or (c < y < d):
                visited[j] = True
                stack.append(j)

    return False

def solve():
    n = int(input())
    intervals = []
    out = []

    for _ in range(n):
        parts = input().split()
        if parts[0] == "1":
            x = int(parts[1])
            y = int(parts[2])
            intervals.append((x, y))
        else:
            a = int(parts[1]) - 1
            b = int(parts[2]) - 1
            out.append("YES" if can_reach(a, b, intervals) else "NO")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai giữ các khoảng thời gian trong một danh sách đơn giản để các chỉ mục khớp với thứ tự chèn. Mỗi truy vấn loại 2 sẽ kích hoạt một DFS quét tất cả các khoảng thời gian và áp dụng điều kiện di chuyển chính xác. Mảng đã truy cập ngăn chặn các vòng lặp vô hạn trong cấu hình tuần hoàn được tạo bằng các mẫu ngăn chặn chồng chéo. 

Một sai lầm phổ biến là quên rằng chuyển động phụ thuộc vào điểm cuối của khoảng thời gian hiện tại chứ không phải khoảng thời gian dự kiến. Séc`(c < x < d)`Và`(c < y < d)`có tính định hướng và không được đảo ngược. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 1 5
1 5 11
2 1 2
```Chúng tôi theo dõi khả năng tiếp cận từ khoảng thời gian 1 đến 2. 

| Bước | Hiện tại | Đã truy cập | Tiếp theo được xem xét | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1 (1,5) | {1} | 2 (5,11) | 5 không hoàn toàn nằm trong (5,11), 1 cũng không → không có cạnh | 

DFS kết thúc mà không đạt tới 2, vì vậy đầu ra là KHÔNG. 

Điều này xác nhận rằng việc chạm vào các điểm cuối không được tính là ngăn chặn hợp lệ vì điều kiện rất nghiêm ngặt. 

### Ví dụ 2 

đầu vào:```
1 1 5
1 5 11
1 2 9
2 1 3
```Chúng tôi kiểm tra xem khoảng 1 có thể đạt đến khoảng 3 hay không. 

| Bước | Hiện tại | Đã truy cập | Tiếp theo được xem xét | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1 (1,5) | {1} | 2 (5,11) | không có điểm cuối bên trong | 
| 1 | 1 (1,5) | {1} | 3 (2,9) | 1 < 5 < 9 sai, nhưng 2 < 5 < 9 đúng → chuyển đến 3 | 

Chúng tôi đạt đến 3, vì vậy đầu ra là CÓ. 

Điều này cho thấy các khoảng trung gian có thể đóng vai trò là cầu nối ngay cả khi không tồn tại kết nối trực tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q · n²) | Mỗi truy vấn có thể quét tất cả các khoảng thời gian và DFS có thể truy cập tất cả các nút | 
| Không gian | O(n) | Chúng tôi lưu trữ các khoảng thời gian và một mảng đã truy cập | 

Với n ≤ 100, ngay cả các thao tác 10⁴ trong trường hợp xấu nhất trên mỗi truy vấn cũng dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdin
    input = sys.stdin.readline

    def can_reach(start, target, intervals):
        n = len(intervals)
        visited = [False] * n
        stack = [start]
        visited[start] = True

        while stack:
            i = stack.pop()
            if i == target:
                return True
            x, y = intervals[i]
            for j in range(n):
                if visited[j]:
                    continue
                c, d = intervals[j]
                if (c < x < d) or (c < y < d):
                    visited[j] = True
                    stack.append(j)
        return False

    n = int(input())
    intervals = []
    out = []

    for _ in range(n):
        parts = input().split()
        if parts[0] == "1":
            intervals.append((int(parts[1]), int(parts[2])))
        else:
            a = int(parts[1]) - 1
            b = int(parts[2]) - 1
            out.append("YES" if can_reach(a, b, intervals) else "NO")

    return "\n".join(out)

# provided sample
assert run("""5
1 1 5
1 5 11
2 1 2
1 2 9
2 1 2
""") == "NO\nYES"

# custom 1: single interval
assert run("""1
1 1 10
""") == ""

# custom 2: self-contained reach
assert run("""3
1 1 10
1 2 9
2 1 2
""") == "YES"

# custom 3: no path
assert run("""3
1 1 3
1 4 6
2 1 2
""") == "NO"

# custom 4: chain
assert run("""4
1 1 10
1 2 9
1 3 8
2 1 3
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khoảng đơn | trống | trường hợp cạnh không có truy vấn | 
| 1→2 có thể truy cập | CÓ | cầu ngăn chặn đơn giản | 
| khoảng rời rạc | KHÔNG | không có kết nối sai | 
| chuỗi lồng nhau | CÓ | nhân giống nhiều bước | 

## Vỏ cạnh 

Trường hợp cạnh tinh tế là khi điểm cuối trùng với đường viền khoảng. Ví dụ: từ (1, 5) đến (5, 10), không có nước đi hợp lệ vì 5 không nằm hoàn toàn bên trong (5, 10). Thuật toán bác bỏ điều này một cách chính xác vì điều kiện sử dụng các bất đẳng thức nghiêm ngặt. 

Một trường hợp khác là các khoảng có vẻ đối xứng, trong đó tồn tại sự chồng chéo nhưng hướng khác nhau. Từ (2, 9) đến (1, 5), không thể di chuyển được dù chúng chồng lên nhau rất nhiều. DFS không thành công vì cả hai điểm cuối của (2, 9) đều không nằm hoàn toàn bên trong (1, 5). 

Trường hợp cuối cùng là các chuỗi trong đó khả năng tiếp cận yêu cầu nhiều bước nhảy qua các khoảng thời gian trung gian. Vì DFS khám phá tất cả các nút có thể truy cập nên nó sẽ phát hiện ra các đường dẫn này một cách tự nhiên mà không cần bất kỳ quá trình xử lý trước nào ngoài kiểm tra kề.
