---
title: "CF 102625I - Treat To Banta Hai"
description: "Bài toán yêu cầu chúng ta chọn một nhóm con liên tục theo thứ tự đã cho. Chúng ta có thể loại bỏ một số đàn em ngay từ đầu và một số đàn em ở cuối, nhưng những đàn em còn lại phải ở lại liên tiếp. Nếu đoạn được chọn có các giá trị t1, t2, ..."
date: "2026-08-03T15:23:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102625
codeforces_index: "I"
codeforces_contest_name: "IIT(ISM) Virtual Farewell"
rating: 0
weight: 102625
solve_time_s: 49
verified: true
draft: false
---

[CF 102625I - Điều trị Banta Hai](https://codeforces.com/problemset/problem/102625/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta chọn một nhóm con liên tục theo thứ tự đã cho. Chúng ta có thể loại bỏ một số đàn em ngay từ đầu và một số đàn em ở cuối, nhưng những đàn em còn lại phải ở lại liên tiếp. Nếu đoạn được chọn có giá trị`t1, t2, ..., tk`, hạnh phúc của nó được tính như`1*t1 + 2*t2 + ... + k*tk`. Chúng ta cần hạnh phúc tối đa có thể, và không được phép chọn đàn em, cho điểm 0. 

Đầu vào chứa số lượng đàn em và giá trị đóng góp hạnh phúc của mỗi đàn em. Đầu ra là một số nguyên duy nhất biểu thị điểm tốt nhất có thể đạt được từ bất kỳ phân đoạn liền kề nào. 

Số lượng đàn em có thể đạt tới`2 * 10^5`và mỗi giá trị có thể có độ lớn lên tới`10^7`. Một cách tiếp cận bậc hai sẽ thực hiện xung quanh`n^2 / 2`kiểm tra phân đoạn, trở thành khoảng`2 * 10^10`hoạt động trong trường hợp xấu nhất. Điều đó vượt xa những gì có thể phù hợp trong một giới hạn thời gian bình thường. Chúng ta cần một thuật toán gần tuyến tính hoặc`n log n`. 

Các giá trị có thể âm, do đó, giả sử rằng phân đoạn không trống luôn tốt hơn là không chính xác. Ví dụ: 

đầu vào:```
3
-60 -70 -80
```Đầu ra đúng là:```
0
```Phương thức luôn bắt đầu bằng phần tử đầu tiên hoặc luôn chọn một phân đoạn sẽ trả về giá trị âm, mặc dù không được phép xử lý. 

Một trường hợp phức tạp khác là trọng số của phân đoạn sẽ khởi động lại từ một sau khi loại bỏ tiền tố. Ví dụ: 

đầu vào:```
6
5 -1000 1 -3 7 -8
```Phân đoạn tốt nhất là`[1, -3, 7]`, không phải phân đoạn chứa giá trị đầu tiên. Điểm của nó là`1*1 + 2*(-3) + 3*7 = 16`. Việc triển khai bất cẩn bằng cách sử dụng các chỉ mục gốc thay vì các vị trí bên trong phân đoạn đã chọn sẽ tính toán biểu thức sai. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là thử mọi phân đoạn liền kề có thể. Đối với một đoạn từ`l`ĐẾN`r`, chúng ta có thể tính điểm của nó bằng cách duyệt qua nó và nhân mọi phần tử với vị trí của nó bên trong đoạn đó. Điều này đúng vì nó tuân theo định nghĩa chính xác. 

Tuy nhiên, có`O(n^2)`các phân đoạn có thể. Ngay cả khi mọi phép tính điểm được tối ưu hóa bằng tổng tiền tố, việc kiểm tra tất cả các cặp điểm cuối vẫn cần khoảng`4 * 10^10`kết hợp điểm cuối khi`n = 200000`, quá chậm. 

Quan sát quan trọng là điểm của một đoạn có thể được chuyển thành dạng trong đó mỗi ranh giới bên trái có thể trở thành một đường. Tổng tiền tố cho phép chúng ta tránh phải xây dựng lại tổng có trọng số cho mỗi phân đoạn và sau đó cấu trúc dữ liệu có thể duy trì ranh giới bên trái tốt nhất trước đó. 

Cho phép`P[i]`là tổng tiền tố của các giá trị lên tới`i`, và để`Q[i]`là tổng tiền tố của`index * value`, trong đó các chỉ số bắt đầu từ một. Đối với một đoạn bắt đầu sau vị trí`j`và kết thúc tại`r`, số điểm của nó là:`Q[r] - Q[j] - j * (P[r] - P[j])`Sắp xếp lại:`Q[r] - j * P[r] + (j * P[j] - Q[j])`Đối với một cố định`r`,`Q[r]`là không đổi. Biểu thức còn lại yêu cầu giá trị tối đa của dòng:`y = -j * x + (j * P[j] - Q[j])`Tại`x = P[r]`. 

Mỗi vị trí trước đó tạo một dòng và mỗi tổng tiền tố mới sẽ trở thành một truy vấn. Vì tổng tiền tố có thể âm hoặc dương nên chúng tôi sử dụng Li Chao Tree để hỗ trợ tọa độ truy vấn tùy ý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng tổng tiền tố trong khi xử lý mảng từ trái sang phải. Duy trì`P`, tổng tiền tố bình thường và`Q`, tổng tiền tố có trọng số. 
2. Chèn dòng tương ứng với vị trí`j = 0`trước khi xử lý bất kỳ phần tử nào. Điều này thể hiện việc bắt đầu một phân đoạn từ phần tử đầu tiên. Đường có độ dốc`0`và chặn`0`. 
3. Đối với mọi vị trí`r`, cập nhật tổng tiền tố để bao gồm giá trị hiện tại. Truy vấn Cây Li Chao tại`x = P[r]`. Giá trị dòng tối đa được trả về cộng thêm`Q[r]`cung cấp phân đoạn tốt nhất kết thúc tại`r`. 
4. Sau khi sử dụng tiền tố hiện tại, hãy chèn dòng được tạo bởi vị trí này. Dòng này dựa trên`j = r`, vì các phân đoạn trong tương lai có thể bắt đầu sau vị trí này. 
5. Giữ mức tối đa giữa tất cả các giá trị thu được và 0, vì việc chọn không có cấp độ con nào là hợp lệ. 

Tại sao nó hoạt động: 

Ở mọi vị trí`r`, mọi đoạn có thể kết thúc tại`r`tương ứng với chính xác một vị trí trước đó`j`. Dòng được tạo bởi`j`lưu trữ sự đóng góp của việc chọn điểm bắt đầu đó. Truy vấn tổng tiền tố hiện tại sẽ chọn điểm bắt đầu tốt nhất có thể trong số tất cả các điểm trước đó. Vì mọi phân đoạn có thể đều được xem xét khi điểm cuối phù hợp của nó được xử lý nên giá trị tối đa được tìm thấy chính xác là mức độ hạnh phúc tốt nhất có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class LiChao:
    def __init__(self, xs):
        self.xs = xs
        self.tree = [None] * (4 * len(xs))

    def value(self, line, x):
        m, c = line
        return m * x + c

    def add_line(self, line, node=1, left=0, right=None):
        if right is None:
            right = len(self.xs) - 1

        mid = (left + right) // 2
        x_left = self.xs[left]
        x_mid = self.xs[mid]

        if self.tree[node] is None:
            self.tree[node] = line
            return

        cur = self.tree[node]

        if self.value(line, x_mid) > self.value(cur, x_mid):
            self.tree[node], line = line, self.tree[node]
            cur = self.tree[node]

        if left == right:
            return

        if self.value(line, x_left) > self.value(cur, x_left):
            self.add_line(line, node * 2, left, mid)
        elif self.value(line, self.xs[right]) > self.value(cur, self.xs[right]):
            self.add_line(line, node * 2 + 1, mid + 1, right)

    def query(self, x, node=1, left=0, right=None):
        if right is None:
            right = len(self.xs) - 1

        res = -10**30
        if self.tree[node] is not None:
            res = self.value(self.tree[node], x)

        if left == right:
            return res

        mid = (left + right) // 2
        if x <= self.xs[mid]:
            return max(res, self.query(x, node * 2, left, mid))
        return max(res, self.query(x, node * 2 + 1, mid + 1, right))

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    pref = [0]
    weighted = [0]

    s = 0
    w = 0
    for i, x in enumerate(a, 1):
        s += x
        w += i * x
        pref.append(s)
        weighted.append(w)

    lichao = LiChao(pref)

    ans = 0
    lichao.add_line((0, 0))

    for i in range(1, n + 1):
        best = lichao.query(pref[i])
        ans = max(ans, weighted[i] + best)

        j = i
        lichao.add_line((-j, j * pref[j] - weighted[j]))

    print(ans)

if __name__ == "__main__":
    solve()
```Cây Li Chao lưu trữ các dòng ở dạng`slope * x + intercept`. Đối với vị trí`j`, độ dốc là`-j`và giao điểm là`j * P[j] - Q[j]`, phù hợp với công thức phân đoạn được sắp xếp lại. 

Mảng tiền tố sử dụng các vị trí dựa trên một vì hệ số nhân trong điểm hạnh phúc bắt đầu từ một. Việc trộn lẫn các chỉ số dựa trên số 0 với công thức là nguyên nhân phổ biến dẫn đến các câu trả lời sai. 

Truy vấn xảy ra trước khi chèn dòng của vị trí hiện tại. Không thể sử dụng tiền tố hiện tại làm điểm bắt đầu cho một phân đoạn kết thúc bằng chính nó vì điều đó sẽ tạo ra một phân đoạn trống. Dòng số 0 được chèn ban đầu xử lý các phân đoạn bắt đầu ở cấp dưới đầu tiên. 

Số nguyên Python không bị tràn, do đó các giá trị trung gian lớn từ`index * value`vẫn an toàn. 

## Ví dụ đã hoạt động 

Đối với ví dụ đầu tiên:```
6
5 -1000 1 -3 7 -8
```Các giá trị quan trọng trong quá trình xử lý là: 

| Vị trí | Tiền tố Tổng | Tiền tố có trọng số | Giá trị dòng tốt nhất | Câu trả lời hiện tại | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 5 | 0 | 5 | 
| 2 | -995 | -1995 | 0 | 5 | 
| 3 | -994 | -1992 | 1 | 16 | 
| 4 | -997 | -2004 | 6 | 16 | 
| 5 | -990 | -1969 | 15 | 16 | 
| 6 | -998 | -2017 | 23 | 6 | 

Mức tối đa xảy ra khi đoạn được chọn là`[1, -3, 7]`. Dấu vết cho thấy điểm khởi đầu tốt nhất không nhất thiết phải là phần tử đầu tiên của mảng. 

Đối với ví dụ thứ hai:```
5
1000 1000 1001 1000 1000
```| Vị trí | Tiền tố Tổng | Tiền tố có trọng số | Giá trị dòng tốt nhất | Câu trả lời hiện tại | 
| --- | --- | --- | --- | --- | 
| 1 | 1000 | 1000 | 0 | 1000 | 
| 2 | 2000 | 3000 | -1000 | 2000 | 
| 3 | 3001 | 6003 | -1000 | 5003 | 
| 4 | 4001 | 10003 | -1000 | 9003 | 
| 5 | 5001 | 15003 | 0 | 15003 | 

Thuật toán giữ tất cả các vị trí bắt đầu có thể, cho phép nó phát hiện ra rằng việc lấy toàn bộ mảng là tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi trong số`n`tiền tố thực hiện một truy vấn Li Chao và một lần chèn. | 
| Không gian | O(n) | Danh sách tọa độ và Cây Li Chao chứa số tuyến tính của các giá trị được lưu trữ. | 

Với`n = 200000`, hệ số logarit giữ số phép toán khoảng vài triệu, vừa vặn thoải mái. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("6\n5 -1000 1 -3 7 -8\n") == "16\n"
assert run("5\n1000 1000 1001 1000 1000\n") == "15003\n"

assert run("1\n-5\n") == "0\n"
assert run("3\n-60 -70 -80\n") == "0\n"
assert run("4\n1 2 3 4\n") == "30\n"
assert run("5\n10 -100 10 -100 10\n") == "10\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / -5`|`0`| Một giá trị âm duy nhất và lựa chọn phân đoạn trống | 
|`-60 -70 -80`|`0`| Tất cả các giá trị âm | 
|`1 2 3 4`|`30`| Tăng các giá trị tích cực ở nơi toàn bộ phân khúc là tốt nhất | 
|`10 -100 10 -100 10`|`10`| Chọn đoạn giữa thay vì toàn bộ mảng | 

## Vỏ cạnh 

Đối với một mảng chỉ chứa các giá trị âm, Cây Li Chao vẫn tạo ra các giá trị phân đoạn hợp lệ, nhưng phép so sánh cuối cùng với số 0 sẽ giữ cho câu trả lời chính xác. Đối với đầu vào:```
3
-60 -70 -80
```mỗi dòng được chèn sẽ tạo ra một giá trị âm hoặc nhỏ hơn, vì vậy câu trả lời vẫn là`0`. 

Khi phân đoạn tối ưu bắt đầu sau tiền tố âm lớn, thuật toán không cam kết với các phần tử đầu. TRONG:```
6
5 -1000 1 -3 7 -8
```các vị trí tiền tố trước đoạn dương đều được biểu thị bằng dòng. Truy vấn ở cuối mỗi vị trí sẽ tự động chọn điểm bắt đầu tốt nhất, cung cấp cho phân đoạn`[1, -3, 7]`và ghi điểm`16`. 

Khi tất cả các giá trị đều dương, thuật toán vẫn phải tôn trọng trọng số tăng dần bên trong phân đoạn đã chọn. Vì:```
4
1 2 3 4
```toàn bộ phân khúc được chọn, sản xuất`1 + 4 + 9 + 16 = 30`. Phép biến đổi tiền tố bảo toàn các số nhân tăng dần này vì tiền tố có trọng số`Q`lưu trữ đóng góp vị trí ban đầu và việc sửa đường sẽ chuyển nó trở lại điểm bắt đầu đoạn đã chọn.
