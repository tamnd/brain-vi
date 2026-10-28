---
title: "CF 106157C - Captcha đầy màu sắc"
description: "Chúng ta được đặt cho hai cái tên màu sắc cầu vồng khác nhau. Màu đầu tiên, C1, là từ mà hệ thống "tầm nhìn con người" đơn giản hóa phải nhận ra từ bảng mã ASCII."
date: "2026-06-25T11:18:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106157
codeforces_index: "C"
codeforces_contest_name: "2025 United Kingdom and Ireland Programming Contest (UKIEPC 2025)"
rating: 0
weight: 106157
solve_time_s: 73
verified: true
draft: false
---

[CF 106157C - Captcha đầy màu sắc](https://codeforces.com/problemset/problem/106157/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được đặt cho hai cái tên màu sắc cầu vồng khác nhau. 

Màu đầu tiên,`C1`, là từ mà hệ thống "tầm nhìn con người" đơn giản hóa phải nhận ra từ nghệ thuật ASCII. Màu thứ hai,`C2`, là từ sẽ xuất hiện trong văn bản thô để mô hình ngôn ngữ đọc hình ảnh dưới dạng các chuỗi thông thường sẽ nhìn thấy màu đó chứ không phải màu cầu vồng nào khác. 

Mô hình nhận dạng cực kỳ yếu. Nó không quan tâm đến hình dạng thực tế của các chữ cái. Mỗi thành phần bốn chữ in hoa được nối với nhau được coi là một chữ cái lớn duy nhất và tính năng duy nhất mà nó sử dụng là số lỗ bên trong thành phần đó. 

Một thành phần có hai lỗ được hiểu là`B`. 

Thành phần có một lỗ được hiểu là một trong`A`,`D`,`O`,`P`,`Q`,`R`. 

Thành phần không có lỗ được hiểu là bất kỳ chữ cái nào khác. 

Vì sáu tên màu cầu vồng có trình tự số lỗ khác nhau nên việc nhận dạng một màu sẽ giảm xuống việc nhận biết trình tự số lỗ. 

Đây là một vấn đề mang tính xây dựng thuần túy. Kích thước đầu vào rất nhỏ, chỉ có một cặp tên màu và đầu ra chỉ cần đáp ứng một tập hợp các ràng buộc hình học. Thách thức không phải là tính hiệu quả mà là tìm ra một công trình luôn hoạt động hiệu quả. 

Cạm bẫy chính là bất kỳ sự xuất hiện ngẫu nhiên nào của tên màu khác bên trong chuỗi được in sẽ ngay lập tức vi phạm tuyên bố. Một sai lầm dễ mắc phải khác là cho phép hai thành phần lân cận chạm vào nhau, điều này sẽ hợp nhất chúng thành một thành phần lớn hơn và thay đổi hoàn toàn trình tự đếm lỗ mà bộ nhận dạng nhìn thấy. 

## Phương pháp tiếp cận 

Một tư duy bạo lực sẽ cố gắng vẽ các chữ cái khối thực tế cho mọi màu và sau đó bằng cách nào đó ẩn màu mô hình ngôn ngữ bên trong bức tranh. Điều đó có hiệu quả về mặt khái niệm, nhưng nó tạo ra một số lượng lớn các trường hợp hình học và khiến cho việc suy luận về tính chính xác trở nên khó khăn. 

Quan sát quan trọng là bộ nhận dạng chỉ đếm lỗ. Nó không bao giờ kiểm tra xem một thành phần có giống với một chữ cái trong bảng chữ cái thực hay không. 

Một khi chúng ta nhận ra điều đó, mọi nhân vật của`C1`có thể được thay thế bằng một mẫu nhỏ với mục đích duy nhất là tạo ra một thành phần có chính xác 0, 1 hoặc 2 lỗ. 

Chúng ta có thể tính toán trước số lỗ liên quan đến mỗi chữ cái xuất hiện với màu sắc cầu vồng. 

Sau đó, chúng tôi xây dựng một thành phần được kết nối cho mỗi chữ cái của`C1`, ngăn cách các thành phần lân cận bằng ít nhất một cột dấu chấm. Bộ nhận dạng sẽ đọc chính xác trình tự đếm lỗ dự kiến. 

Đối với phần mô hình ngôn ngữ, chúng tôi chỉ cần đặt`C2`như văn bản thông thường ở hàng cuối cùng. Vì mỗi hàng khác chỉ sử dụng một chữ cái vô hại nên không có tên màu cầu vồng nào khác có thể xuất hiện dưới dạng chuỗi con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Vẽ chữ thật thủ công | O(1) | O(1) | Phức tạp không cần thiết | 
| Chỉ mã hóa số lượng lỗ | O( | C1 | ) | 

## Hướng dẫn thuật toán 

1. Tạo ánh xạ từ mỗi chữ in hoa xuất hiện với màu sắc cầu vồng đến số lỗ của nó. 
2. Chuẩn bị ba mẫu cố định. 

Thành phần không lỗ:```
XXX
X..
X..
X..
XXX
```Thành phần một lỗ:```
XXX
X.X
X.X
X.X
XXX
```Thành phần hai lỗ:```
XXX
X.X
XXX
X.X
XXX
```3. Đối với mỗi nhân vật của`C1`, tra cứu số lỗ của nó và nối mẫu tương ứng vào ảnh. 
4. Chèn một cột dấu chấm giữa các thành phần liên tiếp để chúng vẫn là các chữ cái riêng biệt. 
5. Thêm một hàng cuối cùng chứa`C2`, theo sau là dấu chấm cho đến khi độ dài của nó khớp với các hàng khác. 
6. In hình chữ nhật thu được. 

### Tại sao nó hoạt động 

Mỗi mẫu tạo thành một thành phần bốn kết nối duy nhất. Mẫu đầu tiên không chứa vùng kèm theo, mẫu thứ hai chứa chính xác một vùng kèm theo và mẫu thứ ba chứa chính xác hai vùng kèm theo. 

Vì các mẫu liền kề được phân tách bằng một cột đầy dấu chấm nên các thành phần không bao giờ chạm vào nhau. Do đó, bộ nhận dạng sẽ nhìn thấy một thành phần cho mỗi ký tự. 

Trình tự đếm lỗ mà bộ nhận dạng nhìn thấy chính xác là trình tự tương ứng với các chữ cái của`C1`, do đó hình ảnh được hiểu là`C1`. 

Hàng cuối cùng chứa rõ ràng`C2`dưới dạng một chuỗi con độc lập, đáp ứng yêu cầu về mô hình ngôn ngữ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

c1 = input().strip()
c2 = input().strip()

holes = {
    'A': 1, 'B': 2, 'C': 0, 'D': 1, 'E': 0, 'F': 0, 'G': 0,
    'H': 0, 'I': 0, 'J': 0, 'K': 0, 'L': 0, 'M': 0, 'N': 0,
    'O': 1, 'P': 1, 'Q': 1, 'R': 1, 'S': 0, 'T': 0, 'U': 0,
    'V': 0, 'W': 0, 'X': 0, 'Y': 0, 'Z': 0
}

templates = {
    0: [
        "XXX",
        "X..",
        "X..",
        "X..",
        "XXX",
    ],
    1: [
        "XXX",
        "X.X",
        "X.X",
        "X.X",
        "XXX",
    ],
    2: [
        "XXX",
        "X.X",
        "XXX",
        "X.X",
        "XXX",
    ]
}

rows = [""] * 5

for i, ch in enumerate(c1):
    pat = templates[holes[ch]]
    for r in range(5):
        rows[r] += pat[r]
        if i + 1 != len(c1):
            rows[r] += "."

width = max(len(rows[0]), len(c2))

for i in range(5):
    rows[i] += "." * (width - len(rows[i]))

rows.append(c2 + "." * (width - len(c2)))

print("\n".join(rows))
```Phần đầu tiên xây dựng bản đồ đếm lỗ. Chỉ có ba giá trị quan trọng: 0, 1 và 2. 

Ba mẫu được lựa chọn cẩn thận sao cho mỗi mẫu được kết nối và có số lượng lỗ cần thiết. 

Vòng lặp xây dựng chuyển đổi mọi ký tự của`C1`vào mẫu của nó và chèn một cột phân cách gồm các dấu chấm giữa các thành phần lân cận. Nếu không có dải phân cách đó, hai thành phần có thể hợp nhất thành một vùng được kết nối lớn hơn. 

Sau khi vẽ xong năm hàng, chúng ta tính chiều rộng hình chữ nhật cần thiết. Mỗi hàng phải có cùng độ dài, vì vậy các hàng ngắn hơn sẽ được đệm bằng dấu chấm. 

Cuối cùng, hàng cuối cùng chứa`C2`và được đệm theo cùng chiều rộng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
BLUE
RED
```Số lỗ cho`BLUE`là: 

| Thư | Lỗ | 
| --- | --- | 
| B | 2 | 
| L | 0 | 
| Bạn | 0 | 
| E | 0 | 

Trình tự thành phần được tạo ra là: 

| Vị trí | Đếm lỗ | 
| --- | --- | 
| 1 | 2 | 
| 2 | 0 | 
| 3 | 0 | 
| 4 | 0 | 

Bộ nhận dạng đọc`BLUE`. 

Hàng cuối cùng chứa`RED`, do đó mục tiêu mô hình ngôn ngữ cũng được thỏa mãn. 

### Ví dụ 2 

đầu vào:```
ORANGE
VIOLET
```Số lỗ cho`ORANGE`là: 

| Thư | Lỗ | 
| --- | --- | 
| Ồ | 1 | 
| R | 1 | 
| A | 1 | 
| N | 0 | 
| G | 0 | 
| E | 0 | 

Người nhận dạng thấy: 

| Thành phần | Lỗ | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 0 | 
| 5 | 0 | 
| 6 | 0 | 

Điều đó xác định duy nhất`ORANGE`. 

Hàng cuối cùng chứa`VIOLET`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( | C1 | 
| Không gian | O( | C1 | 

Màu cầu vồng dài nhất chỉ có sáu chữ cái nên bức tranh được tạo ra thấp hơn nhiều so với giới hạn 10 hàng và 100 cột. 

## Trường hợp thử nghiệm```
# helper sketch

# BLUE -> RED
inp = """BLUE
RED
"""

# RED -> BLUE
inp = """RED
BLUE
"""

# ORANGE -> GREEN
inp = """ORANGE
GREEN
"""

# VIOLET -> YELLOW
inp = """VIOLET
YELLOW
"""

# GREEN -> ORANGE
inp = """GREEN
ORANGE
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| XANH / ĐỎ | Bất kỳ công trình hợp lệ nào | Chứa thành phần hai lỗ | 
| ĐỎ / XANH | Bất kỳ công trình hợp lệ nào | Kết hợp các thành phần một lỗ và không lỗ | 
| CAM / XANH | Bất kỳ công trình hợp lệ nào | Tiền tố một lỗ dài nhất | 
| TÍM / VÀNG | Bất kỳ công trình hợp lệ nào | Đếm lỗ xen kẽ | 
| XANH / CAM | Bất kỳ công trình hợp lệ nào | Màu năm chữ | 

## Vỏ cạnh 

Một màu như`BLUE`bắt đầu bằng chữ cái có hai lỗ duy nhất trong số tất cả các màu cầu vồng. Việc xây dựng xử lý vấn đề này bằng cách chọn mẫu hai lỗ chuyên dụng. Bộ nhận dạng nhìn thấy một thành phần có chính xác hai vùng kèm theo và diễn giải chính xác nó là`B`. 

Một màu như`ORANGE`bắt đầu bằng nhiều chữ cái một lỗ liên tiếp. Vì mọi thành phần được phân tách bằng cột dấu chấm nên các thành phần một lỗ này không bao giờ hợp nhất. Bộ nhận dạng vẫn nhìn thấy ba chữ cái riêng biệt chứ không phải một thành phần lớn. 

Một màu như`GREEN`chứa cả chữ cái 0 lỗ và chữ cái 1 lỗ. Vì số lượng lỗ được mã hóa độc lập cho từng thành phần nên các chuỗi hỗn hợp được xử lý tự động mà không cần bất kỳ logic đặc biệt nào.
