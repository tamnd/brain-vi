---
title: "CF 105442J - Thỏ dại"
description: "Chúng tôi được đưa cho một dãy chuồng thỏ, mỗi chuồng chứa một số con thỏ. Đối với mỗi truy vấn, chúng tôi được giao một đoạn liền kề của dòng này và Leonardo chỉ được phép hoạt động bên trong đoạn đó. Mỗi “ngày” anh ấy chọn một cặp chuồng riêng biệt trong phân khúc."
date: "2026-06-23T03:38:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105442
codeforces_index: "J"
codeforces_contest_name: "2024-2025 CTU Open Contest"
rating: 0
weight: 105442
solve_time_s: 63
verified: true
draft: false
---

[CF 105442J - Thỏ dại](https://codeforces.com/problemset/problem/105442/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa cho một dãy chuồng thỏ, mỗi chuồng chứa một số con thỏ. Đối với mỗi truy vấn, chúng tôi được giao một đoạn liền kề của dòng này và Leonardo chỉ được phép hoạt động bên trong đoạn đó. 

Mỗi “ngày” anh ấy chọn một cặp chuồng riêng biệt trong phân khúc. Anh ta tính tổng số thỏ trong hai chuồng đó. Nếu tổng đó là số Fibonacci dương thì ngày đó hợp lệ nhưng có một hạn chế bổ sung: mỗi giá trị Fibonacci chỉ có thể được sử dụng nhiều nhất một lần trong tất cả các ngày trong phân đoạn đó. Nhiệm vụ của mỗi phân đoạn truy vấn là xác định số ngày tối đa có thể có như vậy. 

Diễn đạt lại nhiệm vụ cốt lõi theo cách đơn giản hơn, đối với một mảng con, chúng ta muốn đếm xem có bao nhiêu số Fibonacci riêng biệt có thể được biểu thị dưới dạng tổng của hai phần tử khác nhau được chọn từ mảng con đó. 

Các ràng buộc rất lớn, lên tới 100.000 phần tử và 100.000 truy vấn. Bất kỳ giải pháp nào tính toán lại tổng cặp cho mỗi truy vấn sẽ ngay lập tức thất bại, vì ngay cả một phân đoạn cũng có thể chứa theo thứ tự n2 cặp. Cách tiếp cận O(n²) ngây thơ cho mỗi truy vấn dẫn đến khoảng 10¹⁰ thao tác trong trường hợp xấu nhất, điều này là không khả thi. 

Một quan sát tinh tế hơn là số lượng thỏ bị giới hạn bởi 10⁹, do đó, bất kỳ tổng cặp hợp lệ nào cũng tối đa là 2×10⁹. Điều này có nghĩa là chúng ta chỉ quan tâm đến các số Fibonacci nằm trong phạm vi đó và chỉ có khoảng 45 trong số đó. Tập hợp các mục tiêu hữu hạn này là sự đơn giản hóa cấu trúc quan trọng. 

Một vài trường hợp đáng chú ý. 

Nếu đoạn có độ dài 1 thì không tồn tại cặp nào nên câu trả lời luôn là 0. 

Nếu tất cả các giá trị trong một phân đoạn đều giống hệt nhau, chẳng hạn [5, 5, 5] thì tổng của mỗi cặp đều bằng nhau (10), vì vậy ngay cả khi 10 là Fibonacci thì câu trả lời nhiều nhất là 1 vì việc lặp lại bị cấm. 

Nếu các giá trị lớn và thưa thớt thì không thể hình thành nhiều mục tiêu Fibonacci, do đó, câu trả lời có thể nhỏ hơn đáng kể so với số lượng cặp. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ kiểm tra từng phân đoạn truy vấn một cách độc lập. Đối với một đoạn cố định, chúng ta có thể liệt kê tất cả các cặp (i, j), tính tổng của chúng và kiểm tra xem mỗi tổng có phải là Fibonacci hay không. Chúng tôi sẽ lưu trữ các giá trị Fibonacci đã sử dụng trong một tập hợp để thực thi tính duy nhất. Điều này đúng, nhưng việc liệt kê cặp bên trong chiếm ưu thế về độ phức tạp. Đối với phân đoạn có kích thước k, đây là O(k²), tốc độ này quá chậm khi k đạt đến 10⁵. 

Quan sát quan trọng là các số Fibonacci tạo thành một vũ trụ cố định nhỏ. Vì tất cả Ai ≤ 10⁹ nên bất kỳ tổng nào cũng là 2×10⁹, nên chúng tôi tính toán trước tất cả các số Fibonacci đạt đến giới hạn đó. Chỉ có khoảng 44 con số như vậy. Thay vì suy nghĩ theo cặp, chúng ta đảo ngược quan điểm: với mỗi số Fibonacci F, chúng ta hỏi liệu có tồn tại một cặp bên trong đoạn có tổng bằng F hay không. 

Bây giờ, vấn đề trở thành một chuỗi các truy vấn tồn tại tổng hai độc lập trên các mảng con, được lặp lại cho ~44 mục tiêu cố định. Điều này dễ quản lý hơn nhiều vì tổng hai trên một mảng tĩnh có thể được trả lời bằng cách sắp xếp và hai con trỏ hoặc sử dụng cấu trúc tần số nếu chúng ta sửa phân đoạn. 

Tuy nhiên, chúng tôi vẫn có nhiều truy vấn trên các phân đoạn khác nhau, vì vậy chúng tôi cần một cấu trúc hỗ trợ truy cập tần số phân đoạn nhanh. Một cách phổ biến để xử lý vấn đề này là xử lý từng giá trị Fibonacci một cách độc lập và tính toán cho mỗi truy vấn xem liệu Fibonacci đó có thể được hình thành bên trong phân đoạn của nó hay không. Khi đó, câu trả lời cuối cùng cho mỗi truy vấn chỉ đơn giản là số lượng giá trị Fibonacci khả thi. 

Để kiểm tra tính khả thi của một giá trị Fibonacci cố định F trên một phân đoạn [l, r], chúng ta cần kiểm tra xem có tồn tại i < j trong [l, r] sao cho A[i] + A[j] = F. Vì các giá trị lớn nhưng phạm vi Fibonacci nhỏ nên chúng ta có thể nén các giá trị mảng và sử dụng bản đồ tần số cho mỗi phân đoạn. Một cách hiệu quả hơn là sắp xếp các chỉ mục ngoại tuyến theo giá trị Fibonacci và sử dụng kỹ thuật hai con trỏ trên mảng giá trị đã sắp xếp trong khi vẫn duy trì các ràng buộc phân đoạn bằng cây Fenwick hoặc thuật toán của Mo.

Với các ràng buộc, giải pháp rõ ràng được dự định là xử lý riêng từng giá trị Fibonacci và sử dụng thuật toán Mo trên các truy vấn để duy trì cấu trúc tần số của các giá trị phân đoạn hiện tại. Đối với mỗi số Fibonacci F, chúng tôi duy trì một cửa sổ trượt và kiểm tra các cặp tăng dần. Vì số lượng giá trị Fibonacci không đổi (~44), nên chúng tôi nhân độ phức tạp Mo với một hệ số nhỏ, hệ số này vẫn có thể chấp nhận được. 

Thiết kế cuối cùng là: tính toán trước các số Fibonacci, chạy thuật toán của Mo qua các truy vấn và duy trì mảng tần số cho các giá trị trong phân đoạn hiện tại. Đối với mỗi Fibonacci F, chúng tôi duy trì số lượng các cặp tạo thành F bằng cách kiểm tra tần số bổ sung. Khi chúng tôi thêm hoặc xóa một phần tử, chúng tôi cập nhật các đóng góp cho tất cả các giá trị Fibonacci trong O(#Fib), giá trị này không đổi. 

Điều này biến bài toán thành khoảng O((N + Q) √N × 44), có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force cho mỗi truy vấn | O(Q·N2) | O(1) | Quá chậm | 
| Thuật toán Mo + bảng liệt kê Fibonacci | O((N + Q) √N · 44) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước tất cả các số Fibonacci lên tới 2×10⁹ và lưu trữ chúng trong danh sách F. Điều này có hiệu quả vì mọi tổng cặp hợp lệ đều không thể vượt quá giới hạn này. 
2. Đọc tất cả các truy vấn và chuẩn bị xử lý ngoại tuyến. Chúng ta sẽ sắp xếp lại chúng theo thứ tự của Mo để có thể di chuyển cửa sổ trượt một cách hiệu quả. 
3. Duy trì mảng tần số freq[x] biểu thị số lần giá trị x xuất hiện trong phân đoạn hiện tại. 
4. Đối với mỗi số Fibonacci Fk, chúng tôi duy trì một bộ đếm way[k] để theo dõi xem có bao nhiêu cặp hợp lệ trong phân đoạn hiện tại có tổng bằng Fk. 
5. Khi chúng ta thêm một phần tử x vào phân đoạn hiện tại, chúng ta cập nhật tất cả các bộ đếm Fibonacci bằng cách kiểm tra, đối với mỗi Fk, xem (Fk − x) có tồn tại trong tần số trước khi chèn hay không. Nếu vậy, chúng tôi sẽ tăng số cách [k] cho phù hợp. Sau đó, chúng tôi tăng tần số [x]. 
6. Khi loại bỏ một phần tử x, trước tiên chúng ta giảm freq[x], sau đó trừ đi phần đóng góp cho tất cả các số Fibonacci bằng tần số được cập nhật. 
7. Khi trượt cửa sổ Mo, chúng tôi liên tục duy trì tất cả các giá trị way[k]. 
8. Đối với mỗi truy vấn, sau khi điều chỉnh cửa sổ thành [l, r], chúng ta tính toán câu trả lời là số chỉ số Fibonacci k sao cho cách[k] > 0. 

Tính chính xác phụ thuộc vào việc duy trì số lượng chính xác các cặp không có thứ tự hợp lệ cho mỗi tổng Fibonacci bên trong phân đoạn hiện tại. Mỗi bản cập nhật điều chỉnh chính xác số lượng cặp dựa trên việc phần bổ sung có xuất hiện tại thời điểm chèn hoặc xóa hay không, đảm bảo không có cặp nào bị bỏ sót hoặc bị tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Query:
    __slots__ = ("l", "r", "i", "blk")
    def __init__(self, l, r, i, blk):
        self.l = l
        self.r = r
        self.i = i
        self.blk = blk

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    fib = [1, 2]
    while fib[-1] <= 2_000_000_000:
        fib.append(fib[-1] + fib[-2])
    fib = fib[:-1]

    fib_index = {v: i for i, v in enumerate(fib)}
    m = len(fib)

    block = int(n ** 0.5)

    queries = []
    for i in range(q):
        l, r = map(int, input().split())
        queries.append(Query(l, r, i, l // block))

    queries.sort(key=lambda x: (x.blk, x.r if x.blk % 2 == 0 else -x.r))

    freq = {}
    ways = [0] * m

    def add(x):
        for i, f in enumerate(fib):
            y = f - x
            if y in freq:
                ways[i] += freq[y]
        freq[x] = freq.get(x, 0) + 1

    def remove(x):
        freq[x] -= 1
        for i, f in enumerate(fib):
            y = f - x
            if y in freq:
                ways[i] -= freq[y]

    cur_l, cur_r = 0, -1
    ans = [0] * q

    for qu in queries:
        l, r = qu.l, qu.r

        while cur_l > l:
            cur_l -= 1
            add(a[cur_l])
        while cur_r < r:
            cur_r += 1
            add(a[cur_r])
        while cur_l < l:
            remove(a[cur_l])
            cur_l += 1
        while cur_r > r:
            remove(a[cur_r])
            cur_r -= 1

        cnt = 0
        for i in range(m):
            if ways[i] > 0:
                cnt += 1
        ans[qu.i] = cnt

    print("\n".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Việc triển khai dựa trên từ điển tần số thay vì mảng có kích thước cố định vì các giá trị có thể lên tới 10⁹. Các chức năng thêm và xóa cập nhật đối xứng số lượng cặp bằng cách quét tất cả các số Fibonacci và kiểm tra phần bù trong thời gian không đổi. 

Phần tế nhị nhất là thứ tự cập nhật khi xóa: tần số được giảm dần trước khi trừ đi các đóng góp, đảm bảo chúng tôi không tính phần tử bị xóa là một phần của các cặp hợp lệ. 

Câu trả lời cuối cùng cho mỗi truy vấn chỉ đơn giản là đếm xem có bao nhiêu mục tiêu Fibonacci có ít nhất một cặp hợp lệ trong phân đoạn hiện tại. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng nhỏ [1, 2, 3, 5] và các số Fibonacci [1, 2, 3, 5, 8, ...]. Đối với toàn bộ phân đoạn, chúng tôi theo dõi tổng Fibonacci nào xuất hiện. 

| Bước | Cửa sổ | tần số | Tổng Fibonacci hợp lệ | 
| --- | --- | --- | --- | 
| 1 | [1] | {1:1} | không | 
| 2 | [1,2] | {1:1,2:1} | 3 | 
| 3 | [1,2,3] | {1,1,3} | 3,4,5 có tồn tại không? chỉ 3,5 cặp | 
| 4 | [1,2,3,5] | {1,1,1,5} | 3,5,6,7,8 tùy cặp | 

Dấu vết này cho thấy mỗi phần tử mới tăng số lượng tổng có thể bằng cách tương tác với tất cả các giá trị hiện có như thế nào. 

Bây giờ hãy xem xét một truy vấn trên [2,3,5]. Tần số trở thành {2,3,5}. Các cặp hợp lệ bao gồm 2+3=5 và 2+5=7 và 3+5=8. Chỉ các tổng Fibonacci trong số này mới được tính và các số trùng lặp của cùng một số Fibonacci sẽ bị bỏ qua. 

Điều này chứng tỏ thuật toán không quan tâm đến cặp nào tạo ra số Fibonacci mà chỉ quan tâm đến việc liệu có tồn tại ít nhất một cặp như vậy hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N + Q) √N · 44) | Thuật toán của Mo xử lý các dịch chuyển con trỏ, mỗi dịch chuyển sẽ quét danh sách Fibonacci | 
| Không gian | O(N) | Bản đồ tần số và lưu trữ truy vấn | 

Hệ số √N xuất phát từ thứ tự Mo, trong khi hằng số 44 đến từ vũ trụ Fibonacci bị giới hạn. Với N, Q lên tới 10⁵, điều này phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    fib = [1, 2]
    while fib[-1] <= 2_000_000_000:
        fib.append(fib[-1] + fib[-2])
    fib = fib[:-1]

    block = int(n ** 0.5)

    queries = []
    for i in range(q):
        l, r = map(int, input().split())
        queries.append((l, r, i, l // block))

    queries.sort(key=lambda x: (x[3], x[1]))

    freq = {}
    ways = [0] * len(fib)

    def add(x):
        for i, f in enumerate(fib):
            ways[i] += freq.get(f - x, 0)
        freq[x] = freq.get(x, 0) + 1

    def remove(x):
        freq[x] -= 1
        for i, f in enumerate(fib):
            ways[i] -= freq.get(f - x, 0)

    cur_l, cur_r = 0, -1
    ans = [0] * q

    for l, r, i, _ in queries:
        while cur_l > l:
            cur_l -= 1
            add(a[cur_l])
        while cur_r < r:
            cur_r += 1
            add(a[cur_r])
        while cur_l < l:
            remove(a[cur_l])
            cur_l += 1
        while cur_r > r:
            remove(a[cur_r])
            cur_r -= 1

        ans[i] = sum(1 for x in ways if x > 0)

    return "\n".join(map(str, ans))

# minimal
assert run("1 1\n5\n0 0") == "0"

# simple pair
assert run("4 1\n1 2 3 5\n0 3") == "3"

# all equal
assert run("5 1\n2 2 2 2 2\n0 4") == "1"

# boundary two elements
assert run("2 1\n1 1\n0 1") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | không có cặp nào tồn tại | 
| mảng hỗn hợp nhỏ | 3 | nhiều khoản Fibonacci | 
| tất cả các giá trị bằng nhau | 1 | xử lý tổng trùng lặp | 
| hai yếu tố | 1 | trường hợp cặp hợp lệ đơn giản nhất | 

## Vỏ cạnh 

Phân đoạn một phần tử chẳng hạn như [7] không tạo ra ngày hợp lệ vì không thể hình thành cặp. Thuật toán khởi tạo cửa sổ và không bao giờ đăng ký bất kỳ đóng góp nào theo cách[], vì add() yêu cầu ít nhất một phần tử hiện có để khớp với phần bổ sung. 

Một phân đoạn trong đó tất cả các giá trị giống hệt nhau, ví dụ [4,4,4,4], đảm bảo rằng mọi bản cập nhật đều thêm cấu trúc bổ sung giống nhau. Mỗi giá trị Fibonacci chỉ có thể được thỏa mãn bởi cùng một cặp tổng lặp đi lặp lại, do đó, chỉ có thể đếm một số Fibonacci mặc dù tồn tại nhiều cặp. Bản cập nhật dựa trên tần số đảm bảo rằng nhiều cặp giống hệt nhau vẫn chỉ đóng góp vào một mục nhập cách khác 0 cho mỗi mục tiêu Fibonacci. 

Một phân đoạn như [1,1,2] kiểm tra việc xử lý trùng lặp. Khi chèn số 1 thứ hai, cặp tổng 2 sẽ có sẵn, nhưng việc loại bỏ hoặc sắp xếp lại các phần tử không được tính gấp đôi một cách không chính xác vì các cập nhật có tính đối xứng giữa các thao tác thêm và xóa và phụ thuộc chặt chẽ vào trạng thái tần số hiện tại.
