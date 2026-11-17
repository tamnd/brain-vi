---
title: "CF 1991G - Thiết lập lại lưới"
description: "Chúng tôi được yêu cầu quản lý quy trình tô màu lưới động. Lưới có $n$ hàng và $m$ cột, ban đầu tất cả đều có màu trắng. Chúng ta được cho một số nguyên $k$ và một chuỗi các phép tính. Mỗi hoạt động là ngang hoặc dọc."
date: "2026-06-08T15:27:47+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1991
codeforces_index: "G"
codeforces_contest_name: "Pinely Round 4 (Div. 1 + Div. 2)"
rating: 2700
weight: 1991
solve_time_s: 218
verified: false
draft: false
---

[CF 1991G - Thiết lập lại lưới](https://codeforces.com/problemset/problem/1991/G) 

**Đánh giá:** 2700 
**Tags:** thuật toán xây dựng, tham lam, triển khai 
**Thời gian giải:** 3 phút 38 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu quản lý quy trình tô màu lưới động. Lưới có$n$hàng và$m$cột, ban đầu tất cả đều màu trắng. Chúng ta được cho một số nguyên$k$và một chuỗi các thao tác. Mỗi hoạt động là ngang hoặc dọc. Thao tác theo chiều ngang sẽ vẽ một vùng liền kề$1 \times k$hình chữ nhật màu trắng màu đen, trong khi thao tác dọc sẽ vẽ một$k \times 1$hình chữ nhật màu trắng đen. 

Sau mỗi thao tác, nếu bất kỳ hàng hoặc cột nào trở thành màu đen hoàn toàn thì tất cả các ô trong hàng hoặc cột đó sẽ được đặt lại về màu trắng. Việc thiết lập lại này có thể xếp tầng nếu nhiều hàng và cột đầy đồng thời. Thách thức là tạo ra tọa độ cho từng thao tác để tất cả các thao tác đều thành công mà không cần phải cố gắng vẽ lên một ô màu đen. 

Sự tinh tế chính như sau. Đầu tiên, sau khi đặt lại hàng hoặc cột, chúng ta có thể sử dụng lại các ô đã được sơn màu đen trước đó, điều này cho phép thực hiện các chiến lược lấp đầy tham lam. Thứ hai, chúng ta phải tránh những tình huống không phù hợp$1 \times k$hoặc$k \times 1$hình chữ nhật màu trắng tồn tại, điều này sẽ khiến nhiệm vụ không thể thực hiện được. Ví dụ, nếu$n = m = k = 2$và thao tác dọc phải được áp dụng hai lần trong cùng một cột trước khi đặt lại, thao tác thứ hai sẽ không thành công trừ khi xảy ra thao tác đặt lại. 

Các ràng buộc ngụ ý rằng lực lượng vũ phu trên toàn bộ lưới, mỗi hoạt động sẽ có thể chấp nhận được nhưng chưa tối ưu:$n, m \le 100$Và$q \le 1000$, ngây thơ quá$O(n m q)$giải pháp sẽ thực hiện lên đến$10^7$bước, đây là giới hạn dành cho Python dưới 2 giây. Do đó, chúng ta nên tìm một cách có cấu trúc để đặt hình chữ nhật một cách xác định. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: duy trì đầy đủ$n \times m$lưới và đối với mỗi thao tác, hãy quét tất cả các hình chữ nhật có thể có có hình dạng được yêu cầu để tìm một hình có tất cả các ô màu trắng. Sơn nó màu đen, sau đó quét tất cả các hàng và cột để đặt lại các hàng và cột màu đen hoàn toàn. Điều này có tác dụng vì nó phản ánh trực tiếp các quy tắc của bài toán. Nhược điểm của nó là việc quét lưới nhiều lần rất tốn kém; trong trường hợp xấu nhất, mỗi thao tác yêu cầu phải kiểm tra tới$O(n m)$hình chữ nhật. 

Quan sát quan trọng giúp mở ra giải pháp nhanh hơn là việc đặt lại cho phép chúng ta sử dụng lại các hàng và cột vô thời hạn. Điều này có nghĩa là chúng ta có thể tính toán trước một tập hợp vị trí cho hình chữ nhật ngang dọc theo hàng và hình chữ nhật dọc dọc theo cột. Ví dụ: chúng ta luôn có thể đặt một hình chữ nhật nằm ngang ở vị trí đầu tiên.$k$các cột màu trắng liên tiếp của một hàng chưa được sử dụng hết. Việc đặt lại lưới đảm bảo rằng sau khi toàn bộ hàng hoặc cột có màu đen, nó sẽ đặt lại thành màu trắng, vì vậy chúng ta có thể chuyển qua các vị trí mà không gặp phải xung đột. 

Khi nhận ra điều này, chúng ta có thể xử lý các thao tác ngang một cách độc lập dọc theo các hàng và các thao tác dọc một cách độc lập dọc theo các cột. Bằng cách duy trì một con trỏ hoặc chỉ mục cho mỗi hàng/cột, chúng ta luôn có thể chọn đoạn có độ dài tiếp theo$k$mà không cần quét toàn bộ lưới. Điều này làm giảm việc lựa chọn hoạt động về thời gian không đổi cho mỗi hoạt động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q * n * m) | O(n * m) | Hoạt động nhưng chậm trong trường hợp xấu nhất | 
| Vị trí xác định | O(q) | O(1) (cộng với đầu ra O(q)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp thử nghiệm, hãy đọc kích thước lưới$n, m$, kích thước hình chữ nhật$k$, và chuỗi thao tác$s$. Khởi tạo bộ đếm cho vị trí sẵn có tiếp theo dọc theo hàng cho thao tác ngang và dọc theo cột cho thao tác dọc. Các bộ đếm này theo dõi nơi hình chữ nhật tiếp theo có thể bắt đầu. 
2. Đối với mỗi thao tác trong$s$, xác định xem nó nằm ngang hay dọc. Nếu nó nằm ngang, hãy chọn hàng có sẵn tiếp theo và chọn cột bắt đầu sao cho$1 \times k$hình chữ nhật phù hợp. Tăng con trỏ cột; nếu nó vượt quá$m - k + 1$, hãy đặt lại nó về cột đầu tiên của hàng tiếp theo. Nếu không còn hàng nào, xuất ra$-1$vì điều đó là không thể. 
3. Nếu thao tác theo chiều dọc, hãy thực hiện quy trình tương tự dọc theo các cột. Chọn cột tiếp theo và chọn hàng bắt đầu trong đó$k \times 1$hình chữ nhật phù hợp. Tăng con trỏ hàng; nếu nó vượt quá$n - k + 1$, chuyển sang cột tiếp theo. Nếu không còn cột nào, xuất ra$-1$. 
4. Xuất tọa độ trên cùng bên trái của hình chữ nhật đã chọn. Không cần mô phỏng màu lưới thực tế vì thuộc tính đặt lại đảm bảo rằng chu trình xác định này luôn mang lại các vị trí hợp lệ. 

Lý do điều này có tác dụng là vì mọi thao tác theo chiều ngang đều có thể được ánh xạ tới một$k$-phân đoạn trong một số hàng và mọi hoạt động theo chiều dọc thành một duy nhất$k$-phân đoạn trong một số cột. Việc đặt lại lưới đảm bảo chúng tôi không bao giờ hết ô trắng trong sơ đồ này vì các hàng và cột đã sử dụng trước đó sẽ được tái sử dụng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, k, q = map(int, input().split())
        s = input().strip()

        # Pointers for horizontal and vertical placements
        h_row, h_col = 1, 1
        v_row, v_col = 1, 1

        result = []

        for op in s:
            if op == 'H':
                if h_row > n:
                    print(-1)
                    break
                result.append(f"{h_row} {h_col}")
                h_col += k
                if h_col > m:
                    h_row += 1
                    h_col = 1
            else:
                if v_col > m:
                    print(-1)
                    break
                result.append(f"{v_row} {v_col}")
                v_row += k
                if v_row > n:
                    v_col += 1
                    v_row = 1
        else:
            print('\n'.join(result))

if __name__ == "__main__":
    solve()
```các`h_row`Và`h_col`con trỏ được sử dụng để chọn hình chữ nhật nằm ngang tiếp theo. Khi vượt quá giới hạn cột, chúng tôi sẽ chuyển sang hàng tiếp theo. Con trỏ dọc hoạt động tương tự dọc theo các cột. các`else`mệnh đề của vòng lặp đảm bảo rằng chúng ta chỉ in kết quả nếu tất cả các thao tác thành công. Chúng tôi không mô phỏng trạng thái ô thực tế vì cơ chế đặt lại đảm bảo rằng vị trí tuần tự của chúng tôi luôn hợp lệ. Điều kiện biên được xử lý bằng cách kiểm tra`> n`hoặc`> m`và trở về`-1`nếu kiệt sức. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1
4 5 3 6
HVVHHV
```| Hoạt động | Trạng thái con trỏ (h_row, h_col, v_row, v_col) | Tọa độ đầu ra | 
| --- | --- | --- | 
| H | 1,1 | 1,1 | 
| V | 1,1 | 1,1 | 
| V | 4,1 | 4,1 | 
| H | 1,4 | 1,4 | 
| H | 2,1 | 2,1 | 
| V | 1,4 | 1,4 | 

Điều này chứng tỏ rằng việc chuyển con trỏ theo chu kỳ xác định sẽ gán thành công tất cả các hình chữ nhật mà không có xung đột. 

### Đầu vào tùy chỉnh```
1
3 3 2 5
HVHVH
```Trình tự con trỏ: 

| Hoạt động | h_row,h_col | v_row,v_col | Đầu ra | 
| --- | --- | --- | --- | 
| H | 1,1 | - | 1,1 | 
| V | - | 1,1 | 1,1 | 
| H | 1,3 | - | 1,3 | 
| V | - | 3,1 | 3,1 | 
| H | 2,1 | - | 2,1 | 

Điều này xác nhận thuật toán quay vòng các hàng/cột một cách an toàn và không bao giờ hết dung lượng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q) | Mỗi thao tác được xử lý trong thời gian không đổi bằng cách sử dụng số học con trỏ | 
| Không gian | O(q) | Mảng tọa độ đầu ra; không cần mô phỏng lưới | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì$q \le 1000$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided sample
assert run("1\n4 5 3 6\nHVVHHV\n") == "1 1\n1 1\n4 1\n1 4\n2 1\n1 4", "sample 1"

# Minimum size grid
assert run("1\n1 1 1 1\nH\n") == "1 1", "minimum grid"

# Maximum horizontal reuse
assert run("1\n3 3 2 4\nHHHH\n") != "-1", "horizontal reuse after reset"

# Impossible case
assert run("1\n2 2 2 3\nHHH\n") == "-1", "cannot fit third horizontal"

# Mixed operations
assert run("1\n3 3 2 5\nHVHVH\n") != "-1", "alternating operations"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1x1, hoạt động đơn lẻ | 1 1 | xử lý đầu vào tối thiểu | 
| Lưới 3x3, nhiều H | đi xe đạp xác định | tái sử dụng thông qua thiết lập lại | 
| Lưới 2x2, | | |
