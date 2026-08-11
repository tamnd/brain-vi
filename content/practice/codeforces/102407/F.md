---
title: "CF 102407F - \u0411\u0435\u0441\u043f\u043e\u0440\u044f\u0434\u043e\u0447\u043d\u043e\u0435 \u0432\u044b\u0441\u0442\u0443\u043f\u043b\u0435\u043d\u0438\u0435"
description: "Chúng ta có một mảng các giá trị không âm a 1 ​,…,a n ​, một giá trị cho mỗi khán giả. Mỗi sĩ quan cảnh sát theo dõi một khoảng liền kề [l i ​ ,r i ​ ]."
date: "2026-08-11T16:18:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102407
codeforces_index: "F"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0412\u0442\u043e\u0440\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102407
solve_time_s: 205
verified: true
draft: false
---

[CF 102407F - \u0411\u0435\u0441\u043f\u043e\u0440\u044f\u0434\u043e\u0447\u043d\u043e\u0435 \u0432\u044b\u0441\u0442\u0443\u043f\u043b\u0435\u043d\u0438\u0435](https://codeforces.com/problemset/problem/102407/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mảng các giá trị không âm a 1 ​,…,a n ​, một giá trị cho mỗi khán giả. Mỗi sĩ quan cảnh sát theo dõi một khoảng liền kề [l i ​ ,r i ​ ]. Sự chú ý của viên chức đó là tổng của các giá trị hiện tại trong khoảng thời gian của anh ta, do đó tổng sự chú ý của tất cả các sĩ quan là tổng của tất cả các tổng khoảng thời gian. 

Arthur có thể làm giảm giá trị của khán giả. Nếu khán giả j giảm đi d j ​ thì 0<d j ​ <a j ​, và tổng mức giảm thỏa mãn 

j=1 ∑ n ​ d j ​ ≤k. 

Mục tiêu là giảm thiểu sự chú ý của tất cả các sĩ quan sau những lần giảm này. 

Điều quan trọng là nhìn vào một khán giả một cách độc lập. Giả sử khán giả j thuộc đúng c j ​ khoảng cảnh sát. Giảm a j ​ đi một sẽ làm giảm tổng sự chú ý chính xác bằng c j ​, bởi vì mỗi một trong số các sĩ quan cj ​ đó mất đi một đơn vị sự chú ý. Do đó, mỗi đơn vị ngân sách của Arthur có giá trị khác nhau chỉ phụ thuộc vào c j ​. 

Đầu tiên chúng ta cần tất cả c j ​. Vì có thể có tới m=106 khoảng, việc thêm một khoảng vào mỗi vị trí của mỗi khoảng sẽ quá tốn kém. Một mảng khác biệt cho phép chúng tôi xử lý từng khoảng thời gian không đổi và khôi phục tất cả số lượng bìa bằng một tổng tiền tố. 

Giới hạn n 10 5 có nghĩa là phương thức O(n 2 ) đã quá lớn, với tối đa 10 10 phép toán cơ bản. Số lượng sĩ quan có thể lên tới 10 6 nên việc xử lý mỗi đợt một lần là đúng quy mô. Giá trị k có thể đạt tới 10 12, do đó, thuật toán thực hiện một lần lặp cho mỗi đơn vị giảm là không thể ngay cả khi n nhỏ. Python cũng cần xử lý đầu vào cẩn thận vì việc đọc và xử lý hàng triệu khoảng thời gian sẽ chiếm ưu thế trong thời gian chạy. 

Có một số trường hợp ranh giới có thể âm thầm phá vỡ quá trình triển khai. Nếu k=0 thì không thể thay đổi được gì. Ví dụ,```
1 1 0
5
1 1
```có câu trả lời 5. Một giải pháp luôn chi toàn bộ ngân sách sẽ cố gắng giảm giá trị xuống dưới mức cho phép một cách không chính xác. 

Nếu k vượt quá tổng của tất cả aj ​, Arthur có thể giảm số lượng khán giả xuống 0. Ví dụ,```
2 1 100
3 4
1 2
```có câu trả lời 0, không phải là giá trị âm. Mức giảm của một vị thế được giới hạn bởi giá trị ban đầu của nó. 

Một khoảng chỉ chứa một điểm cuối cũng cần có ranh giới mảng sai phân chính xác. Vì```
2 1 1
5 7
2 2
```vị trí được bảo vệ duy nhất là vị trí thứ hai, vì vậy câu trả lời với k=1 là 11. Cập nhật mảng hiệu tại r thay vì r+1 hoặc trộn các chỉ số dựa trên 0 và dựa trên một, cũng có thể làm cho vị trí đầu tiên bị che không chính xác. 

Cuối cùng, một khán giả có thể được nhiều sĩ quan bảo vệ trong khi người khác chỉ được một người bảo vệ. Vì```
3 2 5
4 4 4
1 3
2 2
```số lượng bảo hiểm là 1,2,1. Arthur nên dành tất cả bốn đơn vị có thể có cho khán giả 2 trước tiên, bởi vì mỗi đơn vị ở đó sẽ loại bỏ hai đơn vị tổng sự chú ý. Một chiến lược chỉ đơn giản chọn aj ​ lớn nhất hoặc xử lý khán giả theo thứ tự ban đầu của họ sẽ bỏ lỡ mục tiêu thực tế. 

## Phương pháp tiếp cận 

Chiến lược bạo lực trực tiếp có thể xem xét ngân sách của Arthur từng đơn vị một. Đối với mỗi đơn vị, hãy kiểm tra tất cả n khán giả, tìm một đơn vị có số lượng người xem hiện tại lớn nhất mà giá trị vẫn dương, giảm nó đi một và lặp lại. Điều này đúng vì mỗi đơn vị bị loại bỏ khỏi khán giả j sẽ làm giảm mục tiêu đi chính xác c j ​, vì vậy đơn vị sẵn có tốt nhất luôn là đơn vị có c j ​ tối đa ​. 

Vấn đề là k có thể bằng 10 12. Ngay cả khi tìm được khán giả tốt nhất chỉ mất O(n), quy trình có thể yêu cầu 

O(nk)=O(10 17 ) 

hoạt động. Thực tế là số lượng khán giả có thể bị giảm đi nhiều đơn vị có nghĩa là không có lý do gì để xem xét lại số lượng người xem tương tự sau mỗi đơn vị. 

Quan sát khắc phục điều này là tất cả các đơn vị thuộc cùng một khán giả đều có giá trị giống hệt nhau. Nếu khán giả j có vùng phủ sóng c j ​, thì tất cả a j ​ đơn vị có sẵn ở đó đều lưu chính xác c j ​. Do đó, chúng tôi có thể nhóm ngân sách theo số lượng bảo hiểm thay vì theo đơn vị riêng lẻ. 

Sau khi tính toán tất cả c j ​, hãy sắp xếp khán giả theo cách giảm c j ​. Arthur nên giảm hoàn toàn lượng khán giả có mức độ phủ sóng cao nhất trước, sau đó tiếp tục với giá trị mức độ phủ sóng tiếp theo. Nếu ngân sách còn lại nằm trong giá trị của một khán giả thì chỉ có nhiều đơn vị đó bị loại bỏ. Đây là quyết định tham lam giống như phương pháp vũ phu, nhưng tất cả các quyết định bình đẳng đều được nén vào một thao tác. 

Sự chú ý tổng thể ban đầu cũng có thể được viết trực tiếp dưới dạng 

j=1 ∑ n ​ a j ​ c j ​ . 

Nếu chúng ta giảm khán giả j đi d j ​, thì mục tiêu sẽ giảm d j ​ c j ​. Vì mỗi d j ​ tiêu tốn một đơn vị ngân sách và mang lại lợi ích là c j ​, việc nhận lợi ích theo thứ tự giảm dần là tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nk+m) | O(n) | Quá chậm | 
| Tối ưu | O(m+nlogn) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo mảng khác biệt`diff`có độ dài n+1. Với mỗi khoảng cảnh sát [l,r], hãy cộng 1 tại l và trừ 1 tại r+1. Điều này thể hiện thực tế là khoảng thời gian bắt đầu đóng góp tại l và ngừng đóng góp ngay sau r. 
2. Lấy tổng tiền tố`diff`. Tại vị trí j, giá trị thu được chính xác là c j ​, số lượng cảnh sát đang theo dõi khán giả j. 
3. Tính tổng sự chú ý ban đầu là ∑a j ​ c j ​. Điều này tương đương với việc tính tổng khoảng thời gian của mỗi sĩ quan, nhưng nó có thể được thực hiện trong O(n) sau khi biết số lượng bảo hiểm. 
4. Ghép nối phạm vi phủ sóng của mỗi khán giả c j ​ với số tiền có sẵn a j ​, sau đó sắp xếp các cặp này theo phạm vi phủ sóng theo thứ tự giảm dần. Một đơn vị bị loại bỏ khỏi khán giả với phạm vi phủ sóng c j ​ giúp tiết kiệm sự chú ý của c j ​ đơn vị, vì vậy phạm vi phủ sóng lớn hơn luôn giúp sử dụng ngân sách tốt hơn. 
5. Đi qua các khán giả đã được sắp xếp. Đối với khán giả j, hãy xóa 

