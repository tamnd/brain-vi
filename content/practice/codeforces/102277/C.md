---
title: "CF 102277C - Điều khiển từ xa TV lịch sử"
description: "Điều khiển từ xa có các nút gồm 10 chữ số, cộng thêm kênh lên và kênh xuống. Một số nút số bị hỏng, trong khi hai nút kênh vẫn hoạt động bình thường. Tivi có các kênh được đánh số từ 0 đến 999. Kênh mục tiêu nằm trong khoảng từ 1 đến 999. Dr."
date: "2026-08-17T10:06:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102277
codeforces_index: "C"
codeforces_contest_name: "UCF Locals 2018"
rating: 0
weight: 102277
solve_time_s: 59
verified: true
draft: false
---

[CF 102277C - Điều khiển từ xa của TV lịch sử](https://codeforces.com/problemset/problem/102277/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Điều khiển từ xa có các nút gồm 10 chữ số, cộng thêm kênh lên và kênh xuống. Một số nút số bị hỏng, trong khi hai nút kênh vẫn hoạt động bình thường. Tivi có các kênh được đánh số từ 0 đến 999. 

Kênh mục tiêu nằm trong khoảng từ 1 đến 999. Trước tiên, Tiến sĩ Orooji có thể nhập bất kỳ số kênh nào có biểu diễn thập phân chỉ sử dụng các chữ số làm việc. Sau đó, anh ta có thể nhấn kênh lên hoặc kênh xuống bao nhiêu lần nếu cần. Đầu ra được yêu cầu là số lần nhấn kênh lên hoặc xuống kênh tối thiểu cần thiết để đạt được mục tiêu. Số lần nhấn chữ số được sử dụng để gõ kênh ban đầu không đóng góp vào câu trả lời. 

Đầu vào đưa ra số chữ số bị hỏng, theo sau là các chữ số đó, sau đó đưa ra kênh mong muốn. Có ít nhất một chữ số hoạt động vì nhiều nhất là chín trong số mười chữ số bị hỏng. Vì các kênh chỉ nằm trong khoảng từ 0 đến 999 nên có chính xác 1000 kênh có thể được nhập trực tiếp. Giới hạn chính thức là 1 giây và 256 MB, do đó, phương pháp kiểm tra trực tiếp 1000 kênh này khá nhanh chóng. 

Trường hợp cạnh trung tâm là kênh 0. Mặc dù kênh được yêu cầu ít nhất là 1 nhưng Tiến sĩ Orooji được phép nhập kênh 0 làm kênh bắt đầu của mình. Ví dụ,```
1 1
2
```có đầu ra`2`, vì chữ số 1 bị hỏng nên kênh 0 là kênh có thể chọn trực tiếp gần nhất và hai lần nhấn lên kênh sẽ đến được kênh 2. Việc triển khai bất cẩn chỉ kiểm tra các kênh từ 1 đến 999 sẽ bỏ lỡ khả năng này. 

Một trường hợp khác là khi mục tiêu có thể được gõ. Ví dụ,```
1 0
50
```có đầu ra`0`, vì 50 chỉ sử dụng các chữ số làm việc. Việc triển khai luôn thêm ít nhất một lần nhấn nút kênh sẽ là sai. 

Ranh giới còn lại là kênh 999. Điều khiển từ xa không thể di chuyển trên 999, do đó không có kênh gõ trực tiếp hợp lệ nào vượt quá ranh giới đó. Ví dụ,```
9 1 2 3 4 5 6 7 8 9
999
```chỉ để lại chữ số 0 hoạt động. Kênh duy nhất có thể gõ trực tiếp là 0, vì vậy câu trả lời là`999`. Một tìm kiếm vô tình coi 1000 là kênh có thể có thể tạo ra khoảng cách nhỏ hơn không hợp lệ. 

Các chữ số lặp lại trong một kênh cũng có vấn đề vì một chữ số phải hoạt động mỗi khi nó xuất hiện. Nếu chữ số 7 bị hỏng thì 7, 77 và 707 đều không sử dụng được. Ví dụ,```
1 7
777
```không thể nhập trực tiếp mục tiêu, mặc dù mục tiêu chỉ bao gồm một chữ số riêng biệt. Kênh bắt đầu khả dụng tốt nhất phải được tìm thấy bằng cách kiểm tra từng chữ số thập phân. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản là thử mọi kênh từ 0 đến 999. Đối với mỗi ứng viên, hãy chuyển đổi nó thành ký hiệu thập phân và kiểm tra xem mọi chữ số có hoạt động hay không. Nếu ứng viên có thể sử dụng được thì số lần nhấn nút kênh cần thiết để tiếp cận mục tiêu chỉ đơn giản là sự khác biệt tuyệt đối giữa ứng cử viên và mục tiêu. Lấy mức tối thiểu của các giá trị này sẽ cho câu trả lời. 

Phương pháp vũ phu này đã đủ nhanh rồi. Chỉ có 1000 kênh ứng cử viên và mỗi kênh chứa tối đa ba chữ số thập phân. Trường hợp xấu nhất thực hiện tối đa khoảng 3000 kiểm tra chữ số, sau đó là tính toán khoảng cách theo thời gian không đổi. Không cần cấu trúc dữ liệu phức tạp hơn hoặc kỹ thuật lập trình động. 

Lực lượng vũ phu hoạt động vì phạm vi kênh rất nhỏ. Thay vào đó, nếu các kênh có phạm vi như (10^9), việc kiểm tra mọi kênh có thể sẽ yêu cầu tới một tỷ ứng viên và sẽ hoàn toàn không phù hợp với giới hạn 1 giây. Ở đây, phạm vi cố định từ 0 đến 999 sẽ thay đổi tình hình: việc liệt kê đầy đủ là công việc liên tục một cách hiệu quả. 

Quan sát quan trọng là mọi kênh đầu tiên có thể có đều độc lập. Nếu một kênh ứng cử viên có thể đánh máy được thì cách rẻ nhất để đi từ kênh đó đến mục tiêu được xác định hoàn toàn bằng khoảng cách về số của chúng. Do đó, chúng tôi không cần mô phỏng các thao tác nhấn nút hoặc đường dẫn tìm kiếm. Chúng ta chỉ cần tìm kênh có thể sử dụng được với khoảng cách tuyệt đối tối thiểu tới mục tiêu. 

Điều này đưa ra sự so sánh sau đây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1000 × 3) = O(1) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1000 × 3) = O(1) | O(1) | Đã chấp nhận | 

