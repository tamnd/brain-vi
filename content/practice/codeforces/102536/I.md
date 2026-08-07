---
title: "CF 102536I - Vinh quang cho Algotzka"
description: "Hệ thống phân cấp của công ty là một cây có gốc. Nhân viên i là gốc của khu vực được điều tra và một báo cáo hợp lệ là bất kỳ tập hợp nhân viên nào được kết nối có chứa i."
date: "2026-08-06T20:29:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102536
codeforces_index: "I"
codeforces_contest_name: "2020 UP ACM Algolympics Final Round"
rating: 0
weight: 102536
solve_time_s: 88
verified: true
draft: false
---

[CF 102536I - Vinh quang thuộc về Algotzka](https://codeforces.com/problemset/problem/102536/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hệ thống phân cấp của công ty là một cây có gốc. Người lao động`i`là gốc của khu vực được điều tra và một báo cáo hợp lệ là bất kỳ tập hợp nhân viên nào được kết nối có chứa`i`. Bởi vì cấu trúc là một cái cây, điều này có nghĩa là bất cứ khi nào chúng ta bao gồm một hậu duệ, mọi nhân viên trên đường quay lại`i`cũng phải được đưa vào. 

Mỗi nhân viên có một trong hai loại. Một truy vấn hỏi xem có tập hợp kết nối hợp lệ nào đó bên trong cây con của`i`chứa chính xác`c`nhân viên thuộc loại`C`và chính xác`s`nhân viên thuộc loại`S`. 

Thứ tự đầu vào có một thuộc tính hữu ích: mọi cha mẹ đều xuất hiện trước con của nó. Điều này cho phép lập trình động được thực hiện theo thứ tự chỉ mục ngược. Các giới hạn không đối xứng: chỉ có`10000`nhân viên, nhưng lên đến`200000`truy vấn. Một giải pháp khám phá cây cho mọi truy vấn cần có xung quanh`2 * 10^9`lượt truy cập nút trong trường hợp xấu nhất, vượt xa giới hạn thời gian. Giai đoạn tiền xử lý phải thực hiện hầu hết mọi công việc, khiến mỗi truy vấn có thời gian gần như không đổi. 

Một lỗi phổ biến là chỉ lưu trữ các kích thước cây con có thể có. Điều đó làm mất thông tin vì hai tập hợp kết nối có cùng kích thước có thể chứa số lượng khác nhau`C`người lao động. 

Ví dụ, hãy xem xét:```
2 1
0 1
CS
1 1 1
```Câu trả lời là`COMPROMISED`bởi vì việc chọn cả hai nhân viên sẽ mang lại cho một người`C`và một`S`. Một giải pháp chỉ lưu trữ các kích thước có thể sẽ biết kích thước đó`2`là có thể, nhưng nó sẽ không biết phân phối. 

Một trường hợp cạnh khác là truy vấn yêu cầu số lượng lớn hơn kích thước cây con.```
1 2
0
C
1 0 1
1 2 0
```Truy vấn đầu tiên là`COMPROMISED`, bởi vì nút đơn là số lượng xã hội chủ nghĩa bằng 0 và số lượng tư bản chủ nghĩa là một. Truy vấn thứ hai là`NOT COMPROMISED`, vì không có đủ nhân viên. Bất kỳ việc triển khai nào quên kiểm tra giới hạn mảng đều có thể truy cập sai các trạng thái không hợp lệ. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là liệt kê mọi cây con gốc được kết nối cho mỗi truy vấn. Điều này đúng vì mọi câu trả lời có thể đều được kiểm tra, nhưng số lượng cây con được kết nối có thể theo cấp số nhân. Ngay cả một con đường dài`10000`đã có số bậc hai của các lựa chọn kết nối gốc và thực hiện việc này cho`200000`truy vấn là không thể. 

Cải tiến đầu tiên là xử lý trước mọi nút. Đối với một nút`v`, thay vì lưu trữ mọi cây con có thể kết nối, hãy lưu trữ thông tin cho mọi kích thước có thể`t`. Trong số tất cả các bộ kích thước được kết nối`t`bắt nguồn từ`v`, chúng ta chỉ cần số lượng tối thiểu và tối đa có thể có của`C`người lao động. Quan sát quan trọng là mọi giá trị giữa hai thái cực đó đều có thể xảy ra. 

Nếu số lượng tối thiểu`C`nhân viên là`a`và tối đa là`b`, việc chuyển đổi cấu trúc tối thiểu thành cấu trúc tối đa bằng cách thay thế một nút đã chọn tại một thời điểm sẽ thay đổi số lượng tối đa một nút mỗi lần. Chuỗi phải đi qua mọi giá trị từ`a`ĐẾN`b`. 

Do đó, trạng thái lập trình động là:`minC[v][t]`= số lượng tối thiểu`C`nhân viên trong một tập hợp kết nối hợp lệ của`t`các nút bắt nguồn từ`v`.`maxC[v][t]`được định nghĩa tương tự. 

Khi kết hợp trẻ em, chúng tôi hợp nhất những đóng góp có thể có của chúng giống như một chiếc ba lô hình cây. Tổng công việc hợp nhất là bậc hai chứ không phải bậc ba vì một cặp nút chỉ được kết hợp khi tổ tiên chung thấp nhất của chúng được xử lý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Số mũ cho mỗi truy vấn | O(n) | Quá chậm | 
| DP tối ưu | Tiền xử lý O(n²), truy vấn O(1) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xử lý các nút từ`n`xuống tới`1`. Vì cha mẹ luôn có chỉ số nhỏ hơn con nên mọi con đều đã được xử lý rồi. 
2. Khởi tạo trạng thái lập trình động của một nút bằng chính nút đó. Nếu nút là`C`, một tập hợp kết nối có kích thước một có một nhà tư bản. Nếu nó là`S`, giá trị bằng 0. 
3. Hợp nhất mọi nút con vào nút hiện tại. Đối với mọi số nút có thể đã được lấy và mọi số có thể được lấy từ cây con con, hãy cập nhật số lượng tư bản tối thiểu và tối đa. 
4. Lưu trữ mảng kết quả cho mỗi nút. Một truy vấn`(i, c, s)`hỏi về một tập hợp kích thước được kết nối`c+s`. Nếu như`c`nằm giữa`minC[i][c+s]`Và`maxC[i][c+s]`, câu trả lời là`COMPROMISED`. 

Tại sao nó hoạt động: DP lưu trữ số lượng cực lớn các nhà tư bản cho mọi quy mô có thể. Thuộc tính khoảng chứng tỏ rằng không có giá trị nào giữa các cực trị này bị thiếu. Điều kiện truy vấn kiểm tra chính xác xem số lượng nhà tư bản được yêu cầu có thuộc khoảng đó hay không, trong khi tổng kích thước sẽ tự động sửa số lượng nhà xã hội chủ nghĩa. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    parent = list(map(int, input().split()))
    s = input().strip()

    children = [[] for _ in range(n)]
    for i in range(1, n):
        children[parent[i] - 1].append(i)

    INF = 30000
    mins = [None] * n
    maxs = [None] * n

    for v in range(n - 1, -1, -1):
        val = 1 if s[v] == 'C' else 0

        if len(children[v]) == 0:
            mins[v] = array('h', [INF, val])
            maxs[v] = array('h', [-INF, val])
            continue

        if len(children[v]) == 1:
            ch = children[v][0]
            cm = mins[ch]
            cx = maxs[ch]
            mn = array('h', [INF] * (len(cm) + 1))
            mx = array('h', [-INF] * (len(cx) + 1))
            mn[1] = val
            mx[1] = val
            for t in range(1, len(cm)):
                mn[t + 1] = cm[t] + val
                mx[t + 1] = cx[t] + val
            mins[v] = mn
            maxs[v] = mx
            continue

        cur_min = array('h', [INF, val])
        cur_max = array('h', [-INF, val])

        for ch in children[v]:
            cm = mins[ch]
            cx = maxs[ch]
            a = len(cur_min) - 1
            b = len(cm) - 1
            nm = array('h', [INF] * (a + b + 1))
            nx = array('h', [-INF] * (a + b + 1))

            for i in range(1, a + 1):
                if cur_min[i] < nm[i]:
                    nm[i] = cur_min[i]
                if cur_max[i] > nx[i]:
                    nx[i] = cur_max[i]

            for i in range(1, a + 1):
                if cur_min[i] == INF:
                    continue
                for j in range(1, b + 1):
                    if cm[j] != INF:
                        x = cur_min[i] + cm[j]
                        if x < nm[i + j]:
                            nm[i + j] = x
                    if cx[j] != -INF:
                        x = cur_max[i] + cx[j]
                        if x > nx[i + j]:
                            nx[i + j] = x

            cur_min, cur_max = nm, nx

        for i in range(1, len(cur_min)):
            cur_min[i] += val
            cur_max[i] += val

        mins[v] = cur_min
        maxs[v] = cur_max

    out = []
    for _ in range(q):
        i, c, st = map(int, input().split())
        i -= 1
        size = c + st
        if size < len(mins[i]) and mins[i][size] <= c <= maxs[i][size]:
            out.append("COMPROMISED")
        else:
            out.append("NOT COMPROMISED")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc lặp lại ngược lại sẽ tránh được các vấn đề về độ sâu đệ quy vì thứ tự đầu vào đã đưa ra thứ tự từ dưới lên hợp lệ. Mỗi mảng được lưu trữ có một mục nhập cho mỗi kích thước tập hợp được kết nối có thể có. các`array('h')`container giữ mức sử dụng bộ nhớ ở mức thấp vì mọi giá trị được lưu trữ nhiều nhất`10000`. 

Việc tối ưu hóa một con là cần thiết. Nếu không có nó, một cây có hình đường dẫn sẽ liên tục hợp nhất một trạng thái con lớn và phân hủy thành tác phẩm hình khối. Với việc tối ưu hóa, việc hợp nhất ba lô đắt tiền chỉ xảy ra ở các nút phân nhánh, trong đó tổng lượng tương tác theo cặp trên toàn bộ cây vẫn là bậc hai. 

Truy vấn sử dụng`size = c + s`bởi vì mọi nhân viên được chọn đều`C`hoặc`S`. Khi quy mô và số lượng tư bản chủ nghĩa đã được cố định, số lượng xã hội chủ nghĩa sẽ được xác định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n2 + q) | Tiền xử lý ba lô cây cộng với các truy vấn thời gian không đổi | 
| Không gian | O(n²) | Tất cả các khoảng DP được lưu trữ cho mỗi nút | 

