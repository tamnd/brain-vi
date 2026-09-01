---
title: "CF 104454A - Máy tạo câu đố"
description: "Chúng ta được cung cấp một lưới $n lần n$ được chỉ định một phần, nhưng trên thực tế chỉ có hàng đầu tiên là cố định. Hàng đó là hoán vị của các số từ 1 đến $n$, nghĩa là mỗi giá trị xuất hiện đúng một lần."
date: "2026-06-30T14:24:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104454
codeforces_index: "A"
codeforces_contest_name: "ICPC Central Russia Regional Contest, 2021"
rating: 0
weight: 104454
solve_time_s: 75
verified: false
draft: false
---

[CF 104454A - Trình tạo câu đố](https://codeforces.com/problemset/problem/104454/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một phần xác định$n \times n$lưới, nhưng thực tế chỉ có hàng đầu tiên là cố định. Hàng đó là hoán vị của các số từ 1 đến$n$, nghĩa là mọi giá trị xuất hiện chính xác một lần. Nhiệm vụ là xây dựng phần còn lại$n-1$hàng sao cho hình vuông cuối cùng trở thành hình vuông Latin: mỗi hàng chứa mỗi số từ 1 đến$n$chính xác một lần và mỗi cột cũng chứa mỗi số đúng một lần. 

Hạn chế chính là hàng đầu tiên không phải là dữ liệu tùy ý, nó trở thành hạt giống xác định toàn bộ cấu trúc. Mọi hàng khác phải nhất quán với nó về tính duy nhất theo hàng và theo cột. 

Các ràng buộc là nhỏ, với$n \leq 100$. Điều này ngay lập tức loại bỏ mọi lo ngại về hiệu quả. Thậm chí một$O(n^3)$việc xây dựng sẽ diễn ra thoải mái vì tổng số ô chỉ$10^4$. Khó khăn thực sự không phải là hiệu suất mà là việc hiện thực hóa cấu trúc của những lần hoàn thành hợp lệ. 

Một nỗ lực ngây thơ có thể cố gắng điền vào từng hàng một cách độc lập bằng cách tham lam đặt các số chưa sử dụng vào các cột. Điều này nhanh chóng thất bại vì các ràng buộc cột ghép tất cả các hàng lại với nhau. Ví dụ: nếu hàng đầu tiên là$[1,2,3,4]$và chúng tôi cố gắng xây dựng hàng hai một cách độc lập bằng cách dịch chuyển các lựa chọn cục bộ mà không có mẫu chung, chúng tôi có thể vô tình lặp lại một số trong một cột, phá vỡ thuộc tính Latinh. 

Một trường hợp thất bại khác xuất hiện nếu chúng ta thử xáo trộn ngẫu nhiên mỗi hàng. Ngay cả khi mỗi hàng là một hoán vị hợp lệ thì không có gì đảm bảo tính duy nhất của cột. Ví dụ: 

đầu vào:```
3
1 2 3
```Nếu chúng ta tạo các hàng như sau một cách độc lập:```
1 2 3
2 1 3
3 2 1
```Cột 2 trở thành$[2,1,2]$, lặp lại 2 và vi phạm điều kiện. Vấn đề là cấu trúc cột yêu cầu căn chỉnh xác định giữa các hàng. 

Vì vậy, vấn đề không phải là điền số mà là vấn đề mở rộng hoán vị thành một đối tượng có cấu trúc với hành vi tuần hoàn nhất quán. 

## Phương pháp tiếp cận 

Một quan điểm mạnh mẽ sẽ là xây dựng lưới theo từng hàng, thử tất cả các hoán vị cho mỗi hàng trong khi xác thực các ràng buộc cột tăng dần. Đối với mỗi$n-1$các hàng còn lại có$n!$khả năng, và mỗi kiểm tra yêu cầu$O(n)$xác nhận cột. Điều này bùng nổ đến mức gần như$(n!)^n$, điều này hoàn toàn không khả thi ngay cả đối với những$n$. 

Quan sát quan trọng là hàng đầu tiên đã xác định thứ tự hoàn chỉnh của các ký hiệu và chúng ta có thể sử dụng lại thứ tự này một cách nhất quán trên tất cả các hàng. Nếu chúng ta coi hàng đầu tiên là một chuỗi tham chiếu thì mọi hàng tiếp theo có thể được hình thành bằng sự dịch chuyển theo chu kỳ của chuỗi này. 

Cụ thể hơn, nếu hàng đầu tiên là$a_0, a_1, \dots, a_{n-1}$, sau đó chèo$i$có được bằng cách dịch chuyển hàng đầu tiên bằng cách$i$các vị trí bên trái. Điều này duy trì tính hợp lệ của hàng vì mỗi hàng chỉ là một hoán vị của hàng đầu tiên. Nó cũng duy trì tính hợp lệ của cột vì mỗi cột trở thành hoán vị tuần hoàn của cùng một chuỗi, đảm bảo tất cả các giá trị xuất hiện chính xác một lần trên mỗi cột. 

Điều này làm giảm vấn đề từ tìm kiếm tổ hợp đến xây dựng xác định trong thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((n!)^n)$|$O(n^2)$| Quá chậm | 
| Chuyển đổi theo chu kỳ |$O(n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$n$và mảng hàng đầu tiên$a$. Hàng này xác định thứ tự chính xác của các số mà chúng ta phải giữ nguyên trong toàn bộ lưới. 
2. Dựng từng hàng$i$bằng cách dịch mảng sang trái$i$các vị trí. Phần tử di chuyển ra từ phía trước bao quanh đến cuối. Điều này đảm bảo mỗi hàng vẫn là hoán vị của hàng đầu tiên. 
3. In trực tiếp tất cả các hàng được xây dựng. 

Lý do dịch chuyển là phép biến đổi chính xác là vì nó duy trì trật tự tương đối trong khi xoay các vị trí, đó chính xác là điều cần thiết để tránh sự lặp lại trong các cột. 

### Tại sao nó hoạt động 

Mỗi hàng là một hoán vị của hàng đầu tiên, do đó các ràng buộc về hàng sẽ tự động được đáp ứng. Đối với các cột, hãy sửa bất kỳ chỉ mục cột nào$j$. Trên các hàng, giá trị trong cột đó là$a[(i+j) \bmod n]$. BẰNG$i$chạy từ 0 đến$n-1$, biểu thức này tuần hoàn qua tất cả các chỉ số theo modulo$n$chính xác một lần, nghĩa là mỗi cột chứa mọi giá trị đúng một lần. Bất biến này đảm bảo đồng thời cả điều kiện hàng và cột. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
a = list(map(int, input().split()))

for i in range(n):
    row = []
    for j in range(n):
        row.append(a[(i + j) % n])
    print(*row)
```Việc xây dựng cốt lõi xảy ra trong vòng lặp kép. Đối với mỗi chỉ mục hàng$i$, chúng tôi tạo ra sự dịch chuyển theo chu kỳ của mảng ban đầu. biểu hiện`(i + j) % n`là cơ chế thực thi hành vi bao quanh. 

Một điểm tinh tế là chúng tôi không bao giờ sửa đổi mảng ban đầu. Mỗi hàng được lấy trực tiếp từ tham chiếu cố định, điều này tránh được các lỗi tích lũy có thể xảy ra nếu các thay đổi được áp dụng lặp đi lặp lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 2 3 4
```Chúng tôi xây dựng các hàng dưới dạng dịch chuyển theo chu kỳ: 

| tôi | j=0 | j=1 | j=2 | j=3 | Hàng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | 3 | 4 | 1 2 3 4 | 
| 1 | 2 | 3 | 4 | 1 | 2 3 4 1 | 
| 2 | 3 | 4 | 1 | 2 | 3 4 1 2 | 
| 3 | 4 | 1 | 2 | 3 | 4 1 2 3 | 

Điều này xác nhận cả hoán vị hàng và tính duy nhất của cột. 

### Ví dụ 2 

đầu vào:```
3
2 1 3
```| tôi | j=0 | j=1 | j=2 | Hàng | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 1 | 3 | 2 1 3 | 
| 1 | 1 | 3 | 2 | 1 3 2 | 
| 2 | 3 | 2 | 1 | 3 2 1 | 

Mỗi cột lại duyệt qua tất cả các giá trị chính xác một lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Chúng tôi tính toán$n$hàng, mỗi hàng có$n$yếu tố | 
| Không gian |$O(1)$thêm | Ngoài bộ nhớ đầu vào, chúng tôi chỉ sử dụng lại cùng một mảng | 

Các ràng buộc cho phép lên đến$n = 100$, vậy nhiều nhất$10^4$các hoạt động không đáng kể trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    n = int(input().strip())
    a = list(map(int, input().split()))
    for i in range(n):
        row = []
        for j in range(n):
            row.append(a[(i + j) % n])
        print(*row)

    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided sample
assert run("4\n1 2 3 4\n") == "1 2 3 4\n2 3 4 1\n4 1 2 3\n3 4 1 2"

# custom 1: minimum size
assert run("1\n1\n") == "1"

# custom 2: non-trivial permutation
assert run("3\n2 1 3\n") == "2 1 3\n1 3 2\n3 2 1"

# custom 3: reverse order
assert run("4\n4 3 2 1\n") == "4 3 2 1\n3 2 1 4\n2 1 4 3\n1 4 3 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1×1 | 1 | trường hợp cơ sở đúng đắn | 
| hoán vị (2 1 3) | cấu trúc tuần hoàn | xử lý hoán vị không tầm thường | 
| thứ tự đảo ngược | hành vi chu kỳ đầy đủ | sự đúng đắn bao quanh | 

## Vỏ cạnh 

cho$n = 1$, lưới chứa một ô duy nhất và không xảy ra dịch chuyển. Thuật toán tạo ra chính xác hàng đầu vào đã hợp lệ. 

Đối với hàng đầu tiên đảo ngược như$[4,3,2,1]$, thao tác dịch chuyển vẫn duy trì tính chính xác vì việc lập chỉ mục mô-đun không phụ thuộc vào thứ tự số mà chỉ phụ thuộc vào vị trí xoay. Mỗi cột vẫn quay vòng qua tất cả các giá trị một lần, vì mỗi lần thay đổi chỉ mục được sử dụng chính xác một lần trên các hàng.
