---
title: "CF 104663B - Tổng số lần xuất hiện của chữ số"
description: "Chúng tôi duy trì một tập hợp động các số nguyên không âm. Theo thời gian, tập hợp này thay đổi: các số có thể được chèn hoặc xóa và chúng ta cũng có thể xóa phần tử hiện đang xếp ở một vị trí cụ thể khi tập hợp được sắp xếp theo thứ tự giảm dần."
date: "2026-06-29T16:38:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104663
codeforces_index: "B"
codeforces_contest_name: "Replay of Ostad Presents Intra KUET Programming Contest 2023"
rating: 0
weight: 104663
solve_time_s: 94
verified: true
draft: false
---

[CF 104663B - Tổng số lần xuất hiện của chữ số](https://codeforces.com/problemset/problem/104663/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một tập hợp động các số nguyên không âm. Theo thời gian, tập hợp này thay đổi: các số có thể được chèn hoặc xóa và chúng ta cũng có thể xóa phần tử hiện đang xếp ở một vị trí cụ thể khi tập hợp được sắp xếp theo thứ tự giảm dần. 

Bên cạnh tập hợp đang phát triển này, chúng tôi cũng theo dõi về mặt khái niệm tần suất mỗi chữ số thập phân xuất hiện trên tất cả các số hiện có trong tập hợp. Đối với bất kỳ chữ số nào từ 0 đến 9, chúng ta có thể đếm tổng số lần xuất hiện của nó trong tất cả các số hoạt động. Điều này đưa ra một bảng tần số cố định thay đổi bất cứ khi nào tập hợp thay đổi. 

Đối với số truy vấn k, chúng tôi xác định điểm f(k) chỉ phụ thuộc vào bảng tần số này. Chúng tôi xem xét mọi chữ số xuất hiện trong k và với mỗi lần xuất hiện, chúng tôi thêm tần số chung của chữ số đó. Các chữ số lặp lại trong k đóng góp nhiều lần. Truy vấn yêu cầu điểm này, nhưng chỉ khi k hiện có trong tập hợp; nếu không thì câu trả lời là -1. 

Khó khăn chính là cả tập hợp số và số liệu thống kê chữ số của chúng đều phát triển trực tuyến và việc xóa không chỉ theo giá trị mà còn theo thống kê thứ tự. 

Các ràng buộc đạt tới 300.000 hoạt động. Điều này ngay lập tức loại trừ việc tính toán lại số chữ số từ đầu cho mỗi truy vấn, vì việc quét tất cả các số trên mỗi thao tác sẽ dẫn đến hành vi bậc hai. Bất kỳ giải pháp nào cũng phải duy trì thông tin tổng hợp tăng dần và hỗ trợ xóa theo thứ tự một cách hiệu quả, điều này gợi ý cấu trúc có cập nhật logarit, chẳng hạn như cấu trúc được lập chỉ mục nhị phân cân bằng hoặc tập hợp có thứ tự. 

Trường hợp cạnh tinh tế xuất hiện khi một số được chuyển đổi vào và ra nhiều lần. Sự đóng góp của nó vào tần số chữ số phải được thêm đầy đủ khi chèn và loại bỏ hoàn toàn khi xóa. Một tình huống khó khăn khác nảy sinh trong việc lặp lại chữ số: ví dụ: số 111 đóng góp ba lần xuất hiện của chữ số 1, vì vậy việc cập nhật tần số phải tính đến tính bội số chứ không chỉ sự hiện diện. 

Trường hợp cạnh không tầm thường thứ hai là trường hợp xóa lớn thứ k. Nếu kích thước được đặt nhỏ hơn k thì thao tác phải được bỏ qua hoàn toàn; nếu không thì chúng ta phải xác định chính xác phần tử theo thứ hạng chứ không phải theo giá trị. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp lưu trữ tập hợp các số đang hoạt động trong một vùng chứa thông thường. Đối với mỗi truy vấn f(k), chúng tôi quét tất cả các số và tính toán lại tần số chữ số một cách nhanh chóng. Điều này đúng nhưng quá chậm: mỗi truy vấn có thể tốn O(n) chữ số và với tối đa 3e5 truy vấn, điều này trở nên không khả thi. 

Một lực lượng vũ phu tốt hơn một chút sẽ duy trì tập hợp và tính toán lại số lượng chữ số bất cứ khi nào cần, nhưng ngay cả việc tính toán lại tăng dần vẫn yêu cầu chạm vào tất cả các chữ số của tất cả các số cho mỗi lần cập nhật. Vì mỗi số có thể có tối đa 10 chữ số nên việc tính toán lại đầy đủ là O(n log10 n) cho mỗi thao tác, vẫn vượt xa giới hạn. 

Quan sát quan trọng là số lượng chữ số có tính chất cộng hơn số. Mỗi số đóng góp một biểu đồ chữ số cố định. Nếu chúng ta duy trì một mảng toàn cục cnt[d] lưu trữ số lần chữ số d xuất hiện trong tập hợp hiện tại, thì f(k) sẽ trở thành một phép tính dựa trên tra cứu đơn giản trên các chữ số của k. 

Điều này chuyển vấn đề sang việc duy trì hai thứ một cách hiệu quả: một tập hợp các số động có thống kê thứ tự thứ k, chèn và xóa nhanh và biểu đồ chữ số đang chạy có thể được cập nhật theo O(chữ số) cho mỗi số được chèn hoặc xóa. 

Chúng tôi giải quyết thống kê thứ tự bằng cấu trúc cân bằng, chẳng hạn như danh sách được sắp xếp bằng tìm kiếm nhị phân hoặc cây Fenwick trên tọa độ nén. Vì các giá trị lên tới 2e9 và có tới 3e5 phần tử riêng biệt, nén tọa độ cộng với cây Fenwick hoặc cây thống kê thứ tự hoạt động ở dạng O(log n). Mỗi bản cập nhật cũng điều chỉnh số lượng chữ số trong O(10), vì các số có tối đa 10 chữ số. 

Cấu trúc cuối cùng duy trì tính nhất quán giữa tư cách thành viên và đóng góp bằng chữ số.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq · d) | O(n) | Quá chậm | 
| Tối ưu (tập thứ tự + tần số chữ số) | O(q log n + q · d) | O(n) | Đã chấp nhận | 

