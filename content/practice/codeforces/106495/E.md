---
title: "CF 106495E - Erasmus Valthron"
description: "Thư viện chứa mọi số nguyên từ 1 đến N. Mỗi số nguyên được chuyển đổi thành dạng chính tắc bằng cách viết các thừa số nguyên tố theo thứ tự không giảm."
date: "2026-06-25T08:39:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106495
codeforces_index: "E"
codeforces_contest_name: "2026 ICPC Gran Premio de Mexico 1ra Fecha"
rating: 0
weight: 106495
solve_time_s: 48
verified: true
draft: false
---

[CF 106495E - Erasmus Valthron](https://codeforces.com/problemset/problem/106495/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Thư viện chứa mọi số nguyên từ 1 đến N. Mỗi số nguyên được chuyển đổi thành dạng chính tắc bằng cách viết các thừa số nguyên tố theo thứ tự không giảm. Sau đó, các số nguyên được sắp xếp theo thứ tự từ điển của các chuỗi nhân tố này và mỗi truy vấn sẽ hỏi số nguyên nào xuất hiện ở một vị trí nhất định theo thứ tự mới này. 

Ví dụ: số 12 trở thành`[2,2,3]`, trong khi 15 trở thành`[3,5]`. Vì phần tử đầu tiên của`[2,2,3]`nhỏ hơn phần tử đầu tiên của`[3,5]`, 12 xuất hiện sớm hơn. Số đặc biệt 1 có một chuỗi thừa số trống và một chuỗi trống nhỏ hơn mọi chuỗi không trống, vì vậy 1 luôn đứng đầu. 

Các ràng buộc đủ lớn để trực tiếp tạo ra mọi chuỗi nhân tố và sắp xếp tất cả các số là không thực tế. Với N đạt 10^6, cách tiếp cận xung quanh O(N log N) với các phép so sánh nặng nề đã có rủi ro trong Python và giải pháp phải gần với tuyến tính. Số lượng truy vấn cũng lớn nên việc trả lời từng truy vấn bằng cách tính toán lại thứ tự là không thể. Việc đặt hàng phải được chuẩn bị một lần. 

Các trường hợp quan trọng xuất phát từ thứ tự từ điển bất thường. Một số phải xuất hiện trước tất cả các phần mở rộng của nó vì một chuỗi nhỏ hơn một chuỗi dài hơn bắt đầu bằng các phần tử giống nhau. Ví dụ, với đầu vào```
5 5
1
2
3
4
5
```thứ tự bắt đầu bằng 1 vì chuỗi nhân tố của nó trống. Thứ tự còn lại dựa trên`[2]`,`[2,2]`,`[3]`,`[5]`, cho đầu ra```
1
2
4
3
5
```Việc thực hiện bất cẩn chỉ so sánh thừa số nguyên tố đầu tiên sẽ đặt sai số 4 sau tất cả các số bắt đầu bằng số nguyên tố lớn hơn. 

Một sai lầm phổ biến khác là cho phép các yếu tố giảm đi. Thứ tự của 12 là`[2,2,3]`, không`[3,2,2]`. Nếu quá trình truyền tải cho phép một số nguyên tố nhỏ hơn sau một số nguyên tố lớn hơn, thì nó sẽ tạo ra các dạng chính tắc không hợp lệ. Ví dụ: một phép duyệt cho phép giảm hệ số có thể coi 12 là con của 3 theo sau là 2, làm thay đổi thứ tự. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là tính chuỗi thừa số nguyên tố của mọi số nguyên, lưu trữ tất cả N chuỗi và sắp xếp các số bằng cách sử dụng so sánh từ điển. Điều này đúng vì thứ tự yêu cầu chính xác là thứ tự của các chuỗi đó. Vấn đề là chi phí. Có thể có tới một triệu chuỗi và việc so sánh các chuỗi dài trong quá trình sắp xếp có thể khiến tổng công việc lớn hơn nhiều so với giới hạn sẵn có. 

Quan sát hữu ích là các dạng kinh điển tự nhiên tạo thành một tri thức. Một nút đại diện cho tiền tố của các thừa số nguyên tố, cũng là một số. Ví dụ, đường dẫn```
1 -> 2 -> 4 -> 12
```đại diện cho các tiền tố yếu tố```
[]
[2]
[2,2]
[2,2,3]
```Việc duyệt theo chiều sâu đầu tiên của bộ ba này sẽ đưa ra chính xác thứ tự từ điển cần thiết. Số hiện tại được truy cập trước các số con của nó vì một chuỗi nhỏ hơn phần mở rộng của nó. Trẻ em được thăm theo thứ tự tăng dần của thừa số nguyên tố tiếp theo. 

Thay vì xây dựng toàn bộ bộ ba đòi hỏi phải lưu trữ nhiều cạnh, chúng ta có thể tạo trực tiếp các phần tử con. Nếu một số có giá trị hiện tại x và thừa số tiếp theo được chọn từ chỉ số nguyên tố i thì mọi đứa trẻ đều được`x * prime[j]`vì`j >= i`. Việc chuyển chỉ số nguyên tố hiện tại xuống DFS sẽ tự động thực thi các hệ số không giảm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N log N) hoặc tệ hơn tùy thuộc vào so sánh trình tự | O(N log N) | Quá chậm | 
| Trie DFS Generation | O(N + chi phí truyền tải) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách tất cả các số nguyên tố đến N bằng sàng. Quá trình truyền tải cần các số nguyên tố theo thứ tự tăng dần vì các nút con của mỗi nút trie phải được xử lý theo thứ tự từ điển. 
2. Bắt đầu tìm kiếm theo chiều sâu đầu tiên từ gốc đại diện cho chuỗi nhân tố trống. Căn nguyên tương ứng với số 1 nên nó là phần tử đầu tiên được thêm vào thứ tự câu trả lời. 
3. Trong DFS ở số x, hãy thêm x vào thứ tự kết quả trước khi khám phá phần tử con. Điều này xử lý quy tắc rằng một chuỗi xuất hiện trước mọi chuỗi dài hơn có cùng tiền tố. 
4. Hãy thử mọi số nguyên tố p bắt đầu từ chỉ số nguyên tố nhỏ nhất được phép. Nếu như`x * p`nhiều nhất là N, truy cập đệ quy`x * p`trong khi vẫn giữ nguyên chỉ số nguyên tố, vì cùng một số nguyên tố có thể được sử dụng lại. 
5. Lưu trữ mảng thứ tự đầy đủ. Mọi truy vấn sau đó có thể được trả lời bằng cách lập chỉ mục trực tiếp. 

