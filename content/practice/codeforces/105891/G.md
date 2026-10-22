---
title: "CF 105891G - sinh viên"
description: "Chúng ta được xếp một hàng học sinh, mỗi em có một số điểm cố định. Giáo viên liên tục chọn một học sinh và kiểm tra xem học sinh đó có thể bị xóa khỏi hàng hay không."
date: "2026-06-21T15:10:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105891
codeforces_index: "G"
codeforces_contest_name: "The 13th Shaanxi Provincial Collegiate Programming Contest"
rating: 0
weight: 105891
solve_time_s: 72
verified: true
draft: false
---

[CF 105891G - sinh viên](https://codeforces.com/problemset/problem/105891/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được xếp một hàng học sinh, mỗi em có một số điểm cố định. Giáo viên liên tục chọn một học sinh và kiểm tra xem học sinh đó có thể bị xóa khỏi hàng hay không. Dù là sinh viên`i`biến mất chỉ phụ thuộc vào giá trị tương đối của học sinh ở bên trái và bên phải của`i`vào thời điểm đó. 

Một học sinh có thể bị loại bỏ nếu trình tự xung quanh họ bị “trộn lẫn” theo một cách cụ thể. Cụ thể, học sinh sẽ biến mất nếu có bằng chứng về cả điểm cao hơn và điểm thấp hơn được phân chia ở bên trái và bên phải theo một trong hai mô hình đối xứng. Một mẫu yêu cầu giá trị không nhỏ hơn ở bên trái và giá trị không lớn hơn ở bên phải, còn mẫu kia hoán đổi các vai trò này. 

Giáo viên có thể chọn loại bỏ theo bất kỳ thứ tự nào và sau khi loại bỏ, các học sinh còn lại sẽ thay đổi, do đó các điều kiện được đánh giá linh hoạt. Mục đích là để tối đa hóa số lượng học sinh có thể bị xóa, hoặc tương đương là giảm thiểu số lượng học sinh không bao giờ có thể bị xóa cho dù chúng ta có chọn thứ tự xóa khéo léo đến đâu. 

Các ràng buộc cho phép lên đến`2 × 10^5`sinh viên, do đó, bất kỳ giải pháp nào thử xóa hoặc tính toán lại các điều kiện nhiều lần bằng cách quét qua mảng đều sẽ quá chậm. Mô phỏng động bậc hai hoặc thậm chí gần bậc hai đối với việc loại bỏ không thể hoạt động. Chúng ta cần mô tả đặc điểm của phần tử nào vốn đã “ổn định” theo tất cả các lệnh xóa hợp lệ. 

Trường hợp cạnh tinh tế xuất hiện khi mảng đơn điệu. Trong một mảng tăng nghiêm ngặt như`1 2 3 4 5`, mọi phần tử ở giữa có cả phần tử nhỏ hơn ở bên trái và phần tử lớn hơn ở bên phải, vì vậy nhiều phần tử trong số đó có thể tháo rời được. Tuy nhiên, các điểm cuối hoạt động khác vì một bên trống, điều này khiến điều kiện không được thỏa mãn. Bất kỳ giải pháp đúng nào cũng phải xử lý các vị trí biên một cách chính xác. 

Một trường hợp phức tạp khác là khi các giá trị lặp lại, chẳng hạn`3 1 3 2 3`. Chỉ so sánh địa phương là không đủ; điều quan trọng là liệu có tồn tại ít nhất một phần tử lớn hơn và một phần tử nhỏ hơn ở hai phía đối diện của một vị trí hay không. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ quét mảng liên tục, tìm tất cả các học sinh hiện có thể tháo rời, xóa một học sinh và lặp lại cho đến khi không thể xóa được nữa. Mỗi tờ séc cho một học sinh yêu cầu quét cả hai mặt, tính chi phí`O(n)`, và điều này có khả năng xảy ra`O(n)`lần, dẫn đến`O(n^2)`hoặc tệ hơn. Với`n`lên đến`2 × 10^5`, điều này vượt xa giới hạn khả thi. 

Quan sát quan trọng là điều kiện loại bỏ không phụ thuộc vào sự liền kề hoặc cấu trúc cục bộ mà chỉ phụ thuộc vào việc liệu có tồn tại những bất bình đẳng nhất định ở mỗi bên hay không. Khi chúng tôi diễn đạt lại mọi thứ theo thông tin tiền tố và hậu tố, chúng tôi có thể xác định liệu một vị trí có thể xóa được mà không cần mô phỏng thao tác xóa hay không. 

Một học sinh chỉ có thể được gỡ bỏ khi giá trị của chúng bị "kẹp" theo cách phân chia: có một giá trị ở một bên ít nhất là lớn bằng và một giá trị ở phía bên kia lớn nhất là bằng (hoặc tình huống đối xứng). Điều này có nghĩa là khả năng loại bỏ phụ thuộc vào việc mảng có chứa cả giá trị lớn hơn và nhỏ hơn được phân bổ trên các vị trí tương ứng với`i`. 

Những học sinh không bao giờ có thể bị loại bỏ chính xác là những học sinh vẫn còn cực trị theo ít nhất một hướng: tiền tố tối đa, tiền tố tối thiểu, hậu tố tối đa hoặc hậu tố tối thiểu. Bất kỳ vị trí nào không được bảo vệ bởi ít nhất một trong các thuộc tính cực đoan này cuối cùng đều có thể bị lộ và loại bỏ bằng cách chọn thứ tự xóa phù hợp. 

Do đó, thay vì mô phỏng việc xóa, chúng tôi tính toán trước bốn bộ: tiền tố cực đại, tiền tố cực tiểu, hậu tố cực đại và hậu tố cực tiểu. Bất kỳ chỉ mục nào thuộc ít nhất một trong các bộ này đều an toàn và sẽ vẫn ở cấu hình cuối cùng. Câu trả lời là quy mô liên minh của họ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng việc xóa Brute Force | O(n²) | O(n) | Quá chậm | 
| Phân loại cực trị tiền tố/hậu tố | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán số tiền tố tối đa trong khi quét từ trái sang phải, đánh dấu mọi vị trí có giá trị lớn nhất được thấy cho đến nay. Điều này nắm bắt các yếu tố không thể bị chi phối từ bên trái. 
2. Tính toán số tiền tố tối thiểu trong cùng một lần quét, đánh dấu các vị trí nhỏ nhất được thấy cho đến nay. Những phần tử này không thể được “vượt qua xuống” từ phía bên trái. 
3. Tính toán mức tối đa của hậu tố trong khi quét từ phải sang trái, đánh dấu các vị trí có giá trị lớn nhất trong hậu tố của chúng. Những điều này không thể bị thống trị từ bên phải. 
4. Tính hậu tố tối thiểu tương tự từ phải sang trái. 
5. Đánh dấu mọi chỉ mục xuất hiện trong bất kỳ loại nào trong bốn loại. Câu trả lời cuối cùng là số lượng chỉ số được đánh dấu. 

Lý do mỗi danh mục quan trọng là việc loại bỏ đòi hỏi sự tồn tại của cả giá trị lớn hơn và giá trị nhỏ hơn theo các hướng xung đột. Nếu một vị trí là tiền tố tối đa thì không có gì ở bên trái lớn hơn, điều này sẽ chặn một trong hai cách kích hoạt khả năng loại bỏ. Ý tưởng tương tự được áp dụng một cách đối xứng cho ba trường hợp còn lại. 

### Tại sao nó hoạt động 

Điều kiện loại bỏ được yêu cầu phải chứng kiến đồng thời cả hai giá trị không nhỏ hơn`a[i]`ở một bên và có giá trị không lớn hơn`a[i]`ở phía bên kia, trong một số cấu hình phân chia. Nếu một quan điểm là cực đoan theo bất kỳ ý nghĩa nào trong bốn hướng, thì ít nhất một bên không cung cấp được cấu trúc bất bình đẳng cần thiết để “kẹp” nó. Do đó, những vị trí như vậy không bao giờ có thể được bộc lộ đầy đủ bởi bất kỳ chuỗi xóa nào. Ngược lại, bất kỳ vị trí nào không được bảo vệ bởi các ràng buộc cực đoan này đều có thể được di dời bằng cách trước tiên loại bỏ các phần tử chặn, cuối cùng tạo ra cấu hình phân chia cần thiết. Điều này làm giảm vấn đề xác định tất cả các chỉ số cực trị theo ít nhất một hướng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

pref_max = [False] * n
pref_min = [False] * n
suf_max = [False] * n
suf_min = [False] * n

cur = -10**18
for i in range(n):
    if a[i] >= cur:
        pref_max[i] = True
        cur = a[i]

cur = 10**18
for i in range(n):
    if a[i] <= cur:
        pref_min[i] = True
        cur = a[i]

cur = -10**18
for i in range(n - 1, -1, -1):
    if a[i] >= cur:
        suf_max[i] = True
        cur = a[i]

cur = 10**18
for i in range(n - 1, -1, -1):
    if a[i] <= cur:
        suf_min[i] = True
        cur = a[i]

ans = 0
for i in range(n):
    if pref_max[i] or pref_min[i] or suf_max[i] or suf_min[i]:
        ans += 1

print(ans)
```Mã tính toán bốn lần quét tuyến tính trên mảng. Mỗi lần quét duy trì giá trị tốt nhất được thấy cho đến nay theo hướng tương ứng và đánh dấu các vị trí đạt được cực trị mới. 

Vòng lặp cuối cùng chỉ đếm các chỉ số thuộc ít nhất một loại cực trị. Điều này tránh mọi mô phỏng xóa, quá chậm và cũng không cần thiết khi đã biết đặc tính cấu trúc. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng`1 2 3 4 5`. 

Cực đại tiền tố xảy ra ở mọi vị trí, vì mỗi phần tử mới lớn hơn tất cả các phần tử trước đó. 

| tôi | giá trị | tiền tố tối đa? | tiền tố tối thiểu? | hậu tố tối đa? | hậu tố phút? | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | vâng | vâng | không | không | 
| 2 | 2 | vâng | không | không | không | 
| 3 | 3 | vâng | không | không | không | 
| 4 | 4 | vâng | không | không | không | 
| 5 | 5 | vâng | không | vâng | vâng | 

Sự kết hợp của các vị trí được đánh dấu bao gồm tất cả các chỉ số, vì vậy tất cả học sinh đều được giữ lại. 

Bây giờ hãy xem xét`5 1 4 2 3`. 

Chúng tôi theo dõi các vị trí cực đoan: 

| tôi | giá trị | tiền tố tối đa? | tiền tố tối thiểu? | hậu tố tối đa? | hậu tố phút? | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | vâng | vâng | vâng | không | 
| 2 | 1 | không | vâng | không | vâng | 
| 3 | 4 | vâng | không | không | không | 
| 4 | 2 | không | không | không | vâng | 
| 5 | 3 | không | không | vâng | vâng | 

Mỗi chỉ số được bao phủ bởi ít nhất một thuộc tính cực trị, vì vậy câu trả lời cuối cùng là 5. 

Những ví dụ này cho thấy rằng tập hợp còn lại không chỉ gắn với các phân đoạn đơn điệu mà còn gắn với việc một vị trí có bao giờ đặt hoặc khớp với một cực trị định hướng hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Bốn lần quét tuyến tính cộng với một lần quét cuối cùng | 
| Không gian | O(n) | Mảng Boolean cho bốn điểm đánh dấu hướng | 

Giải pháp chạy thoải mái trong giới hạn cho`n ≤ 2 × 10^5`, vì nó chỉ thực hiện một số lần chuyển qua đầu vào không đổi và sử dụng bộ nhớ phụ tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    pref_max = [False] * n
    pref_min = [False] * n
    suf_max = [False] * n
    suf_min = [False] * n

    cur = -10**18
    for i in range(n):
        if a[i] >= cur:
            pref_max[i] = True
            cur = a[i]

    cur = 10**18
    for i in range(n):
        if a[i] <= cur:
            pref_min[i] = True
            cur = a[i]

    cur = -10**18
    for i in range(n - 1, -1, -1):
        if a[i] >= cur:
            suf_max[i] = True
            cur = a[i]

    cur = 10**18
    for i in range(n - 1, -1, -1):
        if a[i] <= cur:
            suf_min[i] = True
            cur = a[i]

    ans = 0
    for i in range(n):
        if pref_max[i] or pref_min[i] or suf_max[i] or suf_min[i]:
            ans += 1

    return str(ans)

# minimum size
assert run("1\n5") == "1", "single element"

# strictly increasing
assert run("5\n1 2 3 4 5") == "5", "increasing"

# strictly decreasing
assert run("5\n5 4 3 2 1") == "5", "decreasing"

# mixed values
assert run("5\n5 1 4 2 3") == "5", "mixed"

# all equal
assert run("4\n7 7 7 7") == "4", "equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1 | xử lý ranh giới tối thiểu | 
| trình tự tăng dần | 5 | tiền tố lan truyền cực trị | 
| dãy giảm dần | 5 | hậu tố lan truyền cực trị | 
| trình tự hỗn hợp | 5 | logic hợp đúng đắn | 
| tất cả đều bình đẳng | 4 | xử lý trùng lặp | 

## Vỏ cạnh 

Đối với một mảng phần tử duy nhất như`10`, cả bốn lần quét ngay lập tức đánh dấu vị trí duy nhất là cực trị theo mọi hướng. Không có bên trái hoặc bên phải, vì vậy thuật toán tính chính xác nó là người sống sót. 

Đối với một chuỗi tăng nghiêm ngặt như`1 2 3 4 5`, mọi vị trí đều là tiền tố tối đa. Trong quá trình quét tiền tố, mỗi phần tử sẽ trở thành mức chạy tối đa mới, do đó tất cả các vị trí đều được đánh dấu, tạo ra một tập hợp người sống sót đầy đủ. 

Đối với một chuỗi có giá trị bằng nhau như`3 3 3 3`, mọi phần tử đồng thời là tiền tố tối đa và tiền tố tối thiểu tại các thời điểm khác nhau tùy thuộc vào việc xử lý đẳng thức. Quá trình quét đánh dấu tất cả các vị trí và liên kết cuối cùng bao gồm chính xác mọi chỉ mục.
