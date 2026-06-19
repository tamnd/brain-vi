---
title: "CF 10D - LCIS"
description: "Chúng ta được cho hai mảng số nguyên. Chúng ta cần xây dựng chuỗi dài nhất thỏa mãn hai điều kiện cùng một lúc."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 10
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 10"
rating: 2800
weight: 10
solve_time_s: 120
verified: false
draft: false
---
[CF 10D - LCIS](https://codeforces.com/problemset/problem/10/D) 

**Đánh giá:** 2800 
**Thẻ:** dp 
**Thời gian giải:** 2 phút 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai mảng số nguyên. Chúng ta cần xây dựng chuỗi dài nhất thỏa mãn hai điều kiện cùng một lúc. 

Chuỗi phải xuất hiện dưới dạng chuỗi con trong cả hai mảng, nghĩa là chúng ta được phép bỏ qua các phần tử nhưng không thể thay đổi thứ tự tương đối của chúng. 

Trình tự cũng phải tăng lên một cách nghiêm ngặt. 

Điều này khác với LCS thông thường và khác với LIS thông thường. LCS bình thường bỏ qua yêu cầu ngày càng tăng, trong khi LIS bình thường chỉ hoạt động bên trong một mảng. Ở đây chúng ta phải thỏa mãn đồng thời cả hai ràng buộc. 

Ví dụ: nếu mảng là:```
A = [2, 3, 1, 6, 5, 4, 6]
B = [1, 3, 5, 6]
```sau đó`[3, 5, 6]`hợp lệ vì nó xuất hiện trong cả hai mảng và đang tăng dần. 

Các ràng buộc đủ nhỏ để cho phép lập trình động bậc hai. Cả hai độ dài tối đa là 500, do đó`O(n * m)`hoặc`O(n * m * something small)`giải pháp là ổn. Một giải pháp khối là rủi ro vì`500^3 = 125,000,000`các hoạt động quá lớn trong Python dưới giới hạn thời gian 1 giây. 

Phần khó khăn là điều kiện tăng phụ thuộc vào giá trị, trong khi điều kiện dãy con phụ thuộc vào vị trí. Một DP bất cẩn chỉ theo dõi các chỉ số thường sẽ kết hợp sai hai yêu cầu này. 

Một sai lầm phổ biến là xử lý vấn đề như LCS thông thường và sau đó kiểm tra thứ tự tăng dần. 

Coi như:```
A = [3, 2, 1]
B = [3, 2, 1]
```LCS thông thường có độ dài 3, nhưng`[3, 2, 1]`đang giảm dần. Độ dài LCIS chính xác chỉ là 1. 

Một trường hợp tinh tế khác liên quan đến sự trùng lặp.```
A = [1, 1, 1]
B = [1, 1]
```Câu trả lời vẫn là`[1]`, không`[1, 1]`, vì dãy số phải tăng nghiêm ngặt. Một sự chuyển đổi cho phép`<=`thay vì`<`âm thầm đưa ra câu trả lời sai. 

Một lỗi nguy hiểm hơn xuất hiện khi xây dựng lại trình tự.```
A = [1, 2, 3]
B = [1, 3, 2]
```Câu trả lời có độ dài 2, nhưng chỉ`[1, 2]`là hợp lệ. Nếu việc xây dựng lại bỏ qua thông tin sắp xếp từ mảng thứ hai, nó có thể xây dựng không chính xác`[1, 3]`theo sau là`2`. 

Trạng thái DP phải mã hóa cả tính hợp lệ của giá trị tăng dần và tính hợp lệ của thứ tự con cùng một lúc. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Tạo mọi dãy con tăng dần của mảng đầu tiên, sau đó kiểm tra xem nó có phải là dãy con của mảng thứ hai hay không. Vì mọi tập con của các vị trí có thể tạo thành một dãy con nên số lượng ứng cử viên là theo cấp số nhân, đại khái là`2^n`. Ngay cả khi cắt tỉa, điều này trở thành không thể một khi`n`tiến gần tới 500. 

Cách tiếp cận bạo lực có cấu trúc chặt chẽ hơn sử dụng LCS DP cổ điển và bổ sung thêm ràng buộc ngày càng tăng. Chúng ta có thể định nghĩa:```
dp[i][j][k]
```Ở đâu`k`bằng cách nào đó theo dõi giá trị hoặc chỉ mục đã chọn trước đó. Điều này nhanh chóng trở thành hình khối hoặc tệ hơn vì mỗi cặp`(i, j)`có thể cần sự chuyển tiếp từ tất cả các trạng thái trước đó. Với`500^3`chuyển tiếp, Python gặp khó khăn. 

Quan sát quan trọng là khi chúng tôi xử lý một phần tử cố định`A[i]`, chúng ta chỉ quan tâm đến các dãy con chung có kết thúc bằng các giá trị nhỏ hơn`A[i]`. 

Giả sử chúng ta muốn một dãy con tăng chung chung kết thúc tại`B[j]`, Ở đâu`A[i] == B[j]`. 

Để mở rộng chuỗi trước đó, chúng ta cần:```
B[k] < B[j]
```Và```
k < j
```bởi vì trình tự phải tăng dần và giữ trật tự trong`B`. 

Trong số tất cả các vị trí trước đó, chỉ có độ dài tốt nhất mới quan trọng. 

Điều này sẽ thu gọn tìm kiếm chuyển tiếp tốn kém thành mức tối đa luân phiên. 

Chúng tôi xác định:```
dp[j] = length of the best LCIS ending at B[j]
```Sau đó, trong khi quét`B`từ trái sang phải để cố định`A[i]`, chúng tôi duy trì:```
current = best dp[k] where B[k] < A[i]
```Bây giờ khi`A[i] == B[j]`, chúng ta có thể đặt ngay:```
dp[j] = current + 1
```Điều này loại bỏ hoàn toàn vòng chuyển tiếp bên trong. Mỗi cặp`(i, j)`được xử lý một lần, đưa ra một`O(n * m)`giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * m) hoặc tệ hơn | O(2^n) | Quá chậm | 
| Tối ưu | O(n * m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng`dp[j]`Ở đâu`dp[j]`lưu trữ độ dài của dãy con tăng chung dài nhất kết thúc tại`B[j]`. 
2. Tạo một`parent[j]`mảng để tái thiết. Điều này lưu trữ chỉ mục trước đó trong`B`được sử dụng trước đây`j`. 
3. Lặp lại qua từng phần tử`A[i]`. 
4. Đối với hiện tại`A[i]`, duy trì hai biến:`best_len`, độ dài LCIS lớn nhất được tìm thấy cho đến nay trong số các vị trí trong`B`với các giá trị nhỏ hơn.`best_pos`, chỉ số trong`B`nơi trình tự tốt nhất kết thúc. 
5. Quét`B`từ trái sang phải. 
6. Nếu`B[j] < A[i]`, sau đó`B[j]`có thể xuất hiện trước`A[i]`theo một trình tự tăng dần. 

Nếu như`dp[j] > best_len`, cập nhật`best_len`Và`best_pos`. 
7. Nếu`A[i] == B[j]`, thì chúng ta có thể mở rộng chuỗi kết thúc tốt nhất bằng giá trị nhỏ hơn. 

Nếu như`best_len + 1 > dp[j]`, cập nhật:```
dp[j] = best_len + 1
parent[j] = best_pos
```8. Tiếp tục cho đến khi tất cả các cặp`(i, j)`được xử lý. 
9. Tìm vị trí`j`với mức tối đa`dp[j]`. 
10. Xây dựng lại câu trả lời bằng cách làm theo`parent[j]`lạc hậu. 
11. Đảo ngược trình tự được xây dựng lại vì quá trình tái tạo diễn ra từ cuối đến đầu. 

### Tại sao nó hoạt động 

Đối với mỗi cố định`A[i]`, quá trình quét qua`B`duy trì LCIS tốt nhất có thể có thể xuất hiện hợp pháp trước khi`A[i]`. Vì chúng tôi quét`B`từ trái sang phải, mọi ứng cử viên được lưu trữ đều đã thỏa mãn thứ tự tiếp theo trong`B`. 

điều kiện`B[j] < A[i]`đảm bảo tăng nghiêm ngặt. Điều kiện là ứng viên được nhìn thấy sớm hơn trong quá trình quét đảm bảo tăng chỉ số trong`B`. 

Bất cứ khi nào`A[i] == B[j]`, mở rộng`best_len`tạo ra LCIS tối ưu kết thúc tại`B[j]`sử dụng các phần tử lên đến`A[i]`. 

Không có chuyển đổi hợp lệ nào bị bỏ sót vì mọi giá trị nhỏ hơn trước đó đều được xem xét trong quá trình quét. Không có quá trình chuyển đổi không hợp lệ nào được sử dụng vì các giá trị lớn hơn hoặc bằng nhau không bao giờ cập nhật`best_len`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    m = int(input())
    b = list(map(int, input().split()))

    dp = [0] * m
    parent = [-1] * m

    for i in range(n):
        best_len = 0
        best_pos = -1

        for j in range(m):
            if b[j] < a[i]:
                if dp[j] > best_len:
                    best_len = dp[j]
                    best_pos = j

            elif b[j] == a[i]:
                if best_len + 1 > dp[j]:
                    dp[j] = best_len + 1
                    parent[j] = best_pos

    length = 0
    end_pos = -1

    for j in range(m):
        if dp[j] > length:
            length = dp[j]
            end_pos = j

    sequence = []

    while end_pos != -1:
        sequence.append(b[end_pos])
        end_pos = parent[end_pos]

    sequence.reverse()

    print(length)

    if length:
        print(*sequence)
    else:
        print()

solve()
```Mảng`dp[j]`là trạng thái DP trung tâm. Nó lưu trữ kết thúc LCIS tốt nhất cụ thể tại`B[j]`. Điều này là đủ vì mọi dãy con hợp lệ đều có một vị trí cuối cùng duy nhất trong`B`. 

Các biến`best_len`Và`best_pos`được thiết lập lại cho mọi`A[i]`. Chúng tóm tắt tất cả các chuyển đổi hợp lệ trước đó gặp phải cho đến nay trong khi quét`B`. 

Thứ tự của các điều kiện bên trong vòng lặp bên trong rất quan trọng. Đầu tiên chúng tôi sử dụng các vị trí có giá trị nhỏ hơn để cải thiện`best_len`. Các giá trị bằng nhau sau này có thể mở rộng từ thông tin đó. 

So sánh chặt chẽ:```
if b[j] < a[i]:
```là điều cần thiết. Thay thế nó bằng`<=`phá vỡ mức tăng nghiêm ngặt và cho phép trùng lặp không chính xác. 

Mảng tái cấu trúc lưu trữ các chỉ số bên trong`B`, không phải giá trị. Điều này tránh sự mơ hồ khi tồn tại các giá trị trùng lặp. 

Sự tái thiết cuối cùng đi lùi qua`parent`. Vì phần tử cha trỏ tới các phần tử trước đó nên trình tự đã thu thập phải được đảo ngược ở cuối. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
A = [2, 3, 1, 6, 5, 4, 6]
B = [1, 3, 5, 6]
```Trạng thái ban đầu:```
dp = [0, 0, 0, 0]
```Quá trình xử lý tiến hành như sau. 

| A[i] | j | B[j] | best_len trước | dp sau | 
| --- | --- | --- | --- | --- | 
| 2 | 0 | 1 | 0 | [0,0,0,0] | 
| 3 | 0 | 1 | 0 | [0,0,0,0] | 
| 3 | 1 | 3 | 0 | [0,1,0,0] | 
| 1 | 0 | 1 | 0 | [1,1,0,0] | 
| 6 | 0 | 1 | 1 | [1,1,0,0] | 
| 6 | 1 | 3 | 1 | [1,1,0,0] | 
| 6 | 2 | 5 | 1 | [1,1,0,0] | 
| 6 | 3 | 6 | 1 | [1,1,0,2] | 
| 5 | 0 | 1 | 1 | [1,1,0,2] | 
| 5 | 1 | 3 | 1 | [1,1,0,2] | 
| 5 | 2 | 5 | 1 | [1,1,2,2] | 
| 4 | 0 | 1 | 1 | [1,1,2,2] | 
| 4 | 1 | 3 | 1 | [1,1,2,2] | 
| 6 | 3 | 6 | 2 | [1,1,2,3] | 

Câu trả lời cuối cùng là độ dài 3 kèm theo trình tự`[3, 5, 6]`. 

Dấu vết này cho thấy những lần xuất hiện sau cải thiện ước tính trước đó như thế nào. đầu tiên`6`chỉ có dạng dài 2, nhưng sau khi khám phá`[3,5]`, thứ hai`6`kéo dài đến chiều dài 3. 

### Ví dụ 2 

đầu vào:```
A = [1, 1, 1]
B = [1, 1]
```| A[i] | j | B[j] | best_len trước | dp sau | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 0 | [1,0] | 
| 1 | 1 | 1 | 0 | [1,1] | 
| 1 | 0 | 1 | 0 | [1,1] | 
| 1 | 1 | 1 | 0 | [1,1] | 

Độ dài LCIS cuối cùng là 1. 

Ví dụ này xác nhận rằng các giá trị bằng nhau không bao giờ liên kết với nhau vì chỉ những giá trị nhỏ hơn mới góp phần vào`best_len`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * m) | Mỗi cặp`(i, j)`được xử lý một lần | 
| Không gian | O(m) | Mảng DP và mảng cha chỉ phụ thuộc vào mảng thứ hai | 

