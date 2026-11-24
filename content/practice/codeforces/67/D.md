---
title: "CF 67D - Thí nghiệm quang học"
description: "Mỗi tia đi vào hộp qua một lỗ ở bên trái và đi ra qua một lỗ ở bên phải. Thứ tự các lỗ ở cả hai bên đều quan trọng."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp"]
categories: ["algorithms"]
codeforces_contest: 67
codeforces_index: "D"
codeforces_contest_name: "Manthan 2011"
rating: 1900
weight: 67
solve_time_s: 113
verified: true
draft: false
---

[CF 67D - Thí nghiệm quang học](https://codeforces.com/problemset/problem/67/D) 

**Xếp hạng:** 1900 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi tia đi vào hộp qua một lỗ ở bên trái và đi ra qua một lỗ ở bên phải. Thứ tự các lỗ ở cả hai bên đều quan trọng. Một tia được xác định bằng nhãn của nó, và hai hoán vị đầu vào mô tả nơi mỗi tia xuất hiện ở phía vào và nơi nó xuất hiện ở phía ra. 

Hai tia cắt nhau nếu thứ tự tương đối của chúng thay đổi giữa hai cạnh. Giả sử tia`a`xuất hiện trước tia`b`ở phía bên trái. Nếu như`a`xuất hiện sau`b`ở phía bên phải, hai tia phải cắt nhau ở đâu đó bên trong hộp. 

Nhiệm vụ yêu cầu tập hợp con lớn nhất của các tia sao cho mọi cặp trong tập hợp con đều giao nhau. Trong ngôn ngữ đồ thị, chúng ta muốn kích thước của cụm lớn nhất trong đồ thị giao nhau của các tia, nhưng hình học cho chúng ta nhiều cấu trúc hơn so với một bài toán đồ thị tổng quát. 

Những hạn chế là thách thức chính.`n`có thể lớn như`10^6`, điều này loại trừ bất cứ điều gì bậc hai ngay lập tức. Thậm chí`O(n log^2 n)`cần triển khai cẩn thận bằng Python ở quy mô này. Chúng tôi cần sạch sẽ`O(n log n)`giải pháp có hệ số không đổi thấp và mức sử dụng bộ nhớ duy trì ở mức tuyến tính nếu có thể. 

Một điểm tinh tế là các tia được cho bởi nhãn chứ không phải trực tiếp bởi vị trí. Đầu tiên chúng ta cần xây dựng lại vị trí của mọi tia ở cả hai phía trước khi có thể suy luận về sự giao nhau. 

Một sai lầm dễ mắc phải khác là nhầm lẫn giữa "giao nhau theo cặp" với "tất cả đều giao nhau tại một điểm chung". Đây là những điều kiện khác nhau về mặt hình học. Ví dụ:```
3
1 2 3
3 1 2
```All three rays do not pairwise intersect. Tia`(1,2)`giao nhau và`(1,3)`cắt nhau nhưng`(2,3)`không. Câu trả lời đúng là`2`. 

Việc thực hiện bất cẩn mà chỉ đếm xem mỗi cung tham gia vào bao nhiêu nghịch đảo có thể kết luận sai rằng cả ba cung đều thuộc cùng một nhóm. 

Một trường hợp cạnh nguy hiểm khác đã bị đảo ngược thứ tự:```
5
1 2 3 4 5
5 4 3 2 1
```Mọi cặp đều giao nhau nên đáp án là`5`. 

Một thuật toán chỉ tìm kiếm các mẫu cục bộ thay vì cấu trúc toàn cục có thể bỏ lỡ sự đảo ngược hoàn toàn này. 

Trường hợp nhỏ nhất cũng có vấn đề:```
1
1
1
```Câu trả lời là`1`, bởi vì một tia đơn lẻ tự nó tạo thành một nhóm hợp lệ. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đầu tiên hãy tính thứ tự của các tia ở bên trái và bên phải. Sau đó, với mỗi cặp tia, hãy kiểm tra xem thứ tự của chúng có thay đổi không. Điều này cho chúng ta một biểu đồ giao nhau. Sau đó, chúng ta sẽ cần tập hợp con lớn nhất mà mọi cặp đều giao nhau. 

Chỉ kiểm tra cặp thôi đã có giá rồi`O(n^2)`, điều đó là không thể đối với`n = 10^6`. Ngay cả việc lưu trữ biểu đồ cũng sẽ cần hàng nghìn tỷ cạnh trong trường hợp xấu nhất. Các thuật toán cụm cực đại tổng quát kém hơn nhiều so với thuật toán bậc hai nên chúng ta cần khai thác cấu trúc đặc biệt của bài toán này. 

Quan sát quan trọng là hình học biến mọi thứ thành hoán vị. 

Giả sử chúng ta liệt kê các tia theo thứ tự chúng xuất hiện ở phía bên trái. Đối với mỗi tia, viết vị trí của nó ở phía bên phải. Ta thu được một hoán vị:```
p[i] = position on right side of the ray appearing at left position i
```Hai tia cắt nhau đúng khi thứ tự của chúng đảo ngược, nghĩa là:```
i < j but p[i] > p[j]
```Bây giờ hãy nghĩ về một nhóm trong đó mỗi cặp giao nhau. Đối với mỗi cặp trong nhóm, tia bên trái trước đó phải có vị trí bên phải lớn hơn. Điều đó có nghĩa là trình tự của`p`giá trị của nhóm đang giảm nghiêm trọng. 

Vì vậy, vấn đề trở thành: 

Tìm dãy con giảm dài nhất của hoán vị. 

Đây là một vấn đề cổ điển. Chúng ta có thể giải quyết nó trong`O(n log n)`sử dụng kỹ thuật sắp xếp kiên nhẫn được sử dụng cho LIS. Dãy con giảm dài nhất có thể được tìm thấy bằng cách tính LIS trên các giá trị phủ định hoặc bằng cách đảo ngược trực tiếp logic so sánh. 

Lúc này bài toán hình học hoàn toàn biến mất. Toàn bộ thách thức trở thành một vấn đề về dãy con trên hoán vị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n²) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng thứ tự vào và mảng thứ tự thoát. 
2. Xây dựng một mảng`pos_right`Ở đâu`pos_right[x]`lưu trữ vị trí của tia`x`ở phía bên phải. 

