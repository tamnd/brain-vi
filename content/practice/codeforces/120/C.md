---
title: "CF 120C - Winnie-the-Pooh và em yêu"
description: "Chúng tôi có vài lọ mật ong và mỗi lọ bắt đầu với một lượng mật ong nhất định. Winnie liên tục chọn lọ hiện có nhiều mật ong nhất. Khi Winnie chọn một cái lọ, có hai điều có thể xảy ra."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 120
codeforces_index: "C"
codeforces_contest_name: "School Regional Team Contest, Saratov, 2011"
rating: 1100
weight: 120
solve_time_s: 99
verified: true
draft: false
---

[CF 120C - Winnie-the-Pooh và em yêu](https://codeforces.com/problemset/problem/120/C) 

**Đánh giá:** 1100 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có vài lọ mật ong và mỗi lọ bắt đầu với một lượng mật ong nhất định. Winnie liên tục chọn lọ hiện có nhiều mật ong nhất. 

Khi Winnie chọn một cái lọ, có hai điều có thể xảy ra. Nếu bình chứa ít hơn`k`kg mật ong, hoặc nếu Winnie đã ăn lọ đó ba lần thì Winnie sẽ ngừng chạm vào lọ đó vĩnh viễn và đưa số mật còn lại cho Heo con. Nếu không thì anh ta ăn chính xác`k`kg từ nó và lọ vẫn có sẵn cho các lượt sau. 

Quá trình tiếp tục cho đến khi mỗi hũ được giao cho Piglet. Chúng ta phải tính tổng lượng mật mà Heo con cuối cùng nhận được. 

Những hạn chế là rất nhỏ. Cả hai`n`và mọi`a[i]`nhiều nhất là`100`. Ngay cả một mô phỏng trực tiếp cũng dễ dàng đủ nhanh. Tổng lượng mật ong nhiều nhất là`100 * 100 = 10000`, và mỗi hành động ăn uống sẽ loại bỏ ít nhất một kg. Một vòng lặp đơn giản với số lượt tìm kiếm tối đa được lặp lại chỉ thực hiện được vài nghìn thao tác. 

Phần khó khăn là hiểu khi nào một cái lọ được lấy ra. Một lọ không bị vứt đi ngay sau lần ăn thứ ba. Winnie có thể ăn trong lọ đúng ba lần. Bình chỉ bị loại bỏ khi nó được chọn lại sau đó. 

Hãy xem xét ví dụ này:```
1 3
10
```Winnie ăn ba lần:```
10 -> 7 -> 4 -> 1
```Bây giờ hũ đã được ăn từ ba lần rồi. Khi được chọn lại, nó sẽ được trao cho Piglet với`1`kg còn lại. Câu trả lời đúng là:```
1
```Việc thực hiện bất cẩn có thể dừng lại ngay sau hành động ăn thứ ba và đưa ra kết quả không chính xác.`4`thay vì. 

Một trường hợp tế nhị khác xảy ra khi một cái bình trở nên nhỏ hơn`k`sau một số thao tác.```
1 5
14
```Sự liên tiếp:```
14 -> 9 -> 4
```Bây giờ lọ có ít hơn`k`, vậy là Piglet nhận được`4`. Câu trả lời là không`0`, bởi vì Winnie không bao giờ ăn một phần lọ. 

Thêm một trường hợp cạnh xuất hiện khi nhiều lọ có cùng giá trị tối đa.```
2 3
6 6
```Một trong hai lọ có thể được chọn đầu tiên. Kết quả cuối cùng vẫn mang tính quyết định vì mỗi bình hoạt động độc lập. Mỗi lọ trở thành`0`sau hai lần ăn no, nên Piglet được`0 + 0 = 0`. 

Quan sát này gợi ý một giải pháp đơn giản hơn nhiều so với mô phỏng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng Winnie chính xác như mô tả. Chúng tôi theo dõi lượng mật ong hiện tại trong mỗi lọ và Winnie đã ăn mỗi lọ bao nhiêu lần. Ở mỗi bước, chúng tôi quét tất cả các lọ để tìm mức tối đa hiện tại. Nếu lọ được chọn đã được ăn ba lần hoặc chứa ít hơn`k`, chúng tôi thêm mật ong còn lại vào câu trả lời và loại nó khỏi sự xem xét. Ngược lại, chúng ta trừ`k`và tăng bộ đếm ăn của nó. 

Mô phỏng này đúng vì nó tuân theo các quy tắc theo đúng nghĩa đen. Với những hạn chế này, nó cũng đủ nhanh. Có nhiều nhất ba hành động ăn uống thành công xảy ra trên mỗi lọ, do đó có nhiều nhất`3n`giảm cộng nhiều nhất`n`việc gỡ bỏ. Mỗi bước quét tất cả các lọ, đưa ra đại khái`O(n^2)`hoạt động. 

Quan sát thú vị là các lọ hoàn toàn độc lập với nhau. Thứ tự Winnie chọn các lọ bằng nhau hoặc khác nhau không ảnh hưởng đến phần còn lại cuối cùng của bất kỳ lọ riêng lẻ nào. 

Đối với một lọ duy nhất với`a[i]`em yêu: 

Nếu Winnie có thể ăn ít hơn ba lần trước khi số lượng giảm xuống dưới`k`, Heo Con nhận được`a[i] mod k`. 

Nếu Winnie có thể ăn ít nhất ba lần thì Winnie sẽ loại bỏ chính xác`3k`tổng số mật ong và sau đó lọ sẽ bị loại bỏ trong lần lựa chọn tiếp theo. Heo con nhận`a[i] - 3k`. 

Vậy số mật ong còn lại trong một hũ chỉ đơn giản là:```
max(a[i] - 3k, a[i] % k)
```Một biểu thức thậm chí còn rõ ràng hơn là: 

Nếu`a[i] < 3k`, Heo con được`a[i] % k`. 

Nếu không, Piglet sẽ nhận được`a[i] - 3k`. 

Điều này làm giảm vấn đề xử lý mỗi bình một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) | O(n) | Đã chấp nhận | 
| Quan sát toán học | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`k`, sau đó đọc mảng kích thước bình. 
2. Khởi tạo câu trả lời cho`0`. 
3. Xử lý từng lọ một cách độc lập. 
4. Đối với lọ đựng`x`kg: 

