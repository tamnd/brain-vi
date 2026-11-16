---
title: "CF 1898B - Milena và người ngưỡng mộ"
description: "Chúng ta bắt đầu với một dãy số nguyên dương. Thao tác duy nhất được phép là chọn một phần tử và chia nó thành hai phần dương có tổng bằng giá trị ban đầu. Mỗi lần chia sẽ tăng chiều dài mảng lên một."
date: "2026-06-08T21:29:34+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1898
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 910 (Div. 2)"
rating: 1500
weight: 1898
solve_time_s: 134
verified: true
draft: false
---

[CF 1898B - Milena và Người ngưỡng mộ](https://codeforces.com/problemset/problem/1898/B) 

**Đánh giá:** 1500 
**Tags:** tham lam, toán học 
**Thời gian giải:** 2m 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một dãy số nguyên dương. Thao tác duy nhất được phép là chọn một phần tử và chia nó thành hai phần dương có tổng bằng giá trị ban đầu. Mỗi lần chia sẽ tăng chiều dài mảng lên một. 

Mục tiêu là làm cho mảng cuối cùng không giảm trong khi sử dụng càng ít thao tác phân tách càng tốt. 

Một cách hữu ích để hình dung về phép toán là sau khi liên tục chia một giá trị`a`, cuối cùng chúng ta thay thế nó bằng một số số nguyên dương có tổng vẫn bằng`a`. Nếu một giá trị được chia thành`k`phần, yêu cầu chính xác`k - 1`hoạt động. 

Đầu vào chứa tối đa`10^4`các trường hợp thử nghiệm và tổng số phần tử mảng trong tất cả các trường hợp thử nghiệm nhiều nhất là`2 · 10^5`. Bất kỳ giải pháp nào chậm hơn đáng kể so với tuyến tính hoặc tuyến tính trong`n`sẽ không phù hợp thoải mái trong giới hạn. Một giải pháp bậc hai sẽ yêu cầu khoảng`4 · 10^10`hoạt động trong trường hợp xấu nhất, vượt xa những gì có thể được thực hiện trong một giây. 

Phần khó khăn là việc tách một phần tử sẽ ảnh hưởng đến tất cả các so sánh trong tương lai liên quan đến phần tử đó. Chúng tôi không xây dựng mảng cuối cùng một cách rõ ràng vì kích thước của nó có thể trở nên rất lớn. Thay vào đó, chúng ta cần suy luận xem mỗi giá trị phải được chia thành bao nhiêu phần. 

Một sai lầm dễ mắc phải là chia một cách tham lam mà không xem xét phần tử bên phải. 

Coi như:```
3
3 2 1
```Câu trả lời là`3`. 

Nếu chúng ta chỉ cố gắng khắc phục sự đảo ngược cục bộ, chúng ta có thể chia nhỏ`3`một lần vào`1 + 2`, sản xuất`[1,2,2,1]`, vẫn không giảm. Tác động của việc phân chia phải được đánh giá dựa trên giá trị tối đa được cho phép bởi mọi thứ ở bên phải nó. 

Một trường hợp tinh vi khác là khi giá trị đã đủ nhỏ.```
2
4 5
```Câu trả lời là`0`. 

Việc triển khai bất cẩn vẫn có thể tính toán một số phần dương và thêm các thao tác không cần thiết. Bất cứ khi nào giá trị hiện tại cao nhất là giới hạn cho phép, chúng ta nên giữ nguyên giá trị đó. 

Trường hợp cạnh thứ ba xuất hiện khi giá trị không chia hết cho số phần đã chọn.```
2
10 3
```Câu trả lời là`3`. 

Tách`10`thành ba mảnh cho kích thước xung quanh`3.33`, vì vậy một mảnh sẽ trở thành`4`, vẫn lớn hơn`3`. Chúng tôi thực sự cần bốn phần:`3,3,2,2`. Vì bốn mảnh cần ba thao tác nên câu trả lời là`3`. Sử dụng cách chia sàn thay vì chia trần ở đây sẽ cho kết quả sai. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là thực hiện phân tách một cách rõ ràng cho đến khi mảng không giảm. Đối với mỗi phần tử vi phạm, chúng ta có thể thử các cách phân vùng khác nhau và tiếp tục đệ quy. Điều này đúng vì mọi chuỗi hoạt động hợp lệ đều có thể được khám phá. 

Vấn đề là số lượng phân vùng có thể tăng lên một cách bùng nổ. Ngay cả một giá trị duy nhất như`100`có thể được chia theo nhiều cách khác nhau và mọi phần mới đều có thể được chia lại. Không gian tìm kiếm trở nên khổng lồ rất lâu trước khi đạt đến giới hạn đầu vào. 

Nhận xét quan trọng là việc sắp xếp chính xác các mảnh hầu như không liên quan. Điều quan trọng là mảnh lớn nhất được tạo ra sau khi chia tách. 

Giả sử chúng ta xử lý mảng từ phải sang trái. Hãy tưởng tượng rằng mọi thứ ở bên phải đã được xử lý và giá trị lớn nhất chúng ta được phép đặt trước hậu tố đó là`mx`. 

Nếu giá trị hiện tại`a[i]`nhiều nhất là đã rồi`mx`, chúng tôi giữ nó không thay đổi. 

Ngược lại, chúng ta phải chia nó thành nhiều phần sao cho mỗi phần thu được nhiều nhất là`mx`. Vì chúng ta muốn số lượng thao tác tối thiểu nên chúng ta muốn số phần tối thiểu có thể. 

Nếu chúng ta chia thành`k`mảnh, mảnh lớn nhất ít nhất là`ceil(a[i] / k)`. Để làm cho mọi phần nhiều nhất`mx`, chúng ta cần:$$\left\lceil \frac{a[i]}{k} \right\rceil \le mx$$Hợp lệ nhỏ nhất`k`là:$$k = \left\lceil \frac{a[i]}{mx} \right\rceil$$Sử dụng ít mảnh hơn sẽ buộc một số mảnh vượt quá`mx`. 

Khi chúng tôi chọn mức tối thiểu này`k`, số lượng hoạt động tăng lên bởi`k - 1`. 

Còn một chi tiết nữa. Sau khi tách, giá trị nào sẽ trở thành giới hạn mới cho các phần tử còn lại? 

Trong số`k`mảnh, mảnh nhỏ nhất có thể là:$$\left\lfloor \frac{a[i]}{k} \right\rfloor$$Phía bên trái không được vượt quá mảnh đầu tiên của khối này nên mảnh nhỏ nhất này trở thành mảnh tối đa mới cho phép. 

Điều này biến vấn đề thành một lần quét tham lam từ phải sang trái đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo câu trả lời cho`0`. 
2. Đặt`mx`đến phần tử cuối cùng của mảng. Đây là giá trị tối đa mà phần tử trước đó cuối cùng có thể tạo ra. 
3. Xử lý mảng từ phải sang trái, bắt đầu từ chỉ số`n - 2`. 
4. Nếu`a[i] <= mx`, không cần chia tách. Cập nhật`mx = a[i]`. 

Bản thân giá trị hiện tại trở thành hạn chế mới cho mọi thứ còn lại. 
5. Ngược lại, hãy tính số mảnh tối thiểu cần thiết để mỗi mảnh có nhiều nhất`mx`:$$k = \left\lceil \frac{a[i]}{mx} \right\rceil$$6. Chia thành`k`miếng yêu cầu`k - 1`hoạt động. Thêm điều này vào câu trả lời. 
7. Phần nhỏ nhất có được trong một phân vùng cân bằng tối ưu là:$$\left\lfloor \frac{a[i]}{k} \right\rfloor$$Bộ:$$mx = \left\lfloor \frac{a[i]}{k} \right\rfloor$$Điều này trở thành giới hạn mới cho các phần tử còn lại. 
8. Sau khi xử lý tất cả các phần tử, xuất ra câu trả lời tích lũy. 

### Tại sao nó hoạt động 

Ở mọi vị trí, hậu tố bên phải đã áp đặt giá trị tối đa được phép`mx`. Mọi thỏa thuận cuối cùng hợp lệ đều phải chia tách`a[i]`đủ số lần để không có sản phẩm nào được sản xuất vượt quá giới hạn này. 

Số phần tối thiểu thỏa mãn điều kiện đó chính xác là`ceil(a[i] / mx)`. Bất kỳ số lượng mảnh nhỏ hơn nào cũng sẽ để lại ít nhất một mảnh lớn hơn`mx`, làm cho mảng không thể sắp xếp theo thứ tự không giảm. 

Trong số tất cả các phân vùng thành số lượng mảnh tối thiểu đó, việc cân bằng các mảnh càng đồng đều càng tốt sẽ tối đa hóa mảnh nhỏ nhất. Phần nhỏ nhất đó bằng`floor(a[i] / k)`, và đó là giới hạn lớn nhất mà chúng ta có thể vượt qua bên trái một cách an toàn. Việc giữ giới hạn này càng lớn càng tốt chỉ có thể giúp các phần tử trong tương lai yêu cầu ít sự phân chia hơn. 

Bởi vì mỗi vị trí được xử lý với lựa chọn cục bộ tối ưu và lựa chọn đó để lại trạng thái dễ dãi nhất cho tiền tố còn lại, nên chiến lược tham lam là tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))

    ans = 0
    mx = a[-1]

    for i in range(n - 2, -1, -1):
        if a[i] <= mx:
            mx = a[i]
        else:
            k = (a[i] + mx - 1) // mx
            ans += k - 1
            mx = a[i] // k

    print(ans)
