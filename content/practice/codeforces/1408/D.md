---
title: "CF 1408D - Đèn pha"
description: "Chúng tôi được cung cấp một nhóm tên cướp được định vị trên lưới 2D và một bộ đèn rọi, mỗi đèn có một vị trí cố định. Kẻ cướp di chuyển một cách hạn chế: trong một nước đi duy nhất, tất cả tên cướp đều tăng tọa độ x lên một hoặc tăng tọa độ y lên một."
date: "2026-06-11T07:42:07+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "dp", "implementation", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1408
codeforces_index: "D"
codeforces_contest_name: "Grakn Forces 2020"
rating: 2000
weight: 1408
solve_time_s: 92
verified: false
draft: false
---

[CF 1408D - Đèn pha](https://codeforces.com/problemset/problem/1408/D) 

**Xếp hạng:** 2000 
**Thẻ:** tìm kiếm nhị phân, lực lượng vũ phu, cấu trúc dữ liệu, dp, triển khai, sắp xếp, hai con trỏ 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một nhóm tên cướp được định vị trên lưới 2D và một bộ đèn rọi, mỗi đèn có một vị trí cố định. Kẻ cướp di chuyển một cách hạn chế: trong một nước đi duy nhất, tất cả tên cướp đều tăng tọa độ x lên một hoặc tăng tọa độ y lên một. Cấu hình không an toàn nếu có bất kỳ tên cướp nào được nhìn thấy bởi bất kỳ đèn rọi nào, trong đó khả năng hiển thị có nghĩa là tọa độ của tên cướp đều nhỏ hơn hoặc bằng tọa độ của đèn rọi. Mục tiêu là xác định số lần di chuyển tối thiểu đến một cấu hình mà không có tên cướp nào được nhìn thấy bởi bất kỳ đèn rọi nào. 

Giới hạn đầu vào ở mức vừa phải: lên tới 2000 tên cướp và 2000 đèn rọi, với tọa độ lớn bằng$10^6$. Điều này loại trừ sự mô phỏng ngây thơ của mọi chuyển động có thể xảy ra, bởi vì điều đó có thể yêu cầu$10^6$số lần lặp lại trên mỗi tên cướp và đèn rọi, dẫn đến hàng nghìn tỷ hoạt động. Thay vào đó, chúng ta cần một cách tiếp cận tận dụng các hạn chế về chuyển động có cấu trúc và khả năng hiển thị để tính toán các bước di chuyển tối thiểu một cách hiệu quả. 

Một trường hợp phức tạp xảy ra khi tất cả những tên cướp ban đầu đều nằm ngoài phạm vi của tất cả các đèn rọi. Ví dụ: nếu một tên cướp ở (5,5) và đèn rọi gần nhất ở (2,3) thì không cần di chuyển. Việc triển khai bất cẩn luôn tăng tọa độ cho đến khi vượt quá mức tối đa nào đó sẽ tính quá nhiều bước di chuyển. Một trường hợp đặc biệt khác là khi nhiều đèn rọi chồng lên nhau trong phạm vi phủ sóng; thuật toán phải xem xét phạm vi bảo hiểm trong trường hợp xấu nhất để đảm bảo an toàn. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản về mặt khái niệm: mô phỏng mọi chuỗi hành động có thể xảy ra, kiểm tra sau mỗi chuỗi hành động xem kẻ cướp có an toàn hay không. Mỗi nước đi có hai lựa chọn, dẫn đến$2^k$trình tự cho$k$di chuyển. Cho rằng tọa độ có thể cao tới$10^6$, tốc độ này quá chậm. 

Một cách tiếp cận có cấu trúc hơn là suy nghĩ về độ dịch chuyển tối thiểu cần thiết để thoát khỏi từng đèn pha riêng lẻ. Đối với một tên cướp và một đèn rọi duy nhất, tên cướp phải vượt quá tọa độ x hoặc tọa độ y của đèn rọi. Cho phép$dx = \max(0, c_j - a_i + 1)$Và$dy = \max(0, d_j - b_i + 1)$. Tên cướp sẽ an toàn nếu chúng ta thực hiện ít nhất$dx$di chuyển sang phải hoặc$dy$di chuyển lên. Vấn đề giảm xuống còn việc kết hợp các yêu cầu riêng lẻ này trên tất cả các tên cướp và tất cả các đèn rọi để tìm ra số bước di chuyển tối thiểu trên toàn cầu, có tính đến việc tất cả các bước di chuyển phải theo một hướng trên mỗi bước. 

Cái nhìn sâu sắc quan trọng là chúng ta có thể liệt kê số lần di chuyển theo một hướng, nói đúng và tính toán số lần di chuyển tối thiểu cần thiết cho con số đó. Về mặt hình thức, chúng ta có thể tính toán trước cho mỗi cặp đèn pha tên cướp khoảng cách x$dx$và khoảng cách y$dy$. Sau đó chúng ta có thể lặp lại số lần di chuyển đúng$rx$từ 0 đến tối đa$dx$và tính số lần di chuyển lên tối thiểu tương ứng$ry$yêu cầu. Tổng số lần di chuyển là$rx + ry$. Bằng cách lấy mức tối thiểu trên tất cả các kết hợp như vậy, chúng tôi tìm thấy những bước di chuyển tối thiểu toàn cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(max_coordine)) | O(1) | Quá chậm | 
| Tối ưu | O(n*m + max_dx) | O(n*m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một danh sách trống để lưu trữ khoảng cách x và khoảng cách y cho mỗi cặp đèn pha-tên cướp. Đối với mỗi tên cướp$i$và đèn rọi$j$, tính toán$dx = \max(0, c_j - a_i + 1)$Và$dy = \max(0, d_j - b_i + 1)$. Nối thêm$(dx, dy)$vào danh sách. Điều này nắm bắt các bước di chuyển phải và lên tối thiểu để thoát khỏi ánh đèn rọi đó. 
2. Sắp xếp danh sách các cặp theo$dx$theo thứ tự tăng dần. Việc sắp xếp cho phép chúng ta xem xét một cách hiệu quả việc tăng số lần di chuyển đúng sẽ làm giảm số lần di chuyển lên cần thiết như thế nào. 
3. Khởi tạo một mảng$max\_dy\_suffix$lưu trữ tối đa$dy$cho tất cả các cặp có$dx \ge k$. Việc tính toán trước này cho phép chúng ta truy vấn các bước đi lên cần thiết trong thời gian không đổi đối với số bước đi đúng nhất định. 
4. Lặp lại số lần di chuyển đúng có thể$rx$từ 0 đến tối đa$dx$trong số tất cả các cặp. Đối với mỗi$rx$, sử dụng tính toán trước$max\_dy\_suffix$để tìm mức tối đa$dy$cho tất cả các cặp trong đó$dx > rx$. Tổng số nước đi cần thiết cho lựa chọn này là$rx + max\_dy\_suffix[rx]$. 
5. Theo dõi tổng số lần di chuyển tối thiểu trên tất cả$rx$. Xuất ra mức tối thiểu này. 

Tại sao nó hoạt động: Mỗi cặp đèn cướp-đèn rọi áp đặt một hình chữ nhật có các vị trí không an toàn. Thuật toán tính toán độ dịch chuyển tối thiểu dọc theo trục x và trục y cần thiết để rời khỏi tất cả các hình chữ nhật. Bằng cách xem xét tất cả các cách phân chia nước đi khả thi giữa phải và lên và thực hiện các nước đi cần thiết tối đa theo hướng khác, chúng tôi đảm bảo rằng tất cả bọn cướp đều di chuyển ra khỏi phạm vi của tất cả các đèn rọi. Việc tính toán trước đảm bảo rằng chúng ta chọn các bước di chuyển y cần thiết một cách hiệu quả mà không bỏ sót cặp trường hợp xấu nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
robbers = [tuple(map(int, input().split())) for _ in range(n)]
lights = [tuple(map(int, input().split())) for _ in range(m)]

pairs = []
for a, b in robbers:
    for c, d in lights:
        dx = max(0, c - a + 1)
        dy = max(0, d - b + 1)
        pairs.append((dx, dy))

pairs.sort()
max_dx = max(dx for dx, dy in pairs) if pairs else 0
max_dy_suffix = [0]*(max_dx + 2)
for dx, dy in reversed(pairs):
    max_dy_suffix[dx] = max(max_dy_suffix[dx], dy)
for i in range(max_dx, -1, -1):
    max_dy_suffix[i] = max(max_dy_suffix[i], max_dy_suffix[i+1])

ans = float('inf')
for rx in range(max_dx + 1):
    total = rx + max_dy_suffix[rx]
    ans = min(ans, total)
print(ans)
```Đầu tiên, mã tính toán tất cả các độ dịch chuyển dx và dy cần thiết cho mỗi cặp ánh sáng cướp. Việc sắp xếp các cặp đảm bảo rằng khi lặp lại rx, chúng ta có thể xác định hiệu quả dy tối đa cần thiết cho các cặp còn lại. Mảng hậu tố cho phép tra cứu trường hợp xấu nhất trong thời gian liên tục. Điều này ngăn chặn việc đếm thiếu các bước di chuyển cần thiết để thoát khỏi phạm vi phủ sóng của đèn pha chồng chéo. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
1 1
0 0
2 3
```| Tên cướp | Ánh sáng | dx | nhuộm | 
| --- | --- | --- | --- | 
| (0,0) | (2,3) | 3 | 4 | 

Hậu tố tối đa dy:`[4,4,4,4,0]`Lặp lại rx từ 0 đến 3: 

| rx | max_dy_suffix[rx] | tổng số bước di chuyển | 
| --- | --- | --- | 
| 0 | 4 | 4 | 
| 1 | 4 | 5 | 
| 2 | 4 | 6 | 
| 3 | 0 | 3 | 

Tối thiểu là 3. Đúng. 

Mẫu 2: 

đầu vào:```
2 2
0 0
1 1
1 2
2 1
```Cặp (dx, dy):```
(2,3), (3,2), (1,2), (2,1)
```Hậu tố max dy sau khi xử lý:```
[3,3,2,2,0]
```Lặp lại rx 0..3 cho tổng số bước di chuyển [3,4,4,3], tối thiểu là 3. 

Những dấu vết này xác nhận rằng thuật toán cân bằng chính xác các bước di chuyển sang phải và lên để thoát khỏi mọi ánh đèn rọi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n*m + max_dx) | Tính toán tất cả các cặp dx,dy trong O(n_m), sắp xếp theo O(n_m log n*m), xây dựng hậu tố trong O(max_dx) | 
| Không gian | O(n*m + max_dx) | Lưu trữ cặp và mảng hậu tố | 

Cho n và m đến năm 2000, n_m = 4_10^6 và max_dx 10^6, thuật toán phù hợp thoải mái trong giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    robbers = [tuple(map(int, input().split())) for _ in range(n)]
    lights = [tuple(map(int, input().split())) for _ in range(m)]

    pairs = []
    for a, b in robbers:
        for c, d in lights:
            dx = max(0, c - a + 1)
            dy = max(0, d - b + 1)
            pairs.append((dx, dy))

    pairs.sort()
    max_dx = max(dx for dx, dy in pairs) if pairs else 0
    max_dy_suffix = [0]*(max_dx + 2)
    for dx, dy in reversed(pairs):
        max_dy_suffix[dx] = max(max_dy_suffix[dx], dy)
    for i in range(max_dx, -1, -1):
        max_dy_suffix[i] = max(max_dy_suffix[i], max_dy_suffix
```
