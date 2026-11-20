---
title: "CF 2132B - Con số bí mật"
description: "Chúng ta được cấp một số $n$ được xây dựng theo một cách rất cụ thể. Ai đó đã bí mật chọn một số nguyên $x$, sau đó tạo một số $y$ khác bằng cách thêm một hoặc nhiều số 0 vào bên phải của $x$. Điều đó có nghĩa là $y = x cdot 10^k$ đối với một số $k ge 1$."
date: "2026-06-08T02:49:29+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 2132
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1043 (Div. 3)"
rating: 900
weight: 2132
solve_time_s: 98
verified: false
draft: false
---

[CF 2132B - Con số bí mật](https://codeforces.com/problemset/problem/2132/B) 

**Xếp hạng:** 900 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một số$n$được xây dựng một cách rất cụ thể. Ai đó đã bí mật chọn một số nguyên$x$, sau đó tạo một số khác$y$bằng cách thêm một hoặc nhiều số 0 vào bên phải của$x$. Điều đó có nghĩa$y = x \cdot 10^k$đối với một số người$k \ge 1$. Cuối cùng, họ chỉ tiết lộ số tiền$n = x + y$. Nhiệm vụ của chúng tôi là khôi phục mọi bản gốc có thể$x$có thể tạo ra cái đã cho$n$. 

Vì vậy, cấu trúc không phải là phép cộng tùy ý. Nó luôn có dạng$$n = x + x \cdot 10^k = x(1 + 10^k)$$đối với một số nguyên$k \ge 1$, Và$x$phải là số nguyên dương. Chúng tôi được yêu cầu tìm tất cả những điều hợp lệ như vậy$x$cho từng trường hợp thử nghiệm. 

Những ràng buộc cho phép$n$lên đến$10^{18}$và lên đến$10^4$trường hợp thử nghiệm. Điều đó ngay lập tức loại trừ bất kỳ điều gì cố gắng phân chia các chữ số có thể có hoặc các ứng cử viên bạo lực để giành được$x$lên đến$n$. Thậm chí$O(n)$mỗi bài kiểm tra là không thể, và thậm chí$O(\sqrt{n})$mỗi bài kiểm tra sẽ quá chậm trong trường hợp xấu nhất. 

Một trường hợp cạnh tinh tế xuất phát từ thực tế là việc xây dựng phụ thuộc vào công suất mười ca. Nếu chúng ta cố gắng đoán$k$không chính xác hoặc giả định các chữ số có độ dài cố định, chúng ta có thể bỏ sót các giải pháp hợp lệ hoặc đếm quá nhiều các giải pháp không hợp lệ. 

Ví dụ, nếu$n = 1111$, câu trả lời hợp lệ tồn tại: 

-$x = 11$, từ$11 + 1100 = 1111$-$x = 101$, từ$101 + 1010 = 1111$Một nỗ lực ngây thơ chỉ kiểm tra các kết quả khớp tiền tố-hậu tố có độ dài bằng nhau sẽ bị bỏ lỡ$x = 11$. Ngược lại, kiểm tra tất cả các chuỗi con là ứng cử viên cho$x$không thực thi ràng buộc đại số sẽ tạo ra nhiều ứng viên không hợp lệ. 

## Phương pháp tiếp cận 

Một chiến lược bạo lực trực tiếp sẽ là thử mọi cách có thể$x$từ$1$ĐẾN$n$, tính toán$y = n - x$, và kiểm tra xem$y$bằng với$x$với một số số 0 ở cuối. Điều này có tác dụng vì nó trực tiếp thực thi định nghĩa, nhưng nó yêu cầu$O(n)$kiểm tra từng trường hợp thử nghiệm, điều này là không thể khi$n$có thể lớn như$10^{18}$. 

Ngay cả khi chúng ta tối ưu hóa bằng cách chỉ lặp qua các tiền tố của các chữ số của$n$, chúng tôi vẫn cần xác thực từng ứng cử viên bằng phép chia hoặc tái cấu trúc chuỗi và số lượng ứng cử viên vẫn tỷ lệ thuận với số chữ số nhân với số lần dịch chuyển có thể. 

Nhận xét quan trọng nhất là cấu trúc$n = x(1 + 10^k)$biến bài toán thành bài toán tìm ước chia với dạng rất đặc biệt. Đối với mỗi cố định$k$, giá trị$(1 + 10^k)$được biết và$x$phải chính xác$n / (1 + 10^k)$nếu chia được. Từ$10^k$tăng theo cấp số nhân, số lượng hợp lệ$k$được giới hạn bởi số chữ số của$n$, nhiều nhất là 18. Điều này làm giảm vấn đề chỉ còn kiểm tra một nhóm nhỏ ứng viên. 

Do đó chúng ta có thể liệt kê tất cả những gì có thể$k$, tính mẫu số ứng cử viên$d = 10^k + 1$, và kiểm tra xem$n$chia hết cho$d$. Nếu vậy, chúng tôi phục hồi$x$. Điều này hiệu quả vì không gian tìm kiếm có tính logarit$n$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên x |$O(n)$|$O(1)$| Quá chậm | 
| Liệt kê k (lũy thừa 10) |$O(\log n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$và chuẩn bị một danh sách trống các câu trả lời. Chúng tôi sẽ thu thập tất cả hợp lệ$x$các giá trị. 
2. Lặp lại các giá trị có thể có của$k$, bắt đầu từ 1 lúc$10^k < n$. Mỗi$k$biểu thị có bao nhiêu số 0 được thêm vào khi hình thành$y = x \cdot 10^k$. 
3. Đối với mỗi$k$, tính toán$d = 10^k + 1$. Điều này xuất phát trực tiếp từ việc viết lại phương trình$n = x(1 + 10^k)$. 
4. Kiểm tra xem$n \mod d = 0$. Nếu không, không hợp lệ$x$tồn tại cho việc này$k$, vậy hãy tiếp tục. 
5. Nếu chia hết thì tính$x = n / d$. Đây là một giải pháp ứng cử viên. 
6. Cửa hàng$x$nếu nó là tích cực. Từ$d > 1$, tính tích cực được đảm bảo khi phép chia chính xác, nhưng chúng tôi vẫn kiểm tra về mặt khái niệm. 
7. Sau khi thử tất cả$k$, sắp xếp những gì đã thu thập được$x$giá trị và xuất chúng. Việc sắp xếp là cần thiết vì khác nhau$k$các giá trị có thể tạo ra kết quả theo thứ tự không đơn điệu. 

### Tại sao nó hoạt động 

Mọi cách xây dựng hợp lệ đều thỏa mãn$n = x(1 + 10^k)$đối với một số nguyên$k \ge 1$. Ngược lại, bất kỳ$k$vì cái gì$1 + 10^k$chia rẽ$n$mang lại một ứng cử viên$x$tái tạo lại một giá trị hợp lệ$y = x \cdot 10^k$. Thuật toán liệt kê tất cả những gì có thể$k$và với mỗi cái, nó sẽ từ chối hoặc xây dựng lại chính xác một cái có thể$x$. Không có giải pháp hợp lệ nào có thể bị bỏ qua vì mọi biểu diễn hợp lệ đều tương ứng với chính xác một giải pháp hợp lệ như vậy.$k$và không thể thêm nghiệm không hợp lệ vì tính chia hết thực thi nghiêm ngặt phương trình ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        res = []

        pow10 = 10
        k = 1

        while pow10 < n:
            d = pow10 + 1
            if n % d == 0:
                x = n // d
                res.append(x)
            pow10 *= 10
            k += 1

        if not res:
            print(0)
        else:
            res.sort()
            print(len(res))
            print(*res)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo quan sát chỉ có lũy thừa của mười vật chất. Chúng tôi duy trì$10^k$tăng dần thay vì tính toán lại mỗi lần, điều này tránh được chi phí lũy thừa không cần thiết. 

Vòng lặp dừng khi$10^k \ge n$bởi vì ngoài điểm đó$1 + 10^k$đã vượt quá$n$, làm cho không thể chia hết được. 

Sắp xếp ở cuối đảm bảo thứ tự đầu ra đúng dù lớn hơn$k$không nhất thiết phải tương ứng với nhỏ hơn$x$. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 1111$Chúng tôi kiểm tra giá trị tăng dần của$k$. 

| k | 10^k | d = 10^k + 1 | n mod d | x = n/d | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 10 | 11 | 0 | 101 | vâng | 
| 2 | 100 | 101 | 0 | 11 | vâng | 
| 3 | 1000 | 1001 | 1111 mod 1001 ≠ 0 | - | không | 

Các giá trị được thu thập là$\{101, 11\}$, mà sau khi sắp xếp sẽ cho$11, 101$. 

Điều này chứng tỏ sự khác biệt$k$các giá trị tương ứng với các phân tách hợp lệ khác nhau của cùng một số. 

### Ví dụ 2:$n = 12$| k | 10^k | d | n mod d | x | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 10 | 11 | 12 mod 11 = 1 | - | không | 

Không còn nữa$k$là hợp lệ bởi vì$100 > 12$. Do đó, đầu ra trống. 

Điều này khẳng định rằng các số không chia hết cho bất kỳ$10^k + 1$không có sự phân rã hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t \log_{10} n)$| Đối với mỗi trường hợp thử nghiệm, chúng tôi thử nhiều nhất một$k$mỗi chữ số, tối đa 18 lần lặp | 
| Không gian |$O(1)$thêm | Chỉ một danh sách nhỏ thí sinh được lưu trữ cho mỗi bài kiểm tra | 

Thuật toán đủ nhanh vì mỗi phép thử chỉ thực hiện một số phép tính số học và thậm chí$10^4$các trường hợp thử nghiệm vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        res = []

        pow10 = 10
        while pow10 < n:
            d = pow10 + 1
            if n % d == 0:
                res.append(n // d)
            pow10 *= 10

        if not res:
            out.append("0")
        else:
            res.sort()
            out.append(str(len(res)))
            out.append(" ".join(map(str, res)))

    return "\n".join(out)

# provided samples
assert run("""5
1111
12
55
999999999999999999
1000000000000000000
""") == """2
11 101
0
1
5
3
999999999 999000999000999 90909090909090909
0"""

# custom cases
assert run("""3
11
1010
1001
""") == """1
1
1
10
1
1"""

assert run("""1
1000000000000000000
""") == """0"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Các trường hợp khớp chính xác nhỏ | Phân rã hợp lệ duy nhất | tính đúng đắn của phép chia cơ bản | 
| Ranh giới sức mạnh mười lớn | 0 | đảm bảo điều kiện dừng hoạt động | 
| Số cấu trúc hỗn hợp | nhiều ứng cử viên hoặc không có | tính chính xác của phép liệt kê | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi$n$chính xác là lũy thừa của mười hoặc gần bằng nó. Ví dụ,$n = 1000000000000000000$. Thuật toán lặp lại$k$, nhưng mọi$10^k + 1$hoặc vượt quá$n$hoặc không phân chia nó. Vòng lặp dừng sớm và không có ứng cử viên nào được tạo ra, xuất ra đúng 0. 

Một trường hợp cạnh khác là khi nhiều giá trị của$k$tạo ra kết quả hợp lệ. Vì$n = 1111$, cả hai$k = 1$Và$k = 2$công việc. Thuật toán thu thập cả hai một cách tự nhiên vì mỗi$k$được kiểm tra độc lập và việc sắp xếp đảm bảo thứ tự đầu ra chính xác. 

Trường hợp tinh tế cuối cùng là khi tính chia hết được giữ nguyên nhưng kết quả tính toán$x$không có ý nghĩa theo cách xây dựng ban đầu. Điều này không thể xảy ra vì$n \mod (10^k + 1) = 0$ngụ ý tái thiết đại số chính xác$n = x(1 + 10^k)$, xác định duy nhất một giá trị hợp lệ$y = x \cdot 10^k$, bảo toàn cấu trúc cần thiết mà không cần kiểm tra bổ sung.
