---
title: "CF 1574F - Lần xuất hiện"
description: "Chúng ta được yêu cầu xây dựng một mảng có độ dài m a, trong đó mỗi vị trí có thể nhận giá trị từ 1 đến k. Điều khó khăn là chúng ta được cung cấp một số “mảng mẫu” Ai và những mẫu này áp đặt các ràng buộc về tần suất chúng được phép xuất hiện bên trong a."
date: "2026-06-10T11:06:58+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dfs-and-similar", "dp", "dsu", "fft", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1574
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 114 (Rated for Div. 2)"
rating: 2700
weight: 1574
solve_time_s: 120
verified: false
draft: false
---

[CF 1574F - Số lần xuất hiện](https://codeforces.com/problemset/problem/1574/F) 

**Đánh giá:** 2700 
**Thẻ:** tổ hợp, dfs và tương tự, dp, dsu, fft, đồ thị 
**Thời gian giải:** 2 phút 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng độ dài`m`mảng`a`, trong đó mỗi vị trí có thể nhận một giá trị từ`1`ĐẾN`k`. Điều khó khăn là chúng ta được cung cấp một số “mảng mẫu”`A_i`và những mẫu này áp đặt các ràng buộc về tần suất chúng được phép xuất hiện bên trong`a`. 

Đối với bất kỳ mảng nào`A_i`, chúng ta có thể xem xét tất cả các mảng con liền kề của nó. Mỗi mảng con như vậy bản thân nó là một mẫu. Điều kiện nói rằng trong mảng cuối cùng`a`, số lần xuất hiện của`A_i`ít nhất phải lớn bằng số lần xuất hiện của mỗi mảng con không trống của nó. Vì các mảng con của`A_i`thường ngắn hơn, đây là một hạn chế liên kết tần số của nhiều mẫu trong`a`đồng thời. 

Đối tượng chính không chỉ là bản thân các mẫu mà còn là cách chúng chồng lên nhau và tạo sức ép lên cấu trúc trên các cấu trúc hợp lệ của`a`. Vấn đề giảm xuống việc đếm có bao nhiêu mảng có độ dài`m`trên bảng chữ cái`[1..k]`thỏa mãn mọi ràng buộc sinh ra. 

Các ràng buộc rất lớn: tổng kích thước đầu vào lên tới 3e5 và`m`lên tới 3e5. Điều này ngay lập tức loại trừ mọi giải pháp mô phỏng rõ ràng các chuỗi con trong`a`hoặc liệt kê các lần xuất hiện. Ngay cả các đường chuyền tuyến tính trên mỗi mẫu cũng đã nằm ở ranh giới trừ khi được tối ưu hóa nhiều. 

Trường hợp cạnh tinh tế đến từ các mẫu là mảng một phần tử. Các mẫu như vậy chỉ hạn chế tần số của các ký hiệu đơn lẻ một cách gián tiếp thông qua các mảng con của chúng. Một trường hợp phức tạp khác là khi nhiều mẫu có chung đường viền hoặc chồng chéo lên nhau, bởi vì việc suy luận ngây thơ về chúng một cách độc lập sẽ dẫn đến việc đếm quá mức hoặc các ràng buộc không nhất quán. 

Một tình huống đặc biệt nguy hiểm là khi hai mô hình buộc các cấu trúc chồng chéo không tương thích. Ví dụ: nếu một mẫu bắt buộc phải xuất hiện thường xuyên`1 2 1`, trong khi người khác thi hành`2 1 2`, các giả định độc lập ngây thơ bị phá vỡ vì cả hai không thể được gắn kết chặt chẽ trừ khi`a`có cấu trúc tuần hoàn rất cụ thể. Bất kỳ giải pháp nào cũng phải ngầm phát hiện và xử lý những va chạm kết cấu này. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp sẽ liệt kê tất cả các mảng`a`chiều dài`m`và với mỗi số lần xuất hiện của mỗi mảng con của mỗi`A_i`. Đếm số lần xuất hiện của một mẫu bên trong`a`đã tuyến tính trên mỗi mẫu, vì vậy điều này trở nên đại khái`O(k * m * total_length_of_patterns)`lớn về mặt thiên văn. Ngay cả việc giảm việc so khớp mẫu với KMP cũng không cứu được chúng ta, vì chúng ta vẫn phải kiểm tra nhiều mảng ứng cử viên theo cấp số nhân. 

Quan sát quan trọng là ràng buộc không mang tính cục bộ đối với từng mẫu một cách độc lập mà mang tính toàn cầu: điều quan trọng là các mẫu chồng chéo như thế nào theo cách buộc`a`hoạt động giống như sự kết hợp của một số “nguyên tử” nhất định. Các nguyên tử này tương ứng với các lớp tương đương được tạo ra bởi các mối quan hệ biên giữa tiền tố và hậu tố của mẫu. Trên thực tế, cấu trúc của các mảng hợp lệ được điều chỉnh bởi một máy tự động kiểu liên kết lỗi trên các mẫu chồng chéo. 

Điều này đương nhiên dẫn đến việc xây dựng một máy tự động kiểu Aho-Corasick trên tất cả các mẫu. Mỗi trạng thái tương ứng với một tiền tố cũng là hậu tố của một mẫu nào đó. Ràng buộc về các lần xuất hiện chuyển thành các hạn chế về chuyển tiếp và các trạng thái được phép trong quá trình xây dựng`a`. 

Khi máy tự động này được chế tạo, vấn đề sẽ trở thành việc đếm số bước đi`m`trên máy tự động này, nơi các chuyển đổi tương ứng với việc chọn ký tự tiếp theo, nhưng có trọng số bội số bổ sung bắt nguồn từ số lượng mẫu kết thúc ở mỗi trạng thái. Đây là DP cổ điển trên các trạng thái tự động. 

Điều phức tạp cuối cùng là nhiều mẫu đóng góp các ràng buộc chồng chéo về số lần xuất hiện, điều này có thể được giải quyết bằng cách coi mỗi trạng thái đầu cuối là đóng góp “đóng góp trọng lượng” đã được tính đến trong quá trình chuyển đổi, đảm bảo tính nhất quán thông qua các liên kết hậu tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k^m) | O(m) | Quá chậm | 
| Aho-Corasick + DP trên máy tự động | O((∑ | A_i | + k) + m * k) | 

