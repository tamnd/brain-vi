---
problem: 982B
contest_id: 982
problem_index: B
name: "Bus of Characters"
contest_name: "Codeforces Round 484 (Div. 2)"
rating: 1300
tags: ["data structures", "greedy", "implementation"]
answer: passed_samples
verified: true
solve_time_s: 64
date: 2026-06-18
model: gpt-5-5
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a337532-184c-83ec-bd9a-f66c9654df9d
---

#CF 982B - Xe nhân vật 

**Đánh giá:** 1300 
**Tags:** cấu trúc dữ liệu, tham lam, triển khai 
**Mẫu:** gpt-5-5 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a337532-184c-83ec-bd9a-f66c9654df9d 

--- 

## Giải pháp 

## Hiểu vấn đề 

Xe buýt gồm có$n$các hàng độc lập, mỗi hàng có đúng hai ghế. Mỗi hàng có chiều rộng cố định$w_i$và tất cả các chiều rộng đều khác nhau, vì vậy chúng ta luôn có thể so sánh các hàng mà không có sự mơ hồ. 

Hành khách đến từng người một theo một thứ tự cố định. Có hai loại hành vi. Một người hướng nội luôn nhất quyết ngồi ở một hàng hoàn toàn trống và trong số tất cả những hàng như vậy hãy chọn hàng có chiều rộng nhỏ nhất. Người hướng ngoại chỉ có thể ngồi ở hàng đã có đúng một chỗ ngồi và trong số những hàng đã được lấp đầy một phần đó, hãy chọn hàng có chiều rộng lớn nhất. 

Nhiệm vụ là mô phỏng quá trình và đầu ra này cho mọi hành khách theo thứ tự đến, họ sẽ ngồi ở hàng nào. 

Các ràng buộc đi lên đến$n = 200{,}000$, do đó việc mô phỏng bao gồm tới$2n = 400{,}000$hoạt động. Bất kỳ giải pháp nào quét tất cả các hàng cho mỗi hành khách sẽ hoạt động như thế nào$O(n^2)$, vượt xa giới hạn khả thi. Điều này ngay lập tức buộc chúng ta phải duy trì các tập hợp hàng ứng cử viên có thứ tự để mỗi quyết định có thể được đưa ra theo thời gian logarit. 

Một vấn đề tế nhị xuất hiện trong sự tương tác giữa hai nhóm. Người hướng nội loại bỏ các hàng khỏi “bộ trống” và chuyển chúng thành “bộ đầy một nửa”. Người hướng ngoại tiêu dùng từ bộ đồ ăn đầy một nửa đó. Việc triển khai đơn giản tính toán lại các bộ này bằng cách quét tất cả các hàng sẽ thất bại không phải vì tính chính xác mà vì nó sẽ hết thời gian chờ rất lâu trước khi kết thúc. 

Các trường hợp cạnh phát sinh khi hàng trống có chiều rộng nhỏ nhất và hàng được lấp đầy một nửa có chiều rộng lớn nhất phát triển theo các cách xen kẽ. Ví dụ: nếu chiều rộng là$[1,2,3]$và thứ tự là$010101$, cấu trúc của các bộ có sẵn sẽ thay đổi sau mỗi lần di chuyển và tính chính xác phụ thuộc vào việc luôn chọn từ biên giới động chính xác thay vì tính toán lại từ đầu. 

## Phương pháp tiếp cận 

Đối với mỗi hành khách, một mô phỏng lực lượng vũ phu sẽ quét tất cả các hàng và kiểm tra xem chúng trống hay có một nửa, sau đó áp dụng quy tắc lựa chọn. Đối với người hướng nội, điều này có nghĩa là xem xét tất cả$n$các hàng để tìm chiều rộng tối thiểu trong số các hàng trống và để người hướng ngoại quét lại để tìm mức tối đa trong số các hàng được lấp đầy một nửa. Mỗi hành khách chi phí$O(n)$, dẫn đến$O(n^2)$tổng số hoạt động, khoảng$1.6 \times 10^{11}$tệ nhất là kiểm tra, điều này là không khả thi. 

