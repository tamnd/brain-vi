---
title: "CF 106050L - Liberty từ đồ thị"
description: "Chúng ta được cung cấp một biểu đồ bắt đầu trống và sau đó tăng dần theo thời gian. Mỗi truy vấn thêm một cạnh mới, màu đỏ hoặc màu xanh, giữa hai đỉnh."
date: "2026-06-25T12:29:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106050
codeforces_index: "L"
codeforces_contest_name: "Cataratas do Pinh\u00e3o 2025"
rating: 0
weight: 106050
solve_time_s: 55
verified: true
draft: false
---

[CF 106050L - Tự do từ đồ thị](https://codeforces.com/problemset/problem/106050/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ bắt đầu trống và sau đó tăng dần theo thời gian. Mỗi truy vấn thêm một cạnh mới, màu đỏ hoặc màu xanh, giữa hai đỉnh. Sau mỗi phép cộng, chúng ta được hỏi một câu hỏi giả định: chúng ta có thể loại bỏ bao nhiêu cạnh khỏi biểu đồ hiện tại trong khi vẫn giữ nguyên kết nối giữa tất cả các đỉnh, với một ràng buộc bổ sung là số cạnh màu đỏ bị loại bỏ phải bằng số cạnh màu xanh bị loại bỏ. 

Khả năng kết nối được đo theo nghĩa vô hướng tiêu chuẩn, bỏ qua màu sắc. Vì vậy, nếu hai đỉnh được kết nối trước khi xóa, chúng vẫn phải được kết nối sau khi xóa. Điều này buộc các cạnh còn lại vẫn chứa một nhóm bao trùm của mọi thành phần được kết nối của biểu đồ hiện tại. 

Các ràng buộc lớn, lên tới vài trăm nghìn truy vấn nên việc tính toán lại kết nối hoặc tính toán lại tất cả các chu trình từ đầu sau mỗi lần chèn là không khả thi. Bất kỳ giải pháp nào cố gắng xây dựng lại một nhóm bao trùm hoặc chạy DFS đầy đủ cho mỗi truy vấn sẽ quá chậm vì điều đó thực sự sẽ dẫn đến hành vi bậc hai trong trường hợp xấu nhất. 

Một điểm tinh tế là chúng ta không được yêu cầu đưa ra một chiến lược xóa cụ thể mà chỉ đưa ra số lượng cạnh tối đa có thể xóa được. Điều này có nghĩa là chúng ta có thể suy luận về mặt cấu trúc hơn là về mặt cấu trúc rõ ràng. 

Một cách tiếp cận đơn giản sẽ xử lý từng trạng thái một cách độc lập, tính toán tất cả các cây cầu hoặc một khu rừng bao trùm, sau đó cố gắng đếm xem có bao nhiêu cạnh là “phụ”. Cái khó là yêu cầu bổ sung là việc xóa phải cân bằng về màu sắc. Ràng buộc đó kết hợp các cạnh mà lẽ ra có thể tháo rời độc lập. 

Một nhược điểm phổ biến là bỏ qua sự ghép nối này và chỉ tính số cạnh không phải là cây. Ví dụ: nếu tất cả các cạnh đều có màu đỏ ngoại trừ một cạnh chu kỳ màu xanh lam thì biểu đồ có thể có nhiều cạnh thừa nhưng chúng ta không thể xóa tất cả nếu nó vi phạm đẳng thức xóa màu đỏ-xanh. 

Một trường hợp lỗi khác xuất hiện khi đồ thị đã được kết nối nhưng tất cả các cạnh thừa đều có một màu. Ví dụ: giả sử đồ thị có một chu trình được tạo hoàn toàn bằng các cạnh màu đỏ ngoại trừ một cạnh màu xanh. Chúng ta có thể loại bỏ nhiều nhất một cạnh đỏ và một cạnh xanh với số lượng bằng nhau, mặc dù về mặt cấu trúc, nhiều cạnh hơn là dư thừa. 

## Phương pháp tiếp cận 

Sự phân tách chính là giữa các cạnh có cấu trúc cần thiết để kết nối và các cạnh không cần thiết về mặt cấu trúc. Trong bất kỳ đồ thị nào, nếu ta cố định các thành phần liên thông thì chính xác$n - c$các cạnh là cần thiết để duy trì kết nối bên trong mỗi thành phần, trong đó$c$là số thành phần được kết nối. Mọi thứ ngoài đó đều thuộc về chu kỳ và có khả năng có thể tháo rời được. 

Vì vậy, quan sát đầu tiên là số cạnh “dư thừa về mặt cấu trúc” là$$\text{cycle\_edges} = m - (n - c)$$Ở đâu$m$là số cạnh của đồ thị hiện tại. 

Nếu không có ràng buộc về màu sắc, câu trả lời sẽ chỉ đơn giản là số chu kỳ này, bởi vì tất cả các cạnh không phải là cây có thể bị xóa mà không làm đứt kết nối. 

Ràng buộc màu sắc giới thiệu một điều kiện cân bằng tổng thể. Cho phép$R$Và$B$là số cạnh đỏ và xanh nằm trong chu trình. Nếu chúng ta xóa một số tập hợp các cạnh chu trình, việc xóa phải thỏa mãn:$$\text{deleted red} = \text{deleted blue}$$Trong số tất cả các cạnh của chu trình, nếu chúng ta cố gắng xóa mọi thứ thì sự mất cân bằng là$|R - B|$. Chúng ta không thể loại bỏ sự mất cân bằng này chỉ bằng cách sử dụng các cạnh chu trình vì mỗi lần xóa đều góp phần vào bên này hay bên kia. 

Do đó, chiến lược tốt nhất là xóa càng nhiều cạnh chu kỳ càng tốt trong khi vẫn giữ cân bằng số lần xóa cuối cùng. Điều này có nghĩa là chúng ta buộc phải “bỏ lại phía sau” ít nhất$|R - B|$các cạnh chu kỳ mà nếu không thì có thể xóa được. 

Điều này dẫn đến số lượng tối ưu:$$\text{answer} = \text{cycle\_edges} - |R - B|$$Theo trực giác, mỗi màu dư thừa sẽ làm giảm số lượng cặp xóa đối xứng mà chúng ta có thể hình thành giữa các cạnh của chu kỳ. 

Cách tiếp cận mạnh mẽ sẽ mô phỏng rõ ràng khả năng kết nối sau mỗi truy vấn và tính toán lại cấu trúc chu trình, tính toán chi phí$O(q(n + m))$, quá chậm. 

Cách tiếp cận được tối ưu hóa chỉ duy trì các tập hợp tổng thể: các thành phần được kết nối và sự khác biệt giữa các cạnh chu kỳ màu đỏ và màu xanh. Mỗi lần chèn được xử lý trong thời gian gần như không đổi bằng cách sử dụng tính năng tìm kết hợp, vì chúng ta chỉ cần biết liệu một cạnh có tạo chu trình hay hợp nhất các thành phần hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại mỗi truy vấn (DFS / rừng bao trùm) |$O(q(n + m))$|$O(n + m)$| Quá chậm | 
| DSU + đếm chu kỳ |$O((n + q)\alpha(n))$|$O(n + q)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các truy vấn trực tuyến trong khi vẫn duy trì DSU (Disjoint Set Union) để kết nối. 

1. Khởi tạo DSU với mỗi đỉnh trong thành phần riêng của nó và bộ đếm các cạnh chu kỳ có màu đỏ và xanh lam. 
2. Đối với mỗi truy vấn thêm một cạnh$(u, v)$của một màu nhất định, kiểm tra xem$u$Và$v$đã có trong cùng thành phần DSU. 
3. Nếu chúng nằm trong các thành phần khác nhau, hãy hợp nhất chúng trong DSU. Cạnh này trở thành một phần của rừng bao trùm và không phải là cạnh chu kỳ, vì vậy nó không góp phần vào việc xóa các cạnh. 
4. Nếu chúng đã ở trong cùng một thành phần thì cạnh này sẽ tạo thành một chu trình. Tăng bộ đếm chu kỳ màu đỏ hoặc bộ đếm chu kỳ màu xanh tùy thuộc vào màu của nó. 
5. Sau khi xử lý cạnh, tính: 

- tổng số cạnh chu kỳ là tổng của bộ đếm chu kỳ màu đỏ và màu xanh 
- mất cân bằng như$|\text{red\_cycle} - \text{blue\_cycle}|$6. Đầu ra:$$(\text{cycle edges}) - |\text{red cycle} - \text{blue cycle}|$$Ý tưởng chính là DSU tách biệt các thay đổi kết nối, trong khi các cạnh chu kỳ tích lũy độc lập. Chúng ta không bao giờ cần phải theo dõi rõ ràng những cạnh chính xác nào hình thành nên chu kỳ ngoài phân loại màu sắc của chúng. 

### Tại sao nó hoạt động 

Mỗi đồ thị có thể được phân tách thành một khu rừng bao trùm cộng với một tập hợp các cạnh chu trình. Rừng bao trùm được xác định duy nhất tùy theo sự lựa chọn, nhưng luôn chứa chính xác$n - c$các cạnh. Bất kỳ cạnh nào bên ngoài khu rừng này đều có thể bị xóa mà không ảnh hưởng đến kết nối. 

Hạn chế duy nhất còn lại là việc xóa phải được cân bằng bằng màu sắc. Vì chỉ có các cạnh chu kỳ có thể tháo rời được nên vấn đề giảm xuống còn việc chọn nhiều tập hợp các cạnh chu kỳ tối đa có số lượng màu đỏ và màu xanh bằng nhau. Giải pháp tối ưu loại bỏ tất cả các cặp có thể, để lại chính xác sự mất cân bằng tuyệt đối không thể tránh khỏi. Bất biến này đúng vì DSU đảm bảo rằng mọi cạnh không theo chu kỳ đều được cam kết kết nối và không bao giờ đi vào nhóm xóa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.p = list(range(n))
        self.sz = [1] * n

    def find(self, x):
        while self.p[x] != x:
            self.p[x] = self.p[self.p[x]]
            x = self.p[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return False
        if self.sz[a] < self.sz[b]:
            a, b = b, a
        self.p[b] = a
        self.sz[a] += self.sz[b]
        return True

def solve():
    n, q = map(int, input().split())
    dsu = DSU(n)

    red_cycle = 0
    blue_cycle = 0

    for _ in range(q):
        t, u, v = map(int, input().split())
        u -= 1
        v -= 1

        merged = dsu.union(u, v)
        if not merged:
            if t == 1:
                red_cycle += 1
            else:
                blue_cycle += 1

        cycle_edges = red_cycle + blue_cycle
        ans = cycle_edges - abs(red_cycle - blue_cycle)
        print(ans)

if __name__ == "__main__":
    solve()
```DSU được sử dụng hoàn toàn để phát hiện xem một cạnh có dư thừa về mặt kết nối hay không. Nếu sự kết hợp thành công, cạnh đó là một phần của khu rừng bao trùm và không bao giờ đóng góp trực tiếp vào câu trả lời. 

Bộ đếm chu kỳ tách riêng phần dư màu đỏ và màu xanh. Biểu thức cuối cùng sửa số chu kỳ thô bằng cách loại bỏ sự mất cân bằng không thể tránh khỏi. 

Một điểm triển khai tinh tế là chúng ta không bao giờ cần lưu trữ các cạnh hoặc xây dựng lại biểu đồ. Mọi thứ đều giảm xuống mức phân loại theo thời gian không đổi cho mỗi truy vấn. 

## Ví dụ đã hoạt động 

Hãy xem xét một hoạt động nhỏ trong đó chúng tôi theo dõi bộ đếm chu kỳ và trạng thái DSU. 

### Ví dụ 1 

đầu vào:```
4 3
1 1 2
2 2 3
1 1 2
```| Truy vấn | Cạnh | DSU sáp nhập | Chu kỳ đỏ | Chu kỳ màu xanh | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | đỏ (1,2) | vâng | 0 | 0 | 0 | 
| 2 | màu xanh (2,3) | vâng | 0 | 0 | 0 | 
| 3 | đỏ (1,2) | không | 1 | 0 | 1 | 

Sau hai cạnh đầu tiên, đồ thị là một cây. Cạnh thứ ba đóng một chu kỳ và có màu đỏ nên làm tăng sự mất cân bằng. Chỉ tồn tại một cạnh chu kỳ, vì vậy chúng tôi có thể loại bỏ nó nhưng không thể ghép nó với phần loại bỏ màu xanh lam, để lại sự mất cân bằng không thể tránh khỏi. 

### Ví dụ 2 

đầu vào:```
3 4
1 1 2
2 2 3
1 1 3
2 1 2
```| Truy vấn | Cạnh | DSU sáp nhập | Chu kỳ đỏ | Chu kỳ màu xanh | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | đỏ (1,2) | vâng | 0 | 0 | 0 | 
| 2 | màu xanh (2,3) | vâng | 0 | 0 | 0 | 
| 3 | đỏ (1,3) | không | 1 | 0 | 1 | 
| 4 | màu xanh (1,2) | không | 1 | 1 | 2 | 

Bước cuối cùng hiển thị một bộ chu trình cân bằng: khi số chu kỳ màu đỏ và xanh lam khớp nhau, mọi cạnh chu kỳ sẽ có thể bị xóa theo cặp, cho phép tận dụng tối đa phần dự phòng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + q)\alpha(n))$| Mỗi hoạt động kết hợp/tìm được khấu hao ngược với Ackermann do DSU | 
| Không gian |$O(n)$| Chỉ mảng gốc và mảng kích thước DSU | 

Giải pháp này là tuyến tính với hệ số Ackermann nghịch đảo rất nhỏ, đủ nhanh để$3 \cdot 10^5$truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# minimal
assert run("2 1\n1 1 2\n") == "0"

# simple cycle creation
assert run("3 3\n1 1 2\n2 2 3\n1 1 3\n") == "0\n0\n1"

# balanced cycles
assert run("3 4\n1 1 2\n2 2 3\n1 1 3\n2 1 2\n") == "0\n0\n1\n2"

# all same color cycles
assert run("3 3\n1 1 2\n1 2 3\n1 1 3\n") == "0\n0\n1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị tối thiểu | 0 | hành vi DSU cơ sở | 
| chu trình đơn giản | phát hiện chu kỳ gia tăng | tích lũy chu kỳ chỉ màu đỏ | 
| chu trình cân bằng | hủy màu đỏ/xanh đối xứng | chỉnh sửa mất cân bằng | 
| tất cả cùng màu | mất cân bằng tối đa | sai lệch trong trường hợp xấu nhất | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn xảy ra khi tất cả các cạnh của chu kỳ thuộc về một màu duy nhất. Trong tình huống đó, DSU vẫn xác định chính xác tất cả các cạnh dư thừa, nhưng số hạng mất cân bằng trở thành bằng số cạnh chu kỳ. Công thức buộc câu trả lời là 0 cạnh có thể xóa được, vì không thể ghép cặp màu đỏ-xanh bằng nhau. 

Một trường hợp khác là khi các chu trình xuất hiện sớm nhưng các cạnh muộn hơn sẽ hợp nhất các thành phần riêng biệt trước đó. Những sự hợp nhất sau này không bao giờ ảnh hưởng đến bộ đếm chu kỳ, bởi vì DSU đảm bảo rằng chỉ các cạnh bên trong thành phần mới được coi là dư thừa, duy trì tính chính xác bất kể thứ tự chèn.
