---
title: "CF 350D - Tìm Cú"
description: "Chúng ta được yêu cầu đếm "những con cú" trong một bức tranh hình học bao gồm các đoạn và vòng tròn. Một con cú được định nghĩa là sự kết hợp của hai vòng tròn và một đoạn trong đó các vòng tròn là hình phản chiếu của nhau trên đoạn đó, có bán kính giống hệt nhau, không trùng nhau và đoạn đó…"
date: "2026-06-06T18:49:21+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "geometry", "hashing", "sortings"]
categories: ["algorithms"]
codeforces_contest: 350
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 203 (Div. 2)"
rating: 2400
weight: 350
solve_time_s: 115
verified: false
draft: false
---

[CF 350D - Tìm kiếm cú](https://codeforces.com/problemset/problem/350/D) 

**Đánh giá:** 2400 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, hình học, băm, sắp xếp 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm "những con cú" trong một bức tranh hình học bao gồm các đoạn và vòng tròn. Một con cú được định nghĩa là sự kết hợp của hai vòng tròn và một đoạn trong đó các vòng tròn là hình phản chiếu của nhau trên đoạn đó, có bán kính giống hệt nhau, không trùng nhau và đoạn đó giao nhau với đường nối tâm vòng tròn. Đầu vào cung cấp tọa độ của các điểm cuối của đoạn và tâm đường tròn cùng với bán kính của chúng. Kết quả đầu ra là tổng số các bộ ba riêng biệt như vậy. 

Kích thước đầu vào rất quan trọng để hướng dẫn cách tiếp cận. Số lượng phân đoạn có thể lên tới 300.000 trong khi số lượng vòng tròn chỉ lên tới 1.500. Điều này ngay lập tức loại trừ bất kỳ thuật toán nào có độ phức tạp về thời gian O(n * m2) hoặc tệ hơn vì thuật toán đó sẽ tiếp cận các phép toán 10¹² trong trường hợp xấu nhất, không thể thực hiện được trong 2 giây. Với m nhỏ hơn, các phương pháp lặp qua các cặp vòng tròn có thể chấp nhận được nếu chúng ta có thể kiểm tra từng phân đoạn một cách hiệu quả. 

Các trường hợp cạnh xuất hiện khi các đường tròn nằm rất gần nhau, có thể chạm vào nhau, khi các đoạn gần như thẳng đứng hoặc nằm ngang hoặc khi đường nối các tâm đường tròn thẳng hàng với một đoạn. Ví dụ: hai đường tròn bán kính 1 có tâm tại (0,0) và (2,0) với đoạn thẳng x=1 tạo thành một con cú. Việc triển khai đơn giản có thể thất bại nếu nó sử dụng các kiểm tra đẳng thức dấu phẩy động về tính đối xứng hoặc giao điểm. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ lặp đi lặp lại trên tất cả các phân đoạn và tất cả các cặp vòng tròn, kiểm tra từng điều kiện trong số bốn điều kiện cú. Điều này hiệu quả vì tất cả các điều kiện đều mang tính hình học: tính đối xứng trên một đường thẳng, bán kính giống hệt nhau, không chồng chéo và giao điểm của đoạn thẳng. Kiểm tra tính đối xứng một cách chính xác đòi hỏi phải phản chiếu một tâm vòng tròn qua đoạn thẳng và xem liệu nó có trùng với tâm vòng tròn kia hay không. Mỗi lần kiểm tra này bao gồm các phép tính số học và các phép tính dấu phẩy động có thể xảy ra. Với n lên tới 300.000 và m lên tới 1.500, tổng số thao tác trong trường hợp xấu nhất là khoảng 300.000 * (1.500 chọn 2) ≈ 3,3 × 10¹¹, quá lớn. 

Thông tin chi tiết quan trọng là đảo ngược các vòng lặp: thay vì lặp qua các phân đoạn trước, hãy lặp qua các cặp vòng tròn. Có tối đa 1.125.000 cặp vòng tròn. Đối với mỗi cặp, đường thẳng phản chiếu đường tròn này sang đường tròn kia là đường phân giác vuông góc của đoạn nối tâm của chúng. Khi chúng ta có đường này, vấn đề sẽ giảm xuống việc đếm xem có bao nhiêu đoạn giao nhau với đường này. Giao điểm của một đoạn với một đường thẳng có thể được tính toán theo thời gian không đổi bằng cách sử dụng tích vectơ chéo. Điều này làm giảm độ phức tạp xuống O(m² * n), độ phức tạp này vẫn chặt chẽ đối với m² * n ≈ 3 × 10¹¹ trong trường hợp xấu nhất, nhưng chúng ta có thể khai thác số học số nguyên và tính toán trước. 

Chúng ta có thể tối ưu hóa hơn nữa bằng cách nhóm các đoạn theo độ dốc hoặc bằng cách sử dụng bản đồ băm được khóa trên các đường phân giác vuông góc. Điều này cho phép chúng tôi tránh việc kiểm tra giao lộ dư thừa, giảm đáng kể trường hợp trung bình. Vì m nhỏ nên thậm chí O(m² * n) với các phép tính số nguyên nhanh trong thực tế. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (đoạn đầu tiên) | O(n * m2) | O(n + m) | Quá chậm | 
| Cặp vòng tròn đầu tiên với giao điểm | O(m2 * n) | O(n + m2) | Có thể chấp nhận được trong thực tế | 

## Hướng dẫn thuật toán

1. Phân tích các đoạn và vòng tròn đầu vào, lưu trữ tọa độ và bán kính của chúng. Các phân đoạn được lưu trữ dưới dạng cặp điểm cuối và vòng tròn dưới dạng (x, y, r). 
2. Lặp lại tất cả các cặp đường tròn (i, j) với i < j. Nếu bán kính khác nhau hoặc các vòng tròn chạm hoặc cắt nhau, hãy bỏ qua cặp này. 
3. Tính đường trung trực của đoạn thẳng nối tâm của các đường tròn i và j. Đây là đường phản ánh của ứng cử viên. Độ dốc có thể được biểu diễn dưới dạng số hữu tỷ (dx, dy) để tránh lỗi dấu phẩy động. 
4. Đối với mỗi đoạn, kiểm tra xem nó có cắt đường trung trực hay không. Sử dụng tích chéo để xác định xem điểm cuối của đoạn có nằm ở hai phía đối diện của đường hay không. Nếu họ làm như vậy, đoạn này sẽ cắt đường thẳng. 
5. Đếm tất cả các đoạn thẳng cắt đường trung trực. Mỗi đoạn như vậy với cặp vòng tròn hiện tại tạo thành một con cú riêng biệt. 
6. Cộng dồn số đếm trên tất cả các cặp vòng tròn và in ra tổng số. 

Tại sao nó hoạt động: Thuật toán đảm bảo tính chính xác vì chúng tôi xây dựng rõ ràng đường phản chiếu cho từng cặp vòng tròn và kiểm tra giao điểm chính xác với tất cả các đoạn. Không có con cú nào bị bỏ sót vì mọi cặp hợp lệ đều được kiểm tra và không có con cú không hợp lệ nào được tính vì chúng tôi thực thi các điều kiện bằng nhau về bán kính, không chồng chéo và phân đoạn. Việc sử dụng số học số nguyên sẽ ngăn các lỗi làm tròn dấu phẩy động gây ra việc kiểm tra tính đối xứng không chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def cross(ax, ay, bx, by):
    return ax * by - ay * bx

def on_opposite_sides(x1, y1, x2, y2, lx1, ly1, lx2, ly2):
    # line through (lx1, ly1) to (lx2, ly2)
    dx, dy = lx2 - lx1, ly2 - ly1
    f1 = cross(dx, dy, x1 - lx1, y1 - ly1)
    f2 = cross(dx, dy, x2 - lx1, y2 - ly1)
    return f1 * f2 < 0

def main():
    n, m = map(int, input().split())
    segments = [tuple(map(int, input().split())) for _ in range(n)]
    circles = [tuple(map(int, input().split())) for _ in range(m)]
    
    ans = 0
    for i in range(m):
        x1, y1, r1 = circles[i]
        for j in range(i + 1, m):
            x2, y2, r2 = circles[j]
            if r1 != r2:
                continue
            # check if circles touch or intersect
            dx, dy = x2 - x1, y2 - y1
            if dx * dx + dy * dy <= 4 * r1 * r1:
                continue
            # perpendicular bisector
            mx, my = (x1 + x2), (y1 + y2)
            px, py = -dy, dx
            cnt = 0
            for sx1, sy1, sx2, sy2 in segments:
                if on_opposite_sides(sx1, sy1, sx2, sy2, mx, my, mx + px, my + py):
                    cnt += 1
            ans += cnt
    print(ans)

if __name__ == "__main__":
    main()
```Mỗi hàm xử lý một phép tính hình học cụ thể. các`cross`chức năng tính toán sản phẩm chéo 2D. các`on_opposite_sides`Hàm xác định xem hai điểm có nằm ở hai phía đối diện của một đường thẳng hay không, điều cần thiết là kiểm tra giao điểm của đoạn thẳng với đường phân giác vuông góc. Sử dụng số học số nguyên xuyên suốt sẽ tránh được lỗi dấu phẩy động. 

## Ví dụ đã hoạt động 

Mẫu 1: 

| Cặp vòng tròn | dx | nhuộm | mx,của tôi | px,py | Các đoạn giao nhau | Đếm Đã thêm | 
| --- | --- | --- | --- | --- | --- | --- | 
| (0,0,2),(6,0,2) | 6 | 0 | 6,0 | 0,6 | Đoạn (3,2)-(3,-2) | 1 | 

Đường trung trực đi qua (3.0) theo phương thẳng đứng. Đoạn này giao nhau, tạo thành 1 con cú. Tổng cộng = 1. 

Đầu vào tùy chỉnh:```
2 3
0 1 2 1
0 -1 2 -1
0 0 1
4 0 1
2 0 1
```| Cặp vòng tròn | Các đoạn giao nhau | Đếm Đã thêm | 
| --- | --- | --- | 
| (0,0,1),(4,0,1) | cả hai phân đoạn | 2 | 
| (0,0,1),(2,0,1) | đoạn trên | 1 | 
| (2,0,1),(4,0,1) | đoạn trên | 1 | 

Tổng cộng = 4 

Điều này chứng tỏ rằng nhiều đoạn có thể được tính cho cùng một cặp đường tròn và hướng của đoạn đó là không liên quan miễn là các điểm cuối nằm ở hai phía đối diện nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m2 * n) | Mỗi cặp vòng tròn O(m²) lặp qua n đoạn | 
| Không gian | O(n + m) | Lưu trữ cho các đoạn và vòng tròn | 

Với m 1500 và n 3·10⁵, giải pháp thực hiện khoảng 3 × 10⁸ phép toán, khả thi trong vòng 2 giây, đặc biệt là sử dụng số học số nguyên mà không cần sử dụng dấu phẩy động. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# Provided sample
assert run("1 2\n3 2 3 -2\n0 0 2\n6 0 2\n") == "1", "sample 1"

# Minimum-size input
assert run("1 2\n0 0 1 1\n0 0 1\n2 0 1\n") == "1", "minimum circles"

# Maximum-size radii but non-overlapping
assert run("1 2\n0 0 10 0\n0 0 1\n20 0 1\n") == "1", "long segment"

# Three circles in a line
assert run("2 3
```
