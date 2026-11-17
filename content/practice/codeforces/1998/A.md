---
title: "CF 1998A - Tìm K điểm phân biệt có tâm cố định"
description: "Chúng ta được cấp một điểm trung tâm mục tiêu $(xc, yc)$ và một số nguyên $k$. Nhiệm vụ không phải là tối ưu hóa bất cứ thứ gì hoặc tìm kiếm một cấu hình đặc biệt. Chúng ta chỉ cần xây dựng chính xác $k$ các điểm tọa độ nguyên riêng biệt có trung bình số học chính xác là $(xc, yc)$."
date: "2026-06-08T14:27:44+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1998
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 965 (Div. 2)"
rating: 800
weight: 1998
solve_time_s: 142
verified: false
draft: false
---

[CF 1998A - Tìm K điểm phân biệt có tâm cố định](https://codeforces.com/problemset/problem/1998/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, triển khai, toán học 
**Thời gian giải:** 2m 22s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một điểm trung tâm mục tiêu$(x_c, y_c)$và một số nguyên$k$. Nhiệm vụ không phải là tối ưu hóa bất cứ thứ gì hoặc tìm kiếm một cấu hình đặc biệt. Chúng ta chỉ cần xây dựng chính xác$k$các điểm tọa độ nguyên riêng biệt có giá trị trung bình số học chính xác$(x_c, y_c)$. 

Điều kiện trung tâm có nghĩa là:$$\frac{\sum x_i}{k} = x_c, \qquad \frac{\sum y_i}{k} = y_c$$Tương đương,$$\sum x_i = k \cdot x_c, \qquad \sum y_i = k \cdot y_c.$$Tọa độ của mọi điểm được tạo phải nằm trong$[-10^9,10^9]$, nhưng tọa độ trung tâm đã cho chỉ nằm giữa$-100$Và$100$, Và$k \le 1000$. Đây là mức cực kỳ nhỏ so với phạm vi tọa độ cho phép, vì vậy chúng ta có sự tự do rất lớn khi xây dựng điểm. 

Tổng số tiền của tất cả$k$giá trị trên các trường hợp thử nghiệm là nhiều nhất$1000$. Thậm chí một$O(k)$xây dựng cho mỗi trường hợp thử nghiệm là quá đủ. 

Mối nguy hiểm chính là vô tình tạo ra các điểm trùng lặp hoặc tạo ra các điểm có mức trung bình chỉ gần đúng. Vì câu trả lời mang tính xây dựng nên mọi điểm được tạo phải thỏa mãn chính xác phương trình trung tâm. 

Coi như$k=1$. Ví dụ:```
10 10 1
```Tâm duy nhất có thể có của một điểm chính là điểm đó, vì vậy chúng ta phải xuất ra:```
10 10
```Việc xây dựng luôn tạo các cặp đối xứng sẽ thất bại vì không có sẵn điểm đối tác. 

Một trường hợp tế nhị khác là kỳ quặc$k$. 

Ví dụ:```
0 0 3
```Nếu chúng ta chỉ xuất ra một cặp đối xứng:```
1 0
-1 0
```chúng ta chỉ có hai điểm. Chúng ta cần điểm phân biệt thứ ba trong khi vẫn giữ tổng số không đổi. Việc xây dựng bất cẩn có thể thêm$(0,0)$ngay cả khi nó trùng lặp một điểm hiện có. 

Cuối cùng, tất cả các điểm phải khác biệt. Ví dụ:```
0 0 2
```Xuất ra```
0 0
0 0
```có tâm đúng nhưng vi phạm yêu cầu về tính phân biệt. 

## Phương pháp tiếp cận 

Một tư duy vũ phu sẽ là tìm kiếm các điểm nguyên tùy ý và liên tục kiểm tra xem mức trung bình của chúng có bằng tâm mong muốn hay không. Vì tồn tại vô số cấu hình hợp lệ nên việc tìm kiếm như vậy cuối cùng cũng thành công. Vấn đề là không có giới hạn trên có ý nghĩa về thời gian tìm kiếm và độ chính xác trở nên khó đảm bảo. 

Cấu trúc của công thức trung tâm mang lại một cách tiếp cận rõ ràng hơn nhiều. Nếu hai điểm được đặt đối xứng quanh tâm thì$$(x_c-d, y_c)$$Và$$(x_c+d, y_c),$$sự đóng góp của họ vào tổng tọa độ là$$2x_c,\quad 2y_c.$$Nói cách khác, mức trung bình của cặp này đã chính xác là trung tâm. Bất kỳ số cặp đối xứng nào như vậy đều có thể được kết hợp mà không làm thay đổi tâm cuối cùng. 

Quan sát này ngay lập tức gợi ý một công trình. 

Nếu như$k$chẵn, tạo ra$k/2$cặp đối xứng xung quanh$(x_c,y_c)$. 

Nếu như$k$là số lẻ, trước hết hãy đặt điểm trung tâm vào. Đóng góp của nó chính xác là$(x_c,y_c)$. Sau đó xây dựng$(k-1)/2$cặp đối xứng xung quanh nó. 

Sử dụng khoảng cách$1,2,3,\ldots$đảm bảo rằng mọi điểm được tạo ra đều khác biệt. 

Từ$k \le 1000$, độ lệch lớn nhất mà chúng tôi sử dụng nhiều nhất là$500$, vì vậy tất cả các tọa độ vẫn nằm trong phạm vi cho phép. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Không giới hạn | Không giới hạn | Không thực tế | 
| Tối ưu | O(k) | O(1) không bao gồm đầu ra | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$x_c$,$y_c$, Và$k$. 
2. Tạo danh sách câu trả lời trống. 
3. Nếu$k$lẻ, thêm điểm trung tâm$(x_c,y_c)$để trả lời. 

Điểm này đóng góp chính xác một bản sao của trung tâm vào tổng số tiền. 
4. Đối với mỗi khoảng cách$d = 1,2,\ldots,\lfloor k/2 \rfloor$, cộng hai điểm:$$(x_c-d,\ y_c)$$Và$$(x_c+d,\ y_c).$$5. Xuất tất cả các điểm được tạo. 

### Tại sao nó hoạt động 

Mỗi cặp được tạo đều đối xứng quanh tâm. Tổng tọa độ của chúng là$$(x_c-d)+(x_c+d)=2x_c,$$Và$$y_c+y_c=2y_c.$$Như vậy mỗi cặp có giá trị trung bình$(x_c,y_c)$. 

Nếu như$k$là chẵn, chúng ta có chính xác$k/2$những cặp như vậy. Tổng tọa độ trở thành$$kx_c,\quad ky_c,$$vì vậy trung tâm là chính xác. 

Nếu như$k$thật kỳ lạ, chúng tôi cũng bao gồm$(x_c,y_c)$. Phần còn lại$k-1$điểm tạo thành các cặp đối xứng có tổng đóng góp là$$(k-1)x_c,\quad (k-1)y_c.$$Thêm điểm trung tâm sẽ mang lại$$kx_c,\quad ky_c.$$Tất cả các khoảng cách đều khác nhau nên không có hai điểm sinh ra trùng nhau. Vì vậy việc xây dựng luôn tạo ra$k$các điểm tọa độ nguyên riêng biệt với tâm yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

out = []

for _ in range(t):
    xc, yc, k = map(int, input().split())

    if k % 2 == 1:
        out.append(f"{xc} {yc}")

    for d in range(1, k // 2 + 1):
        out.append(f"{xc - d} {yc}")
        out.append(f"{xc + d} {yc}")

sys.stdout.write("\n".join(out))
```Việc thực hiện theo sau việc xây dựng trực tiếp. 

Đối với số lẻ$k$, điểm trung tâm được phát ra đầu tiên. Điều này xử lý điểm chưa từng có trong khi vẫn duy trì mức trung bình cần thiết. 

Vòng lặp tạo ra các cặp đối xứng bằng cách tăng khoảng cách từ tâm. Sử dụng các khoảng cách khác nhau sẽ tự động đảm bảo các điểm khác biệt. Vì khoảng cách tối đa là nhiều nhất$500$, mọi tọa độ đều cách xa tọa độ$\pm 10^9$giới hạn. 

Không có số học dấu phẩy động nào xuất hiện ở bất cứ đâu. Bằng chứng hoàn toàn dựa vào tổng số nguyên chính xác, loại bỏ mọi vấn đề về độ chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0 0 3
```| Bước | d | Điểm được tạo | 
| --- | --- | --- | 
| lẻ k | - | (0, 0) | 
| Cặp | 1 | (-1, 0) | 
| Cặp | 1 | (1, 0) | 

Tổng tọa độ: 

| x-tổng | tổng y | Số điểm | 
| --- | --- | --- | 
| 0 | 0 | 3 | 

Trung bình:$$(0/3,\;0/3)=(0,0)$$Ví dụ này chứng tỏ sự kỳ quặc$k$trường hợp. Điểm trung tâm hấp thụ điểm bổ sung trong khi cặp đối xứng đóng góp độ dịch chuyển ròng bằng không. 

### Ví dụ 2 

đầu vào:```
4 -5 4
```| Bước | d | Điểm được tạo | 
| --- | --- | --- | 
| Cặp | 1 | (3, -5) | 
| Cặp | 1 | (5, -5) | 
| Cặp | 2 | (2, -5) | 
| Cặp | 2 | (6, -5) | 

Tổng tọa độ: 

| x-tổng | tổng y | Số điểm | 
| --- | --- | --- | 
| 16 | -20 | 4 | 

Trung bình:$$(16/4,\;-20/4)=(4,-5)$$Ví dụ này cho thấy nhiều cặp đối xứng có thể được xếp chồng lên nhau một cách độc lập mà vẫn giữ nguyên tâm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k) | Mỗi điểm được tạo chính xác một lần | 
| Không gian | O(1) không bao gồm đầu ra | Chỉ có một số biến được duy trì | 

Tổng số tiền của tất cả$k$giá trị trên các trường hợp thử nghiệm là nhiều nhất$1000$. Việc tạo mỗi điểm một lần là chuyện nhỏ trong thời gian giới hạn và mức sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```
# helper: verify construction rather than exact output

def check(points, xc, yc, k):
    assert len(points) == k
    assert len(set(points)) == k

    sx = sum(x for x, y in points)
    sy = sum(y for x, y in points)

    assert sx == xc * k
    assert sy == yc * k

# sample-style case
pts = [(10, 10)]
check(pts, 10, 10, 1)

# minimum size
pts = [(0, 0)]
check(pts, 0, 0, 1)

# even k
pts = [(-1, 0), (1, 0)]
check(pts, 0, 0, 2)

# odd k
pts = [(5, -3), (4, -3), (6, -3)]
check(pts, 5, -3, 3)

# larger case
xc, yc, k = -100, 100, 1000
pts = []
for d in range(1, k // 2 + 1):
    pts.append((xc - d, yc))
    pts.append((xc + d, yc))
check(pts, xc, yc, k)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$x_c=0,y_c=0,k=1$| Điểm trung tâm duy nhất | Trường hợp tối thiểu | 
|$x_c=0,y_c=0,k=2$| Một cặp đối xứng | Thậm chí$k$| 
|$x_c=5,y_c=-3,k=3$| Cặp trung tâm cộng | Số lẻ$k$| 
|$x_c=-100,y_c=100,k=1000$| Nhiều cặp | Tối đa$k$| 
| Tọa độ âm | Trung tâm hợp lệ được duy trì | Xử lý dấu hiệu | 

## Vỏ cạnh 

### Trường hợp cạnh 1:$k=1$đầu vào:```
10 10 1
```Thuật toán đi vào số lẻ$k$nhánh và đầu ra:```
10 10
```Tổng tọa độ là$(10,10)$, và chia cho một sẽ có tâm cần tìm. Không cần cặp đối xứng. 

### Trường hợp Edge 2: Lẻ nhỏ$k$đầu vào:```
0 0 3
```Các đầu ra thuật toán:```
0 0
-1 0
1 0
```Tổng số tiền là:$$0 + (-1) + 1 = 0,$$

$$0 + 0 + 0 = 0.$$Trung tâm vẫn còn$(0,0)$, và mọi điểm đều phân biệt. 

### Trường hợp cạnh 3: Tối đa$k$đầu vào:```
-100 100 1000
```Khoảng cách lớn nhất được sử dụng là$500$. Tọa độ x được tạo nằm trong khoảng từ$$-100-500=-600$$ĐẾN$$-100+500=400.$$Những giá trị này không ở gần giới hạn cho phép của$\pm 10^9$. Sự khác biệt được bảo tồn bởi vì mỗi khoảng cách là duy nhất. 

### Trường hợp cạnh 4: Tọa độ tâm âm 

đầu vào:```
-5 -8 8
```Điểm được tạo là:```
-6 -8
-4 -8
-7 -8
-3 -8
-8 -8
-2 -8
-9 -8
-1 -8
```Mỗi cặp vẫn đối xứng xung quanh$(-5,-8)$. Dấu của tọa độ không ảnh hưởng đến đối số về tính đúng đắn, vì phép chứng minh chỉ phụ thuộc vào việc loại bỏ từng cặp xung quanh tâm.
