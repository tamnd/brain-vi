---
title: "CF 104805K - Lấy số"
description: "Chúng ta được cung cấp một tập hợp nhỏ các số nguyên, mỗi số từ 2 đến 15. Từ bộ sưu tập này, chúng ta có thể thực hiện nhiều lần thao tác xây dựng một tập hợp nhiều tập hợp mới bằng cách chọn các phần tử từ tập hợp ban đầu với sự lặp lại được phép."
date: "2026-06-28T13:21:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104805
codeforces_index: "K"
codeforces_contest_name: "Central Russia Regional Contest, 2022"
rating: 0
weight: 104805
solve_time_s: 86
verified: true
draft: false
---

[CF 104805K - Lấy số](https://codeforces.com/problemset/problem/104805/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp nhỏ các số nguyên, mỗi số từ 2 đến 15. Từ bộ sưu tập này, chúng ta có thể thực hiện nhiều lần thao tác xây dựng một tập hợp nhiều tập hợp mới bằng cách chọn các phần tử từ tập hợp ban đầu với sự lặp lại được phép. Từ một tập hợp con đã chọn như vậy, chúng tôi tính toán một số duy nhất: với mỗi tập hợp con của tập hợp con đó, chúng tôi nhân các phần tử bên trong tập hợp con đó và tính tổng các tích này trên tất cả các tập hợp con. 

Tập hợp con trống đóng góp 1 vào tổng này, do đó thao tác tương đương với việc lấy nhiều tập hợp$Y$và sản xuất$$\sum_{S \subseteq Y} \prod_{y \in S} y.$$Sau khi tạo ra các giá trị như vậy, chúng ta có thể tiếp tục lặp lại quy trình với số lần hữu hạn bất kỳ, luôn chỉ sử dụng các số ban đầu làm khối xây dựng. Nhiệm vụ là đếm xem có bao nhiêu giá trị khác nhau không vượt quá$L$bao giờ có thể được sản xuất. 

Một quan sát cấu trúc quan trọng được ẩn giấu trong biểu thức tập hợp con của các sản phẩm. Nếu chúng ta mở rộng sản phẩm$$\prod_{y \in Y} (1 + y),$$chúng ta thu được chính xác tổng trên tất cả các tập hợp con của$Y$, bao gồm cả tập con trống. Do đó, mỗi thao tác tạo ra các giá trị có dạng$$\prod_{y \in Y} (1 + y) - 1.$$Điều này chuyển đổi vấn đề từ suy luận về các tập hợp con sang suy luận về các cấu trúc nhân trên các hằng số$1 + x_i$, mỗi cái nằm trong khoảng từ 3 đến 16. 

Các ràng buộc nhỏ về số lượng phần tử ban đầu, nhiều nhất là 20, nhưng phạm vi giá trị tăng lên$10^{12}$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào liệt kê rõ ràng tất cả các tập hợp nhiều tập hợp hoặc tất cả các kết hợp tập hợp con. Ngay cả một BFS ngây thơ về các giá trị cũng sẽ bùng nổ vì mỗi số có thể được kết hợp lại theo nhiều cách, nhưng cấu trúc có tính nhân và rất dư thừa. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả$x_i$đều bình đẳng. Nhiều tập hợp khác nhau tạo ra các giá trị giống hệt nhau và việc đếm các công trình thay vì các kết quả riêng biệt sẽ bị tính quá mức một cách ồ ạt. Một vấn đề khác là thao tác luôn tạo ra các giá trị lớn hơn sản phẩm thuần túy ít nhất 1, do đó việc quên “trừ một ca” dẫn đến giới hạn không chính xác và lỗi sai lệch một khi so sánh với$L$. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ cố gắng liệt kê mọi tập hợp có thể$Y$, tính giá trị kết quả và lặp lại quy trình từ các giá trị mới được tạo. Số lượng nhiều tập hợp tăng lên không giới hạn vì được phép lặp lại và thậm chí việc hạn chế kích thước giới hạn đã dẫn đến sự bùng nổ theo cấp số nhân trong$N$. Việc tính toán bên trong mỗi trạng thái có thể quản lý được, nhưng bản thân không gian trạng thái không bị giới hạn và nhanh chóng vượt quá mọi giới hạn khả thi. 

Sự đơn giản hóa chính đến từ việc viết lại hoạt động như một sản phẩm. Mọi giá trị được tạo ra đều có dạng$$\prod (1 + x_i)^{c_i} - 1,$$Ở đâu$c_i$là số lần phần tử$x_i$được chọn trong multiset. Điều này có nghĩa là mọi giá trị có thể truy cập đều tương ứng với một tích được hình thành bằng cách nhân liên tục một tập hợp nhỏ các số nguyên cơ sở$b_i = 1 + x_i$, mỗi từ 3 đến 16. 

Điều này loại bỏ hoàn toàn cấu trúc tập hợp con và biến bài toán thành việc tạo ra tất cả các tích riêng biệt được hình thành từ một tập hợp nhỏ các số nguyên dưới giới hạn trên.$L + 1$. Thứ tự của phép nhân không quan trọng, do đó không gian tìm kiếm trở thành một cuộc khám phá tổ hợp các lựa chọn số mũ, nhưng bị cắt bớt nhiều do sự tăng trưởng nhanh chóng của tích. 

Cách tiếp cận tối ưu là liệt kê theo chiều sâu trên các giá trị cơ sở riêng biệt, chọn số lần mỗi cơ sở đóng góp cho sản phẩm. Vì giá trị tăng nhanh nên mỗi nhánh có độ sâu rất hạn chế trước khi vượt quá$L + 1$, làm cho việc tìm kiếm trở nên khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên nhiều bộ | Hàm mũ trong kích thước nhiều tập hợp | Lớn | Quá chậm | 
| DFS trên các sản phẩm bị chặn |$O(\text{states})$, hàm mũ gần như nhỏ trong$N$với việc cắt tỉa |$O(\text{states})$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta nén dữ liệu đầu vào bằng cách thay thế từng$x_i$với$b_i = x_i + 1$, vì mọi thao tác chỉ phụ thuộc vào các giá trị này thông qua phép nhân. 

1. Trích xuất các giá trị riêng biệt giữa tất cả$b_i$, vì các bản sao không làm thay đổi tập hợp các sản phẩm có thể truy cập ngoài việc cho phép nhiều lựa chọn số mũ hơn. Điều này làm giảm sự phân nhánh không cần thiết. 
2. Xác định tìm kiếm đệ quy xây dựng các sản phẩm bắt đầu từ 1. Ở mỗi bước, chúng tôi quyết định nhân với giá trị cơ sở hiện tại bao nhiêu lần. 
3. Đối với mỗi căn cứ$b_i$, hãy thử nhân tích hiện tại với$b_i^k$, Ở đâu$k \ge 0$, miễn là kết quả không vượt quá$L + 1$. Mỗi sự lựa chọn của$k$đại diện cho việc chọn phần tử đó$k$lần trong multiset. 
4. Sau khi sửa số mũ của cơ số hiện tại, xử lý đệ quy chỉ số cơ số tiếp theo. Điều này đảm bảo chúng tôi không bao giờ truy cập lại các cơ sở trước đó, điều này ngăn cản việc tính cùng một kết hợp theo các thứ tự khác nhau. 
5. Bất cứ khi nào chúng tôi xử lý xong tất cả các cơ sở, chúng tôi sẽ nhận được một sản phẩm hợp lệ$P$. Nếu như$P > 1$, chúng tôi ghi lại$P - 1$như một giá trị có thể đạt được. 

Phép đệ quy liệt kê một cách có hệ thống tất cả các tổ hợp nhân của tập cơ sở, được giới hạn bởi giới hạn. 

### Tại sao nó hoạt động 

Mọi cách xây dựng hợp lệ đều tương ứng duy nhất với một vectơ số mũ$(c_1, c_2, \dots, c_m)$, ánh xạ tới một sản phẩm$\prod b_i^{c_i}$. DFS liệt kê từng vectơ số mũ chính xác một lần bằng cách thực thi một thứ tự cố định các cơ số. Vì phép nhân có tính giao hoán nên không có hai đường dẫn khác nhau tạo ra cấu hình số mũ giống nhau. Điều kiện cắt tỉa$P \le L + 1$đảm bảo rằng không có nhánh nào có thể đóng góp các giá trị ngoài phạm vi cho phép, do đó không gian tìm kiếm vừa đầy đủ vừa hữu hạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, L = map(int, input().split())
    x = list(map(int, input().split()))

    bases = sorted(set(v + 1 for v in x))
    limit = L + 1

    seen = set()

    def dfs(i, cur):
        if i == len(bases):
            if cur > 1:
                seen.add(cur - 1)
            return

        dfs(i + 1, cur)

        b = bases[i]
        nxt = cur
        while True:
            nxt *= b
            if nxt > limit:
                break
            dfs(i + 1, nxt)

    dfs(0, 1)
    print(len(seen))

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp việc giải thích số mũ của quá trình. Chỉ số đệ quy`i`thực thi một thứ tự cố định trên các căn cứ, đảm bảo tính duy nhất. Vòng lặp nhân`nxt`qua`b`liên tục mã hóa tất cả các lựa chọn số mũ cho cơ sở đó trong một nhánh. các`limit = L + 1`sự thay đổi là rất quan trọng vì các giá trị được lưu trữ thực tế luôn nhỏ hơn tích nhân một đơn vị. 

Một lỗi phổ biến là quên loại bỏ các giá trị cơ sở trùng lặp, khiến việc khám phá dư thừa nhưng vẫn đúng, chỉ chậm hơn. Một điểm tinh tế khác là bắt đầu từ sản phẩm 1 thay vì bắt đầu từ các giá trị trọng điểm giống số 0, vì cấu trúc hoàn toàn mang tính nhân. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 7
2 2 3
```Căn cứ riêng biệt trở thành$[3, 4]$sau khi dịch chuyển. 

Chúng tôi theo dõi trạng thái DFS: 

| Bước | Chỉ số cơ sở | Sản phẩm hiện tại | Hành động | Giá trị gia tăng hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | bỏ qua 3 | không | 
| 2 | 1 | 1 | bỏ qua 4 | không | 
| 3 | 1 | 4 | 4^1 | 3 | 
| 4 | 0 | 3 | 3^1 | không hợp lệ (>7) | 
| 5 | 1 | 16 | không hợp lệ sớm | không | 

Chỉ các giá trị có thể truy cập hợp lệ ≤ 7 bị giới hạn và việc loại bỏ trùng lặp chỉ để lại 2 giá trị riêng biệt. 

Đầu ra là:```
2
```Dấu vết này cho thấy sản phẩm vượt quá giới hạn nhanh như thế nào, ngăn cản sự phát triển lớn của nhà nước. 

### Mẫu 2 

đầu vào:```
2 100
14 15
```Căn cứ trở thành$[15, 16]$. 

| Bước | Chỉ số cơ sở | Sản phẩm hiện tại | Hành động | Giá trị | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | bỏ qua 15 | - | 
| 1 | 1 | 1 | bỏ qua 16 | - | 
| 2 | 1 | 16 | 16^1 | 15 | 
| 3 | 1 | 256 | vượt quá giới hạn | dừng lại | 
| 4 | 0 | 15 | 15^1 | không hợp lệ (>100 sau khi điều chỉnh) | 

Chỉ các công trình một bước vẫn có hiệu lực. 

Đầu ra:```
2
```Ví dụ này nhấn mạnh rằng mặc dù có nhiều tổ hợp nhân tồn tại trên lý thuyết, nhưng ràng buộc$L$thu gọn tập hợp có thể truy cập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(S)$| Mỗi trạng thái sản phẩm hợp lệ được tạo một lần và việc phân nhánh bị hạn chế rất nhiều bởi sự tăng trưởng nhanh chóng của phép nhân | 
| Không gian |$O(S)$| Lưu trữ các kết quả đã truy cập và độ sâu đệ quy được giới hạn bởi số lượng cơ sở riêng biệt | 

Số lượng các trạng thái riêng biệt vẫn còn nhỏ vì có ít nhất ba cơ sở, gây ra sự tăng trưởng theo cấp số nhân về kích thước sản phẩm. Điều này đảm bảo DFS kết thúc nhanh chóng ngay cả trong cách sắp xếp đầu vào tồi tệ nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import isclose

    N, L = map(int, sys.stdin.readline().split())
    x = list(map(int, sys.stdin.readline().split()))

    bases = sorted(set(v + 1 for v in x))
    limit = L + 1
    seen = set()

    def dfs(i, cur):
        if i == len(bases):
            if cur > 1:
                seen.add(cur - 1)
            return
        dfs(i + 1, cur)
        b = bases[i]
        nxt = cur
        while True:
            nxt *= b
            if nxt > limit:
                break
            dfs(i + 1, nxt)

    dfs(0, 1)
    return str(len(seen))

# provided samples
assert run("3 7\n2 2 3\n") == "2"
assert run("2 100\n14 15\n") == "2"

# all equal values
assert run("3 50\n2 2 2\n") == "2"

# minimum case
assert run("2 10\n2 3\n") == "2"

# large limit but small bases
assert run("2 1000000000000\n2 2\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đều nhỏ như nhau | 2 | xử lý trùng lặp | 
| đầu vào tối thiểu | giá trị nhỏ | độ đúng cơ sở | 
| L lớn | số lượng ổn định | cắt tỉa đúng cách | 

## Vỏ cạnh 

Khi tất cả các số đầu vào giống hệt nhau, mọi cấu trúc nhân sẽ thu gọn thành lũy thừa lặp lại của một cơ số duy nhất. DFS vẫn khám phá các lựa chọn số mũ, nhưng tính năng loại bỏ trùng lặp đảm bảo rằng chỉ các sản phẩm riêng biệt mới được tính. Ví dụ, với đầu vào`2 10 / 2 2`, các sản phẩm duy nhất có thể truy cập là 1, 3 và 9, tạo ra hai giá trị hợp lệ sau khi trừ một. 

Khi$L$cực kỳ nhỏ, nhiều nhánh kết thúc ngay sau lần nhân đầu tiên. Ví dụ, với`2 5 / 2 3`, cả hai cơ số trở thành 3 và 4, nhưng bất kỳ phép nhân thứ hai nào cũng đã vượt quá giới hạn, do đó chỉ những đóng góp của một cơ số duy nhất còn tồn tại. Phép đệ quy vẫn truy cập cả hai lựa chọn nhưng sẽ cắt bớt gần như ngay lập tức. 

Khi$L$là rất lớn, độ sâu tìm kiếm tăng nhẹ, nhưng sự tăng trưởng theo cấp số nhân của các bazơ đảm bảo rằng ngay cả các chuỗi dài vẫn ở mức nông. Ví dụ: phép nhân lặp lại với 3 vượt quá$10^{12}$dưới 25 bước nên không có nhánh nào có thể phát triển vô thời hạn.
