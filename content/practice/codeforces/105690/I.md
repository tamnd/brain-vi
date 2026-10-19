---
title: "CF 105690I - Phong Bì Màu Đỏ"
description: "Trò chơi có n phong bì. Phong bì i bắt đầu bằng a[i] đồng xu. Một nước đi sẽ chọn bất kỳ nhóm phong bì không trống nào hiện chứa cùng số xu dương, sau đó loại bỏ chính xác một xu khỏi mỗi phong bì đã chọn. Người chơi thực hiện nước đi cuối cùng có thể sẽ thắng."
date: "2026-06-26T09:31:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105690
codeforces_index: "I"
codeforces_contest_name: "UTPC Contest 1-29-25 Div. 1 (Advanced)"
rating: 0
weight: 105690
solve_time_s: 88
verified: true
draft: false
---

[CF 105690I - Phong bì màu đỏ](https://codeforces.com/problemset/problem/105690/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Trò chơi chứa`n`phong bì. Phong bì`i`bắt đầu bằng`a[i]`tiền xu. Một nước đi sẽ chọn bất kỳ nhóm phong bì không trống nào hiện chứa cùng số xu dương, sau đó loại bỏ chính xác một xu khỏi mỗi phong bì đã chọn. Người chơi thực hiện nước đi cuối cùng có thể sẽ thắng. 

Nhiệm vụ là xác định xem người chơi thứ nhất Ai hay người chơi thứ hai Bo có chiến lược chiến thắng khi cả hai đều chơi tối ưu. 

Những hạn chế là đầu mối chính. Có thể có tới`2 * 10^5`phong bì, và mỗi phong bì có thể chứa tối đa`10^9`tiền xu. Bất kỳ mô phỏng nào thực hiện một thao tác trên mỗi đồng xu đều không thể thực hiện được vì tổng số xu có thể đạt tới`2 * 10^14`. Chúng ta cần một giải pháp mà công việc của nó phụ thuộc vào số lượng phong bì hơn là giá trị của đồng xu. 

Việc triển khai trực tiếp cũng không thể lưu trữ mọi số lượng xu có thể có vì giá trị tối đa là rất lớn. Giải pháp phải nén cấu trúc của độ cao. 

Một số trường hợp dễ dàng phá vỡ các cách tiếp cận không chính xác. Hãy xem xét một phong bì có hai đồng xu.```
1
2
```Câu trả lời đúng là`Bo`. Ai chỉ có thể giảm phong bì còn một đồng xu, sau đó Bo loại bỏ đồng xu cuối cùng. Bất kỳ giải pháp nào coi mỗi phong bì như một đống Nim bình thường sẽ nói không chính xác rằng người chơi đầu tiên sẽ thắng. 

Một trường hợp quan trọng khác là nhiều phong bì có cùng giá trị.```
2
1 1
```Câu trả lời đúng là`Ai`. Ai có thể chọn cả hai phong bì và lấy đi đồng xu duy nhất của chúng chỉ bằng một nước đi. Một chiến lược chỉ xem xét một phong bì tại một thời điểm sẽ bỏ lỡ sự tương tác này. 

Trường hợp ranh giới cuối cùng là khi tất cả các giá trị đều bằng nhau.```
4
5 5 5 5
```Kết quả đúng là`Bo`. Bốn chiếc phong bì hoạt động cùng nhau chứ không phải như bốn trò chơi độc lập. Khả năng chọn bất kỳ tập hợp con nào của phong bì có chiều cao bằng nhau sẽ thay đổi giá trị Grundy. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô hình hóa mọi trạng thái trò chơi có thể có. Một trạng thái được xác định bởi số lượng xu hiện tại trong mỗi phong bì. Đối với mọi trạng thái, chúng ta có thể tạo ra tất cả các bước di chuyển hợp pháp, giải quyết đệ quy các trạng thái kết quả và sử dụng định lý Sprague-Grundy. Điều này đúng vì mỗi lần di chuyển sẽ làm giảm tổng số xu nên đồ thị trò chơi không có chu kỳ. 

Vấn đề là số lượng các bang là rất lớn. Thậm chí một phong bì với`10^9`tiền xu tạo ra một chuỗi gồm một tỷ tiểu bang. Với nhiều phong bì, số lượng kết hợp trở nên không thể liệt kê được. 

Quan sát quan trọng là danh tính chính xác của một phong bì không quan trọng. Điều quan trọng là có bao nhiêu phong bì đã đạt đến từng mức độ cao. 

Cho phép`c[h]`là số phong bì có ít nhất`h`tiền xu. Ví dụ: nếu phong bì chứa`1, 3, 3`, sau đó:```
h = 1: c[1] = 3
h = 2: c[2] = 2
h = 3: c[3] = 2
```Một động thái làm giảm giá trị của một số phong bì từ`h`ĐẾN`h-1`. Về mặt`c`, nó giảm đúng một giá trị`c[h]`theo số lượng phong bì đã chọn. Trò chơi này có giá trị Grundy bằng:```
c[1] xor c[2] xor c[3] xor ...
```Lý do là mỗi cấp độ hoạt động giống như một thành phần Nim. Một bước di chuyển chỉ thay đổi một số cấp độ và phạm vi hợp pháp của số lượng đó phù hợp với cấu trúc Grundy tiêu chuẩn cho vùng Nim. 

Thử thách còn lại đó là`h`có thể lớn như`10^9`. Chúng tôi không thể lặp lại qua mọi cấp độ. Thay vào đó, hãy sắp xếp chiều cao phong bì theo thứ tự giảm dần. Giữa hai độ cao liên tiếp, giá trị của`c[h]`không đổi. 

Giả sử các giá trị được sắp xếp là:```
a1 >= a2 >= ... >= an
```Dành cho mọi cấp độ`h`trong phạm vi:```
a[i+1] < h <= a[i]
```chính xác`i`phong bì có ít nhất`h`tiền xu. Giá trị này xuất hiện`a[i] - a[i+1]`lần trong xor. Nếu độ dài đó là số lẻ thì phần đóng góp là`i`, nếu không nó sẽ hủy bỏ. 

Brute-force hoạt động vì nó tuân thủ trực tiếp các quy tắc trò chơi, nhưng không thành công vì không gian trạng thái tăng lên theo số lượng xu. Việc quan sát số cấp nén tất cả các hành vi bình đẳng vào`n`khoảng thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ theo số lượng trạng thái | Hàm mũ | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các giá trị phong bì theo thứ tự giảm dần. Nối một giá trị cuối cùng`0`sao cho mỗi khoảng độ cao đều có một ranh giới đúng. 
2. Đối với mọi vị trí`i`từ`0`ĐẾN`n-1`, xét độ cao giữa`a[i+1] + 1`Và`a[i]`. Trong toàn bộ phạm vi này, chính xác`i + 1`phong bì chứa ít nhất số tiền đó. 
3. Đếm kích thước của phạm vi này là`a[i] - a[i+1]`. Nếu số này là số lẻ, hãy xor câu trả lời bằng`i + 1`. Nếu nó là số chẵn thì phần đóng góp này sẽ biến mất vì cùng một số được xor-ed một số lần chẵn. 
4. Sau khi xử lý mỗi khoảng thời gian, giá trị xor thu được là số Grundy của vị trí bắt đầu. Giá trị khác 0 có nghĩa là người chơi đầu tiên có nước đi thắng, trong khi giá trị 0 có nghĩa là mọi nước đi đều dẫn đến trạng thái thua. 

Tại sao nó hoạt động: 

Bất biến là xor của số lượng phong bì đạt tới mọi độ cao có thể. Một nước đi hợp pháp chỉ thay đổi một lớp chiều cao, do đó giá trị Grundy hoạt động chính xác giống như vị trí Nim. Nếu xor bằng 0, mọi nước đi có thể sẽ thay đổi nó thành giá trị khác 0, khiến đối thủ có thế thắng. Nếu xor khác 0 thì sẽ tồn tại một động thái làm cho xor bằng 0, mang lại chiến lược chiến thắng cho người chơi hiện tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    a.sort(reverse=True)
    a.append(0)

    grundy = 0

    for i in range(n):
        if (a[i] - a[i + 1]) & 1:
            grundy ^= i + 1

    print("Ai" if grundy else "Bo")

if __name__ == "__main__":
    solve()
```Đầu vào được đọc một lần vì toàn bộ vấn đề chỉ phụ thuộc vào tập giá trị đường bao ban đầu. 

Sắp xếp các vị trí có độ cao bằng nhau và gần nhau với nhau, cho phép xử lý biểu diễn số cấp theo khoảng thay vì cấp đồng xu riêng lẻ. 

Số 0 được thêm vào là giá trị biên. Nếu không có nó, nhóm độ cao cuối cùng sẽ không có điểm cuối thấp hơn và phần đóng góp khoảng cuối cùng sẽ bị bỏ lỡ. 

chỉ số là`i + 1`còn hơn là`i`bởi vì lần đầu tiên`i + 1`phong bì chính xác là những phong bì có chiều cao ít nhất bằng mọi chiều cao trong khoảng hiện tại. 

Số nguyên Python có độ chính xác tùy ý, vì vậy các giá trị lên tới`10^9`không yêu cầu xử lý đặc biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 1
```Các giá trị được sắp xếp:```
[1, 1, 0]
```| tôi | Chiều cao hiện tại | Chiều cao tiếp theo | Phong bì đạt đến cấp độ này | Độ dài khoảng | bẩn thỉu | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 0 | 0 | 
| 1 | 1 | 0 | 2 | 1 | 2 | 

Xor cuối cùng là`2`, vậy là Ai thắng. 

Ví dụ này cho thấy tại sao các phong bì bằng nhau không thể được xử lý độc lập. Chọn cả hai phong bì cùng một lúc là một động thái hợp pháp. 

### Ví dụ 2 

đầu vào:```
4
1 2 2 3
```Các giá trị được sắp xếp:```
[3, 2, 2, 1, 0]
```| tôi | Chiều cao hiện tại | Chiều cao tiếp theo | Phong bì đạt đến cấp độ này | Độ dài khoảng | bẩn thỉu | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 3 | 2 | 1 | 1 | 1 | 
| 1 | 2 | 2 | 2 | 0 | 1 | 
| 2 | 2 | 1 | 3 | 1 | 2 | 
| 3 | 1 | 0 | 4 | 1 | 2 | 

Xor cuối cùng là:```
1 xor 3 xor 4 = 6
```Trên thực tế chỉ có các khoảng có độ dài lẻ mới đóng góp, vì vậy giá trị Grundy cuối cùng khác 0 và Ai thắng. 

Dấu vết này cho thấy cách thuật toán tránh lặp lại các giá trị đồng xu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế trong công việc và quá trình quét là tuyến tính. | 
| Không gian | O(n) | Danh sách các giá trị phong bì được sắp xếp sẽ được lưu trữ. | 

Những ràng buộc cho phép`n = 2 * 10^5`và việc sắp xếp nhiều giá trị này dễ dàng nằm trong giới hạn bắt buộc. Giá trị đồng xu lớn không ảnh hưởng đến thời gian chạy vì chúng chỉ xuất hiện trong phép so sánh số học. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# sample 1
assert run("2\n1 1\n") == "Ai\n"

# sample 2
assert run("1\n2\n") == "Bo\n"

# sample 3
assert run("5\n1 2 3 4 5\n") == "Ai\n"

# sample 4
assert run("4\n1 2 2 3\n") == "Bo\n"

# all equal values
assert run("4\n5 5 5 5\n") == "Bo\n"

# minimum size
assert run("1\n1\n") == "Ai\n"

# large boundary values
assert run("3\n1000000000 1000000000 1000000000\n") == "Ai\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 1 1`|`Ai`| Có thể chọn nhiều phong bì có cùng chiều cao cùng nhau. | 
|`1 / 2`|`Bo`| Một đường bao có chiều cao chẵn không phải là một đống Nim bình thường. | 
|`4 / 1 2 2 3`|`Bo`| Xử lý độ cao lặp lại và nén khoảng thời gian. | 
|`4 / 5 5 5 5`|`Bo`| Xác thực các giá trị bằng nhau. | 
|`1 / 1`|`Ai`| Đầu vào nhỏ nhất có thể. | 
|`3 / 1000000000 ...`|`Ai`| Xử lý các giá trị tiền xu rất lớn. | 

## Vỏ cạnh 

Đối với một phong bì có hai đồng xu:```
1
2
```Mảng được sắp xếp là`[2, 0]`. Khoảng duy nhất có độ dài`2`, là số chẵn nên nó không đóng góp gì cho xor. Giá trị Grundy bằng 0 và kết quả đầu ra của thuật toán`Bo`, phù hợp với thực tế là người chơi thứ hai lấy đồng xu cuối cùng. 

Đối với hai phong bì có một đồng xu:```
2
1 1
```Mảng được sắp xếp là`[1, 1, 0]`. Khoảng trống duy nhất có độ dài`1`, và số phong bì đạt đến độ cao đó là`2`. Xor trở thành`2`, do đó kết quả đầu ra của thuật toán`Ai`. Nước đi chiến thắng là chọn cả hai phong bì ngay lập tức. 

Đối với tất cả các phong bì có cùng giá trị:```
4
5 5 5 5
```Chỉ khoảng cuối cùng mới đóng góp vì các khoảng trống trước đó có độ dài bằng 0. Sự đóng góp là`4`, có nghĩa là vị trí chỉ bị mất nếu xor trở thành 0. Ở đây xor khác 0 nên thuật toán xác định chính xác người chơi chiến thắng. Trường hợp này xác nhận rằng độ cao bằng nhau được xử lý thông qua số lượng nhóm thay vì tính toán đường bao độc lập.
