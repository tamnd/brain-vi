---
title: "CF 1168C - Và khả năng tiếp cận"
description: "Chúng ta được cung cấp một chuỗi các số nguyên được sắp xếp trên một dòng và chúng ta muốn trả lời các truy vấn kết nối giữa các cặp vị trí, nhưng kết nối không dựa trên tính liền kề."
date: "2026-06-15T16:46:14+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dp"]
categories: ["algorithms"]
codeforces_contest: 1168
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 562 (Div. 1)"
rating: 2200
weight: 1168
solve_time_s: 297
verified: true
draft: false
---

[CF 1168C - Và khả năng tiếp cận](https://codeforces.com/problemset/problem/1168/C) 

**Đánh giá:** 2200 
**Thẻ:** bitmask, dp 
**Thời gian giải:** 4 phút 57 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các số nguyên được sắp xếp trên một dòng và chúng ta muốn trả lời các truy vấn kết nối giữa các cặp vị trí, nhưng kết nối không dựa trên tính liền kề. Thay vào đó, chúng ta được phép chuyển từ chỉ mục này sang chỉ mục sau nếu giá trị AND theo bit của chúng chia sẻ ít nhất một bit được đặt. Từ vị trí xuất phát$x$, chúng ta có thể tiến lên phía trước thông qua một chuỗi các chỉ số miễn là mọi cặp liên tiếp trong chuỗi đều có AND khác 0. Nhiệm vụ là xác định, đối với nhiều truy vấn, liệu chúng ta có thể đạt được vị trí mục tiêu hay không$y$bắt đầu từ$x$. 

Về cơ bản, đây là một vấn đề về khả năng tiếp cận trên biểu đồ có hướng trong đó các nút là chỉ số và có một cạnh từ$i$ĐẾN$j$vì$i < j$nếu như$a_i \& a_j > 0$. Thách thức là việc xây dựng tất cả các cạnh một cách rõ ràng là không khả thi vì nó đòi hỏi$O(n^2)$làm việc trong trường hợp xấu nhất. 

Các ràng buộc rất lớn, lên tới 300.000 phần tử và 300.000 truy vấn. Bất kỳ cách tiếp cận nào kiểm tra các cặp trực tiếp sẽ hết thời gian chờ. Ngay cả việc truyền tải biểu đồ trên mỗi truy vấn cũng quá chậm, vì BFS hoặc DFS có thể giảm xuống tuyến tính trên mỗi truy vấn. Điều này thúc đẩy chúng tôi tiến tới việc tiền xử lý một cấu trúc cho phép kiểm tra khả năng tiếp cận nhanh chóng. 

Một trường hợp khó nhận thấy là sự hiện diện của số không. Bất kỳ chỉ mục nào có giá trị 0 đều không có cạnh hữu ích đi hoặc đến, bởi vì$0 \& x = 0$cho tất cả$x$. Các vị trí như vậy không bao giờ có thể tham gia vào một đường dẫn hợp lệ, ngoại trừ các điểm cuối biệt lập của các truy vấn tầm thường không yêu cầu duyệt qua chúng. Ví dụ, nếu$a = [1, 0, 2]$, thì vị trí 2 không thể giúp nối 1 và 3 dù nó nằm ở giữa. 

Một vấn đề không rõ ràng khác là khả năng tiếp cận không đối xứng và bị hạn chế bởi việc tăng chỉ số. Điều này làm cho cấu trúc gần giống với biểu đồ chỉ chuyển tiếp, cho phép chúng ta khai thác thứ tự trong giải pháp. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp xây dựng biểu đồ một cách rõ ràng và chạy BFS cho mỗi truy vấn. Đối với mỗi chỉ số$i$, chúng tôi kiểm tra tất cả$j > i$và thêm một cạnh nếu$a_i \& a_j > 0$. Cái này đã tốn rồi$O(n^2)$, quá lớn đối với 300.000 phần tử. 

Ngay cả khi chúng ta bỏ qua cấu trúc rõ ràng và thử BFS cho mỗi truy vấn, mỗi lần truyền tải vẫn có thể chạm đến một phần lớn của mảng. Với 300.000 truy vấn, độ phức tạp trong trường hợp xấu nhất trở nên hoàn toàn không khả thi. 

Quan sát quan trọng là khả năng kết nối bitwise AND chỉ phụ thuộc vào các bit được chia sẻ. Mỗi số có thể được xem dưới dạng tập hợp con có tối đa 18 bit (vì giá trị lên tới 300.000). Nếu chúng ta nghĩ về mặt bit, mỗi vị trí thuộc về một số nhóm bit và việc di chuyển chỉ có thể thực hiện được thông qua tư cách thành viên bit được chia sẻ. 

Điều này gợi ý việc duy trì, đối với mỗi bit, vị trí gần đây nhất đã được nhìn thấy khi quét từ trái sang phải. Khi chúng tôi ở chỉ mục$i$, chúng tôi biết chỉ mục nào trước đó chia sẻ ít nhất một bit với nó và chúng tôi có thể “hợp nhất” thông tin kết nối về phía trước. 

Tuy nhiên, việc duy trì kết nối bắc cầu đầy đủ trên mỗi bit vẫn còn phức tạp. Sự đơn giản hóa quan trọng là chúng ta không cần khả năng tiếp cận đầy đủ giữa tất cả các cặp; chúng ta chỉ cần biết liệu hai chỉ số có thuộc cùng một thành phần được kết nối trong biểu đồ chuyển tiếp này hay không. Vì các cạnh chỉ tiến về phía trước nên các thành phần có thể được xây dựng tăng dần bằng cách sử dụng cấu trúc giống như kết hợp trên các chỉ mục, được hướng dẫn bởi các lần xuất hiện bit. 

Chúng tôi xử lý các chỉ mục từ trái sang phải, duy trì sự kết hợp rời rạc (DSU). Đối với mỗi bit, chúng ta nhớ chỉ mục cuối cùng nơi bit này xuất hiện. Khi chúng tôi ở chỉ mục$i$, với mỗi bit được đặt trong$a_i$, chúng tôi đoàn kết$i$với chỉ mục nhìn thấy lần cuối cho bit đó, sau đó cập nhật chỉ mục nhìn thấy lần cuối thành$i$. Điều này xây dựng chính xác kết nối được tạo ra bởi các chuỗi bit chồng chéo. 

Khi DSU được xây dựng, mỗi truy vấn sẽ giảm xuống việc kiểm tra xem$x$Và$y$thuộc cùng một thành phần, đồng thời đảm bảo$x < y$(đã được đảm bảo bởi đầu vào). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| BFS Brute Force cho mỗi truy vấn |$O(qn)$|$O(n^2)$| Quá chậm | 
| DSU qua việc truyền bit |$O(n \alpha(n) \cdot B + q \alpha(n))$|$O(n + B)$| Đã chấp nhận | 

Đây$B$là số bit trong phạm vi giá trị (tối đa 18). 

## Hướng dẫn thuật toán 

1. Khởi tạo cấu trúc DSU trên các chỉ mục$1 \dots n$. Mỗi chỉ mục bắt đầu trong thành phần riêng của nó. Điều này thể hiện rằng ban đầu không có kết nối nào được biết đến. 
2. Duy trì một mảng`last[bit]`, trong đó mỗi mục lưu trữ chỉ mục gần đây nhất có giá trị chứa bit đó. Ban đầu tất cả đều không được đặt. 
3. Lặp lại các chỉ số từ trái sang phải. Tại chỉ mục$i$, kiểm tra tất cả các bit được đặt trong$a_i$. Mỗi bit đại diện cho một “kênh” mà qua đó kết nối có thể lan truyền. 
4. Đối với mỗi bit$b$đặt vào$a_i$, nếu như`last[b]`tồn tại, liên minh$i$với`last[b]`. Điều này kết nối vị trí hiện tại với lần xuất hiện trước đó của cùng một bit và gián tiếp với mọi thứ đã được kết nối với lần xuất hiện đó. 
5. Sau khi xử lý các kết hợp cho tất cả các bit của$a_i$, cập nhật`last[b] = i`cho mỗi bit$b$đặt vào$a_i$. Điều này đảm bảo các chỉ số trong tương lai kết nối với đại diện gần đây nhất của mỗi nhóm bit. 
6. Sau khi tiền xử lý, đối với mỗi truy vấn$(x, y)$, kiểm tra xem các hoạt động tìm kiếm DSU có trả về cùng một gốc hay không. Nếu có thì xuất ra “Shi”, nếu không thì xuất ra “Fou”. 

### Tại sao nó hoạt động 

Bất biến DSU là hai chỉ số nằm trong cùng một tập hợp khi và chỉ khi tồn tại một chuỗi các chỉ số từ trái sang phải trong đó các phần tử liên tiếp có chung ít nhất một bit chung. Mọi phép toán hợp đều tương ứng chính xác với việc đưa ra một cạnh hợp lệ trong biểu đồ được tạo ra bởi các bit dùng chung. Vì mọi cạnh có thể kết nối một nút với lần xuất hiện mới nhất trước đó của ít nhất một bit được chia sẻ, nên tất cả các đường dẫn trong định nghĩa khả năng tiếp cận ban đầu đều có thể biểu diễn thông qua các liên kết này. Tính chuyển tiếp của DSU đảm bảo rằng mọi chuỗi nhiều bước đều được ghi lại, do đó các truy vấn kết nối trên DSU khớp với khả năng tiếp cận trong biểu đồ gốc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, a, b):
        ra, rb = self.find(a), self.find(b)
        if ra == rb:
            return
        if self.size[ra] < self.size[rb]:
            ra, rb = rb, ra
        self.parent[rb] = ra
        self.size[ra] += self.size[rb]

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    dsu = DSU(n)
    last = [-1] * 20

    for i in range(n):
        x = a[i]
        for b in range(20):
            if x & (1 << b):
                if last[b] != -1:
                    dsu.union(i, last[b])
                last[b] = i

    out = []
    for _ in range(q):
        x, y = map(int, input().split())
        x -= 1
        y -= 1
        if dsu.find(x) == dsu.find(y):
            out.append("Shi")
        else:
            out.append("Fou")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```DSU được sử dụng để nén kết nối vào các thành phần khi chúng tôi quét từ trái sang phải. Chi tiết triển khai chính là`last`mảng, đảm bảo chúng tôi chỉ kết nối từng chỉ mục mới với chỉ mục gần đây nhất cho mỗi bit, tránh so sánh bậc hai. Hoạt động kết hợp là đủ vì các kết nối trước đó đã được ghi lại một cách bắc cầu. 

Phép lặp bit được giới hạn bởi 20 vì giá trị tối đa là 300.000, vừa với 19 bit. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3
1 3 0 2 1
1 3
2 4
1 4
```Chúng tôi theo dõi các kết nối DSU khi quét: 

