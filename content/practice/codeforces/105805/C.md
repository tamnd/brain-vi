---
title: "CF 105805C - Hoán vị sắp xếp lại"
description: "Chúng ta được hoán vị các số từ 1 đến n. Một thao tác chọn hai vị trí có giá trị hiện tại là a và b. Nếu a + b là hợp số thì chúng ta có thể hoán đổi hai giá trị đó."
date: "2026-06-25T15:31:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105805
codeforces_index: "C"
codeforces_contest_name: "TheForces Round #41 (Magical-Forces)"
rating: 0
weight: 105805
solve_time_s: 52
verified: true
draft: false
---

[CF 105805C - Hoán vị sắp xếp lại](https://codeforces.com/problemset/problem/105805/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị của các số từ`1`ĐẾN`n`. 

Một thao tác chọn hai vị trí có giá trị hiện tại là`a`Và`b`. Nếu như`a + b`là hợp số nên ta có thể hoán đổi hai giá trị đó. 

Câu hỏi đặt ra là liệu sau khi thực hiện bất kỳ số lần hoán đổi nào như vậy, chúng ta có thể chuyển đổi hoán vị thành hoán vị được sắp xếp hay không?```
1 2 3 ... n
```Số lượng ca kiểm thử lớn và tổng của tất cả`n`giá trị nhiều nhất là`2 · 10^5`. Bất kỳ giải pháp nào chậm hơn đáng kể so với tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm sẽ gặp khó khăn. Một cái gì đó giống như`O(n^2)`mỗi trường hợp thử nghiệm đã quá đắt khi`n`cách tiếp cận`2 · 10^5`. 

Điều thú vị là hoạt động chỉ phụ thuộc vào các giá trị được hoán đổi chứ không phụ thuộc vào vị trí của chúng. Nếu giá trị`x`Và`y`thỏa mãn điều đó`x + y`là hợp số thì hai giá trị đó có thể hoán đổi cho nhau bất cứ khi nào chúng xuất hiện trong hoán vị. 

Một vài trường hợp nhỏ tiết lộ cấu trúc. 

Vì```
n = 2
p = [2, 1]
```cặp duy nhất là`(1,2)`Và`1 + 2 = 3`, đó là số nguyên tố. Không thể trao đổi được, vì vậy câu trả lời là`NO`. 

Vì```
n = 3
p = [1, 3, 2]
```cặp đôi`(1,3)`có thể đổi chỗ vì`1 + 3 = 4`là tổng hợp, nhưng giá trị`2`bị cô lập. Từ`2`đã ở đúng vị trí của nó thì có thể sắp xếp và câu trả lời là`YES`. 

Việc triển khai bất cẩn có thể cho rằng mọi hoán vị đều có thể sắp xếp được một lần`n ≥ 3`, điều đó là sai. Ví dụ:```
n = 4
p = [2, 1, 4, 3]
```Giá trị`{1,3}`tạo thành một nhóm và`{2,4}`tạo thành một cái khác. Các giá trị chỉ có thể di chuyển trong nhóm của chính chúng. Chức vụ`1`cuối cùng cần giá trị`1`, nhưng hiện chứa giá trị`2`từ một nhóm khác, khiến việc phân loại không thể thực hiện được. 

## Phương pháp tiếp cận 

Chế độ xem brute-force là xây dựng một biểu đồ có các đỉnh là các giá trị`1..n`. Hai giá trị được kết nối nếu tổng của chúng là hợp số. Mỗi phép hoán đổi được phép tương ứng với việc hoán đổi hai đỉnh được nối với nhau bằng một cạnh. 

Người ta có thể mô phỏng nhiều lần các giao dịch hoán đổi và khám phá các hoán vị có thể tiếp cận được. Điều này đúng với các đầu vào nhỏ, nhưng không gian trạng thái`n!`, vì vậy nó trở nên vô vọng gần như ngay lập tức. 

Quan sát quan trọng là chúng ta không thực sự quan tâm đến các chuỗi hoán đổi riêng lẻ. 

Hãy xem xét biểu đồ trên các giá trị`1..n`nơi tồn tại một cạnh giữa`x`Và`y`khi`x + y`là tổng hợp. 

Một thực tế cổ điển từ các nhóm hoán vị là nếu một thành phần đồ thị được kết nối thì các phép hoán đổi dọc theo các cạnh của nó sẽ tạo ra mọi hoán vị của các đỉnh trong thành phần đó. Trong điều kiện thực tế, tất cả các giá trị bên trong cùng một thành phần được kết nối có thể được sắp xếp lại tùy ý giữa các vị trí hiện đang bị các giá trị của thành phần đó chiếm giữ. 

Điều này biến vấn đề thành một câu hỏi thành phần được kết nối. 

Giả sử một thành phần chứa các giá trị`{1,3}`. Không có thao tác nào có thể di chuyển một giá trị từ thành phần này sang vị trí hiện đang bị chiếm giữ bởi một giá trị từ thành phần khác. Tập hợp các vị trí bị chiếm giữ bởi các giá trị thành phần sẽ không thay đổi mãi mãi. 

Để sắp xếp hoán vị, giá trị`i`cuối cùng phải đạt được vị trí`i`. Vì vậy, với mỗi vị trí`pos`, giá trị hiện tại`p[pos]`và giá trị mục tiêu`pos`phải thuộc về cùng một thành phần được kết nối. 

Nhiệm vụ còn lại là tìm hiểu cấu trúc thành phần của đồ thị. 

Vì`n ≥ 5`, đồ thị được kết nối. 

Tất cả các số lẻ tạo thành một tập hợp vì tổng của hai số lẻ ít nhất là một số chẵn`4`, do đó tổng hợp. 

Tất cả các số chẵn tạo thành một nhóm vì tổng của hai số chẵn ít nhất là một số chẵn`4`, do đó tổng hợp. 

Giá trị`5`tồn tại khi`n ≥ 5`, Và```
5 + 2 = 7
5 + 4 = 9
```Ít nhất một số chẵn kết nối với nhóm lẻ, tạo thành cầu nối giữa các nhóm chẵn lẻ. Do đó toàn bộ đồ thị trở nên kết nối. 

Các giá trị đặc biệt duy nhất của`n`là:```
n = 1
component: {1}

n = 2
components: {1}, {2}

n = 3
components: {1,3}, {2}

n = 4
components: {1,3}, {2,4}
```Một khi điều này được biết đến, giải pháp sẽ trở nên rất nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hoán vị. 
2. Nếu`n = 1`, đầu ra`YES`. 
3. Nếu`n ≥ 5`, đầu ra`YES`. 

Đồ thị của các giá trị được kết nối, do đó, bất kỳ hoán vị nào cũng có thể được chuyển đổi thành bất kỳ hoán vị nào khác. 
4. Nếu`n = 2`, mỗi giá trị được cô lập. 

Hoán vị chỉ có thể sắp xếp được nếu nó đã được sắp xếp. 
5. Nếu`n = 3`hoặc`n = 4`, có đúng hai thành phần:`A = {1,3}`Và`B = {2,4}`giới hạn ở những giá trị thực sự tồn tại. 
6. Đối với mọi vị trí`i`, kiểm tra xem`i`Và`p[i]`thuộc cùng một thành phần. 
7. Nếu mọi vị trí đều vượt qua quá trình kiểm tra, hãy xuất ra`YES`; nếu không thì xuất ra`NO`. 

### Tại sao nó hoạt động 

Các thành phần được kết nối xác định giá trị nào có thể trao đổi vị trí. Giá trị từ các thành phần khác nhau không bao giờ có thể vượt qua ranh giới thành phần. 

Bên trong một thành phần được kết nối, các hoán đổi dọc theo các cạnh của biểu đồ sẽ tạo ra các hoán vị tùy ý cho các giá trị của thành phần đó. Do đó, trở ngại duy nhất là liệu một giá trị có cần di chuyển đến vị trí thuộc về một thành phần khác hay không. 

Kiểm tra điều đó`component(i) = component(p[i])`đối với mọi vị trí xác minh chính xác rằng mọi giá trị có thể được sắp xếp lại đến vị trí đích của nó chỉ bằng cách sử dụng các hoán đổi bên trong thành phần của nó. Điều kiện này vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    p = list(map(int, input().split()))

    if n == 1:
        print("YES")
        continue

    if n >= 5:
        print("YES")
        continue

    ok = True

    for i, x in enumerate(p, start=1):
        if (i & 1) != (x & 1):
            ok = False
            break

    print("YES" if ok else "NO")
```Việc thực hiện tuân theo đặc tính thành phần một cách trực tiếp. 

Vì`n = 3`Và`n = 4`, các thành phần chính xác là các giá trị lẻ và các giá trị chẵn. Kiểm tra xem vị trí`i`và giá trị`p[i]`thuộc cùng một thành phần tương đương với việc kiểm tra xem chúng có cùng tính chẵn lẻ hay không. 

Vì`n = 2`, việc kiểm tra tính chẵn lẻ tương tự vẫn hoạt động. Chức vụ`1`chỉ có thể chứa`1`, và vị trí`2`chỉ có thể chứa`2`. 

Sự trở lại sớm của`n ≥ 5`là sự tối ưu hóa chính. Sau khi biểu đồ được kết nối, mọi hoán vị đều có thể sắp xếp được nên không cần thực hiện thêm thao tác nào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
p = [2, 1, 4, 3]
```| Vị trí | Giá trị | Vị trí ngang bằng | Giá trị ngang bằng | Trận đấu | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | lẻ | Thậm chí | Không | 

Vị trí đầu tiên đã vi phạm điều kiện thành phần. 

Đầu ra:```
NO
```Ví dụ này chứng tỏ rằng các giá trị không thể di chuyển giữa các thành phần chẵn và lẻ. 

### Ví dụ 2 

đầu vào:```
n = 4
p = [3, 2, 1, 4]
```| Vị trí | Giá trị | Vị trí ngang bằng | Giá trị ngang bằng | Trận đấu | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | lẻ | lẻ | Có | 
| 2 | 2 | Thậm chí | Thậm chí | Có | 
| 3 | 1 | lẻ | lẻ | Có | 
| 4 | 4 | Thậm chí | Thậm chí | Có | 

Tất cả các vị trí đều thỏa mãn điều kiện thành phần. 

Đầu ra:```
YES
```Giá trị`1`Và`3`có thể trao đổi với nhau, trong khi`2`Và`4`giữ cố định. Có thể sắp xếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần quét hoán vị | 
| Không gian | O(1) | Chỉ có một vài biến được sử dụng | 

Vì tổng của tất cả`n`giá trị trên các trường hợp thử nghiệm là nhiều nhất`2 · 10^5`, một nghiệm tuyến tính dễ dàng nằm trong giới hạn. 

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
        n = int(input())
        p = list(map(int, input().split()))

        if n == 1:
            ans.append("YES")
            continue

        if n >= 5:
            ans.append("YES")
            continue

        ok = True
        for i, x in enumerate(p, start=1):
            if (i & 1) != (x & 1):
                ok = False
                break

        ans.append("YES" if ok else "NO")

    return "\n".join(ans)

# provided samples
assert run(
"""4
1
1
2
2 1
2
1 2
4
2 1 4 3
"""
) == "YES\nNO\nYES\nNO"

# minimum size
assert run(
"""1
1
1
"""
) == "YES"

# n = 2 unsorted
assert run(
"""1
2
2 1
"""
) == "NO"

# n = 4 sortable inside components
assert run(
"""1
4
3 2 1 4
"""
) == "YES"

# n >= 5 always sortable
assert run(
"""1
5
5 4 3 2 1
"""
) == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1`|`YES`| Trường hợp nhỏ nhất có thể | 
|`2 1`vì`n=2`|`NO`| Không có giao dịch hoán đổi có sẵn | 
|`3 2 1 4`vì`n=4`|`YES`| Sắp xếp lại các thành phần bên trong | 
| Hoán vị ngược cho`n=5`|`YES`| Trường hợp đồ thị kết nối | 

## Vỏ cạnh 

Hãy xem xét```
1
2
2 1
```Đồ thị có hai đỉnh cô lập vì`1 + 2 = 3`là nguyên tố. Không có trao đổi là hợp pháp. Việc kiểm tra tính chẵn lẻ thất bại ngay lập tức:```
position 1 -> value 2
odd != even
```Đầu ra của thuật toán`NO`, điều đó đúng. 

Bây giờ hãy xem xét```
1
3
3 2 1
```Các thành phần là`{1,3}`Và`{2}`. 

Việc kiểm tra cho thấy:```
position 1 -> value 3
position 2 -> value 2
position 3 -> value 1
```Mọi vị trí và giá trị đều thuộc về cùng một thành phần. Hoán đổi`1`Và`3`được phép vì tổng của chúng là`4`, do đó hoán vị có thể được sắp xếp. Đầu ra của thuật toán`YES`. 

Cuối cùng, hãy xem xét```
1
5
5 4 3 2 1
```Vì`n ≥ 5`, đồ thị được kết nối. Mọi giá trị có thể đạt đến mọi vị trí thông qua một chuỗi các giao dịch hoán đổi được phép. Thuật toán ngay lập tức trở lại`YES`, phù hợp với cấu trúc khả năng tiếp cận thực tế của biểu đồ.