Quan sát quan trọng là cả hai lựa chọn đều phụ thuộc vào việc duy trì cấu trúc có trật tự thay đổi linh hoạt. Người hướng nội luôn cần số lượng tối thiểu trong số các hàng trống có sẵn và người hướng ngoại luôn cần số lượng tối đa trong số các hàng trống một phần. Đây chính xác là mục đích của hàng đợi ưu tiên (đống) hoặc tập hợp thứ tự cân bằng. Nếu chúng ta lưu trữ các hàng trống trong vùng heap tối thiểu được khóa theo chiều rộng và các hàng được lấp đầy một nửa trong vùng heap tối đa được khóa theo chiều rộng thì mỗi thao tác sẽ trở thành một lần đẩy hoặc bật theo thời gian logarit. 

Chúng ta cũng cần nhớ chiều rộng cho trước tương ứng với hàng nào để khi xuất các phép gán, chúng ta có thể ánh xạ trở lại các chỉ mục hàng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Tối ưu (đống) |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai cấu trúc ưu tiên. Một cửa hàng tất cả các hàng hiện đang trống được sắp xếp theo chiều rộng tăng dần. Các hàng còn lại lưu trữ tất cả các hàng đã có một hành khách, được sắp xếp theo chiều rộng giảm dần. 

Chúng tôi cũng duy trì ánh xạ từ chiều rộng đến chỉ mục hàng, vì việc lựa chọn dựa trên chiều rộng nhưng đầu ra yêu cầu số hàng. 

### bước 

1. Khởi tạo ánh xạ từ mỗi chiều rộng$w_i$vào chỉ mục hàng của nó$i$. Điều này đảm bảo chúng ta có thể chuyển đổi chiều rộng đã chọn trở lại hàng đầu ra chính xác. 
2. Chèn tất cả các hàng vào một đống hàng trống tối thiểu được khóa theo chiều rộng. Khi bắt đầu, mỗi hàng đều trống. 
3. Duy trì vùng heap tối đa trống cho các hàng đã chiếm một nửa. Đống này sẽ lưu trữ các hàng sau khi một người hướng nội ngồi vào ghế đầu tiên. 
4. Xử lý hành khách theo thứ tự từ chuỗi đầu vào. 
5. Nếu hành khách hiện tại là người hướng nội, hãy trích xuất hàng có chiều rộng nhỏ nhất từ ​​đống trống. Chỉ định hàng này cho hành khách, sau đó đẩy nó vào đống đã chiếm một nửa. Lý do là hàng này không còn trống nữa và giờ đây sẽ đủ điều kiện dành cho người hướng ngoại. 
6. Nếu hành khách hiện tại là người hướng ngoại, hãy trích xuất hàng có chiều rộng lớn nhất từ ​​đống chiếm một nửa. Chỉ định hàng này cho hành khách và loại bỏ nó hoàn toàn vì cả hai ghế hiện đã được lấp đầy. Quy tắc lựa chọn đảm bảo tính đúng đắn vì người hướng ngoại luôn thích chiều rộng lớn nhất trong số các hàng được lấp đầy một phần. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, mỗi hàng thuộc về chính xác một trong ba trạng thái: hoàn toàn không được sử dụng, được lấp đầy một phần hoặc được lấp đầy đầy đủ. Quy tắc hướng nội chỉ tương tác với các hàng không được sử dụng và quy tắc hướng ngoại chỉ tương tác với các hàng được lấp đầy một phần. Các đống thực thi rằng ứng cử viên tốt nhất theo từng quy tắc luôn có sẵn trong$O(1)$thời gian truy cập và có thể tháo rời trong$O(\log n)$. Vì các hàng không bao giờ di chuyển lùi giữa các trạng thái nên cấu trúc vẫn nhất quán trong suốt quá trình, duy trì tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    w = list(map(int, input().split()))
    s = input().strip()

    import heapq

    # min-heap for empty rows: (width, row_index)
    empty = []
    # max-heap for half-occupied rows: (-width, row_index)
    half = []

    for i, wi in enumerate(w, 1):
        heapq.heappush(empty, (wi, i))

    res = []

    for c in s:
        if c == '0':
            wi, idx = heapq.heappop(empty)
            res.append(idx)
            heapq.heappush(half, (-wi, idx))
        else:
            wi_neg, idx = heapq.heappop(half)
            res.append(idx)

    print(*res)

if __name__ == "__main__":
    solve()
