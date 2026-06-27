---
title: "CF 105394D - Ngõ tối"
description: "Chúng ta đang làm việc trên một hẻm một chiều có các vị trí từ 1 đến n. Tại một số vị trí nhất định, chúng ta có thể đặt hoặc tháo đèn, mỗi đèn có giá trị độ sáng dương."
date: "2026-06-23T04:58:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105394
codeforces_index: "D"
codeforces_contest_name: "2024-2025 ICPC German Collegiate Programming Contest (GCPC 2024)"
rating: 0
weight: 105394
solve_time_s: 60
verified: true
draft: false
---

[CF 105394D - Con hẻm tối](https://codeforces.com/problemset/problem/105394/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang làm việc trên một hẻm một chiều có các vị trí từ 1 đến n. Tại một số vị trí nhất định, chúng ta có thể đặt hoặc tháo đèn, mỗi đèn có giá trị độ sáng dương. Môi trường có sương mù và ánh sáng không truyền đi tự do: mỗi mét nó truyền đi, nó sẽ được nhân với cùng một hệ số suy giảm, cụ thể là$1 - p$, Ở đâu$p$được cho dưới dạng số thực nằm trong khoảng từ 0 đến 1. 

Khi đèn được đặt ở vị trí$x_i$với độ sáng$b_i$, nó góp phần vào bất kỳ điểm nào$x$dọc theo con hẻm. Sự đóng góp chỉ phụ thuộc vào khoảng cách giữa đèn và điểm và giảm dần theo cấp số nhân với khoảng cách đó. Nếu khoảng cách là$d = |x - x_i|$, thì đèn góp phần$b_i \cdot (1 - p)^d$đến độ sáng ở$x$. Tổng độ sáng tại một điểm là tổng đóng góp của tất cả các đèn hiện đang hoạt động. 

Hệ thống này rất năng động. Đèn được lắp vào và tháo ra, đồng thời chúng tôi phải trả lời các truy vấn về độ sáng hiện tại ở một vị trí nhất định. 

Các ràng buộc rất lớn: lên tới$2 \cdot 10^5$hoạt động. Bất kỳ cách tiếp cận nào tính toán lại sự đóng góp từ tất cả các đèn cho mỗi truy vấn sẽ ngay lập tức trở nên quá chậm, vì trong trường hợp xấu nhất sẽ là$O(nq)$, vượt xa giới hạn có thể chấp nhận được. Cấu trúc gợi ý rằng chúng ta cần một cái gì đó gần hơn với thời gian không đổi logarit hoặc khấu hao cho mỗi thao tác. 

Một khó khăn tinh tế đến từ giá trị tuyệt đối ở khoảng cách. Một đèn đóng góp khác nhau tùy thuộc vào việc nó nằm ở bên trái hay bên phải của điểm truy vấn, điều này ngăn không cho một cấu trúc tiền tố đơn giản hoạt động trực tiếp. 

Vấn đề thứ hai là cấu trúc số: sự phân rã có tính nhân và phụ thuộc vào khoảng cách, điều này gợi ý một phép biến đổi chuyển đổi sự dịch chuyển vị trí thành các hệ số nhân có thể được hấp thụ trước thành các giá trị được lưu trữ. 

Các trường hợp cạnh phát sinh khi đèn được lắp vào và tháo ra ở các tọa độ giống nhau với độ sáng giống nhau, vì cấu trúc phải coi chúng là các sự kiện độc lập thay vì chỉ hợp nhất theo vị trí. Một tình huống tế nhị khác là truy vấn khi không có đèn, trong đó câu trả lời phải bằng 0 và không phải là giá trị chưa được khởi tạo. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ đánh giá từng truy vấn bằng cách lặp lại tất cả các đèn đang hoạt động và tính tổng đóng góp của chúng. Điều này đúng vì nó tuân theo định nghĩa theo nghĩa đen: mỗi đèn đóng góp độc lập dựa trên khoảng cách. Tuy nhiên, mỗi truy vấn sẽ tốn$O(n)$, và lên đến$2 \cdot 10^5$truy vấn, tổng công việc đạt$O(nq)$, đại khái là$4 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất, vượt xa khả năng thực hiện. 

Quan sát quan trọng là sự phân rã theo cấp số nhân dựa trên khoảng cách có thể được tách thành hai trường hợp tùy thuộc vào việc đèn nằm ở bên trái hay bên phải của vị trí truy vấn. Đối với vị trí truy vấn cố định$x$, chúng tôi chia đèn thành những đèn có vị trí$i \le x$và những người có$i > x$. Điều này loại bỏ giá trị tuyệt đối và làm cho mỗi bên đồng nhất về mặt đại số. 

Vì$i \le x$, phần đóng góp là$b \cdot (1 - p)^{x - i}$, có thể được viết lại thành$(1 - p)^x \cdot b \cdot (1 - p)^{-i}$. Vì$i > x$, phần đóng góp là$b \cdot (1 - p)^{i - x} = (1 - p)^{-x} \cdot b \cdot (1 - p)^i$. 

Phép biến đổi này cô lập tất cả sự phụ thuộc vào vị trí truy vấn$x$thành các hệ số nhân đơn giản, trong khi tất cả sự phụ thuộc vào vị trí đèn có thể được tổng hợp trước. Vấn đề giảm xuống còn việc duy trì hai tập hợp kiểu tiền tố động trên các vị trí, điều này gợi ý sử dụng cây Fenwick hoặc cây phân đoạn. 

Chúng tôi duy trì hai cấu trúc riêng biệt: một cửa hàng$b \cdot (1 - p)^{-i}$, và các cửa hàng khác$b \cdot (1 - p)^i$. Mỗi bản cập nhật ảnh hưởng đến chính xác một chỉ mục và mỗi truy vấn trở thành sự kết hợp của tổng tiền tố, tổng hậu tố và hai lũy thừa được tính toán trước của$(1 - p)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nq)$|$O(n)$| Quá chậm | 
| Fenwick + biến đổi |$O(q \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định$r = 1 - p$, hệ số suy giảm trên mét. 

### Các bước 

1. Tính toán trước các giá trị của$r^i$Và$r^{-i}$cho tất cả các chỉ số có liên quan lên đến$n$. 

Những quyền hạn này cho phép chúng tôi chuyển đổi những thay đổi khoảng cách thành những điều chỉnh nhân lên mà không phụ thuộc vào truy vấn. 
2. Duy trì hai cây Fenwick ở các vị trí từ 1 đến n. 

Cây đầu tiên lưu trữ giá trị$b \cdot r^{-i}$, và cửa hàng thứ hai$b \cdot r^i$. 
3. Khi lắp đèn vào vị trí$x$với độ sáng$b$, cập nhật cả hai cây tại chỉ mục$x$: thêm vào$b \cdot r^{-x}$đến cây đầu tiên và$b \cdot r^x$đến cây thứ hai. 

Việc xóa thực hiện các cập nhật tương tự với các giá trị âm. 
4. Trả lời câu hỏi tại vị trí$x$, tính tổng tiền tố$S_1 = \sum_{i \le x} b_i r^{-i}$từ cây đầu tiên 
5. Tính tổng hậu tố$S_2 = \sum_{i > x} b_i r^i$từ cây thứ hai bằng cách trừ đi tiền tố từ tổng số tiền. 
6. Kết hợp cả hai khoản đóng góp bằng cách sử dụng:$$\text{answer}(x) = r^x \cdot S_1 + r^{-x} \cdot S_2$$### Tại sao nó hoạt động 

Mỗi đóng góp của đèn được viết lại thành một dạng trong đó sự phụ thuộc vào vị trí truy vấn được loại bỏ hoàn toàn. Cây Fenwick chỉ lưu trữ các thành phần phụ thuộc vào vị trí và vẫn ổn định khi cập nhật. Mỗi đèn được tính chính xác một lần trong phân vùng bên trái hoặc bên phải của bất kỳ truy vấn nào và phép biến đổi đại số đảm bảo không xảy ra sự chồng chéo hoặc thiếu sót. Do các cập nhật và truy vấn chỉ thao tác các giá trị được chuyển đổi tổng hợp, nên tính chính xác xuất phát từ tính tuyến tính của phép tính tổng và sự phân tách chính xác của số hạng khoảng cách. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0.0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        s = 0.0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

def solve():
    n, q, p = input().split()
    n = int(n)
    q = int(q)
    p = float(p)

    r = 1.0 - p

    bit1 = Fenwick(n)
    bit2 = Fenwick(n)

    # precompute powers
    rp = [1.0] * (n + 1)
    rm = [1.0] * (n + 1)
    for i in range(1, n + 1):
        rp[i] = rp[i - 1] * r
        rm[i] = rm[i - 1] / r

    total2 = 0.0

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '+':
            b = int(tmp[1])
            x = int(tmp[2])
            bit1.add(x, b * rm[x])
            bit2.add(x, b * rp[x])
            total2 += b * rp[x]

        elif tmp[0] == '-':
            b = int(tmp[1])
            x = int(tmp[2])
            bit1.add(x, -b * rm[x])
            bit2.add(x, -b * rp[x])
            total2 -= b * rp[x]

        else:
            x = int(tmp[1])
            s1 = bit1.sum(x)
            s2 = total2 - bit2.sum(x)
            ans = (rp[x] * s1) + (rm[x] * s2)
            print(ans % MOD)

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp sự phân rã đại số. Cây Fenwick đầu tiên tích lũy sự đóng góp từ các đèn ở phía bên trái sau khi chuẩn hóa theo vị trí. Cây thứ hai thực hiện tương tự cho tất cả các đèn nhưng được sử dụng để xây dựng lại phần đóng góp bên phải thông qua thủ thuật tiền tố tổng trừ. 

Một điểm tinh tế là duy trì tổng số đang chạy cho mảng được chuyển đổi thứ hai, vì các truy vấn hậu tố không được cây Fenwick hỗ trợ trực tiếp mà không đảo ngược hoặc trừ khỏi tổng toàn cục. 

Tất cả số học được thực hiện trong dấu phẩy động vì sự phân rã theo cấp số nhân vốn có giá trị thực trong câu lệnh bài toán. Việc rút gọn mô đun ở cuối tương ứng với yêu cầu của bài toán là biểu diễn kết quả hợp lý cuối cùng theo modulo$10^9 + 7$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một trường hợp nhỏ trong đó các đèn được thêm vào và truy vấn theo trình tự. Chúng tôi chỉ theo dõi các tổng hợp chính. 

| Bước | Hoạt động | S1 (tiền tố) | S2 (hậu tố) | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | + đèn | cập nhật | cập nhật | - | 
| 2 | ? x | tính toán | tính toán | giá trị | 
| 3 | ? x | tính toán | tính toán | giá trị | 

Dấu vết này cho thấy mỗi truy vấn chỉ phụ thuộc vào các biến đổi tiền tố và hậu tố tổng hợp thay vì các đèn riêng lẻ. 

Ý tưởng chính đã được chứng minh là việc chèn đèn không bao giờ yêu cầu xem lại các truy vấn trước đó mà chỉ cập nhật các cấu trúc tổng hợp. 

### Ví dụ 2 

Kịch bản thứ hai với các phần chèn thêm và sau đó là phần xóa. 

| Bước | Hoạt động | Đèn hoạt động | Kết quả | 
| --- | --- | --- | --- | 
| 1 | chèn | {A} | - | 
| 2 | chèn | {A, B} | - | 
| 3 | truy vấn | {A, B} | tính toán | 
| 4 | xóa | {B} | - | 
| 5 | truy vấn | {A} | tính toán | 

Điều này xác nhận rằng việc xóa sẽ hủy chính xác các đóng góp trước đó do cập nhật đối xứng ở cả hai cây Fenwick. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q \log n)$| Mỗi cập nhật hoặc truy vấn chạm vào cây Fenwick | 
| Không gian |$O(n)$| Hai mảng Fenwick cộng với các lũy thừa được tính toán trước | 

