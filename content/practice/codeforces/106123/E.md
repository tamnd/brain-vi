---
title: "CF 106123E - Dậm chân khủng long"
description: "Chúng ta có một mảng trong đó mỗi giá trị biểu thị số sao gà trên một đĩa. Trước khi bất cứ thứ gì bị phá hủy, Cole có thể chọn chính xác một vị trí và thay thế giá trị của nó bằng 0."
date: "2026-06-25T11:33:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106123
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 10-15-25 Div. 1 (Advanced)"
rating: 0
weight: 106123
solve_time_s: 47
verified: true
draft: false
---

[CF 106123E - Dậm chân khủng long](https://codeforces.com/problemset/problem/106123/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mảng trong đó mỗi giá trị biểu thị số sao gà trên một đĩa. 

Trước khi bất cứ thứ gì bị phá hủy, Cole có thể chọn chính xác một vị trí và thay thế giá trị của nó bằng`0`. Sau đó, Barney chọn một mảng con liền kề có độ dài`k`và phá hủy mọi tấm bên trong đoạn đó. Barney muốn tiêu diệt càng nhiều ngôi sao gà càng tốt, trong khi Cole muốn khiến lựa chọn tốt nhất của Barney càng yếu càng tốt. 

Nhiệm vụ là xác định số sao gà cuối cùng mà Barney tiêu diệt được khi cả hai người chơi đều hành động tối ưu. 

Kích thước mảng có thể đạt tới`10^5`, điều này ngay lập tức loại trừ bất kỳ giải pháp nào tính toán lại toàn bộ chiều dài-`k`tổng của mảng con sau mỗi lựa chọn có thể có của Cole. MỘT`O(n^2)`thuật toán sẽ thực hiện xung quanh`10^10`trong trường hợp xấu nhất, vượt xa giới hạn. 

Điều tinh tế là Cole chỉ thay đổi một vị trí, nhưng vị trí đó ảnh hưởng đến nhiều chiều dài khác nhau-`k`cửa sổ. Việc triển khai đơn giản thường cập nhật từng cửa sổ một cho từng vị trí ứng viên, việc này trở nên quá chậm. 

Xem xét đầu vào```
5 3
3 5 7 4 7
```Tổng cửa sổ có độ dài-3 là:```
[15, 16, 18]
```Nếu Cole loại bỏ giá trị`7`ở vị trí`3`, tổng cửa sổ trở thành:```
[8, 9, 11]
```và Barney phá hủy`11`. 

Một cách tiếp cận bất cẩn có thể chỉ nhìn vào cửa sổ lớn nhất hiện tại và cố gắng làm suy yếu nó. Điều đó là chưa đủ, vì sau khi làm suy yếu một cửa sổ, một cửa sổ khác có thể trở thành mức tối đa mới. 

Một trường hợp cạnh khác xuất hiện khi`k = n`.```
4 4
1 10 4 8
```Chỉ có một cửa sổ có thể. Cole nên loại bỏ giá trị lớn nhất,`10`, cho```
1 + 0 + 4 + 8 = 13
```và câu trả lời là`13`. 

Bất kỳ giải pháp nào cũng phải xử lý chính xác thực tế là một số cửa sổ bị ảnh hưởng bởi sự lựa chọn của Cole trong khi những cửa sổ khác không thay đổi. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Tính toán mọi chiều dài-`k`tổng cửa sổ. Sau đó, với mỗi vị trí`i`, giả vờ rằng`a[i]`trở thành 0, cập nhật mọi cửa sổ chứa`i`, tìm tổng cửa sổ kết quả lớn nhất và giữ câu trả lời tốt nhất trong tất cả các lựa chọn của`i`. 

Điều này đúng vì nó mô phỏng trực tiếp trò chơi. Vấn đề là chi phí. Một vị trí có thể thuộc về`O(k)`cửa sổ, và có`n`các vị trí. Trong trường hợp xấu nhất điều này trở thành`O(nk)`, đó là`O(n^2)`khi`k`là lớn. 

Quan sát quan trọng là sự thay đổi`a[i]`chỉ ảnh hưởng đến các cửa sổ chứa vị trí`i`. 

Cho phép`S[j]`là tổng của`j`-chiều dài thứ-`k`cửa sổ. 

Sau khi thiết lập`a[i] = 0`:```
windows containing i:  S[j] - a[i]
other windows:         S[j]
```Đối với một vị trí cố định`i`, lựa chọn tốt nhất của Barney là lựa chọn tối đa trong số:```
1. The largest unaffected window.
2. The largest affected window after subtracting a[i].
```Vì vậy, chúng ta cần hai giá trị một cách hiệu quả. 

Cửa sổ không bị ảnh hưởng lớn nhất có thể thu được bằng cách sử dụng các mảng tối đa tiền tố và hậu tố trên tổng cửa sổ. 

Cửa sổ bị ảnh hưởng lớn nhất chỉ đơn giản là:```
max_window_sum_in_containing_range - a[i]
```trở thành truy vấn tối đa phạm vi trên mảng tổng cửa sổ. 

Vì tổng cửa sổ không bao giờ thay đổi nên chúng ta có thể xử lý trước chúng một lần và trả lời mọi truy vấn tối đa trong phạm vi`O(1)`sử dụng một bảng thưa thớt. 

Điều này làm giảm toàn bộ vấn đề xuống`O(n log n)`tiền xử lý và`O(n)`sự đánh giá. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nk) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán tất cả chiều dài-`k`tính tổng bằng cửa sổ trượt. 

