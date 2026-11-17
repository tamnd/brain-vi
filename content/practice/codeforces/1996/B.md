---
title: "CF 1996B - Cân"
description: "Chúng ta có một lưới vuông có kích thước $n nhân n$ trong đó mỗi ô chứa số 0 hoặc số 1. Nhiệm vụ là giảm lưới này đi một hệ số $k$, hệ số này đảm bảo chia $n$ đều."
date: "2026-06-08T14:41:24+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1996
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 962 (Div. 3)"
rating: 800
weight: 1996
solve_time_s: 151
verified: true
draft: false
---

[CF 1996B - Tỷ lệ](https://codeforces.com/problemset/problem/1996/B) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 2m 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới vuông có kích thước$n \times n$trong đó mỗi ô chứa số 0 hoặc số 1. Nhiệm vụ là giảm lưới này đi một hệ số$k$, đảm bảo chia hết$n$đồng đều. Giảm hoạt động bằng cách phân vùng lưới thành$k \times k$các khối không chồng chéo, trong đó mỗi khối chứa cùng một giá trị và thay thế mỗi khối bằng một ô biểu thị giá trị đó. Đầu ra là lưới có kích thước nhỏ hơn$(n/k) \times (n/k)$là kết quả của việc nén khối này. 

Với những hạn chế,$n$có thể lớn tới 1000 nhưng tổng của$n$trên tất cả các trường hợp thử nghiệm nhiều nhất là 1000. Điều này ngụ ý rằng việc lặp lại đơn giản trên tất cả các ô là có thể chấp nhận được, vì chúng ta sẽ không vượt quá khoảng$10^6$tổng số hoạt động. Các trường hợp cạnh không rõ ràng phát sinh khi$k = 1$hoặc$k = n$. Nếu như$k = 1$, lưới rút gọn giống như lưới ban đầu nên chúng ta phải tránh việc vô tình bỏ qua bất kỳ hàng hoặc cột nào. Nếu như$k = n$, toàn bộ lưới sẽ thu gọn thành một ô duy nhất. Việc triển khai bất cẩn có thể cố gắng đọc các chỉ số vượt quá giới hạn lưới hoặc trộn lẫn các điểm bắt đầu của khối trong những trường hợp này. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: lặp lại trên tất cả các khối có kích thước$k \times k$, chọn bất kỳ ô đại diện nào (ví dụ: ô trên cùng bên trái) và đặt giá trị của nó vào lưới đã rút gọn. Điều này hoạt động chính xác vì sự cố đảm bảo tất cả các ô trong một khối đều giống hệt nhau. Việc triển khai đơn giản sẽ kiểm tra từng ô trong từng khối, nhưng điều đó là không cần thiết và dư thừa vì tính bảo đảm. 

Cách tiếp cận tối ưu tận dụng sự đảm bảo này bằng cách chỉ đọc một ô trên mỗi khối. Điều này làm giảm công việc xuống$(n/k)^2$hoạt động trên mỗi lưới, điều này không đáng kể với các ràng buộc. Sự quan sát rằng mọi$k \times k$khối đồng nhất cho phép chúng ta bỏ qua việc lặp lại trên tất cả$k^2$các ô trong khối, giúp giải pháp vừa đơn giản vừa nhanh hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra tất cả các ô trong mỗi khối | O(n^2) | O(n^2) | Được chấp nhận do tổng n nhỏ | 
| Chỉ kiểm tra ô trên cùng bên trái mỗi khối | O((n/k)^2) | O((n/k)^2) | Được chấp nhận, đơn giản hơn và nhanh hơn | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Mỗi trường hợp thử nghiệm bao gồm một kích thước lưới$n$, hệ số giảm$k$, Và$n$dòng của$n$mỗi ký tự. 
2. Đối với mỗi trường hợp thử nghiệm, hãy xác định kích thước của lưới giảm, đó là$m = n/k$. 
3. Khởi tạo một danh sách trống để lưu trữ lưới đã rút gọn. 
4. Lặp lại từng chỉ mục hàng khối$i$từ 0 đến$m-1$. Hàng lưới ban đầu tương ứng là$i \cdot k$. 
5. Trong mỗi hàng khối, lặp lại từng chỉ mục cột khối$j$từ 0 đến$m-1$. Cột lưới ban đầu tương ứng là$j \cdot k$. 
6. Lấy ô trên cùng bên trái của$k \times k$chặn tại$(i \cdot k, j \cdot k)$và nối nó vào hàng hiện tại của lưới đã giảm. 
7. Sau khi xử lý tất cả các cột cho hàng khối hiện tại, hãy nối hàng đó vào lưới đã rút gọn. 
8. In từng hàng lưới đã rút gọn. 

Tại sao nó hoạt động: đảm bảo rằng tất cả các ô trong một$k \times k$khối giống hệt nhau cho phép chúng ta chỉ chọn ô trên cùng bên trái một cách an toàn. Việc lặp lại các chỉ mục khối thay vì các ô riêng lẻ đảm bảo chúng tôi bao gồm tất cả các khối chính xác một lần mà không bỏ sót hoặc sao chép bất kỳ khối nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        grid = [input().strip() for _ in range(n)]
        m = n // k
        reduced = []
        for i in range(m):
            row = []
            for j in range(m):
                row.append(grid[i * k][j * k])
            reduced.append("".join(row))
        print("\n".join(reduced))

if __name__ == "__main__":
    solve()
```Đầu tiên, giải pháp sẽ đọc số lượng ca kiểm thử, sau đó xử lý từng trường hợp kiểm thử riêng biệt. Lưới ban đầu được đọc đầy đủ vào bộ nhớ. Lưới thu gọn được xây dựng bằng cách truy cập vào ô trên cùng bên trái của mỗi ô$k \times k$khối. Mỗi hàng của lưới rút gọn được nối thành một chuỗi và được in tuần tự. Việc sử dụng phép chia số nguyên đảm bảo lập chỉ mục khối chính xác. Điều này xử lý các trường hợp cạnh$k=1$Và$k=n$một cách tự nhiên vì các vòng lặp thích ứng với kích thước giảm$m = n/k$. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
6 3
000111
000111
000111
111000
111000
111000
```chúng tôi có$n = 6, k = 3$, Vì thế$m = 2$. Lưới thu gọn được xây dựng bằng cách lấy phần trên bên trái của mỗi lưới$3 \times 3$khối: 

| Chặn chỉ số | Ô trên cùng bên trái | Lưới giảm | 
| --- | --- | --- | 
| (0,0) | 0 | 0 | 
| (0,1) | 1 | 01 | 
| (1,0) | 1 | 10 | 
| (1,1) | 0 | 10 | 

Lưới giảm là:```
01
10
```Điều này chứng tỏ ánh xạ chính xác từ lưới ban đầu sang lưới đã rút gọn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2 / k^2) cho mỗi trường hợp thử nghiệm | Chúng tôi chỉ lặp lại các chỉ mục khối, đọc một ô trên mỗi khối. | 
| Không gian | O(n^2) | Chúng tôi lưu trữ lưới ban đầu. Lưới giảm là không đáng kể khi so sánh. | 

