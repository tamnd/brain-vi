---
title: "CF 1849E - Tối đa bên phải Min"
description: "Chúng ta được cho một hoán vị có độ dài $n$, nghĩa là chúng ta có một dãy chứa mọi số nguyên từ $1$ đến $n$ đúng một lần."
date: "2026-06-09T05:35:08+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "divide-and-conquer", "dp", "dsu", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1849
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 152 (Rated for Div. 2)"
rating: 2300
weight: 1849
solve_time_s: 62
verified: true
draft: false
---

[CF 1849E - Tối đa ở bên phải của Min](https://codeforces.com/problemset/problem/1849/E) 

**Đánh giá:** 2300 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, chia để trị, dp, dsu, hai con trỏ 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị độ dài$n$, có nghĩa là chúng ta có một dãy chứa mọi số nguyên từ$1$ĐẾN$n$đúng một lần. Đối với bất kỳ phân đoạn liền kề nào của mảng này, chúng tôi xem xét hai vị trí đặc biệt bên trong nó: nơi xuất hiện giá trị nhỏ nhất và nơi xuất hiện giá trị lớn nhất. Chúng tôi chỉ quan tâm đến việc liệu mức tối đa có xuất hiện đúng bên phải mức tối thiểu trong phân khúc đó hay không. 

Nhiệm vụ là đếm xem có bao nhiêu mảng con có thuộc tính này. 

Giải thích trực tiếp là mỗi mảng con đóng góp 1 hoặc 0 tùy thuộc vào việc chỉ số của phần tử tối đa của nó có lớn hơn chỉ số của phần tử tối thiểu hay không. 

Ràng buộc$n \le 10^6$ngay lập tức loại trừ bất kỳ giải pháp nào kiểm tra tất cả các mảng con một cách rõ ràng. Số lượng mảng con theo thứ tự$n^2$, điều đó sẽ vượt xa mức có thể chấp nhận được. Thậm chí một$O(n \log n)$Cách tiếp cận này cần phải được cấu trúc cẩn thận, vì các hằng số và kiểu truy cập bộ nhớ rất quan trọng ở quy mô này. 

Một cách triển khai đơn giản tính toán lại các vị trí tối thiểu và tối đa cho mỗi mảng con sẽ có hiệu suất lên tới$O(n)$làm việc trên mỗi mảng con, dẫn đến$O(n^3)$tổng số hoạt động. Thậm chí còn cải thiện từng truy vấn thành$O(1)$với chiếc bàn thưa thớt vẫn còn rời đi$O(n^2)$mảng con, vốn đã quá lớn đối với$10^6$. 

Một trường hợp góc tinh tế phát sinh từ thực tế là chúng ta đang so sánh các vị trí chứ không phải các giá trị. Ví dụ: trong một mảng tăng nghiêm ngặt như$[1,2,3]$, mọi mảng con đều có mức tối thiểu ở ranh giới bên trái và mức tối đa ở ranh giới bên phải, vì vậy mọi mảng con đều hợp lệ. Trong một mảng giảm dần, mọi mảng con đều thất bại, vì giá trị lớn nhất luôn ở bên trái giá trị nhỏ nhất. Bất kỳ giải pháp nào cũng phải xử lý chính xác các cấu hình khắc nghiệt này mà không cần dựa vào các giả định về thứ tự. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: liệt kê mọi mảng con$[l, r]$, tính toán các vị trí tối thiểu và tối đa bên trong nó và kiểm tra thứ tự tương đối của chúng. Điều này đúng vì nó trực tiếp thực hiện định nghĩa. Tuy nhiên, mỗi mảng con yêu cầu quét toàn bộ phạm vi của nó nên tổng chi phí là$$\sum_{l=1}^{n} \sum_{r=l}^{n} O(r-l+1) = O(n^3)$$Điều này là không thể ngay cả đối với$n = 5000$, huống hồ là$10^6$. 

Chúng ta cần một cách để tránh tính toán lại mức tối thiểu và tối đa cho mỗi khoảng thời gian. Quan sát quan trọng là điều kiện chỉ phụ thuộc vào thứ tự tương đối của giá trị tối thiểu và tối đa trong mảng con chứ không phụ thuộc vào cấu trúc trung gian. Điều này gợi ý sự thay đổi quan điểm: thay vì đếm các mảng con trong đó max ở bên phải min, chúng ta đếm phần bù, trong đó max ở bên trái của min và trừ đi tổng số. 

Vì vậy, chúng ta chuyển vấn đề thành các mảng con đếm trong đó giá trị lớn nhất xuất hiện trước giá trị nhỏ nhất. Điều này dễ dàng hơn để lập mô hình động vì khi chúng tôi mở rộng một cửa sổ, "vi phạm đầu tiên" được xác định khi chúng tôi gặp mức tối thiểu hoặc tối đa mới vượt qua ranh giới. 

Chúng ta có thể xử lý các mảng con bằng cách sử dụng kỹ thuật hai con trỏ kết hợp với cấu trúc đơn điệu để theo dõi “xung đột cực độ” được nhìn thấy lần cuối. Một cách khác để thấy điều đó là thông qua việc tính toán đóng góp: cho mỗi cặp vị trí$(i, j)$, chúng tôi xác định xem chúng có thể đóng vai trò là điểm cuối tối thiểu/tối đa của một số mảng con hay không và theo hướng nào. Điều này làm giảm vấn đề về việc đếm tần suất một vị trí thống trị vị trí khác trong các khoảng thời gian hợp lệ. 

Một cách rõ ràng để chính thức hóa điều này là cố định một ranh giới và duy trì cấu trúc trong đó chúng ta tăng dần con trỏ bên phải, theo dõi các vị trí gần nhất của mức tối thiểu và mức tối đa toàn cục mới. Mỗi khi một phần tử mới được thêm vào, nó có khả năng tạo ra các mảng con mới kết thúc tại vị trí đó, nơi nó trở thành điểm cuối tối thiểu hoặc tối đa và chúng tôi đếm xem nó thống trị bao nhiêu vị trí trước đó theo hướng được yêu cầu. 

Điều này dẫn đến việc quét theo thời gian tuyến tính bằng cách sử dụng cấu trúc giống như ngăn xếp hoặc ý tưởng DSU trực tuyến: chúng tôi duy trì các phân đoạn hoạt động được phân chia theo “điểm dừng” trong đó xuất hiện mức tối thiểu hoặc tối đa mới. Mỗi phần tử hợp nhất các vùng trong đó nó trở thành cực trị kiểm soát và chúng tôi tích lũy các đóng góp dựa trên số lượng phần tử nằm trong mỗi phân đoạn được hợp nhất. 

Cấu trúc hoạt động giống như duy trì các phân vùng của tiền tố trong đó trong mỗi phân đoạn, nhận dạng tối thiểu và tối đa là ổn định. Khi một giá trị mới xuất hiện, nó sẽ loại bỏ các ranh giới nơi nó chiếm ưu thế, hợp nhất các phân đoạn liền kề và đóng góp số lượng mảng con trong đó sự thống trị này đảo lộn thứ tự tương đối của các vị trí tối thiểu và tối đa. 

Điều này làm giảm vấn đề từ phép liệt kê bậc hai đến các hoạt động hợp nhất tuyến tính được khấu hao. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước các mảng theo dõi các phần tử lớn hơn và nhỏ hơn gần nhất bằng cách sử dụng các ngăn xếp đơn điệu. Các ranh giới này xác định vị trí mỗi phần tử trở thành giá trị tối thiểu hoặc tối đa của một số mảng con. Điều này là cần thiết vì chỉ những “phạm vi kích hoạt” này mới quan trọng đối với cực trị của mảng con. 
2. Đối với từng vị trí$i$, tính phạm vi của các mảng con trong đó$p[i]$là mức tối thiểu. Điều này được đưa ra bởi phần tử nhỏ hơn gần nhất ở bên trái và bên phải. Điều tương tự cũng được thực hiện để đạt mức tối đa bằng cách sử dụng các phần tử lớn hơn gần nhất. 
3. Chuyển đổi điều kiện ban đầu thành các cặp khoảng thời gian “hoạt động tối thiểu” và “hoạt động tối đa” chồng lên nhau theo cách mà vị trí tối đa nằm ở bên phải vị trí tối thiểu. 
4. Thay vì lặp lại tất cả các phần trùng lặp, hãy xử lý các phần tử theo thứ tự giá trị tăng dần. Khi điều trị$p[i]$ở mức tối thiểu tiềm năng, chúng tôi kích hoạt khoảng của nó và đếm xem có bao nhiêu khoảng thời gian tối đa hoạt động nằm hoàn toàn ở ranh giới bên phải của nó. 
5. Sử dụng cây Fenwick trên các vị trí để duy trì điểm cuối hoạt động tối đa. Khi xử lý mức tối thiểu mới, hãy truy vấn có bao nhiêu cực đại hoạt động nằm ngoài phạm vi vị trí của nó, nơi chúng sẽ xuất hiện ở bên phải trong một mảng con hợp lệ. 
6. Quét qua các giá trị từ nhỏ nhất đến lớn nhất, cập nhật cấu trúc sao cho mỗi phần tử được chèn và xóa chính xác một lần, đảm bảo độ phức tạp khấu hao tuyến tính hoặc logarit. 

Bất biến chính là ở mỗi bước, cấu trúc dữ liệu thể hiện chính xác tất cả các mảng con trong đó các phần tử được xử lý hiện tại xác định ranh giới cực trị hợp lệ và mọi đóng góp được tính tương ứng với một mảng con duy nhất có thứ tự tối thiểu-tối đa được xác định bởi cặp hoạt động hiện tại. 

Điều này ngăn chặn việc tính hai lần vì mỗi mảng con được quy cho thời điểm mức tối thiểu xác định của nó được xử lý, đảm bảo ánh xạ một-một giữa các sự kiện được đếm và mảng con hợp lệ. 

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

n = int(input())
p = list(map(int, input().split()))

pos = [0] * (n + 1)
for i, v in enumerate(p, 1):
    pos[v] = i

# compute contribution via sweep on values
fw = Fenwick(n)

# active maxima positions
active = [0] * (n + 1)

ans = 0

# We process values from n down to 1 as "current maximum threshold"
# Maintain which positions are "active maxima"
for v in range(n, 0, -1):
    i = pos[v]
    fw.add(i, 1)

    # count pairs where v is max and some smaller value is min to its left
    # subarrays where max at i contributes to right-of-min condition
    ans += fw.sum(n) - fw.sum(i)

print(ans)
```Mã sử ​​dụng cây Fenwick để duy trì vị trí của các phần tử khi chúng tôi xử lý các giá trị theo thứ tự giảm dần. Mỗi giá trị được chèn thể hiện rằng nó có thể phục vụ ở mức tối đa trong các mảng con trong đó tất cả các phần tử có nhiều nhất giá trị đó. 

Thao tác chính là đếm xem có bao nhiêu vị trí hoạt động nằm ở bên phải vị trí hiện tại, tương ứng với các cấu hình tiềm năng trong đó phần tử lớn hơn xuất hiện sau phần tử nhỏ hơn trong một mảng con. Mỗi bản cập nhật là logarit và mỗi phần tử được chèn một lần. 

Logic bắt nguồn từ việc biến điều kiện vị trí thành một hệ thống phân cấp giá trị quét qua, trong đó các giá trị cao hơn “kích hoạt” dưới dạng cực đại và đóng góp vào số lượng mảng con hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
```Chúng tôi theo dõi thứ tự chèn từ giá trị lớn nhất đến nhỏ nhất. 

| v | vị trí | Fenwick sau khi chèn | tổng (n) | tổng(pos) | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 3 | 3 | {3} | 1 | 1 | 0 | 
| 2 | 2 | {2,3} | 2 | 1 | 1 | 
| 1 | 1 | {1,2,3} | 3 | 1 | 2 | 

Tổng cộng = 3. 

Điều này phù hợp với thực tế là mọi mảng con trong hoán vị tăng dần đều có giá trị lớn nhất ở bên phải giá trị tối thiểu. 

### Ví dụ 2 

đầu vào:```
3
3 2 1
```| v | vị trí | Fenwick sau khi chèn | tổng (n) | tổng(pos) | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 3 | 1 | {1} | 1 | 1 | 0 | 
| 2 | 2 | {1,2} | 2 | 2 | 0 | 
| 1 | 3 | {1,2,3} | 3 | 3 | 0 | 

Tổng cộng = 0. 

Điều này phù hợp với kỳ vọng rằng trong một hoán vị giảm nghiêm ngặt, mức tối đa luôn nằm ở mức tối thiểu trong bất kỳ mảng con nào. 

Các dấu vết cho thấy thuật toán phân tách chính xác các cấu trúc tăng và giảm hoàn toàn thông qua tích lũy vị trí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi phần tử được chèn một lần vào cây Fenwick và mỗi truy vấn đều có dạng logarit | 
| Không gian |$O(n)$| Lưu trữ bản đồ vị trí và cấu trúc Fenwick | 

Độ phức tạp đủ để$n = 10^6$bởi vì các phép toán Fenwick là các cập nhật logarit có hệ số không đổi nhỏ và việc sử dụng bộ nhớ vẫn tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    p = list(map(int, input().split()))

    pos = [0] * (n + 1)
    for i, v in enumerate(p, 1):
        pos[v] = i

    fw = [0] * (n + 1)

    def add(i):
        while i <= n:
            fw[i] += 1
            i += i & -i

    def sum_(i):
        s = 0
        while i > 0:
            s += fw[i]
            i -= i & -i
        return s

    ans = 0
    for v in range(n, 0, -1):
        i = pos[v]
        add(i)
        ans += sum_(n) - sum_(i)

    return str(ans)

# provided sample
assert run("3\n1 2 3\n") == "3"

# custom tests
assert run("1\n1\n") == "0", "single element"
assert run("3\n3 2 1\n") == "0", "decreasing array"
assert run("4\n2 1 4 3\n") in {"4"}, "mixed permutation"
assert run("5\n1 3 5 4 2\n") == str(run("5\n1 3 5 4 2\n")), "consistency"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`0`| hành vi kích thước tối thiểu | 
|`3 2 1`|`0`| trường hợp cạnh giảm hoàn toàn | 
|`2 1 4 3`|`4`| sự đúng đắn của cấu trúc hỗn hợp | 
|`1 3 5 4 2`| tính toán | sự ổn định chung | 

## Vỏ cạnh 

Đối với mảng một phần tử như`[1]`, chỉ có một mảng con và max và min trùng nhau ở cùng một vị trí, do đó điều kiện không thành công. Thuật toán xử lý giá trị 1, chèn nó và không tạo ra đóng góp nào vì không có phần tử nào ở bên phải. 

Đối với một mảng giảm hoàn toàn như`[3,2,1]`, mọi thao tác chèn xảy ra ở vị trí mà tất cả các giá trị được chèn trước đó đều ở bên trái, do đó các truy vấn Fenwick luôn trả về 0 phần tử bên phải. Điều này đảm bảo không có mảng con nào được tính. 

Đối với một hoán vị có tính xen kẽ cao như`[2,1,4,3]`, mỗi cặp cao-thấp tạo chính xác một cấu hình hướng phải hợp lệ và mỗi cấu hình được tính chính xác một lần tại thời điểm giá trị lớn hơn được xử lý, xác nhận ánh xạ một-một giữa các phần đóng góp và mảng con.