```Biến`mx`đại diện cho giá trị lớn nhất mà vị trí hiện tại có thể tạo ra mà không phá vỡ hậu tố đã được xử lý. 

Khi`a[i] <= mx`, chúng tôi giữ nguyên giá trị. Vì giá trị này bây giờ là phần tử đầu tiên của hậu tố được xử lý nên nó trở thành giới hạn mới. 

Khi`a[i] > mx`, chúng tôi tính toán số lượng mảnh tối thiểu bằng cách sử dụng phép chia trần:```
k = (a[i] + mx - 1) // mx
```Biểu thức này là việc thực hiện số nguyên của`ceil(a[i] / mx)`. 

The number of operations added is`k - 1`bởi vì chia một giá trị thành`k`các mảnh đòi hỏi chính xác số lần chia tách đó. 

Cuối cùng,```
mx = a[i] // k
```tính toán`floor(a[i] / k)`, là phần nhỏ nhất trong một phân vùng được cân bằng tối ưu. Sử dụng giá trị này là rất quan trọng. Sử dụng phần lớn nhất hoặc phần trung bình sẽ phá vỡ bất biến tham lam. 

Số nguyên Python dễ dàng xử lý câu trả lời vì số thao tác trong trường hợp xấu nhất có thể vượt quá`32-bit`giới hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 3 2
```| tôi | một [tôi] | mx trước | k | hoạt động bổ sung | mx sau | tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 2 | 2 | 1 | 1 | 1 | 
| 0 | 1 | 1 | - | 0 | 1 | 1 | 

