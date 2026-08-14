---
title: "CF 102341L - Lati@s"
description: "Trò chơi trông rất lớn vì một nước đi sẽ thay thế một bộ dữ liệu bằng tối đa (2^n-1) bộ dữ liệu mới và vị trí ban đầu đã chứa (n!) bộ dữ liệu. Cách hữu ích để xem xét nó không phải là một mô phỏng mà là một trò chơi khách quan có các vị trí có giá trị Sprague-Grundy."
date: "2026-08-13T03:26:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102341
codeforces_index: "L"
codeforces_contest_name: "Radewoosh+mnbvmar Contest (supported by AIM Tech)"
rating: 0
weight: 102341
solve_time_s: 161
verified: true
draft: false
---

[CF 102341L - Lati@s](https://codeforces.com/problemset/problem/102341/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi trông rất lớn vì một nước đi sẽ thay thế một bộ dữ liệu bằng tối đa (2^n-1) bộ dữ liệu mới và vị trí ban đầu đã chứa (n!) bộ dữ liệu. Cách hữu ích để xem xét nó không phải là một mô phỏng mà là một trò chơi khách quan có các vị trí có giá trị Sprague-Grundy. 

Một bộ dữ liệu (A=(A_1,\ldots,A_n)) chỉ có thể được phát khi mọi tọa độ đều dương. Một bước di chuyển chọn một bộ dữ liệu tùy ý (B) với (0\le B_i<A_i), loại bỏ (A) và chèn mọi bộ dữ liệu thu được bằng cách chọn độc lập, trong mỗi tọa độ, (A_i) hoặc (B_i), ngoại trừ bản thân bộ dữ liệu ban đầu (A) không được chèn vào. Toàn bộ vị trí là tổng rời rạc của các trò chơi bộ dữ liệu này, vì vậy giá trị Grundy của tất cả các bộ dữ liệu đều được XOR. 

Đầu vào cho ra một ma trận (n\times n) (M). Đối với mỗi hoán vị của các cột của nó, chúng tôi lấy một mục từ mỗi hàng bằng các cột riêng biệt và thu được một bộ dữ liệu ban đầu. Do đó, vị trí ban đầu chứa tích (n!) được liên kết với tất cả các lựa chọn hoán vị từ ma trận. Đầu ra được yêu cầu chỉ đơn giản là liệu XOR của các giá trị Grundy của chúng có bằng 0 hay không. Giá trị 0 nghĩa là người chơi thứ hai thắng, trong khi giá trị khác 0 nghĩa là người chơi thứ nhất thắng. 

Giới hạn (n\le150) loại trừ bất kỳ số mũ nào trong (n), bao gồm cả việc tạo ra các bộ dữ liệu (n!) một cách rõ ràng. Ngay cả đối với (n=20), (n!) đã là khoảng (2,4\cdot10^{18}) và đối với (n=150), nó vượt xa bất kỳ biểu diễn thực tế nào. Ma trận chỉ có (n^2\le22500) mục nhập, điều này gợi ý rõ ràng rằng tổng hoán vị phải được viết lại theo đại số. Các mục nhập ma trận nằm bên dưới (2^{64}), do đó số học 64-bit có dấu thông thường là không an toàn và phép nhân được yêu cầu dù sao cũng không phải là phép nhân số nguyên thông thường. 

Có một số trường hợp nhỏ có thể dễ dàng đánh lừa việc triển khai coi trò chơi như số học thông thường. Ví dụ,```
1
0
```có đầu ra`Second`. Bộ dữ liệu duy nhất là ((0)), vì vậy nó không có động thái hợp pháp. Việc triển khai coi mọi bộ dữ liệu là một đống dương sẽ tuyên bố không chính xác chiến thắng của người chơi đầu tiên. 

Vì```
1
18446744073709551615
```đầu ra là`First`. Với một tọa độ, trò chơi chính xác là một đống Nim bình thường có kích thước đó, vì vậy mọi giá trị dương đều thắng. Trình phân tích cú pháp 64 bit đã ký sẽ không thành công với dữ liệu đầu vào này vì (2^{64}-1) lớn hơn (2^{63}-1). 

Một trường hợp tế nhị khác là các hàng lặp lại. Ví dụ,```
2
1 1
1 1
```có đầu ra`Second`. Có hai bộ hoán vị giống hệt nhau, đó là ((1,1)) hai lần. Giá trị Grundy của chúng bị hủy bởi XOR. Việc triển khai chuyển đổi tổng hoán vị thành tổng số nguyên thông thường sẽ bỏ lỡ việc hủy bỏ đặc tính hai này. 

Cuối cùng, mẫu thứ hai,```
2
1 2
2 3
```cũng mang lại`Second`. Biểu thức giống định thức là (1\otimes3\oplus2\otimes2=3\oplus3=0), trong đó (\otimes) là phép nhân nimber. Sử dụng phép nhân thông thường sẽ cho kết quả (3+4=7), điều này không liên quan gì đến giá trị Grundy của trò chơi. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ liệt kê mọi hoán vị, xây dựng bộ dữ liệu của nó, tính giá trị Grundy của bộ dữ liệu đó và XOR tất cả các giá trị đó. Điều này là không thể vì có (n!) hoán vị. Đối với ma trận trường hợp xấu nhất có mọi mục nhập bằng (2^{64}-1), có (n!) bộ dữ liệu ban đầu và nếu chúng ta cố gắng liệt kê tất cả các lựa chọn hợp pháp của (B) từ một bộ dữ liệu như vậy, thì sẽ có ((2^{64}-1)^n) lựa chọn cho (B), với (2^n-1) bộ dữ liệu được tạo cho mọi lựa chọn. Do đó, ngay cả việc liệt kê lớp đầu tiên của cây trò chơi cũng sẽ liên quan đến các kết hợp (n!(2^{64}-1)^n(2^n-1)). Về mặt khái niệm, vũ lực là đúng, nhưng nó đã thất bại trước khi việc phân tích trò chơi thực tế trở nên phù hợp. 

Quan sát quan trọng là giá trị Grundy của một bộ dữ liệu có dạng đại số đáng chú ý. Bộ dữ liệu một tọa độ chứa (a) có giá trị Grundy (a). Đối với một số tọa độ, việc di chuyển sẽ thay thế (A) bằng tất cả các kết hợp khác trống của (A_i) và (B_i). Bởi vì giá trị Grundy của các thành phần độc lập là XOR, XOR của các bộ dữ liệu được tạo sẽ mở rộng chính xác giống như tích trên đặc tính hai. 

Đây chính xác là cách giải thích trò chơi đệ quy về phép nhân nimber. Nếu một bộ là (A=(A_1,\ldots,A_n)), giá trị Grundy của nó là 

[ 
g(A)=A_1\otimes A_2\otimes\cdots\otimes A_n. 
] 

Lý do là việc di chuyển từ (A) đến (B) đã chọn sẽ tạo ra cùng một cấu trúc ba góc, bốn góc và có chiều cao hơn được sử dụng trong định nghĩa đệ quy tiêu chuẩn của phép nhân nimber. XOR trên tất cả các tập hợp con khác rỗng là phép khai triển tích tương ứng và đặc tính mex của phép nhân nimber mang lại chính xác phép truy toán Grundy cần thiết. Đây là phiên bản (n) chiều của trò chơi hình chữ nhật giảm dần của Conway. 

Do đó, vị trí bắt đầu là XOR của 

[ 
\bigotimes_{i=1}^{n} M_{i,\sigma(i)} 
] 

trên mọi hoán vị (\sigma). Phép cộng nimber là XOR và phép nhân nimber có tính phân phối trên XOR. Việc mở rộng định thức trên một trường có đặc số hai sẽ cho chính xác tổng hoán vị này. Dấu hiệu xác định thông thường biến mất vì (1=-1) ở đặc số hai. Do đó toàn bộ trò chơi quy về tính toán 

[ 
\det(M) 
] 

trong đó mọi phép cộng là XOR và mọi phép nhân đều là phép nhân nhanh nhẹn. Sự giảm thiểu này cũng là quan sát chính của cuộc thảo luận giải pháp được công bố cho vấn đề này. 

Thử thách còn lại là thực hiện phép nhân nimber đủ nhanh. Các giá trị bên dưới (2^{64}) tạo thành trường hữu hạn (\mathrm{GF}(2^{64})) dưới phép cộng và nhân nhanh hơn. Một phép nhân đệ quy đơn giản là quá tốn kém trong việc loại bỏ Gaussian (O(n^3)). Thay vào đó, chúng tôi chia các nimber 64 bit thành các phần 16 bit và tính toán trước biểu diễn logarit/số mũ của trường nimber 16 bit. Sau đó, một sản phẩm 64 bit có thể được lắp ráp chỉ bằng cách sử dụng một số lượng không đổi các sản phẩm trường 16 bit. Đây là cấu trúc phân chia và chinh phục tương tự được sử dụng bởi các thư viện nimber đã thành lập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n!(2^{64})^n2^n)) đã có cho lớp di chuyển đầu tiên | Thiên văn | Quá chậm | 
| Tối ưu | (O(n^3+2^{16})) | (O(n^2+2^{16})) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giải thích mọi mục nhập ma trận dưới dạng nimber 64-bit. Việc cộng các nimber là XOR theo bit thông thường, trong khi phép nhân là tích số nimber đặc biệt. 
2. Coi mỗi bộ dữ liệu ban đầu là một trò chơi khách quan độc lập. Giá trị Grundy của một bộ (A) là 

