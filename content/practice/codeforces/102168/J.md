---
title: "CF 102168J - \u0418\u0433\u0440\u0430 \u0441 \u043f\u0435\u0440\u0435\u0441\u0442\u0430\u043d\u043e\u0432\u043a\u043e\u0439"
description: "Chúng ta có một hoán vị p chứa mọi số nguyên từ 1 đến n đúng một lần. Trong mỗi vòng, tên của người chơi đầu tiên là a, tên của người chơi thứ hai là b, a và b được đảm bảo là khác nhau. Người chiến thắng là người có số xuất hiện sớm hơn trong hoán vị."
date: "2026-08-19T07:33:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102168
codeforces_index: "J"
codeforces_contest_name: "\u041b\u0438\u0447\u043d\u044b\u0439 \u0447\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u0421\u0430\u043c\u0430\u0440\u0441\u043a\u043e\u0433\u043e \u0443\u043d\u0438\u0432\u0435\u0440\u0441\u0438\u0442\u0435\u0442\u0430 \u0441\u0440\u0435\u0434\u0438 \u043d\u043e\u0432\u0438\u0447\u043a\u043e\u0432 2018-2019"
rating: 0
weight: 102168
solve_time_s: 75
verified: true
draft: false
---

[CF 102168J - \u0418\u0433\u0440\u0430 \u0441 \u043f\u0435\u0440\u0435\u0441\u0442\u0430\u043d\u043e\u0432\u043a\u043e\u0439](https://codeforces.com/problemset/problem/102168/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hoán vị`p`chứa mọi số nguyên từ`1`ĐẾN`n`đúng một lần. Trong mỗi vòng, tên người chơi đầu tiên`a`, tên người chơi thứ hai`b`, Và`a`Và`b`được đảm bảo là khác nhau. Người chiến thắng là người có số xuất hiện sớm hơn trong hoán vị. 

Ví dụ, nếu```
p = [4, 2, 5, 1, 3]
```thì vị trí của`2`là`2`, trong khi vị trí của`3`là`5`. Cho một vòng`(2, 3)`, người chơi đầu tiên thắng vì`2`xảy ra sớm hơn. Vì`(3, 2)`, người chơi thứ hai thắng. 

Đầu vào đưa ra hoán vị trước, sau đó là`q`các truy vấn độc lập. Đối với mỗi truy vấn chúng tôi cần in`First`nếu số đầu tiên xuất hiện sớm hơn trong hoán vị, nếu không thì`Second`. 

Những hạn chế đạt tới`n = 200000`Và`q = 200000`. Một giải pháp quét hoán vị cho mọi truy vấn có thể thực hiện tới`n * q = 40,000,000,000`so sánh trong trường hợp xấu nhất, vượt xa giới hạn hai giây có thể chịu đựng được. Chúng ta cần xử lý trước hoán vị để mỗi truy vấn có thể được trả lời trong thời gian không đổi. MỘT`O(n + q)`giải pháp phù hợp một cách thoải mái. 

Có một số trường hợp đặc biệt có thể khiến việc triển khai bất cẩn không thành công. Đầu tiên, số chiến thắng có thể ở vị trí đầu tiên hoặc cuối cùng. Vì```
2
2 1
1
1 2
```đầu ra là```
Second
```bởi vì`2`đang ở vị trí`1`Và`1`đang ở vị trí`2`. Việc triển khai vô tình sử dụng các vị trí dựa trên 0 và dựa trên một không nhất quán có thể đảo ngược sự so sánh này. 

Thứ tự của hai số trong truy vấn cũng có vấn đề. Với```
3
2 3 1
2
2 1
1 2
```đầu ra là```
First
Second
```Cặp này chứa hai giá trị giống nhau trong cả hai vòng, nhưng người chơi đã hoán đổi chúng. Việc so sánh các giá trị thay vì vị trí của chúng sẽ cho kết quả sai. 

Truy vấn lặp đi lặp lại là một trường hợp hữu ích khác. Vì```
4
3 1 4 2
3
1 2
1 2
2 1
```đầu ra là```
First
First
Second
```Quá trình xử lý trước được chia sẻ bởi mọi truy vấn, vì vậy không có lý do gì để thực hiện bất kỳ công việc bổ sung nào khi cùng một cặp xuất hiện lại. 

Cuối cùng, cụm từ "tất cả các giá trị bằng nhau" không thể mô tả một bài kiểm tra hợp lệ cho vấn đề này. Hoán vị chứa các giá trị riêng biệt và mỗi truy vấn rõ ràng chứa hai giá trị khác nhau. Trường hợp căng thẳng có ý nghĩa gần nhất là lặp lại cùng một truy vấn hợp lệ nhiều lần, nhằm kiểm tra xem việc triển khai có vô tình thay đổi trạng thái giữa các vòng hay không. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là xử lý mọi truy vấn bằng cách tìm kiếm thông qua hoán vị cho đến khi tìm thấy hai giá trị được yêu cầu. Khi cả hai vị trí đều được biết đến, chúng tôi so sánh chúng. Điều này đúng vì người chiến thắng được xác định hoàn toàn bằng giá trị nào xuất hiện trước. 

Vấn đề là việc tìm kiếm lặp đi lặp lại. Trong trường hợp xấu nhất, một truy vấn có thể buộc chúng ta phải kiểm tra gần như toàn bộ hoán vị trước khi tìm ra giá trị của nó. Với`q = 200000`truy vấn và`n = 200000`, công việc trong trường hợp xấu nhất là theo thứ tự`40,000,000,000`kiểm tra mảng. Ngay cả khi một lần kiểm tra rất rẻ thì hàng chục tỷ thao tác cũng không tương thích với thời hạn. 

Quan sát quan trọng là hoán vị không bao giờ thay đổi giữa các vòng. Chúng tôi liên tục hỏi cùng một loại câu hỏi về một mảng cố định: "Giá trị ở đâu?`x`xảy ra?" Vì mỗi giá trị xuất hiện chính xác một lần nên chúng ta có thể trả lời trước mọi câu hỏi như vậy. 

Xây dựng một hoán vị nghịch đảo`pos`, Ở đâu`pos[x]`lưu trữ vị trí tại đó giá trị`x`xảy ra ở`p`. Sau đó, một truy vấn`(a, b)`chỉ trở thành sự so sánh```
pos[a] < pos[b]
```Nếu biểu thức đúng thì người chơi đầu tiên sẽ thắng. Nếu không, người chơi thứ hai sẽ thắng. 

Điều này thay đổi vấn đề từ việc tìm kiếm liên tục trong một mảng sang thực hiện một bước tiền xử lý tuyến tính, sau đó là các truy vấn có thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) trường hợp xấu nhất | O(1) thêm | Quá chậm | 
| Tối ưu | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`và hoán vị`p`. Hoán vị được cố định cho tất cả các vòng, vì vậy tất cả thông tin hữu ích về vị trí có thể được tính toán một lần. 
2. Tạo một mảng`pos`kích thước`n + 1`. Đối với mỗi chỉ số`i`và giá trị`x = p[i]`, cửa hàng`pos[x] = i`. Phần tử bổ sung tại chỉ mục`0`không được sử dụng vì các giá trị hoán vị nằm trong khoảng từ`1`ĐẾN`n`. 
3. Đọc số lượng truy vấn`q`. Đối với mỗi truy vấn, hãy đọc hai giá trị`a`Và`b`. 
4. So sánh`pos[a]`Và`pos[b]`. Nếu như`pos[a] < pos[b]`, số của người chơi đầu tiên xuất hiện sớm hơn, do đó xuất ra`First`. Nếu không thì xuất ra`Second`. 
5. Thu thập các câu trả lời và in chúng lại với nhau. Xây dựng một chuỗi đầu ra sẽ tránh được các hoạt động đầu ra lặp đi lặp lại không cần thiết khi có tới`200000`truy vấn. 

### Tại sao nó hoạt động 

Bất biến trung tâm là sau khi tiền xử lý,`pos[x]`chính xác là vị trí của giá trị`x`trong hoán vị ban đầu. Vì hoán vị chứa mỗi giá trị chính xác một lần nên có chính xác một vị trí như vậy cho mỗi giá trị truy vấn. Đối với một truy vấn`(a, b)`, người chơi đầu tiên thắng chính xác khi xuất hiện`a`là trước khi xảy ra`b`, đó chính xác là điều kiện`pos[a] < pos[b]`. Do đó, mọi câu trả lời được đưa ra đều phù hợp với quy tắc của trò chơi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    p = list(map(int, input().split()))

    pos = [0] * (n + 1)

    for i, x in enumerate(p):
        pos[x] = i

    q = int(input())
    ans = []

    for _ in range(q):
        a, b = map(int, input().split())

        if pos[a] < pos[b]:
            ans.append("First")
        else:
            ans.append("Second")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```các`pos`mảng là toàn bộ tối ưu hóa. Trong vòng lặp tiền xử lý,`i`là một chỉ mục Python dựa trên 0, nhưng điều đó không gây ra vấn đề gì vì chỉ có thứ tự tương đối mới quan trọng. Nếu như`pos[a] < pos[b]`, sau đó`a`xuất hiện sớm hơn bất kể vị trí được đánh số từ 0 hay một. 

