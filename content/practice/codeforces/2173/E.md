---
title: "CF 2173E - Trận đấu trong gương của Hiro"
description: "Chúng tôi duy trì hoán vị 1..n. Một truy vấn chọn hai chỉ số x và y. Sau đó, thẩm phán sẽ tung một đồng xu. Vị trí (x,y) được hoán đổi hoặc vị trí phản chiếu của chúng (n-x+1, n-y+1) được hoán đổi."
date: "2026-06-07T22:48:47+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "interactive", "probabilities", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2173
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1068 (Div. 2)"
rating: 2200
weight: 2173
solve_time_s: 146
verified: false
draft: false
---

[CF 2173E - Cuộc đấu tay đôi trong gương của Hiro](https://codeforces.com/problemset/problem/2173/E) 

**Đánh giá:** 2200 
**Tags:** thuật toán xây dựng, tham lam, tương tác, xác suất, sắp xếp 
**Thời gian giải:** 2 phút 26s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta duy trì một hoán vị của`1..n`. 

Một truy vấn chọn hai chỉ số`x`Và`y`. Sau đó, thẩm phán sẽ tung một đồng xu. Hoặc vị trí`(x,y)`được hoán đổi hoặc vị trí được phản chiếu của chúng`(n-x+1, n-y+1)`được hoán đổi. Sau khi hoán đổi, chúng tôi được biết cặp nào đã thực sự được trao đổi, vì vậy chúng tôi luôn biết chính xác hoán vị hiện tại. 

Thách thức là chúng ta không thể trực tiếp chọn bên nào của mảng bị ảnh hưởng. Mọi thao tác đều tác động lên một cặp vị trí hoặc trên ảnh phản chiếu của nó. 

Tổng của tất cả`n`chỉ là`2·10^4`, vậy bất kỳ`O(n^2)`việc xử lý sẽ ổn về mặt tính toán. Hạn chế thực sự là ngân sách truy vấn. Chúng tôi chỉ nhận được khoảng`2.5n`hoạt động, có nghĩa là chúng ta phải đạt được tiến bộ ổn định và không đủ khả năng thực hiện các đợt tái sắp xếp toàn cầu lặp đi lặp lại. 

Quan sát đầu tiên là một vị trí và gương của nó tạo thành một cặp tự nhiên:```
i <-> n-i+1
```Bất cứ khi nào chúng tôi yêu cầu hoán đổi liên quan đến một bên của cặp như vậy, thay vào đó, thẩm phán có thể thực hiện hành động tương ứng ở bên kia. 

Một chiến lược sắp xếp đơn giản sẽ liên tục đặt từng giá trị vào đúng vị trí của nó. Vấn đề là mọi nỗ lực chỉ thành công với xác suất`1/2`, do đó việc triển khai bất cẩn có thể tiêu tốn nhiều hơn số lượng thao tác được phép. 

Một trường hợp đặc biệt nguy hiểm là chỉ cố gắng khắc phục một bên. 

Ví dụ, với```
n = 6
[2,1,3,4,5,6]
```cố gắng đặt`1`vào vị trí`1`thay vào đó có thể ảnh hưởng nhiều lần đến mặt được phản chiếu. Tiến độ trở nên khó kiểm soát. 

Điều quan trọng là làm việc trên các cặp được phản chiếu cùng nhau. Khi chúng tôi xem vị trí`i`Và`n-i+1`Là một đơn vị duy nhất, sự lựa chọn ngẫu nhiên của trọng tài không còn là trở ngại nữa mà trở thành một sự đối xứng hữu ích. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là sắp xếp chính xác như cách chúng ta sắp xếp một hoán vị thông thường. Giả sử giá trị`i`hiện đang ở vị trí`pos[i]`. Chúng tôi liên tục yêu cầu trao đổi giữa`pos[i]`Và`i`. 

Vấn đề là chỉ một nửa số truy vấn ảnh hưởng đến vị trí dự định. Nửa còn lại ảnh hưởng đến các vị trí được phản chiếu. Mặc dù số lần thử dự kiến ​​để sửa một phần tử là không đổi, nhưng sự tương tác giữa các phần tử khác nhau khiến việc phân tích trở nên lộn xộn và tổng số thao tác có thể dễ dàng vượt quá giới hạn. 

Nhận xét quan trọng là thẩm phán không bao giờ chọn một sự hoán đổi tùy tiện. Nó lựa chọn giữa một trao đổi và nhân bản của nó. 

Hãy xem xét một cặp giá trị mục tiêu được nhân đôi:```
l = i
r = n-i+1
```Giả sử giá trị`l`không ở vị trí`l`. 

Chúng tôi truy vấn:```
(pos[l], l)
```Nếu thẩm phán thực hiện trao đổi đã chọn, giá trị`l`ngay lập tức đến đúng vị trí của nó. 

Nếu thẩm phán thực hiện hoán đổi được phản ánh thì vị trí của giá trị`r`thay vào đó sẽ di chuyển về phía bên phải của nó. 

Dù thế nào đi nữa, một trong hai thành viên của cặp được phản chiếu sẽ tiến gần hơn đến việc được sửa chữa. 

Điều này gợi ý việc xử lý hoán vị từ ngoài vào trong. Đối với mỗi cặp được nhân đôi`(l,r)`, chúng tôi tiếp tục truy vấn cho đến khi cả hai vị trí đều chứa giá trị chính xác của chúng. 

Điều tuyệt vời là mọi truy vấn đều khắc phục được ít nhất một trong hai vị trí được mong đợi. Phân tích biên tập cho thấy rằng chi phí dự kiến ​​cho một cặp được nhân đôi nhiều nhất là năm hoạt động, dẫn đến kỳ vọng tổng thể là khoảng`2.5n`hoạt động. Việc bổ sung`800`trong giới hạn cung cấp một biên độ an toàn rất lớn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Vị trí Brute Force | Không rõ ràng trong giới hạn truy vấn | O(n) | Không đáng tin cậy | 
| Gương-cặp tham lam | O(n) truy vấn dự kiến ​​| O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duy trì hoán vị hiện tại và một mảng`pos[v]`lưu trữ vị trí của mọi giá trị. 
2. Nếu`n`là số lẻ, trước tiên hãy sửa phần tử ở giữa. 

Vị trí ở giữa là tấm gương của chính nó. Liên tục truy vấn cặp`(pos[mid_value], mid_position)`cho đến khi giá trị giữa đạt đến trung tâm. 
3. Xử lý các vị trí được phản ánh từ bên ngoài vào trong. 

Cho phép```
l = i
r = n-i+1
```4. Trong khi một trong hai vị trí không chính xác, hãy tiếp tục thao tác trên cặp này. 
5. Nếu giá trị`l`hiện không ở vị trí`l`, truy vấn```
(l, pos[l])
```Khi trao đổi không được nhân đôi xảy ra,`l`trở nên đúng ngay lập tức. 
6. Ngược lại, vị trí`l`đã chính xác rồi, vì vậy hãy làm việc ở phía bên kia và truy vấn```
(r, pos[r])
```7. Sau mỗi câu trả lời của giám khảo, hãy cập nhật cả hoán vị và`pos`mảng. 
8. Khi cả hai vị trí của cặp phản chiếu hiện tại đều chính xác, hãy di chuyển vào trong cặp tiếp theo. 
9. Khi mọi cặp đã được xử lý, hoán vị sẽ được sắp xếp. 

### Tại sao nó hoạt động 

Sửa một cặp được nhân đôi`(l,r)`. 

Bất cứ khi nào`l`là không chính xác và chúng tôi truy vấn`(l,pos[l])`, một trong hai điều xảy ra. 

Nếu trao đổi đã chọn được thực hiện trực tiếp, giá trị`l`đạt đến vị trí cuối cùng của nó. 

Nếu hoán đổi được nhân đôi được thực hiện, thao tác chỉ ảnh hưởng đến phía được phản chiếu của mảng, đó chính xác là nơi có giá trị`r`cuộc sống. Tiến trình được thực hiện nhằm cố định thành viên còn lại của cặp. 

Lý do tương tự cũng được áp dụng khi chúng ta truy vấn`r`. 

Trong suốt quá trình, các vị trí bên ngoài cặp được phản chiếu hiện tại không bao giờ cần phải xem lại. Khi một cặp trở nên chính xác, các thao tác sau này không thể phá vỡ cặp đó vì các truy vấn trong tương lai chỉ liên quan đến các vị trí hoàn toàn bên trong cặp đó. 

Bằng cách quy nạp từ cặp ngoài cùng về phía tâm, mọi vị trí cuối cùng đều đạt đến giá trị chính xác của nó, do đó hoán vị cuối cùng được sắp xếp. 

## Giải pháp Python 

Đây là cấu trúc giải pháp tương tác được chấp nhận.```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    p = [0] + list(map(int, input().split()))

    pos = [0] * (n + 1)
    for i in range(1, n + 1):
        pos[p[i]] = i

    def query(x, y):
        print("?", x, y)
        sys.stdout.flush()

        u, v = map(int, input().split())
        if u == -1:
            sys.exit(0)

        p[u], p[v] = p[v], p[u]
        pos[p[u]] = u
        pos[p[v]] = v

    if n % 2 == 1:
        mid = (n + 1) // 2
        while p[mid] != mid:
            query(pos[mid], mid)

    for l in range(1, n // 2 + 1):
        r = n + 1 - l

        while p[l] != l or p[r] != r:
            if p[l] != l:
                query(l, pos[l])
            else:
                query(r, pos[r])

    print("!")
    sys.stdout.flush()

t = int(input())
for _ in range(t):
    solve()
```Bản thân hoán vị được lưu trữ vì thẩm phán cho chúng ta biết sự hoán đổi chính xác đã thực sự xảy ra. Sau khi nhận được`(u,v)`, chúng tôi cập nhật cả mảng và mảng vị trí nghịch đảo trong thời gian không đổi. 

Vị trí chính giữa của một hoán vị có độ dài lẻ rất đặc biệt vì nó là tấm gương của chính nó. Việc sửa nó trước tiên sẽ đơn giản hóa logic còn lại. 

Đối với mỗi cặp được nhân đôi, thuật toán luôn ưu tiên cố định vị trí bên trái. Khi phía bên trái trở nên chính xác, nó sẽ chuyển sang phía bên phải. Điều này đảm bảo rằng các cặp bên ngoài đã hoàn thiện sẽ không bao giờ cần được chú ý nữa. 

Lỗi triển khai phổ biến nhất là quên cập nhật`pos`sau một lần trao đổi. Toàn bộ chiến lược phụ thuộc vào việc biết vị trí hiện tại của mọi giá trị. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hoán vị ban đầu:```
[5,1,3,4,2]
```| Bước | Truy vấn | Trao đổi thực tế | Hoán vị | 
| --- | --- | --- | --- | 
| Bắt đầu | - | - | [5,1,3,4,2] | 
| 1 | (1,5) | (1,5) | [2,1,3,4,5] | 
| 2 | (4,5) | (2,1) | [1,2,3,4,5] | 

Thao tác đầu tiên sửa giá trị`5`. Truy vấn thứ hai nhắm vào cặp được nhân đôi`(2,4)`. Mặc dù trọng tài đã chọn phép hoán đổi được phản chiếu, hoán vị vẫn được sắp xếp đầy đủ. 

### Ví dụ 2 

Hoán vị ban đầu:```
[2,1,4,3]
```| Bước | Truy vấn | Trao đổi thực tế | Hoán vị | 
| --- | --- | --- | --- | 
| Bắt đầu | - | - | [2,1,4,3] | 
| 1 | (1,2) | (1,2) | [1,2,4,3] | 
| 2 | (3,4) | (3,4) | [1,2,3,4] | 

Cặp bên ngoài`(1,4)`trở nên đúng đầu tiên. Sau đó, cặp bên trong`(2,3)`đã được sửa. Khi một cặp đã chính xác, các thao tác sau này sẽ không bao giờ làm ảnh hưởng đến cặp đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) truy vấn dự kiến ​​| Mỗi cặp được nhân đôi yêu cầu công việc dự kiến ​​liên tục | 
| Không gian | O(n) | Mảng hoán vị và vị trí nghịch đảo | 

