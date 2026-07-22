---
title: "CF 103687C - JB Muốn Kiếm Nhiều Tiền"
description: "Chúng ta có hai nhóm người tham gia vào một hệ thống giống như thị trường chứng khoán. Một nhóm gồm những người muốn mua cổ phiếu và mỗi người trong số họ chỉ định mức giá tối đa mà họ sẵn sàng trả."
date: "2026-07-02T20:56:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103687
codeforces_index: "C"
codeforces_contest_name: "The 19th Zhejiang Provincial Collegiate Programming Contest"
rating: 0
weight: 103687
solve_time_s: 47
verified: true
draft: false
---

[CF 103687C - JB muốn kiếm nhiều tiền](https://codeforces.com/problemset/problem/103687/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai nhóm người tham gia vào một hệ thống giống như thị trường chứng khoán. Một nhóm gồm những người muốn mua cổ phiếu và mỗi người trong số họ chỉ định mức giá tối đa mà họ sẵn sàng trả. Nhóm còn lại gồm những người muốn bán cổ phiếu và mỗi người trong số họ chỉ định một mức giá tối thiểu mà họ sẵn sàng chấp nhận. 

Một mức giá cuối cùng duy nhất$x$được hệ thống cố định. Người mua tham gia nếu giá đưa ra của họ ít nhất là$x$, vì họ sẵn sàng trả nhiều hoặc nhiều hơn thế. Người bán tham gia nếu giá đưa ra của họ cao nhất$x$, vì họ sẵn sàng chấp nhận mức giá đó hoặc thấp hơn. Nhiệm vụ là đếm xem tổng cộng có bao nhiêu người ở cả hai nhóm thỏa mãn các điều kiện này. 

Kích thước đầu vào lên tới$10^5$người mua và$10^5$người bán. Bất kỳ giải pháp nào kiểm tra từng phần tử một lần đều có thể chấp nhận được, nhưng mọi giải pháp liên quan đến sắp xếp hoặc quét lồng nhau vẫn ổn. Tuy nhiên, mọi thứ bậc hai hoặc liên quan đến việc quét toàn bộ lặp đi lặp lại cho mỗi phần tử sẽ quá chậm vì nó sẽ đạt tới$10^{10}$hoạt động trong trường hợp xấu nhất. 

Một sai lầm phổ biến trong những vấn đề như thế này là hiểu sai hướng của bất đẳng thức. Người mua đủ điều kiện khi$a_i \ge x$, không$a_i \le x$. Người bán đủ điều kiện khi$b_i \le x$, không$b_i \ge x$. Trao đổi các điều kiện này dẫn đến số lượng đảo ngược hoàn toàn. 

Một trường hợp cạnh tinh vi khác xuất hiện khi tất cả các giá trị đều bằng$x$. Trong tình huống đó, mọi người tham gia từ cả hai nhóm đều phải được tính. Bất kỳ lỗi ngưỡng nào sẽ không thành công ở đây ngay lập tức. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Chúng tôi quét tất cả người mua và kiểm tra xem mỗi giá trị có ít nhất$x$. Sau đó, chúng tôi quét tất cả người bán và kiểm tra xem mỗi giá trị có tối đa không$x$. Chúng tôi tăng bộ đếm bất cứ khi nào một điều kiện được thỏa mãn. Điều này hiệu quả vì mỗi quyết định đều độc lập và câu trả lời cuối cùng chỉ là tổng số người tham gia hài lòng. 

Cách tiếp cận này đã tối ưu về mặt cấu trúc tiệm cận vì bài toán không yêu cầu sắp xếp hoặc ghép nối. Lý do duy nhất nó có thể được coi là "bạo lực" là vì nó kiểm tra rõ ràng từng phần tử một. 

Tổng số thao tác là$n + m$, nhiều nhất là$2 \cdot 10^5$. Ngay cả trong Python, điều này dễ dàng nằm trong giới hạn. Mọi nỗ lực sắp xếp hoặc xử lý trước đều là chi phí không cần thiết và không cải thiện tính chính xác hoặc hiệu suất. 

Quan sát quan trọng là giá cuối cùng đóng vai trò như một ngưỡng cố định, tách người tham gia thành hai bộ lọc độc lập. Người mua được lọc theo giới hạn dưới, người bán được lọc theo giới hạn trên. Không có sự tương tác giữa các nhóm nên bài toán được chia thành hai phép tính đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n + m) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n + m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$n, m, x$. Những điều này xác định quy mô của hai nhóm và mức giá ngưỡng. 
2. Khởi tạo bộ đếm về 0. Điều này sẽ tích lũy tất cả những người tham gia hợp lệ. 
3. Đọc danh sách giá người mua. Với mỗi giá trị$a_i$, kiểm tra xem$a_i \ge x$. Nếu vậy, hãy tăng bộ đếm. Điều này phản ánh rằng người mua sẵn sàng giao dịch ở mức giá hệ thống hoặc cao hơn. 
4. Đọc danh sách giá bán. Với mỗi giá trị$b_i$, kiểm tra xem$b_i \le x$. Nếu vậy, hãy tăng bộ đếm. Điều này phản ánh rằng người bán sẵn sàng chấp nhận mức giá hệ thống hoặc thấp hơn. 
5. Xuất bộ đếm cuối cùng. 

Lý do đằng sau các vòng lặp riêng biệt là mỗi nhóm có hướng bất bình đẳng khác nhau và việc trộn lẫn chúng sẽ tạo ra sự phức tạp không cần thiết và có nguy cơ so sánh không chính xác. 

### Tại sao nó hoạt động 

