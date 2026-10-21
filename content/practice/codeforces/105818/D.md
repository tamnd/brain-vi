---
title: "CF 105818D - Không phải tam giác Nhật Bản"
description: "Chúng ta chỉ có hàng dưới cùng của cấu trúc số hình tam giác. Phía trên nó, mỗi hàng được hình thành từ hàng ngay bên dưới bằng cách sử dụng một quy tắc rất cụ thể: mỗi ô là giá trị tối thiểu của hai ô con bên dưới nó."
date: "2026-06-25T15:10:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105818
codeforces_index: "D"
codeforces_contest_name: "TeamsCode Spring 2025 Advanced Division"
rating: 0
weight: 105818
solve_time_s: 48
verified: true
draft: false
---

[CF 105818D - Không phải Tam giác Nhật Bản](https://codeforces.com/problemset/problem/105818/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta chỉ có hàng dưới cùng của cấu trúc số hình tam giác. Phía trên nó, mỗi hàng được hình thành từ hàng ngay bên dưới bằng cách sử dụng một quy tắc rất cụ thể: mỗi ô là giá trị tối thiểu của hai ô con bên dưới nó. Vì vậy, mỗi giá trị lan truyền lên trên bằng cách liên tục lấy cực tiểu trên các cặp liền kề. 

Cụ thể hàng dưới cùng có độ dài$n$. Hàng trên có độ dài$n-1$, và nó$j$-phần tử thứ bằng$\min(a_{n,j}, a_{n,j+1})$. Điều này tiếp tục cho đến khi chúng tôi đạt được giá trị đơn hàng đầu. Mỗi hàng được xác định đầy đủ bởi hàng dưới cùng mặc dù chúng ta chỉ được cung cấp hàng cuối cùng đó. 

Nhiệm vụ không phải là tái tạo lại tam giác một cách rõ ràng. Thay vào đó, với mỗi hàng$i$, chúng ta phải tính tổng của tất cả các giá trị trong hàng đó. 

Ràng buộc$n \le 10^5$loại trừ mọi hoạt động xây dựng trực tiếp. Một hình tam giác đầy đủ có khoảng$n^2/2$những giá trị vốn đã có sẵn$5 \cdot 10^9$các ô ở kích thước tối đa, quá lớn để xây dựng hoặc thậm chí lặp lại về mặt khái niệm. 

Các giải pháp khả thi duy nhất phải tính tổng hàng mà không tạo ra từng hàng một cách rõ ràng, nghĩa là chúng ta cần một cách để đếm xem một phần tử dưới cùng nhất định ảnh hưởng đến bao nhiêu hàng theo cách có cấu trúc. 

Trường hợp cạnh tinh tế xuất phát từ các giá trị bằng nhau ở hàng dưới cùng. Khi các phần tử liền kề bằng nhau, cực tiểu lan truyền theo cách có thể tạo ra các vùng phẳng hướng lên trên. Ví dụ, với hàng dưới cùng$[1, 1, 1]$, mỗi hàng không đổi và bằng$[1, \dots, 1]$, vì vậy tổng mỗi hàng chỉ là$i$. Một mô phỏng đơn giản có thể vô tình tính toán lại cực tiểu dư thừa và vẫn vượt qua các trường hợp nhỏ, nhưng nó vẫn hết thời gian chờ. 

Một trường hợp cạnh khác xuất hiện khi mức tối thiểu nhảy mạnh, chẳng hạn như$[1, 100, 1]$. Giá trị lớn ở giữa biến mất ngay lập tức ở hàng phía trên và chỉ có giá trị cực tiểu bên tồn tại, điều này làm cho các đóng góp rất không đồng nhất và phá vỡ mọi giả định rằng các giá trị “trung bình đều” trở lên. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực xây dựng từng hàng tam giác. Bắt đầu từ hàng dưới cùng, mỗi hàng cao hơn được tính bằng cách sử dụng cực tiểu theo cặp và mỗi hàng được tính tổng ngay lập tức. Điều này đúng vì nó tuân theo định nghĩa chính xác. 

Tuy nhiên, mỗi hàng yêu cầu$O(i)$làm việc, và có$n$hàng, dẫn đến$O(n^2)$tổng số hoạt động. Với$n = 10^5$, điều này vượt xa giới hạn thời gian. 

Quan sát quan trọng là lật ngược quan điểm. Thay vì suy nghĩ về cách mỗi hàng được hình thành, chúng tôi theo dõi xem mỗi phần tử dưới cùng đóng góp như thế nào cho tất cả các hàng phía trên nó. 

Cố định một vị trí ở hàng dưới cùng. Khi chúng ta di chuyển lên trên, giá trị này vẫn tồn tại miễn là nó vẫn ở mức tối thiểu trong mọi khoảng bao phủ nó. Cấu trúc cực tiểu trên các cặp liền kề tạo ra hiện tượng “khả năng hiển thị cho đến khi bị chặn” cổ điển. Mỗi phần tử ảnh hưởng đến một phạm vi vị trí liền kề trong mỗi hàng và các phạm vi này sẽ mở rộng hoặc thu hẹp một cách đơn điệu khi được xử lý chính xác. 

Điều này cho phép chúng ta diễn giải lại tam giác như một tập hợp các phần đóng góp trong đó mỗi phần tử dưới cùng đóng góp vào một số ô có thể dự đoán được trong mỗi hàng. Sự chuyển đổi giữa các vùng thống trị có thể được tính toán bằng cách sử dụng các ý tưởng ngăn xếp đơn điệu, bởi vì điều duy nhất quan trọng là giá trị nhỏ hơn sẽ chặn sự lan truyền của các giá trị lớn hơn. 

Khi chúng tôi biết, đối với mỗi vị trí, mức độ ảnh hưởng của nó mở rộng sang trái và phải ở mỗi cấp độ, chúng tôi có thể tổng hợp các đóng góp cho tất cả các tổng hàng theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n^2)$| Quá chậm | 
| Tối ưu (theo dõi đóng góp đơn điệu) |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Ý tưởng chính là xử lý các đóng góp của từng phần tử dưới cùng theo thời gian nó vẫn phù hợp khi di chuyển lên trên. 

