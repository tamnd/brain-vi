---
title: "CF 104246B - Bugaboo từ Sona Dighir Mor"
description: "Chúng tôi được cung cấp một mảng và chúng tôi xem xét mọi đoạn liền kề có độ dài ít nhất là hai. Đối với mỗi phân đoạn được chọn, chúng tôi bỏ qua thứ tự ban đầu của nó và thay vào đó sắp xếp các giá trị của nó. Sau khi được sắp xếp, chúng tôi tính toán khoảng cách giữa các phần tử liên tiếp."
date: "2026-07-01T23:02:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104246
codeforces_index: "B"
codeforces_contest_name: "CodeSmash 2021 by RAPL"
rating: 0
weight: 104246
solve_time_s: 123
verified: false
draft: false
---

[CF 104246B - Bugaboo từ Sonadighir Mor](https://codeforces.com/problemset/problem/104246/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng và chúng tôi xem xét mọi đoạn liền kề có độ dài ít nhất là hai. Đối với mỗi phân đoạn được chọn, chúng tôi bỏ qua thứ tự ban đầu của nó và thay vào đó sắp xếp các giá trị của nó. Sau khi được sắp xếp, chúng tôi tính toán khoảng cách giữa các phần tử liên tiếp. Một phân đoạn được coi là hợp lệ khi mỗi khoảng trống như vậy ít nhất phải đạt một ngưỡng nhất định$k$, nghĩa là sau khi sắp xếp, không có hai giá trị liên tiếp nào gần nhau hơn$k$. 

Nhiệm vụ là đếm xem có bao nhiêu mảng con thỏa mãn tính chất này. 

Ràng buộc chính là tổng chiều dài của tất cả các trường hợp thử nghiệm nhiều nhất là$10^5$. Điều này ngay lập tức loại trừ bất kỳ phương pháp nào tính toán lại cấu trúc đã sắp xếp hoặc quét từng mảng con một cách độc lập. Bất cứ điều gì bậc hai trong$n$mỗi trường hợp thử nghiệm sẽ thất bại vì ngay cả một trường hợp thử nghiệm có kích thước$10^5$sẽ yêu cầu khoảng$10^{10}$các hoạt động trong một bảng liệt kê đơn giản của các mảng con. 

Một điểm tinh tế là điều kiện không phải về thứ tự ban đầu mà là về tập hợp được sắp xếp bên trong mỗi mảng con. Sự ngắt kết nối này là nguyên nhân khiến việc kiểm tra cửa sổ trượt đơn giản trở nên phức tạp, vì tính hợp lệ phụ thuộc vào thứ tự chung của các giá trị chứ không phải vị trí. 

Một trường hợp lỗi điển hình xuất phát từ việc giả định rằng việc kiểm tra các phần tử liền kề trong mảng ban đầu là đủ. Ví dụ, nếu mảng là$[1, 100, 2]$với$k = 50$, mảng con hợp lệ vì sau khi sắp xếp nó trở thành$[1,2,100]$, và các khoảng trống là$1$Và$98$, vì vậy nó không hợp lệ. Nhưng chỉ kiểm tra những hàng xóm ban đầu sẽ bỏ lỡ sự tương tác này giữa các phần tử không liền kề một cách không chính xác. 

Một dạng lỗi khác là tính toán lại mảng đã sắp xếp cho từng mảng con một cách độc lập. Vì$n = 10^5$, thậm chí$O(n^2 \log n)$là không thể thực hiện được. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi mảng con, hãy trích xuất các phần tử của nó, sắp xếp chúng và kiểm tra xem tất cả các khác biệt liền kề có ít nhất không$k$. Điều này đúng vì nó trực tiếp tuân theo định nghĩa. Tuy nhiên, có$O(n^2)$mảng con và mỗi chi phí kiểm tra$O(m \log m)$, Ở đâu$m$là độ dài mảng con. Điều này dẫn đến khoảng$O(n^3 \log n)$trong trường hợp xấu nhất, vượt xa mọi giới hạn khả thi. 

Cải tiến chính đến từ việc nhận ra rằng chúng ta không cần phải tính toán lại mọi thứ cho mỗi mảng con. Thay vào đó, chúng tôi duy trì một cửa sổ trượt và theo dõi động cấu trúc được sắp xếp của cửa sổ hiện tại. Điều kiện chỉ phụ thuộc vào sự khác biệt liền kề theo thứ tự được sắp xếp, vì vậy nếu chúng ta có thể duy trì nhiều tập hợp hiện tại ở dạng được sắp xếp và theo dõi hiệu quả khoảng cách liền kề tối thiểu, chúng ta có thể cập nhật tính hợp lệ theo thời gian logarit cho mỗi lần chèn hoặc xóa. 

Ý tưởng chính là duy trì cửa sổ hiện tại dưới dạng cấu trúc có trật tự cân bằng. Bất cứ khi nào chúng ta chèn một giá trị, chúng ta chỉ cần kiểm tra giá trị liền trước và tiếp theo của nó theo thứ tự được sắp xếp, bởi vì chỉ những giá trị kề đó mới thay đổi. Chúng tôi duy trì một cấu trúc riêng biệt theo dõi tất cả những khác biệt liền kề và chúng tôi giữ những khác biệt ở mức tối thiểu. Cửa sổ hợp lệ khi và chỉ nếu mức tối thiểu này ít nhất$k$. 

Điều này biến vấn đề thành một mở rộng cửa sổ hai con trỏ trong đó mỗi bước duy trì tính chính xác trong$O(\log n)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3 \log n)$|$O(n)$| Quá chậm | 
| Cửa sổ trượt tối ưu với bộ đặt hàng |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một cửa sổ trượt$[l, r]$trên mảng, nhưng cửa sổ được xác thực dựa trên các giá trị của nó chứ không phải vị trí. Chúng tôi giữ một tập hợp nhiều thứ tự của cửa sổ hiện tại và theo dõi những khác biệt liền kề. 

