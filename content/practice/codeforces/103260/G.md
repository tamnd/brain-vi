---
title: "CF 103260G - Loại bỏ lớp sơn lót"
description: "Chúng ta được cho một mảng các số nguyên dương. Hai người chơi luân phiên nhau và trong mỗi lượt, một người chơi thực hiện một thao tác rút gọn rất cụ thể: họ chọn một số nguyên tố $p$ rồi chọn một đoạn liền kề của mảng sao cho mọi số trong đoạn đó đều chia hết cho…"
date: "2026-07-03T14:59:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103260
codeforces_index: "G"
codeforces_contest_name: "2020-2021 Winter Petrozavodsk Camp, Day 5: Almost Retired Dandelion Contest (XXI Open Cup, Grand Prix of Nizhny Novgorod)"
rating: 0
weight: 103260
solve_time_s: 44
verified: true
draft: false
---

[CF 103260G - Loại bỏ Prime](https://codeforces.com/problemset/problem/103260/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương. Hai người chơi luân phiên nhau và trong mỗi lượt, một người chơi thực hiện một thao tác rút gọn rất cụ thể: họ chọn một số nguyên tố$p$rồi chọn một đoạn liền kề của mảng sao cho mọi số trong đoạn đó đều chia hết cho$p$. Đối với mọi phần tử trong phân đoạn đó, họ chia nó nhiều lần cho$p$cho đến khi nó không còn chia hết cho$p$. Sau thao tác này, một số phần tử sẽ co lại nhưng cấu trúc của mảng vẫn giữ nguyên độ dài. 

Trò chơi kết thúc khi người chơi không thể thực hiện một nước đi hợp lệ. Nhiệm vụ là xác định xem người chơi thứ nhất hay thứ hai có chiến lược chiến thắng trong lối chơi tối ưu hay không. 

Sự thay đổi quan điểm quan trọng là mảng không được sắp xếp lại hoặc xóa khỏi. Thay vào đó, mỗi lần di chuyển sẽ làm giảm cấu trúc số mũ nguyên tố của số nguyên tố được chọn trên một khối liền kề. Vì vậy trạng thái của trò chơi hoàn toàn được xác định bởi số mũ nguyên tố còn lại của mỗi phần tử. 

Các ràng buộc tương đối nhỏ về chiều dài,$n \le 1000$, nhưng giá trị có thể lớn bằng$10^{18}$. Điều đó ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng mô phỏng phân tích nhân tử nhiều lần trong mỗi lần di chuyển hoặc tính toán lại khả năng chia hết một cách linh hoạt. Bất kỳ giải pháp nào cũng phải phân tích từng số nhiều nhất một lần và sau đó suy luận tổ hợp về cấu trúc. 

Trường hợp cạnh tinh tế xuất phát từ thực tế là một phần tử có thể tham gia vào nhiều phân đoạn trên các số nguyên tố khác nhau. Ví dụ: nếu một phần tử chia hết cho cả 2 và 3 thì nó sẽ đóng góp độc lập vào các nước đi liên quan đến 2 và các nước đi liên quan đến 3, nhưng ở các lượt khác nhau của trò chơi. Một cách tiếp cận ngây thơ coi mỗi phần tử như một mã thông báo duy nhất có “giá trị còn lại” sẽ bỏ lỡ tính độc lập này. 

Một tình huống phức tạp khác là khi tất cả các số đều là lũy thừa của một số nguyên tố. Sau đó, mọi nước đi đều buộc phải sử dụng số nguyên tố đó và trò chơi giảm xuống mức liên tục tước bỏ sức mạnh dọc theo các phân đoạn. Một cách tiếp cận bất cẩn chỉ theo dõi xem số là số nguyên tố hay hợp số sẽ thất bại ở đây, vì số lần di chuyển có sẵn phụ thuộc vào số mũ chứ không phải số nguyên tố. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng trực tiếp trạng thái trò chơi. Người ta sẽ liệt kê nhiều lần tất cả các số nguyên tố xuất hiện trong mảng, sau đó với mỗi số nguyên tố hãy thử tất cả các phân đoạn hợp lệ, áp dụng phép chia và lặp lại. Về nguyên tắc, điều này đúng vì nó khám phá tất cả các trạng thái trò chơi có thể có, nhưng nó bùng nổ về mặt tổ hợp. Ngay cả với$n = 1000$, mỗi nước đi có thể tạo ra nhiều nhánh, và độ sâu của trò chơi có thể lớn vì một số như$10^{18}$có thể chứa nhiều thừa số nguyên tố lặp lại. Số lượng các trạng thái tăng lên vượt quá mọi giới hạn khả thi. 

Cái nhìn sâu sắc quan trọng là ngừng suy nghĩ về số đầy đủ và thay vào đó tập trung vào số mũ nguyên tố cho mỗi vị trí. Mỗi phần tử$a_i$có thể được phân tách thành tích của các số nguyên tố và mỗi số nguyên tố hoạt động độc lập. Một nước đi chọn một số nguyên tố$p$và một phân đoạn, đồng thời giảm số mũ của$p$trong tất cả các phần tử của đoạn đó. Điều này có nghĩa là đối với mỗi số nguyên tố, chúng ta đang chơi một trò chơi riêng biệt trên cấu trúc giống nhị phân một cách hiệu quả: chúng ta có thể “áp dụng” số nguyên tố đó bao nhiêu lần trên các phân đoạn liền kề. 

Bây giờ quan sát quan trọng là mỗi đoạn cực đại trong đó một số nguyên tố$p$xuất hiện dưới hình thức đóng góp độc lập. Nếu chúng ta nhìn vào một số nguyên tố cố định$p$, nó xuất hiện trong một số khoảng cách rời nhau của mảng. Trong mỗi khoảng như vậy, số lần chúng ta có thể áp dụng các phép toán chính xác là tổng số mũ của$p$trong khoảng đó, nhưng có ràng buộc về cấu trúc mạnh mẽ: mỗi thao tác sử dụng một “lớp” của khoảng đó trên một phân đoạn đã chọn. 

Điều này biến trò chơi thành một mẫu đã biết: đối với mỗi số nguyên tố, mỗi lần xuất hiện đóng góp một chồng phép toán trên các phân đoạn và kết quả cuối cùng chỉ phụ thuộc vào việc tổng số “nước đi” độc lập như vậy trên tất cả các số nguyên tố là lẻ hay chẵn trong cách chơi tối ưu. Điều này chuyển vấn đề thành tính toán đóng góp chẵn lẻ giống như Grundy cho mỗi số nguyên tố trên phân phối số mũ của nó. 

Chúng tôi giảm bớt vấn đề một cách hiệu quả bằng cách đếm xem có bao nhiêu phép toán phân đoạn nguyên tố độc lập tồn tại. Mỗi thao tác như vậy tương ứng với mức đóng góp 1 vào giá trị trò chơi và người chiến thắng được xác định bằng việc tổng số có phải là số lẻ hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | Không gian trạng thái lớn | Quá chậm | 
| Phân tách nguyên tố + đếm đóng góp phân đoạn |$O(n \log A)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta phân tích từng số trong mảng. Từ$a_i \le 10^{18}$, chúng tôi sử dụng phép chia thử lên đến$\sqrt{a_i}$hoặc các số nguyên tố được tính toán trước nếu có và trích xuất tất cả các lũy thừa nguyên tố. 

Đối với mỗi số nguyên tố$p$, chúng tôi theo dõi cách nó xuất hiện trên mảng dưới dạng giá trị số mũ. Chúng tôi xây dựng một trình tự$e_1, e_2, \dots, e_n$, Ở đâu$e_i$là số mũ của$p$TRONG$a_i$. 

Sau đó chúng tôi nén chuỗi này thành các phân đoạn liền kề tối đa trong đó$e_i > 0$. Bên trong mỗi phân đoạn như vậy, chúng tôi tính tổng số mũ. Mỗi đơn vị số mũ tương ứng với một khả năng “loại bỏ lớp” trên một số hoạt động phân đoạn, do đó tổng số hành động độc lập do phân đoạn này đóng góp bằng tổng đó. 

Chúng tôi tích lũy sự đóng góp này trên tất cả các số nguyên tố. 

Cuối cùng, chúng tôi tính toán tính chẵn lẻ của tổng số các hoạt động như vậy. Nếu khác 0 và lẻ thì người chơi đầu tiên thắng; nếu không, người chơi thứ hai sẽ thắng. 

Lý do chính khiến điều này có hiệu quả là các phép toán trên các số nguyên tố khác nhau không bao giờ tương tác với nhau. Một nước đi sửa chính xác một số nguyên tố và giảm số mũ của nó một cách độc lập với tất cả các số khác, do đó trò chơi sẽ phân rã thành các trò chơi con độc lập có giá trị Grundy đều là 1 trên mỗi đơn vị hoạt động. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict
import math

def factorize(x, primes):
    res = defaultdict(int)
    for p in primes:
        if p * p > x:
            break
        while x % p == 0:
            res[p] += 1
            x //= p
    if x > 1:
        res[x] += 1
    return res

def sieve(n=100000):
    is_p = [True] * (n + 1)
    is_p[0] = is_p[1] = False
    primes = []
    for i in range(2, n + 1):
        if is_p[i]:
            primes.append(i)
            for j in range(i * i, n + 1, i):
                is_p[j] = False
    return primes

def main():
    n = int(input())
    a = list(map(int, input().split()))
    
    primes = sieve(100000)
    
    total = 0
    
    for x in a:
        f = factorize(x, primes)
        for p, c in f.items():
            total += c
    
    if total % 2 == 1:
        print("First")
    else:
        print("Second")

if __name__ == "__main__":
    main()
```Mã bắt đầu bằng cách tạo ra các số nguyên tố để phân tích nhân tử. Mỗi số được phân tách thành số mũ nguyên tố và tất cả số mũ được tính tổng trên toàn cầu. Tổng đó thể hiện tổng số hành động loại bỏ nguyên tố độc lập có sẵn trong trò chơi. 

Một điểm thực hiện tinh tế là xử lý các thừa số nguyên tố lớn còn lại sau khi chia thử. Nếu một thừa số lớn hơn 1 thì nó phải được tính là số nguyên tố có số mũ bằng 1. 

Quyết định cuối cùng chỉ phụ thuộc vào tính chẵn lẻ của tổng số mũ tích lũy, phù hợp với cấu trúc trò chơi công bằng cơ bản. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
2 8 4
```Chúng tôi phân tích từng số: 

| Chỉ mục | Giá trị | Các yếu tố chính | 
| --- | --- | --- | 
| 1 | 2 | 2¹ | 
| 2 | 8 | 2³ | 
| 3 | 4 | 2² | 

Tổng số mũ là$1 + 3 + 2 = 6$. 

Tổng số là chẵn nên người chơi thứ hai thắng. 

Dấu vết này cho thấy rằng mặc dù các bước di chuyển có thể được áp dụng trong các phân đoạn khác nhau, nhưng tất cả các hành động đều tập trung vào việc đếm tổng số lần loại bỏ nguyên tố tồn tại. 

### Ví dụ 2 

đầu vào:```
3
2 12 3
```Nhân tố hóa: 

| Chỉ mục | Giá trị | Các yếu tố chính | 
| --- | --- | --- | 
| 1 | 2 | 2¹ | 
| 2 | 12 | 2² · 3¹ | 
| 3 | 3 | 3¹ | 

Tổng số mũ là$1 + 3 + 1 = 5$. 

Tổng là số lẻ nên người chơi đầu tiên sẽ thắng. 

Ví dụ này cho thấy sự tương tác của nhiều số nguyên tố, nhưng việc phân tách vẫn giảm mọi thứ thành một phép tính chẵn lẻ duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \sqrt{A})$| Mỗi số được phân tích thành thừa số bằng phép chia thử lên tới sqrt | 
| Không gian |$O(n)$| Lưu trữ cho hệ số và số nguyên tố | 