Điều này cho phép chúng ta nhanh chóng ánh xạ bất kỳ tia nào tới vị trí thoát của nó. 
3. Đi qua các tia theo thứ tự bên trái và xây dựng một hoán vị`p`. 

Đối với mỗi tia`x`trong mảng bên trái:```
p.append(pos_right[x])
```Hiện nay`p[i]`cho chúng tôi biết nơi`i`- tia bên trái xuất hiện ở phía bên phải. 
4. Quan sát thấy hai tia giao nhau một cách chính xác khi giá trị tương ứng của chúng tạo thành sự đảo ngược. 

Nếu như`i < j`Và`p[i] > p[j]`, thứ tự đảo ngược nên các tia giao nhau. 
5. Một nhóm mà mọi cặp giao nhau đều tương ứng chính xác với một dãy con giảm chặt chẽ trong`p`. 

Bên trong một dãy con như vậy, mọi phần tử trước đều lớn hơn mọi phần tử sau, do đó mỗi cặp tạo thành một đảo ngược. 
6. Tính độ dài của dãy con giảm dài nhất. 

Thay vì triển khai LDS trực tiếp, hãy phủ định các giá trị và tính LIS trên`-p[i]`. 
7. Duy trì một mảng`tails`.`tails[k]`lưu trữ giá trị kết thúc nhỏ nhất có thể cho chuỗi con có độ dài tăng dần`k + 1`. 
8. Với mỗi giá trị`v = -p[i]`, tìm kiếm nhị phân vị trí đầu tiên trong`tails`Ở đâu`v`có thể thay thế một giá trị hiện có. 

Nếu không có vị trí như vậy tồn tại, hãy thêm`v`. 
9. Kích thước cuối cùng của`tails`là câu trả lời. 

### Tại sao nó hoạt động 

Bất biến tới hạn là sự tương đương giữa giao điểm theo cặp và nghịch đảo trong hoán vị. 

Sau khi chuyển đổi hình học sang dạng hoán vị, mọi nhóm hợp lệ sẽ tương ứng với các chỉ số trong đó mọi cặp đều thỏa mãn:```
i < j and p[i] > p[j]
```Điều kiện đó chính xác là định nghĩa của một dãy con giảm nghiêm ngặt. 

