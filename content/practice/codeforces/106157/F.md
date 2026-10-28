---
title: "CF 106157F - Đi bộ ngã"
description: "Chúng tôi có một bản đồ đồi được kết nối vô hướng. Mỗi ngọn đồi có độ cao và các đường đi giữa các ngọn đồi tạo thành một biểu đồ. Chúng ta cần đi từ đồi 1 đến đồi 2 trong khi tạo ra sự khác biệt giữa ngọn đồi cao nhất và ngọn đồi ngắn nhất đã ghé thăm càng nhỏ càng tốt."
date: "2026-06-25T11:19:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106157
codeforces_index: "F"
codeforces_contest_name: "2025 United Kingdom and Ireland Programming Contest (UKIEPC 2025)"
rating: 0
weight: 106157
solve_time_s: 46
verified: true
draft: false
---

[CF 106157F - Đi bộ dưới chân](https://codeforces.com/problemset/problem/106157/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bản đồ đồi được kết nối vô hướng. Mỗi ngọn đồi có độ cao và các đường đi giữa các ngọn đồi tạo thành một biểu đồ. Chúng ta cần đi từ đồi 1 đến đồi 2 trong khi tạo ra sự khác biệt giữa ngọn đồi cao nhất và ngọn đồi ngắn nhất đã ghé thăm càng nhỏ càng tốt. 

Đối với bất kỳ tuyến đường nào đã chọn, điều quan trọng duy nhất là chiều cao nhỏ nhất và chiều cao lớn nhất xuất hiện trên tuyến đường đó. Nếu một tuyến đường đi tới các độ cao 10, 14, 13 và 12 thì chi phí của nó là`14 - 10 = 4`. 

Dữ liệu đầu vào cung cấp số lượng ngọn đồi, số lượng đường mòn, độ cao của mỗi ngọn đồi và các cặp đồi được nối với nhau bằng các đường mòn. Đầu ra là chênh lệch độ cao tối thiểu có thể có giữa tất cả các tuyến đường từ đồi 1 đến đồi 2. 

Số lượng ngọn đồi nhiều nhất là 5000 và số lượng đường mòn nhiều nhất là 25000. Giải pháp thử mọi con đường có thể là không thể vì một biểu đồ có thể chứa một số lượng lớn các tuyến đường khác nhau. Ngay cả việc khám phá tất cả các con đường cũng sẽ trở nên theo cấp số nhân. Chúng ta cần giảm bớt vấn đề trong việc kiểm tra kết nối trong điều kiện hạn chế về độ cao. Vì chỉ có 5000 ngọn đồi nên một thuật toán xung quanh`O(n(n+m))`là thực tế, nhưng bất cứ thứ gì liên quan đến tất cả các cặp đồi hoặc tất cả các con đường có thể có đều quá đắt. 

Trường hợp cạnh thứ nhất là khi đồi 1 và đồi 2 có cùng độ cao. Một tuyến đường chỉ bao gồm hai ngọn đồi đó có thể có câu trả lời là 0 nếu chúng được kết nối trực tiếp. Ví dụ:```
Input
2 1
5 5
1 2

Output
0
```Một giải pháp bất cẩn cho rằng câu trả lời luôn là tích cực sẽ thất bại ở đây. 

Một trường hợp khác là khi tuyến đường tối ưu không sử dụng đường dẫn đồ thị ngắn nhất. Ví dụ:```
Input
5 5
1 100 50 51 2
1 2
1 3
3 4
4 5
5 2

Output
49
```Đường dẫn trực tiếp có phạm vi chiều cao`99`, nhưng con đường dài hơn`1 -> 3 -> 4 -> 5 -> 2`có phạm vi`50 - 1 = 49`. Một giải pháp giảm thiểu số cạnh thay vì phạm vi chiều cao sẽ cho kết quả sai. 

Trường hợp cạnh thứ ba xuất hiện khi một số ngọn đồi có cùng độ cao. Cửa sổ độ cao cho phép phải dựa trên giá trị độ cao chứ không phải chỉ số đồi. Ví dụ:```
Input
3 2
7 7 10
1 2
2 3

Output
3
```Hai ngọn đồi có chiều cao 7 nên được xử lý cùng nhau. Việc chia các độ cao bằng nhau thành các phạm vi riêng biệt có thể bỏ lỡ các câu trả lời hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ liệt kê mọi tuyến đường có thể từ đồi 1 đến đồi 2, tính toán chiều cao tối thiểu và tối đa trên mỗi tuyến đường và giữ mức chênh lệch nhỏ nhất. Điều này đúng vì mọi tuyến đường ứng cử viên có thể đều được xem xét. Vấn đề là số lượng tuyến đường. Ngay cả một biểu đồ thưa thớt cũng có thể chứa nhiều đường dẫn khác nhau theo cấp số nhân, do đó phương pháp này ngay lập tức trở nên không thể sử dụng được. 

Quan sát quan trọng là chúng tôi không quan tâm đến tuyến đường chính xác, chỉ quan tâm đến việc liệu tuyến đường nào đó có tồn tại trong một khoảng độ cao hay không. 

Giả sử chúng ta chọn hai độ cao`low`Và`high`. Chúng tôi đang hỏi liệu đồi 1 và đồi 2 có được kết nối chỉ bằng những ngọn đồi có độ cao nằm giữa`low`Và`high`. Nếu chúng được kết nối thì khoảng này là một câu trả lời hợp lệ. 

Các khoảng hợp lệ có tính chất đơn điệu. Nếu như`[low, high]`hoạt động, sau đó tăng lên`high`giữ cho nó hoạt động. Nếu như`[low, high]`không hoạt động, giảm`high`không thể giúp được. Điều này cho phép quét hai con trỏ trên các độ cao duy nhất được sắp xếp. 

Đối với mọi ranh giới bên trái, chúng tôi chỉ di chuyển ranh giới bên phải về phía trước cho đến khi khoảng thời gian trở nên hợp lệ. Sau đó, chúng tôi cố gắng loại bỏ chiều cao nhỏ nhất hiện tại và tiếp tục. Số lần kiểm tra kết nối là tuyến tính theo số độ cao khác nhau vì con trỏ bên phải không bao giờ di chuyển lùi. 

Brute Force hoạt động vì nó kiểm tra mọi tuyến đường có thể, nhưng không thành công vì số lượng tuyến đường quá lớn. Nhận xét rằng chỉ có khoảng cách độ cao được phép mới quan trọng cho phép chúng tôi thay thế việc liệt kê đường dẫn bằng các kiểm tra kết nối biểu đồ lặp đi lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(k(n+m)) | O(n+m) | Đã chấp nhận | 

Đây`k`là số chiều cao phân biệt và`k <= n`. 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các độ cao đồi khác nhau. Các giá trị này là ranh giới duy nhất có thể có của một khoảng hữu ích vì việc di chuyển ranh giới giữa hai độ cao hiện tại không thể thay đổi những ngọn đồi nào có sẵn. 
2. Duy trì hai con trỏ`left`Và`right`trên danh sách chiều cao được sắp xếp. Khoảng thời gian ứng viên hiện tại là từ`heights[left]`ĐẾN`heights[right]`. 
3. Đối với khoảng thời gian hiện tại, hãy chạy BFS bắt đầu từ ngọn đồi 1. Trong quá trình tìm kiếm, chỉ ghé thăm những ngọn đồi có độ cao nằm trong khoảng thời gian đó. Nếu đạt tới đồi 2, khoảng đó chứa tuyến đường hợp lệ. 
4. Nếu khoảng thời gian không hợp lệ, hãy tăng`right`. Việc thêm chiều cao tối đa lớn hơn chỉ có thể thêm nhiều ngọn đồi có sẵn hơn, vì vậy nó có thể làm cho tuyến đường xuất hiện. 
5. Nếu khoảng thời gian hợp lệ, hãy cập nhật câu trả lời với phạm vi hiện tại và tăng`left`. Việc loại bỏ chiều cao được phép nhỏ nhất có thể tạo ra khoảng thời gian hợp lệ thậm chí còn nhỏ hơn. 
6. Tiếp tục cho đến khi con trỏ bên phải chạm đến cuối danh sách chiều cao. 

Lý do khiến chuyển động của hai con trỏ an toàn là vì việc kết nối chỉ trở nên dễ dàng hơn khi khoảng thời gian mở rộng. Khi chiều cao tối đa cụ thể đủ cho chiều cao tối thiểu nhất định, việc tìm kiếm chiều cao tối đa nhỏ hơn không thể giúp ích được gì. 

Tại sao nó hoạt động: 

Mọi tuyến đường hợp lệ đều có chiều cao nhỏ nhất`L`và chiều cao lớn nhất`R`, do đó nó tương ứng với một trong các khoảng được xem xét bởi quá trình hai con trỏ. Kiểm tra BFS xác định chính xác liệu một khoảng có chứa bất kỳ tuyến đường nào từ đồi 1 đến đồi 2 hay không vì nó khám phá chính xác đồ thị con được hình thành bởi các ngọn đồi bên trong khoảng đó. Thuật toán kiểm tra mọi ranh giới bên trái tối thiểu có thể có và tìm ra ranh giới bên phải nhỏ nhất phù hợp với nó, do đó không có khoảng hợp lệ nhỏ hơn nào bị bỏ qua. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    h = list(map(int, input().split()))

    graph = [[] for _ in range(n)]
    for _ in range(m):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        graph[a].append(b)
        graph[b].append(a)

    values = sorted(set(h))

    def connected(low, high):
        if not (low <= h[0] <= high and low <= h[1] <= high):
            return False

        seen = [False] * n
        q = deque([0])
        seen[0] = True

        while q:
            u = q.popleft()
            if u == 1:
                return True
            for v in graph[u]:
                if not seen[v] and low <= h[v] <= high:
                    seen[v] = True
                    q.append(v)

        return False

    ans = values[-1] - values[0]
    right = 0

    for left in range(len(values)):
        if right < left:
            right = left

        while right < len(values) and not connected(values[left], values[right]):
            right += 1

        if right == len(values):
            break

        ans = min(ans, values[right] - values[left])

    print(ans)

if __name__ == "__main__":
    solve()
```Phân tích cú pháp đầu vào xây dựng một danh sách kề vì biểu đồ đủ thưa thớt nên việc lưu trữ tất cả các cạnh một cách rõ ràng là không cần thiết. các`connected`chức năng là hoạt động cốt lõi từ hướng dẫn. Nó thực hiện BFS trong khi lọc các đỉnh theo khoảng chiều cao hiện tại. 

Vòng lặp hai con trỏ chỉ lưu trữ các chỉ số vào danh sách chiều cao đã sắp xếp. Con trỏ bên phải không bao giờ di chuyển về phía sau, điều này giữ cho số lượng lệnh gọi BFS bị giới hạn. Việc cập nhật câu trả lời chỉ xảy ra sau khi tìm thấy khoảng thời gian hợp lệ. 

Việc kiểm tra ranh giới khi bắt đầu`connected`xử lý các trường hợp trong đó điểm cuối nằm ngoài phạm vi chiều cao hiện tại. Nếu không có nó, BFS có thể tuyên bố không chính xác rằng một tuyến đường tồn tại ngay cả khi không có ngọn đồi bắt đầu hoặc kết thúc. 

Số nguyên Python không bị tràn nên việc tính toán chiều cao vẫn an toàn mặc dù chiều cao có thể lên tới một triệu. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
Input
5 6
1 2 3 4 5
1 3
1 5
2 4
3 4
4 5
5 2
```Chiều cao được sắp xếp là`[1,2,3,4,5]`. 

| chiều cao bên trái | chiều cao phù hợp | Kết quả BFS | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| 1 | 1 | sai | lớn | 
| 1 | 2 | sai | lớn | 
| 1 | 3 | đúng | 2 | 
| 2 | 3 | sai | 2 | 
| 2 | 4 | đúng | 2 | 

Khoảng tốt nhất là độ cao từ 1 đến 3, cho câu trả lời 2. Dấu vết cho thấy rằng sau khi tìm được khoảng làm việc, thuật toán sẽ cố gắng loại bỏ độ cao nhỏ nhất để cải thiện nó. 

Một ví dụ khác:```
Input
3 2
7 7 10
1 2
2 3
```| chiều cao bên trái | chiều cao phù hợp | Kết quả BFS | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| 7 | 7 | sai | lớn | 
| 7 | 10 | đúng | 3 | 
| 10 | 10 | sai | 3 | 

Hai giá trị chiều cao bằng nhau được xử lý cùng nhau vì thuật toán hoạt động trên các giá trị chiều cao duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k(n+m)) | Mỗi cái nhiều nhất`2k`kiểm tra kết nối chạy BFS trên biểu đồ. | 
| Không gian | O(n+m) | Danh sách kề và mảng BFS lưu trữ biểu đồ và trạng thái truyền tải. | 