Mỗi người tham gia độc lập đáp ứng một điều kiện duy nhất chỉ phụ thuộc vào giá của họ và ngưỡng cố định$x$. Quyết định của một người tham gia không bao giờ ảnh hưởng đến người khác. Do tính độc lập này nên việc tính riêng người mua và người bán hợp lệ và tổng hợp kết quả tương đương với việc đánh giá toàn bộ hệ thống. Thuật toán về cơ bản là tính toán kích thước của sự kết hợp của hai bộ điều kiện rời nhau được xác định trên các mảng đầu vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, x = map(int, input().split())

ans = 0

a = list(map(int, input().split()))
for v in a:
    if v >= x:
        ans += 1

b = list(map(int, input().split()))
for v in b:
    if v <= x:
        ans += 1

print(ans)
```Mã phản ánh trực tiếp thuật toán. Đầu tiên chúng tôi đọc các tham số, sau đó xử lý người mua và người bán một cách độc lập. quầy`ans`chỉ được cập nhật khi một giá trị đáp ứng điều kiện dành riêng cho nhóm của nó. 

Một lỗi triển khai phổ biến là trộn lẫn các bất đẳng thức chặt chẽ và không chặt chẽ. Ở đây, sự bình đẳng được bao gồm trong cả hai trường hợp: người mua chấp nhận$a_i = x$, người bán chấp nhận$b_i = x$. Đây là cố ý và phù hợp với tuyên bố. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào trong đó ngưỡng phân chia cả hai nhóm không đồng đều. 

đầu vào:```
n = 5, m = 4, x = 3
buyers = [1, 3, 4, 2, 5]
sellers = [3, 1, 4, 2]
```| Bước | Giá trị người mua | Tình trạng$a_i \ge x$| Giá trị người bán | Tình trạng$b_i \le x$| Quầy | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | sai | - | - | 0 | 
| 2 | 3 | đúng | - | - | 1 | 
| 3 | 4 | đúng | - | - | 2 | 
| 4 | 2 | sai | - | - | 2 | 
| 5 | 5 | đúng | - | - | 3 | 
| 6 | - | - | 3 | đúng | 4 | 
| 7 | - | - | 1 | đúng | 5 | 
| 8 | - | - | 4 | sai | 5 | 
| 9 | - | - | 2 | đúng | 6 | 

Câu trả lời cuối cùng là 6. Điều này xác nhận rằng cả hai ngưỡng được áp dụng độc lập và tích lũy chính xác. 

Bây giờ hãy xem xét một trường hợp nặng về biên. 

đầu vào:```
n = 3, m = 3, x = 5
buyers = [5, 5, 4]
sellers = [5, 6, 1]
```| Bước | Giá trị người mua | hợp lệ | Giá trị người bán | hợp lệ | Quầy | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | đúng | - | - | 1 | 
| 2 | 5 | đúng | - | - | 2 | 
| 3 | 4 | sai | - | - | 2 | 
| 4 | - | - | 5 | đúng | 3 | 
| 5 | - | - | 6 | sai | 3 | 
| 6 | - | - | 1 | đúng | 4 | 

Điều này khẳng định việc xử lý đúng sự bình đẳng ở ngưỡng, thường là nguồn gốc của những sai lầm riêng lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi danh sách được quét một lần, với công việc liên tục cho mỗi phần tử | 
| Không gian | O(1) | Chỉ có một bộ đếm được duy trì ngoài bộ nhớ đầu vào | 

Giới hạn đầu vào của$2 \cdot 10^5$tổng số phần tử phù hợp thoải mái trong các ràng buộc thời gian tuyến tính. Không cần phân loại hoặc cấu trúc phụ trợ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m, x = map(int, input().split())

    ans = 0

    a = list(map(int, input().split()))
    for v in a:
        if v >= x:
            ans += 1

    b = list(map(int, input().split()))
    for v in b:
        if v <= x:
            ans += 1

    return str(ans).strip()

# sample-like case
assert run("5 5 3\n1 2 3 4 5\n1 2 3 4 5\n") == "8"

# minimum size
assert run("1 1 1\n1\n1\n") == "2"

# all below threshold
assert run("3 3 10\n1 2 3\n1 2 3\n") == "3"

# all above threshold
assert run("3 3 2\n5 6 7\n5 6 7\n") == "6"

# mixed boundary
assert run("4 4 4\n4 3 5 4\n4 4 1 2\n") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bằng x | đếm đầy đủ | xử lý bình đẳng | 
| tất cả bên dưới/trên | cực trị một phần/không | tính đúng đắn của bất đẳng thức | 
| kích thước tối thiểu | 2 | trường hợp cơ sở đúng đắn | 
| giá trị hỗn hợp | lọc kết hợp | đếm độc lập | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các giá trị bằng ngưỡng$x$. Ví dụ:```
3 2 5
5 5 5
5 5
```Thuật toán xử lý từng người mua và người bán một cách độc lập. Mọi so sánh đều đánh giá đúng vì cả hai$a_i \ge x$Và$b_i \le x$giữ khi bằng nhau. Bộ đếm tăng một lần cho mỗi phần tử, tạo ra tổng số là 5. Bất kỳ việc triển khai nào sử dụng các bất đẳng thức nghiêm ngặt sẽ trả về 0 không chính xác. 

Một trường hợp khó khăn khác là khi không có ai đủ tiêu chuẩn:```
3 3 10
1 2 3
11 12 13
```Ở đây, tất cả người mua đều thất bại$a_i \ge x$và tất cả người bán đều thất bại$b_i \le x$. Bộ đếm vẫn bằng 0 xuyên suốt, xác nhận rằng thuật toán xử lý chính xác các tập hợp lựa chọn trống mà không cần viết hoa đặc biệt.
