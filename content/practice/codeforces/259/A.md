---
title: "CF 259A - Chú Voi Nhỏ Và Cờ Vua"
description: "Chúng ta được cấp một bảng 8 × 8 trong đó mỗi ô có màu trắng (W) hoặc đen (B). Mục đích là để xác định xem bàn cờ này có thể biến thành bàn cờ tiêu chuẩn hay không. Một bàn cờ tiêu chuẩn có các màu xen kẽ theo chiều ngang và chiều dọc."
date: "2026-06-04T17:39:39+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "strings"]
categories: ["algorithms"]
codeforces_contest: 259
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 157 (Div. 2)"
rating: 1000
weight: 259
solve_time_s: 155
verified: true
draft: false
---

[CF 259A - Chú voi nhỏ và cờ vua](https://codeforces.com/problemset/problem/259/A) 

**Đánh giá:** 1000 
**Tags:** bạo lực, dây 
**Thời gian giải:** 2m 35s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một bảng 8×8 trong đó mỗi ô có màu trắng (`W`) hoặc đen (`B`). Mục đích là để xác định xem bàn cờ này có thể biến thành bàn cờ tiêu chuẩn hay không. 

Một bàn cờ tiêu chuẩn có các màu xen kẽ theo chiều ngang và chiều dọc. Vì góc trên bên trái phải có màu trắng nên chỉ có hai mẫu hàng hợp lệ:```
WBWBWBWB
BWBWBWBW
```Các hàng phải xen kẽ giữa hai mẫu này. 

Thao tác duy nhất được phép là chọn một hàng và thực hiện dịch chuyển phải theo chu kỳ với số lần bất kỳ. Vì một hàng chứa chính xác tám ô nên các phép dịch lặp lại có thể xoay hàng đó vào bất kỳ vị trí nào trong tám vị trí tuần hoàn của nó. 

Kích thước bảng được cố định ở mức 8 × 8. Điều đó có nghĩa là ngay cả một cuộc tìm kiếm toàn diện cũng sẽ rất nhỏ. Chỉ có tám hàng và mỗi hàng có độ dài tám, do đó, bất kỳ thuật toán nào kiểm tra tất cả các ô nhiều lần đều đủ nhanh. Thách thức không phải là hiệu quả mà là nhận ra cấu trúc được tạo ra bởi những thay đổi mang tính chu kỳ. 

Một quan sát tinh tế là việc dịch chuyển một hàng xen kẽ theo một vị trí chỉ đơn giản là hoán đổi nó giữa hai mẫu bàn cờ hợp lệ. Ví dụ:```
WBWBWBWB  -> shift once ->  BWBWBWBW
```Sự dịch chuyển hai vị trí sẽ đưa nó trở lại mẫu ban đầu. Vì độ dài của hàng là chẵn nên mỗi sự dịch chuyển theo chu kỳ của một hàng xen kẽ sẽ tạo ra một trong hai mẫu này và không có gì khác. 

Trường hợp cạnh chính là một hàng không xen kẽ bên trong. 

Ví dụ:```
WWWBWBWB
```Không có sự dịch chuyển theo chu kỳ nào có thể loại bỏ được giá trị bằng liền kề`W`S. Bất kỳ phép quay nào vẫn chứa một cặp ô bằng nhau lân cận ở đâu đó trong hàng. Câu trả lời đúng là`NO`. 

Một sai lầm dễ mắc phải khác là chỉ kiểm tra xem mỗi hàng có thể trở thành mẫu xen kẽ hợp lệ một cách độc lập hay không. 

Ví dụ:```
WBWBWBWB
WBWBWBWB
WBWBWBWB
WBWBWBWB
WBWBWBWB
WBWBWBWB
WBWBWBWB
WBWBWBWB
```Mỗi hàng đều hợp lệ riêng lẻ, nhưng một bàn cờ phù hợp yêu cầu các hàng lân cận phải khác nhau. Câu trả lời đúng vẫn là`YES`, bởi vì chúng ta có thể dịch chuyển mỗi hàng thứ hai một lần và thu được chuỗi hàng xen kẽ. Giải pháp chỉ so sánh các hàng với vị trí hiện tại của chúng sẽ từ chối trường hợp này một cách không chính xác. 

Trường hợp cạnh cuối cùng là một bàn cờ đã tạo thành một bàn cờ thích hợp. Câu trả lời phải là`YES`bởi vì việc sử dụng các thao tác bằng 0 được cho phép. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ nhất là thử mọi phép dịch chuyển theo chu kỳ có thể có cho mỗi hàng và kiểm tra xem bàn cờ kết quả có phải là bàn cờ hợp lệ hay không. 

Mỗi hàng có 8 khả năng quay, nên có:$$8^8 = 16,777,216$$các cấu hình có thể. Mặc dù hội đồng quản trị có quy mô nhỏ nhưng việc kiểm tra hơn 16 triệu bang là không cần thiết. 

Quan sát quan trọng đến từ việc nghiên cứu một hàng duy nhất. Để một hàng xuất hiện trong bàn cờ hợp lệ, các màu của nó phải xen kẽ nhau. Vì độ dài hàng là chẵn nên mỗi phép dịch chuyển theo chu kỳ sẽ bảo toàn thuộc tính xen kẽ. Quan trọng hơn, một hàng xen kẽ chỉ có thể xoay thành một trong hai mẫu:```
WBWBWBWB
BWBWBWBW
```Nếu một hàng không xen kẽ thì không có phép quay nào có thể làm cho nó xen kẽ được. 

Điều này làm giảm vấn đề đáng kể. Thay vì xem xét tất cả các ca có thể xảy ra, chúng ta chỉ cần xác minh rằng mỗi hàng đều luân phiên trong nội bộ. Khi điều đó đúng, mỗi hàng có thể được xoay thành bất kỳ mẫu hàng nào trong hai mẫu hàng trên bàn cờ mà chúng ta cần. 

Đối với các hàng số lẻ của bàn cờ mục tiêu, chúng ta cần:```
WBWBWBWB
```Đối với các hàng được đánh số chẵn, chúng ta cần:```
BWBWBWBW
```Vì mỗi hàng xen kẽ có thể được chuyển đổi thành một trong hai mẫu bằng cách chọn một ca chẵn lẻ thích hợp, nên toàn bộ bảng chỉ khả thi khi và chỉ nếu mỗi hàng xen kẽ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(8^8 · 64) | O(64) | Không cần thiết | 
| Tối ưu | O(64) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tám hàng của bảng. 
2. Đối với mỗi hàng, hãy kiểm tra từng cặp ô liền kề bên trong hàng đó. 
3. Nếu bất kỳ ô liền kề nào có cùng màu, xuất ngay`NO`. 

Một hàng trong bàn cờ phải có màu xen kẽ nhau. Các ca tuần hoàn chỉ xoay các vị trí, không thay đổi thứ tự các màu xung quanh chu kỳ. Nếu hai ô lân cận trong chu kỳ bằng nhau thì khiếm khuyết đó không bao giờ có thể biến mất. 
4. Nếu tất cả các hàng đều vượt qua bước kiểm tra luân phiên, hãy xuất ra`YES`. 

Mỗi hàng bây giờ là một chu kỳ xen kẽ. Vì độ dài của hàng là chẵn nên việc xoay hàng có thể tạo ra một trong hai`WBWBWBWB`hoặc`BWBWBWBW`. Chúng ta có thể chọn phiên bản yêu cầu một cách độc lập cho từng hàng và xây dựng bàn cờ tiêu chuẩn. 

### Tại sao nó hoạt động 

Hãy coi một hàng là một chu kỳ gồm tám ô. Sự dịch chuyển theo chu kỳ chỉ thay đổi vị trí bắt đầu trên chu kỳ đó. 

Nếu một hàng chứa hai ô lân cận bằng nhau ở đâu đó trong chu trình thì mọi phép quay vẫn chứa cùng một cặp bằng nhau đó. Một hàng như vậy không bao giờ có thể trở thành một hàng bàn cờ xen kẽ nhau. 

Ngược lại, nếu mỗi cặp lân cận trong chu kỳ có màu khác nhau thì hàng phải xen kẽ hoàn toàn. Vì độ dài chu kỳ là chẵn nên hàng bao gồm các chuỗi xen kẽ`W`Và`B`xung quanh toàn bộ chu kỳ. Xoay một hàng như vậy có thể bắt đầu ở một trong hai màu, tạo ra chính xác hai mẫu hàng bàn cờ. 

Bởi vì mỗi hàng có thể độc lập trở thành bất kỳ mô hình xen kẽ nào cần thiết, toàn bộ bàn cờ có thể được sắp xếp thành một bàn cờ thích hợp một cách chính xác khi mỗi hàng xen kẽ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    board = [input().strip() for _ in range(8)]

    for row in board:
        for i in range(7):
            if row[i] == row[i + 1]:
                print("NO")
                return

    print("YES")

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo sau bằng chứng. 

Vòng lặp bên ngoài xử lý tất cả tám hàng. Đối với mỗi hàng, chúng tôi kiểm tra bảy cặp liền kề bên trong hàng. Nếu bất kỳ cặp nào chứa các màu giống nhau, hàng đó không thể được chuyển thành hàng bàn cờ xen kẽ bằng bất kỳ chuỗi dịch chuyển tuần hoàn nào, vì vậy chúng tôi in ngay`NO`. 

Nếu mỗi hàng vượt qua bài kiểm tra thì mỗi hàng đã là một chuỗi xen kẽ. Các hàng như vậy có thể được xoay để bắt đầu bằng một trong hai màu, điều này cho phép chúng ta đặt mẫu được yêu cầu trên mỗi hàng bảng. Sau đó chúng tôi in`YES`. 

Một lỗi phổ biến là cố gắng mô phỏng sự thay đổi. Bằng chứng cho thấy việc mô phỏng là không cần thiết. Thuộc tính duy nhất quan trọng là liệu mỗi hàng có thay thế hay không. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
WBWBWBWB
BWBWBWBW
BWBWBWBW
BWBWBWBW
WBWBWBWB
WBWBWBWB
BWBWBWBW
WBWBWBWB
```| Hàng | Tìm thấy cặp bằng nhau liền kề? | 
| --- | --- | 
| WBWBWBWB | Không | 
| BWBWBWBW | Không | 
| BWBWBWBW | Không | 
| BWBWBWBW | Không | 
| WBWBWBWB | Không | 
| WBWBWBWB | Không | 
| BWBWBWBW | Không | 
| WBWBWBWB | Không | 

Tất cả các hàng xen kẽ bên trong, do đó thuật toán xuất ra:```
YES
```Ví dụ này chứng minh rằng ban đầu các hàng không cần phải sắp xếp đúng bàn cờ. Vì mỗi hàng đều xen kẽ nên các phép xoay phù hợp có thể tạo ra mẫu theo yêu cầu. 

### Ví dụ 2 

Hãy xem xét:```
WWWBWBWB
BWBWBWBW
WBWBWBWB
BWBWBWBW
WBWBWBWB
BWBWBWBW
WBWBWBWB
BWBWBWBW
```| Hàng | Cặp liền kề bằng nhau đầu tiên | 
| --- | --- | 
| WWWBWBWB | Vị trí 1 và 2 | 
| BWBWBWBW | Không có | 
| WBWBWBWB | Không có | 
| BWBWBWBW | Không có | 
| WBWBWBWB | Không có | 
| BWBWBWBW | Không có | 
| WBWBWBWB | Không có | 
| BWBWBWBW | Không có | 

Hàng đầu tiên chứa hai chữ liên tiếp`W`S. Không có sự dịch chuyển theo chu kỳ nào có thể loại bỏ được khiếm khuyết đó. 

Đầu ra:```
NO
```Ví dụ này minh họa tính bất biến chính: các ô liền kề bằng nhau vẫn nằm liền kề ở đâu đó sau mỗi lần quay. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(64) | Tám hàng, bảy kiểm tra kề mỗi hàng | 
| Không gian | O(1) | Chỉ bảng đầu vào được lưu trữ | 

Kích thước bảng được cố định ở mức 8 × 8, do đó thời gian chạy hầu như không đổi. Giải pháp thấp hơn nhiều so với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    board = [input().strip() for _ in range(8)]

    for row in board:
        for i in range(7):
            if row[i] == row[i + 1]:
                return "NO"

    return "YES"

# provided sample
assert run(
"""WBWBWBWB
BWBWBWBW
BWBWBWBW
BWBWBWBW
WBWBWBWB
WBWBWBWB
BWBWBWBW
WBWBWBWB
"""
) == "YES", "sample"

# already a perfect chessboard
assert run(
"""WBWBWBWB
BWBWBWBW
WBWBWBWB
BWBWBWBW
WBWBWBWB
BWBWBWBW
WBWBWBWB
BWBWBWBW
"""
) == "YES", "already valid"

# all white
assert run(
"""WWWWWWWW
WWWWWWWW
WWWWWWWW
WWWWWWWW
WWWWWWWW
WWWWWWWW
WWWWWWWW
WWWWWWWW
"""
) == "NO", "all equal"

# one bad row
assert run(
"""WBWBWBWB
BWBWBWBW
WBWWWBWB
BWBWBWBW
WBWBWBWB
BWBWBWBW
WBWBWBWB
BWBWBWBW
"""
) == "NO", "single invalid row"

# all rows alternating but identical
assert run(
"""WBWBWBWB
WBWBWBWB
WBWBWBWB
WBWBWBWB
WBWBWBWB
WBWBWBWB
WBWBWBWB
WBWBWBWB
"""
) == "YES", "rows can be shifted independently"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bàn cờ hoàn hảo | CÓ | Không được phép hoạt động | 
| Tất cả các tế bào trắng | KHÔNG | Không thể sửa các màu bằng nhau liền kề | 
| Một hàng xấu | KHÔNG | Một hàng không hợp lệ khiến bảng không thể hoạt động | 
| Tất cả các hàng bằng nhau và xen kẽ | CÓ | Xoay hàng độc lập là đủ | 

## Vỏ cạnh 

Xét bảng:```
WWWBWBWB
BWBWBWBW
WBWBWBWB
BWBWBWBW
WBWBWBWB
BWBWBWBW
WBWBWBWB
BWBWBWBW
```Hàng đầu tiên chứa`WW`lúc đầu. Trong quá trình quét, thuật toán tìm thấy`row[0] == row[1]`và ngay lập tức quay trở lại`NO`. Việc xoay hàng chỉ di chuyển cặp này đến vị trí khác, do đó việc từ chối là chính xác. 

Hãy xem xét một bảng trong đó mỗi hàng bằng:```
WBWBWBWB
```lặp lại tám lần. 

Thuật toán không tìm thấy ô liền kề bằng nhau trong bất kỳ hàng nào và trả về`YES`. Điều này đúng vì mỗi hàng thứ hai có thể được dịch chuyển một lần, tạo ra:```
WBWBWBWB
BWBWBWBW
WBWBWBWB
BWBWBWBW
...
```đó chính xác là một bàn cờ thích hợp. 

Hãy xem xét một bàn cờ đã là bàn cờ hợp lệ:```
WBWBWBWB
BWBWBWBW
WBWBWBWB
BWBWBWBW
WBWBWBWB
BWBWBWBW
WBWBWBWB
BWBWBWBW
```Mỗi hàng xen kẽ nhau nên thuật toán trả về`YES`. Vì không được phép thực hiện thao tác nào nên đây là kết quả chính xác.