Tại sao nó hoạt động: DFS truy cập trie theo thứ tự trước. Nút trie đại diện cho một chuỗi nhân tố chính tắc và các nút con của nó đại diện cho tất cả các cách hợp lệ để mở rộng chuỗi đó theo một thừa số nguyên tố. Đặt hàng trước đặt một nút trước nút con của nó, khớp với thứ tự từ điển vì các tiền tố bằng nhau ngắn hơn sẽ được xếp trước. Xử lý các phần tử con bằng cách tăng bậc nguyên tố phù hợp với phép so sánh thừa số sai phân thứ nhất. Vì mỗi số nguyên có đúng một thừa số nguyên tố không giảm nên mỗi số xuất hiện đúng một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())

    sieve = bytearray(b'\x01') * (n + 1)
    if n >= 0:
        sieve[0] = 0
    if n >= 1:
        sieve[1] = 0

    primes = []
    for i in range(2, n + 1):
        if sieve[i]:
            primes.append(i)
            if i * i <= n:
                sieve[i * i:n + 1:i] = b'\x00' * (((n - i * i) // i) + 1)

    order = []

    sys.setrecursionlimit(1000000)

    def dfs(x, start):
        order.append(x)
        limit = n // x
        for i in range(start, len(primes)):
            p = primes[i]
            if p > limit:
                break
            dfs(x * p, i)

    dfs(1, 0)

    ans = []
    for _ in range(q):
        k = int(input())
        ans.append(str(order[k - 1]))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Sàng tạo ra danh sách nguyên tố có thứ tự cần thiết cho quá trình duyệt. Nó không cần lưu trữ hệ số nguyên tố nhỏ nhất của mỗi số vì cấu trúc DFS đã đảm bảo các hệ số hợp lệ. 

Hàm đệ quy lưu trữ số hiện tại ngay lập tức và sau đó khám phá các phần mở rộng có thể có. tham số`start`là chỉ số của số nguyên tố nhỏ nhất vẫn có thể chọn được. Đi qua`i`thay vì`i + 1`là cần thiết vì các yếu tố lặp đi lặp lại như`[2,2,2]`là hợp lệ. 

Việc kiểm tra phép nhân được thực hiện thông qua`limit = n // x`thay vì thử nghiệm`x * p <= n`nhiều lần. Điều này tránh được các giá trị trung gian lớn không cần thiết và làm cho điều kiện biên trở nên rõ ràng. 

Mảng cuối cùng sử dụng tính năng lập chỉ mục dựa trên số 0 trong nội bộ, vì vậy vị trí truy vấn`k`được trả lời với`order[k - 1]`. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
10 10
1
2
3
4
5
6
7
8
9
10
```Trạng thái DFS phát triển như sau. 

| Số hiện tại | Được phép chỉ số nguyên tố tiếp theo | Hành động | Đặt hàng cho đến nay | 
| --- | --- | --- | --- | 
| 1 | 0 | Thêm 1 | 1 | 
| 2 | 0 | Thêm 2 | 1,2 | 
| 4 | 0 | Thêm 4 | 1,2,4 | 
| 8 | 0 | Thêm 8 | 1,2,4,8 | 
| 6 | 1 | Thêm 6 | 1,2,4,8,6 | 
| 10 | 2 | Thêm 10 | 1,2,4,8,6,10 | 
| 3 | 1 | Thêm 3 | 1,2,4,8,6,10,3 | 
| 9 | 1 | Thêm 9 | 1,2,4,8,6,10,3,9 | 
| 5 | 2 | Thêm 5 | 1,2,4,8,6,10,3,9,5 | 
| 7 | 3 | Thêm 7 | 1,2,4,8,6,10,3,9,5,7 | 

Điều này chứng tỏ rằng con cháu của số 2 được hoàn thành trước khi chuyển sang số 3 vì mọi dãy bắt đầu bằng`[2]`nhỏ hơn mọi dãy bắt đầu bằng`[3]`. 

Đối với một ví dụ nhỏ hơn:```
5 5
1
2
3
4
5
```| Số hiện tại | Lý do | Đặt hàng cho đến nay | 
| --- | --- | --- | 
| 1 | Dãy trống nhỏ nhất | 1 | 
| 2 | Con đầu tiên của root | 1,2 | 
| 4 | Sự mở rộng`[2,2]`đến trước`[3]`| 1,2,4 | 
| 3 | Con gốc tiếp theo | 1,2,4,3 | 
| 5 | Đứa con cuối cùng còn lại | 1,2,4,3,5 | 

Điều này xác nhận quy tắc tiền tố. Số 4 xuất hiện trước số 3 vì`[2,2]`bắt đầu với thừa số thứ nhất nhỏ hơn`[3]`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + chi phí truyền tải) | Mỗi số được tạo một lần và mỗi cạnh DFS tương ứng với việc thêm một thừa số nguyên tố. | 
| Không gian | O(N) | Mảng câu trả lời lưu trữ một số nguyên cho mỗi số. | 

N tối đa là một triệu nên việc lưu trữ thứ tự là khả thi. Việc truyền tải tránh việc sắp xếp các bộ sưu tập lớn và chỉ thực hiện công việc tỷ lệ với cấu trúc trie được tạo ra. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("""10 10
1
2
3
4
5
6
7
8
9
10
""") == """1
2
4
8
6
10
3
9
5
7
"""

assert run("""10 4
1
4
7
10
""") == """1
8
3
7
"""

assert run("""5 1
5
""") == """5
"""

assert run("""1 3
1
1
1
""") == """1
1
1
"""

assert run("""8 8
1
2
3
4
5
6
7
8
""") == """1
2
4
8
6
3
5
7
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| N = 1 | 1 | Thư viện nhỏ nhất có thể | 
| N = 5 | Trật tự thử đầy đủ | Thứ tự tiền tố | 
| N = 8 | Quyền hạn của hai và anh chị em | Thừa số nguyên tố lặp lại | 
| N = 10 | Đặt hàng mẫu | Độ chính xác truyền tải chung | 

## Vỏ cạnh 

Đối với trường hợp chuỗi nhân tố trống:```
1 1
1
```DFS bắt đầu từ gốc và ngay lập tức ghi lại 1. Không có con nào vì không có số nguyên tố nào có thể được nhân trong khi vẫn nằm trong giới hạn. Câu trả lời là 1. 

Đối với các thừa số nguyên tố lặp lại:```
5 5
1
2
3
4
5
```Khi DFS đạt tới 2, nó giữ nguyên chỉ số nguyên tố đó, do đó nó truy cập 4 thông qua chuỗi nhân tố`[2,2]`. Điều này xảy ra trước khi chuyển sang 3 vì hệ số khác biệt đầu tiên là 2 so với 3. 

Đối với các số có nhiều phần mở rộng hợp lệ:```
10 10
1
2
3
4
5
6
7
8
9
10
```Cây con gốc ở 2 chứa 4, 8, 6 và 10. Quá trình duyệt kết thúc cây con đó trước khi truy cập các số bắt đầu bằng 3, 5 hoặc 7. Đây chính xác là hành vi từ điển được yêu cầu bởi các dạng chuẩn.