[ 
A_1\otimes A_2\otimes\cdots\otimes A_n. 
] 

Quy tắc di chuyển đa chiều chính xác là cấu trúc đệ quy có giá trị Grundy là phép nhân nhanh hơn, vì vậy chúng ta không bao giờ cần mô phỏng các hậu duệ của một bộ dữ liệu. 

1. XOR các giá trị Grundy của tất cả các bộ hoán vị. Bằng cách phân phối, điều này trở thành

[ 
\bigoplus_{\sigma} 
\left( 
M_{1,\sigma(1)} 
\otimes\cdots\otimes 
M_{n,\sigma(n)} 
\đúng). 
] 

Đây là định thức của (M) trên trường nimbers 64-bit. Vì trường có đặc số 2 nên không có dấu riêng cho các hoán vị lẻ. 

1. Tính định thức này bằng phép khử Gaussian. Đối với cột (k), tìm một hàng (p\ge k) có mục nhập khác 0 trong cột đó. Nếu không tồn tại, định thức bằng 0 và câu trả lời là ngay lập tức`Second`. 
2. Hoán đổi hàng (p) với hàng (k). Trong một trường thông thường, việc hoán đổi hàng phủ định định thức, nhưng trong đặc tính hai, sự phủ định của một giá trị là chính nó, do đó định thức không thay đổi. 
3. Nhân tích lũy định thức với trục xoay (A_{k,k}). Sau đó tính nghịch đảo nhân của nó và sử dụng nó để loại bỏ các mục bên dưới trục xoay. Đối với một hàng (i>k), hệ số cần thiết là 

