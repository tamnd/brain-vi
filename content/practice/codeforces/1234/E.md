---
title: "CF 1234E - Hoán vị đặc biệt"
description: "Chúng ta được cho một dãy x có độ dài m. Với mọi giá trị i từ 1 đến n, chúng ta xây dựng một hoán vị đặc biệt: pi = [i, 1, 2, ..., i-1, i+1, ..., n] Hoán vị này khác với hoán vị đồng nhất chỉ vì giá trị i đã được chuyển lên phía trước."
date: "2026-06-11T22:20:19+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1234
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 590 (Div. 3)"
rating: 2000
weight: 1234
solve_time_s: 106
verified: true
draft: false
---

[CF 1234E - Hoán vị đặc biệt](https://codeforces.com/problemset/problem/1234/E) 

**Xếp hạng:** 2000 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một trình tự`x`chiều dài`m`. Với mọi giá trị`i`từ`1`ĐẾN`n`, chúng ta xây dựng một hoán vị đặc biệt:`p_i = [i, 1, 2, ..., i-1, i+1, ..., n]`Hoán vị này khác với hoán vị đồng nhất chỉ vì giá trị`i`đã được chuyển lên phía trước. 

Với mỗi hoán vị, chúng ta phải tính tổng khoảng cách giữa các phần tử liên tiếp của`x`khi những phần tử đó được xem bên trong hoán vị đó. Nếu hai giá trị lân cận trong`x`chiếm vị trí`a`Và`b`trong hoán vị, chúng đóng góp`|a-b|`để trả lời. 

Nhiệm vụ là xuất giá trị này cho mọi hoán vị`p_1, p_2, ..., p_n`. 

Các ràng buộc là điều đầu tiên thúc đẩy giải pháp. Cả hai`n`Và`m`có thể đạt được`2·10^5`. có`n`hoán vị khác nhau và mỗi câu trả lời phụ thuộc vào`m-1`cặp liền kề từ`x`. Một tính toán trực tiếp sẽ yêu cầu khoảng`n·m`, trở thành về`4·10^10`hoạt động trong trường hợp xấu nhất. Điều đó là hoàn toàn không thể trong vòng hai giây. 

Cấu trúc của các hoán vị vô cùng đặc biệt. Mọi hoán vị đều có được từ danh tính bằng cách di chuyển chính xác một giá trị đến vị trí đầu tiên. Điều đó có nghĩa là hầu hết các vị trí không thay đổi và chỉ một tập hợp con khoảng cách nhỏ bị ảnh hưởng khi chúng ta chuyển từ chỉ số hoán vị này sang chỉ số hoán vị khác. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
3 3
2 2 2
```Mỗi cặp liền kề là`(2,2)`, khoảng cách của nó luôn bằng 0 bất kể hoán vị. Đầu ra đúng là:```
0 0 0
```Giải pháp giả định các giá trị liền kề là khác biệt có thể đưa ra các bản cập nhật không chính xác. 

Coi như:```
4 2
1 4
```Câu trả lời cho`p_1`là`|1-4|=3`. Vì`p_4`, vị trí trở thành`(2,1)`, cho khoảng cách`1`. Khoảng cách thay đổi đáng kể khi một điểm cuối bằng phần tử được di chuyển. 

Coi như:```
5 2
2 3
```Cặp này không bao giờ chứa nhiều giá trị, tuy nhiên mọi câu trả lời vẫn phụ thuộc vào vị trí của phần tử được di chuyển so với khoảng`[2,3]`. Bất kỳ cách tiếp cận nào chỉ cập nhật điểm cuối và bỏ qua hiệu ứng khoảng thời gian sẽ tạo ra câu trả lời sai. 

Thách thức trọng tâm là hiểu làm thế nào một cặp liền kề đóng góp vào tất cả các hoán vị cùng một lúc. 

## Phương pháp tiếp cận 

Giải pháp vũ phu rất đơn giản. Với mỗi hoán vị`p_i`, tính toán vị trí của mọi giá trị, sau đó lặp qua`m-1`các cặp liền kề`x`và tính tổng các khoảng cách tương ứng. 

Hàm vị trí trong hoán vị`p_k`là:

-`pos(k)=1`-`pos(v)=v+1`vì`v<k`-`pos(v)=v`vì`v>k`Sử dụng công thức này, mỗi khoảng cách có thể được đánh giá theo thời gian không đổi. Thật không may là chúng ta vẫn có`n`hoán vị và`m-1`cặp, dẫn đến`O(nm)`công việc. Với cả hai tham số bằng`2·10^5`, số lượng hoạt động là khoảng bốn mươi tỷ. 

Quan sát quan trọng là câu trả lời tổng thể là tổng của các cặp liền kề`x`. 

Thay vì sửa một hoán vị và đánh giá từng cặp, chúng ta đảo ngược quan điểm. Lấy một cặp liền kề`(a,b)`và xác định sự đóng góp của nó thay đổi như thế nào trên tất cả các hoán vị. 

Cho phép```
g_k(a,b) = |pos_k(a)-pos_k(b)|
```Ở đâu`k`là giá trị được chuyển lên phía trước. 

Đối với hầu hết các giá trị của`k`, khoảng cách không thay đổi so với hoán vị danh tính. Chỉ khi`k`bằng một điểm cuối hoặc nằm giữa hai điểm cuối thì khoảng cách có thay đổi không. 

Điều này có nghĩa là một cặp chỉ ảnh hưởng đến một khoảng chỉ số hoán vị. Vì chỉ có`m-1`các cặp, chúng ta có thể xử lý từng cặp một cách độc lập và tích lũy hiệu ứng của nó thành một mảng khác biệt. 

Câu trả lời hoán vị danh tính rất dễ tính toán:```
base = Σ |x_i - x_{i+1}|
```Sau đó, đối với mỗi cặp liền kề, chúng tôi tính toán mức độ đóng góp của nó khác với câu trả lời cơ sở cho tất cả các hoán vị có liên quan. Những thay đổi này có thể được thêm vào bằng cách sử dụng các bản cập nhật phạm vi. 

Toàn bộ bài toán trở thành bài toán cộng khoảng trên chỉ số hoán vị`k`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| Tối ưu | O(n + m) | O(n) | Đã chấp nhận | 

## Suy ra công thức đóng góp 

Giả sử`a < b`. 

Trong hoán vị danh tính, sự đóng góp là```
b-a
```Bây giờ hãy xem xét giá trị di chuyển`k`về phía trước. 

Nếu như`k=a`, sau đó```
pos(a)=1
pos(b)=b
```Đóng góp trở thành```
b-1
```Sự khác biệt từ cơ sở:```
(b-1)-(b-a)=a-1
```Nếu như`k=b`, sau đó```
pos(a)=a+1
pos(b)=1
```Đóng góp trở thành```
a
```Sự khác biệt:```
a-(b-a)=2a-b
```Nếu như`a<k<b`, sau đó```
pos(a)=a+1
pos(b)=b
```Đóng góp trở thành```
b-a-1
```Sự khác biệt:```
-1
```Đối với tất cả những người khác`k`, mức đóng góp không thay đổi. 

Vì vậy, một cặp đóng góp:```
a-1          at k=a
-1           for a<k<b
2a-b         at k=b
0            elsewhere
```Mô tả nhỏ gọn này là toàn bộ giải pháp. 

## Hướng dẫn thuật toán 

1. Tính đáp án cho hoán vị đẳng thức. 

Cho phép```
ans = Σ |x_i - x_{i+1}|
```Mọi câu trả lời hoán vị sẽ bắt đầu từ giá trị này và sau đó nhận được sự điều chỉnh. 
2. Tạo một mảng có kích thước khác nhau`n+3`. 

Chúng tôi sẽ lưu trữ tất cả các thay đổi liên quan đến câu trả lời nhận dạng. 
3. Xử lý từng cặp liền kề`(u,v)`TRONG`x`. 

Nếu như`u=v`, mức đóng góp luôn bằng 0 và không cần cập nhật. 
4. Hãy để`a=min(u,v)`Và`b=max(u,v)`. 

Công thức dẫn xuất chỉ phụ thuộc vào các điểm cuối có thứ tự. 
5. Thêm cập nhật điểm cho`k=a`. 

Sự điều chỉnh là`a-1`. 
6. Thêm cập nhật điểm cho`k=b`. 

Sự điều chỉnh là`2*a-b`. 
7. Thêm bản cập nhật phạm vi vào`(a,b)`. 

Mọi chỉ số hoán vị đều nằm giữa`a`Và`b`nhận được`-1`. 

Đây là khoảng thời gian mà giá trị được di chuyển nằm giữa hai điểm cuối, giảm khoảng cách của chúng đi một. 
8. Sau khi tất cả các cặp được xử lý, hãy chuyển đổi mảng sai phân thành các giá trị hiệu chỉnh thực tế bằng cách sử dụng tổng tiền tố. 
9. Đối với mọi`k`từ`1`ĐẾN`n`, đầu ra```
base + correction[k]
```### Tại sao nó hoạt động 

Đối với mỗi cặp giá trị liền kề, chúng tôi tính toán rõ ràng sự thay đổi chính xác về khoảng cách của nó so với hoán vị nhận dạng. Đạo hàm trên sử dụng hết tất cả các vị trí có thể có của giá trị được di chuyển`k`. 

Một cặp chỉ đóng góp trong ba trường hợp: khi`k`bằng điểm cuối bên trái, khi`k`bằng điểm cuối bên phải hoặc khi`k`nằm chặt chẽ giữa chúng. Các quy tắc cập nhật mã hóa chính xác những trường hợp này. 

Bởi vì câu trả lời tổng thể là tổng đóng góp của tất cả các cặp liền kề, tính tuyến tính cho phép chúng ta cộng tất cả các hiệu chỉnh cặp một cách độc lập. Sau khi tất cả các cập nhật được tích lũy, phần hiệu chỉnh được lưu trữ cho hoán vị`k`bằng tổng các thay đổi đóng góp của mỗi cặp, đó chính xác là sự khác biệt giữa`f(p_k)`và câu trả lời nhận dạng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def add_range(diff, l, r, val):
    if l > r:
        return
    diff[l] += val
    diff[r + 1] -= val

def solve():
    n, m = map(int, input().split())
    x = list(map(int, input().split()))

    base = 0
    diff = [0] * (n + 3)

    for i in range(m - 1):
        u = x[i]
        v = x[i + 1]

        if u == v:
            continue

        a = min(u, v)
        b = max(u, v)

        base += b - a

        diff[a] += a - 1
        diff[a + 1] -= a - 1

        if a + 1 <= b - 1:
            add_range(diff, a + 1, b - 1, -1)

        diff[b] += 2 * a - b
        diff[b + 1] -= (2 * a - b)

    cur = 0
    res = []

    for k in range(1, n + 1):
        cur += diff[k]
        res.append(str(base + cur))

    print(" ".join(res))

solve()
```Giải pháp bắt đầu bằng cách tính toán câu trả lời hoán vị danh tính. Điều này đóng vai trò là cơ sở để từ đó đưa ra tất cả các câu trả lời khác. 

Đối với mỗi cặp liền kề, chúng tôi áp dụng ba quy tắc hiệu chỉnh được rút ra trước đó. Cập nhật điểm được triển khai dưới dạng cập nhật phạm vi độ dài một bên trong mảng chênh lệch. Khoảng thời gian`(a,b)`nhận được một bản cập nhật phạm vi của`-1`. 

Mảng khác biệt là rất quan trọng. Không có nó, việc cập nhật mọi chỉ mục giữa`a`Và`b`sẽ làm cho thuật toán bậc hai. Một mảng khác biệt biến mỗi lần sửa đổi khoảng thời gian thành một công việc không đổi. 

Một chi tiết tinh tế là bản cập nhật tại`k=b`. Giá trị của nó là`2a-b`, không`a-b`hoặc`a-1`. Điều này xuất phát trực tiếp từ khoảng cách chính xác khi`b`chính nó được chuyển lên phía trước. 

Một sai lầm dễ mắc phải khác là xử lý các giá trị liên tiếp bằng nhau. Khi`u=v`, khoảng cách luôn bằng không. Các công thức dẫn xuất giả định các điểm cuối khác biệt và nên được bỏ qua hoàn toàn. 

Tất cả số học vừa vặn thoải mái bên trong số nguyên 64 bit. Câu trả lời tối đa là theo thứ tự`m·n`, khoảng`4·10^10`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 4
1 2 3 4
```Các cặp liền kề là`(1,2)`,`(2,3)`,`(3,4)`. 

Câu trả lời cơ bản:```
1 + 1 + 1 = 3
```| Cặp | một | b | Cập nhật tại | Phạm vi | Cập nhật tại b | 
| --- | --- | --- | --- | --- | --- | 
| (1,2) | 1 | 2 | 0 | không | 0 | 
| (2,3) | 2 | 3 | +1 | không | +1 | 
| (3,4) | 3 | 4 | +2 | không | +2 | 

Các hiệu chỉnh tích lũy: 

| k | Sửa chữa | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 3 | 
| 4 | 2 | 

Câu trả lời cuối cùng: 

| k | Căn cứ | Sửa chữa | Trả lời | 
| --- | --- | --- | --- | 
| 1 | 3 | 0 | 3 | 
| 2 | 3 | 1 | 4 | 
| 3 | 3 | 3 | 6 | 
| 4 | 3 | 2 | 5 | 

Đầu ra:```
3 4 6 5
```Ví dụ này cho thấy rằng mặc dù câu trả lời nhận dạng chỉ là`3`, việc di chuyển các giá trị lớn hơn về phía trước sẽ làm tăng đồng thời nhiều khoảng cách giữa các cặp. 

### Mẫu 2 

đầu vào:```
5 5
2 1 5 3 5
```Câu trả lời cơ bản:```
1 + 4 + 2 + 2 = 9
```Đóng góp theo cặp: 

| Cặp | một | b | Hiệu ứng | 
| --- | --- | --- | --- | 
| (2,1) | 1 | 2 | cập nhật gần 1 và 2 | 
| (1,5) | 1 | 5 | khoảng [2,4] được -1 | 
| (5,3) | 3 | 5 | khoảng [4,4] được -1 | 
| (3,5) | 3 | 5 | khoảng [4,4] được -1 | 

Sau khi tích lũy tất cả các bản cập nhật: 

| k | Sửa chữa | Trả lời | 
| --- | --- | --- | 
| 1 | 0 | 9 | 
| 2 | -1 | 8 | 
| 3 | 3 | 12 | 
| 4 | -3 | 6 | 
| 5 | -1 | 8 | 

Đầu ra:```
9 8 12 6 8
```Dấu vết này cho thấy tầm quan trọng của việc cập nhật theo khoảng thời gian. Hầu hết các thay đổi không đến từ điểm cuối mà đến từ các giá trị nằm giữa các điểm cuối của cặp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Một lần vượt qua các cặp liền kề và một lần vượt qua tổng tiền tố | 
| Không gian | O(n) | Mảng khác biệt và mảng đầu ra | 

Các ràng buộc cho phép lên đến`2·10^5`các giá trị. MỘT`O(n+m)`thuật toán chỉ thực hiện vài trăm nghìn thao tác và dễ dàng phù hợp với thời gian giới hạn. Việc sử dụng bộ nhớ là tuyến tính trong`n`, thấp hơn nhiều so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m = map(int, input().split())
    x = list(map(int, input().split()))

    base = 0
    diff = [0] * (n + 3)

    for i in range(m - 1):
        u = x[i]
        v = x[i + 1]

        if u == v:
            continue

        a = min(u, v)
        b = max(u, v)

        base += b - a

        diff[a] += a - 1
        diff[a + 1] -= a - 1

        if a + 1 <= b - 1:
            diff[a + 1] -= 1
            diff[b] += 1

        val = 2 * a - b
        diff[b] += val
        diff[b + 1] -= val

    cur = 0
    ans = []

    for i in range(1, n + 1):
        cur += diff[i]
        ans.append(str(base + cur))

    return " ".join(ans)

# sample 1
assert run("4 4\n1 2 3 4\n") == "3 4 6 5"

# sample 2
assert run("5 5\n2 1 5 3 5\n") == "9 8 12 6 8"

# minimum sizes
assert run("2 2\n1 1\n") == "0 0"

# all equal values
assert run("5 4\n3 3 3 3\n") == "0 0 0 0 0"

# interval update stress
assert run("5 2\n1 5\n") == "4 3 3 3 1"

# symmetric pair
assert run("3 2\n2 3\n") == "1 2 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2 / 1 1`|`0 0`| Hạn chế tối thiểu | 
|`5 4 / 3 3 3 3`|`0 0 0 0 0`| Giá trị lặp lại | 
|`5 2 / 1 5`|`4 3 3 3 1`| Cập nhật khoảng thời gian dài | 
|`3 2 / 2 3`|`1 2 2`| Công thức hiệu chỉnh điểm cuối | 

## Vỏ cạnh 

Hãy xem xét:```
3 3
2 2 2
```Mỗi cặp liền kề có điểm cuối giống hệt nhau. Thuật toán ngay lập tức bỏ qua từng cặp vì`u == v`. Không có bản cập nhật nào được áp dụng, câu trả lời cơ bản vẫn bằng 0 và mọi câu trả lời hoán vị đều bằng 0:```
0 0 0
```Bây giờ hãy xem xét:```
5 2
1 5
```Câu trả lời cơ bản là`4`. Đối với cặp`(1,5)`, khoảng`(1,5)`nhận được`-1`, vậy hoán vị`2,3,4`tất cả đều mất một đơn vị. Tại`k=5`, việc hiệu chỉnh điểm cuối đặc biệt góp phần`-3`, đưa ra câu trả lời`1`. Đầu ra trở thành:```
4 3 3 3 1
```Điều này xác minh rằng bản cập nhật điểm cuối phù hợp`2a-b`được xử lý chính xác. 

Cuối cùng hãy xem xét:```
5 2
2 3
```Câu trả lời cơ bản là`1`. 

Các bản cập nhật là:```
k=2 : +1
k=3 : +1
```Không có khoảng cách tồn tại giữa các điểm cuối. Câu trả lời trở thành:```
1 2 2 1 1
```Điều này kiểm tra trường hợp ranh giới trong đó`b=a+1`, đảm bảo cập nhật theo khoảng thời gian không vô tình được áp dụng cho một phạm vi trống.