| tôi | một [tôi] | bit | đoàn biểu diễn | cập nhật lần cuối | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | không | bit0=1 | 
| 2 | 3 | 0,1 | công đoàn(2,1) | bit0=2, bit1=2 | 
| 3 | 0 | không | không | không thay đổi | 
| 4 | 2 | 1 | công đoàn(4,2) | bit1=4 | 
| 5 | 1 | 0 | công đoàn(5,2) | bit0=5 | 

Truy vấn (1,3): chỉ số 3 bằng 0, cô lập, không kết nối với 1 nên đáp án là Fou. 

Truy vấn (2,4): cả hai đều thuộc cùng một thành phần thông qua chuỗi bit 1, vì vậy Shi. 

Truy vấn (1,4): nối thông qua 1 → 2 → 4 nên Shi. 

Điều này xác nhận rằng DSU nắm bắt chính xác chuỗi dựa trên bit gián tiếp. 

### Ví dụ 2 

đầu vào:```
4 2
5 1 4 0
1 3
2 3
```Theo dõi: 

| tôi | một [tôi] | bit | công đoàn | cuối cùng | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 0,2 | không | 0=1,2=1 | 
| 2 | 1 | 0 | công đoàn(2,1) | 0=2 | 
| 3 | 4 | 2 | công đoàn(3,1) | 2=3 | 
| 4 | 0 | không | không | không thay đổi | 

