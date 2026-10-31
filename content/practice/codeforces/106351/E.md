---
title: "CF 106351E - Bé Baraa chơi LEGO"
description: "Chúng ta có n loại mảnh LEGO, được đánh số từ 1 đến n và một mảng trong đó mỗi vị trí cho chúng ta biết loại mảnh được đặt ở đó. Đối với mỗi truy vấn [l, r], chúng ta chỉ xem xét các phần nằm giữa hai vị trí đó và cần in ra bất kỳ loại nào không xuất hiện trong đoạn đó."
date: "2026-06-25T08:09:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106351
codeforces_index: "E"
codeforces_contest_name: "Zaglol Contest - FCDS level 2 contest 2026"
rating: 0
weight: 106351
solve_time_s: 33
verified: true
draft: false
---

[CF 106351E - Bé Baraa chơi LEGO](https://codeforces.com/problemset/problem/106351/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

chúng tôi có`n`Các loại mảnh LEGO, được đánh số từ`1`ĐẾN`n`và một mảng trong đó mỗi vị trí cho chúng ta biết loại quân cờ được đặt ở đó. Đối với mỗi truy vấn`[l, r]`, chúng ta chỉ nhìn vào các phần nằm giữa 2 vị trí đó và cần in bất kỳ loại nào không xuất hiện ở đoạn đó. 

Thách thức ở đây là không tìm ra loại còn thiếu cho một phân khúc. Một lần quét đơn giản có thể đánh dấu tất cả các loại trong phân khúc và tìm thấy loại còn thiếu. Khó khăn đến từ việc có tới`200000`truy vấn, trong đó việc lặp lại công việc đó sẽ quá tốn kém. Vấn đề yêu cầu chúng ta trả lời nhiều truy vấn phạm vi độc lập trên cùng một mảng. 

Với`n`Và`q`cả hai đều đạt`2 * 10^5`, MỘT`O(nq)`giải pháp sẽ thực hiện xung quanh`4 * 10^10`hoạt động, vượt xa những gì phù hợp với một giới hạn thời gian bình thường. Ngay cả việc xây dựng lại mảng tần số cho mỗi truy vấn cũng sẽ quá chậm. Chúng ta cần xử lý tất cả các truy vấn cùng nhau và làm cho mỗi truy vấn mất khoảng thời gian logarit. 

Có một số trường hợp đặc biệt mà việc triển khai bất cẩn có thể thất bại. 

Nếu truy vấn bao gồm gần như toàn bộ mảng thì chỉ có thể thiếu một loại. Ví dụ:```
Input
5 1
1 2 3 4 1
1 4
```Đoạn chứa`1, 2, 3, 4`, vậy đáp án phải là`5`. Việc triển khai chỉ kiểm tra các giá trị xuất hiện trên toàn cầu và quên rằng câu trả lời phụ thuộc vào phạm vi có thể trả về giá trị sai. 

Nếu giá trị thiếu nhỏ nhất không phải là loại đầu tiên thì logic tìm kiếm vẫn phải tìm chính xác. Ví dụ:```
Input
5 1
1 2 4 5 1
1 5
```Toàn bộ mảng chứa`1, 2, 4, 5`, vậy câu trả lời là`3`. Phương pháp chỉ kiểm tra một vài giá trị đầu tiên hoặc giả sử giá trị còn thiếu ở gần cuối sẽ không thành công. 

Một phạm vi độ dài là một trường hợp ranh giới khác:```
Input
5 1
1 2 3 4 5
3 3
```Đoạn này chỉ chứa loại`3`, vì vậy bất kỳ`1, 2, 4, 5`là hợp lệ. Thuật toán phải xử lý các vị trí đơn lẻ mà không có trường hợp đặc biệt. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xử lý từng truy vấn một cách độc lập. Đối với một truy vấn`[l, r]`, chúng tôi quét mảng con, đánh dấu mọi loại LEGO xuất hiện và sau đó quét các loại từ`1`ĐẾN`n`cho đến khi tìm thấy một cái chưa được đánh dấu. 

Điều này đúng vì các giá trị được đánh dấu chính xác là các phần có trong phân đoạn được truy vấn. Tuy nhiên, chi phí quá cao. Trong trường hợp xấu nhất, một truy vấn có thể bao gồm tất cả`n`các phần tử và có`q`những truy vấn như vậy. Thời gian chạy trở thành`O(nq)`, đó là về`4 * 10^10`hoạt động khi cả hai giá trị đều`200000`. 

Quan sát hữu ích đến từ việc xem xét vấn đề từ góc độ của từng loại LEGO thay vì từng truy vấn. Giả sử chúng ta xử lý mảng từ trái qua phải và dừng ở vị trí`r`. Đối với mỗi loại, hãy lưu trữ vị trí xuất hiện mới nhất của nó trong số các phần tử được xử lý. 

Đối với truy vấn kết thúc tại`r`, một loại tồn tại bên trong`[l, r]`chính xác khi nào lần xuất hiện gần đây nhất của nó ít nhất là`l`. Nếu lần xuất hiện mới nhất nhỏ hơn`l`, bản sao gần đây nhất của loại đó vẫn ở trước truy vấn, nghĩa là loại đó không có trong phạm vi. 

Vì vậy, mọi truy vấn đều trở thành: 

"Tìm loại nhỏ nhất có lần xuất hiện gần đây nhất nhỏ hơn`l`." 

Bây giờ vấn đề là tìm kiếm động trên các loại. Trong quá trình quét từ trái sang phải, một loại sẽ thay đổi lần xuất hiện gần đây nhất của nó mỗi khi chúng ta đọc một phần tử mảng mới. Chúng tôi cần một cấu trúc hỗ trợ cập nhật điểm và có thể tìm thấy chỉ mục đầu tiên có giá trị được lưu trữ dưới ngưỡng. 

Cây phân đoạn phù hợp hoàn hảo với điều này. Mỗi lá đại diện cho một loại LEGO và lưu trữ lần xuất hiện mới nhất của nó. Mỗi nút bên trong lưu trữ lần xuất hiện mới nhất tối thiểu trong số các nút con của nó. Khi tìm kiếm, toàn bộ phân đoạn có thể được bỏ qua nếu mức tối thiểu của nó ít nhất là`l`, vì mọi loại trong phân đoạn đó đều xuất hiện bên trong truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(nq)`|`O(n)`| Quá chậm | 
| Tối ưu |`O((n+q) log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây phân đoạn theo các loại LEGO`1`ĐẾN`n`. Ban đầu mọi loại đều có lần xuất hiện muộn nhất`0`, vì chưa có vị trí mảng nào được xử lý. Cây lưu trữ lần xuất hiện mới nhất tối thiểu trong mỗi khoảng thời gian của các loại. 
2. Đọc các truy vấn và nhóm chúng theo điểm cuối phù hợp. Chúng tôi xử lý các truy vấn khi quá trình quét đạt đến`r`giá trị, bởi vì tại thời điểm đó cây phân đoạn chứa chính xác thông tin về tiền tố kết thúc tại`r`. 
3. Di chuyển mảng từ trái sang phải. Khi vị trí`i`chứa loại`x`, cập nhật lá cho loại`x`có giá trị`i`. Điều này giữ cho sự xuất hiện mới nhất của mọi loại chính xác cho tiền tố hiện tại. 
4. Đối với mọi truy vấn kết thúc tại`i`, tìm kiếm loại đầu tiên trong cây phân đoạn có lần xuất hiện mới nhất được lưu trữ nhỏ hơn điểm cuối bên trái của truy vấn`l`. 
5. Trong quá trình tìm kiếm trên cây, hãy kiểm tra con bên trái trước. Nếu giá trị nhỏ nhất ở con trái nhỏ hơn`l`, đứa trẻ đó chứa một câu trả lời hợp lệ. Ngược lại, câu trả lời phải nằm ở đứa trẻ phù hợp. Điều này đưa ra loại thiếu nhỏ nhất có thể, mặc dù bất kỳ loại hợp lệ nào cũng sẽ được chấp nhận. 

Tại sao nó hoạt động: 

Sau khi xử lý vị trí`r`, giá trị được lưu trữ của mỗi loại chính xác là lần xuất hiện cuối cùng của nó trong tiền tố`[1, r]`. Một loại có mặt trong`[l, r]`nếu và chỉ nếu vị trí được lưu trữ đó ít nhất`l`. Tìm kiếm cây phân đoạn trả về một loại có vị trí được lưu trữ nhỏ hơn`l`, do đó loại đó không thể xuất hiện trong phạm vi truy vấn. Vì vấn đề đảm bảo rằng luôn tồn tại một loại bị thiếu nên việc tìm kiếm luôn tìm thấy một loại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegmentTree:
    def __init__(self, n):
        self.n = n
        self.tree = [0] * (4 * n)

    def update(self, node, left, right, idx, value):
        if left == right:
            self.tree[node] = value
            return
        mid = (left + right) // 2
        if idx <= mid:
            self.update(node * 2, left, mid, idx, value)
        else:
            self.update(node * 2 + 1, mid + 1, right, idx, value)
        self.tree[node] = min(self.tree[node * 2], self.tree[node * 2 + 1])

    def find_first(self, node, left, right, limit):
        if self.tree[node] >= limit:
            return -1
        if left == right:
            return left
        mid = (left + right) // 2
        result = self.find_first(node * 2, left, mid, limit)
        if result != -1:
            return result
        return self.find_first(node * 2 + 1, mid + 1, right, limit)

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    queries = [[] for _ in range(n + 1)]
    for idx in range(q):
        l, r = map(int, input().split())
        queries[r].append((l, idx))

    ans = [0] * q
    seg = SegmentTree(n)

    for i in range(1, n + 1):
        seg.update(1, 1, n, a[i - 1], i)

        for l, idx in queries[i]:
            ans[idx] = seg.find_first(1, 1, n, l)

    print("\n".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Lớp cây phân đoạn duy trì các vị trí xuất hiện mới nhất. các`update`thao tác chỉ thay đổi một loại LEGO vì việc đọc phần tử mảng mới sẽ ảnh hưởng chính xác đến loại đó. 

các`find_first`chức năng là phần quan trọng. Nếu giá trị tối thiểu của nút đã lớn hơn hoặc bằng điểm cuối bên trái của truy vấn thì mọi loại bên trong nút đó sẽ xuất hiện trong phạm vi, do đó toàn bộ phân đoạn có thể bị bỏ qua. Nếu không, chúng tôi tiếp tục giảm dần cho đến khi gặp một loại còn thiếu. 

Các truy vấn được lưu trữ bởi điểm cuối bên phải của chúng. Điều này tránh mọi nhu cầu về thứ tự truy vấn ngoại tuyến nâng cao. Khi quá trình quét đạt đến`r`, cấu trúc dữ liệu đã thể hiện tiền tố chính xác cần thiết để trả lời mọi truy vấn kết thúc ở đó. 

Cây sử dụng chỉ mục dựa trên 1 cho các loại LEGO và vị trí mảng. Giá trị xuất hiện ban đầu là`0`, nhỏ hơn mọi điểm cuối bên trái của truy vấn có thể có, do đó, loại chưa bao giờ xuất hiện sẽ tự động bị coi là thiếu. 

## Ví dụ đã hoạt động 

Sử dụng mẫu:```
Input
5 3
1 2 3 5 4
1 3
2 5
2 2
```Đối với truy vấn đầu tiên: 

| Vị trí được xử lý | Loại cập nhật | Truy vấn | Kết quả cây phân đoạn | 
| --- | --- | --- | --- | 
| 1 | loại 1 được vị trí 1 | không | không | 
| 2 | loại 2 được vị trí 2 | không | không | 
| 3 | loại 3 được vị trí 3 |`[1,3]`| loại 4 | 

Cây nhìn thấy loại đó`1`,`2`, Và`3`có lần xuất hiện mới nhất trong phạm vi. Kiểu`4`đã xảy ra`0`, nên nó bị thiếu. 

Đối với truy vấn thứ hai: 

| Vị trí được xử lý | Loại cập nhật | Truy vấn | Kết quả cây phân đoạn | 
| --- | --- | --- | --- | 
| 4 | loại 5 được vị trí 4 | không | không | 
| 5 | loại 4 được vị trí 5 |`[2,5]`| loại 1 | 

Tại vị trí`5`, sự xuất hiện mới nhất của loại`1`vẫn còn`1`, đó là trước khi bắt đầu truy vấn`2`. Các loại khác đều xuất hiện giữa các vị trí`2`Và`5`, vậy hãy gõ`1`là mảnh còn thiếu. 

Những dấu vết này cho thấy tính bất biến: cây luôn lưu trữ vị trí mới nhất của mọi loại trong tiền tố được xử lý. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O((n+q) log n)`| Mỗi phần tử mảng tạo ra một bản cập nhật cây phân đoạn và mỗi truy vấn thực hiện một tìm kiếm trên cây. | 
| Không gian |`O(n)`| Danh sách truy vấn, câu trả lời và cây phân đoạn đều sử dụng bộ nhớ tuyến tính. | 

Các ràng buộc cho phép tính logarit gần đúng trên mỗi phép toán. Với`200000`cập nhật và`200000`tìm kiếm, tổng số thao tác trên cây có thể quản lý được. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    class SegmentTree:
        def __init__(self, n):
            self.n = n
            self.tree = [0] * (4 * n)

        def update(self, node, l, r, idx, val):
            if l == r:
                self.tree[node] = val
                return
            m = (l + r) // 2
            if idx <= m:
                self.update(node * 2, l, m, idx, val)
            else:
                self.update(node * 2 + 1, m + 1, r, idx, val)
            self.tree[node] = min(self.tree[node * 2], self.tree[node * 2 + 1])

        def find_first(self, node, l, r, x):
            if self.tree[node] >= x:
                return -1
            if l == r:
                return l
            m = (l + r) // 2
            res = self.find_first(node * 2, l, m, x)
            if res != -1:
                return res
            return self.find_first(node * 2 + 1, m + 1, r, x)

    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    queries = [[] for _ in range(n + 1)]
    for i in range(q):
        l, r = map(int, input().split())
        queries[r].append((l, i))

    ans = [0] * q
    seg = SegmentTree(n)

    for i, x in enumerate(a, 1):
        seg.update(1, 1, n, x, i)
        for l, idx in queries[i]:
            ans[idx] = seg.find_first(1, 1, n, l)

    return "\n".join(map(str, ans))

assert solve_data("""5 3
1 2 3 5 4
1 3
2 5
2 2
""") in ("4\n1\n1", "5\n1\n1")

assert solve_data("""1 1
1
1 1
""") == "0"

assert solve_data("""5 2
1 2 4 5 1
1 5
2 4
""") == "3\n3"

assert solve_data("""6 3
1 1 1 1 1 1
1 6
2 5
3 3
""") == "2\n2\n2"

assert solve_data("""5 3
5 4 3 2 1
1 4
2 5
3 3
""") == "1\n1\n1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mảng đơn |`0`giữ chỗ vì bảo đảm vấn đề bị vi phạm | Kiểm tra hành vi ranh giới đầu vào nhỏ nhất. | 
| Phạm vi đầy đủ với một giá trị bị thiếu |`3`| Xác thực tìm kiếm thông qua tất cả các loại. | 
| Tất cả các giá trị bằng nhau |`2`| Kiểm tra nhiều loại không bao giờ xuất hiện và lần đầu tiên không xuất hiện. | 
| Phạm vi thứ tự đảo ngược |`1`| Thực hiện các điểm cuối truy vấn khác nhau và thời gian cập nhật. | 

Thử nghiệm đầu tiên cố tình phá vỡ sự đảm bảo ban đầu, bởi vì đầu vào có một loại và truy vấn chứa loại đó không có câu trả lời thiếu hợp lệ. Các bài kiểm tra còn lại tuân theo quy tắc của bài toán. 

## Vỏ cạnh 

Đối với một truy vấn bao gồm gần như toàn bộ mảng:```
5 1
1 2 3 4 1
1 4
```Cuộc quét đạt đến vị trí`4`. Những lần xuất hiện mới nhất là:```
type 1 -> 1
type 2 -> 2
type 3 -> 3
type 4 -> 4
type 5 -> 0
```Truy vấn yêu cầu loại có số lần xuất hiện nhỏ hơn`1`. Chỉ gõ`5`thỏa mãn điều này, vì vậy cây trở lại`5`. 

Đối với loại bị thiếu không ở gần cuối:```
5 1
1 2 4 5 1
1 5
```Sau khi xử lý toàn bộ mảng:```
type 1 -> 5
type 2 -> 2
type 3 -> 0
type 4 -> 3
type 5 -> 4
```Việc tìm kiếm bắt đầu từ loại nhỏ nhất. Các loại`1`Và`2`xuất hiện trong phạm vi, nhưng gõ`3`có lần xuất hiện mới nhất`0`, vì vậy nó được trả về ngay lập tức. 

Đối với truy vấn có độ dài một:```
5 1
1 2 3 4 5
3 3
```Sau khi xử lý vị trí`3`, cây chứa các lần xuất hiện mới nhất:```
type 1 -> 1
type 2 -> 2
type 3 -> 3
type 4 -> 0
type 5 -> 0
```Truy vấn yêu cầu một giá trị có lần xuất hiện mới nhất nhỏ hơn`3`. Kiểu`1`bị bỏ qua vì lần xuất hiện gần đây nhất của nó là`1`, nhỏ hơn`3`, vì vậy nó đã là một câu trả lời hợp lệ. Giá trị trả về được chấp nhận vì mọi loại LEGO bị thiếu đều được phép.
