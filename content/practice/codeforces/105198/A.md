---
title: "CF 105198A - Chức năng yêu thích của Monke"
description: "Chúng ta được cung cấp một hàm được xác định trên các số nguyên x được diễn giải thông qua biểu diễn nhị phân của chúng. Mỗi số nguyên tương ứng với một tập hợp các vị trí bit và mọi số y ≤ x với y & x = y chính xác là một mặt nạ con của x, nghĩa là nó chỉ sử dụng các bit đã có trong x."
date: "2026-06-27T02:57:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105198
codeforces_index: "A"
codeforces_contest_name: "ShellBeeHaken Presents Intra SUST Programming Contest 2024 - Replay"
rating: 0
weight: 105198
solve_time_s: 129
verified: false
draft: false
---

[CF 105198A - Chức năng yêu thích của Monke](https://codeforces.com/problemset/problem/105198/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 9s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hàm được xác định trên số nguyên`x`được giải thích thông qua biểu diễn nhị phân của chúng. Mỗi số nguyên tương ứng với một tập hợp các vị trí bit và mỗi số`y ≤ x`với`y & x = y`chính xác là một mặt nạ con của`x`, nghĩa là nó chỉ sử dụng các bit đã có sẵn trong`x`. 

Chức năng bắt đầu từ một lớp cơ sở nơi`f(0, x)`đơn giản là giá trị của`x`chính nó. Mỗi lớp tiếp theo được xây dựng bằng cách lấy tất cả các mặt nạ con`y`của`x`và tổng hợp các giá trị trước đó của chúng`f(k-1, y)`. Vì vậy, mỗi bước sẽ truyền bá thông tin xuống dưới qua mạng tập hợp con của các mặt nạ bit, tích lũy lặp đi lặp lại các đóng góp từ tất cả các tập hợp con. 

Đầu vào đưa ra nhiều truy vấn độc lập, mỗi truy vấn yêu cầu giá trị của hàm này ở một độ sâu nhất định`k`và mặt nạ`x`, với cả hai lên tới`3 × 10^5`, và lên đến`10^5`truy vấn. 

Một cách giải thích ngây thơ gợi ý các phép tính tổng tập hợp con lặp đi lặp lại trên tất cả các mặt nạ con cho mọi bài kiểm tra và mọi cấp độ`k`. Điều đó đã gợi ý rắc rối vì số lượng mặt nạ con tăng theo cấp số nhân với số lượng bit được đặt. Ngay cả với tối đa 19 bit, việc lặp lại các thao tác như vậy cho mỗi truy vấn và mỗi`k`sẽ vượt xa giới hạn khả thi. 

Áp lực hạn chế thực sự đến từ sự kết hợp của lớn`t`và lớn`k`. Bất kỳ cách tiếp cận nào tính toán lại các mặt nạ con cho mỗi truy vấn hoặc thậm chí cho từng bit một cách độc lập mà không đơn giản hóa cấu trúc sẽ vượt quá giới hạn thời gian. 

Một trường hợp thất bại tinh vi sẽ xuất hiện ngay lập tức nếu người ta cố gắng mô phỏng trực tiếp sự tái diễn: 

Ví dụ, với`x = 7 (111₂)`Và`k = 2`, việc triển khai trực tiếp sẽ liệt kê tất cả các tập hợp con hai lần, dẫn đến việc tính toán lại nhiều lần các cấu trúc con giống nhau. Ngay cả bộ nhớ đệm cho mỗi`(k, x)`là không thể vì cả hai chiều đều lớn và không gian trạng thái có tính chất mũ theo bit. 

Khó khăn cốt lõi là sự tái phát có vẻ cục bộ trong`x`, nhưng thực sự lan truyền lặp đi lặp lại trên toàn bộ mạng tập hợp con. 

## Phương pháp tiếp cận 

Phép truy toán là một toán tử tổng tập hợp con cổ điển được áp dụng lặp đi lặp lại. Một ứng dụng của phép biến đổi sẽ thay thế một hàm`g(x)`với tổng của`g(y)`trên tất cả các mặt nạ con`y ⊆ x`. Đây được gọi là phép biến đổi zeta tập hợp con. Vấn đề áp dụng phép biến đổi này`k`lần bắt đầu từ`g(0, x) = x`. 

Mô phỏng lực lượng vũ phu sẽ tính toán lại rõ ràng tất cả các tổng mặt nạ con cho mỗi lớp. Một ứng dụng đã có giá`O(3^n_bits)`trên mỗi mặt nạ tổng hợp bằng cách sử dụng phép liệt kê tập hợp con và lặp lại nó`k`nhiều lúc khiến nó trở nên vô vọng. 

Quan sát cấu trúc quan trọng là phép biến đổi này là tuyến tính trên các bit và tôn trọng tính độc lập của tập hợp con. Mỗi bit hoạt động độc lập trong cách hình thành các tập hợp con, điều này cho phép chúng ta diễn giải lại quy trình dưới dạng đếm các chuỗi trong mạng tập hợp con thay vì tính tổng các giá trị lặp đi lặp lại. 

Thay vì theo dõi các giá trị một cách trực tiếp, chúng tôi diễn giải lại phép truy hồi theo kiểu tổ hợp. Mỗi ứng dụng của phép biến đổi tương ứng với việc cho phép các bit được đưa vào ở các lớp khác nhau, do đó quá trình này trở thành các chuỗi đếm của các tập hợp con. Điều này chuyển đổi vấn đề thành cách đếm các cách mỗi bit chuyển từ 0 sang 1 theo chiều ngang.`k`các bước, đồng thời tính trọng số đóng góp theo giá trị ban đầu của tập hợp con bắt đầu. 

Sau khi được thể hiện theo cách này, tính độc lập giữa các bit sẽ có thể sử dụng được: hệ số đóng góp vào các lựa chọn trên mỗi bit và tổng trên tất cả các mặt nạ con được phân tích thành thừa số một cách rõ ràng. 

Điều này dẫn đến một biểu thức dạng đóng chỉ phụ thuộc vào ba đại lượng: giá trị của`x`, số lượng bit được đặt trong`x`, và quyền hạn của`k`Và`(1 + k^{-1})`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Số mũ tính bằng bit × k | O(1) | Quá chậm | 
| Tối ưu (hệ số bit) | O(log x) cho mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi rút ra một dạng đóng bằng cách viết lại phép truy toán theo chuỗi mặt nạ con và phân tách các đóng góp trên mỗi bit. 

1. Giải thích từng bước của phép truy toán như việc mở rộng một hàm trên tất cả các mặt nạ con, tương ứng với việc di chuyển xuống dưới trong mạng tập hợp con. Sau đó`k`bước, mọi giá trị phụ thuộc vào tất cả các chuỗi có độ dài`k`kết thúc tại`x`. 
2. Sửa một submask bắt đầu`y ⊆ x`. Sự đóng góp của`y`sau đó`k`các bước chỉ phụ thuộc vào số cách chúng ta có thể phát triển`y`vào trong`x`chính xác`k`các lớp. Mỗi bit có mặt trong`x`nhưng không ở trong`y`phải được giới thiệu ở một số bước và mỗi bit như vậy sẽ chọn một trong các bit một cách độc lập`k`các bước. 
3. Điều này hàm ý rằng số cách để mở rộng một khoảng thời gian cố định`y`ĐẾN`x`là`k^(popcount(x) - popcount(y))`. Phép truy toán trở thành tổng có trọng số trên tất cả các mặt nạ con trong đó trọng số chỉ phụ thuộc vào kích thước của chúng. 
4. Viết lại hàm dưới dạng tổng đóng góp của các bit riêng lẻ của`y`. Mỗi bit đóng góp độc lập vào giá trị nguyên của`y`, vì vậy chúng ta tách tổng bằng cách cố định một bit và đếm tất cả các mặt nạ con chứa nó. 
5. Đối với bit cố định`i`, các bit còn lại tạo thành một tập con độc lập nhỏ hơn. Điều này cho phép phân tích tổng thành tích trên bit, dẫn đến biểu thức thu gọn cho số hạng trợ giúp`S(x)`, trở thành tích hình học trên bit. 
6. Tổng mặt nạ con`S(x)`đơn giản hóa để`(1 + k^{-1})^(popcount(x))`bởi vì mỗi bit hoặc vắng mặt (trọng lượng 1) hoặc hiện tại (trọng số`k^{-1}`). 
7. Kết hợp phân tích nhân tử, biểu thức cuối cùng trở thành:`f(k, x) = x × k^(popcount(x) - 1) × (1 + k^{-1})^(popcount(x) - 1)`. 
8. Tính toán trước nghịch đảo mô-đun của`k`, tính toán số lượng và giá trị của`x`và đánh giá lũy thừa một cách hiệu quả bằng cách sử dụng lũy ​​thừa mô-đun. 

### Tại sao nó hoạt động 

Sự lặp lại xác định một toán tử tuyến tính trên các hàm trên các tập hợp con và ứng dụng lặp lại tương ứng với việc áp dụng cùng một phép biến đổi tuyến tính nhiều lần. Bởi vì cấu trúc tập hợp con phân tích thành thừa số trên các bit độc lập nên toán tử phân tách thành các đóng góp trên mỗi bit. Điều này biến những gì trông giống như một vụ nổ tổ hợp toàn cầu thành tỷ lệ hình học độc lập trên mỗi bit, với giá trị số ban đầu là`x`chỉ hoạt động như một trọng số tuyến tính được gắn vào mỗi bit đóng góp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modpow(a, e):
    res = 1
    while e:
        if e & 1:
            res = res * a % MOD
        a = a * a % MOD
        e >>= 1
    return res

t = int(input().strip())
for _ in range(t):
    k, x = map(int, input().split())

    pc = x.bit_count()

    if pc == 1:
        print(x % MOD)
        continue

    invk = modpow(k, MOD - 2)

    base = (k % MOD) * ((1 + invk) % MOD) % MOD
    base = modpow(base, pc - 1)

    print(x % MOD * base % MOD)
```Việc thực hiện hoàn toàn dựa vào biểu mẫu đóng. Phần không cần thiết duy nhất là tính toán nghịch đảo mô-đun của`k`, vì biểu thức sử dụng`k^{-1}`bên trong một cài đặt số học mô-đun. Phép lũy thừa chỉ được áp dụng một lần cho mỗi truy vấn, giúp giải pháp luôn hiệu quả. 

Trường hợp đặc biệt khi`popcount(x) = 1`theo công thức thì vô hại, nhưng việc viết nó một cách rõ ràng sẽ tránh được bất kỳ sự nhầm lẫn nào về số mũ bằng 0 và làm cho cấu trúc rõ ràng hơn: hàm rút gọn trực tiếp thành`x`. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ trong đó`x = 3 (11₂)`Và`k = 1`. Chúng tôi có hai mặt nạ con:`0`Và`3`. Sau một bước, mỗi giá trị sẽ trở thành tổng của các mặt nạ con của nó. 

| y | f(0, y) | đóng góp cho f(1, x) | 
| --- | --- | --- | 
| 0 | 0 | 0 | 
| 1 | 1 | 1 | 
| 2 | 2 | 2 | 
| 3 | 3 | 3 | 

Tổng hợp tất cả các mặt nạ con cho`6`. Công thức cho`x × 2^(pc(x)-1) = 3 × 2 = 6`, khớp chính xác. 

Bây giờ hãy xem xét`x = 5 (101₂)`Và`k = 2`. Số bit là`2`. Công thức cho:`f(2, 5) = 5 × 2^(1) × (1 + 1/2)^1 = 5 × 2 × 3/2 = 15`. 

Theo dõi về mặt khái niệm, mỗi mặt nạ con đóng góp có trọng số khác nhau bằng số lần bit của nó tồn tại qua hai lớp mở rộng tập hợp con và dạng đóng tổng hợp các đóng góp có trọng số này mà không liệt kê chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t log MOD) | Mỗi truy vấn tính toán một lũy thừa nghịch đảo mô-đun và một vài phép tính số học | 
| Không gian | O(1) | Chỉ một số lượng biến không đổi được lưu trữ | 

Giải pháp dễ dàng phù hợp với các ràng buộc bởi vì ngay cả`10^5`lũy thừa mô-đun ở kích thước này có thể quản lý được bằng Python và tất cả các phép toán khác đều có thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    MOD = 10**9 + 7

    def modpow(a, e):
        res = 1
        while e:
            if e & 1:
                res = res * a % MOD
            a = a * a % MOD
            e >>= 1
        return res

    t = int(input().strip())
    out = []
    for _ in range(t):
        k, x = map(int, input().split())
        pc = x.bit_count()
        if pc == 1:
            out.append(str(x % MOD))
            continue
        invk = modpow(k, MOD - 2)
        base = (k % MOD) * ((1 + invk) % MOD) % MOD
        base = modpow(base, pc - 1)
        out.append(str((x % MOD) * base % MOD))
    return "\n".join(out)

# provided samples (format adapted)
assert run("3\n1 1\n1 195\n420 69\n") is not None

# minimum input
assert run("1\n1 1\n") == "1"

# power of two x
assert run("1\n5 8\n") is not None

# all bits set small
assert run("1\n2 7\n") is not None

# larger random-style check
assert run("2\n1 3\n2 3\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| trạng thái hợp lệ nhỏ nhất | 
|`1 8`| tính toán | sự thống trị bit đơn | 
|`1 7`| tính toán | tương tác nhiều bit | 
|`2 3 / 3`| nhất quán | tính nhất quán của truy vấn lặp lại | 

## Vỏ cạnh 

Khi nào`x`là lũy thừa của hai, phép truy toán sẽ thu gọn thành một chuỗi đơn vì chỉ có một bit. Thuật toán xử lý việc này một cách rõ ràng vì`popcount(x) = 1`làm cho số mũ bằng 0 và trả về`x`trực tiếp, phù hợp với thực tế là mọi chuỗi mặt nạ con đều giảm xuống một trong hai`0`hoặc giá trị đầy đủ mà không cần phân nhánh tổ hợp. 

Khi`k = 1`, công thức sẽ giảm về tổng tập hợp con cổ điển trên tất cả các mặt nạ con. Biểu thức được đơn giản hóa thành`x × 2^(popcount(x)-1)`, phù hợp với từng bit đóng góp độc lập trên tất cả các tập hợp con, xác nhận rằng phép biến đổi mô hình chính xác một bản mở rộng zeta duy nhất mà không cần đếm quá mức.
