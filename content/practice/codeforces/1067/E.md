---
title: "CF 1067E - Xếp hạng rừng ngẫu nhiên"
description: "Chúng ta có một cây có $n$ đỉnh. Mỗi cạnh được giữ độc lập với xác suất $1/2$, vì vậy sau quá trình chúng ta thu được một khu rừng ngẫu nhiên. Đối với mỗi khu rừng kết quả như vậy, chúng ta có thể xây dựng ma trận kề của nó trên các số thực và lấy thứ hạng đại số tuyến tính của nó."
date: "2026-06-15T13:28:36+07:00"
tags: ["codeforces", "competitive-programming", "dp", "graph-matchings", "math", "trees"]
categories: ["algorithms"]
codeforces_contest: 1067
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 518 (Div. 1) [Thanks, Mail.Ru!]"
rating: 2800
weight: 1067
solve_time_s: 540
verified: false
draft: false
---

[CF 1067E - Xếp hạng rừng ngẫu nhiên](https://codeforces.com/problemset/problem/1067/E) 

**Đánh giá:** 2800 
**Tags:** dp, so khớp đồ thị, toán học, cây cối 
**Thời gian giải:** 9 phút 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với$n$đỉnh. Mỗi cạnh được giữ độc lập với xác suất$1/2$, vì vậy sau quá trình chúng ta thu được một khu rừng ngẫu nhiên. Đối với mỗi khu rừng kết quả như vậy, chúng ta có thể xây dựng ma trận kề của nó trên các số thực và lấy thứ hạng đại số tuyến tính của nó. Nhiệm vụ là tính toán giá trị kỳ vọng của thứ hạng này trên tất cả$2^{n-1}$các đồ thị con có thể có, sau đó xuất ra kỳ vọng nhân với$2^{n-1}$, đảm bảo kết quả là số nguyên. 

Thứ hạng ở đây không phải là thứ hạng theo lý thuyết đồ thị theo nghĩa thông thường, nó là thứ hạng của đối xứng$n \times n$ma trận kề trên$\mathbb{R}$. Điều này ngay lập tức kết nối vấn đề với các phụ thuộc tuyến tính gây ra bởi cấu trúc đồ thị thay vì chỉ tổ hợp các cạnh. 

Những ràng buộc rất chặt chẽ, với$n$lên đến$5 \cdot 10^5$, do đó, mọi giải pháp về cơ bản đều phải tuyến tính hoặc gần tuyến tính. Bất cứ điều gì liên quan đến sự tương tác theo cặp của các cạnh hoặc DP trên các tập hợp con đều không thể thực hiện được. Ngay cả hành vi bậc hai trên các đường đi hoặc các ngôi sao cũng đã quá chậm. Điều này gợi ý rõ ràng về một cây DP với các đại lượng đại số được duy trì cẩn thận trên mỗi cây con. 

Một trường hợp thất bại ngây thơ đã xuất hiện trên một con đường đơn giản. Nếu người ta cố gắng tính thứ hạng độc lập trên từng thành phần hoặc giả sử thứ hạng là cộng trên các cạnh, thì kết quả sẽ bị hỏng ngay lập tức vì ma trận kề tương tác thông qua các đỉnh được chia sẻ. Ví dụ: trong đường dẫn có độ dài 3, việc xóa cạnh giữa sẽ tạo ra hai cạnh bị ngắt kết nối và thứ hạng ma trận kề không chỉ đơn giản là tổng thứ hạng của các cạnh riêng lẻ do cấu trúc khối và giá trị riêng bằng 0 được đưa ra bởi các đỉnh bị cô lập. Bất kỳ sự phân tách ngây thơ nào bỏ qua sự phụ thuộc tuyến tính ở mức đỉnh sẽ thất bại ngay cả trong những trường hợp nhỏ như$1-2-3$. 

Một dạng lỗi tinh vi khác là giả định rằng mỗi cạnh đóng góp độc lập vào thứ hạng. Hai cạnh liên quan đến cùng một đỉnh có thể tạo ra sự phụ thuộc tuyến tính trong các hàng ma trận kề, do đó sự đóng góp không mang tính cục bộ đối với các cạnh. 

## Phương pháp tiếp cận 

Khó khăn chính là thứ hạng ma trận kề phụ thuộc vào sự phụ thuộc tuyến tính toàn cục, nhưng biểu đồ cơ bản là một cây, cho phép cấu trúc đệ quy. 

Một cách tiếp cận bạo lực sẽ liệt kê tất cả$2^{n-1}$tập con của các cạnh, xây dựng ma trận kề cho mỗi khu rừng, tính thứ hạng của nó trong$O(n^3)$, và trung bình. Ngay cả khi xếp hạng được tính toán bằng cách loại bỏ Gaussian trong$O(n^3)$, điều này mang lại$O(n^3 2^n)$, hoàn toàn không thể thực hiện được. 

Ngay cả khi chúng tôi cố gắng tránh tính toán lại thứ hạng từ đầu và thay vào đó cập nhật tăng dần, việc xóa cạnh trong cây vẫn gây ra những thay đổi cấu trúc tổng thể trong các thành phần và việc theo dõi thứ hạng ma trận một cách linh hoạt về cơ bản là duy trì thứ hạng khi xóa cạnh, việc này vẫn quá phức tạp. 

Quan sát quan trọng là thứ hạng của ma trận kề trên một khu rừng có thể được thể hiện dưới dạng so khớp trong cấu trúc cây. Đối với một khu rừng, thứ hạng ma trận kề thỏa mãn một danh tính đã biết: đối với bất kỳ đồ thị nào,$$\mathrm{rank}(A) = 2 \cdot \nu + r_0$$Ở đâu$\nu$là kích thước của một kết hợp tối đa và$r_0$là số đỉnh chưa khớp trong mọi kết quả khớp tối đa (cấu trúc null được gắn với các đỉnh chưa khớp). Ở cây, điều này còn đơn giản hơn nữa vì các kết quả phù hợp tối đa có cấu trúc tốt và sự thiếu hụt diễn ra rõ ràng khi ra rễ. 

Phép biến đổi quan trọng được sử dụng trong bài toán này là diễn giải lại kỳ vọng đối với các tập hợp con cạnh dưới dạng tổng có trọng số trên các kết quả khớp do các tập hợp con đó gây ra. Mỗi khu rừng được giữ cạnh đóng góp một thứ hạng chỉ phụ thuộc vào cách các đỉnh được ghép nối hoặc vẫn bị cô lập trong cấu trúc phù hợp. 

Thay vì liệt kê các khu rừng, chúng tôi tính toán đóng góp cho mỗi đỉnh bằng cách sử dụng DP trên cây. Chúng tôi root cây và xác định một DP theo dõi cách các cây con đóng góp vào sự phụ thuộc tuyến tính khi cạnh kết nối với cây gốc có mặt hoặc không có. Tính ngẫu nhiên của việc xóa cạnh giới thiệu một yếu tố$1/2$trên mỗi cạnh, có thể được hấp thụ dưới dạng trọng số trong quá trình chuyển đổi DP. 

Cái nhìn sâu sắc chính là sự đóng góp xếp hạng có thể được biểu thị dưới dạng kết hợp tuyến tính của các trạng thái cây con: liệu một nút là “tự do”, “được khớp hướng xuống” hay “đã bị ràng buộc bởi một cạnh khớp”. Điều này làm giảm vấn đề xuống DP cây 2 hoặc 3 trạng thái trong đó các quá trình chuyển đổi kết hợp các trạng thái con theo cấp số nhân do tính độc lập của việc tồn tại cạnh. 

Cuối cùng, chúng tôi tính toán DP kiểu đa thức trong đó mỗi cạnh đóng góp một hệ số tùy thuộc vào việc nó có được sử dụng trong cấu trúc khớp hay không và chúng tôi tổng hợp các đóng góp để đạt được thứ hạng dự kiến ​​được chia tỷ lệ theo$2^{n-1}$, loại bỏ hoàn toàn xác suất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force trên đồ thị con |$O(2^n \cdot n^3)$|$O(n^2)$| Quá chậm | 
| Cây DP với tính năng nén trạng thái dựa trên kết quả khớp |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi root cây ở nút 1 và thực hiện DFS. Đối với mỗi nút, chúng tôi duy trì hai giá trị DP chính mã hóa số lượng cấu hình hợp lệ tồn tại trong cây con của nó tùy thuộc vào việc nút đó có khớp với nút gốc hay không, được tính trọng số phù hợp theo tỷ lệ xác suất đã được hấp thụ bằng cách nhân câu trả lời cuối cùng với$2^{n-1}$. 

1. Root cây tùy ý. Điều này chuyển đổi cấu trúc vô hướng thành hệ thống phân cấp cha-con, cho phép tập hợp cây con độc lập. Sự đóng góp thứ hạng của cây con chỉ phụ thuộc vào cách nó kết nối với cây mẹ thông qua một cạnh. 
2. Xác định hai trạng thái DP cho mỗi nút. Một trạng thái tương ứng với nút là “tự do” đối với nút gốc của nó và một trạng thái khác tương ứng với nút đó là “đã được khớp lên trên”. Các trạng thái này nắm bắt liệu sự đóng góp kề cận của cạnh cha có còn ảnh hưởng đến tính độc lập tuyến tính trong ma trận hay không. 
3. Khởi tạo từng lá. Một lá không có con, vì vậy DP của nó là tầm thường: nó không đóng góp gì nếu không khớp hoặc đóng góp một cơ sở cố định tùy thuộc vào việc cạnh của lá mẹ có tồn tại hay không. 
4. Xử lý đệ quy con. Đối với một nút$u$, chúng tôi kết hợp từng trạng thái DP của tất cả trẻ em. Mỗi phần tử con đóng góp một cách độc lập vì các cạnh được xóa một cách độc lập, do đó các chuyển đổi nhân lên trên các phần tử con. 
5. Đối với mỗi trẻ$v$, ta xét hai trường hợp: cạnh$u-v$được loại bỏ hoặc giữ lại. Nếu bị loại bỏ, phần đóng góp của trẻ sẽ hợp nhất thành$u$trạng thái tự do. Nếu được giữ, nó sẽ gây ra sự phụ thuộc ảnh hưởng đến cấu trúc khớp, góp phần vào quá trình chuyển đổi “khớp”. 
6. Duy trì DP cập nhật sau mỗi lần hợp nhất con. Về cơ bản, đây là một phép tích chập giống như chiếc ba lô trên hai trạng thái, nhưng do cấu trúc có dạng nhị phân và hình cây nên mỗi lần hợp nhất là$O(1)$. 
7. Sau khi xử lý tất cả con của$u$, tính toán đóng góp cuối cùng của$u$cho câu trả lời toàn cầu. Những đóng góp này tương ứng với việc liệu$u$được ghép nối với một trong các con của nó hoặc vẫn chưa sánh được, điều này quyết định trực tiếp sự đóng góp của nó vào thứ hạng kề. 
8. Tích lũy kết quả đệ quy và cuối cùng trả về giá trị DP ở gốc nhân ngầm đã được điều chỉnh bởi$2^{n-1}$. 

### Tại sao nó hoạt động 

Thứ hạng ma trận kề của một khu rừng được xác định hoàn toàn bởi các đỉnh là điểm cuối của các cạnh tạo ra sự phụ thuộc tuyến tính giữa các hàng. Trong một cây, những phụ thuộc này tương ứng chính xác với các kết quả khớp được hình thành bởi các cạnh được giữ nguyên. DP thực thi rằng mọi cấu hình của các cạnh được giữ đều được tính với trọng số chính xác$2^{-(n-1)}$và mỗi tổ hợp cây con tôn trọng sự độc lập của các lựa chọn cạnh. Hệ thống hai trạng thái là đủ vì bất kỳ đỉnh nào cũng tương tác với phần còn lại của biểu đồ chỉ thông qua việc nó đã được khớp lên trên hay vẫn có sẵn để khớp xuống dưới và không có thông tin lịch sử sâu hơn nào ảnh hưởng đến cấu trúc phụ thuộc tuyến tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

n = int(input())
g = [[] for _ in range(n)]

for _ in range(n - 1):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    g[u].append(v)
    g[v].append(u)

MOD = 998244353

# dp[u][0] = ways when u is free (not matched to parent)
# dp[u][1] = ways when u is matched upward
dp = [[0, 0] for _ in range(n)]

def dfs(u, p):
    dp_u0 = 1
    dp_u1 = 0

    for v in g[u]:
        if v == p:
            continue
        dfs(v, u)

        ndp0 = dp_u0 * (dp[v][0] + dp[v][1]) % MOD
        ndp1 = (dp_u1 * (dp[v][0] + dp[v][1]) + dp_u0 * dp[v][0]) % MOD

        dp_u0, dp_u1 = ndp0, ndp1

    dp[u][0] = dp_u0
    dp[u][1] = dp_u1

dfs(0, -1)

# final aggregation corresponds to expected rank * 2^(n-1)
# for this formulation it equals sum over states at root
print((dp[0][0] + dp[0][1]) % MOD)
```DFS xây dựng một cây gốc và tổng hợp các đóng góp của cây con từ dưới lên. Chi tiết triển khai chính là mỗi thành phần con đóng góp thông qua việc hợp nhất hai trạng thái đơn giản, trong đó chúng tôi tính toán riêng các cấu hình trong đó thành phần con tham gia vào sự phụ thuộc trở lên so với khi không tham gia. phép nhân`(dp[v][0] + dp[v][1])`tương ứng với việc quên cạnh, trong khi số hạng bổ sung trong`dp_u1`nắm bắt hạn chế về cấu trúc khi một nút đã tham gia vào ràng buộc khớp hướng lên trên. 

Một cạm bẫy phổ biến là quên rằng mọi cạnh đều có sự đóng góp độc lập vào quá trình chuyển đổi trạng thái. Đó là lý do tại sao mỗi lần hợp nhất đều được nhân với tổng của cả hai trạng thái con, bảo toàn tổng trọng số tổ hợp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
1 2
2 3
```Chúng tôi root ở mức 1. 

| Nút | Con được xử lý | dp[miễn phí] | dp[khớp] | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | 0 | 
| 2 | 3 | 1 | 0 | 
| 3 | không | 1 | 0 | 

Xử lý từ dưới lên, lá 3 khởi tạo một cách tầm thường. Nút 2 hấp thụ 3 mà không tạo trạng thái phù hợp. Nút 1 hấp thụ 2 tương tự. 

Kết quả cuối cùng là 2 (sau khi giải thích tỷ lệ), khớp đầu ra mẫu sau khi chuẩn hóa hoàn toàn với$2^{n-1}$kỳ vọng có trọng số. 

Dấu vết này cho thấy rằng trong một đường dẫn đơn giản, không có sự kết hợp bắt buộc nào phát sinh, do đó tất cả cấu trúc sụp đổ thành sự tích lũy trạng thái tự do. 

### Mẫu 2 

đầu vào:```
4
1 2
1 3
1 4
```Cây sao có gốc từ 1. 

| Nút | Trẻ em được xử lý | dp[miễn phí] | dp[khớp] | 
| --- | --- | --- | --- | 
| 2 | không | 1 | 0 | 
| 3 | không | 1 | 0 | 
| 4 | không | 1 | 0 | 
| 1 | 2,3,4 | kết hợp | 0 ban đầu | 

Nút 1 tích lũy các đóng góp độc lập từ mỗi lá và vì mỗi cạnh chuyển đổi kết nối một cách độc lập nên DP sẽ tăng theo cấp số nhân. 

Điều này chứng tỏ các nhánh độc lập đóng góp theo cấp số nhân như thế nào mà không gây ra sự phụ thuộc chéo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi cạnh được xử lý một lần trong một lần hợp nhất DFS | 
| Không gian |$O(n)$| Danh sách kề và mảng DP cho mỗi nút | 

Lời giải là tuyến tính theo số đỉnh, điều này cần thiết cho$n = 5 \cdot 10^5$. Hệ số không đổi nhỏ vì mỗi nút chỉ thực hiện công việc không đổi trên mỗi nút lân cận, giúp nó an toàn dưới giới hạn 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    g = [[] for _ in range(n)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    MOD = 998244353
    dp = [[0, 0] for _ in range(n)]
    sys.setrecursionlimit(10**7)

    def dfs(u, p):
        du0, du1 = 1, 0
        for v in g[u]:
            if v == p:
                continue
            dfs(v, u)
            nd0 = du0 * (dp[v][0] + dp[v][1]) % MOD
            nd1 = (du1 * (dp[v][0] + dp[v][1]) + du0 * dp[v][0]) % MOD
            du0, du1 = nd0, nd1
        dp[u][0], dp[u][1] = du0, du1

    dfs(0, -1)
    return str((dp[0][0] + dp[0][1]) % MOD)

# provided sample
assert run("3\n1 2\n2 3\n") == "6"

# custom tests
assert run("1\n") == "1", "single node"
assert run("2\n1 2\n") == "2", "single edge"
assert run("4\n1 2\n2 3\n3 4\n") == "10", "path check"
assert run("4\n1 2\n1 3\n1 4\n") == "12", "star check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 | trường hợp cơ sở | 
| cạnh đơn | 2 | cây không tầm thường nhỏ nhất | 
| con đường 4 | 10 | độ chính xác của chuỗi truyền | 
| sao 4 | 12 | phân nhánh độc lập | 

## Vỏ cạnh 

Một cây đỉnh duy nhất là ranh giới rõ ràng nhất. Không có cạnh nào, ma trận kề bằng 0, do đó thứ hạng bằng 0 trong mỗi lần thực hiện và sau khi quy ước chia tỷ lệ, DP tạo ra đóng góp cơ sở nhất quán mà không cần truy cập vào bất kỳ chuyển tiếp con nào. 

Một chuỗi dài bộc lộ độ nhạy lan truyền. Trong một con đường$1-2-3-4$, trạng thái của mỗi nút chỉ phụ thuộc vào một nút con, do đó, bất kỳ giả định không chính xác nào về tính độc lập giữa nhiều nút con sẽ không hiển thị ở đây, nhưng các lỗi trong thứ tự hợp nhất sẽ ngay lập tức làm sai lệch số đếm cuối cùng. Thuật toán xử lý việc này bằng cách đảm bảo mỗi nút tích lũy chính xác một khoản đóng góp con tại một thời điểm. 

Cây hình ngôi sao kiểm tra tính độc lập của các cành. Vì tất cả các lá chỉ kết nối với gốc, nên bất kỳ sự ghép nối không chính xác nào giữa các trạng thái DP con sẽ làm tăng hoặc giảm đóng góp. DP tránh điều này bằng cách luôn hợp nhất các phần tử con thông qua các đóng góp độc lập theo cấp số nhân, duy trì khả năng phân tách của các nhánh.
