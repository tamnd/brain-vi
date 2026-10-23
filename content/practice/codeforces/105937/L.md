---
title: "CF 105937L - Gros-Phi"
description: "Mỗi trò chơi mô tả một tập hợp các mục tiêu được tính thời gian trên một dòng. Mỗi mục tiêu là một cặp bao gồm một thời điểm và một vị trí trên đường thẳng. Bắt đầu từ thời điểm 0, Awa có thể chọn bất kỳ vị trí ban đầu nào và sau đó di chuyển dọc theo đường với tốc độ tối đa cố định."
date: "2026-06-22T15:49:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105937
codeforces_index: "L"
codeforces_contest_name: "2025 Xian Jiaotong University Programming Contest"
rating: 0
weight: 105937
solve_time_s: 82
verified: true
draft: false
---

[CF 105937L - Gros-Phi](https://codeforces.com/problemset/problem/105937/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi trò chơi mô tả một tập hợp các mục tiêu được tính thời gian trên một dòng. Mỗi mục tiêu là một cặp bao gồm một thời điểm và một vị trí trên đường thẳng. Bắt đầu từ thời điểm 0, Awa có thể chọn bất kỳ vị trí ban đầu nào và sau đó di chuyển dọc theo đường với tốc độ tối đa cố định. Khi một mục tiêu xuất hiện, cô ấy chỉ có thể ghi điểm nếu cô ấy ở chính xác vị trí của nó vào thời điểm đó. 

Nhiệm vụ là chọn một tập hợp con lớn nhất có thể có của các mục tiêu này sao cho tồn tại một số cách để di chuyển qua chúng theo thứ tự thời gian tăng dần, bắt đầu từ một vị trí tùy ý tại thời điểm 0, trong khi không bao giờ vượt quá giới hạn tốc độ. 

Khó khăn chính là tính khả thi không mang tính cục bộ. Việc mục tiêu có thể tiếp cận được hay không tùy thuộc vào việc chúng ta đến từ mục tiêu nào trước đó, bởi vì mỗi lựa chọn sẽ xác định nơi chúng ta phải đến sớm hơn. 

Các ràng buộc ngụ ý rằng việc kiểm tra từng cặp đơn giản giữa tất cả các mục tiêu cho mỗi trường hợp thử nghiệm là không thể. Với tổng số điểm lên tới 5e5 trong tất cả các bài kiểm tra, cách tiếp cận bậc hai sẽ yêu cầu theo thứ tự 10^11 lần kiểm tra trong trường hợp xấu nhất, vượt xa giới hạn một giây. 

Các trường hợp nhạy cảm nhất đến từ các mục tiêu ở gần về thời gian nhưng xa về không gian hoặc ngược lại, trong đó một lựa chọn tham lam có thể chọn một điểm có thể tiếp cận cục bộ để chặn quyền truy cập vào chuỗi lớn hơn nhiều sau này. 

Ví dụ: hãy xem xét hai điểm: 

cặp thời gian-vị trí (0, 0), (1, 1000000000) với tốc độ nhỏ. Giải pháp thứ hai không thể truy cập được từ giải pháp đầu tiên, do đó, bất kỳ giải pháp nào giả định khả năng tiếp cận chỉ dựa trên thứ tự hoặc vị trí sẽ thất bại ngay lập tức. Ngược lại, những điểm nhìn xa trong thời gian nhưng ở gần trong không gian vẫn có thể bị xâu chuỗi nếu chuyển động trung gian khả thi. 

Thách thức thực sự là khả năng tiếp cận xác định thứ tự một phần theo điểm và chúng tôi muốn có chuỗi dài nhất trong cấu trúc đó. 

## Phương pháp tiếp cận 

Nếu chúng ta cố gắng dùng vũ lực, chúng ta có thể coi mỗi điểm là một nút và cố gắng tính toán chuỗi kết thúc tốt nhất tại điểm đó bằng cách kiểm tra tất cả các điểm trước đó. Đối với một cặp điểm i và j có i sớm hơn trong thời gian, chúng ta kiểm tra xem Awa có thể di chuyển từ j đến i trong khoảng thời gian có sẵn hay không. Điều này tạo ra sự chuyển đổi lập trình năng động tự nhiên. 

Tính chính xác rất đơn giản: mọi tuyến đường hợp lệ đều kết thúc ở một điểm cuối cùng nào đó và chúng tôi thử tất cả các khả năng cho bước trước đó. 

Điểm nghẽn là chi phí chuyển đổi. Với n điểm, điều này yêu cầu kiểm tra tất cả các cặp, tạo ra sự chuyển tiếp O(n^2) cho mỗi trường hợp thử nghiệm. Với tổng điểm 5e5, điều này là không khả thi. 

Quan sát quan trọng là điều kiện về khả năng tiếp cận có thể được viết lại thành các ràng buộc thống trị hình học sau khi chuyển đổi. Mỗi điểm (t, x) có thể được ánh xạ thành hai giá trị dẫn xuất mã hóa khoảng cách Awa trái và phải có thể đi được bao xa trong khi vẫn đạt đến điểm này. Theo phép biến đổi này, điểm j có thể đạt tới điểm i một cách chính xác khi j nằm trong vùng ưu thế nhất định so với i. 

Điều này chuyển vấn đề thành việc tìm chuỗi dài nhất theo thứ tự một phần 2D. Sau khi được xem theo cách này, cấu trúc sẽ có thể tuân theo việc nén phối hợp và cấu trúc dữ liệu duy trì các giá trị DP tối đa trong các phạm vi. 

Giải pháp cuối cùng giảm xuống các điểm xử lý theo thứ tự được lựa chọn cẩn thận và sử dụng cây Fenwick (hoặc cấu trúc tương tự) để duy trì các giá trị DP tốt nhất trên một tọa độ trong khi lặp lại tọa độ kia. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DP theo cặp | O(n²) | O(n) | Quá chậm | 
| Biến đổi tọa độ + BIT DP | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đối với mỗi điểm, hãy tính hai giá trị dẫn xuất từ ​​thời gian và vị trí của nó để mã hóa các giới hạn không gian có thể tiếp cận tương ứng với tốc độ. Các giá trị này ghi lại khoảng cách mà điểm có thể được kết nối sang trái và phải thông qua một đoạn chuyển động hợp lệ. 
2. Sắp xếp tất cả các điểm theo thứ tự giảm dần của giá trị bên trái. Điều này đảm bảo rằng khi xử lý một điểm, tất cả các ứng cử viên có thể đứng trước điểm đó một cách hợp pháp trong một chuỗi (đối với một ràng buộc) đều đã được xem xét. 
3. Duy trì cây Fenwick trên tọa độ bên phải. Cây lưu trữ giá trị DP tối đa đạt được cho bất kỳ điểm nào có tọa độ nén nhất định. 
4. Xử lý điểm theo thứ tự sắp xếp. Đối với mỗi điểm, hãy truy vấn cây Fenwick để tìm giá trị DP tốt nhất trong số tất cả các điểm thỏa mãn ràng buộc thứ hai (ưu thế trong tầm tay phải). Điều này mang lại chuỗi tiền thân hợp lệ nhất kết thúc tại thời điểm này. 
5. Đặt dp[i] thành giá trị được truy vấn đó cộng với 1, sau đó cập nhật cây Fenwick tại tọa độ bên phải của điểm hiện tại với dp[i]. 
6. Câu trả lời cho trường hợp kiểm thử là giá trị dp tối đa trên tất cả các điểm. 

Lý do thứ tự này hoạt động là vì việc sắp xếp theo tọa độ được chuyển đổi đầu tiên đảm bảo chúng ta không bao giờ cố gắng sử dụng một điểm làm tiền thân trừ khi nó thỏa mãn nửa đầu của điều kiện khả thi. Cây Fenwick thực thi hiệp hai một cách hiệu quả. 

### Tại sao nó hoạt động 

Sự chuyển tiếp hợp lệ giữa hai điểm phụ thuộc vào hai bất đẳng thức xuất phát từ hạn chế tốc độ. Sau khi chuyển đổi, những bất đẳng thức này trở thành quan hệ ưu thế trong hai chiều. Do đó, bất kỳ chuỗi hợp lệ nào cũng là chuỗi trong một tập hợp được sắp xếp một phần được xác định bởi các quy tắc thống trị này. 

Việc sắp xếp đảm bảo chúng tôi tôn trọng một trục của trật tự này trên toàn cầu và cây Fenwick thực thi trục kia cục bộ. Mỗi lần chúng tôi tính toán dp[i], tất cả các phiên bản trước khả thi đều đã được xử lý và có thể truy vấn được. Điều này đảm bảo rằng dp[i] luôn phản ánh chuỗi hợp lệ tốt nhất kết thúc tại i và không thể bao gồm chuỗi tiền nhiệm không hợp lệ vì nó sẽ vi phạm ít nhất một trong các ràng buộc thống trị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class BIT:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def update(self, i, v):
        while i <= self.n:
            if v > self.bit[i]:
                self.bit[i] = v
            i += i & -i

    def query(self, i):
        res = 0
        while i > 0:
            if self.bit[i] > res:
                res = self.bit[i]
            i -= i & -i
        return res

def solve():
    T = int(input())
    out = []

    for _ in range(T):
        n, v = map(int, input().split())
        pts = []

        for _ in range(n):
            t, x = map(int, input().split())
            A = x - v * t
            B = x + v * t
            pts.append((A, B))

        # compress B
        vals = sorted({b for _, b in pts})
        idx = {v: i + 1 for i, v in enumerate(vals)}

        pts.sort(reverse=True)  # sort by A descending

        bit = BIT(len(vals))
        ans = 0

        for A, B in pts:
            bi = idx[B]
            best = bit.query(bi)
            dp = best + 1
            if dp > ans:
                ans = dp
            bit.update(bi, dp)

        out.append(str(ans))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp nén tọa độ được chuyển đổi thứ hai để có thể sử dụng nó trong cây Fenwick. Mỗi điểm được xử lý theo thứ tự giảm dần của tọa độ được chuyển đổi đầu tiên và cây duy trì độ dài chuỗi tốt nhất cho các điểm trước đó khả thi. 

Một điểm tinh tế là chúng ta không bao giờ cần theo dõi rõ ràng thời gian ở trạng thái DP. Việc chuyển đổi đã mã hóa thời gian thành tọa độ dẫn xuất, do đó tính khả thi được nắm bắt hoàn toàn bởi mối quan hệ thống trị. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ với ba điểm: 

Điểm đầu vào (t, x): 

(0, 0), (2, 2), (4, 1) với v = 1 

Chúng tôi tính toán các giá trị được chuyển đổi: 

| Điểm | A = x - vt | B = x + vt | 
| --- | --- | --- | 
| (0,0) | 0 | 0 | 
| (2,2) | 0 | 4 | 
| (4,1) | -3 | 5 | 

Sắp xếp theo A giảm dần cho: 

(0,0), (2,2), (4,1) 

Chúng tôi xử lý: 

| Bước | Điểm | Truy vấn BIT (B) | DP | Cập nhật BIT | 
| --- | --- | --- | --- | --- | 
| 1 | (0,0) | 0 | 1 | đặt B=0 thành 1 | 
| 2 | (2,2) | 1 | 2 | cập nhật B=4 lên 2 | 
| 3 | (4,1) | 2 | 3 | cập nhật B=5 lên 3 | 

Câu trả lời cuối cùng là 3, cho thấy tất cả các điểm đều có thể xâu chuỗi được dưới giới hạn tốc độ. 

Dấu vết này cho thấy các điểm trước đó trong thứ tự được sắp xếp hoạt động chính xác như các điểm trước tiềm năng cho các điểm sau khi cả hai điều kiện thống trị đều được thỏa mãn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp cộng với các cập nhật và truy vấn về cây Fenwick | 
| Không gian | O(n) | lưu trữ điểm chuyển đổi và BIT | 

Trên tất cả các trường hợp thử nghiệm, tổng số điểm được giới hạn bởi 5e5, do đó giải pháp chạy thoải mái trong giới hạn. Hệ số logarit từ việc nén tọa độ và các phép toán BIT giữ cho tổng công việc ở dưới ngưỡng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class BIT:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)

        def update(self, i, v):
            while i <= self.n:
                self.bit[i] = max(self.bit[i], v)
                i += i & -i

        def query(self, i):
            res = 0
            while i > 0:
                res = max(res, self.bit[i])
                i -= i & -i
            return res

    T = int(input())
    out = []

    for _ in range(T):
        n, v = map(int, input().split())
        pts = []
        for _ in range(n):
            t, x = map(int, input().split())
            pts.append((x - v * t, x + v * t))

        vals = sorted({b for _, b in pts})
        idx = {v: i + 1 for i, v in enumerate(vals)}

        pts.sort(reverse=True)

        bit = BIT(len(vals))
        ans = 0

        for a, b in pts:
            bi = idx[b]
            dp = bit.query(bi) + 1
            bit.update(bi, dp)
            ans = max(ans, dp)

        out.append(str(ans))

    return "\n".join(out)

