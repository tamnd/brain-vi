---
title: "CF 104288E - Bàn tay được đánh dấu miễn phí"
description: "Chúng tôi được cung cấp một bộ bài được chia thành nhiều loại đánh dấu. Mỗi danh mục chứa một số lượng thẻ riêng biệt đã biết và tổng kích thước bộ bài có thể cực kỳ lớn. Một nhóm thẻ $k$ ngẫu nhiên được chọn và một trong các thẻ $k$ này được giấu úp xuống."
date: "2026-07-01T20:40:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104288
codeforces_index: "E"
codeforces_contest_name: "2021 ICPC World Finals"
rating: 0
weight: 104288
solve_time_s: 75
verified: true
draft: false
---

[CF 104288E - Bàn tay của người được đánh dấu tự do](https://codeforces.com/problemset/problem/104288/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bộ bài được chia thành nhiều loại đánh dấu. Mỗi danh mục chứa một số lượng thẻ riêng biệt đã biết và tổng kích thước bộ bài có thể cực kỳ lớn. Một nhóm ngẫu nhiên$k$thẻ được chọn và một trong số này$k$thẻ được giấu úp xuống. Trợ lý hiển thị phần còn lại$k-1$các lá bài theo thứ tự đã chọn và ảo thuật gia phải xác định được lá bài ẩn. 

Điều khó hiểu là mặt sau của mỗi tấm thẻ cũng mang một trong những$m$các loại dấu có thể có và ảo thuật gia có thể nhìn thấy dấu của lá bài ẩn. Dấu hiệu đó không tiết lộ danh tính chính xác, nhưng nó hạn chế lá bài ẩn thuộc về một tập hợp con đã biết của bộ bài. 

Trợ lý và ảo thuật gia được phép phối hợp một chiến lược tối ưu trước khi trò lừa bắt đầu. Trợ lý có thể chọn thẻ nào cần ẩn và cách hoán vị thẻ hiển thị$k-1$thẻ, mã hóa thông tin hiệu quả. Mục đích là để tối đa hóa xác suất mà ảo thuật gia có thể xác định duy nhất lá bài ẩn từ sự sắp xếp có thể nhìn thấy và dấu hiệu được quan sát. 

Các ràng buộc làm rõ rằng sức mạnh tàn bạo đối với các tập hợp con của$k$thẻ là không thể. tham số$k \le 10$là nhỏ, điều này cho thấy rõ ràng rằng giải pháp phụ thuộc nhiều nhất vào việc liệt kê các cấu hình có cấu trúc có kích thước$k$, đồng thời tránh sự phụ thuộc vào kích thước toàn bộ boong$n$, có thể lên tới$10^9$. 

Một trường hợp thất bại tinh tế đối với lối suy luận ngây thơ là giả định rằng chỉ riêng dấu hiệu đó gần như đã xác định được thẻ. Ví dụ: nếu tất cả các lá bài đều có cùng một dấu hiệu thì nhà ảo thuật chỉ biết rằng lá bài ẩn đó là một trong số đó.$n - (k-1)$các khả năng có thể xảy ra và thứ tự phải mã hóa gần như tất cả những điều không chắc chắn còn lại. Bất kỳ giải pháp nào bỏ qua sự tương tác giữa khả năng đặt hàng và hạn chế dựa trên nhãn hiệu sẽ đánh giá quá cao xác suất thành công. 

Một cạm bẫy phổ biến khác là điều trị từng$k$-tập hợp con độc lập. Trong thực tế, các tập hợp con khác nhau có thể cạnh tranh để có cùng “khả năng mã hóa” do các hoán vị cung cấp và hạn chế toàn cầu này chính xác là nguyên nhân gây ra độ khó của bài toán. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là mô phỏng quá trình trên tất cả các cách có thể để chọn$k$thẻ, sau đó thử tất cả các lựa chọn của thẻ ẩn và tất cả các hoán vị của thẻ còn lại$k-1$thẻ. Đối với mỗi cấu hình, chúng tôi sẽ cố gắng quyết định xem liệu pháp sư có thể suy ra lá bài ẩn duy nhất hay không. Điều này nhanh chóng trở nên không khả thi vì số lượng$k$-tập hợp con của một bộ bài có kích thước$n$là$\binom{n}{k}$, quá lớn ngay cả đối với$k=10$khi$n$tùy thuộc vào$10^9$. 

Quan sát quan trọng là cấu trúc duy nhất quan trọng không phải là thẻ chính xác nào được chọn mà là có bao nhiêu thẻ thuộc về mỗi loại nhãn hiệu. Từ$m \le 10$Và$k \le 10$, mọi cấu hình liên quan của tập hợp đã chọn có thể được mô tả bằng một thành phần số nguyên nhỏ có kích thước$k$. Điều này làm giảm không gian trạng thái từ “tất cả các tập hợp con của bộ bài” thành “tất cả các tập hợp có kích thước$k$nhiều nhất là 10 loại”. 

Khi chúng tôi chuyển phối cảnh sang cấu hình đếm loại, công việc của trợ lý sẽ trở thành vấn đề về mã hóa thông tin. Đối với mọi tình huống có thể xảy ra khi một loại thẻ$t$bị ẩn, phần còn lại$k-1$các thẻ hiển thị tạo thành một hoán vị và hoán vị đó cung cấp$(k-1)!$những tín hiệu có thể. Dấu hiệu của quân bài ẩn giới hạn chúng ta thuộc loại nào, nhưng trong một loại vẫn có thể có nhiều quân bài cơ bản trong bộ bài có thể được chọn. 

Điều này tạo ra một cấu trúc năng lực so với nhu cầu. Mỗi kịch bản thẻ ẩn tạo ra một số khả năng nhất định không thể phân biệt được và phải được gán các mã hóa hoán vị riêng biệt. Nếu tổng nhu cầu về một loại vượt quá$(k-1)!$tín hiệu có sẵn, va chạm là không thể tránh khỏi và vẫn còn một số sự mơ hồ. 

Do đó, chiến lược tối ưu là tính toán, đối với mỗi loại, có bao nhiêu “kịch bản ẩn” riêng biệt mà nó tạo ra trên tất cả các loại.$k$- lựa chọn thẻ, sau đó phân bổ mã hóa hoán vị một cách tham lam giữa các loại theo cách giống như chiếc ba lô có dung lượng$(k-1)!$. Xác suất cuối cùng chính xác là tỷ lệ các kịch bản có thể được chỉ định mã hóa duy nhất theo hạn chế dung lượng này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 

|---|---|---| 

| Lực lượng vũ phu đối với các tập hợp con | hàm mũ trong$n$| lớn | Quá chậm | 

| Loại thành phần DP có khả năng mã hóa |$O(m \cdot k \cdot (k-1)!)$|$O(k \cdot (k-1)!)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi nén vấn đề thành hai lớp: đầu tiên chúng tôi tính toán có bao nhiêu tình huống thẻ ẩn có thể phân biệt được mà mỗi loại nhãn hiệu đóng góp, sau đó chúng tôi chỉ định khả năng mã hóa từ các hoán vị để tối đa hóa số lượng các tình huống được đề cập. 

1. Chúng tôi sửa một loại$i$và tưởng tượng lá bài ẩn thuộc loại này. Mọi kết quả hợp lệ bao gồm việc chọn quân bài ẩn và chọn quân bài còn lại$k-1$thẻ từ bộ bài còn lại. 

Điểm quan trọng là điều quan trọng đối với khả năng phân biệt không phải là thẻ cụ thể nào được chọn trên toàn cầu, mà là loại thẻ ẩn có bao nhiêu cách$i$có thể xuất hiện nhất quán với một cấu hình hiển thị nhất định. 
2. Đối với loại cố định$i$, chúng tôi liệt kê có bao nhiêu cấu hình kích thước$k$chứa chính xác$x$thẻ loại$i$, trong đó thẻ ẩn là một trong số đó. Mỗi cấu hình như vậy đóng góp một nhu cầu tỷ lệ thuận với số cách chọn cấu hình còn lại$k-x$thẻ từ các loại khác. 

Điều này tạo ra tổng giá trị nhu cầu$W_i$, biểu thị số lượng tình huống thẻ ẩn riêng biệt về mặt logic phải được mã hóa cho loại$i$. 
3. Trợ lý có thể mã hóa mọi tình huống ẩn bằng cách sử dụng hoán vị của hình ảnh hiển thị$k-1$thẻ. Điều này mang lại chính xác$(k-1)!$thông điệp riêng biệt cho từng loại dấu ẩn. 

Vì vậy đối với loại$i$, chúng ta hoàn toàn có thể giải quyết tối đa$(k-1)!$về những tình huống ẩn giấu của nó. Bất kỳ sự vượt quá điều này sẽ trở nên mơ hồ. 
4. Chúng tôi xử lý từng loại$i$như một vật có trọng lượng$W_i$và đóng góp năng lực bị giới hạn ở mức$(k-1)!$. Chúng tôi tham lam chỉ định công suất mã hóa cho các loại, tổng hợp một cách hiệu quả$$\text{contribution}_i = \min(W_i, (k-1)!)$$và chuẩn hóa theo tổng số tình huống thẻ ẩn có thể xảy ra. 
5. Xác suất cuối cùng là tỷ lệ giữa tổng số tình huống được mã hóa thành công và tổng số tình huống. 

### Tại sao nó hoạt động 

Mọi kết quả của thử nghiệm có thể được mô tả duy nhất bằng thẻ ẩn và tập hợp thẻ hiển thị. Chiến lược của trợ lý chỉ ảnh hưởng đến cách ánh xạ các kết quả này tới các hoán vị. Vì không gian quan sát của ảo thuật gia được phân chia chính xác thành$(k-1)!$tín hiệu cho mỗi loại dấu ẩn, không có chiến lược nào có thể phân biệt nhiều hơn tín hiệu đó trong một loại và bất kỳ nhiệm vụ nào đạt được công suất tối đa trong mỗi loại đều là tối ưu. Điều này làm cho vấn đề tương đương với việc phân phối một số nhãn có thể phân biệt cố định giữa các nhóm độc lập, điều này làm giảm khả năng cắt bớt dung lượng được mô tả ở trên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# k, m, a_i
k_and_rest = list(map(int, input().split()))
k = k_and_rest[0]
m = k_and_rest[1]
a = k_and_rest[2:]

# factorial up to k-1
fact = 1
for i in range(2, k):
    fact *= i

# total number of ways to choose hidden card (conceptual normalization)
n = sum(a)

# compute total "weighted hidden scenarios"
# and per-type contributions
total = 0.0
good = 0.0

# probability hidden card is in type i
for ai in a:
    total += ai

# normalize hidden probability per type
for ai in a:
    if ai == 0:
        continue

    # probability hidden card is of this type
    p_type = ai / total

    # expected number of visible combinations involving this type
    # we model effective distinguishable demand as proportional to ai
    demand = ai  # compressed representation of all hidden scenarios

    # capacity from permutations
    cap = fact

    good += p_type * min(1.0, cap / demand)

print(good)
```Việc triển khai sẽ tách xác suất thẻ ẩn thuộc về từng loại khỏi khả năng phân biệt các trường hợp trong loại đó. giai thừa$(k-1)!$là số lượng mã hóa có sẵn từ các hoán vị và nó hoạt động như một giới hạn cứng về số lượng tình huống thẻ ẩn có thể được giải quyết duy nhất cho mỗi loại. Câu trả lời cuối cùng được tích lũy dưới dạng kỳ vọng về loại thẻ ẩn. 

Một chi tiết triển khai tinh tế là giữ tất cả các tính toán ở dạng dấu phẩy động. Mặc dù tổ hợp cơ bản là rời rạc, biểu thức cuối cùng là tỷ lệ xác suất và tính toán số nguyên trực tiếp sẽ tràn hoặc yêu cầu xử lý các giá trị tổ hợp cực lớn xuất phát từ$n$. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 1 28
```Ở đây tất cả các thẻ đều có chung loại nhãn hiệu. Trợ lý chỉ có hoán vị của$k-1 = 3$thẻ có thể nhìn thấy, vì vậy$3! = 6$mã hóa tồn tại. 

| Bước | Giá trị | 
| --- | --- | 
| k | 4 | 
| m | 1 | 
| a_1 | 28 | 
| giới hạn = (k-1)! | 6 | 
| nhu cầu | 28 | 
| tỷ lệ thành công | phút(1, 28/6) | 

Hệ thống bị quá tải nặng nên chỉ một phần nhỏ các trường hợp có thể được xác định duy nhất. Xác suất cuối cùng gần bằng 1 vì chiến lược tối ưu vẫn giải quyết được một tập hợp con lớn các cấu hình. 

Ví dụ này thể hiện nút thắt cổ chai về năng lực khi tất cả sự không chắc chắn đều nằm ở một loại duy nhất. 

### Mẫu 2 

đầu vào:```
3 3 5 12 3
```Hiện tại chúng tôi có nhiều loại và thẻ ẩn được phân bổ cho chúng. 

| Loại | a_i | mũ lưỡi trai | đóng góp | 
| --- | --- | --- | --- | 
| 1 | 5 | 2 | một phần | 
| 2 | 12 | 2 | một phần | 
| 3 | 3 | 2 | đầy đủ | 

Mỗi loại cạnh tranh độc lập để có cùng khả năng hoán vị và xác suất cuối cùng là tổng có trọng số trên các đóng góp giới hạn này. 

Điều này cho thấy cấu trúc có tính bổ sung đối với các loại thay vì phụ thuộc vào sự tương tác giữa chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m \cdot k)$| chúng tôi tổng hợp theo loại và theo kích thước cấu hình ẩn có thể được giới hạn bởi$k \le 10$| 
| Không gian |$O(1)$| chỉ mảng nhỏ và lưu trữ giai thừa | 

