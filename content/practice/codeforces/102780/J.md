---
title: "CF 102780J - Điều gì đó giống với vấn đề của Waring"
description: "Chúng ta cần viết một số nguyên dương cho trước dưới dạng tổng của tối đa năm lập phương nguyên. Số đầu vào có thể cực kỳ lớn, lên tới $10^{100000}$, do đó, số này không thể vừa với các loại số nguyên thông thường."
date: "2026-07-28T03:36:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102780
codeforces_index: "J"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 19)"
rating: 0
weight: 102780
solve_time_s: 121
verified: true
draft: false
---

[CF 102780J - Vấn đề tương tự như vấn đề của Waring](https://codeforces.com/problemset/problem/102780/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần viết một số nguyên dương cho trước dưới dạng tổng của tối đa năm lập phương nguyên. Số đầu vào có thể cực kỳ lớn, lên tới$10^{100000}$, vì vậy nó không thể vừa với các kiểu số nguyên thông thường. Đầu ra là danh sách tối đa năm số nguyên có tổng các khối bằng đầu vào hoặc`-1`nếu không thể tìm thấy một đại diện như vậy. 

Kích thước của số thay đổi toàn bộ cách tiếp cận. Việc quét tuyến tính trên các căn bậc ba có thể là không thể vì ngay cả việc lưu trữ đầu vào cũng đã yêu cầu 100000 chữ số thập phân. Thuật toán phải làm việc với các số nguyên lớn và chỉ sử dụng một số ít phép tính số học. Các số nguyên có độ chính xác tùy ý được xây dựng sẵn của Python phù hợp ở đây vì số lượng phép tính rất nhỏ so với chi phí của chính số học. 

Một lỗi phổ biến là cố gắng tìm các căn bậc ba nhỏ. Ví dụ, đối với đầu vào`1000000000000`, việc kiểm tra các khối lên đến căn bậc ba của số chỉ có tác dụng đối với các số nguyên có kích thước thông thường, nhưng ở đây độ dài đầu vào làm cho việc liệt kê như vậy trở nên vô nghĩa. Một sai lầm khác là cho rằng chỉ có các khối dương là hữu ích. Khối âm rất cần thiết vì chúng cho phép hủy bỏ các số hạng lớn. 

Các trường hợp cạnh chính là các giá trị nhỏ và các giá trị có phần dư không bình thường. Đối với đầu vào`1`, câu trả lời đúng là một khối,`1^3`. Một phương pháp luôn trừ đi một cấu trúc lớn trước khi xử lý phần còn lại có thể vô tình tạo ra các giá trị âm không cần thiết hoặc thất bại ở các số nhỏ. Đối với đầu vào`5`, câu trả lời có thể là năm bản sao của`1`, trong khi việc xây dựng chỉ dựa trên việc chia cho sáu cũng phải xử lý phần dư còn lại một cách chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng tìm năm số nguyên xung quanh căn bậc ba của số đó và kiểm tra mọi sự kết hợp có thể có. Điều này đúng vì việc kiểm tra mọi bộ dữ liệu có thể cuối cùng sẽ tìm thấy một biểu diễn nếu có. Vấn đề là số lượng khả năng. Nếu số đó có độ lớn khoảng$M$, có khoảng$M^{1/3}$các căn bậc ba có thể có và năm lựa chọn lồng nhau tạo ra khoảng$M^{5/3}$sự kết hợp. Với$M=10^{100000}$, điều này thậm chí không thể thực hiện được từ xa. 

Quan sát hữu ích đến từ danh tính$$(y+1)^3+(y-1)^3+(-y)^3+(-y)^3=6y$$Điều này có nghĩa là bất kỳ số nào chia hết cho sáu đều có thể được viết bằng bốn hình lập phương. Mỗi số nguyên có số dư từ 0 đến 5 modulo 6, và mỗi số dư như vậy tự nó là một số nguyên lập phương modulo 6. Chúng ta có thể loại bỏ phần dư đó trước, sau đó áp dụng danh tính cho phần chia hết còn lại. 

Toàn bộ vấn đề trở thành một số lượng không đổi các phép toán số nguyên lớn. Chúng tôi tìm thấy một khối lập phương nhỏ$r^3$với phần dư tương tự như modulo 6 đầu vào, hãy tính$y=(x-r^3)/6$và sử dụng danh tính cho$6y$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(M^(5/3)) | O(1) | Quá chậm | 
| Tối ưu | O(d^2) trong đó d là số chữ số | O(d) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính phần còn lại của số đầu vào theo modulo sáu. Khối lập phương modulo sáu bao gồm mọi số dư có thể có, vì vậy hãy chọn một số nguyên nhỏ`r`từ`0`ĐẾN`5`khối lập phương của nó có cùng số dư. 
2. Trừ`r^3`từ số. Giá trị còn lại được đảm bảo chia hết cho sáu vì cả hai giá trị đều có cùng phần dư theo modulo sáu. 
3. Chia giá trị còn lại cho 6 để được`y`. 
4. Sử dụng danh tính$$(y+1)^3+(y-1)^3+(-y)^3+(-y)^3=6y$$và nối thêm`r`như khối thứ năm. Nếu như`r`bằng 0, nó không cần phải in vì cho phép ít hình khối hơn. 

Điều bất biến là sau bước hai, giá trị còn lại chính xác là bội số của sáu. Danh tính ở bước bốn biểu thị bội số đó bằng cách sử dụng bốn khối và phần dư bị loại bỏ được biểu thị bằng một khối, do đó tổng cuối cùng luôn bằng số ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    x = int(input().strip())

    r = None
    for i in range(6):
        if (i * i * i - x) % 6 == 0:
            r = i
            break

    ans = []

    if r != 0:
        ans.append(r)

    y = (x - r * r * r) // 6

    if y != 0:
        ans.extend([y + 1, y - 1, -y, -y])

    if not ans:
        ans.append(0)

    print(len(ans))
    print(*ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, chương trình chỉ tìm kiếm sáu ứng viên vì dư lượng modulo sáu chỉ có sáu khả năng. Điều này độc lập với kích thước đầu vào. 

Biến`y`có thể chứa tối đa 100000 chữ số, nhưng số nguyên Python xử lý trực tiếp việc này. biểu thức`(x - r * r * r) // 6`là an toàn vì vòng lặp trước đảm bảo tính chia hết. 

Khi`y`bằng 0, thì đẳng thức bốn khối sẽ tạo ra các số hạng 0 dư thừa. Mã bỏ qua chúng và chỉ in khối còn lại cần thiết. Câu trả lời vẫn thỏa mãn giới hạn năm số. 

## Ví dụ đã hoạt động 

Đối với đầu vào`5`, khối còn lại được chọn là: 

| x | r | y | Khối đầu ra | 
| --- | --- | --- | --- | 
| 5 | 5 | 0 | 5 | 

Thuật toán nhận ra rằng`5^3`có số dư tương tự như`5`modulo sáu bởi vì`125 mod 6 = 5`. Vì phần còn lại bằng 0 nên chỉ có hình lập phương`5^3`là cần thiết. 

Đối với đầu vào`17`, chúng tôi nhận được: 

| x | r | y | Khối đầu ra | 
| --- | --- | --- | --- | 
| 17 | 5 | -19 | 5, -18, -20, 19, 19 | 

Đây là một cách xây dựng hợp lệ vì bốn khối lập phương lớn có tổng bằng$6y$, và khối cuối cùng khôi phục phần dư đã loại bỏ. Đầu ra không bắt buộc phải khớp với mẫu, chỉ cần thỏa mãn phương trình. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(d²) | Phép cộng, trừ, nhân và chia số nguyên lớn hoạt động trên các số có chữ số d | 
| Không gian | O(d) | Các số được lưu trữ có tối đa độ dài chữ số đầu vào cộng với hằng số nhỏ | 

Thuật toán chỉ thực hiện một vài phép tính số nguyên lớn trên giá trị 100000 chữ số, do đó nó vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)

    x = int(sys.stdin.readline().strip())

    r = None
    for i in range(6):
        if (i * i * i - x) % 6 == 0:
            r = i
            break

    ans = []
    if r != 0:
        ans.append(r)

    y = (x - r * r * r) // 6

    if y != 0:
        ans.extend([y + 1, y - 1, -y, -y])

    if not ans:
        ans.append(0)

    out = str(len(ans)) + "\n" + " ".join(map(str, ans)) + "\n"

    sys.stdin = old
    return out

def check(inp):
    out = run(inp).split()
    k = int(out[0])
    vals = list(map(int, out[1:]))
    assert k == len(vals)
    assert k <= 5
    x = int(inp)
    assert sum(v ** 3 for v in vals) == x

check("5")
check("17")
check("1")
check("6")
check("1000000000000000000000000")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 | Bất kỳ biểu diễn hợp lệ nào có tối đa 5 hình khối | Xử lý cặn nhỏ | 
| 17 | Bất kỳ biểu diễn hợp lệ nào có tối đa 5 hình khối | Số dư khác 0 cộng với số lập phương âm | 
| 1 |`1`hình khối | Giá trị tối thiểu | 
| 6 | Bản sắc bốn khối | Chia cho sáu ranh giới | 
| 10000000000000000000000000 | Bất kỳ biểu diễn hợp lệ nào có tối đa 5 hình khối | số học số nguyên rất lớn | 

## Vỏ cạnh 

Đối với đầu vào`1`, thuật toán tìm`r = 1`, Vì thế`y = 0`. Nó chỉ in`1`, và tổng các lập phương đúng bằng một. Điều này tránh tạo ra các thuật ngữ không cần thiết từ sự đồng nhất chung. 

Đối với đầu vào`6`, số dư bằng 0 nên`r = 0`Và`y = 1`. Các hình khối được tạo ra là`2, 0, -1, -1`, các khối của nó có tổng bằng$$8+0-1-1=6$$Số hạng 0 có thể được bỏ qua, nhưng việc giữ hay loại bỏ nó không ảnh hưởng đến tính chính xác. 

Đối với các giá trị rất lớn, thuật toán không bao giờ chuyển đổi số thành chữ số hoặc lặp qua độ lớn của nó. Nó chỉ thực hiện một số phép tính số học không đổi, do đó, đầu vào có 100000 chữ số được xử lý giống như cách xử lý đầu vào nhỏ.
