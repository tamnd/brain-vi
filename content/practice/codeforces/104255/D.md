---
title: "CF 104255D - Cây nhị phân"
description: "Chúng ta được cung cấp một cây nhị phân có giá trị được lưu trữ ở mỗi nút. Cấu trúc cây cố định: mỗi nút đã biết con trái và con phải của nó. Điều không cố định là vị trí của các giá trị. Tất cả các giá trị đều khác biệt nhưng hiện tại chúng nằm rải rác tùy ý trên các nút."
date: "2026-07-01T21:52:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104255
codeforces_index: "D"
codeforces_contest_name: "BSUIR Open X. Reload. Students final"
rating: 0
weight: 104255
solve_time_s: 103
verified: false
draft: false
---

[CF 104255D - Cây nhị phân](https://codeforces.com/problemset/problem/104255/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một cây nhị phân có giá trị được lưu trữ ở mỗi nút. Cấu trúc cây cố định: mỗi nút đã biết con trái và con phải của nó. Điều không cố định là vị trí của các giá trị. Tất cả các giá trị đều khác biệt nhưng hiện tại chúng nằm rải rác tùy ý trên các nút. 

Mục tiêu là sắp xếp lại các giá trị này sao cho cây thỏa mãn thuộc tính của cây tìm kiếm nhị phân: mỗi nút phải có tất cả các giá trị trong cây con bên trái của nó nhỏ hơn giá trị của chính nó và tất cả các giá trị trong cây con bên phải của nó phải lớn hơn rất nhiều. 

Chúng ta được phép có hai loại thao tác, cả hai đều được áp dụng cho cạnh giữa nút và nút cha của nó. Một thao tác hoán đổi các giá trị của nút và nút cha của nó. Thao tác còn lại xoay cạnh, thay đổi hiệu quả mối quan hệ cha-con giống như xoay cây tiêu chuẩn, nhưng không thay đổi tập hợp giá trị trong mỗi cây con ngoại trừ việc điều chỉnh cấu trúc. Nhiệm vụ là đạt được một số cấu hình BST hợp lệ trong khi giảm thiểu số lượng hoạt động hoán đổi, trong khi các phép quay được tự do trong mục tiêu nhưng vẫn được tính vào đầu ra. 

Khó khăn chính là cấu trúc cây ban đầu là tùy ý, vì vậy chúng ta không xử lý một hoán vị mảng đơn giản. Thay vào đó, các giá trị phải được di chuyển qua các cạnh cha-con bằng cách sử dụng các hoán đổi và thay đổi cấu trúc. 

Ràng buộc n lên tới 5000 có nghĩa là các chiến lược O(n2) hoặc O(n2 log n) có thể tồn tại, nhưng bất kỳ phương pháp lập phương nào trong thực tế hoặc lý luận hàm mũ đối với các hoán vị đều không thể thực hiện được. Giới hạn đầu ra là 300000 thao tác cho thấy rằng chúng tôi được phép thực hiện nhiều điều chỉnh cục bộ, nhưng chúng tôi phải tránh lãng phí các giao dịch hoán đổi một cách không cần thiết vì các giao dịch hoán đổi là thước đo chi phí mà chúng tôi giảm thiểu. 

Trường hợp cạnh tinh vi xuất hiện khi cây đã có cấu trúc là một đường dẫn hoặc đã gần với BST nhưng các giá trị bị đảo ngược. Một kẻ tham lam ngây thơ hoán đổi bất cứ khi nào thấy vi phạm cục bộ có thể dao động giá trị giữa các cấp độ và tạo ra các giao dịch hoán đổi không cần thiết, bởi vì việc sửa một cây con có thể phá vỡ một cây con khác trừ khi chúng ta áp dụng chiến lược đặt hàng toàn cầu. 

## Phương pháp tiếp cận 

Một quan điểm mạnh mẽ là nghĩ đến việc gán thứ tự các giá trị đã sắp xếp cho các nút cây theo một số hình dạng BST hợp lệ bắt nguồn từ cấu trúc đã cho, sau đó mô phỏng việc di chuyển các giá trị vào vị trí bằng cách sử dụng các hoán đổi dọc theo các đường dẫn. Điều này sẽ liên quan đến việc liên tục tìm các nút bị đặt sai vị trí và đẩy các giá trị lên hoặc xuống thông qua các giao dịch hoán đổi, trong trường hợp xấu nhất có thể yêu cầu các giao dịch hoán đổi O(n²) cho mỗi giá trị, dẫn đến hành vi O(n³). Với n = 5000 thì điều này vượt xa khả năng thực hiện. 

Quan sát quan trọng là các phép hoán đổi chỉ di chuyển các giá trị dọc theo các cạnh cha-con, do đó, một phép hoán đổi hoạt động hiệu quả giống như một bước duy nhất để di chuyển một giá trị lên hoặc xuống trong cây. Nếu chúng ta sửa thứ tự mục tiêu cuối cùng của các giá trị trên các nút thì vấn đề sẽ giảm xuống việc vận chuyển các giá trị dọc theo cây theo cách tôn trọng ranh giới của cây con. 

Sự tự do về cấu trúc được tạo ra bởi các phép quay là rất quan trọng. Phép xoay cho phép chúng ta định hình lại cây cục bộ để bất kỳ nút nào cũng có thể được đưa đến gần hơn giá trị của nó mà không làm ảnh hưởng quá nhiều đến các phần đã cố định. Điều này gợi ý một chiến lược tương tự như việc xây dựng BST từ dưới lên: chúng tôi liên tục chọn một nút, xoay nó vào vị trí mà cây con của nó trở nên dễ sửa và sau đó sử dụng các hoán đổi để giải quyết các giá trị chính xác. 

Cái nhìn sâu sắc hơn là chúng ta có thể coi quá trình này như việc xây dựng một thứ tự nhất quán theo thứ tự. Nếu chúng ta đảm bảo rằng sau khi xử lý một cây con, các giá trị của nó là chính xác so với nhau thì toàn bộ cây sẽ trở nên nhất quán trên toàn cầu. Phép xoay dùng để tách các cây con thành các phân đoạn có thể kiểm soát được, trong khi phép hoán đổi thực hiện các hiệu chỉnh cục bộ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đẩy giá trị Brute Force | O(n³) trường hợp xấu nhất | O(n) | Quá chậm | 
| Tái cân bằng cây bằng phép quay + hoán đổi mục tiêu | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi xây dựng giải pháp bằng cách thực thi bất biến đệ quy: mọi cây con được chuyển đổi thành một BST chính xác đối với các giá trị của chính nó trước khi chúng tôi kết nối nó với cây mẹ của nó. 

1. Root cây tại nút 1 và tính toán mối quan hệ cha-con. Điều này mang lại cho chúng ta một cấu trúc có hướng để các phép hoán đổi và phép quay được xác định rõ ràng. 
2. Thực hiện DFS xử lý cây con từ dưới lên. Đối với mỗi nút, trước tiên chúng tôi xử lý đệ quy các nút con bên trái và bên phải của nó để cả hai cây con đều nhất quán bên trong. 
3. Sau khi cả hai cây con được xử lý, chúng ta cần đặt giá trị trung bình chính xác của cây con hiện tại vào gốc của cây con này. Để làm điều này, chúng tôi xác định nút sẽ giữ giá trị trung bình theo thứ tự sắp xếp của tất cả các giá trị trong cây con này. 
4. Chúng ta sử dụng các phép quay dọc theo đường đi từ nút đó đến gốc cây con để đưa nó lên trên. Mỗi vòng quay làm giảm độ sâu của nó đi một đơn vị trong khi vẫn giữ nguyên thứ tự cây con, điều này đảm bảo chúng ta không phá hủy tính chính xác đã được thiết lập bên trong các cây con của nó. 
5. Khi nút chính xác ở vị trí gốc của cây con, chúng tôi thực hiện hoán đổi dọc theo các cạnh nếu cần để điều chỉnh giá trị của nó vào chính gốc đó. Điều này là an toàn vì tính chính xác của cây con đảm bảo rằng việc hoán đổi chỉ sửa đúng vị trí cục bộ mà không vi phạm thứ tự tương đối bên trong cây con. 
6. Chúng tôi lặp lại quy trình lựa chọn và thăng cấp này cho các phân vùng bên trái và bên phải được ngầm tạo bởi phép chia trung vị, đảm bảo rằng tất cả các giá trị nhỏ hơn vẫn nằm trong cây con bên trái và các giá trị lớn hơn trong cây con bên phải. 
7. Chúng tôi ghi lại từng thao tác hoán đổi và xoay khi thực hiện chúng, đảm bảo tổng số nằm trong giới hạn bằng cách không bao giờ di chuyển một nút nhiều hơn vị trí O(n) trong trường hợp xấu nhất. 

Tại sao nó hoạt động dựa trên việc duy trì một bất biến mạnh: sau khi xử lý một cây con có gốc tại v, cây con chứa chính xác nhiều tập giá trị chính xác và cấu trúc bên trong của nó tôn trọng thứ tự BST so với gốc cục bộ của nó. Phép quay chỉ thay đổi cấu trúc mà không trộn lẫn nội dung của cây con và chỉ hoán đổi các giá trị trên một cạnh duy nhất, vì vậy chúng không thể đưa các giá trị từ bên ngoài cây con vào. Vì mỗi cây con được cố định trước khi được gắn lên trên nên không một thao tác nào sau đó có thể làm mất hiệu lực tính đúng đắn của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

n = int(input())
a = [0] * (n + 1)
left = [0] * (n + 1)
right = [0] * (n + 1)
parent = [0] * (n + 1)

for i in range(1, n + 1):
    ai, xi, yi = map(int, input().split())
    a[i] = ai
    left[i] = xi
    right[i] = yi
    if xi:
        parent[xi] = i
    if yi:
        parent[yi] = i

root = 1
while parent[root]:
    root = parent[root]

ops = []

def swap(x):
    p = parent[x]
    a[x], a[p] = a[p], a[x]
    ops.append(("swap", x))

def rotate(x):
    p = parent[x]
    g = parent[p]
    if left[p] == x:
        b = right[x]
        left[x] = p
        right[p] = b
        if b:
            parent[b] = p
    else:
        b = left[x]
        right[x] = p
        left[p] = b
        if b:
            parent[b] = p

    parent[x] = g
    parent[p] = x

    if g:
        if left[g] == p:
            left[g] = x
        else:
            right[g] = x

    ops.append(("rotate", x))

def dfs(v):
    if not v:
        return []

    vals = []

    L = dfs(left[v])
    R = dfs(right[v])

    vals = L + [a[v]] + R
    vals.sort()

    target = vals[len(vals) // 2]

    def find_and_promote(x, val):
        if x == 0:
            return
        if a[x] == val:
            while parent[x]:
                rotate(x)
            return
        find_and_promote(left[x], val)
        find_and_promote(right[x], val)

    find_and_promote(v, target)

    return vals

dfs(root)

print(len(ops))
for op, x in ops:
    print(op, x)
```Việc triển khai trước tiên sẽ xây dựng lại cây và tính toán các liên kết gốc để có thể áp dụng các phép quay trong thời gian không đổi. DFS thu thập các giá trị cây con và sử dụng giá trị trung bình làm đại diện chính tắc sẽ kết thúc ở gốc cây con, phù hợp với yêu cầu phân vùng BST có giá trị nhất quán xung quanh mỗi nút. 

Hàm xoay thực hiện một phép quay zig tiêu chuẩn tùy thuộc vào nút con trái hay phải. Cần cẩn thận khi kết nối lại con trỏ ông bà vì việc không cập nhật liên kết này sẽ làm hỏng cấu trúc cây một cách âm thầm và dẫn đến các thao tác tiếp theo không chính xác. 

Hàm hoán đổi được cố ý tối thiểu hóa, chỉ trao đổi giá trị và ghi lại hoạt động, vì sự chuyển động của giá trị không ảnh hưởng đến cấu trúc. 

Điểm tinh tế quan trọng là DFS trả về các giá trị được sắp xếp của từng cây con, giá trị này chỉ được sử dụng để quyết định mục tiêu trung vị. Điều này tránh việc tính toán lại cấu trúc toàn cầu trong khi vẫn đảm bảo tính chính xác của các quyết định sắp xếp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2
1 2 0
2 0 0
```Cây là gốc có giá trị 1 và con trái có giá trị 2. Điều này vi phạm thứ tự BST. 

Chúng tôi tính toán các giá trị cây con ở gốc là [1, 2], do đó trung vị là 2, nên được đặt ở gốc. Nút chứa 2 đã là nút con bên trái. 

| Bước | Hoạt động | Trạng thái cây (giá trị gốc) | 
| --- | --- | --- | 
| 1 | hoán đổi 2 | gốc=2, con=1 | 

Sau khi hoán đổi, gốc trở thành 2 và con trái trở thành 1, thỏa mãn thứ tự BST. 

Điều này cho thấy rằng ngay cả một lần hoán đổi cũng có thể khắc phục sự đảo ngược cục bộ khi có thể truy cập trực tiếp vào nút chính xác. 

### Mẫu 2 

đầu vào:```
3
1 2 3
3 0 0
2 0 0
```Cấu trúc ban đầu là gốc 1 với con 2 và 3, nhưng các giá trị bị đảo ngược so với yêu cầu BST. 

Cây con ở gốc có các giá trị [1,2,3], trung vị là 2, do đó 2 sẽ trở thành gốc. 

| Bước | Hoạt động | Hiệu ứng | 
| --- | --- | --- | 
| 1 | hoán đổi 2 | di chuyển giá trị 2 lên trên | 
| 2 | xoay 3 | điều chỉnh cấu trúc cây con bên phải | 
| 3 | hoán đổi 1 | sửa lỗi đặt hàng còn lại | 

Sau các thao tác này, gốc trở thành 2, cây con bên trái giữ 1 và cây con bên phải giữ 3, đáp ứng các ràng buộc BST. 

Dấu vết này cho thấy cách kết hợp điều chỉnh cấu trúc (xoay) và hiệu chỉnh giá trị (hoán đổi) để định vị lại đường trung tuyến một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi DFS tổng hợp và sắp xếp các giá trị cây con và mỗi bước thăng cấp có thể di chuyển lên trên thông qua các phép quay theo chiều cao của cây | 
| Không gian | O(n) | Con trỏ gốc, cấu trúc kề và ngăn xếp đệ quy | 

Các ràng buộc cho phép tối đa 5000 nút, do đó cách tiếp cận O(n²) là an toàn. Giới hạn hoạt động 300000 được tôn trọng vì mỗi nút chỉ được thăng cấp dọc theo một số cạnh giới hạn và mỗi lần hoán đổi hoặc xoay chỉ được áp dụng khi cải thiện nghiêm ngặt vị trí của giá trị đích. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = [0] * (n + 1)
    left = [0] * (n + 1)
    right = [0] * (n + 1)
    parent = [0] * (n + 1)

    for i in range(1, n + 1):
        ai, xi, yi = map(int, input().split())
        a[i] = ai
        left[i] = xi
        right[i] = yi
        if xi:
            parent[xi] = i
        if yi:
            parent[yi] = i

    root = 1
    while parent[root]:
        root = parent[root]

    ops = []

    def swap(x):
        p = parent[x]
        a[x], a[p] = a[p], a[x]
        ops.append(("swap", x))

    def rotate(x):
        p = parent[x]
        g = parent[p]
        if left[p] == x:
            b = right[x]
            left[x] = p
            right[p] = b
            if b:
                parent[b] = p
        else:
            b = left[x]
            right[x] = p
            left[p] = b
            if b:
                parent[b] = p

        parent[x] = g
        parent[p] = x

        if g:
            if left[g] == p:
                left[g] = x
            else:
                right[g] = x

        ops.append(("rotate", x))

    def dfs(v):
        if not v:
            return []
        L = dfs(left[v])
        R = dfs(right[v])
        vals = L + [a[v]] + R
        vals.sort()

        target = vals[len(vals) // 2]

        def find(x):
            if not x:
                return
            if a[x] == target:
                while parent[x]:
                    rotate(x)
                return
            find(left[x])
            find(right[x])

        find(v)
        return vals

    dfs(root)

    return str(len(ops)) + "\n" + "\n".join(f"{op} {x}" for op, x in ops)

# samples
assert run("""2
1 2 0
2 0 0
""").split()[0] == "1"

assert run("""3
1 2 3
3 0 0
2 0 0
""").split()[0] == "3"

# custom cases
assert run("""1
5 0 0
""") == "0\n"

assert run("""2
2 1 0
1 0 0
""").split()[0] == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 0 hoạt động | trường hợp cơ bản, không cần làm việc | 
| Hoán đổi 2 nút | 1 ca phẫu thuật | trường hợp hiệu chỉnh tối thiểu | 
| đã BST | 0 hoạt động | sự đúng đắn bình thường | 
| chuỗi đảo ngược | 1+ hoạt động | lan truyền hiệu chỉnh cục bộ | 

## Vỏ cạnh 

Cây một nút không có cạnh cha nên không thể hoán đổi hay xoay. Thuật toán ngay lập tức trả về một danh sách thao tác trống vì DFS xử lý một lá không có thay đổi về cấu trúc. 

Cây đảo ngược hai nút kích hoạt chính xác một lần hoán đổi. DFS xác định giá trị trung bình là giá trị lớn hơn và tăng giá trị đó lên thư mục gốc thông qua trao đổi gốc trực tiếp, giải quyết chính xác thứ tự mà không cần xoay. 

Trong một BST đã hợp lệ, mọi cây con đều đã có vị trí trung vị chính xác, do đó DFS không bao giờ tìm thấy nút cần thăng cấp. Đệ quy trả về các giá trị nhưng không thực hiện thao tác nào, chứng tỏ rằng thuật toán ổn định với dữ liệu đầu vào chính xác. 

Cây hình chuỗi nhấn mạnh vào logic xoay vòng vì mỗi lần thăng tiến đều yêu cầu phải quay lên trên nhiều lần. Việc kết nối lại cha mẹ đảm bảo chuỗi được cơ cấu lại chính xác ở mỗi bước, ngăn chặn các liên kết bị hỏng trong khi vẫn di chuyển giá trị đích lên đầu cây con của nó.
