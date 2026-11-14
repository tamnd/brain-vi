---
title: "CF 1783E - Trò chơi của năm"
description: "Đối với mỗi tên trùm, Monocarp sẽ giết nó trong lần thử cá nhân thứ [i]-th của mình, trong khi Polycarp sẽ giết nó trong lần thử cá nhân thứ b[i]-th của mình. Cuộc chiến được chia thành các khối có kích thước k. Monocarp thực hiện k lần thử, sau đó Polycarp thực hiện k lần thử, sau đó lại thực hiện Monocarp, v.v."
date: "2026-06-09T11:08:07+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1783
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 141 (Rated for Div. 2)"
rating: 2300
weight: 1783
solve_time_s: 126
verified: false
draft: false
---

[CF 1783E - Trò chơi của năm](https://codeforces.com/problemset/problem/1783/E) 

**Đánh giá:** 2300 
**Tags:** Brute Force, cấu trúc dữ liệu, toán học, lý thuyết số 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Đối với mỗi tên trùm, Monocarp sẽ giết nó theo ý mình`a[i]`- nỗ lực cá nhân, trong khi Polycarp sẽ giết nó bằng tay của mình`b[i]`-th nỗ lực cá nhân. 

Cuộc chiến được chia thành các khối có kích thước`k`. Monocarp biểu diễn`k`cố gắng, sau đó Polycarp thực hiện`k`thử, rồi lại Monocarp, v.v. Ngay sau khi một người chơi đạt đến số lần thử giết trùm, trùm sẽ chết và cả hai bộ đếm sẽ được đặt lại cho trùm tiếp theo. 

Chúng ta phải tìm mọi giá trị của`k`giữa`1`Và`n`đến nỗi Monocarp giết chết mọi tên trùm. 

Ràng buộc quan trọng là tổng số tiền`n`trên tất cả các trường hợp thử nghiệm, nhiều nhất là`2·10^5`. Điều này ngay lập tức loại trừ việc kiểm tra từng ông chủ`k`với một mô phỏng. Thậm chí một`O(n²)`giải pháp sẽ thực hiện xung quanh`4·10^10`hoạt động trong trường hợp xấu nhất. 

Thử thách ở đây là chuyển quá trình trò chơi thành một điều kiện số học thuần túy trên`a[i]`,`b[i]`, Và`k`. 

Một vài trường hợp đặc biệt rất dễ hiểu lầm. 

Hãy xem xét một ông chủ với`a=4`,`b=3`. 

Vì`k=2`, thứ tự thử là:```
M: 1 2
P: 1 2
M: 3 4
```Monocarp đạt đến lần thử thứ 4 trước khi Polycarp đạt đến lần thử thứ 3, vì vậy Monocarp thắng. 

Vì`k=1`, thứ tự là:```
M:1
P:1
M:2
P:2
M:3
P:3
```Polycarp đạt được nỗ lực thứ 3 đầu tiên và giành chiến thắng. 

Một sự so sánh ngây thơ chẳng hạn như kiểm tra xem liệu`a < b`sẽ hoàn toàn sai. 

Một trường hợp tế nhị khác là`a=b`. 

Ví dụ:```
a = 5
b = 5
```Bất kể`k`, Monocarp luôn được đi nước đầu tiên trong mỗi hiệp đấu. Khi cả hai người chơi tiêu diệt cùng một số lần thử cá nhân, Monocarp sẽ đạt được lần thử đó trước và giành chiến thắng. Bất kỳ giải pháp nào coi sự bình đẳng là một sự mất mát sẽ thất bại. 

Trường hợp cạnh cuối cùng xảy ra khi`b < a`. 

Ví dụ:```
a = 5
b = 2
```Monocarp vẫn có thể thắng với số tiền đủ lớn`k`. 

Vì`k=5`, Monocarp thực hiện các nỗ lực`1..5`trước khi Polycarp thực hiện bất kỳ động thái nào, vì vậy Monocarp sẽ giết ngay lập tức. 

Câu trả lời không chỉ được xác định bởi liệu`a`lớn hơn`b`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là kiểm tra mọi khả năng`k`. 

Đối với một ông chủ cố định và một ông chủ cố định`k`, chúng ta có thể xác định ai giết người trước bằng cách theo dõi các khối có kích thước xen kẽ`k`. Lặp lại điều này cho tất cả các ông chủ và tất cả các giá trị của`k`đưa ra một giải pháp đúng. 

Thật không may là có`n`các giá trị có thể có của`k`Và`n`các ông chủ. Ngay cả khi chúng ta có thể đánh giá một ông chủ trong thời gian không đổi, chúng ta vẫn cần`O(n²)`công việc, nó quá lớn khi`n`đạt tới`2·10^5`. 

Quan sát quan trọng là chúng ta không thực sự quan tâm ai là người chiến thắng cho một ông chủ cụ thể và`k`bằng mô phỏng. Chúng ta chỉ cần mô tả tập hợp các giá trị xấu của`k`. 

Giả sử một ông chủ có thông số`(a,b)`. 

Nếu như`a ≤ b`, Monocarp luôn thắng. Anh ấy đạt tới`a`-lần thử không muộn hơn khi Polycarp đến được chỗ của mình.`b`-lần thử thứ, và mối quan hệ nghiêng về Monocarp vì anh ta di chuyển trước. 

Trường hợp thú vị là`b < a`. 

Cho phép```
ceil(a / k)
```là số khối Monocarp cần thiết để đạt được nỗ lực`a`, và tương tự```
ceil(b / k)
```cho Polycarp. 

Nếu hai giá trị này khác nhau thì ai cần ít khối hơn sẽ đạt được nỗ lực tiêu diệt trước. 

Từ`b < a`, chúng tôi luôn có```
ceil(b/k) ≤ ceil(a/k)
```Cách duy nhất Monocarp vẫn có thể thắng là khi số lượng cả hai đều bằng nhau. Sau đó, họ đạt được nỗ lực tiêu diệt trong cùng một cặp khối và khối của Monocarp xuất hiện trước. 

Vì vậy, đối với một ông chủ với`b < a`, Monocarp thắng chính xác khi$$\left\lceil \frac{a}{k} \right\rceil = \left\lceil \frac{b}{k} \right\rceil.$$Bây giờ đến sự đơn giản hóa quan trọng. 

Đối với một số nguyên`x`, giá trị`ceil(x/k)`chỉ thay đổi khi`k`vượt qua các ước số nhất định. Thay vì tìm kiếm một cách rõ ràng tất cả những điều tốt đẹp`k`, chúng tôi đánh dấu tất cả các khoảng xấu. 

Quan sát điều đó$$\left\lceil \frac{a}{k} \right\rceil > \left\lceil \frac{b}{k} \right\rceil$$chính xác khi tồn tại bội số của`k`nằm trong khoảng`[b, a-1]`. 

Thật vậy, nếu một số bội số`m = t·k`thỏa mãn$$b \le m < a,$$sau đó`b`Và`a`thuộc các khối trần khác nhau nên có giá trị trần khác nhau. 

Tương đương,`k`là xấu nếu một số bội số của`k`nằm ở`[b,a-1]`. 

Thay vì lặp đi lặp lại`k`, chúng tôi xử lý mọi khoảng thời gian xấu`[b,a)`sử dụng một mảng khác biệt. Điều này dẫn đến một sự rất nhỏ gọn`O(n log n)`giải pháp phong cách, được thực hiện như`O(n log n)`tổng điều hòa qua bội số. Giải pháp chính thức thể hiện nó thậm chí còn tao nhã hơn thông qua việc đếm khoảng thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) hoặc tệ hơn | O(1) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Suy ra điều kiện 

