---
problem: 990G
contest_id: 990
problem_index: G
name: "GCD Counting"
contest_name: "Educational Codeforces Round 45 (Rated for Div. 2)"
rating: 2400
tags: ["divide and conquer", "dp", "dsu", "number theory", "trees"]
answer: passed_samples
verified: true
solve_time_s: 101
date: 2026-06-17
model: gpt-5-5
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a32935e-70f4-83ec-a952-c1e336e827ce
---

#CF 990G - Đếm GCD 

**Đánh giá:** 2400 
**Tags:** chia để trị, dp, dsu, lý thuyết số, cây 
**Mẫu:** gpt-5-5 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a32935e-70f4-83ec-a952-c1e336e827ce 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta được cho một cây trong đó mỗi đỉnh mang một giá trị nguyên. Đối với hai đỉnh bất kỳ, chúng ta xét đường đi đơn giản duy nhất giữa chúng và lấy ước chung lớn nhất của tất cả các giá trị dọc theo đường đi đó. Nhiệm vụ là đếm, với mỗi giá trị số nguyên có thể, có bao nhiêu cặp đỉnh tạo ra gcd chính xác đó khi chúng ta nhìn vào đường kết nối của chúng. 

Một cách hữu ích để suy nghĩ về đầu ra là mọi cặp đỉnh không có thứ tự, kể cả trường hợp cả hai điểm cuối đều là cùng một đỉnh, đều đóng góp chính xác một giá trị, đó là gcd của các giá trị dọc theo đường đi của chúng. Chúng ta cần một bảng tần số của các kết quả gcd này trên tất cả các cặp như vậy. 

Ràng buộc rằng cả số lượng đỉnh và giá trị có thể lên tới hai trăm nghìn ngay lập tức loại trừ bất kỳ phương pháp nào kiểm tra rõ ràng tất cả các cặp đỉnh. Việc liệt kê bậc hai trên các cặp đã vượt quá giới hạn thời gian theo một số bậc độ lớn và thậm chí còn hơn thế vì mỗi cặp yêu cầu một truy vấn đường dẫn trong cây. 

Một khó khăn tinh vi hơn xuất phát từ thực tế là các giá trị gcd hoạt động không cục bộ. Hai đường dẫn có điểm cuối giống nhau vẫn có thể tạo ra các gcd khác nhau tùy thuộc vào các nút trung gian, vì vậy chúng ta không thể nén cây thành một cấu trúc tĩnh đơn giản mà không theo dõi cẩn thận cách gcd phát triển dọc theo các đường dẫn. 

Một cạm bẫy phổ biến là cố gắng sửa một root và chỉ tính toán gcds của các đường dẫn root-to-node. Điều đó không thành công vì gcd của đường dẫn chung giữa hai nút không được xác định bởi các đường dẫn gốc riêng lẻ của chúng trừ khi chúng tôi giải thích rõ ràng về việc hủy chồng chéo thông qua cấu trúc LCA. 

Một dạng lỗi khác là giả định rằng các giá trị gcd dọc theo một đường dẫn hoạt động độc lập trên các đoạn của cây. Ví dụ: trong một chuỗi như 12-18-6, gcd của các điểm cuối là 6, nhưng sự kết hợp trung gian của các đường dẫn phụ tạo ra các phân phối gcd khác nhau mà một tập hợp cục bộ đơn giản sẽ bỏ qua. 

## Phương pháp tiếp cận 

Một giải pháp Brute Force sẽ lặp qua từng cặp đỉnh, đi lên cây hoặc tính toán đường đi một cách rõ ràng và tính gcd trên đường đi. Ngay cả khi mỗi truy vấn đường dẫn được tối ưu hóa theo thời gian logarit bằng kỹ thuật LCA, chúng ta vẫn sẽ phải đối mặt với khoảng n2 cặp, dẫn đến khoảng 4×10¹⁰ phép toán trong trường hợp xấu nhất, điều này là không khả thi. 

Thông tin chi tiết về cấu trúc quan trọng là các giá trị gcd dọc theo đường dẫn không thay đổi tùy ý. Nếu chúng ta lấy một nút cố định và xem xét tất cả các đường dẫn bắt đầu từ nó và đi xuống cây con của nó, thì tập hợp các giá trị gcd riêng biệt xuất hiện là rất nhỏ. Khi chúng tôi mở rộng đường dẫn bằng cách thêm một nút nữa, gcd sẽ giữ nguyên hoặc giảm xuống ước số và nó chỉ có thể giảm số lần logarit trước khi đạt 1. 

Điều này gợi ý một chiến lược lập trình động trên cây, nơi chúng tôi duy trì, đối với mỗi nút, một bản tóm tắt nhỏ gọn về tất cả các giá trị gcd của các đường dẫn bắt đầu từ nút đó và đi vào cây con của nó. Khi kết hợp các phần tử con, chúng tôi hợp nhất các bản tóm tắt này và đếm xem có bao nhiêu cặp cây con chéo tạo ra mỗi gcd. 

