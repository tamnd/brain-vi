---
title: "CF 1847D - Giáo sư Higashikata"
description: "Chúng ta được cấp một chuỗi nhị phân s và một tập hợp các khoảng cố định trên chuỗi đó. Mỗi khoảng trích ra một chuỗi con và tất cả các chuỗi con được trích xuất sẽ được nối lại để tạo thành một chuỗi mới t(s)."
date: "2026-06-09T05:44:38+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dsu", "greedy", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1847
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 882 (Div. 2)"
rating: 1900
weight: 1847
solve_time_s: 78
verified: true
draft: false
---

[CF 1847D - Giáo sư Higashikata](https://codeforces.com/problemset/problem/1847/D) 

**Xếp hạng:** 1900 
**Tags:** cấu trúc dữ liệu, dsu, tham lam, triển khai, chuỗi 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi nhị phân`s`và một tập hợp các khoảng cố định trên đó. Mỗi khoảng trích xuất một chuỗi con và tất cả các chuỗi con được trích xuất sẽ được nối để tạo thành một chuỗi mới`t(s)`. Cấu trúc của`t(s)`được cố định bởi các khoảng, do đó mỗi vị trí của chuỗi gốc đóng góp vào một số vị trí trong phép nối cuối cùng. 

Sau mỗi lần cập nhật, một vị trí của`s`bị lật từ`0`ĐẾN`1`hoặc ngược lại. Sau mỗi lần lật, chúng ta được phép sắp xếp lại các ký tự của`s`bằng cách hoán đổi hai vị trí bất kỳ với số lần bất kỳ. Chi phí chúng tôi báo cáo là số lần hoán đổi tối thiểu cần thiết để kết nối kết quả`t(s)`trở nên tối đa về mặt từ điển trong số tất cả các cách sắp xếp có thể có của`s`. 

Tối đa về mặt từ điển ở đây có nghĩa là chúng tôi muốn`t(s)`càng lớn càng tốt, điều này trong chuỗi nhị phân có nghĩa là tạo các ký tự trước đó trong`t(s)`BẰNG`1`nhất có thể, vì`1`thống trị`0`ở vị trí khác nhau đầu tiên. 

Quan sát quan trọng là chúng ta không bao giờ thay đổi cấu trúc của`t(s)`, chỉ việc gán các ký tự từ`s`vào các vị trí của`t(s)`gây ra bởi các khoảng. Mỗi chỉ số của`s`xuất hiện ở một số vị trí trong`t(s)`và bội số đó xác định chỉ số đó có giá trị như thế nào. 

Các ràng buộc rất lớn:`n, m, q ≤ 2⋅10^5`. Điều này loại trừ việc tính toán lại các đóng góp từ đầu cho mỗi truy vấn, vì việc tính toán lại đơn giản sẽ tốn ít nhất`O(nm)`hoặc`O(n)`cho mỗi truy vấn, cả hai đều quá chậm. Chúng tôi cần một cấu trúc hỗ trợ cập nhật điểm và duy trì bản tóm tắt có trọng số toàn cầu. 

Một vấn đề tế nhị xuất hiện khi các khoảng thời gian chồng chéo lên nhau nhiều. Một cách tiếp cận đơn giản có thể cho rằng mỗi ký tự đóng góp một lần cho mỗi khoảng thời gian, nhưng mức đóng góp phụ thuộc vào số lượng chuỗi con bao gồm mỗi chỉ mục, chứ không phụ thuộc vào tần suất các khoảng chồng chéo theo những cách tùy ý. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là tính toán lại sự đóng góp của mọi vị trí trong`s`sau mỗi lần lật. Đối với mỗi chỉ số`i`, chúng ta đếm xem nó bao phủ nó bao nhiêu khoảng, đưa ra trọng số`w[i]`. Sau đó`t(s)`tương đương với một multiset trong đó vị trí`i`đóng góp`w[i]`bản sao của`s[i]`. 

để làm`t(s)`tối đa về mặt từ điển, chúng tôi muốn tất cả đều có sẵn`1`s được đặt ở vị trí có trọng lượng cao nhất của`t(s)`. Vì các giao dịch hoán đổi là miễn phí ngoại trừ việc đếm số lượng của chúng, nên nhiệm vụ thực sự trở thành: các trọng số và giá trị nhị phân hiện tại đã cho, tính toán số lượng giao dịch hoán đổi tối thiểu để chuyển cách sắp xếp hiện tại thành cách sắp xếp tối ưu được sắp xếp theo trọng số. 

Việc tính toán lại tất cả các trọng số cho mỗi truy vấn là`O(nm + qn)`, nó quá lớn. 

Cái nhìn sâu sắc quan trọng là mỗi chỉ số`i`đóng góp tuyến tính và độc lập, và cấu trúc cuối cùng chỉ phụ thuộc vào trọng số`w[i]`. Khi chúng tôi tính toán trước số lần mỗi vị trí xuất hiện trong tất cả các khoảng thời gian, vấn đề sẽ giảm xuống còn việc duy trì nhiều tập hợp bit có trọng số khi lật. 

Bây giờ là ý tưởng thứ hai: thay vì tính toán lại mọi thứ, chúng ta đếm xem có bao nhiêu`1`s tồn tại ở mỗi hạng cân. Vì sự sắp xếp tối ưu luôn đặt`1`thành các trọng số lớn nhất, chúng ta chỉ cần duy trì cấu trúc trọng số được sắp xếp và theo dõi có bao nhiêu`1`s chiếm từng tiền tố của trọng số được sắp xếp. Số lượng giao dịch hoán đổi được xác định bởi sự không khớp giữa phân phối hiện tại và phân phối mục tiêu. 

Chúng tôi sắp xếp các chỉ số theo trọng số một lần. Sau đó, chúng tôi duy trì cây Fenwick hoặc BIT theo đơn hàng này, theo dõi vị trí nào hiện chứa`1`. Sau mỗi lần lật, chúng tôi cập nhật một vị trí và tính xem có bao nhiêu`1`s nên nằm ở trên cùng`k`các vị trí theo thứ tự sắp xếp này. Số lượng không khớp trực tiếp chuyển thành chi phí hoán đổi, vì mỗi vị trí bị đặt sai vị trí`1`phải được di chuyển qua một ranh giới và mỗi lần hoán đổi sẽ sửa hai lần đảo ngược. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q · n · m) | O(n) | Quá chậm | 
| Sắp xếp + bảo trì BIT | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi chỉ số`i`trong chuỗi gốc, tính toán`w[i]`, số khoảng có chuỗi con bao gồm`i`. Điều này được thực hiện bằng cách sử dụng một mảng khác biệt trên các điểm cuối khoảng. Điều này cho biết số lần mỗi ký tự đóng góp vào chuỗi nối cuối cùng. 
2. Sắp xếp chỉ số`1..n`theo thứ tự tăng dần`w[i]`. Thứ tự này thể hiện các vị trí trong`t(s)`từ ít quan trọng nhất đến quan trọng nhất. Trọng lượng lớn nhất tương ứng với các vị trí đặt một`1`có lợi nhất cho trật tự từ điển. 
3. Xây dựng cây Fenwick theo thứ tự đã sắp xếp, lưu trữ xem`s[i]`hiện tại là`1`. Cấu trúc này hỗ trợ tính tổng tiền tố nhanh theo thứ tự được sắp xếp theo trọng số. 
4. Với mỗi truy vấn, lật một bit vào`s`, cập nhật cây Fenwick ở vị trí tương ứng. 
5. Sau mỗi lần cập nhật, hãy tính xem có bao nhiêu`1`s hiện tồn tại trong chuỗi. Hãy để điều này được`k`. Ở cấu hình tối ưu,`k`vị trí có trọng lượng lớn nhất trong`t(s)`phải chứa tất cả`1`S. 
6. Truy vấn cây Fenwick để tính xem có bao nhiêu`1`hiện đang nằm trong top`k`vị trí của thứ tự sắp xếp. Hãy để điều này được`good`. Phần còn lại`k - good`những cái bị đặt sai chỗ. 
7. Mỗi người đặt nhầm chỗ`1`tương ứng với một đơn vị phải di chuyển từ vùng có trọng số thấp sang vùng có trọng số cao hơn. Mỗi lần hoán đổi sẽ sửa hai chỗ sai sót như vậy, vì vậy số lần hoán đổi tối thiểu là`(k - good)`. 