Nếu`x < 3 * k`, Winnie không thể hoàn thành ba thao tác ăn uống đầy đủ. Anh cứ trừ đi`k`khi có thể, và Piglet cuối cùng cũng nhận được phần còn lại`x % k`. 
5. Ngược lại, Winnie ăn thành công đúng ba lần từ lọ này. 

Sau ba thao tác, lọ chứa`x - 3k`. Vào lần tiếp theo lọ đạt mức tối đa, nó sẽ ngay lập tức được trao cho Piglet vì đã đạt đến giới hạn ba lần ăn. 
6. Cộng số tiền còn lại chính xác của lọ này vào tổng câu trả lời. 
7. In câu trả lời. 

### Tại sao nó hoạt động 

Mỗi lọ phát triển độc lập. Các hành động duy nhất có thể thực hiện được trên một cái lọ là trừ`k`hoặc loại bỏ nó vĩnh viễn. Winnie có thể trừ`k`tối đa ba lần từ bất kỳ lọ nào. 

Nếu một cái lọ chạy bên dưới`k`trước ba lần phẫu thuật, mật ong không thể ăn được nữa, vì vậy phần còn lại cuối cùng chính xác là`x % k`. 

Nếu một cái lọ sống sót sau ba lần thao tác, Winnie sẽ loại bỏ chính xác`3k`tổng số mật ong. Nội quy cấm ăn thêm từ lọ đó, vì vậy số tiền còn lại`x - 3k`đi thẳng tới Piglet. 

Vì phần đóng góp cuối cùng của mỗi bình chỉ phụ thuộc vào giá trị ban đầu của chính nó nên việc tính tổng các kết quả độc lập này sẽ tạo ra tổng số chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    ans = 0

    for x in a:
        if x < 3 * k:
            ans += x % k
        else:
            ans += x - 3 * k

    print(ans)

solve()
```Giải pháp xử lý từng lọ riêng biệt vì lượng mật ong cuối cùng còn lại chỉ phụ thuộc vào số lần Winnie có thể ăn trong lọ đó. 

điều kiện`x < 3 * k`là quan sát chính. Nếu bình chứa ít hơn`3k`, Winnie không thể xóa`k`ba lần đầy đủ. Cuối cùng anh ta dừng lại vì mật ong còn lại trở nên nhỏ hơn`k`, và Piglet nhận phần còn lại`x % k`. 

Ngược lại, Winnie thực hiện thành công đúng ba hành động ăn uống. Sau đó, quy định không được ăn tiếp trong cùng một lọ nên số tiền còn lại là`x - 3 * k`. 

Một sai lầm phổ biến là sử dụng`<=`thay vì`<`. Ví dụ:```
x = 9, k = 3
```Winnie có thể ăn đúng ba lần:```
9 -> 6 -> 3 -> 0
```Heo con nhận`0`, không`9 % 3`một cách tình cờ. Sự bất bình đẳng nghiêm ngặt xử lý điều này một cách chính xác. 

Tất cả số học đều phù hợp thoải mái bên trong các số nguyên Python tiêu chuẩn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
15 8 10
```| Bình | Mật ong ban đầu | Tình trạng | Heo Con Được | 
| --- | --- | --- | --- | 
| 1 | 15 | 15 ≥ 9 | 15 - 9 = 6 | 
| 2 | 8 | 8 < 9 | 8 % 3 = 2 | 
| 3 | 10 | 10 ≥ 9 | 10 - 9 = 1 | 

