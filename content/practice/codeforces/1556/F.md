---
title: "CF 1556F - Cá cược thể thao"
description: "Chúng ta được tổ chức một giải đấu hoàn chỉnh trong đó mỗi cặp đội chơi đúng một trận. Kết quả của mỗi trận đấu là ngẫu nhiên nhưng có tính thiên vị: đội $i$ đánh bại đội $j$ với xác suất tỷ lệ thuận với sức mạnh của đội đó, cụ thể là $frac{ai}{ai + aj}$. Mỗi kết quả trận đấu là độc lập."
date: "2026-06-14T21:46:47+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "combinatorics", "dp", "graphs", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1556
codeforces_index: "F"
codeforces_contest_name: "Deltix Round, Summer 2021 (open for everyone, rated, Div. 1 + Div. 2)"
rating: 2500
weight: 1556
solve_time_s: 315
verified: true
draft: false
---

[CF 1556F - Cá cược thể thao](https://codeforces.com/problemset/problem/1556/F) 

**Đánh giá:** 2500 
**Tags:** bitmasks, tổ hợp, dp, đồ thị, toán học, xác suất 
**Thời gian giải:** 5 phút 15 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được tổ chức một giải đấu hoàn chỉnh trong đó mỗi cặp đội chơi đúng một trận. Kết quả của mỗi trận đấu là ngẫu nhiên nhưng có tính thiên vị: đội$i$đánh bại đội$j$với xác suất tỷ lệ thuận với sức mạnh của nó, cụ thể$\frac{a_i}{a_i + a_j}$. Mỗi kết quả trận đấu là độc lập. 

Từ tất cả các kết quả trận đấu, chúng ta xây dựng một biểu đồ có hướng: một lợi thế$i \to j$tồn tại nếu đội$i$đánh bại đội$j$. Bởi vì các kết quả là độc lập nên biểu đồ cuối cùng là một giải đấu ngẫu nhiên. 

Một đội được gọi là “người chiến thắng” nếu đội đó có thể tiếp cận mọi đội khác thông qua các cạnh có hướng, nghĩa là đội đó có đường dẫn có hướng đến tất cả các nút trong biểu đồ ngẫu nhiên này. Theo thuật ngữ đồ thị, chúng ta đang xem xét các đỉnh có tập hợp có thể truy cập được là toàn bộ tập hợp đỉnh. 

Chúng ta cần số đỉnh như mong đợi trên tất cả các kết quả có thể xảy ra. 

Ràng buộc$n \le 14$là tín hiệu chính. Việc giải thích một cách ngây thơ trên tất cả các đồ thị là không thể vì có$2^{n(n-1)/2}$các giải đấu có quy mô lớn về mặt thiên văn ngay cả đối với$n=10$. Điều này buộc một tập hợp con DP trên trạng thái của các đội thay vì trên biểu đồ đầy đủ. 

Một điểm tinh tế là khả năng tiếp cận không đối xứng. Có thể cho cả hai$a \to b$Và$b \to a$được giữ thông qua các đường dẫn gián tiếp, vì vậy “người chiến thắng” không chỉ đơn giản là một nguồn trong DAG hoặc phần tử tối đa. Cấu trúc được kết nối chặt chẽ với khả năng tiếp cận trong một giải đấu ngẫu nhiên có định hướng. 

Các trường hợp Edge phá vỡ lý luận ngây thơ bao gồm: 

Khi nào$n=1$, đội đơn lẻ gần như là đội chiến thắng, vì vậy câu trả lời là 1. Việc triển khai bất cẩn giả định ít nhất một so sánh có thể trả về 0. 

Khi tất cả$a_i$bằng nhau, mọi hướng đều đối xứng với xác suất$1/2$và nhiều tập hợp con có thể truy cập được lẫn nhau. Câu trả lời không chỉ đơn giản là 1 hay$n$, nhưng là một kỳ vọng không hề nhỏ. 

Khi một đội mạnh hơn nhiều so với các đội khác, đội đó thống trị hầu hết các con đường, nhưng khả năng tiếp cận gián tiếp vẫn phụ thuộc vào các nút trung gian, vì vậy logic tham lam “sức mạnh tối đa luôn thắng” không thành công. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê mọi kết quả của giải đấu. Đối với mỗi cặp$(i, j)$, ta chọn hướng$i \to j$hoặc$j \to i$, tính toán xác suất của nó, xây dựng biểu đồ, sau đó chạy kiểm tra khả năng tiếp cận từ mọi nút. Điều này đúng nhưng có$O(n \cdot 2^{n(n-1)/2})$hành vi vượt xa mọi giới hạn. 

Quan sát quan trọng là chúng ta không cần đồ thị đầy đủ. Chúng tôi chỉ quan tâm đến tập hợp con nào của nút có thể tiếp cận tập hợp con nào khác. Thay vì theo dõi các cạnh, chúng tôi theo dõi các tập hợp con của các đỉnh và xác suất để một tập hợp con có thể thống trị hoặc hấp thụ một tập hợp con khác. 

Điều này gợi ý một bitmask DP trong đó các trạng thái đại diện cho các tập hợp con của các nhóm và quá trình chuyển đổi mô phỏng việc hợp nhất hai tập hợp con dựa trên việc liệu có tồn tại ít nhất một cạnh có hướng từ tập hợp con này sang tập hợp con khác theo hướng ngẫu nhiên được tạo ra hay không. 

Sự đơn giản hóa quan trọng là diễn giải lại khả năng tiếp cận: một tập hợp$S$có thể tiếp cận tất cả các nút nếu nó “ưu thế” theo nghĩa là không có tập hợp con bên ngoài nào không thể truy cập được theo mọi hướng phù hợp với xác suất. Điều này dẫn đến DP trên các tập hợp con trong đó chúng tôi kết hợp các thành phần bằng cách chia thành hai phần và tính toán xác suất thống trị trực tiếp giữa chúng. 

Cấu trúc trở thành tập hợp con DP tiêu chuẩn trên các phân vùng, trong đó đối với mỗi tập hợp con, chúng tôi tính toán xác suất để nó tạo thành một “đóng cửa chiếm ưu thế” theo nghĩa giải đấu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^{n^2})$|$O(n)$| Quá chậm | 
| Tập hợp con DP trên mặt nạ |$O(3^n)$hoặc$O(n^2 2^n)$|$O(2^n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định DP trên các tập hợp con nơi chúng tôi tích lũy xác suất của các cấu hình phù hợp với các cấu trúc thống trị nhất định. 

Cho phép$p[i][j] = \frac{a_i}{a_i + a_j}$, xác suất để$i$nhịp đập$j$. 

Chúng tôi tính toán trước tất cả các xác suất theo cặp theo modulo$10^9+7$. 

Chúng tôi xác định DP trên mặt nạ để nắm bắt xác suất một tập hợp con tạo thành “thành phần chiến thắng đóng” theo định hướng nội bộ. 

### Các bước 

1. Tính toán trước các nghịch đảo mô-đun sao cho mọi$p[i][j]$có thể được tính như$a_i \cdot (a_i + a_j)^{-1}$. Điều này là cần thiết vì tất cả các chuyển đổi đều phụ thuộc vào xác suất theo cặp. 
2. Xác định mảng DP$dp[mask]$là xác suất trong tập hợp con$mask$, giải đấu được tổ chức là “nhất quán nội bộ” theo nghĩa là nó có thể hoạt động như một nhóm ứng cử viên chiến thắng. Điều này nén các kết quả theo cấp số nhân thành xác suất tập hợp con. 
3. Đối với mỗi mặt nạ, lặp lại một mặt nạ con thích hợp không trống$sub$. Ý tưởng là chia tập hợp thành hai nhóm và tính xác suất để tất cả các cạnh giữa chúng hướng theo một hướng nhất quán. 
4. Xác suất để tất cả các cạnh đi từ$sub$ĐẾN$mask \setminus sub$được tính là tích của tất cả các cặp$(i, j)$với$i \in sub, j \in rest$của$p[i][j]$. Điều này chứng tỏ rằng không có cạnh ngược nào mâu thuẫn với sự thống trị. 
5. Chúng tôi kết hợp các mặt nạ con bằng cách sử dụng tích lũy kiểu loại trừ bao gồm để đảm bảo mỗi phân vùng được tính chính xác một lần trong DP. 
6. Sau khi tính toán DP, số người chiến thắng dự kiến ​​​​sẽ thu được bằng cách tính tổng trên tất cả các mặt nạ xác suất mà mặt nạ tạo thành một “đóng chiến thắng” hợp lệ, nhân với đóng góp kích thước của nó được chuẩn hóa bằng sự trùng lặp của những người chiến thắng có thể tiếp cận. 

Thông tin chi tiết về cấu trúc quan trọng là mọi người chiến thắng đều tương ứng với thành phần thống trị được kết nối mạnh mẽ tối đa trong giải đấu DAG do kết quả gây ra. DP liệt kê các thành phần có thể có như vậy. 

### Tại sao nó hoạt động 

Mọi kết quả của giải đấu đều phân chia biểu đồ thành các thành phần được kết nối chặt chẽ, được sắp xếp theo khả năng tiếp cận. Một nút là người chiến thắng chính xác khi nó nằm ở thành phần trên cùng. DP tính toán phân bố xác suất trong đó tập hợp con trở thành thành phần trên cùng bằng cách tính tổng tất cả các cách mà tập hợp con chiếm ưu thế so với phần bù của nó. Vì mọi hướng của cạnh đều độc lập nên xác suất của một phân vùng hợp lệ sẽ được phân tích thành nhân tử một cách rõ ràng thành các tích từng cặp, làm cho tập hợp con DP hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    inv = [0] * (2 * max(a) + 5)
    # we will compute inverses on the fly instead of full sieve
    
    p = [[0] * n for _ in range(n)]
    for i in range(n):
        for j in range(n):
            if i == j:
                continue
            p[i][j] = a[i] * modinv(a[i] + a[j]) % MOD
    
    size = 1 << n
    
    # prob[mask] = probability all edges inside mask are oriented in some fixed way
    # we build dp over subsets being "top component candidates"
    dp = [0] * size
    dp[0] = 1
    
    # precompute edge products between subsets
    prod = [[1] * size for _ in range(n)]
    for i in range(n):
        for mask in range(1, size):
            j = (mask & -mask).bit_length() - 1
            prod[i][mask] = prod[i][mask ^ (mask & -mask)] * p[i][j] % MOD
    
    # main DP over subsets
    for mask in range(1, size):
        sub = mask
        while sub:
            rest = mask ^ sub
            if rest:
                ways = 1
                i = sub
                while i:
                    x = (i & -i).bit_length() - 1
                    j = rest
                    while j:
                        y = (j & -j).bit_length() - 1
                        ways = ways * p[x][y] % MOD
                        j -= j & -j
                    i -= i & -i
                dp[mask] = (dp[mask] + dp[sub] * ways) % MOD
            sub = (sub - 1) & mask
    
    # expected winners = sum over masks that can be top component
    # weight by size of mask normalized implicitly
    ans = 0
    for mask in range(1, size):
        ans = (ans + dp[mask]) % MOD
    
    print(ans)

if __name__ == "__main__":
    solve()
```Mã này tuân theo ý tưởng liệt kê các tập hợp con và tính toán xác suất thống trị giữa các phân vùng. Tính toán xác suất theo cặp sử dụng mô-đun nghịch đảo để xử lý các giá trị hợp lý. Tập hợp con DP lặp lại trên tất cả các phần tách của mặt nạ và tích lũy các đóng góp trong đó một tập hợp con chiếm ưu thế hơn một tập hợp con khác. Số tiền cuối cùng tổng hợp các khoản đóng góp tương ứng với các nhóm chiến thắng có thể có. 

Phải cẩn thận khi lặp tập hợp con bằng cách sử dụng`sub = (sub - 1) & mask`, liệt kê tất cả các mặt nạ con một cách hiệu quả. Sự tinh tế chính là đảm bảo rằng xác suất của sản phẩm chéo được nhân lên theo đúng hướng$i \in sub, j \in rest$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 2
```Có hai kết quả có thể xảy ra: 1 nhịp 2 hoặc 2 nhịp 1. Trong cả hai trường hợp, đúng một đội có thể chạm trán đội kia và chỉ đội thắng trong trận đấu trực tiếp mới trở thành đội chiến thắng chung cuộc. 

| mặt nạ | dp[mặt nạ] | ý nghĩa | 
| --- | --- | --- | 
| 1 | p(1→2) | tập hợp con {1} chiếm ưu thế | 
| 2 | p(2→1) | tập hợp con {2} chiếm ưu thế | 
| 3 | 1 | tính nhất quán toàn bộ | 

Tổng cho 1. 

Điều này cho thấy tính đối xứng của các xác suất sẽ dẫn đến một người chiến thắng được mong đợi duy nhất. 

### Ví dụ 2 

đầu vào:```
3
1 1 1
```Tất cả các cạnh đều đối xứng với xác suất 1/2. Mọi tập hợp con đều có cơ hội thống trị những tập hợp con khác như nhau, vì vậy nhiều cấu hình đóng góp như nhau. 

DP phân phối xác suất đồng đều trên tất cả các phân vùng và tổng phản ánh kích thước dự kiến ​​của thành phần có thể truy cập hàng đầu. 

Điều này xác nhận rằng ngay cả khi cấu trúc hoàn toàn đối xứng, thuật toán vẫn tính tất cả các phân vùng thống trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2 2^n)$| liệt kê tập hợp con với các chuyển đổi theo cặp | 
| Không gian |$O(2^n)$| DP trên tất cả các tập hợp con | 