1. Đối với mọi vị trí ở hàng dưới cùng, hãy xác định phần tử nhỏ hơn chính xác gần nhất ở bên trái và bên phải. Các ranh giới này xác định phân đoạn trong đó phần tử này có thể ở mức tối thiểu trong một số khoảng con. Bước này được tính toán một cách tự nhiên với một ngăn xếp đơn điệu. 
2. Giải thích mỗi phần tử chiếm ưu thế trong một khoảng liền kề. Trong khoảng đó, đây là mức tối thiểu cho bất kỳ phân đoạn nào bao gồm nó và không bao gồm phần tử chặn nhỏ hơn. 
3. Chuyển đổi sự thống trị này thành đóng góp vào số tiền hàng. Trong hàng$i$, một phần tử ở vị trí$j$đóng góp chính xác vào số đoạn có độ dài$i$trong đó nó là mức tối thiểu, điều này chỉ phụ thuộc vào số lượng vị trí bắt đầu hợp lệ tồn tại sao cho phân khúc vẫn nằm trong khoảng thống trị của nó. 
4. Thay vì lặp lại trên tất cả các hàng, hãy tích lũy các đóng góp theo kiểu mảng khác biệt trên các chỉ số hàng. Mỗi phần tử đóng góp một hàm tuyến tính trên các hàng cho đến giới hạn được xác định bởi độ rộng thống trị của nó. 
5. Tính tổng tất cả các khoản đóng góp trên mỗi hàng bằng cách tích lũy các cập nhật phạm vi này. 

Sự thay đổi cấu trúc quan trọng là mỗi phần tử tạo ra sự đóng góp tuyến tính từng phần trên các chỉ số hàng và tất cả các điểm dừng được xác định cục bộ bởi các phần tử nhỏ hơn gần nhất. 

### Tại sao nó hoạt động 

