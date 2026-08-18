---
title: "CF 102261B - \u0421\u043f\u043e\u0440\u0442\u0438\u0432\u043d\u044b\u0439 \u0442\u0443\u0440\u043d\u0438\u0440"
description: "Chúng ta được cung cấp danh sách các ván cờ được chơi trong một giải đấu loại trực tiếp. Có chính xác (n=2^k-1) trận đấu, vậy giải đấu phải có (2^k) người chơi và (k) vòng đấu."
date: "2026-08-17T20:36:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102261
codeforces_index: "B"
codeforces_contest_name: "\u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e - \u043a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u044f (\u042f\u043d\u0434\u0435\u043a\u0441)"
rating: 0
weight: 102261
solve_time_s: 120
verified: true
draft: false
---

[CF 102261B - \u0421\u043f\u043e\u0440\u0442\u0438\u0432\u043d\u044b\u0439 \u0442\u0443\u0440\u043d\u0438\u0440](https://codeforces.com/problemset/problem/102261/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp danh sách các ván cờ được chơi trong một giải đấu loại trực tiếp. Có chính xác (n=2^k-1) trận đấu, vậy giải đấu phải có (2^k) người chơi và (k) vòng đấu. Đối với mỗi trò chơi, chúng tôi chỉ biết hai người tham gia, không biết vòng đấu diễn ra và không biết ai thắng. 

Nhiệm vụ có hai phần cùng một lúc. Đầu tiên, chúng ta phải quyết định xem các cặp được ghi có thể thuộc về một khung loại trừ đơn hợp lệ nào đó hay không. Thứ hai, nếu có một khung như vậy, chúng ta phải in ra hai cầu thủ có thể lọt vào trận chung kết. Người chiến thắng thực sự của họ vẫn chưa được biết, vì vậy cả hai người vào chung kết đều có thể là người chiến thắng giải đấu. Phân tích chính thức sử dụng số lượng trò chơi mà mỗi người tham gia đã chơi để tái tạo lại các vòng đấu một cách gián tiếp. 

Gọi (d(v)) là số trận đấu được ghi có sự tham gia của người chơi (v). Người chơi thua ở vòng (r) có thể chơi chính xác (r) ván, trong khi người vào chung kết chơi tất cả (k) ván. Do đó, hai người vào chung kết phải chính xác là những người chơi có (d(v)=k), miễn là khung được ghi là hợp lệ. 

Ràng buộc (n\le 2^{16}-1=65535) có nghĩa là có thể có tối đa (65536) người tham gia riêng biệt. Việc đọc và xử lý mọi trò chơi với số lần không đổi dễ dàng đủ nhanh trong một giây, trong khi mọi thứ bậc hai về số lượng trò chơi sẽ yêu cầu các thao tác khoảng (4,3\cdot10^9) ở kích thước tối đa. Chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính. 

Có một số trường hợp tinh vi mà chỉ nhìn vào biểu đồ cơ bản là không đủ. Ví dụ: với ba trò chơi```
3
A B
B C
C A
```mỗi người chơi tham gia hai lần, vì vậy việc thực hiện bất cẩn có thể khiến ba người tham gia đều có thể lọt vào vòng chung kết. Câu trả lời đúng là`NO SOLUTION`, bởi vì ba trận trong một giải đấu loại trực tiếp bốn người chơi không thể tạo thành một chu kỳ. Vấn đề không chỉ nằm ở mức độ mà còn là liệu trò chơi có thể được chia thành các vòng hợp lệ hay không. 

Trường hợp quan trọng thứ hai là```
3
A B
C D
B C
```Đầu ra đúng là`B C`. Ở đây (A) và (D) chơi một lần, trong khi (B) và (C) chơi hai lần. Hai trận đầu tiên có thể là vòng đầu tiên, và`B C`có thể là trận chung kết. Một giải pháp giả định thứ tự đầu vào là thứ tự thời gian sẽ chấp nhận ví dụ này, nhưng giả định đó nói chung là không hợp lệ. 

Trường hợp thứ ba là việc tham gia nhiều lần vào cùng một vòng suy luận:```
3
A B
A B
C D
```Số lượng là (d(A)=d(B)=2) và (d(C)=d(D)=1). Hai trò chơi`A B`cả hai đều bị buộc vào vòng tương ứng với số lượng tham gia nhỏ hơn, vì vậy cùng một người chơi sẽ phải chơi hai lần trong một vòng. Câu trả lời đúng là`NO SOLUTION`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ cố gắng gán mọi trò chơi đã ghi vào một trong các vòng (k). Vòng (r) phải chứa chính xác (2^{k-r}) trò chơi, do đó số lần phân công có thể có của (n) trò chơi cho các vòng là 

[ 
\frac{n!}{\prod_{r=1}^{k}(2^{k-r})!}. 
] 

Đối với mỗi nhiệm vụ như vậy, chúng tôi có thể kiểm tra xem liệu không có người chơi nào xuất hiện hai lần trong cùng một vòng đấu hay không và liệu lịch thi đấu có tạo thành một giải đấu loại trực tiếp hay không. Mỗi nhiệm vụ ứng viên cần (\Theta(n)) công việc để kiểm tra, vì vậy số lượng thao tác trong trường hợp xấu nhất là 

[ 
\Theta\left( 
n\cdot 
\frac{n!}{\prod_{r=1}^{k}(2^{k-r})!} 
\đúng). 
] 

Ngay cả đối với (n=7), điều này đã xem xét nhiều khả năng và đối với (n=65535), biểu thức này hoàn toàn không khả thi. Lực lượng vũ phu hữu ích về mặt khái niệm vì nó cho chúng ta biết giải pháp hợp lệ cuối cùng phải được thiết lập là gì, nhưng nó coi các số tròn chưa biết là các lựa chọn độc lập khi chúng thực sự được xác định bởi số lượng người tham gia. 

Nhận xét quan trọng là trận đấu giữa người chơi (u) và (v) phải diễn ra trong hiệp đấu. 

[ 
\min(d(u),d(v)). 
] 

Giả sử (d(u)=3) và (d(v)=5). Người chơi (u) đã ngừng tham gia sau ván thứ ba, vì vậy trận đấu của họ với (v) phải là ván thứ ba và cũng là trận cuối cùng của họ. Do đó, trò chơi thuộc về vòng 3. Không còn quyền tự do nào trong vòng của nó một khi tất cả số lần tham gia đã được biết. 

Điều này cho phép chúng tôi xác định một vòng giả cho mỗi trò chơi. Trò chơi ((u,v)) thuộc vòng giả (r) khi (\min(d(u),d(v))=r). Trong một giải đấu loại trực tiếp thực sự, vòng (r) có chính xác (2^{k-r}) trận và mỗi người chơi xuất hiện tối đa một trận trong vòng đó. Điều đáng chú ý là hai điều kiện này không chỉ cần mà còn đủ. Đây là đặc tính trung tâm được sử dụng trong giải pháp chính thức. 

Tại sao sự đầy đủ là hợp lý? Hãy xem xét vòng giả 1. Mọi người tham gia phải xuất hiện ở đó, bởi vì bất kỳ ai đã chơi ít nhất một trò chơi đều phải bắt đầu giải đấu ở vòng 1. Sau khi vòng đầu tiên đó bị loại bỏ, mọi người tham gia còn lại đã chơi ít hơn một trò chơi một cách hiệu quả, do đó, lập luận tương tự được áp dụng đệ quy cho một giải đấu có ít vòng hơn. Điều này tạo ra cảm ứng trên (k). 

Thuật toán kết quả chỉ cần đếm số người tham gia, phân công vòng giả và kiểm tra xem mỗi vòng giả có chứa số lượng người tham gia riêng biệt được yêu cầu hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (\Theta\left(n\cdot\frac{n!}{\prod_r(2^{k-r})!}\right)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n)) dự kiến ​​| (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả (n) trò chơi và đếm xem mỗi người chơi xuất hiện bao nhiêu lần. Gọi giá trị này là (d(v)). Đây là thông tin duy nhất cần thiết để suy ra vòng đấu của mỗi trò chơi. 
2. Tính toán (k=\log_2(n+1)). Vì đầu vào đảm bảo (n=2^k-1), nên trong Python chúng ta có thể lấy nó trực tiếp dưới dạng`(n + 1).bit_length() - 1`. 
3. Với mỗi ván đấu đã ghi ((u,v)), hãy tính 

[ 
r=\min(d(u),d(v)). 
] 

Đưa trò chơi vào vòng giả (r). Số lượng người tham gia nhỏ hơn xác định người tham gia đã đến trò chơi cuối cùng của họ, vì vậy trò chơi này không thể diễn ra ở vòng sau. 
4. Với mỗi vòng giả (r) từ 1 đến (k-1), hãy xác minh rằng nó chứa chính xác 

[ 
2^{k-r} 
] 

trò chơi. Một vòng đấu thực sự có chính xác số trận đấu này vì có (2^{k-r+1}) người chơi vẫn còn sống khi bắt đầu. 
5. Đối với cùng một vòng giả, hãy duy trì một nhóm người tham gia. Từ chối đầu vào nếu một trong hai điểm cuối của trò chơi đã xuất hiện trong tập hợp đó. Một cầu thủ không thể thi đấu hai lần trong cùng một vòng đấu loại trực tiếp. 
6. Tìm tất cả người chơi có (d(v)=k). Nếu các lần kiểm tra trước đó thành công thì phải có chính xác hai người chơi như vậy. Họ là những người chơi duy nhất sống sót qua mọi vòng đấu nên họ là hai người vào chung kết. 
7. Viết in hai tên đó. Nếu bất kỳ kiểm tra tính hợp lệ nào không thành công, hãy in`NO SOLUTION`. 

### Tại sao nó hoạt động 

Điều bất biến là vòng giả (r) chứa chính xác các trò chơi phải diễn ra trong vòng thực (r). Đối với một trò chơi ((u,v)), ít nhất một điểm cuối phải dừng chơi ngay sau trò chơi đó, do đó vòng của nó chính xác là giá trị nhỏ hơn của (d(u)) và (d(v)). Do đó, không có khung hợp lệ nào có thể đặt trò chơi ở bất kỳ nơi nào khác. 

Nếu mỗi vòng giả có số lượng trò chơi chính xác và không có người tham gia nào xuất hiện hai lần trong một vòng giả thì vòng giả đầu tiên bao gồm các trận đấu rời rạc hợp lệ ở vòng đầu tiên. Mỗi trận đấu có một người chơi có số lần tham gia là 1 và một người chơi tiếp tục. Việc loại bỏ vòng giả đó sẽ giảm số lượng tham gia của mỗi người chơi còn sống xuống một người và để lại các điều kiện giống hệt cho một giải đấu có vòng (k-1). Trường hợp cơ bản là một trò chơi giữa hai người chơi. Bằng quy nạp, toàn bộ loạt trận đấu có thể được sắp xếp thành một bảng đấu loại trực tiếp hợp lệ. 

## Giải pháp Python```python
import sys
from collections import Counter

input = sys.stdin.readline

def solve():
    n = int(input())
    games = [tuple(input().split()) for _ in range(n)]

    # n = 2^k - 1, so n + 1 = 2^k.
    k = (n + 1).bit_length() - 1

    degree = Counter()

    for u, v in games:
        degree[u] += 1
        degree[v] += 1

    rounds = [[] for _ in range(k + 1)]

    for u, v in games:
        r = min(degree[u], degree[v])

        if r < 1 or r > k:
            print("NO SOLUTION")
            return

        rounds[r].append((u, v))

    for r in range(1, k):
        expected = 1 << (k - r)

        if len(rounds[r]) != expected:
            print("NO SOLUTION")
            return

        used = set()

        for u, v in rounds[r]:
            if u in used or v in used:
                print("NO SOLUTION")
                return

            used.add(u)
            used.add(v)

    finalists = [name for name, cnt in degree.items() if cnt == k]

    if len(finalists) != 2:
        print("NO SOLUTION")
        return

    print(finalists[0], finalists[1])

if __name__ == "__main__":
    solve()
```Lần vượt qua đầu tiên`games`xây dựng`degree`, tương ứng trực tiếp với số lượng tham gia (d(v)) từ thuật toán. MỘT`Counter`thuận tiện vì mọi họ đều có thể được sử dụng làm khóa từ điển. 

Đường chuyền thứ hai chỉ định mỗi trò chơi`rounds[min(degree[u], degree[v])]`. Đầu vào đảm bảo tổng số trò chơi có dạng (2^k-1), vì vậy`k`có thể được phục hồi mà không cần logarit dấu phẩy động. Sử dụng các thao tác bit cũng tránh được mọi vấn đề làm tròn. 

Chỉ các vòng giả từ 1 đến (k-1) mới cần kiểm tra va chạm rõ ràng. Sau khi các vòng đó thỏa mãn đặc điểm, các trò chơi còn lại nhất thiết phải tạo thành vòng cuối cùng. Mã vẫn lưu trữ vòng giả (k), nhưng không cần kiểm tra riêng. 

bộ`used`được tạo lại cho mỗi vòng. Điều này là cần thiết vì người chơi có thể xuất hiện một cách hợp pháp một lần trong nhiều vòng khác nhau. Việc từ chối một cái tên chỉ vì nó xuất hiện ở vòng trước sẽ từ chối một cách không chính xác những người chơi đã đạt đến giai đoạn sau. 

Cuối cùng, một giải đấu hợp lệ có đúng hai người chơi có số lần tham gia (k), tức là hai người vào chung kết. Sự rõ ràng`len(finalists) != 2`kiểm tra làm cho việc triển khai trở nên mạnh mẽ ngay cả khi đầu vào vi phạm các giả định về cấu trúc theo cách khiến cho các kiểm tra trước đó không đủ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Bảy trận đấu ngụ ý (k=3), vì vậy một giải đấu hợp lệ có bốn trận ở vòng 1, hai trận ở vòng 2 và một trận chung kết. 

Số lượng tham gia là 

| Người chơi | Trò chơi đã chơi | 
| --- | --- | 
| GORBOVSKII | 3 | 
| ABALKIN | 1 | 
| SIKORSKI | 2 | 
| KAMMERER | 1 | 
| BYKOV | 2 | 
| IURKOVSKII | 3 | 
| RIÊNG TƯ | 1 | 
| KIVRIN | 1 | 

Bây giờ hãy phân loại mọi trò chơi theo số lượng người tham gia nhỏ hơn. 

| Trò chơi | Đếm | Vòng giả | 
| --- | --- | --- | 
| GORBOVSKII ABALKIN | 3, 1 | 1 | 
| SIKORSKI KAMMERER | 2, 1 | 1 | 
| SIKORSKI GORBOVSKII | 2, 3 | 2 | 
| BYKOV IURKOVSKII | 2, 3 | 2 | 
| RIÊNG TƯ BYKOV | 1, 2 | 1 | 
| GORBOVSKII IURKOVSKII | 3, 3 | 3 | 
| IURKOVSKII KIVRIN | 3, 1 | 1 | 

Vòng giả 1 có bốn trò chơi và có tất cả tám người chơi đúng một lần. Vòng giả 2 có hai trò chơi và bao gồm`SIKORSKI`,`GORBOVSKII`,`BYKOV`, Và`IURKOVSKII`đúng một lần. Trận chung kết là`GORBOVSKII IURKOVSKII`. 

Hai người chơi có bậc 3 là`GORBOVSKII`Và`IURKOVSKII`, do đó thuật toán sẽ in chúng. 

### Mẫu 2 

Ở đây (n=3), do đó (k=2). 

| Người chơi | Trò chơi đã chơi | 
| --- | --- | 
| IVANOV | 2 | 
| PETROV | 2 | 
| BOSHIROV | 2 | 

Trò chơi nào cũng có vòng giả 

[ 
\min(2,2)=2. 
] 

Do đó, vòng giả 1 chứa 0 trò chơi thay vì hai trò chơi bắt buộc, trong khi vòng giả 2 chứa cả ba trò chơi thay vì một trò chơi bắt buộc. 

Thuật toán ngay lập tức từ chối đầu vào. Điều này chứng tỏ tại sao biết rằng có ba người tham gia có cùng mức độ tối đa là không đủ. Một bảng đấu loại trực tiếp bốn người hợp lệ phải có hai trận đấu ở vòng đầu tiên trước trận chung kết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) dự kiến ​​| Mỗi trò chơi được xử lý với số lần không đổi và các thao tác cố định được mong đợi (O(1)). | 
| Không gian | (O(n)) | Có tối đa (2n) lần xuất hiện của người tham gia và (n) trò chơi được lưu trữ. | 

Ở mức tối đa (n=65535), thuật toán chỉ thực hiện một vài lượt tuyến tính trong khoảng 65 nghìn trò chơi. Số lượng người tham gia riêng biệt nhiều nhất là (65536), do đó, từ điển, danh sách và bộ tạm thời đều nằm trong giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io
from collections import Counter

def solve():
    input = sys.stdin.readline

    n = int(input())
    games = [tuple(input().split()) for _ in range(n)]

    k = (n + 1).bit_length() - 1

    degree = Counter()
    for u, v in games:
        degree[u] += 1
        degree[v] += 1

    rounds = [[] for _ in range(k + 1)]

    for u, v in games:
        r = min(degree[u], degree[v])
        if r < 1 or r > k:
            print("NO SOLUTION")
            return
        rounds[r].append((u, v))

    for r in range(1, k):
        expected = 1 << (k - r)

        if len(rounds[r]) != expected:
            print("NO SOLUTION")
            return

        used = set()

        for u, v in rounds[r]:
            if u in used or v in used:
                print("NO SOLUTION")
                return
            used.add(u)
            used.add(v)

    finalists = [name for name, cnt in degree.items() if cnt == k]

    if len(finalists) != 2:
        print("NO SOLUTION")
        return

    print(finalists[0], finalists[1])

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def normalized(s: str):
    if s == "NO SOLUTION":
        return s
    return tuple(sorted(s.split()))

sample1 = """\
7
GORBOVSKII ABALKIN
SIKORSKI KAMMERER
SIKORSKI GORBOVSKII
BYKOV IURKOVSKII
PRIVALOV BYKOV
GORBOVSKII IURKOVSKII
IURKOVSKII KIVRIN
"""

sample2 = """\
3
IVANOV PETROV
PETROV BOSHIROV
BOSHIROV IVANOV
"""

assert normalized(run(sample1)) == ("GORBOVSKII", "IURKOVSKII"), "sample 1"
assert run(sample2) == "NO SOLUTION", "sample 2"

minimum_valid = """\
3
A B
C D
B C
"""
assert normalized(run(minimum_valid)) == ("B", "C"), "minimum valid bracket"

all_equal_degrees = """\
3
A B
B C
C A
"""
assert run(all_equal_degrees) == "NO SOLUTION", "cycle with equal degrees"

duplicate_in_round = """\
3
A B
A B
C D
"""
assert run(duplicate_in_round) == "NO SOLUTION", "same player twice in one round"

def make_maximum_valid():
    k = 16
    players = [f"P{i}" for i in range(1 << k)]
    current = players
    games = []

    while len(current) > 1:
        nxt = []
        for i in range(0, len(current), 2):
            u = current[i]
            v = current[i + 1]
            games.append((u, v))
            nxt.append(v)
        current = nxt

    lines = [str(len(games))]
    lines.extend(f"{u} {v}" for u, v in games)
    return "\n".join(lines) + "\n"

maximum_valid = make_maximum_valid()
maximum_answer = normalized(run(maximum_valid))
assert maximum_answer == ("P32767", "P65535"), "maximum valid bracket"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 / A B / C D / B C`|`B C`| Giải đấu hợp lệ tối thiểu và phát hiện chính xác vòng chung kết | 
|`3 / A B / B C / C A`|`NO SOLUTION`| Cả ba người tham gia đều có trình độ ngang nhau nhưng các trò chơi không thể xếp thành một khung | 
|`3 / A B / A B / C D`|`NO SOLUTION`| Một người tham gia xuất hiện hai lần trong một vòng suy luận | 
| Khung trò chơi đã tạo (65535) |`P32767 P65535`| Kích thước đầu vào tối đa, ranh giới tròn và xử lý thời gian tuyến tính | 

## Vỏ cạnh 

Chu kỳ ba người chơi```
3
A B
B C
C A
```cho (k=2) và độ (d(A)=d(B)=d(C)=2). Mọi trò chơi được gán cho vòng giả 2, vì mức tối thiểu luôn là 2. Số trò chơi cần thiết trong vòng giả 1 là (2^{2-1}=2), nhưng nó chứa 0, do đó thuật toán trả về`NO SOLUTION`. Điều này dẫn đến sai lầm khi chỉ coi bằng cấp của người tham gia là đủ. 

Giải đấu hợp lệ tối thiểu```
3
A B
C D
B C
```có độ (1,2,2,1). Các trò chơi`A B`Và`C D`được gán cho vòng giả 1, trong khi`B C`được ấn định vào vòng giả 2. Vòng đầu tiên có hai trò chơi riêng biệt và vòng thứ hai có một trận chung kết. Hai người chơi có bậc 2 là`B`Và`C`, vì vậy đầu ra là`B C`. 

Trường hợp trò chơi lặp đi lặp lại```
3
A B
A B
C D
```có độ (2,2,1,1). Cả hai bản sao của`A B`được gán cho vòng giả 2, trong khi`C D`được xếp vào vòng giả 1. Vòng giả 1 có số ván chơi đúng, nhưng vòng giả 2 có hai ván thay vì một. Việc kiểm tra số lượng sẽ từ chối nó trước khi những người tham gia lặp lại có thể gây ra bất kỳ sự mơ hồ nào. 

Ở kích thước tối đa, (n=65535) cung cấp (k=16) và chính xác (65536) người chơi. Vòng giả đầu tiên phải chứa (32768) trò chơi rời rạc, vòng thứ hai (16384), v.v. cho đến vòng chung kết. Bài kiểm tra tối đa được tạo tuân theo chính xác cấu trúc này và hai người chơi xuất hiện trong tất cả 16 vòng đấu là`P32767`Và`P65535`. Việc triển khai không bao giờ tự xây dựng cây khung mà chỉ xác minh cấu trúc vòng được ngụ ý bởi số lượng người tham gia, đó là lý do tại sao cùng một phương pháp tuyến tính xử lý các giải đấu hợp lệ nhỏ nhất và lớn nhất.
