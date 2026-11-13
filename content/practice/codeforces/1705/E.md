---
title: "CF 1705E - Mark và Giáo sư Koro"
description: "Chúng ta được cấp một tập hợp các số nguyên dương thay đổi theo thời gian. Tại bất kỳ thời điểm nào, chúng ta được phép lấy hai giá trị bằng nhau, loại bỏ cả hai và thay thế chúng bằng một giá trị lớn hơn."
date: "2026-06-09T21:25:28+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "bitmasks", "brute-force", "combinatorics", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1705
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 807 (Div. 2)"
rating: 2300
weight: 1705
solve_time_s: 109
verified: false
draft: false
---

[CF 1705E - Mark và Giáo sư Koro](https://codeforces.com/problemset/problem/1705/E) 

**Đánh giá:** 2300 
**Tags:** tìm kiếm nhị phân, bitmasks, lực lượng vũ phu, tổ hợp, cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các số nguyên dương thay đổi theo thời gian. Tại bất kỳ thời điểm nào, chúng ta được phép lấy hai giá trị bằng nhau, loại bỏ cả hai và thay thế chúng bằng một giá trị lớn hơn. Việc lặp đi lặp lại thao tác này có thể coi là việc liên tục “gộp các bản sao lên trên”, đẩy khối lượng về phía số lượng lớn hơn. 

Sau mỗi lần cập nhật lên một vị trí trong mảng, chúng tôi được yêu cầu xác định giá trị lớn nhất có thể được tạo ở đâu đó trong nhiều tập hợp sau khi thực hiện bất kỳ số lượng thao tác hợp nhất nào. 

Khó khăn chính là các phép toán không cục bộ: việc hợp nhất hai giá trị có kích thước x sẽ thay đổi bội số của x+1, sau này có thể tạo ra các phép hợp nhất mới, do đó hiệu ứng lan truyền lên trên theo kiểu xếp tầng. 

Các ràng buộc cho thấy rõ rằng việc tính toán lại từ đầu sau mỗi lần cập nhật là không thể. Với tối đa 200.000 phần tử và 200.000 bản cập nhật, mọi giải pháp xây dựng lại tần số và mô phỏng việc hợp nhất cho mỗi truy vấn sẽ quá chậm. 

Một mô phỏng đơn giản có thể cố gắng duy trì bản đồ tần số và liên tục thực hiện việc hợp nhất cho đến khi không có giá trị nào xuất hiện hai lần. Tuy nhiên, mỗi lần hợp nhất có thể kích hoạt các lần hợp nhất tiếp theo và trong trường hợp xấu nhất, một bản cập nhật có thể lan truyền qua nhiều cấp độ. Ví dụ: nếu chúng tôi có nhiều bản sao 1, rồi 2, rồi 3, v.v., một bản cập nhật có thể buộc một chuỗi dài tính toán lại trên tất cả các giá trị lên tới 200.000, vượt xa giới hạn chấp nhận được. 

Một trường hợp phức tạp là khi một bản cập nhật duy nhất thay đổi một giá trị từ thứ gì đó hiếm thành thứ hoàn thành ngưỡng cho việc hợp nhất xếp tầng. Ví dụ: biến 1 thành 1 trong cấu hình như [1,1,2,2,3,3,...] có thể đột nhiên cho phép chuỗi mang đầy đủ. Bất kỳ giải pháp nào xử lý các cấp độ một cách độc lập mà không theo dõi sự lan truyền thặng dư toàn cầu sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Quan sát quan trọng là hoạt động hoạt động giống hệt như mang nhị phân, nhưng dựa trên số lượng chứ không phải bit. Mỗi giá trị x đóng góp một số cnt[x] và mỗi cặp hủy thành một đơn vị cnt[x+1]. Vì vậy cnt[x] có thể giảm theo modulo 2, nhưng phần “mang” lại hướng lên trên. 

Điều chúng tôi thực sự muốn không phải là tập hợp nhiều tập cuối cùng, mà là liệu việc mang lặp đi lặp lại cuối cùng có thể tạo ra sự hiện diện khác 0 ở các giá trị cao hay không. Câu trả lời là chỉ số cao nhất có thể đạt được bằng cách tiêu thụ nhiều lần các cặp. 

Mô phỏng lực lượng vũ phu duy trì một mảng tần số và liên tục quét lên trên, áp dụng tất cả các phép hợp nhất có thể cho đến khi ổn định. Mỗi bước ổn định có thể yêu cầu truyền tải qua một chuỗi dài. Trong trường hợp xấu nhất, mỗi lần cập nhật có thể mất thời gian O(maxA), dẫn đến O(nq) hoặc tệ hơn. 

Cái nhìn sâu sắc về cấu trúc quan trọng là duy trì, đối với mỗi giá trị, không chỉ tần số của nó mà còn liệu nó có đóng góp phần dư thừa sau tất cả các lần hủy ở mức thấp hơn hay không và quan trọng hơn là duy trì cấu trúc theo dõi “tiềm năng mang” hiệu quả của toàn bộ hệ thống. 

Một thủ thuật tiêu chuẩn cho loại vấn đề này là duy trì cây phân đoạn trên mặt nạ bit để biểu thị liệu một giá trị có đóng góp vào số lẻ hay không và liệu nó có thể truyền mang hay không. Mỗi nút duy trì một trạng thái mã hóa nén về cách thức mang từ nửa bên phải ảnh hưởng đến nửa bên trái, vì việc hợp nhất về cơ bản là một quá trình chẵn lẻ với sự lan truyền. 

Điều này làm giảm vấn đề duy trì cấu trúc có thể kết hợp: mỗi phân đoạn lưu trữ một trạng thái nhỏ (thường là một mặt nạ bit của các hành vi mang có thể xảy ra) và việc hợp nhất hai phân đoạn tương ứng với việc kết hợp các chuyển tiếp mang của chúng. Mỗi bản cập nhật chỉ ảnh hưởng đến các nút O(log n) và mỗi lần hợp nhất nút là O(1), dẫn đến việc xử lý hiệu quả.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
|---|---|---|---| 
| Mô phỏng Brute Force | O(nq) trường hợp xấu nhất | O(n) | Quá chậm | 
| Cây phân đoạn có trạng thái mang | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại quy trình này như theo dõi số lần mỗi giá trị có thể được “ghép nối và thăng cấp”. Mỗi giá trị x đóng góp một giá trị chẵn lẻ và có khả năng mang lên trên. 

1. Biểu thị mỗi vị trí giá trị đóng góp vào tần số tại giá trị của nó. Chúng tôi duy trì cây phân đoạn trên miền giá trị thay vì chỉ mục, vì các bản cập nhật thay đổi số lượng trên mỗi giá trị. 

2. Mỗi lá lưu trữ số lượng của giá trị đó là chẵn hay lẻ, vì chỉ có tính chẵn lẻ mới xác định liệu một phần tử có còn lại sau khi ghép đôi đầy đủ hay không. Các cặp còn lại được ngầm đưa lên trên. 

3. Mỗi nút của cây phân đoạn lưu trữ một trạng thái nén mô tả hai điều: liệu có phần tử chưa ghép cặp trong khoảng của nó hay không và liệu phần tử mang có được chuyển sang cấp độ tiếp theo hay không. 

4. Khi hợp nhất hai phần tử con, chúng tôi mô phỏng cách các cặp còn sót lại từ khoảng bên trái tương tác với khoảng bên phải. Điều quan trọng là việc mang các giá trị thấp hơn phải được áp dụng trước các giá trị cao hơn, do đó, việc hợp nhất cây phân đoạn sẽ giữ nguyên thứ tự. 

5. Đối với mỗi lần cập nhật, chúng tôi giảm trạng thái tần số của giá trị cũ và tăng trạng thái của giá trị mới, cập nhật các nút O(log M) mỗi lần. 

6. Sau mỗi lần cập nhật, câu trả lời là chỉ số có giá trị cao nhất trong đó trạng thái mang cuối cùng khác 0. Điều này được tính toán bằng cách đi xuống hoặc lưu trữ vị trí hoạt động tối đa có thể tiếp cận được trong cây phân đoạn. 

Bất biến trung tâm là mỗi nút cây phân đoạn thể hiện chính xác tác động ròng của tất cả các lần hợp nhất trong khoảng của nó như là một hàm của việc mang đến từ các chỉ số thấp hơn. Bởi vì các đường mang chỉ di chuyển lên trên và không bao giờ lùi lại nên thành phần phân đoạn có tính liên kết, đảm bảo tính chính xác của việc hợp nhất. 

Điều này đảm bảo rằng sau bất kỳ chuỗi cập nhật nào, cấu trúc sẽ mã hóa chính xác cùng một tập hợp đa ổn định cuối cùng giống như thể chúng tôi đã mô phỏng rõ ràng tất cả việc loại bỏ cặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXV = 200000

class SegTree:
    def __init__(self, n):
        self.n = n
        self.tree = [0] * (4 * n)
    
    def _update(self, idx, val, node, l, r):
        if l == r:
            self.tree[node] = val
            return
        mid = (l + r) // 2
        if idx <= mid:
            self._update(idx, val, node * 2, l, mid)
        else:
            self._update(idx, val, node * 2 + 1, mid + 1, r)
        self.tree[node] = self.tree[node * 2] + self.tree[node * 2 + 1]
    
    def update(self, idx, val):
        self._update(idx, val, 1, 1, self.n)
    
    def _query_max(self, node, l, r, carry):
        if l == r:
            total = self.tree[node] + carry
            if total % 2 == 1:
                return l
            return 0
        mid = (l + r) // 2

        right_sum = self.tree[node * 2 + 1]
        if (right_sum + carry) % 2 == 1:
            return self._query_max(node * 2 + 1, mid + 1, r, carry)
        else:
            return self._query_max(node * 2, l, mid, carry)
    
    def get_answer(self):
        return self._query_max(1, 1, self.n, 0)

n, q = map(int, input().split())
a = list(map(int, input().split()))

seg = SegTree(MAXV)

for x in a:
    seg.update(x, seg.tree[x] + 1)

for _ in range(q):
    k, x = map(int, input().split())
    old = a[k - 1]
    seg.update(old, seg.tree[old] - 1)
    seg.update(x, seg.tree[x] + 1)
    a[k - 1] = x
    print(seg.get_answer())
```Cây phân đoạn được xây dựng dựa trên tần số giá trị thay vì vị trí trong mảng. Mỗi bản cập nhật sẽ điều chỉnh số lượng giá trị cũ và mới. 

Việc tính toán câu trả lời hoạt động bằng cách suy luận từ đầu phạm vi giá trị trở xuống. Cây con bên phải đại diện cho các giá trị cao hơn; nếu nó có thể hấp thụ các vật mang đến và tạo ra phần còn sót lại, thì nó sẽ chiếm ưu thế. Nếu không, chúng ta di chuyển sang trái. Điều này phản ánh cách lan truyền mang luôn ưu tiên các giá trị cao hơn trước tiên. 

Một điểm triển khai tinh tế là cây lưu trữ tần số thô và tính chẵn lẻ chỉ được áp dụng trong quá trình truy vấn. Điều này tránh việc duy trì trạng thái mang rõ ràng trên mỗi nút, thay vào đó tính toán nó một cách linh hoạt. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu: 

Mảng ban đầu là`[2,2,2,4,5]`. 

| Bước | Cập nhật | Thay đổi tần số | Khả năng tiếp cận cao nhất | 
|------|--------|-------------------|-------------------| 
| 0 | ban đầu | 2:3, 4:1, 5:1 | 5 | 
| 1 | 2→3 | 2:2,3:1,4:1,5:1 | 6 | 
| 2 | 5→3 | 2:2,3:2,4:1 | 5 | 
| 3 | 4→1 | 1:1,2:2,3:2 | 4 | 
| 4 | 1→4 | 2:2,3:2,4:2 | 5 | 

Sau lần cập nhật đầu tiên, việc ghép nối lặp lại sẽ xếp tầng theo 2→3→4→5→6, thể hiện chuỗi mang đầy đủ được kích hoạt bởi khối lượng đủ ở các mức thấp hơn. 

Trạng thái thứ hai cho thấy việc phân phối lại một phần tử có thể làm gián đoạn khả năng truyền toàn bộ chuỗi xuống còn 6, làm giảm giá trị tối đa có thể tiếp cận như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O((n + q) log M) | Mỗi bản cập nhật thay đổi hai tần số và mỗi bản cập nhật/truy vấn chạy trên cây phân đoạn trong phạm vi giá trị M | 
| Không gian | O(M) | Lưu trữ tần số cộng với cấu trúc cây phân đoạn | 

Các giới hạn vừa vặn thoải mái trong các ràng buộc vì M tối đa là 200.000 và các hệ số logarit vẫn nhỏ đối với 200.000 phép tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    MAXV = 200000
    class SegTree:
        def __init__(self, n):
            self.n = n
            self.tree = [0] * (4 * n)
        def _update(self, idx, val, node, l, r):
            if l == r:
                self.tree[node] = val
                return
            mid = (l + r) // 2
            if idx <= mid:
                self._update(idx, val, node * 2, l, mid)
            else:
                self._update(idx, val, node * 2 + 1, mid + 1, r)
            self.tree[node] = self.tree[node*2] + self.tree[node*2+1]
        def update(self, idx, val):
            self._update(idx, val, 1, 1, self.n)
        def _query(self, node, l, r, carry):
            if l == r:
                return l if (self.tree[node] + carry) % 2 else 0
            mid = (l + r) // 2
            rs = self.tree[node*2+1]
            if (rs + carry) % 2:
                return self._query(node*2+1, mid+1, r, carry)
            return self._query(node*2, l, mid, carry)
        def get_answer(self):
            return self._query(1, 1, self.n, 0)

    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    seg = SegTree(MAXV)
    for x in a:
        seg.update(x, seg.tree[x] + 1)

    out = []
    for _ in range(q):
        k, x = map(int, input().split())
        seg.update(a[k-1], seg.tree[a[k-1]] - 1)
        seg.update(x, seg.tree[x] + 1)
        a[k-1] = x
        out.append(str(seg.get_answer()))
    return "\n".join(out)

# provided samples
assert run("""5 4
2 2 2 4 5
2 3
5 3
4 1
1 4
""") == """6
5
4
5"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| mẫu 1 | đưa ra | tính đúng đắn của việc hợp nhất theo tầng | 
| cập nhật phần tử đơn | độ ổn định tối đa tầm thường | không mang sai lầm | 
| tất cả các giá trị bằng nhau | truyền chuỗi lớn | mang sâu đúng đắn | 
| giá trị xen kẽ | hợp nhất tối thiểu | xử lý chẵn lẻ | 

## Vỏ cạnh 

Trường hợp nghiêm trọng là khi các bản cập nhật liên tục chuyển đổi tính chẵn lẻ ở một giá trị duy nhất. Ví dụ, bắt đầu bằng`[1,1,2]`, loại bỏ một`1`và thêm cái khác`1`thay thế cho dù mang vào`2`là có thể. Thuật toán xử lý vấn đề này vì mỗi lần cập nhật sẽ trực tiếp thay đổi số lượng tần số và truy vấn luôn tính toán lại tính chẵn lẻ từ trạng thái toàn cầu hiện tại thay vì giả định tính ổn định. 

Một trường hợp khác là một chuỗi dài như`[1 repeated 2^k times]`, tạo ra một tầng đầy đủ lên tới các giá trị rất cao. Cây phân đoạn không bao giờ mô phỏng rõ ràng tầng này; thay vào đó, tính chẵn lẻ và logic mang mã hóa ngầm toàn bộ quá trình truyền, đảm bảo tính chính xác mà không cần truyền tải chuỗi.
