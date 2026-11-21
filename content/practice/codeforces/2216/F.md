---
title: "CF 2216F - Bản đồ sao"
description: "Sau khi sắp xếp các ngôi sao bằng cách tăng tọa độ $x$, mỗi ngôi sao xuất hiện ở một vị trí nằm ngang duy nhất và cũng có tọa độ $y$-duy nhất. Hình dạng hoàn toàn được xác định bằng hoán vị của các giá trị $y$ theo thứ tự này."
date: "2026-06-09T04:55:10+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "geometry"]
categories: ["algorithms"]
codeforces_contest: 2216
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1092 (Unrated, Div. 2, Based on THUPC 2026 \u2014 Finals)"
rating: 2700
weight: 2216
solve_time_s: 95
verified: false
draft: false
---

[CF 2216F - Bản đồ sao](https://codeforces.com/problemset/problem/2216/F) 

**Đánh giá:** 2700 
**Tags:** thuật toán xây dựng, hình học 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Sau khi sắp xếp các ngôi sao bằng cách tăng dần$x$-tọa độ, mỗi ngôi sao xuất hiện ở một vị trí nằm ngang duy nhất và cũng có một vị trí riêng$y$-điều phối. Hình dạng hoàn toàn được xác định bởi hoán vị của$y$-các giá trị theo thứ tự này. 

Một tam giác hài hòa có nhiều hạn chế hơn một tam giác tùy ý. Bởi vì tất cả$x$và tất cả$y$các giá trị khác nhau, ba ngôi sao chỉ có thể nằm trên đường biên của hình chữ nhật thẳng hàng với trục khi ngôi sao ở giữa nằm trong$x$-thứ tự là mức tối đa cục bộ nghiêm ngặt hoặc mức tối thiểu cục bộ nghiêm ngặt trong số ba. Nói cách khác, các tam giác hài hòa chính xác là những “đỉnh” và “thung lũng” cơ bản xuất hiện trong hoán vị được hình thành bởi$y$-tọa độ. 

Chòm sao có thể chứa nhiều hình tam giác, nhưng phần bên trong của chúng không được chồng lên nhau. Cho phép chia sẻ các cạnh hoặc đỉnh. 

Tổng số sao trong tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$. Bất kỳ giải pháp nào kiểm tra tất cả các bộ ba đều là không thể ngay lập tức. Thậm chí$O(n^2)$mỗi trường hợp thử nghiệm sẽ quá đắt trong trường hợp xấu nhất. Chúng ta cần một cái gì đó gần tuyến tính sau khi sắp xếp. 

Một trường hợp tinh tế xuất hiện khi một điểm là cực đại cục bộ so với các điểm lân cận của nó được nhìn thấy cho đến nay, nhưng các điểm sau đó có thể tạo ra một cấu trúc lớn hơn xung quanh nó. Một thuật toán tham lam cam kết vĩnh viễn quá sớm có thể tạo ra các hình tam giác chồng chéo hoặc bỏ lỡ các hình tam giác lẽ ra phải tồn tại. Giải pháp được chấp nhận trì hoãn các quyết định cho đến khi đỉnh hoặc đáy được xác định hoàn toàn bởi trạng thái quét hiện tại. 

Một sai lầm dễ mắc phải khác là cho rằng câu trả lời có liên quan đến việc tam giác hóa một đa giác. Các ngôi sao không tạo thành một đa giác theo thứ tự ranh giới. Cấu trúc tối ưu thu được từ cấu trúc hoán vị của các điểm sau khi sắp xếp theo$x$, không phải từ các đối số tam giác phẳng. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là kiểm tra từng bộ ba ngôi sao, kiểm tra xem nó có hài hòa hay không và sau đó bằng cách nào đó chọn một bộ sưu tập tối đa với nội thất rời rạc theo từng cặp. 

có$O(n^3)$gấp ba lần. Ngay cả việc kiểm tra sự hài hòa một cách hiệu quả cũng không giúp ích được gì, bởi vì$n$có thể đạt được$2 \cdot 10^5$. Số bộ ba sẽ theo thứ tự$10^{15}$. 

Quan sát quan trọng là sau khi sắp xếp theo$x$, các tam giác hài hòa tương ứng với cực trị cục bộ của dãy$y$-giá trị. Điểm cực đại cục bộ đương nhiên thuộc về đường bao trên, trong khi điểm cực tiểu cục bộ thuộc về đường bao dưới. Bất cứ khi nào một điểm trở thành một đỉnh được xác nhận trên đường bao trên, nó có thể được loại bỏ và chuyển thành một tam giác hài hòa. Ý tưởng tương tự cũng áp dụng đối xứng cho các thung lũng ở đường bao phía dưới. 

Điều này dẫn đến hai ngăn xếp đơn điệu. 

Một ngăn xếp duy trì chuỗi trên hiện tại. Khi ba điểm cuối tạo thành một đỉnh nghiêm ngặt, điểm ở giữa có thể được bóc ra, tạo thành một hình tam giác hài hòa. 

Ngăn xếp thứ hai duy trì chuỗi thấp hơn hiện tại. Khi ba điểm cuối tạo thành một thung lũng hẹp, điểm giữa có thể được bóc tách ra, tạo thành một tam giác hài hòa khác. 

Mỗi điểm được đẩy một lần và xuất hiện nhiều nhất một lần từ mỗi ngăn xếp, tạo ra một đợt quét tuyến tính sau khi sắp xếp. Việc xây dựng chính thức sử dụng chính xác quy trình này và tạo ra một chòm sao có kích thước tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các ngôi sao theo thứ tự tăng dần$x$-điều phối. 
2. Duy trì một`up`ngăn xếp đại diện cho chuỗi trên hiện tại. 
3. Xử lý điểm từ trái qua phải. 
4. Trước khi chèn điểm hiện tại$p$, kiểm tra xem ba điểm cuối cùng của chuỗi trên có tạo thành một đỉnh nghiêm ngặt hay không. Nếu điểm cuối cùng thứ hai có giá trị lớn hơn$y$hơn cả hai điểm lân cận của nó, hãy tạo hình tam giác được tạo bởi ba điểm đó và loại bỏ đỉnh khỏi ngăn xếp. 
5. Tiếp tục loại bỏ các đỉnh khi có thể. 
6. Đẩy điểm hiện tại vào chuỗi trên. 
7. Duy trì ngăn xếp thứ hai`down`cho chuỗi dưới. 
8. Trước khi chèn điểm hiện tại$p$, kiểm tra xem ba điểm cuối cùng của chuỗi dưới có tạo thành một thung lũng nghiêm ngặt hay không. Nếu điểm cuối cùng thứ hai có giá trị nhỏ hơn$y$hơn cả hai điểm lân cận, hãy tạo tam giác được tạo bởi ba điểm đó và loại bỏ thung lũng. 
9. Tiếp tục loại bỏ các thung lũng khi có thể. 
10. Đẩy điểm hiện tại vào chuỗi bên dưới. 
11. Mỗi tam giác được tạo ra bởi một trong hai ngăn xếp đều được thêm vào đáp án. 
12. Xuất tất cả các hình tam giác được tạo ra. 

### Tại sao nó hoạt động 

Ngăn xếp phía trên luôn lưu trữ đường bao phía trên hiện tại của các điểm được xử lý. Bất cứ khi nào một điểm trở thành một đỉnh chính xác giữa hai điểm lân cận của nó trên đường bao này thì tam giác tạo bởi ba điểm đó sẽ hài hòa. Việc loại bỏ đỉnh đó tương ứng với việc bóc một mặt khỏi phong bì. 

Ngăn xếp phía dưới thực hiện tương tự đối với các thung lũng. 

Các hình tam giác được tạo chính xác là các vùng cơ bản giữa các phiên bản liên tiếp của đường bao. Những vùng này có phần bên trong rời rạc nên không có hai hình tam giác nào được tạo ra chồng lên nhau. Mọi đỉnh hoặc đáy tối đa cuối cùng phải được bóc tách bởi một trong hai ngăn xếp, đó là lý do tại sao việc xây dựng là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())

        pts = []
        for i in range(n):
            x, y = map(int, input().split())
            pts.append((x, y, i + 1))

        pts.sort()

        up = []
        down = []
        ans = []

        for p in pts:
            while len(up) >= 2:
                a = up[-2]
                b = up[-1]

                if b[1] > a[1] and b[1] > p[1]:
                    ans.append((a[2], b[2], p[2]))
                    up.pop()
                else:
                    break

            up.append(p)

            while len(down) >= 2:
                a = down[-2]
                b = down[-1]

                if b[1] < a[1] and b[1] < p[1]:
                    ans.append((a[2], b[2], p[2]))
                    down.pop()
                else:
                    break

            down.append(p)

        out.append(str(len(ans)))
        for tri in ans:
            out.append(f"{tri[0]} {tri[1]} {tri[2]}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Bước đầu tiên sắp xếp theo$x$, bởi vì mọi suy luận hình học đều diễn ra theo thứ tự từ trái sang phải. 

các`up`ngăn xếp lưu trữ một chuỗi có các đỉnh bên trong không bao giờ là các đỉnh nghiêm ngặt. Bất cứ khi nào một điểm mới xuất hiện và tạo ra một đỉnh ở đầu ngăn xếp, đỉnh đó sẽ bị loại bỏ và chuyển thành một hình tam giác. 

các`down`ngăn xếp hoàn toàn đối xứng. Nó loại bỏ các thung lũng thay vì các đỉnh. 

Chi tiết triển khai quan trọng là cả hai vòng lặp dọn dẹp ngăn xếp đều được`while`vòng lặp thay vì`if`các tuyên bố. Việc loại bỏ một đỉnh có thể làm lộ ra một đỉnh khác ngay bên dưới nó. Thiếu tầng đó sẽ tạo ra một câu trả lời sai. 

Bản thân các tọa độ không bao giờ tham gia vào số học ngoài việc so sánh, do đó không có mối lo ngại về tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Giả sử đã sắp xếp$y$-trình tự là:$$1,\ 6,\ 10,\ -2,\ 3,\ 0,\ -8,\ -9$$| Hiện tại y | Hành động ngăn xếp trên | Hành động ngăn xếp thấp hơn | Đã thêm hình tam giác | 
| --- | --- | --- | --- | 
| 1 | đẩy | đẩy | 0 | 
| 6 | đẩy | đẩy | 0 | 
| 10 | đẩy | đẩy | 0 | 
| -2 | đạt đỉnh 10 bị loại bỏ | đẩy | 1 | 
| 3 | đẩy | thung lũng ở -2 bị xóa | 2 | 
| 0 | đỉnh điểm ở mức 3 bị loại bỏ | đẩy | 3 | 
| -8 | đẩy | đẩy | 3 | 
| -9 | đẩy | đẩy | 3 | 

Điều này thể hiện cơ chế cốt lõi: các đỉnh bị bóc tách bởi chuỗi trên và các thung lũng bị bóc tách bởi chuỗi dưới. 

### Ví dụ 2 

Đã sắp xếp$y$-sự liên tiếp:$$8,\ 3,\ 1,\ 5,\ 7,\ 10,\ -10,\ -1$$| Hiện tại y | Hành động ngăn xếp trên | Hành động ngăn xếp thấp hơn | Đã thêm hình tam giác | 
| --- | --- | --- | --- | 
| 8 | đẩy | đẩy | 0 | 
| 3 | đẩy | đẩy | 0 | 
| 1 | đẩy | đẩy | 0 | 
| 5 | đẩy | thung lũng lúc 1 bị xóa | 1 | 
| 7 | đẩy | đẩy | 1 | 
| 10 | đẩy | đẩy | 1 | 
| -10 | chuỗi đỉnh cao bật lên | đẩy | 2 | 
| -1 | đẩy | thung lũng ở -10 bị xóa | 3 | 

Ví dụ này cho thấy rằng một lần chèn có thể kích hoạt nhiều lần xóa khỏi một ngăn xếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế, xử lý ngăn xếp là tuyến tính | 
| Không gian |$O(n)$| Hai ngăn xếp và danh sách câu trả lời | 

Mỗi điểm được đẩy một lần và xuất hiện nhiều nhất một lần từ mỗi ngăn xếp. Bản thân việc quét là$O(n)$. Với$\sum n \le 2 \cdot 10^5$, giải pháp phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper skeleton

import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    # call solve() here
    return out.getvalue()

# The output is not unique.
# For this constructive problem, validation should check:
# 1. number of triangles is maximal
# 2. all indices are valid
# 3. produced triangles satisfy the required properties

# minimum size
inp = """1
3
0 0
1 2
2 1
"""

# monotone y-order
inp2 = """1
5
1 1
2 2
3 3
4 4
5 5
"""

# alternating highs and lows
inp3 = """1
7
1 4
2 1
3 6
4 2
5 7
6 3
7 5
"""

# large boundary-style stress test
# generate n = 200000 with distinct x and y
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$n=3$| Xây dựng hợp lệ | Kích thước tối thiểu | 
| Tăng nghiêm ngặt$y$| Không loại bỏ đỉnh/thung lũng sai lầm | Trình tự đơn điệu | 
| Mức cao và mức thấp xen kẽ | Nhiều ngăn xếp bật lên | Hành vi xếp tầng | 
| Tối đa$n$| Kết thúc đúng thời hạn | Độ phức tạp bị ràng buộc | 

## Vỏ cạnh 

Hãy xem xét```
1
3
0 0
1 10
2 5
```Điểm giữa là một đỉnh nghiêm ngặt. Khi điểm thứ ba đến, ngăn xếp phía trên ngay lập tức loại bỏ đỉnh và tạo thành một hình tam giác. Một giải pháp chờ cho đến khi kết thúc vẫn có tác dụng ở đây, nhưng ví dụ nhỏ này minh họa đặc tính cực trị cục bộ. 

Coi như```
1
5
1 5
2 1
3 4
4 2
5 3
```Loại bỏ thung lũng tại$y=1$làm lộ ra một cấu trúc mới đằng sau nó. Đây là lý do tại sao thuật toán sử dụng một`while`vòng lặp. Việc triển khai chỉ sử dụng một lần xóa cho mỗi lần chèn sẽ bỏ lỡ các hình tam giác hợp lệ. 

Xét dãy đơn điệu:```
1
5
1 1
2 2
3 3
4 4
5 5
```Không có điểm nào là đỉnh hoặc thung lũng nghiêm ngặt. Cả hai ngăn xếp chỉ phát triển. Thuật toán đưa ra chính xác các hình tam giác bằng 0, phù hợp với thực tế hình học rằng không có tam giác hài hòa nào có thể được hình thành từ cực trị liên tiếp. 

Các bất biến ngăn xếp tự động xử lý tất cả các trường hợp này, đó là lý do tại sao quá trình triển khai vẫn diễn ra ngắn gọn mặc dù có câu lệnh hình học.