[ 
f=A_{i,k}\otimes A_{k,k}^{-1}. 
] 

Với mọi (j>k), thay thế 

[ 
A_{i,j} 
\leftarrow 
A_{i,j}\oplus(f\otimes A_{k,j}). 
] 

Mục nhập trong cột (k) sau đó có thể được đặt trực tiếp về 0. 

1. Tính nghịch đảo bằng cách sử dụng nhận dạng trường 

[ 
x^{-1}=x^{2^{64}-2} 
] 

đối với khác không (x). Lũy thừa nhị phân chỉ cần 64 phép nhân nimber trên mỗi trục, không đáng kể so với các bản cập nhật loại bỏ (O(n^3)). Nhận dạng nghịch đảo trường hữu hạn cũng là nhận dạng được sử dụng trong các giải pháp được công bố. 

1. Sau khi tất cả các cột đã được xử lý, bộ tích lũy xác định là giá trị Grundy của toàn bộ vị trí bắt đầu. In`First`khi nó khác 0 và`Second`nếu không thì. 

### Tại sao nó hoạt động 

Điều bất biến là XOR của các giá trị Grundy của tất cả các trò chơi theo bộ hiện được biểu thị là tổng nim của các giá trị trò chơi riêng lẻ của chúng. Đối với một bộ, quy tắc di chuyển chính xác là trò chơi hình chữ nhật giảm dần đa chiều, có giá trị Grundy là tích nhanh hơn của các tọa độ của nó. Do đó, vị trí ban đầu có giá trị bằng XOR của tích số nhanh hơn được chọn bởi tất cả các hoán vị. Tính phân phối biến hoán vị XOR đó thành định thức của (M) trên trường nimber. Phép khử Gaussian bảo toàn định thức đó trong khi rút gọn ma trận về dạng tam giác và tích của các trục của nó là định thức. Định thức bằng 0 có nghĩa là giá trị Grundy bằng 0, chính xác là vị trí thua đối với người chơi đầu tiên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MASK16 = 65535
ORDER = 65535
PROOT = 10279
PPOLY = 92191

