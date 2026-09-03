---
title: "CF 104468D - Mảng xấu xí DBSucks"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, có một mảng các số nguyên và giá trị giới hạn $M$. Nhiệm vụ là đếm xem có bao nhiêu số nguyên $X$ trong khoảng từ 1 đến $M$ không có thừa số nguyên tố chung với bất kỳ phần tử nào của mảng."
date: "2026-06-30T12:56:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104468
codeforces_index: "D"
codeforces_contest_name: "The 2023 Damascus University Collegiate Programming Contest"
rating: 0
weight: 104468
solve_time_s: 102
verified: false
draft: false
---

[CF 104468D - Mảng xấu xí DBSucks](https://codeforces.com/problemset/problem/104468/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, có một mảng số nguyên và giá trị giới hạn$M$. Nhiệm vụ là đếm có bao nhiêu số nguyên$X$trong khoảng từ 1 đến$M$không có thừa số nguyên tố chung với bất kỳ phần tử nào của mảng. Nói cách khác, để có giá trị hợp lệ$X$, ước chung lớn nhất của$X$và mọi phần tử mảng phải là 1, tương đương với việc nói rằng$X$không chia hết cho bất kỳ số nguyên tố nào xuất hiện trong bất kỳ$A_i$. 

Quan sát quan trọng ẩn trong cách diễn đạt là chúng ta không xử lý tính nguyên thủy theo cặp giữa các số trong mảng mà thay vào đó là việc cấm hoàn toàn một tập hợp các thừa số nguyên tố. Khi một số nguyên tố chia ít nhất một phần tử mảng, số nguyên tố đó sẽ tự động loại bỏ mọi$X$chia hết cho nó. 

Các ràng buộc chặt chẽ nhưng có cấu trúc. Cả hai$N$Và$M$đi lên$10^5$và tổng số tiền trên tất cả các trường hợp thử nghiệm cũng bị giới hạn bởi$10^5$. Điều này gợi ý mạnh mẽ một$O(N \log A + M)$giải pháp kiểu cho mỗi trường hợp thử nghiệm hoặc cách tiếp cận dựa trên sàng toàn cầu được sử dụng lại trong các trường hợp. Bất kỳ cách tiếp cận nào liệt kê tất cả các cặp$(X, A_i)$hoặc kiểm tra gcd một cách rõ ràng cho mọi$X$sẽ quá chậm vì nó dẫn đến khoảng$10^{10}$hoạt động trong trường hợp xấu nhất. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các phần tử mảng có chung một thừa số nguyên tố nhỏ. Ví dụ, nếu tất cả$A_i$là số chẵn thì mọi giá trị đều hợp lệ$X$hẳn là kỳ quặc. Một cách tiếp cận đơn giản để kiểm tra gcd một cách độc lập cho từng$X$vẫn có thể vượt qua các trường hợp nhỏ nhưng sẽ TLE. 

Một trường hợp cạnh khác là khi mảng chứa 1. Vì gcd(1, X) luôn bằng 1 nên 1 không đóng góp hạn chế nào, nhưng việc triển khai bất cẩn trích xuất các thừa số nguyên tố mà không lọc trùng lặp có thể lãng phí thời gian hoặc xử lý sai logic tần số. 

Cuối cùng, những trường hợp$M$lớn nhưng mảng chứa nhiều số lặp lại rất quan trọng đối với hiệu suất, vì việc trích xuất nhân tố lặp lại không được tính toán lại một cách không cần thiết. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ lặp đi lặp lại trên mọi$X$từ 1 đến$M$và kiểm tra xem$\gcd(X, A_i) = 1$cho tất cả các phần tử trong mảng. Điều này đòi hỏi tính toán gcd$N$lần mỗi$X$, dẫn đến$O(NM \log A)$. Với$N, M \approx 10^5$, điều này trở nên đại khái$10^{10}$tính toán gcd, vượt xa mọi giới hạn khả thi. 

Cấu trúc của bài toán gợi ý sự chuyển đổi góc nhìn từ số sang thừa số nguyên tố. Thay vì kiểm tra xem$X$là nguyên tố cùng nhau với mỗi phần tử mảng, chúng ta có thể xác định tất cả các số nguyên tố xuất hiện ở bất kỳ đâu trong mảng. Bất kỳ hợp lệ$X$phải tránh bị chia hết cho bất kỳ số nguyên tố nào trong số này. Điều này chuyển đổi vấn đề thành đếm số trong$[1, M]$không chia hết cho một tập hợp số nguyên tố cho trước. 

Khi đã biết các số nguyên tố bị cấm, chúng ta có thể đánh dấu bội số của chúng trong một dãy tần số lên đến$M$hoặc sử dụng hiệu quả hơn loại trừ bao gồm hoặc quy trình đánh dấu giống như sàng. Vì mỗi số lên đến$M$chỉ được chạm vào một số lần nhỏ (một lần cho mỗi thừa số nguyên tố riêng biệt), nghiệm sẽ trở thành tuyến tính hoặc gần tuyến tính. 

Cải tiến quan trọng là nhận ra rằng mảng chỉ quan trọng thông qua tập thừa số nguyên tố của nó chứ không phải thông qua các giá trị thực hoặc bội số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N \cdot M \cdot \log A)$|$O(1)$| Quá chậm | 
| Sàng chính + đánh dấu |$O(M \log \log M + \sum \text{factorization})$|$O(M)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết từng trường hợp thử nghiệm một cách độc lập bằng cách sử dụng tính năng theo dõi thừa số nguyên tố và mảng đánh dấu trên$[1, M]$. 

