---
title: "CF 102394K - Nuôi Thỏ"
description: "Chúng ta có (n) con thỏ với trọng số ban đầu (w1,w2,ldots,wn). Vào mỗi (k) buổi sáng, có đúng một con thỏ nhận được thêm một đơn vị trọng lượng."
date: "2026-08-10T19:12:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102394
codeforces_index: "K"
codeforces_contest_name: "The 2019 China Collegiate Programming Contest Harbin Site"
rating: 0
weight: 102394
solve_time_s: 60
verified: true
draft: false
---

[CF 102394K - Nuôi thỏ](https://codeforces.com/problemset/problem/102394/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có (n) con thỏ với trọng số ban đầu (w_1,w_2,\ldots,w_n). Vào mỗi (k) buổi sáng, có đúng một con thỏ nhận được thêm một đơn vị trọng lượng. Một con thỏ có trọng lượng hiện tại là (x) sẽ thắng trận sáng hôm đó với xác suất tỷ lệ với (x), vì vậy nếu tổng trọng lượng hiện tại là (S), thì con thỏ (i) được chọn với xác suất (x_i/S). 

Tổng trọng lượng không phải là ngẫu nhiên. Nó bắt đầu lúc 

[ 
W=w_1+w_2+\cdots+w_n 
] 

và tăng đúng một đơn vị sau mỗi củ cà rốt, nên ngay trước củ cà rốt thứ (t) tổng trọng lượng là (W+t). Nhiệm vụ là tìm trọng lượng cuối cùng dự kiến ​​của mỗi con thỏ sau khi đã ăn hết (k) củ cà rốt. 

Các ràng buộc làm cho mô phỏng trực tiếp không phù hợp. Một trường hợp duy nhất có thể có (k=10^9), do đó, ngay cả việc thực hiện một bản cập nhật mỗi sáng cũng có thể yêu cầu hàng tỷ thao tác. Trong tất cả các trường hợp thử nghiệm, (n) có thể đạt tới (10^6), do đó, thuật toán có hệ số như (n^2) cũng vượt xa giới hạn lập trình cạnh tranh thực tế. Chúng ta cần một công thức xử lý mỗi con thỏ về cơ bản một lần. 

Khó khăn chính là xác suất chọn được thỏ thay đổi sau mỗi củ cà rốt. Một giải pháp bất cẩn có thể sử dụng xác suất ban đầu (w_i/W) cho mỗi củ cà rốt, nhưng điều đó sẽ bỏ qua tác dụng củng cố. Ví dụ: với trọng số (1,3) và (k=2), trọng số cuối cùng dự kiến ​​chính xác là (1,5,4,5). Chỉ cần thêm (2\cdot(1/4)) và (2\cdot(3/4)) sẽ cho chính xác những giá trị đó trong trường hợp này, nhưng sự trùng hợp ngẫu nhiên đó không biện minh cho việc sử dụng xác suất cố định. Sau củ cà rốt đầu tiên, trọng lượng đã thay đổi nên xác suất thứ hai là ngẫu nhiên. 

Một trường hợp lộ liễu hơn là một con thỏ. Đối với đầu vào (n=1,k=1,w=[5]), câu trả lời chính xác là (6). Bất kỳ phương pháp xử lý xác suất nào vẫn thực hiện phép tính gần đúng đều không cần thiết vì con thỏ duy nhất phải thắng trong mọi trận chiến. Tổng quát hơn, với (n=1), câu trả lời phải luôn là (w_1+k). 

Trọng số bằng nhau cung cấp một cách kiểm tra hữu ích khác. Với (n=3,k=2,w=[1,1,1]), tính đối xứng cho biết mọi con thỏ đều phải có cùng kỳ vọng. Tổng trọng số cuối cùng là (5), nên câu trả lời là (5/3,5/3,5/3). Giải pháp chỉ cập nhật con thỏ lớn nhất hiện tại hoặc giả sử cùng một con thỏ thắng liên tục sẽ vi phạm tính đối xứng này. 

Trọng số ban đầu lớn cũng bộc lộ những sai sót về mặt số học. Giả sử (n=2,k=1,w=[10^9,1]). Mức tăng dự kiến ​​của con thỏ đầu tiên là (10^9/(10^9+1)), trong khi mức tăng dự kiến ​​của con thỏ thứ hai là (1/(10^9+1)). Mẫu số phải là tổng trọng lượng hiện tại, không phải là giá trị vượt quá loại số nguyên hẹp hoặc vô tình bị thay thế bằng trọng số riêng lẻ. 

## Phương pháp tiếp cận 

Cách tiếp cận chính xác và trực tiếp nhất là xem xét mọi chuỗi người chiến thắng có thể xảy ra. Vào mỗi buổi sáng, có thể có (n) người chiến thắng và xác suất của mỗi nhánh phụ thuộc vào tất cả những người chiến thắng trước đó. Do đó, một chuỗi người chiến thắng hoàn chỉnh có khả năng lên tới (n^k). Ngay cả với (n=2) và (k=10^9), điều này vẫn vô vọng. Lập trình động theo số lần mỗi con thỏ thắng cũng không giải quyết được vấn đề cơ bản, bởi vì số phân bố có thể có của (k) thắng giữa (n) con thỏ là (\binom{n+k-1}{n-1}), cũng rất lớn. 

Một mô phỏng lực lượng vũ phu đơn giản hơn có thể tạo ra một chuỗi ngẫu nhiên những người chiến thắng trong các hoạt động (O(k)), nhưng nó tính toán một đường dẫn mẫu thay vì kỳ vọng chính xác mà vấn đề yêu cầu. Việc lặp lại mô phỏng chỉ tạo ra giá trị gần đúng và bản thân (k) đã có thể là (10^9). 

Quan sát hữu ích là mặc dù các trọng số riêng lẻ là ngẫu nhiên nhưng tổng trọng số của chúng hoàn toàn mang tính xác định. Giả sử con thỏ (i) có trọng lượng (X_i(t)) sau (t) củ cà rốt. Trước củ cà rốt tiếp theo, tổng trọng lượng chính xác là (W+t). Trong trận chiến tiếp theo, thỏ (i) giành được một con với xác suất có điều kiện 

[ 
\frac{X_i(t)}{W+t}. 
] 

Chúng ta có thể lấy kỳ vọng có điều kiện về trọng số tiếp theo của nó: 

X_i(t)+\frac{X_i(t)}{W+t}. 
]

