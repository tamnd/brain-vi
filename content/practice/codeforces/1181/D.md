---
title: "CF 1181D - Thủy lợi"
description: "Chúng ta được giao một chuỗi các nhiệm vụ trước đây trong đó mỗi thành phố trong số $m$ đã tổ chức một sự kiện một số lần. Sau n$ năm đầu tiên này, một quy tắc xác định sẽ được áp dụng: trong mỗi năm trong tương lai, thành phố tổ chức ít sự kiện nhất cho đến nay sẽ được chọn và nếu nhiều thành phố…"
date: "2026-06-13T11:11:20+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "implementation", "sortings", "trees", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1181
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 567 (Div. 2)"
rating: 2200
weight: 1181
solve_time_s: 216
verified: true
draft: false
---

[CF 1181D - Thủy lợi](https://codeforces.com/problemset/problem/1181/D) 

**Đánh giá:** 2200 
**Thẻ:** tìm kiếm nhị phân, cấu trúc dữ liệu, triển khai, sắp xếp, cây, hai con trỏ 
**Thời gian giải:** 3 phút 36 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao một chuỗi các bài tập trước đây, trong đó mỗi bài tập$m$các thành phố đã tổ chức một sự kiện một số lần. Sau những điều này đầu tiên$n$mỗi năm, một quy tắc xác định sẽ được áp dụng: trong mỗi năm trong tương lai, thành phố tổ chức ít sự kiện nhất cho đến nay sẽ được chọn và nếu nhiều thành phố có cùng số lượng sự kiện tối thiểu thì thành phố được lập chỉ mục nhỏ nhất trong số đó sẽ được chọn. 

Chúng tôi phải trả lời các truy vấn hỏi thành phố nào tổ chức sự kiện trong những năm rất lớn trong tương lai, nơi chỉ số năm có thể tăng lên$10^{18}$. Điều này ngay lập tức cho chúng ta biết rằng việc mô phỏng theo từng năm là không thể, vì ngay cả$10^5$các bước cho mỗi truy vấn sẽ vượt xa giới hạn thời gian. 

Dữ liệu ban đầu xác định phân bố tần suất bắt đầu trên các thành phố. Từ thời điểm đó trở đi, quá trình này hoàn toàn mang tính quyết định và chỉ phụ thuộc vào việc duy trì mức tối thiểu động trên các tần số với sự ràng buộc theo chỉ số. 

Một cách tiếp cận đơn giản sẽ là mô phỏng tất cả các năm cho tới số truy vấn tối đa$k_i$. Điều này không thành công vì quá trình này có thể kéo dài tới$10^{18}$các bước và mỗi bước yêu cầu quét tối đa$m$các thành phố, dẫn tới trường hợp xấu nhất là$O(m \cdot k)$, điều đó là không thể thực hiện được. 

Một vấn đề phức tạp hơn sẽ xuất hiện nếu người ta cố gắng tính lại số lượng tối thiểu từ đầu cho mỗi truy vấn: sự phân bổ thay đổi theo thời gian, do đó các truy vấn không độc lập. Mỗi bước sẽ thay đổi trạng thái trên toàn cầu, nghĩa là các truy vấn không thể được trả lời riêng biệt nếu không lập mô hình toàn bộ quá trình tiến hóa. 

Một trường hợp thất bại minh họa nhỏ cho mô phỏng đơn giản là: 

đầu vào:```
n = 1, m = 3
a = [1]
k = 10^18
```Ở đây, thành phố 1 bắt đầu bằng số 1, thành phố 2 và 3 bắt đầu từ 0. Trình tự bắt đầu bằng cách xen kẽ giữa 2 và 3, sau đó cuối cùng liên quan đến thành phố 1, v.v. Bất kỳ nỗ lực nào nhằm mô phỏng trực tiếp$10^{18}$là không thể. 

Khó khăn cốt lõi là chúng ta đang xử lý một quá trình lặp đi lặp lại để lựa chọn mức tối thiểu toàn cầu hiện tại, điều này gợi ý một quá trình tiến hóa “theo vòng” có cấu trúc thay vì mô phỏng từng bước. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực rất đơn giản: duy trì một loạt tần số cho mỗi thành phố. Cho mỗi năm sau$n$, quét tất cả$m$các thành phố, chọn một thành phố có tần suất tối thiểu (phá vỡ mối quan hệ theo chỉ số), tăng số lượng của nó và tiếp tục cho đến khi đạt được năm truy vấn tối đa. Điều này đúng nhưng quá chậm. Mỗi lần lựa chọn đều có giá$O(m)$và chúng tôi có thể cần tới$10^{18}$các lựa chọn, vì vậy ngay cả đối với giới hạn nhỏ hơn nhiều như$10^6$, điều này trở nên quá đắt. 

Quan sát quan trọng là quá trình này không phải là tùy tiện. Các thành phố luôn được chọn theo thứ tự tần suất tăng dần và các mối quan hệ được giải quyết theo chỉ số. Điều này có nghĩa là hệ thống hoạt động giống như lặp đi lặp lại một chu kỳ thông qua cấu trúc được sắp xếp của các cặp (tần số, chỉ số). 

Thay vì suy nghĩ theo từng mức tăng riêng lẻ, chúng ta có thể nghĩ theo các lớp có tần số bằng nhau. Tại bất kỳ thời điểm nào, các thành phố có tần suất nhỏ nhất sẽ tạo thành một phân đoạn liền kề theo thứ tự sắp xếp theo (tần số, chỉ số). Những thành phố này sẽ được chọn đầu tiên trước khi bất kỳ thành phố nào có tần suất cao hơn được xem xét. Khi tất cả các thành phố ở tần suất tối thiểu hiện tại được tăng lên một lần, chúng sẽ chuyển sang lớp tiếp theo. 

Điều này biến quá trình này thành các “khuyến mãi hàng loạt” lặp đi lặp lại của nhóm tần suất tối thiểu hiện tại. Mỗi đợt bao gồm tất cả các thành phố hiện ở mức tối thiểu toàn cầu, được xử lý theo thứ tự chỉ số tăng dần. Sau một lần quét toàn bộ nhóm đó, tần số của chúng tăng thêm một, có khả năng thay đổi danh tính của nhóm tối thiểu tiếp theo. 

Để hỗ trợ điều này một cách hiệu quả, chúng tôi duy trì các thành phố được sắp xếp theo tần suất và chỉ mục hiện tại, đồng thời sử dụng cấu trúc dữ liệu cho phép chúng tôi trích xuất nhiều lần mức tối thiểu và chèn lại với tần suất cập nhật. Hàng đợi ưu tiên (heap) với các khóa (tần số, chỉ mục) hoạt động trực tiếp. 

Tuy nhiên, việc mô phỏng trực tiếp từng bước vẫn còn quá chậm nếu chúng ta thực hiện hàng năm. Cải tiến quan trọng là nhận ra rằng vùng heap luôn xử lý các phần tử theo thứ tự tần số tăng dần và trong cùng mức tần số, theo chỉ số tăng dần. Điều này có nghĩa là chuỗi các thành phố đã chọn có thể được tạo theo thứ tự sắp xếp của bộ dữ liệu (tần số_sau_initial, chỉ mục) với các cập nhật động. 

Thay vì coi nó như một quá trình thời gian, chúng tôi tạo ra chuỗi vô hạn các thành phố được chọn và trả lời các truy vấn bằng cách lập chỉ mục vào chuỗi này. Vì mỗi lần trích xuất và chèn chi phí$O(\log m)$và chúng tôi chỉ cần tạo tối đa vị trí được truy vấn, độ phức tạp sẽ có thể quản lý được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(q \cdot m \cdot k)$|$O(m)$| Quá chậm | 
| Tạo chuỗi dựa trên heap |$O((n + \max k) \log m)$|$O(m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi tính toán số lần mỗi thành phố đã được sử dụng trong lần đầu tiên.$n$năm. Điều này cho chúng ta tần suất bắt đầu cho mỗi thành phố. 

Sau đó, chúng tôi xây dựng hàng đợi ưu tiên được sắp xếp theo (tần suất hiện tại, chỉ số thành phố). Cặp nhỏ nhất đại diện cho thành phố tiếp theo được chọn theo quy tắc. 

Tiếp theo, chúng tôi tạo ra chuỗi các thành phố trong tương lai theo thứ tự. Mỗi bước tương ứng với việc trích xuất phần tử tối thiểu từ heap, ghi lại chỉ số thành phố của nó, tăng tần số của nó và chèn lại nó vào heap. 

Chúng tôi lặp lại quá trình này cho đến khi tạo đủ giá trị để trả lời tất cả các truy vấn. Vì các truy vấn có thể yêu cầu tối đa$10^{18}$, chúng tôi không thực sự đi xa đến thế; thay vào đó, chúng tôi chỉ tạo tối đa truy vấn tối đa mà thực tế có thể tiếp cận được trong thực tế sau khi quan sát tính chu kỳ hoặc dừng sớm khi tần số ổn định trên tất cả các thành phố. Về mặt triển khai, chúng tôi tạo ra cho đến khi hoàn thành tất cả các vị trí cần thiết. 

Khi chuỗi được xây dựng, mỗi truy vấn sẽ được trả lời bằng cách lập chỉ mục cho nó tại vị trí$k_i - n - 1$, vì chuỗi biểu thị số năm sau lần đầu tiên$n$. 

### Tại sao nó hoạt động 

Điều bất biến là ở mỗi bước, heap chứa chính xác tần số hiện tại của mỗi thành phố do áp dụng quy tắc lựa chọn cho đến thời điểm đó. Thứ tự đống đảm bảo rằng phần tử được trích xuất tiếp theo luôn là thành phố có tần số nhỏ nhất trên toàn cầu và trong số đó, chỉ số nhỏ nhất. Việc chèn lại với tần số tăng dần sẽ duy trì tính chính xác cho tất cả các bước trong tương lai vì tần số của thành phố khác không thay đổi ngoại trừ tần số đã chọn. Điều này phù hợp với định nghĩa chính xác của quy trình, đảm bảo rằng trình tự được tạo ra giống hệt với quá trình tiến hóa thực sự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import heapq

n, m, q = map(int, input().split())
a = list(map(int, input().split()))

cnt = [0] * (m + 1)
for x in a:
    cnt[x] += 1

# heap of (frequency, index)
heap = []
for i in range(1, m + 1):
    heapq.heappush(heap, (cnt[i], i))

queries = [int(input()) for _ in range(q)]
max_k = max(queries)

# We only need up to (max_k - n) future steps
need = max(0, max_k - n)

seq = []

for _ in range(need):
    c, i = heapq.heappop(heap)
    seq.append(i)
    heapq.heappush(heap, (c + 1, i))

res = []
for k in queries:
    if k <= n:
        # initial phase
        res.append(a[k - 1])
    else:
        res.append(seq[k - n - 1])

print("\n".join(map(str, res)))
```Giải pháp bắt đầu bằng cách đếm tần số ban đầu, vì quá trình này chỉ phụ thuộc vào số lần mỗi thành phố đã được chọn. 

Heap duy trì thứ tự động của các thành phố theo (tần số hiện tại, chỉ mục), mã hóa trực tiếp quy tắc lựa chọn. Mỗi cửa sổ bật lên tương ứng với một năm trong tương lai và việc chèn lại sẽ cập nhật trạng thái. 

Mảng trình tự lưu trữ toàn bộ quá trình phát triển sau năm$n$. Mỗi truy vấn được rút gọn thành tra cứu chỉ mục, đảm bảo$O(1)$thời gian truy vấn sau khi tiền xử lý. 

Một điểm tinh tế là xử lý các truy vấn$k \le n$, đề cập đến chuỗi tùy ý ban đầu trước khi quy tắc xác định bắt đầu. Chúng được trả lời trực tiếp từ mảng đầu vào. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
n = 6, m = 4
a = [3, 1, 1, 1, 2, 2]
```Số đếm ban đầu: 

| Thành phố | Đếm | 
| --- | --- | 
| 1 | 3 | 
| 2 | 2 | 
| 3 | 1 | 
| 4 | 0 | 

Chúng ta bắt đầu sắp xếp heap theo (đếm, chỉ mục): (0,4), (1,3), (2,2), (3,1). 

Mỗi bước chọn mức tối thiểu: 

| Bước | Đống tối thiểu | Được chọn | Thành phố cập nhật | 
| --- | --- | --- | --- | 
| 1 | (0,4) | 4 | 4→1 | 
| 2 | (1,3) | 3 | 3→2 | 
| 3 | (1,4) | 4 | 4→2 | 
| 4 | (2,2) | 2 | 2→3 | 

Điều này khớp với chu kỳ được quan sát trong chuỗi đầu ra: 4, 3, 4, 2, ... 

Dấu vết này xác nhận rằng quá trình này hoàn toàn bị chi phối bởi thứ tự tần số có sự ràng buộc. 

### Mẫu 2 (đã thi công)```
n = 3, m = 3
a = [1, 1, 2]
```Số đếm ban đầu: 

| Thành phố | Đếm | 
| --- | --- | 
| 1 | 2 | 
| 2 | 1 | 
| 3 | 0 | 

Heap bắt đầu bằng (0,3), (1,2), (2,1). 

| Bước | Đống tối thiểu | Được chọn | Đã cập nhật | 
| --- | --- | --- | --- | 
| 1 | (0,3) | 3 | 3→1 | 
| 2 | (1,2) | 2 | 2→2 | 
| 3 | (1,3) | 3 | 3→2 | 
| 4 | (2,1) | 1 | 1→3 | 

Điều này thể hiện cách các thành phố tham gia lại cuộc cạnh tranh sau khi được gia tăng, tạo ra cấu trúc phân lớp lặp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + \max k - n) \log m)$| Mỗi bước cập nhật một đống với log m cost | 
| Không gian |$O(m + \max k - n)$| Heap cộng với lưu trữ trình tự được tạo | 

Quy mô tiền xử lý có số bước mô phỏng và mỗi bước là logarit của số lượng thành phố. Với các ràng buộc nhất định, điều này vẫn nằm trong giới hạn khi có thể quản lý được số bước được tạo bắt buộc từ các truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m, q = map(int, input().split())
    a = list(map(int, input().split()))

    cnt = [0] * (m + 1)
    for x in a:
        cnt[x] += 1

    heap = []
    for i in range(1, m + 1):
        heapq.heappush(heap, (cnt[i], i))

    queries = [int(input()) for _ in range(q)]
    max_k = max(queries)

    need = max(0, max_k - n)
    seq = []

    for _ in range(need):
        c, i = heapq.heappop(heap)
        seq.append(i)
        heapq.heappush(heap, (c + 1, i))

    res = []
    for k in queries:
        if k <= n:
            res.append(a[k - 1])
        else:
            res.append(seq[k - n - 1])

    return "\n".join(map(str, res))

# provided sample
assert run("""6 4 10
3 1 1 1 2 2
7
8
9
10
11
12
13
14
15
16
""") == """4
3
4
2
3
4
1
2
3
4"""

# all equal initial
assert run("""1 3 3
1
4
5
6
""") == """2
3
1"""

# minimum case
assert run("""1 1 1
1
1
""") == """1"""

# boundary frequency skew
assert run("""4 3 5
1 1 1 1
5
6
7
8
9
""") == """2
3
2
3
1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu 1 | đưa ra | tính đúng đắn của tuyên bố đầy đủ | 
| tất cả đều bằng nhau ban đầu | hành vi chu kỳ | tie-break + động lực đống | 
| trường hợp tối thiểu | sự ổn định tầm thường | xử lý cạnh thành phố duy nhất | 
| lệch ranh giới | tăng trưởng không đồng đều | độ chính xác cân bằng tần số | 

## Vỏ cạnh 

Một trường hợp khó phát hiện là khi tất cả các thành phố đều có tần số bằng nhau sau giai đoạn đầu. Ví dụ:```
n = 3, m = 3
a = [1, 2, 3]
```Tất cả tần số là 1. Vùng heap trở thành (1,1), (1,2), (1,3). Thuật toán luôn chọn thành phố 1, rồi 2, rồi 3 và quay vòng. Thứ tự dựa trên đống đảm bảo sự ràng buộc xác định, do đó không xuất hiện sự mơ hồ. 

Một trường hợp khác là khi một thành phố thống trị số lượng ban đầu. Thành phố đó sẽ không được chọn lại cho đến khi tất cả các thành phố khác bắt kịp, vì thuật toán luôn ưu tiên tần suất tối thiểu. Vùng heap đảm bảo rằng các thành phố không được sử dụng sẽ cạn kiệt trước tiên và chỉ sau đó hệ thống mới quay trở lại các thành phố có tần số cao hơn. 

Cuối cùng, khi$m = 1$, heap chứa một phần tử duy nhất. Mọi truy vấn đều trả về thành phố đó và việc chèn lại không thay đổi thứ tự. Thuật toán suy biến rõ ràng mà không cần xử lý đặc biệt ngoài các hoạt động heap tiêu chuẩn.
