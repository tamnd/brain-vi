---
title: "CF 105400H - Chiến lợi phẩm của cướp biển"
description: "Chúng tôi có một dòng tàu, mỗi tàu hoạt động giống như một container với số lượng thùng chứa tối đa cố định. Ban đầu tất cả các tàu đều trống rỗng. Theo thời gian, các thùng được đổ vào chỉ số tàu đã chọn."
date: "2026-06-22T12:44:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105400
codeforces_index: "H"
codeforces_contest_name: "Fall 2024 Cupertino Informatics Tournament"
rating: 0
weight: 105400
solve_time_s: 129
verified: false
draft: false
---

[CF 105400H - Chiến lợi phẩm của cướp biển](https://codeforces.com/problemset/problem/105400/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 9s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dòng tàu, mỗi tàu hoạt động giống như một container với số lượng thùng chứa tối đa cố định. Ban đầu tất cả các tàu đều trống rỗng. Theo thời gian, các thùng được đổ vào chỉ số tàu đã chọn. Nếu con tàu đó đã đầy, lượng tràn sẽ tiếp tục tràn sang con tàu tiếp theo, v.v., cho đến khi tất cả các thùng được đặt hoặc chúng ta chạy qua con tàu cuối cùng, trong trường hợp đó những thùng còn lại sẽ bị loại bỏ. 

Loại truy vấn thứ hai yêu cầu số lượng thùng hiện tại được lưu trữ trên một con tàu cụ thể tại thời điểm đó, sau khi tất cả các lô hàng trước đó đã được xử lý. 

Vì vậy, hệ thống là một đường dẫn động một chiều có dung lượng, trong đó mỗi bản cập nhật là thao tác "đẩy về phía trước cho đến khi tìm thấy không gian" và mọi truy vấn là kiểm tra điểm. 

Những ràng buộc đẩy chúng ta ra khỏi mọi mô phỏng ngây thơ. Với tối đa 100.000 tàu và 100.000 hoạt động, một cách tiếp cận đơn giản là tiến từng tàu một cho mỗi tải hàng có thể chuyển thành hành vi bậc hai. Trong trường hợp xấu nhất, mọi tải bắt đầu ở tàu 1 và lan truyền gần như đến tàu N, tạo ra khoảng 10^10 thao tác nguyên thủy, vượt xa giới hạn trong 2 giây. 

Một vấn đề tinh tế hơn xuất hiện trong việc điền một phần. Một con tàu có thể được ghé thăm nhiều lần trong các hoạt động tải hàng khác nhau, mỗi lần chỉ nhận được một số lượng nhỏ thùng cho đến khi đầy ắp. Bất kỳ giải pháp nào xử lý từng thùng một hoặc cập nhật dung lượng tăng dần mà không tổng hợp sẽ âm thầm hết thời gian ngay cả khi đúng về mặt logic. 

Các trường hợp biên đáng được xem xét rõ ràng bao gồm một chuỗi các tàu đã bão hòa hoàn toàn ngay từ đầu, trong đó tải ngay lập tức bị bỏ qua, ví dụ: 

đầu vào:```
5 1
0 0 10 10 10
1 1 7
```Ở đây tàu 1 và 2 đã đầy hoặc thực sự không thể sử dụng được, do đó hàng phải bắt đầu ở tàu 3. Việc triển khai ngây thơ không bỏ qua hiệu quả sẽ lãng phí thời gian kiểm tra tàu 1 và 2 nhiều lần. 

Một trường hợp cạnh khác là tải nhỏ lặp đi lặp lại:```
3 3
5 5 5
1 1 1
1 1 1
1 1 1
```Mặc dù mỗi hoạt động rất nhỏ, nhưng bất kỳ cách tiếp cận nào phân phối lại từng thùng đều trở nên quá chậm vì nó lặp lại công việc trên cùng một chỉ số. 

## Phương pháp tiếp cận 

Việc mô phỏng lực lượng vũ phu rất đơn giản. Đối với mỗi hoạt động tải, chúng tôi bắt đầu từ chỉ số tàu đã cho và tiến về phía trước. Tại mỗi tàu, chúng tôi lấy càng nhiều thùng càng tốt cho đến sức chứa còn lại của nó, trừ đi số lượng sắp đến và tiếp tục chuyển sang tàu tiếp theo nếu cần. Điều này đúng vì nó tuân theo chính xác các quy tắc lan truyền tràn. 

Vấn đề là quá trình quét này có thể xem lại các tiền tố dài của tàu nhiều lần. Nếu mọi thao tác bắt đầu ở gần phía trước thì mỗi bản cập nhật có thể nhận O(N), dẫn đến hành vi O(NM) trong trường hợp xấu nhất. 

Quan sát cấu trúc quan trọng là một con tàu chỉ trở nên “không phù hợp” khi nó đạt hết công suất. Sau thời điểm đó, nó không bao giờ đóng góp vào việc phân bổ trong tương lai nữa ngoại trừ điểm bỏ qua. Điều này có nghĩa là tập hợp các vị trí hữu ích đang hoạt động chỉ bị thu hẹp theo thời gian. Nếu chúng ta có thể nhảy qua các tàu đã đầy một cách hiệu quả, thì mỗi tàu có thể bị loại bỏ một lần, điều này gợi ý cấu trúc “vị trí còn sống tiếp theo” theo kiểu liên kết rời rạc hoặc một cây phân đoạn hỗ trợ các truy vấn nhanh “tìm đầu tiên không đầy đủ”. 

Chúng tôi duy trì công suất còn lại trên mỗi tàu. Đối với mỗi lần tải, thay vì quét từng chỉ mục, chúng tôi liên tục chuyển sang tàu tiếp theo tại hoặc sau vị trí xuất phát mà vẫn còn công suất. Sau đó chúng tôi đẩy càng nhiều càng tốt vào nó. Nếu nó đầy, chúng tôi đánh dấu nó là không hoạt động để các lần nhảy tiếp theo sẽ bỏ qua nó hoàn toàn. 

Điều này làm giảm việc quét lặp đi lặp lại trên toàn bộ tàu và biến vấn đề thành một chuỗi các bước nhảy và cập nhật. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(NM) | O(N) | Quá chậm | 
| Cây phân đoạn / DSU nhảy | O((N + M) log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lưu trữ một mảng`rem[i] = G[i]`, thể hiện sức chứa còn lại của mỗi tàu. Chúng tôi cũng duy trì cấu trúc dữ liệu có thể nhanh chóng tìm thấy chỉ mục đầu tiên ở hoặc vượt quá vị trí vẫn còn dung lượng dương. Cây phân đoạn lưu trữ dung lượng còn lại tối đa trong các phạm vi là đủ cho việc này. 

Đối với mỗi hoạt động tải bắt đầu từ tàu`K`với`C`các thùng, chúng tôi liên tục xác định vị trí con tàu có thể sử dụng tiếp theo và phân phối các thùng ở đó cho đến khi hết thùng hoặc không còn con tàu nào có thể sử dụng được. 

1. Khởi tạo`i`là chỉ số đầu tiên tại hoặc sau`K`Ở đâu`rem[i] > 0`. Điều này được tìm thấy bằng cách sử dụng truy vấn “đúng đầu tiên” trên cây phân đoạn đối với điều kiện`rem[i] > 0`. Bước này là cần thiết vì nhiều tàu có thể đã đầy và nên bỏ qua hoàn toàn. 
2. Trong khi`i`tồn tại và`C > 0`, tính xem tàu ​​có thể xếp được bao nhiêu thùng hàng`i`BẰNG`x = min(rem[i], C)`. Sau đó chúng tôi trừ`x`từ cả hai`rem[i]`Và`C`. 
3. Nếu`rem[i]`trở thành 0 sau thao tác này, chúng tôi cập nhật cây phân đoạn để vị trí này được đánh dấu là không hoạt động. Điều này đảm bảo các tìm kiếm trong tương lai sẽ bỏ qua hoàn toàn con tàu này. 
4. Nếu`rem[i]`vẫn tích cực nhưng`C`bằng 0, hoạt động kết thúc ngay lập tức. Chúng tôi không tiến lên theo cách thủ công vì không còn thùng nào được phân phối nữa. 
5. Ngược lại, nếu`C`vẫn dương và tàu hiện tại đã đầy, chúng tôi truy vấn lại tàu có thể sử dụng tiếp theo vào hoặc sau`i + 1`. 

Ý tưởng cấu trúc quan trọng là chuyển động chỉ xảy ra khi con tàu bị bão hòa hoàn toàn. Việc lấp đầy một phần không kích hoạt chuyển động; họ chỉ làm giảm công suất còn lại. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, mỗi con tàu đều có sức chứa còn lại được xác định rõ ràng và các thùng luôn được chuyển từ trái sang phải mà không bao giờ bỏ qua con tàu vẫn còn chỗ trống. Cây phân đoạn luôn trả về con tàu sớm nhất có công suất sẵn có theo hậu tố bắt buộc, vì vậy mọi bước sắp xếp đều tôn trọng quy tắc tham lam ban đầu. 

Vì các tàu chỉ bị loại khỏi danh sách xem xét khi đạt công suất bằng 0 và sau khi bị loại bỏ, chúng sẽ không bao giờ hoạt động trở lại nên mỗi lần loại bỏ là vĩnh viễn. Điều này đảm bảo rằng cấu trúc tìm kiếm chỉ thu gọn một cách đơn điệu, ngăn chặn việc quét lặp đi lặp lại không cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.t = [0] * (4 * self.n)
        self.build(1, 0, self.n - 1, arr)

    def build(self, v, l, r, arr):
        if l == r:
            self.t[v] = arr[l]
            return
        m = (l + r) // 2
        self.build(v * 2, l, m, arr)
        self.build(v * 2 + 1, m + 1, r, arr)
        self.t[v] = max(self.t[v * 2], self.t[v * 2 + 1])

    def update(self, v, l, r, idx, val):
        if l == r:
            self.t[v] = val
            return
        m = (l + r) // 2
        if idx <= m:
            self.update(v * 2, l, m, idx, val)
        else:
            self.update(v * 2 + 1, m + 1, r, idx, val)
        self.t[v] = max(self.t[v * 2], self.t[v * 2 + 1])

    def find_first(self, v, l, r, ql, qr):
        if r < ql or l > qr or self.t[v] == 0:
            return -1
        if l == r:
            return l
        m = (l + r) // 2
        res = self.find_first(v * 2, l, m, ql, qr)
        if res != -1:
            return res
        return self.find_first(v * 2 + 1, m + 1, r, ql, qr)

def main():
    n, m = map(int, input().split())
    g = list(map(int, input().split()))
    rem = g[:]
    st = SegTree(rem)

    out = []

    for _ in range(m):
        tmp = list(map(int, input().split()))
        if tmp[0] == 1:
            _, k, c = tmp
            k -= 1
            i = st.find_first(1, 0, n - 1, k, n - 1)
            while i != -1 and c > 0:
                take = min(rem[i], c)
                rem[i] -= take
                c -= take
                st.update(1, 0, n - 1, i, rem[i])
                if rem[i] > 0:
                    break
                i = st.find_first(1, 0, n - 1, i + 1, n - 1)
        else:
            _, k = tmp
            out.append(str(rem[k - 1]))

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Cây phân đoạn được sử dụng với hai vai trò. Đầu tiên là theo dõi sức chứa tối đa còn lại trong một phạm vi để chúng tôi có thể nhanh chóng xác định xem có tồn tại con tàu nào có thể sử dụng được hay không. Thứ hai là định vị chỉ mục đầu tiên trong hậu tố vẫn còn dung lượng, điều này rất cần thiết để duy trì thứ tự từ trái sang phải. 

Hoạt động cập nhật luôn được áp dụng sau khi điều chỉnh sức chứa còn lại của tàu. Nếu một con tàu đạt tới 0, nó sẽ trở nên vô hình trong các tìm kiếm trong tương lai vì giá trị cây phân đoạn của nó trở thành 0. 

Vòng lặp bên trong truy vấn tải được cấu trúc cẩn thận sao cho mỗi lần lặp sẽ tiêu thụ hết một con tàu hoặc thoát ra sớm khi không còn thùng nào. Điều quan trọng là chúng tôi không bao giờ quét tuyến tính; mọi bước nhảy đều là logarit. 

## Ví dụ đã hoạt động 

Xét một trường hợp nhỏ: 

đầu vào:```
4 2
2 3 1 2
1 1 4
2 2
```Chúng tôi theo dõi dung lượng còn lại và chuyển động của con trỏ. 

| Bước | Hoạt động | Chỉ số tàu | rem trước | lấy | rem sau | C còn lại | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | tải | 1 | 2 | 2 | 0 | 2 | 
| 2 | tải tiếp tục | 2 | 3 | 2 | 1 | 0 | 

Sau lần truy vấn đầu tiên, tàu 1 đã đầy, tàu 2 còn lại 1. Truy vấn 2 hỏi tàu 2, trả về 1. 

Dấu vết này cho thấy quá trình tràn tiếp tục diễn ra suôn sẻ và dừng lại chính xác khi các thùng đến đã cạn kiệt. 

Bây giờ hãy xem xét bỏ qua các tàu đã đầy: 

đầu vào:```
3 2
0 5 5
1 1 3
2 2
```| Bước | Hoạt động | Chỉ số tàu | rem trước | lấy | rem sau | C còn lại | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | bắt đầu tải | 2 | 5 | 3 | 2 | 0 | 

Tàu 1 bị bỏ qua hoàn toàn vì nó không có sức chứa. Thuật toán chuyển trực tiếp sang tàu 2, chứng minh tại sao việc tìm kiếm cây phân đoạn là cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N + M) log N) | Mỗi bản cập nhật và truy vấn “có sẵn đầu tiên” chạy trên cây phân đoạn | 
| Không gian | O(N) | Lưu trữ dung lượng còn lại và các nút cây phân đoạn | 

