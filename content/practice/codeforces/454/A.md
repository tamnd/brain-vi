---
title: "CF 454A - Chú ngựa nhỏ và mỏ pha lê"
description: "Chúng ta cần in một lưới vuông có kích thước lẻ chứa hình kim cương. Mọi ô thuộc về hình thoi được biểu thị bằng ký tự D và mọi ô khác được biểu thị bằng . Đầu vào chứa một số nguyên lẻ n."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 454
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 259 (Div. 2)"
rating: 800
weight: 454
solve_time_s: 87
verified: true
draft: false
---

[CF 454A - Little Pony và Crystal Mine](https://codeforces.com/problemset/problem/454/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần in một lưới vuông có kích thước lẻ chứa hình kim cương. Mỗi ô thuộc viên kim cương được thể hiện bằng ký tự`D`và mọi ô khác được biểu thị bằng`*`. 

Đầu vào chứa một số nguyên lẻ duy nhất`n`. Đầu ra phải bao gồm chính xác`n`hàng, mỗi hàng chứa chính xác`n`nhân vật. Viên kim cương được đặt ở giữa lưới và mở rộng về phía hàng giữa, sau đó co lại một cách đối xứng. 

Những hạn chế là rất nhỏ. Lưới lớn nhất có thể chỉ là`101 × 101`, chứa 10.201 ô. Ngay cả một thuật toán kiểm tra từng ô riêng lẻ cũng có thể dễ dàng đủ nhanh trong giới hạn một giây. Không cần tối ưu hóa nâng cao. 

Thử thách chính không phải là hiệu suất mà là việc xây dựng mô hình kim cương một cách chính xác. 

Một sai lầm dễ mắc phải là xử lý quá trình chuyển đổi giữa nửa trên và nửa dưới không chính xác. 

Ví dụ: 

đầu vào:```
5
```Đầu ra đúng:```
**D**
*DDD*
DDDDD
*DDD*
**D**
```Việc triển khai bất cẩn có thể tiếp tục tăng chiều rộng sau hàng giữa hoặc bắt đầu giảm một hàng quá sớm. 

Một lỗi phổ biến khác là tạo ra các hàng có tổng chiều dài sai. 

Ví dụ: hàng đầu tiên của tinh thể cỡ 5 phải là:```
**D**
```Hàng chứa 2 sao, sau đó là 1`D`, sau đó là 2 sao, với tổng chiều dài là 5. Việc quên các dấu sao ở phía sau sẽ tạo ra:```
**D
```điều này không hợp lệ vì mỗi hàng phải chứa chính xác`n`nhân vật. 

Đầu vào hợp lệ nhỏ nhất cũng đáng được kiểm tra. 

đầu vào:```
3
```Đầu ra:```
*D*
DDD
*D*
```Hàng trung tâm đã chiếm toàn bộ chiều rộng nên các phép tính ranh giới vẫn phải hoạt động khi viên kim cương chạm đến cả hai cạnh. 

## Phương pháp tiếp cận 

Một cách đơn giản để giải quyết vấn đề là kiểm tra mọi vị trí`(row, column)`trong lưới và quyết định xem ô đó có thuộc về viên kim cương hay không. Vì hình thoi được căn giữa nên chúng ta có thể tính khoảng cách từ hàng giữa và cột giữa và đánh dấu các ô tương ứng. Cách tiếp cận này đúng vì mỗi ô được phân loại độc lập. Nó thực hiện`n²`kiểm tra, tối đa là 10.201 thao tác cho các ràng buộc đã cho. 

Có một quan sát thậm chí còn đơn giản hơn. Chúng ta thực sự không cần phải quyết định từng tế bào một. Nếu chúng ta biết có bao nhiêu`D`các ký tự thuộc một hàng, các vị trí còn lại phải là dấu sao. 

Viên kim cương phát triển thêm hai ô mỗi hàng cho đến khi chạm tới tâm:```
1, 3, 5, ..., n
```Sau hàng giữa, nó co lại đối xứng:```
..., 5, 3, 1
```Đối với mỗi hàng, một lần số lượng`D`ký tự đã biết thì số sao ở mỗi bên là:```
(n - D_count) / 2
```Toàn bộ hàng sau đó có thể được xây dựng trực tiếp dưới dạng:```
stars + Ds + stars
```Điều này tránh phải suy nghĩ về các ô riêng lẻ và phù hợp với cấu trúc hình học của viên kim cương. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n²) | O(1) | Đã chấp nhận | 