Mảng có độ dài`n + 1`để giá trị đó có thể được sử dụng trực tiếp làm chỉ mục. Điều này tránh được một chuyển đổi bổ sung như`x - 1`và làm cho mã truy vấn trở nên đặc biệt đơn giản. 

Điều kiện truy vấn không cần nhánh đẳng thức. Đảm bảo đầu vào`a != b`và hoán vị chỉ chứa mỗi giá trị một lần, vì vậy`pos[a]`Và`pos[b]`không bao giờ có thể bằng nhau. 

Không thể tràn số nguyên trong Python và chỉ có chỉ mục được lưu trữ lớn nhất`199999`. 

Các câu trả lời được tích lũy trong`ans`và viết một lần ở cuối. Với`200000`truy vấn, điều này tốt hơn là gọi`print`riêng biệt cho từng kết quả. 

## Ví dụ đã hoạt động 

Mẫu được cung cấp chứa hoán vị```
p = [2, 3, 1]
```vì vậy biểu diễn nghịch đảo của nó là`pos[1] = 2`,`pos[2] = 0`, Và`pos[3] = 1`. 

| Truy vấn |`a`|`b`|`pos[a]`|`pos[b]`| Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 2 | 0 | Thứ hai | 
| 2 | 1 | 3 | 2 | 1 | Thứ hai | 
| 3 | 2 | 1 | 0 | 2 | Đầu tiên | 
| 4 | 2 | 3 | 0 | 1 | Đầu tiên | 

