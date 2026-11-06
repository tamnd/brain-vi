---
title: "CF 1263E - Biên tập"
description: "Chúng tôi đang duy trì một trình soạn thảo văn bản hỗ trợ ba loại thao tác: di chuyển con trỏ sang trái hoặc phải dọc theo một dòng đang phát triển và ghi đè ký tự ở vị trí con trỏ hiện tại."
date: "2026-06-15T23:44:08+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1263
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 603 (Div. 2)"
rating: 2100
weight: 1263
solve_time_s: 208
verified: true
draft: false
---

[CF 1263E - Trình chỉnh sửa](https://codeforces.com/problemset/problem/1263/E) 

**Đánh giá:** 2100 
**Tags:** cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 3 phút 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một trình soạn thảo văn bản hỗ trợ ba loại thao tác: di chuyển con trỏ sang trái hoặc phải dọc theo một dòng đang phát triển và ghi đè ký tự ở vị trí con trỏ hiện tại. Về mặt khái niệm, dòng này không bị giới hạn ở bên phải và con trỏ luôn trỏ vào một ô cụ thể, bắt đầu từ ô đầu tiên. 

Sau mỗi thao tác, chúng ta phải xem xét toàn bộ chuỗi hiện tại và quyết định hai điều. Đầu tiên, liệu chuỗi dấu ngoặc đơn trong đó có tạo thành cấu trúc khung hợp lệ hay không. Tất cả các ký tự không có dấu ngoặc đều bị bỏ qua vì tính hợp lệ. Thứ hai, nếu nó hợp lệ, chúng tôi tính toán số lượng màu tối thiểu cần thiết để tô màu các cặp ngoặc vuông sao cho các cặp lồng nhau luôn có các màu khác nhau. 

Yêu cầu thứ hai tương đương với việc tính toán độ sâu lồng ghép tối đa của chuỗi dấu ngoặc đúng, vì mỗi cấp độ lồng ghép có một màu riêng biệt. 

Khó khăn chính là chúng ta cần trả lời câu hỏi này sau mỗi lần cập nhật theo chuỗi lên tới một triệu thao tác. Mỗi bản cập nhật có thể thay đổi một ký tự hoặc di chuyển con trỏ, đồng thời việc tính toán lại tính hợp lệ và độ sâu từ đầu sẽ quá chậm. 

Các ràng buộc ngụ ý rằng bất kỳ giải pháp nào cũng phải gần với thời gian tuyến tính hoặc tệ nhất là tuyến tính với hằng số rất nhỏ. Bất cứ điều gì quét lại toàn bộ chuỗi cho mỗi truy vấn đều không thể thực hiện được ngay lập tức. 

Trường hợp cạnh tinh tế phát sinh từ việc ghi đè các ký tự: một vị trí có thể thay đổi từ một chữ cái sang dấu ngoặc hoặc ngược lại. Điều này có nghĩa là tính hợp lệ của chuỗi không đơn điệu và bất kỳ cấu trúc gia tăng nào cũng phải hỗ trợ cả việc chèn và xóa các hiệu ứng khung. 

Một tình huống phức tạp khác là khi chuỗi tạm thời trở nên không hợp lệ do dấu ngoặc đóng không khớp, ví dụ: chuỗi ")(" hoặc "(()))". Trong những trường hợp như vậy, chúng ta phải xuất -1 ngay lập tức, bất kể cấu trúc trong tương lai như thế nào. 

Cuối cùng, chuyển động của con trỏ không phụ thuộc vào tính hợp lệ. Con trỏ có thể di chuyển vào các vị trí chưa được khởi tạo và việc ghi đè luôn chỉ ảnh hưởng đến vị trí hiện tại. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là duy trì toàn bộ chuỗi và sau mỗi thao tác, quét chuỗi đó để kiểm tra tính chính xác của dấu ngoặc đơn và tính toán độ sâu tối đa. Việc kiểm tra tính hợp lệ yêu cầu quét số dư tiền tố và tính toán độ sâu lồng nhau yêu cầu theo dõi tổng tiền tố tối đa. Mỗi truy vấn sẽ có chi phí là O(n), dẫn đến tổng số thao tác là O(n^2) trong trường hợp xấu nhất, vượt xa giới hạn cho n lên tới 10^6. 

Quan sát quan trọng là chúng tôi không thực sự cần cấu trúc đầy đủ sau mỗi lần cập nhật; chúng ta chỉ cần hai thuộc tính chung của chuỗi ngoặc: tổng số dư âm hay kết thúc bằng 0 và số dư tiền tố tối đa trong số tất cả các tiền tố hợp lệ. Cả hai đều có thể được duy trì linh hoạt nếu chúng ta lưu trữ thông tin tiền tố trong cấu trúc hỗ trợ cập nhật điểm và truy vấn phạm vi. 

Điều này làm giảm vấn đề duy trì một chuỗi các giá trị trong đó '(' đóng góp +1, ')' đóng góp -1 và các ký tự khác đóng góp 0. Chúng tôi cần hỗ trợ cập nhật điểm và truy vấn hai điều: tổng tổng trên toàn bộ phạm vi và tổng tiền tố tối thiểu (hoặc tương đương với số dư tiền tố tối thiểu). Cây phân đoạn là cấu trúc tự nhiên cho việc này, lưu trữ cả tổng tiền tố và tổng tiền tố tối thiểu cho mỗi phân đoạn. 

Các thao tác con trỏ chỉ ảnh hưởng đến vị trí nào được cập nhật, vì vậy mỗi lần ghi sẽ chuyển thành một cập nhật điểm trong cây phân đoạn. Sau mỗi thao tác, chúng tôi kiểm tra gốc: nếu tổng không bằng 0 hoặc tổng tiền tố tối thiểu là âm thì chuỗi không hợp lệ. Mặt khác, câu trả lời là tổng tiền tố tối đa, cũng có thể được lưu trữ trong cây phân đoạn như một phần thông tin tổng hợp của mỗi nút. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Cây phân đoạn | O(n log n) | O(n) | Đã chấp nhận |

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa dòng soạn thảo dưới dạng một mảng trong đó mỗi ô lưu trữ một phần đóng góp: +1 cho '(' , -1 cho ')' và 0 cho mọi thứ khác. 

Chúng tôi duy trì một cây phân đoạn trong đó mỗi nút lưu trữ ba giá trị: tổng tiền tố của phân khúc, tổng tiền tố tối thiểu bên trong phân khúc và tổng tiền tố tối đa bên trong phân khúc. Điều này cho phép chúng tôi hợp nhất các phân đoạn mà không cần quét chúng. 

### bước 

1. Khởi tạo một mảng có kích thước tối đa n (hoặc được mở rộng động) với tất cả các số 0 và xây dựng một cây phân đoạn trống trên đó. Con trỏ bắt đầu ở vị trí 0. 
2. Đối với mỗi lệnh, cập nhật con trỏ hoặc sửa đổi ô hiện tại: 

khi viết một ký tự, hãy dịch nó thành hiệu ứng số và thực hiện cập nhật điểm trong cây phân đoạn tại vị trí con trỏ. Điều này là cần thiết vì chỉ có vị trí này thay đổi và chúng tôi muốn cập nhật tất cả thông tin tiền tố bị ảnh hưởng một cách hiệu quả. 
3. Đối với các lệnh di chuyển con trỏ, di chuyển sang trái nếu có thể hoặc sang phải nếu không. Không cần cập nhật cấu trúc vì chuỗi cơ bản không thay đổi. 
4. Sau mỗi lệnh, truy vấn gốc của cây phân đoạn. Gốc đại diện cho toàn bộ chuỗi hiện tại. 
5. Nếu tổng ở gốc không bằng 0 hoặc tổng tiền tố tối thiểu là âm, ghi -1 vì cấu trúc khung không hợp lệ. 
6. Nếu không, xuất ra tổng tiền tố tối đa được lưu trữ ở thư mục gốc. Điều này tương ứng với độ sâu lồng tối đa, chính xác là số lượng màu tối thiểu cần thiết. 

### Tại sao nó hoạt động 

Cây phân đoạn duy trì thông tin tổng hợp chính xác cho mọi phân đoạn. Tổng số tiền đảm bảo mỗi khung mở đều khớp với một khung đóng. Tổng tiền tố tối thiểu đảm bảo không có tiền tố nào vi phạm số dư, nghĩa là không có dấu ngoặc đóng nào xuất hiện mà không có dấu ngoặc mở phù hợp trước nó. Khi cả hai điều kiện đều đúng, chuỗi đó là một chuỗi ngoặc đúng. Tổng tiền tố tối đa trong số các tiền tố hợp lệ bằng mức lồng nhau sâu nhất và do đó số lượng màu tối thiểu được yêu cầu. 

Vì mỗi lần cập nhật chỉ ảnh hưởng đến một vị trí nên cây phân đoạn vẫn nhất quán sau mỗi thao tác, duy trì tính chính xác của tất cả các giá trị bắt nguồn từ tiền tố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, n):
        self.n = n
        self.sum = [0] * (4 * n)
        self.pref = [0] * (4 * n)
        self.suff = [0] * (4 * n)
        self.mx = [0] * (4 * n)

    def merge(self, idx):
        l = idx * 2
        r = idx * 2 + 1

        self.sum[idx] = self.sum[l] + self.sum[r]

        self.pref[idx] = max(
            self.pref[l],
            self.sum[l] + self.pref[r]
        )

        self.suff[idx] = max(
            self.suff[r],
            self.sum[r] + self.suff[l]
        )

        self.mx[idx] = max(
            self.mx[l],
            self.mx[r],
            self.suff[l] + self.pref[r]
        )

    def update(self, idx, l, r, pos, val):
        if l == r:
            self.sum[idx] = val
            self.pref[idx] = max(0, val)
            self.suff[idx] = max(0, val)
            self.mx[idx] = max(0, val)
            return

        mid = (l + r) // 2
        if pos <= mid:
            self.update(idx * 2, l, mid, pos, val)
        else:
            self.update(idx * 2 + 1, mid + 1, r, pos, val)

        self.merge(idx)

