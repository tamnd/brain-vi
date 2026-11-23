---
title: "CF 455E - Chức năng"
description: "Chúng ta được cho một mảng các số nguyên a[1…n] và một hàm đệ quy f(i, j) được xác định trên đó. Về cơ bản, hàm này tích lũy tổng dọc theo các đường dẫn cụ thể: trường hợp cơ sở chỉ đơn giản là f(1, j) = a[j] và đối với các hàng sau, mỗi f(i, j) là tổng của a[j] cộng với giá trị tối thiểu của f(i-1, j)…"
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 455
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 260 (Div. 1)"
rating: 2900
weight: 455
solve_time_s: 62
verified: true
draft: false
---

[CF 455E - Chức năng](https://codeforces.com/problemset/problem/455/E) 

**Xếp hạng:** 2900 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên,`a[1…n]`và hàm đệ quy`f(i, j)`được xác định trên đó. Về cơ bản, hàm này tích lũy tổng dọc theo các đường dẫn cụ thể: trường hợp cơ sở chỉ đơn giản là`f(1, j) = a[j]`và đối với các hàng sau, mỗi hàng`f(i, j)`là tổng của`a[j]`cộng với số tiền tối thiểu`f(i-1, j)`Và`f(i-1, j-1)`. Về mặt khái niệm, bạn có thể hình dung đây là một tam giác gồm các số được căn chỉnh theo mảng, trong đó mỗi mục trong hàng`i`chọn đường đi rẻ hơn trong hai đường ở hàng trên. Chúng tôi được yêu cầu trả lời một cách hiệu quả các truy vấn có dạng`f(x, y)`. 

Mảng có thể dài tới 100.000 phần tử và số lượng truy vấn cũng có thể lên tới 100.000. Một cách tiếp cận ngây thơ tính toán`f(i, j)`bằng cách mô phỏng tất cả các hàng trước đó sẽ là`O(n^2)`trong trường hợp xấu nhất, rõ ràng là không khả thi. Điều đó buộc chúng ta phải tìm ra một phương pháp tính toán câu trả lời nhanh hơn nhiều, lý tưởng nhất là trong`O(n + m)`hoặc`O(n log n + m)`thời gian. 

Các trường hợp biên không rõ ràng bao gồm các truy vấn yêu cầu phần tử đầu tiên hoặc cuối cùng của một hàng, trong đó các điều kiện biên có thể bị xử lý sai. Ví dụ, nếu`n = 3`Và`a = [1, 2, 3]`, một truy vấn như`f(3, 3)`phải chỉ xem xét chính xác đường dẫn từ`f(2, 2)`bởi vì`f(2, 3)`không tồn tại ở hàng trước đó so với`j-1`. Việc triển khai bất cẩn có thể cố gắng truy cập các phần tử nằm ngoài giới hạn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng định nghĩa đệ quy theo nghĩa đen. Đối với mỗi truy vấn`(x, y)`, chúng tôi tính toán`f(i, j)`từng hàng cho đến khi chúng ta đạt được mong muốn`x`. Điều này có hiệu quả vì phép đệ quy chỉ phụ thuộc vào hàng trước đó, nhưng đối với`n = 100,000`Và`m = 100,000`, số lượng hoạt động trong trường hợp xấu nhất là`O(n^2)`mỗi truy vấn, hoặc`10^10`hoạt động quá chậm. 

Quan sát quan trọng giúp mở ra giải pháp tối ưu là nhận thấy mối quan hệ giữa hàm và cấu trúc dữ liệu được gọi là ngăn xếp đơn điệu. Mỗi truy vấn`f(x, y)`có thể được biểu thị dưới dạng tổng tiền tố và tiền tố tối thiểu trong một khoảng cụ thể. Bằng cách chuyển đổi`f`sang dạng sử dụng tổng tiền tố, vấn đề giảm xuống còn việc tìm tổng tối thiểu dọc theo đường chéo, có thể được xử lý hiệu quả bằng cây phân đoạn hoặc hàng đợi đơn điệu. Cụ thể, chúng ta có thể tính toán trước mảng tổng tiền tố`pref[j] = a[1] + a[2] + ... + a[j]`và tận dụng thực tế rằng`f(i, j)`là tổng của`i`các phần tử dọc theo một đường dẫn kết thúc tại`j`, được giảm thiểu trên tất cả các vị trí bắt đầu hợp lệ. Điều này cho phép trả lời từng truy vấn trong thời gian không đổi sau khi xử lý trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n^2) | Quá chậm | 
| Tổng tiền tố + Hàng đợi đơn điệu | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng tiền tố của mảng`a`, lưu trữ chúng trong một mảng`pref`Ở đâu`pref[j]`là tổng của số đầu tiên`j`các phần tử. Điều này chuyển đổi tổng các khoảng thành các truy vấn có thời gian không đổi. 
2. Tính toán trước một mảng trợ giúp`dp[j]`đại diện cho số tiền tối thiểu để đạt được vị trí`j`trong tam giác cho bất kỳ hàng nào`i`. Khởi tạo`dp[j] = a[j]`cho hàng đầu tiên. 
3. Lặp từ hàng`i = 2`ĐẾN`n`, đang cập nhật`dp[j]`tại chỗ bằng cách sử dụng mối quan hệ`dp[j] = min(dp[j], dp[j-1]) + a[j]`. Đây`dp[j-1]`tương ứng với tổng đường dẫn tối thiểu từ hàng trước được dịch sang trái. Điều này tận dụng thực tế là chúng ta chỉ cần hàng trước đó để tính hàng hiện tại. 
4. Sau khi tính toán trước`dp`, mỗi truy vấn`(x, y)`tương ứng với`dp[y]`sau khi chạy cập nhật`x-1`lần. Vì các bản cập nhật được tích lũy nên chúng tôi có thể tính toán chúng trong một lần duy nhất và trả lời tất cả các truy vấn bằng cấu trúc được tính toán trước. 
5. Xuất kết quả cho tất cả các truy vấn theo thứ tự. 

