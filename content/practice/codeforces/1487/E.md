---
title: "CF 1487E - Bữa Tối Giá Rẻ"
description: "Chúng tôi có bốn lớp vật phẩm. Lớp đầu tiên chứa các món đầu tiên, lớp thứ hai chứa các món thứ hai, lớp thứ ba chứa đồ uống và lớp thứ tư chứa các món tráng miệng. Mỗi mặt hàng đều có chi phí. Giữa mỗi cặp lớp liền kề, một số sự kết hợp bị cấm."
date: "2026-06-10T23:01:56+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "graphs", "greedy", "implementation", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1487
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 104 (Rated for Div. 2)"
rating: 2000
weight: 1487
solve_time_s: 144
verified: true
draft: false
---

[CF 1487E - Bữa tối giá rẻ](https://codeforces.com/problemset/problem/1487/E) 

**Xếp hạng:** 2000 
**Tags:** brute power, cấu trúc dữ liệu, đồ thị, tham lam, triển khai, sắp xếp, hai con trỏ 
**Thời gian giải:** 2m 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có bốn lớp vật phẩm. 

Lớp đầu tiên chứa các món đầu tiên, lớp thứ hai chứa các món thứ hai, lớp thứ ba chứa đồ uống và lớp thứ tư chứa các món tráng miệng. Mỗi mặt hàng đều có chi phí. 

Giữa mỗi cặp lớp liền kề, một số sự kết hợp bị cấm. Món đầu tiên có thể không tương thích với một số món thứ hai, món thứ hai có thể không tương thích với một số đồ uống nhất định và đồ uống có thể không tương thích với một số món tráng miệng nhất định. 

Chúng ta phải chọn chính xác một mục từ mỗi lớp. Mỗi cặp liền kề trong chuỗi đã chọn phải được cho phép. Trong số tất cả các bữa tối hợp lệ, chúng tôi muốn tổng chi phí tối thiểu có thể. 

Cách tự nhiên nhất để xem vấn đề là dưới dạng biểu đồ xếp lớp. Mỗi mục là một đỉnh. Các cạnh chỉ tồn tại giữa các lớp lân cận và một số cạnh đó bị loại bỏ vì chúng bị cấm. Chúng ta cần đường đi rẻ nhất bắt đầu ở lớp 1 và kết thúc ở lớp 4, trong đó chi phí của một đường đi là tổng chi phí của các đỉnh của nó. 

Các ràng buộc hoàn toàn xác định hướng đi của giải pháp. Mỗi lớp có thể chứa tối đa 150.000 mục, trong khi mỗi bộ cặp bị cấm chứa tối đa 200.000 mục. Một thuật toán bậc hai giữa các lớp là không thể. Ngay cả việc kiểm tra tất cả các cặp giữa hai lớp cũng sẽ yêu cầu khoảng$150000^2$hoạt động vượt xa giới hạn. 

Một quan sát hữu ích là số lượng cặp bị cấm tương đối nhỏ so với số lượng cặp có thể có. Đầu vào không mô tả tất cả các kết nối được phép. Thay vào đó, nó chỉ liệt kê những cái xấu. Điều đó gợi ý mạnh mẽ việc xây dựng một giải pháp xung quanh các tập hợp bị cấm thay vì xung quanh tất cả các cạnh có thể. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét một lớp trong đó mọi mục đều không thể truy cập được.```
Layer A costs: [1]
Layer B costs: [2]
Forbidden: (A1,B1)
```Không có chuyển đổi hợp lệ tồn tại. Giá trị chính xác cho mục ở lớp B là "không thể". Nếu chúng tôi vô tình giữ nguyên giá gốc thì các lớp sau có thể xây dựng giải pháp thông qua đó một cách không chính xác. 

Một trường hợp tinh tế khác xảy ra khi sản phẩm tiền thân rẻ nhất bị cấm nhưng lại có sẵn sản phẩm đắt tiền hơn.```
Previous layer values: [1, 5]
Current item forbids predecessor 1
```Câu trả lời cho hạng mục này là 5 cộng với chi phí của chính nó chứ không phải là vô cùng. Một giải pháp chỉ kiểm tra người tiền nhiệm rẻ nhất toàn cầu sẽ thất bại. 

Trường hợp quan trọng thứ ba là khi mọi vật trước đó đều bị cấm.```
Previous layer values: [3, 7]
Forbidden predecessors: {1, 2}
```Mục này phải trở nên không thể truy cập được. Bất kỳ triển khai nào quên phát hiện tình huống này có thể vô tình sử dụng lại giá trị không hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ thử mọi bữa tối có thể. 

Đối với mỗi khóa học đầu tiên, hãy thử mỗi khóa học thứ hai. Đối với mỗi cặp hợp lệ, hãy thử mọi đồ uống. Đối với mỗi bộ ba hợp lệ, hãy thử mọi món tráng miệng. Bất cứ khi nào tất cả các cặp liền kề được cho phép, hãy tính tổng chi phí và giữ mức tối thiểu. 

Cách tiếp cận này đúng vì nó kiểm tra rõ ràng mọi bữa tối hợp lệ. Thật không may, sự phức tạp của nó$$O(n_1 n_2 n_3 n_4)$$có thể lớn như$$150000^4$$và hoàn toàn không thể thực hiện được. 

Chúng ta cần khai thác cấu trúc của vấn đề. 

Giả sử chúng ta đã biết chi phí thấp nhất có thể đạt được cho mỗi hạng mục trong một lớp. Chúng tôi muốn tính toán chi phí thấp nhất có thể đạt được cho mỗi hạng mục ở lớp tiếp theo. 

Đối với mặt hàng cố định$v$ở lớp tiếp theo,$$dp[v] = cost[v] + \min(dp[u])$$trên tất cả những người tiền nhiệm$u$điều đó không bị cấm với$v$. 

Khó khăn là tìm ra mức tối thiểu đó một cách hiệu quả. 

Việc triển khai đơn giản sẽ quét tất cả các phiên bản trước để tìm mọi mục. Điều đó một lần nữa trở thành bậc hai. 

Quan sát quan trọng là mỗi mục chỉ cấm một số lượng tương đối nhỏ các mục trước đó. Nếu chúng ta sắp xếp tất cả các giá trị tiền thân theo chi phí thì đối với một mặt hàng nhất định, chúng ta có thể duyệt qua danh sách được sắp xếp từ rẻ nhất đến đắt nhất và chọn giá trị tiền thân đầu tiên không bị cấm. 

Tại sao điều này lại hiệu quả? 

Mỗi vật phẩm chỉ lưu trữ những vật phẩm trước đó bị cấm. Số lượng cặp bị cấm tối đa là 200.000. Vì danh sách tiền thân được sắp xếp được chia sẻ bởi tất cả các mục nên việc kiểm tra xem ứng cử viên tiền nhiệm có bị cấm hay không chỉ là tra cứu tập hợp băm. 

Đối với mỗi mục, chúng tôi tìm kiếm danh sách tiền thân được sắp xếp cho đến khi tìm thấy tiền thân được phép đầu tiên. Vì tổng số cặp bị cấm là nhỏ nên quá trình này diễn ra đủ nhanh. 

Chúng tôi thực hiện chuyển đổi này ba lần: 

1. Khóa đầu tiên → khóa thứ hai. 
2. Món thứ hai → đồ uống. 
3. Đồ uống → món tráng miệng. 

Sau lần chuyển đổi cuối cùng, câu trả lời là giá trị tối thiểu có thể đạt được trong lớp tráng miệng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n_1 n_2 n_3 n_4)$|$O(1)$| Quá chậm | 
| Tối ưu |$O((N+M)\log N)$|$O(N+M)$| Đã chấp nhận | 

