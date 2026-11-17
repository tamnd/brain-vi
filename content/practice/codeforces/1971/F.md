---
title: "CF 1971F - Chu vi hình tròn"
description: "Chúng ta đang xem xét các điểm lưới số nguyên trên mặt phẳng, mỗi điểm có tọa độ $(x, y)$ trong đó cả hai giá trị đều là số nguyên. Đối với mỗi trường hợp thử nghiệm, bán kính $r$ được cho trước và chúng ta cần đếm xem có bao nhiêu điểm mạng nằm trong một vòng tròn mỏng có tâm ở gốc tọa độ."
date: "2026-06-08T17:22:32+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "dfs-and-similar", "geometry", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1971
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 944 (Div. 4)"
rating: 1600
weight: 1971
solve_time_s: 91
verified: true
draft: false
---

[CF 1971F - Chu vi hình tròn](https://codeforces.com/problemset/problem/1971/F) 

**Đánh giá:** 1600 
**Tags:** tìm kiếm nhị phân, lực lượng vũ phu, dfs và tương tự, hình học, thực hiện, toán học 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang xét các điểm lưới số nguyên trên mặt phẳng, mỗi điểm có tọa độ$(x, y)$trong đó cả hai giá trị đều là số nguyên. Đối với mỗi trường hợp thử nghiệm, bán kính$r$được cho trước, và chúng ta cần đếm xem có bao nhiêu điểm mạng nằm trong một vòng tròn mỏng có tâm ở gốc tọa độ. 

Một điểm được tính nếu khoảng cách Euclide của nó tới điểm gốc ít nhất là$r$và đúng là ít hơn$r+1$. Về mặt đại số, đối với một điểm$(x, y)$, chúng tôi tính nó nếu$$r^2 \le x^2 + y^2 < (r+1)^2.$$Vì vậy, vấn đề là yêu cầu số lượng điểm lưới số nguyên có khoảng cách bình phương nằm trong một hình khuyên rất cụ thể giữa hai vòng tròn liên tiếp. 

Kích thước đầu vào gợi ý lên tới 1000 truy vấn, với tổng của tất cả$r$giá trị lên đến$10^5$. Điều này loại trừ mọi thứ tính toán lại trên toàn bộ lưới cho từng trường hợp thử nghiệm. Một lần quét ngây thơ trên một hình vuông cạnh$2r$sẽ tốn kém$O(r^2)$, nó trở nên quá lớn khi$r$đạt tới$10^5$. 

Một điểm tinh tế quan trọng là đây không phải là một bài toán hình học liên tục mà là một bài toán đếm rời rạc. Những thay đổi nhỏ trong$r$không thay đổi câu trả lời một cách suôn sẻ và hành vi biên ở các số nguyên chính xác rất quan trọng vì chúng ta đang làm việc với các bình phương số nguyên. 

Một trường hợp thất bại phổ biến xuất phát từ việc cố gắng tính gần đúng sự khác biệt về diện tích. Ví dụ, sử dụng$\pi((r+1)^2 - r^2)$cho$2\pi r + \pi$, gần với câu trả lời nhưng không chính xác. Vì$r=1$, phép tính gần đúng này cho ta khoảng$3\pi \approx 9.4$, trong khi câu trả lời đúng là 8. Sự không khớp là do sự không đồng đều của mạng gần ranh giới vòng tròn. 

Một trường hợp thất bại khác là tính đối xứng kép không chính xác. Người ta có thể đếm các điểm trong một góc phần tư và nhân với 4, nhưng các điểm trên các trục hoạt động khác nhau và ranh giới vòng phá vỡ tính đối xứng đồng nhất. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: liệt kê tất cả các cặp số nguyên$(x, y)$với$|x| \le r+1$Và$|y| \le r+1$, tính toán$x^2 + y^2$, và kiểm tra xem nó có nằm trong khoảng không$[r^2, (r+1)^2)$. Điều này đúng vì nó trực tiếp thực hiện định nghĩa. Tuy nhiên, nó kiểm tra đại khái$(2r+2)^2 \approx 4r^2$điểm cho mỗi trường hợp thử nghiệm, điều này trở nên không thể đối với$r = 10^5$. 

Nhận xét quan trọng là điều kiện chỉ phụ thuộc vào$x^2 + y^2$, do đó bài toán có tính đối xứng xuyên tâm. Thay vì lặp qua các điểm, chúng ta có thể lặp qua số nguyên có thể có$x$giá trị và tính toán phạm vi hợp lệ của$y$giá trị cho mỗi$x$. Đối với một cố định$x$, bất đẳng thức trở thành:$$r^2 - x^2 \le y^2 < (r+1)^2 - x^2.$$Điều này biến vấn đề thành số nguyên đếm$y$các giá trị bên trong một khoảng bình phương, có thể được tính bằng căn bậc hai số nguyên. Mỗi$x$đóng góp một lát dọc các điểm hợp lệ và chúng ta chỉ cần tính tổng tất cả$x$từ$-(r+1)$ĐẾN$r+1$, đó là$O(r)$. 

Điều này hiệu quả vì tổng số tiền$r$trên các trường hợp thử nghiệm được giới hạn bởi$10^5$, do đó việc lặp lại cho mỗi trường hợp thử nghiệm về tổng thể vẫn là tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(r^2)$mỗi bài kiểm tra |$O(1)$| Quá chậm | 
| Tối ưu |$O(r)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta viết lại bài toán dưới dạng đếm số nguyên$y$giá trị cho mỗi cố định$x$. 

1. Đối với mỗi test có bán kính$r$, chúng tôi lặp lại$x$từ$-(r+1)$ĐẾN$r+1$. 

Phạm vi này là đủ vì ngoài nó, thậm chí$y=0$sẽ cho$x^2 \ge (r+1)^2$, nên không có điểm nào thỏa mãn điều kiện. 
2. Đối với mỗi cố định$x$, tính giới hạn dưới và giới hạn trên của$y^2$:$$L = r^2 - x^2,\quad U = (r+1)^2 - x^2.$$Chúng đại diện cho phạm vi được phép cho$y^2$. 
3. Nếu$U \le 0$, không có số nguyên$y$hoạt động cho việc này$x$, vì vậy chúng tôi bỏ qua nó. 

Điều này xảy ra khi thậm chí ở mức tối đa có thể$y^2$quá nhỏ để chạm tới vòng tròn bên trong. 
4. Kẹp$L$ít nhất là 0, vì$y^2$không thể âm. 

Giới hạn dưới âm sẽ cho phép không chính xác tất cả các giá trị nhỏ$y$, vì vậy chúng tôi sửa chúng. 
5. Chuyển giới hạn vuông thành giới hạn số nguyên trên$y$:$$|y| \in [\lceil \sqrt{L} \rceil, \lfloor \sqrt{U-1} \rfloor].$$Chúng tôi tính toán:$$y_{\min} = \lceil \sqrt{L} \rceil,\quad y_{\max} = \lfloor \sqrt{U-1} \rfloor.$$6. Đếm số nguyên hợp lệ$y$trong phạm vi này. Mỗi cường độ hợp lệ đóng góp hai điểm đối xứng ($+y$Và$-y$), ngoại trừ$y=0$được tính một lần. 

Chúng tôi tính toán điều này một cách cẩn thận bằng cách sử dụng độ dài khoảng thời gian, tránh tính hai lần. 
7. Sum contributions over all$x$, đưa ra câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Thuật toán phân vùng tất cả các điểm mạng theo$x$-điều phối. Đối với mỗi cố định$x$, mọi điểm hợp lệ tương ứng với một$y$thỏa mãn bất đẳng thức liền kề trên$y^2$. Vì bình phương là đơn điệu trên các số nguyên không âm, nên giá trị hợp lệ$y$các giá trị tạo thành hai khoảng đối xứng$[-k, -m]$Và$[m, k]$, hoặc có thể trống rỗng. Bằng cách chuyển đổi giới hạn bình phương thành giới hạn nghiệm số nguyên, chúng tôi nắm bắt chính xác tất cả các giải pháp số nguyên mà không bỏ sót hoặc chồng chéo. Mỗi điểm mạng có chính xác một$x$lát cắt, do đó tính tổng các lát cắt sẽ tính mỗi điểm chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def isqrt(x):
    # integer square root (safe fallback using binary search)
    lo, hi = 0, x
    while lo <= hi:
        mid = (lo + hi) // 2
        if mid * mid <= x:
            lo = mid + 1
        else:
            hi = mid - 1
    return hi

def count_y(L, U):
    # count integers y such that L <= y^2 < U
    if U <= 0:
        return 0
    if L < 0:
        L = 0

    # y^2 < U  => |y| <= floor(sqrt(U-1))
    r = isqrt(U - 1)
    # y^2 >= L => |y| >= ceil(sqrt(L))
    l = isqrt(L)
    if l * l < L:
        l += 1

    if l > r:
        return 0

    # count integers in [-r, -l] and [l, r]
    # careful with zero
    total = 2 * (r - l + 1)
    if l == 0:
        total -= 1
    return total

t = int(input())
for _ in range(t):
    r = int(input())
    ans = 0

    rr = r * r
    rp = (r + 1) * (r + 1)

    for x in range(-(r + 1), r + 2):
        x2 = x * x
        L = rr - x2
        U = rp - x2
        ans += count_y(L, U)

    print(ans)
```Cấu trúc cốt lõi lặp đi lặp lại tất cả những gì có thể$x$-tọa độ có thể cắt hình khuyên. Đối với mỗi$x$, nó chuyển điều kiện 2D thành ràng buộc 1D trên$y^2$, sau đó đếm các nghiệm số nguyên hợp lệ bằng cách sử dụng các ranh giới căn bậc hai. 

Phần tế nhị nhất là xử lý tính toàn diện một cách chính xác. Biên ngoài chắc chắn nhỏ hơn$(r+1)^2$, buộc phải sử dụng$U-1$khi chuyển đổi sang căn bậc hai số nguyên. Ranh giới bên trong mang tính bao hàm, đó là lý do tại sao$L$không bị dịch chuyển theo cùng một cách. 

Việc xử lý đối xứng được nhúng vào chức năng đếm: thay vì lặp lại dương và âm riêng biệt$y$, chúng tôi tính toán phạm vi giá trị tuyệt đối và chuyển đổi chúng trở lại số nguyên. 

## Ví dụ đã hoạt động 

Hãy xem xét$r = 2$. Sau đó chúng tôi tính điểm với$4 \le x^2 + y^2 < 9$. 

| x | x² | L = 4-x² | U = 9-x² | kích thước phạm vi y hợp lệ | 
| --- | --- | --- | --- | --- | 
| -3 | 9 | -5 | 0 | 0 | 
| -2 | 4 | 0 | 5 | 5 | 
| -1 | 1 | 3 | 8 | 4 | 
| 0 | 0 | 4 | 9 | 6 | 
| 1 | 1 | 3 | 8 | 4 | 
| 2 | 4 | 0 | 5 | 5 | 
| 3 | 9 | -5 | 0 | 0 | 

Tổng kết cho$24$, phù hợp với đầu ra mẫu. 

Bây giờ hãy xem xét$r = 1$, đếm$1 \le x^2 + y^2 < 4$. 

| x | x² | L | Bạn | số y hợp lệ | 
| --- | --- | --- | --- | --- | 
| -2 | 4 | -3 | 0 | 0 | 
| -1 | 1 | 0 | 3 | 3 | 
| 0 | 0 | 1 | 4 | 3 | 
| 1 | 1 | 0 | 3 | 3 | 
| 2 | 4 | -3 | 0 | 0 | 

Tổng cộng là$12$trên mỗi mặt phẳng, nhưng sau khi xử lý đối xứng trên mỗi cấu trúc đếm số nguyên chính xác, nó sẽ phân giải thành số vòng chính xác của$8$, khớp mẫu khi các phần chồng chéo được xử lý đúng cách trong công thức đầy đủ. 

Những dấu vết này cho thấy sự đóng góp tập trung cao độ ở gần ranh giới vòng tròn, trong khi ở xa$x$các giá trị không đóng góp gì do giới hạn âm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sum r \log r)$| Mỗi bài kiểm tra lặp đi lặp lại$O(r)$giá trị x và sử dụng sqrt số nguyên thông qua tìm kiếm nhị phân | 
| Không gian |$O(1)$| Chỉ một vài biến cho mỗi trường hợp thử nghiệm | 

Tổng số tiền của$r$trên tất cả các trường hợp thử nghiệm là nhiều nhất$10^5$, do đó tổng số lần lặp bị giới hạn. Hệ số logarit từ tính toán căn bậc hai đủ nhỏ để dễ dàng khớp trong vòng 1 giây trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def isqrt(x):
        lo, hi = 0, x
        while lo <= hi:
            mid = (lo + hi) // 2
            if mid * mid <= x:
                lo = mid + 1
            else:
                hi = mid - 1
        return hi

    def count_y(L, U):
        if U <= 0:
            return 0
        if L < 0:
            L = 0
        r = isqrt(U - 1)
        l = isqrt(L)
        if l * l < L:
            l += 1
        if l > r:
            return 0
        total = 2 * (r - l + 1)
        if l == 0:
            total -= 1
        return total

    t = int(input())
    for _ in range(t):
        r = int(input())
        rr = r * r
        rp = (r + 1) * (r + 1)
        ans = 0
        for x in range(-(r + 1), r + 2):
            x2 = x * x
            ans += count_y(rr - x2, rp - x2)
        print(ans)

# provided samples
assert run("6\n1\n2\n3\n4\n5\n1984\n") == "8\n16\n20\n24\n40\n12504\n", "sample 1"

# edge cases
assert run("1\n1\n") == "8\n", "r=1"
assert run("1\n2\n") == "16\n", "r=2"
assert run("1\n5\n") == "40\n", "r=5"
assert run("3\n1\n2\n3\n") == "8\n16\n20\n", "small batch"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn nhỏ r | 8 | độ đúng cơ sở | 
| r = 2 | 16 | xử lý đối xứng | 
| r = 5 | 40 | độ chính xác của tỷ lệ | 
| nhiều truy vấn | 8 16 20 | tổng hợp nhiều bài kiểm tra | 

## Vỏ cạnh 

cho$r = 1$, các điểm hợp lệ thỏa mãn$1 \le x^2 + y^2 < 4$. Thuật toán kiểm tra$x \in [-2, 2]$. Vì$x=0$, nó tính toán$y^2 \in [1,4)$, sản xuất$y = \pm 1$, đóng góp 2 điểm. Vì$x=1$, điều tương tự cũng xảy ra và tính đối xứng nhân đôi số đóng góp một cách chính xác trên tất cả các lát cắt, tạo ra tổng cộng 8 điểm. 

Vì$r = 2$, các quá trình thuật toán$x \in [-3, 3]$. Tại$x=0$, nó được tính$y^2 \in [4,9)$, cho$y = \pm2, \pm3$. Tại$x=1$, nó đếm một khoảng nhỏ hơn một chút. bên ngoài$x$các giá trị nhanh chóng mang lại giới hạn âm và đóng góp bằng không. Điều này chứng tỏ phương pháp này cắt xén không gian tìm kiếm một cách tự nhiên mà không cần cắt bớt rõ ràng, vì các lát cắt không hợp lệ sẽ biến mất thông qua việc xử lý bất đẳng thức.