Gọi số lượng cửa sổ là`W = n - k + 1`. 
2. Xây dựng một mảng tiền tố tối đa trên tổng cửa sổ.`pref[j]`lưu trữ tổng cửa sổ lớn nhất trong số các cửa sổ`1..j`. 
3. Xây dựng một mảng hậu tố tối đa trên các tổng cửa sổ.`suff[j]`lưu trữ tổng cửa sổ lớn nhất trong số các cửa sổ`j..W`. 
4. Xây dựng một bảng thưa thớt cho các truy vấn tối đa phạm vi trên tổng cửa sổ. 

Điều này cho phép chúng ta thu được tổng cửa sổ tối đa bên trong bất kỳ khoảng thời gian nào trong`O(1)`thời gian. 
5. Đối với mọi vị trí`i`, xác định cửa sổ nào chứa nó. 

Một cửa sổ bắt đầu từ`j`chứa vị trí`i`chính xác khi nào:```
j ≤ i ≤ j + k - 1
```Kể từ đây:```
L = max(1, i - k + 1)
R = min(i, W)
```6. Tính cửa sổ lớn nhất không bị ảnh hưởng. 

Tất cả các cửa sổ không bị ảnh hưởng đều nằm bên ngoài`[L, R]`. 

Sử dụng tiền tố và hậu tố cực đại:```
outside = max(
    pref[L - 1] if L > 1,
    suff[R + 1] if R < W
)
```7. Tính cửa sổ bị ảnh hưởng lớn nhất. 

Truy vấn tổng cửa sổ tối đa bên trong`[L, R]`và trừ`a[i]`.```
inside = max_window_in_[L,R] - a[i]
```8. Sự hủy diệt tốt nhất có thể đạt được của Barney sau khi Cole chọn`i`là```
max(outside, inside)
```9. Lấy giá trị tối thiểu trên tất cả các vị trí`i`. 

### Tại sao nó hoạt động 

Đối với một vị trí cố định`i`, mọi cửa sổ đều thuộc về đúng một trong hai nhóm. 

Windows chứa`i`thua chính xác`a[i]`từ tổng của chúng. Windows không chứa`i`vẫn không thay đổi. 

Barney chọn cửa sổ kết quả lớn nhất, vì vậy câu trả lời cho vị trí đó là giá trị tối đa giữa cửa sổ không bị ảnh hưởng tốt nhất và cửa sổ bị ảnh hưởng tốt nhất. Thuật toán tính toán chính xác cả hai đại lượng. 

Vì mọi vị trí có thể`i`được kiểm tra và Cole chọn vị trí tạo ra giá trị nhỏ nhất như vậy, câu trả lời cuối cùng chính xác là giá trị minimax của trò chơi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))

w = n - k + 1

# window sums
S = [0] * w
cur = sum(a[:k])
S[0] = cur

for i in range(k, n):
    cur += a[i] - a[i - k]
    S[i - k + 1] = cur

# prefix max
pref = [0] * w
pref[0] = S[0]
for i in range(1, w):
    pref[i] = max(pref[i - 1], S[i])