Tổng kích thước cây con lớn nhất có thể là khoảng`n²/2`, tức là khoảng năm mươi triệu tiểu bang cho một chuỗi. Các mảng số nguyên nhỏ gọn giữ giá trị này trong giới hạn bộ nhớ rộng rãi. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
5 3
0 1 2 3 4
CSCSC
1 3 2
1 2 2
2 2 1
```Cây là một chuỗi:```
1(C)
 |
2(S)
 |
3(C)
 |
4(S)
 |
5(C)
```Các trạng thái gốc bao gồm: 

| Nút | Kích thước | Tối thiểu C | C tối đa | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 1 | 2 | 1 | 1 | 
| 1 | 3 | 2 | 2 | 
| 1 | 4 | 2 | 2 | 
| 1 | 5 | 3 | 3 | 

Truy vấn đầu tiên yêu cầu kích thước`5`Và`3`các nhà tư bản nằm trong khoảng đó nên được chấp nhận. 

Truy vấn thứ ba hỏi về nút`2`với`3`tổng số lao động và`2`những nhà tư bản. Chuỗi có thể có từ nút`2`có nhân viên`2,3,4,5`, và trong số ba lựa chọn, số lượng tư bản không thể đạt tới hai, vì vậy nó bị từ chối. 

## Trường hợp thử nghiệm```
# The solution above can be tested with the following inputs.

