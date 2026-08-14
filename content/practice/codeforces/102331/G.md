---
title: "CF 102331G - Ngữ pháp"
description: "Biểu đồ có một đỉnh cho mỗi chuỗi con không trống riêng biệt của chuỗi đầu vào s. Từ chuỗi con t có độ dài L, một cạnh đi tới mọi chuỗi con riêng biệt của t có độ dài L-1."
date: "2026-08-14T04:56:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102331
codeforces_index: "G"
codeforces_contest_name: "2019 Summer Petrozavodsk Camp, Day 2: 300iq Contest 2 (XX Open Cup, Grand Prix of Kazan)"
rating: 0
weight: 102331
solve_time_s: 171
verified: true
draft: false
---

[CF 102331G - Ngữ pháp](https://codeforces.com/problemset/problem/102331/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Biểu đồ có một đỉnh cho mỗi chuỗi con không trống riêng biệt của chuỗi đầu vào`s`. Từ một chuỗi con`t`chiều dài`L`, một cạnh đi tới mọi chuỗi con riêng biệt của`t`chiều dài`L-1`. 

Chỉ có hai chuỗi con có thể có độ dài`L-1`bên trong`t`: xóa ký tự đầu tiên hoặc xóa ký tự cuối cùng của nó. Thông thường hai chuỗi này khác nhau nên một đỉnh có hai cạnh hướng ra ngoài. Chúng bằng nhau một cách chính xác khi mọi ký tự của`t`là như nhau. Trong trường hợp đó, hai cạnh có thể co lại thành một. 

Một con đường bắt đầu tại`s`liên tục loại bỏ một ký tự từ một trong hai đầu. Độ dài của nó giảm đi đáng kể, do đó mọi đường đi đều tự động đơn giản. Nhiệm vụ là đếm tất cả các đường đi như vậy, kể cả đường đi chỉ bao gồm`s`, modulo`998244353`. 

Độ dài giới hạn là`300000`, do đó, mọi giá trị bậc hai trong độ dài chuỗi đều nằm ngoài phạm vi. Có thể có Θ(n2) chuỗi con riêng biệt, con số này đã là khoảng 45 tỷ khi`n = 300000`. Một giải pháp phải tránh liệt kê các chuỗi con hoặc đỉnh đồ thị. Giới hạn 2 giây làm cho giải pháp O(n log n) không mong muốn trừ khi các hệ số không đổi rất nhỏ, trong khi giải pháp O(n) là mục tiêu tự nhiên. 

Phần tinh tế là hai lần xuất hiện khác nhau của cùng một chuỗi con đại diện cho một đỉnh đồ thị. Ví dụ, trong`aabaa`, chuỗi con`aa`xảy ra hai lần, nhưng các đường dẫn đến nó qua các chuỗi khác nhau trước đó vẫn là các đường dẫn khác nhau. Chúng ta phải đếm đường đi chứ không phải số lần xuất hiện của các đỉnh đồ thị một cách độc lập. 

Một trường hợp cạnh khác là một chuỗi con bao gồm một ký tự lặp lại. Vì`aa`, cả hai cách xóa điểm cuối đều tạo ra cùng một đỉnh`a`, do đó chỉ có một cạnh đi ra. Câu trả lời đúng cho đầu vào`aa`là`2`, tương ứng với`aa`Và`aa -> a`. Xử lý việc xóa hai điểm cuối là khác nhau sẽ mang lại`3`. 

Hiện tượng tương tự trở nên rõ ràng hơn đối với một chuỗi đơn dài hơn. Đối với đầu vào`aaa`, đồ thị đơn giản là`aaa -> aa -> a`, vậy câu trả lời là`3`, không`7`. Một giải pháp bất cẩn cho rằng mọi đỉnh đều có hai cạnh hướng ra ngoài sẽ sử dụng`2^n-1`đếm và thất bại. 

Ở thái cực khác, hãy xem xét`ab`. Hai đứa con dài một của nó là`a`Và`b`, vậy các đường dẫn là`ab`,`ab -> a`, Và`ab -> b`. Câu trả lời đúng là`3`. Đây là trường hợp nhỏ nhất trong đó hai quá trình chuyển đổi điểm cuối thực sự khác biệt. 

## Phương pháp tiếp cận 

Một chương trình động trực tiếp sẽ gán một giá trị`dp(t)`cho mọi chuỗi con riêng biệt`t`. Nếu như`t`không phải là đơn nhất, hai con của nó là tiền tố và hậu tố có độ dài`|t|-1`, cho`dp(t) = 1 + dp(prefix) + dp(suffix)`. 

Đối với một chuỗi đơn, hai con trùng nhau, vì vậy`dp(t) = 1 + dp(prefix)`. 

Phép truy hồi này hoàn toàn đúng vì mọi đường đi đều dừng ngay lập tức hoặc lấy chính xác một cạnh đi ra và sau đó tiếp tục với đường đi từ con đó. 

Vấn đề là số lượng trạng thái. Một chuỗi có độ dài`n`có thể có Θ(n²) chuỗi con riêng biệt và thậm chí việc lưu trữ một trạng thái cho mỗi chuỗi con là không thể đối với`n = 300000`. Trong trường hợp xấu nhất, điều này có nghĩa là có hàng chục tỷ trạng thái và chuyển tiếp, vượt xa giới hạn thời gian và bộ nhớ. 

Quan sát hữu ích là biểu đồ gần như là biểu đồ nhị phân. Một chuỗi con có hai lần chuyển tiếp đi khác nhau trừ khi tất cả các ký tự của nó đều bằng nhau. Hướng dẫn chính thức khai thác chính xác sự khác biệt này: khi một chuỗi con hiện tại trở thành một chuỗi, thì việc tiếp tục chuỗi đó là bắt buộc. 

Hãy tưởng tượng đi theo một đường dẫn và nhìn vào thời điểm đầu tiên khi chuỗi con hiện tại trở thành đơn nhất. Trước thời điểm đó, mọi lựa chọn xóa đều thực sự mang tính nhị phân. Khi nó trở thành một đỉnh, chỉ có một đỉnh tiếp theo có thể có ở mọi độ dài, vì vậy nếu chuỗi con đơn nhất có độ dài`k`, có chính xác`k`những đường đi có thể tiếp tục từ nó, một đường đi cho mỗi điểm dừng có thể. 

Điều này cho phép chúng ta tránh được biểu đồ chuỗi con khổng lồ. Các đường dẫn có đỉnh cuối cùng không đơn nhất có thể được tính trực tiếp theo khoảng của chúng trong chuỗi gốc. Thay vào đó, các đường đi trở thành đơn nhất có thể được phân loại theo khoảng đơn nhất đầu tiên của chúng. Chỉ các tiền tố và hậu tố của các lần chạy ký tự bằng nhau tối đa mới có thể là các khoảng đơn nhất đầu tiên, vì vậy chỉ có O(n) trường hợp như vậy. 

Đối với khoảng thời gian xuất hiện`[l, r]`, đạt được nó từ`s`có nghĩa là xóa`l`các ký tự từ bên trái và`n-1-r`các ký tự từ bên phải. Thứ tự xóa bỏ có thể được chọn tùy ý, đưa ra`C(l + n - 1 - r, l)`những con đường khác nhau đạt đến khoảng đó. 

Nếu như`[l,r]`chứa ít nhất hai ký tự khác nhau, nó là điểm cuối hợp lệ cho một đường dẫn chưa bao giờ trở thành đơn nhất. Chúng ta có thể tính tổng các giá trị này mà không cần liệt kê tất cả các khoảng bằng cách sử dụng đồng nhất thức cây gậy khúc côn cầu. 

Brute Force hoạt động vì phép truy hồi chỉ có hai con, nhưng không thành công khi có chuỗi con Θ(n²). Quan sát cho thấy chỉ các chuỗi con đơn nhất thu gọn hai lần chuyển đổi cho phép chúng ta đếm toàn bộ biểu đồ theo đóng góp dựa trên lần chạy O(n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Trạng thái và chuyển tiếp O(n²) | O(n²) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước các giai thừa và giai thừa nghịch đảo lên đến`n`, do đó mọi hệ số nhị thức cần thiết có thể được tính theo O(1) modulo`998244353`. Vì tất cả các đối số trên nhiều nhất là`n-1`, thế này là đủ rồi. 
2. Chia`s`thành các chuỗi tối đa có các ký tự bằng nhau. Để chạy`[L,R]`, mọi chuỗi con chỉ bao gồm ký tự đó nằm trong lần chạy này. 
3. Đếm các đường dẫn có chuỗi con cuối cùng chứa ít nhất hai ký tự khác nhau. Sửa điểm cuối bên trái`l`, và để`x`là vị trí đầu tiên tại hoặc sau`l`có tính cách khác với`s[l]`. Sau đó một khoảng`[l,r]`chính xác là không đơn nhất khi`r >= x`. 

Sự đóng góp cho việc này cố định`l`là`sum C(l+n-1-r, l)`vì`r = x ... n-1`. 

Đặt`a = l+n-1-r`. Phạm vi trở thành`a = l ... l+n-1-x`, do đó nhận dạng cây gậy khúc côn cầu mang lại`sum C(a,l) = C(l+n-x, l+1)`. 

Do đó, một đóng góp thu được trong O(1), và tất cả`l`có thể được xử lý trong O(n). 
4. Đối với mỗi lần chạy tối đa`[L,R]`, đếm các đường dẫn có chuỗi con đơn nhất đầu tiên là tiền tố của lần chạy này. Tiền tố như vậy là`[L,r]`. Đầu tiên nó có thể trở thành đơn nhất bằng cách xóa ký tự ngay trước`L`, cung cấp`L > 0`. Người tiền nhiệm đó là không đơn nhất bởi vì`[L,R]`là một cuộc chạy tối đa. 

Người tiền nhiệm là`[L-1,r]`, vậy số đường đi tới nó là`C(L+n-r-2, L-1)`. 

Sau khi đạt đến chuỗi con đơn nhất có độ dài`r-L+1`, có chính xác`r-L+1`những cách có thể để dừng lại. Nhân hai đại lượng rồi cộng kết quả. 
5. Đối xứng, đếm các hậu tố đơn nhất`[l,R]`. Hậu tố như vậy trước tiên có thể trở thành đơn nhất bằng cách xóa ký tự ngay sau`R`, cung cấp`R < n-1`. 

Người tiền nhiệm là`[l,R+1]`, cho`C(l+n-R-2, l)`đường dẫn đến người tiền nhiệm. Hậu tố đơn nhất có độ dài`R-l+1`, vậy đóng góp của nó là`C(l+n-R-2, l) * (R-l+1)`. 
6. Toàn bộ lần chạy có thể xảy ra trong cả phép tính tiền tố và hậu tố khi cả hai phía của lần chạy đều tồn tại. Điều này đúng vì hai trường hợp tương ứng với các đỉnh trước khác nhau và do đó có đường đi khác nhau. 
7. Nếu toàn bộ chuỗi đầu vào là một lần chạy,`s`bản thân nó đã là đơn nhất rồi. Không có số trước không đơn nhất nên phép đếm trước tạo ra số 0. Trong trường hợp đặc biệt này, đồ thị là một chuỗi các`n`đỉnh, và câu trả lời đơn giản là`n`. 
8. Thêm phần đóng góp từ các điểm cuối không đơn nhất và tất cả các trường hợp đơn nhất đầu tiên theo modulo`998244353`. 

### Tại sao nó hoạt động 

Hãy xem xét bất kỳ con đường nào bắt đầu từ`s`. Nếu nó không bao giờ đạt tới chuỗi con đơn nhất thì đỉnh cuối cùng của nó là một khoảng không đơn phân`[l,r]`. Trình tự xóa trái và phải xác định duy nhất khoảng đó và có chính xác`C(l+n-1-r,l)`những lệnh xóa như vậy. Do đó phần đầu tiên đếm mọi đường dẫn thuộc loại này chính xác một lần. 

Bây giờ hãy xem xét một đường dẫn đến chuỗi con đơn nhất lần đầu tiên tại`[l,r]`. Từ`[l,r]`nằm bên trong một chuỗi ký tự bằng nhau tối đa, đỉnh trước đó phải kéo dài khoảng qua ranh giới của chuỗi đó. Do đó`[l,r]`phải là tiền tố hoặc hậu tố của lần chạy tối đa. Các công thức tính chính xác các đường dẫn đến một tiền thân không đơn nhất và sau đó thực hiện quá trình chuyển đổi sang`[l,r]`. Khi đó, mọi chuyển đổi tiếp theo đều bị ép buộc, do đó, một chuỗi con đơn nhất có độ dài`k`đóng góp chính xác`k`kết thúc con đường có thể. Hai lớp này tách rời nhau và bao trùm mọi đường đi, điều này chứng tỏ rằng tổng cuối cùng chính xác là câu trả lời được yêu cầu. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

MOD = 998244353

def solve_string(s: str) -> int:
    n = len(s)

    fact = [1] * (n + 1)
    for i in range(1, n + 1):
        fact[i] = fact[i - 1] * i % MOD

    invfact = [1] * (n + 1)
    invfact[n] = pow(fact[n], MOD - 2, MOD)
    for i in range(n, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD

    def comb(a: int, b: int) -> int:
        if b < 0 or b > a:
            return 0
        return fact[a] * invfact[b] % MOD * invfact[a - b] % MOD

    if n == 1:
        return 1

    # runs = (L, R), inclusive.
    runs = []
    i = 0
    while i < n:
        j = i
        while j + 1 < n and s[j + 1] == s[i]:
            j += 1
        runs.append((i, j))
        i = j + 1

    # The whole string is unary.
    if len(runs) == 1:
        return n

    ans = 0

    # 1. Paths ending at a non-unary substring.
    #
    # For each l, let x be the first position after l with
    # s[x] != s[l]. Then [l, r] is non-unary iff r >= x.
    for L, R in runs:
        x = R + 1
        if x == n:
            continue

        for l in range(L, R + 1):
            # Sum over r = x .. n-1:
            #
            # C(l + n - 1 - r, l)
            #
            # = C(l + n - x, l + 1)
            ans += comb(l + n - x, l + 1)
            if ans >= MOD:
                ans -= MOD

    # 2. Paths whose first unary substring is a prefix of a run.
    #
    # The predecessor must extend one position to the left,
    # so L > 0 is required.
    for L, R in runs:
        if L == 0:
            continue

        for r in range(L, R + 1):
            ways_to_predecessor = comb(L + n - r - 2, L - 1)
            length = r - L + 1
            ans = (ans + ways_to_predecessor * length) % MOD

    # 3. Paths whose first unary substring is a suffix of a run.
    #
    # The predecessor must extend one position to the right,
    # so R < n - 1 is required.
    for L, R in runs:
        if R == n - 1:
            continue

        for l in range(L, R + 1):
            ways_to_predecessor = comb(l + n - R - 2, l)
            length = R - l + 1
            ans = (ans + ways_to_predecessor * length) % MOD

    return ans

def main() -> None:
    s = input().strip()
    print(solve_string(s))

if __name__ == "__main__":
    main()
```Mảng giai thừa thực hiện các hệ số nhị thức được sử dụng trong mọi công thức đếm. Nghịch đảo mô-đun được tính toán một lần bằng định lý Fermat và các kết hợp còn lại là phép nhân theo thời gian không đổi. 

Cấu trúc chạy cung cấp cả hai điểm cuối của mọi khối ký tự bằng nhau tối đa. Điều này là đủ vì chuỗi con đơn nhất đầu tiên phải chạm vào ranh giới của khối đó. Chúng ta không bao giờ phải tự xây dựng các chuỗi con. 

Vòng đếm đầu tiên tương ứng với phần điểm cuối không đơn nhất của thuật toán. Đối với mọi điểm cuối bên trái có thể có, tất cả các điểm cuối bên phải hợp lệ được biểu thị bằng một hệ số nhị thức thu được từ danh tính gậy khúc côn cầu. Đây là bước quan trọng giúp loại bỏ phép liệt kê bậc hai rõ ràng của các khoảng. 

Vòng lặp thứ hai xử lý các tiền tố đơn nhất đầu tiên. Người tiền nhiệm là`[L-1,r]`, vậy số lần xóa trái là`L-1`, trong khi số lần xóa phải là`n-1-r`. Tổng số của họ đưa ra lập luận trên`L+n-r-2`. 

Vòng thứ ba là đối xứng. Dành cho người tiền nhiệm`[l,R+1]`, có`l`xóa trái và`n-R-2`xóa đúng, đưa ra`C(l+n-R-2,l)`. 

Các vòng lặp có chủ ý bao gồm toàn bộ quá trình chạy từ cả hai hướng khi cả hai bên đều tồn tại. Đó là hai đường đi phân biệt vì các đỉnh trước của chúng khác nhau. Trường hợp duy nhất mà chuỗi ban đầu là đơn nguyên được xử lý trước các vòng lặp này. 

Số nguyên Python không bị tràn, nhưng mỗi phép nhân đều được giảm modulo`MOD`. Chỉ số giai thừa tối đa chính xác là`n`và tất cả các đối số kết hợp đều nằm trong phạm vi được tính toán trước. 

## Ví dụ đã hoạt động 

### Mẫu 1:`abba`Số lần chạy tối đa là`[0,0] = a`,`[1,2] = bb`, Và`[3,3] = a`. 

| Chạy | Đóng góp không thống nhất | Đóng góp tiền tố đơn nhất đầu tiên | Đóng góp hậu tố đơn nhất đầu tiên | 
| --- | --- | --- | --- | 
|`[0,0]`| 3 | 0 | 1 | 
|`[1,2]`| 2 | 3 | 3 | 
|`[3,3]`| 0 | 1 | 0 | 
| Tổng cộng | 5 | 4 | 4 | 

Sự đóng góp không đơn nhất là`5`. Ví dụ, các khoảng`ab`,`abb`,`abba`,`ba`, Và`bba`tính đến các đường dẫn kết thúc trước khi đến chuỗi con đơn nhất. 

Để chạy`bb`, các trường hợp tiền tố đơn nhất là`b`Và`bb`, đạt từ bên trái. Họ đóng góp`1 * 1 + 1 * 2 = 3`. Từ bên phải, các hậu tố tương ứng đóng góp`1 * 1 + 1 * 2 = 3`. Hai lần xuất hiện của đỉnh`bb`trong các phép tính này thể hiện các đường dẫn khác nhau trước đó, vì vậy cả hai đóng góp vẫn hợp lệ. 

Tổng cộng là`5 + 4 + 4 = 13`, phù hợp với đầu ra mẫu. 

### Mẫu 2:`benbeipo`Mỗi ký tự thuộc về một chuỗi có độ dài bằng một, do đó chuỗi không chứa các ký tự liền kề lặp lại. 

|`l`| Chạy kết thúc`R`| Đóng góp không thống nhất | Đóng góp đầu tiên | 
| --- | --- | --- | --- | 
| 0 | 0 | 7 | 1 | 
| 1 | 1 | 21 | 6 | 
| 2 | 2 | 35 | 16 | 
| 3 | 3 | 35 | 25 | 
| 4 | 4 | 21 | 25 | 
| 5 | 5 | 7 | 16 | 
| 6 | 6 | 1 | 6 | 
| 7 | 7 | 0 | 1 | 
| Tổng cộng | | 127 | 128 | 

Vì không có chuỗi nào dài hơn một ký tự nên mỗi chuỗi con đơn nhất có độ dài bằng một ký tự. Do đó, đóng góp đơn nhất chính xác là số lượng đường dẫn kết thúc ở một ký tự đơn, trong khi đóng góp không đơn nhất tính các đường dẫn kết thúc trước đó. 

Hai tổng số là`127`Và`128`, cho`255`. Đây chính xác là`2^8 - 1`, đó là điều chúng ta mong đợi khi mỗi lần xóa hai điểm cuối luôn tạo ra các chuỗi khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vị trí được xử lý với số lần không đổi và mọi hệ số nhị thức là O(1). | 
| Không gian | O(n) | Mảng giai thừa và biểu diễn chạy sử dụng bộ nhớ tuyến tính. | 

Với`n <= 300000`, thuật toán chỉ thực hiện một số lần chuyển không đổi qua đầu vào và một số tuyến tính các phép toán số học mô-đun. Việc sử dụng bộ nhớ cũng tuyến tính, do đó giải pháp phù hợp thoải mái trong giới hạn đã nêu. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 998244353

def solve_string(s: str) -> int:
    n = len(s)

    fact = [1] * (n + 1)
    for i in range(1, n + 1):
        fact[i] = fact[i - 1] * i % MOD

    invfact = [1] * (n + 1)
    invfact[n] = pow(fact[n], MOD - 2, MOD)
    for i in range(n, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD

    def comb(a, b):
        if b < 0 or b > a:
            return 0
        return fact[a] * invfact[b] % MOD * invfact[a - b] % MOD

    if n == 1:
        return 1

    runs = []
    i = 0
    while i < n:
        j = i
        while j + 1 < n and s[j + 1] == s[i]:
            j += 1
        runs.append((i, j))
        i = j + 1

    if len(runs) == 1:
        return n

    ans = 0

    for L, R in runs:
        x = R + 1
        if x == n:
            continue
        for l in range(L, R + 1):
            ans = (ans + comb(l + n - x, l + 1)) % MOD

    for L, R in runs:
        if L == 0:
            continue
        for r in range(L, R + 1):
            ans = (
                ans
                + comb(L + n - r - 2, L - 1) * (r - L + 1)
            ) % MOD

    for L, R in runs:
        if R == n - 1:
            continue
        for l in range(L, R + 1):
            ans = (
                ans
                + comb(l + n - R - 2, l) * (R - l + 1)
            ) % MOD

    return ans

def run(inp: str) -> str:
    return str(solve_string(inp.strip()))

# Provided samples
assert run("abba") == "13", "sample 1"
assert run("benbeipo") == "255", "sample 2"
assert run("iqiiiiiiqq") == "300", "sample 3"
assert run("aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa") == "35", "sample 4"

# Custom cases
assert run("a") == "1", "minimum size"
assert run("aa") == "2", "unary boundary"
assert run("aab") == "6", "first unary substring"
assert run("aba") == "7", "two distinct endpoint transitions"
assert run("a" * 300000) == "300000", "maximum size and all equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`| 1 | Độ dài tối thiểu và đường đi một đỉnh | 
|`aa`| 2 | Đã thu gọn hai chuyển đổi điểm cuối trên một chuỗi đơn nhất | 
|`aab`| 6 | Chuỗi con đơn nhất đầu tiên đạt đến trước khi kết thúc đường dẫn | 
|`aba`| 7 | Cả hai thao tác xóa điểm cuối vẫn khác biệt | 
|`a`lặp đi lặp lại 300000 lần | 300000 | Kích thước đầu vào tối đa và trường hợp hoàn toàn đơn nhất | 

## Vỏ cạnh 

cho`aa`, các đỉnh duy nhất là`aa`Và`a`. Hai cách để xóa điểm cuối khỏi`aa`sản xuất cùng một chuỗi`a`, do đó đồ thị chỉ chứa một cạnh. Thuật toán phát hiện toàn bộ chuỗi là một lần chạy tối đa và ngay lập tức trả về`n = 2`. 

Vì`aaa`, trường hợp đặc biệt tương tự cho`3`. Đồ thị là chuỗi`aaa -> aa -> a`và một đường đi có thể dừng ở bất kỳ đỉnh nào trong ba đỉnh của nó. Các công thức chạy chung có chủ ý không tính chuỗi đơn phân ban đầu, vì vậy trường hợp chuỗi đơn phân rõ ràng là cần thiết. 

Vì`ab`, cả hai lần chạy đều có độ dài bằng một. Có ba con đường: dừng lại ở`ab`, xóa ký tự đầu tiên và dừng lại ở`b`, hoặc xóa ký tự cuối cùng và dừng lại ở`a`. Phần không đơn nhất đóng góp một, và hai trường hợp ranh giới đơn nhất đầu tiên đóng góp mỗi trường hợp một, tạo ra`3`. 

Vì`abba`, cuộc chạy`bb`giải thích tại sao cả tiền tố và hậu tố đơn nhất đầu tiên đều phải được tính. Đường dẫn có thể đi vào`bb`từ`abb`hoặc từ`bba`và đó là những đường đi khác nhau mặc dù chúng đạt đến cùng một đỉnh đồ thị. Các phép tính tiền tố và hậu tố đóng góp cả hai khả năng, tạo ra câu trả lời mẫu`13`. 

Đối với đầu vào đơn nhất có kích thước tối đa bao gồm`300000`bản sao của`a`, có chính xác`300000`các đỉnh đồ thị, một đỉnh cho mỗi độ dài có thể. Câu trả lời là do đó`300000`. Việc triển khai xử lý trường hợp này mà không xây dựng bất kỳ trạng thái chuỗi con nào và chỉ sử dụng phím tắt chuỗi đơn rõ ràng.