# suffix max
suff = [0] * w
suff[-1] = S[-1]
for i in range(w - 2, -1, -1):
    suff[i] = max(suff[i + 1], S[i])

# sparse table for RMQ max
lg = [0] * (w + 1)
for i in range(2, w + 1):
    lg[i] = lg[i // 2] + 1

K = lg[w] + 1
st = [S[:]]

j = 1
while (1 << j) <= w:
    prev = st[j - 1]
    length = w - (1 << j) + 1
    row = [0] * length
    half = 1 << (j - 1)

    for i in range(length):
        row[i] = max(prev[i], prev[i + half])

    st.append(row)
    j += 1

def range_max(l, r):
    length = r - l + 1
    p = lg[length]
    return max(st[p][l], st[p][r - (1 << p) + 1])

INF = 10**30
answer = INF

for idx in range(n):
    L = max(0, idx - k + 1)
    R = min(idx, w - 1)

    outside = 0

    if L > 0:
        outside = max(outside, pref[L - 1])

    if R + 1 < w:
        outside = max(outside, suff[R + 1])

    inside = range_max(L, R) - a[idx]

    answer = min(answer, max(outside, inside))

print(answer)
```Phần đầu tiên tính toán tất cả chiều dài-`k`tính tổng cửa sổ theo thời gian tuyến tính bằng cách sử dụng cửa sổ trượt tiêu chuẩn. 

Mảng tiền tố và hậu tố cho phép chúng ta thu được cửa sổ lớn nhất bên ngoài bất kỳ khoảng thời gian nào mà không cần quét lại mảng. 

Bảng thưa thớt lưu trữ phạm vi cực đại của mảng tổng cửa sổ. Vì mảng không bao giờ thay đổi nên bảng thưa là lý tưởng vì nó cung cấp`O(1)`truy vấn tối đa sau khi tiền xử lý. 

Một chi tiết triển khai dễ bị bỏ sót là việc chuyển đổi giữa các vị trí và chỉ mục cửa sổ. Mã sử ​​dụng lập chỉ mục dựa trên 0, do đó khoảng thời gian chứa cửa sổ trở thành:```
L = max(0, idx - k + 1)
R = min(idx, w - 1)
```Một điểm tinh tế khác là khi mọi cửa sổ đều chứa vị trí đã chọn. Trong trường hợp đó không có cửa sổ nào không bị ảnh hưởng. Biến`outside`còn lại`0`, điều này đúng vì tất cả các giá trị mảng đều dương và mọi tổng cửa sổ thực đều không âm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3
3 5 7 4 7
```Tổng cửa sổ:```
[15, 16, 18]
```| Vị trí | Giá trị đã bị xóa | Chứa Windows | Bên trong lớn nhất trước khi loại bỏ | Bên Trong Sau Khi Loại Bỏ | Bên ngoài Max | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 3 | [15] | 15 | 12 | 18 | 18 | 
| 2 | 5 | [15,16] | 16 | 11 | 18 | 18 | 
| 3 | 7 | [15,16,18] | 18 | 11 | 0 | 11 | 
| 4 | 4 | [16,18] | 18 | 14 | 15 | 15 | 
| 5 | 7 | [18] | 18 | 11 | 16 | 16 | 

Kết quả tối thiểu là`11`. 

Ví dụ này cho thấy tình huống trong đó việc xóa một giá trị khỏi một vị trí được chia sẻ bởi nhiều cửa sổ sẽ hiệu quả hơn nhiều so với việc xóa nó khỏi một vị trí ranh giới. 

### Ví dụ 2 

đầu vào:```
4 4
1 10 4 8
```Tổng cửa sổ:```
[23]
```| Vị trí | Giá trị đã bị xóa | Bên Trong Sau Khi Loại Bỏ | Bên ngoài Max | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 22 | 0 | 22 | 
| 2 | 10 | 13 | 0 | 13 | 
| 3 | 4 | 19 | 0 | 19 | 
| 4 | 8 | 15 | 0 | 15 | 

Kết quả tối thiểu là`13`. 

Điều này thể hiện trường hợp đặc biệt chỉ tồn tại một cửa sổ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Tổng cửa sổ, mảng tiền tố/hậu tố và xây dựng bảng thưa thớt | 
| Không gian | O(n log n) | Lưu trữ bảng thưa thớt | 

Với`n ≤ 100000`, MỘT`O(n log n)`giải pháp thoải mái phù hợp trong giới hạn lập trình cạnh tranh điển hình. Các truy vấn tối đa trong phạm vi trở thành thời gian không đổi sau khi xử lý trước, cho phép mọi vị trí được đánh giá một cách hiệu quả. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    w = n - k + 1

    S = [0] * w
    cur = sum(a[:k])
    S[0] = cur

    for i in range(k, n):
        cur += a[i] - a[i - k]
        S[i - k + 1] = cur

    pref = [0] * w
    pref[0] = S[0]
    for i in range(1, w):
        pref[i] = max(pref[i - 1], S[i])

    suff = [0] * w
    suff[-1] = S[-1]
    for i in range(w - 2, -1, -1):
        suff[i] = max(suff[i + 1], S[i])

    lg = [0] * (w + 1)
    for i in range(2, w + 1):
        lg[i] = lg[i // 2] + 1

    K = lg[w] + 1
    st = [S[:]]

    j = 1
    while (1 << j) <= w:
        prev = st[j - 1]
        length = w - (1 << j) + 1
        row = [0] * length
        half = 1 << (j - 1)

        for i in range(length):
            row[i] = max(prev[i], prev[i + half])

        st.append(row)
        j += 1

    def rmq(l, r):
        length = r - l + 1
        p = lg[length]
        return max(st[p][l], st[p][r - (1 << p) + 1])

    ans = 10**30

    for idx in range(n):
        L = max(0, idx - k + 1)
        R = min(idx, w - 1)

        outside = 0

        if L > 0:
            outside = max(outside, pref[L - 1])

        if R + 1 < w:
            outside = max(outside, suff[R + 1])

        inside = rmq(L, R) - a[idx]

        ans = min(ans, max(outside, inside))

    return str(ans) + "\n"

# provided samples
assert run("5 3\n3 5 7 4 7\n") == "11\n", "sample 1"
assert run("4 4\n1 10 4 8\n") == "13\n", "sample 2"

# custom cases
assert run("1 1\n5\n") == "0\n", "single element"
assert run("5 1\n4 4 4 4 4\n") == "4\n", "k equals 1"
assert run("5 5\n7 7 7 7 7\n") == "28\n", "single window all equal"
assert run("3 2\n1 100 1\n") == "1\n", "boundary overlap"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 5`|`0`| Kích thước tối thiểu có thể | 
|`5 1 / 4 4 4 4 4`|`4`| Mọi cửa sổ đều có chiều dài 1 | 
|`5 5 / 7 7 7 7 7`|`28`| Cửa sổ đơn bao phủ toàn bộ mảng | 
|`3 2 / 1 100 1`|`1`| Vị trí được chia sẻ bởi nhiều cửa sổ | 

## Vỏ cạnh 

Hãy xem xét:```
1 1
5
```Chỉ có một tấm và một cửa sổ. Cole đặt tấm đó về 0. Tổng cửa sổ duy nhất trở thành 0, vì vậy Barney phá hủy 0 ngôi sao gà. Thuật toán tính tổng một cửa sổ bằng`5`, sau đó đánh giá vị trí duy nhất và thu được`5 - 5 = 0`. 

Coi như:```
4 4
1 10 4 8
```Mỗi cửa sổ chứa mọi vị trí vì chỉ có một cửa sổ. Đối với vị trí`2`, mức tối đa của cửa sổ bị ảnh hưởng là`23 - 10 = 13`và không có cửa sổ nào không bị ảnh hưởng. Thuật toán rời đi một cách chính xác`outside = 0`và trả về`13`. 

Coi như:```
3 2
1 100 1
```Tổng cửa sổ là`[101, 101]`. Việc loại bỏ giá trị ở giữa sẽ ảnh hưởng đồng thời đến cả hai cửa sổ, tạo ra`[1, 1]`. Thuật toán xác định rằng khoảng thời gian chứa cửa sổ bao trùm tất cả các cửa sổ và tính toán:```
inside = 101 - 100 = 1
outside = 0
```đưa ra câu trả lời đúng`1`. 

Những trường hợp này xác minh rằng tính toán khoảng thời gian cho các cửa sổ bị ảnh hưởng, việc xử lý các cửa sổ không bị ảnh hưởng bị thiếu và logic minimax đều hoạt động chính xác.
