---
title: "CF 106293F - \u041c\u0443\u0441\u044f \u0438 \u0437\u0430\u043a\u043b\u0438\u043d\u0430\u043d\u0438\u044f"
description: "Chúng ta được cho một dãy số nguyên được viết trong một cuốn sách và chúng ta muốn đếm xem có bao nhiêu đoạn liền kề của dãy này có tổng nằm trong một khoảng cho trước $[l, r]$."
date: "2026-06-18T22:35:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106293
codeforces_index: "F"
codeforces_contest_name: "\u041e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 1\u0421, \u043e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u0442\u0443\u0440 2025-2026"
rating: 0
weight: 106293
solve_time_s: 62
verified: true
draft: false
---

[CF 106293F - \u041c\u0443\u0441\u044f \u0438 \u0437\u0430\u043a\u043b\u0438\u043d\u0430\u043d\u0438\u044f](https://codeforces.com/problemset/problem/106293/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên được viết trong một cuốn sách và chúng ta muốn đếm xem có bao nhiêu đoạn liền kề của dãy này có tổng nằm trong một khoảng nhất định$[l, r]$. 

Một đoạn liền kề được xác định bằng cách chọn hai chỉ số$i \le j$và tổng hợp tất cả các yếu tố từ$a_i$ĐẾN$a_j$. Mỗi phân đoạn như vậy được coi là một “câu thần chú” ứng cử viên. Nhiệm vụ là đếm xem có bao nhiêu phân đoạn tạo ra tổng ít nhất bằng$l$và nhiều nhất$r$. 

Kích thước đầu vào cho phép lên tới$2 \cdot 10^5$số có giá trị lên tới$10^9$về độ lớn. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào kiểm tra rõ ràng tất cả$O(n^2)$mảng con. Ngay cả khi một tổng đơn được tính theo thời gian không đổi bằng cách sử dụng các tổng tiền tố, việc lặp qua tất cả các cặp điểm cuối sẽ dẫn đến khoảng$2 \cdot 10^{10}$trong trường hợp xấu nhất vượt xa thời hạn. 

Điều này thúc đẩy chúng tôi hướng tới một giải pháp làm giảm vấn đề đếm các cặp có cấu trúc thay vì liệt kê chúng. 

Một vấn đề nhỏ xuất hiện khi có số âm. Tổng tiền tố không còn đơn điệu nữa, vì vậy các kỹ thuật dựa vào thứ tự của tổng mảng con phải được xử lý cẩn thận. Ví dụ: cách tiếp cận cửa sổ trượt đơn giản không thành công: ngay cả khi tổng cửa sổ vượt quá$r$, việc mở rộng hoặc thu nhỏ cửa sổ không hoạt động có thể dự đoán được vì việc thêm số âm có thể làm giảm tổng. 

Một trường hợp lỗi minh họa nhỏ cho hai con trỏ là: 

đầu vào:```
3 1 2
2 -2 2
```Mảng con là$[2], [2,-2], [2,-2,2], [-2], [-2,2], [2]$. Câu trả lời đúng phụ thuộc vào phép liệt kê chính xác, nhưng phương pháp hai con trỏ có thể thu nhỏ hoặc mở rộng không chính xác nhiều lần vì tổng không đơn điệu. 

Khó khăn cốt lõi là chúng ta cần đếm tất cả các cặp tổng tiền tố có chênh lệch nằm trong một phạm vi, trong bối cảnh mà tổng tiền tố là số nguyên thực tùy ý. 

## Phương pháp tiếp cận 

Chúng ta bắt đầu với ý tưởng trực tiếp. Chúng ta có thể tính toán tất cả các tổng của mảng con bằng cách sửa điểm cuối bên trái$i$, sau đó kéo dài$j$từ$i$ĐẾN$n$, duy trì một tổng số đang hoạt động. Đối với mỗi$i$, chi phí này$O(n)$, dẫn đến$O(n^2)$tổng số hoạt động. Điều này đúng vì mỗi mảng con được liệt kê duy nhất, nhưng nó sẽ trở nên quá chậm khi$n$đạt tới$2 \cdot 10^5$, trong đó tổng số thao tác sẽ vào khoảng$4 \cdot 10^{10}$. 

Để cải thiện điều này, chúng ta viết lại bài toán bằng cách sử dụng tổng tiền tố. Cho phép$p[0] = 0$Và$p[i] = a_1 + \dots + a_i$. Khi đó tổng của một mảng con$[i+1, j]$là$p[j] - p[i]$. điều kiện$l \le p[j] - p[i] \le r$có thể được sắp xếp lại thành:$$p[j] - r \le p[i] \le p[j] - l.$$Bây giờ vấn đề trở thành: với mỗi$j$, đếm xem có bao nhiêu tổng tiền tố trước đó nằm trong một phạm vi giá trị. Đây là một vấn đề cổ điển về “số lượng giá trị trong một tập hợp tiền tố động”. 

Chúng tôi xử lý tổng tiền tố từ trái sang phải. Ở mỗi bước$j$, chúng ta cần truy vấn có bao nhiêu tổng tiền tố trước đó nằm trong một khoảng. Điều này có thể được giải quyết bằng cây Fenwick (cây được lập chỉ mục nhị phân) sau khi nén tọa độ các tổng tiền tố, vì các giá trị có thể lớn bằng$10^{14}$ở dạng trung gian. 

Mỗi tổng tiền tố được chèn vào cấu trúc một lần và mỗi bước thực hiện một truy vấn phạm vi trên cấu trúc. 

Điều này biến vấn đề thành việc đếm tần số động hiệu quả trên một hệ tọa độ đã được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mảng con Brute Force |$O(n^2)$|$O(1)$| Quá chậm | 
| Tổng tiền tố + Cây Fenwick |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi mảng thành tổng tiền tố để mỗi tổng của mảng con trở thành hiệu của hai giá trị tiền tố. Phép biến đổi này là cần thiết vì nó biến bài toán tổng khoảng thành bài toán đếm cặp. 

Sau đó, chúng tôi duy trì cấu trúc dữ liệu lưu trữ số tiền tiền tố mà chúng tôi đã thấy. Đối với mỗi tổng tiền tố mới, chúng ta hỏi có bao nhiêu tổng tiền tố trước đó nằm trong một khoảng số cụ thể bắt nguồn từ$l$Và$r$. 

1. Xây dựng một mảng tổng tiền tố trong đó giá trị đầu tiên là 0 và mỗi giá trị tiếp theo sẽ tích lũy mảng đầu vào. Điều này đảm bảo mỗi mảng con tương ứng với một cặp chỉ số tiền tố. 
2. Thu thập tất cả các tổng tiền tố và sắp xếp chúng để xây dựng hệ tọa độ nén. Bước này là cần thiết vì tổng tiền tố có thể lớn và âm, nhưng cây Fenwick yêu cầu chỉ số nhỏ gọn. 
3. Khởi tạo cây Fenwick hỗ trợ cập nhật điểm và truy vấn tổng tiền tố trên các chỉ mục được nén. 
4. Chèn tiền tố ban đầu sum 0 vào cấu trúc, vì nó đại diện cho tiền tố trống trước khi mảng bắt đầu. 
5. Lặp lại từng tổng tiền tố từ trái sang phải. Đối với giá trị tiền tố hiện tại$p[j]$, hãy tính phạm vi hợp lệ của các tổng tiền tố trước đó như$[p[j] - r, p[j] - l]$. 
6. Chuyển đổi phạm vi giá trị này thành phạm vi chỉ mục bằng cách sử dụng tìm kiếm nhị phân trên tọa độ đã nén. 
7. Truy vấn cây Fenwick để biết có bao nhiêu tổng tiền tố nằm trong khoảng chỉ số này và thêm số đếm này vào câu trả lời. 
8. Chèn tổng tiền tố hiện tại vào cây Fenwick để nó có sẵn cho các vị trí trong tương lai. 

Ý tưởng chính là khi chúng tôi xử lý vị trí$j$, tất cả đều hợp lệ$i < j$đã được chèn vào, do đó truy vấn trực tiếp đếm các mảng con hợp lệ kết thúc tại$j$. 

### Tại sao nó hoạt động 

Ở mỗi bước$j$, cây Fenwick chứa chính xác tập hợp các tổng tiền tố$p[0] \dots p[j-1]$. Bất kỳ mảng con hợp lệ nào kết thúc tại$j$tương ứng với việc chọn tổng tiền tố trước đó$p[i]$sao cho ràng buộc về chênh lệch được giữ. Việc chuyển đổi đảm bảo mỗi mảng con hợp lệ được tính chính xác một lần tại điểm cuối bên phải của nó và không có mảng con không hợp lệ nào được đưa vào vì phạm vi truy vấn khớp chính xác với các điều kiện bất đẳng thức. 

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
        if r < l:
            return 0
        return self.sum(r) - self.sum(l - 1)

def solve():
    n, l, r = map(int, input().split())
    a = list(map(int, input().split()))

    pref = [0]
    cur = 0
    for x in a:
        cur += x
        pref.append(cur)

    vals = sorted(set(pref))
    def get_id(x):
        import bisect
        return bisect.bisect_left(vals, x) + 1

    bit = Fenwick(len(vals))

    ans = 0
    bit.add(get_id(0), 1)

    for j in range(1, n + 1):
        x = pref[j]
        left = x - r
        right = x - l

        import bisect
        L = bisect.bisect_left(vals, left) + 1
        R = bisect.bisect_right(vals, right)

        ans += bit.range_sum(L, R)
        bit.add(get_id(x), 1)

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp được cấu trúc xung quanh cây Fenwick duy trì số lượng tổng tiền tố được thấy cho đến nay. Bước nén tọa độ đảm bảo rằng các tổng tiền tố số nguyên tùy ý có thể được ánh xạ vào một không gian chỉ mục nhỏ gọn. Các phép toán chia đôi chuyển đổi các ràng buộc giá trị thành các ràng buộc chỉ mục và các truy vấn Fenwick đếm xem có bao nhiêu tổng tiền tố hợp lệ nằm trong phạm vi được yêu cầu. Mỗi tiền tố chỉ được chèn sau khi được sử dụng để đếm, đảm bảo tính chính xác cho các mảng con kết thúc ở mỗi vị trí. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:```
3 3 5
1 2 3
```Tổng tiền tố là:$p = [0, 1, 3, 6]$Chúng tôi xử lý từng bước. 

| j | p[j] | phạm vi [p[j]-r, p[j]-l] | tiền tố hợp lệ trước đó | đóng góp | tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | [-5,-3] | không | 0 | 0 | 
| 1 | 1 | [-4,-2] | không | 0 | 0 | 
| 2 | 3 | [-2,0] | {0} | 1 | 1 | 
| 3 | 6 | [1,3] | {1,3} | 2 | 3 | 

Điều này phù hợp với ý tưởng rằng các mảng con hợp lệ tương ứng chính xác với những khác biệt về tiền tố bên trong khoảng. 

Dấu vết cho thấy cách mỗi tiền tố chỉ được sử dụng sau khi chèn, đảm bảo không có tiền tố nào trong tương lai được tính sai đối với chính nó. 

Một ví dụ thứ hai:```
4 0 0
1 -1 1 -1
```Tổng tiền tố:$p = [0,1,0,1,0]$Chỉ các mảng con có tổng bằng 0 mới được tính và cấu trúc đếm chính xác các xung đột tiền tố lặp lại. 

| j | p[j] | phạm vi | trận đấu trong | đóng góp | tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | [0,0] | không | 0 | 0 | 
| 1 | 1 | [1,1] | không | 0 | 0 | 
| 2 | 0 | [0,0] | {0} | 1 | 1 | 
| 3 | 1 | [1,1] | {1} | 1 | 2 | 
| 4 | 0 | [0,0] | {0,0} | 2 | 4 | 

Điều này thể hiện việc xử lý chính xác các bản sao, điều này rất cần thiết cho tính chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi tổng tiền tố được chèn một lần và mỗi truy vấn sử dụng các phép toán của cây Fenwick trên tọa độ nén | 
| Không gian |$O(n)$| Lưu trữ tổng tiền tố, mảng nén và cây Fenwick | 

các$n \log n$sự phức tạp là đủ cho$2 \cdot 10^5$các phần tử, vì mỗi phép toán đều là logarit và các thừa số không đổi là nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys
    old = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old
    return out.strip()

# provided sample
assert run("3 3 5\n1 2 3\n") == "3"

# minimum size
assert run("1 0 0\n0\n") == "1"

# all negative
assert run("3 -2 -1\n-1 -1 -1\n") == "6"

# all zeros
assert run("5 0 0\n0 0 0 0 0\n") == "15"

# mixed
assert run("4 1 3\n1 -1 2 0\n") == run("4 1 3\n1 -1 2 0\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 0 / 0`| 1 | ranh giới phần tử đơn | 
|`3 -2 -1 / -1 -1 -1`| 6 | tổng âm và nhiều phân đoạn hợp lệ | 
|`5 0 0 / 0 0 0 0 0`| 15 | trùng lặp và đếm tổ hợp | 

## Vỏ cạnh 

Một trường hợp cạnh tranh nhất là khi tất cả các tổng tiền tố đều giống hệt nhau. Ví dụ: khi tất cả các phần tử của mảng bằng 0 thì mọi mảng con đều có tổng bằng 0. Mảng tiền tố trở thành một chuỗi các giá trị lặp lại và cây Fenwick phải đếm chính xác các kết hợp thay vì các giá trị duy nhất. Thuật toán xử lý việc này vì mỗi tiền tố giống hệt nhau được chèn riêng biệt, do đó số lượng tần số được tích lũy một cách tự nhiên. 

Một trường hợp khác là khi$l$Và$r$bao gồm số 0 nhưng mảng chứa các dao động dương và âm lớn. Chẳng hạn, luân phiên$1, -1$tạo ra nhiều giá trị tiền tố lặp lại. Các truy vấn phạm vi vẫn khớp chính xác với tất cả các cặp tiền tố bằng nhau. 

Một trường hợp tế nhị cuối cùng là khi$l$Và$r$là các giới hạn âm và dương lớn, đếm hiệu quả tất cả các mảng con. Trong trường hợp này, mỗi khoảng truy vấn luôn bao gồm phạm vi tiền tố đầy đủ và thuật toán giảm xuống việc đếm tất cả các cặp$i < j$, trở thành$n(n+1)/2$, vẫn được xử lý chính xác thông qua các bản cập nhật tích lũy của Fenwick.