def build_small_table():
    dp = [[0] * 256 for _ in range(256)]
    dp[1][1] = 1

    for e in range(1, 4):
        p = 1 << e
        q = p >> 1
        ep = 1 << p
        eq = 1 << q
        mask = eq - 1

        for i in range(ep):
            for j in range(i, ep):
                if i < eq and j < eq:
                    continue

                if min(i, j) <= 1:
                    v = i * j
                else:
                    iu = i >> q
                    il = i & mask
                    ju = j >> q
                    jl = j & mask

                    u = dp[iu][ju]
                    l = dp[il][jl]
                    ul = dp[iu ^ il][ju ^ jl]
                    uq = dp[u][eq >> 1]

                    v = ((ul ^ l) << q) ^ uq ^ l

                dp[i][j] = v
                dp[j][i] = v

    return dp

SMALL = build_small_table()

def nim16_direct(a, b):
    if a == 0 or b == 0:
        return 0
    if min(a, b) <= 1:
        return a * b

    iu = a >> 8
    il = a & 255
    ju = b >> 8
    jl = b & 255

    u = SMALL[iu][ju]
    l = SMALL[il][jl]
    ul = SMALL[iu ^ il][ju ^ jl]
    uq = SMALL[u][128]

    return ((ul ^ l) << 8) ^ uq ^ l

def build_field_tables():
    base = [1] * 16
    for i in range(1, 16):
        base[i] = nim16_direct(base[i - 1], PROOT)

    raw_exp = [0] * ORDER
    raw_exp[0] = 1

    for i in range(1, ORDER):
        x = raw_exp[i - 1]
        raw_exp[i] = (x << 1) ^ (PPOLY if x & 32768 else 0)

    pre = [0] * 65536
    for bit in range(16):
        start = 1 << bit
        end = start << 1
        value = base[bit]
        for x in range(start, end):
            pre[x] = pre[x - start] ^ value

    exp = [0] * ORDER
    log = [0] * 65536

    for i in range(ORDER):
        value = pre[raw_exp[i]]
        exp[i] = value
        log[value] = i

    return exp, log

EXP16, LOG16 = build_field_tables()

def mul16(a, b):
    if a == 0 or b == 0:
        return 0
    return EXP16[(LOG16[a] + LOG16[b]) % ORDER]

def h16(a, shift):
    if a == 0:
        return 0
    return EXP16[(LOG16[a] + shift) % ORDER]

def mul32(a, b):
    ah = a >> 16
    al = a & MASK16
    bh = b >> 16
    bl = b & MASK16

    low = mul16(al, bl)
    cross = mul16(ah ^ al, bh ^ bl)
    high = h16(mul16(ah, bh), 3)

    return ((cross ^ low) << 16) ^ high ^ low