Câu trả lời cuối cùng:`1`. 

giá trị`3`không thể ở trước một giá trị giới hạn`2`. Chia nó thành hai phần là đủ và một thao tác sẽ giải quyết toàn bộ mảng. 

### Ví dụ 2 

đầu vào:```
3
3 2 1
```| tôi | một [tôi] | mx trước | k | hoạt động bổ sung | mx sau | tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 2 | 1 | 1 | 1 | 
| 0 | 3 | 1 | 3 | 2 | 1 | 3 | 

Câu trả lời cuối cùng:`3`. 

Ngoài cùng bên phải`1`buộc mọi phần được tạo ra tối đa`1`. giá trị`2`phải trở thành hai số một và giá trị`3`phải trở thành ba cái một. Tổng số thao tác là`1 + 2 = 3`. 

Ví dụ này chứng minh rằng các ràng buộc lan truyền sang trái. Một khi giới hạn trở thành`1`, mọi giá trị trước đó cuối cùng phải được chia thành một. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần | 
| Không gian | O(1) | Chỉ có một số biến được duy trì | 

Tổng số phần tử trong tất cả các trường hợp thử nghiệm nhiều nhất là`2 · 10^5`, do đó việc quét tuyến tính qua từng trường hợp kiểm thử dễ dàng phù hợp với giới hạn thời gian. Thuật toán chỉ lưu trữ mảng và một số số nguyên, đáp ứng thoải mái giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        ans = 0
        mx = a[-1]

        for i in range(n - 2, -1, -1):
            if a[i] <= mx:
                mx = a[i]
            else:
                k = (a[i] + mx - 1) // mx
                ans += k - 1
                mx = a[i] // k

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run(
"""4
3
1 3 2
4
1 2 3 4
3
3 2 1
7
1 4 4 3 5 7 6
"""
) == "1\n0\n3\n9", "sample"

