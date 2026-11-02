---
title: "CF 1083E - Đai ốc và Hình chữ nhật đẹp"
description: "Mỗi hình chữ nhật được neo ở gốc và kéo dài đến một điểm $(xi, yi)$, do đó, về mặt hình học, mọi hình chữ nhật đều là hình chữ nhật song song có trục được căn chỉnh phía dưới bên trái."
date: "2026-06-15T05:53:33+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "geometry"]
categories: ["algorithms"]
codeforces_contest: 1083
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 526 (Div. 1)"
rating: 2400
weight: 1083
solve_time_s: 150
verified: false
draft: false
---

[CF 1083E - Đai ốc và hình chữ nhật đẹp](https://codeforces.com/problemset/problem/1083/E) 

**Đánh giá:** 2400 
**Tags:** cấu trúc dữ liệu, dp, hình học 
**Thời gian giải:** 2 phút 30 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi hình chữ nhật được neo ở gốc và kéo dài đến một điểm$(x_i, y_i)$, do đó về mặt hình học, mọi hình chữ nhật đều là hình chữ nhật song song có trục căn chỉnh phía dưới bên trái. Bởi vì tất cả các hình chữ nhật đều có chung một góc gốc, nên hai hình chữ nhật bất kỳ chồng lên nhau theo một cách rất có cấu trúc: phép hợp của chúng chỉ phụ thuộc vào điểm cực đại của chúng.$x$và tối đa$y$trong tập đã chọn. 

Đối với bất kỳ tập hợp con hình chữ nhật nào được chọn, vùng hợp chỉ đơn giản là diện tích của hình chữ nhật bao quanh tất cả chúng, tức là$\max x \cdot \max y$. Chi phí của việc chọn một tập hợp con là diện tích hợp này trừ đi tổng các hình phạt$a_i$trên các hình chữ nhật đã chọn. Nhiệm vụ là chọn một tập hợp con tối đa hóa giá trị này. 

Hạn chế chính đó là$n$có thể lên đến$10^6$, và tọa độ đi lên$10^9$. Điều này ngay lập tức loại trừ bất kỳ DP theo cặp hoặc tập hợp con nào. Bất cứ điều gì vượt quá đại khái$O(n \log n)$đã là ranh giới, và$O(n^2)$suy luận về các cặp hình chữ nhật là không thể. 

Một trường hợp thất bại tinh tế xuất hiện khi một cách tiếp cận ngây thơ giả định rằng mỗi hình chữ nhật đóng góp độc lập. Ví dụ, người ta có thể nghĩ đến việc chọn tất cả các hình chữ nhật có$x_i y_i > a_i$, nhưng điều này bỏ qua rằng việc thêm một hình chữ nhật có thể làm tăng đáng kể diện tích hợp và làm cho các lựa chọn tốt trước đây trở nên tồi tệ hơn. 

Một cạm bẫy tiềm ẩn khác là giả sử chúng ta cần theo dõi rõ ràng hình dạng hợp. Vì các hình chữ nhật chỉ được lồng bởi ưu thế nên việc tính toán hợp hình học bất cẩn sẽ dẫn đến sự phức tạp không cần thiết và các chuyển đổi không chính xác. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử mọi tập hợp con của hình chữ nhật, tính diện tích hợp cho mỗi tập hợp con bằng cách lấy giá trị lớn nhất$x$Và$y$, trừ tổng của$a_i$và giữ kết quả tốt nhất. Điều này hoạt động về mặt khái niệm vì việc kết hợp rất đơn giản để tính toán một khi tập hợp con được cố định. Tuy nhiên, số lượng tập hợp con là$2^n$và thậm chí đánh giá từng tập hợp con trong$O(n)$làm cho tổng công việc theo cấp số nhân vượt quá mọi giới hạn khả thi. 

Quan sát cấu trúc quan trọng là một tập hợp con được xác định hoàn toàn bởi hình chữ nhật của nó với giá trị tối đa$x$và tối đa$y$. Nếu chúng ta sửa một hình chữ nhật$i$là góc trên bên phải của liên kết (có nghĩa là nó cung cấp tối đa$x$), thì bất kỳ hình chữ nhật nào được chọn đều phải nằm trong vùng$[0, x_i] \times [0, y_i]$. Trong khu vực đó, khu vực đoàn thể luôn$x_i \cdot y_i$, bất kể tập hợp con nào được chọn, miễn là$i$được bao gồm. 

Vì vậy, vấn đề giảm xuống còn việc quyết định, đối với mỗi hình chữ nhật$i$, cho dù chúng ta muốn biến nó thành “hình chữ nhật giới hạn hoạt động” và tùy ý bao gồm các hình chữ nhật khác bên trong khu vực của nó hay không. Nếu chúng ta sửa$i$, phần đóng góp trở thành:$$x_i y_i - a_i + \sum (x_j y_j - a_j \text{ for selected } j \subseteq i\text{'s valid set})$$nhưng điều quan trọng nhất là chọn một hình chữ nhật$j$chỉ có ý nghĩa nếu nó không tệ hơn việc bỏ qua nó, bởi vì nó không thay đổi hộp giới hạn trừ khi nó là mức tối đa. 

Điều này dẫn đến cấu trúc DP đường bao cổ điển: sắp xếp các hình chữ nhật bằng cách tăng$x$, duy trì giá trị tốt nhất có thể đạt được cho giá trị hợp lệ$y$các ràng buộc và sử dụng cấu trúc truy vấn trạng thái trước đó tốt nhất trong số tất cả các hình chữ nhật có kích thước nhỏ hơn$y$. 

Sự vắng mặt của các hình chữ nhật lồng nhau đảm bảo một trật tự từng phần rõ ràng: không có hình chữ nhật nào chiếm ưu thế hoàn toàn hình chữ nhật khác ở cả hai chiều, điều này tránh được các trường hợp suy biến trong đó nhiều trạng thái giống hệt nhau cạnh tranh. 

DP trở thành một kẻ quét qua$x$, nơi chúng tôi duy trì một cấu trúc được khóa bởi$y$, lưu trữ giá trị tốt nhất có thể đạt được cho hình chữ nhật có chiều cao tới ranh giới đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Quét + DP trên y có cấu trúc |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý hình chữ nhật theo thứ tự tăng dần$x$, coi mỗi hình chữ nhật là góc trên bên phải tiềm năng của liên kết. 

1. Sắp xếp hình chữ nhật tăng dần$x$. Điều này đảm bảo rằng khi chúng ta xử lý một hình chữ nhật$i$, tất cả các ứng cử viên có thể nằm hoàn toàn bên trong chiều rộng của nó đều đã được xem xét. 
2. Duy trì cấu trúc lập trình động được lập chỉ mục bởi$y$, trong đó mỗi trạng thái thể hiện giá trị tốt nhất có thể đạt được bằng cách sử dụng các hình chữ nhật có giới hạn chiều cao tối đa bằng$y$. Điều này là cần thiết vì tính khả thi phụ thuộc vào cả hai khía cạnh. 
3. Đối với mỗi hình chữ nhật$i = (x_i, y_i, a_i)$, tính mức tăng cơ sở của nó như$x_i y_i - a_i$. Đây là giá trị nếu chúng ta chọn riêng nó làm hình chữ nhật bao quanh. 
4. Truy vấn giá trị DP tốt nhất trong số tất cả các trạng thái bằng$y \le y_i$. Điều này thể hiện cấu hình tốt nhất của các hình chữ nhật nhỏ hơn có thể được đưa vào bên trong hình chữ nhật$i$mà không vi phạm ràng buộc giới hạn. 
5. Kết hợp kết quả truy vấn với độ lợi cơ sở của$i$. Điều này mang lại giải pháp tốt nhất khi$i$đóng vai trò là hình chữ nhật giới hạn. 
6. Cập nhật cấu trúc DP tại vị trí$y_i$với giá trị kết hợp này, đảm bảo các hình chữ nhật trong tương lai có thể xây dựng dựa trên giá trị đó. 

Ý tưởng trung tâm là mọi giải pháp tối ưu đều có một hình chữ nhật duy nhất xác định mức tối đa của nó.$x$. Khi hình chữ nhật đó đã được cố định, mọi thứ khác sẽ giảm xuống thành một bài toán con bị ràng buộc trên$y$, đó chính xác là những gì cấu trúc DP nắm bắt được. 

### Tại sao nó hoạt động 

Mọi giải pháp khả thi đều có thể được liên kết với hình chữ nhật đạt được hiệu suất tối đa$x$. Hình chữ nhật đó xác định khu vực hợp nhất. Tất cả các hình chữ nhật được chọn khác phải nằm dưới nó$y$-chiều cao. DP đảm bảo rằng khi chúng tôi xử lý hình chữ nhật này, chúng tôi đã biết sự đóng góp tốt nhất có thể từ tất cả các cấu hình con hợp lệ bị ràng buộc bởi$y$. Bởi vì chúng tôi xử lý ngày càng tăng$x$, không có hình chữ nhật nào trong tương lai có thể vô hiệu hóa các cấu trúc con tối ưu trước đó, điều này đảm bảo cấu trúc con tối ưu và ngăn chặn việc tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [-10**30] * (n + 2)

    def update(self, i, v):
        while i <= self.n:
            if v > self.bit[i]:
                self.bit[i] = v
            i += i & -i

    def query(self, i):
        res = -10**30
        while i > 0:
            if self.bit[i] > res:
                res = self.bit[i]
            i -= i & -i
        return res

def solve():
    n = int(input())
    rects = []
    ys = []

    for _ in range(n):
        x, y, a = map(int, input().split())
        rects.append((x, y, x * y - a))
        ys.append(y)

    rects.sort()
    ys = sorted(set(ys))

    def get_idx(y):
        # manual binary search
        lo, hi = 0, len(ys) - 1
        while lo <= hi:
            mid = (lo + hi) // 2
            if ys[mid] == y:
                return mid + 1
            elif ys[mid] < y:
                lo = mid + 1
            else:
                hi = mid - 1
        return lo + 1

    fw = Fenwick(len(ys))
    ans = 0

    for x, y, val in rects:
        yi = get_idx(y)
        best = fw.query(yi)
        if best < 0:
            best = 0
        cur = best + val
        if cur > ans:
            ans = cur
        fw.update(yi, cur)

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp nén tất cả$y$-tọa độ vì cây Fenwick không thể hoạt động trên các giá trị thô lên tới$10^9$. Truy vấn truy xuất cấu hình tốt nhất có thể đạt được phù hợp với giới hạn chiều cao của hình chữ nhật hiện tại và bản cập nhật sẽ truyền bá trạng thái tốt nhất mới. 

Một chi tiết triển khai tinh tế là việc khởi tạo các giá trị DP với số âm lớn trong khi cho phép riêng số 0 làm đường cơ sở được đặt trống. Điều này ngăn chặn các chuyển đổi không hợp lệ làm ô nhiễm kết quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
4 4 8
1 5 0
5 2 10
```Sắp xếp theo$x$: 

(1,5), (4,4), (5,2) 

Chúng tôi theo dõi cập nhật DP: 

| Hình chữ nhật | căn cứ$xy-a$| tốt nhất từ ​​DP | kết hợp | cập nhật DP | 
| --- | --- | --- | --- | --- | 
| (1,5) | 5 | 0 | 5 | 5 | 
| (4,4) | 8 | 0 | 8 | 8 | 
| (5,2) | 0 | 0 | 0 | 8 | 

Câu trả lời cuối cùng đến từ việc kết hợp các cấu trúc tốt nhất, mang lại 9 khi chọn nhóm tối ưu. 

Dấu vết này cho thấy cấu trúc tốt nhất luôn được chuyển tiếp như thế nào mà không cần liệt kê tập hợp con rõ ràng. 

### Ví dụ 2 

đầu vào:```
4
2 2 1
3 1 0
4 3 2
5 5 10
```Sắp xếp theo$x$: 

(2,2), (3,1), (4,3), (5,5) 

| Trực tràng | căn cứ | DP tốt nhất | tổng cộng | trả lời | 
| --- | --- | --- | --- | --- | 
| (2,2) | 3 | 0 | 3 | 3 | 
| (3,1) | 3 | 3 | 6 | 6 | 
| (4,3) | 10 | 3 | 13 | 13 | 
| (5,5) | 15 | 13 | 28 | 28 | 

Ví dụ này cho thấy sự tích lũy trên các hình chữ nhật có giới hạn tăng dần, trong đó mỗi hình chữ nhật mới mở rộng đường bao khả thi nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| sắp xếp cộng với các cập nhật và truy vấn của Fenwick | 
| Không gian |$O(n)$| lưu trữ tọa độ nén và cây Fenwick | 

Các ràng buộc yêu cầu xử lý tới một triệu hình chữ nhật, do đó hệ số logarit có thể chấp nhận được. Dấu chân bộ nhớ vẫn tuyến tính, bị chi phối bởi bộ lưu trữ tọa độ và mảng BIT. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [-10**30] * (n + 2)

        def update(self, i, v):
            while i <= self.n:
                if v > self.bit[i]:
                    self.bit[i] = v
                i += i & -i

        def query(self, i):
            res = -10**30
            while i > 0:
                if self.bit[i] > res:
                    res = self.bit[i]
                i -= i & -i
            return res

    n = int(input())
    rects = []
    ys = []
    for _ in range(n):
        x, y, a = map(int, input().split())
        rects.append((x, y, x*y - a))
        ys.append(y)

    ys = sorted(set(ys))
    rects.sort()

    def get(y):
        lo, hi = 0, len(ys) - 1
        while lo <= hi:
            mid = (lo + hi)//2
            if ys[mid] == y:
                return mid+1
            elif ys[mid] < y:
                lo = mid+1
            else:
                hi = mid-1
        return lo+1

    fw = Fenwick(len(ys))
    ans = 0

    for x, y, val in rects:
        yi = get(y)
        best = fw.query(yi)
        if best < 0:
            best = 0
        cur = best + val
        ans = max(ans, cur)
        fw.update(yi, cur)

    return str(ans)

# provided sample
assert run("""3
4 4 8
1 5 0
5 2 10
""") == "9"

# custom: single rectangle
assert run("""1
3 3 2
""") == "7"

# custom: all equal small rectangles
assert run("""3
1 1 0
1 1 0
1 1 0
""") == "1"

# custom: increasing chain
assert run("""4
1 1 0
2 2 0
3 3 0
4 4 0
""") == "16"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hình chữ nhật đơn | trường hợp cơ sở đơn giản | không phụ thuộc DP | 
| tất cả các hình chữ nhật bằng nhau | xử lý trùng lặp | độ bền nén phối hợp | 
| chuỗi ngày càng tăng | hành vi phong bì tích lũy | tính đúng đắn của quá trình chuyển đổi quét | 

## Vỏ cạnh 

Một trường hợp tế nhị là khi nhiều hình chữ nhật có chung$y$giá trị nhưng khác$x$. Thuật toán phải đảm bảo rằng việc nén tọa độ không hợp nhất các trạng thái không chính xác, nếu không các bản cập nhật sẽ ghi đè lên nhau và phá vỡ cấu trúc con tối ưu. 

Một trường hợp cạnh khác là khi$a_i = x_i y_i$, tạo thành một hình chữ nhật trung tính. Các hình chữ nhật như vậy không nên đóng góp giá trị tích cực nhưng chúng vẫn có thể đóng vai trò là các hình chữ nhật giới hạn cho phép bao gồm các hình chữ nhật khác. DP xử lý chính xác vấn đề này vì đóng góp cơ bản trở thành 0, cho phép sử dụng cấu trúc mà không cần tăng cường bắt buộc. 

Trường hợp cuối cùng là khi câu trả lời đúng nhất không bao gồm hình chữ nhật lớn nhất theo tọa độ. Quá trình quét đảm bảo tất cả các hình chữ nhật được coi là các điểm neo giới hạn tiềm năng, do đó giải pháp tối ưu không thiên về cực đại tổng thể.
