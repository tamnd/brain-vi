---
title: "CF 451B - Sắp xếp mảng"
description: "Chúng ta được cho một mảng các số nguyên phân biệt. Nhiệm vụ là xác định xem toàn bộ mảng có thể được sắp xếp theo thứ tự tăng dần sau khi đảo ngược chính xác một phân đoạn liền kề hay không. Nếu có thể, chúng ta phải in \"có\" và một phân đoạn hợp lệ [l, r] có sự đảo ngược sắp xếp mảng."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 451
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 258 (Div. 2)"
rating: 1300
weight: 451
solve_time_s: 99
verified: true
draft: false
---

[CF 451B - Sắp xếp mảng](https://codeforces.com/problemset/problem/451/B) 

**Đánh giá:** 1300 
**Tags:** triển khai, sắp xếp 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên phân biệt. Nhiệm vụ là xác định xem toàn bộ mảng có thể được sắp xếp theo thứ tự tăng dần sau khi đảo ngược chính xác một phân đoạn liền kề hay không. 

Nếu có thể, chúng ta phải in`"yes"`và một phân đoạn hợp lệ`[l, r]`sự đảo ngược của nó sắp xếp mảng. Nếu không thể, chúng tôi in`"no"`. 

Kích thước mảng có thể lớn như$10^5$. Ở quy mô này, các thuật toán sao chép hoặc sắp xếp nhiều lần các phần lớn của mảng trở nên đắt đỏ. Một giải pháp có hành vi bậc hai, chẳng hạn như thử mọi phân đoạn có thể và kiểm tra kết quả, sẽ yêu cầu khoảng$10^{10}$trong trường hợp xấu nhất vượt xa thời hạn. Tuyến tính hoặc$O(n \log n)$giải pháp dễ dàng đủ nhanh. 

Một số trường hợp đáng chú ý. 

Hãy xem xét một mảng đã được sắp xếp:```
5
1 2 3 4 5
```Câu trả lời đúng là:```
yes
1 1
```Đảo ngược một đoạn có độ dài không thay đổi gì nên yêu cầu vẫn được thỏa mãn. Việc thực hiện bất cẩn có thể in sai`"no"`vì không có phần giảm nào tồn tại. 

Hãy xem xét một mảng hoàn toàn đảo ngược:```
3
3 2 1
```Câu trả lời đúng là:```
yes
1 3
```Toàn bộ mảng tạo thành một phân đoạn giảm dần và đảo ngược nó sẽ sắp xếp mảng. 

Xem xét nhiều nghịch đảo riêng biệt:```
5
3 1 2 5 4
```Câu trả lời đúng là:```
no
```Không có sự đảo ngược liền kề nào có thể khắc phục được cả hai vị trí rối loạn. Việc triển khai chỉ nhìn vào lần đảo ngược đầu tiên có thể chấp nhận nhầm trường hợp này. 

Hãy xem xét một đoạn giảm dần chạm vào điểm cuối:```
5
1 5 4 3 6
```Câu trả lời đúng là:```
yes
2 4
```Việc kiểm tra ranh giới xung quanh đoạn đảo ngược phải được xử lý cẩn thận vì một bên có thể không tồn tại. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ đơn giản sẽ thử mọi phân khúc có thể$[l,r]$. Đối với mỗi lựa chọn, hãy đảo ngược phân đoạn đó, kiểm tra xem mảng kết quả đã được sắp xếp chưa và dừng lại nếu tìm thấy phân đoạn hợp lệ. 

Cách tiếp cận này đúng vì nó kiểm tra rõ ràng mọi hoạt động được phép. Vấn đề là thời gian chạy. có$O(n^2)$các phân đoạn có thể. Kiểm tra xem mảng kết quả có được sắp xếp chi phí hay không$O(n)$. Tổng độ phức tạp trở thành$O(n^3)$, điều này hoàn toàn không thể thực hiện được đối với$n=10^5$. 

Quan sát quan trọng là nếu một sự đảo ngược duy nhất có thể sắp xếp mảng thì tất cả sự hỗn loạn phải tập trung vào một khối giảm liền kề. 

Một cách hữu ích để thấy điều này là so sánh mảng với phiên bản đã được sắp xếp của nó. Vì tất cả các giá trị đều khác nhau nên mỗi phần tử đều có một vị trí chính xác duy nhất. Nếu việc đảo ngược một phân đoạn sẽ sửa được mảng thì các vị trí mà mảng ban đầu khác với mảng đã sắp xếp phải tạo thành một khoảng liên tục. 

Thay vì kiểm tra tất cả các đảo ngược, chúng ta có thể xây dựng phiên bản đã sắp xếp của mảng. Sau đó, chúng tôi tìm vị trí đầu tiên nơi các mảng khác nhau và vị trí cuối cùng nơi chúng khác nhau. Các vị trí này xác định phân khúc ứng cử viên duy nhất đáng để đảo ngược. 

Nếu đảo ngược phân đoạn đó tạo ra mảng đã được sắp xếp thì câu trả lời là`"yes"`. Ngược lại, không có phân đoạn hợp lệ nào tồn tại. 

Điều này làm giảm vấn đề xuống còn một loại và một xác minh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và tạo một bản sao được sắp xếp của nó. 
2. Tìm tất cả các vị trí mà mảng ban đầu khác với mảng đã sắp xếp. 
3. Nếu không có vị trí nào khác nhau thì mảng đã được sắp xếp. đầu ra`"yes"`và phân khúc`1 1`. 
4. Nếu không, hãy để`l`là vị trí khác nhau đầu tiên và`r`là vị trí khác nhau cuối cùng. 
5. Đảo ngược đoạn từ`l`ĐẾN`r`trong một bản sao của mảng ban đầu. 
6. So sánh mảng đã sửa đổi với mảng đã sắp xếp. 
7. Nếu chúng giống hệt nhau, xuất ra`"yes"`và các chỉ số`l+1`Và`r+1`bởi vì vấn đề sử dụng lập chỉ mục dựa trên 1. 
8. Ngược lại, xuất ra`"no"`. 

### Tại sao nó hoạt động 

hãy để$b$là phiên bản được sắp xếp của mảng. 

Giả sử một số đảo ngược sắp xếp mảng. Bên ngoài đoạn đảo ngược, các phần tử không bao giờ di chuyển nên các vị trí đó phải khớp nhau$b$. Do đó, mọi sự không khớp giữa mảng ban đầu và$b$phải nằm trong một khoảng liền kề. 

Điểm không khớp đầu tiên và điểm không khớp cuối cùng xác định duy nhất khoảng nhỏ nhất chứa tất cả các điểm không khớp. Bất kỳ sự đảo chiều hợp lệ nào cũng phải bao gồm chính xác khoảng thời gian đó. Việc đảo ngược bất kỳ khoảng thời gian nhỏ hơn nào sẽ giữ nguyên ít nhất một khoảng không khớp và việc đảo ngược bất kỳ khoảng thời gian lớn hơn nào sẽ thay đổi các vị trí đã khớp. 

Thuật toán đảo ngược khoảng ứng cử viên duy nhất có thể này và trực tiếp kiểm tra xem kết quả có bằng mảng được sắp xếp hay không. Nếu đúng như vậy thì tồn tại một sự đảo ngược hợp lệ. Nếu không, không có sự đảo ngược đơn lẻ nào có thể sắp xếp mảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    b = sorted(a)

    diff = [i for i in range(n) if a[i] != b[i]]

    if not diff:
        print("yes")
        print(1, 1)
        return

    l = diff[0]
    r = diff[-1]

    c = a[:]
    c[l:r + 1] = reversed(c[l:r + 1])

    if c == b:
        print("yes")
        print(l + 1, r + 1)
    else:
        print("no")

solve()
```Giải pháp bắt đầu bằng cách tạo mảng mục tiêu được sắp xếp đầy đủ. Vì bất kỳ kết quả thành công nào cũng phải bằng phiên bản được sắp xếp này nên nó đóng vai trò là tham chiếu trong suốt thuật toán. 

Danh sách`diff`lưu trữ mọi vị trí mà mảng hiện tại không đồng ý với mảng đã sắp xếp. Nếu danh sách này trống thì mảng đã được sắp xếp. Cho phép đảo ngược một đoạn có độ dài một, vì vậy câu trả lời là ngay lập tức`"yes"`với chỉ số`1 1`. 

Khi tồn tại sự không khớp, sự không khớp đầu tiên và cuối cùng sẽ xác định phân khúc ứng cử viên duy nhất. Việc đảo ngược phạm vi đó sẽ di chuyển tất cả các phần tử có thể bị đặt sai vị trí trong khi vẫn giữ nguyên các vị trí đã chính xác. 

Sự so sánh cuối cùng`c == b`hoạt động như một kiểm tra tính đúng đắn hoàn chỉnh. Điều này tránh được việc phân tích trường hợp phức tạp về ranh giới và các giá trị lân cận. Nếu mảng đảo ngược khớp chính xác với mảng đã sắp xếp thì phân đoạn sẽ hoạt động. Ngược lại, không có phân đoạn hợp lệ nào tồn tại. 

Lỗi triển khai phổ biến nhất là quên rằng vấn đề sử dụng các chỉ mục dựa trên 1. Trong nội bộ, chúng tôi làm việc với tính năng lập chỉ mục dựa trên 0 của Python và chỉ chuyển đổi khi in câu trả lời. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
3 2 1
```Mảng được sắp xếp:```
[1, 2, 3]
```| Bước | Giá trị | 
| --- | --- | 
| Mảng gốc | [3, 2, 1] | 
| Mảng được sắp xếp | [1, 2, 3] | 
| Vị trí không khớp | [0, 2] | 
| tôi | 0 | 
| r | 2 | 
| Sau khi đảo chiều | [1, 2, 3] | 
| Mảng được sắp xếp bằng nhau? | Có | 

Đầu ra:```
yes
1 3
```Ví dụ này cho thấy trường hợp toàn bộ mảng là phân đoạn phải được đảo ngược. 

### Ví dụ 2 

đầu vào:```
5
1 5 4 3 6
```Mảng được sắp xếp:```
[1, 3, 4, 5, 6]
```| Bước | Giá trị | 
| --- | --- | 
| Mảng gốc | [1, 5, 4, 3, 6] | 
| Mảng được sắp xếp | [1, 3, 4, 5, 6] | 
| Vị trí không khớp | [1, 3] | 
| tôi | 1 | 
| r | 3 | 
| Sau khi đảo chiều | [1, 3, 4, 5, 6] | 
| Mảng được sắp xếp bằng nhau? | Có | 

Đầu ra:```
yes
2 4
```Dấu vết này cho thấy khối giảm dần ở giữa mảng. Đảo ngược chính xác khối đó sẽ khôi phục thứ tự đã sắp xếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế trong thời gian chạy | 
| Không gian |$O(n)$| Sắp xếp bản sao và mảng phụ trợ | 

Với$n \le 10^5$, MỘT$O(n \log n)$giải pháp thực hiện thoải mái trong giới hạn. Việc sử dụng bộ nhớ là tuyến tính và dễ dàng phù hợp với 256 MB có sẵn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def solve():
        n = int(input())
        a = list(map(int, input().split()))

        b = sorted(a)

        diff = [i for i in range(n) if a[i] != b[i]]

        out = []

        if not diff:
            out.append("yes")
            out.append("1 1")
            return "\n".join(out)

        l = diff[0]
        r = diff[-1]

        c = a[:]
        c[l:r + 1] = reversed(c[l:r + 1])

        if c == b:
            out.append("yes")
            out.append(f"{l + 1} {r + 1}")
        else:
            out.append("no")

        return "\n".join(out)

    global input
    input = sys.stdin.readline
    return solve()

# provided sample
assert run("3\n3 2 1\n") == "yes\n1 3", "sample"

# already sorted
assert run("5\n1 2 3 4 5\n") == "yes\n1 1", "already sorted"

# middle segment reversal
assert run("5\n1 5 4 3 6\n") == "yes\n2 4", "middle decreasing block"

# impossible case
assert run("5\n3 1 2 5 4\n") == "no", "multiple disorder regions"

# minimum size
assert run("1\n42\n") == "yes\n1 1", "single element"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 42`|`yes 1 1`| Kích thước mảng tối thiểu | 
|`1 2 3 4 5`|`yes 1 1`| Đã sắp xếp mảng | 
|`1 5 4 3 6`|`yes 2 4`| Đảo ngược hoàn toàn bên trong mảng | 
|`3 1 2 5 4`|`no`| Nhiều vùng rối loạn | 
|`3 2 1`|`yes 1 3`| Đảo ngược toàn bộ mảng | 

## Vỏ cạnh 

Hãy xem xét một mảng đã được sắp xếp:```
5
1 2 3 4 5
```Bản sao được sắp xếp giống hệt với mảng ban đầu. Danh sách không khớp trống nên thuật toán ngay lập tức đưa ra:```
yes
1 1
```Việc đảo ngược một chiều dài sẽ khiến mảng không thay đổi, đáp ứng yêu cầu. 

Hãy xem xét một mảng đảo ngược hoàn toàn:```
3
3 2 1
```Các vị trí không khớp là chỉ số đầu tiên và cuối cùng. Thuật toán chọn toàn bộ mảng làm phân đoạn ứng cử viên. Đảo ngược nó tạo ra mảng được sắp xếp, mang lại:```
yes
1 3
```Xem xét nhiều nghịch đảo riêng biệt:```
5
3 1 2 5 4
```Mảng được sắp xếp là:```
1 2 3 4 5
```Sự không khớp đầu tiên xảy ra ở chỉ số 0 và sự không khớp cuối cùng xảy ra ở chỉ mục 4. Việc đảo ngược toàn bộ khoảng sẽ cho:```
4 5 2 1 3
```không được sắp xếp. So sánh cuối cùng không thành công nên thuật toán in chính xác:```
no
```Hãy xem xét một đoạn giảm dần chạm vào điểm cuối:```
5
5 4 3 2 6
```Sự không phù hợp chiếm tiền tố. Thuật toán chọn chỉ số`0`bởi vì`3`, đảo ngược phân đoạn đó và thu được:```
2 3 4 5 6
```phù hợp với mảng được sắp xếp. Đầu ra là:```
yes
1 4
```Không cần xử lý đặc biệt vì kiểm tra đẳng thức cuối cùng sẽ tự động xác nhận các trường hợp biên.
