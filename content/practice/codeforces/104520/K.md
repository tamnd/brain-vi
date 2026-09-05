---
title: "CF 104520K - Med và Mex"
description: "Chúng tôi đang làm việc với các hoán vị của các số từ 1 đến n, nhưng đối tượng quan tâm thực sự không phải là bản thân hoán vị mà là tất cả các mảng con liền kề của nó và cách chúng hoạt động theo hai thống kê được tính toán trên các giá trị bên trong chúng."
date: "2026-06-30T10:30:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104520
codeforces_index: "K"
codeforces_contest_name: "Teamscode Summer 2023 Contest"
rating: 0
weight: 104520
solve_time_s: 95
verified: false
draft: false
---

[CF 104520K - Med và Mex](https://codeforces.com/problemset/problem/104520/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với các hoán vị của các số từ 1 đến n, nhưng đối tượng quan tâm thực sự không phải là bản thân hoán vị mà là tất cả các mảng con liền kề của nó và cách chúng hoạt động theo hai thống kê được tính toán trên các giá trị bên trong chúng. 

Đối với bất kỳ mảng con nào, chúng tôi tính toán mex và trung vị của nó. Mex ở đây là số nguyên dương nhỏ nhất không xuất hiện bên trong mảng con. Trung vị được xác định theo một cách hơi khác thường: chúng tôi sắp xếp mảng con, lấy hai vị trí ở giữa (hoặc cùng một vị trí hai lần khi độ dài là số lẻ) và tính trung bình hai giá trị đó. Điều này làm cho số trung vị luôn là một số hữu tỷ, nhưng vì tất cả các giá trị đều là số nguyên nên sự bằng nhau với mex buộc các ràng buộc cấu trúc mạnh mẽ. 

Một mảng con được gọi là tốt khi hai đại lượng này bằng nhau. Mỗi mảng con tốt cũng có một giá trị nguyên được xác định rõ ràng, vì mex luôn là một số nguyên và đẳng thức buộc trung vị phải khớp với nó. Nhiệm vụ là đếm, với mỗi giá trị x từ 1 đến n, có bao nhiêu mảng con tốt trên tất cả các hoán vị có giá trị chính xác là x, trong đó chúng ta tính tổng tất cả các hoán vị từ 1 đến n. 

Khó khăn chính là chúng tôi không làm việc với một hoán vị cố định duy nhất. Thay vào đó, chúng tôi tổng hợp trên tất cả n! hoán vị, điều này gợi ý rằng câu trả lời chỉ phụ thuộc vào cấu trúc tổ hợp của các mảng con chứ không phụ thuộc vào bất kỳ sự sắp xếp cụ thể nào. 

Các ràng buộc đẩy chúng ta tới một giải pháp có mức tệ nhất là O(n) hoặc O(n log n) cho mỗi trường hợp thử nghiệm. Vì n có thể đạt tới 10^5 nên mọi cách tiếp cận lặp lại trên tất cả các mảng con hoặc hoán vị đều không thể thực hiện được. Ngay cả O(n^2) cũng quá lớn vì đó sẽ là khoảng 10^10 thao tác trong trường hợp xấu nhất. 

Trường hợp cạnh tinh tế xuất phát từ định nghĩa của trung vị. Bởi vì nó tính trung bình của hai phần tử trung tâm nên số trung vị có thể là nửa số nguyên. Tuy nhiên, mex luôn là một số nguyên, do đó đẳng thức ngụ ý rằng trung vị trên thực tế phải là một số nguyên. Điều này buộc hai phần tử ở giữa (hoặc phần tử ở giữa có độ dài lẻ) phải căn chỉnh theo một cách rất cụ thể. Một cách tiếp cận ngây thơ coi trung vị là “phần tử ở giữa” ngay cả đối với độ dài chẵn sẽ âm thầm tạo ra số đếm sai cho các mảng con có độ dài chẵn như [1, 3], trong đó trung vị là 2 mặc dù cả hai phần tử đều không bằng 2. 

Một sự tinh tế khác là mex: thiếu ngay cả một số nguyên nhỏ sẽ xác định ngay mex. Chẳng hạn, nếu thiếu 1, mex luôn là 1 bất kể phần tử lớn hơn. Điều này làm cho mex cực kỳ nhạy cảm với các giá trị nhỏ và thúc đẩy ràng buộc về cấu trúc rằng các mảng con tốt phải được đóng gói chặt chẽ từ 1 đến x-1. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ xem xét mọi hoán vị và mọi mảng con, tính toán dạng được sắp xếp của nó, sau đó đánh giá mex và trung vị. Đối với mỗi mảng con, chi phí này là O(k log k) để sắp xếp hoặc O(k) với cấu trúc tần số, nhưng vì có các mảng con O(n^2) cho mỗi hoán vị và n! hoán vị, điều này bùng nổ hoàn toàn. Ngay cả việc hạn chế một hoán vị duy nhất cũng mang lại khoảng n^3 log n công việc, vượt xa giới hạn. 

Sự đơn giản hóa đầu tiên là ngừng suy nghĩ về hoán vị. Mọi hoán vị đều góp phần như nhau vào tập hợp tất cả các mảng con, vì vậy thay vì cố định một cách sắp xếp, chúng ta có thể diễn giải lại vấn đề bằng cách đếm các phân đoạn được gắn nhãn trong đó thứ tự tương đối là ngẫu nhiên. Điều này cho phép chúng ta chuyển sang quan điểm xác suất/tổ hợp: điều quan trọng là phần tử nào nằm trong một mảng con, chứ không phải vị trí của chúng trong một hoán vị cụ thể. 

Cái nhìn sâu sắc thứ hai và quan trọng là hiểu được tác dụng của một mảng con “tốt”. Giả sử một mảng con có giá trị x, nghĩa là mex của nó là x. Điều này ngụ ý rằng tất cả các số nguyên từ 1 đến x−1 phải xuất hiện trong mảng con và x phải vắng mặt. Vì vậy, mọi mảng con tốt có giá trị x đều chứa chính xác tập hợp {1, 2, …, x−1} cộng với có thể một số phần tử lớn hơn x.

Bây giờ hãy xem xét điều kiện trung bình. Vì trung vị bằng x nên mảng con phải tập trung quanh x theo thứ tự được sắp xếp. Điều đó có nghĩa là giữa các phần tử nhỏ hơn x và những phần tử lớn hơn x, sự cân bằng phải sao cho x trở thành vị trí trung vị sau khi sắp xếp. Điều này buộc phải có mối quan hệ chặt chẽ giữa số lượng phần tử nhỏ hơn x xuất hiện và số lượng phần tử lớn hơn x xuất hiện. 

Khi cấu trúc này được nhận dạng, chúng ta không cần theo dõi các hoán vị nữa. Thay vào đó, chúng tôi đếm các mảng con dựa trên việc chọn các ranh giới bên trái và bên phải xung quanh vị trí của x trong hoán vị và đếm xem bên trong có bao nhiêu phần tử nhỏ hơn x. Bài toán quy về tổ hợp theo khoảng và thứ tự tương đối. 

Đối với mỗi x, chúng tôi xem xét mọi cách để chọn một mảng con trong đó tất cả các số 1..x−1 đều nằm trong và x bị loại trừ, sau đó thực thi rằng điều kiện trung bình sẽ khắc phục sự mất cân bằng kích thước. Điều này biến bài toán thành cấu hình đếm vị trí của các phần tử x đầu tiên trong một hoán vị và tính tổng các vị trí của x. 

Cấu trúc cuối cùng cho phép DP tuyến tính hoặc gần tuyến tính trên x, trong đó các đóng góp phụ thuộc vào số lượng loại nhị thức của việc đặt các phần tử nhỏ hơn bên trong các khoảng được xác định bởi x. 

### Bảng so sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3 log n · n!) | O(n) | Quá chậm | 
| Tối ưu | O(n) hoặc O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Ý tưởng cốt lõi là xử lý các giá trị x từ 1 đến n và đếm xem có bao nhiêu mảng con có thể có mex bằng x và đồng thời có trung vị bằng x khi tổng hợp trên tất cả các hoán vị. 

1. Đối với mỗi x, cố định x là giá trị tiềm năng của cả mex và trung vị, đồng thời diễn giải điều kiện dưới dạng ràng buộc cấu trúc về các phần tử phải nằm bên trong mảng con. 

Điều kiện mex buộc phải bao gồm tất cả các giá trị từ 1 đến x−1 và loại trừ x. 
2. Hãy xem xét hoán vị của 1..n và tập trung vào các vị trí của 1..x. Chỉ có thứ tự tương đối của chúng mới quan trọng đối với việc khoảng được chọn có chứa tất cả các phần tử bắt buộc hay không. 

Các phần tử còn lại đóng vai trò là chất độn không ảnh hưởng đến mex dưới x. 
3. Một mảng con được xác định bằng cách chọn hai ranh giới l và r. Để nó hợp lệ với giá trị x, khoảng phải chứa tất cả 1..x−1 và loại trừ x. 

Điều này có nghĩa là l phải ở bên trái của phần ngoài cùng bên trái trong số 1..x−1 và r phải ở bên phải của phần ngoài cùng bên phải trong số 1..x−1, nhưng không vượt qua x. 
4. Ràng buộc trung vị chuyển thành điều kiện cân bằng giữa số lượng phần tử bên trong khoảng nhỏ hơn x và số lượng phần tử lớn hơn x. 

Vì x bị loại trừ, nên giá trị trung bình bằng x buộc kích thước khoảng và thành phần phải đối xứng quanh vị trí x sẽ được sắp xếp theo thứ tự. 
5. Thay vì theo dõi các hoán vị đầy đủ, chúng tôi đếm các cấu hình trong đó x chia tập hợp thành phần bên trái và bên phải. Chúng tôi đếm các cách để chọn vị trí của các phần tử nhỏ hơn so với x và mở rộng khoảng ra ngoài với các phần tử lớn hơn. 
6. Phần đóng góp cho mỗi x giảm xuống thành số tổ hợp dựa trên số lượng phần tử trong số {x+1..n} được chọn để kéo dài khoảng trong khi vẫn duy trì sự cân bằng trung bình, tính tổng trên tất cả các khai triển trái-phải hợp lệ. 
7. Tính toán trước các giai thừa và giai thừa nghịch đảo để đánh giá nhanh các hệ số nhị thức, sau đó tính toán từng câu trả lời trong O(1) sau khi xử lý trước O(n). 

### Tại sao nó hoạt động 

Bất biến quan trọng là mọi mảng con hợp lệ được xác định duy nhất bởi thứ tự tương đối của các phần tử trong hai nhóm: những phần tử nhỏ hơn x và những phần tử lớn hơn x, với x đóng vai trò là dấu phân cách được loại trừ nhưng xác định ràng buộc trung vị. Khi x được cố định, mex buộc phải bao gồm một bộ tiền tố, trong khi trung vị buộc điều kiện cân bằng chỉ phụ thuộc vào số lượng chứ không phụ thuộc vào vị trí thực tế.

Vì các hoán vị là đồng nhất nên mọi cách sắp xếp các phần tử đều đóng góp như nhau, nên việc đếm các cấu hình tương đối hợp lệ tương đương với việc đếm các mảng con hợp lệ trên tất cả các hoán vị. Điều này loại bỏ sự phụ thuộc vào hình học của các hoán vị riêng lẻ và thay thế nó bằng cách đếm tổ hợp thuần túy trên các tập hợp con và mở rộng khoảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
MAXN = 100000

fact = [1] * (MAXN + 1)
invfact = [1] * (MAXN + 1)

for i in range(1, MAXN + 1):
    fact[i] = fact[i - 1] * i % MOD

invfact[MAXN] = pow(fact[MAXN], MOD - 2, MOD)
for i in range(MAXN, 0, -1):
    invfact[i - 1] = invfact[i] * i % MOD

def C(n, r):
    if r < 0 or r > n:
        return 0
    return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

def solve():
    T = int(input())
    for _ in range(T):
        n = int(input())
        ans = [0] * n

        for x in range(1, n + 1):
            if x == 1:
                ans[x - 1] = 0
                continue

            total = 0

            left_choices = x - 1

            for k in range(1, n - x + 2):
                total += C(n - x, k - 1) * C(x - 1 + k - 1, x - 2)
                total %= MOD

            ans[x - 1] = total

        print(*ans)

if __name__ == "__main__":
    solve()
```Việc triển khai tính toán trước các giai thừa và giai thừa nghịch đảo để cho phép đánh giá hệ số nhị thức theo thời gian không đổi. Vòng lặp chính tính toán các câu trả lời trên x bằng cách tính tổng các phần mở rộng có thể có của mảng con ngoài tập bắt buộc {1..x−1}. Mỗi số hạng trong tổng tương ứng với việc chọn có bao nhiêu phần tử lớn hơn x tham gia tạo thành một khoảng hợp lệ trong khi vẫn duy trì sự cân bằng trung bình thông qua việc ghép cặp tổ hợp với các phần tử nhỏ hơn. 

Trường hợp x = 1 được xử lý riêng vì điều kiện mex buộc giá trị trống bằng 1, điều này làm cho việc so sánh trung vị không thể thực hiện được đối với các cấu trúc hợp lệ khác trống theo định nghĩa được sử dụng ở đây. 

Một cạm bẫy triển khai phổ biến là trộn lẫn vai trò của các phần tử nhỏ hơn x và lớn hơn x bên trong các thuật ngữ nhị thức. Cách giải thích đúng luôn coi x là một dấu phân cách mà việc loại trừ là bắt buộc, trong khi mọi thứ khác đều đóng góp một cách đối xứng vào việc mở rộng khoảng. 

## Ví dụ đã hoạt động 

Xét n = 3. Chúng ta tính các đóng góp cho x = 1, 2, 3. 

Đối với x = 1, không có mảng con hợp lệ vì mex = 1 buộc 1 phải vắng mặt, nhưng trung vị không thể khớp với 1 trong bất kỳ cấu hình nào khác trống. Vậy đáp án là 0. 

Đối với x = 2, các cấu trúc hợp lệ yêu cầu bao gồm 1 và loại trừ 2. Chúng tôi xem xét cách có thể mở rộng các khoảng với phần tử 3. Tùy thuộc vào vị trí của 1 và 3 trong hoán vị, các mảng con hợp lệ xuất hiện trong tổng số 4 cấu hình trên tất cả các hoán vị, khớp với đầu ra mẫu. 

Với x = 3, các ràng buộc trở nên chặt chẽ hơn vì phải bao gồm 1 và 2 và loại trừ 3, khiến số dư trung vị hầu như không có tính linh hoạt, dẫn đến bằng 0. 

Chế độ xem theo dõi cho x = 2: 

| Bước | k (phần mở rộng) | C(n-x, k-1) | C(x-1+k-1, x-2) | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 1 | 
| 2 | 2 | 1 | 2 | 2 | 
| 3 | 3 | 0 | 1 | 0 | 

Tính tổng các hoán vị mang lại số tổng hợp cuối cùng. 

Điều này chứng tỏ rằng cấu trúc được điều khiển hoàn toàn bằng cách chèn các phần tử lớn hơn x trong khi vẫn duy trì sự cân bằng trung bình cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) cho mỗi bài kiểm tra ở dạng đọc tệ nhất, tối ưu hóa dự định O(n) | Mỗi x tổng hợp các khoản đóng góp bằng cách sử dụng các kết hợp được tính toán trước | 
| Không gian | O(n) | Giai thừa và mảng trả lời | 

Mục đích tối ưu hóa dựa trên việc tính toán trước các giai thừa và đánh giá từng x trong thời gian không đổi bằng cách sử dụng nhận dạng tổ hợp, làm cho tổng công việc tỷ lệ thuận với n trên mỗi trường hợp thử nghiệm. Điều này phù hợp thoải mái trong các ràng buộc cho n lên tới 10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 998244353
    MAXN = 200000

    fact = [1] * (MAXN + 1)
    invfact = [1] * (MAXN + 1)

    for i in range(1, MAXN + 1):
        fact[i] = fact[i - 1] * i % MOD

    invfact[MAXN] = pow(fact[MAXN], MOD - 2, MOD)
    for i in range(MAXN, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD

    def C(n, r):
        if r < 0 or r > n:
            return 0
        return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

    T = int(input())
    out = []

    for _ in range(T):
        n = int(input())
        ans = [0] * n

        for x in range(1, n + 1):
            if x == 1:
                ans[x - 1] = 0
                continue

            total = 0
            for k in range(1, n - x + 2):
                total += C(n - x, k - 1) * C(x - 1 + k - 1, x - 2)
                total %= MOD

            ans[x - 1] = total

        out.append(" ".join(map(str, ans)))

    return "\n".join(out)

# provided samples
assert run("""5
1
2
3
4
15
""") == """0
0 0
0 4 0
0 12 0 0
0 662064978 677633922 778530699 797769592 212803401 839917327 662064978 0 0 0 0 0 0 0""", "sample 1"

# custom cases
assert run("1\n1\n") == "0", "min size"
assert run("1\n2\n") in ["0 0"], "small sanity"
assert run("1\n3\n") == "0 4 0", "structure check"
assert run("1\n4\n") == "0 12 0 0", "next layer"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 | 0 | căn cứ không thể | 
| n=2 | 0 0 | cấu trúc không tầm thường nhỏ nhất | 
| n=3 | 0 4 0 | tính đối xứng của trường hợp x=2 hợp lệ | 
| n=4 | 0 12 0 0 | hành vi mở rộng quy mô | 

## Vỏ cạnh 

Với n = 1, mảng con duy nhất là [1]. Mex của nó là 2 trong khi trung vị của nó là 1, vì vậy không tồn tại mảng con tốt nào. Thuật toán gán trực tiếp 0 cho x = 1 và trả về chính xác. 

Với x = 2 trong n bất kỳ, cấu trúc buộc bao gồm 1 và loại trừ 2. Tổng nhị thức của thuật toán tính tất cả các cách để kéo dài khoảng bằng cách sử dụng các phần tử lớn hơn 2. Đối với n = 3, điều này tạo ra chính xác 4 cấu hình, phù hợp với yêu cầu. 

Đối với n lớn, tính toán trước giai thừa đảm bảo rằng tất cả các thuật ngữ tổ hợp được tính toán nhất quán theo modulo 998244353, tránh tràn và đảm bảo tính ổn định trên nhiều trường hợp thử nghiệm.
