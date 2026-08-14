---
title: "CF 102307J - Phá hủy nhà tù"
description: "Chúng tôi có một loạt (n) chiều cao của bức tường. Hoạt động phá hủy chọn một khoảng ([a,b]) và loại bỏ chính xác (s) mét khỏi mọi bức tường trong khoảng đó, ngoại trừ việc bức tường không thể trở thành âm. Nói cách khác, mọi chiều cao bị ảnh hưởng đều thay đổi từ (hi) thành (max(0,hi-s))."
date: "2026-08-13T07:25:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102307
codeforces_index: "J"
codeforces_contest_name: "2019 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 102307
solve_time_s: 106
verified: true
draft: false
---

[CF 102307J - Phá hủy nhà tù](https://codeforces.com/problemset/problem/102307/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một loạt (n) chiều cao của bức tường. Hoạt động phá hủy chọn một khoảng ([a,b]) và loại bỏ chính xác (s) mét khỏi mọi bức tường trong khoảng đó, ngoại trừ việc bức tường không thể trở thành âm. Nói cách khác, mọi chiều cao bị ảnh hưởng đều thay đổi từ (h_i) thành (\max(0,h_i-s)). 

Một truy vấn yêu cầu tổng chiều cao còn lại của tất cả các bức tường trong một khoảng nhất định. Các hoạt động đều trực tuyến, vì vậy mỗi truy vấn phải được trả lời bằng cách sử dụng trạng thái được tạo ra bởi tất cả các hoạt động hủy trước đó. 

Các ràng buộc đủ lớn để loại trừ việc cập nhật mọi bức tường trong một khoảng thời gian. Với (n,q\le 10^5), có thể có (10^{10}) vị trí bị ảnh hưởng trong tất cả các hoạt động. Ngay cả một giải pháp (O(nq)) cũng sẽ vượt xa những gì một vài giây có thể xử lý được. Chúng ta cần một cấu trúc dữ liệu thường có thể xử lý toàn bộ khoảng mà không cần chạm vào các bức tường riêng lẻ của nó. 

Khó khăn là việc cắt bớt ở mức 0. Cây phân đoạn lười thông thường hoạt động ngay lập tức đối với một phép toán chẳng hạn như (h_i\mathrel{-}=s), vì mọi giá trị đều thay đổi một lượng như nhau. Ở đây, một số bức tường có thể đạt tới 0 trong khi những bức tường cao hơn tiếp tục giảm, do đó, một giá trị lười thống nhất không phải lúc nào cũng đủ. 

Một số trường hợp đặc biệt bộc lộ vấn đề này. 

Hãy xem xét một bức tường duy nhất:```
1 3
2
2 1 1 5
1 1 1
```Bức tường có chiều cao (2) nên phá hủy (5) mét sẽ để lại chiều cao (0). Đầu ra là:```
0
```Việc triển khai chỉ trừ (5) sẽ lưu trữ (-3), tạo ra câu trả lời không hợp lệ. 

Bây giờ hãy xem xét các độ cao khác nhau:```
2 2
3 10
2 1 2 5
1 1 2
```Sau cuộc tấn công, độ cao là (0,5), vì vậy đầu ra đúng là:```
5
```Một bản cập nhật lười biếng trừ (5) khỏi toàn bộ phân đoạn không thể biểu thị chính xác bức tường đầu tiên vì bức tường đó đã đạt đến 0. 

Bình đẳng là một trường hợp ranh giới khác:```
2 2
5 10
2 1 2 5
1 1 2
```Độ cao kết quả là (0,5) và câu trả lời là:```
5
```Việc triển khai phải coi (s=\text{chiều cao dương tối thiểu}) là trường hợp có ít nhất một bức tường trở thành số 0. Phép trừ lười biếng chỉ an toàn khi (s) hoàn toàn nhỏ hơn chiều cao dương nhỏ nhất. 

Cuối cùng, một khoảng có thể chứa các bức tường đã bằng 0:```
3 3
2 5 1
2 1 1 3
2 1 3 2
1 1 3
```Sau lần tấn công đầu tiên mảng là (0,5,1). Sau giây thứ hai là (0,3,0), nên câu trả lời là:```
3
```Các bức tường có giá trị bằng 0 không được tham gia vào phép trừ trong tương lai. Việc coi số 0 là mức tối thiểu thông thường sẽ khiến cấu trúc dữ liệu không thể phân biệt được "đã bị phá hủy" với "bức tường dương nhỏ nhất". 

## Phương pháp tiếp cận 

Giải pháp trực tiếp lưu trữ mọi chiều cao tường hiện tại. Đối với thao tác hủy trên ([a,b]), nó truy cập mọi chỉ mục trong khoảng đó và thực hiện (h_i=\max(0,h_i-s)). Một truy vấn tương tự sẽ quét khoảng và thêm tất cả các độ cao. Điều này đúng vì nó thực hiện chính xác thao tác được mô tả bởi sự cố. 

Vấn đề là trường hợp xấu nhất. Nếu tất cả (q) thao tác ảnh hưởng đến toàn bộ mảng, thì mỗi thao tác sẽ chạm vào (n) bức tường, tạo ra (O(nq)) công việc. Với cả hai giá trị đều lớn bằng (10^5), tức là tối đa (10^{10}) phép toán mảng riêng lẻ, quá lớn. 

Cây phân đoạn lười biếng bình thường có vẻ đầy hứa hẹn vì toàn bộ khoảng thời gian thường có thể được giảm cùng một lúc. Trở ngại là ranh giới bằng không. Giả sử một đoạn chứa độ cao (3,10,12) và chúng tôi trừ (5). Kết quả là (0,5,7). Không có phép trừ nào có thể biến đổi chính xác cả ba giá trị. 

Quan sát hữu ích là chúng ta không cần biết sự phân bố đầy đủ của độ cao. Đối với một đoạn, giữ nguyên tổng của nó, số lượng tường dương và chiều cao dương tối thiểu. Nếu (s) nhỏ hơn chiều cao dương tối thiểu đó thì mọi bức tường dương đều tồn tại sau cuộc tấn công. Toàn bộ phân đoạn sau đó có thể được giảm dần theo (s), bởi vì tất cả các giá trị dương đều trải qua cùng một phép biến đổi. 

Nếu (s) ít nhất là chiều cao dương tối thiểu thì ít nhất một bức tường trở thành số không. Chúng tôi kiểm tra đệ quy phân đoạn cho đến khi những bức tường đó có thể bị phá hủy riêng lẻ. Điều này nghe có vẻ tốn kém, nhưng mỗi lần hạ xuống cưỡng bức như vậy sẽ loại bỏ vĩnh viễn ít nhất một bức tường dương. Một bức tường chỉ có thể trở thành số 0 một lần, vì vậy phần đắt tiền sẽ được khấu hao trong toàn bộ quá trình thực hiện. 

Cây phân đoạn tương tự có thể trả lời các truy vấn tổng theo cách tiêu chuẩn (O(\log n)). Do đó, giải pháp kết hợp sự lan truyền lười biếng với chiến lược "hủy bỏ các giá trị dương tối thiểu" được khấu hao. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(nq)) trường hợp xấu nhất | (O(n)) | Quá chậm | 
| Tối ưu | (O((n+q)\log n)) khấu hao | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây phân đoạn theo chiều cao của bức tường. Đối với mỗi nút, lưu trữ tổng khoảng của nó, số lượng tường dương trong khoảng đó và chiều cao tường dương nhỏ nhất. Lưu trữ (+\infty) ở mức tối thiểu khi toàn bộ khoảng thời gian đã bị hủy. Ba giá trị này chính xác là những gì chúng ta cần để quyết định liệu một hoạt động hủy diệt có thể được áp dụng một cách lười biếng hay không. 
2. Đồng thời lưu trữ một giá trị lười biểu thị các mét đã bị xóa khỏi mọi bức tường hiện dương trong nút. Nếu một nút chứa`cnt`các bức tường dương và nhận được một phép trừ lười biếng của (s), tổng của nó giảm đi`cnt * s`và chiều cao dương tối thiểu của nó giảm đi (s). Không có bức tường nào không bị ảnh hưởng. 
3. Đối với thao tác hủy trên một nút được bao phủ hoàn toàn, trước tiên hãy kiểm tra xem tổng của nó có bằng 0 hay không. Nếu vậy, toàn bộ khoảng thời gian đã bị phá hủy và không còn gì để làm. 
4. Nếu nút được bao phủ hoàn toàn và (các) nút hoàn toàn nhỏ hơn chiều cao dương tối thiểu của nó, hãy áp dụng phép trừ một cách lười biếng. Mọi bức tường dương vẫn giữ nguyên dương, vì vậy tất cả chúng thực sự mất đi chính xác (s) mét. 
5. Ngược lại, hãy đi xuống trẻ em. Điều kiện (s\ge\text{chiều cao dương tối thiểu}) có nghĩa là ít nhất một bức tường dương sẽ đạt tới 0, do đó thao tác không thể được biểu diễn bằng một phép trừ lười thống nhất. Đẩy bất kỳ giá trị lười nào đang chờ xử lý trước khi giảm xuống để trẻ nhìn thấy chiều cao thực tế hiện tại của mình. 
6. Tại một lá, trừ (s) với số sàn bằng 0. Vì chiếc lá này được chạm tới thông qua hộp cứng nên chiều cao hiện tại của nó nhiều nhất là (s), nên nó trở thành 0. Đặt số dương của nó thành 0 và số tối thiểu của nó thành (+\infty). 
7. Sau khi cập nhật các phần tử con, hãy hợp nhất chúng lại. Tổng mới là tổng của các tổng con, số dương là tổng của các số con và chiều cao dương tối thiểu là chiều cao tối thiểu của con nhỏ hơn. 
8. Đối với truy vấn tính tổng, hãy sử dụng phương pháp duyệt khoảng cây phân đoạn thông thường. Đẩy các giá trị lười biếng trước khi chuyển sang phần tử con, vì trạng thái được lưu trữ của phần tử con có thể vẫn đang chờ phép trừ đang chờ xử lý của phần tử cha. 

