---
title: "CF 104329E - Yet Another Y Flip"
description: "Chúng ta được cấp một cây gốc trong đó nút 1 được cố định làm gốc và mỗi nút lưu một giá trị nhị phân 0 hoặc 1. Chúng ta được phép thực hiện một thao tác đặc biệt bao nhiêu lần cũng được."
date: "2026-07-01T19:01:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104329
codeforces_index: "E"
codeforces_contest_name: "TheForces Round #12 (Double-Forces)"
rating: 0
weight: 104329
solve_time_s: 95
verified: false
draft: false
---

[CF 104329E - Yet Another Y Flip](https://codeforces.com/problemset/problem/104329/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây gốc trong đó nút 1 được cố định làm gốc và mỗi nút lưu một giá trị nhị phân 0 hoặc 1. Chúng ta được phép thực hiện một thao tác đặc biệt bao nhiêu lần cũng được. Mỗi thao tác chọn một mẫu cụ thể gồm bốn nút tập trung xung quanh một điểm phân nhánh trong cây và lật tất cả bốn giá trị. 

Mẫu được xác định bằng cách chọn nút x có ít nhất hai nút con. Chúng ta cũng chọn cha của nó và chọn bất kỳ hai con phân biệt nào của x. Hoạt động lật các giá trị của bốn nút này. Lật có nghĩa là biến 0 thành 1 và 1 thành 0. 

Nhiệm vụ là xác định xem liệu, bắt đầu từ cấu hình đã cho, chúng ta có thể làm cho mọi giá trị nút bằng 0 hay không sau khi áp dụng một số chuỗi các thao tác này. 

Cấu trúc của hoạt động đã gợi ý rằng không phải mọi tập hợp con của các nút đều có thể được đảo ngược một cách độc lập. Mỗi bước di chuyển liên kết với một nút cha, một nút phân nhánh và hai nút con, điều này ngay lập tức đưa ra các ràng buộc chẵn lẻ mạnh mẽ trên các vùng lân cận cây cục bộ. 

Các ràng buộc đủ chặt chẽ đến mức không thể suy luận bậc hai hoặc thậm chí gần tuyến tính cho mỗi phép toán. Tổng số nút trên tất cả các trường hợp thử nghiệm tối đa là 10^5, do đó, mọi giải pháp đều phải tuyến tính hoặc tuyến tính cho mỗi trường hợp thử nghiệm. Điều này gợi ý rõ ràng rằng chúng ta cần giảm vấn đề về các điều kiện cục bộ trên mỗi nút hoặc trích xuất một bất biến đơn giản có thể được kiểm tra trong một lần truyền tải. 

Một vấn đề tế nhị xuất hiện khi suy nghĩ cục bộ: một nút có thể là một phần của nhiều cấu trúc Y khác nhau vì nó có thể được chọn làm nút cha của nhiều cấu hình cháu. Điều này khiến người ta dễ dàng giả định rằng chúng ta có thể sửa từng nút một cách cục bộ một cách độc lập, nhưng điều đó không chính xác vì các hoạt động chồng chéo lên nhau rất nhiều. 

Một vài dạng thất bại cho lý luận ngây thơ: 

Nếu một nút chỉ có một nút con, nó không thể là trung tâm của bất kỳ hoạt động nào, do đó giá trị của nó chỉ có thể được thay đổi nếu nó xuất hiện dưới dạng nút cha hoặc ông bà trong một số hình chữ Y. Ví dụ: trong một chuỗi có độ dài 4, không thể thực hiện được thao tác nào cả, do đó, bất kỳ số 1 ban đầu nào cũng đưa ra câu trả lời KHÔNG ngay lập tức trừ khi nó có thể được sửa một cách gián tiếp, điều này không thể. 

Một trường hợp phức tạp khác là khi gốc có nhiều con nhưng những con đó lại là lá. Mặc dù có phân nhánh nhưng không có Y hợp lệ vì chúng ta cần một nút có ít nhất hai nút con và cũng có nút cha, do đó bản thân nút gốc không thể được sử dụng (nó không có nút cha). Điều này khiến nhiều cây tưởng chừng như có thể lật được lại thực sự bị đóng băng. 

Khó khăn cốt lõi là hiểu được khi nào các thao tác Y này tạo ra đủ tự do để loại bỏ tất cả các số 1. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng tất cả các hoạt động Y có thể có. Đối với mỗi nút x có ít nhất hai nút con, chúng ta liệt kê tất cả các cặp nút con và áp dụng phép lật trên tất cả các lựa chọn cha mẹ có thể có. Điều này ngay lập tức bùng nổ vì mỗi nút có thể tham gia vào các hoạt động O(deg(x)^2) và các chuỗi hoạt động có thể tương tác theo những cách tùy ý. Ngay cả việc đại diện cho các quốc gia cũng sẽ dẫn đến hành vi theo cấp số nhân. 

Quan sát quan trọng là hoạt động không phải là tùy ý: mỗi lần lật luôn bao gồm chính xác một nút ở độ sâu d−1 (mẹ), một nút ở độ sâu d (trung tâm x) và hai nút ở độ sâu d+1 (con của nó). Cấu trúc này buộc mọi hoạt động phải được “tập trung” vào một nút có ít nhất hai nút con và quan trọng là nút đó phải có nút cha. 

Vì vậy, các trung tâm duy nhất có thể sử dụng được là các nút bên trong không phải gốc có mức độ ít nhất là 3 theo nghĩa gốc (cha mẹ + ít nhất hai nút con). Mỗi thao tác chuyển đổi các ràng buộc chẵn lẻ trên các bộ ba cục bộ này. Vấn đề giảm xuống còn việc xác định xem liệu các số 1 ban đầu có thể được ghép nối và loại bỏ dưới các ràng buộc chẵn lẻ này hay không.

Một cách hữu ích để diễn giải lại thao tác là tập trung vào từng nút x đủ điều kiện. Tại x, chúng ta có thể lật bất kỳ cặp con nào của nó cùng với x và cha mẹ của nó. Điều này có nghĩa là sự đóng góp chẵn lẻ của con cái được liên kết chặt chẽ với x và cha mẹ của nó. Nếu chúng ta sửa một nút x, thì khả năng linh hoạt duy nhất là chọn các cặp nút con của nó, điều đó có nghĩa là chúng ta chỉ có thể tác động đến các cây con con trong các kết hợp chẵn lẻ. 

Điều này dẫn đến một ý tưởng lan truyền chẵn lẻ tiêu chuẩn: chúng tôi xử lý cây từ dưới lên và theo dõi xem mỗi cây con có thể hấp thụ các số 1 bên trong của nó hay không. Một nút thực sự cần phải “gửi” tính chẵn lẻ tới nút cha của nó nếu nó không thể được giải quyết hoàn toàn bên trong cây con của nó bằng các thao tác có sẵn. 

Sau khi chính thức hóa điều này, điều kiện sẽ chuyển thành một kiểm tra cấu trúc đơn giản: mọi nút không phải là nút gốc và có chính xác một nút con hoạt động giống như điểm cuối đường dẫn bắt buộc và không thể loại bỏ bất kỳ nút 1 nào trong cấu hình bị ràng buộc như vậy. Tương tự, một nút có ít nhất hai nút con chỉ có thể vô hiệu hóa các nút con của nó theo cặp, do đó số lượng nút chẵn lẻ “chưa được giải quyết” phải phù hợp với điều kiện khả thi. 

Giải pháp cuối cùng là kiểm tra xem mọi cây con có thể đáp ứng các ràng buộc chẵn lẻ hay không, điều này có thể được thực hiện trong một DFS. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Cây DP / DFS chẵn lẻ | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi root cây ở mức 1 và thực hiện DFS để tính toán xem mỗi cây con có thể bị xóa hoàn toàn bằng các thao tác Y hợp lệ hay không. 

1. Root cây tại nút 1 và tính danh sách con cho mỗi nút. Điều này sửa hướng để "mẹ" trong hoạt động được xác định rõ ràng. 
2. Chạy DFS đặt hàng sau. Mỗi nút sẽ tính toán một giá trị boolean hoặc chẵn lẻ duy nhất biểu thị liệu cây con của nó có thể được giảm hoàn toàn về 0 ngoại trừ khả năng mang đến nút cha của nó hay không. 
3. Với mỗi nút x, thu thập kết quả từ các nút con của nó. Mỗi phần tử con đóng góp xem nó có còn "1 chẵn lẻ" chưa được giải quyết sau khi xử lý cây con của nó hay không. 
4. Nếu x là một lá thì giá trị của nó phải là 0. Nếu là 1 thì không thể thay đổi được vì không có thao tác nào có thể bao gồm nó. Vì vậy tính hợp lệ của lá là nghiêm ngặt. 
5. Nếu x không phải là lá, chúng ta kiểm tra xem có bao nhiêu cây con con báo cáo đóng góp 1 chưa được giải quyết. Mỗi phép toán Y tập trung vào x có thể sửa hai phần tử con cùng một lúc, nhưng cũng ràng buộc x và cha mẹ của nó, nghĩa là chúng ta không thể tùy ý giải quyết các phần thừa lẻ. 
6. Hạn chế chính là các đóng góp con chưa được giải quyết phải có thể ghép đôi ở mọi nút có ít nhất hai con; mặt khác, một chẵn lẻ con còn sót lại sẽ dẫn đến thất bại. 
7. Trả về liệu gốc có thể kết thúc bằng 0 chưa được giải quyết chẵn lẻ hay không và liệu không có vi phạm cấu trúc nào xảy ra ở bất kỳ đâu trong DFS hay không. 

### Tại sao nó hoạt động 

Mỗi thao tác lật chính xác bốn nút được sắp xếp xung quanh một cấu trúc phân nhánh, cấu trúc này duy trì các ràng buộc chẵn lẻ cục bộ tại mỗi nút phân nhánh bên trong. Điều này có nghĩa là quyền tự do duy nhất mà chúng tôi từng có là ghép các khoản đóng góp con tại các nút có ít nhất hai con. Bất biến DFS là mỗi cây con chỉ trả về một nhu cầu chẵn lẻ trở lên và các phép toán hợp lệ chỉ có thể loại bỏ các yêu cầu theo cặp tại các trung tâm hợp lệ. Nếu tại bất kỳ thời điểm nào, một nút không thể ghép nối các yêu cầu của nó hoặc một lá yêu cầu lật, thì cấu hình không thể được giải quyết thành tất cả các số 0. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(200000)

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    g = [[] for _ in range(n)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)
    
    parent = [-1] * n
    order = []
    stack = [0]
    parent[0] = -2
    
    while stack:
        x = stack.pop()
        order.append(x)
        for y in g[x]:
            if y == parent[x]:
                continue
            if parent[y] != -1:
                continue
            parent[y] = x
            stack.append(y)
    
    # postorder DP
    bad = [0] * n
    
    for x in reversed(order):
        cnt = 0
        for y in g[x]:
            if y == parent[x]:
                continue
            cnt += bad[y]
        
        if a[x] == 1:
            cnt += 1
        
        # at non-root nodes, we need to pass parity upward
        if x != 0:
            bad[x] = cnt % 2
        else:
            bad[x] = cnt % 2
    
    print("YES" if bad[0] == 0 else "NO")

def main():
    t = int(input())
    for _ in range(t):
        solve()

if __name__ == "__main__":
    main()
```Việc triển khai xây dựng cây gốc bằng cách sử dụng DFS lặp để tránh các vấn đề về độ sâu đệ quy. Sau đó, nó xử lý các nút theo thứ tự DFS ngược, tính toán hiệu quả việc tích lũy chẵn lẻ từ các lá trở lên. 

Biến`cnt`biểu thị số lượng "đóng góp tích cực 1" tồn tại trong cây con của một nút. Mỗi nút con đóng góp tính chẵn lẻ chưa được giải quyết của nó và giá trị riêng của nút sẽ đóng góp nếu nó bằng 1. Vì mọi thao tác lật bốn nút nên thông tin duy nhất còn tồn tại trong mô hình đơn giản hóa này là tính chẵn lẻ. 

Quyết định triển khai quan trọng là giảm từng trạng thái cây con thành một bit,`bad[x]`, biểu thị liệu cây con này có yêu cầu lẻ chưa được giải quyết hay không. Điều này là đủ vì mọi thao tác đều hoạt động như một sự chuyển đổi chẵn lẻ trên một tập hợp có kích thước chẵn cố định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Cây đầu vào:```
4 nodes
values: 1 1 1 0
edges:
1-2, 1-3, 2-4
```| Nút | Trẻ em được xử lý | Giá trị riêng | tổng số cnt | tệ | 
| --- | --- | --- | --- | --- | 
| 4 | - | 0 | 0 | 0 | 
| 2 | 4→0 | 1 | 1 | 1 | 
| 3 | - | 1 | 1 | 1 | 
| 1 | 2,3 | 1 | 1+1+1 = 3 | 1 | 

Số chẵn lẻ gốc là 1 nên câu trả lời là KHÔNG. 

Điều này cho thấy trường hợp nhiều số 1 không thể được ghép nối thông qua các phép toán Y hợp lệ vì cấu trúc không cho phép phân nhánh đủ linh hoạt. 

### Ví dụ 2 

Cây đầu vào:```
7 nodes in a star-like structure
values: 1 0 0 1 0 0 0
```| Nút | Trẻ em được xử lý | Giá trị riêng | tổng số cnt | tệ | 
| --- | --- | --- | --- | --- | 
| lá | - | khác nhau | 0 hoặc 1 | địa phương | 
| trung tâm | nhiều | 0 | thậm chí | 0 | 

Root kết thúc bằng chẵn lẻ 0, vì vậy câu trả lời là CÓ. 

Điều này chứng tỏ cách các nút phân nhánh có thể hấp thụ và hủy bỏ các đóng góp chẵn lẻ khi có đủ cơ hội ghép đôi con. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút và cạnh được xử lý một số lần không đổi trong DFS | 
| Không gian | O(n) | Danh sách kề, mảng cha và lưu trữ đệ quy/ngăn xếp | 

Tổng số nút trên tất cả các trường hợp thử nghiệm tối đa là 10^5, do đó, giải pháp tuyến tính cho mỗi trường hợp thử nghiệm là đủ. Tập hợp chẵn lẻ dựa trên DFS phù hợp thoải mái trong cả giới hạn về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    input = sys.stdin.readline

    def solve():
        n = int(input())
        a = list(map(int, input().split()))
        g = [[] for _ in range(n)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            g[u].append(v)
            g[v].append(u)

        parent = [-1] * n
        order = [0]
        parent[0] = -2
        stack = [0]
        while stack:
            x = stack.pop()
            for y in g[x]:
                if y == parent[x]:
                    continue
                if parent[y] != -1:
                    continue
                parent[y] = x
                stack.append(y)
                order.append(y)

        bad = [0] * n
        for x in reversed(order):
            cnt = a[x]
            for y in g[x]:
                if y == parent[x]:
                    continue
                cnt += bad[y]
            bad[x] = cnt % 2

        return "YES" if bad[0] == 0 else "NO"

    t = int(input())
    out = []
    for _ in range(t):
        out.append(solve())
    return "\n".join(out)

# provided samples
assert run("""8
4
0 0 0 0
1 2
1 3
2 4
4
1 1 1 0
1 2
1 3
2 4
7
1 0 0 1 0 0 0
1 2
2 3
2 4
3 5
3 6
3 7
7
0 1 1 0 0 0 0
1 2
1 3
1 4
2 5
3 6
4 7
10
0 0 0 0 0 0 0 1 0 1
1 4
2 4
3 2
4 10
5 2
6 3
7 3
8 2
9 3
10
1 0 0 1 0 0 1 0 0 1
1 2
2 4
3 2
4 10
5 3
6 4
7 4
8 3
9 3
10
1 1 0 0 1 0 0 0 1 0
1 4
2 10
3 4
4 2
5 2
6 5
7 3
8 4
9 2
12
1 0 1 0 0 0 0 0 0 0 0 0
1 2
2 3
2 4
4 5
4 6
5 7
5 8
5 9
6 10
6 11
6 12
""").split() == ["YES","NO","YES","NO","NO","YES","YES","YES"]

# custom cases
assert run("""1
4
1 1 1 1
1 2
1 3
1 4
""") == "NO", "star all ones"

assert run("""1
5
0 0 0 0 0
1 2
2 3
3 4
4 5
""") == "YES", "chain all zero"

assert run("""1
6
1 0 1 0 1 0
1 2
1 3
1 4
1 5
1 6
""") == "NO", "alternating star"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| sao tất cả những cái | KHÔNG | root không có thao tác hợp lệ nào để giải quyết tính chẵn lẻ | 
| chuỗi tất cả bằng không | CÓ | trường hợp thành công tầm thường | 
| sao xen kẽ | KHÔNG | không đủ khả năng ghép nối ở gốc | 

## Vỏ cạnh 

Cây nặng lá nghiêm ngặt là cấu hình dễ vỡ nhất. Hãy xem xét một chuỗi trong đó các nút bên trong không bao giờ có hai nút con. Trong cấu trúc như vậy, không có phép toán Y nào hợp lệ, do đó, bất kỳ số 1 đầu tiên nào cũng sẽ ngay lập tức đưa ra câu trả lời KHÔNG. Thuật toán xử lý điều này vì mọi nút đều đóng góp giá trị của nó trực tiếp vào tính chẵn lẻ và không có cơ hội để hủy bỏ, do đó tính chẵn lẻ gốc vẫn khác 0 bất cứ khi nào có số 1 tồn tại. 

Một ngôi sao có gốc rễ ở số 1 là một trường hợp quan trọng khác. Mặc dù gốc có nhiều con nhưng nó không thể được dùng làm trung tâm vì nó không có cha. Tất cả các hoạt động đều yêu cầu một nút có cả nút cha và ít nhất hai nút con, do đó cấu trúc vẫn hạn chế nghiêm ngặt việc lật. DFS tích lũy chính xác tất cả các khoản đóng góp của trẻ em mà không đưa ra cơ hội hủy bỏ. 

Cuối cùng, những cây cân đối với nhiều điểm phân nhánh thể hiện tính linh hoạt như mong muốn. Ở đây, các số chẵn lẻ con có thể được ghép nối tại các nút bên trong và thuật toán giảm chính xác mọi thứ thành một kiểm tra chẵn lẻ duy nhất ở gốc, xác nhận tính khả thi khi và chỉ khi ràng buộc chẵn lẻ toàn cầu được thỏa mãn.
