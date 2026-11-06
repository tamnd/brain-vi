---
title: "CF 124A – Số vị trí"
description: "Petr đang đứng ở đâu đó trong một hàng gồm n người. Các vị trí được đánh số từ 1 ở phía trước đến n ở phía sau. Anh ấy biết hai điều về vị trí của mình. Ít nhất có một người đứng trước anh ta và nhiều nhất b người đứng sau anh ta."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 124
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 92 (Div. 2 Only)"
rating: 1000
weight: 124
solve_time_s: 87
verified: true
draft: false
---

[CF 124A - Số lượng vị trí](https://codeforces.com/problemset/problem/124/A) 

**Đánh giá:** 1000 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Petr đang đứng ở đâu đó trong một hàng có chứa`n`mọi người. Các vị trí được đánh số từ`1`ở phía trước để`n`ở phía sau. 

Anh ấy biết hai điều về vị trí của mình. Ít nhất`a`mọi người đang đứng trước mặt anh ta, và nhiều nhất là`b`mọi người đang đứng đằng sau anh ta. Chúng ta phải đếm xem có bao nhiêu vị trí khác nhau thỏa mãn cả hai điều kiện. 

Nếu Petr đứng ở vị trí`p`, thì chính xác`p - 1`mọi người đang ở trước mặt anh ấy và chính xác`n - p`mọi người đang ở phía sau anh ta. Nhiệm vụ trở thành tìm có bao nhiêu số nguyên`p`thỏa mãn cả hai bất đẳng thức: 

-`p - 1 >= a`-`n - p <= b`Các ràng buộc là rất nhỏ, vì`n <= 100`. Ngay cả một vòng lặp mạnh mẽ trên mọi vị trí cũng đủ nhanh. Không cần tối ưu hóa nâng cao, cấu trúc dữ liệu hoặc lập trình động. Thách thức chính là chuyển từ ngữ thành các bất đẳng thức chính xác mà không mắc phải lỗi sai nào. 

Sai lầm thường gặp nhất là hiểu sai cụm từ “không ít hơn`a`người ở phía trước". Điều này có nghĩa là`at least a`, không chính xác`a`. 

Hãy xem xét ví dụ này:```
5 2 1
```Các vị trí hợp lệ là`3`Và`4`. 

Chức vụ`5`không hợp lệ vì có`0`những người đứng sau Petr, điều này được cho phép, nhưng có`4`người ở phía trước, điều này cũng được cho phép. Trên thực tế vị trí`5`cũng hợp lệ. Việc thực hiện bất cẩn giả định chính xác`a`những người ở phía trước sẽ chỉ trả lại một vị trí không chính xác. 

Một trường hợp cạnh tinh tế khác xuất hiện khi`b = 0`. 

Ví dụ:```
4 1 0
```Petr phải đứng ở phía sau vì không ai có thể đứng sau anh ấy. Vị trí hợp lệ duy nhất là`4`. Một cách giải thích sai lầm như`behind < b`thay vì`behind <= b`sẽ từ chối câu trả lời đúng. 

Ngoài ra còn có một bẫy dễ dàng khi chuyển đổi giữa các vị trí và số lượng. 

Ví dụ:```
3 0 2
```Tất cả các vị trí đều hợp lệ. Chức vụ`1`có`0`người ở phía trước và`2`phía sau. Chức vụ`3`có`2`ở phía trước và`0`phía sau. Quên rằng việc đánh số vị trí bắt đầu từ`1`thường phá vỡ các trường hợp ranh giới này. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu kiểm tra trực tiếp mọi vị trí có thể từ`1`ĐẾN`n`. 

Đối với mỗi vị trí`p`, chúng tôi tính toán: 

- người phía trước =`p - 1`- người đằng sau =`n - p`Nếu cả hai điều kiện đều đúng, chúng ta sẽ tăng câu trả lời. 

Điều này hoạt động vì các ràng buộc là cực kỳ nhỏ. Tệ nhất là chúng tôi kiểm tra`100`các vị trí, có hiệu quả tức thời. 

Phần thú vị là nhận ra rằng chúng ta có thể rút ra câu trả lời bằng toán học mà không cần lặp lại. 

Từ:```
p - 1 >= a
```chúng tôi nhận được:```
p >= a + 1
```Từ:```
n - p <= b
```chúng tôi nhận được:```
p >= n - b
```Vì vậy, vị trí của Petr phải đồng thời thỏa mãn cả hai giới hạn dưới:```
p >= max(a + 1, n - b)
```Vị trí lớn nhất có thể luôn luôn là`n`, vì vậy mọi vị trí hợp lệ đều nằm trong:```
[max(a + 1, n - b), n]
```Số số nguyên trong khoảng này là:```
n - max(a + 1, n - b) + 1
```Điều này đơn giản hóa thành công thức nhỏ gọn nổi tiếng:```
min(n - a, b + 1)
```Cả hai phương pháp đều được chấp nhận hoàn toàn ở đây. Công thức sạch hơn và có thời gian không đổi, trong khi ban đầu, sức mạnh vũ phu dễ dàng được rút ra hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`,`a`, Và`b`. 
2. Petr phải có ít nhất`a`người trước mặt anh. 

Kể từ vị trí`p`có`p - 1`người ở phía trước, vị trí hợp lệ nhỏ nhất là`a + 1`. 
3. Tối đa Petr phải có`b`những người đứng sau anh ta. 

Kể từ vị trí`p`có`n - p`những người đứng sau, chúng tôi giải quyết:```
n - p <= b
```mang lại:```
p >= n - b
```4. Hai giới hạn dưới này chặt chẽ hơn sẽ xác định vị trí hợp lệ đầu tiên:```
start = max(a + 1, n - b)
```5. Mọi vị trí từ`start`bởi vì`n`là hợp lệ. 

Đếm chúng bằng cách sử dụng độ dài khoảng:```
answer = n - start + 1
```6. In câu trả lời. 

### Tại sao nó hoạt động 

Một vị trí có giá trị khi và chỉ nếu nó thỏa mãn đồng thời cả hai ràng buộc. 

Điều kiện đầu tiên loại trừ các vị trí quá gần phía trước. Điều thứ hai cũng loại trừ các vị trí quá xa về phía trước, vì có quá nhiều người sẽ ở lại phía sau Petr. 

Sau khi chuyển đổi cả hai câu lệnh thành bất đẳng thức trên`p`, mọi vị trí hợp lệ tạo thành một khoảng liên tục kết thúc tại`n`. Đếm các số nguyên trong khoảng đó sẽ cho ra chính xác số vị trí hợp pháp, không thiếu cũng không thêm trường hợp nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, a, b = map(int, input().split())

start = max(a + 1, n - b)
answer = n - start + 1

print(answer)
```Chương trình tuân theo đạo hàm toán học trực tiếp từ hướng dẫn. 

biểu hiện`a + 1`là vị trí đầu tiên có ít nhất`a`người ở phía trước. Đây là nơi phổ biến nhất để mắc lỗi ngẫu nhiên. Chức vụ`a`sẽ chỉ có`a - 1`người ở phía trước. 

biểu hiện`n - b`xuất phát từ việc sắp xếp lại tình trạng đối với những người đứng sau Petr. Một lỗi thường gặp khác là viết`n - b + 1`, làm dịch chuyển khoảng không chính xác. 

Cuối cùng,`n - start + 1`tính toán kích thước của một khoảng bao gồm. các`+1`quan trọng vì cả hai điểm cuối đều được tính là vị trí hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 1 1
```| Biến | Giá trị | 
| --- | --- | 
| n | 3 | 
| một | 1 | 
| b | 1 | 
| một + 1 | 2 | 
| n - b | 2 | 
| bắt đầu | 2 | 
| trả lời | 2 | 

Các vị trí hợp lệ là`2`Và`3`. 

Ví dụ này cho thấy câu trả lời tính mọi vị trí từ giới hạn dưới được tính toán cho đến cuối dòng. 

### Ví dụ 2 

đầu vào:```
5 2 3
```| Biến | Giá trị | 
| --- | --- | 
| n | 5 | 
| một | 2 | 
| b | 3 | 
| một + 1 | 3 | 
| n - b | 2 | 
| bắt đầu | 3 | 
| trả lời | 3 | 

Các vị trí hợp lệ là`3`,`4`, Và`5`. 

Dấu vết này chứng tỏ rằng giới hạn dưới chặt chẽ hơn sẽ kiểm soát câu trả lời. Mặc dù`n - b = 2`, Petr vẫn chưa thể đứng vững trước vị trí`3`bởi vì anh ấy cần ít nhất hai người ở phía trước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu bổ sung | 

Các ràng buộc là cực kỳ nhỏ, nhưng dù sao thì giải pháp vẫn là thời gian không đổi. Nó dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    import sys
    input = sys.stdin.readline

    n, a, b = map(int, input().split())

    start = max(a + 1, n - b)
    answer = n - start + 1

    print(answer)

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
assert run("3 1 1\n") == "2\n", "sample 1"

# custom cases
assert run("1 0 0\n") == "1\n", "single person line"
assert run("4 1 0\n") == "1\n", "must stand last"
assert run("5 0 4\n") == "5\n", "all positions valid"
assert run("100 99 99\n") == "1\n", "maximum boundary"
assert run("6 2 2\n") == "3\n", "middle interval"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 0`|`1`| Đầu vào kích thước tối thiểu | 
|`4 1 0`|`1`| Trường hợp ranh giới không ai có thể đứng sau Petr | 
|`5 0 4`|`5`| Mọi vị trí đều hợp lệ | 
|`100 99 99`|`1`| Giá trị tối đa chỉ có một vị trí hợp lệ | 
|`6 2 2`|`3`| Đếm khoảng thời gian chính xác ở phạm vi giữa | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
4 1 0
```Thuật toán tính toán:```
a + 1 = 2
n - b = 4
start = 4
answer = 1
```Vị trí duy nhất`4`hoạt động vì Petr không thể có ai đứng sau anh ta. Thuật toán xử lý chính xác điều kiện bao gồm`<= b`. 

Bây giờ hãy xem xét:```
5 0 4
```Chúng tôi nhận được:```
a + 1 = 1
n - b = 1
start = 1
answer = 5
```Mọi vị trí đều hợp lệ. Điều này xác nhận công thức xử lý trường hợp phạm vi đầy đủ một cách chính xác. 

Cuối cùng, hãy xem xét:```
3 0 2
```Việc tính toán trở thành:```
a + 1 = 1
n - b = 1
start = 1
answer = 3
```Cả ba vị trí đều được tính. Trường hợp này xác minh rằng việc chuyển đổi giữa các vị trí và số người ở phía trước được xử lý mà không có lỗi lẻ tẻ.
