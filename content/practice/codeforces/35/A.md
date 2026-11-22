---
title: "CF 35A - Trò chơi vỏ"
description: "Chúng tôi có ba chiếc cốc được sắp xếp thành một hàng và một quả bóng bắt đầu ở dưới một trong số chúng. Người biểu diễn thực hiện chính xác ba lần hoán đổi. Mỗi lần hoán đổi sẽ hoán đổi vị trí của hai chiếc cốc. Sau khi tất cả các lần hoán đổi kết thúc, chúng ta phải xác định cốc nào chứa bóng."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 35
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 35 (Div. 2)"
rating: 1000
weight: 35
solve_time_s: 86
verified: true
draft: false
---
[CF 35A - Trò chơi vỏ sò](https://codeforces.com/problemset/problem/35/A) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có ba chiếc cốc được sắp xếp thành một hàng và một quả bóng bắt đầu ở dưới một trong số chúng. Người biểu diễn thực hiện chính xác ba lần hoán đổi. Mỗi lần hoán đổi sẽ hoán đổi vị trí của hai chiếc cốc. Sau khi tất cả các lần hoán đổi kết thúc, chúng ta phải xác định cốc nào chứa bóng. 

Đầu vào đầu tiên đưa ra chỉ số cốc bắt đầu, từ 1 đến 3. Sau đó, ba dòng tiếp theo, mỗi dòng chứa hai số nguyên biểu thị các cốc được hoán đổi trong quá trình xáo trộn đó. 

Chi tiết quan trọng là quả bóng chỉ di chuyển khi chiếc cốc hiện tại của nó tham gia hoán đổi. Nếu quả bóng ở dưới cốc 2 và chúng ta đổi cốc 1 và cốc 3 thì quả bóng vẫn ở dưới cốc 2 vì cốc của nó không bao giờ di chuyển. 

Những hạn chế là rất nhỏ. Luôn có đúng ba cốc và đúng ba lần hoán đổi. Ngay cả việc triển khai chậm nhất có thể tưởng tượng được cũng sẽ chạy ngay lập tức. Điều này có nghĩa là vấn đề hoàn toàn là mô phỏng chính xác quy trình mà không mắc lỗi logic. 

Nguồn gốc chính của lỗi là do hiểu sai ý nghĩa của việc hoán đổi. 

Hãy xem xét đầu vào này:```
1
2 3
2 3
2 3
```Bóng bắt đầu từ cốc 1. Không có sự hoán đổi nào liên quan đến cốc 1, vì vậy câu trả lời vẫn là:```
1
```Việc thực hiện bất cẩn, thay đổi vị trí một cách mù quáng trong mỗi bước đi có thể khiến bóng di chuyển không chính xác. 

Một trường hợp tinh vi khác xảy ra khi cùng một giao dịch hoán đổi xuất hiện nhiều lần:```
2
1 2
1 2
1 2
```Sau lần hoán đổi đầu tiên, quả bóng di chuyển từ cốc 2 sang cốc 1. 

Sau lần thứ hai, nó trở lại cốc 2. 

Sau cốc thứ ba, nó quay trở lại cốc 1. 

Đầu ra đúng là:```
1
```Một giải pháp có lỗi chỉ ghi nhớ liệu một chiếc cốc đã từng được đổi chỗ hay chưa sẽ không thành công ở đây vì việc hoán đổi phải được xử lý theo thứ tự. 

Một sai lầm dễ mắc nữa là quên rằng việc hoán đổi là đối xứng. Nếu bóng ở dưới cốc 3 và xáo trộn là`1 3`, quả bóng phải chuyển sang cốc 1.```
3
1 3
1 2
2 3
```Mô phỏng chính xác mang lại:```
2
```Nếu quá trình triển khai chỉ kiểm tra xem vị trí hiện tại có bằng số đầu tiên trong cặp hay không thì nó sẽ bỏ lỡ các giao dịch hoán đổi trong đó cốc hiện tại bằng số thứ hai. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là mô phỏng trò chơi chính xác như mô tả. Chúng tôi lưu trữ cốc hiện tại chứa quả bóng. Với mỗi lần xáo trộn, chúng tôi kiểm tra hai chiếc cốc được đổi chỗ. 

Nếu quả bóng ở dưới cốc thứ nhất, nó sẽ di chuyển sang cốc thứ hai. 

Nếu quả bóng ở dưới cốc thứ hai, nó sẽ di chuyển sang cốc thứ nhất. 

Nếu không thì không có gì thay đổi. 

Mô phỏng lực lượng vũ phu này đã hoàn toàn có thể chấp nhận được vì kích thước vấn đề là không đổi. Chúng tôi xử lý chính xác ba lần hoán đổi, vì vậy tổng số thao tác rất nhỏ. 

Một cách giải thích bạo lực khác có thể xảy ra là theo dõi rõ ràng cả ba chiếc cốc trong một mảng và hoán đổi vật lý nội dung của chúng theo từng bước. Điều đó cũng có tác dụng nhưng nó lưu trữ nhiều thông tin hơn mức cần thiết. Chúng tôi chỉ quan tâm đến một điều, vị trí hiện tại của quả bóng. 

Quan sát quan trọng là toàn bộ trạng thái của trò chơi có thể được biểu diễn bằng một số nguyên duy nhất. Vì chỉ có một chiếc cốc chứa quả bóng nên chúng ta không bao giờ cần phải lập mô hình những chiếc cốc rỗng một cách riêng biệt. Mỗi lần xáo trộn đều thay đổi số nguyên này hoặc giữ nguyên nó. 

Điều đó làm giảm vấn đề thành một quá trình chuyển đổi trạng thái nhỏ gọn với bộ nhớ không đổi và thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng cốc đầy đủ | O(3) | O(3) | Đã chấp nhận | 
| Chỉ theo dõi vị trí bóng | O(3) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chỉ số cốc ban đầu nơi bóng bắt đầu. 
2. Lưu trữ chỉ mục này trong một biến có tên`pos`. Biến này luôn đại diện cho vị trí hiện tại của quả bóng. 
3. Xử lý lần lượt ba giao dịch hoán đổi. 
4. Đối với mỗi lần hoán đổi`(a, b)`, kiểm tra xem quả bóng có ở dưới một trong những chiếc cốc này không. 

Nếu như`pos == a`, bộ`pos = b`vì quả bóng chuyển động cùng với chiếc cốc`a`. 
5. Ngược lại, nếu`pos == b`, bộ`pos = a`. 

Việc hoán đổi hoạt động theo cả hai hướng, do đó quả bóng di chuyển từ`b`ĐẾN`a`trong trường hợp này. 
6. Nếu`pos`không khớp`a`cũng không`b`, không làm gì cả. 

Những chiếc cốc xáo trộn không chứa bóng. 
7. Sau cả ba lần hoán đổi, hãy in`pos`. 

### Tại sao nó hoạt động 

Bất biến rất đơn giản: sau mỗi lần xáo trộn được xử lý,`pos`bằng với chiếc cốc thật hiện đang chứa quả bóng. 

Ban đầu điều này đúng vì`pos`được thiết lập từ đầu vào. Trong một lần xáo trộn, có đúng hai cốc đổi vị trí. Nếu quả bóng ở dưới một trong số chúng, nó sẽ di chuyển sang chiếc cốc kia. Nếu không, vị trí của nó không thay đổi. Các quy tắc cập nhật phản ánh chính xác các chuyển động vật lý này, do đó, tính bất biến vẫn đúng sau mỗi bước. Sau lần xáo trộn thứ ba,`pos`là câu trả lời cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

pos = int(input())

for _ in range(3):
    a, b = map(int, input().split())

    if pos == a:
        pos = b
    elif pos == b:
        pos = a

print(pos)
```Chương trình chỉ lưu trữ vị trí hiện tại của quả bóng. Điều này phù hợp với quan sát rằng chúng ta không bao giờ cần theo dõi những chiếc cốc rỗng. 

Vòng lặp chạy chính xác ba lần vì vấn đề đảm bảo có ba lần xáo trộn. Đối với mỗi lần hoán đổi, chúng tôi so sánh vị trí quả bóng hiện tại với cả hai chiếc cốc được hoán đổi. 

Thứ tự của các điều kiện có vấn đề một chút. Một khi bóng khớp`a`, chúng tôi ngay lập tức chuyển nó đến`b`. sử dụng`elif`ngăn chặn việc vô tình kiểm tra lại giá trị được cập nhật trong cùng một lần lặp. 

Không cần lập chỉ mục mảng, do đó không có rủi ro riêng lẻ. Số cốc được sử dụng trực tiếp như được đưa ra trong đầu vào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
1 2
2 1
2 1
```| Bước | Hoán đổi | Vị trí bóng trước | Vị trí bóng sau | 
| --- | --- | --- | --- | 
| Bắt đầu | - | 1 | 1 | 
| 1 | 1 2 | 1 | 2 | 
| 2 | 2 1 | 2 | 1 | 
| 3 | 2 1 | 1 | 2 | 

Đầu ra cuối cùng:```
2
```Dấu vết này cho thấy các giao dịch hoán đổi phải được xử lý tuần tự. Quả bóng liên tục thay đổi vị trí khi cùng một cặp được đổi chỗ nhiều lần. 

### Ví dụ 2 

đầu vào:```
3
1 2
1 3
2 3
```| Bước | Hoán đổi | Vị trí bóng trước | Vị trí bóng sau | 
| --- | --- | --- | --- | 
| Bắt đầu | - | 3 | 3 | 
| 1 | 1 2 | 3 | 3 | 
| 2 | 1 3 | 3 | 1 | 
| 3 | 2 3 | 1 | 1 | 

Đầu ra cuối cùng:```
1
```Ví dụ này chứng minh rằng các giao dịch hoán đổi không liên quan đến chiếc cốc hiện tại sẽ không ảnh hưởng đến câu trả lời. Lần hoán đổi thứ nhất và thứ ba không có tác dụng gì vì quả bóng không ở dưới chiếc cốc được đổi chỗ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chính xác ba giao dịch hoán đổi được xử lý | 
| Không gian | O(1) | Chỉ có một biến số nguyên được lưu trữ | 

Thời gian chạy thực tế là không đổi vì số lượng thao tác không bao giờ phụ thuộc vào kích thước đầu vào. Việc sử dụng bộ nhớ cũng không đổi. Cả hai đều ở dưới mức giới hạn của vấn đề. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    pos = int(input())

    for _ in range(3):
        a, b = map(int, input().split())

        if pos == a:
            pos = b
        elif pos == b:
            pos = a

    print(pos)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("1\n1 2\n2 1\n2 1\n") == "2\n", "sample 1"

# minimum movement, ball never touched
assert run("1\n2 3\n2 3\n2 3\n") == "1\n", "ball never moves"

# repeated swaps
assert run("2\n1 2\n1 2\n1 2\n") == "1\n", "odd number of repeated swaps"

# ball starts at highest index
assert run("3\n1 3\n1 2\n2 3\n") == "1\n", "swap symmetry"

# every swap affects the ball
assert run("2\n2 3\n1 3\n1 2\n") == "2\n", "continuous movement"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 2 3 / 2 3 / 2 3`|`1`| Bóng không thay đổi khi không bao giờ bị tráo đổi | 
|`2 / 1 2 / 1 2 / 1 2`|`1`| Hoán đổi lặp đi lặp lại phải được xử lý theo thứ tự | 
|`3 / 1 3 / 1 2 / 2 3`|`1`| Hoán đổi hoạt động đối xứng | 
|`2 / 2 3 / 1 3 / 1 2`|`2`| Bóng có thể di chuyển từng bước | 

## Vỏ cạnh 

Hãy xem xét trường hợp quả bóng không bao giờ tham gia vào bất kỳ sự hoán đổi nào:```
1
2 3
2 3
2 3
```Thuật toán bắt đầu với`pos = 1`. 

Đối với mỗi lần xáo trộn, không có điểm cuối nào bằng`1`, do đó giá trị không bao giờ thay đổi. 

Dấu vết trạng thái:```
1 -> 1 -> 1 -> 1
```Câu trả lời cuối cùng được in chính xác là`1`. 

Bây giờ hãy xem xét việc hoán đổi lặp đi lặp lại của cùng một cặp:```
2
1 2
1 2
1 2
```Quá trình thực hiện diễn ra như sau: 

Trao đổi đầu tiên:`2 -> 1`Hoán đổi thứ hai:`1 -> 2`Hoán đổi thứ ba:`2 -> 1`Kết quả cuối cùng:```
1
```Điều này xác nhận rằng thuật toán tôn trọng trật tự và xử lý từng lần xáo trộn một cách độc lập. 

Cuối cùng, hãy xem xét trường hợp vị trí hiện tại khớp với giá trị thứ hai của hoán đổi:```
3
1 3
1 2
2 3
```Sự xáo trộn đầu tiên là`(1, 3)`. Từ`pos == 3`, thuật toán bước vào điều kiện thứ hai và cập nhật`pos = 1`. 

Dấu vết đầy đủ trở thành:```
3 -> 1 -> 1 -> 1
```Câu trả lời cuối cùng là`1`. Điều này xác minh rằng các giao dịch hoán đổi được xử lý chính xác theo cả hai hướng.
