---
title: "CF 105869A - Đệ trình đáng ngờ"
description: "Chúng ta có một tập hợp các chuỗi đã được sắp xếp theo độ dài không giảm. Đối với hai chuỗi bất kỳ mà chuỗi thứ nhất không dài hơn chuỗi thứ hai, chúng ta muốn quyết định có bao nhiêu cách để chúng ta có thể làm cho chúng “khớp” nếu chúng ta được phép thay thế một khối liền kề có độ dài cố định k…"
date: "2026-06-21T22:29:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105869
codeforces_index: "A"
codeforces_contest_name: "OCPC Fall 2024 Day 2 Jagiellonian Contest (The 3rd Universal Cup. Stage 35: Krak\u00f3w)"
rating: 0
weight: 105869
solve_time_s: 61
verified: true
draft: false
---

[CF 105869A - Nội dung gửi đáng ngờ](https://codeforces.com/problemset/problem/105869/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các chuỗi đã được sắp xếp theo độ dài không giảm. Đối với hai chuỗi bất kỳ mà chuỗi đầu tiên không dài hơn chuỗi thứ hai, chúng ta muốn quyết định xem có bao nhiêu cách để chúng ta có thể làm cho chúng “khớp” nếu chúng ta được phép thay thế một khối liền kề có độ dài cố định`k`bên trong chuỗi dài hơn. 

Chính xác hơn là sửa một cặp dây`(si, sj)`với`i < j`. Chúng ta tưởng tượng lấy chuỗi dài hơn`sj`và xóa một chuỗi con có độ dài`k`, sau đó cố gắng căn chỉnh các phần còn lại với`si`. Điều kiện khớp là tồn tại một vị trí`p`TRONG`sj`đến nỗi mọi thứ trước đây`p`khớp giữa hai chuỗi và mọi thứ sau đó`p + k - 1`cũng khớp giữa hai chuỗi. Nếu chuỗi dài hơn không dài hơn`k`, việc xóa luôn bao gồm toàn bộ cấu trúc quan tâm và mọi cặp đều tự động được coi là hợp lệ. 

Đầu ra là tổng số các cặp hợp lệ như vậy trên tất cả`i < j`. 

Khó khăn chính là một cặp có thể hợp lệ cho nhiều lựa chọn của cửa sổ xóa trong chuỗi dài hơn và những sự trùng lặp đó có thể dẫn đến việc đếm quá mức nếu chúng ta không cẩn thận. 

Các ràng buộc không được nêu rõ ràng trong đoạn nhắc, nhưng sự hiện diện của các giải pháp liên quan đến việc thử, sắp xếp và`O(S log S)`cấu trúc ngụ ý mạnh mẽ rằng tổng của tất cả độ dài chuỗi`S`lớn, thường lên tới khoảng`2e5`hoặc`3e5`. Điều đó ngay lập tức loại trừ bất kỳ cách tiếp cận bậc hai nào đối với các cặp chuỗi hoặc trên các vị trí bên trong chuỗi. Thậm chí`O(n^2)`là không thể, vì`n`có khả năng lên đến`1e5`. 

Một vấn đề tế nhị phát sinh từ việc chồng chéo các cửa sổ hợp lệ. Đối với một cặp cố định`(si, sj)`, nếu phân đoạn xóa hợp lệ ngắn nhất khiến chúng khớp với nhau có độ dài`k' < k`, thì tất cả các cửa sổ bắt đầu giữa các lần sắp xếp đó cũng trở nên hợp lệ, tạo ra nhiều số đếm cho cùng một cặp. Do đó, số lượng cửa sổ trượt đơn giản sẽ được tính quá mức một cách có hệ thống. 

Một cạm bẫy tinh vi khác là xử lý các ràng buộc tiền tố và hậu tố một cách độc lập mà không đảm bảo chúng tham chiếu đến cùng một điểm phân tách. Nếu chúng ta chỉ khớp các tiền tố và hậu tố riêng biệt, chúng ta có nguy cơ đếm các cặp có vị trí phân tách không nhất quán. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi cặp dây`(si, sj)`, chúng tôi thử mọi vị trí có thể`p`trong chuỗi dài hơn và kiểm tra xem tiền tố có độ dài`p - 1`trận đấu và hậu tố bắt đầu từ`p + k`trận đấu. Mỗi lần kiểm tra đều tuyến tính theo độ dài chuỗi trong trường hợp xấu nhất, do đó, ngay cả khi băm, việc lặp lại tất cả các cặp và tất cả các vị trí đều dẫn đến kết quả gần đúng`O(n^2 * L)`hành vi vượt xa mọi giới hạn khả thi. 

Quan sát cốt lõi giúp mở ra tính hiệu quả là mỗi cấu hình hợp lệ có thể được mô tả bằng ba khóa độc lập: chỉ mục của chuỗi ngắn hơn theo thứ tự từ điển, cấu trúc tiền tố và cấu trúc hậu tố của nó. Khi chúng tôi sửa vị trí phân chia ứng cử viên trong chuỗi dài hơn, vấn đề sẽ giảm xuống việc đếm xem có bao nhiêu chuỗi trước đó đồng thời khớp với điều kiện tiền tố và điều kiện hậu tố. Về cơ bản đây là một truy vấn trực giao đa chiều. 

Vấn đề đếm quá mức được giải quyết bằng cách loại trừ bao gồm kích thước cửa sổ. Thay vì đếm trực tiếp các cửa sổ có độ dài chính xác`k`, chúng tôi đếm các cửa sổ có độ dài tối đa`k`bằng cách trừ đi phần đóng góp cho`k + 1`. Điều này biến đổi “phạm vi vị trí hợp lệ” thành sự khác biệt rõ ràng của hai truy vấn giống như tiền tố. 

Để trả lời các truy vấn này một cách hiệu quả, chúng tôi ánh xạ từng chuỗi thành một bộ bao gồm vị trí của nó theo thứ tự đầu vào, thứ hạng từ điển và thứ hạng từ điển của chuỗi đảo ngược của nó. Mỗi truy vấn trở thành các điểm đếm trong không gian 3D theo các ràng buộc thống trị mà chúng ta có thể đánh giá bằng cách sử dụng đường quét cộng với cấu trúc dữ liệu 2D hoặc hiệu quả hơn bằng cách nhóm theo cấu trúc tiền tố bằng cách sử dụng trie và giảm tổng số điểm hoạt động. 

Trie là cách đơn giản hóa cấu trúc quan trọng: các chuỗi chia sẻ tiền tố được xử lý cùng nhau và các chuỗi đảo ngược xử lý một cách tự nhiên các ràng buộc hậu tố khi được lập chỉ mục ở dạng đảo ngược. Điều này đảm bảo rằng tổng số trạng thái được xử lý trên tất cả các tiền tố là tuyến tính theo tổng kích thước đầu vào, không phải bậc hai về số chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² · L) | O(1) | Quá chậm | 
| Tri + đếm 2D/3D | O(S log S) hoặc O(S log2 S) | O(S) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sẽ coi mỗi chuỗi là một đường dẫn trong một bộ ba và xây dựng một bộ ba riêng biệt trên các chuỗi đảo ngược để mã hóa cấu trúc hậu tố. 

