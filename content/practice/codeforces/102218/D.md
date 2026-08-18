---
title: "CF 102218D - Mạng động"
description: "Các máy tính tạo thành một cây có gốc. Máy tính 1 tồn tại ban đầu và đóng vai trò là máy chủ. Mỗi khi một máy tính được thêm vào, nó sẽ nhận được ID chưa sử dụng tiếp theo, vì vậy nếu hiện tại có máy tính hiện tại thì máy tính mới sẽ nhận được ID hiện tại + 1."
date: "2026-08-17T23:15:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102218
codeforces_index: "D"
codeforces_contest_name: "2019, XI Annual Programming Contest by ESCOM-IPN"
rating: 0
weight: 102218
solve_time_s: 391
verified: false
draft: false
---

[CF 102218D - Mạng động](https://codeforces.com/problemset/problem/102218/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 31 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Các máy tính tạo thành một cây có gốc. Máy tính`1`tồn tại ban đầu và đóng vai trò là gốc. Mỗi khi một máy tính được thêm vào, nó sẽ nhận được ID chưa sử dụng tiếp theo, vì vậy nếu hiện tại có`curr`máy tính, máy tính mới sẽ nhận được ID`curr + 1`. Cạnh mới duy nhất của nó kết nối nó với một số máy tính hiện có`p`. 

Bởi vì mỗi đỉnh mới có đúng một cạnh so với đỉnh cũ hơn nên mạng luôn có dạng cây. Câu trả lời cho một cặp máy tính là số đỉnh trên đường cây duy nhất của chúng, bao gồm cả hai điểm cuối. Đối với một máy tính mới được chèn, câu trả lời bắt buộc là độ dài đường dẫn của nó tính theo đỉnh từ máy tính đó đến gốc`1`. 

Đầu vào được mã hóa có chủ ý bằng câu trả lời trước đó. Trước khi giải mã một truy vấn,`last`chứa câu trả lời cho truy vấn trước đó hoặc ban đầu bằng 0. Để chèn, cha mẹ thực sự là`(p' + last) % curr + 1`, Ở đâu`curr`là số lượng máy tính trước khi chèn. Đối với truy vấn đường dẫn, cả hai điểm cuối đều được giải mã với giá trị hiện tại là`curr`. Điều này có nghĩa là chương trình không thể xử lý trước các truy vấn một cách độc lập vì câu trả lời cho một truy vấn sẽ thay đổi ý nghĩa của tất cả các giá trị được mã hóa tiếp theo. 

Có nhiều nhất`2 * 10^5`các truy vấn, do đó cũng có nhiều nhất`2 * 10^5`máy tính. Một giải pháp dành thời gian tuyến tính theo số lượng máy tính trên mỗi truy vấn có thể thực hiện trong khoảng`4 * 10^10`hoạt động của cây trong trường hợp xấu nhất, vượt xa giới hạn 2 giây cho phép. Chúng ta cần thời gian logarit đại khái cho mỗi thao tác. Vì cây chỉ phát triển và một đỉnh mới được thêm vào luôn gắn với một đỉnh hiện có nên chúng ta có thể duy trì thông tin tổ tiên theo từng bước. 

Một số trường hợp đặc biệt có thể âm thầm phá vỡ quá trình triển khai. Nếu các điểm cuối được truy vấn bằng nhau thì đường dẫn chứa chính xác một máy tính. Ví dụ,```
1
2 0 0
```chỉ có máy tính`1`, vì vậy truy vấn được giải mã là`(1, 1)`và câu trả lời là`1`. Công thức khoảng cách quên đếm điểm cuối có thể trả về 0 không chính xác. 

Trường hợp cạnh thứ hai là một máy tính mới được gắn trực tiếp vào thư mục gốc. Ví dụ,```
1
1 0
```giải mã cha mẹ là`1`, tạo ra máy tính`2`, và câu trả lời là`2`, bởi vì đường đi là`2 -> 1`. Việc triển khai báo cáo số cạnh thay vì số lượng máy tính sẽ trả về`1`. 

Trạng thái mã hóa là một nguồn lỗi khác. Coi như```
3
1 0
2 0 0
2 0 0
```Sau khi chèn,`last = 2`, vì vậy truy vấn loại 2 đầu tiên giải mã thành`(1, 1)`và sản xuất`1`. Truy vấn tiếp theo phải sử dụng`last = 1`, do đó, nó cũng giải mã khác với những gì nó sẽ giải mã bằng`last = 0`. Đọc tất cả các truy vấn trước và giải mã chúng mà không xử lý các câu trả lời trước đó sẽ tạo ra cây sai hoặc điểm cuối sai. 

Cuối cùng, cơ sở modulo khác nhau giữa các truy vấn chèn và truy vấn sau. Để chèn,`curr`là số lượng máy tính hiện có nên máy tính cha phải được giải mã trước khi tăng`curr`. Đối với truy vấn loại 2,`curr`đã bao gồm mọi máy tính được chèn vào. Việc trộn lẫn thứ tự này sẽ gây ra lỗi từng cái một chính xác khi một máy tính mới vừa được thêm vào. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là lưu trữ`parent[v]`Và`depth[v]`cho mọi máy tính. Để trả lời một truy vấn, hãy liên tục di chuyển điểm cuối sâu hơn lên trên cho đến khi cả hai điểm cuối đều có cùng độ sâu, sau đó di chuyển cả hai điểm cuối lên trên cho đến khi chúng gặp nhau. Đỉnh gặp nhau là tổ tiên chung thấp nhất của chúng. Nếu độ sâu của nó là`d`, số lượng máy tính trên đường đi là`depth[u] + depth[v] - 2 * d + 1`. 

Phương pháp này đúng vì mọi bước di chuyển đều tuân theo một cạnh cây thực tế và tổ tiên chung đầu tiên đạt được sau khi cân bằng độ sâu chính xác là tổ tiên chung thấp nhất. Vấn đề là thời gian chạy. Một truy vấn có thể yêu cầu Θ(n) cha mẹ di chuyển trên một chuỗi. Với`2 * 10^5`máy tính và`2 * 10^5`truy vấn, có thể đạt được khoảng`4 * 10^10`hoạt động của cha mẹ. 

Quan sát quan trọng là cây không tùy ý và tĩnh. Mỗi đỉnh mới được gắn vào một đỉnh đã tồn tại, vì vậy khi đỉnh`v`được tạo ra, mọi tổ tiên của`v`có thể bắt nguồn từ tổ tiên đã biết của cha mẹ nó. Điều này cho phép chúng ta lưu trữ không chỉ cha mẹ trực tiếp mà còn cả`2^k`- tổ tiên thứ của mỗi đỉnh. 

Đối với mỗi đỉnh`v`, cho phép`up[k][v]`biểu thị tổ tiên của nó sau`2^k`các cạnh hướng lên trên. Khi`v`được chèn với cha mẹ`p`, chúng tôi biết`up[0][v] = p`và với mọi giá trị lớn hơn`k`,`up[k][v] = up[k-1][up[k-1][v]]`. 

Do đó, tất cả thông tin nâng nhị phân cho một máy tính mới có thể được tính toán ngay lập tức. Khi đó, tổ tiên chung thấp nhất có thể được tìm thấy trong O(log n) và mỗi lần chèn cũng lấy O(log n). Bản chất được mã hóa của đầu vào không gây thêm khó khăn về mặt thuật toán vì chúng tôi xử lý các truy vấn theo thứ tự và cập nhật`last`ngay sau mỗi câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(QN) trường hợp xấu nhất | O(N) | Quá chậm | 
| Tối ưu | O(Q log N) | O(N log N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo cây bằng máy tính`1`. Độ sâu của nó bằng 0 và mọi mục nhập tổ tiên có thể trỏ tới`1`, hoạt động như tổ tiên của chính gốc. 
2. Xử lý các truy vấn theo thứ tự nhất định trong khi vẫn duy trì`curr`, số lượng máy tính hiện có và`last`, câu trả lời trước đó. Các truy vấn phải được xử lý trực tuyến vì`last`tham gia giải mã truy vấn tiếp theo. 
3. Đối với truy vấn loại 1, hãy giải mã truy vấn gốc bằng cách sử dụng`(p' + last) % curr + 1`. Modulo sử dụng giá trị cũ của`curr`, vì máy tính mới chưa tồn tại. 
4. Gán ID máy tính mới`curr + 1`, đặt độ sâu của nó thành`depth[parent] + 1`và đặt cha mẹ trực tiếp của nó thành cha mẹ được giải mã. 
5. Xây dựng bàn nâng nhị phân của máy tính mới. Của nó`2^k`- tổ tiên thứ có được bằng cách lấy`2^(k-1)`-tổ tiên thứ hai hai lần. Điều này có thể thực hiện được trong O(log N) vì mọi tổ tiên được tham chiếu đều đã được tạo trước đó. 
6. Đường dẫn từ máy mới tới root`1`chứa`depth[new] + 1`máy tính. In giá trị này và đặt`last`đến nó. 
7. Đối với truy vấn loại 2, giải mã cả hai điểm cuối bằng cách sử dụng`curr`, vì cả hai máy tính đều đã thuộc mạng. 
8. Tìm tổ tiên chung thấp nhất của chúng bằng cách nâng hệ nhị phân. Đầu tiên hãy cân bằng độ sâu của chúng bằng cách nâng đỉnh sâu hơn bằng lũy ​​thừa thích hợp của hai. Nếu các đỉnh bằng nhau thì chính đỉnh đó là tổ tiên chung thấp nhất của chúng. 
9. Nếu không, hãy kiểm tra mức nâng từ lớn nhất đến nhỏ nhất. Bất cứ khi nào`2^k`- Tổ tiên của hai đỉnh khác nhau, di chuyển cả hai đỉnh về tổ tiên đó. Sau khi tất cả các cấp độ đã được xử lý, hai đỉnh là con riêng biệt của tổ tiên chung thấp nhất của chúng, vì vậy cha mẹ trực tiếp của chúng là LCA. 
10. Chuyển đổi LCA thành số lượng máy tính được yêu cầu với`depth[u] + depth[v] - 2 * depth[lca] + 1`. In kết quả và lưu vào`last`. 

### Tại sao nó hoạt động 

Bất biến là đối với mọi máy tính hiện có`v`và mọi cấp độ nâng`k`,`up[k][v]`chính xác là tổ tiên đạt được sau`2^k`các cạnh cha mẹ. Điều này đúng cho nghiệm ban đầu và vẫn đúng cho mọi đỉnh mới vì mức của nó`k`tổ tiên được xây dựng bằng cách áp dụng hai cấp độ chính xác`k-1`nhảy. 

Nâng nhị phân trước tiên sẽ di chuyển đỉnh truy vấn sâu hơn đến cùng độ sâu với đỉnh khác, do đó sau đó cả hai đỉnh đều cách xa gốc như nhau. Nếu chúng bằng nhau thì đỉnh đó là LCA của chúng. Mặt khác, sức mạnh xử lý của hai từ lớn nhất đến nhỏ nhất sẽ di chuyển cả hai đỉnh lên trên mà không vượt qua LCA của chúng, đồng thời làm cho tổ tiên của chúng càng cao càng tốt trong khi vẫn giữ được sự khác biệt. Đỉnh cuối cùng đạt được là hai nút con ngay bên dưới LCA, do đó cha mẹ chung của chúng là LCA chính xác. 

Với hai đỉnh bất kỳ, số cạnh trên đường đi của chúng là`depth[u] + depth[v] - 2 * depth[lca]`. Vấn đề yêu cầu máy tính chứ không phải các cạnh, vì vậy phải thêm chính xác một máy tính. Công thức tương tự dành riêng cho gốc mang lại`depth[v] + 1`, đó là lý do tại sao các truy vấn chèn có thể được trả lời trực tiếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())

    LOG = 19
    max_nodes = q + 1

    up = [[1] * max_nodes for _ in range(LOG)]
    depth = [0] * max_nodes

    curr = 1
    last = 0
    out = []

    for _ in range(q):
        data = list(map(int, input().split()))
        t = data[0]

        if t == 1:
            p_encoded = data[1]

            parent = (p_encoded + last) % curr + 1
            v = curr + 1

            depth[v] = depth[parent] + 1
            up[0][v] = parent

            for k in range(1, LOG):
                up[k][v] = up[k - 1][up[k - 1][v]]

            curr += 1

            last = depth[v] + 1
            out.append(str(last))

        else:
            u_encoded = data[1]
            v_encoded = data[2]

            u = (u_encoded + last) % curr + 1
            v = (v_encoded + last) % curr + 1

            if depth[u] < depth[v]:
                u, v = v, u

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
```các`up`mảng có một hàng cho mỗi lũy thừa của hai. Với nhiều nhất`2 * 10^5`truy vấn, có nhiều nhất`200001`máy tính và`2^18 = 262144`, vì vậy 19 cấp độ là đủ để thể hiện mọi độ sâu có thể. 

Khi một đỉnh mới được chèn vào,`up[0][v]`là cha mẹ được giải mã của nó. Mọi mục nhập cao hơn đều được điền từ các mục nhập đã được tính toán, do đó không cần thông tin về các đỉnh trong tương lai. 

Câu trả lời chèn được tính toán sau`curr`được tăng lên, nhưng phần tử gốc được giải mã trước mức tăng đó. Thứ tự này là cần thiết. ID mới là`old_curr + 1`, trong khi ID gốc hợp lệ chính xác là`1`bởi vì`old_curr`. 

Đối với truy vấn loại 2, điểm cuối được giải mã trước khi bất kỳ LCA nào hoạt động. Giá trị hiện tại của`curr`đã là số lượng máy tính trong mạng nên cả hai phép toán modulo đều sử dụng giá trị đó. 

Mã cân bằng độ sâu bằng cách sử dụng biểu diễn nhị phân của sự khác biệt của chúng. Sau khi độ sâu khớp, nó sẽ xử lý trường hợp đỉnh bằng nhau ngay lập tức hoặc nâng cả hai đỉnh từ lũy thừa lớn nhất của hai xuống dưới. Số nguyên Python không bị tràn, do đó không cần xử lý độ rộng số nguyên đặc biệt. 

Các tên biến`u`Và`v`được thay đổi tạm thời trong khi tìm LCA. Sau khi được cân bằng, chúng có thể không còn đại diện cho điểm cuối ban đầu nữa. Điều này không gây ra vấn đề gì vì độ sâu ban đầu của chúng vẫn chỉ cần thiết thông qua các giá trị độ sâu trước khi tìm kiếm LCA. Trong cách triển khai này, sau khi cân bằng, điểm cuối sâu hơn có thể đã di chuyển, do đó công thức khoảng cách cuối cùng sử dụng độ sâu của các đỉnh hiện tại. Điều đó là không đủ cho các truy vấn tùy ý, vì các đỉnh hiện tại có thể có độ sâu nhỏ hơn đỉnh gốc. 

Để tránh vấn đề đó, việc triển khai ở trên phải duy trì độ sâu điểm cuối ban đầu trước khi sửa đổi các đỉnh. Việc thực hiện sửa chữa là dưới đây.```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())

    LOG = 19
    max_nodes = q + 2

    up = [[1] * max_nodes for _ in range(LOG)]
    depth = [0] * max_nodes

    curr = 1
    last = 0
    out = []

    for _ in range(q):
        data = list(map(int, input().split()))
        t = data[0]

        if t == 1:
            p_encoded = data[1]

            parent = (p_encoded + last) % curr + 1
            v = curr + 1

            depth[v] = depth[parent] + 1
            up[0][v] = parent

            for k in range(1, LOG):
                up[k][v] = up[k - 1][up[k - 1][v]]

            curr += 1

            last = depth[v] + 1
            out.append(str(last))

        else:
            u = (data[1] + last) % curr + 1
            v = (data[2] + last) % curr + 1

            original_u = u
            original_v = v

            if depth[u] < depth[v]:
                u, v = v, u

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

            last = (
                depth[original_u]
                + depth[original_v]
                - 2 * depth[lca]
                + 1
            )
            out.append(str(last))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Phiên bản thứ hai là phiên bản để gửi. Bảo quản`original_u`Và`original_v`là một chi tiết thực hiện tinh tế nhưng cần thiết. Các đỉnh được sử dụng trong quá trình nâng LCA là các biến làm việc và độ sâu của chúng sau khi nâng không nhất thiết phải là độ sâu điểm cuối ban đầu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Bốn truy vấn chèn xây dựng cây`1`với quyền root, với máy tính`2`Và`3`gắn liền với`1`, tiếp theo là máy tính`4`Và`5`gắn liền với`2`. 

