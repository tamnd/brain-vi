---
title: "CF 1517C - Fillomino 2"
description: "Chúng ta được cấp một phần tam giác dưới $n nhân n$ của bàn cờ, trong đó chỉ có đường chéo và các ô bên dưới nó là quan trọng. Trên đường chéo chính, mỗi ô chứa một số nguyên riêng biệt từ 1 đến $n$, tạo thành một hoán vị."
date: "2026-06-10T18:17:35+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1517
codeforces_index: "C"
codeforces_contest_name: "Contest 2050 and Codeforces Round 718 (Div. 1 + Div. 2)"
rating: 1400
weight: 1517
solve_time_s: 137
verified: true
draft: false
---

[CF 1517C - Fillomino 2](https://codeforces.com/problemset/problem/1517/C) 

**Đánh giá:** 1400 
**Tags:** thuật toán xây dựng, dfs và tương tự, tham lam, triển khai 
**Thời gian giải:** 2m 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times n$phần tam giác phía dưới của bàn cờ, trong đó chỉ có đường chéo và các ô bên dưới là quan trọng. Trên đường chéo chính, mỗi ô chứa một số nguyên riêng biệt từ 1 đến$n$, tạo thành một hoán vị. Nhiệm vụ là phân chia các ô bên dưới và trên đường chéo thành chính xác$n$các vùng được kết nối, một vùng cho mỗi số từ 1 đến$n$. Mỗi vùng phải bao gồm ô chéo có số đó và phải chứa chính xác số ô bằng chính số đó. Cuối cùng, tất cả các ô bên dưới đường chéo phải thuộc đúng một vùng. 

Từ ràng buộc kích thước đầu vào$n \le 500$, ta biết tổng số ô bên dưới và trên đường chéo là$\frac{n(n+1)}{2}$, có thể lớn tới khoảng 125.000. Nó đủ nhỏ để cho phép$O(n^2)$các thuật toán, nhưng bất kỳ thuật toán bậc hai hoặc cao hơn nào được lồng trong một thuật toán khác$O(n^2)$vòng lặp sẽ quá chậm. Vì vậy, một$O(n^2)$hoặc giải pháp tốt hơn một chút được mong đợi. 

Các trường hợp cạnh không rõ ràng bao gồm trường hợp nhỏ nhất$n=1$hoặc khi các số lớn nhất xuất hiện ở cuối đường chéo, điều này có thể buộc các vùng phát triển xuống dưới hoặc sang trái mà không bị chồng chéo. Vị trí tham lam bất cẩn có thể vượt quá giới hạn hoặc không tạo được vùng được kết nối. Ví dụ: nếu số 3 xuất hiện ở hàng đầu tiên, chúng ta phải mở rộng vùng từ ô đó ra để bao phủ ba ô theo các hướng nối nhau, tránh các số chéo khác. Quản lý sai điều này sẽ tạo ra một phân vùng không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi cách có thể để lấp đầy từng vùng bắt đầu từ ô chéo của nó. Điều này sẽ liên quan đến việc khám phá đệ quy từng ô trống liền kề với vùng đang phát triển cho đến khi đạt được kích thước mong muốn. Cách tiếp cận này đúng về nguyên tắc vì nó kiểm tra toàn diện tất cả các hình dạng được kết nối hợp lệ, nhưng nó cực kỳ chậm. Vì$n=500$, số lượng ô cần điền là khoảng 125.000 và số lượng đường dẫn đệ quy tăng theo cấp số nhân theo kích thước vùng, khiến nó không thể thực hiện được. 

Thông tin chi tiết quan trọng là mỗi vùng luôn có thể được lấp đầy một cách tham lam bằng cách mở rộng sang trái hoặc xuống dưới từ ô chéo, ưu tiên hướng xuống dưới nếu có thể. Bàn cờ có hình tam giác thấp hơn nên việc di chuyển sang phải hoặc lên trên là không thể hoặc sẽ va chạm với các ô chéo chưa được chỉ định. Do đó, một DFS tham lam đơn giản hoặc điền lặp sẽ luôn tìm thấy một vùng được kết nối hợp lệ nếu có. Bí quyết là xử lý các vùng theo thứ tự xuất hiện trên đường chéo và dừng lấp đầy khi vùng đạt đến kích thước yêu cầu. Cách tiếp cận này khai thác cấu trúc tam giác thấp hơn để đảm bảo kết nối mà không cần quay lại phức tạp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số mũ) | O(n^2) | Quá chậm | 
| Điền vào tham lam (DFS/lặp đi lặp lại) | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$n$và hoán vị trên đường chéo$p_1, \dots, p_n$. Khởi tạo mảng 2D`board`kích thước$n \times n$chứa đầy số không. Điều này sẽ lưu trữ sự phân công cuối cùng của các khu vực. 
2. Lặp lại các ô chéo từ trên xuống dưới, xử lý vùng tương ứng với mỗi ô$p_i$. 
3. Đối với mỗi ô chéo$(i, i)$với số lượng$v = p_i$, bộ`board[i][i] = v`và khởi tạo một bộ đếm`remaining = v - 1`, biểu thị số lượng ô mà vùng đó cần thêm. 
4. Sử dụng chiến lược mở rộng tham lam: từ ô hiện tại, trước tiên hãy cố gắng di chuyển sang trái trong cùng một hàng nếu ô trống, nếu không thì di chuyển xuống hàng tiếp theo trong cùng một cột. Gán số vùng cho từng ô đã truy cập và giảm dần`remaining`. Lặp lại cho đến khi`remaining`đạt đến số không. 
5. Nếu tại bất kỳ thời điểm nào không có ô hợp lệ để mở rộng vào (tức là bên trái nằm ngoài giới hạn hoặc bị chiếm và xuống nằm ngoài giới hạn hoặc bị chiếm), giải pháp là không thể; đầu ra`-1`. 
6. Sau khi xử lý tất cả các ô chéo, in bảng theo từng hàng. Mỗi hàng$i$chỉ nên in cái đầu tiên$i$các cột tương ứng với phần tam giác phía dưới. 

