---
title: "CF 102299G - Săn Lehys"
description: "Chúng tôi có n leshys, mỗi người có một sức mạnh cố định. Lúc đầu, mỗi leshy là gốc của hệ thống phân cấp riêng của nó nên có n cây riêng biệt. Một thao tác họp + i j nói rằng leshy j trở thành cấp dưới của leshy i."
date: "2026-08-13T08:12:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102299
codeforces_index: "G"
codeforces_contest_name: "2019 USP Try-outs"
rating: 0
weight: 102299
solve_time_s: 53
verified: true
draft: false
---

[CF 102299G - Săn leshys](https://codeforces.com/problemset/problem/102299/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`n`Leshys, mỗi người có một sức mạnh cố định. Lúc đầu, mỗi leshy là gốc của hệ thống phân cấp của chính nó, vì vậy có`n`cây riêng biệt. 

Một hoạt động cuộc họp`+ i j`nói rằng dâm đãng`j`trở nên phụ thuộc vào leshy`i`. Vì hệ thống phân cấp được đảm bảo không bao giờ chứa chu trình nên thao tác này sẽ nối hai cây phân cấp riêng biệt trước đó. Hướng chính xác giữa cha và con rất quan trọng đối với báo cáo, nhưng để trả lời các truy vấn, chúng ta chỉ cần biết leshy nào thuộc cùng một cây. 

Một hoạt động đe dọa`? i`bắt đầu từ leshy`i`và đi lên cho đến khi đạt tới người đứng đầu của hệ thống phân cấp đó. Sau khi toàn bộ hệ thống phân cấp biết về mối đe dọa, leshy ít mạnh nhất trong toàn bộ cây đó sẽ được giao nhiệm vụ xử lý nó. Do đó, một truy vấn sẽ yêu cầu công suất tối thiểu trong số tất cả các leshy trong thành phần được kết nối có chứa`i`. 

Các ràng buộc làm cho việc trình bày dự kiến ​​khá rõ ràng. Có thể có tới`10^5`leshys và nhiều như vậy`5 * 10^5`hoạt động, với một giới hạn thời gian một giây. Một thuật toán quét toàn bộ hệ thống phân cấp cho mọi truy vấn có thể thực hiện xung quanh`10^5 * 5 * 10^5 = 5 * 10^10`trong trường hợp xấu nhất, vượt xa thời hạn cho phép. Chúng tôi cần công việc được khấu hao gần như không đổi cho mỗi hoạt động. 

Quyền lực có thể lớn như`10^9`, nhưng chúng chỉ được so sánh và lưu trữ, vì vậy số nguyên Python là quá đủ. Thực tế là hệ thống phân cấp được đảm bảo không theo chu kỳ có nghĩa là mọi cuộc họp thành công đều sẽ hợp nhất hai cây khác nhau, đây chính xác là bối cảnh áp dụng cấu trúc liên kết tập hợp rời rạc. 

Trường hợp cạnh đầu tiên là một leshy duy nhất.```
1 1
7
? 1
```Câu trả lời là`7`. Một giải pháp giả định mọi truy vấn đều có truy vấn gốc để theo dõi có thể thất bại ở đây vì leshy được truy vấn đã là người dẫn đầu. 

Một trường hợp cạnh khác là giá trị công suất bằng nhau.```
2 2
5 5
+ 1 2
? 2
```Câu trả lời là`5`. Hệ thống phân cấp xác định ai là người lãnh đạo, nhưng quyền lực quyết định ai xử lý mối đe dọa. Việc nhầm lẫn hai khái niệm đó có thể dẫn đến kết quả sai nếu việc triển khai cố gắng chọn người lãnh đạo dựa trên quyền lực. 

Trường hợp hữu ích thứ ba là khi một cấp dưới mới được gắn bên dưới một leshy không phải gốc.```
3 3
10 4 7
+ 1 2
+ 2 3
? 3
```Hệ thống phân cấp kết quả là`1 -> 2 -> 3`, và câu trả lời là`4`. Thành phần là`{1, 2, 3}`, vì vậy câu trả lời là công suất tối thiểu trong toàn bộ thành phần chứ không chỉ đơn thuần là công suất tối thiểu trên đường đi từ`3`tới tận gốc. Một giải pháp chỉ tuân theo chuỗi gốc và cố gắng kiểm tra các đỉnh đã truy cập sẽ có tác dụng ở đây, nhưng nó vẫn sẽ thực hiện những công việc không cần thiết vì câu trả lời phụ thuộc vào toàn bộ cây. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là duy trì rõ ràng nguồn gốc của mọi leshy. Đối với một truy vấn`? i`, chúng ta có thể đi bộ từ`i`thông qua cha mẹ của nó cho đến khi đạt được người lãnh đạo, thu thập các thành viên của hệ thống phân cấp đó và sau đó tìm ra quyền lực tối thiểu của họ. Điều này đúng vì mọi leshy trong cùng một hệ thống phân cấp cuối cùng đều báo cáo cho cùng một người lãnh đạo và leshy được chỉ định chỉ đơn giản là thành viên có quyền lực tối thiểu trong hệ thống phân cấp đó. 

Vấn đề là khối lượng công việc. Một hệ thống phân cấp có thể chứa tất cả`n`leshys và một truy vấn có thể yêu cầu kiểm tra tất cả chúng. Với`m`hoạt động, một chuỗi trường hợp xấu nhất có thể yêu cầu`O(nm)`công việc, tùy thuộc vào`5 * 10^10`các lượt truy cập cơ bản cho các giới hạn nhất định. Ngay cả khi chúng tôi tối ưu hóa việc truyền tải cấp độ gốc, việc quét liên tục hệ thống phân cấp để tìm mức tối thiểu vẫn quá tốn kém. 

Quan sát hữu ích là câu trả lời cho một truy vấn không phụ thuộc vào hình dạng hoặc hướng của hệ thống phân cấp. Nó chỉ phụ thuộc vào tập hợp các leshy thuộc cùng một hệ thống phân cấp. Một cuộc họp sẽ hợp nhất hai bộ như vậy, trong khi một truy vấn yêu cầu công suất tối thiểu được lưu trữ trong một bộ. 

Đây chính xác là sự trừu tượng được xử lý bởi một tập hợp rời rạc, hay DSU. Mỗi hệ thống phân cấp được đại diện bởi một thành phần DSU. Đối với mọi thành phần, chúng tôi lưu trữ công suất tối thiểu của nó. Khi`+ i j`xảy ra, chúng tôi hợp nhất các thành phần có chứa`i`Và`j`và mức tối thiểu của thành phần mới chỉ đơn giản là mức tối thiểu của hai mức tối thiểu cũ. Khi`? i`xảy ra, chúng tôi tìm thấy đại diện của`i`và trả về mức tối thiểu được lưu trữ cho đại diện đó. 

Phương pháp brute-force hoạt động vì nó xây dựng lại hệ thống phân cấp cho mọi truy vấn nhưng không thành công khi cùng một hệ thống phân cấp lớn được truy vấn lặp đi lặp lại. Quan sát cho thấy chỉ thành viên thành phần và vấn đề tối thiểu của thành phần mới cho phép chúng tôi loại bỏ hoàn toàn cấu trúc phân cấp và duy trì chính xác các truy vấn thông tin cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(nm)`trường hợp xấu nhất |`O(n)`| Quá chậm | 
| Tối ưu |`O((n + m) α(n))`|`O(n)`| Đã chấp nhận | 

Đây`α(n)`là hàm Ackermann nghịch đảo, hàm này tăng chậm đến mức đối với tất cả các ràng buộc lập trình cạnh tranh thực tế, nó hoạt động giống như một hằng số nhỏ. 

## Hướng dẫn thuật toán 

1. Đọc sức mạnh của từng leshy và khởi tạo thành phần DSU chỉ chứa leshy đó. Thành phần tối thiểu của nó là sức mạnh của chính nó vì chưa có leshy nào khác thuộc về nó. 
2. Đối với một`+ i j`hoạt động, tìm đại diện DSU của`i`Và`j`. Cuộc họp kết hợp hai hệ thống phân cấp của chúng, vì vậy các thành phần của chúng phải được hợp nhất. 

Phương hướng nói rằng`j`trở nên phụ thuộc vào`i`không liên quan đến câu trả lời trong tương lai. Bất kể leshy nào là cha mẹ chính thức, cả hai bộ hiện đều thuộc về một hệ thống phân cấp. 
3. Khi hợp nhất hai thành phần, hãy sử dụng liên kết theo kích thước hoặc cấp bậc để cây DSU ở trạng thái nông. Lưu giá trị nhỏ nhất của hai thành phần nhỏ nhất làm giá trị nhỏ nhất của thành phần được hợp nhất. 
4. Đối với một`? i`hoạt động, tìm người đại diện của`i`và xuất ra mức tối thiểu được lưu trữ cho đại diện đó. Mọi leshy trong thành phần đó đều thuộc cùng một hệ thống phân cấp, vì vậy giá trị này chính xác là sức mạnh của leshy xử lý mối đe dọa. 
5. Sử dụng tính năng nén đường dẫn trong`find`. Bất cứ khi nào một đại diện được phát hiện, hãy nén đường dẫn từ leshy được truy vấn trực tiếp tới đại diện đó. Các hoạt động trong tương lai có thể định vị thành phần nhanh hơn nhiều. 

### Tại sao nó hoạt động 

Điều bất biến là mỗi thành phần DSU đại diện chính xác cho một hệ thống phân cấp hiện tại và mức tối thiểu được lưu trữ tại đại diện của nó là công suất tối thiểu trong số tất cả các leshys trong hệ thống phân cấp đó. 

Ban đầu, mỗi hệ thống phân cấp bao gồm một leshy, do đó, bất biến được giữ nguyên. Cuộc họp kết hợp hai hệ thống phân cấp riêng biệt thành một. DSU thực hiện chính xác việc hợp nhất đó và việc lấy mức tối thiểu của hai cực tiểu được lưu trữ sẽ mang lại mức tối thiểu trên hệ thống phân cấp kết hợp. Một truy vấn không sửa đổi hệ thống phân cấp, do đó việc tìm kiếm thành phần của`i`và việc trả lại mức tối thiểu được lưu trữ của nó mang lại mức độ mạnh mẽ nhất trong số những người nhận thức được mối đe dọa. Vì mọi thao tác đều bảo toàn tính bất biến này nên mọi truy vấn đều trả về công suất cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    power = list(map(int, input().split()))

    parent = list(range(n))
    size = [1] * n
    minimum = power[:]

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    out = []

    for _ in range(m):
        op = input().split()

        if op[0] == '+':
            a = int(op[1]) - 1
            b = int(op[2]) - 1

            a = find(a)
            b = find(b)

            if a == b:
                continue

            if size[a] < size[b]:
                a, b = b, a

            parent[b] = a
            size[a] += size[b]
            minimum[a] = min(minimum[a], minimum[b])

        else:
            x = int(op[1]) - 1
            root = find(x)
            out.append(str(minimum[root]))

    sys.stdout.write('\n'.join(out))

if __name__ == "__main__":
    solve()
```các`parent`mảng là rừng DSU. Ban đầu`parent[i] = i`, bởi vì mọi leshy đều bắt đầu với tư cách là đại diện cho hệ thống phân cấp của chính nó. các`size`mảng hỗ trợ liên kết theo kích thước, ngăn cấu trúc DSU bị thoái hóa thành chuỗi dài. 

các`minimum`mảng lưu trữ công suất tối thiểu cho mỗi đại diện thành phần. Các giá trị thuộc về những người không đại diện không cần phải cập nhật vì mọi truy vấn đều gọi trước`find`và sau đó đọc giá trị từ đại diện hiện tại. 

các`find`việc triển khai sử dụng phương pháp giảm một nửa đường dẫn lặp đi lặp lại. Nhiệm vụ`parent[x] = parent[parent[x]]`rút ngắn đường đi trong khi đi lên, tạo ra hành vi tiệm cận tương tự như nén đường đi thông thường mà không đệ quy. Điều này cũng tránh được những lo ngại về độ sâu đệ quy của Python. 

Các chỉ số trong đầu vào là dựa trên một, trong khi mảng Python dựa trên 0, do đó mọi chỉ số đầu vào sẽ giảm đi một ngay lập tức. Hoạt động hợp nhất kiểm tra xem hai đại diện đã bằng nhau chưa. Vấn đề đảm bảo rằng hệ thống phân cấp không bao giờ trở thành tuần hoàn, do đó việc hợp nhất như vậy sẽ không xảy ra ở đầu vào hợp lệ, nhưng việc giữ kiểm tra sẽ làm cho DSU mạnh mẽ và tránh làm hỏng kích thước thành phần hoặc mức tối thiểu. 

Không có vấn đề tràn số nguyên trong Python và dù sao thì lũy thừa lớn nhất cũng vừa vặn thoải mái trong số nguyên Python. Đầu ra được tích lũy trong một danh sách và được ghi một lần ở cuối, điều này tránh được chi phí thực hiện một thao tác đầu ra riêng biệt cho mỗi truy vấn. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên,```
4 6
3 12 5 6
? 2
+ 3 2
? 2
? 3
+ 4 1
? 1
```DSU bắt đầu với bốn thành phần đơn lẻ. 

| Hoạt động | Thành phần của các đỉnh được truy vấn/hợp nhất | Thành phần tối thiểu | Đầu ra | 
| --- | --- | --- | --- | 
|`? 2`|`{2}`|`{2}: 12`|`12`| 
|`+ 3 2`|`{2,3}`|`{2,3}: 5`| | 
|`? 2`|`{2,3}`|`{2,3}: 5`|`5`| 
|`? 3`|`{2,3}`|`{2,3}: 5`|`5`| 
|`+ 4 1`|`{1,4}`,`{2,3}`|`{1,4}: 3`,`{2,3}: 5`| | 
|`? 1`|`{1,4}`|`{1,4}: 3`|`3`| 

Kết quả đầu ra là`12`,`5`,`5`,`3`. Dấu vết cho thấy lý do tại sao hướng phân cấp không quan trọng đối với truy vấn. Sau đó`+ 3 2`, dâm đãng`2`báo cáo thông qua`3`, nhưng cả hai đều thuộc cùng một thành phần DSU có công suất tối thiểu là`5`. 

Đối với mẫu thứ hai,```
5 5
5 2 2 1 9
+ 4 2
+ 3 1
+ 5 3
+ 1 4
? 1
```Các hoạt động dần dần kết nối tất cả năm leshy. 

| Hoạt động | Các thành phần được hợp nhất | Tối thiểu thành phần mới | 
| --- | --- | --- | 
|`+ 4 2`|`{4}`Và`{2}`|`1`| 
|`+ 3 1`|`{3}`Và`{1}`|`2`| 
|`+ 5 3`|`{5}`Và`{3,1}`|`2`| 
|`+ 1 4`|`{5,3,1}`Và`{4,2}`|`1`| 
|`? 1`| thành phần truy vấn`{1,2,3,4,5}`|`1`| 

Câu trả lời là`1`. Ví dụ này chứng minh rằng hệ thống phân cấp cuối cùng có thể được định hình dưới dạng một chuỗi hoặc bất kỳ cây nào khác, trong khi DSU chỉ cần thực tế là tất cả năm đỉnh hiện thuộc về một thành phần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O((n + m) α(n))`| Mỗi liên kết và truy vấn thực hiện một số lượng hoạt động DSU không đổi, mỗi hoạt động được phân bổ theo độ phức tạp Ackermann nghịch đảo. | 
| Không gian |`O(n)`| DSU lưu trữ giá trị gốc, kích thước và giá trị tối thiểu cho mỗi leshy. | 

Với`n <= 10^5`Và`m <= 5 * 10^5`, giải pháp thực hiện công việc được khấu hao không đổi về cơ bản trên mỗi hoạt động. Điều này phù hợp một cách thoải mái với giới hạn thời gian một giây, trong khi việc truyền tải lặp đi lặp lại các hệ thống phân cấp lớn có thể yêu cầu hàng chục tỷ thao tác. 

## Trường hợp thử nghiệm 

Bộ dây thử nghiệm sau đây sử dụng cùng một`solve()`hoạt động như giải pháp được gửi. Thử nghiệm kích thước tối đa tạo ra`100000`leshys và`499999`các hoạt động hợp nhất, đủ lớn để thực hiện hành vi tiệm cận dự kiến ​​trong khi vẫn giữ cho bài kiểm tra mang tính thực tế.```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())
    power = list(map(int, input().split()))

    parent = list(range(n))
    size = [1] * n
    minimum = power[:]

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    out = []

    for _ in range(m):
        op = input().split()

        if op[0] == '+':
            a = int(op[1]) - 1
            b = int(op[2]) - 1

            a = find(a)
            b = find(b)

            if a == b:
                continue

            if size[a] < size[b]:
                a, b = b, a

            parent[b] = a
            size[a] += size[b]
            minimum[a] = min(minimum[a], minimum[b])
        else:
            x = int(op[1]) - 1
            out.append(str(minimum[find(x)]))

    sys.stdout.write('\n'.join(out))

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

