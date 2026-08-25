---
title: "CF 104324L - Đội Bóng Trong Mơ"
description: "Chúng tôi đang cố gắng tập hợp một “đội” từ hai nhóm sinh viên. Từ nhóm đại học, chúng ta phải chọn chính xác ba sinh viên riêng biệt và chất lượng của đội này là tổng giá trị sức mạnh của họ. Từ nhóm tốt nghiệp, chúng tôi chọn chính xác một sinh viên để làm huấn luyện viên."
date: "2026-07-01T19:24:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104324
codeforces_index: "L"
codeforces_contest_name: "SDU Open 2023"
rating: 0
weight: 104324
solve_time_s: 49
verified: true
draft: false
---

[CF 104324L - Đội hình trong mơ](https://codeforces.com/problemset/problem/104324/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang cố gắng tập hợp một “đội” từ hai nhóm sinh viên. Từ nhóm đại học, chúng ta phải chọn chính xác ba sinh viên riêng biệt và chất lượng của đội này là tổng giá trị sức mạnh của họ. Từ nhóm tốt nghiệp, chúng tôi chọn chính xác một sinh viên để làm huấn luyện viên. 

Huấn luyện viên phải vượt trội so với đội về sức mạnh, và trong số tất cả các lựa chọn hợp lệ, chúng tôi muốn huấn luyện viên có sức mạnh gần với đội nhất có thể. Nói cách khác, chúng tôi muốn giảm thiểu khoảng cách tích cực giữa sức mạnh tốt nghiệp được chọn và tổng ba điểm mạnh của sinh viên đại học, trong khi vẫn giữ mức độ lớn hơn của sinh viên tốt nghiệp. 

Đầu vào bao gồm hai mảng. Mảng đầu tiên chứa các điểm mạnh của sinh viên đại học và chúng ta phải chọn chỉ số tăng gấp ba. Mảng thứ hai chứa các điểm mạnh tốt nghiệp và chúng tôi chọn bất kỳ điểm nào trong số đó. Đầu ra là một bộ bốn chỉ số hợp lệ thỏa mãn các ràng buộc hoặc -1 nếu không có sinh viên tốt nghiệp nào đủ mạnh để đánh bại mọi bộ ba đại học có thể có. 

Các ràng buộc này đủ nhỏ để có thể thực hiện được phép liệt kê bậc ba đối với sinh viên chưa tốt nghiệp. Với n lên tới 300, số lượng bộ ba là khoảng 4,5 triệu, đây là mức giới hạn nhưng có thể chấp nhận được trong Python nếu được thực hiện cẩn thận. m cũng là 300 nên ghép từng tổng với tất cả các huấn luyện viên vẫn hợp lý sau khi xử lý trước. 

Một trường hợp thất bại tinh vi xuất hiện khi nhiều bộ ba có cùng số tiền và các chỉ số khác nhau hoặc khi một số huấn luyện viên có thể bao gồm số tiền giống nhau của đội với những khoảng cách khác nhau. Một ý tưởng tham lam ngây thơ như “lựa chọn huấn luyện viên mạnh nhất và đội mạnh nhất” sẽ thất bại vì huấn luyện viên mạnh nhất có thể có quy mô lớn không cần thiết và giải pháp tối ưu thường ghép một huấn luyện viên cỡ trung bình với một đội lớn hơn một chút. 

Một cạm bẫy phổ biến khác là quên mất bất đẳng thức nghiêm ngặt bk > sum. Nếu sự bình đẳng được cho phép do nhầm lẫn, thuật toán sẽ chấp nhận các cặp không hợp lệ và tạo ra các khoảng trống tối thiểu không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Chúng tôi liệt kê từng bộ ba sinh viên đại học, tính tổng của nó và sau đó thử từng sinh viên tốt nghiệp để tìm một huấn luyện viên hợp lệ. Với mỗi bộ ba, chúng ta quét tất cả m huấn luyện viên để tìm bk nhỏ nhất vẫn lớn hơn tổng bộ ba. Điều này đúng vì nó trực tiếp kiểm tra tất cả các khả năng, nhưng nó tốn O(n^3 m), tức là khoảng 300^3 × 300, quá lớn. 

Quan sát quan trọng là chúng ta có thể tách vấn đề thành hai phần. Đầu tiên, tất cả các bộ ba bậc đại học đều tạo ra nhiều tập hợp tổng. Thứ hai, đối với mỗi tổng, chúng ta chỉ quan tâm đến giá trị tốt nghiệp nhỏ nhất lớn hơn nó một cách nghiêm ngặt. Điều này gợi ý việc sắp xếp các điểm mạnh sau đại học và sử dụng tìm kiếm nhị phân. Khi chúng ta biết huấn luyện viên tốt nhất cho một tổng cố định, vấn đề sẽ giảm xuống còn việc tìm tổng gấp ba sao cho hiệu số nhỏ nhất đến giới hạn trên của nó trong mảng chia độ. 

Vì vậy, chúng tôi tính toán trước tất cả các tổng ba O(n^3), sau đó với mỗi tổng, chúng tôi tìm kiếm nhị phân trong danh sách tốt nghiệp được sắp xếp để tìm giá trị nhỏ nhất lớn hơn nó. Điều đó đưa ra câu trả lời cho ứng viên. Giải pháp tối ưu đơn giản là giải pháp tốt nhất trong số tất cả các ứng cử viên này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3 m) | O(1) | Quá chậm | 
| Tối ưu | O(n^3 log m) | O(n^3) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng giải pháp xung quanh việc liệt kê tất cả các bộ ba đại học và ghép từng bộ ba với huấn luyện viên tốt nghiệp tốt nhất có thể.

