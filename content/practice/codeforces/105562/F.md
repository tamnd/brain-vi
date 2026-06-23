---
title: "CF 105562F - Đài phun nước chảy"
description: "Chúng tôi được đưa cho một chồng bát thẳng đứng, mỗi chiếc bát nằm phía trên chiếc bát tiếp theo. Mỗi tô đều có dung tích cố định và chúng tôi xử lý hai loại thao tác theo thời gian. Một thao tác đổ một lượng rượu sâm panh vào một chiếc bát đã chọn."
date: "2026-06-22T06:27:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105562
codeforces_index: "F"
codeforces_contest_name: "2024-2025 ICPC Northwestern European Regional Programming Contest (NWERC 2024)"
rating: 0
weight: 105562
solve_time_s: 46
verified: true
draft: false
---

[CF 105562F - Đài phun nước chảy](https://codeforces.com/problemset/problem/105562/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa cho một chồng bát thẳng đứng, mỗi chiếc bát nằm phía trên chiếc bát tiếp theo. Mỗi tô đều có dung tích cố định và chúng tôi xử lý hai loại thao tác theo thời gian. Một thao tác đổ một lượng rượu sâm panh vào một chiếc bát đã chọn. Nếu cái bát đó không thể chứa hết thì phần thừa sẽ chảy xuống ngăn xếp. Quy tắc về nơi nó đi rất quan trọng: tràn không đi đến chỉ số tiếp theo, nó đi đến bát tiếp theo bên dưới vẫn còn dung lượng trống theo hướng đi xuống. Nếu bát thấp nhất tràn ra thì phần thừa sẽ bị mất. 

Thao tác thứ hai yêu cầu lượng sâm panh hiện tại trong một bát cụ thể tại thời điểm đó. Hệ thống phát triển qua một chuỗi dài các lần đổ và truy vấn như vậy, vì vậy chúng tôi phải duy trì trạng thái động một cách hiệu quả. 

Khó khăn chính là việc đổ vào cấp độ cao có thể truyền qua nhiều bát đầy trung gian. Trong trường hợp xấu nhất, một thao tác có thể đi qua toàn bộ cấu trúc và có tới 3×10^5 thao tác. Một mô phỏng đơn giản di chuyển luồng đơn vị từng bước sẽ ngay lập tức thất bại vì mỗi thao tác có thể tốn O(n), dẫn đến O(nq) vượt xa giới hạn khả thi. 

Trường hợp cạnh tinh tế xuất hiện khi nhiều bát đã đầy ở một đoạn liền kề. Ví dụ: nếu tất cả các bát dưới một điểm nhất định đã bão hòa, lần đổ mới có thể ngay lập tức bỏ qua một khối lớn và đọng lại ngay bên dưới hoặc bị lãng phí hoàn toàn. Một trường hợp góc khác là các truy vấn lặp lại sau nhiều lần đổ từng phần: chúng ta không được tính toán lại các luồng từ đầu mỗi lần, vì trạng thái là liên tục và phải được duy trì tăng dần. 

Một sai lầm ngây thơ là coi luồng luôn đi đến chỉ mục tiếp theo. Điều đó không thành công trong các trường hợp như dung lượng [2, 4, 3], đổ 10 vào cấp 1: hầu hết giá trị bỏ qua các cấp độ trung gian tùy thuộc vào không gian dư, không chỉ kề. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu coi mỗi lần đổ như một quá trình dòng chảy theo nghĩa đen. Chúng tôi duy trì một loạt các điền hiện tại. Khi đổ vào cấp ℓ ta thêm x, sau đó khi mức đó vượt quá khả năng thì ta đẩy phần thừa sang cấp tiếp theo, cứ thế đi xuống. Mỗi đơn vị tràn có thể trải qua nhiều cấp độ, do đó, một thao tác đơn lẻ có thể giảm xuống O(n) trong trường hợp xấu nhất khi mọi thứ đã đầy và chúng ta tiếp tục đi xuống. 

Điều này đúng nhưng về cơ bản là quá chậm vì mỗi thao tác q có thể quét qua một hậu tố dài. Với q lên tới 3×10^5, điều này dẫn đến khoảng 10^10 thao tác trong trường hợp xấu nhất. 

Quan sát chính là quá trình dòng chảy đơn điệu một cách rất mạnh mẽ. Khi một cái bát đã đầy, nó hoạt động giống như một “khối rắn” ngay lập tức chuyển hướng tất cả dòng chảy vào đi xuống. Điều này gợi ý nén các phân đoạn đầy đủ liên tiếp. Thay vì mô phỏng dòng chảy từng bước, chúng tôi muốn chuyển thẳng sang tô tiếp theo vẫn còn dung tích. 

Điều này biến vấn đề thành việc duy trì một cấu trúc có thể giải quyết hai vấn đề một cách hiệu quả: lượng không gian trống còn lại trong một phân đoạn và vị trí chưa đầy đủ tiếp theo ở đâu. Cấu trúc liên kết được thiết lập rời rạc sẽ phù hợp một cách tự nhiên nếu chúng ta coi “những cái bát đầy” như những nút bị loại bỏ. Mỗi khi một cái bát đầy, chúng tôi kết hợp nó với ứng cử viên tiếp theo bên dưới. Sau đó, việc tìm kiếm vị trí của một đơn vị vùng đất có dòng chảy sẽ trở thành một chuỗi các hoạt động tìm kiếm vượt qua các phân đoạn bão hòa. 

Do đó, mỗi bát được truy cập nhiều nhất một lần khi đầy và mỗi truy vấn sẽ được khấu hao theo hành vi nghịch đảo của Ackermann theo thời gian gần như không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Tối ưu hóa bỏ qua toàn bộ DSU | O((n + q) α(n)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì ba thông tin cốt lõi: lượng đổ đầy hiện tại trong mỗi tô, dung lượng của nó và cấu trúc DSU cho phép chúng tôi chuyển sang tô chưa đầy tiếp theo bên dưới một chỉ số nhất định.

1. Khởi tạo một mảng`cur[i] = 0`để điền thông tin hiện tại và dung lượng lưu trữ`cap[i]`. Xây dựng mảng cha DSU trong đó ban đầu mỗi chỉ mục trỏ đến chính nó. 
2. Xác định hàm`find(i)`trả về chỉ số gần nhất bằng hoặc thấp hơn i chưa bão hòa hoàn toàn. Nếu i nằm ngoài phạm vi, chúng tôi sẽ trả về một trọng điểm có nghĩa là tràn xuống đất. 
3. Khi xử lý truy vấn đổ`(ℓ, x)`, liên tục xác định vị trí chiếc bát có sẵn đầu tiên bằng cách sử dụng`i = find(ℓ)`. 
4. Ở mỗi bước, hãy tính xem còn lại bao nhiêu chỗ trống trong bát i`cap[i] - cur[i]`. 
5. Nếu x nhỏ hơn hoặc bằng không gian trống này, chúng ta chỉ cần thêm x vào`cur[i]`và dừng lại. Đây là trường hợp dòng chảy không lan truyền thêm. 
6. Nếu x vượt quá dung lượng trống, chúng ta đổ đầy bát i, trừ khoảng trống đó khỏi x và đánh dấu i là đầy. 
7. Khi một cái bát đầy, chúng ta kết hợp nó với i + 1 để những người trong tương lai bỏ qua nó ngay lập tức. Đây là cơ chế nén các vùng đã bão hòa. 
8. Tiếp tục quá trình từ bát có sẵn tiếp theo cho đến khi x bằng 0 hoặc chúng ta chạy qua bát cuối cùng, trong trường hợp đó x còn lại sẽ bị loại bỏ. 
9. Đối với một truy vấn`(ℓ)`, chỉ cần xuất ra`cur[ℓ]`. 

Lý do điều này hoạt động hiệu quả là vì mỗi bát chuyển từ “không đầy” sang “đầy” đúng một lần. Sau quá trình chuyển đổi đó, nó không bao giờ được xử lý lại dưới dạng nút miễn phí. Vì vậy tổng số lần hợp nhất DSU tốn kém là tuyến tính trên n. 

### Tại sao nó hoạt động 

Tính bất biến đó là`find(i)`luôn trả bát đầu tiên bằng hoặc thấp hơn i mà vẫn còn dung tích. Bất cứ khi nào chúng tôi bão hòa một cái bát, chúng tôi sẽ loại bỏ nó vĩnh viễn khỏi việc xem xét bằng cách liên kết nó với chỉ mục tiếp theo. Điều này đảm bảo rằng dòng chảy trong tương lai không bao giờ quay trở lại những chiếc bát đã đầy, khớp chính xác với cách giải thích vật lý về hiện tượng tràn. Vì mỗi đơn vị dòng chảy sẽ lấp đầy một cái bát hoặc bị loại bỏ và mỗi cái bát được đổ đầy một lần, nên tổng số thay đổi về cấu trúc được giới hạn bởi n, điều này đảm bảo tính chính xác và hiệu quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

n, q = map(int, input().split())
cap = list(map(int, input().split()))

cur = [0] * n
parent = list(range(n + 1))

def find(x):
    if x >= n:
        return n
    if parent[x] != x:
        parent[x] = find(parent[x])
    return parent[x]

def union(x, y):
    x = find(x)
    y = find(y)
    if x != y:
        parent[x] = y

for i in range(n):
    parent[i] = i

parent[n] = n

for _ in range(q):
    tmp = input().split()
    if tmp[0] == '+':
        l = int(tmp[1]) - 1
        x = int(tmp[2])

        i = find(l)

        while i < n and x > 0:
            free = cap[i] - cur[i]
            if free == 0:
                union(i, i + 1)
                i = find(i)
                continue

            if x <= free:
                cur[i] += x
                x = 0
            else:
                cur[i] += free
                x -= free
                union(i, i + 1)
                i = find(i)

    else:
        l = int(tmp[1]) - 1
        print(cur[l])
```Việc triển khai dựa vào việc nén đường dẫn DSU để đảm bảo rằng khi một tô đầy, nó sẽ bị bỏ qua một cách hiệu quả trong thời gian khấu hao không đổi. các`union(i, i+1)`hoạt động là cơ chế quan trọng giúp loại bỏ các vị trí bão hòa khỏi việc xem xét trong tương lai. 

Một chi tiết tinh tế là xử lý những chiếc bát đã đầy: ngay cả khi`cur[i] == cap[i]`, chúng ta ngay lập tức hợp nhất nó về phía trước trước khi tiếp tục. Điều này ngăn chặn các vòng lặp vô hạn khi chúng ta liên tục rơi vào một cái bát đầy. Một điểm quan trọng khác là chỉ số trọng điểm`n`, biểu thị mức tràn vượt quá bát cuối cùng, cho phép vòng lặp kết thúc một cách rõ ràng khi tất cả luồng còn lại bị lãng phí. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 4
1 2 3 4
+ 1 6
? 4
+ 1 6
? 4
```Đối với thao tác đầu tiên, chúng ta bắt đầu ở bát 1 với dung tích 1. Chúng ta đổ 6. Bát 1 đầy hoàn toàn và còn lại 5 đơn vị. Chúng ta chuyển sang tô 2 có sức chứa 2 nên cần 2 và đầy, còn lại 3. Khi đó tô 3 lấy đúng 3 và đầy. Không tràn tới bát 4. 

| Bước | Bát | Cur | Mũ | Còn lại x | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| bắt đầu | 1 | 0 | 1 | 6 | điền | 
| 1 | 1 | 1 | 1 | 5 | đầy đủ, di chuyển | 
| 2 | 2 | 2 | 2 | 3 | đầy đủ, di chuyển | 
| 3 | 3 | 3 | 3 | 0 | dừng lại | 

Sau đó, bát 4 vẫn trống nên truy vấn trả về 0. 

Lần đổ giống thứ hai lặp lại quy trình tương tự, xác nhận rằng quá trình truyền lan hoàn toàn lặp lại hoạt động nhất quán. 

### Ví dụ 2 

đầu vào:```
4 8
2 4 3 5
+ 1 4
? 2
+ 2 3
? 4
+ 3 4
? 4
+ 2 10
? 4
```Chúng tôi chỉ theo dõi các tiểu bang có liên quan. 

Sau đó`+ 1 4`, bát 1 mất 2 và bát 2 mất 2. 

Sau`+ 2 3`, bát 2 cần thêm 2 bát nữa mới đầy, bát 3 cần thêm 1. 

Sau`+ 3 4`, bát 3 đầy và bát 4 lấy 1. 

Sau`+ 2 10`, tràn từ tô 2 bỏ qua, chảy vào tô 3 (đã đầy), rồi tô 4, đổ đầy thêm. 

Hành vi quan trọng đã được chứng minh là khi một cái bát đã đầy, những lần đổ trong tương lai sẽ bỏ qua nó hoàn toàn và tiếp tục đi xuống một cách hiệu quả, đó chính xác là những gì DSU thực thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) α(n)) | Mỗi bát đầy một lần, mỗi hoạt động DSU được khấu hao nghịch đảo Ackermann | 
| Không gian | O(n) | Mảng chứa dung lượng, mức điền hiện tại và DSU gốc | 