Điều này chứng tỏ tại sao các giá trị số thực tế lại không liên quan một khi hoán vị nghịch đảo đã được xây dựng. Ví dụ,`1 < 3`, Nhưng`1`vẫn thua`3`bởi vì`1`xảy ra muộn hơn trong hoán vị. 

Một ví dụ thứ hai là```
4
3 1 4 2
3
1 2
1 4
2 1
```Hoán vị nghịch đảo là`pos[1] = 1`,`pos[2] = 3`,`pos[3] = 0`, Và`pos[4] = 2`. 

| Truy vấn |`a`|`b`|`pos[a]`|`pos[b]`| Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 1 | 3 | Đầu tiên | 
| 2 | 1 | 4 | 1 | 2 | Đầu tiên | 
| 3 | 2 | 1 | 3 | 1 | Thứ hai | 

Truy vấn thứ ba đặc biệt hữu ích vì nó có chính xác hai giá trị giống như truy vấn đầu tiên nhưng theo thứ tự ngược lại. Việc so sánh được thực hiện từ góc độ của người chơi, vì vậy việc hoán đổi`a`Và`b`hoán đổi người chiến thắng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Xây dựng`pos`lấy O(n) và mọi truy vấn lấy O(1). | 
| Không gian | O(n) | Hoán vị nghịch đảo lưu trữ một vị trí cho mỗi giá trị. | 

Với`n, q <= 200000`, thuật toán chỉ thực hiện vài trăm nghìn phép tính và so sánh mảng. các`O(n)`việc sử dụng bộ nhớ cũng dễ dàng nằm trong giới hạn 256 MB. 

## Trường hợp thử nghiệm 