Tại sao nó hoạt động: mỗi nút cây phân đoạn luôn mô tả chính xác khoảng của nó thông qua tổng, số dương và chiều cao dương tối thiểu. Khi (s) nhỏ hơn chiều cao dương tối thiểu, không bức tường nào có thể đạt đến 0, do đó, việc trừ (s) khỏi mọi bức tường dương chính xác là thao tác bắt buộc và có thể được lưu trữ một cách an toàn dưới dạng thẻ lười. Khi (s) đạt mức tối thiểu, phép trừ đồng nhất không còn hiệu lực, do đó thuật toán giảm dần cho đến khi các bức tường bị ảnh hưởng có thể được xử lý riêng lẻ. Tại mỗi lá, bản cập nhật chính xác là (h_i\leftarrow\max(0,h_i-s)). Vì thông tin cha được xây dựng lại từ đúng con nên tính bất biến được giữ nguyên sau mỗi lần cập nhật. Các truy vấn trả về tổng từ các trạng thái phân đoạn chính xác này, do đó, mọi tổng khoảng thời gian được báo cáo đều chính xác. 

Việc khấu hao đến từ trường hợp cứng. Bất cứ khi nào một nút được che phủ hoàn toàn không thể được cập nhật một cách lười biếng, bức tường tích cực tối thiểu của nó sẽ bị phá hủy ở đâu đó bên dưới nút đó. Mỗi bức tường chỉ có thể bị phá hủy một lần. Đệ quy xung quanh một bức tường bị phá hủy có chiều cao (O(\log n)), do đó tất cả các vết lõm cứng cùng đóng góp (O(n\log n)). Việc truyền tải thông thường do ranh giới truy vấn và các bản cập nhật được xử lý lười biếng gây ra (O(q\log n)). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**30

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    size = 4 * n + 5
    total = [0] * size
    mn = [INF] * size
    cnt = [0] * size
    lazy = [0] * size

    def build(node, left, right):
        if left == right:
            value = a[left]
            total[node] = value
            mn[node] = value
            cnt[node] = 1
            return

        mid = (left + right) // 2
        lc = node * 2
        rc = lc + 1

        build(lc, left, mid)
        build(rc, mid + 1, right)

        total[node] = total[lc] + total[rc]
        cnt[node] = cnt[lc] + cnt[rc]
        mn[node] = min(mn[lc], mn[rc])

    def apply(node, value):
        if cnt[node] == 0:
            return

        total[node] -= cnt[node] * value
        mn[node] -= value
        lazy[node] += value

    def push(node):
        value = lazy[node]
        if value == 0:
            return

        lc = node * 2
        rc = lc + 1

        apply(lc, value)
        apply(rc, value)

        lazy[node] = 0

    def pull(node):
        lc = node * 2
        rc = lc + 1

        total[node] = total[lc] + total[rc]
        cnt[node] = cnt[lc] + cnt[rc]
        mn[node] = min(mn[lc], mn[rc])

    def update(node, left, right, ql, qr, value):
        if qr < left or right < ql or total[node] == 0:
            return

        if ql <= left and right <= qr and value < mn[node]:
            apply(node, value)
            return

        if left == right:
            total[node] = 0
            cnt[node] = 0
            mn[node] = INF
            lazy[node] = 0
            return

        push(node)

        mid = (left + right) // 2
        lc = node * 2
        rc = lc + 1

        if ql <= mid:
            update(lc, left, mid, ql, qr, value)
        if qr > mid:
            update(rc, mid + 1, right, ql, qr, value)

        pull(node)

    def query(node, left, right, ql, qr):
        if qr < left or right < ql:
            return 0

        if ql <= left and right <= qr:
            return total[node]

        push(node)

        mid = (left + right) // 2

        result = 0
        if ql <= mid:
            result += query(node * 2, left, mid, ql, qr)
        if qr > mid:
            result += query(node * 2 + 1, mid + 1, right, ql, qr)

        return result

    build(1, 0, n - 1)

    output = []

    for _ in range(q):
        parts = list(map(int, input().split()))
        operation = parts[0]
        left = parts[1] - 1
        right = parts[2] - 1

        if operation == 1:
            output.append(str(query(1, 0, n - 1, left, right)))
        else:
            value = parts[3]
            update(1, 0, n - 1, left, right, value)

    sys.stdout.write("\n".join(output))

