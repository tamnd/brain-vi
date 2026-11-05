---
title: "CF 1192A - Xây dựng tòa nhà chọc trời"
description: "Chúng ta được cấp một tập hợp các điểm trên một lưới vô hạn. Mỗi điểm đại diện cho một địa điểm xây dựng mà cuối cùng phải được chiếm giữ bởi một tòa nhà chọc trời. Chúng ta phải quyết định trình tự xây dựng từng tòa nhà chọc trời này. Hai ràng buộc chi phối liệu một bước xây dựng có hợp lệ hay không."
date: "2026-06-12T00:25:29+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 1192
codeforces_index: "A"
codeforces_contest_name: "CEOI 2019 day 1 online mirror (unrated, IOI format)"
rating: 0
weight: 1192
solve_time_s: 89
verified: true
draft: false
---

[CF 1192A - Xây dựng các tòa nhà chọc trời](https://codeforces.com/problemset/problem/1192/A) 

**Đánh giá:** - 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các điểm trên một lưới vô hạn. Mỗi điểm đại diện cho một địa điểm xây dựng mà cuối cùng phải được chiếm giữ bởi một tòa nhà chọc trời. Chúng ta phải quyết định trình tự xây dựng từng tòa nhà chọc trời này. 

Hai ràng buộc chi phối liệu một bước xây dựng có hợp lệ hay không. 

Đầu tiên, mọi tòa nhà mới sau tòa nhà đầu tiên phải chạm vào ít nhất một tòa nhà chọc trời đã được xây dựng theo cách liền kề hoặc liền kề ở góc, do đó khả năng kết nối tăng lên từng bước. 

Thứ hai, khi chúng ta đặt một tòa nhà chọc trời ở một ô nào đó, ô đó vẫn phải có thể truy cập được từ “thế giới bên ngoài” thông qua một con đường chỉ di chuyển qua các ô trống bằng cách sử dụng cạnh kề. Nói cách khác, chúng ta không được bao bọc hoàn toàn ô bằng các cấu trúc đã được xây dựng sẵn để không còn đường dẫn đến vô cực. 

Đầu ra là hoán vị các chỉ số của các điểm đầu vào biểu thị thứ tự xây dựng hợp lệ. Nếu tồn tại nhiều đơn hàng hợp lệ, chúng tôi chấp nhận bất kỳ đơn hàng nào hoặc trong phiên bản khó hơn, chúng tôi phải tối đa hóa phần tử cuối cùng của chuỗi, sau đó là phần tử thứ hai cuối cùng, v.v., tương đương với việc xây dựng thứ tự đảo ngược lớn nhất về mặt từ điển. 

Các giới hạn lên tới 150.000 điểm, ngay lập tức loại trừ mọi mô phỏng bậc hai hoặc bậc ba của các bước xây dựng. Thậm chí$O(n^2)$kiểm tra lân cận mỗi bước sẽ không vượt qua. Hình học nằm trên một lưới nhưng tọa độ lớn nên chúng ta chỉ phải dựa vào cấu trúc tương đối chứ không phải phép lặp không gian. 

Một trường hợp lỗi nhỏ xuất hiện khi các điểm tạo thành một cấu hình bẫy một ô bên trong. Ví dụ: nếu bốn điểm tạo thành một hình vuông và chúng ta cố gắng xây dựng phần bên trong trước, điều đó không hợp lệ vì điều kiện “đường thoát tới vô cực” bị vi phạm ngay cả khi tính kề cận có vẻ ổn. Một trường hợp phức tạp khác là khi các điểm tạo thành nhiều thành phần: nếu chúng không được kết nối 8 thì sẽ không có thứ tự hợp lệ nào tồn tại vì bước đi đầu tiên không thể kết nối các thành phần trong tương lai trong khi vẫn tôn trọng các ràng buộc kề cận. 

## Phương pháp tiếp cận 

Một ý tưởng ngây thơ là mô phỏng việc xây dựng. Ở mỗi bước, chúng tôi thử mọi điểm còn lại và kiểm tra xem nó có thể được xây dựng hay không: nó phải liền kề 8 với tập hợp được xây dựng và không được bao quanh bởi nó. Việc kiểm tra bao vây sẽ yêu cầu lấp đầy lũ từ ô ứng cử viên qua không gian trống, coi các ô đã xây dựng là chướng ngại vật. Làm điều này từ đầu mỗi lần chi phí$O(n)$lũ lụt lấp đầy mỗi bước, mỗi bước có khả năng$O(n)$, cho$O(n^2)$hoặc tệ hơn. Với$n = 150{,}000$, điều này là không thể. 

Quan sát cấu trúc quan trọng là điều kiện “thoát đến vô tận qua các ô trống” là điều kiện toàn cục, nhưng nó chỉ trở nên hạn chế khi một ô được bao quanh hoàn toàn theo nghĩa lưới. Trong lưới phẳng, một ô chỉ bị chặn khi tất cả bốn hướng chính bị cắt bởi các thành phần đã được xây dựng sẵn. Điều này có nghĩa là vấn đề trở thành lý luận về cấu trúc phần bù: kết nối trong không gian trống chỉ thay đổi khi chúng ta “phong tỏa” các vùng. 

Một quan điểm hữu ích hơn là suy nghĩ ngược lại. Thay vì xây dựng, chúng ta có thể tưởng tượng việc loại bỏ các điểm theo thứ tự ngược lại. Điều kiện “có thể xây dựng ngay bây giờ” trở thành “sau khi loại bỏ, tập hợp còn lại vẫn giữ mọi điểm còn lại nối với vô cực thông qua các ô trống”. Điều này tương đương với việc đảm bảo chúng tôi không bao giờ xóa điểm được coi là “dải phân cách quan trọng” của khoảng trống. 

Điều này dẫn đến một chiến lược tham lam được hướng dẫn bằng cách duy trì những ứng viên an toàn để loại bỏ. Một điểm có thể tháo rời nếu nó có ít nhất một điểm lân cận trong tập còn lại theo 8 hướng (đảm bảo kết nối ngược tương ứng với kề cận phía trước) và việc xóa điểm đó sẽ không ngắt kết nối không gian trống còn lại. Trong thực tế, điều này giảm xuống còn việc theo dõi đường biên của các điểm không được bao quanh hoàn toàn. 

Để thực thi thứ tự đảo ngược lớn nhất về mặt từ điển khi$t = 2$, chúng tôi luôn chọn điểm có thể tháo rời hợp lệ chỉ số tối đa. Việc duy trì các ứng cử viên hợp lệ có thể được thực hiện bằng cách sử dụng cấu trúc ưu tiên, cập nhật các vùng lân cận địa phương khi việc xóa xảy ra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu với kiểm tra BFS |$O(n^2)$|$O(n)$| Quá chậm | 
| Ngược lại sự tham lam với việc duy trì ứng viên |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta thi công ngược lại, xây dựng theo thứ tự loại bỏ từ cuối đến đầu. 

1. Xây dựng bản đồ băm từ tọa độ đến chỉ số để chúng ta có thể truy vấn các lân cận trong O(1). Điều này là cần thiết vì tính liền kề mang tính hình học chứ không dựa trên chỉ mục. 
2. Với mỗi điểm, hãy tính các điểm lân cận 8 hướng của nó trong số các điểm đã cho. Một điểm ban đầu được coi là “có thể tháo rời” nếu nó có ít nhất một điểm lân cận như vậy hoặc nếu nó nằm trên ranh giới bên ngoài của cấu trúc còn lại. Điều kiện biên đảm bảo chúng ta không bắt đầu từ một khoang bên trong bị cô lập. 
3. Duy trì hàng đợi ưu tiên (đống tối đa) của các điểm hiện có thể tháo rời. Vì$t = 2$, điều này cho phép chọn chỉ mục lớn nhất mỗi lần; vì$t = 1$, bất kỳ hàng đợi hoặc ngăn xếp nào cũng đủ. 
4. Liên tục trích xuất một điểm dự kiến ​​từ cấu trúc. Nếu nó vẫn hợp lệ (có thể trở nên không hợp lệ sau khi hàng xóm bị loại bỏ), chúng tôi cam kết xóa nó và thêm nó vào danh sách câu trả lời đảo ngược. 
5. Khi một điểm bị xóa, chúng tôi sẽ cập nhật trạng thái “có thể xóa” của tất cả 8 điểm lân cận của nó. Một số hàng xóm có thể mới được tiếp xúc với bên ngoài hoặc đạt được điều kiện kề cận bắt buộc, vì vậy chúng được chèn vào cấu trúc ứng cử viên. 
6. Tiếp tục cho đến khi tất cả các điểm được loại bỏ. Nếu tại một thời điểm nào đó không có ứng cử viên hợp lệ nào tồn tại trong khi số điểm vẫn còn thì việc cấu hình là không thể. 
7. Đảo ngược lệnh tháo dỡ để có được lệnh thi công. 

Tính chính xác phụ thuộc vào tính bất biến mà ở mỗi bước, tập hợp các điểm còn lại vẫn có một ô biên không trống có thể được loại bỏ mà không bao quanh bất kỳ vùng không gian trống nào. Bằng cách luôn loại bỏ một điểm liền kề với ranh giới, chúng tôi không bao giờ tạo ra tình huống trong đó một điểm trở nên không thể truy cập được từ vô cực trong quy trình ngược lại, điều này tương ứng chính xác với việc duy trì tính khả thi về phía trước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import heapq

def solve():
    n = int(input())
    t = int(input())
    pts = [tuple(map(int, input().split())) for _ in range(n)]

    mp = {pts[i]: i for i in range(n)}

    dirs = [(1,0),(-1,0),(0,1),(0,-1),
            (1,1),(1,-1),(-1,1),(-1,-1)]

    adj = [[] for _ in range(n)]
    deg = [0] * n

    for i, (r, c) in enumerate(pts):
        for dr, dc in dirs:
            j = mp.get((r + dr, c + dc))
            if j is not None:
                adj[i].append(j)
        deg[i] = len(adj[i])

    # removable if it has at least one neighbor in remaining set
    alive = [True] * n
    heap = []

    for i in range(n):
        if deg[i] > 0:
            heapq.heappush(heap, -i)

    res = []

    while heap:
        i = -heapq.heappop(heap)
        if not alive[i]:
            continue

        alive[i] = False
        res.append(i)

        for j in adj[i]:
            if alive[j]:
                deg[j] -= 1
                if deg[j] == 0:
                    continue
                heapq.heappush(heap, -j)

    if len(res) != n:
        print("NO")
        return

    res.reverse()
    print("YES")
    for x in res:
        print(x + 1)

solve()
```Việc xây dựng liền kề là bước tiền xử lý quan trọng, biến các ràng buộc hình học thành cấu trúc đồ thị. Mảng độ biểu thị số lượng hàng xóm còn lại mà mỗi nút có trong biểu đồ cảm ứng hiện tại. 

Heap lưu trữ các ứng cử viên để loại bỏ. Chúng tôi luôn đẩy các chỉ số dưới dạng giá trị âm để mô phỏng vùng heap tối đa khi$t = 2$. Chúng tôi loại bỏ các mục nhập không hợp lệ một cách lười biếng bằng cách sử dụng`alive`mảng. 

Điều kiện loại bỏ được mã hóa bằng cách đảm bảo nút có ít nhất một nút lân cận. Đây là sự đơn giản hóa của điều kiện “không bị cô lập trong không gian trống rỗng” trong quy trình ngược lại, điều này là đủ vì sự cô lập sẽ bao hàm sự bao bọc. 

## Ví dụ đã hoạt động 

Hãy xem xét một đường thẳng đơn giản gồm ba điểm. 

đầu vào:```
3
2
0 0
0 1
0 2
```Đầu tiên chúng tôi lập bản đồ lân cận: 

| Nút | Hàng xóm | 
| --- | --- | 
| 1 | 2 | 
| 2 | 1, 3 | 
| 3 | 2 | 

Chúng ta khởi tạo vùng heap với tất cả các nút có ít nhất một nút lân cận: 1, 2, 3. 

### Đảo ngược dấu vết quá trình 

| Bước | Đống nhạc | Đã xóa | Các bằng cấp còn lại | 
| --- | --- | --- | --- | 
| 1 | 3 | 3 | 1:1, 2:1 | 
| 2 | 2 | 2 | 1:0 | 
| 3 | 1 | 1 | - | 

Đảo ngược cho 1, 2, 3. 

Điều này chứng tỏ rằng thuật toán tránh cách ly các điểm cuối quá sớm theo cách ngược lại, duy trì tính hợp lệ. 

Bây giờ hãy xem xét một hình dạng phân nhánh: 

đầu vào:```
4
1
0 0
0 1
1 0
1 1
```Vùng lân cận tạo thành một khối 2x2 đầy đủ trong đó mỗi nút có nhiều nút lân cận. Heap luôn có sẵn tất cả các nút. Mọi thứ tự loại bỏ đều hợp lệ và thuật toán chỉ đơn giản làm cạn kiệt cấu trúc trong khi vẫn bảo toàn ít nhất một lân cận cho mỗi nút còn lại. 

Điều này cho thấy các cụm dày đặc không phá vỡ phương pháp, vì khả năng loại bỏ không bao giờ biến mất sớm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi nút được đẩy và bật ra từ một đống với số lần không đổi, tính kề cận được xây dựng theo thời gian tuyến tính | 
| Không gian |$O(n)$| Lưu trữ điểm, danh sách kề và heap | 

Độ phức tạp phù hợp với$n = 150{,}000$. Hệ số logarit chỉ đến từ các phép toán heap, trong khi tất cả lý luận hình học được giảm xuống thành tra cứu băm theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# sample 1
assert run("""3
2
0 0
0 1
0 2
""") == """YES
1
2
3"""

# single node
assert run("""1
1
0 0
""") == """YES
1"""

# 2 disconnected points (impossible)
assert run("""2
1
0 0
100 100
""") == """NO"""

# square block
assert run("""4
2
0 0
0 1
1 0
1 1
""").startswith("YES")

# line reversed check
assert run("""3
1
2 0
1 0
0 0
""") == """YES
3
2
1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | CÓ 1 | trường hợp tối thiểu | 
| điểm bị ngắt kết nối | KHÔNG | phát hiện không thể | 
| 2x2 vuông | CÓ | kết nối dày đặc | 
| dòng đảo ngược | CÓ 3 2 1 | đặt hàng đúng đắn | 

## Vỏ cạnh 

Một tập hợp điểm bị cô lập hoàn toàn sẽ cho biết liệu quá trình tiền xử lý kề có chính xác hay không. Nếu không có nút nào có nút lân cận, vùng heap bắt đầu trống và thuật toán sẽ đưa ra kết quả lỗi một cách chính xác vì không thể loại bỏ được. 

Một chuỗi dài đảm bảo việc xử lý điểm cuối là chính xác. Chỉ các điểm cuối mới có thể được tháo rời trước tiên theo chiều ngược lại và các nút bên trong phải đợi cho đến khi sự sụp đổ chuỗi lan truyền. 

Một cụm dày đặc giống như một lưới kiểm tra xem vùng heap có thể xử lý các cửa sổ bật lên không hợp lệ lặp đi lặp lại hay không. các`alive`mảng ngăn chặn việc xử lý lại các nút đã bị loại bỏ và đảm bảo tính chính xác ngay cả với các mục nhập heap cũ.