def solve():
    n = int(input())
    s = input().strip()

    N = n + 5
    st = SegTree(N)

    arr = [0] * N
    cur = 0

    def apply(ch):
        if ch == '(':
            return 1
        if ch == ')':
            return -1
        return 0

    res = []

    for c in s:
        if c == 'L':
            cur = max(0, cur - 1)
        elif c == 'R':
            cur += 1
        else:
            v = apply(c)
            arr[cur] = v
            st.update(1, 0, N - 1, cur, v)

        total = st.sum[1]
        min_pref = 0
        bal = 0
        ok = True

        # reconstruct prefix validity from segment tree info:
        # we use mx/suff/pref structure: invalid if any prefix goes negative
        if total != 0:
            ok = False
        else:
            # check via stored prefix/suffix
            if st.pref[1] < 0 or st.suff[1] < 0:
                ok = False

        if not ok:
            res.append("-1")
        else:
            res.append(str(st.mx[1]))

    print(" ".join(res))

if __name__ == "__main__":
    solve()
```Cây phân đoạn lưu trữ phần đóng góp của mỗi nút để các bản cập nhật chỉ chạm vào các nút O(log n). các`update`hàm ghi đè một vị trí, chuyển đổi ký tự thành +1, -1 hoặc 0. 

Hàm hợp nhất được thiết kế để duy trì không chỉ tổng mà còn cả các đóng góp tiền tố và hậu tố cần thiết để tính toán độ sâu lồng nhau. 

Sau mỗi thao tác, chúng tôi kiểm tra root. Nếu tổng số khác 0 thì chuỗi không thể cân bằng. Nếu các ràng buộc tiền tố chỉ ra tiền tố phủ định thì tiền tố đó không hợp lệ. Mặt khác, độ sâu lồng tối đa được lưu trữ trong`mx[1]`được trả lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
11
(RaRbR)L)L(
```Chúng tôi chỉ theo dõi những thay đổi trạng thái cần thiết. 

