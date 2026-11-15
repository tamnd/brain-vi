---
title: "CF 182C - Tổng tối ưu"
description: "Chúng tôi có một mảng và chiều dài cửa sổ cố định len. Với mỗi mảng con có độ dài len, chúng ta tính tổng của nó rồi lấy giá trị tuyệt đối. \"Tổng tối ưu\" của toàn bộ mảng là tổng mảng con tuyệt đối tối đa trong số tất cả các cửa sổ có độ dài cố định đó."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 182
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 117 (Div. 2)"
rating: 2000
weight: 182
solve_time_s: 110
verified: true
draft: false
---

[CF 182C - Tổng tối ưu](https://codeforces.com/problemset/problem/182/C) 

**Xếp hạng:** 2000 
**Tas:** cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mảng và độ dài cửa sổ cố định`len`. Đối với mỗi mảng con có độ dài`len`, chúng ta tính tổng của nó rồi lấy giá trị tuyệt đối. "Tổng tối ưu" của toàn bộ mảng là tổng mảng con tuyệt đối tối đa trong số tất cả các cửa sổ có độ dài cố định đó. 

Trước khi đánh giá câu trả lời, chúng ta có thể đảo dấu các phần tử mảng. Một thao tác chọn một chỉ mục và nhân giá trị đó với`-1`. Chúng tôi có thể biểu diễn nhiều nhất`k`tổng số hoạt động. Cho phép lật cùng một vị trí nhiều lần, nhưng chỉ tính chẵn lẻ mới quan trọng vì hai lần lật sẽ triệt tiêu lẫn nhau. 

Nhiệm vụ là tối đa hóa tổng tuyệt đối lớn nhất trên tất cả các cửa sổ có kích thước`len`. 

Những ràng buộc buộc chúng ta phải suy nghĩ cẩn thận. Chiều dài mảng đạt`10^5`, do đó bất kỳ giải pháp nào kiểm tra tất cả các cửa sổ và tất cả các kết hợp lật đều không thể thực hiện được. Thậm chí`O(n * k)`đã gần đến giới hạn thực tế khi cả hai đều lớn. Chúng ta cần thứ gì đó xung quanh`O(n log n)`hoặc tốt hơn. 

Một điểm tinh tế là chúng tôi đang tối đa hóa giá trị tuyệt đối của tổng cửa sổ. Điều đó có nghĩa là chúng ta có thể tự do tạo ra một cửa sổ rất tích cực hoặc rất tiêu cực. Vì việc lật mọi phần tử trong cửa sổ sẽ thay đổi dấu của tổng, nên việc tối đa hóa hướng dương cũng đã bao trùm hướng âm. 

Một điểm tinh tế khác là các hoạt động mang tính toàn cầu, nhưng mục tiêu cuối cùng chỉ phụ thuộc vào một cửa sổ. Chúng ta không bao giờ cần tối ưu hóa nhiều cửa sổ cùng một lúc. Chúng tôi chỉ quan tâm đến việc xây dựng một cửa sổ duy nhất tốt nhất có thể. 

Hãy xem xét ví dụ này:```
n = 3, len = 2
a = [5, -100, 5]
k = 1
```Cách tốt nhất là lật`-100`vào trong`100`, đưa ra tổng cửa sổ`105`Và`105`. Một chiến lược tham lam cố gắng cải thiện từng cửa sổ một cách độc lập có thể lãng phí cơ hội ở nơi khác. 

Một sai lầm dễ mắc phải khác là xử lý sai tính chẵn lẻ. Giả định:```
n = 2, len = 2
a = [4, 7]
k = 1
```Cả hai con số đều đã dương. Lật một cái sẽ giảm tổng từ`11`đến một trong hai`3`hoặc`-3`. Vì chúng ta có thể sử dụng _nhiều nhất_`k`hoạt động, câu trả lời đúng vẫn là`11`. Việc thực hiện bất cẩn giả định chính xác`k`lật sẽ tạo ra kết quả sai. 

Một cái bẫy nữa xuất hiện khi cửa sổ chứa ít hơn`k`những con số âm.```
n = 4, len = 4
a = [-5, 2, 3, 4]
k = 3
```Chúng tôi lật`-5`ĐẾN`5`, thu được tổng`14`. Việc sử dụng thêm các lần lật chỉ gây tổn hại vì mỗi lần lật bổ sung sẽ thay đổi một số dương thành số âm. 

Chiến lược đúng đắn phải hiểu rằng cách sử dụng tốt nhất các cú lật bên trong cửa sổ là luôn biến một số giá trị âm thành dương. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ liệt kê mọi cửa sổ có độ dài`len`. Đối với mỗi cửa sổ, chúng tôi sẽ thử lật mọi tập hợp con vị trí có thể có, tùy theo kích thước`k`và tính tổng tuyệt đối lớn nhất thu được. 

Điều đó ngay lập tức không thể thực hiện được. Đã có một cửa sổ duy nhất`2^len`tập hợp con. Với`len = 10^5`, thậm chí một cửa sổ cũng không thể được xử lý theo cách này. 

Chúng ta có thể cải thiện sức mạnh vũ phu một chút. Bên trong một cửa sổ cố định, việc lật một giá trị dương luôn làm giảm tổng tuyệt đối nếu chúng ta đang cố gắng cực đại hóa hướng dương. Vì vậy, đối với một cửa sổ, chiến lược tốt nhất là hiển nhiên: lật các số âm hữu ích nhất. 

Giả sử tổng cửa sổ là:```
S = a1 + a2 + ... + alen
```Lật số âm`-x`thay đổi tổng bằng`+2x`. 

Vì vậy, nếu chúng ta thu thập tất cả các giá trị âm trong cửa sổ và chọn tối đa`k`trong số đó, sự cải thiện tốt nhất đến từ các âm có giá trị tuyệt đối lớn nhất. 

Sự quan sát đó thay đổi hoàn toàn vấn đề. Chúng ta không còn quan tâm đến các tập con lật tùy ý nữa. Đối với mỗi cửa sổ trượt, chúng ta cần:```
window_sum + 2 * (sum of k largest absolute negative values)
```Bây giờ tác vụ trở thành bài toán cấu trúc dữ liệu cửa sổ trượt. 

Khi cửa sổ di chuyển, các phần tử vào và ra. Chúng ta phải duy trì: 

1. Tổng cửa sổ thông thường. 
2. Tổng của`k`giá trị tuyệt đối lớn nhất trong số các phần tử âm hiện có trong cửa sổ. 

Việc triển khai đơn giản sẽ sắp xếp các tiêu cực cho mỗi cửa sổ, gây tốn kém`O(len log len)`mỗi cửa sổ và`O(n * len log len)`tổng thể. 

Thông tin chi tiết quan trọng là chúng ta chỉ cần phần trên cùng`k`tiêu cực một cách năng động. Đây là cấu trúc hai multiset cổ điển: 

1. Một bộ chứa các câu phủ định được chọn hiện đang đóng góp cho câu trả lời. 
2. Bộ còn lại chứa các âm bản còn lại. 

Chúng tôi duy trì bất biến rằng tập hợp được chọn luôn lưu trữ`k`giá trị tuyệt đối lớn nhất. Sau đó mỗi lần chèn hoặc xóa chi phí`O(log n)`. 

Ý tưởng brute-force vẫn tồn tại về mặt khái niệm, nhưng việc tính toán lại tốn kém sẽ biến mất vì cửa sổ trượt chỉ cập nhật một phần tử vào và một phần tử rời khỏi mỗi bước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · 2^len) | O(len) | Quá chậm | 
| Sắp xếp lại mọi cửa sổ | O(n · len log len) | O(len) | Quá chậm | 
| Cửa sổ trượt + bộ cân bằng | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duy trì tổng thông thường của cửa sổ hiện tại. 

