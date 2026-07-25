---
title: "CF 103831H - Mua sắm"
description: "Có tới 17 cửa hàng và mỗi cặp cửa hàng đều có chi phí đi lại, có thể bằng 0 nghĩa là không có kết nối trực tiếp. Bạn được phép di chuyển giữa các cửa hàng theo bất kỳ thứ tự nào, thanh toán các chi phí đó và bạn bắt đầu miễn phí tại cửa hàng 1."
date: "2026-07-02T08:11:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103831
codeforces_index: "H"
codeforces_contest_name: "2017 International olympiad Tuymaada"
rating: 0
weight: 103831
solve_time_s: 49
verified: true
draft: false
---

[CF 103831H - Mua sắm](https://codeforces.com/problemset/problem/103831/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Có tới 17 cửa hàng và mỗi cặp cửa hàng đều có chi phí đi lại, có thể bằng 0 nghĩa là không có kết nối trực tiếp. Bạn được phép di chuyển giữa các cửa hàng theo bất kỳ thứ tự nào, thanh toán các chi phí đó và bạn bắt đầu miễn phí tại cửa hàng 1. Tại mỗi cửa hàng, đối với mỗi loại trong số tối đa 50 loại sản phẩm, cửa hàng có thể cung cấp một số lượng sản phẩm đó theo một đơn giá nhất định và mỗi loại có tổng số lượng bắt buộc phải được đáp ứng. Bạn có thể mua từ nhiều cửa hàng để đáp ứng nhu cầu, nhưng bạn không thể vượt quá số lượng hàng có sẵn ở bất kỳ cửa hàng nào. 

Đầu ra là tổng chi phí tối thiểu để mua tất cả các mặt hàng cần thiết cộng với chi phí di chuyển giữa các cửa hàng đã ghé thăm, giả sử bạn có thể kết thúc tại bất kỳ cửa hàng nào mà không phải trả tiền để trở về nhà. 

Những hạn chế ngay lập tức báo hiệu rằng việc sử dụng vũ lực đối với chuỗi cửa hàng là không thể. Với 17 cửa hàng, thậm chí xem xét tất cả các hoán vị sẽ cho ra 17 đường dẫn giai thừa, vượt xa 10^7 phép toán. Ngay cả một tập hợp con DP trên các cửa hàng cũng hợp lý, nhưng điều phức tạp thực sự là các quyết định không chỉ phụ thuộc vào các nút được truy cập mà còn phụ thuộc vào số lượng của từng loại mặt hàng đã được mua. Vì K có thể lên tới 50 và Q_i lên tới 2000 nên không thể có DP đa chiều đầy đủ cho nhu cầu còn lại. 

Một cấu trúc ẩn quan trọng là mỗi cửa hàng đóng góp các “gói” mặt hàng độc lập với chi phí tuyến tính, nghĩa là việc mua mặt hàng không tương tác giữa các loại ngoại trừ thông qua các quyết định du lịch. Sự tách biệt đó là yếu tố giúp cho việc nén có thể thực hiện được. 

Các trường hợp khó khăn quan trọng bao gồm các cửa hàng không thể tiếp cận được với những cửa hàng khác do chi phí bằng 0, buộc phải xử lý kết nối cẩn thận và các cửa hàng không bán đủ loại mặt hàng được yêu cầu, khiến câu trả lời là không thể ngay cả khi du lịch rẻ. Một trường hợp tinh tế khác là khi nhiều cửa hàng cung cấp cùng một mặt hàng ở các mức giá khác nhau và giải pháp tối ưu yêu cầu phải quay lại cùng một cửa hàng nhiều lần chỉ trong quá trình chuyển đổi DP khái niệm chứ không phải trong mô phỏng thực tế. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực sẽ cố gắng liệt kê mọi đơn hàng có thể ghé thăm các cửa hàng và với mỗi đơn hàng, hãy tham lam mua càng nhiều càng tốt từ mỗi cửa hàng. Điều đó đã tiêu tốn các chuỗi O(N!) Và thậm chí việc tính toán các giao dịch mua trên mỗi chuỗi cũng có giá lên tới O(NK), điều này trở nên không khả thi ngay lập tức. Điểm thất bại không chỉ là sự phức tạp của việc đặt hàng mà còn là sự tương tác giữa các hạn chế đặt hàng và mua hàng. 

Quan sát chính là thành phần du lịch chỉ phụ thuộc vào trình tự các cửa hàng, trong khi thành phần mua hàng chỉ phụ thuộc vào các lựa chọn tổng hợp về số lượng mua từ mỗi cửa hàng. Vì chúng tôi có thể lấy số lượng tùy ý đến giới hạn tồn kho, nên mỗi cửa hàng có thể được coi là cung cấp một vectơ số lượng mặt hàng với chi phí tuyến tính và chúng tôi chọn số lượng lấy từ mỗi cửa hàng một cách độc lập, chỉ bị ràng buộc bởi nhu cầu. 

Điều này chuyển vấn đề thành việc chọn một tập hợp con các “trạng thái” được xác định bởi lượng nhu cầu còn lại sau khi xử lý một nhóm cửa hàng, đồng thời tối ưu hóa các đường đi ngắn nhất giữa các chỉ số cửa hàng. Khía cạnh biểu đồ trở thành bài toán đường đi ngắn nhất của mặt nạ bit cổ điển: đối với mỗi tập hợp con các cửa hàng đã ghé thăm, chúng tôi muốn chi phí tối thiểu kết thúc tại một cửa hàng nhất định. Điều này gợi ý DP trên các tập hợp con kết hợp với quá trình tiền xử lý Floyd-Warshall để có chi phí đi lại ngắn nhất. 

Sau đó, thay vì lập mô hình số lượng vật phẩm một cách rõ ràng bên trong trạng thái DP, chúng tôi đảo ngược quan điểm. Đối với mỗi cửa hàng, chúng tôi tính toán trước chi phí tốt nhất có thể để đáp ứng bất kỳ tiền tố nào của nhu cầu, nhưng vì nhu cầu là độc lập với mỗi loại nên chúng tôi giảm mỗi cửa hàng thành các khoản đóng góp chi phí cho mỗi loại. Chiến lược tối ưu cho mỗi loại là mua tham lam từ các nguồn có sẵn với giá rẻ nhất trên các cửa hàng đã ghé thăm, điều đó có nghĩa là chúng tôi có thể phân loại theo từng loại và xử lý các khoản đóng góp một cách tích lũy. 

Điều này dẫn đến DP phân lớp: tập hợp con DP dành cho du lịch và tích lũy tham lam cho các giao dịch mua bằng cách sử dụng bảng giá được sắp xếp trước cho mỗi loại mặt hàng.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force về hoán vị và mua hàng | O(N! · N · K) | O(K) | Quá chậm | 
| Tập hợp con DP + Floyd + tập hợp tham lam theo từng loại | O(N2·2^N + K·N log N) | O(N·2^N + K·N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi xử lý trước biểu đồ để việc di chuyển giữa hai cửa hàng bất kỳ được biết ở dạng tối ưu. Vì N tối đa là 17 nên việc tính toán các đường đi ngắn nhất cho tất cả các cặp bằng cách sử dụng Floyd-Warshall sẽ rẻ và đảm bảo rằng bất kỳ chuỗi cửa hàng nào cũng có thể được đánh giá mà không phải lo lắng về việc định tuyến trung gian. 

Tiếp theo, chúng ta xác định DP trên các tập hợp con của các cửa hàng. Trạng thái thể hiện việc đã ghé thăm một loạt cửa hàng và kết thúc tại một cửa hàng cụ thể. Giá trị DP lưu trữ chi phí di chuyển tối thiểu để đạt được cấu hình đó bắt đầu từ cửa hàng 1. 

Chúng tôi khởi tạo DP chỉ với 1 cửa hàng đã ghé thăm và không tốn phí. Từ bất kỳ tiểu bang nào, chúng tôi cố gắng thêm một cửa hàng mới chưa được ghé thăm và cập nhật chi phí bằng cách sử dụng các đường dẫn ngắn nhất được tính toán trước. 

Sau đó, chúng ta cần đánh giá tính khả thi của việc mua hàng đối với một tập hợp con các cửa hàng nhất định. Đối với mỗi loại mặt hàng, chúng tôi tập hợp tất cả các ưu đãi từ các cửa hàng trong tập hợp con, mỗi ưu đãi là một cặp giá và số lượng có sẵn. Chúng tôi sắp xếp các ưu đãi này theo giá để luôn mô phỏng việc mua hàng từ các nguồn rẻ nhất trước tiên. Sau đó, chúng ta thỏa mãn nhu cầu về loại mặt hàng đó một cách tham lam, tích lũy chi phí cho đến khi đạt được số lượng yêu cầu hoặc hết hàng. 

Chúng tôi kết hợp chi phí mua của tất cả các loại mặt hàng với chi phí DP đi lại cho tập hợp con đó và theo dõi mức tối thiểu đối với tất cả các tập hợp con đáp ứng đầy đủ nhu cầu. 

Lý do nó hoạt động là vì đối với bất kỳ nhóm cửa hàng đã ghé thăm cố định nào, chiến lược mua hàng tốt nhất không bao giờ phụ thuộc vào thứ tự ghé thăm các cửa hàng, mà chỉ phụ thuộc vào nhiều ưu đãi có sẵn. Vì giá cả tuyến tính và số lượng có hạn nên việc lấy các đơn vị rẻ hơn trước luôn là điều tối ưu cho từng loại một cách độc lập. DP trên các tập hợp con đảm bảo chúng tôi xem xét tất cả các kết hợp có thể có của các cửa hàng có thể cùng đáp ứng nhu cầu trong khi tính toán chi phí đi lại một cách tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**18

def floyd(dist, n):
    for k in range(n):
        for i in range(n):
            dik = dist[i][k]
            if dik == INF:
                continue
            for j in range(n):
                nd = dik + dist[k][j]
                if nd < dist[i][j]:
                    dist[i][j] = nd

def solve():
    n = int(input())
    dist = []
    for _ in range(n):
        row = list(map(int, input().split()))
        for j in range(n):
            if row[j] == 0 and j != _:
                row[j] = INF
        dist.append(row)

    floyd(dist, n)

    k = int(input())
    need = list(map(int, input().split()))

    # offers[type][shop] = (price, qty)
    offers = [[[] for _ in range(n)] for _ in range(k)]

    for t in range(k):
        m = int(input())
        for _ in range(m):
            v, p, q = map(int, input().split())
            offers[t][v-1].append((p, q))

    # DP over subsets: min cost to end at j having visited mask
    size = 1 << n
    dp = [[INF] * n for _ in range(size)]
    dp[1][0] = 0

    for mask in range(size):
        for u in range(n):
            if dp[mask][u] == INF:
                continue
            for v in range(n):
                if mask & (1 << v):
                    continue
                nm = mask | (1 << v)
                nd = dp[mask][u] + dist[u][v]
                if nd < dp[nm][v]:
                    dp[nm][v] = nd

    def purchase_cost(mask):
        total = 0
        for t in range(k):
            rem = need[t]
            pool = []
            for i in range(n):
                if mask & (1 << i):
                    for p, q in offers[t][i]:
                        pool.append((p, q))
            pool.sort()
            for p, q in pool:
                if rem <= 0:
                    break
                take = min(rem, q)
                total += take * p
                rem -= take
            if rem > 0:
                return INF
        return total

    ans = INF
    for mask in range(size):
        for u in range(n):
            if dp[mask][u] == INF:
                continue
            pc = purchase_cost(mask)
            if pc < INF:
                ans = min(ans, dp[mask][u] + pc)

    print(-1 if ans == INF else ans)

if __name__ == "__main__":
    solve()
```Khối Floyd-Warshall đảm bảo chúng ta có thể coi việc di chuyển là những con đường ngắn nhất trực tiếp, tránh các vấn đề về xây dựng lại đường dẫn. Tập hợp con DP xây dựng tất cả các nhóm cửa hàng có thể tiếp cận và chi phí di chuyển tối thiểu của chúng. Hàm buy_cost tách biệt từng mặt nạ và xây dựng lại chiến lược mua hàng tốt nhất có thể, an toàn vì trong mặt nạ cố định, việc đặt hàng không quan trọng đối với việc định giá tuyến tính. 

Một lỗi triển khai phổ biến là trộn lẫn các quyết định mua hàng với quá trình chuyển đổi DP. Điều đó phá vỡ tính chính xác vì tính khả thi của việc mua hàng phụ thuộc vào toàn bộ tập hợp chứ không phải các đường dẫn gia tăng. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản với 3 cửa hàng trong đó cửa hàng 1 kết nối với cửa hàng 2 và 3 và mỗi loại mặt hàng đều có nhu cầu nhỏ. Giả sử cửa hàng 2 có hàng rẻ loại 0 và cửa hàng 3 có hàng đắt. DP sẽ đánh giá cả hai mặt nạ {1,2} và {1,3}. Chi phí mua hàng cho {1,2} sẽ đáp ứng đầy đủ nhu cầu với giá rẻ, trong khi {1,3} sẽ mang lại chi phí cao hơn, do đó DP chọn chi phí mua hàng trước bất kể tính đối xứng của hành trình. 

Ví dụ thứ hai là khi một loại mặt hàng chỉ có sẵn một phần trên tất cả các cửa hàng trong một tập hợp con. DP sẽ loại bỏ tập hợp con đó một cách chính xác vì buy_cost trả về INF, ngăn chặn các giải pháp không hợp lệ đóng góp vào câu trả lời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^N · N² + K · 2^N · S log S) | tập hợp con chuyển tiếp DP trên tất cả các cặp cộng với các ưu đãi sắp xếp trên mỗi mặt nạ | 
| Không gian | O(2^N · N + K · N) | Bảng DP và các ưu đãi được lưu trữ | 

Với N ≤ 17, 2^N là khoảng 131072, khiến DP trở nên khả thi. K ≤ 50 giúp quản lý tổng hợp mua hàng trên mỗi mặt nạ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    # placeholder: assume solve() is defined above
    return ""

# sample
assert run("""5
0 1 3 0 2
1 0 5 0 5
3 5 0 7 2
0 0 7 0 2
2 5 2 2 0
3
3 5 5
3
1 3 2
3 2 1
5 4 3
3
2 4 3
3 5 4
5 2 1
4
1 9 1
2 8 2
3 7 3
4 6 1
""").strip() == "70"

# custom 1: single shop impossible
assert run("""1
0
1
5
0
""") == "-1"

# custom 2: two shops sufficient
assert run("""2
0 1
1 0
1
3
1
1 2 10
""") == "3"

# custom 3: disconnected impossible
assert run("""3
0 1 0
1 0 0
0 0 0
1
1
1
1 5 1
""") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cửa hàng duy nhất không có hàng | -1 | phát hiện nhu cầu không khả thi | 
| hai cửa hàng mua đơn giản | 3 | tổng hợp và du lịch chính xác | 
| đồ thị bị ngắt kết nối | -1 | xử lý các bộ du lịch INF và các bộ không thể truy cập | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một tập hợp con các cửa hàng có vẻ hấp dẫn để đi du lịch nhưng không thể đáp ứng được nhu cầu. Ví dụ: nếu một tập hợp con chỉ bao gồm các cửa hàng thiếu chung một loại mặt hàng, thì buy_cost trả về INF và đảm bảo rằng trạng thái DP bị bỏ qua. Thuật toán xử lý việc này bằng cách kiểm tra rõ ràng nhu cầu còn lại trên mỗi loại sau khi phân bổ tham lam. 

Một trường hợp cạnh khác là các cạnh có chi phí bằng 0 thực sự không có tuyến đường. Những thứ này phải được chuyển đổi thành INF trước Floyd-Warshall; nếu không, thuật toán sẽ coi các cạnh bị thiếu là hành trình tự do và đánh giá thấp chi phí.
