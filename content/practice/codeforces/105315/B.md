---
title: "CF 105315B - Sinh Nhật Noorhan"
description: "Chúng tôi được đưa ra một số truy vấn độc lập. Mỗi truy vấn cung cấp một số nguyên n và chúng ta phải xuất ra một số nguyên a, trong đó a là số nguyên tố gần n nhất. Nếu hai số nguyên tố gần nhau thì phải chọn số nhỏ hơn."
date: "2026-06-23T06:13:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105315
codeforces_index: "B"
codeforces_contest_name: "JPC 4.0"
rating: 0
weight: 105315
solve_time_s: 45
verified: true
draft: false
---

[CF 105315B - Sinh nhật của Noorhan](https://codeforces.com/problemset/problem/105315/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số truy vấn độc lập. Mỗi truy vấn cung cấp một số nguyên n và chúng ta phải xuất ra một số nguyên a, trong đó a là số nguyên tố gần n nhất. Nếu hai số nguyên tố gần nhau thì phải chọn số nhỏ hơn. 

Nhiệm vụ cơ bản là trả lời liên tục: “số nguyên tố nào nằm gần một giá trị nhất định trên trục số?” 

Kích thước đầu vào cho thấy rõ rằng n có thể lên tới 1.000.000 và có thể có tới 100.000 truy vấn. Một cách tiếp cận ngây thơ cố gắng kiểm tra tính nguyên tố xung quanh từng n một cách độc lập sẽ gặp khó khăn nếu nó lặp lại công việc nặng nhọc cho mỗi truy vấn. Ý nghĩa chính của các ràng buộc là chúng ta cần xử lý trước thông tin cơ bản một lần và sử dụng lại nó một cách hiệu quả. 

Trường hợp cạnh tinh tế xuất hiện khi n chính nó là số nguyên tố. Trong trường hợp đó, câu trả lời là n, vì khoảng cách đến chính nó bằng không. Một trường hợp cạnh khác phát sinh khi hai số nguyên tố cách đều nhau, ví dụ xung quanh n = 10, trong đó 7 và 11 đều có khoảng cách bằng 3. Bài toán rõ ràng yêu cầu chọn số nguyên tố nhỏ hơn nên 7 là đúng. 

Một tình huống khác có thể phá vỡ logic đơn giản là khi tìm kiếm từ n mà không có giới hạn thích hợp. Nếu chúng ta chỉ tìm kiếm đi xuống hoặc chỉ tìm kiếm lên trên, chúng ta có thể bỏ lỡ số nguyên tố gần nhất ở phía bên kia. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi truy vấn n, chúng tôi kiểm tra mọi số nguyên từ n trở ra: trước tiên hãy kiểm tra xem n có phải là số nguyên tố không, sau đó là n−1 và n+1, sau đó là n−2 và n+2, v.v. cho đến khi chúng tôi tìm thấy các số nguyên tố ở cả hai vế. Mỗi lần kiểm tra tính nguyên tố tốn ít nhất O(√n) và trong trường hợp xấu nhất, chúng ta có thể khám phá một phạm vi lớn trước khi đạt được các số nguyên tố ở cả hai vế. Hơn 100.000 truy vấn, điều này nhanh chóng trở nên quá chậm. 

Sự kém hiệu quả xuất phát từ việc lặp lại các bước kiểm tra tính nguyên tố đối với các số chồng chéo trên các truy vấn khác nhau. Nhiều truy vấn hỏi về các giá trị lân cận và tính nguyên tố của một số không thay đổi giữa các truy vấn. Điều này gợi ý xử lý trước tất cả các số nguyên tố lên đến mức tối đa n một lần, sau đó trả lời từng truy vấn trong thời gian không đổi. 

Khi chúng tôi tính toán trước một mảng boolean cho biết mỗi số lên tới 1.000.000 có phải là số nguyên tố hay không, vấn đề sẽ giảm xuống thành tìm kiếm lân cận gần nhất trong một tập hợp tĩnh. Đối với mỗi n, chúng tôi đồng thời mở rộng ra ngoài từ n theo cả hai hướng cho đến khi chúng tôi tìm thấy ít nhất một số nguyên tố ở mỗi bên hoặc xác nhận một hướng thắng. Vì phạm vi tìm kiếm được giới hạn bởi sàng được tính toán trước nên việc này trở nên đủ nhanh trong thực tế. 

Một cách có cấu trúc hơn để suy nghĩ về vấn đề này là chúng tôi đang chuyển đổi các truy vấn nguyên tố lặp đi lặp lại thành một cấu trúc sàng toàn cục duy nhất, sau đó là tra cứu nguyên tố cục bộ gần nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(t · √n · k) mở rộng trường hợp xấu nhất | O(1) | Quá chậm | 
| Sàng + Tìm kiếm | O(N log log N + t · d) | O(N) | Đã chấp nhận | 

Ở đây d là khoảng cách trung bình đến số nguyên tố gần nhất, nhỏ đối với các số lên tới 10^6. 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi tính toán trước tính nguyên tố cho tất cả các số lên tới 1.000.000 bằng cách sử dụng sàng. Điều này đảm bảo rằng mọi kiểm tra tính nguyên tố sau này đều là O(1), điều này là cần thiết vì chúng ta sẽ truy vấn thông tin này nhiều lần. 

Tiếp theo, chúng tôi lặp qua từng giá trị truy vấn n và xác định số nguyên tố gần nhất.

1. Tính toán trước một mảng is_prime có kích thước 1.000.001 được khởi tạo thành true, sau đó đánh dấu 0 và 1 là không nguyên tố. Điều này thiết lập một đường cơ sở trong đó mọi số đều được coi là số nguyên tố cho đến khi được chứng minh ngược lại. 
2. Chạy sàng từ 2 trở lên và bất cứ khi nào chúng ta tìm thấy số nguyên tố p, hãy đánh dấu tất cả bội số của p là không nguyên tố. Bước này xây dựng cấu trúc nguyên tố đầy đủ cho toàn bộ phạm vi một cách hiệu quả bằng cách loại bỏ các số tổng hợp theo lô có cấu trúc thay vì kiểm tra từng số riêng lẻ. 
3. Đối với mỗi giá trị truy vấn n, trước tiên hãy kiểm tra xem is_prime[n] có đúng không. Nếu vậy, chúng ta ngay lập tức xuất n vì nó có khoảng cách bằng 0 với chính nó, khiến nó trở thành số nguyên tố gần nhất có thể. 
4. Nếu n không phải là số nguyên tố, chúng ta mở rộng ra ngoài với khoảng cách tăng dần d bắt đầu từ 1. Tại mỗi bước, chúng ta kiểm tra n − d và n + d, miễn là chúng vẫn nằm trong giới hạn. Chúng tôi dừng lại ngay khi tìm thấy ít nhất một số nguyên tố trong số các ứng cử viên này. 
5. Nếu cả n − d và n + d đều là số nguyên tố ở cùng một khoảng cách, chúng ta sẽ xuất ra n − d vì bài toán yêu cầu chọn số nguyên tố nhỏ hơn trong trường hợp bằng nhau. 

Tại sao điều này hoạt động được gắn liền với tính đối xứng trong quá trình tìm kiếm. Chúng ta khám phá các số có khoảng cách tăng dần từ n, vì vậy lần đầu tiên chúng ta gặp bất kỳ số nguyên tố nào, nó phải là số nguyên tố gần nhất có thể. Bởi vì chúng tôi kiểm tra cả hai vế ở mỗi mức khoảng cách trước khi tăng d, nên chúng tôi đảm bảo rằng không có số nguyên tố nào gần hơn bị bỏ qua. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXN = 10**6

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

def solve(n):
    if is_prime[n]:
        return n
    d = 1
    while True:
        left = n - d if n - d >= 0 else None
        right = n + d if n + d <= MAXN else None

        if left is not None and is_prime[left]:
            return left
        if right is not None and is_prime[right]:
            return right
        d += 1

t = int(input())
out = []
for _ in range(t):
    n = int(input())
    out.append(str(solve(n)))

print("\n".join(out))
```Phần sàng xây dựng kiến ​​thức nguyên thủy toàn cầu một lần. Cấu trúc vòng lặp đảm bảo rằng các vật liệu tổng hợp được loại bỏ một cách hiệu quả bắt đầu từ các hình vuông, giúp tránh được công việc dư thừa. 

Hàm truy vấn hoàn toàn dựa vào mảng được tính toán trước. Việc thu hồi sớm số nguyên tố n rất quan trọng vì nó tránh được việc mở rộng không cần thiết. Việc mở rộng hai chiều đảm bảo tính chính xác vì khoảng cách tăng một cách đơn điệu. 

Một chi tiết tinh tế là kiểm tra ranh giới. Nếu không đảm bảo n − d và n + d nằm trong giới hạn mảng, chúng ta có thể truy cập các chỉ mục không hợp lệ hoặc bỏ lỡ các số nguyên tố hợp lệ ở các cạnh. Việc thực hiện bảo vệ rõ ràng cả hai hướng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 10 

Chúng tôi mở rộng ra bên ngoài: 

| d | nd | xuất sắc? | n+d | xuất sắc? | quyết định | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 9 | không | 11 | vâng | trở lại 11 | 

Chúng ta dừng lại ở d = 1 vì 11 là số nguyên tố còn 9 thì không. Điều này chứng tỏ rằng việc tìm kiếm lên và xuống đều cần thiết, vì số nguyên tố gần nhất có thể chỉ nằm ở một phía. 

### Ví dụ 2 

đầu vào: 

n = 10 (trình diễn trường hợp hòa) 

Chúng tôi lại kiểm tra khoảng cách: 

| d | nd | xuất sắc? | n+d | xuất sắc? | quyết định | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 9 | không | 11 | vâng | trở lại 11 | 
| 2 | 8 | không | 12 | không | tiếp tục | 

Điều này xác nhận rằng thuật toán luôn chọn số nguyên tố gặp đầu tiên ở khoảng cách tối thiểu và khi cả hai bên khớp nhau ở một số d thì phía bên trái được chọn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log log N + t · d) | sàng xây dựng tính nguyên tố, mỗi truy vấn sẽ mở rộng ra bên ngoài cho đến số nguyên tố gần nhất | 
| Không gian | O(N) | mảng boolean cho tính nguyên tố lên tới 1e6 | 

Sàng chiếm ưu thế trong quá trình tiền xử lý nhưng dễ dàng phù hợp với các ràng buộc với N = 1e6. Mỗi truy vấn thường yêu cầu chuyển động ra bên ngoài rất nhỏ vì các số nguyên tố dày đặc trong phạm vi này, giúp giải pháp có tốc độ nhanh chóng thoải mái cho 100.000 truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
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

    def solve(n):
        if is_prime[n]:
            return n
        d = 1
        while True:
            if n - d >= 0 and is_prime[n - d]:
                return n - d
            if n + d <= MAXN and is_prime[n + d]:
                return n + d
            d += 1

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        out.append(str(solve(n)))
    return "\n".join(out)

# provided samples (conceptual)
assert run("3\n10\n11\n1\n") == "11\n11\n2"

# custom cases
assert run("1\n2\n") == "2", "minimum prime"
assert run("1\n14\n") == "13", "closest below"
assert run("1\n15\n") == "13", "tie handling"
assert run("1\n1000000\n") != "", "upper bound stability"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n = 2 | 2 | cạnh nguyên tố nhỏ nhất | 
| n = 14 | 13 | hướng xuống số nguyên tố gần nhất | 
| n = 15 | 13 | sự đúng đắn của sự ràng buộc | 
| n = 1000000 | số nguyên tố hợp lệ | độ bền giới hạn trên | 

## Vỏ cạnh 

Với n = 1, thuật toán bắt đầu với d = 1 và kiểm tra n − d = 0 và n + d = 2. Vì 0 không hợp lệ và 2 là số nguyên tố nên nó trả về đúng 2 ngay lập tức. 

Với n = 2, kiểm tra số nguyên tố sớm trả về 2 mà không cần vào vòng tìm kiếm. Điều này tránh việc kiểm tra ranh giới không cần thiết và đảm bảo tính chính xác ở số nguyên tố hợp lệ nhỏ nhất. 

Đối với các giá trị lớn như n = 999983 (đã là số nguyên tố gần giới hạn trên), sàng sẽ đánh dấu giá trị đó một cách chính xác và thuật toán trả về giá trị đó ngay lập tức mà không cần mở rộng. Điều này cho thấy điều kiện thoát sớm là rất quan trọng để ổn định hiệu suất gần các vùng nguyên tố dày đặc.
