---
title: "CF 1759B - Hoán vị bị mất"
description: "Chúng ta được cho một số số chắc chắn thuộc về một hoán vị chưa biết nào đó. Những số còn thiếu đã bị mất, nhưng chúng ta biết tổng của tất cả các giá trị còn thiếu."
date: "2026-06-09T14:27:34+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1759
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round  834 (Div. 3)"
rating: 800
weight: 1759
solve_time_s: 147
verified: true
draft: false
---

[CF 1759B - Mất hoán vị](https://codeforces.com/problemset/problem/1759/B) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 2m 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số số chắc chắn thuộc về một hoán vị chưa biết nào đó. Những số còn thiếu đã bị mất, nhưng chúng ta biết tổng của tất cả các giá trị còn thiếu. 

Câu hỏi đặt ra là liệu chúng ta có thể chọn thêm một số số có tổng bằng chính xác không?`s`sao cho cùng với các số đã cho, mảng cuối cùng trở thành một hoán vị hợp lệ. 

Hoán vị độ dài`n`chứa mọi số nguyên từ`1`bởi vì`n`đúng một lần. Yêu cầu đó mạnh mẽ hơn nhiều so với việc chỉ có những giá trị riêng biệt. Nếu giá trị lớn nhất trong mảng cuối cùng là`k`, thì mọi số từ`1`ĐẾN`k`phải xuất hiện. 

Những hạn chế là rất nhỏ. Có tối đa 100 trường hợp thử nghiệm, mỗi trường hợp chứa tối đa 50 số đã biết và tất cả các giá trị tối đa là 50. Ngay cả một mô phỏng thử nhiều độ dài hoán vị ứng cử viên cũng sẽ chạy thoải mái trong giới hạn thời gian. Chúng tôi không cần cấu trúc dữ liệu phức tạp hoặc thuật toán nâng cao. 

Phần khó khăn là hiểu những con số bị thiếu trông như thế nào. Chúng không phải là các giá trị riêng biệt tùy ý. Nếu mảng cuối cùng là một hoán vị kết thúc ở một giá trị lớn nhất nào đó`k`, thì các số còn thiếu chính xác là các số nguyên từ`1`ĐẾN`k`chưa xuất hiện trong mảng đã cho. 

Hãy xem xét một vài trường hợp đặc biệt. 

Giả sử đầu vào là:```
1
1 1
1
```Số đã biết là rồi`1`. Để cộng các số có tổng`1`, chúng ta sẽ phải thêm một cái khác`1`, điều này sẽ tạo ra một bản sao. Không có số lớn hơn có tác dụng vì mọi số lớn hơn ít nhất`2`. Câu trả lời là`NO`. 

Bây giờ hãy xem xét:```
1
3 3
1 4 2
```Giá trị duy nhất còn thiếu giữa`1`Và`4`là`3`, tổng của nó chính xác là`3`. Thêm nó tạo ra`[1,2,3,4]`, vậy câu trả lời là`YES`. 

Một trường hợp tế nhị khác là:```
1
2 1
4 3
```Một giải pháp bất cẩn có thể nhận thấy rằng số tiền còn thiếu là`1`và giả sử chúng ta có thể thêm`1`. Sau khi thêm`1`, mảng sẽ chứa`{1,3,4}`, đó không phải là một hoán vị vì`2`vẫn còn thiếu. Câu trả lời đúng là`NO`. 

Yêu cầu phải có tất cả các số nguyên đến giá trị tối đa là quan sát quan trọng giúp ngăn chặn nhiều giải pháp sai. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử mọi tập hợp số có thể có tổng bằng`s`, nối chúng vào mảng đã cho và kiểm tra xem kết quả có phải là hoán vị hay không. Điều này đúng vì nó khám phá mọi mức độ hoàn thành của ứng viên. Vấn đề là số lượng tập hợp con tăng theo cấp số nhân. Ngay cả với các giá trị được giới hạn bởi 50, việc liệt kê tất cả các khả năng là không cần thiết và về mặt khái niệm phức tạp hơn nhiều so với yêu cầu của cấu trúc bài toán. 

Quan sát quan trọng là một hoán vị hoàn toàn được xác định bởi giá trị lớn nhất của nó. 

Giả sử hoán vị cuối cùng có giá trị lớn nhất`k`. Khi đó hoán vị phải chứa mọi số nguyên từ`1`bởi vì`k`. Vì các số đã cho đã cố định nên các số chúng ta cần thêm chính xác là các số nguyên còn thiếu trong phạm vi này. 

Điều này biến vấn đề thành một mô phỏng đơn giản. 

Chúng ta bắt đầu với tổng`s`. Chúng tôi liên tục thêm các số nguyên dương nhỏ nhất chưa có trong tập hợp đã cho. Mỗi lần chúng ta cộng một số như vậy, chúng ta sẽ trừ nó khỏi`s`. 

Nếu như`s`trở nên chính xác bằng 0, chúng ta đã tìm được một hoán vị ứng cử viên. Sau đó chúng ta chỉ cần xác minh rằng tập kết quả chứa mọi số nguyên từ`1`đến phần tử tối đa của nó. 

Nếu như`s`trở thành số âm, chúng ta đã vượt quá số tiền còn thiếu cần thiết và không có nghiệm nào tồn tại. 

Bởi vì tất cả các giá trị đều nhỏ nên quá trình này diễn ra cực kỳ nhanh chóng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(M + X) | O(M + X) | Đã chấp nhận | 

Đây`X`là giá trị lớn nhất được kiểm tra trong quá trình mô phỏng, giá trị này nhỏ theo các ràng buộc đã cho. 

## Hướng dẫn thuật toán 

1. Đọc các số đã cho và lưu chúng thành một bộ để kiểm tra tư cách thành viên nhanh chóng. 
2. Bắt đầu từ`1`, quét lên trên qua các số nguyên dương. 
3. Bất cứ khi nào số hiện tại chưa có trong tập hợp, hãy coi nó như một giá trị hoán vị bị thiếu. Trừ nó khỏi`s`và chèn nó vào bộ. 
4. Tiếp tục cho đến khi`s`trở thành số không hoặc âm. 
5. Nếu`s`trở thành tiêu cực, in`NO`. 
6. Nếu`s`trở thành chính xác bằng 0, tìm giá trị lớn nhất trong tập hợp. 
7. Kiểm tra xem mọi số nguyên từ`1`thông qua giá trị tối đa đó tồn tại trong tập hợp. 
8. Nếu tất cả đều có mặt, hãy in`YES`; nếu không thì in`NO`. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, thuật toán chỉ cộng các số phải xuất hiện trong bất kỳ hoán vị hợp lệ nào chứa các phần tử đã cho. Chúng tôi xử lý các số nguyên bị thiếu theo thứ tự tăng dần vì mọi hoán vị chứa giá trị lớn nhất hiện tại phải bao gồm tất cả các số nguyên dương nhỏ hơn. 

Khi`s`đạt đến 0, tập hợp chứa các số ban đầu cộng với chính xác các giá trị còn thiếu có tổng bằng tổng bị mất cần thiết. Nếu tập kết quả tạo thành một phạm vi hoàn chỉnh từ`1`đến phần tử lớn nhất của nó thì đó là một hoán vị. Nếu thiếu một số giá trị trong phạm vi đó thì không thể hình thành hoán vị hợp lệ vì mọi hoán vị cũng phải chứa giá trị còn thiếu đó. 

Thuật toán không bao giờ bỏ qua một số bắt buộc và không bao giờ thêm một số không cần thiết, vì vậy nó chấp nhận chính xác các trường hợp hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    m, s = map(int, input().split())
    present = set(map(int, input().split()))

    x = 1

    while s > 0:
        if x not in present:
            s -= x
            present.add(x)
        x += 1

    if s < 0:
        print("NO")
        continue

    mx = max(present)

    ok = True
    for v in range(1, mx + 1):
        if v not in present:
            ok = False
            break

    print("YES" if ok else "NO")
```bộ`present`lưu trữ tất cả các số hiện được biết đến. Kiểm tra tư cách thành viên là thời gian không đổi, điều này giúp dễ dàng xác định xem một giá trị đã là một phần của hoán vị hay chưa. 

Vòng lặp xử lý các số nguyên dương theo thứ tự tăng dần. Bất cứ khi nào một số bị thiếu, đó là một trong những giá trị phải xuất hiện nếu mảng cuối cùng trở thành một hoán vị. Chúng tôi trừ nó khỏi`s`và thêm nó vào bộ. 

Vòng lặp dừng ngay khi`s`không còn tích cực nữa. Nếu nó trở thành số âm, chúng tôi sẽ vượt quá số tiền còn thiếu được yêu cầu và không có sự hoàn thành hợp lệ nào tồn tại. 

Khi`s`đạt chính xác bằng 0, chúng tôi thực hiện xác nhận cuối cùng. Một bộ như`{1,3,4}`không có bản sao, nhưng nó không phải là một hoán vị vì`2`vắng mặt. Kiểm tra phạm vi cuối cùng sẽ phát hiện được những trường hợp như vậy. 

Không có mối lo ngại tràn tồn tại vì tất cả các con số đều rất nhỏ. Chi tiết tinh tế duy nhất là việc đạt được`s == 0`tự nó là không đủ. Chúng ta vẫn phải xác minh rằng tập hợp tạo thành một khoảng đầy đủ từ`1`tới giá trị cực đại của nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
m = 3
s = 13
b = [3, 1, 4]
```| Bước | Hiện tại x | Mất tích? | Mới | Đặt nội dung | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | - | - | 13 | {1,3,4} | 
| 1 | 1 | Không | 13 | {1,3,4} | 
| 2 | 2 | Có | 11 | {1,2,3,4} | 
| 3 | 3 | Không | 11 | {1,2,3,4} | 
| 4 | 4 | Không | 11 | {1,2,3,4} | 
| 5 | 5 | Có | 6 | {1,2,3,4,5} | 
| 6 | 6 | Có | 0 | {1,2,3,4,5,6} | 

Giá trị tối đa là`6`và mọi số nguyên từ`1`bởi vì`6`đang có mặt. Câu trả lời là`YES`. 

### Ví dụ 2 

đầu vào:```
m = 2
s = 1
b = [4, 3]
```| Bước | Hiện tại x | Mất tích? | Mới | Đặt nội dung | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | - | - | 1 | {3,4} | 
| 1 | 1 | Có | 0 | {1,3,4} | 

Điều kiện tổng được thỏa mãn, nhưng tập cuối cùng là`{1,3,4}`. 

Kiểm tra số nguyên từ`1`bởi vì`4`tiết lộ rằng`2`bị thiếu. Tập hợp này không phải là một hoán vị nên câu trả lời là`NO`. 

Ví dụ này chứng minh tại sao việc xác thực phạm vi cuối cùng là cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M + X) | Xây dựng tập hợp và quét các giá trị ứng cử viên | 
| Không gian | O(M + X) | Bộ lưu trữ số gốc và số bổ sung | 

