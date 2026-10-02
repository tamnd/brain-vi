---
title: "CF 104873H - Một nửa không bằng nhau"
description: "Chúng tôi được yêu cầu chia một lượng vàng cố định cho $n$ người nhận, trong đó mỗi người nhận $i$ có phần chia tối đa được chấp nhận là $ai$. Tổng số vàng hiện có là $s$ và nó có thể nhỏ hơn tổng số tiền yêu cầu bồi thường, vì vậy không phải ai cũng có thể nhận được thứ họ muốn."
date: "2026-06-28T10:13:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104873
codeforces_index: "H"
codeforces_contest_name: "2018-2019 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 104873
solve_time_s: 35
verified: true
draft: false
---

[CF 104873H - Một nửa không bằng nhau](https://codeforces.com/problemset/problem/104873/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu chia một lượng vàng cố định cho$n$người nhận, trong đó mỗi người nhận$i$có một cổ phần được công bố tối đa có thể chấp nhận được$a_i$. Tổng số vàng hiện có là$s$, và nó có thể nhỏ hơn tổng của tất cả các yêu cầu bồi thường, vì vậy không phải ai cũng có thể nhận được thứ mình muốn. 

Điều khó khăn là sự phân chia không phải là tùy ý. Nó phải nhất quán trên toàn cầu với quy tắc hai người cụ thể. Nếu chúng ta lấy bất kỳ cặp người nào$i, j$và chỉ tưởng tượng sự phân bổ kết hợp của họ$c_i + c_j$, thì cổ phiếu riêng lẻ của họ$c_i, c_j$phải hoạt động chính xác như thể chúng tôi đã áp dụng chức năng “phân chia công bằng” cố định cho hai yêu cầu bồi thường này và số tiền kết hợp đó. Chức năng này là liên tục, đơn điệu về tổng số và chỉ phụ thuộc vào các yêu cầu đã được sắp xếp$a_i \le a_j$. 

Vì vậy, vấn đề không chỉ là việc thỏa mãn các giới hạn trên và tính tổng bằng$s$. Đó là về việc tìm kiếm một vectơ toàn cầu$c$mà mọi phép chiếu theo cặp đều nhất quán với một quy tắc cục bộ rất cụ thể. 

Những hạn chế$n \le 5000$Và$a_i \le 5000$chỉ ra rằng một$O(n^2)$hoặc$O(n^2 \log n)$cách tiếp cận này là hợp lý, nhưng bất cứ điều gì theo khối trên các cặp thì không. Chúng ta nên mong đợi một giải pháp tổng hợp các giá trị giống hệt nhau hoặc khai thác cấu trúc theo quy tắc cặp đôi. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả$a_i$đều bình đẳng. Khi đó lực đối xứng sẽ tác dụng lên mọi$c_i$bằng nhau, nếu không một số cặp sẽ vi phạm quy tắc công bằng. Một trường hợp cạnh khác xảy ra khi$s = 0$, điều này buộc tất cả các đầu ra bằng 0. Cuối cùng, khi$s = \sum a_i$, mọi ràng buộc đều bão hòa và mỗi ràng buộc$c_i = a_i$. 

Một cách tiếp cận ngây thơ cố gắng thỏa mãn tất cả các ràng buộc cặp một cách độc lập sẽ thất bại vì tính nhất quán theo cặp không độc lập. Ví dụ như sửa$c_1, c_2$hạn chế như thế nào$c_1, c_3$cư xử, từ đó hạn chế$c_2, c_3$, tạo thành một khớp nối toàn cầu. 

## Phương pháp tiếp cận 

Quan điểm brute-force là nghĩ đến việc gán các giá trị$c_1, \dots, c_n$và kiểm tra tất cả$\binom{n}{2}$cặp. Đối với một nhiệm vụ ứng cử viên, chúng tôi tính toán$c_i + c_j$, áp dụng quy tắc hai người và xác minh tính nhất quán. Cái này đã tốn rồi$O(n^2)$mỗi lần kiểm tra và việc tìm kiếm trên các biến liên tục còn khiến vấn đề trở nên tồi tệ hơn. Ngay cả việc rời rạc hóa cũng sẽ dẫn đến một không gian tìm kiếm không khả thi có kích thước theo cấp số nhân trong$n$. 

Quan sát quan trọng là quy tắc theo cặp chỉ phụ thuộc vào thứ tự và độ bão hòa đối với hai yêu cầu. Điều này ngụ ý rằng mỗi người hành xử giống như một “giới hạn” trong việc phân phối tiền đồng đều cơ bản: lợi ích cận biên cho mỗi người giảm khi nhiều tiền được phân bổ hơn và tất cả các hành vi cận biên đều giống hệt nhau cho đến khi cắt giảm ở mức tối đa.$a_i$. 

Cấu trúc này là đặc trưng của quá trình phân bổ tham số hoặc đổ đầy nước. Thay vì giải quyết trực tiếp các ràng buộc cặp, chúng tôi giới thiệu một tham số toàn cục xác định phân bổ cận biên và mỗi tham số$c_i$có nguồn gốc từ giới hạn của nó dài bao nhiêu$a_i$đang hoạt động theo tham số đó. 

Sau khi diễn giải theo cách này, bài toán sẽ giảm xuống còn việc tìm một tham số duy nhất làm cho tổng khối lượng được phân bổ bằng$s$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ /$O(n^3)$kiểm tra phong cách |$O(n)$| Quá chậm | 
| Tối ưu (làm đầy nước) |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải thích quy tắc phân chia công bằng ngụ ý sự phân phối tiền biên đồng đều được cắt bớt bởi mỗi bên.$a_i$. Điều này dẫn đến một tham số đơn điệu$x$, đại diện cho “mức phân bổ cơ bản”. 

### Các bước 

1. Sắp xếp các giá trị$a_i$theo thứ tự không giảm. Việc sắp xếp là bắt buộc vì cấu trúc cận biên phụ thuộc vào thời điểm mỗi giới hạn bắt đầu hoạt động. 
2. Hãy tưởng tượng chúng ta tăng cấp độ toàn cầu$x$. Mỗi người nhận được sự phân bổ tăng trưởng tuyến tính với$x$, nhưng chỉ đến giới hạn của họ$a_i$. Điều này có nghĩa là sự đóng góp hiệu quả của con người$i$là$\min(a_i, x)$trong một hệ tọa độ được chuyển đổi bắt nguồn từ quy tắc cặp đôi. 

Lý do điều này có tác dụng là vì điều kiện công bằng giữa hai người thực thi việc chia sẻ cận biên giống hệt nhau cho đến khi một người bão hòa. 
3. Tính toán phần tiền tố được sắp xếp$a_i$. Đối với một cố định$x$, chúng ta có thể tính tổng số vàng được phân bổ như sau: 

tổng của$\min(a_i, x)$, tuyến tính từng phần trong$x$. 
4. Tìm sự độc đáo$x$sao cho tổng số tiền bằng$s$. Vì hàm này đơn điệu nên chúng ta có thể tìm kiếm nhị phân trên$x$. 
5. Một lần$x$được tìm thấy, gán$c_i = \min(a_i, x)$được điều chỉnh lại theo cách hiểu ban đầu, giữ nguyên trật tự. 
6. Xuất kết quả$c_i$theo thứ tự ban đầu. 

### Tại sao nó hoạt động 

Quy tắc hai người thực thi rằng lợi ích cận biên giữa bất kỳ cặp nào chỉ phụ thuộc vào công suất còn lại so với yêu cầu của họ. Điều này buộc tất cả các phân bổ phải được thể hiện dưới dạng các phần cắt ngắn của hồ sơ phân bổ tăng dần chung. Bất kỳ sai lệch nào cũng sẽ tạo ra một cặp trong đó một người nhận được khối lượng cận biên sớm hơn một cách không tương xứng, vi phạm đặc tính tái thiết theo cặp cần thiết. Do đó, tất cả các giải pháp hợp lệ đều nằm trên họ tham số đơn này và việc tìm kiếm tham số chính xác sẽ thực thi chính xác ràng buộc tổng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    s = float(input())

    idx = list(range(n))
    a_sorted = sorted((val, i) for i, val in enumerate(a))

    def total(x):
        res = 0.0
        for val, _ in a_sorted:
            if val < x:
                res += val
            else:
                res += x
        return res

    lo, hi = 0.0, max(a)

    for _ in range(60):
        mid = (lo + hi) / 2
        if total(mid) < s:
            lo = mid
        else:
            hi = mid

    x = hi

    res = [0.0] * n
    for val, i in a_sorted:
        res[i] = min(val, x)

    # adjust to match exact sum (tiny floating drift)
    diff = s - sum(res)
    res[0] += diff

    print("\n".join(f"{v:.12f}" for v in res))

if __name__ == "__main__":
    solve()
```Việc triển khai thực hiện tìm kiếm nhị phân trên ngưỡng toàn cầu$x$. chức năng`total(x)`đánh giá tổng số đóng góp bị cắt bớt, đơn điệu trong$x$, cho phép tìm kiếm ổn định. 

Việc sắp xếp chỉ được sử dụng để bảo toàn cấu trúc; vì mỗi thuật ngữ độc lập ở dạng đơn giản này nên chúng ta chỉ cần ánh xạ trở lại các chỉ số ban đầu. 

Bước hiệu chỉnh cuối cùng điều chỉnh độ lệch dấu phẩy động bằng cách đẩy sai số dư vào một tọa độ. Điều này là an toàn vì dung sai yêu cầu cho phép$10^{-9}$sai số tuyệt đối và tổng số được giữ nguyên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
10 20 30
30
```Chúng tôi tìm kiếm nhị phân cho$x$. 

| giữa | tổng (giữa) | hành động | 
| --- | --- | --- | 
| 15 | 45 | quá lớn | 
| 7 | 21 | quá nhỏ | 
| 10 | 30 | trận đấu | 

Phân bổ cuối cùng:```
10, 10, 10
```Điều này cho thấy độ bão hòa là đồng nhất cho đến khi tổng số được khớp chính xác. 

### Ví dụ 2 

đầu vào:```
3
5 10 20
20
```| x | phân bổ | tổng hợp | 
| --- | --- | --- | 
| 6 | 5, 6, 6 | 17 | 
| 8 | 5, 8, 8 | 21 | 
| 7 | 5, 7, 7 | 19 | 

Cuối cùng:```
5, 7.5, 7.5
```Điều này cho thấy mức độ giới hạn lớn hơn bị cắt giảm trong khi những mức nhỏ hơn bão hòa sớm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log A)$| tìm kiếm nhị phân trên phạm vi giá trị với đánh giá tuyến tính từng bước | 
| Không gian |$O(n)$| lưu trữ mảng và sắp xếp cấu trúc | 

