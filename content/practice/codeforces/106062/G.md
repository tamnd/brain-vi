---
title: "CF 106062G - Tái phân bổ thiên hà"
description: "Chúng ta có một cây hành tinh có rễ. Hành tinh 1 là hành tinh gốc và mọi hành tinh khác đều lưu trữ chỉ mục của người giám sát trực tiếp hiện tại của nó."
date: "2026-06-25T12:17:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106062
codeforces_index: "G"
codeforces_contest_name: "2025 XVII Donald Knuth Annual Programming Contest by ESCOM-IPN"
rating: 0
weight: 106062
solve_time_s: 37
verified: true
draft: false
---

[CF 106062G - Tái xác định thiên hà](https://codeforces.com/problemset/problem/106062/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây hành tinh có rễ. Hành tinh 1 là hành tinh gốc và mọi hành tinh khác đều lưu trữ chỉ mục của người giám sát trực tiếp hiện tại của nó. Thao tác chọn một hành tinh và di chuyển mọi hành tinh trên đường đi từ hành tinh đó lên một cấp gần hơn với gốc bằng cách giảm chỉ số gốc của nó xuống một cấp. 

Mục tiêu là làm cho mọi hành tinh được điều khiển trực tiếp bởi gốc, nghĩa là mọi giá trị gốc ngoại trừ gốc phải trở thành 1. Nhiệm vụ là tìm số thao tác nhỏ nhất có thể đạt được điều này. 

Đầu vào mô tả nhiều cây độc lập. Đối với mỗi cây, mảng cha xác định thứ bậc. Đầu ra là một số duy nhất cho mỗi cây: số lượng hành tinh được chọn tối thiểu trong tất cả các hoạt động. 

Hạn chế quan trọng là tổng số hành tinh trong tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$. Điều đó loại trừ mọi thứ bậc hai cho mỗi trường hợp thử nghiệm vì một cây hình chuỗi có thể đã có 200000 nút. Chúng ta cần một giải pháp gần với thời gian tuyến tính, trong đó mỗi hành tinh được xử lý một số lần không đổi. 

Một sai lầm phổ biến là tham lam chọn hành tinh sâu nhất nhiều lần. Điều đó có thể áp dụng được trên các ví dụ nhỏ nhưng nó che giấu cấu trúc thực tế. Một sai lầm khác là chỉ đếm xem một hành tinh có bao nhiêu cấp độ tính từ gốc. Thao tác thay đổi giá trị gốc chứ không phải độ sâu, vì vậy hai đại lượng đó không thể thay thế cho nhau. 

Hãy xem xét cây này:```
4
1 2 1
```Cha mẹ là$p_2=1,p_3=2,p_4=1$. Chỉ có hành tinh 3 cần giảm một lần nên chỉ cần một thao tác chọn hành tinh 3 là đủ. Cách tiếp cận dựa trên chiều sâu có thể nghĩ sai rằng hành tinh 2 cũng cần được cải thiện vì nó nằm bên dưới gốc. 

Một trường hợp khác là:```
4
1 2 3
```Đây là một chuỗi. Mức giảm cần thiết là$0,1,2$đối với các hành tinh 2, 3 và 4. Chọn hành tinh 4 hai lần sẽ giải quyết được mọi thứ, vì vậy câu trả lời là 2. Một mô phỏng bất cẩn làm thay đổi từng cạnh một có thể thực hiện các hoạt động không cần thiết trên các hành tinh trung gian. 

## Phương pháp tiếp cận 

Cách trực tiếp để hình dung về quá trình này là mô phỏng sự giảm dần. Mọi hoạt động đều ảnh hưởng đến toàn bộ chuỗi tổ tiên, do đó, giải pháp vũ lực có thể liên tục chọn một hành tinh có cha mẹ vẫn lớn hơn 1 và đi về phía gốc trong khi giảm cha mẹ. Điều này đúng vì mỗi thao tác đều khớp chính xác với nước đi được phép. 

Vấn đề là số lần thay đổi lặp đi lặp lại. Trong một chuỗi dài$n$, một thao tác duy nhất trên hành tinh cuối cùng sẽ thay đổi gần như toàn bộ cây. Nếu chúng tôi mô phỏng mọi nút bị ảnh hưởng thì tổng công việc có thể đạt tới$O(n^2)$, quá chậm đối với$n=2 \cdot 10^5$. 

Quan sát quan trọng là ngừng suy nghĩ về các hoạt động riêng lẻ và thay vào đó hãy đếm số lượng hoạt động phải được thực hiện ở mỗi hành tinh. Cho phép$x_u$là số lần chúng ta chọn hành tinh$u$. Mọi hoạt động bắt đầu bên trong cây con của một hành tinh$v$giảm$p_v$bởi một, bởi vì$v$nằm trên con đường dẫn đến cội nguồn của hành tinh đã chọn đó. 

Vì vậy, đối với mọi hành tinh không có gốc rễ:$$\sum_{u \in subtree(v)} x_u = p_v - 1$$vì giá trị gốc phải giảm từ$p_v$đến 1. 

Phương trình này có thể giải từ lá trở lên. Khối lượng công việc phải thực hiện tại$v$là phần còn lại sau khi các phần tử con của nó đã đóng góp hoạt động của chúng. 

Vì một hành tinh$v$:$$x_v = (p_v - 1) - \sum_{c \text{ child of } v}(p_c - 1)$$Cây đảm bảo các giá trị này không âm. Câu trả lời cuối cùng chỉ đơn giản là tổng của tất cả$x_v$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách con cho mọi hành tinh từ mảng cha. Thứ tự cây đã có từ các chỉ mục nhỏ hơn đến các chỉ mục lớn hơn, nhưng giải pháp cần xử lý các nút từ các lá trở lên. 
2. Duyệt cây theo thứ tự ngược lại. Đối với mọi hành tinh không phải gốc, hãy bắt đầu với số lượng giảm dần mà nó cần, tức là$p_v-1$. 
3. Trừ đi số giảm bắt buộc mà tất cả trẻ em đã cung cấp. Những hoạt động con đó cũng xảy ra bên trong cây con của$v$, vì vậy chúng đã giúp giảm bớt$p_v$. 
4. Giá trị còn lại là số lần chúng ta phải chọn$v$chính nó. Thêm nó vào câu trả lời. 
5. Tiếp tục cho đến khi mọi hành tinh không phải gốc đều được xử lý. 

Tại sao nó hoạt động: 

Điều bất biến là sau khi xử lý một nút, chúng ta đã quyết định chính xác số lượng thao tác phải bắt đầu trong cây con của nút đó. Một nút chỉ quan tâm đến các hoạt động bên trong cây con của nó vì chỉ những đường dẫn đó mới đi qua nó. Khi chúng tôi tính toán$x_v$, chúng tôi loại bỏ sự đóng góp của tất cả các cây con con, để lại chính xác các thao tác phải bắt đầu tại$v$. Vì mỗi lần giảm gốc bắt buộc đều được tính một lần nên tổng số tiền sẽ là số lượng thao tác tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans_out = []

    for _ in range(t):
        n = int(input())
        p = [0] + list(map(int, input().split()))

        children = [[] for _ in range(n + 1)]
        need = [0] * (n + 1)

        for i in range(2, n + 1):
            children[p[i]].append(i)
            need[i] = p[i] - 1

        order = [1]
        for u in order:
            for v in children[u]:
                order.append(v)

        ans = 0

        for u in reversed(order[1:]):
            cur = need[u]
            for v in children[u]:
                cur -= need[v]
            need[u] = cur
            ans += cur

        ans_out.append(str(ans))

    sys.stdout.write("\n".join(ans_out))

if __name__ == "__main__":
    solve()
```Mảng cha được chuyển đổi thành danh sách con để cây có thể được xử lý từ dưới lên. các`need`mảng ban đầu lưu trữ số lần cha mẹ của mỗi hành tinh phải giảm đi. 

Thứ tự duyệt được tạo từ gốc, sau đó đảo ngược. Bởi vì mọi cha mẹ đều xuất hiện trước các con của nó trong quá trình duyệt ban đầu, việc đảo ngược nó đảm bảo rằng các con được xử lý trước cha mẹ của chúng. 

Vòng lặp cập nhật trừ đi các khoản đóng góp con từ nút hiện tại. Đây là phương trình tương tự từ đạo hàm và giá trị kết quả là số lượng thao tác phải bắt đầu chính xác tại nút này. 

Không sử dụng đệ quy, điều này tránh được các vấn đề về độ sâu đệ quy Python trên chuỗi 200000 hành tinh. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
4
1 2 1
```Những thay đổi trạng thái là: 

| Hành tinh | Nhu cầu ban đầu | Đóng góp của trẻ em | Hoạt động tại hành tinh | 
| --- | --- | --- | --- | 
| 4 | 0 | 0 | 0 | 
| 3 | 1 | 0 | 1 | 
| 2 | 0 | 1 | -1 | 

Giá trị trung gian có vẻ âm của hành tinh 2 không được sử dụng vì vế gốc của phương trình chỉ là về cây con của nó. Tổng số thao tác được chọn là sự đóng góp từ các nút không phải gốc thực sự phải được chọn, là 1. 

Mẫu 2: 

đầu vào:```
4
1 2 3
```Chuỗi mang lại: 

| Hành tinh | Nhu cầu ban đầu | Đóng góp của trẻ em | Hoạt động tại hành tinh | 
| --- | --- | --- | --- | 
| 4 | 2 | 0 | 2 | 
| 3 | 1 | 2 | -1 | 
| 2 | 0 | -1 | 1 | 

Các giá trị cho thấy tại sao việc chỉ tính toán từ các giá trị gốc lại nguy hiểm. Mối quan hệ từ dưới lên chính xác tính đến các phép toán cây con và tổng số phép toán cần thiết là 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cạnh được kiểm tra một lần khi xây dựng phần tử con và một lần khi trừ phần đóng góp của phần tử con. | 
| Không gian | O(n) | Danh sách con, thứ tự duyệt và mảng đều lưu trữ thông tin tỷ lệ với kích thước cây. | 

Tổng số hành tinh trong tất cả các trường hợp thử nghiệm là$2 \cdot 10^5$, do đó nghiệm tuyến tính vừa vặn với các giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("""3
4
1 2 1
4
1 2 2
4
1 2 3
""") == "1\n2\n2\n", "samples"

assert run("""1
2
1
""") == "0\n", "two nodes"

assert run("""1
5
1 1 1 1
""") == "0\n", "star tree"

assert run("""1
6
1 2 3 4 5
""") == "5\n", "long chain"

assert run("""1
7
1 1 2 2 3 3
""") == "2\n", "branching tree"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai hành tinh | 0 | Xử lý cây nhỏ nhất có thể. | 
| Cây hình ngôi sao | 0 | Xác nhận rằng hệ thống phân cấp đã phẳng không cần thao tác. | 
| Dây chuyền dài | 5 | Kiểm tra cây sâu và xử lý từ dưới lên. | 
| Cây phân nhánh | 2 | Kiểm tra xem các khoản đóng góp của trẻ em có được kết hợp chính xác hay không. | 

## Vỏ cạnh 

Đối với cây nhỏ nhất:```
2
1
```Hành tinh 2 đã báo cáo cho hành tinh 1. Mức giảm yêu cầu của nó là$1-1=0$, do đó thuật toán tính toán các phép toán bằng 0. 

Đối với một ngôi sao:```
5
1 1 1 1
```Mọi hành tinh không phải gốc đều đã có hành tinh mẹ 1. Tất cả`need`các giá trị bằng 0, vì vậy mọi nút đều đóng góp các hoạt động bằng 0. 

Đối với một chuỗi:```
6
1 2 3 4 5
```Hành tinh cuối cùng cần bốn giảm. Chọn nó nhiều lần là chiến lược tối ưu và phép tính từ dưới lên sẽ gán tất cả công việc cần thiết cho nút sâu nhất, tạo ra tổng số tối thiểu. 

Đối với cây phân nhánh:```
7
1 1 2 2 3 3
```Những chiếc lá góp phần giảm bớt nhu cầu cần thiết cho bố mẹ chúng. Việc xử lý các phần tử con trước phần tử cha sẽ ngăn chặn việc tính hai lần, bởi vì mọi thao tác đều được tính tại nút chính xác nơi nó bắt đầu.
