---
title: "CF 102254F - Vấn đề về tình bạn"
description: "Có (n) sinh viên, mỗi sinh viên được xác định bằng một tên duy nhất. Ban đầu mỗi học sinh thuộc về một đội riêng biệt. Thao tác loại 1 sẽ hợp nhất các nhóm có chứa hai học sinh được chỉ định. Phép toán loại 2 hỏi xem hai học sinh đó hiện có thuộc cùng một đội hay không."
date: "2026-08-17T21:20:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102254
codeforces_index: "F"
codeforces_contest_name: "IME++ Starters Try-outs 2019"
rating: 0
weight: 102254
solve_time_s: 446
verified: false
draft: false
---

[CF 102254F - Các vấn đề về tình bạn](https://codeforces.com/problemset/problem/102254/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 26s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Có (n) sinh viên, mỗi sinh viên được xác định bằng một tên duy nhất. Ban đầu mỗi học sinh thuộc về một đội riêng biệt. Thao tác loại 1 sẽ hợp nhất các nhóm có chứa hai học sinh được chỉ định. Phép toán loại 2 hỏi xem hai học sinh đó hiện có thuộc cùng một đội hay không. 

Tên là các chuỗi, vì vậy trước khi xử lý các thao tác, chúng ta cần cách dịch từng tên thành một mã định danh số nguyên nhỏ gọn. Khi đã xong, vấn đề thực tế hoàn toàn là việc duy trì các nhóm được kết nối trong quá trình hợp nhất và trả lời các truy vấn kết nối. 

Đầu vào chứa tối đa (10^5) sinh viên và (10^5) thao tác. Với kích thước đó, một giải pháp (O(nq)) có thể thực hiện tối đa (10^{10}) thao tác cơ bản, vượt xa giới hạn thời gian 1 giây cho phép. Bước tiền xử lý (O(n^2)) cũng không thể thực hiện được. Chúng ta cần các phép toán có thời gian không đổi hoặc rất gần với thời gian đó cho mỗi truy vấn. 

Có một số trường hợp việc thực hiện bất cẩn có thể cho kết quả sai. Một học sinh có thể được sáp nhập vào một nhóm mà trước đó chính nó đã được sáp nhập với một nhóm khác. Ví dụ:```
3 3
Ana
Bob
Cat
1 Ana Bob
1 Bob Cat
2 Ana Cat
```Đầu ra đúng là:```
yes
```Một giải pháp chỉ nhớ bạn gần đây nhất của mỗi học sinh có thể trả lời sai`no`, bởi vì Ana chưa bao giờ được sáp nhập trực tiếp với Cat. Tư cách thành viên nhóm có tính bắc cầu, vì vậy toàn bộ thành phần được kết nối đều quan trọng. 

Truy vấn loại 1 cũng có thể liên quan đến hai học sinh đã ở cùng một nhóm:```
2 2
Ana
Bob
1 Ana Bob
1 Ana Bob
```Không có thay đổi thứ hai để thực hiện. Việc triển khai hợp nhất bất cẩn giả định các gốc khác nhau có thể làm hỏng cấu trúc dữ liệu của nó nếu nó không xử lý trường hợp này. 

Cuối cùng, một truy vấn có thể đề cập đến những học sinh có các nhóm có quy mô rất khác nhau:```
4 2
Ana
Bob
Cat
Dan
1 Ana Bob
2 Cat Dan
```Câu trả lời là`no`, bởi vì cả Cat và Dan đều chưa được kết nối với nhau. Một giải pháp vô tình coi các chỉ mục không liên quan là được kết nối do các giá trị gốc mặc định có thể không thành công ở đây. Mỗi học sinh phải bắt đầu với tư cách là gốc của thành phần riêng của mình. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là lưu trữ rõ ràng các thành viên của mỗi nhóm. Ban đầu mỗi đội có một học sinh. Khi việc hợp nhất yêu cầu chúng tôi kết hợp các nhóm của (x) và (y), chúng tôi có thể lấy mọi học sinh từ một nhóm và thay đổi mã định danh nhóm đó thành mã định danh của nhóm kia. Sau đó, truy vấn loại 2 sẽ so sánh hai mã định danh nhóm được lưu trữ. 

Điều này đúng vì sau mỗi lần hợp nhất, mọi học sinh trong nhóm kết quả đều nhận được cùng một mã định danh. Vấn đề là số lượng công việc được yêu cầu khi hợp nhất. Trong trường hợp xấu nhất, một nhóm có thể chứa (10^5) học sinh và một chuỗi truy vấn dài có thể liên tục buộc chúng tôi phải kiểm tra một nhóm lớn. Với (10^5) truy vấn, việc triển khai đơn giản có thể tiếp cận (10^5 \time 10^5 = 10^{10}) thông tin cập nhật của học sinh. 

Phương pháp brute-force hoạt động vì thông tin duy nhất mà truy vấn thực sự cần là liệu hai học sinh có cùng mã định danh thành phần hay không. Thất bại xuất phát từ việc duy trì thông tin đó về mặt vật lý cho mọi thành viên sau mỗi lần hợp nhất. Chúng tôi cần một sự thể hiện trong đó việc tham gia hai nhóm chỉ thay đổi một lượng nhỏ thông tin được lưu trữ, bất kể có bao nhiêu học sinh đã ở trong nhóm. 

Quan sát quan trọng là các nhóm hình thành nên các thành phần được kết nối rời rạc. Một học sinh không cần phải biết hết mọi học sinh khác trong nhóm của mình. Nó chỉ cần dẫn chúng ta đến đại diện của đội đó. Nếu cuối cùng hai học sinh có cùng một đại diện thì họ thuộc cùng một thành phần. 

Đây chính xác là cài đặt cho cấu trúc hợp tập hợp rời rạc, còn được gọi là DSU hoặc tìm hợp. Mỗi thành phần được đại diện bởi một gốc. Hợp nhất nối hai gốc thay vì viết lại mọi thành viên của một trong hai thành phần. Việc nén đường dẫn làm cho các tìm kiếm trong tương lai trở nên rất ngắn, trong khi việc kết hợp theo kích thước hoặc thứ hạng sẽ ngăn các cây bên trong trở nên sâu một cách không cần thiết. 

Với cả hai cách tối ưu hóa, mỗi thao tác đều mất thời gian khấu hao (O(\alpha(n))), trong đó (\alpha) là hàm Ackermann nghịch đảo và thực tế là không đổi đối với tất cả các kích thước đầu vào thực tế. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(nq)) trường hợp xấu nhất | (O(n)) | Quá chậm | 
| DSU với tính năng nén đường dẫn và kết hợp theo kích thước | (O((n+q)\alpha(n))) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Gán cho mỗi học sinh một ID nguyên từ (0) đến (n-1), sử dụng từ điển từ tên đến ID. ID số nguyên làm cho mảng DSU nhỏ gọn và tránh việc lưu trữ hoặc so sánh nhiều lần các chuỗi bên trong cấu trúc dữ liệu. 
2. Tạo một`parent`mảng ở đâu`parent[i] = i`cho mỗi học sinh. Ban đầu mỗi học sinh là thành viên duy nhất trong nhóm của mình, vì vậy mỗi học sinh là đại diện của chính mình. 
3. Tạo một`size`mảng được khởi tạo thành (1). Giá trị ghi lại số lượng sinh viên thuộc thành phần được đại diện bởi mỗi gốc. Nó sẽ được sử dụng để quyết định gốc nào sẽ trở thành gốc trong quá trình hợp nhất. 
4. Đối với truy vấn loại 1, hãy chuyển đổi cả hai tên thành ID số nguyên của chúng và tìm gốc của chúng bằng`find`. Nếu các nghiệm bằng nhau thì các học sinh đã ở cùng một đội nên phép tính không thay đổi gì. 
5. Nếu các gốc khác nhau, hãy so sánh kích thước thành phần và làm cho điểm gốc của thành phần nhỏ hơn thành điểm gốc của thành phần lớn hơn. Thêm kích thước nhỏ hơn vào kích thước của gốc lớn hơn. Gắn cây nhỏ hơn bên dưới cây lớn hơn sẽ giữ cho cây DSU nông. 
6. Đối với truy vấn loại 2, hãy tìm nghiệm của cả hai học sinh. In`yes`nếu các gốc bằng nhau và`no`nếu không thì. Root là đại diện cho các đội hiện tại của họ, vì vậy sự bình đẳng về nguồn gốc chính là điều kiện để các học sinh thuộc cùng một đội. 
7. Trong`find`, đi theo các con trỏ cha cho đến khi đạt tới đỉnh chính là đỉnh cha của nó. Trong khi quay lại, hãy thay thế cha mẹ của mỗi học sinh đã đến thăm bằng gốc đó. Đây là tính năng nén đường dẫn và giúp các truy vấn trong tương lai liên quan đến những sinh viên đó nhanh hơn nhiều. 