Đây$N=n_1+n_2+n_3+n_4$Và$M=m_1+m_2+m_3$. 

## Hướng dẫn thuật toán 

Giả sử chúng ta biết chi phí tốt nhất có thể đạt được cho tất cả các hạng mục ở lớp trước. 

Chúng ta cần một quy trình tính toán chi phí tốt nhất có thể đạt được cho lớp tiếp theo. 

### Thủ tục chuyển tiếp 

1. Sắp xếp tất cả các mục của lớp trước theo giá trị DP hiện tại của chúng. 
2. Đối với mỗi mục trong lớp hiện tại, hãy lưu trữ tập hợp các mục trước đó bị cấm. 
3. Xử lý từng mục của lớp hiện tại một cách độc lập. 
4. Quét danh sách tiền thân được sắp xếp từ rẻ nhất đến đắt nhất. 
5. Tìm tiền thân đầu tiên có chỉ mục không có trong tập bị cấm của mục hiện tại. 
6. Nếu sản phẩm trước đó tồn tại, hãy cộng giá trị DP của nó vào giá của sản phẩm hiện tại. 
7. Nếu không có mục trước đó tồn tại, hãy đánh dấu mục hiện tại là không thể truy cập được bằng cách sử dụng giá trị rất lớn. 

### Thuật toán đầy đủ 