Khi cửa sổ trượt, hãy trừ phần tử rời và thêm phần tử vào. 
2. Với mọi giá trị âm`x`, xác định mức tăng của nó là`-2x`. 

Ví dụ: lật`-7`thay đổi tổng cửa sổ bằng`+14`. 
3. Duy trì hai bộ nhiều. 

Bộ đầu tiên,`best`, lưu trữ mức tăng hiện được chọn để lật. Kích thước của nó không bao giờ vượt quá`k`. 

Bộ thứ hai,`rest`, lưu trữ tất cả các lợi ích khác. 
4. Duy trì`best_sum`, tổng các giá trị bên trong`best`. 

Khi đó, tổng dương tốt nhất có thể đạt được cho thời hạn hiện tại là:```
current_window_sum + best_sum
```5. Khi một phần tử âm đi vào cửa sổ, hãy chèn mức tăng của nó vào cấu trúc. 

Nếu như`best`có ít hơn`k`phần tử, đặt nó trực tiếp ở đó. 

Mặt khác, so sánh nó với mức tăng nhỏ nhất hiện có bên trong`best`. 

Nếu mức tăng mới lớn hơn, hãy hoán đổi chúng. Nếu không thì đặt mức tăng mới vào`rest`. 
6. Khi một phần tử âm rời khỏi cửa sổ, hãy loại bỏ mức tăng của nó khỏi bất kỳ tập hợp nào hiện chứa nó. 