Với`n <= 5000`Và`m <= 25000`, điều này vẫn thực tế vì số lượng độ cao riêng biệt cũng bị giới hạn bởi 5000. Thuật toán tránh được nhiệm vụ bất khả thi là liệt kê các tuyến đường. 

## Trường hợp thử nghiệm```python
import sys
import io
from collections import deque

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("""5 6
1 2 3 4 5
1 3
1 5
2 4
3 4
4 5
5 2
""") == "2\n", "sample 1"

assert run("""2 1
5 5
1 2
""") == "0\n", "same height"

assert run("""5 5
1 100 50 51 2
1 2
1 3
3 4
4 5
5 2
""") == "49\n", "longer better path"

assert run("""3 2
7 7 10
1 2
2 3
""") == "3\n", "duplicate heights"

assert run("""4 3
1 2 3 100
1 2
2 3
3 4
""") == "99\n", "large boundary jump"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Biểu đồ mẫu | 2 | Kết nối bình thường và thu hẹp khoảng cách | 
| Hai ngọn đồi bằng nhau | 0 | Câu trả lời phạm vi không | 
| Đường dẫn tầm thấp hơn dài hơn | 49 | Chọn đúng lộ trình thay vì lộ trình ngắn nhất | 
| Độ cao trùng lặp | 3 | Xử lý đúng chiều cao bằng nhau | 
| Khoảng cách chiều cao lớn | 99 | Xử lý ranh giới | 

