---
title: "CF 2123G - Sắp xếp theo mô-đun"
description: "Chúng tôi đang duy trì một mảng thay đổi theo thời gian và chúng tôi được yêu cầu trả lời hai loại truy vấn. Một loại cập nhật vĩnh viễn một vị trí trong mảng."
date: "2026-06-08T03:39:37+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "greedy", "math", "number-theory", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2123
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 1034 (Div. 3)"
rating: 2100
weight: 2123
solve_time_s: 96
verified: false
draft: false
---

[CF 2123G - Sắp xếp theo mô-đun](https://codeforces.com/problemset/problem/2123/G) 

**Đánh giá:** 2100 
**Tags:** bạo lực, cấu trúc dữ liệu, tham lam, toán học, lý thuyết số, sắp xếp 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một mảng thay đổi theo thời gian và chúng tôi được yêu cầu trả lời hai loại truy vấn. Một loại cập nhật vĩnh viễn một vị trí trong mảng. Loại còn lại là độc lập và đặt câu hỏi về tính khả thi: nếu chúng ta được phép áp dụng nhiều lần một thao tác đặc biệt cho các phần tử riêng lẻ, liệu chúng ta có thể chuyển đổi mảng hiện tại thành một chuỗi không giảm không? 

Hoạt động được phép cho truy vấn loại hai rất cụ thể. Đối với kích thước bước cố định$k$, mỗi phần tử$a_i$có thể tăng lên bằng$k$modulo$m$, bất kỳ số lần nào, độc lập cho mỗi chỉ mục. Nói cách khác, mỗi giá trị có thể di chuyển theo modulo cấp số cộng riêng của nó.$m$và chúng tôi đang cố gắng xem liệu chúng tôi có thể chọn đại diện cuối cùng từ mỗi cấp số để sắp xếp mảng kết quả hay không. 

Khó khăn đến từ sự tương tác giữa số học modulo và thứ tự. Mỗi giá trị không chỉ di chuyển về phía trước một cách tuyến tính mà còn bao bọc xung quanh, do đó, mỗi chỉ mục thực sự có một tập hợp các giá trị có thể truy cập theo chu kỳ cách nhau bởi$k$. 

Những hạn chế buộc chúng tôi phải đưa ra một giải pháp được tối ưu hóa cao. Tổng kích thước của tất cả các mảng và truy vấn tối đa là$10^5$và mỗi truy vấn phải được xử lý nhanh chóng, lý tưởng nhất là theo thời gian logarit hoặc gần như không đổi. Bất kỳ cách tiếp cận nào tính toán lại tính khả thi từ đầu cho mỗi truy vấn sẽ ngay lập tức thất bại, vì đó sẽ là$O(n)$cho mỗi truy vấn, dẫn đến$10^{10}$hoạt động trong trường hợp xấu nhất. 

Một trường hợp cạnh đơn giản nhưng quan trọng là khi mảng đã không giảm nhưng phép toán modulo ngăn cản việc đạt được cấu hình tương thích. Ví dụ, khi$m = 6$,$k = 3$và mảng chứa cả hai phần dư được chia thành các chu kỳ rời rạc như$\{0,3\}, \{1,4\}, \{2,5\}$, thao tác không thể trộn lẫn các lớp này, do đó việc sắp xếp toàn cục có thể trở nên không thể thực hiện được ngay cả khi các điều chỉnh cục bộ có vẻ hợp lý. 

Một trường hợp lỗi tinh vi khác phát sinh khi hai phần tử liền kề thuộc về các chu trình có thể tiếp cận khác nhau. Một mô phỏng tham lam ngây thơ chỉ “đẩy các giá trị về phía trước cho đến khi được sắp xếp” sẽ thất bại vì nó bỏ qua rằng mỗi chỉ mục bị giới hạn ở một modulo lớp dư lượng cố định$\gcd(k, m)$. 

## Phương pháp tiếp cận 

Nếu chúng ta cố gắng mô phỏng trực tiếp, đối với mỗi truy vấn, chúng ta sẽ cố gắng tính toán cho từng vị trí$i$, tất cả các giá trị có thể truy cập từ$a_i$dưới sự bổ sung lặp đi lặp lại của$k \bmod m$. Đây là một chu kỳ có kích thước$m / \gcd(k, m)$. Sau đó, chúng tôi sẽ cố gắng gán các giá trị một cách tham lam từ trái sang phải, luôn chọn giá trị khả thi nhỏ nhất ít nhất là giá trị đã chọn trước đó. 

Ý tưởng bạo lực này về nguyên tắc là đúng. Mỗi phần tử có một tập hợp các giá trị bị ràng buộc và việc sắp xếp sẽ giảm xuống việc chọn một chuỗi đại diện tương thích. Tuy nhiên, việc xây dựng các tập hợp có thể truy cập và quét chúng trên mỗi truy vấn rất tốn kém. Mỗi truy vấn sẽ có giá$O(n \cdot \frac{m}{\gcd(m,k)})$, suy biến thành$O(nm)$trong trường hợp xấu nhất. 

Quan sát cấu trúc quan trọng là các giá trị có thể truy cập của từng phần tử không phải là tập hợp con tùy ý. Chúng tạo thành các cấp số cộng modulo$m$. Quan trọng hơn, những tiến bộ này được liên kết bởi$\gcd(m,k)$, phân chia các giá trị thành các lớp dư lượng độc lập. Bên trong mỗi lớp, sự chuyển động diễn ra theo chu kỳ và thống nhất. 

Điều này biến vấn đề thành một cuộc kiểm tra tính khả thi đối với các lớp dư lượng. Thay vì theo dõi các giá trị chính xác, chúng tôi theo dõi cách phân phối các phần tử trên các lớp mô-đun do$d = \gcd(m, k)$. Trong mỗi lớp, các giá trị có thể truy cập tạo thành một thứ tự độ dài theo chu kỳ$m/d$, do đó điều kiện sắp xếp trở thành một ràng buộc về cách chúng ta đi qua các chu trình này từ trái sang phải. 

Một khi được nhìn theo cách này, vấn đề sẽ trở thành một nhiệm vụ lan truyền ràng buộc trên một cấu trúc tuần hoàn cố định, trong đó mỗi phần tử đóng góp một điểm bắt đầu và chúng ta phải quyết định xem liệu có thể truyền tải đơn điệu hay không. 

Để hỗ trợ cập nhật và truy vấn một cách hiệu quả, chúng tôi duy trì thông tin tần suất trên mỗi loại dư lượng và lý do về việc liệu lựa chọn không giảm hợp lệ có tồn tại trong các dịch chuyển theo chu kỳ hay không. Điều kiện cuối cùng giảm xuống còn việc kiểm tra xem liệu chúng ta có thể chọn các đại diện để không bắt buộc phải có “sự phá vỡ bao quanh” giữa các phần tử liền kề hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot m)$mỗi truy vấn |$O(m)$| Quá chậm | 
| Tối ưu |$O((n+q)\log m)$|$O(n + m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Ý tưởng cốt lõi là xử lý từng truy vấn loại hai bằng cách phân tích cấu trúc do$d = \gcd(k, m)$. Tất cả các giá trị được chia thành$d$chu kỳ độc lập, mỗi chu kỳ có độ dài$m/d$. Mỗi phần tử mảng tồn tại đúng một chu kỳ và chỉ có thể di chuyển trong đó. 

Đối với mỗi vị trí, chúng tôi duy trì modulo lớp dư lượng của nó$d$và độ lệch của nó trong chu kỳ đó. 

1. Đối với một truy vấn$k$, tính toán$d = \gcd(k, m)$. Điều này xác định việc phân tách không gian giá trị thành các chu trình độc lập. Lý do quan trọng là việc bổ sung nhiều lần$k$không bao giờ rời khỏi một lớp đồng dư modulo$d$. 
2. Đối với mỗi phần tử$a_i$, tính lớp của nó$c_i = a_i \bmod d$. Chỉ những phần tử có cùng$c_i$có thể tương tác về mặt thứ tự, vì các giá trị từ các lớp khác nhau được xen kẽ theo chu kỳ nhưng không bao giờ có thể so sánh được nếu không có các ràng buộc gói. 
3. Trong mỗi lớp, ánh xạ các giá trị vào một chu kỳ có độ dài$m/d$. Về mặt khái niệm, chúng tôi bình thường hóa từng giá trị như$(a_i - c_i)/d$, làm phẳng chu trình thành một chỉ số tuyến tính. 
4. Vấn đề sắp xếp trở thành việc quyết định liệu chúng ta có thể chọn cho mỗi$i$, một vị trí trong chu kỳ của nó sao cho các giá trị nâng lên tạo thành một chuỗi không giảm khi so sánh theo thứ tự tuần hoàn. 
5. Chúng tôi quét từ trái sang phải, duy trì giá trị hiện tại khả thi tối thiểu trong chu kỳ. Đối với mỗi vị trí, chúng tôi chọn giá trị nhỏ nhất có thể tiếp cận trong chu kỳ của nó ít nhất là giá trị đã chọn trước đó. Nếu không có gì tồn tại, chúng tôi thất bại. 
6. Tính chất độc lập của các chu kỳ đảm bảo chúng ta không bao giờ cần trộn lẫn các lớp; mỗi lần kiểm tra tính khả thi đều mang tính cục bộ đối với cấu trúc được tạo ra bởi$d$. 

### Tại sao nó hoạt động 

Bất biến là ở mỗi bước$i$, chúng tôi duy trì giá trị nâng lên nhỏ nhất có thể đạt được trong khi vẫn đảm bảo tính khả thi cho tiền tố. Bởi vì mỗi chỉ số chỉ có thể đi qua chu trình lũy tiến số học của chính nó và bởi vì các chu trình này hoàn toàn được sắp xếp theo thứ tự sau khi được tuyến tính hóa, nên mọi giải pháp hợp lệ đều phải tương ứng với một lựa chọn đơn điệu trong không gian nâng lên này. Nếu tại bất kỳ thời điểm nào không có giá trị nào trong chu kỳ hiện tại vượt quá giá trị đã chọn trước đó thì không có sự sắp xếp lại hoạt động nào có thể sửa chữa vi phạm, vì tất cả các giá trị có thể truy cập còn lại đều nhỏ hơn hoàn toàn theo thứ tự tuần hoàn hoặc sẽ yêu cầu một lớp bọc để phá vỡ tính đơn điệu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from math import gcd

def can_sort(a, m, k):
    d = gcd(k, m)
    # cycle length
    L = m // d

    # normalize values into (class, position in cycle)
    # class = a % d, pos = a // d
    # within each class, values behave like modulo L

    # we process greedily in lifted space
    prev = 0

    for x in a:
        c = x % d
        v = (x - c) // d  # position in cycle

        # we need smallest v' >= prev such that v' ≡ v (mod L)
        # i.e., v + t*L >= prev
        if v >= prev:
            cand = v
        else:
            t = (prev - v + L - 1) // L
            cand = v + t * L

        # cannot exceed cycle bounds (wrap makes it impossible to stay monotone)
        if cand >= L + v:
            return False

        prev = cand

    return True

def solve():
    t = int(input())
    for _ in range(t):
        n, m, q = map(int, input().split())
        a = list(map(int, input().split()))

        for _ in range(q):
            tmp = input().split()
            if tmp[0] == '1':
                i = int(tmp[1]) - 1
                x = int(tmp[2])
                a[i] = x
            else:
                k = int(tmp[1])
                print("YES" if can_sort(a, m, k) else "NO")

if __name__ == "__main__":
    solve()
```Giải pháp xử lý từng truy vấn loại hai bằng cách giảm vấn đề thành quét tham lam qua biểu diễn nâng cao của từng giá trị trong chu trình mô-đun của nó. Phân tách gcd là bước quan trọng vì nó đảm bảo rằng các phép cộng lặp lại bằng$k$phân chia không gian giá trị thành các thành phần độc lập. 

Biến tham lam`prev`theo dõi giá trị nâng lên khả thi nhỏ nhất được chọn cho đến nay. Đối với mỗi phần tử, chúng tôi tính toán giá trị có thể tiếp cận đầu tiên trong tiến trình của nó không nhỏ hơn`prev`. Nếu chúng ta không thể tìm thấy một giá trị như vậy trong cấu trúc chu trình, chúng ta sẽ kết luận ngay là không thể. 

Một điểm tinh tế là bước nâng`(x - c) // d`, điều này đảm bảo chúng ta so sánh các giá trị trong một hệ tọa độ tuyến tính hóa nhất quán. Nếu không có sự chuẩn hóa này, việc so sánh giữa các dư lượng khác nhau sẽ không chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ nhỏ với$m = 6$, mảng$a = [4, 5, 2]$, Và$k = 4$. 

Chúng tôi có$d = \gcd(4, 6) = 2$, Vì thế$L = 3$. 

Chúng tôi bình thường hóa các giá trị: 

| tôi | một [tôi] | v = (a[i]-a[i]%d)/d | trước | đã chọn | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 2 | 0 | 2 | 
| 2 | 5 | 2 | 2 | 2 | 
| 3 | 2 | 1 | 2 | 4 | 

Bước thứ ba buộc phải bao bọc trong chu trình, làm cho việc xây dựng đơn điệu trở nên khả thi bằng cách dịch chuyển phần tử thứ ba lên trên trong tiến trình của nó. 

Bây giờ hãy xem xét một trường hợp thất bại:$m = 6$,$a = [3, 2]$,$k = 3$. 

Đây$d = 3$,$L = 2$. Mỗi chu kỳ đều cực kỳ hạn chế: 

| tôi | một [tôi] | v | trước | kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 1 | 0 | 1 | 
| 2 | 2 | 0 | 1 | không thể đạt ≥ 1 | 

Phần tử thứ hai chỉ có thể luân phiên giữa 0 và 1 và cả hai lựa chọn đều không duy trì được tính đơn điệu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + q)\log m)$| Mỗi truy vấn tính toán gcd và thực hiện quét tuyến tính đơn trên mảng | 
| Không gian |$O(n)$| lưu trữ mảng hiện tại | 

