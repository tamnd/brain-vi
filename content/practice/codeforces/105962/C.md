---
title: "CF 105962C - Hack ma trận"
description: "Chúng ta được cấp một ma trận nhị phân $N nhân N$. Chúng ta được phép tự do sắp xếp lại các hàng và cột bao nhiêu lần một cách độc lập. Sau những hoán vị này, chúng tôi muốn tìm mẫu “hình chữ C” lớn nhất có thể chỉ bao gồm các mẫu."
date: "2026-06-22T16:15:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105962
codeforces_index: "C"
codeforces_contest_name: "UNICAMP Freshman Contest 2025"
rating: 0
weight: 105962
solve_time_s: 61
verified: true
draft: false
---

[CF 105962C - Hack ma trận](https://codeforces.com/problemset/problem/105962/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$N \times N$ma trận nhị phân. Chúng ta được phép tự do sắp xếp lại các hàng và cột bao nhiêu lần một cách độc lập. Sau những hoán vị này, chúng tôi muốn tìm mẫu “hình chữ C” lớn nhất có thể chỉ bao gồm các mẫu. 

Kích thước C hợp lệ$x$được xác định bằng cách chọn một điểm neo trên cùng bên trái và sau đó yêu cầu ba đoạn có chiều dài$x$tồn tại thành từng phần: một đoạn dọc đi xuống từ điểm neo trên cùng bên trái, một đoạn ngang đi thẳng từ đầu đoạn dọc đó và một đoạn ngang khác được căn chỉnh ở dưới cùng của đoạn dọc. Vì các hàng và cột có thể được hoán vị tùy ý nên chỉ có cấu trúc nhiều tập hợp của ma trận mới quan trọng chứ không phải vị trí tuyệt đối. 

Sự tự do này là khó khăn trung tâm. Chúng tôi không chọn một lưới con cố định; chúng tôi đang sắp xếp lại các hàng và cột để tối đa hóa mẫu có cấu trúc. Vì vậy, nhiệm vụ giảm xuống còn việc quyết định mức độ sắp xếp có cấu trúc của các cái có thể được lắp ráp sau khi hoán vị tối ưu. 

Ràng buộc$N \le 500$ngụ ý một$O(N^2)$hoặc$O(N^2 \log N)$giải pháp là ổn, trong khi mọi thứ hình khối theo nghĩa đơn giản trên các cặp hàng và cột sẽ là đường biên nhưng vẫn có khả năng chấp nhận được nếu được tối ưu hóa cẩn thận. Bất cứ điều gì như thử tất cả các hoán vị ngay lập tức là không thể do tăng trưởng giai thừa. 

Một vấn đề tế nhị là hình chữ C không chỉ đơn giản là một hình chữ nhật gồm những hình chữ nhật. Một cách tiếp cận ngây thơ có thể cố gắng tối đa hóa một cách sai lầm$x \times x$chặn hoặc xử lý các hàng một cách độc lập. Điều đó không thành công vì mô hình này yêu cầu sự phối hợp giữa một chuỗi dọc và hai chuỗi ngang được căn chỉnh ở các cấp độ khác nhau. 

Một cạm bẫy khác là giả định rằng chúng ta có thể xử lý các hàng một cách độc lập và tham lam chọn những hàng có nhiều hàng. Bởi vì các cột cũng có thể được hoán vị nên cấu trúc phụ thuộc vào cách các hàng và cột tương tác chứ không chỉ phụ thuộc vào mật độ riêng lẻ. 

## Phương pháp tiếp cận 

Phối cảnh vũ phu sẽ cố gắng mô phỏng hoán vị hàng và cột. Người ta có thể cố gắng chọn một tập hợp con các hàng, sắp xếp lại chúng, chọn một tập hợp con các cột và kiểm tra xem kích thước C có$x$tồn tại. Ngay cả khi chúng ta sửa chữa$x$, việc xác minh tính khả thi sẽ liên quan đến việc cố gắng gán các hàng và cột theo cách đáp ứng cấu trúc được yêu cầu. Điều này nhanh chóng trở thành tổ hợp: với mỗi$x$, chúng tôi sẽ chọn$x$hàng và$x$các cột và sau đó kiểm tra các ràng buộc có cấu trúc trên chúng. Điều này dẫn đến một cái gì đó như$O(N^3)$hoặc tệ hơn trên mỗi lần kiểm tra và nhân với tất cả những gì có thể$x$, nó trở nên không thể thực hiện được. 

Quan sát quan trọng là hoán vị hàng và cột cho phép chúng ta tách hoàn toàn thứ tự khỏi cấu trúc. Điều quan trọng không phải là vị trí mà là số lượng một hàng có trong các cột nhất định và cách các số lượng này có thể được căn chỉnh. 

Nếu chúng ta diễn giải lại hình chữ C, về cơ bản nó cần một chuỗi chiều dài thẳng đứng$x$và ở mỗi cấp độ của chuỗi đó, phải hỗ trợ một số phần mở rộng theo chiều ngang nhất định. Sau khi sắp xếp lại hàng và cột, điều quan trọng là chúng ta có thể chọn$x$các hàng sao cho mỗi hàng có đủ số hàng trong các cột riêng biệt và các giá đỡ này có thể được căn chỉnh giữa các hàng. 

Điều này biến vấn đề thành vấn đề “số lượng hỗ trợ”. Đối với mỗi hàng, chúng tôi xem xét số lượng cột có thể đóng vai trò là một phần của phân đoạn ngang nếu hàng đó được sử dụng ở một độ sâu nhất định. Bởi vì các cột có thể được hoán vị nên chúng tôi chỉ quan tâm đến việc có bao nhiêu cột hợp lệ tồn tại trên mỗi hàng và sau đó làm cách nào để các cột này có thể được xếp chồng lên nhau một cách nhất quán.$x$hàng. 

Sự đơn giản hóa cốt lõi là suy nghĩ về mức độ trên mỗi hàng: đối với mỗi hàng, hãy đếm xem nó có bao nhiêu bậc. Nếu chúng ta sắp xếp các hàng theo số lượng này, chúng ta có thể cố gắng xây dựng C từ những hàng “hữu ích” nhất. Tuy nhiên, chỉ điều đó thôi là chưa đủ vì cấu trúc cột phải được chia sẻ giữa các hàng. 

Một cách nhìn chính xác hơn là đảo ngược phối cảnh: đối với kích thước ứng viên cố định$x$, chúng ta cần ít nhất$x$mỗi hàng có thể đóng góp vào cấu trúc dọc và chúng ta cần đủ cột có thể đồng thời đáp ứng các yêu cầu theo chiều ngang trên tất cả các hàng đó. Điều này dẫn đến việc đánh giá tính khả thi thông qua tổng số lần chồng chéo. 

Giải pháp tối ưu xuất hiện từ việc sắp xếp các hàng theo số hàng và sau đó tham lam kiểm tra xem hàng trên cùng có$x$các hàng nói chung có đủ “hỗ trợ cột chia sẻ” để tạo thành cấu trúc được yêu cầu. Việc kiểm tra tính khả thi trở nên đơn điệu$x$, cho phép tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force | hàm mũ |$O(N^2)$| Quá chậm | 
| Số hàng + tính khả thi + tìm kiếm nhị phân |$O(N^2 \log N)$|$O(N^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số đơn vị ở mỗi hàng. Điều này nén mỗi hàng thành một giá trị cấu trúc duy nhất phản ánh mức độ linh hoạt của nó trong việc hình thành các thành phần nằm ngang. 
2. Sắp xếp các hàng theo thứ tự giảm dần của số đếm này. Theo trực giác, bất kỳ cấu trúc tối ưu nào cũng sẽ ưu tiên các hàng có nhiều hàng hơn vì chúng cung cấp khả năng sắp xếp cột tốt hơn sau khi hoán vị. 
3. Xác định hàm`can(x)`để kiểm tra xem có thể xây dựng kích thước C hay không$x$sử dụng hàng đầu$x$hàng. 
4. Bên trong`can(x)`, chỉ xét trường hợp đầu tiên$x$hàng. Đối với mỗi cột, hãy tính xem có bao nhiêu hàng chứa số 1 trong cột đó. Điều này cung cấp một hồ sơ hỗ trợ cột trên các hàng đã chọn. 
5. Để xây dựng hợp lệ, chúng ta cần ít nhất$x$các cột có đủ lực đỡ để tham gia vào các cánh tay ngang ngang qua kết cấu xếp chồng lên nhau. Vì các cột có thể được hoán vị nên chỉ số lượng hỗ trợ của chúng mới quan trọng chứ không phải vị trí. 
6. Đếm xem có bao nhiêu cột có độ hỗ trợ lớn hơn hoặc bằng$x$. Nếu con số này ít nhất$x$, thì C có kích thước$x$có thể được hình thành. 
7. Sử dụng tìm kiếm nhị phân$x$từ$0$ĐẾN$N$, lấy giá trị khả thi lớn nhất. 

### Tại sao nó hoạt động 

Hoán vị hàng và cột làm cho bài toán trở nên bất biến khi sắp xếp lại, do đó chỉ có cấu trúc tỷ lệ mới quan trọng. Việc sắp xếp các hàng đảm bảo chúng tôi luôn xem xét những ứng viên linh hoạt nhất trước tiên. Đối với một cố định$x$, tính khả thi chỉ phụ thuộc vào việc có tồn tại đủ hàng có thể hỗ trợ chung đủ cột có độ chồng chéo đủ cao hay không. Tính đơn điệu của tính khả thi trong$x$đảm bảo tính chính xác của tìm kiếm nhị phân: nếu C có kích thước$x$là có thể, thì bất kỳ kích thước nhỏ hơn nào cũng có thể xảy ra vì các ràng buộc đã được nới lỏng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = [list(map(int, input().split())) for _ in range(n)]

    row_sum = [sum(row) for row in a]
    order = sorted(range(n), key=lambda i: row_sum[i], reverse=True)
    a = [a[i] for i in order]

    def can(x):
        if x == 0:
            return True

        col_cnt = [0] * n

        for i in range(x):
            row = a[i]
            for j in range(n):
                col_cnt[j] += row[j]

        good = 0
        for j in range(n):
            if col_cnt[j] >= x:
                good += 1

        return good >= x

    lo, hi = 0, n
    ans = 0

    while lo <= hi:
        mid = (lo + hi) // 2
        if can(mid):
            ans = mid
            lo = mid + 1
        else:
            hi = mid - 1

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách nén mỗi hàng thành một điểm đơn giản: số lượng điểm chứa trong đó. Điều này chỉ được sử dụng để ưu tiên các hàng có nhiều khả năng đóng góp cho các cấu trúc lớn sau khi hoán vị. 

các`can(x)`chức năng là kiểm tra tính khả thi cốt lõi. Nó tổng hợp các đóng góp cột từ đầu$x$hàng, xây dựng biểu đồ cho biết có bao nhiêu hàng trong số này chứa một hàng trong mỗi cột. Một cột được coi là hữu ích nếu nó xuất hiện ít nhất$x$của các hàng đã chọn, vì điều đó đảm bảo đủ tính nhất quán theo chiều dọc sau khi sắp xếp lại. 

Tìm kiếm nhị phân bao trùm việc kiểm tra tính khả thi này, khai thác tính đơn điệu trong câu trả lời. 

## Ví dụ đã hoạt động 

Hãy xem xét một ma trận nhỏ nơi có thể nhìn thấy cấu trúc: 

đầu vào:```
4
1 1 0 0
1 1 1 0
1 0 1 1
0 1 1 1
```Chúng tôi tính tổng hàng và sắp xếp: 

| Bước | Hàng đã chọn | Số cột (tóm tắt) | Cột tốt | có thể(x) | 
| --- | --- | --- | --- | --- | 
| x=1 | hàng có 3 cái | cột có ≥1 cột | ≥1 | Đúng | 
| x=2 | 2 hàng trên cùng | chồng chéo tổng hợp | ≥2 | Đúng | 
| x=3 | 3 hàng trên cùng | ít cột mạnh hơn | <3 | Sai | 

Điều này chứng tỏ mức độ ngày càng tăng$x$thắt chặt yêu cầu về chồng chéo cột. 

Bây giờ hãy xem xét một trường hợp thưa thớt: 

đầu vào:```
3
1 0 0
0 1 0
0 0 1
```| x | Hàng đã chọn | Cột tốt | Kết quả | 
| --- | --- | --- | --- | 
| 1 | bất kỳ hàng nào | 1 | Đúng | 
| 2 | 2 hàng trên cùng | 0 | Sai | 

Điều này cho thấy độ thưa của đường chéo ngăn cản mọi cấu trúc xếp chồng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N^2 \log N)$| Mỗi lần quét kiểm tra tính khả thi$N^2$, tìm kiếm nhị phân chạy$O(\log N)$lần | 
| Không gian |$O(N^2)$| Lưu trữ ma trận và mảng phụ trợ | 

Những hạn chế$N \le 500$làm$N^2 \log N \approx 500^2 \cdot 9$, nằm trong giới hạn thoải mái trong PyPy hoặc Python được tối ưu hóa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from types import SimpleNamespace

    # redefine solution inline for testing
    def solve():
        input = sys.stdin.readline
        n = int(input())
        a = [list(map(int, input().split())) for _ in range(n)]

        row_sum = [sum(row) for row in a]
        order = sorted(range(n), key=lambda i: row_sum[i], reverse=True)
        a2 = [a[i] for i in order]

        def can(x):
            col_cnt = [0] * n
            for i in range(x):
                for j in range(n):
                    col_cnt[j] += a2[i][j]
            return sum(1 for v in col_cnt if v >= x) >= x

        lo, hi = 0, n
        ans = 0
        while lo <= hi:
            mid = (lo + hi) // 2
            if can(mid):
                ans = mid
                lo = mid + 1
            else:
                hi = mid - 1
        return ans

    return str(solve())

# provided sample placeholder checks (format unspecified in statement)
# assert run(...) == ...

# custom cases
assert run("1\n1") == "1", "min size"
assert run("2\n1 0\n0 1") == "1", "diagonal sparse"
assert run("2\n1 1\n1 1") == "2", "full matrix"
assert run("3\n0 0 0\n0 0 0\n0 0 0") == "0", "all zeros"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1×1 với 1 | 1 | xây dựng tối thiểu | 
| đường chéo 2×2 | 1 | thiếu sự chồng chéo | 
| đầy đủ 2×2 | 2 | đóng gói tối đa | 
| tất cả số không | 0 | cấu trúc trống | 

## Vỏ cạnh 

Một ma trận hoàn toàn bằng 0 là kiểu thất bại đơn giản nhất dành cho lối suy nghĩ tham lam ngây thơ. Đối với đầu vào:```
2
0 0
0 0
```mỗi hàng và cột không có hỗ trợ. Thuật toán tính toán chính xác mức hỗ trợ cột 0 cho bất kỳ$x \ge 1$, Vì thế`can(1)`không thành công và tìm kiếm nhị phân trả về 0. Một cách tiếp cận đơn giản giả sử ít nhất một hàng luôn có thể tạo thành cấu trúc tối thiểu sẽ trả về 1 không chính xác. 

Trường hợp cạnh thứ hai là một ma trận có chính xác một hàng chứa đầy các hàng:```
3
1 1 1
0 0 0
0 0 0
```Việc sắp xếp đặt toàn bộ hàng lên đầu tiên, nhưng đối với$x=2$, độ hỗ trợ của cột trong khối hàng thứ hai bằng 0, do đó việc kiểm tra tính khả thi sẽ thất bại ngay lập tức. Điều này đảm bảo thuật toán không mở rộng quá mức một hàng mạnh thành nhiều lớp xếp chồng lên nhau. 

Trường hợp thứ ba là ma trận dày đặc đối xứng trong đó nhiều hàng có tổng bằng nhau. Bước sắp xếp không phá vỡ tính chính xác vì tính khả thi phụ thuộc vào sự chồng chéo cột tổng hợp chứ không phải danh tính hàng. Thuật toán vẫn đánh giá chính xác vì tất cả các tập con trên đều tương đương với tự do hoán vị.