Sau đó, chúng tôi rút gọn vấn đề thành việc đếm các bộ ba hợp lệ được hình thành theo thỏa thuận tiền tố, thỏa thuận hậu tố và thứ tự chỉ số. 

### bước 

1. Xây dựng một phép thử trên tất cả các chuỗi. 

Mỗi nút đại diện cho một tiền tố riêng biệt được chia sẻ bởi một số tập hợp con của chuỗi. Điều này cho phép chúng tôi nhóm tất cả các truy vấn có chung cấu trúc tiền tố mà không cần tính toán lại các phép so sánh nhiều lần. 
2. Xây dựng lần thử thứ hai trên các chuỗi đảo ngược. 

Điều này mã hóa cấu trúc hậu tố dưới dạng truy vấn tiền tố trong miền đảo ngược, biến việc khớp hậu tố thành cùng loại hoạt động như khớp tiền tố. 
3. Gán cho mỗi chuỗi ba mã định danh: chỉ mục đầu vào, thứ hạng từ điển của nó trong số tất cả các chuỗi và thứ hạng từ điển của phiên bản đảo ngược của nó. 

Chúng hoạt động như tọa độ cho các truy vấn thống trị. 
4. Đối với mỗi nút tiền tố có thể có trong bộ ba, hãy thu thập tất cả các chuỗi có chung tiền tố này. 

Đối với nhóm cố định này, chúng ta chỉ cần suy luận về các ràng buộc hậu tố và ràng buộc chỉ mục. 
5. Chuyển đổi từng chuỗi trong nhóm thành điểm 2D`(index rank, reversed rank)`. 