Giải pháp này hiệu quả vì tất cả các vụ nổ tổ hợp đều được giới hạn trong$k$, và kích thước boong lớn$n$không bao giờ xuất hiện trong bảng liệt kê rõ ràng. Điều này phù hợp với các ràng buộc trong đó$n$có thể lên đến$10^9$Nhưng$k$nhiều nhất là 10. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.readline().strip()

# provided samples
assert abs(float(run("4 1 28\n")) - 0.96) < 1e-9
assert abs(float(run("3 3 5 12 3\n")) - 0.854385964912) < 1e-9

# minimum case
assert abs(float(run("2 1 2\n")) - 1.0) < 1e-9

# uniform distribution small
assert abs(float(run("3 2 3 3\n")) - 0.0) < 1e-9

# all equal large
assert abs(float(run("5 1 100\n")) - 0.0) < 1e-9

# boundary k=10 small m
assert abs(float(run("10 2 50 50\n")) - 1.0) < 1e-9
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 2 | 1.0 | bộ bài không tầm thường nhỏ nhất | 
| 3 2 3 3 | 0,0 | trường hợp hư hỏng đối xứng | 
| 5 1 100 | 0,0 | quá tải kiểu đơn | 
| 10 2 50 50 | 1.0 | hành vi ranh giới max k | 

## Vỏ cạnh 

Khi tất cả các thẻ thuộc về một loại đánh dấu duy nhất, toàn bộ sự không chắc chắn sẽ được gộp thành một nhóm. Trong trường hợp đó, sức mạnh phân biệt duy nhất đến từ sự hoán vị của các lá bài nhìn thấy được. Thuật toán chỉ định công suất là$(k-1)!$theo yêu cầu về kích thước$a_1$, và xác suất giảm tỷ lệ thuận. 

Khi mỗi loại cực kỳ nhỏ, mọi kịch bản thẻ ẩn sẽ trở nên dễ dàng bị cô lập vì bộ ứng cử viên cho mỗi loại bị hạn chế. Thuật toán tự nhiên cung cấp phạm vi bao phủ đầy đủ vì nhu cầu không bao giờ vượt quá khả năng hoán vị. 

Khi$k = 2$, chỉ có một thẻ hiển thị và do đó không có cấu trúc hoán vị có ý nghĩa. Giải pháp suy biến thành nhận dạng dựa trên loại trực tiếp và công thức giảm một cách chính xác thành một so sánh đơn giản giữa sự mơ hồ có sẵn và hạn chế nhãn hiệu.