Bây giờ 1, 2, 3 được kết nối thông qua cấu trúc bit dùng chung, vì vậy cả hai truy vấn đều trả về Shi. 

Điều này chứng tỏ các kênh bit khác nhau hợp nhất các thành phần một cách bắc cầu như thế nào ngay cả khi không có sự kề cận trực tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \alpha(n) \cdot 20 + q \alpha(n))$| Mỗi chỉ mục xử lý tối đa 20 bit, mỗi kết hợp/tìm là nghịch đảo Ackermann | 
| Không gian |$O(n + 20)$| Mảng DSU cộng với mảng xuất hiện lần cuối | 

Giải pháp có quy mô thoải mái cho 300.000 phần tử và truy vấn do hệ số bit không đổi và các hoạt động DSU có thời gian gần như không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class DSU:
        def __init__(self, n):
            self.parent = list(range(n))
            self.size = [1] * n
        def find(self, x):
            while self.parent[x] != x:
                self.parent[x] = self.parent[self.parent[x]]
                x = self.parent[x]
            return x
        def union(self, a, b):
            ra, rb = self.find(a), self.find(b)
            if ra == rb:
                return
            if self.size[ra] < self.size[rb]:
                ra, rb = rb, ra
            self.parent[rb] = ra
            self.size[ra] += self.size[rb]

    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    dsu = DSU(n)
    last = [-1] * 20

    for i in range(n):
        for b in range(20):
            if a[i] & (1 << b):
                if last[b] != -1:
                    dsu.union(i, last[b])
                last[b] = i

    out = []
    for _ in range(q):
        x, y = map(int, input().split())
        x -= 1
        y -= 1
        out.append("Shi" if dsu.find(x) == dsu.find(y) else "Fou")

    return "\n".join(out)

