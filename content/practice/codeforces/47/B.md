---
title: "CF 47B - Tiền xu"
description: "Chúng ta có ba đồng xu được dán nhãn A, B và C. Mỗi cặp đồng xu đã được so sánh một lần bằng cân cân và mỗi lần so sánh sẽ cho chúng ta biết đồng xu nào nặng hơn."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 47
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 44 (Div. 2)"
rating: 1200
weight: 47
solve_time_s: 102
verified: true
draft: false
---
[CF 47B - Tiền xu](https://codeforces.com/problemset/problem/47/B) 

**Đánh giá:** 1200 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có ba đồng xu được dán nhãn`A`,`B`, Và`C`. Mỗi cặp đồng xu đã được so sánh một lần bằng cân cân và mỗi lần so sánh sẽ cho chúng ta biết đồng xu nào nặng hơn. 

Một dòng như`A>B`có nghĩa là đồng xu`A`nặng hơn đồng xu`B`. Vì những đồng xu nặng hơn được cho là có mệnh giá lớn hơn nên nhiệm vụ thực sự là khôi phục thứ tự hoàn chỉnh của ba đồng xu từ nhẹ nhất đến nặng nhất. 

Dữ liệu đầu vào luôn chứa chính xác ba phép so sánh, một phép so sánh cho mỗi cặp trong số ba đồng xu. Đầu ra phải là một chuỗi có độ dài ba đại diện cho các đồng xu từ trọng lượng nhỏ nhất đến trọng lượng lớn nhất. Nếu sự so sánh mâu thuẫn với nhau và không tồn tại thứ tự hợp lệ, chúng ta sẽ in`Impossible`. 

Các ràng buộc rất nhỏ vì chỉ có ba đồng xu. Bất kỳ thuật toán hợp lý nào cũng sẽ dễ dàng phù hợp trong giới hạn. Ngay cả việc kiểm tra mọi thứ tự có thể cũng thực sự là thời gian liên tục vì chỉ có`3! = 6`hoán vị. Thách thức không phải là hiệu suất mà là phát hiện chính xác những mâu thuẫn. 

Phần khó khăn là một số bộ so sánh tạo thành chu kỳ. Ví dụ:```
A>B
B>C
C>A
```Điều này nói`A`nặng hơn`B`,`B`nặng hơn`C`, Và`C`nặng hơn`A`. Không có thứ tự nào có thể thỏa mãn đồng thời cả ba điều kiện. Việc thực hiện bất cẩn mà chỉ đếm số lần thắng hoặc thua có thể vô tình tạo ra một lệnh không hợp lệ thay vì phát hiện ra sự mâu thuẫn. 

Một trường hợp tinh tế khác là khi các so sánh đã được đưa ra theo nhiều hướng khác nhau:```
A<B
C<B
A<C
```Thứ tự đúng là`ACB`. Nếu chúng ta chỉ theo dõi các mối quan hệ trực tiếp mà không xem xét thứ tự đầy đủ, chúng ta có thể đặt sai`B`quá sớm vì nó xuất hiện ở cả hai phía của sự so sánh. 

Trường hợp cạnh thứ ba là khi hai đồng xu có vẻ bị ràng buộc như nhau:```
A>B
A>C
B<C
```Thứ tự đúng là`BCA`. Đồng xu mạnh nhất rất dễ nhận biết nhưng hai đồng còn lại vẫn cần phải được đặt hàng cẩn thận. Việc sắp xếp theo "số lần chiến thắng" chỉ có tác dụng ở đây, nhưng chỉ vì biểu đồ có tính chu kỳ. Nếu không kiểm tra mâu thuẫn, logic tương tự sẽ bị phá vỡ đối với các đầu vào theo chu kỳ. 

## Phương pháp tiếp cận 

Giải pháp mạnh mẽ trực tiếp nhất là tạo ra tất cả sáu thứ tự có thể có của ba đồng tiền và kiểm tra từng thứ tự so với các so sánh. 

Giả sử chúng ta thử hoán vị`CAB`. Điều này có nghĩa`C < A < B`. Đối với mọi quan hệ đầu vào, chúng tôi xác minh xem nó có phù hợp với các vị trí bên trong hoán vị hay không. Nếu cả ba so sánh đều trùng khớp thì chúng ta đã tìm được câu trả lời. 

Cách tiếp cận này đúng vì mọi thứ tự tổng hợp lệ phải xuất hiện trong số sáu hoán vị. Thuật toán chỉ đơn giản kiểm tra tất cả các ứng cử viên cho đến khi một ứng cử viên thỏa mãn mọi điều kiện. 

Chỉ với sáu hoán vị, thời gian chạy thực tế là không đổi. Mặc dù bạo lực hoàn toàn có thể chấp nhận được ở đây, nhưng vẫn hữu ích khi nghĩ xem tại sao nó lại có tác dụng. Chúng tôi đang trực tiếp xác nhận tính nhất quán toàn cầu thay vì cố gắng suy ra thứ tự tăng dần. 

Ngoài ra còn có một giải pháp dựa trên quan sát rõ ràng hơn. 

Mỗi so sánh đưa ra một mối quan hệ trực tiếp giữa các đồng tiền. Nếu chúng ta đếm xem mỗi đồng xu nặng hơn đồng xu khác bao nhiêu lần thì đồng xu nhẹ nhất sẽ không thắng, đồng xu ở giữa sẽ có một lần thắng và đồng xu nặng nhất sẽ có hai lần thắng, giả sử các so sánh là nhất quán. 

Ví dụ:```
A>B
C<B
A>C
```đồng xu`A`thắng hai lần,`C`thắng một lần và`B`thắng không lần nào. Sắp xếp theo số trận thắng mang lại`BCA`, nhưng đó là từ yếu nhất đến mạnh nhất về số chiến thắng. Vì chiến thắng tương ứng với những đồng tiền nặng hơn nên chúng ta đảo ngược cách giải thích và thu được`CBA`theo thứ tự tăng dần của trọng lượng. 

Thông tin chi tiết hữu ích là với chính xác ba yếu tố riêng biệt, một thứ tự hợp lệ sẽ tạo thành một chuỗi nghiêm ngặt. Số lượng ngoài mức độ xác định duy nhất vị trí của mỗi đồng xu trừ khi tồn tại mâu thuẫn. 

Trường hợp mâu thuẫn xuất hiện một cách tự nhiên. Trong một chu kỳ như:```
A>B
B>C
C>A
```mỗi đồng xu có chính xác một chiến thắng. Không có thứ tự duy nhất nào tồn tại vì tất cả số đếm đều giống hệt nhau. Phát hiện số lượng trùng lặp ngay lập tức cho thấy sự không nhất quán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(6 × 3) | O(1) | Đã chấp nhận | 
| Tối ưu | O(3) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo từ điển lưu trữ số lần thắng của mỗi coin:`A`,`B`, Và`C`. 
2. Đọc lần lượt ba câu so sánh. 
3. Đối với mỗi so sánh: 

Nếu mối quan hệ là`X>Y`, tăng số lần thắng của`X`. 

Nếu mối quan hệ là`X<Y`, tăng số lần thắng của`Y`. 

Đồng xu nặng hơn giành được một chiến thắng vì nó lấn át đồng xu nhẹ hơn. 
4. Sau khi xử lý tất cả các so sánh, hãy kiểm tra ba số lần thắng. 
5. Trong một thứ tự hợp lệ, số đếm phải chính xác`0`,`1`, Và`2`theo một thứ tự nào đó. 

Nếu hai đồng xu có cùng số lượng thì mối quan hệ trái ngược nhau, vì vậy hãy in`Impossible`. 
6. Nếu không, hãy sắp xếp các đồng xu theo số lần thắng tăng dần. 

Một đồng xu có ít chiến thắng hơn sẽ nhẹ hơn vì nó nặng hơn các đồng xu khác. 
7. Xuất chuỗi đã sắp xếp dưới dạng chuỗi. 

### Tại sao nó hoạt động 

Mỗi thứ tự hợp lệ của ba đồng xu riêng biệt sẽ tạo thành một thứ hạng nghiêm ngặt. Đồng xu nhẹ nhất thì nặng hơn không ai cả, nên nó sẽ nặng hơn`0`thắng. Đồng xu ở giữa nặng hơn đúng một đồng xu nên nó nặng hơn`1`thắng. Đồng xu nặng nhất nặng hơn cả hai đồng xu còn lại nên nó nặng hơn`2`thắng. 

Một mâu thuẫn tạo ra một chu trình, ngăn cản các số đếm này là duy nhất. Trong một chu kỳ, không có đồng xu nào có thể liên tục chiếm vị trí duy nhất trong bảng xếp hạng. Vì thuật toán chỉ chấp nhận mẫu riêng biệt`0,1,2`, nó bác bỏ mọi cấu hình mâu thuẫn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

wins = {'A': 0, 'B': 0, 'C': 0}

for _ in range(3):
    s = input().strip()

    if s[1] == '>':
        wins[s[0]] += 1
    else:
        wins[s[2]] += 1

values = list(wins.values())

if len(set(values)) < 3:
    print("Impossible")
else:
    order = sorted(wins.keys(), key=lambda x: wins[x])
    print(''.join(order))
```Từ điển`wins`lưu trữ số lần mỗi đồng xu xuất hiện dưới dạng đồng xu nặng hơn. 

Mỗi so sánh có độ dài ba, vì vậy`s[0]`Và`s[2]`là nhãn tiền xu trong khi`s[1]`là một trong hai`<`hoặc`>`. 

Khi mối quan hệ là`A>B`, đồng xu`A`giành được một chiến thắng vì nó nặng hơn. Khi mối quan hệ là`A<B`, đồng xu`B`thay vào đó giành được một chiến thắng. 

Sau khi tất cả các so sánh được xử lý, giải pháp sẽ kiểm tra xem tất cả các số đếm có khác biệt hay không. Một chuỗi ba phần tử hợp lệ phải tạo ra số đếm`{0,1,2}`. Bất kỳ giá trị lặp lại nào đều biểu thị một chu kỳ hoặc mâu thuẫn. 

Bước sắp xếp rất tinh tế. Chúng tôi sắp xếp theo thứ tự chiến thắng tăng dần vì ít chiến thắng hơn có nghĩa là trọng lượng nhẹ hơn. Nếu chúng ta vô tình sắp xếp theo thứ tự giảm dần, đầu ra sẽ trở nên nặng nhất đến nhẹ nhất thay vì nhẹ nhất đến nặng nhất theo yêu cầu. 

Không cần cấu trúc biểu đồ bổ sung vì chỉ có ba nút và số lượng bên ngoài xác định đầy đủ thứ tự. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
A>B
C<B
A>C
```| Bước | So sánh | Thắng A | Thắng B | Thắng C | 
| --- | --- | --- | --- | --- | 
| Ban đầu | - | 0 | 0 | 0 | 
| 1 | A>B | 1 | 0 | 0 | 
| 2 | C<B | 1 | 1 | 0 | 
| 3 | A>C | 2 | 1 | 0 | 

Sắp xếp theo số lần thắng tăng dần mang lại:```
C B A
```Đầu ra:```
CBA
```Dấu vết này thể hiện cấu trúc xếp hạng rõ ràng`0,1,2`. Mỗi đồng xu chiếm một vị trí duy nhất nên thứ tự là hợp lệ. 

### Ví dụ 2 

đầu vào:```
A>B
B>C
C>A
```| Bước | So sánh | Thắng A | Thắng B | Thắng C | 
| --- | --- | --- | --- | --- | 
| Ban đầu | - | 0 | 0 | 0 | 
| 1 | A>B | 1 | 0 | 0 | 
| 2 | B>C | 1 | 1 | 0 | 
| 3 | C>A | 1 | 1 | 1 | 

Các số đếm đều giống hệt nhau. 

Vì các giá trị không khác biệt nên thuật toán sẽ in:```
Impossible
```Ví dụ này cho thấy các chu kỳ phá hủy thứ hạng duy nhất cần thiết cho một đơn hàng hợp lệ như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có ba so sánh và ba đồng tiền được xử lý | 
| Không gian | O(1) | Từ điển lưu trữ dữ liệu cho đúng ba xu | 

Kích thước đầu vào không bao giờ tăng vượt quá một hằng số nhỏ nên giải pháp sẽ chạy ngay lập tức trong giới hạn. Việc sử dụng bộ nhớ cũng không đổi vì chỉ có một số số nguyên và ký tự được lưu trữ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    wins = {'A': 0, 'B': 0, 'C': 0}

    for _ in range(3):
        s = input().strip()

        if s[1] == '>':
            wins[s[0]] += 1
        else:
            wins[s[2]] += 1

    if len(set(wins.values())) < 3:
        print("Impossible")
    else:
        order = sorted(wins.keys(), key=lambda x: wins[x])
        print(''.join(order))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided sample
assert run("A>B\nC<B\nA>C\n") == "CBA", "sample 1"

# cyclic contradiction
assert run("A>B\nB>C\nC>A\n") == "Impossible", "cycle detection"

# already increasing
assert run("A<B\nB<C\nA<C\n") == "ABC", "simple sorted order"

# reverse order
assert run("A>B\nA>C\nB>C\n") == "CBA", "fully descending"

# mixed relations
assert run("A<C\nB>A\nB<C\n") == "ABC", "mixed direction comparisons"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`A>B, B>C, C>A`|`Impossible`| Phát hiện mâu thuẫn mang tính chu kỳ | 
|`A<B, B<C, A<C`|`ABC`| Xử lý đã sắp xếp thứ tự | 
|`A>B, A>C, B>C`|`CBA`| Xử lý xếp hạng đảo ngược hoàn toàn | 
|`A<C, B>A, B<C`|`ABC`| Xác minh hướng so sánh hỗn hợp | 

## Vỏ cạnh 

Xét chu trình nghịch đảo:```
A>B
B>C
C>A
```Thuật toán tính số trận thắng:```
A = 1
B = 1
C = 1
```Vì số đếm không phải là duy nhất nên thuật toán sẽ in ngay lập tức`Impossible`. Điều này từ chối chính xác thứ tự không thể. 

Bây giờ hãy xem xét trường hợp so sánh được viết theo nhiều hướng khác nhau:```
A<B
C<B
A<C
```Xử lý các so sánh cho:```
A = 0
C = 1
B = 2
```Sắp xếp theo chiến thắng tạo ra`ACB`, phù hợp với thứ tự thực sự từ nhẹ nhất đến nặng nhất. Thuật toán không quan tâm đến hướng đầu vào vì nó luôn tăng đồng xu nặng hơn. 

Cuối cùng, hãy xem xét:```
A>B
A>C
B<C
```Số lần thắng trở thành:```
C = 0
B = 1
A = 2
```Đầu ra là`CBA`. Trường hợp này xác nhận rằng đồng xu ở giữa được xác định chính xác ngay cả khi đồng xu mạnh nhất đã rõ ràng ngay từ đầu.
