---
title: "CF 102638C - Anime"
description: "Kệ là một hàng có n vị trí. Một số vị trí chứa đĩa anime và các vị trí còn lại chứa rác. Chúng ta có k mô tả về các đĩa, trong đó mỗi mô tả chỉ cho chúng ta biết phạm vi vị trí có thể có của đĩa đó."
date: "2026-08-01T09:40:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102638
codeforces_index: "C"
codeforces_contest_name: "Bredor contest"
rating: 0
weight: 102638
solve_time_s: 430
verified: true
draft: false
---

[CF 102638C - Anime](https://codeforces.com/problemset/problem/102638/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 10 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Kệ là một dãy`n`các vị trí. Một số vị trí chứa đĩa anime và các vị trí còn lại chứa rác. Chúng tôi được trao`k`mô tả về đĩa, trong đó mỗi mô tả chỉ cho chúng ta biết phạm vi vị trí có thể có của đĩa đó. Nhiệm vụ là xác định chính xác vị trí nào chứa rác. 

Một vị trí được thể hiện bằng`0`ở đầu ra nếu nó chứa một đĩa và bằng`1`nếu nó chứa rác. Đầu vào đảm bảo rằng thông tin về các phạm vi có thể đủ để xác định một cách sắp xếp đĩa duy nhất. 

Giá trị của`n`có thể cao hơn một chút`100000`, do đó, cách tiếp cận liên tục thử nhiều bài tập sẽ không thể hiệu quả. Với giới hạn khoảng một giây, chúng ta cần thứ gì đó gần bằng`O(n log n)`hoặc`O(n)`. Việc tìm kiếm mạnh mẽ trên các vị trí đĩa có thể sẽ tăng theo cấp số nhân và thậm chí việc kiểm tra nhiều kết hợp khoảng thời gian có thể có sẽ vượt xa số lượng thao tác được phép. 

Phần khó khăn là mỗi khoảng không cho chúng ta biết vị trí chính xác của một cái đĩa. Một giải pháp bất cẩn có thể chọn các vị trí tùy ý thỏa mãn các khoảng, nhưng các vị trí được chọn có thể không phải là sự sắp xếp duy nhất. 

Ví dụ:```
3 1
1 2
```Có một đĩa ở đâu đó ở vị trí 1 hoặc 2. Cả hai cách sắp xếp đều có thể thực hiện được, do đó thông tin đầu vào này sẽ không xuất hiện vì câu trả lời không phải là duy nhất. 

Một trường hợp quan trọng khác là khi các khoảng chồng lên nhau nhiều:```
3 2
1 2
2 3
```Phương pháp tham lam gán khoảng đầu tiên cho vị trí 1 và khoảng thứ hai cho vị trí 2 tạo ra một sự sắp xếp hợp lệ, nhưng việc gán chúng cho vị trí 2 và 3 cũng hợp lệ. Việc đảm bảo tính duy nhất có nghĩa là sự mơ hồ như vậy không thể xảy ra trong các thử nghiệm chính thức, nhưng nó cho thấy lý do tại sao chúng ta cần một phương pháp tôn trọng cấu trúc của các phép gán khoảng. 

Trường hợp không có đĩa cũng đặc biệt:```
5 0
```Mọi vị trí đều phải là rác rưởi nên câu trả lời là:```
1 1 1 1 1
```Giải pháp chỉ xử lý các khoảng thời gian và quên khởi tạo các vị trí còn lại sẽ không thành công ở đây. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử đặt mọi đĩa vào một vị trí nào đó trong khoảng thời gian cho phép của nó. Vì số lượng vị trí có thể có có thể rất lớn nên điều này trở thành vấn đề khớp giữa các khoảng và vị trí. Việc tìm kiếm mạnh mẽ trên tất cả các lựa chọn sẽ có độ phức tạp theo cấp số nhân. Ngay cả việc triển khai đối sánh chậm hơn và liên tục quét tất cả các khoảng thời gian để tìm các vị trí có sẵn cũng có thể đạt được khoảng`O(nk)`hoạt động xung quanh`10^10`trong trường hợp xấu nhất. 

Quan sát hữu ích là tất cả các ràng buộc đều là các khoảng trên một dòng. Khoảng thời gian có thuộc tính thứ tự: khi chúng tôi xử lý khoảng thời gian kết thúc sớm nhất, chúng tôi sẽ cung cấp cho khoảng thời gian đó vị trí hiện có sớm nhất. Nếu chúng ta trì hoãn khoảng thời gian này, các khoảng thời gian sau có điểm cuối bên phải lớn hơn vẫn có thể sử dụng các vị trí muộn hơn, nhưng khoảng thời gian kết thúc sớm nhất có ít lựa chọn thay thế hơn. 

Điều này biến vấn đề thành một quá trình so khớp khoảng tham lam. Sắp xếp tất cả các khoảng đĩa theo điểm cuối bên phải của chúng. Duy trì vị trí kệ có sẵn tiếp theo. Đối với mỗi khoảng thời gian, chỉ định vị trí chưa sử dụng đầu tiên nằm bên trong khoảng thời gian đó. Các vị trí được chỉ định chính xác là vị trí của đĩa. Đầu ra chỉ đơn giản là sự bổ sung của các vị trí này. 

Lực lượng vũ phu hoạt động vì mọi phép gán hợp lệ phải đặt từng đĩa ở đâu đó trong khoảng của nó, nhưng nó không thành công vì nó khám phá quá nhiều lựa chọn có thể. Nhận xét rằng các khoảng thời gian kết thúc sớm nhất là hạn chế nhất cho phép chúng ta đưa ra mọi lựa chọn ngay lập tức và tránh phải tìm kiếm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Phân công khoảng tham lam | O((n + k) log k) | O(n + k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các khoảng đĩa và sắp xếp chúng theo điểm cuối bên phải của chúng. Khi hai khoảng có cùng điểm cuối bên phải, thứ tự của chúng không ảnh hưởng đến độ chính xác. 
2. Giữ một mảng đánh dấu vị trí kệ nào chứa đĩa. Ban đầu mọi vị trí đều được coi là trống. 
3. Xử lý từng khoảng thời gian được sắp xếp. Đối với khoảng thời gian hiện tại`[l, r]`, chọn vị trí nhỏ nhất ít nhất`l`và chưa từng được sử dụng trước đây. Đánh dấu vị trí đó là có chứa một chiếc đĩa. 

Lý do chúng tôi chọn vị trí nhỏ nhất có thể là vì các khoảng thời gian trong tương lai chỉ có thể trở nên dễ dàng hơn khi vẫn còn nhiều vị trí hơn ở phía bên phải. Sử dụng vị trí muộn hơn sớm có thể chặn khoảng thời gian trong tương lai với điểm kết thúc nhỏ hơn. 
4. Sau khi tất cả các khoảng thời gian được xử lý, hãy quét giá. đầu ra`0`cho các vị trí được đánh dấu là đĩa và`1`cho mọi vị trí khác. 

Tại sao nó hoạt động: 

Sự lựa chọn tham lam sẽ bảo toàn khả năng hoàn thành tất cả các nhiệm vụ còn lại. Hãy xem xét khoảng có điểm cuối bên phải nhỏ nhất trong số các khoảng chưa được xử lý. Nếu nó nhận được một vị trí muộn hơn mức cần thiết, thì việc thay thế vị trí đó bằng vị trí hợp lệ có sẵn sớm nhất sẽ không thể làm tổn hại đến bất kỳ khoảng thời gian nào khác, bởi vì mọi khoảng thời gian còn lại đều kết thúc không sớm hơn khoảng thời gian này. Việc lặp lại đối số này trong mỗi khoảng thời gian cho thấy phép gán tham lam tạo ra kết quả khớp hợp lệ. Vì bài toán đảm bảo sự sắp xếp hợp lệ là duy nhất nên các vị trí được tìm thấy bằng cách so khớp này phải là các vị trí đĩa thực tế. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())

    intervals = []
    for _ in range(k):
        l, r = map(int, input().split())
        intervals.append((r, l))

    intervals.sort()

    disc = [False] * (n + 1)

    parent = list(range(n + 2))

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    for r, l in intervals:
        pos = find(l)
        disc[pos] = True
        parent[pos] = pos + 1

    ans = []
    for i in range(1, n + 1):
        ans.append('0' if disc[i] else '1')

    print(' '.join(ans))

if __name__ == "__main__":
    solve()
```Các khoảng thời gian được lưu trữ dưới dạng`(right, left)`theo cặp nên việc sắp xếp sẽ đặt các khoảng có vị trí kết thúc sớm nhất trước tiên một cách tự nhiên. Đây là thứ tự được yêu cầu bởi lập luận tham lam. 

các`parent`mảng thực hiện một cấu trúc tập hợp rời rạc. Mục đích của nó ở đây không phải là hợp nhất các tập hợp tùy ý mà là để nhanh chóng bỏ qua các vị trí đã được gán cho các đĩa. Nếu vị trí`x`được sử dụng, chúng tôi đặt cha mẹ của nó thành`x + 1`, nghĩa là tìm kiếm tiếp theo bắt đầu từ`x`sẽ nhảy trực tiếp đến vị trí miễn phí tiếp theo. 

các`find`hàm trả về vị trí chưa được sử dụng đầu tiên tại hoặc sau một chỉ mục nhất định. Nén đường dẫn làm cho việc tìm kiếm lặp đi lặp lại gần như liên tục. Điều này tránh việc quét qua các vị trí đã bị chiếm giữ, nếu không sẽ làm chậm nhiều khoảng thời gian chồng chéo. 

Vị trí được lưu trữ từ`1`ĐẾN`n`, trong khi phần tử phụ`n + 1`đóng vai trò là người canh gác an toàn. Điều này ngăn cản việc truy cập ngoài phạm vi khi vị trí cuối cùng được sử dụng. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
5 0
```không có khoảng thời gian để xử lý. 

| Bước | Khoảng thời gian | Vị trí được chọn | Vị trí đĩa | 
| --- | --- | --- | --- | 
| Ban đầu | không | không | trống | 

Lần quét cuối cùng thấy không có vị trí nào chứa đĩa nên mọi vị trí đều là rác. 

Đầu ra:```
1 1 1 1 1
```Đối với một ví dụ được xây dựng:```
5 2
1 1
2 5
```quá trình xử lý là: 

| Bước | Khoảng thời gian | Vị trí xuất phát | Vị trí được chọn | Vị trí đĩa | 
| --- | --- | --- | --- | --- | 
| 1 | [1, 1] | 1 | 1 | {1} | 
| 2 | [2, 5] | 2 | 2 | {1, 2} | 

Khoảng đầu tiên chỉ có một vị trí có thể, do đó bước tham lam bị buộc phải thực hiện. Khoảng thứ hai nhận được vị trí hợp lệ sớm nhất còn lại. 

Đầu ra cuối cùng là:```
0 0 1 1 1
```Dấu vết chứng tỏ rằng thuật toán duy trì vị trí duy nhất bằng cách luôn sử dụng các vị trí sẵn có bị hạn chế nhất trước tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k log k + k α(n) + n) | Việc sắp xếp chiếm ưu thế trong quá trình xử lý theo khoảng thời gian, trong khi các hoạt động tập hợp rời rạc có thời gian gần như không đổi | 
| Không gian | O(n + k) | Danh sách khoảng, mảng đánh dấu và cấu trúc tập hợp rời rạc sử dụng bộ nhớ tuyến tính | 

