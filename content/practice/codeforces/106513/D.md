---
title: "CF 106513D - Cao tốc"
description: "Chúng ta có một tập hợp các thành phố được kết nối bằng các đường cao tốc tiềm năng, mỗi đường cao tốc có chi phí xây dựng cố định. Những con đường này đã tạo thành một đồ thị vô hướng được kết nối, vì vậy chúng ta biết rằng cây bao trùm luôn tồn tại bằng cách sử dụng một số tập hợp con của các cạnh này."
date: "2026-06-18T19:05:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106513
codeforces_index: "D"
codeforces_contest_name: "2026 Spring UT CS104c Final Exam"
rating: 0
weight: 106513
solve_time_s: 110
verified: true
draft: false
---

[CF 106513D - Đường cao tốc](https://codeforces.com/problemset/problem/106513/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các thành phố được kết nối bằng các đường cao tốc tiềm năng, mỗi đường cao tốc có chi phí xây dựng cố định. Những con đường này đã tạo thành một đồ thị vô hướng được kết nối, vì vậy chúng ta biết rằng cây bao trùm luôn tồn tại bằng cách sử dụng một số tập hợp con của các cạnh này. 

Trong số tất cả các đường cao tốc được đề xuất, bắt buộc phải có một cạnh cụ thể: nó phải xuất hiện trong mạng được kết nối cuối cùng bất kể thế nào. Nhiệm vụ là xây dựng một bộ đường có chi phí tối thiểu vẫn kết nối tất cả các thành phố trong khi bao gồm cả phần bắt buộc này. 

Trong thuật ngữ đồ thị, đây là bài toán cây bao trùm tối thiểu với một ràng buộc bổ sung là một cạnh được chọn trước và phải thuộc về cây bao trùm. 

Kích thước đầu vào làm rõ lý do tại sao điều này là không tầm thường. Với tối đa$10^5$đỉnh và$2 \cdot 10^5$các cạnh, bất kỳ giải pháp nào cố gắng xem xét tất cả các tập hợp con hoặc tính toán lại cây bao trùm từ đầu cho mỗi lần sửa đổi cạnh sẽ quá chậm. Một phép tính lại đơn giản của cây bao trùm sau khi ép từng cạnh có thể sẽ nhân chi phí của Kruskal hoặc Prim với$m$, vượt xa giới hạn khả thi. 

Một điểm tinh tế là việc ép buộc một cạnh thực sự có thể làm cho giải pháp tối ưu trở nên tồi tệ hơn MST toàn cầu, bởi vì nó có thể tạo ra một chu kỳ trong đó cạnh đó là thành viên đắt giá nhất. Việc triển khai bất cẩn chỉ đơn giản là xây dựng một MST bình thường và sau đó “thêm” cạnh bắt buộc sẽ là sai lầm. 

Trong trường hợp lỗi cụ thể, hãy tưởng tượng ba nút tạo thành một hình tam giác có các cạnh:```
1 - 2 (cost 1)
2 - 3 (cost 2)
1 - 3 (cost 100)
```Nếu cạnh bắt buộc là$1-3$, câu trả lời đúng phải bao gồm nó, vì vậy chúng ta không thể chọn các cạnh MST tự nhiên. Kết quả trở thành 1-3 cộng với một trong các cạnh khác, tạo ra chi phí 101. Một MST ngây thơ sẽ cho 3, kết quả này không hợp lệ vì nó bỏ qua ràng buộc. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực sẽ là coi cạnh bắt buộc là cố định và sau đó tính toán lại cây bao trùm tối thiểu trên biểu đồ còn lại với các điểm cuối đã được kết nối. Về mặt khái niệm, chúng tôi “thu gọn” hai điểm cuối của cạnh cần thiết và chạy lại Kruskal. 

Điều này có tác dụng vì cây bao trùm chứa một cạnh cố định tương đương với cây bao trùm của đồ thị trong đó cạnh đó đã được chọn và các điểm cuối của nó được hợp nhất. Tuy nhiên, việc tính toán lại trực tiếp vẫn có vẻ tốn kém nếu được thực hiện nhiều lần hoặc không hiệu quả, vì bản thân Kruskal$O(m \log m)$. 

Quan sát quan trọng là chúng ta không cần phải xây dựng lại bất cứ thứ gì từ đầu theo một cách đặc biệt. Chúng ta có thể chỉ cần buộc cạnh vào cấu trúc DSU trước khi xử lý tất cả các cạnh khác. Khi các điểm cuối của cạnh bắt buộc được thống nhất, chúng ta tiến hành sắp xếp Kruskal tiêu chuẩn trên tất cả các cạnh. 

Điều này hiệu quả vì thuật toán của Kruskal xây dựng MST bằng cách luôn lấy cạnh nhỏ nhất kết nối hai thành phần khác nhau. Bằng cách liên kết trước các điểm cuối của cạnh được yêu cầu và thêm chi phí trả trước của nó, chúng tôi mô phỏng điều kiện là cạnh này đã được chọn và tất cả các quyết định sau này sẽ điều chỉnh một cách tự nhiên. 

Điều tinh tế duy nhất là cạnh bắt buộc phải được tính ngay cả khi nó sẽ bị Kruskal bỏ qua do hình thành một chu trình. Đó chính xác là những gì liên minh cưỡng bức đảm bảo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Tính toán lại MST sau khi buộc cạnh thông qua sự co lại của đồ thị |$O(m \log m)$mỗi lần xây dựng lại |$O(m)$| Quá chậm nếu lặp lại hoặc thực hiện một cách ngây thơ | 
| Kruskal có cạnh bao gồm sẵn |$O(m \log m)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các cạnh bằng cách tăng chi phí, giữ cho cạnh bắt buộc được xác định riêng biệt. Việc sắp xếp đảm bảo chúng tôi luôn xem xét các kết nối rẻ hơn trước tiên, đây là nền tảng cho tính đúng đắn của Kruskal. 
2. Khởi tạo cấu trúc DSU trong đó mỗi thành phố bắt đầu trong thành phần riêng của nó. Điều này theo dõi những nút nào đã được kết nối trong quá trình xây dựng. 
3. Hợp nhất ngay lập tức các điểm cuối của cạnh bắt buộc và cộng chi phí của nó vào câu trả lời. Bước này buộc cạnh vào cấu trúc trước bất kỳ quyết định nào khác, đảm bảo nó không bao giờ bị loại trừ sau này. 
4. Lặp lại tất cả các cạnh theo thứ tự được sắp xếp. Đối với mỗi cạnh, hãy kiểm tra xem điểm cuối của nó có thuộc các thành phần DSU khác nhau hay không. 
5. Nếu chúng nằm trong các thành phần khác nhau, hãy đưa cạnh vào câu trả lời và hợp các thành phần lại với nhau. Điều này duy trì kết nối trong khi tránh chu kỳ, chính xác như trong cấu trúc MST tiêu chuẩn. 
6. Nếu chúng đã được kết nối, hãy bỏ qua cạnh. Việc thêm nó sẽ tạo thành một chu trình và không thể giảm chi phí trong cây bao trùm. 

### Tại sao nó hoạt động 

Thuật toán của Kruskal dựa trên đặc tính là tại bất kỳ thời điểm nào, các cạnh được chọn sẽ tạo thành một khu rừng có chi phí tối thiểu trên tiền tố được xử lý của các cạnh. Bằng cách liên kết trước các điểm cuối của cạnh bắt buộc, chúng tôi đảm bảo rằng cạnh này là một phần của khu rừng ban đầu bất kể trọng lượng của nó. Từ thời điểm đó trở đi, mọi quyết định đều giống với việc chạy Kruskal trên biểu đồ trong đó hai đỉnh đó đã được kết nối với nhau, tương đương với việc buộc cạnh đó vào cấu trúc khung. Thuộc tính lựa chọn tham lam được giữ nguyên vì tất cả các cạnh còn lại vẫn được xử lý theo thứ tự tăng dần toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n + 1))
        self.size = [1] * (n + 1)

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, a, b):
        ra, rb = self.find(a), self.find(b)
        if ra == rb:
            return False
        if self.size[ra] < self.size[rb]:
            ra, rb = rb, ra
        self.parent[rb] = ra
        self.size[ra] += self.size[rb]
        return True