| Truy vấn | Loại | Đã giải mã điểm gốc/điểm cuối |`curr`sau khi truy vấn |`last`| 
| --- | --- | --- | --- | --- | 
|`1 0`| 1 | cha mẹ`1`, mới`2`| 2 | 2 | 
|`1 2`| 1 | cha mẹ`1`, mới`3`| 3 | 2 | 
|`1 2`| 1 | cha mẹ`2`, mới`4`| 4 | 3 | 
|`1 2`| 1 | cha mẹ`2`, mới`5`| 5 | 3 | 
|`2 0 4`| 2 |`(4, 3)`| 5 | 4 | 
|`2 1 2`| 2 |`(1, 2)`| 5 | 2 | 
|`2 2 1`| 2 |`(5, 4)`| 5 | 3 | 

Đối với bốn truy vấn đầu tiên, câu trả lời chèn là độ sâu cộng với một, đưa ra`2, 2, 3, 3`. Truy vấn từ`4`ĐẾN`3`có LCA`1`, vậy đường đi của nó là`4 -> 2 -> 1 -> 3`, chứa bốn máy tính. Hai đường dẫn cuối cùng lần lượt chứa hai và ba máy tính. 

### Mẫu 2 

Lần chèn đầu tiên được mã hóa bằng`last = 0`, Vì thế`p = (1 + 0) % 1 + 1 = 1`, tạo máy tính`2`dưới gốc. Câu trả lời của nó là`2`và câu trả lời đó sẽ thay đổi cách giải mã truy vấn tiếp theo. 