# Sample 1
assert run("""\
4 6
3 12 5 6
? 2
+ 3 2
? 2
? 3
+ 4 1
? 1
""") == "12\n5\n5\n3", "sample 1"

# Sample 2
assert run("""\
5 5
5 2 2 1 9
+ 4 2
+ 3 1
+ 5 3
+ 1 4
? 1
""") == "1", "sample 2"

# Minimum-size input
assert run("""\
1 3
42
? 1
? 1
? 1
""") == "42\n42\n42", "single leshy"

# All powers equal
assert run("""\
4 5
7 7 7 7
+ 1 2
+ 3 4
? 2
+ 2 3
? 4
""") == "7\n7", "equal powers"

# Boundary and off-by-one case
assert run("""\
5 6
10 8 6 4 2
+ 2 5
? 5
+ 4 3
? 3
+ 1 4
? 5
""") == "2\n4\n2", "component boundaries"

# Large input
n = 100000
m = 499999
powers = " ".join(["1000000000"] * n)

ops = []
for i in range(1, n):
    ops.append(f"+ {i} {i + 1}")
ops.append("? 100000")

large_input = f"{n} {m}\n{powers}\n" + "\n".join(
    ops + ["? 1"] * (m - len(ops))
) + "\n"

# The generated sequence has many repeated queries after all merges.
assert run(large_input).splitlines()[0] == "1000000000", "large input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 3 / 42 / ? 1 / ? 1 / ? 1`|`42`,`42`,`42`| Một hệ thống phân cấp chỉ chứa người lãnh đạo của nó | 
| Bốn quyền đều bằng nhau`7`|`7`,`7`| Xử lý tối thiểu khi có nhiều leshys buộc nhau | 
| Năm leshys với các thành phần được hợp nhất dần dần |`2`,`4`,`2`| Ranh giới thành phần và chuyển đổi chỉ số | 
|`100000`leshys với hàng trăm nghìn thao tác |`1000000000`cho truy vấn đầu tiên | Hiệu suất DSU quy mô lớn và các truy vấn lặp lại | 

