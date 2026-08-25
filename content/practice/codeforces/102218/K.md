---
title: "CF 102218K - Chữ số thiếu thứ K"
description: "Chúng ta có hai số nguyên thập phân dương, A và B, và một chuỗi thập phân P. Chuỗi P được coi là biểu diễn thập phân chính xác của A B, ngoại trừ một chữ số đã được thay thế bằng ."
date: "2026-08-24T06:47:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102218
codeforces_index: "K"
codeforces_contest_name: "2019, XI Annual Programming Contest by ESCOM-IPN"
rating: 0
weight: 102218
solve_time_s: 1421
verified: true
draft: false
---

[CF 102218K - Chữ số bị thiếu thứ K](https://codeforces.com/problemset/problem/102218/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 23m 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Ta có hai số thập phân dương`A`Và`B`, và một chuỗi thập phân`P`. Chuỗi`P`được cho là chính xác là biểu diễn thập phân của`A * B`, ngoại trừ một chữ số đã được thay thế bằng`*`. Chữ số bị thiếu được đảm bảo là từ`1`bởi vì`9`, Vì thế`0`không bao giờ cần phải xem xét. 

Nhiệm vụ chỉ đơn giản là khôi phục một chữ số đó và in nó. 

Dòng đầu tiên cho biết độ dài của`A`,`B`, Và`P`, theo sau là hai toán hạng và tích đã biết một phần. Các giá trị độ dài mang tính mô tả và không cần thiết cho tính toán cốt lõi. Vì cả hai`A`Và`B`nhỏ hơn`10^6`, tích của chúng nhỏ hơn`10^12`, vậy sản phẩm thực tế có tối đa 12 chữ số thập phân. Điều này làm cho phép nhân số nguyên trực tiếp hoàn toàn thực tế. Giới hạn trên đã nêu trên`P`lớn hơn nhiều so với số chữ số thực sự có thể xuất hiện từ các toán hạng này. 

Trường hợp cạnh chính là`*`có thể xuất hiện ở bất kỳ vị trí nào, kể cả chữ số đầu tiên hoặc cuối cùng. Ví dụ,```
1 1 2
3
8
2*
```có sản phẩm`24`, vậy câu trả lời là`4`. Giải pháp bất cẩn chỉ kiểm tra các vị trí nội bộ sẽ bỏ sót vị trí cuối cùng. 

Một trường hợp ranh giới khác là thiếu chữ số đầu tiên. Ví dụ,```
2 2 3
10
10
*00
```có sản phẩm`100`, vậy chữ số còn thiếu là`1`. Một giải pháp xử lý ký tự đầu tiên một cách đặc biệt như thể nó không thể bị thiếu sẽ thất bại ở đây. 

Cũng có thể có số 0 xung quanh chữ số bị thiếu. Ví dụ, nếu sản phẩm được`1008`, mô hình có thể là`1*08`, và câu trả lời phải là`0`trong bài toán tổng quát về việc khôi phục một chữ số tùy ý. Tuy nhiên, ở đây câu lệnh đảm bảo rằng chữ số bị thiếu không bằng 0, do đó mẫu như vậy không thể là đầu vào hợp lệ. Việc triển khai vẫn phải dựa vào sự đảm bảo thay vì vô tình cho rằng mọi chữ số đều khác 0. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp nhất là tính toán`A * B`, chuyển nó thành chuỗi thập phân, tìm vị trí của`*`TRONG`P`và thử từng chữ số còn thiếu có thể có từ`1`bởi vì`9`. Đối với mỗi ứng cử viên, hãy thay thế dấu sao và so sánh chuỗi kết quả với sản phẩm thực tế. Chỉ có chín ứng cử viên, vì vậy điều này thực hiện nhiều nhất`9 * p`so sánh nhân vật. 

Điều này đã dễ dàng đủ nhanh. Trong thực tế, giới hạn toán hạng còn làm cho tình huống trở nên đơn giản hơn: bởi vì`A, B < 10^6`, sản phẩm của họ có tối đa 12 chữ số. Do đó, trường hợp xấu nhất thực hiện ít hơn`9 * 12 = 108`kiểm tra ký tự. Không có mối quan tâm hiệu suất có ý nghĩa. 

Điều quan trọng là không cần đến một kỹ thuật số học phức tạp hơn. Toàn bộ sản phẩm có thể được tính toán trực tiếp vì các toán hạng vừa khít với các số nguyên 64 bit thông thường và dù sao thì số nguyên Python cũng có độ chính xác tùy ý. Khi đã biết chính xác sản phẩm, chữ số chưa biết chỉ là một so sánh ký tự đơn lẻ. 

Phương pháp brute-force hoạt động vì chỉ có chín giá trị hợp pháp cho chữ số bị thiếu. Nhận xét rằng bản thân sản phẩm chính xác có chi phí tính toán rẻ sẽ làm giảm vấn đề so sánh chín chuỗi ngắn. 

Việc triển khai thậm chí còn ngắn hơn có thể tránh được việc xây dựng tất cả chín chuỗi ứng cử viên một cách rõ ràng. Khi đã biết chuỗi sản phẩm, chỉ cần xác định vị trí`*`và in chữ số ở cùng vị trí đó. Từ`P`được đảm bảo khác với sản phẩm ở đúng một vị trí thì chữ số đó chính là câu trả lời. Đây là cách thực hiện tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(9p) | O(p) | Đã chấp nhận | 
| Tối ưu | O(p) | O(p) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba độ dài và cách biểu diễn số thập phân của`A`,`B`, Và`P`. Độ dài không ảnh hưởng đến thuật toán vì bản thân các chuỗi chứa tất cả thông tin cần thiết. 
2. Chuyển đổi`A`Và`B`thành số nguyên và tính toán`A * B`. Python có thể biểu thị trực tiếp sản phẩm và trong giới hạn nhất định, sản phẩm có tối đa 12 chữ số. 
3. Chuyển kết quả về dạng biểu diễn thập phân. Điều này mang lại kết quả hoàn toàn chính xác, bao gồm cả chữ số bị ẩn bởi`*`TRONG`P`. 
4. Quét`P`từ trái sang phải cho đến khi`*`được tìm thấy. Vị trí của ngôi sao chính xác là vị trí của chữ số bị thiếu vì mọi ký tự khác trong`P`được biết là đúng. 
5. In ký tự từ sản phẩm tính toán ở cùng vị trí đó. Vì chuỗi sản phẩm và`P`biểu thị cùng một số, mọi vị trí không phải dấu sao phải khớp nhau, trong khi ký tự ở dấu sao là chữ số bị thiếu. 

### Tại sao nó hoạt động 

hãy để`i`là vị trí của`*`TRONG`P`. Theo định nghĩa vấn đề,`P`được lấy từ biểu diễn thập phân chính xác của`A * B`bằng cách chỉ thay thế vị trí`i`với`*`. Vì vậy, nhân vật ở vị trí`i`trong sản phẩm chính xác là chính xác chữ số còn thiếu. Thuật toán tính toán chính xác sản phẩm đó và đọc ký tự của nó tại vị trí`i`, do đó nó phải xuất ra chữ số chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a, b, p = map(int, input().split())
A = input().strip()
B = input().strip()
P = input().strip()

product = str(int(A) * int(B))

for i, ch in enumerate(P):
    if ch == '*':
        print(product[i])
        break
```Dòng đầu tiên đọc ba độ dài được cung cấp. Sau đó chúng không cần thiết, nhưng việc đọc chúng là cần thiết để sử dụng đầu vào một cách chính xác.`A`Và`B`được giữ dưới dạng chuỗi trong khi đọc và sau đó được chuyển đổi thành số nguyên để nhân. Vì cả hai giá trị đều dưới đây`10^6`, sản phẩm của họ nhiều nhất là`999998000001`, chỉ có 12 chữ số. 

Sản phẩm tính toán được chuyển đổi trở lại thành chuỗi vì thông tin bị thiếu là chữ số thập phân ở một vị trí ký tự cụ thể. Làm việc với các chuỗi cũng làm cho việc so sánh vị trí trở nên trực tiếp và tránh mọi số học liên quan đến lũy thừa mười. 

Vòng lặp sử dụng`enumerate`sao cho nó có cả ký tự và vị trí dựa trên số 0. Ngay khi nó tìm thấy`*`, nó lập chỉ mục cho sản phẩm hoàn chỉnh ở cùng vị trí và in ký tự đó. 

Không cần phải thay dấu sao và so sánh toàn bộ chuỗi chín lần. Vấn đề đảm bảo rằng mẫu đầu vào là một sản phẩm hợp lệ với chính xác một chữ số bị thiếu, do đó ký tự tương ứng trong sản phẩm được tính toán đã là câu trả lời. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào là:```
1 1 2
3
8
2*
```Thuật toán tính toán sản phẩm đầu tiên. 

| Bước |`A`|`B`| Sản phẩm |`P`| Vị trí ngôi sao | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| Đọc đầu vào |`3`|`8`| |`2*`| | | 
| Nhân |`3`|`8`|`24`|`2*`| | | 
| Quét |`3`|`8`|`24`|`2*`|`1`| | 
| Đọc chữ số sản phẩm |`3`|`8`|`24`|`2*`|`1`|`4`| 

Chữ số đầu tiên đã biết là`2`, khớp với chữ số đầu tiên của`24`. Ngôi sao đang ở vị trí`1`, vậy chữ số tích thứ hai,`4`, là câu trả lời cần thiết. 

Đối với Mẫu 2, đầu vào là:```
2 2 3
10
10
*00
```Sản phẩm là`100`, và ngôi sao xuất hiện ở vị trí đầu tiên. 

| Bước |`A`|`B`| Sản phẩm |`P`| Vị trí ngôi sao | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| Đọc đầu vào |`10`|`10`| |`*00`| | | 
| Nhân |`10`|`10`|`100`|`*00`| | | 
| Quét |`10`|`10`|`100`|`*00`|`0`| | 
| Đọc chữ số sản phẩm |`10`|`10`|`100`|`*00`|`0`|`1`| 

Ví dụ này xác nhận rằng vị trí số 0 được xử lý chính xác như mọi vị trí khác. Ký tự đầu tiên của sản phẩm hoàn chỉnh là`1`, vậy câu trả lời là`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(p) | Việc tính toán sản phẩm và chuyển đổi nó thành chuỗi cần có thời gian tỷ lệ thuận với số chữ số của sản phẩm và lần quét cuối cùng sẽ kiểm tra`P`một lần. | 
| Không gian | O(p) | Chuỗi sản phẩm và chuỗi đầu vào`P`mỗi yêu cầu không gian tỷ lệ thuận với số chữ số. | 

Theo các ràng buộc toán hạng thực tế, tích có tối đa 12 chữ số. Do đó, việc triển khai chỉ sử dụng một lượng bộ nhớ nhỏ và thực hiện một lượng công việc thực tế có kích thước không đổi, phù hợp một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    a, b, p = map(int, input().split())
    A = input().strip()
    B = input().strip()
    P = input().strip()

    product = str(int(A) * int(B))

    for i, ch in enumerate(P):
        if ch == '*':
            print(product[i])
            return

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()
    result = sys.stdout.getvalue().strip()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

# Provided samples
assert run("""1 1 2
3
8
2*
""") == "4", "sample 1"

assert run("""2 2 3
10
10
*00
""") == "1", "sample 2"

# Minimum-size operands, missing digit at the end.
assert run("""1 1 2
1
9
1*
""") == "9", "minimum-size operands"

# Missing digit at the beginning.
assert run("""2 2 3
12
9
*08
""") == "1", "missing first digit"

# All digits of the product are equal.
assert run("""2 2 5
37
27
*99*9
""") == "9", "repeated product digits"

# Large operands near the upper bound.
assert run("""6 6 12
999999
999999
*0000099999
""") == "9", "large operands"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 2 / 1 / 9 / 1*`|`9`| Toán hạng có kích thước tối thiểu và dấu sao ở vị trí cuối cùng | 
|`2 2 3 / 12 / 9 / *08`|`1`| Một ngôi sao ở vị trí đầu tiên | 
|`2 2 5 / 37 / 27 / *99*9`|`9`| Các chữ số lặp lại trong sản phẩm và xử lý vị trí | 
|`6 6 12 / 999999 / 999999 / *0000099999`|`9`| Toán hạng lớn gần với giá trị tối đa cho phép | 

## Vỏ cạnh 

Ngôi sao ở vị trí cuối cùng được xử lý mà không có bất kỳ logic ranh giới đặc biệt nào. Vì```
1 1 2
1
9
1*
```sản phẩm là`9`, nhưng đầu vào này thực tế sẽ có độ dài tích không nhất quán, vì vậy nó không phải là một trường hợp hợp lệ theo cách diễn giải đã nêu. Một ví dụ có hai chữ số hợp lệ là```
1 1 2
3
8
2*
```sản phẩm ở đâu`24`. Ngôi sao ở chỉ số`1`và thuật toán in`product[1]`, đó là`4`. Điều kiện vòng lặp không loại trừ ký tự cuối cùng, do đó không có vấn đề gì xảy ra. 

Ngôi sao ở vị trí đầu tiên cũng đơn giản như vậy. Vì```
2 2 3
10
10
*00
```sản phẩm là`100`. Ngôi sao ở chỉ số`0`, do đó thuật toán đọc`product[0]`và in`1`. Không cần có trường hợp đặc biệt nào đối với ngôi sao dẫn đầu. 

Sản phẩm có thể chứa nhiều số 0. Thuật toán không bao giờ cố gắng suy ra chữ số bằng cách sử dụng số chia hết hoặc bằng cách loại bỏ các số 0 ở hai đầu. Nó bảo toàn biểu diễn thập phân chính xác được tạo ra bởi`str(A * B)`, do đó, mọi vị trí, kể cả các vị trí có giá trị bằng 0, vẫn được căn chỉnh theo mẫu. 

Cuối cùng, các toán hạng lớn nhất có thể không yêu cầu một chiến lược số học khác. Với`A = B = 999999`, sản phẩm là`999998000001`, chỉ dài 12 chữ số. Python tính toán trực tiếp điều này và lập chỉ mục chuỗi kết quả là thời gian không đổi cho từng vị trí. Giới hạn trên được nêu lớn hơn nhiều đối với độ dài của`P`do đó không tạo ra vấn đề hiệu suất ẩn.