### Tại sao nó hoạt động 

Điều bất biến là mỗi thành phần DSU đại diện cho chính xác một nhóm hiện tại và mọi học sinh trong nhóm đó cuối cùng đều có cùng một gốc. Ban đầu điều này đúng vì mỗi học sinh đều cô đơn. Việc hợp nhất chỉ kết nối hai gốc khác nhau, do đó nó kết hợp chính xác hai nhóm tương ứng thành một thành phần mà không kết nối các nhóm không liên quan. Nén đường dẫn chỉ thay đổi biểu diễn bên trong của một thành phần chứ không thay đổi đỉnh nào thuộc về thành phần đó. Do đó, hai học sinh có cùng một nghiệm chính xác khi đội của họ đã được nối thông qua một số chuỗi phép tính loại 1, vì vậy mọi câu trả lời loại 2 đều đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())

    name_to_id = {}
    for i in range(n):
        name = input().strip()
        name_to_id[name] = i

    parent = list(range(n))
    size = [1] * n

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    out = []

    for _ in range(q):
        t, sx, sy = input().split()
        x = name_to_id[sx]
        y = name_to_id[sy]

        rx = find(x)
        ry = find(y)

        if t == '1':
            if rx == ry:
                continue

            if size[rx] < size[ry]:
                rx, ry = ry, rx

            parent[ry] = rx
            size[rx] += size[ry]

        else:
            out.append("yes" if rx == ry else "no")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Từ điển được xây dựng trước tiên vì tất cả các truy vấn sau này đều xác định tên học sinh. Mỗi tên nhận được một ID số nguyên ổn định, do đó DSU không bao giờ phải hoạt động trực tiếp trên chuỗi. 