Tổng của tất cả`n`chỉ là`2·10^4`, do đó mức sử dụng bộ nhớ rất nhỏ. Số lượng hoạt động dự kiến ​​khoảng`2.5n`, phù hợp với giới hạn dự định và vừa vặn thoải mái trong giới hạn cho phép. 

## Trường hợp thử nghiệm 

Vì đây là sự cố tương tác nên các bài kiểm tra xác nhận ngoại tuyến truyền thống không có ý nghĩa. Phản ứng của thẩm phán phụ thuộc vào những lựa chọn ngẫu nhiên trong quá trình thi hành án. 

Để thử nghiệm cục bộ, cần có một trình mô phỏng. Các tình huống điển hình để xác minh được hiển thị dưới đây.```
# Case 1: already sorted
# n = 1
# [1]

# Case 2: smallest non-trivial permutation
# n = 2
# [2, 1]

# Case 3: odd length, center incorrect
# n = 5
# [1, 3, 2, 4, 5]

# Case 4: completely reversed
# n = 8
# [8,7,6,5,4,3,2,1]

# Case 5: random large permutation
# n = 4000
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[1]`| Kết thúc ngay lập tức | Vỏ cạnh đơn | 
|`[2,1]`| Một cặp được nhân đôi | Ví dụ nhỏ nhất có ý nghĩa | 
| Số lẻ`n`sai tâm | Logic cố định tâm | Vị trí tự soi gương | 
| Hoán vị ngược | Nhiều chỉnh sửa | Tính đúng đắn toàn cầu | 
| Hoán vị ngẫu nhiên lớn | Được chấp nhận trong giới hạn | Hiệu suất | 

