---
title: "CF 102448B - Nỗi nôn nao của Beza"
description: "Đêm có thể được xem như một mảng gồm N vị trí. Vị trí thứ i lưu trữ đồ uống Beza đã tiêu thụ trong giờ thứ i. Thanh cung cấp M tên đồ uống và mỗi tên có một lượng rượu liên quan. Truy vấn loại 1 thay đổi vị trí mảng này thành đồ uống khác."
date: "2026-08-12T08:20:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102448
codeforces_index: "B"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2020"
rating: 0
weight: 102448
solve_time_s: 172
verified: true
draft: false
---

[CF 102448B - Nỗi nôn nao của Beza](https://codeforces.com/problemset/problem/102448/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đêm có thể được xem như một mảng gồm N vị trí. Vị trí thứ i lưu trữ đồ uống Beza đã tiêu thụ trong giờ thứ i. Thanh cung cấp M tên đồ uống và mỗi tên có một lượng rượu liên quan. 

Truy vấn loại 1 thay đổi vị trí mảng này thành đồ uống khác. Truy vấn loại 2 hỏi về một phân nhóm liền kề [L,R]: liệu tổng lượng rượu tiêu thụ trong những giờ đó có đủ để gây ra cảm giác nôn nao không? 

Giả sử khoảng thời gian chứa K=R−L+1 giờ. Beza dành 60K phút để uống rượu nên lượng rượu tối đa không gây nôn nao chỉ bằng một nửa số đó, tức là 30K. Theo đó, câu trả lời là`YES`chính xác khi nào 

i=L ∑ R ​ V i ​ >30(R−L+1), 

trong đó V i ​ là thể tích cồn của đồ uống hiện tại ở vị trí i. Bình đẳng mang lại`NO`, vì số tiền bằng giới hạn vẫn an toàn. 

N tên đồ uống đầu tiên mô tả mảng ban đầu. M dòng tiếp theo tạo thành một từ điển từ tên đồ uống đến thể tích rượu. Sau đó có Q hoạt động động. Tất cả các đại lượng liên quan đều có thể lớn tới 2⋅10 5, do đó, việc quét toàn bộ khoảng thời gian cho mỗi truy vấn có thể yêu cầu khoảng NQ=4⋅10 10 lần truy cập phần tử trong trường hợp xấu nhất. Đó là vượt xa những gì giới hạn 2 giây cho phép. Chúng tôi cần công việc logarit cho mỗi lần cập nhật và truy vấn. 

Có một số trường hợp ranh giới có thể âm thầm gây ra đáp án sai. Đầu tiên, khoảng thời gian một giờ phải sử dụng ngưỡng chính xác là 30 chứ không phải 60. Ví dụ:```
1 1 1
a
a 31
2 1 1
```có đầu ra`YES`, vì 31>30. Một giải pháp so sánh với 60(R−L+1) sẽ in sai`NO`. 

Trường hợp bình đẳng cũng có ý nghĩa. Coi như```
1 1 1
a
a 30
2 1 1
```Đầu ra là`NO`, vì 30 lít chính xác là lượng cho phép. Việc thực hiện bất cẩn bằng cách sử dụng`>=`thay vì`>`sẽ báo cáo tình trạng nôn nao không chính xác. 

Các bản cập nhật phải ảnh hưởng đến các truy vấn tiếp theo ngay lập tức. Ví dụ,```
2 2 3
a a
a 30
b 100
1 1 b
2 1 1
```sản xuất```
YES
```vì vị trí 1 trở thành`b`, có giá trị là 100. Sử dụng mảng ban đầu thay vì mảng hiện tại sẽ trả lời sai`NO`. 

Cuối cùng, cả hai điểm cuối của một khoảng đều thuộc về truy vấn. Với```
2 2 1
a b
a 30
b 31
2 1 2
```tổng số là 61, trong khi ngưỡng là 60, vì vậy câu trả lời là`YES`. Vô tình truy vấn khoảng thời gian nửa mở chẳng hạn như [L,R) sẽ bỏ lỡ đồ uống ở vị trí R. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là chuyển đổi mọi tên đồ uống thành giá trị rượu của nó và lưu trữ các giá trị đó trong một mảng. Đối với truy vấn loại 1, chúng tôi thay thế một phần tử mảng. Đối với truy vấn loại 2, chúng tôi lặp từ L đến R, cộng tất cả các giá trị và so sánh kết quả với 30(R−L+1). Điều này đúng vì truy vấn yêu cầu chính xác tổng các giá trị hiện tại trong khoảng đó. 

Vấn đề là chi phí của truy vấn phạm vi. Một truy vấn có thể kiểm tra N phần tử và với truy vấn Q, trường hợp xấu nhất là O(NQ). Khi N=Q=200000, tức là có tới 40.000.000.000 lượt truy cập mảng, trước cả khi xem xét các hoạt động khác. Giới hạn 2 giây quy định cách tiếp cận này. 

Quan sát hữu ích là mọi truy vấn loại 2 chỉ cần một tổng phạm vi, trong khi mỗi truy vấn loại 1 thay đổi chính xác một giá trị. Đây chính xác là mẫu hoạt động được xử lý bởi cây Fenwick. Cây Fenwick lưu trữ một phần tổng sao cho một điểm thay đổi và một tổng tiền tố đều mất thời gian O(logN). Khi có sẵn tổng tiền tố, tổng của [L,R] được lấy dưới dạng`prefix(R) - prefix(L-1)`. 

Cách rút gọn chính là viết lại điều kiện nôn nao dưới dạng so sánh giữa hai đại lượng cộng. Thay vì suy nghĩ riêng về số phút, chúng ta chỉ có thể lưu trữ các giá trị rượu trong cấu trúc dữ liệu và tính ngưỡng trực tiếp là 30(R−L+1). Không cần thông tin nào khác về đồ uống sau khi tên của chúng đã được chuyển đổi thành giá trị. 

Lực lượng vũ phu hoạt động vì tính tổng trực tiếp khoảng cho ra chính xác số lượng cần thiết, nhưng không thành công khi truy vấn nhiều khoảng thời gian dài. Quan sát chỉ cần thay đổi điểm và tổng phạm vi cho phép chúng tôi thay thế việc quét lặp lại bằng cây Fenwick. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NQ) trường hợp xấu nhất | O(N+M) | Quá chậm | 
| Tối ưu | O((N+M+Q)logN) | O(N+M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tên đồ uống ban đầu và giá trị đồ uống M. Lưu trữ ánh xạ từ mỗi tên đến lượng rượu của nó vì các truy vấn đề cập đến đồ uống theo tên. 
2. Chuyển đổi mỗi đồ uống trong lịch trình ban đầu thành lượng cồn bằng số. Việc giữ các giá trị số trong mảng sẽ tránh việc tra cứu từ điển lặp lại trong khi xử lý các truy vấn. 
3. Xây dựng cây Fenwick trên mảng kết quả. Cây biểu thị các tổng tiền tố, cho phép chúng ta thu được bất kỳ tổng khoảng nào bằng cách trừ hai tổng tiền tố. 
4. Đối với truy vấn loại 1`1 X Y`, tra giá trị rượu của đồ uống`Y`. Đặt giá trị cũ ở vị trí X là`old`và giá trị mới là`new`. Áp dụng sự khác biệt`new - old`vào cây Fenwick ở vị trí X, sau đó thay thế giá trị mảng đã lưu bằng`new`. 

Cập nhật theo chênh lệch là đủ vì mọi nút Fenwick chứa vị trí X chỉ cần tổng được lưu trữ của nó thay đổi chính xác bằng số tiền đó. 
5. Đối với truy vấn loại 2`2 L R`, tính toán 

S=tổng(L,R) 

sử dụng cây Fenwick. Khoảng thời gian chứa R−L+1 giờ, vì vậy giới hạn nồng độ cồn an toàn của nó là 

30(R−L+1). 

In`YES`nếu S>30(R−L+1), và`NO`nếu không thì. 
6. Xử lý tất cả các truy vấn theo thứ tự ban đầu. Một bản cập nhật sẽ thay đổi lịch trình hiện tại, vì vậy mọi truy vấn phạm vi sau này đều phải sử dụng mảng và cây Fenwick đã sửa đổi. 

Điều bất biến là sau mỗi thao tác được xử lý, cây Fenwick chứa tổng chính xác của các giá trị rượu hiện tại ở mọi tiền tố và mảng riêng biệt chứa giá trị hiện tại ở mọi vị trí. Một bản cập nhật điểm thay đổi cả hai cách biểu diễn với cùng một lượng, do đó, bất biến vẫn tồn tại trong các bản cập nhật. Vì mỗi tổng phạm vi được lấy từ hai tổng tiền tố chính xác nên mọi truy vấn loại 2 sẽ so sánh tổng lượng cồn chính xác hiện tại với giới hạn chính xác 30 lít mỗi giờ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, delta):
        while i <= self.n:
            self.bit[i] += delta
            i += i & -i

    def prefix_sum(self, i):
        result = 0
        while i > 0:
            result += self.bit[i]
            i -= i & -i
        return result

    def range_sum(self, l, r):
        return self.prefix_sum(r) - self.prefix_sum(l - 1)

