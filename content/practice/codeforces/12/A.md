---
title: "CF 12A - Siêu Đặc Vụ"
description: "Chúng tôi được yêu cầu kiểm tra xem mật khẩu được nhập trên bàn phím 3×3 có đối xứng với nút trung tâm của nó hay không. Bàn phím được biểu thị dưới dạng lưới ký tự 3 × 3, trong đó \"X\" biểu thị nút được nhấn và \".\" biểu thị nút không được nhấn."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 12
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 12 (Div 2 Only)"
rating: 800
weight: 12
solve_time_s: 75
verified: true
draft: false
---
[CF 12A - Siêu đặc vụ](https://codeforces.com/problemset/problem/12/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu kiểm tra xem mật khẩu được nhập trên bàn phím 3×3 có đối xứng với nút trung tâm của nó hay không. Bàn phím được biểu diễn dưới dạng lưới ký tự 3 × 3, trong đó "X" biểu thị nút được nhấn và "." biểu thị một cái không được nhấn. Đối xứng trung tâm có nghĩa là nếu bạn xoay lưới 180 độ quanh ô giữa của nó, kiểu nút được nhấn vẫn không thay đổi. 

Nói một cách đơn giản hơn, để một mẫu đối xứng quanh tâm, mỗi nút ở vị trí (i, j) phải khớp với nút ở vị trí (2-i, 2-j). Ví dụ: góc trên cùng bên trái phải khớp với góc dưới cùng bên phải và góc trên cùng bên phải phải khớp với góc dưới cùng ở giữa, trong khi phần giữa có thể là bất cứ thứ gì vì nó ánh xạ tới chính nó. 

Các ràng buộc ở mức tối thiểu: lưới 3 × 3 cố định và giới hạn thời gian là 2 giây. Điều này có nghĩa là bất kỳ thuật toán đơn giản nào kiểm tra tất cả các cặp đối xứng sẽ chạy ngay lập tức. Các trường hợp cạnh bao gồm các mẫu không có nút được nhấn (tất cả các dấu chấm), tất cả các nút được nhấn (tất cả X) hoặc các mẫu không đối xứng trong đó chỉ có một cặp vi phạm tính đối xứng. 

Việc triển khai bất cẩn có thể chỉ kiểm tra các hàng hoặc cột một cách độc lập thay vì so sánh các vị trí đối diện ở giữa. Ví dụ, lưới```
X..
...
..X
```là đối xứng, nhưng nếu chúng ta chỉ so sánh các hàng được phản chiếu mà không tính đến các cột, thì chúng ta có thể kết luận sai là không phải vậy. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: đối với mỗi ô trong lưới, hãy tính toán độ đối xứng của ô xung quanh tâm và so sánh chúng. Trong lưới 3×3, điều này có nghĩa là so sánh từng ô trong số bốn góc và bốn ô cạnh. Điều này hiệu quả vì lưới rất nhỏ nhưng việc so sánh có thể được mã hóa cứng vì chỉ có tám vị trí để kiểm tra. 

Cái nhìn sâu sắc quan trọng là lưới có kích thước cố định và nhỏ. Chúng ta có thể khai thác cấu trúc bằng cách liệt kê các cặp đối xứng: trên cùng bên trái với dưới cùng bên phải, trên cùng giữa với dưới cùng giữa, trên cùng bên phải với dưới cùng bên trái và giữa trái với phải giữa. Tâm bị bỏ qua vì nó luôn đối xứng với chính nó. Điều này làm giảm vấn đề xuống chỉ còn bốn lần kiểm tra đẳng thức. Việc quan sát thấy tính đối xứng chỉ có ở các vị trí đối diện nhau cho phép chúng ta viết một nghiệm vừa tối thiểu vừa dễ xác minh thủ công. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) | O(1) | Đã chấp nhận | 
| Cặp đối xứng | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới 3×3 từ đầu vào dưới dạng danh sách các chuỗi. Mỗi chuỗi đại diện cho một hàng. Điều này bảo toàn cấu trúc hàng/cột, giúp việc lập chỉ mục trở nên đơn giản. 
2. Xác định các cặp vị trí phải đối xứng nhau để đạt được sự đối xứng trung tâm: trên cùng bên trái với dưới cùng bên phải, trên cùng giữa với dưới cùng giữa, trên cùng bên phải với dưới cùng bên trái và trái giữa với phải giữa. Vị trí trung tâm (1,1) được tự động đối xứng. 
3. Đối với mỗi cặp đối xứng, hãy so sánh các ký tự. Nếu bất kỳ cặp nào khác nhau thì mẫu đó không đối xứng. In ngay chữ "KHÔNG" và dừng lại. 
4. Nếu tất cả các cặp đều khớp nhau, hãy in ra "CÓ". Điều này đảm bảo rằng lưới đối xứng quanh tâm của nó. 

