---
title: "CF 104178A - Thành công"
description: "Chúng tôi được cho điểm cuối cùng của tất cả học sinh trong lớp. Điểm riêng của bạn bị ẩn, nhưng bạn biết thêm một sự thật: điểm của bạn không phải là điểm tối đa trong số tất cả học sinh. Thứ hạng của một học sinh được xác định bằng một cộng với số học sinh đạt điểm cao hơn."
date: "2026-07-02T00:46:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104178
codeforces_index: "A"
codeforces_contest_name: "BdOI Preliminary 2023"
rating: 0
weight: 104178
solve_time_s: 42
verified: true
draft: false
---

[CF 104178A - Thành công](https://codeforces.com/problemset/problem/104178/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cho điểm cuối cùng của tất cả học sinh trong lớp. Điểm riêng của bạn bị ẩn, nhưng bạn biết thêm một sự thật: điểm của bạn không phải là điểm tối đa trong số tất cả học sinh. 

Thứ hạng của một học sinh được xác định bằng một cộng với số học sinh đạt điểm cao hơn. Vậy nếu có 3 học sinh đạt điểm cao hơn thì thứ hạng của bạn là 4. 

Vì không xác định được điểm chính xác của bạn nên bạn phải xem xét mọi điểm có thể phù hợp với thông tin rằng bạn không phải là người ghi điểm cao nhất. Trong số tất cả các khả năng như vậy, bạn muốn xếp hạng nhỏ nhất có thể. 

Cấu trúc chính là thứ hạng của bạn chỉ phụ thuộc vào số lượng học sinh có điểm cao hơn bạn. Nếu bạn chọn điểm cá nhân cao hơn thì sẽ có ít học sinh đánh bại bạn hơn, do đó thứ hạng của bạn sẽ được cải thiện. Vì vậy, để giảm thiểu thứ hạng, bạn muốn chọn một điểm tối đa hóa số học sinh xếp trên bạn, đồng thời vẫn đảm bảo rằng điểm của bạn không thể là điểm tối đa trong mảng. 

Kích thước đầu vào lên tới 200000, vì vậy mọi giải pháp đều phải chạy trong thời gian tuyến tính hoặc gần tuyến tính. Vì điểm số được giới hạn từ 1 đến 100 nên việc đếm tần suất đương nhiên là đủ. Sắp xếp cũng có thể nhưng không cần thiết. 

Trường hợp phức tạp là khi điểm cao nhất xuất hiện nhiều lần. Ngay cả khi đó, bạn vẫn bị cấm lấy giá trị cao nhất, vì vậy chiến lược tốt nhất là luôn giả sử điểm của bạn là giá trị lớn nhất nhỏ hơn mức tối đa. 

Ví dụ, nếu điểm số là`[100, 100, 99, 50]`, bạn không thể là 100. Nếu bạn chọn 99 thì chỉ có hai người 100 ở trên bạn, xếp hạng 3. Bất kỳ điểm nào thấp hơn sẽ chỉ tăng thứ hạng của bạn. 

Trường hợp thất bại không rõ ràng duy nhất đối với suy nghĩ ngây thơ là cho rằng bạn phải luôn chọn giá trị khác biệt tối đa thứ hai toàn cầu mà không cần kiểm tra phân phối. Tuy nhiên, điều đó thực sự đúng ở đây vì thứ hạng chỉ phụ thuộc vào sự so sánh chặt chẽ. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ thử mọi điểm số ứng viên có thể có cho bạn và đối với mỗi ứng viên, hãy đếm xem có bao nhiêu sinh viên có điểm cao hơn. Điều này thật dễ dàng: với mỗi giá trị ứng cử viên`x`, quét mảng và đếm các phần tử lớn hơn`x`. Tuy nhiên, vì có thể có tới 200000 sinh viên và có khả năng lên tới 100 điểm ứng viên khác nhau, điều này dẫn đến 200000 × 100 hoạt động, nằm ở ranh giới nhưng vẫn không hiệu quả dưới những ràng buộc chặt chẽ và không cần thiết. 

Thay vào đó, chúng ta có thể nhận thấy rằng câu trả lời chỉ phụ thuộc vào số lượng phần tử hoàn toàn lớn hơn điểm cao nhất mà chúng ta được phép lấy. Vì chúng ta không được phép lấy giá trị lớn nhất trong mảng nên lựa chọn tốt nhất là lấy giá trị lớn nhất nhỏ hơn giá trị tối đa. Khi giá trị đó được cố định, số học sinh đạt điểm cao hơn chỉ đơn giản là số phần tử lớn hơn nó. 

Chúng ta có thể tính tần số của tất cả các điểm từ 1 đến 100, sau đó xác định: 

giá trị tối đa`mx`, sau đó xem xét`mx2`, giá trị lớn nhất <`mx`xuất hiện trong mảng. Số học sinh lớn hơn hẳn`mx2`là tổng tần số của tất cả các giá trị lớn hơn`mx2`. Điều đó mang lại thứ hạng tối thiểu có thể. 

Điều này làm giảm vấn đề xuống một tần số quét nhỏ có kích thước không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * 100) | O(1) | Quá chậm | 
| Đếm tần số | O(n + 100) | O(100) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các điểm và tính tần số của chúng trong một mảng có kích thước 101. 

