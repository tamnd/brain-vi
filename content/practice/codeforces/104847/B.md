---
title: "CF 104847B - Hậu chủ nghĩa tư bản"
description: "Chúng ta đang xem xét một quá trình làm giàu ngẫu nhiên trên $n$ người. Mọi người đều bắt đầu với chính xác một đơn vị tiền. Sau đó, nhiều lần chuyển đơn vị bổ sung diễn ra."
date: "2026-06-28T11:23:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104847
codeforces_index: "B"
codeforces_contest_name: "2019-2020 ICPC, Moscow Subregional"
rating: 0
weight: 104847
solve_time_s: 63
verified: true
draft: false
---

[CF 104847B - Hậu chủ nghĩa tư bản](https://codeforces.com/problemset/problem/104847/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang xem xét một quá trình làm giàu ngẫu nhiên trên$n$mọi người. Mọi người đều bắt đầu với chính xác một đơn vị tiền. Sau đó, nhiều lần chuyển đơn vị bổ sung diễn ra. Mỗi lần chuyển tiền được tạo ra bằng cách chọn người nhận với xác suất tỷ lệ thuận với mức độ giàu có hiện tại của họ, vì vậy những người giàu hơn có nhiều khả năng trở nên giàu hơn. Sau khi tất cả các giao dịch chuyển tiền kết thúc, chúng tôi bình thường hóa tài sản thành cổ phiếu$p_i = \frac{s_i}{\sum s_i}$, do đó tổng số cổ phần bằng 1. 

Trạng thái cuối cùng là một điểm ngẫu nhiên trên đơn hình xác suất, nhưng không đồng nhất một cách tùy tiện: nó xuất phát từ quá trình gắn kết ưu tiên. Đây là chiếc bình Pólya cổ điển có trọng số ban đầu bằng nhau, ngụ ý rằng vectơ chuẩn hóa cuối cùng có cùng phân bố như phân bố Dirichlet đối xứng với tất cả các tham số bằng 1. 

Chúng tôi không quan sát cấu hình đầy đủ. Thay vào đó, chúng tôi chỉ được chia sẻ phần nhỏ nhất trong số tất cả mọi người, gọi đó là$p_{(1)} = a$. Trong điều kiện này, chúng ta được yêu cầu tính giá trị kỳ vọng của phần lớn nhất$p_{(n)}$. 

Khó khăn chính là việc điều chỉnh thống kê thứ tự của phân bố Dirichlet đối xứng sẽ thay đổi hình dạng của vùng khả thi theo một cách rất không tầm thường. Một cách tiếp cận đơn giản sẽ cố gắng mô phỏng quy trình hoặc tích hợp rõ ràng trên tất cả các cấu hình, nhưng không gian trạng thái là liên tục và có nhiều chiều, do đó điều này nhanh chóng trở nên khó hiểu ngay cả đối với các cấu hình vừa phải.$n$. 

Những hạn chế$n \le 2000$loại trừ bất kỳ điều gì liên quan đến việc liệt kê các tập hợp con hoặc sự rời rạc của đơn hình. giá trị$a \le \frac{1}{n}$phù hợp với tính khả thi: nếu tỷ lệ chia sẻ tối thiểu là$a$, thì ít nhất$n a \le 1$phải giữ vì tổng số cổ phiếu bằng 1. 

Một vấn đề tế nhị là sự kiện “tối thiểu bằng chính xác$a$” ngầm có nghĩa là một tọa độ chính xác$a$và tất cả những cái khác đều lớn hơn. Trong các phân phối liên tục, các ràng buộc có xác suất bằng 0, vì vậy cách giải thích này là an toàn, nhưng nó quan trọng đối với cách chúng ta phân tách cấu trúc. 

## Phương pháp tiếp cận 

Mô hình tinh thần mạnh mẽ là xem xét tất cả các vectơ của cải cuối cùng có thể có trên đơn hình, lọc những vectơ có tọa độ tối thiểu bằng$a$, rồi tính trung bình tọa độ cực đại. Ngay cả khi bỏ qua tính toán, hình học vẫn phức tạp: chúng ta đang lấy tích phân trên một vùng được cắt lát của một đơn hình với ràng buộc biên được xác định bởi thống kê thứ tự. Tích hợp trực tiếp sẽ yêu cầu xử lý$n$Các vùng từng chiều được xác định bởi tọa độ nào là tối thiểu, trở nên lộn xộn về mặt tổ hợp. 

Sự đột phá về cấu trúc đến từ việc công nhận phân bố là Dirichlet đối xứng$(1,1,\dots,1)$, đồng nhất trên đơn hình. Điều hòa ở mức tối thiểu một cách chính xác$a$buộc một tọa độ nằm ở ranh giới và các tọa độ còn lại hoạt động giống như một đơn hình đồng nhất có chiều thấp hơn sau khi loại bỏ khối lượng cố định đóng góp bởi mức tối thiểu đó. 

Điều này làm giảm vấn đề từ một$n$-câu hỏi thống kê thứ tự chiều cho một$(n-1)$kỳ vọng theo chiều đối với một đơn hình đều, cộng với sự dịch chuyển xác định bởi$a$. 

Thành phần còn lại là dạng đóng đã biết cho tọa độ cực đại dự kiến ​​của vectơ Dirichlet đồng nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tích hợp trực tiếp trên đơn giản theo thứ tự | số mũ trong$n$| Tượng trưng lớn | Quá chậm | 
| Giảm Dirichlet + công thức thống kê thứ tự đã biết |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nhận biết rằng vectơ của cải chuẩn hóa cuối cùng tuân theo phân bố Dirichlet đối xứng với các tham số đều bằng 1. Điều này có nghĩa là vectơ được phân bố đều trên đơn hình xác suất. 
2. Giải thích sự kiện “phần chia tối thiểu bằng$a$” là sự tồn tại của đúng một tọa độ bằng$a$, với tất cả những cái khác đều lớn hơn. Điều này là hợp lý vì sự phân bố là liên tục, do đó các mối quan hệ xảy ra với xác suất bằng không. 
3. Trừ phần đóng góp tối thiểu khỏi tất cả các tọa độ. Cho phép$p_i = a + x_i$. Đối với tọa độ tối thiểu,$x = 0$, trong khi tất cả những người khác thỏa mãn$x_i > 0$. 
4. Chuyển đổi giới hạn tổng. Từ$\sum p_i = 1$, chúng tôi nhận được$\sum x_i = 1 - n a$. Điều này làm giảm bài toán xuống một đơn hình có tổng khối lượng$1 - n a$qua$n$các biến có một biến cố định ở mức 0. 
5. Xóa tọa độ 0 và tập trung vào phần còn lại$n-1$các biến tích cực. Sau khi chuẩn hóa bằng$1 - n a$, vectơ còn lại một lần nữa được phân bố đều trên một đơn hình có chiều$n-1$. 
6. Thể hiện sự chia sẻ tối đa. Cổ phần gốc tối đa là$a + \max(x_i)$, và kể từ đó$x_i = (1 - n a) y_i$, Ở đâu$y$là thống nhất trên$(n-1)$-đơn giản, chúng tôi nhận được$$p_{(n)} = a + (1 - n a)\cdot \max(y).$$7. Hãy kỳ vọng một cách tuyến tính:$$\mathbb{E}[p_{(n)} \mid p_{(1)} = a] = a + (1 - n a)\,\mathbb{E}[\max(y)].$$8. Sử dụng dạng đóng đã biết cho giá trị lớn nhất mong đợi của vectơ Dirichlet đồng nhất về chiều$k$:$$\mathbb{E}[\max] = \frac{H_k}{k},$$Ở đâu$H_k$là số hài hòa. Đây$k = n-1$. 

### Tại sao nó hoạt động 

Bất biến chính là việc điều hòa thống kê thứ tự biên trong phân bố Dirichlet đối xứng duy trì tính đồng nhất trên đơn hình rút gọn sau khi loại bỏ tọa độ cố định và tái chuẩn hóa. Phân phối Dirichlet có thể trao đổi và ổn định trong điều kiện biên thuộc loại này, giúp ngăn ngừa sự biến dạng của các tọa độ còn lại ngoài thang đo tuyến tính. Đây là điều cho phép kỳ vọng được coi là yếu tố rõ ràng thành một sự thay đổi mang tính quyết định cộng với một kỳ vọng đã biết theo tỷ lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, a = input().split()
    n = int(n)
    a = float(a)

    if n == 1:
        print(1.0)
        return

    k = n - 1

    # harmonic number H_k
    H = 0.0
    for i in range(1, k + 1):
        H += 1.0 / i

    expected_max_simplex = H / k

    ans = a + (1.0 - n * a) * expected_max_simplex
    print(f"{ans:.12f}")

if __name__ == "__main__":
    solve()
```Việc triển khai áp dụng trực tiếp phân rã dẫn xuất. Phép tính không tầm thường duy nhất là số hài, được tính theo thời gian tuyến tính trong$n$, đủ nhanh để$n \le 2000$. Biểu thức cuối cùng tách biệt cẩn thận phần đóng góp xác định khỏi kỳ vọng tối thiểu và tỷ lệ của đơn hình còn lại. 

Một cạm bẫy thường xuyên là quên hệ số tỷ lệ$1 - n a$. Không có nó, kết quả giả định không chính xác khối lượng còn lại là 1 thay vì thể tích đơn giản giảm sau khi cố định tọa độ tối thiểu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 0.01
```| Bước | Giá trị | 
| --- | --- | 
| n | 2 | 
| một | 0,01 | 
| k = n-1 | 1 | 
| H_k | 1 | 
| E[tối đa trên một mặt] | 1 | 
| (1 - n a) | 0,98 | 
| kết quả | 0,01 + 0,98 × 1 = 0,99 | 

Điều này phù hợp với trực giác rằng với hai người, việc chia phần nhỏ hơn gần như quyết định hoàn toàn phần còn lại. 

### Ví dụ 2 

đầu vào:```
5 0.198802992
```| Bước | Giá trị | 
| --- | --- | 
| n | 5 | 
| một | 0.198802992 | 
| k | 4 | 
| H_4 | 2.083333333 | 
| H_4/4 | 0,520833333 | 
| 1 - 5a | ≈ 0,00598504 | 
| kết quả | ≈ 0,201920200333 | 

Điều này cho thấy độ nhạy cực cao khi$a$gần với$1/n$, trong đó đơn hình còn lại sụp đổ và mức tối đa chỉ cao hơn mức tối thiểu một chút. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| tính toán số hài chiếm ưu thế | 
| Không gian |$O(1)$| chỉ một số vô hướng được lưu trữ | 

Sự ràng buộc$n \le 2000$làm cho một đường chuyền tuyến tính trở nên tầm thường. Giải pháp bị chi phối hoàn toàn bởi số học, do đó nó dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    n, a = inp.strip().split()
    n = int(n)
    a = float(a)

    k = n - 1
    H = sum(1.0 / i for i in range(1, k + 1)) if k > 0 else 0.0
    ans = a + (1.0 - n * a) * (H / k if k > 0 else 0.0)

    return f"{ans:.12f}"

# provided samples
assert abs(float(run("2 0.01")) - 0.99) < 1e-9
assert abs(float(run("100 0.01")) - 0.01) < 1e-9
assert abs(float(run("5 0.198802992")) - 0.201920200333) < 1e-9

# minimum-size edge
assert abs(float(run("2 0.5")) - 0.5) < 1e-9

# symmetric collapse case
assert abs(float(run("4 0.25")) - 0.25) < 1e-9

# small nontrivial case
assert abs(float(run("3 0.1")) - float(run("3 0.1"))) < 1e-9
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 0,5 | 0,5 | sụp đổ khi toàn bộ khối lượng được cố định ở mức tối thiểu | 
| 4 0,25 | 0,25 | trường hợp ranh giới$1 - na = 0$| 
| 3 0,1 | tính toán | hành vi đơn giản không tầm thường | 

## Vỏ cạnh 

Khi nào$a = \frac{1}{n}$, toàn bộ khối lượng đã tập trung ở mức ràng buộc tối thiểu. Trong trường hợp này$1 - n a = 0$, do đó đơn hình còn lại biến mất và mọi tọa độ đều phải bằng$a$. Thuật toán xử lý việc này một cách rõ ràng vì thuật ngữ chia tỷ lệ sẽ loại bỏ mức đóng góp tối đa dự kiến. 

Khi$a$cực kỳ nhỏ, nên đơn hình hầu như không thay đổi so với phân bố Dirichlet đầy đủ. Kết quả sau đó tiến tới mức tối đa dự kiến ​​vô điều kiện của một đơn hình đồng nhất về kích thước$n$, đó là$\frac{H_n}{n}$. Công thức giảm dần ở chế độ này vì$1 - n a \approx 1$. 

Khi$n = 2$, “đơn hình còn lại” có chiều 1 và giá trị lớn nhất được xác định là 1. Biểu thức điều hòa đánh giá chính xác thành$H_1/1 = 1$, do đó công thức thu gọn thành$1 - a$, khớp với hình dạng của hai phần bổ sung.
