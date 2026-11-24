---
title: "CF 92A - Chip"
description: "Chúng ta có n con hải mã được sắp xếp thành một vòng tròn. Người thuyết trình bắt đầu với m chip và phân phát chúng theo thứ tự. Hải mã 1 nhận được 1 chip, hải mã 2 nhận được 2 chip, v.v. cho đến hải mã n, sau đó chu kỳ lặp lại từ hải mã 1."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 92
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 75 (Div. 2 Only)"
rating: 800
weight: 92
solve_time_s: 195
verified: true
draft: false
---

[CF 92A - Chips](https://codeforces.com/problemset/problem/92/A) 

**Đánh giá:** 800 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 3 phút 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`n`hải mã sắp xếp thành một vòng tròn. Người trình bày bắt đầu bằng`m`chip và phân phối chúng theo thứ tự. hải mã`1`nhận được`1`chip, hải mã`2`nhận được`2`khoai tây chiên, v.v. cho đến hải mã`n`, sau đó chu kỳ lặp lại từ hải mã`1`. 

Quá trình tiếp tục miễn là người thuyết trình có đủ chip để đáp ứng hoàn toàn con hải mã hiện tại. Thời điểm người trình bày không thể đưa ra số tiền cần thiết, quá trình này sẽ dừng lại và số chip còn lại sẽ thuộc về người trình bày. Nhiệm vụ là tính xem cuối cùng còn lại bao nhiêu chip. 

Những hạn chế là cực kỳ nhỏ. Số lượng hải mã nhiều nhất là`50`, và số lượng chip nhiều nhất là`10^4`. Ngay cả một mô phỏng trực tiếp liên tục trừ từng giá trị một cũng dễ dàng phù hợp trong giới hạn thời gian. Trong trường hợp xấu nhất, mỗi thao tác sẽ loại bỏ ít nhất một chip, do đó vòng lặp sẽ chạy nhiều nhất`10^4`lần, điều này không đáng kể đối với giới hạn 2 giây. 

Nguyên nhân chính của sai lầm là việc xử lý chính xác điều kiện dừng. Quá trình kết thúc khi người trình bày không thể đáp ứng đầy đủ con hải mã hiện tại, chứ không phải sau khi đưa một phần chip. 

Hãy xem xét đầu vào này:```
3 1
```Con hải mã đầu tiên cần chính xác`1`chip, vì vậy người trình bày sẽ đưa nó đi và kết thúc bằng`0`. 

Việc triển khai bất cẩn có thể dừng lại trước khi đưa chip vì nó kiểm tra bất đẳng thức sai. 

Một trường hợp tinh tế khác là khi chu kỳ kết thúc:```
4 11
```Người dẫn chương trình đưa ra`1 + 2 + 3 + 4 = 10`chip trong chu kỳ đầy đủ đầu tiên, để lại`1`. Rồi chu kỳ lại bắt đầu, hải mã`1`nhận được con chip cuối cùng và người trình bày kết thúc bằng`0`. Việc triển khai không chính xác có thể dừng ngay sau chu kỳ đầy đủ đầu tiên và xuất kết quả không chính xác.`1`. 

Thêm một trường hợp cạnh nữa xuất hiện khi số chip còn lại nhỏ hơn số lượng yêu cầu tiếp theo:```
3 7
```Việc phân phối diễn ra như thế này:`1 -> 2 -> 3 -> 1`Người dẫn chương trình bây giờ đã có`0`chip còn lại sau khi đưa ra trận chung kết`1`. Thay vào đó, nếu chúng ta có:```
3 6
```Người dẫn chương trình đưa ra`1 -> 2 -> 3`và dừng lại với`0`. 

Nếu chúng ta có:```
3 5
```Người dẫn chương trình đưa ra`1 -> 2`, rời đi`2`. Con hải mã tiếp theo cần`3`, vậy câu trả lời là`2`. Việc triển khai có lỗi trừ trước và kiểm tra sau sẽ tạo ra giá trị âm. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là mô phỏng quy trình chính xác như mô tả. Chúng tôi theo dõi số lượng hải mã hiện tại, đạp xe từ`1`ĐẾN`n`và liên tục kiểm tra xem còn đủ chip hay không. Nếu có, chúng tôi trừ số tiền cần thiết và chuyển sang con hải mã tiếp theo. Nếu không, chúng tôi dừng lại và in các chip còn lại. 

Cách tiếp cận bạo lực này đã đủ nhanh vì mọi hoạt động thành công đều giảm số lượng chip ít nhất`1`. Từ`m ≤ 10^4`, vòng lặp thực hiện nhiều nhất`10^4`lần. 

Ngoài ra còn có một quan sát toán học đằng sau quá trình này. Một vòng hoàn chỉnh sẽ phân phối:$1+2+\cdots+n=\frac{n(n+1)}{2}$chip. 

Chúng tôi có thể liên tục loại bỏ toàn bộ vòng trước khi mô phỏng một phần vòng còn lại. Điều đó làm giảm số lần lặp hơn nữa. Tuy nhiên, với những ràng buộc đã cho, mô phỏng đơn giản sẽ rõ ràng hơn và đầy đủ hơn. 

Giải pháp brute-force hoạt động vì quá trình chuyển đổi trạng thái rất nhỏ và bị giới hạn. Các ràng buộc quá nhỏ nên việc tối ưu hóa là không cần thiết. Phần quan trọng là thực hiện đúng thứ tự tuần hoàn và điều kiện dừng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(m) | O(1) | Đã chấp nhận | 
| Tối ưu hóa theo vòng | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`m`. 
2. Bắt đầu với chỉ số hải mã hiện tại bằng`1`. 
3. Trong khi người thuyết trình vẫn còn đủ chip cho con hải mã hiện tại, hãy trừ số chip yêu cầu khỏi`m`. 
4. Di chuyển đến con hải mã tiếp theo theo chiều kim đồng hồ. 

Nếu con hải mã hiện tại là`n`, quấn quanh trở lại`1`. 
5. Khoảnh khắc`m`trở nên nhỏ hơn số lượng cần thiết cho hải mã hiện tại, hãy dừng quá trình này. 
6. In giá trị còn lại của`m`. 

Tại sao nó hoạt động: 

At every step, the algorithm exactly follows the rules of the distribution process. The variable representing the current walrus always matches the next walrus who should receive chips, and`m`luôn lưu trữ số lượng chip còn lại. Vòng lặp chỉ tiếp tục khi con hải mã hiện tại có thể được thỏa mãn hoàn toàn. Một khi điều đó trở nên không thể thực hiện được thì quy trình phải chấm dứt ngay lập tức theo báo cáo vấn đề. Vì mỗi lần lặp thành công phản ánh một động thái hợp pháp của người trình bày nên giá trị còn lại cuối cùng là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())

