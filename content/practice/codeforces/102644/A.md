---
title: "CF 102644A - Tâm trạng ngẫu nhiên"
description: "Bài toán mô tả một người có tâm trạng chỉ có hai trạng thái: vui và buồn. Mỗi giây, tâm trạng hiện tại có thể thay đổi với xác suất p, hoặc giữ nguyên với xác suất 1 - p."
date: "2026-08-01T10:10:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102644
codeforces_index: "A"
codeforces_contest_name: "Matrix Exponentiation"
rating: 0
weight: 102644
solve_time_s: 227
verified: true
draft: false
---

[CF 102644A - Tâm trạng ngẫu nhiên](https://codeforces.com/problemset/problem/102644/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán mô tả một người có tâm trạng chỉ có hai trạng thái: vui và buồn. Mỗi giây, tâm trạng hiện tại có thể thay đổi theo xác suất`p`, hoặc giữ nguyên với xác suất`1 - p`. Chúng ta bắt đầu ở trạng thái hạnh phúc và cần tìm xác suất để người đó hạnh phúc trở lại sau một thời gian chính xác.`n`giây. 

Đầu vào chứa số giây để mô phỏng và xác suất thay đổi tâm trạng trong một giây. Đầu ra cần thiết là xác suất cuối cùng để được hạnh phúc sau những kết quả đó.`n`chuyển tiếp. 

Giá trị của`n`có thể đạt được`10^9`, điều này ngay lập tức loại trừ việc mô phỏng từng giây. Một mô phỏng trực tiếp sẽ yêu cầu một thao tác mỗi giây và thậm chí một giải pháp tuyến tính cũng sẽ cần một tỷ bản cập nhật trong trường hợp xấu nhất. Xác suất có cấu trúc hai trạng thái đơn giản nên lời giải phải sử dụng phương pháp xử lý các chuyển đổi lặp lại một cách hiệu quả, giảm số phép toán từ tỉ lệ xuống`n`tỷ lệ thuận với số bit trong`n`. 

Những cạm bẫy triển khai chính đến từ các giá trị lớn của`n`và từ việc xử lý xác suất một cách chính xác. Ví dụ, với đầu vào`1 0.7`, câu trả lời là`0.3`bởi vì cách duy nhất để vẫn hạnh phúc sau một giây là không thay đổi tâm trạng. Việc thực hiện bất cẩn có thể quay trở lại`0.7`bằng cách coi xác suất chuyển đổi là xác suất hạnh phúc cuối cùng. 

Một lỗi phổ biến khác xuất hiện khi`n`là chẵn. Đối với đầu vào`2 0.1`, câu trả lời là`0.82`, bởi vì tâm trạng sẽ vui nếu chuyển đổi hai lần hoặc không chuyển đổi chút nào. Một giải pháp chỉ xem xét xác suất không thay đổi trong mỗi giây sẽ tính toán`0.81`và bỏ lỡ những con đường trở lại hạnh phúc sau một số lần chuyển đổi chẵn. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là theo dõi xác suất vui và buồn sau mỗi giây. Ban đầu xác suất hạnh phúc là`1`và xác suất đáng buồn là`0`. Mỗi giây, chúng tôi cập nhật cả hai giá trị bằng cách sử dụng quy tắc chuyển đổi. Nếu xác suất hạnh phúc hiện tại là`h`và xác suất buồn là`s`, thì xác suất hạnh phúc tiếp theo là`h * (1 - p) + s * p`, trong khi xác suất đáng buồn tiếp theo là`h * p + s * (1 - p)`. 

Phương pháp này đúng vì nó trực tiếp tuân theo các chuyển đổi xác suất. Vấn đề là số lần chuyển tiếp. Khi`n`là`10^9`, điều này đòi hỏi một tỷ bản cập nhật, vượt xa thời gian sẵn có. 

Quan sát quan trọng là mỗi giây đều áp dụng cùng một phép biến đổi cho cặp xác suất. Việc lặp lại cùng một phép biến đổi nhiều lần chính là trường hợp mà phép lũy thừa ma trận trở nên hữu ích. Chúng ta có thể biểu diễn một quá trình chuyển đổi dưới dạng ma trận 2 x 2 và nâng ma trận đó lên lũy thừa`n`sử dụng lũy ​​thừa nhị phân. 

Brute-force hoạt động vì mỗi giây là một ứng dụng độc lập của cùng một quy tắc chuyển tiếp, nhưng không thành công khi số giây trở nên quá lớn. Nhận xét rằng bản thân quá trình chuyển đổi có thể được biểu diễn dưới dạng ma trận cho phép chúng ta áp dụng tất cả`n`chuyển tiếp theo thời gian logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm | 
| Tối ưu | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng ma trận chuyển tiếp mô tả một giây thay đổi tâm trạng. 

Nếu trạng thái hiện tại được biểu diễn dưới dạng vectơ cột`[happy, sad]`, một lần chuyển đổi sẽ thay đổi nó thành:```
[1-p  p] [happy]
[ p  1-p] [sad]
```Ma trận chứa mọi xác suất chuyển tiếp có thể có giữa hai trạng thái. 
2. Nâng ma trận chuyển tiếp này lên lũy thừa`n`sử dụng lũy ​​thừa nhị phân. 

Thay vì nhân ma trận với vectơ trạng thái`n`nhiều lần, chúng ta liên tục bình phương ma trận. Khi một chút`n`được đặt, ma trận bình phương đó sẽ góp phần đưa ra câu trả lời cuối cùng. 
3. Áp dụng ma trận thu được cho trạng thái ban đầu. 

Trạng thái ban đầu là`[1, 0]`bởi vì người đó bắt đầu hạnh phúc. Sau khi nhân với ma trận nâng lên`n`lũy thừa thứ, thành phần đầu tiên của vectơ kết quả là xác suất hạnh phúc. 

Tại sao nó hoạt động: 

Ma trận chuyển tiếp thể hiện chính xác một giây của quá trình. Phép nhân ma trận kết hợp các phép chuyển đổi, do đó nhân ma trận với chính nó`n`thời gian đại diện cho việc thực hiện quá trình chuyển đổi`n`lần. Phép lũy thừa nhị phân tính toán cùng một lũy thừa ma trận bằng cách sử dụng phép bình phương lặp lại, giúp duy trì ý nghĩa toán học chính xác trong khi giảm số lượng phép tính. Vì vectơ trạng thái ban đầu đã biết nên phép nhân cuối cùng sẽ rút ra xác suất chính xác để hạnh phúc sau tất cả các lần chuyển đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def multiply(a, b):
    return [
        [
            a[0][0] * b[0][0] + a[0][1] * b[1][0],
            a[0][0] * b[0][1] + a[0][1] * b[1][1],
        ],
        [
            a[1][0] * b[0][0] + a[1][1] * b[1][0],
            a[1][0] * b[0][1] + a[1][1] * b[1][1],
        ],
    ]

def power(matrix, n):
    result = [[1.0, 0.0], [0.0, 1.0]]
    while n > 0:
        if n & 1:
            result = multiply(result, matrix)
        matrix = multiply(matrix, matrix)
        n >>= 1
    return result

def solve():
    n, p = input().split()
    n = int(n)
    p = float(p)

    transition = [
        [1.0 - p, p],
        [p, 1.0 - p],
    ]

    result = power(transition, n)

    answer = result[0][0]
    print("{:.10f}".format(answer))

solve()
```các`multiply`hàm thực hiện phép nhân hai ma trận 2 với 2. Vì các ma trận luôn nhỏ như vậy nên việc viết trực tiếp bốn biểu thức sẽ tránh được các vòng lặp không cần thiết. 

các`power`hàm bắt đầu bằng ma trận nhận dạng vì nó thể hiện việc áp dụng các chuyển đổi bằng 0. Trong quá trình lũy thừa nhị phân, mỗi tập bit của`n`thêm sức mạnh tương ứng của ma trận chuyển tiếp vào câu trả lời. Bình phương ma trận sau mỗi bit cho phép chúng ta bỏ qua những khoảng chuyển tiếp lớn. 

Ma trận cuối cùng đại diện cho tất cả`n`giây chuyển động. Vì trạng thái ban đầu là`[1, 0]`, chỉ có cột đầu tiên của ma trận kết quả là quan trọng và giá trị tại vị trí`[0][0]`là xác suất kết thúc có hậu. 

Độ chính xác của dấu phẩy động là đủ vì lỗi yêu cầu chỉ`10^-6`. Số nguyên Python không liên quan đến kết quả lũy thừa, do đó không có vấn đề tràn. 

## Ví dụ đã hoạt động 

Đối với mẫu 1: 

đầu vào:```
1 0.7
```Ma trận chuyển tiếp là:```
[0.3 0.7]
[0.7 0.3]
```| Biến | Ban đầu | Sau khi xử lý bit | 
| --- | --- | --- | 
| n | 1 | 0 | 
| kết quả[0][0] | 1.0 | 0,3 | 
| ma trận[0][0] | 0,3 | 0,58 | 

Bit được đặt duy nhất là bit đầu tiên của`n`, do đó ma trận chuyển tiếp sẽ trở thành ma trận trả lời. Giá trị trên cùng bên trái là`0.3`, phù hợp với xác suất giữ được hạnh phúc trong một giây. 

Đối với mẫu 2: 

đầu vào:```
2 0.1
```Ma trận chuyển tiếp là:```
[0.9 0.1]
[0.1 0.9]
```| Biến | Ban đầu | Sau hình vuông đầu tiên | 
| --- | --- | --- | 
| n | 2 | 1 | 
| kết quả[0][0] | 1.0 | 1.0 | 
| ma trận[0][0] | 0,9 | 0,82 | 

Biểu diễn nhị phân của`2`là`10`, do đó ma trận chuyển tiếp đầu tiên được bình phương trước khi được áp dụng. Giá trị trên cùng bên trái thu được là`0.82`, biểu thị xác suất không có công tắc hoặc có hai công tắc. 

Những ví dụ này cho thấy tại sao cách tiếp cận ma trận xử lý một cách tự nhiên cả số lẻ và số chẵn của những thay đổi tâm trạng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | Phép lũy thừa nhị phân thực hiện một lượng ma trận 2 x 2 không đổi cho mỗi bit của`n`. | 
| Không gian | O(1) | Chỉ một số ma trận có kích thước cố định được lưu trữ. | 

Giá trị lớn nhất có thể có của`n`chỉ có khoảng 30 bit, do đó thuật toán thực hiện một số lượng rất nhỏ phép nhân ma trận ngay cả trong trường hợp xấu nhất. Nó dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    def multiply(a, b):
        return [
            [
                a[0][0] * b[0][0] + a[0][1] * b[1][0],
                a[0][0] * b[0][1] + a[0][1] * b[1][1],
            ],
            [
                a[1][0] * b[0][0] + a[1][1] * b[1][0],
                a[1][0] * b[0][1] + a[1][1] * b[1][1],
            ],
        ]

    def power(matrix, n):
        result = [[1.0, 0.0], [0.0, 1.0]]
        while n:
            if n & 1:
                result = multiply(result, matrix)
            matrix = multiply(matrix, matrix)
            n >>= 1
        return result

    n, p = sys.stdin.readline().split()
    n = int(n)
    p = float(p)
    m = [[1 - p, p], [p, 1 - p]]
    ans = power(m, n)[0][0]

    sys.stdin = old_stdin
    return "{:.10f}".format(ans)

assert run("1 0.7\n") == "0.3000000000", "sample 1"
assert run("2 0.1\n") == "0.8200000000", "sample 2"
assert run("11 0.06\n") == "0.6225404294", "sample 3"

assert run("1 0.5\n") == "0.5000000000", "single step with equal switching chance"
assert run("1000000000 0.5\n") == "0.5000000000", "large n precision case"
assert run("10 0.000001\n") == "0.9999900001", "small probability case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0.5`|`0.5000000000`| Kiểm tra trường hợp chuyển tiếp đối xứng. | 
|`1000000000 0.5`|`0.5000000000`| Kiểm tra rất lớn`n`được xử lý theo logarit. | 
|`10 0.000001`|`0.9999900001`| Kiểm tra độ chính xác với xác suất chuyển đổi rất nhỏ. | 

## Vỏ cạnh 

Đối với trường hợp một giây`1 0.7`, thuật toán tạo ma trận:```
[0.3 0.7]
[0.7 0.3]
```Số mũ là một nên ma trận không bình phương. Câu trả lời là giá trị trên cùng bên trái,`0.3`, thể hiện chính xác việc duy trì hạnh phúc sau một lần chuyển đổi. 

Đối với trường hợp hai giây`2 0.1`, thuật toán bình phương ma trận chuyển tiếp vì số mũ được biểu thị bằng giá trị nhị phân`10`. Ma trận bình phương chứa xác suất kết hợp của tất cả các đường dẫn hai bước, bao gồm chuyển đổi hai bước và không chuyển đổi chút nào. Giá trị trên cùng bên trái của nó trở thành`0.82`. 

Đối với một giá trị rất lớn như`1000000000 0.5`, mọi chuyển đổi đều có xác suất thay đổi hoặc giữ nguyên như nhau. Sau số giây dương bất kỳ, xác suất hạnh phúc là chính xác`0.5`. Phép lũy thừa nhị phân đạt được câu trả lời này mà không cần lặp lại từng giây một. 

Với những xác suất rất nhỏ như`10 0.000001`, phép nhân lặp đi lặp lại có thể mất độ chính xác nếu thực hiện bất cẩn. Phương pháp ma trận giữ phép tính theo số học dấu phẩy động tiêu chuẩn và tránh trừ các giá trị lớn gần bằng nhau, tạo ra kết quả nằm trong dung sai yêu cầu. 

Bạn có thể điều chỉnh độ sâu hoặc cách diễn đạt của bài xã luận nếu bạn muốn nó gần với phong cách hướng dẫn chính thức của Codeforces hoặc định dạng blog cuộc thi ngắn hơn.
