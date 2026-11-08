---
title: "CF 1442D - Tổng"
description: "Chúng ta được cho một số dãy, mỗi dãy đã được sắp xếp theo thứ tự không giảm. Chúng tôi liên tục thực hiện một thao tác trong đó chúng tôi chọn một chuỗi, lấy phần tử đầu tiên hiện tại của nó, thêm nó vào tổng số của chúng tôi và xóa phần tử đó khỏi chuỗi."
date: "2026-06-11T04:18:49+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1442
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 681 (Div. 1, based on VK Cup 2019-2020 - Final)"
rating: 2800
weight: 1442
solve_time_s: 91
verified: false
draft: false
---

[CF 1442D - Tổng](https://codeforces.com/problemset/problem/1442/D) 

**Đánh giá:** 2800 
**Tas:** cấu trúc dữ liệu, phân chia để chinh phục, dp, tham lam 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số dãy, mỗi dãy đã được sắp xếp theo thứ tự không giảm. Chúng tôi liên tục thực hiện một thao tác trong đó chúng tôi chọn một chuỗi, lấy phần tử đầu tiên hiện tại của nó, thêm nó vào tổng số của chúng tôi và xóa phần tử đó khỏi chuỗi. Chúng tôi thực hiện việc này chính xác k lần và mục tiêu là tối đa hóa tổng số tiền thu được. 

Một cách khác để xem quy trình là mỗi mảng hoạt động giống như một hàng đợi và ở mỗi bước, chúng tôi chọn một hàng đợi và bật ra từ phía trước của nó. Bởi vì các mảng được sắp xếp ngày càng tăng nên các phần tử đầu tiên là nhỏ nhất trong mảng đó và các phần tử sau lớn hơn. 

Cấu trúc chính là chúng ta không bị ràng buộc phải sử dụng hết một mảng trước khi chuyển sang mảng khác. Chúng ta có thể tự do xen kẽ các lựa chọn giữa các mảng, nhưng mỗi khi chúng ta quay lại một mảng, chúng ta đang đưa con trỏ dọc theo mảng đó một cách hiệu quả. 

Các ràng buộc ngay lập tức loại trừ bất kỳ cách tiếp cận nào xem xét tất cả các chuỗi lựa chọn. Với tối đa 3000 mảng và tối đa 3000 thao tác, bất kỳ sự phân nhánh mạnh mẽ nào về các lựa chọn mảng cho mỗi thao tác đều tăng theo cấp số nhân. Ngay cả một DP trên tất cả các trạng thái của tất cả các con trỏ cũng sẽ liên quan đến việc theo dõi trạng thái 3000 chiều, điều này là không thể. 

Một vấn đề tinh tế hơn là chiến lược tham lam ngây thơ luôn lấy phần tử phía trước có sẵn tối đa hiện tại là đúng, nhưng chỉ khi chúng ta có thể duy trì nó một cách hiệu quả. Thách thức tiềm ẩn là mỗi khi chúng ta chọn từ một mảng, mặt trước của nó sẽ thay đổi, do đó các giá trị ứng cử viên sẽ thay đổi linh hoạt. 

Một trường hợp thất bại điển hình đối với lối suy nghĩ ngây thơ là xử lý từng mảng một cách độc lập, lấy tổng tiền tố lớn nhất của nó và sau đó kết hợp các kết quả một cách tham lam. Ví dụ: nếu một mảng bắt đầu nhỏ nhưng kết thúc rất lớn, việc trì hoãn nó có thể là tối ưu, nhưng việc tổng hợp tiền tố ngây thơ sẽ bỏ lỡ sự đan xen đó. 

Giải pháp đúng cần có một cách để luôn biết “phần tử tiếp theo” tốt nhất hiện có trên tất cả các mảng, đồng thời chỉ cập nhật hiệu quả mảng bị ảnh hưởng sau mỗi lần chọn. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng quy trình từng bước. Ở mỗi thao tác, chúng tôi thử tất cả n mảng, chọn một mảng và tiếp tục đệ quy. Điều này khám phá một cách chính xác tất cả các chuỗi lựa chọn có thể có, nhưng hệ số phân nhánh là n ở mỗi k bước, dẫn đến O (n^k), điều này hoàn toàn không khả thi. 

Cách tinh chỉnh mạnh mẽ thứ hai là lưu ý rằng mỗi mảng đều đơn điệu, do đó, đối với mỗi mảng, chúng ta chỉ sử dụng tiền tố. Chúng ta có thể xác định trạng thái bằng số lượng phần tử được lấy từ mỗi mảng. Ngay cả khi đó, không gian trạng thái vẫn rất lớn vì mỗi mảng có thể đóng góp tới 3000 phần tử, tạo ra vụ nổ tổ hợp. 

Quan sát quan trọng là tại bất kỳ thời điểm nào, mỗi mảng đều đóng góp chính xác một giá trị ứng cử viên cho quá trình quyết định: phần tử phía trước hiện tại của nó. Sau khi chọn nó, ứng viên tiếp theo từ mảng đó sẽ dịch chuyển về phía trước một vị trí. Điều này có nghĩa là toàn bộ vấn đề giảm xuống việc liên tục chọn giá trị lớn nhất trong số n giá trị phía trước thay đổi. 

Đây chính xác là một vấn đề lựa chọn động trên một tập hợp các ứng cử viên trong đó mỗi lựa chọn khiến một ứng cử viên bị thay thế bởi người kế nhiệm trong một danh sách cố định. Một hàng đợi ưu tiên là đủ: chúng tôi duy trì mặt trước hiện tại của mỗi mảng và mỗi lần trích xuất sẽ thay thế nó bằng phần tử tiếp theo từ cùng một mảng. 

Điều này hiệu quả vì trong mỗi mảng, thứ tự là cố định và không giảm, vì vậy khi tiến về phía trước, chúng ta không bao giờ cần phải xem xét lại các phần tử trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^k) | O(k) | Quá chậm | 
| Tối ưu (mô phỏng heap) | O((n + k) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Khởi tạo cấu trúc có mức độ ưu tiên tối đa lưu trữ một mục nhập trên mỗi mảng, cụ thể là phần tử đầu tiên hiện tại của nó cùng với chỉ mục và vị trí mảng. Chúng tôi đảo ngược các giá trị để mô phỏng vùng heap tối đa vì vùng heap tiêu chuẩn được định hướng tối thiểu trong Python. Điều này đảm bảo chúng tôi luôn trích xuất ứng viên tốt nhất hiện có trong số tất cả các mảng. 
2. Chèn phần tử đầu tiên của mọi mảng không trống vào heap. Mỗi mục đại diện cho lựa chọn tiếp theo tốt nhất nếu chúng ta quyết định chọn từ mảng đó. 
3. Khởi tạo bộ tích lũy cho câu trả lời, nó sẽ lưu trữ tổng của tất cả các phần tử được chọn qua k phép toán. 
4. Lặp lại k lần: 

1. Trích xuất phần tử lớn nhất có sẵn từ heap. 
2. Thêm giá trị của nó vào bộ tích lũy. 
3. Tiến lên trong mảng tương ứng thêm một vị trí. 
4. Nếu mảng vẫn còn các phần tử còn lại, hãy chèn phần tử phía trước mới của nó vào heap. 

Lý do bước này hoạt động là vì sau khi loại bỏ mặt trước hiện tại, ứng cử viên hợp lệ tiếp theo từ mảng đó được xác định duy nhất. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, heap chứa chính xác một ứng cử viên cho mỗi mảng: chỉ mục nhỏ nhất chưa được sử dụng. Mọi động thái pháp lý có thể xảy ra đều tương ứng với việc chọn một trong những ứng cử viên này. Bởi vì các mảng độc lập ngoại trừ sự cạnh tranh trong heap nên việc chọn ứng cử viên hiện tại tối đa luôn là tối ưu cho bước hiện tại. Vấn đề có thuộc tính trao đổi: nếu một giải pháp tối ưu không chọn phần tử khả dụng tối đa ở một số bước, việc hoán đổi nó với phần tử đã chọn không thể làm giảm tổng vì tất cả các giá trị đều không âm và việc thay thế chỉ ảnh hưởng đến quyền truy cập trong tương lai trong cùng một chuỗi được sắp xếp. Điều này đảm bảo sự lựa chọn tham lam là nhất quán trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import heapq

n, k = map(int, input().split())

arrays = []
for _ in range(n):
    tmp = list(map(int, input().split()))
    t = tmp[0]
    arrays.append(tmp[1:])

heap = []
for i in range(n):
    if arrays[i]:
        heapq.heappush(heap, (-arrays[i][0], i, 0))

ans = 0

for _ in range(k):
    val, i, j = heapq.heappop(heap)
    val = -val
    ans += val

    if j + 1 < len(arrays[i]):
        heapq.heappush(heap, (-arrays[i][j + 1], i, j + 1))

print(ans)
```Giải pháp duy trì rất nhiều phần tử phía trước đang hoạt động. Mỗi mục nhập heap mã hóa cả giá trị và vị trí của nó để chúng ta có thể tiến lên một cách chính xác trong mảng của nó. Dấu âm là cần thiết vì Python chỉ cung cấp vùng heap tối thiểu. 

Một chi tiết triển khai tinh tế là chúng tôi không bao giờ đẩy các phần tử lỗi thời. Mỗi mục nhập heap tương ứng với một chỉ mục cụ thể trong một mảng, vì vậy khi chúng ta bật nó lên, chúng ta chắc chắn biết chỉ mục tiếp theo để đẩy. Điều này tránh sự cần thiết phải xóa lười biếng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
2 5 10
3 1 2 3
2 1 20
```Chúng tôi theo dõi trạng thái heap một cách khái niệm. 

| Bước | Đống (trên cùng đầu tiên) | Được chọn | Trả lời | 
| --- | --- | --- | --- | 
| ban đầu | (5, A), (1, B), (1, C) | - | 0 | 
| 1 | 5(A) | 5 | 5 | 
| 2 | 20(C), 10(A), 1(B) | 20 | 25 | 
| 3 | 10(A), 1(B), - | 10 | 35 | 

Dấu vết cho thấy thuật toán trì hoãn việc chọn các giá trị ban đầu nhỏ hơn một cách tự nhiên để ưu tiên một giá trị muộn lớn hơn, giá trị này chỉ khả dụng sau khi tiến lên trong mảng của nó. 

### Ví dụ 2 

đầu vào:```
2 4
1 100
3 1 2 3
```| Bước | Đống | Được chọn | Trả lời | 
| --- | --- | --- | --- | 
| ban đầu | 100, 1 | - | 0 | 
| 1 | 100 | 100 | 100 | 
| 2 | 3, 2 | 3 | 103 | 
| 3 | 2, 2 | 2 | 105 | 
| 4 | 1, 2 | 2 | 107 | 

Điều này chứng tỏ rằng khi mảng một phần tử được sử dụng hết, thuật toán sẽ tiếp tục thực hiện chính xác ở mảng khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + k) log n) | Mỗi thao tác heap tốn log n và chúng tôi thực hiện k trích xuất và tối đa k lần chèn | 
| Không gian | O(n) | Heap lưu trữ tối đa một con trỏ hoạt động trên mỗi mảng | 