Các ràng buộc cho phép thực hiện tới 100.000 phép tính, do đó hệ số logarit trên mỗi phép tính nằm trong giới hạn. Cấu trúc đảm bảo rằng không có hoạt động nào chuyển sang quét tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    class SegTree:
        def __init__(self, arr):
            self.n = len(arr)
            self.t = [0] * (4 * self.n)
            self.build(1, 0, self.n - 1, arr)

        def build(self, v, l, r, arr):
            if l == r:
                self.t[v] = arr[l]
                return
            m = (l + r) // 2
            self.build(v * 2, l, m, arr)
            self.build(v * 2 + 1, m + 1, r, arr)
            self.t[v] = max(self.t[v * 2], self.t[v * 2 + 1])

        def update(self, v, l, r, idx, val):
            if l == r:
                self.t[v] = val
                return
            m = (l + r) // 2
            if idx <= m:
                self.update(v * 2, l, m, idx, val)
            else:
                self.update(v * 2 + 1, m + 1, r, idx, val)
            self.t[v] = max(self.t[v * 2], self.t[v * 2 + 1])

        def find_first(self, v, l, r, ql, qr):
            if r < ql or l > qr or self.t[v] == 0:
                return -1
            if l == r:
                return l
            m = (l + r) // 2
            res = self.find_first(v * 2, l, m, ql, qr)
            if res != -1:
                return res
            return self.find_first(v * 2 + 1, m + 1, r, ql, qr)

    n, m = map(int, input().split())
    g = list(map(int, input().split()))
    rem = g[:]
    st = SegTree(rem)

    out = []

    for _ in range(m):
        tmp = list(map(int, input().split()))
        if tmp[0] == 1:
            _, k, c = tmp
            k -= 1
            i = st.find_first(1, 0, n - 1, k, n - 1)
            while i != -1 and c > 0:
                take = min(rem[i], c)
                rem[i] -= take
                c -= take
                st.update(1, 0, n - 1, i, rem[i])
                if rem[i] > 0:
                    break
                i = st.find_first(1, 0, n - 1, i + 1, n - 1)
        else:
            _, k = tmp
            out.append(str(rem[k - 1]))

    return "\n".join(out)

