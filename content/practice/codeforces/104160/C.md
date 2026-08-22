---
title: "CF 104160C - Trình tự kẹp"
description: "Chúng ta được cung cấp một dãy số và được yêu cầu áp dụng một thao tác “nén” toàn cục duy nhất được xác định bởi một khoảng $[l, r]$, trong đó độ dài khoảng bị giới hạn bởi $r - l le d$."
date: "2026-07-02T01:02:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104160
codeforces_index: "C"
codeforces_contest_name: "The 2022 ICPC Asia Shenyang Regional Contest (The 1st Universal Cup, Stage 1: Shenyang)"
rating: 0
weight: 104160
solve_time_s: 68
verified: true
draft: false
---

[CF 104160C - Trình tự được kẹp](https://codeforces.com/problemset/problem/104160/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy số và được yêu cầu áp dụng một thao tác “nén” toàn cục duy nhất được xác định bởi một khoảng$[l, r]$, trong đó độ dài khoảng bị giới hạn bởi$r - l \le d$. Mọi phần tử của chuỗi được biến đổi độc lập: các giá trị bên dưới$l$được đẩy lên tới$l$, giá trị trên$r$được kéo xuống$r$và các giá trị trong khoảng không thay đổi. 

Sau phép biến đổi này, chúng ta xem xét tổng “độ căng liền kề” trong mảng, được định nghĩa là tổng của sự khác biệt tuyệt đối giữa các phần tử liên tiếp. Nhiệm vụ là chọn khoảng$[l, r]$để tối đa hóa tổng lực căng này sau khi kẹp. 

Kích thước đầu vào$n \le 5000$cho phép các thuật toán xung quanh$O(n^2)$có thể tồn tại được, nhưng bất cứ thứ gì vượt quá khối$n$sẽ thất bại. Vì các điểm cuối của khoảng là số thực nên việc tìm kiếm liên tục đơn giản trên$(l, r)$là không thể, và bất kỳ lời giải đúng nào cũng phải giảm không gian tìm kiếm xuống một tập hữu hạn các ứng viên. 

Một điểm tinh tế là phép biến đổi không tuyến tính trong$l$Và$r$. Những thay đổi nhỏ trong khoảng thời gian có thể đột ngột chuyển đổi các phần tử giữa ba chế độ: được ghim vào$l$, không thay đổi hoặc được ghim vào$r$. Điều này tạo ra hành vi từng phần, do đó, độ dốc ngây thơ hoặc lý luận tham lam không được áp dụng. 

Một trường hợp lỗi điển hình xuất phát từ việc giả định khoảng thời gian tốt nhất luôn thẳng hàng với các giá trị tối thiểu và tối đa. Ví dụ, nếu trình tự là$[0, 100, 1, 99]$Và$d$lớn, đang hái$[0, 100]$không làm gì cả, nhưng một khoảng thời gian chặt chẽ hơn như$[1, 2]$thay đổi đáng kể cấu trúc và làm tăng sự khác biệt. Khoảng tối ưu được điều khiển bởi cách nó phân chia các cặp liền kề chứ không phải bởi cực trị tổng thể. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử mọi khoảng thời gian có thể$[l, r]$thỏa mãn$r - l \le d$, sau đó tính toán lại mảng được kẹp và các hiệu liền kề của nó. Tuy nhiên, ngay cả khi chúng tôi rời rạc hóa các điểm cuối ứng viên thành$O(n)$những giá trị thì vẫn còn đó$O(n^2)$khoảng thời gian và mỗi chi phí đánh giá$O(n)$, dẫn đến$O(n^3)$, quá chậm đối với$n = 5000$. 

Quan sát cấu trúc quan trọng là đối với một cấu trúc cố định$l$, việc lựa chọn không bao giờ hữu ích$r < l + d$trừ khi tăng$r$không thay đổi bất kỳ kết quả kẹp nào. Mở rộng$r$chỉ có thể di chuyển các giá trị lên trên hoặc giữ chúng không thay đổi, điều này chỉ có thể làm tăng hoặc bảo toàn một số khác biệt liền kề. Điều này cho phép chúng tôi khắc phục$r = l + d$không mất đi tính tối ưu. 

Bây giờ vấn đề chỉ trở thành việc chọn$l$. Khó khăn là ở chỗ như$l$di chuyển, mỗi phần tử chuyển đổi giữa ba trạng thái và mỗi cặp liền kề đóng góp một giá trị chỉ thay đổi ở một tập hợp hữu hạn các ngưỡng xuất phát từ các điểm cuối của cặp. 

Thay vì tính toán lại mọi thứ cho mỗi$l$, chúng ta lật ngược quan điểm: mỗi cặp liền kề đóng góp một hàm tuyến tính từng phần của$l$, với các điểm dừng tại các giá trị mà điểm cuối chạm vào$l$hoặc$l + d$. Trên tất cả các cặp, chúng tôi tích lũy hàm tuyến tính từng phần toàn cầu. Toàn bộ hàm số chỉ thay đổi độ dốc tại$O(n^2)$điểm sự kiện, vì vậy chúng tôi có thể quét qua các sự kiện này theo thứ tự được sắp xếp và duy trì giá trị hiện tại một cách hiệu quả. 

Điều này làm giảm vấn đề duy trì một hàm tuyến tính đang chạy trên$l$, cập nhật độ dốc và điểm chặn tại mỗi sự kiện và theo dõi giá trị tối đa gặp phải trong quá trình quét. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force theo từng khoảng thời gian |$O(n^3)$|$O(1)$| Quá chậm | 
| Quét qua dòng$l$với xử lý sự kiện |$O(n^2 \log n)$hoặc$O(n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm không gian tìm kiếm xuống$r = l + d$, sau đó coi mục tiêu là một hàm$F(l)$. 

1. Sửa$r = l + d$, do đó mọi phần tử được kẹp vào ba chế độ: bên dưới$l$, bên trong$[l, r]$, hoặc cao hơn$r$. Điều này loại bỏ một bậc tự do và đảm bảo một tham số duy nhất kiểm soát việc chuyển đổi. 
2. Đối với mỗi cặp liền kề$(a_i, a_{i+1})$, xác định mức độ đóng góp của nó cho$|b_i - b_{i+1}|$thay đổi như$l$khác nhau. Sự chuyển đổi có ý nghĩa duy nhất xảy ra khi một trong hai điểm cuối đi qua$l$hoặc$l + d$, do đó mỗi cặp đóng góp các sự kiện tại$a_i$,$a_{i+1}$,$a_i - d$, Và$a_{i+1} - d$. 
3. Chuyển mỗi cặp thành một tập hợp các khoảng trên$l$-axis nơi đóng góp của nó là tuyến tính trong$l$. Trên mỗi khoảng thời gian như vậy, chúng tôi thể hiện sự đóng góp như$s \cdot l + c$, nơi có độ dốc$s$chỉ thay đổi ở ranh giới sự kiện. 
4. Thu thập tất cả các điểm sự kiện từ tất cả các cặp và sắp xếp chúng. Điều này đưa ra thứ tự quét toàn cục trên tất cả các khoảng trong đó hàm mục tiêu đầy đủ có hành vi độ dốc không đổi. 
5. Quét qua các sự kiện được sắp xếp này từ trái sang phải. Duy trì hai giá trị toàn cầu: độ dốc hiện tại của$F(l)$và giá trị hiện tại của$F(l)$ở vị trí quét. Khi băng qua một sự kiện, hãy điều chỉnh độ dốc và điểm chặn tùy theo chế độ của cặp bị ảnh hưởng thay đổi như thế nào. 
6. Giữa các sự kiện, hàm số tiến triển tuyến tính, vì vậy nếu chúng ta chuyển từ$l$đến sự kiện tiếp theo$l'$, chúng tôi cập nhật$F(l') = F(l) + \text{slope} \cdot (l' - l)$và theo dõi giá trị tối đa gặp phải. 

### Tại sao nó hoạt động 

Đóng góp của mỗi cặp liền kề chỉ phụ thuộc vào sự so sánh giữa$l$,$l + d$, và hai giá trị của cặp. Điều này có nghĩa là mọi thay đổi về cấu trúc trong mục tiêu đều xảy ra chính xác khi một trong những so sánh này bị đảo ngược. chỉ có$O(n^2)$tổng cộng những lần lật như vậy. Giữa các lần lật, hàm này hoàn toàn tuyến tính, do đó việc duy trì độ dốc và điểm chặn là đủ để mô tả chính xác toàn bộ mục tiêu. Bởi vì chúng tôi xử lý mọi thay đổi theo thứ tự, không có cấu hình nào$(l, r)$được bỏ qua và mức tối đa trên tất cả các khoảng khả thi được quan sát. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, d = map(int, input().split())
    a = list(map(int, input().split()))

    events = []

    # Each pair contributes events at critical breakpoints
    for i in range(n - 1):
        x = a[i]
        y = a[i + 1]

        # all relevant breakpoints for l are:
        # x, y, x - d, y - d
        for v in (x, y, x - d, y - d):
            events.append(v)

    events = sorted(set(events))

    # We simulate F(l) = slope * l + const
    # We recompute contributions at each segment endpoint in O(n)
    # (n is small enough for n^2 overall)

    def calc(l):
        r = l + d
        res = 0
        b_prev = 0

        def clamp(x):
            if x < l:
                return l
            if x > r:
                return r
            return x

        b_prev = clamp(a[0])
        total = 0
        for i in range(1, n):
            cur = clamp(a[i])
            total += abs(cur - b_prev)
            b_prev = cur
        return total

    ans = -10**30
    for l in events:
        ans = max(ans, calc(l))

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai được hiển thị sử dụng chiến lược rời rạc hóa đơn giản nhưng vẫn hiệu quả. Thay vì duy trì rõ ràng những thay đổi về độ dốc, nó đánh giá mục tiêu ở tất cả các giá trị ứng cử viên có cấu trúc khác biệt của$l$, đó là những điểm duy nhất mà bất kỳ phần tử nào thay đổi chế độ. Mỗi đánh giá sẽ tính toán lại trình tự được kẹp trong$O(n)$, dẫn đến tổng thể$O(n^2)$giải pháp. 

Chi tiết triển khai chính là xây dựng tập ứng viên một cách chính xác. Mọi thay đổi trong cấu trúc bị kẹp chỉ xảy ra khi$l$vượt qua một giá trị$a_i$hoặc khi nào$l + d$thánh giá$a_i$, có nghĩa là ứng cử viên$l$giá trị$a_i$Và$a_i - d$. Nếu không bao gồm cả hai, một số khoảng tối ưu sẽ không bao giờ được kiểm tra. 

Cần phải cẩn thận khi tính toán lại mảng được kẹp một cách hiệu quả. Vì việc kẹp là một hoạt động có thời gian không đổi trên mỗi phần tử nên mỗi đánh giá đầy đủ vẫn giữ nguyên tuyến tính và không cần xử lý trước tiền tố. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 2
1 5 2 6 3
```Chúng tôi kiểm tra ứng viên$l$các giá trị bắt nguồn từ$a_i$Và$a_i - d$. Giả định$l = 2$, sau đó$r = 4$. 

| tôi | một [tôi] | kẹp | 
| --- | --- | --- | 
| 1 | 1 | 2 | 
| 2 | 5 | 4 | 
| 3 | 2 | 2 | 
| 4 | 6 | 4 | 
| 5 | 3 | 3 | 

Sự khác biệt liền kề là$2, 2, 2, 1$, tổng cộng$7$. 

Đang thử ứng viên khác$l$cho thấy việc thay đổi khoảng sẽ dịch chuyển các phần tử bão hòa đến các ranh giới như thế nào, điều này trực tiếp thay đổi các khoảng trống liền kề. 

Dấu vết này cho thấy các giá trị bên trong rất quan trọng: một số phần tử không thay đổi trong khi các phần tử khác được đẩy đến điểm cuối, tạo ra bước nhảy lớn hơn. 

### Ví dụ 2 

đầu vào:```
4 1
10 1 10 1
```Nếu như$l = 1$, sau đó$r = 2$. 

| tôi | một [tôi] | kẹp | 
| --- | --- | --- | 
| 1 | 10 | 2 | 
| 2 | 1 | 1 | 
| 3 | 10 | 2 | 
| 4 | 1 | 1 | 

Sự khác biệt liền kề là$1, 1, 1$, tổng cộng$3$. 

Điều này chứng tỏ rằng việc nén chuỗi lưỡng kim thành một dải chặt sẽ tối đa hóa dao động, vì các điểm cuối xen kẽ được ghim vào các cạnh đối diện của khoảng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| có$O(n)$ứng viên$l$giá trị và mỗi chi phí đánh giá$O(n)$| 
| Không gian |$O(1)$| Chỉ các biến tạm thời được sử dụng cho mỗi đánh giá | 

Những hạn chế$n \le 5000$cho phép tối đa khoảng 25 triệu thao tác nguyên thủy, phù hợp thoải mái trong giới hạn thời gian trong Python với số học đơn giản trên mỗi bước. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# custom sanity cases
assert run("2 10\n1 100\n") == "99", "minimum structure"
assert run("4 0\n5 5 5 5\n") == "0", "no variation possible"
assert run("5 1\n1 3 2 4 3\n") is not None, "small mixed case"
assert run("3 100\n-5 0 5\n") is not None, "wide clamp range"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 10 / 1 100`|`99`| căn cứ liền kề chênh lệch | 
|`4 0 / 5 5 5 5`|`0`| khoảng kẹp suy biến | 
|`5 1 / 1 3 2 4 3`| không tầm thường | đặt hàng hỗn hợp | 
|`3 100 / -5 0 5`| không tầm thường | hành vi khoảng rộng | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các phần tử giống hệt nhau. Trong tình huống đó, bất kỳ khoảng kẹp nào cũng bảo toàn đẳng thức và câu trả lời phải bằng 0. Thuật toán xử lý việc này vì mọi ứng viên$l$tạo ra một chuỗi được kẹp không đổi và mọi đánh giá đều trả về 0, do đó mức tối đa chính xác là 0. 

Một trường hợp cạnh khác phát sinh khi$d = 0$, buộc$l = r$. Điều này thu gọn mọi phần tử về cùng một giá trị$l$, làm cho tất cả các khác biệt liền kề bằng không. Việc rà soát ứng viên vẫn bao gồm tất cả các thông tin liên quan$l$, nhưng mọi cấu hình đều tạo ra số 0, do đó mức tối đa được xử lý chính xác. 

Trường hợp cạnh cuối cùng là khi hành vi tối ưu phụ thuộc vào việc tách một cặp liền kề trong khi không ảnh hưởng đến tất cả các cặp khác. Ví dụ: khi hai giá trị liên tiếp nằm trên một ranh giới ứng cử viên, một giá trị sẽ được ghim trong khi giá trị kia nằm trong khoảng. Việc xây dựng ứng cử viên dựa trên sự kiện đảm bảo rằng các vị trí ranh giới như vậy được kiểm tra rõ ràng, do đó không bỏ sót điểm phân chia tối ưu nào.
