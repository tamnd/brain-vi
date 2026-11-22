---
title: "CF 339C - Xenia và Quả cân"
description: "Xenia có quyền truy cập vào một số tập hợp con có trọng lượng từ 1 đến 10 kg. Đối với mỗi loại cân nặng được đánh dấu là có sẵn, cô ấy có thể sử dụng cân nặng đó bao nhiêu lần cũng được. Cô đặt từng quả cân lên một chiếc cân."
date: "2026-06-06T17:00:49+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "dp", "graphs", "greedy", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 339
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 197 (Div. 2)"
rating: 1700
weight: 339
solve_time_s: 134
verified: true
draft: false
---

[CF 339C - Xenia và Trọng lượng](https://codeforces.com/problemset/problem/339/C) 

**Đánh giá:** 1700 
**Thẻ:** thuật toán xây dựng, dfs và tương tự, dp, đồ thị, đường dẫn tham lam, ngắn nhất 
**Thời gian giải:** 2m 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Xenia có quyền truy cập vào một số tập hợp con có trọng lượng từ 1 đến 10 kg. Đối với mỗi loại cân nặng được đánh dấu là có sẵn, cô ấy có thể sử dụng cân nặng đó bao nhiêu lần cũng được. 

Cô đặt từng quả cân lên một chiếc cân. Trọng lượng đầu tiên chuyển sang chảo bên trái, trọng lượng thứ hai lên chảo bên phải, sau đó lại chuyển sang trái, luân phiên mãi mãi. Sau mỗi lần đặt, chiếc chảo vừa nhận được trọng lượng mới phải nặng hơn chiếc chảo đối diện. 

Chuỗi cũng phải đáp ứng một hạn chế khác: hai trọng số được chọn liên tiếp không thể có cùng giá trị. 

Đầu vào mô tả giá trị trọng số nào có sẵn và hỏi liệu có thể thực hiện chính xác hay không`m`di chuyển. Nếu có thể, chúng ta phải xuất ra một chuỗi các trọng số đã chọn hợp lệ. 

Giá trị lớn nhất của`m`là 1000. Điều đó ngay lập tức gợi ý rằng bất kỳ giải pháp nào có trạng thái phụ thuộc vào số lần di chuyển đều khả thi nếu số lượng biến trạng thái bổ sung vẫn còn nhỏ. Vì chỉ có 10 giá trị trọng số có thể, nên một không gian trạng thái có thứ tự`1000 × 10 × something_small`hoàn toàn có thể quản lý được. 

Việc tìm kiếm ngây thơ trên tất cả các chuỗi là vô vọng. Ngay cả khi chỉ có mười lựa chọn cho mỗi nước đi, số lượng các chuỗi có thể xảy ra là khoảng`10^1000`trong trường hợp xấu nhất. 

Phần khó khăn là hiểu được thông tin nào thực sự quan trọng khi quyết định bước đi tiếp theo. 

Hãy xem xét sự khác biệt cân bằng sau mỗi lần di chuyển. Giả sử chảo bên trái hiện vượt quá chảo bên phải 3. Nếu nước đi tiếp theo phải được đặt vào chảo bên phải thì trọng lượng đã chọn phải lớn hơn 3. Sau khi đặt xong, chảo bên phải sẽ nặng hơn chính xác.`weight - 3`. 

Quan sát này có nghĩa là toàn bộ lịch sử của các vị trí trước đó là không liên quan. Chỉ có chênh lệch số dư hiện tại và trọng lượng được chọn cuối cùng mới quan trọng. 

Có một số trường hợp khó bỏ sót. 

Nếu chỉ có một loại trọng lượng và`m > 1`, câu trả lời là không thể vì các trọng số liên tiếp không thể bằng nhau. 

Ví dụ:```
1000000000
2
```Trọng số khả dụng duy nhất là 1. Có thể thực hiện lần di chuyển đầu tiên, nhưng lần di chuyển thứ hai sẽ phải sử dụng lại 1, điều này bị cấm. Câu trả lời đúng là`NO`. 

Một trường hợp tinh vi khác xảy ra khi có các trọng số sẵn có nhưng không có trọng số nào đủ lớn để đảo ngược số dư hiện tại. 

Ví dụ:```
1100000000
3
```Trọng số có sẵn là 1 và 2. 

Một khởi đầu hợp lệ là:```
1
2
```Sau những nước đi này, chênh lệch cân bằng sẽ trở thành 1 nghiêng về bên phải. Nước đi thứ ba diễn ra ở bên trái và phải vượt quá 1. Trọng số 2 sẽ được, nhưng nó bằng trọng lượng trước đó. Trọng lượng 1 quá nhỏ. Không có sự tiếp tục tồn tại. 

Một chiến lược tham lam chỉ chọn trọng lượng lớn nhất hiện có cũng có thể thất bại. Một lựa chọn hợp lệ cục bộ có thể không để lại sự tiếp tục hợp pháp sau này. Từ`m`có thể lớn tới 1000, chúng ta cần một phương pháp có thể khám phá một cách có hệ thống các khả năng trong tương lai. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Ở mỗi lần di chuyển, hãy thử mọi trọng lượng có sẵn khác với trọng lượng trước đó và tạo ra một chảo nặng hơn hợp lệ. Tiếp tục đệ quy cho đến khi một chuỗi có độ dài`m`được tìm thấy hoặc tất cả các khả năng đều thất bại. 

Tìm kiếm này đúng vì nó kiểm tra rõ ràng mọi trình tự pháp lý. Vấn đề là yếu tố phân nhánh. Trong trường hợp xấu nhất, có sẵn mười trọng số ở mỗi bước, dẫn đến gần như`10^m`tiểu bang. Vì`m = 1000`, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là cấu hình đầy đủ của cân là không cần thiết. Sau mỗi nước đi, chỉ có hai thông tin ảnh hưởng đến các quyết định trong tương lai. 

Đầu tiên là chênh lệch số dư hiện tại. Hãy xác định nó là lượng mà chảo hiện tại nặng hơn so với chảo kia. Giá trị này luôn dương. 

Thứ hai là trọng lượng được sử dụng trong lần di chuyển trước, vì trọng lượng bằng nhau liên tiếp đều bị cấm. 

Giả sử sự khác biệt hiện tại là`d`, và bước tiếp theo đặt trọng lượng`w`trên chảo nhẹ hơn. Việc di chuyển có hiệu lực chính xác khi`w > d`. Sau khi đặt nó, sự khác biệt mới trở thành`w - d`. 

Vì vậy, mỗi bước di chuyển đều biến đổi:```
(d, last_weight)
    ->
(w - d, w)
```Ở đâu`w`có sẵn,`w != last_weight`, Và`w > d`. 

Sự khác biệt không bao giờ có thể vượt quá 10 vì tất cả các trọng số tối đa là 10 và sự khác biệt tiếp theo luôn là`w - d`. Điều đó mang lại nhiều nhất 10 điểm khác biệt có thể có và 11 khả năng cho trọng số trước đó (0 cho trạng thái bắt đầu cộng với trọng số 1..10). 

Toàn bộ không gian tìm kiếm trở thành:```
move_number × difference × last_weight
```nhiều nhất là:```
1000 × 10 × 11 = 110000
```tiểu bang. 

Tìm kiếm theo chiều sâu với khả năng ghi nhớ trên không gian trạng thái này dễ dàng nằm trong giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(10^m) | O(m) | Quá chậm | 
| DFS + DP tối ưu | O(m × 10 × 11 × 10) | O(m × 10 × 11) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi sẵn có và thu thập tất cả các giá trị trọng lượng được phép. 
2. Xác định trạng thái DFS`(pos, diff, last)`. 

Đây`pos`là số lượng tạ đã được đặt,`diff`là chênh lệch số dư dương hiện tại và`last`là trọng lượng đã chọn trước đó. 
3. Nếu`pos == m`, chúng ta đã xây dựng thành công một dãy có độ dài`m`. Trả lại thành công. 
4. Đối với mọi trọng lượng có sẵn`w`, kiểm tra xem nó có thể được chọn hay không. 

Nó phải thỏa mãn: 

-`w != last`-`w > diff`Điều kiện đầu tiên thực thi hạn chế trọng lượng liên tiếp. Điều thứ hai đảm bảo rằng chiếc chảo nhận được trọng lượng mới sẽ trở nên nặng hơn rất nhiều. 
5. Tính chênh lệch tiếp theo. 

Vì vật nặng mới được đặt lên chảo nhẹ hơn nên bên nặng hơn sẽ chuyển sang. Lượng dư thừa mới trở thành:```
new_diff = w - diff
```6. Trạng thái tìm kiếm đệ quy:```
(pos + 1, new_diff, w)
```7. Nếu bất kỳ lệnh gọi đệ quy nào thành công, hãy ghi lại`w`như một phần của câu trả lời và truyền bá thành công đi lên. 
8. Ghi nhớ các trạng thái thất bại. Nếu một trạng thái đã được chứng minh là không thể thực hiện được, hãy bỏ qua việc tính toán lại. 
9. Nếu trạng thái ban đầu`(0, 0, 0)`thành công, xuất ra chuỗi được xây dựng. Nếu không thì xuất ra`NO`. 

### Tại sao nó hoạt động 

Tính bất biến đó là`diff`luôn bằng lượng mà cái chảo hiện tại nặng hơn cái chảo nhẹ hơn. 

Ở trạng thái có sự khác biệt`diff`, nước đi tiếp theo phải đặt lên chảo bật lửa. Để làm cho chiếc chảo đó trở nên nặng hơn thì trọng lượng tăng thêm phải vượt quá mức chênh lệch hiện tại. Đó chính xác là điều kiện`w > diff`. 

Sau khi đặt một vật nặng như vậy, cái chảo mới nặng hơn cái kia một khoảng`w - diff`, trở thành sự khác biệt của trạng thái tiếp theo. 

Mỗi trình tự pháp lý tương ứng với chính xác một đường đi qua các trạng thái này và mọi chuyển đổi do DFS tạo ra đều tương ứng với một bước đi hợp pháp. Việc tìm kiếm khám phá tất cả các phần tiếp theo hợp pháp trong khi việc ghi nhớ ngăn chặn công việc lặp đi lặp lại. Nếu DFS báo cáo thành công thì trình tự được xây dựng lại là hợp lệ. Nếu nó báo lỗi thì không có trình tự pháp lý nào tồn tại ở trạng thái đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    m = int(input())

    weights = [i + 1 for i, ch in enumerate(s) if ch == '1']

    dp = {}
    answer = []

    def dfs(pos, diff, last):
        if pos == m:
            return True

        state = (pos, diff, last)
        if state in dp:
            return False

        for w in weights:
            if w == last:
                continue
            if w <= diff:
                continue

            if dfs(pos + 1, w - diff, w):
                answer.append(w)
                return True

        dp[state] = False
        return False

    if dfs(0, 0, 0):
        answer.reverse()
        print("YES")
        print(*answer)
    else:
        print("NO")

solve()
```Danh sách`weights`chứa tất cả các giá trị trọng lượng có thể sử dụng. Vì chỉ có mười giá trị có thể nên việc lặp qua chúng trong mọi trạng thái DFS là rẻ. 

Nhà nước`(pos, diff, last)`nắm bắt chính xác thông tin cần thiết để tiếp tục quá trình. Không có gì khác từ những động thái trước đó ảnh hưởng đến tính hợp pháp trong tương lai. 

Từ điển ghi nhớ chỉ lưu trữ các trạng thái không thành công. Trạng thái thành công ngay lập tức trở lại sau khi ghi lại trọng lượng đã chọn. Phương pháp xây dựng lại này tránh việc lưu trữ các con trỏ gốc cho mọi trạng thái. 

Một chi tiết tinh tế là ý nghĩa của`diff`. Nó luôn dương sau ít nhất một lần di chuyển và nó thể hiện mức dư thừa hiện tại của chảo nặng hơn. Công thức`w - diff`hoạt động vì trọng lượng mới luôn được đặt trên chảo nhẹ hơn và phải lớn hơn trọng lượng dư thừa hiện có. 

Một sai lầm dễ mắc phải khác là quên đảo ngược trình tự đã được xây dựng lại. Chúng tôi thêm các trọng số trong khi quay lại từ các cuộc gọi đệ quy, do đó trình tự được thu thập theo thứ tự ngược lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0000000101
3
```Trọng lượng có sẵn:`{8, 10}`| Di chuyển | Khác biệt hiện tại | Cân nặng cuối cùng | Cân nặng được chọn | Khác biệt mới | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 8 | 8 | 
| 2 | 8 | 8 | 10 | 2 | 
| 3 | 2 | 10 | 8 | 6 | 

Trình tự được xây dựng:```
8 10 8
```Sau mỗi lần di chuyển, chiếc chảo nhận được trọng lượng mới sẽ trở nên nặng hơn rất nhiều. Trọng lượng liên tiếp là khác nhau. 

### Ví dụ 2 

đầu vào:```
1100000000
3
```Trọng lượng có sẵn:`{1, 2}`| Di chuyển | Khác biệt hiện tại | Cân nặng cuối cùng | Cân nặng được chọn | Khác biệt mới | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 1 | 1 | 
| 2 | 1 | 1 | 2 | 1 | 
| 3 | 1 | 2 | không | - | 

Ở nước đi 3, trọng số 1 không lớn hơn chênh lệch hiện tại và trọng số 2 bằng trọng lượng trước đó. Không có nước đi hợp lệ nào tồn tại. 

Đầu ra:```
NO
```Ví dụ này cho thấy tại sao giá trị cục bộ là không đủ. Một trình tự có thể bị mắc kẹt ngay cả khi những bước di chuyển trước đó là hợp pháp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m × 10 × 11 × 10) | Nhiều nhất`m × 10 × 11`bang, mỗi người thử tới 10 tạ | 
| Không gian | O(m × 10 × 11) | Các trạng thái được ghi nhớ cộng với ngăn xếp đệ quy | 