Thuật toán LIS duy trì giá trị kết thúc tối ưu cho mỗi độ dài chuỗi con. Việc thay thế một cái đuôi lớn hơn bằng một cái nhỏ hơn không bao giờ ảnh hưởng đến các khả năng trong tương lai, bởi vì những phần cuối nhỏ hơn luôn linh hoạt hơn khi mở rộng. Theo đối số chính xác tiêu chuẩn để sắp xếp kiên nhẫn, độ dài cuối cùng bằng độ dài chuỗi con tối đa có thể. 

Vì chúng tôi chạy LIS trên các giá trị phủ định, nên chúng tôi thu được độ dài chuỗi con giảm dần dài nhất trong hoán vị ban đầu, bằng với nhóm giao nhau theo cặp lớn nhất. 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline

def solve():
    n = int(input())

    left = list(map(int, input().split()))
    right = list(map(int, input().split()))

    pos_right = [0] * (n + 1)

    for i, x in enumerate(right):
        pos_right[x] = i

    tails = []

    for x in left:
        v = -pos_right[x]

        idx = bisect_left(tails, v)

        if idx == len(tails):
            tails.append(v)
        else:
            tails[idx] = v

    print(len(tails))

solve()
```Bước đầu tiên xây dựng lại vị trí bên phải của mỗi tia. Vì nhãn tia là duy nhất và có phạm vi từ`1`ĐẾN`n`, việc tra cứu mảng nhanh hơn và nhẹ hơn so với từ điển. 

Cấu trúc hoán vị được ẩn bên trong vòng lặp chính. Đối với mỗi tia theo thứ tự bên trái, chúng ta ngay lập tức lấy vị trí bên phải của nó và phủ định nó. Phủ định chuyển đổi bài toán dãy con giảm dài nhất thành bài toán dãy con tăng dài nhất tiêu chuẩn. 

các`tails`array là cấu trúc sắp xếp kiên nhẫn tiêu chuẩn. Giả định`tails[2] = -7`. Điều đó có nghĩa là chúng ta đã tìm thấy một dãy con tăng dần có độ dài`3`kết thúc ở giá trị`-7`và trong số tất cả các dãy con như vậy, giá trị cuối cùng này càng nhỏ càng tốt. 

sử dụng`bisect_left`duy trì sự nghiêm ngặt một cách chính xác. Vì hoán vị chứa các giá trị riêng biệt nên hành vi tăng và không giảm nghiêm ngặt hoàn toàn trùng khớp sau khi phủ định. 

Một sai lầm dễ mắc phải là sử dụng nhãn tia thực tế thay vì vị trí. Các giao điểm chỉ phụ thuộc vào thứ tự tương đối chứ không phụ thuộc vào số tia. 

Một lỗi phổ biến khác là quên rằng các vị trí được lập chỉ mục bằng 0 khi triển khai trong khi mô tả toán học thường được lập chỉ mục một. Sự lựa chọn không thành vấn đề miễn là thứ tự được duy trì một cách nhất quán. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 4 5 2 3
3 4 2 1 5
```Đầu tiên tính toán các vị trí bên phải: 

| Ray | Đúng Vị Trí | 
| --- | --- | 
| 1 | 3 | 
| 2 | 2 | 
| 3 | 0 | 
| 4 | 1 | 
| 5 | 4 | 

Bây giờ xây dựng hoán vị theo thứ tự bên trái: 

| Vị Trí Bên Trái | Ray | Đúng Vị Trí | Giá trị phủ định | đuôi | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 3 | -3 | [-3] | 
| 1 | 4 | 1 | -1 | [-3, -1] | 
| 2 | 5 | 4 | -4 | [-4, -1] | 
| 3 | 2 | 2 | -2 | [-4, -2] | 
| 4 | 3 | 0 | 0 | [-4, -2, 0] | 

Câu trả lời cuối cùng:`3`. 

Dãy con giảm tương ứng ở vị trí ban đầu là:```
4, 2, 0
```Những tia đó cắt nhau theo cặp. 

### Ví dụ 2 