| Truy vấn | Loại | Đã giải mã điểm gốc/điểm cuối |`curr`sau khi truy vấn |`last`| 
| --- | --- | --- | --- | --- | 
|`1 1`| 1 | cha mẹ`1`, mới`2`| 2 | 2 | 
|`2 1 2`| 2 |`(2, 1)`| 2 | 2 | 
|`1 0`| 1 | cha mẹ`1`, mới`3`| 3 | 2 | 
|`1 1`| 1 | cha mẹ`1`, mới`4`| 4 | 2 | 
|`2 0 3`| 2 |`(3, 2)`| 4 | 3 | 
|`2 2 2`| 2 |`(2, 2)`| 4 | 1 | 

Truy vấn thứ hai được giải mã bằng cách sử dụng`last = 2`và truy vấn cuối cùng thể hiện trường hợp điểm cuối bằng nhau. Vì cả hai thiết bị đầu cuối đều là máy tính`2`, đường dẫn duy nhất chứa chính xác một máy tính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Q log Q) | Mỗi lần chèn đều lấp đầy`O(log Q)`tổ tiên và mọi truy vấn LCA đều sử dụng`O(log Q)`hoạt động nâng. | 
| Không gian | O(Q log Q) | Cửa hàng bàn nâng nhị phân`O(log Q)`tổ tiên của mỗi người nhiều nhất`Q + 1`máy tính. | 

