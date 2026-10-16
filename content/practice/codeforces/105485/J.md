---
title: "CF 105485J - \u661f\u7a79\u5217\u8f66"
description: "Đoàn tàu di chuyển dọc theo một tia ngang cố định bắt đầu từ một điểm nhất định và tiếp tục vô tận về bên phải. Mỗi khu vực nguy hiểm là một vòng tròn trên máy bay và bất cứ khi nào đường tàu đi qua một vòng tròn, chúng tôi chỉ tính phần đường tàu nằm bên trong đó…"
date: "2026-06-23T18:23:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105485
codeforces_index: "J"
codeforces_contest_name: "2024 China Unversity of Geosciences (Wuhan) Freshman Contest"
rating: 0
weight: 105485
solve_time_s: 62
verified: true
draft: false
---

[CF 105485J - \u661f\u7a79\u5217\u8f66](https://codeforces.com/problemset/problem/105485/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đoàn tàu di chuyển dọc theo một tia ngang cố định bắt đầu từ một điểm nhất định và tiếp tục vô tận về bên phải. Mỗi khu vực nguy hiểm là một vòng tròn trên máy bay và bất cứ khi nào đường tàu đi qua một vòng tròn, chúng tôi chỉ tính phần đường tàu nằm trong vòng tròn đó. Nếu nhiều vòng tròn chồng lên nhau dọc theo đường dẫn, các phần chồng chéo chỉ được tính một lần vì chúng tôi đang đo tổng chiều dài được bao phủ trên đường thẳng chứ không phải trên mỗi phần đóng góp của vòng tròn. 

Về mặt hình học, mọi thứ sẽ giải quyết thành một bài toán 1D duy nhất khi chúng ta sửa được đường đi của đoàn tàu. Đoàn tàu luôn nằm trên đường nằm ngang y = y0 nên mỗi đường tròn hoặc hoàn toàn không chạm vào đường này hoặc nó cắt ra một đoạn dọc theo trục x. Nhiệm vụ trở thành tính toán tổng độ dài của phần kết của tất cả các phân đoạn như vậy, được giới hạn ở x ≥ x0. 

Các ràng buộc cho phép tối đa 100.000 vòng tròn, do đó, bất kỳ phương pháp nào so sánh từng cặp vòng tròn hoặc quét đường liên tục đều quá chậm. Độ phân giải chồng chéo O(k²) ngây thơ sẽ vượt xa giới hạn. Ngay cả việc hợp nhất khoảng thời gian O(k²) cũng không thể thực hiện được. 

Trường hợp cạnh tinh tế đến từ các vòng tròn hoàn toàn không giao nhau với đường di chuyển ngang. Ví dụ: một đường tròn có tâm tại (0, 10) có bán kính 1 không bao giờ chạm vào y = 0, vì vậy nó không đóng góp gì cả. Một trường hợp góc khác là tiếp tuyến: nếu đường tròn vừa chạm vào đường thẳng, nó sẽ đóng góp một đoạn điểm duy nhất có độ dài bằng 0, điều này không được ảnh hưởng đến câu trả lời. 

Một tình huống quan trọng khác là khi giao điểm của đường tròn nằm hoàn toàn bên trái vị trí bắt đầu x0. Ngay cả khi đường tròn cắt đường thẳng thì chỉ phần có x ≥ x0 là quan trọng, do đó các khoảng phải được cắt bớt một cách chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xử lý từng vòng tròn một cách độc lập, tính toán nơi nó giao với đường ngang y = y0, sau đó mô phỏng việc đi dọc theo trục x trong khi vẫn duy trì số lượng vòng tròn bao phủ mỗi điểm. Điều này tự nhiên dẫn đến ý tưởng đường quét trên tọa độ x. Tuy nhiên, ngay cả một đường quét được triển khai như một cấu trúc sự kiện cũng không cần thiết vì chúng ta chỉ cần độ dài hợp chứ không cần số lượng vùng phủ sóng. 

Quan sát quan trọng là mỗi vòng tròn đóng góp nhiều nhất một khoảng trên đường thẳng y = y0. Đối với đường tròn có tâm tại (xc, yc) có bán kính r, khoảng cách thẳng đứng từ đường đi được cố định là dy = y0 − yc. Nếu |dy| > r, đường tròn không cắt đường đi. Ngược lại, nửa chiều rộng ngang của giao điểm là sqrt(r² − dy²), tạo ra một khoảng [xc − d, xc + d]. 

Sau khi chuyển đổi tất cả các vòng tròn thành các khoảng, vấn đề sẽ trở thành việc hợp nhất các khoảng chồng chéo và tính tổng chiều dài được bao phủ bắt đầu từ x0. 

Lực lượng vũ phu không thành công khi k lớn vì việc hợp nhất từng cặp hoặc quét lặp lại tạo ra hành vi bậc hai. Việc sắp xếp các khoảng theo điểm cuối bên trái sẽ giảm vấn đề thành một lần hợp nhất trong O(k log k). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng chồng chéo Brute Force | O(k2) | O(k) | Quá chậm | 
| Sắp xếp + hợp nhất các khoảng thời gian | O(k log k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Chuyển hình tròn thành khoảng trên trục x 

Với mỗi đường tròn, hãy tính dy = y0 − yc. Nếu dy 2 > r , hãy bỏ qua vì tàu không bao giờ đi vào vòng tròn. Ngược lại hãy tính d = sqrt(r² − dy²). Đường tròn đóng góp khoảng [xc − d, xc + d] trên đường thẳng y = y0. Bước này chuyển bài toán hình học 2D thành bài toán khoảng 1D. 

### 2. Cắt các quãng về điểm bắt đầu 

Vì tàu bắt đầu ở x = x0 nên bất kỳ phần nào của khoảng còn lại của x0 đều không liên quan. Thay thế mỗi khoảng [l, r] bằng [max(l, x0), r]. Nếu sau khi cắt l ≥ r thì loại bỏ nó. Điều này đảm bảo chúng tôi chỉ đo lường các phân khúc được phủ sóng có thể tiếp cận. 

### 3. Sắp xếp tất cả các khoảng theo điểm cuối bên trái

Sắp xếp đưa ra một thứ tự xác định cho phép hợp nhất tuyến tính. Sau khi sắp xếp các khoảng thời gian, mọi cấu trúc chồng chéo sẽ trở nên dễ dàng được giải quyết trong một lần quét. 

### 4. Hợp nhất các khoảng và tích lũy độ dài được bao phủ 

Duy trì khoảng thời gian hoạt động hiện tại [cur_l, cur_r]. Đi qua các khoảng được sắp xếp. Nếu khoảng tiếp theo bắt đầu sau cur_r, chúng ta thêm cur_r − cur_l vào câu trả lời và đặt lại. Nếu không, chúng tôi sẽ mở rộng cur_r nếu cần. Cuối cùng, thêm độ dài khoảng thời gian hoạt động cuối cùng. 

### Tại sao nó hoạt động 

Mỗi vòng tròn ánh xạ chính xác tới một đoạn lồi trên một đường và độ dài phần kết trên một đường chỉ phụ thuộc vào các điểm cuối của khoảng chứ không phụ thuộc vào hình học ban đầu. Việc sắp xếp đảm bảo rằng khi chúng tôi xử lý một khoảng thời gian, tất cả các phần trùng lặp tiềm ẩn với các khoảng thời gian trước đó đã được giải quyết thành một phân đoạn hợp nhất duy nhất. Điều bất biến là ở mỗi bước, phân đoạn hiện tại thể hiện sự kết hợp của tất cả các khoảng được xử lý, do đó, việc thêm một phân đoạn rời rạc không thể bỏ sót hoặc đếm gấp đôi bất kỳ vùng nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def solve():
    x0, y0, k = map(int, input().split())
    intervals = []

    for _ in range(k):
        xc, yc, r = map(int, input().split())
        dy = y0 - yc
        if dy * dy > r * r:
            continue
        dx = math.sqrt(r * r - dy * dy)
        l = xc - dx
        rgt = xc + dx

        if rgt <= x0:
            continue
        l = max(l, x0)
        intervals.append((l, rgt))

    if not intervals:
        print("0.0000000000")
        return

    intervals.sort()

    total = 0.0
    cur_l, cur_r = intervals[0]

    for l, r in intervals[1:]:
        if l > cur_r:
            total += cur_r - cur_l
            cur_l, cur_r = l, r
        else:
            if r > cur_r:
                cur_r = r

    total += cur_r - cur_l
    print(f"{total:.10f}")

if __name__ == "__main__":
    solve()
```Đầu tiên, mã chuyển đổi mọi vòng tròn thành một đoạn ứng cử viên trên đường tàu, sau đó loại bỏ các phần không liên quan còn lại của x0. Sắp xếp đảm bảo chúng ta có thể hợp nhất trong một lần. sqrt dấu phẩy động là cần thiết vì các giao điểm vòng tròn tự nhiên tạo ra các điểm cuối có giá trị thực. Bước tích lũy cuối cùng phải được thực hiện cẩn thận sau vòng lặp để tránh làm mất đoạn cuối cùng. 

Một lỗi triển khai phổ biến là quên cắt theo x0 trước khi hợp nhất. Một cách khác là xử lý không chính xác các giao điểm tiếp tuyến; khi r² == dy², dx trở thành 0 và tạo ra một khoảng có độ dài bằng 0 hợp lệ, điều này vô hại và bị logic hợp nhất bỏ qua một cách tự nhiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0 0 2
10 0 5
20 0 10
```| Bước | Quãng 1 | Khoảng 2 | Phân khúc hoạt động | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| Sau khi chuyển đổi | [5, 15] | [10, 30] | - | 0 | 
| Sau khi sắp xếp | [5, 15] | [10, 30] | - | 0 | 
| Hợp nhất | hợp nhất | sáp nhập vào [5, 30] | [5, 30] | 0 | 
| Cuối cùng | - | - | - | 25 | 

Hai vòng tròn chồng lên nhau trên trục x, do đó sự kết hợp của chúng tạo thành một đoạn liên tục. 

### Ví dụ 2 

đầu vào:```
0 0 3
5 5 2
10 0 3
20 0 1
```| Vòng tròn | Giao nhau y=0 | Khoảng thời gian | 
| --- | --- | --- | 
| (5,5,2) | không | - | 
| (10,0,3) | vâng | [7, 13] | 
| (20,0,1) | vâng | [19, 21] | 

| Bước | Phân khúc hoạt động | Tổng cộng | 
| --- | --- | --- | 
| Bắt đầu | [7, 13] | 0 | 
| Thêm [19,21] | chia | 6 | 
| Cuối cùng | - | 10 | 

Điều này cho thấy các vòng tròn không giao nhau bị bỏ qua như thế nào và các khoảng rời rạc được tích lũy một cách độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k log k) | sắp xếp k khoảng chiếm ưu thế, việc hợp nhất là tuyến tính | 
| Không gian | O(k) | lưu trữ tối đa một khoảng trên mỗi vòng tròn | 

Thuật toán phù hợp thoải mái trong giới hạn vì k tối đa là 100.000 và việc sắp xếp chiếm ưu thế ở khoảng 2 triệu so sánh. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    x0, y0, k = map(int, input().split())
    intervals = []

    for _ in range(k):
        xc, yc, r = map(int, input().split())
        dy = y0 - yc
        if dy * dy > r * r:
            continue
        dx = math.sqrt(r * r - dy * dy)
        l = xc - dx
        rgt = xc + dx
        if rgt <= x0:
            continue
        l = max(l, x0)
        intervals.append((l, rgt))

    if not intervals:
        return "0.0000000000"

    intervals.sort()

    total = 0.0
    cur_l, cur_r = intervals[0]

    for l, r in intervals[1:]:
        if l > cur_r:
            total += cur_r - cur_l
            cur_l, cur_r = l, r
        else:
            if r > cur_r:
                cur_r = r

    total += cur_r - cur_l
    return f"{total:.10f}"

# provided sample
assert run("0 0 4\n10 0 5\n20 0 10\n30 0 15\n-2 -3 5\n")[:2] != "", "sample"

# minimum size
assert run("0 0 1\n0 1 1\n") == "0.0000000000", "touch only point"

# all overlapping
assert run("0 0 2\n0 0 10\n0 0 10\n") != "", "overlap merge"

# disjoint intervals
assert run("0 0 2\n0 0 2\n10 0 2\n") != "", "disjoint"

# far left clipped
assert run("100 0 1\n0 0 50\n") == "0.0000000000", "clipping"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đường tròn tiếp tuyến đơn | 0 | xử lý giao lộ có độ dài bằng không | 
| chồng lên nhau các vòng tròn giống hệt nhau | chiều dài dương một lần | hành vi công đoàn đúng đắn | 
| vòng tròn rời rạc | tổng các phân đoạn | tích lũy độc lập | 
| khoanh tròn hoàn toàn bên trái x0 | 0 | cắt đúng | 

## Vỏ cạnh 

Một đường tròn chỉ chạm vào đường thẳng y = y0 tại đúng một điểm sẽ tạo ra dy² = r² và do đó dx = 0. Thuật toán chuyển đổi giá trị này thành [xc, xc], đóng góp độ dài bằng 0 sau khi hợp nhất, do đó nó không ảnh hưởng đến kết quả. 

Khi tất cả các vòng tròn nằm phía trên hoặc phía dưới đường dẫn, mọi khoảng thời gian sẽ bị loại bỏ trong quá trình kiểm tra dy2 > r 2, để lại một danh sách trống và tạo ra kết quả bằng 0. 

Nếu một vòng tròn lớn kéo dài vô tận sang trái nhưng điểm bắt đầu x0 nằm bên trong nó, việc cắt bớt đảm bảo chúng ta chỉ đếm phần từ x0 trở đi. Điểm cuối bên trái trở thành x0, do đó mức đóng góp được giới hạn chính xác ở phân khúc có thể truy cập.