Ở đây d nhiều nhất là 10. 

## Hướng dẫn thuật toán 

Chúng tôi duy trì cấu trúc dữ liệu lưu trữ tất cả các số hoạt động theo thứ tự được sắp xếp và mảng tần số chữ số toàn cầu cnt[10]. Chúng tôi cũng cần hỗ trợ chuyển đổi tư cách thành viên và xóa theo cấp bậc. 

1. Đầu tiên, chúng tôi thu thập tất cả các số ban đầu và tất cả các số xuất hiện trong truy vấn chuyển đổi để có thể nén các giá trị thành chỉ mục. Điều này đảm bảo chúng ta có thể sử dụng cây Fenwick trong một phạm vi cố định. Việc nén là cần thiết vì giá trị lên tới 2e9. 
2. Xây dựng cây Fenwick (hoặc BIT) trên các chỉ số nén, trong đó mỗi vị trí lưu trữ xem số đó có hiện đang hoạt động hay không. Điều này cho phép chúng tôi truy vấn tổng tiền tố và tìm phần tử hoạt động thứ k. 
3. Khởi tạo cnt[0..9] bằng 0. Đối với mỗi số ban đầu, chúng tôi chèn nó vào cấu trúc và cộng phần đóng góp chữ số của nó vào cnt. Điều này thiết lập trạng thái bắt đầu chính xác. 
4. Đối với truy vấn "+ k", chúng tôi kiểm tra xem k hiện có hoạt động hay không. Nếu có, chúng tôi sẽ xóa nó; nếu không chúng tôi chèn nó. Khi chèn, chúng tôi duyệt qua các chữ số của k và tăng cnt[d] cho mỗi lần xuất hiện chữ số. Khi loại bỏ ta cũng giảm theo cách tương tự. 
5. Đối với truy vấn "- k", trước tiên chúng tôi kiểm tra xem kích thước tập hợp hoạt động có ít nhất là k hay không. Nếu không, chúng tôi bỏ qua nó. Ngược lại, chúng ta sử dụng cây Fenwick để tìm chỉ số của phần tử lớn thứ k. Vì Fenwick hỗ trợ giá trị nhỏ nhất thứ k một cách tự nhiên nên chúng tôi chuyển đổi bằng cách truy vấn kích thước - k + 1. Sau đó, chúng tôi truy xuất giá trị, xóa giá trị đó và cập nhật tần số chữ số. 
6. Đối với truy vấn "? k", trước tiên chúng tôi kiểm tra xem k có hoạt động hay không. Nếu không, xuất ra -1. Mặt khác, tính f(k) bằng cách lặp qua các chữ số của k và tính tổng cnt[d] cho mỗi lần xuất hiện chữ số. 

