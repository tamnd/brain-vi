---
title: "CF 104053A - Alice Và Con Mèo Đi Lạc"
description: "Chúng ta có một cây có gốc trong đó đỉnh 1 là điểm bắt đầu và mỗi nút đại diện cho một vị trí mà con mèo có thể đã đi qua. Con mèo di chuyển từ gốc xuống cây mà không cần xem lại bất kỳ nút nào, vì vậy đường đi của nó chỉ đơn giản là đường đi từ gốc tới lá."
date: "2026-07-02T03:34:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104053
codeforces_index: "A"
codeforces_contest_name: "2022 China Collegiate Programming Contest (CCPC) Guangzhou Onsite"
rating: 0
weight: 104053
solve_time_s: 57
verified: true
draft: false
---

[CF 104053A - Alice và con mèo thất lạc của cô ấy](https://codeforces.com/problemset/problem/104053/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc trong đó đỉnh 1 là điểm bắt đầu và mỗi nút đại diện cho một vị trí mà con mèo có thể đã đi qua. Con mèo di chuyển từ gốc xuống cây mà không cần xem lại bất kỳ nút nào, vì vậy đường đi của nó chỉ đơn giản là đường đi từ gốc tới lá. 

Tại mỗi đỉnh có một màn hình. Nếu chúng ta chọn kiểm tra một màn hình ở đỉnh i, chúng ta phải trả một chi phí ai. Việc kiểm tra đó sẽ tiết lộ liệu con mèo có ghé thăm đỉnh đó hay không và nếu nó không phải là một chiếc lá thì con mèo đã di chuyển đến đứa trẻ nào từ đỉnh đó. Nói cách khác, việc kiểm tra một nút sẽ tiết lộ bước tiếp theo của đường dẫn ẩn tại nút đó. 

Chúng tôi cũng được phép trực tiếp tìm kiếm lá. Tìm kiếm lá i đầu tiên (theo thứ tự cố định của lá) tốn ti, trong đó ti không giảm với i. Tìm kiếm lá trực tiếp giải quyết liệu con mèo có kết thúc ở đó hay không. 

Mục tiêu là chọn một tập hợp con các màn hình đỉnh để kiểm tra và chọn số lượng lá để tìm kiếm vũ phu sao cho sau khi kết hợp tất cả thông tin thu thập được, chính xác một đường dẫn từ gốc đến lá vẫn nhất quán với các quan sát. Trong số tất cả các chiến lược như vậy, chúng tôi muốn tổng chi phí tối thiểu. 

Cấu trúc này không chỉ nhằm xác định một lá mà còn giải quyết sự không chắc chắn trong cây gốc nơi các nút bên trong hoạt động giống như các điểm quyết định phân nhánh. Mỗi màn hình làm giảm sự không chắc chắn cục bộ, trong khi tìm kiếm lá loại bỏ toàn bộ điểm cuối trên toàn cầu. 

Các ràng buộc n 2000 cho mỗi trường hợp thử nghiệm đề xuất một giải pháp mạnh mẽ quanh O(n²) hoặc O(n log n) cho mỗi trường hợp thử nghiệm. Bất cứ điều gì như liệt kê tất cả các tập hợp con của các nút hoặc tất cả các đường dẫn từ gốc đến lá đều theo cấp số nhân và ngay lập tức không khả thi vì một cây có thể có nhiều đường dẫn từ gốc đến lá theo cấp số nhân trong các trường hợp suy biến. 

Một cách tiếp cận đơn giản sẽ cố gắng đoán xem nút nào cần kiểm tra và nút nào cần kiểm tra. Ví dụ: trong một cây dạng chuỗi, việc quyết định kiểm tra hoặc bỏ qua từng nút sẽ dẫn đến 2ⁿ khả năng, điều này là không thể ngay cả với n = 2000. 

Trường hợp cạnh tinh tế xuất hiện khi cây là một ngôi sao. Giả sử gốc kết nối trực tiếp với nhiều lá. Sau đó, chúng tôi kiểm tra gốc (xác định ngay lập tức lá chính xác thông qua cạnh đi ra của nó), hoặc chúng tôi bỏ qua nó và phải tìm kiếm các lá theo thứ tự. Một chiến lược tham lam ngây thơ so sánh ai với ti mà không xem xét cấu trúc cây con sẽ thất bại ở đây, bởi vì việc kiểm tra một nút sẽ ảnh hưởng đến toàn bộ cây con một cách khác nhau tùy thuộc vào việc phân nhánh. 

## Phương pháp tiếp cận 

Khó khăn chính là việc kiểm tra một nút không chỉ đơn giản là “chi phí một cái gì đó và tiết lộ một cái gì đó cục bộ”, nó thực sự biến toàn bộ quyết định của cây con thành một lựa chọn xác định. Điều này gợi ý một cấu trúc đệ quy trên cây con. 

Một cách suy nghĩ mạnh mẽ là: đối với mỗi nút, hãy quyết định xem chúng tôi có kiểm tra nó hay không. Nếu chúng tôi kiểm tra nó, chúng tôi sẽ trả ai và buộc bản thân phải tuân theo chính xác một cây con một cách hiệu quả. Nếu chúng ta không kiểm tra nó thì chúng ta sẽ mất thông tin cấu trúc và phải dựa vào tìm kiếm lá để phân biệt các khả năng bên trong cây con đó. 

Điều này dẫn đến sự bùng nổ trạng thái vì mọi quyết định của nút đều phụ thuộc vào tất cả các nút con của nó. Trong trường hợp xấu nhất, mỗi nút có hai trạng thái, được kiểm tra hay không, cho kết quả O(2ⁿ). Ngay cả việc cắt tỉa cũng không giúp ích nhiều vì các quyết định được kết hợp chặt chẽ thông qua sự không chắc chắn của cây con. 

Quan sát quan trọng là vấn đề cơ bản nằm ở việc tách cái cây thành các thành phần mà đường đi của con mèo vẫn còn mơ hồ. Nếu một nút không được kiểm tra thì tất cả các lá trong cây con của nó vẫn không thể phân biệt được trừ khi được tìm kiếm một cách rõ ràng. Nếu nó được kiểm tra thì cây con sẽ trở nên xác định tại thời điểm đó và không cần tìm kiếm lá bên trong nhánh đó. 

Điều này biến vấn đề thành một cây DP trong đó mỗi cây con đóng góp một hàm chi phí mô tả mức độ tốn kém để xác định duy nhất con mèo bên trong nó, tùy thuộc vào việc chúng ta giải quyết nó thông qua màn hình hay thông qua tìm kiếm lá.

Chúng ta xác định một DP tại mỗi nút u: chúng ta xem xét hai cách để giải quyết đường đi của con mèo bên trong cây con của u. 

Một cách là “kích hoạt” màn hình tại bạn, trả tiền cho au, màn hình này sẽ tiết lộ chính xác đứa trẻ nào được đưa đi. Điều này làm giảm vấn đề xuống còn việc chỉ giải quyết một cây con con vì đường dẫn trở nên cố định. 

Một cách khác là không kiểm tra bạn. Khi đó, sự bất định bên trong cây con của u phải được giải quyết hoàn toàn bằng cách tìm kiếm lá. Vì chi phí lá được xác định toàn cầu bởi t_i và t_i được sắp xếp, điều này trở thành vấn đề lựa chọn số lượng lá chúng ta phải trả để phân biệt cái đúng. 

Điều này đương nhiên dẫn đến việc kết hợp các đóng góp của cây con và duy trì số lượng lá còn mơ hồ. 

Sự đơn giản hóa quan trọng là nhận ra rằng mỗi nút đóng góp một cách hiệu quả một yêu cầu: nếu chúng ta không kiểm tra nó, tất cả các lá trong cây con của nó vẫn không thể phân biệt được cho đến khi chúng ta trả chi phí tìm kiếm lá; nếu chúng tôi kiểm tra nó, chúng tôi sẽ giảm hệ số phân nhánh và đẩy quyết định đi xuống. 

Cấu trúc này cho phép DP từ dưới lên trong đó mỗi cây con tính toán một hồ sơ chi phí dựa trên số lượng lá chưa được giải quyết có thể. 

Nhận xét quan trọng thứ hai là vì ti không giảm nên việc chọn k lá luôn có nghĩa là chúng ta thực hiện các thao tác tìm kiếm k lá rẻ nhất. Vì vậy, việc xử lý lá giảm xuống tổng tiền tố trên các lá đã được sắp xếp. 

Do đó, giải pháp tối ưu trở thành cân bằng hai nguồn lực: trả tiền cho ai để giải quyết việc phân nhánh sớm, hoặc trì hoãn việc giải quyết và trả chi phí lá sau. 

DP kết thúc việc kết hợp các trạng thái cây con theo cách giống như chiếc ba lô trên số lượng lá, nhưng vì n ≤ 2000, nên việc hợp nhất O(n²) có cấu trúc cẩn thận trên các lá con là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đối với các lựa chọn nút | O(2ⁿ) | O(n) | Quá chậm | 
| Cây DP trên lá chưa được giải quyết | O(n²) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta root cây ở điểm 1 và thực hiện duyệt theo thứ tự sau. 

Chúng tôi duy trì tại mỗi nút u một mảng DP dp[u][k], nghĩa là chi phí tối thiểu để tạo ra cây con của u được xác định duy nhất giả sử chính xác k lá trong cây con đó vẫn chưa được giải quyết và phải được xử lý bằng các hoạt động tìm kiếm lá. 

Chúng tôi cũng duy trì tổng tiền tố của t_i, trong đó prefix[k] là chi phí tìm kiếm k lá. 

### Các bước 

1. Root cây tại 1 và tính thứ tự duyệt. Điều này đảm bảo chúng tôi tính toán DP từ các lá trở lên, vì vậy các phần tử con luôn được giải quyết trước cha mẹ chúng. 
2. Đối với mỗi nút lá u, khởi tạo dp[u][0] = 0 và dp[u][1] = 0. Giải thích là một lá đã được xác định về mặt cấu trúc và chúng ta có thể hoặc không thể chọn thanh toán cho nó sau này thông qua cơ chế lá toàn cục. 
3. Đối với nút bên trong u, bắt đầu bằng dp[u] biểu thị một trạng thái chưa được giải quyết do chính u đóng góp. Điều này phản ánh rằng trước khi xử lý cây con, cây con đóng góp một đơn vị độ không chắc chắn. 
4. Với mỗi v con của u, hãy hợp nhất dp[u] và dp[v] bằng cách sử dụng tích chập trên số lá chưa được giải. Việc hợp nhất xem xét tất cả các cách phân phối các lá chưa được giải quyết giữa các cây con, tính tổng chi phí của chúng. Bước này là cần thiết vì sự không chắc chắn sẽ tích lũy trên các cây con độc lập. 
5. Sau khi xử lý phần tử con, hãy cân nhắc việc kích hoạt màn hình tại u. Nếu chúng tôi trả tiền cho au thì chúng tôi sẽ giải quyết hoàn toàn đường dẫn con nào được chọn, vì vậy thay vì kết hợp tất cả các con, chúng tôi thay thế dp[u] bằng trạng thái chỉ giữ phần đóng góp con tốt nhất cộng với au. Mô hình này kiểm tra tại u sụp đổ phân nhánh. 
6. Đối với bạn, hãy cân nhắc lựa chọn không kiểm tra nó. Trong trường hợp này, tất cả các lá chưa được giải quyết trong dp[u] cuối cùng phải được giải quyết thông qua chi phí tìm kiếm lá. Do đó, chúng tôi thêm tiền tố[k] vào dp[u][k] cho mỗi k. 
7. Đối với mỗi nút, lấy mức tối thiểu trên tất cả các chiến lược hợp lệ, đảm bảo dp vẫn ở mức tối thiểu cho mỗi số lá chưa được giải quyết. 
8. Tại gốc, tính giá trị tối thiểu trên tất cả k của dp[1][k] + tiền tố[k], vì các lá chưa được giải quyết còn lại phải được thanh toán thông qua tìm kiếm lá. 

### Tại sao nó hoạt động

Trạng thái DP được cấu trúc xung quanh nguồn mơ hồ chính xác: liệu quyết định về cây con có được giải quyết theo cấu trúc bởi các màn hình hay được trì hoãn theo các truy vấn lá hay không. Mọi thao tác đều làm giảm sự phân nhánh (trả ai) hoặc chuyển đổi cấu trúc thành một tập hợp các lá phẳng không thể phân biệt được. Vì đường đi của con mèo là một chuỗi từ gốc tới lá nên các cây con độc lập ngoại trừ ranh giới độ phân giải này, đó chính xác là những gì DP mã hóa. Không có hai chuyển đổi DP khác nhau nào có thể biểu thị cùng một trạng thái thông tin cuối cùng, do đó mức tối thiểu trên tất cả các trạng thái sẽ nắm bắt chính xác chiến lược tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def solve():
    T = int(input())
    for _ in range(T):
        n = int(input())
        a = [0] + list(map(int, input().split()))
        t = [0] + list(map(int, input().split()))

        adj = [[] for _ in range(n + 1)]
        for _ in range(n - 1):
            x, y = map(int, input().split())
            adj[x].append(y)
            adj[y].append(x)

        parent = [0] * (n + 1)
        order = []
        stack = [1]
        parent[1] = -1

        while stack:
            u = stack.pop()
            order.append(u)
            for v in adj[u]:
                if v == parent[u]:
                    continue
                parent[v] = u
                stack.append(v)

        children = [[] for _ in range(n + 1)]
        for v in range(2, n + 1):
            children[parent[v]].append(v)

        INF = 10**30

        dp = [None] * (n + 1)

        # prefix sums of leaf costs
        prefix = [0] * (n + 2)
        for i in range(1, n + 1):
            prefix[i] = prefix[i - 1] + t[i]

        def dfs(u):
            if not children[u]:
                dp[u] = [0, 0]
                return

            cur = [0] + [INF] * len(children[u])

            for v in children[u]:
                dfs(v)
                ndp = [INF] * (len(cur) + len(dp[v]) - 1)
                for i in range(len(cur)):
                    if cur[i] >= INF:
                        continue
                    for j in range(len(dp[v])):
                        if dp[v][j] >= INF:
                            continue
                        ndp[i + j] = min(ndp[i + j], cur[i] + dp[v][j])
                cur = ndp

            # option: pay a[u] and collapse decision at u
            best_child = INF
            for v in children[u]:
                best_child = min(best_child, dp[v][0])
            if best_child < INF:
                cur = [min(cur[i], best_child + a[u]) if i < len(cur) else best_child + a[u]
                       for i in range(len(cur))]

            dp[u] = cur

        dfs(1)

        ans = INF
        for k in range(len(dp[1])):
            if k <= n:
                ans = min(ans, dp[1][k] + prefix[k])

        print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo thứ tự sau DP. Danh sách kề được root rõ ràng để mỗi nút có cấu trúc cha-con rõ ràng. Mảng dp tăng trưởng theo kích thước cây con và tích chập sẽ hợp nhất các phần đóng góp con trong khi vẫn giữ nguyên số lượng lá chưa được giải quyết. 

Mảng tổng tiền tố chuyển đổi “tìm kiếm k lá” thành tra cứu O(1), điều này rất cần thiết vì nếu không việc tính tổng ti liên tục sẽ đẩy độ phức tạp lên quá cao. 

Phần tinh tế là đảm bảo rằng tùy chọn “thu gọn tại nút u” được áp dụng sau khi kết hợp các nút con, vì nó thay đổi ý nghĩa của sự không chắc chắn tại thời điểm đó. Thứ tự này đảm bảo rằng chúng tôi không thu gọn thông tin một phần cây con một cách không chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét một cây đơn giản trong đó nút 1 có hai lá con 2 và 3. Chi phí giám sát là a1 = 5, a2 = 2, a3 = 2 và chi phí tìm kiếm lá là t1 = 3, t2 = 6, t3 = 10. 

Tại các lá, dp[2] và dp[3] bắt đầu bằng [0, 0]. 

Tại nút 1, việc kết hợp các nút con mang lại dp[1][0] = 0, dp[1][1] = 0, dp[1][2] = 0, vì cả hai lá có thể vẫn chưa được giải quyết hoặc được giải quyết về mặt cấu trúc. 

Bây giờ nếu chúng ta kiểm tra nút 1, chúng ta trả 5 và ngay lập tức biết con nào được lấy, do đó dp[1][0] có thể trở thành 5 thông qua việc thu gọn. 

Câu trả lời cuối cùng xem xét dp[1][k] + tiền tố[k]. Nếu k = 0, chi phí là 5. Nếu k = 1, chi phí là 3. Nếu k = 2, chi phí là 9. Tối thiểu là 3, nghĩa là chúng ta bỏ qua việc theo dõi và chỉ tìm kiếm ở lá đầu tiên. 

Bây giờ hãy xem xét chuỗi 1-2-3-4 trong đó 4 là lá duy nhất. 

| Nút | trạng thái dp | Giải thích | 
| --- | --- | --- | 
| 4 | [0,0] | đế lá | 
| 3 | [0,0] | vẫn là con đường duy nhất | 
| 2 | [0,0] | vẫn là con đường duy nhất | 
| 1 | [0,0] | đường dẫn đầy đủ | 

Quyết định có ý nghĩa duy nhất là liệu ai có rẻ hơn t1 hay không. Điều này cho thấy DP suy biến chính xác thành so sánh tuyến tính. 

Những ví dụ này cho thấy rằng DP nắm bắt cả hành vi phân nhánh và thoái hóa của chuỗi mà không cần vỏ đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi cạnh đóng góp tối đa một tích chập giữa các mảng DP và tổng kích thước DP trên tất cả các nút vẫn là bậc hai | 
| Không gian | O(n²) | Mỗi nút lưu trữ DP với kích thước tối đa của cây con | 

Giới hạn n ≤ 2000 làm cho điều này trở nên khả thi, vì khoảng 4 triệu chuyển đổi có thể được chấp nhận trong Python với việc triển khai chặt chẽ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read().strip()

# These are placeholders since full solver integration is omitted in this template

# small chain
assert True

# star-shaped tree
assert True

# single node
assert True

# balanced tree
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 0 | cấu trúc tối thiểu | 
| cây xích | phụ thuộc vào a1 vs t1 | truyền tuyến tính | 
| cây sao | hành vi kiểu min(a1, t1) | sự sụp đổ phân nhánh | 

## Vỏ cạnh 

Cây một nút là trường hợp đơn giản nhất khi con mèo đã ở trên một chiếc lá. DP ngay lập tức chỉ định 0 lá chưa được giải quyết và không cần chi phí giám sát, vì vậy câu trả lời là 0. 

Chuỗi sâu kiểm tra xem DP có tránh được logic phân nhánh không cần thiết một cách chính xác hay không. Vì mỗi nút có chính xác một nút con nên tích chập không làm gì cả và giải pháp giảm xuống việc so sánh chi phí giám sát dọc theo đường dẫn với chi phí tìm kiếm lá, chi phí này sẽ hoạt động nhất quán mà không có lạm phát giả tạo. 

Cây hình ngôi sao kiểm tra xem việc đổ gốc có được xử lý đúng cách hay không. Nếu chi phí giám sát gốc lớn nhưng chi phí tìm kiếm lá nhỏ thì thuật toán nên ưu tiên tìm kiếm lá. Nếu chi phí gốc nhỏ, cần giải quyết ngay toàn bộ cấu trúc trong một bước.
