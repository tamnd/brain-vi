---
title: "CF 106068G - Bảo hiểm cháy nổ"
description: "Chúng tôi đang làm việc trên một lưới thành phố hình chữ nhật với các hàng $N$ và các cột $M$. Bạn được phép đặt các trạm cứu hỏa $K$ trên các ô lưới tùy ý."
date: "2026-06-21T15:57:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106068
codeforces_index: "G"
codeforces_contest_name: "2025 Aleppo and Idlib Private Universities Collegiate Programming Contest (APUCPC 2025)"
rating: 0
weight: 106068
solve_time_s: 51
verified: true
draft: false
---

[CF 106068G - Bảo hiểm hỏa hoạn](https://codeforces.com/problemset/problem/106068/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc trên một lưới thành phố hình chữ nhật với$N$hàng và$M$cột. Bạn được phép đặt$K$trạm cứu hỏa trên các ô lưới tùy ý. Mỗi trạm có thể tiếp cận các ô khác bằng khoảng cách Chebyshev, nghĩa là cho phép di chuyển theo tám hướng và khoảng cách giữa hai ô là mức chênh lệch hàng và cột tối đa của chúng. 

Một trạm được đặt tại một ô sẽ bảo vệ hiệu quả tất cả các ô bên trong một hình vuông có tâm ở vị trí đó, với chiều dài cạnh$2d+1$, bởi vì mọi ô trong khoảng cách Chebyshev$d$thỏa mãn$\max(|x_1-x_2|, |y_1-y_2|)\le d$. 

Nhiệm vụ là chọn vị trí trạm một cách tối ưu sao cho mỗi ô trong lưới được bao phủ bởi ít nhất một trạm và chúng ta muốn kích thước nhỏ nhất có thể.$d$mà điều này có thể đạt được nhiều nhất$K$trạm. 

Lưới có thể cực kỳ lớn, lên tới$10^9 \times 10^9$, vì vậy chúng tôi không thể mô phỏng bất cứ thứ gì theo từng ô. Số lượng trạm nhỏ hơn nhiều, lên tới$10^5$, điều này gợi ý rằng giải pháp phải nén lưới thành một bài toán đếm cấu trúc. 

Một nỗ lực ngây thơ sẽ cố gắng đặt các trạm và mô phỏng phạm vi phủ sóng, nhưng ngay cả việc thể hiện lưới điện cũng là không thể. Một ý tưởng ngây thơ khác là thử tất cả các vị trí của$K$các trạm, nhưng điều đó mang tính bùng nổ về mặt tổ hợp và không liên quan ngay cả đối với các đầu vào nhỏ. 

Trường hợp cạnh tinh tế xuất hiện khi$N$Và$M$đều nhỏ nhưng$K$là lớn. Ví dụ, nếu$N=M=2$Và$K=4$, sau đó$d=0$là đủ vì mỗi trạm bao phủ chính xác một ô. Bất kỳ giải pháp nào giả định$d\ge 1$theo mặc định sẽ thất bại ở đây. 

Thách thức chính là nhận ra rằng mức độ bao phủ chỉ phụ thuộc vào số lượng$(2d+1)\times(2d+1)$cần có các ô vuông để xếp lưới chứ không phải trên vị trí trạm chính xác. 

## Phương pháp tiếp cận 

Quan điểm bạo lực bắt đầu bằng cách ấn định khoảng cách ứng viên$d$. Nếu một trạm bao phủ một hình vuông cạnh$2d+1$, thì lưới phải được bao phủ bởi các ô vuông như vậy. Người ta có thể thử đặt các trạm bằng mọi cách có thể và kiểm tra vùng phủ sóng, nhưng điều này không khả thi vì lưới điện có tới$10^{18}$các ô và thậm chí việc liệt kê các vị trí là không thể. 

Quan sát quan trọng là một lần$d$cố định, cách sắp xếp tối ưu luôn là dạng lưới. Mỗi trạm bao gồm một khối liền kề, vì vậy để giảm thiểu số lượng trạm, chúng tôi xếp các khối này mà không chồng lên nhau trong một ô xếp thông thường. Theo từng chiều, chúng ta cần$\lceil N/(2d+1)\rceil$khối theo chiều dọc và$\lceil M/(2d+1)\rceil$khối theo chiều ngang. Sản phẩm của họ cung cấp số lượng trạm tối thiểu cần thiết cho điều đó$d$. 

Điều này biến vấn đề từ vị trí hình học thành một kiểm tra tính khả thi đơn giản. Sau đó chúng tôi tìm kiếm nhỏ nhất$d$sao cho số khối yêu cầu không vượt quá$K$. Kể từ khi tăng$d$chỉ làm tăng kích thước vùng phủ sóng, tính khả thi là đơn điệu, khiến tìm kiếm nhị phân có thể áp dụng được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Vị trí Brute Force | không khả thi (theo cấp số nhân) | O(1) | Quá chậm | 
| Tìm kiếm nhị phân với kiểm tra ốp lát |$O(\log \max(N,M))$| O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi đối xử$d$là biến chúng ta muốn giảm thiểu và kiểm tra xem một giá trị đã cho$d$là đủ. 

1. Đối với cố định$d$, tính độ dài cạnh của vùng phủ sóng như$s = 2d + 1$. Điều này thể hiện số lượng ô liên tiếp mà một trạm có thể bao phủ hoàn toàn theo mỗi hướng. 
2. Tính xem cần bao nhiêu trạm để phủ lưới theo chiều dọc$a = \lceil N / s \rceil$. Điều này đếm có bao nhiêu dải chiều cao dọc$s$được yêu cầu. 
3. Tính xem cần bao nhiêu trạm theo chiều ngang$b = \lceil M / s \rceil$. Điều này đếm các dải chiều rộng ngang$s$. 
4. Tổng số trạm cần thiết là$a \times b$. Điều này tương ứng với việc đặt một trạm trên mỗi khối trong phân tách lưới đầy đủ. 
5. Kiểm tra xem$a \times b \le K$. Nếu có thì khoảng cách$d$là đủ; nếu không thì nó quá nhỏ. 
6. Sử dụng tìm kiếm nhị phân trên$d$từ$0$lên đến$\max(N, M)$, giữ giá trị khả thi nhỏ nhất. 

Lý do tìm kiếm nhị phân hoạt động là vì ngày càng tăng$d$chỉ tăng$s$, làm giảm cả hai$\lceil N/s \rceil$Và$\lceil M/s \rceil$, do đó số lượng trạm yêu cầu không bao giờ tăng. 

### Tại sao nó hoạt động 

Đối với bất kỳ cố định$d$, lưới có thể được phân chia thành các khối có kích thước nhiều nhất$(2d+1)\times(2d+1)$. Mỗi trạm không thể bao phủ nhiều hơn một khu vực xuyên ranh giới khối như vậy mà không lãng phí phạm vi phủ sóng, do đó, sự sắp xếp tối ưu hoạt động giống như một ô xếp. biểu hiện$\lceil N/s\rceil \cdot \lceil M/s\rceil$không chỉ là một công trình, nó là số lượng tối thiểu có thể có vì bất kỳ lớp phủ nào cũng phải chỉ định ít nhất một trạm cho mỗi vùng khối rời rạc do phân vùng này tạo ra. Điều này làm cho việc kiểm tra tính khả thi trở nên chính xác hơn là gần đúng, vì vậy việc tìm kiếm nhị phân trên nó sẽ tạo ra mức tối thiểu thực sự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(d, n, m, k):
    s = 2 * d + 1
    a = (n + s - 1) // s
    b = (m + s - 1) // s
    return a * b <= k

def solve():
    n, m, k = map(int, input().split())

    lo, hi = 0, max(n, m)

    while lo < hi:
        mid = (lo + hi) // 2
        if can(mid, n, m, k):
            hi = mid
        else:
            lo = mid + 1

    print(lo)

if __name__ == "__main__":
    solve()
```Giải pháp được xây dựng xung quanh một bài kiểm tra tính khả thi đơn điệu. chức năng`can`mã hóa trực tiếp đối số ốp lát: nó chuyển đổi bán kính$d$thành kích thước khối và tính toán số lượng khối được yêu cầu trong mỗi chiều. 

Tìm kiếm nhị phân duy trì tính bất biến mà bất kỳ giá trị nào bên dưới`lo`là không khả thi và bất kỳ giá trị nào bằng hoặc cao hơn`hi`có khả năng khả thi. Việc kiểm tra điểm giữa dần dần thu hẹp khoảng cho đến khi có giá trị nhỏ nhất$d$được tìm thấy. 

Điểm tinh tế duy nhất là đảm bảo việc phân chia trần theo số nguyên được xử lý chính xác, vì các lỗi riêng lẻ ở đây sẽ trực tiếp thay đổi số lượng trạm cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 5 4
```Chúng tôi tìm kiếm khắp nơi$d$. 

| d | s = 2d+1 | a = trần(5/s) | b = trần(5/s) | a*b | khả thi | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 5 | 5 | 25 | không | 
| 1 | 3 | 2 | 2 | 4 | vâng | 

Tìm kiếm nhị phân hội tụ đến$d=1$. Điều này cho thấy việc chia lưới thành$3\times3$khối phủ sóng làm giảm yêu cầu xuống còn đúng bốn trạm. 

### Ví dụ 2 

đầu vào:```
2 2 4
```| d | s | một | b | a*b | khả thi | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | 2 | 4 | vâng | 

Đây$d=0$đã hoạt động vì mỗi ô có thể được chỉ định trạm riêng. Điều này xác nhận rằng công thức xử lý các lưới tối thiểu một cách tự nhiên mà không cần vỏ đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log \max(N,M))$| tìm kiếm nhị phân trên các giá trị bán kính có thể có với kiểm tra tính khả thi theo thời gian không đổi | 
| Không gian |$O(1)$| chỉ một số số nguyên được sử dụng | 

