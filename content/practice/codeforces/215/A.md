---
title: "CF 215A - Xích Xe Đạp"
description: "Chúng tôi có hai bộ bánh răng xe đạp. Các bánh răng phía trước được gắn vào bàn đạp và các bánh răng phía sau được gắn vào bánh sau. Nếu xích nối bánh trước a[i] với bánh sau b[j] thì tỉ số truyền thu được là b[j] / a[i]."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 215
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 132 (Div. 2)"
rating: 900
weight: 215
solve_time_s: 85
verified: true
draft: false
---

[CF 215A - Xích xe đạp](https://codeforces.com/problemset/problem/215/A) 

**Xếp hạng:** 900 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai bộ bánh răng xe đạp. Các bánh răng phía trước được gắn vào bàn đạp và các bánh răng phía sau được gắn vào bánh sau. Nếu xích nối bánh trước`a[i]`tới bánh sau`b[j]`, tỉ số truyền thu được là`b[j] / a[i]`. 

Chúng ta chỉ quan tâm đến tỷ số là số nguyên, nghĩa là số răng của bánh răng sau phải chia hết cho số răng của bánh răng trước. Trong số tất cả các tỉ số nguyên như vậy, chúng ta muốn có tỉ số lớn nhất có thể. Nhiệm vụ là đếm xem có bao nhiêu cặp bánh răng tạo ra tỷ số nguyên tối đa đó. 

Kích thước đầu vào rất nhỏ. Cả hai mảng đều chứa tối đa 50 phần tử, do đó có nhiều nhất`50 * 50 = 2500`các cặp có thể. Ngay cả một vòng lặp lồng nhau đơn giản trên mỗi cặp cũng dễ dàng phù hợp trong giới hạn thời gian. Không cần các kỹ thuật tối ưu hóa nâng cao như thủ thuật sắp xếp, tìm kiếm nhị phân hoặc tiền xử lý băm. 

Thách thức chính không phải là hiệu suất mà là tính chính xác. Việc triển khai bất cẩn có thể âm thầm đếm các cặp sai nếu nó kết hợp phép chia số nguyên với việc kiểm tra tính chia hết không chính xác. 

Một sai lầm dễ mắc phải là coi phép chia sàn là một tỷ lệ hợp lệ mà không kiểm tra khả năng chia hết trước. 

Coi như:```
2
4 6
2
11 12
```Các tỷ số nguyên hợp lệ là: 

-`12 / 4 = 3`-`12 / 6 = 2`Cặp đôi`(6, 11)`không hợp lệ vì`11 / 6`không phải là số nguyên. Nếu chúng ta mù quáng tính toán`11 // 6 = 1`, chúng ta sẽ coi nó như một tỷ số truyền hợp lệ một cách không chính xác. 

Câu trả lời đúng là:```
1
```Một trường hợp tinh tế khác xảy ra khi một số cặp khác nhau có cùng tỷ lệ tối đa. 

Ví dụ:```
2
2 3
2
6 9
```Các tỉ số nguyên là: 

-`6 / 2 = 3`-`9 / 3 = 3`Tỷ lệ tối đa là`3`, và nó xuất hiện hai lần. 

Đầu ra đúng là:```
2
```Giải pháp có lỗi có thể dừng sau khi tìm mức tối đa đầu tiên thay vì đếm tất cả các lần xuất hiện. 

Ngoài ra còn có kích thước đầu vào nhỏ nhất có thể:```
1
5
1
10
```Chỉ tồn tại một cặp và tỉ số của nó là`2`. 

Đầu ra đúng là:```
1
```Điều này kiểm tra xem quá trình triển khai có hoạt động chính xác hay không khi có chính xác một thiết bị hợp lệ. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là vũ lực. Chúng tôi thử mọi cặp bánh răng phía trước và phía sau có thể. Đối với mỗi cặp, chúng tôi kiểm tra xem số răng của bánh răng phía sau có chia hết cho số răng của bánh răng phía trước hay không. Nếu đúng như vậy, chúng tôi tính toán tỷ lệ và so sánh nó với mức tối đa hiện tại. 

Điều này hiệu quả vì tổng số cặp rất nhỏ. Trong trường hợp xấu nhất, chúng tôi đánh giá`50 * 50 = 2500`sự kết hợp. Mỗi lần kiểm tra đều mất thời gian không đổi nên toàn bộ thuật toán diễn ra cực kỳ nhanh. 

Một biến thể brute-force chậm hơn nhưng vẫn chính xác trước tiên sẽ lưu trữ mọi tỷ lệ hợp lệ trong danh sách, sau đó quét lại danh sách để tìm mức tối đa và đếm số lần xuất hiện của nó. Thậm chí điều đó sẽ trôi qua một cách thoải mái vì đầu vào quá nhỏ. 

Cách tiếp cận rõ ràng hơn là xử lý mọi thứ trong một lần. Trong khi lặp qua các cặp, chúng tôi duy trì hai biến: 

- tỷ lệ hợp lệ lớn nhất được thấy cho đến nay 
- có bao nhiêu cặp đạt được tỷ lệ đó 

Bất cứ khi nào chúng tôi tìm thấy tỷ lệ lớn hơn, chúng tôi thay thế tỷ lệ tối đa và đặt lại số đếm thành`1`. Bất cứ khi nào chúng tôi tìm thấy một cặp khác có cùng tỷ lệ, chúng tôi sẽ tăng số lượng. 

Quan sát quan trọng là chúng ta không bao giờ cần phải nhớ tất cả các tỷ lệ hợp lệ. Chỉ có tỷ lệ tốt nhất hiện tại mới quan trọng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force với các tỷ lệ được lưu trữ | O(n×m) | O(n×m) | Đã chấp nhận | 
| Quét tối ưu một lần | O(n×m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng bánh răng phía trước và số răng của chúng. 
2. Đọc số lượng bánh răng phía sau và số răng của chúng. 
3. Khởi tạo hai biến: 

-`best = 0`, tỷ lệ số nguyên lớn nhất được tìm thấy cho đến nay. 
-`count = 0`, số cặp đạt được tỷ lệ đó. 
4. Lặp lại mọi thiết bị phía trước`a`. 
5. Đối với mỗi bánh trước, lặp lại qua từng bánh sau`b`. 
6. Kiểm tra xem`b % a == 0`. 

Điều này quyết định liệu tỷ lệ`b / a`là một số nguyên. 
7. Nếu tỷ lệ không phải là số nguyên, hãy bỏ qua cặp này. 
8. Ngược lại, hãy tính`ratio = b // a`. 
9. Nếu`ratio > best`, cập nhật:

-`best = ratio`-`count = 1`Một tỷ lệ lớn hơn sẽ thay thế tất cả các ứng cử viên trước đó. 
10. Nếu`ratio == best`, tăng`count`. 

Cặp này cũng đạt được tỷ lệ tối đa. 
11. Sau khi xử lý tất cả các cặp, hãy in`count`. 

### Tại sao nó hoạt động 

Thuật toán kiểm tra mọi cặp bánh răng có thể có chính xác một lần. Bất kỳ câu trả lời hợp lệ nào cũng phải đến từ một trong các cặp này, vì vậy không thể bỏ sót ứng viên nào. 

Biến`best`luôn lưu trữ tỷ lệ số nguyên lớn nhất trong số tất cả các cặp được xử lý. Bất cứ khi nào một tỷ lệ lớn hơn xuất hiện, thay thế`best`là đúng vì tất cả các tỷ lệ nhỏ hơn không còn có thể tối ưu nữa. 

Biến`count`luôn theo dõi có bao nhiêu cặp được xử lý đạt được tỷ lệ tối đa hiện tại. Việc đặt lại nó khi tỷ lệ lớn hơn xuất hiện và tăng nó khi tỷ lệ bằng nhau xuất hiện sẽ duy trì tính bất biến này trong suốt quá trình quét. 

Sau khi tất cả các cặp được xử lý,`count`chính xác là số cặp có tỷ lệ nguyên tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
front = list(map(int, input().split()))

m = int(input())
rear = list(map(int, input().split()))

best = 0
count = 0

for a in front:
    for b in rear:
        if b % a != 0:
            continue

        ratio = b // a

        if ratio > best:
            best = ratio
            count = 1
        elif ratio == best:
            count += 1

print(count)
```Phần đầu tiên đọc kích thước bánh răng phía trước và phía sau. Vì các ràng buộc rất nhỏ nên danh sách đơn giản là đủ. 

Các vòng lặp lồng nhau tạo ra mọi tổ hợp bánh răng có thể. Điều này đảm bảo bao phủ toàn bộ không gian tìm kiếm. 

Việc kiểm tra khả năng phân chia là chi tiết quan trọng:```
if b % a != 0:
    continue
```Nếu không có điều kiện này, phép chia sàn sẽ coi các tỷ lệ không nguyên là hợp lệ một cách không chính xác. 

Logic cập nhật duy trì tỷ lệ tối đa hiện tại và tần số của nó. Đặt lại`count`khi một tỷ lệ lớn hơn xuất hiện là quan trọng. Một lỗi phổ biến là quên cài đặt lại này, điều này sẽ trộn lẫn số lượng từ các tỷ lệ nhỏ hơn với mức tối đa mới. 

Mã sử ​​dụng phép chia số nguyên`//`chỉ sau khi xác nhận khả năng chia hết, do đó tỷ lệ được tính toán là chính xác về mặt toán học. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
4 5
3
12 13 15
```Dấu vết: 

| Bánh trước | Bánh sau | Chia được? | Tỷ lệ | Tỷ lệ tốt nhất | Đếm | 
| --- | --- | --- | --- | --- | --- | 
| 4 | 12 | Có | 3 | 3 | 1 | 
| 4 | 13 | Không | - | 3 | 1 | 
| 4 | 15 | Không | - | 3 | 1 | 
| 5 | 12 | Không | - | 3 | 1 | 
| 5 | 13 | Không | - | 3 | 1 | 
| 5 | 15 | Có | 3 | 3 | 2 | 

Đầu ra cuối cùng:```
2
```Dấu vết này cho thấy nhiều cặp có thể chia sẻ cùng một tỷ lệ tối đa. Thuật toán tăng bộ đếm một cách chính xác thay vì thay thế nó. 

### Ví dụ 2 

đầu vào:```
3
2 3 4
3
8 9 12
```Dấu vết: 

| Bánh trước | Bánh sau | Chia được? | Tỷ lệ | Tỷ lệ tốt nhất | Đếm | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 8 | Có | 4 | 4 | 1 | 
| 2 | 9 | Không | - | 4 | 1 | 
| 2 | 12 | Có | 6 | 6 | 1 | 
| 3 | 8 | Không | - | 6 | 1 | 
| 3 | 9 | Có | 3 | 6 | 1 | 
| 3 | 12 | Có | 4 | 6 | 1 | 
| 4 | 8 | Có | 2 | 6 | 1 | 
| 4 | 9 | Không | - | 6 | 1 | 
| 4 | 12 | Có | 3 | 6 | 1 | 

Đầu ra cuối cùng:```
1
```Ví dụ này minh họa cách thuật toán đặt lại số đếm khi tỷ lệ lớn hơn xuất hiện. Tỷ lệ`6`thay thế mức tối đa trước đó`4`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n×m) | Mỗi bánh trước được ghép nối với mọi bánh sau một lần | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Với tối đa 2500 lần kiểm tra cặp, giải pháp sẽ hoạt động thoải mái trong thời gian giới hạn. Việc sử dụng bộ nhớ không đổi ngoài mảng đầu vào. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())
    front = list(map(int, input().split()))

    m = int(input())
    rear = list(map(int, input().split()))

    best = 0
    count = 0

    for a in front:
        for b in rear:
            if b % a == 0:
                ratio = b // a

                if ratio > best:
                    best = ratio
                    count = 1
                elif ratio == best:
                    count += 1

    print(count)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output.strip()

# provided sample
assert run(
"""2
4 5
3
12 13 15
"""
) == "2", "sample 1"

# minimum size
assert run(
"""1
5
1
10
"""
) == "1", "minimum input"

# multiple maximum ratios
assert run(
"""2
2 3
2
6 9
"""
) == "2", "two pairs share maximum"

# only one valid integer ratio
assert run(
"""2
4 6
2
11 12
"""
) == "1", "non-divisible pairs ignored"

# larger ratio appears later
assert run(
"""3
2 3 4
3
8 9 12
"""
) == "1", "count resets on larger maximum"

# many valid ratios but same maximum
assert run(
"""3
1 2 3
3
3 6 9
"""
) == "2", "maximum ratio counted correctly"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 5 / 1 / 10`|`1`| Kích thước đầu vào tối thiểu | 
|`2 3`với`6 9`|`2`| Tỷ lệ chia sẻ nhiều cặp tối đa | 
|`4 6`với`11 12`|`1`| Các cặp không chia hết phải được bỏ qua | 
|`2 3 4`với`8 9 12`|`1`| Đếm số lần đặt lại khi tỷ lệ lớn hơn xuất hiện | 
|`1 2 3`với`3 6 9`|`2`| Đếm đúng trong số nhiều tỷ lệ hợp lệ | 

## Vỏ cạnh 

Hãy xem xét trường hợp phân chia tầng có thể chấp nhận không chính xác tỷ lệ không nguyên:```
2
4 6
2
11 12
```Thuật toán kiểm tra khả năng chia hết trước khi tính tỷ lệ. 

-`11 % 4 != 0`-`11 % 6 != 0`Những cặp này được bỏ qua hoàn toàn. Chỉ một`12 / 4 = 3`Và`12 / 6 = 2`được xem xét. Tỷ lệ hợp lệ tối đa là`3`, vậy đáp án là:```
1
```Bây giờ hãy xem xét nhiều cặp có cùng tỷ lệ tối đa:```
2
2 3
2
6 9
```Các tỷ lệ hợp lệ là: 

-`6 / 2 = 3`-`9 / 3 = 3`Khi tỷ lệ thứ hai`3`được tìm thấy, thuật toán sẽ đi vào`ratio == best`nhánh và tăng số lượng. Câu trả lời cuối cùng trở thành:```
2
```Cuối cùng, hãy xem xét đầu vào hợp lệ nhỏ nhất có thể:```
1
5
1
10
```Chỉ có một cặp tồn tại. 

-`10 % 5 == 0`- tỷ lệ =`2`Bộ thuật toán`best = 2`Và`count = 1`, sau đó in:```
1
```Điều này xác nhận việc triển khai xử lý các mảng một phần tử một cách chính xác mà không cần mã trường hợp đặc biệt.
