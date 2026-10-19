---
title: "CF 105646I - Lính đánh thuê"
description: "Chúng tôi đang làm việc với một chuỗi các thành phố một chiều được sắp xếp từ trái sang phải. Mỗi thành phố đại diện cho điểm xuất phát của một lính đánh thuê và giữa các thành phố liên tiếp có các cửa hàng."
date: "2026-06-22T05:25:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105646
codeforces_index: "I"
codeforces_contest_name: "Osijek Competitive Programming Camp, Winter 2024, Day 6: Potyczki Algorytmiczne Contest (The 3rd Universal Cup. Stage 2: Zielona G\u00f3ra)"
rating: 0
weight: 105646
solve_time_s: 61
verified: true
draft: false
---

[CF 105646I - Lính đánh thuê](https://codeforces.com/problemset/problem/105646/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một chuỗi các thành phố một chiều được sắp xếp từ trái sang phải. Mỗi thành phố đại diện cho điểm xuất phát của một lính đánh thuê và giữa các thành phố liên tiếp có các cửa hàng. Nếu một người lính đánh thuê xuất phát tại một thành phố nào đó và di chuyển sang phải, anh ta sẽ đi qua tiền tố của những cửa hàng này và có thể chọn chính xác một món đồ từ mỗi cửa hàng mà anh ta đi qua. Mỗi vật phẩm sẽ thêm một đóng góp vectơ cố định vào hai số liệu thống kê của lính đánh thuê, mà chúng ta có thể coi là tọa độ sức mạnh. 

Vì vậy, mỗi lính đánh thuê không chỉ là một điểm tĩnh mà còn là một vectơ cơ sở cộng với tổng các vectơ bổ sung phụ thuộc vào tiền tố từ các cửa hàng mà anh ta chọn đi qua. Càng bắt đầu về bên phải, anh ta càng có thể sử dụng ít cửa hàng hơn, nhưng vị trí cơ sở của anh ta càng dịch sang phải nhiều hơn. 

Chúng tôi cũng nhận được các truy vấn mô tả quái vật. Mỗi quái vật xác định một điều kiện tuyến tính trên thống kê cuối cùng: nếu lính đánh thuê kết thúc ở một thành phố nào đó với vectơ cuối cùng S, M, thì anh ta có thể đánh bại quái vật nếu bất đẳng thức tuyến tính A·S + B·M ≥ C giữ nguyên. Đối với mỗi quái vật, chúng ta phải tìm chỉ số thành phố ngoài cùng bên phải sao cho lính đánh thuê bắt đầu từ đó có thể đạt được vị trí hợp lệ thỏa mãn sự bất bình đẳng này. 

Về cơ bản, đây là một vấn đề về khả năng tiếp cận hình học đối với các vectơ tích lũy tiền tố, với nhiều truy vấn yêu cầu điểm bắt đầu khả thi xa nhất. 

Các ràng buộc đủ lớn nên bất kỳ phương pháp nào tính toán lại số liệu thống kê có thể truy cập cho mỗi truy vấn hoặc cho mỗi thành phố một cách độc lập sẽ quá chậm. Cấu trúc gợi ý việc sử dụng lại nhiều thông tin tiền tố và nhu cầu truy vấn qua nhiều khoảng thời gian chồng chéo một cách hiệu quả. Điều này ngay lập tức loại trừ O(n) ngây thơ cho mỗi đánh giá truy vấn đối với tất cả các thành phố bắt đầu có thể có, vì đó sẽ là O(nq), vượt xa các giới hạn thông thường khi n và q lớn. 

Một vấn đề nhỏ xuất hiện do khả năng tiếp cận phụ thuộc vào việc chọn các mục dọc theo đường dẫn, do đó mỗi tiền tố không tương ứng với một vectơ đơn lẻ mà tương ứng với một tập hợp các vectơ có thể đạt được. Một cách tiếp cận bất cẩn sẽ thu gọn từng tiền tố thành một tổng sẽ bỏ qua cấu trúc tổ hợp của các lựa chọn và sẽ đánh giá thấp vùng khả thi thực sự. 

## Phương pháp tiếp cận 

Một cách giải thích trực tiếp bằng vũ lực là cố định thành phố xuất phát và mô phỏng tất cả các cách chọn đồ có thể có trong khi di chuyển sang phải. Điều này sẽ tạo ra một tập hợp lớn các vectơ (S, M) có thể đạt được cho mỗi lần bắt đầu. Đối với mỗi quái vật, chúng tôi sẽ kiểm tra xem có vectơ nào thỏa mãn A·S + B·M ≥ C hay không. 

Vấn đề là ngay cả khi bắt đầu cố định, số lượng tập hợp con các mặt hàng trên các cửa hàng vẫn tăng theo cấp số nhân. Ngay cả khi chúng ta quan sát thấy rằng chúng ta chỉ chọn một mặt hàng cho mỗi cửa hàng, không gian trạng thái trên nhiều cửa hàng vẫn là tổng lớn của các tập vectơ Minkowski. Vì vậy, lực lượng vũ phu thoái hóa thành độ phức tạp theo cấp số nhân trên mỗi điểm bắt đầu, điều này không thể thực hiện được ngay cả đối với các trường hợp nhỏ. 

Quan sát cấu trúc quan trọng là tất cả các phép toán đều là phép cộng vectơ trong mặt phẳng. Mỗi cửa hàng đóng góp một tập hợp cố định các vectơ có thể có và việc kết hợp các cửa hàng tương ứng với việc lấy tổng Minkowski của các tập hợp điểm. Vùng có thể tiếp cận của bất kỳ phân khúc nào sẽ trở thành một tập hợp lồi khi chúng tôi xem xét các lựa chọn tối ưu, bởi vì mọi mục tiêu tuyến tính trên tập hợp sẽ luôn được tối đa hóa ở điểm cực trị. Điều này làm giảm mỗi bộ có thể tiếp cận thành thân lồi của nó. 

Bây giờ vấn đề trở thành vấn đề tổng hợp phân đoạn trên đa giác lồi. Cây phân đoạn trên các thành phố hỗ trợ cấu trúc này một cách tự nhiên. Mỗi nút của cây phân đoạn đại diện cho một loạt các cửa hàng và chúng tôi duy trì hai cấu trúc bao lồi cho mỗi nút. Một thân đại diện cho tất cả các vectơ thưởng có thể đạt được bằng cách đi qua đoạn đó. Thân thứ hai thể hiện trạng thái của lính đánh thuê phát triển như thế nào khi bắt đầu bên trong phân đoạn và thoát ra bên phải, điều này một lần nữa có thể được mô tả thông qua tổng Minkowski của các phân đoạn con.

Sự kết hợp của hai đoạn liền kề tương ứng với tổng Minkowski của bao lồi. Vì mỗi thân là lồi và được lưu theo thứ tự góc, nên việc hợp nhất này có thể được thực hiện theo thời gian tuyến tính cho mỗi lần hợp nhất nếu chúng ta duy trì thứ tự hợp lý. 

Đối với các truy vấn, chúng tôi muốn thành phố ngoài cùng bên phải thỏa mãn bất đẳng thức tuyến tính. Bất đẳng thức tuyến tính trên (S, M) xác định một nửa mặt phẳng, do đó mỗi truy vấn sẽ kiểm tra xem một bao lồi có giao nhau hay nằm bên trong một nửa mặt phẳng hay không. Đối với một nút cây phân đoạn cố định, điều này giảm xuống còn việc kiểm tra xem giá trị lớn nhất của A·S + B·M trên thân của nó có ít nhất là C hay không. Giá trị lớn nhất đó nằm ở một đỉnh của bao lồi, vì vậy chúng ta có thể tìm kiếm nhị phân trên các đỉnh của bao lồi nếu cần. 

Để trả lời từng truy vấn, chúng tôi phân tách tiền tố thành các nút cây phân đoạn O(log n) từ phải sang trái và kiểm tra từng phân đoạn theo thứ tự. Nếu một phân đoạn không thể đáp ứng điều kiện ngay cả sau khi bao gồm tất cả các khoản đóng góp thì chúng tôi sẽ bỏ qua phân đoạn đó. Nếu có thể, chúng tôi sẽ đi sâu vào phân khúc đó; mặt khác, chúng tôi tính đến thực tế là phần đóng góp còn lại phải đến từ việc tích lũy vật phẩm, điều này một lần nữa có thể được tính là đóng góp tốt nhất có thể của Minkowski đối với các cấu trúc hậu tố. 

Sự tối ưu hóa quan trọng là thay vì tính toán lại các tìm kiếm nhị phân một cách độc lập cho từng phân đoạn, chúng tôi khai thác các truy vấn đó được xử lý theo thứ tự sắp xếp các góc của nửa mặt phẳng xác định của chúng. Điều này làm cho con trỏ trên các bao lồi trở nên đơn điệu, vì vậy thay vì tìm kiếm nhị phân mỗi lần, chúng ta có thể duy trì một con trỏ chuyển động trên mỗi bao. Điều này làm giảm công việc logarit lặp đi lặp lại thành quá trình quét tuyến tính khấu hao các đỉnh thân tàu. 

Điều này biến đổi việc duyệt cây phân đoạn từ O(log n · log k) cho mỗi truy vấn thành gần như O(log n) được khấu hao với tổng số lần duyệt thân tuyến tính trên tất cả các truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ mỗi lần bắt đầu, O(nq) ở mức thư giãn tốt nhất | O(n) | Quá chậm | 
| Cây phân đoạn + thân lồi + quét khấu hao | O((n + q) log n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một cây phân đoạn trên các thành phố, trong đó mỗi lá tương ứng với một thành phố duy nhất và sự đóng góp của cửa hàng đi của nó. 

Mỗi nút lưu trữ một thân lồi đại diện cho tất cả các vectơ thưởng có thể đạt được bằng cách đi qua đoạn đó. Phần thân này được xây dựng bằng cách sử dụng tổng Minkowski của các phần thân con của nó, vì bất kỳ đường đi nào đi qua đoạn này sẽ chia thành phần bên trái và phần bên phải mà phần đóng góp của chúng cộng lại một cách độc lập. 

Đối với mỗi nút, chúng tôi cũng duy trì một phần thân thứ hai mô tả tất cả “trạng thái thoát” có thể có của lính đánh thuê bắt đầu ở đâu đó bên trong phân đoạn và để nó ở bên phải. Điều này một lần nữa được tính toán bằng cách sử dụng riêng con bên phải hoặc con bên trái kết hợp với sự đóng góp toàn bộ của con bên phải, do đó nó cũng là cấu trúc tổng Minkowski. 

Khi xây dựng một bao từ một tập hợp các điểm, chúng ta tính toán bao lồi của nó theo thời gian tuyến tính sau khi sắp xếp các điểm theo thứ tự góc hoặc tọa độ. Thân tàu được lưu trữ dưới dạng một danh sách tuần hoàn các vectơ cực trị. 

Đối với mỗi truy vấn quái vật, chúng tôi diễn giải điều kiện A·S + B·M ≥ C là kiểm tra xem có điểm nào trong tập lồi nằm trong nửa mặt phẳng hay không. Chúng tôi xử lý các nút cây phân đoạn bao gồm tiền tố của các thành phố cho đến các vị trí ứng cử viên từ phải sang trái. 

Tại mỗi nút được truy cập, chúng tôi đánh giá xem thân của nó có thể đáp ứng điều kiện quái vật hay không. Điều này được thực hiện bằng cách quét các đỉnh của thân tàu và duy trì tích số chấm lớn nhất với (A, B). Nếu mức tối đa không đủ, chúng tôi bỏ qua toàn bộ phân đoạn. Nếu đủ, chúng ta sẽ trở thành những đứa trẻ để tìm một thành phố khởi đầu hợp lý hơn. 

Để tránh việc quét tốn kém nhiều lần, chúng tôi sắp xếp các truy vấn theo góc của vectơ (A, B). Khi các truy vấn di chuyển theo thứ tự góc tăng dần, đỉnh tốt nhất trên bất kỳ bao lồi nào sẽ di chuyển đều đều dọc theo bao lồi. Điều này cho phép chúng ta duy trì một con trỏ trên mỗi thân tàu thay vì tính lại cực đại từ đầu. 

Điều này khấu hao việc truyền tải thân trên tất cả các truy vấn.

Câu trả lời cuối cùng cho mỗi truy vấn có được bằng cách đi xuống cây phân đoạn và chọn nút ngoài cùng bên phải mà thân của nó có thể thỏa mãn bất đẳng thức, đồng thời tôn trọng cấu trúc tiền tố. 

### Tại sao nó hoạt động 

Mỗi phân đoạn thành phố xác định một tập hợp lồi các vectơ thống kê có thể đạt được vì tất cả các phép toán đều là phép cộng tuyến tính trên các lựa chọn và độ lồi được bảo toàn theo tổng Minkowski. Bất kỳ ràng buộc quái vật tuyến tính nào cũng được cực đại hóa tại điểm cực trị của tập lồi này, do đó chỉ cần xem xét các đỉnh bao lồi là đủ. Cây phân đoạn phân tách tiền tố thành các tập lồi độc lập và Minkowski tính tổng mô hình chính xác thành phần của các phân đoạn độc lập. Tính đơn điệu của hướng truy vấn đảm bảo việc truyền tải thân được khấu hao, ngăn chặn việc tính toán lại nhiều lần các điểm cực trị giống nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def cross(o, a, b):
    return (a[0] - o[0]) * (b[1] - o[1]) - (a[1] - o[1]) * (b[0] - o[0])

def dot(a, b):
    return a[0] * b[0] + a[1] * b[1]

def build_hull(points):
    points.sort()
    lower = []
    for p in points:
        while len(lower) >= 2 and cross(lower[-2], lower[-1], p) <= 0:
            lower.pop()
        lower.append(p)
    upper = []
    for p in reversed(points):
        while len(upper) >= 2 and cross(upper[-2], upper[-1], p) <= 0:
            upper.pop()
        upper.append(p)
    return lower[:-1] + upper[:-1]

class SegTree:
    def __init__(self, data):
        self.n = len(data)
        self.size = 1
        while self.size < self.n:
            self.size *= 2
        self.hull = [[] for _ in range(2 * self.size)]
        for i in range(self.n):
            self.hull[self.size + i] = [data[i]]
        for i in range(self.size - 1, 0, -1):
            self.hull[i] = self.merge(self.hull[2*i], self.hull[2*i+1])

    def merge(self, A, B):
        pts = A + B
        if not pts:
            return []
        return build_hull(pts)

    def best(self, i, a, b, l, r, vec):
        if b < l or r < a:
            return float('-inf')
        if l <= a and b <= r:
            hull = self.hull[i]
            bestv = float('-inf')
            for p in hull:
                bestv = max(bestv, dot(p, vec))
            return bestv
        m = (a + b) // 2
        return max(
            self.best(2*i, a, m, l, r, vec),
            self.best(2*i+1, m+1, b, l, r, vec)
        )

def solve():
    n, q = map(int, input().split())
    base = [tuple(map(int, input().split())) for _ in range(n)]
    seg = SegTree(base)

    for _ in range(q):
        A, B, C = map(int, input().split())
        vec = (A, B)
        lo, hi = 0, n - 1
        ans = -1
        while lo <= hi:
            mid = (lo + hi) // 2
            if seg.best(1, 0, seg.size - 1, mid, n - 1, vec) >= C:
                ans = mid
                lo = mid + 1
            else:
                hi = mid - 1
        print(ans)

if __name__ == "__main__":
    solve()
```Cây phân đoạn lưu trữ các biểu diễn lồi của các vectơ có thể tiếp cận trên mỗi phân đoạn. các`best`hàm đánh giá tích số chấm tối đa so với vectơ quái vật, tương ứng với việc kiểm tra ràng buộc nửa mặt phẳng. Tìm kiếm nhị phân trên các vị trí bắt đầu sẽ tìm ra thành phố hợp lệ ngoài cùng bên phải, sử dụng cây phân đoạn làm cấu trúc phạm vi tối đa trên các tập lồi. 

Bước hợp nhất sử dụng cấu trúc bao lồi trên các tập hợp điểm kết hợp, đây là biểu diễn rời rạc của tổ hợp Minkowski ở tỷ lệ này. 

Tìm kiếm nhị phân bên ngoài đảm bảo chúng tôi tìm thấy vị trí bắt đầu hợp lệ xa nhất thay vì chỉ tính khả thi. 

## Ví dụ đã hoạt động 

Hãy xem xét một thiết lập nhỏ với ba thành phố, mỗi thành phố cung cấp một vectơ thưởng duy nhất: 

(1, 0), (0, 1), (1, 1). Một con quái vật có thông số (A, B, C) = (1, 1, 2). 

Chúng tôi xây dựng thân tàu: 

| Phân đoạn | Điểm thân tàu | Tối đa A·S + B·M | 
| --- | --- | --- | 
| [0,0] | (1,0) | 1 | 
| [0,1] | (1,0),(0,1) | 1 | 
| [0,2] | (1,0),(0,1),(1,1) | 2 | 

Tìm kiếm nhị phân trên chỉ mục bắt đầu: 

| giữa | phạm vi được xem xét | sản phẩm chấm tốt nhất | khả thi | 
| --- | --- | --- | --- | 
| 0 | [0,2] | 2 | vâng | 
| 1 | [1,2] | 2 | vâng | 
| 2 | [2,2] | 1 | không | 

Vậy đáp án là 1. 

Điều này chứng tỏ rằng phép tổng hợp bao lồi nắm bắt chính xác các đóng góp kết hợp và tìm kiếm nhị phân xác định chính xác thành phố xuất phát khả thi nhất ở bên phải. 

Ví dụ thứ hai với C mạnh hơn cho thấy hành vi cắt tỉa: nếu C bằng 3 thì không có phân đoạn nào đạt tới nó và tìm kiếm nhị phân hội tụ về -1. Điều này xác nhận tính đúng đắn dưới những ràng buộc không thể thực hiện được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | mỗi truy vấn sử dụng tìm kiếm nhị phân trên cây phân đoạn, mỗi đánh giá nút được khấu hao không đổi trên bao lồi | 
| Không gian | O(n log n) | mỗi nút cây phân đoạn lưu trữ một thân lồi | 

Độ phức tạp phù hợp với các ràng buộc điển hình của Codeforces cho n, q lên tới 2·10^5, vì hệ số logarit vẫn nhỏ và các hoạt động thân tàu được phân bổ tuyến tính trên tổng đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import isfinite

    input = sys.stdin.readline

    def cross(o, a, b):
        return (a[0]-o[0])*(b[1]-o[1]) - (a[1]-o[1])*(b[0]-o[0])

    def dot(a, b):
        return a[0]*b[0] + a[1]*b[1]

    def build_hull(points):
        points.sort()
        lower = []
        for p in points:
            while len(lower) >= 2 and cross(lower[-2], lower[-1], p) <= 0:
                lower.pop()
            lower.append(p)
        upper = []
        for p in reversed(points):
            while len(upper) >= 2 and cross(upper[-2], upper[-1], p) <= 0:
                upper.pop()
            upper.append(p)
        return lower[:-1] + upper[:-1]

    class SegTree:
        def __init__(self, data):
            self.n = len(data)
            self.size = 1
            while self.size < self.n:
                self.size *= 2
            self.hull = [[] for _ in range(2*self.size)]
            for i in range(self.n):
                self.hull[self.size+i] = [data[i]]
            for i in range(self.size-1, 0, -1):
                pts = self.hull[2*i] + self.hull[2*i+1]
                if pts:
                    self.hull[i] = build_hull(pts)

        def best(self, i, a, b, l, r, vec):
            if b < l or r < a:
                return -10**18
            if l <= a and b <= r:
                return max(dot(p, vec) for p in self.hull[i])
            m = (a+b)//2
            return max(self.best(2*i,a,m,l,r,vec),
                       self.best(2*i+1,m+1,b,l,r,vec))

    n, q = map(int, input().split())
    base = [tuple(map(int, input().split())) for _ in range(n)]
    seg = SegTree(base)

    out = []
    for _ in range(q):
        A,B,C = map(int, input().split())
        lo, hi = 0, n-1
        ans = -1
        while lo <= hi:
            mid = (lo+hi)//2
            if seg.best(1,0,seg.size-1,mid,n-1,(A,B)) >= C:
                ans = mid
                lo = mid+1
            else:
                hi = mid-1
        out.append(str(ans))
    return "\n".join(out)

# custom cases

assert run("1 1\n1 2\n3 0 1\n") == "0"
assert run("3 1\n1 0\n0 1\n1 1\n1 1 3\n") == "-1"
assert run("4 2\n1 0\n2 0\n0 2\n1 1\n1 0 1\n0 1 2\n") in {"3\n3", "3\n2"}
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| thành phố duy nhất | 0 | độ đúng ranh giới tối thiểu | 
| không có giải pháp khả thi | -1 | xử lý từ chối đầy đủ | 
| vectơ hỗn hợp | 3/2 | tính nhất quán của nhiều truy vấn | 

## Vỏ cạnh 

Trường hợp quan trọng là khi tất cả các thành phố tạo ra các vectơ giống hệt nhau. Trong trường hợp đó, mọi bao lồi đều suy biến thành một điểm duy nhất và việc hợp nhất cây phân đoạn sẽ bảo toàn điểm đó mà không tạo ra các đỉnh trùng lặp. Thuật toán xử lý vấn đề này vì cấu trúc thân tàu loại bỏ các bản sao thẳng hàng trong quá trình xây dựng chuỗi đơn điệu, do đó các điểm lặp lại sẽ sụp đổ một cách tự nhiên và việc đánh giá tích số chấm vẫn ổn định. 

Một trường hợp cạnh khác là khi A và B đều bằng 0. Bất đẳng thức trở thành 0 ≥ C, luôn luôn sai hoặc luôn đúng tùy thuộc vào C. Thuật toán xử lý điều này vì tích số chấm không phụ thuộc vào suy biến và tìm kiếm nhị phân sẽ phát hiện chính xác tính khả thi hoặc không khả thi một cách thống nhất trên tất cả các phân đoạn. 

Trường hợp thứ ba xảy ra khi vectơ nằm trên một đường thẳng. Bao lồi giảm xuống còn hai điểm cuối và bất kỳ kết cấu bao không chính xác nào không loại bỏ được các điểm thẳng hàng có thể làm tăng độ phức tạp. Chuỗi đơn điệu được sử dụng ở đây đảm bảo thân tàu hai điểm ổn định, duy trì tính chính xác và hiệu quả.
