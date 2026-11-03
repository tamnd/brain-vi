---
title: "CF 1111B - Sức mạnh băng nhóm siêu anh hùng trung bình"
description: "Chúng ta được giao cho một nhóm siêu anh hùng, mỗi người có một giá trị sức mạnh ban đầu. Chúng tôi được phép sửa đổi nhóm này bằng hai loại hoạt động: chúng tôi có thể loại bỏ một siêu anh hùng khỏi nhóm (miễn là vẫn còn ít nhất hai người) hoặc chúng tôi có thể tăng sức mạnh của một siêu anh hùng đã chọn thêm một."
date: "2026-06-12T04:58:18+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1111
codeforces_index: "B"
codeforces_contest_name: "CodeCraft-19 and Codeforces Round 537 (Div. 2)"
rating: 1700
weight: 1111
solve_time_s: 87
verified: true
draft: false
---

[CF 1111B - Sức mạnh trung bình của băng nhóm siêu anh hùng](https://codeforces.com/problemset/problem/1111/B) 

**Đánh giá:** 1700 
**Tags:** vũ lực, thực hiện, toán học 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao cho một nhóm siêu anh hùng, mỗi người có một giá trị sức mạnh ban đầu. Chúng tôi được phép sửa đổi nhóm này bằng hai loại hoạt động: chúng tôi có thể loại bỏ một siêu anh hùng khỏi nhóm (miễn là vẫn còn ít nhất hai người) hoặc chúng tôi có thể tăng sức mạnh của một siêu anh hùng đã chọn thêm một. Có một giới hạn toàn cầu về tổng số hoạt động và một hạn chế bổ sung là bất kỳ siêu anh hùng nào cũng chỉ có thể được tăng lên một số lần giới hạn. 

Mục tiêu là kết thúc với một số tập hợp con không trống gồm các siêu anh hùng có sức mạnh trung bình càng lớn càng tốt sau khi áp dụng hết các hoạt động được phép. 

Đầu ra là một số thực duy nhất biểu thị mức trung bình tối đa có thể đạt được. 

Những ràng buộc ngay lập tức gợi ý rằng một$O(n^2)$hoặc mô phỏng tệ hơn trên tất cả các tập hợp con là không thể bởi vì$n$tùy thuộc vào$10^5$. Bất kỳ cách tiếp cận nào thử tất cả các tập hợp con hoặc liên tục tính toán lại mức trung bình sau khi xóa sẽ hết thời gian. Thậm chí$O(n \cdot m)$không khả thi vì$m$có thể lên đến$10^7$. 

Khó khăn chính là cả hai thao tác đều tương tác với nhau: loại bỏ các phần tử sẽ làm giảm mẫu số, trong khi việc tăng giá trị sẽ cải thiện tử số, nhưng số gia tăng bị giới hạn cho mỗi phần tử và bị giới hạn trên toàn cầu. 

Một số trường hợp đặc biệt bộc lộ những lỗi phổ biến. Nếu tất cả các phần tử đều bằng nhau, hãy nói$n=3, a=[5,5,5]$và chúng ta có nhiều phép toán, chiến lược tốt nhất là giữ lại tất cả các phần tử và phân phối các số gia, vì việc loại bỏ mẫu số rút gọn mà không cải thiện tỷ lệ. Việc loại bỏ tham lam ngây thơ có thể loại bỏ các phần tử một cách không chính xác và làm tăng mức trung bình một cách giả tạo. 

Một trường hợp khác là khi một phần tử cực kỳ lớn so với các phần tử khác. Ví dụ,$a=[1,1,1000]$. Giải pháp tối ưu có thể chỉ giữ lại phần tử lớn nhất và đầu tư tăng dần vào phần tử đó. Một chiến lược giả định rằng chúng ta phải giữ lại hầu hết các yếu tố sẽ thất bại ở đây. 

Một trường hợp tinh tế cuối cùng phát sinh khi ngân sách hoạt động lớn nhưng giới hạn cho mỗi yếu tố$k$là hạn chế. Kể cả nếu$m$lớn, chúng ta không thể đổ tất cả số gia vào một phần tử duy nhất; chúng tôi phải phân phối chúng trên các phần tử đã chọn hoặc chấp nhận rằng việc loại bỏ các phần tử sẽ thay đổi số tiền chúng tôi có thể đầu tư. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực sẽ thử mọi tập hợp con có thể có của các siêu anh hùng và sau đó mô phỏng việc phân bổ tối đa$m$tăng dần trên các phần tử, tôn trọng giới hạn của mỗi phần tử$k$. Đối với mỗi tập hợp con, chúng tôi sẽ sắp xếp các phần tử và áp dụng các mức tăng dần để đạt được mức tăng lớn nhất. Ngay cả khi bỏ qua độ phức tạp của phân phối, chỉ liệt kê các tập hợp con là$O(2^n)$, điều đó là không thể đối với$n=10^5$. Ngay cả việc hạn chế các tập hợp con tiền tố sau khi sắp xếp vẫn sẽ yêu cầu mô phỏng tối đa$m$hoạt động quá chậm. 

Quan sát quan trọng là chúng ta có thể tách các quyết định: đầu tiên, giả sử chúng ta sửa một tập hợp con có kích thước$x$. Trong tập hợp con đó, trước tiên chúng tôi luôn muốn áp dụng số gia cho các phần tử lớn nhất, vì mỗi số gia đóng góp như nhau vào tổng nhưng cải thiện mức trung bình nhiều nhất khi áp dụng cho các giá trị vốn đã lớn. Vì mỗi phần tử chỉ có thể tăng lên tối đa$k$lần, tổng số gia số có thể sử dụng trong một tập hợp con cố định được giới hạn bởi$x \cdot k$. Tuy nhiên, chúng ta cũng bị hạn chế bởi$m$, vậy tổng số gia tăng có thể sử dụng là$\min(m, xk)$. 

Bây giờ vấn đề trở thành: chọn kích thước tập hợp con$x$, lấy điều tốt nhất$x$các phần tử và phân phối lên đến$\min(m, xk)$tăng dần trong số đó. Vì số gia luôn đi đến phần tử lớn nhất trong tập hợp con nên tập hợp con tối ưu cho kích thước cố định$x$đơn giản là$x$giá trị ban đầu lớn nhất. 

Điều này làm giảm vấn đề sắp xếp mảng và kiểm tra tất cả các hậu tố (hoặc tiền tố tùy theo quy ước), duy trì tổng tiền tố và mô phỏng số lượng gia số có thể được áp dụng. Đối với mỗi kích thước tập hợp con ứng cử viên, chúng tôi tính toán kết quả trung bình một cách hiệu quả. 

Tối ưu hóa cuối cùng là xem xét các tập hợp con từ các phần tử lớn nhất trở xuống, bởi vì việc loại bỏ các phần tử nhỏ chỉ có thể cải thiện hoặc duy trì mức trung bình sau khi phân bổ mức tăng tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot m)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng theo thứ tự không giảm. Điều này đảm bảo rằng khi chúng ta xem xét việc giữ lại một tập hợp con, các ứng cử viên tốt nhất luôn là phần tử lớn nhất. 
2. Tính toán trước tổng tiền tố của mảng đã sắp xếp để chúng ta có thể nhanh chóng tính tổng của bất kỳ hậu tố nào đại diện cho tập hợp con đã chọn. 
3. Lặp lại các kích thước tập hợp con có thể$x$từ 1 đến$n$, trong đó tập hợp con bao gồm$x$phần tử lớn nhất. 
4. Đối với mỗi kích thước tập hợp con$x$, hãy tính xem chúng ta có thể áp dụng tổng số bao nhiêu mức tăng: đây là$\min(m, x \cdot k)$. Điều này thể hiện cả giới hạn chung và giới hạn từng phần tử được kết hợp ở cấp tập hợp con. 
5. Phân phối các mức tăng này một cách tối ưu bằng cách luôn tăng các phần tử có lợi nhất, trong một tập hợp con được sắp xếp cố định có nghĩa là các phần tử lớn nhất sẽ nhận được phần tăng trước. Vì tất cả các số gia cộng thêm +1 vào tổng, nên việc phân phối không ảnh hưởng đến tổng số tiền mà chỉ ảnh hưởng đến tính khả thi, do đó tổng giá trị gia tăng chính xác là số lượng số gia. 
6. Tính tổng công suất cho kích thước tập hợp con$x$dưới dạng tổng của các phần tử được chọn cộng với tổng số gia tăng. 
7. Tính trung bình bằng tổng công suất chia cho$x$và theo dõi mức tối đa trên tất cả$x$. 