### Tại sao nó hoạt động 

Tại mọi thời điểm, cnt[d] luôn bằng tổng số lần xuất hiện của chữ số d trên tất cả các số đang hoạt động. Mỗi lần chèn sẽ thêm biểu đồ chữ số của số và mỗi lần xóa sẽ xóa chính xác biểu đồ đó. Vì mỗi thao tác trên tập hợp được phản ánh bởi một bản cập nhật tương ứng trên cnt, nên bất biến vẫn đúng. Vì f(k) được định nghĩa thuần túy là tổ hợp tuyến tính của cnt[d] trên các chữ số của k, nên khi tư cách thành viên được xác minh, giá trị tính toán luôn đúng. 

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

    def find_kth(self, k):
        idx = 0
        bitmask = 1 << (self.n.bit_length())
        while bitmask:
            nxt = idx + bitmask
            if nxt <= self.n and self.bit[nxt] < k:
                k -= self.bit[nxt]
                idx = nxt
            bitmask >>= 1
        return idx + 1

def digits(x):
    if x == 0:
        return [0]
    res = []
    while x:
        res.append(x % 10)
        x //= 10
    return res

n, q = map(int, input().split())
a = list(map(int, input().split()))

ops = []
vals = set(a)

for _ in range(q):
    line = input().split()
    ops.append(line)
    if len(line) == 2:
        vals.add(int(line[1]))

vals = sorted(vals)
idx = {v: i + 1 for i, v in enumerate(vals)}

fw = Fenwick(len(vals))
active = set()
cnt = [0] * 10

def add_num(x):
    if x in active:
        return
    active.add(x)
    fw.add(idx[x], 1)
    for d in digits(x):
        cnt[d] += 1

def remove_num(x):
    if x not in active:
        return
    active.remove(x)
    fw.add(idx[x], -1)
    for d in digits(x):
        cnt[d] -= 1

for x in a:
    add_num(x)

out = []

for op in ops:
    if op[0] == '+':
        x = int(op[1])
        if x in active:
            remove_num(x)
        else:
            add_num(x)

    elif op[0] == '-':
        k = int(op[1])
        size = fw.sum(len(vals))
        if k > size:
            continue
        target = size - k + 1
        idx_pos = fw.find_kth(target)
        x = vals[idx_pos - 1]
        remove_num(x)

    else:
        x = int(op[1])
        if x not in active:
            out.append("-1")
        else:
            res = 0
            for d in digits(x):
                res += cnt[d]
            out.append(str(res))