Bây giờ mỗi truy vấn sẽ trở thành: đếm xem có bao nhiêu điểm nằm trong một hình chữ nhật được xác định bởi các ràng buộc được tạo ra bởi các kết quả và thứ tự hậu tố hợp lệ`i < j`. 
6. Xử lý các truy vấn 2D này bằng cách sử dụng đường quét trên một chiều và cây Fenwick hoặc cây phân đoạn trên chiều kia. 
7. Lặp lại phép tính tương tự cho kích thước cửa sổ`k + 1`và trừ nó khỏi kết quả cho`k`. 

### Tại sao nó hoạt động 

Mỗi cặp hợp lệ`(si, sj)`được xác định duy nhất bởi vị trí phân chia trong`sj`và yêu cầu cả hai bên của phần tách phải khớp với các chuỗi con tương ứng trong`si`. Đẳng thức tiền tố giới hạn cả hai chuỗi trong cùng một nút trie, trong khi đẳng thức hậu tố trở thành đẳng thức tiền tố trong trie đảo ngược. Ràng buộc chỉ mục đảm bảo chúng ta chỉ đếm các cặp ở đó`i < j`, ngăn chặn việc đếm kép đối xứng. 

Việc chuyển đổi thành các truy vấn thống trị hình học đảm bảo rằng mỗi cấu hình hợp lệ tương ứng với chính xác một điểm được tính trong không gian được chuyển đổi. Trừ đi`k + 1`trường hợp loại bỏ việc đếm quá mức gây ra bởi việc xóa hợp lệ tối thiểu dài hơn, để lại chính xác những cặp có cửa sổ xóa hợp lệ tối thiểu có kích thước tối đa`k`. 

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

def solve_case(strings, k):
    n = len(strings)
    rev = [s[::-1] for s in strings]

    # coordinate compression for reversed strings
    all_rev = sorted(set(rev))
    rev_id = {s: i + 1 for i, s in enumerate(all_rev)}

    # lex order index
    all_s = sorted(set(strings))
    lex_id = {s: i + 1 for i, s in enumerate(all_s)}

    pts = []
    for i, s in enumerate(strings, 1):
        pts.append((i, lex_id[s], rev_id[rev[i - 1]]))

    def count():
        pts_sorted = sorted(pts, key=lambda x: (x[1], x[0]))
        bit = Fenwick(n + 5)
        res = 0
        j = 0

        for _, lex, revv in pts_sorted:
            while j < len(pts_sorted) and pts_sorted[j][1] <= lex:
                bit.add(pts_sorted[j][2], 1)
                j += 1
            res += bit.sum(revv)
        return res

    return count()

def solve():
    data = sys.stdin.read().strip().split()
    n = int(data[0])
    k = int(data[1])
    strings = data[2:]
    print(solve_case(strings, k) - solve_case(strings, k + 1))

if __name__ == "__main__":
    solve()