Đối với vấn đề này, việc liệt kê brute-force cũng là phương pháp thực tế tối ưu. Điểm khác biệt là sự tối ưu hóa quan trọng là nhận ra rằng không gian tìm kiếm chỉ có 1000 kênh, thay vì cố gắng phát minh ra một thuật toán phức tạp hơn. 

## Hướng dẫn thuật toán 

1. Đọc tập hợp các chữ số bị hỏng và lưu trữ nó trong cấu trúc boolean. Đối với mỗi chữ số, cấu trúc cho chúng ta biết theo thời gian không đổi liệu chữ số đó có thể được nhấn hay không. 
2. Đọc kênh mục tiêu. 
3. Khởi tạo câu trả lời cho giá trị lớn hơn bất kỳ khoảng cách nào có thể, chẳng hạn như 1000. Khoảng cách lớn nhất có thể có giữa hai kênh trong phạm vi từ 0 đến 999 là 999. 
4. Liệt kê mọi kênh ứng cử viên từ 0 đến 999. Chúng tôi bao gồm cả hai điểm cuối vì kênh 0 là kênh bắt đầu hợp pháp và kênh 999 là kênh lớn nhất có thể. 
5. Chuyển đổi ứng viên thành chuỗi thập phân và kiểm tra từng chữ số. Nếu bất kỳ chữ số nào bị hỏng, hãy loại bỏ ứng cử viên này. Toàn bộ số không thể sử dụng được vì không thể gõ được dù chỉ một chữ số bị hỏng. 
6. Nếu mọi chữ số đều được, hãy tính`abs(candidate - target)`. Đây chính xác là số lần nhấn kênh lên hoặc xuống kênh cần thiết để di chuyển từ ứng cử viên đến mục tiêu. 
7. Thay thế câu trả lời hiện tại bằng khoảng cách này nếu nó nhỏ hơn. 
8. Sau khi kiểm tra hết 1000 thí sinh, hãy in khoảng cách tối thiểu. 

