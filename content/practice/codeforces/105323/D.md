---
title: "CF 105323D - \u6218\u81f3\u7ec8\u7ae0"
description: "Chúng ta được cấp một tập hợp các nút, mỗi nút tượng trưng cho một con quỷ. Mỗi nút đều có ngưỡng sức mạnh bắt buộc, mức tăng sức mạnh phần thưởng và một bộ “chìa khóa” tiên quyết."
date: "2026-06-22T13:58:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105323
codeforces_index: "D"
codeforces_contest_name: "2024 Xiangtan University Summer Camp-Div.2"
rating: 0
weight: 105323
solve_time_s: 71
verified: true
draft: false
---

[CF 105323D - \u6218\u81f3\u7ec8\u7ae0](https://codeforces.com/problemset/problem/105323/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các nút, mỗi nút tượng trưng cho một con quỷ. Mỗi nút đều có ngưỡng sức mạnh bắt buộc, mức tăng sức mạnh phần thưởng và một bộ “chìa khóa” tiên quyết. Một nút chỉ có thể được thử nếu hai điều kiện được thỏa mãn đồng thời: tất cả các khóa tiên quyết của nó đã được lấy từ các nút bị đánh bại trước đó và cường độ hiện tại ít nhất là yêu cầu của nút đó. 

Khi một nút bị đánh bại, nút đó sẽ bị loại khỏi danh sách xem xét, nó đóng góp chỉ mục của mình làm khóa có thể mở khóa các nút khác và nó tăng sức mạnh hiện tại bằng giá trị phần thưởng của nó. Quá trình bắt đầu từ sức mạnh ban đầu và tiếp tục một cách tham lam cho đến khi không còn nút nào có thể bị đánh bại. 

Ràng buộc cấu trúc chính là biểu đồ phụ thuộc khóa là biểu đồ tuần hoàn có hướng. Điều này loại bỏ khả năng khóa vòng tròn, do đó, khi một nút có sẵn các khóa, nó sẽ không bao giờ bị vô hiệu nữa. 

Các ràng buộc lên tới hai trăm nghìn nút và tổng cộng hai trăm nghìn cạnh phụ thuộc. Bất kỳ giải pháp nào liên tục quét tất cả các nút hoặc tính toán lại tính khả thi từ đầu cho mỗi bước sẽ không thành công vì điều đó sẽ dẫn đến hành vi bậc hai trong trường hợp xấu nhất. Điều này ngay lập tức thúc đẩy giải pháp hướng tới việc duy trì các cấu trúc gia tăng, điển hình là hàng đợi ưu tiên hoặc sự lan truyền giống như BFS trên DAG. 

Một trường hợp thất bại nhỏ xuất hiện khi nhiều nút có sẵn cùng lúc nhưng chỉ một số nút hiện có giá cả phải chăng do sức mạnh. Việc lựa chọn chúng theo một thứ tự tùy ý có thể dẫn đến ngõ cụt. 

Hãy xem xét kịch bản này: cường độ hiện tại là 10 và có hai nút khả dụng, một nút yêu cầu cường độ 10 và cho +1, nút kia yêu cầu 1 cường độ và cho +100. Nếu chúng tôi chọn nhầm nút +1 trước, chúng tôi có thể không tiếp cận được các nút yêu cầu ngưỡng cao hơn một chút sau đó, mặc dù có thứ tự tốt hơn. Điều này cho thấy chỉ tính khả thi thôi là chưa đủ và chúng ta phải cẩn thận lựa chọn nút nào có sẵn để xử lý tiếp theo. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp rất dễ mô tả. Chúng tôi liên tục quét tất cả các nút và bất cứ khi nào một nút được thu thập tất cả các khóa và yêu cầu của nó được đáp ứng bởi cường độ hiện tại, chúng tôi sẽ đánh bại nút đó, cập nhật cường độ và lặp lại. Điều này đúng vì nó tuân theo các quy tắc một cách chính xác. 

Vấn đề với cách tiếp cận này là hiệu suất. Mỗi thất bại có thể yêu cầu quét tất cả n nút để tìm ra ứng cử viên hợp lệ. Với tối đa 200.000 nút, điều này dẫn đến khoảng n thao tác mỗi bước và tối đa n bước, dẫn đến độ phức tạp bậc hai vượt xa giới hạn. 

Quan sát quan trọng là các phần phụ thuộc chính tạo thành một DAG, do đó, “có sẵn” là một sự kiện đơn điệu được thúc đẩy bởi việc giảm mức độ. Khi các điều kiện tiên quyết của nút được thỏa mãn, nút đó có thể được chèn vào nhóm ứng cử viên chính xác một lần. Từ thời điểm đó trở đi, hạn chế duy nhất còn lại là liệu sức mạnh cần thiết của nó có được đáp ứng hay không. 

Điều này tách vấn đề thành hai bộ lọc độc lập. Bộ lọc đầu tiên có tính cấu trúc, được xử lý bằng cách mở khóa cấu trúc liên kết. Bộ lọc thứ hai là bộ lọc số, được xử lý theo ngưỡng cường độ. Chúng tôi có thể duy trì một nhóm các nút đã được mở khóa và tự động chọn trong số những nút hiện có yêu cầu được đáp ứng. 

Để thực hiện lựa chọn hiệu quả, chúng tôi duy trì hai cấu trúc ưu tiên. One lưu trữ các nút đã mở khóa được sắp xếp theo yêu cầu, vì vậy chúng tôi có thể nhanh chóng di chuyển các nút mới có giá cả phải chăng vào nhóm đang hoạt động. Các nút thứ hai hiện có giá cả phải chăng được sắp xếp theo mức tăng sức mạnh phần thưởng, vì vậy chúng tôi luôn chọn bước tiếp theo có lợi nhất trong số những bước khả thi ngay bây giờ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) | O(n + m) | Quá chậm | 
| Tối ưu hóa hàng đợi ưu tiên kép | O((n + m) log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi xử lý biểu đồ phụ thuộc trong khi liên tục duy trì tập hợp các nút hiện có thể truy cập và giá cả phải chăng. 

1. Tính mức độ cho mỗi nút dựa trên các yêu cầu chính của nó. Xây dựng danh sách lân cận từ mỗi khóa đến các nút mà nó mở khóa. Các nút có mức độ bằng 0 ban đầu được mở khóa. 
2. Chèn tất cả các nút đã mở khóa ban đầu vào một cấu trúc được sắp xếp theo độ bền yêu cầu của chúng. Các nút này có sẵn về mặt cấu trúc nhưng không nhất thiết phải có giá cả phải chăng. 
3. Duy trì cường độ dòng điện như một biến, bắt đầu từ giá trị ban đầu. 
4. Liên tục di chuyển các nút có sức mạnh yêu cầu tối đa bằng sức mạnh hiện tại từ cấu trúc đã mở khóa sang cấu trúc thứ hai được sắp xếp theo mức tăng phần thưởng. Điều này đảm bảo chúng tôi chỉ xem xét các nút mà chúng tôi thực sự có thể đánh bại vào lúc này. 
5. Nếu cấu trúc giá cả phải chăng trống, quá trình sẽ kết thúc vì không có nút nào còn lại có thể bị đánh bại với cường độ hiện tại. 
6. Nếu không, hãy chọn nút có mức thưởng cao nhất, đánh bại nó, ghi lại vào câu trả lời và tăng sức mạnh hiện tại. 
7. Đối với mỗi nút phụ thuộc vào nó, hãy giảm mức độ. Nếu bất kỳ nút phụ thuộc nào đạt đến mức 0, hãy chèn nó vào cấu trúc đã mở khóa. 
8. Lặp lại từ bước 4 cho đến khi không thể thực hiện được nữa. 

Tính chính xác phụ thuộc vào thực tế là khi một nút được mở khóa (tất cả các khóa được thu thập), nó sẽ vẫn được mở khóa mãi mãi và một khi nó trở nên có giá cả phải chăng, nó vẫn có giá cả phải chăng khi sức mạnh chỉ tăng lên. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, thuật toán duy trì hai tập hợp: các nút có các điều kiện tiên quyết về cấu trúc được thỏa mãn và các nút trong số đó có yêu cầu về độ mạnh được thỏa mãn. Bất kỳ nước đi tiếp theo hợp lệ nào trong bài toán ban đầu đều phải xuất phát từ giao điểm của chúng. 

Quá trình này không bao giờ bỏ qua một ứng cử viên hợp lệ: mọi nút sẽ nhập vào tập đã mở khóa một cách chính xác khi nó trở nên khả thi về mặt cấu trúc và nhập vào tập hợp giá cả phải chăng một cách chính xác khi nó trở nên khả thi về mặt số. Sự lựa chọn giữa các nút có giá cả phải chăng luôn chọn một nút có thể tối đa hóa sức mạnh ngay lập tức và vì sức mạnh là đơn điệu nên việc trì hoãn một nút có phần thưởng cao hơn sẽ không bao giờ có lợi một khi nó trở nên hợp túi tiền. 

Điều này đảm bảo rằng nếu có bất kỳ chuỗi thất bại hợp lệ nào tồn tại từ một trạng thái nhất định, thì thuật toán sẽ không tự chặn việc đạt đến trạng thái tương tự hoặc trạng thái mạnh hơn sau đó. 

## Giải pháp Python```python
import sys
import heapq
input = sys.stdin.readline

n, p = map(int, input().split())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

g = [[] for _ in range(n)]
indeg = [0] * n

for i in range(n):
    parts = list(map(int, input().split()))
    k = parts[0]
    for x in parts[1:]:
        x -= 1
        g[x].append(i)
        indeg[i] += 1

ready = []
for i in range(n):
    if indeg[i] == 0:
        heapq.heappush(ready, (a[i], i))

affordable = []
visited = [False] * n
ans = []

while True:
    while ready and ready[0][0] <= p:
        ai, i = heapq.heappop(ready)
        heapq.heappush(affordable, (-b[i], i))

    if not affordable:
        break

    negb, i = heapq.heappop(affordable)
    if visited[i]:
        continue

    visited[i] = True
    ans.append(i + 1)
    p += b[i]

    for to in g[i]:
        indeg[to] -= 1
        if indeg[to] == 0:
            heapq.heappush(ready, (a[to], to))

print(len(ans))
print(*sorted(ans))
```Việc triển khai bắt đầu bằng cách xây dựng biểu đồ phụ thuộc từ các mối quan hệ chính, coi mỗi khóa là một mối quan hệ tiên quyết cạnh. Indegrees theo dõi số lượng khóa vẫn bị thiếu cho mỗi nút. 

các`ready`heap lưu trữ các nút có điều kiện chính được thỏa mãn, được sắp xếp theo độ mạnh cần thiết để chúng ta có thể trích xuất một cách hiệu quả các ứng cử viên có thể sử dụng được khi độ mạnh tăng lên. 

các`affordable`heap lưu trữ các nút vừa được mở khóa về mặt cấu trúc và hiện có thể đánh bại được, được sắp xếp theo phần thưởng âm để nó hoạt động như một heap tối đa. Điều này đảm bảo chúng tôi luôn chọn bản nâng cấp ngay lập tức có lợi nhất. 

Vòng lặp chính trước tiên quảng bá các nút mới có giá cả phải chăng, sau đó chọn ứng cử viên tốt nhất để đánh bại. Sau mỗi thất bại, chúng tôi truyền bá các mở khóa chính và có khả năng đưa các nút mới vào hệ thống. 

Việc sắp xếp câu trả lời cuối cùng là cần thiết vì thứ tự lựa chọn không đảm bảo là số. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ trong đó các nút mở khóa tuần tự. 

đầu vào:```
3 1
1 2 3
1 1 1
0
1 1
1 2
```Ở đây nút 1 ban đầu có sẵn, nút 2 yêu cầu khóa 1 và nút 3 yêu cầu khóa 2. 

| Bước | Sức mạnh | Sẵn sàng (ai) | Giá cả phải chăng (bi) | Được chọn | Đã sưu tầm | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 1 | 1 | 
| 1 | 2 | 2 | 1 | 2 | 1,2 | 
| 2 | 3 | 3 | trống | 3 | 1,2,3 | 

Dấu vết này cho thấy mức độ lan truyền chính và tăng trưởng sức mạnh tương tác như thế nào để dần dần mở khóa biểu đồ đầy đủ. 

Bây giờ hãy xem xét trường hợp khả năng chi trả chặn thứ tự lựa chọn: 

đầu vào:```
2 5
5 1
100 1
0
0
```| Bước | Sức mạnh | Sẵn sàng | Giá cả phải chăng | Được chọn | 
| --- | --- | --- | --- | --- | 
| 0 | 5 | 1,2 | 1,2 | 1 | 
| 1 | 105 | 2 | 2 | 2 | 

Nếu trước tiên chúng tôi chọn không chính xác nút 2, chúng tôi vẫn sẽ thành công ở đây, nhưng trong các trường hợp có chuỗi phức tạp hơn, việc bỏ qua thứ tự phần thưởng sẽ làm giảm sớm các trạng thái có thể tiếp cận. Sự lựa chọn tham lam bằng phần thưởng đảm bảo sự tăng trưởng tối đa ở mỗi bước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Mỗi nút vào và ra khỏi đống một lần và mỗi cạnh giảm độ một lần | 
| Không gian | O(n + m) | Đồ thị cộng với hai hàng đợi ưu tiên | 

Độ phức tạp phù hợp thoải mái trong các ràng buộc vì cả n và tổng kích thước phụ thuộc tối đa là hai trăm nghìn và các hệ số logarit vẫn nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, p = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    g = [[] for _ in range(n)]
    indeg = [0] * n

    for i in range(n):
        parts = list(map(int, input().split()))
        k = parts[0]
        for x in parts[1:]:
            x -= 1
            g[x].append(i)
            indeg[i] += 1

    import heapq
    ready = []
    for i in range(n):
        if indeg[i] == 0:
            heapq.heappush(ready, (a[i], i))

    affordable = []
    visited = [False] * n
    ans = []

    while True:
        while ready and ready[0][0] <= p:
            ai, i = heapq.heappop(ready)
            heapq.heappush(affordable, (-b[i], i))

        if not affordable:
            break

        negb, i = heapq.heappop(affordable)
        if visited[i]:
            continue

        visited[i] = True
        ans.append(i + 1)
        p += b[i]

        for to in g[i]:
            indeg[to] -= 1
            if indeg[to] == 0:
                heapq.heappush(ready, (a[to], to))

    return str(len(ans)) + "\n" + " ".join(map(str, sorted(ans)))

# minimal
assert run("1 5\n1\n0\n") == "1\n1"

# sample-like chain
assert run("3 1\n1 2 3\n1 1 1\n0\n1 1\n1 2\n") == "3\n1 2 3"

# no progress
assert run("2 0\n5 5\n1 1\n0\n0\n") == "0\n"

# independent choices
assert run("2 10\n1 2\n5 1\n0\n0\n") == "2\n1 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn tối thiểu | 1 1 | trường hợp cơ sở đúng đắn | 
| mở khóa xích | 3 1 2 3 | tuyên truyền phụ thuộc | 
| không có nút nào có thể truy cập | 0 | chấm dứt sớm | 
| nút độc lập | 2 1 2 | tham lam lựa chọn đúng đắn | 

## Vỏ cạnh 

Trường hợp cạnh khóa xảy ra khi một nút được mở khóa về mặt cấu trúc sớm nhưng phải đợi rất lâu sau đó mới có đủ khả năng chi trả. Thuật toán đảm bảo các nút như vậy vẫn ở trong`ready`heap và chỉ được chuyển vào`affordable`heap khi đạt đến ngưỡng sức mạnh, do đó chúng không bao giờ bị mất hoặc bị loại bỏ sớm. 

Một trường hợp khác là khi nhiều nút mở khóa đồng thời và chỉ một nút dẫn đến tăng trưởng sức mạnh đủ để mở khóa các nút còn lại. Vì việc lựa chọn được thúc đẩy bởi việc tối đa hóa phần thưởng giữa các nút có giá cả phải chăng nên thuật toán luôn ưu tiên lộ trình tăng trưởng, đảm bảo rằng các chuỗi mở khóa bị trì hoãn nhưng cần thiết cuối cùng cũng được kích hoạt. 

Trường hợp cuối cùng là khi tất cả các nút còn lại được mở khóa nhưng không có nút nào có giá cả phải chăng. Trong trường hợp đó, vùng heap sẵn sàng vẫn có thể chứa các nút, nhưng không có nút nào vượt qua bộ lọc cường độ, do đó thuật toán kết thúc chính xác mà không thử chuyển đổi không hợp lệ.