def solve():
    n, m, q = map(int, input().split())

    drinks = input().split()

    value = {}
    for _ in range(m):
        name, v = input().split()
        value[name] = int(v)

    arr = [value[name] for name in drinks]

    fw = Fenwick(n)

    for i, v in enumerate(arr, 1):
        fw.add(i, v)

    out = []

    for _ in range(q):
        query = input().split()
        t = int(query[0])

        if t == 1:
            x = int(query[1])
            y = query[2]

            new_value = value[y]
            old_value = arr[x - 1]

            fw.add(x, new_value - old_value)
            arr[x - 1] = new_value

        else:
            l = int(query[1])
            r = int(query[2])

            total = fw.range_sum(l, r)
            limit = 30 * (r - l + 1)

            if total > limit:
                out.append("YES")
            else:
                out.append("NO")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Từ điển`value`thực hiện chuyển đổi tên thành rượu trong thời gian dự kiến ​​​​O(1). Lịch trình ban đầu được chuyển đổi một lần trước khi bắt đầu bất kỳ hoạt động động nào. 

các`arr`mảng lưu trữ giá trị số hiện tại ở mọi vị trí. Điều này là cần thiết trong quá trình cập nhật vì cây Fenwick chỉ lưu trữ tổng hợp, vì vậy chúng ta cần giá trị cũ để tính toán chênh lệch cần được nhân rộng. 

