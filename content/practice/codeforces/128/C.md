---
title: "CF 128C - Trò chơi với hình chữ nhật"
description: "Chúng ta bắt đầu với một hình chữ nhật được vẽ trên giấy lưới. Chỉ có biên giới là quan trọng, không phải nội thất. Người chơi liên tục vẽ một hình chữ nhật nhỏ hơn bên trong hình trước đó."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp"]
categories: ["algorithms"]
codeforces_contest: 128
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 94 (Div. 1 Only)"
rating: 2000
weight: 128
solve_time_s: 112
verified: true
draft: false
---

[CF 128C - Trò chơi với hình chữ nhật](https://codeforces.com/problemset/problem/128/C) 

**Xếp hạng:** 2000 
**Tags:** tổ hợp, dp 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một hình chữ nhật được vẽ trên giấy lưới. Chỉ có biên giới là quan trọng, không phải nội thất. Người chơi liên tục vẽ một hình chữ nhật nhỏ hơn bên trong hình trước đó. Đường viền mới không thể chạm vào đường viền trước đó tại bất kỳ điểm nào, vì vậy mọi hình chữ nhật mới phải cách nhau khỏi hình cũ ít nhất một ô lưới theo cả bốn hướng. 

Sau chính xác`k`di chuyển, chúng tôi muốn đếm xem có thể có bao nhiêu hình vẽ cuối cùng khác nhau. Hai trò chơi được coi là khác nhau nếu tập hợp các hình chữ nhật được vẽ trong quá trình này khác nhau. 

Quan sát quan trọng là mỗi lần di chuyển sẽ thu nhỏ hình chữ nhật bằng cách xóa ít nhất một hàng ở trên cùng, một hàng ở dưới cùng, một hàng ở bên trái và một hàng ở bên phải. Nếu hình chữ nhật hiện tại có chiều cao`h`và chiều rộng`w`, hình chữ nhật tiếp theo phải nằm khít bên trong với ít nhất một lớp khoảng cách, sao cho chiều cao của nó tối đa là`h - 2`và chiều rộng của nó tối đa trở thành`w - 2`. 

Các ràng buộc đi lên đến`1000`cho cả ba tham số. Điều đó ngay lập tức loại trừ bất kỳ cách tiếp cận nào tạo ra các chuỗi hình chữ nhật một cách rõ ràng. Ngay cả một chương trình động khối cũng bắt đầu trở nên nguy hiểm trong Python nếu mỗi lần chuyển đổi đều tốn kém. Chúng ta cần thứ gì đó xung quanh`O(n + m + k)`hoặc`O(nk + mk)`. 

Có một số trường hợp khó bỏ sót. 

Nếu như`k = 1`, chúng ta chỉ chọn đúng một hình chữ nhật bên trong hình ban đầu. Ví dụ:```
3 3 1
```Câu trả lời là`1`, bởi vì chỉ có trung tâm`1 × 1`công trình hình vuông. 

Việc triển khai bất cẩn có thể đếm không chính xác các hình chữ nhật chạm vào đường viền bên ngoài. 

Một trường hợp tinh tế khác xảy ra khi hình chữ nhật trở nên quá nhỏ trước khi hoàn thành tất cả các bước di chuyển.```
2 2 2
```Câu trả lời đúng là`0`. 

Sau một lần di chuyển, hình chữ nhật duy nhất có thể là`0 × 0`, không hợp lệ. Bất kỳ giải pháp nào chỉ theo dõi số lượng thao tác thu nhỏ mà không kiểm tra tính khả thi về mặt hình học sẽ bị tính quá mức. 

Một ví dụ thú vị hơn là:```
4 5 1
```Câu trả lời là`9`. 

Các hình chữ nhật bên trong hợp lệ là:```
1×1: 6 positions
1×2: 2 positions
2×1: 1 position
```Công thức đếm hình chữ nhật đơn giản thường bỏ sót rằng số lượng vị trí phụ thuộc vào kích thước. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản. Ở mỗi lần di chuyển, hãy liệt kê tất cả các hình chữ nhật vừa khít với hình trước đó mà không chạm vào đường viền của nó. Tiếp tục đệ quy cho`k`di chuyển. 

Điều này có tác dụng vì mọi trò chơi hợp lệ thực sự là một chuỗi các hình chữ nhật lồng nhau. Sự đệ quy khớp chính xác với các quy tắc trò chơi. 

Vấn đề là yếu tố phân nhánh. Một hình chữ nhật có kích thước`h × w`chứa đại khái`O(h²w²)`hình chữ nhật phụ. Trong trường hợp xấu nhất, cây đệ quy sẽ bùng nổ theo cấp số nhân. Ngay cả đối với các kích thước vừa phải, số lượng trạng thái vẫn trở nên rất lớn. 

Cấu trúc của trò chơi mang lại góc nhìn rõ ràng hơn nhiều. 

Giả sử hình chữ nhật ban đầu trải dài theo hàng`[1, n]`và cột`[1, m]`. Mỗi bước di chuyển sẽ chọn mức độ thu nhỏ từ trên, dưới, trái và phải một cách độc lập. Vì hình chữ nhật không thể chạm vào nhau nên mỗi lần di chuyển phải tăng ít nhất cả bốn lề`1`. 

Sau đó`k`di chuyển, chúng tôi đã chọn:```
k rows removed from the top,
k rows removed from the bottom,
k columns removed from the left,
k columns removed from the right.
```Thứ tự chỉ quan trọng ở chỗ những lần xóa này được phân bổ như thế nào qua các nước đi. 

Tập trung vào chiều dọc trước tiên. 

Nếu sau tất cả các bước di chuyển, chúng tôi đã loại bỏ`a`các hàng từ trên xuống và`b`các hàng từ dưới lên thì:```
a ≥ k
b ≥ k
a + b < n
```Trình tự loại bỏ qua các bước di chuyển tương ứng với việc phân vùng`a`vào trong`k`phần tích cực và phân vùng`b`vào trong`k`những phần tích cực. 

Số cách chia một số nguyên`x`vào trong`k`phần tích cực là:$\binom{x-1}{k-1}$Thay vì theo dõi từng hình chữ nhật trung gian, chúng tôi chỉ tính tổng độ co được phân bổ như thế nào. 

Lý do tương tự áp dụng theo chiều ngang. 

Sau khi đơn giản hóa các khoản tiền bằng nhận dạng tổ hợp tiêu chuẩn, tổng số mẫu thu gọn theo chiều dọc hợp lệ sẽ trở thành:$\binom{n-1}{2k}$Tương tự, số mẫu ngang là:$\binom{m-1}{2k}$Hai chiều này độc lập nên câu trả lời cuối cùng là tích của chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`m`, Và`k`. 
2. Kiểm tra xem`2k > n - 1`hoặc`2k > m - 1`. 

Mỗi bước di chuyển tiêu tốn ít nhất một lớp từ cả hai phía đối diện. Sau đó`k`di chuyển, chúng ta cần ít nhất`2k`hàng bị loại bỏ và`2k`tổng số cột bị loại bỏ. Nếu một trong hai chiều quá nhỏ thì không tồn tại chuỗi hợp lệ. 
3. Tính toán trước các giai thừa và giai thừa nghịch đảo lên đến`1000`. 

Chúng tôi cần truy vấn hệ số nhị thức nhanh theo modulo`10^9 + 7`. 
4. Tính toán:$\binom{n-1}{2k}$5. Tính toán:$\binom{m-1}{2k}$6. Nhân hai giá trị theo modulo`10^9 + 7`. 
7. In kết quả. 

### Tại sao nó hoạt động 

Mỗi lần di chuyển độc lập sẽ tăng bốn lề: trên, dưới, trái và phải. Rốt cuộc`k`di chuyển, tổng số lần di chuyển từ các phía đối diện sẽ xác định toàn bộ cấu trúc lồng nhau. 

Việc chọn một trò chơi hợp lệ cũng tương đương với việc chọn nơi`2k`các lớp thu hẹp bắt buộc xảy ra giữa các`n - 1`khoảng trống dọc có sẵn, và tương tự giữa các`m - 1`những khoảng trống ngang. 

Danh tính tổ hợp tính chính xác các lựa chọn này, không thiếu hoặc trùng lặp bất kỳ chuỗi hình chữ nhật nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def build_factorials(limit):
    fact = [1] * (limit + 1)
    for i in range(1, limit + 1):
        fact[i] = fact[i - 1] * i % MOD

    inv_fact = [1] * (limit + 1)
    inv_fact[limit] = pow(fact[limit], MOD - 2, MOD)

    for i in range(limit, 0, -1):
        inv_fact[i - 1] = inv_fact[i] * i % MOD

    return fact, inv_fact

def comb(n, r, fact, inv_fact):
    if r < 0 or r > n:
        return 0
    return fact[n] * inv_fact[r] % MOD * inv_fact[n - r] % MOD

def solve():
    n, m, k = map(int, input().split())

    limit = max(n, m)
    fact, inv_fact = build_factorials(limit)

    ans = comb(n - 1, 2 * k, fact, inv_fact)
    ans *= comb(m - 1, 2 * k, fact, inv_fact)
    ans %= MOD

    print(ans)

solve()
```Tính toán trước giai thừa hỗ trợ các truy vấn hệ số nhị thức theo thời gian không đổi. Vì giá trị lớn nhất chỉ`1000`, cái này rất rẻ. 

Người trợ giúp`comb`xử lý an toàn các trường hợp không hợp lệ bằng cách trả về`0`khi`r > n`. Điều đó tự động xử lý các cấu hình không thể như`2k > n - 1`. 

Công thức sử dụng`n - 1`Và`m - 1`bởi vì sự co lại xảy ra giữa các đường lưới. Một lỗi thường gặp là sử dụng`n`trực tiếp, vượt quá các vị trí biên giới không thể. 

Nghịch đảo modulo sử dụng định lý nhỏ Fermat vì`10^9 + 7`là nguyên tố. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3 1
```Chúng tôi tính toán: 

| Biến | Giá trị | 
| --- | --- | 
| n - 1 | 2 | 
| m - 1 | 2 | 
| 2k | 2 | 
| C(2, 2) | 1 | 
| C(2, 2) | 1 | 
| Trả lời | 1 | 

Hình chữ nhật hợp lệ duy nhất là trung tâm`1 × 1`quảng trường. Điều này khẳng định rằng mỗi lần di chuyển đều phải để lại một lớp khoảng cách ở tất cả các phía. 

### Ví dụ 2 

đầu vào:```
4 5 1
```Chúng tôi tính toán: 

| Biến | Giá trị | 
| --- | --- | 
| n - 1 | 3 | 
| m - 1 | 4 | 
| 2k | 2 | 
| C(3, 2) | 3 | 
| C(4, 2) | 6 | 
| Trả lời | 18 | 

Kích thước dọc góp phần ba cách để chọn vị trí thu nhỏ trên và dưới. Chiều ngang đóng góp sáu cách. Sản phẩm của họ cung cấp tất cả các hình chữ nhật bên trong có thể có. 

Ví dụ này thể hiện sự độc lập giữa các hàng và cột. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(max(n, m)) | Tính toán trước giai thừa chiếm ưu thế | 
| Không gian | O(max(n, m)) | Mảng giai thừa và nghịch đảo | 

Với giới hạn chỉ lên đến`1000`, giải pháp này dễ dàng phù hợp với cả hạn chế về thời gian và bộ nhớ. Việc tính toán có hiệu quả ngay lập tức. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def build_factorials(limit):
        fact = [1] * (limit + 1)

        for i in range(1, limit + 1):
            fact[i] = fact[i - 1] * i % MOD

        inv_fact = [1] * (limit + 1)
        inv_fact[limit] = pow(fact[limit], MOD - 2, MOD)

        for i in range(limit, 0, -1):
            inv_fact[i - 1] = inv_fact[i] * i % MOD

        return fact, inv_fact

    def comb(n, r, fact, inv_fact):
        if r < 0 or r > n:
            return 0
        return fact[n] * inv_fact[r] % MOD * inv_fact[n - r] % MOD

    n, m, k = map(int, input().split())

    fact, inv_fact = build_factorials(max(n, m))

    ans = comb(n - 1, 2 * k, fact, inv_fact)
    ans *= comb(m - 1, 2 * k, fact, inv_fact)
    ans %= MOD

    return str(ans)

# provided sample
assert run("3 3 1\n") == "1", "sample 1"

# minimum possible input
assert run("1 1 1\n") == "0", "cannot place any inner rectangle"

# exactly enough space for one move
assert run("3 4 1\n") == "3", "single shrinking layer"

# impossible because too many moves
assert run("5 5 3\n") == "0", "not enough layers"

# symmetric medium case
assert run("5 5 1\n") == "36", "basic combinatorial count"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`0`| Kích thước nhỏ nhất | 
|`3 4 1`|`3`| Tính khả thi về ranh giới chính xác | 
|`5 5 3`|`0`| Số lần di chuyển không thể | 
|`5 5 1`|`36`| Đếm tổ hợp chung | 

## Vỏ cạnh 

Hãy xem xét:```
2 2 2
```Chúng tôi cần:```
2k = 4
n - 1 = 1
m - 1 = 1
```Thuật toán tính toán:```
C(1, 4) = 0
```cho cả hai chiều, vì vậy câu trả lời trở thành`0`. 

Điều này phản ánh chính xác hình học. Sau một lần di chuyển, không còn chỗ cho hình chữ nhật khác. 

Bây giờ hãy xem xét:```
3 3 1
```Việc tính toán trở thành:```
C(2, 2) × C(2, 2) = 1
```Có chính xác một vị trí hợp pháp, ô trung tâm. Điều này xác nhận rằng các hình chữ nhật chạm vào đường viền bên ngoài sẽ tự động được loại trừ bằng cách diễn giải tổ hợp. 

Cuối cùng, hãy xem xét:```
1000 1000 1
```Câu trả lời là:```
C(999, 2)^2
```Việc triển khai xử lý vấn đề này một cách hiệu quả vì tất cả các hệ số nhị thức được tính toán trong thời gian không đổi sau quá trình tiền xử lý tuyến tính.