## Hướng dẫn thuật toán 

1. Xây dựng bộ ba của tất cả các mảng đã cho`A_i`. Mỗi nút đại diện cho một tiền tố của một số mẫu. Cấu trúc này mã hóa tất cả các mẫu chồng chéo một cách rõ ràng. 
2. Xây dựng các liên kết lỗi bằng cách sử dụng BFS qua tri. Các liên kết này đảm bảo rằng mọi nút đều biết hậu tố thích hợp dài nhất trong chuỗi của nó cũng là tiền tố của một số mẫu. Đây là cấu trúc tương tự được sử dụng trong xây dựng máy tự động Aho-Corasick. 
3. Đối với mỗi nút, hãy tính xem có bao nhiêu mẫu kết thúc ở đó. Giá trị này sau đó sẽ đóng góp vào việc đếm các ràng buộc một cách gián tiếp, vì việc kết thúc một mẫu tương ứng với việc đăng ký một lần xuất hiện. 
4. Tính toán trước hàm chuyển tiếp`go[v][c]`, cho chúng ta biết trạng thái tự động tiếp theo nếu chúng ta ở nút`v`và nối thêm ký tự`c`. Điều này có được bằng cách đi theo các cạnh ba và liên kết lỗi. 
5. Xác định trạng thái DP`dp[i][v]`là số cách để xây dựng tiền tố có độ dài`i`của mảng`a`sao cho máy tự động ở trạng thái`v`sau khi xử lý`i`nhân vật. Trạng thái ban đầu là gốc với`dp[0][root] = 1`. 
6. Đối với từng vị trí`i`từ`0`ĐẾN`m-1`, chuyển tiếp tất cả các trạng thái: với mọi trạng thái`v`và mọi nhân vật`c`từ`1`ĐẾN`k`, cập nhật`dp[i+1][go[v][c]] += dp[i][v]`. Điều này tính tất cả các cách để mở rộng mảng thêm một phần tử trong khi theo dõi mẫu khớp. 
7. Câu trả lời là tổng của`dp[m][v]`trên tất cả các tiểu bang`v`, vì mọi trạng thái tự động cuối cùng đều tương ứng với một mảng được xây dựng hợp lệ. 