# minimum size
assert run(
"""1
1
7
"""
) == "0", "single element"

# all equal
assert run(
"""1
5
4 4 4 4 4
"""
) == "0", "already non-decreasing"

# divisibility edge case
assert run(
"""1
2
10 3
"""
) == "3", "requires ceiling division"

# strictly decreasing
assert run(
"""1
4
4 3 2 1
"""
) == "6", "many splits"

# large value followed by one
assert run(
"""1
2
1000000000 1
"""
) == "999999999", "large answer"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`7`|`0`| Mảng một phần tử | 
|`4 4 4 4 4`|`0`| Đã không giảm với giá trị bằng nhau | 
|`10 3`|`3`| Sử dụng vách ngăn trần đúng cách | 
|`4 3 2 1`|`6`| Tuyên truyền lặp đi lặp lại một giới hạn chặt chẽ | 
|`1000000000 1`|`999999999`| Số lượng hoạt động rất lớn | 

## Vỏ cạnh 

### Giá trị lớn ngay trước giới hạn nhỏ 

đầu vào:```
1
2
10 3
```Bắt đầu với`mx = 3`. 

Vì`10`, chúng tôi tính toán:$$k=\left\lceil\frac{10}{3}\right\rceil=4$$Ba hoạt động được yêu cầu. Giới hạn mới trở thành:$$\left\lfloor\frac{10}{4}\right\rfloor=2$$Đầu ra của thuật toán`3`. 

A solution using`10 // 3 = 3`các mảnh sẽ cho rằng ba mảnh là đủ một cách không chính xác, nhưng một mảnh nhất thiết phải có kích thước`4`. 

### Đã có mảng không giảm 

đầu vào:```
1
4
1 2 3 4
```Quá trình quét tiến hành như sau:```
mx = 4
3 <= 4
2 <= 3
1 <= 2
```Không có phép chia nào được thực hiện và câu trả lời vẫn còn`0`. 

Điều này xác minh rằng thuật toán tham lam không đưa ra các hoạt động không cần thiết. 

### Giới hạn trở thành một 

đầu vào:```
1
3
3 2 1
```Sửa phần tử ngoài cùng bên phải`mx = 1`. 

Vì`2`, chúng tôi cần`k = 2`, thêm một thao tác. 

Vì`3`, chúng tôi cần`k = 3`, thêm hai thao tác nữa. 

Câu trả lời cuối cùng là`3`. 

Trường hợp này cho thấy tại sao việc xử lý từ phải sang trái là tự nhiên. Khi mức tối đa cho phép trở thành`1`, mọi giá trị trước đó phải được phân tách hoàn toàn thành một và thuật toán sẽ nắm bắt giá trị đó ngay lập tức.