Đối với một ông chủ với`b < a`, Monocarp thua bất cứ khi nào tồn tại bội số của`k`bên trong:$$[b,\;a)$$bởi vì sau đó$$\left\lceil \frac{a}{k}\right\rceil > \left\lceil \frac{b}{k}\right\rceil.$$Vì vậy mỗi ông chủ như vậy cấm tất cả các giá trị của`k`có ít nhất một bội số trong khoảng đó. 

### Tái cấu trúc mảng khác biệt 

hãy để`dx[x]`đếm xem có bao nhiêu khoảng thời gian`[b,a)`chứa điểm`x`. 

Đối với mỗi ông chủ với`b < a`:```
dx[b] += 1
dx[a] -= 1
```Sau khi lấy tổng tiền tố,```
dx[x]
```bằng số khoảng xấu bao gồm điểm nguyên`x`. 

### Kiểm tra giá trị của k 

Một giá trị`k`hợp lệ nếu không có khoảng cấm nào chứa bất kỳ bội số nào của`k`. 

bội số của`k`là:```
k, 2k, 3k, ...
```Nếu có bội số`mk`nằm trong khoảng cấm thì`dx[mk] > 0`. 

Kể từ đây:```
k is valid
⇔ dx[mk] = 0 for every multiple mk of k
```### Thuật toán 

1. Tạo mảng khác biệt`dx`kích thước`n+1`. 
2. Đối với mỗi sếp: 