Với hạn chế là tổng của$n$trên tất cả các trường hợp thử nghiệm không vượt quá 1000, tổng số thao tác nhỏ và dễ dàng nằm gọn trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided sample
assert run("4\n4 4\n0000\n0000\n0000\n0000\n6 3\n000111\n000111\n000111\n111000\n111000\n111000\n6 2\n001100\n001100\n111111\n111111\n110000\n110000\n8 1\n11111111\n11111111\n11111111\n11111111\n11111111\n11111111\n11111111\n11111111\n") == "0\n01\n10\n010\n111\n100\n11111111\n11111111\n11111111\n11111111\n11111111\n11111111\n11111111\n11111111"

# Custom cases
assert run("1\n1 1\n1\n") == "1", "minimum size"
assert run("1\n4 2\n1111\n1111\n1111\n1111\n") == "11\n11", "all equal"
assert run("1\n4 4\n1010\n1010\n1010\n1010\n") == "1", "full collapse"
assert run("1\n4 1\n1010\n0101\n1010\n0101\n") == "1010\n0101\n1010\n0101", "k=1 identity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | Lưới kích thước tối thiểu | 
| 4 2 | 11\n11 | Khối đồng nhất | 
| 4 4 | 1 | Thu gọn toàn bộ lưới thành một ô | 
| 4 1 | 1010\n0101\n1010\n0101 | k=1 ánh xạ nhận dạng | 

## Vỏ cạnh 

Khi nào$k = 1$, thuật toán lặp lại trên từng ô riêng lẻ, sao chép lưới gốc một cách hiệu quả. Vì$k = n$, lưới rút gọn là một ô duy nhất. Cả hai trường hợp đều hoạt động mà không cần xử lý đặc biệt vì giới hạn vòng lặp được tính là$m = n/k$, trở thành 1 trong trường hợp sụp đổ và$n$trong trường hợp nhận dạng. Lựa chọn luôn đọc ô trên cùng bên trái của mỗi khối sẽ thúc đẩy việc đảm bảo tính đồng nhất một cách an toàn, tránh mọi lỗi sai lệch một hoặc chỉ mục.