Số lượng trạng thái nhiều nhất là khoảng 110.000 và mỗi trạng thái thực hiện tối đa mười lần chuyển đổi. Điều này dễ dàng phù hợp trong thời hạn. Việc sử dụng bộ nhớ cũng thoải mái dưới mức giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    s = input().strip()
    m = int(input())

    weights = [i + 1 for i, ch in enumerate(s) if ch == '1']

    dp = {}
    answer = []

    def dfs(pos, diff, last):
        if pos == m:
            return True

        state = (pos, diff, last)
        if state in dp:
            return False

        for w in weights:
            if w == last:
                continue
            if w <= diff:
                continue

            if dfs(pos + 1, w - diff, w):
                answer.append(w)
                return True

        dp[state] = False
        return False

    out = []

    if dfs(0, 0, 0):
        answer.reverse()
        out.append("YES")
        out.append(" ".join(map(str, answer)))
    else:
        out.append("NO")

    return "\n".join(out) + "\n"

# provided sample
assert run("0000000101\n3\n").startswith("YES")

# minimum size
assert run("1000000000\n1\n") == "YES\n1\n"

# only one weight available, length > 1
assert run("1000000000\n2\n") == "NO\n"

# impossible because sequence gets stuck
assert run("1100000000\n3\n") == "NO\n"

