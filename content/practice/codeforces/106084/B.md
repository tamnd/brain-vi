---
title: "CF 106084B - Người bảo vệ song sinh"
description: "Chúng tôi được đưa ra một số truy vấn độc lập. Mỗi truy vấn chứa hai số nguyên và chúng ta cần quyết định xem hai số này có tạo thành một cặp số nguyên tố sinh đôi hay không."
date: "2026-06-20T12:59:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106084
codeforces_index: "B"
codeforces_contest_name: "2025 ICPC Asia Taiwan Online Programming Contest"
rating: 0
weight: 106084
solve_time_s: 43
verified: true
draft: false
---

[CF 106084B - Người bảo vệ song sinh](https://codeforces.com/problemset/problem/106084/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số truy vấn độc lập. Mỗi truy vấn chứa hai số nguyên và chúng ta cần quyết định xem hai số này có tạo thành một cặp số nguyên tố sinh đôi hay không. Khái niệm số nguyên tố sinh đôi ở đây rất cụ thể: hai số chỉ đủ điều kiện nếu chúng đều là số nguyên tố và hiệu của chúng chính xác là hai. 

Vì vậy, đối với mỗi cặp, nhiệm vụ không phải là phân tích nhân tử hoặc tính toán bất cứ điều gì phức tạp ngoài tính nguyên tố và kiểm tra mối quan hệ đơn giản giữa hai giá trị. Đầu ra là một quyết định nhị phân đơn giản cho mỗi trường hợp thử nghiệm: in phản hồi dương nếu cả hai số đều là số nguyên tố và khác nhau đúng hai, nếu không thì in phản hồi âm. 

Các ràng buộc đủ nhỏ để chúng ta có thể đủ khả năng kiểm tra tính nguyên tố trực tiếp trên mỗi số. Mỗi giá trị tối đa là 1.000.000 và có tối đa 10 trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi thứ như xử lý trước nặng nề cho mỗi truy vấn nhưng thoải mái cho phép kiểm tra tính nguyên thủy đơn giản trong$O(\sqrt{n})$hoặc một cái sàng được tính toán trước trong$O(N \log \log N)$. Ngay cả cách tiếp cận mỗi truy vấn chậm hơn cũng dễ dàng nằm trong giới hạn vì tổng số lần kiểm tra tính nguyên thủy là rất nhỏ. 

Các trường hợp khó phát hiện duy nhất đến từ các đầu vào gần như thỏa mãn điều kiện. Một cặp số có thể khác nhau hai đơn vị nhưng bao gồm một số tổng hợp như (12, 14) hoặc cả hai số có thể là số nguyên tố nhưng không khác nhau hai đơn vị như (11, 13) thực sự hợp lệ hoặc (5, 7) cũng hợp lệ. Một trường hợp cạnh khác là khi một số là số nguyên tố còn số kia thì không, ví dụ (3, 5) hợp lệ nhưng (2, 4) không thành công vì 4 không phải là số nguyên tố mặc dù chênh lệch là đúng. Những trường hợp này quan trọng vì việc triển khai đơn giản chỉ có thể kiểm tra sự khác biệt và quên đi hoàn toàn tính nguyên thủy. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi trường hợp thử nghiệm, chúng tôi kiểm tra độc lập xem cả hai số có phải là số nguyên tố hay không. Kiểm tra tính nguyên tố đơn giản nhất là thử chia số đó cho mọi số nguyên từ 2 cho đến căn bậc hai của nó. Nếu không có số nào có ước số, chúng ta chỉ chấp nhận cặp đó nếu hiệu của chúng đúng bằng hai. 

Điều này hoạt động chính xác vì tính nguyên thủy được xác định trực tiếp và không cần thủ thuật cấu trúc. Sự kém hiệu quả xuất hiện khi chúng ta xem xét trường hợp xấu nhất: việc kiểm tra tính nguyên tố cho một số lên tới một triệu cần khoảng 1.000 phép chia, do đó, một trường hợp thử nghiệm đơn lẻ sẽ tốn khoảng 2.000 lần kiểm tra. Với 10 trường hợp thử nghiệm, con số này chỉ là khoảng 20.000 thao tác, điều này vốn đã tầm thường, do đó, ngay cả cách tiếp cận bạo lực này cũng có thể chấp nhận được trong các ràng buộc. 

Tuy nhiên, chúng ta có thể đơn giản hóa việc suy luận hơn nữa bằng cách tính toán trước tất cả các số nguyên tố lên tới một triệu bằng cách sử dụng sàng. Khi mảng đó được xây dựng, mỗi truy vấn sẽ trở thành thời gian không đổi: hai lần tra cứu và một lần kiểm tra phép trừ. Việc sàng chuyển hoạt động từ thời gian truy vấn sang thời gian tiền xử lý, không đáng kể đối với kích thước ràng buộc này. 

Quan sát quan trọng là vấn đề hoàn toàn liên quan đến tư cách thành viên trong tập hợp các số nguyên tố và tập hợp đó được cố định cho tất cả các truy vấn. Điều đó làm cho quá trình tiền xử lý trở nên tốt hơn về mặt đơn giản và các truy vấn lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra tính nguyên thủy của Brute Force |$O(t \sqrt{n})$|$O(1)$| Đã chấp nhận | 
| Sàng + Tra cứu |$O(N \log \log N + t)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sử dụng một cái rây để đánh dấu tất cả các số nguyên tố lên tới một triệu. 

1. Xây dựng mảng boolean`is_prime`có kích thước 1.000.001, ban đầu giả sử tất cả các số đều là số nguyên tố ngoại trừ 0 và 1. Điều này thiết lập đường cơ sở trong đó chúng tôi chỉ loại bỏ các số tổng hợp đã biết. 
2. Chạy sàng từ số 2 trở lên. Đối với mỗi số vẫn được đánh dấu là số nguyên tố, hãy đánh dấu tất cả các bội số của nó là không nguyên tố. Bước này đảm bảo mọi số tổng hợp được loại bỏ chính xác một lần cho mỗi thừa số nguyên tố nhỏ nhất. 
3. Sau khi tiền xử lý, đọc từng cặp truy vấn (a, b). 
4. Đối với mỗi cặp, trước tiên hãy kiểm tra xem`b - a == 2`. Điều này là cần thiết vì các số nguyên tố sinh đôi phải cách nhau đúng hai. 
5. Nếu điều kiện chênh lệch không thành công, ngay lập tức xuất N vì không cần kiểm tra tính nguyên tố. 
6. Nếu điều kiện chênh lệch được giữ nguyên, hãy kiểm tra xem cả hai có`a`Và`b`được đánh dấu là số nguyên tố trong bảng sàng. 
7. Chỉ xuất Y nếu cả hai đều là số nguyên tố, nếu không thì xuất N. 

### Tại sao nó hoạt động 

Sàng đảm bảo phân loại chính xác mọi số nguyên đến giới hạn. Vì các số nguyên tố sinh đôi được xác định hoàn toàn bằng hai điều kiện nguyên tố độc lập cộng với ràng buộc sai phân cố định, nên việc kiểm tra tư cách thành viên trong tập hợp nguyên tố được tính toán trước tương đương với việc thực hiện kiểm tra tính nguyên tố đầy đủ. Thuật toán không bao giờ dựa vào kiểm tra một phần hoặc heuristic, vì vậy mọi cặp được chấp nhận đều thỏa mãn cả hai thuộc tính xác định một cách chính xác và mọi cặp bị từ chối đều vi phạm ít nhất một trong số chúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXN = 10**6

# Sieve of Eratosthenes
is_prime = [True] * (MAXN + 1)
is_prime[0] = is_prime[1] = False

p = 2
while p * p <= MAXN:
    if is_prime[p]:
        step = p
        start = p * p
        for x in range(start, MAXN + 1, step):
            is_prime[x] = False
    p += 1

t = int(input())
for _ in range(t):
    a, b = map(int, input().split())
    if b - a == 2 and is_prime[a] and is_prime[b]:
        print("Y")
    else:
        print("N")
```Sàng được xây dựng một lần trước khi xử lý bất kỳ truy vấn nào. Điều này đảm bảo mỗi lần kiểm tra sau này là một lần tra cứu mảng đơn giản. điều kiện`b - a == 2`được kiểm tra trước tiên để tránh việc truy cập bảng nguyên tố không cần thiết trong những trường hợp lỗi rõ ràng, mặc dù điều này chủ yếu mang tính phong cách do những hạn chế nhỏ. 

Ý tưởng cốt lõi là thông tin nguyên thủy được lưu trữ hoàn toàn vào bộ nhớ đệm, do đó, mỗi truy vấn sẽ giảm xuống mức xác minh liên tục. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`11 13`| Bước | một | b | b - một | is_prime[a] | is_prime[b] | Quyết định | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 11 | 13 | 2 | Đúng | Đúng | Y | 

Cả hai số đều là số nguyên tố và cách nhau đúng hai, do đó kết quả đầu ra là Y. 

### Ví dụ 2 

đầu vào:`12 14`| Bước | một | b | b - một | is_prime[a] | is_prime[b] | Quyết định | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 12 | 14 | 2 | Sai | Sai | N | 

Mặc dù điều kiện hiệu được thỏa mãn nhưng không có số nào là số nguyên tố nên cặp số bị loại bỏ. 

Những ví dụ này cho thấy rằng cả hai điều kiện đều cần thiết một cách độc lập và việc không đáp ứng được một trong hai điều kiện sẽ làm mất hiệu lực của cặp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log \log N + t)$| sàng tiền xử lý cộng với các truy vấn liên tục | 
| Không gian |$O(N)$| mảng boolean lưu trữ tính nguyên tố lên tới 1e6 | 

Các ràng buộc cho phép tối đa 10 truy vấn và mỗi truy vấn sẽ trở thành một lần tra cứu liên tục. Sàng chạy một lần và dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ cho$N = 10^6$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from io import StringIO as _StringIO

    # Re-run solution inline for testing
    input = sys.stdin.readline

    MAXN = 10**6
    is_prime = [True] * (MAXN + 1)
    is_prime[0] = is_prime[1] = False

    p = 2
    while p * p <= MAXN:
        if is_prime[p]:
            for x in range(p * p, MAXN + 1, p):
                is_prime[x] = False
        p += 1

    t = int(input())
    out = []
    for _ in range(t):
        a, b = map(int, input().split())
        if b - a == 2 and is_prime[a] and is_prime[b]:
            out.append("Y")
        else:
            out.append("N")

    return "\n".join(out)

# provided samples (as described in statement)
assert run("5\n2 3\n11 13\n12 14\n3 5\n5 7\n") == "N\nY\nN\nY\nY"

# custom cases
assert run("1\n2 4\n") == "N", "one prime, one composite"
assert run("1\n17 19\n") == "Y", "valid twin primes"
assert run("1\n19 21\n") == "N", "difference ok but composite"
assert run("1\n4 6\n") == "N", "both composite"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 4 | N | một số là hợp số mặc dù khoảng cách đúng | 
| 17 19 | Y | cặp số nguyên tố sinh đôi hợp lệ | 
| 19 21 | N | khoảng cách đúng nhưng số thứ hai không phải là số nguyên tố | 
| 4 6 | N | cả hai số đều không phải số nguyên tố | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi điều kiện sai phân được thỏa mãn nhưng tính nguyên tố không thành công. Ví dụ: đầu vào (12, 14) vượt qua kiểm tra khoảng cách nhưng cả hai số đều là hợp số. Sàng đánh dấu cả hai là sai, do đó thuật toán đưa ra chính xác N. 

Một trường hợp khác là khi cả hai số đều là số nguyên tố nhưng khoảng cách không chính xác là hai, chẳng hạn như (11, 13) hợp lệ nhưng (11, 17) bị bác bỏ mặc dù cả hai đều là số nguyên tố. Trước tiên, thuật toán sẽ kiểm tra sự khác biệt và ngay lập tức loại bỏ các khoảng trống không khớp, ngăn chặn công việc tra cứu không cần thiết. 

Trường hợp cuối cùng là số nguyên tố nhỏ nhất. Với (2, 3), hiệu bằng 1 nên bị bác bỏ mặc dù cả hai đều là số nguyên tố. Đối với (3, 5), cả hai điều kiện đều đúng và thuật toán xác định chính xác nó là cặp nguyên tố sinh đôi.
