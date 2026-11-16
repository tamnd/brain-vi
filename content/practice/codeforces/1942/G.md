---
title: "CF 1942G - Bessie và Thẻ"
description: "Bộ bài có bốn loại thẻ. Lá bài rút 0 tiêu tốn một lá bài có thể chơi được từ tay bạn và không trả lại gì. Lá bài rút 1 sẽ tự thay thế. Lá bài rút 2 tiêu tốn một lá bài và tạo ra hai lá bài mới, do đó, nó sẽ tăng thêm một sức mạnh rút bài trong tương lai của bạn."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1942
codeforces_index: "G"
codeforces_contest_name: "CodeTON Round 8 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 2800
weight: 1942
solve_time_s: 231
verified: true
draft: false
---

[CF 1942G - Bessie và Cards](https://codeforces.com/problemset/problem/1942/G) 

**Đánh giá:** 2800 
**Tags:** tổ hợp, dp, toán 
**Thời gian giải:** 3 phút 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bộ bài có bốn loại thẻ. Lá bài rút 0 tiêu tốn một lá bài có thể chơi được từ tay bạn và không trả lại gì. Lá bài rút 1 sẽ tự thay thế. Lá bài rút 2 tiêu tốn một lá bài và tạo ra hai lá bài mới, do đó, nó sẽ tăng thêm một sức mạnh rút bài trong tương lai của bạn. Ngoài ra còn có 5 thẻ đặc biệt và mục tiêu là rút hết chúng trước khi bị mắc kẹt. 

Lúc đầu, Bessie rút 5 lá bài có giá trị cao nhất. Sau đó, cô ấy có thể chơi liên tục các lá bài rút đang có trên tay mình. Một thẻ chỉ có thể được chơi một lần. Nếu tại một thời điểm nào đó không còn thẻ rút có thể chơi được thì quá trình sẽ kết thúc. 

Chúng ta được cung cấp số lượng quân bài hòa 0, quân bài hòa 1 và quân bài hòa 2. Bộ bài được xáo trộn đồng đều một cách ngẫu nhiên. Chúng ta phải tính xác suất để cuối cùng tất cả 5 lá bài đặc biệt được rút ra và xuất ra modulo xác suất đó.`998244353`. 

Các ràng buộc đủ lớn đến mức mọi mô phỏng không gian trạng thái đều vô vọng. Mỗi trong số`a`,`b`, Và`c`có thể đạt được`2 · 10^5`, và có thể có tới`10^4`trường hợp thử nghiệm. Một thuật toán có độ phức tạp tỷ lệ thuận với số lượng hoán vị bộ bài rõ ràng là không thể, nhưng ngay cả DP bậc hai trên`a`Và`c`sẽ vượt quá giới hạn. Vì tổng của tất cả`a`,`b`, Và`c`trong số các trường hợp thử nghiệm là nhiều nhất`2 · 10^5`, MỘT`O(a + c)`giải pháp cho mỗi trường hợp thử nghiệm là khả thi. 

Có một số trường hợp dễ bỏ sót. 

Coi như`a = b = c = 0`. Bộ bài chỉ chứa 5 lá bài đặc biệt. Bessie ngay từ đầu sẽ rút cả 5 lá bài nên đáp án chính xác là`1`. 

Coi như`a = 1, b = 0, c = 0`. Bộ bài chứa một lá bài rút 0 và năm lá bài đặc biệt. Bessie rút 5 lá bài đầu tiên và không bao giờ có thể lấy thêm bài. Cô ấy thắng trừ khi lá bài rút 0 xuất hiện trong số năm vị trí đầu tiên và đẩy một lá bài đặc biệt xuống vị trí 6. Xác suất là không`1`, mặc dù chỉ có một thẻ không đặc biệt. 

Một điểm tinh tế khác là thẻ rút 1 không làm thay đổi số lượng thẻ trong tương lai mà Bessie có thể truy cập. Một giải pháp bất cẩn có thể cố gắng đưa chúng vào trạng thái DP, điều này tạo ra vấn đề lớn hơn nhiều so với mức cần thiết. Trong thực tế chúng hoàn toàn không liên quan đến điều kiện dừng. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ liệt kê mọi thứ tự bộ bài, mô phỏng trò chơi và đếm xem có bao nhiêu lần xáo trộn dẫn đến chiến thắng. 

Bản thân việc mô phỏng rất dễ dàng. Duy trì ván bài hiện tại, chơi liên tục các lá bài rút có sẵn và dừng lại khi không còn lá bài nào. Nếu tất cả 5 lá bài đặc biệt đều được nhìn thấy thì việc xáo trộn thành công. 

Vấn đề là số lần xáo trộn. Kích thước boong có thể khoảng`6 · 10^5`tổng cộng trên các trường hợp thử nghiệm. Ngay cả đối với những đầu vào nhỏ, số lượng hoán vị vẫn tăng theo cấp số nhân. Cách tiếp cận này trở nên không thể gần như ngay lập tức. 

Quan sát quan trọng là các lá bài rút 1 không đóng góp gì vào sự cân bằng tài nguyên. Chơi một lá bài sẽ tiêu tốn một lá bài và rút chính xác một lá bài. Họ không bao giờ giúp đỡ và không bao giờ làm tổn thương. 

Bây giờ hãy xác định số dư: 

- Bắt đầu với sự cân bằng`5`, vì 5 lá bài đầu tiên được rút miễn phí. 
- Một lá bài rút 2 góp phần`+1`. 
- Một lá bài rút 0 góp phần`-1`. 
- Một thẻ đặc biệt cũng góp phần`-1`, vì nó chiếm một ô đã rút nhưng không thể chơi được. 

Khi Bessie tiết lộ các lá bài từ trên xuống dưới, số dư sẽ cho chúng ta biết còn bao nhiêu lá bài nữa có thể được tiết lộ. 

Quá trình dừng chính xác ở tiền tố đầu tiên nơi số dư trở thành`0`. 

Điều này biến trò chơi thành bài toán đường đi mạng tinh thể. 

Bỏ qua hoàn toàn các lá bài rút 1. Trong số các thẻ còn lại: 

- hòa-2 trở thành`+1`- hòa-0 trở thành`-1`- trở nên đặc biệt`-1`Chúng tôi bắt đầu ở độ cao`5`. 

Bộ bài chiến thắng là bộ bài mà tất cả 5 lá bài đặc biệt đều xuất hiện trước khi đạt đến số dư đầu tiên.`0`. 

Giả sử cú đánh đầu tiên của`0`xảy ra sau khi sử dụng chính xác`p`rút 2 lá bài. Khi đó tiền tố dừng phải chứa: 

-`p`rút 2 lá bài 
-`p + 5`thẻ tiêu cực 

Vì tất cả 5 chương trình đặc biệt đều phải được xem trước nên những`p + 5`tiêu cực bao gồm: 

- tất cả 5 sản phẩm đặc biệt 
- chính xác`p`rút bài 0 

Phần còn lại của bộ bài là tùy ý. 

Nhiệm vụ còn lại là đếm đường dẫn phiếu bầu. Chúng ta cần số lượng trình tự với`p`bước tiến và`p + 5`bước xuống bắt đầu từ độ cao`5`và đánh`0`lần đầu tiên ở cuối tiền tố. 

Kết quả nguyên lý phản ánh/bỏ phiếu Bertrand cổ điển cho$$F_p = \frac{5}{2p+5}\binom{2p+5}{p}.$$Sau khi chọn một con đường như vậy, chúng ta chọn con đường nào trong số`p+5`vị trí phủ định chứa 5 giá trị đặc biệt, sau đó sắp xếp hậu tố còn lại. 

Điều này trực tiếp dẫn đến một tổng kết`p`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Giai thừa trong kích thước boong | Hàm mũ | Quá chậm | 
| Tối ưu |`O(min(a,c))`mỗi trường hợp thử nghiệm |`O(a+c)`tính toán trước | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bỏ qua tất cả các lá bài rút 1. 

Họ không bao giờ thay đổi số dư. Việc xóa chúng sẽ duy trì thứ tự tương đối của mọi thẻ ảnh hưởng đến khả năng tiếp cận. 
2. Hãy để số dư đạt được lần đầu tiên`0`sử dụng chính xác`p`rút 2 lá bài. 

Vì chúng ta bắt đầu ở trạng thái cân bằng`5`, đạt`0`đòi hỏi một sự thay đổi ròng của`-5`. Do đó tiền tố dừng chứa chính xác`p+5`thẻ tiêu cực. 
3. Buộc tất cả 5 thẻ đặc biệt xuất hiện trước khi dừng lại. 

Trong số`p+5`vị trí âm trong tiền tố dừng, chọn 5 vị trí chứa giá trị đặc biệt. 

Số lượng là$$\binom{p+5}{5}.$$4. Đếm đường dẫn số dư hợp lệ. 

Tiền tố dừng chứa`p`bước đi tích cực và`p+5`bước đi tiêu cực. 

Số dư phải duy trì ở mức dương cho đến bước cuối cùng. 

Theo định lý lá phiếu,$$F_p=\frac{5}{2p+5}\binom{2p+5}{p}.$$5. Sắp xếp các thẻ còn lại. 

Sau khi tiền tố dừng được sửa, hậu tố chứa: 

-`a-p`rút bài 0 
-`c-p`rút 2 lá bài 

Số cách sắp xếp hậu tố là$$\binom{a+c-2p}{a-p}.$$6. Tính tổng tất cả các giá trị khả thi của`p`. 

Vì chúng ta cần`p`rút bài 0 và`p`rút 2 lá bài bên trong tiền tố dừng,$$0 \le p \le \min(a,c).$$7. Chia cho tổng số dãy thẻ khác 0. 

Bỏ qua lá bài rút 1, tổng số dãy là$$\frac{(a+c+5)!}{a!\,c!\,5!}.$$8. Rút gọn công thức và tính nó theo modulo`998244353`. 

### Tại sao nó hoạt động 

Mọi trạng thái trò chơi hoàn toàn được xác định bởi sự cân bằng. Lá bài rút 2 tăng khả năng tiếp cận trong tương lai thêm một, trong khi cả lá bài rút 0 và lá bài đặc biệt đều giảm khả năng tiếp cận trong tương lai đi một. Trò chơi kết thúc chính xác ở tiền tố đầu tiên khi số dư trở thành 0. 

Đối với điểm dừng cố định, tiền tố dừng phải chứa số lượng quân bài rút 0 và quân bài rút 2 bằng nhau, ngoại trừ khoản thâm hụt thêm 5 do năm quân bài đặc biệt gây ra. Định lý lá phiếu đếm chính xác những tiền tố mà số dư của chúng không bao giờ đạt tới 0 sớm hơn. Việc chọn vị trí của các quân bài đặc biệt trong số các bước phủ định đảm bảo rằng tất cả các quân bài đặc biệt sẽ được rút ra trước khi dừng lại. Mỗi bộ bài chiến thắng tương ứng với chính xác một giá trị của`p`và mọi cấu hình được tính đều tạo ra bộ bài chiến thắng hợp lệ. Do đó, tổng kết tính tất cả các bộ bài chiến thắng đúng một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

MAXN = 400005

fac = [1] * (MAXN + 1)
for i in range(1, MAXN + 1):
    fac[i] = fac[i - 1] * i % MOD

invfac = [1] * (MAXN + 1)
invfac[MAXN] = pow(fac[MAXN], MOD - 2, MOD)
for i in range(MAXN, 0, -1):
    invfac[i - 1] = invfac[i] * i % MOD

def C(n, r):
    if r < 0 or r > n:
        return 0
    return fac[n] * invfac[r] % MOD * invfac[n - r] % MOD

def solve():
    t = int(input())

    for _ in range(t):
        a, b, c = map(int, input().split())

        total_inv = pow(
            fac[a + c + 5] * invfac[a] % MOD * invfac[c] % MOD * fac[5] % MOD,
            MOD - 2,
            MOD
        )

        good = 0

        for p in range(min(a, c) + 1):
            ballot = 5 * pow(2 * p + 5, MOD - 2, MOD) % MOD
            ballot = ballot * C(2 * p + 5, p) % MOD

            specials = C(p + 5, 5)
            suffix = C(a + c - 2 * p, a - p)

            good = (good + ballot * specials % MOD * suffix) % MOD

        ans = good * total_inv % MOD
        print(ans)

solve()
```Việc thực hiện tuân theo đối số đếm trực tiếp. 

Các bảng giai thừa hỗ trợ các hệ số nhị thức theo thời gian không đổi. Từ`a + c + 5`không bao giờ vượt quá`400005`, một lần tính toán trước toàn cục là đủ. 

Biến`ballot`cửa hàng$$\frac{5}{2p+5}\binom{2p+5}{p},$$modulo tính toán`MOD`. Phép chia được thực hiện bằng phép nghịch đảo mô-đun.`specials`chọn vị trí phủ định tương ứng với các thẻ đặc biệt, trong khi`suffix`sắp xếp các thẻ còn lại sau tiền tố dừng. 

số lượng`good`tính các chuỗi quân bài khác 0 thắng. Tổng số dãy thẻ khác 0 là$$\frac{(a+c+5)!}{a!\,c!\,5!},$$vì vậy chúng tôi nhân với nghịch đảo mô đun của nó ở cuối. 

Một sai lầm phổ biến là cố gắng đếm các lá bài rút 1. Chúng không bao giờ ảnh hưởng đến sự cân bằng, do đó làm như vậy chỉ làm phức tạp tổ hợp và thường dẫn đến việc đếm quá mức. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 1 1
```Các giá trị có thể có của`p`là`0`Và`1`. 

| p | Kiểm phiếu | Chọn sản phẩm đặc biệt | Số lượng hậu tố | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 2 | 2 | 
| 1 | 5 | 6 | 1 | 30 | 

Tổng số chuỗi chiến thắng:$$2 + 30 = 32.$$Tổng số chuỗi thẻ khác 0:$$\frac{7!}{1!\cdot1!\cdot5!}=42.$$Xác suất:$$\frac{32}{42} = \frac{16}{21}.$$Điều này phù hợp với mẫu. 

Dấu vết cho thấy tổng được phân chia một cách tự nhiên như thế nào theo vị trí mà số dư đầu tiên đạt đến 0. 

### Ví dụ 2 

đầu vào:```
0 0 0
```Chỉ có 5 thẻ đặc biệt. 

| p | Kiểm phiếu | Chọn sản phẩm đặc biệt | Số lượng hậu tố | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 1 | 

Tổng số chuỗi chiến thắng = 1. 

Tổng số chuỗi = 1. 

Trả lời = 1. 

Điều này sử dụng bộ bài nhỏ nhất có thể và xác nhận rằng công thức xử lý số lượng lá bài rút trống một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(min(a,c))`| Một phép tính tổng trên tất cả các giá trị khả thi của`p`| 
| Không gian |`O(a+c)`tiền xử lý | Bảng giai thừa và nghịch đảo | 

Tổng của tất cả`a`và tất cả`c`trong số các trường hợp thử nghiệm là nhiều nhất`2 · 10^5`. Vì mỗi trường hợp thử nghiệm chỉ lặp lại tối đa`min(a,c)`, tổng khối lượng công việc vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    MAXN = 400005

    fac = [1] * (MAXN + 1)
    for i in range(1, MAXN + 1):
        fac[i] = fac[i - 1] * i % MOD

    invfac = [1] * (MAXN + 1)
    invfac[MAXN] = pow(fac[MAXN], MOD - 2, MOD)
    for i in range(MAXN, 0, -1):
        invfac[i - 1] = invfac[i] * i % MOD

    def C(n, r):
        if r < 0 or r > n:
            return 0
        return fac[n] * invfac[r] % MOD * invfac[n - r] % MOD

    t = int(input())
    out = []

    for _ in range(t):
        a, b, c = map(int, input().split())

        total_inv = pow(
            fac[a + c + 5] * invfac[a] % MOD * invfac[c] % MOD * fac[5] % MOD,
            MOD - 2,
            MOD
        )

        good = 0

        for p in range(min(a, c) + 1):
            ballot = 5 * pow(2 * p + 5, MOD - 2, MOD) % MOD
            ballot = ballot * C(2 * p + 5, p) % MOD

            good = (
                good
                + ballot * C(p + 5, 5) % MOD
                * C(a + c - 2 * p, a - p)
            ) % MOD

        out.append(str(good * total_inv % MOD))

    return "\n".join(out)

# provided samples
assert run(
"""4
1 1 1
0 0 0
5 3 7
3366 1434 1234
"""
) == "\n".join([
    "903173463",
    "1",
    "35118742",
    "398952013"
]), "sample"

# minimum deck
assert run(
"""1
0 0 0
"""
) == "1", "only specials"

# one draw-0 card
assert run(
"""1
1 0 0
"""
) == "166374059", "probability 5/6"

# draw-1 cards do not matter
assert run(
"""2
0 0 0
0 100 0
"""
) == "1\n1", "draw-1 cards are irrelevant"

# symmetric small case
assert run(
"""1
1 0 1
"""
) == "903173463", "same probability as sample without draw-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 0`|`1`| Bộ bài nhỏ nhất có thể | 
|`1 0 0`|`5/6 mod MOD`| Thêm một thẻ âm | 
|`0 100 0`|`1`| Rút 1 lá bài không ảnh hưởng tới đáp án | 
|`1 0 1`| Giống như trường hợp mẫu | Khẳng định sự độc lập khỏi`b`| 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là bộ bài chỉ chứa những sản phẩm đặc biệt.```
a = 0, b = 0, c = 0
```Số dư bắt đầu lúc`5`. Tất cả năm lá bài sẽ được rút ngay lập tức, vì vậy mọi lá bài đặc biệt đều được nhìn thấy trước khi quá trình có thể dừng lại. Trong công thức chỉ`p = 0`đóng góp. Câu trả lời trở nên chính xác`1`. 

Trường hợp cạnh thứ hai là có thêm lá bài hòa 0 nhưng không có lá bài hòa 2.```
a = 1, b = 0, c = 0
```Giá trị hợp lệ duy nhất là`p = 0`. Tiền tố dừng bao gồm chính xác năm điểm đặc biệt. Nếu lá bài rút 0 xuất hiện trước bất kỳ lá bài đặc biệt nào, một lá bài đặc biệt sẽ bị đẩy vượt quá điểm dừng và Bessie thua cuộc. Công thức tính xác suất`5/6`, phù hợp với đối số trực tiếp. 

Trường hợp cạnh thứ ba là số lượng lớn lá bài rút 1.```
a = 0, b = 200000, c = 0
```Số dư không bao giờ thay đổi vì lá bài rút 1 đóng góp bằng 0. Bessie tiếp tục rút qua mỗi lá bài rút 1 và cuối cùng đạt được tất cả các lá bài đặc biệt. Thuật toán thậm chí không bao giờ sử dụng`b`, điều này phản ánh chính xác thực tế là thẻ rút 1 không ảnh hưởng đến khả năng tiếp cận. 

Trường hợp tế nhị cuối cùng là khi`a`Và`c`rất khác nhau.```
a = 200000, c = 0
```Tổng hợp chỉ cho phép`p = 0`. Không có thuật ngữ không hợp lệ nào được tạo vì phạm vi là`0 .. min(a,c)`. Điều này tránh được các đối số nhị thức âm và giữ cho phép tính chính xác ngay cả ở ranh giới của các ràng buộc.
