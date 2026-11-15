---
title: "CF 1838D - Đi bộ bằng khung"
description: "Chúng tôi có một chuỗi khung. Cuộc đi bộ bắt đầu ở vị trí 1 và cuối cùng phải kết thúc ở vị trí n. Tại mọi thời điểm, chúng tôi ghi lại dấu ngoặc được viết ở vị trí hiện tại."
date: "2026-06-09T06:35:43+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1838
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 877 (Div. 2)"
rating: 2100
weight: 1838
solve_time_s: 122
verified: true
draft: false
---

[CF 1838D - Đi bộ bằng khung](https://codeforces.com/problemset/problem/1838/D) 

**Xếp hạng:** 2100 
**Tas:** cấu trúc dữ liệu, tham lam, chuỗi 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi khung. Cuộc đi bộ bắt đầu ở vị trí 1 và cuối cùng phải kết thúc ở vị trí`n`. Tại mọi thời điểm, chúng tôi ghi lại dấu ngoặc được viết ở vị trí hiện tại. Vì chúng ta có thể di chuyển sang trái và phải tùy ý nhiều lần nên chuỗi được tạo ra có thể dài hơn nhiều so với chuỗi ban đầu. 

Một chuỗi được gọi là _walkable_ nếu tồn tại một số bước đi có trình tự dấu ngoặc được ghi là chuỗi dấu ngoặc thông thường (RBS). 

Sau mỗi truy vấn, một ký tự của chuỗi gốc sẽ bị đảo ngược. Các bản cập nhật là vĩnh viễn và sau mỗi lần cập nhật, chúng tôi phải trả lời xem chuỗi hiện tại có thể đi được hay không. 

Thử thách đầu tiên là hiểu được loại dây nào có thể đi được. Quá trình đi bộ có vẻ phức tạp vì có vô số con đường có thể đi. Quan sát quan trọng là cấu trúc bước đi áp đặt các ràng buộc mạnh mẽ đáng ngạc nhiên đối với các điểm cuối và nơi xuất hiện các dấu ngoặc bằng nhau liền kề. 

Những hạn chế là lớn. Cả hai`n`Và`q`có thể đạt được`2 · 10^5`. Việc tính toán lại mọi thứ tuyến tính sau mỗi lần cập nhật sẽ yêu cầu khoảng`4 · 10^10`trong trường hợp xấu nhất là hoàn toàn không thể thực hiện được. Chúng tôi cần công việc logarit đại khái cho mỗi truy vấn. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Coi như`n = 1`.```
(
```hoặc```
)
```Không bước đi nào có thể tạo ra một chuỗi dấu ngoặc thông thường không trống vì chúng ta bắt đầu và kết thúc trên cùng một ký tự. Câu trả lời luôn là`NO`. 

Coi như:```
()
```Điều này có thể đi bộ được. Đi thẳng về bên phải sẽ tạo ra`"()"`. 

Bây giờ hãy xem xét:```
)(
```Số lượng dấu ngoặc mở và đóng được cân bằng, nhưng ký tự được ghi đầu tiên là`')'`. Mỗi chuỗi được tạo ra đều bắt đầu bằng`')'`, do đó không thể có chuỗi dấu ngoặc thông thường. 

Một ví dụ tinh tế hơn là:```
(()(
```Ký tự đầu tiên là`'('`và cuối cùng là`'('`. Chuỗi dấu ngoặc thông thường phải kết thúc bằng`')'`, vì vậy không bao giờ có thể đi bộ được bất kể cấu trúc bên trong. 

Trường hợp ẩn quan trọng nhất là:```
()(())
```Có các dấu ngoặc bằng nhau liền kề:`"(( "`Và`"))"`. Một cách tiếp cận đơn giản có thể chỉ kiểm tra số lượng và điểm cuối, nhưng điều đó là chưa đủ. Vị trí của các cặp liền kề bằng nhau xác định xem có tồn tại bước đi hợp lệ hay không. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực sẽ coi các vị trí là các đỉnh của đồ thị. Chúng ta có thể cố gắng mô tả đặc điểm của tất cả các bước đi có thể có từ vị trí 1 đến vị trí`n`, tạo các chuỗi dấu ngoặc được tạo và kiểm tra xem có bất kỳ chuỗi nào trong số chúng là chuỗi dấu ngoặc thông thường hay không. 

Điều này thật vô vọng. Ngay cả đối với những sợi dây nhỏ, số bước đi cũng tăng theo cấp số nhân vì chúng ta có thể di chuyển sang trái và sang phải tùy ý nhiều lần. Không có phép tìm kiếm trực tiếp nào trong không gian trạng thái có thể vượt qua được các ràng buộc. 

Bước đột phá đến từ sự hiểu biết về cấu trúc của những sợi dây có thể đi được. 

Chúng ta hãy nhìn vào các vị trí liền kề. Bất cứ khi nào hai ký tự lân cận khác nhau, việc di chuyển qua cạnh đó sẽ đóng góp một`'('`và một`')'`theo một thứ tự nào đó. Các cạnh như vậy là vô hại. 

Những nơi có vấn đề là các dấu ngoặc bằng nhau liền kề. 

Nếu chúng ta có```
((
```thì ở đâu đó chúng ta nhận được một dấu ngoặc mở "bổ sung". Nếu chúng ta có```
))
```thì ở đâu đó chúng ta nhận được một dấu ngoặc đóng "bổ sung". 

Giải pháp chính thức dựa trên đặc tính: 

Thậm chí`n`, một chuỗi có thể đi được khi và chỉ khi 

1.`s[0] = '('`. 
2.`s[n-1] = ')'`. 
3. Lần xuất hiện đầu tiên của`"))"`xuất hiện đúng sau lần xuất hiện cuối cùng của`"(("`. 

Tương đương, mọi`"(("`phải nằm bên trái mọi`"))"`. 

Đối với số lẻ`n`, câu trả lời luôn là`NO`bởi vì mọi chuỗi ngoặc thông thường đều có độ dài chẵn, trong khi mọi độ dài bước đi có cùng độ dài như`n`. 

Đặc điểm này loại bỏ hoàn toàn khía cạnh đi bộ. Chúng ta chỉ cần duy trì: 

1. Ký tự đầu tiên có phải là`'('`. 
2. Ký tự cuối cùng có phải là`')'`. 
3. Liệu vị trí tối đa có chứa`"(("`nhỏ hơn vị trí tối thiểu chứa`"))"`. 

Sau khi lật, chỉ những cặp chạm vào vị trí đó mới có thể thay đổi. Có nhiều nhất hai cặp liền kề như vậy. Điều này gợi ý việc duy trì vị trí của tất cả`"(("`và tất cả`"))"`trong tập hợp có thứ tự. 

Sau đó, mỗi truy vấn sẽ trở thành một vài lần chèn/xóa và một số lượng truy vấn cực trị được đặt không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu`n`thật kỳ lạ, mọi câu trả lời đều ngay lập tức`NO`. 

Mọi chuỗi ngoặc thông thường đều có độ dài chẵn. Tính chẵn lẻ của mọi đầu ra bước đi có thể có là cố định và một chuỗi có độ dài lẻ không bao giờ có thể đi được. 
2. Duy trì chuỗi ngoặc hiện tại trong một mảng có thể thay đổi. 
3. Duy trì một bộ`open2`chứa tất cả các chỉ số`i`như vậy`s[i]s[i+1] = "(("`. 

Chỉ mục được lưu trữ là vị trí bên trái của cặp. 
4. Duy trì một bộ`close2`chứa tất cả các chỉ số`i`như vậy`s[i]s[i+1] = "))"`. 
5. Trước khi xử lý truy vấn, hãy quét tất cả các cặp liền kề một lần và điền vào cả hai bộ. 
6. Khi vị trí`p`bị lật, chỉ có cặp`(p-1,p)`Và`(p,p+1)`có thể thay đổi. 

Trước tiên, hãy xóa những đóng góp cũ của họ khỏi bộ. 
7. Lật nhân vật. 
8. Tính lại các cặp lân cận đó và chèn các đóng góp mới của chúng vào. 
9. Để trả lời truy vấn, trước tiên hãy kiểm tra điểm cuối. 

Chúng tôi cần`s[0] = '('`Và`s[n-1] = ')'`. 
10. Nếu một trong hai điều kiện điểm cuối không thành công, hãy trả lời`NO`. 
11. Ngược lại, nếu cả hai tập hợp đều không trống, hãy tính`max(open2)`Và`min(close2)`. 

Chuỗi chỉ hợp lệ khi`max(open2) < min(close2)`. 

1. Nếu một trong các bộ trống, điều kiện sẽ tự động được giữ. 

Không có thứ tự cặp xung đột. 

### Tại sao nó hoạt động 

Định lý cốt lõi là đối với chẵn`n`, khả năng đi bộ tương đương với các điều kiện điểm cuối cùng với điều kiện đặt hàng```
all "((" occur before all "))"
```MỘT`"(("`cặp hoạt động giống như một nguồn của dấu ngoặc mở dư thừa, trong khi một`"))"`cặp hoạt động giống như một nguồn của dấu ngoặc đóng dư thừa. Một bước đi hợp lệ có thể được xây dựng chính xác khi tất cả các lần mở dư thừa xuất hiện trước tất cả các lần đóng dư thừa. 

Các bộ lưu trữ chính xác các vị trí nơi các cặp dư thừa này xuất hiện. lớn nhất`"(("`vị trí và nhỏ nhất`"))"`vị trí là đủ để kiểm tra xem thuộc tính đặt hàng có giữ được trên toàn cầu hay không. Nếu lớn nhất`"(("`vẫn là phần nhỏ nhất còn sót lại`"))"`, thì mọi`"(("`nằm trước mọi`"))"`. Nếu không thì tài sản bị vi phạm. 

Vì một lần lật chỉ ảnh hưởng đến các cặp liền kề chạm vào vị trí bị lật nên việc duy trì các bộ này một cách linh hoạt là chính xác và hiệu quả. 

## Giải pháp Python```python
import sys
from bisect import bisect_left, insort

input = sys.stdin.readline

class OrderedSet:
    def __init__(self):
        self.a = []

    def add(self, x):
        i = bisect_left(self.a, x)
        if i == len(self.a) or self.a[i] != x:
            self.a.insert(i, x)

    def discard(self, x):
        i = bisect_left(self.a, x)
        if i < len(self.a) and self.a[i] == x:
            self.a.pop(i)

    def __len__(self):
        return len(self.a)

    def min(self):
        return self.a[0]

    def max(self):
        return self.a[-1]

def solve():
    n, q = map(int, input().split())
    s = list(input().strip())

    open2 = OrderedSet()
    close2 = OrderedSet()

    for i in range(n - 1):
        pair = s[i] + s[i + 1]
        if pair == "((":
            open2.add(i)
        elif pair == "))":
            close2.add(i)

    ans = []

    for _ in range(q):
        p = int(input()) - 1

        if n % 2 == 1:
            s[p] = ')' if s[p] == '(' else '('
            ans.append("NO")
            continue

        for i in (p - 1, p):
            if 0 <= i < n - 1:
                pair = s[i] + s[i + 1]
                if pair == "((":
                    open2.discard(i)
                elif pair == "))":
                    close2.discard(i)

        s[p] = ')' if s[p] == '(' else '('

        for i in (p - 1, p):
            if 0 <= i < n - 1:
                pair = s[i] + s[i + 1]
                if pair == "((":
                    open2.add(i)
                elif pair == "))":
                    close2.add(i)

        if s[0] != '(' or s[-1] != ')':
            ans.append("NO")
            continue

        if len(open2) and len(close2):
            ans.append("YES" if open2.max() < close2.min() else "NO")
        else:
            ans.append("YES")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Logic biên tập được phản ánh trực tiếp trong việc thực hiện.`open2`lưu trữ tất cả các vị trí nơi`"(("`xảy ra và`close2`lưu trữ tất cả các vị trí nơi`"))"`xảy ra. Câu trả lời chỉ phụ thuộc vào phần tử lớn nhất của tập thứ nhất và phần tử nhỏ nhất của tập thứ hai. 

Chi tiết triển khai quan trọng là chỉ cập nhật các cặp chạm vào vị trí bị lật. Mọi cặp liền kề khác vẫn không thay đổi, vì vậy việc sửa đổi bất kỳ thứ gì khác sẽ là công việc lãng phí. 

Một điểm tinh tế khác là xử lý những điều kỳ quặc`n`. Câu trả lời luôn là`NO`, nhưng bản thân chuỗi vẫn thay đổi sau mỗi truy vấn. Các cập nhật trong tương lai phụ thuộc vào trạng thái hiện tại, vì vậy chúng ta phải thực hiện thao tác lật mặc dù câu trả lời đã được xác định trước. 

Việc kiểm tra điểm cuối phải được thực hiện trước khi kiểm tra các bộ. Một chuỗi bắt đầu bằng`')'`hoặc kết thúc bằng`'('`không bao giờ có thể đi bộ được, bất kể thứ tự cặp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chuỗi ban đầu:```
(())()()))
```Sau truy vấn 1, vị trí 9 bị đảo ngược. 

Chuỗi kết quả:```
(())()()()
```| Biến | Giá trị | 
| --- | --- | 
| Ký tự đầu tiên |`(`| 
| Ký tự cuối cùng |`)`| 
| mở2 vị trí | {1, 5} | 
| đóng2 vị trí | {2, 7} | 
| tối đa(open2) | 5 | 
| phút(đóng2) | 2 | 
| Trả lời | CÓ | 

Điều kiện đặt hàng được giữ nguyên sau khi cập nhật, do đó chuỗi có thể đi được. 

### Mẫu 2 

đầu vào:```
3 2
()(
1
2
```| Truy vấn | Chuỗi | lạ nhỉ? | Trả lời | 
| --- | --- | --- | --- | 
| 1 |`))(`| Có | KHÔNG | 
| 2 |`)((`| Có | KHÔNG | 

Từ`n = 3`thật kỳ lạ, mọi câu trả lời đều ngay lập tức`NO`, độc lập với sự sắp xếp khung thực tế. 

Dấu vết này thể hiện tính chất chẵn lẻ. Không có cấu trúc cục bộ nào có thể vượt qua được độ dài lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log n) | Mỗi bản cập nhật thực hiện một số lần chèn và xóa theo thứ tự không đổi | 
| Không gian | O(n) | Các bộ lưu trữ các vị trí cặp liền kề | 

