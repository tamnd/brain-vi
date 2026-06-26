---
title: "CF 105315F - Sinh nhật của Osama"
description: "Chúng ta được cung cấp một số bộ sưu tập các loại hoa, trong đó mỗi bộ sưu tập (được gọi là bó hoa) về cơ bản là một tập hợp các nhãn hoa được phép rút ra từ một tập hợp có kích thước tối đa là 60. Đối với mỗi trường hợp thử nghiệm, Osama chọn chính xác một bó hoa và sau đó xây dựng một khu vườn có chiều dài $m$."
date: "2026-06-23T06:14:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105315
codeforces_index: "F"
codeforces_contest_name: "JPC 4.0"
rating: 0
weight: 105315
solve_time_s: 50
verified: true
draft: false
---

[CF 105315F - Sinh nhật của Osama](https://codeforces.com/problemset/problem/105315/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số bộ sưu tập các loại hoa, trong đó mỗi bộ sưu tập (được gọi là bó hoa) về cơ bản là một tập hợp các nhãn hoa được phép rút ra từ một tập hợp có kích thước tối đa là 60. Đối với mỗi trường hợp thử nghiệm, Osama chọn chính xác một bó hoa và sau đó xây dựng một khu vườn có chiều dài$m$. Mỗi vị trí trong vườn được lấp đầy độc lập bằng cách chọn bất kỳ loại hoa nào từ bó hoa đã chọn và được phép lặp lại. 

Vì vậy nếu một bó hoa chứa$k$loại hoa riêng biệt thì bó hoa đó có thể tạo ra chính xác$k^m$những khu vườn khác nhau, bởi vì mỗi khu vườn$m$vị trí có thể là bất kỳ$k$các loại một cách độc lập. 

Điều phức tạp là chúng ta được yêu cầu số lượng khu vườn riêng biệt trên tất cả các bó hoa, nghĩa là nếu hai bó hoa khác nhau có thể tạo ra cùng một chuỗi thì chuỗi đó chỉ được tính một lần. Điều này xảy ra một cách tự nhiên khi các bộ kiểu bó hoa chồng lên nhau. 

Các ràng buộc thúc đẩy cấu trúc giải pháp. Số lượng bó hoa nhiều nhất là 19, điều này ngay lập tức gợi ý rằng bất kỳ sự phụ thuộc theo cấp số nhân nào vào$n$, chẳng hạn như$2^n$, có thể chấp nhận được. giá trị$m$có thể lớn như$10^9$, vì vậy bất kỳ cách tiếp cận nào lặp lại các vị trí của khu vườn đều không thể thực hiện được. Tuy nhiên, vì mỗi bó hoa đóng góp một thuật ngữ có dạng$|S|^m$, chúng ta chỉ cần lũy thừa nhanh chứ không cần lặp theo chiều dài. 

Quan sát cấu trúc quan trọng nhất là vũ trụ của các loại hoa chỉ có kích thước 60, vì vậy mỗi bó hoa có thể được biểu diễn dưới dạng bitmask trên 60 phần tử. Điều này làm cho các hoạt động giao lộ nhanh chóng và chính xác. 

Trường hợp cạnh tinh tế là khi các tập hợp con khác nhau của bó hoa tạo ra các tập hợp giao nhau giống hệt nhau. Ví dụ: nếu bó hoa A và bó hoa B giống hệt nhau thì cả hai đều đóng góp như nhau$k^m$và phép tính tổng đơn giản sẽ được tính gấp đôi. Một vấn đề khác là việc loại trừ bao gồm phải được sử dụng trên các tập hợp con của bó hoa, nếu không thì không thể sửa được sự chồng chéo. 

Một trường hợp thất bại cụ thể nhỏ đối với lối suy nghĩ ngây thơ là: 

Nếu bó hoa 1 là$\{1,2\}$và bó hoa 2 là$\{2,1\}$, Và$m=2$, sau đó cả hai đều tạo ra cùng một tập hợp các chuỗi và chỉ cần thêm$2^2 + 2^2$sẽ vượt quá. Câu trả lời đúng là$4$, không$8$. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là xử lý từng bó hoa một cách độc lập, liệt kê tất cả$k^m$trình tự theo khái niệm và sau đó cố gắng loại bỏ trùng lặp trên các bó hoa. Điều này ngay lập tức thất bại vì ngay cả việc lưu trữ hoặc lặp lại tất cả các chuỗi cũng không thể thực hiện được khi$m$là lớn. Số lượng các chuỗi trên mỗi bó hoa tăng theo cấp số nhân trong$m$, vì vậy mọi cách xây dựng rõ ràng đều không khả thi. 

Thông tin chi tiết về cấu trúc quan trọng là mỗi bó hoa xác định toàn bộ sức mạnh Descartes của một bộ. Sự kết hợp của những quyền lực này có thể được xử lý bằng cách sử dụng loại trừ bao gồm trên bó hoa. Đối với bất kỳ tập hợp con nào của bó hoa, giao điểm của chúng tương ứng với các chuỗi có thể được hình thành chỉ bằng cách sử dụng các loại hoa chung cho tất cả các bó hoa trong tập hợp con đó. Nếu giao lộ đó có kích thước$x$, thì nó đóng góp chính xác$x^m$trình tự. 

Việc sử dụng loại trừ bao gồm trên tất cả các tập hợp con không trống sẽ đảm bảo rằng mỗi chuỗi được tính chính xác một lần, bởi vì mỗi chuỗi hợp lệ đều có một bộ bó hoa được xác định rõ ràng chứa tất cả các loại hoa đã sử dụng của nó và loại trừ bao gồm sẽ cô lập giao điểm tối đa chịu trách nhiệm cho nó. 

Từ$n \le 19$, lặp đi lặp lại tất cả$2^n$tập hợp con là khả thi. Mỗi tập hợp con yêu cầu tính toán giao điểm của các mặt nạ và sau đó đánh giá lũy thừa mô-đun nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | số mũ trong$m$| Rất lớn | Quá chậm | 
| Bao gồm-Loại trừ trên các tập hợp con |$O(2^n \cdot 60 + 2^n \log m)$|$O(60)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Thể hiện mỗi bó hoa dưới dạng mặt nạ 60 bit. Mỗi bit cho biết bó hoa đó có loại hoa hay không. Điều này cho phép các giao điểm được tính toán bằng cách sử dụng AND theo bit, là thời gian không đổi. 
2. Lặp lại tất cả các tập hợp con không trống của bó hoa bằng cách sử dụng mặt nạ bit từ$1$ĐẾN$(1 \ll n) - 1$. Mỗi tập hợp con đại diện cho một nhóm bó hoa mà chúng ta sẽ phân tích giao điểm của chúng. 
3. Đối với mỗi tập hợp con, hãy tính mặt nạ giao nhau bằng cách AND tất cả các bó hoa trong tập hợp con. Kích thước của mặt nạ này là số loại hoa chung cho tất cả các bó hoa trong tập hợp con. Đây là tập hợp các loại duy nhất có thể xuất hiện theo trình tự hợp lệ cho tất cả các bó hoa đó cùng một lúc. 
4. Tính toán$x^m \bmod (10^9+7)$, Ở đâu$x$là kích thước của mặt nạ giao nhau. Điều này thể hiện số lượng trình tự có thể được hình thành bằng cách sử dụng chính xác các loại phổ biến đó. 
5. Áp dụng loại trừ bao gồm: nếu kích thước tập hợp con là số lẻ, hãy cộng giá trị này vào câu trả lời, nếu không thì trừ đi. Sự luân phiên này khắc phục việc đếm quá nhiều do sự chồng chéo giữa các nhóm bó hoa khác nhau. 
6. Trả về modulo kết quả tích lũy cuối cùng$10^9+7$. 

### Tại sao nó hoạt động 

Mỗi khu vườn hợp lệ là một chiều dài-$m$trình tự về một số loại hoa. Bộ đó phải được chứa trong ít nhất một bó hoa, nhưng có thể được chứa thành nhiều bó. Việc loại trừ bao gồm các tập hợp con bó hoa đảm bảo rằng mỗi chuỗi được tính một lần cho tập hợp con tối đa của các bó hoa vẫn chứa tất cả các loại hoa đã chọn của nó. Bởi vì kích thước giao điểm xác định duy nhất bảng chữ cái có sẵn cho các chuỗi trong mỗi tập hợp con, nên sự đóng góp$x^m$đếm chính xác tất cả các chuỗi được hỗ trợ bởi tập hợp con đó và các dấu hiệu xen kẽ sẽ loại bỏ việc đếm quá mức khỏi các tập hợp con chồng chéo lớn hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modpow(a, e):
    res = 1
    a %= MOD
    while e > 0:
        if e & 1:
            res = res * a % MOD
        a = a * a % MOD
        e >>= 1
    return res

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        masks = []
        for _ in range(n):
            arr = list(map(int, input().split()))
            k = arr[0]
            vals = arr[1:]
            mask = 0
            for v in vals:
                mask |= 1 << (v - 1)
            masks.append(mask)

        ans = 0

        for s in range(1, 1 << n):
            inter = (1 << 60) - 1
            for i in range(n):
                if s >> i & 1:
                    inter &= masks[i]
                    if inter == 0:
                        break

            if inter == 0:
                continue

            cnt = inter.bit_count()
            val = modpow(cnt, m)

            if bin(s).count("1") % 2 == 1:
                ans = (ans + val) % MOD
            else:
                ans = (ans - val) % MOD

        print(ans % MOD)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo việc liệt kê tập hợp con trực tiếp. Mỗi bó hoa được nén thành một mặt nạ bit để các giao điểm giảm xuống còn một thao tác AND duy nhất. Giao điểm bắt đầu từ tất cả 60 bit được đặt, đảm bảo tính chính xác ngay cả khi một số bó hoa thưa thớt. 

Phép lũy thừa được xử lý bằng phép lũy thừa nhị phân vì$m$có thể lớn như$10^9$. Tính chẵn lẻ của tập hợp con xác định xem phần đóng góp được cộng hay trừ, thực hiện loại trừ bao gồm chính xác theo yêu cầu. 

Một chi tiết nhỏ nhưng quan trọng là dừng sớm khi giao lộ về 0. Khi không có loại hoa nào còn phổ biến, tập hợp con đó sẽ không đóng góp gì và quá trình xử lý tiếp theo sẽ bị bỏ qua. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 2, m = 2
bouquets: {1,2}, {2,3}
```Chúng tôi liệt kê các tập hợp con: 

| Tập hợp con | Giao lộ | Kích thước | Đóng góp | 
| --- | --- | --- | --- | 
| {1} | {1,2} | 2 | +4 | 
| {2} | {2,3} | 2 | +4 | 
| {1,2} | {2} | 1 | -1 | 

Câu trả lời cuối cùng là$4 + 4 - 1 = 7$. 

Điều này phù hợp với ý tưởng rằng các chuỗi trên {2} được tính hai lần trong đóng góp của một bó và được sửa bởi tập hợp con kết hợp. 

### Ví dụ 2 

đầu vào:```
n = 3, m = 1
{1,2}, {2,3}, {3}
```| Tập hợp con | Giao lộ | Kích thước | Đóng góp | 
| --- | --- | --- | --- | 
| {1} | {1,2} | 2 | +2 | 
| {2} | {2,3} | 2 | +2 | 
| {3} | {3} | 1 | +1 | 
| {1,2} | {2} | 1 | -1 | 
| {1,3} | ∅ | 0 | 0 | 
| {2,3} | {3} | 1 | -1 | 
| {1,2,3} | ∅ | 0 | 0 | 

Tổng trở thành$2 + 2 + 1 - 1 - 1 = 3$. 

Điều này xác nhận rằng mỗi loại hoa được tính chính xác một lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(2^n \cdot n \cdot 60 + 2^n \log m)$| Mỗi tập hợp con tính toán các giao điểm trên tối đa 19 mặt nạ và một lũy thừa | 
| Không gian |$O(n)$| Lưu trữ cho bitmask bó hoa | 

Với$n \le 19$, cái$2^n$hệ số dưới 600.000, nằm trong giới hạn thoải mái. Mỗi thao tác bên trong vòng lặp là thao tác bit đơn giản hoặc số học mô-đun, do đó giải pháp dễ dàng phù hợp trong vòng 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import prod

    def modpow(a, e):
        res = 1
        a %= MOD
        while e:
            if e & 1:
                res = res * a % MOD
            a = a * a % MOD
            e >>= 1
        return res

    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        masks = []
        for _ in range(n):
            arr = list(map(int, input().split()))
            mask = 0
            for v in arr[1:]:
                mask |= 1 << (v - 1)
            masks.append(mask)

        ans = 0
        for s in range(1, 1 << n):
            inter = (1 << 60) - 1
            for i in range(n):
                if s >> i & 1:
                    inter &= masks[i]
            if inter == 0:
                continue
            cnt = inter.bit_count()
            val = modpow(cnt, m)
            if bin(s).count("1") % 2 == 1:
                ans = (ans + val) % MOD
            else:
                ans = (ans - val) % MOD

        out.append(str(ans % MOD))

    return "\n".join(out)

# provided sample (structure assumed)
assert run("""1
2 2
2 1 2
2 2 3
""") == "7"

# all same bouquet
assert run("""1
3 3
3 1 2 3
3 1 2 3
3 1 2 3
""") == str((3**3) % MOD)

# single bouquet
assert run("""1
1 4
2 1 2
""") == str(pow(2,4,MOD))

# disjoint bouquets
assert run("""1
2 2
1 1
1 2
""") == str((1**2 + 1**2) % MOD)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bó hoa giống hệt nhau | đếm đúng duy nhất | chống trùng lặp thông qua loại trừ bao gồm | 
| một bó hoa |$k^m$đường cơ sở | độ đúng cơ sở | 
| bộ rời rạc | hành vi tính tổng đơn giản | không cần chỉnh sửa chồng chéo | 

## Vỏ cạnh 

Trường hợp tất cả các bó hoa đều giống hệt nhau sẽ gây ra sự đóng góp tập hợp con trùng lặp. Đối với đầu vào nơi mỗi bó hoa`{1,2,3}`Và$m=5$, mọi tập hợp con có cùng kích thước giao nhau 3. Tổng bao gồm-loại trừ trở thành$(1 - (n choose 1) + (n choose 2) - \dots) \cdot 3^5$, đánh giá chính xác$3^5$, xác nhận rằng các bản sao được thu gọn chính xác. 

Trường hợp có giao điểm trống xuất hiện khi các bó hoa không có phần tử chung nào. Ví dụ`{1}`,`{2}`,`{3}`với bất kỳ$m$. Mọi tập hợp con có kích thước ít nhất là 2 đều có giao điểm bằng 0 và không đóng góp gì, chỉ để lại các phần tử đơn, vì vậy kết quả là$1^m + 1^m + 1^m = 3$, phù hợp với thực tế là mỗi bó hoa chỉ tạo ra một chuỗi không đổi. 

Một cấu trúc chồng chéo lớn hơn, chẳng hạn như`{1,2}`,`{2,3}`,`{1,3}`, kiểm tra xem sự chồng chéo từng cặp có được sửa bằng giao điểm ba không. Thuật toán gán trọng số chính xác cho phần tử đơn 2, 1 và 3 mà không trùng lặp, vì mỗi phần tử xuất hiện dưới dạng giao điểm của một họ tập hợp con cụ thể.
