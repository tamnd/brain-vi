---
title: "CF 190B - Bao quanh"
description: "Chúng ta được cung cấp hai “vùng đe dọa” hình tròn trên máy bay. Mỗi cái được xác định bởi một điểm trung tâm và bán kính. Trung tâm là một thành phố và bán kính mô tả khoảng cách mà vòng vây của kẻ thù kéo dài từ thành phố đó. Chúng tôi được phép đặt một radar ở bất cứ đâu trên máy bay."
date: "2026-06-03T01:22:56+07:00"
tags: ["codeforces", "competitive-programming", "geometry"]
categories: ["algorithms"]
codeforces_contest: 190
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 120 (Div. 2)"
rating: 1800
weight: 190
solve_time_s: 84
verified: true
draft: false
---

[CF 190B - Bao vây](https://codeforces.com/problemset/problem/190/B) 

**Đánh giá:** 1800 
**Thẻ:** hình học 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp hai “vùng đe dọa” hình tròn trên máy bay. Mỗi cái được xác định bởi một điểm trung tâm và bán kính. Trung tâm là một thành phố và bán kính mô tả khoảng cách mà vòng vây của kẻ thù kéo dài từ thành phố đó. 

Chúng tôi được phép đặt một radar ở bất cứ đâu trên máy bay. Radar này phát hiện bất cứ thứ gì nằm trong bán kính đĩa$r$tập trung tại vị trí radar. Radar được coi là thành công nếu tại thời điểm cuộc tấn công bắt đầu, nó có thể phát hiện ít nhất một điểm từ mỗi vòng trong số hai vòng tròn của kẻ thù. Về mặt hình học, đĩa radar phải giao nhau với cả hai đường tròn đã cho. 

Nhiệm vụ là chọn vị trí radar và bán kính của nó sao cho có thể thực hiện được điều này, đồng thời giảm thiểu bán kính. 

Kích thước đầu vào không đổi, chỉ có hai vòng tròn được đưa ra, do đó, bất kỳ giải pháp nào liên quan đến một lượng tính toán hình học cố định nhỏ đều dễ dàng đủ nhanh. Không cần các thủ thuật tối ưu hóa tiệm cận, nhưng việc xử lý trường hợp chính xác và chính xác là rất quan trọng. 

Một sai lầm ngây thơ nhưng nguy hiểm là cho rằng vị trí radar tốt nhất phải ở một trong số ít vị trí rõ ràng như tâm vòng tròn hoặc điểm giữa của các tâm. Điều này không thành công vì điểm tối ưu có thể nằm ở bất kỳ đâu trong mặt phẳng, thường dọc theo đoạn giữa các tâm, nhưng không nhất thiết phải ở một ứng cử viên riêng biệt. 

Một cạm bẫy phổ biến khác là bỏ qua các trường hợp ngăn chặn. Nếu một vòng tròn nằm hoàn toàn bên trong vòng tròn kia hoặc nếu chúng chồng lên nhau nhiều thì bán kính tối ưu sẽ hoạt động khác với trường hợp rời rạc. 

Ví dụ: nếu cả hai vòng tròn trùng nhau, câu trả lời đơn giản là bán kính nhỏ hơn, vì việc đặt radar ở trung tâm đã đảm bảo phát hiện cả hai ranh giới ở khoảng cách bằng bán kính đó. Bất kỳ cách tiếp cận nào chỉ xem xét khoảng cách giữa các trung tâm sẽ trả về 0 không chính xác. 

## Phương pháp tiếp cận 

Chúng tôi muốn có một điểm$P$giảm thiểu khoảng cách tối đa cần thiết để đến ranh giới của cả hai vòng tròn. 

Đối với vị trí radar cố định$P$, bán kính tối thiểu cần tìm là:$$\max(d(P, C_1) - r_1,\; d(P, C_2) - r_2,\; 0)$$Ở đâu$C_i$là tâm của đường tròn$i$, Và$r_i$là bán kính của nó. Chúng tôi trừ$r_i$bởi vì chúng ta chỉ cần chạm vào ranh giới vòng tròn chứ không thể chạm tới tâm của nó. 

Về mặt hình học, điều này có nghĩa là mỗi vòng tròn tạo ra một vùng “hạn chế khoảng cách”: chúng ta phải ở trong khoảng cách$r + r_i$của trung tâm$C_i$. Vì vậy để cố định$r$, tính khả thi trở thành: có tồn tại một điểm$P$nằm trong cả hai đĩa có bán kính$r + r_1$xung quanh$C_1$và bán kính$r + r_2$xung quanh$C_2$. 

Điều này làm giảm bài toán xuống việc tìm giá trị nhỏ nhất$r$sao cho hai đĩa giao nhau. 

Quan sát chính là tính đơn điệu: nếu bán kính$r$hoạt động, bất kỳ bán kính lớn hơn cũng hoạt động. Điều này cho phép tìm kiếm nhị phân trên$r$. 

Để kiểm tra tính khả thi của một$r$, chúng tôi kiểm tra xem hai vòng tròn có giao nhau không:$$d(C_1, C_2) \le (r + r_1) + (r + r_2)$$mà đơn giản hóa thành:$$d(C_1, C_2) \le 2r + r_1 + r_2$$Vì thế:$$r \ge \frac{d(C_1, C_2) - r_1 - r_2}{2}$$Từ$r \ge 0$, câu trả lời là:$$\max\left(0, \frac{d - r_1 - r_2}{2}\right)$$Vấn đề nằm ở việc tính khoảng cách Euclide giữa các tâm và áp dụng công thức này. 

Việc giải thích vũ lực sẽ thử tất cả các vị trí radar có thể, điều này là không thể trong không gian liên tục. Ngay cả việc rời rạc hóa mặt phẳng cũng không giúp ích gì vì các vị trí tối ưu không bị giới hạn trong lưới. 

Cái nhìn sâu sắc là ràng buộc hoàn toàn được xác định bởi một giá trị vô hướng duy nhất: khoảng cách giữa các tâm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (máy bay tìm kiếm) | vô hạn / không khả thi | O(1) | Quá chậm | 
| Tối ưu (giảm hình học) | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai mô tả vòng tròn, mỗi mô tả chứa tọa độ và bán kính. Những điều này xác định các đối tượng hình học duy nhất trong bài toán, vì vậy mọi lý luận sâu hơn đều phụ thuộc vào vị trí tương đối của chúng. 
2. Tính khoảng cách Euclide giữa hai tâm:$$d = \sqrt{(x_1 - x_2)^2 + (y_1 - y_2)^2}$$Khoảng cách này nắm bắt đầy đủ mối quan hệ không gian giữa hai vùng đe dọa. 
3. Tính thâm hụt chồng chéo hiệu quả:$$d - (r_1 + r_2)$$Điều này đo lường khoảng cách giữa các vòng tròn đã chạm hoặc chồng lên nhau. 
4. Nếu giá trị này âm hoặc bằng 0, các vòng tròn đã giao nhau hoặc một vòng tròn chứa vòng tròn kia. Trong trường hợp đó, radar đặt ở khu vực giao lộ có thể chạm vào cả hai mà không cần thêm bán kính, vì vậy câu trả lời là 0. 
5. Nếu không, hãy chia khoảng cách vượt quá cho 2. Điều này thể hiện mức độ radar phải mở rộng ra bên ngoài để đồng thời “thu hẹp” khoảng cách với cả hai vòng tròn. 
6. Xuất giá trị kết quả với độ chính xác đủ về dấu phẩy động. 

### Tại sao nó hoạt động 

Vị trí radar giảm thiểu bán kính yêu cầu luôn nằm trên đoạn đường nối hai tâm. Đối với bất kỳ ứng cử viên nào chỉ ra khỏi đường này, việc chiếu nó lên đoạn không làm tăng khoảng cách đến một trong hai tâm theo cách cải thiện hạn chế tối đa. Điều này làm giảm việc tối ưu hóa từ tìm kiếm liên tục 2D sang vấn đề cân bằng khoảng cách 1D. Sau khi bị giới hạn ở cấu trúc đó, bán kính tối thiểu chính xác bằng một nửa khoảng cách không được che phủ giữa các vòng tròn được mở rộng hoặc bằng 0 nếu chúng đã chồng lên nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

x1, y1, r1 = map(int, input().split())
x2, y2, r2 = map(int, input().split())

dx = x1 - x2
dy = y1 - y2

d = (dx * dx + dy * dy) ** 0.5

ans = (d - r1 - r2) / 2.0
if ans < 0:
    ans = 0.0

print(ans)
```Việc triển khai trực tiếp tính toán mức giảm hình học có được trước đó. Điểm tinh tế duy nhất là đảm bảo đủ độ chính xác của dấu phẩy động; sử dụng float chính xác kép của Python là đủ với các ràng buộc. 

Bước trừ phải xảy ra trước khi chia cho hai, vì các giá trị âm biểu thị các trường hợp trùng lặp phải được giữ bằng 0. Bỏ qua phần kẹp này dẫn đến câu trả lời phủ định sai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0 0 1
6 0 3
```Chúng tôi tính toán:$$d = 6$$

$$r_1 + r_2 = 4$$| Bước | Giá trị | 
| --- | --- | 
| Khoảng cách$d$| 6 | 
| Tổng bán kính | 4 | 
| Khoảng cách$d - r_1 - r_2$| 2 | 
| Trả lời | 1 | 

Điều này cho thấy trường hợp các vòng tròn được tách ra nhưng đủ gần để radar tối ưu chỉ cần “thu hẹp” một khoảng cách nhỏ. 

### Ví dụ 2 

đầu vào:```
0 0 5
3 0 5
```| Bước | Giá trị | 
| --- | --- | 
| Khoảng cách$d$| 3 | 
| Tổng bán kính | 10 | 
| Khoảng cách$d - r_1 - r_2$| -7 | 
| Trả lời | 0 | 

Điều này xác nhận trường hợp ngăn chặn/chồng chéo, trong đó không cần thêm bán kính radar. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số phép tính số học không đổi được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Các ràng buộc là cực kỳ nhỏ về số lượng đối tượng, vì vậy một công thức hình học trực tiếp là tối ưu và nằm trong giới hạn. Các phép toán dấu phẩy động chiếm ưu thế nhưng vẫn duy trì thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    x1, y1, r1 = map(int, input().split())
    x2, y2, r2 = map(int, input().split())

    dx = x1 - x2
    dy = y1 - y2
    d = math.sqrt(dx * dx + dy * dy)

    ans = (d - r1 - r2) / 2.0
    if ans < 0:
        ans = 0.0

    return f"{ans:.10f}"

# provided sample
assert abs(float(run("0 0 1\n6 0 3\n")) - 1.0) < 1e-9

# identical circles
assert abs(float(run("0 0 5\n0 0 5\n")) - 0.0) < 1e-9

# one inside another
assert abs(float(run("0 0 10\n1 1 1\n")) - 0.0) < 1e-9

# far apart
assert abs(float(run("0 0 1\n100 0 1\n")) - 49.0) < 1e-9

# tangent case
assert abs(float(run("0 0 1\n2 0 1\n")) - 0.0) < 1e-9
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| vòng tròn giống hệt nhau | 0 | ngăn chặn hoàn toàn | 
| cái này trong cái khác | 0 | xử lý vòng tròn lồng nhau | 
| xa nhau | 49 | độ chính xác tách lớn | 
| đường tròn tiếp tuyến | 0 | trường hợp đẳng thức biên | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một vòng tròn chứa hoàn toàn vòng tròn kia. Đối với đầu vào:```
0 0 10
1 0 1
```Thuật toán tính toán$d \approx 1$, Và$r_1 + r_2 = 11$, do đó khoảng cách là âm. Lực kẹp trả lời bằng không. Về mặt hình học, bất kỳ điểm nào bên trong vòng tròn lớn hơn đều đảm bảo giao nhau với điểm nhỏ hơn, do đó không cần thêm bán kính radar. 

Một trường hợp cạnh khác là tiếp tuyến:```
0 0 1
2 0 1
```Đây$d = 2$, Và$r_1 + r_2 = 2$, cho khoảng cách bằng không. Thuật toán trả về 0 một cách chính xác, khớp với thực tế là các vòng tròn đã chạm vào nhau, do đó, một radar đặt tại điểm tiếp xúc sẽ phát hiện cả hai ngay lập tức. 

Trường hợp tinh tế thứ ba là sự phân tách lớn trong đó bán kính nhỏ:```
0 0 1
100 0 1
```Khoảng cách là 98, vì vậy câu trả lời là 49. Điều này phản ánh tính đối xứng: radar tối ưu nằm ở giữa “ranh giới tăng cao” của các vòng tròn, cân bằng tầm với cần thiết cho cả hai bên như nhau.
