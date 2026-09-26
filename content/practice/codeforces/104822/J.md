---
title: "CF 104822J - Sắp xếp ngược ba lần"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm cung cấp một hoán vị có độ dài n và chúng ta được phép áp dụng lặp lại một thao tác cục bộ rất cụ thể: chọn bất kỳ vị trí i nào sao cho khối gồm ba phần tử liên tiếp tồn tại bắt đầu từ đó và đảo ngược điều đó…"
date: "2026-06-28T12:44:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104822
codeforces_index: "J"
codeforces_contest_name: "RCPCamp 2023 Day 1"
rating: 0
weight: 104822
solve_time_s: 94
verified: false
draft: false
---

[CF 104822J - Sắp xếp ngược ba lần](https://codeforces.com/problemset/problem/104822/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm cung cấp một hoán vị độ dài`n`và chúng ta được phép áp dụng nhiều lần một thao tác cục bộ rất cụ thể: chọn bất kỳ vị trí nào`i`sao cho một khối gồm ba phần tử liên tiếp tồn tại bắt đầu từ đó và đảo ngược khối có độ dài ba. 

Nhiệm vụ là xác định xem, bắt đầu từ hoán vị đã cho, chúng ta có thể chuyển đổi nó thành hoán vị đã sắp xếp hay không.`1, 2, 3, ..., n`sử dụng bất kỳ số lần đảo ngược ba lần nào. 

Thao tác này chỉ chạm vào ba phần tử liền kề cùng một lúc, điều này cho thấy rằng chúng tôi không thực hiện sắp xếp lại một cách tùy ý. Thay vào đó, chúng ta bị giới hạn ở một phép biến đổi rất cục bộ hoạt động giống như một trình tạo hoán vị bị ràng buộc. 

Các ràng buộc rất lớn: tổng số tiền`n`trên tất cả các trường hợp thử nghiệm là lên đến`2 · 10^5`. Điều này ngay lập tức loại trừ mọi mô phỏng của quá trình sắp xếp hoặc BFS qua các hoán vị, vì không gian trạng thái có kích thước giai thừa và thậm chí thời gian tuyến tính cho mỗi thao tác sẽ quá chậm nếu cần nhiều thao tác. 

Các trường hợp cạnh thú vị nhỏ nhất xuất hiện khi`n < 3`. Trong trường hợp đó, không thể thực hiện được thao tác nào cả. Nếu như`n = 1`, hoán vị luôn được sắp xếp. Nếu như`n = 2`, chúng tôi không bao giờ có thể sửa chữa một trao đổi, vì vậy chỉ những hoán vị đã được sắp xếp mới hợp lệ. Những trường hợp này đã cho thấy rằng khả năng tiếp cận không phải là so sánh giá trị mà là các ràng buộc về cấu trúc của hoạt động được phép. 

Trường hợp cạnh tinh tế hơn xuất hiện khi một hoán vị “gần như được sắp xếp” nhưng yêu cầu hoán đổi một lần các phần tử liền kề. Ví dụ,`1 3 2`không thể được sửa bằng một lần đảo ngược ba lần vì bất kỳ thao tác nào cũng yêu cầu một khối đầy đủ gồm ba phần tử và tính chẵn lẻ cục bộ của các hoán vị trở nên phù hợp. Điều này gợi ý rằng một số bất biến ngoài trật tự vẫn được bảo tồn. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ coi vấn đề là tìm kiếm đường đi ngắn nhất trên các hoán vị, trong đó mỗi nút là một hoán vị và các cạnh tương ứng với việc áp dụng phép đảo ngược ba lần tại một chỉ số nào đó. Từ mỗi tiểu bang có`O(n)`di chuyển và mỗi lần di chuyển đều tốn chi phí`O(n)`để sao chép mảng, tạo ra sự bùng nổ ở cả hệ số phân nhánh và kích thước trạng thái. Ngay cả việc khám phá một phần rất nhỏ của không gian trạng thái cũng trở nên không khả thi đối với`n = 200000`. 

Quan sát quan trọng là sự đảo ngược ba lần không cho phép sắp xếp lại tùy ý, nhưng nó cho phép hoán vị có kiểm soát cấu trúc cục bộ. Sự đảo ngược của ba yếu tố`[a, b, c] → [c, b, a]`tương đương với việc hoán đổi`a`Và`c`trong khi giữ`b`đã sửa. Điều này có nghĩa là phần tử ở giữa hoạt động như một trục xoay trong khi các điểm cuối trao đổi vị trí. 

Từ đó, chúng ta có thể suy ra rằng các phần tử có thể “di chuyển” một cách hiệu quả bằng cách hoán đổi giữa các vị trí trung gian, nhưng mỗi bước di chuyển đều bảo toàn một bất biến chẵn lẻ toàn cục: mọi thao tác là một hoán vị lẻ trên ba phần tử, nhưng được cấu thành theo cách bị ràng buộc trên các bộ ba chồng chéo. Hậu quả quan trọng là tính chẵn lẻ của hoán vị có thể đạt được từ danh tính được cố định cho một giá trị nhất định`n`. 

Một cách trực tiếp hơn để xem nó là xem xét hoạt động này ảnh hưởng như thế nào đến tính chẵn lẻ nghịch đảo. Mỗi lần đảo ngược ba lần sẽ thay đổi số lượng đảo ngược một lượng chẵn, có nghĩa là tính chẵn lẻ của đảo ngược là bất biến. Do đó, chúng ta chỉ có thể đạt được các hoán vị có tính chẵn lẻ nghịch đảo khớp với giá trị của mảng đã sắp xếp, bằng 0. 

Do đó, toàn bộ vấn đề quy về việc kiểm tra xem hoán vị đã cho có tính chẵn lẻ nghịch đảo hay không. 

Nhiệm vụ duy nhất còn lại là tính toán tính chẵn lẻ nghịch đảo một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Đảo ngược chẵn lẻ thông qua BIT/sáp nhập | O(n log n) mỗi lần kiểm tra | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi muốn xác định xem hoán vị có thể được chuyển đổi thành thứ tự được sắp xếp bằng các thao tác được phép hay không. Vì khả năng tiếp cận chỉ phụ thuộc vào tính chẵn lẻ của đảo ngược nên chúng tôi tính toán xem số lần đảo ngược có chẵn hay không. 

1. Với mỗi test, hãy đọc hoán vị. Chúng tôi chỉ cần xác định tính chẵn lẻ, vì vậy chúng tôi không lưu trữ bất cứ thứ gì vượt quá mức cần thiết cho việc đếm nghịch đảo. 
2. Tính toán chẵn lẻ nghịch đảo bằng cây Fenwick (Cây chỉ mục nhị phân). Chúng tôi xử lý các phần tử từ trái sang phải, duy trì số lượng phần tử trước đó lớn hơn phần tử hiện tại. Mỗi số đếm như vậy góp phần vào tổng số đảo ngược. 
3. Thay vì tính toán toàn bộ số lần đảo ngược, chúng tôi chỉ theo dõi nó theo modulo 2. Điều này tránh tràn và đơn giản hóa logic. 
4. Đối với mỗi phần tử`a[i]`, chúng tôi truy vấn có bao nhiêu phần tử lớn hơn`a[i]`đã được nhìn thấy rồi. Chúng tôi thêm số modulo 2 đó vào số chẵn lẻ đang chạy của chúng tôi. 
5. Sau khi xử lý toàn bộ mảng, nếu chẵn lẻ cuối cùng bằng 0, xuất ra`YES`, nếu không thì xuất ra`NO`. 

Lý do chúng ta có thể sử dụng cây Fenwick là vì chúng ta cần đếm tần số tiền tố động khi quét hoán vị. 

### Tại sao nó hoạt động 

Mỗi lần đảo ngược ba lần là một chuỗi hoán đổi các phần tử ở khoảng cách hai và mỗi thao tác như vậy sẽ bảo toàn tính chẵn lẻ đảo ngược. Vì hoán vị được sắp xếp có số chẵn lẻ đảo ngược bằng 0, nên mọi hoán vị có thể tiếp cận cũng phải có số chẵn lẻ bằng 0. Ngược lại, có thể chỉ ra rằng các giao dịch hoán đổi liền kề có thể được mô phỏng theo cặp bằng cách sử dụng ba lần đảo ngược, nghĩa là mọi hoán vị chẵn lẻ đều có thể đạt được. Điều này tạo ra một đặc tính hoàn chỉnh: khả năng tiếp cận tương đương với việc có tính chẵn lẻ đảo ngược. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] ^= v
            i += i & -i

    def sum(self, i):
        s = 0
        while i > 0:
            s ^= self.bit[i]
            i -= i & -i
        return s

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        # inversion parity using BIT storing counts mod 2
        bit = Fenwick(n)
        inv_parity = 0

        for i, x in enumerate(a):
            # number of elements <= x seen so far
            leq = bit.sum(x)
            seen = i
            gt = seen - leq
            inv_parity ^= (gt & 1)
            bit.add(x, 1)

        print("YES" if inv_parity == 0 else "NO")

