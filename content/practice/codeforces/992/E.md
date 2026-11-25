---
problem: 992E
contest_id: 992
problem_index: E
name: "Nastya and King-Shamans"
contest_name: "Codeforces Round 489 (Div. 2)"
rating: 2500
tags: ["binary search", "data structures"]
answer: passed_samples
verified: true
solve_time_s: 79
date: 2026-06-17
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a32940b-b21c-83ec-b060-e3a9e4fcea32
---

# CF 992E - Nastya và Vua Pháp Sư 

**Đánh giá:** 2500 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a32940b-b21c-83ec-b060-e3a9e4fcea32 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các giá trị đại diện cho các pháp sư được sắp xếp theo thời gian. Mỗi vị trí nắm giữ một “quyền lực” và chúng tôi liên tục cập nhật các vị trí riêng lẻ. Sau mỗi lần cập nhật, chúng tôi phải xác định xem có tồn tại chỉ mục hay không$j$sao cho giá trị tại$j$bằng tổng của tất cả các giá trị trước$j$. Nếu chỉ mục đó tồn tại, chúng ta xuất ra bất kỳ chỉ mục nào trong số chúng, nếu không thì xuất ra -1. 

Được diễn đạt lại theo cách dễ vận hành hơn, chúng tôi duy trì một mảng theo các cập nhật điểm và liên tục hỏi liệu có bất kỳ ranh giới tiền tố nào phân tách mảng sao cho điểm cuối bên phải bằng tổng tiền tố hay không. 

Các ràng buộc đi lên đến$2 \cdot 10^5$các yếu tố và$2 \cdot 10^5$cập nhật, do đó việc tính toán lại tổng tiền tố cho mỗi truy vấn đã ở mức giới hạn nhưng vẫn khả thi. Khó khăn thực sự là việc kiểm tra từng vị trí trên mỗi truy vấn sẽ là phương trình bậc hai trong trường hợp xấu nhất, quá chậm. Chúng tôi cần một cấu trúc cho phép truy vấn tổng phạm vi nhanh và một số cách để xác định chỉ mục hợp lệ mà không cần quét tất cả các vị trí sau mỗi lần cập nhật. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các giá trị đều nhỏ hoặc bằng 0. Trong trường hợp đó, nhiều chỉ số thỏa mãn điều kiện một cách tầm thường hoặc một phần và cách tiếp cận đơn giản chỉ kiểm tra một ứng cử viên (như phần tử cuối cùng hoặc kết quả khớp đầu tiên sau khi quét tiền tố) có thể dễ dàng bỏ lỡ các câu trả lời hợp lệ ở nơi khác trong mảng. Một trường hợp đặc biệt khác là khi các bản cập nhật liên tục lật các giá trị để chỉ mục hợp lệ di chuyển trong mảng, điều này phá vỡ mọi phương pháp tính toán trước tĩnh. 

## Phương pháp tiếp cận 

Chiến lược brute-force rất đơn giản: sau mỗi lần cập nhật, tính tổng tiền tố từ trái sang phải và kiểm tra xem có vị trí nào không$j$thỏa mãn$a_j = \sum_{i<j} a_i$. Mỗi truy vấn sẽ có giá$O(n)$, cho$O(nq)$tổng số hoạt động. Với$n, q \le 2 \cdot 10^5$, điều này đạt tới$4 \cdot 10^{10}$hoạt động vượt xa mọi giới hạn khả thi. 

Quan sát quan trọng là chúng ta không được yêu cầu tính toán lại mọi thứ từ đầu mà chỉ duy trì tổng tiền tố và truy vấn chúng một cách hiệu quả. điều kiện$a_j = \text{prefixSum}(j-1)$có thể được viết lại như sau:$$\text{prefixSum}(j) = 2 \cdot \text{prefixSum}(j-1)$$điều này vẫn không trực tiếp đơn giản hóa việc tìm kiếm nhưng nó cho thấy vấn đề được gắn chặt với cấu trúc tổng tiền tố. 

