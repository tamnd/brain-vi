---
title: "CF 105408G - Mảng huyền bí GCDland"
description: "Chúng ta được cung cấp một danh sách các số nguyên và được yêu cầu xác minh một thuộc tính cấu trúc rất cụ thể: mỗi cặp số trong danh sách phải có cùng ước số chung lớn nhất."
date: "2026-06-23T04:46:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105408
codeforces_index: "G"
codeforces_contest_name: "2024 ICPC Gran Premio de Mexico Repechaje"
rating: 0
weight: 105408
solve_time_s: 117
verified: true
draft: false
---

[CF 105408G - Mảng huyền bí GCDland](https://codeforces.com/problemset/problem/105408/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên và được yêu cầu xác minh một thuộc tính cấu trúc rất cụ thể: mỗi cặp số trong danh sách phải có cùng ước số chung lớn nhất. Nói cách khác, nếu bạn chọn bất kỳ hai vị trí nào trong mảng, hãy tính gcd của chúng, giá trị không được phụ thuộc vào cặp bạn đã chọn. 

Ý nghĩa ngay lập tức là mảng bị hạn chế rất nhiều. Nếu thậm chí một cặp tạo ra giá trị gcd khác với cặp khác thì cấu trúc sẽ bị phá vỡ và chúng ta phải từ chối nó. 

Kích thước đầu vào có thể đạt tới một trăm nghìn phần tử và mỗi giá trị có thể lớn tới mười triệu. Điều này đẩy chúng ta ra khỏi bất kỳ cách tiếp cận nào cố gắng kiểm tra rõ ràng tất cả các cặp, vì điều đó sẽ yêu cầu đại khái$10^{10}$tính toán gcd trong trường hợp xấu nhất, vượt xa giới hạn một giây cho phép. Bất kỳ giải pháp nào có thể chấp nhận được đều phải giảm vấn đề xuống gần tuyến tính hoặc gần tuyến tính trong kích thước của mảng. 

Trường hợp góc tinh tế xuất phát từ các giá trị nhỏ và các yếu tố lặp lại. Ví dụ: một mảng như$[2, 4, 8]$cư xử khác với$[2, 3, 4]$, mặc dù cả hai đều có gcds nhỏ. Một tình huống phức tạp khác là khi tất cả các số đều có chung một thừa số, nhưng vẫn khác nhau về cách yếu tố đó tương tác với các số nguyên tố còn lại, điều này có thể ảnh hưởng đến tính nhất quán của gcd theo cặp. 

Ví dụ, hãy xem xét$[6, 10, 15]$. Gcd của cả ba là$1$, nhưng gcds theo cặp khác nhau:$\gcd(6,10)=2$,$\gcd(6,15)=3$,$\gcd(10,15)=5$. Một giả định ngây thơ rằng “gcd toàn cầu bằng 1 là đủ” đã thất bại ngay lập tức. 

Một trường hợp thất bại khác là cho rằng chỉ cần kiểm tra các phần tử liền kề sau khi sắp xếp là đủ. Vì$[6, 10, 15]$, sắp xếp cho$[6,10,15]$và các gcd liền kề là$2$Và$5$, vốn đã khác nhau, nhưng ngay cả khi các gcd liền kề khớp với nhau trong một số trường hợp được chế tạo, các cặp không liền kề vẫn có thể vi phạm điều kiện. 

## Phương pháp tiếp cận 

Một cách giải thích trực tiếp gợi ý tính toán gcd cho mỗi cặp và kiểm tra sự bằng nhau. Điều này đúng nhưng cực kỳ tốn kém. Với$N$lên đến$10^5$, chúng ta sẽ tính toán đại khái$N(N-1)/2$hoạt động gcd. Mỗi gcd là logarit, vì vậy điều này vẫn sụp đổ trong giới hạn thời gian. 

Quan sát quan trọng là nếu tất cả các cặp gcd giống hệt nhau thì tồn tại một giá trị duy nhất$g$sao cho mỗi cặp phần tử có cùng một gcd$g$. Điều này buộc phải có một cấu trúc chặt chẽ: tất cả các số phải là bội số của$g$và sau khi chia từng phần tử cho$g$, bất kỳ cặp số kết quả nào cũng phải có gcd bằng$1$. Nếu hai số rút gọn bất kỳ có chung thừa số nguyên tố thì gcd của chúng sẽ vượt quá$1$, mâu thuẫn với yêu cầu 

Điều này chuyển vấn đề thành việc phát hiện xem có bất kỳ thừa số nguyên tố nào xuất hiện ở nhiều hơn một số sau khi chuẩn hóa hay không. 

Đầu tiên chúng ta tính gcd toàn cục của toàn bộ mảng. Giá trị này phải bằng gcd theo cặp chung nếu điều kiện là đúng. Sau đó, chúng tôi chia tất cả các phần tử cho gcd này. Vấn đề bây giờ hoàn toàn trở thành mảng rút gọn: chúng ta phải đảm bảo rằng không có thừa số nguyên tố nào được chia sẻ giữa hai phần tử khác nhau. 

Điều này có thể được kiểm tra một cách hiệu quả bằng cách sử dụng hệ số nguyên tố với sàng các thừa số nguyên tố nhỏ nhất. Mỗi số được phân tích thành thừa số một lần và chúng tôi theo dõi những số nguyên tố nào đã xuất hiện. Nếu một số nguyên tố xuất hiện trong nhiều phần tử thì điều kiện bị vi phạm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra gcd theo cặp Brute Force | O(N2 log A) | O(1) | Quá chậm | 
| GCD + theo dõi thừa số nguyên tố | O(N log A) | O(A) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính gcd của toàn bộ mảng và lưu nó dưới dạng$g$. Giá trị này đại diện cho ứng cử viên duy nhất có thể có cho gcd theo cặp chung, vì mọi gcd theo cặp phải chia tất cả các phần tử. 
2. Thay thế mọi phần tử$a_i$với$a_i / g$. Điều này loại bỏ yếu tố chung bắt buộc để chúng ta chỉ có thể tập trung vào cấu trúc chia sẻ bổ sung. 
3. Xây dựng bảng thừa số nguyên tố nhỏ nhất đến giá trị lớn nhất trong mảng bằng sàng. Điều này cho phép phân tích nhanh từng số. 
4. Với mỗi số rút gọn, hãy trích ra các thừa số nguyên tố riêng biệt của nó. Chúng ta chỉ quan tâm đến việc liệu một số nguyên tố có xuất hiện hay không, chứ không phải tính bội số của nó, bởi vì các lũy thừa lặp lại không thay đổi dù gcd có lớn hơn một hay không. 
5. Duy trì một tập hợp hoặc mảng boolean đánh dấu các số nguyên tố đã được sử dụng trong các số trước đó. 
6. Nếu bất kỳ thừa số nguyên tố nào xuất hiện ở nhiều hơn một số riêng biệt, ngay lập tức kết luận rằng không phải tất cả các gcd theo cặp đều bằng nhau và trả về NO. 
7. Nếu tất cả các số được xử lý không có xung đột, hãy trả về CÓ. 

Ý tưởng trung tâm là gcd theo cặp bằng nhau sẽ tạo ra sự rời rạc của các hỗ trợ chính sau khi chuẩn hóa. Bất kỳ sự trùng lặp nào về thừa số nguyên tố giữa hai phần tử khác nhau sẽ trực tiếp tạo ra một cặp có gcd vượt quá đường cơ sở chung, phá vỡ điều kiện. 

## Tại sao nó hoạt động 

Sau khi chia cho gcd toàn cầu$g$, mọi số đều trở thành số nguyên tố cùng nhau đối với cấu trúc mục tiêu dự kiến. Nếu hai số rút gọn có chung một thừa số nguyên tố$p$, thì gcd của họ ít nhất là$p$, lớn hơn$1$. Điều đó có nghĩa là gcd theo cặp ban đầu vượt quá$g$, mâu thuẫn với giả định rằng tất cả các gcd theo cặp đều bằng nhau. Ngược lại, nếu không có thừa số nguyên tố nào xuất hiện trong nhiều hơn một số rút gọn thì mỗi cặp đều có cùng một gcd$g$, vì không có cấu trúc chung bổ sung nào tồn tại ngoài yếu tố toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXV = 10_000_000

# smallest prime factor sieve
spf = list(range(MAXV + 1))
for i in range(2, int(MAXV ** 0.5) + 1):
    if spf[i] == i:
        step = i
        start = i * i
        for j in range(start, MAXV + 1, step):
            if spf[j] == j:
                spf[j] = i

def factorize(x):
    res = []
    while x > 1:
        p = spf[x]
        res.append(p)
        while x % p == 0:
            x //= p
    return res

n = int(input())
a = list(map(int, input().split()))

import math

g = 0
for v in a:
    g = math.gcd(g, v)

used = set()

for v in a:
    v //= g
    if v == 1:
        continue
    factors = factorize(v)
    for p in set(factors):
        if p in used:
            print("NO")
            sys.exit()
        used.add(p)

print("YES")
```Giai đoạn đầu tiên tính toán gcd toàn cục trên tất cả các phần tử. Điều này neo giữ cấu trúc chung tối thiểu phải tồn tại nếu điều kiện được giữ. 

Sàng chuẩn bị phân tích nhân tử nhanh, điều này rất cần thiết vì phép chia thử lặp đi lặp lại sẽ quá chậm đối với một trăm nghìn số. 

Mỗi số được giảm theo gcd chung và sau đó hỗ trợ chính của nó được trích xuất. Việc sử dụng tập hợp các số nguyên tố cho mỗi số sẽ đảm bảo các lũy thừa lặp lại không gây ra xung đột một cách sai lầm. 

Toàn cầu`used`set đảm bảo rằng mỗi số nguyên tố xuất hiện tối đa một số rút gọn, thực thi điều kiện rời rạc đặc trưng cho các mảng hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
10 2 4 12 15
```Đầu tiên chúng ta tính gcd toàn cục, bằng 1. Không có phép giảm nào làm thay đổi mảng. 

| Bước | Giá trị hiện tại | Các yếu tố chính | Số nguyên tố đã qua sử dụng | Quyết định | 
| --- | --- | --- | --- | --- | 
| 10 | 2, 5 | {2,5} | {2,5} | được | 
| 2 | 2 | {2} | xung đột với 2 | KHÔNG | 

Ở bước thứ hai, số 2 giới thiệu một số nguyên tố đã được 10 sử dụng nên chúng ta ngay lập tức bác bỏ. Điều này cho thấy cấu trúc nguyên tố chồng chéo giữa các phần tử sẽ phá vỡ điều kiện ngay cả khi gcd toàn cầu nhỏ. 

### Ví dụ 2 

đầu vào:```
3
2 4 6
```gcd toàn cầu là 2, vì vậy chúng tôi chia để có được$[1, 2, 3]$. 

| Bước | Giá trị | Giá trị giảm | Các yếu tố chính | Số nguyên tố đã qua sử dụng | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 1 | 1 | - | {} | được | 
| 4 | 2 | 2 | {2} | {2} | được | 
| 6 | 3 | 3 | {3} | {2,3} | được | 

Không có số nguyên tố nào lặp lại trên các số khác nhau, vì vậy cấu trúc hợp lệ và chúng tôi xuất ra CÓ. 

Điều này chứng tỏ rằng ngay cả khi các số gốc chia sẻ nhiều mẫu gcd, việc chuẩn hóa sẽ tiết lộ liệu cấu trúc nguyên tố cơ bản có rời rạc hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(MAXV \log \log MAXV + N \log MAXV)$| sàng xây dựng thừa số nguyên tố nhỏ nhất, mỗi số được phân tích thành thừa số một lần | 
| Không gian |$O(MAXV)$| Cấu trúc theo dõi và mảng SPF | 

Sàng chiếm ưu thế trong quá trình tiền xử lý nhưng có thể chấp nhận được đối với hạn chế$10^7$. Mỗi phần tử được xử lý theo thời gian logarit nhờ hệ số hóa nhanh, giữ cho lời giải nằm trong giới hạn cho$10^5$các phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MAXV = 10_000_000
    spf = list(range(MAXV + 1))
    for i in range(2, int(MAXV ** 0.5) + 1):
        if spf[i] == i:
            for j in range(i * i, MAXV + 1, i):
                if spf[j] == j:
                    spf[j] = i

    def factorize(x):
        res = []
        while x > 1:
            p = spf[x]
            res.append(p)
            while x % p == 0:
                x //= p
        return res

    import math

    n = int(input())
    a = list(map(int, input().split()))

    g = 0
    for v in a:
        g = math.gcd(g, v)

    used = set()

    for v in a:
        v //= g
        if v == 1:
            continue
        for p in set(factorize(v)):
            if p in used:
                return "NO"
            used.add(p)

    return "YES"

# provided samples
assert run("5\n10 2 4 12 15\n") == "NO", "sample 1"
assert run("3\n2 4 6\n") == "YES", "sample 2"
assert run("4\n2 4 6 8\n") == "NO", "sample 3"

# custom cases
assert run("2\n7 7\n") == "YES", "all equal"
assert run("3\n6 10 15\n") == "NO", "distinct primes overlap structure"
assert run("3\n1 1 1\n") == "YES", "all ones"
assert run("3\n2 3 5\n") == "YES", "pairwise coprime after gcd=1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 số giống nhau | CÓ | các phần tử giống hệt nhau hoạt động nhất quán | 
| 6 10 15 | KHÔNG | xung đột nguyên tố chồng chéo cổ điển | 
| tất cả những cái | CÓ | trường hợp cạnh chuẩn hóa gcd | 
| 2 3 5 | CÓ | cấu trúc nguyên tố hoàn toàn rời rạc | 

## Vỏ cạnh 

Đối với các mảng trong đó tất cả các phần tử giống hệt nhau, gcd toàn cục sẽ bằng chính phần tử đó và sau khi giảm, mọi giá trị sẽ trở thành 1. Vì không còn số nguyên tố nào nên không xảy ra xung đột và thuật toán trả về đúng CÓ. 

Đối với mảng như$[6, 10, 15]$, gcd toàn cầu là 1, do đó không có sự giảm nào xảy ra. Các tập hợp nguyên tố chồng lên nhau một cách gián tiếp thông qua các cặp khác nhau, nhưng trong thực tế, số nguyên tố lặp lại được phát hiện đầu tiên ngay lập tức vi phạm điều kiện duy nhất, tạo ra NO một cách chính xác. 

Đối với các mảng chứa nhiều số 1, các số này không đóng góp thừa số nguyên tố nào sau khi rút gọn. Chúng không can thiệp vào cấu trúc theo dõi, đảm bảo tính chính xác ngay cả khi phần lớn mảng là trung tính đối với cấu trúc gcd.