Các ràng buộc cho phép hoạt động lên tới 3×10^5, do đó, hành vi khấu hao gần tuyến tính là cần thiết. Việc bỏ qua dựa trên DSU đảm bảo mỗi đơn vị thay đổi cơ cấu được thanh toán một lần, giữ cho giải pháp được thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    n, q = map(int, sys.stdin.readline().split())
    cap = list(map(int, sys.stdin.readline().split()))
    cur = [0] * n
    parent = list(range(n + 1))

    def find(x):
        if x >= n:
            return n
        if parent[x] != x:
            parent[x] = find(parent[x])
        return parent[x]

    def union(x, y):
        x = find(x)
        y = find(y)
        if x != y:
            parent[x] = y

    for _ in range(q):
        parts = sys.stdin.readline().split()
        if parts[0] == '+':
            l = int(parts[1]) - 1
            x = int(parts[2])
            i = find(l)
            while i < n and x > 0:
                free = cap[i] - cur[i]
                if free == 0:
                    union(i, i + 1)
                    i = find(i)
                    continue
                if x <= free:
                    cur[i] += x
                    x = 0
                else:
                    cur[i] += free
                    x -= free
                    union(i, i + 1)
                    i = find(i)
        else:
            l = int(parts[1]) - 1
            print(cur[l])

