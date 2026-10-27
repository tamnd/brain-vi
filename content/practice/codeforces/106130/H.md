---
title: "CF 106130H - \u6211\u4e0d\u5403\u6c34\u679c"
description: "Chúng ta có một chiếc bánh $n lần n$ được biểu diễn dưới dạng lưới. Một ô không chứa quả (0) hoặc không chứa quả (1). Miếng mà Xiao Z cắt phải ở góc trên bên trái của chiếc bánh."
date: "2026-06-19T19:50:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106130
codeforces_index: "H"
codeforces_contest_name: "GDUT 2025 Monthly competition"
rating: 0
weight: 106130
solve_time_s: 53
verified: true
draft: false
---

[CF 106130H - \u6211\u4e0d\u5403\u6c34\u679c](https://codeforces.com/problemset/problem/106130/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một$n \times n$bánh được biểu diễn dưới dạng lưới. Một tế bào không chứa quả (`0`) hoặc quả (`1`). 

Miếng mà Xiao Z cắt phải ở góc trên bên trái của chiếc bánh. Vì chỉ được phép cắt dọc theo ranh giới hàng và cột nên bất kỳ phần nào như vậy là hình chữ nhật có góc trên cùng bên trái được cố định tại ô$(1,1)$. Hình chữ nhật được xác định hoàn toàn bằng cách chọn hàng dưới cùng và cột bên phải của nó. 

Nhiệm vụ là tìm hình chữ nhật có diện tích lớn nhất$$[1..r] \times [1..c]$$sao cho mọi tế bào bên trong nó đều`0`. 

Kích thước lưới tối đa là 1000, vì vậy có tới một triệu ô. Đọc đầu vào đã có giá$O(n^2)$, có nghĩa là một giải pháp được chấp nhận sẽ nằm ở mức độ phức tạp đó. Bất kỳ cách tiếp cận nào kiểm tra tất cả các hình chữ nhật có thể một cách độc lập sẽ quá tốn kém. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Nếu ô trên cùng bên trái chứa một loại trái cây thì không tồn tại hình chữ nhật hợp lệ nào vì mọi hình chữ nhật ứng viên đều phải bao gồm ô đó. 

Ví dụ:```
1
1
```Câu trả lời là:```
0
```Một giải pháp bất cẩn cho rằng hình chữ nhật luôn có diện tích dương sẽ thất bại ở đây. 

Một trường hợp tinh vi khác là khi một loại trái cây xuất hiện ở xa bên phải ở hàng đầu.```
3
0 0 1
0 0 0
0 0 0
```Hình chữ nhật hợp lệ lớn nhất chỉ có chiều rộng 2, mặc dù các hàng sau hoàn toàn trống. Câu trả lời là$3 \times 2 = 6$. Bất kỳ phương pháp nào xem xét các hàng một cách độc lập mà không ghi nhớ các hạn chế được áp đặt bởi các hàng trước đó sẽ trả về 9 không chính xác. 

Trường hợp thứ ba là khi hàng giới hạn xuất hiện ở giữa.```
4
0 0 0 0
0 1 0 0
0 0 0 0
0 0 0 0
```Hàng thứ hai chặn mọi hình chữ nhật rộng hơn 1. Hình chữ nhật hợp lệ lớn nhất là toàn bộ cột đầu tiên, diện tích 4. Hạn chế do một hàng tạo ra sẽ ảnh hưởng đến tất cả các hàng bên dưới nó. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực trực tiếp nhất là thử mọi hàng dưới cùng có thể$r$và cột bên phải$c$, sau đó kiểm tra xem mọi ô bên trong hình chữ nhật có$[1..r]\times[1..c]$là số không. 

có$n^2$hình chữ nhật có thể. Việc kiểm tra một hình chữ nhật có thể yêu cầu kiểm tra tới$n^2$tế bào, cho$O(n^4)$thời gian. Với$n=1000$, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là mọi hình chữ nhật hợp lệ đều được neo ở góc trên cùng bên trái. 

Đối với mỗi hàng, chúng ta chỉ quan tâm đến việc có thể kéo dài bao xa từ cột 1 trước khi gặp quả đầu tiên. 

Cho phép$$width_i$$là số số 0 liên tiếp bắt đầu từ cột 1 trong hàng$i$. 

Ví dụ, nếu một hàng là```
0 0 0 1 0
```sau đó`width = 3`. 

Giả sử chúng ta muốn một hình chữ nhật kết thúc ở hàng$r$. Chiều rộng của nó không được vượt quá chiều rộng nhỏ nhất cho phép trong số chiều rộng đầu tiên$r$các hàng, bởi vì mỗi hàng bên trong hình chữ nhật phải không có trái cây. 

Vì vậy với mỗi hàng$r$,$$\text{max valid width}
=
\min(width_1,\ldots,width_r).$$Hình chữ nhật tốt nhất kết thúc ở hàng$r$có diện tích$$r \times \min(width_1,\ldots,width_r).$$Chúng tôi có thể xử lý các hàng từ trên xuống dưới trong khi vẫn duy trì độ rộng tối thiểu đang chạy. Điều này làm giảm vấn đề xuống còn một lần truyền qua lưới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^4)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n^2)$|$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới. 
2. Với mỗi hàng, hãy tính`width`, số các số 0 liên tiếp bắt đầu từ cột đầu tiên. 

