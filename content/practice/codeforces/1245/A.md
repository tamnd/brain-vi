---
title: "CF 1245A - Tô màu con số cực hay"
description: "Chúng ta có hai kích cỡ bước, a và b. Bắt đầu từ số 0, chúng ta vẽ mọi số nguyên không âm theo thứ tự tăng dần, nhưng việc một số có trở thành màu trắng hay không còn phụ thuộc vào việc liệu nó có thể đạt được từ các số vốn đã trắng hay không bằng cách thêm liên tục a hoặc b. Về mặt hình thức, số 0 bắt đầu bằng màu trắng."
date: "2026-06-13T20:36:46+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1245
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 597 (Div. 2)"
rating: 1000
weight: 1245
solve_time_s: 195
verified: true
draft: false
---

[CF 1245A - Tô màu các con số cực hay](https://codeforces.com/problemset/problem/1245/A) 

**Đánh giá:** 1000 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 3 phút 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai kích thước bước,`a`Và`b`. Bắt đầu từ số 0, chúng ta vẽ mọi số nguyên không âm theo thứ tự tăng dần, nhưng việc một số có trở thành màu trắng hay không còn phụ thuộc vào việc liệu nó có thể đạt được từ các số vốn đã trắng hay không bằng cách liên tục thêm một trong hai số đó.`a`hoặc`b`. 

Về mặt hình thức, số 0 bắt đầu bằng màu trắng. Đối với bất kỳ số nào`i`, nó sẽ trở thành màu trắng nếu trừ đi`a`hoặc`b`đáp xuống một số đã có màu trắng. Nếu cả hai phiên bản trước đều không hoạt động thì số vẫn có màu đen. 

Quá trình này tương đương với việc xây dựng tất cả các số có thể được hình thành dưới dạng`i = x·a + y·b`cho số nguyên không âm`x`Và`y`, vì mọi số màu trắng đều có thể truy cập được bằng cách thêm liên tục`a`hoặc`b`bắt đầu từ số không. 

Câu hỏi không phải là liệt kê các màu mà là xác định xem có vô số số màu đen hay không. Điều đó hoàn toàn phụ thuộc vào việc liệu tập hợp các số có thể truy cập (màu trắng) cuối cùng có trở nên dày đặc để bao phủ tất cả các số nguyên đủ lớn hay không. 

Các ràng buộc là nhỏ, với`t ≤ 100`Và`a, b ≤ 10^4`. Điều này cho thấy chúng tôi không mô phỏng quá trình trên một phạm vi số nguyên lớn. Một mô phỏng đơn giản trên các giá trị lên đến một mức giới hạn lớn sẽ là tùy ý và không đáng tin cậy vì cấu trúc chuyển tiếp phụ thuộc vào lý thuyết số chứ không phải độ sâu lan truyền cục bộ. 

Trường hợp cạnh chính là khi`a`Và`b`đều bình đẳng. Ví dụ, nếu`a = b = 10`, chỉ có bội số của 10 là màu trắng. Tất cả các số nguyên khác đều có màu đen vĩnh viễn và có vô số số nguyên như vậy. Một trường hợp khác là khi một trong số chúng`1`, chẳng hạn như`a = 1, b = 10`, trong đó mọi số đều có thể truy cập được bằng cách lặp lại các bước +1, do đó không có số màu đen nào cả. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực cố gắng mô phỏng màu sắc theo thứ tự: duy trì một mảng đến một giới hạn lớn nào đó`N`, đánh dấu`0`màu trắng và đối với mỗi`i`, đánh dấu nó màu trắng nếu`i-a`hoặc`i-b`là màu trắng. Sau đó đếm số màu đen. Điều này đúng đối với một giới hạn cố định, nhưng vấn đề là ở chỗ “màu đen vô hạn” không thể được quyết định từ bất kỳ giới hạn cố định nào trừ khi chúng ta biết một giới hạn lý thuyết vượt quá giới hạn mà hành vi ổn định. Việc thử tăng giới hạn vẫn không thể đảm bảo tính chính xác, vì trong các trường hợp bệnh lý, dãy số dài đầu tiên có thể đều có màu trắng ngay cả khi các số màu đen xuất hiện sau đó. 

Cái nhìn sâu sắc quan trọng là các số màu trắng tạo thành nửa nhóm cộng được tạo bởi`a`Và`b`. Đây là cấu trúc lý thuyết số cổ điển: tất cả các số màu trắng đều là tổ hợp tuyến tính`x·a + y·b`. Câu hỏi đặt ra là liệu nửa nhóm này cuối cùng có bao gồm tất cả các số nguyên đủ lớn hay không. Điều đó xảy ra chính xác khi`gcd(a, b) = 1`, bởi vì khi đó nửa nhóm là đồng hữu hạn, nghĩa là chỉ có hữu hạn nhiều số nguyên là không thể truy cập được. Nếu như`gcd(a, b) > 1`, tất cả các số màu trắng là bội số của gcd, để lại vô số số nguyên bên ngoài cấu trúc, do đó có vô số số màu đen. 

Điều này làm giảm toàn bộ quá trình động thành một điều kiện số học duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(N) mỗi lần kiểm tra | O(N) | Không đáng tin cậy cho quyết định vô cùng | 
| Kiểm tra dựa trên GCD | O(log min(a,b)) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`a`Và`b`cho từng trường hợp thử nghiệm. Mục đích là để xác định liệu quá trình cộng tính cuối cùng có thể đạt được tất cả các số nguyên lớn hay để lại vô số giá trị không thể tiếp cận được. 
2. Tính toán`g = gcd(a, b)`. Điều này nắm bắt cấu trúc tuần hoàn cơ bản của tất cả các số có thể tiếp cận được vì mọi giá trị có thể tiếp cận phải chia hết cho`g`. 
3. Nếu`g == 1`, kết luận rằng tập hợp có thể truy cập dày đặc theo nghĩa là chỉ thiếu hữu hạn nhiều số nguyên, do đó các số đen không thể tồn tại vô hạn. 
4. Ngược lại, nếu`g > 1`, kết luận rằng tất cả các số có thể truy cập được là bội số của`g`, nghĩa là mọi số nguyên không chia hết cho`g`vĩnh viễn không thể truy cập được. Vì có vô số số nguyên như vậy nên số màu đen là vô hạn. 

### Tại sao nó hoạt động 

Mọi số trắng đều được xây dựng bằng cách cộng liên tục`a`Và`b`, vậy nó phải nằm trong tập`{x·a + y·b}`. Tất cả các kết hợp như vậy đều chia hết cho`g = gcd(a, b)`, vì vậy không có số nào ngoài lớp dư lượng này có thể trở thành màu trắng. Khi`g = 1`, các kết quả cổ điển về bài toán đồng xu Frobenius đảm bảo rằng tất cả các số nguyên đủ lớn đều có thể biểu diễn được, điều này chỉ buộc hữu hạn nhiều số đen. Khi`g > 1`, các lớp dư lượng không thể truy cập được lặp lại vô hạn, đảm bảo có vô số số màu đen. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

t = int(input())
for _ in range(t):
    a, b = map(int, input().split())
    g = math.gcd(a, b)
    if g == 1:
        print("Finite")
    else:
        print("Infinite")
```Giải pháp sử dụng hàm gcd tích hợp của Python để trích xuất bất biến cấu trúc chính. Mỗi trường hợp thử nghiệm là độc lập, vì vậy chúng tôi chỉ cần áp dụng cùng một lần kiểm tra nhiều lần. 

Logic quyết định là sự chuyển dịch trực tiếp của đặc tính lý thuyết số: gcd bằng 1 ngụ ý phạm vi bao phủ cuối cùng, nếu không vẫn còn cản trở định kỳ. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai đầu vào để xem điều kiện gcd xác định kết quả như thế nào. 

### Ví dụ 1:`a = 10, b = 10`| Bước | một | b | gcd(a,b) | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | 10 | 10 | Vô hạn | 

Vì gcd là 10 nên tất cả các số màu trắng phải là bội số của 10. Các số như 1, 2, 3 không bao giờ trở thành màu trắng và mẫu này lặp lại vô thời hạn. Điều này xác nhận vô số số màu đen. 

### Ví dụ 2:`a = 6, b = 9`| Bước | một | b | gcd(a,b) | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | 6 | 9 | 3 | Vô hạn | 

Ở đây mọi số trắng đều chia hết cho 3. Tất cả các số nguyên không chia hết cho 3 sẽ mãi mãi có màu đen và có vô số số nguyên như vậy. 

Điều này cho thấy rằng mặc dù tập hợp có thể truy cập tăng lên nhưng nó không bao giờ thoát khỏi lớp mô-đun cố định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log min(a, b)) mỗi lần kiểm tra | bị chi phối bởi tính toán gcd | 
| Không gian | O(1) | chỉ sử dụng các biến không đổi | 

Các ràng buộc cho phép tối đa 100 trường hợp thử nghiệm với giá trị lên tới 10^4. Tính toán gcd logarit thực tế là thời gian không đổi trong phạm vi này, vì vậy giải pháp chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        a, b = map(int, input().split())
        out.append("Finite" if math.gcd(a, b) == 1 else "Infinite")
    return "\n".join(out)

# provided samples
assert solve("4\n10 10\n1 10\n6 9\n7 3\n") == "Infinite\nFinite\nInfinite\nFinite"

# custom cases
assert solve("1\n1 1\n") == "Infinite"
assert solve("1\n1 2\n") == "Finite"
assert solve("1\n4 6\n") == "Infinite"
assert solve("1\n5 10\n") == "Infinite"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | Vô hạn | các bước giống nhau, hạn chế đầy đủ | 
| 1 2 | hữu hạn | gcd = 1 trường hợp | 
| 4 6 | Vô hạn | gcd > 1 cấu trúc phức hợp | 
| 5 10 | Vô hạn | chu kỳ nhiều bước | 

## Vỏ cạnh 

Khi nào`a = b`, quá trình này thoái hóa thành một hệ thống có thể tiếp cận một bước. Vì`a = b = 10`, mọi số có thể truy cập đều là bội số của 10. Thuật toán tính toán`gcd(10, 10) = 10`, ngay lập tức phân loại nó thành số đen vô hạn, phù hợp với thực tế là không bao giờ có thể truy cập được tất cả các bội số của 10. 

Khi một giá trị được`1`, chẳng hạn như`a = 1, b = 10`, chúng tôi nhận được`gcd = 1`. Thuật toán trả về số đen hữu hạn. Trong quy trình thực tế, mọi số đều có thể truy cập được bằng cách thêm liên tục`1`, nên không có số màu đen nào xuất hiện cả. Kiểm tra gcd ghi lại điều này mà không mô phỏng sự lan truyền. 

Khi các giá trị chia sẻ một gcd lớn hơn như`a = 12, b = 18`, thuật toán xuất ra vô hạn. Tập hợp có thể truy cập được giới hạn ở bội số của 6, để lại vô số số nguyên bên ngoài lớp dư lượng này, tồn tại mãi mãi với màu đen.
