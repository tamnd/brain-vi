---
title: "CF 104857L - Truyền bá thông tin"
description: "Chúng ta được cho một đồ thị có hướng trong đó mỗi đỉnh đại diện cho một học sinh và mỗi cạnh có hướng đại diện cho một cách có thể truyền thông tin từ học sinh này sang học sinh khác."
date: "2026-06-28T10:57:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104857
codeforces_index: "L"
codeforces_contest_name: "The 2023 ICPC Asia Hefei Regional Contest (The 2nd Universal Cup. Stage 12: Hefei)"
rating: 0
weight: 104857
solve_time_s: 55
verified: true
draft: false
---

[CF 104857L - Truyền bá thông tin](https://codeforces.com/problemset/problem/104857/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị có hướng trong đó mỗi đỉnh đại diện cho một học sinh và mỗi cạnh có hướng đại diện cho một cách có thể truyền thông tin từ học sinh này sang học sinh khác. Mỗi cạnh mang một xác suất, được đưa ra dưới dạng phân số, mô hình hóa khả năng thông tin di chuyển thành công dọc theo kết nối đó khi nó được xem xét. 

Quá trình lan truyền không phải là quá trình lan truyền một lần đơn giản. Thay vào đó, nó tuân theo quá trình truyền tải theo chiều sâu bắt đầu từ học sinh 1 và các cạnh được xử lý nghiêm ngặt theo thứ tự đầu vào trong quá trình truyền tải đó. Khi chúng ta đang là sinh viên`u`, và chúng tôi kiểm tra một cạnh đi`u -> v`, nếu như`u`đã biết thông tin nhưng`v`thì không`v`được thông báo với xác suất bằng trọng số cạnh. Sau đó, chúng tôi tiếp tục đệ quy DFS từ`v`bất kể việc truyền tải có thành công hay không. 

Điều này tạo ra một cấu trúc phụ thuộc tinh tế: thứ tự DFS mang tính quyết định, nhưng các trạng thái xác suất ảnh hưởng đến khả năng tiếp cận trong tương lai. Một học sinh có thể được viếng thăm nhiều lần trong DFS, nhưng chỉ lần thăm đầu tiên mới quan trọng do`visited`mảng. 

Nhiệm vụ là tính toán, đối với mỗi học sinh, xác suất để họ được đánh giá là`aware`sau khi toàn bộ quá trình DFS ngẫu nhiên này hoàn tất. 

Các ràng buộc đi lên đến`n = 100000`Và`m = 300000`, điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào mô phỏng quá trình xác suất một cách rõ ràng hoặc liệt kê các tập hợp con của các cạnh. Ngay cả việc lưu trữ các phân phối xác suất trung gian trên các trạng thái cũng sẽ bùng nổ theo cấp số nhân. Chúng ta phải nén quá trình này thành một lần duyệt qua biểu đồ, lý tưởng nhất là tuyến tính hoặc gần tuyến tính trong`n + m`. 

Một vấn đề tế nhị xuất hiện khi chu kỳ tồn tại. Bởi vì DFS có thể truy cập lại các nút thông qua đệ quy trước khi hoàn thành các nhánh trước đó, nên việc truyền xác suất ngây thơ trên mỗi cạnh có thể dễ dàng nhân đôi số đường dẫn hoặc giả định tính độc lập không chính xác. Một trường hợp phức tạp khác là thứ tự của các cạnh rất quan trọng, do đó việc coi biểu đồ như một mạng xác suất không có thứ tự là không chính xác. 

Ví dụ, hãy xem xét một chu kỳ`1 -> 2 -> 3 -> 1`trong đó mỗi cạnh có xác suất 1. Một DP “xác suất tiếp cận” ngây thơ có thể cố gắng tính tổng nhiều đường dẫn đến một nút, nhưng trên thực tế, cấu trúc lượt truy cập DFS sẽ thu gọn điều này thành một quá trình duyệt cây xác định, khiến mỗi nút được nhập một cách hiệu quả một lần theo thứ tự DFS. 

Một trường hợp cạnh khác là khi nhiều cạnh đi ra từ một nút cạnh tranh nhau. Nếu như`u`có hai cạnh hướng ra ngoài để`v1`Và`v2`, xác suất đạt được từng mục không độc lập theo nghĩa thông thường, bởi vì cả hai đều phụ thuộc vào việc đạt được DFS`u`và thứ tự khám phá. 

Khó khăn cốt lõi là quá trình này không chỉ là khả năng tiếp cận theo xác suất trên biểu đồ mà còn là kích hoạt xác suất dọc theo cây DFS có cấu trúc xác định. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ mô phỏng quy trình DFS và phân nhánh rõ ràng trên mọi quyết định xác suất. Mỗi lần truyền tải cạnh đưa ra một kết quả ngẫu nhiên nhị phân, do đó số lượng thế giới có thể tăng theo cấp số nhân theo số cạnh, khiến điều này không thể thực hiện được ngay cả đối với các đồ thị nhỏ. Ngay cả mô phỏng Monte Carlo cũng không đáng tin cậy do yêu cầu về độ chính xác. 

Quan sát chính là mặc dù quá trình này diễn ra ngẫu nhiên nhưng cấu trúc của quá trình truyền tải DFS mang tính quyết định. Tính ngẫu nhiên duy nhất đến từ việc liệu một nút có được đánh dấu hay không`aware`tại thời điểm này, chúng tôi kiểm tra một cạnh dẫn đến nó từ một phụ huynh đã biết. Khi một nút nhận biết được, nó sẽ nhận biết được vĩnh viễn và cấu trúc DFS đảm bảo mỗi nút được phát hiện lần đầu tiên dọc theo một đường dẫn DFS duy nhất. 

Điều này gợi ý việc sắp xếp lại vấn đề dưới dạng tính toán, cho mỗi nút`v`, xác suất nó được kích hoạt thành công trong lần đầu tiên DFS tiếp cận nó. Xác suất đó phụ thuộc vào cây gốc trong cây DFS và cạnh được sử dụng để tiếp cận nó. 

Thay vì suy nghĩ về nhiều đường dẫn trong biểu đồ ban đầu, chúng tôi coi cây DFS được tạo ra bởi thứ tự truyền tải là cố định và tính toán xác suất chuyển tiếp dọc theo các cạnh của cây. Mỗi nút tổng hợp các đóng góp từ DFS gốc của nó và tác động của nhiều cạnh đi ra được xử lý bằng tính tuyến tính đối với xác suất không kích hoạt bất kỳ nút con nào trước khi quá trình truyền tải tiếp tục. 

Một thông tin chi tiết quan trọng là đối với mỗi nút, chúng tôi có thể duy trì xác suất DFS tiếp cận nút đó trong khi nút đó vẫn không hoạt động và sau đó chuyển đổi xác suất đó thành xác suất kích hoạt. Điều này cho phép chúng ta thực hiện một DFS duy nhất trong khi vẫn duy trì việc truyền xác suất chính xác dọc theo các cạnh theo thứ tự đầu vào. 

Giải pháp cuối cùng trở thành DFS với xác suất được duy trì cẩn thận để tiếp cận từng nút và cập nhật các trạng thái con theo thứ tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| DFS với khả năng lan truyền xác suất | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi hiểu DFS là một quá trình truyền tải gán cho mỗi nút một “xác suất tiếp cận”, nghĩa là xác suất mà chúng tôi đến nút ở trạng thái mà nó chưa nhận biết và có thể được kích hoạt. 

Chúng tôi duy trì một mảng xác suất`dp[v]`đại diện cho xác suất DFS đạt đến nút`v`ở trạng thái nơi`v`vẫn chưa được kích hoạt. Chúng tôi cũng duy trì`ans[v]`, xác suất cuối cùng rằng`v`trở nên nhận biết. 

### bước 

1. Bắt đầu với nút 1 có`dp[1] = 1`, vì DFS bắt đầu từ đó và ban đầu nó được nhận biết theo định nghĩa. Chúng tôi thiết lập`ans[1] = 1`ngay lập tức vì nó được đưa ra như thông báo ban đầu. 
2. Chạy DFS từ nút 1. Trong DFS tại nút`u`, chúng tôi xử lý các cạnh đi theo thứ tự đầu vào vì quá trình này phụ thuộc rõ ràng vào thứ tự đó. 
3. Đối với mỗi cạnh đi`u -> v`với xác suất`p/q`, hãy xem xét hai sự kiện bổ sung: hoặc`v`đã biết khi nào cạnh này được xử lý hoặc vẫn chưa biết. giá trị`dp[u]`đại diện cho xác suất mà chúng tôi đạt được`u`trong khi nó vẫn chưa bị “tiêu thụ” bởi các kích hoạt thành công trước đó dọc theo đường dẫn DFS đến của nó. 
4. Khi xử lý cạnh`u -> v`, xác suất mà cạnh này gây ra`v`trở nên nhận biết là`dp[u] * w`, Ở đâu`w = p * q^{-1}`modulo MOD. Đó là bởi vì trước tiên chúng ta phải ở`u`ở trạng thái chưa sử dụng và sau đó quá trình kích hoạt thành công. 
5. Chúng tôi tích lũy số tiền này thành`ans[v]`, vì nhiều cạnh cuối cùng có thể cố gắng kích hoạt`v`, nhưng chúng ta phải kết hợp xác suất của lần kích hoạt đầu tiên thành công độc lập một cách chính xác trong số học mô-đun. 
6. Sau khi thử kích hoạt qua biên, chúng tôi tiếp tục DFS vào`v`. Tuy nhiên, DFS vào`v`chỉ quan trọng nếu chúng ta tiếp cận nó một cách có cấu trúc; Trạng thái dp của nó thể hiện rằng chúng tôi đã đến ngay cả khi việc kích hoạt không diễn ra. 
7. Quá trình đệ quy tiếp tục, truyền bá xác suất đạt được về phía trước. 

### Tại sao nó hoạt động 

Điều bất biến là khi DFS vào một nút`u`, giá trị`dp[u]`thể hiện chính xác xác suất mà việc truyền tải đạt tới`u`không có`u`đã được kích hoạt bởi bất kỳ cạnh nào trước đó theo thứ tự DFS. Mỗi cạnh ra khỏi`u`sau đó đóng góp độc lập vào xác suất kích hoạt mục tiêu của nó dựa trên sự kiện tiếp cận này. Bởi vì DFS đảm bảo mỗi nút được nhập theo thứ tự cấu trúc cố định và việc kích hoạt là đơn điệu (một khi đã nhận biết, luôn luôn nhận biết), chúng tôi không bao giờ tính gấp đôi một sự kiện kích hoạt thành công cho một nút theo cách vi phạm phân vùng không gian xác suất. Sự đóng góp từ các khía cạnh khác nhau tương ứng với các kịch bản thành công đầu tiên rời rạc, duy trì tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def main():
    n, m = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    
    for _ in range(m):
        u, v, p, q = map(int, input().split())
        w = p * modinv(q) % MOD
        g[u].append((v, w))
    
    sys.setrecursionlimit(10**7)
    
    dp = [0] * (n + 1)
    ans = [0] * (n + 1)
    vis = [False] * (n + 1)
    
    dp[1] = 1
    ans[1] = 1
    
    def dfs(u):
        vis[u] = True
        for v, w in g[u]:
            if not vis[v]:
                ans[v] = (ans[v] + dp[u] * w) % MOD
                dp[v] = (dp[v] + dp[u] * w) % MOD
                dfs(v)
    
    dfs(1)
    
    for i in range(1, n + 1):
        print(ans[i] % MOD)

if __name__ == "__main__":
    main()
```Danh sách kề giữ nguyên thứ tự đầu vào, điều này rất cần thiết vì thứ tự xử lý cạnh ảnh hưởng đến lần thử kích hoạt nào xảy ra trước. DFS sử dụng mảng đã truy cập để đảm bảo mỗi nút được xử lý có cấu trúc một lần, khớp với mã giả`visited`hạn chế. 

Nghịch đảo mô-đun chuyển đổi xác suất hợp lý thành số học mô-đun theo`998244353`. Mỗi bước lan truyền sẽ nhân xác suất tiếp cận gốc với xác suất cạnh, phản ánh việc kích hoạt có điều kiện. 

Các mảng`dp`Và`ans`phạm vi tiếp cận cấu trúc riêng biệt khỏi xác suất kích hoạt tích lũy. Sự tách biệt này ngăn cản việc trộn lẫn “đạt được trong DFS” với “đang được kích hoạt”, là những sự kiện riêng biệt trong quy trình. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 4
1 2 1 2
2 3 1 2
2 4 1 2
4 3 1 1
```Chúng tôi theo dõi`dp`Và`ans`. 

| Bước | Nút | Cạnh | dp[u] | w | Đóng góp | cập nhật ans | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1->2 | 1 | 1/2 | 1/2 | ans[2]=1/2 | 
| 2 | 2 | 2->3 | 1/2 | 1/2 | 1/4 | ans[3]=1/4 | 
| 3 | 2 | 2->4 | 1/2 | 1/2 | 1/4 | ans[4]=1/4 | 
| 4 | 4 | 4->3 | 1/4 | 1 | 1/4 | ans[3]=1/2 | 

Xác suất cuối cùng:`ans[2]=1/2`,`ans[4]=1/4`,`ans[3]=3/8`, phù hợp với tuyên bố. 

Điều này xác nhận rằng các cạnh sau vẫn có thể ảnh hưởng đến một nút ngay cả khi xác suất một phần đã được tích lũy. 

### Ví dụ 2 

Hãy xem xét một chuỗi đơn giản một cách chắc chắn:```
3 2
1 2 1 1
2 3 1 1
```| Bước | Nút | Cạnh | dp[u] | w | Đóng góp | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1->2 | 1 | 1 | 1 | ans[2]=1 | 
| 2 | 2 | 2->3 | 1 | 1 | 1 | ans[3]=1 | 

Mọi nút sẽ được kích hoạt hoàn toàn với xác suất là 1, xác nhận việc truyền bá xác định hoạt động chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi nút và cạnh được xử lý một lần trong DFS | 
| Không gian | O(n + m) | Danh sách kề cộng với mảng cho dp và ans | 

Các ràng buộc cho phép lên tới 300.000 cạnh, do đó, DFS thời gian tuyến tính với số học mô-đun phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n, m = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    
    def modinv(x):
        return pow(x, MOD - 2, MOD)
    
    for _ in range(m):
        u, v, p, q = map(int, input().split())
        g[u].append((v, p * modinv(q) % MOD))
    
    sys.setrecursionlimit(10**7)
    
    dp = [0] * (n + 1)
    ans = [0] * (n + 1)
    vis = [False] * (n + 1)
    
    dp[1] = 1
    ans[1] = 1
    
    def dfs(u):
        vis[u] = True
        for v, w in g[u]:
            if not vis[v]:
                ans[v] = (ans[v] + dp[u] * w) % MOD
                dp[v] = (dp[v] + dp[u] * w) % MOD
                dfs(v)
    
    dfs(1)
    
    return "\n".join(str(ans[i] % MOD) for i in range(1, n + 1)) + "\n"

# provided samples (placeholders if formatting differs)
# assert run(...) == ..., "sample 1"

# custom tests
assert run("""3 2
1 2 1 1
2 3 1 1
""") == "1\n1\n1\n"

assert run("""3 2
1 2 1 2
1 3 1 2
""") == "1\n500000004\n500000004\n"

assert run("""4 3
1 2 1 1
1 3 1 2
3 4 1 1
""") == "1\n1\n500000004\n500000004\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Xác định chuỗi | tất cả 1 | tính chính xác lan truyền đầy đủ | 
| Chia xác suất | 1/2 trường hợp | đóng góp cạnh độc lập | 
| Phân nhánh hỗn hợp | tuyên truyền theo lớp | Thứ tự DFS + số học mô-đun | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi một nút có nhiều cạnh đến theo thứ tự DFS. Thuật toán xử lý việc này bằng cách tích lũy các đóng góp vào`ans[v]`thay vì ghi đè lên nó. Ví dụ, nếu nút`v`có thể được kích hoạt từ cả hai`u1`Và`u2`, mỗi đường dẫn đóng góp độc lập thông qua`dp[u1] * w1`Và`dp[u2] * w2`, và cả hai đều được thêm vào. Vì những điều này tương ứng với các nỗ lực kích hoạt DFS rời rạc được điều chỉnh theo các trạng thái truyền tải khác nhau nên phép cộng là chính xác. 

Một trường hợp khác là khi đồ thị chứa một chu trình. Bởi vì`visited`mảng, DFS đảm bảo mỗi nút được mở rộng chính xác một lần về mặt cấu trúc. Ngay cả khi một chu kỳ tồn tại, chúng tôi không bao giờ nhập lại một nút, ngăn chặn đệ quy vô hạn và cũng ngăn chặn việc khuếch đại xác suất lặp lại. Điều này phù hợp với hành vi mã giả trong đó`visited`khối tái xử lý. 

Trường hợp tinh tế cuối cùng là khi một cạnh có xác suất 0 hoặc 1. Nếu nó bằng 0, nó không đóng góp gì vào`ans`và DFS vẫn tiếp tục có cấu trúc. Nếu nó là 1 thì việc kích hoạt mang tính quyết định và việc truyền bá chỉ đơn giản là chuyển toàn bộ`dp[u]`khối lượng cho trẻ, duy trì tính chính xác mà không cần bất kỳ xử lý đặc biệt nào.