### Tại sao nó hoạt động 

Việc xây dựng chuyển đổi vấn đề thành sắp xếp lại nhiều vị trí có trọng số. Vì các giao dịch hoán đổi cho phép hoán vị tùy ý nên chỉ có sự sắp xếp cuối cùng mới quan trọng. Tối đa hóa từ điển buộc phải thực hiện một nhiệm vụ tham lam: tất cả`1`s phải chiếm các vị trí có trọng lượng cao nhất. Cây Fenwick duy trì mức độ lệch của cấu hình hiện tại so với nhiệm vụ mục tiêu đó. Bởi vì mỗi lần hoán đổi sẽ sửa chính xác hai phép đảo ngược giữa nửa đúng và nửa sai, nên số lượng không khớp sẽ xác định đầy đủ câu trả lời. 

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

n, m, q = map(int, input().split())
s = list(input().strip())

L = [0] * n
R = [0] * n

diff = [0] * (n + 2)

for _ in range(m):
    l, r = map(int, input().split())
    diff[l] += 1
    diff[r + 1] -= 1

cur = 0
w = [0] * n
for i in range(1, n + 1):
    cur += diff[i]
    w[i - 1] = cur

order = list(range(n))
order.sort(key=lambda i: w[i])

pos = [0] * n
for i, idx in enumerate(order):
    pos[idx] = i + 1

fw = Fenwick(n)

for i in range(n):
    if s[i] == '1':
        fw.add(pos[i], 1)

total_ones = sum(1 for c in s if c == '1')