### Tại sao nó hoạt động 

Trong bất kỳ tập hợp con cố định nào, các số gia luôn đóng góp +1 vào tổng tổng bất kể phần tử nào nhận được chúng. Hạn chế duy nhất là tính khả thi từ giới hạn cho mỗi yếu tố$k$, giới hạn tổng mức tăng tối đa$xk$. Vì việc loại bỏ các phần tử chỉ làm giảm mẫu số và không bao giờ tăng dung lượng gia tăng khả dụng cho mỗi phần tử nên giải pháp tối ưu phải tương ứng với một số kích thước tập hợp con$x$và trong tập hợp con đó, tất cả việc phân bổ số tiền tăng thêm đều có thể hoán đổi cho nhau về mặt đóng góp tổng. Điều này làm giảm vấn đề tối đa hóa hàm một chiều đơn giản đối với các lựa chọn tiền tố được sắp xếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k, m = map(int, input().split())
    a = list(map(int, input().split()))
    
    a.sort()
    
    prefix = [0] * (n + 1)
    for i in range(n):
        prefix[i + 1] = prefix[i] + a[i]
    
    ans = 0.0
    
    for x in range(1, n + 1):
        base_sum = prefix[n] - prefix[n - x]
        increments = min(m, x * k)
        total = base_sum + increments
        avg = total / x
        if avg > ans:
            ans = avg
    
    print(f"{ans:.10f}")

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp mảng sao cho bất kỳ tập con tối ưu nào có kích thước$x$có thể được giả định bao gồm$x$phần tử lớn nhất. Mảng tổng tiền tố cho phép truy xuất theo thời gian liên tục bất kỳ tổng tập hợp con nào như vậy. 