đầu vào:```
5
1 2 3 4 5
5 4 3 2 1
```Các vị trí bên phải: 

| Ray | Đúng Vị Trí | 
| --- | --- | 
| 1 | 4 | 
| 2 | 3 | 
| 3 | 2 | 
| 4 | 1 | 
| 5 | 0 | 

Hoán vị:```
4 3 2 1 0
```Dấu vết: 

| Bước | Giá trị | Phủ định | đuôi | 
| --- | --- | --- | --- | 
| 1 | 4 | -4 | [-4] | 
| 2 | 3 | -3 | [-4, -3] | 
| 3 | 2 | -2 | [-4, -3, -2] | 
| 4 | 1 | -1 | [-4, -3, -2, -1] | 
| 5 | 0 | 0 | [-4, -3, -2, -1, 0] | 

Câu trả lời cuối cùng:`5`. 

Điều này chứng tỏ trường hợp cực đoan khi mọi cặp giao nhau, tạo ra hoán vị giảm hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi trong số`n`các phần tử thực hiện một tìm kiếm nhị phân trong`tails`| 
| Không gian | O(n) | Mảng vị trí và trạng thái LIS | 

Với`n = 10^6`, thuật toán bậc hai là không thể.`O(n log n)`đủ nhanh trong Python khi được triển khai cẩn thận với mảng và xử lý lặp. Việc sử dụng bộ nhớ cũng thoải mái nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from bisect import bisect_left

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        n = int(input())

        left = list(map(int, input().split()))
        right = list(map(int, input().split()))

        pos_right = [0] * (n + 1)

        for i, x in enumerate(right):
            pos_right[x] = i

        tails = []

        for x in left:
            v = -pos_right[x]

            idx = bisect_left(tails, v)

            if idx == len(tails):
                tails.append(v)
            else:
                tails[idx] = v

        return str(len(tails))

    return solve() + "\n"

# provided sample
assert run(
"""5
1 4 5 2 3
3 4 2 1 5
""") == "3\n", "sample 1"

# minimum size
assert run(
"""1
1
1
""") == "1\n", "single ray"

# completely reversed
assert run(
"""5
1 2 3 4 5
5 4 3 2 1
""") == "5\n", "all pairs intersect"

# already aligned
assert run(
"""5
1 2 3 4 5
1 2 3 4 5
""") == "1\n", "no intersections"

# mixed ordering
assert run(
"""6
1 2 3 4 5 6
2 1 4 3 6 5
""") == "2\n", "multiple small inversions"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tia đơn | 1 | Đầu vào hợp lệ nhỏ nhất | 
| Hoán vị đảo ngược hoàn toàn | 5 | Mỗi cặp giao nhau | 
| Hoán vị giống hệt nhau | 1 | Không có cặp giao nhau | 
| Hoán đổi xen kẽ | 2 | Xử lý đúng các nghịch đảo cục bộ | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1
1
1
```Vị trí bên phải trở thành:```
pos_right[1] = 0
```Trình tự được tạo ra là:```
[0]
```LIS trên các giá trị phủ định có độ dài`1`, do đó kết quả đầu ra của thuật toán`1`. Một tia luôn tạo thành một nhóm hợp lệ vì không có cặp xung đột nào cần kiểm tra. 

Bây giờ xét trường hợp không có tia nào giao nhau:```
5
1 2 3 4 5
1 2 3 4 5
```Hoán vị được tạo ra là:```
0 1 2 3 4
```Dãy này không có dãy con giảm nào dài hơn`1`. Trong quá trình LIS trên các giá trị phủ định:```
0 -> [0]
-1 replaces 0
-2 replaces -1
-3 replaces -2
-4 replaces -3
```các`tails`mảng không bao giờ phát triển quá chiều dài`1`, vậy đáp án đúng`1`. 

Cuối cùng, hãy xem xét cấu trúc giao nhau tối đa:```
5
1 2 3 4 5
5 4 3 2 1
```Hoán vị trở thành:```
4 3 2 1 0
```Mỗi cặp tạo thành một sự đảo ngược nên mọi cặp tia đều giao nhau. Độ dài dãy con giảm dần là`5`và thuật toán mở rộng`tails`ở mỗi bước, trả về chính xác kích thước đầy đủ của tập hợp.
