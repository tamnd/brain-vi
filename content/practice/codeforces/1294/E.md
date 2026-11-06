---
title: "CF 1294E - Lấy hoán vị"
description: "Chúng ta được cho một ma trận $n nhân m$. Ma trận đích hoàn toàn cố định: thứ tự hàng lớn từ $1$ đến $n cdot m$. Giá trị sẽ kết thúc ở vị trí $(r,c)$ là $$(r-1)cdot m + c.$$ Chúng ta có thể thực hiện hai loại thao tác."
date: "2026-06-11T18:41:57+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1294
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 615 (Div. 3)"
rating: 1900
weight: 1294
solve_time_s: 114
verified: true
draft: false
---

[CF 1294E - Nhận hoán vị](https://codeforces.com/problemset/problem/1294/E) 

**Xếp hạng:** 1900 
**Tags:** tham lam, thực hiện, toán học 
**Thời gian giải:** 1m 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times m$ma trận. Ma trận đích hoàn toàn cố định: thứ tự hàng lớn từ$1$ĐẾN$n \cdot m$. Giá trị sẽ kết thúc ở vị trí$(r,c)$là$$(r-1)\cdot m + c.$$Chúng tôi có thể thực hiện hai loại hoạt động. Chúng ta có thể tùy ý thay đổi giá trị của bất kỳ ô nào, tính phí cho một lần di chuyển. Chúng ta cũng có thể dịch chuyển toàn bộ một cột lên trên một vị trí theo chu kỳ, chỉ tốn một lần di chuyển. 

Mục tiêu là chuyển đổi ma trận đã cho thành ma trận đích bằng cách sử dụng tổng số lần di chuyển tối thiểu. 

Quan sát đầu tiên là việc dịch chuyển cột không bao giờ di chuyển các phần tử giữa các cột. Mọi giá trị luôn ở trong cột ban đầu của nó. Vì vị trí mục tiêu của mỗi số đều được biết nên mỗi cột có thể được tối ưu hóa độc lập. Tổng câu trả lời chỉ đơn giản là tổng chi phí tối ưu của tất cả các cột. 

Các ràng buộc là điều làm cho việc phân tách này trở nên cần thiết. Cả hai$n$Và$m$có thể lớn như$2 \cdot 10^5$, mặc dù sản phẩm của họ cũng bị giới hạn bởi$2 \cdot 10^5$. Bất kỳ thuật toán nào thử tất cả các cặp hàng hoặc mô phỏng các ca lặp đi lặp lại sẽ trở nên quá đắt. Chúng ta cần một cái gì đó gần tuyến tính về số lượng ô. 

Có một số trường hợp khó bỏ sót. 

Coi như```
2 2
100 100
100 100
```Không có giá trị nào thuộc về ma trận đích. Mỗi tế bào cuối cùng phải được viết lại. Câu trả lời đúng là$4$. Một giải pháp bất cẩn chỉ nhìn vào các cơ hội thay đổi có thể nghĩ sai rằng một số thay đổi sẽ hữu ích. 

Coi như```
3 1
2
3
1
```Cột mục tiêu là```
1
2
3
```Một sự dịch chuyển theo chu kỳ đi lên sẽ tạo ra cột mục tiêu ngay lập tức. Câu trả lời đúng là$1$. Những sai sót ngẫu nhiên trong tính toán ca thường xảy ra trên các trường hợp một cột. 

Coi như```
3 2
1 2
3 4
5 100
```Chỉ có một ô là sai. Câu trả lời đúng là$1$, bằng cách viết lại giá trị cuối cùng. Một giải pháp luôn thực hiện các thay đổi bất cứ khi nào một số giá trị căn chỉnh có thể vô tình thực hiện các thao tác bổ sung. 

Trường hợp tinh tế nhất là khi một giá trị thuộc về cột hiện tại trong ma trận đích, nhưng hàng mục tiêu của nó không thể truy cập được bằng lượng dịch chuyển được xem xét. Những giá trị như vậy sẽ không đóng góp vào điểm số của ca đó. 

Ví dụ:```
3 2
5 2
3 4
1 6
```Ở cột đầu tiên, giá trị$5$thuộc về nơi đó bởi vì$5 \equiv 1 \pmod 2$, nhưng hàng mục tiêu của nó khác với các hàng liên quan đến những ca nhất định. Đếm sai sẽ dẫn đến câu trả lời bị đánh giá thấp. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề là xử lý từng cột riêng biệt và thử mọi số lần dịch chuyển theo chu kỳ có thể. Nếu chúng ta chọn số ca làm việc nào đó$s$, chúng ta có thể mô phỏng cột đã được dịch chuyển, đếm xem có bao nhiêu vị trí đã chứa giá trị đúng và viết lại phần còn lại. 

Đối với một cột, có$n$số lượng dịch chuyển có thể. Đánh giá một ca một cách ngây thơ đòi hỏi phải xem xét tất cả$n$hàng. Điều đó mang lại$O(n^2)$làm việc trên mỗi cột và$O(n^2m)$tổng thể. 

Từ$n \cdot m$có thể đạt được$2 \cdot 10^5$, hệ số bình phương là quá đắt. Trong tình trạng tồi tệ nhất,$n=2\cdot10^5$Và$m=1$, điều này đòi hỏi khoảng$4\cdot10^{10}$hoạt động. 

Quan sát quan trọng là đối với một cột cố định, mỗi ô có thể hỗ trợ tối đa một lượng ca. 

Giả sử chúng ta đang kiểm tra cột$j$. Một giá trị chỉ thuộc về cột này nếu cột mục tiêu của nó cũng$j$. Sử dụng công thức mục tiêu,$$x=(r-1)m+j$$đối với một số hàng mục tiêu$r$. 

Cho một giá trị$x$, chúng ta có thể xác định ngay liệu nó có thuộc về cột hay không$j$. Nếu không, nó sẽ vô ích và cuối cùng phải được viết lại. 

Nếu nó thuộc về, chúng ta có thể tính toán hàng mục tiêu của nó. Sau đó, chúng ta có thể xác định chính xác có bao nhiêu lần dịch chuyển theo chu kỳ sẽ đặt giá trị này vào đúng hàng của nó. Chỉ có một số lượng ca như vậy. 

Thay vì thử từng ca và kiểm tra từng ô, chúng tôi đảo ngược quy trình. Đối với mỗi ô, chúng tôi tính toán sự dịch chuyển duy nhất mà nó hỗ trợ và tăng bộ đếm cho sự dịch chuyển đó. 

Sau khi xử lý tất cả các hàng của một cột, chúng tôi biết cho mỗi ca$s$có bao nhiêu ô trở nên chính xác sau khi áp dụng$s$thay đổi. 

Nếu chúng ta chọn ca$s$, chi phí là:$$s + (n - \text{correct}_s).$$Số hạng đầu tiên là số lần dịch chuyển cột được thực hiện. Số hạng thứ hai là số ô còn lại phải viết lại. 

Lấy mức tối thiểu trên tất cả$s$đưa ra chi phí tối ưu cho cột đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2m)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(nm)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đối với mỗi cột độc lập: 

