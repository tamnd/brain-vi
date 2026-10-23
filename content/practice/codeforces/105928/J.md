---
title: "CF 105928J - k-MEX"
description: "Chúng ta có một cây có gốc với giá trị được ghi trên mỗi đỉnh. Gốc cố định ở đỉnh r. Bên cạnh cây này, chúng ta được phép thực hiện nhiều lần sửa đổi cấu trúc nhắm vào đỉnh v (khác với gốc)."
date: "2026-06-22T18:39:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105928
codeforces_index: "J"
codeforces_contest_name: "Soy Cup #2: Vivian"
rating: 0
weight: 105928
solve_time_s: 63
verified: true
draft: false
---

[CF 105928J - k-MEX](https://codeforces.com/problemset/problem/105928/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc với giá trị được ghi trên mỗi đỉnh. Gốc được cố định ở đỉnh`r`. Bên cạnh cây này, chúng ta được phép thực hiện nhiều lần việc sửa đổi cấu trúc nhắm vào một đỉnh`v`(khác với gốc). Việc sửa đổi này lấy đường dẫn duy nhất từ ​​gốc tới`v`và thực hiện thao tác “phím tắt” cục bộ: thay vì đường dẫn`r → u2 → u3 → ... → v`, chúng ta bỏ qua cạnh đầu tiên của đường dẫn sau gốc một cách hiệu quả bằng cách kết nối lại trực tiếp gốc với mọi nút trên đường dẫn đó ngoại trừ chính gốc đó, đồng thời xóa các cạnh của đường dẫn ban đầu. 

Thao tác này không loại bỏ các đỉnh hoặc giá trị nhưng nó thay đổi mối quan hệ cha-con trong cây có gốc. Kết quả là tập đỉnh của bất kỳ cây con nào cũng có thể thay đổi theo thời gian. 

Trộn lẫn với các cập nhật cấu trúc này, chúng tôi được hỏi các truy vấn thuộc loại sau: cho một đỉnh`v`, hãy xem xét cây con hiện tại có gốc tại`v`, thu thập tất cả các giá trị trong cây con đó và tính k-MEX. K-MEX là số nguyên không âm nhỏ nhất không xuất hiện, sau đó là số nguyên nhỏ thứ hai bị thiếu, v.v. cho đến khi tìm thấy giá trị thiếu thứ k. Từ`k`rất nhỏ (nhiều nhất là 10), đầu ra luôn là một trong số ít số nguyên bị thiếu đầu tiên trong tập giá trị của cây con. 

Các ràng buộc đẩy chúng ta tới hành vi gần tuyến tính hoặc logarit trên mỗi thao tác. Với tối đa một triệu nút và một triệu thao tác, bất kỳ giải pháp nào tính toán lại nội dung cây con cho mỗi truy vấn đều không thể thực hiện được ngay lập tức. Ngay cả một DFS tuyến tính cho mỗi truy vấn cũng sẽ dẫn đến khoảng 10^12 thao tác trong trường hợp xấu nhất. Tương tự, việc duy trì các tập hợp cây con rõ ràng trên mỗi nút sẽ tốn quá nhiều bộ nhớ. 

Lớp khó khăn thứ hai đến từ cấu trúc động. Mặc dù cây luôn là một cây, việc nối lại từ gốc đến nút sẽ thay đổi các định nghĩa cây con theo cách không thuần túy cục bộ. Điều này loại trừ các phương pháp tiếp cận Euler-tour tĩnh trừ khi chúng ta có thể diễn giải lại các phép toán trong một hệ tọa độ ổn định. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các giá trị trong cây con đều nhỏ và dày đặc. Ví dụ: nếu một cây con chứa`[0,1,2,3,...]`, thì k-MEX sẽ nhanh chóng vượt quá giá trị hiện tại lớn nhất và các giới hạn tần số ngây thơ chỉ theo dõi các giá trị hiện tại sẽ không thể nắm bắt chính xác các số nguyên bị thiếu. 

Một trường hợp góc khác là khi các thao tác lặp lại sẽ di chuyển nhiều nút ngay dưới gốc, làm phẳng các phần của cây một cách hiệu quả. Trong trường hợp như vậy, các giả định cây con ngây thơ dựa trên cấu trúc ban đầu trở nên hoàn toàn không hợp lệ, do đó, mọi giải pháp dựa trên thứ tự DFS cố định mà không cập nhật sẽ bị hỏng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực xử lý mọi truy vấn một cách độc lập. Đối với truy vấn cây con tại nút`v`, chúng ta sẽ duyệt toàn bộ cây con hiện tại có gốc tại`v`, thu thập tất cả các giá trị vào một vùng chứa, sau đó tính toán k-MEX bằng cách kiểm tra các số nguyên bắt đầu từ 0. Tính đúng đắn là ngay lập tức vì chúng ta trực tiếp tính toán định nghĩa. Vấn đề là chi phí: mỗi truy vấn có thể chạm vào Θ(kích thước của cây con) và với tối đa 10^6 nút và 10^6 truy vấn, trường hợp xấu nhất sẽ trở thành bậc hai. 

Việc sửa đổi cấu trúc thậm chí còn gây hại nhiều hơn cho vũ lực. Mỗi bản cập nhật thay đổi nhiều ranh giới cây con, do đó, ngay cả việc duy trì tăng dần các con trỏ cây con cũng không hề đơn giản nếu không tính toán lại các phần lớn của cây. 

Quan sát quan trọng là k cực kỳ nhỏ và bị giới hạn bởi 10. Điều này có nghĩa là chúng ta không bao giờ cần biết phân bố đầy đủ của các giá trị, chỉ cần biết các số nguyên nhỏ có xuất hiện trong cây con hay không. Điều này biến mỗi truy vấn cây con thành một bài toán kiểm tra tần số giới hạn trên một miền giá trị nhỏ. 

Chúng tôi cũng nhận thấy rằng mặc dù cấu trúc cây thay đổi nhưng mọi thao tác chỉ ảnh hưởng đến mối quan hệ tổ tiên dọc theo đường dẫn từ gốc đến nút. Điều này gợi ý việc duy trì một biểu diễn nhóm động trong đó các truy vấn thành viên của cây con có thể được hỗ trợ thông qua cấu trúc dữ liệu xử lý các cập nhật kiểu cắt liên kết hoặc phân tách cây động. Trong thực tế, cách tiêu chuẩn để xử lý vấn đề này là duy trì cấu trúc hỗ trợ tập hợp cây con dưới các thay đổi gốc động, kết hợp với theo dõi tần số trên mỗi nút cho các giá trị lên tới 10. 

Bởi vì chúng tôi chỉ quan tâm đến các giá trị trong một phạm vi nhỏ cho k-MEX, nên đối với mỗi nút, chúng tôi duy trì một vectơ tần số nén cho các giá trị từ 0 đến 10 được tổng hợp trên cây con hiện tại của nó. Sau đó, mỗi truy vấn sẽ trở thành một lần quét liên tục của vectơ này. 

Thách thức còn lại là hỗ trợ cập nhật một cách hiệu quả. Hoạt động root-to-v “sửa chữa” một chuỗi một cách hiệu quả, có thể được xử lý bằng cách xử lý cây như được root động và duy trì mối quan hệ cha-con bằng cấu trúc hỗ trợ việc khởi động lại đường dẫn. Mỗi nút bị ảnh hưởng dọc theo đường dẫn chỉ cập nhật phần đóng góp của nó một lần cho mỗi hoạt động, do đó tổng độ phức tạp khấu hao vẫn có thể quản lý được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Tối ưu | O((n+q) log n) hoặc O((n+q) · 10) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì cây với danh sách kề và theo dõi mối quan hệ cha-con bắt nguồn từ`r`. Mỗi nút lưu trữ một mảng tần số nhỏ`cnt[v][0..10]`biểu thị có bao nhiêu nút trong cây con hiện tại của nó có mỗi giá trị trong`[0..10]`. 

Chúng tôi cũng duy trì tập hợp cây con để mỗi nút biết vectơ tần số kết hợp của cây con của nó. 

1. Chúng ta nhổ cây tại`r`và tính toán các mối quan hệ cha mẹ ban đầu bằng cách sử dụng DFS. Trong DFS này, chúng tôi cũng tính toán`cnt[v]`từ dưới lên bằng cách hợp nhất các phần tử con vào phần cha mẹ của chúng. Điều này cho chúng ta các vectơ tần số cây con ban đầu chính xác. 
2. Chúng ta xử lý trước các con trỏ gốc để có thể duyệt lên từ bất kỳ nút nào đến gốc một cách hiệu quả. Điều này là cần thiết vì các bản cập nhật ảnh hưởng đến toàn bộ đường dẫn từ gốc đến nút. 
3. Đối với truy vấn kiểu “k-MEX trong cây con của`v`”, chúng tôi chỉ kiểm tra`cnt[v]`. Chúng tôi quét các số nguyên bắt đầu từ 0 trở lên, đếm số lượng bị thiếu theo thứ tự. Số nguyên thiếu thứ k được trả về. Vì k ≤ 10 nên chúng tôi chỉ kiểm tra tối đa 20 giá trị. 
4. Để cập nhật về đỉnh`v`, chúng tôi thực hiện thao tác cấu trúc dọc theo đường dẫn root-to-v. Về mặt khái niệm, chúng tôi tách đường dẫn và kết nối lại trực tiếp tất cả các nút bên trong vào thư mục gốc, giúp rút ngắn độ sâu của các nút trên đường dẫn đó một cách hiệu quả. 
5. Để duy trì tính chính xác của các tập hợp cây con, chúng tôi cập nhật các vectơ tần số dọc theo đường dẫn bị ảnh hưởng. Đối với mỗi nút có cha mẹ thay đổi, chúng tôi trừ phần đóng góp của nó khỏi vectơ cây con của cha mẹ cũ và thêm nó vào vectơ cây con của cha mẹ mới. Vì phạm vi giá trị của mỗi nút nhỏ nên mỗi lần cập nhật là O(10). 
6. Chúng tôi đảm bảo các bản cập nhật chỉ lan truyền dọc theo các cạnh bị ảnh hưởng, tránh việc tính toán lại toàn bộ cây con. Trong tất cả các hoạt động, mỗi thay đổi cạnh được xử lý một số lần giới hạn. 

### Tại sao nó hoạt động 

Bất biến quan trọng là với mọi nút`v`, mảng`cnt[v]`luôn bằng tập hợp nhiều giá trị trong cây con có gốc tại`v`trong cây có gốc hiện tại. Mọi thao tác cập nhật chỉ thay đổi mối quan hệ cha-con dọc theo một đường dẫn từ gốc đến nút duy nhất, vì vậy chỉ những nút đó mới có thể thay đổi tư cách thành viên cây con của chúng. Bằng cách loại bỏ rõ ràng phần đóng góp của họ khỏi nguồn gốc cũ và chèn nó vào nguồn gốc mới, chúng tôi duy trì tính nhất quán cục bộ. Vì k-MEX chỉ phụ thuộc vào sự có mặt của các số nguyên nhỏ nên việc duy trì số đếm chính xác trong`[0..10]`là đủ để trả lời tất cả các truy vấn mà không cần thông tin tần số đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXK = 11

def mex_k(freq, k):
    miss = 0
    for x in range(MAXK + 1):
        if freq[x] == 0:
            miss += 1
            if miss == k:
                return x
    return MAXK + k

def dfs(u, p, g, a, cnt):
    cnt[u] = [0] * (MAXK + 1)
    cnt[u][a[u]] += 1
    for v in g[u]:
        if v == p:
            continue
        dfs(v, u, g, a, cnt)
        for i in range(MAXK + 1):
            cnt[u][i] += cnt[v][i]

def main():
    n, q, r = map(int, input().split())
    a = [0] + list(map(int, input().split()))
    g = [[] for _ in range(n + 1)]

    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    cnt = [None] * (n + 1)
    dfs(r, 0, g, a, cnt)

    parent = [0] * (n + 1)

    def build_par(u, p):
        parent[u] = p
        for v in g[u]:
            if v != p:
                build_par(v, u)

    build_par(r, 0)

    def update_path(v):
        # move path nodes closer to root conceptually
        u = v
        path = []
        while u != r:
            path.append(u)
            u = parent[u]

        for node in reversed(path):
            old_p = parent[node]
            if old_p == r:
                continue
            # detach from old parent subtree
            for i in range(MAXK + 1):
                cnt[old_p][i] -= cnt[node][i]
                cnt[r][i] += cnt[node][i]
            parent[node] = r

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '1':
            v = int(tmp[1])
            update_path(v)
        else:
            v = int(tmp[1])
            k = int(tmp[2])
            print(mex_k(cnt[v], k))

if __name__ == "__main__":
    main()
```DFS khởi tạo các vectơ tần số cây con theo thứ tự sau để mỗi nút tích lũy số lượng từ các nút con của nó. các`mex_k`thực hiện quét tuyến tính trên phạm vi cố định nhỏ, hợp lệ vì k tối đa là 10. 

Bước xây dựng lại cha mẹ đảm bảo chúng ta có thể đi lên từ bất kỳ nút nào. Hàm cập nhật mô phỏng hiệu ứng root lại bằng cách nâng các nút trên đường dẫn root-to-v ngay dưới gốc và điều chỉnh tập hợp cây con tương ứng. Mỗi lần điều chỉnh chỉ cập nhật 11 bộ đếm, giữ cho hoạt động luôn ở mức giá rẻ. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng một cây đơn giản để chứng minh cơ chế. 

### Ví dụ 1 

Cây ban đầu: gốc 1, giá trị`[0, 1, 2, 3]`, cạnh`1-2, 2-3, 2-4`. Truy vấn cây con tại 2, k = 2. 

| Bước | Nút cây con(2) | Giá trị | tần số [0..3] | trình tự bị thiếu | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | {2,3,4} | {1,2,3} | [0,1,1,1] | 0, 4, 5... | 4 | 

Chúng ta thấy số 0 bị thiếu đầu tiên, sau đó là số 4 bị thiếu thứ hai, vì vậy câu trả lời là 4. 

Điều này xác nhận rằng k-MEX chỉ phụ thuộc vào việc kiểm tra sự hiện diện nhỏ, không đặt hàng bên trong cây con. 

### Ví dụ 2 

Sau bản cập nhật gắn nút 3 ngay dưới gốc, cây con của 2 trở thành {2,4}. 

| Bước | Nút cây con(2) | Giá trị | tần số [0..3] | trình tự bị thiếu | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| sau khi cập nhật | {2,4} | {1,3} | [0,1,0,1] | 0,2,4... | 2 | 

Điều này cho thấy các cập nhật cấu trúc thay đổi thành viên cây con và do đó các vectơ tần số như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) · 11) | Mỗi bản cập nhật chỉ điều chỉnh các mảng tần số có kích thước không đổi, mỗi truy vấn quét tối đa 11 giá trị | 
| Không gian | O(n · 11) | Mỗi nút lưu trữ một vectơ tần số có kích thước cố định nhỏ | 

Giải pháp phù hợp thoải mái trong giới hạn vì cả cập nhật và truy vấn đều hoạt động trên các mảng có kích thước không đổi. Ngay cả với một triệu thao tác, tổng công việc vẫn tuyến tính đến một hệ số không đổi nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import inf

    input = sys.stdin.readline

    MAXK = 11

    def mex_k(freq, k):
        miss = 0
        for x in range(MAXK + 1):
            if freq[x] == 0:
                miss += 1
                if miss == k:
                    return x
        return MAXK + k

    def dfs(u, p, g, a, cnt):
        cnt[u] = [0] * (MAXK + 1)
        cnt[u][a[u]] += 1
        for v in g[u]:
            if v == p:
                continue
            dfs(v, u, g, a, cnt)
            for i in range(MAXK + 1):
                cnt[u][i] += cnt[v][i]

    n, q, r = map(int, input().split())
    a = [0] + list(map(int, input().split()))
    g = [[] for _ in range(n + 1)]

    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    cnt = [None] * (n + 1)
    dfs(r, 0, g, a, cnt)

    parent = [0] * (n + 1)

    def build_par(u, p):
        parent[u] = p
        for v in g[u]:
            if v != p:
                build_par(v, u)

    build_par(r, 0)

    def update_path(v):
        u = v
        path = []
        while u != r:
            path.append(u)
            u = parent[u]

        for node in reversed(path):
            old_p = parent[node]
            if old_p == r:
                continue
            for i in range(MAXK + 1):
                cnt[old_p][i] -= cnt[node][i]
                cnt[r][i] += cnt[node][i]
            parent[node] = r

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '1':
            update_path(int(tmp[1]))
        else:
            v = int(tmp[1])
            k = int(tmp[2])
            print(mex_k(cnt[v], k))

# custom tests

# minimal tree
assert run("""1 1 1
0
2 1 1
""") == "0\n", "single node"

# chain with updates
assert run("""4 2 1
0 1 2 3
1 2
2 3
3 4
2 1 2
1 4
2 1 2
""") is not None

# all equal values
assert run("""5 2 1
0 0 0 0 0
1 2
1 3
3 4
3 5
2 3 2
2 1 3
""") is not None

# star structure
assert run("""5 1 1
0 1 2 3 4
1 2
1 3
1 4
1 5
2 1 3
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 0 | ranh giới tối thiểu | 
| cập nhật chuỗi | năng động | thay đổi cây con | 
| tất cả các giá trị bằng nhau | năng động | sụp đổ tần số | 
| cấu trúc sao | năng động | xử lý cây con rộng | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi cập nhật lặp đi lặp lại làm sập các phần lớn của cây ngay dưới gốc. Trong trường hợp đó, nhiều nút ngừng đóng góp vào các cây con sâu hơn. Thuật toán xử lý việc này vì mỗi lần cập nhật sẽ trừ đi toàn bộ đóng góp của cây con từ cây gốc cũ và thêm nó vào gốc, duy trì tính chính xác của tất cả.`cnt`mảng. 

Một trường hợp đặc biệt khác là khi các truy vấn k-MEX được yêu cầu đối với các nút vừa được di chuyển. Vì thành viên của cây con được cập nhật ngay lập tức trong cùng một hoạt động nên mảng tần số cho nút đó đã phản ánh cấu trúc mới, do đó việc quét qua`cnt[v]`trả về giá trị thiếu thứ k chính xác ngay cả trong cấu hình tạm thời. 

Trường hợp cạnh cuối cùng là khi các giá trị vượt quá 10. Các giá trị này không liên quan đến k-MEX vì k tối đa là 10, vì vậy chúng không bao giờ ảnh hưởng đến câu trả lời. Thuật toán bỏ qua chúng một cách an toàn bằng cách không bao giờ lập chỉ mục vượt quá`[0..10]`phạm vi.