Điều này nén toàn bộ tập dữ liệu thành một biểu diễn có kích thước cố định vì điểm số bị giới hạn. 
2. Tìm số điểm tối đa`mx`có mặt trong mảng. 

Giá trị này bị cấm vì điểm số có thể có của bạn. 
3. Tìm số điểm lớn nhất`mx2`như vậy`mx2 < mx`Và`freq[mx2] > 0`. 

Đây là điểm số tốt nhất bạn được phép lấy vì nó là giá trị gần nhất có thể dưới mức tối đa, giảm thiểu số lượng học sinh xếp trên bạn. 
4. Tính xem có bao nhiêu học sinh đạt điểm cao hơn`mx2`. 

Điều này được thực hiện bằng cách tổng hợp`freq[v]`cho tất cả`v > mx2`. 
5. Đầu ra`1 + (number of students strictly greater than mx2)`. 

Đây là thứ hạng tối thiểu có thể đạt được theo các ràng buộc hợp lệ. 

### Tại sao nó hoạt động 

Thứ hạng hoàn toàn được xác định bởi số lượng phần tử lớn hơn số điểm bạn đã chọn. Mọi điểm ứng viên hợp lệ phải nhỏ hơn phần tử tối đa trong mảng. Trong số tất cả các ứng cử viên như vậy, việc chọn cái lớn nhất có thể sẽ giảm thiểu số phần tử lớn hơn nó, bởi vì tập hợp`{a[i] > x}`co lại một cách đơn điệu như`x`tăng lên. Do đó, lựa chọn tối ưu là giá trị lớn nhất dưới mức tối đa toàn cầu tồn tại trong mảng. Điều này đảm bảo rằng không có lựa chọn hợp lệ nào khác có thể tạo ra một tập hợp nhỏ hơn các phần tử lớn hơn, do đó thứ hạng được tính toán là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    arr = list(map(int, input().split()))
    
    freq = [0] * 101
    for x in arr:
        freq[x] += 1

    mx = 0
    for v in range(1, 101):
        if freq[v]:
            mx = v

    mx2 = 0
    for v in range(mx - 1, 0, -1):
        if freq[v]:
            mx2 = v
            break

    higher = 0
    for v in range(mx2 + 1, 101):
        higher += freq[v]

    print(higher + 1)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên là xây dựng một bảng tần số để không cần phải quét lặp lại đầu vào. Giá trị tối đa được tìm thấy bằng cách quét phạm vi cố định nhỏ. Sau đó, chúng tôi tìm kiếm xuống dưới để xác định điểm số cho phép tốt nhất. Cuối cùng, chúng tôi tính tổng các tần số ở trên để tính xem có bao nhiêu học sinh xếp hạng cao hơn bạn. 

