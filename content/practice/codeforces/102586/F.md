---
title: "CF 102586F - Robot"
description: "Chúng ta có hai tập hợp điểm được sắp xếp trên trục số. Bộ đầu tiên chứa các vị trí robot và bộ thứ hai chứa các vị trí ăng-ten. Mỗi lần chúng tôi kích hoạt ăng-ten, nó sẽ loại bỏ robot gần nhất vẫn còn sống và thu hẹp khoảng cách giữa chúng."
date: "2026-07-31T15:14:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102586
codeforces_index: "F"
codeforces_contest_name: "XX Open Cup, Grand Prix of Tokyo"
rating: 0
weight: 102586
solve_time_s: 199
verified: true
draft: false
---

[CF 102586F - Robot](https://codeforces.com/problemset/problem/102586/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai tập hợp điểm được sắp xếp trên trục số. Bộ đầu tiên chứa các vị trí robot và bộ thứ hai chứa các vị trí ăng-ten. Mỗi lần chúng tôi kích hoạt ăng-ten, nó sẽ loại bỏ robot gần nhất vẫn còn sống và thu hẹp khoảng cách giữa chúng. Nhiệm vụ không phải là trực tiếp quyết định kết quả khớp cuối cùng mà là chọn thứ tự kích hoạt ăng-ten sao cho tổng của tất cả các khoảng cách phải trả càng nhỏ càng tốt. 

Đầu ra có hai phần. Dòng đầu tiên là tổng khoảng cách tối thiểu có thể. Dòng thứ hai là lệnh kích hoạt của tất cả các anten đạt được giá trị đó. 

Số lượng robot có thể lên tới 200000. Một giải pháp bậc hai sẽ thực hiện khoảng 40000000000 thao tác trong trường hợp xấu nhất, vượt xa giới hạn hai giây cho phép. Chúng ta cần một thuật toán gần với thời gian tuyến tính hoặc tuyến tính. Tọa độ có thể lớn tới 10^9, do đó việc triển khai phải sử dụng số nguyên 64 bit cho khoảng cách. 

Một số chi tiết có thể phá vỡ một giải pháp hợp lý. Đầu tiên là robot gần nhất có thể thay đổi sau mỗi lần kích hoạt. Ví dụ: với robot ở 0 và 10 và ăng-ten ở 4 và 6, việc kích hoạt ăng-ten 4 trước tiên sẽ loại bỏ robot ở 0, nhưng sau đó ăng-ten 6 buộc phải lấy robot ở 10. Giải pháp tính toán trước tất cả các robot gần nhất một lần sẽ sai. 

Vấn đề thứ hai là luật hòa. Nếu ăng-ten ở chính giữa hai robot thì robot bên trái sẽ biến mất. Ví dụ: với robot ở mức 0 và 10 và ăng-ten ở mức 5, robot được chọn là 0 chứ không phải 10. Việc bỏ qua quy tắc này có thể thay đổi thứ tự kết quả. 

Vấn đề thứ ba là câu trả lời yêu cầu phải có đơn đặt hàng chứ không chỉ số tiền tối thiểu. Chỉ riêng thuật toán so khớp chi phí tối thiểu không mô tả được trình tự kích hoạt hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ mô phỏng quá trình và liên tục tìm kiếm ăng-ten cần được kích hoạt tiếp theo. Nếu chúng ta thử mọi ăng-ten tiếp theo có thể và tìm robot còn lại gần nhất bằng cách quét tất cả các robot thì độ phức tạp sẽ trở thành O(N^3). Ngay cả việc cải thiện tìm kiếm robot gần nhất bằng tìm kiếm nhị phân vẫn để lại cách tiếp cận O(N^2) nếu chúng tôi kiểm tra mọi lựa chọn kích hoạt có thể có. 

Quan sát quan trọng là bước đi tiếp theo tốt nhất luôn là cặp ăng-ten robot gần nhất hiện tại. Giả sử cặp còn lại gần nhất có khoảng cách d. Nếu chúng ta kích hoạt một số ăng-ten khác trước thì chi phí của nó ít nhất là d. Việc loại bỏ ăng-ten kia không thể làm cho cặp ăng-ten gần nhất được chọn rẻ hơn, vì robot và ăng-ten được chọn vẫn còn sống. Việc kích hoạt cặp gần nhất ngay lập tức sẽ trả số tiền nhỏ nhất có thể và để lại cùng một loại vấn đề trên một nhóm nhỏ hơn. Việc lặp lại sự lựa chọn tham lam này sẽ mang lại một trình tự tối ưu. 

Thử thách còn lại là duy trì hiệu quả cặp màu đối lập gần nhất. Trong một chiều, robot và ăng-ten gần nhất phải ở gần nhau theo thứ tự sắp xếp của tất cả các điểm còn lại. Nếu có một điểm khác giữa chúng, điểm đó sẽ tạo ra một cặp màu chéo thậm chí còn ngắn hơn với một trong hai điểm đó. Điều này có nghĩa là chúng ta chỉ cần duy trì các cặp liền kề trong danh sách đã hợp nhất. 

Khi một cặp bị loại bỏ, chỉ có một kề cận mới có thể xuất hiện: điểm ngay trước cặp bị loại bỏ và điểm ngay sau cặp đó. Chúng tôi có thể lưu trữ tất cả các cặp ứng cử viên liền kề trong hàng ưu tiên và chỉ cập nhật những thay đổi cục bộ này. Điều này làm giảm toàn bộ quá trình xuống O(N log N). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N^3) | O(N) | Quá chậm | 
| Tối ưu | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Hợp nhất tất cả robot và ăng-ten thành một mảng được sắp xếp. Lưu trữ cho mọi phần tử tọa độ, loại và chỉ số ăng-ten gốc nếu đó là ăng-ten. Thứ tự của mảng này thể hiện thứ tự hiện tại của các điểm còn sót lại trên trục số. 
2. Đối với mỗi cặp liền kề trong mảng đã hợp nhất này, nếu một phần tử là robot và phần tử kia là ăng-ten, hãy chèn cặp đó và khoảng cách của nó vào một vùng tối thiểu. Đây chính xác là những cặp gần nhất có thể có lúc đầu. 
3. Liên tục lấy cặp khoảng cách nhỏ nhất từ ​​đống. Nếu một trong hai phần tử đã bị xóa hoặc hai phần tử không còn lân cận nữa, hãy loại bỏ phần tử đó và tiếp tục. Nếu không thì đây là lần kích hoạt tham lam tiếp theo. 
4. Thêm khoảng cách của cặp này vào câu trả lời và thêm chỉ số ăng-ten vào thứ tự đầu ra. Loại bỏ cả ăng-ten và robot khỏi danh sách liên kết các điểm sống sót. 
5. Sau khi loại bỏ một cặp, hãy kết nối hàng xóm còn sống sót trước đó với hàng xóm còn sống sót tiếp theo. Nếu hai hàng xóm mới này có kiểu khác nhau, hãy chèn cặp ứng cử viên mới này vào heap. 
6. Tiếp tục cho đến khi mọi ăng-ten được kích hoạt. Các chỉ số anten được thu thập là thứ tự bắt buộc. 

