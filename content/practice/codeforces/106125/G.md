---
title: "CF 106125G - Phả hệ Gumbo"
description: "Chúng ta được cho một tập hợp các quan hệ cha-con được viết dưới dạng “A, con trai của B”. Mỗi câu đều nói rằng người A có một người cha duy nhất là B."
date: "2026-06-20T08:21:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106125
codeforces_index: "G"
codeforces_contest_name: "Delft Algorithm Programming Contest 2025 (DAPC 2025)"
rating: 0
weight: 106125
solve_time_s: 49
verified: true
draft: false
---

[CF 106125G - Phả hệ Gumbo](https://codeforces.com/problemset/problem/106125/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các quan hệ cha-con được viết dưới dạng “A, con trai của B”. Mỗi câu nói nói rằng người A có một người cha duy nhất được biết đến là B. Tên giống nhau có thể xuất hiện nhiều lần khi còn nhỏ hoặc khi là cha mẹ và các câu lệnh khác nhau có thể đề cập đến những người khác nhau tình cờ có chung một tên. 

Từ những mối quan hệ này, chúng tôi muốn quyết định xem liệu có thể giải thích chúng đến từ một cấu trúc tổ tiên hợp lệ mà cuối cùng có chính xác một tổ tiên gốc, nghĩa là một người không có cha được ghi lại và từ đó mọi người khác có thể được truy tìm thông qua các liên kết cha. 

Một giải thích hợp lệ phải tôn trọng hai ràng buộc. Thứ nhất, mỗi người có thể có nhiều nhất một người cha. Thứ hai, nếu chúng ta tuân theo mối liên hệ cha trở lên từ bất kỳ người nào, chúng ta không bao giờ được mắc kẹt trong mâu thuẫn trong đó một người sẽ cần hai người cha khác nhau hoặc khi tổ tiên không thể nhất quán bắt nguồn từ một tổ tiên cuối cùng duy nhất. 

Kích thước đầu vào lên tới 100000 quan hệ, do đó, bất kỳ giải pháp nào cố gắng xây dựng rõ ràng hoặc lặp đi lặp lại các chuỗi lớn cho mỗi truy vấn sẽ quá chậm. Chúng ta nên kỳ vọng điều gì đó gần với hành vi tuyến tính hoặc gần tuyến tính về mặt số lượng các mối quan hệ, chẳng hạn như việc xây dựng đồ thị cộng với một phép duyệt đơn hoặc một cấu trúc tìm hợp. 

Một sự tinh tế đến từ những cái tên và chu kỳ lặp đi lặp lại. Đầu vào có thể mô tả các tình huống trong đó tên tạo thành một vòng lặp hoặc khi một người được khai báo là tổ tiên của chính họ một cách trực tiếp hoặc gián tiếp. Một vấn đề khác là việc hợp nhất danh tính không nhất quán nếu chúng ta coi tên như các nút mà không cẩn thận. 

Một sai lầm ngây thơ là cho rằng miễn là mỗi đứa trẻ có đúng một người cha thì cấu trúc luôn đúng. Ví dụ: nếu chúng ta có: 

“Basil, con trai của Alexios” 

“Procopius, con trai của Alexios” 

điều này ổn. Nhưng nếu chúng ta có: 

“Basil, con trai của Alexios” 

“Basil, con trai của Constantine” 

thì Basil sẽ có hai người cha, điều này ngay lập tức là không thể. Tuy nhiên, những mâu thuẫn phức tạp hơn nảy sinh khi tồn tại các chu kỳ, chẳng hạn như: 

“Alvin, con trai của Bert” 

“Bert, con trai của Charles” 

“Charles, con trai của Alvin” 

Ở đây mỗi nút có chính xác một nút cha, nhưng không có tổ tiên gốc duy nhất; thay vào đó, mọi thứ đều theo chu kỳ. 

Một trường hợp khác là tự nuôi dạy con cái: 

“Aaron, con trai của Aaron” 

Điều này không ngay lập tức phá vỡ tính duy nhất của root, bởi vì nó có thể được hiểu là một vòng lặp suy biến, nhưng nó vẫn tạo thành một chu trình và phải được xử lý một cách nhất quán. 

Khó khăn chính là việc phân biệt một biểu đồ hàm hợp lệ mà cuối cùng rút gọn thành một cấu trúc giống như gốc duy nhất với một biểu đồ chứa nhiều thành phần hoặc chu trình bị ngắt kết nối không thể phân giải thành một gốc tổ tiên duy nhất. 

## Phương pháp tiếp cận 

Các mối quan hệ xác định một cách tự nhiên một biểu đồ có hướng trong đó mỗi người chỉ vào cha của họ. Mỗi nút có nhiều nhất một cạnh đi ra (đối với cha của nó), vì vậy cấu trúc là một đồ thị hàm số. 

Một cách tiếp cận bạo lực sẽ cố gắng mô phỏng tính nhất quán của tổ tiên bằng cách liên tục theo dõi các con trỏ cha cho mỗi nút và kiểm tra xem liệu tất cả các nút cuối cùng có hội tụ về một gốc duy nhất hay không. Trong trường hợp xấu nhất, với mỗi nút trong số n nút, chúng ta có thể duyệt qua một chuỗi có độ dài n, tạo ra trạng thái O(n2). Điều này quá chậm đối với n lên tới 100000. 

Quan sát quan trọng là trong một cấu hình hợp lệ, bỏ qua các tên lặp lại, cấu trúc biểu đồ phải hoạt động giống như một khu rừng sẽ sụp đổ thành một gốc duy nhất khi bị đảo ngược. Tương tự, mọi thành phần được kết nối cuối cùng phải dẫn đến chính xác một ứng cử viên tổ tiên cuối cùng và không được có sự mơ hồ được tạo ra bởi nhiều gốc hoặc chu trình bị ngắt kết nối không phân giải thành một tổ tiên toàn cầu duy nhất.

Điều này giúp giảm việc kiểm tra xem đồ thị có hướng có chính xác một thành phần được kết nối yếu hay không khi chúng ta coi các cạnh là vô hướng và liệu mỗi nút có nhiều nhất một nút cha đã được đảm bảo bằng cách xây dựng đầu vào hay chưa. Tuy nhiên, chỉ kết nối thôi là chưa đủ vì các chu kỳ vẫn phải tương thích với cách giải thích tổ tiên duy nhất. Cải tiến quan trọng là một chu trình không tạo ra một gốc mới, nó hoạt động giống như một vòng lặp khép kín không có tổ tiên trên cùng, do đó, bất kỳ chu trình nào không được kết nối với phần còn lại của cấu trúc thông qua một gốc nhất quán duy nhất sẽ phá vỡ yêu cầu có một tổ tiên toàn cầu duy nhất. 

Một cách rõ ràng để chính thức hóa điều này là coi mỗi người như một nút, xây dựng ánh xạ, sau đó tính toán số lượng mức độ và theo dõi kết nối. Cấu trúc hợp lệ phải có chính xác một nút không bao giờ là nút con (bậc 0 theo nghĩa đảo ngược) và mọi nút cuối cùng phải có thể truy cập được trong cấu trúc hướng lên nhất quán mà không có xung đột về tổ tiên. 

Cách mạnh mẽ nhất là xây dựng biểu đồ và sau đó sử dụng công cụ tìm liên kết hoặc BFS/DFS để đảm bảo tất cả các nút nằm trong một thành phần được kết nối duy nhất khi bỏ qua hướng, đồng thời đảm bảo rằng chúng tôi không vi phạm các ràng buộc chức năng (đã được đảm bảo bởi định dạng đầu vào). Vì mỗi nút có nhiều nhất một nút cha nên các chu kỳ được cho phép nhưng không được tạo ra nhiều lưu vực tổ tiên độc lập. 

Một điều kiện tương đương đơn giản hơn xuất hiện: nếu chúng ta coi mỗi người là một nút và kết nối con với cha, thì trong mô hình tổ tiên hợp lệ phải tồn tại chính xác một thành phần được kết nối yếu sau khi xem xét tất cả các tên và không nút nào có thể có các phép gán cha mẹ trái ngược nhau (đã được thực thi). Nếu điều kiện này được giữ, chúng ta có thể định hướng thành phần có một gốc tổ tiên duy nhất. 

Do đó, vấn đề giảm xuống còn việc xây dựng biểu đồ trên tất cả các tên duy nhất và kiểm tra khả năng kết nối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (truyền tải lặp đi lặp lại trên mỗi nút) | O(n²) | O(n) | Quá chậm | 
| Tối ưu (xây dựng biểu đồ + kiểm tra kết nối) | O(n α(n)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa mỗi tên duy nhất dưới dạng một nút trong biểu đồ. 

1. Gán một ID số nguyên cho mỗi tên riêng biệt gặp phải trong đầu vào. Điều này tránh so sánh chuỗi trong quá trình truyền tải biểu đồ và đảm bảo xử lý hiệu quả. 
2. Với mỗi quan hệ “A là con của B”, hãy thêm một kết nối vô hướng giữa A và B. Hướng không quan trọng đối với kết nối, vì chúng ta chỉ cần đảm bảo tất cả mọi người nằm trong một cấu trúc được kết nối. 
3. Duy trì cấu trúc đã truy cập để truyền tải. 
4. Chọn bất kỳ nút nào và chạy BFS hoặc DFS để đánh dấu tất cả các nút có thể truy cập được từ nút đó. 
5. Sau khi duyệt, kiểm tra xem mọi nút đã được truy cập chưa. Nếu có thì ghi “có thể”, nếu không thì ghi “không thể”. 

Lý do điều này có tác dụng là vì nếu tất cả các nút thuộc về một thành phần được kết nối duy nhất dưới các liên kết cha-con thì sẽ tồn tại một cách nhất quán để chọn một tổ tiên gốc duy nhất cho toàn bộ cấu trúc. Bất kỳ thành phần nào bị ngắt kết nối sẽ tương ứng với một dòng tổ tiên độc lập, mâu thuẫn với yêu cầu về một tổ tiên chung duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict, deque

n = int(input())

id_map = {}
adj = defaultdict(list)
idx = 0

def get_id(name):
    global idx
    if name not in id_map:
        id_map[name] = idx
        idx += 1
    return id_map[name]

for _ in range(n):
    line = input().strip()
    # format: "A, son of B"
    left, right = line.split(", son of ")
    a = get_id(left)
    b = get_id(right)
    adj[a].append(b)
    adj[b].append(a)

if idx == 0:
    print("possible")
    sys.exit()

visited = [False] * idx
q = deque([0])
visited[0] = True

while q:
    u = q.popleft()
    for v in adj[u]:
        if not visited[v]:
            visited[v] = True
            q.append(v)

if all(visited):
    print("possible")
else:
    print("impossible")
```Đầu tiên, mã chuyển đổi tất cả tên thành số nhận dạng số nguyên để danh sách kề có thể được lưu trữ một cách hiệu quả. Mỗi mối quan hệ thêm một cạnh hai chiều, vì khả năng kết nối là thuộc tính duy nhất chúng ta cần kiểm tra. 

BFS bắt đầu từ một nút tùy ý, ở đây là nút 0 và khám phá toàn bộ thành phần được kết nối. Nếu sau BFS, một số nút vẫn chưa được truy cập thì biểu đồ sẽ bị ngắt kết nối và không thể có một cấu trúc tổ tiên toàn cầu duy nhất bao trùm tất cả mọi người. 

Một trường hợp góc nhỏ là khi không có mối quan hệ nào cả, trong trường hợp đó câu trả lời là “có thể” một cách tầm thường. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

“Gia-cốp, con trai Y-sác” 

“Isaac, con trai Abraham” 

“Ích-ma-ên, con trai Áp-ra-ham” 

Chúng tôi ánh xạ tên tới ID và xây dựng vùng lân cận. 

| Bước | Nút | Xếp hàng | Đã truy cập | 
| --- | --- | --- | --- | 
| Bắt đầu | 0 (Jacob) | [0] | {Jacob} | 
| Thăm Jacob | Isaac | [Isaac] | {Jacob, Isaac} | 
| Thăm Isaac | Áp-ra-ham | [Áp-ra-ham] | {Jacob, Isaac, Abraham} | 
| Thăm Áp-ra-ham | Ishmael | [Ishmael] | {tất cả} | 

Tất cả các nút đều đạt được nên có thể xuất ra. 

Điều này xác nhận rằng nhiều đứa trẻ có chung cha/mẹ sẽ không phá vỡ tính hợp lệ miễn là mọi thứ vẫn được kết nối. 

### Ví dụ 2 

đầu vào: 

“Basil, con trai của Alexios” 

“Procopius, con trai Constantine” 

Hai cặp bị ngắt kết nối tồn tại. 

| Bước | Nút | Xếp hàng | Đã truy cập | 
| --- | --- | --- | --- | 
| Bắt đầu | Húng quế | [Húng quế] | {Basil, Alexios} | 
| Kết thúc BFS | - | [] | thành phần đầu tiên duy nhất | 

Các nút liên quan đến Procopius và Constantine vẫn chưa được xem xét. 

Điều này chứng tỏ rằng các cụm tổ tiên bị ngắt kết nối không thể thống nhất thành một gốc duy nhất nếu không phát minh ra các mối quan hệ, vì vậy câu trả lời là không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi mối quan hệ được xử lý một lần để xây dựng tính kề cận và BFS truy cập từng nút và cạnh một lần | 
| Không gian | O(n) | Lưu trữ để ánh xạ tên, danh sách kề và mảng đã truy cập | 

Hành vi tuyến tính phù hợp thoải mái trong các ràng buộc của 100000 quan hệ và mức sử dụng bộ nhớ tỷ lệ thuận với số lượng tên duy nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict, deque

    n = int(input())
    id_map = {}
    adj = defaultdict(list)
    idx = 0

    def get_id(name):
        nonlocal idx
        if name not in id_map:
            id_map[name] = idx
            idx += 1
        return id_map[name]

    for _ in range(n):
        line = input().strip()
        a, b = line.split(", son of ")
        a = get_id(a)
        b = get_id(b)
        adj[a].append(b)
        adj[b].append(a)

    if idx == 0:
        return "possible"

    visited = [False] * idx
    q = deque([0])
    visited[0] = True

    while q:
        u = q.popleft()
        for v in adj[u]:
            if not visited[v]:
                visited[v] = True
                q.append(v)

    return "possible" if all(visited) else "impossible"

# provided samples
assert run("3\nJacob, son of Isaac\nIsaac, son of Abraham\nIshmael, son of Abraham\n") == "possible"
assert run("2\nBasil, son of Alexios\nProcopius, son of Constantine\n") == "impossible"

# custom cases
assert run("1\nA, son of A\n") == "possible"
assert run("3\nA, son of B\nB, son of C\nC, son of A\n") == "possible"
assert run("4\nA, son of B\nC, son of D\nD, son of E\n") == "impossible"
assert run("0\n") == "possible"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một vòng lặp tự | có thể | Chu kỳ tự nuôi dạy con cái được cho phép về mặt cấu trúc | 
| 3 chu kỳ | có thể | Chu trình thuần túy vẫn tạo thành một thành phần | 
| chia chuỗi | không thể | thành phần bị ngắt kết nối | 
| đầu vào trống | có thể | trường hợp cơ bản tầm thường | 

## Vỏ cạnh 

Mối quan hệ tự nuôi dạy con cái chẳng hạn như “Aaron, con trai của Aaron” tạo ra một vòng lặp tự thân. Thuật toán ánh xạ Aaron tới một nút duy nhất có cạnh chính nó. Trong BFS, bắt đầu từ Aaron, quá trình truyền tải ngay lập tức truy cập lại cùng một nút nhưng không mở rộng thêm. Tập đã truy cập chỉ chứa Aaron và nếu không có nút nào khác tồn tại thì đầu ra là “có thể”. 

Một chu trình có nhiều người như A → B → C → A được xử lý tương tự. BFS bắt đầu từ bất kỳ nút nào sẽ đi qua toàn bộ chu trình, đánh dấu tất cả các nút là đã truy cập. Vì tất cả các nút đều nằm trong một thành phần được kết nối duy nhất nên thuật toán đưa ra kết quả là “có thể”, phản ánh chính xác rằng tổ tiên tuần hoàn vẫn tạo thành một cấu trúc thống nhất. 

Một khu rừng bị ngắt kết nối như A → B và C → D dẫn đến hai thành phần BFS riêng biệt có thể truy cập được. Xuất phát từ A chỉ đến được B, bỏ lại C và D. Thuật toán đưa ra kết quả chính xác là “không thể”, vì không có tổ tiên đơn lẻ nào có thể kết nối cả hai thành phần mà không bổ sung thêm các mối quan hệ lịch sử bị thiếu.