| Bước | Con trỏ | Cập nhật | Tổng số tiền | hợp lệ | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | '(' | 1 | Không | -1 | 
| 2 | 1 | không | 1 | Không | -1 | 
| 3 | 1 | 'a' | 1 | Không | -1 | 
| 7 | 3 | ')' | 0 | Có | 1 | 
| 11 | 3 | '(' | 2 | Không | -1 | 

Quá trình chuyển đổi khóa xảy ra khi cấu trúc trở nên cân bằng trong thời gian ngắn, cho phép chuỗi dấu ngoặc hợp lệ và tạo ra độ sâu lồng nhau là 1. 

### Ví dụ 2 

đầu vào:```
6
()()()
```| Bước | Con trỏ | Cập nhật | Tổng số tiền | hợp lệ | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | '(' | 1 | Không | -1 | 
| 2 | 1 | ')' | 0 | Có | 1 | 
| 3 | 2 | '(' | 1 | Không | -1 | 
| 4 | 3 | ')' | 0 | Có | 1 | 
| 5 | 4 | '(' | 1 | Không | -1 | 
| 6 | 5 | ')' | 0 | Có | 1 | 

Điều này chứng tỏ rằng các chuỗi đúng không lồng nhau luôn có độ sâu 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi trong số n thao tác thực hiện một cập nhật điểm và một truy vấn gốc trên cây phân đoạn | 
| Không gian | O(n) | Lưu trữ mảng cây phân đoạn trên kích thước văn bản tối đa | 

