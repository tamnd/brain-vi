---
title: "CF 459B - Pashmak và Hoa"
description: "Chúng ta được trao tặng những giá trị vẻ đẹp của n bông hoa. Chúng ta phải chọn đúng hai bông hoa. Trong số tất cả các cặp có thể có, chúng ta chỉ quan tâm đến những cặp có sự khác biệt về vẻ đẹp càng lớn càng tốt."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 459
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 261 (Div. 2)"
rating: 1300
weight: 459
solve_time_s: 91
verified: true
draft: false
---

[CF 459B - Pashmak và Hoa](https://codeforces.com/problemset/problem/459/B) 

**Đánh giá:** 1300 
**Tags:** tổ hợp, triển khai, sắp xếp 
**Thời gian giải:** 1 phút 31 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được ban tặng những giá trị vẻ đẹp của`n`hoa. Chúng ta phải chọn đúng hai bông hoa. 

Trong số tất cả các cặp có thể có, chúng ta chỉ quan tâm đến những cặp có sự khác biệt về vẻ đẹp càng lớn càng tốt. Nhiệm vụ là xuất ra hai giá trị: 

Đầu tiên, sự khác biệt tối đa có thể có giữa vẻ đẹp của hai bông hoa được chọn. 

Thứ hai, số lượng cặp hoa riêng biệt đạt được sự khác biệt tối đa này. Hoa được phân biệt bởi vị trí của chúng, vì vậy hai bông hoa có vẻ đẹp giống nhau vẫn là những lựa chọn khác nhau nếu chúng đến từ các chỉ số khác nhau. 

Những hạn chế là lớn. Có thể có tới`2 * 10^5`hoa và giá trị vẻ đẹp có thể lớn bằng`10^9`. Một thuật toán bậc hai để kiểm tra mọi cặp sẽ yêu cầu khoảng$$\frac{n(n-1)}{2}$$so sánh. Vì`n = 200000`, tức là khoảng 20 tỷ cặp, điều này hoàn toàn không thể thực hiện được trong vòng một giây. 

Bản thân giá trị cái đẹp đã lớn nhưng chúng ta chỉ cần so sánh và đếm. Không cần lập trình động hoặc cấu trúc dữ liệu phức tạp. 

Một trường hợp góc cạnh tinh tế xuất hiện khi tất cả các bông hoa đều có vẻ đẹp giống nhau. 

đầu vào:```
5
7 7 7 7 7
```Mỗi cặp đều có sự khác biệt`0`, đó tự động là sự khác biệt tối đa. Câu trả lời là:```
0 10
```bởi vì có$$\binom{5}{2}=10$$các cặp khác nhau. Giải pháp bất cẩn chỉ tính hoa có vẻ đẹp tối thiểu nhân với hoa có vẻ đẹp tối đa sẽ cho kết quả sai.`25`. 

Một trường hợp quan trọng khác là khi giá trị tối thiểu và tối đa xuất hiện nhiều lần. 

đầu vào:```
5
1 3 1 5 5
```Sự khác biệt tối đa là`5 - 1 = 4`. 

Có hai bông hoa đẹp`1`và hai bông hoa với vẻ đẹp`5`, vậy số cặp tối ưu là:$$2 \times 2 = 4$$Đầu ra đúng là:```
4 4
```Giải pháp chỉ tìm thấy một lần xuất hiện tối thiểu và tối đa sẽ trả về sai`1`. 

Trường hợp góc cuối cùng là đầu vào nhỏ nhất có thể. 

đầu vào:```
2
100 1
```Chỉ có một cặp tồn tại nên câu trả lời phải là:```
99 1
```Bất kể giá trị nào, chỉ có duy nhất một lựa chọn hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là kiểm tra từng cặp hoa. Đối với mỗi cặp, hãy tính toán sự khác biệt về vẻ đẹp, theo dõi sự khác biệt lớn nhất được thấy cho đến nay và đếm xem có bao nhiêu cặp đạt được sự khác biệt đó. 

Phương pháp brute-force này đúng vì nó kiểm tra rõ ràng mọi cặp có thể. Vấn đề là thời gian chạy của nó. Với`n = 200000`, số lượng cặp là khoảng 20 tỷ, vượt xa những gì có thể được xử lý trong giới hạn. 

Nhận xét quan trọng là sự khác biệt lớn nhất có thể có phải đến từ giá trị vẻ đẹp nhỏ nhất và giá trị vẻ đẹp lớn nhất. 

Giả sử vẻ đẹp tối thiểu trong toàn bộ mảng là`mn`và vẻ đẹp tối đa là`mx`. 

Không có cặp nào có thể có sự khác biệt lớn hơn`mx - mn`, bởi vẻ đẹp nào cũng nằm giữa hai thái cực đó. Đồng thời, chọn một loài hoa có vẻ đẹp`mn`và một người khác với vẻ đẹp`mx`đạt được chính xác sự khác biệt đó. 

Điều này ngay lập tức mang lại sự khác biệt tối đa:$$mx - mn$$Câu hỏi còn lại là có bao nhiêu cặp đạt được điều đó. 

Nếu như`mn != mx`, mỗi cặp tối ưu phải chứa một bông hoa có giá trị tối thiểu và một bông hoa có giá trị tối đa. Bất kỳ giá trị vẻ đẹp nào khác sẽ làm giảm sự khác biệt. 

Nếu giá trị tối thiểu xuất hiện`cnt_min`lần và giá trị tối đa xuất hiện`cnt_max`lần, thì mỗi bông hoa tối thiểu có thể được ghép với mọi bông hoa tối đa:$$cnt\_min \times cnt\_max$$các cặp tối ưu 

Nếu như`mn == mx`, thì mọi bông hoa đều có vẻ đẹp giống nhau. Mỗi cặp đều đạt được sự khác biệt`0`, vì vậy chúng ta phải đếm tất cả các cặp không có thứ tự:$$\binom{n}{2} = \frac{n(n-1)}{2}$$Điều này làm giảm vấn đề tìm giá trị tối thiểu, giá trị tối đa và tần số của chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng hoa và giá trị vẻ đẹp của chúng. 
2. Tìm giá trị vẻ đẹp tối thiểu`mn`. 
3. Tìm giá trị vẻ đẹp tối đa`mx`. 
4. Tính chênh lệch lớn nhất là`mx - mn`. 
5. Nếu`mn == mx`, tất cả các loài hoa đều có vẻ đẹp giống nhau. 

Sự khác biệt tối đa là`0`và mọi cặp không có thứ tự đều tối ưu. Tính toán:$$\frac{n(n-1)}{2}$$6. Nếu không thì đếm xem có bao nhiêu bông hoa có vẻ đẹp`mn`. Gọi đây`cnt_min`. 
7. Đếm xem có bao nhiêu bông hoa có vẻ đẹp`mx`. Gọi đây`cnt_max`. 
8. Số cặp tối ưu là:$$cnt\_min \times cnt\_max$$9. Xuất ra chênh lệch lớn nhất và số cặp tối ưu. 

### Tại sao nó hoạt động 

Sự khác biệt về vẻ đẹp lớn nhất có thể đạt được từ bất kỳ cặp nào không thể vượt quá sự khác biệt giữa giá trị tối đa toàn cầu và giá trị tối thiểu toàn cầu. Vì việc chọn một bông hoa có giá trị tối thiểu và một bông hoa có giá trị tối đa đạt được chính xác sự khác biệt đó nên sự khác biệt tối ưu được xác định duy nhất là`mx - mn`. 

Khi`mn != mx`, mỗi cặp tối ưu phải sử dụng một bông hoa từ mỗi cực. Việc thay thế một trong hai bông hoa bằng bất kỳ vẻ đẹp trung gian nào sẽ làm giảm sự khác biệt. Như vậy tổng số cặp tối ưu chính xác là số cách để chọn một bông hoa tối thiểu và một bông hoa tối đa. 

Khi`mn == mx`, mỗi bông hoa đều có vẻ đẹp giống nhau nên mỗi cặp hoa đều có sự khác biệt`0`. Đếm tất cả các cặp không có thứ tự sẽ cho câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    mn = min(a)
    mx = max(a)

    diff = mx - mn

    if mn == mx:
        ways = n * (n - 1) // 2
    else:
        cnt_min = a.count(mn)
        cnt_max = a.count(mx)
        ways = cnt_min * cnt_max

    print(diff, ways)

solve()
```Bước đầu tiên là tìm ra những giá trị vẻ đẹp nhỏ nhất và lớn nhất. Hai giá trị này hoàn toàn xác định sự khác biệt tối đa có thể đạt được. 

Sau khi tính toán sự khác biệt, việc triển khai được chia thành hai trường hợp. 

Nếu giá trị tối thiểu và tối đa bằng nhau thì mọi bông hoa đều có vẻ đẹp giống nhau. Số cặp tối ưu là số cặp không có thứ tự trong số`n`hoa được tính bằng công thức tổ hợp`n * (n - 1) // 2`. 

Mặt khác, các cặp tối ưu duy nhất là những cặp được hình thành bởi một bông hoa có giá trị tối thiểu và một bông hoa có giá trị tối đa. Mã đếm số lần xuất hiện của cả hai giá trị và nhân số lượng đó. 

Một chi tiết dễ bị bỏ qua là kích thước của câu trả lời. Với`n = 200000`, số lượng các cặp có thể lên tới gần 20 tỷ. Các ngôn ngữ có số nguyên 32 bit có kích thước cố định sẽ tràn. Số nguyên Python tự động tăng lên nên không cần xử lý đặc biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 2
```| Bước | mn | mx | khác biệt | cnt_min | cnt_max | cách | 
| --- | --- | --- | --- | --- | --- | --- | 
| Sau khi quét | 1 | 2 | 1 | - | - | - | 
| Đếm tần số | 1 | 2 | 1 | 1 | 1 | 1 | 

Đầu ra:```
1 1
```Chỉ có một đôi hoa. Sự khác biệt của nó là`1`, đây tự động là mức chênh lệch tối đa có thể có. 

### Ví dụ 2 

đầu vào:```
5
1 3 1 5 5
```| Bước | mn | mx | khác biệt | cnt_min | cnt_max | cách | 
| --- | --- | --- | --- | --- | --- | --- | 
| Sau khi quét | 1 | 5 | 4 | - | - | - | 
| Đếm tần số | 1 | 5 | 4 | 2 | 2 | 4 | 

Đầu ra:```
4 4
```Có hai bông hoa đẹp`1`và hai bông hoa với vẻ đẹp`5`. Mỗi bông hoa tối thiểu có thể ghép với mọi bông hoa tối đa, tạo ra`2 × 2 = 4`các cặp tối ưu 

Ví dụ này chứng minh rằng việc đếm số lần xuất hiện là điều cần thiết. Chỉ nhìn vào các giá trị riêng biệt sẽ bỏ lỡ nhiều cặp hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Việc tìm tần số tối thiểu, tối đa và tần số yêu cầu quét tuyến tính | 
| Không gian | O(1) | Chỉ một số biến được sử dụng ngoài mảng đầu vào | 

Thuật toán xử lý mảng một số lần không đổi. Ngay cả đối với`200000`hoa, tổng công việc rất nhỏ so với thời gian. Việc sử dụng bộ nhớ cũng thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    mn = min(a)
    mx = max(a)

    if mn == mx:
        ways = n * (n - 1) // 2
    else:
        ways = a.count(mn) * a.count(mx)

    print(mx - mn, ways)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    global input
    input = sys.stdin.readline

    solve()

    out = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("2\n1 2\n") == "1 1", "sample 1"

# minimum size
assert run("2\n100 1\n") == "99 1", "minimum n"

# all equal
assert run("5\n7 7 7 7 7\n") == "0 10", "all pairs optimal"

# repeated minima and maxima
assert run("5\n1 3 1 5 5\n") == "4 4", "multiple optimal pairs"

# many middle values
assert run("6\n1 2 3 4 5 6\n") == "5 1", "single min and max"

# boundary counting
assert run("6\n1 1 1 10 10 10\n") == "9 9", "3 * 3 optimal pairs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 100 1`|`99 1`| Đầu vào nhỏ nhất được phép | 
|`5 / 7 7 7 7 7`|`0 10`| Tất cả các giá trị bằng nhau | 
|`5 / 1 3 1 5 5`|`4 4`| Nhiều cực tiểu và cực đại | 
|`6 / 1 2 3 4 5 6`|`5 1`| Tối thiểu và tối đa duy nhất | 
|`6 / 1 1 1 10 10 10`|`9 9`| Logic nhân tần số | 

## Vỏ cạnh 

Xét trường hợp tất cả các bông hoa đều có vẻ đẹp như nhau:```
5
7 7 7 7 7
```Thuật toán tìm thấy`mn = mx = 7`, vì vậy nó đi vào nhánh đặc biệt. Sự khác biệt là`0`và số cặp tối ưu trở thành:$$\frac{5 \cdot 4}{2} = 10$$Đầu ra:```
0 10
```Điều này tránh được lỗi phổ biến trong tính toán`5 × 5 = 25`, tính các cặp tự ghép không hợp lệ và các cặp đếm đôi. 

Hãy xem xét một trường hợp với các thái cực lặp đi lặp lại:```
5
1 3 1 5 5
```Thuật toán tìm thấy`mn = 1`,`mx = 5`,`cnt_min = 2`, Và`cnt_max = 2`. 

Số cặp tối ưu là:$$2 \times 2 = 4$$Đầu ra:```
4 4
```Mỗi cặp tối ưu đều có một bông hoa đẹp`1`và một bông hoa với vẻ đẹp`5`. Không có cặp nào liên quan đến hoa với sắc đẹp`3`có thể đạt tới mức chênh lệch tối đa. 

Hãy xem xét đầu vào hợp lệ nhỏ nhất:```
2
100 1
```Thuật toán tính toán`mn = 1`,`mx = 100`và đếm một lần xuất hiện của mỗi lần. Câu trả lời trở thành:```
99 1
```Chỉ tồn tại một cặp và công thức đếm đương nhiên cho kết quả chính xác.
