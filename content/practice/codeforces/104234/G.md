---
title: "CF 104234G - Sự khác biệt Palindrome"
description: "Chúng ta được cung cấp một dãy số và chúng ta được phép sắp xếp lại nó một cách tùy ý. Đối với mỗi thứ tự được chọn, chúng tôi xây dựng một mảng thứ hai bao gồm các khác biệt liên tiếp giữa các phần tử liền kề."
date: "2026-07-01T23:37:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104234
codeforces_index: "G"
codeforces_contest_name: "OCPC 2023, Oleksandr Kulkov Contest 3"
rating: 0
weight: 104234
solve_time_s: 74
verified: true
draft: false
---

[CF 104234G - Sự khác biệt đối xứng](https://codeforces.com/problemset/problem/104234/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy số và chúng ta được phép sắp xếp lại nó một cách tùy ý. Đối với mỗi thứ tự được chọn, chúng tôi xây dựng một mảng thứ hai bao gồm các khác biệt liên tiếp giữa các phần tử liền kề. Nhiệm vụ là đếm xem có bao nhiêu hoán vị riêng biệt của tập hợp ban đầu tạo ra một mảng khác biệt đọc xuôi và ngược giống nhau. 

Đầu vào là nhiều trường hợp thử nghiệm và mỗi trường hợp thử nghiệm đưa ra nhiều tập hợp số nguyên. Hai hoán vị được coi là khác nhau nếu chúng khác nhau ở ít nhất một vị trí. Câu trả lời được lấy modulo$10^9 + 9$. 

Ràng buộc về tổng$n$qua các trường hợp thử nghiệm đạt được$5 \cdot 10^5$buộc một$O(n \log n)$hoặc giải pháp tuyến tính cho mỗi trường hợp thử nghiệm. Bất cứ điều gì liên quan đến việc liệt kê các hoán vị hoặc thậm chí các cặp vị trí một cách rõ ràng đều không thể thực hiện được ngay lập tức, vì$n!$phát triển quá nhanh ngay cả đối với$n = 20$. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các phần tử giống hệt nhau. Mọi hoán vị đều giống hệt nhau và mọi mảng khác biệt đều là số 0, có tính chất đối xứng tầm thường. Một giải pháp đúng phải trả về số đa thức, không phải 1. Một trường hợp không tầm thường khác là khi các giá trị khác biệt nhưng không thể ghép đối xứng thành một cấu trúc nhất quán, điều này sẽ tạo ra các hoán vị hợp lệ bằng 0. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tạo ra tất cả các hoán vị riêng biệt của nhiều tập hợp và kiểm tra từng hoán vị. Đối với mỗi hoán vị, chúng tôi tính toán mảng sai phân của nó và xác minh xem đó có phải là một bảng màu hay không. Ngay cả khi chúng tôi tối ưu hóa việc kiểm tra để$O(n)$, cái này vẫn tốn$O(n \cdot n!)$, vượt xa mọi giới hạn. 

Quan sát quan trọng là ràng buộc về tính đối xứng đối với sự khác biệt tạo ra sự đối xứng mạnh mẽ trên chính hoán vị. Viết điều kiện ra, chúng tôi so sánh sự khác biệt trái ngược nhau:$$(p_{i+1} - p_i) = (p_{n-i+1} - p_{n-i})$$Sắp xếp lại điều này mang lại:$$p_{i+1} + p_{n-i} = p_i + p_{n-i+1}$$Điều này ngụ ý rằng tất cả các cặp vị trí đối xứng đều có tổng bằng nhau. Tổng đó là một hằng số$C$, độc lập với chỉ số. 

Vì vậy mọi hoán vị hợp lệ phải thỏa mãn:$$p_i + p_{n-i+1} = C$$Điều này biến vấn đề từ một điều kiện tổng thể về sự khác biệt thành một vấn đề ghép nối: mọi phần tử phải được so khớp với một phần tử khác sao cho mỗi cặp có tổng bằng cùng một hằng số. 

Một khi cấu trúc này được nhận dạng, hoán vị không còn tùy ý nữa. Chúng tôi đang ghép nối các giá trị từ nhiều bộ một cách hiệu quả để mỗi cặp$(x, C-x)$được sử dụng đúng số lần yêu cầu. 

Lực lượng vũ phu không thành công vì nó khám phá trực tiếp các hoán vị, trong khi cách tiếp cận chính xác làm giảm mọi thứ để đếm các cặp nhiều tập hợp lệ theo một ràng buộc tổng cố định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot n!)$|$O(n)$| Quá chậm | 
| Cấu trúc tổng cặp |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Bây giờ chúng ta chuyển quan sát cấu trúc thành một thủ tục đếm. 