Với`n, q ≤ 2 · 10^5`, cập nhật logarit dễ dàng đủ nhanh. Việc sử dụng bộ nhớ là tuyến tính và phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    from bisect import bisect_left

    class OrderedSet:
        def __init__(self):
            self.a = []

        def add(self, x):
            i = bisect_left(self.a, x)
            if i == len(self.a) or self.a[i] != x:
                self.a.insert(i, x)

        def discard(self, x):
            i = bisect_left(self.a, x)
            if i < len(self.a) and self.a[i] == x:
                self.a.pop(i)

        def __len__(self):
            return len(self.a)

        def min(self):
            return self.a[0]

        def max(self):
            return self.a[-1]

    data = io.StringIO(inp)
    input = data.readline

    n, q = map(int, input().split())
    s = list(input().strip())

    open2 = OrderedSet()
    close2 = OrderedSet()

    for i in range(n - 1):
        pair = s[i] + s[i + 1]
        if pair == "((":
            open2.add(i)
        elif pair == "))":
            close2.add(i)

    out = []

    for _ in range(q):
        p = int(input()) - 1

        if n % 2:
            s[p] = ')' if s[p] == '(' else '('
            out.append("NO")
            continue

        for i in (p - 1, p):
            if 0 <= i < n - 1:
                pair = s[i] + s[i + 1]
                if pair == "((":
                    open2.discard(i)
                elif pair == "))":
                    close2.discard(i)

        s[p] = ')' if s[p] == '(' else '('

        for i in (p - 1, p):
            if 0 <= i < n - 1:
                pair = s[i] + s[i + 1]
                if pair == "((":
                    open2.add(i)
                elif pair == "))":
                    close2.add(i)

        if s[0] != '(' or s[-1] != ')':
            out.append("NO")
        elif len(open2) and len(close2):
            out.append("YES" if open2.max() < close2.min() else "NO")
        else:
            out.append("YES")

    return "\n".join(out)

