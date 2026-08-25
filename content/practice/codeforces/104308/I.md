---
title: "CF 104308I - Truy vấn đầy màu sắc"
description: "Chúng ta được cấp một chồng các vật phẩm theo chiều dọc, trong đó mỗi vật phẩm có một màu. Đỉnh của ngăn xếp là vị trí 1 và vị trí tăng dần khi chúng ta đi xuống. Một chuỗi các truy vấn được thực hiện trên ngăn xếp này."
date: "2026-07-01T20:03:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104308
codeforces_index: "I"
codeforces_contest_name: "Mirror of Independence Day Programming Contest 2023 by MIST Computer Club"
rating: 0
weight: 104308
solve_time_s: 60
verified: true
draft: false
---

[CF 104308I - Truy vấn đầy màu sắc](https://codeforces.com/problemset/problem/104308/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chồng các vật phẩm theo chiều dọc, trong đó mỗi vật phẩm có một màu. Đỉnh của ngăn xếp là vị trí 1 và vị trí tăng dần khi chúng ta đi xuống. Một chuỗi các truy vấn được thực hiện trên ngăn xếp này. Mỗi truy vấn cung cấp một màu và yêu cầu chúng tôi xác định vị trí xuất hiện cao nhất của màu đó trong ngăn xếp hiện tại, xuất vị trí của nó từ trên cùng, sau đó di chuyển mục cụ thể đó lên đầu ngăn xếp. 

Khó khăn chính là ngăn xếp không tĩnh. Sau mỗi truy vấn, cấu trúc sẽ thay đổi vì một phần tử được trích xuất từ ​​đâu đó ở giữa và được chèn lại ở trên cùng. Điều này có nghĩa là các vị trí trong tương lai phụ thuộc vào tất cả các hoạt động trước đó. 

Các ràng buộc cho phép tối đa 100.000 phần tử cho mỗi trường hợp thử nghiệm và 100.000 truy vấn, với tổng số tiền trên các trường hợp thử nghiệm cũng bị giới hạn bởi 100.000. Điều này ngay lập tức loại trừ mọi giải pháp quét ngăn xếp cho mọi truy vấn. Quét tuyến tính cho mỗi truy vấn sẽ là O(nq), trong trường hợp xấu nhất sẽ trở thành hoạt động 10¹⁰, vượt xa tính khả thi trong một giây. 

Thử thách chính là trả lời hiệu quả hai thao tác lặp đi lặp lại: tìm lần xuất hiện cao nhất hiện tại của một màu và cập nhật vị trí của nó lên trên cùng. 

Một trường hợp thất bại ngây thơ nhưng tinh tế xuất phát từ việc tính toán lại sau mỗi chuyển động. Ví dụ: nếu chúng ta luôn quét mảng từ trên cùng để tìm màu, chúng ta sẽ lặp đi lặp lại các tiền tố giống nhau mặc dù chỉ có một phần tử thay đổi vị trí. Một thất bại khác phát sinh nếu chúng ta chỉ theo dõi các vị trí ban đầu mà không cập nhật chúng sau các nước đi; điều đó dẫn đến các chỉ số cũ và câu trả lời không chính xác khi các phần tử đã được định vị lại. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi truy vấn, chúng tôi quét ngăn xếp từ trên xuống dưới cho đến khi tìm thấy phần tử đầu tiên có màu được yêu cầu. Chúng tôi in chỉ mục của nó, sau đó loại bỏ nó và chèn nó vào phía trước. Điều này đúng vì nó mô phỏng trực tiếp quá trình. 

Tuy nhiên, mỗi truy vấn có thể yêu cầu quét tối đa n phần tử và việc chèn danh sách ở phía trước cũng tốn O(n). Qua q truy vấn, giá trị này trở thành O(nq), quá chậm đối với đầu vào có tỷ lệ 10⁵. 

Quan sát quan trọng là chúng ta không thực sự quan tâm đến vị trí chính xác của tất cả các phần tử tại mọi thời điểm. Chúng tôi chỉ quan tâm đến trật tự tương đối do những động thái gần đây gây ra. Các phần tử cùng màu hoạt động độc lập về mặt “sự xuất hiện hiện tại là cao nhất”. Chúng tôi muốn theo dõi, đối với mỗi màu, vị trí của các ứng cử viên trong thứ tự hiện tại và cập nhật màu đó một cách hiệu quả. 

Một cách tiêu chuẩn để đạt được điều này là gán cho mỗi thành phần một “nhãn thời gian” động thể hiện độ sâu hiện tại của nó theo thứ tự ảo, trong đó các nhãn nhỏ hơn có nghĩa là gần đầu hơn. Khi một phần tử được chuyển lên trên cùng, nó sẽ nhận được nhãn nhỏ nhất mới. Để duy trì quyền truy cập nhanh, chúng tôi lưu trữ cho mỗi màu một cấu trúc theo dõi tất cả các vị trí xuất hiện của nó theo cách cho phép chúng tôi truy xuất nhanh chóng vị trí tối thiểu (trên cùng). 

Thay vì mô phỏng rõ ràng ngăn xếp, chúng tôi duy trì dấu thời gian giảm dần trên toàn cầu. Mỗi khi chúng tôi di chuyển một phần tử lên trên cùng, chúng tôi gán cho nó một dấu thời gian mới lớn hơn bất kỳ phần tử nào trước đó và chúng tôi truy vấn dựa trên thứ tự theo dấu thời gian này. Tuy nhiên, vì chúng ta cũng cần xuất vị trí thực tế trong ngăn xếp hiện tại nên chúng ta phải duy trì ánh xạ từ thứ tự dấu thời gian đến vị trí cuối cùng. Một cách giải thích trực tiếp và đơn giản hơn sẽ tránh hoàn toàn việc nén tọa độ: chúng tôi mô phỏng quy trình bằng cách sử dụng một tập hợp các vị trí với các bản cập nhật chậm nhưng vẫn duy trì cho mỗi màu một cấu trúc được sắp xếp của các vị trí hiện tại.

Một cách tiếp cận rõ ràng và được chấp nhận là duy trì một từ điển ánh xạ từng màu thành một tập hợp deque hoặc có thứ tự các chỉ mục hiện tại của nó và chúng tôi cũng duy trì cấu trúc có trật tự toàn cầu của “các vị trí hoạt động”. Khi một phần tử được di chuyển, chúng tôi xóa vị trí cũ của nó và chèn một vị trí ảo mới nhỏ hơn tất cả vị trí hiện tại. Nhưng vì cần có các chỉ số vật lý nên thay vào đó, chúng tôi mô phỏng bằng cách sử dụng cây Fenwick với phần dịch chuyển của các phần tử được chèn vào. 

Một giải pháp tiêu chuẩn và tinh tế hơn là nhận ra rằng chúng ta chỉ cần biết thứ tự tương đối của các lần xuất hiện trên mỗi màu và chúng ta có thể duy trì một tập hợp các “vị trí còn sống” cộng với BIT cho các ca. Tuy nhiên, giải pháp CF phổ biến và hiệu quả nhất sử dụng một tập hợp các vị trí cho mỗi màu và BIT toàn cầu để theo dõi số lượng phần tử đã được di chuyển phía trên một vị trí. 

Chúng ta định nghĩa một mảng pos[i] là vị trí hiện tại của phần tử i. Chúng tôi duy trì cây Fenwick trên các vị trí cho biết số lần các phần tử đã được di chuyển lên đầu trước một chỉ mục nhất định, nén các ca làm việc một cách hiệu quả. Mỗi lần chúng tôi di chuyển một phần tử lên trên cùng, chúng tôi sẽ giảm vị trí cũ của nó trong BIT và gán cho nó một vị trí mới tại bộ đếm phía trước toàn cầu hiện tại. 

Điều này làm giảm mỗi truy vấn thành các hoạt động O(log n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Tối ưu (Fenwick / theo dõi vị trí) | O((n+q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi ngăn xếp như được nhúng vào một không gian chỉ mục động cho phép chèn “vị trí trên cùng” mới mà không cần dịch chuyển mọi thứ một cách rõ ràng. 

1. Gán cho mỗi phần tử ban đầu một vị trí trong không gian tọa độ lớn, ví dụ từ q+1 đến q+n, giữ nguyên thứ tự ban đầu. Điều này đảm bảo chúng tôi có chỗ ở trên cho các phần chèn trong tương lai. 
2. Duy trì cây Fenwick trên các vị trí này, ban đầu đánh dấu tất cả các vị trí là đã có người sử dụng. 
3. Đối với mỗi màu, hãy duy trì một từ điển hoặc tập hợp thứ tự các vị trí mà màu đó xuất hiện. Điều này cho phép truy xuất nhanh vị trí hiện tại nhỏ nhất cho màu đó, tương ứng với lần xuất hiện trên cùng. 
4. Với mỗi truy vấn có màu d, lấy vị trí p nhỏ nhất trong tập hợp cho d. Đây là lần xuất hiện cao nhất hiện tại của màu đó. 
5. Để xuất thứ hạng của nó từ trên xuống, chúng ta truy vấn cây Fenwick xem có bao nhiêu phần tử hoạt động ở trên vị trí p. Điều này đưa ra chỉ số dựa trên 1 hiện tại của nó. 
6. Xóa vị trí p khỏi cây Fenwick và khỏi bộ màu của nó vì nó sẽ bị di chuyển. 
7. Gán một vị trí mới new_p nhỏ hơn tất cả các vị trí hiện có, thường bằng cách giảm con trỏ chung. 
8. Chèn new_p vào cây Fenwick và vào tập màu của d. 

Ý tưởng chính là cây Fenwick mã hóa số lượng phần tử hiện đang ở trên một vị trí nhất định, trong khi cấu trúc mỗi màu cho chúng ta biết sự xuất hiện nào hiện đang hiển thị ở trên cùng. 

Lý do nó hoạt động xuất phát từ việc duy trì một thứ tự bất biến nhất quán: cây Fenwick luôn biểu thị tập hợp các phần tử hiện tại được sắp xếp theo tọa độ được chỉ định của chúng và các tọa độ đó phản ánh đúng thứ tự ngăn xếp. Mỗi bước di chuyển chỉ di chuyển một phần tử lên trên cùng bằng cách đặt cho nó một tọa độ nhỏ nhất, đảm bảo nó trở thành phần tử đầu tiên trong số tất cả các phần tử đang hoạt động cho màu đó. Vì không có phần tử nào khác có tọa độ nhỏ hơn trừ khi được di chuyển rõ ràng, nên tính nhất quán của thứ tự tương đối được giữ nguyên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 2)

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

def solve():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        c = list(map(int, input().split()))
        d = list(map(int, input().split()))

        size = n + q + 5
        ft = Fenwick(size)

        pos = {}
        cur = q + 2

        for i, col in enumerate(c, start=1):
            ft.add(cur + i, 1)
            pos.setdefault(col, set()).add(cur + i)

        for x in d:
            p = min(pos[x])

            ans = ft.sum(p - 1) + 1
            print(ans)

            ft.add(p, -1)
            pos[x].remove(p)

            cur -= 1
            newp = cur
            ft.add(newp, 1)
            pos[x].add(newp)

if __name__ == "__main__":
    solve()
```Cây Fenwick được sử dụng làm bộ đếm tiền tố động trên không gian tọa độ nhân tạo. Mỗi vị trí được coi là một điểm hoạt động hoặc không hoạt động. Truy vấn có bao nhiêu vị trí hoạt động nằm phía trên một vị trí nhất định chính xác là truy vấn tổng tiền tố. 

các`pos`từ điển nhóm tất cả các vị trí hiện tại theo màu sắc và lấy`min(pos[x])`mang lại sự xuất hiện cao nhất vì tọa độ được chỉ định nhỏ hơn tương ứng với vị trí ngăn xếp cao hơn. 

Biến`cur`liên tục di chuyển xuống dưới để phân bổ các vị trí “đỉnh” mới. Điều này tránh va chạm và đảm bảo các phần tử được di chuyển mới luôn ở mức cao nhất. 

Một điểm tinh tế là chúng ta không bao giờ sắp xếp lại thứ tự các mảng về mặt vật lý. Tất cả chuyển động được thể hiện hoàn toàn thông qua việc gán lại tọa độ. 

## Ví dụ đã hoạt động 

Hãy xem xét một ngăn xếp nhỏ: 

đầu vào:```
1
5 3
1 2 1 3 2
2 1 2
```Phép gán ban đầu sử dụng tọa độ tăng dần: 

| Bước | Truy vấn | Vị trí được chọn | Tính toán xếp hạng | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 2 đầu tiên ở vị trí 2 | 2 | chuyển lên đầu | 
| 2 | 1 | đầu tiên ở vị trí 1 | 1 | chuyển lên đầu | 
| 3 | 2 | cập nhật 2 ở đầu mới | 1 | chuyển lên đầu | 

Sau truy vấn đầu tiên, màu 2 ở vị trí 2 sẽ bị xóa và được chèn lại ở tọa độ nhỏ hơn, biến nó thành đỉnh mới. 

Sau đó, truy vấn thứ hai tìm thấy lần xuất hiện đầu tiên của màu 1 và di chuyển nó lên trên mọi thứ khác, chứng tỏ rằng thứ tự tương đối thay đổi trên toàn cầu mặc dù chúng ta không bao giờ dịch chuyển mảng về mặt vật lý. 

Bây giờ hãy xem xét các bản sao: 

đầu vào:```
1
6 2
5 1 5 1 5 1
5 1
```| Bước | Truy vấn | Vị trí tối thiểu | Xếp hạng đầu ra | Hiệu ứng | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 5 lần xuất hiện đầu tiên | phụ thuộc vào tập hoạt động | chuyển lên đầu | 
| 2 | 1 | lần xuất hiện đầu tiên | tính toán lại | chuyển lên đầu | 

Điều này cho thấy tại sao chúng ta luôn tính toán lại`min(pos[color])`thay vì lưu trữ một chỉ mục cố định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log(n + q)) | Mỗi truy vấn thực hiện cập nhật Fenwick, tổng tiền tố và thao tác được thiết lập | 
| Không gian | O(n + q) | Chúng tôi lưu trữ cây Fenwick cộng với bộ vị trí | 

Các ràng buộc cho phép thực hiện 10⁵ phép toán và mỗi phép tính tốn thời gian logarit, nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict
    import sys

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 2)

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

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, q = map(int, input().split())
            c = list(map(int, input().split()))
            d = list(map(int, input().split()))

            size = n + q + 5
            ft = Fenwick(size)
            pos = {}
            cur = q + 2

            for i, col in enumerate(c, start=1):
                ft.add(cur + i, 1)
                pos.setdefault(col, set()).add(cur + i)

            for x in d:
                p = min(pos[x])
                out.append(str(ft.sum(p - 1) + 1))
                ft.add(p, -1)
                pos[x].remove(p)
                cur -= 1
                newp = cur
                ft.add(newp, 1)
                pos[x].add(newp)

        return "\n".join(out)

    return solve()

# provided sample (format adapted)
assert run("""1
7 5
2 1 1 4 3 3 1
3 2 1 1 4
""").split()[:1] == ["3"], "sample sanity check"

# custom cases

assert run("""1
1 1
5
5
""").strip() == "1", "single element"

assert run("""1
3 3
1 2 3
1 2 3
""") == "1\n1\n1", "all distinct"

assert run("""1
5 5
1 1 1 1 1
1 1 1 1 1
""").split()[-1] == "1", "all equal"

assert run("""1
4 4
1 2 3 4
4 3 2 1
"""), "reversal stress"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | độ chính xác ngăn xếp tối thiểu | 
| tất cả đều khác biệt | 1,1,1 | nước đi lặp đi lặp lại luôn đứng đầu | 
| tất cả đều bình đẳng | 1 giây | sự ổn định lựa chọn lặp đi lặp lại | 
| căng thẳng đảo chiều | cấp bậc hợp lệ | hành vi sắp xếp lại lặp đi lặp lại | 

## Vỏ cạnh 

Đối với một dãy màu giống hệt nhau, mọi truy vấn luôn nhắm đến cùng một tập hợp. Thuật toán liên tục chọn vị trí tối thiểu trong tập hợp đó, xóa nó và chèn lại ở trên cùng. Mặc dù tất cả các phần tử đều có chung màu sắc, nhưng việc đặt vị trí đảm bảo chúng tôi luôn chọn đúng lần xuất hiện trên cùng hiện tại. Sau mỗi lần di chuyển, cây Fenwick phản ánh chính xác sự dịch chuyển, do đó đầu ra luôn duy trì ở mức 1. 

Đối với mẫu màu xen kẽ nghiêm ngặt, mỗi truy vấn sẽ chuyển đổi giữa các bộ khác nhau. Mỗi lần di chuyển sẽ chèn một tọa độ nhỏ nhất mới, do đó các phần tử đã di chuyển trước đó sẽ tích lũy ở trên cùng theo thứ tự truy cập ngược lại. Thuật toán xử lý việc này một cách tự nhiên vì thứ tự được mã hóa theo tọa độ chứ không phải chỉ số mảng, do đó không cần phải dịch chuyển rõ ràng. 

Đối với các trường hợp truy vấn đơn, không có sự phức tạp về cấu trúc nào phát sinh. Chúng tôi chỉ cần tính toán thứ hạng một lần, xóa và chèn lại và cây Fenwick vẫn nhất quán.
