---
title: "CF 1006E - Vấn đề quân sự"
description: "Chúng ta được cấp một cây sĩ quan gốc trong đó mỗi sĩ quan có một cấp trên trực tiếp duy nhất ngoại trừ sĩ quan gốc 1. Điều này tạo ra một hệ thống phân cấp trong đó mỗi nút đại diện cho một sĩ quan và các cạnh từ cấp trên đến cấp dưới."
date: "2026-06-16T23:11:42+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs", "trees"]
categories: ["algorithms"]
codeforces_contest: 1006
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 498 (Div. 3)"
rating: 1600
weight: 1006
solve_time_s: 86
verified: true
draft: false
---

[CF 1006E - Vấn đề quân sự](https://codeforces.com/problemset/problem/1006/E) 

**Đánh giá:** 1600 
**Thẻ:** dfs và tương tự, đồ thị, cây 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây sĩ quan gốc trong đó mỗi sĩ quan có một cấp trên trực tiếp duy nhất ngoại trừ sĩ quan gốc 1. Điều này tạo ra một hệ thống phân cấp trong đó mỗi nút đại diện cho một sĩ quan và các cạnh từ cấp trên đến cấp dưới. Mỗi truy vấn yêu cầu chúng tôi mô phỏng một quy trình truyền tải rất cụ thể bắt đầu từ một sĩ quan nhất định và sau đó báo cáo sĩ quan nào là người thứ k nhận được lệnh trong quá trình truyền tải đó. 

Quy tắc truyền tải không phải là thứ tự trước tiêu chuẩn được viết theo thứ tự đầu vào. Thay vào đó, mỗi sĩ quan lần lượt truyền lệnh cho cấp dưới trực tiếp của mình, luôn chọn cấp dưới có chỉ số nhỏ nhất chưa được xử lý. Mỗi cấp dưới được chọn hoàn thành đầy đủ cây con của riêng mình trước khi sĩ quan hiện tại tiếp tục với cấp dưới tiếp theo. Đây thực sự là một tìm kiếm theo chiều sâu, trong đó trẻ em được truy cập theo thứ tự chỉ số tăng dần. 

Đầu ra cho mỗi truy vấn là nút thứ k theo thứ tự DFS này bắt đầu từ một gốc nhất định của quá trình truyền tải hoặc -1 nếu có ít hơn k nút được truy cập. 

Các ràng buộc rất lớn, lên tới 200.000 nút và 200.000 truy vấn. Bất kỳ giải pháp nào mô phỏng DFS cho mỗi truy vấn sẽ quá chậm vì một lần truyền tải có thể mất O(n) thời gian và việc thực hiện điều đó cho mọi truy vấn sẽ dẫn đến O(nq), điều này hoàn toàn không khả thi. Ngay cả việc lưu trữ toàn bộ mảng truyền tải cho từng nút một cách độc lập cũng sẽ quá nặng về bộ nhớ. 

Cấu trúc của bài toán đề xuất rõ ràng việc xử lý trước thứ tự DFS toàn cục của cây, vì mọi truy vấn đều sử dụng cùng một quy tắc truyền tải xác định. Thách thức chính là chúng ta phải nhanh chóng hạn chế sự chú ý đến cây con của một nút nhất định và trả lời các truy vấn vị trí thứ k bên trong phân đoạn đó. 

Trường hợp cạnh tinh tế xuất hiện khi k vượt quá kích thước của cây con. Ví dụ: nếu một nút chỉ có hai nút con trong phạm vi DFS của nó và k = 5 thì câu trả lời đúng là -1. Một trường hợp khác là khi một nút là một lá, trong trường hợp đó danh sách truyền tải của nó chỉ là chính nó nên chỉ có k = 1 là hợp lệ. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sẽ xây dựng danh sách truyền tải bắt đầu từ nút truy vấn bằng cách chạy DFS và nối thêm các nút khi chúng được truy cập. Điều này đúng vì quy tắc truyền tải có tính xác định và khớp DFS với các danh sách kề được sắp xếp. Tuy nhiên, mỗi truy vấn có thể yêu cầu duyệt toàn bộ cây con, trong trường hợp xấu nhất là O(n). Với tối đa 2e5 truy vấn, điều này trở thành O(nq), vượt xa mọi giới hạn khả thi. 

Quan sát quan trọng là việc duyệt DFS trên toàn bộ cây, nếu chúng ta cố định các cây con theo thứ tự tăng dần, sẽ tạo ra một thứ tự toàn cục trong đó mỗi cây con tương ứng với một đoạn liền kề. Đây là thuộc tính giống như chuyến tham quan Euler cổ điển của thứ tự DFS. Khi chúng ta tính toán thứ tự này một lần, mỗi cây con sẽ trở thành một phần của mảng này. 

Để hỗ trợ các truy vấn nhanh, chúng tôi ghi lại cho mỗi nút thời gian vào khi DFS truy cập lần đầu và thời gian thoát sau khi kết thúc cây con của nó. Khi đó toàn bộ cây con của nút u tương ứng với một đoạn liền kề trong mảng DFS từ tin[u] đến tout[u]. Nút được truy cập thứ k trong quá trình truyền tải từ u chỉ đơn giản là phần tử thứ (tin[u] + k - 1)-trong mảng này, miễn là nó không vượt quá tout[u]. 

Do đó, quá trình tiền xử lý sẽ xây dựng một đơn hàng DFS duy nhất và lưu trữ các vị trí đầu vào. Mỗi truy vấn trở thành một phép kiểm tra số học O(1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DFS cho mỗi truy vấn | O(nq) | O(n) | Quá chậm | 
| Thứ tự DFS được tính toán trước + lập chỉ mục | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách chuyển việc duyệt cây con thành lập chỉ mục phạm vi theo thứ tự DFS được tính toán trước.

1. Xây dựng cây bằng danh sách kề và sắp xếp các nút con của mỗi nút bằng cách tăng chỉ số. Điều này đảm bảo thứ tự duyệt phù hợp với quy tắc luôn chọn cấp dưới nhỏ nhất có sẵn trước tiên. 
2. Chạy một DFS duy nhất bắt đầu từ nút 1, duy trì bộ đếm thời gian chung và một mảng`order`. Mỗi lần chúng tôi nhập một nút, chúng tôi ghi lại thời gian nhập của nút đó và thêm nó vào`order`. Điều này ghi lại thời điểm chính xác mà nút được truy cập trong quá trình truyền tải. 
3. Sau khi truy cập tất cả các nút con của một nút, chúng tôi ghi lại thời gian thoát của nó. Khoảng thời gian`[tin[u], tout[u]]`bây giờ thể hiện chính xác phân đoạn của thứ tự DFS thuộc cây con của u. 
4. Đối với mỗi truy vấn`(u, k)`, tính chỉ số`pos = tin[u] + k - 1`. Nếu như`pos`lớn hơn`tout[u]`, cây con không chứa k phần tử nên trả về -1. 
5. Nếu không thì trả lại`order[pos]`, là nút được truy cập thứ k trong quá trình truyền tải DFS của bạn. 

Lý do hoạt động của nó xuất phát từ thuộc tính cấu trúc của DFS trên cây: khi một nút được nhập, tất cả các nút trong cây con của nó sẽ được truy cập trước khi quay về nút gốc của nó và vì các nút con được xử lý theo thứ tự được sắp xếp nên chuỗi toàn cục thu được sẽ tuân thủ chính xác quy tắc trải rộng bắt buộc. Điều này làm cho mỗi cây con là một khoảng thời gian liên tục trong quá trình truyền tải toàn cục, do đó các truy vấn vị trí giảm xuống việc lập chỉ mục đơn giản. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

n, q = map(int, input().split())
p = list(map(int, input().split()))

g = [[] for _ in range(n + 1)]
for i, par in enumerate(p, start=2):
    g[par].append(i)

for i in range(1, n + 1):
    g[i].sort()

order = []
tin = [0] * (n + 1)
tout = [0] * (n + 1)
timer = 0

def dfs(u):
    global timer
    tin[u] = timer
    order.append(u)
    timer += 1

    for v in g[u]:
        dfs(v)

    tout[u] = timer - 1

dfs(1)

for _ in range(q):
    u, k = map(int, input().split())
    pos = tin[u] + k - 1
    if pos > tout[u]:
        print(-1)
    else:
        print(order[pos])
```DFS xây dựng một biểu diễn tuyến tính duy nhất của quá trình truyền tải. Mảng`order`chính xác là trình tự trong đó các sĩ quan nhận được lệnh trong một quá trình truyền tải đầy đủ bắt đầu từ gốc. Các mảng`tin`Và`tout`xác định phân đoạn của chuỗi này tương ứng với mỗi cây con. 

Việc sắp xếp danh sách kề là cần thiết vì nếu không có nó thì thứ tự duyệt sẽ phụ thuộc vào thứ tự đầu vào thay vì quy tắc bắt buộc phải có chỉ mục đầu tiên tối thiểu. 

Mỗi truy vấn được trả lời bằng cách chuyển đổi vị trí thứ k thành chỉ mục trực tiếp theo thứ tự chung này. Việc kiểm tra ranh giới đảm bảo chúng tôi không truy cập bên ngoài phân đoạn cây con. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một hệ thống phân cấp nhỏ:```
1 -> {2, 3}
2 -> {4}
3 -> {}
4 -> {}
```Thứ tự DFS là`[1, 2, 4, 3]`. 

| Bước | Hành động | đặt hàng | tin/tout cập nhật | 
| --- | --- | --- | --- | 
| 1 | thăm 1 | [1] | thiếc[1]=0 | 
| 2 | đi đến 2 | [1,2] | thiếc[2]=1 | 
| 3 | đi đến 4 | [1,2,4] | thiếc[4]=2 | 
| 4 | trở lại, đi đến 3 | [1,2,4,3] | thiếc[3]=3 | 

Bây giờ truy vấn`(2,2)`yêu cầu nút thứ hai trong cây con của 2. Thứ tự cây con là`[2,4]`, vậy đáp án là 4. Việc tính toán sử dụng`tin[2]=1`, vì vậy vị trí là 2 khớp với chỉ số 2 trong mảng DFS. 

### Ví dụ 2 

Cây dạng chuỗi:```
1 -> 2 -> 3 -> 4
```Thứ tự DFS là`[1,2,3,4]`. 

Đối với truy vấn`(3,1)`, chúng tôi nhận được`tin[3]=2`, vậy vị trí là 2 và câu trả lời là`order[2]=3`. Vì`(3,2)`, vị trí trở thành 3, hợp lệ và trả về 4. Đối với`(3,3)`, vị trí vượt quá`tout[3]=3`, vì vậy đầu ra là -1. 

Điều này xác nhận rằng các phân đoạn cây con hoạt động như các khoảng liền kề nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | DFS truy cập mỗi nút một lần, mỗi truy vấn là O(1) | 
| Không gian | O(n) | danh sách kề, thứ tự DFS và mảng thời gian | 

Quá trình tiền xử lý là tuyến tính theo kích thước của cây và mỗi truy vấn được giải quyết bằng số học không đổi, phù hợp thoải mái trong giới hạn cho n, q lên tới 200.000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    import subprocess, textwrap
    return subprocess.run(
        ["python3", "-c", solution_code],
        input=inp.encode(),
        stdout=subprocess.PIPE
    ).stdout.decode().strip()

solution_code = r"""
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

n, q = map(int, input().split())
p = list(map(int, input().split()))

g = [[] for _ in range(n + 1)]
for i, par in enumerate(p, start=2):
    g[par].append(i)

for i in range(1, n + 1):
    g[i].sort()

order = []
tin = [0] * (n + 1)
tout = [0] * (n + 1)
timer = 0

def dfs(u):
    global timer
    tin[u] = timer
    order.append(u)
    timer += 1
    for v in g[u]:
        dfs(v)
    tout[u] = timer - 1

dfs(1)

for _ in range(q):
    u, k = map(int, input().split())
    pos = tin[u] + k - 1
    if pos > tout[u]:
        print(-1)
    else:
        print(order[pos])
"""

# sample test (structure only placeholder since full sample omitted)
# assert run("...") == "..."
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây xích k=1 | tự sửa sai | lá và ranh giới | 
| cây con k tràn | -1 | ngoài tầm k | 
| cây cân bằng | DFS đúng | đặt hàng đúng đắn | 

## Vỏ cạnh 

Truy vấn nút lá kiểm tra xem thuật toán có xử lý chính xác các cây con một nút hay không. Từ`tin[u] == tout[u]`, chỉ một`k = 1`các bản đồ bên trong khoảng hợp lệ và bất kỳ k lớn hơn nào đều không thực hiện được việc kiểm tra phạm vi. 

Chuỗi sâu kiểm tra xem thứ tự DFS có phù hợp với việc cắt cây con hay không. Mặc dù các nút được lồng nhau, thuộc tính khoảng liền kề vẫn giữ nguyên, đảm bảo lập chỉ mục chính xác. 

Một nút có nhiều nút con đảm bảo việc sắp xếp là cần thiết. Nếu không sắp xếp các danh sách kề, thứ tự duyệt sẽ không khớp với quy tắc chỉ mục đầu tiên tối thiểu được yêu cầu, dẫn đến trình tự DFS không chính xác và câu trả lời sai cho tất cả các truy vấn liên quan đến thứ tự anh em.