def mul64(a, b):
    if a == 0 or b == 0:
        return 0

    ah = a >> 32
    al = a & 0xffffffff
    bh = b >> 32
    bl = b & 0xffffffff

    low = mul32(al, bl)
    cross = mul32(ah ^ al, bh ^ bl)

    high_part = mul32(ah, bh)
    h_high = (
        (h16((high_part >> 16) ^ (high_part & MASK16), 3) << 16)
        ^ h16(high_part >> 16, 6)
    )

    return ((cross ^ low) << 32) ^ h_high ^ low

def nim_pow(a, e):
    result = 1
    while e:
        if e & 1:
            result = mul64(result, a)
        a = mul64(a, a)
        e >>= 1
    return result

def nim_inverse(a):
    return nim_pow(a, (1 << 64) - 2)

def determinant(mat):
    n = len(mat)
    det = 1

    for col in range(n):
        pivot = col
        while pivot < n and mat[pivot][col] == 0:
            pivot += 1

        if pivot == n:
            return 0

        if pivot != col:
            mat[pivot], mat[col] = mat[col], mat[pivot]

        p = mat[col][col]
        det = mul64(det, p)
        inv = nim_inverse(p)

        pivot_row = mat[col]

        for i in range(col + 1, n):
            value = mat[i][col]
            if value == 0:
                continue

            factor = mul64(value, inv)
            row = mat[i]

            row[col] = 0
            for j in range(col + 1, n):
                row[j] ^= mul64(factor, pivot_row[j])

    return det

def solve():
    n = int(input())
    mat = [list(map(int, input().split())) for _ in range(n)]

    ans = determinant(mat)
    print("First" if ans else "Second")

if __name__ == "__main__":
    solve()