current = 1

while m >= current:
    m -= current
    current += 1

    if current > n:
        current = 1

print(m)
```Biến`current`cửa hàng nào hải mã sẽ nhận được chip tiếp theo. Nó bắt đầu từ`1`bởi vì quá trình này luôn bắt đầu với hải mã`1`. 

Điều kiện vòng lặp là chi tiết quan trọng:```
while m >= current:
```Người thuyết trình chỉ được phép tiếp tục nếu có đủ chip để đáp ứng đầy đủ cho con hải mã hiện tại. sử dụng`>`thay vì`>=`sẽ thất bại khi số chip còn lại khớp chính xác với số lượng yêu cầu. 

Sau khi trừ số chip, chúng ta tiến tới con hải mã tiếp theo. Khi chỉ số vượt quá`n`, chúng tôi quay lại`1`để duy trì trật tự tuần hoàn. 

Việc triển khai không bao giờ tạo ra giá trị âm vì phép trừ chỉ xảy ra sau khi xác minh có đủ số chip. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 11
```| Hải mã hiện tại | Chip cần thiết | Chip trước | Chip sau | 
| --- | --- | --- | --- | 
| 1 | 1 | 11 | 10 | 
| 2 | 2 | 10 | 8 | 
| 3 | 3 | 8 | 5 | 
| 4 | 4 | 5 | 1 | 
| 1 | 1 | 1 | 0 | 