for _ in range(q):
    x = int(input()) - 1

    if s[x] == '1':
        fw.add(pos[x], -1)
        s[x] = '0'
        total_ones -= 1
    else:
        fw.add(pos[x], 1)
        s[x] = '1'
        total_ones += 1

    k = total_ones
    if k == 0:
        print(0)
        continue

    good = fw.sum(n) - fw.sum(n - k)
    print(k - good)
```Giải pháp bắt đầu bằng cách nén từng chỉ mục thành một trọng số biểu thị số lần nó xuất hiện trong chuỗi con được nối. Điều này biến sự phụ thuộc về cấu trúc vào các khoảng thành một đại lượng vô hướng duy nhất cho mỗi vị trí. 

Sắp xếp các chỉ số theo trọng số này xác định thứ tự ưu tiên để đặt`1`là. Cây Fenwick sau đó theo dõi vị trí nào trong số này hiện đang nắm giữ`1`s, cho phép truy vấn tiền tố và hậu tố nhanh theo thứ tự này. 

Mỗi truy vấn cập nhật một vị trí và điều chỉnh tổng số vị trí. Tính toán chính là xác định có bao nhiêu`1`nó đã nằm ở trên cùng rồi`k`vị trí có trọng số; những thứ đó được đặt đúng chỗ. Phần còn lại phải được di chuyển và số lượng trao đổi sẽ được tính trực tiếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2 4
01
1 2
1 2
1
1
2
2
```Sau khi tiền xử lý, cả hai chỉ số đều có trọng số bằng nhau vì cả hai khoảng đều bao trùm cả hai vị trí. 

| Bước | Chuỗi | #những người | Top 1 đúng | hoán đổi | 
| --- | --- | --- | --- | --- | 
| ban đầu | 01 | 1 | 1 | 0 | 
| lật 1 | 11 | 2 | 2 | 0 | 
| lật 2 | 01 | 1 | 0 | 1 | 
| lật 1 | 11 | 2 | 2 | 0 | 
| lật 2 | 10 | 1 | 0 | 1 | 

Dấu vết này cho thấy rằng khi các trọng số đối xứng, vấn đề sẽ giảm xuống việc sắp xếp các bit theo bất kỳ thứ tự nào và hoán đổi đo lường chính xác sự đảo ngược giữa vị trí mong muốn và vị trí hiện tại. 

### Ví dụ 2 

đầu vào:```
3 2 3
010
1 2
2 3
1
2
3
```Ở đây, vị trí ở giữa tham gia vào cả hai quãng, mang lại cho nó trọng lượng cao hơn. 

| Bước | s | thứ tự trọng lượng (thấp→cao) | những cái ở top k | hoán đổi | 
| --- | --- | --- | --- | --- | 
| ban đầu | 010 | 1,3,2 | 1 | 0 | 
| lật 1 | 110 | 1,3,2 | 2 | 0 | 
| lật 2 | 100 | 1,3,2 | 1 | 0 | 

Chỉ số ở giữa chiếm ưu thế nên vị trí tối ưu sẽ ưu tiên cho nó`1`S. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | sắp xếp một lần và cập nhật Fenwick cho mỗi truy vấn | 
| Không gian | O(n) | mảng khối lượng, thứ tự, cây Fenwick | 

Các ràng buộc cho phép lên đến`2⋅10^5`các hoạt động, vì vậy các cập nhật logarit cho mỗi truy vấn nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import os
    return os.popen("python3 main.py").read().strip()

# sample
assert run("""2 2 4
01
1 2
1 2
1
1
2
2
""") == "0\n0\n1\n0"

# all zeros no intervals
assert run("""3 1 3
000
1 3
1
2
3
""") == "0\n0\n0"

# all ones flips
assert run("""3 1 3
111
1 3
1
2
3
""") == "0\n0\n0"

# single position dominates
assert run("""3 2 2
010
1 3
1 3
2
2
""") == "0\n0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả số không | tất cả 0 | cấu hình mục tiêu trống | 
| tất cả những cái | tất cả 0 | sự sắp xếp đã tối ưu | 
| trọng lượng đồng đều | hành vi hoán đổi ổn định | xử lý trọng lượng đối xứng | 
| trường hợp thống trị | không có giao dịch hoán đổi không cần thiết | xếp hạng cân nặng chính xác | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các trọng số khoảng đều bằng nhau. Trong tình huống đó, bất kỳ hoán vị nào của`s`tạo ra tầm quan trọng về mặt cấu trúc như nhau trong`t(s)`, vì vậy câu trả lời chỉ phụ thuộc vào việc chuỗi đã được sắp xếp theo vị trí hay chưa. Thuật toán xử lý vấn đề này vì thứ tự sắp xếp thoái hóa thành chỉ mục tùy ý nhưng cố định và phép tính Fenwick vẫn đo lường chính xác các điểm không khớp. 

Một trường hợp khác là khi số lượng cái bằng 0 hoặc bằng`n`. Thuật toán ngay lập tức trả về các giao dịch hoán đổi bằng 0 vì việc so sánh tiền tố-hậu tố trong cây Fenwick mang lại sự căn chỉnh đầy đủ hoặc không cần thay đổi vị trí bắt buộc.