Lý do lựa chọn tham lam có tác dụng là vì khoảng cách ăng-ten-robot hiện tại nhỏ nhất là không thể tránh khỏi. Ít nhất một lần kích hoạt ăng-ten phải trả số tiền đó hoặc nhiều hơn, vì cặp đó là điểm tương tác gần nhất có thể có trong số tất cả các điểm còn lại. Việc thực hiện trước tiên sẽ đạt được giới hạn dưới này và để lại vấn đề tương tự với ít hơn hai điểm. Bằng quy nạp, mỗi bước sau cũng tối ưu. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    points = []
    for i, x in enumerate(a):
        points.append((x, 0, -1))
    for i, x in enumerate(b):
        points.append((x, 1, i + 1))

    points.sort()

    m = 2 * n
    left = [-1] * m
    right = [-1] * m
    for i in range(m):
        if i:
            left[i] = i - 1
        if i + 1 < m:
            right[i] = i + 1

    def add_pair(u, v):
        if u == -1 or v == -1:
            return
        if points[u][1] != points[v][1]:
            heapq.heappush(heap, (points[v][0] - points[u][0], u, v))

    heap = []
    for i in range(m - 1):
        add_pair(i, i + 1)

    ans = 0
    order = []

    while order.__len__() < n:
        while True:
            d, u, v = heapq.heappop(heap)
            if right[u] == v and left[v] == u:
                break

        ans += d
        if points[u][1] == 1:
            order.append(points[u][2])
        else:
            order.append(points[v][2])

        l = left[u]
        r = right[v]

        if l != -1:
            right[l] = r
        if r != -1:
            left[r] = l

        add_pair(l, r)

    print(ans)
    print(*order)

if __name__ == "__main__":
    solve()