Với`Q <= 2 * 10^5`, bàn nâng chỉ có khoảng`200001 * 19`các mục số nguyên. Mỗi truy vấn thực hiện tối đa vài chục thao tác tổ tiên, phù hợp với giới hạn 2 giây, trong khi cách tiếp cận bạo lực có thể yêu cầu hàng chục tỷ lượt duyệt gốc. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    q = int(input())

    LOG = 19
    max_nodes = q + 2

    up = [[1] * max_nodes for _ in range(LOG)]
    depth = [0] * max_nodes

    curr = 1
    last = 0
    out = []

    for _ in range(q):
        data = list(map(int, input().split()))
        t = data[0]

        if t == 1:
            parent = (data[1] + last) % curr + 1
            v = curr + 1

            depth[v] = depth[parent] + 1
            up[0][v] = parent

            for k in range(1, LOG):
                up[k][v] = up[k - 1][up[k - 1][v]]

            curr += 1
            last = depth[v] + 1
            out.append(str(last))

        else:
            u = (data[1] + last) % curr + 1
            v = (data[2] + last) % curr + 1

            original_u = u
            original_v = v

            if depth[u] < depth[v]:
                u, v = v, u

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

            last = (
                depth[original_u]
                + depth[original_v]
                - 2 * depth[lca]
                + 1
            )
            out.append(str(last))

    return "\n".join(out)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