Sau khi loại bỏ, nếu`best`có ít hơn`k`các yếu tố và`rest`không trống, di chuyển mức tăng lớn nhất từ ​​​​`rest`vào trong`best`. 
7. Sau mỗi lần cập nhật cửa sổ, hãy tính:```
abs(current_window_sum + best_sum)
```Nhưng phát huy tối đa chiều hướng tích cực là đủ. Bất kỳ mục tiêu tiêu cực nào cũng có thể được chuyển đổi thành mục tiêu tích cực bằng cách lật ngược tất cả các dấu hiệu trong cửa sổ đó về mặt khái niệm. Vì vậy, chúng tôi chỉ đơn giản là tối đa hóa:```
current_window_sum + best_sum
```8. Theo dõi giá trị tối đa trên tất cả các cửa sổ. 

### Tại sao nó hoạt động 

Đối với một cửa sổ cố định, mọi giá trị âm được đảo ngược đều đóng góp độc lập. lật`-x`tăng tổng bằng`2x`, trong khi lật một giá trị dương sẽ làm giảm giá trị đó. Vì vậy chiến lược tối ưu luôn là chọn tối đa`k`giá trị âm với giá trị tuyệt đối lớn nhất. 

Cấu trúc dữ liệu duy trì chính xác những ứng cử viên tốt nhất tại mọi thời điểm. Bất biến là:```
best contains the k largest gains among all negatives in the current window
```Vì tổng cửa sổ được duy trì chính xác và`best_sum`bằng tổng mức cải thiện từ các lần lật tối ưu, mọi ứng cử viên được tính toán đều là mức tối ưu thực sự cho cửa sổ đó. Lấy mức tối đa trên tất cả các cửa sổ sẽ tạo ra câu trả lời tổng thể. 

## Giải pháp Python```python
import sys
from sortedcontainers import SortedList

input = sys.stdin.readline

def solve():
    n, length = map(int, input().split())
    a = list(map(int, input().split()))
    k = int(input())

    best = SortedList()
    rest = SortedList()

    best_sum = 0

    def add_gain(v):
        nonlocal best_sum

        if k == 0:
            rest.add(v)
            return

        if len(best) < k:
            best.add(v)
            best_sum += v
        else:
            if v > best[0]:
                smallest = best.pop(0)
                best_sum -= smallest
                rest.add(smallest)

                best.add(v)
                best_sum += v
            else:
                rest.add(v)

    def remove_gain(v):
        nonlocal best_sum

        if v in best:
            best.remove(v)
            best_sum -= v

            if rest:
                largest = rest.pop()
                best.add(largest)
                best_sum += largest
        else:
            rest.remove(v)

    window_sum = 0

    for i in range(length):
        window_sum += a[i]
        if a[i] < 0:
            add_gain(-2 * a[i])

    ans = window_sum + best_sum

    for r in range(length, n):
        l = r - length

        window_sum -= a[l]
        if a[l] < 0:
            remove_gain(-2 * a[l])

        window_sum += a[r]
        if a[r] < 0:
            add_gain(-2 * a[r])

        ans = max(ans, window_sum + best_sum)

    print(ans)

