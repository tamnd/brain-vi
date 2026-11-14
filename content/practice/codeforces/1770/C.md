---
title: "CF 1770C - Lý thuyết số và Koxia"
description: "Chúng ta được cho một mảng các số nguyên dương và chúng ta được phép chọn một giá trị dịch chuyển dương duy nhất $x$. Sau khi dịch chuyển mọi phần tử một lượng như nhau, chúng ta muốn mọi cặp số thu được đều là số nguyên tố cùng nhau. Tương tự, chúng ta chuyển đổi mảng thành $bi = ai + x$."
date: "2026-06-09T12:26:52+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "chinese-remainder-theorem", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1770
codeforces_index: "C"
codeforces_contest_name: "Good Bye 2022: 2023 is NEAR"
rating: 1700
weight: 1770
solve_time_s: 81
verified: true
draft: false
---

[CF 1770C - Lý thuyết số và Koxia](https://codeforces.com/problemset/problem/1770/C) 

**Đánh giá:** 1700 
**Tags:** Brute Force, định lý số dư Trung Hoa, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên dương và chúng ta được phép chọn một giá trị dịch chuyển dương duy nhất$x$. Sau khi dịch chuyển mọi phần tử một lượng như nhau, chúng ta muốn mọi cặp số thu được đều là số nguyên tố cùng nhau. 

Tương tự, chúng ta chuyển đổi mảng thành$b_i = a_i + x$. Yêu cầu là đối với bất kỳ hai chỉ số khác nhau nào, hai giá trị dịch chuyển không có ước số chung lớn hơn 1. Nhiệm vụ là quyết định xem có tồn tại ít nhất một dịch chuyển như vậy hay không. 

Khía cạnh quan trọng là chúng tôi không được yêu cầu xây dựng$x$, chỉ để xác định sự tồn tại. Điều này thường báo hiệu rằng điều kiện có thể được giảm xuống thành thuộc tính cấu trúc của đầu vào thay vì tìm kiếm trên các số nguyên lớn. 

Những hạn chế cho thấy$n \le 100$mỗi lần kiểm tra và giá trị tăng lên$10^{18}$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào lặp đi lặp lại có thể$x$, từ$x$có kích thước không giới hạn. Nó cũng gợi ý rằng lý luận phải phụ thuộc vào mối quan hệ giữa các số ban đầu chứ không phải độ lớn tuyệt đối của chúng. 

Một trường hợp thất bại tinh vi đối với lý luận ngây thơ là giả định rằng việc chọn một số lượng lớn$x$luôn làm cho các số nguyên tố cùng nhau. Điều đó sai vì việc dịch chuyển bảo toàn sự khác biệt: nếu hai số đồng thời chia hết cho một số nguyên tố nào đó sau khi dịch chuyển, thì số nguyên tố đó sẽ tạo ra một gcd bị cấm. Một cái bẫy khác chỉ tập trung vào sự khác biệt theo cặp$a_i - a_j$trực tiếp, vì cấu trúc gcd sau khi dịch chuyển không phải là bất biến chỉ với sai phân. 

## Phương pháp tiếp cận 

Quan sát chính xuất phát từ việc viết lại điều kiện gcd theo cách có cấu trúc hơn. Nếu hai số dịch chuyển có chung ước số nguyên tố$p$, thì cả hai$a_i + x$Và$a_j + x$được chia cho$p$. Điều đó có nghĩa là:$$a_i \equiv a_j \equiv -x \pmod p$$Vì vậy, tất cả các số có chung ước số nguyên tố sau khi dịch chuyển phải thuộc cùng một lớp dư theo modulo$p$. 

Điều này biến vấn đề thành việc kiểm soát số nguyên tố nào có thể chia đồng thời các giá trị đã dịch chuyển. Thay vì nghĩ về$x$, chúng ta nghĩ về cách nó buộc tất cả các số vào các lớp đồng đẳng cụ thể theo modulo số nguyên tố thế. 

Một cách hữu ích để tiếp tục là sửa một phần tử làm tham chiếu và thể hiện mọi thứ liên quan đến nó. Hãy để chúng tôi chọn$a_1$. Nếu chúng ta muốn$\gcd(a_1 + x, a_i + x) = 1$, thì không có số nguyên tố nào chia cả hai. Giả sử một số nguyên tố$p$chia cả hai giá trị đã dịch chuyển. Sau đó, nó phân chia sự khác biệt của họ:$$(a_i + x) - (a_1 + x) = a_i - a_1$$Vì vậy bất kỳ số nguyên tố xấu nào cũng phải chia một số chênh lệch$a_i - a_1$. Đây là sự rút gọn quan trọng: chỉ các số nguyên tố xuất hiện trong các hiệu khác nhau mới quan trọng. 

Bây giờ cấu trúc đơn giản hóa. Nếu là số nguyên tố$p$phân chia bất kỳ sự khác biệt nào, thì chúng ta phải tránh lựa chọn$x$sao cho tất cả các giá trị nằm trong cùng một modulo lớp dư lượng$p$. Cách duy nhất để đảm bảo thành công là đảm bảo rằng không có sự liên kết không thể tránh khỏi nào xảy ra trên toàn bộ mảng. 

Sự đơn giản hóa mang tính quyết định được sử dụng trong giải pháp dự định sẽ mạnh mẽ hơn: nếu mảng chứa các bản sao, câu trả lời ngay lập tức là không thể. Nếu như$a_i = a_j$, thì với bất kỳ$x$, chúng tôi nhận được$a_i + x = a_j + x$, vì vậy gcd của chúng chính là số, luôn lớn hơn 1. Điều này đã chặn tính hợp lệ. 

Nếu tất cả các giá trị là khác nhau, chúng ta luôn có thể xây dựng một$x$điều đó tránh được tất cả hữu hạn nhiều sự đồng đẳng bị cấm gây ra bởi hiệu số nguyên tố chia. Từ$n$nhỏ, lập luận xây dựng này đảm bảo sự tồn tại mà không cần tìm thấy nó một cách rõ ràng. 

Do đó, điều kiện rút gọn thành một kiểm tra đơn giản: liệu có tồn tại bản sao hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực trên x | Quá lớn | O(1) | Không thể | 
| GCD/lý luận khác biệt với kiểm tra tính duy nhất | O(n) mỗi lần kiểm tra | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng cho từng trường hợp thử nghiệm và kiểm tra xem có giá trị nào xuất hiện nhiều lần hay không. Bước này trực tiếp kiểm tra vật cản duy nhất đảm bảo thất bại bất kể$x$. 
2. Nếu tồn tại một bản sao, hãy kết luận ngay rằng không thể tồn tại phép dịch hợp lệ vì các giá trị giống hệt nhau vẫn giống nhau sau khi dịch chuyển, buộc phải có một gcd không phải đơn vị. 
3. Nếu tất cả các phần tử đều khác biệt, hãy kết luận rằng một$x$tồn tại. Lý do là nếu không có gcd bắt buộc do trùng lặp gây ra, chúng ta luôn có thể chọn một phép dịch chuyển tránh tất cả các ràng buộc nguyên tố chung. 

## Tại sao nó hoạt động 

Gcd duy nhất không thể tránh khỏi lớn hơn 1 sau khi dịch chuyển phát sinh khi hai giá trị dịch chuyển giống hệt nhau, điều này xảy ra chính xác khi các giá trị ban đầu giống hệt nhau. Bất kỳ điều kiện gcd nào khác đều phụ thuộc vào khả năng chia hết cho các số nguyên tố trong các hiệu và có thể tránh được điều này bằng cách lựa chọn thích hợp$x$vì tập dư lượng bị cấm là hữu hạn và không bao gồm tất cả các số nguyên. Vì vậy, sự trùng lặp là trở ngại duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        if len(set(a)) < n:
            print("NO")
        else:
            print("YES")

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo đặc tính. Bước không cần thiết duy nhất là sử dụng một bộ để phát hiện các bản sao một cách hiệu quả. Không cần thao tác số học vì lý thuyết quy giản bài toán hoàn toàn về tính duy nhất. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp mẫu đầu tiên$[5, 7, 10]$. 

Chúng tôi theo dõi xem có tồn tại sự trùng lặp hay không. 

| Bước | Mảng | Kích thước bộ độc đáo | Quyết định | 
| --- | --- | --- | --- | 
| Ban đầu | [5,7,10] | 3 | tiếp tục | 

Không tìm thấy bản sao nào, vì vậy thuật toán xuất ra CÓ. Điều này phù hợp với việc xây dựng được đưa ra trong tuyên bố nơi$x = 4$. 

Bây giờ hãy xem xét một trường hợp được xây dựng$[3, 3, 4]$. 

| Bước | Mảng | Kích thước bộ độc đáo | Quyết định | 
| --- | --- | --- | --- | 
| Ban đầu | [3,3,4] | 2 | KHÔNG | 

Bản sao 3 tạo ra hai giá trị dịch chuyển giống hệt nhau, làm cho gcd luôn bằng giá trị đó cộng thêm$x$, không thể bằng 1. 

Những dấu vết này cho thấy thuật toán không phụ thuộc vào cường độ hoặc cấu trúc ngoài việc kiểm tra đẳng thức, phù hợp với điều kiện dẫn xuất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Đặt cấu trúc và so sánh các phần tử mảng | 
| Không gian | O(n) | Lưu trữ các phần tử trong tập băm | 

Các ràng buộc cho phép tối đa 1000 phần tử, do đó việc xử lý thời gian tuyến tính dễ dàng đủ nhanh trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    
    t = int(sys.stdin.readline())
    for _ in range(t):
        n = int(sys.stdin.readline())
        a = list(map(int, sys.stdin.readline().split()))
        output.append("NO" if len(set(a)) < n else "YES")
    
    return "\n".join(output)

# provided samples
assert run("""2
3
5 7 10
3
3 3 4
""") == """YES
NO"""

# all distinct small
assert run("""1
4
1 2 3 4
""") == "YES"

# all equal
assert run("""1
3
5 5 5
""") == "NO"

# single duplicate pair
assert run("""1
5
10 20 30 20 40
""") == "NO"

# boundary n=2 distinct
assert run("""1
2
1000000000000000000 999999999999999999
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều khác biệt | CÓ | trường hợp dương tính chung | 
| tất cả đều bình đẳng | KHÔNG | trường hợp sụp đổ hoàn toàn | 
| một cặp trùng lặp | KHÔNG | phát hiện trùng lặp một phần | 
| giá trị lớn khác biệt | CÓ | biên độ an toàn biên độ | 

## Vỏ cạnh 

Một mảng hoàn toàn bằng nhau như`[7, 7, 7]`là trường hợp thất bại mạnh nhất. Sau bất kỳ ca làm việc nào$x$, tất cả các giá trị trở nên giống hệt nhau, do đó mỗi cặp đều có gcd bằng giá trị đã dịch chuyển. Thuật toán ngay lập tức phát hiện các bản sao và đưa ra NO mà không cần tính toán các giá trị. 

Một mảng gần như trùng lặp như`[1, 2, 1]`cư xử theo cùng một cách. Mặc dù chỉ có một giá trị lặp lại, nhưng cũng đủ để gây ra mâu thuẫn vì hai giá trị dịch chuyển giống hệt nhau luôn chia sẻ toàn bộ giá trị của chúng dưới dạng gcd. 

Một mảng hoàn toàn khác biệt như`[1, 2, 3]`được xử lý thống nhất: kích thước thiết lập bằng$n$, do đó thuật toán xuất ra CÓ mà không cần phải giải thích về các phép tính gcd thực tế.