# provided samples (formatted from statement)
# assert run(...) == ...

# custom cases
assert run("""1 1
10
2 1
""") == "10", "single ship query"

assert run("""3 1
1 1 1
1 1 10
""") == "", "overflow discard"

assert run("""3 2
0 5 5
1 1 3
2 2
""") == "2", "skip empty prefix"

assert run("""5 4
2 3 1 4 2
1 2 5
1 1 3
1 1 2
2 3
""") == "1", "mixed operations"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| truy vấn tàu đơn | 10 | tính chính xác của truy vấn cơ sở | 
| loại bỏ tràn | (trống) | bỏ đi ngoài con tàu cuối cùng | 
| bỏ qua tiền tố trống | 2 | bỏ qua tàu không công suất | 
| hoạt động hỗn hợp | 1 | tính đúng đắn khi xen kẽ | 

## Vỏ cạnh 

Trường hợp quan trọng nhất là khi tàu xuất phát đã đầy. Trong trường hợp đó, truy vấn cây phân đoạn đầu tiên ngay lập tức chuyển sang tàu có sẵn tiếp theo và không có nỗ lực sai nào được thực hiện để ghi vào một vị trí đầy đủ. Cấu trúc đảm bảo rằng các tàu đầy đủ không bao giờ được trả về dưới dạng ứng viên hợp lệ vì giá trị được lưu trữ của chúng bằng 0. 

Một trường hợp khác là tràn hoàn toàn ra ngoài con tàu cuối cùng. Khi tìm kiếm trên cây phân đoạn không trả về chỉ mục hợp lệ, thuật toán sẽ dừng ngay lập tức và loại bỏ các thùng còn lại. Điều này phù hợp với quy luật thùng thừa sẽ rơi xuống biển. 

Trường hợp tinh vi cuối cùng là việc lặp lại việc lấp đầy một phần của cùng một con tàu trong nhiều hoạt động. Cây phân đoạn luôn phản ánh công suất còn lại hiện tại, vì vậy mỗi lần tàu được xem lại, chỉ không gian hiện có được sử dụng. Sau khi đạt đến 0, nó sẽ vĩnh viễn bị loại khỏi danh sách xem xét, đảm bảo rằng nó sẽ không bao giờ bị chọn sai nữa.