các`parent`mảng lưu trữ cấu trúc rừng. Một gốc thỏa mãn`parent[root] == root`, mang lại`find`điều kiện dừng chính xác. Việc triển khai lặp lại sẽ tránh được những lo ngại về độ sâu đệ quy của Python, trong khi`parent[x] = parent[parent[x]]`thực hiện giảm một nửa đường dẫn trong quá trình tìm kiếm. 

Đối với một sự kết hợp, gốc rễ được tìm thấy trước khi thay đổi bất cứ điều gì. Nếu chúng đã bằng nhau thì thao tác sẽ bị bỏ qua. Ngược lại, thành phần nhỏ hơn sẽ được gắn vào thành phần lớn hơn. Kích thước của gốc mới chỉ tăng lên sau khi mối quan hệ cha mẹ đã được thay đổi và kích thước được lưu trữ ở gốc con không còn quan trọng nữa. 

Đối với một truy vấn, không có cấu trúc nào được sửa đổi có chủ ý ngoài việc nén đường dẫn được thực hiện bởi`find`. Hai gốc được so sánh trực tiếp. Không có vấn đề về ranh giới lập chỉ mục vì ID nằm trong khoảng từ (0) đến (n-1) và số nguyên Python không gặp vấn đề tràn. 

Đầu ra được tích lũy thành một danh sách và được ghi một lần vào cuối. Với tối đa (10^5) truy vấn, điều này tránh được các lệnh gọi đầu ra lặp lại không cần thiết và giữ I/O thoải mái trong giới hạn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Trạng thái DSU quan trọng có thể được biểu diễn bằng các bộ thành phần. Tên gốc bên dưới mô tả các thành phần về mặt khái niệm, trong khi việc triển khai lưu trữ các gốc nguyên. 

