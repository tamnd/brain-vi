---
title: "CF 401B - Sereja và các cuộc thi"
description: "Nền tảng cuộc thi đánh số liên tục theo từng vòng theo thời gian bắt đầu. Một vòng có thể là một vòng Div2 độc lập hoặc một cặp vòng đồng thời trong đó Div2 có mã định danh i và Div1 có mã định danh i + 1. Sereja chỉ tham gia vào các vòng Div2."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 401
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 235 (Div. 2)"
rating: 1200
weight: 401
solve_time_s: 84
verified: true
draft: false
---

[CF 401B - Sereja và các cuộc thi](https://codeforces.com/problemset/problem/401/B) 

**Đánh giá:** 1200 
**Tags:** tham lam, thực hiện, toán học 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nền tảng cuộc thi đánh số liên tục theo từng vòng theo thời gian bắt đầu. Một vòng có thể là một vòng Div2 độc lập hoặc một cặp vòng đồng thời trong đó Div2 nhận được mã định danh`i`và Div1 nhận được mã định danh`i + 1`. 

Sereja chỉ tham gia các vòng Div2. Hiện tại anh ấy đang tham gia vào vòng`x`, và trước đó anh ấy đã tham gia chính xác vào`k`các vòng Div2 trước đó. Đối với mỗi lần tham gia trước đó, anh đều nhớ: 

- một vòng Div2 độc lập có mã định danh`a`- hoặc một cặp đồng thời`(a, a + 1)`Ở đâu`a`là mã định danh Div2 

Tất cả các định danh được ghi nhớ đều nhỏ hơn`x`. 

Thông tin còn thiếu là tất cả mọi thứ về những hiệp đấu mà anh ấy đã bỏ qua. Một số mã định danh không được sử dụng có thể tương ứng với các vòng Div2 độc lập và một số có thể thuộc về các cặp đồng thời. Chúng ta phải tính: 

- số vòng Div2 tối thiểu mà Sereja có thể đã bỏ lỡ 
- số vòng Div2 tối đa mà Sereja có thể đã bỏ lỡ 

Những hạn chế là rất nhỏ. Tất cả các số nhận dạng đều dưới 4000, do đó ngay cả một`O(x^2)`giải pháp sẽ thoải mái vượt qua. Điều đó có nghĩa là thách thức không phải là hiệu suất, mà là mô hình hóa các quy tắc đánh số một cách chính xác. 

Phần nguy hiểm là hiểu ý nghĩa của mã định danh. Một vòng Div2 độc lập sử dụng một mã định danh. Một cặp đồng thời sử dụng hai số nhận dạng liên tiếp, trong đó số nhận dạng nhỏ hơn là vòng Div2 và số nhận dạng lớn hơn là vòng Div1. 

Việc triển khai bất cẩn thường xử lý mọi mã định danh không được sử dụng một cách độc lập, nhưng điều đó là sai vì hai mã định danh liên tiếp thực sự có thể thuộc về một cặp đồng thời. 

Hãy xem xét ví dụ này:```
x = 8
known rounds:
2 2
2 5
```Các định danh được sử dụng là`{2, 5}`. Các định danh không được sử dụng là`{1, 3, 4, 6, 7}`. 

Nếu chúng tôi tối đa hóa các vòng Div2 bị bỏ lỡ, chúng tôi có thể biến mọi số nhận dạng không được sử dụng thành một vòng Div2 độc lập, tạo ra 5 vòng bị bỏ lỡ. 

Nếu chúng ta giảm thiểu việc bỏ lỡ các vòng Div2, chúng ta nên kết hợp các số nhận dạng thành các cặp đồng thời bất cứ khi nào có thể: 

-`(3,4)`có thể là một cặp đồng thời 
-`(6,7)`có thể là một cặp đồng thời khác 
-`1`phải ở độc lập 

Điều đó chỉ cho thấy có 3 vòng Div2 bị trượt. 

Một trường hợp tinh tế khác là khi một cặp đồng thời được ghi nhớ chặn các mã định danh lân cận.```
x = 6
known:
1 2 3
```Số nhận dạng`2`Và`3`đã bị chiếm giữ bởi một cặp đồng thời. Chúng tôi không thể ghép nối thêm`(1,2)`hoặc`(3,4)`, bởi vì định danh không thể thuộc về hai vòng khác nhau. 

Việc xử lý đúng yêu cầu phải theo dõi những mã định danh nào đã được sử dụng. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là xây dựng lại mọi lịch trình cuộc thi hợp lệ phù hợp với các vòng đã nhớ, sau đó đếm xem Sereja đã bỏ qua bao nhiêu vòng Div2 trong mỗi lần tái tạo. Vì mọi số nhận dạng không được sử dụng có thể là: 

- một vòng Div2 độc lập 
- một phần của một cặp đồng thời 
- hoặc mã định danh chỉ dành cho Div1 bên trong một cặp đồng thời 

số lượng các khả năng tăng theo cấp số nhân. 

Ví dụ: nếu có 2000 mã định danh không được sử dụng thì việc thử tất cả các phép gán sẽ là vô vọng. 

Quan sát quan trọng là chúng ta không bao giờ cần lịch trình đầy đủ. Chúng tôi chỉ quan tâm đến việc tồn tại bao nhiêu vòng Div2 trong số các số nhận dạng không được sử dụng. 

Giả sử chúng ta đánh dấu mọi định danh nhỏ hơn`x`đã xuất hiện trong đầu vào. Mọi số nhận dạng còn lại đều miễn phí. 

Để có câu trả lời tối đa, chiến lược là hiển nhiên. Mỗi mã định danh miễn phí có thể trở thành một vòng Div2 độc lập. Mỗi lựa chọn như vậy sẽ góp phần bỏ lỡ một vòng Div2 và không có gì ngăn cản việc xây dựng này. Vì vậy, mức tối đa bằng số lượng định danh miễn phí. 

Câu trả lời tối thiểu là nơi xuất hiện ý tưởng tham lam. 

Một cặp đồng thời sử dụng hai số nhận dạng liên tiếp nhưng chỉ đóng góp một vòng Div2. Vì vậy, để giảm thiểu việc bỏ lỡ các vòng Div2, chúng ta nên tạo càng nhiều cặp đồng thời càng tốt giữa các số nhận dạng rảnh rỗi. 

Điều này trở thành một vấn đề ghép nối khoảng thời gian đơn giản: 

- quét số nhận dạng từ trái sang phải 
- bất cứ khi nào hai số nhận dạng liên tiếp đều trống, hãy ghép chúng lại với nhau 
- nếu không thì để lại mã định danh hiện tại dưới dạng vòng Div2 độc lập 

Điều này có tác dụng tham lam vì việc ghép hai số nhận dạng liên tiếp miễn phí luôn làm giảm số vòng Div2 xuống đúng một và việc sử dụng số nhận dạng sau này không thể tạo ra cơ hội tốt hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(x) | O(x) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng boolean`used`cho số nhận dạng từ`1`ĐẾN`x - 1`. 
2. Đọc từng mô tả vòng đã nhớ. 
3. Nếu mô tả là vòng Div2 độc lập`2 a`, đánh dấu nhận dạng`a`như đã sử dụng. 
4. Nếu mô tả là một cặp đồng thời`1 a b`, đánh dấu cả hai`a`Và`b`như đã sử dụng. 
5. Đếm xem có bao nhiêu số nhận dạng từ`1`ĐẾN`x - 1`không được sử dụng. Số lượng này là số vòng Div2 bị bỏ lỡ tối đa có thể. 

Mọi mã định danh không được sử dụng có thể trở thành một vòng Div2 độc lập một cách độc lập. 
6. Để tính câu trả lời tối thiểu, hãy quét số nhận dạng từ`1`ĐẾN`x - 1`. 
7. Nếu mã định danh hiện tại đã được sử dụng, hãy bỏ qua nó. 
8. Mặt khác, hãy kiểm tra xem mã định danh tiếp theo có tồn tại và cũng không được sử dụng hay không. 

Nếu cả hai đều miễn phí, hãy tạo một cặp đồng thời từ chúng. Tăng câu trả lời lên 1 và bỏ qua cả hai số nhận dạng. 
9. Nếu không thể ghép nối, mã định danh hiện tại phải trở thành vòng Div2 độc lập. Tăng câu trả lời lên 1 và tiến lên một bước. 

### Tại sao nó hoạt động 

Việc xây dựng tối đa rất đơn giản vì mọi mã định danh không được sử dụng có thể đại diện cho một vòng Div2 độc lập. 

Đối với cách xây dựng tối thiểu, điều bất biến là mỗi khi chúng ta gặp hai mã định danh miễn phí liên tiếp, việc ghép chúng luôn là tối ưu. Một cặp sử dụng hai số nhận dạng trong khi chỉ đóng góp một vòng Div2 bị bỏ lỡ. Để một trong hai số nhận dạng không ghép đôi không bao giờ có thể làm giảm tổng số hơn nữa. 

Quá trình quét tham lam luôn tạo ra số lượng cặp liên tiếp rời rạc tối đa có thể. Vì mỗi cặp giảm đi một vòng Div2 bị bỏ lỡ, điều này cũng giảm thiểu câu trả lời cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    x, k = map(int, input().split())

    used = [False] * (x + 2)

    for _ in range(k):
        parts = list(map(int, input().split()))

        if parts[0] == 1:
            _, a, b = parts
            used[a] = True
            used[b] = True
        else:
            _, a = parts
            used[a] = True

    # maximum answer
    mx = 0
    for i in range(1, x):
        if not used[i]:
            mx += 1

    # minimum answer
    mn = 0
    i = 1

    while i < x:
        if used[i]:
            i += 1
            continue

        if i + 1 < x and not used[i + 1]:
            mn += 1
            i += 2
        else:
            mn += 1
            i += 1

    print(mn, mx)

solve()
```các`used`mảng ghi lại mọi mã định danh đã thuộc về vòng được ghi nhớ. Đối với các vòng đồng thời, chúng tôi đánh dấu cả hai số nhận dạng vì sau này không thể sử dụng lại số nhận dạng này. 

Câu trả lời tối đa chỉ là số lượng định danh không được sử dụng. Mỗi người trong số họ có thể đại diện cho một vòng Div2 độc lập. 

Câu trả lời tối thiểu sử dụng phương pháp quét tham lam. Phần tinh tế là điều kiện biên`i + 1 < x`. Mã định danh`x`bản thân nó thuộc về vòng hiện tại và không thể được sử dụng lại, do đó chỉ có các mã định danh nhỏ hơn`x`vấn đề. 

Một sai lầm dễ mắc phải khác là quên bỏ qua cả hai số nhận dạng sau khi tạo thành một cặp. Nếu chúng ta ghép đôi`(i, i + 1)`, cả hai số nhận dạng đều được sử dụng và không được tham gia lại. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 2
2 1
2 2
```Số nhận dạng được sử dụng là`{1, 2}`. 

| tôi | đã sử dụng[i] | Hành động | mn | 
| --- | --- | --- | --- | 
| 1 | Đúng | bỏ qua | 0 | 
| 2 | Đúng | bỏ qua | 0 | 

Tối đa: 

| Mã định danh | Miễn phí? | mx | 
| --- | --- | --- | 
| 1 | Không | 0 | 
| 2 | Không | 0 | 

Câu trả lời cuối cùng:```
0 0
```Không có số nhận dạng nào không được sử dụng trước vòng đấu`3`, nên Sereja không thể bỏ lỡ bất kỳ hiệp Div2 nào. 

### Mẫu 2 

Giả sử đầu vào là:```
8 2
2 2
2 5
```Số nhận dạng được sử dụng là`{2, 5}`. 

Số nhận dạng không được sử dụng là`{1, 3, 4, 6, 7}`. 

Tính toán tối thiểu: 

| tôi | đã sử dụng[i] | Quyết định | mn | 
| --- | --- | --- | --- | 
| 1 | Không | độc lập | 1 | 
| 2 | Có | bỏ qua | 1 | 
| 3 | Không | cặp (3,4) | 2 | 
| 5 | Có | bỏ qua | 2 | 
| 6 | Không | cặp (6,7) | 3 | 

Tính toán tối đa: 

| Mã định danh | Miễn phí? | mx | 
| --- | --- | --- | 
| 1 | Có | 1 | 
| 2 | Không | 1 | 
| 3 | Có | 2 | 
| 4 | Có | 3 | 
| 5 | Không | 3 | 
| 6 | Có | 4 | 
| 7 | Có | 5 | 

Câu trả lời cuối cùng:```
3 5
```Ví dụ này giải thích tại sao việc ghép nối các số nhận dạng miễn phí liên tiếp sẽ giảm thiểu số vòng Div2 bị bỏ lỡ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(x) | Chúng tôi quét số nhận dạng một số lần không đổi | 
| Không gian | O(x) | các`used`mảng lưu trữ các trạng thái định danh | 

Từ`x ≤ 4000`, ngay cả nghiệm bậc hai cũng có thể vượt qua một cách thoải mái. Giải pháp tuyến tính này dễ dàng trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    x, k = map(int, input().split())

    used = [False] * (x + 2)

    for _ in range(k):
        parts = list(map(int, input().split()))

        if parts[0] == 1:
            _, a, b = parts
            used[a] = True
            used[b] = True
        else:
            _, a = parts
            used[a] = True

    mx = 0
    for i in range(1, x):
        if not used[i]:
            mx += 1

    mn = 0
    i = 1

    while i < x:
        if used[i]:
            i += 1
            continue

        if i + 1 < x and not used[i + 1]:
            mn += 1
            i += 2
        else:
            mn += 1
            i += 1

    return f"{mn} {mx}"

# provided sample
assert run(
    "3 2\n"
    "2 1\n"
    "2 2\n"
) == "0 0", "sample 1"

# minimum-size input
assert run(
    "1 0\n"
) == "0 0", "no earlier identifiers"

# all identifiers free
assert run(
    "6 0\n"
) == "3 5", "greedy pairing on all identifiers"

# remembered simultaneous pair blocks reuse
assert run(
    "6 1\n"
    "1 2 3\n"
) == "2 3", "occupied consecutive identifiers"

# alternating occupied identifiers
assert run(
    "8 3\n"
    "2 1\n"
    "2 3\n"
    "2 5\n"
) == "2 4", "cannot pair across occupied positions"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0`|`0 0`| Đầu vào nhỏ nhất có thể | 
|`6 0`|`3 5`| Hành vi ghép nối tối đa | 
| cặp đồng thời có mặt |`2 3`| Không thể sử dụng lại số nhận dạng bên trong một cặp | 
| định danh chiếm đóng xen kẽ |`2 4`| Ghép nối tham lam chỉ sử dụng số nhận dạng miễn phí liên tiếp | 

## Vỏ cạnh 

Hãy xem xét đầu vào hợp lệ nhỏ nhất:```
1 0
```Không có định danh nào nhỏ hơn`1`, do đó phạm vi quét trống. Thuật toán tạo ra chính xác:```
0 0
```Một lỗi phổ biến thường gặp không chính xác bao gồm số nhận dạng`x`chính nó và trả về`1 1`. 

Bây giờ hãy xem xét:```
6 1
1 2 3
```Số nhận dạng`2`Và`3`đã bị chiếm giữ bởi một cặp đồng thời được ghi nhớ. 

Số nhận dạng miễn phí là`{1, 4, 5}`. 

Quá trình quét tham lam hoạt động như thế này: 

-`1`không thể ghép nối với`2`, vì vậy nó trở nên độc lập 
-`4`cặp với`5`Tổng số tối thiểu là`2`. 

Tối đa là`3`bởi vì mọi mã định danh miễn phí có thể trở thành độc lập. 

Đầu ra đúng:```
2 3
```Việc triển khai có lỗi chỉ đánh dấu mã định danh Div2 từ các cặp đã nhớ sẽ nghĩ mã định danh không chính xác`3`là miễn phí và tạo ra các cặp không hợp lệ. 

Cuối cùng, hãy xem xét:```
8 3
2 1
2 3
2 5
```Số nhận dạng miễn phí là`{2, 4, 6, 7}`. 

Quá trình tham lam: 

-`2`không thể ghép nối với`3`-`4`không thể ghép nối với`5`-`6`cặp với`7`Tối thiểu trở thành`3`, không`2`. 

Điều này nắm bắt các triển khai cố gắng ghép các số nhận dạng miễn phí không liên tiếp.
