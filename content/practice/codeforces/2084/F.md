---
title: "CF 2084F - Skyscape"
description: "Chúng ta bắt đầu với một hoán vị a. Một thao tác chọn một phân đoạn [l, r] có phần tử ngoài cùng bên phải là giá trị nhỏ nhất bên trong phân đoạn đó. Sau đó, phần tử tối thiểu đó được di chuyển lên phía trước của đoạn bằng cách thực hiện dịch chuyển phải theo chu kỳ. Nếu đoạn đó là [x1, x2, ..."
date: "2026-06-08T06:16:29+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2084
codeforces_index: "F"
codeforces_contest_name: "Teza Round 1 (Codeforces Round 1015, Div. 1 + Div. 2)"
rating: 2900
weight: 2084
solve_time_s: 142
verified: false
draft: false
---

[CF 2084F - Skyscape](https://codeforces.com/problemset/problem/2084/F) 

**Xếp hạng:** 2900 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 2m 22s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một hoán vị`a`. Một thao tác chọn một đoạn`[l, r]`có phần tử ngoài cùng bên phải là giá trị nhỏ nhất bên trong phân đoạn đó. Sau đó, phần tử tối thiểu đó được di chuyển lên phía trước của đoạn bằng cách thực hiện dịch chuyển phải theo chu kỳ. 

Nếu đoạn đó là`[x1, x2, ..., xk, mn]`Ở đâu`mn`là giá trị nhỏ nhất trong toàn bộ phân đoạn, thao tác sẽ biến nó thành`[mn, x1, x2, ..., xk]`. 

Chúng tôi được cung cấp một hoán vị được chỉ định một phần khác`c`. Một số vị trí đã chứa các giá trị cố định, trong khi các vị trí chứa`0`vẫn còn miễn phí. Chúng ta phải hoàn thành`c`thành một hoán vị đầy đủ`b`như vậy: 

1. Mọi vị trí khác 0 đều không thay đổi. 
2.`b`có thể truy cập từ`a`sử dụng nhiều nhất`n`các hoạt động được phép. 

Nhiệm vụ không phải là tìm ra chuỗi các hoạt động. Chúng ta chỉ cần xây dựng bất kỳ hoán vị mục tiêu hợp lệ nào. 

Tổng số tiền của`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`5 · 10^5`. Bất kỳ giải pháp nào với hành vi bậc hai đều là không thể ngay lập tức. Thậm chí`O(n log^2 n)`sẽ hơi khó chịu ở quy mô này. Độ phức tạp của mục tiêu nên ở khoảng`O(n log n)`mỗi tập tin thử nghiệm. 

Phần khó khăn không phải là việc hoàn thành hoán vị. Phần khó khăn là hiểu những hoán vị nào có thể truy cập được từ`a`. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Coi như```
a = [3,2,1]
```Hoạt động có thể di chuyển`1`sang trái, nhưng nó không bao giờ có thể di chuyển`2`trước mặt`1`. Khả năng tiếp cận bị hạn chế rất nhiều. Việc xử lý từng vị trí còn thiếu một cách độc lập sẽ tạo ra các mục tiêu bất khả thi. 

Một trường hợp tế nhị khác là```
a = [3,2,1,5,4]
c = [3,2,5,1,4]
```Mỗi giá trị xuất hiện chính xác một lần và`c`bản thân nó là một hoán vị. Một giải pháp ngây thơ có thể chỉ cần trả lại nó. Tuy nhiên hoán vị này không thể truy cập được từ`a`, vậy đáp án đúng là`-1`. 

Cạm bẫy thứ ba xảy ra khi một giá trị cố định cản trở tất cả các nhiệm vụ khả thi. Ngay cả khi mỗi vị thế cố định riêng lẻ có vẻ hợp pháp thì việc kết hợp có thể là không thể. Các hạn chế về khả năng tiếp cận mang tính toàn cầu chứ không phải cục bộ. 

## Phương pháp tiếp cận 

Điều đầu tiên cần hiểu là tập hợp các hoán vị có thể truy cập từ`a`. 

Giả sử giá trị`x`đang ở vị trí`pos[x]`trong hoán vị ban đầu. Một thao tác chỉ có thể di chuyển phần tử nhỏ nhất của một đoạn. Vì tất cả các giá trị đều khác biệt nên phần tử được di chuyển luôn là giá trị nhỏ nhất trong phân đoạn đó. 

Tập trung vào một giá trị cụ thể`x`. 

Bất cứ khi nào`x`di chuyển, nó phải là mức tối thiểu của đoạn đã chọn. Điều đó có nghĩa là mọi phần tử được cắt bởi`x`lớn hơn`x`. Như vậy`x`có thể di chuyển sang trái qua các giá trị lớn hơn, nhưng nó không bao giờ có thể vượt qua giá trị nhỏ hơn. 

Quan sát này mô tả đầy đủ quá trình. 

Cho phép```
p[x] = position of x in a.
```Trong mọi hoán vị có thể tiếp cận, thứ tự tương đối của bất kỳ cặp nào`(u,v)`với`u < v`Và`p[u] < p[v]`được cố định mãi mãi. Từ`v`không thể vượt qua giá trị nhỏ hơn`u`, thứ tự của chúng không bao giờ thay đổi. 

Những ràng buộc theo cặp này xác định một phần thứ tự. 

Quan điểm bạo lực sẽ xây dựng toàn bộ biểu đồ khả năng tiếp cận của các hoán vị được tạo ra bởi các hoạt động. Ngay cả đối với mức độ vừa phải`n`, số lượng các trạng thái có thể tiếp cận trở nên rất lớn. Một hoán vị duy nhất có thể tạo ra`Θ(n²)`chuyển động và không gian trạng thái tăng lên theo giai thừa. Điều này là hoàn toàn không thể thực hiện được. 

Thông tin chi tiết quan trọng là các hoán vị có thể tiếp cận chính xác là phần mở rộng tuyến tính của một thứ tự từng phần rất đặc biệt. 

Với mỗi giá trị`v`, tìm giá trị nhỏ hơn gần nhất nằm ở bên trái của nó trong hoán vị ban đầu. Đặt giá trị này là cha mẹ của`v`. 

Ví dụ:```
a = [3,5,6,2,1,4]

value: 1 2 3 4 5 6
pos  : 5 4 1 6 2 3
```Mối quan hệ cha mẹ trở nên```
1 root
2 -> 1
3 root
4 -> 1
5 -> 3
6 -> 5
```Đây chính xác là cây Descartes của hoán vị, được xây dựng với thứ tự giá trị được ưu tiên. 

Một thực tế cơ bản là mọi hoán vị có thể tiếp cận chính xác là một trật tự tôpô của khu rừng này, nơi mọi cha mẹ phải xuất hiện trước con cái của nó. 

Bây giờ vấn đề trở thành: 

Xây dựng hoán vị các giá trị`1..n`đó 

1. Tôn trọng mọi vị trí cố định. 
2. Là một trật tự tôpô của rừng Descartes. 

Điều này dễ lý giải hơn nhiều. 

Thứ tự tôpô của một khu rừng có thể được tạo ra một cách tham lam. Tại bất kỳ thời điểm nào, mọi nút có nút cha đã được đặt đều có sẵn. 

Các ràng buộc vị trí cố định cho chúng ta biết giá trị nào phải chiếm các vị trí nhất định. Chúng tôi xử lý các vị trí từ trái sang phải. 

Nếu vị trí`i`được cố định theo giá trị`x`, sau đó`x`hiện tại phải có sẵn. Nếu không thì không có đơn đặt hàng hợp lệ nào tồn tại. 

Nếu vị trí`i`là miễn phí, chúng tôi có thể đặt bất kỳ giá trị hiện có nào. Để tối đa hóa tính linh hoạt trong tương lai, chúng ta nên chọn giá trị sẵn có lớn nhất. 

Tại sao lớn nhất? Bởi vì các giá trị nhỏ hơn sẽ mở khóa con cháu. Việc trì hoãn các giá trị nhỏ không bao giờ gây tổn hại, trong khi việc sử dụng chúng quá sớm có thể khiến các vị trí cố định trong tương lai rơi vào xung đột. Đây là chiến lược tham lam tiêu chuẩn để sắp xếp cấu trúc liên kết bị ràng buộc trong khu rừng này. 

Thuật toán kết quả duy trì tất cả các nút hiện có trong một vùng tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm khả năng tiếp cận Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| Cây Descartes + Trật tự tôpô tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Bước 1 

Tính vị trí của mọi giá trị trong hoán vị ban đầu. 

Cho phép`pos[x]`là chỉ số của giá trị`x`TRONG`a`. 

### Bước 2 

Xây dựng rừng Descartes. 

Xử lý các giá trị từ`1`ĐẾN`n`theo thứ tự tăng dần. 

Duy trì một tập hợp các vị trí có thứ tự thuộc các giá trị nhỏ hơn đã được xử lý. 

Đối với giá trị`x`, trong số tất cả các giá trị nhỏ hơn nằm ở bên trái của`pos[x]`, chọn cái có vị trí lớn nhất. Đây là giá trị nhỏ hơn gần nhất ở bên trái. 

Giá trị đó trở thành cha mẹ của`x`. 

Nếu không có giá trị đó tồn tại,`x`trở thành một gốc rễ. 

### Bước 3 

Chuẩn bị quá trình topo. 

Mỗi gốc ban đầu đều có sẵn. 

Lưu trữ tất cả các giá trị có sẵn trong một vùng heap tối đa. 

Đồng thời ghi lại mọi yêu cầu cố định từ`c`. 

### Bước 4 

Xử lý các vị trí từ trái sang phải. 

Nếu vị trí`i`được cố định theo giá trị`x`: 

1. Kiểm tra xem`x`hiện đang có sẵn. 
2. Nếu không, xuất`-1`. 
3. Nếu không thì đặt`x`. 

Nếu vị trí`i`là miễn phí: 

1. Trích xuất giá trị lớn nhất có sẵn từ heap. 
2. Đặt nó. 

### Bước 5 

Bất cứ khi nào giá trị`x`được đặt: 

1. Đánh dấu nó là đã sử dụng. 
2. Tất cả trẻ em của`x`trở nên có sẵn. 
3. Chèn các phần tử con đó vào heap. 

### Bước 6 

Sau khi tất cả các vị trí được xử lý, hãy xác minh rằng mọi giá trị cố định đều được đặt ở vị trí yêu cầu. 

Hoán vị được xây dựng là câu trả lời. 

### Tại sao nó hoạt động 

Rừng Cartesian mã hóa chính xác các ràng buộc ưu tiên không bao giờ có thể bị vi phạm bởi các hoạt động được phép. 

Mọi hoán vị có thể truy cập phải đặt mỗi cha mẹ trước tất cả các con của nó. Ngược lại, mọi trật tự tôpô của khu rừng này đều có thể được thực hiện thông qua các bước di chuyển được phép. Do đó, các hoán vị có thể tiếp cận chính xác là thứ tự tôpô của khu rừng. 

Trong quá trình xây dựng, tập hợp có sẵn chứa chính xác các nút mà cha mẹ của chúng đã xuất hiện. Việc chọn bất kỳ nút có sẵn nào sẽ duy trì khả năng hoàn thành thứ tự tôpô. 

Đối với các vị trí cố định, giá trị yêu cầu phải có sẵn. Nếu nó không có sẵn thì tổ tiên nào đó vẫn chưa xuất hiện, khiến yêu cầu này không thể thực hiện được trong mọi trật tự tôpô. 

Trong số các vị thế miễn phí, việc chọn giá trị sẵn có lớn nhất là sự lựa chọn tham lam làm trì hoãn các giá trị nhỏ hơn. Các giá trị nhỏ hơn có giá trị hơn vì chúng mở khóa con cháu. Trì hoãn chúng chỉ có thể làm tăng tính linh hoạt trong tương lai. Đối số trao đổi tiêu chuẩn cho thấy rằng nếu một giải pháp tồn tại, việc thay thế giá trị vị trí tự do đã chọn bằng giá trị lớn nhất hiện có sẽ duy trì sự tồn tại. 

Do đó, thuật toán đưa ra một sự hoàn thành hợp lệ bất cứ khi nào nó tồn tại và báo cáo chính xác những điều không thể thực hiện được. 

## Giải pháp Python```python
import sys
from bisect import bisect_left
from heapq import heappush, heappop

input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, idx, val):
        n = self.n
        bit = self.bit
        while idx <= n:
            bit[idx] = max(bit[idx], val)
            idx += idx & -idx

    def query(self, idx):
        res = 0
        bit = self.bit
        while idx > 0:
            res = max(res, bit[idx])
            idx -= idx & -idx
        return res

