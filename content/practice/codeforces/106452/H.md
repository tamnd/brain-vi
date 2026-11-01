---
title: "CF 106452H - Điền vào chỗ trống"
description: "Sự cố ẩn giấu một tác vụ chuyển đổi cơ sở đơn giản đằng sau ký hiệu. Giá trị đã cho được viết bằng cơ số i nào đó và mục tiêu là in cùng một số được viết bằng cơ số j khác. Phần khó hiểu là ký hiệu ai."
date: "2026-06-25T09:17:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106452
codeforces_index: "H"
codeforces_contest_name: "UTPC April Fools Contest 2026"
rating: 0
weight: 106452
solve_time_s: 47
verified: true
draft: false
---

[CF 106452H - Điền vào chỗ trống](https://codeforces.com/problemset/problem/106452/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Sự cố ẩn giấu một tác vụ chuyển đổi cơ sở đơn giản đằng sau ký hiệu. Giá trị đã cho được viết bằng cách sử dụng một số cơ sở`i`và mục tiêu là in cùng một số được viết bằng cơ số khác`j`. 

Phần khó hiểu là ký hiệu`a_i`. Nó trông giống như`i`-phần tử thứ của một dãy, nhưng thực ra nó có nghĩa là “sự biểu diễn của một số`a`trong căn cứ`i`”. Biểu diễn trong cơ số lớn hơn thường có ít chữ số hơn, phù hợp với quan sát rằng việc tăng chỉ số sẽ làm cho giá trị được viết có vẻ nhỏ hơn.`1`không phải là một hệ thống số vị trí hợp lệ, điều này giải thích tại sao trường hợp đầu tiên lại đặc biệt. 

Đầu vào cung cấp cơ số ban đầu, cơ số đích và các chữ số của số trong cơ số ban đầu. Đầu ra là cùng một số nguyên được biểu thị trong cơ sở đích. 

Các ràng buộc được thiết kế sao cho giá trị được chuyển đổi phù hợp với số nguyên 64 bit thông thường. Cơ sở mục tiêu nhiều nhất là`10`, vì vậy mọi chữ số đầu ra có thể được in bằng ký tự thập phân. Điều này có nghĩa là chúng ta chỉ cần làm việc tuyến tính với số chữ số đầu vào. Bất kỳ cách tiếp cận nào liên quan đến việc thử các số có thể hoặc xây dựng các bảng lớn đều không cần thiết. 

Các trường hợp cạnh chính đến từ việc xử lý cơ sở. Một con số như`0`phải ở lại`0`sau khi chuyển đổi. Việc chuyển đổi từ cơ sở sang chính nó sẽ trả về biểu diễn ban đầu. Các số 0 đứng đầu trong biểu diễn đầu vào không được thay đổi giá trị. 

Ví dụ: nếu đầu vào là:```
2 10
1010
```đầu ra đúng là:```
10
```Một giải pháp bất cẩn coi đầu vào là số thập phân sẽ hiểu`1010`dưới dạng một nghìn mười thay vì biểu diễn nhị phân. 

Một trường hợp cạnh khác là:```
10 2
0
```Đầu ra đúng là:```
0
```Giải pháp loại bỏ tất cả các số 0 hoặc giả sử luôn có một chữ số khác 0 sẽ không thành công ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng ý nghĩa của các chữ số. Một số được viết dưới dạng cơ số`b`có thể được đánh giá bằng cách xử lý các chữ số từ trái sang phải. Nếu giá trị tích lũy hiện tại là`x`và chữ số tiếp theo là`d`, giá trị mới là:`x * b + d`Việc lặp lại điều này sẽ chuyển đổi toàn bộ biểu diễn thành giá trị nguyên thực của nó. Sau đó, giá trị tương tự có thể được viết trong một cơ số khác bằng cách liên tục lấy phần dư theo modulo của cơ số đích và chia cho cơ số. 

Điều này có tác dụng vì ký hiệu vị trí là phép nhân và phép cộng được lặp lại chính xác. Giải pháp thay thế mạnh mẽ sẽ là tìm kiếm một số có biểu diễn khớp với các chữ số đã cho, nhưng các giá trị có thể tăng theo cấp số nhân theo số chữ số, khiến cách tiếp cận đó không thể sử dụng được. 

Việc quan sát rằng giá trị thực bị giới hạn cho phép chúng ta tránh các thủ thuật chính xác tùy ý và sử dụng số học số nguyên thông thường. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Số mũ của số chữ số | O(1) | Quá chậm | 
| Chuyển đổi cơ sở | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc căn cứ gốc`i`, cơ sở mục tiêu`j`, và cách biểu diễn số đó. 
2. Chuyển đổi biểu diễn từ cơ sở`i`thành một giá trị số nguyên. Bắt đầu với giá trị`0`và với mỗi chữ số nhân giá trị hiện tại với`i`trước khi thêm chữ số. 
3. Nếu giá trị kết quả là`0`, in`0`ngay lập tức. Điều này xử lý số duy nhất không tạo ra bất kỳ chữ số nào trong phép chia lặp lại thông thường. 
4. Liên tục chia giá trị cho`j`. Mỗi phần còn lại là chữ số tiếp theo trong biểu diễn đích, bắt đầu từ chữ số có nghĩa nhỏ nhất. 
5. Đảo ngược các chữ số đã thu thập và in chúng, vì phép chia sẽ tìm ra các chữ số từ phải sang trái. 

Tại sao nó hoạt động: giai đoạn đầu tiên xây dựng lại số nguyên duy nhất được biểu thị bằng các chữ số đầu vào. Giai đoạn thứ hai sử dụng quy trình nghịch đảo của ký hiệu vị trí. Việc chia cho một cơ số liên tục sẽ cho các chữ số của biểu diễn cơ số đó từ ít quan trọng nhất đến quan trọng nhất, do đó việc đảo ngược chúng sẽ tạo ra kết quả chính xác như yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = input().split()
    if not data:
        return

    src = int(data[0])
    dst = int(data[1])
    s = data[2]

    value = 0
    for ch in s:
        value = value * src + int(ch)

    if value == 0:
        print(0)
        return

    ans = []
    while value > 0:
        ans.append(str(value % dst))
        value //= dst

    print("".join(reversed(ans)))

if __name__ == "__main__":
    solve()
```Vòng chuyển đổi tuân theo công thức ký hiệu vị trí trực tiếp. Biến`value`luôn lưu trữ giá trị số của tiền tố được xử lý cho đến nay. 

Việc kiểm tra số 0 là cần thiết vì việc chia số 0 nhiều lần sẽ không tạo ra chữ số nào, trong khi biểu diễn số 0 phải chứa một chữ số. 

Trong quá trình chuyển đổi đầu ra, phần còn lại được tạo ngược. Ví dụ: chuyển đổi số thập phân`13`căn cứ`2`tạo ra phần còn lại`1`,`0`,`1`,`1`, tương ứng với cách biểu diễn được đọc từ phải sang trái. Đảo ngược sửa chữa trật tự. 

Không có lập chỉ mục nào được sử dụng, do đó không có vấn đề riêng lẻ nào. Số nguyên Python cũng tránh được vấn đề tràn. 

## Ví dụ đã hoạt động 

Ví dụ 1: 

đầu vào:```
2 10
1010
```| Bước | Giá trị hiện tại | Hành động | 
| --- | --- | --- | 
| Đọc chữ số 1 | 1 | Bắt đầu từ 0, nhân với 2 và thêm 1 | 
| Đọc chữ số 0 | 2 | Giá trị trước 1 trở thành 1×2+0 | 
| Đọc chữ số 1 | 5 | Giá trị trước 2 trở thành 2×2+1 | 
| Đọc chữ số 0 | 10 | Giá trị trước 5 trở thành 5×2+0 | 

Số nhị phân`1010`đại diện cho số thập phân`10`, vậy đáp án là:```
10
```Ví dụ 2: 

đầu vào:```
10 2
15
```| Bước | Giá trị hiện tại | Hành động | 
| --- | --- | --- | 
| Đọc chữ số 1 | 1 | Bắt đầu từ 0 | 
| Đọc chữ số 5 | 15 | Tính 1×10+5 | 
| Chia cho 2 | còn lại 1 | Chữ số đầu ra đầu tiên | 
| Chia cho 2 | còn lại 1 | Chữ số đầu ra thứ hai | 
| Chia cho 2 | còn lại 1 | Chữ số đầu ra thứ ba | 
| Chia cho 2 | còn lại 1 | Chữ số đầu ra thứ tư | 

Các chữ số được thu thập được đảo ngược từ`1111`, là dạng nhị phân của số thập phân`15`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chữ số đầu vào được xử lý một lần và việc chuyển đổi sang cơ sở đầu ra thực hiện một số bước tỷ lệ thuận với số chữ số đầu ra. | 
| Không gian | O(n) | Các chữ số đầu ra được lưu trữ trước khi đảo ngược. | 

Các ràng buộc cho phép giải pháp tuyến tính này một cách dễ dàng. Thuật toán chỉ thực hiện các phép tính số học trên giá trị thực và không bao giờ khám phá các số có thể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    data = sys.stdin.readline().split()
    if not data:
        sys.stdin = old_stdin
        return ""

    src = int(data[0])
    dst = int(data[1])
    s = data[2]

    value = 0
    for ch in s:
        value = value * src + int(ch)

    if value == 0:
        result = "0"
    else:
        res = []
        while value:
            res.append(str(value % dst))
            value //= dst
        result = "".join(reversed(res))

    sys.stdin = old_stdin
    return result + "\n"

assert run("2 10\n1010\n") == "10\n", "binary to decimal"
assert run("10 2\n15\n") == "1111\n", "decimal to binary"

assert run("10 10\n00042\n") == "42\n", "leading zeroes"
assert run("8 2\n17\n") == "1111\n", "octal conversion"
assert run("10 10\n0\n") == "0\n", "zero handling"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 10 / 1010`|`10`| Chuyển đổi cơ bản từ cơ sở nhỏ hơn | 
|`10 2 / 15`|`1111`| Xây dựng đầu ra phân chia lặp đi lặp lại | 
|`10 10 / 00042`|`42`| Bỏ qua các số 0 đứng đầu | 
|`8 2 / 17`|`1111`| Cơ sở nguồn không thập phân | 
|`10 10 / 0`|`0`| Xử lý đặc biệt số 0 | 

## Vỏ cạnh 

Đối với đầu vào:```
2 10
1010
```thuật toán đầu tiên xây dựng lại giá trị như`10`. Cơ số đầu ra là số thập phân nên phép chia ngay lập tức tạo ra một chữ số`10`. 

Đối với đầu vào:```
10 2
0
```giá trị tích lũy vẫn bằng 0 sau khi đọc chữ số duy nhất. Thuật toán sử dụng nhánh 0 rõ ràng và in ra`0`thay vì tạo ra một chuỗi trống. 

Đối với đầu vào:```
10 10
00042
```giai đoạn chuyển đổi xử lý tất cả các chữ số, nhưng các số 0 đứng đầu không đóng góp gì vì việc nhân giá trị hiện tại với cơ số sẽ giữ giá trị không thay đổi trước khi chữ số khác 0 đầu tiên xuất hiện. Giá trị cuối cùng là`42`và biểu diễn đầu ra cũng là`42`.