Mẫu số không chứa đại lượng ngẫu nhiên, do đó việc lấy lại kỳ vọng sẽ cho kết quả 

\left(1+\frac{1}{W+t}\right)\mathbb E[X_i(t)]. 
] 

Đây là sự đơn giản hóa quan trọng. Quá trình ngẫu nhiên phức tạp đã trở thành một sự tái diễn mang tính quyết định đối với kỳ vọng của mỗi con thỏ. 

Bắt đầu từ (\mathbb E[X_i(0)]=w_i), việc áp dụng lặp lại phép truy toán sẽ cho 

w_i 
\prod_{t=0}^{k-1}\frac{W+t+1}{W+t}. 
] 

Các sản phẩm kính thiên văn: 

\frac{W+k}{W}. 
] 

Do đó trọng lượng dự kiến cuối cùng chỉ đơn giản là 

[ 
\boxed{\mathbb E[X_i(k)] = w_i\frac{W+k}{W}}. 
] 

Cách tiếp cận bạo lực có hiệu quả vì nó tuân theo quy trình ngẫu nhiên thực tế, nhưng nó thất bại vì số lượng lịch sử có thể có là rất lớn. Quan sát cho thấy tổng trọng số có tính xác định cho phép chúng ta thực hiện kỳ ​​vọng từng bước một và kết quả tái diễn sẽ thu gọn thành một tích số thu gọn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^k)) lịch sử | (O(n)) mỗi tiểu bang | Quá chậm | 
| Mô phỏng ngẫu nhiên | (O(k)) trên mỗi mô phỏng | (O(n)) | Không đưa ra kỳ vọng chính xác | 
| Tối ưu | (O(n)) mỗi trường hợp | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng thỏ, số lượng cà rốt (k) và tất cả trọng lượng ban đầu. Tính tổng của chúng (W), vì tổng trọng lượng sau (t) củ cà rốt sẽ luôn là (W+t). 
2. Tính số nhân chung 