def main():
    n, m = map(int, input().split())
    j = int(input()) - 1

    edges = []
    forced = None

    for i in range(m):
        u, v, c = map(int, input().split())
        edges.append((c, u, v, i))
        if i == j:
            forced = (c, u, v)

    edges.sort()

    dsu = DSU(n)
    ans = 0

    fc, fu, fv = forced
    ans += fc
    dsu.union(fu, fv)

    for c, u, v, _ in edges:
        if dsu.union(u, v):
            ans += c

    print(ans)

if __name__ == "__main__":
    main()
```Việc triển khai DSU sử dụng tính năng nén đường dẫn thông qua việc giảm một nửa lặp đi lặp lại và kết hợp theo kích thước, giúp duy trì các hoạt động liên tục một cách hiệu quả. Chi tiết triển khai quan trọng là thêm chi phí cạnh bắt buộc trước bất kỳ quá trình xử lý nào, chứ không phải sau, vì nếu không nó có thể bị bỏ qua một cách không chính xác nếu nó tạo thành một chu trình trong cấu trúc cuối cùng. 

Bước sắp xếp đảm bảo trật tự tham lam toàn cầu của Kruskal được giữ nguyên. Kiểm tra hợp đảm bảo chúng ta không bao giờ tạo chu trình, điều này rất cần thiết vì cây bao trùm phải duy trì tính không tuần hoàn ngay cả khi một cạnh là bắt buộc. 

## Ví dụ đã hoạt động 

Hãy xem xét biểu đồ mẫu:```
n = 4, m = 5, forced = 3
edges:
1 2 10
2 3 5
3 4 50   (forced)
1 4 20
2 4 15
```Chúng tôi theo dõi trạng thái DSU. 

| Bước | Cạnh | Hành động | DSU sáp nhập | Chi phí | 
| --- | --- | --- | --- | --- | 
| ban đầu | buộc 3-4 | tiền đoàn | (3,4) | 50 | 
| 1 | 2-3 (5) | hợp nhất | (2,3,4) | 55 | 
| 2 | 1-2 (10) | hợp nhất | tất cả được kết nối | 65 | 
| 3 | 2-4 (15) | bỏ qua | đã kết nối | 65 | 
| 4 | 1-4 (20) | bỏ qua | đã kết nối | 65 | 

Dấu vết cho thấy sau khi ép cạnh, Kruskal tự nhiên lấp đầy cấu trúc còn lại bằng những kết nối rẻ nhất có thể. 

Bây giờ hãy xem xét một chu kỳ nhỏ trong đó cạnh cưỡng bức không tối ưu:```
1-2 (1), 2-3 (2), 1-3 (100), forced = 1-3
```| Bước | Cạnh | Hành động | DSU sáp nhập | Chi phí | 
| --- | --- | --- | --- | --- | 
| ban đầu | 1-3 cưỡng bức | công đoàn | (1,3) | 100 | 
| 1 | 1-2 | hợp nhất | (1,2,3) | 101 | 
| 2 | 2-3 | bỏ qua | chu kỳ | 101 | 

Điều này xác nhận rằng ngay cả khi cạnh cưỡng bức rất tốn kém, thuật toán vẫn tạo ra cây bao trùm hợp lệ chứa nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m \log m)$| Sắp xếp các cạnh chiếm ưu thế; Hoạt động DSU gần như có thời gian khấu hao cố định | 
| Không gian |$O(n + m)$| Lưu trữ cho các cạnh và mảng DSU | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các cạnh và hệ số logarit có thể dễ dàng quản lý được trong giới hạn điển hình. Sắp xếp cộng với DSU nằm trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

# sample
assert run("""4 5
3
1 2 10
2 3 5
3 4 50
1 4 20
2 4 15
""") == "65"

# minimal graph
assert run("""2 1
1
1 2 10
""") == "10"

# forced edge is expensive cycle
assert run("""3 3
3
1 2 1
2 3 2
1 3 100
""") == "101"

# star graph
assert run("""4 3
2
1 2 5
2 3 1
2 4 1
""") == "7"

# already optimal MST unaffected by forcing a low-cost edge
assert run("""4 5
1
1 2 1
2 3 1
3 4 1
1 3 10
2 4 10
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp mẫu | 65 | tính chính xác của đầu vào chung | 
| 2 nút | 10 | trường hợp cạnh tối thiểu | 
| tam giác buộc cạnh đắt | 101 | xử lý chu trình với cạnh cưỡng bức | 
| đồ thị sao | 7 | cấu trúc MST phân nhánh | 
| cạnh thay thế dày đặc | 3 | bỏ qua các cạnh chéo đắt tiền | 

## Vỏ cạnh 

Khi cạnh cưỡng bức đã thuộc về MST tự nhiên, thuật toán vẫn hoạt động trơn tru vì việc hợp nhất trước không làm thay đổi các quyết định sau này. Ví dụ: trong biểu đồ đường, việc buộc cạnh giữa chỉ đơn giản phản ánh hành vi Kruskal tiêu chuẩn. 

Khi cạnh cưỡng bức có giá đắt nhất trong một chu kỳ, DSU vẫn sẽ chấp nhận nó trước và tất cả các cạnh rẻ hơn cuối cùng sẽ kết nối các thành phần. Điều này chứng tỏ cấu trúc tham lam không bị vi phạm, chỉ bị hạn chế khi khởi tạo. 

Khi đồ thị đã được kết nối tối thiểu với chính xác$n-1$các cạnh, buộc bất kỳ cạnh nào chỉ cần thêm chi phí đó và phần còn lại của thuật toán không thực hiện liên kết nào, điều này mang lại chính xác cây bao trùm duy nhất chứa cạnh được yêu cầu.