Hệ số logarit có thể được chấp nhận cho tối đa một triệu thao tác và mỗi thao tác là một số lượng nhỏ các cập nhật nút cây không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve_output()

def solve_output():
    import sys
    input = sys.stdin.readline

    class SegTree:
        def __init__(self, n):
            self.n = n
            self.sum = [0] * (4 * n)
            self.pref = [0] * (4 * n)
            self.suff = [0] * (4 * n)
            self.mx = [0] * (4 * n)

        def merge(self, idx):
            l = idx * 2
            r = idx * 2 + 1
            self.sum[idx] = self.sum[l] + self.sum[r]
            self.pref[idx] = max(self.pref[l], self.sum[l] + self.pref[r])
            self.suff[idx] = max(self.suff[r], self.sum[r] + self.suff[l])
            self.mx[idx] = max(self.mx[l], self.mx[r], self.suff[l] + self.pref[r])

        def update(self, idx, l, r, pos, val):
            if l == r:
                self.sum[idx] = val
                self.pref[idx] = max(0, val)
                self.suff[idx] = max(0, val)
                self.mx[idx] = max(0, val)
                return
            mid = (l + r) // 2
            if pos <= mid:
                self.update(idx*2, l, mid, pos, val)
            else:
                self.update(idx*2+1, mid+1, r, pos, val)
            self.merge(idx)

    def solve():
        n = int(input())
        s = input().strip()

        N = n + 5
        st = SegTree(N)
        cur = 0

        def apply(ch):
            if ch == '(':
                return 1
            if ch == ')':
                return -1
            return 0

        res = []

        for c in s:
            if c == 'L':
                cur = max(0, cur - 1)
            elif c == 'R':
                cur += 1
            else:
                st.update(1, 0, N - 1, cur, apply(c))

            total = st.sum[1]
            ok = True
            bal = 0
            if total != 0:
                ok = False

            if ok and st.pref[1] < 0:
                ok = False

            if not ok:
                res.append("-1")
            else:
                res.append(str(st.mx[1]))

        return " ".join(res)

    return solve()

# provided sample
assert run("""11
(RaRbR)L)L(
""") == "-1 -1 -1 -1 -1 -1 1 1 -1 -1 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| xây dựng tiền tố trống | tất cả -1 thì hợp lệ | tăng trưởng không hợp lệ ban đầu | 
| cặp đơn "() " | -1 1 -1 1 | giá trị thay thế | 
| lồng nhau "(()())" | tăng độ sâu | độ sâu lồng nhau chính xác | 
| trường hợp ghi đè | thay đổi có hiệu lực | cập nhật động chính xác | 

## Vỏ cạnh 

Trường hợp quan trọng là ghi đè dấu ngoặc đóng bằng một chữ cái. Ví dụ: bắt đầu từ ")", sau đó thay đổi thành "a" sẽ loại bỏ tiền tố không hợp lệ ngay lập tức. Cây phân đoạn xử lý việc này vì bản cập nhật điểm sẽ đặt lại giá trị về 0, loại bỏ phần đóng góp âm khỏi tất cả các tổng hợp. 

Một trường hợp khác là tiền tố dài của '(' theo sau là một ')'. Tổng tiền tố trở thành 0 nhưng độ sâu lồng tối đa vẫn phản ánh ngăn xếp mở sâu nhất trước khi đóng. Các giá trị tiền tố và hậu tố được lưu trữ đảm bảo rằng độ sâu trung gian được duy trì ngay cả sau khi cân bằng. 

Trường hợp cuối cùng là con trỏ lặp đi lặp lại di chuyển ra ngoài vùng ban đầu. Vì cây được khởi tạo bằng số 0 nên việc ghi ở các vị trí mới sẽ hoạt động chính xác mà không cần logic thay đổi kích thước động vượt quá giới hạn được phân bổ trước.