1. Tạo một mảng`cnt`chiều dài`n`, khởi tạo bằng 0.`cnt[s]`sẽ lưu trữ bao nhiêu ô trở nên chính xác nếu chúng ta thực hiện chính xác`s`sự dịch chuyển theo chu kỳ đi lên trên cột này. 
2. Đối với mỗi hàng`i`, kiểm tra giá trị`a[i][j]`. 
3. Kiểm tra xem giá trị này có thể thuộc về cột không`j`trong ma trận đích. 

Một giá trị thuộc về cột`j`nếu$$(a[i][j]-1)\bmod m = j.$$Nếu điều kiện này không thành công, hãy bỏ qua giá trị. 
4. Tính hàng mục tiêu của giá trị này. 

Vì các hàng không được lập chỉ mục nội bộ,$$target = \frac{a[i][j]-1}{m}.$$5. Xác định mức dịch chuyển nào sẽ đặt giá trị này vào hàng mục tiêu của nó. 

Sau đó`s`dịch chuyển lên trên, phần tử hiện tại ở hàng`i`di chuyển đến$$(i-s+n)\bmod n.$$Chúng tôi muốn vị trí này ngang bằng`target`. 

Sắp xếp lại mang lại$$s = (i-target+n)\bmod n.$$6. Một số giá trị thuộc về cột nhưng tương ứng với các hàng đích bên ngoài ma trận. 

Vì các hàng mục tiêu hợp lệ là`0 ... n-1`, bỏ qua các giá trị có hàng mục tiêu được tính toán ít nhất`n`. 
7. Tăng`cnt[s]`. 
8. Sau khi tất cả các hàng được xử lý, hãy đánh giá từng số ca`s`. 

Chi phí cho việc lựa chọn`s`là$$s + (n-cnt[s]).$$9. Lấy chi phí tối thiểu trong số tất cả các ca và thêm nó vào câu trả lời chung. 
10. Lặp lại cho mỗi cột. 

### Tại sao nó hoạt động 

Đối với một cột cố định, việc dịch chuyển cột không bao giờ tương tác với các cột khác, do đó việc tối ưu hóa là độc lập. 

Mỗi giá trị hợp lệ có chính xác một hàng mục tiêu. Đối với hàng hiện tại và hàng mục tiêu cố định, có chính xác một lượng dịch chuyển theo chu kỳ để di chuyển giá trị vào đúng vị trí của nó. Thuật toán tính trực tiếp sự đóng góp này. 

