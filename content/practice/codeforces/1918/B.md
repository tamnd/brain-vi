---
title: "CF 1918B - Giảm thiểu sự đảo ngược"
description: "Cho hai hoán vị a và b có cùng độ dài. Hoạt động được phép là không bình thường: bất cứ khi nào chúng ta hoán đổi vị trí i và j, chúng ta phải thực hiện cùng một thao tác hoán đổi trong cả hai mảng. Điều này có nghĩa là chúng ta không bao giờ có thể tách a[k] khỏi b[k]."
date: "2026-06-08T19:42:13+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1918
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 922 (Div. 2)"
rating: 900
weight: 1918
solve_time_s: 172
verified: false
draft: false
---

[CF 1918B - Giảm thiểu sự đảo ngược](https://codeforces.com/problemset/problem/1918/B) 

**Xếp hạng:** 900 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, tham lam, triển khai, sắp xếp 
**Thời gian giải:** 2m 52s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai hoán vị`a`Và`b`có cùng độ dài. Hoạt động được phép là không bình thường: bất cứ khi nào chúng ta hoán đổi vị trí`i`Và`j`, chúng ta phải thực hiện cùng một thao tác hoán đổi trong cả hai mảng cùng một lúc. 

Điều này có nghĩa là chúng ta không bao giờ có thể tách rời`a[k]`từ`b[k]`. Mọi vị trí hoạt động giống như một đối tượng được ghép nối`(a[k], b[k])`. Điều duy nhất chúng tôi được phép làm là sắp xếp lại các cặp này. 

Sau khi sắp xếp lại các cặp, chúng ta thu được hoán vị mới`a'`Và`b'`. Mục tiêu của chúng tôi là kiếm được tổng`inversions(a') + inversions(b')`càng nhỏ càng tốt. 

Quan sát quan trọng là thao tác không sửa đổi bất kỳ cặp nào. Nó chỉ thay đổi thứ tự xuất hiện của các cặp. 

Những hạn chế là lớn. Tổng của tất cả`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`2·10^5`, vì vậy một giải pháp xung quanh`O(n log n)`mỗi trường hợp thử nghiệm là đủ nhanh. Các thuật toán bậc hai sẽ yêu cầu khoảng`4·10^10`trong trường hợp xấu nhất là hoàn toàn không thể thực hiện được. 

Một điểm tinh tế là chúng ta không cố gắng giảm thiểu nghịch đảo trong một hoán vị một cách độc lập. Bất kỳ sự sắp xếp lại nào đều ảnh hưởng đến cả hai mảng cùng một lúc. 

Coi như:```
a = [1, 2]
b = [2, 1]
```Các cặp là:```
(1,2), (2,1)
```Nếu chúng ta giữ trật tự này,`a`có`0`sự đảo ngược và`b`có`1`. 

Nếu chúng ta đảo ngược thứ tự,`a`có`1`đảo ngược và`b`có`0`. 

Dù bằng cách nào thì tổng số là`1`. Một nỗ lực tham lam chỉ sắp xếp một hoán vị sẽ bỏ lỡ mục tiêu thực sự. 

Một sai lầm dễ mắc phải khác là nghĩ rằng chúng ta có thể tự do sắp xếp cả hai mảng. Ví dụ:```
a = [1, 2]
b = [2, 1]
```Không có cách nào để sắp xếp cả hai mảng cùng một lúc vì bản thân các cặp đã cố định. Sắp xếp lại các cặp không thể chuyển đổi`(1,2)`Và`(2,1)`vào trong`(1,1)`Và`(2,2)`. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ xem mọi vị trí như một cặp`(a[i], b[i])`và thử tất cả các thứ tự có thể có của các cặp này. Vì có`n!`hoán vị, điều này trở nên không thể ngay cả đối với các giá trị vừa phải của`n`. 

Để hiểu những gì phải được tối ưu hóa, hãy xem xét hai cặp:```
(x1, y1)
(x2, y2)
```Giả sử hai cặp này xuất hiện theo thứ tự này. 

Đóng góp của họ vào tổng số lần đảo ngược chỉ đến từ việc so sánh giữa hai vị trí. 

Có bốn khả năng. 

Nếu như`x1 < x2`, chúng không tạo ra sự đảo ngược trong`a`. 

Nếu như`x1 > x2`, họ tạo ra một sự đảo ngược trong`a`. 

Tương tự cho`y1`Và`y2`TRONG`b`. 

Vì vậy, sự đóng góp của cặp vị trí này là:```
[x1 > x2] + [y1 > y2]
```Bây giờ giả sử chúng ta hoán đổi thứ tự của hai cặp. Sự đóng góp trở thành:```
[x2 > x1] + [y2 > y1]
```Hãy xem xét thứ tự tương đối của hai cặp. 

Nếu cả hai tọa độ đều đồng ý:```
x1 < x2 and y1 < y2
```sau đó đặt cặp 1 trước cặp 2 đóng góp`0`, trong khi đảo ngược chúng góp phần`2`. 

Rõ ràng cặp 1 nên đến trước. 

Nếu cả hai tọa độ không đồng ý:```
x1 < x2 and y1 > y2
```sau đó một sự đảo ngược xuất hiện bất kể cặp nào đến trước. 

Sự đóng góp luôn`1`. 

Đây là quan sát quan trọng. 

Đối với mỗi cặp đối tượng:```
(a[i], b[i])
(a[j], b[j])
```chỉ có hai trường hợp. 

Khi thứ tự của chúng đồng ý ở cả hai tọa độ, thì cách sắp xếp tương đối này sẽ tốt hơn cách sắp xếp tương đối kia. 

Khi mệnh lệnh của họ không đồng ý, cả hai thỏa thuận đều đưa ra mức đóng góp như nhau. 

Vì vậy, mỗi cặp có tọa độ trùng nhau sẽ áp đặt một thứ tự ưu tiên:```
a[i] < a[j] and b[i] < b[j]
```có nghĩa là cặp`i`sẽ xuất hiện trước cặp`j`. 

Bởi vì`a`Và`b`là những hoán vị, những ưu tiên này không thể tạo thành chu trình. Họ xác định một phần trật tự. 

Phần mở rộng tuyến tính tự nhiên của thứ tự từng phần này thu được bằng cách sắp xếp tất cả các cặp theo`a`. 

Sau khi sắp xếp theo`a`, bất cứ khi nào```
a[i] < a[j]
```giữ, cặp có nhỏ hơn`a`là sớm hơn. 

Đối với những cặp có`b`các giá trị cũng được sắp xếp theo cách tương tự, chúng tôi đạt được sự đóng góp tốt hơn`0`. 

Đối với những cặp có`b`các giá trị được sắp xếp trái ngược nhau, phần đóng góp được cố định ở`1`dù sao đi nữa, vì vậy vị trí tương đối của họ không thành vấn đề. 

Do đó sắp xếp theo`a`đạt được tổng số lần đảo ngược tối thiểu có thể. 

### So sánh độ phức tạp 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n²) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách các cặp`(a[i], b[i])`. 
2. Sắp xếp các cặp theo tọa độ đầu tiên`a[i]`. 

Từ`a`là một hoán vị, tất cả tọa độ đầu tiên đều khác nhau nên thứ tự được xác định duy nhất. 
3. Sau khi sắp xếp, xây dựng lại mảng. 

Tọa độ đầu tiên của các cặp được sắp xếp trở thành hoán vị mới`a'`. 

Tọa độ thứ hai trở thành hoán vị mới`b'`. 
4. Xuất các mảng được xây dựng lại. 

### Tại sao nó hoạt động 

Lấy hai cặp bất kỳ. 

Nếu đơn đặt hàng của họ trong`a`Và`b`đồng ý, một sự sắp xếp tương đối góp phần`0`sự đảo ngược và sự sắp xếp ngược lại góp phần`2`. Bất kỳ giải pháp tối ưu nào cũng phải đặt chúng theo thứ tự ưu tiên. 

Sắp xếp theo`a`tôn trọng mọi mối quan hệ ưu tiên như vậy. 

Nếu đơn đặt hàng của họ trong`a`Và`b`không đồng ý, cả hai sự sắp xếp có thể đều đóng góp chính xác`1`, vì vậy không có lựa chọn nào có thể cải thiện hoặc làm xấu đi câu trả lời. 

Do đó, mỗi cặp đối tượng đều đóng góp số lượng tối thiểu có thể đạt được. Vì tổng số nghịch đảo là tổng của tất cả các cặp không có thứ tự nên sự sắp xếp thu được là tối ưu toàn cục. 

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

        pairs = list(zip(a, b))
        pairs.sort()  # sort by a

        out.append(" ".join(str(x) for x, _ in pairs))
        out.append(" ".join(str(y) for _, y in pairs))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo sau bằng chứng. 

Bước lập mô hình quan trọng là coi mỗi vị trí là một cặp cố định. Vì thao tác luôn hoán đổi đồng thời cả hai mảng nên cặp này không bao giờ có thể bị tách rời. 

Việc sắp xếp danh sách các cặp sẽ tự động sắp xếp theo tọa độ đầu tiên vì Python so sánh các bộ dữ liệu theo từ điển. Từ`a`là một hoán vị, tất cả tọa độ đầu tiên đều khác biệt nên không cần phải lo lắng về việc xử lý ràng buộc. 

Sau khi sắp xếp, chúng ta chỉ cần giải nén các cặp thành hai mảng và in chúng. 

Không cần đếm ngược. Bằng chứng cho thấy thứ tự sắp xếp đã là tối ưu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = [3, 1, 2]
b = [3, 1, 2]
```Cặp ban đầu: 

| Vị trí | Cặp | 
| --- | --- | 
| 1 | (3,3) | 
| 2 | (1,1) | 
| 3 | (2,2) | 

Sau khi sắp xếp theo`a`: 

| Vị trí được sắp xếp | Cặp | 
| --- | --- | 
| 1 | (1,1) | 
| 2 | (2,2) | 
| 3 | (3,3) | 

Kết quả:```
a' = [1,2,3]
b' = [1,2,3]
```Cả hai hoán vị đều được sắp xếp, do đó tổng số lần đảo ngược là`0`. 

Ví dụ này cho thấy kịch bản tốt nhất có thể xảy ra trong đó cả hai tọa độ đều tạo ra thứ tự giống hệt nhau. 

### Ví dụ 2 

đầu vào:```
a = [1, 2, 3, 4, 5]
b = [5, 4, 3, 2, 1]
```Cặp ban đầu: 

| Vị trí | Cặp | 
| --- | --- | 
| 1 | (1,5) | 
| 2 | (2,4) | 
| 3 | (3,3) | 
| 4 | (4,2) | 
| 5 | (5,1) | 

Sắp xếp theo`a`không có gì thay đổi: 

| Vị trí được sắp xếp | Cặp | 
| --- | --- | 
| 1 | (1,5) | 
| 2 | (2,4) | 
| 3 | (3,3) | 
| 4 | (4,2) | 
| 5 | (5,1) | 

Kết quả:```
a' = [1,2,3,4,5]
b' = [5,4,3,2,1]
```Mỗi cặp vị trí không đồng ý giữa`a`Và`b`, do đó, mỗi cặp không có thứ tự đóng góp chính xác một phép nghịch đảo bất kể các cặp được sắp xếp như thế nào. Không sắp xếp lại có thể cải thiện câu trả lời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp`n`cặp thống trị | 
| Không gian | O(n) | Lưu trữ danh sách cặp | 

Tổng của tất cả`n`nhiều nhất là`2·10^5`, vậy tổng công việc là khoảng`2·10^5 log(2·10^5)`, dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def solve():
        input = sys.stdin.readline
        t = int(input())
        out = []

        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))
            b = list(map(int, input().split()))

            pairs = list(zip(a, b))
            pairs.sort()

            out.append(" ".join(str(x) for x, _ in pairs))
            out.append(" ".join(str(y) for _, y in pairs))

        return "\n".join(out)

    return solve()

# sample 1
assert run(
"""1
3
3 1 2
3 1 2
"""
) == \
"""1 2 3
1 2 3"""

# minimum size
assert run(
"""1
1
1
1
"""
) == \
"""1
1"""

# already sorted a
assert run(
"""1
2
1 2
2 1
"""
) == \
"""1 2
2 1"""

# reverse permutation
assert run(
"""1
5
5 4 3 2 1
1 2 3 4 5
"""
) == \
"""1 2 3 4 5
5 4 3 2 1"""

# random small case
assert run(
"""1
4
2 4 1 3
3 1 4 2
"""
) == \
"""1 2 3 4
4 3 2 1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n = 1`| Mảng giống nhau | Trường hợp hợp lệ nhỏ nhất | 
|`[1,2]`,`[2,1]`| Không thay đổi sau khi sắp xếp theo`a`| Trường hợp đặt hàng ngược lại | 
| Đảo ngược`a`| Đã sắp xếp`a`sau khi tái thiết | Sắp xếp lại cặp đúng | 
| Hoán vị ngẫu nhiên | Sắp xếp cặp thích hợp | Tính đúng đắn chung | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1
1
1
1
```Chỉ có một cặp`(1,1)`. Sắp xếp khiến nó không thay đổi. Cả hai số lần đảo ngược đều bằng 0. Thuật toán xử lý chính xác việc không có bất kỳ so sánh nào. 

Hãy xem xét các thứ tự hoàn toàn ngược lại:```
a = [1,2,3]
b = [3,2,1]
```Các cặp là:```
(1,3), (2,2), (3,1)
```Sắp xếp theo`a`không có gì thay đổi Mỗi cặp không có thứ tự không đồng ý giữa hai tọa độ, vì vậy mỗi cặp đóng góp chính xác một nghịch đảo bất kể sự sắp xếp. Thuật toán đạt được mức tối ưu vì không có sự sắp xếp nào có thể làm tốt hơn. 

Hãy xem xét các hoán vị giống hệt nhau:```
a = [3,1,2]
b = [3,1,2]
```Mọi so sánh cặp đều đồng ý ở cả hai tọa độ. Sắp xếp theo`a`sản xuất:```
a' = [1,2,3]
b' = [1,2,3]
```Mọi nghịch đảo đều biến mất. Điều này chứng tỏ trường hợp mọi thứ tự ưu tiên có thể được đáp ứng đồng thời. 

Hãy xem xét một ví dụ hỗn hợp:```
a = [2,1,3]
b = [1,3,2]
```Cặp:```
(2,1), (1,3), (3,2)
```Sau khi sắp xếp theo`a`:```
(1,3), (2,1), (3,2)
```cho đi```
a' = [1,2,3]
b' = [3,1,2]
```Các cặp có thứ tự tọa độ đồng ý được đặt ở vị trí tối ưu, trong khi các cặp không đồng ý sẽ đóng góp một số tiền cố định bất kể thế nào. Đây chính xác là cấu trúc được sử dụng trong chứng minh tính đúng đắn.
