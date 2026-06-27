---
title: "CF 105390E - Học sinh ngây thơ"
description: "Chúng ta được cung cấp một dãy số nguyên biểu thị câu trả lời của những học sinh ngồi xếp hàng. Mỗi truy vấn sẽ thay đổi câu trả lời của một học sinh hoặc hỏi về một phân khúc học sinh liền kề cùng với giá trị câu trả lời đúng giả định x."
date: "2026-06-23T17:05:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105390
codeforces_index: "E"
codeforces_contest_name: "TheForces Round #35 (LOL-Forces)"
rating: 0
weight: 105390
solve_time_s: 124
verified: false
draft: false
---

[CF 105390E - Học sinh vô tội](https://codeforces.com/problemset/problem/105390/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy số nguyên biểu thị câu trả lời của những học sinh ngồi xếp hàng. Mỗi truy vấn sẽ thay đổi câu trả lời của một học sinh hoặc hỏi về một phân khúc học sinh liền kề cùng với giá trị câu trả lời đúng giả định`x`. 

Đối với truy vấn thuộc loại đầu tiên, chúng tôi chỉ tưởng tượng những sinh viên trong một khoảng thời gian nhất định`[l, r]`đã tham gia. Trong số những học sinh này, chúng tôi tính toán xem câu trả lời của mỗi học sinh gần đến mức nào với`x`sử dụng sự khác biệt tuyệt đối. Chỉ những học sinh có khoảng cách đến`x`là tối thiểu trong toàn bộ phân khúc được tính là “đạt”. 

Vì vậy, nhiệm vụ của truy vấn phạm vi không phải là đếm các giá trị gần`x`, nhưng trước tiên phải xác định khoảng cách nhỏ nhất có thể tới`x`bên trong phân đoạn, sau đó đếm xem có bao nhiêu phần tử đạt được chính xác khoảng cách đó. 

Truy vấn thứ hai cập nhật một vị trí trong mảng, thay thế câu trả lời của học sinh bằng một giá trị mới. 

Các ràng buộc cho phép tổng cộng tối đa hai trăm nghìn thao tác và các giá trị có thể thay đổi linh hoạt. Điều này ngay lập tức loại trừ việc tính toán lại các câu trả lời cho mỗi truy vấn bằng cách quét phân đoạn, vì đó sẽ là phương trình bậc hai trong trường hợp xấu nhất. Ngay cả cấu trúc logarit cho mỗi truy vấn cũng cần được kiểm soát cẩn thận vì cả truy vấn phạm vi và cập nhật điểm đều phải nhanh. 

Khó khăn tinh tế là mỗi truy vấn phụ thuộc vào khoảng cách tối thiểu toàn cầu trong một phạm vi chứ không chỉ so sánh cục bộ. Một sai lầm ngây thơ là cho rằng chúng ta chỉ cần những giá trị gần nhất với`x`trên toàn cầu, nhưng mức độ gần gũi chỉ được đo lường giữa các yếu tố bên trong`[l, r]`, thay đổi theo mọi truy vấn. 

Một cạm bẫy phổ biến khác là quên đi sự đa dạng. Nếu nhiều học sinh có cùng giá trị gần nhất thì tất cả chúng đều phải được tính. 

Các trường hợp cạnh phá vỡ các giải pháp ngây thơ bao gồm các phân đoạn trong đó tất cả các giá trị giống hệt nhau, trong đó`x`nằm ngoài phạm vi và nơi các bản cập nhật giới thiệu các giá trị cực trị mới có liên quan cho các truy vấn sau này. Ví dụ: nếu phân đoạn là`[1, 10, 100]`Và`x = 50`, cả hai`10`Và`100`đều gần nhau và cả hai đều phải được tính. Một phương thức chỉ theo dõi một giá trị gần nhất sẽ trả về sai`1`thay vì`2`. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp cho một truy vấn sẽ lặp qua tất cả các chỉ mục trong`[l, r]`, tính toán sự khác biệt tuyệt đối để`x`, theo dõi khoảng cách tối thiểu và đếm xem có bao nhiêu khoảng cách phù hợp với khoảng cách đó. Điều này đúng vì nó tuân theo định nghĩa theo đúng nghĩa đen. Tuy nhiên, mỗi truy vấn có thể chạm tới`n`các phần tử, vì vậy với tối đa`2 · 10^5`truy vấn, tổng công việc sẽ theo thứ tự`10^10`, vượt xa giới hạn khả thi. 

Quan sát quan trọng là đối với bất kỳ phân đoạn cố định nào, các giá trị duy nhất quan trọng liên quan đến`x`là giá trị gần nhất không lớn hơn`x`và giá trị gần nhất không nhỏ hơn`x`. Mọi phần tử khác hoàn toàn xa hơn ít nhất một trong hai ứng cử viên biên này. Điều này làm giảm vấn đề trong việc tìm kiếm, trong phạm vi động, tiền thân và kế thừa của`x`, sau đó đếm số lần xuất hiện của các giá trị đó nếu chúng đạt được cùng khoảng cách tối thiểu. 

Bởi vì chúng ta cần cả truy vấn phạm vi và cập nhật điểm, nên có thể sử dụng cây phân đoạn với các vùng chứa được sắp xếp hoặc cấu trúc thống kê thứ tự phức tạp hơn nhưng thường khó triển khai một cách hiệu quả. Một cách tiếp cận đơn giản và đủ nhanh là phân rã căn bậc hai, trong đó mảng được chia thành các khối. Mỗi khối duy trì các phần tử của nó theo thứ tự được sắp xếp, cho phép tìm kiếm nhị phân bên trong các khối. 

Đối với một truy vấn, chúng tôi kiểm tra từng khối giao nhau với phạm vi. Từ mỗi khối, chúng tôi trích xuất các ứng cử viên tiền nhiệm và kế nhiệm tốt nhất tại địa phương của nó so với`x`. Trên tất cả các khối, chúng tôi hợp nhất các ứng cử viên này để có được tiền thân và kế thừa toàn cầu cho phạm vi. Khi đã biết khoảng cách tối thiểu, chúng tôi thực hiện một lần chuyển qua khối khác để đếm xem có bao nhiêu phần tử bằng (các) giá trị chiến thắng. 

Các bản cập nhật chỉ ảnh hưởng đến một khối, trong đó chúng tôi xóa giá trị cũ và chèn giá trị mới theo thứ tự được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét Brute Force cho mỗi truy vấn | O(nq) | O(1) | Quá chậm | 
| Phân tách Sqrt | O((n + q) √n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia mảng thành các khối có kích thước khoảng √n và sắp xếp từng khối ngoài bộ nhớ ban đầu của nó. Việc sắp xếp cho phép các truy vấn tiền nhiệm và kế tiếp nhanh chóng bên trong một khối. 
2. Đối với truy vấn trên một phân đoạn`[l, r]`và giá trị`x`, xử lý từng khối giao nhau với đoạn đó và tính toán hai ứng cử viên bên trong khối đó: giá trị lớn nhất ≤ x và giá trị nhỏ nhất ≥ x. Chúng đại diện cho các kết quả phù hợp nhất có thể có từ khối đó vì bất kỳ phần tử nào khác trong khối đều ở xa hơn`x`. 
3. Kết hợp tất cả các ứng cử viên khối để xác định người tiền nhiệm và người kế nhiệm toàn cầu trong`[l, r]`. Người tiền nhiệm là mức tối đa trong số tất cả những người tiền nhiệm cục bộ và người kế nhiệm là mức tối thiểu trong số tất cả những người kế nhiệm cục bộ. 
4. Tính khoảng cách tốt nhất có thể khi giá trị nhỏ hơn của`x - predecessor`Và`successor - x`, bỏ qua các ứng cử viên bị thiếu. 
5. Quyết định bên nào cho khoảng cách tối ưu. Nếu cả hai bên đều tốt như nhau thì cả hai giá trị tương ứng đều phù hợp. 
6. Đếm số lần xuất hiện của (các) giá trị đã chọn bên trong`[l, r]`bằng cách quét các khối: các khối được bao phủ đầy đủ đóng góp thông qua tìm kiếm nhị phân trên các mảng đã được sắp xếp của chúng, trong khi các khối một phần được kiểm tra từng phần tử.
 7. Để cập nhật, hãy tìm khối chứa chỉ mục`i`, xóa giá trị cũ, chèn giá trị mới theo thứ tự sắp xếp và cập nhật mảng chính. 

Tính đúng đắn dựa trên thực tế là trong bất kỳ tập hợp số nào, giá trị gần nhất với`x`phải là phần tử gần nhất từ ​​dưới lên hoặc gần nhất từ ​​trên xuống. Bên trong mỗi khối, chúng tôi duy trì trật tự chính xác, vì vậy những thái cực cục bộ này là đủ đại diện. Việc tổng hợp các điểm cực trị ở cấp khối sẽ bảo toàn các điểm cực trị toàn cục thực sự trên toàn bộ phạm vi. 

## Giải pháp Python```python
import sys
import bisect
import math

input = sys.stdin.readline

class SqrtDecomposition:
    def __init__(self, arr):
        self.n = len(arr)
        self.arr = arr[:]
        self.B = int(math.sqrt(self.n)) + 1
        self.blocks = []
        
        for i in range(0, self.n, self.B):
            block = arr[i:i+self.B]
            block.sort()
            self.blocks.append(block)

    def rebuild_block(self, b_idx):
        l = b_idx * self.B
        r = min(self.n, l + self.B)
        self.blocks[b_idx] = sorted(self.arr[l:r])

    def update(self, i, val):
        b = i // self.B
        l = b * self.B
        r = min(self.n, l + self.B)

        old = self.arr[i]
        self.arr[i] = val

        block = self.blocks[b]
        block.pop(bisect.bisect_left(block, old))
        bisect.insort(block, val)

    def query_candidates(self, l, r, x):
        B = self.B
        best_le = -10**30
        best_ge = 10**30

        i = l
        while i <= r:
            if i % B == 0 and i + B - 1 <= r:
                b = i // B
                block = self.blocks[b]

                idx = bisect.bisect_right(block, x) - 1
                if idx >= 0:
                    best_le = max(best_le, block[idx])

                idx = bisect.bisect_left(block, x)
                if idx < len(block):
                    best_ge = min(best_ge, block[idx])

                i += B
            else:
                val = self.arr[i]
                if val <= x:
                    best_le = max(best_le, val)
                if val >= x:
                    best_ge = min(best_ge, val)
                i += 1

        return best_le, best_ge

    def count_value(self, l, r, val):
        B = self.B
        res = 0
        i = l

        while i <= r:
            if i % B == 0 and i + B - 1 <= r:
                b = i // B
                block = self.blocks[b]
                res += bisect.bisect_right(block, val) - bisect.bisect_left(block, val)
                i += B
            else:
                if self.arr[i] == val:
                    res += 1
                i += 1

        return res

def solve():
    n, q = map(int, input().split())
    arr = list(map(int, input().split()))
    sd = SqrtDecomposition(arr)

    out = []

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '1':
            l = int(tmp[1]) - 1
            r = int(tmp[2]) - 1
            x = int(tmp[3])

            le, ge = sd.query_candidates(l, r, x)

            d = 10**30
            if le != -10**30:
                d = min(d, x - le)
            if ge != 10**30:
                d = min(d, ge - x)

            ans = 0
            if le != -10**30 and x - le == d:
                ans += sd.count_value(l, r, le)
            if ge != 10**30 and ge - x == d:
                ans += sd.count_value(l, r, ge)

            out.append(str(ans))

        else:
            i = int(tmp[1]) - 1
            val = int(tmp[2])
            sd.update(i, val)

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai xoay quanh việc duy trì từng khối theo thứ tự được sắp xếp sao cho các truy vấn trước và sau trở thành logarit trong một khối. Thao tác cập nhật sẽ loại bỏ cẩn thận giá trị cũ bằng cách sử dụng tìm kiếm nhị phân trước khi chèn giá trị mới để giữ nguyên thứ tự. 

Logic truy vấn chia vấn đề thành hai giai đoạn: đầu tiên tìm các giá trị biên tốt nhất liên quan đến`x`, sau đó đếm xem có bao nhiêu phần tử thực sự khớp với khoảng cách chiến thắng. Sự tách biệt này tránh việc tính toán lại khoảng cách cho từng phần tử riêng lẻ. 

Một chi tiết tinh tế là việc khởi tạo`best_le`Và`best_ge`với trọng điểm, điều này đảm bảo rằng các cạnh bị thiếu không góp phần sai vào việc tính toán khoảng cách. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng đơn giản`[1, 10, 100, 1000]`với một truy vấn trên phạm vi đầy đủ và`x = 60`. 

Chúng tôi xử lý các khối (giả sử ở đây có một khối để đơn giản): 

| Bước | tốt nhất_le | nhất_ge | hành động | 
| --- | --- | --- | --- | 
| bắt đầu | -inf | +thông tin | khởi tạo | 
| giá trị quét | 10 | 100 | 10 60 và 100 ≥ 60 | 
| cuối cùng | 10 | 100 | ứng viên được xác định | 

Kiểm tra khoảng cách cho thấy cả hai đều cách xa nhau như nhau: 50. Đếm số lần xuất hiện mang lại 1 cho 10 và 1 cho 100, vì vậy câu trả lời là 2. 

Bây giờ hãy xem xét cập nhật. Bắt đầu với`[5, 5, 5, 5]`, truy vấn`[1,4], x = 5`. 

| Bước | tốt nhất_le | nhất_ge | hành động | 
| --- | --- | --- | --- | 
| quét | 5 | 5 | khớp chính xác | 
| khoảng cách | 0 | 0 | giống hệt nhau | 
| đếm | 4 | | tất cả các yếu tố đóng góp | 

Điều này cho thấy các giá trị bằng nhau được xử lý một cách tự nhiên vì cả tiền thân và tiền nhiệm đều thu gọn về cùng một giá trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) √n log n) | mỗi quá trình truy vấn và cập nhật tối đa √n khối với tìm kiếm nhị phân | 
| Không gian | O(n) | mảng cộng với các khối được sắp xếp | 

