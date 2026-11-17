---
title: "CF 1985D - Vòng tròn Manhattan"
description: "Chúng ta được cung cấp một lưới gồm các dấu chấm và dấu băm. Ở đâu đó trong lưới này có một hình được hình thành bởi tất cả các ô có khoảng cách từ Manhattan đến tâm ẩn hoàn toàn nhỏ hơn bán kính. Điều này tạo ra một vùng hình kim cương thẳng hàng với các trục lưới."
date: "2026-06-08T16:19:43+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1985
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 952 (Div. 4)"
rating: 900
weight: 1985
solve_time_s: 89
verified: true
draft: false
---

[CF 1985D - Vòng tròn Manhattan](https://codeforces.com/problemset/problem/1985/D) 

**Xếp hạng:** 900 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới gồm các dấu chấm và dấu băm. Ở đâu đó trong lưới này có một hình được hình thành bởi tất cả các ô có khoảng cách từ Manhattan đến tâm ẩn hoàn toàn nhỏ hơn bán kính. Điều này tạo ra một vùng hình kim cương thẳng hàng với các trục lưới. Mỗi ô bên trong vùng này được đánh dấu bằng`#`, và mọi thứ bên ngoài đều là`.`. Nhiệm vụ là khôi phục tọa độ nguyên của tâm của viên kim cương này. 

Vòng tròn Manhattan có cấu trúc rất cứng nhắc. Nếu bạn di chuyển từ tâm, ranh giới sẽ mở rộng bằng nhau theo bốn hướng chéo, tạo thành một hình thoi đối xứng. Tính đối xứng này là thuộc tính quan trọng cho phép xây dựng lại từ lưới mà không cần biết bán kính. 

Các ràng buộc cho phép tổng số lên tới 200.000 ô trong tất cả các trường hợp thử nghiệm, do đó, bất kỳ giải pháp nào kiểm tra từng ô với số lần không đổi đều có thể chấp nhận được. Bất kỳ phương trình bậc hai nào trong một trường hợp thử nghiệm sẽ quá chậm nếu lưới lớn. 

Một sai lầm ngây thơ là cố gắng mô phỏng hoặc khớp các vòng tròn bằng cách kiểm tra mọi tâm và bán kính có thể có. Đối với mỗi trung tâm ứng viên, người ta sẽ cần phải xác minh tất cả`#`các ô thỏa mãn điều kiện Manhattan, điều này làm cho độ phức tạp khoảng O(n2m2) trong trường hợp xấu nhất. Một cách tiếp cận sai lầm khác là giả sử tâm là trung điểm hình học của tất cả`#`tế bào mà không có lý do chính đáng. Điều đó có thể thất bại nếu hình dạng không đối xứng hoàn hảo trong không gian tọa độ nhưng vẫn hợp lệ như một quả bóng Manhattan. 

Một cạm bẫy cụ thể là giả sử tâm là tâm của hộp giới hạn của`#`. Điều đó có tác dụng ở đây, nhưng chỉ khi bạn hiểu tại sao quả bóng Manhattan luôn bão hòa đều cả bốn hướng cực trị. 

## Phương pháp tiếp cận 

Một chiến lược bạo lực sẽ là coi mỗi tế bào như một trung tâm tiềm năng. Đối với mỗi ứng viên`(i, j)`, chúng tôi tính toán khoảng cách tối đa của Manhattan tới bất kỳ`#`di động và kiểm tra xem tất cả`#`nằm trong một bán kính nhất quán. Điều này yêu cầu quét toàn bộ lưới trên mỗi ứng viên, dẫn đến hành vi O((nm)²) trong trường hợp xấu nhất, quá chậm ngay cả đối với các lưới vừa phải. 

Quan sát quan trọng là một quả bóng Manhattan có tâm ở`(h, k)`có hình dạng ranh giới rất cụ thể. Trên cùng`#`tế bào nằm chính xác ở khoảng cách`r-1`phía trên phần giữa, phần dưới cùng ở cùng khoảng cách bên dưới và tương tự cho phần ngoài cùng bên trái và ngoài cùng bên phải. Điều này có nghĩa là tâm có thể được phục hồi hoàn toàn từ tọa độ cực trị của`#`tế bào. 

Cho phép`min_row`,`max_row`,`min_col`,`max_col`là hộp giới hạn của tất cả`#`tế bào. Quả bóng Manhattan hoàn toàn đối xứng, do đó tâm phải nằm chính xác ở nửa giữa cực trên và cực dưới và nửa giữa cực trái và cực phải. Điểm giữa đó được đảm bảo là số nguyên vì cấu trúc của hình thoi đảm bảo sự liên kết chẵn lẻ. 

Điều này làm giảm vấn đề xuống còn một lần truyền qua lưới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Kiểm tra trung tâm Brute Force | O((nm)2) | O(1) | Quá chậm | 
| Điểm giữa của hộp giới hạn | O(nm) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bốn biến`min_row`,`max_row`,`min_col`,`max_col`để theo dõi các vị trí cực đoan của tất cả`#`tế bào. Chúng tôi bắt đầu chúng ở các thái cực đối lập nhau để bất kỳ ô hợp lệ nào cũng cập nhật chúng một cách chính xác. 
2. Quét từng ô trong lưới theo hàng. Khi chúng ta gặp phải một`#`, cập nhật tất cả bốn giới hạn bằng tọa độ của nó. 
3. Sau khi xử lý lưới, hãy tính hàng giữa là`(min_row + max_row) // 2`. Điều này hiệu quả vì viên kim cương đối xứng theo chiều dọc, do đó tâm nằm chính xác ở giữa điểm cao nhất và thấp nhất`#`. 
4. Tính cột giữa là`(min_col + max_col) // 2`sử dụng cùng một đối số đối xứng. 
5. Xuất kết quả`(center_row, center_col)`. 

### Tại sao nó hoạt động 

Vòng tròn Manhattan được xác định bởi tất cả các điểm có khoảng cách từ Manhattan đến tâm hoàn toàn nhỏ hơn bán kính cố định. Điều này tạo ra một viên kim cương lồi trong đó mọi cực trị ngang và dọc được xác định duy nhất bởi tâm. Trên cùng`#`chính xác là`r-1`bậc phía trên trung tâm, bậc dưới cùng là`r-1`các bước bên dưới và tương tự cho bên trái và bên phải. Vì hình dạng đã được điền đầy đủ nên không`#`nằm ngoài những thái cực này và ít nhất một nằm trên mỗi ranh giới. Do đó, hộp giới hạn được căn giữa chính xác tại`(h, k)`, làm cho điểm giữa của các cực bằng với tâm thực. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        
        min_r = n
        max_r = 1
        min_c = m
        max_c = 1
        
        for i in range(1, n + 1):
            row = input().strip()
            for j, ch in enumerate(row, start=1):
                if ch == '#':
                    if i < min_r:
                        min_r = i
                    if i > max_r:
                        max_r = i
                    if j < min_c:
                        min_c = j
                    if j > max_c:
                        max_c = j
        
        center_r = (min_r + max_r) // 2
        center_c = (min_c + max_c) // 2
        
        print(center_r, center_c)

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì bốn biến biên và cập nhật chúng trong một lần thực hiện. Việc sử dụng lập chỉ mục dựa trên 1 phù hợp trực tiếp với báo cáo vấn đề, tránh chuyển đổi từng cái một. Phép chia số nguyên cho 2 là an toàn vì đường tròn Manhattan đảm bảo tính đối xứng nên tổng các cực trị luôn là số chẵn. 

Một chi tiết tinh tế là chúng tôi không bao giờ cố gắng tái tạo lại bán kính một cách rõ ràng. Bán kính được mã hóa ngầm theo khoảng cách mà hộp giới hạn kéo dài tính từ tâm, nhưng nó không cần thiết cho đầu ra. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 5
.....
.....
..#..
.....
.....
```| Bước | phút_r | max_r | phút_c | max_c | 
| --- | --- | --- | --- | --- | 
| sau khi quét | 3 | 3 | 3 | 3 | 

duy nhất`#`là ở (3, 3), nên mọi giới hạn đều sụp đổ về điểm đó. 

Đầu ra:```
3 3
```Điều này xác nhận thuật toán xử lý chính xác trường hợp bán kính suy biến-0. 

### Ví dụ 2 

đầu vào:```
5 5
..#..
.###.
#####
.###.
..#..
```| Bước | phút_r | max_r | phút_c | max_c | 
| --- | --- | --- | --- | --- | 
| sau khi quét | 1 | 5 | 1 | 5 | 

Tính toán trung tâm:`(1 + 5) // 2 = 3`,`(1 + 5) // 2 = 3`Đầu ra:```
3 3
```Điều này chứng tỏ rằng ngay cả khi hình dạng lớn, tính đối xứng cực đại vẫn xác định chính xác tâm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ô được xử lý một lần cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Chỉ có bốn biến số nguyên được duy trì | 

Tổng số ô trên tất cả các trường hợp thử nghiệm được giới hạn bởi 200.000, do đó, một lần quét tuyến tính cho mỗi trường hợp thử nghiệm dễ dàng phù hợp với giới hạn thời gian. Việc sử dụng bộ nhớ không đổi ngoài bộ nhớ đầu vào. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    
    solve()
    
    return out.getvalue().strip()

def solve():
    import sys
    input = sys.stdin.readline
    
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        
        min_r = n
        max_r = 1
        min_c = m
        max_c = 1
        
        for i in range(1, n + 1):
            row = input().strip()
            for j, ch in enumerate(row, start=1):
                if ch == '#':
                    min_r = min(min_r, i)
                    max_r = max(max_r, i)
                    min_c = min(min_c, j)
                    max_c = max(max_c, j)
        
        print((min_r + max_r)//2, (min_c + max_c)//2)

# provided samples
assert run("""6
5 5
.....
.....
..#..
.....
.....
5 5
..#..
.###.
#####
.###.
..#..
5 6
......
......
.#....
###...
.#....
1 1
#
5 6
...#..
..###.
.#####
..###.
...#..
2 10
..........
...#......""") == "3 3\n3 3\n4 2\n1 1\n3 4\n2 4"

# custom cases
assert run("""1
1 1
#""") == "1 1", "single cell"

assert run("""1
3 3
###
###
###""") == "2 2", "full grid"

assert run("""1
3 5
..#..
.###.
..#..""") == "2 3", "centered diamond"

assert run("""1
4 4
....
.##.
.##.
....""") == "2 2", "small square-like cluster"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1`#`| 1 1 | lưới tối thiểu | 
| khối 3x3 đầy đủ | 2 2 | hộp giới hạn thống nhất | 
| viên kim cương nhỏ | 2 3 | phục hồi trung tâm đối xứng | 
| Khối 2x2 bên trong lưới | 2 2 | an toàn từng người một | 

## Vỏ cạnh 

Một lưới tối thiểu chứa một`#`rất quan trọng vì cả tọa độ tối thiểu và tối đa đều thu gọn ngay lập tức. Thuật toán đặt tất cả các giới hạn cho vị trí đó và điểm giữa không thay đổi, tạo ra tâm chính xác. 

Một khối hình chữ nhật được lấp đầy có hình dạng khác với một viên kim cương Manhattan, nhưng vẫn có giá trị theo chế độ bảo hành. Trong trường hợp đó, hộp giới hạn là chính xác và điểm giữa vẫn nằm ở tâm đối xứng thực sự. 

Những viên kim cương mỏng, trong đó hình dạng chỉ kéo dài một vài hàng hoặc cột, kiểm tra xem phép chia số nguyên có hoạt động chính xác hay không. Bởi vì các vòng tròn Manhattan luôn có các khoảng cách cực trị đối xứng nên tổng tọa độ tối thiểu và tối đa vẫn bằng nhau, ngăn ngừa các vấn đề làm tròn.
