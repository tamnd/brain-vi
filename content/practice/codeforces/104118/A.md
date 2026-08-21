---
title: "CF 104118A - Một bài toán tính toán dễ"
description: "Chúng ta được cho một hàm $f(x)$ được xác định trên các số thực, nhưng được chia thành ba vùng của $x$. Mỗi vùng sử dụng một công thức khác nhau: một biểu thức tuyến tính ở phía bên trái, một biểu thức tuyến tính khác ở giữa và một đa thức bậc ba ở bên phải."
date: "2026-07-02T01:50:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104118
codeforces_index: "A"
codeforces_contest_name: "2022 ICPC Asia-Manila Regional Contest"
rating: 0
weight: 104118
solve_time_s: 43
verified: true
draft: false
---

[CF 104118A - Một bài toán tính toán dễ](https://codeforces.com/problemset/problem/104118/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chức năng$f(x)$được xác định trên số thực, nhưng được chia thành ba vùng$x$. Mỗi vùng sử dụng một công thức khác nhau: một biểu thức tuyến tính ở phía bên trái, một biểu thức tuyến tính khác ở giữa và một đa thức bậc ba ở bên phải. 

Nhiệm vụ không phải là tìm ra các hằng số, vì bài toán đã cung cấp các giá trị duy nhất làm cho hàm hoạt động trơn tru ở mọi nơi. Thay vào đó, chúng tôi trực tiếp đánh giá hàm bằng cách sử dụng các hệ số cố định đó cho một đầu vào số nguyên duy nhất.$x$. Đầu vào được đảm bảo nhỏ, giữa$-10$Và$10$, vì vậy chúng ta chỉ cần đánh giá theo thời gian không đổi. 

Chi tiết cấu trúc quan trọng là chức năng này được thực hiện từng phần. Điều đó có nghĩa là phần không cần thiết duy nhất của phép tính là quyết định công thức nào áp dụng cho kết quả đã cho$x$. Khi phân đoạn chính xác được chọn, việc đánh giá là số học đơn giản. 

Một sai lầm ngây thơ ở đây là hiểu sai ranh giới khoảng. Phần bên trái bao gồm$x \le -3$, phần giữa bao gồm$-3 < x \le 2$, và phần bên phải bao gồm$x > 2$. Các điểm ranh giới$-3$Và$2$do đó đặc biệt quan trọng. Ví dụ, tại$x = -3$, việc sử dụng sai nhánh sẽ thay đổi hoàn toàn câu trả lời mặc dù cả hai công thức đều là biểu thức hợp lệ. 

Bởi vì phạm vi đầu vào rất nhỏ nên không có vấn đề gì về hiệu suất. Bất kỳ triển khai chính xác nào đánh giá một nhánh trong thời gian không đổi là đủ. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ cố gắng mã hóa hàm chính xác như được viết, thậm chí có thể cố gắng tính toán hoặc xác minh các điều kiện khả vi. Điều đó là không cần thiết ở đây vì các hệ số đã được cố định. Công việc duy nhất còn lại là đánh giá một biểu thức từng phần một lần. 

Quan sát quan trọng là hàm này được xác định đầy đủ bởi thành viên khoảng. Chúng ta không cần lý luận tính toán hoặc thao tác biểu tượng. Chúng ta chỉ cần kiểm tra xem ở đâu$x$nói dối và áp dụng công thức tương ứng. 

Nỗ lực mạnh mẽ vẫn sẽ chạy trong thời gian không đổi cho mỗi truy vấn, nhưng nó có thể gây ra sự phức tạp không cần thiết như tính toán lại công thức hoặc xử lý các biểu thức tượng trưng. Cách tiếp cận tối ưu đơn giản hóa mọi thứ thành ba phép tính trực tiếp và một vài phép so sánh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đánh giá lực lượng vũ phu của hình thức chung từng phần | O(1) | O(1) | Đã chấp nhận | 
| Đánh giá chi nhánh trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng ta viết lại hàm bằng cách sử dụng các hằng số được cung cấp để việc đánh giá trở nên máy móc. 

Vì$x \le -3$, hàm số là$f(x) = -(x + 4) + 8 = -x + 4$. 

Vì$-3 < x \le 2$, hàm số là$f(x) = -2x + 1$. 

Vì$x > 2$, hàm số là$f(x) = x^3 - 14x + 17$. 

Bây giờ việc tính toán trở thành một quá trình quyết định đơn giản. 

1. Đọc số nguyên$x$. Đây là giá trị đầu vào duy nhất và xác định hoàn toàn nhánh. 
2. Kiểm tra xem$x \le -3$. Nếu điều kiện này đúng thì tính$-x + 4$. Nhánh này tương ứng với đoạn tuyến tính ngoài cùng bên trái, vì vậy không có thuật ngữ nào khác có liên quan. 
3. Nếu không, hãy kiểm tra xem$x \le 2$. Nếu điều này đúng, hãy tính$-2x + 1$. Đây là đoạn giữa và bao gồm cả$x = -2$Và$x = 2$, do đó bất đẳng thức phải bao hàm ở điểm cuối bên phải. 
4. Nếu cả hai điều trên đều không đúng thì$x > 2$. Tính toán$x^3 - 14x + 17$. Đây là khu vực duy nhất còn lại nên không cần kiểm tra thêm. 

Tính đúng đắn xuất phát từ thực tế là ba khoảng này tạo thành một phân vùng hoàn chỉnh, không chồng chéo của đường thực. Mỗi đầu vào số nguyên có thể thuộc về chính xác một nhánh, do đó, chính xác một công thức được áp dụng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

x = int(input().strip())

if x <= -3:
    print(-x + 4)
elif x <= 2:
    print(-2 * x + 1)
else:
    print(x**3 - 14 * x + 17)
```Việc triển khai phản ánh trực tiếp logic khoảng thời gian. Điều kiện đầu tiên xử lý tất cả các giá trị lên đến và bao gồm$-3$, vì vậy nó phải được kiểm tra trước. Điều kiện thứ hai tự động ngụ ý phạm vi$-3 < x \le 2$vì nhánh trước đã loại trừ các giá trị nhỏ hơn. Mọi thứ khác rơi vào hộp hình khối. 

Sự tinh tế duy nhất là đảm bảo thứ tự của các điều kiện được giữ nguyên. Sắp xếp lại chúng không chính xác có thể gây ra các giá trị biên như$-3$hoặc$2$được gán cho biểu thức sai. 

## Ví dụ đã hoạt động 

Hãy xem xét$x = -3$. Điều này nằm trong khoảng đầu tiên bởi vì$x \le -3$, vì vậy chúng tôi tính toán$-(-3) + 4 = 7$. 

| Bước | Kiểm tra tình trạng | Biểu thức được sử dụng | Kết quả | 
| --- | --- | --- | --- | 
| 1 |$x \le -3$là đúng |$-x + 4$| 7 | 

Điều này cho thấy các giá trị biên được hấp thụ vào đoạn bên trái như thế nào. 

Bây giờ hãy xem xét$x = 3$. Khoảng này nằm trong khoảng thứ ba vì nó lớn hơn 2 nên chúng ta sử dụng dạng lập phương. 

| Bước | Kiểm tra tình trạng | Biểu thức được sử dụng | Kết quả | 
| --- | --- | --- | --- | 
| 1 |$x \le -3$sai | bỏ qua | | 
| 2 |$x \le 2$sai | bỏ qua | | 
| 3 | trường hợp mặc định |$x^3 - 14x + 17$|$27 - 42 + 17 = 2$| 

Điều này xác nhận rằng khi giá trị vượt quá 2, chỉ có biểu thức bậc ba là quan trọng và các phần tuyến tính là không liên quan. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số lượng không đổi các phép so sánh và phép tính số học được thực hiện. | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng ngoài một biến duy nhất. | 

Những ràng buộc hạn chế$x$đến một phạm vi nhỏ, nhưng ngay cả khi không có hạn chế đó, giải pháp sẽ vẫn giữ nguyên thời gian không đổi vì việc đánh giá hàm không mở rộng theo kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    x = int(sys.stdin.readline().strip())

    if x <= -3:
        return str(-x + 4)
    elif x <= 2:
        return str(-2 * x + 1)
    else:
        return str(x**3 - 14 * x + 17)

# sample-style checks
assert run("-3") == "7"
assert run("3") == "2"

# boundary cases
assert run("-10") == "14"
assert run("-4") == "8"
assert run("2") == "-3"
assert run("0") == "1"

# cubic edge
assert run("10") == str(1000 - 140 + 17)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| -10 | 14 | nhánh trái cực đúng | 
| -3 | 7 | bao gồm ranh giới bên trái | 
| 2 | -3 | bao gồm ranh giới giữa | 
| 0 | 1 | độ chính xác của khoảng giữa | 
| 10 | 877 | nhánh khối đúng đắn | 

## Vỏ cạnh 

Các trường hợp cạnh quan trọng nhất chính xác là các ranh giới khoảng tại$x = -3$Và$x = 2$. 

Vì$x = -3$, đầu vào sẽ kích hoạt nhánh đầu tiên vì điều kiện là$x \le -3$. Việc tính toán là$-(-3) + 4 = 7$. Nếu một lập trình viên sử dụng nhầm bất đẳng thức nghiêm ngặt hoặc kiểm tra nhánh giữa trước, giá trị này sẽ rơi vào công thức tuyến tính ở giữa một cách không chính xác. 

Vì$x = 2$, nhánh thứ hai được áp dụng vì nó được định nghĩa là$x \le 2$. Việc tính toán trở thành$-2 \cdot 2 + 1 = -3$. Nếu điều kiện được viết sai như$x < 2$, sau đó$x = 2$sẽ nhảy sang biểu thức bậc ba một cách không chính xác, tạo ra một kết quả hoàn toàn khác. 

Cả hai trường hợp đều cho thấy rằng tính đúng đắn không phụ thuộc vào số học mà phụ thuộc vào việc bảo toàn phân vùng khoảng chính xác.
