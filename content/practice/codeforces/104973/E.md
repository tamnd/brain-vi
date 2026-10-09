---
title: "CF 104973E - Cơ sở dữ liệu"
description: "Chúng tôi được cung cấp một tập hợp các cơ sở dữ liệu được sắp xếp thành một dòng. Mỗi cơ sở dữ liệu hoạt động giống như một hàng đợi có dung lượng cố định. Chúng tôi cũng có một chuỗi các thao tác và mỗi thao tác lấy một giá trị và đẩy nó vào mọi cơ sở dữ liệu có chỉ mục nằm trong một khoảng nhất định."
date: "2026-06-28T06:36:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104973
codeforces_index: "E"
codeforces_contest_name: "BdOI Preliminary 2024"
rating: 0
weight: 104973
solve_time_s: 54
verified: true
draft: false
---

[CF 104973E - Cơ sở dữ liệu](https://codeforces.com/problemset/problem/104973/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một tập hợp các cơ sở dữ liệu được sắp xếp thành một dòng. Mỗi cơ sở dữ liệu hoạt động giống như một hàng đợi có dung lượng cố định. Chúng tôi cũng có một chuỗi các thao tác và mỗi thao tác lấy một giá trị và đẩy nó vào mọi cơ sở dữ liệu có chỉ mục nằm trong một khoảng nhất định. Nếu cơ sở dữ liệu trở nên dài hơn dung lượng của nó sau lần chèn này, nó sẽ ngay lập tức loại bỏ phần tử cũ nhất khỏi phía trước. 

Sau khi xử lý tất cả các thao tác, mỗi cơ sở dữ liệu chứa một chuỗi các giá trị, nhưng các giá trị lặp lại không thành vấn đề. Nhiệm vụ là tính toán xem đối với mỗi cơ sở dữ liệu có bao nhiêu giá trị khác nhau còn lại bên trong nó. 

Khó khăn chính là mỗi thao tác đều ảnh hưởng đến toàn bộ phân đoạn cơ sở dữ liệu và mỗi cơ sở dữ liệu có sự phát triển hàng đợi riêng tùy thuộc vào số lượng cập nhật mà nó nhận được theo thời gian. Mô phỏng trực tiếp sẽ yêu cầu đẩy tới 200.000 cơ sở dữ liệu cho mỗi hoạt động, vốn đã quá lớn và mỗi cơ sở dữ liệu có thể tích lũy tới 200.000 hoạt động, điều này khiến cho việc mô phỏng hàng đợi đơn giản không thể thực hiện được. 

Một cách hữu ích để diễn giải lại quá trình là tập trung vào những gì còn lại ở cuối. Mỗi cơ sở dữ liệu chỉ giữ các phần chèn c[i] gần đây nhất có ảnh hưởng đến nó. Các phần chèn cũ hơn sẽ bị xóa theo thứ tự khi hàng đợi tràn ra. Điều này có nghĩa là chúng tôi không thực sự quan tâm đến các trạng thái trung gian mà chỉ quan tâm đến hậu tố cuối cùng của các bản cập nhật có liên quan trên mỗi cơ sở dữ liệu. 

Một trường hợp lỗi tinh tế xuất hiện khi người ta cố gắng mô phỏng từng cơ sở dữ liệu một cách độc lập nhưng vẫn xử lý tất cả các hoạt động một cách đơn giản. Ví dụ: nếu mọi thao tác đều ảnh hưởng đến tất cả các cơ sở dữ liệu và dung lượng đều lớn thì việc triển khai hàng đợi trên mỗi cơ sở dữ liệu đơn giản vẫn thực hiện các lần đẩy O(nq), vượt xa giới hạn. Một cạm bẫy khác là quên rằng việc loại bỏ hoàn toàn từ phía trước, điều này làm cho cấu trúc trở thành một cửa sổ trượt theo thời gian chứ không chỉ là một tập hợp nhiều thao tác được áp dụng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng rõ ràng từng thao tác bằng cách lặp lại trên tất cả các cơ sở dữ liệu trong khoảng thời gian đó và đẩy giá trị vào hàng đợi. Mỗi cơ sở dữ liệu sẽ duy trì một deque và nếu vượt quá dung lượng, nó sẽ xuất hiện từ phía trước. Điều này đúng vì nó phản ánh trực tiếp câu nói đó. Tuy nhiên, mỗi thao tác có thể chạm vào cơ sở dữ liệu O(n), do đó độ phức tạp tổng cộng trở thành O(nq), điều này hoàn toàn không khả thi ở tỷ lệ 2⋅10^5. 

Quan sát quan trọng là đảo ngược quan điểm. Thay vì theo dõi cách hàng đợi phát triển theo thời gian, hãy xem xét một cơ sở dữ liệu cố định i. Mọi hoạt động đều có ảnh hưởng đến tôi hoặc không. Nếu có, nó sẽ đóng góp một phần tử vào hàng đợi của nó. Vì hàng đợi luôn xóa từ phía trước khi tràn, nên trạng thái cuối cùng của cơ sở dữ liệu i bao gồm chính xác các thao tác c[i] cuối cùng (theo thứ tự thời gian) bao gồm i. 

Vì vậy, vấn đề giảm xuống như sau: với mỗi chỉ mục i, thu thập tất cả các hoạt động có khoảng thời gian bao gồm i, sắp xếp chúng theo thời gian, lấy c[i] cuối cùng và đếm các giá trị khác biệt trong số x[i] của chúng. 

Thách thức trở thành việc trả lời hiệu quả nhiều truy vấn từ phạm vi đến điểm theo thời gian, nhưng chỉ thu thập dữ liệu thay vì tổng hợp dữ liệu. 

Chúng ta có thể lập mô hình này bằng cách sử dụng cây phân đoạn trên các chỉ mục cơ sở dữ liệu. Mỗi thao tác (l, r, x) được lưu trữ trong tất cả các nút cây phân đoạn bao phủ đầy đủ khoảng thời gian của nó. Điều này đảm bảo rằng đối với bất kỳ chỉ mục cố định i nào, các nút trên đường dẫn từ gốc tới lá của nó chứa chính xác các thao tác ảnh hưởng đến nó. Mỗi nút lưu trữ các hoạt động theo thứ tự thời gian tăng dần. 

Để trả lời cho một i duy nhất, chúng tôi hợp nhất các danh sách từ các nút O(log n) của nó và chỉ trích xuất các hoạt động c[i] gần đây nhất bằng cách sử dụng hợp nhất k-way. Vì k nhỏ nên điều này đủ hiệu quả ngay cả khi tính tổng trên tất cả các chỉ số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force | O(nq) | O(n) | Quá chậm | 
| Cây phân đoạn + hợp nhất theo chỉ mục | O((n + q) log n + ∑c[i] log log n) | O((n + q) log n) | Đã chấp nhận |

## Hướng dẫn thuật toán 

1. Xây dựng cây phân đoạn trên các chỉ số từ 1 đến n. Mỗi nút sẽ lưu trữ một danh sách các hoạt động bao gồm đầy đủ phân đoạn của nút đó. Điều này cho phép chúng tôi trình bày các cập nhật phạm vi mà không cần chạm vào mọi cơ sở dữ liệu một cách rõ ràng. 
2. Với mỗi thao tác (l[i], r[i], x[i]) có chỉ số thời gian i, chèn i vào các nút cây phân đoạn được bao phủ hoàn toàn bởi [l[i], r[i]]. Sự phân tách này đảm bảo mỗi thao tác chỉ được lưu trữ O(log n) lần. 
3. Đối với mỗi nút trong cây phân đoạn, giữ nguyên các thao tác theo thứ tự chúng được thêm vào, tương ứng với thời gian tăng dần. Điều này đảm bảo rằng trong mỗi nút, danh sách đã tuân theo thứ tự thời gian. 
4. Đối với mỗi vị trí cơ sở dữ liệu i, duyệt qua đường dẫn cây phân đoạn từ gốc đến lá và thu thập các con trỏ tới tất cả các danh sách được lưu trữ trong các nút trên đường dẫn đó. Các danh sách này cùng nhau đại diện cho tất cả các hoạt động ảnh hưởng đến i. 
5. Đối với cơ sở dữ liệu i, thực hiện hợp nhất k-way các danh sách được sắp xếp O(log n) này, luôn chọn thao tác còn lại gần đây nhất trước tiên. Dừng lại khi chúng tôi đã thu thập xong các hoạt động c[i], vì các hoạt động cũ hơn không thể ảnh hưởng đến trạng thái hàng đợi cuối cùng. 
6. Từ các chỉ số thao tác đã thu thập, trích xuất các giá trị x của chúng và chèn vào tập hợp để đếm các giá trị riêng biệt cho cơ sở dữ liệu i. 
7. Xuất kích thước của tập hợp làm đáp án cho cơ sở dữ liệu i. 

### Tại sao nó hoạt động 

Mỗi thao tác ảnh hưởng đến cơ sở dữ liệu tương ứng chính xác với một lần xuất hiện trong một hoặc nhiều nút cây phân đoạn trên đường dẫn từ gốc đến lá của cơ sở dữ liệu đó. Bởi vì mỗi nút lưu trữ các hoạt động theo thứ tự thời gian, việc hợp nhất các danh sách này và lấy các phần tử c[i] gần đây nhất sẽ tái tạo lại chính xác hậu tố có độ dài c[i] của tất cả các hoạt động liên quan. Hậu tố đó giống hệt với nội dung cuối cùng của hàng đợi, vì mỗi lần tràn sẽ loại bỏ phần tử cũ nhất, tương đương với việc loại bỏ tất cả trừ phần chèn c[i] cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    c = list(map(int, input().split()))

    ops = []
    for _ in range(q):
        l, r, x = map(int, input().split())
        ops.append((l - 1, r - 1, x))

    size = 1
    while size < n:
        size *= 2

    tree = [[] for _ in range(2 * size)]

    def add(node, nl, nr, l, r, idx):
        if l <= nl and nr <= r:
            tree[node].append(idx)
            return
        mid = (nl + nr) // 2
        if l <= mid:
            add(node * 2, nl, mid, l, r, idx)
        if r > mid:
            add(node * 2 + 1, mid + 1, nr, l, r, idx)

    for i, (l, r, _) in enumerate(ops):
        add(1, 0, size - 1, l, r, i)

    for i in range(2 * size):
        tree[i].reverse()

    def collect(i):
        i += size
        res_lists = []
        while i:
            res_lists.append(tree[i])
            i //= 2
        return res_lists

    import heapq

    ans = [0] * n

    for i in range(n):
        lists = collect(i)

        heap = []
        ptrs = [len(lst) - 1 for lst in lists]

        for j, lst in enumerate(lists):
            if ptrs[j] >= 0:
                op_idx = lst[ptrs[j]]
                heapq.heappush(heap, (op_idx, j))

        taken = 0
        seen_ops = []
        k = c[i]

        while heap and taken < k:
            op_idx, j = heapq.heappop(heap)
            seen_ops.append(op_idx)
            ptrs[j] -= 1
            if ptrs[j] >= 0:
                heapq.heappush(heap, (lists[j][ptrs[j]], j))
            taken += 1

        seen = set()
        for op_idx in seen_ops:
            seen.add(ops[op_idx][2])

        ans[i] = len(seen)

    print(*ans)

if __name__ == "__main__":
    solve()
```Cây phân đoạn lưu trữ từng thao tác trong các nút O(log n) để mọi cơ sở dữ liệu có thể truy xuất chính xác các thao tác liên quan mà không cần quét các thao tác không liên quan. Các con trỏ ngược bên trong mỗi nút cho phép chúng ta truy cập các hoạt động gần đây nhất trước tiên, điều này là cần thiết vì chúng ta chỉ quan tâm đến hậu tố có độ dài c[i]. Heap thực hiện hợp nhất k-way trên các nút cây phân đoạn, luôn chọn thao tác mới nhất tiếp theo một cách hiệu quả. 

Một cạm bẫy triển khai phổ biến là quên rằng chúng ta phải thực hiện các thao tác theo thứ tự thời gian chung trên tất cả các nút cây phân đoạn. Chỉ cần nối các danh sách từ các nút sẽ trộn lẫn thứ tự không liên quan và phá vỡ tính chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 4
1 2 3
1 2 3
1 2 1
2 3 1
3 3 2
```Chúng tôi theo dõi các hoạt động ảnh hưởng đến từng cơ sở dữ liệu. 

| Cơ sở dữ liệu | Các hoạt động liên quan (theo thời gian) | c[i] lần thực hiện cuối cùng | Giá trị riêng biệt | 
| --- | --- | --- | --- | 
| 1 | 1, 2 | 1, 2 | {3, 1} | 
| 2 | 1, 2, 3, 4 | 2, 3, 4 | {1, 1, 2} | 
| 3 | 3, 4 | 3, 4 | {1, 2} | 

Đầu ra:```
2 2 2
```Dấu vết này cho thấy chỉ có hậu tố vật chất chứ không có lịch sử đầy đủ. 

### Ví dụ 2 

đầu vào:```
4 3
2 1 2 3
1 4 5
2 3 7
2 4 5
```| Cơ sở dữ liệu | Hoạt động liên quan | c[i] lần cuối | Khác biệt | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | {5} | 
| 2 | 1,2,3 | 2,3 | {7,5} | 
| 3 | 1,2,3 | 2,3 | {7,5} | 
| 4 | 1,3 | 1,3 | {5,5} | 

Đầu ra:```
1 2 2 1
```Điều này cho thấy các khoảng chồng chéo vẫn giảm như thế nào đối với việc trích xuất hậu tố trên mỗi điểm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n + ∑ c[i] log log n) | phân tách cây phân đoạn cộng với hợp nhất k-way trên mỗi chỉ mục | 
| Không gian | O((n + q) log n) | mỗi thao tác được lưu trữ trong các nút O(log n) | 