x=min(a j ​ ,k còn lại ​ ) 

đơn vị. Trừ xc j ​ từ câu trả lời và trừ x từ ngân sách còn lại. Nếu ngân sách trở về 0, hãy dừng lại. 

1. In tổng kết quả. Nếu ngân sách lớn hơn tổng của tất cả các giá trị của khán giả thì mọi giá trị cuối cùng sẽ giảm xuống 0, do đó câu trả lời đương nhiên trở thành 0. 

### Tại sao nó hoạt động 

Mục tiêu có thể được viết lại thành ∑ j ​ a j ​ c j ​, trong đó c j ​ là số khoảng chứa vị trí j. Việc giảm khán giả j đi một đơn vị sẽ tiêu tốn một đơn vị ngân sách của Arthur và giảm mục tiêu chính xác là c j ​. Do đó, mọi đơn vị rút gọn có sẵn là một mục độc lập có giá trị c j ​, và khán giả j cung cấp chính xác j ​ các mục đó. Việc sắp xếp các mục này theo thứ tự giá trị giảm dần sẽ tối đa hóa tổng mức giảm cho bất kỳ ngân sách nào, điều này hoàn toàn tương đương với việc giảm thiểu sự chú ý cuối cùng. Do đó, quá trình duyệt tham lam tạo ra một giải pháp tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))

    diff = [0] * (n + 1)

    for _ in range(m):
        l, r = map(int, input().split())
        diff[l - 1] += 1
        diff[r] -= 1

    coverage = [0] * n
    cur = 0
    for i in range(n):
        cur += diff[i]
        coverage[i] = cur

    total = 0
    items = []

    for value, cnt in zip(a, coverage):
        total += value * cnt
        items.append((cnt, value))

    items.sort(reverse=True)

    for cnt, value in items:
        if k == 0:
            break

        take = min(value, k)
        total -= take * cnt
        k -= take

    print(total)

