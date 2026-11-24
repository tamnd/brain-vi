---
title: "CF 65A - Harry Potter và Ba Phép Thuật"
description: "Chúng tôi có ba phép thuật biến đổi có thể chuyển đổi vật liệu này thành vật liệu khác. Câu thần chú đầu tiên biến một gam cát thành b gam chì. Phép thuật thứ hai biến c gam chì thành d gam vàng. Câu thần chú thứ ba biến gam vàng thành gam cát."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 65
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 60"
rating: 1800
weight: 65
solve_time_s: 122
verified: true
draft: false
---

[CF 65A - Harry Potter và ba câu thần chú](https://codeforces.com/problemset/problem/65/A) 

**Đánh giá:** 1800 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có ba phép thuật biến đổi có thể chuyển đổi vật liệu này thành vật liệu khác. 

Phép thuật đầu tiên biến`a`gram cát vào`b`gam chì. 

Phép thuật thứ hai biến`c`gam chì vào`d`gram vàng. 

Phép thuật thứ ba biến`e`gram vàng vào`f`gam cát. 

Chúng tôi có thể áp dụng những phép thuật này bao nhiêu lần, theo bất kỳ thứ tự nào, miễn là chúng tôi hiện có đủ nguyên liệu cho phép thuật mà chúng tôi muốn sử dụng. Câu hỏi đặt ra là liệu có tồn tại một lượng cát ban đầu hữu hạn để cuối cùng chúng ta có thể sản xuất ra lượng vàng lớn tùy ý hay không. 

Cụm từ khóa là “lớn hơn bất kỳ số nào được chỉ định trước”. Chúng tôi không được yêu cầu số tiền tối đa cụ thể. Chúng ta phải xác định xem liệu sản xuất vàng có thể tăng trưởng không giới hạn hay không. 

Những hạn chế là rất nhỏ. Mỗi số nằm giữa`0`Và`1000`. Điều đó có nghĩa là chúng ta có thể tự do sử dụng lý luận toán học trực tiếp mà không cần lo lắng về hiệu suất. Ngay cả việc khám phá trạng thái vũ phu trong phạm vi vừa phải cũng sẽ phù hợp với giới hạn thời gian. Thách thức thực sự không phải là hiệu quả mà là xử lý chính xác mọi tình huống suy biến liên quan đến số không. 

Những trường hợp nguy hiểm đều đến từ những bùa phép tạo ra vật chất “từ hư vô”. 

Hãy xem xét đầu vào này:```
1 1 0 1 1 1
```Phép thuật thứ hai chuyển đổi`0`dẫn vào`1`vàng. Chúng ta có thể sử dụng nó vô số lần ngay lập tức. Câu trả lời là`"Ron"`. 

Việc thực hiện bất cẩn chỉ kiểm tra tỷ lệ nhân chu kỳ đầy đủ sẽ bỏ lỡ điều này. 

Một trường hợp tế nhị khác:```
1 1 1 1 0 1
```Phép thuật thứ ba chuyển đổi`0`vàng vào`1`cát. Bắt đầu từ số 0 cát, chúng ta có thể tạo ra cát vô hạn miễn phí, sau đó sử dụng phép thuật thứ nhất và thứ hai để tạo ra vàng vô hạn. Câu trả lời là`"Ron"`. 

Một giải pháp giả định rằng chúng ta luôn cần nguồn lực ban đầu tích cực sẽ thất bại ở đây. 

Cũng có những trường hợp mọi phép thuật đều bảo tồn hoặc phá hủy vật chất:```
100 50 100 50 100 50
```Mỗi lần biến đổi sẽ mất đi một nửa vật chất. Việc lặp lại chu trình này chỉ làm giảm tài nguyên, vì vậy câu trả lời là`"Hermione"`. 

Một trường hợp cạnh khác:```
0 0 0 0 0 0
```All spells do nothing. Vàng vô hạn là không thể. 

Phần khó nhất của vấn đề là phân biệt chính xác giữa khuếch đại chính hãng và chu kỳ vô hại. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ tự nhiên là mô phỏng các trạng thái có thể tiếp cận được. Chúng ta có thể bắt đầu từ một lượng cát giới hạn và liên tục áp dụng tất cả các phép thuật có thể, đánh dấu mọi bộ ba có thể tiếp cận được.`(sand, lead, gold)`. 

Điều này hoạt động về mặt khái niệm vì hệ thống có tính xác định và việc chuyển đổi trạng thái rất đơn giản. Nếu tồn tại sự tăng trưởng vô hạn, chúng ta kỳ vọng giá trị tài nguyên sẽ tiếp tục tăng. 

Vấn đề là không có giới hạn trên rõ ràng cho không gian trạng thái. Nếu câu trả lời là`"Ron"`, số lượng có thể tăng mãi mãi. Ngay cả khi chúng tôi giới hạn tất cả tài nguyên ở một số giới hạn như`10^6`, chúng ta vẫn không thể chứng minh được tính đúng đắn. Một chu kỳ có lợi nhuận có thể yêu cầu tích lũy tạm thời một số tiền lớn trước khi trở nên bền vững. 

Cấu trúc của các phép thuật mang lại một lộ trình rõ ràng hơn nhiều. 

Quan sát rằng các vật liệu tạo thành một chu trình có hướng:```
sand -> lead -> gold -> sand
```Nếu chúng ta bắt đầu với một lượng cát nhất định và thực hiện toàn bộ chu trình một lần, tổng lượng cát thay đổi theo hệ số nhân:```
(b / a) * (d / c) * (f / e)
```Nếu hệ số này lớn hơn`1`, sau đó một chu kỳ hoàn chỉnh sẽ làm tăng lượng cát. Việc lặp lại chu kỳ này sẽ làm tăng tài nguyên theo cấp số nhân và chúng ta có thể khai thác số vàng không giới hạn. 

Sắp xếp lại tránh phân số:```
b * d * f > a * c * e
```Điều đó xử lý trường hợp thông thường trong đó tất cả các mẫu số đều dương. 

Thử thách còn lại là xử lý bằng không. Một câu thần chú không có đầu vào có thể tạo ra vật chất từ ​​hư vô. Những phép thuật như vậy hoàn toàn bỏ qua đối số nhân. 

Ví dụ, nếu`c = 0`Và`d > 0`, phép thứ hai tạo ra vàng miễn phí. Vàng vô hạn trở nên ngay lập tức. 

Tương tự, nếu`e = 0`Và`f > 0`, chúng ta có thể tạo ra cát vô tận từ con số không. Nếu hai phép thuật đầu tiên cuối cùng có thể biến cát thành vàng thì vàng vô tận sẽ theo sau. 

Toàn bộ giải pháp trở thành một tập hợp nhỏ các kiểm tra logic. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Không giới hạn / không thực tế | Không giới hạn | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Kiểm tra xem câu thần chú thứ hai có tạo ra vàng từ hư vô hay không. 

Nếu như`c == 0`Và`d > 0`, thì mỗi lần đúc sẽ tạo ra vàng mà không tiêu tốn chì. Chúng ta có thể sử dụng nó vô số lần ngay lập tức, vì vậy câu trả lời là`"Ron"`. 
2. Kiểm tra xem câu thần chú thứ ba có tạo ra cát từ hư vô hay không. 

Nếu như`e == 0`Và`f > 0`, thì chúng ta có thể tạo ra cát không giới hạn miễn phí. Để biến cát đó thành vàng, chúng ta cũng cần phép thuật đầu tiên để thực sự tạo ra chì và phép thuật thứ hai để thực sự tạo ra vàng. 

Cụ thể chúng ta cần:`a > 0`,`b > 0`,`c > 0`, Và`d > 0`. 

Nếu tất cả đều đúng, câu trả lời là`"Ron"`. 
3. Xử lý các chu trình không thể có mẫu số bằng 0. 

Nếu bất kỳ mẫu số nào giữa`a`,`c`, hoặc`e`bằng 0, công thức chu trình nhân trở nên không hợp lệ. 

Tại thời điểm này, tất cả các trường hợp sản xuất tự do đều đã được xử lý. Bất kỳ mẫu số 0 nào còn lại đều không thể tạo ra vàng vô hạn, vì vậy câu trả lời là`"Hermione"`. 
4. So sánh tỷ lệ khuếch đại toàn chu kỳ. 

Tính toán xem:```
b * d * f > a * c * e
```Nếu đúng, một chu kỳ đầy đủ sẽ làm tăng lượng cát. Việc lặp lại chu kỳ vô số lần sẽ mang lại nguồn tài nguyên không giới hạn và do đó vàng không giới hạn. 
5. Ngược lại, xuất ra`"Hermione"`. 

### Tại sao nó hoạt động 

Hệ thống chỉ chứa một chu trình tài nguyên:```
sand -> lead -> gold -> sand
```Bất kỳ chiến lược tăng trưởng vô hạn bền vững nào cũng phải trải qua chu kỳ này nhiều lần. Hiệu ứng ròng của một lần di chuyển là nhân cát với:```
(b/a) * (d/c) * (f/e)
```Nếu sản phẩm này vượt quá`1`, chu kỳ lặp đi lặp lại sẽ phát triển tài nguyên mà không bị ràng buộc. Nếu nhiều nhất là`1`, mỗi chu trình hoàn chỉnh sẽ bảo toàn hoặc giảm tổng lượng vật liệu có thể sử dụng, do đó không thể tăng trưởng vô hạn. 

Ngoại lệ duy nhất là khi một câu thần chú không tiêu thụ đầu vào. Trong tình huống đó, vật chất có thể xuất hiện từ hư vô và chúng ta phải xử lý rõ ràng những trường hợp đó một cách riêng biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a, b, c, d, e, f = map(int, input().split())

# Free gold
if c == 0 and d > 0:
    print("Ron")
    sys.exit()

# Free sand
if e == 0 and f > 0:
    if a > 0 and b > 0 and c > 0 and d > 0:
        print("Ron")
    else:
        print("Hermione")
    sys.exit()

# Invalid cycle denominators
if a == 0 or c == 0 or e == 0:
    print("Hermione")
    sys.exit()

# Profitable cycle
if b * d * f > a * c * e:
    print("Ron")
else:
    print("Hermione")
```Hai lần kiểm tra đầu tiên sẽ cô lập các phép thuật không có đầu vào nguy hiểm. Những điều này đặc biệt vì chúng phá vỡ lối suy luận theo phong cách bảo toàn thông thường. Một câu thần chú không tiêu tốn nguyên liệu có thể được lặp lại mãi mãi. 

Điều kiện thứ hai cần được chăm sóc thêm. Chỉ có cát miễn phí là không đủ. Chúng ta vẫn phải có khả năng biến cát thành vàng bằng hai phép thuật đầu tiên. Đó là lý do tại sao mã xác minh đầu vào và đầu ra tích cực cho những chuyển đổi đó. 

Việc kiểm tra mẫu số ngăn ngừa các lỗi logic chia cho 0. Khi tất cả các trường hợp sản xuất tự do bị loại bỏ, mọi mẫu số 0 còn lại có nghĩa là chu trình không thể hoạt động có lãi. 

Phép so sánh cuối cùng chỉ sử dụng số học số nguyên. Nhân cả hai vế sẽ tránh hoàn toàn các vấn đề về độ chính xác của dấu phẩy động. 

Số nguyên Python tự động xử lý các sản phẩm lớn một cách an toàn, mặc dù ở đây giá trị tối đa chỉ là:```
1000 * 1000 * 1000 = 10^9
```đã phù hợp thoải mái trong phạm vi số nguyên tiêu chuẩn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
100 200 250 150 200 250
```| Bước | Tình trạng | Kết quả | 
| --- | --- | --- | 
| Kiểm tra vàng miễn phí |`c == 0`→`250 == 0`| Sai | 
| Kiểm tra cát miễn phí |`e == 0`→`200 == 0`| Sai | 
| Kiểm tra mẫu số | tất cả đều tích cực | Tiếp tục | 
| Khả năng sinh lời |`200 * 150 * 250 = 7,500,000`| | 
| So sánh |`100 * 250 * 200 = 5,000,000`| | 
| Cuối cùng |`7,500,000 > 5,000,000`|`"Ron"`| 

Chu trình làm tăng tài nguyên theo hệ số lớn hơn`1`. Mỗi lần lặp lại đều để lại vật liệu bổ sung, vì vậy việc sản xuất vàng có thể tiếp tục mãi mãi. 

### Ví dụ 2 

đầu vào:```
100 50 100 50 100 50
```| Bước | Tình trạng | Kết quả | 
| --- | --- | --- | 
| Kiểm tra vàng miễn phí |`c == 0`| Sai | 
| Kiểm tra cát miễn phí |`e == 0`| Sai | 
| Kiểm tra mẫu số | tất cả đều tích cực | Tiếp tục | 
| Khả năng sinh lời |`50 * 50 * 50 = 125000`| | 
| So sánh |`100 * 100 * 100 = 1000000`| | 
| Cuối cùng |`125000 > 1000000`| Sai | 

Mỗi chu kỳ đầy đủ sẽ mất vật liệu. Việc lặp đi lặp lại các phép biến đổi chỉ làm giảm nguồn tài nguyên sẵn có nên số vàng vô hạn là không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính và so sánh số học cố định | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Các hạn chế là cực kỳ nhỏ nên giải pháp này sẽ chạy ngay lập tức. Thuật toán chỉ thực hiện một số phép kiểm tra số nguyên và phép nhân. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    a, b, c, d, e, f = map(int, input().split())

    if c == 0 and d > 0:
        print("Ron")
        return

    if e == 0 and f > 0:
        if a > 0 and b > 0 and c > 0 and d > 0:
            print("Ron")
        else:
            print("Hermione")
        return

    if a == 0 or c == 0 or e == 0:
        print("Hermione")
        return

    if b * d * f > a * c * e:
        print("Ron")
    else:
        print("Hermione")

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided sample
assert run("100 200 250 150 200 250\n") == "Ron", "sample 1"

# all zero
assert run("0 0 0 0 0 0\n") == "Hermione", "all zero"

# free gold
assert run("1 1 0 1 5 5\n") == "Ron", "free gold creation"

# free sand but impossible conversion
assert run("0 0 1 1 0 10\n") == "Hermione", "cannot convert sand"

# profitable cycle
assert run("1 2 1 2 1 2\n") == "Ron", "growth factor > 1"

# non-profitable cycle
assert run("2 1 2 1 2 1\n") == "Hermione", "growth factor < 1"

# equality case
assert run("1 1 1 1 1 1\n") == "Hermione", "exact conservation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 0 0 0 0`|`Hermione`| Hệ thống hoàn toàn không hoạt động | 
|`1 1 0 1 5 5`|`Ron`| Tạo vàng miễn phí | 
|`0 0 1 1 0 10`|`Hermione`| Chỉ có cát miễn phí là không đủ | 
|`1 2 1 2 1 2`|`Ron`| Chu kỳ nhân lợi nhuận | 
|`2 1 2 1 2 1`|`Hermione`| Mất tài nguyên mỗi chu kỳ | 
|`1 1 1 1 1 1`|`Hermione`| Bảo tồn chính xác không cho phép tăng trưởng vô hạn | 

## Vỏ cạnh 

Hãy xem xét trường hợp vàng miễn phí:```
1 1 0 5 1 1
```Thuật toán đầu tiên kiểm tra:```
c == 0 and d > 0
```trở thành:```
0 == 0 and 5 > 0
```Điều này là đúng nên câu trả lời là ngay lập tức`"Ron"`. 

Điều đó đúng vì câu thần chú thứ hai tạo ra`5`gram vàng mà không tiêu thụ bất kỳ chì nào. Chúng ta có thể sử dụng nó vô số lần. 

Bây giờ hãy xem xét cát tự do mà không cần chuyển đổi có thể sử dụng được:```
0 0 1 1 0 10
```Phép thứ ba tạo cát miễn phí, nhưng phép thứ nhất không thể biến cát thành chì vì cả hai đều`a`Và`b`là số không. 

Thuật toán đi vào nhánh cát tự do:```
e == 0 and f > 0
```Sau đó, nó kiểm tra xem hai phép thuật đầu tiên có tạo thành một đường dẫn hợp lệ thành vàng hay không:```
a > 0 and b > 0 and c > 0 and d > 0
```Điều này không thành công, vì vậy câu trả lời trở thành`"Hermione"`. 

Cuối cùng, hãy xem xét bảo tồn chính xác:```
1 1 1 1 1 1
```Phép so sánh nhân trở thành:```
1 * 1 * 1 > 1 * 1 * 1
```đó là sai. 

Mỗi chu kỳ trả lại chính xác cùng một lượng cát. Tài nguyên không bao giờ phát triển nên vàng không thể trở nên lớn tùy ý. Thuật toán in chính xác`"Hermione"`.