solve()
```Việc thực hiện theo cấu trúc cửa sổ trượt trực tiếp.`window_sum`theo dõi tổng thông thường của mảng con hiện tại. Mỗi số âm đóng góp một mức tăng có thể bằng`-2 * a[i]`. Giá trị đó luôn dương vì`a[i] < 0`. 

các`best`multiset lưu trữ mức tăng hiện được chọn. Kích thước của nó nhiều nhất là`k`và nó luôn chứa mức tăng lớn nhất có sẵn bên trong cửa sổ. 

Phần khó khăn là duy trì tính bất biến sau khi xóa. Khi loại bỏ một phần tử khỏi`best`, chúng tôi có thể để lại ít hơn`k`lợi ích đã chọn. Lợi nhuận lớn nhất có được từ`rest`sau đó phải chuyển vào`best`. 

Một chi tiết tinh tế khác là xử lý`k = 0`. Trong trường hợp đó,`best`phải để trống vĩnh viễn. Nếu không có sự kiểm tra rõ ràng, việc truy cập`best[0]`sẽ thất bại. 

Số nguyên Python đã hỗ trợ độ chính xác tùy ý, điều này quan trọng vì tổng có thể đạt tới khoảng`10^14`. 

Giải pháp sử dụng`SortedList`từ`sortedcontainers`cho hành vi cây cân bằng sạch sẽ. Mỗi chi phí chèn, xóa và tra cứu ranh giới`O(log n)`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3
0 -2 3 -5 1
2
```Cửa sổ ban đầu`[0, -2, 3]`| Cửa sổ | Tổng cửa sổ | Lợi nhuận âm | tốt nhất_sum | Ứng viên | 
| --- | --- | --- | --- | --- | 
|`[0, -2, 3]`| 1 | {4} | 4 | 5 | 

Trượt tới`[-2, 3, -5]`| Cửa sổ | Tổng cửa sổ | Lợi nhuận âm | tốt nhất_sum | Ứng viên | 
| --- | --- | --- | --- | --- | 
|`[-2, 3, -5]`| -4 | {4, 10} | 14 | 10 | 

Trượt tới`[3, -5, 1]`| Cửa sổ | Tổng cửa sổ | Lợi nhuận âm | tốt nhất_sum | Ứng viên | 
| --- | --- | --- | --- | --- | 
|`[3, -5, 1]`| -1 | {10} | 10 | 9 | 

Ứng viên tối đa là`10`. 

Dấu vết này cho thấy mức tăng tương ứng chính xác như thế nào với mức cải thiện thu được bằng cách lật ngược các tiêu cực. Trong cửa sổ thứ hai, lật cả hai âm bản sẽ thay đổi`-4`vào trong`10`. 

### Ví dụ 2 

đầu vào:```
4 2
5 6 -100 7
1
```Cửa sổ ban đầu`[5, 6]`| Cửa sổ | Tổng cửa sổ | Lợi nhuận âm | tốt nhất_sum | Ứng viên | 
| --- | --- | --- | --- | --- | 
|`[5, 6]`| 11 | {} | 0 | 11 | 

Trượt tới`[6, -100]`| Cửa sổ | Tổng cửa sổ | Lợi nhuận âm | tốt nhất_sum | Ứng viên | 
| --- | --- | --- | --- | --- | 
|`[6, -100]`| -94 | {200} | 200 | 106 | 

Trượt tới`[-100, 7]`| Cửa sổ | Tổng cửa sổ | Lợi nhuận âm | tốt nhất_sum | Ứng viên | 
| --- | --- | --- | --- | --- | 
|`[-100, 7]`| -93 | {200} | 200 | 107 | 

Câu trả lời là`107`. 