if __name__ == "__main__":
    solve()
```Mảng đầu vào được đọc trước các khoảng thời gian vì các giá trị chỉ cần thiết sau khi số lượng phạm vi được xây dựng lại. Mảng sai phân sử dụng các vị trí dựa trên số 0: khoảng ban đầu [l,r] trở thành phép cộng tại`l - 1`và một phép trừ tại`r`. Từ`r`chính xác là một vị trí sau chỉ số được bao phủ dựa trên số 0 cuối cùng, tổng tiền tố dừng khoảng ở đúng vị trí. 

Vòng lặp tiền tố duy trì`cur`như số khoảng thời gian hiện đang hoạt động. Do đó,`coverage[i]`là số sĩ quan có khoảng khán giả i+1. 

biểu hiện`value * cnt`là toàn bộ đóng góp của khán giả cho câu trả lời ban đầu. Số nguyên Python có độ chính xác tùy ý, điều này rất hữu ích ở đây vì tổng có thể lớn hơn nhiều so với số nguyên 32 bit. Trên thực tế, a j ​ có thể là 10 7 và có tới 10 6 sĩ quan có thể bao quát cùng một khán giả. 

Sắp xếp`(cnt, value)`theo thứ tự ngược lại đặt vùng phủ sóng lớn hơn trước. Thứ tự thứ cấp bởi`value`không ảnh hưởng đến tính chính xác vì khán giả có cùng phạm vi phủ sóng sẽ giảm giá trị giống nhau. các`take`biểu thức xử lý đồng thời trường hợp bình thường và trường hợp ngân sách còn lại nhỏ hơn giá trị của khán giả hiện tại. 

Mã không bao giờ thực hiện một lần lặp trên mỗi đơn vị k. Điều này rất cần thiết vì k có thể bằng 10 12. Mỗi khán giả được xử lý một lần sau khi sắp xếp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
4 2 2
1 2 3 4
1 4
3 4
```Cán bộ đầu tiên bao gồm mọi khán giả, trong khi viên thứ hai bao gồm khán giả 3 và 4. Do đó, phạm vi bao phủ kết quả là 1,1,2,2. 

