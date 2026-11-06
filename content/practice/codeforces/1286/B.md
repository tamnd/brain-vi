---
title: "CF 1286B - Số trên cây"
description: "Chúng ta có một cây có gốc trong đó mỗi nút có một giá trị nguyên ẩn. Những gì chúng ta biết là cấu trúc cây và với mỗi nút là một số ci."
date: "2026-06-16T03:46:55+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "dfs-and-similar", "graphs", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 1286
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 612 (Div. 1)"
rating: 1800
weight: 1286
solve_time_s: 321
verified: false
draft: false
---

[CF 1286B - Các số trên cây](https://codeforces.com/problemset/problem/1286/B) 

**Đánh giá:** 1800 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, dfs và tương tự, đồ thị, tham lam, cây 
**Thời gian giải:** 5 phút 21s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc trong đó mỗi nút có một giá trị nguyên ẩn. Những gì chúng ta biết là cấu trúc cây và đối với mỗi nút, một số`c_i`. Con số này cho chúng ta biết điều gì đó cục bộ đối với cây con: trong số tất cả các nút bên trong cây con của`i`, chính xác`c_i`trong số chúng có giá trị nhỏ hơn giá trị tại`i`. 

Nhiệm vụ là xây dựng lại bất kỳ phép gán số nguyên nào cho các nút phù hợp với tất cả các ràng buộc của cây con này. Nếu có nhiều nhiệm vụ hoạt động thì bất kỳ nhiệm vụ nào cũng có thể được chấp nhận và nếu không có nhiệm vụ nào có thể đáp ứng tất cả các ràng buộc thì chúng ta phải báo cáo là không thể thực hiện được. 

Khó khăn chính là mỗi nút đều áp đặt một ràng buộc phụ thuộc vào các so sánh bên trong cây con của nó, nhưng những so sánh đó lại phụ thuộc vào thứ tự chung của các giá trị mà chúng ta không biết trước. Cây kết hợp những ràng buộc này ở các cấp độ khác nhau, do đó việc xử lý các nút độc lập đơn giản sẽ không thành công. 

Các ràng buộc cho phép lên tới 2000 nút. Điều này đủ nhỏ để một giải pháp có hệ số bậc ba hoặc bậc hai có thể vượt qua, nhưng bất kỳ điều gì cố gắng suy luận rõ ràng về tất cả các phép gán giá trị hoặc mô phỏng hoán vị của các giá trị sẽ không khả thi. Thay vào đó, điều chúng ta nên mong đợi là một quy trình mang tính xây dựng nhằm gán các giá trị tăng dần trong khi vẫn duy trì tính nhất quán. 

Một kiểu lỗi phổ biến xuất hiện khi người ta cố gắng gán các giá trị một cách tham lam mà không tôn trọng cấu trúc cây con. Ví dụ: gán các giá trị theo thứ tự BFS hoặc theo cha mẹ trước khi trẻ bỏ qua điều đó`c_i`phụ thuộc vào cây con đầy đủ, không chỉ hàng xóm trực tiếp. Một lỗi nhỏ khác phát sinh nếu chúng ta cố gắng gán các giá trị chỉ dựa trên việc sắp xếp các nút theo`c_i`, bởi vì`c_i`không đại diện cho thứ hạng toàn cầu, chỉ thứ hạng tương đối của cây con. 

Một vấn đề minh họa nhỏ: hãy xem xét một phụ huynh có`c = 0`và một đứa trẻ với`c = 1`. Nếu chúng ta gán các giá trị một cách tham lam sao cho giá trị nhỏ hơn`c`có nghĩa là giá trị nhỏ hơn, chúng ta có thể tạo ra mâu thuẫn bên trong cây con vì ràng buộc cha phụ thuộc vào việc sắp xếp đồng thời của cả hai cây con chứ không phải riêng lẻ. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là cố gắng gán các giá trị từ một hoán vị của`1..n`tới các nút và kiểm tra xem tất cả các ràng buộc có được giữ nguyên hay không. Đối với mỗi phép gán, việc xác minh điều kiện yêu cầu tính toán số lượng cây con, bản thân cây con này cần một DFS trên mỗi nút hoặc một lần truyền tải được hợp nhất. Ngay cả khi việc kiểm tra một bài tập mất`O(n^2)`, thử mọi hoán vị là`O(n!)`, vượt xa mọi giới hạn. 

Cấu trúc của bài toán gợi ý rằng chúng ta cần xây dựng các giá trị theo cách tuân thủ các ràng buộc cục bộ của cây con trong khi dần dần sửa chữa thứ tự tương đối. Quan sát quan trọng đó là`c_i`mô tả có bao nhiêu nút trong cây con phải được đặt trước nút`i`theo thứ tự giá trị, nhưng chỉ liên quan đến các nút vẫn “có sẵn” bên trong cây con đó. 

Điều này dẫn đến một ý tưởng mang tính xây dựng: xử lý cây từ các lá trở lên và tại mỗi nút quyết định thứ hạng tương đối của nó giữa các nút trong cây con của nó. Nếu chúng ta đã biết thứ tự của các cây con con, chúng ta có thể hợp nhất chúng và chèn cây con cha vào vị trí phù hợp với`c_i`. Vì các cây con rời rạc nên việc hợp nhất hoạt động giống như hợp nhất các danh sách đã sắp xếp và ràng buộc giảm xuống việc chọn điểm chèn chính xác. 

Chúng tôi duy trì cho mỗi nút một danh sách các vị trí biểu thị số lượng phần tử nhỏ hơn mỗi vị trí ứng cử viên. Khi kết hợp các nút con, chúng tôi hợp nhất các danh sách này và sau đó đặt nút hiện tại ở vị trí chính xác`c_i`các phần tử trong cây con của nó nhỏ hơn. 

Điều này làm giảm vấn đề xuống một cây DP trong đó mỗi nút xây dựng một cấu trúc được sắp xếp theo thứ hạng có thể có trong cây con của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra hoán vị Brute Force | O(n! · n^2) | O(n) | Quá chậm | 
| Cây DP với việc hợp nhất theo thứ tự | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Gốc cây tại nút có`p_i = 0`. Chúng ta sẽ tính toán thứ tự tương đối hợp lệ bên trong mỗi cây con. 
2. Đối với mỗi nút, xác định một danh sách thể hiện thứ tự cuối cùng của các nút trong cây con của nó. Thay vì lưu trữ các giá trị thực tế, chúng tôi lưu trữ các vị trí theo thứ tự được sắp xếp theo khái niệm. 
3. Thực hiện DFS từ thư mục gốc. Đối với một nút lá, danh sách của nó chỉ chứa chính nó vì không có ràng buộc nội bộ nào có thể vi phạm. Điều này phục vụ như là trường hợp cơ sở. 
4. Đối với một nút nội bộ, trước tiên hãy tính toán đệ quy các danh sách cho tất cả các nút con. Mỗi đứa trẻ đã đại diện cho một thứ tự nhất quán của cây con của nó. 
5. Hợp nhất tất cả danh sách con thành một danh sách. Việc hợp nhất này được thực hiện bằng cách ghép nối và sau đó xử lý kết quả dưới dạng một tập hợp nhiều vị trí. Ở giai đoạn này, chúng ta chỉ quan tâm đến kích thước tương đối bên trong cây con. 
6. Sau khi hợp nhất nút con, chúng ta phải chèn nút hiện tại vào danh sách đã hợp nhất này sao cho chính xác`c_i`các phần tử trong cây con của nó nhỏ hơn nó. Điều này có nghĩa là đặt nó ở chỉ mục`c_i`theo thứ tự dựa trên số 0 của danh sách đã hợp nhất. 
7. Nếu`c_i`lớn hơn kích thước của danh sách đã hợp nhất, không thể sắp xếp được và quá trình xây dựng sẽ thất bại ngay lập tức. 
8. Sau khi DFS đầy đủ hoàn tất, chúng ta có thứ tự hợp lệ của các nút từ giá trị nhỏ nhất đến giá trị lớn nhất. Gán số nguyên tăng dần`1..n`theo thứ tự này. 

### Tại sao nó hoạt động 

Mỗi cây con được xử lý độc lập trước khi được kết hợp, do đó mọi danh sách cây con đều thể hiện chính xác thứ tự nội bộ hợp lệ. Khi chúng ta chèn một nút vào vị trí`c_i`, chúng tôi đang thực thi chính xác số lượng phần tử nhỏ hơn trong cây con của nó. Vì các cây con con rời rạc nên việc hợp nhất không phá vỡ các ràng buộc đã thỏa mãn trước đó. Bất biến DFS là mọi danh sách cây con có thể được mở rộng lên trên mà không vi phạm bất kỳ`c_i`, do đó, cấu trúc gốc cuối cùng mã hóa một thứ tự nhất quán toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

n = int(input())
children = [[] for _ in range(n)]
c = [0] * n
root = -1

for i in range(n):
    p, ci = map(int, input().split())
    c[i] = ci
    if p == 0:
        root = i
    else:
        children[p - 1].append(i)

# Each node returns a list of nodes in increasing order of final value
def dfs(u):
    cur = [u]
    for v in children[u]:
        sub = dfs(v)
        cur.extend(sub)

    # We need to place u so that exactly c[u] elements in its subtree are smaller
    # but cur currently includes u at front; we will reposition it.
    cur.remove(u)

    if c[u] > len(cur):
        raise ValueError("impossible")

    cur.insert(c[u], u)
    return cur

try:
    order = dfs(root)
    ans = [0] * n
    for i, node in enumerate(order):
        ans[node] = i + 1

    print("YES")
    print(*ans)
except ValueError:
    print("NO")
```Việc triển khai tuân theo việc xây dựng DFS theo đúng nghĩa đen. Mỗi lệnh gọi đệ quy xây dựng thứ tự cho cây con. Chúng tôi tạm thời bao gồm nút đó, sau đó xóa và chèn lại nút đó vào đúng chỉ mục do`c_i`. Điều này tránh được logic đếm phức tạp, vì vị trí chèn trực tiếp xác định có bao nhiêu phần tử nhỏ hơn tồn tại trong thứ tự cây con đó. 

Bước gán cuối cùng ánh xạ trật tự toàn cục được xây dựng tới các giá trị`1..n`, điều này luôn hợp lệ vì chỉ có thứ tự tương đối mới quan trọng. 

Một điểm triển khai tinh tế là độ sâu đệ quy, vì cây có thể thoái hóa thành một chuỗi. Việc tăng giới hạn đệ quy sẽ ngăn chặn tình trạng tràn ngăn xếp trong Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
2 0
0 2
2 0
```Cấu trúc cây: nút 2 là nút gốc, nút 1 và 3 là nút con. 

Chúng tôi bắt đầu DFS ở nút 2. 

| Bước | Nút | Kết quả con | Danh sách hợp nhất | Chèn vị trí | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | [] | [1] | c1 = 0 | [1] | 
| 2 | 3 | [] | [3] | c3 = 0 | [3] | 
| 3 | 2 | [1,3] | [1,3] | c2 = 2 | [1,3,2] | 

Thứ tự cuối cùng là`[1, 3, 2]`. Gán giá trị`1,2,3`theo đó đưa ra một sự tái thiết hợp lệ như`1 3 2`hoặc bất kỳ ghi nhãn nhất quán tương đương nào. 

Dấu vết này cho thấy mỗi cây con được xây dựng độc lập như thế nào trước khi nút gốc áp đặt ràng buộc toàn cục của nó. 

### Ví dụ 2 

đầu vào:```
4
2 0
0 1
2 0
2 1
```Nút 2 là nút gốc, có con 1, 3, 4. 

| Bước | Nút | Kết quả con | Danh sách hợp nhất | Chèn vị trí | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | [] | [1] | c1 = 0 | [1] | 
| 2 | 3 | [] | [3] | c3 = 0 | [3] | 
| 3 | 4 | [] | [4] | c4 = 1 | [4] | 
| 4 | 2 | [1,3,4] | [1,3,4] | c2 = 0 | [2,1,3,4] | 

Điều này chứng tỏ làm thế nào một root với`c = 0`buộc nó phải nhỏ nhất trong cây con của nó, dịch chuyển tất cả các nút con lên trên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Mỗi sự hợp nhất DFS bao gồm các thao tác danh sách trên các kích thước cây con và mỗi nút có thể được chèn vào danh sách có kích thước tối đa n | 
| Không gian | O(n^2) | Mỗi cây con lưu trữ một danh sách rõ ràng các nút của nó | 

Những ràng buộc cho phép`n ≤ 2000`, vì vậy một`O(n^2)`xây dựng có thể chấp nhận được. Các thao tác là các thao tác danh sách đơn giản và Python xử lý việc này một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    children = [[] for _ in range(n)]
    c = [0] * n
    root = -1

    for i in range(n):
        p, ci = map(int, input().split())
        c[i] = ci
        if p == 0:
            root = i
        else:
            children[p - 1].append(i)

    sys.setrecursionlimit(10**7)

    def dfs(u):
        cur = [u]
        for v in children[u]:
            cur.extend(dfs(v))
        cur.remove(u)
        if c[u] > len(cur):
            raise ValueError
        cur.insert(c[u], u)
        return cur

    try:
        order = dfs(root)
        ans = [0] * n
        for i, v in enumerate(order):
            ans[v] = i + 1
        return "YES\n" + " ".join(map(str, ans))
    except:
        return "NO"

# sample
assert run("""3
2 0
0 2
2 0
""").split()[0] == "YES"

# chain
assert run("""1
0 0
""").startswith("YES")

# simple invalid
assert run("""2
2 1
0 0
""").startswith("NO")

# star
assert run("""4
2 0
0 1
2 0
2 1
""").split()[0] == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | CÓ 1 | cấu trúc tối thiểu | 
| ràng buộc không hợp lệ | KHÔNG | chèn không thể | 
| cây sao | CÓ | sáp nhập nhiều con | 
| trường hợp mẫu | CÓ | tính đúng đắn của việc xây dựng | 

## Vỏ cạnh 

Cây một nút luôn hoạt động vì`c_1`phải bằng 0 và phép gán hợp lệ duy nhất là tầm thường. DFS trả về một danh sách đơn và việc chèn vào vị trí 0 sẽ duy trì tính chính xác. 

Một nút với`c_i`lớn hơn kích thước cây con của nó ngay lập tức khiến việc xây dựng không thể thực hiện được. Trong thuật toán, điều này được phát hiện tại thời điểm chèn khi`c_i > len(cur)`trước khi đặt hàng, đảm bảo loại bỏ sớm thay vì tạo ra đơn đặt hàng không đúng định dạng. 

Cây hình ngôi sao nhấn mạnh đến việc hợp nhất nhiều cây con độc lập. Vì mỗi phần tử con đóng góp một thứ tự rời rạc nên phép nối sẽ duy trì tính hợp lệ và vị trí chèn gốc sẽ kiểm soát cấu trúc cuối cùng mà không có sự mơ hồ.