1. Khởi tạo giá trị DP của lớp đầu tiên theo chi phí của chúng. 
2. Áp dụng quy trình chuyển đổi từ lớp 1 sang lớp 2. 
3. Áp dụng quy trình chuyển đổi từ lớp 2 sang lớp 3. 
4. Áp dụng quy trình chuyển đổi từ lớp 3 sang lớp 4. 
5. Lấy giá trị DP tối thiểu ở lớp thứ tư. 
6. Nếu giá trị tối thiểu là vô cùng, xuất ra`-1`. Nếu không thì xuất ra mức tối thiểu đó. 

### Tại sao nó hoạt động 

Đối với bất kỳ món nào trong lớp hiện tại, mọi bữa tối hợp lệ kết thúc tại món đó đều phải thông qua một số món ăn trước đó được phép. 

Danh sách tiền thân được sắp xếp kiểm tra các tiền thân theo thứ tự DP tăng dần. Người tiền nhiệm đầu tiên không bị cấm chính xác là người tiền nhiệm được phép có giá trị DP tối thiểu. 

Do đó quá trình chuyển đổi tính toán$$cost[v]+\min_{\text{allowed }u} dp[u]$$đó chính xác là giá trị tối ưu cho mặt hàng$v$. 

Vì mỗi lớp được tính toán từ các giá trị chính xác của lớp trước đó nên quy nạp cho thấy tất cả các giá trị DP vẫn ở mức tối ưu. Mức tối thiểu cuối cùng trong số các nút tráng miệng chính xác là bữa tối hợp lệ rẻ nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**18

n1, n2, n3, n4 = map(int, input().split())

a = list(map(int, input().split()))
b = list(map(int, input().split()))
c = list(map(int, input().split()))
d = list(map(int, input().split()))

def build(prev_dp, cur_costs):
    m = int(input())

    bad = [set() for _ in range(len(cur_costs))]
    for _ in range(m):
        x, y = map(int, input().split())
        bad[y - 1].add(x - 1)

    order = sorted(range(len(prev_dp)), key=lambda i: prev_dp[i])

    res = [INF] * len(cur_costs)

    for i in range(len(cur_costs)):
        for p in order:
            if p not in bad[i]:
                if prev_dp[p] < INF:
                    res[i] = cur_costs[i] + prev_dp[p]
                break

    return res

dp2 = build(a, b)
dp3 = build(dp2, c)
dp4 = build(dp3, d)

ans = min(dp4)