```Giai đoạn tiền xử lý đầu tiên xây dựng phép nhân cho tất cả các bộ nhớ bên dưới (256). Phép truy hồi chia giá trị 8 bit thành hai nửa và sử dụng định nghĩa đệ quy của phép nhân nimber. Bảng này chỉ chứa (256^2) mục. 

Giai đoạn tiếp theo xây dựng một biểu diễn của trường với 65536 phần tử. giá trị`PPOLY = 92191`mô tả đa thức tối giản được sử dụng để biểu diễn đa thức, trong khi`PROOT = 10279`cung cấp một phần tử nguyên thủy trong biểu diễn số. Bảng số mũ chuyển đổi lũy thừa của phần tử nguyên thủy đó thành số 16 bit thực tế và`LOG16`thực hiện ánh xạ ngược. Cấu trúc này là phiên bản 16-bit của việc triển khai nhanh chóng tiêu chuẩn. 

Khi đó, một sản phẩm nhanh nhẹn hơn 16 bit chỉ là hai lần tra cứu bảng và một thao tác chỉ mục. Các sản phẩm 32-bit và 64-bit sử dụng đặc tính nhận dạng kiểu Karatsuba từ lĩnh vực nimber. Đặc biệt,`mul32`yêu cầu ba sản phẩm 16 bit và`mul64`yêu cầu ba sản phẩm 32-bit. Sự thay đổi được biểu thị bằng`h16`tương ứng với phép nhân với (2^{15}) trong phân tách trường 16 bit. 

Quy trình xác định thực hiện phép loại bỏ Gaussian thông thường bằng phép cộng XOR thay thế. Vì trường có đặc tính hai nên việc hoán đổi hàng không tạo ra sự thay đổi dấu. Bản thân trục xoay được nhân thành`det`trước khi hàng được sử dụng để loại bỏ, do đó giá trị cuối cùng vẫn là yếu tố quyết định mặc dù các hàng loại bỏ không được chuẩn hóa. 

Số mũ nghịch đảo là`(1 << 64) - 2`, chính xác là (2^{64}-2). Số nguyên Python có độ chính xác tùy ý, do đó không có hiện tượng tràn khi xây dựng số mũ đó hoặc khi phân tích cú pháp các giá trị đầu vào. Bản thân các mục nhập ma trận vẫn ở bên dưới (2^{64}) và mọi thao tác nhanh nhẹn đều trả về một phần tử trường 64 bit khác. 

Thứ tự thực hiện các thao tác trong quá trình loại bỏ rất quan trọng. Hệ số phải được tính bằng cách sử dụng nghịch đảo của trục hiện tại trước khi hàng trục được sửa đổi. Trong quá trình triển khai này, hàng tổng hợp không bao giờ được chuẩn hóa, vì vậy các mục nhập ban đầu của nó vẫn có sẵn cho mỗi lần cập nhật hàng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Ma trận là```
0 1 2
1 2 3
1 2 1
```Dấu vết loại bỏ là: 

| Cột | Hàng xoay | Xoay vòng | Cập nhật hàng | Quyết định | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | (R_1\leftrightarrow R_0), sau đó (R_2\leftarrow R_2\oplus R_0) | 1 | 
| 1 | 1 | 1 | Không có mục nào khác 0 dưới trục xoay | 1 | 
| 2 | 2 | 2 | Không có hàng nào bên dưới | 2 | 

Sau lần hoán đổi đầu tiên, ma trận là```
1 2 3
0 1 2
1 2 1
```Thừa số của hàng thứ ba là (1\otimes1^{-1}=1), nên hàng thứ ba trở thành```
0 0 2
```Do đó, các trục xoay tam giác là (1,1,2) và định thức là 

[ 
1\otimes1\otimes2=2. 
] 

Định thức khác 0, do đó trò chơi ban đầu có giá trị Grundy khác 0 và câu trả lời là`First`. 

Dấu vết này cũng chứng minh tại sao phép nhân thông thường không được thay thế vào định thức. Ma trận tương tự đang được đánh giá trong trường nimber, ví dụ: trong đó (2\otimes2=3). 

### Mẫu 2 

Ma trận là```
1 2
2 3
```Dấu vết là: 

| Cột | Xoay vòng | Nghịch đảo | Yếu tố hàng | Hàng cập nhật | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 2 | ([0,,3\oplus(2\otimes2)]=[0,0]) | 1 | 
| 1 | 0 | không có sẵn | không | không | 0 | 

Vì (2\otimes2=3), mục nhập thứ hai trở thành 

[ 
3\oplus3=0. 
] 

Trục xoay thứ hai không tồn tại nên định thức bằng 0 và đáp án là`Second`. 

Ví dụ này thực hiện tình huống chính xác trong đó ma trận trở thành số ít vì phép nhân nhanh hơn khác với phép nhân số nguyên thông thường. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n^3+2^{16})) | Việc loại bỏ Gaussian thực hiện các thao tác trường (O(n^3)), trong khi các bảng 16 bit yêu cầu xử lý trước (O(2^{16})) | 
| Không gian | (O(n^2+2^{16})) | Ma trận cần lưu trữ (O(n^2)) và bảng số mũ/logarit cần lưu trữ (O(2^{16})) | 

Đối với (n\le150), việc loại bỏ Gaussian chỉ có khoảng vài triệu thao tác ở cấp độ ma trận và mỗi phép nhân nhanh chóng giảm xuống một số lượng truy cập bảng 16 bit không đổi. Quá trình tiền xử lý độc lập với (n), do đó nghiệm phù hợp với giới hạn đa thức dự định thay vì kích thước giai thừa của tập hợp hoán vị ban đầu. 

## Trường hợp thử nghiệm 

Khai thác sau đây giả định việc triển khai đã gửi được lưu dưới dạng`solution.py`và phơi bày`solve()`chức năng. Trường hợp kích thước tối đa được tạo theo chương trình để bản thân tệp thử nghiệm vẫn có thể đọc được.```python
# test_solution.py
import sys
import io
from solution import solve

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
assert run(
    """3
0 1 2
1 2 3
1 2 1
"""
) == "First", "sample 1"

# Provided sample 2
assert run(
    """2
1 2
2 3
"""
) == "Second", "sample 2"

# Minimum size, zero tuple is immediately losing.
assert run(
    """1
0
"""
) == "Second", "single zero"

# Minimum size, largest allowed input value is positive.
MAX64 = (1 << 64) - 1
assert run(
    f"""1
{MAX64}
"""
) == "First", "single maximum value"

# Identity matrix has determinant 1.
assert run(
    """2
1 0
0 1
"""
) == "First", "identity matrix"