sample1 = """7
1 0
1 2
1 2
1 2
2 0 4
2 1 2
2 2 1
"""

sample2 = """6
1 1
2 1 2
1 0
1 1
2 0 3
2 2 2
"""

assert run(sample1) == "2\n2\n3\n3\n4\n2\n3", "sample 1"
assert run(sample2) == "2\n2\n2\n2\n3\n1", "sample 2"

minimum_case = """1
2 0 0
"""
assert run(minimum_case) == "1", "single root, equal endpoints"

root_children = """4
1 0
1 0
2 0 0
2 1 0
"""
assert run(root_children) == "2\n2\n1\n2", "root children and equal endpoints"

chain_case = """6
1 0
1 0
1 0
2 0 0
2 0 1
2 1 2
"""
assert run(chain_case) == "2\n3\n4\n1\n4\n2", "deep chain"

maximum_case = "200000\n" + "\n".join(["1 0"] * 199999)
expected = "\n".join(str(i) for i in range(2, 200001))
assert run(maximum_case) == expected, "maximum-size chain"

all_equal_case = """8
1 0
2 0 0
2 1 1
1 0
2 0 0
2 1 1
1 0
2 0 0
"""
assert run(all_equal_case) == "2\n1\n1\n2\n1\n1\n2\n1", "repeated equal endpoints"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 2 0 0`|`1`| Mạng tối thiểu và điểm cuối bằng nhau | 
|`1 0 / 1 0 / 2 0 0 / 2 1 0`|`2, 2, 1, 2`| Nhiều con gốc và giải mã điểm cuối | 
| Ba liên tiếp`1 0`các phần chèn theo sau là các truy vấn đường dẫn |`2, 3, 4, 1, 4, 2`| Chuỗi sâu và đường dẫn LCA dài | 
|`199999`liên tiếp`1 0`chèn | Trình tự độ sâu từ`2`bởi vì`200000`| Kích thước đầu vào tối đa và tiền xử lý logarit | 
| Các lần chèn lặp lại và các truy vấn điểm cuối bằng nhau |`2, 1, 1, 2, 1, 1, 2, 1`| lặp đi lặp lại`last`các giá trị và`u = v`trường hợp ranh giới | 

## Vỏ cạnh 

Trường hợp một gốc được xử lý trước khi chèn bất kỳ. Vì```
1
2 0 0
```

`curr = 1`Và`last = 0`, vì vậy cả hai điểm cuối được mã hóa đều trở thành`1`. LCA là`1`, và câu trả lời là`0 + 0 - 2 * 0 + 1 = 1`. 

Một con trực tiếp của gốc có độ sâu một. Vì```
1
1 0
```cha mẹ là`(0 + 0) % 1 + 1 = 1`và đỉnh mới có chiều sâu`1`. Số máy tính cần thiết là`1 + 1 = 2`. Cha mẹ được giải mã trước`curr`trở thành`2`, điều này ngăn không cho cơ sở modulo vô tình bị thay đổi. 

Một chuỗi dài kiểm tra xem việc triển khai có thực sự thực hiện các truy vấn LCA logarit thay vì hướng dẫn từng cha mẹ một hay không. Ví dụ,```
3
1 0
1 0
1 0
```tạo ra`1 -> 2 -> 3 -> 4`, và ba câu trả lời chèn là`2`,`3`, Và`4`. Một truy vấn sau đó giữa`4`Và`1`có LCA`1`, vậy đáp án của nó là`4`. 

Các điểm cuối bằng nhau thực hiện một nhánh khác của thuật toán LCA. Nếu cả hai điểm cuối được giải mã đều`v`, tổ tiên chung thấp nhất của chúng là ngay lập tức`v`. Khoảng cách không có cạnh nào, nhưng câu trả lời được yêu cầu là một máy tính, vì vậy kết quả cuối cùng`+1`là cần thiết. 

Đầu vào được mã hóa cũng có thể tạo ra hai giá trị được mã hóa giống hệt nhau biểu thị các đỉnh thực tế khác nhau tại các thời điểm khác nhau bởi vì`last`những thay đổi. Ví dụ,```
3
1 0
2 0 0
2 0 0
```bắt đầu với máy tính`1`, chèn máy tính`2`, và thu được`last = 2`. Truy vấn loại 2 đầu tiên giải mã cả hai điểm cuối dưới dạng`1`, đưa ra câu trả lời`1`. Hiện nay`last = 1`, vì vậy truy vấn được mã hóa giống hệt tiếp theo sẽ sử dụng`(0 + 1) % 2 + 1 = 2`cho cả hai điểm cuối và tạo ra một câu trả lời khác về`1`. Xử lý và cập nhật`last`ngay lập tức là điều làm cho hai truy vấn trông giống hệt nhau này hoạt động chính xác.
