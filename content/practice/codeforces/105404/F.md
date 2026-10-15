---
title: "CF 105404F - Tranh vuông"
description: "Chúng ta bắt đầu với một lưới trống có kích thước $n nhân m$, trong đó mọi ô ban đầu đều có màu trắng. Cách duy nhất để sửa đổi lưới là chọn toàn bộ hàng hoặc toàn bộ cột và sơn chúng hoàn toàn màu đen."
date: "2026-06-23T04:49:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105404
codeforces_index: "F"
codeforces_contest_name: "XXIV Spain Olympiad in Informatics, Online Qualifier 2"
rating: 0
weight: 105404
solve_time_s: 73
verified: true
draft: false
---

[CF 105404F - Tranh hình vuông](https://codeforces.com/problemset/problem/105404/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một lưới có kích thước trống$n \times m$, trong đó mọi ô ban đầu đều có màu trắng. Cách duy nhất để sửa đổi lưới là chọn toàn bộ hàng hoặc toàn bộ cột và sơn chúng hoàn toàn màu đen. Khi một hàng được sơn, mọi ô trong hàng đó sẽ chuyển sang màu đen; điều tương tự cũng áp dụng cho các cột. 

Sau khi thực hiện bất kỳ số thao tác nào như vậy, chúng tôi muốn số lượng ô trắng còn lại chính xác$k$. Mỗi trường hợp thử nghiệm cho chúng ta$n$,$m$và giá trị mục tiêu này$k$và chúng ta phải quyết định liệu nó có thể đạt được hay không. 

Một quan sát quan trọng về cấu trúc của các phép toán là cấu hình cuối cùng chỉ phụ thuộc vào hàng nào và cột nào được vẽ chứ không phụ thuộc vào thứ tự của các phép toán. Nếu chúng ta vẽ một tập hợp các hàng$R$và một tập hợp các cột$C$, thì mọi ô trong$R$hoặc$C$trở nên đen. Các ô màu trắng còn lại chính xác là những ô nằm ngoài cả hai bộ, tạo thành một khối hình chữ nhật rõ ràng có kích thước$(n - |R|) \times (m - |C|)$. 

Điều này có nghĩa là số lượng tế bào trắng cuối cùng luôn có dạng$$(n - r)(m - c)$$đối với một số số nguyên$0 \le r \le n$,$0 \le c \le m$. 

Vì vậy vấn đề không còn là về các phép toán nữa mà là về việc liệu chúng ta có thể biểu diễn$k$là diện tích của một số hình chữ nhật phụ được căn chỉnh theo kích thước lưới. 

Từ những hạn chế,$t$có thể lên tới 30000 nên mỗi bài kiểm tra phải rất nhanh, lý tưởng nhất là$O(1)$hoặc$O(\sqrt{k})$lúc tệ nhất. Từ$n$,$m$, Và$k$có thể lớn, bất cứ điều gì liên quan đến mô phỏng trên lưới hoặc các hoạt động liệt kê đều không thể thực hiện được. 

Một trường hợp thất bại phổ biến xuất phát từ lý luận theo “trình tự vẽ” thay vì hình học cuối cùng. Ví dụ: người ta có thể nghĩ sai về sự chồng chéo giữa các thao tác hàng và cột, làm thay đổi số lượng một cách phức tạp. Trong thực tế, sự chồng chéo không thành vấn đề: một ô có màu đen nếu hàng hoặc cột của nó được chọn, do đó kết quả hoàn toàn là phép nhân. 

Một sai lầm tinh vi khác là điều trị$k$tùy ý mà quên rằng nó phải bằng diện tích hình chữ nhật bên trong lưới. Ví dụ, nếu$n = 3$,$m = 3$, Và$k = 5$, không có cách nào để hình thành một$3 \times 3$diện tích lưới của chính xác 5 ô trắng còn lại, mặc dù 5 ô có vẻ “có thể truy cập được” thông qua trực giác vẽ một phần. 

## Phương pháp tiếp cận 

Phối cảnh bạo lực sẽ mô phỏng tất cả các lựa chọn về hàng và cột được sơn. Đối với mỗi tập hợp con của hàng và cột, chúng tôi tính toán số lượng ô trắng thu được. Điều này ngay lập tức trở thành cấp số nhân trong$n + m$, vì có$2^n \cdot 2^m$các trạng thái có thể. Ngay cả đối với các lưới điện nhỏ, điều này sẽ phát nổ và không thể sử dụng được. 

Sự đơn giản hóa chính xuất phát từ việc nhận ra rằng tất cả các trạng thái cuối cùng hợp lệ chỉ được xác định bằng số lượng hàng và cột được loại bỏ hoàn toàn. Nếu chúng ta loại bỏ$r$hàng và$c$cột, vùng màu trắng còn lại là một hình chữ nhật hoàn hảo có kích thước$(n-r) \times (m-c)$. Điều này chuyển vấn đề thành một phép kiểm tra nhân tử thuần túy: chúng ta đang hỏi liệu có tồn tại số nguyên hay không$x \le n$Và$y \le m$như vậy$x \cdot y = k$. 

Vì vậy nhiệm vụ giảm xuống còn việc tìm xem liệu$k$có một số chia$x$phù hợp với bên trong$n$, với thương số tương ứng$y = k/x$phù hợp bên trong$m$. 

Chúng ta không còn cần phải nghĩ về các phép toán nữa mà chỉ nghĩ về các ước của$k$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tập hợp con hàng/cột) |$O(2^{n+m})$|$O(1)$| Quá chậm | 
| Kiểm tra các ước của$k$|$O(\sqrt{k})$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quan sát sau khi chọn hàng và cột cần tô, các ô màu trắng còn lại tạo thành hình chữ nhật có kích thước$(n-r) \times (m-c)$. Điều này làm giảm vấn đề chọn độ dài hai cạnh. 
2. Viết lại bài toán dưới dạng tìm số nguyên$x, y$như vậy$x \cdot y = k$, với$x \le n$Và$y \le m$. Đây$x$đại diện cho các hàng chưa được sơn còn lại và$y$các cột chưa được sơn còn lại. 
3. Lặp lại tất cả các ước số có thể$x$của$k$. Đối với mỗi$x$, kiểm tra xem nó có chia không$k$sạch sẽ. Điều này đảm bảo chúng tôi chỉ xem xét các kích thước hình chữ nhật hợp lệ. 
4. Với mỗi ước số hợp lệ$x$, tính toán$y = k / x$, sau đó kiểm tra xem$x \le n$Và$y \le m$. 
5. Nếu không có cặp nào như vậy hoạt động, hãy kết luận rằng$k$không thể được tạo thành một vùng hình chữ nhật hợp lệ bên trong lưới. 