Dừng quét hàng ngay lần đầu tiên`1`xuất hiện, bởi vì bất cứ thứ gì ở bên phải của nó không bao giờ có thể thuộc về hình chữ nhật hợp lệ ở trên cùng bên trái. 
3. Duy trì`min_width`, tối thiểu`width`được thấy cho đến nay trong số tất cả các hàng được xử lý. 

Giá trị này thể hiện chiều rộng lớn nhất mà mỗi hàng từ 1 đến hàng hiện tại có thể hỗ trợ đồng thời. 
4. Sau khi xử lý hàng`i`, tính toán$$area = (i+1) \times min\_width$$bởi vì hình chữ nhật kéo dài đầu tiên`i+1`hàng và có thể có nhiều nhất`min_width`cột rộng. 
5. Cập nhật câu trả lời với diện tích tối đa gặp phải. 
6. Xuất ra câu trả lời. 

### Tại sao nó hoạt động 

Đối với một hình chữ nhật kết thúc ở hàng$r$, mỗi hàng từ 1 đến$r$phải không có quả trong phạm vi chiều rộng đã chọn. Hàng ngang$k$cho phép nhiều nhất`width_k`cột. Chiều rộng lớn hơn mức tối thiểu của các giá trị này sẽ bao gồm một loại trái cây trong ít nhất một hàng, làm cho hình chữ nhật không hợp lệ. 

Ngược lại, việc chọn chính xác độ rộng tối thiểu sẽ đảm bảo rằng mọi ô trong hình chữ nhật đều thuộc về tiền tố 0 của hàng của nó, do đó toàn bộ hình chữ nhật không có kết quả. 

Do đó, hình chữ nhật hợp lệ lớn nhất kết thúc ở hàng$r$có chiều rộng$$\min(width_1,\ldots,width_r),$$và kiểm tra tất cả các hàng sẽ xem xét mọi ranh giới đáy có thể có. Diện tích lớn nhất trong số đó là đáp án tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    min_width = n
    ans = 0

    for i in range(n):
        row = list(map(int, input().split()))

        width = 0
        while width < n and row[width] == 0:
            width += 1

        min_width = min(min_width, width)
        ans = max(ans, (i + 1) * min_width)

    print(ans)

solve()
```Nhiệm vụ đầu tiên của mỗi hàng là tìm xem có bao nhiêu số 0 liên tiếp xuất hiện từ cạnh trái. Sau khi đạt được quả đầu tiên, việc mở rộng sang phải xa hơn là không thể đối với bất kỳ hình chữ nhật hợp lệ nào ở trên cùng bên trái, vì vậy không có gì ngoài vị trí đó ảnh hưởng đến câu trả lời.`min_width`lưu trữ giới hạn chiều rộng chặt chẽ nhất trong số tất cả các hàng được xử lý cho đến nay. Khi một hàng mới được thêm vào, hình chữ nhật chỉ có thể rộng bằng chiều rộng tiền tố nhỏ nhất nhìn thấy ở bất kỳ đâu phía trên nó. 

Việc tính diện tích sử dụng`(i + 1)`bởi vì các hàng được xử lý bằng cách lập chỉ mục dựa trên 0 trong vòng lặp, trong khi chiều cao hình chữ nhật là số lượng hàng thực tế được bao gồm. 

Không cần xử lý đặc biệt khi ô trên cùng bên trái chứa trái cây. Trong trường hợp đó hàng đầu tiên có`width = 0`, gây ra`min_width`trở thành 0 và tất cả các khu vực được tính toán vẫn bằng 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
0 0 0
0 0 1
1 1 1
```Độ rộng tiền tố hàng là:```
3, 2, 0
```| Hàng | chiều rộng | chiều rộng tối thiểu | Khu vực | Tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | 3 | 3 | 
| 2 | 2 | 2 | 4 | 4 | 
| 3 | 0 | 0 | 0 | 4 | 