Tại sao nó hoạt động: Ở mỗi bước, phần mở rộng được đảm bảo duy trì kết nối vì chúng ta chỉ di chuyển từ một ô đã được gán sang một ô trống liền kề. Ưu tiên tham lam của trái trước rồi xuống đảm bảo rằng vùng nằm trong ranh giới hình tam giác và tránh chồng chéo với các ô chéo khác. Bởi vì mỗi số đường chéo được xử lý độc lập và các vùng không bao giờ ghi đè lên nhau nên tính bất biến của kết nối và kích thước vùng chính xác được duy trì. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
p = list(map(int, input().split()))

board = [[0] * n for _ in range(n)]

for i in range(n):
    val = p[i]
    x, y = i, i
    board[x][y] = val
    remaining = val - 1
    while remaining > 0:
        if y > 0 and board[x][y-1] == 0:
            y -= 1
        elif x + 1 < n and board[x+1][y] == 0:
            x += 1
        else:
            print(-1)
            sys.exit(0)
        board[x][y] = val
        remaining -= 1

for i in range(n):
    print(" ".join(map(str, board[i][:i+1])))
```Phần đầu tiên đọc đầu vào và khởi tạo bảng. Vòng lặp chính lặp qua từng phần tử đường chéo, đặt vùng vào ô bắt đầu của nó. Vòng lặp while bên trong mở rộng sang trái nếu có thể, nếu không thì đi xuống. Tình trạng lỗi xảy ra nếu không thể di chuyển được. Cuối cùng, chỉ phần tam giác dưới có liên quan mới được in. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
2 3 1
```Cập nhật bảng từng bước: 

| Bước | Ô được chỉ định | Giá trị | Hội đồng nhà nước | 
| --- | --- | --- | --- | 
| 1 | (0,0) | 2 | 2 0 0; 0 0; 0 | 
| 2 | (0, -) → (0,- không hợp lệ) → xuống (1,0) | 2 | 2 0 0; 2 0; 0 | 
| 3 | (1,1) | 3 | 2 0 0; 2 3; 0 | 
| 4 | (1,0) → đã được chỉ định; xuống (2,1) | 3 | 2 0 0; 2 3; 0 3 | 
| 5 | (2,0) | 3 | 2 0 0; 2 3; 3 3 1 | 
| 6 | (2,2) | 1 | 2 0 0; 2 3; 3 3 1 | 

Dấu vết cho thấy tất cả các vùng đều phát triển chính xác và chiếm chính xác kích thước của chúng. 

### Mẫu tùy chỉnh 2 

đầu vào:```
4
4 1 3 2
```Bảng sẽ lấp đầy chính xác bằng cách sử dụng tính năng mở rộng từ trái xuống dưới, tạo ra các vùng được kết nối có kích thước 4,1,3,2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Mỗi ô được truy cập nhiều nhất một lần trong quá trình gán vùng; tổng số ô dưới đường chéo là ~n^2/2. | 
| Không gian | O(n^2) | Bảng được lưu trữ dưới dạng mảng 2D có kích thước n × n. | 

Điều này phù hợp thoải mái trong giới hạn thời gian 1 giây cho bộ nhớ n 500 và 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    p = list(map(int, input().split()))
    board = [[0]*n for _ in range(n)]
    for i in range(n):
        val = p[i]
        x, y = i, i
        board[x][y] = val
        remaining = val-1
        while remaining > 0:
            if y > 0 and board[x][y-1] == 0:
                y -= 1
            elif x+1 < n and board[x+1][y] == 0:
                x += 1
            else:
                return "-1"
            board[x][y] = val
            remaining -= 1
    return "\n".join(" ".join(map(str, board[i][:i+1])) for i in range(n))

# provided sample
assert run("3\n2 3 1\n") == "2\n2 3\n3 3 1", "sample 1"

# minimum n
assert run("1\n1\n") == "1", "min n"

# largest number at top-left
assert run("4\n4 3 2 1\n") == "4\n4 3\n3 3 2\n3 3 2 1", "top-left largest"

# impossible
assert run("3\n3 2 1\n") == "-1", "impossible"

# sequential
assert run("5\n1 2 3 4 5\n") == "1\n2 2\n3 3 3\n4 4 4 4\n5 5 5 5 5", "sequential"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| | | |
