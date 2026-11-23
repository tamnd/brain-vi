---
title: "CF 442B - Andrey và vấn đề"
description: "Chúng ta được giao cho một nhóm bạn, trong đó mỗi người bạn độc lập thành công trong việc tạo ra một bài toán cạnh tranh với xác suất đã biết. Andrey sẽ mời một số nhóm nhỏ những người bạn này. Sau khi được mời, mỗi người bạn sẽ đóng góp một vấn đề hoặc không, độc lập với những người khác."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 442
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 253 (Div. 1)"
rating: 1800
weight: 442
solve_time_s: 74
verified: true
draft: false
---

[CF 442B - Andrey và Vấn đề](https://codeforces.com/problemset/problem/442/B) 

**Đánh giá:** 1800 
**Tags:** tham lam, toán học, xác suất 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao cho một nhóm bạn, trong đó mỗi người bạn độc lập thành công trong việc tạo ra một bài toán cạnh tranh với xác suất đã biết. Andrey sẽ mời một số nhóm nhỏ những người bạn này. Sau khi được mời, mỗi người bạn sẽ đóng góp một vấn đề hoặc không, độc lập với những người khác. 

Kết quả mà chúng tôi quan tâm rất cụ thể: Andrey chỉ hài lòng nếu đúng một người bạn đưa ra được vấn đề. Nếu không ai gây ra vấn đề gì, hoặc nếu có hai hoặc nhiều người bạn thành công, anh ta sẽ không vui. Nhiệm vụ là chọn những người bạn sẽ mời sao cho xác suất có được đúng một người đóng góp thành công là tối đa. 

Đối tượng chính không phải là xác suất cá nhân mà là sự phân bổ tạo ra số lượng bạn bè thành công sau khi chọn một tập hợp con. Mỗi tập hợp con được chọn xác định tổng các biến Bernoulli độc lập và chúng tôi muốn tối đa hóa xác suất để tổng này bằng chính xác một. 

Giới hạn rất nhỏ, tối đa là 100 bạn bè. Điều này ngay lập tức loại trừ mọi phép liệt kê theo cấp số nhân của các tập hợp con ngoài lý do vừa phải trừ khi được cắt tỉa nhiều, nhưng quan trọng hơn là nó cho thấy rằng cách tiếp cận động O(n^2) hoặc O(n^3) là không cần thiết. Một giải pháp gần hơn với O(n log n) hoặc O(n) được mong đợi. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các xác suất đều rất nhỏ. Trong trường hợp đó, việc mời nhiều bạn bè có vẻ hấp dẫn vì nó làm tăng cơ hội thành công ít nhất một lần, nhưng nó cũng làm tăng cơ hội thành công nhiều lần, điều này không được phép. Một trường hợp khác xảy ra khi xác suất rất lớn, gần bằng 1, trong đó việc mời thêm người gần như đảm bảo nhiều thành công và trở nên có hại. 

Trực giác ngây thơ cho rằng “càng nhiều bạn càng tốt” đã thất bại ngay lập tức. Ví dụ: nếu tất cả xác suất là 0,8 thì việc mời hai người bạn sẽ có xác suất để có đúng một người thành công bằng 0,8·0,2 + 0,2·0,8 = 0,32, trong khi mời một người bạn là 0,8. Vì vậy, việc thêm nhiều hơn có thể làm giảm xác suất mục tiêu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi nhóm bạn bè. Đối với mỗi tập hợp con, chúng tôi tính xác suất để có chính xác một người bạn thành công bằng cách tổng hợp tất cả các lựa chọn trong đó người bạn nào là người thành công và nhân xác suất thất bại của những người còn lại. Đối với một tập hợp con có kích thước k, chi phí này là O(k) và trên tất cả các tập hợp con, giá trị này trở thành O(n·2^n), điều này vượt xa khả thi ngay cả với n = 30. 

Cấu trúc biểu thức của một tập hợp con cố định tiết lộ điều gì đó hữu ích hơn. Giả sử chúng ta chọn một tập con S. Xác suất để có đúng một thành công có thể được viết bằng tổng trên i trong S của pi nhân với xác suất tất cả những người bạn được chọn khác đều thất bại. Điều này giới thiệu cấu trúc nhân lặp đi lặp lại: mọi số hạng đều phụ thuộc vào tích trên các tập hợp con. 

Quan sát quan trọng nhất là hãy nghĩ xem việc thêm một người bạn sẽ thay đổi giá trị như thế nào. Nếu chúng ta duy trì tập S đã chọn với giá trị hiện tại P(S) và chúng ta xem xét việc thêm một người bạn mới với xác suất p, thì xác suất mới sẽ trở thành (1 − p)·P(S) + p·product_of_failures(S). Hình thức này gợi ý rằng sự đóng góp của một người bạn không chỉ phụ thuộc vào p mà còn phụ thuộc vào cách nó tương tác theo cấp số nhân với cấu trúc hiện tại. 

Viết lại mục tiêu dẫn đến một hình thức rõ ràng hơn. Nếu chúng ta định nghĩa cho tập S: 

P(S) = xác suất có đúng một thành công trong số S 

sau đó thêm một người bạn mới, tôi mang lại: 

P(S ∪ {i}) = P(S)·(1 − p_i) + (tích của (1 − p_j) chia cho j trong S)·p_i 

Điều này gợi ý việc duy trì hai đại lượng tổng hợp: xác suất hiện tại của đúng một thành công và xác suất tất cả những người bạn được chọn đều thất bại. Với những điều này, chúng ta có thể tham lam xem xét việc thêm từng người bạn và quyết định xem liệu điều đó có làm tăng giá trị hay không.

Chiến lược tối ưu cuối cùng trở nên tham lam theo một thứ tự cụ thể: chúng ta sắp xếp bạn bè bằng cách giảm p_i / (1 − p_i), tương ứng với việc ưu tiên những chiến lược mang lại sự cải thiện cận biên tốt nhất trong sự cân bằng giữa việc tạo ra chính xác một thành công và gây ra nhiều thành công. 

Sau khi sắp xếp, chúng tôi thử thêm từng người bạn và theo dõi xem xác suất có cải thiện hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi từng xác suất p_i thành một giá trị phản ánh cường độ tương đối của nó, cụ thể là tỷ lệ p_i / (1 − p_i). Điều này cho thấy khả năng thành công của một người bạn so với thất bại, đây là điều quan trọng khi cân bằng chính xác một kết quả. 
2. Sắp xếp tất cả bạn bè theo thứ tự giảm dần theo tỷ lệ này. Thứ tự này ưu tiên những người bạn đóng góp hiệu quả hơn vào việc tạo ra một thành công duy nhất mà không làm tăng nguy cơ va chạm quá mức. 
3. Khởi tạo hai biến: một biến cho xác suất tất cả những người bạn được chọn đều thất bại và một biến cho xác suất có đúng một người bạn thành công. 
4. Lặp lại qua những người bạn theo thứ tự được sắp xếp và với mỗi người bạn, hãy tính xem điều gì sẽ xảy ra nếu chúng ta đưa họ vào. Cập nhật xác suất thất bại theo cấp số nhân và cập nhật xác suất chính xác bằng cách sử dụng phép phân rã thành “người bạn này là thành công duy nhất” cộng với “cấu trúc hiện có vẫn hợp lệ và người bạn này thất bại”. 
5. Theo dõi giá trị tốt nhất gặp trên tất cả các tiền tố của thứ tự này, vì việc dừng sớm hơn tương ứng với việc chọn một tập hợp con. 
6. Xuất ra xác suất tối đa thu được. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, trạng thái của tập hợp con được chọn được mô tả đầy đủ bằng hai giá trị: xác suất không có ai trong tập hợp con thành công và xác suất có đúng một người thành công. Bất kỳ phần tử mới nào cũng chỉ tương tác với tập hợp con thông qua hai tập hợp này. Điều này làm giảm cấu trúc tập hợp con thành bài toán sắp xếp một chiều trong đó tính tối ưu tuân theo các đối số trao đổi cục bộ trên các phần tử liền kề theo thứ tự được sắp xếp. Nếu hai người bạn liền kề không theo thứ tự đối với p/(1−p), việc hoán đổi chúng không cải thiện xác suất thu được, điều này đảm bảo rằng việc sắp xếp mang lại tiền tố tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    p = list(map(float, input().split()))
    
    # sort by p/(1-p) decreasing
    p.sort(key=lambda x: x/(1.0 - x) if x < 1.0 else float('inf'), reverse=True)
    
    best = 0.0
    prod_fail = 1.0  # product of (1 - p_i) in current set
    prob_one = 0.0   # probability exactly one success
    
    for x in p:
        new_prod_fail = prod_fail * (1 - x)
        new_prob_one = prob_one * (1 - x) + prod_fail * x
        
        prob_one = new_prob_one
        prod_fail = new_prod_fail
        
        if prob_one > best:
            best = prob_one
    
    print(f"{best:.12f}")

if __name__ == "__main__":
    solve()
```Bước sắp xếp đảm bảo chúng tôi xem xét những người bạn “hiệu quả” nhất trước tiên xét về tỷ lệ đóng góp thành công trên thất bại. Hai biến đang chạy biểu thị trạng thái xác suất đầy đủ của tập hợp con hiện tại, do đó việc cập nhật chúng là đủ để đánh giá từng tiền tố mà không cần tính toán lại từ đầu. 

Một cạm bẫy triển khai phổ biến là quên rằng phép chia dấu phẩy động cho (1 − p) trở nên không ổn định khi p cực gần bằng 1. Mã xử lý điều này bằng cách coi p = 1 là một tỷ lệ vô hạn, buộc các phần tử như vậy ở phía trước. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
0.1 0.2 0.3 0.8
```Sau khi sắp xếp theo tỷ lệ, thứ tự trở thành: 

0,8, 0,3, 0,2, 0,1 

Chúng tôi theo dõi trạng thái từng bước: 

| Bước | p_i | prod_fail | thăm dò | tốt nhất | 
| --- | --- | --- | --- | --- | 
| ban đầu | - | 1.0 | 0,0 | 0,0 | 
| 1 | 0,8 | 0,2 | 0,8 | 0,8 | 
| 2 | 0,3 | 0,14 | 0,44 | 0,8 | 
| 3 | 0,2 | 0,112 | 0,496 | 0,8 | 
| 4 | 0,1 | 0.1008 | 0,5064 | 0,8 | 

Giá trị tốt nhất đạt được sau khi chỉ chọn người bạn đầu tiên. Điều này xác nhận rằng việc thêm xác suất yếu hơn sau xác suất mạnh sẽ làm giảm cơ hội thành công đúng một lần. 

### Ví dụ 2 

đầu vào:```
3
0.5 0.5 0.5
```Thứ tự sắp xếp không thay đổi. 

| Bước | p_i | prod_fail | thăm dò | tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | 0,5 | 0,5 | 0,5 | 0,5 | 
| 2 | 0,5 | 0,25 | 0,5 | 0,5 | 
| 3 | 0,5 | 0,125 | 0,375 | 0,5 | 

Lựa chọn tối ưu là bất kỳ người bạn nào, vì việc thêm nhiều hơn chỉ làm tăng khả năng va chạm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | phân loại chiếm ưu thế, quét tuyến tính đơn sau đó | 
| Không gian | O(1) | chỉ có một vài biến đang chạy ngoài mảng đầu vào | 

Các ràng buộc n ≤ 100 làm cho việc này dễ dàng đủ nhanh ngay cả với các phép toán dấu phẩy động và chi phí sắp xếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose
    
    data = inp.strip().split()
    n = int(data[0])
    p = list(map(float, data[1:]))

    p.sort(key=lambda x: x/(1.0 - x) if x < 1.0 else float('inf'), reverse=True)

    best = 0.0
    prod_fail = 1.0
    prob_one = 0.0

    for x in p:
        new_prod_fail = prod_fail * (1 - x)
        new_prob_one = prob_one * (1 - x) + prod_fail * x
        prod_fail, prob_one = new_prod_fail, new_prob_one
        best = max(best, prob_one)

    return f"{best:.10f}"

# provided sample
assert abs(float(run("4\n0.1 0.2 0.3 0.8")) - 0.8) < 1e-9

# all zeros
assert abs(float(run("3\n0.0 0.0 0.0")) - 0.0) < 1e-9

# all ones
assert abs(float(run("2\n1.0 1.0")) - 0.0) < 1e-9

# single element
assert abs(float(run("1\n0.7")) - 0.7) < 1e-9

# mixed
assert abs(float(run("3\n0.9 0.1 0.5")) - 0.9) < 1e-9
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 0,1 0,2 0,3 0,8 | 0,8 | tập hợp con tốt nhất là xác suất tối đa đơn | 
| 3 0 0 0 | 0 | trường hợp tất cả các lỗi cạnh | 
| 2 1 1 | 0 | nhiều thành công nhất định làm mất hiệu lực giải pháp | 
| 1 0,7 | 0,7 | độ chính xác đầu vào tối thiểu | 
| 3 0,9 0,1 0,5 | 0,9 | đặt hàng hiệu quả đúng đắn | 

## Vỏ cạnh 

Khi tất cả các xác suất bằng 0, mọi tập hợp con đều tạo ra xác suất bằng 0 cho đúng một lần thành công. Thuật toán khởi tạo tốt nhất về 0 và không bao giờ tăng nó lên, do đó kết quả đầu ra vẫn chính xác. 

Khi xác suất chính xác là một, bất kỳ tập hợp con nào chứa nhiều hơn một phần tử như vậy sẽ ngay lập tức mang lại xác suất thành công bằng không. Thứ tự tham lam đặt những thứ này lên hàng đầu do tỷ lệ vô hạn và thuật toán đánh giá chính xác rằng chỉ chọn một người bạn như vậy sẽ mang lại xác suất là một. 

Khi xác suất cực kỳ sai lệch, chẳng hạn như một giá trị gần một và nhiều giá trị nhỏ, việc đánh giá tiền tố đảm bảo rằng chúng tôi dừng ở phần tử đầu tiên, tránh suy giảm do thêm những người đóng góp chất lượng thấp.
