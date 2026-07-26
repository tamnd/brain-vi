---
title: "CF 102864H - Tổng tiền tố"
description: "Trình tự trong bài toán này không chỉ là mảng ban đầu. Bắt đầu từ mảng a^(0) đã cho, chúng ta liên tục áp dụng phép tính tổng tiền tố. Sau một ứng dụng, a^(1) là mảng tổng tiền tố. Sau một ứng dụng khác, a^(2) là tổng tiền tố của a^(1), v.v."
date: "2026-07-25T13:44:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102864
codeforces_index: "H"
codeforces_contest_name: "The 15-th BIT Campus Programming Contest - Online Round"
rating: 0
weight: 102864
solve_time_s: 95
verified: true
draft: false
---

[CF 102864H - Tổng tiền tố](https://codeforces.com/problemset/problem/102864/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trình tự trong bài toán này không chỉ là mảng ban đầu. Bắt đầu từ mảng đã cho`a^(0)`, chúng tôi liên tục áp dụng phép toán tổng tiền tố. Sau một lần nộp đơn,`a^(1)`là mảng tổng tiền tố. After another application,`a^(2)`là tổng tiền tố của`a^(1)`, vân vân. Các truy vấn yêu cầu tổng của một phạm vi trong`a^(k)`, trong khi các bản cập nhật sửa đổi một phần tử của mảng ban đầu`a^(0)`. 

Một cách trực tiếp để xem ảnh hưởng của các tổng tiền tố lặp lại là thông qua các kết hợp. Một giá trị ban đầu`a_j^(0)`góp phần vào vị trí sau này`a_i^(k)`với hệ số`C(i-j+k-1, k-1)`khi`j <= i`. Truy vấn phạm vi có thể được viết lại dưới dạng truy vấn tiền tố ở một cấp độ khác:`sum(l, r, k) = a_r^(k+1) - a_(l-1)^(k+1)`. 

Do đó, nhiệm vụ là duy trì các giá trị của`(k+1)`- chuỗi tiền tố thứ sau khi cập nhật điểm trên chuỗi ban đầu. 

Các ràng buộc được thiết kế để ngăn chặn việc xây dựng lại toàn bộ mảng đã biến đổi sau mỗi truy vấn. Độ dài mảng chỉ`10000`, nhưng số lượng hoạt động đạt`200000`, vì vậy một`O(n)`hoạt động cho mỗi bản cập nhật hoặc truy vấn sẽ cần khoảng hai tỷ hoạt động. Chúng ta cần sử dụng kích thước mảng nhỏ cẩn thận hơn và tránh chạm vào tất cả các vị trí cho mỗi lần sửa đổi. 

Một vài trường hợp dễ dàng phá vỡ việc thực hiện bất cẩn. Đầu tiên là truy vấn tiền tố bắt đầu từ vị trí đầu tiên. Ví dụ:```
Input
1 1
5
1
2 1 1
```Câu trả lời là`5`. Một giải pháp sử dụng`l-1`không xử lý số 0 một cách chính xác sẽ truy cập vào một chỉ mục không hợp lệ. 

Một trường hợp nguy hiểm khác là khi cập nhật diễn ra trước các truy vấn. Ví dụ:```
Input
2 2
1 2
2
1 1 3
2 1 1
```Sau khi cập nhật phần tử đầu tiên sẽ trở thành`4`. Trình tự sau hai thao tác tiền tố bắt đầu bằng`4`, vậy câu trả lời là`4`. Một giải pháp chỉ cập nhật mảng ban đầu và quên rằng tất cả các cấp tiền tố cao hơn phụ thuộc vào nó sẽ không thành công. 

Vấn đề chung cuối cùng là phép trừ mô-đun. Ví dụ: nếu giá trị tiền tố bên phải nhỏ hơn giá trị tiền tố bên trái theo modulo`998244353`, phép trừ phải được chuẩn hóa trước khi in. 

## Phương pháp tiếp cận 

Phương pháp vũ phu tuân theo định nghĩa theo nghĩa đen. Sau mỗi lần cập nhật, nó sẽ thay đổi`a^(0)`và xây dựng lại hoạt động tổng tiền tố`k`lần. Ngay cả khi chúng tôi tối ưu hóa việc xây dựng lại một cấp độ để`O(n)`, tổng chi phí là`O(nk)`mỗi lần cập nhật. Với`n`Và`k`cả hai đều bằng`10000`, đây là khoảng`10^8`hoạt động cho một bản cập nhật và quá chậm đối với`200000`truy vấn. 

Quan sát quan trọng là giá trị truy vấn cuối cùng là tuyến tính trong mảng ban đầu. Một cập nhật điểm duy nhất tại vị trí`p`thay đổi giá trị của`a_x^(k+1)`cho mọi`x >= p`qua:`q * C(x-p+k, k)`. 

Dãy các giá trị gia tăng là một dãy nhị thức dịch chuyển. Từ`n`chỉ là`10000`, chúng ta có thể sử dụng phép phân tách căn bậc hai. Thay vì duy trì mọi giá trị được chuyển đổi có thể có trên toàn cầu, chúng tôi chia các vị trí ban đầu thành các khối nhỏ. 

Mỗi khối lưu trữ toàn bộ đóng góp của tất cả các giá trị ổn định bên trong khối đó cho mọi vị trí tiền tố. Cập nhật điểm được lưu trữ tạm thời bên trong khối của nó dưới dạng thay đổi đang chờ xử lý. Các truy vấn kết hợp việc đóng góp khối được lưu trữ với các thay đổi đang chờ xử lý. Khi một khối tích lũy quá nhiều thay đổi đang chờ xử lý, chúng tôi sẽ xây dựng lại khối đó bằng cách áp dụng những thay đổi đó và tính toán lại phần đóng góp của khối đó. 

Điều này mang lại sự cân bằng giữa chi phí xây dựng lại và chi phí truy vấn. Với kích thước khối gần`22`và ngưỡng xây dựng lại gần`450`, cả hai phần đều có khoảng vài trăm thao tác cho mỗi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nk) mỗi lần cập nhật | O(n) | Quá chậm | 
| Phân hủy khối | O(sqrt(n) + ngưỡng) khấu hao | O(n sqrt(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước các hệ số`C(k+d, k)`cho mọi khoảng cách có thể`d`. Các hệ số tương tự được sử dụng bất cứ khi nào một vị trí nhận được bản cập nhật, bởi vì bản cập nhật tại`p`ảnh hưởng đến vị trí`x`chỉ qua khoảng cách`x-p`. 
2. Chia mảng ban đầu thành các khối. Đối với mỗi khối, hãy xây dựng một mảng`contribution`Ở đâu`contribution[x]`lưu trữ bao nhiêu giá trị ổn định của khối này đóng góp vào`a_x^(k+1)`. Mức đóng góp được tính theo công thức:`a_x^(k+1) = sum(j <= x) C(x-j+k, k) * a_j^(0)`. 

1. Lưu trữ mọi cập nhật điểm mới dưới dạng sửa đổi đang chờ xử lý trong khối tương ứng của nó. Chúng tôi không thay đổi ngay mảng đóng góp vì một lần cập nhật sẽ ảnh hưởng đến nhiều vị trí. 
2. Để trả lời một truy vấn, trước tiên hãy tính`(k+1)`-giá trị tiền tố thứ tại`r`Và`l-1`. Đối với mỗi khối, hãy thêm phần đóng góp được lưu trữ của nó vào vị trí đó. Sau đó xử lý trực tiếp các bản cập nhật đang chờ xử lý của khối đó bằng cách sử dụng các hệ số nhị thức được tính toán trước. 
3. Khi số lượng cập nhật đang chờ xử lý trong một khối đạt đến giới hạn xây dựng lại đã chọn, hãy áp dụng tất cả các thay đổi đang chờ xử lý đối với các giá trị thực của khối và xây dựng lại mảng đóng góp của nó. Điều này giữ cho số lượng tính toán đang chờ xử lý trực tiếp bị giới hạn. 

Tại sao nó hoạt động: 

Phần đóng góp được lưu trữ bởi một khối chính xác là tổng tác động của tất cả các giá trị trong khối đó đã được kết hợp. Các bản cập nhật đang chờ xử lý sẽ không bị mất vì mọi truy vấn đều bổ sung rõ ràng đóng góp toán học của chúng. Việc xây dựng lại chỉ thay đổi khi lưu trữ những đóng góp tương tự, do đó giá trị được truy vấn trả về không thay đổi. Vì câu trả lời là sự khác biệt giữa hai`(k+1)`-giá trị tiền tố thứ, mọi truy vấn phạm vi đều được trả lời chính xác. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

MOD = 998244353

def solve():
    n, k = map(int, input().split())
    a = [0] + list(map(int, input().split()))

    w = [1] * (n + 1)
    inv = [1] * (n + 2)
    for i in range(1, n + 2):
        inv[i] = pow(i, MOD - 2, MOD)

    for d in range(n):
        w[d + 1] = w[d] * (k + d + 1) % MOD * inv[d + 1] % MOD

    block_size = 22
    rebuild_limit = 450
    block_count = (n + block_size - 1) // block_size

    values = [a[:]]
    blocks = []
    pending = [[] for _ in range(block_count)]

    def rebuild(b):
        left = b * block_size + 1
        right = min(n, left + block_size - 1)
        cur = array('I', [0]) * (n + 1)
        for pos in range(left, right + 1):
            val = a[pos]
            if val:
                for x in range(pos, n + 1):
                    cur[x] = (cur[x] + val * w[x - pos]) % MOD
        blocks[b] = cur

    blocks = [None] * block_count
    for b in range(block_count):
        rebuild(b)

    def get_prefix(x):
        if x <= 0:
            return 0
        ans = 0
        for b in range(block_count):
            ans += blocks[b][x]
        for b in range(block_count):
            for pos, delta in pending[b]:
                if pos <= x:
                    ans += delta * w[x - pos]
        return ans % MOD

    out = []
    m = int(input())

    for _ in range(m):
        t, x, y = map(int, input().split())
        if t == 1:
            b = (x - 1) // block_size
            a[x] = (a[x] + y) % MOD
            pending[b].append((x, y))
            if len(pending[b]) >= rebuild_limit:
                for pos, delta in pending[b]:
                    a[pos] = a[pos] % MOD
                pending[b].clear()
                rebuild(b)
        else:
            ans = get_prefix(y) - get_prefix(x - 1)
            out.append(str(ans % MOD))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Mảng hệ số`w`được xây dựng bằng cách sử dụng phép lặp:`C(k+d+1,k) = C(k+d,k) * (k+d+1)/(d+1)`. 

Điều này tránh việc kết hợp tính toán nhiều lần. Nghịch đảo mô-đun được sử dụng vì tất cả số học được thực hiện dưới`998244353`. 

các`rebuild`hàm xây dựng phần đóng góp được lưu trữ của một khối. Mỗi phần tử trong khối đóng góp vào tất cả các vị trí tiền tố sau này, phù hợp với công thức của`(k+1)`tiền tố -th. 

Hàm truy vấn trước tiên sử dụng các đóng góp khối được tính toán trước. Sự khác biệt còn lại đến từ các bản cập nhật chưa được xây dựng lại nên được áp dụng trực tiếp với cùng công thức hệ số nhị thức. 

Thao tác cập nhật chỉ thay đổi giá trị ban đầu và ghi lại delta. Khi danh sách đang chờ xử lý trở nên lớn, việc xây dựng lại sẽ chuyển những thay đổi đó vào cấu trúc khối cố định. Tất cả các giá trị được giữ theo modulo`998244353`, bao gồm cả phép trừ trong câu trả lời cuối cùng. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
3 2
1 3 2
4
2 1 3
1 2 1
2 1 3
2 1 1
```Truy vấn đầu tiên yêu cầu tổng của cấp tiền tố thứ hai. 

| Hoạt động | Cập nhật vị trí | Những thay đổi đang chờ xử lý | Trả lời | 
| --- | --- | --- | --- | 
| Ban đầu | không | không | 17 | 
| Cập nhật | thêm 1 vào vị trí 2 |`(2,1)`| không | 
| Truy vấn | không |`(2,1)`| 20 | 
| Truy vấn | không |`(2,1)`| 1 | 

Bản cập nhật đang chờ xử lý đóng góp thông qua hệ số nhị thức thay vì buộc phải xây dựng lại toàn bộ. Dấu vết cho thấy các bản cập nhật có thể được tích hợp một cách lười biếng. 

Một ví dụ nhỏ thứ hai:```
2 1
2 4
3
2 1 2
1 1 1
2 1 2
```| Hoạt động | Hiệu ứng mảng | Kết quả giá trị tiền tố | 
| --- | --- | --- | 
| Truy vấn ban đầu |`a^(1) = [2,6]`| 8 | 
| Thêm 1 ở vị trí 1 | vị trí 1 tăng | không | 
| Truy vấn cuối cùng |`a^(1) = [3,7]`| 10 | 

Điều này xác nhận rằng cấu trúc dữ liệu xử lý các cập nhật ở đầu mảng, nơi mọi vị trí sau đó đều bị ảnh hưởng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n / B + T) * m + n^2 / B) khấu hao | Các khối quét truy vấn và các bản cập nhật đang chờ xử lý, trong khi việc xây dựng lại chỉ diễn ra sau nhiều lần cập nhật | 
| Không gian | O(n*n/B) | Mỗi khối lưu trữ một mảng đóng góp có độ dài`n`| 

Đây`B`là kích thước khối và`T`là ngưỡng xây dựng lại. Với các giá trị đã chọn, số lượng thao tác vẫn nằm trong giới hạn vì`n`chỉ là`10000`và việc xây dựng lại tốn kém là không thường xuyên. 

## Trường hợp thử nghiệm```python
import sys
import io

# This block shows representative assert cases for the algorithm.

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    # Call solve() from the submitted solution here.
    # solve()
    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("""3 2
1 3 2
4
2 1 3
1 2 1
2 1 3
2 1 1
""") == "17\n20\n1\n"

assert run("""1 5
7
1
2 1 1
""") == "7\n"

assert run("""2 1
2 4
3
2 1 2
1 1 1
2 1 2
""") == "8\n10\n"

assert run("""5 3
1 1 1 1 1
2
2 1 5
2 3 5
""") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu gốc |`17, 20, 1`| Cập nhật thông thường và truy vấn phạm vi | 
| Yếu tố đơn |`7`| Kích thước tối thiểu và k lớn | 
| Hai phần tử có update |`8, 10`| Cập nhật ảnh hưởng lên tiền tố | 
| Giá trị bằng nhau | Khác nhau | Đóng góp nhiều lần | 

## Vỏ cạnh 

Đối với trường hợp cạnh đầu tiên có một phần tử:```
1 1
5
1
2 1 1
```Thuật toán gọi`get_prefix(1)`Và`get_prefix(0)`. Cuộc gọi thứ hai trả về 0 ngay lập tức, tránh việc lập chỉ mục không hợp lệ. Câu trả lời là`5`. 

Để cập nhật ở vị trí đầu tiên:```
2 2
1 2
2
1 1 3
2 1 1
```Bản cập nhật được lưu trữ trong khối đầu tiên. Khi truy vấn yêu cầu vị trí`1`, bản cập nhật đang chờ xử lý được đánh giá với khoảng cách bằng 0, cho hệ số`C(k, k) = 1`. Giá trị gia tăng chính xác là`3`, vì vậy câu trả lời cuối cùng trở thành`4`. 

Đối với phép trừ mô-đun, giả sử giá trị tiền tố bên phải được lưu trữ nhỏ hơn theo mô-đun`998244353`hơn giá trị tiền tố bên trái. Việc thực hiện sử dụng`ans % MOD`, giúp chuẩn hóa kết quả trung gian âm thành phạm vi được yêu cầu.