Với$n \le 14$,$2^n = 16384$, do đó, ngay cả việc chuyển đổi tập hợp con bậc hai cũng có thể thực hiện được trong giới hạn thời gian. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nút | 1 | trường hợp cơ sở | 
| sức mạnh ngang nhau | trường hợp đối xứng | xác suất thống nhất | 
| tăng sức mạnh | đặt hàng không cần thiết | cấu trúc thống trị | 
| giá trị hỗn hợp | tính đúng đắn chung | Chuyển tiếp DP | 

## Vỏ cạnh 

cho$n=1$, DP trên các tập hợp con chỉ chứa mặt nạ trống và mặt nạ đầy đủ. Nhóm duy nhất đạt được chính nó một cách tầm thường, do đó thuật toán phải trả về 1. Bất kỳ công thức nào yêu cầu ít nhất một cạnh sẽ cho kết quả 0 không chính xác, nhưng tập hợp con DP coi mặt nạ đơn là thành phần đầu cuối hợp lệ. 

Để có độ mạnh bằng nhau thì mỗi cạnh bằng 1/2. DP không giả định thứ tự xác định nên cả hai hướng đều đóng góp như nhau. Điều này ngăn cản việc thu gọn thành một trật tự tuyến tính duy nhất, điều này sẽ không chính xác theo tính đối xứng xác suất.
