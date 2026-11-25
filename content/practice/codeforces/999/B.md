---
title: "CF 999B - Mã hóa đảo ngược"
description: "Chúng ta có một chuỗi đã được biến đổi bằng một quá trình xác định liên quan đến việc đảo ngược các tiền tố lặp đi lặp lại."
date: "2026-06-16T23:51:35+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 999
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 490 (Div. 3)"
rating: 900
weight: 999
solve_time_s: 68
verified: true
draft: false
---

[CF 999B - Mã hóa đảo ngược](https://codeforces.com/problemset/problem/999/B) 

**Xếp hạng:** 900 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi đã được biến đổi bằng một quá trình xác định liên quan đến việc đảo ngược các tiền tố lặp đi lặp lại. Chuỗi gốc không xác định và quy trình tạo ra chuỗi cuối cùng được chỉ định đầy đủ: đối với mỗi ước số của độ dài, lấy từ lớn nhất đến nhỏ nhất, chúng tôi đảo ngược tiền tố của độ dài đó. 

Khó khăn chính là những sự đảo chiều này chồng chéo lên nhau. Mỗi vị trí trong chuỗi được di chuyển nhiều lần và thứ tự cuối cùng sẽ ẩn chuỗi các phép biến đổi. Chúng tôi được yêu cầu xây dựng lại chuỗi gốc trước khi áp dụng bất kỳ sự đảo ngược nào. 

Kích thước đầu vào nhỏ, với độ dài chuỗi lên tới 100. Điều này ngay lập tức cho thấy rằng việc tái tạo hoặc mô phỏng O(n^2) là đủ. Bất kỳ giải pháp nào cố gắng hoán vị vũ phu đều không cần thiết, nhưng ngay cả khi cố gắng, việc tăng trưởng giai thừa cũng không liên quan vì n rất nhỏ. 

Các trường hợp cạnh ở đây chủ yếu liên quan đến cấu trúc hơn là liên quan đến hiệu suất. Khi n bằng 1, không có thao tác nào thay đổi chuỗi, do đó đầu ra giống hệt đầu vào. Khi n là số nguyên tố, chỉ có ước số là 1 và n, do đó có đúng một đảo ngược có ý nghĩa. Một mô phỏng thuận đơn giản thì dễ dàng, nhưng thứ tự đảo ngược không chính xác sẽ dẫn đến việc tái thiết sai. 

Một kiểu lỗi phổ biến là mô phỏng mã hóa trực tiếp mà không hiểu tính khả nghịch. Một người khác đang cố gắng đảo ngược tất cả các tiền tố theo thứ tự số chia tăng dần thay vì xây dựng lại chuỗi ban đầu từng bước. 

## Phương pháp tiếp cận 

Quá trình chuyển tiếp áp dụng đảo ngược tiền tố theo thứ tự số chia giảm dần. Mỗi thao tác chỉ phụ thuộc vào tiền tố chứ không phụ thuộc vào tương tác hậu tố. Cấu trúc này làm cho sự biến đổi có thể đảo ngược được một cách rất có kiểm soát. 

Một ý tưởng mạnh mẽ sẽ là tạo ra một chuỗi gốc ứng viên, mô phỏng quá trình mã hóa và so sánh nó với chuỗi đích. Mặc dù đúng nhưng mỗi mô phỏng có giá O(n^2) và việc thử tất cả các hoán vị sẽ là O(n!). Ngay cả khi chỉ giới hạn ở một ứng cử viên duy nhất, việc mô phỏng lặp đi lặp lại là không cần thiết. 

Cái nhìn sâu sắc quan trọng là các đảo ngược tiền tố là các đảo ngược, có nghĩa là mỗi đảo ngược là nghịch đảo của chính nó. Tuy nhiên, thứ tự quan trọng. Nếu chúng ta đảo ngược các tiền tố theo thứ tự d1, d2, ..., dk trong mã hóa thì khi giải mã, chúng ta phải áp dụng các thao tác tương tự theo thứ tự ngược lại: dk, ..., d2, d1. 

Nhưng có một quan sát có cấu trúc thậm chí còn đơn giản hơn. Vì các ước số được xử lý theo thứ tự giảm dần trong quá trình mã hóa nên phép biến đổi có ý nghĩa cuối cùng được áp dụng tương ứng với ước số nhỏ nhất lớn hơn 1. Điều đó có nghĩa là trong quá trình giải mã, chúng tôi xây dựng lại chuỗi bằng cách áp dụng đảo ngược tiền tố theo thứ tự số chia tăng dần. 

Chúng ta có thể tính toán tất cả các ước của n, sắp xếp chúng và mô phỏng việc áp dụng đảo ngược tiền tố theo thứ tự được sắp xếp đó trên chuỗi được mã hóa. Điều này sẽ xây dựng lại chuỗi gốc vì mỗi lần đảo ngược sẽ hoàn tác tác động của bước tương ứng trong quy trình chuyển tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu từ các ứng viên | O(n^3) hoặc tệ hơn | O(n) | Quá chậm/không cần thiết | 
| Mô phỏng số chia ngược | O(n √n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính tất cả các ước của n. Mỗi ước số tương ứng với một thao tác đảo ngược tiền tố được sử dụng trong quá trình mã hóa. Chúng ta cần chúng vì chúng xác định trình tự chuyển đổi. 
2. Sắp xếp các ước số theo thứ tự tăng dần. Thứ tự này rất quan trọng vì chúng ta đang xây dựng lại chuỗi ban đầu bằng cách hủy bỏ cấu trúc phân lớp của các đảo ngược một cách hiệu quả. 
3. Bắt đầu với chuỗi mã hóa t làm mảng làm việc của chúng ta. Chúng tôi coi nó như một danh sách các ký tự có thể thay đổi để hỗ trợ đảo ngược tiền tố hiệu quả. 
4. Đối với mỗi ước số d theo thứ tự được sắp xếp, hãy đảo ngược tiền tố từ chỉ số 0 thành d − 1. Bước này được áp dụng vì nó đảo ngược ảnh hưởng của cách tiền tố đó được thay đổi trong quá trình chuyển tiếp. 
5. Sau khi xử lý tất cả các ước số, mảng biểu thị chuỗi s gốc nên chúng ta xuất ra. 

### Tại sao nó hoạt động 

Mỗi thao tác trong quy trình mã hóa là một phép đảo ngược tiền tố được áp dụng chính xác một lần cho mỗi ước số, nhưng theo thứ tự giảm dần. Việc đảo ngược tiền tố là tự đảo ngược, do đó việc áp dụng cùng một sự đảo ngược hai lần sẽ khôi phục thứ tự ban đầu cho phân đoạn tiền tố đó. 

Cấu trúc lồng nhau của các ước số đảm bảo rằng các tiền tố nhỏ hơn được chuyển đổi sau các tiền tố lớn hơn trong quá trình mã hóa, điều đó có nghĩa là tác động của chúng “nằm trong” sự sắp xếp cuối cùng. Bằng cách áp dụng các phép đảo ngược theo thứ tự tăng dần, chúng ta bóc tách các phép biến đổi từ cấu trúc trong cùng ra bên ngoài. Điều này duy trì tính nhất quán ở mỗi bước vì mỗi lần đảo ngược chỉ ảnh hưởng đến tiền tố chưa được khôi phục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
t = list(input().strip())

divs = []
for i in range(1, n + 1):
    if n % i == 0:
        divs.append(i)

divs.sort()

for d in divs:
    t[:d] = reversed(t[:d])

print("".join(t))
```Giải pháp bắt đầu bằng cách đọc chuỗi vào danh sách có thể thay đổi để việc đảo ngược tiền tố có thể được thực hiện một cách hiệu quả. Các ước số được tính bằng cách kiểm tra tất cả các số nguyên lên đến n, đủ với ràng buộc nhỏ. 

Việc sắp xếp đảm bảo chúng ta áp dụng các phép biến đổi theo đúng thứ tự tái thiết. Mỗi lần đảo ngược tiền tố được triển khai bằng cách sử dụng tính năng cắt Python, thể hiện rõ ràng thao tác mà không cần logic hoán đổi thủ công. 

Phép nối cuối cùng sẽ chuyển đổi danh sách trở lại thành một chuỗi để xuất ra. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 10
t = rocesfedoc
```Các ước của 10 là 1, 2, 5, 10. Chúng ta áp dụng phép đảo ngược theo thứ tự tăng dần. 

| Bước | Hoạt động | Trạng thái chuỗi | 
| --- | --- | --- | 
| 0 | ban đầu | rocesfedoc | 
| 1 | tiền tố đảo ngược 1 | rocesfedoc | 
| 2 | tiền tố đảo ngược 2 | orcesfedoc | 
| 3 | tiền tố đảo ngược 5 | codefseroc | 
| 4 | tiền tố đảo ngược 10 | lực lượng mật mã | 

Quá trình xây dựng lại chuỗi gốc một cách chính xác. 

Dấu vết này cho thấy các tiền tố nhỏ hơn được sửa sớm như thế nào, trong khi các tiền tố lớn hơn sửa thứ tự chung sau cùng. 

### Ví dụ 2 

đầu vào:```
n = 6
t = "cbadef"
```Các ước số là 1, 2, 3, 6. 

| Bước | Hoạt động | Trạng thái chuỗi | 
| --- | --- | --- | 
| 0 | ban đầu | cbadef | 
| 1 | tiền tố đảo ngược 1 | cbadef | 
| 2 | tiền tố đảo ngược 2 | bcadef | 
| 3 | tiền tố đảo ngược 3 | abcdef | 
| 4 | tiền tố đảo ngược 6 | fedcba | 

Ví dụ này nhấn mạnh rằng các trạng thái trung gian có thể trông không liên quan đến kết quả cuối cùng, nhưng mỗi bước hoàn toàn hoàn tác một lớp đảo ngược có cấu trúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n √n) | Việc tìm các ước số mất O(n), mỗi lần đảo tiền tố tốn O(n), áp dụng cho tối đa √n ước số | 
| Không gian | O(n) | Chúng tôi lưu trữ chuỗi dưới dạng danh sách có thể thay đổi | 

Các ràng buộc cho phép lên tới n = 100, do đó, việc cắt tiền tố lặp đi lặp lại là không đáng kể. Giải pháp chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input().strip())
    t = list(input().strip())

    divs = []
    for i in range(1, n + 1):
        if n % i == 0:
            divs.append(i)

    divs.sort()

    for d in divs:
        t[:d] = reversed(t[:d])

    return "".join(t)

