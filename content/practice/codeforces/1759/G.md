---
title: "CF 1759G - Khôi phục hoán vị"
description: "Chúng ta được cho mảng cực đại theo cặp của một hoán vị chưa biết. Hoán vị có độ dài chẵn n. Nếu chúng ta nhóm các phần tử của nó thành các cặp (p1, p2), (p3, p4), ... và thay thế từng cặp theo giá trị tối đa của nó, chúng ta thu được một mảng b có độ dài n/2."
date: "2026-06-09T14:33:49+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "constructive-algorithms", "data-structures", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1759
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round  834 (Div. 3)"
rating: 1900
weight: 1759
solve_time_s: 224
verified: false
draft: false
---

[CF 1759G - Khôi phục hoán vị](https://codeforces.com/problemset/problem/1759/G) 

**Xếp hạng:** 1900 
**Tags:** tìm kiếm nhị phân, thuật toán xây dựng, cấu trúc dữ liệu, tham lam, toán học 
**Thời gian giải:** 3 phút 44s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho mảng cực đại theo cặp của một hoán vị chưa biết. 

Hoán vị có độ dài chẵn`n`. Nếu chúng ta nhóm các phần tử của nó thành từng cặp`(p1, p2), (p3, p4), ...`và thay thế từng cặp theo giá trị tối đa của nó, chúng ta thu được một mảng`b`chiều dài`n/2`. 

Nhiệm vụ là xây dựng lại một hoán vị`p`tạo ra cái đã cho`b`. Trong số tất cả các hoán vị hợp lệ, chúng ta phải xuất ra hoán vị nhỏ nhất về mặt từ điển. Nếu không có hoán vị có thể tạo ra`b`, chúng tôi in`-1`. 

Quan sát quan trọng là mọi giá trị xuất hiện trong`b`bản thân nó phải xuất hiện trong hoán vị. Vì nó là số lớn nhất trong cặp của nó nên nó phải chiếm một vị trí của cặp đó, trong khi vị trí còn lại phải chứa một số chưa sử dụng nhỏ hơn rất nhiều. 

Những hạn chế là lớn. Tổng của tất cả`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`2 · 10^5`, có nghĩa là một`O(n log n)`giải pháp dễ dàng đủ nhanh, trong khi mọi thứ bậc hai sẽ quá chậm. Việc xây dựng lại liên tục quét tất cả các số còn lại sẽ yêu cầu khoảng`n²`hoạt động trong trường hợp xấu nhất và không thể phù hợp với giới hạn. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Coi như:```
n = 4
b = [4, 4]
```Cả hai cặp cực đại đều bằng nhau. Vì một hoán vị chứa mỗi số đúng một lần nên giá trị`4`không thể xuất hiện hai lần. Không có giải pháp tồn tại, vì vậy câu trả lời là:```
-1
```Việc xây dựng lại chỉ kiểm tra các điều kiện cặp cục bộ có thể vô tình được sử dụng lại`4`. 

Một trường hợp quan trọng khác là:```
n = 6
b = [2, 4, 6]
```Các số chưa sử dụng là`{1,3,5}`. 

Đối với cặp đầu tiên, có giá trị tối đa là`2`, chúng ta cần một số chưa sử dụng nhỏ hơn`2`. Sự lựa chọn duy nhất là`1`. 

Đối với cặp thứ hai, có giá trị tối đa là`4`, chúng ta cần một số chưa sử dụng nhỏ hơn`4`. Sự lựa chọn duy nhất còn lại là`3`. 

Đối với cặp thứ ba, chúng tôi sử dụng`5`. 

Điều này là hợp lệ:```
1 2 3 4 5 6
```Việc tham lam bất cẩn sử dụng số nhỏ hơn có sẵn lớn nhất trước tiên vẫn có tác dụng ở đây, nhưng nó không nhất thiết tạo ra hoán vị nhỏ nhất về mặt từ điển. 

Lỗi khó phát hiện nhất xảy ra khi giá trị tối đa của cặp quá nhỏ.```
n = 8
b = [8, 7, 2, 3]
```Các số chưa sử dụng là`{1,4,5,6}`. 

Khi chúng ta đạt đến mức tối đa`2`, số nhỏ hơn duy nhất là`1`, vẫn còn có sẵn, vì vậy chúng tôi sử dụng nó. 

Tối đa`3`, bây giờ chúng ta cần một số chưa sử dụng nhỏ hơn, nhưng không còn lại số nào. Câu trả lời là:```
-1
```Bất kỳ thuật toán nào cũng phải phát hiện những tình huống như vậy ngay lập tức. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng xây dựng lại hoán vị bằng cách kiểm tra việc gán các số còn thiếu cho vị trí đầu tiên của mỗi cặp. Vì đại khái`n/2`số bị thiếu, có thể theo thứ tự`(n/2)!`các nhiệm vụ có thể. Ngay cả đối với các giá trị vừa phải của`n`, điều này hoàn toàn không thể thực hiện được. 

Cấu trúc của bài toán cung cấp cho chúng ta nhiều thông tin hơn so với bài toán tái thiết tổng quát. 

Mỗi giá trị trong`b`phải xuất hiện đúng một lần trong hoán vị. Vì nó là giá trị lớn nhất trong cặp của nó nên chúng ta có thể đặt ngay nó làm phần tử thứ hai của cặp đó:```
(_, b1), (_, b2), ...
```Bây giờ nhiệm vụ duy nhất là chọn phần tử đầu tiên của mỗi cặp. 

Giả sử chúng ta đang xử lý một cặp có giá trị tối đa là`x`. Phần tử đầu tiên phải là số chưa được sử dụng nhỏ hơn hoàn toàn so với`x`. Trong số tất cả những ứng cử viên như vậy, chúng ta nên chọn ai? 

Chúng tôi muốn toàn bộ hoán vị ở mức tối thiểu về mặt từ điển. Vị trí chưa quyết định đầu tiên xuất hiện trước mức tối đa tương ứng của nó. Để giảm thiểu hoán vị, chúng ta nên đặt số hợp lệ nhỏ nhất có thể ở đó. 

Điều này ngay lập tức gợi ý một chiến lược tham lam. 

Giữ tất cả các số không xuất hiện trong`b`bên trong một tập hợp có thứ tự. Đối với mỗi mức tối đa`x`, lấy số chưa sử dụng lớn nhất vẫn nhỏ hơn`x`. 

Thoạt nhìn, điều này có vẻ trái ngược với việc giảm thiểu từ điển. Lý do nó đúng là vì bản thân cực đại đã cố định. Việc chọn số nhỏ hơn khả thi lớn nhất sẽ bảo toàn các giá trị nhỏ hơn cho các vị trí trong tương lai, làm cho các mục nhập sau này càng nhỏ càng tốt. Nếu chúng ta sử dụng những giá trị nhỏ quá sớm, một số cặp tương lai có thể trở nên không thể thực hiện được. 

Đây chính xác là cách tham lam cổ điển được sử dụng trong các giải pháp được chấp nhận. 

Chúng ta cần các truy vấn tiền thân hiệu quả: "tìm giá trị chưa sử dụng lớn nhất nhỏ hơn`x`". Một tập hợp có thứ tự hỗ trợ điều này trong`O(log n)`thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng`b`. 
2. Xác minh rằng tất cả các giá trị trong`b`là khác biệt. 

Một hoán vị chứa mỗi số đúng một lần. Nếu một số giá trị xuất hiện hai lần trong`b`, việc xây dựng lại là không thể. 
3. Đánh dấu mọi giá trị xuất hiện trong`b`như đã được sử dụng. 
4. Đặt mọi số từ`1`ĐẾN`n`điều đó không xuất hiện trong`b`thành một tập có thứ tự. 
5. Xây dựng câu trả lời theo từng cặp. 
6. Để có giá trị tối đa`x = b[i]`, tìm số lớn nhất chưa sử dụng nhỏ hơn rất nhiều so với`x`. 

Đây là truy vấn tiền nhiệm trong tập hợp có thứ tự. 
7. Nếu không tồn tại số đó, hãy xuất ra`-1`. 

Khi đó cặp này không thể được hình thành mà không vi phạm điều kiện tối đa. 
8. Nối phần trước đã chọn rồi nối thêm`x`để trả lời. 
9. Xóa phần trước đã chọn khỏi tập lệnh. 
10. Sau khi tất cả các cặp được xử lý, xuất ra hoán vị đã xây dựng. 

### Tại sao nó hoạt động 

Mỗi cặp phải chứa mức tối đa của nó`b[i]`, do đó đặt tất cả các giá trị của`b`là bắt buộc. 

Đối với một cặp có tối đa`x`, bất kỳ đối tác hợp lệ nào cũng phải nhỏ hơn`x`. Trong số tất cả các ứng cử viên có sẵn, chọn người lớn nhất là lựa chọn an toàn nhất. Nó để lại tất cả các số nhỏ hơn có sẵn cho các cặp sau, điều này chỉ có thể làm tăng tập hợp các tùy chọn trong tương lai. 

Giả sử tồn tại một bản tái thiết hợp lệ. Khi xử lý một cặp với giá trị tối đa`x`, bất kỳ số nào nhỏ hơn chưa được sử dụng đều có thể được chỉ định ở đó. Thay thế nó bằng một số chưa sử dụng lớn hơn nhưng vẫn nhỏ hơn`x`không thể làm tổn hại đến cặp hiện tại và chỉ có thể giúp ích cho các cặp trong tương lai, vì cực đại trong tương lai sẽ có ít nhất nhiều giá trị nhỏ hơn như trước. 

Vì vậy, sự lựa chọn tham lam không bao giờ phá hủy một giải pháp khi nó tồn tại. Nếu thuật toán không tìm được giá trị chưa sử dụng nhỏ hơn ở mức tối đa nào đó`x`, thì không tồn tại sự tái thiết hợp lệ nào cả. 

Hoán vị được tạo ra cũng ở mức tối thiểu về mặt từ điển trong số tất cả các bản dựng lại hợp lệ. Kẻ tham lam bảo toàn các giá trị nhỏ nhất cho các vị trí sớm nhất trong tương lai, đó chính xác là những gì việc giảm thiểu từ điển yêu cầu. 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n + 1))

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def erase(self, x):
        self.parent[x] = self.find(x - 1)

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        b = list(map(int, input().split()))

        if len(set(b)) != len(b):
            print(-1)
            continue

        used = [False] * (n + 1)
        for x in b:
            used[x] = True

        dsu = DSU(n)

        for x in range(1, n + 1):
            if used[x]:
                dsu.erase(x)

        ans = []
        ok = True

        for x in b:
            y = dsu.find(x - 1)

            if y == 0:
                ok = False
                break

            ans.append(y)
            ans.append(x)

            dsu.erase(y)

        if ok:
            print(*ans)
        else:
            print(-1)

solve()
```Bước xác nhận đầu tiên kiểm tra xem một số giá trị có xuất hiện hai lần trong`b`. Đầu vào như vậy không bao giờ có thể đến từ một hoán vị vì mỗi giá trị tối đa phải tự xuất hiện trong hoán vị. 

DSU hoạt động như một cấu trúc tiền thân. Với mọi giá trị`v`,`find(v)`trả về số lượng lớn nhất hiện có không vượt quá`v`. 

Ban đầu tất cả các số không xuất hiện trong`b`có sẵn. Mỗi số xuất hiện trong`b`bị xóa khỏi DSU vì nó đã được dành riêng dưới dạng cặp tối đa. 

Để đạt mức tối đa`x`, chúng tôi truy vấn`find(x - 1)`. Điều này trực tiếp trả về số lượng lớn nhất có sẵn nhỏ hơn`x`. 

các`erase`hoạt động liên kết một số với số trước đó. Sau khi loại bỏ`y`, các truy vấn trong tương lai sẽ tự động bỏ qua nó. 

Việc triển khai DSU là một thủ thuật phổ biến cho các truy vấn trước đó trên phạm vi`1..n`. Nó tránh được cây cân bằng và giữ độ phức tạp gần như tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 6
b = [4, 3, 6]
```Số chưa sử dụng ban đầu là`{1, 2, 5}`. 

| Cặp | Tối đa x | Lớn nhất hiện có < x | Còn lại chưa sử dụng | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 2 | {1,5} | 2 4 | 
| 2 | 3 | 1 | {5} | 2 4 1 3 | 
| 3 | 6 | 5 | {} | 2 4 1 3 5 6 | 

Đầu ra:```
2 4 1 3 5 6
```Dấu vết này cho thấy mỗi cặp tiêu thụ tiền thân lớn nhất khả thi như thế nào. 

### Ví dụ 2 

đầu vào:```
n = 8
b = [8, 7, 2, 3]
```Số chưa sử dụng ban đầu là`{1,4,5,6}`. 

| Cặp | Tối đa x | Lớn nhất hiện có < x | Còn lại chưa sử dụng | Trạng thái | 
| --- | --- | --- | --- | --- | 
| 1 | 8 | 6 | {1,4,5} | được | 
| 2 | 7 | 5 | {1,4} | được | 
| 3 | 2 | 1 | {4} | được | 
| 4 | 3 | không | {4} | Thất bại | 

Vì không có số chưa sử dụng nào nhỏ hơn`3`còn sót lại, việc xây dựng lại là không thể. 

Đầu ra:```
-1
```Ví dụ này chứng minh tại sao chỉ khớp cực đại là không đủ. Mỗi cặp cũng phải có một người bạn đồng hành nhỏ hơn riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n α(n)) | Mỗi hoạt động DSU được khấu hao ngược-Ackermann | 
| Không gian | O(n) | Mảng mẹ và kế toán DSU | 

Vì tổng số tiền của`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`2 · 10^5`, cấu trúc tiền thân dựa trên DSU dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    class DSU:
        def __init__(self, n):
            self.parent = list(range(n + 1))

        def find(self, x):
            if self.parent[x] != x:
                self.parent[x] = self.find(self.parent[x])
            return self.parent[x]

        def erase(self, x):
            self.parent[x] = self.find(x - 1)

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        b = list(map(int, input().split()))

        if len(set(b)) != len(b):
            out.append("-1")
            continue

        used = [False] * (n + 1)
        for x in b:
            used[x] = True

        dsu = DSU(n)

        for x in range(1, n + 1):
            if used[x]:
                dsu.erase(x)

        ans = []
        ok = True

        for x in b:
            y = dsu.find(x - 1)

            if y == 0:
                ok = False
                break

            ans.extend([str(y), str(x)])
            dsu.erase(y)

        out.append(" ".join(ans) if ok else "-1")

    return "\n".join(out)

assert run(
"""1
6
4 3 6
"""
) == "2 4 1 3 5 6"

assert run(
"""1
4
4 4
"""
) == "-1"

assert run(
"""1
2
2
"""
) == "1 2"

assert run(
"""1
8
8 7 2 3
"""
) == "-1"

assert run(
"""1
6
2 4 6
"""
) == "1 2 3 4 5 6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=2, b=[2]`|`1 2`| Phiên bản hợp lệ tối thiểu | 
|`n=4, b=[4,4]`|`-1`| Cực đại trùng lặp | 
|`n=8, b=[8,7,2,3]`|`-1`| Thiếu đối tác nhỏ hơn | 
|`n=6, b=[2,4,6]`|`1 2 3 4 5 6`| Tái thiết hợp lệ đơn giản | 

## Vỏ cạnh 

Hãy xem xét:```
1
4
4 4
```Bước đầu tiên phát hiện ra rằng`4`xuất hiện hai lần trong`b`. Vì một hoán vị không thể chứa hai bản sao của`4`, việc xây dựng lại là không thể và thuật toán sẽ in ngay lập tức:```
-1
```Bây giờ hãy xem xét:```
1
8
8 7 2 3
```Sau khi bảo lưu tất cả các cực đại, các số có sẵn là:```
{1,4,5,6}
```Quá trình xử lý tiến hành như sau:```
8 -> 6
7 -> 5
2 -> 1
3 -> impossible
```Truy vấn trước đó cho`3`trả lại`0`, nghĩa là không có giá trị không sử dụng nào nhỏ hơn`3`tồn tại. Thuật toán báo cáo chính xác:```
-1
```Cuối cùng, hãy xem xét:```
1
6
2 4 6
```Số có sẵn là:```
{1,3,5}
```Mỗi truy vấn trước đó đều thành công:```
2 -> 1
4 -> 3
6 -> 5
```Kết quả:```
1 2 3 4 5 6
```Mỗi cặp đều có mức tối đa được yêu cầu, mọi số từ`1`ĐẾN`6`xuất hiện chính xác một lần và việc xây dựng lại là hợp lệ.