[ 
M=\frac{W+k}{W}. 
] 

Hệ số nhân này giống nhau đối với mọi con thỏ. Quá trình ngẫu nhiên ảnh hưởng đến con thỏ nào nhận được từng củ cà rốt, nhưng nó không ảnh hưởng đến hệ số tăng trưởng nhân lên dự kiến. 

1. Với mỗi con thỏ (i), xuất ra 

[ 
w_iM. 
] 

Không cần thiết phải mô phỏng bất kỳ buổi sáng nào hoặc duy trì trọng số đang thay đổi, bởi vì sự tái diễn đã tính đến mọi chuỗi chiến thắng có thể xảy ra. 

1. In các giá trị có đủ độ chính xác thập phân, chẳng hạn như mười chữ số sau dấu thập phân. Lỗi bắt buộc là (10^{-4}), do đó số học dấu phẩy động có độ chính xác kép thông thường là quá đủ. 

### Tại sao nó hoạt động 

Gọi (X_i(t)) là trọng lượng của con thỏ (i) sau (t) củ cà rốt. Tổng trọng lượng tại thời điểm đó được xác định rõ ràng (W+t). Có điều kiện (X_i(t)), xác suất để thỏ (i) nhận được củ cà rốt tiếp theo là (X_i(t)/(W+t)). Do đó, 

X_i(t)\frac{W+t+1}{W+t}. 
] 

Việc lấy kỳ vọng sẽ cho ra hệ số nhân tương tự cho (\mathbb E[X_i(t)]). Bắt đầu từ (w_i), tất cả các hệ số thiên văn, rời khỏi (w_i(W+k)/W). Vì phương trình này tuân theo trực tiếp kỳ vọng có điều kiện ở mỗi bước riêng lẻ nên nó tính đến mọi chuỗi người chiến thắng có thể có một cách chính xác chứ không phải gần đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    out = []

    for _ in range(t):
        n, k = map(int, input().split())
        w = list(map(int, input().split()))

        total = sum(w)
        factor = (total + k) / total

        out.append(" ".join(f"{x * factor:.10f}" for x in w))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã đầu tiên tính toán`total`, là trọng lượng kết hợp ban đầu (W). Bởi vì chính xác một củ cà rốt có trọng lượng bằng một củ được tiêu thụ mỗi ngày nên tổng trọng lượng sau tất cả (k) ngày là (W+k). 

biểu hiện`(total + k) / total`tính toán trực tiếp sản phẩm kính thiên văn. Việc tính toán từng nhân tố một sẽ yêu cầu (k) lần lặp, điều này sẽ làm mất đi mục đích của việc suy ra. 

Mỗi trọng lượng ban đầu được nhân với cùng một hệ số. Phép nhân được thực hiện dưới dạng phép toán dấu phẩy động vì đầu ra được yêu cầu là số thực. 

Số nguyên Python xử lý giá trị lớn của tổng trọng lượng một cách an toàn. Tổng số ban đầu lớn nhất có thể là (10^5\cdot10^9=10^{14}) và phép cộng (10^9) cũng nằm trong phạm vi số nguyên chính xác của Python. Việc chuyển đổi cuối cùng sang dấu phẩy động là an toàn với dung sai lỗi (10^{-4}) bắt buộc. 

Mã xử lý mọi trọng số chính xác một lần sau khi tính tổng, đưa ra công việc tuyến tính về số lượng thỏ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Hộp mẫu đầu tiên có một con thỏ có trọng lượng (2) và một củ cà rốt. 

Tổng số ban đầu là (W=2). Vì chỉ có một con thỏ nên xác suất nó nhận được củ cà rốt là một. Công thức cho 

[ 
2\cdot\frac{2+1}{2}=3. 
] 