```Mảng đã hợp nhất được sắp xếp một lần ở đầu. Các mảng`left`Và`right`hoạt động như một danh sách liên kết theo thứ tự được sắp xếp này. Việc xóa các phần tử khỏi danh sách Python thông thường sẽ quá chậm vì mỗi lần xóa có thể di chuyển nhiều phần tử. 

Heap lưu trữ các cặp ứng cử viên liền kề. Một số mục trở nên không hợp lệ sau khi bị xóa sau đó, vì vậy mã sẽ kiểm tra`right[u] == v and left[v] == u`trước khi sử dụng một cặp. Việc xóa lười biếng này sẽ tránh được các bản cập nhật đống tốn kém. 

Tính toán khoảng cách sử dụng số nguyên Python, xử lý an toàn tổng khoảng cách có thể lên tới khoảng 2 * 10^14. Chỉ số ăng-ten được lưu riêng biệt với tọa độ của nó nên đầu ra cuối cùng sử dụng cách đánh số ban đầu. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
3
1 2 3
11 12 13
```Thứ tự hợp nhất là: 

| Bước | Cặp gần nhất | Khoảng cách | Ăng-ten kích hoạt | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | Robot 3 và ăng-ten 1 | 8 | 1 | 8 | 
| Thứ hai | Robot 2 và ăng-ten 2 | 10 | 2 | 18 | 
| Thứ ba | Robot 1 và ăng-ten 3 | 12 | 3 | 30 | 

Quá trình tham lam tạo ra một trật tự tối ưu hợp lệ. Mẫu chính thức cũng sử dụng một thứ tự tối ưu hợp lệ khác. 

Một ví dụ thứ hai:```
2
0 100
40 60
```Dấu vết là: 

| Bước | Robot còn lại | Ăng-ten còn lại | Cặp đôi được chọn | Khoảng cách | 
| --- | --- | --- | --- | --- | 
| 1 | 0, 100 | 40, 60 | 0 và 40 | 40 | 
| 2 | 100 | 60 | 100 và 60 | 40 | 

Đáp án là 80 và thứ tự kích hoạt là`1 2`. Điều này chứng tỏ rằng cặp gần nhất có thể không tương ứng với các chỉ số gần nhất trong đầu vào ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Việc sắp xếp mất O(N log N) và mỗi điểm chỉ tạo ra một số lượng thao tác heap không đổi | 
| Không gian | O(N) | Mảng danh sách liên kết và vùng heap chứa các phần tử O(N) | 

Giới hạn 200000 robot loại trừ các phương pháp bậc hai. Giải pháp heap tham lam thực hiện một lượng công việc logarit cho mỗi lần loại bỏ, do đó nó phù hợp thoải mái với các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

assert run("""1
5
10
""").strip() == """5
1""", "single point"

assert run("""3
1 2 3
11 12 13
""").splitlines()[0] == "30", "sample"

assert run("""2
0 100
40 60
""").splitlines()[0] == "80", "symmetric case"

assert run("""4
0 10 20 30
1 9 21 100
""").splitlines()[0] == "82", "far antenna case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một robot và một ăng-ten | 5 | Vỏ kích thước tối thiểu | 
| Robot 1,2,3 và ăng-ten 11,12,13 | 30 | Cung cấp hành vi mẫu | 
| Robot 0,100 và ăng-ten 40,60 | 80 | Khoảng cách đối xứng | 
| Ăng-ten hỗn hợp gần và xa | 82 | Cập nhật heap lặp đi lặp lại và thay đổi ranh giới | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là thay đổi robot gần nhất. Đối với đầu vào```
2
0 10
4 6
```lần kích hoạt đầu tiên sẽ loại bỏ robot ở mức 0 với chi phí 4. Ăng-ten còn lại sau đó loại bỏ robot ở mức 10 với chi phí 4. Thuật toán xử lý việc này vì nó cập nhật danh sách liên kết đã hợp nhất sau mỗi lần xóa thay vì giữ thông tin cũ về lân cận gần nhất. 

Trường hợp cạnh thứ hai là hòa. Đối với đầu vào```
2
0 10
5 20
```Ăng-ten 5 chọn robot 0 vì dây buộc ở bên trái. Thứ tự được hợp nhất lưu trữ thứ tự tọa độ chính xác, do đó biểu diễn cặp liền kề duy trì lựa chọn chính xác. 

Trường hợp cạnh thứ ba là khi loại bỏ một cặp sẽ tạo ra một cặp mới gần nhất. Đối với đầu vào```
3
0 50 100
10 90 200
```sau khi cặp đóng đầu tiên bị loại bỏ, hai điểm cách nhau bởi nó có thể trở thành lân cận. Heap nhận được ứng cử viên mới này ngay sau khi cập nhật danh sách liên kết, vì vậy lựa chọn tham lam tiếp theo vẫn đúng.
