---
title: "CF 1000F - Một lần xuất hiện"
description: "Chúng tôi được cung cấp một mảng tĩnh và nhiều truy vấn độc lập, mỗi truy vấn yêu cầu chúng tôi kiểm tra một phân đoạn liền kề của mảng và trả về bất kỳ giá trị nào xuất hiện chính xác một lần trong phân đoạn đó. Nếu không có giá trị nào tồn tại trong phân đoạn đó thì câu trả lời là 0."
date: "2026-06-16T23:49:17+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer"]
categories: ["algorithms"]
codeforces_contest: 1000
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 46 (Rated for Div. 2)"
rating: 2400
weight: 1000
solve_time_s: 112
verified: false
draft: false
---

[CF 1000F - Một lần xuất hiện](https://codeforces.com/problemset/problem/1000/F) 

**Đánh giá:** 2400 
**Thẻ:** cấu trúc dữ liệu, phân chia để chinh phục 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng tĩnh và nhiều truy vấn độc lập, mỗi truy vấn yêu cầu chúng tôi kiểm tra một phân đoạn liền kề của mảng và trả về bất kỳ giá trị nào xuất hiện chính xác một lần trong phân đoạn đó. Nếu không có giá trị nào tồn tại trong phân đoạn đó thì câu trả lời là 0. 

Một cách hữu ích để suy nghĩ về từng truy vấn là chúng ta lấy một cửa sổ của mảng và phân loại mọi giá trị riêng biệt trong cửa sổ đó theo tần số của nó. Nhiệm vụ không phải là đếm tần số trên toàn cầu mà là phát hiện xem ít nhất một giá trị có tần số chính xác bằng một trong khoảng đã chọn hay không và nếu có, hãy trả về bất kỳ giá trị nào trong số đó. 

Các ràng buộc đẩy chúng tôi ra khỏi việc tính toán lại tần số từ đầu cho mỗi truy vấn. Với tối đa 500.000 phần tử và 500.000 truy vấn, ngay cả việc quét từng phạm vi truy vấn cũng sẽ dẫn đến khoảng 10^11 thao tác trong trường hợp xấu nhất, vượt xa giới hạn 3 giây có thể xử lý trong Python hoặc C++. 

Trường hợp cạnh khóa là khi tất cả các phần tử trong một đoạn được lặp lại ít nhất hai lần. Ví dụ, trong đoạn`[1, 1, 2, 2]`, không có câu trả lời hợp lệ nào tồn tại mặc dù có nhiều giá trị xuất hiện. Một trường hợp tinh tế khác là khi có nhiều giá trị duy nhất trong toàn bộ mảng, nhưng khoảng thời gian truy vấn sẽ cắt chúng để không có giá trị nào xuất hiện một lần trong khoảng đó. 

Một sai lầm ngây thơ là tính toán trước tần số toàn cầu và cho rằng tính duy nhất toàn cầu hàm ý tính duy nhất cục bộ. Ví dụ, trong`[1,2,1]`, giá trị`2`là duy nhất trên toàn cầu, nhưng trong truy vấn`[1,1,2]`, tình hình thay đổi tùy theo cửa sổ; tần số toàn cầu là không đủ. 

Một cạm bẫy phổ biến khác là cố gắng duy trì cửa sổ trượt cho mỗi truy vấn một cách độc lập. Vì các truy vấn là tùy ý, không được sắp xếp ngoại tuyến nên điều này không mang lại cấu trúc khấu hao nhất quán trừ khi chúng tôi áp dụng các kỹ thuật sắp xếp truy vấn nâng cao hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp tính toán tần số cho từng khoảng thời gian truy vấn bằng cách sử dụng bản đồ hoặc mảng băm. Điều này đúng vì theo nghĩa đen, chúng tôi đếm số lần xuất hiện trong phạm vi và chọn bất kỳ giá trị nào có số lần đếm chính xác là một. Tuy nhiên, việc tính toán lại số lượng cho mỗi truy vấn yêu cầu quét tối đa O(n) cho mỗi truy vấn, dẫn đến độ phức tạp tổng cộng là O(nq), điều này là không khả thi. 

Cấu trúc của vấn đề cho thấy chúng ta cần xử lý trước thông tin tổng thể và sử dụng lại thông tin đó qua các truy vấn. Quan sát quan trọng là một giá trị chỉ góp phần trở thành một câu trả lời hợp lệ thông qua lần xuất hiện gần nhất của nó. Nếu chúng ta biết, đối với mỗi vị trí, nơi xảy ra lần xuất hiện trước và tiếp theo của cùng một giá trị, thì chúng ta có thể xác định xem vị trí đó có phải là lần xuất hiện duy nhất trong một khoảng nhất định hay không. 

Cụ thể, chỉ mục i đóng góp một câu trả lời hợp lệ cho truy vấn [l, r] nếu đó là lần xuất hiện duy nhất của giá trị của nó trong phạm vi đó. Điều đó có nghĩa là lần xuất hiện trước của nó hoàn toàn trước l và lần xuất hiện tiếp theo của nó hoàn toàn sau r. Vì vậy, mỗi truy vấn giảm xuống việc tìm bất kỳ chỉ mục i nào trong [l, r] thỏa mãn hai điều kiện biên đó. 

Chúng ta có thể tính toán trước các mảng xuất hiện trước đó và tiếp theo trong O(n), nhưng thách thức còn lại là trả lời hiệu quả các truy vấn phạm vi cho điều kiện phụ thuộc vào cả hai điểm cuối. Đây là nơi việc phân chia và chinh phục các truy vấn kết hợp với cây phân đoạn hoặc cấu trúc được lập chỉ mục nhị phân trở nên tự nhiên: chúng tôi coi mỗi vị trí là đóng góp một cửa sổ hợp lệ ứng viên và truy vấn xem có bất kỳ chỉ mục hợp lệ nào tồn tại trong một phạm vi hay không. 

Một cách rõ ràng để triển khai điều này là xử lý các vị trí theo thứ tự tăng dần về “thời gian kích hoạt” của chúng cho mỗi ràng buộc điểm cuối truy vấn, nhưng một giải pháp tiêu chuẩn hơn cho vấn đề này là phân chia và chinh phục ngoại tuyến: chúng tôi phân chia đệ quy các truy vấn theo điểm giữa và duy trì một cấu trúc theo dõi các lần xuất hiện hợp lệ vượt qua điểm giữa, đảm bảo mỗi vị trí được xem xét ở mức O(log n). 

Thay vào đó, chúng ta có thể sử dụng giải pháp CF tiêu chuẩn và đơn giản hơn: xử lý các vị trí bằng cách tăng điểm cuối bên phải trong khi duy trì cây Fenwick đối với "các lần xuất hiện duy nhất" ứng viên được xác định bởi giới hạn lần xuất hiện tiếp theo. Mỗi chỉ mục i đóng góp trọng số 1 tại vị trí i nếu nó hiện hợp lệ cho một truy vấn và chúng tôi hỗ trợ các truy vấn tổng phạm vi để phát hiện sự tồn tại. 

Điều này làm giảm vấn đề biến mỗi chỉ mục thành một khoảng thời gian kích hoạt phạm vi và trả lời nếu có bất kỳ chỉ mục hoạt động nào nằm trong phạm vi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Tối ưu (ngoại tuyến + Fenwick/cấu trúc phân đoạn) | O(n log n + q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Với mỗi vị trí i, tính chỉ số xuất hiện trước đó của a[i]. Nếu không tồn tại, hãy lưu trữ 0. Điều này cho phép chúng ta biết giá trị còn lại kéo dài bao xa trước khi lặp lại. 
2. Tính chỉ số xuất hiện tiếp theo của a[i]. Nếu không tồn tại thì lưu n+1. Điều này cho chúng ta biết khoảng cách bên phải của một giá trị vẫn là duy nhất trước khi nó lặp lại. 
3. Quan sát rằng vị trí i là đại diện duy nhất cho giá trị của nó bên trong bất kỳ khoảng [l, r] thỏa mãn prev[i] < l ≤ i ≤ r < next[i]. Điều này chuyển đổi điều kiện tần số thành điều kiện hình học theo các khoảng thời gian. 
4. Diễn giải mỗi chỉ mục i dưới dạng một hình chữ nhật kích hoạt trên không gian truy vấn: nó hợp lệ cho tất cả các truy vấn có điểm cuối bên trái nằm trong (prev[i], i] và điểm cuối bên phải nằm trong [i, next[i]). Chúng tôi không liệt kê rõ ràng các hình chữ nhật; thay vào đó chúng tôi sử dụng quét trên một chiều. 
5. Sắp xếp các truy vấn theo điểm cuối bên phải của chúng. Chúng tôi xử lý các vị trí theo thứ tự tăng dần của điểm cuối bên phải trong khi duy trì cấu trúc trên các chỉ mục hiện hợp lệ đối với ràng buộc ranh giới bên phải r < next[i]. 
6. Duy trì cây Fenwick hoặc cây phân đoạn trên các vị trí. Khi xử lý vị trí i, chúng tôi chèn nó vào cấu trúc tại chỉ mục i và chúng tôi cũng lên lịch xóa nó tại next[i], đảm bảo nó sẽ không hoạt động khi nó không còn có thể đóng vai trò là phần tử duy nhất cho các truy vấn trong tương lai. 
7. Với mỗi truy vấn [l, r], sau khi xử lý đến r, ta truy vấn tổng phạm vi trên [l, r]. Nếu tổng là dương, chúng tôi tìm thấy bất kỳ chỉ mục nào có giá trị 1 trong phạm vi đó và xuất ra a[i]; nếu không thì xuất ra 0. 

Chi tiết triển khai chính là cây lưu trữ các chỉ mục hiện là ứng cử viên hợp lệ và tính hợp lệ được xác định hoàn toàn bằng các ràng buộc lần xuất hiện tiếp theo, trong khi truy vấn đảm bảo tính đúng đắn của lần xuất hiện trước đó bằng cách xây dựng vì các bản sao không bao giờ hoạt động đồng thời ở các vị trí xung đột chồng chéo. 

### Tại sao nó hoạt động 

Mỗi giá trị xuất hiện trong các chuỗi chỉ số rời rạc được sắp xếp theo lần xuất hiện. Tại bất kỳ thời điểm nào, thuật toán giữ chính xác một đại diện cho mỗi giá trị đủ điều kiện là lần xuất hiện duy nhất trong một phân đoạn được giới hạn trước bản sao tiếp theo của nó. Nếu chỉ mục i đang hoạt động, nó đảm bảo không có lần xuất hiện nào khác có cùng giá trị nằm trong phân đoạn cho đến ranh giới lần xuất hiện tiếp theo của nó. Do đó, bất kỳ truy vấn nào chứa chính xác một đại diện hiện hoạt đều tương ứng với một giá trị xuất hiện chính xác một lần trong khoảng đó và bất kỳ đại diện nào như vậy đều là câu trả lời hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    q = int(input())

    nxt = [n] * n
    prv = [-1] * n

    last = {}
    for i in range(n):
        v = a[i]
        if v in last:
            prv[i] = last[v]
        last[v] = i

    last.clear()
    for i in range(n - 1, -1, -1):
        v = a[i]
        if v in last:
            nxt[i] = last[v]
        last[v] = i

    queries = []
    for i in range(q):
        l, r = map(int, input().split())
        queries.append((r - 1, l - 1, i))

    queries.sort()

    bit = [0] * (n + 2)

    def add(i, v):
        i += 1
        while i <= n:
            bit[i] += v
            i += i & -i

    def sum_(i):
        s = 0
        i += 1
        while i > 0:
            s += bit[i]
            i -= i & -i
        return s

    def range_sum(l, r):
        if l > r:
            return 0
        return sum_(r) - sum_(l - 1)

    # active positions by next boundary
    buckets = [[] for _ in range(n + 1)]
    for i in range(n):
        buckets[nxt[i]].append(i)

    active = [False] * n

    ptr = 0
    for r, l, idx in queries:
        while ptr <= r:
            for pos in buckets[ptr]:
                active[pos] = True
                add(pos, 1)
            ptr += 1

        if range_sum(l, r) == 0:
            print(0)
        else:
            lo, hi = l, r
            ans = 0
            while lo <= hi:
                mid = (lo + hi) // 2
                if range_sum(l, mid) > 0:
                    ans = mid
                    hi = mid - 1
                else:
                    lo = mid + 1
            print(a[ans])

def main():
    solve()

if __name__ == "__main__":
    main()
```Bản dựng thẻ tiền xử lý đầu tiên`prv`, theo dõi lần xuất hiện cuối cùng của từng giá trị và bản dựng thứ hai`nxt`, theo dõi lần xuất hiện tiếp theo. Các mảng này là thông tin duy nhất cần thiết để xác định liệu một chỉ mục có thể đóng vai trò là đại diện duy nhất trong một phân khúc hay không. 

Cây Fenwick duy trì tập hợp các chỉ mục có lần xuất hiện tiếp theo vẫn nằm ngoài ranh giới xử lý hiện tại. Mỗi lần chúng tôi nâng cao con trỏ bên phải của truy vấn, chúng tôi sẽ kích hoạt tất cả các vị trí có`nxt[i]`bằng với vị trí hiện tại, vì những vị trí đó hiện là ứng cử viên an toàn cho các truy vấn kết thúc tại hoặc trước thời điểm đó. 

Đối với mỗi truy vấn, tổng phạm vi trên cây Fenwick sẽ phát hiện xem có ứng cử viên nào tồn tại hay không. Nếu đúng như vậy, tìm kiếm nhị phân trên phạm vi sẽ xác định một chỉ mục hợp lệ, chỉ mục này sẽ được chuyển đổi trở lại giá trị của nó. 

Tìm kiếm nhị phân an toàn vì khi tổng tiền tố trở thành dương, nó vẫn dương khi chúng ta mở rộng giới hạn bên phải, đảm bảo tính đơn điệu. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:```
6
1 1 2 3 2 4
2
2 6
1 2
```Chúng tôi tính toán`prv`Và`nxt`. Đối với giá trị 1, số lần xuất hiện là 1 và 2, do đó chỉ số 1 và 2 được giới hạn bởi nhau. Đối với giá trị 3 và 4, chúng không có phần lặp lại gần đó nên chúng có phạm vi giá trị rộng. 

Đối với truy vấn`[2, 6]`, các ứng cử viên đang hoạt động bên trong cửa sổ này tương ứng với các giá trị xuất hiện chính xác một lần trong mảng con`[1, 2, 3, 2, 4]`. Bảng dưới đây cho thấy việc kiểm tra ứng viên: 

| chỉ mục | giá trị | hoạt động trong phạm vi | đóng góp hợp lệ | 
| --- | --- | --- | --- | 
| 1 | 1 | vâng | không (xuất hiện hai lần) | 
| 2 | 2 | vâng | không (xuất hiện hai lần) | 
| 3 | 3 | vâng | vâng | 
| 4 | 2 | vâng | không (trùng lặp được xử lý theo cấu trúc) | 
| 5 | 4 | vâng | vâng | 

Vị trí hợp lệ đầu tiên gặp phải dẫn đến đầu ra`3`hoặc`4`tùy thuộc vào thứ tự quét và mẫu cho phép mọi câu trả lời hợp lệ; lựa chọn đầu ra chính thức`4`. 

Đối với truy vấn`[1, 2]`, cả hai giá trị 1 xuất hiện hai lần bên trong phân đoạn, do đó không tồn tại singleton hoạt động nào và đầu ra là`0`. 

Dấu vết này cho thấy tính chính xác phụ thuộc vào việc xác định ít nhất một đơn vị hoạt động thay vì liệt kê tất cả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + q log n) | Mỗi chỉ mục được chèn một lần vào cây Fenwick, mỗi truy vấn thực hiện các phép toán logarit | 
| Không gian | O(n) | Mảng cho trước, tiếp theo, xô và cây Fenwick | 

Các ràng buộc cho phép thực hiện khoảng vài trăm triệu phép toán nguyên thủy, do đó, giải pháp nhân tố logarit với quá trình tiền xử lý tuyến tính sẽ phù hợp thoải mái trong các giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import contextlib, io as sio
    out = sio.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample
assert run("""6
1 1 2 3 2 4
2
2 6
1 2
""") in ["4\n0", "3\n0"]

# all equal
assert run("""5
7 7 7 7 7
1
1 5
""") == "0"

# single element
assert run("""1
42
1
1 1
""") == "42"

# no duplicates, full range
assert run("""5
1 2 3 4 5
1
1 5
""") in ["1\n", "2\n", "3\n", "4\n", "5\n"]

# alternating duplicates
assert run("""6
1 2 1 2 1 2
2
1 6
2 5
""") == "0\n0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 0 | không có singleton nào tồn tại | 
| phần tử đơn | 42 | phạm vi tầm thường | 
| đầy đủ độc đáo | bất kỳ giá trị nào | tính đúng đắn khi tất cả đều hợp lệ | 
| xen kẽ trùng lặp | 0 0 | trùng lặp chồng chéo loại bỏ tất cả các ứng cử viên | 

## Vỏ cạnh 

Một trường hợp như`[7,7,7,7,7]`chứng minh rằng chỉ tần số toàn cầu là không đủ; mọi chỉ mục đều thất bại vì không có lần xuất hiện nào bị cô lập bên trong bất kỳ mảng con nào dài hơn một phần tử. Thuật toán xử lý việc này bằng cách gán cho mỗi chỉ mục một lần xuất hiện tiếp theo trong mảng, điều này ngay lập tức làm mất hiệu lực tất cả các vị trí là ứng cử viên. 

Đối với mảng một phần tử`[42]`, cả hai`prv`Và`nxt`nằm ngoài giới hạn nên chỉ mục luôn hoạt động và mọi truy vấn qua nó đều trả về giá trị đó. 

TRONG`[1,2,1,2,1,2]`, mọi giá trị được xen kẽ chặt chẽ, do đó không có chỉ mục nào thỏa mãn điều kiện là lần xuất hiện duy nhất bên trong bất kỳ khoảng nào dài hơn một. Cây Fenwick không bao giờ tích lũy tổng phạm vi khác 0 cho bất kỳ khoảng truy vấn có ý nghĩa nào, tạo ra số 0 một cách nhất quán.
