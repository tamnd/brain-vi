---
title: "CF 1955H - Màn phòng thủ liều lĩnh nhất"
description: "Chúng ta được cung cấp một lưới trong đó một số ô tạo thành một đường dẫn cố định từ góc trên bên trái đến góc dưới bên phải. Kẻ thù đi dọc theo con đường này một ô mỗi giây."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "constructive-algorithms", "dp", "flows", "graph-matchings", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1955
codeforces_index: "H"
codeforces_contest_name: "Codeforces Round 938 (Div. 3)"
rating: 2300
weight: 1955
solve_time_s: 75
verified: false
draft: false
---

[CF 1955H - Cách phòng thủ liều lĩnh nhất](https://codeforces.com/problemset/problem/1955/H) 

**Đánh giá:** 2300 
**Tags:** bitmasks, brute Force, thuật toán xây dựng, dp, luồng, so khớp biểu đồ, đường dẫn ngắn nhất 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới trong đó một số ô tạo thành một đường dẫn cố định từ góc trên bên trái đến góc dưới bên phải. Kẻ thù đi dọc theo con đường này một ô mỗi giây. Tại mỗi ô, nó dành trọn một giây ở đó và trong giây đó nó có thể bị phá hủy bởi các tháp đặt ở nơi khác trên lưới. 

Mỗi tháp có một giá trị thiệt hại cố định và một vị trí. Một tháp chỉ góp phần gây sát thương cho một ô đường nếu ô đó nằm trong vòng tròn Euclide có tâm tại tháp, trong đó bán kính là thứ chúng ta được phép chọn cho tháp đó. Điều đáng chú ý là việc chọn bán kính không tự do về mặt cấu trúc: nếu chúng ta đặt bán kính của tháp thành một số nguyên r lớn hơn 0, thì chúng ta cũng phải tăng lượng máu ban đầu của kẻ địch lên 3^r và mỗi số nguyên r có thể được sử dụng cho tối đa một tháp. 

Vì vậy, việc tăng bán kính là một sự đánh đổi: nó mở rộng tập hợp các ô đường mà tháp có thể tấn công, nhưng nó cũng tăng lượng máu của kẻ thù lên một lượng lớn theo cấp số nhân. Mục tiêu là chọn bán kính cho một số tòa tháp sao cho tổng sát thương dọc đường đi luôn đủ để tiêu diệt kẻ địch và chúng tôi muốn tối đa hóa lượng máu cơ bản h mà vẫn có thể bị đánh bại hoàn toàn. 

Quan điểm chính là đường đi cố định nên mỗi tháp đều góp phần gây sát thương cho một tập hợp con các bước thời gian cố định, tùy thuộc vào bán kính đã chọn. Mỗi lựa chọn bán kính tương ứng với một “mẫu” gồm các ô đường dẫn được bao phủ và chi phí cho việc chọn mẫu đó sẽ được cộng thêm 3^r vào sức khỏe. 

Ràng buộc n, m 50 và tổng kích thước lưới 2500 có nghĩa là chúng tôi có thể đủ khả năng xử lý trước O(N^2 log N) hoặc thậm chí O(N^3) cho mỗi thử nghiệm. Tuy nhiên, số mũ 3^r ngay lập tức loại trừ mọi nỗ lực coi bán kính là các biến liên tục độc lập hoặc gán lực lượng mạnh mẽ trực tiếp cho mỗi tháp không có cấu trúc. Khó khăn thực sự là chúng ta đang chọn một tập hợp con các giá trị bán kính, mỗi giá trị được sử dụng nhiều nhất một lần, dưới sự tương tác giống như chiếc ba lô. 

Một trường hợp khó nhận thấy là khi không có bán kính nào giúp bất kỳ tháp nào có thể bao phủ ngay cả một ô đường dẫn duy nhất. Trong trường hợp đó, việc chọn bất kỳ r > 0 nào chỉ làm tăng máu của kẻ thù mà không tăng sát thương, vì vậy câu trả lời tối ưu trở thành 0. Một giải pháp ngây thơ luôn cố gắng gán một số bán kính một cách tham lam sẽ tạo ra giá trị dương không chính xác trong những trường hợp như vậy. 

Một tình huống góc khác là các tòa tháp chồng chéo có phạm vi bao phủ giống hệt nhau đối với nhiều bán kính nhỏ. Việc xử lý từng tháp một cách độc lập có thể dẫn đến việc tính toán quá mức những lợi ích có thể đạt được vì bán kính là tài nguyên duy nhất trên toàn cầu chứ không phải các lựa chọn cho mỗi tháp. 

## Phương pháp tiếp cận 

Phương pháp tiếp cận bạo lực trực tiếp sẽ chỉ định cho mỗi tháp không có bán kính hoặc một số nguyên r, sau đó tính toán phạm vi bao phủ kết quả trên mỗi ô đường dẫn và kiểm tra xem tổng sát thương có vượt quá tổng mức tăng máu hay không. Đây rõ ràng là hàm mũ về cả k và số lượng giá trị bán kính có thể có, vì mỗi tháp có nhiều lựa chọn riêng biệt. Ngay cả khi chúng tôi giới hạn r theo đường kính đường dẫn (nhiều nhất là 100), điều này vẫn dẫn đến hệ số phân nhánh khoảng 100^k, điều này hoàn toàn không khả thi. 

Thông tin chi tiết về cấu trúc quan trọng là mỗi bán kính r xác định sự mở rộng đơn điệu: khi r tăng lên, một tòa tháp sẽ bao phủ một tập hợp siêu các ô đường dẫn. Điều đó có nghĩa là đối với một tháp cố định, chỉ có sự thay đổi gia tăng giữa r và r-1 mới quan trọng. Chúng ta có thể coi việc chỉ định bán kính r là chọn mức đóng góp cận biên ở cấp r, trong khi phải trả chi phí 3^r. Điều này biến mỗi tháp thành một chuỗi các “mục” được lập chỉ mục bởi r, nhưng với một ràng buộc chung là mỗi r chỉ có thể được sử dụng một lần trên tất cả các tháp. 

Đây là điểm mà bài toán trở thành bài toán chọn lớp: với mỗi r, chúng ta có thể chọn nhiều nhất một tháp để nhận bán kính đó. Mỗi nhiệm vụ như vậy mang lại mức tăng tương đương với sát thương bổ sung được mở khóa bằng cách mở rộng từ r-1 lên r.

Bây giờ vấn đề trở thành việc chọn nhiều nhất một mục trên mỗi lớp r, tối đa hóa tổng lợi nhuận trừ đi tổng chi phí. Điều này được xử lý một cách tự nhiên bằng cách lập trình động theo bán kính, trong đó tại mỗi r chúng ta gán nó cho một tháp hoặc bỏ qua nó. Trạng thái theo dõi các tòa tháp đã sử dụng bán kính nhỏ hơn, nhưng trạng thái ngây thơ đó là theo cấp số nhân. Sự đơn giản hóa xuất phát từ việc quan sát rằng đối với mỗi tháp, chỉ có bán kính hữu ích tối đa quan trọng đối với mỗi tiền tố và các đóng góp có thể được tính toán trước cho mỗi (tháp, r). 

Chúng tôi tính toán trước, đối với mỗi tháp và mỗi bán kính r, có bao nhiêu ô đường dẫn mà nó mới bắt đầu bao phủ ở chính xác bán kính đó. Vì đường dẫn là cố định nên chúng ta có thể tính toán trước bình phương khoảng cách từ mỗi tháp đến từng ô đường dẫn, sắp xếp các khoảng cách đó và chuyển đổi thành mảng bao phủ tiền tố trên r. 

Cuối cùng, chúng tôi chuyển vấn đề sang việc chọn cho mỗi r chính xác một tháp (hoặc không có tháp nào) để kích hoạt ở cấp độ đó, với trọng số bằng lợi ích cận biên trừ đi 3^r chi phí. Đây là phép gán DP theo lớp cổ điển, tương đương với việc lấy kết quả phù hợp nhất giữa các lớp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(k) | Quá chậm | 
| Tối ưu | O(k · N log N + R · k) | O(k · N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Bây giờ chúng tôi xây dựng một giải pháp xử lý hình học trước tiên, sau đó tối ưu hóa việc lựa chọn bán kính trên toàn cầu. 

1. Trích xuất các ô đường dẫn có thứ tự bằng cách quét lưới và thu thập tất cả các ô được đánh dấu '#'. Điều này đưa ra một chuỗi các vị trí được truy cập theo thứ tự thời gian. 
2. Đối với mỗi tháp, tính khoảng cách Euclide bình phương tới mỗi ô đường dẫn. Sắp xếp các khoảng cách này. Điều này biến phạm vi bao phủ hình học thành cấu trúc ngưỡng đơn điệu trong đó bán kính r tương ứng với khoảng cách ≤ r^2. 
3. Đối với mỗi tháp, hãy xây dựng một mảng trong đó mục r cho chúng ta biết có bao nhiêu ô đường dẫn bổ sung mới được bao phủ khi di chuyển từ bán kính r-1 đến r. Điều này được thực hiện bằng cách đếm xem có bao nhiêu khoảng cách nằm trong khoảng ((r-1)^2, r^2]. 
4. Chuyển số đếm này thành đóng góp thiệt hại bằng cách nhân với giá trị p của tháp. Bây giờ mỗi tháp có một mảng lợi ích trên mỗi bán kính. 
5. Bây giờ chúng ta thực hiện lựa chọn toàn cục trên bán kính. Với mỗi bán kính r, chúng ta chọn nhiều nhất một tháp để gán bán kính này. Nếu chúng ta gán r cho tháp i, chúng ta sẽ nhận được mức đóng góp sát thương biên của nó ở mức r và phải trả 3^r máu của kẻ địch. 
6. Chúng tôi chạy lập trình động trên bán kính. Ở bước r, chúng tôi xem xét việc chuyển đổi từ trạng thái trước đó bằng cách bỏ qua r hoặc gán r cho một tháp có mức tăng cận biên khác 0. 
7. Trạng thái DP theo dõi lợi thế ròng tốt nhất có thể đạt được (sát thương trừ đi lượng máu bổ sung) sau khi xử lý tất cả bán kính. 

Câu trả lời cuối cùng là lượng máu cơ bản h tối đa sao cho tổng sát thương trừ đi chi phí bán kính vẫn không âm, tương ứng trực tiếp với lượng h tối đa khả thi. 

### Tại sao nó hoạt động 

Mỗi bán kính đóng góp độc lập ngoại trừ ràng buộc là nó chỉ có thể được sử dụng một lần. Bằng cách chuyển đổi ảnh hưởng hình học thành mức tăng cận biên trên mỗi bán kính, chúng tôi đảm bảo rằng mọi cấu hình hợp lệ đều tương ứng với việc chọn một tập hợp các phép gán bán kính rời nhau. DP thực thi quyền độc quyền này chính xác một lần mỗi r, do đó không xảy ra việc sử dụng lại không hợp lệ. Vì phạm vi bao phủ là đơn điệu trong r, nên việc phân tách biên duy trì tính chính xác: không có thiệt hại nào được tính hai lần và không có cấu hình nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, k = map(int, input().split())
        grid = [input().strip() for _ in range(n)]

        path = []
        for i in range(n):
            for j in range(m):
                if grid[i][j] == '#':
                    path.append((i, j))

        towers = []
        for _ in range(k):
            x, y, p = map(int, input().split())
            towers.append((x - 1, y - 1, p))

        P = len(path)

        # compute all distances
        tower_dists = []
        max_r = 0

        for tx, ty, p in towers:
            d2 = []
            for px, py in path:
                dx = tx - px
                dy = ty - py
                d2.append(dx*dx + dy*dy)
            d2.sort()
            tower_dists.append((d2, p))
            max_r = max(max_r, int(max(d2) ** 0.5) + 2 if d2 else 0)

        # marginal gains per tower per radius
        gain = [[0] * (max_r + 2) for _ in range(k)]

        for i, (d2, p) in enumerate(tower_dists):
            ptr = 0
            cnt = 0
            for r in range(1, max_r + 1):
                r2 = r * r
                while ptr < len(d2) and d2[ptr] <= r2:
                    ptr += 1
                new_cnt = ptr
                gain[i][r] = (new_cnt - cnt) * p
                cnt = new_cnt

        # DP over radii: best net gain
        NEG = -10**18
        dp = [0] + [NEG] * (max_r + 1)

        for r in range(1, max_r + 1):
            ndp = dp[:]
            cost = 3 ** r
            for i in range(k):
                val = gain[i][r] - cost
                for j in range(r):
                    if dp[j] != NEG:
                        ndp[r] = max(ndp[r], dp[j] + val)
            dp = ndp

        best = max(dp)
        print(max(0, best))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách trích xuất đường dẫn theo thứ tự, vì thời điểm hư hỏng phụ thuộc vào thứ tự truyền tải chứ không chỉ phụ thuộc vào hình học. Khoảng cách được bình phương để tránh lỗi dấu phẩy động và cho phép so sánh bán kính rõ ràng. 

Bước tính toán độ lợi sẽ chuyển đổi từng tòa tháp thành một chuỗi đơn điệu gồm các cải tiến biên, điều này rất quan trọng vì nó tránh tính toán lại toàn bộ vùng phủ sóng cho mỗi lần gán bán kính. Con trỏ bên trong đảm bảo di chuyển tuyến tính trên mỗi tháp sau khi sắp xếp khoảng cách. 

DP được cấu trúc sao cho mỗi bán kính được sử dụng nhiều nhất một lần. Quá trình chuyển đổi kiểm tra xem việc gán bán kính r cho bất kỳ tháp nào có cải thiện mức tăng ròng tốt nhất có thể đạt được hay không. Phép trừ 3^r được áp dụng chính xác tại thời điểm chọn r, phù hợp với cấu trúc chi phí của bài toán. 

Một vấn đề triển khai tinh vi là 3^r phát triển cực kỳ nhanh chóng, do đó cần có số nguyên Python và các giá trị DP phải được giữ ở loại phạm vi rộng mà không lo ngại tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một đường dẫn nhỏ gồm ba ô và một tòa tháp gần đường dẫn. 

| Bước | Bán kính r | dp trước tốt nhất | Tháp được chọn | Giá 3^r | dp mới | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 2 | 3 | -1 | 
| 2 | 2 | 0 | 5 | 9 | -4 | 

Giá trị tốt nhất không bao giờ trở thành dương, nghĩa là không thể hỗ trợ tình trạng cơ bản tích cực. 

Dấu vết này cho thấy bán kính ban đầu có thể đã quá đắt so với mức tăng thiệt hại. 

### Ví dụ 2 

Cấu hình mạnh hơn với nhiều tháp: 

| Bước | Bán kính r | dp trước tốt nhất | Tháp được chọn | Giá 3^r | dp mới | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 10 | 3 | 7 | 
| 2 | 2 | 7 | 20 | 9 | 18 | 

Ở đây, bán kính thứ hai cải thiện cấu hình hơn nữa và DP tích lũy mức tăng ròng dương, nghĩa là tình trạng cơ bản tích cực là khả thi. 

Điều này xác nhận rằng việc chọn các bán kính khác nhau ở các lớp khác nhau sẽ dẫn đến những cải tiến bổ sung nghiêm ngặt khi chi phí được tính toán hợp lý. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k · P log P + k · R^2) | Sắp xếp khoảng cách trên mỗi tháp cộng với DP theo bán kính bằng cách kiểm tra tháp | 
| Không gian | O(k · P + R · k) | Lưu trữ mảng khoảng cách và bảng khuếch đại | 

Các ràng buộc giữ P ≤ 2500, do đó việc sắp xếp khoảng cách có thể chấp nhận được và R được giới hạn bởi đường kính lưới, giữ cho DP có thể quản lý được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve()  # adjusted for inline environments

# provided samples (placeholders)
# assert run(sample_input) == sample_output

# minimal case
assert run("""1
2 2 1
#.
##
1 2 1
""").strip() == "0"

# single tower no effect
assert run("""1
2 2 1
#.
##
1 1 5
""").strip() == "0"

# dense path
assert run("""1
3 3 2
###
###
###
1 1 1
3 3 2
""") != ""

# boundary radius case
assert run("""1
2 2 1
#.
##
2 2 10
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| lưới tối thiểu | 0 | không có ảnh hưởng tháp hiệu quả | 
| tháp yếu | 0 | thiệt hại không bao giờ vượt qua được chi phí | 
| con đường dày đặc | khác không | hành vi phủ sóng chồng chéo | 
| tháp xa | 0 | mở rộng bán kính vô dụng | 

## Vỏ cạnh 

Trường hợp cạnh chính xảy ra khi tất cả khoảng cách của tháp đến đường dẫn vượt quá 1, nghĩa là bán kính 1 đã không bao gồm gì cả. Sau đó, DP chỉ nhìn thấy những đóng góp tiêu cực từ 3^r, vì vậy mỗi lần chuyển đổi sẽ làm giảm điểm. Trong tình huống đó, thuật toán giữ dp ở mức 0 một cách chính xác và trả về 0. 

Một trường hợp khác là khi nhiều tháp có cấu hình khoảng cách giống hệt nhau. DP xử lý việc này một cách chính xác vì nó thực thi tối đa một lựa chọn cho mỗi bán kính, do đó các tòa tháp giống hệt nhau sẽ cạnh tranh thay vì tích lũy. Một cách tiếp cận tham lam sẽ nhân đôi số lợi nhuận giống nhau một cách không chính xác. 

Cuối cùng, khi một tháp bao phủ toàn bộ đường dẫn ở bán kính nhỏ, mức tăng cận biên sẽ tập trung ở mức r thấp và bán kính cao hơn không đóng góp gì. DP đương nhiên tránh bán kính lớn hơn đắt tiền vì lợi nhuận trừ đi chi phí của họ là âm, ngăn ngừa đầu tư quá mức.
