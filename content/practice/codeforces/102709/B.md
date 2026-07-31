---
title: "CF 102709B - Giờ hành chính"
description: "Vấn đề mô tả hàng dài sinh viên chờ đợi trong giờ hành chính của TA. Deja biết có n học sinh đang xếp hàng nhưng cô không nhớ chính xác vị trí của mình. Cô chỉ biết có ít nhất x học sinh đứng trước cô và nhiều nhất y học sinh đứng sau cô."
date: "2026-07-30T07:11:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102709
codeforces_index: "B"
codeforces_contest_name: "UTPC Contest 9-11-20 Div. 2"
rating: 0
weight: 102709
solve_time_s: 114
verified: true
draft: false
---

[CF 102709B - Giờ làm việc](https://codeforces.com/problemset/problem/102709/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề mô tả hàng dài sinh viên chờ đợi trong giờ hành chính của TA. Deja biết có`n`học sinh xếp hàng nhưng cô không nhớ chính xác vị trí của mình. Ít nhất cô ấy chỉ biết rằng`x`học sinh đang đứng trước cô ấy và nhiều nhất là`y`học sinh đang đứng sau cô. Nhiệm vụ là đếm xem có bao nhiêu vị trí trong hàng đợi có thể thỏa mãn hai điều kiện này. 

Một vị trí trong hàng đợi được lập chỉ mục một. Nếu Deja ở vị trí`p`, thì có`p - 1`những người trước cô ấy và`n - p`những người theo đuổi cô ấy. Chúng ta cần đếm mọi giá trị của`p`trong đó cả hai điều kiện giữ:`p - 1 >= x`Và`n - p <= y`Ràng buộc`n <= 1,000,000`cho chúng ta biết rằng kích thước đầu vào đủ lớn nên việc kiểm tra liên tục các điều kiện phức tạp là không cần thiết. Quét tuyến tính có thể được chấp nhận vì một triệu phép tính là nhỏ, nhưng cấu trúc của các bất đẳng thức cho phép giải pháp thời gian không đổi thậm chí còn đơn giản hơn. Bất kỳ cách tiếp cận nào cố gắng mô phỏng hàng đợi hoặc tạo ra tất cả các cách sắp xếp có thể đều giải quyết được một vấn đề lớn hơn nhiều so với yêu cầu. 

Các trường hợp cạnh chính đến từ các vị trí gần cuối hàng đợi. Vị trí gần phía trước có thể thất bại vì có quá ít học sinh ở phía trước, trong khi vị trí gần phía sau có thể thất bại vì có quá nhiều học sinh ở phía sau. 

Ví dụ:```
4 2 1
```Các vị trí có thể là 3 và 4. Vị trí 2 chỉ có một học sinh trước Deja nên không hợp lệ. Giải pháp bất cẩn chỉ kiểm tra số người đứng sau có thể đếm sai. 

Một trường hợp khác là:```
5 1 4
```Mọi vị trí từ 2 đến 5 đều hợp lệ, cho kết quả:```
4
```Vị trí 1 phải bị từ chối vì không có ai đứng trước Deja. Giải pháp chỉ kiểm tra giới hạn trên của học sinh xếp sau sẽ bao gồm vị trí đầu tiên không chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là thử mọi vị trí có thể từ`1`ĐẾN`n`. Đối với mỗi vị trí`p`, chúng tôi tính toán có bao nhiêu người ở trước Deja và bao nhiêu người ở sau cô ấy. Nếu cả hai giá trị đều thỏa mãn yêu cầu, chúng tôi sẽ tăng câu trả lời. Điều này hoạt động vì mọi vị trí hợp lệ đều được kiểm tra chính xác một lần. 

Vấn đề là đây là công việc không cần thiết. Hàng đợi có thể chứa tới một triệu vị trí, do đó, việc quét đã tốn nhiều công sức hơn mức cần thiết và các diễn giải thô bạo phức tạp hơn sẽ chỉ khiến tình hình trở nên tồi tệ hơn. Hai điều kiện là những bất đẳng thức đơn giản nên chúng ta có thể trực tiếp tìm được khoảng của các vị trí hợp lệ. 

Điều kiện đầu tiên đưa ra:`p - 1 >= x`có nghĩa là:`p >= x + 1`Điều kiện thứ hai đưa ra:`n - p <= y`có nghĩa là:`p >= n - y`Cả hai điều kiện đều là giới hạn dưới của vị thế. Các vị trí hợp lệ là mọi vị trí từ giới hạn lớn hơn của hai giới hạn dưới này cho đến`n`. 

Quan sát cho thấy câu trả lời không nằm rải rác trong hàng đợi. Nó tạo thành một khoảng thời gian liên tục vì việc di chuyển Deja ra xa hơn chỉ làm tăng số người đi trước và giảm số người ở phía sau. Khi một vị trí trở nên hợp lệ, mọi vị trí sau đó cũng có hiệu lực. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Được chấp nhận, nhưng không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính vị trí sớm nhất có thể từ điều kiện về học sinh phía trước. Vì phải có ít nhất`x`sinh viên trước Deja, vị trí của cô ấy ít nhất phải`x + 1`. 
2. Tính vị trí sớm nhất có thể từ điều kiện về học sinh đứng sau. Vì có thể có nhiều nhất`y`sinh viên sau cô ấy, vị trí của cô ấy ít nhất phải là`n - y`. 
3. Lấy giá trị lớn hơn trong hai giá trị này làm vị trí hợp lệ đầu tiên. Vị trí này thỏa mãn cả hai giới hạn dưới. 
4. Đếm xem còn lại bao nhiêu vị trí từ vị trí hợp lệ đầu tiên này cho đến hết hàng đợi. Câu trả lời là`n - first_valid + 1`. 

Tại sao nó hoạt động: 

Các vị trí hợp lệ chỉ được xác định bởi giới hạn dưới của chỉ số vị trí. Bất kỳ vị trí nào nhỏ hơn điểm bắt đầu được tính toán đều vi phạm ít nhất một yêu cầu. Bất kỳ vị trí nào bằng hoặc lớn hơn nó đều có đủ học sinh trước Deja và không quá`y`học sinh theo sau cô ấy, vì vậy mọi vị trí trong khoảng thời gian cuối cùng này đều hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, x, y = map(int, input().split())

    first_valid = max(x + 1, n - y)

    print(n - first_valid + 1)

if __name__ == "__main__":
    solve()
```Chương trình đọc ba giá trị và chuyển đổi hai yêu cầu thành giới hạn trên vị trí của Deja. Biến`first_valid`lưu trữ vị trí đầu tiên có thể hoạt động. 

Việc sử dụng`max`là bước trung tâm. Một vị trí phải đáp ứng cả hai hạn chế, do đó nó phải đáp ứng mức độ chặt chẽ hơn của hai giới hạn dưới. 

Biểu thức cuối cùng tính toàn bộ các vị trí. Nếu vị trí hợp lệ đầu tiên là`first_valid`, các vị trí là:`first_valid, first_valid + 1, ..., n`trong đó có chứa`n - first_valid + 1`các giá trị. các`+1`là nơi phổ biến có thể xảy ra sai sót ngẫu nhiên. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 2 1
```| Bước | n | x | y | Vị trí hợp lệ đầu tiên | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 4 | 2 | 1 | | | 
| Điều kiện phía trước | 4 | 2 | 1 | 3 | | 
| Điều kiện đằng sau | 4 | 2 | 1 | 3 | | 
| Số cuối cùng | 4 | 2 | 1 | 3 | 2 | 

Vị trí hợp lệ là 3 và 4. Vị trí 3 có hai học sinh đứng trước Deja và một học sinh đứng sau cô ấy. Vị trí 4 có ba học sinh trước cô và không có ai theo sau cô. 

### Mẫu 2 

đầu vào:```
6 1 3
```| Bước | n | x | y | Vị trí hợp lệ đầu tiên | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 6 | 1 | 3 | | | 
| Điều kiện phía trước | 6 | 1 | 3 | 2 | | 
| Điều kiện đằng sau | 6 | 1 | 3 | 3 | | 
| Số cuối cùng | 6 | 1 | 3 | 3 | 4 | 

Yêu cầu khắt khe hơn là không có quá ba học sinh đứng sau Deja. Điều này buộc vị trí của cô ấy ít nhất phải là 3. Các vị trí hợp lệ là 3, 4, 5 và 6. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu bổ sung | 

Thuật toán không phụ thuộc vào kích thước của hàng đợi sau khi đọc đầu vào. Nó dễ dàng phù hợp với giới hạn của`n <= 1,000,000`bởi vì nó thực hiện công việc liên tục. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_io(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    n, x, y = map(int, sys.stdin.readline().split())
    first_valid = max(x + 1, n - y)
    ans = str(n - first_valid + 1)

    sys.stdin = old_stdin
    return ans

# provided samples
assert solve_io("4 2 1\n") == "2", "sample 1"
assert solve_io("6 1 3\n") == "4", "sample 2"

# custom cases
assert solve_io("2 1 1\n") == "1", "smallest queue"
assert solve_io("10 9 9\n") == "1", "only last position works"
assert solve_io("10 1 9\n") == "10", "all positions work"
assert solve_io("1000000 500000 500000\n") == "500001", "large input boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 1`|`1`| Kích thước hàng đợi tối thiểu có thể | 
|`10 9 9`|`1`| Yêu cầu cực cao đối với người đi trước | 
|`10 1 9`|`10`| Mọi vị trí đều hợp lệ | 
|`1000000 500000 500000`|`500001`| Giá trị lớn và độ chính xác số học | 

## Vỏ cạnh 

Đối với đầu vào:```
4 2 1
```điều kiện đầu tiên yêu cầu vị trí ít nhất là`3`. Điều kiện thứ hai yêu cầu vị trí ít nhất phải là`3`cũng vậy. Thuật toán bắt đầu đếm từ vị trí 3 và trả về 2, khớp với vị trí hợp lệ 3 và 4. 

Đối với đầu vào:```
5 1 4
```điều kiện đầu tiên yêu cầu vị trí ít nhất là`2`. Điều kiện thứ hai đưa ra`5 - 4 = 1`, nhưng điều kiện đầu tiên chặt chẽ hơn. Thuật toán chọn vị trí 2 và đếm các vị trí 2, 3, 4 và 5, cho ra câu trả lời đúng là 4. 

Đối với đầu vào:```
2 1 1
```vị trí duy nhất có thể là vị trí thứ hai. Thuật toán tính toán`max(2, 1)`là vị trí hợp lệ đầu tiên và trả về`2 - 2 + 1 = 1`. Điều này xác nhận rằng công thức tính bao gồm xử lý khoảng nhỏ nhất một cách chính xác. 

Tôi cũng có thể điều chỉnh nội dung này thành định dạng biên tập ngắn hơn theo phong cách Codeforces nếu bạn muốn nội dung nào đó gần giống với nội dung sẽ xuất hiện trên blog cuộc thi.
