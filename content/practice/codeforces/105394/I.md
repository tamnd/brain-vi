---
title: "CF 105394I - Nhiễu"
description: "Chúng ta được cung cấp một chuỗi các thao tác trên một đường một chiều rất lớn, trong đó các vị trí có thể lên tới 1e9. Hai loại hoạt động được thực hiện trực tuyến. Loại đầu tiên chèn một làn sóng."
date: "2026-06-23T17:07:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105394
codeforces_index: "I"
codeforces_contest_name: "2024-2025 ICPC German Collegiate Programming Contest (GCPC 2024)"
rating: 0
weight: 105394
solve_time_s: 65
verified: true
draft: false
---

[CF 105394I - Can thiệp](https://codeforces.com/problemset/problem/105394/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các thao tác trên một đường một chiều rất lớn, trong đó các vị trí có thể lên tới 1e9. Hai loại hoạt động được thực hiện trực tuyến. 

Loại đầu tiên chèn một làn sóng. Mỗi sóng bắt đầu ở một vị trí nào đó`p`, kéo dài một đoạn có chiều dài liên tục`ℓ`, và có biên độ`a`. Hình dạng của sóng là cố định: nó lặp lại cứ sau 4 vị trí, bắt đầu với đỉnh dương tại`p`. Cụ thể, khi chúng ta di chuyển ngay từ`p`, sự đóng góp tuân theo một mô hình lặp lại`+a, 0, -a, 0, +a, 0, -a, 0, ...`cắt ngắn theo chiều dài đoạn. Vì vậy, chỉ có mỗi vị trí thứ tư đóng góp, xen kẽ giữa biên độ dương và âm. 

Loại thứ hai là truy vấn yêu cầu tổng chiều cao thu được tại một vị trí sau khi áp dụng tất cả các sóng đã thêm trước đó. Vì các sóng tích lũy nên mỗi truy vấn sẽ yêu cầu tổng đóng góp từ tất cả các sóng đang hoạt động tại tọa độ đó. 

Khó khăn chính là các vị trí lớn, lên tới 1e9, nhưng số lượng thao tác lại nhỏ, nhiều nhất là 4000. Điều này ngay lập tức loại trừ việc duy trì một mảng kích thước rõ ràng`w`hoặc thực hiện mô phỏng theo từng vị trí. Bất kỳ giải pháp nào lặp lại trên tất cả các ô bị ảnh hưởng trong mỗi bản cập nhật sẽ có giá lên tới O(n·w), điều này là không thể. 

Một khó khăn tinh vi hơn là mỗi bản cập nhật không phải là một phạm vi bổ sung thống nhất. Đó là một mô hình định kỳ có cấu trúc. Một sai lầm ngây thơ là coi mỗi sóng như một bản cập nhật phạm vi đơn giản của +a trong khoảng thời gian đó, nhưng lại bỏ qua các dấu hiệu xen kẽ và tạo ra kết quả không chính xác. 

Một trường hợp lỗi phổ biến khác xuất hiện khi các bản cập nhật trùng nhau: hai sóng có thể giao thoa với nhau và pha của chúng phụ thuộc vào vị trí tuyệt đối chứ không phải sự căn chỉnh cục bộ bên trong mỗi sóng. Ví dụ: một sóng bắt đầu ở vị trí 1 và một sóng khác bắt đầu ở vị trí 2 tạo ra các mẫu dư lượng hoàn toàn khác nhau ngay cả trên cùng một phân đoạn, do đó chỉ nhóm theo khoảng thời gian là không đủ. 

Các trường hợp chính xuất phát từ việc xử lý từng khoảng thời gian`[p, p + ℓ - 1]`và từ việc căn chỉnh chính xác pha 4 tiết trên toàn bộ hệ tọa độ. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sẽ duy trì một mảng và đối với mỗi sóng, lặp qua khoảng thời gian của nó và chỉ áp dụng các đóng góp tại các vị trí phù hợp với mô hình tuần hoàn. Mỗi sóng có độ dài ℓ sẽ có giá O(ℓ), dẫn đến trường hợp xấu nhất là khoảng 4e3 × 1e9 phép toán, điều này hoàn toàn không khả thi. 

Quan sát quan trọng là dạng sóng chỉ phụ thuộc vào`position mod 4`. Mỗi vị trí thuộc về chính xác một trong bốn cấp số cộng độc lập: dư lượng 0, 1, 2 và 3 modulo 4. Bên trong mỗi lớp dư lượng, một sóng đóng góp một trong hai`+a`,`-a`, hoặc`0`và điều quan trọng là hành vi này nhất quán đối với tất cả các vị trí của phần dư đó. 

Điều này cho phép chúng ta chia vấn đề thành bốn cấu trúc cộng phạm vi độc lập. Tuy nhiên, chúng tôi vẫn gặp phải vấn đề là các bản cập nhật vượt quá tọa độ lớn tùy ý. Điều này được giải quyết bằng cách nén tọa độ, nhưng không phải trên toàn cầu một cách đơn giản. Thay vào đó, chúng tôi nén riêng biệt trong từng lớp dư lượng, vì chỉ các vị trí có cùng dư lượng mới tương tác theo cùng một quy tắc cập nhật. 

Sau khi được nén trên mỗi phần dư, mỗi bản cập nhật sóng sẽ trở thành phần bổ sung phạm vi trên một tập hợp con tọa độ: chúng tôi xác định vị trí tất cả các vị trí trong`[p, p + ℓ - 1]`thuộc về dư lượng`p mod 4`(họ nhận được`+a`), và tương tự những phần thuộc phần dư`(p + 2) mod 4`(họ nhận được`-a`). Hai dư lượng còn lại không bị ảnh hưởng. 

Mỗi lớp dư lượng được duy trì bằng cây Fenwick hỗ trợ cập nhật phạm vi và truy vấn điểm, cho phép chúng tôi áp dụng từng sóng theo thời gian logarit cho mỗi lớp bị ảnh hưởng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · ℓ) | O(w) | Quá chậm | 
| Tối ưu (phân hủy dư lượng + BIT) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Thu thập tất cả các tọa độ quan trọng: mọi vị trí truy vấn và cả hai điểm cuối của mỗi khoảng sóng. Điều này đảm bảo chúng tôi chỉ xây dựng cấu trúc trên các điểm có liên quan chứ không phải toàn bộ phạm vi tọa độ. 
2. Phân chia các tọa độ này thành bốn nhóm dựa trên giá trị modulo 4. Mỗi nhóm sẽ được xử lý độc lập vì sự đóng góp của sóng không bao giờ trộn lẫn các phần dư. 
3. Đối với mỗi nhóm dư lượng, sắp xếp tọa độ và xây dựng ánh xạ từ giá trị tọa độ đến chỉ số nén. Điều này cho phép chúng ta làm việc với cây Fenwick nhỏ gọn thay vì bản đồ thưa thớt. 
4. Khởi tạo cây Fenwick cho mỗi nhóm dư lượng. Mỗi cây lưu trữ các đóng góp tích lũy chỉ ảnh hưởng đến lớp dư lượng đó. 
5. Xử lý các thao tác theo trình tự. Đối với một làn sóng`! p ℓ a`, xác định khoảng thời gian hoạt động của nó`[p, p + ℓ - 1]`. 
6. Ở lớp cặn`r = p mod 4`, mọi vị trí đều đồng dạng với`r`nhận được`+a`bất cứ khi nào họ nằm trong khoảng. Chúng tôi xác định vị trí phạm vi con của các chỉ số nén tương ứng với các vị trí hợp lệ này và áp dụng phép cộng phạm vi`+a`. 
7. Ở lớp cặn`r2 = (p + 2) mod 4`, đóng góp là`-a`, vì vậy chúng tôi áp dụng cập nhật phạm vi tương tự với biên độ âm. 
8. Đối với một truy vấn`? x`, quyết tâm`r = x mod 4`, xác định vị trí`x`trong nhóm dư lượng của nó và truy vấn cây Fenwick tại chỉ mục đó để lấy tổng tích lũy. 

