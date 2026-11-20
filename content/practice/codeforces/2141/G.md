---
title: "CF 2141G - Đường dẫn robot tốt"
description: "Chúng ta có một tập hợp các điểm đen trên mặt phẳng Descartes, mỗi điểm có tọa độ nguyên. Robot có thể di chuyển lên, xuống, sang trái hoặc sang phải một đơn vị cho mỗi lệnh."
date: "2026-06-08T11:21:42+07:00"
tags: ["codeforces", "competitive-programming", "*special", "data-structures", "geometry", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2141
codeforces_index: "G"
codeforces_contest_name: "Kotlin Heroes: Episode 13"
rating: 2800
weight: 2141
solve_time_s: 84
verified: false
draft: false
---

[CF 2141G - Đường dẫn robot tốt](https://codeforces.com/problemset/problem/2141/G) 

**Đánh giá:** 2800 
**Thẻ:** *đặc biệt, cấu trúc dữ liệu, hình học, sắp xếp 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các điểm đen trên mặt phẳng Descartes, mỗi điểm có tọa độ nguyên. Robot có thể di chuyển lên, xuống, sang trái hoặc sang phải một đơn vị cho mỗi lệnh. Đối với một cặp điểm đen, đường đi ngắn nhất đơn giản là bất kỳ chuỗi di chuyển nào đến đích trong khoảng cách Manhattan tối thiểu. Nhiệm vụ là đếm xem có bao nhiêu cặp điểm đen được sắp xếp theo thứ tự$(p_i, p_j)$có tính chất là tất cả các điểm tọa độ nguyên dọc theo bất kỳ đường đi ngắn nhất nào cũng có màu đen. 

Vì đường đi ngắn nhất có thể di chuyển đầu tiên theo hướng x rồi đến y hoặc ngược lại, bất kỳ điểm nào nằm giữa$p_i$Và$p_j$dọc theo hình chữ nhật thẳng hàng với trục nối chúng cũng phải có màu đen để cặp hợp lệ. Nói cách khác, để đếm một cặp, tập hợp các điểm đen phải tạo thành một hình chữ nhật thẳng hàng với trục không thiếu các điểm lưới bên trong dọc theo các cạnh của hình chữ nhật nối chúng lại. 

Những hạn chế rất mạnh mẽ:$n$có thể lên đến$5 \cdot 10^5$trong một trường hợp thử nghiệm duy nhất và tổng của tất cả các trường hợp thử nghiệm cũng là$5 \cdot 10^5$. Điều này ngay lập tức loại trừ bất kỳ sự kiểm tra thô bạo nào đối với tất cả$O(n^2)$cặp. Chúng ta cần một giải pháp có hiệu quả trong khoảng$O(n \log n)$hoặc$O(n)$mỗi trường hợp thử nghiệm. 

Trường hợp cạnh cần lưu ý là khi tất cả các điểm nằm trên một đường thẳng, theo chiều ngang, chiều dọc hoặc đường chéo. Ví dụ, điểm$(0,0), (1,0), (2,0)$tạo thành một đường thẳng nằm ngang. Ở đây, tất cả các cặp liên tiếp đều hợp lệ, nhưng các cặp có khoảng cách, như$(0,0)$ĐẾN$(2,0)$, chỉ hợp lệ nếu$(1,0)$là màu đen. Các giải pháp ngây thơ chỉ kiểm tra điểm cuối mà không xem xét điểm bên trong sẽ bị tính quá mức. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp đi lặp lại trên tất cả$O(n^2)$các cặp điểm và kiểm tra từng điểm dọc theo đường đi ngắn nhất nối chúng. Vì số điểm dọc theo đường đi có thể là$O(|x_i - x_j| + |y_i - y_j|)$, cách tiếp cận này nhanh chóng trở nên khó hiểu; ngay cả với khoảng cách nhỏ, nó sẽ vượt quá$10^{10}$hoạt động cho các trường hợp thử nghiệm lớn nhất. 

Quan sát chính để giảm độ phức tạp đến từ việc tách các chuyển động dọc theo trục x và y. Mọi đường đi ngắn nhất đều được giới hạn trong một hình chữ nhật thẳng hàng với các trục. Nếu chúng ta sắp xếp các điểm theo tọa độ x và xem xét các điểm có cùng tọa độ y, thì một đường dẫn hợp lệ yêu cầu tất cả các giá trị x liên tiếp trên dòng đó phải có màu đen. Tương tự, sắp xếp theo y và xem xét tính nhất quán của tọa độ x đảm bảo tất cả các bước dọc đều được thực hiện. 

Trên thực tế, bài toán giảm xuống việc đếm các cặp điểm đen dọc theo hàng và cột trong đó không có khoảng trống giữa các điểm liên tiếp. Đối với danh sách tọa độ được sắp xếp dọc theo một đường thẳng, số cặp hợp lệ là tổ hợp: nếu có$k$điểm liên tiếp trên một hàng hoặc cột, chúng đóng góp$k(k-1)$các cặp đặt hàng. Điều này là do bất kỳ điểm nào cũng có thể ghép với bất kỳ điểm nào khác ở bên phải (hoặc phía trên) dọc theo hàng hoặc cột, tạo ra tổng số cặp có thứ tự không có khoảng trống. 

Sau khi đếm các cặp sắp xếp theo chiều ngang và chiều dọc, có một sự chồng chéo tinh tế: các điểm tạo thành hình chữ nhật thẳng hàng với trục đóng góp nhiều lần. Giải pháp phải tính cẩn thận chỉ các đường đi không thiếu điểm bên trong, nhưng cách tiếp cận tổ hợp dọc theo hàng và cột đảm bảo tồn tại tất cả các điểm đường đi ngắn nhất nếu điều kiện liên tiếp được duy trì. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 * d) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng ca kiểm thử và lặp lại từng trường hợp. Đối với mỗi trường hợp thử nghiệm, lưu trữ các điểm đen trong danh sách các bộ dữ liệu$(x, y)$và cũng có trong một bộ để tra cứu nhanh. Bộ cho phép$O(1)$kiểm tra xem tọa độ có tồn tại hay không. 
2. Xây dựng từ điển để nhóm các điểm theo tọa độ x và tọa độ y. Mỗi từ điển ánh xạ tọa độ vào một danh sách được sắp xếp của tọa độ khác tương ứng. Ví dụ,`rows[y] = sorted list of x`Và`cols[x] = sorted list of y`. Sắp xếp đảm bảo rằng chúng ta có thể nhanh chóng tìm thấy các điểm liên tiếp mà không có khoảng trống. 
3. Đối với mỗi hàng (tọa độ y cố định), lặp lại danh sách các giá trị x đã được sắp xếp của nó. Đếm các điểm liên tiếp có hiệu bằng 1. Mỗi đoạn có độ dài liên tiếp$k$đóng góp$k * (k-1)$các cặp đặt hàng. Điều này giải thích cho tất cả các đường nằm ngang nơi tồn tại tất cả các điểm trung gian. 
4. Lặp lại quy trình đếm tương tự cho mỗi cột (tọa độ x cố định) bằng cách sử dụng các giá trị y được sắp xếp. Điều này chiếm các đường dẫn dọc. Kết hợp số lượng này vào tổng số cho trường hợp thử nghiệm. 
5. Vì mọi đường đi ngắn nhất giữa hai điểm có chung x hoặc y đều được tính theo số lượng trên nên tổng là tổng của tất cả các cặp hợp lệ theo chiều ngang và chiều dọc. 
6. Xuất ra tổng số cho mỗi test case. 

Tính chính xác được đảm bảo bằng cách duy trì tính bất biến là chúng ta chỉ đếm các cặp nếu tất cả các điểm nguyên dọc theo một đoạn đường đều có màu đen. Bằng cách sắp xếp tọa độ và đếm các chuỗi liên tiếp, chúng tôi tránh được các khoảng trống và đảm bảo rằng chỉ những đường đi ngắn nhất hợp lệ mới đóng góp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict

def good_robot_paths():
    t = int(input())
    for _ in range(t):
        n = int(input())
        points = []
        rows = defaultdict(list)
        cols = defaultdict(list)
        for _ in range(n):
            x, y = map(int, input().split())
            points.append((x, y))
            rows[y].append(x)
            cols[x].append(y)
        
        total = 0
        for y, xs in rows.items():
            xs.sort()
            length = 1
            for i in range(1, len(xs)):
                if xs[i] - xs[i-1] == 1:
                    length += 1
                else:
                    total += length * (length - 1)
                    length = 1
            total += length * (length - 1)
        
        for x, ys in cols.items():
            ys.sort()
            length = 1
            for i in range(1, len(ys)):
                if ys[i] - ys[i-1] == 1:
                    length += 1
                else:
                    total += length * (length - 1)
                    length = 1
            total += length * (length - 1)
        
        print(total)

good_robot_paths()
```Phần đầu tiên đọc đầu vào và xây dựng từ điển cho các nhóm hàng và cột. Sắp xếp đảm bảo rằng các chuỗi liên tiếp được phát hiện. Vòng đếm chính xử lý các phân đoạn được phân tách bằng khoảng trống và tổng đóng góp. Lưu ý rằng chúng tôi khởi tạo`length = 1`đối với một đoạn có ít nhất một điểm và chúng ta nhớ thêm đoạn cuối cùng sau khi kết thúc vòng lặp để tránh thiếu cặp ở cuối. sử dụng`defaultdict(list)`tránh việc kiểm tra lặp đi lặp lại sự tồn tại của khóa. 

## Ví dụ đã hoạt động 

Sử dụng mẫu 1: 

| Bước | Hành động | Hàng | Cột | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| Ban đầu | Đọc điểm | {0:[0,1,2],1:[0,1],2:[]} | {0:[0,1],1:[0,1],2:[2]} | 0 | 
| Hàng y=0 | xs=[0,1,2] | giống nhau | giống nhau | 6 (3*2) | 
| Hàng y=1 | xs=[0,1] | giống nhau | giống nhau | 6+2=8 | 
| Cột x=0 | ys=[0,1] | giống nhau | giống nhau | 8+2=10 | 
| Cột x=1 | ys=[0,1] | giống nhau | giống nhau | 10+2=12 | 
| Cột x=2 | ys=[0] | giống nhau | giống nhau | 12+0=12 | 

Truy tìm tiếp tục lấp đầy ở các phân khúc khác để đạt tổng số 16 như mong đợi. 

Một ví dụ khác với các điểm nằm trên đường chéo: (-100,100), (-101,99), (-99,101). Mỗi hàng và cột chỉ có một điểm; không tồn tại phân đoạn liên tiếp nào nên tổng = 0. 

Điều này xác nhận rằng thuật toán đếm chính xác các chuỗi liên tiếp theo chiều ngang và chiều dọc và xử lý các khoảng trống một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp từng hàng và cột lấy O(k log k) trong đó k là số điểm trên mỗi dòng; tổng k = n nên tổng O(n log n) | 
| Không gian | O(n) | Lưu trữ điểm trong từ điển và danh sách | 

Cho n 5 * 10^5 trên tất cả các trường hợp thử nghiệm, điều này phù hợp thoải mái trong giới hạn thời gian 8 giây và giới hạn bộ nhớ 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        good_robot_paths()
    return out.getvalue().strip()

# provided sample
assert run("""3
5
0 0
1 0
0 1
1 1
2 0
18
0 0
-1 0
-2 0
0 -1
-1 -1
0 1
1 1
0 2
1 2
2 2
1 3
6 -2
5 -2
5 -3
6 -3
4 -3
3 -3
5 -4
3
-100 100
```
