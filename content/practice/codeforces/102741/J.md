---
title: "CF 102741J - Giải đấu thể thao điện tử"
description: "Hệ thống duy trì mạng xã hội của người dùng. Kết nối tình bạn sẽ kết nối hai người dùng vào cùng một nhóm bạn, trong đó các nhóm là các thành phần được kết nối của biểu đồ. Đối với mỗi truy vấn về giải đấu, chúng tôi được hỏi có thể thành lập bao nhiêu đội có quy mô chính xác."
date: "2026-07-29T00:49:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102741
codeforces_index: "J"
codeforces_contest_name: "UTPC Contest 9-25-20 Div. 1"
rating: 0
weight: 102741
solve_time_s: 57
verified: true
draft: false
---

[CF 102741J - Giải đấu thể thao điện tử](https://codeforces.com/problemset/problem/102741/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Hệ thống duy trì mạng xã hội của người dùng. Kết nối tình bạn sẽ kết nối hai người dùng vào cùng một nhóm bạn, trong đó các nhóm là các thành phần được kết nối của biểu đồ. Đối với mỗi truy vấn về giải đấu, chúng tôi được hỏi có bao nhiêu đội có quy mô chính xác`s`có thể được hình thành. Một nhóm phải hoàn toàn đến từ một nhóm bạn và một nhóm có thể đóng góp nhiều nhóm miễn là họ không chia sẻ người dùng. Do đó, câu trả lời là tổng của`floor(size_of_group / s)`trên tất cả các nhóm bạn hiện tại. 

Đầu vào chứa tối đa`10^5`người dùng và`10^5`hoạt động. Các phép toán hữu nghị chỉ thêm các cạnh, do đó các thành phần được kết nối có thể được duy trì bằng cấu trúc hợp tập hợp rời rạc. Tuy nhiên, một truy vấn có thể yêu cầu bất kỳ quy mô nhóm nào từ`1`ĐẾN`n`, có nghĩa là không thể tính toán lại tất cả các thành phần cho mọi truy vấn. Với`10^5`hoạt động, thậm chí là một`O(n)`câu trả lời cho mỗi truy vấn sẽ tiếp cận`10^10`hoạt động và không thể phù hợp với giới hạn cuộc thi điển hình. 

Phần khó khăn là không duy trì được các thành phần được kết nối. Phần khó khăn là giữ các giá trị`sum floor(component_size / s)`cho mọi thứ có thể`s`trong khi kích thước thành phần thay đổi. 

Một lỗi phổ biến là cập nhật mọi quy mô nhóm có thể có sau khi hợp nhất. Ví dụ: nếu một thành phần tăng từ kích thước`1`để kích thước`50000`, trực tiếp thay đổi mọi`s`lên đến`50000`hoạt động cho một lần hợp nhất, nhưng hàng nghìn lần hợp nhất sẽ khiến giải pháp trở nên quá chậm. 

Một trường hợp khác là kích thước xử lý`1`thành phần. Ban đầu mỗi người dùng đều ở một mình, do đó, truy vấn về quy mô nhóm`1`phải quay lại`n`, trong khi truy vấn có kích thước lớn hơn phải trả về`0`. 

Ví dụ:```
5 3
2 1
2 2
1 1 2
```Truy vấn đầu tiên yêu cầu các nhóm có quy mô`1`, vậy câu trả lời là`5`. Sau khi người dùng`1`Và`2`trở thành bạn bè, các nhóm có quy mô`2,1,1,1`, vậy một kích thước`2`truy vấn nhóm trả về`1`, không`2`, vì nhóm hai người dùng chỉ có thể cung cấp một nhóm hoàn chỉnh. 

Một trường hợp khác là hợp nhất những người dùng đã được kết nối. Biểu đồ tình bạn không thay đổi khi hai người dùng trong cùng một nhóm lại trở thành bạn bè. Việc triển khai bất cẩn có thể loại bỏ thành phần hai lần và làm hỏng số lượng được lưu trữ. 

Ví dụ:```
3 3
1 1 2
1 2 1
2 2
```Kích thước thành phần cuối cùng là`2,1`, vậy câu trả lời là`1`. Hoạt động kết hợp thứ hai phải được bỏ qua. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản sử dụng tập hợp rời rạc để duy trì các thành phần. Đối với mọi truy vấn thuộc loại`2`, chúng tôi có thể lặp lại tất cả các kích thước thành phần hiện tại và tính toán xem mỗi thành phần đóng góp bao nhiêu nhóm. Điều này đúng vì mỗi nhóm độc lập đóng góp`size // s`các đội. 

Vấn đề là chi phí truy vấn. có thể có`10^5`các truy vấn và cũng có thể có`10^5`thành phần. Trong trường hợp xấu nhất, phương pháp này thực hiện xung quanh`10^10`hoạt động quá chậm. 

Quan sát quan trọng là một thành phần có kích thước`x`đóng góp`floor(x / s)`cho câu trả lời của mọi quy mô nhóm`s`. Chúng tôi không cần cập nhật tất cả`s`riêng lẻ. Giá trị của`floor(x / s)`chỉ thay đổi về`2 * sqrt(x)`lần. Ví dụ, tất cả`s`các giá trị trong một khoảng nhất định có thể tạo ra cùng một thương số. 

Điều này cho phép chúng ta biểu diễn tác động của việc thêm hoặc bớt một thành phần dưới dạng một số lượng nhỏ các phép cộng phạm vi. Vì các truy vấn yêu cầu một thông tin cụ thể`s`, cây Fenwick trên một mảng sai phân có thể hỗ trợ phép cộng phạm vi và truy vấn điểm. 

Khi hai thành phần có kích thước`a`Và`b`hợp nhất thành kích thước`a+b`, chúng tôi loại bỏ sự đóng góp của`a`Và`b`và thêm sự đóng góp của`a+b`. DSU xử lý kết nối, trong khi cây Fenwick duy trì các câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(nq)`|`O(n)`| Quá chậm | 
| Tối ưu |`O((n+q) * sqrt(n) * log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo mỗi người dùng dưới dạng thành phần DSU của chính nó. Một người dùng duy nhất đóng góp`floor(1/s)`các nhóm, vì vậy đóng góp ban đầu có thể được thêm vào bằng cách sử dụng cùng một quy trình cập nhật thành phần. 
2. Duy trì một cây Fenwick để lưu trữ các phạm vi bổ sung. Giá trị tại vị trí`s`đại diện cho số lượng đội có quy mô hợp lệ hiện tại`s`. 
3. Khi thêm một thành phần có kích thước`x`, tìm mọi khoảng quy mô nhóm trong đó`floor(x/s)`có cùng giá trị. Với mỗi khoảng như vậy`[l,r]`, thêm thương số đó vào cây Fenwick trong phạm vi đó. 
4. Khi tháo một thành phần, thực hiện quá trình tương tự với dấu ngược lại. Điều này giúp mọi câu trả lời về quy mô nhóm có thể được đồng bộ hóa với quy mô thành phần hiện tại. 
5. Đối với hoạt động tình bạn, hãy tìm hai gốc DSU. Nếu chúng đã bằng nhau thì không có gì thay đổi. Nếu không thì loại bỏ cả hai đóng góp thành phần cũ, hợp nhất các bộ DSU và thêm đóng góp của kích thước thành phần mới. 
6. Đối với truy vấn giải đấu có quy mô đội`s`, hãy hỏi cây Fenwick về giá trị tại vị trí`s`. 

Lý do các khoảng thương số nhỏ là vì`floor(x/s)`lúc đầu giảm chậm sau đó thay đổi nhanh ở gần cuối. Số lượng các giá trị riêng biệt được giới hạn bởi`O(sqrt(x))`, vì vậy mỗi lần cập nhật thành phần đều hiệu quả. 

Tại sao nó hoạt động: 

Bất biến được duy trì là giá trị của cây Fenwick ở mọi vị trí`s`bằng tổng của`floor(size/s)`trên tất cả các thành phần được kết nối hiện tại. Ban đầu điều này đúng vì mọi thành phần đều được chèn vào. Việc hợp nhất sẽ loại bỏ chính xác hai thành phần cũ và chèn chính xác thành phần mới, do đó, bất biến vẫn đúng sau mỗi thao tác kết bạn. Một truy vấn chỉ cần đọc giá trị được duy trì cho quy mô nhóm được yêu cầu, chính xác là số lượng nhóm được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 2)

    def add(self, idx, val):
        while idx <= self.n:
            self.bit[idx] += val
            idx += idx & -idx

    def range_add(self, l, r, val):
        if l > r:
            return
        self.add(l, val)
        self.add(r + 1, -val)

    def query(self, idx):
        res = 0
        while idx:
            res += self.bit[idx]
            idx -= idx & -idx
        return res

def solve():
    n, q = map(int, input().split())

    parent = list(range(n + 1))
    size = [1] * (n + 1)

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    fw = Fenwick(n)

    def update_component(x, delta):
        l = 1
        while l <= x:
            val = x // l
            r = x // val
            fw.range_add(l, r, val * delta)
            l = r + 1

    update_component(1, n)

    ans = []

    for _ in range(q):
        query = list(map(int, input().split()))
        if query[0] == 1:
            a, b = query[1], query[2]
            ra, rb = find(a), find(b)
            if ra != rb:
                update_component(size[ra], -1)
                update_component(size[rb], -1)
                if size[ra] < size[rb]:
                    ra, rb = rb, ra
                parent[rb] = ra
                size[ra] += size[rb]
                update_component(size[ra], 1)
        else:
            ans.append(str(fw.query(query[1])))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Cây Fenwick lưu trữ một mảng khác biệt thay vì trực tiếp các câu trả lời. Bản cập nhật phạm vi sẽ thêm ở ranh giới bên trái và trừ đi sau ranh giới bên phải, đồng thời tổng tiền tố sẽ tái tạo lại câu trả lời hiện tại cho một quy mô nhóm cụ thể. 

các`update_component`chức năng là tối ưu hóa cốt lõi. Nó đi qua những khoảng thời gian nơi`x // s`là không đổi. dòng`r = x // val`nhảy thẳng đến cuối khoảng thời gian hiện tại, tránh lặp lại mọi quy mô nhóm có thể có. 

DSU sử dụng tính năng nén đường dẫn và kết hợp theo kích thước. Thứ tự hợp nhất được xử lý cẩn thận vì kích thước thành phần được lưu trữ thuộc về phần gốc sau khi hợp nhất. Nếu cả hai người dùng đều có cùng một gốc thì không có thay đổi đóng góp nào được thực hiện. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
5 6
2 2
1 1 2
2 2
2 3
1 1 3
2 3
```| Bước | Hoạt động | Kích thước thành phần | Câu trả lời truy vấn | 
| --- | --- | --- | --- | 
| 0 | Trạng thái ban đầu |`1,1,1,1,1`| | 
| 1 | Hỏi kích thước`2`|`1,1,1,1,1`|`0`| 
| 2 | Hợp nhất`1,2`|`2,1,1,1`| | 
| 3 | Hỏi kích thước`2`|`2,1,1,1`|`1`| 
| 4 | Hỏi kích thước`3`|`2,1,1,1`|`0`| 
| 5 | Hợp nhất`1,3`|`3,1,1`| | 
| 6 | Hỏi kích thước`3`|`3,1,1`|`1`| 

Dấu vết này cho thấy chỉ có các đội hoàn thành mới được tính. Thành phần kích thước`3`có thể cung cấp một đội có quy mô`3`, trong khi các thành phần nhỏ hơn không đóng góp gì. 

Đối với mẫu thứ hai:```
7 9
2 1
2 2
1 1 2
1 2 3
1 4 5
1 5 6
1 6 7
2 3
2 4
```| Bước | Hoạt động | Kích thước thành phần | Câu trả lời truy vấn | 
| --- | --- | --- | --- | 
| 0 | Trạng thái ban đầu |`1,1,1,1,1,1,1`| | 
| 1 | Hỏi kích thước`1`|`1,1,1,1,1,1,1`|`7`| 
| 2 | Hỏi kích thước`2`|`1,1,1,1,1,1,1`|`0`| 
| 3 | Hợp nhất`1,2`|`2,1,1,1,1,1`| | 
| 4 | Hợp nhất`2,3`|`3,1,1,1,1`| | 
| 5 | Hợp nhất`4,5`|`3,2,1,1`| | 
| 6 | Hợp nhất`5,6`|`3,3,1`| | 
| 7 | Hợp nhất`6,7`|`3,4`| | 
| 8 | Hỏi kích thước`3`|`3,4`|`2`| 
| 9 | Hỏi kích thước`4`|`3,4`|`1`| 

Dấu vết xác nhận rằng câu trả lời chỉ phụ thuộc vào kích thước thành phần chứ không phụ thuộc vào sự sắp xếp nội bộ của tình bạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O((n+q) sqrt(n) log n)`| Mỗi lần hợp nhất thay đổi ba thành phần và mỗi bản cập nhật thành phần sử dụng`O(sqrt(n))`khoảng thương với phép toán Fenwick. | 
| Không gian |`O(n)`| Mảng DSU và cây Fenwick lưu trữ dữ liệu có kích thước tuyến tính. | 

Các ràng buộc cho phép khoảng`10^5`hoạt động. Việc phân rã thương số được tối ưu hóa sẽ tránh được chi phí cập nhật tuyến tính khiến mỗi lần hợp nhất trở nên quá tốn kém, giữ cho tổng công việc trong giới hạn yêu cầu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = []
    n, q = map(int, input().split())

    parent = list(range(n + 1))
    size = [1] * (n + 1)

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 2)
        def add(self, i, v):
            while i <= self.n:
                self.bit[i] += v
                i += i & -i
        def ra(self, l, r, v):
            self.add(l, v)
            self.add(r + 1, -v)
        def get(self, i):
            s = 0
            while i:
                s += self.bit[i]
                i -= i & -i
            return s

    fw = Fenwick(n)

    def add_comp(x, d):
        l = 1
        while l <= x:
            v = x // l
            r = x // v
            fw.ra(l, r, v * d)
            l = r + 1

    def find(x):
        if parent[x] != x:
            parent[x] = find(parent[x])
        return parent[x]

    add_comp(1, n)

    for _ in range(q):
        a = list(map(int, input().split()))
        if a[0] == 1:
            x, y = find(a[1]), find(a[2])
            if x != y:
                add_comp(size[x], -1)
                add_comp(size[y], -1)
                parent[y] = x
                size[x] += size[y]
                add_comp(size[x], 1)
        else:
            out.append(str(fw.get(a[1])))

    sys.stdin = old
    return "\n".join(out)