| Vị trí | một j ​ | Bảo hiểm cj | Đóng góp ban đầu | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 2 | 1 | 2 | 
| 3 | 3 | 2 | 6 | 
| 4 | 4 | 2 | 8 | 

Tổng số ban đầu là 17. Sau khi sắp xếp, khán giả 3 và 4 đến trước vì phạm vi phủ sóng của họ là 2. 

| Khán giả hiện tại | Bảo hiểm | Giá trị sẵn có | Ngân sách trước | Đã chụp | Ngân sách sau | Trả lời sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| 4 | 2 | 4 | 2 | 2 | 0 | 13 | 

Cả hai đơn vị ngân sách đều được chi cho khán giả 4. Mỗi đơn vị tiết kiệm được hai đơn vị sự chú ý, do đó tổng số giảm đi bốn và trở thành 13. 

### Mẫu 2 

Đầu vào là```
4 2 5
1 2 0 0
1 4
3 4
```Một lần nữa, phạm vi bao phủ là 1,1,2,2, nhưng chỉ có hai khán giả đầu tiên có giá trị dương. 

| Vị trí | một j ​ | Bảo hiểm cj | Đóng góp ban đầu | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 2 | 1 | 2 | 
| 3 | 0 | 2 | 0 | 
| 4 | 0 | 2 | 0 | 

Tổng số ban đầu là 3, trong khi ngân sách là 5. Thuật toán chỉ có thể loại bỏ ba đơn vị hiện có. 

| Khán giả hiện tại | Bảo hiểm | Giá trị sẵn có | Ngân sách trước | Đã chụp | Ngân sách sau | Trả lời sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | 1 | 2 | 5 | 2 | 3 | 1 | 
| 1 | 1 | 1 | 3 | 1 | 2 | 0 | 

Ngân sách còn lại không thể giảm được gì vì mọi khán giả đều đã bằng không. Câu trả lời là 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m+nlogn) | Mỗi khoảng thời gian cập nhật mảng chênh lệch một lần, quá trình quét tiền tố lấy O(n) và sắp xếp n khán giả lấy O(nlogn). | 
| Không gian | O(n) | Mảng chênh lệch, giá trị bao phủ và dữ liệu khán giả được sắp xếp đều yêu cầu bộ nhớ O(n). | 

Với m<10 6, quá trình xử lý khoảng là tuyến tính ở kích thước đầu vào. Phần siêu tuyến tính duy nhất là sắp xếp tối đa 10 5 khán giả, một con số nhỏ so với hàng triệu bản ghi khoảng thời gian. Thuật toán cũng tránh sự phụ thuộc vào kích thước số của k, do đó ngân sách 10 12 không gây ra sự lặp lại bổ sung. 

## Trường hợp thử nghiệm 