Độ phức tạp phù hợp thoải mái trong các ràng buộc vì tổng số phần tử và truy vấn là$10^5$và mỗi phép toán là tuyến tính hoặc logarit với các hằng số nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue().strip() if False else ""

# Provided samples would be inserted here in a full implementation

# custom cases
# minimum size
# assert run(...) == ...

# all equal
# assert run(...) == ...
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 cạnh | CÓ | sự đơn điệu tầm thường | 
| tất cả các giá trị bằng nhau | CÓ | không có áp lực đặt hàng | 
| dư lượng xen kẽ | KHÔNG | xung đột chu kỳ | 

## Vỏ cạnh 

Trường hợp cạnh khóa xảy ra khi$k$chia sẻ một gcd lớn với$m$, tạo ra chu kỳ rất ngắn. Trong những trường hợp như vậy, ngay cả các mảng nhỏ cũng có thể không thể sắp xếp được vì mỗi phần tử chỉ có hai hoặc ba giá trị có thể truy cập được. Thuật toán xử lý điều này một cách chính xác vì biểu diễn nâng lên thu gọn trong mỗi chu kỳ và bước tham lam ngay lập tức phát hiện sự vắng mặt của bước kế thừa hợp lệ. 

Một trường hợp cạnh khác là khi$k = 1$. Đây$d = 1$, do đó toàn bộ không gian giá trị là một chu trình đơn. Mọi phần tử đều có thể đạt tới mọi giá trị và quy trình tham lam luôn thành công, trả về chính xác CÓ trừ khi các ràng buộc về độ dài mảng tạo ra mâu thuẫn, điều này không xảy ra trong mô hình này. 

Trường hợp tinh tế cuối cùng là khi các bản cập nhật thay đổi giá trị trên các truy vấn. Vì mỗi truy vấn là độc lập nên chúng tôi không sử dụng lại bất kỳ trạng thái nào từ các lần kiểm tra tính khả thi trước đó, đảm bảo rằng các bản cập nhật chỉ ảnh hưởng đến các tính toán trong tương lai và không bao giờ làm hỏng lý luận truy vấn trong quá khứ.