## Hướng dẫn thuật toán 

1. Phân tích từng phần tử$A_i$và thu thập tất cả các thừa số nguyên tố phân biệt trong một tập hợp. Điều này đảm bảo chúng ta chỉ giữ các số nguyên tố thực sự ràng buộc các giá trị hợp lệ của$X$. Các số nguyên tố lặp đi lặp lại trên các phần tử khác nhau không có ý nghĩa gì ngoài sự tồn tại của chúng. 
2. Khởi tạo một mảng`bad`kích thước$M+1$với tất cả các giá trị sai. Mảng này sẽ theo dõi xem một số có bị loại hay không vì nó chia hết cho ít nhất một số nguyên tố bị cấm. 
3. Đối với mỗi số nguyên tố$p$trong tập hợp đã thu thập, lặp lại bội số của$p$từ$p$ĐẾN$M$, đánh dấu mỗi bội số là xấu. Bước này trực tiếp mã hóa ràng buộc mà bất kỳ giá trị hợp lệ nào$X$không thể bao gồm bất kỳ thừa số nguyên tố bị cấm nào. 
4. Đếm xem có bao nhiêu chỉ số$X$TRONG$[1, M]$vẫn không được đánh dấu. Đây chính xác là những số nguyên không chia sẻ thừa số nguyên tố nào với mảng. 
5. Xuất số đếm này. 

Ý tưởng tính toán chính là biến các ràng buộc nhân (đồng nguyên tố) thành bao phủ cộng (đánh dấu bội số), điều này làm cho vấn đề trở nên dễ giải quyết. 

### Tại sao nó hoạt động 

Mỗi số nguyên$X$được xác định đầy đủ, về mặt chia hết, bởi các thừa số nguyên tố của nó. Nếu như$X$chia hết cho bất kỳ số nguyên tố nào xuất hiện trong bất kỳ$A_i$, thì tồn tại một số$A_i$chia sẻ thừa số nguyên tố đó, ngụ ý$\gcd(X, A_i) \neq 1$. Ngược lại, nếu$X$tránh tất cả các số nguyên tố như vậy, nó không chia sẻ thừa số nguyên tố nào với bất kỳ phần tử mảng nào, vì vậy gcd của nó với mọi$A_i$là 1. Quá trình đánh dấu nắm bắt chính xác tập hợp số nguyên tố bị cấm này và loại trừ tất cả các bội số bị ảnh hưởng, chỉ để lại các số nguyên hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXA = 100000

# smallest prime factor sieve
spf = list(range(MAXA + 1))
for i in range(2, int(MAXA ** 0.5) + 1):
    if spf[i] == i:
        for j in range(i * i, MAXA + 1, i):
            if spf[j] == j:
                spf[j] = i

def factorize(x):
    primes = set()
    while x > 1:
        p = spf[x]
        primes.add(p)
        while x % p == 0:
            x //= p
    return primes

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    arr = list(map(int, input().split()))

    forbidden = set()
    for v in arr:
        if v > 1:
            forbidden |= factorize(v)

    bad = [0] * (m + 1)

    for p in forbidden:
        for x in range(p, m + 1, p):
            bad[x] = 1

    ans = 0
    for i in range(1, m + 1):
        if not bad[i]:
            ans += 1

    print(ans)