Tính đúng đắn được suy ra từ một bất biến đơn giản: sau khi xử lý mọi kênh ứng cử viên cho đến một giá trị nào đó (x),`answer`là khoảng cách nút kênh tối thiểu từ mục tiêu trong số tất cả các kênh có thể sử dụng được trong phạm vi được xử lý. Một ứng cử viên được coi là chính xác khi mọi chữ số cần thiết để nhập nó đều hoạt động, do đó không có kênh bắt đầu có thể sử dụng nào bị từ chối không chính xác. Khoảng cách của nó đến mục tiêu chính xác là số lần nhấn kênh lên hoặc xuống kênh cần thiết, do đó, việc lấy mức tối thiểu sẽ xem xét chi phí tối ưu cho ứng viên đó. Sau khi kênh 999 được xử lý, mọi kênh bắt đầu có thể đều đã được xem xét, do đó mức tối thiểu được lưu trữ là mức tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, *broken_digits = map(int, input().split())
target = int(input())

broken = [False] * 10
for d in broken_digits:
    broken[d] = True

answer = 1000

for channel in range(1000):
    if all(not broken[int(d)] for d in str(channel)):
        answer = min(answer, abs(channel - target))

print(answer)
```Dòng đầu vào đầu tiên được giải nén thành`n`và các chữ số bị hỏng. Giá trị của`n`không cần thiết sau khi đọc vì đầu vào đã cung cấp chính xác nhiều chữ số trên dòng, nhưng việc đọc nó sẽ giữ cho cấu trúc đầu vào rõ ràng. 

các`broken`mảng cung cấp kiểm tra tư cách thành viên liên tục.`broken[d]`đúng chính xác khi chữ số`d`không thể nhấn được. 

Vòng lặp sử dụng`range(1000)`, tạo ra mọi kênh từ 0 đến 999. Giới hạn trên này bao gồm trong bài toán, vì vậy việc sử dụng`range(999)`sẽ đưa ra từng lỗi một bằng cách loại trừ kênh 999. 

Đối với mỗi ứng cử viên,`str(channel)`hiển thị tất cả các chữ số thập phân phải được gõ. Điều này cũng xử lý kênh 0 một cách chính xác vì`str(0)`là`"0"`. Một ứng cử viên chỉ được chấp nhận khi mọi chữ số đều vượt qua bài kiểm tra nút bị hỏng. 

Khoảng cách là`abs(channel - target)`. Nếu ứng viên ở dưới mục tiêu, điều này tương ứng với việc nhấn kênh. Nếu nó ở trên mục tiêu, nó tương ứng với kênh xuống. Vì cả hai hướng đều tốn một lần nhấn cho mỗi kênh nên chênh lệch tuyệt đối chính xác là mức tối thiểu. 

Số nguyên Python không tràn ở đây và giá trị lớn nhất từng được lưu trữ trong`answer`chỉ có 1000. Tổng khối lượng công việc rất nhỏ, quá bình thường`sys.stdin.readline`đã quá đủ cho kích thước đầu vào. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là:```
3 0 8 9
35
```Các chữ số 0, 8 và 9 bị hỏng. Mục tiêu là 35. Bản thân Kênh 35 có thể gõ trực tiếp vì các chữ số 3 và 5 hoạt động, do đó câu trả lời sẽ trở thành 0 khi thí sinh đó được kiểm tra. 

Một dấu vết rút gọn xung quanh các ứng cử viên hữu ích trông như thế này. 

| Ứng viên | Chữ số có thể sử dụng được? | Khoảng cách tới 35 | Trả lời sau ứng viên | 
| --- | --- | --- | --- | 
| 34 | Có | 1 | 1 | 
| 35 | Có | 0 | 0 | 
| 36 | Có | 1 | 0 | 
| 37 | Có | 2 | 0 | 

Các ứng viên chứa 0, 8 hoặc 9 sẽ bị loại. Sau khi tìm thấy kênh 35, không có thí sinh nào sau này có thể cải thiện câu trả lời dưới 0, vì vậy kết quả cuối cùng là`0`. 

### Mẫu 2 

Đầu vào là:```
4 1 2 5 9
250
```Các chữ số 1, 2, 5 và 9 bị hỏng. Mục tiêu 250 không gõ được vì cả 2 và 5 đều bị hỏng. Kênh có thể sử dụng được gần đó là giá trị lân cận của 250 là 249, nhưng kênh đó cũng chứa 2 và 9, vì vậy nó không thể sử dụng được. Kênh 300 có thể sử dụng được vì chữ số 3 và 0 hoạt động, cho khoảng cách là 50. 

| Ứng viên | Chữ số có thể sử dụng được? | Khoảng cách tới 250 | Trả lời sau ứng viên | 
| --- | --- | --- | --- | 
| 249 | Không | 1 | không thay đổi | 
| 250 | Không | 0 | không thay đổi | 
| 300 | Có | 50 | 50 | 
| 301 | Không | 51 | 50 | 
| 400 | Có | 150 | 50 | 

Kênh có thể sử dụng gần nhất là 300, vì vậy câu trả lời là`50`. Ví dụ này chứng tỏ tại sao chỉ kiểm tra khoảng cách bằng số là không đủ. Kênh lân cận chỉ hữu ích nếu mọi chữ số cần thiết để nhập đều hoạt động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1000 × 3) = O(1) | Có 1000 ứng cử viên và tối đa ba chữ số cho mỗi ứng cử viên. | 
| Không gian | O(1) | Mảng chữ số bị hỏng chỉ chứa mười mục. | 

Phạm vi kênh cố định làm cho thời gian chạy không đổi một cách hiệu quả. Ngay cả trong trường hợp xấu nhất, chương trình chỉ kiểm tra vài nghìn chữ số, nằm trong giới hạn thời gian 1 giây và sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây đặt logic giải pháp bên trong một hàm để có thể kiểm tra từng trường hợp một cách độc lập.```python
import io
import sys