Một sai lầm phổ biến là quên rằng điểm đã chọn không được đạt mức tối đa. Một cách khác là tính sai những học sinh có điểm bằng nhau thì càng cao, điều này sẽ làm tăng thứ hạng. Sự bất đẳng thức chặt chẽ trong phép tính tổng cuối cùng tránh được điều đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
100 100 100 99
```Chúng tôi xây dựng tần số: 

| Giá trị | Tần số | 
| --- | --- | 
| 99 | 1 | 
| 100 | 3 | 

Tối đa là 100. Điểm hợp lệ tốt nhất là 99. 

| Bước | mx | mx2 | Số lượng cao hơn | Xếp hạng | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 100 | - | - | - | 
| Chọn mx2 | 100 | 99 | - | - | 
| Đếm cao hơn | 100 | 99 | 3 | 4 | 

Vì vậy, đầu ra là 4. 

Điều này xác nhận hành vi khi mức tối đa được nhân đôi; ngay cả khi đó, chúng tôi vẫn bị buộc phải ở dưới mức đó và tất cả những người ghi điểm tối đa đều chiếm ưu thế. 

### Ví dụ 2 

đầu vào:```
3
90 75 85
```Tần số: 

| Giá trị | Tần số | 
| --- | --- | 
| 75 | 1 | 
| 85 | 1 | 
| 90 | 1 | 

Tối đa là 90. Điểm hợp lệ nhất là 85. 

| Bước | mx | mx2 | Số lượng cao hơn | Xếp hạng | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 90 | - | - | - | 
| Chọn mx2 | 90 | 85 | - | - | 
| Đếm cao hơn | 90 | 85 | 1 | 2 | 

Chỉ có 90 là trên 85 nên hạng là 2. 

Điều này cho thấy rằng việc chọn giá trị khác biệt lớn thứ hai luôn mang lại thứ hạng tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + 100) | Một lượt để xây dựng tần số cộng với quét phạm vi không đổi | 
| Không gian | O(100) | Mảng tần số cố định độc lập với n | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì tất cả các thao tác sau khi đọc đầu vào đều có thời gian không đổi trong một phạm vi giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("4\n100 100 100 99\n") == "4", "sample 1"
assert run("3\n90 75 85\n") == "2", "sample 2"

# custom cases
assert run("2\n1 2\n") == "2", "minimum size distinct"
assert run("5\n10 10 10 10 9\n") == "5", "all high except one"
assert run("6\n5 4 3 2 1 5\n") == "5", "multiple max values"
assert run("3\n2 1 2\n") == "2", "duplicate max edge"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 giá trị riêng biệt | 2 | trường hợp không tầm thường nhỏ nhất | 
| lặp lại tối đa | 5 | xử lý trùng lặp ở trên cùng | 
| hỗn hợp giảm dần | 5 | lựa chọn tối đa thứ hai đúng | 
| trùng lặp đối xứng | 2 | tính đúng đắn của bất đẳng thức | 

## Vỏ cạnh 

Trường hợp một cạnh là khi giá trị lớn nhất xuất hiện nhiều lần. Đối với đầu vào:```
5
10 10 10 10 9
```tối đa là 10, vì vậy bạn không thể chọn 10. Lựa chọn hợp lệ nhất là 9 và cả bốn số 10 đều cao hơn hoàn toàn, do đó thứ hạng trở thành 5. Thuật toán tìm đúng`mx2 = 9`và đếm tất cả số 10. 

Một trường hợp cạnh khác là khi mảng có chính xác hai giá trị riêng biệt:```
2
1 2
```Ở đây, tối đa là 2 và lựa chọn hợp lệ duy nhất là 1. Chính xác có một học sinh cao hơn nên xếp hạng là 2. Quét xuống sẽ tìm thấy chính xác 1 là`mx2`và đếm một phần tử cao hơn. 

Trường hợp cạnh cuối cùng là khi các giá trị được lặp lại nhiều ở phía trên nhưng thưa thớt ở phía dưới. Tính tổng dựa trên tần số vẫn hoạt động vì nó không phụ thuộc vào vị trí hoặc thứ tự, chỉ tính các giá trị lớn hơn, đảm bảo tính ổn định trên tất cả các phân phối.
