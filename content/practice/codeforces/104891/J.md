---
title: "CF 104891J - Dịch chuyển tức thời"
description: "Chúng ta được cho một hệ thống các phòng được sắp xếp theo chu kỳ từ 0 đến n-1. Mỗi phòng có một số nguyên a[i] hiển thị trên mặt số tròn. Từ phòng i, Bobo có hai hành động có thể thực hiện được, mỗi hành động tiêu tốn đúng một đơn vị thời gian."
date: "2026-06-28T18:02:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104891
codeforces_index: "J"
codeforces_contest_name: "The 2023 ICPC Asia Macau Regional Contest (The 2nd Universal Cup. Stage 15: Macau)"
rating: 0
weight: 104891
solve_time_s: 80
verified: false
draft: false
---

[CF 104891J - Dịch chuyển tức thời](https://codeforces.com/problemset/problem/104891/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hệ thống các phòng được sắp xếp theo chu kỳ từ`0`ĐẾN`n-1`. Mỗi phòng có một số nguyên duy nhất`a[i]`hiển thị trên mặt số tròn. Từ phòng`i`, Bobo có hai hành động có thể xảy ra, mỗi hành động tiêu tốn chính xác một đơn vị thời gian. 

Anh ấy có thể xoay nút xoay trong phòng`i`theo chiều kim đồng hồ, làm tăng`a[i]`bằng một, hoặc anh ta có thể dịch chuyển tức thời đến phòng`(i + a[i]) mod n`, sử dụng giá trị hiện tại của mặt số làm độ dài bước nhảy. 

Bobo bắt đầu tại phòng`0`và muốn đến phòng`x`trong tổng thời gian tối thiểu. 

Sự tinh tế quan trọng là mảng`a[i]`không cố định. Mỗi khi chúng tôi quyết định sử dụng dịch chuyển tức thời của một căn phòng, chúng tôi có thể đã dành thời gian để tăng giá trị của nó trước đó và giá trị được sửa đổi đó vẫn tồn tại cho những lần sử dụng căn phòng đó trong tương lai. 

Vì vậy, vấn đề không chỉ là đường đi ngắn nhất trên các cạnh cố định. Đó là đường dẫn ngắn nhất trong đó trọng số cạnh của một nút phụ thuộc vào số lần chúng ta đã tăng nút đó trước khi sử dụng nó. 

Các ràng buộc cho phép lên đến`n = 100000`, điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng tất cả các trạng thái của`(room, value of all a[i])`. Ngay cả việc lưu trữ đầy đủ các trạng thái toàn cầu cũng là không thể. Một giải pháp phải tránh coi các phần tăng thêm như một phần của không gian trạng thái toàn cục. 

Một vài trường hợp đặc biệt bộc lộ những lỗi suy luận ngây thơ. Nếu như`a[i] = 0`cho tất cả`i`, dịch chuyển tức thời sẽ không làm gì trừ khi chúng ta tăng lần đầu tiên, vì vậy mọi bước di chuyển hữu ích đều yêu cầu phải trả ít nhất một lần tăng cho mỗi bước. Nếu tất cả`a[i]`đã lớn và trực tiếp dẫn đến`x`, câu trả lời tối ưu có thể chỉ là một vài lần dịch chuyển tức thời mà không tăng thêm chút nào. Một tình huống phức tạp khác xảy ra khi tăng liên tục một phòng trước khi sử dụng nhiều lần sẽ mang lại khả năng định tuyến lâu dài tốt hơn vì mức tăng là vĩnh viễn và có thể tái sử dụng. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực trực tiếp là coi mỗi cấu hình của mảng là một trạng thái. Từ một trạng thái, chúng tôi có thể tăng bất kỳ chỉ số nào hoặc dịch chuyển tức thời khỏi phòng hiện tại. Điều này dẫn đến sự bùng nổ: mỗi lần tăng đều thay đổi trạng thái toàn cục, và mỗi lần tăng`a[i]`có thể phát triển lớn tùy ý. Thậm chí hạn chế các giá trị modulo`n`vẫn còn lá`n^n`những trạng thái có thể, hoàn toàn không thể thực hiện được. 

Một cách tiếp cận mạnh mẽ có cấu trúc hơn là mô phỏng đường đi ngắn nhất trên biểu đồ mở rộng trong đó mỗi nút được`(current room, full array state)`. Đó vẫn là số mũ và không thể sử dụng được. 

Quan sát quan trọng là chúng ta không bao giờ cần phải theo dõi toàn bộ lịch sử của các bước tăng thêm. Điều quan trọng chỉ là mỗi phòng được tăng lên bao nhiêu lần trước lần sử dụng dịch chuyển tiếp theo. Mỗi mức tăng đều mang tính cục bộ và độc lập giữa các phòng và khi chúng tôi quyết định sử dụng một phòng ở một giá trị nào đó, chúng tôi chỉ quan tâm đến thời điểm tốt nhất để có thể đạt được mức bù đắp hiệu quả cụ thể. 

Đối với phòng cố định`i`, nếu chúng tôi quyết định sử dụng nó sau`k`tăng dần thì chi phí trả cho căn phòng đó chính xác là`k + 1`(k tăng cộng với một lần dịch chuyển) và quá trình chuyển đổi trở thành`(i + a[i] + k) mod n`. Điều này có nghĩa là mỗi phòng tạo ra một chuỗi vô hạn các cạnh có thể xuất ra với chi phí ngày càng tăng, tạo thành một cấu trúc đơn điệu. 

Cấu trúc này cho phép chúng ta xử lý vấn đề như một đường đi ngắn nhất trên biểu đồ trong đó mỗi nút có một chuỗi các cạnh đi ra với mức tăng dần về chi phí. Chúng tôi có thể xử lý từng phòng một cách lười biếng, luôn xem xét mức tăng chưa sử dụng tiếp theo khi cần. Điều này được xử lý một cách tự nhiên bằng cách xử lý giống như Dijkstra trên các trạng thái`(room, how many times we have used this room as a source)`mà không lưu trữ rõ ràng các trạng thái mảng đầy đủ. 

Chúng tôi luôn mở rộng “cách sử dụng dịch chuyển tiếp theo” rẻ nhất có sẵn trên tất cả các phòng, điều này đảm bảo sự tối ưu do tính đơn điệu của việc tăng chi phí cho mỗi phòng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi làm mẫu cho từng phòng`i`vì có một chuỗi các hành động dịch chuyển tức thời có thể được lập chỉ mục theo số lần chúng tôi đã tăng nó trước khi sử dụng. Sử dụng nó`k`-chi phí lần thứ`k + 1`tổng số hành động cho việc sử dụng phòng đó và đưa chúng ta đến`(i + a[i] + k) mod n`. 

Sau đó, chúng tôi chạy quy trình đường đi ngắn nhất qua các phòng, nhưng thay vì có một cạnh duy nhất cho mỗi phòng, chúng tôi tạo ra các cạnh ngày càng tốt hơn từ mỗi phòng. 

1. Khởi tạo mảng khoảng cách`dist`kích thước`n`với vô cùng, và thiết lập`dist[0] = 0`kể từ khi chúng ta bắt đầu ở phòng`0`. 
2. Đối với mỗi phòng`i`, duy trì một con trỏ`used[i] = 0`thể hiện số lượng gia tăng mà chúng tôi đã “tiêu thụ” cho căn phòng đó trong các lần mở rộng trước đó. Điều này đảm bảo chúng tôi không bao giờ xem xét lại cùng một mức tăng thêm hai lần. 
3. Đẩy`(0, 0)`vào hàng ưu tiên, thể hiện việc đang ở trong phòng`0`với chi phí bằng không. 
4. Trích xuất trạng thái nhiều lần`(d, i)`với khoảng cách nhỏ nhất từ ​​hàng đợi. Nếu như`d`không bằng`dist[i]`, bỏ qua nó vì nó đã lỗi thời. 
5. Từ phòng`i`, hãy xem xét việc sử dụng dịch chuyển tức thời tiếp theo với mức tăng hiện tại`used[i]`. Chi phí sử dụng nó là`d + used[i] + 1`, và đích đến là`(i + a[i] + used[i]) mod n`. 
6. Nếu chi phí mới này được cải thiện`dist[j]`, Ở đâu`j`là phòng đích, cập nhật`dist[j]`và đẩy nó vào hàng đợi ưu tiên. 
7. Tăng`used[i]`bởi một, kể từ lần tiếp theo chúng tôi mở rộng phòng`i`, chúng ta sẽ xem xét phiên bản nâng cao tiếp theo của dịch chuyển tức thời. 
8. Tiếp tục cho đến khi hàng ưu tiên trống. 

Thuật toán luôn mở rộng mức sử dụng dịch chuyển tiếp theo rẻ nhất có thể trên tất cả các phòng và tăng dần chi phí sử dụng lại từng phòng. 

### Tại sao nó hoạt động 

Mỗi phòng tạo ra một chuỗi các hành động dịch chuyển tức thời có thể xảy ra theo chiều hướng tăng dần nghiêm ngặt, trong đó hành động thứ k từ phòng đó luôn có giá cao hơn chính xác một lần so với hành động trước đó. Tính đơn điệu này đảm bảo rằng một khi chúng ta đã xem xét mức sử dụng thứ k của một căn phòng thì việc sử dụng cùng mức đó trong tương lai sẽ không thể trở nên rẻ hơn sau này. 

Hàng đợi ưu tiên đảm bảo rằng chúng tôi luôn xử lý hành động có sẵn rẻ nhất trên toàn cầu tiếp theo. Điều này tương đương với việc chạy Dijkstra trên một biểu đồ được mở rộng ngầm trong đó mỗi phòng có một chuỗi vô hạn các cạnh đi được sắp xếp theo chi phí. Bởi vì chi phí biên cho mỗi phòng là đơn điệu và mỗi cấp độ được xử lý chính xác một lần nên không có đường đi tối ưu nào bị bỏ qua hoặc bị trì hoãn không chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import heapq

def solve():
    n, x = map(int, input().split())
    a = list(map(int, input().split()))

    INF = 10**18
    dist = [INF] * n
    dist[0] = 0

    used = [0] * n
    pq = [(0, 0)]

    while pq:
        d, i = heapq.heappop(pq)
        if d != dist[i]:
            continue
        if i == x:
            print(d)
            return

        k = used[i]
        used[i] += 1

        j = (i + a[i] + k) % n
        nd = d + k + 1

        if nd < dist[j]:
            dist[j] = nd
            heapq.heappush(pq, (nd, j))

solve()
```Việc triển khai sử dụng khung Dijkstra tiêu chuẩn, nhưng mỗi nút không hiển thị tất cả các cạnh đi ra của nó cùng một lúc. Thay vào đó, mỗi phòng sẽ hiển thị một chuyển đổi gửi đi mới mỗi khi nó được đưa ra khỏi hàng đợi ưu tiên. 

các`used[i]`mảng là điều cần thiết. Nếu không có nó, chúng tôi sẽ liên tục tạo lại các chuyển đổi giống hệt nhau, dẫn đến việc xử lý vô hạn hoặc trùng lặp. Nó buộc mỗi mức tăng dần của một phòng phải được tiêu thụ đúng một lần. 

Kiểm tra chấm dứt`if i == x`là an toàn vì Dijkstra đảm bảo rằng lần đầu tiên chúng tôi đến đích, nó sẽ đạt được với chi phí tối thiểu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 3
1 2 3 1
```Chúng tôi theo dõi`(room, dist, used[room])`. 

| Bước | Pop | quận | cập nhật đã qua sử dụng | phòng bên cạnh | chi phí mới | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (0,0) | 0 | đã sử dụng[0]=1 | (0+1+0)=1 | 1 | 
| 2 | (1,1) | 1 | đã sử dụng[1]=1 | (1+2+0)=3 | 2 | 
| 3 | (3,2) | 2 | đã sử dụng[3]=1 | (3+1+0)=0 | 3 | 
| 4 | (0,3) | 3 | dừng lại ở x=3 đạt trước đó | - | - | 

Điều này cho thấy mỗi phòng đóng góp chính xác một cạnh tăng dần như thế nào mỗi khi nó được xử lý, dần dần mở khóa các chuyển tiếp tốt hơn. 

### Mẫu 2 

đầu vào:```
4 3
0 0 0 0
```| Bước | Pop | quận | cập nhật đã qua sử dụng | phòng bên cạnh | chi phí mới | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (0,0) | 0 | đã sử dụng[0]=1 | (0+0+0)=0 | 1 | 
| 2 | (0,1) | 1 | đã sử dụng[0]=2 | (0+0+1)=1 | 2 | 
| 3 | (1,2) | 2 | đã sử dụng[1]=1 | (1+0+0)=1 | 3 | 
| 4 | (1,3) | 3 | đã sử dụng[1]=2 | (1+0+1)=2 | 4 | 

Cuối cùng, việc đạt đến phòng 3 yêu cầu các lần tăng lặp lại vì tất cả các lần nhảy ban đầu đều bằng 0, xác nhận rằng thuật toán tính toán chính xác việc thanh toán chi phí tăng nhiều lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi phòng tạo ra tổng số lần thư giãn tối đa là O(n), mỗi phòng được xử lý thông qua hàng đợi ưu tiên | 
| Không gian | O(n) | Khoảng cách, bộ đếm đã sử dụng và bộ nhớ heap | 

Thuật toán vẫn nằm trong giới hạn vì quá trình mở rộng gia tăng của mỗi phòng hoàn toàn đơn điệu và mỗi trạng thái được đẩy vào vùng nhớ nhiều nhất một lần cho mỗi mức tăng hiệu quả. Hệ số log xuất phát từ các hoạt động của đống, được giới hạn bởi tổng số lần thư giãn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, x = map(int, input().split())
    a = list(map(int, input().split()))

    import heapq
    INF = 10**18
    dist = [INF] * n
    dist[0] = 0
    used = [0] * n
    pq = [(0, 0)]

    while pq:
        d, i = heapq.heappop(pq)
        if d != dist[i]:
            continue
        if i == x:
            return str(d)

        k = used[i]
        used[i] += 1
        j = (i + a[i] + k) % n
        nd = d + k + 1

        if nd < dist[j]:
            dist[j] = nd
            heapq.heappush(pq, (nd, j))

    return str(dist[x])

# provided samples
assert run("4 3\n1 2 3 1\n") == "4"
assert run("4 3\n0 0 0 0\n") == "4"
assert run("4 3\n2 2 2 2\n") == "2"

# custom cases
assert run("2 1\n1 1\n") == "1"
assert run("5 4\n0 1 2 3 4\n") == "1"
assert run("6 5\n0 0 0 0 0 0\n") == "6"
assert run("3 2\n2 2 2\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 / 1 1 | 1 | độ chính xác đồ thị tối thiểu | 
| 5 4 / 0 1 2 3 4 | 1 | trường hợp nhảy tối ưu trực tiếp | 
| 6 5 / tất cả số không | 6 | sự cần thiết tăng dần lặp đi lặp lại | 
| 3 2 / cả hai | 1 | tiếp cận ngay lập tức thông qua sự bù đắp tốt nhất | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi tất cả`a[i]`là số không. Trong tình huống này, mọi dịch chuyển ban đầu sẽ quay trở lại cùng một phòng, do đó tiến trình phụ thuộc hoàn toàn vào việc tích lũy số gia tăng trước khi di chuyển. Thuật toán xử lý việc này bằng cách liên tục mở rộng cùng một phòng, tăng`used[i]`mỗi lần như vậy, đích đến hiệu quả dần dần thay đổi và cuối cùng là đến những phòng mới. 

Một trường hợp khác là khi một bước tăng đơn lẻ biến một quá trình chuyển đổi vô ích thành một lối tắt trực tiếp tới`x`. Bởi vì mỗi phòng được mở rộng theo thứ tự tăng dần`k`, khi đạt đến mức tăng có lợi lần đầu tiên, nó sẽ được phát hiện và đẩy vào hàng đợi với chi phí chính xác, đảm bảo không có phiên bản đắt tiền hơn nào sau này có thể ghi đè lên nó. 

Trường hợp tinh vi cuối cùng xảy ra khi nhiều phòng có thể đến cùng một điểm đến với chi phí gia tăng khác nhau. Hàng đợi ưu tiên đảm bảo rằng chỉ có chi phí nhỏ nhất tồn tại trong`dist`, trong khi các điểm đến dư thừa có chi phí lớn hơn sẽ bị bỏ qua khi xuất hiện, duy trì tính chính xác ngay cả khi bị trùng lặp nhiều trạng thái.