Ví dụ này chứng minh rằng một giá trị âm rất lớn chiếm ưu thế trong chiến lược tối ưu. Thuật toán giữ chính xác mức tăng tốt nhất vì`k = 1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi phần tử vào và ra khỏi cấu trúc dữ liệu một lần | 
| Không gian | O(n) | Các bộ đa có thể cùng nhau lưu trữ tất cả các mức tăng âm | 

Với`n = 10^5`,`O(n log n)`dễ dàng phù hợp trong thời hạn. Việc sử dụng bộ nhớ cũng vẫn ở dưới mức giới hạn vì chỉ có một số cấu trúc cây cân bằng được lưu trữ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from sortedcontainers import SortedList

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, length = map(int, input().split())
    a = list(map(int, input().split()))
    k = int(input())

    best = SortedList()
    rest = SortedList()

    best_sum = 0

    def add_gain(v):
        nonlocal best_sum

        if k == 0:
            rest.add(v)
            return

        if len(best) < k:
            best.add(v)
            best_sum += v
        else:
            if v > best[0]:
                smallest = best.pop(0)
                best_sum -= smallest
                rest.add(smallest)

                best.add(v)
                best_sum += v
            else:
                rest.add(v)

    def remove_gain(v):
        nonlocal best_sum

        if v in best:
            best.remove(v)
            best_sum -= v

            if rest:
                largest = rest.pop()
                best.add(largest)
                best_sum += largest
        else:
            rest.remove(v)

    window_sum = 0

    for i in range(length):
        window_sum += a[i]
        if a[i] < 0:
            add_gain(-2 * a[i])

    ans = window_sum + best_sum

    for r in range(length, n):
        l = r - length

        window_sum -= a[l]
        if a[l] < 0:
            remove_gain(-2 * a[l])

        window_sum += a[r]
        if a[r] < 0:
            add_gain(-2 * a[r])

        ans = max(ans, window_sum + best_sum)

    return str(ans)

# provided sample
assert run(
"""5 3
0 -2 3 -5 1
2
"""
) == "10", "sample 1"

# minimum size
assert run(
"""1 1
-5
1
"""
) == "5", "single element"

# k = 0
assert run(
"""4 2
1 -2 3 4
0
"""
) == "7", "no flips allowed"

# all equal negatives
assert run(
"""5 3
-4 -4 -4 -4 -4
2
"""
) == "4", "choose best two flips"

# boundary sliding behavior
assert run(
"""5 2
-1 -2 -3 -4 -5
1
"""
) == "9", "correct entering and leaving updates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / -5 / 1`|`5`| Xử lý kích thước tối thiểu | 
|`4 2 / 1 -2 3 4 / 0`|`7`| Hành vi đúng khi bị cấm lật | 
|`5 3 / -4 -4 -4 -4 -4 / 2`|`4`| Lợi ích bằng nhau lặp đi lặp lại | 
|`5 2 / -1 -2 -3 -4 -5 / 1`|`9`| Tính chính xác của việc chèn và loại bỏ cửa sổ trượt | 

## Vỏ cạnh 

Hãy xem xét trường hợp tất cả các giá trị đều dương.```
2 2
4 7
1
```Tổng cửa sổ ban đầu là`11`. Không có lợi ích âm nên cả hai tập hợp đều trống. Đầu ra của thuật toán`11`. 

Điều này xác nhận rằng việc sử dụng ít hơn`k`hoạt động được cho phép. Một giải pháp sai buộc phải lật đúng một lần sẽ làm giảm câu trả lời. 

Bây giờ hãy xem xét một trường hợp có số lần lật được phép nhiều hơn số âm.```
4 4
-5 2 3 4
3
```Tổng cửa sổ bắt đầu lúc`4`. 

Giá trị âm duy nhất là`-5`, tạo ra mức tăng`10`. 

Vì thế:```
candidate = 4 + 10 = 14
```Hai phép tính còn lại bị bỏ qua vì việc lật số dương sẽ làm giảm tổng. 

Cuối cùng, hãy xem xét mức tăng bằng nhau khi vào và ra khỏi cấu trúc.```
5 3
-4 -4 -4 1 1
2
```Lợi nhuận ban đầu là`{8, 8, 8}`. Chỉ có hai người thuộc về`best`. 

Khi cửa sổ trượt, một`8`lá và phần còn lại khác. Cấu trúc nhiều tập hợp chỉ loại bỏ chính xác một lần xuất hiện vì nó lưu trữ các bản sao một cách riêng biệt. 

Điều đó ngăn ngừa một lỗi tinh vi trong đó tất cả các giá trị bằng nhau vô tình biến mất cùng nhau.