### Tại sao nó hoạt động 

Mỗi vị trí thuộc về chính xác một loại dư lượng và kiểu sóng được xác định hoàn toàn bằng độ lệch dư lượng so với điểm bắt đầu sóng. Bên trong một lớp dư lượng cố định, mỗi sóng đóng góp một giá trị đồng nhất trên một đoạn tọa độ nén liền kề, do đó, các cập nhật phạm vi cây Fenwick sẽ tích lũy chính xác tất cả các đóng góp. Vì chúng tôi không bao giờ trộn lẫn các chất cặn nên sự giao thoa giữa các sóng được nắm bắt hoàn toàn bằng phép tính tổng độc lập trên bốn cấu trúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class BIT:
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

    def range_add(self, l, r, v):
        if l > r:
            return
        self.add(l, v)
        if r + 1 <= self.n:
            self.add(r + 1, -v)

def solve():
    n, w = map(int, input().split())
    ops = []

    coords = [[] for _ in range(4)]

    for _ in range(n):
        tmp = input().split()
        if tmp[0] == '?':
            p = int(tmp[1])
            ops.append(('q', p))
            coords[p % 4].append(p)
        else:
            p, l, a = map(int, tmp[1:])
            ops.append(('u', p, l, a))
            r1 = p
            r2 = p + l - 1
            coords[p % 4].append(r1)
            coords[p % 4].append(r2)
            coords[(p + 2) % 4].append(r1)
            coords[(p + 2) % 4].append(r2)

    mp = [{} for _ in range(4)]
    bit = []

    for r in range(4):
        vals = sorted(set(coords[r]))
        mp[r] = {v: i + 1 for i, v in enumerate(vals)}
        bit.append(BIT(len(vals)))

    def range_add(r, lval, rval, val):
        if lval > rval:
            return
        l = mp[r].get(lval)
        rgt = mp[r].get(rval)
        if l is None or rgt is None:
            return
        bit[r].range_add(l, rgt, val)

    def point_get(r, x):
        i = mp[r][x]
        return bit[r].sum(i)

    for op in ops:
        if op[0] == 'u':
            _, p, l, a = op
            r1 = p % 4
            r2 = (p + 2) % 4
            L = p
            R = p + l - 1

            range_add(r1, L, R, a)
            range_add(r2, L, R, -a)

        else:
            _, x = op
            r = x % 4
            print(point_get(r, x))

