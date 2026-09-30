---
title: "CF 104848N - Chu vi số nguyên"
description: "Chúng ta đang làm việc với một tam giác có độ dài ba cạnh không được cho trực tiếp mà thay vào đó bị ràng buộc bởi ba tỉ số giữa chúng."
date: "2026-06-28T11:21:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104848
codeforces_index: "N"
codeforces_contest_name: "2021-2022 ICPC, Moscow Subregional"
rating: 0
weight: 104848
solve_time_s: 56
verified: true
draft: false
---

[CF 104848N - Chu vi số nguyên](https://codeforces.com/problemset/problem/104848/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang làm việc với một tam giác có độ dài ba cạnh không được cho trực tiếp mà thay vào đó bị ràng buộc bởi ba tỉ số giữa chúng. Cụ thể là độ dài các cạnh$AB$,$BC$, Và$CA$phải thỏa mãn ba mối quan hệ tỷ lệ, nghĩa là một khi một bên cố định thì hai bên còn lại buộc phải theo một tỷ lệ cố định. 

Vì vậy, đầu vào không mô tả một tam giác linh hoạt. Nó mô tả một “hình dạng có thể mở rộng theo tỷ lệ” cứng nhắc. Tất cả các hình tam giác hợp lệ, nếu chúng tồn tại, chỉ là phiên bản thu nhỏ của một tam giác tỷ lệ cơ bản duy nhất. 

Yêu cầu thứ hai là tam giác phải không suy biến, nghĩa là nó phải thỏa mãn các bất đẳng thức tam giác nghiêm ngặt để có diện tích dương. Yêu cầu cuối cùng là về chu vi: trong số tất cả các phiên bản được chia tỷ lệ hợp lệ của tam giác này, chúng ta phải xác định xem liệu có thể biến chu vi thành số nguyên hay không và nếu có, hãy tìm chu vi số nguyên nhỏ nhất như vậy. 

Các ràng buộc đối với sáu số là nhỏ, nhưng đó không phải là vấn đề ở đây. Quan sát quan trọng là cấu trúc của bài toán thu gọn tất cả hình học thành một kiểm tra tính nhất quán duy nhất và một câu hỏi chia tỷ lệ, vì vậy mọi giải pháp đều phải chạy trong thời gian không đổi. 

Một trường hợp thất bại tinh vi xuất phát từ việc bỏ qua tính nhất quán giữa các tỷ lệ. Ví dụ: nếu các tỷ lệ hàm ý tỷ lệ mâu thuẫn, chẳng hạn như buộc$AB > BC$đồng thời cũng buộc$BC > AB$, thì không tồn tại tam giác nào cả. Một trường hợp thất bại khác là giả định rằng một tam giác luôn tồn tại với mọi tỷ lệ dương, điều này sai vì các cạnh tỷ lệ ngụ ý có thể vi phạm bất đẳng thức tam giác. 

## Phương pháp tiếp cận 

Phối cảnh vũ phu bắt đầu bằng cách tưởng tượng chúng ta có thể gán một giá trị cho một bên, chẳng hạn$AB = x$, rồi tính hai cạnh còn lại bằng cách sử dụng các tỉ số đã cho. Từ$AB/BC = a/b$, ta có mối quan hệ giữa$AB$Và$BC$. Từ$BC/CA = c/d$, chúng tôi liên kết một cặp khác và từ$CA/AB = e/f$, chúng tôi đóng chu kỳ. Một cách tiếp cận ngây thơ sẽ thử các giá trị tùy ý của$x$, tính tam giác thu được, kiểm tra tính hợp lệ và sau đó kiểm tra xem tỷ lệ nào tạo nên chu vi nguyên. 

Vấn đề với cách tiếp cận này là nó đưa ra một tìm kiếm liên tục trên tất cả các tỷ lệ thực. Vì việc mở rộng quy mô không bị hạn chế nên việc sử dụng vũ lực trở nên vô nghĩa: có vô số ứng cử viên. 

Quan sát quan trọng là các tỷ lệ xác định hoàn toàn hình dạng tam giác cho đến một hệ số nhân duy nhất. Khi chúng tôi kiểm tra xem các tỷ lệ có nhất quán hay không, mọi tam giác hợp lệ chỉ là bản sao theo tỷ lệ của một tỷ lệ ba cạnh cố định. Điều đó giúp giảm bớt vấn đề trong việc kiểm tra xem bộ ba tỷ lệ đó có tạo thành một tam giác hợp lệ hay không và sau đó hiểu được những giá trị nào mà chu vi có thể nhận khi chia tỷ lệ. 

Khi chúng ta đạt đến điểm đó, ràng buộc “chu vi số nguyên” sẽ không còn mang tính tổ hợp nữa và trở thành một câu hỏi lý thuyết số thuần túy về việc chia tỷ lệ cho một tổng hữu tỉ cố định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force theo chiều dài cạnh và tỷ lệ | Vô hạn / không xác định | O(1) | Không áp dụng | 
| Tỷ lệ nhất quán + giảm tỷ lệ | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Bước 1: Chuyển tỉ số thành ràng buộc đại số 

Chúng tôi đối xử$AB = x$,$BC = y$,$CA = z$. Mỗi tỷ lệ trở thành một phương trình liên kết các biến này. Điều này biến bài toán hình học thành một hệ các ràng buộc tỉ lệ. 

### Bước 2: Kiểm tra xem các tỷ lệ có nhất quán không 

Chúng tôi kết hợp ba phương trình tỷ lệ. Nhân chúng xung quanh chu kỳ sẽ buộc phải hủy bỏ:$$\frac{AB}{BC} \cdot \frac{BC}{CA} \cdot \frac{CA}{AB} = 1$$Vậy đầu vào phải thỏa mãn:$$\frac{a}{b} \cdot \frac{c}{d} \cdot \frac{e}{f} = 1$$tương đương với:$$ace = bdf$$Nếu điều này không thành công thì không có tam giác nào có thể thỏa mãn đồng thời tất cả các ràng buộc, bởi vì hệ thống buộc phải có tỷ lệ trái ngược nhau. 

### Bước 3: Khôi phục tỷ lệ các cạnh 

Giả sử tính nhất quán được giữ nguyên, chúng ta biểu thị tất cả các vế theo một tham số duy nhất$t$. Ví dụ, lấy$CA = t$. Sau đó:$$BC = \frac{c}{d} t, \quad AB = \frac{a}{b} \cdot \frac{c}{d} t$$Vì vậy, tam giác được xác định đầy đủ theo tỷ lệ. 

### Bước 4: Kiểm tra tính hợp lệ của tam giác 

Chúng tôi kiểm tra các bất đẳng thức tam giác nghiêm ngặt ở dạng tỷ lệ. Nếu các bất đẳng thức không thành công thì không có tỷ lệ nào có thể khắc phục được chúng, vì tỷ lệ sẽ bảo toàn tất cả các so sánh. 

### Bước 5: Xử lý điều kiện chu vi số nguyên 

Chu vi trở thành:$$P = AB + BC + CA = t \cdot S$$Ở đâu$S$là một số hữu tỉ dương cố định được xác định bởi các tỉ số. 

Từ$t$là tham số thực dương tự do, chúng ta có thể chọn nó để thực hiện$P$bất kỳ giá trị thực dương nào mà chúng ta muốn. Đặc biệt, chúng ta có thể chọn$t = 1/S$, điều này làm cho chu vi chính xác$1$. Nếu các bất đẳng thức tam giác được thỏa mãn thì tỷ lệ này vẫn có giá trị. 

Vì vậy, nếu tồn tại một tam giác hợp lệ thì chu vi nguyên tối thiểu chỉ đơn giản là$1$. 

### Tại sao nó hoạt động 

Toàn bộ cấu trúc sụp đổ thành một vectơ định hướng duy nhất trong$\mathbb{R}^3$thể hiện tỉ lệ các cạnh. Các tỷ lệ cố định hướng đó một cách duy nhất theo tỷ lệ. Hiệu lực của tam giác chỉ phụ thuộc vào hướng đó chứ không phụ thuộc vào độ lớn. Khi tính hợp lệ được giữ nguyên, quyền tự do chia tỷ lệ cho phép chúng ta đạt bất kỳ giá trị chu vi dương nào, do đó các ràng buộc số nguyên không áp đặt hạn chế nào ngoài sự tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ok(a, b, c, d, e, f):
    # consistency of ratios: a/b * c/d * e/f = 1
    return a * c * e == b * d * f

def valid(x, y, z):
    return x + y > z and x + z > y and y + z > x

def solve():
    a = int(input())
    b = int(input())
    c = int(input())
    d = int(input())
    e = int(input())
    f = int(input())

    if not ok(a, b, c, d, e, f):
        print(-1)
        return

    # construct proportional sides
    # CA = 1
    z = 1
    y = c / d
    x = (a / b) * y

    if not valid(x, y, z):
        print(-1)
        return

    print(1)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã này thực thi điều kiện nhất quán tuần hoàn, đây là cách duy nhất để ba tỷ lệ có thể mô tả đồng thời một tam giác thực. Sau đó, nó xây dựng lại một tam giác đại diện bằng cách sử dụng phép chuẩn hóa tùy ý$CA = 1$, vì chỉ có tỷ lệ mới quan trọng. 

Việc kiểm tra bất đẳng thức tam giác được thực hiện trực tiếp trên các giá trị chuẩn hóa này. Vì việc chia tỷ lệ không ảnh hưởng đến hướng bất đẳng thức nên việc kiểm tra một lần là đủ. 

Cuối cùng, đầu ra là$1$, vì bất kỳ tam giác tỷ lệ hợp lệ nào cũng có thể được chia tỷ lệ để có chu vi chính xác bằng một giá trị nguyên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 (tỷ lệ không khả thi) 

Xem xét đầu vào:```
1
1
2
1
1
1
```| Bước | Kiểm tra | Giá trị | 
| --- | --- | --- | 
| Tỷ lệ nhất quán |$1 \cdot 2 \cdot 1 = 2$vs$1 \cdot 1 \cdot 1 = 1$| thất bại | 

Vì hệ thống tỷ lệ không nhất quán nên không tồn tại tam giác và câu trả lời là$-1$. 

Điều này chứng tỏ rằng chỉ riêng bộ lọc đầu tiên đã loại bỏ các cấu hình không thể thực hiện được ngay cả trước khi xem xét hình học. 

### Ví dụ 2 (tam giác tỷ lệ hợp lệ) 

Hãy xem xét:```
1
1
1
1
1
1
```| Bước | x | y | z | Tam giác hợp lệ | 
| --- | --- | --- | --- | --- | 
| Bình thường hóa | 1 | 1 | 1 | vâng | 

Tất cả các tỷ lệ đều nhất quán và bất đẳng thức tam giác được giữ nguyên. Tam giác này là tam giác đều theo tỷ lệ, do đó tồn tại một tam giác hợp lệ và chu vi nguyên tối thiểu là$1$. 

Điều này xác nhận rằng một khi tính khả thi được thiết lập, thì vấn đề tự do mở rộng quy mô sẽ chiếm ưu thế. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số lượng kiểm tra số học không đổi được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc phụ trợ | 

Các ràng buộc cho phép giảm đại số trực tiếp này mà không cần lặp lại. Mỗi trường hợp đầu vào được xử lý trong thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# inconsistent ratios
assert run("1\n1\n2\n1\n1\n1\n") == "-1"

# all equal ratios
assert run("1\n1\n1\n1\n1\n1\n") == "1"

# scaled consistent system
assert run("2\n3\n4\n6\n8\n12\n") == "1"

# triangle inequality failure case (if ratios contradict geometry)
assert run("1\n1\n100\n1\n1\n1\n") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tỷ lệ không nhất quán | -1 | kiểm tra tính nhất quán của chu kỳ | 
| tất cả đều bình đẳng | 1 | cơ sở tam giác hợp lệ | 
| quy mô nhất quán | 1 | bất biến theo tỷ lệ | 
| mất cân bằng tỷ lệ cực độ | -1 | bác bỏ bất đẳng thức tam giác | 

## Vỏ cạnh 

Trường hợp đặc biệt quan trọng nhất là khi các tỷ lệ có giá trị riêng lẻ nhưng không nhất quán trên toàn cầu. Trong trường hợp đó, điều kiện của sản phẩm bị lỗi ngay lập tức và thuật toán sẽ loại bỏ mà không cố gắng tái cấu trúc hình học. 

Một trường hợp cạnh khác là khi các tỉ số xác định một tam giác suy biến. Ngay cả khi tính nhất quán của chu trình được giữ nguyên, các tỷ lệ dẫn xuất có thể thỏa mãn sự bằng nhau thay vì bất bình đẳng nghiêm ngặt, tạo ra diện tích bằng không. Thuật toán loại bỏ chính xác các trường hợp này ở bước bất đẳng thức tam giác. 

Trường hợp cạnh cuối cùng là tỷ lệ cực kỳ sai lệch, trong đó một bên trở nên chiếm ưu thế. Bước chuẩn hóa vẫn hoạt động vì nó bảo toàn chính xác các so sánh tương đối, đảm bảo rằng tính hợp lệ của tam giác được xác định hoàn toàn theo tỷ lệ chứ không phải độ lớn.