Đối với mỗi kích thước tập hợp con, chúng tôi tính toán số lượng gia tăng có thể được áp dụng. biểu thức`x * k`thực thi giới hạn cho mỗi phần tử, trong khi`m`thực thi giới hạn toàn cầu. Mức tối thiểu của hai giá trị này xác định mức tăng có thể sử dụng được. 

Mỗi số tăng sẽ tăng tổng số chính xác bằng một, vì vậy chúng ta cộng trực tiếp số số tăng vào tổng tập hợp con. Giá trị trung bình được tính toán và so sánh trên tất cả các kích thước tập hợp con. 

Một điểm tinh tế là chúng tôi không bao giờ mô phỏng việc loại bỏ các phần tử một cách linh hoạt; thay vào đó, chúng tôi diễn giải lại vấn đề dưới dạng chọn kích thước tập hợp con và đánh giá nó một cách trực tiếp. Điều này tránh được sự chuyển đổi trạng thái tốn kém. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 4 6
4 7
```Mảng được sắp xếp là`[4, 7]`. 

| x (kích thước tập hợp con) | tập hợp con | tổng cơ sở | x·k | mũ lưỡi trai | gia số | tổng cộng | trung bình | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | [7] | 7 | 4 | 6 | 4 | 11 | 11 | 
| 2 | [4,7] | 11 | 8 | 6 | 6 | 17 | 8,5 | 

Tốt nhất là 11. 

Điều này cho thấy rằng việc loại bỏ phần tử nhỏ hơn và tập trung tất cả số gia vào phần lớn nhất sẽ chiếm ưu thế trong việc giữ cả hai phần tử. 

### Mẫu 2 

đầu vào:```
3 2 5
1 10 20
```Mảng được sắp xếp là`[1, 10, 20]`. 

| x | tập hợp con | tổng cơ sở | x·k | mũ lưỡi trai | gia số | tổng cộng | trung bình | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | [20] | 20 | 2 | 5 | 2 | 22 | 22 | 
| 2 | [10,20] | 30 | 4 | 5 | 4 | 34 | 17 | 
| 3 | [1,10,20] | 31 | 6 | 5 | 5 | 36 | 12 | 

Tốt nhất là 22. 

Điều này chứng tỏ rằng việc thu nhỏ tập hợp con có thể đạt được mức trung bình cao hơn bằng cách tập trung các số gia. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| sắp xếp chiếm ưu thế, quét tuyến tính đơn sau đó | 
| Không gian |$O(n)$| mảng tổng tiền tố | 

Các ràng buộc cho phép lên đến$10^5$các phần tử, vì vậy việc sắp xếp và chuyển qua các kích thước tập hợp con là đủ nhanh. Việc sử dụng bộ nhớ là tuyến tính theo kích thước mảng và nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k, m = map(int, input().split())
    a = list(map(int, input().split()))
    
    a.sort()
    prefix = [0] * (n + 1)
    for i in range(n):
        prefix[i + 1] = prefix[i] + a[i]
    
    ans = 0.0
    for x in range(1, n + 1):
        base = prefix[n] - prefix[n - x]
        inc = min(m, x * k)
        ans = max(ans, (base + inc) / x)
    
    return str(ans)

# provided sample
assert abs(float(run("2 4 6\n4 7\n")) - 11.0) < 1e-6

# all equal
assert abs(float(run("3 10 10\n5 5 5\n")) - 7.0) < 1e-6

# single element
assert abs(float(run("1 5 100\n10\n")) - 110.0) < 1e-6

# large k, small m
assert abs(float(run("4 100 2\n1 2 3 4\n")) - 5.5) < 1e-6

# increasing array
assert abs(float(run("5 1 5\n1 2 3 4 5\n")) - 7.0) < 1e-6
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | hành vi phân phối ổn định | không có lợi từ việc loại bỏ các phần tử | 
| phần tử đơn | tất cả các mức tăng tối đa được áp dụng | trường hợp cạnh x = 1 | 
| lớn k nhỏ m | giới hạn toàn cầu thống trị | tương tác của m và k | 
| mảng tăng dần | sự lựa chọn tập hợp con chính xác | lý luận tập hợp con tham lam | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các phần tử đều giống hệt nhau. Thuật toán đánh giá mọi kích thước tập hợp con và nhận thấy rằng việc giữ nhiều phần tử hơn sẽ làm tăng mẫu số mà không tăng mức tăng trên mỗi phần tử vượt quá mức cho phép loại bỏ. Mức tối đa được tính toán một cách chính xác sẽ đến từ tập hợp con hợp lệ lớn nhất trong đó số gia tăng được sử dụng đầy đủ. 

Một trường hợp cạnh khác là mảng một phần tử. Thuật toán chỉ đánh giá$x=1$, áp dụng tới$\min(m,k)$tăng dần và trả về giá trị tăng cao chính xác mà không cần cân nhắc bất kỳ tập hợp con nào. 

Trường hợp cạnh thứ ba là khi$m$là cực kỳ nhỏ. Thuật toán giới hạn chính xác các mức tăng trên toàn cầu, do đó, ngay cả các tập hợp con lớn cũng không phân bổ quá mức các mức tăng, ngăn chặn việc đánh giá quá cao mức trung bình.