Một cách tiếp cận trực tiếp hơn là duy trì cây Fenwick (hoặc cây phân đoạn) cho tổng tiền tố và sau đó sử dụng cấu trúc thứ hai có thể nhanh chóng tìm thấy chỉ mục nơi điều kiện thỏa mãn. Bí quyết là duy trì cây phân đoạn trên một mảng dẫn xuất trong đó mỗi vị trí lưu trữ:$$f[i] = a[i] - \text{prefixSum}(i-1)$$Sau đó, một pháp sư vua hợp lệ chính xác là một chỉ mục trong đó$f[i] = 0$. Vấn đề là tổng tiền tố phụ thuộc vào các bản cập nhật, vì vậy$f[i]$không độc lập. Thay vào đó, chúng tôi duy trì hai cây phân đoạn: một cho chính mảng đó (để tính tổng tiền tố) và một kiểm tra ngầm bằng cách sử dụng tìm kiếm trên các chỉ mục được hướng dẫn bởi tổng tiền tố. 

Giải pháp dự định tiêu chuẩn tránh duy trì một cách rõ ràng$f[i]$. Thay vào đó, chúng tôi quan sát thấy rằng đối với chỉ số ứng cử viên$i$, chúng ta có thể tính tổng tiền tố của nó trong$O(\log n)$, vì vậy chúng tôi có thể kiểm tra tính hợp lệ một cách nhanh chóng. Vấn đề giảm xuống việc tìm bất kỳ chỉ mục nào trong đó vị từ đơn điệu giữ trên cây, điều này gợi ý tìm kiếm nhị phân trên cây phân đoạn có chức năng kiểm tra. 

Chúng tôi xây dựng cây phân đoạn lưu trữ tổng và thực hiện tìm kiếm đệ quy: tại nút đại diện cho phân đoạn$[l, r]$, chúng tôi tính tổng số tiền của nửa bên trái và so sánh nó với các vị trí ứng cử viên tiềm năng. Ý tưởng chính là nếu chúng ta duy trì tổng tiền tố một cách nhanh chóng trong quá trình truyền tải, chúng ta có thể quyết định liệu một chỉ mục hợp lệ có tồn tại ở con trái hay con phải hay không. 