| Bước | (W) | (k) | Hệ số nhân | Cân nặng dự kiến ​​| 
| --- | --- | --- | --- | --- | 
| Ban đầu | 2 | 1 | (3/2) | 2 | 
| Cuối cùng | 2 | 1 | (3/2) | 3 | 

Kết quả là`3.00000000`. Ví dụ này xác nhận trường hợp biên (n=1). Công thức không đưa ra bất kỳ sự ngẫu nhiên nhân tạo nào khi người chiến thắng mang tính quyết định. 

### Mẫu 2 

Hộp mẫu thứ hai có hai con thỏ có trọng lượng (1,3) và hai củ cà rốt. 

Tổng số ban đầu là (W=4) và tổng số cuối cùng là (6). Cả hai trọng số dự kiến đều được nhân với 

[ 
\frac{6}{4}=1,5. 
] 

| Thỏ | Trọng lượng ban đầu | (W) | (W+k) | Hệ số nhân | Trọng lượng cuối cùng dự kiến ​​| 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 4 | 6 | 1,5 | 1,5 | 
| 2 | 3 | 4 | 6 | 1,5 | 4,5 | 

Đầu ra là`1.50000000 4.50000000`. Tổng trọng số dự kiến ​​là (6), khớp với tổng cuối cùng xác định. Đây là một kiểm tra tính nhất quán hữu ích cho công thức. 

### Mẫu 3 

Mẫu thứ ba có ba con thỏ bằng nhau, mỗi con bắt đầu từ số (1) và hai củ cà rốt. 

Ở đây (W=3) và (W+k=5), vậy nên mọi con thỏ đều nhận được cùng một hệ số nhân (5/3). 

| Thỏ | Trọng lượng ban đầu | (W) | (W+k) | Trọng lượng cuối cùng dự kiến ​​| 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 5 | (5/3) | 
| 2 | 1 | 3 | 5 | (5/3) | 
| 3 | 1 | 3 | 5 | (5/3) | 

Kết quả là khoảng`1.66666667 1.66666667 1.66666667`. Sự bình đẳng được mong đợi từ tính đối xứng, mặc dù các lần thực hiện ngẫu nhiên riêng lẻ có thể cho các trọng số cuối cùng khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) mỗi trường hợp, (O(\sum n)) tổng thể | Các trọng số được tính tổng và sau đó mỗi con thỏ được xử lý một lần | 
| Không gian | (O(n)) mỗi trường hợp | Trọng số đầu vào và đầu ra được tạo được lưu trữ | 

Vì tổng (n) trên tất cả các trường hợp thử nghiệm nhiều nhất là (10^6), nên toàn bộ dữ liệu đầu vào chỉ yêu cầu tính toán tuyến tính với trọng số (10^6). Giá trị của (k) hoàn toàn không xuất hiện trong thời gian chạy, điều này rất cần thiết vì (k) có thể là (10^9). 

## Trường hợp thử nghiệm 

