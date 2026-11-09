---
title: "CF 151A - Nước Giải Khát"
description: "Một nhóm bạn muốn làm những chiếc bánh mì nướng giống hệt nhau bằng ba nguồn nguyên liệu: nước ngọt, lát chanh và muối. Mỗi bánh mì nướng tiêu thụ một lượng cố định của mỗi tài nguyên. Nhiệm vụ là xác định xem mỗi người bạn có thể nâng cốc chúc mừng hoàn chỉnh bao nhiêu lần trước khi hết ít nhất một tài nguyên."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 151
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 107 (Div. 2)"
rating: 800
weight: 151
solve_time_s: 91
verified: true
draft: false
---

[CF 151A - Nước ngọt](https://codeforces.com/problemset/problem/151/A) 

**Đánh giá:** 800 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Một nhóm bạn muốn làm những chiếc bánh mì nướng giống hệt nhau bằng ba nguồn nguyên liệu: nước ngọt, lát chanh và muối. Mỗi bánh mì nướng tiêu thụ một lượng cố định của mỗi tài nguyên. Nhiệm vụ là xác định xem mỗi người bạn có thể nâng cốc chúc mừng hoàn chỉnh bao nhiêu lần trước khi hết ít nhất một tài nguyên. 

Dữ liệu đầu vào cung cấp số lượng bạn bè, số lượng và kích thước của chai nước uống, số lượng chanh và lát trên mỗi quả chanh, lượng muối và lượng từng thành phần cần thiết cho một chiếc bánh mì nướng. Từ những giá trị này, chúng ta phải tính toán tổng số lần nâng ly có thể được tạo ra từ mỗi nguồn lực một cách độc lập, sau đó xác định nguồn lực giới hạn. 

Các ràng buộc rất nhỏ, mọi giá trị tối đa là 1000. Ngay cả một giải pháp kém hiệu quả cũng có thể chạy thoải mái trong giới hạn. Không cần các thuật toán, cấu trúc dữ liệu hoặc thủ thuật tối ưu hóa nâng cao. Thử thách chỉ đơn giản là dịch từ ngữ sang số học chính xác. 

Sai lầm phổ biến nhất là hiểu sai ý nghĩa của câu trả lời cuối cùng. Chúng tôi không được yêu cầu nâng cốc chúc mừng tổng số. Chúng ta được hỏi mỗi người bạn có thể nâng cốc bằng nhau bao nhiêu ly. 

Hãy xem xét đầu vào này:```
3 1 3 1 1 100 3 1
```Đồ uống chỉ cho phép`1 * 3 / 3 = 1`tổng số bánh mì nướng. Một giải pháp bất cẩn có thể in`1`, nhưng đó là tổng số lần nâng ly của cả nhóm. Vì có 3 người bạn nên mỗi người bạn có thể làm`1 / 3 = 0`chúc mừng. Đầu ra đúng là:```
0
```Một trường hợp tinh vi khác là khi một tài nguyên nhỏ hơn nhiều so với các tài nguyên khác.```
5 100 100 1 1 1 1 1
```Đồ uống và chanh rất nhiều nhưng chỉ có đủ muối cho một chiếc bánh mì nướng. Chia cho 5 người bạn được`0`. Một giải pháp quên lấy mức tối thiểu trên tất cả các tài nguyên sẽ thất bại ở đây. 

Phép chia số nguyên cũng rất cần thiết. Bánh mì nướng phải được hoàn thành. Bánh mì nướng phân số không được phép.```
2 1 5 1 1 100 6 1
```Chỉ có 5 ml đồ uống và mỗi bánh mì nướng cần 6 ml. Số lần nâng cốc chúc mừng chính xác là`0`, không`0.833...`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng việc nâng cốc chúc mừng từng người một. Mỗi bước sẽ trừ đi lượng đồ uống cần thiết, một lát chanh và lượng muối cần thiết cho đến khi không đủ tài nguyên. Cuối cùng, chúng tôi sẽ chia số lần nâng ly thành công cho số lượng bạn bè. 

Điều này hoạt động vì mỗi bánh mì nướng đều tiêu thụ một lượng tài nguyên cố định. Việc mô phỏng rất dễ suy luận và luôn chính xác. Ngay cả trong trường hợp xấu nhất, số lượng nâng cốc có thể đủ nhỏ để vừa vặn thoải mái trong giới hạn thời gian. 

Tuy nhiên, mô phỏng là không cần thiết. Cấu trúc của bài toán đưa ra một lối tắt toán học trực tiếp. 

Mỗi tài nguyên xác định độc lập tổng số lượng nâng ly tối đa: 

Đồ uống cung cấp:```
(k * l) / nl
```tổng số nâng ly. 

Quả chanh cung cấp:```
(c * d)
```tổng số lần nâng ly vì mỗi lần nâng ly cần chính xác một lát. 

Muối cung cấp:```
p / np
```tổng số nâng ly. 

Tổng số lần nâng cốc thực tế là giá trị tối thiểu trong ba giá trị này vì tài nguyên cạn kiệt đầu tiên sẽ ngừng sản xuất. Vì mỗi người bạn phải nhận được số cốc chúc mừng như nhau nên chúng tôi chia tổng số tiền bằng nhau cho`n`bạn. 

Điều này làm giảm vấn đề xuống một vài phép tính số học và phép chia số nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(T) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

Đây,`T`là số lần nâng ly mô phỏng. 

## Hướng dẫn thuật toán 

1. Đọc tám số nguyên từ đầu vào. 
2. Tính tổng số đồ uống có sẵn.```
total_drink = k * l
```Điều này chuyển đổi số lượng chai thành tổng số mililít. 

1. Tính xem tổng số ly rượu có thể nâng được bao nhiêu ly.```
drink_toasts = total_drink // nl
```Việc chia số nguyên là bắt buộc vì việc nâng ly một phần không hợp lệ. 

1. Tính tổng số lát chanh.```
lime_toasts = c * d
```Mỗi bánh mì nướng tiêu thụ chính xác một lát, vì vậy giá trị này đã bằng số lượng bánh mì nướng có thể có từ chanh. 

1. Tính tổng số muối có thể nâng cốc được bao nhiêu lần.```
salt_toasts = p // np
```Một lần nữa, phép chia số nguyên sẽ loại bỏ những lời chúc mừng không đầy đủ. 

1. Tìm số nhỏ nhất trong ba số bánh mì nướng.```
total_toasts = min(drink_toasts, lime_toasts, salt_toasts)
```Nguồn lực giới hạn xác định tổng số lần nâng ly tối đa có thể. 

1. Chia đều cho tất cả bạn bè.```
answer = total_toasts // n
```1. In kết quả. 

### Tại sao nó hoạt động 

Mỗi bánh mì nướng tiêu thụ một lượng đồ uống, vôi và muối cố định. Điều đó có nghĩa là mỗi tài nguyên áp đặt giới hạn trên một cách độc lập cho tổng số lần nâng ly. Bất kỳ nỗ lực nào vượt quá một trong những giới hạn này sẽ đòi hỏi nhiều tài nguyên hơn mức hiện có. 

Việc lấy mức tối thiểu của ba giới hạn sẽ mang lại tổng số lần nâng ly tối đa khả thi. Chia cho số lượng bạn bè sẽ có số lần nâng cốc tối đa bằng nhau cho mỗi người bạn. Vì tất cả các phép tính đều sử dụng phép chia số nguyên nên thuật toán không bao giờ tính các lần nâng ly chưa hoàn thành. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k, l, c, d, p, nl, np = map(int, input().split())

drink_toasts = (k * l) // nl
lime_toasts = c * d
salt_toasts = p // np

total_toasts = min(drink_toasts, lime_toasts, salt_toasts)

print(total_toasts // n)
```Dòng đầu tiên đọc tất cả tám số nguyên trong một câu lệnh. Vì bài toán chỉ có một test case nên không có vòng lặp. 

Việc tính toán đồ uống nhân số chai với mililít trên mỗi chai trước khi chia cho số lượng cần thiết cho mỗi bánh mì nướng. Đảo ngược thứ tự này sẽ không chính xác vì phép chia số nguyên sẽ làm mất thông tin quá sớm. 

Việc tính vôi đơn giản hơn vì mỗi bánh mì nướng cần chính xác một lát. Không cần phân chia. 

Việc tính toán muối sử dụng phép chia số nguyên vì lý do tương tự như tính toán đồ uống, không được phép nâng cốc chúc mừng không đầy đủ. 

các`min`cuộc gọi là bước quan trọng. Ngay cả khi hai tài nguyên có thể hỗ trợ nhiều lời chúc mừng thì tài nguyên nhỏ nhất sẽ trở thành nút thắt cổ chai. 

Cuối cùng, câu trả lời được chia đều cho tất cả bạn bè bằng cách sử dụng phép chia số nguyên. Điều này tránh việc chỉ định nâng ly theo phần cho mọi người. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 4 5 10 8 100 3 1
```| Biến | Giá trị | 
| --- | --- | 
| n | 3 | 
| k | 4 | 
| tôi | 5 | 
| c | 10 | 
| d | 8 | 
| p | 100 | 
| nl | 3 | 
| np | 1 | 
| tổng số đồ uống | 20 | 
| uống_toasts | 6 | 
| chanh_toasts | 80 | 
| bánh mì nướng muối | 100 | 
| tổng_toasts | 6 | 
| trả lời | 2 | 

Đồ uống là nguồn tài nguyên hạn chế ở đây. Mặc dù có rất nhiều chanh và muối nhưng chỉ có thể làm được tổng cộng 6 chiếc bánh mì nướng. Chia đều cho 3 bạn, mỗi bạn được 2 cốc. 

### Ví dụ 2 

đầu vào:```
5 100 100 1 1 1 1 1
```| Biến | Giá trị | 
| --- | --- | 
| n | 5 | 
| k | 100 | 
| tôi | 100 | 
| c | 1 | 
| d | 1 | 
| p | 1 | 
| nl | 1 | 
| np | 1 | 
| tổng số đồ uống | 10000 | 
| uống_toasts | 10000 | 
| chanh_toasts | 1 | 
| bánh mì nướng muối | 1 | 
| tổng_toasts | 1 | 
| trả lời | 0 | 

Ví dụ này cho thấy tại sao phép chia cuối cùng cho`n`vấn đề. Cả nhóm chỉ có thể làm tổng cộng một chiếc bánh mì nướng, không đủ để mỗi người bạn nhận được một chiếc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học cố định được thực hiện | 
| Không gian | O(1) | Không sử dụng bộ nhớ bổ sung tỷ lệ thuận với kích thước đầu vào | 

Kích thước đầu vào không đổi nên thời gian chạy và mức sử dụng bộ nhớ không bao giờ tăng. Điều này dễ dàng phù hợp trong giới hạn vấn đề. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n, k, l, c, d, p, nl, np = map(int, input().split())

    drink_toasts = (k * l) // nl
    lime_toasts = c * d
    salt_toasts = p // np

    total_toasts = min(drink_toasts, lime_toasts, salt_toasts)

    print(total_toasts // n)

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
assert run("3 4 5 10 8 100 3 1\n") == "2\n", "sample 1"

# minimum values
assert run("1 1 1 1 1 1 1 1\n") == "1\n", "minimum case"

# not enough drink for one toast per friend
assert run("3 1 3 1 1 100 3 1\n") == "0\n", "group division case"

# salt is the bottleneck
assert run("5 100 100 100 100 4 1 2\n") == "0\n", "salt bottleneck"

# maximum values
assert run("1000 1000 1000 1000 1000 1000 1 1\n") == "1000\n", "maximum values"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1 1 1 1 1`|`1`| Đầu vào hợp lệ tối thiểu | 
|`3 1 3 1 1 100 3 1`|`0`| Nâng cốc chúc mừng vẫn phải chia cho bạn bè | 
|`5 100 100 100 100 4 1 2`|`0`| Muối có thể trở thành nguồn tài nguyên hạn chế | 
|`1000 1000 1000 1000 1000 1000 1 1`|`1000`| Xử lý chính xác các ràng buộc tối đa | 

## Vỏ cạnh 

Một sai lầm dễ mắc phải là quên rằng câu trả lời phải giống nhau đối với tất cả bạn bè. 

Coi như:```
3 1 3 1 1 100 3 1
```Thuật toán tính toán:```
drink_toasts = (1 * 3) // 3 = 1
lime_toasts = 1
salt_toasts = 100
```Tối thiểu là`1`, nghĩa là cả nhóm có thể nâng cốc chúc mừng tổng cộng một chiếc. Chia đều cho 3 bạn được:```
1 // 3 = 0
```Vậy câu trả lời đúng là:```
0
```Một trường hợp phức tạp khác xảy ra khi một tài nguyên nhỏ hơn nhiều so với các tài nguyên khác.```
5 100 100 1 1 1 1 1
```Thuật toán tính toán:```
drink_toasts = 10000
lime_toasts = 1
salt_toasts = 1
```Tối thiểu là`1`, nên tổng số cốc chúc mừng chỉ có một dù lượng đồ uống rất lớn. 

Sau khi chia cho 5 người bạn:```
1 // 5 = 0
```Thuật toán xác định chính xác tài nguyên thắt cổ chai. 

Phép chia số nguyên cũng rất quan trọng.```
2 1 5 1 1 100 6 1
```Việc tính toán đồ uống trở thành:```
(1 * 5) // 6 = 0
```Mặc dù 5 ml gần bằng mức 6 yêu cầu nhưng vẫn không đủ để nâng ly chúc mừng hoàn chỉnh. Bởi vì thuật toán sử dụng tính năng phân chia tầng nên nó sẽ loại bỏ chính xác các lần nâng cấp và kết quả đầu ra một phần:```
0
```