# maximum-length style stress case
res = run("1111111111\n1000\n")
assert res.startswith("YES")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1000000000 / 1`|`YES`| Ví dụ nhỏ nhất có thể | 
|`1000000000 / 2`|`NO`| Hạn chế bằng nhau liên tiếp | 
|`1100000000 / 3`|`NO`| Bang bị mắc kẹt bất chấp tiền tố pháp lý | 
|`1111111111 / 1000`|`YES`| Độ sâu lớn và hiệu quả DP | 

## Vỏ cạnh 

Hãy xem xét:```
1000000000
2
```Chỉ có trọng số 1 tồn tại. Bước đi đầu tiên tạo ra trạng thái`(1, 1, 1)`. Nước đi tiếp theo không thể sử dụng trọng số 1 vì nó bằng với lựa chọn trước đó. DFS không tìm thấy quá trình chuyển đổi và báo cáo lỗi. Đầu ra của thuật toán`NO`. 

Bây giờ hãy xem xét:```
1100000000
3
```DFS có thể bắt đầu với trọng số 1. Các trạng thái kết quả là:```
(0,0,0)
→ (1,1,1)
→ (2,1,2)
```Từ`(2,1,2)`, trọng số 1 quá nhỏ và trọng số 2 lặp lại lựa chọn trước đó. Trạng thái được ghi nhớ là không thể. Mọi nhánh khác đều thất bại tương tự, nên câu trả lời cuối cùng là`NO`. 

Cuối cùng hãy xem xét:```
0000000101
3
```Trình tự:```
8 → 10 → 8
```tạo ra sự khác biệt:```
8 → 2 → 6
```Mỗi trọng số được chọn hoàn toàn lớn hơn chênh lệch hiện tại và khác với trọng số trước đó. DFS đạt chiều dài`m`và xây dựng lại trình tự thành công.
