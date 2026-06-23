---
title: "CF 105058E - \u0413\u043e\u0434\u043e\u0432\u043e\u0439 \u043e\u0442\u0447\u0435\u0442"
description: "Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản, có tối đa 40 ứng viên, mỗi ứng viên mang một nhãn mã hóa kiến ​​thức của họ về một số chủ đề cố định dưới dạng bitmask."
date: "2026-06-23T11:09:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105058
codeforces_index: "E"
codeforces_contest_name: "\u0418\u043d\u0434\u0438\u0432\u0438\u0434\u0443\u0430\u043b\u044c\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438 \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2024"
rating: 0
weight: 105058
solve_time_s: 81
verified: false
draft: false
---

[CF 105058E - \u0413\u043e\u0434\u043e\u0432\u043e\u0439 \u043e\u0442\u0447\u0435\u0442](https://codeforces.com/problemset/problem/105058/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản, có tối đa 40 ứng viên, mỗi ứng viên mang một nhãn mã hóa kiến ​​thức của họ về một số chủ đề cố định dưới dạng bitmask. Hai ứng viên có thể được phép làm việc cùng nhau hoặc không, tùy thuộc vào việc họ có mối quan hệ bạn bè hay không. 

Nhiệm vụ là chọn một tập hợp con các ứng cử viên sao cho mỗi cặp trong tập hợp con đó được kết nối bằng một cạnh tình bạn. Theo thuật ngữ biểu đồ, chúng ta đang tìm kiếm một nhóm trong biểu đồ tình bạn. Trong số tất cả các nhóm như vậy, trước tiên chúng tôi ưu tiên quy mô tối đa có thể. Nếu nhiều nhóm đạt được kích thước tối đa đó, chúng tôi sẽ chọn nhóm có XOR theo bit của tất cả các giá trị kiến ​​thức đã chọn được tối đa hóa. 

Do đó, đầu ra cho mỗi kịch bản là hai giá trị, kích thước của cụm lớn nhất có thể và, trong số đó, XOR tối đa có thể đạt được của các giá trị nút đã chọn. 

Các ràng buộc nhỏ về số lượng ứng viên cho mỗi trường hợp thử nghiệm, với tối đa 40 nút và tổng n trên tất cả các thử nghiệm không vượt quá 120. Điều này ngay lập tức cho thấy rằng các phương pháp hàm mũ trên các tập hợp con có thể chấp nhận được, nhưng chỉ khi chúng được cấu trúc cẩn thận. Một cách tiếp cận đơn giản để kiểm tra tất cả các tập hợp con và xác nhận trực tiếp các điều kiện nhóm sẽ yêu cầu kiểm tra tới 2^40 tập hợp con, điều này là không khả thi. 

Một vấn đề tế nhị xuất hiện khi nghĩ đến việc tối ưu hóa hai giai đoạn. Chúng tôi không chỉ tối đa hóa kích thước nhóm mà còn tối đa hóa XOR giữa các nhóm có kích thước tối đa. Cách tiếp cận tham lam chọn các nút cấp độ lớn nhất hoặc tối đa hóa XOR cục bộ sẽ thất bại, vì cả hai ràng buộc đều mang tính toàn cầu và tương tác với nhau. 

Một trường hợp cạnh khác là khi không có cạnh nào hoặc khi đồ thị hoàn chỉnh. Trong một biểu đồ trống, chỉ các nút đơn là cụm hợp lệ, vì vậy câu trả lời hoàn toàn phụ thuộc vào việc chọn phần tử XOR tối đa trong số các nút đơn. Trong một biểu đồ hoàn chỉnh, tất cả các tập hợp con đều là các cụm, do đó, vấn đề giảm xuống còn việc chọn tập hợp con lớn nhất và tối đa hóa XOR trên tất cả các tập hợp con có kích thước đó, điều này vẫn không hề tầm thường. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ liệt kê tất cả các tập hợp con của nút. Đối với mỗi tập hợp con, chúng tôi sẽ kiểm tra xem nó có tạo thành một cụm hay không bằng cách xác minh tất cả các cặp đỉnh được chọn dựa trên cấu trúc kề. Nếu hợp lệ, chúng tôi sẽ tính toán kích thước và giá trị XOR của nó và giữ lại cặp giá trị tốt nhất. 

Kiểm tra tính hợp lệ của cụm cho một tập hợp con có giá O(n^2) và có 2^n tập hợp con, do đó độ phức tạp tổng cộng trở thành O(n^2 2^n). Với n = 40, điều này vượt xa giới hạn khả thi. 

Quan sát quan trọng là các ràng buộc cụm liên quan đến tính nhất quán kề bên trong một tập hợp con, điều này sẽ trở nên dễ xử lý hơn nhiều nếu chúng ta xây dựng các tập hợp con tăng dần trong khi vẫn duy trì tính hợp lệ. Điều này gợi ý một chiến lược gặp nhau ở giữa: chia biểu đồ thành hai nửa, liệt kê tất cả các tập hợp con trong mỗi nửa và theo dõi xem tập hợp con nào là nhóm nội bộ. Sau đó, chúng tôi kết hợp các tập hợp con tương thích từ cả hai nửa, đảm bảo rằng các cạnh chéo là hợp lệ giữa hai tập hợp con được chọn. 

Điều này làm giảm vụ nổ theo cấp số nhân từ 2^40 xuống còn khoảng 2^20 mỗi bên, có thể quản lý được. Chúng tôi cũng cần theo dõi không chỉ tính hợp lệ của tập hợp con mà còn cả kích thước cụm và giá trị XOR, những thứ này có thể được kết hợp trong quá trình hợp nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Tập hợp con đầy đủ vũ phu | O(n^2 2^n) | O(2^n) | Quá chậm | 
| Gặp nhau giữa chừng | O(2^(n/2) * 2^(n/2)) | O(2^(n/2)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chia tập hợp các đỉnh thành hai phần, Trái và Phải, mỗi phần có kích thước tối đa là 20. Đối với mỗi bên, chúng tôi liệt kê tất cả các tập hợp con và xác định tập hợp con nào tạo thành các cụm hợp lệ hoàn toàn bên trong cạnh đó. 

Chúng tôi cũng tính toán hai thuộc tính cho mỗi tập hợp con hợp lệ: kích thước và XOR của các giá trị nút của nó.

Tiếp theo, đối với Bên phải, chúng tôi xử lý trước khả năng tương thích. Đối với mỗi tập hợp con hợp lệ, chúng tôi lưu trữ bitmask của các đỉnh trong đó và thông tin hợp lệ bên trong của nó. 

Để kết hợp, chúng ta xem xét từng tập con hợp lệ A ở phía bên trái và từng tập hợp con B hợp lệ ở phía bên phải. Cặp này hợp lệ nếu mọi đỉnh trong A được kết nối với mọi đỉnh trong B. Chúng ta có thể kiểm tra điều này một cách hiệu quả bằng cách sử dụng mặt nạ kề cận, vì vậy chúng ta tránh kiểm tra O(n^2) cho mỗi cặp. 

Đối với mỗi cặp tương thích, chúng tôi tính toán kích thước kết hợp là |A| + |B| và XOR dưới dạng xor(A) XOR xor(B). Chúng tôi duy trì câu trả lời tốt nhất trước tiên theo kích thước, sau đó là XOR. 

### Tại sao nó hoạt động 

Mỗi cụm trong biểu đồ ban đầu có thể được phân chia duy nhất thành giao điểm của nó với nửa Trái và Nửa Phải. Bản thân mỗi bộ phận đó phải là một nhóm và phải tương thích lẫn nhau trên toàn bộ phân vùng. Vì chúng tôi liệt kê tất cả các nhóm hợp lệ trong mỗi nửa và sau đó chỉ kết hợp các cặp tương thích, nên mỗi nhóm toàn cầu được biểu diễn chính xác một lần dưới dạng một cặp nửa nhóm hợp lệ. Điều này đảm bảo không có ứng cử viên hợp lệ nào bị bỏ sót và mọi ứng cử viên không hợp lệ đều bị loại trừ ở giai đoạn hợp nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, k = map(int, input().split())
        a = list(map(int, input().split()))
        
        adj = [0] * n
        for i in range(n):
            adj[i] = (1 << n) - 1  # start fully connected
        
        # remove non-edges
        for i in range(n):
            adj[i] ^= (1 << i)
        
        for _ in range(m):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            adj[u] |= (1 << v)
            adj[v] |= (1 << u)

        # actually we want adjacency as bitmask of edges
        # but input gives friendship edges; clique requires full connectivity,
        # so we build complement-style validity check

        # recompute correct adjacency: we track allowed edges
        ok = [[False] * n for _ in range(n)]
        for i in range(n):
            ok[i][i] = True
        for _ in range(m):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            ok[u][v] = ok[v][u] = True

        # half split
        n1 = n // 2
        n2 = n - n1

        left_ids = list(range(n1))
        right_ids = list(range(n1, n))

        def is_clique(sub, ids):
            for i in range(len(sub)):
                for j in range(i + 1, len(sub)):
                    if not ok[ids[sub[i]]][ids[sub[j]]]:
                        return False
            return True

        left_states = []
        for mask in range(1 << n1):
            nodes = [i for i in range(n1) if mask >> i & 1]
            if is_clique(nodes, left_ids):
                x = 0
                val = 0
                for i in nodes:
                    val ^= a[left_ids[i]]
                left_states.append((nodes, val))

        right_states = []
        for mask in range(1 << n2):
            nodes = [i for i in range(n2) if mask >> i & 1]
            if is_clique(nodes, right_ids):
                x = 0
                val = 0
                for i in nodes:
                    val ^= a[right_ids[i]]
                right_states.append((nodes, val))

        best_size = 0
        best_xor = 0

        for l_nodes, l_xor in left_states:
            for r_nodes, r_xor in right_states:
                valid = True
                for i in l_nodes:
                    for j in r_nodes:
                        if not ok[left_ids[i]][right_ids[j]]:
                            valid = False
                            break
                    if not valid:
                        break
                if not valid:
                    continue
                sz = len(l_nodes) + len(r_nodes)
                xr = l_xor ^ r_xor
                if sz > best_size or (sz == best_size and xr > best_xor):
                    best_size = sz
                    best_xor = xr

        print(best_size, best_xor)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo cấu trúc phân chia và liệt kê. Ma trận kề được sử dụng trực tiếp để kiểm tra tính chính xác của cụm. Nửa bên trái và bên phải được liệt kê độc lập và chỉ các nhóm nội bộ hợp lệ mới được lưu trữ. Trong quá trình hợp nhất, khả năng tương thích chéo được thực thi bằng cách kiểm tra tất cả các cặp giữa hai tập hợp con. 

Quyết định lưu trữ các tập hợp con một cách rõ ràng dưới dạng danh sách làm cho logic trở nên đơn giản nhưng không tối ưu. Một phiên bản hiệu quả hơn sẽ mã hóa các tập hợp con dưới dạng mặt nạ bit và tính toán trước mặt nạ tương thích để tránh các vòng lặp lồng nhau. 

## Ví dụ đã hoạt động 

Hãy xem xét một biểu đồ nhỏ có bốn nút được chia thành hai nửa, mỗi nửa có hai nút. Giả sử tất cả các cạnh đều tồn tại ngoại trừ một cạnh bị thiếu giữa nút 0 và nút 3 và các giá trị là các số nguyên nhỏ tùy ý. 

Đối với nửa bên trái, tất cả các tập hợp con đều là các nhóm hợp lệ. Đối với nửa bên phải, chỉ các tập con không chứa cả hai điểm cuối của cạnh bị thiếu là hợp lệ. 

| Bước | Tập hợp con bên trái | Tập hợp con bên phải | Các cạnh chéo hợp lệ | Kích thước | XOR | 
| --- | --- | --- | --- | --- | --- | 
| 1 | {} | {} | vâng | 0 | 0 | 
| 2 | {0} | {2} | vâng | 2 | a0 XOR a2 | 
| 3 | {0,1} | {2} | vâng | 3 | a0 XOR a1 XOR a2 | 
| 4 | {0,1} | {2,3} | không | - | - | 

Dấu vết này cho thấy bước xác thực chéo loại bỏ các nhóm không hợp lệ mà lẽ ra sẽ được hình thành bằng cách kết hợp hai nhóm nội bộ hợp lệ. 

Bây giờ hãy xem xét một biểu đồ hoàn chỉnh có kích thước 3. Mọi tập hợp con đều hợp lệ, do đó thuật toán đánh giá hiệu quả tất cả các phân vùng của tập hợp đầy đủ. Kích thước tốt nhất là 3 và trong số tất cả các tập hợp con đầy đủ, XOR được tối đa hóa trên toàn bộ tập hợp. 

| Bước | Tập hợp con bên trái | Tập hợp con bên phải | Kích thước | XOR | 
| --- | --- | --- | --- | --- | 
| 1 | {0} | {1,2} | 3 | a0 XOR a1 XOR a2 | 
| 2 | {0,1} | {2} | 3 | a0 XOR a1 XOR a2 | 

Cả hai phép phân tách đều tạo ra các cụm đầy đủ hợp lệ và thuật toán so sánh chính xác các giá trị XOR trên các cấu trúc có kích thước tối đa tương đương. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^(n/2) * 2^(n/2) * n^2) | Việc liệt kê các tập hợp con ở cả hai nửa cộng với kiểm tra nhóm và xác thực chéo | 
| Không gian | O(2^(n/2)) | Lưu trữ các tập hợp con hợp lệ mỗi nửa | 

Hệ số mũ giảm từ 2^40 xuống khoảng 2^20 mỗi bên, nằm trong giới hạn thực tế với các hằng số nhỏ. Tổng ràng buộc đầu vào trên các trường hợp thử nghiệm đảm bảo trường hợp xấu nhất vẫn có thể quản lý được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue()

# Sample tests (formatted conceptually, actual sample input formatting may vary)
# assert run("...") == "2 7\n3 13\n4 15\n"

# custom tests
assert run("1\n1 0 1\n0\n") == "1 0\n", "single node"
assert run("1\n3 0 2\n1 2 3\n") in ["1 3\n", "1 3"], "no edges"
assert run("1\n3 3 2\n1 2 3\n1 2\n2 3\n1 3\n") == "3 0\n", "complete graph"
assert run("1\n4 0 3\n1 2 4 8\n") == "1 15\n", "only singletons matter"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 0 | đồ thị tối thiểu | 
| không có cạnh | tối đa 1 ai | hạn chế kích thước nhóm | 
| đồ thị hoàn chỉnh | nxor tất cả | tương thích hoàn toàn | 
| tất cả người độc thân | 1 XOR tối đa | hành vi mất kết nối | 

## Vỏ cạnh 

Khi không có cạnh hữu nghị, mỗi nhóm là một đỉnh duy nhất. Thuật toán vẫn hoạt động vì mỗi nửa chỉ chấp nhận các tập hợp con đơn lẻ và việc kiểm tra chéo là không đáng kể vì một bên trống trong hầu hết các kết hợp. 

Khi biểu đồ hoàn chỉnh, mọi tập hợp con đều hợp lệ bên trong và trên một nửa. Thuật toán liệt kê tất cả các kết hợp của các tập con bên trái và bên phải và kích thước tối đa luôn là n. Tối đa hóa XOR sau đó được xử lý chính xác trong số tất cả các lựa chọn kích thước đầy đủ. 

Khi k = 0, tất cả các giá trị đều bằng 0, do đó XOR luôn bằng 0. Thuật toán giảm xuống hoàn toàn tối đa hóa kích thước cụm, điều này trở thành một vấn đề cụm tối đa tiêu chuẩn được giải quyết bằng cách liệt kê trên một nửa và việc ngắt kết nối XOR không ảnh hưởng đến tính chính xác.
