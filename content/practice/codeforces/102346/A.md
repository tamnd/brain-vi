---
title: "CF 102346A - Tác phẩm nghệ thuật"
description: "Căn phòng là hình chữ nhật có các góc (0, 0) và (M, N). Kẻ trộm cần một đường đi liên tục từ góc dưới bên trái đến góc trên bên phải trong khi vẫn ở bên ngoài đĩa phát hiện của mọi cảm biến."
date: "2026-08-14T05:19:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102346
codeforces_index: "A"
codeforces_contest_name: "2019-2020 ACM-ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 102346
solve_time_s: 78
verified: true
draft: false
---

[CF 102346A - Tác phẩm nghệ thuật](https://codeforces.com/problemset/problem/102346/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Căn phòng có hình chữ nhật với các góc`(0, 0)`Và`(M, N)`. Kẻ trộm cần một đường đi liên tục từ góc dưới bên trái đến góc trên bên phải trong khi vẫn ở bên ngoài đĩa phát hiện của mọi cảm biến. 

Mỗi cảm biến`(x, y, s)`có thể được xem về mặt hình học như một vòng tròn bán kính khép kín`s`tập trung vào`(x, y)`. Câu hỏi không phải là tìm ra một con đường thực tế. Câu hỏi hữu ích là liệu việc thu thập các đĩa bị cấm có thể tạo thành một bức tường liên tục ngăn cách hai góc cần thiết hay không. 

Với`K <= 1000`, việc kiểm tra từng cặp cảm biến là khả thi vì chỉ có`K(K-1)/2`, nhiều nhất là khoảng`500,000`, cặp. Do đó, một thuật toán bậc hai là thực tế. Một cách tiếp cận hình khối sẽ có sẵn`10^9`hoạt động ở giới hạn trên và quá chậm so với giới hạn cuộc thi một giây. Kích thước phòng có thể đạt`10^4`và bán kính cảm biến cũng có thể đạt tới`10^4`, do đó, bản thân các giá trị tọa độ không làm cho mô phỏng lưới trở nên hấp dẫn. Trên thực tế, một tấm lưới bao quanh căn phòng có thể chứa khoảng`10^8`vị trí vừa không cần thiết vừa lớn hơn nhiều so với số lượng cảm biến. 

Có hai chi tiết ranh giới rất dễ xử lý sai. Đầu tiên, việc chạm vào được coi là chặn vì kẻ trộm phải ở một khoảng cách hoàn toàn lớn hơn độ nhạy của cảm biến. Ví dụ: hai cảm biến tiếp xúc chính xác phải được coi là được kết nối:```
10 10 2
3 5 2
7 5 2
```Hai đĩa chạm vào nhau`(5, 5)`và tạo thành một bức tường từ bên trái sang bên phải. Câu trả lời đúng là`N`. Sử dụng một cách nghiêm ngặt`<`so sánh sẽ báo cáo sai`S`. 

Thứ hai, cảm biến có thể chạm tới ranh giới của phòng mặc dù tâm của nó nằm hoàn toàn bên trong phòng. Ví dụ:```
10 10 1
5 5 5
```Đĩa chạm vào cả 4 mặt nên tách biệt hoàn toàn góc dưới bên trái và góc trên bên phải. Câu trả lời đúng là`N`. Việc thực hiện bất cẩn chỉ kiểm tra xem các trung tâm cảm biến có nằm trên ranh giới hay không sẽ bỏ sót vật cản. 

Bốn cặp ranh giới liên quan cũng dễ bị nhầm lẫn. Một chướng ngại vật được kết nối chạm vào phía trên và bên trái sẽ chặn góc dưới bên trái với phần còn lại của căn phòng. Một thành phần chạm vào cạnh dưới và bên phải sẽ chặn góc trên bên phải. Một thành phần chạm trên và dưới sẽ ngăn cách hai cạnh dọc, trong khi một thành phần chạm trái và phải sẽ tách hai cạnh ngang. Do đó, điều kiện chặn chính xác là một thành phần cảm biến được kết nối chạm vào ít nhất một phía từ`{top, left}`và ít nhất một phía từ`{bottom, right}`. 

## Phương pháp tiếp cận 

Cách tiếp cận hình học trực tiếp sẽ cố gắng xây dựng vùng không gian tự do và tìm kiếm đường đi qua nó. Một khả năng là tách phòng thành một lưới mịn và chạy BFS, đánh dấu mọi điểm lưới được cảm biến bao phủ. Điều này đơn giản về mặt khái niệm và nó sẽ hoạt động trên một căn phòng nhỏ, nhưng căn phòng đó có thể`10^4`qua`10^4`. Thậm chí một tế bào trên một mét cũng sẽ cho`10^8`các ô, trong khi đầu vào chỉ chứa`1000`cảm biến. Sự rời rạc hóa cũng đưa ra những câu hỏi khó về độ chính xác vì những trở ngại thực sự là những hình tròn chứ không phải những hình vuông đơn vị. 

Quan sát hữu ích là đường đi chỉ trở nên không thể thực hiện được khi các vòng tròn cấm tạo thành một rào cản liên tục. Hai cảm biến thuộc về cùng một rào chắn khi đĩa của chúng giao nhau hoặc chạm vào nhau. Đối với cảm biến`i`Và`j`, điều này xảy ra khi`(xi - xj)^2 + (yi - yj)^2 <= (si + sj)^2`. 

Do đó, chúng ta có thể quên hình dạng chính xác của vùng chồng lấp sau khi thiết lập kết nối. Biến mọi cảm biến thành một đỉnh đồ thị và kết nối hai đỉnh bất cứ khi nào đĩa của chúng giao nhau hoặc chạm vào. Mỗi thành phần được kết nối của biểu đồ này đại diện cho một vùng bị cấm được kết nối. 

Cấu trúc biểu đồ brute-force kiểm tra từng cặp cảm biến, chỉ`O(K^2)`. Sau khi đồ thị được xây dựng, DFS sẽ truy cập mọi đỉnh và cạnh một lần. Đối với mỗi thành phần, chúng tôi ghi lại ranh giới phòng mà vòng tròn của nó chạm vào. Nếu thành phần kết nối một ranh giới từ`{top, left}`đến một từ`{bottom, right}`, nó tạo thành một dải phân cách giữa`(0, 0)`Và`(M, N)`, vậy câu trả lời là`N`. Nếu không có thành phần nào thực hiện điều này thì không gian trống chứa một tuyến đường giữa hai góc, vì vậy câu trả lời là`S`. 

Ý tưởng tương tự có thể được thực hiện bằng cấu trúc hợp nhất được thiết lập rời rạc thay vì DFS. Đối với vấn đề này, DFS đủ đơn giản và làm cho việc diễn giải thành phần được kết nối trở nên rõ ràng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lưới | Có khả năng`O(MN)`hoặc tệ hơn |`O(MN)`| Quá lớn | 
| Hình học đường dẫn Brute-force | Có khả năng tệ hơn nhiều so với bậc hai | Lớn | Quá chậm | 
| Biểu đồ giao điểm cảm biến + DFS |`O(K^2)`|`O(K^2)`với một biểu đồ rõ ràng | Đã chấp nhận | 
| Đồ thị giao điểm cảm biến + DSU |`O(K^2 α(K))`|`O(K)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bảo quản mọi cảm biến như`(x, y, s)`. Xử lý vùng phát hiện của nó như một đĩa kín, vì khoảng cách chính xác bằng`s`cũng được phát hiện. 
2. Đối với mỗi cặp cảm biến, hãy tính bình phương khoảng cách giữa tâm của chúng và so sánh nó với tổng bình phương bán kính của chúng. Nếu như`dx² + dy² <= (si + sj)²`, 

kết nối hai cảm biến trong đồ thị. Khoảng cách bình phương tránh hoàn toàn số học dấu phẩy động và các giá trị trung gian lớn nhất được xử lý dễ dàng bằng số nguyên Python. 
3. Chạy DFS từ mọi cảm biến chưa được truy cập. Trong DFS, hãy thu thập các ranh giới phòng mà mọi cảm biến trong thành phần được kết nối đó chạm tới. Một cảm biến chạm vào ranh giới bên trái khi`x - s <= 0`, ranh giới bên phải khi`x + s >= M`, ranh giới dưới cùng khi`y - s <= 0`, và ranh giới trên cùng khi`y + s >= N`. 
4. Sau khi hoàn thiện một thành phần được kết nối, hãy kiểm tra xem nó có chạm vào ít nhất một ranh giới trong`{top, left}`và ít nhất một ranh giới trong`{bottom, right}`. Nếu vậy thì thành phần là bức tường liên tục ngăn cách góc lối vào với góc sơn nên xuất ngay`N`. 
5. Nếu mọi thành phần được kết nối đều thất bại trong lần kiểm tra đó, hãy xuất ra`S`. Không có thành phần bị cấm nào ngăn cách hai góc, do đó tồn tại một con đường liên tục xuyên qua phần còn lại của căn phòng. 

Tại sao nó hoạt động: điều bất biến là mọi thành phần DFS đại diện chính xác cho một tập hợp các đĩa cảm biến được kết nối. Bất cứ khi nào hai đĩa chạm nhau, kẻ trộm không thể đi qua điểm tiếp xúc của chúng vì sự bình đẳng bị cấm, do đó, việc hợp nhất các đĩa chạm nhau sẽ bảo tồn cấu trúc liên kết liên quan đến sự tồn tại của đường dẫn. Một bộ cấm được kết nối nối từ trên xuống dưới hoặc từ trái sang phải sẽ tách hai cạnh đối diện của hình chữ nhật và việc nối từ trên sang trái hoặc từ dưới sang phải sẽ cắt đứt một trong hai góc mục tiêu. Ngược lại, nếu không có thành phần nào kết nối một phía của`{top, left}`với một bên của`{bottom, right}`, không có tường chắn nào được nối có khả năng ngăn cách hai góc chéo. Thuật toán kiểm tra chính xác các dấu phân cách có thể có này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    M, N, K = map(int, input().split())
    sensors = [tuple(map(int, input().split())) for _ in range(K)]

    graph = [[] for _ in range(K)]

    for i in range(K):
        x1, y1, r1 = sensors[i]
        for j in range(i + 1, K):
            x2, y2, r2 = sensors[j]

            dx = x1 - x2
            dy = y1 - y2
            sr = r1 + r2

            if dx * dx + dy * dy <= sr * sr:
                graph[i].append(j)
                graph[j].append(i)

    visited = [False] * K

    for start in range(K):
        if visited[start]:
            continue

        stack = [start]
        visited[start] = True

        top = False
        right = False
        bottom = False
        left = False

        while stack:
            u = stack.pop()
            x, y, r = sensors[u]

            if y + r >= N:
                top = True
            if x + r >= M:
                right = True
            if y - r <= 0:
                bottom = True
            if x - r <= 0:
                left = True

            for v in graph[u]:
                if not visited[v]:
                    visited[v] = True
                    stack.append(v)

        if (top or left) and (bottom or right):
            print("N")
            return

    print("S")

if __name__ == "__main__":
    solve()
```Vòng lặp lồng nhau đầu tiên xây dựng biểu đồ giao điểm. Không có căn bậc hai vì việc so sánh khoảng cách bình phương cho kết quả giống hệt nhau và tránh được các lỗi ranh giới dấu phẩy động. các`<=`so sánh là có chủ ý: hai đĩa phát hiện tiếp tuyến không để lại đường đi hợp lệ nào qua điểm tiếp xúc của chúng. 

DFS sử dụng ngăn xếp rõ ràng thay vì DFS đệ quy. Với`K = 1000`, đệ quy Python có thể là đủ sau khi tăng giới hạn đệ quy, nhưng việc truyền tải lặp lại sẽ tránh phụ thuộc vào độ sâu đệ quy và làm cho việc triển khai trở nên mạnh mẽ hơn. 

Mỗi cảm biến đóng góp bốn bài kiểm tra ranh giới độc lập. Sự bất bình đẳng cũng mang tính bao hàm vì một chiếc đĩa chỉ chạm vào ranh giới phòng sẽ tham gia vào một bức tường chắn. Ví dụ,`x - r == 0`có nghĩa là cảm biến đạt đến ranh giới bên trái và phải được tính. 

Thành phần này bị từ chối khi`(top or left) and (bottom or right)`là đúng. Biểu thức rút gọn này biểu thị bốn cặp phân tách: trên-dưới, trên-phải, trái-dưới và trái-phải sau khi xem xét hướng góc được sử dụng bởi hệ tọa độ. Với`top`ghép nối với`left`như một nhóm và`bottom`ghép nối với`right`còn lại, nó tương đương với việc kiểm tra bốn rào cản có thể ngăn cách hai góc chéo. 

Đầu vào chỉ có một trường hợp thử nghiệm, vì vậy`solve()`được gọi một lần. Các số nguyên có độ chính xác tùy ý của Python cũng loại bỏ mọi lo ngại về tràn khỏi các phép tính tọa độ và bán kính bình phương. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là:```
10 22 2
4 6 5
6 16 5
```Hai cảm biến có khoảng cách trung tâm bình phương`(4 - 6)^2 + (6 - 16)^2 = 104`. 

Bán kính tổng hợp của chúng là`10`, do đó bình phương bán kính tổng hợp là`100`. Từ`104 > 100`, các đĩa không tiếp xúc và tạo thành hai thành phần riêng biệt. 

| Thành phần | Cảm biến | Đầu trang | Đúng | Dưới cùng | Trái | Chặn? | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 |`(4,6,5)`| Không | Không | Không | Có | Không | 
| 2 |`(6,16,5)`| Không | Không | Không | Không | Không | 

Không có thành phần nào nối hai ranh giới ngăn cách nên kẻ trộm có thể đột nhập từ lối vào bức tranh. 

Đầu ra là`S`. 

### Mẫu 2 

Đầu vào là:```
10 10 2
3 7 4
5 4 4
```Bình phương khoảng cách giữa các tâm cảm biến là`(3 - 5)^2 + (7 - 4)^2 = 13`. 

Tổng bình phương bán kính của chúng là`8² = 64`, do đó hai đĩa phát hiện chồng lên nhau và trở thành một thành phần được kết nối. 

| Thành phần | Cảm biến | Đầu trang | Đúng | Dưới cùng | Trái | Chặn? | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | cả hai cảm biến | Có | Có | Có | Có | Có | 

Thành phần này đạt đến tất cả bốn ranh giới. Đặc biệt, nó kết nối ranh giới từ nhóm thứ nhất với ranh giới từ nhóm thứ hai, tạo ra một rào cản hoàn chỉnh giữa hai góc mục tiêu. 

Đầu ra là`N`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(K^2)`| Mỗi cặp cảm biến được kiểm tra một lần và DFS xử lý mọi cạnh của biểu đồ một lần. | 
| Không gian |`O(K^2)`| Trong trường hợp xấu nhất, mọi cặp cảm biến đều cắt nhau nên đồ thị chứa`O(K^2)`các cạnh. | 

Với`K <= 1000`, có nhiều nhất`499,500`cặp cảm biến. Nó đủ nhỏ cho một giải pháp bậc hai, trong khi kích thước phòng không bao giờ cần phải mở rộng thành một lưới. Biểu đồ và đường truyền của nó nằm trong giới hạn dự kiến. 

## Trường hợp thử nghiệm 

Bộ khai thác thử nghiệm sau đây sử dụng cùng một`solve()`logic thông qua một hàm chấp nhận một chuỗi. Các trường hợp tùy chỉnh bao gồm kích thước phòng tối thiểu, một cảm biến đơn, cảm biến tiếp tuyến, cảm biến chạm tới góc lối vào và một bộ cảm biến dày đặc.```python
import sys
import io

def solve(inp: str) -> str:
    data = io.StringIO(inp)
    input_fn = data.readline

    M, N, K = map(int, input_fn().split())
    sensors = [tuple(map(int, input_fn().split())) for _ in range(K)]

    graph = [[] for _ in range(K)]

    for i in range(K):
        x1, y1, r1 = sensors[i]
        for j in range(i + 1, K):
            x2, y2, r2 = sensors[j]

            dx = x1 - x2
            dy = y1 - y2
            sr = r1 + r2

            if dx * dx + dy * dy <= sr * sr:
                graph[i].append(j)
                graph[j].append(i)

    visited = [False] * K

    for start in range(K):
        if visited[start]:
            continue

        stack = [start]
        visited[start] = True

        top = right = bottom = left = False

        while stack:
            u = stack.pop()
            x, y, r = sensors[u]

            top |= y + r >= N
            right |= x + r >= M
            bottom |= y - r <= 0
            left |= x - r <= 0

            for v in graph[u]:
                if not visited[v]:
                    visited[v] = True
                    stack.append(v)

        if (top or left) and (bottom or right):
            return "N\n"

    return "S\n"

assert solve("""10 22 2
4 6 5
6 16 5
""") == "S\n", "sample 1"

assert solve("""10 10 2
3 7 4
5 4 4
""") == "N\n", "sample 2"

assert solve("""100 100 3
40 50 30
5 90 50
90 10 5
""") == "S\n", "sample 3"

assert solve("""10 10 1
5 5 1
""") == "S\n", "minimum room, isolated sensor"

assert solve("""10 10 2
3 5 2
7 5 2
""") == "N\n", "tangent sensors form a left-right wall"

assert solve("""10 10 1
3 3 5
""") == "N\n", "sensor reaches the entrance corner"

assert solve("""10 10 1
5 5 5
""") == "N\n", "one disk reaches all four boundaries"

assert solve("""10 10 4
2 2 3
8 2 3
2 8 3
8 8 3
""") == "N\n", "four equal sensors create connected barriers"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`10 10 1 / 5 5 1`|`S`| Kích thước phòng tối thiểu và chướng ngại vật cách ly | 
|`10 10 2 / 3 5 2 / 7 5 2`|`N`| Tiếp tuyến chính xác phải được tính là giao lộ | 
|`10 10 1 / 3 3 5`|`N`| Một cảm biến lớn có thể chặn góc lối vào | 
|`10 10 1 / 5 5 5`|`N`| Một thành phần có thể chạm tới mọi ranh giới | 
| Bốn cảm biến bằng nhau ở`(2,2)`,`(8,2)`,`(2,8)`,`(8,8)`|`N`| Bán kính bằng nhau, nhiều kết nối và hợp nhất thành phần | 

## Vỏ cạnh 

Một cặp tiếp tuyến là lỗi ranh giới số phổ biến nhất. Coi như:```
10 10 2
3 5 2
7 5 2
```Các tâm cách nhau bốn mét và tổng bán kính là bốn, do đó phép so sánh là`16 <= 16`. Các đĩa được kết nối chính xác tại một điểm. Sự tiếp xúc đó đủ để chặn đường đi qua điểm vì kẻ trộm phải ở xa hơn độ nhạy của cảm biến. Biểu đồ đặt cả hai cảm biến vào một thành phần và thành phần đó chạm vào ranh giới bên trái và bên phải. Đầu ra của thuật toán`N`. 

Một cảm biến lớn có thể chạm tới một góc mặc dù mọi trung tâm cảm biến đều nằm ngay bên trong phòng. Vì:```
10 10 1
3 3 5
```khoảng cách từ tâm cảm biến đến`(0,0)`là`sqrt(18)`, nhỏ hơn`5`. Bản thân lối vào nằm bên trong đĩa phát hiện nên không có điểm bắt đầu hợp pháp ở lối vào. Việc kiểm tra ranh giới cũng cho thấy đĩa đạt đến cả mặt trái và mặt dưới. Logic phân tách của thuật toán xử lý các rào cản ranh giới được kết nối, trong khi diễn giải hình học làm rõ lý do tại sao đây là trường hợp lối vào bị chặn. 

Một cảm biến chạm vào ranh giới chính xác phải được tính. Ví dụ:```
10 10 1
5 5 5
```cho`x-r = 0`,`x+r = 10`,`y-r = 0`, Và`y+r = 10`. Thành phần này chạm vào mọi phía nên điều kiện`(top or left) and (bottom or right)`là đúng và câu trả lời là`N`. Việc thay thế bất kỳ so sánh ranh giới nào bằng một bất đẳng thức nghiêm ngặt sẽ bỏ lỡ trường hợp này một cách không chính xác. 

Cuối cùng, một số cảm biến có thể không tạo thành một bức tường riêng lẻ nhưng có thể tạo thành một bức tường sau khi hợp nhất bắc cầu. Giả sử cảm biến A chạm vào B, B chạm vào C và C chạm vào ranh giới phòng. A được kết nối với ranh giới đó ngay cả khi bản thân A ở xa nó. DFS nắm bắt được điều này vì cả ba cảm biến đều thuộc về cùng một thành phần được kết nối. Thành phần này ghi lại mọi ranh giới mà bất kỳ thành viên nào của nó đạt được, đây chính xác là thông tin cần thiết để quyết định xem toàn bộ chuỗi có chặn phòng hay không.
