---
title: "CF 11A - Dãy số tăng dần"
description: "Chúng tôi có một mảng các số nguyên và chúng tôi muốn mọi phần tử trở nên lớn hơn phần tử trước nó. Thao tác duy nhất được phép là chọn một phần tử duy nhất và tăng nó lên chính xác d. Mỗi lần sử dụng thao tác này được tính là một lần di chuyển."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 11
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 11"
rating: 900
weight: 11
solve_time_s: 84
verified: true
draft: false
---
[CF 11A - Dãy số tăng dần](https://codeforces.com/problemset/problem/11/A) 

**Xếp hạng:** 900 
**Tags:** thuật toán xây dựng, triển khai, toán học 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mảng các số nguyên và chúng tôi muốn mọi phần tử trở nên lớn hơn phần tử trước nó. Thao tác duy nhất được phép là chọn một phần tử duy nhất và tăng nó lên một cách chính xác`d`. Mỗi lần sử dụng thao tác này được tính là một lần di chuyển. 

Nhiệm vụ là tìm số lần di chuyển tối thiểu cần thiết để biến mảng thành một chuỗi tăng dần. 

Các ràng buộc đủ nhỏ để ngay cả các nghiệm bậc hai cũng có thể vượt qua một cách thoải mái. Với`n ≤ 2000`, MỘT`O(n²)`thuật toán thực hiện khoảng bốn triệu thao tác trong trường hợp xấu nhất, điều này phù hợp với giới hạn 1 giây trong Python. Đồng thời, cấu trúc của bài toán gợi ý rằng chúng ta có thể xử lý mảng một cách tham lam trong một lần duy nhất. 

Quan sát quan trọng là mỗi vị trí chỉ phụ thuộc vào vị trí trước đó. Khi chúng tôi quyết định giá trị cuối cùng của`a[i-1]`, giá trị hợp lệ nhỏ nhất cho`a[i]`đã được cố định: nó phải trở thành ít nhất`a[i-1] + 1`. 

Một số trường hợp nguy hiểm có thể dễ dàng phá vỡ việc triển khai bất cẩn. 

Hãy xem xét một mảng đang tăng lên:```
3 5
1 7 20
```Câu trả lời là`0`. Việc triển khai sai vẫn có thể áp dụng các thao tác vì nó kiểm tra`<=`không chính xác hoặc cập nhật giá trị không cần thiết. 

Các giá trị liền kề bằng nhau là một cạm bẫy phổ biến khác:```
3 2
1 1 1
```Câu trả lời đúng là`3`. 

Trình tự phát triển như thế này:```
1 1 1
1 3 1
1 3 5
```Phần tử thứ hai cần một thao tác, nhưng phần tử thứ ba bây giờ phải vượt quá`3`, không phải bản gốc`1`. Việc quên cập nhật mảng sau khi sửa đổi sẽ tạo ra số đếm sai. 

Một trường hợp tinh vi hơn xuất hiện khi khoảng cách không chia hết cho`d`:```
2 3
5 6
```Mảng đã tăng lên nên câu trả lời là`0`. 

Nếu không có:```
2 3
5 5
```giá trị thứ hai phải trở thành ít nhất`6`. Một thao tác thay đổi nó từ`5`ĐẾN`8`, vậy câu trả lời là`1`. 

Sử dụng phép chia số nguyên thông thường như:```
needed = (target - current) // d
```thất bại ở đây bởi vì`1 // 3 = 0`. Chúng ta cần phân chia trần nhà. 

Một ví dụ phức tạp khác là:```
2 4
10 1
```Giá trị thứ hai phải vượt quá`10`, do đó nó phải đạt ít nhất`11`. 

Trình tự các giá trị có thể truy cập là:```
1 -> 5 -> 9 -> 13
```Cần phải có ba nước đi chứ không phải hai nước. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Chúng tôi quét mảng từ trái sang phải. Bất cứ khi nào`a[i]`không thực sự lớn hơn`a[i-1]`, chúng tôi liên tục thêm`d`cho đến khi điều kiện trở thành đúng. Mỗi lần bổ sung được tính là một lần di chuyển. 

Phương pháp này đúng vì mọi thao tác chỉ làm tăng phần tử hiện tại và việc làm cho nó lớn hơn sớm hơn không bao giờ có thể ảnh hưởng đến các vị trí trong tương lai. Vấn đề là hiệu quả. Nếu như`d = 1`và khoảng cách lớn, chúng ta có thể thực hiện nhiều phép cộng riêng lẻ. Trong trường hợp xấu nhất, các giá trị có thể tăng lên hàng triệu, khiến số lần lặp vòng lặp lớn một cách không cần thiết. 

Ví dụ:```
2 1
1000000 1
```Phần tử thứ hai sẽ yêu cầu 1.000.000 lần lặp của một vòng lặp. 

Điểm mấu chốt là chúng ta không bao giờ cần phải mô phỏng từng phần bổ sung một. Đối với giá trị hiện tại`x`và yêu cầu tối thiểu`need`, chúng ta có thể tính toán trực tiếp số lần bổ sung cần thiết. 

Chúng tôi muốn:```
x + k*d > previous
```Tương đương:```
x + k*d >= previous + 1
```Vì thế:```
k >= (previous + 1 - x) / d
```Số nguyên hợp lệ nhỏ nhất`k`là trần của phân số đó. 

Điều này biến sức mạnh vũ phu cập nhật lặp đi lặp lại thành một phép tính toán học trực tiếp. Chúng ta vẫn xử lý mảng từ trái sang phải, nhưng mọi vị trí đều được xử lý theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng số hoạt động) | O(1) | Có khả năng quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và khởi tạo bộ đếm câu trả lời`0`. 
2. Xử lý mảng từ trái sang phải bắt đầu từ chỉ số`1`. 
3. Đối với mỗi vị trí, kiểm tra xem`a[i] > a[i-1]`. 
4. Nếu điều kiện đã được giữ, hãy chuyển sang phần tử tiếp theo vì vị trí này không cần thay đổi. 
5. Mặt khác, hãy tính giá trị tối thiểu mà phần tử hiện tại phải đạt được:```
target = a[i-1] + 1
```6. Tính xem có bao nhiêu phép cộng của`d`được yêu cầu phải đạt ít nhất`target`. 