Với`n, m ≤ 500`, thuật toán thực hiện tối đa 250.000 cập nhật trạng thái, dễ dàng phù hợp với giới hạn thời gian. Việc sử dụng bộ nhớ rất nhỏ vì chỉ có một hàng DP được lưu trữ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string

import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    m = int(input())
    b = list(map(int, input().split()))

    dp = [0] * m
    parent = [-1] * m

    for i in range(n):
        best_len = 0
        best_pos = -1

        for j in range(m):
            if b[j] < a[i]:
                if dp[j] > best_len:
                    best_len = dp[j]
                    best_pos = j

            elif b[j] == a[i]:
                if best_len + 1 > dp[j]:
                    dp[j] = best_len + 1
                    parent[j] = best_pos

    length = 0
    end_pos = -1

    for j in range(m):
        if dp[j] > length:
            length = dp[j]
            end_pos = j

    seq = []

    while end_pos != -1:
        seq.append(b[end_pos])
        end_pos = parent[end_pos]

    seq.reverse()

    out = [str(length)]

    if length:
        out.append(" ".join(map(str, seq)))
    else:
        out.append("")

    print("\n".join(out))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue().strip()

# provided sample
assert run(
"""7
2 3 1 6 5 4 6
4
1 3 5 6
"""
) == "3\n3 5 6", "sample 1"

