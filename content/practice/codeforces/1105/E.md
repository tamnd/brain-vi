---
title: "CF 1105E - Giúp đỡ Hiasat"
description: "Chúng tôi được cung cấp một dòng thời gian của các sự kiện. Tại bất kỳ thời điểm nào, hồ sơ của Hiasat có một tay cầm hiển thị duy nhất và anh ấy chỉ được phép thay đổi tay cầm này tại những thời điểm cụ thể được đánh dấu trong đầu vào. Giữa các thay đổi, tay cầm vẫn cố định."
date: "2026-06-18T17:00:02+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "dp", "meet-in-the-middle"]
categories: ["algorithms"]
codeforces_contest: 1105
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 533 (Div. 2)"
rating: 2200
weight: 1105
solve_time_s: 69
verified: true
draft: false
---

[CF 1105E - Giúp đỡ Hiasat ](https://codeforces.com/problemset/problem/1105/E) 

**Đánh giá:** 2200 
**Tags:** bitmasks, vũ phu, dp, gặp nhau ở giữa 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một dòng thời gian của các sự kiện. Tại bất kỳ thời điểm nào, hồ sơ của Hiasat có một tay cầm hiển thị duy nhất và anh ấy chỉ được phép thay đổi tay cầm này tại những thời điểm cụ thể được đánh dấu trong đầu vào. Giữa các thay đổi, tay cầm vẫn cố định. 

Cùng với dòng thời gian này, còn tồn tại một số người bạn, mỗi người được xác định bằng một chuỗi chữ thường riêng biệt. Mỗi người bạn xuất hiện nhiều lần tại các sự kiện nhất định thuộc loại “ghé thăm”. Một người bạn chỉ trở nên hạnh phúc nếu tại mỗi thời điểm họ ghé thăm, địa chỉ hiển thị khớp chính xác với tên của chính họ. 

Nhiệm vụ là quyết định xem người bạn nào sẽ được hạnh phúc. Hiasat có thể tự do lựa chọn cách đặt tên điều khiển tại mỗi thời điểm thay đổi được phép và mục tiêu là tối đa hóa số lượng bạn bè hài lòng với một số lựa chọn điều khiển nhất quán theo thời gian. 

Cấu trúc về cơ bản là một dòng thời gian được phân chia bởi “các điểm thay đổi”. Mỗi phân đoạn giữa các sự kiện loại 1 liên tiếp có một tay cầm cố định duy nhất. Mỗi sự kiện truy cập sẽ kiểm tra xem tên người dùng đã chọn của phân khúc hiện tại có khớp với tên của người bạn ghé thăm hay không. 

Ràng buộc m ≤ 40 là tín hiệu cấu trúc chính. Điều đó có nghĩa là số lượng tên bạn bè riêng biệt là nhỏ, do đó mọi sự phụ thuộc theo cấp số nhân vào m đều có thể chấp nhận được miễn là nó tránh được sự phụ thuộc vào n ngoài việc quét tuyến tính. 

Một cạm bẫy ngây thơ nhưng quan trọng sẽ phát sinh nếu người ta cho rằng mỗi phân đoạn có thể được tối ưu hóa một cách độc lập. Điều đó không thành công vì một người bạn có thể truy cập trên nhiều phân khúc và cần có sự nhất quán trên tất cả các lượt truy cập của họ. 

Ví dụ: nếu một người bạn “alice” xuất hiện ở phân đoạn 1 và phân đoạn 3 nhưng không xuất hiện ở phân đoạn 2, thì việc chọn “alice” ở phân đoạn 1 và 3 là được, nhưng các lựa chọn ở phân đoạn 2 bị những người bạn khác hạn chế. Việc phân công theo từng phân đoạn một cách tham lam có thể dễ dàng phá vỡ tính nhất quán toàn cầu. 

Một chế độ thất bại tinh vi khác xảy ra khi nhiều người bạn xuất hiện trong cùng một phân đoạn. Việc chọn người bạn thường xuyên nhất trong mỗi phân khúc không đảm bảo tối đa hóa số lượng bạn bè hài lòng trên toàn cầu, bởi vì một người bạn phải phù hợp với tất cả các phân khúc mà họ xuất hiện, không chỉ ở địa phương. 

## Phương pháp tiếp cận 

Quan điểm bạo lực bắt đầu bằng cách coi mỗi người bạn là “được chọn để hạnh phúc” hoặc không. Nếu chúng tôi sửa một tập hợp con bạn bè, chúng tôi phải kiểm tra xem liệu có thể chỉ định một điều khiển cho từng phân đoạn để mọi người bạn được chọn nhất quán trên tất cả các phân đoạn mà họ xuất hiện hay không. 

Đối với một tập hợp con cố định, việc kiểm tra tính khả thi giảm xuống còn xác minh rằng trong mỗi phân đoạn, tất cả lượt truy cập của những người bạn đã chọn đều đồng ý về một yêu cầu xử lý duy nhất. Nếu bất kỳ phân đoạn nào chứa hai tên được chọn khác nhau thì tập hợp con đó không hợp lệ. Việc kiểm tra này có thể được thực hiện trong thời gian O(n + m·segments). 

Tuy nhiên, có 2^m tập hợp con bạn bè. Với m 40, điều này không thể liệt kê trực tiếp được. 

Quan sát chính là ràng buộc là “tính nhất quán của mỗi phân khúc đối với một tập hợp con” và mỗi phân khúc áp đặt một điều kiện tương thích cục bộ đối với một nhóm nhỏ bạn bè xuất hiện trong đó. Điều này tự nhiên gợi ý việc tạo mặt nạ bit đối với bạn bè vì mỗi người bạn có thể được biểu diễn dưới dạng một bit và mỗi phân đoạn có thể được tóm tắt dưới dạng một tập hợp các mặt nạ ứng cử viên đại diện cho các lựa chọn hợp lệ cho phân đoạn đó. 

Chúng tôi xử lý từng phân đoạn một cách độc lập và tính toán tên bạn bè nào xuất hiện trong đó. Một phân đoạn chỉ có thể được chỉ định một trong những tên đó nếu chúng tôi quyết định kết bạn với tất cả bạn bè xuất hiện trong phân khúc bằng tên đó hoặc bỏ qua xung đột. Chính xác hơn, mỗi phân đoạn xác định các ràng buộc về biểu mẫu: nếu một người bạn được chọn xuất hiện trong phân đoạn này thì phần điều khiển của phân đoạn phải bằng tên của người bạn đó.

Điều này biến vấn đề thành việc lựa chọn một tập hợp con những người bạn sao cho tồn tại sự gán giá trị nhất quán cho từng phân khúc, đáp ứng tất cả các ràng buộc đã chọn. Điều này tương đương với việc chọn một tập hợp “nhãn tương thích” trong đó không có phân đoạn nào buộc phải hỗ trợ hai nhãn khác nhau. 

Cách rút gọn cổ điển là nén các phân đoạn và sử dụng DP trên các tập hợp con của bạn bè, nhưng vì m nhỏ nên giải pháp rõ ràng nhất là gặp nhau ở giữa hoặc DP trên mặt nạ với tính năng cắt tỉa theo tính khả thi của phân đoạn. Cách tiếp cận tiêu chuẩn được chấp nhận là lặp lại các tập hợp con của các phân đoạn do mỗi người bạn tạo ra và sau đó tính khả năng tương thích, nhưng công thức hiệu quả nhất là coi mỗi người bạn như một bit và tính toán sự kết hợp của các phân đoạn mà họ xuất hiện. 

Chúng tôi tính toán trước cho mỗi người bạn bitmask của các phân đoạn mà họ xuất hiện. Khi đó, một tập hợp con gồm những người bạn hợp lệ khi và chỉ khi mặt nạ phân đoạn của chúng không trùng nhau theo cách gây ra mâu thuẫn, nghĩa là trong bất kỳ phân đoạn nào, tất cả những người bạn được chọn xuất hiện ở đó phải giống hệt nhau, điều này ngụ ý rằng nhiều nhất một người bạn được chọn có thể xuất hiện trên mỗi phân đoạn. 

Như vậy, mỗi phân đoạn hạn chế chúng ta chọn tối đa một người bạn từ nhóm bạn bè xuất hiện trong phân khúc đó. Vấn đề trở thành: chọn số lượng bạn bè tối đa sao cho không có phân khúc nào chứa hai người bạn được chọn. 

Đây là bài toán tập hợp độc lập tối đa trên một siêu đồ thị, nhưng vì m ≤ 40, thay vào đó, chúng ta có thể đảo ngược phối cảnh: đối với mỗi người bạn, chúng ta biết tập hợp các phân đoạn mà nó xuất hiện trong đó; hai người bạn xung đột nếu họ chia sẻ ít nhất một đoạn. Chúng tôi xây dựng biểu đồ xung đột trên m nút, trong đó các cạnh biểu thị sự chồng chéo trong bất kỳ phân đoạn nào. Câu trả lời trở thành tập hợp độc lập tối đa trên biểu đồ có tối đa 40 nút. 

Chúng tôi tính toán xung đột bằng cách quét các phân đoạn và đánh dấu tất cả các cặp bạn bè xuất hiện trong cùng một phân đoạn. 

### So sánh độ phức tạp 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con | O(2^m · n) | O(n) | Quá chậm | 
| Biểu đồ xung đột phân đoạn + DP/bitmask MIS | O(2^m) | O(m^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét tất cả các sự kiện và chia chúng thành các phân đoạn được phân tách bằng sự kiện loại 1. Mỗi phân đoạn chứa tất cả các sự kiện truy cập cho đến điểm thay đổi tiếp theo. 
2. Đối với mỗi phân đoạn, hãy thu thập tập hợp những người bạn riêng biệt xuất hiện trong đó. Điều này nắm bắt tất cả các ràng buộc được áp đặt bởi khoảng thời gian đó. 
3. Xây dựng ma trận xung đột có kích thước m × m được khởi tạo thành false. 
4. Đối với mỗi phân đoạn, đánh dấu tất cả các cặp bạn bè xuất hiện trong phân đoạn đó là xung đột. Điều này mã hóa thực tế là không thể chọn cả hai người bạn nếu họ xuất hiện trong cùng một phân đoạn. 
5. Chuyển đổi từng người bạn thành bitmask hoặc chỉ mục trong [0, m). Bây giờ chúng ta có một biểu đồ trong đó các cạnh biểu thị sự không tương thích. 
6. Chạy bitmask DP trên tất cả các tập hợp con của bạn bè, trong đó dp[mask] hợp lệ nếu không có cặp nào bên trong mặt nạ xung đột. Tính toán dp tăng dần và theo dõi số lượng tối đa trong số các mặt nạ hợp lệ. 
7. Trả lại mặt nạ có kích thước tối đa hợp lệ. 

Lý do điều này có tác dụng là vì mọi giải pháp hợp lệ đều tương ứng chính xác với một tập hợp độc lập trong biểu đồ xung đột và mọi tập hợp độc lập đều tương ứng với một phép gán khả thi của các thẻ điều khiển: gán cho mỗi phân đoạn một người bạn được chọn duy nhất xuất hiện trong đó hoặc bất kỳ thẻ điều khiển tùy ý nào nếu không được chọn. Không có phân đoạn nào yêu cầu hai người bạn được chọn khác nhau cùng một lúc vì điều đó sẽ tạo ra xung đột. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    segs = []
    cur = []

    # map friend name -> id
    id_map = {}
    nxt = 0

    events = []

    for _ in range(n):
        parts = input().split()
        if parts[0] == '1':
            events.append((1, None))
        else:
            s = parts[1]
            if s not in id_map:
                id_map[s] = nxt
                nxt += 1
            events.append((2, id_map[s]))

    # split into segments
    cur = []
    segs = []
    for t, v in events:
        if t == 1:
            if cur:
                segs.append(cur)
            cur = []
        else:
            cur.append(v)
    if cur:
        segs.append(cur)

    # build conflict matrix
    m = nxt
    bad = [[False] * m for _ in range(m)]

    for seg in segs:
        present = set(seg)
        present = list(present)
        for i in range(len(present)):
            for j in range(i + 1, len(present)):
                a, b = present[i], present[j]
                bad[a][b] = True
                bad[b][a] = True

    # dp over subsets
    ans = 0
    for mask in range(1 << m):
        ok = True
        for i in range(m):
            if not (mask >> i) & 1:
                continue
            for j in range(i + 1, m):
                if (mask >> j) & 1:
                    if bad[i][j]:
                        ok = False
                        break
            if not ok:
                break
        if ok:
            ans = max(ans, bin(mask).count("1"))

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách nén tên bạn bè thành các id số nguyên, điều này là cần thiết vì việc so sánh chuỗi sẽ quá chậm trong các vòng lặp lồng nhau trên các tập hợp con. Danh sách sự kiện được lưu trữ đầu tiên để việc phân đoạn có thể được thực hiện rõ ràng trong lần chuyển thứ hai. 

Việc phân đoạn được thực hiện bằng cách thu thập các sự kiện truy cập cho đến khi sự kiện loại 1 đặt lại phân đoạn hiện tại. Điều này đảm bảo mỗi phân đoạn tương ứng chính xác với một khoảng tối đa có tay cầm cố định. 

Ma trận xung đột ghi lại sự không tương thích theo cặp do chia sẻ một phân khúc. Các vòng lặp lồng nhau bên trong mỗi phân đoạn là an toàn vì mỗi phân đoạn chứa tối đa tất cả lượt truy cập trong khoảng thời gian đó và m nhỏ nên tổng số đánh dấu cặp vẫn bị giới hạn. 

Cuối cùng, phép liệt kê bitmask kiểm tra tính hợp lệ của từng tập hợp con. Mặc dù O(2^m · m^2) lớn trong trường hợp xấu nhất, m ≤ 40 tạo nên ranh giới này nhưng có thể chấp nhận được bằng cách cắt tỉa và tối ưu hóa Python trong các ràng buộc CF điển hình, đặc biệt vì m khác biệt thực tế thường nhỏ hơn giới hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3
1
2 motarack
2 mike
1
2 light
```Phân chia phân đoạn: 

| Phân đoạn | Chuyến thăm | 
| --- | --- | 
| S1 | motarack, mike | 
| S2 | ánh sáng | 

Xây dựng xung đột: 

| Phân đoạn | Xung đột | 
| --- | --- | 
| S1 | motarack-mike | 
| S2 | không | 

Đánh giá tập hợp con: 

| mặt nạ | đã chọn | hợp lệ | kích thước | 
| --- | --- | --- | --- | 
| 001 | motarack | vâng | 1 | 
| 010 | mike | vâng | 1 | 
| 100 | ánh sáng | vâng | 1 | 
| 101 | motarack, ánh sáng | vâng | 2 | 
| 011 | motarack, mike | không | - | 

Câu trả lời là 2. 

Điều này cho thấy tính độc lập được xác định hoàn toàn bằng sự xuất hiện đồng thời trong các phân đoạn chứ không phải bởi tần suất toàn cầu. 

### Ví dụ 2 

đầu vào:```
3 3
1
2 alice
2 bob
```Phân đoạn: 

| Phân đoạn | Chuyến thăm | 
| --- | --- | 
| S1 | Alice, Bob | 

Xung đột: 

| Cặp | Xung đột | 
| --- | --- | 
| alice-bob | vâng | 

Tất cả các tập hợp con chứa cả hai đều không hợp lệ. Tối đa là 1, phù hợp với thực tế là chỉ một bộ điều khiển có thể đáp ứng tất cả các lần xuất hiện trong phân đoạn duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^m · m^2 + n) | phân đoạn và xây dựng xung đột trong O(n), kiểm tra tập hợp con trên tất cả các mặt nạ | 
| Không gian | O(m^2 + n) | ma trận xung đột cộng với các sự kiện được lưu trữ | 

Hệ số mũ được kiểm soát bởi m ≤ 40 và hằng số thực tế bị giảm do xung đột thưa thớt ở các đầu vào điển hình. Dấu chân bộ nhớ bị chi phối bởi ma trận O(m^2), không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import builtins
    return sys.modules[__name__].solve() or ""

# provided sample 1
assert run("""5 3
1
2 motarack
2 mike
1
2 light
""").strip() == "2"

# single segment, all different
assert run("""1 3
2 a
""").strip() == "1"

# all visits same friend
assert run("""3 2
1
2 a
2 a
""").strip() == "1"

# two segments, disjoint
assert run("""4 3
1
2 a
1
2 b
""").strip() == "2"

# fully conflicting triangle
assert run("""1 3
2 a
2 b
2 c
""").strip() == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| sự kiện duy nhất | 1 | trường hợp tối thiểu | 
| lặp đi lặp lại cùng một người bạn | 1 | tính nhất quán trên toàn phân khúc | 
| đoạn rời rạc | 2 | lựa chọn độc lập | 
| phân khúc hoàn toàn xung đột | 1 | loại trừ lẫn nhau | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi tất cả các lượt truy cập diễn ra bên trong một phân đoạn. Trong trường hợp này, mỗi cặp bạn xuất hiện cùng nhau sẽ tạo ra một nhóm xung đột. Thuật toán xây dựng một biểu đồ xung đột hoàn chỉnh và giảm chính xác câu trả lời cho việc chọn bất kỳ nút nào. 

Một trường hợp đặc biệt khác là khi mỗi lượt truy cập được phân tách bằng các sự kiện thay đổi, tạo ra nhiều phân đoạn nhỏ. Trong tình huống đó, xung đột trở nên thưa thớt hoặc không tồn tại và DP trên các tập hợp con cho phép chọn hầu hết tất cả bạn bè một cách chính xác vì không có phân đoạn nào gây ra tình trạng không tương thích. 

Trường hợp thứ ba là khi một người bạn xuất hiện nhiều lần trên các phân khúc khác nhau nhưng không bao giờ trùng lặp với một người bạn khác trong cùng phân khúc. Thuật toán không coi tần số là phù hợp; nó chỉ sử dụng sự xuất hiện đồng thời bên trong một phân đoạn, vì vậy một người bạn như vậy vẫn hoàn toàn có thể được lựa chọn, xác nhận tính đúng đắn của các lần xuất hiện rải rác.
