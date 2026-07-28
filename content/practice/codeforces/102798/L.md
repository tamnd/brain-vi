---
title: "CF 102798L - Bậc thầy đồng hồ"
description: "Chúng tôi có một bộ sưu tập các bánh răng. Một bánh răng có răng t có giá t xu, và sau k chu kỳ nó sẽ chỉ hướng k mod t. Đối với một bộ bánh răng đã chọn, không phải mọi bộ hướng dẫn có thể đều có thể truy cập được."
date: "2026-07-27T17:55:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102798
codeforces_index: "L"
codeforces_contest_name: "2020 China Collegiate Programming Contest, Weihai Site"
rating: 0
weight: 102798
solve_time_s: 67
verified: true
draft: false
---

[CF 102798L - Bậc thầy đồng hồ](https://codeforces.com/problemset/problem/102798/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ sưu tập các bánh răng. Một thiết bị với`t`chi phí răng`t`tiền xu và sau đó`k`giai đoạn nó cho thấy hướng`k mod t`. Đối với một bộ bánh răng đã chọn, không phải mọi bộ hướng dẫn có thể đều có thể truy cập được. Câu hỏi đặt ra là chọn các bánh răng có tổng chi phí lớn nhất`b`sao cho số lượng bộ dữ liệu hướng có thể tiếp cận càng lớn càng tốt. Thay vì in con số khổng lồ này, chúng tôi in logarit tự nhiên của nó. 

Đối với bánh răng có kích thước`t1, t2, ...`, số trạng thái khác nhau mà đồng hồ có thể hiển thị chính xác là:$$\operatorname{lcm}(t_1,t_2,\dots,t_n)$$bởi vì sau một bội số chung nhỏ nhất đầy đủ, mọi con trỏ sẽ quay trở lại hướng bắt đầu và mỗi thời gian nhỏ hơn trong chu kỳ đó sẽ tạo ra một trạng thái toàn cục khác. 

Ngân sách nhiều nhất là`30000`, và có thể có tới`30000`trường hợp thử nghiệm. Một giải pháp hoạt động riêng biệt cho mọi truy vấn không thể đáp ứng được bất cứ điều gì gần với công việc hàm mũ hoặc thậm chí bậc hai. Cách tiếp cận phù hợp là xử lý trước tất cả các câu trả lời với ngân sách tối đa một lần, sử dụng phương pháp lập trình động xung quanh`30000^2`hoạt động. 

Một số trường hợp rất dễ xử lý sai. Với ngân sách`2`, chọn bánh răng 2 răng sẽ có hai trạng thái nên đáp án là`ln(2)`. Một chương trình bắt đầu từ một tích trống nhưng quên rằng luôn cho phép một bánh răng có thể trả về số 0. 

Dành cho ngân sách`7`, chọn bánh răng có kích thước`3`Và`4`cho một lcm của`12`, tốt hơn bánh răng 7 răng. Câu trả lời là:```
7
```với đầu ra:```
2.484906650
```Một chiến lược tham lam luôn sử dụng trang bị lớn nhất hiện có sẽ chọn`7`và thất bại. 

Một cái bẫy khác là việc chọn nhiều lũy thừa có cùng số nguyên tố. Lấy bánh răng`2`Và`4`chi phí`6`, nhưng lcm của họ chỉ`4`, không`8`. Phép biến đổi tối ưu phải tránh tính các thừa số nguyên tố trùng lặp. 

## Phương pháp tiếp cận 

Cách giải thích mạnh mẽ là thử mọi tập hợp bánh răng có thể có tổng chi phí nằm trong ngân sách, tính toán lcm của nó và giữ lại bộ lớn nhất. Điều này đúng vì nó trực tiếp kiểm tra mọi thiết kế hợp lệ. Tuy nhiên, số lượng tập hợp con có thể tăng theo cấp số nhân. Ngay cả khi chỉ có 20 lựa chọn thiết bị khả thi thì cũng đã có hơn một triệu tập hợp con và số lượng lựa chọn hữu ích thực tế còn lớn hơn nhiều. 

Quan sát quan trọng là lcm được xây dựng từ các lũy thừa chính. Nếu hai bánh răng được chọn có lũy thừa nguyên tố khác nhau`p^a`Và`q^b`, việc thay thế chúng bằng hai lũy thừa chính đó sẽ mang lại mức đóng góp lcm tương tự trong khi vẫn giữ chi phí nhỏ hơn. Hướng dẫn chính thức mô tả mức giảm này: một giải pháp tối ưu chỉ cần các công suất chính làm bánh răng. 

Sau sự giảm bớt này, vấn đề trở thành một cái ba lô. Mỗi số nguyên tố đóng góp một nhóm lựa chọn: không chọn thiết bị nào của số nguyên tố này hoặc chọn một trong số`p`,`p^2`,`p^3`, vân vân. Lựa chọn`p^k`thêm vào`p^k`đến chi phí và`ln(p^k)`để trả lời. Đây là một chiếc ba lô được nhóm lại vì chỉ có thể chọn một lũy thừa của mỗi số nguyên tố. 

Nhà nước lưu trữ logarit tốt nhất có thể đạt được với một ngân sách nhất định. Vì logarit biến phép nhân của lcm thành phép cộng, nên quá trình chuyển đổi giá trị chiếc ba lô chỉ đơn giản là phép cộng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Ba lô nhóm | O(B² / log B) | O(B) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo tất cả các số nguyên tố lên đến`30000`bằng một cái sàng. Mỗi số nguyên tố sẽ đại diện cho một nhóm trong ba lô. 
2. Với mọi số nguyên tố`p`, tạo ra các bánh răng có thể`p, p², p³, ...`trong khi giá trị là nhiều nhất`30000`. Đây là những lựa chọn hữu ích duy nhất cho số nguyên tố này vì việc lấy hai lũy thừa của cùng một số nguyên tố không làm tăng lcm. 
3. Chạy ba lô được nhóm lên trên các nhóm chính. Đối với mỗi số nguyên tố, hãy sao chép trạng thái DP hiện tại và thử cộng mọi lũy thừa có thể có của số nguyên tố đó. Sự chuyển tiếp là:$$dp[new\_cost] = \max(dp[new\_cost], dp[old\_cost] + \ln(power))$$Việc sao chép là cần thiết vì cùng một số nguyên tố không thể được chọn hai lần. 

1. Sau khi xử lý tất cả các số nguyên tố, hãy trả lời mọi truy vấn bằng cách sử dụng`dp[b]`. 

Bất biến là sau khi xử lý một số số nguyên tố đầu tiên,`dp[x]`là logarit tối đa của lcm có thể được hình thành chỉ bằng cách sử dụng các số nguyên tố có tổng chi phí nhiều nhất`x`. Mọi lựa chọn hợp lệ đều bỏ qua số nguyên tố hiện tại hoặc chọn chính xác một trong các lũy thừa của nó, do đó quá trình chuyển đổi được nhóm sẽ xem xét mọi trạng thái tối ưu có thể có. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

MAX_B = 30000

def build():
    is_prime = [True] * (MAX_B + 1)
    is_prime[0] = is_prime[1] = False
    primes = []
    for i in range(2, MAX_B + 1):
        if is_prime[i]:
            primes.append(i)
            if i * i <= MAX_B:
                for j in range(i * i, MAX_B + 1, i):
                    is_prime[j] = False

    dp = [0.0] * (MAX_B + 1)

    for p in primes:
        powers = []
        x = p
        while x <= MAX_B:
            powers.append(x)
            x *= p

        old = dp[:]
        for cost in range(MAX_B + 1):
            best = old[cost]
            for x in powers:
                if cost >= x:
                    best = max(best, old[cost - x] + math.log(x))
            dp[cost] = best

    return dp

def solve():
    dp = build()
    t = int(input())
    ans = []
    for _ in range(t):
        b = int(input())
        ans.append("{:.9f}".format(dp[b]))
    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Sàng xây dựng các nhóm nguyên tố một lần vì tất cả các trường hợp thử nghiệm đều có chung ngân sách tối đa. Danh sách lũy thừa cho một số nguyên tố chỉ chứa các lũy thừa nguyên tố hợp lệ, do đó không có đóng góp nguyên tố trùng lặp nào có thể nhập vào lcm. 

Việc chuyển đổi được nhóm sử dụng`old = dp[:]`trước khi xử lý một số nguyên tố. Nếu không có bản sao này, quá trình chuyển đổi có thể chọn hai lũy thừa có cùng số nguyên tố trong một lần lặp, tạo ra lcm không hợp lệ. 

Các giá trị dấu phẩy động ở đây an toàn vì đầu ra được yêu cầu chỉ là logarit với`1e-6`độ chính xác. Giá trị lớn nhất có thể đủ nhỏ cho số học dấu phẩy động của Python. 

## Ví dụ đã hoạt động 

Dành cho ngân sách`7`, các trạng thái hữu ích là: 

| Bước | Bánh răng được chọn | Chi phí | Giá trị nhật ký | 
| --- | --- | --- | --- | 
| Bắt đầu | không | 0 | 0 | 
| Chọn 3 | 3 | 3 | ln(3) | 
| Chọn 4 | 4 | 4 | ln(4) | 
| Kết hợp 3 và 4 | 3,4 | 7 | ln(12) | 

Trạng thái cuối cùng sử dụng hai thừa số nguyên tố khác nhau và đạt tới 12 vị trí đồng hồ có thể có. 

Dành cho ngân sách`10`: 

| Bước | Bánh răng được chọn | Chi phí | LCM | 
| --- | --- | --- | --- | 
| Bắt đầu | không | 0 | 1 | 
| Chọn 5 | 5 | 5 | 5 | 
| Chọn 8 | 8 | 8 | 8 | 
| Chọn 3 và 7 | 3,7 | 10 | 21 | 
| Chọn 2,3,5 | 10 | 30? | không thể | 

Lựa chọn tối ưu được tìm thấy bằng các chuyển đổi DP chứ không phải bằng quy tắc tham lam. Đầu ra là:```
3.401197382
```cái nào bằng`ln(30)`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(B² / log B) | Có O(B/log B) lũy thừa nguyên tố được nhóm theo số nguyên tố và mỗi nhóm sẽ quét phạm vi ngân sách. | 
| Không gian | O(B) | Chỉ có mảng ba lô hiện tại được lưu trữ. | 

Với`B = 30000`, quá trình tiền xử lý phù hợp thoải mái trong giới hạn. Mỗi trường hợp thử nghiệm sau đó được trả lời trong thời gian không đổi. 

## Trường hợp thử nghiệm```python
import math

def solve_string(inp):
    import sys, io
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    sys.stdin = old

# Expected manual checks:
# 1
# 0.693147181

# 3
# 0.693147181
# 2.484906650
# 3.401197382

# Custom:
# Budget 1 cannot buy a useful gear except a 1-tooth gear, giving one state.
# Budget 6 should prefer 5 and 1? Actually 2 and 3 gives lcm 6.
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`0.000000000`| Xử lý ngân sách tối thiểu | 
|`1 / 2`|`0.693147181`| Bánh răng đơn | 
|`1 / 7`|`2.484906650`| Sự kết hợp của các số nguyên tố khác nhau | 
|`1 / 6`|`1.791759469`| Tránh trùng lặp quyền lực chính | 

## Vỏ cạnh 

Dành cho ngân sách`1`, bánh răng duy nhất có thể có một hướng. DP bắt đầu bằng giá trị 0, biểu thị lcm bằng 1, do đó, nó trả về chính xác`ln(1)=0`. 

Dành cho ngân sách`6`, việc thực hiện bất cẩn có thể chọn bánh răng`2`Và`4`và yêu cầu một lcm`8`. Chiếc ba lô được nhóm lại không bao giờ cho phép sự chuyển đổi đó vì cả hai lựa chọn đều thuộc nhóm chính cho`2`. Nó có thể chọn`4`, hoặc`2`, nhưng không phải cả hai. 

Dành cho ngân sách`7`, việc lựa chọn tham lam không thành công vì thiết bị lớn nhất không phải lúc nào cũng tốt nhất. DP có thể chọn nhóm làm nguyên tố`3`có giá trị`3`và nhóm thủ tướng`2`có giá trị`4`, sản xuất lcm`12`, đánh bại mọi thiết bị có giá cao nhất`7`. 

Đối với ngân sách lớn, thuật toán vẫn hoạt động vì quá trình tiền xử lý chỉ phụ thuộc vào ngân sách tối đa chứ không phụ thuộc vào số lượng truy vấn. Đầu vào có thể chứa nhiều yêu cầu, nhưng mỗi yêu cầu đều được trả lời bằng cách tra cứu trực tiếp.