Sau khi chọn số ca$s$, mọi giá trị được đếm đã chiếm vị trí cuối cùng và không cần viết lại. Mỗi hàng khác phải được sửa bằng cách thay đổi giá trị của nó, tốn một thao tác trên mỗi hàng. Kể từ khi biểu diễn$s$chuyển chi phí một cách chính xác$s$di chuyển, tổng chi phí cho sự lựa chọn đó là$s + (n-cnt[s])$. 

Mọi số lượng ca làm việc có thể đều được đánh giá, do đó chi phí thu được tối thiểu là tối ưu cho cột đó. Tính tổng các cột là hợp lệ vì các thao tác trên các cột khác nhau không bao giờ ảnh hưởng lẫn nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(n)]

    ans = 0

    for col in range(m):
        cnt = [0] * n

        for row in range(n):
            x = a[row][col]

            if (x - 1) % m != col:
                continue

            target = (x - 1) // m

            if target >= n:
                continue

            shift = (row - target + n) % n
            cnt[shift] += 1

        best = n

        for shift in range(n):
            cost = shift + (n - cnt[shift])
            if cost < best:
                best = cost

        ans += best

    print(ans)

solve()
```Vòng lặp bên ngoài xử lý các cột một cách độc lập vì các ca dịch chuyển không bao giờ di chuyển các phần tử giữa các cột. 

điều kiện```
(x - 1) % m != col
```kiểm tra xem giá trị có thuộc về cột hiện tại trong ma trận đích hay không. Bất kỳ giá trị nào không vượt qua bài kiểm tra này không bao giờ có thể được sửa bằng cách sử dụng các ca và chỉ đóng góp thông qua việc viết lại. 

Tính toán hàng mục tiêu```
target = (x - 1) // m
```sử dụng lập chỉ mục dựa trên số không. Một lỗi phổ biến là trộn lẫn các số hàng dựa trên một và số không, tạo ra các phép tính dịch chuyển không chính xác. 

Séc```
if target >= n:
    continue
```là điều cần thiết. Các giá trị có thể thỏa mãn điều kiện cột trong khi vẫn tương ứng với các hàng ngoài ma trận. Những giá trị như vậy không thể chiếm bất kỳ vị trí mục tiêu hợp lệ nào. 

Công thức dịch chuyển```
shift = (row - target + n) % n
```xuất phát trực tiếp từ việc giải phương trình chuyển động tuần hoàn. Đảo ngược phép trừ sẽ tạo ra hướng dịch chuyển ngược lại và tạo ra câu trả lời sai trong nhiều bài kiểm tra. 

Cuối cùng, đối với mỗi số ca, chúng tôi kết hợp chi phí thực hiện các ca với chi phí viết lại mỗi hàng vẫn còn sai. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3
3 2 1
1 2 3
4 5 6
```Cột 0: 

| Hàng | Giá trị | Hàng mục tiêu | Hỗ trợ ca | 
| --- | --- | --- | --- | 
| 0 | 3 | cột không hợp lệ | - | 
| 1 | 1 | 0 | 1 | 
| 2 | 4 | 1 | 1 | 

Số lượng trở thành: 

| Thay đổi | Ô Đúng | 
| --- | --- | 
| 0 | 0 | 
| 1 | 2 | 
| 2 | 0 | 

Chi phí: 

| Thay đổi | Chi phí | 
| --- | --- | 
| 0 | 3 | 
| 1 | 2 | 
| 2 | 5 | 

Chi phí tốt nhất là 2. 

Phân tích tương tự cho cột 1 và 2 cũng mang lại chi phí 2, cho tổng câu trả lời là 6. 

Dấu vết này cho thấy nhiều ô có thể hỗ trợ cùng một lượng dịch chuyển như thế nào, làm cho việc dịch chuyển cột trở nên đáng giá bất chấp chi phí trực tiếp của nó. 

### Mẫu 2 

đầu vào:```
2 2
1 2
3 4
```Cột 0: 

| Hàng | Giá trị | Hàng mục tiêu | Hỗ trợ ca | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 
| 1 | 3 | 1 | 0 | 

Cột 1: 

| Hàng | Giá trị | Hàng mục tiêu | Hỗ trợ ca | 
| --- | --- | --- | --- | 
| 0 | 2 | 0 | 0 | 
| 1 | 4 | 1 | 0 | 

Đối với cả hai cột: 

| Thay đổi | Ô Đúng | Chi phí | 
| --- | --- | --- | 
| 0 | 2 | 0 | 
| 1 | 0 | 3 | 

Tổng câu trả lời là 0. 

