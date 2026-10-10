---
title: "CF 104992B - \u041a\u0438\u0440\u0438\u043b\u043b \u0438 \u043a\u0440\u043e\u043b\u0438\u043a\u0438"
description: "Chúng ta đang xem xét một hệ thống các con vật giống hệt nhau, trong đó mỗi con vật tiêu thụ một số lượng cà rốt cố định trong mỗi bữa ăn. Lượng mỗi bữa đó là như nhau trong tất cả các bữa ăn đối với một con vật nhất định và cũng giống nhau ở tất cả các con vật."
date: "2026-06-28T04:26:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104992
codeforces_index: "B"
codeforces_contest_name: "qual VKOSHP Junior 24"
rating: 0
weight: 104992
solve_time_s: 78
verified: false
draft: false
---

[CF 104992B - \u041a\u0438\u0440\u0438\u043b\u043b \u0438 \u043a\u0440\u043e\u043b\u0438\u043a\u0438](https://codeforces.com/problemset/problem/104992/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang xem xét một hệ thống các con vật giống hệt nhau, trong đó mỗi con vật tiêu thụ một số lượng cà rốt cố định trong mỗi bữa ăn. Lượng mỗi bữa đó là như nhau trong tất cả các bữa ăn đối với một con vật nhất định và cũng giống nhau ở tất cả các con vật. Điểm khác biệt duy nhất là các loài động vật khác nhau có thể chọn các số nguyên khác nhau, nhưng mỗi số nguyên đó phải nằm trong một phạm vi cố định.$[a, b]$. 

Chúng ta được biết tổng số cà rốt mà tất cả các loài động vật tiêu thụ trong đúng hai bữa ăn là$n$. Vì mỗi con vật ăn một lượng như nhau trong cả hai bữa nên con vật nào ăn$x$cà rốt mỗi bữa đóng góp chính xác$2x$cà rốt vào tổng số. Nếu có$k$động vật có giá trị tiêu thụ mỗi bữa ăn$x_1, x_2, \dots, x_k$, thì tổng số là$$2(x_1 + x_2 + \dots + x_k) = n.$$Vì vậy, vấn đề giảm xuống còn việc hỏi liệu chúng ta có thể biểu diễn$n/2$như một tổng của$k$số nguyên, mỗi số trong phạm vi$[a, b]$, và chúng tôi muốn tối đa hóa$k$. 

Các ràng buộc đi lên đến$10^{15}$, do đó, bất kỳ giải pháp nào cố gắng liệt kê các ứng cử viên hoặc mô phỏng các phân vùng đều không khả thi ngay lập tức. Thậm chí lặp lại tất cả các giá trị có thể có của$k$hoặc tất cả các tác phẩm có thể có sẽ quá chậm, vì$k$bản thân nó có thể lớn bằng$10^{15}$. 

Một quan sát quan trọng đầu tiên là nếu$n$thật kỳ lạ, tình huống này là không thể ngay lập tức. Vì mỗi đóng góp đều$2x$, tổng số phải chẵn. 

Một trường hợp thất bại tinh vi khác xảy ra khi$n/2$quá nhỏ hoặc quá lớn để có thể biểu diễn dưới dạng tổng các giá trị trong$[a, b]$. Ví dụ, nếu$n = 10$,$a = 6$,$b = 7$, thì mỗi con vật sẽ ăn tổng cộng từ 12 đến 14 củ cà rốt trong hai bữa ăn. Ngay cả một con vật cũng đã vượt quá 10, vì vậy câu trả lời đúng là$-1$. Một cách tiếp cận đơn giản chỉ kiểm tra khả năng chia hết cho 2 sẽ xuất ra sai 1 hoặc nhiều hơn. 

Tương tự, nếu$n/2$lớn nhưng bị ràng buộc quá chặt chẽ, chẳng hạn như$n = 9$,$a = 2$,$b = 5$, sau đó$n/2 = 4.5$không phải là số nguyên, do đó câu trả lời ngay lập tức không hợp lệ mặc dù giới hạn cục bộ có vẻ tương thích. 

Khó khăn chính là chúng tôi không chỉ định các giá trị cho từng bữa ăn riêng biệt mà cho các số nguyên cố định cho mỗi con vật bị ràng buộc bởi tính khả thi toàn cầu. 

## Phương pháp tiếp cận 

Một quan điểm bạo lực sẽ cố gắng xác định có bao nhiêu động vật$k$chúng ta có thể có bằng cách kiểm tra xem$n/2$có thể bị phân hủy thành$k$mỗi số nguyên ở giữa$a$Và$b$. Đối với một cố định$k$, chúng ta cần kiểm tra xem tổng của$k$giá trị trong$[a, b]$có thể bằng$S = n/2$. Điều này tương đương với việc kiểm tra xem$$k \cdot a \le S \le k \cdot b.$$Nếu điều này đúng thì sự phân rã như vậy tồn tại bằng cách phân phối phần thặng dư từ$a$đồng đều trên một số phần tử, điều chỉnh trong giới hạn. 

Vì vậy đối với mỗi$k$, kiểm tra tính khả thi là thời gian không đổi. Cách tiếp cận vũ phu sẽ thử tất cả$k$từ$1$ĐẾN$S/a$. Trong trường hợp xấu nhất, khi$a = 1$, điều này có nghĩa là lặp lại tối đa$10^{15}$những giá trị hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là tính khả thi chỉ phụ thuộc vào sự bất bình đẳng liên quan đến$k$, không phải trên cấu trúc bên trong của phân vùng. Một khi chúng ta viết lại điều kiện$$k \cdot a \le S \le k \cdot b,$$chúng ta có thể cô lập$k$:$$\frac{S}{b} \le k \le \frac{S}{a}.$$Do đó bài toán quy về việc tìm số nguyên lớn nhất$k$thỏa mãn các giới hạn này, tức là:$$k_{\max} = \left\lfloor \frac{S}{a} \right\rfloor,$$miễn là điều này$k$cũng thỏa mãn$k \cdot b \ge S$. Nếu không, không hợp lệ$k$tồn tại. 

Điều này thu gọn toàn bộ cấu trúc tổ hợp thành một lần kiểm tra khoảng thời gian duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force kết thúc$k$|$O(S/a)$|$O(1)$| Quá chậm | 
| Giảm bất bình đẳng |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán$S = n / 2$. Nếu như$n$kì quặc, quay lại ngay$-1$. Điều này là cần thiết vì mọi đóng góp đều được nhân đôi khi xây dựng. 
2. Tính số lượng động vật tối đa có thể là$k = \left\lfloor S / a \right\rfloor$. Điều này tương ứng với việc sử dụng mức tiêu thụ nhỏ nhất được phép cho mỗi con vật để tối đa hóa số lượng. 
3. Kiểm tra xem ứng viên này có$k$có giá trị bằng cách xác minh rằng tổng số lớn nhất có thể với$k$động vật, đó là$k \cdot b$, ít nhất là$S$. Nếu không, ngay cả nhiệm vụ hào phóng nhất cũng không thể đạt được tổng số yêu cầu. 
4. Nếu kiểm tra thành công, xuất ra$k$. Ngược lại, xuất ra$-1$. 

### Tại sao nó hoạt động 

Mỗi con vật đóng góp một số nguyên độc lập vào$[a, b]$, do đó tập hợp các khoản tiền có thể đạt được cho$k$động vật chính xác là khoảng thời gian$[k a, k b]$. Không có khoảng trống vì chúng ta có thể tăng giá trị của một con vật lên 1 trong khi vẫn ở trong giới hạn. Do đó, tính khả thi giảm xuống còn việc kiểm tra xem liệu$S$nằm trong khoảng này đối với một số$k$. Chọn phương án lớn nhất có thể thực hiện được$k$tương đương với việc đẩy$k$cho đến khi giới hạn dưới$k a \le S$chặt chẽ, đồng thời đảm bảo giới hạn trên vẫn bao phủ$S$. Điều này đảm bảo cả tính chính xác và tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
a = int(input().strip())
b = int(input().strip())

if n % 2 == 1:
    print(-1)
    sys.exit()

S = n // 2

k = S // a

if k == 0:
    print(-1)
    sys.exit()

if k * b < S:
    print(-1)
else:
    print(k)
```Mã đầu tiên thực thi ràng buộc chẵn lẻ vì tất cả các tổng hợp lệ đều là số chẵn. Sau đó nó giảm bớt vấn đề để làm việc với$S = n/2$, tổng lượng tiêu thụ mỗi bữa ăn. 

biểu hiện`S // a`chọn số lượng động vật tối đa có thể với mức tiêu thụ nhỏ nhất được phép cho mỗi con vật. Điều kiện tiếp theo`k * b < S`xác minh xem ngay cả việc tối đa hóa mức tiêu thụ của mỗi con vật vẫn không thể đạt được số tiền yêu cầu hay không, điều đó có nghĩa là không tồn tại cấu hình hợp lệ. 

Sự rõ ràng`k == 0`kiểm tra xử lý các trường hợp$S < a$, nghĩa là ngay cả một con vật cũng không thể được gán giá trị hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8
2
3
```Đây$S = 4$,$a = 2$,$b = 3$. 

| Bước | S | k = S//a | k·b | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 2 | 6 | hợp lệ | 

Từ$2 \cdot 2 = 4$nằm bên trong$[4, 6]$, đầu ra là 2. 

Điều này xác nhận rằng hai con vật mỗi con ăn 2 củ cà rốt mỗi bữa hoàn toàn khớp với tổng số lượng cần thiết. 

### Ví dụ 2 

đầu vào:```
15
3
4
```Đây$S = 7.5$, không phải là số nguyên nên việc tính toán đã thất bại. 

| Bước | S hợp lệ | Quyết định | 
| --- | --- | --- | 
| 1 | không phải số nguyên | không hợp lệ | 

Vì tổng lượng tiêu thụ trong mỗi bữa ăn không phải là số nguyên nên không có phép gán số nguyên giống hệt nhau cho mỗi con vật có thể tạo ra số tiền này. Đầu ra là$-1$. 

Điều này chứng tỏ ràng buộc chẵn lẻ là cần thiết chứ không chỉ là sự tiện lợi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ một vài phép tính số học và so sánh | 
| Không gian |$O(1)$| Không sử dụng cấu trúc phụ trợ | 

Giải pháp phù hợp thoải mái trong giới hạn ngay cả đối với các giá trị lên tới$10^{15}$, vì tất cả các phép toán đều là số học số nguyên theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input().strip())
    a = int(input().strip())
    b = int(input().strip())

    if n % 2 == 1:
        return "-1"

    S = n // 2
    k = S // a

    if k == 0:
        return "-1"

    if k * b < S:
        return "-1"
    return str(k)

# provided samples (interpreted format)
assert run("8\n2\n3\n") == "2"
assert run("15\n3\n4\n") == "-1"

# custom cases
assert run("1\n1\n1\n") == "-1", "odd total"
assert run("2\n2\n2\n") == "1", "single exact fit"
assert run("100\n10\n10\n") == "5", "fixed value range"
assert run("100\n6\n7\n") == "8", "upper feasibility boundary"
assert run("100\n60\n70\n") == "-1", "too large minimum sum"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| lẻ n | -1 | từ chối chẵn lẻ | 
| chính xác phù hợp duy nhất | 1 | độ đúng ranh giới | 
| cố định a=b | 5 | xử lý khoảng suy biến | 
| giới hạn trên chặt chẽ | 8 | cạnh khả thi | 
| không thể lớn a | -1 | số tiền tối thiểu không khả thi | 

## Vỏ cạnh 

Khi nào$n$thật kỳ lạ, chẳng hạn như$n = 1$, thuật toán ngay lập tức bác bỏ nó vì$S = n/2$không phải là số nguyên nên không có phép gán số nguyên nào cho mỗi con vật có thể tạo ra tổng hợp lệ. 

Khi$S < a$, Ví dụ$n = 5$,$a = 3$,$b = 10$, chúng tôi nhận được$S = 2$. Việc tính toán mang lại$k = 0$và thuật toán trả về$-1$, phản ánh chính xác rằng ngay cả một con vật cũng không thể được chỉ định mức tiêu thụ hợp lệ. 

Khi mức tiêu thụ tối thiểu của mỗi con vật lớn, chẳng hạn như$n = 100$,$a = 60$,$b = 70$, chúng tôi nhận được$S = 50$, và thậm chí một con vật cũng đã vượt quá tổng số. điều kiện$k = S // a = 0$kích hoạt sự từ chối, xử lý chính xác ranh giới này. 

Khi$a = b$, chẳng hạn như$a = b = 5$, cấu trúc tổng duy nhất có thể có là cứng nhắc. Thuật toán giảm xuống để kiểm tra xem$S$chia hết cho$5$, và trả về$k = S/5$nếu hợp lệ, nếu không$-1$.