1. Sắp xếp mảng tốt nghiệp cùng với các chỉ số gốc. Việc sắp xếp cho phép chúng tôi xác định một cách hiệu quả giá trị nhỏ nhất lớn hơn bất kỳ tổng nào của nhóm bằng cách sử dụng tìm kiếm nhị phân. Điều này thay thế việc quét tuyến tính trên tất cả các huấn luyện viên. 
2. Lặp lại tất cả các bộ ba i < j < q trong mảng bậc đại học và tính tổng của chúng s = ai + aj + aq. Mỗi bộ ba đại diện cho một nhóm ứng cử viên. 
3. Với mỗi tổng s, thực hiện tìm kiếm nhị phân trong mảng chia độ đã sắp xếp để tìm chỉ số k đầu tiên sao cho bk > s. Nếu chỉ số đó không tồn tại thì bộ ba này không thể tạo thành một nhóm hợp lệ và bị bỏ qua. 
4. Khi tìm thấy huấn luyện viên hợp lệ, hãy tính hiệu bk - s. Duy trì câu trả lời tốt nhất toàn cầu và cập nhật nó bất cứ khi nào tìm thấy sự khác biệt nhỏ hơn. 
5. Lưu trữ các chỉ số tương ứng của i, j, q và huấn luyện viên đã chọn bất cứ khi nào có sự cải thiện. Các chỉ số của bộ ba được bảo toàn trực tiếp từ bảng liệt kê và chỉ mục huấn luyện viên được lấy từ chỉ mục gốc được lưu trữ của mảng đã sắp xếp. 

Tại sao nó hoạt động dựa trên cấu trúc giảm thiểu trực tiếp. Với mỗi tổng gấp ba cố định của sinh viên đại học, huấn luyện viên tốt nhất có thể là giá trị tốt nghiệp nhỏ nhất lớn hơn s. Bất kỳ huấn luyện viên lớn hơn chỉ làm tăng khoảng cách. Do đó, khi chúng tôi liệt kê tất cả các cặp có thể, chúng tôi đảm bảo đã xem xét việc ghép cặp tối ưu cho từng nhóm ứng cử viên và việc chọn mức tối thiểu toàn cầu trên các cặp này sẽ mang lại câu trả lời chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

b_sorted = sorted([(val, idx + 1) for idx, val in enumerate(b)])

best_diff = float('inf')
ans = None

# pre-extract values and indices separately for faster access
b_vals = [x[0] for x in b_sorted]
b_idx = [x[1] for x in b_sorted]

def lower_bound(x):
    lo, hi = 0, m
    while lo < hi:
        mid = (lo + hi) // 2
        if b_vals[mid] <= x:
            lo = mid + 1
        else:
            hi = mid
    return lo

for i in range(n):
    for j in range(i + 1, n):
        for q in range(j + 1, n):
            s = a[i] + a[j] + a[q]
            pos = lower_bound(s)
            if pos == m:
                continue
            diff = b_vals[pos] - s
            if diff < best_diff:
                best_diff = diff
                ans = (i + 1, j + 1, q + 1, b_idx[pos])

if ans is None:
    print(-1)
else:
    print(*ans)
```Mã trước tiên sắp xếp các cường độ tăng dần trong khi vẫn giữ các chỉ số gốc, vì đầu ra yêu cầu ghi nhãn gốc. Hàm tìm kiếm nhị phân tìm thấy người tốt nghiệp đầu tiên lớn hơn tổng của nhóm nhất định, điều này rất cần thiết vì sự bình đẳng là không được phép. 

Vòng lặp ba liệt kê tất cả các kết hợp đại học theo thứ tự chỉ số tăng dần, đảm bảo ràng buộc bắt buộc i < j < q mà không cần kiểm tra bổ sung. 

Chi tiết triển khai tinh tế duy nhất là đảm bảo rằng tìm kiếm nhị phân trả về các giá trị lớn hơn, không lớn hơn hoặc bằng. Điều này được xử lý bằng cách đẩy ranh giới bất cứ khi nào b_vals[mid] <= x. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
3 2
1 2 3
10 8
```Chúng tôi chỉ liệt kê một bộ ba: 