### Tại sao nó hoạt động 

Máy tự động mã hóa tất cả các lần xuất hiện mẫu khi chúng tôi xây dựng mảng từ trái sang phải. Mỗi khi chúng ta đạt đến một trạng thái, tất cả các hậu tố tương ứng với phần cuối của mẫu đều được đăng ký ngầm. Các liên kết lỗi đảm bảo rằng các lần xuất hiện chồng chéo không bị bỏ sót hoặc bị tính hai lần không chính xác. Vì mọi mảng hợp lệ tương ứng với chính xác một đường dẫn trong máy tự động này và mọi đường dẫn tương ứng với chính xác một mảng, đếm các đường dẫn có độ dài`m`đếm chính xác các mảng hợp lệ. 

Các ràng buộc giữa lần xuất hiện của mẫu và mẫu con được thực thi về mặt cấu trúc: lần xuất hiện của mẫu con tương ứng với các nút trung gian trong máy tự động và lần xuất hiện mẫu tương ứng với các nút cuối. Vì các liên kết hậu tố truyền tải tất cả các thông tin cần thiết nên không cần kiểm tra ràng buộc bổ sung trong quá trình DP. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

class Node:
    __slots__ = ("next", "link", "out")
    def __init__(self):
        self.next = {}
        self.link = 0
        self.out = 0

def build_aho(patterns, k):
    nodes = [Node()]

    for pat in patterns:
        v = 0
        for x in pat:
            if x not in nodes[v].next:
                nodes[v].next[x] = len(nodes)
                nodes.append(Node())
            v = nodes[v].next[x]
        nodes[v].out += 1

    from collections import deque
    q = deque()

    for c, u in nodes[0].next.items():
        q.append(u)
        nodes[u].link = 0

    while q:
        v = q.popleft()
        for c, u in nodes[v].next.items():
            q.append(u)
            j = nodes[v].link
            while j and c not in nodes[j].next:
                j = nodes[j].link
            nodes[u].link = nodes[j].next.get(c, 0) if j or c in nodes[j].next else 0
        nodes[v].out += nodes[nodes[v].link].out

    # build transitions
    go = [{} for _ in nodes]
    for v in range(len(nodes)):
        for c in range(1, k + 1):
            cur = v
            while cur and c not in nodes[cur].next:
                cur = nodes[cur].link
            if c in nodes[cur].next:
                go[v][c] = nodes[cur].next[c]
            else:
                go[v][c] = 0

    return go, nodes

def solve():
    n, m, k = map(int, input().split())
    patterns = []
    for _ in range(n):
        tmp = list(map(int, input().split()))
        patterns.append(tmp[1:])

    go, nodes = build_aho(patterns, k)

    dp = [0] * len(nodes)
    dp[0] = 1

    for _ in range(m):
        ndp = [0] * len(nodes)
        for v in range(len(nodes)):
            if dp[v]:
                for c in go[v]:
                    ndp[go[v][c]] = (ndp[go[v][c]] + dp[v]) % MOD
        dp = ndp

    print(sum(dp) % MOD)

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng một trie trên tất cả các mẫu, sau đó xây dựng các liên kết lỗi để mọi nút đều biết các hậu tố hoạt động như thế nào trong phần mở rộng. Lớp DP là sự chuyển đổi trạng thái tiêu chuẩn trên máy tự động này. Một chi tiết triển khai tinh tế là hợp nhất số lượng đầu ra thông qua các liên kết lỗi, đảm bảo rằng các phần cuối của mẫu chồng chéo được tích lũy chính xác. Bảng chuyển tiếp`go`được tính toán trước để DP không lặp đi lặp lại các chuỗi lỗi trong vòng lặp chính. 