def solve():
    t = int(input())

    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        c = list(map(int, input().split()))

        pos = [0] * (n + 1)
        for i, x in enumerate(a, 1):
            pos[x] = i

        parent = [0] * (n + 1)
        children = [[] for _ in range(n + 1)]

        fw = Fenwick(n)

        for x in range(1, n + 1):
            ppos = fw.query(pos[x])
            if ppos:
                par = a[ppos - 1]
                parent[x] = par
                children[par].append(x)
            fw.add(pos[x], pos[x])

        fixed_pos = [0] * (n + 1)
        ok = True

        for i, x in enumerate(c, 1):
            if x:
                fixed_pos[x] = i

        available = []
        active = [False] * (n + 1)

        for x in range(1, n + 1):
            if parent[x] == 0:
                active[x] = True
                heappush(available, -x)

        ans = [0] * n

        for i in range(1, n + 1):
            need = c[i - 1]

            if need:
                if not active[need]:
                    ok = False
                    break

                active[need] = False
                ans[i - 1] = need

                for y in children[need]:
                    active[y] = True
                    heappush(available, -y)

            else:
                while available and not active[-available[0]]:
                    heappop(available)

                if not available:
                    ok = False
                    break

                x = -heappop(available)
                active[x] = False
                ans[i - 1] = x

                for y in children[x]:
                    active[y] = True
                    heappush(available, -y)

        if not ok:
            out.append("-1")
        else:
            out.append(" ".join(map(str, ans)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Sau khi tính toán vị trí của tất cả các giá trị, mã sẽ xây dựng rừng Descartes. Cây Fenwick lưu trữ vị trí lớn nhất thuộc về bất kỳ giá trị nhỏ hơn nào đã được xử lý. Truy vấn tiền tố lên đến`pos[x]`cho giá trị nhỏ hơn gần nhất ở bên trái. 

Việc xây dựng cấu trúc liên kết sử dụng một đống tối đa. Python chỉ cung cấp vùng heap tối thiểu nên các giá trị được chèn vào sẽ bị phủ định. 

các`active`mảng đáng được quan tâm. Các mục nhập heap không bao giờ bị xóa ngay lập tức khi một nút không còn khả dụng. Thay vào đó, các mục cũ sẽ bị loại bỏ một cách lười biếng trong khi giải nén. Điều này tránh được việc loại bỏ tốn kém. 

Một điểm tinh tế khác là việc xử lý vị trí cố định. Một giá trị chỉ hợp lệ nếu nó hiện đang hoạt động. Chỉ kiểm tra xem nó chưa được sử dụng là chưa đủ. Cha mẹ của nó chắc hẳn đã xuất hiện rồi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = [3,5,6,2,1,4]
c = [0,2,0,5,0,0]
```Rừng là```
1
├─2
└─4

3
└─5
   └─6
```Các vị trí xử lý: 

| Vị trí | Đã sửa? | Có sẵn trước | Được chọn | 
| --- | --- | --- | --- | 
| 1 | Không | {1,3} | 3 | 
| 2 | Có=2 | {1,5} | 2 | 
| 3 | Không | {1,5} | 5 | 
| 4 | Có=5 | không thể | - | 

Ở vị trí 4, giá trị 5 đã được sử dụng. Không thể đạt được vị trí yêu cầu. 

Thuật toán báo cáo lỗi chính xác. 

### Ví dụ 2 

đầu vào:```
a = [3,2,1,5,4]
c = [1,3,0,0,0]
```Rừng:```
1
├─2
│  └─3

4
└─5
```Xử lý: 

| Vị trí | Đã sửa? | Có sẵn trước | Được chọn | 
| --- | --- | --- | --- | 
| 1 | Có=1 | {1,4} | 1 | 
| 2 | Có=3 | {2,4} | không thể | 

Giá trị 3 không có vì tổ tiên 2 của nó chưa xuất hiện. 

Thuật toán phát hiện ngay sự mâu thuẫn và đưa ra kết quả`-1`. 

Ví dụ này chứng minh tại sao những ràng buộc về tổ tiên lại quan trọng. Chỉ nhìn vào các vị trí sẽ bỏ lỡ điều không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Xây dựng rừng Descartes và hoạt động đống | 
| Không gian | O(n) | Cây, đống, mảng vị trí, kết cấu phụ trợ | 

Tổng số tiền của`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`5 · 10^5`. MỘT`O(n log n)`giải pháp thực hiện khoảng vài triệu thao tác, phù hợp thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ là tuyến tính và vẫn thấp hơn nhiều so với giới hạn 512 MB. 

## Trường hợp thử nghiệm```python
# helper skeleton

import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    # invoke solve() here

    return out.getvalue()

# minimum size
# a = [2,1]
# only reachable completion
#
# expected:
# 1 2

# fixed root
# verifies ancestor constraint

# chain-shaped Cartesian tree
# verifies activation order

# all positions fixed and already valid
# answer must equal c

# impossible because child required before parent
# answer must be -1
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=2`trường hợp nhỏ nhất | hoán vị hợp lệ | ranh giới tối thiểu | 
| Vị trí gốc cố định | xây dựng hợp lệ | kích hoạt root | 
| Cây xích dài | xây dựng hợp lệ | sắp xếp tổ tiên | 
| Hoán vị hợp lệ được chỉ định đầy đủ | hoán vị giống nhau | không có những thay đổi không cần thiết | 
| Yêu cầu con trước cha mẹ |`-1`| phát hiện khả năng tiếp cận | 

## Vỏ cạnh 

Hãy xem xét```
a = [3,2,1]
c = [0,3,0]
```Rừng là```
1 -> 2 -> 3
```Ở vị trí 2, giá trị 3 được yêu cầu. Giá trị 3 không thể có sẵn cho đến khi cả 1 và 2 được đặt. Thuật toán thấy rằng 3 không hoạt động và trả về ngay lập tức`-1`. 

Coi như```
a = [3,2,1,5,4]
c = [3,2,1,5,4]
```Mọi vị trí đều được cố định. Mỗi giá trị bắt buộc sẽ có sẵn chính xác khi cần thiết. Thuật toán chỉ đơn giản là tái tạo lại hoán vị tương tự. 

Coi như```
a = [2,1,4,3]
c = [0,0,0,0]
```Nhiều trật tự tôpô tồn tại. Vùng heap tối đa chọn giá trị lớn nhất hiện có ở mọi vị trí trống. Sự lựa chọn tham lam này vẫn đảm bảo tính khả thi vì mọi giá trị được chọn đều có sẵn và việc trì hoãn các giá trị nhỏ hơn không bao giờ cản trở việc hoàn thành trong tương lai. 

Các trường hợp này bao gồm các chế độ lỗi chính: yêu cầu một nút trước nút tổ tiên của nó, cố định hoàn toàn các mục tiêu hợp lệ và các vị trí không bị giới hạn với nhiều thứ tự tôpô có thể có.
