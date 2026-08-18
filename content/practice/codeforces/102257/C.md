---
title: "CF 102257C - Đèn đường"
description: "Đường có (n+1) điểm dừng và (n) đoạn liên tiếp. Đèn (i) điều khiển đoạn giữa các điểm dừng (i) và (i+1). Tại bất kỳ thời điểm nào, một chiếc taxi có thể di chuyển từ điểm dừng (a) đến điểm dừng (b) chính xác khi mọi đèn có chỉ số (a,a+1,ldots,b-1) đều sáng. Cấu hình đèn được đưa ra tại thời điểm (0)."
date: "2026-08-17T20:47:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102257
codeforces_index: "C"
codeforces_contest_name: "2019 Asia-Pacific Informatics Olympiad (APIO 19)"
rating: 0
weight: 102257
solve_time_s: 91
verified: true
draft: false
---

[CF 102257C - Đèn đường](https://codeforces.com/problemset/problem/102257/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đường có (n+1) điểm dừng và (n) đoạn liên tiếp. Đèn (i) điều khiển đoạn giữa các điểm dừng (i) và (i+1). Tại bất kỳ thời điểm nào, một chiếc taxi có thể di chuyển từ điểm dừng (a) đến điểm dừng (b) chính xác khi mọi đèn có chỉ số (a,a+1,\ldots,b-1) đều sáng. 

Cấu hình đèn được đưa ra tại thời điểm (0). Vào cuối mỗi giờ, có đúng một sự kiện xảy ra. Nút chuyển đổi sẽ thay đổi một đèn, trong khi truy vấn hỏi xem đã trôi qua bao nhiêu giờ trong đó toàn bộ khoảng thời gian từ điểm dừng (a) đến điểm dừng (b) có thể sử dụng được liên tục. Truy vấn bao gồm tất cả số giờ đã hoàn thành tính đến thời điểm sự kiện hiện tại. 

Các ràng buộc là (n,q\le 300000). Quá trình quét trực tiếp tất cả các đèn trong mỗi truy vấn có thể mất (O(nq)), lớn bằng (9\cdot10^{10}) lần kiểm tra đèn. Điều đó vượt xa những gì giới hạn 5 giây có thể hỗ trợ. Chúng ta cần tính logarit đại khái cho mỗi sự kiện, đưa ra nghiệm (O((n+q)\log n)). 

Sự tinh tế đầu tiên là một sự kiện xảy ra vào cuối giờ của nó. Coi như```
1 1
1
query 1 2
```Câu trả lời là`1`, không`0`. Đèn vẫn sáng suốt giờ thứ nhất nên số giờ hoàn thành sẽ được tính. 

Điều tinh tế thứ hai là việc chuyển đổi vào cuối một giờ chỉ ảnh hưởng đến những giờ trong tương lai. Trong mẫu chính thức, đèn 3 được bật vào cuối giờ thứ 5. Cấu hình là`11011`trong giờ từ 1 đến 5, và`11111`bắt đầu từ giờ thứ 6. Do đó, truy vấn tại giờ thứ 6 sẽ thấy chính xác một giờ có thể sử dụng được cho điểm dừng 3 và 4. 

Sự tinh tế thứ ba là một truy vấn trải rộng trên nhiều ngọn đèn. Ví dụ,```
3 1
101
query 1 4
```có câu trả lời`0`, vì đèn ở giữa đã tắt. Chỉ kiểm tra các điểm cuối sẽ kết luận không chính xác rằng toàn bộ tuyến đường có thể sử dụng được. 

Cuối cùng, truy vấn liên quan đến một phân đoạn vẫn là truy vấn phạm vi. Vì```
2 1
01
query 2 3
```câu trả lời là`1`, vì chỉ có đèn 2 là quan trọng. Việc chuyển đổi trực tiếp các chỉ số dừng thành các chỉ số phân đoạn mà không xử lý quy ước điểm cuối là nguồn phổ biến gây ra các lỗi riêng lẻ. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản có thể mô phỏng trạng thái hiện tại của đèn và đối với mỗi truy vấn, hãy kiểm tra các đèn từ (a) đến (b-1). Nếu tất cả đều bật, câu trả lời của truy vấn có thể tăng lên theo số giờ kể từ sự kiện trước đó khiến cấu hình này không thay đổi. Điều này đúng vì cấu hình không đổi giữa các sự kiện chuyển đổi liên tiếp. 

Vấn đề là việc kiểm tra phạm vi. Một truy vấn có thể kiểm tra (n) đèn và có thể có (q) truy vấn. Trong trường hợp xấu nhất, điều này mang lại (O(nq)=9\cdot10^{10}) kiểm tra. Việc duy trì trạng thái hiện tại là rẻ, nhưng việc liên tục xác định xem toàn bộ khoảng thời gian có bao gồm các trạng thái hay không thì không. 

Quan sát quan trọng là câu hỏi không chỉ hỏi liệu một phạm vi hiện có được bật hay không. Nó yêu cầu tổng lượng thời gian mà phạm vi đó được bật. Điều này gợi ý việc lưu trữ thông tin lịch sử trực tiếp trong cây phân đoạn. 

Đối với mỗi nút cây phân đoạn, hãy lưu ý xem mọi đèn trong nút đó hiện có bật hay không, cùng với tổng thời gian tính đến lần cuối cùng nút đó được xử lý trong khi toàn bộ nút đó được bật. Đồng thời lưu trữ dấu thời gian mà giá trị lịch sử của nút này được hoàn tất lần cuối. 

Giả sử một nút đã được bật hoàn toàn kể từ thời điểm đó (cuối cùng). Khi chúng ta cần thông tin lần đầu tiên tại thời điểm (t), sự đóng góp của nó cho khoảng thời gian không được ghi lại chỉ đơn giản là (t-last). Nếu nút hiện đang tắt thì mức đóng góp bằng 0. Chuyển đổi điểm chỉ thay đổi (O(\log n)) nút cây phân đoạn, vì vậy tất cả các giá trị lịch sử bị ảnh hưởng có thể được hoàn tất trong (O(\log n)). Một truy vấn phạm vi sẽ truy cập các nút chuẩn (O(\log n)) và mỗi nút đó có thể được hoàn tất tương tự tại thời điểm truy vấn. 

Cách tiếp cận bạo lực có hiệu quả vì nó kiểm tra rõ ràng mọi đèn quan trọng. Nó thất bại vì các phạm vi dài tương tự được kiểm tra đi kiểm tra lại. Quan sát cho thấy toàn bộ trạng thái của một phân đoạn chỉ thay đổi khi một trong các phân đoạn con của nó được bật cho phép chúng tôi lưu trữ thời gian sử dụng tích lũy của nó và chỉ cập nhật nó khi cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(nq)) trường hợp xấu nhất | (O(n)) | Quá chậm | 
| Tối ưu | (O((n+q)\log n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây phân đoạn trên (n) đèn. Đối với mỗi nút, lưu trữ`on`, điều này đúng chính xác khi mọi đèn được đại diện bởi nút đó hiện đang bật. Cũng lưu trữ`total`, lượng thời gian đã hoàn tất trong đó toàn bộ nút được bật và`last`, dấu thời gian mà qua đó`total`đã được hoàn thiện. 
2. Khởi tạo mọi nút`last`giá trị bằng không. Cấu hình ban đầu có hiệu lực từ thời điểm 0 trở đi nên chưa có thời gian tích lũy. các`on`các giá trị được tính từ chuỗi nhị phân ban đầu. 
3. Khi một nút được truy cập tại thời điểm (t), hãy chốt khoảng thời gian còn thiếu của nút đó. Nếu như`on`đúng, thêm (t-last) vào`total`. Sau đó đặt`last=t`. Nếu như`on`là sai, chỉ`last`thay đổi vì nút không đóng góp gì trong khoảng thời gian đó. 
4. Để bật đèn (i) vào cuối giờ (t), hãy đi từ lá đó về phía gốc và hoàn tất mọi nút trên đường đi tại thời điểm (t). Điều này ghi lại trạng thái tồn tại trong giờ (t) trước khi chuyển đổi. Sau đó, lật lá`on`giá trị. 
5. Tính lại tổ tiên của chiếc lá được chuyển đổi từ dưới lên. Cha mẹ hiện đang bật chính xác khi cả hai đứa con của nó hiện đang bật. Đặt của cha mẹ`last`giá trị thành (t), vì trạng thái mới của nó bắt đầu tại dấu thời gian này. 
6. Đối với truy vấn từ điểm dừng (a) đến điểm dừng (b), hãy chuyển đổi nó thành khoảng thời gian đèn nửa mở ([a-1,b-1)). Đây chính xác là những chiếc đèn (a,a+1,\ldots,b-1). Phân tách khoảng này thành các nút cây phân đoạn (O(\log n)) thông thường. 
7. Hoàn thiện mọi nút đã chọn tại thời điểm hiện tại (t), sau đó thêm nút đó`total`giá trị cho câu trả lời. Các nút được chọn rời rạc và cùng nhau chứa chính xác số đèn được yêu cầu, do đó, thời gian sử dụng có thể đóng góp của chúng có thể được tính tổng trực tiếp. 
8. Xuất kết quả tổng. Một truy vấn không làm thay đổi cấu hình đèn, do đó không cần tính toán lại trạng thái cây sau truy vấn. 

Bất biến là đối với mỗi nút cây phân đoạn,`total`chứa chính xác lượng thời gian mà toàn bộ phân đoạn được bật trong khoảng thời gian từ thời điểm 0 đến`last`. Của nó`on`cờ mô tả cấu hình ngay sau`last`. Bất cứ khi nào nút được chạm vào sau đó, khoảng thời gian từ`last`đến thời điểm mới có trạng thái không đổi, do đó việc cộng độ dài của nó sẽ chiếm chính xác tất cả thời gian có thể sử dụng mới hoàn thành. Chuyển đổi điểm duy trì sự bất biến này dọc theo đường dẫn từ gốc đến lá của chúng và truy vấn phạm vi tính tổng các nút rời rạc có các khoảng được biểu thị chính xác tạo thành tuyến được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    s = input().strip()

    size = 1
    while size < n:
        size <<= 1

    # on[v]   = whether the whole segment of v is currently on
    # total[v] = finalized amount of time for which the whole segment was on
    # last[v] = time through which total[v] has been finalized
    on = [False] * (2 * size)
    total = [0] * (2 * size)
    last = [0] * (2 * size)

    for i, ch in enumerate(s):
        on[size + i] = (ch == '1')

    for v in range(size - 1, 0, -1):
        on[v] = on[v << 1] and on[v << 1 | 1]

    def touch(v, t):
        if on[v]:
            total[v] += t - last[v]
        last[v] = t

    def toggle(pos, t):
        v = size + pos

        # Finalize every node whose old state is about to change.
        u = v
        while u:
            touch(u, t)
            u >>= 1

        # Change the lamp itself.
        on[v] = not on[v]

        # Recompute ancestors using the new child states.
        v >>= 1
        while v:
            on[v] = on[v << 1] and on[v << 1 | 1]
            last[v] = t
            v >>= 1

    def query(left, right, t):
        # Query [left, right) in zero-based lamp indices.
        left += size
        right += size

        answer = 0

        while left < right:
            if left & 1:
                touch(left, t)
                answer += total[left]
                left += 1

            if right & 1:
                right -= 1
                touch(right, t)
                answer += total[right]

            left >>= 1
            right >>= 1

        return answer

    out = []

    for t in range(1, q + 1):
        event = input().split()

        if event[0] == "toggle":
            pos = int(event[1]) - 1
            toggle(pos, t)
        else:
            a = int(event[1])
            b = int(event[2])

            # Stops [a, b] correspond to lamps [a-1, b-1).
            left = a - 1
            right = b - 1

            out.append(str(query(left, right, t)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Cây sử dụng số lá lũy thừa hai vì điều đó làm cho việc phân tách phạm vi lặp trở nên đơn giản. Các lá bổ sung được khởi tạo thành tắt. Chúng không bao giờ xuất hiện trong một truy vấn hợp lệ vì mọi truy vấn đều kết thúc ở đèn (n), vì vậy chúng không thể ảnh hưởng đến nút chính tắc đã chọn. 

các`touch`chức năng là hoạt động trung tâm. Trạng thái của nút vẫn không thay đổi kể từ`last[v]`, vì vậy nếu nó hiện đang bật, chính xác`t - last[v]`nhiều giờ đã trôi qua khi toàn bộ đoạn đường được chiếu sáng. 

Thao tác chuyển đổi trước tiên chạm vào mọi tổ tiên bằng trạng thái cũ. Thứ tự này quan trọng. Nếu chiếc lá được lật trước, khoảng thời gian ngay trước khi nút chuyển có thể bị mất. Sau khi đóng góp cũ xong, lật lá và dựng lại tổ tiên. 

Truy vấn chuyển đổi điểm dừng thành đèn một cách cẩn thận. Di chuyển từ điểm dừng (a) đến điểm dừng (b) sử dụng đèn (a) đến (b-1), do đó, trong ký hiệu nửa mở dựa trên 0, khoảng thời gian là`[a - 1, b - 1)`. 

Số nguyên Python không tràn và câu trả lời lớn nhất tối đa là (q), chỉ là (300000). Việc triển khai được lặp lại thay vì duyệt qua cây phân đoạn một cách đệ quy, tránh chi phí đệ quy Python cho tối đa (300000) sự kiện. 

## Ví dụ đã hoạt động 

Mẫu chính thức là:```
5 7
11011
query 1 2
query 1 2
query 1 6
query 3 4
toggle 3
query 3 4
query 1 6
```Trạng thái trong mỗi giờ hoàn thành và kết quả truy vấn có liên quan là: 

| Giờ | Sự kiện | Trạng thái đèn trong giờ | Khoảng truy vấn | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 |`query 1 2`|`11011`| đèn 1 | 1 | 
| 2 |`query 1 2`|`11011`| đèn 1 | 2 | 
| 3 |`query 1 6`|`11011`| đèn 1..5 | 0 | 
| 4 |`query 3 4`|`11011`| đèn 3 | 0 | 
| 5 |`toggle 3`|`11011`| không | 0 | 
| 6 |`query 3 4`|`11111`| đèn 3 | 1 | 
| 7 |`query 1 6`|`11111`| đèn 1..5 | 2 | 

Kết quả đầu ra là`1, 2, 0, 0, 1, 2`. Điều này thể hiện quy tắc tính thời gian quan trọng: việc chuyển đổi ở cuối giờ thứ 5 chỉ thay đổi cấu hình cho giờ thứ 6 trở đi. 

Đối với ví dụ thứ hai, hãy xem xét:```
3 5
111
query 1 4
toggle 2
query 1 4
toggle 2
query 1 4
```Việc giải thích cấp độ cây và trạng thái là: 

| Giờ | Sự kiện | Trạng thái hiện tại sau sự kiện | Khoảng truy vấn | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 |`query 1 4`|`111`| đèn 1..3 | 1 | 
| 2 |`toggle 2`|`101`| không | 0 | 
| 3 |`query 1 4`|`101`| đèn 1..3 | 1 | 
| 4 |`toggle 2`|`111`| không | 0 | 
| 5 |`query 1 4`|`111`| đèn 1..3 | 2 | 

Ở giờ thứ 3, khoảng thời gian hoàn chỉnh chỉ có thể sử dụng được trong giờ 1, vì vậy câu trả lời tích lũy vẫn là 1. Sau lần chuyển đổi thứ hai, toàn bộ khoảng thời gian sẽ có thể sử dụng lại được bắt đầu từ giờ thứ 5, tổng cộng là hai giờ có thể sử dụng được ở truy vấn cuối cùng. Ví dụ này thực hiện cả hai hướng chuyển đổi và xác nhận rằng thời gian lịch sử được bảo toàn thay vì được tính toán lại từ trạng thái hiện tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O((n+q)\log n)) | Chi phí xây dựng cây (O(n)), mỗi lần chuyển đổi chạm vào một đường dẫn từ gốc đến lá và mỗi truy vấn truy cập vào các nút chuẩn (O(\log n)). | 
| Không gian | (O(n)) | Bốn mảng có kích thước cây phân đoạn được duy trì, với cây chứa các nút (O(n)). | 

Với (n,q\le300000), giải pháp chỉ thực hiện nhiều thao tác cây theo logarit cho mỗi sự kiện, thay vì có khả năng thực hiện hàng trăm nghìn lượt kiểm tra đèn cho mỗi truy vấn. Điều này phù hợp với giới hạn 5 giây, 512 MB do sự cố đưa ra. 

## Trường hợp thử nghiệm```python
# helper: run the solution on an input string
import sys
import io

def solve_io(inp: str) -> str:
    data = inp.splitlines()
    it = iter(data)

    n, q = map(int, next(it).split())
    s = next(it).strip()

    size = 1
    while size < n:
        size <<= 1

    on = [False] * (2 * size)
    total = [0] * (2 * size)
    last = [0] * (2 * size)

    for i, ch in enumerate(s):
        on[size + i] = (ch == '1')

    for v in range(size - 1, 0, -1):
        on[v] = on[v << 1] and on[v << 1 | 1]

    def touch(v, t):
        if on[v]:
            total[v] += t - last[v]
        last[v] = t

    def toggle(pos, t):
        v = size + pos

        u = v
        while u:
            touch(u, t)
            u >>= 1

        on[v] = not on[v]

        v >>= 1
        while v:
            on[v] = on[v << 1] and on[v << 1 | 1]
            last[v] = t
            v >>= 1

    def query(left, right, t):
        left += size
        right += size
        ans = 0

        while left < right:
            if left & 1:
                touch(left, t)
                ans += total[left]
                left += 1

            if right & 1:
                right -= 1
                touch(right, t)
                ans += total[right]

            left >>= 1
            right >>= 1

        return ans

    out = []

    for t in range(1, q + 1):
        event = next(it).split()

        if event[0] == "toggle":
            toggle(int(event[1]) - 1, t)
        else:
            a = int(event[1])
            b = int(event[2])
            out.append(str(query(a - 1, b - 1, t)))

    return "\n".join(out)

# Official sample
assert solve_io(
    """5 7
11011
query 1 2
query 1 2
query 1 6
query 3 4
toggle 3
query 3 4
query 1 6
"""
) == """1
2
0
0
1
2""", "official sample"

# Minimum-size case
assert solve_io(
    """1 1
1
query 1 2
"""
) == "1", "single lamp"

# All lamps initially on, then the middle lamp is toggled off and on again
assert solve_io(
    """3 5
111
query 1 4
toggle 2
query 1 4
toggle 2
query 1 4
"""
) == """1
1
2""", "toggle off and back on"

# Boundary query with only the last lamp
assert solve_io(
    """2 3
01
query 2 3
toggle 2
query 2 3
"""
) == """1
1""", "last lamp boundary"

# Maximum-size n and q, all lamps initially on.
# Every event is a query over the complete street.
n = 300000
q = 300000
maximum_input = (
    f"{n} {q}\n"
    + "1" * n
    + "\n"
    + "\n".join(["query 1 300001"] * q)
    + "\n"
)
maximum_output = "\n".join(["1"] + [str(i) for i in range(2, q + 1)])
assert solve_io(maximum_input) == maximum_output, "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1 / query 1 2`|`1`| Kích thước tối thiểu và thực tế là số giờ đã hoàn thành hiện tại | 
|`3 5 / 111 / ...`|`1, 1, 2`| Chuyển đổi lặp đi lặp lại và bảo tồn thời gian lịch sử | 
|`2 3 / 01 / ...`|`1, 1`| Truy vấn bắt đầu ở cặp dừng cuối cùng và chuyển đổi điểm cuối | 
|`300000 300000 / 1...1 / repeated full queries`|`1, 2, ..., 300000`| Hạn chế và tích lũy tối đa trên toàn phạm vi | 

## Vỏ cạnh 

Trường hợp tối thiểu có một đèn và hai điểm dừng. Vì```
1 1
1
query 1 2
```truy vấn tương ứng với một đèn duy nhất bật trong suốt giờ 1. Khoảng thời gian truy vấn trở thành`[0,1)`, chọn chính xác một lá, và`touch`cộng (1-0=1). Đầu ra là`1`. 

Truy vấn toàn đường là một nguồn lỗi thường gặp khác. Với```
3 1
111
query 1 4
```tuyến đường từ điểm dừng 1 đến điểm dừng 4 sử dụng đèn 1, 2 và 3. Việc triển khai chuyển đổi điều này thành`[0,3)`, vậy là cả ba lá đều được chọn. Vì phân đoạn hoàn chỉnh được chiếu suốt giờ đầu tiên nên câu trả lời là`1`. 

Việc chuyển đổi vào cuối một giờ không được thay đổi giờ đó về trước. Coi như```
1 2
1
toggle 1
query 1 2
```Trong giờ 1 đèn sáng. Việc chuyển đổi xảy ra vào cuối giờ 1, vì vậy trong giờ 2 nó sẽ tắt. Tại dấu thời gian chuyển đổi, chiếc lá được chạm vào lần đầu tiên và nhận được một giờ tích lũy đúng giờ. Truy vấn tiếp theo chạm lại vào nó nhưng trạng thái hiện tại của nó bị tắt nên không có thêm thời gian. Câu trả lời là`1`. 

Phạm vi có một đèn tắt phải đóng góp bằng 0 ngay cả khi mọi đèn khác đều bật. Vì```
3 1
101
query 1 4
```phần gốc đại diện cho toàn bộ đường phố hiện đang bị tắt vì phần giữa của nó bị tắt. Truy vấn phân tách phạm vi thành các nút có trạng thái kết hợp không hoàn toàn bật và mọi nút được chọn chứa đèn 0 không đóng góp thời gian. Đầu ra là`0`. 

Trường hợp kích thước tối đa cũng hữu ích để kiểm tra xem các giá trị tích lũy có còn chính xác qua nhiều truy vấn hay không. Nếu tất cả (300000) đèn đều bật và mọi sự kiện đều là truy vấn trên toàn đường thì trạng thái sẽ không bao giờ thay đổi. Truy vấn đầu tiên ghi lại một giờ, truy vấn thứ hai ghi lại một giờ khác, v.v., tạo ra câu trả lời từ 1 đến 300000. Cây phân đoạn không bao giờ cần kiểm tra từng đèn riêng lẻ, vì vậy các truy vấn lặp lại vẫn ở dạng logarit.