Cây Fenwick được khởi tạo bằng cách cộng mọi giá trị mảng. Việc này cần O(NlogN) với cách triển khai đơn giản ở trên. Có thể xây dựng O(N), nhưng ở đây không cần thiết vì N 2⋅10 5. 

Để cập nhật tại vị trí`x`, biểu thức`new_value - old_value`có thể dương, âm hoặc bằng không. Chênh lệch bằng 0 là vô hại và chính xác là không thay đổi tất cả các tổng liên quan. 

Truy vấn phạm vi sử dụng`prefix_sum(r) - prefix_sum(l - 1)`. các`l - 1`là cái làm cho khoảng bao gồm cả hai vế. Vì các vị trí đầu vào được đánh chỉ mục 1 nên cây Fenwick cũng được đánh chỉ mục 1 một cách có chủ ý. 

Số nguyên Python không tràn và tổng số lớn nhất có thể chỉ là 100⋅200000=20.000.000. Đầu ra được tích lũy trong một danh sách và được ghi một lần, điều này tránh được việc gọi quá nhiều đến`print`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Giá trị đồ uống là`vodka = 30`,`pitu = 35`,`beats = 15`,`whisky = 20`, Và`cuba = 50`. Do đó, mảng số ban đầu là`[30, 35, 15, 20, 30, 50]`. 

| Hoạt động | Mảng liên quan hiện tại | Tổng phạm vi | Giới hạn | Đầu ra | 
| --- | --- | --- | --- | --- | 
|`2 3 4`|`[30,35,15,20,30,50]`| 15+20=35 | 30⋅2=60 |`NO`| 
|`1 3 cuba`|`[30,35,50,20,30,50]`| | | | 
|`2 3 3`|`[30,35,50,20,30,50]`| 50 | 30 |`YES`| 
|`1 5 cuba`|`[30,35,50,20,50,50]`| | | | 
|`2 1 5`|`[30,35,50,20,50,50]`| 180 | 150 |`YES`| 