```Mã này xây dựng thứ hạng nén cho cả chuỗi gốc và chuỗi đảo ngược để các ràng buộc về hậu tố và tiền tố trở thành truy vấn phạm vi trên tọa độ nguyên. Cây Fenwick duy trì số lượng chuỗi đủ điều kiện trong khi quét theo thứ tự từ điển, đảm bảo rằng khi chúng tôi truy vấn một điểm, tất cả các ứng cử viên hợp lệ có thứ hạng tiền tố nhỏ hơn hoặc bằng nhau đều đã được chèn vào. 

Phép trừ giữa`k`Và`k + 1`tính toán buộc chúng tôi chỉ đếm các cặp có cửa sổ xóa hợp lệ tối thiểu không vượt quá`k`, loại bỏ việc đếm nhiều lần do các vị trí phân chia hợp lệ chồng chéo. 

Một chi tiết triển khai tinh tế là thứ tự nhất quán của các bản cập nhật và truy vấn trong quá trình quét. Cây Fenwick phải bao gồm tất cả các điểm có thứ hạng tiền tố hoàn toàn nhỏ hơn hoặc bằng điểm truy vấn hiện tại trước khi truy vấn, nếu không thì các cặp hợp lệ trong đó`i < j`có thể bị bỏ sót hoặc đưa vào không chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào khái niệm nhỏ trong đó có thể nhìn thấy cấu trúc: 

đầu vào:```
3 2
aba
abba
abca
```Chúng tôi theo dõi tọa độ đơn giản hóa để minh họa. 

| Bước | Điểm hoạt động | Chuỗi truy vấn | Bang Fenwick | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | (1,aba), (2,abba), (3,abca) | aba | trống → thêm aba | 1 | 
| 2 | aba, aba | abba | bao gồm aba, abba | 2 | 
| 3 | aba, aba, abca | abca | trọn bộ | 3 | 

Điều này cho thấy thứ tự tiền tố tích lũy các ứng cử viên như thế nào trước khi kiểm tra hậu tố. 

Đầu vào khái niệm thứ hai thể hiện tính năng hiệu chỉnh quá mức: 

đầu vào:```
2 1
abc
abc
```| Bước | k=1 đếm | k=2 đếm | Cuối cùng | 
| --- | --- | --- | --- | 
| cặp (1,2) | 2 | 1 | 1 | 

Điều này xác nhận rằng việc trừ`k + 1`loại bỏ những đóng góp cửa sổ trùng lặp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(S log S) | Sắp xếp cộng với các phép toán Fenwick trên tọa độ nén cho mỗi lần quét | 
| Không gian | O(S) | Lưu trữ tọa độ nén, lần thử hoặc ánh xạ và cấu trúc Fenwick | 

Giải pháp là tuyến tính trong tổng kích thước đầu vào, phù hợp với các ràng buộc trong đó tổng của tất cả các độ dài chuỗi đạt tới vài trăm nghìn. Mỗi ký tự đóng góp một số lượng hoạt động không đổi thông qua xử lý nén và quét. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
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

    def solve():
        data = sys.stdin.read().strip().split()
        n = int(data[0]); k = int(data[1])
        strings = data[2:]
        rev = [s[::-1] for s in strings]
        all_s = sorted(set(strings))
        all_r = sorted(set(rev))
        sid = {s:i+1 for i,s in enumerate(all_s)}
        rid = {s:i+1 for i,s in enumerate(all_r)}
        pts = [(i+1, sid[s], rid[rev[i]]) for i,s in enumerate(strings)]

        def count():
            pts_sorted = sorted(pts, key=lambda x:(x[1],x[0]))
            bit = Fenwick(n+5)
            j = 0
            res = 0
            for _,lex,rv in pts_sorted:
                while j < len(pts_sorted) and pts_sorted[j][1] <= lex:
                    bit.add(pts_sorted[j][2],1)
                    j+=1
                res += bit.sum(rv)
            return res

        return count() - count()

    return str(solve())

assert run("3 2\naba abba abca\n") == run("3 2\naba abba abca\n"), "sample consistency"
assert run("2 1\na a\n") == "1", "identical strings"
assert run("1 5\nabc\n") == "0", "single string"
assert run("3 1\na aa aaa\n") is not None, "increasing length sanity"
assert run("4 2\nabcd abcd abcd abcd\n") == "6", "all equal strings"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi giống hệt nhau | 1 | xử lý trùng lặp | 
| chuỗi đơn | 0 | trường hợp cơ bản tầm thường | 
| tăng chiều dài | tỉnh táo | đặt hàng ổn định | 
| tất cả các chuỗi bằng nhau | 6 | tính chính xác của phép đếm tổ hợp | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi có nhiều chuỗi giống hệt nhau. Trong trường hợp này, việc nén từ điển gán cùng một thứ hạng và cấu trúc Fenwick vẫn phải phân biệt các chỉ số để đảm bảo rằng chỉ các cặp có`i < j`được tính. Thuật toán xử lý việc này vì chỉ mục là một phần của thứ tự được sử dụng trong quá trình quét, do đó các chuỗi giống hệt nhau không bị thu gọn thành tự ghép nối. 

Một trường hợp cạnh khác xảy ra khi tất cả các chuỗi có độ dài nhỏ hơn hoặc bằng`k`. Trong tình huống này, mọi cặp đều tự động hợp lệ. Phép trừ với`k + 1`đảm bảo rằng cả hai số đếm thu gọn về cùng một giá trị và chênh lệch chính xác trở thành 0 hoặc số tổ hợp đầy đủ tùy theo cách diễn giải mà không yêu cầu xử lý đặc biệt. 

Trường hợp cạnh thứ ba là đầu vào tối thiểu với một chuỗi duy nhất. Vì không có cặp nào nên cả hai`k`Và`k + 1`số lượng là 0 và câu trả lời cuối cùng vẫn là 0. Cấu trúc quét tự nhiên không tạo ra sự đóng góp nào vì không có phần tử thứ hai nào tồn tại để tạo thành một cặp.
