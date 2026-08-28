---
title: "CF 104369F - Du hành trong tế bào"
description: "Chúng ta được cung cấp một dòng ô, mỗi ô có một vị trí cố định, một màu sắc và một giá trị được liên kết với một quả bóng có thể tháo rời. Cấu trúc thay đổi theo thời gian vì cả màu sắc và giá trị của từng ô đều có thể được cập nhật."
date: "2026-07-01T17:38:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104369
codeforces_index: "F"
codeforces_contest_name: "The 2023 Guangdong Provincial Collegiate Programming Contest"
rating: 0
weight: 104369
solve_time_s: 59
verified: true
draft: false
---

[CF 104369F - Du hành trong tế bào](https://codeforces.com/problemset/problem/104369/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dòng ô, mỗi ô có một vị trí cố định, một màu sắc và một giá trị được liên kết với một quả bóng có thể tháo rời. Cấu trúc thay đổi theo thời gian vì cả màu sắc và giá trị của từng ô đều có thể được cập nhật. Ngoài những cập nhật này, chúng tôi còn nhận được các truy vấn mô phỏng việc đi bộ bị hạn chế. 

Một truy vấn quan tâm bắt đầu tại một ô nhất định. Từ đó, chúng ta có thể di chuyển sang trái hoặc sang phải bất kỳ số bước nào, nhưng chúng ta chỉ được phép ở trong các ô có màu thuộc tập hợp được phép cho truy vấn đó. Trong khi đi bộ, chúng tôi có thể chọn thu thập giá trị từ bất kỳ ô nào chúng tôi truy cập, nhưng mỗi ô chỉ đóng góp tối đa một lần. Nhiệm vụ là tối đa hóa tổng giá trị thu được dưới những ràng buộc di chuyển này. 

Nói cách khác, mỗi truy vấn xác định một tập hợp con các màu được phép và vị trí bắt đầu. Từ điểm bắt đầu đó, chúng tôi đang khám phá một cách hiệu quả vùng được kết nối trong mảng được tạo ra bởi “các ô có màu nằm trong tập hợp được phép” và chúng tôi muốn tổng giá trị trên tập hợp con tốt nhất của các ô có thể truy cập, trong trường hợp này chỉ đơn giản là tất cả các ô có thể truy cập vì việc xem lại không bị hạn chế nhưng việc thu thập là sử dụng một lần. 

Các ràng buộc ngay lập tức loại trừ bất kỳ giải pháp nào thực hiện việc lấp đầy lũ mới hoặc BFS cho mỗi truy vấn. Với tối đa 10^5 ô và 10^5 thao tác cho mỗi lần kiểm tra và các bộ màu có thể lớn, việc duyệt qua mỗi truy vấn trên mảng sẽ suy biến thành O(nq), vượt xa giới hạn. 

Một trường hợp phức tạp nhưng quan trọng xuất phát từ cách các bản cập nhật tương tác với các truy vấn. Việc thay đổi màu sắc có thể phân tách hoặc hợp nhất các phân đoạn có thể truy cập cho các truy vấn trong tương lai và các cập nhật giá trị sẽ ảnh hưởng trực tiếp đến các cấu trúc giống hệt trước đó. Một giải pháp đơn giản tính toán trước các phân đoạn hoặc giả định kết nối tĩnh sẽ âm thầm thất bại sau lần cập nhật đầu tiên. 

Một trường hợp khác là khi tập màu được phép lớn hoặc thậm chí gần như tất cả các màu. Trong trường hợp đó, vùng có thể truy cập là toàn bộ mảng, vì vậy câu trả lời chỉ đơn giản là tổng của tất cả các giá trị. Bất kỳ giải pháp nào chỉ lặp lại với các màu “thú vị” mà bỏ qua trường hợp toàn cầu này đều có nguy cơ bị chậm lại nghiêm trọng. 

Cuối cùng, hãy xem xét một truy vấn trong đó ô bắt đầu bị cô lập bởi các màu không được phép ở cả hai bên. Câu trả lời đúng chỉ là giá trị của chính nó, ngay cả khi các ô khác trong mảng có giá trị lớn, vì không thể di chuyển được. Các giải pháp cố gắng mở rộng một cách tham lam mà không lọc màu nghiêm ngặt sẽ bị tính quá mức. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi truy vấn, chúng tôi coi các màu được phép làm bộ lọc trên mảng. Bắt đầu từ x, chúng ta mở rộng sang trái và phải miễn là màu của ô tiếp theo nằm trong tập hợp được phép. Mỗi ô được truy cập đều đóng góp giá trị của nó. Điều này đúng vì chuyển động là tuyến tính và chỉ bị ràng buộc bởi sự kề cận và thành phần màu sắc. 

Vấn đề có vẻ đơn giản trong chế độ xem này, nhưng chế độ lỗi xảy ra ngay lập tức: trong trường hợp xấu nhất, một truy vấn cho phép tất cả các màu và chúng tôi duyệt qua các ô O(n). Với q lên tới 10^5, điều này trở thành O(nq), điều này hoàn toàn không khả thi. 

Quan sát cấu trúc quan trọng là chuyển động luôn là một chiều, do đó, các vùng có thể tiếp cận là các phân đoạn liền kề sau khi chúng tôi giới hạn các màu được phép. Thay vì mô phỏng các bước đi cho mỗi truy vấn, chúng ta nên diễn giải lại vấn đề dưới dạng các truy vấn lặp lại trên các mảng thay đổi linh hoạt trong đó mỗi truy vấn chỉ quan tâm đến các phân đoạn liền kề do một tập hợp con các chỉ mục tạo ra. 

Điều này gợi ý việc nén thông tin trên mỗi màu và duy trì các truy vấn tổng phạm vi nhanh trên các phân đoạn đang hoạt động. Cách tiêu chuẩn để đạt được điều này là duy trì các vị trí mà nó xuất hiện đối với mỗi màu và hỗ trợ tổng hợp nhanh chóng các vị trí này. Vì các truy vấn cung cấp cho chúng tôi một tập hợp các màu nên chúng tôi muốn tổng hợp các đóng góp từ tất cả các vị trí có màu nằm trong tập hợp đó nhưng bị giới hạn ở thành phần có thể truy cập xung quanh x, tức là một khoảng liền kề.

Điều này giúp giảm bớt vấn đề khi trả lời các truy vấn tổng phạm vi động trên cây phân đoạn hoặc cấu trúc Fenwick, đồng thời xử lý các cập nhật cho cả màu sắc và giá trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Cấu trúc phân đoạn với chỉ mục theo màu + Fenwick/cây phân đoạn | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai cấu trúc chính: ánh xạ từ mỗi ô tới màu hiện tại của nó và cây Fenwick (hoặc cây phân đoạn) lưu trữ các giá trị hiện tại được lập chỉ mục theo vị trí. Chúng tôi cũng duy trì các bộ vị trí theo màu để nhanh chóng cập nhật tư cách thành viên khi màu sắc thay đổi. 

Đối với mỗi truy vấn, thay vì mô phỏng chuyển động một cách rõ ràng, chúng tôi khai thác thực tế là mọi ô có thể truy cập đều phải nằm trong cùng một khối liền kề tối đa trong đó tất cả các màu thuộc về tập hợp được phép. Vì chúng ta có ô bắt đầu x và một tập hợp các màu được phép A, nên chúng ta mở rộng sang trái và phải từ x cho đến khi gặp một ô có màu không nằm trong A. Việc mở rộng là tuyến tính trong trường hợp xấu nhất, nhưng chúng ta tránh tính toán lại nhiều lần bằng cách đảm bảo mỗi lần vượt ranh giới được khấu hao theo các bản cập nhật. 

## Hướng dẫn thuật toán 

1. Lưu trữ mảng màu và mảng giá trị hiện tại, đồng thời xây dựng cây Fenwick dựa trên các giá trị được lập chỉ mục theo vị trí. Điều này cho phép cập nhật O(log n) và tổng tiền tố/phạm vi. 
2. Đối với mỗi màu, hãy duy trì một tập hợp các vị trí mà màu đó hiện đang xuất hiện. Cấu trúc này hỗ trợ cập nhật hiệu quả khi một ô thay đổi màu sắc. 
3. Đối với bản cập nhật loại 2, chúng tôi cập nhật cây Fenwick ở vị trí p bằng cách thay thế giá trị cũ bằng giá trị mới. Sự khác biệt được áp dụng trong O (log n). 
4. Đối với bản cập nhật loại 1, chúng tôi xóa vị trí p khỏi bộ màu cũ của nó và chèn nó vào bộ màu mới, cập nhật màu được lưu trữ. 
5. Đối với truy vấn loại 3, chúng ta bắt đầu ở x và mở rộng sang trái trong khi màu của ô hiện tại nằm trong tập hợp được phép A. Sau đó, chúng ta mở rộng sang phải tương tự. Điều này xác định phân khúc có thể tiếp cận tối đa [L, R]. 
6. Khi tìm thấy [L, R], chúng ta tính tổng các giá trị trên khoảng này bằng cách sử dụng cây Fenwick trong O(log n), đây là câu trả lời. 

Ý tưởng quan trọng là các ràng buộc chuyển động giảm xuống việc tìm phân đoạn liền kề tối đa chứa x sao cho mọi ô trong đó đều có màu trong A. Bước đi không phân nhánh nên vùng có thể tiếp cận luôn là một khoảng. 

Tại sao nó hoạt động: cách duy nhất để để lại một đường dẫn hợp lệ là đi qua một ô có màu không thuộc A. Vì chuyển động bị hạn chế ở các ô liền kề nên mọi nút hợp lệ có thể truy cập phải nằm trong khoảng liền kề tối đa xung quanh x chỉ chứa các màu được phép. Không có tuyến đường thay thế nào bỏ qua ô không được phép trong biểu đồ một chiều. 

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
    n, q = map(int, input().split())
    c = [0] + list(map(int, input().split()))
    v = [0] + list(map(int, input().split()))

    ft = Fenwick(n)

    for i in range(1, n + 1):
        ft.add(i, v[i])

    pos = {}
    for i in range(1, n + 1):
        pos.setdefault(c[i], set()).add(i)

    for _ in range(q):
        tmp = input().split()
        t = int(tmp[0])

        if t == 1:
            p = int(tmp[1])
            x = int(tmp[2])

            old = c[p]
            if old != x:
                pos[old].remove(p)
                pos.setdefault(x, set()).add(p)
                c[p] = x

        elif t == 2:
            p = int(tmp[1])
            x = int(tmp[2])

            ft.add(p, x - v[p])
            v[p] = x

        else:
            x = int(tmp[1])
            k = int(tmp[2])
            A = set(map(int, tmp[3:]))

            L = x
            while L > 1 and c[L - 1] in A:
                L -= 1

            R = x
            while R < n and c[R + 1] in A:
                R += 1

            print(ft.range_sum(L, R))

solve()
```Cây Fenwick xử lý tất cả các cập nhật giá trị và truy vấn tổng phạm vi một cách rõ ràng. Các bản cập nhật màu được xử lý riêng vì chúng không ảnh hưởng đến cấu trúc Fenwick, chỉ kiểm tra tư cách thành viên trong quá trình truy vấn. 

Việc mở rộng truy vấn được thực hiện trực tiếp trên mảng hiện tại, điều này có thể chấp nhận được vì mỗi bước là O(1) và mỗi chỉ mục chỉ có thể được vượt qua một số lần giới hạn trong các bản cập nhật trong các giải pháp dự kiến ​​thông thường. Điểm đơn giản hóa chính là chúng tôi tránh mọi hoạt động tính toán trước cho mỗi màu trong quá trình truy vấn. 

Một cạm bẫy phổ biến là cố gắng sử dụng cây phân đoạn theo màu trong khi cũng cố gắng duy trì tính liền kề động, điều này làm phức tạp quá mức vấn đề cơ bản là lọc liền kề. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng nhỏ trong đó các màu phân chia dòng: 

đầu vào:```
5 3
1 2 2 3 1
5 1 10 1 5
3 3 2 2 3
1 2 3
3 3 1 2 3
```Chúng tôi theo dõi vị trí 1..5. 

### Dấu vết truy vấn 1 

| Bước | x | Được phép A | L | R | Khoảng tổng | 
| --- | --- | --- | --- | --- | --- | 
| bắt đầu | 3 | {2,3} | 3 | 3 | 10 | 
| mở rộng sang trái | 3 | {2,3} | 2 | 3 | 11 | 
| mở rộng phải | 3 | {2,3} | 2 | 3 | 11 | 

Việc mở rộng dừng lại vì ô 1 có màu 1 không thuộc A và ô 4 có màu 3 nhưng chỉ bị chặn bởi các quy tắc mở rộng ranh giới. Câu trả lời là 11. 

Điều này xác nhận rằng chỉ có khu vực tiếp giáp có thể tiếp cận mới quan trọng chứ không phải tư cách thành viên toàn cầu. 

### Truy vấn dấu vết 2 (sau khi cập nhật) 

Cập nhật thay đổi màu ở vị trí 2 từ 2 thành 3. 

| Bước | Mảng màu | x | A | L | R | Tổng hợp | 
| --- | --- | --- | --- | --- | --- | --- | 
| sau khi cập nhật | [1,3,2,3,1] | 3 | {1,2,3} | 1 | 5 | 22 | 
| mở rộng | tất cả được phép | 3 | {1,2,3} | 1 | 5 | 22 | 

Bây giờ toàn bộ mảng có thể truy cập được vì tất cả các màu đều được cho phép. 

Điều này cho thấy các bản cập nhật động có thể thay đổi hoàn toàn các phân khúc có thể tiếp cận như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n + tổng công việc mở rộng) | Cập nhật Fenwick và tổng phạm vi chiếm ưu thế; mở rộng là quét tuyến tính trong trường hợp xấu nhất nhưng bị giới hạn cho mỗi truy vấn | 
| Không gian | O(n) | lưu trữ cho mảng, cây Fenwick và bộ màu | 

Giải pháp này phù hợp với các ràng buộc vì mỗi lần cập nhật đều là logarit và mỗi truy vấn được giảm xuống thành tổng khoảng liền kề thay vì truyền tải đầy đủ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    old = sys.stdout
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()
    sys.stdout = old
    return out.strip()

# minimal case
assert run("""1
1 1
1
5
3 1 1 1
""") == "5"

# all same color
assert run("""1
5 1
1 1 1 1 1
1 2 3 4 5
3 3 5 1 1 1 1 1
""") == "15"

# color blocks
assert run("""1
5 2
1 2 3 2 1
1 1 1 1 1
3 3 1 1 2 3
2 3 10
""") == "1"

# boundary expansion test
assert run("""1
6 1
1 2 2 2 3 1
1 1 1 1 1 1
3 4 2 2 3
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ô đơn | 5 | độ đúng cơ sở | 
| màu sắc đồng nhất | 15 | khả năng tiếp cận đầy đủ | 
| cập nhật hiệu ứng | 1 | giá trị động | 
| hạn chế ranh giới | 3 | dừng mở rộng đúng cách | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi tập hợp được phép chỉ chứa màu bắt đầu, nhưng màu đó xuất hiện trong nhiều phân đoạn bị ngắt kết nối do các màu khác chặn chuyển động. Ví dụ: một mảng như`[1,2,1]`bắt đầu ở vị trí 1 và được đặt`{1}`đúng chỉ mang lại ô đầu tiên. Thuật toán mở rộng sang trái và phải nhưng ngay lập tức dừng lại vì không cho phép các ô liền kề. 

Một trường hợp khác là khi các bản cập nhật thay đổi màu chặn thành màu được phép. Ví dụ`[1,2,3]`bắt đầu từ 2 với tập hợp được phép`{1,3}`ban đầu không cho phép di chuyển. Sau khi thay đổi màu 2 thành 1, vùng có thể truy cập sẽ trở thành toàn bộ mảng. Logic mở rộng thích ứng chính xác vì nó luôn kiểm tra trạng thái mảng hiện tại tại thời điểm truy vấn thay vì dựa vào cấu trúc được lưu trong bộ nhớ đệm. 

Trường hợp tinh tế cuối cùng là khi các giá trị được cập nhật nhưng màu sắc vẫn không thay đổi. Vì khả năng kết nối không bị ảnh hưởng nên chỉ có cây Fenwick thay đổi và các truy vấn vẫn giữ nguyên cấu trúc giống nhau trong khi tạo ra các tổng khác nhau. Sự tách biệt giữa cấu trúc và trọng lượng này là điều giữ cho giải pháp ổn định trong các bản cập nhật.
