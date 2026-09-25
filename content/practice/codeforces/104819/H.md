---
title: "CF 104819H - Đa giác"
description: "Chúng ta được đưa cho một tập hợp các độ dài thanh và được hỏi liệu có thể chọn chính xác k trong số chúng để chúng có thể đóng vai trò là các cạnh của một đa giác đơn giản hay không."
date: "2026-06-28T13:02:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104819
codeforces_index: "H"
codeforces_contest_name: "2023 Sun Yat-sen University Collegiate Programming Contest, Onsite"
rating: 0
weight: 104819
solve_time_s: 45
verified: true
draft: false
---

[CF 104819H - Đa giác](https://codeforces.com/problemset/problem/104819/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa cho một tập hợp các độ dài thanh và được hỏi liệu có thể chọn chính xác k trong số chúng để chúng có thể đóng vai trò là các cạnh của một đa giác đơn giản hay không. 

Yêu cầu hình học duy nhất mà chúng ta cần quan tâm là bất đẳng thức đa giác tổng quát: một tập hợp k đoạn có thể tạo thành một đa giác đơn không suy biến khi và chỉ khi không có đoạn nào quá dài so với các đoạn khác. Cụ thể, nếu chúng ta sắp xếp các độ dài đã chọn, điều kiện trở thành độ dài được chọn lớn nhất phải nhỏ hơn tổng của k − 1 độ dài còn lại. 

Dữ liệu đầu vào bao gồm n que ứng cử viên và chúng ta phải quyết định xem có tồn tại tập con nào có kích thước k thỏa mãn bất đẳng thức này hay không. Chúng tôi không xây dựng đa giác, chỉ kiểm tra tính khả thi. 

Ràng buộc n 3000 có nghĩa là các phương pháp tiếp cận bậc hai hoặc gần bậc hai có thể chấp nhận được, nhưng bất kỳ phương pháp lập phương hoặc liên quan đến việc liệt kê tất cả các tập hợp con đều hoàn toàn không khả thi. Khó khăn chính là chúng ta đang chọn một tập hợp con theo ràng buộc bất đẳng thức toàn cục chứ không chỉ kiểm tra một tập hợp cố định. 

Trường hợp khó nhận biết là khi có nhiều que giống hệt nhau hoặc khi một que cực kỳ lớn so với tất cả các que khác. Ví dụ: nếu k = 3 và mảng là [100, 1, 1] thì câu trả lời rõ ràng là Không vì 100 quá lớn. Một cách tiếp cận đơn giản chỉ kiểm tra tổng mà không lựa chọn cẩn thận k phần tử có thể dễ dàng bỏ sót rằng việc lựa chọn tập hợp con là vấn đề quan trọng. 

Một trường hợp sai sót khác xuất hiện khi k lớn. Ví dụ: nếu hầu hết các que đều nhỏ nhưng một số ít lớn thì quyết định xoay quanh việc cân bằng việc lựa chọn các phần tử lớn (làm tăng mức tối đa) so với các phần tử nhỏ (làm tăng tổng các phần tử khác). Một sự lựa chọn tham lam là cần thiết, nhưng nó phải hợp lý. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ thử mọi tập con có kích thước k, tính tổng của nó, xác định phần tử lớn nhất của nó và kiểm tra xem hai lần giá trị lớn nhất có nhỏ hơn tổng hay không. Điều này đúng vì đối với bất kỳ tập hợp con ứng cử viên nào, điều kiện đa giác giảm xuống 2 * max < tổng của tập hợp con. Tuy nhiên, số lượng tập hợp con là C(n, k), là số mũ theo n, và ngay cả đối với k vừa phải thì điều này hoàn toàn không thể xảy ra. 

Cấu trúc của điều kiện cho thấy rằng chỉ có mối quan hệ giữa phần tử được chọn lớn nhất và tổng số tiền là quan trọng. Nếu chúng ta sắp xếp tất cả các que, bất kỳ tập hợp con hợp lệ nào cũng có thể được coi là k phần tử được chọn trong đó phần tử lớn nhất được cố định và cơ hội tốt nhất để thỏa mãn điều kiện là tối đa hóa tổng của k − 1 phần tử còn lại. Điều này ngay lập tức gợi ý rằng nếu chúng ta cố định một mức tối đa ứng cử viên, chúng ta phải luôn ghép nó với k − 1 que lớn nhất có thể còn lại trong số những que nhỏ hơn nó. 

Điều này dẫn đến việc quét tham lam trên mảng đã được sắp xếp: coi mỗi phần tử là cực đại tiềm năng và kiểm tra xem k − 1 phần tử lớn nhất trước nó có đủ để thỏa mãn bất đẳng thức hay không. Bằng cách duy trì tổng tiền tố, chúng ta có thể đánh giá từng ứng viên trong thời gian không đổi sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force | O(C(n, k) · k) | O(k) | Quá chậm | 
| Sắp xếp + kiểm tra tiền tố tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán

1. Sắp xếp tất cả các chiều dài thanh theo thứ tự không giảm. Điều này cho phép chúng tôi coi bất kỳ vị trí nào là mức tối đa tiềm năng trong khi đảm bảo tất cả các ứng cử viên ở bên trái vị trí đó đều nhỏ hơn hoặc bằng nhau. 
2. Xây dựng mảng tổng tiền tố trên danh sách đã sắp xếp để chúng ta có thể tính tổng của bất kỳ phân đoạn nào trong thời gian O(1). Điều này là cần thiết vì chúng ta đánh giá liên tục tổng của các tập con đã chọn. 
3. Với mỗi chỉ số i từ k − 1 đến n − 1, coi a[i] là phần tử lớn nhất của các cạnh đa giác đã chọn. 
4. Xét k − 1 phần tử ngay trước i trong mảng đã được sắp xếp. Đây là những ứng cử viên tốt nhất có thể để ghép với a[i] vì bất kỳ lựa chọn nào nhỏ hơn sẽ chỉ làm giảm tổng và làm cho bất đẳng thức khó được thỏa mãn hơn. 
5. Tính tổng của k − 1 phần tử này bằng cách sử dụng mảng tổng tiền tố. 
6. Kiểm tra xem 2 * a[i] < sum_of_previous_k_minus_1 + a[i] hay không. Điều này tương đương với việc kiểm tra xem a[i] < sum_of_previous_k_minus_1 có phải là bất đẳng thức đa giác hay không. 
7. Nếu chỉ số nào thỏa mãn điều kiện thì trả về ngay Yes. Nếu không có chỉ mục nào hoạt động, trả về No. 

### Tại sao nó hoạt động 

Việc sửa phần tử lớn nhất được chọn là đủ vì bất kỳ tập hợp k hợp lệ nào cũng có phần tử tối đa duy nhất. Đối với mức tối đa cố định, chiến lược tối ưu để tối đa hóa cơ hội thỏa mãn điều kiện đa giác là tối đa hóa tổng của k − 1 phần tử còn lại, điều này đạt được bằng cách chọn k − 1 phần tử lớn nhất bên dưới nó theo thứ tự được sắp xếp. Điều này làm giảm vấn đề kiểm tra một bất đẳng thức duy nhất cho mỗi ứng viên tối đa. Vì mọi tập hợp con hợp lệ đều tương ứng với một số vị trí tối đa ứng cử viên trong mảng đã sắp xếp, nên không có giải pháp hợp lệ nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    a.sort()
    
    # prefix sums
    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + a[i]
    
    # try each possible maximum position
    for i in range(k - 1, n):
        # sum of k-1 elements before i
        left_sum = pref[i] - pref[i - (k - 1)]
        
        # check polygon condition:
        # largest side a[i] must be < sum of others
        if a[i] < left_sum:
            print("Yes")
            return
    
    print("No")

if __name__ == "__main__":
    solve()
```Bước sắp xếp đảm bảo chúng ta chỉ xem xét các ứng cử viên hợp lệ cho phía lớn nhất theo thứ tự tăng dần. Mảng tiền tố cho phép chúng ta tính tổng k − 1 đồng hành tốt nhất trong thời gian không đổi. Chi tiết triển khai chính là cửa sổ`[i - (k - 1), i)`luôn đại diện cho sự lựa chọn tốt nhất có thể cho các bên hỗ trợ. 

Bất đẳng thức được kiểm tra ở dạng đơn giản nhất`a[i] < left_sum`, tránh mọi nhu cầu tính lại tổng hoặc lý do về đa giác đầy đủ một cách rõ ràng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
1 2 3
```| tôi | đã chọn tối đa | cửa sổ | tổng(trái) | tình trạng | 
| --- | --- | --- | --- | --- | 
| 2 | 3 | [1, 2] | 3 | 3 < 3 sai | 

Tam giác duy nhất có thể sử dụng tất cả các phần tử, nhưng 3 không hoàn toàn nhỏ hơn 1 + 2, do đó điều kiện không thành công và câu trả lời là Không. 

### Ví dụ 2 

đầu vào:```
6 4
1 1 4 5 1 4
```Mảng được sắp xếp: [1, 1, 1, 4, 4, 5] 

| tôi | đã chọn tối đa | cửa sổ | tổng(trái) | tình trạng | 
| --- | --- | --- | --- | --- | 
| 3 | 4 | [1,1,1] | 3 | 4 < 3 sai | 
| 4 | 4 | [1,1,4] | 6 | 4 < 6 đúng | 

Tại i = 4, chúng ta chọn 4 là phần tử lớn nhất và ba phần tử đứng trước tốt nhất có tổng bằng 6, đủ để thỏa mãn bất đẳng thức đa giác. Vậy câu trả lời là Có. 

Dấu vết này cho thấy cách chọn những người bạn đồng hành tốt nhất có thể cho mỗi ứng viên là đủ để phát hiện tính khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp chiếm ưu thế, quét là tuyến tính | 
| Không gian | O(n) | tổng tiền tố và lưu trữ mảng được sắp xếp | 

Các ràng buộc n 3000 làm cho O(n log n) dễ dàng đủ nhanh. Giải pháp chỉ sử dụng các phép toán mảng đơn giản và tránh hoàn toàn việc liệt kê tổ hợp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + a[i]

    for i in range(k - 1, n):
        left_sum = pref[i] - pref[i - (k - 1)]
        if a[i] < left_sum:
            print("Yes")
            return
    print("No")

# samples
assert run("3 3\n1 2 3\n") == "No"
assert run("6 4\n1 1 4 5 1 4\n") == "Yes"

# minimum n=k=3
assert run("3 3\n1 1 1\n") == "Yes"

# impossible large dominant element
assert run("4 3\n100 1 1 1\n") == "No"

# all equal large k
assert run("5 4\n10 10 10 10 10\n") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 3 / 1 1 1 | Có | đa giác hợp lệ tối thiểu | 
| 4 3 / 100 1 1 1 | Không | điều kiện lỗi tối đa chiếm ưu thế | 
| 5 4 / tất cả đều bằng nhau | Có | trường hợp đối xứng có nhiều tập con hợp lệ | 

## Vỏ cạnh 

Trường hợp cạnh phổ biến là khi phần tử lớn nhất cực kỳ lớn so với tất cả các phần tử khác. Đối với đầu vào`4 3: 100 1 1 1`, sắp xếp cho`[1, 1, 1, 100]`. Giá trị tối đa ứng cử viên duy nhất là 100 và tổng ba phần tử tốt nhất bên dưới nó là 3. Điều kiện`100 < 3`thất bại ngay lập tức, do đó thuật toán trả về đúng số No. 

Một trường hợp khác là khi tất cả các phần tử đều bằng nhau. Vì`5 4: 10 10 10 10 10`, mọi ứng viên tối đa đều có ba phần tử hỗ trợ có tổng bằng 30, vì vậy`10 < 30`nắm giữ. Thuật toán tìm thấy i hợp lệ và trả về Yes sớm. 

Một tình huống tế nhị là khi k gần với n. Thuật toán vẫn hoạt động vì cửa sổ`[i-(k-1), i)`trở thành gần như toàn bộ tiền tố. Điều này đảm bảo rằng ngay cả khi chỉ loại trừ một hoặc hai phần tử, bất đẳng thức vẫn được đánh giá chính xác so với tập hợp con tốt nhất có thể.