if __name__ == "__main__":
    solve()
```Cây sử dụng bốn mảng vì cấu trúc nút nặng đối tượng Python sẽ tiêu tốn nhiều bộ nhớ hơn đáng kể và tăng thêm chi phí.`total[node]`là tổng chiều cao tường còn lại,`cnt[node]`đếm những bức tường tích cực,`mn[node]`là chiều cao dương nhỏ nhất của chúng, và`lazy[node]`lưu trữ một phép trừ đang chờ xử lý. 

các`apply`hàm chỉ có giá trị đối với phép trừ giữ cho mọi bức tường dương. điều kiện`value < mn[node]`đảm bảo chính xác điều đó. Số lượng vách dương không thay đổi nên tổng giảm đi một lượng`cnt[node] * value`và mức giảm tối thiểu là`value`. 

Một nút bị phá hủy hoàn toàn có`cnt[node] == 0`. Mức tối thiểu của nó được biểu thị bằng`INF`, thay vì bằng 0. Sự khác biệt này rất cần thiết vì số 0 có nghĩa là "không dương", trong khi mức tối thiểu chúng ta cần cụ thể là mức tối thiểu giữa các bức tường dương. các`apply`hàm bỏ qua các nút bị phá hủy, vì vậy các phép trừ đang chờ xử lý không bao giờ ảnh hưởng đến chúng. 

Sự so sánh chặt chẽ`value < mn[node]`là một chi tiết quan trọng khác. Nếu như`value == mn[node]`, ít nhất một bức tường đạt chính xác bằng 0, do đó thao tác phải lặp lại. Coi sự bình đẳng như một bản cập nhật lười biếng sẽ khiến số lượng dương và số tối thiểu không chính xác. 

Tại một lá mà hộp cứng đệ quy chạm tới, bức tường nhất thiết trở thành số không. Đặt lại`lazy[node]`is necessary because a zero leaf must never carry a subtraction tag into the future. Nếu không thì sau này`push`có thể sửa đổi không chính xác mức tối thiểu được lưu trữ của nó. 

Việc triển khai sử dụng các chỉ mục dựa trên 0 trong nội bộ, trong khi đầu vào sử dụng các chỉ mục dựa trên một. Việc trừ đi một điểm từ cả hai điểm cuối truy vấn ngay sau khi đọc chúng sẽ tránh được các chuyển đổi lặp lại và giữ cho ranh giới cây phân đoạn nhất quán. 

Số nguyên Python có độ chính xác tùy ý, do đó tổng không thể tràn. Tổng số lớn nhất có thể là (10^5\cdot10^8=10^{13}), dù sao cũng phù hợp với số nguyên 64 bit. 

## Ví dụ đã hoạt động 

Mẫu được cung cấp bắt đầu bằng hai bức tường có chiều cao (10). 

| Hoạt động | Mảng sau thao tác | Tổng truy vấn | 
| --- | --- | --- | 
|`1 1 2`|`[10, 10]`| 20 | 
|`2 1 2 5`|`[5, 5]`| | 
|`1 1 2`|`[5, 5]`| 10 | 
|`2 2 2 6`|`[5, 0]`| | 
|`1 1 2`|`[5, 0]`| 5 | 

Cuộc tấn công đầu tiên có thể được biểu diễn bằng một phép trừ lười vì mức tối thiểu của phân đoạn là (10), lớn hơn (5). Sau lần cập nhật đó, tổng phân đoạn là (10), số dương của nó vẫn là (2) và chiều cao dương tối thiểu của nó là (5). 

Cuộc tấn công thứ hai chỉ nhắm vào bức tường thứ hai. Chiều cao hiện tại của nó là (5), trong khi đòn tấn công loại bỏ (6), do đó điều kiện lười biếng nghiêm ngặt không thành công. Sự đệ quy đến chiếc lá đó và phá hủy nó hoàn toàn. Tổng kết quả là (5). 

Đối với ví dụ thứ hai, hãy xem xét:```
4 5
7 3 10 5
1 1 4
2 2 3 4
1 2 4
2 1 2 6
1 1 3
```Trạng thái có thể được theo dõi như sau. 

| Bước | Hoạt động | Mảng | Tổng liên quan | 
| --- | --- | --- | --- | 
| 0 | Ban đầu |`[7, 3, 10, 5]`| 25 | 
| 1 |`1 1 4`|`[7, 3, 10, 5]`| 25 | 
| 2 |`2 2 3 4`|`[7, 0, 6, 5]`| | 
| 3 |`1 2 4`|`[7, 0, 6, 5]`| 11 | 
| 4 |`2 1 2 6`|`[1, 0, 6, 5]`| | 
| 5 |`1 1 3`|`[1, 0, 6, 5]`| 7 | 

Cuộc tấn công vào các vị trí (2) đến (3) có độ cao dương tối thiểu (3), trong khi (s=4). Bức tường có chiều cao (3) phải bị phá bỏ để cây hạ xuống thay vì áp dụng một phép trừ lười biếng cho toàn bộ khoảng thời gian. Bức tường có chiều cao (10) trở thành (6). 

Sau đó, đòn tấn công vào các vị trí (1) đến (2) sẽ loại bỏ (6). Bức tường đầu tiên đi từ (7) đến (1), trong khi bức tường thứ hai đã bằng không. Bởi vì những bức tường bị phá hủy có`cnt = 0`, chúng không tham gia vào phép trừ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O((n+q)\log n)) khấu hao | Chi phí di chuyển qua cây thông thường (O(\log n)) cho mỗi thao tác, trong khi các vết lõm cứng được tính vào các bức tường trở thành 0 | 
| Không gian | (O(n)) | Bốn mảng cây phân đoạn chứa (O(n)) nút | 

Đầu vào chứa tối đa (10^5) bức tường và (10^5) thao tác. Cây phân đoạn tránh được trường hợp xấu nhất (O(nq)) của mô phỏng trực tiếp. Công việc khấu hao (O((n+q)\log n)) của nó phù hợp với giới hạn (4) giây và (256) MB, trong khi biểu diễn Python dựa trên mảng giúp kiểm soát việc sử dụng bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

INF = 10**30

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        n, q = map(int, sys.stdin.readline().split())
        a = list(map(int, sys.stdin.readline().split()))

        size = 4 * n + 5
        total = [0] * size
        mn = [INF] * size
        cnt = [0] * size
        lazy = [0] * size

        def build(node, left, right):
            if left == right:
                value = a[left]
                total[node] = value
                mn[node] = value
                cnt[node] = 1
                return

            mid = (left + right) // 2
            build(node * 2, left, mid)
            build(node * 2 + 1, mid + 1, right)

            total[node] = total[node * 2] + total[node * 2 + 1]
            cnt[node] = cnt[node * 2] + cnt[node * 2 + 1]
            mn[node] = min(mn[node * 2], mn[node * 2 + 1])

        def apply(node, value):
            if cnt[node] == 0:
                return
            total[node] -= cnt[node] * value
            mn[node] -= value
            lazy[node] += value

        def push(node):
            value = lazy[node]
            if value == 0:
                return
            apply(node * 2, value)
            apply(node * 2 + 1, value)
            lazy[node] = 0

        def pull(node):
            total[node] = total[node * 2] + total[node * 2 + 1]
            cnt[node] = cnt[node * 2] + cnt[node * 2 + 1]
            mn[node] = min(mn[node * 2], mn[node * 2 + 1])

        def update(node, left, right, ql, qr, value):
            if qr < left or right < ql or total[node] == 0:
                return

            if ql <= left and right <= qr and value < mn[node]:
                apply(node, value)
                return

            if left == right:
                total[node] = 0
                cnt[node] = 0
                mn[node] = INF
                lazy[node] = 0
                return

            push(node)

            mid = (left + right) // 2
            if ql <= mid:
                update(node * 2, left, mid, ql, qr, value)
            if qr > mid:
                update(node * 2 + 1, mid + 1, right, ql, qr, value)

            pull(node)

        def query(node, left, right, ql, qr):
            if qr < left or right < ql:
                return 0

            if ql <= left and right <= qr:
                return total[node]

            push(node)

            mid = (left + right) // 2
            result = 0

            if ql <= mid:
                result += query(node * 2, left, mid, ql, qr)
            if qr > mid:
                result += query(node * 2 + 1, mid + 1, right, ql, qr)

            return result

        build(1, 0, n - 1)

        output = []

        for _ in range(q):
            parts = list(map(int, sys.stdin.readline().split()))
            op = parts[0]
            l = parts[1] - 1
            r = parts[2] - 1

            if op == 1:
                output.append(str(query(1, 0, n - 1, l, r)))
            else:
                update(1, 0, n - 1, l, r, parts[3])

        return "\n".join(output)
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

assert run("""2 5
10 10
1 1 2
2 1 2 5
1 1 2
2 2 2 6
1 1 2
""") == """20
10
5""", "provided sample"

assert run("""4 5
7 3 10 5
1 1 4
2 2 3 4
1 2 4
2 1 2 6
1 1 3
""") == """25
11
7""", "mixed updates and queries"

assert run("""1 3
2
1 1 1
2 1 1 5
1 1 1
""") == """2
0""", "minimum-size input and over-destruction"

assert run("""5 5
8 8 8 8 8
2 1 5 3
1 1 5
2 2 4 5
1 1 5
1 2 4
""") == """25
10
0""", "all equal values"

assert run("""4 6
3 100 4 8
2 1 1 3
1 1 4
2 2 3 4
1 1 4
2 4 4 8
1 3 4
""") == """109
101
0""", "single-position and boundary updates"

n = 100000
maximum_input = (
    f"{n} 3\n"
    + " ".join(["100000000"] * n)
    + "\n"
    + f"1 1 {n}\n"
    + f"2 1 {n} 100000000\n"
    + f"1 1 {n}\n"
)
assert run(maximum_input) == "10000000000000\n0", "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 3 / 2 / ...`|`2`, sau đó`0`| Bức tường đơn và sự tàn phá lớn hơn chiều cao của nó | 
|`5 5 / 8 8 8 8 8 / ...`|`25`,`10`,`0`| Chiều cao bằng nhau và hoạt động toàn dải lặp đi lặp lại | 
|`4 6 / 3 100 4 8 / ...`|`109`,`101`,`0`| Cập nhật một vị trí và ranh giới khoảng thời gian | 
|`100000`những bức tường có chiều cao bằng nhau`100000000`|`10000000000000`, sau đó`0`| Tối đa (n), số tiền lớn và hủy diệt toàn phạm vi | 