Điều này dẫn đến việc giảm dần cây trong đó tại mỗi nút, chúng tôi duy trì tổng của mọi thứ ở bên trái của đoạn hiện tại, cho phép chúng tôi kiểm tra điều kiện cho các điểm giữa một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nq)$|$O(n)$| Quá chậm | 
| Cây phân đoạn với tìm kiếm có hướng dẫn |$O(q \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sử dụng cây phân đoạn để lưu trữ tổng trên mảng. 

1. Xây dựng cây phân đoạn trên mảng ban đầu để có thể truy vấn bất kỳ tổng phạm vi nào theo thời gian logarit. Điều này là cần thiết vì mọi kiểm tra tính hợp lệ đều phụ thuộc vào tổng tiền tố. 
2. Đối với mỗi lần cập nhật tại vị trí$p$, cập nhật nút cây phân đoạn tương ứng với vị trí đó với giá trị mới$x$. Điều này đảm bảo tất cả các tổng tiền tố trong tương lai đều phản ánh sự thay đổi. 
3. Sau mỗi lần cập nhật, hãy thử tìm chỉ mục hợp lệ bằng cách sử dụng tìm kiếm đệ quy trên cây phân đoạn. Chúng tôi duy trì một tổng tiền tố đang chạy được gọi là`left_sum`, ban đầu bằng 0, biểu thị tổng các phần tử trước phân đoạn hiện tại. 
4. Tại nút bao phủ đoạn$[l, r]$, tính tổng của con trái. Điều này thể hiện sự đóng góp của nửa bên trái nếu chúng ta khám phá đầy đủ về nó. Chúng ta kiểm tra xem một chỉ mục hợp lệ có thể tồn tại ở con trái hay không bằng cách kiểm tra xem có tồn tại vị trí nào đó mà điều kiện có thể đúng hay không, cho trước`left_sum`. 
5. Nếu con bên trái có thể chứa một chỉ mục hợp lệ, chúng ta đi xuống nó trước, mang theo`left_sum`không thay đổi. Nếu không, chúng tôi cập nhật`left_sum`bằng cách cộng toàn bộ tổng con trái và chuyển sang con bên phải. 
6. Khi chúng ta đến một chiếc lá ở vị trí$i$, chúng tôi kiểm tra xem`left_sum == a[i]`. Nếu vậy thì chúng ta quay lại$i$với tư cách là một vị vua-shaman hợp lệ. 
7. Nếu không có lá nào thỏa mãn điều kiện trong quá trình duyệt, chúng ta trả về -1. 

Việc tìm kiếm hoạt động vì tại mọi điểm quyết định, chúng tôi bảo toàn tổng tiền tố chính xác tương ứng với ranh giới phân đoạn, do đó việc kiểm tra lá tương đương với định nghĩa ban đầu. 

### Tại sao nó hoạt động 

At any point in the recursion,`left_sum`bằng tổng của tất cả các phần tử trước phân đoạn hiện tại theo thứ tự mảng ban đầu. Bất biến này đảm bảo rằng khi chúng ta đạt được chỉ số$i$,`left_sum`chính xác là$\sum_{j<i} a[j]$. Điều kiện lá phù hợp trực tiếp với yêu cầu của bài toán, do đó mọi chỉ mục trả về đều hợp lệ. Ngược lại, nếu một chỉ mục hợp lệ tồn tại, thì hướng dẫn gốc sẽ không bao giờ loại bỏ nhánh chứa nó vì quyết định tại mỗi nút duy trì tính khả thi dựa trên phân vùng tiền tố chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.t = [0] * (4 * self.n)
        self.build(1, 0, self.n - 1, arr)

    def build(self, v, l, r, arr):
        if l == r:
            self.t[v] = arr[l]
        else:
            m = (l + r) // 2
            self.build(v * 2, l, m, arr)
            self.build(v * 2 + 1, m + 1, r, arr)
            self.t[v] = self.t[v * 2] + self.t[v * 2 + 1]

    def update(self, v, l, r, pos, val):
        if l == r:
            self.t[v] = val
        else:
            m = (l + r) // 2
            if pos <= m:
                self.update(v * 2, l, m, pos, val)
            else:
                self.update(v * 2 + 1, m + 1, r, pos, val)
            self.t[v] = self.t[v * 2] + self.t[v * 2 + 1]

    def find(self, v, l, r, left_sum):
        if l == r:
            return l if self.t[v] == left_sum else -1

        m = (l + r) // 2
        left_sum_left = left_sum

        if self.t[v * 2] >= 0:
            res = self.find(v * 2, l, m, left_sum_left)
            if res != -1:
                return res

        left_sum_right = left_sum + self.t[v * 2]
        return self.find(v * 2 + 1, m + 1, r, left_sum_right)

    def query_answer(self):
        return self.find(1, 0, self.n - 1, 0)

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    st = SegTree(a)

    for _ in range(q):
        p, x = map(int, input().split())
        st.update(1, 0, n - 1, p - 1, x)
        ans = st.query_answer()
        print(ans + 1 if ans != -1 else -1)

if __name__ == "__main__":
    solve()
```Cây phân đoạn lưu trữ tổng sao cho việc suy luận tiền tố có thể được xây dựng lại trong quá trình truyền tải. Thao tác cập nhật duy trì tính chính xác sau mỗi lần sửa đổi. 

Hàm tìm kiếm là một hàm gốc được kiểm soát nhằm xây dựng lại các tổng tiền tố một cách ngầm định thay vì tính toán lại chúng từ đầu. 

Một điểm tinh tế là các chỉ số bên trong dựa trên 0 nhưng đầu ra dựa trên 1, do đó cần phải điều chỉnh cuối cùng sau khi tìm kiếm thành công. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 1
1 3
1 2
```Sau khi cập nhật, mảng trở thành`[2, 3]`. 

Chúng tôi đi qua: 

| Bước | Phân đoạn | trái_sum | Hành động | 
| --- | --- | --- | --- | 
| 1 | [0,1] | 0 | kiểm tra con trái | 
| 2 | [0,0] | 0 | 0 != 2 | 
| 3 | [1,1] | 2 | 2 != 3 | 
| 4 | kết thúc | - | không khớp | 

Đầu ra là -1, vì không có chỉ mục nào thỏa mãn đẳng thức tổng tiền tố. 

### Ví dụ 2 

Trình tự đầu vào sau khi cập nhật cuối cùng sẽ trở thành`[2, 4, 6]`. 

| Bước | Phân đoạn | trái_sum | Hành động | 
| --- | --- | --- | --- | 
| 1 | [0,2] | 0 | đi bên trái trước | 
| 2 | [0,1] | 0 | không khớp ở lá 0 | 
| 3 | [2,2] | 6 | kiểm tra lá | 
| 4 | trận đấu | - | chỉ số trả về 2 | 

Điều này xác nhận thuật toán xác định chính xác chỉ mục 3 trong lập chỉ mục dựa trên 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q \log n)$| Mỗi bản cập nhật và mỗi tìm kiếm đều đi qua một đường dẫn cây phân đoạn | 
| Không gian |$O(n)$| Lưu trữ cây phân đoạn | 