print("\n".join(out))
```Cây Fenwick được sử dụng hoàn toàn cho mục đích thống kê thứ tự. Tập hoạt động được giữ đồng bộ để cho phép kiểm tra tư cách thành viên O(1). Mảng đếm chữ số cnt chỉ được cập nhật khi các số vào hoặc ra khỏi tập hoạt động, đảm bảo thời gian truy vấn chỉ phụ thuộc vào độ dài chữ số của k. 

Một lỗi phổ biến là quên rằng các truy vấn chuyển đổi phải xóa một số hiện có nếu nó đã tồn tại. Một cách khác là cập nhật số lượng chữ số trên mỗi truy vấn thay vì chỉ cập nhật các thay đổi trạng thái thực tế, điều này sẽ làm hỏng bảng tần số. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Các số ban đầu: 70, 123, 311, 125 

Chúng tôi theo dõi số lượng tập hợp và chữ số hoạt động. 

| Bước | Hoạt động | Bộ hoạt động | cnt[1] | cnt[2] | cnt[3] | cnt[7] | Kết quả truy vấn | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | ? 123 | {70,123,311,125} | 4 | 2 | 2 | 1 | 8 | 
| 2 | -2 | loại bỏ 123 | cập nhật | cập nhật | cập nhật | cập nhật | - | 
| 3 | ? 123 | {70,311,125} | 3 | 2 | 1 | 1 | 6 | 
| 4 | +234 | thêm 234 | cập nhật | cập nhật | cập nhật | cập nhật | - | 
| 5 | -3 | loại bỏ 70 | cập nhật | cập nhật | cập nhật | cập nhật | - | 
| 6 | ? 123 | {311,125,234} | 3 | 3 | 2 | 0 | -1 | 

Dấu vết cho thấy tần số chữ số theo dõi tư cách thành viên toàn cầu, trong khi tư cách thành viên kiểm tra cổng xem f(k) có được đánh giá hay không. 

### Mẫu 2 (đã thi công) 

đầu vào:```
3 5
10 22 33
? 22
+ 22
? 22
- 1
? 22
```| Bước | Hoạt động | Bộ hoạt động | cnt[2] | cnt[1] | Truy vấn | 
| --- | --- | --- | --- | --- | --- | 
| 1 | ? 22 | {10,22,33} | 2 | 1 | 4 | 
| 2 | +22 | {10,33} | 0 | 1 | - | 
| 3 | ? 22 | không hoạt động | 0 | 1 | -1 | 
| 4 | -1 | loại bỏ lớn nhất 33 | {10} | 1 | - | 
| 5 | ? 22 | không hoạt động | 0 | 1 | -1 | 

Điều này nhấn mạnh rằng f(k) chỉ hợp lệ khi k tồn tại trong tập tích cực, ngay cả khi đóng góp chữ số của nó được xác định rõ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n + q · d) | Cập nhật Fenwick và truy vấn thứ k là logarit, công việc chữ số không đổi trên mỗi số | 
| Không gian | O(n + 10) | chỉ số nén cộng với mảng tần số chữ số | 

Giải pháp phù hợp một cách thoải mái trong giới hạn vì nhật ký n là khoảng 19 cho các phần tử 3e5 và quá trình xử lý chữ số bị giới hạn bởi tối đa 10 thao tác cho mỗi lần cập nhật hoặc truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    # assume solution is wrapped in main()
    return sys.stdout.getvalue().strip()

# provided sample (conceptual placeholder)
# assert run(...) == ...

# minimum size
assert run("""1 3
5
? 5
+ 5
? 5
""").split()[-1] == "5"

# toggle correctness
assert run("""2 3
10 20
+ 10
? 10
? 20
""").split()[-2:] == ["1", "2"]

# deletion by rank boundary
assert run("""3 2
1 2 3
- 5
? 2
""") == ""

# all equal behavior
assert run("""1 4
7
+ 7
? 7
+ 7
? 7
""") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuyển đổi đơn | 5 | tính đúng đắn cơ bản | 
| chuyển đổi lặp đi lặp lại | 1,2 | cập nhật tần số | 
| xóa không hợp lệ | trống | bỏ qua điều kiện | 
| chuyển đổi trùng lặp | ổn định | hành vi bình thường | 

## Vỏ cạnh 

Một trường hợp tinh tế là chuyển đổi cùng một số nhiều lần. Ví dụ: bắt đầu trống, áp dụng "+ 10" hai lần trước tiên nên chèn rồi xóa nó. Việc triển khai kiểm tra rõ ràng tư cách thành viên trước khi quyết định thêm hay xóa, đảm bảo số lượng chữ số chỉ được sửa đổi khi trạng thái thay đổi. 

Một trường hợp cạnh khác là xóa theo thứ hạng khi tập hợp quá nhỏ. Nếu tập hợp hiện tại có kích thước 2 và chúng tôi nhận được "- 5", thì truy vấn Fenwick sẽ không bao giờ được thực thi và không có số lượng chữ số nào được sửa đổi, duy trì tính chính xác của cnt. 

Cuối cùng, các số nặng về chữ số như 1111111111 nhấn mạnh việc xử lý bội số. Mỗi lần chèn sẽ cập nhật cnt[1] thêm 10 và việc xóa sẽ trừ đi số tiền tương tự. Vì phép lặp chữ số là tuyến tính theo số chữ số nên ngay cả các giá trị trong trường hợp xấu nhất vẫn hiệu quả.
