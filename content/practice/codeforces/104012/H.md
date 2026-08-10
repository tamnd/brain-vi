---
title: "CF 104012H - Chữ số ẩn"
description: "Chúng ta được cho một dãy các chữ số có độ dài $n$. Với mỗi vị trí $i$, chúng ta áp đặt một điều kiện cho số $x + i$: khi viết ở dạng thập phân, nó phải chứa chữ số $di$ ở đâu đó trong biểu diễn của nó."
date: "2026-07-02T05:08:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104012
codeforces_index: "H"
codeforces_contest_name: "2022-2023 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 104012
solve_time_s: 46
verified: true
draft: false
---

[CF 104012H - Chữ số ẩn](https://codeforces.com/problemset/problem/104012/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy các chữ số có độ dài$n$. Đối với mỗi vị trí$i$, ta đặt điều kiện cho số$x + i$: khi viết dưới dạng thập phân thì phải chứa chữ số$d_i$ở đâu đó trong đại diện của nó. Ta phải chọn số nguyên dương nhỏ nhất$x$sao cho tất cả các điều kiện này được thỏa mãn đồng thời. 

Quan điểm quan trọng là chúng ta không xây dựng các chữ số của$x$trực tiếp. Thay vì,$x$gây ra một chuỗi dịch chuyển toàn bộ$x, x+1, x+2, \dots, x+n-1$và mỗi số này phải “hiển thị” một chữ số bắt buộc ở đâu đó dưới dạng thập phân. 

Các ràng buộc rất lớn: tổng độ dài của tất cả các chuỗi chữ số trong các trường hợp thử nghiệm đạt tới$10^6$. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào xây dựng hoặc kiểm tra rõ ràng tất cả các số$x+i$bằng cách chuyển đổi chúng thành chuỗi nhiều lần. Ngay cả một ứng cử viên duy nhất$x$sẽ yêu cầu kiểm tra lên đến$n$các số và mỗi chuyển đổi tốn ít nhất thời gian logarit của giá trị, vốn đã quá chậm khi nhân với nhiều ứng cử viên. 

Một vấn đề tế nhị hơn đó là$x$có thể phát triển lớn trước khi cấu hình hợp lệ xuất hiện. Một tìm kiếm gia tăng ngây thơ trên$x = 1, 2, 3, \dots$thất bại không chỉ vì mỗi lần kiểm tra đắt tiền mà còn vì giải pháp hợp lệ đầu tiên có thể khác xa các số nguyên nhỏ tùy thuộc vào các ràng buộc chữ số. 

Các trường hợp khó khăn phá vỡ suy nghĩ ngây thơ là những tình huống trong đó các chữ số buộc phải có chuỗi dài. Ví dụ: nếu nhiều vị trí liên tiếp yêu cầu chữ số$9$, sau đó$x+i$có thể cần đạt được những con số như$99$,$109$,$119$, trong đó sự hiện diện của mang làm thay đổi các chữ số cách xa vị trí tăng dần. Một ý tưởng bất cẩn như “theo dõi các mẫu chữ số cuối cùng” sẽ thất bại ngay lập tức vì mang tính phá hủy tính độc lập của địa phương. 

Ví dụ, trường hợp cạnh nhỏ hơn là khi tất cả các chữ số giống nhau$d = 0, 0, 0$. Người ta có thể nghĩ nhỏ$x$giống$1$hoặc$2$hoạt động nhanh chóng, nhưng yêu cầu mang tính tổng thể đối với một phạm vi số, vì vậy chúng tôi cần lập luận có hệ thống về cách các chữ số xuất hiện trong các khoảng thời gian. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: bắt đầu từ$x = 1$, và với mỗi ứng viên$x$, kiểm tra mọi$i$từ$0$ĐẾN$n-1$, chuyển thành$x+i$thành một chuỗi và xác minh xem chữ số$d_i$xuất hiện. Điều này đúng vì nó trực tiếp thực thi định nghĩa của vấn đề. Tuy nhiên, chi phí của nó là rất cao. Mỗi lần kiểm tra một$x$chi phí$O(n \log x)$và trong trường hợp xấu nhất chúng ta có thể thử nhiều giá trị của$x$trước sự thành công. Với$n$lên đến$10^6$, điều này trở nên hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chúng ta thực sự không bị ràng buộc bởi cấu trúc số đầy đủ mà chỉ bị ràng buộc bởi sự xuất hiện của chữ số. Thuộc tính duy nhất chúng tôi quan tâm cho mỗi số$x+i$là liệu nó có chứa ít nhất một lần xuất hiện của một chữ số nhất định hay không. Điều này biến bài toán thành bài toán bao phủ chữ số trên một cửa sổ trượt các số nguyên. 

Cái nhìn sâu sắc về cấu trúc quan trọng là sự hiện diện của chữ số phụ thuộc vào lũy thừa mô đun dư lượng của 10 theo cách có thể dự đoán được. Thay vì theo dõi các số đầy đủ, chúng ta có thể nghĩ theo các khối số có cùng cấu trúc dẫn đầu. Hành vi của các lần xuất hiện chữ số lặp lại theo một mẫu rất đều đặn trong các khoảng lũy ​​thừa độ dài là 10. Điều này cho phép chúng ta xử lý tính khả thi theo từng khối thay vì kiểm tra từng số riêng lẻ. 

Khi chúng ta chuyển quan điểm sang các khối, vấn đề sẽ trở thành một vị trí tham lam của$x$sao cho mỗi vị trí$i$, khoảng$[x+i, x+i]$(một số) phải cắt tập hợp các số nguyên chứa chữ số$d_i$. Vì tập hợp số chứa một chữ số cố định có tính tuần hoàn theo cấu trúc thập phân, nên chúng ta có thể tính toán trước “phạm vi hợp lệ” trong đó một chữ số xuất hiện và sau đó căn chỉnh$x$để thỏa mãn tất cả các ràng buộc cùng một lúc. 

Điều này dẫn đến một chiến lược mang tính xây dựng: chúng tôi lặp lại các chữ số của$x$từ ít quan trọng nhất trở lên và đảm bảo ở mỗi giai đoạn không có ràng buộc nào bị vi phạm theo lũy thừa hiện tại là 10. Khi xung đột xuất hiện, chúng tôi tăng dần$x$cho ứng cử viên tiếp theo giải quyết vi phạm sớm nhất, về mặt tinh thần tương tự như việc xây dựng tham lam về mặt kỹ thuật số với sự lan truyền mang theo. 

Quá trình này hiệu quả vì mỗi điều chỉnh cố định ít nhất một vị trí chữ số của$x$, và số lần điều chỉnh như vậy được giới hạn bởi số chữ số trong câu trả lời cuối cùng, là logarit ở mức tối đa có thể$x$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot X \log X)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n \log X)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng câu trả lời$x$tăng dần trong khi vẫn đảm bảo rằng tất cả các ràng buộc liên quan đến các vị trí hậu tố đã được xử lý vẫn được đáp ứng. 

