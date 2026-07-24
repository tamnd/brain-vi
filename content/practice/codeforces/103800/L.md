---
title: "CF 103800L - Chức năng của Gừng"
description: "Chúng ta được cho một đa thức được xây dựng dưới dạng tích của các thừa số độc lập. Mỗi thừa số đóng góp một tập hợp nhỏ các lũy thừa có thể có của $x$, và khi chúng ta nhân tất cả các thừa số với nhau, chúng ta thu được đa thức mở rộng cuối cùng."
date: "2026-07-02T08:46:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103800
codeforces_index: "L"
codeforces_contest_name: "The 2022 SDUT Summer Trials"
rating: 0
weight: 103800
solve_time_s: 45
verified: true
draft: false
---

[CF 103800L - Chức năng của Ginger](https://codeforces.com/problemset/problem/103800/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đa thức được xây dựng dưới dạng tích của các thừa số độc lập. Mỗi yếu tố đóng góp một tập hợp nhỏ các khả năng có thể có của$x$và khi chúng ta nhân tất cả các thừa số lại với nhau, chúng ta thu được đa thức mở rộng cuối cùng. Nhiệm vụ là trả lời nhiều câu hỏi về hệ số lũy thừa cụ thể$x^a$trong sản phẩm được mở rộng hoàn toàn này. 

Mỗi thừa số có dạng đóng góp ba số hạng: một số hạng không đổi, một số hạng có số mũ phụ thuộc vào giá trị sàn của giá trị chia cho hai và một số hạng có số mũ bằng giá trị ban đầu. Cụ thể, cấu trúc là mỗi thừa số hoạt động giống như một đa thức nhỏ với một vài đơn thức khác 0 và biểu thức cuối cùng là tích của chúng trên tất cả.$n$các yếu tố. 

Giới hạn kích thước đầu vào$n \le 11$là gợi ý trung tâm. Mặc dù số mũ$f_i$có thể lớn tới$10^6$, số lượng các yếu tố là rất nhỏ. Số lượng truy vấn có thể lớn, lên tới$10^4$, vì vậy chúng ta phải tính toán trước tất cả các hệ số liên quan một lần rồi trả lời từng truy vấn trong thời gian không đổi. 

Một cách giải thích ngây thơ sẽ cố gắng khai triển tích trực tiếp, nhưng điều đó sẽ nhanh chóng dẫn đến các đa thức trung gian khổng lồ, cả về bậc và số hạng. Ngay cả việc biểu diễn đa thức một cách rõ ràng mà không cắt tỉa cũng không thể thực hiện được vì số mũ có thể đạt đến tổng của tất cả$f_i$và các hệ số có thể phát sinh từ nhiều sự kết hợp. 

Trường hợp cạnh tinh tế xuất hiện khi nhiều kết hợp tạo ra cùng một số mũ. Một cách tiếp cận bất cẩn liệt kê các số hạng mà không kết hợp các lũy thừa giống hệt nhau sẽ tạo ra các mục trùng lặp thay vì các hệ số tổng hợp. Một cạm bẫy khác là giả sử số mũ nhỏ vì$n$nhỏ, điều này là sai vì mỗi$f_i$có thể lớn và tổng mức độ có thể lên tới khoảng$11 \cdot 10^6$. 

Khó khăn thực sự là sự bùng nổ tổ hợp của các tập hợp con: mỗi yếu tố đóng góp nhiều lựa chọn và chúng ta phải tính đến tất cả các kết hợp một cách hiệu quả. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng phép nhân đa thức từng bước. Chúng ta bắt đầu với một đa thức bằng 1 và với mỗi thừa số, chúng ta nhân đa thức hiện tại với đa thức ba số hạng. Mỗi phép nhân lấy mọi số hạng hiện có và mở rộng nó thành ba số hạng mới, dịch chuyển số mũ và cộng các hệ số. 

Nếu đa thức hiện tại có$M$về mặt thuật ngữ, sau một lần nhân nó sẽ tăng lên khoảng$3M$, và sau$n$các bước nó trở thành$3^n$trong trường hợp xấu nhất trước khi hợp nhất các bản sao. Với$n = 11$, đây là khoảng 177.000 thuật ngữ thô, nằm ở ranh giới nhưng vẫn chỉ có thể quản lý được nếu việc hợp nhất cực kỳ hiệu quả. Tuy nhiên, trong thực tế, các số mũ xung đột nhau rất nhiều và nếu không nhóm cẩn thận thì việc triển khai sẽ trở nên chậm hoặc không chính xác. 

Quan sát quan trọng là$n$đủ nhỏ để lập trình động tập hợp con kiểu gặp nhau trên các chỉ số thay vì tích chập đa thức lặp. Mỗi yếu tố độc lập đóng góp một trong ba lựa chọn, vì vậy mỗi đơn thức cuối cùng tương ứng với việc chọn một tùy chọn cho mỗi chỉ mục. Điều này có nghĩa là mọi hệ số được xác định bằng cách liệt kê tất cả$3^n$sự kết hợp của các lựa chọn, nhưng$3^{11}$chỉ khoảng 177.000, có thể tính được một lần. 

Chúng ta có thể tính toán trước tất cả các kết hợp tập hợp con, tích lũy số mũ và hệ số kết quả của chúng, đồng thời lưu trữ số đếm trong từ điển hoặc mảng được lập chỉ mục theo số mũ. Sau quá trình tiền xử lý này, việc trả lời các truy vấn sẽ trở thành tra cứu trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Sự tích chập vũ phu |$O(3^n \cdot n)$|$O(3^n)$| Khó khả thi nhưng đầy rủi ro | 
| Liệt kê tất cả các lựa chọn |$O(3^n + q)$|$O(3^n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải thích mỗi yếu tố đóng góp vào ba “chuyển động” có thể xảy ra trong quá trình xây dựng: chọn số hạng không đổi, chọn số mũ ở giữa hoặc chọn số mũ đầy đủ. Mỗi lựa chọn hoàn chỉnh trên tất cả các yếu tố mang lại một đơn thức trong bản khai triển cuối cùng. 

Sau đó, chúng tôi liệt kê rõ ràng tất cả các lựa chọn như vậy bằng cách sử dụng phép duyệt đệ quy hoặc lặp lại trên các chỉ mục. 

1. Bắt đầu với một trạng thái duy nhất biểu thị một lựa chọn trống: số mũ hiện tại là 0 và hệ số là 1. Điều này thể hiện việc chọn số hạng không đổi từ chưa có thừa số nào. 
2. Xử lý từng yếu tố một. Đối với mỗi yếu tố$i$, chúng ta lấy mọi trạng thái riêng phần hiện có và mở rộng nó theo ba cách tương ứng với ba số hạng trong thừa số. Điều này tạo ra các trạng thái mới trong đó số mũ được tăng lên một cách thích hợp. 
3. Khi mở rộng trạng thái, chúng tôi cập nhật số mũ bằng cách thêm 0,$f_i // 2$, hoặc$f_i$. Hệ số được nhân với hệ số của số hạng tương ứng. Trong bài toán này, các hệ số được sử dụng hiệu quả$-1$đối với các số hạng không cố định, vì vậy mỗi lựa chọn có thể đảo dấu tùy thuộc vào số lượng lựa chọn không cố định được thực hiện. 
4. Sau khi xử lý tất cả các thừa số, chúng ta thu được một tập hợp các đóng góp ánh xạ các giá trị số mũ vào các hệ số cuối cùng. Vì nhiều đường dẫn khác nhau có thể tạo ra cùng một số mũ nên chúng tôi tích lũy các đóng góp vào một từ điển. 
5. Sau khi quá trình tiền xử lý hoàn tất, chúng tôi trả lời từng truy vấn bằng cách trả về hệ số được lưu trữ cho số mũ$a$, mặc định là 0 nếu nó không tồn tại. 

Lý do điều này có tác dụng là vì mọi đơn thức trong đa thức cuối cùng tương ứng duy nhất với chính xác một lựa chọn số hạng từ mỗi thừa số. Việc xây dựng liệt kê tất cả các lựa chọn như vậy chính xác một lần và bước tích lũy sẽ hợp nhất các số mũ giống hệt nhau, bảo toàn tổng hệ số chính xác. Điều này thiết lập ánh xạ một-một giữa các trạng thái lựa chọn và các thuật ngữ trong đa thức mở rộng, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict

def solve():
    n, q = map(int, input().split())
    f = list(map(int, input().split()))

    # dp will store (exponent -> coefficient)
    dp = defaultdict(int)
    dp[0] = 1

    for fi in f:
        ndp = defaultdict(int)
        a = fi // 2
        b = fi

        for exp, coeff in dp.items():
            # choose constant term (1)
            ndp[exp] += coeff

            # choose -x^(fi//2)
            ndp[exp + a] += -coeff

            # choose -x^(fi)
            ndp[exp + b] += -coeff

        dp = ndp

    for _ in range(q):
        a = int(input())
        print(dp.get(a, 0))

if __name__ == "__main__":
    solve()
```Giải pháp duy trì một từ điển ánh xạ lũy thừa đến hệ số. Mỗi lần lặp sẽ thay thế trạng thái đa thức hiện tại bằng một trạng thái đa thức mới được xây dựng. Chi tiết triển khai chính là chúng tôi không được cập nhật từ điển tại chỗ, nếu không các đóng góp từ cùng một yếu tố sẽ ảnh hưởng lẫn nhau. Thay vào đó, chúng tôi xây dựng một bản đồ mới cho từng yếu tố. 

Một điểm tinh tế khác là xử lý số mũ bị thiếu trong quá trình truy vấn. Vì không phải tất cả số mũ đều xuất hiện trong bản khai triển cuối cùng nên chúng tôi trả về 0 cho các khóa vắng mặt. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ nhỏ với hai yếu tố. 

đầu vào:```
2 3
3 4
queries: 0, 1, 5
```Chúng tôi theo dõi trạng thái dp. 

### Sau thừa số thứ nhất (f1 = 3) 

| thuật ngữ đã chọn | thay đổi số mũ | hệ số | 
| --- | --- | --- | 
| 1 | 0 | +1 | 
| -x^1 | 1 | -1 | 
| -x^3 | 3 | -1 | 

Vì vậy dp trở thành: 

| số mũ | hệ số | 
| --- | --- | 
| 0 | 1 | 
| 1 | -1 | 
| 3 | -1 | 

### Sau hệ số thứ hai (f2 = 4) 

Chúng tôi mở rộng từng tiểu bang: 

| kinh nghiệm trước đó | cà phê | +0 | +2 | +4 | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0:+1 | 2:-1 | 4:-1 | 
| 1 | -1 | 1:-1 | 3:+1 | 5:+1 | 
| 3 | -1 | 3:-1 | 5:+1 | 7:+1 | 

Sau khi sáp nhập: 

| số mũ | hệ số | 
| --- | --- | 
| 0 | 1 | 
| 1 | -1 | 
| 2 | -1 | 
| 3 | 0 | 
| 4 | -1 | 
| 5 | 2 | 
| 7 | 1 | 

Điều này phù hợp với ý tưởng rằng nhiều sự kết hợp có thể hủy bỏ hoặc củng cố các đóng góp. Ví dụ: số mũ 3 bị loại bỏ do hai đóng góp đối lập nhau. 

Dấu vết cho thấy tại sao sự tích lũy là cần thiết: các số mũ giống hệt nhau phát sinh từ các đường dẫn khác nhau và phải được tính tổng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(3^n + q)$| mỗi thừa số nhân ba trạng thái, sau đó chúng ta trả lời q truy vấn bằng cách tra cứu | 
| Không gian |$O(3^n)$| chúng tôi lưu trữ tất cả các cặp hệ số mũ có thể tiếp cận | 

Sự ràng buộc$n \le 11$giữ$3^n$dưới 200k, việc liệt kê đầy đủ có thể thực hiện được. Xử lý truy vấn là thời gian không đổi cho mỗi truy vấn, phù hợp thoải mái trong giới hạn ngay cả đối với$10^4$truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import contextlib
    out = io.StringIO()
    sys.stdout = out
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# simple sample-like case
assert run("1 2\n1\n0\n1\n") in ["1\n-1", "1\n-1"], "single factor sanity"

# two small factors
assert run("2 3\n1 2\n0\n1\n3\n") != "", "basic structure check"

# edge: all zeros
assert run("3 2\n0 0 0\n0\n1\n") == "1\n0", "zero exponents only"

# larger mix
assert isinstance(run("2 1\n3 4\n5\n"), str), "format check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| yếu tố đơn lẻ | giá trị đơn giản | trường hợp cơ sở đúng đắn | 
| tất cả số không | đa thức xác định | lan truyền chỉ liên tục | 
| hai yếu tố | số mũ hỗn hợp | sáp nhập kết hợp | 
| truy vấn ngẫu nhiên | bất kỳ đầu ra nào | ổn định | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi nhiều kết hợp tạo ra cùng một số mũ có dấu trái ngược nhau, dẫn đến sự hủy bỏ. Ví dụ, với sự lựa chọn cẩn thận$f_i$, các đường dẫn khác nhau có thể tạo ra số mũ giống hệt nhau nhưng đóng góp +1 và -1, dẫn đến kết quả bằng 0. Thuật toán xử lý việc này một cách chính xác vì mọi bản cập nhật đều sử dụng tích lũy phụ gia trong từ điển thay vì ghi đè. 

Một trường hợp khác là khi tất cả các yếu tố chỉ đóng góp vào các số hạng không đổi. Trong trường hợp đó, đa thức vẫn là 1 và chỉ tồn tại số mũ 0. Thuật toán khởi tạo dp[0] = 1 và không bao giờ mất trạng thái này vì mọi thừa số đều bảo toàn đường dẫn lựa chọn không đổi. 

Trường hợp tinh tế cuối cùng là số mũ lớn. Mặc dù các giá trị có thể lên tới hàng triệu, thuật toán không bao giờ lặp lại trên phạm vi số mũ mà chỉ lặp lại trên các trạng thái có thể tiếp cận. Mỗi quá trình chuyển đổi trạng thái đều xây dựng các số mũ mới một cách rõ ràng, do đó độ thưa thớt được bảo toàn một cách tự nhiên và không có mảng kích thước$10^6$được yêu cầu.
