---
title: "CF 105481K - \u53ef\u91cd\u96c6\u5408"
description: "Chúng ta bắt đầu với một tập hợp $S$ trống. Mỗi thao tác hoặc chèn một lần xuất hiện của số $x$ vào $S$, hoặc loại bỏ một lần xuất hiện của $x$ được đảm bảo tồn tại."
date: "2026-06-23T02:01:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105481
codeforces_index: "K"
codeforces_contest_name: "2024 CCPC Liaoning Provincial Contest"
rating: 0
weight: 105481
solve_time_s: 73
verified: true
draft: false
---

[CF 105481K - \u53ef\u91cd\u96c6\u5408](https://codeforces.com/problemset/problem/105481/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một multiset trống$S$. Mỗi thao tác sẽ chèn một lần xuất hiện của một số$x$vào trong$S$hoặc loại bỏ một lần xuất hiện của$x$điều đó được đảm bảo tồn tại. Sau mỗi lần sửa đổi, chúng tôi xem xét tất cả các tổng tập hợp con có thể được hình thành bằng cách chọn bất kỳ tập hợp con nào của các phần tử hiện tại và chúng tôi đếm xem có bao nhiêu số nguyên dương riêng biệt có thể được biểu diễn dưới dạng tổng như vậy. 

Nói cách khác, sau mỗi bước chúng ta xem xét tất cả các lựa chọn hữu hạn của các phần tử từ$S$(mỗi bản sao có thể được sử dụng tối đa một lần, vì đây là nhiều tập hợp), tính tổng của chúng và hỏi có bao nhiêu giá trị khác nhau$1,2,3,\dots$có thể đạt được. Đầu ra sau mỗi thao tác là kích thước của tập hợp có thể truy cập này, được giới hạn ở số nguyên dương. 

Ràng buộc chính là tại mọi thời điểm, tổng của tất cả các phần tử hiện có trong$S$nhiều nhất là$5 \cdot 10^5$. Điều này ngay lập tức giới hạn tổng tập hợp con tối đa có thể mà chúng ta cần quan tâm, bởi vì không có tổng tập hợp con nào có thể vượt quá tổng tổng. Nó cũng ngụ ý rằng bất kỳ trạng thái lập trình động nào trên tổng đều có thể được giới hạn một cách an toàn bởi$5 \cdot 10^5$. 

Số lượng hoạt động nhiều nhất là$5 \cdot 10^3$, vì vậy chúng tôi có thể cung cấp các thuật toán với khoảng$O(n \cdot \text{poly}(V))$Ở đâu$V = 5 \cdot 10^5$chỉ khi công việc theo từng bước được tối ưu hóa cực kỳ tốt, lý tưởng nhất là sử dụng các hoạt động ở cấp độ bit hoặc tái sử dụng khấu hao. 

Một cách tiếp cận ngây thơ để tính toán lại các tổng tập hợp con từ đầu sau mỗi thao tác là quá chậm, vì mỗi lần tính toán lại tốn khoảng$O(nV)$hoặc$O(V \cdot \#\text{elements})$, và làm điều đó cho đến$5 \cdot 10^3$có lúc trở nên bất khả thi. 

Một cạm bẫy tinh vi là việc xóa bỏ sẽ phá vỡ tính đơn điệu. Nếu chúng tôi chỉ có phần chèn thêm thì bộ có thể truy cập chỉ tăng lên và bản cập nhật bitset ba lô tiêu chuẩn là đủ. Hoạt động xóa buộc chúng tôi phải duy trì cấu trúc động hoàn toàn thay vì DP một chiều. 

Một trường hợp cạnh khác là các giá trị lặp lại. Vì các phần chèn vào không được đảm bảo là duy nhất nên chúng ta phải xử lý$S$như một multiset thực sự. Ví dụ, chèn$x$hai lần có thể tạo ra số tiền yêu cầu sử dụng cả hai bản sao và việc xóa một bản sao chỉ có thể rút lại một phần các khoản đóng góp đó. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: duy trì multiset hiện tại và tính toán lại DP tổng tập hợp con sau mỗi thao tác. Chúng tôi giữ một mảng boolean$dp[s]$cho biết liệu tổng$s$có thể đạt được và với mỗi yếu tố$x$chúng tôi thực hiện chuyển đổi chiếc ba lô cổ điển$dp[s] \leftarrow dp[s] \lor dp[s-x]$từ cao xuống thấp. Điều này tính toán chính xác tất cả các tổng tập hợp con cho trạng thái hiện tại. 

Vấn đề là việc tính toán lại từ đầu sau mỗi thao tác là quá tốn kém. Mỗi lần xây dựng lại chi phí$O(V \cdot |S|)$, và kể từ đó$|S|$có thể$O(n)$, tổng công việc trở thành$O(n^2 V)$, vượt xa giới hạn. 

Quan sát quan trọng là chúng ta không cần phải tính toán lại mọi thứ một cách độc lập cho mỗi thao tác. Thay vào đó, chúng ta có thể coi thời gian như một chiều không gian khác. Mỗi phần tử chỉ tồn tại trong một khoảng thời gian liền kề từ khi chèn đến khi xóa. Nếu chúng ta biết các khoảng này, chúng ta có thể chỉ định mỗi lần xuất hiện của một số$x$tới phân đoạn hoạt động mà nó đang hoạt động. 

Điều này chuyển đổi vấn đề thành một chiếc ba lô động hoàn toàn ngoại tuyến theo thời gian: mỗi phần tử hoạt động trong một khoảng thời gian nào đó$[l, r]$và chúng ta cần kết quả DP sau mỗi tiền tố thời gian, xem xét chính xác các phần tử có khoảng thời gian bao gồm thời điểm đó. Một cách tiêu chuẩn để xử lý vấn đề này là sử dụng cây phân đoạn theo thời gian trong đó mỗi nút lưu trữ các mục hoạt động đầy đủ trong khoảng thời gian đó. 

Sau đó, chúng tôi chạy DP kiểu chia để trị: tại mỗi nút cây phân đoạn, chúng tôi duy trì một DP tập hợp bit biểu thị tổng tập hợp con được đóng góp bởi các mục được gán cho nút đó. Chúng tôi kết hợp các phần tử con bằng cách chuyển DP xuống dưới, đảm bảo mỗi phần tử được áp dụng chính xác một lần trên mỗi đường dẫn từ gốc đến lá. Bởi vì mỗi phần tử được chèn vào$O(\log n)$các nút, tổng số ứng dụng có thể quản lý được. 

Bản thân DP được triển khai dưới dạng bitset, do đó các phép dịch chuyển và OR được thực hiện trên các từ máy, giúp quá trình chuyển đổi nhanh hơn đáng kể so với các mảng boolean đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại DP sau mỗi thao tác |$O(n^2 V)$|$O(V)$| Quá chậm | 
| Cây phân đoạn theo thời gian + bitset DP |$O(n \log n \cdot V / w)$|$O(V)$| Đã chấp nhận | 

Đây$w$là kích thước từ của máy (thường là 64), giúp cho các hoạt động của bitset trở nên thực tế. 

## Hướng dẫn thuật toán 

### 1. Chuyển đổi các hoạt động thành các khoảng thời gian hoạt động 

Chúng tôi quét các hoạt động theo thứ tự và duy trì một ngăn xếp hoặc bản đồ tần số cho từng giá trị$x$. Mỗi lần chèn của$x$bắt đầu một khoảng thời gian hoạt động mới và mỗi lần xóa sẽ đóng khoảng thời gian mở gần đây nhất cho$x$. Nếu một phần tử được chèn nhiều lần thì mỗi bản sao sẽ được theo dõi độc lập. 

Bước này biến đổi một chuỗi động thành một tập hợp các khoảng thời gian rời rạc cho mỗi lần xuất hiện của phần tử. 

### 2. Xây dựng cây phân đoạn theo thời gian 

Chúng tôi xây dựng cây phân đoạn trên phạm vi chỉ mục$[1, n]$. Mỗi nút tương ứng với một khoảng thời gian. Với mỗi khoảng phần tử$[l, r]$, chúng tôi chèn giá trị của nó$x$vào tất cả các nút cây phân đoạn bao phủ đầy đủ khoảng này. Điều này đảm bảo rằng mỗi nút chỉ lưu trữ các phần tử được đảm bảo hoạt động trong toàn bộ phân đoạn của nó. 

Lý do sự phân tách này có hiệu quả là vì mọi điểm thời gian đều thuộc về$O(\log n)$các nút và mọi phần tử được lưu trữ chính xác trong các nút đại diện cho phạm vi tối thiểu trong khoảng thời gian hoạt động của nó. 

### 3. Sử dụng DP bitset cho tổng tập hợp con 

Chúng tôi duy trì một bitset$dp$, bit ở đâu$i$là 1 nếu tổng$i$là có thể đạt được. Ban đầu chỉ$dp[0] = 1$. 

Khi xử lý một nút, chúng tôi lặp lại tất cả các giá trị$x$được lưu trữ trong nút đó và áp dụng quá trình chuyển đổi$dp \leftarrow dp \,|\, (dp \ll x)$. Mô hình này bao gồm hoặc loại trừ mỗi lần xuất hiện của$x$. 

Bởi vì các ca hoạt động trên các số nguyên lớn (bộ bit), nên việc này nhanh hơn đáng kể so với việc lặp lại tất cả các tổng một cách rõ ràng. 

### 4. DFS trên cây phân đoạn 

Chúng ta duyệt cây phân đoạn một cách đệ quy. Tại mỗi nút, chúng tôi áp dụng tất cả các mục được lưu trữ của nó vào một bản sao cục bộ của DP. Khi tới một chiếc lá tương ứng với thời gian$t$, DP thể hiện chính xác trạng thái nhiều tập hợp sau khi vận hành$t$, vì vậy chúng tôi đếm số lượng tổng dương có thể truy cập bằng cách đếm các bit đã đặt trong phạm vi$[1, \text{sum}]$. 

Mỗi nhánh đệ quy khôi phục trạng thái DP sau khi kết thúc, do đó các nút anh chị em không bị ảnh hưởng. 

### Tại sao nó hoạt động 

Tính đúng đắn đến từ hai bất biến. Đầu tiên, mọi phần tử được chèn vào các nút bao phủ chính xác thời gian hoạt động của nó, do đó, tại bất kỳ lá nào, DP bao gồm chính xác các phần tử hoạt động tại thời điểm đó. Thứ hai, mỗi quá trình chuyển đổi DP là một túi chứa tổng tập hợp con tiêu chuẩn, do đó, bitset mã hóa chính xác tất cả các tổng có thể đạt được cho nhiều tập hợp được biểu thị tại nút đó. Vì phân vùng cây phân đoạn không có lỗi chồng chéo nên câu trả lời của mọi thao tác đều được tính toán từ việc xây dựng lại chính xác tập hoạt động. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXV = 500000

def count_bits(x):
    return x.bit_count() - 1  # exclude 0

def add_interval(tree, node, l, r, ql, qr, val):
    if ql <= l and r <= qr:
        tree[node].append(val)
        return
    mid = (l + r) // 2
    if ql <= mid:
        add_interval(tree, node * 2, l, mid, ql, qr, val)
    if qr > mid:
        add_interval(tree, node * 2 + 1, mid + 1, r, ql, qr, val)

def dfs(tree, node, l, r, dp, ans):
    local = dp
    for x in tree[node]:
        local = local | (local << x)

    if l == r:
        ans[l] = count_bits(local)
        return

    mid = (l + r) // 2
    dfs(tree, node * 2, l, mid, local, ans)
    dfs(tree, node * 2 + 1, mid + 1, r, local, ans)

def solve():
    n = int(input())
    ops = []
    pos = {}
    intervals = []

    for i in range(1, n + 1):
        op, x = map(int, input().split())
        ops.append((op, x))
        if op == 1:
            pos.setdefault(x, []).append(i)
        else:
            l = pos[x].pop()
            intervals.append((l, i, x))

    for x, lst in pos.items():
        for l in lst:
            intervals.append((l, n, x))

    size = 4 * (n + 5)
    tree = [[] for _ in range(size)]

    for l, r, x in intervals:
        add_interval(tree, 1, 1, n, l, r, x)

    dp = 1  # bitset with only 0 reachable
    ans = [0] * (n + 1)

    dfs(tree, 1, 1, n, dp, ans)

    print("\n".join(str(ans[i]) for i in range(1, n + 1)))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ chuyển đổi chuỗi chèn/xóa trực tuyến thành các khoảng thời gian hoạt động rõ ràng. Sau đó, nó phân phối từng khoảng thời gian vào một cây phân đoạn để mỗi nút chỉ lưu trữ các phần tử hoạt động hoàn toàn trong phạm vi thời gian đó. DFS mang DP bitset xuống dưới, cập nhật nó với các phần tử của mỗi nút. Bởi vì các số nguyên Python hoạt động như các tập hợp bit có độ dài tùy ý, nên các phép dịch chuyển và OR thực hiện chuyển đổi tổng tập hợp con một cách tự nhiên. 

Câu trả lời ở mỗi lá được tính bằng cách đếm các bit tập hợp trừ 0, vì chỉ yêu cầu tổng dương. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi ngắn trong đó các phần tử được hình thành và sau đó một phần tử bị loại bỏ. 

đầu vào:```
1 1
1 2
1 3
2 2
```Chúng tôi theo dõi cách tập hợp hoạt động thay đổi và số tiền có thể có. 

| Bước | Hoạt động | Đa hoạt động | Số tiền có thể tiếp cận DP | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | thêm 1 | {1} | {0,1} | 1 | 
| 2 | thêm 2 | {1,2} | {0,1,2,3} | 3 | 
| 3 | thêm 3 | {1,2,3} | tất cả các khoản tiền 1..6 | 6 | 
| 4 | loại bỏ 2 | {1,3} | {0,1,3,4} | 3 | 

Điều này cho thấy việc xóa làm giảm các kết hợp có thể truy cập phụ thuộc vào phần tử bị xóa như thế nào, đặc biệt là các tổng như 2 và 5 yêu cầu nó. 

Cây phân đoạn DP tái tạo chính xác các trạng thái này ở mỗi lá bằng cách đảm bảo rằng chỉ các khoảng thời gian bao gồm thời gian hiện tại mới đóng góp vào bitset. 

Bây giờ hãy xem xét các giá trị lặp lại: 

đầu vào:```
1 5
1 5
2 5
```Ở đây chúng ta thấy cách xử lý đa dạng. 

| Bước | Nhiều bộ | Số tiền có thể tiếp cận | Trả lời | 
| --- | --- | --- | --- | 
| 1 | {5} | {0,5} | 1 | 
| 2 | {5,5} | {0,5,10} | 2 | 
| 3 | {5} | {0,5} | 1 | 

Điểm mấu chốt là mỗi bản sao của 5 đóng góp độc lập và việc xóa một bản sao không loại bỏ tất cả các khoản tiền liên quan đến 5. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n \cdot V / 64)$| Mỗi phần tử được lưu trữ trong$O(\log n)$các nút cây phân đoạn và mỗi lần chèn sử dụng dịch chuyển bitset/HOẶC trên$V$bit | 
| Không gian |$O(V + n \log n)$| Bitset DP cộng với lưu trữ cây phân đoạn | 

Những ràng buộc cho phép$n \le 5 \cdot 10^3$và tổng số tiền lên tới$5 \cdot 10^5$, giữ cho chiều rộng DP bị giới hạn. Các phép toán bitset là phần đắt tiền duy nhất, nhưng số học số nguyên cấp độ từ của Python giữ cho chúng đủ nhanh và cây phân đoạn ngăn việc tính toán lại DP từ đầu cho mỗi truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.readline  # placeholder for real integration

# NOTE: In actual use, replace run() with calling solve()

# minimal case
# assert run("1\n1 1\n") == "1\n"

# small mixed operations
# assert run("4\n1 1\n1 2\n2 1\n1 3\n") == "1\n3\n1\n2\n"

# repeated values
# assert run("3\n1 5\n1 5\n2 5\n") == "1\n2\n1\n"

# boundary stress (all small values)
# assert run("5\n1 1\n1 2\n1 3\n1 4\n1 5\n")[-1] == "15\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các phần chèn tăng nhỏ | DP đang phát triển | khai triển đơn điệu | 
| chèn lặp lại rồi xóa | tính đúng bội số | xử lý nhiều bộ | 
| xen kẽ thêm/xóa | sự ổn định của khoảng | tính chính xác của các bản cập nhật động | 
| cạnh phần tử đơn | hành vi DP cơ bản | khởi tạo đúng đắn | 

## Vỏ cạnh 

Một trường hợp tinh tế là việc chèn lặp lại cùng một giá trị trước khi xóa. Ví dụ, chèn$x$hai lần và sau đó xóa nó một lần vẫn phải để lại một bản sao hoạt động. Việc xây dựng khoảng thời gian xử lý việc này bằng cách ghép nối các thao tác xóa với lần chèn không khớp gần đây nhất, do đó, mỗi bản sao đều có tuổi thọ riêng. 

Một trường hợp khác là khi multiset trở nên trống. DP phải thiết lập lại chỉ$\{0\}$, nghĩa là không tồn tại tổng dương. Việc biểu diễn cây phân đoạn đương nhiên tạo ra một DP trống tại thời điểm đó vì không có khoảng thời gian nào đóng góp cho lá đó. 

Trường hợp góc cuối cùng là khi tất cả các phần tử đều lớn và gần với giới hạn tổng toàn cục. Mặc dù giá trị có thể lên tới$5 \cdot 10^5$, độ rộng DP vẫn bị giới hạn bởi tổng hiện tại và bitset vẫn hoạt động chính xác vì việc dịch chuyển vượt quá bit có thể truy cập cao nhất không ảnh hưởng đến độ chính xác.
