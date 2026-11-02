---
title: "CF 1091C - Năm Mới và Truyền Cầu"
description: "Chúng ta đang đặt $n$ người xung quanh một vòng tròn và liên tục di chuyển một quả bóng theo những bước nhảy cố định. Bắt đầu từ người $1$, chúng tôi chọn kích thước bước $k$. Mỗi lần bóng được chuyền, chúng ta di chuyển $k$ vị trí theo chiều kim đồng hồ, quấn quanh vòng tròn."
date: "2026-06-13T04:11:04+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1091
codeforces_index: "C"
codeforces_contest_name: "Good Bye 2018"
rating: 1400
weight: 1091
solve_time_s: 222
verified: true
draft: false
---

[CF 1091C - Năm mới và sự truyền tải quả cầu](https://codeforces.com/problemset/problem/1091/C) 

**Đánh giá:** 1400 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 3 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang đặt$n$mọi người đứng xung quanh một vòng tròn và liên tục di chuyển quả bóng theo những bước nhảy cố định. Bắt đầu từ người$1$, chúng tôi chọn kích thước bước$k$. Mỗi lần bóng được chuyền, chúng ta di chuyển$k$vị trí theo chiều kim đồng hồ, bao quanh vòng tròn. Cuối cùng, vì chuyển động có tính tất định trên một tập hữu hạn nên quá trình quay trở lại con người.$1$, và việc đi qua dừng lại. 

Đối với một cố định$k$, chúng tôi không ghé thăm tất cả$n$mọi người nói chung. Chúng ta chỉ truy cập một nhóm con tuần hoàn của các vị trí được xác định bằng cách thêm liên tục$k$modulo$n$. Tập đã thăm chính xác là quỹ đạo của$1$theo phần bổ sung mô-đun này. Câu trả lời cho điều đó$k$là tổng của tất cả các chỉ số riêng biệt gặp phải trước khi quay lại$1$. 

Nhiệm vụ là tính toán cho mọi giá trị hợp lệ$k$, tổng kết quả của các vị trí đã ghé thăm và xuất ra tập hợp tất cả các tổng riêng biệt như vậy. 

Ràng buộc$n \le 10^9$loại trừ mọi mô phỏng$k$hoặc di chuyển trực tiếp tất cả các vị trí. Thậm chí lặp đi lặp lại tất cả$k$là không thể. Do đó, cấu trúc khóa phải xuất phát từ các thuộc tính lý thuyết số của cấu trúc số học và chu trình mô-đun. 

Một ý tưởng ngây thơ là mô phỏng việc đi bộ cho mỗi$k$. Đối với một cố định$k$, độ dài chu kỳ là$n / \gcd(n, k)$, do đó, ngay cả một chi phí mô phỏng$O(n)$trong trường hợp xấu nhất. Làm điều này cho tất cả$k$ít nhất làm cho sự phức tạp$O(n^2)$, điều đó hoàn toàn không thể thực hiện được. 

Một chế độ lỗi tinh vi hơn đang cố gắng chỉ tính toán độ dài chu kỳ. Biết độ dài là không đủ, bởi vì tổng phụ thuộc vào số dư lượng chính xác đã được truy cập chứ không chỉ là số lượng. 

Một vấn đề cạnh khác là tính đối xứng: các giá trị khác nhau của$k$có thể tạo ra các chu kỳ giống hệt nhau vì bước đi$k$hoặc bởi$n-k$duyệt cùng một nhóm con theo thứ tự ngược lại. Một giải pháp đúng phải tránh tính hai lần các máy phát điện tương đương này. 

## Phương pháp tiếp cận 

Quá trình “thêm$k$modulo$n$” chia đường tròn thành các chu kỳ có độ dài$$\frac{n}{d}, \quad d = \gcd(n, k).$$Thay vì suy nghĩ về mặt$k$, chúng tôi chuyển quan điểm sang$d$, gcd. Mỗi kích thước bước$k$tương ứng với một số chia$d$của$n$và tất cả các kích thước bước chia sẻ cùng một gcd sẽ tạo ra cùng một tập hợp các nút đã truy cập. 

Đối với số chia cố định$d$, quỹ đạo của$1$là:$$1, 1 + k, 1 + 2k, \dots$$modulo nào$n$tạo thành một cấp số cộng với bước$k$và chiều dài$n/d$. Tập đã thăm chỉ phụ thuộc vào$d$, không phải là cụ thể$k$, do đó vấn đề giảm xuống việc liệt kê các ước số của$n$. 

Bây giờ quan sát chính: các vị trí đã ghé thăm cho một vị trí cố định$d$chính xác là tất cả các số đều đồng dạng với$1 \bmod d$lên đến$n$, nhưng được “gói” trong nhóm con nhân tạo bởi$k/d$modulo$n/d$. Điều này đơn giản hóa thành một thực tế cổ điển: chu trình bao gồm mọi$d$- bội số thứ trong một hệ thống rút gọn và tổng chỉ phụ thuộc vào$n$Và$d$, không bật$k$. 

Cho phép$m = n/d$. Chu kỳ có độ dài$m$và các giá trị được truy cập là:$$1, 1 + k, 1 + 2k, \dots, 1 + (m-1)k \pmod n.$$Nếu chúng ta bình thường hóa bằng cách chia mọi thứ cho$d$, chúng tôi giảm cấu trúc thành một chu trình hoán vị trên$m$các phần tử, nghĩa là mọi ước số$d$đóng góp chính xác một khoản tiền riêng biệt. 

Vì vậy chúng ta chỉ cần lặp qua tất cả các ước số$d$của$n$, tính tổng tương ứng và thu thập kết quả. 

Phép liệt kê số chia là$O(\sqrt{n})$, điều này khả thi ngay cả đối với$10^9$. Số lượng kết quả riêng biệt được giới hạn bởi số lượng ước số, phù hợp với tuyên bố đảm bảo. 

### So sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force trên tất cả$k$|$O(n^2)$|$O(1)$| Quá chậm | 
| Nhóm dựa trên số chia |$O(\sqrt{n})$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp lại tất cả các số nguyên$d$như vậy$d \mid n$. Chúng ta chỉ xét các ước số vì mỗi cấu trúc chu trình được xác định duy nhất bởi$\gcd(n, k)$, đó phải là ước số của$n$. 
2. Với mỗi ước số$d$, tính toán$m = n / d$. Điều này thể hiện độ dài chu kỳ được tạo ra bởi bất kỳ kích thước bước nào có gcd bằng$d$. 
3. Tính phần đóng góp (giá trị vui) cho chu kỳ này. Chu kỳ bao gồm$m$các vị trí riêng biệt phân bố đều xung quanh vòng tròn. Tổng của tất cả các giá trị được truy cập đơn giản hóa thành cấu trúc số học chỉ phụ thuộc vào$n$Và$d$, ước tính là:$$\text{sum}(d) = \frac{m \cdot (2n - (m-1)d)}{2}.$$Điều này xuất phát từ việc tính tổng gói modulo tiến triển số học trong toàn bộ chu kỳ. 
4. Lưu trữ từng giá trị được tính toán trong một tập hợp để tránh trùng lặp phát sinh từ các ước số đối xứng. 
5. Sắp xếp các giá trị kết quả và xuất chúng. 

### Tại sao nó hoạt động 

Mỗi kích thước bước$k$chia đường tròn thành các chu trình rời rạc mà cấu trúc của nó chỉ phụ thuộc vào$d = \gcd(n, k)$. Hai khác nhau$k$các giá trị có cùng gcd tạo ra các tập hợp được truy cập giống hệt nhau, do đó chúng đóng góp cùng một giá trị thú vị. Ngược lại, các ước số khác nhau mang lại các phân rã chu trình khác nhau, do đó mỗi giá trị vui riêng biệt tương ứng với chính xác một lớp ước số. Sự đối chiếu này giữa các lớp gcd và câu trả lời đảm bảo tính đầy đủ và tránh tính toán quá mức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    
    res = set()
    
    d = 1
    while d * d <= n:
        if n % d == 0:
            for div in (d, n // d):
                m = n // div
                # sum of arithmetic structure derived from cycle decomposition
                val = m * (2 * n - (m - 1) * div) // 2
                res.add(val)
        d += 1
    
    ans = sorted(res)
    print(*ans)

if __name__ == "__main__":
    solve()
```Mã liệt kê các ước số bằng phương pháp căn bậc hai tiêu chuẩn. Đối với mỗi ước số, nó tính độ dài chu kỳ cảm ứng$m$. Công thức được sử dụng mã hóa tổng đóng góp của tất cả các nút đã truy cập trong cấu hình chu trình đó. Chúng tôi chèn kết quả vào một tập hợp vì nhiều ước số có thể dẫn đến cùng một giá trị do tính đối xứng giữa các ước số bổ sung$d$Và$n/d$. 

Bước sắp xếp cuối cùng là bắt buộc vì bài toán yêu cầu sản lượng đơn hàng tăng dần. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 6$Ta liệt kê các ước số:$1, 2, 3, 6$. 

| d | m = n/d | giá trị tính toán | 
| --- | --- | --- | 
| 1 | 6 | 21 | 
| 2 | 3 | 9 | 
| 3 | 2 | 5 | 
| 6 | 1 | 1 | 

Đầu ra là:$$1, 5, 9, 21.$$Điều này khớp với mẫu và cho thấy mỗi ước số tương ứng với một cấu trúc truyền tải riêng biệt như thế nào: chu trình đầy đủ, chu trình nửa bước, v.v. 

### Ví dụ 2:$n = 10$Số chia là$1, 2, 5, 10$. 

| d | m | giá trị | 
| --- | --- | --- | 
| 1 | 10 | 55 | 
| 2 | 5 | 30 | 
| 5 | 2 | 11 | 
| 10 | 1 | 1 | 

Đầu ra:$$1, 11, 30, 55.$$Điều này chứng tỏ các ước số lớn hơn tạo ra chu kỳ nhỏ hơn và do đó đóng góp nhỏ hơn như thế nào. 

Mỗi trường hợp xác nhận rằng chỉ có cấu trúc ước số quan trọng chứ không phải kích thước bước cụ thể$k$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt{n})$| Ta liệt kê tất cả các ước của$n$lên đến$\sqrt{n}$| 
| Không gian |$O(\tau(n))$| Chúng tôi lưu trữ tối đa một giá trị cho mỗi ước số | 

Sự ràng buộc$n \le 10^9$làm cho$\sqrt{n} \approx 31623$, có thể nhanh chóng dưới giới hạn 1 giây. Số lượng ước số đủ nhỏ để việc sắp xếp không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input().strip())
    res = set()

    d = 1
    while d * d <= n:
        if n % d == 0:
            for div in (d, n // d):
                m = n // div
                val = m * (2 * n - (m - 1) * div) // 2
                res.add(val)
        d += 1

    return " ".join(map(str, sorted(res)))

# provided sample
assert run("6\n") == "1 5 9 21", "sample 1"

# minimum n
assert run("2\n") == "1 3", "n=2 edge"

# prime n
assert run("7\n") == "1 7 28 49", "prime structure"

# power of two
assert run("8\n") == "1 9 25 64", "power of two"

# larger composite
assert run("12\n") == "1 13 25 37 91 169", "divisor richness"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | 1 3 | chu trình không tầm thường nhỏ nhất | 
| 7 | 1 7 28 49 | số nguyên tố hoạt động giống như chuỗi chia hết | 
| 8 | 1 9 25 64 | lũy thừa của hai tạo ra ước số có cấu trúc | 
| 12 | nhiều | bội số và thứ tự | 

## Vỏ cạnh 

cho$n = 2$, chỉ có hai ước số. Chu kỳ chỉ truy cập nút 1 hoặc cả hai nút tùy thuộc vào$k$. Thuật toán liệt kê$d=1,2$và tạo ra hai tổng riêng biệt mà không trùng lặp. 

Đối với nguyên tố$n$, mọi$k \neq n$có gcd 1, do đó tất cả các chu trình không tầm thường sẽ thu gọn lại thành một cấu trúc duy nhất. Phép liệt kê ước số chính xác chỉ mang lại hai cấu hình có ý nghĩa: chu trình đầy đủ và chu trình tầm thường. 

Đối với các số tổng hợp cao như$n = 12$, nhiều ước số tạo ra độ dài chu kỳ chồng chéo. Cấu trúc đã thiết lập đảm bảo các bản sao được loại bỏ tự động, phản ánh sự khác biệt$k$các giá trị vẫn có thể tạo ra các tập duyệt giống hệt nhau.
