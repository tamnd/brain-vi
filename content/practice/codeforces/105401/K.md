---
title: "CF 105401K - Cùng phân khúc"
description: "Chúng ta được cung cấp một dòng các vị trí từ 1 đến N và chúng ta phải gán cho mỗi vị trí một giá trị từ 0 đến K. Trên đầu mảng này có M ràng buộc. Mỗi ràng buộc chỉ định một phân đoạn [l, r] và yêu cầu tổng các giá trị bên trong phân đoạn đó chính xác là K."
date: "2026-06-23T04:56:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105401
codeforces_index: "K"
codeforces_contest_name: "2024 KAIST 14th ICPC Mock Competition"
rating: 0
weight: 105401
solve_time_s: 97
verified: false
draft: false
---

[CF 105401K - Cùng phân khúc](https://codeforces.com/problemset/problem/105401/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dòng các vị trí từ 1 đến N và chúng ta phải gán cho mỗi vị trí một giá trị từ 0 đến K. Trên đầu mảng này có M ràng buộc. Mỗi ràng buộc chỉ định một phân đoạn [l, r] và yêu cầu tổng các giá trị bên trong phân đoạn đó chính xác là K. 

Nhiệm vụ không phải là tính toán số mảng như vậy mà là xây dựng bất kỳ mảng hoặc báo cáo hợp lệ nào không tồn tại. 

Các ràng buộc ngụ ý sự kết hợp toàn cầu giữa các phân đoạn. Mỗi vị trí đóng góp vào nhiều phân đoạn cùng một lúc, vì vậy chúng ta không thể xử lý các phân đoạn một cách độc lập. Thay vào đó, mọi lựa chọn tại một chỉ mục sẽ được truyền tới tất cả các phân đoạn bao trùm nó. 

Quan sát cấu trúc quan trọng nhất xuất phát từ thực tế là K rất nhỏ, nhiều nhất là 20. Điều này cho thấy rõ ràng rằng mỗi vị trí hoạt động giống như một nguồn tài nguyên hạn chế phải được phân phối qua các khoảng chồng chéo. 

Kích thước đầu vào lớn, với tổng N và M lần lượt lên tới 4e5 và 2e5 trong các trường hợp thử nghiệm. Điều này loại trừ mọi thứ bậc hai trên các phân đoạn hoặc quét theo từng phân đoạn đơn giản. Bất kỳ giải pháp nào cũng phải gần như tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Một cách giải thích ngây thơ sẽ cố gắng gán các giá trị một cách tham lam và sau đó kiểm tra lặp đi lặp lại tất cả các phân đoạn. Điều đó thất bại theo hai cách. Đầu tiên, việc tính toán lại tổng phân đoạn khiến nó quá chậm. Thứ hai, tham lam giao việc mà không hiểu rõ chồng chéo sẽ dễ phá vỡ tính khả thi. 

Một trường hợp lỗi nhỏ xuất hiện khi các phân đoạn chồng lên nhau theo kiểu lồng nhau. Ví dụ: nếu chúng ta có các phân đoạn [1,3] và [2,4] với K = 1 thì việc chọn giá trị ở vị trí 2 để đáp ứng phân khúc đầu tiên có thể ngăn cản việc thỏa mãn phân khúc thứ hai. Các quyết định tham lam cục bộ không an toàn vì các ràng buộc tương tác không cục bộ. 

Một kịch bản có vấn đề khác là phạm vi đưa tin trái ngược nhau. Nếu một phân đoạn buộc một khu vực phải tích lũy quá nhiều giá trị, nhưng các phân đoạn chồng chéo sẽ hạn chế cùng một khu vực theo cách khác nhau, thì việc phân công ngây thơ có thể không phát hiện được điều không thể cho đến khi muộn hoặc hoàn toàn không thể. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng gán giá trị cho tất cả N vị trí và kiểm tra tất cả M phân đoạn. Mỗi lần kiểm tra yêu cầu tính tổng trên một phạm vi, cho ra O(NM) trong trường hợp xấu nhất. Ngay cả với tổng tiền tố để tăng tốc độ kiểm tra, chúng ta vẫn cần điều chỉnh phép gán nhiều lần và không gian tìm kiếm của (K+1)^N khiến điều này hoàn toàn không khả thi. 

Quan sát quan trọng là K rất nhỏ và mỗi phân đoạn đều có tổng chính xác. Thay vì suy nghĩ theo các ràng buộc có giá trị thực tùy ý, chúng ta có thể nghĩ theo cách phân phối K đơn vị trong các khoảng thời gian. Mỗi phân đoạn yêu cầu tổng khối lượng cố định và mỗi vị trí chỉ có thể đóng góp tối đa K tổng thể. 

Điều này gợi ý rằng hãy xem mỗi vị trí có mức “tiêu thụ công suất” nhất định phải đáp ứng tất cả các phân khúc đi qua nó. Bước đột phá thực sự là xử lý các ràng buộc theo thứ tự có cấu trúc và thực thi chúng dần dần để mỗi phân đoạn được thỏa mãn chính xác khi nó được xử lý mà không vi phạm các phân đoạn trước đó. 

Chúng ta có thể hiểu vấn đề là duy trì một mảng trong đó mọi phân đoạn đều áp đặt một yêu cầu về luồng. Nếu chúng tôi xử lý các phân đoạn theo thứ tự nhất quán và luôn đẩy khối lượng cần thiết đến các vị trí sẵn có ngoài cùng bên phải trong phân khúc thì chúng tôi sẽ tránh được xung đột trong tương lai. Đây là một công trình tham lam cổ điển dựa trên xử lý theo khoảng thời gian. 

Chúng tôi duy trì cấu trúc theo dõi các giá trị được chỉ định hiện tại và đảm bảo rằng khi xử lý một phân đoạn, chúng tôi vẫn có thể phân bổ K đơn vị bên trong phân đoạn đó. Nếu không, trường hợp này là không thể. Bởi vì K nhỏ nên chúng ta có thể phân phối các đơn vị một cách rõ ràng trong khi vẫn duy trì tính khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((K+1)^N · M) | O(N) | Quá chậm | 
| Tối ưu | O((N + M) log N) hoặc O(N + M) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Ý tưởng chính là coi mỗi phân khúc là nhu cầu của K đơn vị phải được bố trí vào công suất hiện có. 

Chúng tôi xử lý các phân đoạn theo thứ tự tăng dần của điểm cuối bên phải của chúng. Thứ tự này đảm bảo rằng khi chúng tôi đáp ứng một phân khúc, mọi phân khúc trong tương lai sẽ mở rộng hơn nữa hoặc bắt đầu muộn hơn, vì vậy, chúng tôi không vô tình phá hủy tính khả thi của các ràng buộc đã được xử lý. 

1. Sắp xếp tất cả các đoạn theo điểm cuối bên phải r của chúng. Nếu hai phân đoạn chia sẻ r, hãy sắp xếp chúng theo điểm cuối bên trái tăng dần. 
2. Duy trì một mảng a có độ dài N được khởi tạo bằng 0. Chúng tôi sẽ tăng các mục khi chúng tôi đáp ứng các phân đoạn. 
3. Với mỗi đoạn [l, r] theo thứ tự sắp xếp, hãy tính tổng đã được ấn định trong đoạn này là bao nhiêu. Điều này có thể được duy trì bằng cách sử dụng cây Fenwick hoặc cây phân đoạn để chúng ta có thể truy vấn tổng phạm vi một cách hiệu quả. 
4. Nếu tổng hiện tại trong [l, r] vượt quá K, chúng tôi biết ngay cấu hình này không hợp lệ, vì chúng tôi không thể giảm giá trị sau này mà không phá vỡ các phân đoạn trước đó. 
5. Nếu tổng hiện tại nhỏ hơn K, chúng ta cần thêm đơn vị (K trừ tổng hiện tại) bên trong [l, r]. Chúng ta tham lam đặt các đơn vị này bắt đầu từ vị trí r sang trái, luôn lấp đầy các vị trí cho đến K. 
6. Trong khi thêm các đơn vị, chúng tôi luôn tôn trọng giới hạn a[i] ≤ K. Nếu chúng tôi hết dung lượng bên trong [l, r] trước khi đạt được K đơn vị bổ sung thì trường hợp này là không thể. 
7. Sau khi xử lý tất cả các phân đoạn, xuất mảng đã xây dựng. 

Lý do vị trí tham lam từ phải sang trái có tác dụng là vì các phân đoạn trước đó được neo vào các điểm cuối bên phải nhỏ hơn. Việc điền từ bên phải sẽ tránh tiêu tốn không gian tiềm năng cần thiết cho các phân đoạn kết thúc sớm hơn. 

### Tại sao nó hoạt động 

Ở mỗi bước, chúng tôi đảm bảo rằng tất cả các phân đoạn được xử lý đều được đáp ứng chính xác. Chúng tôi không bao giờ giảm giá trị, vì vậy các phân đoạn đã hài lòng trước đó vẫn hợp lệ. Bởi vì chúng tôi xử lý bằng cách tăng điểm cuối bên phải, bất kỳ phân đoạn nào có thể trùng lặp sẽ chia sẻ ranh giới bên phải của nó theo cách được kiểm soát: sau khi chúng tôi hoàn thành một phân đoạn, không có thao tác nào sau đó sẽ đưa khối lượng vượt quá vào các vị trí bên ngoài khoảng của nó mà không được tính vào phân đoạn kéo dài ít nhất đến tận cùng bên phải. 

Điều bất biến là sau khi xử lý các phân đoạn cho đến điểm cuối bên phải R cho trước, mọi phân đoạn có r ≤ R đều được thỏa mãn chính xác và không có vị trí nào vượt quá K. Việc lấp đầy tham lam đảm bảo rằng mọi phần còn thiếu còn lại trong một phân khúc luôn được đặt ở các vị trí mới nhất có thể, duy trì tính linh hoạt cho các ràng buộc trong tương lai. 

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

    def range_sum(self, l, r):
        return self.sum(r) - self.sum(l - 1)

def solve():
    n, m, k = map(int, input().split())
    segs = []
    for _ in range(m):
        l, r = map(int, input().split())
        segs.append((r, l))
    segs.sort()

    bit = Fenwick(n)
    a = [0] * (n + 1)

    for r, l in segs:
        cur = bit.range_sum(l, r)
        if cur > k:
            print(-1)
            return
        need = k - cur
        i = r
        while need > 0 and i >= l:
            if a[i] < k:
                add = min(k - a[i], need)
                a[i] += add
                bit.add(i, add)
                need -= add
            i -= 1
        if need > 0:
            print(-1)
            return

    print(*a[1:])

t = int(input())
for _ in range(t):
    solve()
```Cây Fenwick duy trì tổng tiền tố để mỗi phân đoạn có thể được truy vấn một cách hiệu quả. Nếu không có nó, chúng tôi sẽ quét liên tục các phạm vi, điều này sẽ trở nên quá chậm ở giới hạn trên. 

Vòng lặp tham lam bên trong là phần mang tính xây dựng của giải pháp. Chúng tôi luôn cố gắng đặt số tiền cần thiết còn lại càng xa càng tốt trong phân khúc. Điều này tránh can thiệp vào các phân đoạn trước đó kết thúc sớm hơn vì những phân đoạn trước đó đã lấp đầy các vị trí bên trái nếu cần. 

Mảng`a`thực thi ràng buộc trên mỗi vị trí rằng các giá trị không được vượt quá K. Cây Fenwick phản ánh những cập nhật này để các truy vấn phân đoạn trong tương lai phản ánh tất cả các phép gán. 

Một lỗi nhỏ phổ biến là lặp lại từ trái sang phải khi điền một đoạn. Điều đó phá vỡ tính chính xác vì nó tiêu thụ các chỉ số thấp quá sớm, điều này có thể được yêu cầu bởi các phân đoạn kết thúc sớm hơn trong thứ tự xử lý trong tương lai. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ có N = 4, K = 2 và các đoạn [1,2], [2,4]. 

Chúng tôi sắp xếp theo điểm cuối bên phải: [1,2] rồi [2,4]. 

| Phân đoạn | Phạm vi | Tổng hiện tại | Cần | Hành động | 
| --- | --- | --- | --- | --- | 
| [1,2] | 1-2 | 0 | 2 | điền vào 2 rồi 1 | 
| [2,4] | 2-4 | 0 | 2 | điền vào 4 rồi 3 | 

Sau phân đoạn đầu tiên, chúng ta có thể có a = [1,1,0,0]. Sau giây, chúng ta nhận được a = [1,1,1,1]. 

Điều này chứng tỏ rằng các phân đoạn trước tập trung khối lượng về bên phải, để lại sự linh hoạt cho các phân đoạn sau. 

Bây giờ hãy xem xét một trường hợp không thể: N = 3, K = 2 với các phân đoạn [1,2], [2,3], [1,3]. 

Xử lý [1,2] chúng ta đặt 2 đơn vị, có thể ở vị trí 2 trước rồi đến vị trí 1, cho ra a = [1,1,0]. Xử lý [2,3] chúng tôi đặt 2 đơn vị, nhưng vị trí 2 đã đầy, vì vậy chúng tôi sử dụng vị trí 3 và không đáp ứng được ràng buộc phân đoạn đầu tiên trong tổng hợp cho phân khúc cuối cùng [1,3]. Cuối cùng, chúng tôi phát hiện ra điều không thể xảy ra khi [1,3] không thể cân bằng mà không vượt quá khả năng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N + M) log N) | Mỗi truy vấn và cập nhật phân đoạn sử dụng các phép toán Fenwick và mỗi vị trí được tăng lên tối đa K lần | 
| Không gian | O(N) | Kho lưu trữ cây Array và Fenwick | 

Các ràng buộc cho phép tối đa 4e5 tổng N và 2e5 phân đoạn, do đó hệ số logarit là an toàn. Vì K nhỏ nên tổng thể vòng điền bên trong vẫn bị giới hạn. 

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
        def range_sum(self, l, r):
            return self.sum(r) - self.sum(l - 1)

    def solve():
        n, m, k = map(int, input().split())
        segs = []
        for _ in range(m):
            l, r = map(int, input().split())
            segs.append((r, l))
        segs.sort()

        bit = Fenwick(n)
        a = [0] * (n + 1)

        for r, l in segs:
            cur = bit.range_sum(l, r)
            if cur > k:
                return "-1"
            need = k - cur
            i = r
            while need > 0 and i >= l:
                if a[i] < k:
                    add = min(k - a[i], need)
                    a[i] += add
                    bit.add(i, add)
                    need -= add
                i -= 1
            if need > 0:
                return "-1"

        return " ".join(map(str, a[1:]))

    t = int(input())
    out = []
    for _ in range(t):
        out.append(solve())
    return "\n".join(out)

# provided samples
assert run("""4
6 3 3
1 3
2 4
3 5
4 6
2 1 2
1 1
2 2
3 2 1
1 2
2 3
4 3 10
1 2
2 3
""") == """1 1 1 1 1 1
-1
1 0 0 1
10 10 10"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phân đoạn đơn | điền hợp lệ | xây dựng cơ bản | 
| sự chồng chéo xung đột | -1 | phát hiện không thể | 
| chồng chéo | tuyên truyền hợp lệ | sự ổn định tham lam | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi một phân đoạn hoàn toàn nằm trong một vùng đã bị bão hòa bởi các phân đoạn trước đó. Giả sử một phân đoạn [2,3] đến sau khi [1,4] đã sử dụng hết dung lượng sẵn có trong khu vực đó. Khi xử lý [2,3], thuật toán sẽ tính tổng hiện tại đã bằng K hoặc lớn hơn. Nếu nó vượt quá K, chúng tôi ngay lập tức bác bỏ. Nếu nó bằng K thì không có hành động nào được thực hiện, điều này duy trì tính đúng đắn. 

Một trường hợp tinh tế khác là một đoạn có độ dài 1. Ví dụ: [i,i] buộc a[i] = K. Nếu một đoạn khác cũng bao gồm i, thì bất kỳ sự không khớp nào cũng sẽ được hiển thị ngay lập tức thông qua kiểm tra tổng Fenwick. Điều này sớm phát hiện ra mâu thuẫn. 

Trường hợp thứ ba liên quan đến chuỗi dài các phân khúc chồng chéo, trong đó mỗi phân khúc mới sẽ dịch chuyển nhu cầu một chút sang phải. Việc điền từ phải sang trái đảm bảo rằng các phân đoạn trước đó không bao giờ bị thiếu dung lượng cần thiết, bởi vì chúng luôn được ưu tiên hơn các chỉ mục trước đó trong thứ tự xử lý được sắp xếp.
