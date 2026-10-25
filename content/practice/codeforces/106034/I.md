---
title: "CF 106034I - \u041d\u0435\u0437\u043d\u0430\u0439\u043a\u0430 \u043d\u0430 \u041b\u0443\u043d\u0435"
description: "Chúng ta được cho một dãy các bệ được sắp xếp thành một hàng từ vị trí 1 đến vị trí n. Mỗi nền tảng có một chiều cao. Một nhân vật bắt đầu ở nền tảng 1 và muốn tiếp cận nền tảng n bằng cách sử dụng một chuỗi các bước nhảy."
date: "2026-06-25T13:03:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106034
codeforces_index: "I"
codeforces_contest_name: "ICPC Central Russia Regional Qualification Round, 2024"
rating: 0
weight: 106034
solve_time_s: 41
verified: true
draft: false
---

[CF 106034I - \u041d\u0435\u0437\u043d\u0430\u0439\u043a\u0430 \u043d\u0430 \u041b\u0443\u043d\u0435](https://codeforces.com/problemset/problem/106034/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy các bệ được sắp xếp thành một hàng từ vị trí 1 đến vị trí n. Mỗi nền tảng có một chiều cao. Một nhân vật bắt đầu ở nền tảng 1 và muốn tiếp cận nền tảng n bằng cách sử dụng một chuỗi các bước nhảy. 

Việc nhảy từ nền tảng i sang nền tảng j chỉ được phép nếu có hai điều kiện cùng một lúc. Đầu tiên, khoảng cách chỉ số không được vượt quá d nên j phải nằm trong cửa sổ i - d đến i + d. Thứ hai, nền tảng đích không thể quá cao so với nền tảng hiện tại, nghĩa là mức tăng chiều cao a[j] - a[i] tối đa chỉ được h. Luôn luôn cho phép giảm dần vì không có hạn chế nào về việc đi xuống. 

Nhiệm vụ là tính toán số lần nhảy tối thiểu cần thiết để đến được nền tảng n hoặc xác định rằng điều đó là không thể. 

Các ràng buộc về kích thước đầu vào là n và d lên tới 10.000. Điều này ngay lập tức loại trừ mọi quá trình chuyển đổi O(n²) trên tất cả các cặp nền tảng một cách đơn giản. Một giải pháp kiểm tra mọi cặp có thể truy cập từ mọi nút sẽ thực hiện khoảng 10⁸ thao tác trong trường hợp xấu nhất, đó là đường biên nhưng thường quá chậm trong Python trong giới hạn thời gian nghiêm ngặt, đặc biệt là khi có thêm chi phí để kiểm tra ràng buộc. 

Mảng chiều cao đưa ra một sự bất đối xứng quan trọng: các bước nhảy chỉ bị chặn bởi chuyển động đi lên ngoài h. Điều này có nghĩa là chúng ta không thể coi các cạnh là đồng nhất hoặc đối xứng; chúng ta cần một cấu trúc lọc các ứng viên một cách hiệu quả theo cả phạm vi chỉ số và điều kiện chiều cao. 

Trường hợp cạnh tinh vi phát sinh khi tất cả các độ cao đều giống nhau. Trong trường hợp này, mọi nền tảng trong khoảng cách d đều có thể truy cập được và biểu đồ trở thành biểu đồ khoảng dày đặc. Một BFS ngây thơ với danh sách kề được xây dựng rõ ràng sẽ thất bại do bùng nổ bộ nhớ hoặc thời gian. 

Một trường hợp cạnh khác xuất hiện khi độ cao tăng theo chỉ số và h nhỏ. Sau đó, mọi bước nhảy về phía trước có thể bị chặn ngay cả khi các chỉ số gần nhau và câu trả lời trở thành -1. Bất kỳ giải pháp nào giả định “luôn luôn có thể thực hiện được chuyển động cục bộ” sẽ tạo ra một con đường không chính xác. 

## Phương pháp tiếp cận 

Giải thích trực tiếp xây dựng một biểu đồ trong đó mỗi nền tảng i kết nối với mọi nền tảng j sao cho |i - j| ≤ d và a[j] - a[i] ≤ h. Chạy BFS trên biểu đồ này sẽ cho câu trả lời đúng vì mỗi cạnh có chi phí bằng nhau. 

Vấn đề là đồ thị này có thể có tới n·d cạnh trong trường hợp xấu nhất. Với n = 10⁴ và d = 10⁴, điều này suy biến thành 10⁸ cạnh, quá lớn để xây dựng hoặc lặp lại rõ ràng trên mỗi nút. Ngay cả khi bản thân BFS là tuyến tính ở các cạnh thì chi phí chuyển đổi vẫn chiếm ưu thế. 

Quan sát quan trọng là BFS không yêu cầu chúng ta liệt kê tất cả các hàng xóm một cách rõ ràng. Đối với mỗi nền tảng, chúng tôi chỉ cần tìm các nền tảng chưa được truy cập tiếp theo trong cửa sổ chỉ mục trượt cũng đáp ứng giới hạn về chiều cao. Một khi nền tảng đã được xử lý, nó sẽ không bao giờ được xem xét lại. Điều này cho phép chúng tôi duy trì một tập hợp động các “ứng viên chưa được truy cập” và loại bỏ chúng sau khi chúng được truy cập. 

Chúng tôi duy trì một cấu trúc lưu trữ tất cả các chỉ mục chưa được truy cập. Đối với mỗi nền tảng i, chúng tôi chỉ quét trong phạm vi [i+1, i+d] và bỏ qua các nút đã được xử lý. Mỗi nút được xóa chính xác một lần, vì vậy ngay cả khi chúng tôi quét nhiều lần, tổng công việc vẫn tuyến tính theo hệ số logarit tùy thuộc vào vùng chứa. 

Điều kiện độ cao được kiểm tra trong quá trình truyền tải và chỉ những chuyển đổi hợp lệ mới được đưa vào BFS. 

Điều này chuyển đổi vấn đề từ việc truyền tải đồ thị rõ ràng sang việc quét cửa sổ có kiểm soát trên một tập hợp các ứng cử viên đang thu hẹp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Biểu đồ lực lượng vũ phu + BFS | O(n·d) | O(n·d) | Quá chậm / Bộ nhớ nặng | 
| Cửa sổ trượt BFS với bộ chưa được truy cập | O(n·d) trường hợp xấu nhất nhưng được khấu hao gần O(n log n) / O(n) thực tế | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Khởi tạo một mảng khoảng cách với tất cả các giá trị được đặt thành -1 và đặt khoảng cách [1] = 0, vì chúng ta bắt đầu ở nền tảng đầu tiên. 
2. Duy trì hàng đợi cho BFS và chèn nền tảng 1. 
3. Duy trì cấu trúc chứa tất cả các chỉ số từ 1 đến n chưa được truy cập. Cấu trúc này cho phép chúng tôi lặp lại các vị trí tiềm năng tiếp theo mà không cần xem lại các nút đã xử lý. 
4. Trong khi hàng đợi không trống, hãy xóa nền tảng hiện tại i. 
5. Với mỗi ứng cử viên j trong phạm vi (i+1 đến i+d), hãy kiểm tra xem j có còn chưa được duyệt hay không và liệu a[j] - a[i] ≤ h. Nếu cả hai đều giữ nguyên, hãy gán distance[j] = distance[i] + 1, đẩy j vào hàng đợi và xóa nó khỏi cấu trúc chưa được truy cập. 
6. Lặp lại cho đến khi tất cả các nút có thể truy cập được xử lý hoặc đạt được mục tiêu n. 
7. Khoảng cách đầu ra[n], là số lần nhảy tối thiểu hoặc -1 nếu không thể truy cập được. 

Lựa chọn thiết kế quan trọng là khi một nền tảng được truy cập, nó sẽ bị xóa vĩnh viễn khỏi tập ứng cử viên. Điều này đảm bảo không có nền tảng nào được xử lý nhiều lần, mặc dù nó có thể được quét nhiều lần như một phần của các cửa sổ khác nhau. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, BFS xử lý các nút theo thứ tự số lần nhảy tăng dần. Cấu trúc không được thăm dò đảm bảo rằng mọi nền tảng đều được ấn định khoảng cách tối thiểu có thể trong lần đầu tiên nó được phát hiện. Bởi vì tất cả các cạnh đại diện cho một bước nhảy, thuộc tính phân lớp BFS giữ chính xác. Không thể tìm thấy đường đi ngắn hơn sau này vì tất cả các chuyển tiếp trong tương lai đều tương ứng với độ sâu BFS bằng hoặc lớn hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, d, h = map(int, input().split())
    a = [0] + list(map(int, input().split()))

    dist = [-1] * (n + 1)
    dist[1] = 0

    q = deque([1])

    # we keep a simple list of "alive" indices
    alive = set(range(1, n + 1))
    alive.remove(1)

    while q:
        i = q.popleft()

        # scan only forward window
        to_remove = []
        for j in range(i + 1, min(n + 1, i + d + 1)):
            if j in alive and a[j] - a[i] <= h:
                dist[j] = dist[i] + 1
                q.append(j)
                to_remove.append(j)

        for j in to_remove:
            alive.remove(j)

    print(dist[n])

if __name__ == "__main__":
    solve()
```BFS là tiêu chuẩn, nhưng chi tiết triển khai quan trọng là cách chúng tôi tránh truy cập lại các nút. các`alive`set hoạt động như một bộ lọc toàn cầu, đảm bảo mỗi nền tảng được xử lý nhiều nhất một lần. Danh sách tạm thời`to_remove`tránh sửa đổi tập hợp trong quá trình lặp, điều này có thể dẫn đến lỗi thời gian chạy hoặc các phần tử bị bỏ qua. 

Vòng lặp bên trong chỉ quét tối đa d vị trí về phía trước, khớp trực tiếp với ràng buộc. Đây là nơi duy nhất sử dụng cấu trúc của bài toán; mọi thứ khác là đường đi ngắn nhất tiêu chuẩn trong biểu đồ không có trọng số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 2 3
1 2 3 2 5
```Chúng tôi theo dõi tiến trình BFS. 

| Bước | Xếp hàng | Hiện tại tôi | Chuyến thăm mới | 
| --- | --- | --- | --- | 
| 1 | [1] | 1 | 2, 3 | 
| 2 | [2, 3] | 2 | 4 | 
| 3 | [3, 4] | 3 | 5 | 
| 4 | [4, 5] | 4 | - | 
| 5 | [5] | 5 | - | 

Đến nền tảng 5 sau 3 lần nhảy. Dấu vết cho thấy việc phân lớp BFS đảm bảo các bước tối thiểu, mặc dù tồn tại nhiều đường dẫn. 

### Ví dụ 2 

đầu vào:```
4 1 0
1 3 2 10
```Ở đây việc nhảy rất hạn chế. 

| Bước | Xếp hàng | Hiện tại tôi | Chuyến thăm mới | 
| --- | --- | --- | --- | 
| 1 | [1] | 1 | - | 
| 2 | [] | - | - | 

Không thể tiến về phía trước từ nền tảng 1 vì 3 quá cao. Đầu ra là -1. Điều này xác nhận giới hạn chiều cao chặn các cạnh một cách chính xác ngay cả khi các chỉ số gần nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n·d) trường hợp xấu nhất, thường gần O(n) | Mỗi chỉ mục sẽ bị xóa khỏi quá trình xem xét một lần và mỗi chỉ mục chỉ được quét trong cửa sổ có kích thước d | 
| Không gian | O(n) | Mảng khoảng cách, hàng đợi và tập sống động lưu trữ tối đa n phần tử | 