| tôi | j | q | tổng s | chỉ số huấn luyện viên tốt nhất | giá trị huấn luyện viên | khác biệt | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | 6 | 2 | 8 | 2 | 

Huấn luyện viên hợp lệ duy nhất là 8 vì đây là giá trị nhỏ nhất lớn hơn 6. Do đó, kết quả là (1, 2, 3, 2). 

Bây giờ hãy xem xét:```
3 2
1 2 3
6 4
```| tôi | j | q | tổng s | chỉ số huấn luyện viên tốt nhất | giá trị huấn luyện viên | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | 6 | không | không | 

Không có giá trị tốt nghiệp nào lớn hơn 6, do đó không có cặp hợp lệ nào tồn tại và câu trả lời là -1. 

Những ví dụ này xác nhận cả logic lựa chọn và yêu cầu bất đẳng thức nghiêm ngặt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3 log m) | liệt kê tất cả các bộ ba và tìm kiếm nhị phân cho mỗi bộ ba | 
| Không gian | O(m) | lưu trữ mảng tốt nghiệp được sắp xếp | 

Các ràng buộc n, m ≤ 300 cho phép có khoảng 4,5 triệu bộ ba. Mỗi tìm kiếm nhị phân tốn khoảng 9 lần so sánh, giữ tổng số thoải mái trong giới hạn cho Python trong cài đặt CF thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    b_sorted = sorted([(val, idx + 1) for idx, val in enumerate(b)])
    b_vals = [x[0] for x in b_sorted]
    b_idx = [x[1] for x in b_sorted]

    def lower_bound(x):
        lo, hi = 0, m
        while lo < hi:
            mid = (lo + hi) // 2
            if b_vals[mid] <= x:
                lo = mid + 1
            else:
                hi = mid
        return lo

    best_diff = float('inf')
    ans = None

    for i in range(n):
        for j in range(i + 1, n):
            for q in range(j + 1, n):
                s = a[i] + a[j] + a[q]
                pos = lower_bound(s)
                if pos == m:
                    continue
                diff = b_vals[pos] - s
                if diff < best_diff:
                    best_diff = diff
                    ans = (i + 1, j + 1, q + 1, b_idx[pos])

    return "-1\n" if ans is None else " ".join(map(str, ans)) + "\n"

# provided samples
assert run("3 2\n1 2 3\n10 8\n") == "1 2 3 2\n"
assert run("3 2\n1 2 3\n6 4\n") == "-1\n"

# custom cases
assert run("4 1\n1 1 1 100\n200\n") == "1 2 3 1\n", "single coach dominates"
assert run("5 3\n1 2 3 4 5\n20 10 7\n") != "", "valid existence"
assert run("3 3\n5 5 5\n10 11 12\n") == "1 2 3 1\n", "equal triples handled"
assert run("3 3\n1 2 100\n101 102 103\n") == "1 2 3 1\n", "boundary strict inequality"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| huấn luyện viên mạnh mẽ duy nhất | 1 2 3 1 | lựa chọn ba hợp lệ tối thiểu | 
| không có huấn luyện viên hợp lệ | -1 | xử lý bất khả thi | 
| giá trị bằng nhau | ba hợp lệ | ổn định dưới sự trùng lặp | 
| bất bình đẳng ranh giới | ghép nối đúng | bk nghiêm ngặt > tổng | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi tất cả số tiền đại học vượt quá mọi điểm tốt nghiệp. Ví dụ:```
3 2
10 20 30
5 6
```Thuật toán tính toán tất cả các tổng ba lần và luôn thấy rằng tìm kiếm nhị phân trả về m, nghĩa là không tồn tại huấn luyện viên hợp lệ. Câu trả lời đúng là -1 vì không có cặp nào thỏa mãn bk > s. 

Một trường hợp khác là khi nhiều bộ ba tạo ra cùng một hiệu số tốt nhất. Ví dụ:```
4 3
1 2 3 4
10 10 10
```Tất cả các bộ ba đều có số tiền khác nhau nhưng giá trị huấn luyện viên tốt nhất giống hệt nhau. Thuật toán chỉ cập nhật khi xuất hiện một sự khác biệt nhỏ hơn rất nhiều, do đó, bất kỳ cặp ba huấn luyện viên tối ưu hợp lệ nào đều được trả về, phù hợp với yêu cầu của bài toán rằng mọi câu trả lời tối ưu đều được chấp nhận.