```Sàng ở trên cùng tính toán trước các thừa số nguyên tố nhỏ nhất sao cho việc phân tích từng hệ số$A_i$là nhanh chóng. Điều này là cần thiết vì việc phân chia thử nghiệm ngây thơ lặp đi lặp lại vẫn sẽ quá chậm trong trường hợp đầu vào xấu nhất. 

các`forbidden`set đảm bảo các bản sao không làm tăng tác dụng khi đánh dấu bội số. Mỗi số nguyên tố được xử lý một lần và bội số của nó được đánh dấu theo vòng lặp giống như sàng. 

Vòng đếm cuối cùng là quét trực tiếp$[1, M]$, đó là tối ưu với các ràng buộc. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ trong đó mảng là`[2, 3]`Và$M = 6$. 

| Bước | Số nguyên tố bị cấm | Đánh dấu hành động | Mảng xấu (1..6) | 
| --- | --- | --- | --- | 
| Bắt đầu | ∅ | không | 000000 | 
| Sau 2 | {2} | đánh dấu 2,4,6 | 010101 | 
| Sau 3 | {2,3} | điểm 3,6 | 011101 | 

Bây giờ chúng tôi đếm các giá trị không được đánh dấu: 1 và 5. Đầu ra là 2. 

Dấu vết này cho thấy cách xử lý các bội số chồng chéo một cách tự nhiên. Số 6 được đánh dấu hai lần nhưng vẫn được đánh dấu đơn giản, xác nhận rằng việc lặp lại không ảnh hưởng đến tính chính xác. 

Bây giờ hãy xem xét`[6]`với$M = 10$. Các số nguyên tố bị cấm là`{2, 3}`. 

| Bước | Số nguyên tố bị cấm | Đánh dấu hành động | Mảng xấu (1..10) | 
| --- | --- | --- | --- | 
| Sau khi nhân tố hóa | {2,3} | đánh dấu bội số của 2 và 3 | 0101011010 | 

Các số hợp lệ là 1, 5, 7. Đầu ra là 3. Điều này cho thấy các phần tử mảng tổng hợp phân tách rõ ràng thành các ràng buộc nguyên tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log A + M \log \log M)$| phân tích nhân tử thông qua SPF cộng với việc đánh dấu bội số của các số nguyên tố riêng biệt | 
| Không gian |$O(M + MAXA)$| sàng lưu trữ và đánh dấu mảng | 

Độ phức tạp phù hợp thoải mái với các ràng buộc vì tổng của$N$Và$M$trên nhiều trường hợp thử nghiệm là nhiều nhất$10^5$, có nghĩa là công việc đánh dấu tổng thể là tuyến tính khi được khấu hao. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    MAXA = 100000
    spf = list(range(MAXA + 1))
    for i in range(2, int(MAXA ** 0.5) + 1):
        if spf[i] == i:
            for j in range(i * i, MAXA + 1, i):
                if spf[j] == j:
                    spf[j] = i

    def factorize(x):
        primes = set()
        while x > 1:
            p = spf[x]
            primes.add(p)
            while x % p == 0:
                x //= p
        return primes

    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        arr = list(map(int, input().split()))

        forbidden = set()
        for v in arr:
            if v > 1:
                forbidden |= factorize(v)

        bad = [0] * (m + 1)
        for p in forbidden:
            for x in range(p, m + 1, p):
                bad[x] = 1

        ans = sum(1 for i in range(1, m + 1) if not bad[i])
        out.append(str(ans))

    return "\n".join(out)

# provided sample (interpreted)
assert run("1\n3 5\n1 2 3\n") == "2"

# all ones: no restriction
assert run("1\n3 10\n1 1 1\n") == "10"

# single prime restriction
assert run("1\n1 10\n2\n") == "5"

# multiple primes
assert run("1\n2 10\n6 15\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả những cái | 10 | không có trường hợp số nguyên tố bị cấm | 
| số nguyên tố đơn | 5 | lọc bội số chính xác | 
| chồng chéo tổng hợp | 3 | xử lý liên minh thừa số nguyên tố | 

## Vỏ cạnh 

Khi mọi phần tử mảng bằng 1, tập hợp bị cấm trống. Vòng đánh dấu không bao giờ chạy nên tất cả các số từ 1 đến$M$vẫn còn hiệu lực. Ví dụ, đầu vào`N=3, A=[1,1,1], M=5`tạo ra tất cả các số không trong`bad`mảng và kết quả đầu ra 5. 

Khi tất cả các phần tử có chung một thừa số nguyên tố duy nhất, chẳng hạn như tất cả đều là số chẵn, thì tập bị cấm sẽ trở thành`{2}`. Thuật toán đánh dấu tất cả các số chẵn, để lại chính xác tỷ lệ cược. Vì`M=6`, mảng được đánh dấu trở thành`[1,0,1,0,1,0]`, và đầu ra là 3. 

Khi các phần tử là những hợp số lớn như 6, 10, 15, sự kết hợp các thừa số nguyên tố của chúng sẽ tạo ra các ràng buộc chồng chéo. Bước đánh dấu sẽ hợp nhất các phần chồng chéo một cách tự nhiên mà không cần tính hai lần. Ví dụ: 30 được đánh dấu nhiều lần nhưng vẫn là một loại trừ duy nhất, xác nhận tính ổn định trong các yếu tố lặp lại.