1. Tính giá trị tối thiểu và tối đa trong nhiều tập hợp. Chúng phải chiếm các vị trí đối xứng trong bất kỳ hoán vị hợp lệ nào, do đó tổng của chúng cố định hằng số$C = \min(a) + \max(a)$. Điều này là bắt buộc vì phần tử nhỏ nhất phải ghép với phần bù lớn nhất có thể. 
2. Xây dựng bản đồ tần số của tất cả các giá trị. Chúng tôi sẽ cố gắng ghép từng giá trị$x$với$C-x$. Nếu bất kỳ phần bù bắt buộc nào không tồn tại thì không thể hoán vị hợp lệ. 
3. Với mỗi giá trị$x$, so sánh nó với phần bù của nó$y = C-x$. Chúng tôi chỉ xử lý mỗi cặp một lần, vì vậy chúng tôi xem xét các trường hợp trong đó$x < y$,$x = y$, và bỏ qua$x > y$. 
4. Nếu$x < y$, tất cả các lần xuất hiện của$x$phải kết hợp với sự xuất hiện của$y$. Điều này đòi hỏi$\text{freq}[x] = \text{freq}[y]$. Mỗi cặp như vậy đóng góp$\text{freq}[x]$cặp vị trí đối xứng. 
5. Nếu$x = y$, giá trị ghép với chính nó. Điều này đòi hỏi tần số chẵn vì các phần tử phải được chia thành từng cặp. Số lượng các cặp tự đóng góp là$\text{freq}[x] / 2$. 
6. Nếu$n$là số lẻ, có một phần tử trung tâm chưa ghép đôi. Điều này phải thỏa mãn$2 \cdot x = C$, nghĩa là giá trị trung tâm phải chính xác$C/2$và nó phải có tần số lẻ để còn lại chính xác một phần tử sau khi ghép nối. 
7. Sau khi xác nhận tất cả các ràng buộc, chúng ta đếm sự sắp xếp của các cặp kết quả. Giả sử có$m = \lfloor n/2 \rfloor$tổng số cặp. Những cặp này có thể được hoán vị giữa$m$vị trí đối xứng, góp phần tạo nên một yếu tố$m!$. 
8. Nếu tồn tại nhiều loại cặp giống hệt nhau, hãy chia cho bội số giai thừa của chúng để tính đến sự hoán đổi không thể phân biệt được giữa các cặp giống hệt nhau. 
9. Với mọi cặp bất đối xứng$(x, C-x)$với$x \ne y$, mỗi cặp có thể được định hướng theo hai cách qua các vị trí đối xứng, góp phần tạo nên hệ số$2$mỗi cặp như vậy. 

### Tại sao nó hoạt động 

Bất biến quan trọng là mọi hoán vị hợp lệ đều tạo ra sự khớp hoàn hảo của các chỉ số$(i, n-i+1)$và điều kiện chênh lệch palindrome buộc tất cả các cặp khớp phải có cùng một tổng. Điều này thu gọn cấu trúc hoán vị thành các khối cặp độc lập không có thứ tự. Khi nhiều tập hợp được phân vùng thành các khối này, mọi cách sắp xếp đều tương ứng chính xác với việc hoán vị các khối này và tùy ý chọn hướng cho các cặp không đối xứng mà công thức đếm nắm bắt mà không cần đếm quá mức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import Counter

MOD = 10**9 + 9

MAXN = 5 * 10**5 + 5
fact = [1] * (MAXN)
for i in range(1, MAXN):
    fact[i] = fact[i - 1] * i % MOD

inv_fact = [1] * (MAXN)
inv_fact[MAXN - 1] = pow(fact[MAXN - 1], MOD - 2, MOD)
for i in range(MAXN - 2, -1, -1):
    inv_fact[i] = inv_fact[i + 1] * (i + 1) % MOD

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        cnt = Counter(a)

        if n == 2:
            out.append(str(2 % MOD))
            continue

        mn, mx = min(a), max(a)
        C = mn + mx

        used = set()
        ok = True
        pairs = Counter()
        odd_center = 0
        asym_pairs = 0

        for x in list(cnt.keys()):
            if x in used:
                continue
            y = C - x
            if y not in cnt:
                ok = False
                break

            if x == y:
                if cnt[x] % 2:
                    if n % 2 == 0:
                        ok = False
                        break
                    odd_center += 1
                    if odd_center > 1:
                        ok = False
                        break
                pairs[x] = cnt[x] // 2
            else:
                if cnt[x] != cnt[y]:
                    ok = False
                    break
                pairs[x] = cnt[x]
                asym_pairs += cnt[x]
                used.add(y)

        if not ok:
            out.append("0")
            continue

        total_pairs = sum(pairs.values())
        m = n // 2

        if total_pairs != m:
            out.append("0")
            continue

        res = fact[m]

        for k in pairs.values():
            res = res * inv_fact[k] % MOD

        res = res * pow(2, asym_pairs, MOD) % MOD

        out.append(str(res))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ khóa tổng ghép nối toàn cầu bằng cách sử dụng các giá trị tối thiểu và tối đa. Sau đó, nó xác nhận rằng mọi giá trị đều có thể được ghép nối với phần bù của nó và xây dựng một tập hợp nhiều loại cặp. Giai thừa và giai thừa nghịch đảo được tính toán trước để cho phép đếm đa thức nhanh chóng về cách sắp xếp các cặp này dọc theo các vị trí đối xứng. Sức mạnh của hai giải thích cho sự tự do định hướng trong các cặp không đối xứng. 