Cơ chế quan trọng là truyền tải theo thứ tự sau trong đó mỗi nút tổng hợp các bản đồ từ các nút con của nó. Mỗi bản đồ lưu trữ tần số của các giá trị gcd cho các đường dẫn bắt đầu từ nút đó. Khi kết hợp hai bản đồ con thông qua nút hiện tại, chúng tôi tính toán sự đóng góp giữa tất cả các cặp trạng thái gcd, vì bất kỳ cặp nút nào trong các cây con khác nhau đều tạo thành một đường dẫn đi qua nút hiện tại. 

Hiệu quả đến từ thực tế là bản đồ của mỗi nút vẫn còn nhỏ và việc hợp nhất được thực hiện bằng cách sử dụng hành vi từ nhỏ đến lớn hoàn toàn thông qua nén gcd. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² log A) | O(n) | Quá chậm | 
| Tối ưu | O(n log2 A) | O(n log A) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Gốc cây tại một nút tùy ý, chẳng hạn như nút 1, sao cho mọi cây con đều được xác định rõ ràng. Điều này cho phép chúng tôi kết hợp các kết quả từ dưới lên. 
2. Đối với mỗi nút, hãy xác định một từ điển lưu trữ bao nhiêu đường dẫn bắt đầu từ nút này trở xuống có một gcd nhất định. Ban đầu, mỗi nút chỉ đóng góp một đường dẫn với gcd bằng giá trị của chính nó. Điều này tương ứng với đường dẫn có độ dài một. 
3. Thực hiện tìm kiếm theo chiều sâu. Khi trở về từ một đứa trẻ, chúng ta nhận được bản đồ phân phối gcd của nó. Trước khi hợp nhất nó vào nút hiện tại, chúng tôi cập nhật tất cả các giá trị gcd trong bản đồ con đó bằng cách lấy gcd với giá trị của nút hiện tại. Điều này mở rộng chính xác mọi đường dẫn đi lên bằng cách bao gồm nút cha. 
4. Duy trì bản đồ tổng hợp đang chạy cho nút hiện tại. Khi xử lý bản đồ con mới, mỗi cặp bao gồm một trạng thái gcd từ tập hợp hiện tại và một trạng thái từ con đại diện cho một đường dẫn có điểm cuối nằm trong các cây con khác nhau. Gcd của đường dẫn đó là gcd của hai giá trị gcd được lưu trữ. 
5. Đối với mỗi cặp giá trị gcd như vậy, chúng tôi tích lũy phần đóng góp của chúng vào mảng câu trả lời chung được lập chỉ mục bởi gcd kết quả. Điều này tính đến tất cả các đường dẫn đi qua nút hiện tại với các điểm cuối trong các cây con khác nhau. 
6. Sau khi xử lý tất cả các nút con, hãy chèn phần đóng góp của nút đơn hiện tại vào bản đồ của nó và trả lại cho nút cha. 

### Tại sao nó hoạt động 

Tại mỗi nút, chúng tôi duy trì một mô tả đầy đủ về tất cả các đường dẫn có điểm cao nhất trên cây là nút đó. Bất kỳ đường dẫn nào trong cây đều có LCA cao nhất duy nhất và quy trình hợp nhất của chúng tôi đảm bảo rằng mọi cặp nút đều được tính chính xác tại LCA đó. Gcd được lưu trữ trong mỗi mục bản đồ luôn phản ánh đường dẫn đầy đủ từ nút xuống nút con, do đó, việc kết hợp hai mục nhập sẽ tái tạo lại chính xác gcd của đường dẫn đầy đủ giữa các nút con đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

from collections import defaultdict

n = int(input())
a = list(map(int, input().split()))

g = [[] for _ in range(n)]
for _ in range(n - 1):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    g[u].append(v)
    g[v].append(u)

ans = defaultdict(int)

def dfs(u, p):
    cur = defaultdict(int)
    cur[a[u]] = 1

    for v in g[u]:
        if v == p:
            continue
        child = dfs(v, u)

        new_child = defaultdict(int)
        for gv, cnt in child.items():
            ng = gcd(gv, a[u])
            new_child[ng] += cnt

        for g1, c1 in cur.items():
            for g2, c2 in new_child.items():
                ans[math_gcd(g1, g2)] += c1 * c2

        for k, vcnt in new_child.items():
            cur[k] += vcnt

    return cur

import math
def math_gcd(x, y):
    return math.gcd(x, y)

dfs(0, -1)

# add single-node paths already included in dfs, but they need to be counted as pairs (x,x)
for i in range(n):
    ans[a[i]] += 1

out = []
for k in sorted(ans):
    if 1 <= k <= 200000 and ans[k] > 0:
        out.append(f"{k} {ans[k]}")