Nếu`b[i] < a[i]`, thêm khoảng`[b[i], a[i])`vào mảng khác biệt. 
3. Tính tổng tiền tố sao cho`dx[x]`trở thành số khoảng bao phủ`x`. 
4. Đối với mọi`k`từ`1`ĐẾN`n`: 

Kiểm tra tất cả các bội số của`k`. 

Nếu mọi bội số đều thỏa mãn`dx[multiple] = 0`, sau đó`k`là một câu trả lời hợp lệ 
5. Xuất ra tất cả các giá trị hợp lệ. 

### Tại sao nó hoạt động 

Đối với mỗi ông chủ với`b < a`, khoảng`[b,a)`đại diện chính xác cho các vị trí mà bội số của`k`sẽ tách ra`b`Và`a`thành các khối trần khác nhau. Bất cứ khi nào một nhóm như vậy tồn tại, Polycarp thực hiện nỗ lực tiêu diệt của mình sớm hơn và Monocarp sẽ mất tên trùm đó. 

Mảng tổng tiền tố lưu trữ, đối với mỗi điểm nguyên, có bao nhiêu khoảng mất chứa nó. bội số của`k`nằm trong bất kỳ khoảng thua nào cũng tương đương với việc một con trùm nào đó đánh bại Monocarp. 