Mặc dù cả hai cách tiếp cận đều có độ phức tạp tiệm cận như nhau, nhưng việc xây dựng các hàng trực tiếp sẽ đơn giản và tự nhiên hơn đối với tác vụ in mẫu này. 

## Hướng dẫn thuật toán 

1. Đọc số nguyên lẻ`n`. 
2. Tính chỉ số hàng giữa là`mid = n // 2`. 
3. Lặp lại qua từng chỉ mục hàng`i`từ`0`ĐẾN`n - 1`. 
4. Tính khoảng cách từ hàng giữa:```
dist = abs(i - mid)
```Các hàng cách đều nhau ở giữa phải trông giống hệt nhau vì hình thoi đối xứng. 
5. Xác định có bao nhiêu`D`các ký tự thuộc hàng này:```
d = n - 2 * dist
```Mỗi bước rời khỏi trung tâm sẽ loại bỏ một`D`từ phía bên trái và một từ phía bên phải. 
6. Tính số sao ở mỗi bên:```
stars = (n - d) // 2
```7. Dựng hàng:```
"*" * stars + "D" * d + "*" * stars
```8. Xuất hàng. 

### Tại sao nó hoạt động 

Đối với một hàng ở khoảng cách`dist`từ trung tâm, viên kim cương trở nên hẹp hơn đúng một ô ở mỗi bên. Điều đó làm giảm chiều rộng của viên kim cương bằng`2 × dist`, cho`n - 2 × dist`tế bào kim cương Các vị trí còn lại được chia đều cho hai bên trái và phải vì hình đối xứng quanh cột giữa. Mỗi hàng do thuật toán tạo ra có chính xác số lượng sao và`D`các ký tự, do đó lưới kết quả chính xác là viên kim cương cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

mid = n // 2

for i in range(n):
    dist = abs(i - mid)
    d = n - 2 * dist
    stars = (n - d) // 2
    print("*" * stars + "D" * d + "*" * stars)
```Giải pháp bắt đầu bằng việc xác định vị trí hàng giữa. Mỗi hàng khác được mô tả hoàn toàn bằng khoảng cách của nó với tâm đó. 

giá trị`d`lưu trữ số lượng liên tiếp`D`các ký tự trong hàng hiện tại. Ở hàng giữa,`dist = 0`, Vì thế`d = n`, nghĩa là toàn bộ hàng bao gồm`D`nhân vật. Di chuyển một hàng ra xa sẽ làm giảm chiều rộng đi hai, phù hợp với hình dạng của viên kim cương. 

giá trị`stars`được tính toán từ các vị trí chưa sử dụng. Vì mẫu có tính đối xứng nên một nửa số vị trí đó xuất hiện ở bên trái và một nửa ở bên phải. 

sử dụng`abs(i - mid)`tránh logic riêng biệt cho nửa trên và nửa dưới. Công thức tương tự sẽ tự động xử lý cả hai mặt của viên kim cương và loại bỏ nhiều lỗi phổ biến. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
```| Hàng ngang`i`|`dist`|`d`|`stars`| Hàng đầu ra | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 |`*D*`| 
| 1 | 0 | 3 | 0 |`DDD`| 
| 2 | 1 | 1 | 1 |`*D*`| 

Đầu ra:```
*D*
DDD
*D*
```Dấu vết này cho thấy sự đối xứng xung quanh hàng trung tâm. Hàng 0 và 2 có giá trị giống nhau vì chúng cách đều nhau. 