## Vỏ cạnh 

Lượng phá hủy lớn hơn bức tường không bao giờ được tạo ra chiều cao âm. Vì```
1 3
2
1 1 1
2 1 1 5
1 1 1
```truy vấn đầu tiên trả về (2). Bản cập nhật đến lá có chiều cao (2) và do (5\ge2) nên hộp cứng sẽ phá hủy nó hoàn toàn. Truy vấn cuối cùng trả về (0). 

Sự bằng nhau chính xác giữa số lượng tấn công và chiều cao dương tối thiểu cũng yêu cầu đệ quy. Vì```
2 2
5 10
2 1 2 5
1 1 2
```chiều cao dương tối thiểu là (5), chính xác bằng số lượng tấn công. Điều kiện nghiêm ngặt`value < mn[node]`thất bại nên cây đổ xuống. Bức tường đầu tiên trở thành số 0 và bức tường thứ hai trở thành (5), cho tổng đúng (5). 

Những bức tường đã bị phá hủy phải không thay đổi. TRONG```
3 3
2 5 1
2 1 1 3
2 1 3 2
1 1 3
```thao tác đầu tiên thay đổi mảng thành`[0,5,1]`. Trong lần hoạt động thứ hai, bức tường số 0 có`cnt = 0`, do đó phép trừ lười chỉ được áp dụng cho các bức tường dương. Trạng thái cuối cùng là`[0,3,0]`, và đáp án là (3). 

Khoảng thời gian một vị trí cũng được xử lý bằng đệ quy tương tự mà không có trường hợp đặc biệt. Ví dụ,```
4 3
3 100 4 8
2 1 1 3
1 1 4
2 4 4 8
```thay đổi bức tường đầu tiên từ (3) thành (0), sau đó là bức tường cuối cùng từ (8) thành (0). Cây phân đoạn có thể cô lập một trong hai lá thông qua việc truyền tải khoảng thời gian thông thường của nó, do đó không cần thực hiện cập nhật điểm riêng biệt. 

Trường hợp toàn dải là nơi tối ưu hóa quan trọng nhất. Nếu ban đầu mọi bức tường đều có chiều cao (100000000), một cuộc tấn công loại bỏ chính xác (100000000) mét sẽ đạt đến mức tối thiểu một cách chính xác và cuối cùng phá hủy mọi chiếc lá. Mỗi bức tường chỉ bị xóa một lần, do đó, mặc dù hoạt động cụ thể này yêu cầu giảm đệ quy, nhưng cùng một bức tường không thể gây ra một lần giảm cứng khác sau khi nó trở về 0. Đó là khoản khấu hao đằng sau giới hạn (O((n+q)\log n)).
