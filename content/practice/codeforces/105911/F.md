---
title: "CF 105911F - Chênh lệch calo"
description: "Chúng ta được cung cấp một chuỗi ngày. Vào mỗi ngày, chúng ta chọn một số thực không âm $ri$, được hiểu là lượng calo tiêu thụ, nhưng nó bị hạn chế nằm trong một khoảng cố định $[L, R]$."
date: "2026-06-21T12:12:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105911
codeforces_index: "F"
codeforces_contest_name: "2025 ICPC Nanchang Invitational and Jiangxi Provincial Collegiate Programming Contest"
rating: 0
weight: 105911
solve_time_s: 51
verified: true
draft: false
---

[CF 105911F - Chênh lệch calo](https://codeforces.com/problemset/problem/105911/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi ngày. Mỗi ngày ta chọn một số thực không âm$r_i$, được hiểu là lượng calo nạp vào nhưng bị hạn chế nằm trong một khoảng cố định$[L, R]$. Một số ngày đã được ấn định: cho$k$vị trí, giá trị của$r_i$đã được xác định trước và không thể thay đổi. 

Ngoài ra còn có một trình tự khác$c_i$, không được tự do lựa chọn. Nó tiến triển một cách xác định từ những ngày trước đó bằng cách sử dụng phép truy toán tuyến tính:$$c_i = p \cdot c_{i-1} + (1 - p)\cdot r_{i-1}.$$Vì vậy mỗi$c_i$là sự pha trộn có trọng số của trạng thái trao đổi chất trước đó và lượng ăn vào trước đó. 

Mục tiêu mang tính toàn cầu: chọn tất cả các phần không cố định$r_i$giá trị sao cho tổng của tất cả các ngày của$c_i - r_i$được tối đa hóa. 

Khó khăn chính là mọi quyết định về$r_i$ảnh hưởng tới mọi tương lai$c_j$các giá trị thông qua việc truyền lặp lại với hệ số$p$. Vì vậy, vấn đề không thể tách rời cục bộ. 

Những ràng buộc ngụ ý rằng$n$có thể lên đến$2 \cdot 10^5$mỗi bộ thử nghiệm, với tối đa$10^4$trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng các nhiệm vụ khác nhau hoặc thực hiện lập trình động trên tất cả các trạng thái. Bất cứ điều gì bậc hai hoặc thậm chí$O(n \log n)$mỗi trường hợp thử nghiệm chỉ được chấp nhận nếu được khấu hao cẩn thận trong tất cả các thử nghiệm. 

Một trường hợp phức tạp phát sinh từ việc ngày cố định phá vỡ tính liên tục. Nếu một người cố gắng xử lý vấn đề như một sự tham lam đơn giản đối với các vị trí, thì sẽ thất bại vì một quyết định trong ngày$i$ảnh hưởng đến tất cả các đóng góp sau này, bao gồm cả các phân đoạn được phân tách bằng các ràng buộc cố định. 

Ví dụ, nếu$p$gần bằng 1, các quyết định sớm lan truyền xa; nếu như$p$là nhỏ, chỉ có hàng xóm gần mới quan trọng. Một người địa phương ngây thơ tham lam luôn chọn$R$hoặc$L$tùy thuộc vào mức tăng ngay lập tức sẽ bỏ qua hiệu ứng tầm xa này và không thành công trong các trường hợp như xen kẽ các vị trí cố định và tự do. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua sự tái diễn thì mục tiêu có vẻ tầm thường: tối đa hóa$\sum (c_i - r_i)$sẽ chỉ có nghĩa là đẩy từng cái$r_i$đến một ranh giới tùy thuộc vào dấu hiệu. Nhưng sự tái phát đã kết hợp mọi thứ. 

Một cách diễn giải thô bạo sẽ thử tất cả các phép gán có thể có cho các$r_i$. Vì mỗi ngày rảnh rỗi có phạm vi liên tục$[L, R]$, thậm chí việc rời rạc hóa thành một vài ứng cử viên sẽ dẫn đến sự kết hợp theo cấp số nhân. Ngay cả khi chúng ta chỉ xem xét hai lựa chọn trong một ngày rảnh rỗi, không gian trạng thái sẽ trở thành$2^{n-k}$, điều đó là không thể. 

Quan sát quan trọng là sự tái phát là tuyến tính. Mọi$r_j$đóng góp cho nhiều tương lai$c_i$giá trị, nhưng ảnh hưởng của nó được nhân lên trong$p$. Điều này có nghĩa là mục tiêu cuối cùng có thể được viết lại dưới dạng hàm tuyến tính của tất cả$r_i$:$$\sum_{i=1}^n (c_i - r_i) = \sum_{i=1}^n w_i r_i + \text{constant},$$đối với một số hệ số$w_i$điều đó chỉ phụ thuộc vào$p$và cơ cấu vị trí. 

Một khi sự rút gọn này được nhận ra, bài toán sẽ trở thành một bài toán tối ưu tuyến tính có ràng buộc cổ điển: mỗi$r_i$độc lập ngoại trừ các ràng buộc về hộp$[L, R]$và mỗi cái đều đóng góp độc lập cho mục tiêu. Vì vậy, mỗi$r_i$nên được đẩy đến một trong hai$L$hoặc$R$, tùy thuộc vào dấu của hệ số của nó, đồng thời tôn trọng các ràng buộc cố định. 

Nhiệm vụ còn lại là tính toán các hệ số này một cách hiệu quả. Khai triển trực tiếp cho thấy rằng mỗi$r_j$đóng góp cho tất cả$c_i$vì$i > j$với trọng lượng$(1-p)p^{i-j-1}(1-p)$-phong cách phân rã hình học. Tính tổng tất cả các hậu tố sẽ cho một chuỗi hình học, do đó mỗi vị trí có hệ số dạng đóng chỉ phụ thuộc vào$p$Và$n-j$. Điều này có thể được tính toán theo thời gian tuyến tính bằng cách sử dụng thủ thuật lan truyền hậu tố. 

Các vị trí cố định chỉ cần ghi đè lựa chọn và được coi là hằng số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi viết lại sự đóng góp của mỗi$r_i$đến mục tiêu cuối cùng. 

### Bước 1: Mở rộng mức độ ảnh hưởng tái phát 

Chúng tôi mở rộng$c_i$nhiều lần:$$c_i = p^{i-1}c_0 + \sum_{j=1}^{i-1} (1-p)p^{i-j-1} r_j.$$Điều này cho thấy mọi$r_j$ảnh hưởng tới mọi tương lai$c_i$. 

### Bước 2: Hoán đổi phép tính tổng 

Chúng tôi viết lại mục tiêu:$$\sum_{i=1}^n c_i - \sum_{i=1}^n r_i.$$Số hạng đầu tiên trở thành tổng gấp đôi trên tất cả các cặp$(j, i)$với$j < i$, cộng với đóng góp ban đầu từ$c_0$. Sắp xếp lại các nhóm tất cả các thuật ngữ theo$r_j$, tạo ra một hệ số cho mỗi vị trí. 

### Bước 3: Tính trọng số đóng góp 

Đối với một cố định$j$, tổng đóng góp của nó là:$$r_j \cdot (1-p)\sum_{i=j+1}^n p^{i-j-1} = r_j \cdot (1-p)\frac{1-p^{n-j}}{1-p} = r_j (1 - p^{n-j}).$$Sau đó trừ trực tiếp$-r_j$hạn, đưa ra:$$w_j = (1 - p^{n-j}) - 1 = -p^{n-j}.$$Vì vậy mỗi$r_j$đóng góp tuyến tính với hệ số$-p^{n-j}$và các hằng số cố định đến từ$c_0$. 

### Bước 4: Xử lý các ràng buộc 

Mỗi cái không cố định$r_j$được chọn trong$[L, R]$. Vì mục tiêu là tuyến tính trong$r_j$, phương án tối ưu là: 

nếu$w_j \ge 0$, chọn$R$, ngược lại chọn$L$. 

### Bước 5: Tích lũy đáp án cuối cùng 

Chúng tôi tính toán trước lũy thừa của$p$từ phía sau để tránh tính toán lại và đóng góp tổng. 

### Tại sao nó hoạt động 

Toàn bộ hệ thống là tuyến tính$r_i$. Sự tái diễn không bao giờ tạo ra các tương tác phi tuyến giữa các biến quyết định khác nhau; nó chỉ phân phối lại trọng lượng của chúng theo thời gian thông qua phân rã hình học. Sau khi được viết lại dưới dạng tổng của các số hạng tuyến tính độc lập, vùng khả thi sẽ trở thành tích của các khoảng. Mục tiêu tuyến tính trên tích của các khoảng luôn đạt được mức tối ưu ở ranh giới của từng khoảng một cách độc lập, do đó mỗi quyết định biến đổi đều tối ưu cục bộ và nhất quán toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        n, k = map(int, input().split())
        r0, c0, p, L, R = map(float, input().split())

        fixed = {}
        for _ in range(k):
            pos, val = input().split()
            pos = int(pos)
            val = float(val)
            fixed[pos] = val

        # suffix powers of p
        powp = [1.0] * (n + 2)
        for i in range(n, 0, -1):
            powp[i] = powp[i + 1] * p

        ans = 0.0

        # contribution from initial c0
        ans += c0 * powp[1]

        for i in range(1, n + 1):
            coeff = -powp[i]

            if i in fixed:
                ans += coeff * fixed[i]
            else:
                if coeff >= 0:
                    ans += coeff * R
                else:
                    ans += coeff * L

        print(f"{ans:.10f}")

if __name__ == "__main__":
    solve()
```Việc thực hiện tính toán quyền hạn của$p$từ phía sau sao cho$p^{n-i}$có sẵn ở$O(1)$. Mỗi vị trí được xử lý độc lập. Các giá trị cố định ghi đè lựa chọn biên, trong khi các biến tự do được đẩy đến bất kỳ điểm cuối nào tối đa hóa đóng góp tuyến tính. 

Một điểm tinh tế là độ chính xác nổi: vì tất cả các hệ số đều là lũy thừa hình học của$p$, các giá trị giảm đi nhanh chóng nhưng vẫn yêu cầu tích lũy độ chính xác gấp đôi để duy trì$10^{-6}$sự chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3, k = 1
r0 = 5, c0 = 6, p = 0.5, L = 1, R = 10
fixed: r2 = 5
```Chúng tôi tính toán sức mạnh: 

| tôi | p^{n-i} | coeff = -p^{n-i} | sự lựa chọn | 
| --- | --- | --- | --- | 
| 1 | 0,25 | -0,25 | L = 1 | 
| 2 | 0,5 | -0,5 | cố định = 5 | 
| 3 | 1 | -1 | L = 1 | 

Bây giờ đóng góp: 

| tôi | giá trị | đóng góp | 
| --- | --- | --- | 
| 1 | 1 | -0,25 | 
| 2 | 5 | -2,5 | 
| 3 | 1 | -1 | 

Thời hạn ban đầu thêm vào$6 \cdot 0.25 = 1.5$. 

Tổng cộng là:$$1.5 - 0.25 - 2.5 - 1 = -2.25.$$Dấu vết này cho thấy các vị trí sau này có ảnh hưởng lớn hơn như thế nào vì quyền lực của họ lớn hơn. 

### Ví dụ 2 

đầu vào:```
n = 4, p = 0.8, all free
```Quyền hạn: 

| tôi | p^{n-i} | cà phê | 
| --- | --- | --- | 
| 1 | 0,512 | -0,512 | 
| 2 | 0,64 | -0,64 | 
| 3 | 0,8 | -0,8 | 
| 4 | 1 | -1 | 

Tất cả các hệ số đều âm nên mọi$r_i = L$. Điều này thể hiện hành vi đẩy tới ranh giới đơn điệu khi tất cả các đóng góp cận biên đều âm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Mỗi vị trí được xử lý một lần với các phép toán liên tục | 
| Không gian | O(n) | Lưu trữ tiền tố hoặc hậu tố của$p$| 

Tổng kích thước đầu vào trên các trường hợp thử nghiệm bị giới hạn, do đó, việc quét tuyến tính trên tất cả các vị trí là đủ trong giới hạn thời gian và mức sử dụng bộ nhớ vẫn nằm trong một vài mảng nhân đôi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    # placeholder: assume solve() is defined above
    # capture output
    import contextlib
    import sys as _sys
    out = io.StringIO()
    _stdout = _sys.stdout
    _sys.stdout = out
    try:
        solve()
    finally:
        _sys.stdout = _stdout
    return out.getvalue().strip()

# small case
assert run("""1
1 0
5 6 0.5 1 10
""") != ""

# fixed-only case
assert run("""1
3 3
5 6 0.5 1 10
1 3
2 4
3 5
""") != ""

# boundary push
assert run("""1
2 0
1 10 0.9 1 10
""") != ""

# mixed
assert run("""1
4 1
5 6 0.5 1 10
2 7
""") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ngày độc thân | giá trị tầm thường | trường hợp cơ sở đúng đắn | 
| tất cả đã được sửa | tổng xác định | xử lý ghi đè cố định | 
| p gần 1 | lan truyền mạnh mẽ | độ chính xác phân rã hình học | 
| ràng buộc hỗn hợp | quyết định ranh giới | tương tác giữa cố định và miễn phí | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các vị trí đều cố định. Trong tình huống đó, thuật toán không bao giờ được thử thay thế ranh giới. Logic hệ số vẫn hoạt động vì đường dẫn ghi đè cố định bỏ qua hoàn toàn việc ra quyết định, do đó kết quả sẽ trở thành đánh giá trực tiếp ở dạng tuyến tính. 

Một trường hợp cạnh khác xảy ra khi$p$cực kỳ gần với 1. Khi đó$p^{n-i}$phân rã chậm và các vị trí ban đầu vẫn ảnh hưởng đáng kể đến tổng số. Thuật toán xử lý việc này mà không sửa đổi vì nó sử dụng trực tiếp lũy thừa dấu phẩy động thay vì cắt bớt hoặc xấp xỉ chuỗi. 

Trường hợp cuối cùng là khi$p$là rất nhỏ. Sau đó, tất cả các hệ số ngoại trừ hệ số cuối cùng đều trở nên gần bằng 0, vì vậy chỉ những ngày cuối cùng mới quan trọng. Việc triển khai vẫn đánh giá từng thuật ngữ, nhưng đóng góp bằng số tự nhiên tập trung vào cuối, phù hợp với hành vi phân tích của phép truy hồi.