| Truy vấn | Hoạt động | Linh kiện sau khi vận hành | Đầu ra | 
| --- | --- | --- | --- | 
| 1 |`1 Naum Rebeca`|`{Naum, Rebeca}`,`{Navarro}`,`{Arnon}`,`{Matheus}`,`{Xavier}`| | 
| 2 |`2 Rebeca Naum`| không thay đổi |`yes`| 
| 3 |`1 Matheus Xavier`|`{Matheus, Xavier}`,`{Naum, Rebeca}`,`{Navarro}`,`{Arnon}`| | 
| 4 |`1 Navarro Arnon`|`{Navarro, Arnon}`,`{Matheus, Xavier}`,`{Naum, Rebeca}`| | 
| 5 |`2 Matheus Navarro`| không thay đổi |`no`| 
| 6 |`2 Rebeca Matheus`| không thay đổi |`no`| 
| 7 |`1 Navarro Matheus`|`{Navarro, Arnon, Matheus, Xavier}`,`{Naum, Rebeca}`| | 
| 8 |`2 Xavier Arnon`| không thay đổi |`yes`| 
| 9 |`2 Xavier Rebeca`| không thay đổi |`no`| 
| 10 |`1 Rebeca Arnon`|`{Navarro, Arnon, Matheus, Xavier, Naum, Rebeca}`| | 
| 11 |`2 Naum Rebeca`| không thay đổi |`yes`| 
| 12 |`2 Naum Matheus`| không thay đổi |`yes`| 
| 13 |`2 Naum Xavier`| không thay đổi |`yes`| 

Phần thú vị là truy vấn 10. Rebeca thuộc thành phần chứa Naum, trong khi Arnon thuộc thành phần chứa Navarro, Matheus và Xavier. Sự kết hợp giữa hai gốc đó nên cả sáu học sinh bây giờ đều có cùng một người đại diện. Ba truy vấn cuối cùng chứng minh tính bắc cầu: Naum chưa bao giờ được hợp nhất trực tiếp với Matheus hoặc Xavier, nhưng cả ba đều trở thành thành viên của cùng một thành phần. 

### Mẫu 2 

| Truy vấn | Hoạt động | Linh kiện sau khi vận hành | Đầu ra | 
| --- | --- | --- | --- | 
| 1 |`1 Sergio Mateus`|`{Sergio, Mateus}`,`{Cesar}`,`{Gustavo}`,`{Caio}`,`{Yu}`| | 
| 2 |`1 Cesar Yu`|`{Cesar, Yu}`,`{Sergio, Mateus}`,`{Gustavo}`,`{Caio}`| | 
| 3 |`1 Cesar Gustavo`|`{Cesar, Yu, Gustavo}`,`{Sergio, Mateus}`,`{Caio}`| | 
| 4 |`2 Cesar Sergio`| không thay đổi |`no`| 
| 5 |`1 Caio Mateus`|`{Caio, Sergio, Mateus}`,`{Cesar, Yu, Gustavo}`| | 
| 6 |`1 Gustavo Yu`| không thay đổi, đã có cùng một thành phần | | 
| 7 |`2 Caio Sergio`| không thay đổi |`yes`| 
| 8 |`2 Gustavo Sergio`| không thay đổi |`no`| 

Truy vấn 6 là một trường hợp hữu ích. Gustavo và Yu đã được kết nối thông qua Cesar nên liên minh không tạo ra thành phần mới. DSU phát hiện điều này vì cả hai tên đều tạo ra cùng một gốc. Truy vấn 7 sau đó xác nhận rằng việc kết nối Caio với Mateus cũng kết nối Caio với Sergio thông qua thành phần hiện có. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O((n+q)\alpha(n))) | Việc xây dựng bản đồ tên mất (O(n)) và mọi hoạt động DSU đều có chi phí khấu hao (O(\alpha(n))). | 
| Không gian | (O(n)) | Bản đồ tên, mảng cha, mảng kích thước và bộ nhớ đầu ra đều tăng tuyến tính với kích thước đầu vào. | 