Tổng cộng:```
6 + 2 + 1 = 9
```Đầu ra:```
9
```Ví dụ này cho thấy cả hai hành vi. Lọ thứ nhất và thứ ba tồn tại được sau ba lần ăn, trong khi lọ thứ hai trở nên quá nhỏ trước khi đạt đến giới hạn. 

### Ví dụ 2 

đầu vào:```
2 5
14 20
```| Bình | Mật ong ban đầu | Tình trạng | Heo Con Được | 
| --- | --- | --- | --- | 
| 1 | 14 | 14 < 15 | 14 % 5 = 4 | 
| 2 | 20 | 20 ≥ 15 | 20 - 15 = 5 | 

Tổng cộng:```
4 + 5 = 9
```Đầu ra:```
9
```Lọ đầu tiên thể hiện "ít hơn`k`" điều kiện dừng. Bình thứ hai thể hiện điều kiện dừng "ba lần ăn". 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi lọ được xử lý một lần | 
| Không gian | O(1) | Chỉ có một vài biến được sử dụng | 

Với`n ≤ 100`, thậm chí mô phỏng cũng sẽ trôi qua một cách thoải mái. Phương pháp toán học đơn giản hơn và nhanh hơn, chỉ cần một lần duyệt qua mảng. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    ans = 0

    for x in a:
        if x < 3 * k:
            ans += x % k
        else:
            ans += x - 3 * k

    print(ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.strip()

# provided sample
assert run("3 3\n15 8 10\n") == "9", "sample 1"

# minimum size
assert run("1 1\n1\n") == "0", "minimum case"

# exactly three full eats
assert run("1 3\n9\n") == "0", "exactly 3 operations"

# becomes smaller than k
assert run("1 5\n14\n") == "4", "remainder case"

# all equal values
assert run("4 2\n7 7 7 7\n") == "4", "all equal"

# maximum style values
assert run("3 1\n100 100 100\n") == "291", "large values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1`|`0`| Đầu vào hợp lệ nhỏ nhất | 
|`1 3 / 9`|`0`| Chính xác là ba hoạt động ăn uống thành công | 
|`1 5 / 14`|`4`| Bình trở nên nhỏ hơn`k`| 
|`4 2 / 7 7 7 7`|`4`| Giá trị bằng nhau lặp lại | 
|`3 1 / 100 100 100`|`291`| Giá trị lớn và phép trừ lặp đi lặp lại | 

## Vỏ cạnh 

Hãy xem xét trường hợp một cái lọ có thể được ăn đúng ba lần. 

đầu vào:```
1 3
10
```Thực thi:```
10 -> 7 -> 4 -> 1
```Lúc này Winnie đã ăn được ba lần rồi nên phần còn lại`1`kilo đi đến Piglet. Thuật toán kiểm tra:```
10 >= 3 * 3
```nên Piglet nhận được:```
10 - 9 = 1
```phù hợp chính xác với mô phỏng. 

Bây giờ hãy xem xét một cái lọ trở nên quá nhỏ trước ba thao tác. 

đầu vào:```
1 5
14
```Thực thi:```
14 -> 9 -> 4
```Số tiền còn lại dưới đây`k`, vậy là Piglet nhận được`4`. Thuật toán sử dụng:```
14 < 15
```và tính toán:```
14 % 5 = 4
```cái nào đúng. 

Cuối cùng, hãy xem xét nhiều lọ tối đa bằng nhau. 

đầu vào:```
2 3
6 6
```Mỗi lọ phát triển độc lập:```
6 -> 3 -> 0
```Heo con nhận`0`từ mỗi lọ, để có câu trả lời cuối cùng về`0`. 

Thứ tự mà Winnie chọn giữa các lọ bằng nhau không bao giờ thay đổi số tiền còn lại cuối cùng, đó là lý do tại sao công thức độc lập cho mỗi lọ là hợp lệ.