print(-1 if ans >= INF else ans)
```Việc thực hiện tuân theo công thức lập trình động trực tiếp. 

Mảng được truyền dưới dạng`prev_dp`đã chứa chi phí tối thiểu có thể đạt được cho mỗi mục ở lớp trước. các`build`tính toán các giá trị DP của lớp tiếp theo. 

Mỗi mục của lớp hiện tại lưu trữ các mục trước đó bị cấm trong một tập hợp băm. Kiểm tra tư cách thành viên trở thành$O(1)$trung bình. 

Mảng đã sắp xếp`order`là sự tối ưu hóa quan trọng. Nó liệt kê các chỉ số tiền thân từ giá trị DP nhỏ nhất đến lớn nhất. Khi xử lý một mục hiện tại, chúng tôi tìm kiếm danh sách này cho đến khi gặp mục trước đó không bị cấm. Vì danh sách đã được sắp xếp nên danh sách trước đó tự động là danh sách được phép rẻ nhất. 

Séc`prev_dp[p] < INF`ngăn chặn các trạng thái không thể truy cập được sử dụng làm tiền thân hợp lệ. 

Các chỉ số trong đầu vào dựa trên 1, trong khi danh sách Python dựa trên 0. Việc chuyển đổi được thực hiện ngay lập tức khi đọc các cặp bị cấm. 

giá trị`INF = 10**18`vượt quá mọi câu trả lời hợp lệ có thể một cách an toàn. Chi phí bữa tối lớn nhất nhiều nhất là$4 \times 10^8$, do đó không có nguy cơ nhầm lẫn giữa trạng thái có thể truy cập và không thể truy cập. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 3 2 1
1 2 3 4
5 6 7
8 9
10
...
```Sau khi đọc lớp đầu tiên: 

| Mục | DP | 
| --- | --- | 
| A1 | 1 | 
| A2 | 2 | 
| A3 | 3 | 
| A4 | 4 | 

Chuyển sang khóa học thứ hai: 

| Khóa thứ hai | Những khóa học đầu tiên bị cấm | Người tiền nhiệm được phép rẻ nhất | DP | 
| --- | --- | --- | --- | 
| B1 | {1} | A2 (2) | 7 | 
| B2 | {1} | A2 (2) | 8 | 
| B3 | {} | A1 (1) | 8 | 

Chuyển sang đồ uống: 

| Uống | Khóa học thứ hai bị cấm | Người tiền nhiệm được phép rẻ nhất | DP | 
| --- | --- | --- | --- | 
| C1 | {3} | B1 (7) | 15 | 
| C2 | {3} | B1 (7) | 16 | 

Chuyển sang món tráng miệng: 

| Món tráng miệng | Đồ uống bị cấm | Người tiền nhiệm được phép rẻ nhất | DP | 
| --- | --- | --- | --- | 
| D1 | {1} | C2 (16) | 26 | 

Câu trả lời cuối cùng: **26**. 

Dấu vết này cho thấy ý tưởng trung tâm. Bất cứ khi nào tiền thân rẻ nhất bị cấm, thuật toán sẽ tự động chuyển sang tiền thân hợp lệ rẻ nhất tiếp theo. 

