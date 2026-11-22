---
title: "CF 39A - Tính toán C*++"
description: "Chúng ta được cho một biểu thức số học được xây dựng từ các số hạng chứa một biến duy nhất a. Mỗi thuật ngữ là một trong hai dạng:"
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "expression-parsing", "greedy"]
categories: ["algorithms"]
codeforces_contest: 39
codeforces_index: "A"
codeforces_contest_name: "School Team Contest 1 (Winter Computer School 2010/11)"
rating: 2000
weight: 39
solve_time_s: 162
verified: true
draft: false
---
[CF 39A - Phép tính C*++](https://codeforces.com/problemset/problem/39/A) 

**Xếp hạng:** 2000 
**Tas:** phân tích biểu thức, tham lam 
**Thời gian giải:** 2m 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một biểu thức số học được xây dựng từ các thuật ngữ liên quan đến một biến duy nhất`a`. Mỗi thuật ngữ là một trong hai dạng:```
k * a++
k * ++a
```Ở đâu`k`là hệ số nguyên giữa`0`Và`1000`. Nếu thiếu hệ số thì được coi là`1`. 

Toàn bộ biểu thức là tổng và trừ các số hạng đó. Phần bất thường là thứ tự đánh giá. Ngôn ngữ cho phép các thuật ngữ được đánh giá theo bất kỳ thứ tự nào trước khi kết hợp chúng với`+`Và`-`. 

Mỗi thuật ngữ thay đổi giá trị của`a`. 

Vì`a++`, giá trị hiện tại của`a`được sử dụng đầu tiên, sau đó`a`tăng lên bởi`1`. 

Vì`++a`,`a`tăng đầu tiên, sau đó giá trị mới được sử dụng. 

Nhiệm vụ là chọn thứ tự đánh giá sao cho giá trị cuối cùng của toàn bộ biểu thức là lớn nhất. 

Số lần triệu tập nhiều nhất là`1000`, vì vậy bất kỳ thuật toán nào chậm hơn nhiều so với thuật toán bậc hai đều không cần thiết. Việc thử mọi hoán vị là không thể bởi vì`1000!`có kích thước lớn về mặt thiên văn. Dù chỉ dành cho`15`Nói cách khác, vũ lực đã trở nên không thể thực hiện được. 

Bản thân biểu thức đủ nhỏ để việc phân tích cú pháp trực tiếp bằng quét tuyến tính trở nên đơn giản. Chúng tôi chỉ cần trích xuất, cho mỗi lần triệu tập:```
sign: +1 or -1
coefficient: k
type: post-increment or pre-increment
```Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
0
a++-++a
```Nếu chúng ta đánh giá`a++`Đầu tiên:```
0 - 2 = -2
```Nếu chúng ta đánh giá`++a`Đầu tiên:```
1 - 1 = 0
```Tối đa là`0`. Một cách tiếp cận ngây thơ chỉ đếm tổng số gia tăng mà không xem xét thứ tự sẽ làm mất đi sự khác biệt này. 

Một trường hợp tế nhị khác là hệ số`0`. 

đầu vào:```
5
0*a++-0*++a
```Cả hai điều khoản đều đóng góp`0`về mặt số lượng, nhưng họ vẫn sửa đổi`a`. Thuật ngữ đầu tiên tăng sau khi đọc`a`, số thứ hai tăng trước khi đọc nó. Thứ tự vẫn quan trọng mặc dù hệ số bằng 0. 

Cái bẫy thứ ba là quên rằng phép trừ làm đảo lộn các ưu tiên. 

đầu vào:```
1
10*a++-1*++a
```Để tối đa hóa kết quả, hệ số dương lớn sẽ có giá trị lớn là`a`, trong khi số hạng phủ định sẽ có giá trị nhỏ. Đánh giá thuật ngữ phủ định đầu tiên là tốt hơn. 

Một người tham lam bất cẩn chỉ sắp xếp theo hệ số lớn mà không xét dấu sẽ cho kết quả sai. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu là trực tiếp. Phân tích tất cả các lệnh triệu tập, tạo mọi thứ tự đánh giá có thể có, mô phỏng biểu thức và giữ kết quả tối đa. 

Đối với mỗi hoán vị, chúng tôi duy trì giá trị hiện tại của`a`. Khi đánh giá một thuật ngữ: 

cho`k * a++`, chúng tôi thêm`k * a`đến kết quả, sau đó tăng`a`. 

Vì`k * ++a`, chúng tôi tăng`a`đầu tiên, sau đó thêm`k * a`. 

Sau khi tất cả các điều khoản được đánh giá, chúng tôi kết hợp chúng với bản gốc`+`Và`-`dấu hiệu. 

Điều này hiệu quả vì ngữ nghĩa của ngôn ngữ được mô tả chính xác bằng cách đánh giá tuần tự. Vấn đề là số lượng hoán vị. Với`n`triệu tập, chúng tôi cần`n!`đơn đặt hàng. Thậm chí`12!`đã về rồi`4.8 * 10^8`, vượt xa giới hạn. 

Quan sát quan trọng là mọi số hạng đều tăng`a`đúng một lần. Câu hỏi duy nhất là số hạng nào sẽ nhận giá trị nhỏ hơn của`a`, và cái nào sẽ nhận được cái lớn hơn. 

Chúng ta hãy viết lại từng thuật ngữ một cách cẩn thận. 

Giả sử hiện tại`a = x`. 

Đối với một thuật ngữ tích cực:```
+k * a++  =>  +k * x
+k * ++a  =>  +k * (x + 1)
```Đối với từ phủ định:```
-k * a++  =>  -k * x
-k * ++a  =>  -k * (x + 1)
```Mọi số hạng đều phụ thuộc tuyến tính vào giá trị hiện tại của`a`. 

Nếu một thuật ngữ có hệ số hiệu quả lớn hơn trên`x`, chúng tôi muốn nó được đánh giá sau khi`a`trở nên lớn hơn. Nếu nó có hệ số nhỏ hơn, chúng tôi muốn nó sớm hơn. 

Điều này trở thành một đối số hoán đổi theo cặp cổ điển. 

Lấy hai số hạng với hệ số hiệu dụng`c1`Và`c2`. 

Nếu chúng ta đánh giá thuật ngữ 1 trước thuật ngữ 2:```
c1 * x + c2 * (x + 1)
```Nếu chúng ta trao đổi chúng:```
c2 * x + c1 * (x + 1)
```Thứ tự thứ hai tốt hơn chính xác khi:```
c1 > c2
```Vì vậy, các số hạng nên được xử lý theo thứ tự không giảm của hệ số hiệu dụng. 

Bây giờ chúng ta chỉ cần xác định hệ số hiệu quả cho mỗi lệnh triệu tập. 

Vì:```
+k * something
```hệ số hiệu dụng là`+k`. 

Vì:```
-k * something
```hệ số hiệu dụng là`-k`. 

các`++a`so với`a++`sự khác biệt chỉ đóng góp một phần bù không đổi. Cụ thể,`++a`thêm một bản sao bổ sung của hệ số đã ký vì nó tăng trước khi đọc. 

Sau khi sắp xếp, chúng tôi mô phỏng đánh giá theo thứ tự đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích biểu thức thành các lệnh riêng lẻ. 

Đối với mỗi triệu tập, trích xuất: 

- dấu hiệu của nó,`+1`hoặc`-1`- hệ số của nó`k`- liệu có phải vậy không`a++`hoặc`++a`2. Với mỗi lệnh triệu tập, hãy tính hệ số hiệu dụng của lệnh đó. 

Một thuật ngữ tích cực góp phần`+k * current_a`. 

Một thuật ngữ phủ định góp phần`-k * current_a`. 

Cửa hàng:```
effective = sign * k
```3. Sắp xếp tất cả các lệnh triệu tập theo`effective`theo thứ tự không giảm. 

Các hệ số nhỏ hơn sẽ tiêu thụ các giá trị nhỏ hơn của`a`. Hệ số lớn hơn sẽ tiêu thụ giá trị lớn hơn của`a`. 
4. Mô phỏng đánh giá theo thứ tự sắp xếp. 

Duy trì: 

- giá trị hiện tại của`a`- câu trả lời tích lũy 
5. Khi xử lý`a++`:

Thêm vào:```
sign * k * a
```sau đó tăng lên`a`. 
6. Khi xử lý`++a`: 

Tăng`a`đầu tiên, sau đó thêm:```
sign * k * a
```7. Xuất giá trị tích lũy cuối cùng. 

### Tại sao nó hoạt động 

Mỗi học kỳ tăng`a`chính xác một lần, vì vậy sau khi đánh giá`t`điều khoản, giá trị hiện tại của`a`được cố định bất kể thứ tự. 

Quyền tự do duy nhất là quyết định thuật ngữ nào nhận được giai đoạn nào của`a`. 

Xem xét hai thuật ngữ lân cận với hệ số hiệu quả`c1`Và`c2`. 

Nếu hiện tại`a = x`, đánh giá chúng như`(1,2)`mang lại:```
c1*x + c2*(x+1)
```Hoán đổi chúng mang lại:```
c2*x + c1*(x+1)
```Biểu thức thứ hai lớn hơn chính xác khi`c1 > c2`. 

Vì vậy, bất kỳ nghịch đảo nào mà hệ số lớn hơn xuất hiện sớm hơn đều có thể được cải thiện bằng cách hoán đổi. Việc lặp lại đối số này sẽ chuyển đổi mọi thứ tự tối ưu thành thứ tự được sắp xếp. 

Điều đó chứng tỏ trật tự tham lam là tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a = int(input())
    s = input().strip()

    terms = []

    i = 0
    sign = 1

    while i < len(s):
        if s[i] == '+':
            sign = 1
            i += 1
        elif s[i] == '-':
            sign = -1
            i += 1

        coeff = 0
        has_coeff = False

        while i < len(s) and s[i].isdigit():
            coeff = coeff * 10 + int(s[i])
            has_coeff = True
            i += 1

        if has_coeff:
            i += 1  # skip '*'
        else:
            coeff = 1

        if s[i:i + 3] == "++a":
            typ = 1
            i += 3
        else:
            typ = 0
            i += 3

        effective = sign * coeff
        terms.append((effective, sign, coeff, typ))

    terms.sort(key=lambda x: x[0])

    ans = 0
    cur = a

    for _, sign, coeff, typ in terms:
        if typ == 1:
            cur += 1
            ans += sign * coeff * cur
        else:
            ans += sign * coeff * cur
            cur += 1

    print(ans)

solve()
```Trình phân tích cú pháp quét biểu thức từ trái sang phải. Mỗi lần lặp trích xuất chính xác một lệnh triệu hồi. 

Việc phân tích hệ số cần được quan tâm đặc biệt vì các hệ số có thể bị bỏ qua. Nếu không tìm thấy chữ số nào thì hệ số phải trở thành`1`. 

Sự khác biệt giữa`++a`Và`a++`được đại diện với`typ`.```
typ = 1  -> ++a
typ = 0  -> a++
```Khóa sắp xếp chỉ là hệ số hiệu quả:```
sign * coeff
```Đó là đại lượng kiểm soát xem một thuật ngữ thích các giá trị sớm hơn hay muộn hơn của`a`. 

Trong quá trình mô phỏng, thứ tự tăng dần rất quan trọng. Vì`++a`, sự gia tăng xảy ra trước khi đóng góp. Vì`a++`, đóng góp xảy ra trước khi tăng. Trộn lẫn hai trường hợp này là lỗi triển khai phổ biến nhất. 

Số nguyên Python tự động xử lý tất cả các giá trị có thể một cách an toàn, do đó việc tràn không phải là vấn đề đáng lo ngại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
5*a++-3*++a+a++
```Các thuật ngữ được phân tích cú pháp: 

| Kỳ hạn | Hiệu quả | 

|---|---|---| 

|`5*a++`|`5`| 

|`-3*++a`|`-3`| 

|`a++`|`1`| 

Thứ tự sắp xếp:```
-3*++a
a++
5*a++
```Mô phỏng: 

| Bước | Kỳ hạn | một trước | Đóng góp | sau | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 |`-3*++a`| 1 |`-3 * 2 = -6`| 2 | -6 | 
| 2 |`a++`| 2 |`2`| 3 | -4 | 
| 3 |`5*a++`| 3 |`15`| 4 | 11 | 

Câu trả lời cuối cùng:```
11
```Ví dụ này thể hiện ý tưởng tham lam chính. Hệ số âm được đánh giá đầu tiên nên nó nhận giá trị nhỏ nhất có thể của`a`. 

### Ví dụ 2 

đầu vào:```
3
a+++a++
```Các thuật ngữ được phân tích cú pháp: 

| Kỳ hạn | Hiệu quả | 
| --- | --- | 
|`a++`|`1`| 
|`a++`|`1`| 

Bất kỳ đơn hàng nào cũng tương đương. 

Mô phỏng: 

| Bước | Kỳ hạn | một trước | Đóng góp | sau | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 |`a++`| 3 | 3 | 4 | 3 | 
| 2 |`a++`| 4 | 4 | 5 | 7 | 

Câu trả lời cuối cùng:```
7
```Điều này xác nhận rằng các hệ số bằng nhau có thể xuất hiện theo bất kỳ thứ tự nào mà không làm thay đổi kết quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp các lệnh triệu tập chiếm ưu thế | 
| Không gian | O(n) | lưu trữ các thuật ngữ được phân tích cú pháp | 

Với nhiều nhất`1000`triệu tập,`O(n log n)`đủ nhanh một cách dễ dàng. Việc sử dụng bộ nhớ rất nhỏ vì chúng tôi chỉ lưu trữ một vài số nguyên cho mỗi thuật ngữ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        a = int(input())
        s = input().strip()

        terms = []

        i = 0
        sign = 1

        while i < len(s):
            if s[i] == '+':
                sign = 1
                i += 1
            elif s[i] == '-':
                sign = -1
                i += 1

            coeff = 0
            has_coeff = False

            while i < len(s) and s[i].isdigit():
                coeff = coeff * 10 + int(s[i])
                has_coeff = True
                i += 1

            if has_coeff:
                i += 1
            else:
                coeff = 1

            if s[i:i + 3] == "++a":
                typ = 1
                i += 3
            else:
                typ = 0
                i += 3

            effective = sign * coeff
            terms.append((effective, sign, coeff, typ))

        terms.sort(key=lambda x: x[0])

        ans = 0
        cur = a

        for _, sign, coeff, typ in terms:
            if typ == 1:
                cur += 1
                ans += sign * coeff * cur
            else:
                ans += sign * coeff * cur
                cur += 1

        print(ans)

    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = old_stdout
    return out.getvalue()

# provided sample
assert run("1\n5*a++-3*++a+a++\n") == "11\n", "sample 1"

# minimum size
assert run("0\na++\n") == "0\n", "single term"

# ordering with negative coefficient
assert run("0\na++-++a\n") == "0\n", "negative term should go first"

# zero coefficients still increment
assert run("5\n0*a++-0*++a\n") == "0\n", "zero coefficient behavior"

# equal coefficients
assert run("3\na+++a++\n") == "7\n", "equal priorities"

# larger coefficients later
assert run("1\n10*a++-1*a++\n") == "19\n", "sorting correctness"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0\na++`|`0`| phân tích cú pháp triệu tập duy nhất | 
|`0\na++-++a`|`0`| điều kiện phủ định nên sớm | 
|`5\n0*a++-0*++a`|`0`| hệ số 0 vẫn sửa đổi`a`| 
|`3\na+++a++`|`7`| hệ số hiệu quả bằng nhau | 
|`1\n10*a++-1*a++`|`19`| thứ tự sắp xếp tham lam | 

## Vỏ cạnh 

Hãy xem xét:```
0
a++-++a
```Các hệ số hiệu quả được phân tích cú pháp là:```
1
-1
```Thứ tự sắp xếp:```
-++a
a++
```Mô phỏng: 

| Bước | Kỳ hạn | một trước | Đóng góp | sau | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 |`-++a`| 0 | -1 | 1 | -1 | 
| 2 |`a++`| 1 | 1 | 2 | 0 | 

Câu trả lời cuối cùng:```
0
```Thuật toán đặt chính xác hệ số âm đầu tiên để nó nhận được giá trị nhỏ nhất có thể của`a`. 

Bây giờ hãy xem xét các hệ số bằng 0:```
5
0*a++-0*++a
```Mặc dù cả hai đóng góp bằng số đều bằng 0, cả hai số hạng vẫn tăng`a`. 

Sắp xếp hệ số hiệu dụng:```
0
0
```Mô phỏng: 

| Bước | Kỳ hạn | một trước | Đóng góp | sau | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 |`0*a++`| 5 | 0 | 6 | 0 | 
| 2 |`-0*++a`| 6 | 0 | 7 | 0 | 

Câu trả lời cuối cùng vẫn còn`0`. 

Trường hợp này xác nhận rằng việc triển khai xử lý các tác dụng phụ một cách độc lập với sự đóng góp số học. 

Cuối cùng, hãy xem xét:```
1
10*a++-1*a++
```Hệ số hiệu quả:```
10
-1
```Thứ tự sắp xếp:```
-1*a++
10*a++
```Mô phỏng: 

| Bước | Kỳ hạn | một trước | Đóng góp | sau | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 |`-1*a++`| 1 | -1 | 2 | -1 | 
| 2 |`10*a++`| 2 | 20 | 3 | 19 | 

Đầu ra:```
19
```Một đánh giá ngây thơ từ trái sang phải chỉ đưa ra:```
10 - 2 = 8
```Ví dụ này chứng minh tại sao việc sắp xếp lại là cần thiết.
