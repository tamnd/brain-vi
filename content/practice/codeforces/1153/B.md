---
title: "CF 1153B - Gạch phục vụ và đồ chơi"
description: "Chúng ta có một sự sắp xếp 3 chiều của các viên gạch đơn vị được sắp xếp theo lưới $n lần m$. Chiều cao của các viên gạch ở vị trí $(i,j)$ là không xác định, nhưng chúng ta được cung cấp ba góc nhìn một phần: mặt trước, bên trái và mặt trên."
date: "2026-06-12T02:52:01+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1153
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 551 (Div. 2)"
rating: 1200
weight: 1153
solve_time_s: 145
verified: false
draft: false
---

[CF 1153B - Gạch phục vụ và đồ chơi](https://codeforces.com/problemset/problem/1153/B) 

**Đánh giá:** 1200 
**Tags:** thuật toán mang tính xây dựng, tham lam 
**Thời gian giải:** 2m 25s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một sự sắp xếp 3 chiều của các viên gạch đơn vị được sắp xếp theo một$n \times m$lưới. Chiều cao gạch tại vị trí$(i,j)$chưa được biết, nhưng chúng tôi được cung cấp ba chế độ xem một phần: mặt trước, bên trái và mặt trên. Chế độ xem phía trước cho chiều cao tối đa trong mỗi$m$các cột dọc theo hướng hàng, chế độ xem bên trái sẽ cung cấp chiều cao tối đa trong mỗi cột$n$các hàng dọc theo hướng cột và chế độ xem trên cùng là nhị phân$n \times m$ma trận cho biết liệu một viên gạch có tồn tại ở một vị trí cụ thể hay không. Nhiệm vụ là tái tạo lại **bất kỳ** sự sắp xếp 3D nào của các viên gạch phù hợp với cả ba chế độ xem. 

Mỗi chiều cao được giới hạn bởi chiều cao tối đa cho phép$h$và số lượng hàng và cột nhiều nhất là 100, đủ nhỏ để cho phép$O(n \cdot m)$giải pháp. Việc liệt kê cưỡng bức tất cả các kết hợp chiều cao có thể là không cần thiết và sẽ không hiệu quả ngay cả đối với những giới hạn nhỏ này vì nó đòi hỏi phải cố gắng$h^{n \cdot m}$cấu hình. 

Trường hợp cạnh không rõ ràng phát sinh khi chế độ xem trên cùng cấm gạch ở một vị trí nhưng chế độ xem phía trước hoặc bên trái cho phép chiều cao dương. Ví dụ: giả sử chế độ xem trên cùng có$0$Tại$(2,3)$, chế độ xem bên trái cho thấy hàng 2 có tối đa$4$và chế độ xem phía trước cho thấy cột 3 có tối đa$5$. Việc triển khai bất cẩn có thể cố gắng chỉ định chiều cao tối đa được phép mà không tôn trọng chế độ xem từ trên xuống, dẫn đến cấu hình không hợp lệ. 

Một trường hợp cạnh khác xảy ra khi giới hạn chiều cao tối đa từ chế độ xem phía trước và bên trái khác nhau. Chẳng hạn, nếu chế độ xem bên trái cho hàng 1 là$3$và giao diện phía trước cho cột 2 là$2$, thì ô$(1,2)$không thể vượt quá$2$, mặc dù hàng cho phép$3$. Xử lý những xung đột này một cách cẩn thận là rất quan trọng. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ là thử gán độ cao từ$1$ĐẾN$h$cho mỗi ô được đánh dấu là$1$ở chế độ xem trên cùng và kiểm tra xem mức tối đa trong mỗi hàng và cột có khớp với chế độ xem bên trái và phía trước hay không. Về nguyên tắc, điều này đúng nhưng cực kỳ kém hiệu quả vì nó đòi hỏi phải kiểm tra đến$h^{n \cdot m}$cấu hình, điều này là không thể thực hiện được ngay cả đối với$n = m = 100$. 

Cái nhìn sâu sắc quan trọng là mỗi ô có một$1$ở chế độ xem trên cùng **bị ràng buộc độc lập** bởi mức tối đa của hàng và cột tương ứng. Cụ thể, chiều cao của một viên gạch tại$(i,j)$không thể vượt quá chế độ xem bên trái cho hàng$i$hoặc chế độ xem trước cho cột$j$, vì vượt quá mức tối đa sẽ vi phạm một trong các chế độ xem. Vì chế độ xem trên cùng cho phép ô khác 0 nên lựa chọn tối ưu và đơn giản nhất là gán **tối thiểu** của cực đại của hàng và cột. Tế bào có$0$ở chế độ xem trên cùng bị buộc về 0, bất kể các ràng buộc về hàng hoặc cột. 

Quan sát này làm giảm vấn đề xuống một$O(n \cdot m)$chuyển qua ma trận, trong đó đối với mỗi ô, chúng tôi kiểm tra chế độ xem trên cùng và gán giá trị 0 hoặc giá trị tối thiểu của cực đại hàng và cột. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(h^{n·m}) | O(n·m) | Quá chậm | 
| Tối ưu | O(n·m) | O(n·m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$n$,$m$, Và$h$. Chúng xác định kích thước của vật thể 3D và chiều cao tối đa được phép. 
2. Đọc mảng độ dài xem trước$m$. Mỗi mục thể hiện chiều cao tối đa cho cột tương ứng. 
3. Đọc mảng độ dài bên trái$n$. Mỗi mục đại diện cho chiều cao tối đa cho hàng tương ứng. 
4. Đọc góc nhìn từ trên xuống$n \times m$ma trận, với các mục$0$hoặc$1$. 
5. Khởi tạo một khoảng trống$n \times m$ma trận`H`để lưu trữ các độ cao được xây dựng lại. 
6. Lặp lại từng ô$(i,j)$:

Một. Nếu nhìn từ trên xuống tại$(i,j)$là$0$, giao phó`H[i][j] = 0`. 

b. Nếu nhìn từ trên xuống tại$(i,j)$là$1$, giao phó`H[i][j] = min(left[i], front[j])`. Điều này tôn trọng cả các ràng buộc về chế độ xem bên trái và phía trước trong khi vẫn đảm bảo chiều cao dương. 
7. In ma trận`H`từng hàng một. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến không vượt quá mức tối đa của hàng hoặc cột trong khi vẫn đảm bảo rằng tất cả các ô được biểu thị bằng chế độ xem trên cùng đều dương. Việc chỉ định các đảm bảo tối thiểu rằng tất cả các cực đại của chế độ xem phía trước và bên trái vẫn có thể đạt được bằng ít nhất một ô trong mỗi hàng và cột. Các ô được đánh dấu là$0$ở chế độ xem trên cùng được đảm bảo duy trì ở mức 0, ngăn ngừa vi phạm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, h = map(int, input().split())
front = list(map(int, input().split()))
left = list(map(int, input().split()))
top = [list(map(int, input().split())) for _ in range(n)]

H = [[0]*m for _ in range(n)]

for i in range(n):
    for j in range(m):
        if top[i][j]:
            H[i][j] = min(left[i], front[j])
        else:
            H[i][j] = 0

for row in H:
    print(' '.join(map(str, row)))
```Phần đầu tiên đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`. Các vòng lặp lồng nhau chỉ định chiều cao tối thiểu tương ứng với các chế độ xem trên cùng, bên trái và phía trước. sử dụng`min(left[i], front[j])`đảm bảo rằng chiều cao được chỉ định không bao giờ vi phạm mức tối đa của hàng hoặc cột. Cuối cùng, chúng tôi in từng hàng của ma trận chiều cao được xây dựng lại. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 7 3
2 3 0 0 2 0 1
2 1 3
1 0 0 0 1 0 0
0 0 0 0 0 0 1
1 1 0 0 0 0 0
```Dấu vết bước: 

| tôi | j | đầu[i][j] | trái[i] | phía trước[j] | H[i][j] | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 2 | 2 | 2 | 
| 0 | 1 | 0 | 2 | 3 | 0 | 
| 0 | 4 | 1 | 2 | 2 | 2 | 
| 1 | 6 | 1 | 1 | 1 | 1 | 
| 2 | 0 | 1 | 3 | 2 | 2 | 
| 2 | 1 | 1 | 3 | 3 | 3 | 

Đầu ra:```
2 0 0 0 2 0 0
0 0 0 0 0 0 1
2 3 0 0 0 0 0
```Điều này xác nhận thuật toán tôn trọng tất cả các chế độ xem và chỉ định độ cao chính xác. 

### Mẫu 2 

đầu vào:```
2 3 4
3 4 1
4 2
1 0 1
1 1 1
```Dấu vết bước: 

| tôi | j | đầu[i][j] | trái[i] | phía trước[j] | H[i][j] | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 4 | 3 | 3 | 
| 0 | 1 | 0 | 4 | 4 | 0 | 
| 0 | 2 | 1 | 4 | 1 | 1 | 
| 1 | 0 | 1 | 2 | 3 | 2 | 
| 1 | 1 | 1 | 2 | 4 | 2 | 
| 1 | 2 | 1 | 2 | 1 | 1 | 

Đầu ra:```
3 0 1
2 2 1
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n·m) | Một lần truyền qua lưới n×m, công việc không đổi trên mỗi ô | 
| Không gian | O(n·m) | Lưu trữ chế độ xem từ trên xuống và độ cao được xây dựng lại | 

Những hạn chế của vấn đề ($n, m ≤ 100$) đảm bảo rằng một$O(n·m)$giải pháp thực hiện thoải mái trong giới hạn thời gian 1 giây. Việc sử dụng bộ nhớ cũng thấp hơn nhiều so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m, h = map(int, input().split())
    front = list(map(int, input().split()))
    left = list(map(int, input().split()))
    top = [list(map(int, input().split())) for _ in range(n)]
    H = [[0]*m for _ in range(n)]
    for i in range(n):
        for j in range(m):
            if top[i][j]:
                H[i][j] = min(left[i], front[j])
            else:
                H[i][j] = 0
    return '\n'.join(' '.join(map(str,row)) for row in H)

# provided samples
assert run
```
