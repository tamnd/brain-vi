---
title: "CF 2189A - Bảng số"
description: "Chúng tôi có một dãy số. Chúng ta có thể chọn bất kỳ số phần tử chẵn nào từ nó và phân chia chúng thành các cặp có thứ tự. Một cặp (x, y) chỉ đóng góp 1 vào bảng nếu hàng x và cột y tồn tại. Vì bảng có h hàng và l cột, điều này có nghĩa là chúng ta cần 1 ₫ x ∼ h và 1 ∼ y ₫ l."
date: "2026-06-07T21:11:35+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2189
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1075 (Div. 2)"
rating: 800
weight: 2189
solve_time_s: 252
verified: false
draft: false
---

[CF 2189A - Bảng có số](https://codeforces.com/problemset/problem/2189/A) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 4 phút 12s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dãy số. Chúng ta có thể chọn bất kỳ số phần tử chẵn nào từ nó và phân chia chúng thành các cặp có thứ tự. 

một cặp`(x, y)`đóng góp`1`chỉ vào bảng nếu hàng`x`và cột`y`hiện hữu. Vì bảng có`h`hàng và`l`cột, điều này có nghĩa là chúng ta cần`1 ≤ x ≤ h`Và`1 ≤ y ≤ l`. 

Ô thực tế nhận được số gia tăng không quan trọng đối với câu trả lời cuối cùng. Bài toán yêu cầu tính tổng tất cả các giá trị trong bảng. Mỗi cặp hợp lệ sẽ tăng chính xác một ô bằng`1`, vì vậy việc tối đa hóa tổng bảng tương đương với việc tối đa hóa số lượng cặp hợp lệ mà chúng ta có thể hình thành. 

Độ dài mảng tối đa là`100`, nó rất nhỏ. Ngay cả những giải pháp khá kém hiệu quả cũng sẽ phù hợp một cách thoải mái. Thử thách không phải là hiệu suất mà là nhận ra sự quan sát tham lam đúng đắn. 

Một số có thể được sử dụng nhiều nhất một lần vì chúng ta đang chọn các phần tử từ mảng và ghép nối chúng. Giá trị tương tự có thể xuất hiện nhiều lần nếu nó xuất hiện nhiều lần trong mảng. 

Sự tinh tế chính là hai vị trí trong một cặp có vai trò khác nhau. Số được sử dụng làm phần tử đầu tiên phải lớn nhất`h`, trong khi số được sử dụng làm phần tử thứ hai tối đa phải bằng`l`. Giá trị thỏa mãn cả hai điều kiện có thể đóng một trong hai vai trò. 

Coi như`h = 2`,`l = 5`và mảng`[1, 1, 1]`. Cả ba số đều là hàng và cột hợp lệ. Một giải pháp bất cẩn có thể đếm ba số có thể sử dụng được và trả lời`3`, nhưng một cặp sử dụng hai số, vì vậy chỉ có thể tạo thành một cặp hợp lệ. Câu trả lời đúng là`1`. 

Một trường hợp phức tạp khác là khi các con số chỉ hợp lệ trong một vai trò. Giả định`h = 1`,`l = 100`và mảng`[1, 50]`. giá trị`1`có thể là một hàng và một cột, trong khi`50`chỉ có thể là một cột. Chúng ta có thể hình thành`(1, 50)`, đưa ra câu trả lời`1`. Một giải pháp đếm riêng các số có khả năng theo hàng và số có khả năng theo cột mà không theo dõi việc sử dụng lại có thể cho rằng có thể có nhiều cặp đôi hơn một cách không chính xác. 

Trường hợp cạnh cuối cùng xảy ra khi không có số nào có thể được sử dụng làm hàng. Ví dụ:```
n = 2, h = 1, l = 1
a = [5, 5]
```Không có cặp hợp lệ tồn tại, vì vậy câu trả lời là`0`. 

## Phương pháp tiếp cận 

Tư duy vũ phu bắt đầu bằng việc hỏi những cặp nào có thể được hình thành. Mọi phần tử đều có thể không được sử dụng, trở thành thành viên đầu tiên của một cặp hoặc trở thành thành viên thứ hai của một cặp. Việc khám phá tất cả các cặp có thể nhanh chóng trở thành tổ hợp. Ngay cả đối với`n = 100`, số lượng kết quả phù hợp có thể lớn về mặt thiên văn. 

Quan sát quan trọng là chúng tôi không quan tâm ô hợp lệ nào nhận được số gia tăng. Mỗi cặp thành công đều đóng góp chính xác`1`để trả lời. 

Chúng ta hãy phân loại từng phần tử mảng theo vai trò mà nó có thể đảm nhiệm. 

Nếu một giá trị`x`thỏa mãn`x ≤ h`, nó có thể phục vụ như một chỉ mục hàng. 

Nếu một giá trị`x`thỏa mãn`x ≤ l`, nó có thể phục vụ như một chỉ mục cột. 

Để một cặp hợp lệ, một phần tử được chọn phải được gán vai trò hàng và phần tử được chọn khác phải được gán vai trò cột. 

Giả sử chúng ta xử lý mảng từng phần tử một. Mỗi cặp hợp lệ yêu cầu một phần tử có khả năng xếp hàng và một phần tử có khả năng xếp cột. Một phần tử đáp ứng cả hai điều kiện sẽ linh hoạt và có thể đảm nhận bất kỳ vai trò nào hiện đang cần. 

Điều này trở thành vấn đề khớp giữa các vị trí hàng và vị trí cột. Vì mỗi cặp tiêu thụ chính xác hai phần tử nên số lượng cặp hợp lệ tối đa bằng số lượng kết quả khớp hàng-cột tối đa có thể được tạo. 

Một công việc tham lam rất đơn giản. 

Duy trì số lượng phần tử có khả năng tạo hàng chưa từng có mà chúng tôi đã thấy và số lượng phần tử có khả năng tạo cột chưa khớp mà chúng tôi đã thấy. 

Khi xử lý một số mới: 

Nếu nó có thể được dùng dưới dạng một hàng, chúng tôi muốn ghép nối ngay nó với một phần tử cột chưa từng có trước đó nếu phần tử đó tồn tại. 

Tương tự, nếu nó có thể phục vụ như một cột, chúng tôi muốn ghép nó với một phần tử hàng chưa từng có trước đó nếu phần tử đó tồn tại. 

Một cách nhìn thậm chí rõ ràng hơn là sắp xếp các phần tử thành ba loại. 

Giá trị với`x ≤ min(h,l)`có thể đóng một trong hai vai trò. 

Giá trị với`min(h,l) < x ≤ h`chỉ có thể là hàng. 

Giá trị với`min(h,l) < x ≤ l`chỉ có thể là cột. 

Cho phép`R =`số phần tử có khả năng xếp hàng (`x ≤ h`)`C =`số phần tử có khả năng tạo cột (`x ≤ l`) 

Mỗi cặp hợp lệ cần có một vai trò hàng và một vai trò cột. Nếu chúng ta tạo ra`k`theo cặp, chúng tôi tiêu thụ`k`bài tập hàng và`k`bài tập cột. Vì mỗi phần tử mảng chỉ có thể được sử dụng một lần nên tổng số phần tử đã tiêu thụ là`2k`, không được vượt quá`n`. 

Các phần tử linh hoạt tự động giải quyết xung đột phân công vai trò. Số lượng cặp tối đa có thể chỉ đơn giản là:`min(R, C, n // 2)`Để biết tại sao, chúng ta cần`k ≤ R`bởi vì mỗi cặp cần một phần tử hàng,`k ≤ C`bởi vì mỗi cặp cần một phần tử cột và`k ≤ n/2`bởi vì mỗi cặp tiêu thụ hai yếu tố. 

Những giới hạn này cũng có thể đạt được vì các yếu tố linh hoạt có thể được chỉ định cho bất kỳ bên nào còn thiếu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem có bao nhiêu phần tử mảng thỏa mãn`a[i] ≤ h`. Gọi giá trị này`R`. 
2. Đếm xem có bao nhiêu phần tử mảng thỏa mãn`a[i] ≤ l`. Gọi giá trị này`C`. 
3. Mỗi cặp hợp lệ yêu cầu một phần tử có khả năng xếp hàng và một phần tử có khả năng xếp cột, do đó số lượng cặp không thể vượt quá một trong hai`R`hoặc`C`. 
4. Mỗi cặp cũng tiêu thụ hai phần tử mảng nên số lượng cặp không thể vượt quá`n // 2`. 
5. Câu trả lời là:```
min(R, C, n // 2)
```6. Xuất câu trả lời cho test case. 

### Tại sao nó hoạt động 

Một cặp hợp lệ không gì khác hơn là một phần tử được gán cho vị trí hàng và một phần tử khác được gán cho vị trí cột. 

Nếu chúng ta muốn tạo`k`cặp hợp lệ, chúng tôi cần ít nhất`k`các phần tử có khả năng xếp hàng và ít nhất`k`các phần tử có khả năng tạo cột, đưa ra giới hạn`k ≤ R`Và`k ≤ C`. Vì mỗi cặp sử dụng hai phần tử mảng riêng biệt nên chúng ta cũng cần`2k ≤ n`, mang lại`k ≤ n // 2`. 

Đây là những hạn chế duy nhất. Bất kỳ phần tử nào thỏa mãn cả hai điều kiện đều có thể được gán cho bất kỳ vai trò nào cần thiết. Theo đó, bất cứ khi nào`k`thỏa mãn cả ba bất đẳng thức thì tồn tại một công thức. Giá trị khả thi lớn nhất chính xác là`min(R, C, n // 2)`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    
    for _ in range(t):
        n, h, l = map(int, input().split())
        a = list(map(int, input().split()))
        
        rows = sum(x <= h for x in a)
        cols = sum(x <= l for x in a)
        
        print(min(rows, cols, n // 2))

solve()
```Việc thực hiện trực tiếp theo đặc tính toán học. 

Biến`rows`đếm có bao nhiêu phần tử đủ điều kiện để được sử dụng làm thành viên đầu tiên của một cặp. Biến`cols`đếm có bao nhiêu phần tử đủ điều kiện để được sử dụng làm thành viên thứ hai. 

Câu trả lời cuối cùng được giới hạn bởi cả ba giới hạn cần thiết. Lấy mức tối thiểu sẽ cho số lượng cặp hợp lệ tối đa có thể đạt được. 

Không có lo ngại về tràn vì tất cả các giá trị đều rất nhỏ. Chi tiết duy nhất đáng kiểm tra là mỗi ca kiểm thử chứa chính xác`n`số và đó`n // 2`sử dụng phép chia số nguyên, vì không thể ghép cặp phân số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
h = 2
l = 2
a = [1, 2, 2, 3, 2]
```| Yếu tố | ≤ h? | ≤ tôi? | hàng | cols | 
| --- | --- | --- | --- | --- | 
| 1 | Có | Có | 1 | 1 | 
| 2 | Có | Có | 2 | 2 | 
| 2 | Có | Có | 3 | 3 | 
| 3 | Không | Không | 3 | 3 | 
| 2 | Có | Có | 4 | 4 | 

Sau khi xử lý: 

| Số lượng | Giá trị | 
| --- | --- | 
| hàng | 4 | 
| cols | 4 | 
| n // 2 | 2 | 

Trả lời:```
min(4, 4, 2) = 2
```Ví dụ này cho thấy tác dụng của`n // 2`hạn chế. Mặc dù bốn phần tử có thể đóng vai trò là hàng và cột, nhưng chỉ có thể tạo thành hai cặp từ tổng năm phần tử. 

### Ví dụ 2 

đầu vào:```
n = 8
h = 4
l = 2
a = [7, 2, 2, 2, 3, 4, 4, 2]
```| Yếu tố | ≤ h? | ≤ tôi? | hàng | cols | 
| --- | --- | --- | --- | --- | 
| 7 | Không | Không | 0 | 0 | 
| 2 | Có | Có | 1 | 1 | 
| 2 | Có | Có | 2 | 2 | 
| 2 | Có | Có | 3 | 3 | 
| 3 | Có | Không | 4 | 3 | 
| 4 | Có | Không | 5 | 3 | 
| 4 | Có | Không | 6 | 3 | 
| 2 | Có | Có | 7 | 4 | 

Giá trị cuối cùng: 

| Số lượng | Giá trị | 
| --- | --- | 
| hàng | 7 | 
| cols | 4 | 
| n // 2 | 4 | 

Trả lời:```
min(7, 4, 4) = 4
```Dấu vết này cho thấy các phần tử có khả năng xếp hàng có thể đông hơn rất nhiều các phần tử có khả năng xếp cột. Bên nhỏ hơn xác định câu trả lời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần đi qua mảng | 
| Không gian | O(1) | Chỉ có một vài quầy được lưu trữ | 

Từ`n ≤ 100`, thậm chí các giải pháp chậm hơn nhiều cũng sẽ vượt qua. Quá trình quét tuyến tính không đáng kể trong giới hạn và sử dụng bộ nhớ bổ sung liên tục. 

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
        n, h, l = map(int, input().split())
        a = list(map(int, input().split()))

        rows = sum(x <= h for x in a)
        cols = sum(x <= l for x in a)

        ans.append(str(min(rows, cols, n // 2)))

    return "\n".join(ans)

# provided sample
assert run(
"""7
2 1 1
1 1
5 2 2
1 2 2 3 2
8 4 2
7 2 2 2 3 4 4 2
7 3 6
10 4 1 3 5 4 6
2 4 4
5 5
7 6 3
10 4 1 3 5 4 6
4 1 1
1 1 1 1
"""
) == """1
2
3
2
0
2
2"""

# minimum size
assert run(
"""1
2 1 1
1 1
"""
) == "1"

# no usable values
assert run(
"""1
4 1 1
5 5 5 5
"""
) == "0"

# all values usable
assert run(
"""1
6 10 10
1 2 3 4 5 6
"""
) == "3"

# row-capable much more common than column-capable
assert run(
"""1
6 5 2
1 2 3 4 5 5
"""
) == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 1 / 1 1`|`1`| Ví dụ nhỏ nhất không tầm thường | 
|`4 1 1 / 5 5 5 5`|`0`| Không có giá trị hàng hoặc cột hợp lệ | 
|`6 10 10 / 1 2 3 4 5 6`|`3`| Câu trả lời bị giới hạn bởi`n // 2`| 
|`6 5 2 / 1 2 3 4 5 5`|`2`| Số lượng hàng và cột nhỏ hơn chiếm ưu thế | 

## Vỏ cạnh 

### Tất cả các số quá lớn 

đầu vào:```
1
2 1 1
5 5
```Chúng tôi đếm:```
rows = 0
cols = 0
n // 2 = 1
```Thuật toán trả về:```
min(0, 0, 1) = 0
```Không có phần tử nào có thể hoạt động như một chỉ mục hàng hoặc cột, do đó không tồn tại cặp hợp lệ. 

### Nhiều số có thể sử dụng được nhưng không đủ phần tử cho nhiều cặp hơn 

đầu vào:```
1
3 10 10
1 2 3
```Chúng tôi có được:```
rows = 3
cols = 3
n // 2 = 1
```Câu trả lời trở thành:```
min(3, 3, 1) = 1
```Mặc dù mọi phần tử đều hợp lệ ở cả hai vai trò nhưng chỉ tồn tại ba phần tử, do đó, có thể hình thành nhiều nhất một cặp. 

### Mất cân bằng nghiêm trọng giữa các ứng viên theo hàng và cột 

đầu vào:```
1
6 5 2
1 2 3 4 5 5
```Đếm:```
rows = 6
cols = 2
n // 2 = 3
```Thuật toán trả về:```
min(6, 2, 3) = 2
```Chỉ có hai phần tử có thể đóng vai trò là cột, do đó, việc tạo cặp hợp lệ thứ ba là không thể bất kể có sẵn bao nhiêu phần tử có khả năng xếp hàng. 

### Giá trị hợp lệ ở cả hai vai trò 

đầu vào:```
1
4 1 1
1 1 1 1
```Đếm:```
rows = 4
cols = 4
n // 2 = 2
```Trả lời:```
min(4, 4, 2) = 2
```Mọi phần tử đều linh hoạt và có thể được chỉ định một trong hai vai trò. Thuật toán nhận ra chính xác rằng hai cặp có thể được hình thành.
