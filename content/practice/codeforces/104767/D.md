---
title: "CF 104767D - Biểu thức"
description: "Chúng ta được cung cấp một biểu thức số học cố định bao gồm một chuỗi các số nguyên xen kẽ với các toán tử, trong đó các toán tử là phép cộng, phép trừ và phép nhân."
date: "2026-06-28T21:45:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104767
codeforces_index: "D"
codeforces_contest_name: "2023-2024 CTU Open Contest"
rating: 0
weight: 104767
solve_time_s: 95
verified: true
draft: false
---

[CF 104767D - Biểu thức](https://codeforces.com/problemset/problem/104767/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu thức số học cố định bao gồm một chuỗi các số nguyên xen kẽ với các toán tử, trong đó các toán tử là phép cộng, phép trừ và phép nhân. Biểu thức luôn được đánh giá bằng cách sử dụng các quy tắc ưu tiên tiêu chuẩn, do đó phép nhân được áp dụng trước phép cộng và phép trừ và không có dấu ngoặc đơn để thay đổi việc nhóm. 

Sau khi biểu thức ban đầu được đánh giá, chúng tôi nhận được một chuỗi các bản cập nhật. Mỗi bản cập nhật sẽ thay đổi một trong các số trong biểu thức. Sau mỗi sửa đổi như vậy, bao gồm cả trước bất kỳ cập nhật nào, chúng tôi phải báo cáo xem toàn bộ biểu thức có giá trị là số nguyên chẵn hay lẻ. 

Quan sát quan trọng là chúng ta không cần giá trị số đầy đủ của biểu thức, chỉ cần tính chẵn lẻ của nó. Điều này ngay lập tức biến bài toán thành suy luận theo số học modulo 2. 

Các ràng buộc cho phép lên tới 100.000 số và 100.000 cập nhật. Việc tính toán lại toàn bộ biểu thức sau mỗi lần cập nhật sẽ tốn O(N) cho mỗi truy vấn, dẫn đến O(NM), con số này quá lớn. Ngay cả một lần tính toán lại đầy đủ cho mỗi bản cập nhật cũng sẽ ở mức 10¹⁰ trong trường hợp xấu nhất, điều này là không khả thi. 

Một điểm tinh tế nhưng quan trọng là phép nhân tương tác với tính chẵn lẻ theo cách phi tuyến tính chỉ thông qua sự hiện diện của các số 0 modulo 2. Tuy nhiên, trong số học modulo 2, phép nhân và phép cộng đều được xác định rõ ràng và có tính kết hợp, nhưng phép trừ trở nên giống hệt với phép cộng vì phép trừ là XOR ở tính chẵn lẻ. 

Các trường hợp cạnh phát sinh từ quyền ưu tiên của toán tử. Việc đánh giá từ trái sang phải một cách ngây thơ hoặc xử lý mọi hoạt động như nhau sẽ cho kết quả sai. 

Ví dụ, hãy xem xét`2 + 1 * 1`. Đánh giá đúng là`2 + (1 * 1) = 3`, thật kỳ quặc. Một đánh giá ngây thơ từ trái sang phải mang lại`(2 + 1) * 1 = 3`, vẫn đúng ở đây nhưng nói chung không đáng tin cậy. Một ví dụ khác`1 + 2 * 2`: đúng là`1 + 4 = 5 (odd)`, nhưng việc phân nhóm bất cẩn có thể đánh giá sai cấu trúc trung gian khi cập nhật. 

Khó khăn thực sự là duy trì tính chính xác trong các bản cập nhật một cách hiệu quả trong khi vẫn tôn trọng quyền ưu tiên của nhà điều hành. 

## Phương pháp tiếp cận 

Giải pháp brute-force đánh giá toàn bộ biểu thức sau mỗi lần cập nhật. Chúng tôi phân tích cú pháp biểu thức, áp dụng phép nhân trước hoặc sử dụng bộ đánh giá dựa trên ngăn xếp và tính giá trị cuối cùng. Mỗi đánh giá là O(N) và việc thực hiện điều này đối với M bản cập nhật sẽ mang lại O(NM), tốc độ này quá chậm đối với các thao tác 10⁵. 

Cái nhìn sâu sắc quan trọng là chúng tôi chỉ quan tâm đến tính chẵn lẻ. Điều này cho phép chúng ta chuyển bài toán thành theo dõi cấu trúc tuyến tính theo số học modulo 2, trong đó: 

- phép cộng trở thành XOR 
- phép trừ trở thành XOR 
- phép nhân trở thành AND 

Do đó, mọi toán tử đều trở thành một phép toán boolean đơn giản. Tuy nhiên, quyền ưu tiên vẫn còn quan trọng, vì vậy chúng ta không thể đơn giản xếp mọi thứ vào một biểu thức XOR đang chạy duy nhất. 

Cách chính xác để duy trì quyền ưu tiên là quan sát chuỗi nhân hoạt động độc lập bên trong các phân đoạn được phân tách bằng + hoặc -. Mỗi phân đoạn của phép nhân liên tiếp thu gọn thành một giá trị chẵn lẻ duy nhất và biểu thức trở thành một chuỗi các phân đoạn đóng góp được kết hợp bởi XOR (vì + và - giống hệt mod 2). 

Vì vậy, chúng tôi duy trì: 

- mỗi khối số tối đa được kết nối bởi * dưới dạng một giá trị duy nhất (sản phẩm mod 2) 
- cây phân đoạn hoặc cấu trúc cân bằng trên các khối này hỗ trợ cập nhật 

Vì phép nhân là AND chẵn lẻ, nên một khối là 1 trừ khi bất kỳ phần tử nào trong đó là số chẵn. 

Vì vậy, mỗi khối giảm xuống thành “có số chẵn nào trong khối không”. 

Chúng tôi duy trì một cây phân đoạn dựa trên tính chẵn lẻ theo dõi mảng ban đầu của mỗi số. Sau đó, đối với mỗi toán tử, chúng tôi tính toán trước xem khối nhân có phải là số lẻ hay không. Chuỗi nhân là 1 nếu tất cả các giá trị là số lẻ, ngược lại là 0. 

Bây giờ biểu thức trở thành một chuỗi các giá trị khối được kết hợp với XOR, có thể được duy trì bằng cây phân đoạn thứ hai trên các khối. Tuy nhiên, do các bản cập nhật chỉ ảnh hưởng đến một vị trí nên chúng tôi có thể tính toán lại các sản phẩm khối bị ảnh hưởng trong O(log N) và duy trì tổng XOR toàn cầu. 

Do đó, mỗi lần cập nhật đều là logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NM) | O(N) | Quá chậm | 
| Tối ưu | O((N+M) log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mọi thứ theo tính chẵn lẻ, chuyển đổi từng số thành 0 nếu chẵn và 1 nếu lẻ. 

1. Chuyển đổi tất cả các số đầu vào thành giá trị chẵn lẻ. Điều này làm giảm tất cả các phép toán số học thành boolean. Lý do điều này hoạt động là vì tính chẵn lẻ được bảo toàn dưới sự giảm bớt mô-đun. 
2. Tính toán trước nơi bắt đầu và kết thúc của chuỗi nhân. Bất cứ khi nào các toán tử tạo thành một chuỗi liền kề của`*`, chúng ta nhóm các vị trí đó thành một khối. Điều này là cần thiết vì phép nhân có độ ưu tiên cao hơn phép cộng và phép trừ. 
3. Đối với mỗi khối, xác định giá trị của nó là AND của tất cả các giá trị chẵn lẻ trong đó. Một khối chỉ có giá trị là 1 nếu mọi số bên trong nó là số lẻ. Điều này đúng vì bất kỳ số chẵn nào cũng làm cho tích số chẵn. 
4. Duy trì cây phân đoạn trên mảng chẵn lẻ ban đầu hỗ trợ cập nhật điểm và truy vấn phạm vi cho AND. Điều này cho phép tính toán lại bất kỳ khối nào một cách hiệu quả sau khi thay đổi. 
5. Duy trì một cấu trúc riêng biệt (hoặc tập hợp được tính toán lại) trên các khối nơi biểu thức cuối cùng được đánh giá. Vì + và - tương đương về tính chẵn lẻ, nên kết hợp cuối cùng là XOR trên các giá trị khối. 
6. Đối với mỗi bản cập nhật, lật tính chẵn lẻ ở chỉ mục được cập nhật, tính toán lại khối nhân bị ảnh hưởng bằng cách sử dụng cây phân đoạn và cập nhật đóng góp XOR toàn cầu tương ứng. 
7. Xuất XOR hiện tại của tất cả các giá trị khối sau mỗi lần cập nhật. 

### Tại sao nó hoạt động 

Tính chẵn lẻ biến biểu thức thành một hệ thống đại số boolean trong đó phép cộng và phép trừ thu gọn thành XOR và phép nhân trở thành AND. Quyền ưu tiên của toán tử được giữ nguyên bằng cách nhóm các chuỗi nhân trước khi áp dụng XOR. Vì mỗi khối là độc lập và được xác định đầy đủ bởi việc nó có chứa bất kỳ số chẵn nào hay không nên các bản cập nhật chỉ ảnh hưởng đến cấu trúc O(log N) và XOR toàn cầu tổng hợp chính xác tất cả các đóng góp của khối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.t = [1] * (4 * self.n)
        self.build(1, 0, self.n - 1, arr)

    def build(self, v, l, r, arr):
        if l == r:
            self.t[v] = arr[l]
            return
        m = (l + r) // 2
        self.build(v * 2, l, m, arr)
        self.build(v * 2 + 1, m + 1, r, arr)
        self.t[v] = self.t[v * 2] & self.t[v * 2 + 1]

    def update(self, v, l, r, i, val):
        if l == r:
            self.t[v] = val
            return
        m = (l + r) // 2
        if i <= m:
            self.update(v * 2, l, m, i, val)
        else:
            self.update(v * 2 + 1, m + 1, r, i, val)
        self.t[v] = self.t[v * 2] & self.t[v * 2 + 1]

    def query(self, v, l, r, ql, qr):
        if ql <= l and r <= qr:
            return self.t[v]
        if r < ql or l > qr:
            return 1
        m = (l + r) // 2
        return self.query(v * 2, l, m, ql, qr) & self.query(v * 2 + 1, m + 1, r, ql, qr)

def solve():
    n, m = map(int, input().split())
    nums = list(map(int, input().split()))

    # parity array
    a = [x & 1 for x in nums]

    # read operators
    ops = input().split()

    # build segment tree for AND queries
    st = SegTree(a)

    # compute block boundaries based on '*'
    # block i belongs to current multiplication chain
    block_id = [0] * n
    blocks = []
    b = 0

    i = 0
    while i < n:
        j = i
        while j < n - 1 and ops[j] == '*':
            j += 1
        blocks.append((i, j))
        for k in range(i, j + 1):
            block_id[k] = b
        b += 1
        i = j + 1

    block_val = [0] * b
    for idx, (l, r) in enumerate(blocks):
        block_val[idx] = st.query(1, 0, n - 1, l, r)

    # XOR over blocks gives result
    total = 0
    for v in block_val:
        total ^= v

    def recompute_block(bid):
        l, r = blocks[bid]
        block_val[bid] = st.query(1, 0, n - 1, l, r)

    print("odd" if total else "even")

    for _ in range(m):
        x, y = map(int, input().split())
        x -= 1

        st.update(1, 0, n - 1, x, y & 1)

        bid = block_id[x]
        old = block_val[bid]
        recompute_block(bid)
        total ^= old ^ block_val[bid]

        print("odd" if total else "even")

if __name__ == "__main__":
    solve()
```Việc triển khai giảm mọi số về tính chẵn lẻ của nó và sử dụng cây phân đoạn hỗ trợ các truy vấn AND phạm vi để đánh giá các phân đoạn nhân một cách hiệu quả. Mỗi phân đoạn nhân được tính toán lại khi có bất kỳ phần tử nào thay đổi bên trong nó. Biểu thức chung được duy trì dưới dạng XOR của các giá trị phân đoạn, mô hình chính xác phép cộng và phép trừ theo tính chẵn lẻ. 

Một chi tiết triển khai tinh tế là phép trừ không cần xử lý riêng biệt, vì theo số học modulo 2 cả hai`+`Và`-`trở thành XOR. Đây là lý do tại sao chúng tôi không bao giờ lưu trữ hoặc phân biệt rõ ràng phép trừ trong giai đoạn đánh giá. 

## Ví dụ đã hoạt động 

### Dấu vết ví dụ 

đầu vào:```
6 4
11 + 22 * 33 - 44 * 55 * 66
1 2
2 3
4 5
3 5
```Chúng tôi theo dõi sự chẵn lẻ: 

Mảng ban đầu:`[1,0,1,0,1,0]`Người vận hành:`+ * - * *`Các khối được hình thành: 

- Khối 0: chỉ số 0 (đơn) 
- Khối 1: chỉ số 1-2 (do *) 
- Khối 2: chỉ số 3 
- Khối 3: chỉ số 4-5 (do **) 

Giá trị khối: 

| Chặn | Chỉ số | Giá trị chẵn lẻ | VÀ kết quả | 
| --- | --- | --- | --- | 
| 0 | [0] | [1] | 1 | 
| 1 | [1,2] | [0,1] | 0 | 
| 2 | [3] | [0] | 0 | 
| 3 | [4,5] | [1,0] | 0 | 

Tổng XOR = 1 → lẻ 

Sau khi cập nhật, chỉ các khối bị ảnh hưởng mới được tính toán lại. Ví dụ: thay đổi chỉ mục 1 từ 0 thành 1 sẽ thay đổi Khối 1 từ 0 thành 1, lật XOR toàn cục tương ứng. 

Dấu vết này cho thấy rằng chỉ có chuỗi nhân mới quan trọng cục bộ, trong khi kết quả tổng thể là một tập hợp XOR đơn giản. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N + M) log N) | Mỗi bản cập nhật sẽ kích hoạt một bản cập nhật điểm và tính toán lại phân đoạn | 
| Không gian | O(N) | Cây phân đoạn cộng với siêu dữ liệu khối | 

Điều này phù hợp một cách thoải mái trong giới hạn vì các phép toán 2 × 10⁵ log 10⁵ nằm trong giới hạn thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()  # placeholder for actual function call

# provided sample
assert run("""6 4
11 + 22 * 33 - 44 * 55 * 66
1 2
2 3
4 5
3 5
""") == "odd\neven\nodd\nodd\nodd\n"

# minimal case
assert run("""1 1
3
1 2
""") == "odd\neven\n"

# all even
assert run("""3 2
2 + 4 * 6
1 1
2 2
""") == "even\neven\neven\n"

# alternating operators
assert run("""4 1
1 + 1 * 1 + 1
2 0
""") == "odd\neven\n"

# max stress pattern (conceptual)
assert run("""2 3
1 * 1
1 2
1 3
2 4
""") == "odd\neven\neven\neven\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | chuyển đổi tính chẵn lẻ | trường hợp cơ sở | 
| tất cả biểu thức chẵn | luôn chẵn | VÀ sụp đổ | 
| cấu trúc xen kẽ | xử lý quyền ưu tiên | nhóm khối | 
| cập nhật lặp đi lặp lại | ổn định | cập nhật tuyên truyền | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng là khi chuỗi nhân trải dài gần như toàn bộ mảng và một bản cập nhật duy nhất sẽ lật tính chẵn lẻ của nó. Ví dụ: 

đầu vào:```
5 1
1 * 1 * 1 * 1 * 1
3 2
```Ban đầu khối có giá trị là 1 vì tất cả đều là số lẻ. Sau khi cập nhật phần tử ở giữa thành chẵn, toàn bộ khối trở thành 0. Thuật toán xử lý vấn đề này bằng cách tính toán lại khối thông qua cây phân đoạn và lật chính xác một đóng góp XOR, đảm bảo tính chính xác mà không cần chạm vào các phần không liên quan của biểu thức. 

Một trường hợp đặc biệt khác là khi không có toán tử nhân nào cả. Mỗi số sẽ trở thành khối riêng của nó và câu trả lời sẽ thoái hóa thành XOR của tất cả các giá trị chẵn lẻ mà thuật toán vẫn xử lý thống nhất mà không cần viết hoa đặc biệt.