### Ví dụ không thể thực hiện được```
1 1 1 1
1
2
3
4
1
1 1
0
0
```Chuyển tiếp đầu tiên: 

| Khóa thứ hai | Người tiền nhiệm bị cấm | Kết quả | 
| --- | --- | --- | 
| B1 | {A1} | INF | 

Chuyển tiếp thứ hai: 

| Uống | Người tiền nhiệm rẻ nhất | 
| --- | --- | 
| C1 | không thể truy cập được | 

Kết quả vẫn còn`INF`. 

Chuyển tiếp thứ ba: 

| Món tráng miệng | Người tiền nhiệm rẻ nhất | 
| --- | --- | 
| D1 | không thể truy cập được | 

Kết quả vẫn còn`INF`. 

Giá trị nhỏ nhất ở lớp cuối cùng vẫn là vô cùng nên đáp án là`-1`. 

Ví dụ này thể hiện sự lan truyền của các trạng thái không thể truy cập được qua các lớp sau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((N+M)\log N)$| Sắp xếp từng lớp chiếm ưu thế, cấm kiểm tra là tra cứu băm | 
| Không gian |$O(N+M)$| Mảng DP cộng với bộ cặp cấm | 

Tổng số vật phẩm tối đa là 600.000 và tổng số cặp bị cấm tối đa là 600.000. Thuật toán thực hiện ba thao tác sắp xếp và lưu trữ các quan hệ bị cấm một cách rõ ràng. Cả thời gian và trí nhớ đều thoải mái nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    INF = 10**18

    n1, n2, n3, n4 = map(int, input().split())

    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    c = list(map(int, input().split()))
    d = list(map(int, input().split()))

    def build(prev_dp, cur_costs):
        m = int(input())
        bad = [set() for _ in range(len(cur_costs))]

        for _ in range(m):
            x, y = map(int, input().split())
            bad[y - 1].add(x - 1)

        order = sorted(range(len(prev_dp)), key=lambda i: prev_dp[i])

        res = [INF] * len(cur_costs)

        for i in range(len(cur_costs)):
            for p in order:
                if p not in bad[i]:
                    if prev_dp[p] < INF:
                        res[i] = cur_costs[i] + prev_dp[p]
                    break

        return res

    dp2 = build(a, b)
    dp3 = build(dp2, c)
    dp4 = build(dp3, d)

    ans = min(dp4)
    return str(-1 if ans >= INF else ans)

# custom minimum case
assert run(
"""1 1 1 1
1
2
3
4
0
0
0
"""
) == "10"

# impossible immediately
assert run(
"""1 1 1 1
1
2
3
4
1
1 1
0
0
"""
) == "-1"

# all equal costs
assert run(
"""2 2 2 2
5 5
5 5
5 5
5 5
0
0
0
"""
) == "20"

# cheapest predecessor forbidden
assert run(
"""2 1 1 1
1 100
5
7
9
1
1 1
0
0
"""
) == "121"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mục duy nhất trong mỗi lớp | 10 | Phiên bản hợp lệ có kích thước tối thiểu | 
| Chỉ kết nối bị cấm | -1 | Tuyên truyền không thể truy cập | 
| Mọi chi phí đều bằng nhau | 20 | Nhiều giải pháp tối ưu | 
| Người tiền nhiệm rẻ nhất bị cấm | 121 | Dự phòng chính xác cho người tiền nhiệm tốt thứ hai | 

## Vỏ cạnh 

Hãy xem xét trường hợp một mục cấm mọi mục trước đó.```
Previous DP: [3, 7]
Forbidden predecessors: {1, 2}
```Thứ tự sắp xếp trước đó là`[3, 7]`. Thuật toán kiểm tra cả hai mục, tìm thấy cả hai mục bị cấm và không bao giờ cập nhật kết quả. Giá trị của mặt hàng vẫn còn`INF`, đánh dấu chính xác là không thể truy cập được. 

Hãy xem xét trường hợp tiền thân rẻ nhất bị cấm nhưng tồn tại một tiền thân hợp lệ khác.```
Previous DP: [1, 5, 10]
Forbidden predecessors: {1}
```Thuật toán kiểm tra giá trị DP`1`, từ chối vì nó bị cấm, sau đó kiểm tra giá trị DP`5`và chấp nhận nó. Quá trình chuyển đổi kết quả sử dụng chi phí`5`, đây chính xác là mức tối thiểu trong số tất cả những người tiền nhiệm được phép. 

Hãy xem xét trường hợp tất cả các mục trong toàn bộ lớp không thể truy cập được.```
Layer 1: [1]
Layer 2: [2]
Forbidden: (1,1)
```Mục duy nhất ở lớp 2 nhận được giá trị`INF`. Khi các chuyển đổi sau này được tính toán, điều kiện`prev_dp[p] < INF`ngăn không cho mục không thể truy cập này được sử dụng. Sự bất khả thi lan truyền một cách chính xác đến câu trả lời cuối cùng, trở thành`-1`.
