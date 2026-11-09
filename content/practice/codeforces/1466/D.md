---
title: "CF 1466D - Cuộc lao công thứ 13 của Heracles"
description: "Chúng ta được cho một cây có các đỉnh mang trọng số. Với mọi giá trị của $k$ từ $1$ đến $n-1$, chúng ta có thể gán một trong các màu $k$ cho mỗi cạnh. Các cạnh có cùng màu tạo thành một đồ thị con."
date: "2026-06-11T01:45:34+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "sortings", "trees"]
categories: ["algorithms"]
codeforces_contest: 1466
codeforces_index: "D"
codeforces_contest_name: "Good Bye 2020"
rating: 1500
weight: 1466
solve_time_s: 147
verified: true
draft: false
---

[CF 1466D - Cuộc lao công thứ 13 của Heracles](https://codeforces.com/problemset/problem/1466/D) 

**Đánh giá:** 1500 
**Tags:** cấu trúc dữ liệu, tham lam, sắp xếp, cây 
**Thời gian giải:** 2m 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cây có các đỉnh mang trọng số. Với mọi giá trị của$k$từ$1$ĐẾN$n-1$, chúng ta có thể chỉ định một trong$k$màu sắc cho từng cạnh. Các cạnh có cùng màu tạo thành một đồ thị con. Giá trị của một màu là tổng trọng số tối đa giữa các thành phần được kết nối của nó và giá trị của toàn bộ màu là tổng của các giá trị mỗi màu này. 

Nhiệm vụ là tính giá trị tô màu lớn nhất có thể cho mỗi$k$. 

Điều đầu tiên cần chú ý là câu trả lời không thực sự nằm ở màu sắc. Màu sắc chỉ là một cơ chế cho phép chúng ta chia cây thành nhiều phần. Thách thức là hiểu được mức độ đóng góp của mỗi trọng lượng đỉnh khi chúng ta tăng số lượng màu. 

Tổng số đỉnh của tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$. Bất kỳ giải pháp nào cố gắng xây dựng màu sắc một cách rõ ràng, hãy mô phỏng các thành phần được kết nối cho mọi$k$hoặc thực hiện việc duyệt đồ thị lặp đi lặp lại sẽ quá chậm. MỘT$O(n^2)$giải pháp sẽ yêu cầu khoảng$4 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất. Chúng ta cần một cái gì đó gần gũi$O(n \log n)$mỗi tập tin thử nghiệm. 

Trường hợp cạnh tinh tế xuất hiện khi cây chỉ có một cạnh duy nhất.```
2
21 32
1 2
```Câu trả lời là:```
53
```Chỉ có một giá trị có thể có của$k$, cụ thể là$1$. Một số triển khai cố gắng tạo ra những đóng góp bổ sung từ bằng cấp một cách nhầm lẫn và tạo ra các giá trị bổ sung. 

Một trường hợp quan trọng khác là một con đường.```
4
10 10 10 10
1 2
2 3
3 4
```Câu trả lời là:```
40 50 60
```Chỉ có hai đỉnh bên trong có bậc lớn hơn một. Lá không thể đóng góp nhiều hơn một lần. Bất kỳ giải pháp nào coi trọng lượng đỉnh là có thể sử dụng được nhiều lần sẽ bị tính quá mức. 

Cây hình ngôi sao thể hiện ý chính.```
5
100 1 1 1 1
1 2
1 3
1 4
1 5
```Câu trả lời là:```
104 204 304 404
```Trung tâm có bằng cấp$4$, vì vậy trọng lượng của nó có thể xuất hiện tổng cộng$4$lần. Một giải pháp tham lam bỏ qua độ đỉnh sẽ hoàn toàn bỏ qua hành vi này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng lý luận trực tiếp về chất tạo màu. Đối với một cố định$k$, chúng ta có thể xem xét tất cả các cách để phân chia các cạnh giữa các màu, tính toán các thành phần được kết nối bên trong mỗi lớp màu, đánh giá giá trị của chúng và lấy giá trị tối đa. Điều này đúng về mặt định nghĩa, nhưng ngay cả đối với những cây có kích thước khiêm tốn, số lượng màu sắc ở cạnh cũng theo cấp số nhân. Vì có$n-1$các cạnh và lên đến$n-1$màu sắc, không gian tìm kiếm trở nên lớn về mặt thiên văn. 

Để tìm thứ gì đó nhanh hơn, chúng ta cần hiểu điều gì thực sự thay đổi khi chúng ta tăng số lượng màu. 

Bắt đầu với$k=1$. Mọi cạnh đều có cùng màu nên toàn bộ cây tạo thành một thành phần được kết nối. Câu trả lời đơn giản là tổng trọng số của tất cả các đỉnh. 

Bây giờ hãy tưởng tượng chia cây xa hơn. Mỗi khi chúng ta tách một phần của cây, một số đỉnh có thể được tính lại trong thành phần tốt nhất của màu khác. Một đỉnh bậc$d$là sự cố với$d$các cạnh. Trên tất cả các phần phân chia có thể có, trọng lượng của nó có thể xuất hiện nhiều nhất$d$tổng số lần. 

Lần xuất hiện đầu tiên đã được bao gồm trong số tiền ban đầu. Phần còn lại$d-1$sự xuất hiện là lợi nhuận bổ sung mà chúng tôi có thể thu thập sau này. 

Điều này biến đổi vấn đề hoàn toàn. Thay vì nghĩ về chất tạo màu, hãy nghĩ về những đóng góp bổ sung có sẵn. 

Với mọi đỉnh$v$:$$w_v$$đã được tính một lần trong câu trả lời ban đầu. 

Ngoài ra, chúng ta có thể thêm trọng lượng của nó vào$$\deg(v)-1$$lần. 

Since every extra appearance contributes exactly$w_v$, the best strategy is obvious: collect the largest available extra contributions first.

 Chúng tôi tạo một danh sách có chứa$w_v$lặp đi lặp lại$\deg(v)-1$lần cho mỗi đỉnh. Sau khi sắp xếp danh sách này theo thứ tự giảm dần, chúng tôi bắt đầu từ tổng trọng số và liên tục thêm phần đóng góp lớn nhất tiếp theo. Mỗi phép cộng tạo ra câu trả lời cho giá trị tiếp theo của$k$. 

Quá trình tham lam này khớp chính xác với chuỗi câu trả lời tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc trọng số của các đỉnh và tính tổng của chúng. 
2. Tính bậc của mỗi đỉnh trong khi đọc các cạnh của cây. 
3. Khởi tạo chuỗi câu trả lời với tổng trọng số. Đây là giá trị cho$k=1$. 
4. Đối với mỗi đỉnh, hãy chèn chính xác trọng số của nó vào danh sách$\deg(v)-1$lần. 

Bản sao đầu tiên của trọng số mỗi đỉnh đã có trong tổng số. Mỗi bản sao bổ sung thể hiện thêm một lần mà đỉnh này có thể đóng góp. 
5. Sắp xếp danh sách này theo thứ tự giảm dần. 

Trọng số lớn hơn nên được sử dụng sớm hơn vì mỗi bước sẽ bổ sung thêm chính xác một đóng góp. 
6. Bắt đầu từ tổng, cộng liên tục giá trị tiếp theo từ danh sách đã sắp xếp. 
7. Sau mỗi lần cộng, hãy thêm tổng hiện tại vào chuỗi câu trả lời. 
8. In bản đầu tiên$n-1$các giá trị. 

Tại sao nó hoạt động: 

Ban đầu mỗi đỉnh đóng góp một lần, tạo ra tổng trọng số. Một đỉnh có bậc$d$có thể thuộc thành phần cực đại của$d$các lớp màu khác nhau, vì vậy nó có thể đóng góp trọng lượng của nó$d$lần về tổng thể. Vì một bản sao đã được bao gồm nên có chính xác$d-1$bản sao bổ sung có sẵn. 

Mỗi lần tăng số lượng màu sẽ tiêu tốn thêm đúng một đóng góp. Lợi nhuận từ việc lựa chọn đóng góp độc lập với tất cả những đóng góp khác và bằng trọng số đỉnh tương ứng. Bởi vì tất cả lợi nhuận đều không âm và độc lập nên việc lấy lợi nhuận còn lại lớn nhất ở mỗi bước sẽ tối đa hóa mọi tổng tiền tố. Sắp xếp tất cả các đóng góp bổ sung theo thứ tự giảm dần và cộng từng cái một sẽ tạo ra chuỗi câu trả lời tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        w = list(map(int, input().split()))

        deg = [0] * n

        for _ in range(n - 1):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            deg[u] += 1
            deg[v] += 1

        total = sum(w)

        extra = []
        for i in range(n):
            extra.extend([w[i]] * (deg[i] - 1))

        extra.sort(reverse=True)

        ans = [total]
        cur = total

        for x in extra:
            cur += x
            ans.append(cur)

        out.append(" ".join(map(str, ans[:n - 1])))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Mảng độ là cấu trúc dữ liệu quan trọng. Sau khi đọc cây, mỗi đỉnh đóng góp trọng lượng của nó vào`extra`liệt kê chính xác`degree - 1`lần. 

Tổng số phần tử được chèn là$$\sum (\deg(v)-1)=2(n-1)-n=n-2$$vì vậy kích thước danh sách vẫn tuyến tính. 

Việc sắp xếp danh sách sẽ đưa ra thứ tự tối ưu để sử dụng các khoản đóng góp bổ sung. Chúng tôi bắt đầu từ tổng trọng lượng đỉnh và tiếp tục cộng từng phần đóng góp một. Vì có chính xác$n-2$đóng góp thêm, chúng tôi có được chính xác$1 + (n-2) = n-1$câu trả lời. 

Số nguyên Python tự động xử lý các giá trị lớn có liên quan. Câu trả lời tối đa có thể vượt quá phạm vi 32 bit vì trọng số lên tới$10^9$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
3 5 4 6
2 1
3 1
4 3
```Bằng cấp là: 

| Đỉnh | Cân nặng | Bằng cấp | Bản sao bổ sung | 
| --- | --- | --- | --- | 
| 1 | 3 | 2 | 1 | 
| 2 | 5 | 1 | 0 | 
| 3 | 4 | 2 | 1 | 
| 4 | 6 | 1 | 0 | 

Số tiền ban đầu:$$18$$Danh sách bổ sung:$$[3,4]$$Sắp xếp giảm dần:$$[4,3]$$| Bước | Giá trị gia tăng | Tổng hiện tại | Trả lời được sản xuất | 
| --- | --- | --- | --- | 
| Ban đầu | 0 | 18 | 18 | 
| 1 | 4 | 22 | 22 | 
| 2 | 3 | 25 | 25 | 

Đầu ra:```
18 22 25
```Ví dụ này cho thấy mỗi đỉnh không có lá đóng góp thêm một bản sao trọng lượng của nó như thế nào. 

### Ví dụ 2 

đầu vào:```
6
20 13 17 13 13 11
2 1
3 1
4 1
5 1
6 1
```Đây là một ngôi sao có tâm ở đỉnh 1. 

| Đỉnh | Cân nặng | Bằng cấp | Bản sao bổ sung | 
| --- | --- | --- | --- | 
| 1 | 20 | 5 | 4 | 
| 2 | 13 | 1 | 0 | 
| 3 | 17 | 1 | 0 | 
| 4 | 13 | 1 | 0 | 
| 5 | 13 | 1 | 0 | 
| 6 | 11 | 1 | 0 | 

Số tiền ban đầu:$$87$$Danh sách bổ sung:$$[20,20,20,20]$$| Bước | Giá trị gia tăng | Tổng hiện tại | 
| --- | --- | --- | 
| Ban đầu | 0 | 87 | 
| 1 | 20 | 107 | 
| 2 | 20 | 127 | 
| 3 | 20 | 147 | 
| 4 | 20 | 167 | 

Đầu ra:```
87 107 127 147 167
```Dấu vết này làm nổi bật sự quan sát trung tâm. Một đỉnh bậc năm có thể đóng góp tổng trọng lượng của nó năm lần, một lần lúc đầu và bốn lần sau đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp danh sách đóng góp thêm chiếm ưu thế | 
| Không gian | O(n) | Mảng bằng cấp và danh sách đóng góp bổ sung | 

Tổng số đỉnh của tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$. Sắp xếp tối đa$n-2$đóng góp bổ sung cho mỗi trường hợp thử nghiệm mang lại độ phức tạp tổng thể của$O(n \log n)$, dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    out = []

    t = int(input())
    for _ in range(t):
        n = int(input())
        w = list(map(int, input().split()))

        deg = [0] * n

        for _ in range(n - 1):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            deg[u] += 1
            deg[v] += 1

        total = sum(w)

        extra = []
        for i in range(n):
            extra.extend([w[i]] * (deg[i] - 1))

        extra.sort(reverse=True)

        ans = [total]
        cur = total

        for x in extra:
            cur += x
            ans.append(cur)

        out.append(" ".join(map(str, ans[:n - 1])))

    return "\n".join(out)

# provided samples
assert run(
"""4
4
3 5 4 6
2 1
3 1
4 3
2
21 32
2 1
6
20 13 17 13 13 11
2 1
3 1
4 1
5 1
6 1
4
10 6 6 6
1 2
2 3
4 1
"""
) == (
"""18 22 25
53
87 107 127 147 167
28 38 44"""
), "sample"

# minimum tree
assert run(
"""1
2
7 9
1 2
"""
) == "16"

# path graph
assert run(
"""1
4
10 10 10 10
1 2
2 3
3 4
"""
) == "40 50 60"

# star graph
assert run(
"""1
5
100 1 1 1 1
1 2
1 3
1 4
1 5
"""
) == "104 204 304 404"

# all weights zero
assert run(
"""1
4
0 0 0 0
1 2
2 3
3 4
"""
) == "0 0 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai đỉnh | 16 | Cây hợp lệ tối thiểu | 
| Đường đi có độ dài 3 | 40 50 60 | Các đỉnh bên trong đóng góp một lần nữa | 
| Ngôi sao có tâm ở đỉnh nặng | 104 204 304 404 | Bằng cấp cao tạo ra nhiều đóng góp bổ sung | 
| Tất cả trọng lượng bằng 0 | 0 0 0 | Xử lý chính xác các đóng góp có giá trị bằng 0 | 

## Vỏ cạnh 

Xét cây nhỏ nhất:```
1
2
21 32
1 2
```Tổng số tiền là$53$. Cả hai đỉnh đều có bậc$1$, do đó không có đóng góp thêm. Danh sách bổ sung trống và thuật toán chỉ xuất ra:```
53
```điều đó đúng bởi vì chỉ$k=1$tồn tại. 

Hãy xem xét một con đường:```
1
4
10 10 10 10
1 2
2 3
3 4
```Bằng cấp là$[1,2,2,1]$. Danh sách bổ sung trở thành$[10,10]$. Bắt đầu từ$40$, chúng ta cộng từng phần đóng góp này và thu được:```
40 50 60
```Các lá không bao giờ xuất hiện trong danh sách bổ sung vì mức độ của chúng là một. Điều này ngăn ngừa việc đếm quá mức. 

Hãy xem xét một ngôi sao:```
1
5
100 1 1 1 1
1 2
1 3
1 4
1 5
```Trung tâm có bằng cấp$4$, do đó trọng lượng của nó xuất hiện$4-1=3$lần trong danh sách bổ sung. Danh sách là:```
100 100 100
```Bắt đầu từ$104$, phép cộng liên tiếp tạo ra:```
104 204 304 404
```Thuật toán nắm bắt chính xác thực tế là một đỉnh bậc cao có thể đóng góp nhiều lần, trong khi các lá chỉ đóng góp một lần.