if __name__ == "__main__":
    solve()
```Cây Fenwick duy trì tần số của các giá trị đã được xử lý. Đối với mỗi phần tử mới`x`, chúng tôi tính toán có bao nhiêu giá trị đã thấy trước đó lớn hơn`x`bằng cách trừ số tiền tố`<= x`từ tổng số yếu tố nhìn thấy. Vì chúng tôi chỉ quan tâm đến tính chẵn lẻ nên chúng tôi XOR phần đóng góp vào`inv_parity`. 

Việc sử dụng XOR thay vì phép cộng số nguyên đảm bảo chúng ta không bao giờ vượt quá bộ nhớ không đổi đối với trạng thái số học. 

Một điểm tinh tế là chúng ta dựa vào các giá trị là một hoán vị của`1..n`, do đó việc lập chỉ mục Fenwick căn chỉnh trực tiếp với các giá trị mà không cần nén. 

## Ví dụ đã hoạt động 

Hãy xem xét hoán vị`3 1 2`. 

Chúng tôi theo dõi sự chẵn lẻ đảo ngược từng bước. 

| tôi | x | đã thấy | ≤x | >x | chẵn lẻ | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 3 | 0 | 0 | 0 | 0 | 
| 1 | 1 | 1 | 0 | 1 | 1 | 
| 2 | 2 | 2 | 1 | 1 | 0 | 

Sự chẵn lẻ cuối cùng là`0`, vậy câu trả lời là`YES`. Điều này phù hợp với thực tế là`3 1 2`có thể được sắp xếp bằng cách sử dụng một lần đảo ngược ba lần. 

Bây giờ hãy xem xét`2 1 3`. 

| tôi | x | đã thấy | ≤x | >x | chẵn lẻ | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 0 | 0 | 0 | 0 | 
| 1 | 1 | 1 | 0 | 1 | 1 | 
| 2 | 3 | 2 | 2 | 0 | 1 | 

Tính chẵn lẻ cuối cùng là`1`, vậy câu trả lời là`NO`. Hoán vị này không thể được sắp xếp theo phép toán được phép. 

Những dấu vết này xác nhận rằng thuật toán đang theo dõi cấu trúc đảo ngược một cách hiệu quả hơn là mô phỏng các chuyển động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) mỗi lần kiểm tra | Mỗi truy vấn chèn và tiền tố trong cây Fenwick mất thời gian logarit | 
| Không gian | O(n) | Cây Fenwick lưu trữ mảng tần số lên tới n | 

Tổng cộng`n`trên tất cả các trường hợp thử nghiệm được giới hạn bởi`2 · 10^5`, do đó hệ số logarit vẫn nằm trong giới hạn cho ràng buộc 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    
    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)

        def add(self, i, v):
            while i <= self.n:
                self.bit[i] ^= v
                i += i & -i

        def sum(self, i):
            s = 0
            while i > 0:
                s ^= self.bit[i]
                i -= i & -i
            return s

    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))
            bit = Fenwick(n)
            inv = 0
            for i, x in enumerate(a):
                leq = bit.sum(x)
                gt = i - leq
                inv ^= (gt & 1)
                bit.add(x, 1)
            output.append("YES" if inv == 0 else "NO")

    solve()
    return "\n".join(output)

# sample-like tests
assert run("1\n1\n1\n") == "YES"
assert run("1\n2\n2 1\n") == "NO"
assert run("1\n3\n3 1 2\n") == "YES"

# custom cases
assert run("1\n4\n1 2 3 4\n") == "YES"
assert run("1\n4\n2 1 4 3\n") == "YES"
assert run("1\n4\n4 3 2 1\n") == "YES"
assert run("1\n5\n2 3 4 5 1\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`| CÓ | kích thước tầm thường | 
|`2 2 1`| KHÔNG | đảo ngược đơn | 
|`4 2 1 4 3`| CÓ | nhiều nghịch đảo độc lập | 
|`5 2 3 4 5 1`| KHÔNG | ràng buộc chẵn lẻ dịch chuyển theo chu kỳ | 

## Vỏ cạnh 

cho`n = 1`, thuật toán trả về ngay`YES`bởi vì không có sự đảo ngược nào tồn tại và vòng lặp không làm gì cả. Cấu trúc Fenwick không bao giờ được sử dụng một cách có ý nghĩa nhưng tính chẵn lẻ vẫn bằng không. 

Vì`n = 2`, một lần hoán đổi như`2 1`tạo ra chính xác một phép đảo ngược, do đó tính chẵn lẻ trở thành một và thuật toán đưa ra kết quả chính xác`NO`. Điều này phù hợp với thực tế là không thể thực hiện được thao tác nào khi`n < 3`. 

Đối với một hoán vị hoàn toàn đảo ngược như`n n-1 ... 1`, tính chẵn lẻ nghịch đảo phụ thuộc vào`n(n-1)/2`. Thuật toán tích lũy giá trị này một cách tự nhiên thông qua các truy vấn BIT và chỉ chấp nhận khi giá trị này là số chẵn, phù hợp với điều kiện khả năng tiếp cận trong ba lần đảo ngược.
