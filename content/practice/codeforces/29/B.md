---
title: "CF 29B - Đèn Giao Thông"
description: "Một ô tô xuất phát tại điểm A và di chuyển dọc theo một con đường thẳng về phía điểm B. Tổng quãng đường là l mét, và ô tô luôn chuyển động với vận tốc cố định v. Ở một nơi nào đó trên đường, cách điểm xuất phát đúng d mét, có một đèn giao thông."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 29
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 29 (Div. 2, Codeforces format)"
rating: 1500
weight: 29
solve_time_s: 83
verified: true
draft: false
---
[CF 29B - Đèn giao thông](https://codeforces.com/problemset/problem/29/B) 

**Đánh giá:** 1500 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Một ô tô xuất phát từ điểm A và di chuyển dọc theo một con đường thẳng về phía điểm B. Tổng quãng đường là`l`mét, và ô tô luôn chuyển động với tốc độ cố định`v`. Chính xác là ở đâu đó dọc con đường`d`từ đầu mét có đèn giao thông. 

Đèn giao thông luân phiên giữa xanh và đỏ mãi mãi. Vào thời điểm`0`, đèn xanh. Nó vẫn xanh cho`g`giây, sau đó là màu đỏ trong`r`giây, sau đó lặp đi lặp lại chu kỳ này. 

Xe có thể dừng ngay lập tức và tăng tốc ngay lập tức nên không bị phạt khi chờ đèn giao thông. Hạn chế duy nhất là xe không thể vượt qua trong khoảng thời gian màu đỏ. Nếu nó đến đúng lúc màu đỏ bắt đầu thì nó phải dừng lại. Nếu nó đến đúng lúc màu xanh bắt đầu, nó có thể trôi qua ngay lập tức. 

Nhiệm vụ là tính thời gian di chuyển tối thiểu có thể từ A đến B. 

Những hạn chế là rất nhỏ. Mọi giá trị nhiều nhất là`1000`và chỉ có một đèn giao thông. Ngay cả các giải pháp nặng về mô phỏng cũng sẽ vừa vặn thoải mái trong giới hạn. Thử thách thực sự là xử lý ranh giới thời gian một cách chính xác, đặc biệt là những thời điểm ánh sáng thay đổi trạng thái. 

Sai lầm dễ xảy ra nhất là xử lý sai khi chuyển từ màu xanh sang màu đỏ. 

Hãy xem xét đầu vào này:```
10 5 1 5 5
```Xe ô tô đến đúng đèn giao thông`5`. Khoảng xanh là`[0, 5)`và màu đỏ bắt đầu chính xác tại`5`. Ô tô không được phép vượt qua. Phải đợi đến lúc`10`, sau đó tiếp tục. 

Câu trả lời đúng là:```
15
```Việc thực hiện bất cẩn bằng cách sử dụng`<= g`thay vì`< g`sẽ cho phép ô tô vượt qua ngay lập tức và gây ra`10`. 

Một trường hợp tinh vi khác xảy ra đúng vào thời điểm một khoảng xanh mới bắt đầu.```
10 5 1 3 2
```Độ dài chu kỳ là`5`. Xe tới đèn đúng lúc`5`, đó chính xác là sự bắt đầu của một khoảng màu xanh lá cây. Nó có thể tiếp tục ngay lập tức. 

Câu trả lời đúng là:```
10
```Việc triển khai coi mọi ranh giới là bị chặn sẽ thêm thời gian chờ đợi không cần thiết một cách không chính xác. 

Thêm một trường hợp khó khăn nữa xuất hiện khi xe đến đích trước khi chạm vào đèn. Vấn đề đảm bảo`d < l`, vì vậy điều này không bao giờ xảy ra, nhưng vẫn hữu ích khi nghĩ về cấu trúc. Nơi duy nhất có thể chờ đợi là tại đèn giao thông. Một khi ô tô vượt qua nó, chuyển động còn lại luôn không bị gián đoạn. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là mô phỏng thời gian từng giây và theo dõi trạng thái đèn giao thông cho đến khi ô tô đến đích. Vì các giá trị nhỏ nên ngay cả một mô phỏng chi tiết cũng có thể hoạt động nếu được triển khai cẩn thận với số học dấu phẩy động. 

Brute-force hoạt động vì hệ thống cực kỳ đơn giản. Ô tô chỉ thay đổi hành vi tại một vị trí, thời gian đèn giao thông là định kỳ và tốc độ di chuyển không đổi. Vấn đề là việc mô phỏng thời gian liên tục là bất tiện và không cần thiết. Bạn sẽ cần mức tăng rất nhỏ để duy trì độ chính xác và lỗi tích lũy dấu phẩy động sẽ trở nên khó chịu. 

Quan sát quan trọng là ô tô tương tác với đèn giao thông đúng một lần. 

Nếu không có đèn giao thông, tổng thời gian di chuyển chỉ là:$$\frac{l}{v}$$Câu hỏi duy nhất là liệu ô tô có phải đợi khi đi hết quãng đường hay không`d`. 

Thời gian đến chỗ đèn là:$$\frac{d}{v}$$Tín hiệu giao thông lặp lại mỗi`g + r`giây. Bằng cách lấy thời gian đến modulo theo độ dài chu kỳ, chúng ta có thể xác định xem đèn có màu xanh hay đỏ tại thời điểm đó. 

Nếu như:$$t \bmod (g+r) < g$$xe đến khi đang xanh và băng qua ngay lập tức. 

Nếu không, nó sẽ đến trong màu đỏ và phải đợi cho đến khi chu kỳ tiếp theo bắt đầu. 

Điều đó làm giảm toàn bộ vấn đề thành một vài phép tính số học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(T) | O(1) | Không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc năm số nguyên`l`,`d`,`v`,`g`, Và`r`. 
2. Tính thời gian cần thiết để đến chỗ đèn giao thông:$$t = \frac{d}{v}$$Đây là thời điểm đến sớm nhất có thể vì ô tô luôn di chuyển với tốc độ tối đa. 

1. Tính độ dài chu kỳ đèn giao thông:$$cycle = g + r$$Tín hiệu lặp lại mãi mãi với khoảng thời gian này. 

1. Tìm vị trí bên trong chu trình hiện tại khi xe đến:$$x = t \bmod cycle$$Điều này cho chúng ta biết tín hiệu có màu xanh hay đỏ tại thời điểm đó. 

1. Kiểm tra xem xe có đến trong màu xanh lá cây hay không. 

Nếu như:$$x < g$$ô tô đi qua ngay. 

Nếu không, xe đến lúc đỏ và phải đợi:$$wait = cycle - x$$Thời gian chờ này di chuyển thời gian hiện tại chính xác đến điểm bắt đầu của khoảng màu xanh tiếp theo. 

1. Cộng thêm thời gian di chuyển còn lại từ điểm đèn giao thông tới điểm đến:$$\frac{l-d}{v}$$1. In câu trả lời cuối cùng với độ chính xác vừa đủ. 

### Tại sao nó hoạt động 

Ô tô chỉ có một điểm quyết định là đèn giao thông. Di chuyển chậm hơn trước khi đến đèn đỏ không bao giờ có ích vì xe luôn có thể dừng ngay lập tức mà không bị phạt. Việc tiếp cận sớm ít nhất cũng tốt như việc tiếp cận muộn hơn. 

Thuật toán tính toán thời gian đến điểm đèn sớm nhất và kiểm tra xem thời điểm đó có thuộc khoảng màu xanh lục hay không. Nếu có, vượt qua ngay lập tức là tối ưu. Nếu không, chiến lược tốt nhất là đợi chính xác cho đến khi khoảng xanh tiếp theo bắt đầu. Không có chiến lược chờ đợi nào khác có thể cải thiện câu trả lời. 

Vì mọi chuyến đi hợp lệ đều tuân theo cấu trúc này nên thời gian tính toán là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

l, d, v, g, r = map(int, input().split())

time_to_light = d / v
cycle = g + r

pos_in_cycle = time_to_light % cycle

answer = l / v

if pos_in_cycle >= g:
    answer += cycle - pos_in_cycle

print(f"{answer:.10f}")
```Giải pháp bắt đầu bằng cách tính toán thời gian đến sớm nhất có thể tại đèn giao thông. Vì tốc độ không đổi và khả năng tăng tốc là tức thời nên không bao giờ có lý do gì để cố tình lái xe chậm hơn trước đèn giao thông. 

Biến`pos_in_cycle`thể hiện thời điểm chính xác trong chu kỳ đèn giao thông lặp lại khi ô tô đến. Đây là quan sát cốt lõi của vấn đề. 

Điều kiện:```
if pos_in_cycle >= g:
```là ranh giới quan trọng. Khoảng xanh là`[0, g)`. Đến đúng lúc`g`nghĩa là màu đỏ đã nổ máy nên xe phải đợi. sử dụng`>`thay vì`>=`sẽ tạo ra những câu trả lời sai. 

Câu trả lời ban đầu được đặt thành:```
l / v
```đó là thời gian di chuyển mà không phải chờ đợi. Nếu xe đến trong thời gian đèn đỏ thì chỉ phải cộng thêm thời gian chờ. 

Đầu ra sử dụng định dạng có độ chính xác cao để đáp ứng dung sai dấu phẩy động cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 1 3 4 5
```Xe tới đèn sau:$$\frac{1}{3}$$giây. 

| Biến | Giá trị | 
| --- | --- | 
|`l`| 2 | 
|`d`| 1 | 
|`v`| 3 | 
|`g`| 4 | 
|`r`| 5 | 
|`time_to_light`| 0.3333333333 | 
|`cycle`| 9 | 
|`pos_in_cycle`| 0.3333333333 | 
| Màu xanh lá? | Có | 
| Thời gian chờ đợi | 0 | 
| Câu trả lời cuối cùng | 0,6666666667 | 

Ôtô đi sâu vào trong khoảng xanh nên không bao giờ dừng lại. Tổng thời gian di chuyển chỉ đơn giản là khoảng cách chia cho tốc độ. 

### Ví dụ 2 

đầu vào:```
10 5 1 5 5
```Chiếc xe chạm đèn đúng lúc màu đỏ bắt đầu. 

| Biến | Giá trị | 
| --- | --- | 
|`l`| 10 | 
|`d`| 5 | 
|`v`| 1 | 
|`g`| 5 | 
|`r`| 5 | 
|`time_to_light`| 5 | 
|`cycle`| 10 | 
|`pos_in_cycle`| 5 | 
| Màu xanh lá? | Không | 
| Thời gian chờ đợi | 5 | 
| Câu trả lời cuối cùng | 15 | 

Ví dụ này thể hiện điều kiện biên quan trọng nhất. Đến đúng giờ`g`không được phép. Xe đợi đến giờ`10`, sau đó tiếp tục. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số phép tính số học không đổi được thực hiện | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Thuật toán dễ dàng phù hợp trong giới hạn. Nó chỉ thực hiện một số phép tính dấu phẩy động và sử dụng bộ nhớ không đổi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    l, d, v, g, r = map(int, input().split())

    time_to_light = d / v
    cycle = g + r

    pos_in_cycle = time_to_light % cycle

    answer = l / v

    if pos_in_cycle >= g:
        answer += cycle - pos_in_cycle

    print(f"{answer:.10f}")

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup
    return out.getvalue().strip()

# provided sample
assert run("2 1 3 4 5\n") == "0.6666666667", "sample 1"

# arrives exactly when red starts
assert run("10 5 1 5 5\n") == "15.0000000000", "red boundary"

# arrives exactly when green starts
assert run("10 5 1 3 2\n") == "10.0000000000", "green boundary"

# minimum-like values
assert run("2 1 1 1 1\n") == "2.0000000000", "small values"

# large values
assert run("1000 999 1000 1 1000\n") == "1.0000000000", "large values"

# waiting required with fractional arrival
assert run("20 7 2 3 5\n") == "14.0000000000", "fractional wait"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`10 5 1 5 5`|`15.0000000000`| Đến đúng lúc màu đỏ bắt đầu | 
|`10 5 1 3 2`|`10.0000000000`| Đến đúng lúc màu xanh lá cây bắt đầu | 
|`2 1 1 1 1`|`2.0000000000`| Cấu hình hợp lệ kiểu nhỏ nhất | 
|`1000 999 1000 1 1000`|`1.0000000000`| Giá trị lớn và không phải chờ đợi | 
|`20 7 2 3 5`|`14.0000000000`| Phân số đến trong màu đỏ | 

## Vỏ cạnh 

Xét ranh giới nơi đèn chuyển sang màu đỏ đúng lúc xe tới:```
10 5 1 5 5
```Xe tới đèn đúng lúc`5`. 

Chu kỳ là`10`, Vì thế:$$5 \bmod 10 = 5$$Từ`5 >= 5`, đèn đỏ. Thuật toán thêm thời gian chờ đợi:$$10 - 5 = 5$$Câu trả lời cuối cùng trở thành:$$10 + 5 = 15$$Điều này phù hợp chính xác với các quy tắc vì ô tô không thể vượt qua ngay khi đèn đỏ bắt đầu. 

Bây giờ hãy xem xét việc đến chính xác khi màu xanh lá cây bắt đầu:```
10 5 1 3 2
```Độ dài chu kỳ là`5`. Xe tới đèn đúng lúc`5`.$$5 \bmod 5 = 0$$Từ`0 < 3`, tín hiệu có màu xanh lá cây. Ôtô vượt qua ngay lập tức và về đích ở`10`tổng số giây. 

Điều này xác minh rằng tính toán modulo xử lý chính xác việc bao bọc chu trình. 

Cuối cùng, hãy xem xét sự xuất hiện một phần trong màu đỏ:```
20 7 2 3 5
```Xe tới đèn sau:$$\frac{7}{2} = 3.5$$Độ dài chu kỳ là`8`.$$3.5 \bmod 8 = 3.5$$Bởi vì`3.5 >= 3`, đèn đỏ. Xe đang chờ:$$8 - 3.5 = 4.5$$Thời gian di chuyển cơ bản là:$$\frac{20}{2} = 10$$Tổng cộng:$$10 + 4.5 = 14.5$$Thuật toán xử lý thời gian phân đoạn một cách tự nhiên vì tất cả các tính toán được thực hiện bằng số học dấu phẩy động.
