---
title: "CF 106415C - Vụ Trộm Siêu Thị Aziza"
description: "Tác vụ yêu cầu chúng ta tạo một chuỗi các số nguyên dương có thể hoạt động như một khóa bảo mật hợp lệ. Với độ dài n cho trước, chúng ta cần in n số trong đó tổng giá trị thu được bằng cách cộng tất cả các số hoàn toàn giống với giá trị thu được bằng cách nhân tất cả các số với nhau."
date: "2026-06-25T09:43:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106415
codeforces_index: "C"
codeforces_contest_name: "Winter Cup 8.0 Online Mirror Contest"
rating: 0
weight: 106415
solve_time_s: 37
verified: true
draft: false
---

[CF 106415C - Vụ cướp siêu thị Aziza](https://codeforces.com/problemset/problem/106415/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Tác vụ yêu cầu chúng ta tạo một chuỗi các số nguyên dương có thể hoạt động như một khóa bảo mật hợp lệ. Đối với một độ dài nhất định`n`, chúng ta cần in`n`những số trong đó tổng giá trị thu được bằng cách cộng tất cả các số hoàn toàn giống với giá trị thu được bằng cách nhân tất cả các số với nhau. Mọi số phải ở mức dương và thấp hơn`10^9`. Thẩm phán chấp nhận bất kỳ chuỗi nào thỏa mãn phương trình chứ không phải một câu trả lời cụ thể. 

Đầu vào chứa một số yêu cầu độc lập. Mỗi yêu cầu chỉ cung cấp độ dài mong muốn của chuỗi và đầu ra phải chứa một chuỗi hợp lệ cho mỗi độ dài. Vì tổng số độ dài được yêu cầu có thể lên tới hàng triệu nên giải pháp chỉ phải dành thời gian không đổi cho mỗi số được sản xuất. Bất kỳ cách tiếp cận nào tìm kiếm thông qua các giá trị có thể hoặc thử nhiều kết hợp sẽ nhanh chóng trở nên không thể thực hiện được. 

Khó khăn chính không phải là kiểm tra trình tự, vì điều đó rất dễ. Khó khăn là xây dựng một. Một ý tưởng ngây thơ có thể cố gắng xây dựng một tập hợp các số nhỏ và điều chỉnh chúng cho đến khi tổng và tích khớp nhau, nhưng ngay cả đối với một số lớn`n`, không gian tìm kiếm là rất lớn. Những ràng buộc thúc đẩy chúng ta tìm ra một mô hình toán học trực tiếp tạo ra câu trả lời. 

Một số trường hợp đặc biệt có thể phá vỡ việc triển khai chỉ xem xét mẫu chung. Vì`n = 1`, câu trả lời không thể sử dụng cấu trúc với hai số đặc biệt vì không có chỗ trống cho chúng. Đầu vào và đầu ra đúng là:```
Input:
1

Output:
1
```Một số phải bằng cả tổng và tích của chính nó, nên chỉ`1`hoạt động. 

Một trường hợp quan trọng khác là`n = 2`. Việc thực hiện bất cẩn có thể cố gắng in`[2, n]`cho mọi`n`, nhưng ở đây điều đó trở thành`[2, 2]`, điều này xảy ra có hiệu quả. Việc kiểm tra là:```
Input:
2

Output:
2 2
```Tổng số tiền là`4`và sản phẩm cũng`4`. 

Đối với các giá trị lớn hơn, vị trí của những giá trị đó rất quan trọng. Ví dụ, với`n = 5`, in ấn`[1, 2, 3, 4, 5]`có vẻ hợp lý nhưng không thành công vì tổng là`15`và sản phẩm là`120`. Việc xây dựng đúng mang lại:```
Input:
5

Output:
1 1 1 2 5
```Tổng số tiền là`10`và sản phẩm là`10`. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ cố gắng đoán các con số và kiểm tra xem tổng của chúng có bằng tích của chúng hay không. Thật dễ dàng để xác minh một ứng cử viên trong`O(n)`, nhưng việc tìm kiếm ứng viên lại là một vấn đề. Nếu chúng ta thử các giá trị đạt tới giới hạn nào đó cho mọi vị trí thì số khả năng sẽ tăng theo cấp số nhân với`n`. Với tổng kích thước đầu ra lên tới hàng triệu con số, ngay cả một tìm kiếm nhỏ trên mỗi trường hợp thử nghiệm cũng là quá chậm. 

Quan sát mở ra vấn đề là việc nhân với`1`không làm gì cả, trong khi thêm`1`chỉ tăng tổng lên một. Điều này có nghĩa là chúng ta có thể tự do thêm nhiều số để kiểm soát sự khác biệt giữa tổng và tích. 

Vì`n >= 2`, hãy cân nhắc việc đặt`n - 2`số một và chọn hai số còn lại. Gọi hai số đó là`x`Và`y`. Sản phẩm chỉ đơn giản là`xy`bởi vì tất cả những cái đó đều biến mất. Tổng số tiền là:`(n - 2) + x + y`Chúng tôi cần:`xy = (n - 2) + x + y`Sắp xếp lại mang lại:`xy - x - y = n - 2`Thêm một vào cả hai bên ở dạng hữu ích:`(x - 1)(y - 1) = n - 1`Có thể phân tích nhân tử rất đơn giản vì`1 * (n - 1) = n - 1`. Điều này có nghĩa là chúng ta có thể chọn`x = 2`Và`y = n`. Trình tự trở thành:`1, 1, ..., 1, 2, n`Bây giờ sản phẩm được`2n`. Tổng số tiền là`(n - 2) + 2 + n = 2n`, nên phương trình luôn đúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Số mũ trong n | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc độ dài`n`của trình tự cần thiết. 

Nếu như`n`là`1`, đầu ra`1`ngay lập tức. Một giá trị duy nhất phải thỏa mãn`a = a`, Vì thế`1`là sự lựa chọn duy nhất có thể. 

1. Nếu`n`ít nhất là`2`, tạo một chuỗi chứa`n - 2`bản sao của`1`. 

Các giá trị này rất hữu ích vì chúng không thay đổi tích nhưng cho phép tổng tăng chính xác một đơn vị cho mỗi giá trị được chèn. 

1. Nối hai giá trị còn lại`2`Và`n`. 

Sản phẩm trở nên`2 * n`. Tổng trở thành số đơn vị cộng với hai giá trị này, bằng`(n - 2) + 2 + n = 2n`. 

1. In trình tự đã xây dựng. 

Việc xây dựng chỉ sử dụng số nguyên dương và giá trị lớn nhất là`n`, nằm trong giới hạn yêu cầu. 

Tại sao nó hoạt động: điều bất biến là mỗi lần chèn vào`1`giữ cho sản phẩm không thay đổi trong khi tăng tổng lên một. Sau khi thêm tất cả những giá trị bắt buộc, hai giá trị cuối cùng được chọn để sản phẩm khớp chính xác với tổng mới. Sự đẳng thức không được tìm thấy bằng cách thử và sai, nó được đảm bảo bằng đồng nhất thức đại số`(2 - 1)(n - 1) = n - 1`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())

        if n == 1:
            ans.append("1")
        else:
            ans.append(" ".join(["1"] * (n - 2) + ["2", str(n)]))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Mã xử lý độ dài đặc biệt duy nhất,`n = 1`, riêng biệt. Đối với mọi trường hợp khác, nó xây dựng danh sách những cái đó và đính kèm hai giá trị đặc biệt từ việc xây dựng. 

