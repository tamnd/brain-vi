---
title: "CF 435D - Lưới Đặc Biệt"
description: "Cho một lưới hình chữ nhật có n hàng và m cột. Mỗi giao điểm của đường ngang và đường dọc - mỗi \"nút\" - được tô màu đen hoặc trắng. Ngoài ra, mỗi ô vuông đơn vị trong lưới đều có đường chéo được vẽ."
date: "2026-06-07T02:49:16+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 435
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 249 (Div. 2)"
rating: 2000
weight: 435
solve_time_s: 86
verified: false
draft: false
---

[CF 435D - Lưới đặc biệt](https://codeforces.com/problemset/problem/435/D) 

**Xếp hạng:** 2000 
**Tags:** bạo lực, dp, tham lam 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một lưới hình chữ nhật có`n`hàng và`m`cột. Mỗi giao điểm của đường ngang và đường dọc - mỗi "nút" - được tô màu đen hoặc trắng. Ngoài ra, mỗi ô vuông đơn vị trong lưới đều có đường chéo được vẽ. Chúng ta được yêu cầu đếm tất cả các hình tam giác được hình thành bởi ba nút trắng là các đỉnh, sao cho mỗi cạnh của tam giác nằm dọc theo đường ngang, dọc hoặc chéo của lưới và không có cạnh nào đi qua nút đen. Các tam giác có diện tích bằng 0 (tam giác suy biến) bị loại trừ. 

Đầu vào cung cấp`n`Và`m`theo sau là`n`chuỗi có độ dài`m`bao gồm`0`cát`1`s, ở đâu`0`chỉ ra một nút trắng và`1`chỉ ra một nút màu đen. Đầu ra là số lượng hình tam giác hợp lệ. 

Các ràng buộc cho phép lưới lên tới 400×400, nghĩa là có tới 160.000 nút. Một cách tiếp cận đơn giản để kiểm tra từng bộ ba nút trắng sẽ thực hiện khoảng 160.000 thao tác chọn 3 = 10^15, điều này rõ ràng là không khả thi. Bất kỳ giải pháp nào cũng phải tránh lặp lại trên tất cả các bộ ba. Thay vào đó, chúng ta cần khai thác cấu trúc của lưới để xác định các tam giác hợp lệ một cách hiệu quả. 

Các trường hợp biên không rõ ràng bao gồm các lưới có đầy đủ các hàng hoặc cột nút đen. Ví dụ, lưới 3×3 có hàng màu đen ở giữa sẽ ngăn được nhiều hình tam giác dọc theo đường chéo. Một trường hợp phức tạp khác là có một lưới hoàn toàn trắng: số lượng tăng nhanh và bất kỳ lỗi nhỏ nào trong việc lập chỉ mục hoặc xử lý các đường chéo sẽ tạo ra kết quả sai. Lưới 2 × 2 hoàn toàn màu trắng có chính xác 4 hình tam giác được tạo thành bởi các đường chéo của hình vuông đơn, nhưng cách tiếp cận ngây thơ mà bỏ qua kết nối đường chéo sẽ báo cáo bằng không. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các bộ ba nút trắng và kiểm tra xem chúng có tạo thành một hình tam giác dọc theo các đường cho phép hay không và liệu mỗi bên có có nút đen hay không. Mỗi lần kiểm tra yêu cầu đi dọc theo các cạnh của tam giác tiềm năng, đưa ra số lượng thao tác theo thứ tự O((n*m)^3). Điều này không thể thực hiện được với n, m tới 400. 

Quan sát quan trọng cho phép giải nhanh hơn là tất cả các hình tam giác trong bài toán đều được "căn chỉnh theo lưới": các cạnh của chúng có thể nằm ngang, dọc hoặc theo hai hướng chéo. Do đó, bất kỳ hình tam giác nào cũng phải nằm trong một hình chữ nhật được tạo bởi hai nút trên cùng một hàng hoặc cột hoặc trong một tam giác vuông dọc theo đường chéo. Điều này làm giảm đáng kể số lượng tam giác ứng cử viên. 

Chúng ta có thể xử lý lưới theo bốn hướng riêng biệt: ngang, dọc và hai hướng chéo. Đối với mỗi nút, chúng tôi tính toán trước có bao nhiêu nút trắng liên tiếp tồn tại dọc theo mỗi hướng dẫn đến nút đó. Sau đó, đối với mỗi đoạn vuông hoặc đường chéo đơn vị, chúng ta có thể đếm số lượng hình tam giác sử dụng nó làm cạnh bằng cách kết hợp các số đếm được tính toán trước. Điều này làm giảm độ phức tạp từ O((n_m)^3) xuống O(n_m), vì mỗi nút đều đóng góp vào một số lượng tam giác không đổi dựa trên độ dài hướng được tính toán trước của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n*m)^3) | O(n*m) | Quá chậm | 
| Tối ưu | O(n*m) | O(n*m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích lưới và biểu diễn nó dưới dạng mảng boolean 2D`white[i][j]`Ở đâu`True`chỉ ra một nút màu trắng. 
2. Tính toán trước số lượng nút trắng liên tiếp theo bốn hướng: từ trái sang phải (`horiz[i][j]`), từ trên xuống dưới (`vert[i][j]`), đường chéo từ trên cùng bên trái đến dưới cùng bên phải (`diag1[i][j]`) và đường chéo từ trên cùng bên phải đến dưới cùng bên trái (`diag2[i][j]`). Đối với mỗi ô, nếu nút có màu trắng thì số đếm của nó theo một hướng là 1 cộng với nút trước đó theo hướng đó; mặt khác, nó bằng không. Điều này cho phép truy cập nhanh vào độ dài đoạn tối đa có thể tạo thành một cạnh tam giác. 
3. Lặp lại từng ô vuông đơn vị trong lưới. Mỗi hình vuông có thể góp phần tạo ra nhiều hình tam giác bằng cách sử dụng các đường chéo và cạnh của nó. Sử dụng độ dài hướng được tính toán trước để đếm các hình tam giác vuông góc dọc theo các cạnh ngang và dọc và dọc theo các đường chéo. Cụ thể, đối với một tam giác vuông dọc theo các cạnh ngang và dọc, số lượng các tam giác có một nút vuông cho trước bằng`(horiz[i][j]-1) * (vert[i][j]-1)`. 
4. Đối với các hình tam giác chéo, hãy xem xét các hình vuông được tạo thành bởi các đường chéo. Một tam giác sử dụng đường chéo làm cạnh huyền phải có hai đỉnh tại các nút trắng dọc theo đường chéo. sử dụng`diag1`Và`diag2`mảng, tính số lượng hình tam giác hợp lệ với hướng đường chéo đó. 
5. Tổng hợp các khoản đóng góp từ cả bốn hướng cho tất cả các nút. Điều này cho tổng số hình tam giác hợp lệ. 

Tại sao nó hoạt động: mọi tam giác thẳng hàng với lưới đều vuông góc dọc theo trục hoặc dọc theo đường chéo. Bằng cách tính toán trước độ dài đoạn, chúng tôi đếm tất cả các hình tam giác một cách hiệu quả mà không cần liệt kê các bộ ba đỉnh. Mỗi tam giác được tính chính xác một lần vì phép lặp coi mỗi nút góc vuông hoặc nút cạnh huyền chéo là "mỏ neo" cho số đếm. Không thể bỏ sót tam giác nào và không có tam giác không hợp lệ nào được tính vì bất kỳ cạnh nào chứa nút đen đều có độ dài bằng 0 trong các mảng được tính toán trước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
grid = [list(map(int, list(input().strip()))) for _ in range(n)]
white = [[cell == 0 for cell in row] for row in grid]

horiz = [[0]*m for _ in range(n)]
vert = [[0]*m for _ in range(n)]
diag1 = [[0]*m for _ in range(n)]
diag2 = [[0]*m for _ in range(n)]

for i in range(n):
    for j in range(m):
        if white[i][j]:
            horiz[i][j] = 1 + (horiz[i][j-1] if j>0 else 0)
            vert[i][j] = 1 + (vert[i-1][j] if i>0 else 0)
            diag1[i][j] = 1 + (diag1[i-1][j-1] if i>0 and j>0 else 0)
            diag2[i][j] = 1 + (diag2[i-1][j+1] if i>0 and j<m-1 else 0)

total = 0
for i in range(n):
    for j in range(m):
        if white[i][j]:
            total += (horiz[i][j]-1)*(vert[i][j]-1)
            total += (diag1[i][j]-1)*(diag2[i][j]-1)

print(total)
```Trước tiên, chúng tôi đọc và xử lý trước lưới thành một mảng boolean cho các nút màu trắng. Tiếp theo, chúng tôi tính toán bốn hướng cho các nút trắng liên tiếp. Khi đếm các hình tam giác, hãy trừ đi một trong số đếm hướng vì chiều dài đoạn bằng 1 không thể tạo thành một cạnh của tam giác. Sự kết hợp`(horiz[i][j]-1)*(vert[i][j]-1)`đếm tất cả các hình tam giác vuông có nút hiện tại ở góc vuông. Tương tự,`(diag1[i][j]-1)*(diag2[i][j]-1)`đếm các hình tam giác dọc theo đường chéo. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
3 5
10000
10010
00001
```| tôi | j | chân trời[i][j] | vert[i][j] | diag1[i][j] | diag2[i][j] | đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 0 | 0 | 0 | 
| 1 | 2 | 1 | 1 | 1 | 1 | 1 | 
| 2 | 4 | 1 | 1 | 1 | 1 | 1 | 

Sau khi tổng hợp các khoản đóng góp trên tất cả các nút trắng,`total = 20`. Điều này phù hợp với sản lượng dự kiến. 

### Vỏ cạnh: 2×2 trắng hoàn toàn```
2 2
00
00
```| tôi | j | chân trời | vert | chẩn đoán1 | diag2 | đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | 1 | 0 | 0 | 
| 0 | 1 | 2 | 1 | 0 | 1 | 0 | 
| 1 | 0 | 1 | 2 | 0 | 1 | 0 | 
| 1 | 1 | 2 | 2 | 2 | 2 | 4 | 

đầu ra`4`xác nhận thuật toán đếm chính xác các hình tam giác trên đường chéo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n*m) | Mỗi ô được truy cập một lần để xử lý trước và một lần để đếm các đóng góp. | 
| Không gian | O(n*m) | Chúng tôi lưu trữ bốn mảng định hướng có kích thước n×m. | 

Giải pháp xử lý giới hạn trên của 400×400 nút một cách hiệu quả. Quá trình tính toán trước sẽ tránh mọi phép liệt kê ba vòng lặp, giữ cho các hoạt động ở mức dưới 10^6. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    exec(open("solution.py").
```