## Vỏ cạnh 

Trường hợp phân cấp đơn lẻ được xử lý bởi trạng thái DSU ban đầu. Vì```
1 1
7
? 1
```

`find(1)`ngay lập tức trả về người đại diện duy nhất và`minimum[0]`là`7`, vì vậy đầu ra là`7`. Không cần mã trường hợp đặc biệt vì hệ thống phân cấp một phần tử đã là thành phần DSU hợp lệ. 

Quyền lực ngang nhau cũng được xử lý một cách tự nhiên. Vì```
2 2
5 5
+ 1 2
? 2
```sự hợp nhất tính toán`min(5, 5)`, đó là`5`. Thuật toán không bao giờ sử dụng chỉ số leshy hoặc người đứng đầu phân cấp để thay thế cho quyền lực, vì vậy các giá trị bằng nhau sẽ không gây ra sự mơ hồ. 

Việc hợp nhất có thể đính kèm một hệ thống phân cấp bên dưới một leshy không phải là đại diện của chính nó. Ví dụ,```
3 3
10 4 7
+ 1 2
+ 2 3
? 3
```Sau lần hợp nhất đầu tiên,`{1,2}`có tối thiểu`4`. Sự hợp nhất thứ hai tham gia leshy`3`với thành phần đó, đưa ra mức tối thiểu`4`lại. Truy vấn tìm thấy đại diện của`3`và trả về`4`. DSU không cần biết rằng hệ thống phân cấp chính thức là`1 -> 2 -> 3`, bởi vì toàn bộ tập hợp là yếu tố quyết định câu trả lời. 

Giá trị công suất lớn nhất là một ranh giới khác đáng để kiểm tra. Một giá trị như`1000000000`được lưu trữ trực tiếp trong`minimum`, Và`min()`so sánh nó bình thường. Python có các số nguyên có độ chính xác tùy ý, do đó không có rủi ro tràn ngay cả khi việc triển khai sau đó được điều chỉnh để lưu trữ các giá trị số nguyên dẫn xuất khác. 

Cuối cùng, một truy vấn có thể đến ngay trước bất kỳ sự hợp nhất nào liên quan đến leshy được truy vấn. Ví dụ,```
3 2
8 3 5
? 3
? 1
```cả hai truy vấn đều hoạt động trên các thành phần đơn lẻ, tạo ra`5`Và`8`. Biểu diễn DSU có giá trị ngay từ thao tác đầu tiên, do đó các truy vấn không yêu cầu bất kỳ giai đoạn xây dựng trước nào.
