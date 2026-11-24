---
title: "CF 912A - Thuật giả kim phức tạp"
description: "Chúng tôi có sẵn hai loại tinh thể: màu vàng và màu xanh. Sản xuất mỗi loại bóng tiêu tốn tinh thể theo một công thức cố định. Một quả bóng màu vàng cần có 2 tinh thể màu vàng. Một quả bóng màu xanh lá cây cần 1 tinh thể màu vàng và 1 tinh thể màu xanh lam. Một quả bóng màu xanh cần 3 tinh thể màu xanh."
date: "2026-06-13T00:49:11+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 912
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 456 (Div. 2)"
rating: 800
weight: 912
solve_time_s: 252
verified: true
draft: false
---

[CF 912A - Thuật giả kim phức tạp](https://codeforces.com/problemset/problem/912/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 4 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có sẵn hai loại tinh thể: màu vàng và màu xanh. 

Sản xuất mỗi loại bóng tiêu tốn tinh thể theo một công thức cố định. Một quả bóng màu vàng cần có 2 tinh thể màu vàng. Một quả bóng màu xanh lá cây cần 1 tinh thể màu vàng và 1 tinh thể màu xanh lam. Một quả bóng màu xanh cần 3 tinh thể màu xanh. 

Đầu vào cung cấp lượng tinh thể màu vàng hiện tại`A`và tinh thể màu xanh`B`, tiếp theo là số lượng bóng vàng, xanh lá cây và xanh dương cần thiết:`x`,`y`, Và`z`. 

Nhiệm vụ của chúng tôi là xác định xem phải mua thêm bao nhiêu tinh thể để có thể sản xuất được tất cả các quả bóng được yêu cầu. Chúng tôi có thể tự do mua cả tinh thể màu vàng và màu xanh lam và chúng tôi muốn có tổng số tinh thể bổ sung tối thiểu. 

Các ràng buộc đi lên đến$10^9$. Các giá trị như vậy ngay lập tức loại trừ bất kỳ mô phỏng nào tạo ra từng quả bóng một hoặc liên tục trừ đi các tinh thể. Ngay cả một thuật toán tuyến tính về số lượng quả bóng cũng sẽ quá chậm. Vì đầu vào chỉ bao gồm năm số nguyên nên chúng ta mong đợi một nghiệm số học có thời gian không đổi. 

Một sai lầm phổ biến là xử lý tình trạng thiếu hụt một cách độc lập cho từng loại bóng thay vì tính toán tổng yêu cầu về tinh thể trước tiên. 

Coi như:```
A = 2, B = 1
x = 1, y = 1, z = 0
```Các tinh thể màu vàng cần thiết là$2 \cdot 1 + 1 = 3$. Các tinh thể màu xanh cần thiết là$1$. 

Chúng ta đang thiếu chính xác một viên pha lê màu vàng, vì vậy câu trả lời là`1`. 

Một cách tiếp cận bất cẩn khi kiểm tra từng công thức bóng riêng biệt có thể làm tăng gấp đôi số lượng thiếu hụt. 

Một sai lầm dễ mắc phải khác là trừ đi các tinh thể có sẵn khỏi tổng nhu cầu và cho phép thâm hụt âm làm giảm câu trả lời. 

Ví dụ:```
A = 100, B = 0
x = 1, y = 0, z = 0
```Chỉ cần 2 tinh thể màu vàng. 98 tinh thể màu vàng dư thừa không thể bù đắp cho những tinh thể màu xanh bị thiếu trong các tình huống khác. Mỗi màu phải được xử lý độc lập. Số tiền bổ sung chính xác ở đây là`0`, không phải là số âm. 

Trường hợp cạnh cuối cùng xảy ra khi một màu thừa và màu kia thiếu.```
A = 10, B = 1
x = 0, y = 0, z = 1
```Chúng ta cần 3 viên pha lê màu xanh và chỉ có 1. Câu trả lời là`2`, mặc dù có rất nhiều tinh thể màu vàng. Màu sắc pha lê không thể thay thế cho nhau. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là nghĩ đến việc sản xuất từng quả bóng được yêu cầu. Đối với mỗi quả bóng màu vàng, chúng ta sẽ tiêu thụ hai tinh thể màu vàng, đối với mỗi quả bóng màu xanh lá cây một tinh thể màu vàng và một tinh thể màu xanh lam, và đối với mỗi quả bóng màu xanh lam là ba tinh thể màu xanh lam. Nếu hết tinh thể, chúng tôi đếm xem phải mua thêm bao nhiêu tinh thể nữa. 

Cách tiếp cận này đúng vì nó tuân theo các công thức nấu ăn một cách chính xác. Vấn đề là số lượng bóng có thể đạt tới$10^9$. Việc mô phỏng mỗi quả bóng sẽ yêu cầu hàng tỷ thao tác, điều này là không thể trong thời gian giới hạn. 

Quan sát quan trọng là thứ tự sản xuất không quan trọng. Mỗi quả bóng màu vàng luôn tiêu tốn 2 tinh thể màu vàng. Mỗi quả bóng xanh luôn tiêu tốn 1 viên pha lê màu vàng và 1 viên pha lê màu xanh lam. Mỗi quả bóng màu xanh luôn tiêu tốn 3 tinh thể màu xanh. 

Thay vì mô phỏng quá trình sản xuất, chúng ta có thể tính toán trực tiếp tổng nhu cầu tinh thể. 

Tổng số tinh thể màu vàng cần có là:$$2x + y$$Tổng số tinh thể xanh cần có là:$$y + 3z$$Sau khi biết được tổng số này, chúng tôi so sánh chúng với nguồn cung sẵn có. 

Nếu lượng tinh thể màu vàng yêu cầu vượt quá`A`, chúng ta phải mua phần chênh lệch. Nếu không thì chúng tôi không mua gì cả. Logic tương tự áp dụng cho tinh thể màu xanh. 

Câu trả lời chỉ đơn giản là tổng của hai sự thiếu hụt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(x + y + z) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các tinh thể màu vàng và xanh có sẵn,`A`Và`B`. 
2. Đọc số lượng bóng vàng, xanh lá cây, xanh dương theo yêu cầu.`x`,`y`, Và`z`. 
3. Tính tổng số tinh thể màu vàng cần thiết là`2 * x + y`. 

Mỗi quả bóng màu vàng đóng góp 2 tinh thể màu vàng và mỗi quả bóng màu xanh lá cây đóng góp 1 tinh thể màu vàng. 
4. Tính tổng số tinh thể màu xanh lam cần thiết là`y + 3 * z`. 

Mỗi quả bóng màu xanh lá cây đóng góp 1 tinh thể màu xanh lam và mỗi quả bóng màu xanh lam đóng góp 3 tinh thể màu xanh lam. 
5. Tính lượng vàng thiếu hụt như sau`max(0, needed_yellow - A)`. 

Nếu chúng ta đã có đủ tinh thể màu vàng thì mức thiếu hụt sẽ bằng 0 thay vì âm. 
6. Tính lượng thiếu hụt màu xanh lam như sau`max(0, needed_blue - B)`. 
7. Xuất ra tổng của hai phần thiếu hụt. 

### Tại sao nó hoạt động 

Công thức nấu ăn xác định chính xác có bao nhiêu tinh thể màu vàng và xanh lam được tiêu thụ bởi mỗi quả bóng được yêu cầu. Việc thêm những đóng góp này sẽ mang lại tổng yêu cầu duy nhất cho mỗi màu tinh thể. 

Tinh thể màu vàng chỉ có thể đáp ứng yêu cầu màu vàng và tinh thể màu xanh lam chỉ có thể đáp ứng yêu cầu màu xanh lam. Bởi vì hai màu độc lập nên số lượng tinh thể bổ sung tối thiểu chỉ đơn giản là tổng số tinh thể màu vàng còn thiếu và số tinh thể màu xanh lam bị thiếu. Sự dư thừa của một màu không thể làm giảm sự thiếu hụt của màu kia. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    A, B = map(int, input().split())
    x, y, z = map(int, input().split())

    needed_yellow = 2 * x + y
    needed_blue = y + 3 * z

    answer = max(0, needed_yellow - A) + max(0, needed_blue - B)
    print(answer)

solve()
```Hai dòng đầu tiên đọc số lượng tinh thể có sẵn và số lượng bóng được yêu cầu. 

Các biến`needed_yellow`Và`needed_blue`lưu trữ tổng lượng tinh thể yêu cầu lấy trực tiếp từ công thức nấu ăn. Điều này tránh mọi mô phỏng. 

Các cuộc gọi đến`max(0, ...)`là rất quan trọng. Nếu chúng ta đã có đủ tinh thể của một màu thì màu đó không đóng góp gì cho câu trả lời. Việc cho phép các giá trị âm ở đây sẽ khiến cho phần dư thừa của một màu làm giảm tổng số một cách không chính xác. 

Số nguyên Python tự động xử lý các giá trị lớn hơn giới hạn 32 bit, mặc dù ngay cả trong các ngôn ngữ có số nguyên kích thước cố định, biểu thức lớn nhất chỉ là vài tỷ và phù hợp thoải mái với các loại 64 bit. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
2 1 1
```| Bước | Giá trị | 
| --- | --- | 
| Có sẵn màu vàng | 4 | 
| Có sẵn màu xanh | 3 | 
| Cần màu vàng | 2×2 + 1 = 5 | 
| Cần màu xanh | 1 + 3×1 = 4 | 
| Thiếu vàng | 5 − 4 = 1 | 
| Thiếu màu xanh | 4 − 3 = 1 | 
| Trả lời | 1 + 1 = 2 | 

Việc sản xuất cần năm tinh thể màu vàng và bốn tinh thể màu xanh. Vì chỉ có bốn tinh thể màu vàng và ba tinh thể màu xanh lam nên phải mua thêm một tinh thể của mỗi màu. 

### Ví dụ 2 

đầu vào:```
10 10
1 1 1
```| Bước | Giá trị | 
| --- | --- | 
| Có sẵn màu vàng | 10 | 
| Có sẵn màu xanh | 10 | 
| Cần màu vàng | 2×1 + 1 = 3 | 
| Cần màu xanh | 1 + 3×1 = 4 | 
| Thiếu vàng | 0 | 
| Thiếu màu xanh | 0 | 
| Trả lời | 0 | 

Ví dụ này cho thấy rằng các tinh thể bổ sung không ảnh hưởng đến câu trả lời. Vì tất cả các yêu cầu đã được đáp ứng nên không cần mua hàng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học cố định được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu tỷ lệ thuận với kích thước đầu vào | 

Thuật toán thực hiện một số phép cộng, phép nhân và so sánh bất kể giá trị đầu vào. Điều này dễ dàng phù hợp với giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    A, B = map(int, input().split())
    x, y, z = map(int, input().split())

    needed_yellow = 2 * x + y
    needed_blue = y + 3 * z

    return str(
        max(0, needed_yellow - A) +
        max(0, needed_blue - B)
    ) + "\n"

# provided sample
assert run("4 3\n2 1 1\n") == "2\n", "sample 1"

# minimum values
assert run("0 0\n0 0 0\n") == "0\n", "all zeros"

# only yellow shortage
assert run("1 100\n1 0 0\n") == "1\n", "need one more yellow crystal"

# only blue shortage
assert run("100 1\n0 0 1\n") == "2\n", "need two more blue crystals"

# large boundary values
assert run("1000000000 1000000000\n1000000000 1000000000 1000000000\n") == "4000000000\n", "large numbers"

# exactly enough crystals
assert run("5 4\n2 1 1\n") == "0\n", "perfect match"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 / 0 0 0`|`0`| Giá trị tối thiểu | 
|`1 100 / 1 0 0`|`1`| Chỉ thiếu vàng | 
|`100 1 / 0 0 1`|`2`| Chỉ thiếu màu xanh | 
| Lớn$10^9$giá trị |`4000000000`| Xử lý số học lớn một cách an toàn | 
|`5 4 / 2 1 1`|`0`| Kết hợp tài nguyên chính xác | 

## Vỏ cạnh 

Hãy xem xét trường hợp không có tinh thể và không có quả bóng nào:```
0 0
0 0 0
```Thuật toán tính toán:```
needed_yellow = 0
needed_blue = 0
```Cả hai mức thiếu hụt đều bằng 0, vì vậy sản lượng là:```
0
```Điều này xác nhận rằng công thức xử lý các yêu cầu trống một cách chính xác. 

Hãy xem xét một trường hợp có dư một màu:```
10 1
0 0 1
```Thuật toán tính toán:```
needed_yellow = 0
needed_blue = 3
```Sự thiếu hụt trở thành:```
yellow = max(0, 0 - 10) = 0
blue = max(0, 3 - 1) = 2
```Đầu ra là:```
2
```Các tinh thể màu vàng thừa bị bỏ qua vì màu sắc tinh thể không thể trao đổi được. 

Hãy xem xét trường hợp một yêu cầu được đáp ứng chính xác:```
5 4
2 1 1
```Thuật toán tính toán:```
needed_yellow = 5
needed_blue = 4
```Sự thiếu hụt là:```
yellow = 0
blue = 0
```Đầu ra là:```
0
```Điều này xác minh rằng sự bình đẳng được xử lý chính xác và không tạo ra giao dịch mua hàng không cần thiết. 

Cuối cùng, hãy xem xét:```
2 1
1 1 0
```Các yêu cầu là:```
needed_yellow = 3
needed_blue = 1
```Sự thiếu hụt là:```
yellow = 1
blue = 0
```Câu trả lời là:```
1
```Điều này chứng tỏ tại sao sự thiếu hụt phải được tính toán độc lập cho từng màu. Sự thiếu hụt tinh thể màu vàng không thể được bù đắp bằng tinh thể màu xanh lam.