### Ví dụ 2 

đầu vào:```
5
```| Hàng ngang`i`|`dist`|`d`|`stars`| Hàng đầu ra | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 1 | 2 |`**D**`| 
| 1 | 1 | 3 | 1 |`*DDD*`| 
| 2 | 0 | 5 | 0 |`DDDDD`| 
| 3 | 1 | 3 | 1 |`*DDD*`| 
| 4 | 2 | 1 | 2 |`**D**`| 

Đầu ra:```
**D**
*DDD*
DDDDD
*DDD*
**D**
```Ví dụ này thể hiện sự giãn nở về phía trung tâm và sự co lại tương ứng sau đó. Trình tự của`D`đếm là`1, 3, 5, 3, 1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | In từng cái`n²`ký tự một lần | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Lưới lớn nhất có thể chỉ chứa 10.201 ký tự, do đó việc tạo và in mẫu dễ dàng trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(sys.stdin.readline())
    mid = n // 2

    out = []
    for i in range(n):
        dist = abs(i - mid)
        d = n - 2 * dist
        stars = (n - d) // 2
        out.append("*" * stars + "D" * d + "*" * stars)

    return "\n".join(out) + "\n"

# provided sample
assert run("3\n") == "*D*\nDDD\n*D*\n", "sample 1"

# minimum valid size
assert run("3\n") == "*D*\nDDD\n*D*\n", "minimum size"

# small symmetric case
assert run("5\n") == "**D**\n*DDD*\nDDDDD\n*DDD*\n**D**\n", "size 5"

# larger odd size
assert run("7\n") == "***D***\n**DDD**\n*DDDDD*\nDDDDDDD\n*DDDDD*\n**DDD**\n***D***\n", "size 7"

# maximum size, verify dimensions
res = run("101\n").splitlines()
assert len(res) == 101
assert all(len(row) == 101 for row in res)
assert res[50] == "D" * 101
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3`| Viên kim cương nhỏ | Kích thước hợp lệ tối thiểu | 
|`5`| Trình tự chiều rộng`1,3,5,3,1`| Mở rộng và co lại | 
|`7`| Trình tự chiều rộng`1,3,5,7,5,3,1`| Đối xứng chung | 
|`101`| Hàng giữa có 101`D`s | Xử lý hạn chế tối đa | 

## Vỏ cạnh 

### Tinh thể hợp lệ nhỏ nhất 

đầu vào:```
3
```Chỉ số hàng giữa là`1`. 

Đối với hàng`0`Và`2`, khoảng cách từ tâm là`1`, Vì thế`d = 1`Và`stars = 1`. Đối với hàng`1`, khoảng cách là`0`, Vì thế`d = 3`Và`stars = 0`. 

Đầu ra:```
*D*
DDD
*D*
```Công thức hoạt động mà không yêu cầu bất kỳ thao tác xử lý đặc biệt nào đối với kích thước nhỏ nhất được phép. 

### Hàng giữa chiếm toàn bộ chiều rộng 

đầu vào:```
5
```Tại hàng`2`, đó là hàng trung tâm:```
dist = 0
d = 5
stars = 0
```Hàng được tạo ra là:```
DDDDD
```Không có ngôi sao nào được thêm vào vì viên kim cương chạm đến cả hai cạnh. Việc tính toán xử lý trường hợp này một cách tự nhiên. 

### Sự đối xứng giữa nửa trên và nửa dưới 

đầu vào:```
7
```Hàng`1`Và`5`cả hai đều có:```
dist = 2
d = 3
stars = 2
```Cả hai hàng trở thành:```
**DDD**
```Bởi vì thuật toán chỉ phụ thuộc vào khoảng cách tuyệt đối từ tâm nên các hàng được phản chiếu được đảm bảo giống hệt nhau, giúp duy trì hình dạng kim cương cần thiết.