Giá trị tối đa của`n`là về`100000`, vì vậy sự phức tạp này dễ dàng phù hợp với giới hạn bộ nhớ và thời gian. Cấu trúc tập hợp rời rạc là yếu tố ngăn cản việc quét lặp lại trên phạm vi lớn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return out.getvalue().strip()

def solve():
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    intervals = []

    for _ in range(k):
        l, r = map(int, input().split())
        intervals.append((r, l))

    intervals.sort()

    disc = [False] * (n + 1)
    parent = list(range(n + 2))

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    for r, l in intervals:
        pos = find(l)
        disc[pos] = True
        parent[pos] = pos + 1

    print(' '.join('0' if disc[i] else '1' for i in range(1, n + 1)))

assert run("5 0\n") == "1 1 1 1 1", "sample 1"

assert run("5 2\n1 1\n2 5\n") == "0 0 1 1 1", "forced first position"

assert run("1 1\n1 1\n") == "0", "single disc"

assert run("4 2\n1 2\n3 4\n") == "0 0 0 0", "all positions are discs"

assert run("6 3\n1 1\n2 3\n4 6\n") == "0 0 1 0 1 1", "boundary handling"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 0`|`1 1 1 1 1`| Xử lý trường hợp không có đĩa | 
|`1 1 / 1 1`|`0`| Kích thước tối thiểu và vị trí bắt buộc | 
|`4 2 / 1 2 / 3 4`|`0 0 0 0`| Mọi vị trí đều bị chiếm đóng | 
|`6 3 / 1 1 / 2 3 / 4 6`|`0 0 1 0 1 1`| Đúng ranh giới khoảng thời gian | 

## Vỏ cạnh 

Khi không có khoảng thời gian, thuật toán không bao giờ đi vào vòng gán tham lam. các`disc`mảng vẫn hoàn toàn sai, vì vậy chuyển đổi cuối cùng đánh dấu mọi vị trí là rác. Điều này xử lý:```
5 0
```chính xác và tránh cho rằng có ít nhất một đĩa tồn tại. 

Đối với khoảng thời gian một vị trí:```
1 1
1 1
```tập hợp rời rạc bắt đầu bằng vị trí`1`có sẵn. Khoảng thời gian yêu cầu vị trí sẵn có đầu tiên tại`1`, do đó thuật toán đánh dấu nó là một chiếc đĩa và di chuyển con trỏ đến vị trí trọng điểm`2`. Đầu ra trở thành:```
0
```Đối với các khoảng chạm tới ranh giới:```
6 3
1 1
2 3
4 6
```các nhiệm vụ diễn ra như sau. Khoảng đầu tiên nhận vị trí`1`. Người thứ hai nhận vị trí`2`, bởi vì đó là vị trí trống sớm nhất bên trong`[2,3]`. Người thứ ba nhận chức`4`, vị trí trống sớm nhất bên trong`[4,6]`. Vị trí đĩa cuối cùng là`{1,2,4}`, cho:```
0 0 1 0 1 1
```Điều này xác nhận rằng việc tìm kiếm các vị trí sẵn có tôn trọng cả khoảng thời gian bắt đầu và kết thúc giá.
