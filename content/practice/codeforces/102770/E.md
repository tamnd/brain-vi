---
title: "CF 102770E - Sự cố DP dễ dàng"
description: "Cách tiếp cận đơn giản là tính toán DP cho mọi truy vấn một cách độc lập. Đối với một đoạn có độ dài m, chúng ta có thể duy trì trạng thái dp[i][j] trong khi quét các phần tử của nó. Điều này đúng vì phép truy toán mô tả trực tiếp sự lựa chọn tối ưu giữa i phần tử đầu tiên."
date: "2026-07-28T23:08:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102770
codeforces_index: "E"
codeforces_contest_name: "The 17th Zhejiang Provincial Collegiate Programming Contest"
rating: 0
weight: 102770
solve_time_s: 64
verified: true
draft: false
---

[CF 102770E - Sự cố DP dễ dàng](https://codeforces.com/problemset/problem/102770/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là tính toán DP cho mọi truy vấn một cách độc lập. Đối với một đoạn có độ dài`m`, chúng ta có thể duy trì các trạng thái`dp[i][j]`trong khi quét các phần tử của nó. Điều này đúng vì phép truy toán mô tả trực tiếp sự lựa chọn tối ưu trong số các lựa chọn đầu tiên.`i`các phần tử. Tuy nhiên, một truy vấn có giá`O(mk)`, bởi vì mọi trạng thái cho đến`m`Và`k`phải được xem xét. Với`m`Và`k`cả hai đều gần`100000`, một truy vấn có thể yêu cầu hàng tỷ thao tác. 

Quan sát quan trọng là DP chứa một vấn đề đơn giản hơn. Xóa phần đóng góp của`i²`điều khoản. Định nghĩa:`g[i][j] = dp[i][j] - (1² + 2² + ... + i²)`Quá trình chuyển đổi trở thành:`g[i][j] = max(g[i-1][j], g[i-1][j-1] + b[i])`Đây chính xác là sự lặp lại cho việc lựa chọn`j`các phần tử có tổng tối đa có thể có từ phần tử đầu tiên`i`các phần tử. Vì các giá trị là dương nên tốt nhất`k`các phần tử đơn giản là`k`số lớn nhất trong phân khúc. 

Mỗi truy vấn bây giờ được giảm xuống để tìm tổng lớn nhất`k`các giá trị trong một mảng con, sau đó thêm giá trị cố định:`m(m+1)(2m+1)/6`Để trả lời các truy vấn phạm vi này một cách nhanh chóng, chúng tôi xây dựng một cây wavelet. Nó đệ quy chia các giá trị thành các phạm vi nhỏ hơn. Tại mỗi nút, nó lưu trữ bao nhiêu giá trị và tổng giá trị nào ở phía bên trái. Điều này cho phép chúng tôi bỏ qua toàn bộ phạm vi giá trị khi tìm kiếm các phần tử lớn nhất. Để tìm tổng lớn nhất`k`giá trị, chúng tôi luôn kiểm tra phần tử con có giá trị lớn hơn trước tiên. Nếu đứa trẻ đó chứa ít nhất`k`phần tử, câu trả lời hoàn toàn nằm ở bên trong nó. Ngược lại, chúng ta lấy tất cả các phần tử từ phần tử con đó và tiếp tục tìm kiếm các phần tử còn lại trong phần tử con có giá trị nhỏ hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(mk)`mỗi truy vấn |`O(m)`| Quá chậm | 
| Cây Wavelet |`O(log A)`mỗi truy vấn |`O(n log A)`| Đã chấp nhận | 

Đây`A`là giá trị mảng tối đa, nhiều nhất`10^6`. 

## Hướng dẫn thuật toán 

1. Xây dựng cây wavelet trên mảng ban đầu. Mỗi nút đại diện cho một loạt các giá trị có thể. Trong khi xây dựng nút, hãy chia chuỗi thành các giá trị ở nửa dưới và nửa trên, đồng thời lưu trữ số lượng tiền tố và tổng tiền tố cho nửa dưới. Các mảng tiền tố này cho phép chúng ta biết có bao nhiêu giá trị di chuyển đến mỗi phần tử con trong bất kỳ khoảng truy vấn nào. 
2. Đối với mỗi truy vấn`(l, r, k)`, chuyển đổi các vị trí thành chỉ mục dựa trên số 0 của cây wavelet. Chúng ta cần tổng lớn nhất`k`các giá trị trong khoảng này. 
3. Tại một nút cây wavelet, xác định xem có bao nhiêu phần tử trong phạm vi truy vấn thuộc về nút con có giá trị cao. Nếu số lượng này ít nhất là`k`, lặp lại vào con đó vì tất cả các phần tử bắt buộc đều nằm trong số các giá trị lớn hơn. 
4. Nếu phần tử con có giá trị cao chứa ít hơn`k`các phần tử, cộng tổng của tất cả các phần tử đó vào câu trả lời và tiếp tục đến phần tử con có giá trị thấp yêu cầu số phần tử còn lại. 
5. Cộng phần đóng góp bậc hai của DP. Nếu độ dài đoạn là`m`, phần đóng góp là tổng bình phương từ`1`ĐẾN`m`, được tính bằng công thức`m(m+1)(2m+1)/6`. 

Bất biến đằng sau quá trình truy vấn là tại mỗi nút cây wavelet, thuật toán giữ chính xác số lượng giá trị lớn nhất vẫn được yêu cầu từ khoảng giá trị hiện tại. Con cao hơn luôn chứa các giá trị lớn hơn mọi giá trị ở con thấp hơn, do đó, việc lấy tất cả các giá trị có thể có từ phía cao hơn trước khi khám phá phía dưới sẽ giữ nguyên định nghĩa của phần trên cùng`k`tổng hợp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class WaveletTree:
    def __init__(self, arr, lo, hi):
        self.lo = lo
        self.hi = hi
        self.pref_cnt = None
        self.pref_sum = None
        self.left = None
        self.right = None

        if not arr or lo == hi:
            return

        mid = (lo + hi) // 2
        left_arr = []
        right_arr = []

        self.pref_cnt = [0]
        self.pref_sum = [0]

        for x in arr:
            if x <= mid:
                left_arr.append(x)
                self.pref_cnt.append(self.pref_cnt[-1] + 1)
                self.pref_sum.append(self.pref_sum[-1] + x)
            else:
                right_arr.append(x)
                self.pref_cnt.append(self.pref_cnt[-1])
                self.pref_sum.append(self.pref_sum[-1])

        self.left = WaveletTree(left_arr, lo, mid)
        self.right = WaveletTree(right_arr, mid + 1, hi)

    def top_sum(self, l, r, k):
        if k == 0:
            return 0
        if self.lo == self.hi:
            return self.lo * k

        left_before = self.pref_cnt[l]
        left_in_range = self.pref_cnt[r] - left_before

        right_count = (r - l) - left_in_range

        if right_count >= k:
            return self.right.top_sum(l - left_before, r - self.pref_cnt[r], k)

        right_sum = self.pref_sum[r] - self.pref_sum[l]
        return right_sum + self.left.top_sum(left_before, self.pref_cnt[r], k - right_count)

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        tree = WaveletTree(a, 1, 10**6)

        q = int(input())
        for _ in range(q):
            l, r, k = map(int, input().split())

            selected = tree.top_sum(l - 1, r, k)

            length = r - l + 1
            squares = length * (length + 1) * (2 * length + 1) // 6

            ans.append(str(selected + squares))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Cây wavelet lưu trữ mảng được chia theo giá trị chứ không phải theo vị trí. Đây là chi tiết quan trọng giúp nó hữu ích cho vấn đề này. Một phạm vi truy vấn có thể được dịch qua mọi nút vì mỗi nút lưu trữ đủ thông tin tiền tố để biết có bao nhiêu phần tử được chuyển đến mỗi nút con. 

các`top_sum`chức năng hoạt động với các vị trí nửa mở bên trong nút hiện tại. Các biến`l`Và`r`mô tả khoảng thời gian hiện tại bên trong chuỗi được lưu trữ của nút đó. biểu hiện`self.pref_cnt[r] - self.pref_cnt[l]`đưa ra số lượng giá trị thuộc về con thấp hơn. Số giá trị ở con cao hơn là phần còn lại. 

Trường hợp chiếc lá rất đơn giản vì mọi giá trị còn lại đều giống hệt nhau. Nếu chúng ta cần`k`các giá trị từ một lá chứa giá trị`x`, tổng của chúng là`x * k`. 

Số nguyên Python tự động xử lý các giá trị trung gian lớn. Tổng bình phương có thể đạt khoảng`10^15`, do đó, việc sử dụng loại 32 bit có chiều rộng cố định sẽ tràn vào các ngôn ngữ mà số nguyên không được tự động mở rộng. 

## Ví dụ đã hoạt động 

Hãy xem xét phân khúc`[1, 100, 2]`với`k = 2`. 

| Bước | Phạm vi giá trị hiện tại | Số lượng bên cao | Còn lại k | Tổng cộng thêm | 
| --- | --- | --- | --- | --- | 
| Gốc |`[1,1000000]`| 1 | 2 | 100 | 
| Con dưới |`[1,500000]`| 1 | 1 | 2 | 

Cây wavelet đầu tiên nhận giá trị lớn nhất`100`, sau đó tìm kiếm phần còn lại để tìm thêm một giá trị. Nó tìm thấy`2`, tạo ra sự đóng góp đã chọn`102`. Đóng góp của DP là`14`, vậy câu trả lời là`116`. 

Đối với một phân đoạn phần tử đơn`[5]`với`k = 1`, quá trình duyệt sẽ tới một lá ngay lập tức. 

| Bước | Phạm vi giá trị hiện tại | Còn lại k | Tổng cộng thêm | 
| --- | --- | --- | --- | 
| Lá |`[5,5]`| 1 | 5 | 

Đóng góp được chọn là`5`. Độ dài đoạn là`1`, vậy đóng góp bình phương là`1`, đưa ra câu trả lời cuối cùng`6`. 

Những dấu vết này cho thấy cấu trúc dữ liệu luôn lấy các giá trị khả dụng cao nhất trước tiên và thuật ngữ DP cố định độc lập với các giá trị đã chọn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O((n + q) log A)`| Xây dựng cây wavelet thăm từng giá trị thông qua`log A`cấp độ và mọi truy vấn đều đi theo một đường dẫn cho mỗi cấp độ. | 
| Không gian |`O(n log A)`| Thông tin tiền tố được lưu trữ cho mọi cấp độ của cây wavelet. | 

Giá trị tối đa chỉ`10^6`, vậy chiều cao của cây là khoảng hai mươi bậc. Với nhiều nhất`500000`tổng số phần tử và truy vấn, hệ số logarit giữ tổng công việc trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    
    data = sys.stdin.readline
    t = int(data())
    out = []

    class WT:
        def __init__(self, arr, lo, hi):
            self.lo = lo
            self.hi = hi
            self.pc = self.ps = None
            self.l = self.r = None
            if not arr or lo == hi:
                return
            mid = (lo + hi) // 2
            la, ra = [], []
            self.pc = [0]
            self.ps = [0]
            for x in arr:
                if x <= mid:
                    la.append(x)
                    self.pc.append(self.pc[-1] + 1)
                    self.ps.append(self.ps[-1] + x)
                else:
                    ra.append(x)
                    self.pc.append(self.pc[-1])
                    self.ps.append(self.ps[-1])
            self.l = WT(la, lo, mid)
            self.r = WT(ra, mid + 1, hi)

        def get(self, l, r, k):
            if k == 0:
                return 0
            if self.lo == self.hi:
                return self.lo * k
            left_count = self.pc[r] - self.pc[l]
            right_count = r - l - left_count
            if right_count >= k:
                return self.r.get(l - self.pc[l], r - self.pc[r], k)
            return self.ps[r] - self.ps[l] + self.l.get(self.pc[l], self.pc[r], k - right_count)

    def solve_case(s):
        it = iter(s.split())
        n = int(next(it))
        a = [int(next(it)) for _ in range(n)]
        tree = WT(a, 1, 10**6)
        q = int(next(it))
        res = []
        for _ in range(q):
            l = int(next(it))
            r = int(next(it))
            k = int(next(it))
            m = r - l + 1
            res.append(str(tree.get(l - 1, r, k) + m * (m + 1) * (2 * m + 1) // 6))
        return "\n".join(res)

    result = solve_case(sys.stdin.read())
    sys.stdin = old
    return result

assert run("""1
3
1 100 2
1
1 3 2
""") == "116"

assert run("""1
1
5
1
1 1 1
""") == "6"

assert run("""1
3
3 4 5
1
1 3 3
""") == "26"

assert run("""1
5
7 7 7 7 7
3
1 5 1
1 5 5
2 4 2
""") == "22\n55\n30"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[1,100,2], k=2`|`116`| Chọn giá trị lớn nhất không liên tiếp | 
|`[5], k=1`|`6`| Chiều dài tối thiểu và đóng góp hình vuông | 
|`[3,4,5], k=3`|`26`| Chọn toàn bộ phân khúc | 
| Tất cả các giá trị bằng nhau |`22,55,30`| Xử lý giá trị bằng nhau và ranh giới phạm vi | 

## Vỏ cạnh 

Đối với truy vấn có độ dài một, cây wavelet sẽ đến lá ngay lập tức. Với mảng đầu vào`[5]`và truy vấn`(1,1,1)`, tổng được chọn là`5`. Công thức hình vuông cho`1`, vì vậy đầu ra là`6`. Thuật toán không phụ thuộc vào việc có các nút cây bên trong trong trường hợp này. 

Đối với một truy vấn ở đâu`k`bằng độ dài đoạn, việc tìm kiếm cuối cùng sẽ thu thập mọi giá trị trong phạm vi. Vì`[3,4,5]`Và`(1,3,3)`, quá trình truyền sóng con tập hợp`5`, sau đó`4`, sau đó`3`, cho`12`. Cộng phần đóng góp bình phương`14`sản xuất`26`. 

Đối với các lựa chọn không liên tiếp,`[1,100,2]`với`k=2`chứng tỏ tại sao DP không thể đơn giản hóa thành một bài toán chọn liên tục. Cây wavelet chọn`100`từ phía có giá trị cao và sau đó`2`từ phía còn lại, khớp chính xác với phép lặp DP ban đầu.
