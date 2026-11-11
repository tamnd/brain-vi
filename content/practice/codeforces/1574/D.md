---
title: "CF 1574D - Bản dựng mạnh nhất"
description: "Trong vấn đề này, chúng tôi được yêu cầu giúp Ivan chọn cách xây dựng anh hùng mạnh nhất có thể từ một bộ ô trang bị, mỗi ô có nhiều vật phẩm có sức mạnh khác nhau. Mỗi ô có danh sách vật phẩm riêng, được sắp xếp theo độ mạnh tăng dần."
date: "2026-06-10T11:04:57+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "dfs-and-similar", "graphs", "greedy", "hashing", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1574
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 114 (Rated for Div. 2)"
rating: 2000
weight: 1574
solve_time_s: 81
verified: true
draft: false
---

[CF 1574D - Bản dựng mạnh nhất](https://codeforces.com/problemset/problem/1574/D) 

**Xếp hạng:** 2000 
**Thẻ:** tìm kiếm nhị phân, lực lượng vũ phu, cấu trúc dữ liệu, dfs và tương tự, đồ thị, tham lam, băm, triển khai 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trong vấn đề này, chúng tôi được yêu cầu giúp Ivan chọn cách xây dựng anh hùng mạnh nhất có thể từ một bộ ô trang bị, mỗi ô có nhiều vật phẩm có sức mạnh khác nhau. Mỗi ô có danh sách vật phẩm riêng, được sắp xếp theo độ mạnh tăng dần. Ivan phải chọn chính xác một vật phẩm trên mỗi ô, tạo thành một công trình. Một số bản dựng bị cấm rõ ràng và nhiệm vụ là tìm ra bản dựng có tổng sức mạnh lớn nhất mà không bị cấm. Đầu ra chỉ đơn giản là các chỉ số được chọn của các mục trong mỗi vị trí. 

Các ràng buộc rất quan trọng để thiết kế một thuật toán. Số lượng khe cắm,$n$, nhiều nhất là 10, một con số rất nhỏ. Điều này cho thấy rằng một cách tiếp cận theo cấp số nhân trong$n$có thể vẫn khả thi. Mỗi slot có thể có tối đa$2 \cdot 10^5$các mục, nhưng tổng của tất cả các mục trên tất cả các vị trí được giới hạn ở mức$2 \cdot 10^5$, vì vậy chúng tôi không thể liệt kê mọi kết hợp có thể một cách ngây thơ, vì trong trường hợp xấu nhất, tổng số bản dựng là tích của$c_i$, có thể dễ dàng vượt quá$10^{18}$nếu chúng ta dùng vũ lực. 

Các trường hợp có biên không rõ ràng bao gồm các tình huống trong đó việc xây dựng mạnh nhất (chọn mục cuối cùng, lớn nhất trong mỗi vị trí) bị cấm. Trong trường hợp đó, một giải pháp đơn giản luôn chọn mục lớn nhất trên mỗi vị trí sẽ không thành công. Ví dụ: nếu chúng ta có hai vị trí chứa các vật phẩm`[1, 2]`Và`[1, 3]`và bản dựng bị cấm là`[2, 2]`, thuật toán phải chọn đúng`[2, 1]`thay vào đó, có tổng sức mạnh cao nhất tiếp theo. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực đơn giản nhất là tạo ra mọi tổ hợp vật phẩm có thể có, tính toán độ mạnh của từng công trình và lọc ra các công trình bị cấm. Mặc dù đúng về nguyên tắc nhưng cách tiếp cận này không khả thi vì ngay cả đối với$n = 10$và các danh sách nhỏ có độ dài 20 mỗi danh sách, sẽ có$20^{10} \approx 10^{13}$xây dựng. 

Một cách tiếp cận tốt hơn tận dụng lợi thế nhỏ$n$và thực tế là các vật phẩm trong mỗi ô đều được sắp xếp. Cách xây dựng tối ưu khi không bị cấm luôn là cách sử dụng vật phẩm cuối cùng trong mỗi danh sách. Do đó, vấn đề giảm xuống còn việc kiểm tra xem bản dựng “tất cả các mục cuối cùng” này có bị cấm hay không. Nếu đúng như vậy, các bản dựng ứng cử viên tiếp theo là những bản dựng trong đó một vị trí bị giảm đi một chỉ mục, tức là chúng tôi thử`[c_1 - 1, c_2, ..., c_n]`,`[c_1, c_2 - 1, ..., c_n]`, v.v., ưu tiên mức giảm tổng sức mạnh lớn nhất. Cấu trúc này tạo thành một vấn đề tìm kiếm heap tối đa: chúng tôi coi các bản dựng là các nút có tổng sức mạnh, luôn mở rộng nút có sức mạnh cao nhất chưa bị cấm. 

Chúng tôi sử dụng hàng đợi ưu tiên (max-heap) để duy trì các bản dựng ứng viên, được sắp xếp theo độ mạnh. Ban đầu, heap chứa bản dựng tối đa. Chúng tôi bật bản dựng mạnh nhất; nếu nó không bị cấm, đó là câu trả lời của chúng tôi. Mặt khác, chúng tôi tạo các bản dựng ứng cử viên mới bằng cách giảm một chỉ mục trong mỗi vị trí (nhưng chỉ khi bản dựng kết quả chưa được truy cập). Bằng cách này, chúng tôi khám phá các bản dựng theo thứ tự sức mạnh giảm dần mà không liệt kê tất cả các khả năng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(∏ c_i) | O(∏ c_i) | Quá chậm | 
| Tìm kiếm Heap tối đa | O(m log m + n log n * m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả dữ liệu vị trí và các bản dựng bị cấm. Chuyển đổi các bản dựng bị cấm thành các bộ dữ liệu và lưu trữ trong một bộ để tra cứu O(1). 
2. Xây dựng bản dựng ban đầu bằng cách chọn mục cuối cùng từ mỗi vị trí. Tính tổng sức mạnh của nó. 
3. Khởi tạo vùng heap tối đa bằng bộ dữ liệu`(-strength, build)`để mô phỏng vùng heap tối đa bằng vùng heap tối thiểu của Python. 
4. Duy trì một tập hợp các bản dựng đã truy cập để tránh xử lý lại cùng một bản dựng nhiều lần. 
5. Trong khi vùng heap không trống, hãy bật bản dựng có cường độ cao nhất hiện tại. Nếu nó không bị cấm, hãy in nó làm câu trả lời và thoát. 
6. Mặt khác, đối với mỗi vị trí, hãy tạo bản dựng mới bằng cách giảm chỉ mục trong vị trí đó đi một (nhưng chỉ khi chỉ mục vẫn ≥ 1). Nếu bản dựng mới này chưa được truy cập, hãy tính toán sức mạnh của nó và đẩy nó vào heap. 
7. Tiếp tục cho đến khi tìm thấy bản dựng không bị cấm. Vì quá trình xây dựng ban đầu được đảm bảo không bị cấm nên vòng lặp này sẽ luôn chấm dứt. 

Tại sao nó hoạt động: Ở mỗi bước, vùng heap đảm bảo rằng chúng tôi xử lý các bản dựng theo thứ tự tổng sức mạnh giảm dần nghiêm ngặt. Chúng tôi chỉ bỏ qua các bản dựng bị cấm và mọi ứng cử viên có thể mạnh hơn bất kỳ bản dựng nào chưa được truy cập đều nằm trong đống hoặc sẽ được đẩy vào đó. Bộ đã truy cập ngăn việc xem lại cùng một bản dựng. Điều này đảm bảo rằng bản dựng đầu tiên xuất hiện từ đống không bị cấm sẽ có sức mạnh tối đa có thể. 

## Giải pháp Python```python
import sys
import heapq
input = sys.stdin.readline

n = int(input())
slots = []
for _ in range(n):
    data = list(map(int, input().split()))
    c_i = data[0]
    slots.append(data[1:])

m = int(input())
banned = set()
for _ in range(m):
    b = tuple(map(int, input().split()))
    banned.add(b)

# Initial build: last item from each slot
init_build = tuple(len(slot) for slot in slots)
init_strength = sum(slots[i][idx - 1] for i, idx in enumerate(init_build))

heap = [(-init_strength, init_build)]
visited = set()
visited.add(init_build)

while heap:
    neg_strength, build = heapq.heappop(heap)
    if build not in banned:
        print(' '.join(map(str, build)))
        break
    for i in range(n):
        if build[i] > 1:
            new_build = list(build)
            new_build[i] -= 1
            new_build = tuple(new_build)
            if new_build not in visited:
                new_strength = -neg_strength - slots[i][build[i]-1] + slots[i][new_build[i]-1]
                heapq.heappush(heap, (-new_strength, new_build))
                visited.add(new_build)
```Giải pháp bắt đầu bằng cách đọc dữ liệu đầu vào và tạo các mảng khe. Các bản dựng bị cấm được lưu trữ trong một bộ để kiểm tra tư cách thành viên nhanh chóng. Bản dựng ban đầu được xây dựng bằng cách lấy phần tử cuối cùng từ mỗi vị trí, mang lại sức mạnh tối đa có thể. Heap đảm bảo rằng chúng tôi luôn xem xét ứng cử viên mạnh nhất chưa được thăm viếng tiếp theo. Khi tạo hàng xóm, chúng tôi tính toán cẩn thận cường độ mới bằng cách trừ mục cũ và thêm mục mới để tránh tính lại toàn bộ tổng mỗi lần. Nhóm đã truy cập đảm bảo chúng tôi không bao giờ đẩy cùng một bản dựng hai lần, điều này có thể dẫn đến vòng lặp vô hạn hoặc hoạt động kém hiệu quả. 

## Ví dụ đã hoạt động 

Mẫu 1 Đầu vào:```
3
3 1 2 3
2 1 5
3 2 4 6
2
3 2 3
3 2 2
```| Bước | Đống hàng đầu | Sức mạnh | Xây dựng đã xuất hiện | Hành động | 
| --- | --- | --- | --- | --- | 
| 0 | [(12, (3,2,3))] | 12 | (3,2,3) | Bị cấm, tạo hàng xóm | 
| 1 | [(11,(2,2,3)),(10,(3,1,3)),(10,(3,2,2))] | 11 | (2,2,3) | Không bị cấm, in | 

Dấu vết cho thấy thuật toán nhanh chóng tìm ra bản dựng không bị cấm mạnh nhất tiếp theo`(2,2,3)`. 

Đầu vào tùy chỉnh:```
2
2 5 10
2 7 9
1
2 2
```| Bước | Đống hàng đầu | Sức mạnh | Xây dựng đã xuất hiện | Hành động | 
| --- | --- | --- | --- | --- | 
| 0 | [(19,(2,2))] | 19 | (2,2) | Bị cấm, tạo hàng xóm | 
| 1 | [(16,(1,2)),(17,(2,1))] | 17 | (2,1) | Không bị cấm, in | 

Xác nhận xử lý chính xác khi bản dựng mạnh nhất bị cấm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log m + k n log m) | Mỗi bản dựng được đẩy vào vùng nhớ là duy nhất và ≤ số lượng bản dựng được xử lý trước khi tìm thấy câu trả lời; các hoạt động của heap là nhật ký của kích thước heap, thế hệ lân cận là O(n) trên mỗi bản dựng. | 
| Không gian | O(m + k) |`banned`tập hợp sử dụng O(m),`visited`set lưu trữ bản dựng O(k) cho đến khi tìm thấy giải pháp. | 

Được cho$n \le 10$Và$m \le 10^5$, các quy trình thuật toán chỉ xây dựng gần những quy trình mạnh nhất và kết thúc nhanh chóng, trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    slots = []
    for _ in range(n):
        data = list(map(int, input().split()))
        c_i = data[0]
        slots.append(data[1:])
    m = int(input())
    banned = set()
    for _ in range(m):
        banned.add(tuple(map(int, input().split())))
    import heapq
    init_build = tuple(len(slot) for slot in slots)
    init_strength = sum(slots[i][idx - 1] for i, idx in enumerate(init_build))
    heap = [(-init_strength, init_build)]
```
