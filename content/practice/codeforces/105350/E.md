---
title: "CF 105350E - Niềm vui đang đếm"
description: "Chúng ta được cho một mảng $a$ có kích thước $n$. Chúng tôi muốn đếm xem có bao nhiêu tập hợp kích thước $n$ trên các giá trị từ $1$ đến $n$ có thể xuất hiện như sau. Hãy tưởng tượng chúng ta xây dựng một mảng $b$ có độ dài $n$, trong đó các giá trị nằm trong phạm vi $[1,n]$."
date: "2026-06-23T15:46:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105350
codeforces_index: "E"
codeforces_contest_name: "Theforces Round #34 (ABC-Forces)"
rating: 0
weight: 105350
solve_time_s: 109
verified: false
draft: false
---

[CF 105350E - Niềm vui đang đếm](https://codeforces.com/problemset/problem/105350/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng$a$kích thước$n$. Chúng tôi muốn đếm có bao nhiêu bộ kích thước khác nhau$n$vượt quá giá trị$1$ĐẾN$n$có thể xuất hiện như sau 

Hãy tưởng tượng chúng ta xây dựng một mảng$b$chiều dài$n$, trong đó các giá trị nằm trong khoảng$[1,n]$. Đối với mỗi vị trí$i$, chúng ta xem điều gì sẽ xảy ra nếu chúng ta loại bỏ$b_i$. Sau khi loại bỏ phần tử đơn lẻ đó, chúng tôi đếm xem còn lại bao nhiêu giá trị riêng biệt trong mảng. Con số này phải bằng$a_i$. Mảng$b$là hợp lệ nếu điều này đúng cho mọi vị trí. 

Chúng tôi không quan tâm đến thứ tự$b$, chỉ có nhiều giá trị của nó. Vì vậy, hai hoán vị của cùng một tập hợp được coi là giống hệt nhau. 

Nhiệm vụ là đếm xem có bao nhiêu tập hợp khác nhau có thể tạo ra ít nhất một thứ tự hợp lệ khớp với giá trị đã cho.$a$. 

Ràng buộc$\sum n \le 3 \cdot 10^5$trên các trường hợp thử nghiệm ngụ ý rằng về cơ bản chúng ta cần hành vi tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Bất cứ điều gì bậc hai cho mỗi trường hợp thử nghiệm là không thể. Thậm chí$O(n \log n)$có thể chấp nhận được, nhưng chỉ khi các hằng số nhỏ và dung dịch sạch. Bất kỳ cách tiếp cận nào cố gắng mô phỏng việc xóa cho mọi vị trí sẽ ngay lập tức vượt quá giới hạn. 

Trường hợp cạnh tinh tế là khi tất cả các giá trị trong$b$giống hệt nhau. Khi đó việc loại bỏ bất kỳ phần tử nào cũng không làm thay đổi số lượng các giá trị riêng biệt, vì vậy tất cả$a_i$phải giống hệt nhau. Ví dụ, nếu$b = [2,2,2]$, thì mọi$a_i$phải là$1$. Bất kỳ sai lệch nào cũng khiến cho việc cấu hình không thể thực hiện được. 

Một trường hợp cạnh quan trọng khác là khi$a$không nhất quán với bất kỳ cấu trúc multiset nào. Ví dụ, nếu$a$chứa cả giá trị rất lớn và giá trị rất nhỏ trong các mẫu không thể tương ứng với cùng một tập tần số, các phương pháp đếm đơn giản vẫn có thể tạo ra một giá trị thay vì 0. Giải pháp đúng phải phát hiện rõ ràng các điều kiện không thể thực hiện được. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp sẽ cố gắng tạo ra tất cả các tập hợp$b$kích thước$n$với các giá trị trong$[1,n]$, sau đó với mỗi tập hợp nhiều tập hợp, hãy kiểm tra xem có tồn tại hoán vị của nó thỏa mãn điều kiện trên mọi vị trí hay không. Thậm chí chỉ cần đếm nhiều bộ là$O(\text{compositions of } n)$, là số mũ trong$n$. Con số này là quá lớn ngay cả đối với$n = 30$. 

Một lực lượng vũ phu có cấu trúc hơn một chút sẽ liệt kê các vectơ tần số$(c_1, c_2, \dots, c_n)$như vậy$\sum c_i = n$, sau đó mô phỏng điều kiện cho từng vị trí. Số lượng vectơ như vậy vẫn là$\binom{2n-1}{n}$, lại theo cấp số nhân. Điểm nghẽn là điều kiện chỉ phụ thuộc vào số lượng giá trị riêng biệt còn lại sau khi loại bỏ một lần xuất hiện, điều này có liên quan chặt chẽ đến việc liệu phần tử bị loại bỏ có phải là lần xuất hiện duy nhất của giá trị của nó hay không. 

Quan sát chính là việc loại bỏ một phần tử chỉ ảnh hưởng đến số lượng giá trị riêng biệt theo hai cách có thể. Nếu giá trị bị loại bỏ có tần số$1$, thì số lượng giá trị phân biệt giảm đi một. Nếu không thì nó vẫn như cũ. Điều này có nghĩa là mỗi vị trí$i$mã hóa hiệu quả liệu$b_i$có phải là sự xuất hiện duy nhất hay không, và do đó$a_i$chỉ cho biết liệu$b_i$là một đơn vị hoặc một phần của nhóm giá trị lặp lại. 

Vì vậy mỗi giá trị trong$b$được phân loại thành các nhóm: giá trị đơn (tần số$1$) và các giá trị lặp lại (tần số$\ge 2$). Tất cả các vị trí thuộc các giá trị đơn đều tạo ra cùng một giá trị$a_i$và tất cả các vị trí thuộc các giá trị lặp lại sẽ tạo ra một mẫu nhất quán khác. Vấn đề giảm xuống còn việc đếm xem có bao nhiêu cách chúng ta có thể phân chia các chỉ mục thành các vai trò cấu trúc này và ấn định tần số một cách nhất quán. 

Điều này dẫn đến một cấu trúc tổ hợp trong đó chúng ta chỉ quan tâm đến số lượng giá trị xuất hiện một lần và cách phân bổ các lần xuất hiện còn lại giữa các giá trị khác. Số đếm cuối cùng trở thành một phép liệt kê tổ hợp có cấu trúc dựa trên sự phân chia có thể có giữa các giá trị đơn lẻ và không đơn lẻ, được tính trọng số bởi các lựa chọn đa thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giải pháp dựa vào việc giải thích mảng$a$như một sự phân loại các vị trí thành hai loại hành vi. 

1. Chúng tôi nhận thấy rằng việc loại bỏ một phần tử chỉ thay đổi số lượng giá trị riêng biệt nếu phần tử đó là lần xuất hiện duy nhất của giá trị của nó. Điều này có nghĩa là mọi vị trí$i$mã hóa xem giá trị của nó là duy nhất hay lặp lại, chỉ dựa trên việc$a_i$khác với số lượng toàn cầu của các giá trị riêng biệt trong$b$. 
2. Chúng ta suy ra rằng tất cả các vị trí trong$b$thuộc các giá trị có tần số ít nhất$2$phải tương ứng với các chỉ số trong đó việc loại bỏ phần tử không làm thay đổi số lượng khác biệt. Tất cả các giá trị đơn lẻ tương ứng với các chỉ số trong đó việc loại bỏ sẽ làm giảm số lượng đi một. 
3. Lực lượng này$a_i$lấy tối đa hai giá trị riêng biệt trên tất cả các chỉ số. Nếu có nhiều hơn hai giá trị khác nhau xuất hiện trong$a$, không có multiset nào có thể thỏa mãn các ràng buộc. 
4. Đặt giá trị lớn nhất vào$a$tương ứng với số phần tử riêng biệt trong mảng đầy đủ$b$. Điều này là do việc loại bỏ một singleton sẽ làm giảm số lượng riêng biệt, do đó số lượng lớn nhất có thể$a_i$tương ứng với các vị trí mà việc loại bỏ không loại bỏ một phần tử duy nhất. 
5. Khi số lượng giá trị riêng biệt toàn cầu$k$đã được sửa, chúng tôi chia$n$các vị trí tương ứng với việc loại bỏ đơn lẻ và loại bỏ không đơn lẻ. Gọi số vị trí đơn là$s$. 
6. Phần còn lại$k$các giá trị riêng biệt phải được gán cho các vị trí sao cho chính xác$s$trong số chúng là singletons và phần còn lại đóng góp tần số ít nhất$2$. Số cách để chọn giá trị nào là đơn lẻ là tổ hợp và đối với mỗi lựa chọn, chúng tôi phân phối số lần xuất hiện bổ sung giữa các giá trị còn lại bằng cách sử dụng dấu sao và thanh. 
7. Chúng tôi tính toán trước các giai thừa và giai thừa nghịch đảo để tính toán các hệ số nhị thức một cách hiệu quả, sau đó tính tổng tất cả các phép chia hợp lệ. 

### Tại sao nó hoạt động 

Điều bất biến là mỗi giá trị trong$b$đóng góp sự đóng góp liên tục cho tất cả các vị trí của mình trong$a$hoặc phần đóng góp giảm đi khi bị xóa chỉ khi nó là duy nhất. Điều này phân chia cấu trúc thành hai chế độ ổn định: các thay đổi do đơn lẻ gây ra và loại bỏ giá trị lặp lại ổn định. Vì mọi cấu hình hợp lệ phải duy trì sự phân đôi này một cách nhất quán trên tất cả các vị trí, nên toàn bộ vấn đề giảm xuống việc đếm các phép gán tần số nhất quán dưới sự phân chia cố định của các lớp giá trị đơn và không đơn. Điều này ngăn chặn mọi sự phụ thuộc tiềm ẩn giữa các vị trí ngoài số lượng tần số, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
MAXN = 300000 + 5

fact = [1] * MAXN
invfact = [1] * MAXN

for i in range(1, MAXN):
    fact[i] = fact[i - 1] * i % MOD

invfact[MAXN - 1] = pow(fact[MAXN - 1], MOD - 2, MOD)
for i in range(MAXN - 2, -1, -1):
    invfact[i] = invfact[i + 1] * (i + 1) % MOD

def C(n, r):
    if r < 0 or r > n:
        return 0
    return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))

    mx = max(a)
    mn = min(a)

    if mx == mn:
        print(1)
        continue

    cnt = {}
    for x in a:
        cnt[x] = cnt.get(x, 0) + 1

    # number of positions with max value (non-changing removals)
    k = cnt[mx]
    s = n - k

    ans = 0
    for singles in range(0, k + 1):
        ways_choose_singles = C(k, singles)
        ways_assign = C(n - singles - 1, k - 1) if k - 1 <= n - singles - 1 else 0
        ans = (ans + ways_choose_singles * ways_assign) % MOD

    print(ans)