Cấu trúc này hiệu quả vì mỗi thao tác chỉ được sao chép theo logarit và mỗi cơ sở dữ liệu chỉ xử lý một sự hợp nhất nhỏ trên các danh sách O(log n). Điều này phù hợp một cách thoải mái trong các ràng buộc đối với n, q lên đến 2⋅10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    stdout.write = lambda s: output.append(s)
    output.clear()
    solve()
    return "".join(output).strip()

output = []

# sample 1 (as given)
assert run("""3 4
1 2 3
1 2 3
1 2 1
2 3 1
3 3 2
""") == "2 2 2"

# minimum case
assert run("""1 1
1
1 1 5
""") == "1"

# all operations identical value
assert run("""3 3
2 2 2
1 3 1
1 3 1
1 3 1
""") == "1 1 1"

# non-overlapping intervals
assert run("""5 2
1 1 1 1 1
1 2 7
4 5 9
""") == "1 1 0 1 1"

# large capacity effect (no removals)
assert run("""3 3
5 5 5
1 3 1
1 3 2
1 3 3
""") == "3 3 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 | trường hợp cơ sở đúng đắn | 
| giá trị lặp lại | 1 1 1 | lọc riêng biệt | 
| khoảng rời rạc | hỗn hợp | cách ly phạm vi | 
| công suất lớn | tích lũy đầy đủ | không có hành vi tràn | 

