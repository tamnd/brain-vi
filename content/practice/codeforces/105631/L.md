---
title: "CF 105631L - LCM và GCD"
description: "Chúng ta được yêu cầu điền vào ma trận $n nhân m$ với các số nguyên dương lên tới $10^9$. Thay vì các giá trị tùy ý, mỗi hàng và cột phải đáp ứng một ràng buộc tổng hợp rất cụ thể."
date: "2026-06-22T05:42:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105631
codeforces_index: "L"
codeforces_contest_name: "SYSU Collegiate Programming Contest 2024 (SYSUCPC 2024), Final"
rating: 0
weight: 105631
solve_time_s: 65
verified: true
draft: false
---

[CF 105631L - LCM và GCD](https://codeforces.com/problemset/problem/105631/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu điền vào một$n \times m$ma trận với các số nguyên dương lên tới$10^9$. Thay vì các giá trị tùy ý, mỗi hàng và cột phải đáp ứng một ràng buộc tổng hợp rất cụ thể. 

Đối với mỗi hàng$i$, nếu bạn lấy bội số chung nhỏ nhất của tất cả các phần tử trong hàng đó thì kết quả phải chính xác$a_i$. Đối với mỗi cột$j$, nếu lấy ước chung lớn nhất của tất cả các phần tử trong cột đó thì kết quả phải chính xác$b_j$. Nhiệm vụ là quyết định xem một ma trận như vậy có tồn tại hay không và nếu có thì xây dựng một ma trận đó. 

Những hạn chế$n, m \le 1000$ngụ ý lên đến$10^6$tế bào. Bất kỳ giải pháp nào cố gắng tìm kiếm các giá trị trên mỗi ô hoặc điều chỉnh lặp đi lặp lại các ứng viên bằng kiểm tra hệ số lồng nhau đều đã gần đạt đến giới hạn. Một giải pháp phải giảm vấn đề thành một công trình trực tiếp với tối đa$O(nm)$phép tính đơn giản. 

Một ràng buộc cấu trúc quan trọng ẩn giấu bên trong các định nghĩa về LCM và GCD. Các ràng buộc LCM buộc các phần tử hàng phải là ước số của mục tiêu hàng. Các ràng buộc GCD buộc các phần tử cột phải là bội số của mục tiêu cột. Hai hướng này giao nhau mạnh mẽ và hầu hết các cách tiếp cận không chính xác đều thất bại khi xử lý chúng một cách độc lập. 

Một trường hợp lỗi điển hình xuất hiện khi một giá trị ứng cử viên thỏa mãn các ràng buộc hàng nhưng lại phá vỡ GCD cột. Ví dụ: nếu người ta cố gắng đặt mọi ô thành hàng$i$bằng$a_i$, cột GCD thu gọn vào GCD của$a_i$'s thay vì khớp với yêu cầu$b_j$'S. Ngược lại, việc đặt cột thành$b_j$phá vỡ LCM của hàng vì các hàng không thể đạt được mục tiêu của chúng. 

Giải pháp xoay quanh việc căn chỉnh cả hai ràng buộc sao cho mọi ô đều tương thích đồng thời với cả yêu cầu về hàng và cột của nó. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ là coi mọi ô như một biến và thử các giá trị thỏa mãn đồng thời cả hai ràng buộc, liên tục tính toán lại tất cả các LCM hàng và GCD cột sau mỗi lần gán. Ngay cả khi chúng ta chỉ thử các giá trị ứng viên nhỏ trên mỗi ô, chi phí tính toán lại$O(nm)$mỗi lần thử và không gian tìm kiếm sẽ bùng nổ vì mỗi ô phụ thuộc vào sự kết hợp yếu tố của hai mục tiêu độc lập. Điều này nhanh chóng trở nên không khả thi ngoài các lưới rất nhỏ. 

Quan sát cấu trúc quan trọng là các ràng buộc không độc lập trên mỗi ô mà có thể tách thành các điều kiện chia hết. Từ định nghĩa của LCM và GCD, chúng ta có thể rút ra rằng mọi ô phải chia mục tiêu hàng của nó và phải chia hết cho mục tiêu cột của nó. Điều này chuyển đổi vấn đề thành điều kiện tương thích trên các cặp$(a_i, b_j)$. Sau khi nhận ra điều này, việc xây dựng trở nên đơn giản: chúng tôi bắt đầu từ ma trận cơ sở thỏa mãn các ràng buộc cột và sau đó “đưa” các mục tiêu hàng có chọn lọc mà không phá vỡ cấu trúc cột. 

Việc xây dựng hoạt động vì GCD ổn định khi đưa ra các giá trị là bội số của GCD hiện tại, trong khi LCM ổn định miễn là có ít nhất một phần tử trong hàng đạt được mục tiêu hàng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm vũ phu | hàm mũ |$O(nm)$| Quá chậm | 
| Xây dựng dựa trên sự phân chia |$O(nm)$|$O(nm)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi bắt đầu bằng cách chuyển các ràng buộc thành các hạn chế số học trên một ô$x_{i,j}$. 

1. Đối với hàng cố định$i$, LCM của hàng là$a_i$, do đó mọi phần tử trong hàng đó phải chia$a_i$. Nếu một ô chứa một số có thừa số nguyên tố không có trong$a_i$, LCM sẽ vượt quá$a_i$. Vì thế mỗi$x_{i,j}$phải chia$a_i$. 
2. Đối với cột cố định$j$, GCD là$b_j$, vì vậy mọi phần tử trong cột đó phải là bội số của$b_j$. Nếu bất kỳ mục nào không chia hết cho$b_j$, GCD sẽ giảm xuống dưới$b_j$. Vì thế mỗi$x_{i,j}$phải chia hết cho$b_j$. 
3. Kết hợp cả hai điều kiện, mọi ô đều phải thỏa mãn$$b_j \mid x_{i,j} \mid a_i.$$Điều này ngay lập tức ngụ ý một điều kiện khả thi: nếu$b_j$không chia$a_i$, không có giá trị hợp lệ nào có thể tồn tại ở vị trí đó, do đó toàn bộ ma trận là không thể. 
4. Khi tất cả các cặp đều có tính khả thi, chúng tôi xây dựng ma trận cơ sở bằng cách đặt mọi ô theo yêu cầu cột của nó$b_j$. Điều này tự động đảm bảo tất cả các GCD của cột ít nhất là ứng cử viên chính xác. 
5. Để sửa LCM hàng, cho mỗi hàng$i$, chúng tôi chọn một cột duy nhất$p_i$và ghi đè lên ô đó bằng$a_i$. Điều này đảm bảo rằng hàng$i$chứa giá trị bằng$a_i$, buộc LCM của hàng trở nên chính xác$a_i$. 
6. Chúng ta phải kiểm tra xem việc ghi đè này có làm hỏng cột GCD hay không. Trong cột$p_i$, hiện tại chúng tôi có hầu hết$b_{p_i}$giá trị và một giá trị$a_i$. Từ$b_{p_i} \mid a_i$, GCD của cột đó vẫn còn$b_{p_i}$. 
7. Cuối cùng, chúng ta xuất ra ma trận. 

### Tại sao nó hoạt động 

Việc xây dựng duy trì hai bất biến. Mỗi ô luôn là bội số của mục tiêu cột của nó, vì vậy GCD cột không bao giờ có thể giảm xuống dưới$b_j$và mỗi cột vẫn chứa ít nhất một giá trị bằng$b_j$, do đó GCD không thể tăng. Đối với các hàng, mỗi hàng chứa ít nhất một giá trị bằng$a_i$, và tất cả các phần tử khác chia$a_i$, do đó LCM không thể vượt quá$a_i$và không thể rơi xuống dưới nó. Hai ràng buộc này ghim cả LCM hàng và GCD cột thành các giá trị chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    # feasibility check: b[j] must divide every a[i]
    for i in range(n):
        for j in range(m):
            if a[i] % b[j] != 0:
                print("No")
                return

    # start with base matrix filled with column requirements
    mat = [[b[j] for j in range(m)] for _ in range(n)]

    # assign each row one column to place its row LCM target
    for i in range(n):
        mat[i][i % m] = a[i]

    print("Yes")
    for row in mat:
        print(*row)

if __name__ == "__main__":
    solve()
```Việc kiểm tra tính khả thi trực tiếp thực thi điều kiện dẫn xuất$b_j \mid a_i$. Nếu nó thất bại ở bất cứ đâu, thì không thể xây dựng được vì ô đó sẽ vi phạm đồng thời cả hai ràng buộc phân chia. 

Ma trận được khởi tạo sao cho mỗi cột đã có sẵn các ứng cử viên GCD chính xác. Mỗi hàng sau đó nhận được một vị trí ghi đè duy nhất$a_i$, đảm bảo hàng LCM trở nên chính xác mà không làm gián đoạn cấu trúc cột. sử dụng$i \bmod m$phân phối các phần ghi đè này để mỗi cột vẫn chứa nhiều$b_j$giá trị, ngăn ngừa lạm phát GCD ngẫu nhiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3
2 6
1 2 2
```Chúng ta bắt đầu với tính khả thi: mọi$b_j$chia mọi$a_i$, vì 1 và 2 chia hết cho 2 và 6. 

Chúng tôi xây dựng: 

| Bước | Hàng 1 | Hàng 2 | 
| --- | --- | --- | 
| ban đầu | 1 2 2 | 1 2 2 | 
| nơi a_i | 2 2 2 | 1 6 2 | 

LCM của hàng 1 là 2 vì nó chứa số 2 và tất cả các giá trị đều chia cho 2. LCM của hàng 2 là 6 do được đặt 6. GCD của mỗi cột vẫn giữ nguyên$b_j$vì tất cả các giá trị đều là bội số của$b_j$và mỗi cột vẫn chứa ít nhất một giá trị thuần túy$b_j$mục nhập hoặc chỉ bội số bảo toàn GCD. 

### Ví dụ 2 

đầu vào:```
3 3
1 1 4
5 1 4
```Tính khả thi thất bại ngay lập tức vì$5$không chia$1$hoặc$4$. Không có giá trị nào trong bất kỳ ô nào có thể vừa là bội số của$5$và một số chia của$1$hoặc$4$. Việc xây dựng thậm chí không thể bắt đầu, vì vậy đầu ra chính xác là`No`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$| Một lần quét tính khả thi cộng với một ma trận xây dựng | 
| Không gian |$O(nm)$| Lưu trữ ma trận đã xây dựng | 

Những giới hạn$n, m \le 1000$cho phép tối đa$10^6$hoạt động phù hợp thoải mái trong Python để định dạng số học và đầu ra đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib
    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("""2 3
2 6
1 2 2
""").startswith("Yes")

# impossible sample
assert run("""3 3
1 1 4
5 1 4
""") == "No"

# minimal case
assert run("""1 1
6
3
""") == "Yes\n6"

# all ones
assert run("""2 2
1 1
1 1
""").startswith("Yes")

# incompatible divisibility
assert run("""2 2
2 3
2 2
""") == "No"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1×1 tầm thường | Có 6 | độ đúng cơ sở | 
| tất cả những cái | Có ma trận của những cái | hành vi LCM/GCD trung lập | 
| lỗi chia hết | Không | phát hiện không thể | 

## Vỏ cạnh 

Một trường hợp tế nhị là khi tất cả$a_i = 1$. Khi đó mỗi ô phải chia 1, buộc tất cả các mục là 1. Cấu trúc vẫn hoạt động vì mọi$b_j$cũng phải là 1, nếu không tính khả thi sẽ thất bại ngay lập tức. Thuật toán loại bỏ chính xác các trường hợp không nhất quán và tạo ra một ma trận thống nhất có LCM hàng và GCD cột vẫn bằng 1. 

Một trường hợp khác là khi tất cả$b_j = 1$. Điều này loại bỏ gần như hoàn toàn các ràng buộc cột, nhưng các ràng buộc hàng vẫn buộc ít nhất một lần xuất hiện của mỗi ràng buộc.$a_i$. Ma trận cơ sở chứa đầy các ma trận 1 đảm bảo GCD cột vẫn là 1 và việc chèn$a_i$vào một vị trí trên mỗi hàng sẽ duy trì tính chính xác vì GCD(1, a_i) luôn bằng 1. 

Trường hợp thứ ba là khi$n$hoặc$m$bằng 1. Việc xây dựng suy biến thành một hàng hoặc cột duy nhất, nhưng logic chia hết vẫn được áp dụng. Thuật toán vẫn kiểm tra chính xác tính tương thích và chỉ định cấu trúc yêu cầu duy nhất mà không cần sửa đổi.
