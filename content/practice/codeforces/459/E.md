---
title: "CF 459E - Pashmak và đồ thị"
description: "Chúng ta có một đồ thị có hướng trong đó mỗi cạnh đều có trọng số. Chúng ta muốn đường đi dài nhất có thể được đo bằng số cạnh, với một hạn chế: bất cứ khi nào chúng ta di chuyển từ cạnh này sang cạnh tiếp theo, cạnh mới phải có trọng số lớn hơn cạnh trước đó."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "sortings"]
categories: ["algorithms"]
codeforces_contest: 459
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 261 (Div. 2)"
rating: 1900
weight: 459
solve_time_s: 107
verified: true
draft: false
---

[CF 459E - Pashmak và Đồ thị](https://codeforces.com/problemset/problem/459/E) 

**Xếp hạng:** 1900 
**Thẻ:** dp, sắp xếp 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị có hướng trong đó mỗi cạnh đều có trọng số. Chúng ta muốn đường đi dài nhất có thể được đo bằng số cạnh, với một hạn chế: bất cứ khi nào chúng ta di chuyển từ cạnh này sang cạnh tiếp theo, cạnh mới phải có trọng số lớn hơn cạnh trước đó. 

Con đường không cần phải đơn giản. Các đỉnh có thể được xem lại. Chu kỳ được cho phép trong biểu đồ. Điều quan trọng duy nhất là chuỗi trọng số của các cạnh dọc theo đường đi đã chọn đang tăng lên một cách nghiêm ngặt. 

Đối với mỗi cạnh có hướng`(u → v, w)`, chúng ta có thể coi nó như một phần mở rộng có thể có của một số đường dẫn được xây dựng trước đó kết thúc tại`u`, nhưng chỉ khi cạnh cuối cùng được sử dụng trong đường dẫn đó có trọng số nhỏ hơn`w`. 

Biểu đồ có thể chứa tới`3 · 10^5`đỉnh và`3 · 10^5`các cạnh. Bất kỳ thuật toán nào khám phá các đường dẫn một cách rõ ràng đều không thể thực hiện được ngay lập tức. Thậm chí một`O(m²)`giải pháp lập trình động sẽ yêu cầu khoảng`9 · 10^10`hoạt động trong trường hợp xấu nhất. Chúng ta cần một cái gì đó gần tuyến tính hoặc`O(m log m)`. 

Các trường hợp nguy hiểm nhất đến từ các cạnh có cùng trọng lượng. 

Coi như:```
3 2
1 2 5
2 3 5
```Câu trả lời đúng là:```
1
```Hai cạnh không thể xích được vì trọng lượng phải tăng nghiêm ngặt. Một bản cập nhật DP bất cẩn được thực hiện ngay lập tức trong khi xử lý các cạnh trọng lượng`5`có thể tạo ra chiều dài không chính xác`2`. 

Một trường hợp quan trọng khác là một chu trình có trọng số tăng dần:```
3 3
1 2 1
2 3 2
3 1 3
```Câu trả lời đúng là:```
3
```Mặc dù biểu đồ chứa một chu trình, nhưng việc tăng trọng số sẽ ngăn cản đường dẫn vô hạn. Câu trả lời là hữu hạn vì mỗi cạnh tiếp theo phải có trọng số lớn hơn. 

Trường hợp tinh vi thứ ba là nhiều cạnh đến có cùng trọng số:```
3 3
1 2 1
1 3 1
2 3 2
```Câu trả lời đúng là:```
2
```Cả hai cạnh có trọng số-1 đều tạo các đường dẫn có độ dài 1. Cạnh có trọng số-2 sẽ mở rộng đường dẫn tốt nhất kết thúc ở đỉnh 2. DP phải bảo toàn giá trị tối đa cho mỗi đỉnh. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng xây dựng mọi con đường tăng trọng số hợp lệ. Bắt đầu từ mọi cạnh, chúng ta có thể đệ quy đi theo tất cả các cạnh đi ra có trọng số lớn hơn. Điều này đúng vì nó liệt kê chính xác các đường dẫn được sự cố cho phép. 

Vấn đề là số lượng đường dẫn như vậy có thể theo cấp số nhân. Ngay cả một đồ thị chỉ có một vài lựa chọn phân nhánh trên mỗi đỉnh cũng có thể tạo ra một số lượng lớn các dãy tăng dần. Với tối đa`3 · 10^5`các cạnh, việc thăm dò toàn diện là hoàn toàn không khả thi. 

Quan sát quan trọng là cấu trúc đường dẫn thực tế ít quan trọng hơn nhiều so với điểm cuối và trọng số cuối cùng được sử dụng. 

Giả sử chúng ta xử lý các cạnh theo thứ tự trọng số tăng dần. Khi xem xét một cạnh`(u → v, w)`, mọi đường dẫn hợp lệ kết thúc tại`u`có trọng lượng cạnh cuối cùng nhỏ hơn`w`có thể được mở rộng qua cạnh này. 

Cho phép`dp[x]`biểu thị độ dài đường đi tốt nhất kết thúc ở đỉnh`x`chỉ sử dụng trọng lượng đã được xử lý. 

Sau đó cạnh`(u → v, w)`có thể tạo một đường dẫn có độ dài:```
dp[u] + 1
```Sự phức tạp là trọng lượng bằng nhau. 

Nếu hai cạnh có cùng trọng số thì chúng không được ảnh hưởng lẫn nhau vì đường đi yêu cầu tăng nghiêm ngặt. Xử lý từng cái một và cập nhật`dp`ngay lập tức sẽ vô tình cho phép xâu chuỗi các cạnh có trọng lượng bằng nhau. 

Cách khắc phục là xử lý tất cả các cạnh có cùng trọng lượng với nhau. 

Đối với mọi cạnh của trọng lượng`w`, trước tiên chúng tôi tính giá trị ứng cử viên của nó bằng cách sử dụng trạng thái DP tồn tại trước trọng số`w`bắt đầu. Chỉ sau mỗi cạnh của trọng lượng`w`đã được đánh giá để chúng tôi cam kết cập nhật. 

Điều này tạo ra sự tách biệt rõ ràng giữa trọng lượng nhỏ hơn và trọng lượng hiện tại. 

Sau khi sắp xếp các cạnh theo trọng lượng, mỗi cạnh được xử lý một lần, đưa ra kết quả`O(m log m)`giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(m log m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các cạnh và lưu trữ chúng dưới dạng`(weight, from, to)`. 
2. Sắp xếp các cạnh theo trọng số. 

Việc sắp xếp đảm bảo rằng khi chúng tôi xử lý trọng lượng`w`, tất cả các trọng số nhỏ hơn đã đóng góp vào trạng thái DP. 
3. Duy trì một mảng`dp[v]`.`dp[v]`biểu thị độ dài tối đa của đường dẫn có trọng số tăng dần hợp lệ kết thúc ở đỉnh`v`, chỉ sử dụng các trọng số nhỏ hơn nhóm hiện đang được xử lý. 
4. Xử lý các cạnh được nhóm theo trọng số bằng nhau. 

Tất cả các cạnh có cùng trọng lượng phải được xử lý cùng nhau để chúng không thể kéo dài lẫn nhau. 
5. Cho mọi cạnh`(u → v)`ở nhóm cân nặng hiện tại, hãy tính:```
candidate = dp[u] + 1
```Đây là con đường tốt nhất kết thúc tại`u`sử dụng trọng số nhỏ hơn, được mở rộng bởi cạnh hiện tại. 
6. Lưu trữ tạm thời các cập nhật ứng viên này thay vì ghi chúng trực tiếp vào`dp`. 

Điều này ngăn cản các cạnh có cùng trọng lượng ảnh hưởng lẫn nhau. 
7. Sau khi mỗi cạnh trong nhóm đã được đánh giá, hãy áp dụng:```
dp[v] = max(dp[v], candidate)
```cho tất cả các bản cập nhật được lưu trữ. 
8. Tiếp tục với nhóm cân nặng tiếp theo. 
9. Câu trả lời là giá trị lớn nhất có trong`dp`. 

### Tại sao nó hoạt động 

Điều bất biến là trước khi xử lý một nhóm trọng số`w`,`dp[v]`bằng độ dài tối đa của đường dẫn hợp lệ kết thúc tại`v`có trọng lượng cạnh cuối cùng nhỏ hơn`w`. 

Khi chúng ta đánh giá một cạnh`(u → v, w)`, giá trị`dp[u] + 1`đại diện cho mọi đường dẫn hợp lệ có thể đi trước cạnh này một cách hợp pháp, bởi vì tất cả các đường dẫn như vậy đều kết thúc với trọng số nhỏ hơn. 

Cập nhật bị trì hoãn cho đến khi toàn bộ nhóm hoàn thành. Kết quả là, không có cạnh trọng lượng`w`có thể sử dụng một cạnh khác của trọng lượng`w`như người tiền nhiệm của nó. Điều này hoàn toàn phù hợp với yêu cầu bất bình đẳng nghiêm ngặt. 

Bằng cách quy nạp theo trọng số tăng dần, mọi đường dẫn hợp lệ đều được biểu diễn và mọi đường dẫn được biểu diễn đều hợp lệ. Do đó giá trị DP tối đa cuối cùng là độ dài của đường tăng trọng số dài nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    edges = []
    for _ in range(m):
        u, v, w = map(int, input().split())
        edges.append((w, u, v))

    edges.sort()

    dp = [0] * (n + 1)

    i = 0
    while i < m:
        j = i
        updates = []

        while j < m and edges[j][0] == edges[i][0]:
            _, u, v = edges[j]
            updates.append((v, dp[u] + 1))
            j += 1

        for v, val in updates:
            if val > dp[v]:
                dp[v] = val

        i = j

    print(max(dp))

if __name__ == "__main__":
    solve()
```Bước đầu tiên lưu trữ mọi cạnh cùng với trọng số của nó và sắp xếp danh sách. Điều này mang lại thứ tự xử lý tự nhiên từ trọng lượng nhỏ hơn đến trọng số lớn hơn.`dp[v]`lưu trữ độ dài đường đi tốt nhất kết thúc ở đỉnh`v`. Ban đầu mọi giá trị đều bằng 0 vì không có cạnh nào được sử dụng. 

Chi tiết triển khai quan trọng nhất là tạm thời`updates`mảng. Khi kiểm tra một nhóm cân nặng, chúng tôi không bao giờ sửa đổi`dp`trực tiếp. Mọi giá trị ứng viên đều được tính từ trạng thái DP cũ. Chỉ sau khi toàn bộ nhóm đã được xử lý thì các bản cập nhật mới được cam kết. 

Nếu không có sự tách biệt này, hai cạnh có trọng số bằng nhau có thể kéo dài lẫn nhau và vi phạm điều kiện tăng nghiêm ngặt. 

Câu trả lời cuối cùng đơn giản là độ dài đường dẫn lớn nhất được lưu trữ ở bất kỳ đâu trong mảng DP. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3
1 2 1
2 3 1
3 1 1
```Sau khi sắp xếp, tất cả các cạnh đều thuộc cùng một nhóm trọng số. 

| Cạnh | dp trước | Ứng viên | Cập nhật được lưu trữ | 
| --- | --- | --- | --- | 
| 1→2 | dp[1]=0 | 1 | (2,1) | 
| 2→3 | dp[2]=0 | 1 | (3,1) | 
| 3→1 | dp[3]=0 | 1 | (1,1) | 

Sau khi áp dụng các bản cập nhật: 

| Đỉnh | dp | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 1 | 

Trả lời:```
1
```Ví dụ này chứng tỏ tại sao các cạnh có trọng số bằng nhau không thể tương tác. Mặc dù chúng tạo thành một chu trình, nhưng không cái nào có thể mở rộng cái khác vì tất cả các trọng số đều giống hệt nhau. 

### Mẫu 2 

Hãy xem xét:```
3 3
1 2 1
2 3 2
1 3 3
```Xử lý trọng lượng 1: 

| Cạnh | dp trước | Ứng viên | 
| --- | --- | --- | 
| 1→2 | 0 | 1 | 

Sau khi cập nhật: 

| Đỉnh | dp | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 0 | 

Xử lý trọng lượng 2: 

| Cạnh | dp trước | Ứng viên | 
| --- | --- | --- | 
| 2→3 | 1 | 2 | 

Sau khi cập nhật: 

| Đỉnh | dp | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 2 | 

Xử lý trọng lượng 3: 

| Cạnh | dp trước | Ứng viên | 
| --- | --- | --- | 
| 1→3 | 0 | 1 | 

Sau khi cập nhật: 

| Đỉnh | dp | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 2 | 

Trả lời:```
2
```Dấu vết này cho thấy thuật toán luôn giữ đường đi tốt nhất kết thúc ở mỗi đỉnh. Cạnh trực tiếp của vật nặng 3 yếu hơn sợi xích qua vật nặng 1 và 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log m) | Sắp xếp chiếm ưu thế, xử lý DP là tuyến tính | 
| Không gian | O(n + m) | Mảng DP cộng với các cạnh được lưu trữ | 

Với nhiều nhất`3 · 10^5`các cạnh, việc sắp xếp đòi hỏi khoảng`m log m`hoạt động phù hợp một cách thoải mái trong giới hạn. Việc sử dụng bộ nhớ cũng dưới 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m = map(int, input().split())

    edges = []
    for _ in range(m):
        u, v, w = map(int, input().split())
        edges.append((w, u, v))

    edges.sort()

    dp = [0] * (n + 1)

    i = 0
    while i < m:
        j = i
        updates = []

        while j < m and edges[j][0] == edges[i][0]:
            _, u, v = edges[j]
            updates.append((v, dp[u] + 1))
            j += 1

        for v, val in updates:
            dp[v] = max(dp[v], val)

        i = j

    return str(max(dp))

# provided sample
assert run(
    "3 3\n"
    "1 2 1\n"
    "2 3 1\n"
    "3 1 1\n"
) == "1", "sample 1"

# minimum graph
assert run(
    "2 1\n"
    "1 2 5\n"
) == "1", "single edge"

# increasing chain
assert run(
    "4 3\n"
    "1 2 1\n"
    "2 3 2\n"
    "3 4 3\n"
) == "3", "full chain"

# all equal weights
assert run(
    "4 3\n"
    "1 2 7\n"
    "2 3 7\n"
    "3 4 7\n"
) == "1", "equal weights cannot chain"

# equal-weight trap
assert run(
    "3 2\n"
    "1 2 5\n"
    "2 3 5\n"
) == "1", "must not chain same weight"

# cycle with increasing weights
assert run(
    "3 3\n"
    "1 2 1\n"
    "2 3 2\n"
    "3 1 3\n"
) == "3", "cycle still finite"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cạnh đơn | 1 | Đồ thị không tầm thường tối thiểu | 
| Tăng chuỗi | 3 | Xây dựng đường dẫn bình thường | 
| Tất cả trọng lượng bằng nhau | 1 | Yêu cầu bất bình đẳng nghiêm ngặt | 
| Bẫy trọng lượng bằng nhau | 1 | Cập nhật bị trì hoãn là cần thiết | 
| Chu kỳ tăng | 3 | Chu kỳ không phá vỡ DP | 

## Vỏ cạnh 

Xét hai cạnh có trọng số bằng nhau:```
3 2
1 2 5
2 3 5
```Cả hai cạnh đều thuộc cùng một nhóm trọng lượng. Trong quá trình đánh giá, thuật toán tính toán các ứng viên sử dụng trạng thái DP cũ:```
(2,1)
(3,1)
```Chỉ sau khi cả hai ứng cử viên được thu thập thì các bản cập nhật mới được áp dụng. Cạnh thứ hai không bao giờ thấy tác dụng của cạnh thứ nhất nên đáp án vẫn là`1`, điều đó đúng. 

Bây giờ hãy xem xét một chu kỳ:```
3 3
1 2 1
2 3 2
3 1 3
```Các nhóm trọng số được xử lý theo thứ tự`1`,`2`,`3`. Các giá trị DP trở thành:```
dp[2] = 1
dp[3] = 2
dp[1] = 3
```Đầu ra của thuật toán`3`. Cho phép xem lại một đỉnh, nhưng việc tăng trọng số sẽ ngăn cản sự tăng trưởng vô hạn. 

Cuối cùng, xét nhiều đường đi hội tụ về cùng một đỉnh:```
4 4
1 3 1
2 3 2
3 4 3
1 4 10
```Sau khi xử lý trọng lượng`1`Và`2`, đỉnh`3`lưu trữ độ dài đường dẫn tốt nhất có thể. Khi cân nặng`3`được xử lý, cạnh`3 → 4`mở rộng con đường mạnh nhất sẵn có. Bản cập nhật DP sử dụng`max`, vì vậy những lựa chọn thay thế yếu hơn sẽ không bao giờ ghi đè lên những lựa chọn tốt hơn. Câu trả lời kết quả là`2`, tương ứng với một trong hai`1→3→4`hoặc`2→3→4`.