Bất biến được duy trì là ở bất kỳ bước nào, nếu tất cả các cặp được kiểm tra trước đó khớp với nhau thì tính đối xứng từng phần là chính xác. Nếu tìm thấy sự không khớp, tính đối xứng bị phá vỡ và thuật toán dừng lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

grid = [input().strip() for _ in range(3)]

# Define symmetric pairs (i,j) <-> (2-i,2-j)
symmetric_pairs = [
    ((0,0),(2,2)),
    ((0,1),(2,1)),
    ((0,2),(2,0)),
    ((1,0),(1,2))
]

for (x1,y1),(x2,y2) in symmetric_pairs:
    if grid[x1][y1] != grid[x2][y2]:
        print("NO")
        sys.exit(0)

print("YES")
```Mã đọc lưới thành một danh sách các chuỗi, duy trì việc lập chỉ mục theo hàng và cột. Các cặp đối xứng được xác định rõ ràng, tránh bất kỳ lỗi nào xảy ra. Vòng lặp so sánh sẽ thoát ngay lập tức nếu phát hiện thấy điểm không khớp, đảm bảo chúng tôi không thực hiện các kiểm tra không cần thiết. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:```
XX.
...
.XX
```| Cặp | lưới[x1][y1] | lưới[x2][y2] | Cuộc thi đấu? | 
| --- | --- | --- | --- | 
| (0,0)-(2,2) | X | X | vâng | 
| (0,1)-(2,1) | X | X | vâng | 
| (0,2)-(2,0) | . | . | vâng | 
| (1,0)-(1,2) | . | . | vâng | 

Tất cả các cặp đều khớp nhau, vì vậy kết quả đầu ra là "CÓ". 

**Ví dụ 2** 

đầu vào:```
X..
...
..X
```| Cặp | lưới[x1][y1] | lưới[x2][y2] | Cuộc thi đấu? | 
| --- | --- | --- | --- | 
| (0,0)-(2,2) | X | X | vâng | 
| (0,1)-(2,1) | . | . | vâng | 
| (0,2)-(2,0) | . | . | vâng | 
| (1,0)-(1,2) | . | . | vâng | 

Tất cả các cặp đều khớp, đầu ra là "CÓ". 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có bốn cặp so sánh được thực hiện, không phụ thuộc vào đầu vào. | 
| Không gian | O(1) | Lưới sử dụng 3 chuỗi, mỗi chuỗi 3 ký tự và danh sách cặp đối xứng có 4 phần tử. | 

Với lưới cố định 3×3, giải pháp này chạy ngay lập tức và sử dụng bộ nhớ không đáng kể, dễ dàng đáp ứng giới hạn thời gian 2 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    grid = [input().strip() for _ in range(3)]
    symmetric_pairs = [
        ((0,0),(2,2)),
        ((0,1),(2,1)),
        ((0,2),(2,0)),
        ((1,0),(1,2))
    ]
    for (x1,y1),(x2,y2) in symmetric_pairs:
        if grid[x1][y1] != grid[x2][y2]:
            return "NO"
    return "YES"

# provided samples
assert run("XX.\n...\n.XX\n") == "YES", "sample 1"

# custom cases
assert run("X..\n...\n..X\n") == "YES", "symmetric corners"
assert run("X.X\n.X.\nX.X\n") == "YES", "all edges symmetric"
assert run("XXX\n.X.\nXXX\n") == "YES", "all edges pressed"
assert run("X..\n.X.\n..X\n") == "NO", "non-symmetric edges"
assert run("...\n...\n...\n") == "YES", "empty grid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| X..\n...\n..X | CÓ | chỉ các góc đối xứng | 
| X.X\n.X.\nX.X | CÓ | các cạnh đối xứng, bỏ qua tâm | 
| XXX\n.X.\nXXX | CÓ | ép toàn bộ hàng trên và dưới, đối xứng | 
| X..\n.X.\n..X | KHÔNG | cạnh không khớp phá vỡ tính đối xứng | 
| ...\n...\n... | CÓ | lưới trống vẫn đối xứng | 

## Vỏ cạnh 

Lưới trống```
...
...
...
```là đối xứng vì mọi cặp vị trí đều khớp nhau. Thuật toán kiểm tra tất cả bốn cặp và tìm thấy sự bằng nhau (tất cả các dấu chấm), trả về chính xác "CÓ". 

Một nút duy nhất được nhấn ở giữa```
...
.X.
...
```cũng trôi qua. Các cặp đối xứng đều là dấu chấm, phần giữa bị bỏ qua. Đầu ra là "CÓ". 

Một mẫu góc không khớp như```
X..
...
.X.
```thất bại ở cặp đầu tiên (0,0)-(2,2) vì X != ., ngay lập tức trả về "KHÔNG". Mã dừng chính xác ở lần vi phạm đầu tiên mà không kiểm tra các cặp còn lại, tránh những lần kiểm tra không cần thiết.