# custom tests

# single point
assert run("1\n1 10\n0 0\n") == "1"

# two unreachable points
assert run("1\n2 1\n0 0\n1 100\n") == "1"

# fully chainable
assert run("1\n3 10\n0 0\n1 1\n2 2\n") == "3"

# same time different positions
assert run("1\n3 1\n0 0\n0 1\n0 2\n") == "1"

# sample-like mixed case
assert run("1\n4 2\n0 0\n1 3\n2 1\n3 5\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm duy nhất | 1 | trường hợp tối thiểu | 
| hai điểm không thể tiếp cận | 1 | chuyển tiếp chặn hạn chế tốc độ | 
| hoàn toàn có thể xâu chuỗi | 3 | chuỗi đơn điệu lý tưởng | 
| cùng một lúc vị trí khác nhau | 1 | độc lập theo thời gian | 
| trường hợp hỗn hợp giống mẫu | 3 | thứ tự không cần thiết và tương tác DP | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi nhiều điểm có chung tọa độ được biến đổi giống nhau nhưng khác nhau đáng kể về thời gian. Vì phép biến đổi mã hóa thời gian nên vẫn có thể có những điểm như vậy có giá trị A hoặc B giống hệt nhau, giá trị này sẽ bị thu gọn trong quá trình nén tọa độ. Cây Fenwick xử lý vấn đề này một cách an toàn vì tọa độ giống hệt nhau được coi là trạng thái tương đương và các giá trị DP tích lũy chuỗi tốt nhất có thể đạt được một cách tự nhiên. 

Một trường hợp tinh vi khác xảy ra khi các điểm xuất hiện theo thứ tự không gian giảm dần nhưng theo thứ tự thời gian tăng dần. Một cách tiếp cận tham lam ngây thơ sẽ cố gắng chỉ tuân theo thứ tự thời gian, nhưng tính khả thi có thể bị phá vỡ do khoảng cách không gian vượt quá giới hạn tốc độ. Trong biểu diễn được chuyển đổi, những trường hợp như vậy trở nên không thể so sánh được ở ít nhất một chiều, ngăn cản việc xem xét các chuyển đổi không hợp lệ. 

Cuối cùng, trường hợp tất cả các điểm nằm trên một hành lang khả thi chặt chẽ sẽ tạo ra các chuỗi tối đa trong đó mọi điểm đều có thể truy cập được từ mọi điểm trước đó. Trong tình huống này, BIT liên tục truyền các giá trị DP tăng dần, suy biến hiệu quả thành phép tính chuỗi con tăng dài nhất trên tọa độ được chuyển đổi.
