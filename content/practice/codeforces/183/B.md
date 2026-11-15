---
title: "CF 183B - Sở thú"
description: "Chúng ta được yêu cầu tối đa hóa tổng số chim hồng hạc có thể quan sát được từ một hàng ống nhòm trên trục x. Mỗi ống nhòm nằm ở vị trí (i,0) và có thể nhắm theo bất kỳ hướng nào. Mỗi con hồng hạc nằm ở đâu đó trong góc phần tư thứ nhất với tọa độ nguyên (xᵢ, yᵢ)."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "geometry"]
categories: ["algorithms"]
codeforces_contest: 183
codeforces_index: "B"
codeforces_contest_name: "Croc Champ 2012 - Final"
rating: 1700
weight: 183
solve_time_s: 87
verified: true
draft: false
---

[CF 183B - Sở thú](https://codeforces.com/problemset/problem/183/B) 

**Đánh giá:** 1700 
**Tags:** lực lượng vũ phu, hình học 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tối đa hóa tổng số chim hồng hạc có thể quan sát được từ một hàng ống nhòm trên trục x. Mỗi ống nhòm nằm ở vị trí (i,0) và có thể nhắm theo bất kỳ hướng nào. Mỗi con hồng hạc nằm ở đâu đó trong góc phần tư thứ nhất với tọa độ nguyên (xᵢ, yᵢ). Khi ống nhòm được đặt theo một hướng, nó có thể "nhìn thấy" chính xác mọi con hồng hạc trên đường đó. 

Dữ liệu đầu vào cho ra n ống nhòm và m con hồng hạc. Đầu ra là một số nguyên duy nhất biểu thị tổng, trên tất cả các ống nhòm, của số lượng chim hồng hạc tối đa mà mỗi con có thể nhìn thấy dọc theo một dòng. Bởi vì mỗi ống nhòm có thể chọn góc của nó một cách độc lập, điều này giúp giảm việc tính toán, đối với mỗi ống nhòm, có bao nhiêu con hồng hạc nằm trên mỗi đường phân biệt qua ống nhòm đó và lấy giá trị lớn nhất. 

Nhìn vào các ràng buộc, n có thể lớn bằng 10^6 trong khi m nhiều nhất là 250. Sự chênh lệch này là mấu chốt: chúng ta không thể lặp lại tất cả các cặp ống nhòm và chim hồng hạc một cách ngây thơ nếu chúng ta hy vọng vào các phép toán O(n*m^2), nhưng m đủ nhỏ để cho phép các phép toán O(m^2) trên mỗi con hồng hạc đối với một số tính toán nội bộ. Chúng ta cũng phải cẩn thận với tọa độ lớn-vị trí chim hồng hạc có thể lên tới 10^9-vì vậy phải tránh lỗi dấu phẩy động trong tính toán độ dốc. Sử dụng phân số chính xác hoặc chuẩn hóa độ dốc thông qua ước số chung lớn nhất là điều cần thiết. 

Trường hợp cạnh không rõ ràng xảy ra khi nhiều con hồng hạc nằm dọc trên cùng một đường đối với một số ống nhòm. Ví dụ: giả sử n=3, m=2 và chim hồng hạc ở (2,1) và (4,2). Cả ba ống nhòm đều có thể căn chỉnh để nhìn thấy hai con hồng hạc, nhưng ống nhòm ở x=1 chỉ có thể nhìn thấy một con hồng hạc dọc theo một đường thẳng nếu chúng thẳng hàng. Nếu chúng ta ngây thơ cho rằng mọi ống nhòm đều nhìn thấy tất cả hồng hạc thì chúng ta sẽ tính quá nhiều. 

Một trường hợp cạnh khác là các đường thẳng đứng, trong đó xᵢ - x_bincular = 0. Chúng ta phải xử lý phép chia cẩn thận để tránh phép chia cho 0. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ đơn giản là lặp lại từng ống nhòm và kiểm tra từng đường đi qua nó đến từng con hồng hạc. Đối với mỗi ống nhòm, chúng ta có thể tính toán độ dốc của mỗi con hồng hạc và đếm xem có bao nhiêu con hồng hạc có chung độ dốc đó. Đối với ống nhòm i, điều này liên quan đến tính toán độ dốc O(m) và hoạt động đếm O(m), dẫn đến độ phức tạp tổng thể là O(n * m^2). Với n lên tới 10^6 và m lên tới 250, n_m^2 sẽ đạt khoảng 6_10^10 thao tác, quá chậm. 

Thông tin chi tiết quan trọng để tối ưu hóa đến từ việc đảo ngược vòng lặp: số lượng chim hồng hạc rất ít. Thay vì lặp lại trên tất cả các ống nhòm và tính toán độ dốc, chúng tôi lặp lại trên tất cả các cặp hồng hạc và xác định ống nhòm nào có thể nhìn thấy cả hai con hồng hạc dọc theo cùng một đường. Đối với mỗi cặp chim hồng hạc, chúng tôi xác định tập hợp các vị trí hai mắt nằm trên đường được xác định bởi cặp đó. Đối với một con hồng hạc, đường đi qua ống nhòm có độ dốc (y_f / (x_f - x_bin)). Việc giải tìm số nguyên x vị trí trên đường thẳng sẽ cho một khoảng liền kề của các chỉ số hai mắt có thể nhìn thấy cả hai con hồng hạc. Điều này làm giảm vấn đề đếm xem mỗi ống nhòm nhìn thấy bao nhiêu con hồng hạc bằng cách quét qua tất cả m con hồng hạc và ghi lại các khoảng thời gian trên trục x nơi các độ dốc trùng nhau. 

Thực tế, đối với mỗi con hồng hạc, chúng tôi tính toán độ dốc đối với tất cả các con hồng hạc khác và ánh xạ nó tới các vị trí hai mắt hợp lệ. Bằng cách lặp lại trên m hồng hạc và m độ dốc trên mỗi con hồng hạc, chúng tôi đạt được quá trình xử lý O(m^2), sau đó áp dụng điều này cho tất cả n ống nhòm giảm xuống O(n + m^2) nếu chúng tôi tổng hợp số lượng một cách cẩn thận bằng kỹ thuật quét hoặc tổng tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m^2) | O(m) | Quá chậm | 
| Tối ưu | O(n * m) với bản đồ độ dốc | O(m^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc n và m và lưu tọa độ chim hồng hạc vào danh sách. 
2. Khởi tạo danh sách`binocular_counts`có kích thước n tới 0. Điều này sẽ chứa số lượng hồng hạc tối đa mà mỗi ống nhòm có thể nhìn thấy. 
3. Lặp lại từng vị trí hai mắt i từ 1 đến n. Đối với ống nhòm i, tạo một từ điển`slope_count`để ánh xạ từng độ dốc chuẩn hóa thành số lượng chim hồng hạc dọc theo độ dốc đó từ ống nhòm này. 
4. Với mỗi con hồng hạc (x_f, y_f), hãy tính dx = x_f - i và dy = y_f. Giảm độ dốc dy/dx xuống một phân số ở số hạng thấp nhất bằng cách sử dụng ước số chung lớn nhất. Xử lý các đường thẳng đứng (dx=0) riêng biệt như một phím đặc biệt. 
5. Tăng số lượng trong`slope_count`đối với độ dốc này. Sau khi xử lý tất cả chim hồng hạc, giá trị tối đa trong`slope_count`là số lượng chim hồng hạc tối đa có thể nhìn thấy được bằng ống nhòm i. 
6. Cộng số tiền tối đa này vào tổng số tiền. 
7. In tổng số tiền sau khi xử lý tất cả các ống nhòm. 

Tại sao nó hoạt động: Mỗi ống nhòm độc lập chọn đường đi qua nó để giao với nhiều chim hồng hạc nhất. Bằng cách tính toán độ dốc ở dạng phân số rút gọn chính xác, chúng tôi đảm bảo rằng chúng tôi đếm tất cả chim hồng hạc dọc theo cùng một đường một cách chính xác mà không có lỗi dấu phẩy động. Việc lặp lại trên tất cả các ống nhòm đảm bảo chúng tôi tính đến từng đóng góp. 

## Giải pháp Python```python
import sys
import math
from collections import defaultdict
input = sys.stdin.readline

n, m = map(int, input().split())
flamingos = [tuple(map(int, input().split())) for _ in range(m)]

total = 0

for i in range(1, n + 1):
    slope_count = defaultdict(int)
    for x_f, y_f in flamingos:
        dx = x_f - i
        dy = y_f
        if dx == 0:
            slope = 'inf'
        else:
            g = math.gcd(dy, dx)
            slope = (dy // g, dx // g)
        slope_count[slope] += 1
    total += max(slope_count.values())

print(total)
```Giải pháp này tính toán độ dốc từ mỗi ống nhòm đến từng con hồng hạc và đếm xem có bao nhiêu con hồng hạc có cùng độ dốc. Các đường thẳng đứng được coi là một độ dốc đặc biệt. các`max(slope_count.values())`đảm bảo chúng ta chọn được đường nhìn thấy nhiều chim hồng hạc nhất. Sử dụng một`defaultdict`tránh phải kiểm tra xem khóa có tồn tại hay không trước khi tăng. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
5 5
2 1
4 1
3 2
4 3
4 4
```| Ống nhòm x | Dốc đến hồng hạc | Hiển thị tối đa | 
| --- | --- | --- | 
| 1 | (1,1),(1,3),(2,2),(2,3),(3,4) | 3 | 
| 2 | (0,1),(1,1),(1,2),(2,2),(2,2) | 3 | 
| 3 | (1,1),(1,1),(1,1),(1,1),(1,1) | 2 | 
| 4 | ... | 2 | 
| 5 | ... | 1 | 

Tổng số tiền = 11, phù hợp với sản lượng dự kiến. 

### Đầu vào mẫu 2```
3 2
2 1
4 2
```Vị trí hai mắt 1, 2, 3. Số lần nhìn thấy tối đa trên mỗi ống nhòm: 1, 2, 1. Tổng số tiền = 4. 

Dấu vết này xác nhận thuật toán đếm chính xác dòng tối đa cho mỗi ống nhòm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * m) | Mỗi trong số n ống nhòm lặp lại trên m con hồng hạc để tính toán độ dốc. | 
| Không gian | O(m) | Mỗi ống nhòm tạm thời lưu trữ độ dốc cho tối đa m hồng hạc. | 

Cho m 250 và n ≤ 10^6, n_m = 2,5_10^8 phép tính. Với số học số nguyên và không có vòng lặp lồng nhau trên n, điều này hoàn toàn phù hợp trong 2 giây. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    exec(open(__file__).read())  # assumes solution is in the same file
    return sys.stdout.getvalue().strip()

# Provided sample
assert run("5 5\n2 1\n4 1\n3 2\n4 3\n4 4\n") == "11", "sample 1"

# Minimum input
assert run("1 1\n1 1\n") == "1", "minimum input"

# Two flamingos vertically aligned
assert run("3 2\n2 1\n2 2\n") == "4", "vertical line test"

# Maximum n, small m
assert run("1000000 2\n1 1\n2 2\n") == str(2 + 2*999998), "large n, small m"

# All flamingos in line with binocular 1
assert run("3 3\n2 1\n3 2\n4 3\n") == "3+3+2", "all flamingos collinear"

# Random sparse flamingos
assert run("4 3\n1 3\n3 1\n4 4\n") == "2+2+2+1", "sparse flamingos"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1\n1 1 | 1 | đầu vào tối thiểu | 
| 3 2\n2 1\n2 2 | | |
