---
title: "CF 105079B - Chấm bi"
description: "Chúng ta được cho một ngưỡng tối thiểu $n$ và chúng ta muốn chọn một số $x$ biểu thị số chấm bi trên một chiếc bánh cupcake. Con số này phải thỏa mãn hai ràng buộc cùng một lúc. Đầu tiên, $x ge n$. Thứ hai, $x$ không được là số nguyên tố."
date: "2026-06-27T21:25:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105079
codeforces_index: "B"
codeforces_contest_name: "UTPC x WiCS Contest 04-05-23 (UT Internal)"
rating: 0
weight: 105079
solve_time_s: 59
verified: true
draft: false
---

[CF 105079B - Polkadots](https://codeforces.com/problemset/problem/105079/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một ngưỡng tối thiểu$n$, và chúng tôi muốn chọn một số$x$đại diện cho số lượng chấm bi trên một chiếc bánh cupcake. Con số này phải thỏa mãn hai ràng buộc cùng một lúc. Đầu tiên,$x \ge n$. Thứ hai,$x$không được là số nguyên tố. Trong số tất cả các giá trị hợp lệ như vậy, chúng ta phải trả về giá trị nhỏ nhất có thể. 

Vì vậy, nhiệm vụ cơ bản là truy vấn “số hợp lệ tiếp theo” bắt đầu từ$n$, trong đó tính hợp lệ có nghĩa là “tổng hợp hoặc 1”, vì 1 không phải là số nguyên tố và được tính là chấp nhận được theo quy tắc “không phải số nguyên tố”. 

Ràng buộc$2 \le n \le 1000$ngay lập tức gợi ý rằng chúng ta đang ở trong một không gian tìm kiếm nhỏ. Ngay cả một lần quét ngây thơ trở lên từ$n$tệ nhất là 1000 lượt kiểm tra, con số này không đáng kể. Tính toán thực sự duy nhất trong mỗi lần kiểm tra là kiểm tra tính nguyên thủy. 

Các trường hợp cạnh tinh tế duy nhất xuất phát từ cách xác định tính nguyên tố xung quanh các số nguyên nhỏ. Số 2 là số nguyên tố nên nếu$n = 2$, chúng ta phải bỏ qua nó và tiến về phía trước. Số 3 cũng là số nguyên tố nên có thể xảy ra hiện tượng bỏ qua liên tiếp. Một trường hợp tinh tế khác là câu trả lời có thể bằng$n$chính nó nếu$n$chẳng hạn đã là số nguyên tố$n = 1000$. Một cách tiếp cận bất cẩn cho rằng chúng ta phải luôn tăng$n$sẽ không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là bắt đầu từ$x = n$và liên tục kiểm tra xem$x$là nguyên tố. Nếu là số nguyên tố thì ta tăng$x$và thử lại. Giá trị đầu tiên không vượt qua được bài kiểm tra tính nguyên thủy sẽ được trả về. 

Điều này hoạt động vì không gian tìm kiếm đơn điệu: khi một giá trị không hợp lệ (nguyên tố), chúng ta chỉ cần tiến về phía trước và giá trị hợp lệ đầu tiên gặp phải nhất thiết phải là tối ưu vì chúng ta quét theo thứ tự tăng dần. 

Nút cổ chai là thử nghiệm tính nguyên thủy. Một cuộc kiểm tra tính nguyên thủy ngây thơ đang diễn ra$O(\sqrt{x})$và chúng tôi có thể thực hiện nó tới khoảng 1000 lần trong trường hợp xấu nhất. Điều này mang lại khoảng$1000 \cdot 31$hoạt động, đó là tầm thường. Ngay cả khi triển khai chậm hơn một chút, nó vẫn nằm trong giới hạn. 

Một cách tối ưu hóa có cấu trúc hơn một chút là tính toán trước các số nguyên tố lên tới 1000 bằng cách sử dụng sàng, sau đó chỉ cần quét lên trên và kiểm tra mảng boolean. Điều này làm giảm mỗi lần kiểm tra xuống còn$O(1)$, làm cho nghiệm hoàn toàn tuyến tính trong trường hợp xấu nhất. 

Cả hai cách tiếp cận đều được chấp nhận; sàng sẽ sạch hơn nếu chúng ta mong đợi nhiều truy vấn, trong khi việc kiểm tra tính nguyên tố trực tiếp là đơn giản nhất đối với một đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (kiểm tra tính nguyên thủy từng bước) |$O((m-n)\sqrt{m})$|$O(1)$| Đã chấp nhận | 
| Sàng + Quét |$O(M \log \log M + (m-n))$|$O(M)$| Đã chấp nhận | 

Đây$M = 1000$. 

## Hướng dẫn thuật toán 

Chúng tôi mô tả cách tiếp cận dựa trên sàng vì nó làm cho lần quét cuối cùng trở nên đơn giản. 

### Các bước 

1. Xây dựng mảng boolean`is_prime`có kích thước 1001 được khởi tạo thành true cho tất cả các chỉ số từ 2 đến 1000. 

Chúng ta coi 0 và 1 ngay lập tức là không nguyên tố vì chúng không thỏa mãn định nghĩa nguyên tố. 
2. Chạy sàng Eratosthenes từ 2 đến 1000. 

Đối với mỗi số$i$vẫn được đánh dấu là số nguyên tố, đánh dấu tất cả bội số$2i, 3i, \dots$như không nguyên tố. 

Bước này đúng vì mọi hợp số đều phải có thừa số nguyên tố nhỏ nhất và nó sẽ bị loại bỏ khi lặp lại thừa số đó. 
3. Bắt đầu từ$x = n$và di chuyển lên trên. 
4. Đối với mỗi$x$, kiểm tra xem`is_prime[x]`là sai. 

Nếu sai thì xuất ngay$x$. 

Nếu không thì tăng$x$và tiếp tục. 

Giá trị đầu tiên gặp phải không phải là số nguyên tố là câu trả lời hợp lệ nhỏ nhất vì chúng ta đang quét theo thứ tự tăng dần mà không bỏ qua bất kỳ ứng viên nào. 

### Tại sao nó hoạt động 

Sàng phân chia chính xác các số thành số nguyên tố và không phải số nguyên tố lên đến 1000. Sau khi xử lý trước, các truy vấn về tính nguyên tố sẽ trở thành tra cứu theo thời gian liên tục. Quá trình quét từ$n$trở lên bảo toàn thứ tự, do đó, số nguyên tố đầu tiên gặp phải là tối thiểu khi xây dựng. Không có cách nào để bỏ lỡ một câu trả lời hợp lệ nhỏ hơn vì mọi ứng cử viên nhỏ hơn kết quả đầu ra đều đã được kiểm tra và bị từ chối làm số nguyên tố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

N = 1000

is_prime = [True] * (N + 1)
is_prime[0] = is_prime[1] = False

p = 2
while p * p <= N:
    if is_prime[p]:
        for m in range(p * p, N + 1, p):
            is_prime[m] = False
    p += 1

n = int(input().strip())

x = n
while True:
    if not is_prime[x]:
        print(x)
        break
    x += 1
```Sàng được chế tạo một lần trên phạm vi cố định lên tới 1000. Vòng bên trong bắt đầu tại$p^2$vì các bội số nhỏ hơn đã được đánh dấu bằng các số nguyên tố nhỏ hơn, tránh làm việc dư thừa. 

Vòng quét rất đơn giản: nó chỉ tiến lên cho đến khi tìm thấy hợp số hoặc 1. Điều kiện`not is_prime[x]`xử lý chính xác 1 là số không nguyên tố và cũng xử lý tất cả các số tổng hợp. 

## Ví dụ đã hoạt động 

### Ví dụ 1: Nhập 5 

Chúng tôi tính toán trước tính nguyên tố lên tới ít nhất là 5. 

| x | is_prime[x] | Hành động | 
| --- | --- | --- | 
| 5 | Đúng | bỏ qua | 
| 6 | Sai | dừng và xuất | 

Thuật toán kiểm tra 5 trước tiên, tìm số nguyên tố, sau đó chuyển sang 6, là số tổng hợp và trả về số đó. Điều này cho thấy rằng câu trả lời không nhất thiết phải lớn hơn$n$, chỉ không nguyên tố và ít nhất$n$. 

### Ví dụ 2: Nhập 2 

| x | is_prime[x] | Hành động | 
| --- | --- | --- | 
| 2 | Đúng | bỏ qua | 
| 3 | Đúng | bỏ qua | 
| 4 | Sai | đầu ra | 

Quá trình quét bỏ qua các số nguyên tố liên tiếp một cách chính xác. Đầu ra trở thành 4, là số tổng hợp nhỏ nhất lớn hơn hoặc bằng 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log \log N)$| sàng lên tới 1000 cộng với quét tuyến tính | 
| Không gian |$O(N)$| mảng boolean cho tính nguyên tố | 

Giới hạn hạn chế$n$ở mức 1000, do đó cả quá trình tiền xử lý và quét đều có thời gian không đổi trong thực tế. Giải pháp dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    N = 1000
    is_prime = [True] * (N + 1)
    is_prime[0] = is_prime[1] = False

    p = 2
    while p * p <= N:
        if is_prime[p]:
            for m in range(p * p, N + 1, p):
                is_prime[m] = False
        p += 1

    n = int(sys.stdin.readline().strip())
    x = n
    while True:
        if not is_prime[x]:
            return str(x)
        x += 1

# provided samples
assert run("2\n") == "4"
assert run("5\n") == "6"
assert run("1000\n") == "1000"

# custom cases
assert run("3\n") == "4", "next composite after prime"
assert run("4\n") == "4", "already composite"
assert run("6\n") == "6", "composite stays same"
assert run("997\n") == "1000", "jump across primes near upper bound"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 | 4 | tổ hợp tiếp theo ngay sau số nguyên tố | 
| 4 | 4 | trường hợp n đã hợp lệ | 
| 6 | 6 | trường hợp nhận dạng tổng hợp | 
| 997 | 1000 | hành vi ranh giới gần giới hạn trên | 

## Vỏ cạnh 

Đối với các số nguyên tố nhỏ như 2 và 3, quá trình quét phải bỏ qua nhiều giá trị liên tiếp một cách chính xác. Đối với đầu vào 2, thuật toán kiểm tra 2 và 3 là số nguyên tố, sau đó dừng ở 4. Điều này xác nhận rằng các lần tăng lặp lại không bỏ sót các ứng viên hợp lệ. 

Đối với các đầu vào đã là hỗn hợp, chẳng hạn như 4 hoặc 1000, lần kiểm tra đầu tiên sẽ thành công ngay lập tức vì`is_prime[x]`là sai. Vòng lặp không tăng thêm nữa, xác nhận tính đúng đắn khi câu trả lời bằng chính đầu vào.
