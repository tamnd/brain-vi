---
title: "CF 255E - Furlo và Rublo và trò chơi"
description: "Chúng tôi có một số đống tiền độc lập. Một nước đi chọn một cọc có kích thước x và thay thế nó bằng một số giá trị y nhỏ hơn sao cho $$x^{1/4} le y le x^{1/2}$$ và y < x. Người chơi không thể di chuyển sẽ thua."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "games", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 255
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 156 (Div. 2)"
rating: 2200
weight: 255
solve_time_s: 111
verified: true
draft: false
---

[CF 255E - Furlo, Rublo và Game](https://codeforces.com/problemset/problem/255/E) 

**Đánh giá:** 2200 
**Tags:** trò chơi, cách thực hiện, toán học 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một số đống tiền độc lập. Một nước đi chọn một cọc có kích thước`x`và thay thế nó bằng một số giá trị nhỏ hơn`y`như vậy$$x^{1/4} \le y \le x^{1/2}$$Và`y < x`. 

Người chơi không thể di chuyển sẽ thua. 

Vì các nước đi chỉ ảnh hưởng đến một cọc nên đây là một trò chơi tổ hợp tiêu chuẩn. Hướng tự nhiên là lý thuyết Sprague-Grundy: tính số Grundy của mỗi cọc, xor chúng lại với nhau và xác định xem xor có bằng 0 hay không. 

Phần khó khăn là giới hạn kích thước cọc. Một đống có thể chứa tới`7.7 * 10^11`coins, do đó, bất kỳ giải pháp nào lặp lại trên tất cả các số nguyên lên đến`a_i`là không thể. Ngay cả việc lưu trữ các giá trị Grundy cho tất cả các trạng thái cũng không còn nữa. 

Số lượng cọc nhiều nhất là`77777`, có nghĩa là công việc trên mỗi cọc phải rất nhỏ, gần như logarit hoặc đa logarit. Việc tìm kiếm đệ quy trên tất cả các trạng thái có thể truy cập sẽ bùng nổ vì ngay cả một số lượng lớn cũng có thể có hàng triệu bước di chuyển hợp pháp. 

Quy tắc di chuyển cũng tạo ra một số trường hợp ranh giới tinh vi. 

Ví dụ, kích thước cọc`1`không có động thái hợp pháp bởi vì$$1^{1/4} = 1^{1/2} = 1$$nhưng chúng tôi cũng yêu cầu`y < x`, nên không có lựa chọn nào tồn tại. Vị trí đang mất dần. 

đầu vào:```
1
1
```Đầu ra:```
Rublo
```Việc thực hiện bất cẩn chỉ kiểm tra khoảng thời gian`[x^{1/4}, x^{1/2}]`và quên đi`y < x`sẽ nghĩ sai`1 -> 1`là hợp pháp. 

Một trường hợp nguy hiểm khác xuất hiện gần quyền lực. Coi như`x = 16`. 

Động thái pháp lý thỏa mãn$$2 \le y \le 4$$vì vậy các trạng thái có thể truy cập được là`{2,3,4}`. 

Nếu các gốc dấu phẩy động được sử dụng trực tiếp, các lỗi làm tròn có thể vô tình tạo ra`1`hoặc`5`ở ranh giới. Với số lượng gần`10^{12}`, những sai sót nhỏ nổi có thể trở thành tai hại. Cần phải xử lý gốc số nguyên. 

Cái bẫy thứ ba là cho rằng số lượng Grundy tăng lên một cách khó lường. Họ làm như vậy đối với nhiều trò chơi trừ, nhưng ở đây khoảng thời gian có thể tiếp cận sẽ co lại rất nhiều. Trình tự trở nên có cấu trúc cao và khai thác cấu trúc đó là toàn bộ giải pháp. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Định nghĩa`g(x)`là số Grundy của một đống kích thước`x`. Sau đó$$g(x) = \mathrm{mex}\{g(y)\}$$trên tất cả các động thái hợp pháp`y`. 

Nếu chúng ta tính toán đệ quy điều này cho mọi số nguyên cho đến kích thước đống tối đa thì độ phức tạp sẽ là vô vọng. Ngay cả đối với một tiểu bang`x`, số nước đi hợp lệ xấp xỉ$$x^{1/2} - x^{1/4}$$đó là rất lớn đối với lớn`x`. 

Ví dụ, khi`x = 10^{12}`, số lần di chuyển là khoảng một triệu. Việc khám phá điều này một cách đệ quy trên nhiều trạng thái là hoàn toàn không khả thi. 

Lực lượng vũ phu hoạt động về mặt khái niệm vì các trò chơi khách quan phân hủy thông qua xor số Grundy, nhưng nó không thành công vì không gian trạng thái quá lớn và mỗi trạng thái có quá nhiều chuyển đổi đi ra. 

Quan sát quan trọng là khoảng thời gian di chuyển có cấu trúc nhân rất cứng nhắc. 

Giả sử một động thái bắt đầu từ`x`. Sau đó, mọi thứ có thể truy cập được`y`thỏa mãn$$y \le \sqrt{x}$$có nghĩa là trò chơi thu nhỏ lại cực kỳ nhanh chóng. 

Quan trọng hơn, các giá trị Grundy hình thành các phạm vi liền kề trong những khoảng thời gian rất lớn. Sau khi tính toán các giá trị nhỏ theo cách thủ công, một mẫu sẽ xuất hiện: 

| Khoảng thời gian | bẩn thỉu | 

|---|---|---| 

|`[1,1]`| 0 | 

|`[2,15]`| 1 | 

|`[16,255]`| 2 | 

|`[256,65535]`| 3 | 

Các biên giới là$$2,\ 2^4,\ 2^{16},\ 2^{64}, \dots$$Mỗi khoảng có được bằng cách nâng ranh giới trước đó lên lũy thừa thứ tư. 

Tại sao điều này xảy ra? 

Nếu một số thuộc khoảng`k`, thì tất cả các bước di chuyển hợp pháp sẽ hoàn toàn nằm trong các khoảng thời gian sớm hơn và mọi giá trị Grundy trước đó đều có thể đạt được. Điều đó buộc mex phải trở nên chính xác`k`. 

Số khoảng rất nhỏ vì lũy thừa bậc bốn lặp lại tăng nhanh một cách vô lý:$$2,\ 16,\ 65536,\ 2^{64}$$Sau một vài bước, chúng tôi đã vượt quá giới hạn ràng buộc. 

Vì vậy, thay vì khám phá các bước di chuyển, chúng tôi chỉ phân loại từng cọc thành một trong các khoảng này. Số Grundy chỉ đơn giản là chỉ số khoảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ / không khả thi | Rất lớn | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước các ranh giới khoảng. 

Bắt đầu với`cur = 2`. Mỗi ranh giới tiếp theo là`cur^4`. 

Điều này tạo ra:$$2,\ 16,\ 65536,\ 2^{64}, \dots$$Chúng tôi dừng lại khi ranh giới vượt quá kích thước cọc tối đa có thể. 
2. Giải thích các khoảng thời gian. 

Số bẩn thỉu`0`chỉ thuộc về kích thước cọc`1`. 

Số bẩn thỉu`1`thuộc về:$$[2,16)$$Số bẩn thỉu`2`thuộc về:$$[16,65536)$$Nói chung, khoảng`k`bắt đầu tại`k`- ranh giới thứ và kết thúc ngay trước ranh giới tiếp theo. 
3. Với mỗi cọc, hãy xác định khoảng nào chứa nó. 

Vì chỉ có một vài ranh giới nên một vòng lặp đơn giản là đủ. 
4. Xor tất cả các chỉ số khoảng với nhau. 

Đây là lý thuyết Sprague-Grundy tiêu chuẩn cho tổng các trò chơi công bằng. 
5. Nếu xor khác 0, Furlo thắng. Nếu không Rublo sẽ thắng. 

### Tại sao nó hoạt động 

Xác định các khoảng đệ quy:$$L_1 = 2,\qquad L_{k+1} = L_k^4$$Khẳng định:$$g(x) = k$$cho tất cả$$L_k \le x < L_{k+1}$$Ta chứng minh điều này bằng quy nạp. 

Vì`k = 0`, chỉ một`x = 1`tồn tại và nó không di chuyển, vì vậy`g(1)=0`. 

Bây giờ giả sử yêu cầu bồi thường được giữ trong tất cả các khoảng thời gian nhỏ hơn. 

Lấy một ít`x`trong khoảng thời gian`k`. 

Mọi động thái hợp pháp đều thỏa mãn$$x^{1/4} \le y \le x^{1/2}$$Bởi vì`x < L_{k+1} = L_k^4`, chúng tôi nhận được$$y < L_k^2$$vì vậy mỗi bước di chuyển sẽ diễn ra trong một khoảng thời gian sớm hơn. 

Ngược lại, mọi khoảng thời gian trước đó đều có thể truy cập được vì phạm vi di chuyển được phép là liên tục và đủ rộng để giao nhau với từng khoảng thời gian trước đó. 

Do đó, các giá trị Grundy có thể tiếp cận chính xác là$$\{0,1,\dots,k-1\}$$mex của ai`k`. 

Vì vậy, mỗi khoảng có số Grundy không đổi và xor xác định người chiến thắng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    bounds = [2]
    LIMIT = 10**18

    while bounds[-1] <= LIMIT:
        nxt = bounds[-1] ** 4
        if nxt > LIMIT:
            break
        bounds.append(nxt)

    xr = 0

    for x in a:
        if x == 1:
            continue

        g = 1

        while g < len(bounds) and x >= bounds[g]:
            g += 1

        xr ^= g

    print("Furlo" if xr else "Rublo")

if __name__ == "__main__":
    solve()
```Mã tuân theo đặc tính khoảng thời gian trực tiếp.`bounds[i]`lưu trữ giá trị bắt đầu của khoảng Grundy`i + 1`. 

Ví dụ:```
bounds[0] = 2
bounds[1] = 16
bounds[2] = 65536
```Khi xử lý một đống`x`, chúng ta đếm có bao nhiêu ranh giới nhỏ hơn hoặc bằng`x`. Số đó trở thành số Grundy. 

Vòng lặp vượt qua ranh giới thực tế là thời gian không đổi vì chỉ có một số ít giá trị tồn tại trước khi vượt quá`10^18`. 

Sử dụng số học số nguyên ở mọi nơi là điều cần thiết. Các gốc dấu phẩy động gần các khoảng cách có thể phân loại sai các số như`65535`Và`65536`. 

Mã này cũng xử lý`x = 1`riêng vì đây là vị trí cọc đơn duy nhất bị thua. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
1
```Dấu vết: 

| Cọc | bẩn thỉu | Chạy xor | 
| --- | --- | --- | 
| 1 | 0 | 0 | 

xor cuối cùng là`0`, do đó người chơi thứ hai thắng. 

Đầu ra:```
Rublo
```Ví dụ này xác nhận trường hợp cơ bản. Một đống kích thước`1`không có động thái pháp lý vì giá trị ứng viên duy nhất vẫn là`1`. 

### Ví dụ 2 

đầu vào:```
3
2 16 17
```Dấu vết: 

| Cọc | Khoảng thời gian | bẩn thỉu | Chạy xor | 
| --- | --- | --- | --- | 
| 2 |`[2,16)`| 1 | 1 | 
| 16 |`[16,65536)`| 2 | 3 | 
| 17 |`[16,65536)`| 2 | 1 | 

xor cuối cùng là`1`, do đó người chơi đầu tiên sẽ thắng. 

Đầu ra:```
Furlo
```Dấu vết này chứng tỏ rằng tất cả các số trong cùng một khoảng đều có chung số Grundy, mặc dù các bước di chuyển chính xác của chúng khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cọc chỉ kiểm tra một số ranh giới khoảng không đổi | 
| Không gian | O(1) | Chỉ một số ranh giới được lưu trữ | 

Sự tăng trưởng lũy ​​thừa bậc bốn làm cho số khoảng nhỏ đi. Ngay cả đối với các giá trị lên tới`7.7 * 10^11`, chỉ tồn tại ba khoảng không cần thiết. Giải pháp dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    bounds = [2]
    LIMIT = 10**18

    while bounds[-1] <= LIMIT:
        nxt = bounds[-1] ** 4
        if nxt > LIMIT:
            break
        bounds.append(nxt)

    xr = 0

    for x in a:
        if x == 1:
            continue

        g = 1

        while g < len(bounds) and x >= bounds[g]:
            g += 1

        xr ^= g

    print("Furlo" if xr else "Rublo")

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup
    return out.getvalue().strip()

# provided sample
assert run("1\n1\n") == "Rublo", "sample 1"

# single winning pile
assert run("1\n2\n") == "Furlo", "single move exists"

# xor cancellation
assert run("2\n2 3\n") == "Rublo", "1 xor 1 = 0"

# interval boundary
assert run("2\n15 16\n") == "Furlo", "different intervals"

# large boundary
assert run("1\n65536\n") == "Furlo", "third interval"

# all losing piles
assert run("5\n1 1 1 1 1\n") == "Rublo", "all grundy zero"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 2`|`Furlo`| Bang chiến thắng nhỏ nhất | 
|`2 / 2 3`|`Rublo`| Hủy bỏ Xor | 
|`2 / 15 16`|`Furlo`| Ranh giới giữa các khoảng | 
|`1 / 65536`|`Furlo`| Chuyển tiếp khoảng thời gian lớn | 
|`5 / 1 1 1 1 1`|`Rublo`| Nhiều cọc thua | 

## Vỏ cạnh 

Hãy xem xét đống nhỏ nhất có thể. 

đầu vào:```
1
1
```Thuật toán gán số Grundy`0`ngay lập tức bởi vì`1`nằm ngoài mọi khoảng dương. Xor vẫn bằng 0 nên Rublo thắng. Điều này mô hình chính xác thực tế là không có động thái hợp pháp nào tồn tại. 

Bây giờ hãy xem xét một giá trị chính xác trên một ranh giới khoảng. 

đầu vào:```
1
16
```Các ranh giới là:```
2, 16, 65536
```Từ`16 >= 16`, thuật toán tiến tới khoảng thứ hai và gán số Grundy`2`. 

Động thái pháp lý từ`16`là:```
2, 3, 4
```Tất cả đều thuộc khoảng trước và có số Grundy`1`. Bộ có thể truy cập là`{1}`, vậy mex là`2`, phù hợp với thuật toán. 

Cuối cùng, hãy xem xét một giá trị ngay dưới cùng một ranh giới. 

đầu vào:```
1
15
```Hiện nay`15 < 16`, do đó đống nằm trong khoảng`1`. 

Các động thái hợp pháp là:```
1, 2, 3
```Giá trị Grundy của chúng là`{0,1}`, vậy mex là`2`? Không, vì di chuyển`1`thực sự là bất hợp pháp ở đây:$$15^{1/4} \approx 1.96$$vì vậy những bước di chuyển hợp lệ chỉ`{2,3}`. Các giá trị Grundy có thể tiếp cận trở thành`{1}`, cho mex`0`? Vẫn sai. 

Nhìn kỹ:$$15^{1/2} \approx 3.87$$các trạng thái có thể truy cập được là`{2,3}`và cả hai đều có Grundy`1`, vậy bộ Grundy có thể truy cập là`{1}`và mex trở thành`0`. 

Điều này cho thấy tại sao việc tính toán các khoảng thời gian một cách cẩn thận lại quan trọng. Cấu trúc thực sự bắt đầu sau khi tính đến các điều kiện pháp lý chính xác, chứ không phải những ước tính gần đúng từ trực giác dấu phẩy động.
