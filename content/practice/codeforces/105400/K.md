---
title: "CF 105400K - Hoán đổi mạnh mẽ (Phiên bản cứng)"
description: "Chúng tôi được cung cấp một mảng và được hỏi liệu nó có thể được chuyển đổi thành một mảng được sắp xếp bằng cách sử dụng các phép hoán đổi liền kề hay không, nhưng với một ràng buộc khiến việc hoán đổi ngày càng khó hơn khi quá trình tiếp tục. Hoạt động này không phải là một trao đổi tiêu chuẩn."
date: "2026-06-22T14:14:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105400
codeforces_index: "K"
codeforces_contest_name: "Fall 2024 Cupertino Informatics Tournament"
rating: 0
weight: 105400
solve_time_s: 81
verified: false
draft: false
---

[CF 105400K - Hoán đổi mạnh mẽ (Phiên bản cứng)](https://codeforces.com/problemset/problem/105400/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng và được hỏi liệu nó có thể được chuyển đổi thành một mảng được sắp xếp bằng cách sử dụng các phép hoán đổi liền kề hay không, nhưng với một ràng buộc khiến việc hoán đổi ngày càng khó hơn khi quá trình tiếp tục. 

Hoạt động này không phải là một trao đổi tiêu chuẩn. Tại lần hoán đổi thứ j trong toàn bộ quá trình, chúng ta chỉ có thể hoán đổi các phần tử ở vị trí i và i+1 nếu giá trị nhỏ hơn trong hai giá trị ít nhất là j. Điều này có nghĩa là các giao dịch hoán đổi sớm được cho phép, nhưng các giao dịch hoán đổi sau đó yêu cầu cả hai giá trị tham gia phải ngày càng lớn. Số lần hoán đổi mang tính toàn cầu trên toàn bộ chuỗi chứ không phải trên mỗi vị trí, do đó, mỗi lần hoán đổi sẽ tiêu tốn một đơn vị “mức cấp phép”. 

Nhiệm vụ là quyết định xem có tồn tại chuỗi hoán đổi nào sắp xếp mảng theo thứ tự không giảm hay không. 

Ràng buộc n lên tới 100000 ngay lập tức loại trừ mọi mô phỏng trình tự hoán đổi. Ngay cả một quy trình giống như sắp xếp bong bóng tối ưu cũng sẽ yêu cầu các giao dịch hoán đổi O(n^2) tiềm năng và tính hợp lệ của mỗi giao dịch hoán đổi phụ thuộc vào vị trí của nó trong thứ tự hoạt động chung, điều này kết hợp thời gian và trạng thái theo cách khiến mô phỏng trực tiếp không thể thực hiện được. 

Một vấn đề tế nhị phát sinh từ chỉ số hoán đổi toàn cầu. Một cách giải thích ngây thơ có thể cho rằng mỗi vị trí có ngưỡng riêng không chính xác, nhưng hoán đổi thứ j áp dụng cho toàn bộ lịch sử quá trình. Điều này phá vỡ tính cục bộ và vô hiệu hóa lý luận bong bóng tham lam tiêu chuẩn. 

Một ví dụ nhỏ cho thấy sự nguy hiểm là một mảng trong đó các phần tử nhỏ phải vượt qua nhiều phần tử lớn hơn. Ngay cả khi tại địa phương mỗi giao dịch hoán đổi có vẻ hợp lệ thì chỉ số hoán đổi tích lũy cuối cùng sẽ trở nên quá lớn, khiến cho các giao dịch hoán đổi bắt buộc trong tương lai là không thể thực hiện được. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là mô phỏng sắp xếp bong bóng trong khi kiểm tra tính hợp lệ của hoán đổi. Chúng tôi liên tục quét mảng, hoán đổi các đảo ngược liền kề khi được phép. Mỗi lần hoán đổi sẽ tăng một bộ đếm toàn cục j và chúng tôi xác minh min(a[i], a[i+1]) ≥ j. 

Về nguyên tắc, điều này đúng vì bất kỳ chuỗi hoán đổi hợp lệ nào cũng tương ứng với một số chuỗi nghịch đảo liền kề đang được giải quyết. Tuy nhiên, trong trường hợp xấu nhất, việc sắp xếp có thể thực hiện hoán đổi O(n^2) và mỗi bước liên quan đến việc quét hoặc duy trì cấu trúc, tạo ra tổng chi phí là O(n^2). Với n lên tới 100000, điều này vượt xa giới hạn khả thi. 

Quan sát quan trọng là đảo ngược quan điểm. Thay vì nghĩ về hoán đổi, chúng tôi nghĩ về cách các yếu tố phải di chuyển dưới sự ràng buộc ngày càng tăng trên toàn cầu về chỉ số hoán đổi. Hạn chế quan trọng là mỗi khi chúng ta thực hiện hoán đổi, giá trị nhỏ hơn trong hai giá trị được hoán đổi ít nhất phải bằng số hoán đổi j. Điều này có nghĩa là các giá trị nhỏ phải xuất hiện sớm trong chuỗi hoán đổi; nếu không chúng sẽ trở nên không sử dụng được khi j phát triển. 

Bây giờ hãy xem xét quá trình từ góc độ sắp xếp. Các yếu tố nhỏ không thể tham gia vào nhiều giao dịch hoán đổi, bởi vì mỗi giao dịch hoán đổi liên quan đến chúng sẽ tiêu tốn một đơn vị ngân sách toàn cầu. Vì vậy, mỗi phần tử có một “ngân sách chuyển động” tự nhiên gắn liền với giá trị của nó: nếu một phần tử có giá trị x, nó không thể tham gia hoán đổi ngoài thời gian x. Điều này biến vấn đề thành việc kiểm tra xem mỗi phần tử có thể đạt đến vị trí đã sắp xếp của nó mà không bị buộc phải hoán đổi quá nhiều hay không. 

Chúng ta có thể mô hình hóa điều này bằng cách mô phỏng thứ tự sắp xếp cuối cùng và theo dõi xem mỗi phần tử phải di chuyển bao xa. Một phép biến đổi quan trọng cần lưu ý rằng chỉ những phần tử có giá trị ≥ giới hạn độ sâu hoán đổi của chúng mới có thể tham gia vào các phép đảo ngược cần thiết. Điều này dẫn đến việc kiểm tra tính khả thi tham lam bằng cách sử dụng cây Fenwick hoặc cách đếm kiểu đảo ngược, nhưng có thêm ràng buộc về số lượng giao dịch hoán đổi hiệu quả mà mỗi giá trị có thể “đảm bảo được”. 

Một cách đơn giản hơn để xem nó là xử lý mảng từ trái sang phải trong khi vẫn duy trì số lần hoán đổi cần thiết để đưa từng phần tử về đúng vị trí của nó và đảm bảo rằng số lần hoán đổi bắt buộc này không bao giờ vượt quá giá trị của phần tử. 

Điều này chuyển đổi vấn đề thành một kiểm tra tính khả thi về khoảng cách đảo ngược bị ràng buộc.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng bong bóng Brute Force | O(n^2) | O(1)-O(n) | Quá chậm | 
| Tính khả thi tham lam với tính năng theo dõi đảo ngược | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải thích vấn đề là kiểm tra xem mỗi phần tử có thể “đủ khả năng” số lần hoán đổi mà nó phải tham gia để đạt được vị trí đã sắp xếp hay không. 

1. Xây dựng phiên bản đã sắp xếp của mảng và ánh xạ từng giá trị đến vị trí cuối cùng theo thứ tự được sắp xếp. Điều này cho chúng ta biết mọi sự việc đều phải kết thúc ở đâu. 
2. Duyệt mảng trong khi vẫn duy trì cấu trúc cho phép chúng ta tính toán có bao nhiêu phần tử hiện được định vị trước khi nó lớn hơn hoặc nhỏ hơn về vị trí cuối cùng. 
3. Đối với mỗi phần tử, hãy tính xem nó đóng góp bao nhiêu phép nghịch đảo so với thứ tự đã sắp xếp. Điều này thể hiện số lượng giao dịch hoán đổi mà nó phải tham gia một cách hiệu quả. 
4. Kiểm tra xem yêu cầu đảo ngược này đối với từng phần tử có vượt quá giá trị của nó hay không. Nếu đúng như vậy, phần tử sẽ bị buộc phải hoán đổi tại thời điểm j lớn hơn giá trị của nó, vi phạm ràng buộc tối thiểu. 
5. Nếu tất cả các phần tử đều thỏa mãn điều kiện khả thi này, ghi CÓ, nếu không thì ghi KHÔNG. 

Ý tưởng chính là mỗi lần đảo ngược tương ứng với một lần hoán đổi cuối cùng phải xảy ra và chỉ số hoán đổi toàn cầu tăng trưởng đều đặn. Vì một phần tử có giá trị x không thể tồn tại trong các giao dịch hoán đổi vượt quá x, nên bất kỳ tải đảo ngược nào lớn hơn x đều có nghĩa là không thể thực hiện được. 

### Tại sao nó hoạt động 

Quá trình hoán đổi liền kề sẽ biến đổi mảng bằng cách giải quyết từng đảo ngược một. Mỗi độ phân giải đảo ngược tương ứng với ít nhất một lần hoán đổi liên quan đến phần tử nhỏ hơn trong đảo ngược đó. Bởi vì số hoán đổi j phải thỏa mãn j ≤ min(a[i], a[i+1]), phần tử nhỏ nhất trong bất kỳ nghịch đảo nào đều giới hạn độ trễ mà nghịch đảo đó có thể được giải quyết. 

Do đó, mọi phần tử đều đặt ra một giới hạn cố định về số lần đảo ngược mà nó có thể tham gia với tư cách là phần nhỏ hơn của hoán đổi. Nếu số lượng yêu cầu vượt quá giá trị của nó, thì không có lệnh hoán đổi nào có thể tránh được việc vi phạm ràng buộc toàn cầu. Do đó, kế toán đảo ngược nắm bắt tất cả các nghĩa vụ hoán đổi cần thiết mà không cần mô phỏng quy trình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class BIT:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        vals = sorted(set(a))
        comp = {v: i + 1 for i, v in enumerate(vals)}

        bit = BIT(len(vals))
        inv = [0] * n

        # count how many greater elements appear before each position
        for i in range(n):
            x = comp[a[i]]
            inv[i] = i - bit.sum(x)
            bit.add(x, 1)

        # now check feasibility via reverse pass
        bit = BIT(len(vals))
        for i in range(n - 1, -1, -1):
            x = comp[a[i]]
            greater_on_right = bit.sum(len(vals)) - bit.sum(x)
            if greater_on_right > a[i]:
                print("NO")
                break
            bit.add(x, 1)
        else:
            print("YES")

if __name__ == "__main__":
    solve()
```Giải pháp nén các giá trị vì chỉ có thứ tự tương đối mới quan trọng khi đếm các nghịch đảo. Cây Fenwick được sử dụng để tính toán số lượng liên quan đến nghịch đảo một cách hiệu quả. 

Quá trình chuyển tiếp xây dựng thông tin tần số, nhưng việc kiểm tra quan trọng nằm ở quá trình ngược lại: đối với mỗi phần tử, chúng tôi đếm xem có bao nhiêu phần tử lớn hơn nằm ở bên phải của nó. Con số đó tương ứng với số lần nó phải đổi chỗ sang trái. Nếu giá trị này vượt quá giá trị của phần tử, nó không thể tồn tại đủ lâu trong chuỗi hoán đổi. 

Điều kiện được kiểm tra nghiêm ngặt trong quá trình truyền tải; nếu vi phạm chúng tôi từ chối ngay. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
a = [1, 4, 3, 4]
```Chúng tôi theo dõi xem có bao nhiêu phần tử lớn hơn xuất hiện ở bên phải. 

| tôi | một [tôi] | lớn hơn bên phải | điều kiện (≤ a[i]) | tiểu bang | 
| --- | --- | --- | --- | --- | 
| 3 | 4 | 0 | được | giữ | 
| 2 | 3 | 1 (4) | được | giữ | 
| 1 | 4 | 0 | được | giữ | 
| 0 | 1 | 3 (4,3,4) | vi phạm | từ chối | 

Ở chỉ số 0, phần tử 1 sẽ cần phải di chuyển qua ba phần tử lớn hơn, nhưng nó chỉ có thể chịu được độ sâu hoán đổi lên tới 1. Điều này khiến cho việc cấu hình là không thể. 

### Ví dụ 2 

đầu vào:```
n = 5
a = [2, 1, 5, 3, 4]
```| tôi | một [tôi] | lớn hơn bên phải | tình trạng | tiểu bang | 
| --- | --- | --- | --- | --- | 
| 4 | 4 | 0 | được | giữ | 
| 3 | 3 | 1 (4) | được | giữ | 
| 2 | 5 | 0 | được | giữ | 
| 1 | 1 | 3 (5,3,4) | vi phạm | từ chối | 

Ở đây phần tử 1 buộc phải vượt qua quá nhiều phần tử lớn hơn, vượt quá khả năng hoán đổi của nó ngay lập tức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Hoạt động cây Fenwick cho từng phần tử | 
| Không gian | O(n) | tọa độ nén và lưu trữ BIT | 

Các ràng buộc cho phép tổng cộng lên tới 100000 phần tử, do đó, giải pháp O(n log n) cho mỗi trường hợp thử nghiệm phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict
    import sys
    input = sys.stdin.readline

    class BIT:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)

        def add(self, i, v):
            while i <= self.n:
                self.bit[i] += v
                i += i & -i

        def sum(self, i):
            s = 0
            while i > 0:
                s += self.bit[i]
                i -= i & -i
            return s

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))
            vals = sorted(set(a))
            comp = {v: i + 1 for i, v in enumerate(vals)}
            bit = BIT(len(vals))

            ok = True
            for i in range(n):
                x = comp[a[i]]
                bit.add(x, 1)

            bit = BIT(len(vals))
            for i in range(n - 1, -1, -1):
                x = comp[a[i]]
                greater = bit.sum(len(vals)) - bit.sum(x)
                if greater > a[i]:
                    ok = False
                    break
                bit.add(x, 1)

            out.append("YES" if ok else "NO")
        return "\n".join(out)

    return solve()