Truy vấn đầu tiên là an toàn vì 35 nằm dưới giới hạn 60 lít. Sau khi vị trí 3 thay đổi từ`beats`ĐẾN`cuba`, giá trị của nó trở thành 50, khiến truy vấn kéo dài một giờ vượt quá giới hạn 30 lít. Bản cập nhật thứ hai thay đổi vị trí 5 từ 30 lên 50, nâng năm vị trí đầu tiên lên 180, vượt quá 150. 

### Ví dụ được xây dựng 

Hãy xem xét trường hợp nhỏ này:```
4 2 5
a a b a
a 30
b 60
2 1 4
1 2 b
2 1 2
1 4 b
2 3 4
```Mảng ban đầu là`[30,30,60,30]`. 

| Hoạt động | Mảng sau thao tác | Tổng phạm vi | Giới hạn | Đầu ra | 
| --- | --- | --- | --- | --- | 
|`2 1 4`|`[30,30,60,30]`| 150 | 120 |`YES`| 
|`1 2 b`|`[30,60,60,30]`| | | | 
|`2 1 2`|`[30,60,60,30]`| 90 | 60 |`YES`| 
|`1 4 b`|`[30,60,60,60]`| | | | 
|`2 3 4`|`[30,60,60,60]`| 120 | 60 |`YES`| 

Dấu vết này chứng tỏ rằng các cập nhật được phản ánh ngay lập tức trong các tổng phạm vi tiếp theo. Nó cũng kiểm tra việc xử lý điểm cuối toàn diện trong truy vấn cuối cùng, trong đó cả vị trí 3 và 4 đều đóng góp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N+M+Q)logN) | Ánh xạ đồ uống mất O(M), quá trình khởi tạo Fenwick mất O(NlogN) và mọi truy vấn cập nhật hoặc phạm vi đều mất O(logN). | 
| Không gian | O(N+M) | Từ điển đồ uống sử dụng O(M), trong khi mảng hiện tại và cây Fenwick sử dụng O(N). | 

Với N,M,Q<2⋅10 5, giải pháp chỉ thực hiện vài triệu phép tính logarit Fenwick thay vì hàng chục tỷ lần quét theo khoảng thời gian. Việc sử dụng bộ nhớ thoải mái dưới 256 MB. 

## Trường hợp thử nghiệm 

