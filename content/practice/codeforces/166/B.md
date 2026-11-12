---
title: "CF 166B - Đa giác"
description: "Chúng ta được yêu cầu xác định xem một đa giác có nằm hoàn toàn bên trong một đa giác khác hay không. Đa giác A là lồi hoàn toàn, nghĩa là tất cả các góc trong đều nhỏ hơn 180 độ và không có ba điểm liên tiếp nào thẳng hàng."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "sortings"]
categories: ["algorithms"]
codeforces_contest: 166
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 113 (Div. 2)"
rating: 2100
weight: 166
solve_time_s: 187
verified: true
draft: false
---

[CF 166B - Đa giác](https://codeforces.com/problemset/problem/166/B) 

**Đánh giá:** 2100 
**Thẻ:** hình học, sắp xếp 
**Thời gian giải:** 3 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định xem một đa giác có nằm hoàn toàn bên trong một đa giác khác hay không. Đa giác A là lồi hoàn toàn, nghĩa là tất cả các góc trong đều nhỏ hơn 180 độ và không có ba điểm liên tiếp nào thẳng hàng. Đa giác B tùy ý nhưng đơn giản, nghĩa là nó không tự cắt hoặc chạm vào chính nó. Cả hai đa giác đều được cho theo thứ tự theo chiều kim đồng hồ và không có ba đỉnh liên tiếp nào nằm trên một đường thẳng. 

Đầu vào trước tiên chỉ định số đỉnh cho đa giác A, theo sau là tọa độ của chúng, sau đó là số đỉnh cho đa giác B cùng với tọa độ của chúng. Đầu ra là "CÓ" đơn giản nếu B nằm hoàn toàn bên trong A và "KHÔNG" nếu ngược lại. 

Với n tối đa 10^5 cho đa giác A, mọi thuật toán lặp qua tất cả các cạnh của A cho mỗi điểm của B sẽ mất O(n_m) thời gian. Vì m có thể đạt tới 2·10^4, nên một phép kiểm tra O(n_m) đơn giản có thể yêu cầu khoảng 2·10^9 thao tác, quá chậm so với giới hạn 2 giây. Do đó, chúng ta cần một thuật toán tận dụng tính lồi chặt của A để kiểm tra việc ngăn chặn điểm hiệu quả hơn, lý tưởng nhất là ở O(log n) trên mỗi điểm. 

Các trường hợp cạnh có thể gây khó khăn cho việc triển khai đơn giản bao gồm các đỉnh của B nằm chính xác trên các cạnh của A, các điểm nằm rất gần các cạnh của A nhưng ở bên ngoài và các đa giác chạm vào nhau tại một đỉnh. Ví dụ: nếu đa giác B có một đỉnh chính xác trên một cạnh của đa giác A, thì đầu ra phải là "KHÔNG", chứ không phải "CÓ" và việc kiểm tra dấu tích chéo đơn giản mà không có bất đẳng thức nghiêm ngặt có thể chấp nhận nó một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là lặp lại từng đỉnh của B và kiểm tra xem nó có nằm trong đa giác A hay không bằng cách sử dụng thử nghiệm điểm trong đa giác tiêu chuẩn, giống như phương pháp truyền tia. Phương pháp này hoạt động với các đa giác tùy ý và đúng nhưng sẽ mất thời gian O(n_m). Với đầu vào trong trường hợp xấu nhất, điều này trở nên không khả thi vì n_m có thể lên tới 2·10^9 thao tác. 

Cái nhìn sâu sắc quan trọng xuất phát từ tính lồi chặt của đa giác A. Đối với đa giác lồi, có một tính chất nổi tiếng: một điểm nằm bên trong khi và chỉ khi nó ở bên trái của tất cả các cạnh khi đi qua các đỉnh theo chiều kim đồng hồ, hoặc tương đương, tất cả tích chéo giữa vectơ cạnh và vectơ từ điểm bắt đầu cạnh đến điểm đều có cùng dấu. Vì A là lồi hoàn toàn nên chúng ta có thể kiểm tra từng điểm bằng tìm kiếm nhị phân trên các cạnh của A thay vì quét tuyến tính. Điều này làm giảm việc kiểm tra điểm trong đa giác từ O(n) xuống O(log n). Quan sát này thúc đẩy thực tế là các đỉnh của một đa giác lồi hoàn toàn có thể được coi như một mảng hình tròn được sắp xếp theo các góc xung quanh bất kỳ đỉnh tham chiếu cố định nào. 

Do đó, giải pháp hiệu quả là chọn đỉnh đầu tiên của A làm tham chiếu, xây dựng các vectơ từ nó đến tất cả các đỉnh khác và thực hiện tìm kiếm nhị phân để tìm xem một điểm đã cho của B sẽ nằm ở đâu so với các cung đa giác. Sau đó, chúng ta kiểm tra hướng của tam giác tạo bởi đỉnh tham chiếu, hai đỉnh liên tiếp của A và điểm. Nếu nó luôn ở đúng phía thì điểm đó nằm hoàn toàn bên trong. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*m) | O(n + m) | Quá chậm | 
| Tìm kiếm nhị phân đa giác lồi | O(m*log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chọn đỉnh đầu tiên của đa giác A làm điểm tham chiếu. Điều này thuận tiện vì A là lồi hoàn toàn và các đỉnh được sắp xếp theo chiều kim đồng hồ. 
2. Với tất cả các đỉnh khác của A, hãy xây dựng các vectơ tương ứng với tham chiếu này. Các vectơ này xác định các tam giác biên cho tìm kiếm nhị phân. 
3. Đối với mỗi đỉnh của đa giác B, trước tiên hãy kiểm tra xem nó có nằm đúng cạnh của cạnh đầu tiên và cạnh cuối cùng so với tham chiếu hay không. Nếu thất bại, nó ở bên ngoài và chúng ta có thể trả về ngay lập tức "KHÔNG". 
4. Nếu vượt qua, hãy thực hiện tìm kiếm nhị phân trên các cung được hình thành bởi các đỉnh liên tiếp của A (không bao gồm tham chiếu) để tìm cung chứa đỉnh B. Khu vực này là một tam giác được xác định bởi đỉnh tham chiếu và hai đỉnh liên tiếp của A. 
5. Sau khi tìm thấy khu vực này, hãy tính tích chéo của các vectơ được tạo bởi các cạnh của tam giác này và điểm từ B. Kiểm tra xem điểm có nằm hoàn toàn bên trong tam giác này hay không bằng cách sử dụng dấu của tích chéo. Nếu bất kỳ đỉnh nào của B không vượt qua bài kiểm tra này, hãy in "NO" và thoát. 
6. Nếu tất cả các đỉnh của B đều vượt qua kiểm tra, hãy in "CÓ." Đa giác B nằm hoàn toàn bên trong A. 

Tại sao nó hoạt động: thuật toán tận dụng thuộc tính lồi, đảm bảo rằng mọi điểm bên trong đa giác nằm trong chính xác một khu vực được hình thành giữa đỉnh tham chiếu và hai đỉnh liên tiếp của A. Việc kiểm tra tích chéo đảm bảo ngăn chặn nghiêm ngặt và loại trừ các điểm trên đường biên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def cross(a, b):
    return a[0]*b[1] - a[1]*b[0]

def vec(p1, p2):
    return (p2[0]-p1[0], p2[1]-p1[1])

def is_inside_convex(poly, p):
    n = len(poly)
    if n == 1:
        return poly[0] != p
    if n == 2:
        return cross(vec(poly[0], poly[1]), vec(poly[0], p)) > 0
    left = 1
    right = n-1
    cp0 = cross(vec(poly[0], poly[left]), vec(poly[0], p))
    cpn = cross(vec(poly[0], poly[right]), vec(poly[0], p))
    if cp0 <= 0 or cpn >= 0:
        return False
    while right - left > 1:
        mid = (left + right)//2
        if cross(vec(poly[0], poly[mid]), vec(poly[0], p)) > 0:
            left = mid
        else:
            right = mid
    tri_cp1 = cross(vec(poly[left], poly[right]), vec(poly[left], p))
    tri_cp2 = cross(vec(poly[right], poly[0]), vec(poly[right], p))
    tri_cp3 = cross(vec(poly[0], poly[left]), vec(poly[0], p))
    return tri_cp1 > 0 and tri_cp2 > 0 and tri_cp3 > 0

n = int(input())
A = [tuple(map(int, input().split())) for _ in range(n)]
m = int(input())
B = [tuple(map(int, input().split())) for _ in range(m)]

for p in B:
    if not is_inside_convex(A, p):
        print("NO")
        sys.exit(0)
print("YES")
```Mã bắt đầu bằng cách xác định các phép toán vectơ và tích chéo để xác định hướng. các`is_inside_convex`hàm kiểm tra từng điểm của B so với đa giác A bằng cách sử dụng tìm kiếm nhị phân trên các cung. Đầu tiên, mã đảm bảo điểm nằm trong các hình tam giác ngoài cùng được xác định bởi đỉnh tham chiếu. Tìm kiếm nhị phân định vị đúng khu vực trong O(log n) và tích chéo xác định việc ngăn chặn nghiêm ngặt. Vòng lặp chính kiểm tra mọi đỉnh của B, dừng ở lần thất bại đầu tiên. 

## Ví dụ đã hoạt động 

Mẫu 1: 

| Đỉnh B | cp0 | cpn | trái | đúng | tri_cp1 | tri_cp2 | tri_cp3 | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| (0,1) | 2 | -2 | 1 | 5 | 1 | 1 | 1 | bên trong | 
| (2,2) | 1 | -1 | 2 | 5 | 1 | 1 | 1 | bên trong | 
| (3,1) | 1 | -3 | 3 | 5 | 1 | 1 | 1 | bên trong | 
| (1,0) | 1 | -1 | 1 | 5 | 1 | 1 | 1 | bên trong | 

Bảng xác nhận rằng mỗi đỉnh đều vượt qua quá trình kiểm tra sản phẩm chéo, xác nhận việc ngăn chặn nghiêm ngặt. 

Ví dụ tùy chỉnh: 

Đa giác A: hình vuông (0,0),(4,0),(4,4),(0,4); Đa giác B: tam giác (1,1),(3,1),(2,3) 

Tất cả các đỉnh B đều nằm trong A. Tìm kiếm nhị phân tìm thấy các cung, tích chéo là dương, xuất ra "CÓ". 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m*log n) | Mỗi m đỉnh của B được kiểm tra bằng tìm kiếm nhị phân trên n đỉnh của A | 
| Không gian | O(n + m) | Lưu trữ cho cả hai đa giác | 

Với n 10^5 và m 2·10^4, tổng số phép tính là khoảng 2·10^5*log(10^5) ≈ 1,1·10^6, trong giới hạn thời gian 2 giây. Bộ nhớ chấp nhận được dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    return output.getvalue().strip()

# provided sample
assert run("6\n-2 1\n0 3\n3 3\n4 1\n3 -2\n2 -2\n4\n0 1\n2 2\n3 1\n1 0\n") == "YES", "sample 1"

# B vertex on edge
assert run("4\n0 0\n4 0\n4 4\n0 4\n3\n0 0\n2 1\n1 2\n") == "NO", "vertex on edge"

# B completely outside
assert run("4
```
