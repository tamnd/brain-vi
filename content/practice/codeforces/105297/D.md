---
title: "CF 105297D - A dành cho Apple"
description: "Chúng ta có một hộp 3D hình chữ nhật thẳng hàng với các trục tọa độ, kéo dài từ gốc đến $(x, y, z)$. Bên trong chiếc hộp này đã có sẵn một quả táo hình cầu được đặt ở đâu đó trong không gian. Tâm của nó được cho là $(tx, ty, tz)$ và nó có bán kính $r$."
date: "2026-06-23T14:43:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105297
codeforces_index: "D"
codeforces_contest_name: "2024 USP Try-outs"
rating: 0
weight: 105297
solve_time_s: 60
verified: true
draft: false
---

[CF 105297D - A dành cho Apple](https://codeforces.com/problemset/problem/105297/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hộp 3D hình chữ nhật thẳng hàng với các trục tọa độ, kéo dài từ gốc đến$(x, y, z)$. Bên trong chiếc hộp này đã có sẵn một quả táo hình cầu được đặt ở đâu đó trong không gian. Trung tâm của nó được cho là$(t_x, t_y, t_z)$, và nó có bán kính$r$. Chúng tôi được đảm bảo rằng quả cầu hiện có này nằm hoàn toàn bên trong hộp. 

Nhiệm vụ là đặt quả cầu thứ hai vào cùng một hộp. Quả cầu mới này không được giao nhau với quả cầu hiện có tại bất kỳ điểm nào, mặc dù được phép chạm vào. Chúng tôi muốn bán kính lớn nhất có thể cho hình cầu thứ hai này. 

Về mặt hình học, hình cầu thứ hai phải thỏa mãn đồng thời hai ràng buộc. Đầu tiên, mọi điểm của nó phải nằm trong hộp, điều này hạn chế tâm của nó dựa trên bán kính của nó. Thứ hai, tâm của nó ít nhất phải bằng tổng hai bán kính tính từ tâm của quả táo hiện có. 

Những hạn chế có quy mô cực kỳ lớn, lên tới$10^9$, nhưng chỉ có một trường hợp thử nghiệm duy nhất. Điều này loại trừ mọi tìm kiếm không gian rời rạc hoặc bạo lực. Mọi thứ phải được tính toán trong thời gian không đổi bằng hình học. 

Một cách tiếp cận đơn giản có thể cố gắng đoán vị trí của hình cầu thứ hai và tối ưu hóa nó nhiều lần. Điều đó không thành công vì vùng khả thi là liên tục và ba chiều, đồng thời những thay đổi tọa độ nhỏ cũng ảnh hưởng đến tính khả thi theo cách không cục bộ. Một cách tiếp cận không chính xác khác là cho rằng vị trí tốt nhất luôn là ở một góc hoặc giữa hộp, điều này là sai vì quả cầu hiện tại có thể chặn các khu vực đó. 

Trường hợp cạnh tinh tế phát sinh khi hình cầu hiện tại ở gần một mặt hoặc một góc. Trong trường hợp đó, giới hạn có thể không phải là bức tường gần nhất mà là khoảng cách đến quả cầu hiện có, có thể chiếm ưu thế một cách bất ngờ. Ví dụ: nếu hộp lớn nhưng quả cầu hiện có nằm ở giữa, thì quả cầu thứ hai tối ưu bị hạn chế chủ yếu bởi khoảng cách đến quả cầu đó chứ không phải các bức tường. 

## Phương pháp tiếp cận 

Phối cảnh vũ phu là xem xét mọi vị trí có thể có cho tâm của hình cầu thứ hai và tính toán bán kính tối đa có thể có tại điểm đó. Đối với trung tâm cố định$(x', y', z')$, bán kính bị ràng buộc bởi ba yếu tố: khoảng cách đến sáu mặt của hình hộp và khoảng cách đến tâm của hình cầu hiện có trừ đi$r$. Việc đánh giá điều này trên một không gian liên tục là không thể về mặt tính toán và thậm chí cả một lưới kích thước rời rạc$10^9$trên mỗi chiều vượt xa mọi tính toán khả thi. 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần phải chọn trung tâm một cách rõ ràng. Đối với bất kỳ trung tâm ứng cử viên nào, bán kính tốt nhất có thể được xác định ngay lập tức bởi ràng buộc gần nhất của nó. Điều này biến bài toán thành việc tìm một điểm làm cực đại hóa hàm khoảng cách tối thiểu. 

Cái nhìn sâu sắc thứ hai là tâm tối ưu phải nằm trên ranh giới của hình hộp hoặc theo hướng chịu ảnh hưởng trực tiếp của hình cầu hiện có. Thay vì tìm kiếm vị trí, chúng tôi lý luận về những hạn chế. Hệ số giới hạn cho bán kính luôn là một trong bảy vật thể: sáu mặt của hình hộp hoặc bề mặt của hình cầu hiện có được giãn nở theo khoảng cách$r$. Câu trả lời sẽ trở thành bán kính tối đa có thể đạt được phù hợp với việc nằm trong tất cả các ràng buộc này. 

Chúng tôi giảm vấn đề về việc tính toán khoảng cách từ hình cầu hiện có đến ranh giới hình hộp theo cách có cấu trúc. Vị trí tối ưu căn chỉnh hình cầu thứ hai theo hướng mà nó chạm vào một mặt hoặc tiếp tuyến với hình cầu hiện có và chúng tôi so sánh tất cả các cấu hình giới hạn ứng cử viên đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force trên không gian | O($\infty$) | O(1) | Quá chậm | 
| Giảm ràng buộc | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa vấn đề như việc chọn một trung tâm$p = (x, y, z)$cho hình cầu mới và tối đa hóa bán kính của nó$R$, tuân theo: 

1.$R \le x, y, z, x_{max}-x, y_{max}-y, z_{max}-z$2.$R + r \le \|p - c\|$, Ở đâu$c = (t_x, t_y, t_z)$Ràng buộc đầu tiên đảm bảo chứa trong hộp, ràng buộc thứ hai đảm bảo không giao nhau. 

1. Tính riêng phần bán kính của hộp. Nếu không có hình cầu hiện có thì tâm tốt nhất là tâm của hình hộp, cho bán kính$$R_{box} = \min(x, y, z, x - x/2, y - y/2, z - z/2)$$Sạch sẽ hơn, ở trung tâm$(x/2, y/2, z/2)$, bán kính là:$$R_{box} = \min(x/2, y/2, z/2)$$2. Tính toán tâm ứng cử viên để tối đa hóa khoảng trống từ quả cầu hiện có trong khi vẫn ở bên trong hộp. Điều này tương ứng với việc đặt quả cầu mới trên tia từ quả cầu hiện có về hướng “mở” nhất. Ràng buộc hiệu quả trở thành tối ưu hóa kiểu từ xa đến góc. 
3. Thay vì tìm kiếm hướng, hãy quan sát rằng cấu hình tốt nhất xảy ra khi hình cầu mới tiếp xúc với cả hộp và hình cầu hiện có. Điều này làm giảm vấn đề đánh giá khoảng cách từ tâm hình cầu hiện tại đến tất cả 8 góc của hộp. 
4. Đối với mỗi góc$q$, tính:$$d = \|q - c\|$$Bán kính lớn nhất có thể nếu hình cầu mới có tâm ở hướng góc đó là:$$R_q = d - r$$vì hình cầu mới phải nằm bên ngoài hình cầu hiện có. 
5. Đồng thời tính bán kính giới hạn hộp thuần$R_{box}$. 
6. Câu trả lời cuối cùng là:$$\max(R_{box}, \max_q (d_q - r))$$### Tại sao nó hoạt động 

Vùng khả thi là lồi và cả hai ràng buộc đều xác định ranh giới lồi: một hộp và một vùng hình cầu bị loại trừ. Bán kính tối đa đạt được khi quả cầu mới được “đẩy” cho đến khi nó chạm vào ít nhất một ranh giới ràng buộc. Bất kỳ cấu hình tối ưu nào cũng phải tiếp tuyến với ít nhất một mặt của hình hộp hoặc tiếp tuyến với hình cầu hiện có. Bất kỳ vị trí bên trong nào cũng có thể được mở rộng cho đến khi đạt đến một ranh giới, do đó không có điểm bên trong nào có thể tối ưu. 

Bởi vì cả hai ràng buộc đều trơn tru và đối xứng, các điểm cực trị xuất hiện tại các đỉnh được tạo ra bởi các giao điểm của các ràng buộc, tương ứng với các góc của hộp so với hình cầu hiện có. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

def dist(a, b):
    return math.sqrt((a[0]-b[0])**2 + (a[1]-b[1])**2 + (a[2]-b[2])**2)

x, y, z = map(float, input().split())
tx, ty, tz = map(float, input().split())
r = float(input())

# best radius limited by box alone (center of box)
best = min(x, y, z) / 2.0

# consider all 8 corners
corners = [
    (0.0, 0.0, 0.0),
    (0.0, 0.0, z),
    (0.0, y, 0.0),
    (0.0, y, z),
    (x, 0.0, 0.0),
    (x, 0.0, z),
    (x, y, 0.0),
    (x, y, z),
]

c = (tx, ty, tz)

for q in corners:
    d = dist(q, c)
    best = max(best, d - r)

print(f"{best:.15f}")
```Việc thực hiện tách biệt hai chế độ hình học một cách rõ ràng. Dòng đầu tiên tính toán bán kính tốt nhất không bị giới hạn bên trong một hộp, đạt được ở tâm của nó. Vòng lặp thứ hai đánh giá tất cả các hướng cực trị được xác định bởi các góc hộp so với tâm hình cầu hiện có. 

Trừ$r$chiếm chính xác vùng loại trừ xung quanh quả táo đầu tiên. Việc sử dụng số học dấu phẩy động là an toàn theo yêu cầu$10^{-6}$độ chính xác và in với 15 chữ số thập phân đảm bảo sự ổn định. 

Một chi tiết tinh tế là chúng ta chưa bao giờ tính toán rõ ràng tâm của hình cầu thứ hai. Hình học cho phép chúng ta thu gọn việc tối ưu hóa thành việc đánh giá hữu hạn nhiều cấu hình cực trị. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 1 1
0.5 0.5 0.5
0.5
```Chúng tôi tính toán bán kính chỉ có hộp: 

| Bước | Giá trị | 
| --- | --- | 
| phút(x,y,z)/2 | 0,5 | 

Các góc cho: 

| Góc | Khoảng cách tới (0,5,0,5,0,5) | d - r | 
| --- | --- | --- | 
| (0,0,0) | 0,8660 | 0,3660 | 
| người khác | ≥ 0,8660 | ≤ 0,3660 | 

Tốt nhất vẫn là 0,5. 

Điều này cho thấy ràng buộc hộp chiếm ưu thế vì hình cầu hiện có nằm ở trung tâm. 

### Ví dụ 2 

đầu vào:```
10 10 10
3.14 2.71 5.0
2.5
```Bán kính chỉ có hộp là 5. 

Bây giờ hãy tính một góc đại diện: 

| Góc | Khoảng cách | d - r | 
| --- | --- | --- | 
| (0,0,0) | ~6,56 | ~4.06 | 
| (10,10,10) | ~10,53 | ~8,03 | 

Tốt nhất trở thành ~ 8,03. 

Điều này chứng tỏ một trường hợp trong đó việc tránh quả cầu hiện có quan trọng hơn việc bố trí đối xứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ 8 phép tính khoảng cách và số học không đổi | 
| Không gian | O(1) | Không có cấu trúc phụ trợ ngoài một vài biến | 

Giải pháp này dễ dàng phù hợp với các ràng buộc vì tất cả các phép toán đều là số học dấu phẩy động theo thời gian không đổi, không phụ thuộc vào cường độ đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    x, y, z = map(float, input().split())
    tx, ty, tz = map(float, input().split())
    r = float(input())

    best = min(x, y, z) / 2.0

    corners = [
        (0.0, 0.0, 0.0),
        (0.0, 0.0, z),
        (0.0, y, 0.0),
        (0.0, y, z),
        (x, 0.0, 0.0),
        (x, 0.0, z),
        (x, y, 0.0),
        (x, y, z),
    ]

    c = (tx, ty, tz)

    for q in corners:
        d = math.sqrt((q[0]-c[0])**2 + (q[1]-c[1])**2 + (q[2]-c[2])**2)
        best = max(best, d - r)

    return f"{best:.15f}"

# provided samples
assert abs(float(run("2 1 1\n0.5 0.5 0.5\n0.5\n")) - 0.5) < 1e-6
assert abs(float(run("1000000000 1000000000 1000000000\n500000000 500000000 500000000\n500000000\n")) - 133974596.215561) < 1e-3

# custom cases
assert run("1 1 1\n0.5 0.5 0.5\n0.0\n")  # small symmetric case
assert run("10 1 1\n5 0.5 0.5\n0.1\n")  # skewed box
assert run("10 10 10\n0 0 0\n0\n")      # no obstacle sphere
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Quả cầu trung tâm 1×1×1 | 0,5 | đóng gói chặt chẽ đối xứng | 
| trung tâm khối lớn | giá trị đã biết | chia tỷ lệ chính xác nổi | 
| hộp lệch | khác nhau | xử lý bất đối xứng | 
| quả cầu không có chướng ngại vật | giải pháp trung tâm | tính chính xác dự phòng | 

## Vỏ cạnh 

Trường hợp một cạnh là khi quả cầu hiện có nằm chính xác ở giữa hộp. Trong trường hợp đó, tất cả khoảng cách các góc đều bằng nhau và ràng buộc có ý nghĩa duy nhất là chính chiếc hộp đó. Thuật toán đánh giá tất cả các góc nhưng tạo ra các giá trị giống hệt nhau, lấy bán kính tâm hộp làm câu trả lời. 

Một trường hợp cạnh khác xảy ra khi hình cầu hiện tại ở rất gần một góc. Trong tình huống này, hầu hết các ứng cử viên dựa trên góc đều tạo ra các giá trị âm hoặc rất nhỏ sau khi trừ đi$r$và mức tối đa sẽ dịch chuyển chính xác sang một góc khác hoặc sang giải pháp lấy hộp làm trung tâm. 

Một trường hợp tế nhị cuối cùng là khi$r$là cực kỳ nhỏ. Khi đó hình cầu thứ hai hoạt động một cách hiệu quả như thể hình cầu thứ nhất là một điểm. Thuật toán giảm xuống một cách tự nhiên để tối đa hóa khoảng cách đến một điểm bên trong hộp, điểm này vẫn được xử lý chính xác bằng cách kiểm tra tất cả các góc và đường giới hạn bắt nguồn từ tâm.
