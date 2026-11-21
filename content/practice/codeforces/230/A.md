---
title: "CF 230A - Rồng"
description: "Kirito bắt đầu với một số sức mạnh ban đầu và phải đánh bại mọi con rồng trong cấp độ. Mỗi con rồng có hai giá trị: sức mạnh tối thiểu cần thiết để đánh bại nó và sức mạnh cộng thêm mà Kirito nhận được sau đó."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 230
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 142 (Div. 2)"
rating: 1000
weight: 230
solve_time_s: 208
verified: true
draft: false
---

[CF 230A - Rồng](https://codeforces.com/problemset/problem/230/A) 

**Đánh giá:** 1000 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 3 phút 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Kirito bắt đầu với một số sức mạnh ban đầu và phải đánh bại mọi con rồng trong cấp độ. Mỗi con rồng có hai giá trị: sức mạnh tối thiểu cần thiết để đánh bại nó và sức mạnh cộng thêm mà Kirito nhận được sau đó. 

Một cuộc chiến chỉ có thể xảy ra nếu sức mạnh hiện tại của Kirito hoàn toàn lớn hơn sức mạnh của con rồng. Sức mạnh tương đương là không đủ. Sau khi chiến thắng, Kirito ngay lập tức trở nên mạnh mẽ hơn và có thể đánh bại được những con rồng cứng rắn hơn sau này. 

Thứ tự chiến đấu không cố định. Chúng ta phải quyết định xem liệu có tồn tại một số loài rồng để Kirito đánh bại tất cả chúng mà không bao giờ thua hay không. 

Những hạn chế là rất nhỏ. Có nhiều nhất$10^3$rồng, thậm chí$O(n^2)$hoặc$O(n^3)$các giải pháp sẽ thoải mái phù hợp trong giới hạn. Điều đó có nghĩa là thách thức thực sự không phải là tối ưu hóa mà là nhận ra chiến lược tham lam đúng đắn. 

Phần nguy hiểm của vấn đề là sự so sánh chặt chẽ. Kirito phải có sức mạnh lớn hơn sức mạnh của rồng. Việc thực hiện bất cẩn bằng cách sử dụng`>=`thay vì`>`âm thầm tạo ra những câu trả lời sai. 

Hãy xem xét đầu vào này:```
5 1
5 10
```Đầu ra đúng là:```
NO
```Sức mạnh của Kirito ngang bằng với sức mạnh của rồng nên anh ta thua ngay lập tức. sử dụng`>=`sẽ chấp nhận trường hợp này một cách sai lầm. 

Một trường hợp tinh tế khác là chọn rồng không đúng thứ tự.```
3 2
2 1
4 100
```Đầu ra đúng là:```
YES
```Nếu Kirito chiến đấu với con rồng mạnh hơn trước, anh ấy sẽ thua. Nếu anh ta chiến đấu với con rồng yếu hơn trước, sức mạnh của anh ta sẽ trở nên$4$, điều đó vẫn chưa đủ vì sự so sánh rất nghiêm ngặt. Vì vậy, ví dụ này thực sự là:```
NO
```Thay đổi phần thưởng đầu tiên một chút sẽ mang lại:```
3 2
2 2
4 100
```Bây giờ đầu ra đúng là:```
YES
```Sau khi đánh bại con rồng đầu tiên, sức mạnh của Kirito trở nên$5$, đủ để đánh bại con rồng thứ hai. 

Một cách tiếp cận ngây thơ, không suy nghĩ cẩn thận về trật tự chiến đấu có thể dễ dàng thất bại trong quá trình chuyển đổi ranh giới này. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực là thử mọi mệnh lệnh có thể có của rồng. Đối với mỗi hoán vị, chúng tôi mô phỏng các trận chiến và kiểm tra xem Kirito có sống sót sau tất cả các trận đấu đó hay không. 

Điều này hiệu quả vì kết quả của một thứ tự cố định rất dễ mô phỏng. Chúng tôi chỉ đơn giản là bước qua những con rồng theo thứ tự đó, duy trì sức mạnh hiện tại của Kirito. 

Vấn đề là số lượng hoán vị. Với$n$rồng, có$n!$các đơn đặt hàng có thể. Ngay cả đối với$n = 15$, điều này đã rất lớn rồi. Với$n = 1000$, vũ lực là hoàn toàn không thể. 

Quan sát quan trọng là việc chiến đấu với những con rồng yếu hơn sớm hơn ít nhất luôn tốt bằng việc chiến đấu với những con rồng mạnh hơn sớm hơn. 

Giả sử Kirito có thể đánh bại hai con rồng, một con có sức mạnh 3 và một con có sức mạnh 10. Nếu anh đánh bại con yếu hơn trước, anh ấy sẽ nhận được thêm sức mạnh sớm hơn, khiến các trận chiến trong tương lai trở nên dễ dàng hơn. Trì hoãn các trận đấu dễ dàng không mang lại lợi ích gì vì tiền thưởng không âm. 

Điều này biến vấn đề thành một chiến lược tham lam đơn giản: 

1. Sắp xếp rồng theo sức mạnh. 
2. Chiến đấu với chúng từ yếu nhất đến mạnh nhất. 
3. Nếu Kirito không thể đánh bại con rồng hiện tại thì câu trả lời là không thể. 

Tại sao mệnh lệnh tham lam này lại có tác dụng? Bởi vì ở mọi thời điểm nó đều giảm thiểu cường độ dòng điện cần thiết. Nếu Kirito không thể đánh bại con rồng yếu nhất còn lại thì anh ấy cũng không thể đánh bại bất kỳ con rồng nào mạnh hơn còn lại. 

Việc sắp xếp loại bỏ hoàn toàn nhu cầu khám phá các hoán vị khác nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n! \cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc sức mạnh ban đầu của Kirito và danh sách các loài rồng. 
2. Lưu trữ mỗi con rồng thành một cặp`(strength, bonus)`. 
3. Sắp xếp những con rồng theo sức mạnh của chúng theo thứ tự tăng dần. 

Con rồng yếu nhất luôn là cuộc chiến an toàn nhất. Đánh bại những con rồng dễ dàng hơn trước sẽ mang lại sức mạnh cộng thêm sớm hơn. 
4. Lặp lại lần lượt từng con rồng được sắp xếp. 
5. Đối với mỗi con rồng, hãy so sánh sức mạnh hiện tại của Kirito với sức mạnh của con rồng. 

Nếu sức mạnh của Kirito nhỏ hơn hoặc bằng sức mạnh của rồng thì anh ta sẽ thua ngay lập tức và câu trả lời là`"NO"`. 
6. Ngược lại, Kirito đánh bại con rồng và nhận được sức mạnh cộng thêm. 

Cập nhật:$$s = s + bonus$$7. Nếu tất cả rồng bị tiêu diệt thành công, hãy in`"YES"`. 

### Tại sao nó hoạt động 

Điều bất biến là trước mỗi trận chiến, Kirito đã đạt được sức mạnh tối đa có thể đạt được sau khi đánh bại tất cả những con rồng yếu hơn. 

Giả sử con rồng yếu nhất còn lại không thể bị đánh bại. Mọi con rồng còn lại ít nhất cũng mạnh như vậy nên không con nào có thể bị đánh bại. Điều đó có nghĩa là trò chơi đã bị mất bất kể thứ tự nào. 

Nếu con rồng yếu nhất có thể bị đánh bại, việc lấy nó ngay lập tức luôn an toàn vì tiền thưởng không âm. Chiến đấu với nó sớm hơn chỉ làm tăng sức mạnh trong tương lai và không bao giờ cản trở bất kỳ lựa chọn nào trong tương lai. 

Vì đặc tính này nên việc sắp xếp theo sức mạnh của rồng sẽ tạo ra một thứ tự tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s, n = map(int, input().split())

    dragons = []
    for _ in range(n):
        x, y = map(int, input().split())
        dragons.append((x, y))

    dragons.sort()

    for x, y in dragons:
        if s <= x:
            print("NO")
            return
        s += y

    print("YES")

solve()
```Phần đầu tiên đọc sức mạnh ban đầu và tất cả các con rồng. Mỗi con rồng được lưu trữ dưới dạng một bộ dữ liệu trong đó giá trị đầu tiên là sức mạnh của con rồng và giá trị thứ hai là phần thưởng sau khi đánh bại nó. 

Cuộc gọi tới`sort()`là bước tham lam cốt lõi. Python sắp xếp các bộ dữ liệu theo từ điển, vì vậy rồng được sắp xếp theo thứ tự bằng cách tự động tăng sức mạnh. 

Vòng lặp mô phỏng các trận đánh theo thứ tự đó. điều kiện`s <= x`là rất quan trọng. Vấn đề đòi hỏi sức mạnh lớn hơn, nên bình đẳng vẫn có nghĩa là thất bại. 

Sau khi thắng một trận, tiền thưởng sẽ được cộng ngay lập tức. Sức mạnh cập nhật này sau đó sẽ được sử dụng cho những con rồng sau này. 

Giải pháp sẽ sớm có hiệu lực ngay khi Kirito thua trận. Không có lý do gì để tiếp tục mô phỏng sau khi thất bại. 

Số nguyên Python có độ chính xác tùy ý, vì vậy lỗi tràn không bao giờ là vấn đề đáng lo ngại ở đây. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2
1 99
100 0
```Rồng được sắp xếp:```
(1, 99), (100, 0)
```| Bước | Sức mạnh hiện tại | Sức Mạnh Rồng | Tiền thưởng | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 99 | Thắng, sức mạnh trở thành 101 | 
| 2 | 101 | 100 | 0 | Thắng, sức mạnh còn 101 | 

Đầu ra cuối cùng:```
YES
```Dấu vết này cho thấy tại sao việc đánh bại những con rồng yếu hơn trước lại có giá trị. Con rồng đầu tiên tăng sức mạnh rất lớn giúp cho cuộc chiến thứ hai có thể diễn ra. 

### Ví dụ 2 

đầu vào:```
5 1
5 10
```Rồng được sắp xếp:```
(5, 10)
```| Bước | Sức mạnh hiện tại | Sức Mạnh Rồng | Tiền thưởng | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 5 | 10 | Thua ngay | 

Đầu ra cuối cùng:```
NO
```Ví dụ này thể hiện quy tắc so sánh nghiêm ngặt. Sức mạnh tương đương là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp thống trị thời gian chạy | 
| Không gian |$O(n)$| Danh sách rồng lưu trữ tất cả các cặp | 

Với tối đa 1000 con rồng, giải pháp này còn thấp hơn nhiều so với thời hạn. Việc sắp xếp 1000 phần tử cực kỳ nhanh và mô phỏng sau đó là tuyến tính. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        s, n = map(int, input().split())

        dragons = []
        for _ in range(n):
            x, y = map(int, input().split())
            dragons.append((x, y))

        dragons.sort()

        for x, y in dragons:
            if s <= x:
                return "NO"
            s += y

        return "YES"

    return solve()

# provided sample
assert run(
    "2 2\n"
    "1 99\n"
    "100 0\n"
) == "YES", "sample 1"

# minimum-size losing case
assert run(
    "1 1\n"
    "1 0\n"
) == "NO", "strict inequality"

# minimum-size winning case
assert run(
    "2 1\n"
    "1 0\n"
) == "YES", "single easy dragon"

# requires sorting to succeed
assert run(
    "3 2\n"
    "4 100\n"
    "2 2\n"
) == "YES", "correct greedy ordering"

# impossible even after smaller fights
assert run(
    "3 2\n"
    "2 1\n"
    "4 100\n"
) == "NO", "boundary transition"

# many equal dragons
assert run(
    "10 4\n"
    "1 0\n"
    "1 0\n"
    "1 0\n"
    "1 0\n"
) == "YES", "all equal weak dragons"

# large bonuses chain correctly
assert run(
    "2 3\n"
    "1 100\n"
    "50 100\n"
    "120 0\n"
) == "YES", "strength accumulation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1 0`|`NO`| Sự bình đẳng đang mất đi | 
|`2 1 / 1 0`|`YES`| Trận chiến thành công đơn giản nhất | 
| Trường hợp chưa giải quyết được |`YES`| Sắp xếp là cần thiết | 
| Trường hợp chuyển tiếp ranh giới |`NO`| Tiền thưởng nhỏ có thể vẫn chưa đủ | 
| Nhiều con rồng yếu như nhau |`YES`| Chiến đấu dễ dàng lặp đi lặp lại | 
| Chuỗi thưởng lớn |`YES`| Tích lũy sức mạnh qua các trận chiến | 

## Vỏ cạnh 

Một sai lầm phổ biến là để sức mạnh ngang nhau giành chiến thắng. 

đầu vào:```
5 1
5 10
```Những con rồng đã được sắp xếp. Kirito bắt đầu với sức mạnh 5. Thuật toán kiểm tra:```
5 <= 5
```Điều kiện này đúng nên thuật toán in ngay`"NO"`. 

Điều đó phù hợp với quy tắc của vấn đề vì Kirito chắc chắn phải mạnh hơn. 

Một trường hợp tinh tế khác là khi sắp xếp làm thay đổi kết quả. 

đầu vào:```
3 2
4 100
2 2
```Sau khi sắp xếp:```
(2, 2), (4, 100)
```Mô phỏng: 

1. Kirito đánh bại rồng sức mạnh-2, sức mạnh trở thành 5. 
2. Kirito đánh bại rồng sức mạnh-4. 

Câu trả lời là`"YES"`. 

Nếu không phân loại, Kirito sẽ chiến đấu với con rồng sức mạnh-4 trước và thua ngay lập tức. Điều này chứng tỏ tại sao việc đặt hàng tham lam lại quan trọng. 

Trường hợp cuối cùng là khi những con rồng nhỏ hơn vẫn không phát triển đủ. 

đầu vào:```
3 2
2 1
4 100
```Sau khi sắp xếp, thứ tự vẫn giữ nguyên. 

1. Kirito đánh bại con rồng đầu tiên và đạt sức mạnh 4. 
2. Con rồng thứ 2 cũng có sức mạnh 4. 

Vì so sánh quá khắt khe nên Kirito vẫn thua. Thuật toán in chính xác`"NO"`thay vì coi sự bình đẳng là thành công một cách sai lầm.