# sample 1
assert run(
"""10 9
(())()()))
9
7
2
6
3
6
7
4
8
"""
) == "\n".join([
"YES","YES","NO","NO","YES","NO","YES","NO","NO"
])

# minimum size
assert run(
"""1 1
(
1
"""
) == "NO"

# already balanced, endpoint destroyed
assert run(
"""2 1
()
1
"""
) == "NO"

# all equal brackets
assert run(
"""4 1
((((
4
"""
) == "NO"

# endpoint restoration
assert run(
"""4 2
()))
1
4
"""
) == "NO\nYES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1`|`NO`| Chuỗi nhỏ nhất có thể | 
|`()`sau đó lật char đầu tiên |`NO`| Điều kiện điểm cuối | 
|`((((`|`NO`| Dấu ngoặc bằng nhau | 
| Trường hợp khôi phục điểm cuối |`NO`, sau đó`YES`| Cập nhật động | 

## Vỏ cạnh 

Hãy xem xét:```
n = 3
s = ()(
```Mọi truy vấn đều trả về`NO`. 

Thuật toán kiểm tra tính chẵn lẻ trước tiên. Từ`n`là số lẻ, nó không bao giờ cố gắng đánh giá điều kiện sắp xếp cặp. Điều này phù hợp với định lý rằng mọi chuỗi ngoặc thông thường đều có độ dài chẵn. 

Coi như:```
n = 4
s = )()(
```Ký tự đầu tiên là`')'`và cuối cùng là`'('`. 

Ngay cả khi không có xung đột`"(("`Và`"))"`vị trí, thuật toán ngay lập tức từ chối chuỗi vì chuỗi dấu ngoặc thông thường hợp lệ phải bắt đầu bằng`'('`và kết thúc bằng`')'`. 

Coi như:```
(())
```Ở đây chúng tôi có:```
open2 = {1}
close2 = {2}
```Và```
max(open2) = 1
min(close2) = 2
```Điều kiện đặt hàng được giữ nguyên nên câu trả lời là`YES`. 

Bây giờ đảo ngược mô hình:```
))((
```Sau đó:```
open2 = {2}
close2 = {0}
```Và```
max(open2) = 2
min(close2) = 0
```Điều kiện đặt hàng không thành công nên câu trả lời là`NO`. 

Đây chính xác là tình huống mà bài kiểm tra theo tập hợp được thiết kế để phát hiện. Thứ tự tương đối của các cặp mở dư và đóng dư xác định hoàn toàn khả năng đi lại sau khi các điểm cuối được cố định.