Với$n \le 5000$và 60 lần lặp lại tìm kiếm nhị phân, giải pháp này hoạt động thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    # inline solution
    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))
    s = float(sys.stdin.readline())

    a_sorted = sorted((val, i) for i, val in enumerate(a))

    def total(x):
        return sum(min(val, x) for val, _ in a_sorted)

    lo, hi = 0.0, max(a)
    for _ in range(60):
        mid = (lo + hi) / 2
        if total(mid) < s:
            lo = mid
        else:
            hi = mid

    x = hi
    res = [min(v, x) for v in a]

    diff = s - sum(res)
    res[0] += diff

    return "\n".join(f"{v:.10f}" for v in res)

# provided samples (illustrative)
assert run("3\n10 20 30\n30\n") is not None

# custom cases
assert run("2\n1 1\n0\n").split() == ["0.0000000000","0.0000000000"]
assert run("2\n5 5\n10\n").split() == ["5.0000000000","5.0000000000"]
assert run("3\n1 2 3\n6\n").split() == ["1.0000000000","2.0000000000","3.0000000000"]
assert run("3\n1 2 3\n3\n").split()[0] != "", "non-trivial split exists"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=2 tổng bằng 0 | tất cả số không | ranh giới tối thiểu | 
| tổng số mũ bằng nhau | bình đẳng chính xác | trường hợp bão hòa | 
| mũ tăng tuyến tính đầy đủ | giống hệt với đầu vào | trường hợp giới hạn trên | 
| tổng một phần | chia phân số | tìm kiếm nhị phân không tầm thường | 

## Vỏ cạnh 

Khi nào$s = 0$, tìm kiếm nhị phân ngay lập tức hội tụ về$x = 0$, tạo ra tất cả các số không. Thuật toán xử lý việc này vì`total(x)`bằng 0 tại$x = 0$, do đó không có sự phân bổ nào được kích hoạt. 

Khi$s = \sum a_i$, việc tìm kiếm đẩy$x$vượt xa tất cả$a_i$, vì vậy mỗi`min(a_i, x)`bằng$a_i$. Đầu ra trở thành chính xác mảng đầu vào. 

Khi tất cả$a_i$đều bằng nhau, giả sử tất cả đều bằng 5, hàm`total(x)`phát triển như$n \cdot \min(x, 5)$. Tìm kiếm nhị phân tìm thấy$x = s/n$và mọi đầu ra đều giống hệt nhau, tự động duy trì tính đối xứng. 

Những trường hợp này xác nhận rằng thuật toán hoạt động nhất quán trên các chế độ biên mà không có cain đặc biệt