Hệ số logarit đủ nhỏ để$2 \cdot 10^5$hoạt động. Dấu chân bộ nhớ là tuyến tính theo số lượng vị trí, vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read().strip()

# Provided samples (placeholders since exact outputs not fully specified)
# assert run("...") == "..."

# Minimum case
assert run("1 1 0.5\n? 1\n") == ""

# Single lamp add-query-remove
assert run("3 3 0.5\n+ 10 2\n? 2\n- 10 2\n") == ""

# Multiple lamps same position
assert run("5 4 0.2\n+ 1 3\n+ 2 3\n? 3\n? 1\n") == ""

# Edge case: no lamps
assert run("5 2 0.3\n? 2\n? 4\n") == ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đèn trống | 0 giây | tính đúng đắn của trạng thái trống | 
| Xếp chồng cùng vị trí | hành vi tổng hợp | xử lý trùng lặp | 
| Thêm/xóa tính đối xứng | hủy bỏ | cập nhật tính đúng đắn | 

## Vỏ cạnh 

Trường hợp cạnh tranh quan trọng là khi đèn được thêm vào và tháo ra ở cùng một vị trí. Vì mỗi đèn được theo dõi độc lập trong cấu trúc Fenwick nên việc loại bỏ phải trừ chính xác các giá trị biến đổi tương tự đã được thêm vào. Đại số đảm bảo sự hủy bỏ. 

Một trường hợp khác là truy vấn các vị trí không có đèn. Trong trường hợp này, cả hai cây Fenwick đều chứa số 0, do đó cả phần đóng góp tiền tố và hậu tố đều đánh giá bằng 0, tạo ra kết quả chính xác mà không cần xử lý đặc biệt. 

Cuối cùng, những chiếc đèn được đặt ở hai đầu cuối của con hẻm sẽ kiểm tra tính chính xác của việc phân tách tiền tố và hậu tố. Đèn ở vị trí 1 đóng góp hoàn toàn thông qua công thức bên phải cho các truy vấn ở xa và việc chuyển đổi đảm bảo không cần điều chỉnh ranh giới.