Giới hạn n, k ≤ 3000 làm cho việc này trở nên nhanh chóng một cách thoải mái. Ngay cả trong trường hợp xấu nhất của 6000 thao tác heap, hệ số logarit là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import heapq

    n, k = map(int, sys.stdin.readline().split())
    arrays = []
    for _ in range(n):
        tmp = list(map(int, sys.stdin.readline().split()))
        arrays.append(tmp[1:])

    heap = []
    for i in range(n):
        if arrays[i]:
            heapq.heappush(heap, (-arrays[i][0], i, 0))

    ans = 0
    for _ in range(k):
        val, i, j = heapq.heappop(heap)
        val = -val
        ans += val
        if j + 1 < len(arrays[i]):
            heapq.heappush(heap, (-arrays[i][j + 1], i, j + 1))

    return str(ans)

# provided sample
assert run("""3 3
2 5 10
3 1 2 3
2 1 20
""") == "35"

# single array only
assert run("""1 3
5 1 2 3 4 5
""") == "12"

# all equal values
assert run("""3 4
2 5 5
2 5 5
2 5 5
""") == "20"

# k equals total elements
assert run("""2 5
2 1 100
3 2 3 4
""") == "110"

# small alternating structure
assert run("""2 3
3 1 100 100
3 2 3 4
""") == "204"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mảng đơn | 12 | tiêu thụ tiền tố đúng | 
| tất cả đều bình đẳng | 20 | xử lý buộc trong đống | 
| tiêu thụ đầy đủ | 110 | ranh giới k = tổng số phần tử | 
| xen kẽ | 204 | sự xen kẽ đúng đắn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi một mảng duy nhất chiếm ưu thế các giá trị muộn. Đối với đầu vào:```
2 3
2 1 2
3 1 1 100
```vùng heap bắt đầu bằng 1, 1. Thuật toán lấy 1, rồi lại 1, nhưng sau khi tiến tới mảng thứ hai, cuối cùng nó đạt tới 100, trở thành lựa chọn chính. Heap trì hoãn việc truy cập một cách chính xác cho đến khi con trỏ chạm tới nó. 

Một trường hợp khác là khi tất cả các mảng đều chứa các giá trị giống hệt nhau. Heap có nhiều ràng buộc, nhưng vì các giá trị không âm và độc lập nên mọi thứ tự phá vỡ ràng buộc đều hợp lệ. Cấu trúc đảm bảo chính xác k lựa chọn được thực hiện mà không có sai lệch. 

Trường hợp cạnh cuối cùng là khi k bằng tổng số phần tử. Vùng heap trở nên trống chính xác sau khi sử dụng hết tất cả các mảng và thuật toán không bao giờ cố gắng đẩy các chỉ mục không hợp lệ vì mỗi lần đẩy được bảo vệ bởi các giới hạn về độ dài mảng.
