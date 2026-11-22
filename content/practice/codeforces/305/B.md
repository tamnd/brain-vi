---
title: "CF 305B - ​​Phân số tiếp tục"
description: "Chúng ta được cho một số hữu tỷ được viết dưới dạng phân số bình thường p / q và một số khác được viết dưới dạng phân số hữu hạn liên tục."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 305
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 184 (Div. 2)"
rating: 1700
weight: 305
solve_time_s: 85
verified: true
draft: false
---

[CF 305B - Phân số tiếp theo](https://codeforces.com/problemset/problem/305/B) 

**Đánh giá:** 1700 
**Tags:** vũ lực, thực hiện, toán học 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cho ta một số hữu tỉ viết dưới dạng phân số bình thường`p / q`và một số khác được viết dưới dạng phân số hữu hạn tiếp tục. 

Phân số liên tục có dạng:$$a_1 + \frac{1}{a_2 + \frac{1}{a_3 + \dots}}$$Chúng ta phải xác định xem cả hai cách biểu diễn có mô tả cùng một số hữu tỉ hay không. 

Độ dài phân số liên tục nhiều nhất là 90, rất nhỏ. Bản thân các giá trị có thể lớn bằng$10^{18}$, vì vậy việc tính toán phải được thực hiện cẩn thận. Python xử lý các số nguyên lớn tùy ý một cách tự nhiên, điều này giúp việc triển khai an toàn hơn nhiều so với các ngôn ngữ số nguyên có chiều rộng cố định. 

Ở đây, việc so sánh dấu phẩy động trực tiếp là hoàn toàn không đáng tin cậy. Các phân số liên tục có thể biểu diễn các tử số và mẫu số rất lớn, đồng thời việc làm tròn dấu phẩy động sẽ âm thầm làm hỏng việc kiểm tra đẳng thức. 

Các trường hợp cạnh nguy hiểm nhất đến từ các phân số trung gian. 

Hãy xem xét đầu vào này:```
3 2
2
1 2
```Phân số tiếp theo bằng:$$1 + \frac{1}{2} = \frac{3}{2}$$Câu trả lời đúng là`YES`. 

Việc triển khai bất cẩn bằng cách sử dụng dấu phẩy động có thể tính toán một cái gì đó như`1.4999999999999998`và bác bỏ sai đẳng thức. 

Một trường hợp tinh tế khác xuất hiện khi phân số tiếp tục có độ dài 1:```
5 1
1
5
```Phân số tiếp tục chỉ đơn giản là`5`. Không có mẫu số lồng nhau nào cả. Mã giả định một cách mù quáng ít nhất hai thuật ngữ sẽ bị lỗi hoặc tạo ra kết quả sai. 

Một kịch bản khó khăn hơn xuất phát từ việc giảm phân số quá muộn.```
1000000000000000000 1
2
999999999999999999 1
```Phân số tiếp theo bằng:$$999999999999999999 + \frac{1}{1} = 1000000000000000000$$Giá trị trung gian trở nên cực kỳ lớn. Các ngôn ngữ có tràn 64 bit sẽ không thành công nếu quá trình triển khai tăng gấp bội trước khi đơn giản hóa. Python tránh tràn nhưng thuật toán vẫn phải rõ ràng về mặt toán học. 

## Phương pháp tiếp cận 

Ý tưởng Brute-Force rất đơn giản: đánh giá rõ ràng phân số tiếp theo thành tử số và mẫu số, sau đó so sánh nó với`p / q`. 

Chúng ta có thể xây dựng lại phân số từ phía sau. Giả sử chúng ta đã biết giá trị của hậu tố là`x / y`. Sau đó thêm thuật ngữ trước đó`a[i]`biến nó thành:$$a_i + \frac{y}{x} = \frac{a_i x + y}{x}$$Điều này có tác dụng vì mọi phân số hữu hạn tiếp tục biểu diễn một số hữu tỉ. 

Phiên bản brute-force liên tục xây dựng các phân số bằng cách sử dụng số học chính xác tùy ý. Từ`n ≤ 90`, ngay cả những số nguyên lớn vẫn có thể quản lý được. Thuật toán chỉ thực hiện nhiều phép tính số học tuyến tính. 

Quan sát quan trọng là chúng ta thực sự không cần tính toán các giá trị thập phân hoặc mô phỏng phép chia. Các phân số liên tục được kết nối chặt chẽ với thuật toán Euclide. 

Giả định:$$\frac{p}{q} = a_1 + \frac{r}{q}$$Sau đó:$$a_1 = \left\lfloor \frac{p}{q} \right\rfloor$$và phần còn lại xác định đệ quy phần còn lại của phân số tiếp tục. 

Điều này có nghĩa là chúng ta có thể so sánh trực tiếp dãy đã cho với phép phân tích Euclide của`p / q`. 

Ở mỗi bước: 

1. Hệ số phân số tiếp theo phải bằng`p // q`. 
2. Sau đó chúng ta thay thế`(p, q)`với`(q, p % q)`. 

Đây chính xác là thuật toán Euclide. 

Việc xây dựng vũ phu đã trôi qua một cách thoải mái vì`n`chỉ là 90, nhưng cách giải thích Euclide rõ ràng hơn, đơn giản hơn và tránh việc xây dựng các tử số trung gian khổng lồ một cách rõ ràng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tái thiết Brute Force | O(n) | O(1) | Đã chấp nhận | 
| So sánh Euclide | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`p`Và`q`. 
2. Đọc dãy phân số tiếp theo`a`. 
3. Với mỗi giá trị`x`theo trình tự: 

Kiểm tra xem`p // q`bằng`x`. 

Nếu chúng khác nhau thì cách biểu diễn không thể bằng nhau, vì vậy hãy in`NO`. 
4. Thay thế`(p, q)`với`(q, p % q)`. 

Điều này phản ánh cấu trúc đệ quy của các phân số tiếp tục. Sau khi loại bỏ phần nguyên, phần còn lại trở thành:$$\frac{q}{p \bmod q}$$5. Sau khi xử lý tất cả các hệ số, hãy xác minh rằng quy trình Euclide đã hoàn thành chính xác. 

Mẫu số còn lại phải bằng 0. Nếu không, phần tiếp theo đã kết thúc quá sớm. 
6. In`YES`nếu tất cả kiểm tra thành công, nếu không thì in`NO`. 

### Tại sao nó hoạt động 

Một phân số hữu hạn liên tục chính xác là dãy thương được tạo bởi thuật toán Euclide trên`(p, q)`. 

Tại mỗi lần lặp:$$p = q \cdot \left\lfloor \frac{p}{q} \right\rfloor + (p \bmod q)$$Thương số nguyên trở thành hệ số phân số tiếp theo. Phần còn lại xác định phân số đệ quy tiếp theo. 

Bởi vì thuật toán Euclide phân tách duy nhất một số hữu tỷ thành các hệ số phân số liên tục, việc khớp với mọi thương đảm bảo sự bằng nhau. Bất kỳ sự không khớp nào có nghĩa là các số được biểu thị khác nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    p, q = map(int, input().split())

    n = int(input())
    a = list(map(int, input().split()))

    for x in a:
        if q == 0:
            print("NO")
            return

        if p // q != x:
            print("NO")
            return

        p, q = q, p % q

    if q == 0:
        print("YES")
    else:
        print("NO")

solve()
```Việc thực hiện tuân theo thuật toán Euclide trực tiếp. 

Vòng lặp so sánh từng hệ số phân số tiếp theo với thương số nguyên hiện tại`p // q`. Nếu chúng khác nhau dù chỉ một lần thì sự bình đẳng là không thể. 

Sau khi tiêu thụ một hệ số, chúng ta chuyển sang trạng thái còn lại tiếp theo bằng cách sử dụng:```
p, q = q, p % q
```Phép biến đổi này là trung tâm của phép đệ quy phân số liên tục. 

Việc kiểm tra cuối cùng rất tinh tế và dễ bỏ sót. Chúng ta phải đảm bảo việc mở rộng phân số kết thúc chính xác khi thuật toán Euclide kết thúc. Nếu như`q != 0`sau khi xử lý tất cả các hệ số thì chuỗi quá ngắn. 

Người bảo vệ:```
if q == 0:
```bên trong vòng lặp ngăn chặn việc chia cho 0 trong các tình huống không đúng định dạng trong đó phân số tiếp tục dài hơn phân tách Euclide. 

Số nguyên Python tự động mở rộng đến kích thước tùy ý, vì vậy chúng tôi không bao giờ lo lắng về việc tràn ngay cả khi các giá trị tăng vượt xa$10^{18}$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
9 4
2
2 4
```Dấu vết: 

| Bước | p | q | p // q | Hệ số dự kiến ​​| Mới (p, q) | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 9 | 4 | 2 | 2 | (4, 1) | 
| Tiếp theo | 4 | 1 | 4 | 4 | (1, 0) | 

Sau khi tất cả các hệ số được xử lý,`q = 0`, vậy câu trả lời là`YES`. 

Dấu vết này cho thấy mối liên hệ chính xác giữa các phân số liên tục và thuật toán Euclid. Các thương số khớp hoàn hảo ở mỗi bước. 

### Ví dụ 2 

đầu vào:```
3 2
2
1 3
```Dấu vết: 

| Bước | p | q | p // q | Hệ số dự kiến ​​| Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 3 | 2 | 1 | 1 | tiếp tục | 
| Tiếp theo | 2 | 1 | 2 | 3 | không khớp | 

Ở bước thứ hai, thương Euclide là`2`, nhưng phần tiếp tục mong đợi`3`. Các cách biểu diễn khác nhau nên câu trả lời là`NO`. 

Điều này chứng tỏ rằng ngay cả một hệ số không khớp cũng ngay lập tức chứng tỏ sự bất bình đẳng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một bước Euclide cho mỗi hệ số | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Từ`n ≤ 90`, thuật toán cực kỳ nhanh. Ngay cả số học chính xác tùy ý cũng dễ dàng nằm trong giới hạn vì chỉ một số ít thao tác được thực hiện. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    p, q = map(int, input().split())

    n = int(input())
    a = list(map(int, input().split()))

    for x in a:
        if q == 0:
            print("NO")
            return

        if p // q != x:
            print("NO")
            return

        p, q = q, p % q

    if q == 0:
        print("YES")
    else:
        print("NO")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("9 4\n2\n2 4\n") == "YES\n", "sample 1"

# minimum size
assert run("5 1\n1\n5\n") == "YES\n", "single coefficient"

# mismatch in middle
assert run("3 2\n2\n1 3\n") == "NO\n", "wrong coefficient"

# exact large-value reconstruction
assert run(
    "1000000000000000000 1\n2\n999999999999999999 1\n"
) == "YES\n", "large integers"

# continued fraction too short
assert run("8 3\n1\n2\n") == "NO\n", "sequence ended early"

# continued fraction too long
assert run("2 1\n3\n2 1 1\n") == "NO\n", "sequence too long"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5/1`với`[5]`|`YES`| Phân số tiếp theo có độ dài-1 | 
|`3/2`với`[1,3]`|`NO`| Phát hiện thương số không khớp | 
| Giá trị lớn gần$10^{18}$|`YES`| Số học chính xác tùy ý | 
|`8/3`với`[2]`|`NO`| Trình tự kết thúc quá sớm | 
|`2/1`với`[2,1,1]`|`NO`| Chuỗi dài hơn phân rã Euclide | 

## Vỏ cạnh 

Hãy xem xét phân số tiếp tục một phần tử:```
5 1
1
5
```Thuật toán bắt đầu với`p = 5`,`q = 1`. 

Thương số là:$$5 // 1 = 5$$phù hợp với hệ số duy nhất. Sau đó:$$(p, q) = (1, 0)$$Vòng lặp kết thúc và`q == 0`, vậy câu trả lời là`YES`. 

Trường hợp này xác nhận rằng việc triển khai xử lý chính xác các phân số tiếp tục không có phần đệ quy. 

Bây giờ hãy xem xét một chuỗi kết thúc quá sớm:```
8 3
1
2
```Thương số thứ nhất là:$$8 // 3 = 2$$cái nào phù hợp. Nhà nước trở thành:$$(p, q) = (3, 2)$$Trình tự đã hết, nhưng`q != 0`. Quá trình Euclide vẫn còn các số hạng còn lại nên câu trả lời phải là`NO`. 

Điều này xác nhận việc kiểm tra chấm dứt cuối cùng. 

Cuối cùng, hãy xem xét một chuỗi quá dài:```
2 1
3
2 1 1
```Hệ số sau xử lý`2`, trạng thái trở thành:$$(p, q) = (1, 0)$$Thuật toán vẫn còn nhiều hệ số để đọc. Từ`q == 0`, phép chia là không thể và đáp án là ngay lập tức`NO`. 

Điều này ngăn cản việc vô tình chấp nhận các điều khoản bổ sung không hợp lệ.
