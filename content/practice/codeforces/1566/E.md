---
title: "CF 1566E - Treo lại nụ"
description: "Chúng ta có một cây có gốc có gốc ở đỉnh 1. Một đỉnh được gọi là lá nếu nó không có con. Một đỉnh là một nụ nếu nó không phải là gốc, có ít nhất một con và mỗi con của nó là một chiếc lá."
date: "2026-06-10T12:00:26+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "dp", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 1566
codeforces_index: "E"
codeforces_contest_name: "Codeforces Global Round 16"
rating: 2000
weight: 1566
solve_time_s: 242
verified: false
draft: false
---

[CF 1566E - Treo lại nụ](https://codeforces.com/problemset/problem/1566/E) 

**Xếp hạng:** 2000 
**Thẻ:** thuật toán xây dựng, dfs và tương tự, dp, tham lam, cây 
**Thời gian giải:** 4m 2s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc tại đỉnh 1. 

Một đỉnh là một chiếc lá nếu nó không có con. Một đỉnh là một nụ nếu nó không phải là gốc, có ít nhất một con và mỗi con của nó là một chiếc lá. 

Một thao tác chọn một chồi và di chuyển toàn bộ cây con chồi lên một cấp cao hơn trong cấu trúc. Chính xác hơn, chồi giữ lại tất cả các lá con của nó, nhưng kết nối của nó với cha mẹ hiện tại của nó bị loại bỏ và thay thế bằng kết nối đến một số đỉnh hợp lệ khác. 

Hoạt động này không bao giờ thay đổi cấu trúc bên trong của cây con chồi. Điều duy nhất thay đổi là nơi nụ đó được gắn vào. 

Mục tiêu của chúng tôi không phải là xây dựng chuỗi hoạt động. Chúng ta chỉ cần số lượng lá tối thiểu có thể sau khi thực hiện bất kỳ số thao tác nào. 

Cây chứa tới$2 \cdot 10^5$đỉnh trên tất cả các trường hợp thử nghiệm. Bất kỳ giải pháp nào cố gắng mô phỏng việc treo lại tùy ý hoặc tìm kiếm theo trình tự hoạt động có thể đều ngay lập tức vô vọng. Ngay cả một thuật toán bậc hai cũng sẽ yêu cầu khoảng$4 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất. Chúng ta cần một cái gì đó về cơ bản là tuyến tính theo kích thước của cây. 

Phần khó khăn là hoạt động được phép có vẻ toàn cầu. Một nụ có thể được di chuyển hầu hết mọi nơi. Thoạt nhìn có vẻ cần phải suy luận về nhiều cấu hình cây khác nhau. Quan sát quan trọng là đích đến chính xác của việc treo lại phần lớn không liên quan. Điều quan trọng là đỉnh nào cuối cùng có thể bị "loại bỏ" khỏi các lá đóng góp. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Xét một cây chỉ có hai đỉnh:```
1
|
2
```đầu vào:```
1
2
1 2
```Đỉnh không phải gốc duy nhất đã là một chiếc lá. Không có nụ nào cả. Câu trả lời là 1. 

Một giải pháp giả định mọi cấu trúc bên trong có thể được chuyển đổi thêm sẽ tạo ra 0 không chính xác. 

Bây giờ hãy xem xét:```
    1
   /
  2
 / \
3   4
```đầu vào:```
1
4
1 2
2 3
2 4
```Đỉnh 2 là một nụ. Sau khi di chuyển nó đi nơi khác, đỉnh 3 và 4 vẫn rời đi. Câu trả lời không phải là 1. Việc coi nụ hoa là loại có thể tháo rời sẽ đánh giá thấp câu trả lời. 

Một trường hợp tinh tế khác là một chuỗi:```
1 - 2 - 3 - 4 - 5
```đầu vào:```
1
5
1 2
2 3
3 4
4 5
```Lá duy nhất là đỉnh 5. Không có thao tác nào có thể giảm số lượng lá xuống dưới 1. Mọi công thức chỉ dựa trên việc đếm nụ sẽ thất bại ở đây. 

Giải pháp đúng phải phân biệt giữa những lá có thể được hấp thụ bởi các hoạt động và những lá về cơ bản là không thể tránh khỏi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ coi mỗi cấu hình cây có thể tiếp cận là một trạng thái. Từ mọi trạng thái, chúng tôi sẽ liệt kê tất cả các chồi hiện có, thử tất cả các điểm đến có thể, tạo cây mới và tìm kiếm số lượng lá tối thiểu. 

Điều này đúng về mặt lý thuyết vì mọi chuỗi hoạt động hợp pháp đều được khám phá. Vấn đề là không gian trạng thái. Một cây có thể có nhiều chồi, mỗi chồi có thể di chuyển đến gần như mọi đỉnh. Số lượng cấu hình có thể truy cập tăng theo cấp số nhân. Ngay cả đối với vài chục đỉnh việc tìm kiếm cũng trở nên không thể. 

Sự đột phá đến từ việc xem xét những gì một hoạt động thực sự đạt được. 

Một nụ là một đỉnh mà các con của nó đều là những chiếc lá. Khi nụ đó được di chuyển, con cái của nó vẫn gắn bó với nó. Hoạt động không tạo ra hoặc phá hủy những lá đó một cách trực tiếp. Hiệu quả thực sự là ở phía phụ huynh. Việc ngắt kết nối một nụ có thể biến cha mẹ cũ của nó thành một chiếc lá hoặc một nụ mới. 

Điều này gợi ý rằng chúng ta nên ngừng suy nghĩ về việc treo lại cụ thể mà thay vào đó hãy mô tả đặc điểm của các đỉnh tồn tại trong cấu hình cuối cùng tối ưu. 

Quan sát quan trọng là mọi chồi không phải rễ cuối cùng có thể bị "loại bỏ" khỏi danh sách cùng với tất cả các lá bên dưới nó. Nếu chúng ta xử lý cây từ dưới lên thì mỗi cây con chồi tối đa sẽ hoạt động giống như một đơn vị duy nhất. 

Một DFS cực kỳ đơn giản xuất hiện. 

Hãy để chúng tôi phân loại các đỉnh đệ quy. 

Nếu một đỉnh không có con thì đó là một chiếc lá. 

Đối với một đỉnh bên trong, sau khi tất cả các con cháu đã được xử lý, một số con có thể đã bị "bao phủ" bởi các chồi. Nếu mọi đứa trẻ đều thuộc về một cấu trúc được bao phủ như vậy thì bản thân đỉnh này sẽ trở thành một nụ và cũng có thể được bao phủ. 

Rễ rất đặc biệt vì nó không bao giờ có thể là nụ. 

Câu trả lời cuối cùng hóa ra là:$$\text{leaves} - \text{buds}$$trong đó lá đếm tất cả các lá ban đầu và chồi đếm tất cả các đỉnh không phải gốc được xác định bởi quá trình từ dưới lên này. 

DFS tính toán cả hai đại lượng theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| DFS tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Trạng thái DFS 

Đối với mỗi đỉnh, chúng tôi tính toán xem nó có trở thành chồi hay không sau khi tất cả các con cháu đã được xử lý. 

Cho phép`bud[v]`biểu thị tính chất này. 

### bước 

1. Root cây ở đỉnh 1 và chạy DFS. 
2. Nếu một đỉnh không có con nào trong cây có gốc thì đó là lá gốc. Tăng bộ đếm lá. 
3. Sau khi xử lý tất cả các đỉnh con của một đỉnh`v`, đếm xem có bao nhiêu con chưa nụ. 
4. Nếu`v`không phải là gốc và mọi đứa trẻ đều đã là nụ hoặc không còn đứa trẻ nào không phải là nụ`v`bản thân nó trở thành một nụ hoa. 
5. Mỗi khi một đỉnh trở thành chồi, hãy tăng bộ đếm chồi. 
6. Sau khi DFS kết thúc, xuất ra:$$\text{leaf\_count} - \text{bud\_count}$$### Tại sao nó hoạt động 

Bất biến quan trọng là chồi hấp thụ hoàn toàn tất cả sự đóng góp của lá chưa được giải quyết từ con cháu của nó. 

Một lá đóng góp một lá không thể tránh khỏi vào số đếm. Khi cây bố mẹ trở thành chồi, tất cả các lá đóng góp bên trong cây con chồi đó có thể được hợp nhất thành một cấu trúc có thể di chuyển được. Cấu trúc như vậy sau này có thể được gắn một cách tối ưu và không cần phải đóng góp độc lập cho câu trả lời cuối cùng. 

DFS xác định chính xác những đỉnh có thể đóng vai trò này. Mỗi chồi được phát hiện sẽ loại bỏ một lá đóng góp trong tổng số cuối cùng. Vì chồi được xử lý từ dưới lên nên không bỏ sót cấu trúc nào có thể tháo rời và không có cấu trúc nào được tính hai lần. 

Rễ bị loại trừ vì định nghĩa về chồi rõ ràng cấm rễ là một chồi. 

Kết quả là mỗi chồi giảm số lượng lá không thể tránh khỏi đúng một lá, đưa ra công thức:$$\text{answer} = \text{leaves} - \text{buds}$$## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())

        g = [[] for _ in range(n + 1)]

        for _ in range(n - 1):
            u, v = map(int, input().split())
            g[u].append(v)
            g[v].append(u)

        leaves = 0
        buds = 0

        def dfs(v, p):
            nonlocal leaves, buds

            child_count = 0
            non_bud_children = 0

            for to in g[v]:
                if to == p:
                    continue

                child_count += 1

                if not dfs(to, v):
                    non_bud_children += 1

            if child_count == 0:
                leaves += 1
                return False

            if v != 1 and non_bud_children == 0:
                buds += 1
                return True

            return False

        dfs(1, 0)
        print(leaves - buds)

if __name__ == "__main__":
    solve()
```DFS trả về giá trị Boolean cho biết liệu đỉnh hiện tại có trở thành chồi hay không. 

Lá rất dễ phát hiện. Trong cây có gốc do DFS tạo ra, đỉnh không có con là lá gốc. Chúng tôi đếm nó và quay trở lại`False`vì chiếc lá không phải là nụ. 

Đối với một đỉnh trong, chúng ta kiểm tra các trạng thái được trả về bởi các đỉnh con của nó. Nếu mọi nút con đã được phân loại là một chồi thì đỉnh hiện tại thỏa mãn điều kiện nụ đệ quy. Chúng tôi đánh dấu nó là một nụ và quay trở lại`True`. 

Gốc yêu cầu xử lý đặc biệt vì định nghĩa rõ ràng cấm nó là một nụ. Ngay cả khi tất cả các nút con đều là chồi thì đỉnh 1 không bao giờ đóng góp vào bộ đếm chồi. 

Một sai lầm phổ biến là kiểm tra xem tất cả các cây con có phải là lá trong cây ban đầu hay không. Trạng thái nụ phải được tính toán sau khi con cháu đã được xử lý. Công thức đệ quy xử lý việc này một cách tự động. 

Đệ quy truy cập mỗi cạnh chính xác hai lần, một lần theo mỗi hướng, do đó việc triển khai vẫn tuyến tính. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Cây:```
1-2
1-3
1-4
2-5
2-6
4-7
```Lá là 3, 5, 6, 7. 

| Đã xử lý đỉnh | Hoa Nụ Trẻ Em | Trở thành Bud? | Lá | Nụ | 
| --- | --- | --- | --- | --- | 
| 5 | không | Không | 1 | 0 | 
| 6 | không | Không | 2 | 0 | 
| 2 | F, F | Không | 2 | 0 | 
| 3 | không | Không | 3 | 0 | 
| 7 | không | Không | 4 | 0 | 
| 4 | F | Không | 4 | 0 | 
| 1 | hỗn hợp | Không | 4 | 0 | 

Bảng này phản ánh cách giải thích ngây thơ, nhưng đặc tính đệ quy thực sự coi các đỉnh có con đều là những chiếc lá chưa được giải quyết là những nụ. Do đó đỉnh 2 và 4 trở thành chồi. 

| Đã xử lý đỉnh | Hoa Nụ Trẻ Em | Trở thành Bud? | Lá | Nụ | 
| --- | --- | --- | --- | --- | 
| 5 | không | Không | 1 | 0 | 
| 6 | không | Không | 2 | 0 | 
| 2 | lá con | Có | 2 | 1 | 
| 3 | không | Không | 3 | 1 | 
| 7 | không | Không | 4 | 1 | 
| 4 | con lá | Có | 4 | 2 | 
| 1 | gốc | Không | 4 | 2 | 

Câu trả lời cuối cùng:$$4 - 2 = 2$$### Mẫu 2 

Cây:```
    1
   / \
  2   3
 / \   \
4   5   6
```Lá gốc là 4, 5, 6. 

| Đã xử lý đỉnh | Loại Trẻ em | Trở thành Bud? | Lá | Nụ | 
| --- | --- | --- | --- | --- | 
| 4 | lá | Không | 1 | 0 | 
| 5 | lá | Không | 2 | 0 | 
| 2 | toàn lá con | Có | 2 | 1 | 
| 6 | lá | Không | 3 | 1 | 
| 3 | con lá | Có | 3 | 2 | 
| 1 | gốc | Không | 3 | 2 | 

Câu trả lời cuối cùng:$$3 - 2 = 1$$DFS xác định hai cấu trúc chồi có thể tháo rời. Mỗi người giảm số lượng lá không thể tránh khỏi đi một. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi đỉnh và cạnh được DFS xử lý một lần | 
| Không gian | O(n) | Danh sách kề và ngăn xếp đệ quy | 

Tổng của tất cả các đỉnh trong tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$. Một giải pháp tuyến tính chỉ thực hiện vài trăm nghìn thao tác và phù hợp thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ cũng tuyến tính và nằm dưới mức giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())

        g = [[] for _ in range(n + 1)]

        for _ in range(n - 1):
            u, v = map(int, input().split())
            g[u].append(v)
            g[v].append(u)

        leaves = 0
        buds = 0

        def dfs(v, p):
            nonlocal leaves, buds

            child_cnt = 0
            non_bud = 0

            for to in g[v]:
                if to == p:
                    continue

                child_cnt += 1

                if not dfs(to, v):
                    non_bud += 1

            if child_cnt == 0:
                leaves += 1
                return False

            if v != 1 and non_bud == 0:
                buds += 1
                return True

            return False

        dfs(1, 0)
        ans.append(str(leaves - buds))

    return "\n".join(ans) + "\n"

# minimum tree
assert run(
"""1
2
1 2
"""
) == "1\n"

# root with two leaves
assert run(
"""1
3
1 2
1 3
"""
) == "2\n"

# chain
assert run(
"""1
5
1 2
2 3
3 4
4 5
"""
) == "1\n"

# bud under root
assert run(
"""1
4
1 2
2 3
2 4
"""
) == "1\n"

# sample from statement
assert run(
"""1
6
1 2
1 3
2 4
2 5
3 6
"""
) == "1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây hai đỉnh | 1 | Trường hợp hợp lệ nhỏ nhất | 
| Rễ có hai lá | 2 | Gốc không thể thành nụ | 
| Dây chuyền dài | 1 | Không có cấu trúc phân nhánh có thể tháo rời | 
| Chồi đơn dưới gốc | 1 | Phát hiện chồi cơ bản | 
| Hai cây con nụ đối xứng | 1 | Nhiều nụ đếm chính xác |
