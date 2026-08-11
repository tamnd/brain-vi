---
title: "CF 102397F - Trò Chơi Kỳ Lạ"
description: "Có n chiếc bánh nướng nhỏ trên bàn và Mahmoud thực hiện bước đi đầu tiên. Ở mỗi lượt, người chơi hiện tại có thể ăn chính xác một chiếc bánh cupcake. Nếu số lượng bánh nướng hiện tại là chẵn, người chơi cũng có tùy chọn ăn đúng một nửa trong số đó."
date: "2026-08-11T05:11:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102397
codeforces_index: "F"
codeforces_contest_name: "Asu Coding Cup 4"
rating: 0
weight: 102397
solve_time_s: 412
verified: true
draft: false
---

[CF 102397F - Trò chơi kỳ lạ](https://codeforces.com/problemset/problem/102397/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 52 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

có`n`những chiếc bánh nướng nhỏ trên bàn và Mahmoud thực hiện bước đầu tiên. Ở mỗi lượt, người chơi hiện tại có thể ăn chính xác một chiếc bánh cupcake. Nếu số lượng bánh nướng hiện tại là chẵn, người chơi cũng có tùy chọn ăn đúng một nửa trong số đó. Người chơi nào đối mặt đúng một chiếc bánh cupcake sẽ thua ngay lập tức, vì vậy trò chơi kết thúc khi một lượt bắt đầu với`1`. 

Nhiệm vụ là xác định người chơi nào thắng khi cả hai người chơi đều chọn nước đi tối ưu. Đầu vào chứa một số nguyên`n`, đại diện cho số lượng bánh nướng nhỏ ban đầu. Đầu ra là`Mahmoud`nếu người chơi đầu tiên có chiến lược chiến thắng và`Bashar`nếu không thì. 

Ràng buộc`n <= 10^9`là khó khăn chính. Một giải pháp kiểm tra mọi giá trị từ`1`bởi vì`n`sẽ yêu cầu tới một tỷ trạng thái, vượt xa giới hạn thời gian 1,5 giây có thể hỗ trợ. Thậm chí một`O(n)`chương trình năng động ở đây quá chậm. Chúng ta cần rút gọn trò chơi thành thuộc tính thời gian không đổi của`n`. 

Các trạng thái nhỏ nhất bộc lộ một số trường hợp nguy hiểm có thể đánh lừa việc triển khai trực tiếp. Vì`n = 1`, Mahmoud thua ngay lập tức vì vị trí bắt đầu đã chứa một chiếc bánh cupcake nên kết quả đúng là`Bashar`. Việc thực hiện bất cẩn khi cố gắng thực hiện một nước đi trước tiên sẽ coi đây là trạng thái trò chơi bình thường một cách không chính xác. 

Vì`n = 2`, Mahmoud có thể ăn một chiếc bánh cupcake và rời đi`1`, nên Bashar thua và câu trả lời là`Mahmoud`. Hoạt động một nửa cũng tồn tại ở đây, nhưng nó tạo ra cùng một trạng thái, điều đó có nghĩa là việc triển khai giả định hoạt động một nửa luôn khác biệt về mặt ý nghĩa có thể xử lý sai ranh giới này. 

Vì`n = 3`, Mahmoud chỉ có thể ăn một chiếc bánh cupcake, bỏ đi`2`. Bashar sau đó rời đi`1`, thế là Mahmoud thua. Đầu ra đúng là`Bashar`. Đây là vị thế thua lẻ nhỏ nhất sau`1`và rất hữu ích để nắm bắt các giải pháp chỉ kiểm tra xem`n`lớn hơn một. 

Vì`n = 4`, câu trả lời đúng là`Mahmoud`. Mahmoud có thể ăn một chiếc bánh cupcake và rời đi`3`. Bashar sau đó bị buộc phải rời đi`2`, và Mahmoud rời đi`1`. Điểm mấu chốt là Mahmoud không cần sử dụng nửa thao tác. Một giải pháp luôn được lựa chọn`n / 2`khi có thể có thể tạo ra người chiến thắng sai. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là phân loại từng số lượng bánh nướng nhỏ thành thắng hoặc thua. Một thế cờ sẽ bị thua nếu mỗi nước đi hợp lệ đều mang lại cho đối thủ một thế thắng. Sẽ thắng nếu có ít nhất một nước đi hợp lệ khiến đối phương rơi vào thế thua. Bắt đầu với`1`khi thua, chúng ta có thể tính toán câu trả lời cho`2, 3, ..., n`. 

Chương trình động này đúng vì mọi chuyển động từ`x`dẫn đến một số nhỏ hơn`x - 1`hoặc`x / 2`. Do đó, khi chúng ta xử lý`x`, kết quả của mọi điểm đến có thể đều đã được biết trước. Tuy nhiên, nó đòi hỏi phải kiểm tra tới`n`tiểu bang. Vì`n = 10^9`, điều đó có nghĩa là khoảng một tỷ đánh giá trạng thái và việc kiểm tra các chuyển đổi có sẵn có thể nâng công việc lên gần hai tỷ kiểm tra chuyển đổi. Yêu cầu về bộ nhớ cũng sẽ là`O(n)`nếu toàn bộ bảng được lưu trữ. 

Tính toán brute-force hoạt động vì đồ thị trò chơi luôn di chuyển về phía các số nhỏ hơn nhưng không thành công vì giới hạn trên quá lớn. Quan sát giúp tìm ra giải pháp nhanh hơn xuất phát từ việc xem xét tính chẵn lẻ của các vị trí. vị trí`1`đang thua. Mọi số lẻ lớn hơn`1`chỉ có một động thái có thể, từ`x`ĐẾN`x - 1`, tức là chẵn. Mọi số chẵn đều di chuyển tới`x - 1`, thật kỳ lạ. Nếu tất cả các vị trí lẻ đều thua và tất cả các vị trí chẵn đều thắng thì mối quan hệ này sẽ tự tái tạo với mọi số lượng lớn hơn. 

Do đó, một vị thế lẻ sẽ thua vì nước đi duy nhất của nó dẫn đến một vị thế thắng chẵn. Vị trí chẵn là thắng vì ăn một chiếc bánh cupcake sẽ dẫn đến vị trí thua lẻ. Thao tác một nửa không làm ảnh hưởng đến sự phân loại này: khi một số chẵn bị chia đôi, kết quả có thể là chẵn hoặc lẻ, nhưng vị trí chẵn đã có nước đi thắng.`n - 1`, nên nước đi thêm không thể làm nó thua. 

Do đó, toàn bộ trò chơi chỉ phụ thuộc vào việc số lượng bánh nướng ban đầu là số lẻ hay số chẵn. Nếu như`n`chẵn, Mahmoud thắng. Nếu như`n`thật kỳ lạ, Bashar thắng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DP | O(n) | O(n) | Quá chậm | 
| Quan sát chẵn lẻ tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng bánh cupcake ban đầu`n`. Thuộc tính duy nhất chúng ta cần sau khi rút ra mô hình trò chơi là tính chẵn lẻ của nó. 
2. Nếu`n`chẵn, đầu ra`Mahmoud`. Vị trí chẵn là thắng vì Mahmoud có thể ăn đúng một chiếc bánh cupcake và rời đi.`n - 1`, là số lẻ và do đó sẽ thua người chơi tiếp theo. 
3. Nếu`n`là số lẻ, đầu ra`Bashar`. Mahmoud chỉ có thể di chuyển từ một số lẻ`n`đến số chẵn`n - 1`, và vị trí chẵn sẽ thuộc về người chơi nhận được nó. 
4. Trường hợp đặc biệt`n = 1`đã được bao phủ bởi trường hợp kỳ lạ. Mahmoud bắt đầu với một chiếc bánh cupcake và thua cuộc, nên kết quả là`Bashar`. 

### Tại sao nó hoạt động 

Điều bất biến là mọi số lẻ bánh nướng nhỏ là một vị trí thua và mọi số chẵn là một vị trí thắng. Trường hợp cơ sở`1`đang thua. Đối với một điều kỳ lạ`n > 1`, động thái hợp pháp duy nhất là`n - 1`, tỷ số chẵn và do đó chiến thắng thuộc về đối thủ. Như vậy vị thế lẻ đang thua. Thậm chí`n`, ăn một chiếc bánh cupcake sẽ chuyển sang`n - 1`, là số lẻ nên thua đối thủ, nên thế chẵn là thắng. Điều này chứng tỏ sự phân loại cho mọi số nguyên dương, bao gồm phạm vi đầy đủ lên đến`10^9`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

if n % 2 == 0:
    print("Mahmoud")
else:
    print("Bashar")
```Đầu vào bao gồm một số nguyên duy nhất, vì vậy một lệnh gọi tới`input()`là đủ. Chúng tôi chuyển đổi nó thành một số nguyên và kiểm tra`n % 2`. 

Các bản in nhánh chẵn`Mahmoud`bởi vì Mahmoud luôn có thể loại bỏ một chiếc bánh cupcake và để lại một thế thua kỳ lạ. Những hình in nhánh lẻ`Bashar`, kể cả trường hợp biên`n = 1`. 

Không cần phải mô phỏng các bước di chuyển, xây dựng bảng lập trình động hoặc sử dụng đệ quy. Các số nguyên Python có thể biểu diễn giá trị đã cho một cách thoải mái, mặc dù ngay cả một số nguyên có dấu 32 bit có chiều rộng cố định cũng đủ để`10^9`. Phép toán modulo không có vấn đề riêng lẻ vì điều kiện thua chính xác là tính chẵn lẻ lẻ, bao gồm`1`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với mẫu đầu tiên,`n = 4`. 

| n | Người chơi hiện tại | Lý luận có sẵn | Kết quả n | Vị trí | 
| --- | --- | --- | --- | --- | 
| 4 | Mahmoud | Ăn 1 | 3 | Thua | 
| 3 | Bashar | Ăn 1 | 2 | Chiến thắng | 
| 2 | Mahmoud | Ăn 1 | 1 | Thua | 
| 1 | Bashar | Không di chuyển, thua | 1 | Nhà ga | 

Trình tự tối ưu khiến Bashar phải đối mặt với một chiếc bánh nướng nhỏ. Dấu vết thể hiện tính bất biến trung tâm: vị trí chẵn là thắng và vị trí lẻ là thua. 

Chương trình thấy rằng`4 % 2 == 0`và in ngay lập tức`Mahmoud`. 

### Mẫu 2 

Đối với mẫu thứ hai,`n = 3`. 

| n | Người chơi hiện tại | di chuyển có sẵn | Kết quả n | Vị trí | 
| --- | --- | --- | --- | --- | 
| 3 | Mahmoud | Ăn 1 | 2 | Chiến thắng | 
| 2 | Bashar | Ăn 1 | 1 | Thua | 
| 1 | Mahmoud | Không di chuyển, thua | 1 | Nhà ga | 

Mahmoud buộc phải nhường cho Bashar một vị thế ngang bằng. Bashar sau đó giao cho Mahmoud vị trí cuối cùng nên Mahmoud thua cuộc. 

Chương trình thấy rằng`3 % 2 == 1`và in`Bashar`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một thao tác modulo và một thao tác đầu ra được thực hiện | 
| Không gian | O(1) | Không có cấu trúc dữ liệu nào phát triển cùng với`n`| 

Đầu vào tối đa là`10^9`, nhưng thuật toán không bao giờ lặp lại giá trị đó. Nó thực hiện một số thao tác không đổi, do đó, nó vừa vặn thoải mái trong giới hạn thời gian 1,5 giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    n = int(input())

    if n % 2 == 0:
        print("Mahmoud")
    else:
        print("Bashar")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run("4\n") == "Mahmoud", "sample 1"
assert run("3\n") == "Bashar", "sample 2"

# Minimum-size input
assert run("1\n") == "Bashar", "n = 1 is immediately losing"

# Smallest even input
assert run("2\n") == "Mahmoud", "n = 2 is winning"

# Odd boundary
assert run("999999999\n") == "Bashar", "large odd input"

# Maximum-size input
assert run("1000000000\n") == "Mahmoud", "maximum even input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`Bashar`| Đầu vào tối thiểu và trạng thái mất ngay lập tức | 
|`2`|`Mahmoud`| Bang chiến thắng nhỏ nhất và ranh giới chẵn | 
|`999999999`|`Bashar`| Giá trị lẻ lớn không cần mô phỏng | 
|`1000000000`|`Mahmoud`| Ràng buộc tối đa và giá trị chẵn lớn | 

## Vỏ cạnh 

cho`n = 1`, thuật toán lấy nhánh lẻ và in ra`Bashar`. Không có động thái nào được thực hiện, điều này mô phỏng chính xác quy tắc rằng người chơi có lượt bắt đầu bằng một chiếc bánh cupcake sẽ thua. 

Vì`n = 2`, thuật toán lấy nhánh chẵn và in`Mahmoud`. Mahmoud có thể ăn một chiếc bánh cupcake, tạo ra`1`cho Bashar. Điều này cũng chứng tỏ tại sao giải pháp không được coi hoạt động một nửa là bắt buộc. Dù Mahmoud cân nhắc việc ăn một hay một nửa thì vị trí đó vẫn thuộc về anh ta. 

Vì`n = 3`, thuật toán lấy nhánh lẻ và in ra`Bashar`. Mahmoud chỉ có thể di chuyển đến`2`, chiến thắng thuộc về Bashar. Bashar sau đó di chuyển đến`1`, khiến Mahmoud thua ở vị trí cuối cùng. 

Vì`n = 4`, thuật toán lấy nhánh chẵn và in`Mahmoud`. Nước đi thắng lợi là`4 -> 3`. Từ`3`đang thua, Mahmoud đã thiết lập chiến lược thắng. Nửa bước di chuyển`4 -> 2`là không liên quan vì thế thắng chỉ cần một nước đi thắng. 

Để có đầu vào tối đa`n = 10^9`, số đó là số chẵn nên chương trình in ra`Mahmoud`sau một lần kiểm tra tính chẵn lẻ. Không có vòng lặp nào phụ thuộc vào độ lớn của`n`, đó chính xác là lý do tại sao giải pháp duy trì thời gian không đổi ở đầu vào lớn nhất được phép.