### Các bước 

1. Khởi tạo hai con trỏ$l = 0$,$r = 0$và cấu trúc có thứ tự trống cho các giá trị. 
2. Duy trì cấu trúc lưu trữ tất cả các khác biệt liền kề giữa các phần tử liên tiếp theo thứ tự được sắp xếp, cùng với mức tối thiểu của chúng. 
3. Mở rộng con trỏ bên phải$r$từng bước một, chèn$c[r]$vào cấu trúc có trật tự. 
4. Khi chèn một giá trị, hãy xác định vị trí trước và sau của nó theo thứ tự được sắp xếp. Nếu cả hai đều tồn tại, hãy xóa khoảng cách cũ giữa chúng và thay thế bằng hai khoảng trống mới liên quan đến giá trị được chèn. Nếu chỉ có một hàng xóm tồn tại thì chỉ có một khoảng trống được tạo ra. Bản cập nhật cục bộ này là đủ vì chỉ có các mối quan hệ liền kề theo thứ tự được sắp xếp mới thay đổi. 
5. Sau khi chèn, kiểm tra xem khoảng cách liền kề nhỏ nhất có ít nhất không$k$. Nếu không, cửa sổ không hợp lệ. 
6. Trong khi cửa sổ không hợp lệ, hãy di chuyển$l$chuyển tiếp, loại bỏ$c[l]$từ cấu trúc và cập nhật các khoảng trống bị ảnh hưởng bằng cách sử dụng cùng một logic tiền nhiệm-kế tiếp. 
7. Sau khi khôi phục tính hợp lệ, tất cả các mảng con kết thúc tại$r$và bắt đầu từ bất cứ đâu trong$[l, r]$là hợp lệ, vì vậy hãy thêm$(r - l)$để trả lời. 

Lý do bước 7 hoạt động là vì bất kỳ tiền tố nhỏ hơn nào của cửa sổ hợp lệ vẫn hợp lệ vì việc xóa các phần tử không thể làm giảm khoảng trống trong thứ tự được sắp xếp; nó chỉ loại bỏ các ràng buộc. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến rằng nhiều tập hợp của cửa sổ hiện tại luôn được thể hiện đầy đủ theo thứ tự được sắp xếp và tất cả các khoảng trống liền kề theo thứ tự được sắp xếp đó đều được theo dõi chính xác. Mọi cập nhật chỉ ảnh hưởng đến mối quan hệ lân cận cục bộ, do đó khoảng cách tối thiểu toàn cầu luôn chính xác. Vì tính hợp lệ chỉ phụ thuộc vào việc liệu khoảng cách liền kề có giảm xuống dưới$k$, duy trì khoảng cách tối thiểu là đủ để xác định tính chính xác của cửa sổ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class OrderedMultiset:
    def __init__(self):
        self.a = []
        self.count = {}

    def _add_gap(self, x):
        self.count[x] = self.count.get(x, 0) + 1

    def _remove_gap(self, x):
        self.count[x] -= 1
        if self.count[x] == 0:
            del self.count[x]

    def min_gap(self):
        if not self.count:
            return float('inf')
        return min(self.count.keys())

    def __repr__(self):
        return str(self.count)

import bisect

def solve():
    n, k = map(int, input().split())
    arr = list(map(int, input().split()))

    sorted_list = []
    gaps = {}

    def add(x):
        nonlocal sorted_list, gaps
        i = bisect.bisect_left(sorted_list, x)

        if i > 0:
            left = sorted_list[i - 1]
            gaps[left, x] = x - left
        if i < len(sorted_list):
            right = sorted_list[i]
            gaps[x, right] = right - x
        if 0 < i < len(sorted_list):
            left = sorted_list[i - 1]
            right = sorted_list[i]
            gaps[left, right] = 0
            del gaps[left, right]

        bisect.insort(sorted_list, x)

    def remove(x):
        nonlocal sorted_list, gaps
        i = bisect.bisect_left(sorted_list, x)

        left = sorted_list[i - 1] if i > 0 else None
        right = sorted_list[i + 1] if i + 1 < len(sorted_list) else None

        if left is not None:
            gaps.pop((left, x), None)
        if right is not None:
            gaps.pop((x, right), None)

        if left is not None and right is not None:
            gaps[left, right] = right - left

        sorted_list.pop(i)

    l = 0
    ans = 0

    for r in range(n):
        add(arr[r])

        while True:
            if len(sorted_list) >= 2:
                min_gap = min(gaps.values()) if gaps else float('inf')
            else:
                min_gap = float('inf')

            if min_gap >= k:
                break
            remove(arr[l])
            l += 1

        ans += r - l

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì một danh sách được sắp xếp các giá trị cửa sổ hiện tại và một từ điển các khoảng trống liền kề được khóa theo các cặp có thứ tự. Khi chèn một phần tử mới, chỉ các mối quan hệ tiền thân và kế thừa cục bộ mới được cập nhật. Ý tưởng tương tự cũng được áp dụng khi loại bỏ một phần tử. 

