---
title: "CF 105485L - \u6570\u7ec4\u4ea4\u6362"
description: "Chúng tôi được cung cấp một mảng có độ dài lẻ. Quá trình chạy trong đúng một nửa chiều dài được làm tròn xuống và mỗi vòng luôn loại bỏ hai phần tử đầu tiên sau khi cho phép hoán đổi liền kề ở đâu đó trong mảng."
date: "2026-06-23T01:57:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105485
codeforces_index: "L"
codeforces_contest_name: "2024 China Unversity of Geosciences (Wuhan) Freshman Contest"
rating: 0
weight: 105485
solve_time_s: 61
verified: true
draft: false
---

[CF 105485L - \u6570\u7ec4\u4ea4\u6362](https://codeforces.com/problemset/problem/105485/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng có độ dài lẻ. Quá trình chạy trong đúng một nửa chiều dài được làm tròn xuống và mỗi vòng luôn loại bỏ hai phần tử đầu tiên sau khi cho phép hoán đổi liền kề ở đâu đó trong mảng. 

Việc hoán đổi không bị hạn chế về vị trí nhưng được giới hạn ở một lần trao đổi liền kề trong mỗi vòng. Sau đó, hai phần tử đầu tiên sẽ bị xóa vĩnh viễn và hậu tố còn lại sẽ trở thành mảng mới cho vòng tiếp theo. Điều này tiếp tục cho đến khi chỉ còn lại một phần tử và nhiệm vụ là tối đa hóa phần tử sống sót cuối cùng đó bằng cách chọn các giao dịch hoán đổi một cách tối ưu. 

Khó khăn chính là việc hoán đổi được lựa chọn toàn cầu nhưng có hiệu lực cục bộ và việc xóa luôn được cố định ở phía trước. Điều này tạo ra sự căng thẳng giữa tốc độ các phần tử trôi về phía trước do bị xóa nhiều lần và mức độ chúng ta có thể trì hoãn hoặc tăng tốc độ trôi đó bằng cách sử dụng tối đa một lần hoán đổi mỗi vòng. 

Các ràng buộc cho phép n lên tới 2 × 10^5, loại trừ bất kỳ mô phỏng nào thử tất cả các quyết định hoán đổi có thể có hoặc theo dõi tất cả các trạng thái có thể có của mảng. Bất kỳ giải pháp nào phân nhánh theo các lựa chọn mỗi vòng hoặc mỗi vị trí sẽ bùng nổ theo cấp số nhân hoặc ít nhất là bậc hai. 

Một trường hợp thất bại tinh vi đối với lý luận ngây thơ xuất hiện khi người ta cho rằng chiến lược tốt nhất luôn là đưa phần tử tối đa về phía trước. Trực giác đó bị phá vỡ vì mặt trước chính xác là nơi việc xóa xảy ra ngay sau khi hoán đổi. Ví dụ: trong một mảng nhỏ như [1, 100, 2, 3, 4], việc đẩy 100 về phía trước liên tục vẫn có thể thất bại nếu nó bị kẹt ở hai vị trí đầu tiên quá sớm, vì việc xóa là vô điều kiện. 

Một trường hợp sai lầm khác là giả sử câu trả lời luôn là mức tối đa toàn cầu. Trong [10, 6, 2, 1, 5, 3, 4], tối đa là 10 nhưng nó không thể tồn tại vì nó bị loại bỏ quá sớm trong quá trình xóa bất kể hoán đổi. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực là mô phỏng mỗi vòng và ở mỗi vòng hãy thử tất cả các giao dịch hoán đổi liền kề có thể có trước khi xóa hai phần tử đầu tiên. Ngay cả khi chúng tôi chỉ theo dõi một mảng, việc lựa chọn vị trí hoán đổi sẽ tạo ra sự phân nhánh O(n) mỗi vòng và có các vòng O(n), dẫn đến số lượng chuỗi có thể có theo cấp số nhân. Ngay cả một mô phỏng đơn giản thử tất cả các vị trí hoán đổi mỗi vòng cũng đã tiêu tốn các hoạt động O(n^2) theo cách giải thích xác định tốt nhất, quá chậm đối với n lên tới 2 × 10^5. 

Cái nhìn sâu sắc quan trọng là quá trình này không thực sự là về sức mạnh sắp xếp lại cục bộ mà là về sự sống sót trước một lịch trình xóa cố định. Mỗi vòng sẽ loại bỏ chính xác hai phần tử ở phía trước, do đó số phận của mỗi phần tử phụ thuộc vào việc nó có thể tránh trôi vào hai vị trí đầu tiên trong bao lâu trước khi bị loại. Việc hoán đổi duy nhất mỗi vòng chỉ cho phép một phần tử thay đổi vị trí hiệu quả của nó tối đa một bước mỗi vòng, trong khi việc xóa sẽ dịch chuyển mọi thứ còn lại hai bước mỗi vòng. 

Điều này tạo ra một mô hình trôi tuyến tính: mỗi phần tử di chuyển sang trái hai do áp lực xóa và có thể được đẩy sang phải tối đa một phần tử mỗi vòng bằng cách sử dụng các hoán đổi. Hiệu ứng thực sự là mỗi phần tử chỉ có thể “trì hoãn” cái chết của nó một lượng giới hạn và giới hạn này chỉ phụ thuộc vào vị trí ban đầu của nó chứ không phụ thuộc vào giá trị của các phần tử khác. Điều này giải quyết vấn đề thành việc xác định chỉ số nào có khả năng tồn tại cho đến vòng cuối cùng. 

Khi quan điểm này được thiết lập, vấn đề sẽ giảm xuống còn việc xác định hậu tố của các vị trí có thể tồn tại trong tất cả các vòng và sau đó lấy giá trị lớn nhất trong số chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) đến O(exp(n)) | O(n) | Quá chậm | 
| Phân tích sự sống còn của vị trí | O(n) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Bây giờ chúng tôi chuyển lý do sinh tồn thành một phép tính trực tiếp. 