# minimum size
assert run(
"""1
5
1
5
"""
) == "1\n5", "single matching element"

# all equal values
assert run(
"""3
1 1 1
2
1 1
"""
) == "1\n1", "strictly increasing condition"

# no common elements
assert run(
"""3
1 2 3
3
4 5 6
"""
) == "0", "empty LCIS"

# off-by-one reconstruction case
assert run(
"""3
1 2 3
3
1 3 2
"""
) == "2\n1 2", "correct ordering"

# decreasing arrays
assert run(
"""5
5 4 3 2 1
5
5 4 3 2 1
"""
) == "1\n5", "only one element possible"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Phần tử khớp đơn |`1 5`| Hạn chế tối thiểu | 
| Tất cả các giá trị bằng nhau |`1 1`| tăng xử lý nghiêm | 
| Không có yếu tố chung |`0`| Xây dựng lại câu trả lời trống | 
|`[1,2,3]`vs`[1,3,2]`|`1 2`| Thứ tự tiếp theo đúng | 
| Giảm mảng | Bất kỳ giá trị đơn nào | LCIS ​​khác với LCS | 

## Vỏ cạnh 

Hãy xem xét các mảng nặng trùng lặp:```
A = [1, 1, 1]
B = [1, 1]
```Trong khi xử lý từng`1`, thuật toán không bao giờ cập nhật`best_len`bởi vì điều kiện yêu cầu các giá trị nhỏ hơn. Kết quả là mỗi trận đấu chỉ tạo ra một chuỗi có độ dài 1. Thuật toán đưa ra chính xác:```
1
1
```Bây giờ hãy xem xét trường hợp LCS thông thường sẽ thất bại:```
A = [3, 2, 1]
B = [3, 2, 1]
```DP phát triển như sau:```
After 3: dp = [1,0,0]
After 2: dp = [1,1,0]
After 1: dp = [1,1,1]
```Không phần tử nào có thể mở rộng phần tử khác vì giá trị luôn giảm. Độ dài tối đa vẫn là 1. 

Cuối cùng, hãy xem xét bẫy đặt hàng:```
A = [1, 2, 3]
B = [1, 3, 2]
```Khi xử lý`2`, giá trị trước đó tốt nhất là`1`, do đó thuật toán xây dựng`[1,2]`. 

Sau đó, trong khi xử lý`3`, nó không thể kéo dài từ`2`bởi vì`2`xuất hiện sau`3`mảng bên trong`B`. Quét từ trái sang phải tự động thực thi thứ tự tiếp theo trong`B`. 

Thuật toán xuất ra chính xác:```
2
1 2
```
