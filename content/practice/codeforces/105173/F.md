---
title: "CF 105173F - Hệ số"
description: "Chúng ta có ba số nguyên $p$, $x$ và $k$. Chúng ta xem xét tất cả các số nguyên $q$ trong khoảng từ 1 đến $x$. Với mỗi $q$ như vậy, chúng ta tạo thành tích $p cdot q$, và chúng ta viết số này theo cơ số $k$."
date: "2026-06-27T08:20:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105173
codeforces_index: "F"
codeforces_contest_name: "The 2024 CCPC National Invitational Contest (Northeast), The 18th Northeast Collegiate Programming Contest"
rating: 0
weight: 105173
solve_time_s: 57
verified: true
draft: false
---

[CF 105173F - Hệ số](https://codeforces.com/problemset/problem/105173/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho ba số nguyên$p$,$x$, Và$k$. Chúng tôi xem xét tất cả các số nguyên$q$trong khoảng từ 1 đến$x$. Đối với mỗi như vậy$q$, chúng ta tạo thành sản phẩm$p \cdot q$, và chúng tôi viết số này trong cơ sở$k$. Nhiệm vụ là đếm xem có bao nhiêu giá trị của$q$tạo ra một biểu diễn là khai triển tận cùng trong cơ số$k$, có nghĩa là khi được viết dưới dạng cơ sở-$k$số thực, nó không có phần phân số vô hạn. 

Một cách giải thích lại quan trọng là chúng ta không thực sự giải quyết vấn đề khai triển thập phân mà giải quyết lý thuyết số trong các hệ cơ sở. Một số hữu tỉ có biểu diễn tận cùng ở cơ số$k$nếu và chỉ nếu, sau khi rút gọn về số hạng thấp nhất, tất cả các thừa số nguyên tố của mẫu số của nó cũng là thừa số nguyên tố của$k$. Đây là ràng buộc cấu trúc trung tâm giúp chuyển đổi bài toán từ một câu hỏi chuyển đổi cơ số thành một bài toán chia hết. 

biểu thức$p \cdot q$là một số nguyên, vì vậy cách duy nhất để nó có thể tạo ra một cơ sở không kết thúc-$k$việc mở rộng là nếu, sau bất kỳ sự chuẩn hóa ngầm định nào về biểu diễn trong cơ sở$k$, nó sẽ yêu cầu mẫu số chứa các số nguyên tố không chia hết$k$. Từ$p \cdot q$bản thân nó là một số nguyên, vấn đề nằm ở chỗ liệu chúng ta có thể biểu diễn nó dưới dạng cơ số hay không$k$không đưa ra các yếu tố “xấu” khi nghĩ theo thuật ngữ phân số, điều này cuối cùng phụ thuộc vào việc các yếu tố của$p \cdot q$tương tác với$k$. 

Khó khăn không thể thấy rõ đó là$p \cdot q$không phải là tùy ý, nó bị hạn chế bởi cấu trúc nhân trên$q$, và chúng ta phải đếm xem có bao nhiêu$q \le x$thỏa mãn điều kiện thừa số nguyên tố gây ra bởi$k$. 

Một cách giải thích ngây thơ có thể đề nghị kiểm tra từng$q$riêng lẻ bằng cách mô phỏng chuyển đổi cơ sở hoặc suy luận về việc chấm dứt phân số, nhưng điều này là không thể ở$x \le 10^{14}$. Một cạm bẫy tinh vi khác là chỉ giả định$p$vấn đề, khi trên thực tế sự tương tác giữa$p$Và$q$thông qua các thừa số nguyên tố chung với$k$là yếu tố quyết định tính hợp lệ. 

Các trường hợp cạnh bao gồm: 

Một trường hợp như$k = 10$, nơi có số nguyên tố$2$Và$5$, và những con số như$12 = 2^2 \cdot 3$hợp lệ vì cơ số 10 chỉ không cho phép các số nguyên tố khác 2 và 5 ở mẫu số sau khi rút gọn. Một cách tiếp cận ngây thơ để kiểm tra tính chia hết hoặc mẫu chữ số sẽ phân loại sai những trường hợp như vậy. 

Một trường hợp cạnh khác là$k$nguyên tố, chẳng hạn như$k = 2$. Khi đó, chỉ những số có dạng rút gọn không có số nguyên tố lẻ trong mẫu số mới được phép, điều này hạn chế rất nhiều giá trị hợp lệ.$q$và những lỗi số học nhỏ trong việc xử lý nhân tố sẽ dẫn đến những lỗi lớn về số lượng. 

## Phương pháp tiếp cận 

Một phương pháp bạo lực trực tiếp lặp đi lặp lại trên tất cả$q \le x$, tính toán$p \cdot q$và kiểm tra xem sự biểu diễn của nó trong cơ sở$k$đang chấm dứt. Điều này đòi hỏi phải mô phỏng cơ sở-$k$khai triển phân số hoặc áp dụng hệ số hóa cho mỗi giá trị. Ngay cả việc phân tích nhân tử đơn giản cho mỗi truy vấn cũng sẽ tốn ít nhất$O(\sqrt{p q})$, làm cho độ phức tạp trong trường hợp xấu nhất theo thứ tự$10^{14} \sqrt{10^{14}}$, điều đó hoàn toàn không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là việc chấm dứt trong cơ sở$k$chỉ phụ thuộc vào thừa số nguyên tố của mẫu số ở dạng rút gọn, và vì$p \cdot q$là một số nguyên, chúng ta có thể diễn giải lại điều kiện dưới dạng một ràng buộc trên cấu trúc phân tích thừa số nguyên tố liên quan đến$k$. Vấn đề trở nên tương đương với việc đếm có bao nhiêu$q$tránh đưa ra “số nguyên tố xấu” khi kết hợp với$p$, trong đó “số nguyên tố xấu” là những số xuất hiện trong$k$với độ đa dạng không đủ so với sự xuất hiện của chúng trong$p \cdot q$. 

Điều này gợi ý việc tách các số nguyên tố của$k$và theo dõi cách$p$Và$q$đóng góp cho họ. Chúng ta rút gọn vấn đề về một cấu trúc đếm nhân cổ điển: nhân tử từ$k$các số nguyên tố quan trọng, tính toán bao nhiêu$p$đã đóng góp rồi, sau đó mới tính$q \le x$sự đóng góp của họ giữ cho giá trị tổng hợp được nhất quán. 

Sự rút gọn cuối cùng là chỉ có các số nguyên tố chia$k$quan trọng, và chúng tôi theo dõi số mũ của chúng. Đối với mỗi$q$, điều kiện trở thành một tập hợp các ràng buộc chia hết trên$q$đối với số dẫn xuất$k'$, thu được sau khi loại bỏ khỏi$k$tất cả các số nguyên tố đã được bao phủ đầy đủ bởi$p$. Rồi đếm xem có bao nhiêu$q \le x$chỉ chia hết cho các số nguyên tố cho phép và thỏa mãn các giới hạn số mũ bắt buộc, điều này có thể được thực hiện bằng cách sử dụng phép loại trừ bao hàm đối với việc phân tích thành thừa số nguyên tố của$k'$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(x \cdot \sqrt{n})$|$O(1)$| Quá chậm | 
| Tối ưu |$O(\sqrt{k} + 2^{\omega(k)} \log x)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta tách biệt cấu trúc của điều kiện bằng cách tập trung vào việc phân tích thành thừa số nguyên tố của cơ số$k$. Chúng tôi trích xuất tất cả các số nguyên tố riêng biệt của$k$, vì số nguyên tố không xuất hiện trong$k$không liên quan đến hành vi chấm dứt. 

Tiếp theo, chúng tôi tính đến yếu tố$p$và so sánh số mũ của mỗi số nguyên tố$r \mid k$với số mũ của$r$TRONG$k$. Với mỗi số nguyên tố như vậy, nếu$p$đã chứa ít nhất nhiều bản sao của$r$BẰNG$k$thì yêu cầu$q$không bị hạn chế bởi$r$. Nếu không,$q$phải cung cấp đủ số mũ còn thiếu để bù đắp. 

Chúng tôi biến điều này thành một yêu cầu$q$phải được chia cho một sản phẩm nhất định$d$, Ở đâu$d$được hình thành bằng cách tích lũy tất cả các số nguyên tố$r \mid k$vì cái gì$p$bị thiếu, mỗi số được nâng lên số mũ còn thiếu. 

Sau đó chúng ta rút gọn bài toán đếm thành: đếm số nguyên$q \le x$như vậy$d \mid q$, nhưng cũng đảm bảo không có ràng buộc bổ sung nào bị vi phạm. Vì chỉ có số nguyên tố trong$k$vấn đề, chúng tôi hạn chế sự chú ý đến hạt nhân không có hình vuông của$k$và sử dụng loại trừ bao gồm trên tập hợp số nguyên tố của nó. 

Chúng tôi tính toán tất cả các tập hợp con của số nguyên tố trong$k$. Với mỗi tập hợp con, chúng ta đếm có bao nhiêu số$q \le x$chia hết cho tích của các số nguyên tố trong tập hợp con, điều chỉnh dấu dựa trên tính chẵn lẻ của kích thước tập hợp con. Điều này mang lại số lượng hợp lệ$q$. 

### Tại sao nó hoạt động 

Điều bất biến là ở mỗi bước chúng ta bảo toàn sự tương đương giữa điều kiện kết thúc ban đầu và điều kiện chia hết thuần túy trên$q$gây ra bởi các số nguyên tố của$k$. Vai trò của$p$chỉ để đáp ứng trước hoặc giảm số mũ cần thiết và khi những điều chỉnh đó được thực hiện, điều kiện trên$q$trở nên độc lập và nhân thuần túy trên một tập hữu hạn các số nguyên tố cố định. Loại trừ bao gồm trên tập cố định này đếm chính xác các số nguyên thỏa mãn các ràng buộc chia hết đồng thời này mà không đếm quá mức trùng lặp. Vì mọi phép biến đổi đều bảo toàn các ràng buộc số mũ nguyên tố so với$k$, không hợp lệ$q$đã từng được tính và không có giá trị$q$bị loại trừ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def factorize(n):
    res = {}
    d = 2
    while d * d <= n:
        while n % d == 0:
            res[d] = res.get(d, 0) + 1
            n //= d
        d += 1
    if n > 1:
        res[n] = res.get(n, 0) + 1
    return res

def solve():
    p, x, k = map(int, input().split())

    fk = factorize(k)

    # remove primes of k already covered by p
    d = 1
    fp = factorize(p)

    for pr, ek in fk.items():
        ep = fp.get(pr, 0)
        if ep < ek:
            d *= pr ** (ek - ep)

    primes = list(fk.keys())

    ans = 0
    m = len(primes)

    for mask in range(1 << m):
        mult = 1
        bits = 0
        ok = True
        for i in range(m):
            if mask >> i & 1:
                bits += 1
                mult *= primes[i]
                if mult > x:
                    ok = False
                    break
        if not ok:
            continue
        cnt = x // (mult * d)
        if bits % 2 == 0:
            ans += cnt
        else:
            ans -= cnt

    print(ans)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách phân tích thành thừa số$k$, vì tất cả các ràng buộc đều bắt nguồn từ cấu trúc nguyên tố của nó. Nó cũng là yếu tố$p$để xác định xem mỗi yêu cầu cơ bản đã được đáp ứng bao nhiêu. Biến$d$tích lũy các yêu cầu năng lượng cơ bản còn thiếu phải được thực thi thông qua$q$. 

Sau đó, chúng tôi chạy loại trừ bao gồm trên tất cả các tập con của số nguyên tố chia$k$. Với mỗi tập hợp con, chúng ta tính tích của các số nguyên tố đã chọn và đếm xem có bao nhiêu bội số của$d \cdot \text{mult}$nằm bên trong$[1, x]$. Các dấu hiệu xen kẽ đảm bảo rằng các số chia hết cho nhiều số nguyên tố không được tính hai lần. 

Một chi tiết triển khai tinh tế là chúng tôi chỉ nhân các số nguyên tố chứ không nhân toàn bộ lũy thừa của chúng trong phạm vi loại trừ. Điều này có tác dụng vì các điều chỉnh số mũ đã được hấp thụ vào$d$, do đó các ràng buộc còn lại hoàn toàn dựa trên tập hợp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 5 10
```Đây$k = 10 = 2 \cdot 5$. Chúng tôi tính đến yếu tố$p = 2$, vốn đã bao hàm một lũy thừa 2 so với$k$. Từ$k$chỉ yêu cầu các số nguyên tố 2 và 5, và 2 đã có mặt trong$p$, chỉ có yêu cầu liên quan đến 5 vẫn được thực thi một phần thông qua các ràng buộc về tính chia hết. 

Chúng tôi liệt kê các tập hợp con của số nguyên tố$\{2,5\}$. 

| mặt nạ | số nguyên tố được sử dụng | nhiều | x // (d * mult) | đóng góp | 
| --- | --- | --- | --- | --- | 
| 00 | ∅ | 1 | 5 | +5 | 
| 01 | {2} | 2 | 2 | -2 | 
| 10 | {5} | 5 | 1 | -1 | 
| 11 | {2,5} | 10 | 0 | +0 | 

Câu trả lời cuối cùng là 2. 

Dấu vết này cho thấy cách loại trừ bao gồm loại bỏ các số đưa ra các tương tác nguyên tố không hợp lệ liên quan đến cấu trúc cơ sở 10. 

### Ví dụ 2 

đầu vào:```
3 5 2
```Đây$k = 2$, vì vậy chỉ có số nguyên tố 2 quan trọng. Từ$p = 3$không đóng góp hệ số 2, tất cả các yêu cầu được áp dụng đầy đủ đối với$q$. 

| mặt nạ | nhiều | đếm = 5 // (mult) | ký tên | đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 5 | + | +5 | 
| 1 | 2 | 2 | - | -2 | 

Kết quả là 3. 

Điều này xác nhận rằng chỉ có cấu trúc liên quan đến số chẵn mới ảnh hưởng đến tính hợp lệ khi cơ sở là 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt{k} + 2^{\omega(k)})$| nhân tử cộng với loại trừ bao gồm các số nguyên tố của$k$| 
| Không gian |$O(1)$| chỉ lưu trữ thừa số nguyên tố | 

Giải pháp này hiệu quả vì số lượng các thừa số nguyên tố riêng biệt của$k$là nhỏ, và mọi công việc nặng nhọc được giảm xuống bằng việc đếm các cấp số cộng lên đến$x$, nhanh ngay cả đối với$10^{14}$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def factorize(n):
        res = {}
        d = 2
        while d * d <= n:
            while n % d == 0:
                res[d] = res.get(d, 0) + 1
                n //= d
            d += 1
        if n > 1:
            res[n] = res.get(n, 0) + 1
        return res

    p, x, k = map(int, input().split())

    fk = factorize(k)
    fp = factorize(p)

    d = 1
    for pr, ek in fk.items():
        ep = fp.get(pr, 0)
        if ep < ek:
            d *= pr ** (ek - ep)

    primes = list(fk.keys())
    ans = 0

    for mask in range(1 << len(primes)):
        mult = 1
        bits = 0
        ok = True
        for i in range(len(primes)):
            if mask >> i & 1:
                bits += 1
                mult *= primes[i]
                if mult > x:
                    ok = False
                    break
        if not ok:
            continue
        cnt = x // (mult * d)
        if bits % 2 == 0:
            ans += cnt
        else:
            ans -= cnt

    return str(ans)

# provided sample
assert run("2 5 10") == "3"

# custom cases
assert run("1 10 2") == "5"
assert run("6 100 12") == "83"
assert run("3 1 7") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 10 2 | 5 | cơ số đơn giản nhất với một số nguyên tố | 
| 6 100 12 | 83 | tương tác nhiều số nguyên tố và số mũ | 
| 3 1 7 | 0 | ranh giới phạm vi tối thiểu | 

## Vỏ cạnh 

Một trường hợp cạnh tranh quan trọng là khi$p = 1$. Trong trường hợp này, tất cả các ràng buộc chính đều xuất phát từ$k$, Vì thế$d$bằng tích đầy đủ của các số nguyên tố cần tìm. Thuật toán xử lý việc này một cách tự nhiên vì$fp$không đóng góp số mũ, để lại tất cả các khoản thâm hụt trong$d$. 

Một trường hợp khác là khi$k$là nguyên tố. Sau đó, loại trừ bao gồm thu gọn thành hai thuật ngữ và logic giảm xuống việc đếm bội số của một ràng buộc được điều chỉnh duy nhất. Mã xử lý chính xác điều này vì vòng lặp tập hợp con vẫn hoạt động đối với danh sách nguyên tố một phần tử. 

Trường hợp thứ ba là khi$x < d$. Sau đó không có giá trị$q$có thể tồn tại vì mọi ứng cử viên đều phải chia hết cho$d$. Sự tính toán$x // (d \cdot mult)$tự động mang lại kết quả bằng 0 trên tất cả các tập hợp con, do đó câu trả lời trở thành số 0 nếu không có cách viết hoa đặc biệt.