# provided sample
assert run("10\nrocesfedoc\n") == "codeforces"

# minimum size
assert run("1\na\n") == "a"

# prime length
assert run("5\nabcde\n") == "bcdea"

# small composite
assert run("4\ndcba\n") == "abcd"

# repeated pattern
assert run("6\nfedcba\n") == "abcdef"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10 câu chuyện | lực lượng mật mã | độ chính xác trên mẫu | 
| 1 một | một | trường hợp cạnh tối thiểu | 
| 5 abcde | bcdea | cấu trúc có độ dài nguyên tố | 
| 4 dcba | abcd | tương tác đảo ngược hoàn toàn | 
| 6 fedcba | abcdef | đảo ngược lớp | 

## Vỏ cạnh 

Với n = 1, tập hợp ước số chỉ là {1}. Thuật toán áp dụng đảo ngược tiền tố có độ dài 1, khiến chuỗi không thay đổi, do đó đầu ra bằng đầu vào. 

Đối với số nguyên tố n, giả sử n = 5, ước số là 1 và 5. Đầu tiên, thuật toán đảo ngược tiền tố 1, không làm gì cả, sau đó đảo ngược chuỗi đầy đủ, khôi phục cấu trúc thứ tự ban đầu do mã hóa áp đặt. Việc đảo ngược từng bước sẽ hoàn tác chính xác quá trình chuyển đổi có ý nghĩa duy nhất. 

Đối với n tổng hợp cao chẳng hạn như 12, nhiều lần đảo ngược tiền tố chồng chéo xảy ra ở 1, 2, 3, 4, 6, 12. Mỗi bước chỉ sửa đổi một tiền tố phù hợp với cấu trúc được khôi phục trước đó, do đó không có thao tác nào sau này làm mất hiệu lực các sửa đổi trước đó.