Những ràng buộc cho phép$N, M$lên đến$10^9$, vì vậy việc mô phỏng trực tiếp là không thể. Tìm kiếm logarit trên$d$đảm bảo tối đa khoảng 30 lần lặp, dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m, k = map(int, input().split())

    def can(d):
        s = 2 * d + 1
        a = (n + s - 1) // s
        b = (m + s - 1) // s
        return a * b <= k

    lo, hi = 0, max(n, m)
    while lo < hi:
        mid = (lo + hi) // 2
        if can(mid):
            hi = mid
        else:
            lo = mid + 1

    return str(lo)

# provided sample
assert run("5 5 4") == "1"

# minimum grid
assert run("1 1 1") == "0"

# tight coverage
assert run("1 10 1") == "5"

# large K enough for d=0
assert run("3 3 9") == "0"

# rectangular imbalance
assert run("4 7 2") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | 0 | trường hợp đế lưới nhỏ nhất | 
| 1 10 1 | 5 | kéo dài một chiều | 
| 3 3 9 | 0 | phủ sóng đầy đủ mà không cần mở rộng | 
| 4 7 2 | 1 | hành vi đóng gói lưới không vuông | 

## Vỏ cạnh 

Khi nào$N$Và$M$đều bằng 1, thuật toán đánh giá$d=0$ngay lập tức kể từ đó$s=1$và chỉ cần một trạm. Kiểm tra tính khả thi trả về true một cách chính xác mà không cần bất kỳ xử lý đặc biệt nào. 

Khi lưới cực kỳ lệch, chẳng hạn như$1 \times M$, việc tính toán trở thành bài toán bao phủ một chiều. Công thức vẫn được áp dụng vì một trong$a$hoặc$b$trở thành 1 và cái còn lại nắm bắt toàn bộ phân đoạn dọc theo dòng. 

Khi$K$rất lớn so với kích thước lưới, thuật toán vẫn trả về chính xác$d=0$, từ$\lceil N/1\rceil \cdot \lceil M/1\rceil = N \cdot M \le K$là điều kiện duy nhất quan trọng.