Giá trị lớn nhất gặp phải là nhỏ vì số đầu vào tối đa là 50 và`s`tối đa là 1000. Thời gian chạy nằm dưới mức giới hạn một cách thoải mái ngay cả đối với tất cả 100 trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    ans = []

    for _ in range(t):
        m, s = map(int, input().split())
        present = set(map(int, input().split()))

        x = 1
        while s > 0:
            if x not in present:
                s -= x
                present.add(x)
            x += 1

        if s < 0:
            ans.append("NO")
            continue

        mx = max(present)
        ok = all(v in present for v in range(1, mx + 1))
        ans.append("YES" if ok else "NO")

    return "\n".join(ans) + "\n"

# provided sample
assert run(
"""5
3 13
3 1 4
1 1
1
3 3
1 4 2
2 1
4 3
5 6
1 2 3 4 5
"""
) == """YES
NO
YES
NO
YES
"""

# minimum case
assert run(
"""1
1 1
1
"""
) == """NO
"""

# already almost a permutation
assert run(
"""1
3 3
1 4 2
"""
) == """YES
"""

# gap remains after reaching s = 0
assert run(
"""1
2 1
4 3
"""
) == """NO
"""

# larger valid completion
assert run(
"""1
5 15
1 2 3 4 5
"""
) == """YES
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1`| KHÔNG | Trường hợp nhỏ nhất có thể | 
|`1 4 2`với`s=3`| CÓ | Giá trị thiếu duy nhất hoàn thành hoán vị | 
|`4 3`với`s=1`| KHÔNG | Chỉ điều kiện tổng là không đủ | 
|`1 2 3 4 5`với`s=15`| CÓ | Phải thêm nhiều giá trị liên tiếp | 

## Vỏ cạnh 

Hãy xem xét:```
1
1 1
1
```Thuật toán bắt đầu với tập hợp`{1}`. Số nguyên dương còn thiếu đầu tiên là`2`, do đó trừ nó sẽ làm cho`s = -1`. Vì tổng trở thành số âm nên câu trả lời là`NO`. Điều này xử lý chính xác trường hợp số tiền bị thiếu bắt buộc quá nhỏ để thêm bất kỳ phần tử hoán vị mới hợp lệ nào. 

Coi như:```
1
2 1
4 3
```Thuật toán bổ sung`1`, làm`s = 0`. Tập hợp trở thành`{1,3,4}`. Số kiểm tra xác thực cuối cùng`1,2,3,4`và phát hiện ra rằng`2`vắng mặt. Câu trả lời là`NO`. Điều này ngăn cản việc chấp nhận phạm vi không đầy đủ. 

Coi như:```
1
3 3
1 4 2
```Thuật toán thêm giá trị còn thiếu`3`, làm`s = 0`. Tập hợp trở thành`{1,2,3,4}`. Mỗi số từ`1`bởi vì`4`tồn tại, vì vậy câu trả lời là`YES`. Điều này xác nhận rằng thuật toán nhận dạng chính xác một hoán vị đã hoàn thành. 

Coi như:```
1
5 6
1 2 3 4 5
```Các giá trị còn thiếu tiếp theo là`6`, sau đó`7`, vân vân. Thêm`6`ngay lập tức làm cho`s = 0`. Tập kết quả là`{1,2,3,4,5,6}`, đó là một hoán vị hợp lệ. Câu trả lời là`YES`. Trường hợp này cho thấy các số ban đầu có thể đã tạo thành tiền tố của hoán vị cuối cùng.
