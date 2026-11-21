---
title: "CF 231A - Đội"
description: "Ba người bạn đánh giá từng bài thi một cách độc lập. Đối với mọi vấn đề, chúng ta có ba giá trị, mỗi giá trị là 0 hoặc 1. Giá trị 1 có nghĩa là người bạn đó tự tin rằng họ biết cách giải quyết vấn đề. Giá trị 0 có nghĩa là họ không tự tin."
date: "2026-06-04T09:23:48+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy"]
categories: ["algorithms"]
codeforces_contest: 231
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 143 (Div. 2)"
rating: 800
weight: 231
solve_time_s: 322
verified: false
draft: false
---

[CF 231A - Nhóm](https://codeforces.com/problemset/problem/231/A) 

**Đánh giá:** 800 
**Tags:** vũ phu, tham lam 
**Thời gian giải:** 5 phút 22s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Ba người bạn đánh giá từng bài thi một cách độc lập. Đối với mọi vấn đề, chúng ta có ba giá trị, mỗi giá trị`0`hoặc`1`. 

Một giá trị của`1`có nghĩa là người bạn đó tự tin rằng họ biết cách giải quyết vấn đề. Một giá trị của`0`có nghĩa là họ không tự tin. 

Đội có một quy tắc đơn giản: họ sẽ chỉ thử giải một bài toán nếu có ít nhất hai trong số ba người bạn tự tin. Nhiệm vụ của chúng ta là đếm xem có bao nhiêu bài toán thỏa mãn quy tắc đó. 

Đầu vào bắt đầu bằng`n`, số lượng vấn đề. Sau đó làm theo`n`mỗi dòng chứa ba giá trị nhị phân mô tả ý kiến ​​của Petya, Vasya và Tonya về một vấn đề. Đầu ra là một số nguyên duy nhất, số lượng bài toán mà nhóm sẽ giải được. 

Những hạn chế là rất nhỏ. Có nhiều nhất 1000 bài toán, mỗi bài chỉ chứa ba số. Ngay cả một giải pháp đơn giản xử lý mọi vấn đề một cách độc lập cũng chỉ thực hiện được vài nghìn thao tác. Bất kỳ giải pháp thời gian tuyến tính nào cũng đủ nhanh trong giới hạn 2 giây. 

Một lỗi phổ biến là kiểm tra xem cả ba người bạn có tự tin hay không thay vì ít nhất hai người. 

Ví dụ:```
1
1 1 0
```Đầu ra đúng là:```
1
```Hai người bạn đồng ý nên vấn đề nên được tính. Việc thực hiện bất cẩn bằng cách sử dụng`sum == 3`sẽ xuất ra không chính xác`0`. 

Một sai lầm khác là tính toán các bài toán khi có ít nhất một người bạn tự tin. 

Ví dụ:```
1
1 0 0
```Đầu ra đúng là:```
0
```Chỉ có một người bạn tự tin, điều này không thỏa mãn quy tắc của đội. 

Trường hợp cuối cùng là khi có chính xác hai người bạn tự tin.```
1
0 1 1
```Đầu ra đúng là:```
1
```Điều kiện là "ít nhất hai" chứ không phải "nhiều hơn hai". 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là kiểm tra từng vấn đề một cách riêng biệt. Đối với một bài toán nhất định, chúng ta cộng ba giá trị nhị phân. Tổng kết quả cho chúng ta biết có bao nhiêu người bạn tự tin. 

Nếu tổng ít nhất là 2, chúng tôi sẽ tăng câu trả lời của mình. 

Cách tiếp cận bạo lực này đã đủ vì chỉ có 1000 vấn đề. Trường hợp xấu nhất thực hiện khoảng 3000 lần đọc số nguyên và 1000 phép cộng, điều này không đáng kể. 

Quan sát chính là các giá trị đầu vào là nhị phân. Tổng của ba số chính xác là số bạn tự tin. Điều đó có nghĩa là chúng ta không cần bất kỳ logic phức tạp nào hoặc kiểm tra theo cặp. Một sự bổ sung duy nhất ngay lập tức cho chúng ta biết liệu nhóm có giải quyết được vấn đề hay không. 

Vì mọi vấn đề đều được xử lý độc lập và chính xác một lần nên giải pháp tối ưu là quét tuyến tính đơn giản thông qua đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

Đối với vấn đề này, các phương pháp tiếp cận mạnh mẽ và tối ưu thực sự giống nhau vì các ràng buộc quá nhỏ và quá trình quét trực tiếp đã là tối ưu. 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`, số lượng vấn đề. 
2. Khởi tạo`answer = 0`. 
3. Đối với mỗi`n`vấn đề, đọc ba giá trị nhị phân. 
4. Tính tổng của chúng. Tổng bằng số bạn bè tự tin về vấn đề đó. 
5. Nếu tổng ít nhất`2`, tăng`answer`. 

Điều này phù hợp với quy tắc của nhóm là ít nhất hai người bạn phải chắc chắn về giải pháp. 
6. Sau khi xử lý xong mọi vấn đề, hãy in`answer`. 

### Tại sao nó hoạt động 

Với mọi vấn đề, mỗi người bạn tự tin đều đóng góp chính xác`1`đến tổng số. Vì chỉ có ba người bạn nên tổng chính xác là số thành viên tự tin trong nhóm. 

Nhóm giải quyết được vấn đề khi và chỉ khi có ít nhất hai người bạn tự tin. Kiểm tra xem tổng có nhỏ nhất không`2`do đó hoàn toàn tương đương với việc kiểm tra điều kiện chấp nhận của bài toán. Vì chúng tôi áp dụng bài kiểm tra này cho mọi vấn đề và đếm tất cả những vấn đề thành công nên câu trả lời cuối cùng là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
answer = 0

for _ in range(n):
    a, b, c = map(int, input().split())
    if a + b + c >= 2:
        answer += 1

print(answer)
```Đầu tiên chương trình sẽ đọc số lượng bài toán. 

Đối với mỗi vấn đề, nó đọc ba giá trị nhị phân và tính tổng của chúng. Bởi vì các giá trị chỉ`0`hoặc`1`, tổng thể hiện trực tiếp số lượng bạn bè tự tin. 

Bất cứ khi nào tổng ít nhất là`2`, bộ đếm tăng lên. Sau khi tất cả các vấn đề đã được xử lý, bộ đếm sẽ chứa chính xác số vấn đề mà nhóm sẽ thử. 

Không có điều kiện biên phức tạp. Điều kiện phải là`>= 2`, không`== 2`, vì một vấn đề với cả ba người bạn tự tin cũng nên được tính. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
1 1 0
1 1 1
1 0 0
```| Vấn đề | Giá trị | Tổng hợp | Được tính? | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 1 0 | 2 | Có | 1 | 
| 2 | 1 1 1 | 3 | Có | 2 | 
| 3 | 1 0 0 | 1 | Không | 2 | 

Đầu ra cuối cùng:```
2
```Ví dụ này cho thấy cả hai tình huống được chấp nhận: chính xác là hai người bạn tự tin và cả ba người bạn tự tin. 

### Mẫu 2 

đầu vào:```
2
1 0 0
0 1 1
```| Vấn đề | Giá trị | Tổng hợp | Được tính? | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 0 0 | 1 | Không | 0 | 
| 2 | 0 1 1 | 2 | Có | 1 | 

Đầu ra cuối cùng:```
1
```Ví dụ này thể hiện hành vi ngưỡng. Một người bạn tự tin là không đủ, trong khi hai người bạn là đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vấn đề được xử lý đúng một lần | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Với tối đa 1000 vấn đề, quá trình quét tuyến tính thực hiện một số lượng nhỏ thao tác và dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n = int(input())
    answer = 0

    for _ in range(n):
        a, b, c = map(int, input().split())
        if a + b + c >= 2:
            answer += 1

    return str(answer)

# provided sample
assert run("3\n1 1 0\n1 1 1\n1 0 0\n") == "2", "sample 1"

# custom cases
assert run("1\n0 0 0\n") == "0", "minimum input, nobody confident"

assert run("1\n1 1 1\n") == "1", "all friends confident"

assert run("1\n0 1 1\n") == "1", "exactly two friends confident"

assert run("4\n1 0 0\n0 1 0\n0 0 1\n1 1 0\n") == "1", "only last problem qualifies"

assert run(
    "5\n1 1 0\n0 1 1\n1 0 1\n1 1 1\n0 0 0\n"
) == "4", "mixed cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0 0 0`|`0`| Đầu vào kích thước tối thiểu | 
|`1 / 1 1 1`|`1`| Cả ba người bạn đều tự tin | 
|`1 / 0 1 1`|`1`| Chính xác là hai người bạn tự tin | 
| Bốn vấn đề hỗn hợp |`1`| Đếm đúng trên nhiều hàng | 
| Năm vấn đề đa dạng |`4`| Tính đúng đắn chung của đầu vào hỗn hợp | 

## Vỏ cạnh 

Hãy xem xét trường hợp có đúng hai người bạn tự tin:```
1
1 1 0
```Thuật toán tính toán:```
sum = 1 + 1 + 0 = 2
```Từ`2 >= 2`, câu trả lời trở thành`1`. Điều này đúng vì nội quy của đội yêu cầu ít nhất hai thành viên tự tin. 

Bây giờ hãy xem xét một vấn đề trong đó chỉ có một người bạn tự tin:```
1
1 0 0
```Thuật toán tính toán:```
sum = 1
```Từ`1 < 2`, câu trả lời vẫn còn`0`. Nhóm không có đủ sự đồng ý để giải quyết vấn đề. 

Cuối cùng, hãy coi cả ba người bạn đều tự tin:```
1
1 1 1
```Thuật toán tính toán:```
sum = 3
```Từ`3 >= 2`, vấn đề được tính. Điều này xác nhận rằng điều kiện là "ít nhất hai" chứ không phải "chính xác là hai".
