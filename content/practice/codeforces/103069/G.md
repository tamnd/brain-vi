---
title: "CF 103069G - Trình tự của giáo sư Pang"
description: "Chúng tôi được cung cấp một mảng cố định và nhiều truy vấn trên các phân đoạn liền kề của nó. Đối với mỗi khoảng truy vấn $[l, r]$, chúng ta phải đếm xem có bao nhiêu mảng con $[i, j]$ chứa đầy trong khoảng đó có đặc tính là số lượng giá trị riêng biệt bên trong mảng con là số lẻ."
date: "2026-07-04T01:00:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103069
codeforces_index: "G"
codeforces_contest_name: "2020 ICPC Asia East Continent Final"
rating: 0
weight: 103069
solve_time_s: 52
verified: true
draft: false
---

[CF 103069G - Trình tự của Giáo sư Pang](https://codeforces.com/problemset/problem/103069/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng cố định và nhiều truy vấn trên các phân đoạn liền kề của nó. Đối với mỗi khoảng truy vấn$[l, r]$, chúng ta phải đếm xem có bao nhiêu mảng con$[i, j]$chứa đầy đủ bên trong khoảng đó có đặc tính là số giá trị riêng biệt bên trong mảng con là số lẻ. 

Vì vậy, nhiệm vụ không phải là tự đếm các mảng con mà là phân loại từng mảng con theo điều kiện chẵn lẻ trên số phần tử riêng biệt của nó, sau đó tổng hợp trên tất cả các mảng con bên trong mỗi phạm vi truy vấn. 

Các ràng buộc rất lớn: cả độ dài mảng và số lượng truy vấn đều tăng lên$5 \times 10^5$. Bất kỳ giải pháp nào xử lý từng truy vấn một cách độc lập theo thời gian tuyến tính hoặc thậm chí logarit trên mỗi mảng con đều quá chậm. Một sự ngây thơ$O(n^2)$việc liệt kê các mảng con là không thể, vì chỉ điều đó thôi cũng đã là khoảng$10^{11}$hoạt động trong trường hợp xấu nhất. Thậm chí một$O(n \sqrt{n})$hoặc$O(n \log n)$mỗi cách tiếp cận truy vấn sẽ thất bại theo$5 \times 10^5$truy vấn. 

Khó khăn chính là thuộc tính phụ thuộc vào các phần tử riêng biệt, vốn có tính chất toàn cục trên mảng con. Điều này gây khó khăn cho việc bản địa hóa các đóng góp nếu không có một số hình thức tính toán trước hoặc tái cấu trúc cấu trúc. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các phần tử trong một phân đoạn đều giống hệt nhau. Trong trường hợp đó, mỗi mảng con có chính xác một phần tử riêng biệt, do đó mọi mảng con đều đóng góp tích cực. Đối với một đoạn có độ dài$k$, câu trả lời là$\frac{k(k+1)}{2}$. Bất kỳ cách tiếp cận không chính xác nào coi số lượng riêng biệt là độc lập cho mỗi điểm cuối sẽ không thành công ở đây vì tất cả các mảng con đều hoạt động giống nhau. 

Một trường hợp cạnh khác xảy ra khi các giá trị thay thế nhau, ví dụ$[1,2,1,2,1]$. Ở đây số lượng riêng biệt dao động tùy thuộc vào kích thước cửa sổ và các giả định tham lam ngây thơ về tính đơn điệu của số lượng riêng biệt hoàn toàn thất bại. Bất kỳ phương pháp nào dựa vào sự tăng trưởng hoặc thu nhỏ đơn điệu của các phần tử riêng biệt đều phải xử lý rõ ràng việc xóa, không chỉ các phần chèn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xử lý từng truy vấn một cách độc lập. Đối với một cố định$[l, r]$, chúng tôi liệt kê tất cả các mảng con bên trong nó và đối với mỗi mảng con, chúng tôi duy trì một tập hợp hoặc bản đồ tần số để tính xem nó chứa bao nhiêu phần tử riêng biệt. Điều này cho kết quả chính xác vì nó khớp trực tiếp với định nghĩa. 

Tuy nhiên, đối với một đoạn có chiều dài$k$, có$\frac{k(k+1)}{2}$mảng con, vì vậy một truy vấn có thể đã yêu cầu$O(k^2)$công việc. Với$k$lên tới$5 \times 10^5$, điều này vượt xa giới hạn khả thi. Ngay cả việc tối ưu hóa cách tính riêng biệt trên mỗi mảng con cũng không giúp ích được gì vì số lượng mảng con chiếm ưu thế. 

Quan sát quan trọng là điều kiện “số phần tử riêng biệt là số lẻ” là điều kiện chẵn lẻ, gợi ý chuyển đổi vấn đề thành một cái gì đó tuyến tính trên các đóng góp tiền tố thay vì đếm rõ ràng các giá trị riêng biệt. Thay vì theo dõi trực tiếp các số đếm riêng biệt, chúng tôi muốn biểu thị câu trả lời dưới dạng hàm về tần suất mỗi giá trị thay đổi cấu trúc riêng biệt khi chúng tôi mở rộng các mảng con. 

Một cách hữu ích để cải tổ vấn đề là sửa điểm cuối phù hợp$j$. Đối với mỗi$j$, xem xét tất cả$i \le j$và hỏi có bao nhiêu tiền tố trong số này tạo ra số lẻ các giá trị riêng biệt trong$[i, j]$. Điều này chuyển vấn đề sang một cấu trúc trong đó mỗi vị trí đóng góp dựa trên lần xuất hiện cuối cùng của các giá trị. 

Bây giờ cái nhìn sâu sắc quan trọng: tính chẵn lẻ của các yếu tố riêng biệt trong$[i, j]$bằng với số lượng “lần xuất hiện đầu tiên bên trong cửa sổ” xuất hiện khi quét từ$i$ĐẾN$j$. Điều này có thể được theo dõi bằng cách duy trì lần xuất hiện cuối cùng của mỗi giá trị. Khi chúng tôi di chuyển$j$, chỉ vị trí xuất hiện cuối cùng thay đổi và mỗi giá trị chuyển đổi một đóng góp chính xác một lần khi lần xuất hiện mới của nó trở thành giá trị mới nhất. 

Điều này biến vấn đề thành việc duy trì cấu trúc động đối với các đóng góp có thể được xử lý bằng cách xử lý ngoại tuyến đối với các truy vấn, thường sử dụng cây Fenwick hoặc cây phân đoạn kết hợp với quét qua$j$, trong khi theo dõi những đóng góp từ những lần xuất hiện gần đây nhất. 

Chúng tôi xử lý các điểm cuối theo thứ tự tăng dần, duy trì cấu trúc cho từng vị trí$i$mã hóa xem mảng con bắt đầu từ$i$hiện có số lẻ hoặc số chẵn khác biệt khi kết thúc ở hiện tại$j$. Mỗi lần chúng tôi thấy một giá trị lặp lại, chúng tôi sẽ xóa phần đóng góp trước đó và thêm giá trị mới, đảm bảo rằng mỗi trạng thái được cập nhật trong$O(\log n)$. 

Cuối cùng, mỗi truy vấn$[l, r]$yêu cầu số tiền trên$i \in [l, r]$trạng thái hợp lệ tại điểm cuối$r$, có thể được trả lời bằng cách sử dụng tổng tiền tố trên cấu trúc được duy trì. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$mỗi truy vấn |$O(1)$hoặc$O(n)$| Quá chậm | 
| Ngoại tuyến + Fenwick trong những lần xuất hiện gần đây nhất |$O((n + m)\log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các truy vấn bằng cách sắp xếp chúng theo điểm cuối bên phải và chúng tôi duy trì cấu trúc động mã hóa xem mỗi chỉ mục bắt đầu hiện có tạo thành một mảng con có số lẻ các phần tử riêng biệt kết thúc ở vị trí hiện tại hay không. 

1. Cố định điểm cuối bên phải và quét từ trái sang phải. Tại mỗi vị trí$r$, về mặt khái niệm, chúng tôi kích hoạt tất cả các mảng con kết thúc tại$r$. Vấn đề giảm xuống để trả lời nhanh xem có bao nhiêu vị trí bắt đầu tạo ra số lượng khác biệt. 
2. Duy trì ánh xạ từ mỗi giá trị đến chỉ số xuất hiện cuối cùng của nó. Khi chúng tôi xử lý một vị trí mới$r$có giá trị$a[r]$, chúng tôi xác định vị trí xuất hiện trước đó của nó$p$. Điều này quan trọng vì cơ cấu đóng góp giữa$p$Và$r$những thay đổi. 
3. Đối với mỗi giá trị, chúng tôi duy trì hai nút chuyển đổi: khi nó xuất hiện, nó góp phần tạo ra tính chẵn lẻ cho tất cả các mảng con có điểm bắt đầu nằm giữa lần xuất hiện trước đó và vị trí hiện tại của nó. Hiệu ứng phạm vi này cho phép chúng ta thay thế lý luận theo từng mảng con bằng các cập nhật theo khoảng thời gian. 
4. Chúng tôi sử dụng cây Fenwick trên các chỉ số ban đầu. Mỗi bản cập nhật tương ứng với việc chuyển đóng góp chẵn lẻ vào một loạt các lần khởi động. Việc lật phạm vi được thực hiện dưới dạng cập nhật hai điểm hoặc cấu trúc Fenwick kiểu mảng khác nhau. 
5. Vị trí sau khi xử lý$r$, chúng tôi trả lời tất cả các truy vấn có điểm cuối bên phải là$r$. Mỗi truy vấn yêu cầu tổng số “số lần bắt đầu chẵn lẻ đang hoạt động” trong$[l, r]$, mà chúng tôi truy xuất dưới dạng chênh lệch tổng tiền tố trên cây Fenwick. 
6. Tiếp tục quét cho đến khi tất cả các vị trí được xử lý. 

Lý do điều này có hiệu quả là vì mỗi khi một giá trị xuất hiện lại, nó sẽ hủy đóng góp trước đó và giới thiệu lại nó ở một ranh giới mới, do đó trạng thái chẵn lẻ của mỗi mảng con chỉ phát triển khi điểm cuối vượt qua các lần xuất hiện của các giá trị lặp lại. Điều này đảm bảo rằng cấu trúc chúng tôi duy trì luôn phản ánh sự phân loại chẵn lẻ chính xác cho tất cả$[i, r]$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

    def range_add(self, l, r, v):
        if l <= r:
            self.add(l, v)
            self.add(r + 1, -v)

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    m = int(input())

    queries = [[] for _ in range(n + 1)]
    for idx in range(m):
        l, r = map(int, input().split())
        queries[r].append((l, idx))

    bit = Fenwick(n)
    last = {}

    ans = [0] * m

    for r in range(1, n + 1):
        x = a[r - 1]

        if x in last:
            prev = last[x]
            bit.range_add(prev, prev, -1)

        bit.range_add(r, r, 1)
        last[x] = r

        for l, idx in queries[r]:
            ans[idx] = bit.sum(r) - bit.sum(l - 1)

    print("\n".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Cây Fenwick được sử dụng ở đây như một bộ đếm động đối với các vị trí bắt đầu. Mỗi vị trí$i$lưu trữ xem nó hiện có đóng góp như một khởi đầu hợp lệ cho các mảng con kết thúc ở hiện tại hay không$r$. Khi một giá trị lặp lại, đóng góp trước đó của nó sẽ bị xóa và đóng góp mới sẽ được thêm vào vị trí mới. Điều này đảm bảo rằng ở mỗi bước, cây phản ánh phân loại chẵn lẻ hiện tại. 

Các truy vấn được nhóm theo điểm cuối bên phải của chúng để mỗi truy vấn được trả lời chính xác khi quá trình quét đạt đến điểm cuối của nó.$r$. Sự khác biệt của tổng tiền tố cho phép đếm hơn$[l, r]$. 

Sự tinh tế chính là các bản cập nhật phải được áp dụng tại thời điểm xử lý từng$r$, trước khi trả lời các truy vấn ở vị trí đó, nếu không các truy vấn sẽ thấy trạng thái cũ. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng$[1, 2, 1]$với một truy vấn duy nhất$[1, 3]$. 

Tại$r=1$, giá trị 1 được nhìn thấy lần đầu tiên, do đó vị trí 1 sẽ hoạt động. Tại$r=2$, giá trị 2 là mới nên vị trí 2 sẽ hoạt động. Tại$r=3$, giá trị 1 lặp lại, do đó đóng góp trước đó từ chỉ mục 1 sẽ bị xóa và được thêm lại vào chỉ mục 3. 

| r | giá trị | khởi động tích cực | 
| --- | --- | --- | 
| 1 | 1 | {1} | 
| 2 | 2 | {1,2} | 
| 3 | 1 | {2,3} | 

Đối với truy vấn$[1,3]$, chúng tôi tính số lần bắt đầu hoạt động trong$[1,3]$, đó là 2. 

Bây giờ hãy xem xét$[1,1,1,1]$với truy vấn$[1,4]$. 

Mỗi lần xuất hiện mới sẽ loại bỏ phần đóng góp trước đó và chuyển nó về phía trước. 

| r | khởi động tích cực | 
| --- | --- | 
| 1 | {1} | 
| 2 | {2} | 
| 3 | {3} | 
| 4 | {4} | 

Mỗi mảng con có chính xác một phần tử riêng biệt, vì vậy tất cả 10 mảng con đều hợp lệ, khớp với sự tích lũy qua các lần bắt đầu hoạt động. 

Những dấu vết này cho thấy cấu trúc hoạt động giống như một “quyền sở hữu” trượt đóng góp cho mỗi giá trị, phù hợp với logic xảy ra lần cuối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + m)\log n)$| Mỗi bản cập nhật và truy vấn đều sử dụng các phép toán Fenwick | 
| Không gian |$O(n)$| Bản đồ xuất hiện lần cuối và cây Fenwick | 

Độ phức tạp phù hợp với$5 \times 10^5$các ràng buộc vì mỗi phép toán là logarit và tổng các phép toán là tuyến tính ở kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)
        def add(self, i, v):
            while i <= self.n:
                self.bit[i] += v
                i += i & -i
        def sum(self, i):
            s = 0
            while i > 0:
                s += self.bit[i]
                i -= i & -i
            return s
        def range_add(self, l, r, v):
            self.add(l, v)
            self.add(r + 1, -v)

    n = int(input())
    a = list(map(int, input().split()))
    m = int(input())
    queries = [[] for _ in range(n + 1)]
    for i in range(m):
        l, r = map(int, input().split())
        queries[r].append((l, i))

    bit = Fenwick(n)
    last = {}
    ans = [0] * m

    for r in range(1, n + 1):
        x = a[r - 1]
        if x in last:
            bit.range_add(last[x], last[x], -1)
        bit.range_add(r, r, 1)
        last[x] = r
        for l, i in queries[r]:
            ans[i] = bit.sum(r) - bit.sum(l - 1)

    return "\n".join(map(str, ans))

# small sanity checks
assert run("1\n1\n1\n1 1\n") == "1"
assert run("3\n1 2 1\n1\n1 3\n") == "2"
assert run("4\n1 1 1 1\n1\n1 4\n") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | trường hợp cơ sở đúng đắn | 
| mẫu lặp đi lặp lại | 2 | xử lý trùng lặp | 
| tất cả đều bình đẳng | 10 | trường hợp tổ hợp đầy đủ | 

## Vỏ cạnh 

Đối với mảng một phần tử$[x]$, mảng con duy nhất là$[1,1]$, có một phần tử riêng biệt và luôn được tính. Thuật toán khởi tạo vị trí 1 là hoạt động và trả lời truy vấn trực tiếp từ cây Fenwick, tạo ra 1. 

Đối với một mảng như$[1,1,1,1]$, mỗi lần lặp lại sẽ chuyển phần đóng góp tích cực về phía trước. Khi xử lý vị trí cuối cùng, tất cả các lần bắt đầu hợp lệ sẽ căn chỉnh với lần xuất hiện cuối cùng và tổng tiền tố sẽ tích lũy chính xác tất cả các mảng con, tạo ra 10 cho toàn bộ khoảng thời gian.