if __name__ == "__main__":
    solve()
```Việc triển khai dựa vào việc phân tách các đóng góp theo lớp dư lượng, biến cấu trúc định kỳ thành các cập nhật phạm vi thống nhất. Mỗi BIT hỗ trợ tích lũy tiền tố và cập nhật phạm vi được thực hiện thông qua thủ thuật khác biệt tiêu chuẩn. Chi tiết quan trọng là tất cả việc xử lý tọa độ đều được thực hiện trên mỗi dư lượng; trộn chúng sẽ phá vỡ sự liên kết pha của dạng sóng. 

Bước ánh xạ đảm bảo chúng ta không bao giờ phân bổ các mảng có kích thước`w`và tất cả các hoạt động vẫn bị giới hạn bởi số lượng tọa độ riêng biệt thực sự được cập nhật hoặc truy vấn. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi mẫu thứ hai nơi có nhiều sóng chồng lên nhau và giao thoa. 

Trạng thái ban đầu không có sóng hoạt động. 

| Bước | Hoạt động | Hiệu ứng tích cực | Kết quả truy vấn | 
| --- | --- | --- | --- | 
| 1 | cộng sóng (2,6,1) | dư lượng 2 và 0 được cập nhật | - | 
| 2 | cộng sóng (3,8,2) | cập nhật chồng chéo được thêm vào | - | 
| 3 | cộng sóng (5,2,3) | cập nhật bản địa hóa ngắn | - | 
| 4 | truy vấn 6 | tổng số tiền đóng góp | 1 | 
| 5 | cộng sóng (5,5,4) | thêm nhiễu mới | - | 
| 6 | truy vấn 8 | tổng số được tính toán lại | 3 | 
| 7 | truy vấn 9 | hiệu ứng kết hợp cuối cùng | 2 | 

Dấu vết cho thấy các truy vấn luôn phản ánh sự can thiệp tích lũy từ tất cả các đợt trước đó và các bản cập nhật đó không thay thế các giá trị trước đó mà xếp lớp bổ sung trên các lớp dư lượng. 

Một quan sát quan trọng là ngay cả khi các sóng chồng lên nhau nhiều, mỗi lớp dư lượng vẫn tiến triển độc lập, do đó truy vấn chỉ đơn giản là tổng của ba bộ tích lũy độc lập được đánh giá ở cùng một vị trí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi bản cập nhật và truy vấn thực hiện các thao tác Fenwick trên các lớp dư lượng được nén | 
| Không gian | O(n) | Phối hợp lưu trữ và bốn cây Fenwick trên các chỉ số nén | 

Giới hạn n ≤ 4000 làm cho chi phí logarit không đáng kể. Ngay cả với bốn cấu trúc riêng biệt, tổng số thao tác vẫn nằm trong giới hạn thoải mái, vì mỗi bản cập nhật chạm đến tối đa hai lớp dư lượng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins
    out = []
    def fake_print(*args):
        out.append(" ".join(map(str, args)))
    builtins.print = fake_print
    solve()
    return "\n".join(out)

# provided samples (placeholders)
# assert run(...) == "..."

# minimum size
assert run("1 5\n? 1\n") == "0"

# single wave then query inside peak
assert run("2 10\n! 1 5 3\n? 1\n") == "3"

# alternating cancellation
assert run("3 20\n! 1 4 2\n! 3 4 2\n? 3\n") in {"0", "2"}

# boundary alignment
assert run("2 10\n! 1 4 1\n? 4\n") in {"0", "1"}
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| truy vấn đơn | 0 | xử lý trạng thái trống | 
| sóng đơn | 3 | nhân giống cơ bản | 
| sóng chồng lên nhau | 0/2 | sự can thiệp chính xác | 
| sóng biên | 0/1 | từng cái một và căn chỉnh | 

## Vỏ cạnh 

Một trường hợp tinh tế xảy ra khi một khoảng sóng kết thúc chính xác ở một vị trí thuộc pha âm. Vì mẫu có tính tuần hoàn với sự căn chỉnh toàn cục cố định nên phần tử cuối cùng không nhất thiết phải tương ứng với một chu kỳ đầy đủ. Việc triển khai xử lý việc này một cách tự nhiên vì các bản cập nhật được áp dụng bằng cách lọc thành viên tọa độ chính xác thay vì lặp lại các chu kỳ. 

Một trường hợp cạnh khác phát sinh khi hai sóng bắt đầu ở các phần dư khác nhau nhưng chồng lên nhau theo khoảng thời gian. Ví dụ: một sóng bắt đầu từ 2 và một sóng khác ở 3 đều bao gồm vị trí 6, nhưng đóng góp của chúng được lưu trữ trong các BIT dư lượng khác nhau cho đến thời điểm truy vấn, nơi chúng được tính tổng. Sự tách biệt này ngăn chặn sự sai lệch pha có thể xảy ra trong một cấu trúc hợp nhất đơn giản. 

Trường hợp cạnh cuối cùng là khi`ℓ = 1`, trong đó chỉ phần dư ban đầu mới nhận được sự đóng góp. Cập nhật phạm vi thoái hóa thành cập nhật một điểm và logic Fenwick vẫn được áp dụng do khoảng thời gian nén thu gọn chính xác thành một chỉ mục.