```Đống trống lưu trữ tất cả các hàng không sử dụng được sắp xếp theo chiều rộng để người hướng nội luôn có thể chọn hàng nhỏ nhất hiện có. Sau khi một hàng được chọn, nó sẽ được chuyển vào vùng chứa một nửa với chiều rộng bị phủ định để vùng heap tối thiểu của Python hoạt động giống như vùng heap tối đa. 

Đống được lấp đầy một nửa đảm bảo người hướng ngoại luôn lấy hàng có chiều rộng tối đa trong số những hàng có sẵn. Khi một người hướng ngoại ngồi vào chỗ, hàng ghế đó sẽ được lấp đầy và bị loại bỏ hoàn toàn. 

Chi tiết triển khai chính là các hàng không bao giờ được chèn lại vào vùng nhớ trống và mỗi hàng sẽ di chuyển chính xác một lần vào vùng nhớ đã được lấp đầy một nửa, đảm bảo chuyển đổi trạng thái chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 2
w = [3, 1]
s = 0011
```Chúng tôi theo dõi cả hai đống. 

| Bước | Hành khách | Đống trống | Nửa đống | Hàng đã chọn | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | [(1,2),(3,1)] | [] | 2 | 
| 2 | 0 | [(3,1)] | [(1,2)] | 1 | 
| 3 | 1 | [(3,1)] | [(1,2)] | 1 | 
| 4 | 1 | [(3,1)] | [] | 2 | 

Người hướng nội đầu tiên ngồi ở hàng 2 vì nó có chiều rộng là 1, nhỏ nhất. Người hướng nội thứ hai ngồi ở hàng 1. Người hướng ngoại đầu tiên ngồi ở hàng 1 vì đây là hàng duy nhất được lấp đầy một phần. Người hướng ngoại cuối cùng ngồi ở hàng thứ 2. 

### Ví dụ 2 

đầu vào:```
n = 3
w = [5, 2, 4]
s = 010110
```| Bước | Hành khách | Đống trống | Nửa đống | Hàng đã chọn | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | [(2,2),(5,1),(4,3)] | [] | 2 | 
| 2 | 1 | [(4,3),(5,1)] | [(2,2)] | 2 | 
| 3 | 0 | [(4,3),(5,1)] | [(2,2)] | 3 | 
| 4 | 1 | [(5,1)] | [(2,2),(4,3)] | 3 | 
| 5 | 1 | [(5,1)] | [(2,2)] | 3 | 
| 6 | 0 | [] | [(2,2)] | 1 | 

Những dấu vết này cho thấy rằng mỗi vùng nhớ heap duy trì độc lập cực trị chính xác cho trạng thái của nó và các quá trình chuyển đổi duy trì trật tự mà không cần quét lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi hàng được đẩy và xuất hiện nhiều nhất một lần trong mỗi lần chuyển đổi heap | 
| Không gian |$O(n)$| Hai đống lưu trữ tối đa tất cả các hàng | 

Chi phí logarit đủ nhỏ để$2 \cdot 10^5$hàng và tổng số hoạt động vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("2\n3 1\n0011\n") == "2 1 1 2"

# minimum size
assert run("1\n10\n01\n") == "1 1"

# already sorted small
assert run("3\n1 2 3\n000111\n") == "1 2 3 3 2 1"

# reverse order
assert run("3\n3 2 1\n000111\n") == "3 2 1 1 2 3"

# alternating pattern
assert run("2\n5 10\n0101\n") == "1 1 2 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hàng đơn | 1 1 | xử lý ranh giới tối thiểu | 
| tăng chiều rộng | 1 2 3 3 2 1 | chuyển đổi thứ tự đúng | 
| chiều rộng giảm dần | 3 2 1 1 2 3 | tính đúng đắn đối xứng | 
| mô hình xen kẽ | 1 1 2 2 | cập nhật đống xen kẽ | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi hàng trống nhỏ nhất trở thành hàng lớn nhất trong số các hàng được điền một nửa sau này trong chuỗi. Vì độ rộng là khác nhau và chuyển đổi trạng thái là một chiều nên thuật toán không cần phải điều hòa xung đột giữa các vùng nhớ; khi một hàng rời khỏi đống trống, nó sẽ không bao giờ cạnh tranh ở đó nữa. 

Đối với đầu vào:```
n = 2
w = [2, 1]
s = 0101
```Từng bước một: 

Người hướng nội chọn hàng 2 trước (chiều rộng 1), sau đó người hướng ngoại phải chọn hàng 2 hoặc 1 tùy theo nửa đống. Các đống đảm bảo hàng 2 nằm chính xác trong cấu trúc được lấp đầy một nửa, do đó hành vi hướng ngoại vẫn nhất quán. 

Điều này xác nhận rằng việc tách trạng thái chứ không phải tính toán lại là yếu tố duy trì tính chính xác trong suốt quá trình mô phỏng đầy đủ.
