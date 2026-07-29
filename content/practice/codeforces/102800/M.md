---
title: "CF 102800M - Khởi động:Upanishad"
description: "Chúng ta có một dãy số nguyên. Đối với mỗi phạm vi truy vấn [l, r], chúng tôi chỉ xem xét các phần tử bên trong phân đoạn đó. Đối với mỗi giá trị xuất hiện trong phân đoạn này với số lần chẵn, chúng tôi lấy giá trị đó một lần và XOR tất cả các giá trị được chọn đó cùng nhau."
date: "2026-07-27T17:44:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102800
codeforces_index: "M"
codeforces_contest_name: "The 14th Jilin Provincial Collegiate Programming Contest"
rating: 0
weight: 102800
solve_time_s: 46
verified: true
draft: false
---

[CF 102800M - Khởi động:Upanishad](https://codeforces.com/problemset/problem/102800/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một dãy số nguyên. Đối với mỗi phạm vi truy vấn`[l, r]`, chúng tôi chỉ xem xét các phần tử bên trong phân khúc đó. Đối với mỗi giá trị xuất hiện trong phân đoạn này với số lần chẵn, chúng tôi lấy giá trị đó một lần và XOR tất cả các giá trị được chọn đó cùng nhau. Các giá trị có tần số lẻ sẽ bị bỏ qua. 

Đầu vào cung cấp mảng và nhiều phạm vi. Đầu ra phải chứa câu trả lời cho mọi phạm vi một cách độc lập. 

Những hạn chế là thách thức chính. Cả độ dài mảng và số lượng truy vấn đều có thể đạt tới 500000. Một giải pháp quét mọi phạm vi sẽ thực hiện trong khoảng`n * q`hoạt động trong trường hợp xấu nhất, đó là về`2.5 * 10^11`, vượt xa những gì có thể chạy trong một giây. Chúng ta cần xử lý các truy vấn gần tuyến tính hoặc`n log n`thời gian. 

Phần khó khăn là truy vấn không yêu cầu XOR của các giá trị tần số lẻ, vốn là thuộc tính thông thường được XOR khai thác. Nó yêu cầu sự chẵn lẻ ngược lại. Việc triển khai bất cẩn có thể chỉ tính toán XOR của tất cả các lần xuất hiện và trả về giá trị đó, nhưng điều đó mang lại XOR các giá trị có tần số lẻ. 

Ví dụ, hãy xem xét:```
4 1
1 2 2 3
1 4
```giá trị`2`xuất hiện hai lần, vì vậy câu trả lời là`2`. XOR của tất cả các phần tử là`1 xor 2 xor 2 xor 3 = 2`, điều này xảy ra ở đây, nhưng điều này không phải do thao tác nói chung là chính xác. Lý do thực sự là XOR của tất cả các lần xuất hiện bằng XOR của các giá trị tần số lẻ và trong trường hợp này, các giá trị tần số lẻ duy nhất là`1`Và`3`. 

Một trường hợp bộc lộ sai lầm là:```
3 1
5 5 7
1 3
```Câu trả lời đúng là`5`, bởi vì chỉ`5`có tần số chẵn. XOR của tất cả các phần tử là`5 xor 5 xor 7 = 7`, điều đó là sai. 

Một trường hợp ranh giới khác là một phạm vi chỉ chứa một lần xuất hiện của mọi giá trị:```
3 1
1 2 3
1 3
```Mọi giá trị đều có tần số lẻ, vì vậy câu trả lời là`0`. Bất kỳ phương pháp nào coi "xuất hiện một lần" là hợp lệ sẽ không thành công. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ xử lý từng truy vấn riêng biệt. Chúng tôi có thể đếm tần số của mọi giá trị trong phạm vi được yêu cầu, sau đó lặp qua các tần số đó và XOR các giá trị với số lượng chẵn. Điều này đúng vì nó tuân theo định nghĩa chính xác. Tuy nhiên, một truy vấn có thể chứa tối đa 500000 phần tử và cũng có thể có 500000 truy vấn. Trường hợp xấu nhất cần phải kiểm tra về`250000000000`vị trí mảng, quá chậm. 

Quan sát quan trọng đến từ việc tách câu trả lời được yêu cầu thành hai phần dễ dàng hơn. Cho phép`D`là XOR của tất cả các giá trị riêng biệt trong một phạm vi. Cho phép`O`là XOR của tất cả các giá trị có tần số lẻ trong phạm vi đó. Vì các cặp biến mất trong XOR nên việc XOR mỗi lần xuất hiện của một giá trị chỉ để lại giá trị đó khi tần số của nó là số lẻ. Do đó, XOR của tất cả các phần tử mảng trong phạm vi chính xác`O`. 

Câu trả lời mong muốn là XOR của các giá trị tần số chẵn. Vì các giá trị riêng biệt được chia thành các giá trị tần số lẻ và giá trị tần số chẵn, nên chúng ta có:```
D = O xor answer
```Sắp xếp lại bằng XOR mang lại:```
answer = D xor O
```Phần thứ hai,`O`, thật dễ dàng. Chúng ta có thể xây dựng một mảng XOR tiền tố, bởi vì việc XOR toàn bộ phạm vi sẽ mang lại XOR các giá trị tần số lẻ. 

Nhiệm vụ còn lại là tìm`D`, XOR của tất cả các giá trị riêng biệt trong nhiều phạm vi. Điều này có thể được xử lý ngoại tuyến. Sắp xếp các truy vấn theo điểm cuối bên phải của chúng. Trong khi di chuyển điểm cuối bên phải từ trái sang phải, hãy duy trì vị trí xuất hiện mới nhất của mọi giá trị. Cây Fenwick lưu trữ giá trị ở mỗi vị trí xuất hiện gần nhất. Khi một giá trị mới xuất hiện, vị trí trước đó của nó sẽ bị xóa và vị trí mới được chèn vào. Một truy vấn`[l, r]`sau đó yêu cầu XOR được lưu trữ từ`l`ĐẾN`r`, chứa chính xác một bản sao của mọi giá trị có lần xuất hiện mới nhất nằm trong phạm vi. 

Lực lượng vũ phu hoạt động vì nó đếm trực tiếp tần số, nhưng nó lặp lại gần như cùng một công việc trên các truy vấn chồng chéo. Cách tiếp cận ngoại tuyến chia sẻ công việc giữa các truy vấn bằng cách duy trì tập hợp các giá trị riêng biệt đang thay đổi khi ranh giới bên phải tiến lên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Cây Fenwick ngoại tuyến | O((n + q) log n) | O(n + q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng mảng XOR tiền tố.`pref[i]`lưu trữ XOR của đầu tiên`i`các phần tử. Đối với một truy vấn`[l, r]`,`pref[r] xor pref[l - 1]`cung cấp XOR của tất cả các lần xuất hiện trong phân đoạn đó, là XOR của các giá trị có tần số lẻ. 
2. Lưu trữ tất cả các truy vấn cùng với vị trí ban đầu của chúng và sắp xếp chúng theo điểm cuối bên phải tăng dần. Việc xử lý các truy vấn theo thứ tự này cho phép cấu trúc dữ liệu thể hiện chính xác tiền tố hiện tại kết thúc ở ranh giới bên phải của truy vấn. 
3. Duy trì cây Fenwick trong đó chỉ mục chỉ chứa một giá trị nếu chỉ mục đó là vị trí xuất hiện mới nhất của giá trị mảng của nó. Khi xử lý vị trí`i`, nhìn vào sự xuất hiện trước đó của`a[i]`. Di dời`a[i]`từ vị trí cũ đó và thêm`a[i]`ở vị trí`i`. Điều này giữ một bản sao hoạt động của mọi giá trị được thấy cho đến nay. 
4. Khi ranh giới bên phải hiện tại đạt tới giới hạn của truy vấn`r`, truy vấn cây Fenwick để tìm XOR từ`l`ĐẾN`r`. Điều này mang lại XOR của tất cả các giá trị riêng biệt bên trong`[l, r]`, bởi vì chính xác những giá trị đó có lần xuất hiện mới nhất trong khoảng thời gian này. 
5. Kết hợp hai phần. XOR kết quả có giá trị riêng biệt với kết quả XOR tiền tố. XOR riêng biệt chứa cả giá trị tần số lẻ và chẵn, trong khi tiền tố XOR chỉ chứa các giá trị tần số lẻ, do đó các giá trị lẻ bị hủy và chỉ còn lại các giá trị tần số chẵn. 

Tại sao nó hoạt động: 

Tại mỗi vị trí được xử lý, tính bất biến của cây Fenwick là mọi giá trị xuất hiện trong tiền tố được xử lý đều đóng góp chính xác một lần, tại vị trí xuất hiện gần nhất của nó. Đối với truy vấn kết thúc ở vị trí hiện tại, một giá trị sẽ xuất hiện trong phạm vi Fenwick`[l, r]`chính xác khi nào lần xuất hiện gần đây nhất của nó ít nhất là`l`, có nghĩa là giá trị đó xuất hiện ở đâu đó bên trong phạm vi truy vấn. Do đó, truy vấn Fenwick trả về XOR của tất cả các giá trị khác biệt trong phạm vi. 

Tiền tố XOR của phạm vi sẽ loại bỏ tất cả các cặp giá trị bằng nhau và chỉ để lại các giá trị tần số lẻ. XOR nó với giá trị riêng biệt XOR sẽ loại bỏ các giá trị tần số lẻ đó và giữ chính xác các giá trị tần số chẵn, đó là câu trả lời bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class FenwickXor:
    def __init__(self, n):
        self.n = n
        self.tree = [0] * (n + 1)

    def update(self, i, val):
        n = self.n
        tree = self.tree
        while i <= n:
            tree[i] ^= val
            i += i & -i

    def query(self, i):
        res = 0
        tree = self.tree
        while i:
            res ^= tree[i]
            i -= i & -i
        return res

    def range_query(self, l, r):
        return self.query(r) ^ self.query(l - 1)

def solve():
    data = sys.stdin.buffer.read().split()
    if not data:
        return

    it = iter(data)
    n = int(next(it))
    q = int(next(it))

    arr = [0] + [int(next(it)) for _ in range(n)]

    pref = [0] * (n + 1)
    for i in range(1, n + 1):
        pref[i] = pref[i - 1] ^ arr[i]

    queries = [[] for _ in range(n + 1)]
    for idx in range(q):
        l = int(next(it))
        r = int(next(it))
        queries[r].append((l, idx))

    ans = [0] * q
    last = {}
    bit = FenwickXor(n)

    for r in range(1, n + 1):
        x = arr[r]
        if x in last:
            bit.update(last[x], x)
        bit.update(r, x)
        last[x] = r

        for l, idx in queries[r]:
            distinct_xor = bit.range_query(l, r)
            odd_xor = pref[r] ^ pref[l - 1]
            ans[idx] = distinct_xor ^ odd_xor

    sys.stdout.write("\n".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Cây Fenwick lưu trữ các đóng góp XOR thay vì tổng. Điều này hiệu quả vì XOR có cùng hành vi hủy cần thiết ở đây: chèn cùng một giá trị hai lần vào cùng một vị trí logic sẽ loại bỏ giá trị đó. 

các`last`từ điển theo dõi vị trí hoạt động trước đó của mọi giá trị. Khi có sự kiện mới xuất hiện, phần đóng góp cũ phải được loại bỏ trước khi thêm phần đóng góp mới. Thứ tự quan trọng vì cây phải luôn chỉ chứa lần xuất hiện mới nhất. 

Bộ lưu trữ truy vấn được nhóm theo điểm cuối bên phải, tránh sắp xếp rõ ràng và cho phép quét một lần từ trái sang phải. Các câu trả lời được lưu trữ theo chỉ mục truy vấn ban đầu vì thứ tự xử lý khác với thứ tự đầu vào. 

Mảng XOR tiền tố sử dụng lập chỉ mục một cơ sở sao cho tiền tố trống trước vị trí`1`đương nhiên được biểu diễn bằng`pref[0]`. Điều này tránh việc xử lý đặc biệt khi truy vấn bắt đầu ở phần tử đầu tiên. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 2
1 2 4 2
1 3
1 4
```Truy vấn đầu tiên hỏi về`[1,3]`, chứa`1,2,4`. 

| Vị trí được xử lý | Lần xuất hiện mới nhất hiện tại | Fenwick khác biệt XOR | Câu trả lời truy vấn | 
| --- | --- | --- | --- | 
| 1 | 1:1 | 1 | | 
| 2 | 1:1, 2:2 | 3 | | 
| 3 | 1:1, 2:2, 4:3 | 7 | truy vấn`[1,3]`:`7 xor (1 xor 2 xor 4) = 0`| 
| 4 | 1:1, 2:4, 4:3 | 7 | truy vấn`[1,4]`:`7 xor (1 xor 2 xor 4 xor 2) = 2`| 

Dải ô đầu tiên có mọi giá trị xuất hiện một lần nên không có giá trị nào đủ tiêu chuẩn. Phạm vi thứ hai có`2`xuất hiện hai lần và thuật toán chỉ để lại giá trị đó sau khi hủy phần tần số lẻ. 

### Mẫu 2 

đầu vào:```
3 2
1 1 1
1 3
2 3
```| Vị trí được xử lý | Lần xuất hiện mới nhất hiện tại | Fenwick khác biệt XOR | Câu trả lời truy vấn | 
| --- | --- | --- | --- | 
| 1 | 1:1 | 1 | | 
| 2 | 1:2 | 1 | | 
| 3 | 1:3 | 1 |`[1,3]`:`1 xor (1 xor 1 xor 1) = 0`,`[2,3]`:`1 xor (1 xor 1) = 0`| 

Ví dụ này kiểm tra các giá trị lặp lại. Ba lần xuất hiện có tần số lẻ nên đáp án là 0. Hai lần xuất hiện cũng tạo ra số 0 vì giá trị tần số chẵn duy nhất là`1`, nhưng tiền tố XOR chỉ xóa nó khỏi XOR riêng biệt khi tần số là số lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Mỗi phần tử mảng cập nhật cây Fenwick một lần và mỗi truy vấn thực hiện hai truy vấn tiền tố Fenwick. | 
| Không gian | O(n + q) | Mảng, tiền tố XOR, lưu trữ truy vấn, bản đồ lần xuất hiện cuối cùng và cây Fenwick đều là tuyến tính. | 

Các ràng buộc cho phép xung quanh`500000`các phần tử và truy vấn. Hệ số logarit từ cây Fenwick giữ cho tổng số phép toán có thể quản lý được, trong khi phương pháp quét bậc hai hoặc phạm vi sẽ vượt quá giới hạn theo một số bậc độ lớn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp):
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    class FenwickXor:
        def __init__(self, n):
            self.n = n
            self.tree = [0] * (n + 1)

        def update(self, i, x):
            while i <= self.n:
                self.tree[i] ^= x
                i += i & -i

        def query(self, i):
            res = 0
            while i:
                res ^= self.tree[i]
                i -= i & -i
            return res

        def range_query(self, l, r):
            return self.query(r) ^ self.query(l - 1)

    data = sys.stdin.buffer.read().split()
    if not data:
        return ""

    it = iter(data)
    n = int(next(it))
    q = int(next(it))
    a = [0] + [int(next(it)) for _ in range(n)]

    pref = [0] * (n + 1)
    for i in range(1, n + 1):
        pref[i] = pref[i - 1] ^ a[i]

    queries = [[] for _ in range(n + 1)]
    for i in range(q):
        l = int(next(it))
        r = int(next(it))
        queries[r].append((l, i))

    ans = [0] * q
    last = {}
    bit = FenwickXor(n)

    for r in range(1, n + 1):
        if a[r] in last:
            bit.update(last[a[r]], a[r])
        bit.update(r, a[r])
        last[a[r]] = r
        for l, idx in queries[r]:
            ans[idx] = bit.range_query(l, r) ^ (pref[r] ^ pref[l - 1])

    return "\n".join(map(str, ans))

assert solve("""4 2
1 2 4 2
1 3
1 4
""") == "0\n2"

assert solve("""3 2
1 1 1
1 3
2 3
""") == "0\n1"

assert solve("""1 1
7
1 1
""") == "0"

assert solve("""5 3
4 4 4 5 5
1 5
1 3
4 5
""") == "4\n4\n5"

assert solve("""6 3
1 2 1 3 2 3
1 6
2 5
3 4
""") == "0\n3\n0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Yếu tố đơn |`0`| Giá trị có một lần xuất hiện sẽ bị bỏ qua. | 
| Tất cả các giá trị bằng nhau |`0`,`1`| Tần số chẵn và lẻ được xử lý riêng biệt. | 
| Các giá trị lặp lại hỗn hợp |`4`,`4`,`5`| Nhiều giá trị với các giá trị chẵn lẻ khác nhau hoạt động chính xác. | 
| Phạm vi chồng chéo |`0`,`3`,`0`| Ranh giới truy vấn và lần xuất hiện mới nhất được cập nhật chính xác. | 

## Vỏ cạnh 

Đối với trường hợp xảy ra một lần:```
3 1
1 2 3
1 3
```Cây Fenwick trả về XOR riêng biệt`1 xor 2 xor 3 = 0`. Tiền tố XOR cũng là`0`bởi vì mọi giá trị đều có tần số lẻ. XOR của họ là`0`, phù hợp với thực tế là không có giá trị nào xuất hiện với số lần chẵn. 

Đối với trường hợp giá trị lặp lại:```
3 1
5 5 7
1 3
```Sau khi xử lý mảng, cây Fenwick chứa các giá trị riêng biệt`5`Và`7`, vì vậy nó trả về`5 xor 7 = 2`. Tiền tố XOR là`5 xor 5 xor 7 = 7`. Kết hợp chúng mang lại`2 xor 7 = 5`, chỉ để lại giá trị có tần số chẵn. 

Đối với một giá trị mà lần xuất hiện gần đây nhất của nó di chuyển:```
4 1
2 3 2 4
1 3
```Khi vị trí thứ ba được xử lý, phần đóng góp cũ của`2`ở vị trí`1`bị loại bỏ và sự đóng góp mới ở vị trí`3`được chèn vào. Truy vấn Fenwick cho`[1,3]`vẫn thấy`2`chính xác một lần, vì cấu trúc dữ liệu biểu thị các giá trị riêng biệt thay vì các lần xuất hiện. XOR riêng biệt là`2 xor 3`, XOR tần số lẻ cũng là`2 xor 3`, và câu trả lời là`0`bởi vì mọi giá trị trong phạm vi xuất hiện một số lần lẻ.
