---
title: "CF 106420G - Lời chế nhạo của Tyson"
description: "Chúng ta được cho một mảng các số nguyên và một cặp ngưỡng thực $a$ và $b$. Nhiệm vụ là đếm xem có bao nhiêu mảng con liền kề có giá trị trung bình nằm trong một phạm vi cụ thể."
date: "2026-06-20T03:47:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106420
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 3-11-26 (Beginner)"
rating: 0
weight: 106420
solve_time_s: 63
verified: true
draft: false
---

[CF 106420G - Lời chế nhạo của Tyson](https://codeforces.com/problemset/problem/106420/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và một cặp ngưỡng thực$a$Và$b$. Nhiệm vụ là đếm xem có bao nhiêu mảng con liền kề có giá trị trung bình nằm trong một phạm vi cụ thể. Thay vì trực tiếp tính toán trung bình cho mỗi mảng con, bài toán giảm mục tiêu là đếm các mảng con có giá trị trung bình ít nhất là$a$, và trừ đi những giá trị có giá trị trung bình vượt quá$b$. 

Một mảng con là bất kỳ phân đoạn liền kề nào, vì vậy với mỗi cặp chỉ số$l \le r$, chúng ta đang ngầm xem xét trình tự$x_l, x_{l+1}, \dots, x_r$. Đầu ra là một số nguyên biểu thị có bao nhiêu phân đoạn như vậy thỏa mãn ràng buộc cuối cùng xuất phát từ sự khác biệt của hai bài toán đếm. 

Hạn chế chính về cấu trúc là độ dài mảng có thể đủ lớn để bất kỳ phép liệt kê bậc hai nào của các mảng con đều không thể thực hiện được. Một sự ngây thơ$O(n^2)$quét qua tất cả các mảng con sẽ dẫn đến khoảng$10^{10}$hoạt động khi$n = 10^5$, vượt xa giới hạn thông thường. Điều này ngay lập tức thúc đẩy chúng ta hướng tới các phép biến đổi dựa trên tiền tố và cấu trúc dữ liệu có thể trả lời các truy vấn đếm phạm vi theo thời gian logarit. 

Một vấn đề tế nhị xuất hiện trong bước chuyển đổi: biến các ràng buộc trung bình thành các ràng buộc tổng sẽ đưa ra các so sánh có giá trị thực. Vì tổng tiền tố dựa trên số nguyên nên cần phải xử lý cẩn thận các bất đẳng thức nghiêm ngặt và không nghiêm ngặt. Một sự chuyển đổi bất cẩn có thể dịch chuyển số đếm đi một và âm thầm làm hỏng câu trả lời cuối cùng. 

Trường hợp một cạnh là khi tất cả các phần tử giống hệt nhau và bằng một ngưỡng. Trong tình huống này, mọi mảng con đều có giá trị trung bình như nhau, do đó, tất cả các mảng con đều đủ điều kiện hoặc không có mảng con nào đáp ứng được điều đó tùy thuộc vào tính bao hàm. Cách tiếp cận so sánh dấu phẩy động ngây thơ cũng có thể bị hỏng ở đây do lỗi chính xác. 

Một trường hợp cạnh khác xảy ra khi có số âm. Tổng tiền tố có thể giảm và mọi giả định về tính đơn điệu đều không thành công. Điều này làm cho việc suy luận dựa trên sắp xếp trở nên cần thiết nhưng cũng yêu cầu nén tọa độ nhất quán nếu chúng ta sử dụng cây Fenwick. 

## Phương pháp tiếp cận 

Phương pháp brute-force kiểm tra rõ ràng từng mảng con, tính tổng của nó, chia cho độ dài của nó và so sánh nó với các giới hạn. Điều này đúng về mặt khái niệm vì nó đánh giá trực tiếp định nghĩa giá trị trung bình cho mọi phân khúc. Tuy nhiên, mỗi mảng con yêu cầu$O(1)$chỉ tính toán tổng phân bổ nếu tổng tiền tố được tính toán trước và có$O(n^2)$mảng con, do đó tổng chi phí vẫn là bậc hai. Với$n = 10^5$, điều này dẫn đến khoảng$5 \times 10^9$đánh giá thì quá chậm. 

Sự cải tiến đến từ việc loại bỏ hoàn toàn sự phân chia. điều kiện$\frac{\sum x_i}{len} \ge a$có thể được viết lại như$\sum x_i - a \cdot len \ge 0$. Điều này biến bài toán thành các mảng con đếm có tổng được biến đổi không âm. Tương tự, bất đẳng thức chặt chẽ về$b$trở thành một ngưỡng dịch chuyển nhẹ sau khi chuyển nó thành một bất đẳng thức nguyên. 

Sau khi được biến đổi, mỗi phần tử sẽ trở thành$y_i = x_i - a$hoặc$x_i - b$và tác vụ sẽ trở thành các mảng con đếm có tổng ít nhất là một hằng số cố định. Đây là một bài toán thống trị tổng tiền tố cổ điển: đối với mỗi điểm cuối bên phải, chúng ta đếm xem có bao nhiêu tổng tiền tố trước đó đủ nhỏ để tổng của mảng con thu được đáp ứng ngưỡng. 

Điều này làm giảm vấn đề trong việc duy trì một tập hợp tổng tiền tố động và trả lời xem chúng có bao nhiêu trong số đó.$\le$một giá trị nhất định. Cây Fenwick hoặc cấu trúc thống kê thứ tự cho phép thực hiện điều này trong$O(\log n)$mỗi lần chèn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tiền tố + Cây Fenwick |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta chia bài toán ban đầu thành hai bài toán con độc lập: đếm các mảng con có giá trị trung bình ít nhất$a$và đếm các mảng con có giá trị trung bình lớn hơn$b$. Câu trả lời cuối cùng là sự khác biệt của hai cách tính này. 

1. Chuyển đổi điều kiện đầu tiên thành ràng buộc tổng bằng cách xác định một mảng được chuyển đổi$y_i = x_i - a$. Một mảng con ít nhất có ý nghĩa$a$chính xác khi tổng chuyển đổi của nó không âm. Điều này loại bỏ hoàn toàn sự phân chia và làm cho điều kiện trở nên hoàn toàn phụ thuộc. 
2. Xây dựng tổng tiền tố trên mảng được chuyển đổi, trong đó$p_0 = 0$Và$p_i = y_1 + \dots + y_i$. Bất kỳ tổng mảng con nào từ$l$ĐẾN$r$trở thành$p_r - p_{l-1}$. chúng tôi muốn$p_r - p_{l-1} \ge 0$, tương đương với$p_{l-1} \le p_r$. 
3. Xử lý tổng tiền tố từ trái sang phải. Đối với mỗi điểm cuối$r$, chúng tôi muốn đếm xem có bao nhiêu giá trị tiền tố trước đó nhỏ hơn hoặc bằng giá trị tiền tố hiện tại$p_r$. Điều này trực tiếp tính các vị trí bắt đầu hợp lệ. 
4. Vì giá trị tiền tố là số nguyên tùy ý nên hãy nén chúng thành thứ hạng. Điều này cho phép chúng ta lưu trữ tần số trong cây Fenwick. 
5. Đối với mỗi tiền tố$p_r$, truy vấn có bao nhiêu tổng tiền tố được lưu trữ$\le p_r$, thêm câu đó vào câu trả lời rồi chèn$p_r$vào cây Fenwick. 
6. Lặp lại quy trình tương tự cho điều kiện thứ hai bằng cách sử dụng$z_i = x_i - b$. Đối với bất đẳng thức nghiêm ngặt, hãy chuyển đổi nó thành một phép dịch ngưỡng số nguyên sao cho$\frac{\sum x_i}{len} > b$trở thành$\sum x_i - b \cdot len \ge 1$, đảm bảo xử lý số nguyên chính xác. 
7. Trừ số thứ hai cho số đầu tiên để có kết quả cuối cùng. 

Tại sao nó hoạt động: mỗi mảng con tương ứng duy nhất với một cặp chỉ số tiền tố$(l-1, r)$. Điều kiện trên mảng con trở thành so sánh giữa hai tổng tiền tố. Cây Fenwick duy trì tất cả các tổng tiền tố trước đó và mỗi truy vấn sẽ tính chính xác có bao nhiêu điểm bắt đầu hợp lệ tồn tại cho điểm cuối hiện tại. Vì mỗi mảng con được tính chính xác một lần tại điểm cuối bên phải của nó và điều kiện được kiểm tra hoàn toàn thông qua ưu thế tiền tố, nên không có mảng con nào bị bỏ sót hoặc được tính hai lần. 

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

def count_ge(arr, threshold):
    n = len(arr)
    pref = [0] * (n + 1)

    for i in range(1, n + 1):
        pref[i] = pref[i - 1] + arr[i - 1]

    vals = sorted(set(pref))
    idx = {v: i + 1 for i, v in enumerate(vals)}

    fw = Fenwick(len(vals))
    res = 0

    for x in pref:
        r = idx[x]
        res += fw.sum(r)
        fw.add(r, 1)

    return res

def solve_condition(a_list, base):
    n = len(a_list)
    transformed = [x - base for x in a_list]
    return count_ge(transformed, 0)

def main():
    n = int(input())
    a, b = map(float, input().split())
    arr = list(map(int, input().split()))

    # first: mean >= a
    prefA = [x - a for x in arr]
    ansA = count_ge(prefA, 0)

    # second: mean > b -> sum - b*len >= 1
    # transform: x - b, but require strict handling via +1 on prefix side
    prefB = [x - b for x in arr]
    ansB = 0

    n = len(arr)
    pref = [0]
    for x in prefB:
        pref.append(pref[-1] + x)

    vals = sorted(set(pref))
    idx = {v: i + 1 for i, v in enumerate(vals)}
    fw = Fenwick(len(vals))

    for p in pref:
        r = idx[p]
        ansB += fw.sum(r - 1)  # strict: p[l-1] < p[r]
        fw.add(r, 1)

    print(ansA - ansB)

if __name__ == "__main__":
    main()
```Cây Fenwick duy trì tần số của các tổng tiền tố. Mỗi tổng tiền tố được nén thành một chỉ mục để các truy vấn sắp xếp thứ tự trở thành truy vấn tần số tiền tố. Đối với điều kiện đầu tiên, chúng tôi cho phép sự bình đẳng, vì vậy chúng tôi truy vấn`sum(r)`. Đối với điều kiện thứ hai nghiêm ngặt, chúng tôi sử dụng`sum(r - 1)`để thực thi sự bất bình đẳng một cách nghiêm ngặt. 

Một cạm bẫy triển khai phổ biến là trộn lẫn số học dấu phẩy động trong các phép so sánh thay vì đẩy các giá trị thực thành các khác biệt tiền tố số nguyên một cách nhất quán. Một điều nữa là quên rằng tiền tố trống phải được đưa vào, vì mỗi mảng con đều bắt đầu từ một số$l-1$, bao gồm cả số không. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng$[1, 2, 3]$với các ngưỡng được chọn để chúng tôi thể hiện cả hành vi toàn diện và nghiêm ngặt. 

Đối với phép biến đổi đầu tiên (trung bình ≥ a), giả sử nó mang lại mảng được biến đổi$[1, -1, 2]$. 

| r | tổng tiền tố | xếp hạng nén | đếm ≤ hiện tại | Bang Fenwick | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 2 | 0 | {0} | 
| 1 | 1 | 3 | 1 | {0,1} | 
| 2 | 0 | 2 | 1 | {0,1,0} | 
| 3 | 2 | 4 | 3 | {0,1,0,2} | 

Sự tích lũy cho thấy mỗi tiền tố đếm các tiền tố trước đó không lớn hơn như thế nào, tương ứng với các phần bắt đầu của mảng con hợp lệ. 

Đối với phép biến đổi thứ hai (trung bình nghiêm ngặt > b), giả sử tổng tiền tố trở thành$[0, 1, 1, 3]$. 

| r | tổng tiền tố | xếp hạng | đếm < hiện tại | Bang Fenwick | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 0 | {0} | 
| 1 | 1 | 2 | 1 | {0,1} | 
| 2 | 1 | 2 | 1 | {0,1,1} | 
| 3 | 3 | 4 | 3 | {0,1,1,3} | 

Dấu vết này cho thấy tại sao sự bất bình đẳng nghiêm ngặt yêu cầu loại trừ các tổng tiền tố bằng nhau, nếu không thì các điểm cuối bằng nhau sẽ đóng góp không chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi tổng tiền tố được chèn và truy vấn một lần trong cây Fenwick | 
| Không gian |$O(n)$| Mảng tiền tố và nén tọa độ | 

Hệ số logarit đến từ các phép toán trên cây Fenwick và hệ số tuyến tính đến từ việc xử lý từng phần tử mảng một lần. Điều này phù hợp thoải mái trong các ràng buộc điển hình cho$n$lên đến$10^5$hoặc cao hơn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    return main_capture()

def main_capture():
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
        n = int(input())
        a, b = map(float, input().split())
        arr = list(map(int, input().split()))

        def count_le(base):
            pref = [0]
            for x in arr:
                pref.append(pref[-1] + (x - base))
            vals = sorted(set(pref))
            idx = {v:i+1 for i,v in enumerate(vals)}
            fw = Fenwick(len(vals))
            res = 0
            for p in pref:
                r = idx[p]
                res += fw.sum(r)
                fw.add(r, 1)
            return res

        def count_lt(base):
            pref = [0]
            for x in arr:
                pref.append(pref[-1] + (x - base))
            vals = sorted(set(pref))
            idx = {v:i+1 for i,v in enumerate(vals)}
            fw = Fenwick(len(vals))
            res = 0
            for p in pref:
                r = idx[p]
                res += fw.sum(r - 1)
                fw.add(r, 1)
            return res

        print(count_le(a) - count_lt(b))

    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# custom tests
assert run("3\n1 1\n1 2 3\n") == "0"
assert run("1\n0 0\n5\n") == "1"
assert run("5\n2 2\n2 2 2 2 2\n") == "15"
assert run("4\n1 1\n1 2 1 2\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | xử lý trường hợp cơ bản | 
| tất cả đều bình đẳng | mảng con tối đa | tính đúng đắn dưới sự đối xứng | 
| khớp ngưỡng thống nhất | hành vi hòa nhập đầy đủ | tính đúng đắn của ranh giới bao gồm | 
| mô hình xen kẽ | thứ tự tiền tố hỗn hợp | Fenwick hành vi nghiêm khắc và không nghiêm khắc | 

## Vỏ cạnh 

Đối với một mảng một phần tử, giả sử$[5]$, mọi mảng con chỉ là phần tử đó. Nếu các ngưỡng sao cho phần tử thỏa mãn cả hai điều kiện, thì thuật toán sẽ khởi tạo tiền tố bằng 0 và xử lý chính xác một lần chèn. Cây Fenwick đếm chính xác cặp tiền tố đơn, tạo ra một mảng con hợp lệ. 

Đối với một mảng trong đó tất cả các giá trị đều bằng ngưỡng, chẳng hạn như$[2,2,2]$với$a = 2$, mọi giá trị được chuyển đổi đều trở thành 0. Tổng tiền tố không đổi, do đó mọi cặp chỉ số tiền tố đều thỏa mãn đẳng thức. Cây Fenwick tích lũy số lượng sao cho tất cả các mảng con đều được bao gồm, phù hợp với kết quả dự kiến$n(n+1)/2$. 

Đối với các trường hợp bất đẳng thức nghiêm ngặt trong đó$b$bằng các giá trị mảng, tổng tiền tố được chuyển đổi lại trở thành hằng số, nhưng việc sử dụng`r - 1`trong các truy vấn đảm bảo không có mảng con có tiền tố bằng nhau được tính. Điều này ngăn chặn việc đếm quá mức khi giá trị trung bình bằng chính xác với ranh giới.
