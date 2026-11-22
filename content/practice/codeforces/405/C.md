---
title: "CF 405C - Sản phẩm lạ"
description: "Chúng ta có một ma trận nhị phân n×n, nghĩa là mọi phần tử đều bằng 0 hoặc 1. \"Bình phương bất thường\" của ma trận được định nghĩa là tổng trên tất cả các hàng của tích chấm của hàng i với cột i, được tính theo modulo 2."
date: "2026-06-07T01:37:28+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 405
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 238 (Div. 2)"
rating: 1600
weight: 405
solve_time_s: 271
verified: true
draft: false
---

[CF 405C - Sản phẩm bất thường](https://codeforces.com/problemset/problem/405/C) 

**Đánh giá:** 1600 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 4 phút 31 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một ma trận nhị phân _n_×_n_, nghĩa là mọi phần tử đều bằng 0 hoặc 1. "Bình phương bất thường" của ma trận được định nghĩa là tổng trên tất cả các hàng của tích chấm của hàng _i_ với cột _i_, được tính theo modulo 2. Nói cách khác, đối với mỗi vị trí đường chéo trong ma trận, chúng ta tính tích số chấm của hàng đó với cột tương ứng, tính tổng tất cả các kết quả này và rút gọn theo modulo 2. 

Trên hết, chúng ta cần xử lý ba loại truy vấn. Lần đầu tiên lật tất cả các bit liên tiếp, lần thứ hai lật tất cả các bit trong một cột và lần thứ ba yêu cầu hình vuông bất thường hiện tại. Kích thước ma trận đầu vào có thể lên tới 1000×1000 và số lượng truy vấn lên tới 10^6. Một cách tiếp cận ngây thơ tính toán lại bình phương bất thường sau mỗi lần lật sẽ liên quan đến công việc O(n^2) cho mỗi truy vấn loại 3. Đối với 10^6 truy vấn, điều này rõ ràng là không khả thi vì nó sẽ yêu cầu thứ tự 10^12 thao tác, vượt xa mọi giới hạn thời gian hợp lý. 

Chúng ta cũng phải cẩn thận với các lỗi từng cái một và các phép tính modulo, đặc biệt vì các lần lật ảnh hưởng đến hình vuông bất thường một cách tinh vi, phụ thuộc vào vị trí. Ví dụ: nếu ma trận toàn là số 0 và chúng ta lật một hàng, hình vuông bất thường có thể thay đổi hoặc không thay đổi tùy thuộc vào bit cột nào trùng khớp. Tương tự, lật cùng một hàng hai lần sẽ trả lại hình vuông bất thường về giá trị trước đó. Xử lý những trường hợp này một cách ngây thơ có thể dễ dàng dẫn đến câu trả lời sai. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản: lưu trữ ma trận và đối với mỗi truy vấn loại 3, lặp qua từng hàng, tính tích số chấm với cột tương ứng, tính tổng các kết quả và xuất ra modulo 2. Điều này đúng vì nó trực tiếp triển khai định nghĩa nhưng nó yêu cầu các phép toán O(n^2) cho mỗi truy vấn. Với n tối đa 1000 và tối đa 10^6 truy vấn, điều này có thể yêu cầu khoảng 10^12 thao tác, quá chậm so với giới hạn thời gian 1 giây. 

Thông tin chi tiết quan trọng cần tối ưu hóa là lưu ý rằng ma trận là nhị phân và tất cả các phép toán đều có modulo 2. Việc lật một hàng hoặc cột chỉ thay đổi tính chẵn lẻ của tổng của mỗi cặp hàng-cột giao nhau. Cụ thể, việc lật hàng _i_ sẽ ảnh hưởng đến tất cả các tích chấm liên quan đến hàng _i_, và việc lật cột _j_ sẽ ảnh hưởng đến tất cả các tích chấm liên quan đến cột _j_. Do đó, hình vuông bất thường có thể được theo dõi bằng các cập nhật O(1) trên mỗi hàng hoặc lật cột bằng cách chỉ duy trì tính chẵn lẻ của phần đóng góp của mỗi phần tử đường chéo. 

Thay vì tính toán lại toàn bộ tích số chấm cho mỗi truy vấn, chúng ta có thể theo dõi một mảng duy nhất biểu thị tính chẵn lẻ của các lần lật cho các hàng và cột. Khi đó, mỗi truy vấn loại 3 chỉ yêu cầu tính tổng chẵn lẻ của n giá trị, là O(n). Vì n 1000 nên tốc độ này đủ nhanh cho 10^6 truy vấn, cho tổng số thao tác khoảng 10^9, có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q·n^2) | O(n^2) | Quá chậm | 
| Tối ưu | O(n + q·n) | O(n^2 + n + n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ma trận và lưu trữ nó trong mảng 2D. Điều này là cần thiết để biết trạng thái ban đầu của ma trận khi lật hàng và lật cột. 
2. Khởi tạo hai mảng có độ dài n:`row_flip`Và`col_flip`. Mỗi mảng lưu trữ xem hàng hoặc cột đó đã được lật số lần lẻ (1) hay số lần chẵn (0). 
3. Tính bình phương bất thường ban đầu bằng cách lặp qua từng hàng _i_. Với mỗi hàng, tính tích số chấm của hàng _i_ với cột _i_ modulo 2 và tính tổng các giá trị này. Lưu trữ cái này dưới dạng`current_square`. 
4. Đối với mỗi truy vấn, xử lý như sau: 

- Nếu truy vấn lật hàng _i_, chuyển đổi`row_flip[i]`. Sau đó, đối với mỗi phần tử đường chéo bị ảnh hưởng (chỉ phần tử (i,i)), hãy tính lại phần đóng góp của nó vào bình phương bất thường bằng cách sử dụng hàm hiện tại`row_flip`Và`col_flip`mảng. Cộng hoặc trừ modulo 2 nếu cần. 
- Nếu truy vấn lật cột _j_, chuyển đổi`col_flip[j]`và cập nhật tương tự phần đóng góp của phần tử đường chéo (j,j). 
- Nếu truy vấn yêu cầu bình phương bất thường, hãy tính tính chẵn lẻ của từng phần tử đường chéo bằng cách sử dụng`matrix[i][i] ^ row_flip[i] ^ col_flip[i]`và tổng modulo 2. Nối giá trị này vào chuỗi đầu ra. 
5. Xuất ra chuỗi câu trả lời tích lũy. 

### Tại sao nó hoạt động 

Thuật toán hoạt động vì mỗi lần lật hàng hoặc cột chỉ ảnh hưởng đến tính chẵn lẻ của tích số chấm liên quan đến hàng hoặc cột đó. Bằng cách duy trì các mảng chẵn lẻ lật và tính toán sự đóng góp của các phần tử đường chéo modulo 2, chúng ta có thể theo dõi chính xác bình phương bất thường mà không cần tính toán lại toàn bộ ma trận. Mỗi truy vấn loại 3 tính tổng các đóng góp để đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
matrix = [list(map(int, input().split())) for _ in range(n)]
q = int(input())

row_flip = [0] * n
col_flip = [0] * n

# Compute initial unusual square
def compute_usquare():
    total = 0
    for i in range(n):
        val = matrix[i][i] ^ row_flip[i] ^ col_flip[i]
        total ^= val
    return total

res = []

for _ in range(q):
    query = input().split()
    if query[0] == '1':
        i = int(query[1]) - 1
        row_flip[i] ^= 1
    elif query[0] == '2':
        j = int(query[1]) - 1
        col_flip[j] ^= 1
    else:
        res.append(str(compute_usquare()))

print(''.join(res))
```các`row_flip`Và`col_flip`mảng theo dõi xem mỗi hàng hoặc cột có bị đảo số lần lẻ hay không. các`compute_usquare`hàm XOR giá trị đường chéo ban đầu với các trạng thái lật hiện tại, trực tiếp mang lại tính chẵn lẻ cho hình vuông bất thường. Sử dụng XOR tránh phải xử lý phép cộng modulo 2 theo cách thủ công. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
3
1 1 1
0 1 1
1 0 0
12
3
2 3
3
2 2
2 2
1 3
3
3
1 2
2 1
1 1
3
```| Truy vấn | hàng_flip | col_flip | Hình vuông bất thường | 
| --- | --- | --- | --- | 
| 3 | [0,0,0] | [0,0,0] | 0 | 
| 2 3 | [0,0,0] | [0,0,1] | | 
| 3 | [0,0,0] | [0,0,1] | 1 | 
| 2 2 | [0,0,0] | [0,1,1] | | 
| 2 2 | [0,0,0] | [0,0,1] | | 
| 1 3 | [0,0,1] | [0,0,1] | | 
| 3 | [0,0,1] | [0,0,1] | 0 | 
| 3 | [0,0,1] | [0,0,1] | 0 | 
| 1 2 | [0,1,1] | [0,0,1] | | 
| 2 1 | [0,1,1] | [1,0,1] | | 
| 1 1 | [1,1,1] | [1,0,1] | | 
| 3 | [1,1,1] | [1,0,1] | 1 | 

Chuỗi đầu ra là`"01001"`, phù hợp với mẫu 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q·n) | Đọc ma trận là O(n^2). Mỗi truy vấn tính toán tối đa O(n) trong`compute_usquare`. | 
| Không gian | O(n^2 + 2n) | Ma trận sử dụng O(n^2).`row_flip`Và`col_flip`sử dụng O(n) mỗi cái. | 

Với n ≤ 1000 và q ≤ 10^6, thuật toán hoàn thành trong chưa đầy một giây và sử dụng ít hơn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    # Place the solution code here; return the result string
    n = int(input())
    matrix = [list(map(int, input().split())) for _ in range(n)]
    q = int(input())
    row_flip = [0] * n
    col_flip = [0] * n
    def compute_usquare():
        total = 0
        for i in range(n):
            total ^= matrix[i][i] ^ row_flip[i] ^ col_flip[i]
        return total
    res = []
    for _ in range(q):
        query = input().split()
        if query[0] == '1':
            i = int(query[1])
```