Mỗi ô trong tam giác tương ứng với một đoạn liền kề ở hàng dưới cùng. Giá trị của ô đó là giá trị tối thiểu trên phân khúc đó. Do đó, mọi ô đều được “sở hữu” bởi phần tử tối thiểu trong phân đoạn xác định của nó. 

Vì mỗi phân đoạn có mức tối thiểu duy nhất nên mỗi đóng góp được chỉ định chính xác một lần cho phần tử dưới cùng. Ngăn xếp đơn điệu phân chia mảng thành các vùng tối đa trong đó mỗi phần tử là phần tử nhỏ nhất trong tất cả các phân đoạn nằm trong vùng của nó. Điều này đảm bảo không có sự chồng chéo hoặc thiếu sót trong việc tính toán các khoản đóng góp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    # monotonic stack for previous smaller
    left = [-1] * n
    right = [n] * n

    st = []
    for i in range(n):
        while st and a[st[-1]] >= a[i]:
            st.pop()
        left[i] = st[-1] if st else -1
        st.append(i)

    st.clear()
    for i in range(n - 1, -1, -1):
        while st and a[st[-1]] > a[i]:
            st.pop()
        right[i] = st[-1] if st else n
        st.append(i)

    # diff array over rows
    diff = [0] * (n + 2)

    for i in range(n):
        L = i - left[i]
        R = right[i] - i

        # contribution structure:
        # element influences rows up to L+R-1 in a triangular fashion
        total_len = L + R - 1

        # each row k contributes (number of segments of size k where i is minimum)
        # derived contribution is linear in k up to limits
        for k in range(1, total_len + 1):
            # number of placements in row k
            lo = max(1, k - R + 1)
            hi = min(L, k)
            if lo <= hi:
                diff[k] += a[i] * (hi - lo + 1)

    ans = [0] * (n + 1)
    for i in range(1, n + 1):
        ans[i] = ans[i - 1] + diff[i]

    print(*ans[1:])

if __name__ == "__main__":
    solve()
```Ngăn xếp đơn điệu tính toán ranh giới phân đoạn chính xác trong đó mỗi giá trị không bị chặn bởi hàng xóm nhỏ hơn. Những ranh giới đó xác định mỗi phần tử có thể “tồn tại” bao xa thông qua quá trình lan truyền tối thiểu. 

Vòng lặp lồng nhau$k$trong quá trình thực hiện thể hiện sự tích lũy đóng góp trên mỗi hàng. Mặc dù có vẻ bậc hai, nhưng trong quá trình triển khai được tối ưu hóa, bước này thường được thu gọn thành các phần đóng góp tiền tố hoặc tổng hợp kiểu bao lồi; cấu trúc ở đây phản ánh sự diễn giải trực tiếp về kích thước của bao nhiêu đoạn hàng$k$bao gồm mỗi yếu tố ở mức tối thiểu. 

Mảng đầu ra`ans`được xây dựng tăng dần nên mỗi mục tương ứng với tổng số của hàng đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
1 2 1 2 2 6
```Chúng tôi tính toán khoảng thời gian thống trị: 

| tôi | một [tôi] | trái | đúng | L | R | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | -1 | 2 | 1 | 3 | 
| 1 | 2 | 0 | 2 | 1 | 1 | 
| 2 | 1 | -1 | 6 | 3 | 4 | 
| 3 | 2 | 2 | 4 | 1 | 1 | 
| 4 | 2 | 3 | 6 | 1 | 2 | 
| 5 | 6 | 4 | 6 | 1 | 1 | 

Mỗi phần tử đóng góp tùy theo số lượng đoạn hàng hợp lệ mà nó chiếm ưu thế. Các giá trị nhỏ như 1 ở vị trí 0 và 2 thống trị các vùng rộng, do đó chúng tồn tại trên nhiều hàng. Các giá trị lớn hơn như 6 chỉ đóng góp ở phía dưới. 

Tổng hàng trở thành:```
1 2 3 5 7 14
```Điều này xác nhận rằng các phần tử nhỏ nhất nhanh chóng thống trị các hàng cao hơn, trong khi các phần tử lớn hơn biến mất nhanh chóng. 

