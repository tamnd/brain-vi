---
title: "CF 2137A - Giả thuyết Collatz"
description: "Chúng ta nhận được kết quả khi chạy thao tác Collatz chính xác k lần và giá trị cuối cùng là x. Nhiệm vụ không phải là tìm giá trị ban đầu duy nhất, vì nhiều giá trị ban đầu khác nhau có thể dẫn đến cùng một kết quả."
date: "2026-06-08T02:30:25+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 2137
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1047 (Div. 3)"
rating: 800
weight: 2137
solve_time_s: 119
verified: false
draft: false
---

[CF 2137A - Phỏng đoán Collatz](https://codeforces.com/problemset/problem/2137/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, toán học 
**Thời gian giải:** 1 phút 59s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được kết quả chạy thao tác Collatz một cách chính xác`k`lần và giá trị cuối cùng`x`. Nhiệm vụ không phải là tìm giá trị ban đầu duy nhất, vì nhiều giá trị ban đầu khác nhau có thể dẫn đến cùng một kết quả. Chúng ta chỉ cần xây dựng bất kỳ số nguyên bắt đầu nào tạo ra giá trị cuối cùng đã cho sau khi chính xác`k`hoạt động. 

Hoạt động rất đơn giản. Nếu số hiện tại là số chẵn thì số tiếp theo sẽ trở thành`x / 2`. Nếu số hiện tại là số lẻ thì số tiếp theo sẽ trở thành`3x + 1`. 

Những hạn chế là cực kỳ nhỏ. Cả hai`k`Và`x`nhiều nhất là`20`, và có nhiều nhất`400`trường hợp thử nghiệm. Ngay cả các công trình xây dựng theo cấp số nhân cũng sẽ dễ dàng phù hợp. Điều này cho thấy giải pháp mong muốn không phải là tối ưu hóa mà là hiểu cách đảo ngược quá trình. 

Khó khăn chính là phép toán Collatz không phải lúc nào cũng có thể nghịch đảo được. Ví dụ: nếu chúng ta biết giá trị hiện tại là`4`, giá trị trước đó có thể là`8`bởi vì`8 → 4`thông qua quy tắc chẵn, hoặc nó có thể đã được`1`bởi vì`1 → 4`thông qua quy tắc kỳ lạ. 

Một cấu trúc đảo ngược bất cẩn có thể cố gắng sử dụng tiền lẻ bất cứ khi nào có thể. Coi như:```
k = 1, x = 5
```phương trình`(5 - 1) / 3`không phải là số nguyên nên không có số lẻ đứng trước. Câu trả lời đúng là`10`, từ`10 → 5`. 

Một cái bẫy khác là quên rằng chúng ta chỉ cần một câu trả lời hợp lệ. Vì```
k = 1, x = 4
```cả hai`1`Và`8`là những người tiền nhiệm hợp lệ. Tìm kiếm tất cả các khả năng là không cần thiết. 

Trường hợp tinh tế thứ ba xảy ra khi liên tục đảo ngược nhiều bước. Vì```
k = 5, x = 4
```một bản tái thiết hợp lệ là```
4 ← 8 ← 16 ← 32 ← 64 ← 21
```đưa ra câu trả lời`21`. Chiến lược đảo ngược phải đảm bảo rằng mỗi bước lùi tương ứng với một bước di chuyển Collatz hợp lệ về phía trước. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là vũ lực. Chúng ta có thể thử mọi số bắt đầu có thể và mô phỏng`k`Collatz cho đến khi tìm được giá trị cuối cùng bằng giá trị đã cho`x`. 

Điều này hiệu quả vì các ràng buộc rất nhỏ nhưng không có giới hạn trên rõ ràng về giá trị bắt đầu chính xác có thể lớn đến mức nào. Ngay cả đối với nhỏ`k`, nhiều ứng viên sẽ cần phải được kiểm tra. Cách tiếp cận này không có sự đảm bảo rõ ràng. 

Quan sát quan trọng là chúng ta có thể đảo ngược quá trình. 

Giả sử giá trị hiện tại là`y`. 

Nếu giá trị trước đó là số chẵn thì bước chuyển tiếp sẽ chia giá trị đó cho hai:```
previous = 2y
```Người tiền nhiệm này luôn tồn tại. 

Nếu giá trị trước đó là số lẻ thì:```
3 · previous + 1 = y
```mang lại```
previous = (y - 1) / 3
```nhưng điều này chỉ đúng khi`(y - 1)`chia hết cho`3`và kết quả thật kỳ lạ. 

Thực tế quan trọng là chúng ta không cần mọi người đi trước. Chúng tôi chỉ cần một người tiền nhiệm hợp lệ ở mỗi bước. Vì việc nhân đôi luôn tạo ra một số tiền trước hợp lệ nên chúng ta có thể áp dụng nhiều lần:```
y ← 2y
```chính xác`k`lần. 

Sau khi đảo ngược`k`bước theo cách này thì số thu được là:```
x · 2^k
```Chạy quy trình Collatz từ con số này sẽ thực hiện`k`chia liên tiếp cho hai và trả về chính xác cho`x`. 

Điều này mang lại một giải pháp mang tính xây dựng ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Tìm kiếm không giới hạn | O(1) | Không thực tế | 
| Tối ưu | O(k) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`k`Và`x`. 
2. Xử lý giá trị đã cho`x`làm trạng thái sau khi tất cả các thao tác đã được thực hiện. 
3. Đảo ngược một bước Collatz bằng cách chọn người tiền nhiệm được đảm bảo`2x`. 

Điều này luôn hợp lệ vì mọi số chẵn đều ánh xạ tới một nửa giá trị của nó trong quá trình chuyển tiếp. 
4. Lặp lại chính xác bước trước đó`k`lần. 

Sau một bước đảo ngược, giá trị sẽ trở thành`2x`, sau hai bước nó trở thành`4x`, và sau`k`các bước nó trở thành`x · 2^k`. 
5. Xuất ra số kết quả. 

### Tại sao nó hoạt động 

Mỗi bước đảo ngược tạo nên một số mà bước di chuyển về phía trước của Collatz là phép chia cho hai. Nếu chúng ta bắt đầu từ`x · 2^k`, thao tác đầu tiên tạo ra`x · 2^(k-1)`, thứ hai tạo ra`x · 2^(k-2)`, vân vân. Sau chính xác`k`hoạt động chúng tôi đến`x`. 

Vì mọi giá trị trung gian đều là số chẵn nên quy trình Collatz tuân theo quy tắc chia hai ở mỗi bước, làm cho việc tái cấu trúc trở nên hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    k, x = map(int, input().split())
    print(x * (1 << k))
```Giải pháp trực tiếp thực hiện việc xây dựng ngược lại. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi cần một số có số tiếp theo`k`Hoạt động của Collatz đều là phép chia cho hai. nhân với`2^k`tạo ra chính xác một con số như vậy. 

biểu hiện`1 << k`tính toán`2^k`sử dụng hiệu quả dịch chuyển bit. Từ`k ≤ 20`, kết quả rất nhỏ và dễ dàng khớp với các số nguyên Python. 

Không cần mô phỏng. Chúng tôi xây dựng câu trả lời ngay lập tức và in nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
k = 1
x = 4
```| Bước | Giá trị hiện tại | 
| --- | --- | 
| Giá trị cuối cùng được đưa ra | 4 | 
| Đảo ngược 1 bước | 8 | 

Đầu ra:```
8
```Chuyển tiếp xác minh: 

| Hoạt động | Giá trị | 
| --- | --- | 
| Bắt đầu | 8 | 
| Chia cho 2 | 4 | 

Sau một thao tác, chúng ta thu được giá trị cuối cùng cần thiết. 

### Ví dụ 2 

đầu vào:```
k = 5
x = 4
```| Bước | Giá trị hiện tại | 
| --- | --- | 
| Giá trị cuối cùng được đưa ra | 4 | 
| Đảo ngược 1 bước | 8 | 
| Đảo ngược 2 bước | 16 | 
| Đảo ngược 3 bước | 32 | 
| Đảo ngược 4 bước | 64 | 
| Đảo ngược 5 bước | 128 | 

Đầu ra:```
128
```Chuyển tiếp xác minh: 

| Hoạt động | Giá trị | 
| --- | --- | 
| Bắt đầu | 128 | 
| 1 | 64 | 
| 2 | 32 | 
| 3 | 16 | 
| 4 | 8 | 
| 5 | 4 | 

Dấu vết này cho thấy sự bất biến một cách rõ ràng. Mỗi bước là một số chẵn, vì vậy mỗi thao tác Collatz buộc phải chia cho hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Một phép nhân cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Chỉ có một vài biến được sử dụng | 

Những hạn chế là rất nhỏ nhưng giải pháp này thậm chí còn mạnh mẽ hơn. Mỗi trường hợp thử nghiệm yêu cầu thời gian không đổi và bộ nhớ không đổi, dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    ans = []

    for _ in range(t):
        k, x = map(int, input().split())
        ans.append(str(x * (1 << k)))

    return "\n".join(ans) + "\n"

# provided sample format (answers need not match statement exactly)
assert run("3\n1 4\n1 5\n5 4\n") == "8\n10\n128\n"

# minimum values
assert run("1\n1 1\n") == "2\n"

# largest values
assert run("1\n20 20\n") == f"{20 * (1 << 20)}\n"

# x = 1 with several reverse steps
assert run("1\n5 1\n") == "32\n"

# multiple test cases
assert run("2\n2 3\n3 7\n") == "12\n56\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 1`|`2`| Trường hợp hợp lệ nhỏ nhất | 
|`1 / 20 20`|`20971520`| Giá trị ràng buộc lớn nhất | 
|`1 / 5 1`|`32`| Xây dựng ngược lặp đi lặp lại | 
|`2 / (2,3) (3,7)`|`12, 56`| Nhiều trường hợp thử nghiệm độc lập | 

## Vỏ cạnh 

Hãy xem xét:```
1
1 5
```giá trị`5`không có tiền thân lẻ hợp lệ vì`(5 - 1) / 3`không phải là số nguyên lẻ. Thuật toán không bao giờ dựa vào các thuật toán tiền nhiệm lẻ. Nó chỉ đơn giản là nhân đôi một lần và quay trở lại`10`. Chạy một bước Collatz mang lại`10 → 5`, vậy câu trả lời là hợp lệ. 

Coi như:```
1
1 4
```Có nhiều tiền thân hợp lệ, bao gồm`1`Và`8`. Thuật toán chọn`8`bởi vì nhân đôi luôn có tác dụng. Chạy một bước sẽ mang lại`8 → 4`. 

Coi như:```
1
20 20
```Thuật toán trả về:```
20 × 2^20 = 20971520
```Tất cả hai mươi phép tính Collatz đều được chia cho hai:```
20971520 → 10485760 → ... → 20
```Việc xây dựng vẫn có giá trị ngay cả ở các giá trị lớn nhất cho phép. 

Coi như:```
1
5 1
```Các đầu ra thuật toán:```
32
```Trình tự chuyển tiếp là:```
32 → 16 → 8 → 4 → 2 → 1
```Sau đúng năm thao tác, kết quả là giá trị cuối cùng được yêu cầu`1`, xác nhận rằng việc nhân đôi lặp lại một cách chính xác sẽ tái tạo lại số bắt đầu hợp lệ.
