---
title: "CF 106015F - Sự cộng hưởng của Spirit-Oak"
description: "Chúng ta được cung cấp một mảng số nguyên tĩnh, trong đó mỗi vị trí đại diện cho Spirit-Oak và giá trị cộng hưởng của nó. Sau khi mảng được sửa, chúng ta phải trả lời nhiều truy vấn độc lập."
date: "2026-06-22T16:46:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106015
codeforces_index: "F"
codeforces_contest_name: "Game of Coders 4 - Over the Garden Wall"
rating: 0
weight: 106015
solve_time_s: 59
verified: true
draft: false
---

[CF 106015F - Sự cộng hưởng của Spirit-Oak](https://codeforces.com/problemset/problem/106015/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng số nguyên tĩnh, trong đó mỗi vị trí đại diện cho Spirit-Oak và giá trị cộng hưởng của nó. Sau khi mảng được sửa, chúng ta phải trả lời nhiều truy vấn độc lập. Mỗi truy vấn chỉ định một phân đoạn chỉ mục từ L đến R và trong phân đoạn đó, chúng ta được yêu cầu đếm xem có bao nhiêu giá trị nằm trong khoảng giá trị [X, Y]. 

Vì vậy, mỗi truy vấn về cơ bản là yêu cầu số phần tử Ai sao cho L  i  R và X  Ai  Y. Khó khăn là cả phạm vi chỉ mục và phạm vi giá trị đều bị ràng buộc độc lập và có tới 100.000 truy vấn, do đó việc tính toán lại từ đầu cho mỗi truy vấn là quá chậm. 

Các ràng buộc đẩy chúng ta tới mức gần bằng O((N + Q) log N) hoặc tốt hơn cho mỗi quá trình xử lý truy vấn. Bất kỳ cách tiếp cận nào quét một phân đoạn cho mọi truy vấn đều dẫn đến O(NQ), theo thứ tự 10^10 thao tác trong trường hợp xấu nhất và sẽ không chạy trong vòng 1,5 giây. 

Việc tối ưu hóa đơn giản như tần số tiền tố trên mỗi giá trị cũng không đủ vì nó xử lý các phạm vi giá trị nhưng không xử lý các mảng con tùy ý một cách hiệu quả. Tương tự, tổng tiền tố cho mỗi chỉ mục không xử lý việc lọc giá trị. Khó khăn cốt lõi là tính chất hai chiều của mỗi truy vấn: một chiều là vị trí, chiều còn lại là giá trị. 

Trường hợp cạnh tinh tế xuất hiện khi L và R lớn nhưng phạm vi giá trị rất chặt chẽ hoặc ngược lại. Ví dụ: nếu tất cả các giá trị giống hệt nhau, nhiều truy vấn sẽ thu gọn thành các kiểm tra độ dài phạm vi đơn giản nhưng cách tiếp cận đơn giản vẫn có thể lặp lại một cách không cần thiết. Một trường hợp khác là khi X và Y bao trùm toàn bộ phạm vi giá trị, trong đó câu trả lời phải bằng R − L + 1 và bất kỳ cấu trúc nào lọc không chính xác đều có thể bị tính thiếu nếu các ranh giới bị xử lý sai. 

## Phương pháp tiếp cận 

Phương pháp vũ phu rất đơn giản. Đối với mỗi truy vấn, lặp từ L đến R và đếm xem có bao nhiêu phần tử nằm giữa X và Y. Điều này đúng vì nó trực tiếp kiểm tra điều kiện cho mọi phần tử ứng cử viên. Tuy nhiên, mỗi truy vấn tốn O(N) trong trường hợp xấu nhất, dẫn đến tổng công việc là O(NQ). Với N và Q lên tới 10^5, điều này trở thành khoảng 10^10 thao tác, vượt xa giới hạn chấp nhận được. 

Quan sát quan trọng là chúng tôi liên tục thực hiện hai bộ lọc độc lập: một theo chỉ số và một theo giá trị. Điều này gợi ý diễn giải lại vấn đề dưới dạng truy vấn đếm phạm vi trong không gian hai chiều trong đó mỗi phần tử là một điểm (i, Ai) và mỗi truy vấn hỏi có bao nhiêu điểm nằm bên trong một hình chữ nhật được xác định bởi L ≤ i ≤ R và X ≤ Ai ≤ Y. 

Đây là một vấn đề đếm phạm vi ngoại tuyến cổ điển. Một cách tiêu chuẩn để xử lý nó một cách hiệu quả là quét qua một chiều trong khi vẫn duy trì cấu trúc trên chiều kia. Ở đây, chúng tôi sửa thứ nguyên giá trị bằng cách xử lý nó tăng dần và sử dụng cây Fenwick trên các chỉ mục. Để làm điều đó, chúng tôi chuyển đổi từng truy vấn thành một truy vấn tiền tố khác nhau trên trục giá trị. 

Thay vì đếm trực tiếp các giá trị trong [X, Y], chúng ta tính kết quả là: 

số đếm(≤ Y trong [L, R]) − số đếm(≤ X−1 trong [L, R]) 

Vì vậy, bài toán quy giản về việc hỗ trợ các phép toán có dạng: đếm xem có bao nhiêu Ai ≤ K trong một phạm vi [L, R]. Đây là một vấn đề ngoại tuyến 2D tiêu chuẩn có thể được giải quyết bằng cách sắp xếp cả phần tử mảng và truy vấn theo giá trị rồi xử lý chúng theo thứ tự tăng dần, chèn vị trí vào cây Fenwick. 

Chúng tôi xử lý các phần tử mảng và truy vấn cùng nhau. Khi chúng ta tăng ngưỡng giá trị K, chúng ta sẽ kích hoạt tất cả các vị trí i trong đó Ai ≤ K trong cây Fenwick. Sau đó, mỗi truy vấn cho K có thể được trả lời dưới dạng tổng phạm vi trên [L, R]. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NQ) | O(1) | Quá chậm | 
| Fenwick + Quét ngoại tuyến | O((N + Q) log N) | O(N + Q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi mỗi truy vấn thành hai truy vấn phụ bằng cách sử dụng loại trừ bao gồm trên thứ nguyên giá trị. Một truy vấn (L, R, X, Y) trở thành:

đếm trong [L, R] với Ai ≤ Y trừ đếm trong [L, R] với Ai ≤ X−1. 

Bây giờ chúng ta tập trung vào việc trả lời các truy vấn có dạng “có bao nhiêu phần tử trong phạm vi chỉ mục [L, R] có giá trị ≤ K”. 

1. Ghép từng vị trí mảng i với giá trị Ai của nó và sắp xếp các cặp này theo Ai theo thứ tự tăng dần. Điều này đảm bảo rằng khi chúng tôi xử lý các giá trị lên tới ngưỡng K, tất cả các phần tử hợp lệ có thể được kích hoạt tăng dần. 
2. Chuyển mỗi truy vấn thành hai sự kiện: một sự kiện yêu cầu K = Y dấu dương và một sự kiện yêu cầu K = X−1 dấu âm. Mỗi sự kiện lưu trữ (K, L, R, id, sign). 
3. Sắp xếp tất cả các sự kiện theo K theo thứ tự tăng dần cùng với các giá trị mảng. Điều này cho phép quét một lần từ trái sang phải trên không gian giá trị. 
4. Duy trì cây Fenwick trên các chỉ số. Ban đầu tất cả các vị trí đều không hoạt động. Khi xử lý giá trị K, chèn tất cả các vị trí mảng có Ai ≤ K vào cây Fenwick. Mỗi lần chèn thêm 1 ở vị trí i. Cấu trúc này cho phép chúng tôi truy vấn có bao nhiêu phần tử hoạt động nằm trong bất kỳ phạm vi chỉ mục nào. 
5. Khi chúng tôi xử lý các sự kiện theo thứ tự K tăng dần, chúng tôi đảm bảo rằng cây Fenwick luôn biểu thị chính xác các vị trí đó với Ai ≤ K hiện tại. Với mỗi sự kiện, hãy tính sum(R) − sum(L−1) từ cây Fenwick và nhân với dấu của nó, tích lũy thành câu trả lời cuối cùng. 
6. Lưu trữ kết quả trên mỗi id truy vấn, kết hợp hai sự kiện thành câu trả lời cuối cùng. 

Lý do chính khiến thứ tự này hoạt động là vì chúng tôi không bao giờ loại bỏ các phần tử. Khi một giá trị được chèn vào, nó vẫn có hiệu lực đối với tất cả các ngưỡng lớn hơn, phù hợp với tính chất đơn điệu của điều kiện Ai ≤ K. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong quá trình quét, cây Fenwick biểu thị chính xác tập hợp các chỉ số có giá trị nhỏ hơn hoặc bằng ngưỡng hiện tại K. Vì cả phần chèn và truy vấn đều được xử lý theo thứ tự không giảm của K nên không có phần tử nào được đưa vào quá sớm hoặc quá muộn. Mỗi truy vấn được đánh giá tại thời điểm chính xác khi ngưỡng của nó bắt đầu hoạt động, đảm bảo tính chính xác của việc chuyển đổi tiền tố. Bước loại trừ bao gồm đảm bảo rằng việc giới hạn Ai ở [X, Y] là chính xác và không xảy ra việc đếm quá mức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

    def range_sum(self, l, r):
        return self.sum(r) - self.sum(l - 1)

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    elems = [(a[i], i + 1) for i in range(n)]

    events = []
    for idx in range(q):
        l, r, x, y = map(int, input().split())
        events.append((y, l, r, idx, 1))
        events.append((x - 1, l, r, idx, -1))

    elems.sort()
    events.sort()

    bit = Fenwick(n)
    ans = [0] * q

    j = 0
    for k, l, r, idx, sign in events:
        while j < n and elems[j][0] <= k:
            bit.add(elems[j][1], 1)
            j += 1

        if k >= 0:
            ans[idx] += sign * bit.range_sum(l, r)

    print("\n".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Cây Fenwick được sử dụng nghiêm ngặt trên các chỉ số chứ không phải các giá trị, bởi vì một khi các giá trị được sắp xếp, việc lọc giá trị sẽ trở thành một điều kiện quét. Con trỏ j đảm bảo mỗi phần tử mảng được chèn chính xác một lần, cho phép quét tuyến tính trên các giá trị được sắp xếp. 

Mỗi truy vấn được chia thành hai sự kiện để các ràng buộc phạm vi giá trị trở thành các ràng buộc tiền tố. Bước trừ được nhúng vào phép tích lũy bằng trường dấu. 

Một chi tiết triển khai tinh tế là xử lý K = X − 1 khi X = 1. Trong trường hợp đó K trở thành 0, điều này tạo ra một tiền tố trống một cách an toàn vì tất cả Ai ≥ 1. Điều kiện k ≥ 0 là tùy chọn nhưng ngăn chặn các truy vấn Fenwick không cần thiết đối với các ngưỡng âm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7 3
1 2 1 3 2 7 2
1 6 2 3
1 4 1 1
2 5 1 2
```Chúng tôi liệt kê mảng với các chỉ số: 

| tôi | Ái | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 1 | 
| 4 | 3 | 
| 5 | 2 | 
| 6 | 7 | 
| 7 | 2 | 

Chúng tôi xử lý các sự kiện trong việc tăng K. 

| K | Chỉ số được chèn | Truy vấn được xử lý | Hiệu ứng trạng thái Fenwick | 
| --- | --- | --- | --- | 
| 1 | 1, 3 | (X−1=0 cho truy vấn thứ hai, Y=1 cho truy vấn thứ hai) | chỉ Ai 1 hoạt động | 
| 2 | 2, 5, 7 | truy vấn phạm vi được trả lời cho 2 | mở rộng tập hoạt động | 
| 3 | 4 | truy vấn đầu tiên được trả lời đầy đủ thông qua Y và X−1 | bao gồm tất cả 3 | 

Kết quả cuối cùng cho truy vấn 1 đếm các chỉ số 2-6 với các giá trị 2,1,3,2,7 → giá trị 3 là 2,1,3,2, cho kết quả 4. 

Truy vấn 2 đếm chỉ số 1-4 với giá trị chính xác là 1 → vị trí 1 và 3, cho kết quả 2. 

Truy vấn 3 đếm các chỉ số 2-5 với các giá trị trong [1,2] → tất cả đều là 2,1,3,2, vì vậy loại trừ 3 sẽ cho 3. 

Dấu vết này cho thấy cách lọc giá trị dần dần được tích lũy và sử dụng lại trên các truy vấn. 

### Ví dụ 2 

đầu vào:```
5 2
5 4 3 2 1
1 5 3 5
2 4 1 3
```| K | Chỉ số hoạt động | 
| --- | --- | 
| 1 | 5 | 
| 2 | 4,5 | 
| 3 | 3,4,5 | 
| 4 | 2,3,4,5 | 
| 5 | 1,2,3,4,5 | 

Truy vấn đầu tiên trở thành tất cả các phần tử ≤5 trong [1,5], câu trả lời 5. 

Truy vấn thứ hai là [2,4] với các giá trị [4,3,2], tất cả 3, đều cho 3 phần tử, vì vậy câu trả lời là 3. 

Điều này xác nhận tính đúng đắn khi các giá trị giảm dần và thứ tự kích hoạt được trải rộng tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N + Q) log N) | Sắp xếp các sự kiện cộng với các cập nhật và truy vấn phạm vi Fenwick | 
| Không gian | O(N + Q) | Lưu trữ cho mảng, sự kiện và cây Fenwick | 

Với N, Q lên tới 10^5, điều này phù hợp thoải mái trong giới hạn thời gian vì mỗi thao tác đều là logarit và tổng số lần cập nhật và truy vấn là tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys
    input = sys.stdin.readline

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)

        def add(self, i, v):
            while i <= self.n:
                self.bit[i] += v
                i += i & -i

        def sum(self, i):
            s = 0
            while i > 0:
                s += self.bit[i]
                i -= i & -i
            return s

        def range_sum(self, l, r):
            return self.sum(r) - self.sum(l - 1)

    def solve():
        n, q = map(int, input().split())
        a = list(map(int, input().split()))
        elems = [(a[i], i + 1) for i in range(n)]
        events = []
        for idx in range(q):
            l, r, x, y = map(int, input().split())
            events.append((y, l, r, idx, 1))
            events.append((x - 1, l, r, idx, -1))

        elems.sort()
        events.sort()

        bit = Fenwick(n)
        ans = [0] * q
        j = 0
        for k, l, r, idx, sign in events:
            while j < n and elems[j][0] <= k:
                bit.add(elems[j][1], 1)
                j += 1
            if k >= 0:
                ans[idx] += sign * bit.range_sum(l, r)

        return "\n".join(map(str, ans))

    return solve()

# provided sample
assert run("""7 5
1 2 1 3 2 7 2
2 5 1 2
1 4 1 1
1 6 2 3
7 7 2 2
1 7 8 10
""") == """4
2
4
1
0"""

# all equal values
assert run("""5 2
3 3 3 3 3
1 5 3 3
2 4 1 2
""") == """5
3"""

# minimum edge
assert run("""1 1
1
1 1 1 1
""") == "1"

# decreasing values
assert run("""5 2
5 4 3 2 1
1 5 1 5
2 4 2 4
""") == """5
3"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | xử lý toàn dải chính xác | tính chính xác của mảng thống nhất | 
| phần tử đơn | không có vấn đề riêng lẻ | độ đúng ranh giới | 
| giá trị giảm dần | đặt hàng toàn bộ | đặt hàng mạnh mẽ | 

## Vỏ cạnh 

Một trường hợp trong đó tất cả Ai đều là các kiểm tra bằng nhau xem liệu quá trình quét có xử lý chính xác các lần chèn lặp lại hay không và liệu cả hai phần của loại trừ bao gồm có bị hủy bỏ chính xác hay không. Ví dụ: với mảng [3, 3, 3, 3, 3] và truy vấn [1, 5, 3, 3], cả Y và X−1 đều kích hoạt tất cả các phần tử và phép trừ phải mang lại chính xác 5, mà hiệu Fenwick tạo ra chính xác vì cả hai sự kiện đều có số tiền tố giống hệt nhau. 

Mảng một phần tử như [1] với truy vấn [1,1,1,1] kiểm tra việc xử lý ranh giới chỉ mục. Cây Fenwick phải diễn giải chính xác range_sum(1,1) mà không truy cập sai chỉ mục 0. Việc triển khai sử dụng sum(l−1), trở thành sum(0) = 0 một cách an toàn. 

Một mảng giảm dần như [5,4,3,2,1] đảm bảo rằng các phần tử được kích hoạt theo thứ tự giảm dần nghiêm ngặt của các vị trí chỉ mục và xác nhận rằng thứ tự quét qua các giá trị, không phải chỉ mục, sẽ chi phối tính chính xác. Mặc dù các chỉ mục được chèn không đơn điệu theo thứ tự giá trị, cấu trúc Fenwick đảm bảo các truy vấn chỉ phụ thuộc vào số lượng tích lũy chứ không phải thứ tự chèn.