Câu trả lời là:```
4
```Hình chữ nhật tốt nhất sử dụng hai hàng đầu tiên và hai cột đầu tiên. 

### Ví dụ 2 

đầu vào:```
3
1 0 0
1 0 1
1 1 1
```Độ rộng tiền tố hàng là:```
0, 0, 0
```| Hàng | chiều rộng | chiều rộng tối thiểu | Khu vực | Tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 0 | 
| 2 | 0 | 0 | 0 | 0 | 
| 3 | 0 | 0 | 0 | 0 | 

Câu trả lời là:```
0
```Vì ô trên cùng bên trái đã chứa một loại trái cây nên không có hình chữ nhật hợp lệ nào tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Mỗi ô lưới được đọc một lần và mỗi hàng chỉ được quét cho đến kết quả đầu tiên | 
| Không gian |$O(1)$thêm | Chỉ có một số biến được duy trì bên cạnh hàng hiện tại | 

Bản thân đầu vào chứa$n^2$các giá trị, vì vậy$O(n^2)$thời gian về cơ bản là tối ưu. Với$n \le 1000$, việc xử lý một triệu tế bào có thể dễ dàng quản lý được. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n = int(input())

    min_width = n
    ans = 0

    for i in range(n):
        row = list(map(int, input().split()))

        width = 0
        while width < n and row[width] == 0:
            width += 1

        min_width = min(min_width, width)
        ans = max(ans, (i + 1) * min_width)

    return str(ans)

# sample-like cases
assert run("3\n0 0 0\n0 0 1\n1 1 1\n") == "4"
assert run("3\n1 0 0\n1 0 1\n1 1 1\n") == "0"

# minimum size, empty cell
assert run("1\n0\n") == "1"

# minimum size, fruit
assert run("1\n1\n") == "0"

# all zeros
assert run("3\n0 0 0\n0 0 0\n0 0 0\n") == "9"

# limiting row in the middle
assert run("4\n0 0 0 0\n0 1 0 0\n0 0 0 0\n0 0 0 0\n") == "4"

# width shrinks gradually
assert run("4\n0 0 0 0\n0 0 0 1\n0 0 1 1\n0 1 1 1\n") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0`|`1`| Trường hợp hợp lệ nhỏ nhất | 
|`1 / 1`|`0`| Trái cây trên cùng bên trái | 
| Tất cả số không |`9`| Toàn bộ lưới có thể được lấy | 
| Hạn chế ở hàng thứ hai |`4`| Chạy logic chiều rộng tối thiểu | 
| Chiều rộng dần dần thu hẹp |`6`| Tối đa có thể xảy ra trước hàng cuối cùng | 

## Vỏ cạnh 

### Ô trên cùng bên trái chứa một loại trái cây 

đầu vào:```
2
1 0
0 0
```Hàng đầu tiên có`width = 0`. Ngay lập tức`min_width`trở thành 0. 

| Hàng | chiều rộng | chiều rộng tối thiểu | Khu vực | 
| --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 
| 2 | 2 | 0 | 0 | 

Thuật toán cho kết quả là 0, điều này đúng vì mọi hình chữ nhật hợp lệ phải chứa kết quả tại$(1,1)$. 

### Hàng đầu giới hạn tất cả các hàng sau 

đầu vào:```
3
0 1 0
0 0 0
0 0 0
```Chiều rộng là:```
1, 3, 3
```Quá trình xử lý mang lại: 

| Hàng | chiều rộng | chiều rộng tối thiểu | Khu vực | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 3 | 1 | 2 | 
| 3 | 3 | 1 | 3 | 

Câu trả lời là 3. Mặc dù hai hàng cuối cùng cho phép chiều rộng là 3, nhưng trái cây ở hàng đầu tiên giới hạn vĩnh viễn mọi hình chữ nhật hợp lệ ở trên cùng bên trái ở chiều rộng 1. 

### Hình chữ nhật đẹp nhất không kết thúc ở hàng cuối cùng 

đầu vào:```
4
0 0 0
0 0 0
0 0 0
0 1 1
```Chiều rộng là:```
3, 3, 3, 1
```| Hàng | chiều rộng | chiều rộng tối thiểu | Khu vực | 
| --- | --- | --- | --- | 
| 1 | 3 | 3 | 3 | 
| 2 | 3 | 3 | 6 | 
| 3 | 3 | 3 | 9 | 
| 4 | 1 | 1 | 4 | 

Diện tích tối đa là 9 ở hàng 3. Thuật toán kiểm tra mọi hàng dưới cùng có thể có, do đó nó tìm thấy chính xác một hình chữ nhật kết thúc trước hàng cuối cùng.