sample = """5 3
0 1 2 3 4
CSCSC
1 3 2
1 2 2
2 2 1
"""

case1 = """1 2
0
C
1 1 0
1 0 1
"""

case2 = """3 3
0 1 1
SSS
1 0 3
2 0 2
2 1 1
"""

case3 = """4 3
0 1 1 2
CCCC
1 4 0
1 2 2
2 1 1
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Nút đơn |`COMPROMISED`,`NOT COMPROMISED`| Cây nhỏ nhất và số lượng không hợp lệ | 
| Tất cả`S`nút | Phạm vi chỉ dành cho xã hội chủ nghĩa hợp lệ | Tất cả các giá trị bằng nhau | 
| Tất cả`C`nút | Ranh giới tư bản | Xử lý kích thước chính xác | 

## Vỏ cạnh 

Một cây nhân viên được xử lý bằng cách chỉ khởi tạo trạng thái có kích thước một. Không có sự hợp nhất con nào xảy ra, vì vậy câu trả lời chỉ phụ thuộc vào loại riêng của nhân viên. 

Đối với một chuỗi dài, quá trình chuyển đổi một con sao chép các trạng thái của con bằng sự thay đổi kích thước. Điều này tránh việc hợp nhất lồng nhau tốn kém và giữ thời gian chạy bậc hai. 

Đối với cây phân nhánh, bước hợp nhất giữ cả hai thái cực. Một truy vấn yêu cầu giá trị ở giữa được chấp nhận vì thuộc tính khoảng bao gồm mọi số lượng tư bản có thể có giữa hai thái cực.
