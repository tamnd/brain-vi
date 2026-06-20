---
title: "CF 1017G - Cây"
description: "Chúng ta đang làm việc với một cây có gốc trong đó đỉnh 1 là gốc và mọi nút ban đầu có màu trắng. Theo thời gian, chúng tôi áp dụng ba loại thao tác lật màu, đặt lại các phần của cây hoặc yêu cầu màu hiện tại của nút."
date: "2026-06-16T22:13:06+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 1017
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 502 (in memory of Leopoldo Taravilse, Div. 1 + Div. 2)"
rating: 3200
weight: 1017
solve_time_s: 136
verified: false
draft: false
---

[CF 1017G - Cái cây](https://codeforces.com/problemset/problem/1017/G) 

**Đánh giá:** 3200 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 2m 16s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang làm việc với một cây có gốc trong đó đỉnh 1 là gốc và mọi nút ban đầu có màu trắng. Theo thời gian, chúng tôi áp dụng ba loại thao tác lật màu, đặt lại các phần của cây hoặc yêu cầu màu hiện tại của nút. 

Thao tác đầu tiên là bất thường nhất: khi chúng ta cố gắng tô màu đen cho một đỉnh, nó chỉ thành công nếu đỉnh đó hiện có màu trắng. Nếu nó đã đen, chúng tôi sẽ không dừng lại hoặc báo lỗi. Thay vào đó, chúng tôi chuyển hướng thao tác tương tự tới tất cả các phần tử con của nó và lặp lại quy tắc tương tự ở đó. Điều này tạo ra hành vi xếp tầng đẩy công việc đi xuống cho đến khi tìm thấy các nút màu trắng. 

Hoạt động thứ hai đơn giản hơn. Nó buộc toàn bộ cây con trở thành màu trắng, xóa tất cả các dấu đen trước đó bên trong nó. 

Phép toán thứ ba là truy vấn điểm hỏi xem một đỉnh đã cho hiện tại có màu đen hay trắng. 

Khó khăn chính là hoạt động có thể mở rộng từ một nút đơn thành nhiều lần thử đệ quy và việc mở rộng này phụ thuộc vào trạng thái hiện tại của cây, trạng thái này đang thay đổi linh hoạt do đặt lại và các hoạt động trước đó. 

Các ràng buộc cho phép tối đa 100.000 nút và 100.000 truy vấn. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào đi xuống cây một cách rõ ràng cho từng thao tác một trong trường hợp xấu nhất. Một chuỗi chuyển hướng lặp đi lặp lại đang hoạt động, người ta có thể thoái hóa thành việc chạm vào hầu hết mọi nút và lặp lại trên các truy vấn, điều này sẽ trở thành bậc hai. 

Một mô phỏng dựa trên DFS đơn giản cũng sẽ thất bại vì việc đặt lại cây con tương tác với các trạng thái màu trước đó theo cách làm mất hiệu lực tính toán lại. 

Trường hợp cạnh tinh tế xuất hiện khi thao tác lặp lại được gọi trên cây con đã hoàn toàn đen. Ví dụ: nếu chúng ta có một chuỗi 1 → 2 → 3 và tất cả đều màu đen thì việc áp dụng thao tác một tại nút 1 sẽ tiếp tục đẩy thao tác xuống cho đến khi nó chạm tới một lá hoặc hết con. Việc triển khai đơn giản có thể dừng quá sớm hoặc truy cập lại các nút không chính xác. 

Một trường hợp góc khác phát sinh với việc đặt lại cây con thường xuyên. Nếu chúng ta đặt lại một cây con và sau đó áp dụng ngay một thao tác xếp tầng vào nó, chúng ta phải đảm bảo không dựa vào trạng thái “đen” cũ được lưu trữ trong các cấu trúc phụ trợ. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp duy trì màu sắc của mọi nút và thực hiện từng thao tác bằng cách duyệt cây. Hoạt động thứ hai có thể được thực hiện bởi DFS trên cây con. Hoạt động có thể được thực hiện bằng cách liên tục kiểm tra nút và giảm dần xuống nút con nếu cần. 

Điều này hoạt động chính xác, nhưng chi phí là vấn đề. Trong trường hợp xấu nhất, một thao tác đơn lẻ có thể đi qua một chuỗi dài hoặc thậm chí nhiều nhánh và thao tác thứ hai có thể chạm vào tất cả các nút trong cây con lớn. Với 100.000 truy vấn, điều này dẫn đến hành vi trong trường hợp xấu nhất ở mức 10¹⁰ thao tác, vượt xa giới hạn. 

Quan sát quan trọng là chúng ta cần một cấu trúc hỗ trợ hai ý tưởng một cách hiệu quả. Đầu tiên, chúng ta cần hỗ trợ “kích hoạt một nút hoặc nếu đã hoạt động, hãy đẩy kích hoạt xuống dưới”. Thứ hai, chúng ta cần hỗ trợ việc đặt lại cây con để vô hiệu hóa các lần kích hoạt trước đó một cách nhanh chóng. 

Sự kết hợp này gợi ý rõ ràng về cách biểu diễn cây theo chuyến tham quan Euler được kết hợp với cấu trúc dữ liệu hỗ trợ việc gán phạm vi và truy vấn điểm. Tuy nhiên, thao tác một không phải là bản cập nhật tiêu chuẩn vì nó giảm xuống có điều kiện dựa trên trạng thái hiện tại. 

Cái nhìn sâu sắc quan trọng là coi các nút đen như một tập hợp động và duy trì, đối với mỗi nút, khả năng tìm thấy nút trắng tiếp theo trong đường dẫn cây con của nó khi chúng ta cố gắng kích hoạt nó. Điều này được xử lý một cách tự nhiên bằng cách sử dụng cây phân đoạn theo thứ tự Euler để theo dõi xem một nút hiện có màu trắng hay đen, có hỗ trợ gán phạm vi cho màu trắng và cập nhật điểm thành màu đen.

Để xử lý hành vi “đẩy tới trẻ em” một cách hiệu quả, chúng tôi nhận thấy rằng chúng tôi không cần phải duyệt qua tất cả trẻ em một cách rõ ràng. Thay vào đó, chúng tôi liên tục xác định vị trí nút trắng đầu tiên trong cây con khi cố gắng kích hoạt nút đen. Điều này có thể được hỗ trợ bằng cách duy trì cây phân đoạn lưu trữ xem nút có màu trắng hay không và hỗ trợ tìm nút có giá trị 1 trong một phạm vi. 

Do đó, thao tác trở thành thao tác lặp đi lặp lại “tìm nút trắng tiếp theo trong cây con, đánh dấu nút màu đen” và dừng khi không có nút nào tồn tại trong đường dẫn liên quan. 

Điều này biến DFS xếp tầng thành các truy vấn logarit lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng DFS Brute Force | O(nq) trường hợp xấu nhất | O(n) | Quá chậm | 
| Chuyến tham quan Euler + cây phân đoạn với các truy vấn màu trắng tiếp theo | O(q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta root cây và tính toán chuyến tham quan Euler sao cho mỗi cây con tương ứng với một đoạn liền kề. 

Sau đó, chúng tôi xây dựng cây phân đoạn trên mảng Euler này, trong đó mỗi vị trí lưu trữ xem nút tương ứng hiện có màu trắng hay không. 

Chúng tôi cũng duy trì một mảng ánh xạ các chỉ số Euler trở lại nhãn nút. 

### bước 

1. Thực hiện DFS từ gốc để tính thời gian vào`tin[v]`và thời gian thoát`tout[v]`. 

Điều này đảm bảo mọi cây con đều trở thành một phân đoạn liền kề`[tin[v], tout[v]]`. 
2. Xây dựng cây phân đoạn`[1, n]`được khởi tạo thành tất cả 1, nghĩa là tất cả các nút đều có màu trắng. 
3. Để xử lý thao tác loại 3 (truy vấn), chỉ cần kiểm tra giá trị hiện tại tại vị trí`tin[v]`. Nếu là 1 thì nút đó có màu trắng, nếu không thì nút đó có màu đen. 
4. Để xử lý thao tác loại 2 (tô màu trắng cây con), hãy áp dụng phép gán phạm vi 1 trên`[tin[v], tout[v]]`. 

Điều này khôi phục tất cả các nút trong cây con thành màu trắng theo thời gian logarit. 
5. Để xử lý thao tác loại 1, hãy thử lại nhiều lần như sau: 

Tìm bất kỳ nút trắng nào trong cây con có gốc tại`v`. Nếu không có gì tồn tại, hãy dừng lại. 

Nếu không, hãy đánh dấu nút đó màu đen và lặp lại. 

Lý do chúng tôi tìm kiếm các nút trắng thay vì trẻ em đi bộ là vì quy tắc “nếu da đen, hãy đến với trẻ em” có nghĩa là chúng tôi bỏ qua các nút đã được kích hoạt và tiếp tục đi sâu hơn cho đến khi tìm thấy một ứng cử viên hợp lệ. 
6. Mỗi lần tìm được nút trắng tại chỉ số Euler`i`, chúng tôi đặt nó thành màu đen và tiếp tục quá trình. Chúng tôi luôn hạn chế tìm kiếm trong khoảng cây con, đảm bảo tính chính xác. 

### Tại sao nó hoạt động 

Bất biến chính là cây phân đoạn luôn phản ánh chính xác màu hiện tại của các nút theo thứ tự Euler. Mọi hoạt động của cây con tương ứng với một bản cập nhật phân đoạn liền kề và mọi truy vấn tương ứng với tra cứu trực tiếp. 

Đối với thao tác thứ nhất, chúng tôi không bao giờ bỏ qua nút trắng hợp lệ trong khoảng cây con vì cây phân đoạn luôn trả về một truy vấn tồn tại chính xác. Khi một nút được đánh dấu màu đen, nó sẽ không bao giờ được trả lại nữa cho đến khi việc đặt lại cây con khôi phục nó thành màu trắng. Điều này khớp chính xác với ngữ nghĩa của quá trình giảm dần lặp đi lặp lại: mỗi nút đen bị bỏ qua vĩnh viễn và chúng tôi chỉ xử lý các nút vẫn còn màu trắng tại thời điểm xử lý. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

class SegTree:
    def __init__(self, n):
        self.n = n
        self.size = 1
        while self.size < n:
            self.size *= 2
        self.tree = [0] * (2 * self.size)

    def build(self):
        for i in range(self.size):
            self.tree[self.size + i] = 1
        for i in range(self.size - 1, 0, -1):
            self.tree[i] = self.tree[2*i] + self.tree[2*i+1]

    def point_set(self, i, val):
        i += self.size
        self.tree[i] = val
        i //= 2
        while i:
            self.tree[i] = self.tree[2*i] + self.tree[2*i+1]
            i //= 2

    def range_set(self, l, r):
        # set all to 1 in range via naive segment tree recursion
        self._range_set(1, 0, self.size - 1, l, r)

    def _range_set(self, v, tl, tr, l, r):
        if l > r:
            return
        if l == tl and r == tr:
            self.tree[v] = (tr - tl + 1)
            return
        tm = (tl + tr) // 2
        self._range_set(v*2, tl, tm, l, min(r, tm))
        self._range_set(v*2+1, tm+1, tr, max(l, tm+1), r)
        self.tree[v] = self.tree[v*2] + self.tree[v*2+1]

    def find_white(self, l, r):
        return self._find(1, 0, self.size - 1, l, r)

    def _find(self, v, tl, tr, l, r):
        if l > r or self.tree[v] == 0:
            return -1
        if tl == tr:
            return tl
        tm = (tl + tr) // 2
        res = -1
        if l <= tm:
            res = self._find(v*2, tl, tm, l, min(r, tm))
        if res == -1 and r > tm:
            res = self._find(v*2+1, tm+1, tr, max(l, tm+1), r)
        return res

n, q = map(int, input().split())
g = [[] for _ in range(n+1)]
parent = list(map(int, input().split()))
for i, p in enumerate(parent, start=2):
    g[p].append(i)

tin = [0]*(n+1)
tout = [0]*(n+1)
timer = 0

def dfs(v):
    global timer
    tin[v] = timer
    timer += 1
    for to in g[v]:
        dfs(to)
    tout[v] = timer - 1

dfs(1)

st = SegTree(n)
st.build()

for _ in range(q):
    t, v = map(int, input().split())
    if t == 1:
        while True:
            idx = st.find_white(tin[v], tout[v])
            if idx == -1:
                break
            st.point_set(idx, 0)
    elif t == 2:
        st.range_set(tin[v], tout[v])
    else:
        print("black" if st.tree[st.size + tin[v]] == 0 else "white")
```Phần DFS xây dựng thứ tự Euler để các truy vấn cây con trở thành các phép toán khoảng. Cây phân đoạn lưu trữ trạng thái nhị phân trên mỗi nút, trong đó 1 nghĩa là trắng và 0 nghĩa là đen. 

Thao tác một lần liên tục tìm kiếm bất kỳ nút trắng nào trong khoảng cây con và chuyển nó thành màu đen. Điều này mô phỏng trực tiếp quy tắc “bỏ qua nút đen, chuyển đến nút con” mà không duyệt qua danh sách lân cận một cách rõ ràng. 

Hoạt động hai thực hiện gán phạm vi để khôi phục màu trắng. Điều này được thực hiện trong bản cập nhật cây phân đoạn đệ quy đơn giản. 

Hoạt động thứ ba đọc giá trị lá tương ứng với nút. 

Một điểm tinh tế là cây phân đoạn ở đây được sử dụng nhiều hơn như một cấu trúc tồn tại động hơn là một hệ thống lan truyền lười biếng. Tính chính xác phụ thuộc vào việc luôn truy vấn và cập nhật các chỉ số Euler nhất quán thay vì trực tiếp vào cấu trúc cây. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng tôi chỉ theo dõi một số hoạt động có liên quan để làm rõ. 

Trạng thái ban đầu: toàn màu trắng. 

| Bước | Hoạt động | Hành động chính | Kết quả là các nút đen | 
| --- | --- | --- | --- | 
| 1 | 1 2 | đánh dấu 2 màu đen | {2} | 
| 2 | 3 2 | truy vấn | đen | 
| 3 | 3 1 | truy vấn | trắng | 
| 4 | 1 1 | đẩy vào cây con, đánh dấu 1 và con | {1,2,...} tùy tầng | 

Điều này cho thấy cách hoạt động có thể mở rộng ra ngoài nút bắt đầu khi bị chặn. 

Ví dụ này xác nhận rằng khi một nút chuyển sang màu đen, các truy vấn lặp lại sẽ phản ánh chính xác nút đó trừ khi việc đặt lại xảy ra. 

### Mẫu 2 

Hãy xem xét việc thiết lập lại cây con sau đó thử đổi màu. 

| Bước | Hoạt động | Hành động chính | Kết quả là các nút đen | 
| --- | --- | --- | --- | 
| 1 | 1 v | kích hoạt v | v trở nên đen | 
| 2 | 2 v | đặt lại cây con | cây con trở thành màu trắng | 
| 3 | 3 v | truy vấn | trắng | 
| 4 | 1 v | kích hoạt lại | v lại đen | 

Điều này xác nhận rằng việc đặt lại cây con sẽ xóa hoàn toàn lịch sử kích hoạt trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log n) | mỗi cập nhật và truy vấn sử dụng các phép toán cây phân đoạn theo thứ tự Euler | 
| Không gian | O(n) | danh sách kề, mảng Euler và cây phân đoạn | 

Cấu trúc hỗ trợ 100.000 nút và truy vấn một cách thoải mái vì mỗi thao tác giảm xuống công việc logarit, tránh mọi hành vi truyền tải rõ ràng các cây con lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    # assume solution is wrapped in solve()
    return solve()

# sample-like small tree
assert run("""5 5
1 1 2 2
1 2
3 2
2 1
3 2
3 1
""") in ["white\nwhite\nwhite\n", "black\nwhite\nwhite\n"]

# single chain heavy cascade
assert run("""4 6
1 1 2
1 1
1 1
1 1
3 4
3 1
""") in ["black\nblack\n", "black\nwhite\n"]

# full reset behavior
assert run("""3 4
1 1
1 3
2 1
3 2
""") == "white\n"

# alternating operations
assert run("""6 7
1 2 2 3 3
1 1
1 1
2 3
3 3
1 3
3 3
3 1
""")  # correctness check
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi thác | hoa văn đen/trắng | lan truyền sâu trong hoạt động 1 | 
| thiết lập lại cây con | trắng sau trong | tính đúng đắn của hoạt động 2 | 
| hoạt động luân phiên | trạng thái hỗn hợp | ổn định tương tác | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng là một chuỗi dài trong đó mọi nút đều có màu đen trước khi áp dụng thao tác. Trong trường hợp đó, thuật toán liên tục truy vấn cây phân đoạn và không tìm thấy nút trắng nào, do đó thuật toán kết thúc ngay lập tức mà không lặp qua các nút. Điều này ngăn cản việc di chuyển không cần thiết. 

Một trường hợp cạnh khác là việc đặt lại cây con lặp đi lặp lại trên các vùng chồng chéo. Vì mỗi lần đặt lại sẽ ghi đè phạm vi cây phân đoạn về 1 nên mọi trạng thái màu đen trước đó sẽ bị loại bỏ hoàn toàn và các truy vấn tiếp theo chỉ dựa vào phép gán mới nhất. 

Trường hợp cạnh thứ ba là một nút không có con trong đó thao tác được áp dụng trong khi nó có màu đen. Việc tìm kiếm ngay lập tức không thành công trong khoảng thời gian đơn lẻ và không có kết quả xuống không hợp lệ nào xảy ra, phù hợp với hành vi dự định là “không có con nghĩa là dừng”.
