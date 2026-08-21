---
title: "CF 104069E - Máy phân loại El"
description: "Chúng tôi đang quản lý một bộ sưu tập các cỡ giày được lưu trữ trong một cấu trúc giống như nhiều bộ nơi việc xóa bỏ là vĩnh viễn. Mỗi khi khách hàng đến, họ chỉ định cỡ giày tối thiểu có thể chấp nhận được và chúng tôi phải cung cấp cho họ chiếc giày nhỏ nhất có sẵn có cỡ ít nhất là ngưỡng đó."
date: "2026-07-02T02:59:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104069
codeforces_index: "E"
codeforces_contest_name: "VII MaratonUSP Freshman Contest"
rating: 0
weight: 104069
solve_time_s: 43
verified: true
draft: false
---

[CF 104069E - El Classificador](https://codeforces.com/problemset/problem/104069/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang quản lý một bộ sưu tập các cỡ giày được lưu trữ trong một cấu trúc giống như nhiều bộ nơi việc xóa bỏ là vĩnh viễn. Mỗi khi khách hàng đến, họ chỉ định cỡ giày tối thiểu có thể chấp nhận được và chúng tôi phải cung cấp cho họ chiếc giày nhỏ nhất có sẵn có cỡ ít nhất là ngưỡng đó. Khi một chiếc giày được chỉ định, nó sẽ biến mất khỏi kho và không thể sử dụng lại được. 

Đầu vào đưa ra một mảng kích cỡ giày ban đầu và sau đó là một chuỗi truy vấn. Mỗi truy vấn độc lập ở đầu vào nhưng không độc lập trong quá trình thực thi, vì các phép gán trước đó sẽ thay đổi nhóm có sẵn cho các truy vấn sau này. Đối với mỗi truy vấn, chúng tôi xuất ra kích thước của chiếc giày được chỉ định hoặc −1 nếu không có chiếc giày phù hợp. 

Các ràng buộc cho phép tối đa 200.000 giày và 200.000 truy vấn, với giá trị lên tới 10^9. Bất kỳ giải pháp nào cố gắng quét toàn bộ mảng cho mọi truy vấn sẽ thực hiện theo thứ tự các phép toán n·q, tức là lên tới 4·10^10 phép so sánh trong trường hợp xấu nhất. Điều đó vượt xa những gì có thể chạy trong vài giây bằng Python hoặc thậm chí trong C++ được tối ưu hóa. 

Cách tiếp cận đơn giản sẽ thất bại trong trường hợp đơn giản như danh sách đã được sắp xếp với số truy vấn giảm dần. Ví dụ: nếu giày là [1, 2, 3, 4, 5] và truy vấn đều là 5 thì mỗi truy vấn sẽ quét toàn bộ danh sách để tìm phần tử hợp lệ cuối cùng còn lại. Mặc dù câu trả lời là hiển nhiên nhưng việc quét tuyến tính lặp đi lặp lại vẫn chiếm ưu thế trong thời gian chạy. 

Trường hợp cạnh tinh tế được lặp lại các giá trị bằng nhau. Nếu đầu vào là [4, 4, 4] và truy vấn là [4, 4, 4, 4], hành vi đúng là sử dụng một 4 cho mỗi truy vấn cho đến khi hết, sau đó trả về −1. Một giải pháp ngây thơ mà quên đánh dấu các phần tử là đã bị loại bỏ hoặc xử lý không chính xác các phần tử trùng lặp sẽ sử dụng lại cùng một chiếc giày nhiều lần hoặc bỏ qua các kết quả trùng khớp hợp lệ. 

## Phương pháp tiếp cận 

Chiến lược brute-force rất đơn giản: đối với mỗi truy vấn, quét toàn bộ mảng từ trái sang phải, tìm phần tử đầu tiên có ít nhất x, xuất phần tử đó và xóa phần tử đó khỏi mảng. Việc loại bỏ có thể được thực hiện bằng cách xóa vật lý phần tử hoặc đánh dấu nó. Tính đúng đắn là ngay lập tức vì chúng ta mô phỏng trực tiếp quy tắc được đưa ra trong câu lệnh. 

Điểm thất bại là hiệu suất. Mỗi truy vấn có chi phí O(n) trong trường hợp xấu nhất và có q truy vấn, dẫn đến O(nq). Với n và q đều lên tới 2·10^5 thì điều này trở nên không khả thi. 

Quan sát quan trọng là chúng ta cần lặp đi lặp lại hai thao tác trên một tập hợp giá trị động: tìm phần tử nhỏ nhất ít nhất là x và loại bỏ phần tử đó. Đây chính xác là một truy vấn giống như truy vấn trước đó trên một tập hợp thứ tự động. Khi chúng tôi sắp xếp mảng ban đầu, vấn đề sẽ giảm xuống còn việc duy trì cấu trúc hỗ trợ tìm kiếm giới hạn dưới và xóa một cách hiệu quả. 

Cây tìm kiếm nhị phân cân bằng hoặc nhiều bộ hỗ trợ trực tiếp điều này, nhưng trong lập trình cạnh tranh, chúng ta có thể mô phỏng nó một cách hiệu quả bằng cách sử dụng cấu trúc được sắp xếp cộng với tìm kiếm nhị phân. Tuy nhiên, việc xóa trong danh sách Python là tuyến tính, vì vậy chúng ta cần một cấu trúc trong đó việc xóa không làm dịch chuyển các phân đoạn lớn. Cây phân đoạn theo số tần số hoặc vùng chứa được sắp xếp với chia đôi cộng với xóa lười cũng hoạt động, nhưng cách tiếp cận rõ ràng nhất là cây phân đoạn trên miền giá trị nén hoặc cây Fenwick lưu trữ số lần xuất hiện, cho phép chúng tôi xác định giá trị sẵn có tiếp theo thông qua nâng cấp nhị phân. 

Chúng tôi nén các giá trị vì kích thước lên tới 10^9 nhưng chỉ tồn tại 2·10^5 giá trị riêng biệt. Sau khi nén, chúng tôi duy trì tần số. Mỗi truy vấn sẽ trở thành một “tìm chỉ mục đầu tiên có tổng tiền tố ≥ vị trí đích theo thứ tự giá trị bị ràng buộc bởi x”. Trước tiên, chúng tôi xác định chỉ mục nén đầu tiên có giá trị ≥ x bằng cách sử dụng tìm kiếm nhị phân, sau đó tìm chỉ mục hoạt động có sẵn tiếp theo bằng cách sử dụng bước đi kiểu “kth” của cây Fenwick. Điều đó mang lại cho chúng tôi chiếc giày hợp lệ nhỏ nhất hiện có và chúng tôi giảm số lượng của nó. 

Điều này làm giảm mỗi truy vấn xuống O(log n), tạo ra tổng O((n + q) log n).

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Tối ưu (Mô phỏng Fenwick / multiset) | O((n+q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi sắp xếp và nén các kích cỡ giày để có thể suy luận về chúng theo thứ tự tăng dần trong khi vẫn ánh xạ trở lại giá trị ban đầu. Mỗi kích thước riêng biệt sẽ có một chỉ mục trong một mảng được sắp xếp. 

Chúng tôi xây dựng cây Fenwick dựa trên các chỉ số này, trong đó mỗi vị trí lưu trữ số lượng giày có kích cỡ đó hiện có. 

Với mỗi truy vấn x, chúng ta dịch x thành chỉ mục đầu tiên trong mảng nén có giá trị ít nhất là x. Từ thời điểm đó trở đi, chúng ta chỉ quan tâm đến hậu tố của cấu trúc, vì mọi thứ trước x đều không hợp lệ đối với truy vấn này. 

Sau đó, chúng tôi sử dụng cây Fenwick để tìm vị trí đầu tiên tại hoặc sau chỉ số đó mà vẫn có số dương. Đây là thao tác tiêu chuẩn “tìm tiền tố đầu tiên trong đó tần số tích lũy đạt đến mục tiêu”, được điều chỉnh để bắt đầu từ chỉ số giới hạn dưới thay vì từ 1. 

Khi xác định được vị trí đó, chúng tôi xuất giá trị của nó và giảm tần số của nó trong cây Fenwick. 

Nếu không có vị trí như vậy tồn tại, chúng ta xuất ra −1. 

### Tại sao nó hoạt động 

Ở mỗi bước, cây Fenwick duy trì chính xác nhiều bộ giày có sẵn. Bước tìm kiếm nhị phân đảm bảo chúng tôi không bao giờ xem xét các giá trị nhỏ hơn ngưỡng truy vấn. Việc truyền tải “giống thứ k” đảm bảo chúng tôi chọn chỉ mục nhỏ nhất vẫn còn tồn tại trong số các ứng cử viên hợp lệ. Vì mỗi lần xóa sẽ cập nhật cấu trúc ngay lập tức nên các truy vấn trong tương lai luôn phản ánh chính xác khoảng không quảng cáo còn lại, do đó mô phỏng khớp chính xác với định nghĩa vấn đề. 

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

    def kth(self, k):
        pos = 0
        bitmask = 1 << (self.n.bit_length())
        while bitmask:
            nxt = pos + bitmask
            if nxt <= self.n and self.bit[nxt] < k:
                k -= self.bit[nxt]
                pos = nxt
            bitmask >>= 1
        return pos + 1

def lower_bound(arr, x):
    lo, hi = 0, len(arr)
    while lo < hi:
        mid = (lo + hi) // 2
        if arr[mid] >= x:
            hi = mid
        else:
            lo = mid + 1
    return lo

def main():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    vals = sorted(set(a))
    idx = {v: i + 1 for i, v in enumerate(vals)}

    fw = Fenwick(len(vals))

    for v in a:
        fw.add(idx[v], 1)

    for _ in range(q):
        x = int(input())
        pos = lower_bound(vals, x)
        if pos == len(vals):
            print(-1)
            continue

        # convert to Fenwick index range [pos+1 ... end]
        # we need first active in suffix, so we compare counts
        total_before = fw.sum(pos)
        total_all = fw.sum(len(vals))
        if total_all - total_before <= 0:
            print(-1)
            continue

        # find (total_before + 1)-th alive element overall
        # but must ensure it's within suffix; this is guaranteed by construction
        k = total_before + 1
        i = fw.kth(k)

        if i < pos + 1:
            # fallback safety, should not happen
            i = fw.kth(total_before + 1)

        print(vals[i - 1])
        fw.add(i, -1)

if __name__ == "__main__":
    main()
```Cây Fenwick duy trì tần số của từng cỡ giày ở dạng nén. các`lower_bound`hàm định vị chỉ mục kích thước đủ điều kiện đầu tiên. Sau đó, chúng tôi tính toán có bao nhiêu đôi giày hợp lệ tồn tại trước chỉ mục đó và sử dụng truy vấn thứ k toàn cục để chọn chiếc giày có sẵn tiếp theo. Điều này có tác dụng vì tất cả các ứng cử viên không hợp lệ đều bị bỏ qua một cách hiệu quả bằng cách hạn chế thứ hạng ban đầu. 

Một điều tinh tế phổ biến là đảm bảo chúng ta không vô tình chọn một chiếc giày nhỏ hơn x. Đó là lý do tại sao chúng tôi so sánh rõ ràng số lượng tiền tố và bắt đầu lựa chọn từ thứ hạng hợp lệ đầu tiên. Một điểm tinh tế khác là xử lý các bản sao một cách chính xác: cây Fenwick lưu trữ bội số, do đó, các kích thước giống hệt nhau sẽ được tiêu thụ một cách tự nhiên từng cái một. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3
1 2 3 4 5
2
4
3
```Chúng tôi nén các giá trị [1,2,3,4,5] trực tiếp. 

| Truy vấn | x | tư thế giới hạn dưới | số tiền tố | chỉ số đã chọn | đầu ra | nhiều tập còn lại | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 1 | 2 | 2 | [1,3,4,5] | 
| 2 | 4 | 4 | 2 | 4 | 4 | [1,3,5] | 
| 3 | 3 | 3 | 1 | 3 | 3 | [1,5] | 

Dấu vết này cho thấy cấu trúc luôn bỏ qua các phần tử bị loại bỏ và tôn trọng ràng buộc tối thiểu như thế nào. 

### Ví dụ 2 

đầu vào:```
3 4
4 4 4
4
4
4
4
```| Truy vấn | x | số tiền tố trước x | đã chọn | đầu ra | còn lại | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 0 | Thứ nhất | 4 | [4,4] | 
| 2 | 4 | 0 | Thứ nhất | 4 | [4] | 
| 3 | 4 | 0 | Thứ nhất | 4 | [] | 
| 4 | 4 | 0 | không | -1 | [] | 

Điều này xác nhận việc xử lý chính xác các bản sao và sự cạn kiệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | mỗi lần chèn và truy vấn đều sử dụng các phép toán Fenwick | 
| Không gian | O(n) | giá trị nén cộng với cây Fenwick | 

Các ràng buộc cho phép tối đa 2·10^5 phép toán và các hệ số logarit khoảng 18 dễ dàng đủ nhanh trong Python khi được triển khai bằng các vòng lặp đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    import contextlib
    output = io.StringIO()
    with contextlib.redirect_stdout(output):
        main()
    return output.getvalue().strip()

# provided sample-style cases
assert run("5 3\n1 2 3 4 5\n2\n4\n3\n") == "2\n4\n3"
assert run("3 4\n4 4 4\n4\n4\n4\n4\n") == "4\n4\n4\n-1"

# custom edge cases
assert run("1 2\n10\n10\n10\n") == "10\n-1"
assert run("2 2\n1 100\n50\n50\n") == "100\n-1"
assert run("5 5\n5 4 3 2 1\n1\n1\n1\n1\n1\n") == "1\n2\n3\n4\n5"
assert run("4 3\n2 2 2 2\n3\n1\n2\n") == "-1\n2\n2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạn kiệt yếu tố đơn | 10, -1 | xóa bỏ ranh giới | 
| ngưỡng giá trị hỗn hợp | 100, -1 | độ đúng giới hạn dưới | 
| đảo ngược thứ tự tiêu thụ đầy đủ | 1..5 | logic thứ k lặp lại | 
| trùng lặp + bỏ qua ngưỡng | -1,2,2 | xử lý trùng lặp | 

## Vỏ cạnh 

Đối với một chiếc giày trong hệ thống, thuật toán sẽ xử lý chính xác tình trạng kiệt sức ngay lập tức. Nếu đầu vào là`10`theo sau là hai truy vấn`10, 10`, cây Fenwick ban đầu có một phần tử tích cực. Truy vấn đầu tiên tìm thấy nó, loại bỏ nó và truy vấn thứ hai không thấy phần tử nào còn lại, trả về −1. 

Đối với các bản sao, cấu trúc xử lý từng lần xuất hiện một cách độc lập. Trong trường hợp như`[4,4,4]`với các truy vấn`[4,4,4,4]`, mỗi truy vấn thành công sẽ giảm tần suất đi đúng một. Truy vấn cuối cùng quan sát thấy tổng tiền tố bằng 0 vượt quá ngưỡng và xuất ra chính xác −1. 

Để bỏ qua ngưỡng, hãy xem xét`[1,100]`với truy vấn`50`. Giới hạn dưới bắt đầu từ 100, do đó thuật toán không bao giờ xét đến 1. Khi đó, cây Fenwick trực tiếp trả về 100 là phần tử hợp lệ đầu tiên, duy trì tính chính xác ngay cả khi các phần tử nhỏ hơn không hợp lệ tồn tại trước đó trong mảng.
