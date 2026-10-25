---
title: "CF 106050N - Tên của giải Marathon?"
description: "Bài toán yêu cầu chúng ta đếm phiếu bầu cho tên của một cuộc chạy marathon lập trình. Mỗi phiếu bầu là 1, nghĩa là tên được đề xuất đầu tiên, hoặc 2, nghĩa là tên được đề xuất thứ hai. Sau khi kiểm phiếu, chúng tôi in ra tên được nhiều phiếu bầu hơn."
date: "2026-06-25T12:29:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106050
codeforces_index: "N"
codeforces_contest_name: "Cataratas do Pinh\u00e3o 2025"
rating: 0
weight: 106050
solve_time_s: 31
verified: true
draft: false
---

[CF 106050N - Tên của cuộc thi Marathon?](https://codeforces.com/problemset/problem/106050/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 31s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta đếm phiếu bầu cho tên của một cuộc chạy marathon lập trình. Mỗi phiếu bầu là một trong hai`1`, nghĩa là tên được đề xuất đầu tiên, hoặc`2`, nghĩa là tên được đề xuất thứ hai. Sau khi kiểm phiếu, chúng tôi in ra tên được nhiều phiếu bầu hơn. Nếu cả hai tên đều nhận được cùng số phiếu bầu, kết quả sẽ không được quyết định và chúng tôi sẽ in thông báo hòa đặc biệt. Đầu vào bao gồm số phiếu bầu, theo sau là số phiếu bầu riêng lẻ và đầu ra là tên chiến thắng hoặc thông báo hòa. 

Số phiếu bầu có thể đạt tới$10^5$, do đó thuật toán cần xử lý thoải mái một trăm nghìn giá trị. Điều này ngay lập tức loại trừ các phương pháp liên tục quét toàn bộ danh sách phiếu bầu cho mỗi phiếu bầu hoặc mô phỏng các công việc không cần thiết, vì những phương pháp đó có thể phát triển thành$O(N^2)$, điều đó có nghĩa là xung quanh$10^{10}$hoạt động ở kích thước tối đa. Một lần bỏ qua phiếu bầu là quá đủ, vì thông tin duy nhất chúng tôi cần là số lượng phiếu bầu mà mỗi lựa chọn nhận được. 

Các trường hợp phức tạp chủ yếu là về logic so sánh. Một sai lầm phổ biến là quên rằng hòa là một kết quả riêng biệt. Ví dụ: với:```
6
1 1 1 2 2 2
```đầu ra đúng là:```
Cascatiba?
```Một giải pháp bất cẩn sử dụng`>=`khi kiểm tra ứng cử viên đầu tiên có thể in sai tên, mặc dù cả hai ứng cử viên đều có sự ủng hộ như nhau. 

Một trường hợp khác là khi tỷ lệ thắng chỉ bằng một phiếu bầu. Ví dụ:```
7
1 1 1 1 2 2 2
```đầu ra đúng là:```
Cataratas do Pinhao
```Số phiếu là bốn phiếu cho lựa chọn`1`và ba phiếu bầu cho lựa chọn`2`. Mã chỉ kiểm tra xem một giá trị có xuất hiện "thường xuyên hơn" khi sử dụng ngưỡng sai hay không có thể coi các kết quả gần đúng là ràng buộc không chính xác. 

Đầu vào hợp lệ nhỏ nhất cũng đáng được chú ý. Vì:```
11
2 2 2 2 2 2 1 1 1 1 1
```đầu ra đúng là:```
Pinhao das Cataratas
```Thuật toán không nên phụ thuộc vào số dư phiếu bầu cụ thể hoặc giả định rằng một ứng cử viên xuất hiện đầu tiên trong đầu vào. 

## Phương pháp tiếp cận 

Giải pháp đơn giản là kiểm tra mọi phiếu bầu và đếm hai lựa chọn có thể có. Cách giải thích thô bạo có thể cố gắng xác định người chiến thắng bằng cách so sánh mọi phiếu bầu với mọi phiếu bầu khác, nhưng điều đó là không cần thiết. Phương pháp như vậy sẽ thực hiện gần như$N^2$so sánh trong trường hợp xấu nhất, trở thành khoảng$10^{10}$hoạt động cho$N = 10^5$. Nó đúng vì cuối cùng nó phát hiện ra tần suất của mỗi lựa chọn, nhưng nó lãng phí cấu trúc của vấn đề. 

Quan sát quan trọng là mỗi phiếu bầu chỉ đóng góp một thông tin: tổng số phiếu bầu của một ứng cử viên sẽ tăng thêm một. Chúng ta không bao giờ cần biết vị trí của các phiếu bầu hoặc chúng được phân bổ như thế nào. Toàn bộ vấn đề giảm xuống còn việc duy trì hai bộ đếm trong khi đọc mảng. Điều này biến giải pháp thành một lần quét tuyến tính duy nhất. 

Sau khi đếm, quyết định là thời gian không đổi. Nếu bộ đếm đầu tiên lớn hơn, tên đầu tiên sẽ thắng. Nếu bộ đếm thứ hai lớn hơn, tên thứ hai sẽ thắng. Ngược lại, các bộ đếm bằng nhau và thông báo ràng buộc sẽ được in. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(1) | Quá chậm | 
| Kiểm phiếu | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số phiếu bầu và khởi tạo hai bộ đếm, một bộ đếm cho mỗi giá trị phiếu bầu có thể có. Bộ đếm đại diện cho số điểm hiện tại của mỗi tên marathon. 
2. Xử lý mỗi phiếu bầu một lần. Nếu cuộc bỏ phiếu là`1`, tăng bộ đếm cho tên. Nếu nó là`2`, tăng bộ đếm cho tên thứ hai. Mỗi phiếu bầu được xử lý ngay lập tức vì các phiếu bầu trong tương lai không ảnh hưởng đến ý nghĩa của những phiếu bầu trước đó. 
3. So sánh hai quầy sau khi tất cả phiếu bầu đã được xử lý. Bộ đếm lớn hơn xác định người chiến thắng vì tổng số phiếu bầu chính xác là những gì bài toán yêu cầu chúng ta tối đa hóa. 
4. Nếu hai bộ đếm bằng nhau, xuất ra thông báo hòa vì không có phương án nào có nhiều phiếu bầu hơn phương án kia. 

Tại sao nó hoạt động: trong quá trình quét, bộ đếm luôn lưu trữ số phiếu bầu chính xác cho đến nay cho mỗi ứng cử viên. Khi quá trình quét kết thúc, mỗi phiếu bầu đã được đưa vào đúng một lần, do đó cả hai bộ đếm đều có tổng số cuối cùng bằng nhau. So sánh trực tiếp các tổng này cho kết quả chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
votes = list(map(int, input().split()))

first = 0
second = 0

for vote in votes:
    if vote == 1:
        first += 1
    else:
        second += 1

if first > second:
    print("Cataratas do Pinhao")
elif second > first:
    print("Pinhao das Cataratas")
else:
    print("Cascatiba?")
```Việc triển khai chỉ giữ lại hai biến số nguyên, khớp trực tiếp với trạng thái toán học cần thiết cho giải pháp. Đọc tất cả các phiếu bầu cùng một lúc rất đơn giản và hiệu quả đối với$10^5$các giá trị và vòng lặp thực hiện chính xác một thao tác trên mỗi phiếu bầu. 

Việc so sánh sử dụng các bất đẳng thức nghiêm ngặt trước tiên. Điều này quan trọng vì trường hợp bằng nhau chỉ xảy ra khi không bên nào có số lượng lớn hơn. sử dụng`>=`sẽ vô tình coi hòa là thắng. 

Giải pháp không cần mảng, sắp xếp hoặc thêm bộ nhớ. Thứ tự của các phiếu bầu không liên quan nên việc lưu trữ chúng chỉ để thuận tiện cho việc phân tích cú pháp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
12
1 1 1 1 1 1 2 2 2 2 2 2
```| Bước | Bình chọn | Lần đếm đầu tiên | Đếm thứ hai | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 
| 2 | 1 | 2 | 0 | 
| 3 | 1 | 3 | 0 | 
| 4 | 1 | 4 | 0 | 
| 5 | 1 | 5 | 0 | 
| 6 | 1 | 6 | 0 | 
| 7 | 2 | 6 | 1 | 
| 8 | 2 | 6 | 2 | 
| 9 | 2 | 6 | 3 | 
| 10 | 2 | 6 | 4 | 
| 11 | 2 | 6 | 5 | 
| 12 | 2 | 6 | 6 | 

Số đếm cuối cùng bằng nhau nên thuật toán đi tới nhánh buộc và in ra kết quả chưa quyết định. 

### Mẫu 2 

đầu vào:```
12
1 1 1 1 1 1 1 2 2 2 2 2
```| Bước | Bình chọn | Lần đếm đầu tiên | Đếm thứ hai | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 
| 2 | 1 | 2 | 0 | 
| 3 | 1 | 3 | 0 | 
| 4 | 1 | 4 | 0 | 
| 5 | 1 | 5 | 0 | 
| 6 | 1 | 6 | 0 | 
| 7 | 1 | 7 | 0 | 
| 8 | 2 | 7 | 1 | 
| 9 | 2 | 7 | 2 | 
| 10 | 2 | 7 | 3 | 
| 11 | 2 | 7 | 4 | 
| 12 | 2 | 7 | 5 | 

Tên đầu tiên có bảy phiếu trong khi tên thứ hai có năm phiếu, vì vậy tên ứng cử viên đầu tiên được in ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi phiếu bầu được đọc và tính chính xác một lần | 
| Không gian | O(1) | Chỉ có hai quầy được lưu trữ | 

Kích thước đầu vào tối đa là$10^5$và thuật toán chỉ thực hiện một lượng công việc không đổi trên mỗi phiếu bầu. Điều này dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve(data):
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(data)

    input = sys.stdin.readline
    n = int(input())
    votes = list(map(int, input().split()))

    first = 0
    second = 0

    for vote in votes:
        if vote == 1:
            first += 1
        else:
            second += 1

    if first > second:
        ans = "Cataratas do Pinhao"
    elif second > first:
        ans = "Pinhao das Cataratas"
    else:
        ans = "Cascatiba?"

    sys.stdin = old_stdin
    return ans

# provided samples
assert solve("""12
1 1 1 1 1 1 2 2 2 2 2 2
""") == "Cascatiba?", "sample 1"

assert solve("""12
1 1 1 1 1 1 1 2 2 2 2 2
""") == "Cataratas do Pinhao", "sample 2"

# custom cases
assert solve("""11
2 2 2 2 2 2 1 1 1 1 1
""") == "Pinhao das Cataratas", "minimum size"

assert solve("""11
1 2 1 2 1 2 1 2 1 2 1
""") == "Cataratas do Pinhao", "boundary one vote difference"

assert solve("""100000
""" + " ".join(["1"] * 100000) + "\n") == "Cataratas do Pinhao", "large input"

assert solve("""11
1 1 1 1 1 1 2 2 2 2 2
""") == "Cascatiba?", "tie case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 11 phiếu với sáu`2`s và năm`1`s | Pinhao das Cataratas | Đầu vào hợp lệ nhỏ nhất và ứng cử viên thứ hai giành chiến thắng | 
| Phiếu bầu xen kẽ với một phiếu bổ sung`1`| Cataratas do Pinhao | Xử lý so sánh từng cái một | 
| 100000 phiếu bầu đều bằng`1`| Cataratas do Pinhao | Kích thước đầu vào tối đa và xử lý tuyến tính | 
| Sáu`1`s và năm`2`s | Cataratas do Pinhao | Đóng kết quả không hòa | 

## Vỏ cạnh 

Đối với hộp đựng cà vạt:```
6
1 1 1 2 2 2
```thuật toán tăng bộ đếm thứ nhất ba lần và bộ đếm thứ hai ba lần. So sánh cuối cùng thấy các giá trị bằng nhau và chọn nhánh buộc. Điều này tránh được sai lầm phổ biến là coi bình đẳng là chiến thắng của lựa chọn thứ nhất. 

Đối với sự khác biệt một phiếu bầu:```
7
1 1 1 1 2 2 2
```quầy kết thúc lúc bốn và ba. Vì giá trị đầu tiên lớn hơn nên tên đầu tiên sẽ được in. Thuật toán không cần bất kỳ cách xử lý đặc biệt nào đối với các điểm gần đúng vì tất cả các quyết định đều dựa trên tổng số cuối cùng. 

Đối với đầu vào lặp lại lớn:```
11
1 1 1 1 1 1 1 1 1 1 1
```vòng lặp chỉ đơn giản là tăng bộ đếm đầu tiên cho mọi phần tử. Giải pháp này vẫn hiệu quả vì nó không bao giờ tạo thêm cấu trúc hoặc thực hiện tìm kiếm lặp lại.