Số tăng cần thiết là:```
target - a[i]
```Vì mọi thao tác đều cộng chính xác`d`, số lần thực hiện là:```
ceil((target - a[i]) / d)
```Trong số học số nguyên:```
ops = (target - a[i] + d - 1) // d
```7. Thêm`ops`để trả lời. 
8. Cập nhật giá trị mảng hiện tại:```
a[i] += ops * d
```Giá trị cập nhật này trở thành tham chiếu cho phần tử tiếp theo. 

### Tại sao nó hoạt động 

Thuật toán duy trì bất biến sau khi xử lý chỉ số`i`, tiền tố`a[0...i]`đang tăng lên một cách nghiêm ngặt với số lượng hoạt động tối thiểu có thể. 

Khi xử lý`a[i]`, giá trị trước đó đã được cố định một cách tối ưu. Bất kỳ giá trị cuối cùng hợp lệ nào cho`a[i]`phải lớn hơn`a[i-1]`. Việc chọn giá trị nhỏ nhất có thể đạt tới như vậy luôn là tối ưu vì việc tạo`a[i]`lớn không cần thiết chỉ làm cho các yếu tố trong tương lai khó đáp ứng hơn. Vì mọi thao tác đều thay đổi giá trị một cách chính xác`d`, bộ phận trần tính toán chính xác số lượng hoạt động tối thiểu được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, d = map(int, input().split())
a = list(map(int, input().split()))

answer = 0

for i in range(1, n):
    if a[i] <= a[i - 1]:
        target = a[i - 1] + 1
        ops = (target - a[i] + d - 1) // d

        answer += ops
        a[i] += ops * d

print(answer)
```Giải pháp tuân theo chiến lược tham lam từ trái sang phải một cách trực tiếp. 

Vòng lặp bắt đầu từ chỉ mục`1`bởi vì phần tử đầu tiên không bao giờ cần sửa đổi. Đối với mỗi vị trí, chúng tôi chỉ quan tâm đến giá trị ngay trước đó. Các phần tử trước đó đã tạo thành tiền tố tăng tối ưu. 

Công thức chia trần là chi tiết thực hiện quan trọng nhất:```
(target - a[i] + d - 1) // d
```Việc sử dụng phép chia thông thường sẽ thất bại khi mức tăng yêu cầu không phải là bội số của`d`. 

Cập nhật mảng:```
a[i] += ops * d
```cũng là điều cần thiết. Các phần tử trong tương lai phải so sánh với giá trị đã sửa đổi, không phải giá trị ban đầu. 

Số nguyên Python tự động xử lý các giá trị lớn, do đó việc tràn không phải là vấn đề đáng lo ngại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 2
1 3 3 2
```| tôi | Giá trị trước đó | Giá trị hiện tại | Mục tiêu | Hoạt động | Giá trị cập nhật | Tổng số lượt di chuyển | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | - | 0 | 3 | 0 | 
| 2 | 3 | 3 | 4 | 1 | 5 | 1 | 
| 3 | 5 | 2 | 6 | 2 | 6 | 3 | 

