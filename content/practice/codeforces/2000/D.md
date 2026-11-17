---
title: "CF 2000D - Phải Trái Sai"
description: "Chúng tôi có một dòng tế bào. Mỗi ô chứa một giá trị dương a[i] và một ký tự L hoặc R. Một thao tác chọn một cặp vị trí (l, r) có l < r, trong đó vị trí l hiện chứa L và vị trí r hiện chứa R."
date: "2026-06-09T02:33:23+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2000
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 966 (Div. 3)"
rating: 1200
weight: 2000
solve_time_s: 364
verified: false
draft: false
---

[CF 2000D - Phải Trái Sai](https://codeforces.com/problemset/problem/2000/D) 

**Đánh giá:** 1200 
**Tags:** tham lam, thực hiện, hai con trỏ 
**Thời gian giải:** 6 phút 4 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dòng tế bào. Mỗi ô chứa một giá trị dương`a[i]`và một nhân vật, hoặc`L`hoặc`R`. 

Một thao tác chọn một cặp vị trí`(l, r)`với`l < r`, vị trí ở đâu`l`hiện có chứa`L`và vị trí`r`hiện có chứa`R`. Chúng tôi đạt được tổng của tất cả các giá trị giữa chúng, bao gồm cả. Sau đó, mọi vị trí trong khoảng`[l, r]`trở nên không sử dụng được. 

Các khoảng được tạo bởi các phép toán khác nhau không bao giờ có thể trùng lặp vì khi một khoảng được chọn, tất cả các vị trí của nó sẽ biến mất khỏi việc xem xét trong tương lai. 

Mục tiêu là tối đa hóa tổng số điểm. 

Các ràng buộc ngay lập tức gợi ý rằng chúng ta cần một cái gì đó gần với thời gian tuyến tính. Trong tất cả các trường hợp thử nghiệm, tổng số ô nhiều nhất là`2 · 10^5`, có nghĩa là một`O(n)`hoặc`O(n log n)`giải pháp dễ dàng đủ nhanh. MỘT`O(n²)`cách tiếp cận sẽ yêu cầu khoảng`4 · 10^10`trong trường hợp xấu nhất là hoàn toàn không thể thực hiện được. 

Khó khăn chính là việc chọn một khoảng sẽ loại bỏ nhiều vị trí, điều này ảnh hưởng đến những khoảng có thể được chọn sau này. Một chiến lược tham lam luôn chiếm khoảng thời gian lớn nhất hiện có rõ ràng là không đúng. 

Một số trường hợp đáng chú ý. 

Coi như:```
3
1 2 3
LLL
```không có`R`không hề. Không có hoạt động hợp lệ tồn tại, vì vậy câu trả lời là`0`. 

Việc thực hiện bất cẩn dẫn đến mọi`L`có thể được kết hợp sẽ thất bại ở đây. 

Coi như:```
4
5 5 5 5
LRLR
```Sự lựa chọn tối ưu là toàn bộ khoảng thời gian`[1,4]`, cho`20`. 

Lựa chọn`[1,2]`đầu tiên chỉ cho`10`, sau đó`[3,4]`đưa ra cái khác`10`. Tổng số vẫn là`20`, cho thấy các phân tách khác nhau có thể tạo ra kết quả tương tự. 

Bây giờ hãy xem xét:```
4
1 100 100 1
LRRL
```Cặp hợp lệ duy nhất sử dụng cặp cuối cùng`L`là không thể bởi vì không có`R`ở bên phải của nó. Sự lựa chọn tốt nhất là`[1,3]`, đáng giá`201`. 

Giải pháp cố gắng khớp các ký tự cục bộ mà không xem xét thứ tự vị trí có thể đếm không chính xác các cặp không hợp lệ. 

Một trường hợp tế nhị khác là:```
5
1 2 3 4 5
RLLLR
```Ký tự ngoài cùng bên trái là`R`, vì vậy nó không bao giờ có thể đóng vai trò là điểm cuối bên trái. Khoảng thời gian hữu ích duy nhất là`[2,5]`, đáng giá`14`. 

Thuật toán phải bỏ qua các ký tự không khớp ở ranh giới. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là suy nghĩ đệ quy. Bất cứ lúc nào chúng ta có thể chọn bất kỳ cặp hợp lệ nào`(l,r)`, lấy tổng khoảng tương ứng, xóa khoảng đó và tiếp tục với các phần còn lại. Khám phá tất cả các lựa chọn có thể cuối cùng tìm thấy sự tối ưu. 

Điều này đúng vì nó kiểm tra mọi trình tự hoạt động hợp pháp. 

Vấn đề là số lượng khả năng. Ngay cả việc quyết định khoảng thời gian hợp lệ nào sẽ thực hiện trước cũng có thể liên quan đến việc`O(n²)`các lựa chọn, và mỗi lựa chọn đều dẫn đến nhiều khả năng hơn. Không gian tìm kiếm tăng theo cấp số nhân và trở nên vô vọng từ rất lâu trước khi`n = 2 · 10^5`. 

Để tìm ra thứ gì đó nhanh hơn, chúng ta cần hiểu giải pháp tối ưu trông như thế nào. 

Giả sử chúng ta chọn một khoảng`[l,r]`. Mọi vị trí bên trong nó đều biến mất. Vì tất cả các giá trị đều dương nên bất cứ khi nào chúng ta có thể mở rộng một khoảng mà không vi phạm các yêu cầu về điểm cuối thì số điểm đạt được chỉ tăng lên. 

Quan sát này thay đổi cách chúng ta nhìn nhận vấn đề. Hãy tưởng tượng lấy phần ngoài cùng bên trái có sẵn`L`và ngoài cùng bên phải có sẵn`R`. Nếu`L`nằm trước`R`, khi đó việc sử dụng hai điểm cuối này sẽ thu được khoảng cách lớn nhất có thể có trong số tất cả các vị trí hiện còn lại. 

Điều gì xảy ra sau khi loại bỏ khoảng thời gian đó? Mọi vị trí bên trong đều biến mất nên mọi hoạt động trong tương lai phải diễn ra hoàn toàn bên ngoài nó. Vì chúng ta đã sử dụng các điểm cuối ngoài cùng có thể nên vấn đề còn lại sẽ trở thành vấn đề tương tự đối với các ô còn lại bên ngoài. 

Điều này tự nhiên dẫn đến một quá trình hai con trỏ. 

Đặt một con trỏ ở đầu bên trái và một con trỏ khác ở đầu bên phải. 

Di chuyển con trỏ bên trái cho đến khi nó đạt tới một`L`. 

Di chuyển con trỏ sang phải cho đến khi nó đạt tới một`R`. 

Nếu con trỏ bên trái vẫn ở trước con trỏ bên phải thì chúng ta nên ghép chúng lại. Bởi vì tất cả các số đều dương nên việc lấy cặp hợp lệ ngoài cùng sẽ nắm bắt mọi giá trị giữa chúng và không thể tệ hơn việc chia vùng đó thành các khoảng nhỏ hơn. 

Điểm của một khoảng có thể đạt được trong`O(1)`thời gian sử dụng tổng tiền tố. 

Sau khi sử dụng cặp, hãy di chuyển cả hai con trỏ vào trong và lặp lại. 

Toàn bộ mảng chỉ được quét một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Hai con trỏ tối ưu + Tổng tiền tố | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một mảng tổng tiền tố sao cho tổng của bất kỳ khoảng nào`[l,r]`có thể được tính toán trong thời gian không đổi. 
2. Khởi tạo hai con trỏ,`i = 0`Và`j = n - 1`. 
3. Di chuyển`i`ở bên phải cho đến khi nó trỏ đến một`L`hoặc vượt qua`j`. 
4. Di chuyển`j`sang trái cho đến khi nó trỏ tới một`R`hoặc vượt qua`i`. 
5. Nếu`i >= j`, không còn cặp hợp lệ nào nên dừng lại. 
6. Cộng tổng khoảng`[i, j]`cho câu trả lời bằng cách sử dụng tổng tiền tố. 
7. Tăng`i`và giảm dần`j`. 
8. Lặp lại từ bước 3. 

Hành động chính là bước 6. Sau khi còn lại ngoài cùng bên trái`L`và phần còn lại ngoài cùng bên phải`R`được tìm thấy, việc ghép nối chúng sẽ ngay lập tức ghi lại khoảng thời gian lớn nhất có thể có trong phân đoạn còn lại hiện tại. Vì mọi giá trị đều dương nên việc loại trừ bất kỳ ô nào khỏi khoảng đó chỉ có thể làm giảm điểm. 

### Tại sao nó hoạt động 

Ở bất kỳ giai đoạn nào, hãy xem xét phần còn lại ngoài cùng bên trái`L`và phần còn lại ngoài cùng bên phải`R`. Mọi thao tác hợp lệ đều phải sử dụng điểm cuối bên trái không vượt quá điểm cuối đó`L`và một điểm cuối đúng không có quyền nào hơn thế`R`. 

Khoảng được hình thành bởi các điểm cuối hợp lệ ngoài cùng này chứa mọi khoảng có thể được hình thành bằng cách sử dụng các điểm cuối còn lại khác. Vì tất cả các giá trị mảng đều dương nên tổng của khoảng lớn hơn ít nhất cũng lớn bằng tổng của bất kỳ khoảng nhỏ hơn nào bên trong nó. 

Sau khi chọn khoảng ngoài cùng này, tất cả các vị trí bên trong nó sẽ biến mất. Mọi hoạt động trong tương lai phải nằm hoàn toàn bên ngoài khoảng đó. Do đó bài toán con còn lại độc lập với khoảng chúng ta vừa lấy. 

Việc lặp lại lập luận này ở mỗi bước cho thấy lựa chọn tham lam luôn an toàn và thuật toán tạo ra số điểm tối đa có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        s = input().strip()

        pref = [0] * (n + 1)
        for i in range(n):
            pref[i + 1] = pref[i] + a[i]

        i = 0
        j = n - 1
        ans = 0

        while True:
            while i < n and s[i] != 'L':
                i += 1

            while j >= 0 and s[j] != 'R':
                j -= 1

            if i >= j:
                break

            ans += pref[j + 1] - pref[i]

            i += 1
            j -= 1

        print(ans)

solve()
```Mảng tổng tiền tố cho phép tính tổng khoảng thời gian theo thời gian không đổi. Giá trị của khoảng`[i, j]`là:```
pref[j + 1] - pref[i]
```Hai con trỏ tìm kiếm lần sử dụng tiếp theo`L`từ bên trái và lần sử dụng tiếp theo`R`từ bên phải. 

Sau khi sử dụng một cặp, cả hai con trỏ đều di chuyển vào trong. Điều này sẽ loại bỏ toàn bộ khoảng thời gian đó một cách hiệu quả khỏi việc xem xét trong tương lai, khớp với lập luận tham lam khỏi bằng chứng. 

Một sai lầm dễ dàng là sử dụng`pref[j] - pref[i]`, điều này sẽ loại trừ`a[j]`. Một lỗi phổ biến khác là quên rằng con trỏ phải tiếp tục tìm kiếm sau khi được di chuyển vào trong, vì các vị trí tiếp theo có thể không phải là điểm cuối hợp lệ. 

Số nguyên Python không bị giới hạn nên không có vấn đề tràn. Câu trả lời tối đa có thể có thể lớn hơn nhiều so với`32-bit`giới hạn số nguyên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
3 5 1 4 3 2
LRLLLR
```Tổng tiền tố:```
[0, 3, 8, 9, 13, 16, 18]
```| Bước | tôi | j | Khoảng thời gian | Đạt được | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 5 | [0,5] | 18 | 18 | 
| Dừng lại | 1 | 4 | không có cặp hợp lệ | 0 | 18 | 

Ký tự đầu tiên đã có rồi`L`và ký tự cuối cùng đã có rồi`R`, nên lựa chọn tham lam sẽ ngay lập tức chiếm toàn bộ dải. Vì tất cả các giá trị đều dương nên không có phép phân tách thay thế nào có thể vượt quá tổng tổng của toàn bộ mảng. 

### Ví dụ 2 

đầu vào:```
5
1 2 3 4 5
LRLRR
```Tổng tiền tố:```
[0,1,3,6,10,15]
```| Bước | tôi | j | Khoảng thời gian | Đạt được | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 4 | [0,4] | 15 | 15 | 
| 2 | 2 | 3 | [2,3] | 7 | 22 | 
| Dừng lại | 3 | 2 | không có cặp hợp lệ | 0 | 22 | 

Hoạt động đầu tiên sử dụng các điểm cuối và lợi ích ngoài cùng`15`. Sau khi di chuyển vào trong, một cặp hợp lệ khác vẫn còn lại, tạo ra một cặp bổ sung`7`. Câu trả lời cuối cùng là`22`. 

Ví dụ này chứng minh rằng các khoảng không bắt buộc phải rời rạc trong mảng ban đầu. Quá trình tham lam đang hoạt động trên các điểm cuối còn lại chưa bị xóa, tương ứng chính xác với trò chơi dự định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi con trỏ di chuyển qua mảng nhiều nhất một lần | 
| Không gian | O(n) | Cửa hàng mảng tổng tiền tố`n + 1`giá trị | 

Tổng chiều dài của tất cả các trường hợp thử nghiệm nhiều nhất là`2 · 10^5`. Quét tuyến tính cho mỗi trường hợp thử nghiệm chỉ thực hiện tổng thể vài trăm nghìn thao tác, nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    out = []

    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        s = input().strip()

        pref = [0] * (n + 1)
        for i in range(n):
            pref[i + 1] = pref[i] + a[i]

        l, r = 0, n - 1
        ans = 0

        while True:
            while l < n and s[l] != 'L':
                l += 1

            while r >= 0 and s[r] != 'R':
                r -= 1

            if l >= r:
                break

            ans += pref[r + 1] - pref[l]

            l += 1
            r -= 1

        out.append(str(ans))

    return "\n".join(out) + "\n"

# provided samples
assert run(
"""4
6
3 5 1 4 3 2
LRLLLR
2
2 8
LR
2
3 9
RL
5
1 2 3 4 5
LRLRR
"""
) == "18\n10\n0\n22\n"

# minimum size, valid pair
assert run(
"""1
2
1 1
LR
"""
) == "2\n"

# minimum size, invalid pair
assert run(
"""1
2
1 1
RL
"""
) == "0\n"

# all L
assert run(
"""1
5
1 2 3 4 5
LLLLL
"""
) == "0\n"

# all R
assert run(
"""1
5
1 2 3 4 5
RRRRR
"""
) == "0\n"

# nested pair structure
assert run(
"""1
4
5 5 5 5
LRLR
"""
) == "20\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=2, LR`|`2`| Trường hợp hợp lệ nhỏ nhất | 
|`n=2, RL`|`0`| Không có cặp hợp lệ nào tồn tại | 
| Tất cả`L`|`0`| Thiếu điểm cuối đúng | 
| Tất cả`R`|`0`| Thiếu điểm cuối bên trái | 
|`LRLR`có giá trị bằng nhau |`20`| Nhiều cặp tham lam và tổng khoảng cách | 

## Vỏ cạnh 

Hãy xem xét:```
1
2
3 9
RL
```Con trỏ bên trái tìm kiếm một`L`và tìm thấy vị trí`2`. Con trỏ bên phải tìm kiếm một`R`và tìm thấy vị trí`1`. Từ`i >= j`, không tồn tại khoảng hợp lệ. 

Các đầu ra thuật toán:```
0
```cái nào đúng. 

Coi như:```
1
5
1 2 3 4 5
LLLLL
```Con trỏ bên phải không bao giờ tìm thấy`R`. Nó di chuyển qua phần đầu của mảng, gây ra`i >= j`. Thuật toán kết thúc ngay lập tức và trả về`0`. 

Coi như:```
1
5
1 2 3 4 5
RLLLR
```Con trỏ trái bỏ qua ký tự đầu tiên vì nó`R`. Cuối cùng nó dừng lại ở vị trí`2`, trong khi con trỏ bên phải dừng ở vị trí`5`. 

Khoảng thời gian được chọn là:```
[2,5]
```với giá trị:```
2 + 3 + 4 + 5 = 14
```Đầu ra của thuật toán`14`, bỏ qua một cách chính xác những gì chưa từng có`R`lúc đầu. 

Cuối cùng, hãy xem xét:```
1
4
5 5 5 5
LRLR
```Cặp đầu tiên sử dụng vị trí`(1,4)`và lợi ích`20`. Sau khi di chuyển vào trong, các điểm cuối còn lại giao nhau và quá trình dừng lại. 

Đầu ra là:```
20
```phù hợp với số điểm tối đa có thể và minh họa lý do tại sao lấy cặp hợp lệ ngoài cùng là tối ưu.