Kiểm tra tất cả các bội số của`k`đảm bảo rằng không có khoảng mất nào chứa bội số của`k`. Vì vậy, mọi ông chủ đều bị Monocarp đánh bại. Ngược lại, nếu bội số nào đó nằm trong khoảng thua thì ông chủ đó sẽ thua. Việc mô tả đặc tính là chính xác nên thuật toán trả về chính xác tất cả các giá trị hợp lệ của`k`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        dx = [0] * (n + 2)

        for ai, bi in zip(a, b):
            if bi < ai:
                dx[bi] += 1
                dx[ai] -= 1

        for i in range(1, n + 1):
            dx[i] += dx[i - 1]

        ans = []

        for k in range(1, n + 1):
            ok = True

            m = k
            while m <= n:
                if dx[m] != 0:
                    ok = False
                    break
                m += k

            if ok:
                ans.append(k)

        out.append(str(len(ans)))
        out.append(" ".join(map(str, ans)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên xây dựng tập hợp các khoảng bị cấm. Chỉ có ông chủ với`b < a`vấn đề. Nếu như`a ≤ b`, Monocarp luôn thắng được con trùm đó và không hạn chế`k`là cần thiết. 

Mảng chênh lệch lưu trữ các phần bổ sung khoảng thời gian không đổi. Sau tổng tiền tố,`dx[x]`cho chúng ta biết liệu điểm`x`thuộc ít nhất một khoảng cấm. 

Vòng lặp cuối cùng sử dụng phép lặp chuỗi hài cổ điển trên bội số. Đối với một cố định`k`, chúng tôi kiểm tra`k, 2k, 3k, ...`. Nếu bất kỳ điểm nào trong số đó bị bao phủ bởi một khoảng cấm thì`k`không hợp lệ. 

Một lỗi phổ biến là quên rằng khoảng đó là nửa mở,`[b,a)`. Việc thực hiện chính thức thực hiện:```
dx[b] += 1
dx[a] -= 1
```đại diện chính xác cho phạm vi đó. sử dụng`[b,a]`sẽ từ chối không chính xác một số giá trị hợp lệ của`k`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
a = [1,1,1]
b = [2,3,1]
```Không có ông chủ nào hài lòng`b < a`. 

| Ông chủ | một | b | Đã thêm khoảng thời gian | 
| --- | --- | --- | --- | 
| 1 | 1 | 2 | không | 
| 2 | 1 | 3 | không | 
| 3 | 1 | 1 | không | 

Sau tổng tiền tố: 

| x | dx[x] | 
| --- | --- | 
| 1 | 0 | 
| 2 | 0 | 
| 3 | 0 | 

Kiểm tra từng`k`: 

| k | Bội số | Tất cả đều bằng không? | 
| --- | --- | --- | 
| 1 | 1,2,3 | Có | 
| 2 | 2 | Có | 
| 3 | 3 | Có | 

Trả lời:```
1 2 3
```Điều này chứng tỏ trường hợp dễ dàng là mọi ông chủ đều tự động thắng. 

### Ví dụ 2 

đầu vào:```
n = 4
a = [1,4,3,2]
b = [3,3,4,1]
```Chỉ có ông chủ 2 và 4 đóng góp khoảng thời gian. 

| Ông chủ | một | b | Khoảng thời gian | 
| --- | --- | --- | --- | 
| 2 | 4 | 3 | [3,4) | 
| 4 | 2 | 1 | [1,2) | 

Sau tổng tiền tố: 

| x | dx[x] | 
| --- | --- | 
| 1 | 1 | 
| 2 | 0 | 
| 3 | 1 | 
| 4 | 0 | 

Bây giờ hãy kiểm tra mọi`k`. 

| k | Bội số | Bao phủ nhiều? | hợp lệ | 
| --- | --- | --- | --- | 
| 1 | 1,2,3,4 | vâng | không | 
| 2 | 2,4 | không | vâng | 
| 3 | 3 | vâng | không | 
| 4 | 4 | không | vâng | 

Trả lời:```
2 4
```Ví dụ này cho thấy chính xác các khoảng cấm loại bỏ các giá trị của`k`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Tổng tiền tố là O(n), kiểm tra bội số sử dụng chuỗi hài`n/1 + n/2 + ... + n/n`| 
| Không gian | O(n) | Mảng khác biệt và danh sách câu trả lời | 

Tổng số tiền của`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`2·10^5`. MỘT`O(n log n)`giải pháp tổng thể chỉ thực hiện vài triệu thao tác, phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        dx = [0] * (n + 2)

        for ai, bi in zip(a, b):
            if bi < ai:
                dx[bi] += 1
                dx[ai] -= 1

        for i in range(1, n + 1):
            dx[i] += dx[i - 1]

        ans = []

        for k in range(1, n + 1):
            ok = True
            m = k

            while m <= n:
                if dx[m] != 0:
                    ok = False
                    break
                m += k

            if ok:
                ans.append(k)

        out.append(str(len(ans)))
        out.append(" ".join(map(str, ans)))

    return "\n".join(out)

# provided sample
assert run(
"""3
3
1 1 1
2 3 1
1
1
1
4
1 4 3 2
3 3 4 1
"""
) == (
"""3
1 2 3
1
1
2
2 4"""
)

# minimum size
assert run(
"""1
1
1
1
"""
) == (
"""1
1"""
)

# all bosses automatically won
assert run(
"""1
4
1 1 1 1
4 4 4 4
"""
) == (
"""4
1 2 3 4"""
)

# single forbidden interval [1,2)
assert run(
"""1
2
2 1
1 2
"""
) == (
"""1
2"""
)

# boundary interval ending at n
assert run(
"""1
5
5 1 1 1 1
1 5 5 5 5
"""
) == (
"""2
3 5"""
)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1`|`1`| Ví dụ nhỏ nhất có thể | 
| Tất cả`a≤b`| Mọi`k`hợp lệ | Không có khoảng thời gian nào được thêm vào | 
| Khoảng thời gian`[1,2)`| Chỉ một`k=2`hợp lệ | Xử lý khoảng thời gian nửa mở | 
| Khoảng thời gian kết thúc tại`n`| Hành vi phân chia cụ thể | Cập nhật ranh giới trong mảng khác biệt | 

## Vỏ cạnh 

Hãy xem xét:```
n = 1
a = [5]
b = [5]
```Đây`a = b`. Không có khoảng thời gian nào được thêm vào vì`b < a`là sai. Mảng chênh lệch vẫn bằng 0 ở mọi nơi. Mọi`k`vượt qua bài kiểm tra bội số. 

Điều này phù hợp với tiến trình của trò chơi vì Monocarp luôn đạt đến lần thử thứ năm trước khi Polycarp đạt đến lần thử thứ năm. 

Coi như:```
n = 1
a = [5]
b = [2]
```Khoảng thời gian`[2,5)`được thêm vào. 

Vì`k = 1`, bội số`2`nằm trong khoảng nên`k`bị từ chối. 

Vì`k = 5`, bội số duy nhất được kiểm tra là`5`, nằm ngoài khoảng nửa mở. Thuật toán chấp nhận`k=5`. 

Điều đó phù hợp với trò chơi thực tế, trong đó Monocarp thực hiện năm lần thử trước khi Polycarp di chuyển. 

Coi như:```
n = 1
a = [4]
b = [3]
```Khoảng thời gian là`[3,4)`. 

Vì`k = 1`, nhiều`3`bị che phủ nên thuật toán sẽ loại bỏ nó. 

Vì`k = 2`, bội số là`2`Và`4`, cả hai đều không thuộc về`[3,4)`. Thuật toán chấp nhận nó. 

Đây chính xác là ví dụ về việc Monocarp thua cuộc`k=1`nhưng chiến thắng cho`k=2`, xác nhận rằng đặc tính khoảng thời gian đã nắm bắt được trò chơi một cách chính xác.
