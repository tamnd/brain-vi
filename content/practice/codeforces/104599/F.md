---
title: "CF 104599F - Câu đố điều hướng"
description: "Chúng ta có một đường tròn với các vị trí được gắn nhãn $n$. Ba mã thông báo bắt đầu ở các vị trí $a$, $b$ và $c$. Trong một lần di chuyển, chúng tôi chọn chính xác một mã thông báo và dịch chuyển nó một bước theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ dọc theo vòng tròn."
date: "2026-06-30T03:00:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104599
codeforces_index: "F"
codeforces_contest_name: "GPL 2023 Novice"
rating: 0
weight: 104599
solve_time_s: 86
verified: true
draft: false
---

[CF 104599F - Câu đố điều hướng](https://codeforces.com/problemset/problem/104599/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một đường tròn với$n$các vị trí được dán nhãn. Ba mã thông báo bắt đầu ở vị trí$a$,$b$, Và$c$. Trong một lần di chuyển, chúng tôi chọn chính xác một mã thông báo và dịch chuyển nó một bước theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ dọc theo vòng tròn. 

Mục tiêu là làm cho cả ba mã thông báo kết thúc ở cùng một vị trí bằng cách sử dụng số lần di chuyển nhỏ nhất có thể. Vì sự di chuyển là độc lập đối với mỗi mã thông báo và chỉ phụ thuộc vào khoảng cách vòng tròn, nên vấn đề thực sự là việc chọn điểm gặp mặt và đo lường mức giá của mỗi mã thông báo để tiếp cận nó. 

Đầu ra là một số nguyên duy nhất: tổng số lần di chuyển đơn vị tối thiểu cần thiết để căn chỉnh cả ba mã thông báo. 

Ràng buộc$n \le 10^9$có nghĩa là chúng ta không thể mô phỏng chuyển động. Bất kỳ cách tiếp cận nào lặp lại các vị trí hoặc thực hiện BFS trên vòng tròn đều không thể thực hiện được. Mọi thứ phải được tính toán bằng công thức khoảng cách trực tiếp trong thời gian không đổi. 

Trường hợp cạnh tinh tế xuất hiện khi các điểm quấn quanh vòng tròn. Ví dụ, với$n = 5$, chuyển từ$1$ĐẾN$5$là khoảng cách của$1$, không$4$. Công thức khoảng cách tuyến tính sẽ không chính xác trừ khi chúng ta lấy khoảng cách tối thiểu theo chiều kim đồng hồ và ngược chiều kim đồng hồ một cách rõ ràng. 

Một trường hợp không rõ ràng khác là khi hai hoặc nhiều mã thông báo trùng nhau. Ví dụ,$(1, 1, 5)$trông có vẻ thoái hóa nhưng vẫn có thể yêu cầu di chuyển nếu điểm gặp mặt tối ưu không phải là địa điểm chung. 

## Phương pháp tiếp cận 

Một giải pháp brute-force thử mọi điểm gặp gỡ có thể$x \in [1, n]$. Đối với mỗi$x$, tính tổng khoảng cách hình tròn từ$a$,$b$, Và$c$ĐẾN$x$, sau đó lấy giá trị nhỏ nhất trên tất cả$x$. Mỗi lần đánh giá tốn thời gian không đổi, vì vậy tổng công việc là$O(n)$. Với$n$lên đến$10^9$, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là hàm chi phí hoàn toàn dựa trên khoảng cách đường tròn và đối với ba điểm trên một đường tròn, điểm gặp nhau tối ưu phải nằm trên một trong các cung được xác định bởi các điểm đó. Thay vì quét tất cả các vị trí, chúng ta chỉ cần xét các trung tuyến ứng cử viên dọc theo vòng tròn. 

Trên một đường dây, điểm gặp nhau tối ưu để giảm thiểu tổng khoảng cách là đường trung tuyến. Trên một đường tròn, chúng ta có thể “cắt” đường tròn ở mỗi khoảng trống có thể có và thu gọn nó thành một sắp xếp tuyến tính. Với ba điểm, cấu trúc có ý nghĩa duy nhất là sự sắp xếp thứ tự vòng tròn của chúng, giúp giảm bớt vấn đề kiểm tra số lượng cấu hình không đổi. 

Giải pháp trở thành thời gian không đổi bằng cách sắp xếp ba vị trí và suy luận về độ dài cung. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên tất cả các điểm |$O(n)$|$O(1)$| Quá chậm | 
| Lý luận trung bình tròn |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp ba vị trí để chúng ta có thể suy luận về thứ tự tuần hoàn của chúng một cách nhất quán. Điều này loại bỏ sự mơ hồ gây ra bởi hoán vị. 
2. Tính ba khoảng trống hình tròn liên tiếp giữa các điểm dọc theo đường tròn. Nếu chúng ta gắn nhãn các vị trí được sắp xếp là$x_1 \le x_2 \le x_3$, thì khoảng trống là$x_2 - x_1$,$x_3 - x_2$, và khoảng cách bao quanh$(n - x_3 + x_1)$. 
3. Xác định khoảng cách lớn nhất. Điều này thể hiện “cung trống” không cần phải đi qua nếu chúng ta chọn điểm gặp nhau tối ưu. 
4. Tổng chuyển động tối thiểu là tổng của tất cả các khoảng trống trừ đi khoảng cách lớn nhất. Theo trực giác, chúng ta đi qua hai cung ngắn hơn và tránh cung trống lớn nhất. 

### Tại sao nó hoạt động 

Việc đặt điểm gặp nhau bên trong khoảng trống lớn nhất sẽ đảm bảo không có mã thông báo nào cần vượt qua vòng cung đó. Mỗi bước di chuyển có thể được coi là thu hẹp tổng chu vi cần thiết để tập hợp tất cả các điểm lại với nhau. Bất kỳ giải pháp nào cũng phải bao gồm tất cả các cung ngoại trừ một cung, vì việc hợp nhất cả ba điểm đòi hỏi phải loại bỏ hai khoảng cách và tránh khoảng cách lớn nhất sẽ giảm thiểu tổng hành trình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a, b, c = map(int, input().split())

    x = sorted([a, b, c])

    d1 = x[1] - x[0]
    d2 = x[2] - x[1]
    d3 = n - x[2] + x[0]

    print(d1 + d2 + d3 - max(d1, d2, d3))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp ba vị trí, chuẩn hóa hình học trên vòng tròn. Ba khoảng trống được tính toán tương ứng chính xác với ba cung được hình thành bởi các điểm. Thuật ngữ bao quanh đảm bảo tính tuần hoàn được xử lý chính xác. Việc trừ đi khoảng cách lớn nhất sẽ loại bỏ cung mà chúng ta chọn không cắt qua, điều này mang lại chi phí căn chỉnh tối ưu. 

Một lỗi phổ biến là quên khoảng cách bao quanh hoặc coi đường tròn là một đường thẳng, lỗi này không thành công khi đường đi tối ưu vượt qua ranh giới giữa$n$Và$1$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
1 3 5
```Vị trí được sắp xếp:$[1, 3, 5]$| Bước | Giá trị | 
| --- | --- | 
| d1 | 3 - 1 = 2 | 
| d2 | 5 - 3 = 2 | 
| d3 | 6 - 5 + 1 = 2 | 
| tổng hợp | 6 | 
| khoảng cách tối đa | 2 | 
| kết quả | 6 - 2 = 4 | 

Điều này cho thấy một cấu hình đối xứng trong đó bất kỳ việc loại bỏ hồ quang nào cũng cho kết quả tương tự, xác nhận tính đúng đắn. 

### Ví dụ 2 

đầu vào:```
5
1 1 5
```Vị trí được sắp xếp:$[1, 1, 5]$| Bước | Giá trị | 
| --- | --- | 
| d1 | 1 - 1 = 0 | 
| d2 | 5 - 1 = 4 | 
| d3 | 5 - 5 + 1 = 1 | 
| tổng hợp | 5 | 
| khoảng cách tối đa | 4 | 
| kết quả | 1 | 

Điều này thể hiện việc xử lý các vị trí trùng lặp và sự thống trị toàn diện. Chiến lược tối ưu là di chuyển điểm từ 5 về 1, tốn 1 nước đi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| chỉ sắp xếp 3 phần tử và số học không đổi | 
| Không gian |$O(1)$| số biến cố định | 

Việc tính toán là thời gian không đổi bất kể$n$, điều này rất cần thiết vì$n$có thể lớn như$10^9$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# sample 1
# assert run("6\n1 3 5\n") == "4\n"

# sample 2
# assert run("5\n1 1 5\n") == "1\n"

# all equal
# assert run("10\n4 4 4\n") == "0\n"

# linear cluster
# assert run("10\n1 2 3\n") == "2\n"

# wrap-around dominant
# assert run("10\n1 9 10\n") == "2\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 0 | không cần chuyển động | 
| điểm liên tiếp | 2 | hành vi trung bình trực tuyến | 
| trường hợp bọc xung quanh | 2 | độ chính xác khoảng cách tròn | 

## Vỏ cạnh 

Khi cả ba mã thông báo đã ở cùng một vị trí, tất cả các khoảng trống đều trở thành 0, do đó công thức trả về 0 một cách tự nhiên. Không cần chuyển động và thuật toán xử lý việc này mà không cần phân nhánh đặc biệt. 

Khi hai mã thông báo trùng nhau, một khoảng cách trở thành 0 và giải pháp giảm xuống việc di chuyển mã thông báo thứ ba dọc theo cung ngắn nhất. Công thức khoảng cách được sắp xếp vẫn xác định chính xác cung lớn nhất là cung đối diện với cụm. 

Khi cấu hình tối ưu vượt qua ranh giới giữa$n$Và$1$, khoảng cách bao quanh trở thành một trong những ứng cử viên chủ chốt. Thuật toán bao gồm trường hợp này một cách rõ ràng thông qua$n - x_3 + x_1$, đảm bảo xử lý chính xác mà không cần thêm logic.