### Hướng dẫn thuật toán

1. Quan sát rằng có (n − 1) / 2 vòng và mỗi vòng sẽ xóa chính xác hai phần tử ở phía trước, do đó quá trình sẽ loại bỏ tất cả trừ một phần tử. 
2. Theo dõi một phần tử ban đầu ở vị trí i. Sau k vòng mà không hoán đổi, nó dịch chuyển sang trái 2k vị trí do bị xóa, do đó vị trí hiệu dụng của nó trở thành i − 2k. 
3. Sử dụng một lần hoán đổi mỗi vòng làm cơ chế điều chỉnh. Mỗi vòng cho phép tối đa một lần hoán đổi liền kề, do đó, trong k vòng, một phần tử có thể được đẩy sang phải tối đa k vị trí so với độ lệch xóa. 
4. Kết hợp cả hai hiệu ứng. Sau k vòng, vị trí hiệu quả trong trường hợp tốt nhất của một phần tử ban đầu tại i là i − 2k + k, đơn giản hóa thành i − k. 
5. Để một phần tử tồn tại cho đến vòng cuối cùng k = (n − 1)/2, vị trí hiệu dụng của nó phải duy trì dương tại thời điểm đó. Điều này đưa ra điều kiện i − (n − 1) / 2 > 0. 
6. Điều này đơn giản hóa thành i > (n − 1) / 2, nghĩa là chỉ các phần tử ở nửa sau của mảng mới có thể tồn tại cho đến cuối. 
7. Do đó, câu trả lời cuối cùng là giá trị lớn nhất giữa các chỉ số từ (n + 1)/2 đến n trong cách lập chỉ mục dựa trên 1. 

### Tại sao nó hoạt động 

Quá trình xóa sẽ loại bỏ một cách xác định hai phần tử mỗi vòng khỏi phía trước, điều này tạo ra sự trôi dạt sang trái cứng nhắc cho mọi phần tử. Điều khiển duy nhất được thuật toán cung cấp là điều chỉnh giới hạn sang phải tối đa một vị trí trên mỗi vòng thông qua một lần hoán đổi liền kề. Bởi vì cả hai hiệu ứng đều tỷ lệ tuyến tính với số vòng, chuyển vị ròng của bất kỳ phần tử nào cũng tuyến tính và được xác định đầy đủ bởi chỉ số bắt đầu của nó. 

Điều này có nghĩa là sự tồn tại không bị ảnh hưởng bởi những thay đổi thứ tự tương đối giữa các cặp phần tử tùy ý nằm sâu trong mảng mà chỉ bị ảnh hưởng bởi liệu một phần tử có thể tránh đi vào vùng xóa trước vòng cuối cùng hay không. Khi một phần tử bắt đầu ở nửa đầu, khả năng điều chỉnh sang phải tối đa có thể của nó không đủ để chống lại việc xóa lặp đi lặp lại, do đó cuối cùng nó phải bị xóa. Các yếu tố trong nửa sau có đủ độ trễ để nằm ngoài khoảng thời gian xóa đủ lâu để tồn tại đến cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    start = n // 2  # 0-based index of (n+1)/2 in 1-based
    print(max(a[start:]))