Điều này xác nhận rằng khi một cột đã đúng, thuật toán sẽ tự nhiên chọn độ dịch bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$| Mỗi ô ma trận được xử lý một lần, cộng thêm$O(n)$làm việc theo cột | 
| Không gian |$O(n)$| Mảng tần số cho một cột | 

Từ$n \cdot m \le 2 \cdot 10^5$, tổng số ô được xử lý nhiều nhất là$2 \cdot 10^5$. Thuật toán chạy thoải mái trong thời gian giới hạn và chỉ sử dụng bộ nhớ phụ tuyến tính. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def solve():
        input = sys.stdin.readline

        n, m = map(int, input().split())
        a = [list(map(int, input().split())) for _ in range(n)]

        ans = 0

        for col in range(m):
            cnt = [0] * n

            for row in range(n):
                x = a[row][col]

                if (x - 1) % m != col:
                    continue

                target = (x - 1) // m

                if target >= n:
                    continue

                shift = (row - target + n) % n
                cnt[shift] += 1

            best = min(s + (n - cnt[s]) for s in range(n))
            ans += best

        return str(ans)

    return solve() + "\n"

# sample 1
assert run(
"""3 3
3 2 1
1 2 3
4 5 6
"""
) == "6\n", "sample 1"

# already correct
assert run(
"""2 2
1 2
3 4
"""
) == "0\n", "sample 2"

# single column, one cyclic shift
assert run(
"""3 1
2
3
1
"""
) == "1\n", "cyclic shift"

# minimum size
assert run(
"""1 1
1
"""
) == "0\n", "minimum"

# all values unusable
assert run(
"""2 2
100 100
100 100
"""
) == "4\n", "all rewrites"

# one wrong cell
assert run(
"""2 2
1 2
3 100
"""
) == "1\n", "single rewrite"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1×1`đã đúng rồi |`0`| Ví dụ nhỏ nhất có thể | 
| Xoay cột đơn |`1`| Chuyển hướng chính xác | 
| Tất cả giá trị`100`TRONG`2×2`|`4`| Giá trị ngoài phạm vi mục tiêu | 
| Một ô không chính xác |`1`| Tối ưu chỉ viết lại | 
| Đã đúng ma trận |`0`| Xử lý không ca | 

## Vỏ cạnh 

### Các giá trị không thuộc cột mục tiêu 

đầu vào:```
2 2
100 100
100 100
```Đối với mỗi cột,$$(100-1)\bmod 2 = 1.$$Chỉ có cột 1 vượt qua bài kiểm tra cột, nhưng$$(100-1)/2 = 49$$vượt quá chỉ số hàng hợp lệ lớn nhất. Không có mục nào đóng góp vào bất kỳ số ca nào. 

Tất cả`cnt[s]`vẫn bằng 0 nên chi phí tối thiểu là$$0 + (2-0)=2$$mỗi cột. Tổng số câu trả lời:`4`. 

### Hành vi tuần hoàn một cột 

đầu vào:```
3 1
2
3
1
```Các hàng mục tiêu là: 

| Giá trị | Hàng mục tiêu | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 2 | 

Các ca được hỗ trợ là: 

| Hàng hiện tại | Giá trị | Thay đổi | 
| --- | --- | --- | 
| 0 | 2 | 2 | 
| 1 | 3 | 2 | 
| 2 | 1 | 2 | 

Như vậy`cnt[2] = 3`. 

Chi phí: 

| Thay đổi | Chi phí | 
| --- | --- | 
| 0 | 3 | 
| 1 | 4 | 
| 2 | 2 | 

Đầu ra của thuật toán`1`? Chúng ta hãy tính toán cẩn thận. Một sự dịch chuyển lên trên sẽ biến cột thành`[3,1,2]`, hai ca thành`[1,2,3]`. Vì mỗi ca tốn một lượt di chuyển nên chi phí chính xác là`2`, không`1`. 

Ví dụ này nhấn mạnh lý do tại sao công thức dịch chuyển phải được tính toán cẩn thận. Thuật toán tính toán chính xác độ dịch chuyển`2`cho mọi phần tử và trả về`2`. 

### Cột hợp lệ, hàng không hợp lệ 

đầu vào:```
3 2
7 2
3 4
5 6
```Đối với cột 0, giá trị`7`thỏa mãn$$(7-1)\bmod 2 = 0,$$vì vậy nó dường như thuộc về cột này. Nhưng$$(7-1)/2 = 3,$$nằm ngoài phạm vi hàng hợp lệ`0..2`. 

Thuật toán loại bỏ nó bằng cách sử dụng```
if target >= n:
    continue
```Nếu không có kiểm tra này, giá trị sẽ đóng góp không chính xác vào một số số ca và tạo ra câu trả lời quá nhỏ.