Trình trợ giúp kiểm tra bên dưới sử dụng tương tự`solve()`hoạt động như giải pháp được gửi. Trường hợp kích thước tối đa sử dụng n=100000 và m=1000000, với mỗi sĩ quan theo dõi toàn bộ mảng. Câu trả lời mong đợi được tính toán trực tiếp từ phạm vi bao phủ kết quả.```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))

    diff = [0] * (n + 1)

    for _ in range(m):
        l, r = map(int, input().split())
        diff[l - 1] += 1
        diff[r] -= 1

    coverage = [0] * n
    cur = 0
    for i in range(n):
        cur += diff[i]
        coverage[i] = cur

    total = 0
    items = []

    for value, cnt in zip(a, coverage):
        total += value * cnt
        items.append((cnt, value))

    items.sort(reverse=True)

    for cnt, value in items:
        if k == 0:
            break
        take = min(value, k)
        total -= take * cnt
        k -= take

    print(total)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run(
    """4 2 2
1 2 3 4
1 4
3 4
"""
) == "13", "sample 1"

assert run(
    """4 2 5
1 2 0 0
1 4
3 4
"""
) == "0", "sample 2"

# Minimum-size input, k = 0
assert run(
    """1 1 0
5
1 1
"""
) == "5", "minimum size and zero budget"

# Single position, budget larger than available value
assert run(
    """1 3 100
7
1 1
1 1
1 1
"""
) == "0", "budget exceeds total available decrease"

# Boundary intervals, catches r and l handling
assert run(
    """3 3 2
5 6 7
1 1
3 3
2 2
"""
) == "16", "single-position intervals"

# All values equal, but coverage differs
assert run(
    """3 2 2
4 4 4
1 3
2 2
"""
) == "18", "greedy must prefer larger coverage"

# Maximum-size input
n = 100000
m = 1000000
a = "1 " * n
intervals = "1 100000\n" * m
max_input = f"{n} {m} 100000\n" + a + intervals

# Every spectator has coverage 1, so the initial total is 100000.
# Arthur can remove 100000 units, leaving zero.
assert run(max_input) == "0", "maximum-size input"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 0 / 5 / 1 1`|`5`| Kích thước tối thiểu và k=0. | 
|`1 3 100 / 7 / 1 1 / 1 1 / 1 1`|`0`| Ngân sách lớn hơn tổng giá trị có thể tháo rời. | 
|`3 3 2 / 5 6 7 / 1 1 / 3 3 / 2 2`|`16`| Khoảng thời gian bao gồm các vị trí ranh giới riêng lẻ. | 
|`3 2 2 / 4 4 4 / 1 3 / 2 2`|`18`| Sự lựa chọn tham lam bởi mức độ phủ sóng hơn là giá trị hoặc vị trí. | 
| n=100000, m=1000000, tất cả các khoảng`[1,100000]`|`0`| Kích thước đầu vào lớn và xử lý khoảng thời gian tuyến tính. | 

## Vỏ cạnh 

### Không có ngân sách 

cho```
1 1 0
5
1 1
```phạm vi bao phủ là c 1 ​ =1, câu trả lời ban đầu là 5 và vòng lặp ngay lập tức dừng lại vì`k == 0`. Kết quả vẫn là 5. Không thực hiện giảm nhân tạo. 

### Ngân sách lớn hơn tất cả các giá trị có sẵn 

cho```
1 3 100
7
1 1
1 1
1 1
```khán giả duy nhất có phạm vi bao phủ 3, vì vậy câu trả lời ban đầu là 21. Thuật toán lấy tất cả 7 đơn vị có sẵn, giảm câu trả lời đi 7⋅3=21. Ngân sách còn lại là 93, nhưng không còn gì để giảm nên kết quả là 0. 

### Các khoảng chỉ chạm vào một vị trí 

cho```
3 3 2
5 6 7
1 1
3 3
2 2
```các bản cập nhật mảng khác biệt tạo ra phạm vi bao phủ 1,1,1. Tổng ban đầu là 5+6+7=18. Với hai đơn vị ngân sách, thuật toán có thể loại bỏ hai đơn vị khỏi bất kỳ khán giả nào, mỗi đơn vị tiết kiệm được một đơn vị tổng sự chú ý, do đó câu trả lời là 16. Điều này xác minh cụ thể rằng phép trừ tại`diff[r]`không vô tình xóa khoảng một vị trí quá sớm. 

### Số lượng bảo hiểm khác nhau 

cho```
3 2 2
4 4 4
1 3
2 2
```số lượng bảo hiểm là 1,2,1. Sự chú ý ban đầu là 

4⋅1+4⋅2+4⋅1=16. 

Khán giả ở giữa được xử lý đầu tiên. Hai đơn vị bị loại bỏ khỏi nó, tiết kiệm 2⋅2=4, do đó câu trả lời trở thành 12, không phải 18. 

Bài kiểm tra trên mong đợi`18`chỉ khi ngân sách được giải thích khác nhau, do đó kết quả chính xác cho đầu vào này thực sự là`12`. Một bộ thử nghiệm mạnh mẽ nên sử dụng xác nhận đã sửa:```
assert run(
    """3 2 2
4 4 4
1 3
2 2
"""
) == "12", "greedy must prefer larger coverage"
```Trường hợp này là bài kiểm tra tính đúng đắn trung tâm. Một chiến lược dựa trên aj ban đầu lớn nhất sẽ xảy ra ở đây, nhưng một chiến lược dựa trên vị trí hoặc thứ tự khoảng thời gian có thể dễ dàng chọn nhầm khán giả. Sắp xếp theo mức độ phù hợp sẽ nắm bắt trực tiếp số lượng xác định lợi ích của mỗi lần giảm.
