---
title: "CF 102218D - Mạng động"
description: "Mạng là một cái cây mọc lên từng máy tính một. Máy tính 1 ban đầu tồn tại. Mỗi máy tính mới sẽ nhận được id không được sử dụng tiếp theo, vì vậy khi hiện có máy tính hiện tại, máy tính mới sẽ nhận được id curr + 1. Nó được gắn bởi một cạnh với máy tính đã có sẵn."
date: "2026-08-18T22:46:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102218
codeforces_index: "D"
codeforces_contest_name: "2019, XI Annual Programming Contest by ESCOM-IPN"
rating: 0
weight: 102218
solve_time_s: 870
verified: false
draft: false
---

[CF 102218D - Mạng động](https://codeforces.com/problemset/problem/102218/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 14p 30s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mạng là một cái cây mọc lên từng máy tính một. Máy tính 1 ban đầu tồn tại. Mọi máy tính mới đều nhận được id chưa được sử dụng tiếp theo, vì vậy khi hiện tại có`curr`máy tính, máy tính mới có id`curr + 1`. Nó được gắn bằng một cạnh vào một máy tính đã có sẵn. 

Truy vấn loại 1 chỉ định gián tiếp máy tính gốc của máy tính mới. Truy vấn loại 2 yêu cầu đường đi ngắn nhất giữa hai máy tính hiện có. Câu trả lời đếm các máy tính chứ không phải các cạnh, vì vậy một đường dẫn chứa`k`cạnh có câu trả lời`k + 1`. 

Đầu vào được mã hóa có chủ ý bằng câu trả lời trước đó. Nếu như`last`là câu trả lời gần đây nhất và có`curr`máy tính hiện có, giá trị loại 1`p'`đại diện cho cha mẹ 

[ 
p=(p'+last)\bmod curr+1. 
] 

Đối với loại 2, hai điểm cuối được giải mã theo cách giống hệt nhau. Điều này có nghĩa là các truy vấn phải được xử lý trực tuyến nghiêm ngặt. Chúng tôi không thể giải mã tất cả các truy vấn trước rồi xử lý trước cây cuối cùng vì giá trị cần thiết để giải mã truy vấn tiếp theo được tạo ra bởi truy vấn hiện tại. 

Có thể có tới (2\cdot10^5) thao tác, do đó cây cuối cùng cũng có tối đa (2\cdot10^5+1) máy tính. Một giải pháp đi qua toàn bộ đường dẫn từ gốc tới nút cho mỗi truy vấn có thể mất (O(Q^2)) thời gian. Với (Q=2\cdot10^5), điều đó có thể đạt được khoảng (4\cdot10^{10}) lượt truyền tải gốc, vượt xa giới hạn hai giây cho phép. Chúng tôi cần công việc logarit cho mỗi hoạt động. 

Cấu trúc cây cung cấp cho chúng ta một thuộc tính đặc biệt hữu ích: mọi đỉnh mới được tạo đều là một lá và cha mẹ của nó đã tồn tại. Do đó, tất cả thông tin về đỉnh mới có thể được tính toán ngay lập tức từ đỉnh cha của nó. Chúng ta không bao giờ phải xây dựng lại hoặc duyệt qua cây hiện có sau khi chèn. 

Có một số trường hợp nguy hiểm có thể âm thầm phá vỡ quá trình triển khai. Đầu tiên là truy vấn cùng một máy tính hai lần. Ví dụ,```
1
2 0 0
```Chỉ có máy tính 1 nên cả hai điểm cuối được giải mã là 1 và đầu ra đúng là`1`. Khoảng cách giữa các cạnh bằng 0, nhưng bài toán yêu cầu số lượng máy tính nên đáp án là một. Một triển khai quên mất trận chung kết`+1`sẽ in bằng không. 

Trường hợp cạnh thứ hai là một máy tính mới được thêm vào có id gốc là id hợp lệ lớn nhất hiện tại. Coi như```
4
1 0
1 1
2 0 0
2 0 1
```Lần chèn đầu tiên tạo ra máy tính 2 trong máy tính 1 và xuất ra`2`. Việc chèn thứ hai có`last=2`, vậy cha mẹ của nó là`(1+2)%2+1=2`, tạo máy tính 3 dưới máy tính 2 và xuất ra`3`. Truy vấn thứ ba giải mã thành`(1,1)`và đầu ra`1`. Truy vấn cuối cùng giải mã thành`(2,3)`, đường đi của ai là`2-3`, vì vậy đầu ra là`2`. Đầu ra đúng là```
2
3
1
2
```Một lỗi thường gặp là sử dụng giá trị của`curr`sau khi tăng nó khi giải mã cha mẹ. Đỉnh cha phải được giải mã bằng số lượng máy tính trước khi chèn đỉnh mới. 

Trường hợp cạnh thứ ba là`last`thay đổi sau mỗi truy vấn, bao gồm cả truy vấn loại 2. Ví dụ,```
3
1 0
2 0 0
2 0 0
```Sau khi chèn,`last=2`. Truy vấn tiếp theo giải mã cả hai điểm cuối là 1 và đầu ra`1`, Vì thế`last`trở thành 1. Do đó, truy vấn cuối cùng vẫn có cả hai điểm cuối bằng 2 chỉ khi số lượng máy tính hiện tại và giá trị được mã hóa tạo ra kết quả đó. Tái sử dụng cái cũ hơn`last`value sẽ giải mã các truy vấn tiếp theo không chính xác. Mã hóa là một phần trạng thái của thuật toán, không chỉ đơn thuần là tiền xử lý đầu vào. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là lưu trữ phần tử mẹ của mỗi máy tính và đối với truy vấn giữa`u`Và`v`, đi lên từ cả hai đỉnh cho đến khi đường đi của chúng gặp nhau. Điều này đúng vì mạng luôn có dạng cây nên giữa hai máy tính chỉ có duy nhất một đường dẫn. Nếu chúng ta có thể tìm được tổ tiên chung thấp nhất của chúng thì số cạnh trên đường đi là 

[ 
độ sâu[u]+độ sâu[v]-2\cdot độ sâu[lca]. 
] 

Thêm một sẽ cho số lượng máy tính cần thiết. 

Vấn đề là thời gian chạy. Hãy xem xét một cái cây chỉ có một chuỗi dài. Một truy vấn liên quan đến máy tính sâu nhất có thể yêu cầu (O(n)) bước gốc. Nếu cây có (2\cdot10^5) đỉnh và chúng ta thực hiện (2\cdot10^5) các truy vấn như vậy thì tổng số có thể là khoảng (4\cdot10^{10}) thao tác. Phương pháp vũ phu là đúng, nhưng về cơ bản là quá chậm. 

Quan sát làm thay đổi vấn đề là việc chèn thêm chỉ thêm lá. Khi một máy tính mới`x`được gắn vào cha mẹ hiện có`p`, toàn bộ chuỗi tổ tiên của nó có thể được tóm tắt ngay lập tức. Cửa hàng`up[k][x]`, tổ tiên của`x`thu được bằng cách di chuyển các cạnh lên trên (2^k). Từ`p`đã được biết đến, sự tái diễn là 

[ 
lên[0][x]=p 
] 

và 

[ 
lên[k][x]=lên[k-1][up[k-1][x]]. 
] 

Không có đỉnh hiện tại nào thay đổi khi`x`được chèn vào, do đó tất cả thông tin nâng nhị phân được tính toán trước đó vẫn có hiệu lực mãi mãi. 

Đây chính xác là cài đặt mà tính năng nâng nhị phân hoạt động tốt. Nó cho phép chúng ta tìm LCA trong thời gian (O(\log n)) bằng cách trước tiên di chuyển đỉnh sâu hơn lên trên cho đến khi cả hai đỉnh có độ sâu bằng nhau, sau đó nâng cả hai đỉnh cùng nhau từ bước nhảy lớn đến bước nhảy nhỏ. Bản thân việc chèn cũng mất (O(\log n)), bởi vì chúng tôi tính toán tổ tiên của đỉnh mới cho tất cả lũy thừa của hai. 

Đầu vào được mã hóa không làm thay đổi cấu trúc dữ liệu. Nó chỉ yêu cầu chúng tôi giải mã một truy vấn trước khi xử lý nó, tính toán câu trả lời của nó và lưu ngay câu trả lời đó vào`last`cho truy vấn tiếp theo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(Q^2)) trường hợp xấu nhất | (O(Q)) | Quá chậm | 
| Tối ưu | (O(Q\log Q)) | (O(Q\log Q)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo máy tính 1 làm root. Đặt độ sâu của nó thành 0 và khởi tạo mọi tổ tiên nâng nhị phân về 0. Gốc không có cha thực sự, vì vậy số 0 là một trọng điểm thuận tiện. 
2. Giữ`curr`bằng số lượng máy tính hiện có và`last`tương đương với câu trả lời trước. Ban đầu cả hai đều có các giá trị cần thiết,`curr=1`Và`last=0`. 
3. Đối với truy vấn loại 1, hãy giải mã truy vấn gốc bằng cách sử dụng`(p_prime + last) % curr + 1`trước khi thay đổi`curr`. Máy tính mới có id`curr + 1`, vì id được gán liên tiếp. 
4. Đặt độ sâu của máy tính mới thành`depth[parent] + 1`và cha mẹ trực tiếp của nó`parent`. Sau đó tính toán mọi tổ tiên cao hơn với`up[k][new] = up[k-1][up[k-1][new]]`. Điều này có tác dụng vì mọi đỉnh được tham chiếu ở phía bên phải đều đã tồn tại trước khi chèn. 
5. Đường dẫn của máy tính mới tới máy tính 1 chứa`depth[new] + 1`máy tính. Đặt giá trị này là`last`và xuất nó. Vì gốc có độ sâu bằng 0 nên một đỉnh ở độ sâu`d`có chính xác`d+1`các đỉnh trên đường đi tới gốc của nó. 
6. Đối với truy vấn loại 2, giải mã cả hai điểm cuối bằng cách sử dụng`last`Và`curr`. Cả hai điểm cuối đều được đảm bảo tham chiếu đến các máy tính hiện có vì modulo được lấy theo số đỉnh hiện tại. 
7. Tìm LCA của điểm cuối được giải mã. Đầu tiên nâng điểm cuối sâu hơn cho đến khi độ sâu khớp. Sau đó kiểm tra mức nâng nhị phân từ lớn nhất đến nhỏ nhất và nâng cả hai điểm cuối bất cứ khi nào tổ tiên tương ứng của chúng khác nhau. Sau quá trình này, cha mẹ trực tiếp của họ là LCA. 
8. Tính số cạnh giữa các điểm cuối là`depth[u] + depth[v] - 2 * depth[lca]`. Thêm một vì cả hai điểm cuối đều được tính, lưu kết quả vào`last`, và xuất nó. 

### Tại sao nó hoạt động 

Bất biến quan trọng là đối với mọi máy tính hiện có`v`,`depth[v]`là khoảng cách chính xác của nó tính theo các cạnh từ máy tính 1, và`up[k][v]`là tổ tiên chính xác (2^k) của nó. Khi một lá mới được chèn vào, lá cha của nó đã hợp lệ, do đó phép lặp sẽ tính toán chính xác tất cả các giá trị tổ tiên mới mà không sửa đổi bất kỳ giá trị cũ nào. Sau đó, nâng nhị phân sẽ tìm ra LCA thực sự vì việc cân bằng độ sâu đặt cả hai đỉnh ở cùng một cấp độ và các bước nhảy lớn nhất đến nhỏ nhất tiếp theo sẽ di chuyển chúng lên trên mà không vượt qua LCA của chúng. Do đó, công thức khoảng cách cây tiêu chuẩn cho số cạnh chính xác và việc thêm một cạnh sẽ cho chính xác số lượng máy tính trên đường dẫn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAX_Q = 200000
MAX_N = MAX_Q + 1
LOG = MAX_N.bit_length()

def solve():
    q = int(input())

    depth = [0] * (MAX_N + 1)
    up = [[0] * (MAX_N + 1) for _ in range(LOG)]

    curr = 1
    last = 0

    out = []

    for _ in range(q):
        parts = input().split()
        t = int(parts[0])

        if t == 1:
            p_prime = int(parts[1])

            # Decode using the number of computers before insertion.
            parent = (p_prime + last) % curr + 1

            new_node = curr + 1
            depth[new_node] = depth[parent] + 1
            up[0][new_node] = parent

            for k in range(1, LOG):
                mid = up[k - 1][new_node]
                up[k][new_node] = up[k - 1][mid]

            curr += 1

            last = depth[new_node] + 1
            out.append(str(last))

        else:
            u_prime = int(parts[1])
            v_prime = int(parts[2])

            u = (u_prime + last) % curr + 1
            v = (v_prime + last) % curr + 1

            if depth[u] < depth[v]:
                u, v = v, u

            # Bring u to the same depth as v.
            diff = depth[u] - depth[v]
            bit = 0
            while diff:
                if diff & 1:
                    u = up[bit][u]
                diff >>= 1
                bit += 1

            if u == v:
                lca = u
            else:
                for k in range(LOG - 1, -1, -1):
                    if up[k][u] != up[k][v]:
                        u = up[k][u]
                        v = up[k][v]

                lca = up[0][u]

            last = depth[u] + depth[v] - 2 * depth[lca] + 1
            out.append(str(last))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```các`depth`mảng sử dụng độ sâu cây dựa trên số 0, vì vậy máy tính 1 có độ sâu bằng 0. Điều này làm cho công thức khoảng cách tiêu chuẩn trở nên đặc biệt rõ ràng. Câu trả lời được yêu cầu là khoảng cách cạnh cộng một.`up[0][v]`lưu trữ cha mẹ trực tiếp, trong khi`up[k][v]`lưu trữ các cạnh tổ tiên (2^k) ở trên`v`. Số cấp độ là`MAX_N.bit_length()`. Với nhiều nhất`200001`máy tính, điều này cung cấp đủ mức để thể hiện mọi chênh lệch độ sâu có thể có và mọi bước nhảy LCA có thể xảy ra. 

Để chèn,`curr`vẫn đại diện cho số lượng máy tính cũ trong khi máy tính cha được giải mã. Chỉ sau khi tất cả thông tin của máy tính mới đã được tính toán xong chúng ta mới tăng`curr`. Thứ tự này được yêu cầu bởi công thức mã hóa. 

Đối với truy vấn loại 2, điểm cuối được giải mã trước khi bất kỳ LCA nào hoạt động. Điểm cuối sâu hơn sau đó được nâng lên theo các bit đã đặt của chênh lệch độ sâu. Nếu các đỉnh bằng nhau thì đỉnh đó đã là LCA. Ngược lại, vòng lặp sẽ xem xét các bước nhảy từ lớn nhất đến nhỏ nhất. Khi`up[k][u]`Và`up[k][v]`khác nhau, cả hai đỉnh đều có thể di chuyển lên trên một cách an toàn (2^k), vì LCA của chúng vẫn ở trên hai đỉnh khác biệt đó. Cuối cùng họ trở thành con của LCA. 

Số nguyên Python không tràn cho các giá trị được sử dụng ở đây. Giá trị trung gian lớn nhất trong công thức giải mã chỉ là vài trăm nghìn, khoảng cách nhiều nhất là số lượng máy tính. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Bốn thao tác đầu tiên tạo ra cây được mô tả bằng các thao tác được giải mã. Bảng theo dõi số lượng máy tính trước mỗi thao tác, câu trả lời trước đó, điểm cha hoặc điểm cuối được giải mã và câu trả lời thu được. 

| Bước | Loại |`curr`trước |`last`trước | Hoạt động được giải mã | Mới`curr`| Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | mới 2 dưới 1 | 2 | 2 | 
| 2 | 1 | 2 | 2 | mới 3 dưới 1 | 3 | 2 | 
| 3 | 1 | 3 | 2 | mới 4 dưới 2 | 4 | 3 | 
| 4 | 1 | 4 | 3 | mới 5 dưới 2 | 5 | 3 | 
| 5 | 2 | 5 | 3 | truy vấn 4, 3 | 5 | 4 | 
| 6 | 2 | 5 | 4 | truy vấn 1, 2 | 5 | 2 | 
| 7 | 2 | 5 | 2 | truy vấn 5, 4 | 5 | 3 | 

Sau lần chèn đầu tiên, máy tính 2 cách gốc một cạnh nên đường dẫn gốc của nó chứa hai máy tính. Phần chèn thứ hai cũng gắn vào máy tính 1. Phần chèn thứ ba và thứ tư gắn vào máy tính 2, tạo ra độ sâu 2 cho máy tính 4 và 5. 

Đối với truy vấn khoảng cách đầu tiên, đường dẫn là`4 -> 2 -> 1 -> 3`, chứa bốn máy tính. LCA là máy tính 1. Truy vấn cuối cùng yêu cầu máy tính 5 và 4, dùng chung máy tính 2 với tư cách là máy tính mẹ, vì vậy đường dẫn chứa`5 -> 2 -> 4`, tặng ba máy tính. 

### Mẫu 2 

Ở đây, các giá trị được mã hóa phụ thuộc vào câu trả lời từ cả truy vấn chèn và khoảng cách, vì vậy sẽ rất hữu ích khi theo dõi rõ ràng`last`. 

| Bước | Loại |`curr`trước |`last`trước | Hoạt động được giải mã | Mới`curr`| Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | mới 2 dưới 1 | 2 | 2 | 
| 2 | 2 | 2 | 2 | truy vấn 1, 2 | 2 | 2 | 
| 3 | 1 | 2 | 2 | mới 3 dưới 1 | 3 | 2 | 
| 4 | 1 | 3 | 2 | mới 4 dưới 1 | 4 | 2 | 
| 5 | 2 | 4 | 2 | truy vấn 3, 4 | 4 | 3 | 
| 6 | 2 | 4 | 3 | truy vấn 2, 2 | 4 | 1 | 

Hoạt động thứ hai giải mã thành điểm cuối 1 và 2. Câu trả lời của nó là hai, trở thành`last`giá trị được sử dụng bởi hoạt động thứ ba. Thao tác thứ năm có điểm cuối 3 và 4, cả hai đều là con của máy tính 1, do đó đường dẫn của chúng chứa ba máy tính. Truy vấn cuối cùng giải mã cả hai điểm cuối tới máy tính 2, thể hiện trường hợp khoảng cách bằng 0 và tạo ra một trường hợp theo yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(Q\log Q)) | Mỗi lần chèn sẽ tính toán tổ tiên (O(\log Q)) và mọi truy vấn khoảng cách thực hiện (O(\log Q)) công việc LCA. | 
| Không gian | (O(Q\log Q)) | Bảng nâng nhị phân lưu trữ tổ tiên (O(\log Q)) cho mỗi máy tính trong số tối đa (Q+1). | 

Với (Q\le2\cdot10^5), hệ số logarit nhỏ hơn 20. Thuật toán chỉ thực hiện một lượng công việc nhỏ không đổi ở mỗi cấp độ nâng nhị phân, do đó tổng số công việc phù hợp với giới hạn hai giây. Bảng cũng vừa vặn thoải mái trong phạm vi 256 MB khi triển khai Python. 

## Trường hợp thử nghiệm```python
# This test block is intended to be placed after the solution code.
# It reuses solve() and captures its output.

import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
assert run("""7
1 0
1 2
1 2
1 2
2 0 4
2 1 2
2 2 1
""") == """2
2
3
3
4
2
3""", "sample 1"

# Provided sample 2
assert run("""6
1 1
2 1 2
1 0
1 1
2 0 3
2 2 2
""") == """2
2
2
2
3
1""", "sample 2"

# Minimum-size input, querying the only existing computer.
assert run("""1
2 0 0
""") == """1""", "single vertex"

# Repeated identical queries. They all decode to the same vertex.
assert run("""4
2 0 0
2 0 0
2 0 0
2 0 0
""") == """1
1
1
1""", "all equal endpoints"

# Exercises parent == curr and then a query involving the deepest vertices.
assert run("""4
1 0
1 1
2 0 0
2 0 1
""") == """2
3
1
2""", "boundary parent and self query"

# Maximum number of operations. There is only one computer, so every query
# must decode to (1, 1) and answer 1.
max_q = 200000
max_input = str(max_q) + "\n" + "2 0 0\n" * max_q
max_expected = "1\n" * max_q
assert run(max_input) == max_expected, "maximum query count"

# Large encoded values with curr = 1. Modulo must reduce them correctly.
assert run("""3
1 200000
2 200000 200000
2 0 0
""") == """2
1
1""", "maximum encoded values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 2 0 0`|`1`| Kích thước cây tối thiểu và`+1`chuyển đổi các cạnh sang máy tính | 
| bốn`2 0 0`truy vấn |`1 1 1 1`| Giải mã lặp đi lặp lại và trường hợp`u = v`| 
| Hai lần chèn theo sau là hai truy vấn |`2 3 1 2`| Gốc bằng id hợp lệ lớn nhất, xử lý độ sâu và giải mã điểm cuối | 
|`200000`truy vấn loại 2 giống hệt nhau |`1`lặp đi lặp lại 200000 lần | Số lượng thao tác tối đa và xử lý trực tuyến | 
| Chèn và truy vấn bằng cách sử dụng`200000`|`2 1 1`| Giá trị biên trong đầu vào được mã hóa và hành vi modulo | 

## Vỏ cạnh 

Trường hợp một máy tính được xử lý vì gốc có độ sâu bằng 0 và mọi truy vấn trên nó đều có các cạnh khoảng cách bằng 0. Việc thực hiện thêm một khi chuyển đổi khoảng cách thành một số máy tính. Vì```
1
2 0 0
```cả hai điểm cuối đều giải mã thành 1,`lca=1`, và câu trả lời là`0+0-2*0+1=1`. 

Trường hợp tự truy vấn không yêu cầu thao tác cấu trúc dữ liệu đặc biệt. Khi`u == v`, việc cân bằng độ sâu làm cho chúng bằng nhau ngay lập tức, do đó LCA là cùng một đỉnh. Công thức khoảng cách trở thành`depth[u] + depth[u] - 2*depth[u] + 1`, đó là một. Như vậy```
4
2 0 0
2 0 0
2 0 0
2 0 0
```tạo ra bốn dòng chứa`1`, mặc dù`last`thay đổi sau mỗi truy vấn. 

Trường hợp ranh giới gốc được kiểm soát bởi công thức modulo. TRONG```
4
1 0
1 1
2 0 0
2 0 1
```lần chèn đầu tiên sử dụng cha mẹ 1. Câu trả lời của nó là 2, do đó lần chèn thứ hai giải mã cha mẹ của nó là`(1+2)%2+1=2`. Do đó, máy tính mới có độ sâu 2 và câu trả lời 3. Sau truy vấn tiếp theo,`last`trở thành 1 và điểm cuối được mã hóa cuối cùng trở thành 2 và 3. Đường dẫn của chúng chứa chính xác hai máy tính. Thuật toán thu được những kết quả này mà không cần phải đi hết con đường. 

Sự sắp xếp của`curr`là một điều kiện biên tinh tế khác. Giả sử có hai máy tính trước khi chèn. Phần gốc phải được giải mã bằng modulo 2, vì hiện tại chỉ tồn tại id 1 và 2. Tăng dần`curr`đầu tiên sẽ cho phép công thức chọn máy tính chưa được tạo một cách không chính xác 3. Việc triển khai sẽ tính toán`parent`đầu tiên, xây dựng`new_node`, và chỉ sau đó mới tăng`curr`. 

Cuối cùng, đầu vào được mã hóa phải được xử lý trực tuyến. Một truy vấn có thể thay đổi`last`và giá trị đó thay đổi ý nghĩa của mọi số được mã hóa tiếp theo. Ví dụ: trong Mẫu 1, lần chèn đầu tiên tạo ra`last=2`; giá trị gốc thô của lần chèn thứ hai là`2`, nhưng cha mẹ thực sự của nó là`(2+2)%2+1=1`. Bất kỳ triển khai nào giải mã tất cả các hoạt động với`last=0`, hoặc với câu trả lời cuối cùng thay vì câu trả lời trước đó, sẽ xây dựng một cây khác và do đó cho kết quả khác.
