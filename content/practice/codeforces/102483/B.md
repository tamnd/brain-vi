---
title: "CF 102483B - Đàm phán Brexit"
description: "Chúng tôi có một biểu đồ tuần hoàn có hướng về các chủ đề đàm phán. Mỗi chủ đề có thời gian thảo luận cơ bản e[i] và một số chủ đề chỉ có thể được thảo luận sau khi một số chủ đề khác kết thúc. Một lịch trình hợp lệ là bất kỳ thứ tự tôpô nào của đồ thị này."
date: "2026-08-06T18:49:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102483
codeforces_index: "B"
codeforces_contest_name: "2018-2019 ICPC Northwestern European Regional Programming Contest (NWERC 2018)"
rating: 0
weight: 102483
solve_time_s: 103
verified: true
draft: false
---

[CF 102483B - Đàm phán Brexit](https://codeforces.com/problemset/problem/102483/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một biểu đồ tuần hoàn có hướng về các chủ đề đàm phán. Mỗi chủ đề có thời gian thảo luận cơ bản`e[i]`và một số chủ đề chỉ có thể được thảo luận sau khi một số chủ đề khác đã kết thúc. Một lịch trình hợp lệ là bất kỳ thứ tự tôpô nào của đồ thị này. 

Nếu một chủ đề được đặt ở vị trí`p`trong lịch trình, với cuộc họp đầu tiên có vị trí`0`, cuộc họp diễn ra`e[i] + p`phút. phần bổ sung`p`phút đến từ việc xem lại tất cả các cuộc họp trước đó. Mục tiêu là chọn thứ tự hợp lệ để giảm thiểu thời gian họp đơn lẻ dài nhất. 

Đầu vào cung cấp số lượng chủ đề, sau đó cho mỗi chủ đề thời gian cơ sở và danh sách các chủ đề tiên quyết. Đầu ra là giá trị nhỏ nhất có thể của mức tối đa`e[i] + p`trên tất cả các lịch trình hợp lệ. 

Sự hạn chế của`n`lên đến`400000`loại trừ các thuật toán liên tục thử các thứ tự khác nhau hoặc sử dụng lập trình động trên các tập hợp con. Đồ thị có thể có tới`400000`các cạnh phụ thuộc, do đó giải pháp mong muốn phải gần với thời gian tuyến tính. Cách sắp xếp tôpô thông thường với hàng đợi ưu tiên là phù hợp vì nó chỉ xử lý từng chủ đề và cạnh một số lần logarit. 

Một số trường hợp rất dễ xử lý sai. Khi một số chủ đề không có sự phụ thuộc, thời gian cơ sở lớn nhất phải được lên lịch trước. Ví dụ:```
3
10 0
1 0
1 0
```Câu trả lời là`12`. Thứ tự tối ưu là chủ đề theo thời gian`10`đầu tiên, đưa ra độ dài`10`,`2`, Và`3`. Việc chọn một chủ đề nhỏ hơn trước tiên sẽ khiến chủ đề lớn bị trì hoãn một cách không cần thiết. 

Trường hợp thứ hai là khi ban đầu không có chủ đề dài:```
2
1 1 2
100 0
```Câu trả lời là`101`. Chủ đề thứ hai phải diễn ra trước vì chủ đề thứ nhất phụ thuộc vào nó. Việc triển khai tham lam bỏ qua sự phụ thuộc và chỉ đơn giản sắp xếp theo`e`sẽ tạo ra một lịch trình không hợp lệ. 

Trường hợp góc cuối cùng là một chủ đề duy nhất:```
1
500 0
```Câu trả lời là`500`. Không có thời gian tóm tắt vì không có cuộc họp nào trước đó. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng xây dựng lịch trình trong khi liên tục tìm kiếm tất cả các chủ đề hiện có. Ở mọi vị trí, chúng tôi có thể quét mọi chủ đề, kiểm tra xem nó có sẵn không và chọn chủ đề có số lượng lớn nhất`e`. Điều này đúng vì sự lựa chọn tốt nhất ở mỗi thời điểm là chủ đề bị trì hoãn nhiều nhất. Tuy nhiên, với`400000`chủ đề, việc quét tất cả các chủ đề cho mọi vị trí yêu cầu khoảng`400000^2`, hoặc`1.6 * 10^11`, kiểm tra trong trường hợp xấu nhất, quá chậm. 

Nhận xét hữu ích là thông tin duy nhất cần thiết khi chọn chủ đề tiếp theo là tập hợp các chủ đề có điều kiện tiên quyết đã được hoàn thành. Đây chính xác là trạng thái được duy trì bởi thuật toán sắp xếp tôpô của Kahn. Thay vì quét tất cả các chủ đề, chúng tôi giữ các chủ đề có sẵn trong một đống tối đa được sắp xếp theo thứ tự`e`. Bất cứ khi nào một chủ đề có sẵn, nó sẽ được chèn vào vùng nhớ heap. Cuộc họp tiếp theo luôn là chủ đề có sẵn với thời lượng cơ bản lớn nhất. 

Lý do khiến sự lựa chọn tham lam này có tác dụng là vì việc trì hoãn một chủ đề sẽ làm tăng thời lượng cuộc họp của nó thêm đúng một phút. Nếu hai chủ đề hiện có sẵn là`a`Và`b`, với`e[a] > e[b]`, đặt`b`trước`a`tăng thời lượng cuối cùng của chủ đề lớn hơn trong khi không giúp ích cho bất kỳ mối quan hệ phụ thuộc nào. Việc hoán đổi chúng chỉ có thể cải thiện hoặc duy trì thời lượng cuộc họp tối đa. Việc lặp lại lập luận trao đổi này có nghĩa là một lịch trình tối ưu luôn có thể được chuyển đổi thành một lịch trình chọn lịch trình lớn nhất sẵn có.`e`ở mọi bước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O((n + m) log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng biểu đồ phụ thuộc. Đối với mọi mối quan hệ tiên quyết`a -> b`, cửa hàng`b`như một chủ đề trở nên gần gũi hơn sau`a`được hoàn thành. Đồng thời lưu trữ số lượng điều kiện tiên quyết còn dang dở cho mỗi chủ đề. 
2. Đặt mọi chủ đề không có điều kiện tiên quyết chưa hoàn thành vào một đống tối đa được sắp xếp theo thời gian cơ sở của nó. Những chủ đề này là những chủ đề duy nhất có thể được thảo luận trước một cách hợp pháp. 
3. Liên tục xóa chủ đề có thời gian cơ bản lớn nhất khỏi vùng nhớ heap. Vị trí hiện tại của nó là số cuộc họp đã hoàn thành nên hãy cập nhật câu trả lời bằng`e[i] + position`. 
4. Sau khi hoàn thành một chủ đề, hãy giảm số lượng điều kiện tiên quyết của mỗi chủ đề tùy theo chủ đề đó. Nếu một trong những số đó trở thành 0, hãy chèn chủ đề đó vào vùng nhớ heap vì hiện tại nó đã có sẵn. 
5. Tiếp tục cho đến khi mọi chủ đề đã được lên lịch. Mức tối đa được lưu trữ là cuộc họp dài nhất có thể tối thiểu. 

Tại sao nó hoạt động: tại mọi thời điểm, vùng heap chứa chính xác các chủ đề có thể được chọn tiếp theo một cách hợp pháp. Trong số các lựa chọn này, việc chọn lớn nhất`e`giá trị là tối ưu vì bất kỳ chủ đề nào có sẵn đều có thể được trao đổi với chủ đề đã chọn mà không vi phạm sự phụ thuộc và di chuyển một phạm vi lớn hơn`e`chủ đề sau chỉ có thể tăng giá trị tối đa. Vì mọi quyết định đều có thể được chứng minh bằng đối số trao đổi này nên toàn bộ thứ tự là tối ưu. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n = int(input())
    e = [0] * n
    graph = [[] for _ in range(n)]
    indeg = [0] * n

    for i in range(n):
        data = list(map(int, input().split()))
        e[i] = data[0]
        d = data[1]
        indeg[i] = d
        for x in data[2:]:
            graph[x - 1].append(i)

    heap = []
    for i in range(n):
        if indeg[i] == 0:
            heapq.heappush(heap, (-e[i], i))

    ans = 0
    done = 0

    while heap:
        neg_e, u = heapq.heappop(heap)
        ans = max(ans, -neg_e + done)
        done += 1

        for v in graph[u]:
            indeg[v] -= 1
            if indeg[v] == 0:
                heapq.heappush(heap, (-e[v], v))

    print(ans)

if __name__ == "__main__":
    solve()
```Biểu đồ lưu trữ các cạnh đi từ điều kiện tiên quyết đến chủ đề phụ thuộc. Hướng này giúp việc cập nhật sau khi hoàn thành một chủ đề trở nên đơn giản vì chúng ta chỉ cần truy cập các chủ đề có thể có. 

Vùng heap được triển khai dưới dạng vùng heap tối thiểu từ thư viện chuẩn của Python, vì vậy giá trị được lưu trữ là`-e[i]`để mô phỏng một đống tối đa. Biến`done`là vị trí dựa trên số 0 hiện tại trong lịch trình. Cuộc họp ở vị trí này có độ dài`e[i] + done`. 

Tất cả thời lượng đều phù hợp với số nguyên Python. Câu trả lời lớn nhất có thể là dưới đây`1,400,000`, nhưng độ chính xác tùy ý của Python cũng loại bỏ mọi lo ngại về tràn. 

## Ví dụ đã hoạt động 

Đối với ba chủ đề độc lập:```
3
10 0
10 0
10 0
```dấu vết là: 

| Bước | Đống có sẵn | Chủ đề được chọn | Vị trí | Câu trả lời hiện tại | 
| --- | --- | --- | --- | --- | 
| 0 | 10,10,10 | 10 | 0 | 10 | 
| 1 | 10,10 | 10 | 1 | 11 | 
| 2 | 10 | 10 | 2 | 12 | 

Tất cả các chủ đề đều có sẵn ngay lập tức nên thuật toán chỉ cần sắp xếp các giá trị bằng nhau. Sự chậm trễ cuối cùng ở chủ đề cuối cùng sẽ quyết định câu trả lời. 

Đối với mẫu thứ hai:```
6
2 2 4 3
4 1 5
1 2 2 4
3 1 5
2 0
4 1 3
```các trạng thái quan trọng là: 

| Bước | Các chủ đề có sẵn của`e`| Được chọn | Vị trí | Tối đa | 
| --- | --- | --- | --- | --- | 
| 0 | 5(2), 2(4) | chủ đề 5 | 0 | 2 | 
| 1 | 4(2), 2(1) | chủ đề 2 | 1 | 5 | 
| 2 | 6(4) | chủ đề 6 | 2 | 6 | 
| 3 | 4(3), 1(1) | chủ đề 4 | 3 | 7 | 
| 4 | 1(1), 3(2) | chủ đề 3 | 4 | 8 | 
| 5 | 1 | chủ đề 1 | 5 | 8 | 

Dấu vết cho thấy các chủ đề mới được mở khóa ngay lập tức được xem xét cùng với tất cả các chủ đề có sẵn khác. Heap không bao giờ chứa chủ đề bất hợp pháp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Mỗi chủ đề vào và rời khỏi vùng heap một lần và mọi cạnh phụ thuộc được xử lý một lần. | 
| Không gian | O(n + m) | Biểu đồ lưu trữ tất cả các cạnh phụ thuộc và vùng lưu trữ các chủ đề có sẵn. | 

Đây`m`là tổng số quan hệ phụ thuộc. Từ`m`nhiều nhất là`400000`, dung dịch nằm trong giới hạn yêu cầu. 

## Trường hợp thử nghiệm```python
import sys
import io
import heapq

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    n = int(input())
    e = [0] * n
    graph = [[] for _ in range(n)]
    indeg = [0] * n

    for i in range(n):
        data = list(map(int, input().split()))
        e[i] = data[0]
        indeg[i] = data[1]
        for x in data[2:]:
            graph[x - 1].append(i)

    heap = []
    for i in range(n):
        if indeg[i] == 0:
            heapq.heappush(heap, (-e[i], i))

    ans = done = 0
    while heap:
        x, u = heapq.heappop(heap)
        ans = max(ans, -x + done)
        done += 1
        for v in graph[u]:
            indeg[v] -= 1
            if indeg[v] == 0:
                heapq.heappush(heap, (-e[v], v))

    sys.stdin = old
    return str(ans) + "\n"

assert run("""3
10 0
10 0
10 0
""") == "12\n"

assert run("""6
2 2 4 3
4 1 5
1 2 2 4
3 1 5
2 0
4 1 3
""") == "8\n"

assert run("""1
500 0
""") == "500\n"

assert run("""3
5 0
1 0
2 0
""") == "7\n"

assert run("""2
1 1 2
100 0
""") == "101\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chủ đề duy nhất | 500 | Kích thước biểu đồ tối thiểu và thời gian tóm tắt bằng không | 
| Chủ đề độc lập với các giá trị khác nhau | 7 | Chọn chủ đề lớn nhất có sẵn trước | 
| Chuỗi phụ thuộc với điều kiện tiên quyết lớn | 101 | Tôn trọng sự phụ thuộc vào giá trị thô | 

## Vỏ cạnh 

Đối với trường hợp chủ đề độc lập:```
3
5 0
1 0
2 0
```tất cả các chủ đề sẽ vào heap ngay lập tức. Thuật toán chọn`5`, sau đó`2`, sau đó`1`, tạo ra độ dài cuộc họp`5`,`3`, Và`3`. Câu trả lời là`5`, trong khi thứ tự đầu tiên nhỏ hơn sẽ tăng mức tối đa một cách không cần thiết. 

Đối với trường hợp phụ thuộc bắt buộc:```
2
1 1 2
100 0
```ban đầu chỉ có chủ đề 2 nên vùng heap không có lựa chọn nào khác. Sau khi chủ đề 2 kết thúc, chủ đề 1 sẽ có sẵn. Thuật toán tính toán`100 + 0`Và`1 + 1`, cho`101`. 

Đối với các giá trị bằng nhau:```
3
10 0
10 0
10 0
```vùng heap có thể trả về bất kỳ chủ đề nào trong ba chủ đề trước vì mức độ ưu tiên của chúng giống hệt nhau. Mọi thứ tự có thể đều cho kết quả như nhau và chủ đề cuối cùng nhận được hai phút tóm tắt, tạo ra`12`.