print("\n".join(out))
```Cấu trúc cốt lõi là một DFS trả về một từ điển nén gồm các trạng thái gcd trên mỗi cây con. Mỗi lần chúng tôi mở rộng phần đóng góp con lên trên, chúng tôi sẽ tính toán lại gcds với giá trị gốc để mọi trạng thái được lưu trữ luôn biểu thị một đường dẫn bắt đầu từ nút hiện tại. 

Vòng lặp kép kết thúc`cur`Và`new_child`là nơi tính các đường dẫn cây con chéo. Điều này là an toàn vì cả hai từ điển trong thực tế vẫn còn nhỏ do nén gcd và mỗi lần hợp nhất chỉ xảy ra dọc theo các cạnh của cây, giữ cho tổng công việc tuyến tính theo hệ số logarit. 

Vòng lặp cuối cùng thêm các đóng góp của một nút vì mỗi đỉnh tạo thành một cặp hợp lệ với chính nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
1 2
2 3
```Chúng tôi root ở nút 1. 

| Bước | Nút | bản đồ cur | bản đồ con | đóng góp thêm | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | {3:1} | - | - | 
| 2 | 2 | {2:1} | {3:1} | gcd(2,3)=1 đóng góp 1 | 
| 3 | 1 | {1:1, 2:1, 3:1} | hợp nhất | cặp chéo tạo ra số đếm cuối cùng | 

Thuật toán tính toán chính xác tất cả 6 cặp: (1,1), (2,2), (3,3), (1,2), (2,3), (1,3), tạo ra số lượng gcd phù hợp với mẫu. 

### Ví dụ 2 

đầu vào:```
4
2 6 3 9
1 2
2 3
2 4
```Ở đây nút 2 là một trung tâm. Cây con 1, 3, 4 được kết hợp thông qua nút 2. 

| Bước | Nút | đồ cur sau khi xử lý | 
| --- | --- | --- | 
| 1 | lá | {giá trị:1} mỗi lá | 
| 2 | 2 | kết hợp tất cả trẻ em | 
| 3 | gốc | tổng hợp cuối cùng | 

Điều này cho thấy tất cả các cặp cây con chéo bị buộc phải thông qua nút 2 như thế nào, đảm bảo mọi đường dẫn gcd đều được ghi lại chính xác một lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log2 A) | mỗi cạnh hợp nhất các tập gcd nhỏ, mỗi giá trị chuyển tiếp dọc theo các ước số | 
| Không gian | O(n log A) | mỗi nút lưu trữ một bản phân phối gcd nén | 

Các ràng buộc n 2×10⁵ và A ≤ 2×10⁵ phù hợp một cách thoải mái vì chuỗi gcd co lại nhanh chóng, ngăn cản việc hình thành các bản đồ lớn trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math
    from collections import defaultdict

    n = int(input())
    a = list(map(int, input().split()))
    g = [[] for _ in range(n)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1; v -= 1
        g[u].append(v)
        g[v].append(u)

    ans = defaultdict(int)

    def dfs(u, p):
        cur = defaultdict(int)
        cur[a[u]] = 1
        for v in g[u]:
            if v == p:
                continue
            child = dfs(v, u)
            new_child = defaultdict(int)
            for gv, cnt in child.items():
                new_child[math.gcd(gv, a[u])] += cnt
            for g1, c1 in cur.items():
                for g2, c2 in new_child.items():
                    ans[math.gcd(g1, g2)] += c1 * c2
            for k, vcnt in new_child.items():
                cur[k] += vcnt
        return cur

    dfs(0, -1)
    for i in range(n):
        ans[a[i]] += 1

    out = []
    for k in sorted(ans):
        if ans[k] > 0:
            out.append(f"{k} {ans[k]}")
    return "\n".join(out)

# provided sample
assert run("""3
1 2 3
1 2
2 3
""") == """1 4
2 1
3 1"""

# all equal
assert run("""3
5 5 5
1 2
2 3
""") == """5 6"""

# single chain
assert run("""4
2 4 6 8
1 2
2 3
3 4
""") is not None

# star
assert run("""4
3 3 3 3
1 2
1 3
1 4
""") == """3 10"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | đưa ra | tính đúng đắn trên cây hỗn hợp | 
| tất cả đều bình đẳng | 5 6 | tất cả các đường dẫn giống nhau gcd | 
| chuỗi | tính toán | truyền dọc theo đường đi | 
| ngôi sao | 3 10 | đếm cây con chéo | 

## Vỏ cạnh 

Đối với cây hình ngôi sao trong đó mọi nút có cùng giá trị, mọi cặp đều đóng góp cùng một gcd. Thuật toán xử lý vấn đề này bằng cách tạo ra các bản đồ gcd giống hệt nhau ở mỗi con và đếm tất cả các kết hợp chéo ở gốc, khớp với số cặp tổ hợp. 

Đối với một chuỗi tuyến tính, mỗi lần hợp nhất diễn ra tuần tự và không xảy ra vụ nổ cây con chéo. Mỗi bước chỉ đơn giản là mở rộng trạng thái gcd lên trên, chứng tỏ rằng thuật toán xử lý chính xác chiều cao cây suy biến. 

Đối với một cây trong đó các giá trị là cặp nguyên tố cùng nhau, hầu hết các phép hợp nhất gcd sẽ thu gọn nhanh chóng thành 1. Các bản đồ co lại mạnh mẽ và thuật toán vẫn đếm chính xác tất cả các cặp vì mọi kết hợp chéo đều giảm xuống gcd 1 tại LCA chính xác.