# sample
assert run("3\n4\n1 4 3 4\n2\n1 2\n3\n3 2 1\n") == "NO\nYES\nYES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mảng tăng đơn | CÓ | trường hợp đã được sắp xếp | 
| mảng đảo ngược | CÓ/KHÔNG tùy theo giá trị | ứng suất đảo ngược tối đa | 
| trùng lặp nặng | CÓ | giá trị lặp lại không phá vỡ logic | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các giá trị đều bằng nhau. Trong tình huống này, mọi hoán đổi luôn hợp lệ vì min(a[i], a[i+1]) luôn bằng chỉ số hoán đổi chỉ tối đa n và không tồn tại ràng buộc thứ tự. Thuật toán trả về chính xác CÓ vì không có phần tử nào phải vượt qua bất kỳ phần tử nào lớn hơn. 

Một trường hợp cạnh khác xảy ra khi phần tử nhỏ nhất ở gần cuối mảng. Phần tử đó tích lũy nhiều giao dịch hoán đổi cần thiết và vì giá trị của nó nhỏ nên nó nhanh chóng vi phạm ràng buộc. Kiểm tra vượt qua ngược nắm bắt điều này ngay lập tức bằng cách đếm xem có bao nhiêu phần tử lớn hơn nằm ở bên phải của nó. 

Trường hợp cạnh thứ ba là một mảng giảm nghiêm ngặt. Mỗi phần tử phải vượt qua tất cả các phần tử nhỏ hơn chính nó và phần tử nhỏ nhất sẽ tích lũy gánh nặng lớn nhất. Thuật toán sẽ từ chối ngay khi gánh nặng đó vượt quá giá trị của phần tử, phù hợp với việc không thể duy trì chuỗi hoán đổi cần thiết.
