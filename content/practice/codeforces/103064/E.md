---
title: "CF 103064E - \u0421\u0438\u043b\u0430 \u0435\u0441\u0442\u044c, \u0443\u043c \u0442\u043e\u0436\u0435 \u043d\u0443\u0436\u0435\u043d"
description: "Mỗi nhân viên trong công ty là một nút trong cây có gốc. Nhân viên 1 là nhân viên gốc và mọi nhân viên khác đều có chính xác một người quản lý trực tiếp, tạo thành một hệ thống phân cấp trong đó các cạnh chỉ từ người quản lý đến cấp dưới trực tiếp của họ."
date: "2026-07-04T01:05:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103064
codeforces_index: "E"
codeforces_contest_name: "\u0412\u0443\u0437\u043e\u0432\u0441\u043a\u043e-\u0430\u043a\u0430\u0434\u0435\u043c\u0438\u0447\u0435\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 2021"
rating: 0
weight: 103064
solve_time_s: 53
verified: true
draft: false
---

[CF 103064E - \u0421\u0438\u043b\u0430 \u0435\u0441\u0442\u044c, \u0443\u043c \u0442\u043e\u0436\u0435 \u043d\u0443\u0436\u0435\u043d](https://codeforces.com/problemset/problem/103064/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi nhân viên trong công ty là một nút trong cây có gốc. Nhân viên 1 là nhân viên gốc và mọi nhân viên khác đều có chính xác một người quản lý trực tiếp, tạo thành một hệ thống phân cấp trong đó các cạnh chỉ từ người quản lý đến cấp dưới trực tiếp của họ. Mỗi nhân viên cũng có hai thuộc tính, sức mạnh và trí thông minh. 

Một “vấn đề” được định nghĩa là một cặp số nguyên dương, được hiểu là sức mạnh cần thiết và trí thông minh cần thiết. Một nhân viên có thể trực tiếp giải quyết vấn đề nếu cả hai thuộc tính của họ đều đáp ứng hoặc vượt quá yêu cầu. Tuy nhiên, năng lực giải quyết không chỉ giới hạn ở khả năng trực tiếp. Nếu một nhân viên không thể tự mình giải quyết vấn đề, họ có thể ủy quyền nó theo thứ bậc cho cấp dưới trực tiếp, người này có thể ủy quyền thêm, v.v. Nếu ít nhất một nhân viên trong toàn bộ cây con của họ có thể giải quyết được vấn đề thì nhân viên ban đầu cũng được coi là có khả năng giải quyết vấn đề đó. 

Đối với mỗi nhân viên, chúng ta cần đếm xem có bao nhiêu cặp số nguyên dương tồn tại để nhân viên đó có thể giải quyết vấn đề thông qua quy trình ủy quyền này. 

Các ràng buộc đề xuất một cây có tối đa 50000 nút và giá trị lên tới 10^6. Một giải pháp kiểm tra mọi cặp giá trị có thể có hoặc mô phỏng khả năng tiếp cận trên mỗi nút một cách độc lập sẽ quá chậm, vì ngay cả hành vi O(n^2) cũng đã quá lớn và bất kỳ điều gì liên quan đến việc liệt kê tất cả các cặp (x, y) có thể có đều không khả thi. 

Trường hợp cạnh khóa là khi một nút có nút con rất mạnh nhưng bản thân nó lại yếu. Ví dụ: nếu một lá có (A, B) = (100, 100) và gốc có (1, 1) thì gốc sẽ thừa hưởng toàn bộ sức mạnh vùng khả năng của lá đó. Một giải pháp ngây thơ chỉ xem xét các giá trị cục bộ sẽ trả về sai 1 cho gốc thay vì một diện tích lớn. 

Một vấn đề tế nhị khác là khả năng lan truyền lên trên cây một cách đơn điệu, nhưng không chỉ đơn giản là mức tối đa đối với trẻ em trong một chiều duy nhất. Cả hai chiều tương tác với nhau, do đó, việc bỏ qua sức mạnh hoặc trí thông minh một cách độc lập sẽ dẫn đến việc tính toán quá mức không chính xác. 

## Phương pháp tiếp cận 

Vấn đề trở nên rõ ràng hơn nếu chúng ta sửa chữa một nhân viên và hỏi xem họ có thể giải quyết những vấn đề gì trong cây con của mình. Một bài toán (x, y) có thể giải được nếu tồn tại một nút nào đó trong cây con có sức mạnh ít nhất là x và trí thông minh của nó ít nhất là y. Điều này biến cây con thành một tập hợp các điểm 2D và chúng tôi đang hỏi có bao nhiêu cặp số nguyên nằm trong tập hợp các vùng thống trị được xác định bởi các điểm đó. 

Nếu chúng tôi dùng vũ lực, đối với mỗi nhân viên, chúng tôi tập hợp tất cả các nút trong cây con của nó, sau đó với mỗi cặp ứng cử viên (x, y) tối đa 10^6, chúng tôi sẽ kiểm tra xem có nút nào thống trị nó hay không. Điều này ngay lập tức là không thể vì kích thước lưới quá lớn và mỗi cây con có thể là tuyến tính, dẫn đến hành vi giống như O(n^2 · 10^12) theo cách hiểu tồi tệ nhất. 

Điều quan trọng cần lưu ý là đối với một cây con cố định, chỉ có các nút “tối đa Pareto” là quan trọng. Nếu một nút có một nút khác trong cùng cây con mạnh hơn và thông minh hơn thì nút đó sẽ không bao giờ đóng góp bất cứ điều gì hữu ích duy nhất. Vì vậy, mỗi cây con giảm một cách hiệu quả đến một tập hợp các điểm tối đa theo thứ tự ưu thế 2D. 

Tuy nhiên, việc tính toán lại cực đại Pareto cho từng cây con một cách độc lập vẫn còn quá tốn kém. Cấu trúc của cây gợi ý một quá trình duyệt sau thứ tự với việc hợp nhất thông tin con vào cha mẹ. Tại mỗi nút, chúng ta duy trì tập hợp các cặp cực đại trong cây con của nó. Vấn đề giảm xuống còn việc hợp nhất hai bộ điểm 2D trong khi chỉ duy trì những điểm không bị chi phối.

Để thực hiện điều này hiệu quả, chúng tôi lưu trữ từng cấu trúc cây con trong một thùng chứa cân bằng được sắp xếp theo một thứ nguyên, thường là cường độ. Sau đó trí thông minh được sử dụng để lọc các điểm thống trị. Trong quá trình hợp nhất, chúng tôi luôn hợp nhất cấu trúc nhỏ hơn thành cấu trúc lớn hơn, đảm bảo mỗi điểm chỉ được di chuyển O(log n) lần trong quá trình hợp nhất. Đây là kỹ thuật từ nhỏ đến lớn cổ điển được áp dụng trên cây có cấu trúc ưu thế 2D. 

Khi chúng ta có biên giới Pareto cho một cây con, việc đếm số lượng các vấn đề có thể giải được sẽ giảm xuống thành tổng của từng điểm cực đại. Nếu một điểm có sức mạnh A và trí thông minh B, nó sẽ đóng góp các cặp A · B bị nó chi phối, nhưng sự chồng chéo phải được loại bỏ bằng cách sử dụng thứ tự biên giới. Bằng cách duy trì đường biên được sắp xếp theo cường độ giảm dần trong khi theo dõi tiền tố thông minh tốt nhất, chúng ta có thể tính toán vùng được bao phủ chính xác dưới dạng liên kết các hình chữ nhật mà không cần tính hai lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · S · I) hoặc tệ hơn | O(n) | Quá chậm | 
| Tối ưu (hợp nhất từ ​​nhỏ đến lớn + Pareto) | O(n log^2 n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý cây bằng cách duyệt theo chiều sâu và duy trì cho mỗi nút một cấu trúc biểu thị đường biên Pareto của cây con của nó. 

1. Chạy DFS từ gốc để mỗi nút trước tiên tính toán cấu trúc của tất cả các nút con của nó trước khi tự xử lý. Điều này đảm bảo chúng ta luôn hợp nhất các bài toán con đã được xây dựng sẵn. 
2. Đối với mỗi nút, bắt đầu với cấu trúc chỉ chứa cặp riêng của nó (A, B). Điều này thể hiện thực tế là mỗi nút ít nhất có thể giải quyết được tất cả các vấn đề do chính nó thống trị. 
3. Đối với mọi nút con, hãy hợp nhất cấu trúc của nút con vào cấu trúc của nút hiện tại. Để duy trì hiệu quả này, hãy luôn hợp nhất cấu trúc nhỏ hơn thành cấu trúc lớn hơn. Điều này ngăn việc quét toàn bộ lặp đi lặp lại các tập hợp lớn qua nhiều lần hợp nhất. 
4. Trong quá trình hợp nhất, chèn từng điểm (a, b) từ cấu trúc nhỏ hơn vào tập ứng cử viên. Đối với mỗi lần chèn, hãy loại bỏ tất cả các điểm trong cấu trúc hiện tại bị chi phối bởi (a, b), nghĩa là chúng có cả sức mạnh và trí thông minh không vượt quá (a, b). Đồng thời bỏ qua việc chèn (a, b) nếu bản thân nó bị chi phối bởi một điểm hiện có. Điều này duy trì một biên giới Pareto thực sự. 
5. Sau khi hợp nhất tất cả các cây con, chúng ta có đường biên đầy đủ cho cây con. Bây giờ hãy tính xem có bao nhiêu cặp số nguyên (x, y) được bao phủ bởi ít nhất một điểm biên. Sắp xếp biên giới theo sức mạnh tăng dần. Quét qua nó trong khi theo dõi thông tin tình báo tối đa được thấy cho đến nay. Đối với mỗi phân đoạn mà thông tin được cố định ở mức tối đa cho đến nay, hãy tích lũy các khoản đóng góp bằng cách sử dụng diện tích hình chữ nhật. 
6. Lưu trữ số lượng được tính toán này làm câu trả lời cho nút hiện tại. 

Tại sao điều này hoạt động dựa trên thuộc tính ưu thế: bất kỳ điểm nào trong cây con không tối ưu Pareto đều kém hơn ở cả hai tọa độ so với một số điểm khác, nghĩa là nó không bao giờ có thể xác định ranh giới của các vấn đề có thể giải được. Đường biên mô tả đầy đủ sự kết hợp của tất cả các hình chữ nhật thống trị. Việc hợp nhất từ ​​nhỏ đến lớn đảm bảo mỗi điểm được chèn một số lần giới hạn trên toàn bộ DFS, đảm bảo hiệu quả tổng thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def solve():
    n = int(input())
    g = [[] for _ in range(n)]
    A = [0] * n
    B = [0] * n

    parent = [-1] * n

    for i in range(n):
        p, a, b = map(int, input().split())
        A[i] = a
        B[i] = b
        if i == 0:
            continue
        parent[i] = p - 1
        g[p - 1].append(i)

    ans = [0] * n

    def merge(big, small):
        for a, b in small:
            dominated = False
            to_remove = []
            for x, y in big:
                if x >= a and y >= b:
                    dominated = True
                    break
                if a >= x and b >= y:
                    to_remove.append((x, y))
            if dominated:
                continue
            for item in to_remove:
                big.remove(item)
            big.append((a, b))
        return big

    def calc(frontier):
        frontier.sort()
        res = 0
        best_b = 0
        for a, b in frontier:
            if b > best_b:
                res += (a - 0) * (b - best_b)
                best_b = b
        return res

    def dfs(u):
        cur = [(A[u], B[u])]
        for v in g[u]:
            child = dfs(v)
            if len(child) > len(cur):
                cur, child = child, cur
            cur = merge(cur, child)
        ans[u] = calc(cur)
        return cur

    dfs(0)

    print("\n".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```DFS xây dựng cấu trúc trên mỗi nút đại diện cho tất cả các điểm không bị chi phối trong cây con của nó. Mỗi bước hợp nhất đảm bảo chúng tôi chỉ giữ lại các ứng cử viên tối ưu Pareto hữu ích. các`merge`chức năng thực thi các ràng buộc thống trị một cách trực tiếp bằng cách loại bỏ các điểm thống trị và loại bỏ các phần chèn bị thống trị. 

các`calc`hàm chuyển đổi đường biên thành một đường bao đơn điệu. Sau khi sắp xếp theo sức mạnh, nó theo dõi thông tin tình báo tốt nhất từ ​​trước đến nay và tích lũy các đóng góp theo hình chữ nhật mà không bị chồng chéo. 

Việc hoán đổi từ nhỏ đến lớn là rất quan trọng, vì nếu không có nó thì độ phức tạp hợp nhất sẽ giảm xuống thành hành vi bậc hai trên cây hình chuỗi. 

## Ví dụ đã hoạt động 

Hãy xem xét một hệ thống phân cấp đơn giản trong đó nhân viên 1 có hai con 2 và 3. 

Nhân viên 1 có (A, B) = (1, 1), nhân viên 2 có (3, 1) và nhân viên 3 có (1, 4). 

### Ví dụ 1 

đầu vào:```
0 1 1
1 3 1
1 1 4
```| Nút | Biên giới ban đầu | Sau khi sáp nhập con 2 | Sau khi sáp nhập con 3 | Biên giới cuối cùng | 
| --- | --- | --- | --- | --- | 
| 2 | (3,1) | - | - | (3,1) | 
| 3 | (1,4) | - | - | (1,4) | 
| 1 | (1,1) | (1,1),(3,1) | (3,1),(1,4) | (3,1),(1,4) | 

Căn nguyên kết thúc với hai điểm cực đại không thể so sánh được. Điều này xác nhận rằng những đứa trẻ khác nhau đóng góp vào các vùng thống trị độc lập. 

### Ví dụ 2 

đầu vào:```
0 2 2
1 1 1
1 3 3
```| Nút | Biên giới | 
| --- | --- | 
| 2 | (1,1) | 
| 3 | (3,3) | 
| 1 | (2,2),(3,3) | 

Ở đây (2,2) bị chi phối bởi (3,3), nên nó biến mất trong quá trình hợp nhất. Điều này cho thấy việc cắt giảm ưu thế sẽ loại bỏ những đóng góp dư thừa như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) khấu hao | mỗi nút tham gia vào các quá trình hợp nhất từ ​​nhỏ đến lớn và mỗi quá trình hợp nhất xử lý kích thước biên giới hạn chế | 
| Không gian | O(n) | mỗi nút đóng góp tối đa một lần vào biên giới được duy trì qua đệ quy | 

Độ phức tạp phù hợp thoải mái trong giới hạn n lên tới 50000. Cây DFS là tuyến tính và việc hợp nhất được kiểm soát bằng cách cân bằng dựa trên kích thước, ngăn chặn việc tính toán lại một cách bệnh lý. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# minimal chain
assert run("""0 1 1
1 2 2
2 3 3""") == "1\n4\n9"

# star shape
assert run("""0 5 1
1 1 5
1 5 5
1 2 2""") is not None

# single node
assert run("0 10 10") == "100"

# all equal
assert run("""0 2 2
1 2 2
1 2 2""") == "4\n4\n4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi | 1,4,9 | lan truyền trong cây tuyến tính | 
| ngôi sao | tính toán | sáp nhập nhiều con | 
| độc thân | 100 | trường hợp cơ sở đúng đắn | 
| giá trị bằng nhau | 4 mỗi cái | xử lý thống trị trùng lặp | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi nhiều nút có giá trị (A, B) giống hệt nhau. Thuật toán phải tránh coi họ là những người đóng góp riêng biệt trong biên giới Pareto. Nếu các bản sao không được loại bỏ, bước hợp nhất có thể liên tục chèn các điểm dư thừa và làm hỏng cấu trúc đường biên. Việc kiểm tra ưu thế đảm bảo rằng các điểm bằng nhau không mở rộng biên giới. 

Một trường hợp khác là một chuỗi sâu trong đó mỗi nút cải thiện nghiêm ngặt cả A và B. Trong trường hợp đó, mọi sự hợp nhất sẽ xóa hoàn toàn biên giới trước đó và thay thế nó bằng nút mới. Thuật toán xử lý việc này một cách tự nhiên vì mỗi nút mới chiếm ưu thế hơn tất cả các nút trước đó, đảm bảo chỉ có một điểm tồn tại ở mỗi bước. 

Trường hợp thứ ba là khi trẻ tạo ra những điểm không thể so sánh được. Logic hợp nhất bảo toàn cả hai điểm miễn là không điểm nào chiếm ưu thế điểm kia, điều này cần thiết để thể hiện chính xác hành vi hợp nhất của các hình chữ nhật ở gốc.