Mảng cuối cùng:```
1 3 5 6
```Trả lời:```
3
```Dấu vết này cho thấy tại sao việc cập nhật mảng lại quan trọng. Sau chỉ mục`2`trở thành`5`, phần tử cuối cùng phải vượt quá`5`, không phải bản gốc`3`. 

### Ví dụ 2 

đầu vào:```
5 3
1 1 1 1 1
```| tôi | Giá trị trước đó | Giá trị hiện tại | Mục tiêu | Hoạt động | Giá trị cập nhật | Tổng số lượt di chuyển | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 2 | 1 | 4 | 1 | 
| 2 | 4 | 1 | 5 | 2 | 7 | 3 | 
| 3 | 7 | 1 | 8 | 3 | 10 | 6 | 
| 4 | 10 | 1 | 11 | 4 | 13 | 10 | 

Mảng cuối cùng:```
1 4 7 10 13
```Trả lời:```
10
```Ví dụ này thể hiện hiệu ứng xếp tầng. Mọi giá trị được cập nhật sẽ trở thành giới hạn dưới cho vị trí tiếp theo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử mảng được xử lý một lần | 
| Không gian | O(1) | Chỉ có một vài biến bổ sung được sử dụng | 

Với`n ≤ 2000`, giải pháp này dễ dàng nằm trong giới hạn. Thuật toán chỉ thực hiện số học đơn giản và quét tuyến tính đơn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n, d = map(int, input().split())
    a = list(map(int, input().split()))

    answer = 0

    for i in range(1, n):
        if a[i] <= a[i - 1]:
            target = a[i - 1] + 1
            ops = (target - a[i] + d - 1) // d

            answer += ops
            a[i] += ops * d

    print(answer)

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
assert run("4 2\n1 3 3 2\n") == "3\n", "sample 1"

# minimum size, already increasing
assert run("2 1\n1 2\n") == "0\n", "minimum increasing"

# all equal values
assert run("3 2\n1 1 1\n") == "3\n", "all equal"

# ceiling division case
assert run("2 3\n5 5\n") == "1\n", "needs ceiling division"

# large gap
assert run("2 4\n10 1\n") == "3\n", "large jump"

# strictly decreasing
assert run("5 1\n5 4 3 2 1\n") == "20\n", "decreasing sequence"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 / 1 2`|`0`| Đã tăng | 
|`3 2 / 1 1 1`|`3`| Cập nhật xếp tầng | 
|`2 3 / 5 5`|`1`| Phân chia trần đúng cách | 
|`2 4 / 10 1`|`3`| Bước nhảy yêu cầu lớn | 
|`5 1 / 5 4 3 2 1`|`20`| Lan truyền lặp lại qua mảng | 

## Vỏ cạnh 

Xem xét các giá trị liền kề bằng nhau:```
3 2
1 1 1
```Tại chỉ mục`1`, giá trị trước đó là`1`, do đó mục tiêu trở thành`2`. Một thao tác thay đổi giá trị từ`1`ĐẾN`3`. 

Bây giờ mảng là:```
1 3 1
```Tại chỉ mục`2`, giá trị trước đó bây giờ là`3`, do đó mục tiêu trở thành`4`. Hai thao tác thay đổi`1`vào trong`5`. 

Tổng số hoạt động:```
1 + 2 = 3
```Thuật toán thành công vì nó luôn so sánh với giá trị được cập nhật trước đó. 

Bây giờ hãy xem xét trường hợp mức tăng cần thiết không chia hết cho`d`:```
2 4
7 4
```Giá trị thứ hai ít nhất phải trở thành`8`. 

Số tăng cần thiết là:```
8 - 4 = 4
```Một thao tác là đủ:```
4 + 4 = 8
```Công thức tính:```
(4 + 4 - 1) // 4 = 1
```cái nào đúng. 

Cuối cùng, hãy xem xét một chuỗi đã hợp lệ:```
4 10
1 20 30 40
```Mọi phần tử đều thỏa mãn bất đẳng thức chặt chẽ. Điều kiện:```
if a[i] <= a[i - 1]:
```không bao giờ kích hoạt, vì vậy câu trả lời vẫn còn`0`. 

Điều này xác nhận thuật toán không thực hiện các hoạt động không cần thiết.