Đối với (n,q \le 10^5), các phép toán DSU thực tế là thời gian không đổi. Giải pháp này chỉ thực hiện một lượng tiền xử lý tuyến tính và một số lượng thao tác rất nhỏ trên mỗi truy vấn, do đó, giải pháp này vừa vặn thoải mái trong giới hạn 1 giây và 256 MB. Số lượng sinh viên và truy vấn được lưu trữ tối đa cũng đủ nhỏ cho các từ điển và mảng số nguyên của Python. 

## Trường hợp thử nghiệm 

Trình trợ giúp kiểm tra bên dưới sử dụng tương tự`solve`logic như chương trình được gửi, nhưng chấp nhận một chuỗi và ghi lại kết quả đầu ra của nó để có thể kiểm tra các trường hợp bằng các xác nhận.```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n, q = map(int, input().split())

    name_to_id = {}
    for i in range(n):
        name_to_id[input().strip()] = i

    parent = list(range(n))
    size = [1] * n

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    out = []

    for _ in range(q):
        t, sx, sy = input().split()
        x = name_to_id[sx]
        y = name_to_id[sy]

        rx = find(x)
        ry = find(y)

        if t == '1':
            if rx == ry:
                continue

            if size[rx] < size[ry]:
                rx, ry = ry, rx

            parent[ry] = rx
            size[rx] += size[ry]
        else:
            out.append("yes" if rx == ry else "no")

    return "\n".join(out)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solve()
    finally:
        sys.stdin = old_stdin

sample1 = """\
6 13
Navarro
Arnon
Matheus
Xavier
Rebeca
Naum
1 Naum Rebeca
2 Rebeca Naum
1 Matheus Xavier
1 Navarro Arnon
2 Matheus Navarro
2 Rebeca Matheus
1 Navarro Matheus
2 Xavier Arnon
2 Xavier Rebeca
1 Rebeca Arnon
2 Naum Rebeca
2 Naum Matheus
2 Naum Xavier
"""

assert run(sample1) == """\
yes
no
no
yes
no
yes
yes
yes""", "sample 1"

sample2 = """\
6 8
Sergio
Yu
Mateus
Cesar
Gustavo
Caio
1 Sergio Mateus
1 Cesar Yu
1 Cesar Gustavo
2 Cesar Sergio
1 Caio Mateus
1 Gustavo Yu
2 Caio Sergio
2 Gustavo Sergio
"""

assert run(sample2) == """\
no
yes
no""", "sample 2"

minimum_case = """\
2 4
Aa
Bb
2 Aa Bb
1 Aa Bb
2 Aa Bb
1 Aa Bb
"""

assert run(minimum_case) == """\
no
yes""", "minimum size and repeated union"

transitive_case = """\
5 8
Aa
Bb
Cc
Dd
Ee
1 Aa Bb
1 Cc Dd
2 Aa Dd
1 Bb Cc
2 Aa Dd
2 Bb Dd
1 Aa Dd
2 Aa Ee
"""

assert run(transitive_case) == """\
no
yes
yes
no""", "transitive connectivity"

repeated_queries_case = """\
4 7
Aa
Bb
Cc
Dd
2 Aa Bb
2 Cc Dd
1 Aa Bb
2 Aa Bb
1 Aa Bb
1 Cc Dd
2 Cc Dd
"""

assert run(repeated_queries_case) == """\
no
no
yes
yes""", "repeated queries and no-op unions"

n = 100000
names = [f"A{i}" for i in range(n)]
maximum_case = [f"{n} 3"]
maximum_case.extend(names)
maximum_case.append(f"1 {names[0]} {names[-1]}")
maximum_case.append(f"2 {names[0]} {names[-1]}")
maximum_case.append(f"2 {names[1]} {names[-2]}")
maximum_case = "\n".join(maximum_case) + "\n"

assert run(maximum_case) == """\
yes
no""", "maximum size"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 |`yes`,`no`,`no`,`yes`,`no`,`yes`,`yes`,`yes`| Nhiều sự hợp nhất, kết nối bắc cầu và hợp nhất hai thành phần lớn | 
| Mẫu 2 |`no`,`yes`,`no`| Một liên minh không hoạt động và các thành phần riêng biệt | 
| Trường hợp tối thiểu |`no`,`yes`| Số lượng sinh viên tối thiểu và sự kết hợp lặp lại của một thành phần hiện có | 
| Trường hợp chuyển tiếp |`no`,`yes`,`yes`,`no`| Kết nối thông qua một số học sinh trung cấp | 
| Trường hợp truy vấn lặp lại |`no`,`no`,`yes`,`yes`| Truy vấn trước và sau các kết hợp, bao gồm cả các kết hợp lặp lại | 
| Trường hợp tối đa |`yes`,`no`| (10^5) sinh viên và ID ranh giới ở cả hai đầu của mảng | 

## Vỏ cạnh 

Một chuỗi các công đoàn kiểm tra xem giải pháp có hiểu được sự kết nối hơn là các mối quan hệ trực tiếp hay không. Ví dụ:```
3 3
Ana
Bob
Cat
1 Ana Bob
1 Bob Cat
2 Ana Cat
```Sau lần kết hợp đầu tiên, Ana và Bob có cùng gốc. Sau phép kết thứ hai, thành phần của Bob được nối với thành phần của Cat. Do đó, Ana và Cat có cùng một gốc nên kết quả là:```
yes
```DSU xử lý việc này mà không kết nối rõ ràng từng cặp sinh viên. 

Sự kết hợp lặp đi lặp lại không được tạo thành phần mới hoặc làm hỏng kích thước của nó. Coi như:```
2 4
Aa
Bb
1 Aa Bb
1 Aa Bb
2 Aa Bb
2 Bb Aa
```Liên minh đầu tiên tạo ra một thành phần. Liên minh thứ hai tìm thấy các gốc giống hệt nhau và quay trở lại ngay lập tức. Cả hai truy vấn sau đó so sánh cùng một gốc và tạo ra:```
yes
yes
```Một truy vấn được thực hiện trước bất kỳ sự hợp nhất nào phải phân biệt các thành phần đơn lẻ riêng biệt. Ví dụ:```
2 1
Aa
Bb
2 Aa Bb
```Cả hai học sinh ban đầu đều có gốc riêng, vì vậy gốc khác nhau và kết quả là:```
no
```Điều này kiểm tra việc khởi tạo của`parent`, vì việc gán cho mỗi học sinh một gốc mặc định chung ngẫu nhiên sẽ tạo ra kết quả không chính xác`yes`. 

Trường hợp ranh giới kích thước tối đa sử dụng ID sinh viên đầu tiên và cuối cùng:```
4 3
Aaaa
Bbbb
Cccc
Dddd
1 Aaaa Dddd
2 Aaaa Dddd
2 Bbbb Dddd
```Sau lần phẫu thuật đầu tiên, chỉ`Aaaa`Và`Dddd`chia sẻ một thành phần Đầu ra là:```
yes
no
```Việc triển khai không bao giờ giả định rằng ID của học sinh có liên quan đến tên hoặc vị trí của nó ngoài ánh xạ từ điển, do đó, học sinh ở cả hai đầu của phạm vi ID được xử lý giống hệt nhau. 

Trường hợp khó phát hiện cuối cùng là khi một phép hợp kết nối hai thành phần đã chứa nhiều sinh viên:```
5 6
Aa
Bb
Cc
Dd
Ee
1 Aa Bb
1 Cc Dd
1 Aa Cc
2 Bb Dd
2 Aa Dd
2 Bb Ee
```Sau lần hợp thứ ba, bốn học sinh đầu tiên thuộc về một thành phần. Cả hai`Bb Dd`Và`Aa Dd`do đó sản xuất`yes`, trong khi`Bb Ee`sản xuất`no`. DSU đạt được điều này bằng cách thay đổi một con trỏ gốc thay vì cập nhật riêng lẻ từng học sinh, đây là lý do chính khiến phương pháp này chia tỷ lệ thành các phép toán (10^5).
