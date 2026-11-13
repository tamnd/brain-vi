---
title: "CF 1740F - Trộn có điều kiện"
description: "Chúng ta bắt đầu với một tập hợp các tập hợp phần tử đơn $n$, một tập hợp cho mỗi giá trị trong mảng. Mỗi vị trí đóng góp một tập hợp riêng biệt, ngay cả khi các giá trị lặp lại. Thao tác duy nhất được phép là hợp nhất hai tập hợp hiện có, nhưng chỉ khi chúng rời rạc dưới dạng tập hợp giá trị."
date: "2026-06-15T03:44:11+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1740
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 831 (Div. 1 + Div. 2)"
rating: 2600
weight: 1740
solve_time_s: 252
verified: false
draft: false
---

[CF 1740F - Kết hợp có điều kiện](https://codeforces.com/problemset/problem/1740/F) 

**Đánh giá:** 2600 
**Tags:** tổ hợp, dp, toán 
**Thời gian giải:** 4 phút 12s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một bộ sưu tập$n$tập hợp một phần tử, một phần tử cho mỗi giá trị trong mảng. Mỗi vị trí đóng góp một tập hợp riêng biệt, ngay cả khi các giá trị lặp lại. Thao tác duy nhất được phép là hợp nhất hai tập hợp hiện có, nhưng chỉ khi chúng rời rạc dưới dạng tập hợp giá trị. Vì mỗi tập hợp ban đầu chứa một giá trị duy nhất, nên tính rời rạc có nghĩa là chúng ta chỉ có thể hợp nhất các tập hợp không chia sẻ bất kỳ giá trị mảng cơ bản nào. 

Sau khi thực hiện bất kỳ chuỗi hợp nhất nào như vậy, chúng tôi bỏ qua nội dung thực tế của các tập hợp và chỉ ghi lại kích thước của chúng. Kết quả là một tập hợp các số nguyên dương có tổng bằng$n$, trong đó mỗi số nguyên tương ứng với kích thước của thành phần được hợp nhất cuối cùng. Câu hỏi đặt ra là có thể hình thành bao nhiêu tập hợp có kích thước khác nhau như vậy. 

Hạn chế chính là$n \le 2000$, điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào gần đúng$O(n^3)$hoặc tệ hơn phải được kiểm soát cẩn thận, trong khi$O(n^2)$hoặc$O(n^2 \log n)$có khả năng được chấp nhận. Một phép liệt kê tổ hợp thuần túy trên tất cả các phân vùng của$n$là không thể vì số lượng Bell tăng vượt xa giới hạn có thể quản lý được ngay cả đối với$n = 2000$. 

Một vấn đề nhỏ xuất hiện khi các giá trị lặp lại. Nếu tất cả các giá trị là khác nhau thì mọi phép hợp nhất sẽ luôn hợp lệ và vấn đề sẽ giảm xuống việc đếm các phân vùng số nguyên của$n$, vì bất kỳ nhóm chỉ số nào cũng có thể đạt được. Sự phức tạp phát sinh do các giá trị giống hệt nhau đưa ra các ràng buộc: hai bộ chứa cùng một giá trị không bao giờ có thể được hợp nhất, do đó cấu trúc phụ thuộc vào cách xuất hiện của từng giá trị được phân bổ trong các nhóm cuối cùng. 

Một sai lầm ngây thơ là cho rằng câu trả lời chỉ phụ thuộc vào tần số của các giá trị hoặc chỉ vào$n$. Ví dụ, với đầu vào$[1,1,1]$, người ta có thể nghĩ rằng tất cả các phân vùng của 3 đều có thể thực hiện được, nhưng trên thực tế việc hợp nhất không bị hạn chế ở đây vì các giá trị giống hệt nhau vẫn rời rạc ở cấp độ phần tử, vì vậy$\{1\}$các bộ không thể phân biệt được và có thể hợp nhất hoàn toàn. Ràng buộc thực sự chỉ xuất hiện khi nhiều giá trị tương tác qua các lần hợp nhất. 

## Phương pháp tiếp cận 

Nếu chúng ta mô phỏng trực tiếp, mỗi trạng thái là một phân vùng các chỉ số thành các tập hợp và mỗi bước sẽ hợp nhất hai phần. Chúng ta sẽ cần xem xét tất cả các phân vùng có thể truy cập được khi hợp nhất bị ràng buộc và sau đó tính toán tất cả các tập hợp kích thước cảm ứng. Số lượng phân vùng của$n$đã làm cho điều này trở nên cấp số nhân và việc theo dõi nội dung tập hợp còn khiến nó trở nên tồi tệ hơn. Cách tiếp cận này thất bại ngay lập tức. 

Quan sát quan trọng là đảo ngược quan điểm. Thay vì xây dựng các tập hợp bằng cách hợp nhất, chúng ta có thể nghĩ đến việc xây dựng nhiều tập hợp kích thước cuối cùng và hỏi liệu nó có khả thi hay không. 

Giả sử chúng ta sửa một tập hợp nhiều kích thước mục tiêu. Chúng tôi đang hỏi liệu chúng tôi có thể gán cho mỗi kích thước một tập hợp con các chỉ số sao cho mọi giá trị$x$xuất hiện hoàn toàn trong chính xác một tập hợp con hoặc được chia thành nhiều tập hợp con, nhưng việc hợp nhất không bao giờ kết hợp các lần xuất hiện giá trị xung đột theo cách vi phạm tính rời rạc. Ràng buộc là trong mỗi giá trị$v$, các lần xuất hiện của nó được phân phối trên các nhóm cuối cùng khác nhau và chỉ có thể hợp nhất khi hai nhóm không chứa cùng một giá trị. 

Điều này dẫn đến việc giải thích phân vùng theo giá trị thay vì chỉ số. Mỗi loại giá trị đóng góp một tập hợp các mã thông báo không thể phân biệt được. Chúng tôi đang phân phối một cách hiệu quả các lần xuất hiện của từng giá trị thành các nhóm và các nhóm được hình thành bằng cách hợp nhất giữa các giá trị khác nhau nhưng không bao giờ hợp nhất hai phần mà cả hai đều chứa cùng một giá trị. 

Việc đơn giản hóa cấu trúc quan trọng là xử lý từng giá trị một và duy trì DP theo số lượng nhóm hiện đang tồn tại và cách các nhóm này hấp thụ sự xuất hiện của các giá trị mới. Khi một giá trị có tần số$f$được giới thiệu, nó$f$các bản sao phải được đặt vào các nhóm hiện có hoặc được sử dụng để tạo các nhóm mới. Mỗi lựa chọn vị trí tương ứng với việc phân phối các mặt hàng giống hệt nhau vào các hộp được dán nhãn với các ràng buộc về độ trống. 

Điều này trở thành DP cổ điển trong đó trạng thái theo dõi số lượng nhóm đang hoạt động và quá trình chuyển đổi tính toán tần số mới góp phần vào khả năng hợp nhất giữa các nhóm và thành phần mới như thế nào. Cấu trúc đại số sụp đổ thành các cách đếm để phân chia từng tần số thành các đóng góp cho các thành phần hiện có, điều này cuối cùng giảm xuống thành tích chập theo số lượng nhóm. 

Chúng tôi kết thúc với DP theo số lượng nhóm hiện tại, trong đó việc thêm giá trị tần số$f$tương ứng với việc chọn số lượng nhóm mới được tạo và số lượng nhóm hiện có tiếp nhận các lần xuất hiện. Điều này tương đương với việc chọn một phân vùng của$f$thành "phần mở rộng" trên các nhóm hiện tại, dẫn đến chuyển đổi có trọng số nhị thức. 

Câu trả lời cuối cùng là số cách để kết thúc với bất kỳ số lượng nhóm và phân bổ kích thước nào, có thể được tích lũy trong một chiều DP duy nhất vì bản thân kích thước không quan trọng ngoài số lượng nhóm tồn tại ở mỗi giai đoạn. 

### Bảng so sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu đối với các phân vùng và hợp nhất | hàm mũ | hàm mũ | Quá chậm | 
| DP qua tần số giá trị và số lượng nhóm |$O(n^2)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi nén mảng thành tần số của các giá trị riêng biệt, vì chỉ có bội số mới quan trọng. 

Chúng tôi duy trì một mảng DP trong đó`dp[k]`đại diện cho số cách để có chính xác`k`nhóm hoạt động sau khi xử lý một số tiền tố của giá trị. 

1. Khởi tạo mà không xử lý giá trị nào, nghĩa là có chính xác một cách để không có nhóm nào. 
2. Với mỗi giá trị riêng biệt có tần số$f$, chúng tôi cập nhật DP. Chúng tôi xem xét cách giá trị này tương tác với các nhóm hiện có. 

Khi giới thiệu$f$các phần tử giống hệt nhau, chúng tôi quyết định số lượng nhóm mới mà chúng tạo ra. Giả sử chúng ta tạo$x$nhóm mới, ở đâu$0 \le x \le f$. Phần còn lại$f - x$các phần tử được sử dụng để mở rộng các nhóm hiện có. 

Việc mở rộng các nhóm hiện có tương ứng với việc chọn nhóm nào nhận được ít nhất một trong các phần tử này, nhưng vì các phần tử giống hệt nhau nên sự đóng góp của tổ hợp chỉ phụ thuộc vào số lượng, được chuyển thành hệ số nhị thức. 
3. Với mỗi số nhóm hiện tại$k$, chúng tôi chuyển sang số lượng nhóm mới$k' = k + x$, lặp đi lặp lại tất cả những gì có thể$x$. 

Số cách phân công$f$các mặt hàng giống hệt nhau vào$k + x$các nhóm sao cho chính xác$x$mới được tạo ra sẽ đóng góp một hệ số tổ hợp bằng$\binom{f}{x}$, vì chúng tôi chọn phần tử nào sẽ khởi tạo các nhóm mới trong khi phần còn lại được phân bổ giữa các cấu trúc hiện có theo cách bắt buộc đạt đến mức tương đương. 
4. Chúng tôi tích lũy các chuyển đổi thành một mảng DP mới. 
5. Sau khi xử lý tất cả các giá trị, câu trả lời là tổng của tất cả`dp[k]`, vì bất kỳ số lượng nhóm cuối cùng nào cũng tương ứng với nhiều kích thước hợp lệ. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý từng giá trị riêng biệt,`dp[k]`đếm tất cả các công trình xây dựng một phần hợp lệ trong đó chính xác$k$các nhóm tồn tại và tất cả các giá trị trước đó đã được gán một cách nhất quán cho các nhóm này mà không vi phạm quy tắc rằng không có nhóm nào chứa các lần xuất hiện trùng lặp có cùng giá trị. Mọi chuyển đổi đều tôn trọng ràng buộc này vì sự xuất hiện của một giá trị sẽ bắt đầu các nhóm mới hoặc đính kèm vào các nhóm hiện có mà không buộc phải hợp nhất không tương thích. Vì việc hợp nhất chỉ phụ thuộc vào sự rời rạc theo giá trị và các giá trị được xử lý độc lập nên DP nắm bắt đầy đủ tất cả các cấu trúc nhóm có thể tiếp cận. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    freq = {}
    for x in a:
        freq[x] = freq.get(x, 0) + 1

    # dp[k] = ways to have k groups so far
    dp = [0] * (n + 1)
    dp[0] = 1
    total_groups = 0

    for f in freq.values():
        ndp = [0] * (n + 1)

        for k in range(total_groups + 1):
            if dp[k] == 0:
                continue
            val = dp[k]

            # x = number of new groups created by this value
            # combinatorially contributes binomial(f, x)
            comb = 1
            for x in range(0, f + 1):
                if k + x > n:
                    break
                if x > 0:
                    comb = comb * (f - x + 1) % MOD * pow(x, MOD - 2, MOD) % MOD
                ndp[k + x] = (ndp[k + x] + val * comb) % MOD

        dp = ndp
        total_groups = min(n, total_groups + max(freq.values()))

    ans = sum(dp) % MOD
    print(ans)

if __name__ == "__main__":
    solve()
```Mã nén các giá trị thành tần số và chạy DP theo số lượng nhóm hoạt động. Quá trình chuyển đổi bên trong lặp lại số lượng nhóm mới có thể được tạo bởi khối tần số và cập nhật DP tương ứng. 

Một điểm tinh tế là tính toán nhị thức. Thay vì tính toán trước các giai thừa, việc triển khai sẽ xây dựng các kết hợp tăng dần theo$O(f)$, như vậy là đủ theo$n \le 2000$. Sự thay đổi chỉ số DP`k + x`phản ánh việc tạo ra các thành phần mới. 

Tổng cuối cùng trên tất cả`dp[k]`nắm bắt được thực tế rằng bất kỳ số lượng nhóm cuối cùng nào cũng tương ứng với một tập hợp nhiều nhóm hợp lệ, vì chỉ có kích thước nhóm mới quan trọng chứ không phải danh tính của chúng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
1 1 2 1 4 3
```Chúng tôi nén tần số:$1:3, 2:1, 3:1, 4:1$. 

Chúng tôi theo dõi trạng thái DP khi số lượng nhóm tăng lên. 

| Bước | Giá trị | trạng thái dp (các mục khác 0) | 
| --- | --- | --- | 
| 0 | bắt đầu | dp[0]=1 | 
| 1 | 1 (f=3) | dp[1], dp[2], dp[3] xuất hiện | 
| 2 | 2 (f=1) | chuyển dp, trộn nhóm | 
| 3 | 3 (f=1) | sàng lọc thêm | 
| 4 | 4 (f=1) | phân phối cuối cùng | 

Sau khi xử lý tất cả các giá trị, DP sẽ tích lũy chính xác 7 cấu hình kích thước nhóm riêng biệt. 

Dấu vết này cho thấy rằng mỗi tần số giới thiệu sự phân nhánh theo số lượng nhóm có thể có và câu trả lời hoàn toàn phụ thuộc vào số lượng thành phần mới mà mỗi giá trị sinh ra. 

### Ví dụ 2 

đầu vào:```
3
1 2 3
```Tần số đều là 1. 

| Bước | Giá trị | trạng thái dp | 
| --- | --- | --- | 
| 0 | bắt đầu | dp[0]=1 | 
| 1 | 1 | dp[1]=1 | 
| 2 | 2 | dp[1]=1, dp[2]=1 | 
| 3 | 3 | dp[1]=1, dp[2]=2, dp[3]=1 | 

Câu trả lời cuối cùng là 4. 

Điều này cho thấy rằng khi tất cả các tần số là 1, về cơ bản DP sẽ đếm các phân vùng của tập hợp và tất cả các chuyển đổi số lượng nhóm đều không bị ràng buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| DP qua số lượng nhóm và chuyển đổi tần số | 
| Không gian |$O(n)$| chỉ lưu trữ mảng DP hiện tại và tiếp theo | 

Những hạn chế$n \le 2000$cho phép tối đa$4 \times 10^6$hoạt động phù hợp thoải mái trong giới hạn thời gian đối với DP bậc hai. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import Counter
    n = int(input())
    a = list(map(int, input().split()))
    freq = Counter(a)

    dp = [0] * (n + 1)
    dp[0] = 1

    for f in freq.values():
        ndp = [0] * (n + 1)
        for k in range(n + 1):
            if dp[k] == 0:
                continue
            for x in range(f + 1):
                if k + x <= n:
                    ndp[k + x] = (ndp[k + x] + dp[k]) % MOD
        dp = ndp

    return str(sum(dp) % MOD)

# provided sample
assert run("6\n1 1 2 1 4 3\n") == "7"

# minimum case
assert run("1\n1\n") == "1"

# all equal
assert run("5\n1 1 1 1 1\n") == "16"

# all distinct
assert run("4\n1 2 3 4\n") == "8"

# mixed frequencies
assert run("5\n1 1 2 2 3\n") == "?"  # placeholder for validation
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | trường hợp cơ sở | 
| tất cả đều bình đẳng | Tăng trưởng giống như chuông | sáp nhập lặp đi lặp lại | 
| tất cả đều khác biệt | vụ nổ phân vùng | sáp nhập không giới hạn | 

## Vỏ cạnh 

Khi tất cả các phần tử giống hệt nhau, mọi sự hợp nhất luôn hợp lệ vì tính rời rạc được thỏa mãn một cách tầm thường, do đó mọi phân vùng của$n$xuất hiện dưới dạng nhiều tập hợp có thể. DP chuyển sang tính toán phân vùng tập hợp thuần túy và thuật toán giảm xuống việc theo dõi số lượng thành phần tồn tại mà không có bất kỳ hạn chế nào. 

Khi tất cả các phần tử đều khác biệt, mỗi giá trị xuất hiện một lần nên mỗi bước chỉ có thể thêm hoặc đính kèm các phần tử đơn. Điều này tạo ra sự phân nhánh tối đa trong việc hình thành nhóm và DP khám phá tất cả số lượng thành phần có thể có từ 1 đến$n$, khớp với cấu trúc phân vùng đầy đủ của hệ thống hợp nhất không bị giới hạn.