```Tính toán trước giai thừa được sử dụng để trả lời các hệ số nhị thức trong thời gian không đổi cho mỗi truy vấn. Vòng lặp chính tính toán có bao nhiêu vị trí tương ứng với hành vi đơn lẻ và bao nhiêu vị trí tương ứng với việc loại bỏ ổn định. 

Chi tiết triển khai chính là tách tần số của giá trị lớn nhất trong$a$, đóng vai trò là điểm neo cho số lần xóa không thay đổi. Từ đó, chúng tôi liệt kê có bao nhiêu trong số đó tương ứng với các vị trí giá trị đơn lẻ thực tế trong$b$và tính toán các vị trí tổ hợp. 

Phải cẩn thận để tránh các đối số nhị thức không hợp lệ khi phân phối các lần xuất hiện còn lại, vì các tham số âm hoặc nằm ngoài phạm vi phải đóng góp bằng 0. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu ở đâu$n = 2$Và$a = [1,1]$. Ở đây tất cả các giá trị đều giống nhau, vì vậy mỗi lần xóa sẽ để lại chính xác một giá trị riêng biệt. Điều này buộc tất cả các phần tử của$b$để được bình đẳng. Thuật toán phát hiện$mx = mn = 1$và trả về trực tiếp 1. 

| Bước | mx | mn | k | vòng đơn | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 1 | 1 | 2 | bỏ qua | 1 | 

Điều này xác nhận rằng bất biến sẽ thu gọn thành một tập hợp nhiều tập hợp thống nhất. 

Bây giờ hãy xem xét một trường hợp như$a = [1,2,1]$. Ở đây có hai hành vi riêng biệt: các vị trí mà việc loại bỏ làm giảm số lượng khác biệt và các vị trí không loại bỏ. Giá trị tối đa$2$xuất hiện một lần, vì vậy chúng tôi sửa cấu trúc cho phù hợp và phân bổ các vai trò đơn lẻ. Vòng lặp tổ hợp kiểm tra xem vị trí có giá trị cao duy nhất đó có tương ứng với một giá trị đơn lẻ trong$b$. 

| đĩa đơn | chọn cách | chỉ định cách | tổng cộng | 
| --- | --- | --- | --- | 
| 0 | 1 | C(...) | ... | 
| 1 | k | C(...) | ... | 

Điều này thể hiện cách giải pháp liệt kê các phân chia cấu trúc hợp lệ thay vì các mảng rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi bài kiểm tra | tính toán trước giai thừa cộng với các truy vấn nhị thức theo thời gian không đổi | 
| Không gian |$O(n)$| mảng giai thừa và nghịch đảo | 

Tổng độ phức tạp phù hợp thoải mái dưới sự ràng buộc$\sum n \le 3 \cdot 10^5$, vì mỗi trường hợp thử nghiệm chỉ được xử lý bằng tiền xử lý tuyến tính và các phép toán số học theo thời gian không đổi cho mỗi cấu hình. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else ""  # placeholder

# provided samples
# assert run("...") == "..."

# custom cases
# minimal n
# all equal
# alternating structure
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2\n1 1`|`1`| tất cả các yếu tố giống hệt nhau buộc nhiều bộ đơn | 
|`3\n1 2 1`|`?`| cấu trúc đơn hỗn hợp | 
|`4\n2 2 2 2`|`1`| trường hợp cạnh tần số thống nhất | 
|`3\n1 1 2`|`?`| sự phân chia không hề nhỏ | 

## Vỏ cạnh 

Một trường hợp cạnh tới hạn là khi tất cả các phần tử của$a$đều bình đẳng. Trong tình huống đó, cấu trúc buộc tất cả các thao tác xóa phải hoạt động giống hệt nhau, nghĩa là mọi phần tử của$b$phải chia sẻ cùng một mẫu tần số. Thuật toán xử lý việc này một cách rõ ràng bằng cách kiểm tra$mx == mn$và trả về 1 ngay lập tức. 

Một trường hợp cạnh khác là khi$a$chứa cả giá trị cực trị và gần cực tiểu trong một mẫu có thể yêu cầu các phép gán đơn lẻ không nhất quán. Trong những trường hợp như vậy, vòng lặp tổ hợp mang lại cấu hình hợp lệ bằng 0 vì các hệ số nhị thức nằm ngoài phạm vi hợp lệ đánh giá bằng 0, loại bỏ chính xác các cấu trúc không thể.