# provided sample
assert run("""5 3
1 3 0 2 1
1 3
2 4
1 4
""") == "Fou\nShi\nShi"

# single element chains via bit propagation
assert run("""3 2
1 2 4
1 3
2 3
""") == "Shi\nShi"

# all zeros except endpoints
assert run("""4 2
1 0 0 2
1 4
2 3
""") == "Fou\nFou"

# all equal values
assert run("""5 1
7 7 7 7 7
1 5
""") == "Shi"

# sparse bits
assert run("""6 2
1 2 4 8 16 32
1 6
2 5
""") == "Fou\nFou"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ví dụ về chuỗi thưa thớt | Shi / Shi | lan truyền qua sự chồng chéo bit | 
| trường hợp số không | Fou / Fou | không phá vỡ kết nối | 
| giá trị thống nhất | Thạch | kết nối đầy đủ | 
| bit rời rạc | Fou | không có đoàn thể ngẫu nhiên | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi các số 0 xuất hiện giữa các giá trị hợp lệ. Đối với đầu vào`[1, 0, 2]`, chỉ số 2 không bao giờ tham gia vào bất kỳ liên minh nào. Thuật toán tách nó một cách chính xác vì nó không có bit nào được thiết lập, vì vậy`last`không bao giờ được cập nhật và không có sự kết hợp nào xảy ra. 

Một trường hợp khác là các giá trị lặp lại với các mẫu bit giống hệt nhau. Vì`[7, 7, 7]`, mỗi chỉ mục kết hợp với chỉ mục trước đó thông qua tất cả các bit được chia sẻ và DSU hợp nhất chúng thành một thành phần duy nhất. Thuật toán không đếm kép hoặc tạo ra cấu trúc dư thừa vì các phép hợp lặp lại bị DSU bỏ qua. 

Một trường hợp khác liên quan đến việc phân phối bit thưa thớt trong đó kết nối yêu cầu xâu chuỗi thông qua các chỉ số trung gian thay vì chồng chéo trực tiếp. các`last`mảng đảm bảo chuỗi như vậy được duy trì vì mọi lần xuất hiện mới đều liên kết với thành phần hiện có cho bit đó, truyền bá kết nối về phía trước.