Các xác nhận bên dưới so sánh các kết quả đầu ra của dấu phẩy động bằng số thay vì yêu cầu trình bày văn bản chính xác. Điều này phù hợp với quy tắc đánh giá dựa trên lỗi của vấn đề.```python
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n, k = map(int, input().split())
        w = list(map(int, input().split()))

        total = sum(w)
        factor = (total + k) / total

        out.append(" ".join(f"{x * factor:.10f}" for x in w))

    sys.stdin = old_stdin
    return "\n".join(out)

def run(inp: str):
    return solve_data(inp).split()

def assert_close(actual, expected, eps=1e-7):
    assert len(actual) == len(expected)
    for a, e in zip(actual, expected):
        assert abs(float(a) - e) <= eps, (a, e)

# Provided samples
sample = """\
3
1 1
2
2 2
1 3
3 2
1 1 1
"""

assert_close(
    run(sample),
    [3.0, 1.5, 4.5, 5 / 3, 5 / 3, 5 / 3]
)

# Minimum-size input: one rabbit, one carrot.
assert_close(
    run("""\
1
1 1
1
"""),
    [2.0]
)

# No probabilistic choice exists with one rabbit, even for huge k.
assert_close(
    run("""\
1
1 1000000000
1000000000
"""),
    [2000000000.0]
)

# All equal weights. Every rabbit must have the same expectation.
assert_close(
    run("""\
1
4 5
2 2 2 2
"""),
    [3.25, 3.25, 3.25, 3.25]
)

# Strongly unequal weights and k=1.
# The expected values are w_i + w_i / total.
assert_close(
    run("""\
1
2 1
1000000000 1
"""),
    [
        1000000000 * 2000000001 / 1000000001,
        2000000001 / 1000000001
    ]
)

# k=0 is not allowed by the original constraints, but this checks
# that the formula has the correct natural boundary behavior.
assert_close(
    run("""\
1
3 0
1 4 7
"""),
    [1.0, 4.0, 7.0]
)

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 1 / 1`|`2`| Số lượng thỏ và cà rốt tối thiểu | 
|`1 / 1 1000000000 / 1000000000`|`2000000000`| Người chiến thắng rất lớn (k) và xác định | 
|`1 / 4 5 / 2 2 2 2`|`3.25`cho mọi con thỏ | Đối xứng và trọng lượng ban đầu bằng nhau | 
|`1 / 2 1 / 1000000000 1`| Khoảng`1000000000.999999999`,`0.000000001`thêm vào trọng lượng thứ hai | Trọng số không bằng nhau và ranh giới một bước | 
|`1 / 3 0 / 1 4 7`|`1 4 7`| Ranh giới không có cà rốt tự nhiên của công thức | 

## Vỏ cạnh 

Trường hợp một con thỏ là hoàn toàn xác định. Đối với đầu vào```
1
1 1
5
```chúng ta có (W=5) và (k=1), do đó hệ số nhân là (6/5). Kết quả là (5\cdot6/5=6). Với (k=10^9), lý luận tương tự sẽ cho (5+10^9). Thuật toán không bao giờ cố gắng lập mô hình phân bố xác suất khi không tồn tại. 

Trọng lượng bằng nhau đòi hỏi kỳ vọng phải bằng nhau vì thỏ có thể hoán đổi cho nhau. Vì```
1
3 2
1 1 1
```tổng số bắt đầu từ (3), tổng số cuối cùng là (5) và mọi con thỏ đều nhận được hệ số nhân (5/3). Đầu ra là (5/3) cho mỗi con thỏ. Các lần thực thi riêng lẻ có thể tạo ra các trọng số như ((3,1,1)) hoặc ((2,2,1)), nhưng giá trị mong đợi của chúng giống hệt nhau. 

Trường hợp một củ cà rốt là trường hợp có thể kiểm tra trực tiếp sự tái phát. Vì```
1
2 1
2 3
```tổng số là (5). Con thỏ thắng với xác suất (2/5), do đó trọng số cuối cùng dự kiến ​​của nó là (2+2/5=2,4). Thỏ thắng hai với xác suất (3/5), cho (3+3/5=3,6). Công thức tạo ra (2\cdot6/5=2.4) và (3\cdot6/5=3.6), khớp chính xác với phép tính trực tiếp. 

(k) rất lớn không được gây ra vòng lặp trong buổi sáng. Vì```
1
2 1000000000
1 1
```tổng ban đầu là (2), do đó hệ số nhân là (1000000002/2=500000001). Cả hai con thỏ đều có trọng lượng cuối cùng dự kiến ​​(500000001). Thuật toán đạt được điều này ngay lập tức, trong khi mô phỏng sẽ yêu cầu một tỷ lần lặp. 

Cuối cùng, các trọng số dự kiến ​​phải luôn có tổng bằng tổng cuối cùng xác định (W+k). Đối với bất kỳ đầu vào nào, việc tính tổng công thức sẽ cho 

# \frac{W+k}{W}\sum_i w_i 

W+k. 
] 

Danh tính này là một công cụ kiểm tra gỡ lỗi hữu ích vì việc triển khai vô tình sử dụng các mẫu số khác nhau hoặc cập nhật hệ số nhân không chính xác thường sẽ vi phạm nó.
