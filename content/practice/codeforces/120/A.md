---
title: "CF 120A - Thang máy"
description: "Chúng ta được cung cấp hai thông tin về một người đang đi thang máy. Đầu vào đầu tiên cho chúng ta biết người đó đã từng vào cửa nào, cửa trước hay cửa sau. Đầu vào thứ hai cho chúng ta biết người đó đang giữ đường ray nào, đường ray 1 hay đường ray 2."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 120
codeforces_index: "A"
codeforces_contest_name: "School Regional Team Contest, Saratov, 2011"
rating: 1000
weight: 120
solve_time_s: 86
verified: true
draft: false
---

[CF 120A - Thang máy](https://codeforces.com/problemset/problem/120/A) 

**Đánh giá:** 1000 
**Tags:** vũ lực, thực hiện, toán học 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp hai thông tin về một người đang đi thang máy. 

Đầu vào đầu tiên cho chúng ta biết người đó đã từng vào cửa nào, cửa trước hay cửa sau. Đầu vào thứ hai cho chúng ta biết người đó đang giữ đường ray nào, đường ray 1 hay đường ray 2. 

Thang máy đối xứng, nhưng ý nghĩa của "trái" và "phải" thay đổi tùy theo người đi vào từ phía nào. Ray 1 nằm ở bên trái khi đi vào bằng cửa trước nhưng ở bên phải khi đi vào bằng cửa sau. Vì mọi người cầm lan can bằng tay khỏe hơn nên chúng ta phải xác định xem VIP thuận tay trái hay thuận tay phải. 

Những hạn chế là cực kỳ nhỏ. Chỉ có bốn kết hợp đầu vào có thể: 

- mặt trước + đường ray 1 
- mặt trước + đường ray 2 
- lưng + ray 1 
- lưng + ray 2 

Bất kỳ giải pháp thời gian không đổi nào cũng đủ nhanh. Ngay cả việc tra cứu mạnh mẽ trên tất cả các khả năng cũng dễ dàng nằm trong giới hạn. 

Khó khăn chính không phải ở hiệu suất mà là tránh nhầm lẫn về hướng của thang máy. Việc triển khai bất cẩn có thể cho rằng các số đường ray luôn tương ứng với cùng một mặt vật lý, điều này là sai vì phối cảnh thay đổi khi đi vào từ cửa đối diện. 

Một lỗi dễ xảy ra với đầu vào này:```
back
1
```Câu trả lời đúng là:```
R
```Ai đó đi vào từ phía sau sẽ nhìn thấy đường ray số 1 ở bên phải chứ không phải bên trái. Một giải pháp luôn ánh xạ đường ray 1 sang bên trái sẽ in không chính xác`L`. 

Một trường hợp phức tạp khác là:```
front
2
```Câu trả lời đúng là:```
R
```Từ lối vào phía trước, đường ray số 2 nằm ở phía bên phải của người lái. Việc quên chuyển đổi giữa trái và phải dựa trên số đường ray sẽ dẫn đến kết quả sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mã hóa thủ công mọi cấu hình có thể. Vì chỉ có bốn trường hợp nên chúng ta có thể viết trực tiếp câu lệnh điều kiện cho từng trường hợp. 

Ví dụ:

-`front + 1 -> L`-`front + 2 -> R`-`back + 1 -> R`-`back + 2 -> L`Điều này hoạt động vì không gian đầu vào rất nhỏ. Độ phức tạp về thời gian là không đổi vì chúng tôi chỉ đánh giá một vài so sánh. 

Mặc dù giải pháp vũ phu đã được chấp nhận, chúng ta có thể đơn giản hóa logic hơn nữa bằng cách quan sát mô hình đằng sau các trường hợp. 

Khi vào từ phía trước, số đường ray khớp trực tiếp với bên: 

- ray 1 nghĩa là tay trái 
- ray 2 nghĩa là tay phải 

Khi vào từ phía sau, hướng đảo ngược: 

- ray 1 trở thành tay phải 
- ray 2 trở thành tay trái 

Quan sát quan trọng là câu trả lời phụ thuộc vào việc đường ray xuất hiện ở bên trái hay bên phải của người lái xe từ góc nhìn hiện tại của họ. Hướng vào hoàn toàn xác định liệu ánh xạ là bình thường hay đảo ngược. 

Điều đó cho phép chúng ta rút gọn vấn đề thành một cấu trúc điều kiện đơn giản thay vì xử lý tất cả bốn kết hợp một cách độc lập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc cửa ra vào dưới dạng một chuỗi. 

Giá trị là hoặc`"front"`hoặc`"back"`. 
2. Đọc số đường ray. 

Giá trị là hoặc`1`hoặc`2`. 
3. Nếu người đó đi vào bằng cửa trước, hãy dịch đường ray một cách bình thường. 

Đường ray 1 nằm ở bên trái nên hãy in`L`. 

Rail 2 nằm ở bên phải nên hãy in`R`. 
4. Nếu người vào bằng cửa sau thì giải thích ngược lại. 

Rail 1 hiện ở bên phải, vì vậy hãy in`R`. 

Rail 2 hiện ở bên trái, vì vậy hãy in`L`. 

### Tại sao nó hoạt động 

Hình dạng thang máy là cố định nhưng góc nhìn của người lái thay đổi tùy thuộc vào lối vào mà họ sử dụng. Đi vào từ phía đối diện sẽ hoán đổi ý nghĩa của trái và phải. 

Thuật toán mô hình hóa chính xác sự thay đổi quan điểm này. Đối với lối vào phía trước, việc đánh số đường ray khớp với thứ tự tự nhiên từ trái sang phải. Đối với lối vào phía sau, thứ tự đảo ngược. Vì người lái luôn giữ tay vịn bằng tay khỏe hơn nên việc xác định cạnh của tay vịn sẽ trực tiếp xác định người lái thuận tay trái hay tay phải. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

door = input().strip()
rail = int(input())

if door == "front":
    if rail == 1:
        print("L")
    else:
        print("R")
else:
    if rail == 1:
        print("R")
    else:
        print("L")
```Chương trình bắt đầu bằng cách đọc hướng vào và số đường ray. 

Điều kiện đầu tiên phân tách hai quan điểm có thể có của người lái. Bên trong mỗi nhánh, số đường ray xác định xem đường ray nằm ở bên trái hay bên phải của người lái. 

Việc triển khai vẫn rõ ràng thay vì nén mọi thứ vào một biểu thức duy nhất. Đối với một vấn đề triển khai nhỏ như thế này, sự rõ ràng có giá trị hơn sự thông minh. 

các`.strip()`cuộc gọi rất quan trọng vì dòng đầu vào chứa ký tự dòng mới ở cuối. Không loại bỏ nó, so sánh như`door == "front"`sẽ thất bại. 

Không có vấn đề về ranh giới hoặc lo ngại tràn vì miền đầu vào cực kỳ nhỏ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
front
1
```| Bước | cửa | đường sắt | Giải thích | Đầu ra | 
| --- | --- | --- | --- | --- | 
| Đọc đầu vào | phía trước | 1 | Lối vào phía trước giữ định hướng | | 
| Xác định bên | phía trước | 1 | Đường ray 1 nằm bên trái | L | 

Đầu ra cuối cùng:```
L
```Ví dụ này thể hiện trường hợp định hướng bình thường. Nhập từ phía trước có nghĩa là đánh số đường ray theo bố cục từ trái sang phải tự nhiên. 

### Ví dụ 2 

đầu vào:```
back
1
```| Bước | cửa | đường sắt | Giải thích | Đầu ra | 
| --- | --- | --- | --- | --- | 
| Đọc đầu vào | trở lại | 1 | Lối vào phía sau đảo ngược hướng | | 
| Xác định bên | trở lại | 1 | Đường sắt 1 trở thành bên phải | R | 

Đầu ra cuối cùng:```
R
```Ví dụ này thể hiện sự đảo ngược quan điểm quan trọng. Số đường ray tương tự tương ứng với tay đối diện vì người lái đi vào từ phía đối diện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số so sánh được thực hiện | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Giải pháp dễ dàng nằm trong giới hạn vì nó thực hiện công việc liên tục bất kể đầu vào. Việc sử dụng bộ nhớ cũng không đổi vì chỉ có hai biến được lưu trữ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    door = input().strip()
    rail = int(input())

    if door == "front":
        if rail == 1:
            print("L")
        else:
            print("R")
    else:
        if rail == 1:
            print("R")
        else:
            print("L")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output

# provided sample
assert run("front\n1\n") == "L\n", "sample 1"

# custom cases
assert run("front\n2\n") == "R\n", "front entrance, right rail"
assert run("back\n1\n") == "R\n", "back entrance reverses orientation"
assert run("back\n2\n") == "L\n", "back entrance with second rail"
assert run("front\n1\n") == "L\n", "minimum valid configuration"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`front 2`|`R`| Lập bản đồ chính xác cho lối vào phía trước | 
|`back 1`|`R`| Xử lý đảo ngược phối cảnh | 
|`back 2`|`L`| Hướng ngược lại cho đường ray thứ hai | 
|`front 1`|`L`| Ánh xạ bên trái cơ bản | 

## Vỏ cạnh 

Hãy xem xét đầu vào này:```
back
1
```Thuật toán đi vào`"back"`chi nhánh. Bên trong nhánh đó, đường ray 1 ánh xạ về phía bên phải vì người lái đang quay mặt về hướng ngược lại so với lối vào phía trước. 

Dấu vết thực hiện: 

-`door = "back"`-`rail = 1`- nhập logic cửa sau 
- ray 1 tương ứng với tay phải 
- in`R`Đầu ra là:```
R
```Trường hợp này nắm bắt được giải pháp quên đảo hướng cho cửa sau. 

Bây giờ hãy xem xét:```
front
2
```Dấu vết thực hiện: 

-`door = "front"`-`rail = 2`- nhập logic cửa trước 
- ray 2 tương ứng với tay phải 
- in`R`Đầu ra là:```
R
```Trường hợp này xác minh rằng ánh xạ hướng bình thường được xử lý chính xác. 

Cuối cùng, hãy xem xét:```
back
2
```Dấu vết thực hiện: 

-`door = "back"`-`rail = 2`- nhập logic cửa sau 
- ray 2 tương ứng với tay trái 
- in`L`Đầu ra là:```
L
```Điều này xác nhận rằng cả hướng vào và số đường ray đều được xử lý cùng nhau một cách chính xác chứ không phải độc lập.