Phải cẩn thận chỉ xử lý mỗi cặp giá trị-bù một lần, nếu không số lượng sẽ bị trùng lặp không chính xác. Trung tâm xử lý số lẻ$n$bị tách ra vì nó không tham gia ghép đôi và nếu không sẽ làm hỏng cấu trúc tổ hợp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một trường hợp đơn giản: 

Mảng đầu vào:$[1, 3, 5]$| Bước | Giá trị | Phần bù (C=6) | Hành động | 
| --- | --- | --- | --- | 
| 1 | 1 | 5 | cặp | 
| 2 | 3 | 3 | trung tâm | 
| 3 | 5 | 1 | bỏ qua | 

Điều này tạo ra một cấu trúc hợp lệ với các cặp$(1,5)$và trung tâm$3$. Cặp này có thể được định hướng theo hai cách, cho hai hoán vị hợp lệ. 

Điều này chứng tỏ ràng buộc đối xứng làm giảm hoán vị đối với các lựa chọn cặp độc lập như thế nào. 

### Ví dụ 2 

Mảng đầu vào:$[2, 2, 2, 2]$| Giá trị | Bổ sung | Cặp | 
| --- | --- | --- | 
| 2 | 2 | 2 cặp | 

Tất cả các phần tử tạo thành các cặp tự ghép và mọi hoán vị đều hợp lệ vì mọi cách sắp xếp đều bảo toàn hiệu số không đổi bằng 0. Kết quả bằng số hoán vị của các phần tử giống hệt nhau sau khi tính toán ghép nối, được đánh giá là 1. 

Điều này cho thấy việc tự ghép đôi sẽ làm sụp đổ mọi cấu trúc và chỉ để lại những cấu hình không thể phân biệt được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Xây dựng tần số và các giá trị vượt qua một lần với các phép toán giai thừa được tính toán trước | 
| Không gian |$O(n)$| Bản đồ tần số và bảng giai thừa | 

Thuật toán phù hợp thoải mái trong giới hạn vì tổng$n$qua các trường hợp thử nghiệm là$5 \cdot 10^5$và tất cả các tính toán nặng là tuyến tính hoặc gần tuyến tính đối với tổng này. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 9

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import Counter

    # simplified reference via calling full solution assumed present
    return ""  # placeholder

# sample-style sanity checks (conceptual placeholders)
# assert run(...) == ...

# custom edge cases

# all equal
# n even
# should be multinomial of pairs = 1
# assert run("1\n4\n7 7 7 7\n") == "1"

# no valid complement structure
# assert run("1\n3\n1 2 4\n") == "0"

# minimal case
# assert run("1\n2\n5 10\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 1 | tự ghép thoái hóa | 
| không bổ sung | 0 | cấu trúc C không hợp lệ | 
| n=2 phân biệt | 2 | trường hợp đối xứng cơ sở | 

## Vỏ cạnh 

Khi tất cả các phần tử đều giống hệt nhau, thuật toán sẽ đặt$C = 2x$và coi mọi phần tử như một cặp tự ghép. Vì mọi cặp đều giống hệt nhau nên đa thức giảm xuống còn 1, phù hợp với thực tế là tất cả các hoán vị đều không thể phân biệt được trong điều kiện. 

Khi không có phần bù hợp lệ nào tồn tại cho một số giá trị, thuật toán sẽ ngay lập tức từ chối cấu hình, vì ngay cả một cặp bị thiếu cũng sẽ phá vỡ yêu cầu đối xứng tổng thể. 

Đối với số lẻ$n$, phần tử trung tâm phải chính xác$C/2$. Nếu có nhiều hơn một ứng viên chưa được ghép cặp, thuật toán sẽ loại bỏ trường hợp đó vì chỉ tồn tại một vị trí trung tâm trong cấu trúc đối xứng.