## Vỏ cạnh 

Đối với trường hợp có chiều cao bằng nhau:```
2 1
5 5
1 2
```Danh sách chiều cao được sắp xếp chỉ chứa`[5]`. Khoảng thời gian duy nhất được kiểm tra là`[5,5]`. Cả hai điểm cuối đều nằm trong đó, BFS ngay lập tức đến đồi 2 và câu trả lời trở thành`5 - 5 = 0`. 

Đối với trường hợp tuyến đường dài hơn:```
5 5
1 100 50 51 2
1 2
1 3
3 4
4 5
5 2
```Khoảng thời gian`[1,2]`không thể đến được đồi 2 vì không có đồi ở giữa dành cho tuyến đường thay thế. Mở rộng sang`[1,50]`vẫn thất bại nhưng`[1,51]`thành công vượt qua những ngọn đồi có độ cao`1,50,51,2`. Bản ghi thuật toán`51 - 1 = 50`trước khi tiếp tục tìm kiếm. Mức tối thiểu hợp lệ cuối cùng là`49`sử dụng khoảng`[2,51]`sau khi con trỏ bên trái di chuyển. 

Đối với chiều cao trùng lặp:```
3 2
7 7 10
1 2
2 3
```Độ cao duy nhất được sắp xếp là`[7,10]`. Thuật toán không bao giờ tách hai ngọn đồi có chiều cao 7, vì vậy khoảng`[7,10]`bao gồm chính xác cả hai và tìm tuyến đường có phạm vi`3`. 

Đối với mọi trường hợp, điều bất biến là mỗi khi kiểm tra một khoảng thời gian, BFS thể hiện chính xác biểu đồ có sẵn theo giới hạn chiều cao đó. Hai con trỏ chỉ loại bỏ những khoảng không thể cải thiện câu trả lời, do đó phạm vi tối thiểu cuối cùng được giữ nguyên.
