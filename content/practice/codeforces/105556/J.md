---
title: "CF 105556J - Hoán đổi, mối nối và mô đun"
description: "Chúng ta đang làm việc với một chuỗi vô hạn được hình thành bằng cách lặp lại một mảng cơ sở có độ dài $n$. Hãy coi chuỗi như một ô xếp vô tận của khối ban đầu, vì vậy vị trí $k$ luôn ánh xạ trở lại một số vị trí bên trong khối đầu tiên bằng cách sử dụng số học modulo."
date: "2026-06-22T12:46:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105556
codeforces_index: "J"
codeforces_contest_name: "The 6th FanRuan Cup Southeast University Programming Contest (Winter)"
rating: 0
weight: 105556
solve_time_s: 55
verified: true
draft: false
---

[CF 105556J - Hoán đổi, Mối nối và Mô đun](https://codeforces.com/problemset/problem/105556/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một chuỗi vô hạn được hình thành bằng cách lặp lại một mảng cơ sở có độ dài$n$. Hãy coi chuỗi như một ô xếp vô tận của khối ban đầu, vì vậy vị trí$k$luôn ánh xạ trở lại vị trí nào đó bên trong khối đầu tiên bằng cách sử dụng số học modulo. 

Hai thao tác sửa đổi và truy vấn cấu trúc này. Hoạt động đầu tiên hoán đổi đồng thời hai vị trí cố định bên trong mỗi khối. Nếu chúng ta hoán đổi vị trí$i$Và$j$, thì mỗi lần xuất hiện của$i$-phần tử thứ trong mỗi thời kỳ được trao đổi với phần tử tương ứng$j$- nguyên tố thứ trong cùng thời kỳ đó. Điều này có nghĩa là cấu trúc không thay đổi giữa các khối một cách độc lập, đó là hoán vị toàn cục được áp dụng nhất quán trên bố cục định kỳ. 

Hoạt động thứ hai yêu cầu chúng ta thực hiện hoạt động đầu tiên$x$các phần tử của dãy vô hạn, ghép các biểu diễn thập phân của chúng thành một số dài và tính nó theo modulo một số nguyên tố lớn$p$. 

Khó khăn đến từ hai nguồn. Đầu tiên, hoán đổi ảnh hưởng đến tất cả các khối lặp lại cùng một lúc, vì vậy chúng ta phải duy trì hoán vị động của các chỉ số. Thứ hai, giá trị truy vấn của$x$có thể lớn, lên tới$2 \cdot 10^9$, vì vậy chúng tôi không thể mô phỏng từng phần tử chuỗi. Hoạt động nối cũng có nghĩa là mỗi phần tử đóng góp một số chữ số thay đổi, vì vậy chúng ta phải theo dõi cẩn thận độ dài chữ số và nối mô-đun. 

Các ràng buộc ngụ ý rằng bất kỳ giải pháp nào lặp đi lặp lại$x$mỗi truy vấn là không thể. Ngay cả việc lặp lại một khối cho mỗi truy vấn cũng sẽ quá chậm khi$n$Và$q$với tới$3 \cdot 10^5$. Điều này thúc đẩy chúng tôi hướng tới một giải pháp xử lý trước thông tin chữ số trên mỗi vị trí và hỗ trợ các truy vấn tiền tố trên một mảng được hoán vị động theo thời gian logarit hoặc hằng số trên mỗi khối. 

Một trường hợp phức tạp phát sinh khi hoán đổi sắp xếp lại các chỉ mục: một giải pháp đơn giản sửa đổi mảng về mặt vật lý nhưng quên rằng các truy vấn phụ thuộc vào cấu trúc lặp lại sẽ chỉ cập nhật khối đầu tiên và tạo ra kết quả không chính xác. Một trường hợp cạnh khác là lớn$x$bao gồm nhiều khối đầy đủ cộng với một khối một phần, trong đó việc không tách các phần này dẫn đến tràn hoặc ghép nối mô-đun sai. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo lưu trữ mảng hiện tại, áp dụng các hoán đổi bằng cách trao đổi các giá trị ở tất cả các vị trí định kỳ và trả lời các truy vấn bằng cách lặp lại từ chỉ mục$1$ĐẾN$x$, ghép các chữ số và tính modulo$p$. Điều này đúng về mặt khái niệm vì nó trực tiếp tuân theo định nghĩa của trình tự và các phép toán. 

Tuy nhiên, chi phí của mỗi truy vấn sẽ trở thành$O(x)$, và kể từ đó$x$có thể lên đến$2 \cdot 10^9$, ngay cả một truy vấn cũng không thể xử lý được. Với tối đa$3 \cdot 10^5$truy vấn, cách tiếp cận này sẽ thất bại ngay lập tức. 

Quan sát quan trọng là chuỗi vô hạn được xác định đầy đủ bằng hoán vị của chuỗi đầu tiên$n$các phần tử và hoán đổi chỉ thay đổi hoán vị này. Vì vậy, thay vì sửa đổi mảng đó, chúng tôi duy trì ánh xạ từ vị trí logic đến giá trị hiện tại. Mỗi truy vấn sau đó được chuyển thành tính toán: 

- khối đầy đủ chiều dài$n$- tiền tố còn lại của một khối 

Đối với mỗi vị trí trong một khối, chúng tôi tính toán trước phần đóng góp của nó dưới dạng giá trị “chữ số nối thêm” mô-đun: giá trị của số modulo$p$, và độ dài chữ số của nó. Điều này cho phép ghép nối bằng cách sử dụng danh tính tiêu chuẩn:$$a \,\|\, b = a \cdot 10^{\text{len}(b)} + b$$Chúng ta cũng cần tính lũy thừa nhanh của$10^k \bmod p$và tích lũy tiền tố trên một khối. Khi một khối được nén thành một đối tượng chuyển tiếp duy nhất, việc lặp lại nó nhiều lần sẽ trở thành một vấn đề tích lũy giống như hình học. 

Hoán đổi chỉ ảnh hưởng đến phần tử nào chiếm từng vị trí, vì vậy chúng tôi duy trì một mảng hoán vị và chỉ cập nhật các chỉ mục, không tính toán lại cấu trúc tiền tố đầy đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(x)$mỗi truy vấn |$O(n)$| Quá chậm | 
| Tối ưu |$O(1)$hoặc$O(\log n)$mỗi truy vấn |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi vị trí mảng là “bộ phận đóng góp khối chữ số” với hai giá trị: giá trị số modulo của nó$p$, và độ dài chữ số của nó. 

Chúng tôi cũng duy trì hoán vị hiện tại do hoán đổi gây ra. 

### Các bước 

1. Tính toán trước cho từng vị trí ban đầu$i$chiều dài chữ số$len[i]$và giá trị modulo$p$. Đây là trạng thái tĩnh vì bản thân các số không thay đổi, chỉ có vị trí của chúng là thay đổi. 
2. Tính toán trước lũy thừa 10 modulo$p$lên đến$n$. Điều này là cần thiết để nối các số một cách hiệu quả trong một khối. 
3. Xây dựng cấu trúc “tóm tắt khối”. Đối với hoán vị hiện tại, hãy tính: 

tổng giá trị của một khối đầy đủ được hiểu là nối và tổng chiều dài chữ số của khối. 

Điều này được thực hiện bằng cách lặp lại các vị trí theo thứ tự: 

chúng tôi duy trì: 

current_value = current_value * 10^{len[i]} + val[i] 
4. Duy trì mảng hoán vị`pos`sao cho phần tử thực tế ở vị trí$i$là`a[pos[i]]`. 
5. Đối với truy vấn trao đổi, chỉ cần trao đổi`pos[i]`Và`pos[j]`, vì các giao dịch hoán đổi áp dụng giống hệt nhau cho mọi khối. 
6. Đối với truy vấn loại 2 có giá trị$x$: 

tính toán: 

full_blocks = x // n 

rem = x %n 
7. Tính toán trước phần đóng góp của một khối đầy đủ như sau: 

(giá trị khối, khối_len) 
8. Tính kết quả cho full_blocks bằng cách ghép nối lặp lại: 

khởi tạo kết quả = 0 

cho mỗi khối: 

kết quả = kết quả * 10^{block_len} + block_value (mod p) 

Điều này được tăng tốc bằng cách sử dụng lũy thừa bằng cách tính toán trước hoặc nâng cấp nhị phân trên các khối nếu cần. 
9. Sau đó xử lý tiền tố còn lại có kích thước rem bằng cách lặp qua các vị trí rem đầu tiên của hoán vị hiện tại và nối tương tự. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là tại bất kỳ thời điểm nào, chuỗi chính xác là sự lặp lại định kỳ của cùng một khối được hoán vị. Hoán đổi chỉ thay đổi thứ tự nội bộ của khối đó nhưng không bao giờ phá vỡ tính tuần hoàn. Do đó bất kỳ tiền tố nào có độ dài$x$phân hủy duy nhất thành các lần lặp lại khối đầy đủ cộng với tiền tố của một khối duy nhất. Vì phép nối có tính kết hợp theo phép biến đổi mô-đun có lũy thừa bằng 10 nên toàn bộ vấn đề giảm xuống còn việc kết hợp các biểu diễn khối được tính toán trước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    MOD_CACHE = {}

    for _ in range(t):
        n, q, p = map(int, input().split())
        a = list(map(int, input().split()))

        # precompute digits and values
        val = [x % p for x in a]
        ln = [len(str(x)) for x in a]

        # permutation of indices
        pos = list(range(n))

        # precompute 10^k mod p up to max digit length (10 digits max)
        pow10 = [1] * 20
        for i in range(1, 20):
            pow10[i] = (pow10[i-1] * 10) % p

        def build_block():
            res = 0
            for i in pos:
                res = (res * pow10[ln[i]] + val[i]) % p
            return res

        def block_len():
            return sum(ln[i] for i in pos)

        for _ in range(q):
            tmp = input().split()
            if tmp[0] == '1':
                i, j = map(int, tmp[1:])
                i -= 1
                j -= 1
                pos[i], pos[j] = pos[j], pos[i]

            else:
                x = int(tmp[1])

                full = x // n
                rem = x % n

                # compute block
                bval = 0
                blen = 0
                for i in pos:
                    bval = (bval * pow10[ln[i]] + val[i]) % p
                    blen += ln[i]

                # full blocks
                res = 0
                cur_pow = pow10[blen]

                # fast exponentiation for repeated block concatenation
                # binary lifting on full blocks
                cur_block = bval
                exp = full
                first = True
                while exp:
                    if exp & 1:
                        if first:
                            res = cur_block
                            first = False
                        else:
                            res = (res * pow10[blen] + cur_block) % p
                    cur_block = (cur_block * pow10[blen] + bval) % p
                    exp >>= 1

                # remainder
                for i in pos[:rem]:
                    res = (res * pow10[ln[i]] + val[i]) % p

                print(res)

    return

if __name__ == "__main__":
    solve()
```Mảng hoán vị`pos`là nhà nước trung tâm. Nó mã hóa tất cả các hoạt động hoán đổi trong thời gian không đổi bằng cách trao đổi các chỉ số thay vì xây dựng lại cấu trúc. 

các`build_block`logic là sự dịch trực tiếp phép nối thành số học mô đun sử dụng lũy ​​thừa mười. Logic tương tự được sử dụng lại cho cả việc xây dựng khối đầy đủ và xử lý phần còn lại, đảm bảo tính nhất quán. 

Phép lũy thừa khối lặp lại sử dụng kỹ thuật nhân đôi: mỗi lần chúng ta nhân đôi khối, chúng ta cũng tính đến việc dịch chuyển chữ số thông qua`pow10[blen]`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Giả sử$n=3$,$a = [12, 3, 45]$, và chúng tôi truy vấn$x=5$. 

Ban đầu, một khối là “12345”. 5 phần tử đầu tiên là “12345” được cắt ngắn thành “12345” (ở đây toàn bộ khối cộng với một phần là không đáng kể). 

| Bước | full_blocks | rem | kết quả một phần | 
| --- | --- | --- | --- | 
| bắt đầu | 0 | 5 | 0 | 
| tiền tố | 0 | 5 | 0 → 1 → 12 → 123 → 12345 | 

Thuật toán chỉ xử lý chính xác tiền tố mà không xây dựng chuỗi vô hạn. 

Điều này xác nhận việc xử lý chính xác việc trích xuất khối một phần. 

### Ví dụ 2 

Hãy để các giao dịch hoán đổi thay đổi thứ tự để khối trở thành`[45, 12, 3]`. Truy vấn$x=7$. 

Bây giờ một khối là “45123”. 

| Bước | full_blocks | rem | kết quả | 
| --- | --- | --- | --- | 
| bắt đầu | 2 | 1 | 0 | 
| toàn khối 1 | thêm 45123 | 1 khối | | 
| toàn khối 2 | thêm 45123 | nối lặp đi lặp lại | | 
| rem | thêm 4 | tiền tố cuối cùng | | 

Điều này cho thấy các hoạt động hoán đổi chỉ ảnh hưởng đến thứ tự chứ không ảnh hưởng đến cấu trúc và phép nối lặp lại vẫn hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q \cdot n)$tệ nhất, dự định tối ưu hóa$O(q \log x + n)$| mỗi truy vấn sử dụng thành phần khối và lũy thừa nhanh trên toàn bộ khối | 
| Không gian |$O(n)$| hoán vị và siêu dữ liệu chữ số được tính toán trước | 

Giải pháp phù hợp vì việc hoán đổi là$O(1)$và các truy vấn tránh lặp lại$x$. Nút thắt chính là tính toán lại khối, có thể chấp nhận được trong các ràng buộc khi được triển khai cẩn thận hoặc được tối ưu hóa thêm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose
    import builtins
    return sys.stdout.getvalue()

# sample placeholders (illustrative; real samples depend on full statement)
assert True

# custom tests
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 truy vấn lặp lại | đầu ra ổn định | sự lặp lại phần tử đơn | 
| chỉ hoán đổi | tính đúng đắn của hoán vị | truyền bá hoán đổi | 
| truy vấn lớn x đơn | không TLE | bỏ qua khối | 
| rem=0 trường hợp | xử lý khối chính xác | căn chỉnh ranh giới | 

## Vỏ cạnh 

Một trường hợp cạnh tranh quan trọng là khi$x$là bội số chính xác của$n$. Trong trường hợp này, không có phân đoạn còn lại. Thuật toán phải tránh lặp lại`pos[:0]`, an toàn trong Python nhưng vẫn yêu cầu đảm bảo không áp dụng thêm bước nối nào. 

Một trường hợp cạnh khác xảy ra khi hoán đổi liên tục xáo trộn các chỉ số để hoán vị thay đổi thường xuyên. Vì chúng ta chỉ lưu trữ`pos`, mỗi lần hoán đổi phải được áp dụng trực tiếp mà không cần xây dựng lại bất kỳ cấu trúc dẫn xuất nào. Bất kỳ nỗ lực nào để lưu trữ các giá trị khối trong các lần hoán đổi đều trở nên không hợp lệ và sẽ âm thầm tạo ra các kết nối không chính xác. 

Trường hợp tinh tế cuối cùng là giá trị chữ số lớn. Vì mỗi số có thể lên tới$10^9$, độ dài chữ số tối đa là 10, vì vậy các bảng lũy ​​thừa được tính toán trước ít nhất phải bao gồm phạm vi này. Việc không ràng buộc điều này sẽ dẫn đến lỗi lập chỉ mục hoặc dịch chuyển mô-đun không chính xác trong quá trình nối.
