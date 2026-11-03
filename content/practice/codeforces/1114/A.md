---
title: "CF 1114A - Có nho không?"
description: "Chúng tôi có ba người có sở thích nho khác nhau và ba đống nho. Andrew muốn chính xác loại nho đầu tiên, nho xanh. Nếu anh ta cần x nho, tất cả đều phải đến từ đống xanh. Dmitry không thích nho đen. Anh ta có thể ăn nho xanh hoặc tím."
date: "2026-06-12T04:55:35+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1114
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 538 (Div. 2)"
rating: 800
weight: 1114
solve_time_s: 324
verified: false
draft: false
---

[CF 1114A - Có nho nào không?](https://codeforces.com/problemset/problem/1114/A) 

**Đánh giá:** 800 
**Tags:** vũ phu, tham lam, thực hiện 
**Thời gian giải:** 5 phút 24s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có ba người có sở thích nho khác nhau và ba đống nho. 

Andrew muốn chính xác loại nho đầu tiên, nho xanh. Nếu anh ấy cần`x`nho, tất cả đều phải đến từ đống xanh. 

Dmitry không thích nho đen. Anh ta có thể ăn nho xanh hoặc tím. Anh ấy cần ít nhất`y`tổng số nho từ hai màu đó. 

Michal không có hạn chế. Anh ta có thể ăn nho bất kỳ màu nào và cần ít nhất`z`quả nho. 

Đầu vào cung cấp số tiền cần thiết`x`,`y`, Và`z`, theo sau là số lượng có sẵn`a`,`b`, Và`c`nho xanh, tím và đen. Chúng ta phải xác định liệu có tồn tại một phân phối nào đó làm hài lòng cả ba người hay không. 

Những hạn chế là rất nhỏ. Mọi giá trị nhiều nhất là`10^5`và chỉ có một trường hợp thử nghiệm duy nhất. Ngay cả việc tìm kiếm mạnh mẽ trên nhiều khả năng cũng sẽ phù hợp một cách thoải mái, nhưng cấu trúc của vấn đề cho phép một giải pháp thời gian không đổi thậm chí còn đơn giản hơn. 

Phần khó khăn là màu sắc có mức độ linh hoạt khác nhau. Nho xanh là nguồn tài nguyên hạn chế nhất vì Andrew chỉ được ăn nho xanh. Nho tím dẻo hơn vì Dmitry có thể sử dụng chúng. Nho đen là loại nho dẻo nhất chỉ dành cho Michal, vì không ai khác có thể sử dụng chúng. Việc thực hiện bất cẩn có thể chỉ kiểm tra tổng số nho và bỏ lỡ những hạn chế này. 

Hãy xem xét ví dụ này:```
3 1 1
2 100 100
```Tổng số nho là quá đủ, nhưng Andrew cần ba quả nho xanh và chỉ có hai quả tồn tại. Câu trả lời đúng là`NO`. 

Một trường hợp tế nhị khác là Andrew có thể hài lòng nhưng Dmitry thì không.```
2 5 1
2 4 100
```Sau khi đưa cho Andrew hai quả nho xanh, không còn quả nho xanh nào nữa. Dmitry chỉ có thể sử dụng bốn quả nho tím, không đủ để đạt tới năm quả. Câu trả lời đúng là`NO`. 

Trường hợp thứ ba là cả Andrew và Dmitry đều có thể hài lòng nhưng Michal thì không.```
1 1 10
1 1 8
```Andrew lấy một quả nho xanh. Dmitry lấy một quả nho màu tím. Không còn lại gì, vì vậy Michal không nhận được quả nho nào mặc dù anh ấy cần mười quả. Câu trả lời đúng là`NO`. 

Những ví dụ này cho thấy thứ tự phân bổ có vấn đề. Những người tiêu dùng bị hạn chế nhất nên được kiểm tra đầu tiên. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử mọi cách có thể để phân chia số nho xanh giữa Andrew và Dmitry, sau đó kiểm tra xem số nho còn lại có thể làm hài lòng những người khác hay không. Vì mỗi kích thước cọc có thể đạt tới`10^5`, việc liệt kê tất cả các phân phối sẽ cần tới khoảng`10^5`khả năng. Điều đó vẫn có thể quản lý được ở đây, nhưng nó phức tạp không cần thiết. 

Quan sát quan trọng là các hạn chế tạo thành một hệ thống phân cấp tự nhiên. 

Andrew là người bị hạn chế nhất vì anh chỉ được ăn nho xanh. Nếu có ít hơn`x`nho xanh, không có giải pháp tồn tại. 

Sau khi Andrew nhận được nho của mình, số nho xanh còn lại sẽ có sẵn cho Dmitry. Dmitry có thể sử dụng cả nho xanh và nho tím. Nếu tổng số nho xanh cộng với tất cả nho tím còn lại ít hơn`y`, không có giải pháp tồn tại. 

Sau khi làm hài lòng Andrew và Dmitry, mọi quả nho còn sót lại, bất kể màu sắc, đều có thể được trao cho Michal. Nếu tổng số nho còn lại ít nhất`z`, tồn tại một phân phối hợp lệ. 

Bởi vì mỗi bước đều thỏa mãn người bị hạn chế nhất trước tiên, chúng tôi không bao giờ lãng phí nguồn lực có thể cần thiết sau này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(a) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Kiểm tra xem có ít nhất`x`nho xanh. Nếu như`a < x`, đầu ra`NO`bởi vì Andrew không thể hài lòng. 
2. Đưa cho Andrew số nho cần thiết bằng cách trừ đi`x`từ`a`. Số nho xanh còn lại hiện đã có sẵn cho Dmitry. 
3. Kiểm tra xem số nho xanh còn lại cộng với tất cả nho tím có thể làm hài lòng Dmitry hay không. Nếu như`a + b < y`, đầu ra`NO`. 
4. Đưa cho Dmitry những quả nho mà anh ấy yêu cầu. Vì Dmitry có thể sử dụng nho xanh trước rồi đến nho tím, hãy tính xem còn lại bao nhiêu nho sau khi tiêu thụ`y`nho từ hồ bơi kết hợp. 
5. Hãy để`remaining = a + b - y + c`. Đây là tổng số nho còn lại của Michal. 
6. Nếu`remaining >= z`, đầu ra`YES`. Nếu không thì xuất ra`NO`. 

### Tại sao nó hoạt động 

Yêu cầu của Andrew là nghiêm ngặt nhất. Mọi giải pháp hợp lệ đều phải bảo lưu`x`nho xanh cho anh ấy. Kiểm tra điều này đầu tiên là bắt buộc. 

Sau khi Andrew hài lòng, Dmitry có thể sử dụng tất cả những quả nho không có màu đen còn lại. Nếu tổng lượng nho xanh và nho tím còn lại ít hơn`y`, không có sự sắp xếp nào có thể giúp ích được vì đó là những màu duy nhất mà Dmitry chấp nhận. 

Sau khi Andrew và Dmitry hài lòng, Michal có thể tiêu thụ số nho còn sót lại. Tại thời điểm đó, chỉ có tổng số nho còn lại mới quan trọng. Nếu ít nhất`z`grapes remain, Michal can always take them regardless of color.

 Mỗi lần kiểm tra đều tương ứng chính xác với một điều kiện cần, và sau khi đáp ứng được những người hạn chế hơn trước thì những điều kiện đó cũng đủ. Hence the algorithm is correct.

 ## Giải pháp Python```python
import sys
input = sys.stdin.readline

x, y, z = map(int, input().split())
a, b, c = map(int, input().split())

if a < x:
    print("NO")
    sys.exit()

a -= x

if a + b < y:
    print("NO")
    sys.exit()

remaining = a + b - y + c

if remaining >= z:
    print("YES")
else:
    print("NO")
```Điều kiện đầu tiên xử lý Andrew. Nếu không có đủ nho xanh thì câu trả lời ngay lập tức là không thể. 

Sau khi trừ đi số nho của Andrew, biến`a`tượng trưng cho số nho xanh còn lại. Dmitry có thể sử dụng những thứ này cùng với tất cả nho tím, vì vậy điều kiện thứ hai sẽ kiểm tra`a + b`. 

biểu hiện`a + b - y`biểu thị số lượng nho xanh và tím còn lại sau khi Dmitry nhận được chính xác`y`quả nho. Thêm`c`bao gồm tất cả nho đen mà chỉ Michal mới có thể cần. 

Việc triển khai tránh việc xây dựng một bản phân phối một cách rõ ràng vì chỉ có số lượng mới quan trọng. Không cần vòng lặp và tất cả số học dễ dàng nằm gọn trong phạm vi số nguyên bình thường. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1 6 2
4 3 3
```| Bước | một | b | c | Hành động | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 4 | 3 | 3 | Bắt đầu | 
| Andrew kiểm tra | 4 | 3 | 3 | 4 ≥ 1, có thể | 
| Sau Andrew | 3 | 3 | 3 | Trừ 1 màu xanh lá cây | 
| Kiểm tra Dmitry | 3 | 3 | 3 | 3 + 3 = 6 ≥ 6 | 
| Sau Dmitry | 0 | 0 | 3 | Tiêu thụ 6 quả nho | 
| Kiểm tra Michal | 0 | 0 | 3 | 3 ≥ 2 | 

Kết quả:`YES`Ví dụ này cho thấy luồng dự định. Andrew ăn số nho xanh cần thiết trước tiên. Dmitry sau đó sử dụng tất cả nho xanh và tím còn lại. Những trái nho đen còn lại dành cho Michal. 

### Mẫu 2```
3 1 1
2 5 5
```| Bước | một | b | c | Hành động | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 2 | 5 | 5 | Bắt đầu | 
| Andrew kiểm tra | 2 | 5 | 5 | 2 < 3 | 

Kết quả:`NO`Ví dụ này thể hiện ràng buộc hạn chế nhất. Mặc dù có rất nhiều nho nhưng Andrew không thể nhận đủ nho xanh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một vài phép tính số học và so sánh | 
| Không gian | O(1) | Sử dụng số lượng biến không đổi | 

Thuật toán thực hiện một lượng công việc cố định bất kể giá trị đầu vào. Nó dễ dàng phù hợp với giới hạn thời gian một giây và mức sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    x, y, z = map(int, input().split())
    a, b, c = map(int, input().split())

    if a < x:
        return "NO"

    a -= x

    if a + b < y:
        return "NO"

    remaining = a + b - y + c

    return "YES" if remaining >= z else "NO"

# provided sample
assert run("1 6 2\n4 3 3\n") == "YES", "sample 1"

# custom cases
assert run("1 1 1\n1 1 1\n") == "YES", "minimum feasible"
assert run("3 1 1\n2 100 100\n") == "NO", "not enough green grapes"
assert run("2 5 1\n2 4 100\n") == "NO", "not enough green+purple for Dmitry"
assert run("100000 100000 100000\n100000 100000 100000\n") == "YES", "maximum values"
assert run("1 1 10\n1 1 8\n") == "NO", "not enough total remaining for Michal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 / 1 1 1`| CÓ | Trường hợp khả thi nhỏ nhất | 
|`3 1 1 / 2 100 100`| KHÔNG | Hạn chế của Andrew | 
|`2 5 1 / 2 4 100`| KHÔNG | Hạn chế của Dmitry | 
|`100000 100000 100000 / 100000 100000 100000`| CÓ | Giới hạn tối đa | 
|`1 1 10 / 1 1 8`| KHÔNG | Kiểm tra nho còn lại của Michal | 

## Vỏ cạnh 

###Không đủ nho xanh 

đầu vào:```
3 1 1
2 100 100
```Thuật toán kiểm tra ngay lập tức`a < x`. Từ`2 < 3`, nó xuất ra`NO`. 

Một giải pháp là không thể vì Andrew chỉ chấp nhận nho xanh. Nho thêm tím hoặc đen không thể bù đắp được. 

### Đủ nho xanh nhưng không đủ nho cho Dmitry 

đầu vào:```
2 5 1
2 4 100
```Andrew ăn hai quả nho xanh rồi bỏ đi:```
a = 0
b = 4
```Thuật toán kiểm tra`a + b < y`:```
0 + 4 < 5
```Điều này đúng, vì vậy câu trả lời là`NO`. 

Dmitry không thể ăn nho đen nên đống đen lớn không liên quan. 

### Đủ cho Andrew và Dmitry, nhưng chưa đủ cho Michal 

đầu vào:```
1 1 10
1 1 8
```Sau Andrew:```
a = 0
```Dmitry ăn một quả nho:```
a + b - y = 0
```Số nho còn lại bằng:```
0 + 8 = 8
```Từ`8 < 10`, Michal không thể thỏa mãn và thuật toán đưa ra`NO`. 

Điều này xác nhận rằng việc làm hài lòng hai người đầu tiên không tự động có nghĩa là còn đủ số nho cho người thứ ba.