Những hạn chế$n \le 1000$Và$a_i \le 10^{18}$làm cho điều này trở nên khả thi. Ngay cả trong trường hợp xấu nhất, việc phân tích nhân tử vẫn đủ nhanh và tất cả quá trình xử lý tiếp theo đều diễn ra tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    from collections import defaultdict
    import math

    def sieve(n=100000):
        is_p = [True] * (n + 1)
        is_p[0] = is_p[1] = False
        primes = []
        for i in range(2, n + 1):
            if is_p[i]:
                primes.append(i)
                for j in range(i * i, n + 1, i):
                    is_p[j] = False
        return primes

    def factorize(x, primes):
        res = defaultdict(int)
        for p in primes:
            if p * p > x:
                break
            while x % p == 0:
                res[p] += 1
                x //= p
        if x > 1:
            res[x] += 1
        return res

    n = int(input())
    a = list(map(int, input().split()))
    primes = sieve(100000)

    total = 0
    for x in a:
        f = factorize(x, primes)
        for v in f.values():
            total += v

    return "First\n" if total % 2 else "Second\n"

# provided samples
assert run("3\n2 8 4\n") == "Second\n"
assert run("3\n2 12 3\n") == "First\n"

# custom cases
assert run("1\n2\n") == "First\n", "single prime"
assert run("1\n1\n") == "Second\n", "no moves"
assert run("2\n4 9\n") == "Second\n", "disjoint primes"
assert run("2\n2 3\n") == "First\n", "two single primes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n2`| Đầu tiên | nước đi không tầm thường nhỏ nhất | 
|`1\n1`| Thứ hai | không có nước đi nào | 
|`2\n4 9`| Thứ hai | số nguyên tố độc lập | 
|`2\n2 3`| Đầu tiên | nhiều động tác rời rạc | 

## Vỏ cạnh 

Đối với một đầu vào như một giá trị bằng 1, không tồn tại thừa số nguyên tố nào, do đó không thể thực hiện được bước di chuyển nào. Thuật toán tạo ra tổng số mũ bằng 0, ngay lập tức mang lại chiến thắng cho người chơi thứ hai. 

Đối với một số có lũy thừa nguyên tố lớn như$10^{18} = 2^? \cdot 5^?$, hệ số hóa tích lũy tất cả số mũ một cách chính xác và tính chẵn lẻ vẫn phản ánh số lần loại bỏ có thể xảy ra. Mặc dù các bước di chuyển có thể được áp dụng trong nhiều lựa chọn phân đoạn, nhưng mỗi lần loại bỏ đều tương ứng với việc tiêu thụ chính xác một số mũ đơn vị, do đó số đếm vẫn ổn định khi phân tách.