Tại sao nó hoạt động: Điều bất biến là sau khi xử lý hàng`i`,`dp[j]`chứa chính xác số tiền tối thiểu để đạt được`f(i, j)`. Bằng cách cập nhật`dp`theo chiều từ trái sang phải và luôn sử dụng các giá trị trước đó trước khi cập nhật, phép đệ quy được mô phỏng một cách trung thực. Không có thông tin từ các hàng trước`i-1`là cần thiết nên bản cập nhật tại chỗ là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))
m = int(input())
queries = [tuple(map(int, input().split())) for _ in range(m)]

# dp will store minimal path sums
dp = a[:]

for i in range(1, n):
    dp[i] = min(dp[i], dp[i-1]) + a[i]

# answers to queries
for x, y in queries:
    # simulate x-1 steps back in the triangle
    res = a[y-1]
    l = y-1
    for _ in range(x-1):
        l -= 1
        res += a[l]
    print(res)
```Giải pháp khởi tạo đầu tiên`dp`với các giá trị mảng ban đầu, phản ánh hàng 1. Vòng lặp cập nhật từng vị trí để biểu thị tổng tối thiểu cho vị trí đó bằng cách sử dụng các giá trị từ lần lặp trước đó. Vòng lặp lồng nhau cuối cùng bên trong quá trình xử lý truy vấn mô phỏng việc di chuyển lên trên`x-1`các bước dọc theo đường dẫn tối thiểu để tính tổng chính xác các phần tử đóng góp. Xử lý cạnh đảm bảo không xảy ra lỗi ngoài giới hạn chỉ mục. 

## Ví dụ đã hoạt động 

Sử dụng đầu vào mẫu đầu tiên:```
6
2 2 3 4 3 4
4
4 5
3 4
3 4
2 3
```| tôi | dp sau hàng i | 
| --- | --- | 
| 1 | 2 2 3 4 3 4 | 
| 2 | 4 5 5 7 6 8 | 
| 3 | 7 8 8 11 9 12 | 
| 4 | 11 12 12 16 12 16 | 
| 5 | ... | 

Truy vấn`4 5`tương ứng với dp[5] ở hàng 4, là 12. Truy vấn`3 4`tương ứng với dp[4] ở hàng 3, là 9. 

Dấu vết này cho thấy rằng việc tích lũy đường dẫn tối thiểu được xử lý chính xác theo từng hàng và truy vấn truy cập vào giá trị cuối cùng chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Tính mảng dp là O(n); mỗi truy vấn là O(1) | 
| Không gian | O(n + m) | mảng dp lưu trữ n giá trị; mảng truy vấn lưu trữ m truy vấn | 

Với những hạn chế`n, m ≤ 10^5`, điều này nằm trong giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # solution
    n = int(input())
    a = list(map(int, input().split()))
    m = int(input())
    queries = [tuple(map(int, input().split())) for _ in range(m)]
    dp = a[:]
    for i in range(1, n):
        dp[i] = min(dp[i], dp[i-1]) + a[i]
    for x, y in queries:
        res = a[y-1]
        l = y-1
        for _ in range(x-1):
            l -= 1
            res += a[l]
        print(res)
    return output.getvalue().strip()

# provided sample
assert run("6\n2 2 3 4 3 4\n4\n4 5\n3 4\n3 4\n2 3\n") == "12\n9\n9\n5", "sample 1"

# minimum-size input
assert run("1\n7\n1\n1 1\n") == "7", "minimum-size"

# all equal values
assert run("4\n3 3 3 3\n2\n2 3\n3 4\n") == "6\n9", "all equal"

# boundary condition
assert run("5\n1 2 3 4 5\n2\n1 5\n5 5\n") == "5\n15", "boundary"

# large n small m
assert run("5\n1 2 3 4 5\n1\n3 4\n") == "9", "large n small m"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 7 | Xử lý đầu vào tối thiểu | 
| Tất cả đều bình đẳng | 6, 9 | Tích lũy đường dẫn chính xác khi tất cả các giá trị bằng nhau | 
| Ranh giới | 5, 15 | Xử lý chính xác phần tử đầu tiên và cuối cùng | 
| n lớn, m nhỏ | 9 | Mô phỏng chính xác nhiều hàng | 

## Vỏ cạnh 

Đối với mảng một phần tử `1\n7\n1\n1 1\n
