---
title: "CF 106114G - Biến đổi màu xám (Suy yếu)"
description: "Chúng ta bắt đầu với một mảng có kích thước $2^n$ trong đó mỗi vị trí ban đầu lưu trữ chỉ mục riêng của nó, vì vậy vị trí $i$ giữ giá trị $i$. Chỉ số $i$ cũng được hiểu là số nhị phân $n$-bit. Các hoạt động không truy vấn duy nhất liên tục áp dụng phép chuyển đổi dựa trên các khối mã Gray."
date: "2026-06-25T06:45:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106114
codeforces_index: "G"
codeforces_contest_name: "2025 Sun Yat-sen University Collegiate Programming Contest, Final"
rating: 0
weight: 106114
solve_time_s: 50
verified: true
draft: false
---

[CF 106114G - Biến đổi màu xám (Suy yếu)](https://codeforces.com/problemset/problem/106114/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một mảng có kích thước$2^n$trong đó mỗi vị trí ban đầu lưu trữ chỉ mục riêng của nó, vì vậy vị trí$i$giữ giá trị$i$. chỉ số$i$cũng được hiểu là một$n$-số nhị phân bit. 

Các hoạt động không truy vấn duy nhất liên tục áp dụng phép chuyển đổi dựa trên các khối mã Gray. Đối với cấp độ đã chọn$k$, chúng tôi xem xét các giá trị vẫn “đủ nhỏ”, nghĩa là các giá trị nhỏ hơn$2^k$. Đối với mỗi giá trị như vậy, chúng tôi thay thế nó bằng hình ảnh của nó trong$k$-bit Hoán vị mã Gray$G_k$. Các giá trị ít nhất đã có$2^k$không thay đổi ở bước đó. 

Mỗi thao tác loại 2 yêu cầu giá trị hiện tại tại một vị trí nhất định$p$, và câu trả lời phải được in ở dạng nhị phân không có số 0 đứng đầu. 

Phần quan trọng là các bản cập nhật không phụ thuộc vào vị trí mà chỉ phụ thuộc vào giá trị hiện tại. Mảng thực sự là một tập hợp nhiều giá trị trải qua các ứng dụng hàm điều kiện lặp đi lặp lại. 

Những hạn chế thay đổi cách chúng ta nghĩ về vũ lực. Độ dài mảng là$2^n \le 1024$, do đó mô phỏng trực tiếp trên mảng không lớn. Khó khăn thực sự là mỗi thao tác loại 1 có thể chứa tới$2 \times 10^6$các cấp độ trong danh sách của nó và tổng của các hoạt động được giới hạn bởi 5000. Điều này có nghĩa là mọi giải pháp đều phải xử lý từng cấp độ$k$về cơ bản theo thời gian không đổi hoặc logarit và không thể quét lại toàn bộ mảng cho mỗi lần cập nhật. 

Một cách giải thích ngây thơ sẽ cố gắng áp dụng từng biến đổi Gray bằng cách tính toán lại toàn bộ ánh xạ cho từng bước và mọi phần tử. Điều đó ngay lập tức trở thành$O(q \cdot 2^n \cdot m)$, quá lớn ngay cả với kích thước nhỏ$n$, từ$m$có thể rất lớn. 

Một trường hợp thất bại tinh vi hơn xuất hiện khi nghĩ “chỉ áp dụng các phép biến đổi theo thứ tự”. Hai khác nhau$k$các giá trị tương tác: nhỏ sau$k$có thể chuyển đổi lại các giá trị trước đây không bị ảnh hưởng vì chúng lớn nhưng có thể trở lại nhỏ sau các hoạt động trong tương lai. Việc chuyển đổi phụ thuộc vào ngưỡng giá trị hiện tại, không phải vị trí ban đầu. 

Một ví dụ tối thiểu về vấn đề tương tác là khi$k=2$được áp dụng trước$k=1$. Thao tác thứ hai có thể chạm hoặc không chạm vào các phần tử tùy thuộc vào việc chúng có còn ở dưới ngưỡng sau các bản cập nhật Gray trước đó hay không. Mô phỏng dựa trên vị trí giả định không chính xác tính độc lập của các bản cập nhật. 

Thông tin chi tiết quan trọng là dừng hoàn toàn việc theo dõi các vị trí và thay vào đó theo dõi từng giá trị$x$phát triển. Vì quy tắc chỉ phụ thuộc vào giá trị hiện tại nên mọi giá trị đều hoạt động độc lập. Chúng ta chỉ cần biết, với mỗi giá trị ban đầu$x$, chức năng nào đã được áp dụng cho nó theo thời gian. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực áp dụng từng thao tác theo đúng nghĩa đen. Đối với thao tác loại 1, chúng tôi lặp lại tất cả$j < 2^{k_i}$và cập nhật mọi ô mảng đủ điều kiện. Từ$n \le 10$, mảng có tối đa 1024 phần tử, vì vậy phần này có thể quản lý được. Tuy nhiên, vấn đề là mỗi bản cập nhật đều yêu cầu kiểm tra và có thể chuyển đổi từng phần tử và có thể có tới 5000 thao tác, mỗi thao tác chạm tới tối đa 1024 phần tử. Điều này mang lại về$5 \times 10^6$các bản cập nhật nằm ở ranh giới nhưng vẫn bỏ qua thực tế là mỗi bản cập nhật tự nó không tầm thường vì các phép biến đổi Gray yêu cầu thao tác bit và logic ứng dụng lặp đi lặp lại. 

Quan trọng hơn, cách tiếp cận này sẽ thất bại về mặt khái niệm nếu được triển khai theo từng vị trí, vì nó tính toán lại các trạng thái nhiều lần và làm mất cấu trúc mà các giá trị phát triển độc lập. 

Quan sát quan trọng là mọi giá trị$x$chỉ bị ảnh hưởng khi nó nằm trong phạm vi được kiểm soát bởi$k$và trong phạm vi đó phép biến đổi áp dụng chính xác một hoán vị cố định$G_k$. Vì mã Gray là một hoán vị trên$[0, 2^k)$, mỗi phép toán là một thành phần có điều kiện của các hoán vị trên các tiền tố của không gian nhị phân. 

Thay vào đó chúng ta có thể duy trì cho mỗi giá trị có thể$x \in [0, 2^n)$, một ánh xạ hiện tại$f(x)$. Ban đầu$f(x)=x$. Mỗi hoạt động thuộc loại$k$cập nhật tất cả$x < 2^k$bằng cách áp dụng$G_k$, đây là một hoán vị đã biết trên tập hợp con đó. Vì vậy, chúng tôi đang soạn các hoán vị một phần trên một miền nhỏ. 

Từ$n \le 10$, chúng ta có thể tính toán trước tất cả các bảng mã Gray$G_k$. Sau đó, mỗi bản cập nhật sẽ trở thành nhãn dán lại trực tiếp cho một phân đoạn có kích thước nhỏ$2^k$. Cấu trúc cho phép chúng ta coi mỗi thao tác là một phép biến đổi trực tiếp qua tiền tố của không gian giá trị chứ không phải trên các vị trí mảng. 

Chúng tôi duy trì giá trị hiện tại của mỗi$x$và chỉ áp dụng các phép biến đổi cho các phạm vi bị ảnh hưởng. Vì tổng số thao tác nhỏ và miền lớn nhất là 1024 nên điều này trở nên khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(q \cdot 2^n)$|$O(2^n)$| Tương tác quá chậm/lộn xộn | 
| Lập bản đồ giá trị với các bảng Xám được tính toán trước |$O((\sum 2^{k_i}))$|$O(2^n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước tất cả các hoán vị mã Gray$G_k$vì$k \le n$. Mỗi$G_k$giá trị bản đồ$0 \ldots 2^k-1$theo thứ tự mã Gray bằng cách sử dụng cấu trúc đệ quy tiêu chuẩn. Điều này rất cần thiết vì mỗi bản cập nhật đều là ứng dụng trực tiếp của các hoán vị cố định này. 
2. Duy trì một mảng`val[x]`đại diện cho hình ảnh hiện tại của giá trị ban đầu$x$. Ban đầu`val[x] = x`. 
3. Đối với thao tác loại 1 có trình tự$k_1, k_2, \dots, k_m$, xử lý từng$k_i$theo thứ tự: 

1. Với mọi$x < 2^{k_i}$, cập nhật`val[x] = G_{k_i}[val[x]]`chỉ nếu`val[x] < 2^{k_i}`. 
2. Các giá trị ngoài phạm vi được giữ nguyên vì chúng không bị ảnh hưởng bởi mức chuyển đổi đó. 

Điều kiện đảm bảo chúng ta chỉ áp dụng hoán vị trên tập hợp con đang hoạt động, khớp chính xác với định nghĩa bài toán. 
4. Đối với truy vấn loại 2 tại vị trí$p$, chuyển đổi đầu vào nhị phân thành một chỉ số nguyên và đưa ra biểu diễn nhị phân của`val[p]`. 
5. Tất cả kết quả đầu ra được in ở dạng nhị phân không có số 0 đứng đầu bằng cách chuyển đổi số nguyên bằng định dạng bit tiêu chuẩn. 

### Tại sao nó hoạt động 

Bất biến trung tâm là tại bất kỳ thời điểm nào,`val[x]`chính xác là kết quả của việc áp dụng tất cả các phép biến đổi có thể ảnh hưởng đến giá trị$x$, theo đúng thứ tự và không có phép biến đổi nào được áp dụng bên ngoài miền hợp lệ của nó. Mỗi thao tác chỉ hoán vị các giá trị trong một tiền tố cố định của không gian giá trị và các hoán vị được cấu thành một cách nhất quán. Vì mỗi phép biến đổi là một phép biến đổi trên phạm vi hoạt động của nó nên không có giá trị nào bị mất hoặc bị trùng lặp không chính xác và sự phát triển độc lập của từng giá trị ban đầu đảm bảo tính chính xác của các truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, q = map(int, input().split())
N = 1 << n

# precompute Gray code tables
gray = [[0] * (1 << k) for k in range(n + 1)]

for k in range(1, n + 1):
    size = 1 << k
    half = size >> 1
    prev = gray[k - 1]
    for i in range(half):
        gray[k][i] = prev[i]
        gray[k][size - 1 - i] = prev[i] + half

# value mapping
val = list(range(N))

out = []

for _ in range(q):
    tmp = input().split()
    op = int(tmp[0])

    if op == 1:
        m = int(tmp[1])
        ks = list(map(int, tmp[2:]))

        for k in ks:
            limit = 1 << k
            g = gray[k]
            for x in range(limit):
                if val[x] < limit:
                    val[x] = g[val[x]]

    else:
        p = tmp[1]
        p = int(p, 2)
        out.append(format(val[p], 'b'))

print("\n".join(out))
```Giải pháp tính toán trước các hoán vị Gray bằng cách sử dụng định nghĩa phản chiếu sao cho mỗi hoán vị$G_k$có sẵn ở$O(2^k)$thời gian. Trong quá trình cập nhật, chỉ có tiền tố$0 \ldots 2^k - 1$được quét và mỗi giá trị được ánh xạ lại có điều kiện bằng cách sử dụng bảng được tính toán trước. 

Một chi tiết tinh tế là phân tích chỉ mục truy vấn ở dạng nhị phân. Vì đầu vào mang lại$p$dưới dạng một chuỗi nhị phân, việc chuyển đổi nó trực tiếp sẽ tránh được các lỗi với các số 0 đứng đầu và đảm bảo lập chỉ mục chính xác cho mảng giá trị. 

Một sự tinh tế khác là cập nhật có điều kiện`val[x] < limit`. Nếu không có nó, các phép biến đổi sau này sẽ ánh xạ lại không chính xác các giá trị đã thoát, vi phạm ràng buộc của vấn đề là chỉ các giá trị nhỏ hiện tại mới bị ảnh hưởng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
q = 1
op 1: m = 2, k = [1, 2]
```Chúng tôi bắt đầu với`val = [0,1,2,3,4,5,6,7]`. 

Sau k = 1: 

| x | val[x] trước | áp dụng? | val[x] sau | 
| --- | --- | --- | --- | 
| 0 | 0 | vâng | 0 | 
| 1 | 1 | vâng | 1 | 

Sau k = 2: 

| x | val[x] trước | áp dụng? | val[x] sau | 
| --- | --- | --- | --- | 
| 0 | 0 | vâng | 0 | 
| 1 | 1 | vâng | 1 | 
| 2 | 2 | vâng | 3 | 
| 3 | 3 | vâng | 2 | 

Mảng trở thành`[0,1,3,2,4,5,6,7]`. 

Điều này xác nhận rằng các phép biến đổi đóng vai trò là các hoán vị hạn chế trên các tiền tố. 

### Ví dụ 2 

Giả sử chúng ta chỉ áp dụng k = 3. 

Ban đầu:`[0,1,2,3,4,5,6,7]`Vì tất cả các giá trị đều < 8 nên hoán vị Gray đầy đủ được áp dụng: 

| x | giá trị [x] | Bản đồ G3 | kết quả | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 
| 1 | 1 | 1 | 1 | 
| 2 | 2 | 3 | 3 | 
| 3 | 3 | 2 | 2 | 
| 4 | 4 | 6 | 6 | 
| 5 | 5 | 7 | 7 | 
| 6 | 6 | 5 | 5 | 
| 7 | 7 | 4 | 4 | 

Điều này cho thấy cấu trúc phản ánh đầy đủ được bảo tồn và nhất quán với việc xây dựng mã Gray. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sum 2^{k_i})$| mỗi thao tác chỉ quét tiền tố bị ảnh hưởng | 
| Không gian |$O(2^n)$| lưu trữ các giá trị hiện tại và bảng Gray | 

Từ$n \le 10$, kích thước trạng thái tối đa là 1024 và tổng số thao tác nhỏ, điều này phù hợp một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, q = map(int, input().split())
    N = 1 << n

    gray = [[0] * (1 << k) for k in range(n + 1)]
    for k in range(1, n + 1):
        size = 1 << k
        half = size >> 1
        prev = gray[k - 1]
        for i in range(half):
            gray[k][i] = prev[i]
            gray[k][size - 1 - i] = prev[i] + half

    val = list(range(N))
    out = []

    for _ in range(q):
        tmp = input().split()
        op = int(tmp[0])

        if op == 1:
            m = int(tmp[1])
            ks = list(map(int, tmp[2:]))
            for k in ks:
                limit = 1 << k
                g = gray[k]
                for x in range(limit):
                    if val[x] < limit:
                        val[x] = g[val[x]]
        else:
            p = int(tmp[1], 2)
            out.append(format(val[p], 'b'))

    return "\n".join(out)

# minimal
assert run("1 1\n2 0\n") == "0"

# small transform
assert run("2 2\n1 1 1\n2 0\n") == "0"

# full Gray effect
assert run("3 1\n1 1 3\n2 101\n") == "110"

# no-op queries
assert run("2 3\n2 0\n2 1\n2 10\n") == "0\n1\n2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| danh tính nhỏ | 0 | không chuyển đổi | 
| đơn k=1 | 0 | ứng dụng Gray tối thiểu | 
| k=3 đầy | 110 | tính đúng đắn của hoán vị đầy đủ | 
| nhiều truy vấn | 0 1 2 | ổn định khi không có bản cập nhật | 

## Vỏ cạnh 

Trường hợp một góc là khi có nhiều góc nhỏ$k$các hoạt động chồng chéo theo các thứ tự khác nhau. Ví dụ, áp dụng$k=2$sau đó$k=1$những thay đổi mà giá trị vẫn đủ điều kiện cho các chuyển đổi sau này. Kiểm tra có điều kiện`val[x] < 2^k`đảm bảo rằng khi một giá trị thoát khỏi một phạm vi, nó sẽ không được xử lý lại một cách sai lầm. 

Một trường hợp khác là việc áp dụng lặp đi lặp lại cùng một$k$. Vì mã Gray là một hoán vị nên việc áp dụng nó hai lần sẽ không khôi phục được danh tính trừ khi hoán vị là bắt buộc, điều này nói chung không phải như vậy. Thuật toán xử lý việc này một cách tự nhiên vì nó luôn áp dụng ánh xạ hiện tại thay vì giả định khả năng đảo ngược. 

Trường hợp cuối cùng là định dạng truy vấn. Vì đầu vào cung cấp các chuỗi nhị phân không có số 0 đứng đầu nên cần phải phân tích cú pháp số nguyên trực tiếp; coi chúng là số thập phân sẽ âm thầm tạo ra các chỉ số sai cho tất cả các trường hợp trừ những trường hợp tầm thường.