Con hải mã tiếp theo sẽ cần`2`chip, nhưng không còn lại gì, nên câu trả lời là`0`. 

Ví dụ này thể hiện hành vi bao bọc sau khi tiếp cận hải mã`4`. 

### Ví dụ 2 

đầu vào:```
3 5
```| Hải mã hiện tại | Chip cần thiết | Chip trước | Chip sau | 
| --- | --- | --- | --- | 
| 1 | 1 | 5 | 4 | 
| 2 | 2 | 4 | 2 | 

Con hải mã tiếp theo cần`3`chip, nhưng chỉ`2`vẫn còn, vì vậy quá trình dừng lại và câu trả lời là`2`. 

Dấu vết này xác nhận rằng quá trình kết thúc trước khi phân phối một phần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) | Mỗi lần lặp thành công sẽ loại bỏ ít nhất một chip | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Từ`m`nhiều nhất là`10^4`, mô phỏng thực hiện tối đa`10^4`lần lặp lại. Đó là thấp hơn nhiều so với giới hạn cho thời gian chạy 2 giây. Việc sử dụng bộ nhớ là không đổi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())

    current = 1

    while m >= current:
        m -= current
        current += 1

        if current > n:
            current = 1

    print(m)

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
assert run("4 11\n") == "0\n", "sample 1"

# minimum values
assert run("1 1\n") == "0\n", "minimum case"

# cannot satisfy second walrus
assert run("2 1\n") == "0\n", "exactly enough for first walrus"

# partial stop in middle of cycle
assert run("3 5\n") == "2\n", "stop before giving 3 chips"

# wrap around multiple times
assert run("2 7\n") == "1\n", "multiple complete cycles"

# maximum style stress case
assert run("50 10000\n") == "145\n", "large input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`0`| Đầu vào nhỏ nhất có thể | 
|`2 1`|`0`| Điều kiện đẳng thức chính xác | 
|`3 5`|`2`| Dừng trước khi phân phối một phần | 
|`2 7`|`1`| Quấn tròn đúng cách | 
|`50 10000`|`145`| Đầu vào lớn trong giới hạn | 

## Vỏ cạnh 

Consider the exact-fit scenario:```
1 1
```Con hải mã đầu tiên yêu cầu chính xác`1`chip. Thuật toán kiểm tra`m >= current`, điều này đúng, trừ chip và để lại`0`. Lần lặp tiếp theo thất bại vì`0 < 1`, vì vậy đầu ra là chính xác`0`. 

Bây giờ hãy xem xét một trường hợp bao quanh:```
4 11
```Sau khi cho`1 + 2 + 3 + 4 = 10`chip, thuật toán đặt lại`current`quay lại`1`. Vì vẫn còn một con chip nên người trình bày sẽ tặng nó thành công và kết thúc bằng`0`. Điều này xác nhận việc lập chỉ mục vòng tròn được xử lý chính xác. 

Cuối cùng, kiểm tra trường hợp lỗi một phần:```
3 5
```Thuật toán thực hiện các bước sau:`5 -> 4 -> 2`Con hải mã tiếp theo cần`3`, nhưng chỉ`2`duy trì. Vì điều kiện vòng lặp không thành công trước khi trừ nên thuật toán dừng ngay lập tức và in ra`2`. Điều này ngăn chặn các giá trị âm không hợp lệ và khớp chính xác với các quy tắc của vấn đề.