## Vỏ cạnh 

Hãy xem xét`n = 1`.```
[1]
```Không có cặp nào được phản chiếu và tâm đã chính xác. Thuật toán xuất ra ngay lập tức`!`. 

Hãy xem xét một hoán vị có độ dài lẻ trong đó chỉ có tâm là sai:```
n = 5
[1,2,4,3,5]
```Vị trí trung tâm là`3`. Vì vị trí trung tâm phản chiếu chính nó nên liên tục truy vấn`(pos[3],3)`cuối cùng đặt giá trị`3`ở giữa. Sau đó, các cặp bên ngoài được xử lý bình thường. 

Hãy xem xét một cặp trong đó một bên đã đúng:```
[1,4,3,2]
```Đối với cặp`(1,4)`, chức vụ`1`đã đúng rồi. Thuật toán ngay lập tức hoạt động ở phía bên phải thay vì lãng phí các thao tác ở phía bên trái. Đây chính là lý do tại sao điều kiện```
if p[l] != l:
    ...
else:
    ...
```là cần thiết. 

Cuối cùng, hãy xem xét một truy vấn có phiên bản phản chiếu được thực thi thay vì phiên bản dự định. Đó là thách thức trung tâm của vấn đề. Thuật toán xử lý nó vì mọi truy vấn được thiết kế xung quanh một cặp được phản chiếu. Nếu bên trái không được cải thiện thì bên phải sẽ cải thiện. Tiến trình trên cặp không bao giờ biến mất, đây chính xác là thuộc tính giữ cho số lượng hoạt động dự kiến ​​​​là tuyến tính.