Bộ khai thác thử nghiệm sau đây sử dụng cùng một`solve()`thực hiện và tạm thời thay thế đầu vào và đầu ra tiêu chuẩn. Trường hợp kích thước tối đa sử dụng 200000 vị trí và 200000 truy vấn, đủ để thực hiện hành vi tiệm cận mà không cần nhúng một chuỗi ký tự khổng lồ vào bài xã luận.```python
import sys
import io

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, delta):
        while i <= self.n:
            self.bit[i] += delta
            i += i & -i

    def prefix_sum(self, i):
        result = 0
        while i > 0:
            result += self.bit[i]
            i -= i & -i
        return result

    def range_sum(self, l, r):
        return self.prefix_sum(r) - self.prefix_sum(l - 1)

def solve():
    input = sys.stdin.readline

    n, m, q = map(int, input().split())
    drinks = input().split()

    value = {}
    for _ in range(m):
        name, v = input().split()
        value[name] = int(v)

    arr = [value[name] for name in drinks]

    fw = Fenwick(n)
    for i, v in enumerate(arr, 1):
        fw.add(i, v)

    out = []

    for _ in range(q):
        query = input().split()
        if query[0] == "1":
            x = int(query[1])
            y = query[2]

            new_value = value[y]
            old_value = arr[x - 1]

            fw.add(x, new_value - old_value)
            arr[x - 1] = new_value
        else:
            l = int(query[1])
            r = int(query[2])

            total = fw.range_sum(l, r)
            limit = 30 * (r - l + 1)

            out.append("YES" if total > limit else "NO")

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

sample1 = """\
6 6 5
vodka pitu beats whisky vodka cuba
vodka 30
caipirinha 10
pitu 35
beats 15
whisky 20
cuba 50
2 3 4
1 3 cuba
2 3 3
1 5 cuba
2 1 5
"""

assert run(sample1) == "NO\nYES\nYES", "sample 1"

minimum = """\
1 1 2
a
a 30
2 1 1
1 1 a
"""

assert run(minimum) == "NO", "minimum-size equality case"

boundary = """\
2 2 4
a b
a 30
b 31
2 1 2
2 2 2
1 1 b
2 1 1
"""

assert run(boundary) == "YES\nYES\nYES", "inclusive boundaries and update"

all_equal = """\
5 1 4
a a a a a
a 31
2 1 5
1 3 a
2 3 3
2 2 4
"""

assert run(all_equal) == "YES\nYES\nYES", "all-equal values"

n = 200000
q = 200000
max_input = (
    f"{n} 1 {q}\n"
    + " ".join(["a"] * n)
    + "\n"
    + "a 1\n"
    + "\n".join(["2 1 1"] * q)
    + "\n"
)

assert run(max_input) == ("NO\n" * q).rstrip("\n"), "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 |`NO`,`YES`,`YES`| Chuỗi truy vấn và cập nhật chính thức | 
|`minimum`|`NO`| N=1 và đẳng thức chính xác với giới hạn an toàn | 
|`boundary`|`YES`,`YES`,`YES`| Điểm cuối bao gồm, phạm vi một phần tử và cập nhật điểm | 
|`all_equal`|`YES`,`YES`,`YES`| Lặp lại các giá trị và truy vấn giống hệt nhau trong các khoảng thời gian khác nhau | 
|`max_input`|`NO`lặp đi lặp lại 200000 lần | N và Q lớn, nhấn mạnh việc thực hiện logarit | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là khoảng thời gian một giờ. Vì```
1 1 1
a
a 31
2 1 1
```cây Fenwick trả về 31 cho`[1,1]`. Thuật toán tính giới hạn là 30(1−1+1)=30, sau đó kiểm tra 31>30, tạo ra`YES`. Phép tính sử dụng số giờ chứ không phải số phút trực tiếp vì việc chuyển đổi sang phút đã được tích hợp vào hệ số 30. 

Ranh giới bình đẳng hoạt động khác nhau:```
1 1 1
a
a 30
2 1 1
```Tổng phạm vi là 30 và giới hạn cũng là 30. Việc so sánh hoàn toàn nghiêm ngặt`total > limit`, vậy kết quả là`NO`. Điều này phù hợp với quy tắc rằng số tiền không vượt quá giới hạn là an toàn. 

Một bản cập nhật có thể thay thế một giá trị bằng một giá trị lớn hơn nhiều:```
2 2 1
a a
a 30
b 100
1 1 b
2 1 1
```Sau khi cập nhật,`arr[0]`trở thành 100 và cây Fenwick nhận được chênh lệch 100−30=70 ở vị trí 1. Truy vấn tiếp theo trả về 100, so sánh nó với 30 và in ra`YES`. Giá trị cũ không bao giờ còn lại trong bất kỳ tiền tố Fenwick nào. 

Điểm cuối bao gồm được kiểm tra bởi```
2 2 1
a b
a 30
b 31
2 1 2
```Phạm vi`[1,2]`chứa cả hai giá trị, cho kết quả là 61. Ngưỡng là 30⋅2=60, do đó kết quả là`YES`. Biểu thức Fenwick`prefix(2) - prefix(0)`bao gồm cả hai vị trí, đó chính xác là những gì truy vấn yêu cầu. 

Trường hợp tinh tế cuối cùng là một khoảng dài có tổng chính xác là giới hạn. Ví dụ,```
3 1 1
a a a
a 30
2 1 3
```cho tổng số 90 và giới hạn là 30⋅3=90. Thuật toán in`NO`, xác nhận rằng bất đẳng thức nghiêm ngặt được áp dụng độc lập với độ dài khoảng.
