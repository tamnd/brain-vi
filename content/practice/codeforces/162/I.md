---
title: "CF 162I - Số nguyên tố cắt ngắn"
description: "Chúng ta được cấp một số nguyên duy nhất và phải quyết định xem nó có thỏa mãn một thuộc tính nguyên tố rất cụ thể hay không. Một số được coi là có thể cắt bớt nếu mọi hậu tố được hình thành bằng cách liên tục loại bỏ chữ số ngoài cùng bên trái vẫn là số nguyên tố."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 162
codeforces_index: "I"
codeforces_contest_name: "VK Cup 2012 Wild-card Round 1"
rating: 2000
weight: 162
solve_time_s: 82
verified: true
draft: false
---

[CF 162I - Số nguyên tố có thể cắt ngắn](https://codeforces.com/problemset/problem/162/I) 

**Xếp hạng:** 2000 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một số nguyên duy nhất và phải quyết định xem nó có thỏa mãn một thuộc tính nguyên tố rất cụ thể hay không. 

Một số được coi là có thể cắt bớt nếu mọi hậu tố được hình thành bằng cách liên tục loại bỏ chữ số ngoài cùng bên trái vẫn là số nguyên tố. Ví dụ, bắt đầu từ`9137`, chúng tôi kiểm tra:`9137`,`137`,`37`,`7`Cả 4 số đều là số nguyên tố nên đáp án là`"YES"`. 

Số này cũng không được chứa chữ số`0`. Một giá trị như`103`ngay lập tức thất bại ngay cả khi một số hậu tố là số nguyên tố, vì định nghĩa rõ ràng cấm số không. 

Giới hạn đầu vào tối đa là`10^7`, đủ nhỏ để việc kiểm tra tính nguyên tố trực tiếp là hoàn toàn thực tế. Việc kiểm tra tính nguyên tố căn bậc hai cho một số mất khoảng`O(sqrt(n))`hoạt động. Vì một số lên đến`10^7`có nhiều nhất 8 chữ số, chúng ta chỉ cần kiểm tra tối đa 8 hậu tố. Ngay cả trong trường hợp xấu nhất, đây cũng chỉ là vài chục nghìn phép tính số học, thấp hơn rất nhiều so với thời hạn. 

Phần khó khăn không phải là hiệu suất, nó tạo và xác thực chính xác mọi hậu tố. 

Một sai lầm dễ mắc phải là quên rằng hậu tố có một chữ số cũng phải là số nguyên tố. 

Coi như:```
19
```Các hậu tố là:`19`,`9`Mặc dù`19`chính nó là số nguyên tố`9`không, vậy câu trả lời đúng là`"NO"`. 

Một lỗi phổ biến khác là bỏ qua hạn chế về số không. 

Coi như:```
1013
```Các hậu tố là:`1013`,`13`,`3`Tất cả đều là số nguyên tố, nhưng số đó vẫn thất bại vì nó chứa số 0. Câu trả lời đúng là`"NO"`. 

Vấn đề tế nhị thứ ba là việc xử lý`1`như nguyên tố. 

Coi như:```
11
```Các hậu tố là:`11`,`1`Từ`1`không phải là số nguyên tố, câu trả lời phải là`"NO"`. 

Việc triển khai bất cẩn sử dụng thử nghiệm tính nguyên thủy không chính xác có thể vô tình chấp nhận nó. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là tạo ra mọi hậu tố và kiểm tra độc lập xem nó có phải là số nguyên tố hay không. 

Giả sử số được biểu diễn dưới dạng một chuỗi. Chúng ta có thể xóa đi ký tự đầu tiên nhiều lần:```
9137
137
37
7
```Đối với mỗi giá trị, chúng tôi chạy thử nghiệm tính nguyên thủy. Nếu bất kỳ hậu tố nào là hợp số hoặc nếu số ban đầu chứa số 0, chúng tôi sẽ trả lời ngay`"NO"`. 

Ý tưởng mạnh mẽ này đã đủ nhanh vì đầu vào rất nhỏ. Một con số lên tới`10^7`có nhiều nhất 8 hậu tố. Mỗi lần kiểm tra tính nguyên thủy có chi phí tối đa`O(sqrt(n))`, đó là về`3162`lặp lại trong trường hợp xấu nhất. Ngay cả việc nhân chúng lại với nhau cũng mang lại thời gian chạy rất nhỏ. 

Không cần sàng lọc, tiền xử lý hoặc kiểm tra xác suất nguyên thủy. Các ràng buộc được thiết kế sao cho việc phân chia thử nghiệm đơn giản được thực hiện một cách thoải mái. 

Quan sát quan trọng là số lượng hậu tố cực kỳ nhỏ. Thay vì tìm kiếm qua nhiều số ứng cử viên, chúng tôi chỉ xác thực một chuỗi có tối đa 8 giá trị. Điều đó làm giảm vấn đề về việc kiểm tra tính nguyên thủy lặp đi lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force sử dụng phép chia đơn giản để kiểm tra tới n | O(n) mỗi hậu tố | O(1) | Về mặt khái niệm quá chậm | 
| Tối ưu bằng cách sử dụng kiểm tra tính nguyên tố căn bậc hai | O(d × sqrt(n)) | O(1) | Đã chấp nhận | 

Đây,`d`là số chữ số, nhiều nhất là 8. 

## Hướng dẫn thuật toán 

1. Đọc số nguyên dưới dạng chuỗi. 

Việc sử dụng chuỗi giúp việc tạo hậu tố trở nên đơn giản và tránh logic trích xuất chữ số lặp lại. 
2. Kiểm tra xem chuỗi có chứa chữ số không`'0'`. 

Nếu có thì in ngay`"NO"`bởi vì các số nguyên tố cắt ngắn không được phép chứa số không. 
3. Tạo mọi hậu tố của số. 

Đối với chỉ mục`i`, hậu tố là`s[i:]`. 
4. Chuyển mỗi hậu tố thành số nguyên và kiểm tra xem nó có phải là số nguyên tố hay không. 

Một số là số nguyên tố nếu: 

- ít nhất là 2 
- không có số nguyên từ`2`ĐẾN`sqrt(x)`chia nó 
5. Nếu bất kỳ hậu tố nào không phải là số nguyên tố, hãy in`"NO"`và dừng lại. 
6. Nếu tất cả các hậu tố đều là số nguyên tố, hãy in`"YES"`. 

### Tại sao nó hoạt động 

Thuật toán khớp trực tiếp với định nghĩa của số nguyên tố cắt ngắn. 

Mỗi hậu tố hợp lệ được kiểm tra chính xác một lần. Giới hạn 0 được thực thi trước bất kỳ thử nghiệm nguyên thủy nào. Kiểm tra tính nguyên thủy từ chối chính xác`1`và mọi hợp số. 

Vì câu trả lời là`"YES"`chỉ khi tất cả các hậu tố thỏa mãn điều kiện nguyên tố và`"NO"`mặt khác, thuật toán là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_prime(x):
    if x < 2:
        return False

    d = 2
    while d * d <= x:
        if x % d == 0:
            return False
        d += 1

    return True

def solve():
    s = input().strip()

    if '0' in s:
        print("NO")
        return

    for i in range(len(s)):
        value = int(s[i:])

        if not is_prime(value):
            print("NO")
            return

    print("YES")

solve()
```Giải pháp bắt đầu bằng cách đọc số dưới dạng chuỗi vì hậu tố được biểu diễn tự nhiên theo cách này. Việc sử dụng tính năng cắt chuỗi sẽ tránh được việc tính toán thủ công để loại bỏ các chữ số đứng đầu. 

Việc kiểm tra số 0 xảy ra trước tiên vì đây là hành vi bị loại ngay lập tức. Không có lý do gì để dành thời gian cho việc kiểm tra tính nguyên tố nếu con số đó đã vi phạm định nghĩa. 

các`is_prime`hàm sử dụng phép chia thử tiêu chuẩn lên đến căn bậc hai. Nếu tồn tại một hợp số thì ít nhất phải có nhiều nhất một thừa số`sqrt(x)`, vì vậy việc kiểm tra ngoài điểm đó là không cần thiết. 

điều kiện`x < 2`là rất quan trọng. Không có nó, hậu tố`1`sẽ được coi là số nguyên tố một cách không chính xác. 

Vòng lặp lặp qua mọi hậu tố`s[i:]`. Thời điểm một hậu tố không thành công, thuật toán sẽ thoát sớm với`"NO"`. Điều này giúp việc thực hiện đơn giản và hiệu quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
19
```| Bước | Hậu tố | Xuất sắc? | 
| --- | --- | --- | 
| 1 | 19 | Có | 
| 2 | 9 | Không | 

Hậu tố thứ hai là`9`, đó là hợp chất. Thuật toán in ngay lập tức`"NO"`. 

Ví dụ này chứng tỏ rằng bản thân số nguyên tố thôi là chưa đủ. Mọi hậu tố cũng phải là số nguyên tố. 

### Ví dụ 2 

đầu vào:```
9137
```| Bước | Hậu tố | Xuất sắc? | 
| --- | --- | --- | 
| 1 | 9137 | Có | 
| 2 | 137 | Có | 
| 3 | 37 | Có | 
| 4 | 7 | Có | 

Tất cả các hậu tố đều là số nguyên tố nên thuật toán in ra`"YES"`. 

Dấu vết này xác nhận tính bất biến rằng mọi hậu tố còn lại phải thỏa mãn tính nguyên tố một cách độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(d × sqrt(n)) | Nhiều nhất`d`hậu tố, mỗi hậu tố được kiểm tra bằng phép chia thử căn bậc hai | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Từ`d ≤ 8`Và`sqrt(10^7) ≈ 3162`, thời gian chạy rất nhỏ so với giới hạn 3 giây. Giải pháp dễ dàng phù hợp với cả hạn chế về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def is_prime(x):
        if x < 2:
            return False

        d = 2
        while d * d <= x:
            if x % d == 0:
                return False
            d += 1

        return True

    def solve():
        s = input().strip()

        if '0' in s:
            return "NO"

        for i in range(len(s)):
            if not is_prime(int(s[i:])):
                return "NO"

        return "YES"

    return solve()

# provided sample
assert run("19\n") == "NO", "sample 1"

# custom cases
assert run("9137\n") == "YES", "valid truncatable prime"
assert run("2\n") == "YES", "single digit prime"
assert run("11\n") == "NO", "suffix becomes 1"
assert run("1013\n") == "NO", "contains zero"
assert run("9999991\n") == "NO", "large composite suffix"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`9137`|`YES`| Chuỗi hậu tố thành công điển hình | 
|`2`|`YES`| Số nguyên tố hợp lệ nhỏ nhất | 
|`11`|`NO`| Xử lý đúng hậu tố`1`| 
|`1013`|`NO`| Từ chối chữ số không | 
|`9999991`|`NO`| Giá trị lớn hơn và phát hiện tổng hợp | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
11
```Các hậu tố là:```
11
1
```Thuật toán kiểm tra đầu tiên`11`, đó là số nguyên tố. Sau đó nó sẽ kiểm tra`1`. Vì hàm nguyên tố từ chối rõ ràng tất cả các số bên dưới`2`, thuật toán in chính xác`"NO"`. 

Bây giờ hãy xem xét:```
1013
```Trước khi tạo hậu tố, thuật toán sẽ phát hiện chữ số`'0'`trong chuỗi. Nó lập tức quay lại`"NO"`mà không cần phải làm gì thêm. Điều này hoàn toàn phù hợp với định nghĩa. 

Cuối cùng, hãy xem xét trường hợp hợp lệ nhỏ nhất:```
2
```Chỉ có một hậu tố,`2`chính nó. Việc kiểm tra tính nguyên tố thành công nên thuật toán in ra`"YES"`. 

Điều này xác nhận rằng số nguyên tố có một chữ số được chấp nhận chính xác.