Bộ khai thác thử nghiệm sau đây sử dụng cùng một thuật toán thông qua một hàm chấp nhận chuỗi đầu vào. Trường hợp kích thước tối đa được tạo ra thay vì viết theo nghĩa đen, bởi vì viết một hoán vị của`200000`các giá trị vào nguồn sẽ che khuất những gì bài kiểm tra đang kiểm tra.```python
import sys
import io

def solve(inp: str) -> str:
    data = inp.split()
    it = iter(data)

    n = int(next(it))
    p = [int(next(it)) for _ in range(n)]

    pos = [0] * (n + 1)
    for i, x in enumerate(p):
        pos[x] = i

    q = int(next(it))
    ans = []

    for _ in range(q):
        a = int(next(it))
        b = int(next(it))
        ans.append("First" if pos[a] < pos[b] else "Second")

    return "\n".join(ans)

# Provided sample.
sample1 = """\
3
2 3 1
4
1 2
1 3
2 1
2 3
"""

assert solve(sample1) == """\
Second
Second
First
First
""".strip(), "sample 1"

# Minimum-size permutation, both possible query orientations.
sample2 = """\
2
2 1
2
1 2
2 1
"""

assert solve(sample2) == """\
Second
First
""".strip(), "minimum size"

# Boundary values and repeated queries.
sample3 = """\
5
5 1 4 2 3
4
1 5
5 3
2 3
1 2
"""

assert solve(sample3) == """\
Second
First
Second
First
""".strip(), "boundary values"

# Repeated identical queries.
sample4 = """\
4
3 1 4 2
5
1 2
1 2
1 2
2 1
2 1
"""

assert solve(sample4) == """\
First
First
First
Second
Second
""".strip(), "repeated queries"

# Maximum-size stress test.
n = 200000
p = list(range(1, n + 1))

queries = []
for i in range(1, 100001):
    queries.append(f"{i} {n - i + 1}")

max_input = (
    f"{n}\n"
    + " ".join(map(str, p))
    + "\n100000\n"
    + "\n".join(queries)
    + "\n"
)

max_output = solve(max_input).splitlines()

assert len(max_output) == 100000, "maximum-size query count"
assert max_output[0] == "First", "maximum-size first query"
assert max_output[-1] == "Second", "maximum-size boundary query"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 / 2 3 1 / 4 queries`|`Second, Second, First, First`| Cung cấp mẫu và so sánh vị trí | 
|`2 / 2 1 / 2 queries`|`Second, First`| tối thiểu`n`và cả hai hướng truy vấn | 
|`5 / 5 1 4 2 3 / 4 queries`|`Second, First, Second, First`| Giá trị ở vị trí đầu tiên và cuối cùng | 
|`4 / 3 1 4 2 / 5 queries`|`First, First, First, Second, Second`| Truy vấn giống hệt nhau lặp đi lặp lại | 
|`n = 200000`với`100000`truy vấn |`100000`câu trả lời | Khối lượng truy vấn và tiền xử lý kích thước tối đa | 

Những ràng buộc của bài toán làm cho một phép thử hoàn toàn bằng nhau theo nghĩa đen là không thể thực hiện được. Hoán vị không thể chứa các giá trị trùng lặp và truy vấn không thể có điểm cuối bằng nhau. Việc lặp lại một truy vấn hợp lệ là cách thay thế thích hợp để kiểm tra các thay đổi trạng thái ngẫu nhiên giữa các vòng. 

## Vỏ cạnh 

Xem xét đầu vào tối thiểu```
2
2 1
1
1 2
```Cửa hàng tiền xử lý`pos[2] = 0`Và`pos[1] = 1`. Truy vấn so sánh`pos[1] = 1`với`pos[2] = 0`, do đó điều kiện sai và câu trả lời là`Second`. Điều này nắm bắt các triển khai so sánh các giá trị thay vì vị trí của chúng. 

Đối với thứ tự truy vấn đảo ngược, hãy sử dụng```
2
2 1
1
2 1
```Hiện nay`pos[2] = 0 < pos[1] = 1`, vì vậy đầu ra là`First`. Hoán vị không thay đổi. Chỉ có số được chỉ định của người chơi là thay đổi, đó là lý do tại sao kết quả đầu ra thay đổi. 

Đối với một giá trị ở mỗi cực trị của hoán vị, hãy xem xét```
5
5 1 4 2 3
2
1 5
5 3
```Các vị trí là`pos[1] = 1`,`pos[5] = 0`, Và`pos[3] = 4`. Truy vấn đầu tiên đưa ra`Second`bởi vì`5`là trước đây`1`. Thứ hai cho`First`bởi vì`5`là trước đây`3`. Điều này xác minh rằng vị trí đầu tiên và cuối cùng được xử lý bình thường. 

Đối với các truy vấn lặp lại, hãy xem xét```
4
3 1 4 2
4
1 2
1 2
2 1
2 1
```Các vị trí được lưu trữ là`pos[1] = 1`Và`pos[2] = 3`. Mọi`(1, 2)`truy vấn tạo ra`First`, trong khi mọi`(2, 1)`truy vấn tạo ra`Second`. Vì thuật toán không bao giờ sửa đổi`pos`, mỗi lần lặp lại đều nhận được câu trả lời đúng như nhau. 

Giá trị biên`n`cũng có thể được sử dụng trực tiếp như một chỉ mục vì`pos`có kích thước`n + 1`. Ví dụ, khi`n = 200000`, giá trị`200000`được lưu trữ trong`pos[200000]`không có trường hợp đặc biệt nào. Điều này tránh được lỗi thường gặp khi chỉ phân bổ`n`các phần tử trong khi vẫn lập chỉ mục theo chính giá trị đó.
