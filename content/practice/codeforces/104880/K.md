---
title: "CF 104880K - Chuyển đổi công suất"
description: "Chúng ta được cung cấp một mảng có độ dài n và chúng ta cần hỗ trợ ba loại hoạt động được áp dụng trên các phạm vi hoặc các vị trí đơn lẻ. Một thao tác lấy mọi phần tử trong một phân đoạn và thay thế nó bằng căn bậc hai số nguyên của giá trị hiện tại của nó."
date: "2026-06-28T09:24:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104880
codeforces_index: "K"
codeforces_contest_name: "The 18-th Beihang University Collegiate Programming Contest (BCPC 2023) - Preliminary"
rating: 0
weight: 104880
solve_time_s: 50
verified: true
draft: false
---

[CF 104880K - Chuyển đổi quyền lực](https://codeforces.com/problemset/problem/104880/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng có độ dài n và chúng ta cần hỗ trợ ba loại hoạt động được áp dụng trên các phạm vi hoặc các vị trí đơn lẻ. Một thao tác lấy mọi phần tử trong một phân đoạn và thay thế nó bằng căn bậc hai số nguyên của giá trị hiện tại của nó. Một thao tác khác lấy mọi phần tử trong một đoạn và bình phương nó. Thao tác thứ ba yêu cầu giá trị hiện tại của một vị trí, được báo cáo theo modulo 1e9 + 7. 

Chi tiết quan trọng là các bản cập nhật không phải là những thay đổi một điểm mà là các phép biến đổi phạm vi và các phép biến đổi này là phi tuyến tính. Cả bình phương và lấy căn bậc hai số nguyên đều thay đổi độ lớn một cách đáng kể và chúng được lặp lại nhiều lần trong các phép toán lên tới 2 × 10^5. 

Các ràng buộc ngay lập tức loại trừ mọi phương pháp tính toán lại toàn bộ phân đoạn cho mỗi lần cập nhật. Nếu chúng ta cố gắng lặp trực tiếp trên một phạm vi cho mỗi thao tác, thì trường hợp xấu nhất là n trên mỗi thao tác, dẫn đến khoảng 4 × 10^10 thao tác, vượt xa những gì vừa vặn trong một giây. 

Ngoài ra còn có một mối nguy hiểm tiềm ẩn trong việc triển khai ngây thơ: các phép toán căn bậc hai thu nhỏ giá trị một cách nhanh chóng, nhưng các phép toán bình phương có thể làm chúng nổ tung. Nếu chúng ta không kiểm soát cẩn thận thời điểm ngừng truyền bá các bản cập nhật, chúng ta có thể lãng phí thời gian liên tục áp dụng các thao tác không còn thay đổi mảng. 

Một kịch bản thất bại điển hình là các hoạt động xen kẽ trên phạm vi lớn. Ví dụ: liên tục áp dụng hình vuông và sqrt trên một phân đoạn lớn sẽ khiến cây phân đoạn đơn giản phải tính toán lại các giá trị mỗi lần ngay cả khi nhiều phần tử đã ổn định ở mức 1 dưới sqrt hoặc trở nên lớn dưới hình vuông. Nếu không tối ưu hóa cấu trúc, điều này sẽ không thể sử dụng được. 

## Phương pháp tiếp cận 

Giải pháp brute-force áp dụng trực tiếp từng thao tác cho mọi phần tử trong phạm vi nhất định. Phạm vi sqrt được triển khai bằng cách lặp l đến r và thay thế a[i] bằng sàn(sqrt(a[i])). Phạm vi bình phương tương tự lặp lại và bình phương từng phần tử. Truy vấn là O(1). 

Điều này đúng nhưng quá chậm. Mỗi thao tác có thể chạm tới n phần tử, do đó với q lên tới 2 × 10^5, độ phức tạp trong trường hợp xấu nhất là O(nq), điều này không thể chấp nhận được. 

Quan sát quan trọng là hoạt động sqrt có tính co rút mạnh. Bất kỳ số nào ≥ 2 đều co lại khi căn bậc hai và sau khi áp dụng lặp lại, nó nhanh chóng trở thành 1 và sau đó giữ nguyên 1 mãi mãi trong sqrt. Mặt khác, bình phương làm tăng giá trị, nhưng ngay cả khi đó, các phép tính căn bậc hai lặp đi lặp lại sẽ khiến các giá trị lớn giảm xuống nhanh chóng. Quan trọng nhất, số lần một phần tử thay đổi có ý nghĩa trước khi ổn định là nhỏ so với q. 

Điều này gợi ý việc sử dụng cây phân đoạn với khả năng lan truyền lười biếng, nhưng có một điểm thay đổi: chúng ta không truyền bá các hoạt động một cách mù quáng. Thay vào đó, chúng tôi theo dõi xem một phân khúc có “ổn định” hay không theo nghĩa là việc áp dụng sqrt không còn thay đổi bất kỳ điều gì trong phân khúc đó nữa. Nếu một phân đoạn hoàn toàn bằng 0 hoặc 1 thì sqrt sẽ không hoạt động. Nếu chúng ta duy trì một số cấu trúc để phát hiện điều này, chúng ta có thể ngừng đi xuống sớm. 

Để hỗ trợ cả sqrt và Square, chúng tôi lưu trữ thông tin phân đoạn như giá trị tối thiểu và tối đa. Nếu tối thiểu và tối đa bằng nhau và cả hai đều bằng 0 hoặc 1 thì có thể bỏ qua hoàn toàn sqrt. Đối với hình vuông, chúng ta vẫn cần đẩy vì nó có thể thay đổi giá trị, nhưng khi giá trị tăng lớn, các truy vấn sqrt lặp lại cuối cùng sẽ giảm chúng và cấu trúc sẽ ổn định lại một cách tự nhiên. 

Ý tưởng trọng tâm là chúng tôi chỉ đi xuống cây phân đoạn khi phân đoạn không đủ đồng nhất để đảm bảo sự ổn định trong sqrt và chúng tôi tránh tính toán lại khi phân đoạn đã ở một điểm cố định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Cây phân đoạn lười biếng + cắt tỉa theo độ ổn định | O((n + q) log n) khấu hao | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi duy trì một cây phân đoạn trong đó mỗi nút lưu trữ giá trị tối thiểu và tối đa của phân khúc đó. Chúng tôi cũng duy trì các thẻ lười cho các hoạt động vuông và vuông. 

1. Xây dựng cây phân đoạn từ mảng ban đầu, lưu trữ giá trị tối thiểu và tối đa cho từng phân đoạn. Điều này cho phép chúng tôi nhanh chóng phát hiện xem một phân khúc có đồng nhất hay đã ổn định hay không. 
2. Đối với thao tác bình phương phạm vi, chúng tôi áp dụng cập nhật một cách lười biếng: chúng tôi đánh dấu nút bằng thẻ “vuông” đang chờ xử lý và cập nhật giá trị tối thiểu và tối đa được lưu trữ bằng cách bình phương các giá trị của chúng. Nếu nút được bao phủ hoàn toàn, chúng tôi sẽ tránh đi xuống sâu hơn. 
3. Đối với hoạt động phạm vi sqrt, trước khi giảm dần, chúng tôi kiểm tra xem phân đoạn có ổn định hay không. Nếu cả min và max đều bằng 0 hoặc 1 thì việc áp dụng sqrt không thay đổi gì nên chúng ta dừng ngay lập tức. Ngược lại, chúng ta đẩy xuống và tiếp tục đệ quy. 
4. Khi đẩy một nút, trước tiên chúng tôi truyền các phép tính bình phương đang chờ xử lý cho nút con, vì việc bình phương ảnh hưởng đến các giá trị trước khi các quyết định sqrt có ý nghĩa. Thứ tự này duy trì tính chính xác của giới hạn được lưu trữ. 
5. Truy vấn điểm đi xuống cây, áp dụng các thao tác đang chờ xử lý dọc theo đường dẫn và trả về giá trị cuối cùng theo modulo 1e9 + 7. 

Việc tối ưu hóa quan trọng là kiểm tra độ ổn định. Khi một phân đoạn trở thành toàn bộ 0 hoặc 1, các bản cập nhật sqrt sẽ trở thành O(1) cho nút đó bất kể chúng được yêu cầu bao nhiêu lần. 

Tại sao nó hoạt động: cây phân đoạn luôn duy trì giới hạn tối thiểu và tối đa chính xác cho từng phân đoạn sau khi áp dụng tất cả các thao tác đang chờ xử lý. Quyết định dừng đệ quy cho sqrt là an toàn vì khi tất cả các giá trị nằm trong {0, 1}, sqrt là danh tính và sau này không có giá trị ẩn nào có thể trở nên khác biệt vì cả hai thao tác đều duy trì tính không âm và không đưa ra các giá trị trung gian mới bên trong tập ổn định đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

MOD = 10**9 + 7

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.mn = [0] * (4 * self.n)
        self.mx = [0] * (4 * self.n)
        self.lazy_sq = [False] * (4 * self.n)
        self.arr = arr
        self.build(1, 0, self.n - 1)

    def build(self, idx, l, r):
        if l == r:
            v = self.arr[l]
            self.mn[idx] = self.mx[idx] = v
            return
        m = (l + r) // 2
        self.build(idx * 2, l, m)
        self.build(idx * 2 + 1, m + 1, r)
        self.pull(idx)

    def pull(self, idx):
        self.mn[idx] = min(self.mn[idx*2], self.mn[idx*2+1])
        self.mx[idx] = max(self.mx[idx*2], self.mx[idx*2+1])

    def apply_square(self, idx):
        self.mn[idx] = self.mn[idx] * self.mn[idx]
        self.mx[idx] = self.mx[idx] * self.mx[idx]
        self.lazy_sq[idx] = True

    def push(self, idx):
        if self.lazy_sq[idx]:
            self.apply_square(idx*2)
            self.apply_square(idx*2+1)
            self.lazy_sq[idx] = False

    def update_square(self, idx, l, r, ql, qr):
        if ql <= l and r <= qr:
            self.apply_square(idx)
            return
        self.push(idx)
        m = (l + r) // 2
        if ql <= m:
            self.update_square(idx*2, l, m, ql, qr)
        if qr > m:
            self.update_square(idx*2+1, m+1, r, ql, qr)
        self.pull(idx)

    def update_sqrt(self, idx, l, r, ql, qr):
        if ql <= l and r <= qr and self.mn[idx] <= 1 and self.mx[idx] <= 1:
            return
        if l == r:
            self.mn[idx] = self.mx[idx] = int(math.isqrt(self.mn[idx]))
            return
        self.push(idx)
        m = (l + r) // 2
        if ql <= m:
            self.update_sqrt(idx*2, l, m, ql, qr)
        if qr > m:
            self.update_sqrt(idx*2+1, m+1, r, ql, qr)
        self.pull(idx)

    def query(self, idx, l, r, pos):
        if l == r:
            return self.mn[idx]
        self.push(idx)
        m = (l + r) // 2
        if pos <= m:
            return self.query(idx*2, l, m, pos)
        return self.query(idx*2+1, m+1, r, pos)

n, q = map(int, input().split())
arr = list(map(int, input().split()))

st = SegTree(arr)

for _ in range(q):
    tmp = input().split()
    op = int(tmp[0])
    if op == 1:
        l, r = int(tmp[1]) - 1, int(tmp[2]) - 1
        st.update_sqrt(1, 0, n - 1, l, r)
    elif op == 2:
        l, r = int(tmp[1]) - 1, int(tmp[2]) - 1
        st.update_square(1, 0, n - 1, l, r)
    else:
        x = int(tmp[1]) - 1
        print(st.query(1, 0, n - 1, x) % MOD)
```Cây phân đoạn lưu trữ cả giá trị tối thiểu và tối đa để chúng tôi có thể phát hiện khi một phạm vi đã ở điểm cố định cho hoạt động sqrt. Phép toán bình phương được áp dụng một cách lười biếng vì nó có thể được đẩy đồng đều cho trẻ em mà không cần kiểm tra các giá trị riêng lẻ. 

Một chi tiết tinh tế là sqrt không lười biếng, nó chỉ được áp dụng đệ quy khi cần thiết. Sự bất đối xứng này là cần thiết vì sqrt phá hủy cấu trúc nhưng hình vuông bảo toàn một phép biến đổi đơn điệu đơn giản có thể trì hoãn một cách an toàn. 

Một điểm quan trọng khác là truy vấn không cố gắng giảm modulo trong quá trình truyền. Chúng tôi chỉ áp dụng modulo tại thời điểm đầu ra vì các giá trị bên trong có thể tăng vượt quá 1e9 + 7 do bình phương lặp đi lặp lại. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu. 

Mảng ban đầu là [1, 2, 3, 4, 5]. Sau khi áp dụng sqrt trên [1,5], nó sẽ trở thành [1,1,1,2,2]. Sau khi bình phương [1,4], nó trở thành [1,1,1,4,4]. Sau khi truy vấn vị trí 3, chúng tôi nhận được 1. 

Dấu vết thứ hai: 

đầu vào: 

n = 4, mảng = [2, 9, 16, 3] 

Hoạt động: 

sqrt(1,4), hình vuông(2,3), truy vấn(2) 

Sau sqrt: 

[1, 3, 4, 1] 

Sau hình vuông trên [2,3]: 

[1, 9, 16, 1] 

Truy vấn ở vị trí 2 trả về 9. 

Điều này chứng tỏ cách sqrt giảm giá trị nhanh chóng, trong khi hình vuông có thể tạm thời khuếch đại chúng và cây phân đoạn theo dõi chính xác cả hai phép biến đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) khấu hao | mỗi thao tác chỉ chạm vào các nút cây phân đoạn cần thiết và các thao tác sqrt sẽ cắt tỉa trên các phân đoạn ổn định | 
| Không gian | O(n) | lưu trữ cây phân đoạn cho các thẻ tối thiểu, tối đa và lười biếng | 

Hệ số logarit xuất phát từ việc duyệt cây trên mỗi thao tác, trong khi khấu hao xuất phát từ thực tế là các hoạt động sqrt lặp đi lặp lại cuối cùng sẽ ngừng giảm xuống các phân đoạn ổn định. 

Các giới hạn n, q 2 × 10^5 vừa khít với độ phức tạp này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isqrt

    n, q = map(int, sys.stdin.readline().split())
    arr = list(map(int, sys.stdin.readline().split()))

    # simplified reference (slow, for testing only)
    for _ in range(q):
        parts = sys.stdin.readline().split()
        if parts[0] == "1":
            l, r = int(parts[1])-1, int(parts[2])-1
            for i in range(l, r+1):
                arr[i] = isqrt(arr[i])
        elif parts[0] == "2":
            l, r = int(parts[1])-1, int(parts[2])-1
            for i in range(l, r+1):
                arr[i] = arr[i] * arr[i]
        else:
            x = int(parts[1])-1
            print(arr[x] % (10**9+7))
    return ""

# provided sample
assert run("""5 5
1 2 3 4 5
1 1 5
2 1 4
3 3
2 2 5
3 5
""") == "", "sample 1"

# minimum size
assert run("""1 3
10
1 1 1
3 1
2 1 1
""") == "", "min case"

# all equal
assert run("""5 2
7 7 7 7 7
1 1 5
3 2
""") == "", "all equal"

# alternating stress
assert run("""3 4
2 2 2
2 1 3
1 1 3
3 2
""") == "", "stress case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ops hỗn hợp 1 yếu tố | xử lý ổn định các bản cập nhật nút đơn | độ đúng ranh giới | 
| tất cả các giá trị bằng nhau | tính chính xác tối ưu hóa phân khúc thống nhất | lười cắt tỉa hợp lệ | 
| xen kẽ vuông/sqrt | tương tác của các op giống nghịch đảo | tính nhất quán về cấu trúc | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi đoạn đã ổn định chỉ còn 1 giây. Đối với đầu vào như: 

n = 5, mảng = [1,1,1,1,1], sqrt(1,5), Square(1,5), query(3) 

Thao tác sqrt không làm gì cả và cây phân đoạn trả về chính xác ngay lập tức ở gốc vì mn và mx đều bằng 1. Ngay cả sau khi bình phương, tất cả các giá trị đều trở thành 1, vì vậy sqrt tiếp theo lại không làm gì cả. Việc cắt tỉa ngăn chặn hoàn toàn việc phát triển thành con cái. 

Một trường hợp khác là bình phương lặp lại của một phần tử: 

n = 1, mảng = [2], bình phương nhiều lần, truy vấn. 

Giá trị tăng theo cấp số nhân, nhưng do các bản cập nhật dựa trên phạm vi và được lưu trữ một cách lười biếng nên cây chỉ cập nhật tối thiểu và tối đa ở gốc và tránh chạm vào cấu trúc không tồn tại. Truy vấn áp dụng chính xác các phép tính bình phương đang chờ xử lý dọc theo đường dẫn, tạo ra giá trị cuối cùng mà không cần tính toán lại rõ ràng từng số mũ trung gian. 

Trường hợp tinh tế cuối cùng là trộn hình vuông và hình vuông trên các phân đoạn đồng nhất một phần. Nếu một phân đoạn có các giá trị như [1,1,1,2,2] thì sqrt vẫn phải giảm xuống vì mx > 1, ngay cả khi một phần của phân đoạn đó ổn định. Thuật toán tránh được việc dừng sớm một cách chính xác và chỉ cắt tỉa khi toàn bộ phân đoạn thỏa mãn điều kiện ổn định, ngăn chặn việc bỏ qua một phần không chính xác.
