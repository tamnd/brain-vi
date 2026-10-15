---
title: "CF 105444M - Phép nhân theo phương pháp"
description: "Cho ta hai số tự nhiên nhưng chúng không được viết dưới dạng thập phân. Thay vào đó, mỗi số được mã hóa bằng số học Peano, trong đó một số được biểu diễn dưới dạng các ứng dụng lặp lại của hàm kế thừa được áp dụng cho số 0."
date: "2026-06-23T03:33:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105444
codeforces_index: "M"
codeforces_contest_name: "2020-2021 ACM-ICPC Nordic Collegiate Programming Contest (NCPC 2020)"
rating: 0
weight: 105444
solve_time_s: 53
verified: true
draft: false
---

[CF 105444M - Phép nhân theo phương pháp](https://codeforces.com/problemset/problem/105444/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Cho ta hai số tự nhiên nhưng chúng không được viết dưới dạng thập phân. Thay vào đó, mỗi số được mã hóa bằng số học Peano, trong đó một số được biểu diễn dưới dạng các ứng dụng lặp lại của hàm kế thừa được áp dụng cho số 0. Cụ thể số 0 được viết là`0`, một là`S(0)`, hai là`S(S(0))`, vân vân. 

Nhiệm vụ là đọc hai cách biểu diễn như vậy và tính tích của chúng, sau đó đưa ra kết quả ở cùng định dạng ký hiệu. 

Mặc dù đầu vào trông giống như ký hiệu hàm lồng nhau nhưng không cần phải mô phỏng trực tiếp phép nhân Peano. Giá trị thực của mỗi số chỉ đơn giản là số lượng`S(`giấy gói xuất hiện xung quanh trận chung kết`0`. Vì vậy, mỗi chuỗi đầu vào mã hóa một số nguyên bằng độ sâu lồng nhau của nó. 

Đầu ra lại phải là đại diện Peano của sản phẩm, nghĩa là chúng ta phải in`S`đã áp dụng điều đó nhiều lần để`0`, trong đó số đếm bằng tích của hai số nguyên được giải mã. 

Giới hạn kích thước đầu vào tối đa 1000 ký tự trên mỗi số có nghĩa là số nguyên được giải mã tối đa là 1000. Sản phẩm của họ tối đa là 1.000.000, đủ nhỏ để xây dựng trực tiếp ở dạng đầu ra mà không phải lo ngại về tràn hoặc các vấn đề về hiệu suất. 

Một sai lầm ngây thơ là cố gắng đánh giá biểu tượng theo nghĩa đen của các tiên đề Peano. Ví dụ: mở rộng phép nhân đệ quy bằng cách sử dụng định nghĩa`x · S(y) = x · y + x`sẽ dẫn đến sự bùng nổ theo cấp số nhân trong các lần bổ sung lặp đi lặp lại. Ngay cả đệ quy tuyến tính cho mỗi thao tác cũng trở thành chi phí không cần thiết vì cấu trúc đã mã hóa trực tiếp giá trị số. 

Cạm bẫy phổ biến thứ hai là phân tích sai chuỗi. Cấu trúc được đảm bảo ở dạng tốt, nhưng người ta chỉ phải đếm chính xác`S`lần xuất hiện, bỏ qua dấu ngoặc đơn và số không. Bất kỳ sự không khớp nào trong việc đếm đều dẫn đến việc tái tạo số không chính xác. 

Các trường hợp cạnh bao gồm: 

Một đĩa đơn`0`nhân với bất cứ thứ gì sẽ mang lại`0`. Ví dụ, đầu vào`0`Và`S(S(0))`phải sản xuất`0`. Một trình phân tích cú pháp bất cẩn giả định ít nhất một`S`có thể xuất ra không chính xác`S(0)`. 

Một trường hợp khác là lồng tối đa như 1000`S`ứng dụng. Nếu cả hai đầu vào đều tối đa thì đầu ra phải chứa một triệu`S(`giấy gói. Bất kỳ việc xây dựng chuỗi đệ quy nào mà không xử lý sự lặp lại cẩn thận đều có nguy cơ gây tràn ngăn xếp hoặc chi phí nối quá mức. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thực hiện trực tiếp phép nhân Peano bằng cách sử dụng các định nghĩa đệ quy của nó. Đầu tiên người ta sẽ thực hiện phép cộng bằng cách áp dụng nhiều lần`x + S(y) = S(x + y)`, sau đó nhân bằng phép cộng lặp lại bằng cách sử dụng`x · S(y) = x · y + x`. Điều này trung thực và chính xác về mặt toán học nhưng lại tốn kém về mặt vận hành. 

Nếu x và y đều có kích thước n ở dạng Peano, phép nhân sẽ mở rộng thành y cộng của x và bản thân mỗi phép cộng có thể đi qua toàn bộ cấu trúc của x. Điều này dẫn đến độ phức tạp trong trường hợp xấu nhất theo thứ tự của các phép toán tượng trưng O(n²) và tệ hơn trong thực tế do chi phí tái cấu trúc chuỗi hoặc cây. Với kích thước đầu vào lên tới 1000, điều này là quá mức cần thiết. 

Quan sát quan trọng là ký hiệu Peano là đơn nhất. Mỗi số được mã hóa theo đúng nghĩa đen dưới dạng giá trị của nó, do đó việc giải mã chỉ đơn giản là đếm các ký hiệu. Khi cả hai giá trị được khôi phục dưới dạng số nguyên, phép nhân sẽ trở thành một phép toán số học đơn lẻ. Kết quả sau đó được mã hóa lại bằng cách lặp lại`S(`chính xác số lần sản phẩm. 

Điều này làm giảm toàn bộ vấn đề thành phân tích cú pháp tuyến tính cộng với việc xây dựng đầu ra tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đệ quy Brute Force Peano | O(n²) | O(n²) | Quá chậm | 
| Đếm + xây dựng | O(n + xy) | O(xy) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc từng chuỗi đầu vào và diễn giải nó dưới dạng một chuỗi mã thông báo. Cấu trúc luôn có dạng lặp lại`S(`theo sau là một đĩa đơn`0`rồi đóng dấu ngoặc đơn. Thông tin có ý nghĩa duy nhất là bao nhiêu lần`S(`xuất hiện trước`0`. Số này là giá trị số. 
2. Quét chuỗi đầu tiên và đếm số lần xuất hiện của mẫu chuỗi con tương ứng với ứng dụng kế tiếp. Mỗi số hợp lệ đóng góp chính xác một`S`, do đó số đếm trực tiếp cho x. Điều này hoạt động vì biểu diễn Peano là mã hóa đơn nhất của số nguyên. 
3. Lặp lại quy trình tương tự cho chuỗi thứ hai để thu được y. 
4. Tính tích z = x × y bằng phép nhân số nguyên tiêu chuẩn. Vì x và y nhiều nhất là 1000, nên z nhiều nhất là 1.000.000, điều này là an toàn. 
5. Xây dựng chuỗi đầu ra bằng cách lặp lại mẫu`S(`chính xác z lần, theo sau là`0`, theo sau là dấu ngoặc đơn đóng z`)`. 
6. Xuất chuỗi đã tạo. 

### Tại sao nó hoạt động 

Biểu diễn Peano có cấu trúc đẳng cấu với các số nguyên đơn phân, trong đó mỗi ứng dụng của hàm kế thừa sẽ tăng giá trị lên đúng một. Bước phân tích cú pháp sẽ trích xuất độ dài đơn phân này. Vì phép nhân được xác định trên các số tự nhiên không phụ thuộc vào biểu diễn nên khi các giá trị được khôi phục chính xác, số học có thể được thực hiện ở dạng số nguyên tiêu chuẩn. Bước xây dựng lại duy trì tính chính xác vì mỗi bước kế tiếp tương ứng chính xác với một trình bao bọc cấu trúc trong mã hóa ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def parse_peano(s: str) -> int:
    # count occurrences of 'S'
    return s.count('S')

a = input().strip()
b = input().strip()

x = parse_peano(a)
y = parse_peano(b)

z = x * y

# build Peano representation
# S(S(...S(0)...))
print("S(" * z + "0" + ")" * z)
```Hàm phân tích cú pháp dựa trên thực tế là mọi số chỉ được xây dựng từ các số lặp lại`S(`giấy gói, cứ thế đếm`S`tương đương với việc giải mã giá trị số nguyên. Không cần xác thực cấu trúc ngoài việc tin cậy vào định dạng đầu vào. 

Phép nhân được thực hiện bằng cách sử dụng các số nguyên gốc, hiệu quả với các giới hạn nhỏ. 

Việc xây dựng đầu ra là bước nặng nề duy nhất. Nó tạo ra một chuỗi có kích thước O(z), điều này là không thể tránh khỏi vì bản thân đầu ra cũng lớn như vậy. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
S(S(0))
S(S(S(0)))
```Ở đây số đầu tiên có hai`S`ứng dụng, do đó x = 2. Ứng dụng thứ hai có ba ứng dụng, do đó y = 3. Do đó z = 6. 

| Bước | Giá trị đầu tiên x | Giá trị thứ hai y | Sản phẩm z | 
| --- | --- | --- | --- | 
| Phân tích | 2 | 3 | - | 
| Nhân | - | - | 6 | 
| Xây dựng đầu ra | - | - | S lặp lại 6 lần | 

Đầu ra:```
S(S(S(S(S(S(0)))))))
```Điều này xác nhận rằng phép nhân đơn phân tương ứng với việc ghép các lớp kế tiếp. 

### Mẫu 2 

đầu vào:```
S(S(S(S(S(0)))))
S(0)
```Ở đây x = 5 và y = 1, nên z = 5. 

| Bước | x | y | z | 
| --- | --- | --- | --- | 
| Phân tích | 5 | 1 | - | 
| Nhân | - | - | 5 | 
| Xây dựng đầu ra | - | - | giấy gói 5S | 

Đầu ra:```
S(S(S(S(S(0)))))
```Điều này thể hiện tính chất đồng nhất của phép nhân với một, được bảo toàn trực tiếp trong cấu trúc đơn phân. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + xy) | quét tuyến tính để đếm S cộng với việc xây dựng đầu ra tỷ lệ thuận với kích thước kết quả | 
| Không gian | O(xy) | chuỗi đầu ra chi phối việc sử dụng bộ nhớ | 

Các ràng buộc cho phép tối đa 1000 ký hiệu cho mỗi đầu vào, vì vậy x và y tối đa là 1000. Do đó, kích thước đầu ra tối đa là 1.000.000 ký tự, có thể chấp nhận được trong môi trường Codeforces điển hình cho một chuỗi đầu ra. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def parse_peano(s: str) -> int:
        return s.count('S')

    a = input().strip()
    b = input().strip()

    x = parse_peano(a)
    y = parse_peano(b)
    z = x * y

    return "S(" * z + "0" + ")" * z

# provided samples (interpreted)
assert run("S(S(0))\nS(S(S(0)))\n") == "S(" * 6 + "0" + ")" * 6

# minimum case
assert run("0\n0\n") == "0"

# identity case
assert run("S(0)\nS(S(S(0)))\n") == "S(" * 3 + "0" + ")" * 3

# zero times anything
assert run("0\nS(S(S(S(0))))\n") == "0"

# large-ish balanced case
assert run("S(" * 100 + "0\n" + "S(" * 100 + "0\n") == "S(" * 10000 + "0" + ")" * 10000
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0, 0 | 0 | hấp thụ bằng không | 
| S(0), S(S(S(0))) | SSSS...0 | phép nhân cơ bản | 
| 0, khác không | 0 | trường hợp chính xác của cạnh | 
| 100S và 100S | sản lượng cân bằng lớn | hiệu suất và mở rộng quy mô | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một trong hai đầu vào chính xác`0`. Trong trường hợp đó, giá trị được phân tích bằng 0, do đó kết quả bằng 0. Thuật toán tạo ra`"S(" * 0 + "0" + ")" * 0`, nó sụp đổ hoàn toàn thành`"0"`. Điều này tránh tạo ra cấu trúc không hợp lệ. 

Một trường hợp khác là lồng tối đa. Nếu cả hai đầu vào chứa 1000 liên tiếp`S(`ứng dụng, phân tích cú pháp mang lại x = 1000 và y = 1000, do đó z = 1.000.000. Bước xây dựng tạo ra một triệu lần lặp lại`"S("`Và`")"`, bao quanh một đơn`"0"`. Việc thực thi vẫn duy trì tuyến tính ở kích thước đầu ra và tính chính xác xuất phát từ ngữ nghĩa lặp lại trực tiếp của mã hóa đơn nhất. 

Một trường hợp tinh tế là trực giác không đúng về việc đếm dấu ngoặc đơn thay vì`S`. Chỉ một`S`vấn đề, vì dấu ngoặc đơn là tiếng ồn cấu trúc. Thuật toán vẫn đúng ngay cả khi dấu ngoặc đơn thay đổi, miễn là`S`count khớp với định nghĩa mã hóa.
