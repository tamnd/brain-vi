---
title: "CF 323C - Hai hoán vị"
description: "Mỗi giá trị từ 1 đến n xuất hiện đúng một lần trong cả hai hoán vị. Với giá trị v, đặt: - posP[v] là vị trí của nó trong hoán vị đầu tiên. - posQ[v] là vị trí của nó trong hoán vị thứ hai."
date: "2026-06-06T05:54:23+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 323
codeforces_index: "C"
codeforces_contest_name: "Testing Round 7"
rating: 2400
weight: 323
solve_time_s: 125
verified: true
draft: false
---

[CF 323C - Hai hoán vị](https://codeforces.com/problemset/problem/323/C) 

**Đánh giá:** 2400 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi giá trị từ`1`ĐẾN`n`xuất hiện đúng một lần trong cả hai hoán vị. 

Đối với một giá trị`v`, cho phép:

-`posP[v]`là vị trí của nó trong hoán vị đầu tiên. 
-`posQ[v]`là vị trí của nó trong hoán vị thứ hai. 

Một truy vấn yêu cầu số lượng giá trị có vị trí trong hoán vị đầu tiên nằm bên trong`[l1, r1]`và vị trí của nó trong hoán vị thứ hai nằm bên trong`[l2, r2]`. 

Một cách hữu ích để trực quan hóa dữ liệu là các điểm trên lưới. Mọi giá trị`v`đóng góp một điểm$$(posP[v],\; posQ[v])$$và truy vấn hỏi có bao nhiêu điểm nằm bên trong một hình chữ nhật thẳng hàng với trục. 

Vì cả hai mảng đều là hoán vị nên có chính xác`n`điểm và không có hai điểm nào có cùng tọa độ x hoặc cùng tọa độ y. 

Các ràng buộc loại trừ hoàn toàn bất kỳ lần quét theo truy vấn nào. Với`n = 10^6`Và`m = 2 \cdot 10^5`, thậm chí là một`O(n)`truy vấn sẽ yêu cầu đại khái`2 \cdot 10^{11}`hoạt động. 

Chúng tôi cần một cái gì đó gần với thời gian logarit cho mỗi truy vấn. 

Các tham số truy vấn cũng được mã hóa bằng câu trả lời trước đó. Điều này ngăn cản các kỹ thuật sắp xếp lại ngoại tuyến như thuật toán của Mo. Các câu hỏi phải được trả lời trực tuyến, theo thứ tự nhất định. 

Một nguồn sai lầm phổ biến là quên rằng đường viền hình chữ nhật đã bao hàm. 

Ví dụ:```
p = [1,2,3]
q = [1,2,3]
```Truy vấn```
[1,2] × [2,3]
```chỉ chứa các giá trị`2`, vậy câu trả lời là`1`. 

Cách giải thích nửa mở sẽ tạo ra sai sót`0`. 

Một trường hợp tế nhị khác đến từ sự chuyển đổi trực tuyến. Giả định`n = 5`và câu trả lời trước đó là`2`. Sau đó`x = 3`, Và$$f(5)=((5-1+3)\bmod 5)+1=3.$$Bao quanh modulo là một phần của việc tạo truy vấn và phải được áp dụng trước khi lấy cực tiểu và cực đại. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là xử lý từng truy vấn một cách độc lập. 

Với mọi giá trị`v`, chúng tôi kiểm tra xem$$l_1 \le posP[v] \le r_1$$Và$$l_2 \le posQ[v] \le r_2.$$Điều này đúng vì nó thực hiện định nghĩa của truy vấn theo đúng nghĩa đen. Thật không may nó đòi hỏi`O(n)`làm việc cho mỗi truy vấn. Với`10^6`giá trị và`2 \cdot 10^5`truy vấn, trường hợp xấu nhất vượt quá`10^{11}`séc. 

Điều quan trọng là khi chúng ta biết vị trí trong hoán vị thứ hai, bài toán sẽ trở thành bài toán đếm phạm vi một chiều. 

Xây dựng một mảng$$A[i] = posQ[p_i].$$Chức vụ`i`trong mảng này tương ứng với tọa độ x`i`và giá trị được lưu trữ tương ứng với tọa độ y của cùng một điểm. 

Bây giờ là hình chữ nhật truy vấn$$[l_1,r_1] \times [l_2,r_2]$$trở thành: 

> Trong mảng con`A[l1..r1]`, đếm xem có bao nhiêu giá trị thuộc về`[l2,r2]`. 

Đây là một vấn đề đếm phạm vi tĩnh cổ điển. 

Bởi vì`A`là một hoán vị của`1..n`, Cây Wavelet là sự phù hợp tự nhiên. Nó hỗ trợ:$$\text{count values } \le k$$bên trong bất kỳ mảng con nào trong`O(log n)`thời gian. 

Sau đó$$count([l_2,r_2]) = count(\le r_2) - count(\le l_2-1).$$Cấu trúc được xây dựng một lần, sau đó mọi truy vấn sẽ được trả lời trực tuyến theo thời gian logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Cây Wavelet | O(n log n + m log n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc cả hai hoán vị. 
2. Tính toán`posQ[v]`cho mọi giá trị`v`. 

Điều này cho phép chúng tôi tìm thấy ngay vị trí bất kỳ giá trị nào xuất hiện trong hoán vị thứ hai. 
3. Xây dựng mảng$$A[i] = posQ[p_i].$$Mỗi giá trị bây giờ trở thành một điểm`(i, A[i])`. 
4. Xây dựng cây Wavelet`A`. 

Đối với mọi cấp độ, số lượng tiền tố lưu trữ mô tả số lượng phần tử được chuyển đến phần tử con bên trái. Số lượng tiền tố này chính xác là thứ cho phép chúng tôi hạn chế khoảng thời gian truy vấn trong khi đi xuống cây. 
5. Duy trì`last_answer`. 

Tuyên bố xác định$$x = last\_answer + 1$$ngoại trừ truy vấn đầu tiên trong đó`x = 0`. 
6. Giải mã bốn tham số truy vấn bằng hàm đã cho$$f(z)=((z-1+x)\bmod n)+1.$$7. Tính toán$$l_1=\min(f(a),f(b)), \quad r_1=\max(f(a),f(b)),$$và tương tự cho khoảng thứ hai. 
8. Truy vấn Cây Wavelet để biết số giá trị trong`A[l1..r1]`thuộc về`[l2,r2]`. 

Điều này bằng$$count_{\le r_2} - count_{\le l_2-1}.$$9. Xuất câu trả lời và lưu dưới dạng`last_answer`. 

### Tại sao nó hoạt động 

Mảng`A`chứa chính xác tọa độ y của điểm có tọa độ x là chỉ số mảng. 

Lựa chọn vị trí`l1..r1`TRONG`A`tương đương với việc giới hạn các điểm trong phạm vi x của hình chữ nhật. Đếm các giá trị của`A`bên trong`[l2,r2]`tương đương với việc giới hạn các điểm trong phạm vi y của hình chữ nhật. 

Một điểm thuộc hình chữ nhật khi và chỉ khi cả hai điều kiện đồng thời xảy ra. Cây Wavelet đếm chính xác các giá trị đó trong phạm vi y được yêu cầu trong khi chỉ xem xét phạm vi x được yêu cầu. Mỗi điểm hợp lệ được tính một lần và không có điểm không hợp lệ nào được tính. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

class WaveletTree:
    def __init__(self, data):
        self.pref = []
        self.lo = []
        self.hi = []

        cur = array('I', data)
        segments = [(cur, 1, len(cur))]

        while segments:
            next_segments = []

            for arr, lo, hi in segments:
                self.lo.append(lo)
                self.hi.append(hi)

                if lo == hi:
                    self.pref.append(None)
                    continue

                mid = (lo + hi) >> 1

                pref = array('I', [0])
                left = array('I')
                right = array('I')

                cnt = 0
                for x in arr:
                    if x <= mid:
                        cnt += 1
                        left.append(x)
                    else:
                        right.append(x)
                    pref.append(cnt)

                self.pref.append(pref)

                next_segments.append((left, lo, mid))
                next_segments.append((right, mid + 1, hi))

            segments = next_segments

    def count_leq(self, l, r, k):
        if l > r or k <= 0:
            return 0

        ans = 0
        stack = [(0, l, r)]

        while stack:
            idx, l, r = stack.pop()

            lo = self.lo[idx]
            hi = self.hi[idx]

            if l > r or k < lo:
                continue

            if hi <= k:
                ans += r - l + 1
                continue

            if lo == hi:
                continue

            pref = self.pref[idx]

            left_l = pref[l - 1]
            left_r = pref[r]

            left_count = left_r - left_l

            left_idx = idx * 2 + 1
            right_idx = idx * 2 + 2

            stack.append((
                right_idx,
                l - left_l,
                r - left_r
            ))

            stack.append((
                left_idx,
                left_l + 1,
                left_r
            ))

        return ans

    def range_count(self, l, r, low, high):
        return self.count_leq(l, r, high) - self.count_leq(l, r, low - 1)

def main():
    n = int(input())

    p = list(map(int, input().split()))
    q = list(map(int, input().split()))

    pos_q = [0] * (n + 1)
    for i, v in enumerate(q, 1):
        pos_q[v] = i

    arr = [pos_q[v] for v in p]

    wt = WaveletTree(arr)

    m = int(input())

    last = 0
    out = []

    for qi in range(m):
        a, b, c, d = map(int, input().split())

        x = 0 if qi == 0 else last + 1

        def f(z):
            return ((z - 1 + x) % n) + 1

        fa = f(a)
        fb = f(b)
        fc = f(c)
        fd = f(d)

        l1 = min(fa, fb)
        r1 = max(fa, fb)

        l2 = min(fc, fd)
        r2 = max(fc, fd)

        last = wt.range_count(l1, r1, l2, r2)

        out.append(str(last))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Giai đoạn đầu tiên tính toán hoán vị nghịch đảo của`q`. Điều này chuyển đổi mọi giá trị thành vị trí của nó trong hoán vị thứ hai. 

Mảng`arr`là biểu diễn hình học của bài toán. chỉ mục`i`là tọa độ x và`arr[i]`là tọa độ y tương ứng. 

Cây Wavelet lưu trữ, đối với mỗi nút bên trong, một mảng tiền tố cho biết có bao nhiêu phần tử của mỗi tiền tố được di chuyển sang nút con bên trái. Trong quá trình truy vấn, số tiền tố này chuyển một khoảng mảng con từ nút cha sang các khoảng tương ứng bên trong nút con. 

Thói quen truy vấn tính toán tối đa số lượng giá trị`k`. Số lượng phạm vi có được bằng cách trừ. Điều này tránh việc thực hiện một thao tác đếm hình chữ nhật riêng biệt. 

Một sai lầm dễ dàng là lập chỉ mục. Các công thức truy vấn Cây Wavelet giả định các khoảng dựa trên 1, vì vậy tất cả các khoảng truy vấn được giữ dựa trên 1 trong suốt quá trình triển khai. 

Một điểm tinh tế khác là giải mã trực tuyến. giá trị`x`sử dụng câu trả lời trước cộng một, không phải câu trả lời hiện tại. Truy vấn đầu tiên là ngoại lệ duy nhất trong đó`x = 0`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
3 1 2
3 2 1
1
1 2 3 3
```Đầu tiên ta tính: 

| Giá trị | posQ | 
| --- | --- | 
| 1 | 3 | 
| 2 | 2 | 
| 3 | 1 | 

Sau đó: 

| Vị trí trong p | Giá trị | A[vị trí] | 
| --- | --- | --- | 
| 1 | 3 | 1 | 
| 2 | 1 | 3 | 
| 3 | 2 | 2 | 

Vì thế`A = [1,3,2]`. 

Việc giải mã truy vấn mang lại: 

| Biến | Giá trị | 
| --- | --- | 
| l1 | 1 | 
| r1 | 2 | 
| l2 | 3 | 
| r2 | 3 | 

Chúng tôi kiểm tra`A[1..2] = [1,3]`. 

Chỉ có một giá trị nằm ở`[3,3]`, cụ thể là`3`. 

Trả lời:```
1
```Dấu vết này cho thấy việc giảm từ truy vấn hình chữ nhật thành truy vấn phạm vi giá trị bên trong một mảng con. 

### Ví dụ tùy chỉnh 

đầu vào:```
4
1 2 3 4
2 1 4 3
1
1 4 1 2
```Vị trí nghịch đảo của`q`: 

| Giá trị | posQ | 
| --- | --- | 
| 1 | 2 | 
| 2 | 1 | 
| 3 | 4 | 
| 4 | 3 | 

Do đó: 

| Vị trí | A | 
| --- | --- | 
| 1 | 2 | 
| 2 | 1 | 
| 3 | 4 | 
| 4 | 3 | 

Hình chữ nhật là`[1,4] × [1,2]`. 

| mảng con | Giá trị | 
| --- | --- | 
| A[1..4] | 2,1,4,3 | 

Giá trị bên trong`[1,2]`là`2`Và`1`. 

Trả lời:```
2
```Ví dụ này cho thấy rằng lọc phạm vi x và lọc phạm vi y trở thành lựa chọn mảng con và lựa chọn giá trị bên trong`A`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + m log n) | Xây dựng cây Wavelet cộng với truy vấn logarit | 
| Không gian | O(n log n) | Số tiền tố được lưu trữ ở mỗi cấp độ | 

Với`n = 10^6`, chiều cao xấp xỉ`20`. Tổng lượng thông tin tiền tố được lưu trữ là khoảng`20n`, phù hợp thoải mái trong giới hạn bộ nhớ khi được lưu trữ trong mảng số nguyên nhỏ gọn. Số lượng truy vấn chỉ là`2 · 10^5`, Vì thế`O(log n)`mỗi truy vấn có thể dễ dàng đủ nhanh. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    from array import array

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    # call solution here
    main()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# sample 1
assert run(
"""3
3 1 2
3 2 1
1
1 2 3 3
"""
) == "1"

# n = 1
assert run(
"""1
1
1
1
1 1 1 1
"""
) == "1"

# identical permutations
assert run(
"""3
1 2 3
1 2 3
1
1 3 2 3
"""
) == "2"

# full rectangle
assert run(
"""4
1 2 3 4
2 1 4 3
1
1 4 1 4
"""
) == "4"

# single point rectangle
assert run(
"""4
1 2 3 4
2 1 4 3
1
2 2 1 1
"""
) == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | 1 | Tính đúng đắn cơ bản | 
| n = 1 | 1 | Kích thước tối thiểu | 
| Hoán vị giống hệt nhau | 2 | Ranh giới bao gồm | 
| Hình chữ nhật đầy đủ | 4 | Toàn bộ điểm được tính | 
| Hình chữ nhật một điểm | 1 | Khớp tọa độ chính xác | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1
1
1
1
1 1 1 1
```Điểm duy nhất là`(1,1)`. Hình chữ nhật truy vấn cũng chính xác`(1,1)`. Cây Wavelet chứa một giá trị duy nhất và số phạm vi trả về`1`. Không cần xử lý đặc biệt. 

Hãy xem xét một khoảng bao gồm một vị trí duy nhất:```
4
1 2 3 4
2 1 4 3
1
2 2 1 1
```Hình chữ nhật hỏi xem điểm ở tọa độ x`2`có tọa độ y`1`. Từ`A[2] = 1`, câu trả lời là`1`. Khoảng thời gian truy vấn vẫn hợp lệ vì việc triển khai sử dụng các ranh giới bao hàm ở mọi nơi. 

Xem xét việc bọc modulo trong quá trình giải mã truy vấn. Cho phép`n = 5`và giả sử câu trả lời trước đó là`2`. Sau đó`x = 3`. 

Đối với giá trị đầu vào`5`:$$f(5)=((5-1+3)\bmod 5)+1=3.$$Thuật toán thực hiện phép biến đổi này trước khi xây dựng các điểm cuối hình chữ nhật. Sau khi giải mã, nó vẫn áp dụng`min`Và`max`, đảm bảo một khoảng thời gian hợp lệ ngay cả khi việc gói xảy ra. 

Cuối cùng, hãy xem xét một hình chữ nhật bao phủ toàn bộ mặt phẳng:```
l1 = 1, r1 = n
l2 = 1, r2 = n
```Mọi điểm đều thỏa mãn truy vấn. Cây Wavelet tính toán$$count(\le n)-count(\le 0)=n-0=n,$$đó chính xác là câu trả lời đúng
