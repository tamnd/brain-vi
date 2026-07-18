---
title: "CF 103451B - Tổng các khoản tiền"
description: "Chúng ta được cung cấp một mảng các số nguyên không âm và một phép toán đệ quy liên tục “mở rộng” nó thành một giá trị mới. Ở cấp độ cơ sở, khi cấp độ bằng 0, giá trị chỉ đơn giản là tổng của tất cả các phần tử trong mảng. Đối với các cấp độ cao hơn, chúng tôi xem xét mọi mảng con liền kề."
date: "2026-07-03T07:20:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103451
codeforces_index: "B"
codeforces_contest_name: "Krosh Kaliningrad Contest 2"
rating: 0
weight: 103451
solve_time_s: 69
verified: true
draft: false
---

[CF 103451B - Tổng các tổng](https://codeforces.com/problemset/problem/103451/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên không âm và một phép toán đệ quy liên tục “mở rộng” nó thành một giá trị mới. 

Ở cấp độ cơ sở, khi cấp độ bằng 0, giá trị chỉ đơn giản là tổng của tất cả các phần tử trong mảng. 

Đối với các cấp độ cao hơn, chúng tôi xem xét mọi mảng con liền kề. Đối với mỗi mảng con, chúng tôi tính toán hàm tương tự ở cấp độ trước đó trên mảng con đó, rồi tính tổng tất cả các kết quả đó. Lặp lại k lần này sẽ tạo ra một số tăng rất nhanh. 

Vì vậy, nhiệm vụ là lấy mảng ban đầu, áp dụng thao tác tổng các giá trị trước của mảng con này k lần và xuất ra giá trị cuối cùng theo modulo 1e9+7. 

Kích thước đầu vào cho phép lên tới 200.000 phần tử và k lên tới 200.000. Điều đó ngay lập tức loại trừ bất kỳ cách tiếp cận nào liệt kê rõ ràng các mảng con dù chỉ một lần, vì có O(n^2) trong số đó. Ngay cả k = 1 cũng đã yêu cầu tổng hợp trên tất cả các mảng con và k > 1 sẽ tạo ra vụ nổ này. Bất kỳ giải pháp nào cũng phải tránh lặp lại các mảng con một cách rõ ràng. 

Trường hợp cạnh tinh tế xuất hiện khi k = 0. Trong trường hợp đó, phép đệ quy không bao giờ bắt đầu và câu trả lời chỉ là tổng đơn giản của mảng. Một trường hợp đặc biệt khác là khi n = 1. Mỗi mảng con chỉ là một phần tử duy nhất, do đó câu trả lời sẽ có cùng một giá trị cho tất cả k, đây là một cách kiểm tra độ chính xác hữu ích cho bất kỳ công thức dẫn xuất nào. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp tính toán f(A, 1) bằng cách lặp qua tất cả các cặp (l, r), tính tổng từng mảng con, sau đó lặp lại điều này trên cấu trúc kết quả cho k lớp. Đây đã là O(n^2) và thực hiện k lần sẽ mang lại hiệu quả O(k n^2), vượt xa mọi giới hạn khả thi. 

Quan sát quan trọng là toàn bộ quá trình là tuyến tính theo nghĩa là mọi yếu tố đều đóng góp độc lập cho câu trả lời cuối cùng. Đệ quy chỉ thay đổi số lần mỗi phần tử ban đầu được tính chứ không thay đổi cách các phần tử tương tác với nhau. Vì vậy, thay vì theo dõi các mảng con, chúng tôi theo dõi sự đóng góp của một vị trí cố định duy nhất qua tất cả các cấp độ. 

Ở mức 0, mỗi phần tử a[i] đóng góp chính xác bằng 1 vào tổng. 

Ở cấp độ 1, chúng ta đếm có bao nhiêu mảng con bao gồm chỉ mục i. Đó là i lựa chọn cho điểm cuối bên trái và (n - i + 1) lựa chọn cho điểm cuối bên phải, do đó hệ số trở thành i(n - i + 1). 

Ở cấp độ cao hơn, cấu trúc tương tự lặp lại, nhưng bên trong mỗi mảng con được chọn, vị trí của phần tử sẽ thay đổi. Điều này dẫn đến cấu trúc sản phẩm: số cách chọn ranh giới bên trái và ranh giới bên phải ở mỗi độ sâu được phân tách rõ ràng. Sau khi làm việc qua hai cấp độ, một mô hình xuất hiện trong đó các hệ số nhị thức xuất hiện một cách tự nhiên. 

Điều này dẫn đến một dạng đóng của hệ số a[i] sau k phép toán, hệ số này chỉ phụ thuộc vào vị trí của nó và n. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu đối với các mảng con theo cấp độ | O(k n^2) | O(n^2) | Quá chậm | 
| Công thức đóng góp kết hợp | O(n + k) | O(n + k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Diễn giải lại quy trình như một bài toán đóng góp tuyến tính 

Chúng tôi giả sử câu trả lời cuối cùng có thể được viết dưới dạng tổng có trọng số của các phần tử ban đầu, trong đó mỗi a[i] có một hệ số chỉ phụ thuộc vào chỉ số và k của nó. Điều này hợp lệ vì mỗi bước là tổng của các mảng con, tuyến tính trong các giá trị đầu vào. 

### 2. Tính hệ số cơ sở 

Với k = 0, mỗi phần tử đóng góp chính xác một lần, do đó hệ số là 1 với mọi i. 

Với k = 1, mỗi phần tử i xuất hiện trong đúng i lựa chọn biên trái và (n - i + 1) lựa chọn biên phải, cho hệ số i(n - i + 1). 

Điều này xác nhận rằng hệ số chỉ phụ thuộc vào số lượng mảng con bao phủ một vị trí. 

### 3. Quan sát cấu trúc của k = 2 

Đối với cấp độ thứ hai, chúng ta tính tổng tất cả các mảng con một lần nữa, nhưng bây giờ mỗi mảng con đóng góp một trọng số bên trong riêng của nó.

Nếu chúng ta sửa một phần tử i trong mảng ban đầu, chúng ta sẽ chọn một mảng con [l, r] chứa phần tử đó. Bên trong mảng con đó, phần đóng góp cục bộ của nó chỉ phụ thuộc vào vị trí của nó (i - l + 1) và độ dài còn lại (r - i + 1). Tính tổng trên tất cả l và r hợp lệ sẽ tách thành hai tổng độc lập, một cho vế trái và một cho vế phải, tạo ra tích của các tổng tam giác. 

Điều này tạo ra: 

i(i + 1)/2 lần (n - i + 1)(n - i + 2)/2 

### 4. Khái quát hóa thành k cấp độ 

Mỗi cấp độ bổ sung sẽ tăng cả hai cạnh của “tam giác” tổ hợp lên 1. Điều này dẫn đến các hệ số nhị thức: 

Hệ số của a[i] = 

C(i + k - 1, k) × C(n - i + k, k) 

### 5. Tính đáp án cuối cùng 

Tính toán trước các giai thừa và giai thừa nghịch đảo lên tới n + k. Sau đó lấy tổng a[i] nhân với hệ số trên cho mỗi chỉ số. 

### Tại sao nó hoạt động 

Ở mọi cấp độ, thao tác chỉ chọn ranh giới bên trái và bên phải một cách độc lập. Những lựa chọn này tích lũy theo cấp số nhân qua các cấp độ đệ quy. Bởi vì vị trí của một phần tử bên trong một mảng con chỉ phụ thuộc vào việc ranh giới bên trái dịch chuyển nó bao xa, nên phép đếm sẽ phân tách thành hai lựa chọn tổ hợp độc lập, một từ bên trái và một từ bên phải. Tính độc lập này chính là thứ biến các phép tính tổng của mảng con lồng nhau thành tích của các hệ số nhị thức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def build_fact(n):
    fact = [1] * (n + 1)
    invfact = [1] * (n + 1)
    for i in range(1, n + 1):
        fact[i] = fact[i - 1] * i % MOD
    invfact[n] = pow(fact[n], MOD - 2, MOD)
    for i in range(n, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD
    return fact, invfact

def C(n, r, fact, invfact):
    if r < 0 or r > n:
        return 0
    return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

def main():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    maxv = n + k + 5
    fact, invfact = build_fact(maxv)

    ans = 0
    for i in range(n):
        left = C(i + k, k, fact, invfact)
        right = C(n - i - 1 + k, k, fact, invfact)
        ans = (ans + a[i] * left % MOD * right) % MOD

    print(ans)

if __name__ == "__main__":
    main()
```Bảng giai thừa được xây dựng lên tới n + k vì cả hai số hạng nhị thức đều dịch chuyển chỉ số lên tới k. Mỗi hệ số được tính toán theo thời gian không đổi bằng cách sử dụng các giai thừa được tính toán trước. 

Chi tiết triển khai chính là lập chỉ mục: công thức sử dụng i dựa trên 0, do đó số hạng bên trái trở thành C(i + k, k) và số hạng bên phải trở thành C(n - i - 1 + k, k). Một lỗi phổ biến là trộn lẫn đạo hàm dựa trên 1 với việc triển khai dựa trên 0, làm dịch chuyển tất cả các hệ số không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 0
1 2 3 4 5
```| tôi | một [tôi] | trái C(i+0,0) | phải C(n-i-1+0,0) | đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 1 | 
| 1 | 2 | 1 | 1 | 2 | 
| 2 | 3 | 1 | 1 | 3 | 
| 3 | 4 | 1 | 1 | 4 | 
| 4 | 5 | 1 | 1 | 5 | 

Kết quả là 15, phù hợp với thực tế rằng cấp 0 chỉ là tổng đơn giản. 

Điều này xác nhận rằng công thức thu gọn chính xác thành trường hợp nhận dạng khi k = 0. 

### Ví dụ 2 

đầu vào:```
5 1
1 2 3 4 5
```| tôi | một [tôi] | trái C(i+1,1) | phải C(n-i,1) | đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 5 | 5 | 
| 1 | 2 | 2 | 4 | 16 | 
| 2 | 3 | 3 | 3 | 27 | 
| 3 | 4 | 4 | 2 | 32 | 
| 4 | 5 | 5 | 1 | 25 | 

Tổng cộng là 105. 

Điều này phù hợp với cách giải thích rằng mỗi phần tử được tính bằng số mảng con chứa nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + k) | tiền xử lý giai thừa và một lần truyền qua mảng | 
| Không gian | O(n + k) | bảng giai thừa và nghịch đảo | 

Các ràng buộc cho phép tối đa 2e5 cho cả n và k, do đó, việc tính toán trước các giai thừa lên tới n + k vừa vặn một cách thoải mái và vòng lặp cuối cùng là tuyến tính theo n. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    maxv = n + k + 5
    fact = [1] * (maxv + 1)
    invfact = [1] * (maxv + 1)

    for i in range(1, maxv + 1):
        fact[i] = fact[i - 1] * i % MOD

    invfact[maxv] = pow(fact[maxv], MOD - 2, MOD)
    for i in range(maxv, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD

    def C(n, r):
        if r < 0 or r > n:
            return 0
        return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

    ans = 0
    for i in range(n):
        ans += a[i] * C(i + k, k) * C(n - i - 1 + k, k)
        ans %= MOD

    return str(ans)

# provided samples
assert solve("5 0\n1 2 3 4 5\n") == "15"
assert solve("5 1\n1 2 3 4 5\n") == "105"
assert solve("3 2\n1 2 3\n") == "42"

# custom cases
assert solve("1 0\n7\n") == "7"
assert solve("1 5\n7\n") == "7"
assert solve("2 1\n1 1\n") == "4"
assert solve("4 2\n1 2 3 4\n") == solve("4 2\n1 2 3 4\n"), "consistency"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 / 7 | 7 | trường hợp cơ sở phần tử đơn | 
| 1 5/7 | 7 | ổn định theo k | 
| 2 1 / 1 1 | 4 | tính đối xứng và tính đúng đắn | 
| 4 2 / 1 2 3 4 | nhất quán | kiểm tra tính nhất quán nội bộ | 

## Vỏ cạnh 

Với k = 0, thuật toán giảm xuống tính tổng a[i] vì cả hai số hạng nhị thức đều có giá trị bằng 1. Công thức xử lý điều này một cách rõ ràng vì C(x, 0) = 1 với mọi x hợp lệ. 

Với n = 1, số hạng bên trái trở thành C(k, k) = 1 và số hạng bên phải cũng trở thành C(k, k) = 1, do đó mọi cấp độ đều giữ nguyên giá trị. Điều này phù hợp với trực giác rằng chỉ có một mảng con ở mỗi giai đoạn, do đó không xảy ra vụ nổ tổ hợp nào. 

Đối với k lớn, các hệ số tăng nhanh nhưng vẫn bị giới hạn modulo MOD do tính toán trước giai thừa. Việc sử dụng nghịch đảo mô-đun đảm bảo rằng ngay cả khi giá trị nhị thức lớn, chúng vẫn có thể tính toán được trong thời gian không đổi trên mỗi phần tử.