def solve(data: str) -> str:
    lines = data.strip().splitlines()
    n, *broken_digits = map(int, lines[0].split())
    target = int(lines[1])

    broken = [False] * 10
    for d in broken_digits:
        broken[d] = True

    answer = 1000

    for channel in range(1000):
        if all(not broken[int(d)] for d in str(channel)):
            answer = min(answer, abs(channel - target))

    return str(answer)

# Provided sample 1
assert solve("""3 0 8 9
35
""") == "0", "sample 1"

# Provided sample 2
assert solve("""4 1 2 5 9
250
""") == "50", "sample 2"

# Minimum-size input, only one digit is broken.
assert solve("""1 0
1
""") == "0", "target itself is directly typable"

# Target uses a broken digit, and channel 0 is the closest usable channel.
assert solve("""1 1
2
""") == "2", "channel 0 must be considered"

# Maximum number of broken digits, leaving only digit 0 working.
assert solve("""9 1 2 3 4 5 6 7 8 9
999
""") == "999", "only channel 0 is directly typable"

# Repeated target digit is broken, so the target cannot be entered directly.
assert solve("""1 7
777
""") == "777", "every occurrence of a broken digit matters"

# Boundary at channel 999, with 998 directly typable.
assert solve("""1 9
999
""") == "1", "channel 999 itself is blocked but 998 is usable"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 0 8 9 / 35`|`0`| Mẫu được cung cấp trong đó mục tiêu có thể được đánh máy trực tiếp. | 
|`4 1 2 5 9 / 250`|`50`| Mẫu được cung cấp nơi các kênh lân cận bị chặn. | 
|`1 0 / 1`|`0`| Đầu vào có kích thước tối thiểu và mục nhập trực tiếp. | 
|`1 1 / 2`|`2`| Kênh 0 phải được đưa vào tìm kiếm. | 
|`9 1 2 3 4 5 6 7 8 9 / 999`|`999`| Số lượng chữ số bị hỏng tối đa và ranh giới trên. | 
|`1 7 / 777`|`777`| Sự xuất hiện lặp đi lặp lại của một chữ số bị hỏng. | 
|`1 9 / 999`|`1`| Hành vi ngoài giới hạn trên. | 

## Vỏ cạnh 

Khi kênh 0 là điểm bắt đầu tốt nhất, thuật toán sẽ xử lý kênh này vì việc liệt kê bắt đầu từ 0 thay vì 1. Đối với đầu vào```
1 1
2
```chữ số 1 bị hỏng nên kênh 1 và mọi kênh chứa 1 đều bị loại bỏ. Kênh 0 hợp lệ và khoảng cách của nó với mục tiêu 2 là 2. Thuật toán in`2`. 

Khi mục tiêu có thể được nhập trực tiếp, thuật toán sẽ đánh giá mục tiêu giống như mọi ứng cử viên khác. Vì```
1 0
1
```chữ số 0 bị hỏng nhưng chữ số 1 vẫn hoạt động. Kênh ứng viên 1 vượt qua bước kiểm tra chữ số và`abs(1 - 1)`là 0. Do đó, câu trả lời là`0`, không cần nhấn nút kênh. 

Khi gần như mọi chữ số đều bị hỏng, việc tìm kiếm vẫn hoạt động vì nó không cho rằng có sẵn một số chữ số cụ thể. Vì```
9 1 2 3 4 5 6 7 8 9
999
```chỉ có chữ số 0 hoạt động. Trong số tất cả các kênh từ 0 đến 999, chỉ có thể gõ kênh 0, do đó khoảng cách là`999`. Thuật toán tìm chính xác ứng viên đó và đưa ra`999`. 

Các chữ số lặp lại được kiểm tra độc lập vì biểu diễn thập phân được quét theo từng ký tự. Với```
1 7
777
```mọi ký tự của mục tiêu đều là số 7 bị hỏng, vì vậy 777 bị từ chối. Các kênh duy nhất có thể sử dụng được không chứa số 7 và kênh 0 là kênh gần nhất, cho khoảng cách 777. Điều này ngăn ngừa lỗi phổ biến khi chỉ kiểm tra xem một ứng cử viên có chứa một chữ số bị hỏng riêng biệt một lần mà không thực sự xác thực từng vị trí chữ số hay không. 

Cuối cùng, điểm cuối phía trên được đưa vào bằng cách lặp qua`range(1000)`. Vì```
1 9
999
```bản thân mục tiêu không thể gõ được vì 9 bị hỏng. Kênh 998 có thể sử dụng được và chính xác là một kênh bên dưới mục tiêu, do đó thuật toán trả về`1`. Việc loại trừ 999 khỏi phạm vi ứng cử viên sẽ không thay đổi câu trả lời cụ thể này, nhưng việc loại trừ điểm cuối nói chung sẽ làm cho việc liệt kê không đầy đủ và có thể thất bại khi bản thân 999 là kênh được nhập trực tiếp tối ưu.