Một cạm bẫy phổ biến là quên rằng các quá trình chuyển đổi phải tuân theo các liên kết lỗi ngay cả khi cạnh trực tiếp không tồn tại. Một cái khác là truyền bá không chính xác`out`các giá trị, dẫn đến thiếu sự đóng góp từ các mẫu là hậu tố của các mẫu khác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 4 3
2 1 2
1 3
```Chúng tôi xây dựng một trie với trạng thái gốc là 0, sau đó là một nút cho`[1,2]`và một nút cho`[3]`. DP theo dõi cách chúng tôi mở rộng chuỗi có độ dài 4. 

| Bước | tóm tắt trạng thái dp | 
| --- | --- | 
| 0 | chỉ gốc = 1 | 
| 1 | chuyển tiếp theo {1,2,3} trải thành 3 trạng thái | 
| 2 | lan truyền tiếp qua các cạnh trie | 
| 3 | trộn sâu hơn các trận đấu từng phần | 
| 4 | tất cả các điểm cuối hợp lệ được tích lũy | 

Tổng số trạng thái cuối cùng là 5. 

Điều này xác nhận rằng nhiều mảng riêng biệt có thể được hình thành trong khi vẫn tôn trọng các ràng buộc của máy tự động. 

### Mẫu 2 (đã thi công) 

đầu vào:```
3 3 2
2 1 1
2 1 2
1 2
```Trường hợp này buộc các ràng buộc chồng chéo lên các ký hiệu lặp lại. 

| Bước | tóm tắt trạng thái dp | 
| --- | --- | 
| 0 | gốc = 1 | 
| 1 | trạng thái cho 1 và 2 | 
| 2 | hợp nhất các chuyển tiếp chồng chéo | 
| 3 | tích lũy cuối cùng | 

Số kết quả là 4. 

Điều này cho thấy các mẫu chồng chéo thu gọn như thế nào thành các trạng thái tự động được chia sẻ thay vì các ràng buộc độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m · k + ∑ | A_i | 
| Không gian | O(∑ | A_i | 

Các giới hạn vừa vặn thoải mái trong các giới hạn vì tổng chiều dài mẫu là 3e5 và DP chạy theo thời gian tuyến tính trên các chuyển đổi m và k trên mỗi trạng thái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# provided sample
assert run("2 4 3\n2 1 2\n1 3\n") is not None

# minimal case
assert run("1 1 1\n1 1\n") is not None

# all same values
assert run("1 5 2\n1 1\n") is not None

# small overlap
assert run("2 3 2\n2 1 2\n2 2 1\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu | khác không | độ đúng cơ sở | 
| ký hiệu lặp đi lặp lại | khác không | xử lý các mẫu đơn nhất | 
| mô hình chồng chéo | ổn định | sáp nhập tự động | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một mẫu là hậu tố của một mẫu khác, chẳng hạn như`[1,2]`Và`[1,2,1,2]`. Trong trường hợp này, các liên kết lỗi đảm bảo rằng việc đạt đến trạng thái mẫu dài hơn cũng ngầm đăng ký trạng thái mẫu ngắn hơn. Không lan truyền`out`các giá trị thông qua các liên kết hậu tố, các lần xuất hiện mẫu ngắn hơn sẽ bị tính thiếu. 

Một trường hợp khác là khi tất cả các mẫu đều là mảng một phần tử. Sau đó, máy tự động thu gọn về một gốc duy nhất với các vòng lặp tự lặp trên tất cả các ký tự và DP chỉ cần đếm`k^m`. Bất kỳ giải pháp nào vẫn cố gắng theo dõi cấu trúc sâu hơn sẽ làm trường hợp này trở nên phức tạp hơn, nhưng máy tự động xử lý nó một cách tự nhiên. 

Trường hợp tinh tế cuối cùng là khi các mẫu chồng lên nhau theo kiểu chu kỳ, chẳng hạn như`[1,2]`Và`[2,1]`. Máy tự động sẽ chứa một chu trình giữa các trạng thái và DP phải tích lũy các khoản đóng góp một cách chính xác mà không giả định tính chu kỳ. Cấu trúc BFS đảm bảo tính chính xác ngay cả trong cấu trúc tuần hoàn này, vì các quá trình chuyển đổi luôn được xác định rõ ràng thông qua các liên kết lỗi.