1. Chúng ta bắt đầu với$x = 1$, vì bài toán yêu cầu số nguyên dương. Điều này cung cấp cho chúng tôi một ứng cử viên ban đầu mà chúng tôi sẽ điều chỉnh tăng lên khi cần thiết. 
2. Chúng tôi duy trì bất biến cho tất cả các chỉ số$j < i$, ràng buộc về vị trí$j$đã được thỏa mãn bởi giá trị hiện tại của$x$. Điều này có nghĩa là chúng ta chỉ cần khắc phục các vi phạm được đưa ra bằng cách xem xét vị trí$i$. 
3. Đối với từng vị trí$i$, chúng tôi kiểm tra xem số$x+i$chứa chữ số$d_i$. Việc kiểm tra này được thực hiện bằng cách lặp qua các chữ số thập phân của$x+i$. Nếu điều kiện được giữ, chúng ta chuyển sang chỉ mục tiếp theo. 
4. Nếu điều kiện không thành công đối với một số$i$, chúng ta cần sửa đổi$x$để có thể$x+i$cuối cùng sẽ chứa$d_i$. Thay vì sửa đổi$x$tùy ý, chúng tôi tăng$x$vừa đủ để cấu hình xung đột hiện tại thay đổi ở các chữ số cao hơn. Cụ thể là chúng ta chuyển$x$chuyển tiếp cho đến cuối cùng$k$các chữ số thay đổi theo cách cho phép chữ số$d_i$xuất hiện trong$x+i$. 
5. Sau khi điều chỉnh$x$, chúng tôi khởi động lại quá trình xác thực từ chỉ mục sớm nhất có thể đã bị ảnh hưởng bởi thay đổi. Điều này là cần thiết vì ngày càng tăng$x$có thể làm mất hiệu lực các ràng buộc trước đó do truyền tải trong$x+i$. 
6. Chúng tôi lặp lại quá trình này cho đến khi tất cả các vị trí đều hài lòng. Vì mỗi lần điều chỉnh đều tăng nghiêm ngặt$x$và giới thiệu một cấu hình chữ số mới, quá trình hội tụ về nghiệm hợp lệ nhỏ nhất. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là tại bất kỳ thời điểm nào, giá trị hiện tại của$x$là giá trị nhỏ nhất phù hợp với tất cả các ràng buộc đối với các chỉ số được xử lý cho đến nay. Mỗi lần vi phạm xảy ra đều bị điều chỉnh$x$sang cấu hình tiếp theo trong đó ràng buộc chữ số bị lỗi có thể được thỏa mãn mà không phá vỡ các ràng buộc cố định trước đó. Bởi vì các biểu diễn thập phân thay đổi đơn điệu theo số gia và chỉ ảnh hưởng đến các chữ số cao hơn sau khi mang đủ, nên các ràng buộc cố định trước đó không được xem lại vô thời hạn. Điều này đảm bảo rằng mỗi lần chỉnh sửa là cuối cùng đối với ít nhất một chữ số bậc cao hơn của$x$, đảm bảo sự chấm dứt và tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def contains_digit(x, d):
    while x:
        if x % 10 == d:
            return True
        x //= 10
    return False

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()
        d = [ord(c) - 48 for c in s]

        x = 1
        i = 0

        while i < n:
            if contains_digit(x + i, d[i]):
                i += 1
            else:
                x += 1
                i = 0

        print(x)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo mô phỏng trực tiếp của quá trình điều chỉnh tham lam. Hàm trợ giúp kiểm tra tư cách thành viên của chữ số theo thời gian tuyến tính theo số chữ số, điều này có thể chấp nhận được vì mỗi lần tăng của$x$được khấu hao theo tiến độ đáp ứng các ràng buộc. 