# Equal rows make the determinant zero.
assert run(
    """2
1 1
1 1
"""
) == "Second", "equal rows"

# Boundary value combined with a zero entry.
assert run(
    f"""2
{MAX64} 0
0 1
"""
) == "First", "maximum boundary value"

# Maximum n, all rows equal, so determinant is zero.
max_case = "150\n" + "\n".join(["7 " * 149 + "7"] * 150) + "\n"
assert run(max_case) == "Second", "maximum n with equal rows"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0`|`Second`| Một bộ chứa số 0 không di chuyển | 
|`1 / 2^64-1`|`First`| Ranh giới không dấu 64-bit và trò chơi một chiều | 
|`2 / identity`|`First`| Một định thức khác 0 với các mục ngoài đường chéo bằng 0 | 
|`2 / equal rows`|`Second`| Đặc tính-hai ma trận hủy và số ít | 
|`2 / [[2^64-1,0],[0,1]]`|`First`| Giá trị đầu vào tối đa không có dấu tràn | 
|`150 / all 7`|`Second`| Kích thước ma trận tối đa và định thức số 0 tức thời | 

## Vỏ cạnh 

Trường hợp bộ dữ liệu bằng 0 được xử lý trước khi thực hiện bất kỳ phép nghịch đảo hoặc loại bỏ nào. Vì```
1
0
```định thức là một mục duy nhất (0), do đó thuật toán trả về 0 và in`Second`. Điều này phù hợp với trò chơi vì không thể chọn một bộ chứa số 0. 

Để có giá trị lớn nhất có thể,```
1
18446744073709551615
```định thức cũng chính là nimber khác 0 đó. Trò chơi một chiều là một đống Nim thông thường, vì vậy giá trị Grundy của nó chính là kích thước của đống. Thuật toán không bao giờ chuyển đổi giá trị thành kiểu có dấu và định thức khác 0 tạo ra`First`. 

Các bộ dữ liệu lặp lại được XOR xử lý tự động. Với```
2
1 1
1 1
```cả hai hoán vị đều tạo ra cùng một bộ dữ liệu ((1,1)). Giá trị Grundy của nó là (1\otimes1=1) và hai bản sao đóng góp (1\oplus1=0). Định thức cũng bằng 0 vì hai hàng bằng nhau nên thuật toán in ra`Second`. 

Trục xoay bằng 0 không ngụ ý định thức bằng 0 cho đến khi thuật toán tìm kiếm tất cả các hàng bên dưới nó. Ví dụ,```
2
0 1
1 0
```bắt đầu bằng số 0 ở vị trí trục xoay đầu tiên, nhưng hàng thứ hai cung cấp trục xoay khác 0. Sau khi hoán đổi các hàng, các trục xoay là (1) và (1), cho định thức (1) và đầu ra`First`. Việc triển khai chỉ đơn giản là kiểm tra`matrix[k][k] == 0`không tìm kiếm hàng khác sẽ trả về không chính xác`Second`. 

Mẫu thứ hai cho thấy sự khác biệt giữa số học thông thường và số học nhanh nhẹn. Vì```
2
1 2
2 3
```hệ số loại bỏ là (2) và giá trị phía dưới bên phải trở thành 

[ 
3\oplus(2\otimes2)=3\oplus3=0. 
] 

Định thức bằng 0 nên kết quả là`Second`. Thay vào đó, việc loại bỏ số nguyên thông thường sẽ sử dụng (2\cdot2=4) và thu được (3-4=-1), điều này không liên quan đến đại số của trò chơi. 

Ở kích thước tối đa, hãy xem xét ma trận (150\times150) trong đó mọi mục nhập là (7). Trục xoay đầu tiên có thể được chọn, nhưng mọi hàng khác đều giống hệt với trục đó, do đó việc loại bỏ sẽ làm cho tất cả các cột trục xoay sau đó biến mất. Tương tự, định thức có hai hàng bằng nhau và bằng 0. Thuật toán kết thúc với`Second`mà không bao giờ xây dựng các bộ dữ liệu ban đầu (150!)
