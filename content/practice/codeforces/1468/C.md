---
title: "CF 1468C - Berpizza"
description: "Chúng tôi xử lý một chuỗi các sự kiện trong một tiệm bánh pizza. Mỗi khi một truy vấn loại 1 xuất hiện, một khách hàng mới sẽ đến. Khách hàng nhận ID liên tiếp bắt đầu từ 1, theo thứ tự hàng đến. Mỗi khách hàng cũng có một giá trị chi tiêu dự đoán m."
date: "2026-06-11T01:25:11+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1468
codeforces_index: "C"
codeforces_contest_name: "2020-2021 ICPC, NERC, Southern and Volga Russian Regional Contest (Online Mirror, ICPC Rules)"
rating: 1400
weight: 1468
solve_time_s: 367
verified: true
draft: false
---

[CF 1468C - Berpizza](https://codeforces.com/problemset/problem/1468/C) 

**Đánh giá:** 1400 
**Tags:** cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 6 phút 7 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi xử lý một chuỗi các sự kiện trong một tiệm bánh pizza. 

Mỗi lần có một truy vấn thuộc loại`1 m`xuất hiện, một khách hàng mới đến. Khách hàng nhận ID liên tiếp bắt đầu từ`1`, theo thứ tự đến. Mỗi khách hàng cũng có một giá trị chi tiêu dự đoán`m`. 

Một truy vấn thuộc loại`2`yêu cầu chúng tôi phục vụ khách hàng sớm nhất chưa được phục vụ. 

Một truy vấn thuộc loại`3`yêu cầu chúng tôi phục vụ khách hàng hiện chưa được phục vụ với mức chi tiêu dự đoán tối đa. Nếu nhiều khách hàng có cùng giá trị chi tiêu thì phải chọn người đến sớm nhất trong số họ. 

Mỗi khách hàng có thể được phục vụ đúng một lần. Đối với mọi truy vấn thuộc loại`2`hoặc`3`, chúng ta phải xuất ID của khách hàng được phục vụ. 

Ràng buộc`q ≤ 5 · 10^5`ngay lập tức loại trừ việc quét tất cả khách hàng đang hoạt động bất cứ khi nào một truy vấn dịch vụ xuất hiện. Trong trường hợp xấu nhất có thể có hàng trăm nghìn khách hàng cùng lúc chờ đợi. MỘT`O(q)`quét mỗi thao tác sẽ dẫn đến khoảng`O(q²)`công việc, nó quá lớn. 

Chúng tôi cần cấu trúc dữ liệu có thể trả về cả hai: 

1. Khách hàng chưa được phục vụ sớm nhất. 
2. Khách hàng chưa được phục vụ có mức độ ưu tiên cao nhất. 

Mỗi thao tác nên chạy theo thời gian logarit hoặc tốt hơn. 

Một vấn đề tế nhị là cùng một khách hàng có thể được người phục vụ lựa chọn. Sau khi được phục vụ, khách hàng đó phải biến mất khỏi cả hai hệ thống lựa chọn. Việc quên điều này sẽ tạo ra các câu trả lời trùng lặp. 

Coi như:```
1 10
1 20
3
2
```Khách hàng`2`được phục vụ đầu tiên bởi Polycarp. Câu trả lời cho truy vấn thứ hai phải là khách hàng`1`, không phải khách hàng`2`lại. 

Một trường hợp tinh vi khác xuất hiện khi nhiều khách hàng có mức chi tiêu như nhau.```
1 10
1 10
3
```Câu trả lời phải là khách hàng`1`, bởi vì mối quan hệ bị phá vỡ bởi thứ tự đến. 

## Phương pháp tiếp cận 

Giải pháp brute-force lưu trữ tất cả khách hàng đang hoạt động vào một danh sách. 

Đối với loại truy vấn`2`, nó sẽ quét về phía trước cho đến khi tìm thấy khách hàng chưa được phục vụ đầu tiên. 

Đối với loại truy vấn`3`, nó quét tất cả các khách hàng đang hoạt động để tìm ra giá trị chi tiêu lớn nhất rồi áp dụng quy tắc tie-break. 

Cách tiếp cận này đúng vì nó tuân theo định nghĩa vấn đề. Thật không may, một truy vấn có thể yêu cầu kiểm tra`O(q)`khách hàng. Với tối đa`5 · 10^5`hoạt động, độ phức tạp trong trường hợp xấu nhất trở thành`O(q²)`. 

Quan sát quan trọng là hai người phục vụ sử dụng hai cách gọi món khác nhau. 

Monocarp luôn muốn chỉ số đến nhỏ nhất. 

Polycarp luôn muốn giá trị chi tiêu lớn nhất, với đơn hàng đến là chìa khóa phụ. 

Thay vì duy trì một cấu trúc phức tạp, chúng tôi duy trì hai quan điểm độc lập về cùng một khách hàng. 

Cấu trúc giống như hàng đợi cho phép truy cập vào điểm đến sớm nhất. 

Hàng đợi ưu tiên cho phép truy cập vào giá trị chi tiêu lớn nhất. 

Thử thách còn lại là sự đồng bộ hóa. Khi một người phục vụ phục vụ một khách hàng, khách hàng đó vẫn tồn tại bên trong cơ cấu kia. Việc loại bỏ các phần tử tùy ý khỏi đống hoặc hàng đợi là rất tốn kém. 

Giải pháp tiêu chuẩn là xóa lười. Chúng tôi giữ một mảng boolean cho biết liệu khách hàng đã được phục vụ chưa. Bất cứ khi nào một cấu trúc trả về một khách hàng, chúng tôi liên tục loại bỏ các mục nhập đã được đánh dấu là đã phục vụ. 

Điều này biến cả hai hoạt động thành các truy vấn thời gian logarit hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Lực lượng vũ phu | O(q²) | O(q) | Quá chậm | 
| Hai cấu trúc + xóa lười | O(q log q) | O(q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chỉ định cho mỗi khách hàng đến một ID duy nhất. 

2. Lưu trữ từng khách hàng đến theo thứ tự đến bên trong cấu trúc mảng hoặc hàng đợi. 

3. Đồng thời chèn khách hàng vào vùng heap tối đa được khóa bởi`(spending, arrival order)`. 

4. Duy trì mảng boolean`served[id]`. 

5. Đối với loại truy vấn`2`, chuyển con trỏ qua đơn hàng đến cho đến khi nó đến được một khách hàng chưa được phục vụ. Xuất ID đó và đánh dấu nó được phục vụ. 

6. Đối với loại truy vấn`3`, liên tục xóa các mục heap có khách hàng đã được phục vụ. Mục còn lại đầu tiên là câu trả lời hợp lệ. Xuất ID của nó và đánh dấu nó được phục vụ. 

7. Tiếp tục xử lý tất cả các truy vấn. 

### Tại sao nó hoạt động 

Con trỏ đến luôn di chuyển từ trái sang phải và bỏ qua chính xác những khách hàng đã được đánh dấu là đã phục vụ. Do đó, khách hàng đầu tiên chưa được phục vụ mà nó tiếp cận chính xác là khách hàng mà Monocarp sẽ chọn. 

Heap luôn chứa mọi khách hàng đã từng đến. Tính năng xóa lười chỉ xóa các mục lỗi thời khi chúng đạt đến đỉnh. Sau khi tất cả các khách hàng được phục vụ ở trên cùng bị loại bỏ, phần tử hàng đầu còn lại là khách hàng chưa được phục vụ với mức chi tiêu tối đa. Vì ID đến được bao gồm trong khóa heap nên các mối quan hệ được giải quyết đối với những lần đến trước đó một cách chính xác theo yêu cầu. 

Vì mỗi khách hàng được đánh dấu là phục vụ tối đa một lần nên không người phục vụ nào có thể phục vụ cùng một khách hàng hai lần. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    q = int(input())

    arrivals = []
    served = [False] * (q + 5)

    heap = []

    next_id = 1
    front = 0

    ans = []

    for _ in range(q):
        parts = input().split()

        if parts[0] == '1':
            m = int(parts[1])

            arrivals.append(next_id)

            heapq.heappush(heap, (-m, next_id))

            next_id += 1

        elif parts[0] == '2':
            while served[arrivals[front]]:
                front += 1

            cid = arrivals[front]
            served[cid] = True
            ans.append(str(cid))

        else:
            while heap and served[heap[0][1]]:
                heapq.heappop(heap)

            _, cid = heapq.heappop(heap)

            served[cid] = True
            ans.append(str(cid))

    print(" ".join(ans))

solve()
```các`arrivals`mảng lưu trữ khách hàng theo đúng thứ tự họ vào nhà hàng. Biến`front`hoạt động giống như người đứng đầu hàng đợi. 

Heap lưu trữ các cặp`(-money, id)`. Python cung cấp một vùng heap tối thiểu, do đó việc phủ định giá trị chi tiêu sẽ biến nó thành vùng heap tối đa. Việc sử dụng ID khách hàng làm thành phần thứ hai sẽ tự động thực thi quy tắc ràng buộc bắt buộc vì ID nhỏ hơn tương ứng với những người đến sớm hơn. 

các`served`mảng là chìa khóa để xóa lười biếng. Bất cứ khi nào một khách hàng được phục vụ bởi một người phục vụ, chúng tôi sẽ đánh dấu khách hàng đó. Cấu trúc dữ liệu khác cuối cùng sẽ gặp phải bản ghi cũ và loại bỏ nó. 

Không có khách hàng nào bị loại bỏ khỏi phần giữa của cả hai cấu trúc, điều này giúp mọi hoạt động luôn hiệu quả. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu```
8
1 8
1 10
1 6
3
2
1 9
2
3
```| Bước | Truy vấn | Xếp hàng đợi | đống tốt nhất | Đầu ra | 
|---|---|---|---|---| 
| 1 | 1 8 | 1 | 1 | | 
| 2 | 1 10 | 1,2 | 2 | | 
| 3 | 1 6 | 1,2,3 | 2 | | 
| 4 | 3 | 1,2,3 | 2 | 2 | 
| 5 | 2 | 1,2,3 | 1 | 1 | 
| 6 | 1 9 | 1,2,3,4 | 4 | | 
| 7 | 2 | 1,2,3,4 | 4 | 3 | 
| 8 | 3 | 1,2,3,4 | 4 | 4 | 

Các kết quả đầu ra là:```
2 1 3 4
```Ví dụ này thể hiện việc xóa lười biếng. Sau khi khách hàng`2`được phục vụ bởi Polycarp, khách hàng`2`vẫn còn bên trong cấu trúc đến. Điểm đánh dấu đã phục vụ sẽ ngăn không cho bản ghi cũ đó được chọn lại. 

### Ví dụ về Tie-Break```
1 10
1 10
3
```| Khách hàng | Chi tiêu | Đến | 
|---|---|---| 
| 1 | 10 | Đầu tiên | 
| 2 | 10 | Thứ hai | 

Cả hai giá trị chi tiêu đều bằng nhau. Vùng heap so sánh các ID tiếp theo, vì vậy khách hàng`1`được chọn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(q log q) | Mỗi lần chèn và loại bỏ đống chi phí O(log q) | 
| Không gian | O(q) | Tất cả khách hàng được lưu trữ một lần | 

Mỗi khách hàng vào heap đúng một lần và rời khỏi heap nhiều nhất một lần. Con trỏ đến chỉ di chuyển về phía trước. Tổng công việc vừa vặn thoải mái trong giới hạn cho`5 · 10^5`hoạt động. 

## Trường hợp thử nghiệm```python
# sample

# input:
# 8
# 1 8
# 1 10
# 1 6
# 3
# 2
# 1 9
# 2
# 3
#
# output:
# 2 1 3 4

# earliest customer after highest-value service
# expected: 2 1

# equal spending values
# 1 10
# 1 10
# 3
# expected: 1

# repeated lazy deletions
# 1 5
# 1 10
# 1 7
# 3
# 3
# 2
# expected: 2 3 1

# single customer
# 1 100
# 2
# expected: 1
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| Trường hợp mẫu | 2 1 3 4 | Hoạt động hỗn hợp bình thường | 
| Ưu tiên như nhau | 1 | Hòa khi đến | 
| Nhiều truy vấn loại 3 | 2 3 1 | Sắp xếp thứ tự đống chính xác | 
| Khách hàng đơn lẻ | 1 | Trường hợp không cần thiết tối thiểu | 

## Vỏ cạnh 

Hãy xem xét:```
1 10
1 20
3
2
```Khách hàng`2`được phục vụ đầu tiên. Khi Monocarp phục vụ tiếp theo, con trỏ đến sẽ bỏ qua khách hàng`2`bởi vì`served[2] = True`và trả lại khách hàng một cách chính xác`1`. 

Coi như:```
1 10
1 10
3
```Cả hai khách hàng đều có mức chi tiêu bằng nhau. Các cửa hàng đống`(-10, 1)`Và`(-10, 2)`. So sánh bộ dữ liệu từ điển làm cho khách hàng`1`người chiến thắng. 

Coi như:```
1 5
1 8
2
3
```Khách hàng`1`được phục vụ bởi Monocarp. Heap vẫn chứa khách hàng`1`, nhưng việc xóa lười sẽ xóa mục nhập cũ đó trước khi chọn khách hàng`2`. Điều này đảm bảo rằng không có khách hàng nào được phục vụ hai lần. 
:::
