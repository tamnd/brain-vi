---
title: "CF 404B - Marathon"
description: "Valera chạy quanh chu vi của một sân vận động hình vuông. Hình vuông có cạnh dài a và tuyến đường đi theo ranh giới theo thứ tự ngược chiều kim đồng hồ. Điểm bắt đầu là góc dưới bên trái (0, 0)."
date: "2026-06-07T01:27:58+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 404
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 237 (Div. 2)"
rating: 1500
weight: 404
solve_time_s: 260
verified: true
draft: false
---

[CF 404B - Marathon](https://codeforces.com/problemset/problem/404/B) 

**Đánh giá:** 1500 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 4 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Valera chạy quanh chu vi của một sân vận động hình vuông. Hình vuông có độ dài cạnh`a`và tuyến đường đi theo ranh giới theo thứ tự ngược chiều kim đồng hồ. 

Điểm bắt đầu là góc dưới bên trái`(0, 0)`. Sau khi chạy`a`mét anh ấy đạt tới`(a, 0)`, nối tiếp cái khác`a`mét anh ấy đạt tới`(a, a)`, vân vân. Tổng chu vi là`4a`, vì vậy sau mỗi vòng đua anh ấy quay lại`(0, 0)`và tiếp tục lặp lại con đường tương tự. 

Huấn luyện viên cho Valera đồ uống mỗi ngày`d`mét. Chúng ta phải xuất tọa độ của Valera sau khi anh ấy chạy`d`,`2d`,`3d`, ...,`nd`mét. 

Điều quan trọng cần lưu ý là chỉ có vị trí dọc theo vòng đua hiện tại mới quan trọng. Nếu Valera đã chạy tổng cộng một quãng đường`s`, thì vị trí của anh ta được xác định bởi`s mod (4a)`, bởi vì mọi chu vi hoàn chỉnh sẽ đưa anh ta trở lại đúng vị trí cũ. 

Các ràng buộc đủ lớn để chúng ta phải tạo ra tối đa`10^5`câu trả lời. Bất kỳ giải pháp nào mô phỏng chuyển động của từng mét đều không thể thực hiện được. Ngay cả khi khoảng cách được biểu diễn dưới dạng số thực, mô phỏng từng bước nhỏ sẽ yêu cầu hàng tỷ thao tác trong trường hợp xấu nhất. Chúng ta cần một công thức trực tiếp tính toán từng câu trả lời trong thời gian không đổi. 

Một số trường hợp đặc biệt có thể dễ dàng gây ra câu trả lời sai. 

Coi như:```
a = 2
d = 2
n = 1
```Sau khi chạy chính xác`2`mét, Valera ở`(2, 0)`, góc đầu tiên Thay vào đó, việc thực hiện bất cẩn bằng cách sử dụng các bất đẳng thức nghiêm ngặt có thể đặt anh ta vào tình thế khó khăn hơn. 

Một ví dụ khác:```
a = 3
d = 12
n = 1
```Từ`12 = 4a`, anh ta hoàn thành đúng một vòng và quay trở lại`(0, 0)`. Nếu chúng ta quên lấy modulo`4a`, chúng ta có thể cố gắng xác định sai vị trí của anh ta bên ngoài chu vi. 

Một trường hợp tế nhị hơn là:```
a = 2
d = 5
n = 1
```Chu vi là`8`, Vì thế`5 mod 8 = 5`. Khoảng cách`5`nằm ở cạnh trên. Vị trí đúng là`(1, 2)`. Giải pháp chỉ giả định một vòng hoặc tính toán chuyển tiếp cạnh không chính xác sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là mô phỏng quá trình chạy. Đối với mỗi sự kiện đồ uống, chúng ta có thể bắt đầu tại`(0,0)`và liên tục di chuyển dọc theo các cạnh cho đến khi đạt được khoảng cách cần thiết. Điều này đúng vì nó đi theo con đường chính xác. 

Vấn đề là quy mô. Khoảng cách có thể lớn như`10^5`, và có tới`10^5`truy vấn. Việc mô phỏng chuyển động tăng dần sẽ đòi hỏi quá nhiều thao tác. Ngay cả việc xử lý từng truy vấn bằng cách trừ đi độ dài các cạnh liên tục cũng là công việc không cần thiết. 

Cấu trúc của đường dẫn đưa ra một giải pháp đơn giản hơn nhiều. Ranh giới sân vận động là một chu kỳ có độ dài`4a`. Sau mỗi vòng đua đầy đủ, người chạy sẽ trở về vị trí cũ. Đối với`i`- lần uống thứ 2, tổng quãng đường đã đi là$$s=i\cdot d$$Chỉ một$$r=s\bmod (4a)$$vấn đề. 

Bây giờ chu vi được chia thành bốn đoạn có chiều dài`a`. 

Nếu như`r`thuộc đoạn đầu tiên, Valera nằm ở cạnh dưới. 

Nếu như`r`thuộc đoạn thứ hai, anh ấy ở rìa bên phải. 

Nếu như`r`thuộc đoạn thứ ba, anh ấy ở cạnh trên. 

Nếu như`r`thuộc đoạn thứ tư, anh ấy ở rìa trái. 

Mỗi trường hợp cho tọa độ thông qua một công thức đơn giản. Vì mọi truy vấn được xử lý độc lập trong thời gian không đổi nên toàn bộ giải pháp sẽ chạy trong`O(n)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · khoảng cách) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính chiều dài chu vi`p = 4 * a`. 
2. Duy trì tổng quãng đường đã đi sau mỗi lần uống. Thay vì tính toán lại`i * d`, chúng ta có thể giữ giá trị đang chạy:`cur = (cur + d) mod p`. 

Sau khi`i`-lần lặp thứ,`cur`bằng`(i*d) mod p`. 
3. Xác định cạnh nào của hình vuông`cur`dối trá. 

Nếu như`0 <= cur <= a`, người chạy ở cạnh dưới. 

Nếu như`a < cur <= 2a`, người chạy ở rìa bên phải. 

Nếu như`2a < cur <= 3a`, người chạy ở cạnh trên. 

Nếu không thì anh ta ở rìa bên trái. 
4. Chuyển khoảng cách dọc theo cạnh đó thành tọa độ. 

Đối với cạnh dưới:$$(x,y)=(cur,0)$$Đối với cạnh phải:$$(x,y)=(a,cur-a)$$Đối với cạnh trên:$$(x,y)=(a-(cur-2a),a)$$Đối với cạnh trái:$$(x,y)=(0,a-(cur-3a))$$5. Xuất tọa độ. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, vị trí của người chạy chỉ phụ thuộc vào việc anh ta đã tiến được bao xa qua vòng đua hiện tại. Lấy modulo`4a`loại bỏ tất cả các vòng đã hoàn thành trong khi vẫn giữ được vị trí chính xác trên chu vi. 

Sự phân hủy khoảng của`[0,4a)`trùng với bốn cạnh của hình vuông. Trong mỗi khoảng, chuyển động xảy ra dọc theo một cạnh thẳng với tốc độ đơn vị, do đó các công thức tọa độ mô tả điểm duy nhất có khoảng cách từ điểm bắt đầu dọc theo chu vi bằng`cur`. Vì mọi giá trị có thể có của`cur`thuộc đúng một cạnh thì thuật toán luôn cho ra vị trí đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a, d = map(float, input().split())
n = int(input())

perimeter = 4.0 * a
cur = 0.0

out = []

for _ in range(n):
    cur = (cur + d) % perimeter

    if cur <= a:
        x = cur
        y = 0.0
    elif cur <= 2.0 * a:
        x = a
        y = cur - a
    elif cur <= 3.0 * a:
        x = 3.0 * a - cur
        y = a
    else:
        x = 0.0
        y = 4.0 * a - cur

    out.append(f"{x:.10f} {y:.10f}")

sys.stdout.write("\n".join(out))
```Phần đầu tiên đọc chiều dài cạnh hình vuông và khoảng thời gian uống. Chu vi được tính một lần vì nó được sử dụng lại cho mọi truy vấn. 

Biến`cur`lưu trữ khoảng cách hiện tại dọc theo chu vi sau khi loại bỏ các vòng đã hoàn thành. Cập nhật nó với```
cur = (cur + d) % perimeter
```tránh tính toán nhiều lần`i * d`và giữ các giá trị nhỏ về mặt số lượng. 

Bốn nhánh điều kiện tương ứng trực tiếp với bốn cạnh của hình vuông. Các công thức bắt nguồn từ khoảng cách mà người chạy dọc theo cạnh hiện tại. 

Một điểm tinh tế là xử lý các vị trí góc. sử dụng`<=`giữ khoảng cách chính xác bằng`a`,`2a`, hoặc`3a`trên một mặt được xác định rõ ràng. Các tọa độ giống hệt nhau bất kể cạnh nào được chọn, vì vậy điều này tránh được sự mơ hồ. 

Âm lượng đầu ra lớn, lên tới`10^5`dòng. Thu thập câu trả lời trong danh sách và in một lần nhanh hơn nhiều so với viết từng dòng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 5
2
```Chu vi = 8. 

| Uống # | cur = khoảng cách mod 8 | Bên | Tọa độ | 
| --- | --- | --- | --- | 
| 1 | 5 | Đầu trang | (1, 2) | 
| 2 | 2 | Dưới cùng | (2, 0) | 

Đầu ra:```
1.0000000000 2.0000000000
2.0000000000 0.0000000000
```Lần uống đầu tiên diễn ra sau khi vượt qua hai góc và chạm tới mép trên. Lần uống thứ hai diễn ra sau một vòng hoàn chỉnh cộng thêm hai mét nữa, đặt Valera ở vị trí thứ hai.`(2,0)`. 

### Ví dụ 2 

đầu vào:```
3 4
4
```Chu vi = 12. 

| Uống # | cur | Bên | Tọa độ | 
| --- | --- | --- | --- | 
| 1 | 4 | Đúng | (3, 1) | 
| 2 | 8 | Đầu trang | (1, 3) | 
| 3 | 0 | Góc dưới cùng | (0, 0) | 
| 4 | 4 | Đúng | (3, 1) | 

Dấu vết này thể hiện tính chất tuần hoàn của tuyến đường. Sau đúng một chiều dài chu vi,`cur`trở thành 0 và người chạy quay trở lại điểm xuất phát. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Công việc liên tục cho mỗi`n`vị trí uống | 
| Không gian | O(1) phụ trợ, lưu trữ đầu ra O(n) | Chỉ có một số biến được sử dụng ngoài bộ đệm đầu ra | 

Với nhiều nhất`10^5`truy vấn, một`O(n)`giải pháp thực hiện thoải mái trong giới hạn. Việc sử dụng bộ nhớ cũng nhỏ, ngay cả khi lưu trữ tất cả các dòng đầu ra trước khi in. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    a, d = map(float, input().split())
    n = int(input())

    perimeter = 4.0 * a
    cur = 0.0

    out = []

    for _ in range(n):
        cur = (cur + d) % perimeter

        if cur <= a:
            x, y = cur, 0.0
        elif cur <= 2.0 * a:
            x, y = a, cur - a
        elif cur <= 3.0 * a:
            x, y = 3.0 * a - cur, a
        else:
            x, y = 0.0, 4.0 * a - cur

        out.append(f"{x:.10f} {y:.10f}")

    return "\n".join(out)

# provided sample
assert run("2 5\n2\n") == (
    "1.0000000000 2.0000000000\n"
    "2.0000000000 0.0000000000"
)

# minimum-size input
assert run("1 1\n1\n") == "1.0000000000 0.0000000000"

# exactly one full lap
assert run("3 12\n1\n") == "0.0000000000 0.0000000000"

# corner transitions
assert run("2 2\n4\n") == (
    "2.0000000000 0.0000000000\n"
    "2.0000000000 2.0000000000\n"
    "0.0000000000 2.0000000000\n"
    "0.0000000000 0.0000000000"
)

# repeated cycling
assert run("3 4\n4\n") == (
    "3.0000000000 1.0000000000\n"
    "1.0000000000 3.0000000000\n"
    "0.0000000000 0.0000000000\n"
    "3.0000000000 1.0000000000"
)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1`|`(1,0)`| Phiên bản hợp lệ tối thiểu | 
|`3 12 / 1`|`(0,0)`| Chu vi chính xác bội số | 
|`2 2 / 4`| Bốn góc theo thứ tự | Xử lý ranh giới | 
|`3 4 / 4`| Vị trí lặp lại sau một vòng | Logic modulo đúng | 

## Vỏ cạnh 

### Hạ cánh chính xác ở một góc 

đầu vào:```
2 2
1
```Vị trí chu vi là`2`, bằng độ dài cạnh. Thuật toán đi vào nhánh đầu tiên (`cur <= a`) và kết quả đầu ra:```
2.0000000000 0.0000000000
```Đây là góc chính xác. Bất kỳ cách giải thích cạnh lân cận nào cũng sẽ tạo ra cùng tọa độ, do đó kết quả là rõ ràng. 

### Khoảng cách bằng toàn bộ chu vi 

đầu vào:```
3 12
1
```Chu vi là`12`. 

Thuật toán tính toán:```
cur = 12 mod 12 = 0
```Từ`cur = 0`, tọa độ trở thành:```
0.0000000000 0.0000000000
```Người chạy đã hoàn thành hết một vòng và quay lại điểm xuất phát. 

### Khoảng cách lớn kéo dài nhiều vòng 

đầu vào:```
2 21
1
```Chu vi là`8`. 

Thuật toán tính toán:```
cur = 21 mod 8 = 5
```Từ`5`nằm giữa`4`Và`6`, người chạy ở cạnh trên. 

Tọa độ:```
x = 6 - 5 = 1
y = 2
```Đầu ra:```
1.0000000000 2.0000000000
```Chỉ có phần còn lại là quan trọng. Hai mươi mét tính từ vòng đua đã hoàn thành không ảnh hưởng đến vị trí cuối cùng. 

### Bao bọc lặp đi lặp lại 

đầu vào:```
2 7
3
```Chu vi = 8. 

Dãy số dư là:```
7, 6, 5
```sản xuất:```
0 1
0 2
1 2
```Thuật toán không bao giờ theo dõi các vòng hoàn chỉnh một cách rõ ràng. Hoạt động modulo giữ cho mọi truy vấn được ánh xạ vào chu trình chu vi hiện tại, đây chính xác là thông tin cần thiết để xác định vị trí.
