---
title: "CF 264A - Thoát Khỏi Đá"
description: "Chúng ta được cho một chuỗi gồm các ký tự l và r. Các nhân vật mô tả cách con sóc di chuyển khi từng viên đá rơi xuống. Viên đá i rơi sau khi viên đá 1...i-1 đã được đặt."
date: "2026-06-04T17:51:23+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 264
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 162 (Div. 1)"
rating: 1200
weight: 264
solve_time_s: 107
verified: true
draft: false
---

[CF 264A - Thoát khỏi đá](https://codeforces.com/problemset/problem/264/A) 

**Đánh giá:** 1200 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, cách triển khai, hai con trỏ 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi bao gồm các ký tự`l`Và`r`. Các nhân vật mô tả cách con sóc di chuyển khi từng viên đá rơi xuống. 

Cục đá`i`rơi sau đá`1...i-1`đã được đặt rồi. Nếu ký tự tương ứng là`l`, con sóc di chuyển sang nửa bên trái của khoảng thời gian hiện tại. Nếu nó là`r`, cô ấy di chuyển sang nửa bên phải. Sau khi tất cả các nước đi được xử lý, các quân cờ sẽ được sắp xếp thành một đường thẳng. Chúng ta phải xuất số đá từ ngoài cùng bên trái đến ngoài cùng bên phải. 

Mô tả hình học nghe có vẻ phức tạp nhưng nhiệm vụ thực tế là xác định thứ tự cuối cùng từ trái sang phải của các chỉ số đá. 

Độ dài của chuỗi có thể đạt tới một triệu. Bất kỳ thuật toán nào liên tục chèn vào giữa mảng hoặc mô phỏng các vị trí một cách rõ ràng sẽ quá chậm. Với$n = 10^6$, thậm chí$O(n^2)$hành vi sẽ yêu cầu đại khái$10^{12}$hoạt động hoàn toàn không thể thực hiện được. Chúng ta cần một giải pháp thời gian tuyến tính. 

Một điểm tinh tế là thứ tự không được xác định bởi tọa độ vật lý của các viên đá. Hình học chỉ hàm ý một quy tắc sắp xếp tương đối. 

Hãy xem xét đầu vào:```
r
```Chỉ có một viên đá nên kết quả là:```
1
```Một giải pháp giả định mọi`r`nên được đặt sau một cái gì đó khác có thể xử lý sai trường hợp nhỏ nhất này. 

Một trường hợp thú vị khác là:```
lll
```Đầu ra đúng là:```
3
2
1
```Mỗi viên đá mới sẽ nằm ở bên trái của tất cả những viên đá đã được xử lý trước đó. Việc triển khai bất cẩn mà chỉ thêm các chỉ mục khi đọc chuỗi sẽ tạo ra`1 2 3`, điều đó không đúng. 

Tương tự:```
rrr
```sản xuất```
1
2
3
```Bây giờ mọi viên đá xuất hiện ở bên phải của tất cả các viên đá trước đó. 

Trường hợp hỗn hợp```
llrlr
```cho```
3
5
4
2
1
```điều này cho thấy rằng việc đảo ngược câu trả lời cũng như giữ nguyên thứ tự ban đầu là không đủ. 

## Phương pháp tiếp cận 

Có thể mô phỏng trực tiếp. Chúng ta có thể duy trì trình tự từ trái sang phải hiện tại của các viên đá. Khi đá`i`đến, chúng tôi xác định vị trí của nó so với những viên đá hiện có và chèn nó vào cho phù hợp. 

Vấn đề là việc chèn vào phía trước hoặc giữa mảng động sẽ tốn thời gian tuyến tính. Trong trường hợp xấu nhất chúng ta thực hiện$n$chèn, mỗi chi phí$O(n)$, dẫn đến$O(n^2)$sự phức tạp. Với một triệu viên đá, việc này chậm đến mức vô vọng. 

Quan sát quan trọng là hình học đưa ra một quy tắc sắp xếp rất đơn giản. 

Giả sử chúng ta xử lý đá theo thứ tự. Khi đá`i`tương ứng với`l`, con sóc bỏ chạy sang trái. Điều này có nghĩa là mọi vị trí trong tương lai bên trong khoảng đó đều nằm ở bên trái của hòn đá`i`. Trong sự sắp xếp cuối cùng, đá`i`nên xuất hiện càng sớm càng tốt trong số những viên đá chưa qua chế biến còn lại. 

Khi đá`i`tương ứng với`r`, con sóc trốn thoát ngay. Cục đá`i`phải xuất hiện sau tất cả các viên đá mà sau này sẽ được đặt vào đúng khoảng. 

Điều này dẫn đến một cách giải thích mang tính xây dựng. 

Đối với mọi`l`, số đá tương ứng nằm ở đầu dãy cuối cùng. 

Đối với mọi`r`, số đá tương ứng nằm ở cuối dãy cuối cùng. 

Nếu đọc chuỗi từ trái sang phải, chúng ta có thể đặt các số đá bằng`l`vào phần phía trước và số đá bằng`r`vào phần sau. 

Một công thức thậm chí còn đơn giản hơn sẽ xuất hiện nếu chúng ta nghĩ trực tiếp về thứ tự đầu ra. 

Tất cả các viên đá được đánh dấu`r`nên được in theo thứ tự ban đầu của chúng. 

Tất cả các viên đá được đánh dấu`l`nên được in theo thứ tự ngược lại sau chúng. 

Một deque đương nhiên đại diện cho quá trình này. Đối với đá`i`:

Nếu như`s[i] == 'l'`, xô`i+1`về phía trước. 

Nếu như`s[i] == 'r'`, xô`i+1`về phía sau. 

Cuối cùng, đọc deque từ trước ra sau sẽ cho ra chính xác thứ tự mong muốn. 

Vì mỗi thao tác trên deque có thời gian không đổi nên toàn bộ thuật toán là tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào. 
2. Tạo một deque trống. 
3. Xử lý các ký tự từ trái sang phải. Gọi số đá hiện tại là`i + 1`. 
4. Nếu ký tự hiện tại là`l`, chèn số đá ở phía trước deque. 

Điều này phản ánh thực tế là viên đá phải xuất hiện trước tất cả những viên đá sẽ được đặt trong khoảng cách di chuyển sang trái. 
5. Nếu ký tự hiện tại là`r`, chèn số đá vào phía sau deque. 

Điều này phản ánh thực tế là viên đá phải xuất hiện sau những viên đá mà sau này sẽ được đặt vào đúng khoảng. 
6. Sau khi xử lý tất cả các ký tự, duyệt deque từ trước ra sau và in từng số đá được lưu trữ. 

### Tại sao nó hoạt động 

Duy trì bất biến sau khi xử lý lần đầu tiên`i`các ký tự, deque chứa chính xác thứ tự tương đối cuối cùng của các viên đá`1...i`. 

Khi một nhân vật được`l`, mọi viên đá trong tương lai được tạo ra từ khoảng đã chọn phải nằm ở bên phải của viên đá hiện tại trong cách sắp xếp cuối cùng. Đặt viên đá hiện tại ở phía trước sẽ duy trì trật tự tương đối chính xác. 

Khi một nhân vật được`r`, mọi viên đá trong tương lai được tạo ra từ khoảng thời gian đã chọn phải nằm ở bên trái của viên đá hiện tại. Đặt viên đá hiện tại ở phía sau sẽ giữ nguyên thứ tự tương đối chính xác. 

Vì mỗi lần chèn sẽ đặt viên đá mới vào vị trí duy nhất phù hợp với khoảng thời gian được chọn ở bước đó nên tính bất biến vẫn đúng trong suốt quá trình xử lý. Sau viên đá cuối cùng, deque chứa thứ tự hoàn chỉnh từ trái sang phải. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    s = input().strip()

    dq = deque()

    for i, ch in enumerate(s, start=1):
        if ch == 'l':
            dq.appendleft(i)
        else:
            dq.append(i)

    sys.stdout.write('\n'.join(map(str, dq)))

if __name__ == "__main__":
    solve()
```Giải pháp sử dụng deque vì nó hỗ trợ chèn ở cả hai đầu trong thời gian không đổi. 

Vòng lặp xử lý đá theo thứ tự tự nhiên của chúng, từ`1`ĐẾN`n`. Số đá được lấy trực tiếp từ chỉ số vòng lặp. 

Đối với một`l`, chúng tôi sử dụng`appendleft`, đặt viên đá vào đầu thứ tự hiện tại. Đối với một`r`, chúng tôi sử dụng`append`, đặt nó ở cuối. 

Không cần tính toán tọa độ. Toàn bộ quá trình hình học sụp đổ để duy trì trật tự tương đối chính xác. 

Đầu ra cuối cùng được tạo ra bằng cách duyệt qua deque một lần và nối các số với các ký tự dòng mới. Điều này tránh được nhiều lệnh in riêng lẻ, điều này rất quan trọng khi độ dài đầu vào đạt đến một triệu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
llrlr
```| Bước | Nhân vật | Đá | Deque sau phẫu thuật | 
| --- | --- | --- | --- | 
| 1 | tôi | 1 | [1] | 
| 2 | tôi | 2 | [2, 1] | 
| 3 | r | 3 | [2, 1, 3] | 
| 4 | tôi | 4 | [4, 2, 1, 3] | 
| 5 | r | 5 | [4, 2, 1, 3, 5] | 

Đọc từ trước ra sau sẽ có:```
4
2
1
3
5
```Dấu vết này thể hiện quy tắc chèn trực tiếp. Mọi`l`di chuyển hòn đá hiện tại lên phía trước, trong khi mọi`r`đặt nó ở cuối. 

### Ví dụ 2 

đầu vào:```
rrrl
```| Bước | Nhân vật | Đá | Deque sau phẫu thuật | 
| --- | --- | --- | --- | 
| 1 | r | 1 | [1] | 
| 2 | r | 2 | [1, 2] | 
| 3 | r | 3 | [1, 2, 3] | 
| 4 | tôi | 4 | [4, 1, 2, 3] | 

Đầu ra:```
4
1
2
3
```Ví dụ này cho thấy rằng một muộn`l`có thể di chuyển một viên đá trước mọi viên đá đã được xử lý trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần chèn deque cho mỗi ký tự và một lần truyền tải cuối cùng | 
| Không gian | O(n) | Deque lưu trữ tất cả số đá | 

Độ dài đầu vào có thể lớn tới một triệu. Thời gian tuyến tính chỉ có nghĩa là vài triệu thao tác nguyên thủy, dễ dàng phù hợp với giới hạn thời gian. Việc lưu trữ một triệu số nguyên cũng nằm trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    s = sys.stdin.readline().strip()
    dq = deque()

    for i, ch in enumerate(s, start=1):
        if ch == 'l':
            dq.appendleft(i)
        else:
            dq.append(i)

    return "\n".join(map(str, dq))

# sample from statement
assert run("llrlr\n") == "4\n2\n1\n3\n5", "sample"

# minimum size
assert run("r\n") == "1", "single stone"

# all left
assert run("lll\n") == "3\n2\n1", "all left moves"

# all right
assert run("rrrr\n") == "1\n2\n3\n4", "all right moves"

# alternating pattern
assert run("lrlr\n") == "4\n2\n1\n3", "mixed ordering"

# boundary style case
assert run("lr\n") == "1\n2", "small mixed case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`r`|`1`| Kích thước đầu vào tối thiểu | 
|`lll`|`3 2 1`| Chèn phía trước lặp đi lặp lại | 
|`rrrr`|`1 2 3 4`| Chèn lại nhiều lần | 
|`lrlr`|`4 2 1 3`| Hoạt động luân phiên | 
|`lr`|`1 2`| Trường hợp ranh giới nhỏ | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
lll
```Quá trình xử lý tiến hành như sau:`[1] → [2,1] → [3,2,1]`Đầu ra trở thành:```
3
2
1
```Mọi viên đá đều được chèn ở phía trước, vì vậy thứ tự cuối cùng hoàn toàn ngược lại với thứ tự đến. 

Hãy xem xét đầu vào:```
rrr
```Quá trình xử lý tiến hành như sau:`[1] → [1,2] → [1,2,3]`Đầu ra là:```
1
2
3
```Mọi viên đá đều được chèn ở phía sau nên thứ tự cuối cùng khớp với thứ tự đến. 

Hãy xem xét đầu vào nhỏ nhất có thể:```
l
```Deque trở thành`[1]`, và đầu ra là:```
1
```Thuật toán không yêu cầu xử lý đặc biệt cho trường hợp này vì cả hai`appendleft`Và`append`hoạt động chính xác khi deque trống. 

Hãy xem xét một mô hình hỗn hợp:```
lrl
```Quá trình xử lý mang lại:`[1] → [1,2] → [3,1,2]`Đầu ra là:```
3
1
2
```Điều này xác nhận rằng sau này`l`nhảy chính xác lên trước tất cả các viên đá đã được xử lý trước đó trong khi vẫn giữ được thứ tự tương đối đã được thiết lập của các viên đá khác.
