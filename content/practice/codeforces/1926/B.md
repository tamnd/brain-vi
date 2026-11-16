---
title: "CF 1926B - Vlad và Shapes"
description: "Chúng ta được cung cấp một hình ảnh nhị phân rất nhỏ, thường nhiều nhất là 10 x 10 ô. Mọi ô đều trống hoặc đầy. Lưới chứa chính xác một hình hình học được kết nối được tạo thành bởi những hình đó và hình đó được đảm bảo là một hình vuông hoàn hảo hoặc một hình tam giác ở giữa (thẳng đứng hoặc…"
date: "2026-06-08T18:58:43+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1926
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 928 (Div. 4)"
rating: 800
weight: 1926
solve_time_s: 82
verified: true
draft: false
---

[CF 1926B - Vlad và Shapes](https://codeforces.com/problemset/problem/1926/B) 

**Đánh giá:** 800 
**Tags:** hình học, thực hiện 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hình ảnh nhị phân rất nhỏ, thường nhiều nhất là 10 x 10 ô. Mọi ô đều trống hoặc đầy. Lưới chứa chính xác một hình hình học được kết nối được tạo thành bởi các hình và hình đó được đảm bảo là một hình vuông hoàn hảo hoặc một hình tam giác ở giữa (thẳng đứng hoặc lộn ngược). 

Hình vuông ở đây có nghĩa là một khối k x k đặc gồm các khối thẳng hàng với lưới, không có khoảng trống bên trong. Hình tam giác có nghĩa là một chồng các hàng trong đó số hàng tăng hoặc giảm đối xứng hai hàng trên một hàng, tạo thành hình kim tự tháp ở giữa. 

Nhiệm vụ là xác định hình nào trong hai hình có mặt. 

Ràng buộc n 10 loại bỏ mọi nhu cầu tối ưu hóa. Ngay cả việc quét toàn diện tất cả các hàng, cột và giả thuyết hình dạng cũng đủ nhanh. Một giải pháp có thể tự do kiểm tra từng tế bào nhiều lần. 

Trường hợp chính xuất phát từ việc dựa vào các mẫu cục bộ thay vì cấu trúc toàn cầu. Ví dụ: hàng trên cùng của hình tam giác và hình vuông có thể trông giống nhau nếu nó chứa các hàng liên tiếp, nhưng đặc điểm xác định của hình tam giác là độ dài hàng thay đổi, trong khi hình vuông giữ độ dài hàng không đổi trên tất cả các hàng của hình. Một trường hợp tinh vi khác là hình tam giác lộn ngược, trong đó hàng rộng nhất nằm ở trên cùng thay vì ở dưới cùng. 

Cách tiếp cận bất cẩn thường thất bại khi chỉ kiểm tra hàng đầu tiên hoặc đếm tổng số hàng mà không xác minh cấu trúc. Một hình tam giác và hình vuông đôi khi có thể có tổng tương tự nhau, ví dụ: cả hai đều có thể có 9 số, nhưng chỉ có một số khớp với mẫu hàng được yêu cầu. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để giải quyết vấn đề là thử xác định hộp giới hạn của tất cả những số 1 và sau đó kiểm tra xem nó có tạo thành một hình vuông hoàn hảo hay nó tạo thành một hình tam giác với một cột ở giữa. 

Đối với kiểm tra hình vuông, chúng tôi xác minh rằng tất cả các ô bên trong hộp giới hạn đều là một ô. Để kiểm tra hình tam giác, chúng tôi thử từng hàng đỉnh và cột trung tâm có thể có, đồng thời xác thực xem độ dài hàng có khớp với 1, 3, 5, v.v. hay mẫu ngược lại cho hình tam giác ngược. Điều này hiệu quả vì lưới rất nhỏ nên ngay cả việc kiểm tra mọi tâm và chiều cao có thể cũng không tốn kém. 

Sự kém hiệu quả của lực lượng vũ phu chỉ xuất hiện nếu lưới lớn, bởi vì chúng tôi sẽ thử nhiều trung tâm có thể. Ở đây, vì n 10, nên ngay cả hành vi O(n³) hoặc O(n⁴) cũng không đáng kể. 

Sự đơn giản hóa chính là chúng ta thực sự không cần phải “đoán” cấu trúc. Chúng tôi có thể trực tiếp phát hiện xem tất cả những cái đó có tạo thành một hình chữ nhật đầy hay không. Nếu có thì nó phải là hình vuông. Nếu không, cấu trúc hợp lệ duy nhất còn lại là một hình tam giác và cấu trúc của nó buộc phải căn chỉnh một tâm duy nhất, điều này có thể được xác thực bằng cách kiểm tra tính đối xứng của hàng xung quanh điểm giữa. 

Điều này làm giảm vấn đề xuống việc xác minh mẫu đơn giản hơn là tái tạo hình dạng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phù hợp với hình dạng Brute Force | O(n⁴) | O(1) | Đã chấp nhận | 
| Kiểm tra mẫu trực tiếp | O(n²) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi dựa vào một quan sát đơn giản: hình vuông là hình duy nhất trong đó mỗi hàng bên trong hình có cùng số hàng liên tiếp, trong khi hình tam giác luôn có độ dài hàng khác nhau.

1. Quét lưới và xác định vị trí tất cả các vị trí có chứa các vị trí đó. Chúng tôi ghi lại các chỉ số hàng và cột tối thiểu và tối đa có chứa một. Điều này cho chúng ta hình chữ nhật giới hạn của hình dạng. 
2. Đếm xem có bao nhiêu cái tồn tại bên trong hình chữ nhật bao quanh này. Nếu số lượng đơn vị bằng diện tích hình chữ nhật thì mọi ô bên trong đều được lấp đầy, nghĩa là hình đó là một hình chữ nhật đặc. 
3. Nếu chúng ta có một hình chữ nhật đặc, chúng ta kiểm tra xem chiều cao của nó có bằng chiều rộng hay không. Nếu vậy, hình đó là hình vuông, vì bài toán đảm bảo chỉ tồn tại hình vuông hoặc hình tam giác. 
4. Nếu hình chữ nhật bao quanh không được lấp đầy thì cấu trúc phải là hình tam giác. Trong trường hợp đó, chúng tôi ngầm xác minh điều đó bằng cách thừa nhận rằng vùng được lấp đầy không thể là hình chữ nhật trừ khi nó là hình vuông, do đó cấu hình hợp lệ duy nhất còn lại là hình tam giác. 
5. Xuất kết quả tương ứng. 

Việc triển khai tránh việc tái tạo tam giác rõ ràng vì vấn đề đảm bảo tính hợp lệ của các hình dạng đầu vào. 

### Tại sao nó hoạt động 

Một hình vuông có đặc điểm độc đáo là có độ dài hàng không đổi trên tất cả các hàng của hộp giới hạn và chiếm hết tất cả các ô. Bất kỳ sai lệch nào so với mức chiếm toàn bộ sẽ ngay lập tức phá vỡ thuộc tính hình chữ nhật và cấu trúc hợp lệ duy nhất còn lại trong các ràng buộc của bài toán là hình tam giác. Vì đầu vào được đảm bảo chứa chính xác một hình dạng hợp lệ nên việc phân loại theo tính hoàn chỉnh của hình chữ nhật là đủ và không thể phân loại sai một đầu vào hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    g = [input().strip() for _ in range(n)]
    
    cells = [(i, j) for i in range(n) for j in range(n) if g[i][j] == '1']
    
    min_r = min(i for i, j in cells)
    max_r = max(i for i, j in cells)
    min_c = min(j for i, j in cells)
    max_c = max(j for i, j in cells)
    
    total_ones = len(cells)
    area = (max_r - min_r + 1) * (max_c - min_c + 1)
    
    if total_ones == area and (max_r - min_r) == (max_c - min_c):
        print("SQUARE")
    else:
        print("TRIANGLE")
```Giải pháp này hoạt động bằng cách trích xuất hộp giới hạn chặt chẽ xung quanh tất cả các hộp và kiểm tra xem nó có được lấp đầy hoàn hảo hay không. Nếu đúng như vậy thì hình đó phải là hình vuông vì chỉ có hình vuông mới chiếm trọn hình chữ nhật. Mặt khác, cấu hình hợp lệ duy nhất còn lại dưới các ràng buộc của bài toán là một hình tam giác. 

Một điểm tinh tế là chúng ta không bao giờ cố gắng tái tạo lại hình học tam giác một cách rõ ràng. Điều đó tránh được các sai sót do hiểu sai các hình tam giác lộn ngược hoặc các trường hợp lệch tâm, vì sự đảm bảo đảm bảo không tồn tại các hỗn hợp mơ hồ. 

## Ví dụ đã hoạt động 

Hãy xem xét một hình vuông 3 x 3:```
111
111
111
```| Bước | phút_r | max_r | phút_c | max_c | những cái | khu vực | quyết định | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| quét | 0 | 2 | 0 | 2 | 9 | 9 | kiểm tra vuông | 
| kiểm tra | 0 | 2 | 0 | 2 | 9 | 9 | VUÔNG | 

Hộp giới hạn được lấp đầy hoàn toàn và có hình vuông nên kết quả là SQUARE. 

Bây giờ hãy xem xét một hình tam giác:```
00111
00010
00000
00000
00000
```| Bước | phút_r | max_r | phút_c | max_c | những cái | khu vực | quyết định | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| quét | 0 | 1 | 2 | 4 | 5 | 6 | kiểm tra tam giác | 
| kiểm tra | 0 | 1 | 2 | 4 | 5 | 6 | TAM GIÁC | 

Hộp giới hạn không được điền đầy đủ nên không thể là hình vuông mà phải là hình tam giác. 

Những dấu vết này cho thấy thuật toán dựa hoàn toàn vào tính hoàn chỉnh về cấu trúc hơn là tái cấu trúc hình học. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Chúng tôi quét từng ô một lần để thu thập các ô và tính giới hạn | 
| Không gian | O(1) | Chỉ các tọa độ và bộ đếm giới hạn được lưu trữ | 

Kích thước lưới tối đa là 100 ô cho mỗi trường hợp thử nghiệm, vì vậy giải pháp này khá nhanh trong điều kiện ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

def solve():
    import sys
    input = sys.stdin.readline
    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        g = [input().strip() for _ in range(n)]
        cells = [(i, j) for i in range(n) for j in range(n) if g[i][j] == '1']
        min_r = min(i for i, j in cells)
        max_r = max(i for i, j in cells)
        min_c = min(j for i, j in cells)
        max_c = max(j for i, j in cells)
        ones = len(cells)
        area = (max_r - min_r + 1) * (max_c - min_c + 1)
        if ones == area and (max_r - min_r) == (max_c - min_c):
            res.append("SQUARE")
        else:
            res.append("TRIANGLE")
    print("\n".join(res))

# provided samples
assert run("""6
3
000
011
011
4
0000
0000
0100
1110
2
11
11
5
00111
00010
00000
00000
00000
10
0000000000
0000000000
0000000000
0000000000
0000000000
1111111110
0111111100
0011111000
0001110000
0000100000
3
111
111
111
""") == """SQUARE
TRIANGLE
SQUARE
TRIANGLE
TRIANGLE
SQUARE"""

# small custom case
assert run("""1
3
111
101
111
""") == "TRIANGLE"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hình vuông đầy đủ | VUÔNG | phát hiện hình chữ nhật hoàn hảo | 
| hình chữ nhật vỡ | TAM GIÁC | loại bỏ vùng không rắn | 

## Vỏ cạnh 

Trường hợp cạnh khóa là một hình tam giác có hộp giới hạn vẫn là hình chữ nhật. Ví dụ: một hình tam giác rộng có thể có một hình chữ nhật bao quanh trông gần như được lấp đầy nhưng lại thiếu các ô bên trong. Trong trường hợp như vậy, việc kiểm tra khu vực không thành công vì số ô nhỏ hơn tổng số ô trong hộp, phân loại chính xác nó là TAM GIÁC. 

Một trường hợp cạnh khác là kích thước hình dạng hợp lệ tối thiểu là 2. Hình vuông 2 x 2 vượt qua cả kiểm tra độ đầy và hình vuông, trong khi bất kỳ hình tam giác 2 hàng nào nhất thiết phải để lại các khoảng trống hoặc độ dài hàng không đồng đều, khiến nó không đạt được điều kiện hình chữ nhật.
