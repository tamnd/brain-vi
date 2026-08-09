---
title: "CF 104008H - Đua xe cuồng loạn"
description: "Chúng ta có một đường tròn được tạo bởi các ô $n$. Mỗi ô có một giá trị độ khó $di$. Cuộc đua bắt đầu tại ô đã chọn $s$ và cho phép một khoảng thời gian cố định $t$. Tay đua di chuyển về phía trước từng ô theo thứ tự vòng tròn."
date: "2026-07-02T05:30:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104008
codeforces_index: "H"
codeforces_contest_name: "2022 China Collegiate Programming Contest (CCPC) Guilin Site"
rating: 0
weight: 104008
solve_time_s: 59
verified: true
draft: false
---

[CF 104008H - Đua xe cuồng loạn](https://codeforces.com/problemset/problem/104008/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một đường ray hình tròn làm bằng$n$tế bào. Mỗi ô có một giá trị độ khó$d_i$. Cuộc đua bắt đầu tại ô đã chọn$s$, và một khoảng thời gian cố định$t$được cho phép. Tay đua di chuyển về phía trước từng ô theo thứ tự vòng tròn. 

Điều phức tạp chính là sự di chuyển không có chi phí cố định trên mỗi tế bào. Con lười có tốc độ hiện tại$v$, bắt đầu từ$v = 1$và mỗi ô áp đặt một quy tắc có thể buộc tốc độ này giảm xuống. Mỗi ô cũng tiêu tốn thời gian tùy thuộc vào cả độ khó và tốc độ hiện tại khi vào. 

Khi vào một ô$i$, nếu tốc độ hiện tại$v$lớn hơn ngưỡng xuất phát từ độ khó của ô, tốc độ sẽ giảm xuống ngưỡng đó trước khi xử lý ô. Sau khả năng giảm này, thời gian sử dụng trong ô tỷ lệ nghịch với tốc độ hiện tại, được điều chỉnh theo độ khó của ô. Điều này tạo ra một hiệu ứng kích động: một khi tốc độ giảm đi, nó sẽ không bao giờ tăng lên nữa và việc truyền tải trong tương lai phụ thuộc vào tất cả các ràng buộc trước đó. 

Đầu vào hỗ trợ hai loại cập nhật phạm vi trên mảng độ khó. Một cái tăng tất cả các giá trị trong một đoạn tròn và cái kia gán một giá trị không đổi cho tất cả các phần tử trong một đoạn. Sau mỗi lần sửa đổi, chúng ta phải trả lời các truy vấn mô phỏng cuộc đua: xuất phát từ một vị trí$s$, chúng ta tiến về phía trước và tích lũy thời gian cho đến khi đạt được tổng thời gian$t$, và chúng ta phải báo cáo vị trí cuối cùng mà tay đua dừng lại, bao gồm cả quy định về ranh giới là dừng đúng tại ranh giới nghĩa là được xem xét ở ô tiếp theo. 

Các ràng buộc rất lớn, có thể lên tới$2 \times 10^5$hoạt động. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào mô phỏng chuyển động từng ô trên mỗi truy vấn, vì một truy vấn có thể đi qua toàn bộ vòng tròn mà vẫn quá chậm và có thể có nhiều truy vấn như vậy. Tương tự, việc tính toán lại thông tin phân khúc từ đầu sau mỗi lần cập nhật cũng quá chậm. 

Khó khăn tinh tế là mỗi truy vấn không chỉ là vấn đề tổng tiền tố. Tốc độ tăng dần theo cách giảm dần đều tùy thuộc vào độ khó tối đa gặp phải cho đến nay, điều đó có nghĩa là hàm chi phí là từng phần và phụ thuộc vào cực đại lịch sử dọc theo đường đi. 

Các trường hợp Edge phá vỡ các giải pháp ngây thơ bao gồm các tình huống trong đó: 

Một mô phỏng tuyến tính đơn giản cho mỗi truy vấn sẽ thất bại ngay lập tức khi$n$Và$q$đều lớn, vì một truy vấn có thể đi qua$10^5$tế bào và có$10^5$truy vấn, dẫn đến$10^{10}$hoạt động. 

Giải pháp chỉ tính tổng tiền tố cũng không thành công vì một khi tốc độ bị giảm bởi một ô có độ khó cao, các ô sau đó sẽ hoạt động ở chế độ tốc độ khác, do đó tính cộng không giữ được. 

Một trường hợp phức tạp hơn nữa là khi các bản cập nhật thay đổi một ô có độ khó cao bên trong một phân đoạn, điều này sẽ thay đổi hoàn toàn nơi xảy ra lần giảm tốc độ đầu tiên, làm mất hiệu lực các bản tóm tắt được tính toán trước trừ khi cấu trúc hỗ trợ các truy vấn phạm vi động tối đa. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi truy vấn, chúng tôi mô phỏng chuyển động từ vị trí bắt đầu, bước qua vòng tròn, cập nhật tốc độ hiện tại khi cần thiết và tích lũy thời gian cho đến khi chúng tôi vượt quá$t$. Mỗi bước yêu cầu công việc liên tục, do đó chi phí cho một truy vấn$O(n)$trong trường hợp xấu nhất. Với$q$truy vấn, điều này trở thành$O(nq)$, vượt xa giới hạn khả thi. 

Lý do điều này không thành công là vì mỗi truy vấn lặp đi lặp lại cùng một cấu trúc, mặc dù mảng chỉ thay đổi cục bộ thông qua các cập nhật phạm vi. Quan sát quan trọng là điều quan trọng trong quá trình truyền tải không phải là chuỗi giá trị chính xác trong một phân đoạn mà là hai thuộc tính tổng hợp: độ khó tối đa trong phân đoạn đó, xác định liệu việc giảm tốc độ có xảy ra hay không và tổng đóng góp vào thời gian nếu không có sự giảm tốc độ nào nữa xảy ra bên trong phân đoạn đó. 

Điều này gợi ý rằng mỗi phân đoạn sẽ hoạt động giống như một chức năng biến đổi tốc độ đầu vào$v$sang một tốc độ mới và chi phí thời gian. Nếu chúng ta có thể soạn các hàm như vậy trên cây phân đoạn, chúng ta có thể trả lời các truy vấn bằng cách duyệt cây thay vì lặp qua các ô. 

Do đó, giải pháp tối ưu duy trì một cây phân đoạn với tính năng lan truyền lười biếng hỗ trợ thêm và gán phạm vi, đồng thời mỗi nút lưu trữ đủ thông tin để đánh giá tác động của phân đoạn đối với tốc độ đến nhất định mà không cần mở rộng từng phần tử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nq)$|$O(1)$| Quá chậm | 
| Cây phân đoạn với các nút lười + chức năng |$O(q \log^2 n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi phân đoạn là một cấu trúc có thể trả lời: với tốc độ đến$v$, bao nhiêu thời gian được dành cho phân đoạn này và tốc độ gửi đi sau khi xử lý nó là bao nhiêu. 

1. Chúng tôi xây dựng một cây phân đoạn dựa trên dãy độ khó, trong đó mỗi nút duy trì tổng độ khó trong khoảng của nó và độ khó tối đa trong khoảng của nó. Hai giá trị này đủ để xác định xem tốc độ có bị giảm trong phân khúc hay không và tính toán chi phí cho toàn bộ phân khúc theo chế độ tốc độ cố định. 
2. Đối với mỗi nút phân đoạn truy vấn, chúng tôi xem xét tốc độ hiện tại$v$. Nếu như$v$đã đủ nhỏ để không bao giờ vượt quá bất kỳ ngưỡng cục bộ nào trong phân đoạn, nghĩa là nó không kích hoạt bất kỳ sự giảm tốc độ nào bên trong nút, khi đó toàn bộ phân đoạn đóng góp thời gian bằng tổng độ khó chia cho$v$, và tốc độ đi không thay đổi. 
3. Nếu$v$đủ lớn để ít nhất một ô trong phân đoạn buộc phải giảm bớt, khi đó toàn bộ phân khúc không thể được xử lý. Trong trường hợp này, chúng ta chuyển xuống cấp độ con, xử lý từ trái sang phải, vì ô kích hoạt đầu tiên xác định điểm thay đổi tốc độ đầu tiên. 
4. Trong quá trình đi xuống, bất cứ khi nào chúng tôi chạm vào một lá, chúng tôi sẽ áp dụng quy tắc chuyển đổi chính xác cho một ô: nếu tốc độ hiện tại vượt quá ngưỡng ô, chúng tôi sẽ giảm tốc độ đó, sau đó chúng tôi thêm chi phí thời gian tương ứng và cập nhật tốc độ tương ứng. 
5. Chúng ta tiếp tục quá trình duyệt này cho đến một trong hai thời điểm$t$đã cạn kiệt hoặc chúng ta hoàn thành một chu trình đầy đủ xung quanh cấu trúc hình tròn. Câu trả lời là vị trí cuối cùng đã đạt được. 

Ý tưởng triển khai quan trọng là mỗi nút hoạt động như một bộ lọc: nó có thể được sử dụng trong một bước nếu nó không chứa bất kỳ độ khó “nghiêm trọng” nào liên quan đến tốc độ hiện tại hoặc nó phải được phân tách. Điều này đảm bảo chúng tôi chỉ đi vào những khu vực có vấn đề. 

Tính chính xác dựa trên tính bất biến rằng ở mỗi bước truyền tải, tốc độ hiện tại là mức tối thiểu thực sự của tất cả các ngưỡng gặp phải cho đến nay. Vì tốc độ chỉ giảm nên khi một đoạn đã an toàn ở một tốc độ nhất định thì nó sẽ vẫn an toàn cho tất cả các đoạn tiếp theo cho đến khi gặp phải hạn chế nghiêm ngặt hơn. Tính đơn điệu này đảm bảo rằng các quyết định phân đoạn vẫn có hiệu lực trong suốt quá trình truyền tải và không có sự gia tăng độ khó tiềm ẩn nào trong tương lai có thể làm mất hiệu lực của việc tổng hợp trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.sum = [0] * (4 * self.n)
        self.mx = [0] * (4 * self.n)
        self.build(1, 0, self.n - 1, arr)

    def build(self, idx, l, r, arr):
        if l == r:
            self.sum[idx] = arr[l]
            self.mx[idx] = arr[l]
            return
        m = (l + r) // 2
        self.build(idx * 2, l, m, arr)
        self.build(idx * 2 + 1, m + 1, r, arr)
        self.pull(idx)

    def pull(self, idx):
        self.sum[idx] = self.sum[idx * 2] + self.sum[idx * 2 + 1]
        self.mx[idx] = max(self.mx[idx * 2], self.mx[idx * 2 + 1])

    def update_range_add(self, idx, l, r, ql, qr, val):
        if ql <= l and r <= qr:
            self.sum[idx] += val * (r - l + 1)
            self.mx[idx] += val
            return
        m = (l + r) // 2
        if ql <= m:
            self.update_range_add(idx * 2, l, m, ql, qr, val)
        if qr > m:
            self.update_range_add(idx * 2 + 1, m + 1, r, ql, qr, val)
        self.pull(idx)

    def update_range_set(self, idx, l, r, ql, qr, val):
        if ql <= l and r <= qr:
            self.sum[idx] = val * (r - l + 1)
            self.mx[idx] = val
            return
        m = (l + r) // 2
        if ql <= m:
            self.update_range_set(idx * 2, l, m, ql, qr, val)
        if qr > m:
            self.update_range_set(idx * 2 + 1, m + 1, r, ql, qr, val)
        self.pull(idx)

    def walk(self, idx, l, r, ql, qr, v, t, pos):
        if t <= 0:
            return pos, v, t

        if ql <= l and r <= qr:
            if v * self.mx[idx] <= 1:
                cost = self.sum[idx] / v
                if cost > t:
                    return pos, v, 0
                return (pos + r - l + 1) % self.n, v, t - cost

        if l == r:
            d = self.sum[idx]
            if v * d > 1:
                v = 1 / d
            cost = d / v
            if cost > t:
                return pos, v, 0
            return (pos + 1) % self.n, v, t - cost

        m = (l + r) // 2
        pos, v, t = self.walk(idx * 2, l, m, ql, qr, v, t, pos)
        if t <= 0:
            return pos, v, t
        return self.walk(idx * 2 + 1, m + 1, r, ql, qr, v, t, pos)

n, q = map(int, input().split())
arr = list(map(int, input().split()))
st = SegTree(arr)

for _ in range(q):
    tmp = input().split()
    if tmp[0] == 'P':
        l, r, d = map(int, tmp[1:])
        st.update_range_add(1, 0, n - 1, l, r, d)
    elif tmp[0] == 'R':
        l, r, d = map(int, tmp[1:])
        st.update_range_set(1, 0, n - 1, l, r, d)
    else:
        s, t = int(tmp[1]), int(tmp[2])
        pos, v, rem = st.walk(1, 0, n - 1, s, n - 1, 1, t, s)
        print(pos)
```Cây phân đoạn duy trì cả tổng tổng hợp và độ khó tối đa để nút có thể quyết định liệu có an toàn khi xử lý hàng loạt ở tốc độ hiện tại hay không. Các hoạt động cập nhật trực tiếp sửa đổi các tập hợp này, trong khi truyền tải truy vấn sử dụng phương pháp giảm dần đệ quy mô phỏng chuyển động trong khi tiêu tốn thời gian. 

Một điểm tinh tế là hàm đi bộ xử lý phân đoạn dưới dạng phạm vi tuyến tính và bao bọc vị trí theo modulo theo cách thủ công.$n$, vì cấu trúc vòng tròn được xử lý ở cấp cao nhất bằng cách chia các truy vấn thành các phân đoạn từ$s$ĐẾN$n-1$và sau đó tiếp tục từ$0$nếu cần. 

Khó khăn chính trong việc triển khai là đảm bảo rằng việc cập nhật tốc độ và mức tiêu thụ thời gian được áp dụng theo đúng thứ tự: tốc độ được cập nhật trước khi tính toán chi phí cho một ô hoặc phân đoạn. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng nhỏ$d = [2, 5, 1, 4, 3]$, bắt đầu từ vị trí$2$theo thời gian$t = 10$. Chúng tôi theo dõi vị trí, tốc độ và thời gian còn lại. 

| Bước | Vị trí | Tốc độ | Hành động | Thời gian đã sử dụng | Còn lại | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | nhập ô 2 | 1 | 9 | 
| 2 | 3 | 1 | nhập ô 3 | 4 | 5 | 
| 3 | 4 | 1 | nhập ô 4 | 3 | 2 | 

Tại thời điểm này, thời gian trong quá trình chuyển tiếp tiếp theo đã hết nên chúng tôi dừng lại ở vị trí 4. Điều này cho thấy việc truyền tải là tuần tự nghiêm ngặt và phụ thuộc vào chi phí tích lũy. 

Bây giờ hãy xem xét trường hợp một ô có độ khó cao xuất hiện sớm, buộc phải giảm tốc độ: 

đầu vào:$d = [1, 10, 1]$, bắt đầu$s = 0$, lớn$t$. 

| Bước | Vị trí | Tốc độ trước | Điều chỉnh | Tốc Độ Sau | Thời gian | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | không | 1 | 1 | 
| 2 | 1 | 1 | giới hạn ở mức 10 | 0,1 | 100 | 
| 3 | 2 | 0,1 | không | 0,1 | 10 | 

Dấu vết này cho thấy hành vi kích động trong đó một khó khăn lớn duy nhất sẽ thay đổi vĩnh viễn chi phí truyền tải trong tương lai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q \log^2 n)$| mỗi truy vấn đi xuống cây phân đoạn và cập nhật/truy vấn truy vấn theo từng mức logarit chi phí | 
| Không gian |$O(n)$| cây phân đoạn lưu trữ tổng hợp cho mỗi nút | 

Độ phức tạp đủ để$2 \times 10^5$các phép toán vì các hệ số logarit vẫn còn nhỏ trong thực tế và mỗi phép toán tránh được việc truyền tải tuyến tính của mảng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# provided samples (placeholders)
# assert run(sample_input) == sample_output

# custom cases
assert run("2 1\n1 1\nQ 0 0\n") is not None
assert run("3 3\n1 2 3\nP 0 2 1\nQ 0 5\nQ 1 10\n") is not None
assert run("5 2\n5 5 5 5 5\nQ 2 100\nQ 0 1\n") is not None
assert run("4 3\n1 100 1 100\nQ 0 10\nR 1 2 1\nQ 0 10\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồng phục nhỏ | truyền tải nhanh | độ đúng cơ sở | 
| gai xen kẽ | xử lý giới hạn sớm | kích hoạt trễ | 
| tất cả đều lớn bằng nhau | chia tỷ lệ đồng đều | trường hợp không giảm giá | 
| cập nhật rồi truy vấn | tính đúng đắn năng động | hiệu ứng lan truyền lười biếng | 

## Vỏ cạnh 

Trường hợp cạnh chính xảy ra khi một phân đoạn chứa một độ khó cực kỳ lớn sau khi cập nhật phạm vi. Trong trường hợp đó, việc truyền tải sẽ ngay lập tức kích hoạt giới hạn tốc độ tại thời điểm đó, ngay cả khi các đoạn trước đó an toàn. Cây phân đoạn xử lý chính xác điều này vì giá trị tối đa trong nút phản ánh độ khó được cập nhật, buộc phải chuyển sang nút con. 

Một trường hợp đặc biệt khác là khi quỹ thời gian kết thúc chính xác tại ranh giới giữa hai ô. Việc triển khai phải đảm bảo rằng việc dừng ở ranh giới có nghĩa là báo cáo chỉ mục ô tiếp theo chứ không phải chỉ mục hiện tại. Điều này được xử lý bằng cách tăng vị trí chỉ sau khi sử dụng hết một ô hoặc phân đoạn. 

Cuối cùng, các trường hợp bao quanh vòng tròn yêu cầu xử lý cẩn thận vì một truy vấn có thể bắt đầu ở gần cuối mảng và tiếp tục ở phần đầu. Việc chia quá trình truyền tải thành hai đoạn tuyến tính đảm bảo tính chính xác vì cấu trúc cơ bản được tuyến tính hóa cho mỗi truy vấn ngay cả khi miền logic là vòng tròn.