### Tại sao nó hoạt động 

Mọi chuỗi thao tác hợp lệ đều tương ứng chính xác với việc chọn một tập hợp các hàng và cột cần loại bỏ và lựa chọn đó luôn tạo ra một vùng màu trắng còn lại hình chữ nhật. Ngược lại, mọi hình chữ nhật có kích thước$x \times y$bên trong lưới có thể thu được bằng cách rời khỏi$x$hàng và$y$cột không sơn và sơn phần còn lại. Điều này tạo ra sự tương ứng một-một giữa các cấu hình khả thi và các cặp yếu tố của$k$bị ràng buộc bởi kích thước lưới, vì vậy việc kiểm tra tất cả các ước số là cần thiết và đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def possible(n, m, k):
    x = 1
    while x * x <= k:
        if k % x == 0:
            y = k // x
            if x <= n and y <= m:
                return True
            if y <= n and x <= m:
                return True
        x += 1
    return False

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n, m, k = map(int, input().split())
        out.append("SI" if possible(n, m, k) else "NO")
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```chức năng`possible`thực hiện quét số chia lên đến$\sqrt{k}$. Với mỗi số chia$x$, nó kiểm tra cả hai hướng của hình chữ nhật, vì$x$có thể đại diện cho các hàng còn lại hoặc các cột còn lại. 

Việc kiểm tra đối xứng là cần thiết vì chúng ta không biết trước liệu$x$nên tương ứng với$n$-kích thước hoặc$m$-kích thước. Thiếu sự hoán đổi này là nguyên nhân phổ biến dẫn đến các câu trả lời sai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 50 81
```Chúng tôi cố gắng bày tỏ$81$BẰNG$x \cdot y$với$x \le 2$,$y \le 50$. 

