---
title: "CF 104360C - \u0421\u0442\u0430\u0431\u0438\u043b\u044c\u043d\u044b\u0435 \u043f\u0430\u0440\u0430\u043b\u043b\u0435\u043b\u0438"
description: "Chúng tôi được cung cấp một tập hợp các cấp độ kỹ năng của học sinh và chúng tôi muốn chia chúng thành nhiều nhóm gọi là các lớp song song. Trong mỗi lớp, nếu chúng ta sắp xếp học sinh theo kỹ năng thì mỗi cặp liền kề phải khác nhau tối đa một giá trị cố định x."
date: "2026-07-01T17:56:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104360
codeforces_index: "C"
codeforces_contest_name: "\u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438\u043c. \u041c\u0441\u0442\u0438\u0441\u043b\u0430\u0432\u0430 \u041a\u0435\u043b\u0434\u044b\u0448\u0430 - 2021"
rating: 0
weight: 104360
solve_time_s: 53
verified: true
draft: false
---

[CF 104360C - \u0421\u0442\u0430\u0431\u0438\u043b\u044c\u043d\u044b\u0435 \u043f\u0430\u0440\u0430\u043b\u043b\u0435\u043b\u0438](https://codeforces.com/problemset/problem/104360/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một tập hợp các cấp độ kỹ năng của học sinh và chúng tôi muốn chia chúng thành nhiều nhóm gọi là các lớp song song. Trong mỗi lớp, nếu chúng ta sắp xếp học sinh theo kỹ năng thì mỗi cặp liền kề phải khác nhau tối đa một giá trị cố định x. Điều kiện đó buộc mỗi lớp phải chứa những học sinh có thể được “kết nối” thông qua những bước nhảy nhỏ theo thứ tự được sắp xếp một cách hiệu quả mà không có bất kỳ khoảng cách lớn nào. 

Chúng tôi được phép chèn thêm tối đa k học sinh với các giá trị kỹ năng tùy ý. Những học sinh được chèn này không cần phải thuộc về bất kỳ phân phối ban đầu nào và có thể được chọn một cách chiến lược để giúp giảm số lượng lớp bắt buộc. 

Mục tiêu là giảm thiểu số lượng lớp ổn định như vậy mà chúng ta cần sau khi chèn tối đa k giá trị bổ sung một cách tối ưu. 

Kích thước đầu vào lớn, lên tới 200000 sinh viên và cả k và x đều có thể cực lớn, lên tới 10^18. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng trực tiếp việc chèn thêm hoặc thử tất cả các vị trí của học sinh bổ sung. Bất kỳ cách tiếp cận nào tính toán theo từng khoảng trống và chạy theo thời gian tuyến tính hoặc gần tuyến tính đối với dữ liệu đã sắp xếp đều có thể chấp nhận được, trong khi mọi cách tiếp cận bậc hai hoặc tổ hợp trên k đều không thể thực hiện được. 

Một điểm tinh tế quan trọng là các sinh viên được chèn hoàn toàn miễn phí về giá trị. Họ không bị hạn chế phải đến từ những khoảng trống hoặc vị trí hiện có. Điều đó có nghĩa là chúng có thể được sử dụng làm “cầu nối” giữa những khoảng trống lớn, chia tách một cách hiệu quả một bước nhảy lớn thành nhiều bước nhảy nhỏ hơn. 

Một sai lầm ngây thơ xuất hiện khi nghĩ rằng chúng ta có thể tham lam chia mảng thành các đoạn bất cứ khi nào hiệu liền kề vượt quá x. Điều đó đúng khi k = 0, nhưng hoàn toàn sai khi chúng ta có thể chèn giá trị. Ví dụ: nếu chúng ta có [1, 100] và x = 10, một giải pháp đơn giản sẽ cho rằng chúng ta cần hai lớp. Nhưng với k = 1, chúng ta có thể chèn 50 và tạo thành [1, 50, 100], làm cho nó ổn định. 

Một trường hợp phức tạp khác là khi những khoảng trống lớn có thể được thu hẹp một phần. Nếu một khoảng trống có kích thước d, thì không phải lúc nào chúng ta cũng cần chèn d/x theo nghĩa đơn giản; số lượng chính xác dựa trên số điểm trung gian cần thiết để giảm mỗi lần nhảy xuống tối đa x. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua khả năng chèn học sinh thì vấn đề sẽ trở nên đơn giản. Chúng tôi sắp xếp mảng và chia nó thành các phân đoạn tối đa trong đó các chênh lệch liền kề nhiều nhất là x. Mỗi khi thấy khoảng trống lớn hơn x, chúng ta sẽ bắt đầu một lớp mới. Điều này đưa ra một câu trả lời cơ bản. 

Khó khăn đến từ khả năng chèn tới k giá trị. Mỗi lần chèn có thể được sử dụng để giảm khoảng cách lớn. Giả sử chúng ta có hai giá trị được sắp xếp liên tiếp a[i] và a[i+1] với hiệu d. Nếu không có phần chèn thêm, điều này sẽ đóng góp 0 hoặc 1 điểm ngắt tùy thuộc vào việc d ≤ x. Với phần chèn, chúng ta có thể đặt các giá trị trung gian để khoảng cách được phân tách thành các bước nhỏ hơn. Nếu chèn t số vào giữa chúng, chúng ta có thể chia khoảng trống thành các đoạn t+1, mỗi đoạn có kích thước tối đa là x. Vậy ta cần t thỏa mãn (t + 1) * x ≥ d, nghĩa là t ≥ ceil(d/x) - 1. 

Điều này chuyển đổi vấn đề từ “chúng ta có thể kết nối mọi thứ” thành “chúng ta cần sửa chữa bao nhiêu lỗ hổng và chi phí sửa chữa chúng là bao nhiêu”. 

Đầu tiên chúng ta sắp xếp mảng. Sau đó, chúng ta xem xét mọi khoảng trống liền kề lớn hơn x. Mỗi khoảng trống như vậy đóng góp một số lượng phần chèn cần thiết bằng ceil(d / x) - 1. Nếu có đủ k, chúng ta có thể giảm số lượng lớp kết quả bằng cách “trả tiền” cho các khoảng trống cầu nối. Mỗi khoảng trống được bắc cầu sẽ hợp nhất hai phân đoạn riêng biệt trước đó. 

Bây giờ chúng tôi diễn giải lại cấu trúc: không cần chèn thêm, mọi khoảng trống lớn sẽ tạo ra sự tách biệt giữa các thành phần. Nếu chúng ta sửa một khoảng trống bằng cách chèn thêm, chúng ta sẽ hợp nhất hai thành phần thành một. Vì mỗi khoảng trống được hợp nhất làm giảm số lượng thành phần đi một nên chúng tôi muốn chi k để khắc phục càng nhiều khoảng trống càng tốt, ưu tiên những khoảng trống có chi phí nhỏ nhất về số lần chèn bắt buộc.

Vì vậy, chúng tôi tính toán tất cả các khoảng trống vượt quá x, tính chi phí chèn cần thiết của chúng và sắp xếp các chi phí này. Ban đầu, số lớp bằng một cộng với số khoảng trống đó. Sau đó, chúng tôi tham lam lấp đầy những khoảng trống rẻ nhất trong khi vẫn còn ngân sách k, mỗi lần sửa sẽ giảm số lượng lớp xuống một. 

Ý tưởng chính là chúng ta không bao giờ cần xem xét sự tương tác giữa các phần tử không liền kề, bởi vì chỉ những khoảng trống được sắp xếp liền kề mới xác định được sự phân tách. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng chèn lực Brute | Hàm mũ | O(n) | Quá chậm | 
| Sắp xếp + chi phí chênh lệch + sáp nhập tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng kỹ năng của học sinh theo thứ tự không giảm. Việc sắp xếp là cần thiết vì tính ổn định được xác định theo sự khác biệt liền kề sau khi sắp xếp. 
2. Duyệt mảng đã sắp xếp và tính tất cả các hiệu liền kề. Bất cứ khi nào chênh lệch d vượt quá x, hãy ghi lại nó dưới dạng “khoảng cách ngắt” và tính toán cần bao nhiêu lần chèn để làm cho nó hợp lệ: t = ceil(d / x) - 1. Giá trị này biểu thị chi phí để sửa chữa khoảng cách đó để hai bên có thể thuộc cùng một lớp. 
3. Đếm số lớp ban đầu là một cộng với số khoảng trống trong đó d > x. Mỗi khoảng trống như vậy sẽ chia mảng đã sắp xếp thành các phân đoạn ổn định riêng biệt trước khi chèn vào. 
4. Tập hợp tất cả chi phí sửa chữa đã tính toán cho những khoảng trống này vào danh sách. Mỗi chi phí thể hiện mức độ đắt đỏ của việc hợp nhất hai thành phần liền kề. 
5. Sắp xếp danh sách chi phí theo thứ tự tăng dần. Điều này cho phép chúng tôi ưu tiên hợp nhất các phân khúc có chi phí kết nối rẻ nhất. 
6. Bắt đầu từ chi phí nhỏ nhất, liên tục sử dụng k sẵn có để “trả” cho việc sửa chữa lỗ hổng. Mỗi lần chúng ta có đủ khả năng chi trả, hãy giảm k và giảm số lớp đi một. 
7. Dừng lại khi hết k hoặc không còn khoảng trống nào để sửa. Số thành phần còn lại chính là đáp án. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, cấu trúc là tuyến tính và mọi ranh giới lớp chỉ được xác định bằng khoảng cách vượt quá x. Mỗi ranh giới như vậy là độc lập theo nghĩa là việc bắc cầu chỉ phụ thuộc vào việc chèn đủ các giá trị trung gian cho khoảng cụ thể đó. Bất kỳ giải pháp hợp lệ nào đều tương ứng với việc chọn một tập hợp con các khoảng trống để sửa chữa và mỗi lần sửa chữa sẽ giảm số lượng lớp chính xác một trong khi tiêu tốn một chi phí cố định. Vì chi phí là độc lập và cộng gộp nên việc chọn chi phí nhỏ nhất trước tiên là tối ưu trong một ngân sách cố định, đây là một lập luận trao đổi tham lam trực tiếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k, x = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()

    gaps = []

    for i in range(n - 1):
        d = a[i + 1] - a[i]
        if d > x:
            t = (d + x - 1) // x - 1
            gaps.append(t)

    components = 1 + len(gaps)

    gaps.sort()

    for cost in gaps:
        if cost <= k:
            k -= cost
            components -= 1
        else:
            break

    print(components)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp các học sinh sao cho tất cả các ràng buộc về độ ổn định giảm xuống các sai phân liền kề. Vòng lặp vượt qua các khoảng trống chỉ trích xuất những khoảng vi phạm ngưỡng x. Công thức`(d + x - 1) // x - 1`tính toán cần bao nhiêu giá trị được chèn để chia một khoảng trống lớn thành các bước hợp lệ có kích thước tối đa là x. 

Số lượng thành phần ban đầu là số lượng các phân đoạn không thể tránh khỏi khi không sử dụng phần chèn thêm. Mỗi khoảng trống tương ứng với một hoạt động hợp nhất tiềm năng và mỗi lần hợp nhất đều có chi phí về số lần chèn cần thiết. Việc sắp xếp các chi phí này đảm bảo rằng chúng tôi luôn chi k cho việc hợp nhất rẻ nhất trước tiên, tối đa hóa việc giảm số lượng thành phần. 

Câu trả lời cuối cùng chỉ đơn giản là có bao nhiêu phân đoạn còn lại sau khi thực hiện càng nhiều thao tác hợp nhất càng tốt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 8, k = 2, x = 3 

a = [1, 1, 5, 8, 12, 13, 20, 22] 

Mảng sắp xếp đã được đưa ra. 

Chúng tôi tính toán các khoảng trống: 

| tôi | một [tôi] | a[i+1] | khác d | d > x | chi phí | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | không | - | 
| 1 | 1 | 5 | 4 | vâng | trần(4/3)-1 = 1 | 
| 2 | 5 | 8 | 3 | không | - | 
| 3 | 8 | 12 | 4 | vâng | 1 | 
| 4 | 12 | 13 | 1 | không | - | 
| 5 | 13 | 20 | 7 | vâng | trần(7/3)-1 = 2 | 
| 6 | 20 | 22 | 2 | không | - | 

Các thành phần ban đầu = 4 (ba khoảng trống vi phạm cộng một). 

Chúng tôi sắp xếp chi phí: [1, 1, 2]. 

Chúng ta có k = 2. 

Chúng tôi lấy chi phí 1 → k = 1, thành phần = 3. 

Chúng ta lấy chi phí 1 → k = 0, thành phần = 2. 

Chúng tôi không thể lấy chi phí 2. 

Câu trả lời cuối cùng là 2 thành phần. 

Dấu vết này cho thấy rằng nhiều lỗ hổng có thể được sửa chữa một phần một cách độc lập và việc lựa chọn tham lam về chi phí sửa chữa nhỏ nhất sẽ trực tiếp tối đa hóa số lượng sáp nhập mà chúng ta có thể chi trả. 

### Ví dụ 2 

đầu vào: 

n = 6, k = 0, x = 5 

a = [1, 2, 3, 100, 101, 102] 

Khoảng trống: 

| tôi | khác d | d > x | 
| --- | --- | --- | 
| 1-2 | 1 | không | 
| 2-3 | 1 | không | 
| 3-4 | 97 | vâng | 
| 4-5 | 1 | không | 
| 5-6 | 1 | không | 

Chỉ tồn tại một khoảng trống lớn nên các thành phần = 2. 

Vì k = 0 nên không thể sửa được. Câu trả lời cuối cùng vẫn là 2. 

Điều này chứng tỏ trường hợp cơ bản trong đó nghiệm rút gọn chính xác việc đếm các điểm gián đoạn theo thứ tự được sắp xếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, tất cả các hoạt động khác là tuyến tính hoặc sắp xếp chi phí chênh lệch | 
| Không gian | O(n) | Lưu trữ cho mảng và danh sách các khoảng trống | 

Các ràng buộc cho phép tối đa 200000 phần tử, do đó, giải pháp O(n log n) phù hợp thoải mái trong giới hạn thời gian và bộ nhớ bổ sung tuyến tính không đáng kể dưới 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.readline()  # placeholder, replace with solve() capturing output

# corrected runner
def run(inp: str) -> str:
    import sys
    from io import StringIO
    sys.stdin = StringIO(inp)
    from contextlib import redirect_stdout
    out = StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample-like cases
assert run("8 2 3\n1 1 5 8 12 13 20 22\n") == "2"

# minimum case
assert run("1 0 10\n5\n") == "1"

# all equal
assert run("5 0 1\n10 10 10 10 10\n") == "1"

# no k, large gaps
assert run("4 0 1\n1 100 200 300\n") == "3"

# enough k to fully connect
assert run("3 100 1\n1 10 20\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | cấu trúc tối thiểu | 
| tất cả đều bình đẳng | 1 | không có khoảng trống | 
| không k có khoảng trống lớn | nhiều | phân đoạn cơ sở | 
| k lớn | 1 | sáp nhập đầy đủ | 

## Vỏ cạnh 

Khi tất cả các giá trị đều bằng nhau, việc sắp xếp sẽ tạo ra khoảng cách bằng 0, do đó không tồn tại ranh giới lớp. Thuật toán trả về chính xác một thành phần vì danh sách khoảng trống và không cần sửa chữa. 

Khi k bằng 0, thuật toán suy biến thành các khoảng trống đếm trong đó chênh lệch liền kề vượt quá x. Mỗi khoảng trống như vậy sẽ phân chia vĩnh viễn cấu trúc và vì không thể hợp nhất nên vòng lặp tham lam không bao giờ kích hoạt. 

Khi k cực kỳ lớn, mọi khoảng cách đều có thể được sửa chữa. Mỗi lần sửa chữa sẽ hợp nhất hai thành phần liền kề cho đến khi chỉ còn lại một thành phần. Thuật toán đạt đến trạng thái này một cách tự nhiên vì mọi chi phí đều được xử lý theo thứ tự tăng dần và tất cả đều phải chăng. 

Khi các khoảng trống yêu cầu nhiều lần chèn, công thức chi phí sẽ đảm bảo tính toán chính xác. Khoảng cách có kích thước 10 với x = 3 yêu cầu phải có ceil(10/3)-1 = 3 lần chèn, phản ánh rằng cần có ba điểm trung gian để giữ tất cả các bước trong giới hạn.