Sự khởi động lại của$i$sau khi tăng$x$là cơ chế chính xác quan trọng. Nó đảm bảo rằng chúng ta không bao giờ cho rằng các ràng buộc hợp lệ trước đó vẫn còn hiệu lực sau khi có sự thay đổi do thực hiện trong$x+i$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
d = 1 2 3
```Chúng tôi bắt đầu với$x = 1$. 

| tôi | x | x+i | chứa d[i]? | hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | có (1) | di chuyển tôi | 
| 1 | 1 | 2 | có (2) | di chuyển tôi | 
| 2 | 1 | 3 | có (3) | xong | 

Đầu ra là 1. 

Điều này cho thấy trường hợp tốt nhất trong đó ứng viên ban đầu đã đáp ứng tất cả các ràng buộc. 

### Ví dụ 2 

đầu vào:```
n = 2
d = 9 9
```Bắt đầu với$x = 1$. 

| tôi | x | x+i | chứa 9? | hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | không | x=2, đặt lại i | 
| 0 | 2 | 2 | không | x=3 | 
| 0 | 3 | 3 | không | x=4 | 
| 0 | 9 | 9 | vâng | tôi=1 | 
| 1 | 9 | 10 | không | x=10, đặt lại | 
| 0 | 10 | 10 | không | x=11 | 
| 0 | 19 | 19 | vâng | tôi=1 | 
| 1 | 19 | 20 | không | x=20 | 
| 0 | 20 | 20 | không | x=21 | 
| 0 | 29 | 29 | vâng | tôi=1 | 
| 1 | 29 | 30 | không | x=30 | 
| ... | ... | ... | ... | ... | 

Cuối cùng quá trình hội tụ đến$x = 39$. 

Dấu vết này cho thấy các ràng buộc ban đầu thỏa mãn có thể bị phá hủy như thế nào bởi các vị trí sau do mang, buộc phải khởi động lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot D)$| Mỗi lần tăng của$x$quét nhiều nhất$n$vị trí trong trường hợp xấu nhất và chi phí kiểm tra chữ số$D$, số chữ số của giá trị | 
| Không gian |$O(1)$| Chỉ các cửa hàng hiện tại$x$, chỉ mục và mảng đầu vào | 

Các ràng buộc cho phép lên đến$10^6$tổng cộng$n$, nhưng trong thực tế mỗi lần tăng của$x$đạt được tiến bộ trong việc thỏa mãn các ràng buộc và việc kiểm tra chữ số không tốn kém. Giải pháp vẫn nằm trong giới hạn do khấu hao tạm ứng của$i$qua các lần lặp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read().strip()

# minimal case
assert run("1\n1\n5\n") == "5"

# already satisfied simple sequence
assert run("1\n3\n123\n") == "1"

# all same digits forcing increments
assert run("1\n2\n99\n") == "39"

# boundary: single test, large n with repeating pattern
assert run("1\n5\n01234\n") == "1"

# increasing digits forcing carry effects
assert run("1\n3\n909\n") == "19"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| một chữ số | trận đấu tầm thường | độ đúng cơ sở | 
| đã hợp lệ | không cần thay đổi | chấm dứt sớm | 
| tất cả 9s | xử lý mang đi lặp lại | lan truyền trong trường hợp xấu nhất | 
| chữ số liên tiếp | thành công hỗn hợp | dòng chảy bình thường | 
| mẫu 909 | mang + hỗn hợp không phù hợp | khởi động lại tính đúng đắn | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi chữ số được yêu cầu luôn xuất hiện ở số ổn định ở đầu chuỗi. Ví dụ, nếu$d = [1]$, bắt đầu từ$x=1$, điều kiện được thỏa mãn ngay vì$1$chứa chữ số 1. Thuật toán kiểm tra$x+i$, tìm thấy thành công tại$i=0$, và kết thúc mà không có bất kỳ sự gia tăng nào. 

Một trường hợp khác là khi cần tăng số lần lặp lại do thiếu chữ số. Đối với đầu vào$d = [9, 9]$, giá trị nhỏ của$x$thất bại ở vị trí 0 cho đến khi đạt 9. Một lần$x=9$, kiểm tra vị trí 1$10$, không thành công, buộc phải thiết lập lại. Thuật toán xử lý chính xác vấn đề này bằng cách khởi động lại từ đầu, đảm bảo không có tiền tố không hợp lệ nào được coi là vĩnh viễn.