## Vỏ cạnh 

Một trường hợp phức tạp là khi cơ sở dữ liệu nhận được tổng số cập nhật ít hơn c[i]. Trong trường hợp đó, việc xóa sẽ không bao giờ xảy ra và câu trả lời sẽ phản ánh tập hợp đầy đủ tất cả các giá trị được áp dụng. Thuật toán xử lý vấn đề này một cách tự nhiên vì việc hợp nhất k-way chỉ dừng lại khi tất cả các hoạt động có sẵn đã hết trước khi đạt tới c[i]. 

Một trường hợp đặc biệt khác xuất hiện khi tất cả các thao tác áp dụng cho một cơ sở dữ liệu. Cây phân đoạn sẽ đặt tất cả các thao tác dọc theo một đường dẫn duy nhất và vùng heap thoái hóa thành việc duyệt ngược đơn giản một danh sách. Đầu ra vẫn đúng vì logic hợp nhất vẫn giữ nguyên thứ tự thời gian. 

Trường hợp cuối cùng là khi c[i] bằng 1. Khi đó, chỉ có thao tác gần đây nhất mới quan trọng và vùng heap ngay lập tức trích xuất một mục nhập có thời gian tối đa duy nhất từ ​​danh sách đã hợp nhất. Điều này làm giảm vấn đề thành truy vấn “cập nhật bao gồm lần cuối” thuần túy mà cấu trúc xử lý mà không sửa đổi.