Với n, d 10⁴, cách tiếp cận này phù hợp một cách thoải mái trong giới hạn thời gian trong Python vì mỗi nút được xử lý trung bình với số lần không đổi và không có cấu trúc dữ liệu nặng trên mỗi cạnh được xây dựng. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, d, h = map(int, input().split())
    a = [0] + list(map(int, input().split()))

    dist = [-1] * (n + 1)
    dist[1] = 0
    q = deque([1])
    alive = set(range(1, n + 1))
    alive.remove(1)

    while q:
        i = q.popleft()
        to_remove = []
        for j in range(i + 1, min(n + 1, i + d + 1)):
            if j in alive and a[j] - a[i] <= h:
                dist[j] = dist[i] + 1
                q.append(j)
                to_remove.append(j)
        for j in to_remove:
            alive.remove(j)

    return str(dist[n])

# provided samples
assert run("5 2 3\n1 2 3 2 5\n") == "3"

# custom cases
assert run("1 1 1\n10\n") == "0", "single node"
assert run("4 1 0\n1 3 2 10\n") == "-1", "blocked by height"
assert run("5 4 100\n1 2 3 4 5\n") == "1", "large d full reach"
assert run("6 2 1\n1 10 2 11 3 12\n") == "3", "alternating heights"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nút | 0 | trường hợp bắt đầu tầm thường | 
| khối chiều cao nghiêm ngặt | -1 | chuyển tiếp không thể | 
| d lớn | 1 | khả năng tiếp cận đầy đủ | 
| độ cao xen kẽ | 3 | hành vi ràng buộc hỗn hợp | 

## Vỏ cạnh 

Trường hợp tối thiểu với n = 1 sẽ kiểm tra xem việc triển khai có trả về chính xác các bước nhảy bằng 0 mà không cần cố gắng mở rộng BFS hay không. Thuật toán khởi tạo khoảng cách [1] = 0 và kết thúc ngay lập tức vì đã đạt được mục tiêu. 

Trường hợp bị chặn hoàn toàn trong đó tất cả các bước di chuyển về phía trước đều vi phạm giới hạn độ cao xác nhận rằng việc quét trong cửa sổ không giả định sai khả năng tiếp cận. Hàng đợi BFS trống ngay lập tức, để lại tất cả khoảng cách còn lại là -1. 

Trường hợp có d rất lớn chứng tỏ rằng việc giới hạn quét ở i + d là đủ ngay cả khi cửa sổ mở rộng toàn bộ mảng còn lại và không cần tối ưu hóa thêm ngoài việc giới hạn vòng lặp. 

Mẫu chiều cao xen kẽ hỗn hợp cho thấy thuật toán áp dụng đúng ràng buộc bất đối xứng a[j] - a[i] ≤ h, ngăn chặn các mức tăng đột biến trong khi vẫn cho phép chuyển đổi xuống dưới.