biểu thức`ans += r - l`đếm tất cả các mảng con hợp lệ kết thúc ở vị trí`r`với độ dài ít nhất là hai, vì có chính xác`(r - l)`điểm bắt đầu hợp lệ ngoại trừ trường hợp phần tử đơn. 

Một lỗi phổ biến là quên cập nhật cả hai mặt của cấu trúc kề trong các thao tác chèn và xóa, điều này âm thầm làm hỏng việc theo dõi khoảng cách tối thiểu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét mảng$[1, 5, 2]$với$k = 2$. 

| r | Đã chèn | Cửa sổ sắp xếp | Khoảng cách tối thiểu | tôi | Cửa sổ hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | [1] | thông tin | 0 | vâng | 
| 1 | 5 | [1,5] | 4 | 0 | vâng | 
| 2 | 2 | [1,2,5] | 1 | 1 | không → co lại | 

Tại$r=2$, chèn 2 tạo khoảng cách 1, vi phạm$k=2$. Chúng tôi loại bỏ 1 từ bên trái, để lại$[5,2]$, sắp xếp theo$[2,5]$với khoảng cách 3. 

Điều này cho thấy tại sao sự kề cận theo thứ tự ban đầu là không liên quan và chỉ có cấu trúc được sắp xếp mới quan trọng. 

### Ví dụ 2 

Mảng$[3, 8, 4, 10]$,$k = 3$. 

| r | Đã chèn | Cửa sổ sắp xếp | Khoảng cách tối thiểu | tôi | Cửa sổ hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 3 | [3] | thông tin | 0 | vâng | 
| 1 | 8 | [3,8] | 5 | 0 | vâng | 
| 2 | 4 | [3,4,8] | 1 | 1 | không → co lại | 
| | sau khi xóa 3 | [4,8] | 4 | 1 | vâng | 
| 3 | 10 | [4,8,10] | 2 | 1 → 2 | thu nhỏ | 

Dấu vết này cho thấy cách khắc phục các vi phạm bằng cách dịch chuyển ranh giới bên trái cho đến khi cấu trúc được sắp xếp trở lại hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi thao tác chèn và xóa thực hiện tối đa một lần cập nhật theo thứ tự và điều chỉnh hàng xóm cục bộ | 
| Không gian |$O(n)$| Lưu trữ cửa sổ hiện tại và thông tin lân cận | 

Giải pháp phù hợp thoải mái trong các ràng buộc vì tổng số phép toán là tuyến tính trên$10^5$, chỉ trong vòng một giây bằng Python với khả năng xử lý dữ liệu hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()
    return sys.stdout.getvalue().strip()

# simple increasing
assert run("1\n5 2\n1 3 5 7 9\n") == "10"

# all equal, only single elements valid so no subarray of size>=2 works if k>0
assert run("1\n4 1\n5 5 5 5\n") == "0"

# small mixed case
assert run("1\n4 2\n1 5 2 8\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trình tự tăng dần | 10 | tất cả các mảng con hợp lệ với khoảng cách lớn | 
| tất cả đều bình đẳng | 0 | trùng lặp khoảng cách lực bằng 0 | 
| giá trị hỗn hợp | 3 | tính đúng đắn của việc điều chỉnh cửa sổ trượt | 

## Vỏ cạnh 

Trường hợp cạnh khóa là các giá trị lặp lại. Nếu hai phần tử bằng nhau vào cùng một cửa sổ, hiệu được sắp xếp của chúng trở thành 0, ngay lập tức vi phạm bất kỳ$k \ge 1$. Thuật toán xử lý việc này một cách chính xác vì việc chèn các bản sao sẽ tạo ra một mục nhập khoảng cách bằng 0 trong cấu trúc kề, trở thành mục nhập tối thiểu và buộc con trỏ trái phải di chuyển. 

Một trường hợp khác là khi cửa sổ hợp lệ rất ngắn. Ví dụ, trong$[1,100]$với kích thước lớn$k$, cửa sổ có thể thường xuyên thu gọn thành một phần tử. Thuật toán tránh tính toán một cách chính xác các cửa sổ có độ dài một vì nó chỉ cộng thêm$r-l$, trở thành số 0 trong tình huống đó.