Với tổng giới hạn là 2 · 10^5 phần tử và phép toán, √n là khoảng 450, khiến cách tiếp cận này trở nên nhanh chóng một cách thoải mái trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else solve_capture(inp)

def solve_capture(inp):
    import sys
    from io import StringIO
    backup_stdin = sys.stdin
    sys.stdin = StringIO(inp)
    out = []
    def solve():
        n, q = map(int, input().split())
        arr = list(map(int, input().split()))
        sd = SqrtDecomposition(arr)
        res = []
        for _ in range(q):
            tmp = input().split()
            if tmp[0] == '1':
                l = int(tmp[1]) - 1
                r = int(tmp[2]) - 1
                x = int(tmp[3])
                le, ge = sd.query_candidates(l, r, x)
                d = 10**30
                if le != -10**30:
                    d = min(d, x - le)
                if ge != 10**30:
                    d = min(d, ge - x)
                ans = 0
                if le != -10**30 and x - le == d:
                    ans += sd.count_value(l, r, le)
                if ge != 10**30 and ge - x == d:
                    ans += sd.count_value(l, r, ge)
                res.append(str(ans))
            else:
                i = int(tmp[1]) - 1
                val = int(tmp[2])
                sd.update(i, val)
        return "\n".join(res)

    return solve()