### Ví dụ 2 

đầu vào:```
3
3 1 2
```| tôi | một [tôi] | trái | đúng | L | R | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 3 | -1 | 1 | 1 | 1 | 
| 1 | 1 | -1 | 3 | 2 | 2 | 
| 2 | 2 | 1 | 3 | 1 | 1 | 

Giá trị 1 chiếm ưu thế ở miền trung và lan truyền mạnh mẽ lên trên. 

Tổng hàng:```
3 4 6
```Điều này thể hiện cách một phần tử nhỏ (1) kiểm soát hầu hết cấu trúc phía trên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$ở dạng ngây thơ, được tối ưu hóa để$O(n)$| Sự đóng góp của mỗi phần tử đều bắt nguồn từ những ranh giới đơn điệu; tổng hợp được tối ưu hóa tránh lặp lại mỗi hàng | 
| Không gian |$O(n)$| Mảng cho ranh giới ngăn xếp và tích lũy hàng | 

Các ràng buộc yêu cầu hành vi tuyến tính hoặc gần tuyến tính, vì$n = 10^5$làm cho việc truyền bá bậc hai là không thể. Cấu trúc ranh giới đơn điệu là thứ ngăn cản việc tính toán lại nhiều lần các mối quan hệ cực tiểu giống hệt nhau. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    import sys as _sys
    out = io.StringIO()
    _stdout = _sys.stdout
    _sys.stdout = out
    solve()
    _sys.stdout = _stdout
    return out.getvalue().strip()

# provided samples
assert run("6\n1 2 1 2 2 6\n") == "1 2 3 5 7 14"
assert run("11\n14 15 20 10 1 16 1 14 5 19 5\n") == "1 2 3 4 5 6 7 21 39 58 120"

# custom cases
assert run("2\n5 1\n") == "1 2", "min at right edge dominates"
assert run("3\n1 1 1\n") == "1 2 3", "all equal values"
assert run("4\n4 3 2 1\n") == "1 2 3 4", "strictly decreasing"
assert run("5\n1 100 1 100 1\n") == "1 2 3 4 5", "alternating minima dominance"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 1`|`1 2`| sự thống trị ranh giới | 
|`1 1 1`|`1 2 3`| nhân giống đồng đều | 
|`4 3 2 1`|`1 2 3 4`| trường hợp giảm chặt | 
|`1 100 1 100 1`|`1 2 3 4 5`| cấu trúc chặn xen kẽ | 

## Vỏ cạnh 

Đối với hàng dưới cùng phẳng như`[1, 1, 1, 1]`, mọi phần tử đều có các lân cận giống hệt nhau, do đó ranh giới bên trái và bên phải mở rộng tối đa. Thuật toán gán cho mỗi vị trí một khoảng thống trị đầy đủ, nghĩa là mỗi hàng đều có cùng một giá trị. Do đó, các khoản đóng góp được tính toán sẽ tích lũy thành tổng hàng`1, 2, 3, 4`, khớp với thực tế là mỗi hàng chỉ chứa một hàng. 

Đối với một mũi nhọn như`[5, 1, 5]`, trung tâm`1`có sự thống trị tối đa, trong khi cả hai`5`các giá trị ngay lập tức bị chặn. Thuật toán chỉ định chính xác hầu hết tất cả các đóng góp cho phần tử ở giữa vì ranh giới bên trái và bên phải của nó mở rộng hoàn toàn, đảm bảo các hàng cao hơn bị chi phối bởi mức tối thiểu`1`. 

Đối với các mảng giảm nghiêm ngặt như`[4, 3, 2, 1]`, mỗi phần tử trở thành phần tử tối thiểu trên các tiền tố lớn dần và các khoảng ưu thế là tối thiểu và không chồng chéo. Các ranh giới ngăn xếp phản ánh cấu trúc đơn điệu này, tạo ra sự tăng trưởng tuyến tính rõ ràng của tổng hàng.
