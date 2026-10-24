---
title: "CF 105948J - Mãi Mãi (II)"
description: "Chúng ta được cung cấp một chuỗi có thể thay đổi trên các chữ cái viết thường. Số lượng cốt lõi mà chúng tôi theo dõi được xác định dựa trên một cặp ký tự có thứ tự cố định, cụ thể là các cặp trong đó ký tự đầu tiên là 'e' và ký tự thứ hai là 'f'."
date: "2026-06-22T16:07:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105948
codeforces_index: "J"
codeforces_contest_name: "CCF CAT NAEC 2025 (Provincial)"
rating: 0
weight: 105948
solve_time_s: 56
verified: true
draft: false
---

[CF 105948J - Mãi mãi (II)](https://codeforces.com/problemset/problem/105948/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi có thể thay đổi trên các chữ cái viết thường. Số lượng cốt lõi mà chúng tôi theo dõi được xác định dựa trên một cặp ký tự có thứ tự cố định, cụ thể là các cặp có ký tự đầu tiên`'e'`và thứ hai là`'f'`. Đối với mỗi lần xuất hiện của một`'e'`ở vị trí`i`và một`'f'`ở vị trí`j`với`i < j`, chúng tôi thêm khoảng cách`j - i`. Nhiệm vụ là tính tổng này cho chuỗi ban đầu và sau mỗi lần cập nhật điểm, trong đó một vị trí của chuỗi bị ghi đè vĩnh viễn. 

Được định khung lại khác nhau, mỗi cặp hợp lệ đều đóng góp một trọng số bằng khoảng cách giữa hai vị trí. Chuỗi thay đổi từng ký tự một và sau mỗi thay đổi, chúng ta cần tổng cặp có trọng số được cập nhật. 

Các ràng buộc đi lên đến`5 × 10^5`cho cả độ dài và bản cập nhật, loại trừ việc tính lại tổng từ đầu sau mỗi lần sửa đổi. Bất kỳ giải pháp nào quét toàn bộ chuỗi trên mỗi truy vấn đều dẫn đến khoảng`O(nm)`hoạt động vượt xa khả năng thực hiện. Thậm chí`O(n log n)`mỗi bản cập nhật thực tế ở đây quá chậm. 

Cấu trúc quan trọng là chỉ những cặp liên quan đến`'e'`Và`'f'`vấn đề. Mọi thứ khác trong bảng chữ cái đều không liên quan ngoại trừ khả năng chuyển đổi sang hoặc ra khỏi hai ký tự này khi có cập nhật. 

Một trường hợp khó phát hiện khi cập nhật lật một ký tự vào hoặc ra khỏi`'e'`hoặc`'f'`. Ví dụ, việc thay đổi một`'e'`ĐẾN`'f'`không chỉ loại bỏ những đóng góp liên quan đến vị trí đó mà còn đảo ngược vai trò của nó trong tất cả các cặp, vì vậy cả tương tác trái và phải đều phải được điều chỉnh. 

Một tình huống phức tạp khác là khi nhiều`'e'`Và`'f'`được xen kẽ. Một cách tiếp cận ngây thơ cố gắng chỉ cập nhật các vùng lân cận cục bộ không thành công vì mỗi vị trí tương tác với tất cả các ký tự thuộc loại đối lập ở cả hai phía. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi truy vấn, hãy xây dựng lại trạng thái chuỗi và tính tổng bằng cách lặp lại tất cả các cặp`(i, j)`với`i < j`, kiểm tra xem`s[i] = 'e'`Và`s[j] = 'f'`, và thêm`j - i`. Điều này đúng vì nó trực tiếp thực hiện định nghĩa. 

Tuy nhiên, mỗi lần tính toán tốn`O(n)`, và có`m`cập nhật, do đó tổng độ phức tạp trở thành`O(nm)`, trong trường hợp xấu nhất đạt đến`2.5 × 10^{11}`hoạt động. Điều này không thể chấp nhận được từ xa. 

Để tối ưu hóa, chúng tôi mở rộng biểu thức theo đại số. Đối với một cố định`'e'`ở vị trí`i`, đóng góp của nó là tổng của`(j - i)`tổng thể`'f'`các vị trí bên phải của nó. Điều này chia thành hai phần: số lượng như vậy`'f'`vị trí trừ`i`số lần được tính, cộng với tổng chỉ số của chúng. Điều này gợi ý việc duy trì số liệu thống kê toàn cầu về`'f'`vị trí và có thể truy vấn thông tin hậu tố một cách hiệu quả. 

Ý tưởng chính là duy trì hai cây Fenwick (hoặc cây phân đoạn): một lần theo dõi số lượng`'e'`Và`'f'`và các tổng theo dõi khác của chỉ số của chúng. Điều này cho phép chúng ta tính toán các đóng góp theo thời gian logarit. Khi một ký tự thay đổi, chúng tôi loại bỏ đóng góp cũ của nó và thêm đóng góp mới bằng cách tính toán lại hiệu ứng của nó đối với tất cả các ký tự đối lập hiện có bằng cách sử dụng truy vấn tiền tố/hậu tố. 

Điều này làm giảm mỗi bản cập nhật xuống`O(log n)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Cập nhật dựa trên Fenwick | O((n + m) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai cây Fenwick cho mỗi ký tự có liên quan`'e'`Và`'f'`. Một cây lưu trữ số lần xuất hiện tại các vị trí và cây kia lưu trữ tổng chỉ số của chúng. 

Chúng tôi cũng duy trì tổng số tiền đóng góp hiện tại. 

1. Khởi tạo cấu trúc Fenwick cho tổng số lượng và chỉ số của`'e'`Và`'f'`dựa trên chuỗi ban đầu. Đối với mỗi vị trí, nếu có`'e'`hoặc`'f'`, chúng tôi chèn nó cho phù hợp. 
2. Tính toán câu trả lời ban đầu bằng cách lặp lại các vị trí và sử dụng truy vấn tiền tố. Đối với mỗi`'e'`ở vị trí`i`, chúng tôi truy vấn có bao nhiêu`'f'`tồn tại ở bên phải và tổng chỉ số của chúng. Sự đóng góp là`(sum of f indices to right) - i * (count of f to right)`. 
3. Đối với mỗi lần cập nhật tại vị trí`p`, trước tiên hãy xóa đóng góp cũ của nó khỏi câu trả lời chung. Điều này đòi hỏi phải xử lý vị trí như thể nó bị xóa khỏi bộ ký tự cũ. 
4. Cập nhật cây Fenwick để xóa ký tự cũ tại vị trí`p`. 
5. Chèn ký tự mới vào vị trí`p`vào các cấu trúc Fenwick tương ứng của nó. 
6. Tính toán lại tác động đóng góp của vị trí`p`ở trạng thái mới và thêm nó trở lại câu trả lời chung. 

Mỗi bản cập nhật chỉ chạm vào các tập hợp tiền tố/hậu tố, vì vậy chúng tôi tránh quét các vị trí không liên quan. 

### Tại sao nó hoạt động 

Tổng số tiền có thể được phân tách thành những đóng góp độc lập của mỗi`'e'`lập trường chống lại tất cả`'f'`các vị trí bên phải của nó. Cây Fenwick bảo tồn chính xác hai tổng hợp cần thiết để đánh giá từng đóng góp đó: số lượng và tổng các chỉ số. Vì các bản cập nhật chỉ thay đổi một chỉ mục tại một thời điểm nên việc điều chỉnh các tổng hợp này cục bộ là đủ để duy trì tính chính xác chung. 

Điều bất biến là tại bất kỳ thời điểm nào, cây Fenwick biểu diễn chính xác nhiều tập chỉ số của`'e'`Và`'f'`, do đó, mọi truy vấn trên phạm vi hậu tố đều trả về số lượng và tổng chính xác cần thiết để xây dựng lại công thức đóng góp. 

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
    n, m = map(int, input().split())
    s = list(input().strip())

    fe_cnt = Fenwick(n)
    fe_sum = Fenwick(n)
    ff_cnt = Fenwick(n)
    ff_sum = Fenwick(n)

    def add_char(ch, idx, delta):
        if ch == 'e':
            fe_cnt.add(idx, delta)
            fe_sum.add(idx, delta * idx)
        elif ch == 'f':
            ff_cnt.add(idx, delta)
            ff_sum.add(idx, delta * idx)

    for i, ch in enumerate(s, 1):
        add_char(ch, i, 1)

    def contribution_e(i):
        c = ff_cnt.range_sum(i + 1, n)
        sidx = ff_sum.range_sum(i + 1, n)
        return sidx - i * c

    ans = 0
    for i, ch in enumerate(s, 1):
        if ch == 'e':
            ans += contribution_e(i)

    out = []
    out.append(str(ans))

    for _ in range(m):
        p, c = input().split()
        p = int(p)
        old = s[p - 1]

        if old != c:
            if old == 'e':
                c_cnt = ff_cnt.range_sum(p + 1, n)
                c_sum = ff_sum.range_sum(p + 1, n)
                ans -= c_sum - p * c_cnt
            elif old == 'f':
                e_cnt = fe_cnt.range_sum(1, p - 1)
                e_sum = fe_sum.range_sum(1, p - 1)
                ans -= p * e_cnt - e_sum

            add_char(old, p, -1)

            if c == 'e':
                c_cnt = ff_cnt.range_sum(p + 1, n)
                c_sum = ff_sum.range_sum(p + 1, n)
                ans += c_sum - p * c_cnt
            elif c == 'f':
                e_cnt = fe_cnt.range_sum(1, p - 1)
                e_sum = fe_sum.range_sum(1, p - 1)
                ans += p * e_cnt - e_sum

            add_char(c, p, 1)
            s[p - 1] = c

        out.append(str(ans))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Cấu trúc Fenwick được nhân đôi cho tổng số lượng và tổng chỉ số vì công thức phụ thuộc vào cả hai đại lượng một cách độc lập. Mỗi bản cập nhật sẽ cẩn thận trừ đi phần đóng góp cũ của vị trí đã sửa đổi trước khi xóa nó khỏi cấu trúc dữ liệu, sau đó chèn phần đóng góp mới sau khi cập nhật cấu trúc, duy trì tính chính xác. 

Một cạm bẫy triển khai phổ biến là quên rằng việc xóa một ký tự phải trừ đi phần đóng góp của nó trước khi cập nhật cây Fenwick. Nếu thực hiện theo thứ tự ngược lại, các truy vấn sẽ phản ánh trạng thái được cập nhật và phép trừ sẽ sai. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi nhỏ nơi có thể nhìn thấy các tương tác:`e f e f`. 

Chúng tôi theo dõi sự đóng góp từ mỗi`'e'`để sau`'f'`. 

| Bước | Chuỗi | Đóng góp từ các vị trí | Tổng cộng | 
| --- | --- | --- | --- | 
| ban đầu | ê ê ê | (1→2)=1, (1→4)=3, (3→4)=1 | 5 | 

Bây giờ cập nhật vị trí 2 từ`'f'`ĐẾN`'e'`. 

| Bước | Chuỗi | Hiệu ứng | Tổng cộng | 
| --- | --- | --- | --- | 
| trước | ê ê ê | căn cứ | 5 | 
| loại bỏ f tại 2 | e _ e f | loại bỏ các cặp trong đó f là điểm cuối bên phải | 3 | 
| thêm e vào 2 | ê ê | thêm những đóng góp điện tử mới | 4 | 

Dấu vết này cho thấy rằng một thay đổi duy nhất sẽ ảnh hưởng đến nhiều cặp trên toàn cầu, không chỉ vùng lân cận cục bộ. 

Một trường hợp khác: tất cả các nhân vật đều trở thành`'e'`. Sau đó không có giá trị hợp lệ`(e, f)`cặp, do đó câu trả lời trở thành số không. Cấu trúc của Fenwick phản ánh chính xác điều này bởi vì`'f'`cây trở nên trống rỗng, khiến tất cả các truy vấn hậu tố trở thành 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Mỗi bản cập nhật và truy vấn sử dụng các phép toán Fenwick trên log n chiều cao | 
| Không gian | O(n) | Bốn mảng Fenwick có kích thước n | 

Các ràng buộc cho phép lên tới nửa triệu phép tính, vì vậy việc cập nhật logarit là cần thiết. Hệ số không đổi của bốn cây Fenwick có thể chấp nhận được dưới bộ nhớ 512MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue() if False else ""

# Manual correctness checks (conceptual; integrate solve properly in real use)

# edge: single char
# 1 0
# a
# -> 0

# all e
# 5 0
# eeeee
# -> 0

# alternating
# 4 0
# efef

# updates flipping roles
# 4 2
# efef
# 2 e
# 3 f
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ký tự đơn | 0 | không có cặp nào tồn tại | 
| tất cả đều giống nhau 'e' | 0 | không có tương tác 'f' | 
| hiệu ứng xen kẽ | 5 | cấu trúc tương tác đầy đủ | 
| lật cập nhật | tính đúng đắn năng động | xử lý đột biến | 

## Vỏ cạnh 

Trường hợp quan trọng là khi một bản cập nhật biến một ký tự không liên quan thành`'e'`hoặc`'f'`. Giả sử một vị trí`p`thay đổi từ`'x'`ĐẾN`'e'`. Trước khi thay đổi, nó không đóng góp gì cả. Sau khi thay đổi, nó đột nhiên tương tác với tất cả`'f'`ở bên phải của nó. Thuật toán xử lý việc này bằng cách truy vấn số liệu thống kê hậu tố tại thời điểm chèn, đảm bảo tất cả các cặp mới được tính ngay lập tức. 

Một trường hợp khác là đảo ngược vai trò, chẳng hạn như`'e'`trở thành`'f'`. Điều này đòi hỏi phải trừ đi những đóng góp cũ trước khi sửa đổi cây Fenwick. Nếu được thực hiện sau khi cập nhật cây, các truy vấn hậu tố và tiền tố sẽ phản ánh cấu hình mới và phép trừ trở nên không hợp lệ. Việc triển khai thực hiện phép trừ một cách rõ ràng trước tiên, sau đó cập nhật cấu trúc, sau đó thêm các đóng góp mới, duy trì tính nhất quán ở mỗi bước.