| x | y = 81/x | x 2 | y 50 | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | 81 | vâng | không | không | 
| 3 | 27 | không | vâng | không | 
| 9 | 9 | không | vâng | không | 

Không có cặp yếu tố nào phù hợp với các ràng buộc của lưới, vì vậy câu trả lời là KHÔNG. 

### Ví dụ 2 

đầu vào:```
29 22 546
```Chúng tôi tìm kiếm các ước của 546. 

| x | y = 546/x | x 29 | y 22 | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 2 | 273 | vâng | không | không | 
| 3 | 182 | vâng | không | không | 
| 6 | 91 | vâng | không | không | 
| 7 | 78 | vâng | không | không | 
| 14 | 39 | vâng | không | không | 
| 21 | 26 | vâng | không | không | 
| 22 | 24.8 | không | - | không | 
| 26 | 21 | vâng | vâng | vâng | 

Chúng tôi tìm thấy một cặp hợp lệ$26 \times 21 = 546$, và cả hai đều vừa khít bên trong$29 \times 22$, vậy đáp án là SI. 

Dấu vết này cho thấy chúng ta chỉ cần một cặp nhân tố hợp lệ chứ không phải tất cả các khả năng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t \sqrt{k})$| Mỗi bài kiểm tra sẽ quét các ước của$k$lên đến$\sqrt{k}$| 
| Không gian |$O(1)$| Chỉ sử dụng các biến không đổi | 

Mặc dù$\sqrt{k}$về mặt lý thuyết có thể lớn, phép toán là số học số nguyên đơn giản và tổng số lần kiểm tra vẫn nằm trong giới hạn điển hình cho 30000 trường hợp thử nghiệm trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def possible(n, m, k):
        x = 1
        while x * x <= k:
            if k % x == 0:
                y = k // x
                if x <= n and y <= m:
                    return True
                if y <= n and x <= m:
                    return True
            x += 1
        return False

    t = int(input())
    out = []
    for _ in range(t):
        n, m, k = map(int, input().split())
        out.append("SI" if possible(n, m, k) else "NO")
    return "\n".join(out)

# provided samples
assert run("3\n2 50 81\n29 22 546\n27 13 168\n") == "NO\nSI\nSI"

# minimum case
assert run("1\n1 1 1\n") == "SI"

# impossible small factor case
assert run("1\n2 2 3\n") == "NO"

# perfect rectangle case
assert run("1\n10 10 25\n") == "SI"

# large asymmetric case
assert run("1\n100 2 200\n") == "SI"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1×1 | SI | trường hợp cơ bản tầm thường | 
| 2×2, k=3 | KHÔNG | diện tích không thể tính được | 
| 10×10, k=25 | SI | xử lý hệ số bình phương | 
| 100×2, k=200 | SI | hoán đổi định hướng | 

## Vỏ cạnh 

Trường hợp góc là khi$k = 1$. Mặc dù vấn đề nêu$k \ge 2$, lý luận về nó làm rõ tính đúng đắn: chỉ$1 \times 1$hình chữ nhật sẽ hợp lệ, nghĩa là lưới phải cho phép ít nhất một hàng và cột không bị chạm. 

Một trường hợp tế nhị khác là khi$k$là nguyên tố. Ví dụ,$k = 13$với$n = 3$,$m = 10$. Hệ số hóa duy nhất là$1 \times 13$, không thành công trừ khi một chiều có thể chứa 13. Thuật toán loại bỏ chính xác tất cả các giá trị khác vì không tồn tại ước số nào khác ngoài 1. 

Một trường hợp thú vị hơn là khi cả hai hệ số đều tồn tại nhưng chỉ có một hệ số phù hợp. Vì$n = 5$,$m = 20$,$k = 50$, chúng ta có cặp$(1,50)$,$(2,25)$,$(5,10)$,$(10,5)$. Chỉ một$(5,10)$Và$(10,5)$có liên quan và ít nhất một thỏa mãn các ràng buộc, do đó thuật toán sẽ chấp nhận ngay lập tức khi gặp nó.