biểu hiện`["1"] * (n - 2)`tạo ra chính xác số lượng phần tử trung tính cần thiết. Hai giá trị cuối cùng được thêm vào sau khi chuyển đổi`n`thành một chuỗi vì đầu ra được tập hợp dưới dạng văn bản. Vì số nguyên Python có độ chính xác tùy ý nên không có vấn đề tràn. 

Tổng khối lượng công việc tỷ lệ thuận với tổng kích thước đầu ra. Điều này phù hợp với yêu cầu là tổng của tất cả`n`các giá trị có thể lớn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
1
2
3
```Dấu vết: 

| n | Trình tự được tạo | Tổng hợp | Sản phẩm | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 2 2 | 4 | 4 | 
| 3 | 1 2 3 | 6 | 6 | 

Hàng đầu tiên hiển thị cách xử lý đặc biệt đối với độ dài nhỏ nhất có thể. Các hàng khác hiển thị cấu trúc chung. 

### Mẫu 2 

đầu vào:```
1
6
```Dấu vết: 

| Bước | Trình tự hiện tại | Tổng hợp | Sản phẩm | 
| --- | --- | --- | --- | 
| Sau những cái | 1 1 1 1 | 4 | 1 | 
| Thêm 2 | 1 1 1 1 2 | 6 | 2 | 
| Thêm n | 1 1 1 1 2 6 | 12 | 12 | 

Điều này chứng tỏ giá trị cuối cùng`n`bù đắp cho tất cả những gì thêm. Sản phẩm nhảy từ`2`ĐẾN`12`, chính xác đạt đến tổng cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng n) | Mỗi số đầu ra được tạo một lần trên tất cả các trường hợp thử nghiệm | 
| Không gian | O(tổng n) | Chuỗi câu trả lời lưu trữ kết quả đầu ra được yêu cầu trước khi in | 

Giải pháp này phù hợp một cách thoải mái vì nó không bao giờ thực hiện bất kỳ tìm kiếm hoặc tính toán lặp lại nào. Chi phí của nó là chi phí không thể tránh khỏi của việc viết chính kết quả đầu ra. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        if n == 1:
            out.append("1")
        else:
            out.append(" ".join(["1"] * (n - 2) + ["2", str(n)]))

    print("\n".join(out))

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# provided samples
assert run("3\n1\n2\n3\n") == "1\n2 2\n1 2 3\n", "sample 1"

# minimum sizes
assert run("2\n1\n2\n") == "1\n2 2\n", "minimum n"

# custom cases
assert run("1\n5\n") == "1 1 1 2 5\n", "five elements"
assert run("1\n10\n") == "1 1 1 1 1 1 1 1 2 10\n", "larger construction"
assert run("1\n3\n") == "1 2 3\n", "small general case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Xử lý trường hợp đặc biệt của một phần tử | 
|`2`|`2 2`| Kiểm tra trường hợp nhỏ nhất bằng công thức chung | 
|`5`|`1 1 1 2 5`| Xác nhận cấu trúc một lớp đệm | 
|`10`| Tám cái theo sau`2 10`| Kiểm tra độ dài lớn hơn và tạo đầu ra | 

## Vỏ cạnh 

cho`n = 1`, thuật toán không bao giờ cố gắng tạo`n - 2`những cái đó vì điều đó sẽ là tiêu cực. Nó trực tiếp trả về`1`, thỏa mãn điều kiện vì tổng và tích đều bằng một. 

Vì`n = 2`, thuật toán tạo ra số 0 và chỉ nối thêm`2`Và`n`. Trình tự trở thành`2 2`. Sản phẩm là`4`và tổng là`4`, do đó trường hợp ranh giới được bao phủ mà không cần thêm logic. 

Vì`n = 5`, thuật toán tạo ra ba cái và nối thêm`2`Và`5`. Cách tính tổng là`1 + 1 + 1 + 2 + 5 = 10`, trong khi sản phẩm là`1 * 1 * 1 * 2 * 5 = 10`. Điều này cho thấy tại sao số lượng cái phải chính xác`n - 2`.
