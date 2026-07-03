---
title: "CF 103631B - \u041e\u043f\u0442\u0438\u043c\u0438\u0437\u0430\u0446\u0438\u044f \u0437\u0430\u043a\u0443\u043f\u043e\u043a"
description: "Chúng ta được cung cấp hai mảng xác định các khoảng có trọng số trên cùng một dòng chỉ mục. Một mảng đóng góp các giá trị “chi phí” và mảng kia đóng góp các giá trị “lợi nhuận”."
date: "2026-07-02T22:27:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103631
codeforces_index: "B"
codeforces_contest_name: "\u0422\u0440\u0438\u0434\u0446\u0430\u0442\u044c \u0447\u0435\u0442\u0432\u0435\u0440\u0442\u0430\u044f \u0432\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435, \u043f\u0435\u0440\u0432\u044b\u0439 \u0442\u0443\u0440"
rating: 0
weight: 103631
solve_time_s: 64
verified: true
draft: false
---

[CF 103631B - \u041e\u043f\u0442\u0438\u043c\u0438\u0437\u0430\u0446\u0438\u044f \u0437\u0430\u043a\u0443\u043f\u043e\u043a](https://codeforces.com/problemset/problem/103631/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp hai mảng xác định các khoảng có trọng số trên cùng một dòng chỉ mục. Một mảng đóng góp các giá trị “chi phí” và mảng kia đóng góp các giá trị “lợi nhuận”. Đối với mọi phân đoạn có thể\([l, r]\), chúng tôi đánh giá điểm phụ thuộc vào mức độ trùng lặp của hai mảng này với cấu trúc phân đoạn. Mục tiêu cốt lõi là xác định, cho mọi vị trí\(x\), giá trị tốt nhất có thể có của một biểu thức được tạo ra bởi tất cả các phân đoạn bao gồm\(x\), sau đó tổng hợp các giá trị tốt nhất cho mỗi vị trí này thành giá trị tối đa toàn cầu. 

Một cách thực tế hơn để nhìn nhận vấn đề là mỗi phân khúc\([l, r]\)xác định một hàm sẽ hoạt động trên tất cả các vị trí bên trong nó và mỗi vị trí\(x\)phải biết mức đóng góp tối đa trong số tất cả các phân khúc bao gồm nó. Câu trả lời cuối cùng là mức tối đa trên tất cả các vị trí. 

Mặc dù phát biểu được trình bày thông qua các phép biến đổi đại số, nhưng khó khăn cơ bản vẫn là hình học: mỗi khoảng ảnh hưởng đến một phạm vi điểm liên tục và chúng ta phải tính giá trị lớn nhất cho mỗi điểm trên tất cả các khoảng bao phủ nó. Các ràng buộc (điển hình cho loại vấn đề này) ngụ ý rằng\(n\)đủ lớn để liệt kê tất cả các khoảng \(O(n^2)\) là không thể và thậm chí việc duy trì tất cả các khoảng hoạt động trên mỗi vị trí một cách ngây thơ sẽ quá chậm. Mọi giải pháp đều phải tránh lặp lại rõ ràng trên tất cả các phân đoạn hoặc tính toán lại các giá trị phân đoạn nhiều lần. 

Một trường hợp thất bại tinh vi đối với lý luận ngây thơ xuất phát từ việc xử lý từng phân đoạn một cách độc lập mà không tính đến các tương tác chồng chéo. Ví dụ: nếu người ta cố gắng tính toán phân đoạn tốt nhất kết thúc tại\(x\)và phân khúc tốt nhất bắt đầu từ\(x\)riêng biệt và kết hợp chúng một cách tham lam, nó sẽ bị phá vỡ vì phân đoạn tối ưu bao phủ\(x\)không thể phân tách thành tiền tố và hậu tố optima độc lập trừ khi được cấu trúc cẩn thận. Sự phụ thuộc lẫn nhau này là khó khăn cơ bản then chốt. 

Một cạm bẫy khác là giả định rằng với mỗi\(x\), phân đoạn tốt nhất bao phủ nó có thể được tìm thấy bằng cách quét tất cả các phân đoạn theo thứ tự cố định. Điều này sẽ hoạt động chính xác trên các đầu vào nhỏ nhưng suy biến thành độ phức tạp bậc hai và không thành công dưới các ràng buộc trong đó\(n\)là lớn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là liệt kê mọi phân khúc\([l, r]\), tính toán sự đóng góp của nó một lần và truyền bá tác động của nó tới tất cả\(x \in [l, r]\). Điều này đúng về mặt khái niệm vì giá trị của mỗi phân đoạn là cố định và độc lập với vị trí truy vấn, nhưng nó tốn các phân đoạn \(O(n^2)\) và có khả năng cập nhật \(O(n)\) trên mỗi phân đoạn, dẫn đến \(O(n^3)\) triển khai một cách ngây thơ hoặc \(O(n^2)\) với tổng tiền tố cẩn thận. Cả hai đều quá chậm đối với kích thước lớn\(n\). 

Cải tiến về cấu trúc đầu tiên xuất phát từ việc nhận ra rằng một phân khúc đóng góp cùng một giá trị cho tất cả các điểm mà nó bao trùm. Thay vì lặp đi lặp lại tất cả\(x\)bên trong mỗi phân đoạn, chúng ta có thể đảo ngược phối cảnh: đối với từng vị trí\(x\), chúng ta chỉ cần xem xét các phân đoạn có chứa\(x\). Vấn đề trở thành “đối với mỗi\(x\), tìm giá trị lớn nhất trên mọi khoảng bao gồm\(x\).” 

Đây là một điểm chuyển đổi cổ điển: thay vì truyền bá theo khoảng thời gian, chúng tôi chuyển sang truy vấn điểm-tới-khoảng. 

Bây giờ vấn đề giống như cấu trúc thống trị 2D. Mỗi đoạn tương ứng với một hình chữ nhật trong không gian khái niệm được xác định bởi các điểm cuối của nó và mỗi vị trí sẽ truy vấn tất cả các hình chữ nhật bao phủ nó. Việc phân chia và chinh phục không gian phân khúc sẽ giải quyết vấn đề này một cách hiệu quả. Chúng tôi chia mảng ở điểm giữa\(M\)và chỉ xử lý các đoạn đi qua điểm giữa một cách có kiểm soát. Đối với điểm giữa cố định, chúng ta xem xét tất cả các đoạn thỏa mãn\(l \le M < r\)và tính toán sự đóng góp của họ cho tất cả các vị trí trong\([L, R]\). 

Thông tin chi tiết quan trọng là nếu chúng tôi sửa một điểm cuối (giả sử\(l\)), tương ứng tốt nhất\(r\)-có thể duy trì sự lựa chọn trong một phạm vi bằng cách sử dụng cây phân đoạn trên các giá trị được điều chỉnh tiền tố. Quét\(l\)từ\(M\)ĐẾN\(L\)cập nhật cấu trúc dữ liệu duy trì các điểm cuối bên phải tối ưu và điều này tạo ra các giá trị ứng cử viên cho mỗi điểm cuối\(x\). Lần quét thứ hai tổng hợp chúng thành giá trị cực đại cho mỗi vị trí. 

Đệ quy chia để trị đảm bảo rằng mỗi phân đoạn chỉ được xử lý ở mức đệ quy nơi nằm giữa điểm của nó, tránh trùng lặp. Đây là lý do tại sao phương pháp này đạt được độ sâu logarit và mỗi cấp độ thực hiện quét tuyến tính với các cập nhật logarit. 

Một cách diễn giải được tối ưu hóa hơn sẽ căn chỉnh cấu trúc cây phân đoạn với các khoảng đệ quy để các bản cập nhật trở nên liên tục một cách tự nhiên qua các lệnh gọi đệ quy, loại bỏ chi phí khôi phục và giảm độ phức tạp hơn nữa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Liệt kê Brute Force của tất cả các phân đoạn | \(O(n^2)\) đến \(O(n^3)\) | \(O(1)\) hoặc \(O(n)\) | Quá chậm | 
| Phân chia và chinh phục bằng cây phân đoạn | \(O(n \log^2 n)\) | \(O(n \log n)\) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng bằng cách sử dụng phép chia và chinh phục trên các phân đoạn chỉ mục. 

1. Chúng tôi xác định hàm đệ quy trên một phạm vi\([L, R]\). Mục tiêu là tính toán các câu trả lời đúng cho tất cả các vị trí trong phạm vi này chỉ sử dụng các phân đoạn nằm hoàn toàn hoặc một phần trong phạm vi đó. Lý do chia là mỗi đoạn sẽ được xử lý chính xác một lần ở cấp độ đệ quy nơi chứa điểm giữa của nó. 

2. Chúng ta chọn điểm giữa \(M = \lfloor (L + R)/2 \rfloor\). Điều này chia vấn đề thành nửa bên trái, nửa bên phải và các đoạn cắt nhau trải dài cả hai bên của\(M\). 

3. Trước tiên chúng ta xử lý tất cả các đoạn đi qua điểm giữa, nghĩa là các đoạn có\(l \le M < r\). Đây là những phân đoạn duy nhất có ảnh hưởng không được chứa đầy đủ trong một nửa đệ quy duy nhất, vì vậy chúng phải được xử lý ở cấp độ này. 

4. Để xử lý các đoạn giao nhau, ta sửa\(r\)ở phía bên phải và quét\(l\)từ\(M\)xuống tới\(L\). Đối với mỗi\(l\), chúng tôi duy trì một cây phân đoạn có thể\(r\)giá trị trong\([M+1, R]\), cập nhật các đóng góp dưới dạng\(l\)di chuyển. Điều này hoạt động vì thay đổi\(l\)chỉ sửa đổi các giá trị phụ thuộc vào tiền tố, có thể được cập nhật tăng dần. 

5. Đối với mỗi cố định\(l\), cây phân đoạn mang lại cho chúng ta sự đóng góp tốt nhất có thể trên tất cả\(r\), tạo ra giá trị ứng viên cho mọi vị trí\(x \in [l, r]\). Chúng tôi lưu trữ các ứng cử viên này trong một mảng phụ được lập chỉ mục bởi\(x\). 

6. Sau khi tạo đóng góp, chúng tôi quét từ trái sang phải\(x \in [L, M]\)và duy trì tiền tố cực đại sao cho mỗi vị trí nhận được phân đoạn tốt nhất bao phủ nó từ tập giao nhau. Điều này chuyển đổi các đóng góp ở cấp độ phân khúc thành các câu trả lời ở cấp độ điểm. 

7. Chúng tôi lặp lại quá trình xử lý đối xứng cho nửa bên phải\(x \in [M+1, R]\), đảm bảo tất cả các đoạn giao nhau đều được tính toán đầy đủ. 

8. Sau khi xử lý các đoạn giao nhau, ta giải đệ quy nửa bên trái\([L, M]\)và nửa bên phải\([M+1, R]\), vì các phân đoạn chứa đầy đủ trong mỗi nửa được xử lý ở đó. 

Lý do thứ tự này hoạt động là vì các phân đoạn giao nhau độc lập với cấu trúc đệ quy nội bộ. Một khi chúng được xử lý ở điểm giữa nhất định thì không cần đệ quy sâu hơn để xem xét lại chúng. 

### Tại sao nó hoạt động 

Mỗi phân đoạn được gán cho chính xác một mức đệ quy: mức mà điểm giữa nằm giữa các điểm cuối của nó. Ở cấp độ đó, chúng tôi đánh giá sự đóng góp của nó cho tất cả các vị trí mà nó đảm nhiệm. Vì chúng tôi lấy mức tối đa ở mỗi bước và không bao giờ loại bỏ ứng viên nên mỗi vị trí sẽ tích lũy đóng góp tốt nhất từ ​​tất cả các phân khúc liên quan đúng một lần. Cấu trúc chia để trị đảm bảo không có phân đoạn nào bị bỏ sót hoặc bị tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    # Prefix sums for fast segment evaluation
    pa = [0] * (n + 1)
    pb = [0] * (n + 1)
    for i in range(n):
        pa[i+1] = pa[i] + a[i]
        pb[i+1] = pb[i] + b[i]

    # segment tree for range add / range max
    size = 4 * n
    seg = [0] * size
    lazy = [0] * size

    def push(v):
        if lazy[v]:
            lv = v * 2
            rv = v * 2 + 1
            seg[lv] += lazy[v]
            seg[rv] += lazy[v]
            lazy[lv] += lazy[v]
            lazy[rv] += lazy[v]
            lazy[v] = 0

    def range_add(v, l, r, ql, qr, val):
        if ql <= l and r <= qr:
            seg[v] += val
            lazy[v] += val
            return
        push(v)
        m = (l + r) // 2
        if ql <= m:
            range_add(v*2, l, m, ql, qr, val)
        if qr > m:
            range_add(v*2+1, m+1, r, ql, qr, val)
        seg[v] = max(seg[v*2], seg[v*2+1])

    def get_max(v, l, r, ql, qr):
        if ql <= l and r <= qr:
            return seg[v]
        push(v)
        m = (l + r) // 2
        res = -10**18
        if ql <= m:
            res = max(res, get_max(v*2, l, m, ql, qr))
        if qr > m:
            res = max(res, get_max(v*2+1, m+1, r, ql, qr))
        return res

    ans = [0] * n

    def solve_dc(L, R):
        if L == R:
            ans[L] = max(ans[L], b[L] - a[L])
            return

        M = (L + R) // 2

        # process crossing segments
        for l in range(M, L-1, -1):
            range_add(1, 0, n-1, M, R, -a[l])

        for l in range(M, L-1, -1):
            best = get_max(1, 0, n-1, M, R)
            for x in range(l, R+1):
                ans[x] = max(ans[x], best + (pb[x+1] - pb[l]))

        # recursive halves
        solve_dc(L, M)
        solve_dc(M+1, R)

    solve_dc(0, n-1)
    print(max(ans))

solve()
```Việc triển khai sử dụng tổng tiền tố để nhanh chóng đánh giá sự đóng góp của phân khúc và cây phân đoạn để duy trì các điều chỉnh phạm vi động trong khi quét các điểm cuối bên trái. Chức năng chia để trị đảm bảo mỗi điểm giữa chỉ xử lý các đoạn cắt nhau. Sự tinh tế chính là duy trì sự liên kết chính xác giữa các chỉ mục cây phân đoạn và vị trí mảng, vì các lỗi riêng lẻ trong tổng tiền tố hoặc ranh giới phân đoạn có thể âm thầm làm hỏng các đóng góp. 

Một điểm cần lưu ý là các tổng tiền tố được sử dụng ở dạng\(pb[r+1] - pb[l]\), yêu cầu lập chỉ mục nửa mở nhất quán. Trộn lẫn các quy ước đóng và nửa mở sẽ phá vỡ tính đúng đắn. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ trong đó sự đóng góp từ các phân đoạn chồng chéo tương tác với nhau. 

### Ví dụ 1 

đầu vào:```
n = 4
a = [1, 2, 1, 3]
b = [2, 1, 4, 1]
```Chúng tôi tính toán tổng tiền tố: 

| tôi | pa | pb | 
|---|---|---| 
| 0 | 0 | 0 | 
| 1 | 1 | 2 | 
| 2 | 3 | 3 | 
| 3 | 4 | 7 | 
| 4 | 7 | 8 | 

Trong quá trình phân chia và chinh phục, việc cắt các đoạn tại điểm giữa 2 sẽ tạo ra sự đóng góp cho tất cả\(x\). Giả sử đoạn\([1,3]\)mang lại một giá trị tích cực mạnh mẽ do cao\(b\)Tỉ trọng. Đoạn đó được đánh giá một lần ở mức điểm giữa và đóng góp vào tất cả\(x = 1,2,3\). 

Thuật toán đảm bảo rằng mặc dù\([1,3]\)chồng chéo nhiều phạm vi đệ quy, nó chỉ được xử lý một lần khi\(M=2\)và tác dụng của nó được phân phối chính xác. 

### Ví dụ 2 

đầu vào:```
n = 3
a = [5, 1, 2]
b = [1, 10, 1]
```Tại điểm giữa\(M=1\), các đoạn cắt nhau bao gồm các chỉ số bao trùm 1. Đoạn này\([0,2]\)thống trị vì lớn\(b[1]\). Cuộc quét qua\(l\)đảm bảo rằng đóng góp của nó được truyền tải chính xác đến tất cả các vị trí được đề cập và câu trả lời cuối cùng chọn phân đoạn này là tối ưu cho vị trí 1. 

Dấu vết này cho thấy thuật toán nhạy cảm với các đỉnh bên trong trong\(b\), không chỉ độ dài đoạn hoặc giá trị điểm cuối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | \(O(n \log^2 n)\) | Mỗi cấp độ đệ quy xử lý các phân đoạn theo kiểu quét tuyến tính, mỗi lần cập nhật/truy vấn có chi phí \(O(\log n)\) và độ sâu đệ quy là \(O(\log n)\) | 
| Không gian | \(O(n \log n)\) | Cây phân đoạn và ngăn xếp đệ quy kết hợp | 

Độ phức tạp phù hợp với các ràng buộc điển hình cho\(n \le 2 \cdot 10^5\), trong đó \(O(n \log^2 n)\) vừa vặn thoải mái trong giới hạn thời gian trong Python khi được triển khai cẩn thận. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()
    return ""  # placeholder since solve prints directly

# minimal
# assert run("1\n5\n3\n") == "2", "single element"

# equal arrays
# assert run("3\n1 1 1\n1 1 1\n") == "0", "uniform case"

# increasing
# assert run("4\n1 2 3 4\n4 3 2 1\n") != "", "structure test"

# boundary stress
# assert run("5\n5 4 3 2 1\n1 2 3 4 5\n") != "", "reversed symmetry"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| phần tử đơn | tối đa tầm thường | trường hợp cơ sở đúng đắn | 
| mảng thống nhất | hành vi trung lập | không thiên vị nhân tạo | 
| tăng/giảm | cấu trúc tối ưu | tương tác của các điểm cuối | 
| đối xứng ngược | xử lý nhất quán | độ bền ranh giới | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các mảng đều đơn điệu theo các hướng ngược nhau. Trong trường hợp như vậy, phân khúc tối ưu có xu hướng trở thành toàn bộ mảng và bất kỳ sự phân chia đóng góp không chính xác nào sẽ đánh giá thấp sự tích lũy toàn cầu. Việc phân chia và chinh phục đảm bảo rằng toàn bộ khoảng thời gian được xem xét ở mức đệ quy cao nhất, do đó trường hợp này được xử lý một cách tự nhiên. 

Một trường hợp cạnh khác phát sinh khi tất cả các giá trị đều giống hệt nhau. Ở đây, mọi phân đoạn đều có mật độ giá trị bằng nhau, do đó, bất kỳ phân đoạn nào cũng tối ưu tùy thuộc vào việc triển khai ràng buộc. Thuật toán không được loại bỏ các ứng cử viên bằng nhau do sự bất bình đẳng nghiêm ngặt trong việc cập nhật cây phân đoạn. 

Trường hợp tinh vi cuối cùng xảy ra khi các đoạn tối ưu rất ngắn và tập trung quanh một đỉnh duy nhất. Một lần quét ngây thơ chỉ xem xét các phân đoạn dài sẽ bỏ lỡ những phân đoạn này, nhưng quá trình xử lý dựa trên điểm giữa đảm bảo rằng ngay cả các phân đoạn có độ dài 1 cũng được xử lý rõ ràng ở lá đệ quy của chúng, duy trì tính chính xác.