assert run("""5 6
2 2
1 1 2
2 2
2 3
1 1 3
2 3
""") == "0\n1\n0\n1"

assert run("""3 3
1 1 2
1 2 1
2 2
""") == "1"

assert run("""1 3
2 1
2 2
2 1
""") == "1\n0\n1"

assert run("""6 5
1 1 2
1 2 3
1 4 5
1 5 6
2 3
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Truy vấn người dùng duy nhất |`1,0,1`| Quy mô tối thiểu và quy mô nhóm lớn hơn dân số | 
| Tình bạn lặp đi lặp lại |`1`| Bỏ qua các công đoàn bên trong một thành phần | 
| Hai thành phần kích thước`3`Và`3`|`2`| Nhiều nhóm đóng góp đội | 

## Vỏ cạnh 

Khi mỗi người dùng ở một mình, thuật toán sẽ chèn`n`bản sao có kích thước`1`thành phần. Quy trình cập nhật xử lý việc này bằng cách thêm`1`chỉ theo quy mô nhóm`1`, do đó quy mô nhóm lớn hơn vẫn bằng không. 

Đối với tình bạn lặp đi lặp lại, DSU nhận thấy rằng cả hai người dùng đều có cùng một gốc. Vì kích thước thành phần không thay đổi nên cây Fenwick không bị ảnh hưởng và các câu trả lời được lưu trữ vẫn hợp lệ. 

Đối với một thành phần lớn, chẳng hạn như một thành phần có kích thước`100000`, thuật toán không lặp lại tất cả các quy mô nhóm có thể có. Thay vào đó nó nhảy giữa các phạm vi thương số bằng nhau. Điều này giúp chi phí cập nhật ở mức thấp ngay cả khi một nhóm bạn có chứa mọi người dùng.