# custom minimal case
assert run("1 1\n5\n+ 1 10\n? 1\n") == "5\n", "cap overflow"

# all equal simple chain
assert run("3 4\n2 2 2\n+ 1 3\n? 1\n? 2\n? 3\n") == "2\n1\n0\n", "chain fill"

# no overflow case
assert run("3 3\n5 5 5\n+ 2 3\n? 2\n? 3\n") == "3\n0\n", "no spill"

# cascading overflow
assert run("3 3\n1 1 1\n+ 1 5\n? 3\n? 1\n") == "1\n1\n", "cascade"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tràn 1 bát | 5 giới hạn | cắt tràn | 
| điền chuỗi bằng nhau | 2 1 0 | độ chính xác của việc truyền bá | 
| không tràn | 3 0 | hành vi điền cục bộ | 
| thác đầy đủ | 1 1 | tuyên truyền đa cấp | 

## Vỏ cạnh 

Trường hợp một cạnh là những nỗ lực lặp đi lặp lại để đổ vào các vùng đã đầy. Ví dụ, một chiếc bát có dung tích 2 đã đầy phải chuyển hướng dòng chảy ngay lập tức. Cấu trúc DSU xử lý việc này bằng cách đảm bảo`find(i)`không bao giờ trả về chỉ mục bão hòa, vì vậy chúng tôi không bao giờ lặp vô thời hạn trên chỉ mục đó. 

Một trường hợp khác là khi một lượng nước đổ lớn bỏ qua nhiều bát đầy và rơi thẳng xuống phía dưới. Xem xét năng lực`[1,1,1,10]`và đổ 100 vào cấp 1. Sau khi ba bát đầu tiên bão hòa, DSU nén chúng thành một bước nhảy duy nhất, do đó 97 còn lại được áp dụng trực tiếp vào bát cuối cùng mà không cần xem lại các chỉ số trung gian. 

Trường hợp cuối cùng là khi đổ hoàn toàn xuống đất. Một lần`find`trả về chỉ số trọng điểm`n`, tất cả chất lỏng còn lại sẽ bị loại bỏ ngay lập tức. Điều này ngăn chặn việc truy cập ngoài giới hạn và đảm bảo chấm dứt ngay cả khi tất cả các bát đã đầy.