if __name__ == "__main__":
    solve()
```Việc triển khai áp dụng trực tiếp ranh giới tồn tại dẫn xuất. Sự tinh tế duy nhất là lập chỉ mục: trong lập chỉ mục dựa trên 0, phân đoạn còn lại bắt đầu tại n // 2. 

Phần còn lại là mức tối đa đơn giản trên một hậu tố. Không cần mô phỏng vì tất cả hành vi động của các giao dịch hoán đổi đã được đưa vào bất đẳng thức sinh tồn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7
10 6 2 1 5 3 4
```Chúng tôi theo dõi khu vực còn sống sót. 

| Bước | Hậu tố mảng được xem xét | Lý do | 
| --- | --- | --- | 
| Ban đầu | [10, 6, 2, 1, 5, 3, 4] | mảng đầy đủ | 
| Sau ranh giới | [1, 5, 3, 4] | chỉ (n+1)/2 phần tử cuối cùng tồn tại | 
| Kết quả | 5 | hậu tố tối đa | 

Điều này xác nhận rằng chiến lược tối ưu không quan tâm đến mức tối đa toàn cầu mà chỉ quan tâm đến những phần tử nào được phép tồn tại về mặt cấu trúc sau quá trình xóa. 

### Ví dụ 2 

đầu vào:```
5
1 2 3 4 5
```| Bước | Hậu tố mảng được xem xét | Lý do | 
| --- | --- | --- | 
| Ban đầu | [1, 2, 3, 4, 5] | mảng đầy đủ | 
| Sau ranh giới | [3, 4, 5] | ba yếu tố cuối cùng tồn tại | 
| Kết quả | 5 | hậu tố tối đa | 

Điều này cho thấy ngay cả những phần tử nhỏ ở phía trước cũng không liên quan nếu chúng nằm ngoài vùng có thể sống sót, vì chúng được đảm bảo sẽ bị xóa trước khi quá trình kết thúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | một lượt để tính hậu tố tối đa | 
| Không gian | O(1) | chỉ sử dụng bộ nhớ mảng đầu vào | 

Giải pháp này dễ dàng phù hợp với các giới hạn cho n lên đến 2 × 10^5, vì nó chỉ thực hiện một lần quét tuyến tính và không mô phỏng quá trình xóa hoán đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    start = n // 2
    return str(max(a[start:]))

# provided sample
assert run("7\n10 6 2 1 5 3 4\n") == "5"

# minimum size
assert run("1\n42\n") == "42"

# simple increasing
assert run("5\n1 2 3 4 5\n") == "5"

# all equal
assert run("5\n7 7 7 7 7\n") == "7"

# maximum at front but not reachable
assert run("5\n100 1 1 1 1\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 phần tử đơn | 42 | trường hợp cơ bản tầm thường | 
| mảng tăng dần | 5 | hậu tố thống trị | 
| tất cả đều bình đẳng | 7 | không thiên vị về cấu trúc | 
| phía trước lớn tối đa | 1 | các yếu tố ban đầu không thể tồn tại | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi phần tử lớn nhất nằm ở nửa đầu của mảng. Trong tình huống đó, thật hấp dẫn khi cho rằng các giao dịch hoán đổi luôn có thể giải cứu nó, nhưng sự trôi dạt do việc xóa đi nhiều lần đã lấn át ngân sách hoán đổi hạn chế. 

Đối với đầu vào:```
5
100 1 2 3 4
```thuật toán chọn hậu tố [2, 3, 4] và trả về 4. Phần tử 100 bắt đầu ở vị trí 1, nhưng mỗi vòng sẽ loại bỏ hai phần tử ở phía trước và việc hoán đổi một lần cho mỗi vòng không thể bù đắp cho việc luôn ở trong vùng xóa sớm trong quá trình. 

Một trường hợp cạnh khác là các mảng thống nhất như:```
5
7 7 7 7 7
```Ở đây mọi chiến lược đều mang lại giá trị cuối cùng như nhau. Quy tắc hậu tố vẫn được áp dụng và thuật toán trả về chính xác 7 bằng cách lấy tối đa ba phần tử cuối cùng.