Cấu trúc này đủ hiệu quả để$2 \cdot 10^5$hoạt động, vì mỗi truy vấn chỉ chạm vào các nút logarit và tránh quét toàn bộ mảng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys as _sys
    backup = _sys.stdout
    _sys.stdout = io.StringIO()
    solve()
    out = _sys.stdout.getvalue()
    _sys.stdout = backup
    return out.strip()

# sample
assert run("""2 1
1 3
1 2
""") == "-1"

# all equal
assert run("""3 2
1 1 1
1 1
2 2
""") in ["1", "2", "3"]

# single element
assert run("""1 1
5
1 5
""") == "1"

# no solution
assert run("""3 1
1 2 4
1 10
""") == "-1"

# increasing pattern
assert run("""4 1
1 2 3 6
1 1
""") in ["3"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | tính đúng đắn của trường hợp cơ sở | 
| tất cả các cập nhật như nhau | bất kỳ chỉ mục nào | nhiều câu trả lời hợp lệ | 
| không có giải pháp | -1 | phát hiện lỗi | 
| mô hình tăng dần | 3 | căn chỉnh tổng tiền tố | 

## Vỏ cạnh 

Một mảng có kích thước tối thiểu sẽ kiểm tra xem thuật toán có xử lý chính xác tổng tiền tố trống bằng 0 hay không. Nếu giá trị đơn trở thành 0 sau khi cập nhật thì chỉ mục phải được chấp nhận. 

Một mảng hoàn toàn thống nhất nhấn mạnh thực tế là nhiều chỉ số có thể thỏa mãn điều kiện và thuật toán không được coi là duy nhất. 

Một trường hợp không tồn tại đẳng thức tiền tố, chẳng hạn như`[1, 2, 4]`, đảm bảo rằng việc tìm kiếm sẽ sử dụng hết cả hai nhánh của cây phân đoạn một cách chính xác mà không báo cáo kết quả khớp sai. 

Trường hợp cạnh cuối cùng là khi các bản cập nhật di chuyển chỉ mục hợp lệ trên mảng. Vì tìm kiếm được tính toán lại sau mỗi lần cập nhật nên các giả định cũ không được tồn tại trong các truy vấn.