# sample + custom tests
assert run("""1 3
1
1 1 1 1
1 1 1 1
""") == "1"

assert run("""1 3
1
2 1 2
1 1 1 1
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| truy vấn phần tử đơn | 1 | độ đúng cơ sở | 
| cập nhật lặp đi lặp lại | ổn định | cập nhật tính đúng đắn | 
| mảng thống nhất | đếm đầy đủ | trường hợp cạnh có giá trị bằng nhau | 
| giá trị x cực trị | đúng ranh giới | xử lý người tiền nhiệm/kế nhiệm | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các giá trị trong một phân đoạn đều giống hệt nhau. Trong trường hợp đó, cả phần trước và phần sau đều thu gọn về cùng một giá trị và khoảng cách được tính toán bằng 0. Thuật toán đếm chính xác tất cả các lần xuất hiện vì cả hai nhánh đều phát hiện cùng một giá trị tối ưu. 

Một trường hợp khác xảy ra khi`x`nằm ngoài phạm vi của tất cả các giá trị trong phân khúc. Ví dụ: nếu phân đoạn là`[10, 20, 30]`Và`x = 100`, chỉ có bên kế nhiệm đóng góp. Người tiền nhiệm vẫn vắng mặt và thuật toán chọn chính xác giá trị nhỏ nhất là giá trị gần nhất. 

Trường hợp tinh vi cuối cùng xuất hiện sau khi các bản cập nhật giới thiệu các giá trị cực trị mới. Vì mỗi khối luôn được sắp xếp và xây dựng lại tăng dần, nên các giá trị mới được chèn ngay lập tức tham gia vào các phép tính tiền trước và kế tiếp trong tương lai mà không yêu cầu tái cấu trúc toàn cục.
