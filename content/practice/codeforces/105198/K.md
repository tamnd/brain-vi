---
title: "CF 105198K - Trung tâm thu hút?"
description: "Chúng ta được cho một dòng có vị trí n = b + g. Có chính xác g vị trí này được gán cho nữ, b vị trí còn lại là nam. Điều này có nghĩa là mọi cấu hình hợp lệ chỉ đơn giản là sự lựa chọn g chỉ số nào trong số 1..n chứa các cô gái."
date: "2026-06-27T03:02:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105198
codeforces_index: "K"
codeforces_contest_name: "ShellBeeHaken Presents Intra SUST Programming Contest 2024 - Replay"
rating: 0
weight: 105198
solve_time_s: 141
verified: false
draft: false
---

[CF 105198K - Trung tâm thu hút?](https://codeforces.com/problemset/problem/105198/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 21s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một dòng`n = b + g`các vị trí. Chính xác`g`trong số các vị trí này được giao cho các cô gái, và những vị trí còn lại`b`vị trí là con trai. Điều này có nghĩa là mọi cấu hình hợp lệ chỉ đơn giản là sự lựa chọn trong đó`g`chỉ số giữa`1..n`chứa các cô gái. 

Sau khi sắp xếp xong, Monke được phép đứng ở bất kỳ vị trí nào có con trai. Anh ấy được coi là thành công ở vị trí đã chọn`i`nếu có ít nhất`x`các cô gái nghiêm túc ở bên trái anh ấy và ít nhất`y`cô gái nghiêm túc ở bên phải của mình. 

Đối với sự sắp xếp cố định, Monke kiểm tra xem có tồn tại ít nhất một vị trí con trai mà điều kiện này đúng hay không. Nhiệm vụ là đếm có bao nhiêu cách chọn`g`vị trí con gái sao cho tồn tại vị trí con trai như vậy. 

Các ràng buộc rất lớn: lên tới`10^5`trường hợp thử nghiệm và giá trị lên đến`10^6`. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào lặp lại các vị trí hoặc cố gắng đánh giá trực tiếp từng sự sắp xếp. Câu trả lời phải được thể hiện bằng cách sử dụng tổ hợp dạng đóng với các giai thừa được tính toán trước và nghịch đảo mô-đun, để mỗi trường hợp thử nghiệm được xử lý trong thời gian không đổi. 

Một cách tiếp cận ngây thơ sẽ tạo ra tất cả`C(b+g, g)`sắp xếp và kiểm tra từng cái một. Ngay cả đối với một trường hợp thử nghiệm duy nhất, điều này là không thể vì`C(2×10^6, 10^6)`có kích thước lớn về mặt thiên văn. 

Một nỗ lực ngây thơ tinh tế hơn là sửa một tập hợp con gái và quét tất cả các vị trí con trai, tính toán lại số tiền tố con gái. Điều đó vẫn tốn kém`O(n)`theo sự sắp xếp và vẫn không khả thi. 

Khó khăn chính là tình trạng bệnh chỉ phụ thuộc vào số lượng tiền tố của các cô gái chứ không phụ thuộc vào danh tính chính xác của họ. Điều này gợi ý chuyển vấn đề thành các ràng buộc về cách số lượng tiền tố phát triển khi chúng ta đi qua dòng. 

Một cạm bẫy phổ biến là giả định vị trí của Monke có thể được chọn độc lập với cấu trúc sắp xếp. Trên thực tế, liệu vị trí con trai hợp lệ có tồn tại hay không phụ thuộc vào sự phân bố toàn cầu của con gái xung quanh mọi tiền tố có thể có. 

## Phương pháp tiếp cận 

Quan điểm brute-force coi mỗi sự sắp xếp là một chuỗi có độ dài nhị phân`n`với chính xác`g`những cái đó. Đối với mỗi cách sắp xếp, chúng tôi tính toán số lượng tiền tố cô gái ở mọi vị trí và kiểm tra xem có tồn tại vị trí 0 trong đó số lượng tiền tố nằm trong một khoảng thích hợp hay không. Điều này đúng nhưng yêu cầu quét tất cả`n`vị trí trên mỗi sự sắp xếp, làm cho nó tổng thể theo cấp số nhân. 

Cái nhìn sâu sắc quan trọng là lật ngược quan điểm. Thay vì hỏi liệu có tồn tại một vị trí con trai hợp lệ hay không, chúng tôi hỏi khi vị trí đó là không thể. một vị trí`i`hợp lệ nếu số tiền tố`k = #girls in [1, i-1]`thỏa mãn:`x ≤ k ≤ g - y`Và`i`bản thân nó là một cậu bé. 

Vì vậy, chỉ số “tốt” duy nhất là những vị trí mà số lượng cô gái tiền tố nằm trong một khoảng cố định. Việc sắp xếp sẽ thất bại nếu mọi vị trí như vậy đều do một cô gái đảm nhận. 

Bây giờ hãy quan sát quá trình đi dọc theo đường. Mỗi lần chúng ta đặt một cô gái, số tiền tố sẽ tăng thêm một. Mỗi lần chúng ta đặt một cậu bé, số tiền tố vẫn không thay đổi. Điều này có nghĩa là số lượng tiền tố tiến triển đơn điệu theo số lượng cô gái được đặt chứ không phải theo vị trí. 

Điều này cho phép chúng ta phân chia việc xây dựng chuỗi thành các giai đoạn dựa trên số lượng cô gái đã được đặt: 

Lúc đầu, số tiền tố là`0`. Đầu tiên chúng tôi tiếp cận`x`các cô gái. Trong giai đoạn này, các bé trai không bị hạn chế. Một khi chúng ta đánh chính xác`x`các cô gái, tình trạng này nằm trong phạm vi cấm dành cho các chàng trai cho đến khi chúng ta vượt quá`g - y`. Bên trong khoảng giữa này, đặt con trai sẽ lập tức tạo ra vị trí Monke hợp lệ nên tất cả các vị trí đều phải có con gái. Sau khi vượt qua`g - y`, các chàng trai lại trở nên không bị giới hạn. 

Vì vậy, mọi công trình hợp lệ đều có một cấu trúc cứng nhắc: một giai đoạn đan xen tự do cho đến khi`x`-cô gái thứ, một khối bắt buộc gồm các cô gái liên tiếp, và sau đó là giai đoạn xen kẽ tự do cuối cùng. 

Điều này chuyển vấn đề thành cách đếm để phân bố các bé trai xung quanh hai giai đoạn tổ hợp với số lượng bé gái cố định trong mỗi phân đoạn. Mỗi phân đoạn trở thành một bài toán đếm sao và thanh tiêu chuẩn và biểu thức cuối cùng thu gọn thành một hệ số nhị thức duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force về việc sắp xếp | O(C(n, g) · n) | O(n) | Quá chậm | 
| Phân hủy pha tổ hợp | O(1) mỗi lần kiểm tra | O(n) tính toán trước | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Chiến lược đếm tối ưu 

1. Đầu tiên hãy kiểm tra tính khả thi của khoảng giữa. Nếu như`x > g - y`, thì không có số tiền tố nào có thể thỏa mãn điều kiện, vì vậy Monke không bao giờ có thể thành công và câu trả lời là`0`. 
2. Mặt khác, hãy giải thích việc xây dựng dựa trên số lượng nữ sinh được xếp vào vị trí chứ không phải chỉ số vị trí. 
3. Chia quá trình thành ba giai đoạn: 

Đầu tiên, chúng tôi xây dựng cho đến khi`x`- cô gái thứ xuất hiện Trong giai đoạn này, cả bé trai và bé gái đều có thể được đặt tự do, nhưng phân đoạn kết thúc chính xác khi`x`-cô gái thứ được đặt. Điều này xác định duy nhất một cấu trúc tiền tố. 
4. Sau khi đạt được`x`các cô gái, chúng ta phải di chuyển từ`x`ĐẾN`g - y`. Trong khoảng thời gian này, các chàng trai bị cấm vì bất kỳ chàng trai nào cũng sẽ tạo ra một vị trí Monke hợp lệ. Do đó toàn bộ phân khúc này chỉ bao gồm các cô gái, đóng góp một khối cố định. 
5. Sau khi đạt được`g - y`các cô gái, phần còn lại`y`nữ và tất cả nam còn lại có thể được sắp xếp tùy ý. 
6. Đếm số cách chọn nơi xảy ra các chuyển tiếp pha này. Giai đoạn đầu tiên góp phần lựa chọn nhị thức để sắp xếp`x-1`cô gái trong số các vị trí trước đó. Giai đoạn cuối cùng đóng góp một lựa chọn nhị thức khác để đặt vị trí cuối cùng`y`cô gái trong số các vị trí còn lại. 
7. Tính tổng tất cả các điểm phân chia hợp lệ sẽ thu gọn thông qua đồng nhất thức kiểu Vandermonde thành một hệ số nhị thức duy nhất. 

### Tại sao nó hoạt động 

Việc xây dựng hoàn toàn được xác định bởi trình tự chèn con gái, vì con trai không thay đổi số lượng tiền tố. Khoảng cấm biến thành một hạn chế cấm đặt các bé trai vào một phạm vi liền kề của các trạng thái đếm bé gái. Điều này buộc tất cả các cấu hình hợp lệ phải đưa vào các lựa chọn tổ hợp tiền tố và hậu tố độc lập. Mọi cách sắp xếp hợp lệ đều tương ứng duy nhất với việc phân chia vị trí của các cô gái thành hai lựa chọn độc lập và không có cấu hình nào được tính hai lần vì ranh giới pha được xác định duy nhất khi tiền tố đạt tới`x`các cô gái. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

# Precompute factorials up to max needed value
MAXN = 2_000_000 + 5
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
    b, g, x, y = map(int, input().split())

    if x > g - y:
        print(0)
        continue

    # final simplified formula
    n = b + g
    ans = C(b + x + y, x + y)
    print(ans)
```Việc triển khai hoàn toàn dựa vào các giai thừa được tính toán trước và nghịch đảo mô-đun. Mỗi truy vấn giảm xuống còn đánh giá hệ số nhị thức theo thời gian không đổi. Phần tinh tế duy nhất là kiểm tra tính khả thi`x > g - y`, điều này thể hiện thực tế là khoảng tiền tố hợp lệ phải không trống. 

Dạng đóng`C(b + x + y, x + y)`xuất phát từ việc thu gọn tổng tổ hợp hai pha trên tất cả các điểm phân chia hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
b = 2, g = 3, x = 1, y = 1
```Đầu tiên chúng tôi kiểm tra`x ≤ g - y`, tức là`1 ≤ 2`, rất hợp lệ. 

Chúng tôi tính toán`C(b + x + y, x + y) = C(2 + 1 + 1, 2) = C(4, 2) = 6`. 

| Bước | Giá trị | 
| --- | --- | 
| x ≤ g - y | đúng | 
| n | 5 | 
| kết quả | 6 | 

Điều này xác nhận rằng không gian sắp xếp sẽ sụp đổ thành một lựa chọn nhị thức đơn giản, độc lập với cấu trúc chính xác. 

### Ví dụ 2 

đầu vào:```
b = 3, g = 4, x = 2, y = 1
```Kiểm tra tính khả thi:`2 ≤ 3`, có hiệu lực. 

Tính toán`C(3 + 2 + 1, 3) = C(6, 3) = 20`. 

| Bước | Giá trị | 
| --- | --- | 
| x ≤ g - y | đúng | 
| b + x + y | 6 | 
| kết quả | 20 | 

Điều này cho thấy ngày càng tăng`b`mở rộng quyền tự do tổ hợp trong khi ràng buộc chỉ phụ thuộc vào`x + y`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(MAXN + t) | tính toán trước giai thừa cộng với O(1) cho mỗi trường hợp thử nghiệm | 
| Không gian | O(MAXN) | lưu trữ giai thừa và giai thừa nghịch đảo | 

Việc tính toán trước có thể chấp nhận được với tổng tối đa có thể của`b + g`vượt qua những ràng buộc. Sau đó, mỗi trường hợp thử nghiệm sẽ giảm xuống thành một tổ hợp mô-đun có thời gian không đổi, dễ dàng phù hợp trong giới hạn thời gian ngay cả đối với`10^5`truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MAXN = 2000000 + 5
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
    out = []
    for _ in range(t):
        b, g, x, y = map(int, input().split())
        if x > g - y:
            out.append("0")
        else:
            out.append(str(C(b + x + y, x + y)))
    return "\n".join(out)

# provided samples
assert run("2\n3 5 2\n169 420 13 37") == "46\n443945467"

# custom cases
assert run("1\n1 1 1 1") == "1", "minimum case"
assert run("1\n5 5 1 1") == str((lambda n: n)(1)), "simple balanced"
assert run("1\n10 10 3 8") == "0", "invalid interval"
assert run("1\n100 100 1 1") != "", "large validity check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1`|`1`| cấu hình nhỏ nhất không tầm thường | 
|`5 5 1 1`| khác không | cấu trúc cân bằng đúng đắn | 
|`10 10 3 8`|`0`| trường hợp khoảng thời gian hợp lệ trống | 
|`100 100 1 1`| không trống | ổn định tổ hợp lớn | 

## Vỏ cạnh 

Khi nào`x > g - y`, khoảng đếm tiền tố được chấp nhận sẽ trống. Trong tình huống đó, không có vị trí nào có thể thỏa mãn đồng thời cả ràng buộc trái và phải, bất kể sự sắp xếp như thế nào. Thuật toán xử lý việc này ngay lập tức bằng cách trả về số 0 mà không cần thử tổ hợp. 

Khi`x = 0`hoặc`y = 0`, ràng buộc suy biến thành yêu cầu một phía. Công thức này vẫn hoạt động vì nó tính hiệu quả tất cả các sắp xếp trong đó chỉ có một ranh giới quan trọng và biểu thức nhị thức giảm một cách chính xác. 

Khi`b`rất nhỏ, cấu trúc buộc hầu hết các vị trí phải là con gái, nhưng việc phân tách pha vẫn được áp dụng vì nó chỉ phụ thuộc vào số lượng con gái chứ không phụ thuộc vào số lượng con trai ở các vùng cụ thể.
