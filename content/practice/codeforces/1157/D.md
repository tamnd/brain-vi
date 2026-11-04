---
title: "CF 1157D - N Sự cố trong K ngày"
description: "Chúng ta cần phân phối chính xác n bài toán đã giải trong k ngày liên tiếp. Gọi a[i] là số bài toán được giải quyết vào ngày thứ i. Mỗi ngày phải có ít nhất một vấn đề."
date: "2026-06-12T02:34:16+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1157
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 555 (Div. 3)"
rating: 1900
weight: 1157
solve_time_s: 105
verified: false
draft: false
---

[CF 1157D - N sự cố trong K ngày](https://codeforces.com/problemset/problem/1157/D) 

**Xếp hạng:** 1900 
**Tags:** thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần phân phối chính xác`n`đã giải quyết được vấn đề xuyên suốt`k`ngày liên tiếp. 

Cho phép`a[i]`là số lượng vấn đề được giải quyết trong ngày`i`. Mỗi ngày phải có ít nhất một vấn đề. Trình tự phải tăng dần và giá trị của mỗi ngày có thể tăng tối đa theo hệ số hai:$$a_i < a_{i+1} \le 2a_i$$Tổng của tất cả các giá trị phải chính xác`n`. 

Nhiệm vụ không phải là tối ưu hóa bất cứ điều gì. Chúng ta chỉ cần xây dựng một chuỗi hợp lệ hoặc xác định rằng không tồn tại chuỗi đó. 

Những hạn chế là bất thường. Số ngày có thể lớn như`10^5`, trong khi tổng số vấn đề có thể lớn như`10^9`. Bất kỳ thuật toán nào cố gắng tìm kiếm trên các chuỗi có thể đều là vô vọng. Ngay cả việc lưu trữ nhiều trạng thái ứng cử viên mỗi ngày cũng sẽ quá tốn kém. Chúng ta cần một phương pháp xây dựng trực tiếp chạy trong thời gian gần như tuyến tính trong`k`. 

Quan sát đầu tiên xuất phát từ chuỗi hợp lệ nhỏ nhất có thể. Vì các giá trị phải tăng dần theo số nguyên dương nên dãy tối thiểu là$$1,2,3,\ldots,k$$tổng của ai là$$\frac{k(k+1)}2$$Nếu như`n`nhỏ hơn giá trị này thì không có nghiệm nào tồn tại vì mọi dãy hợp lệ phải có ít nhất bằng đó tổng số bài toán. 

Trường hợp tinh tế thứ hai xảy ra khi chuỗi bị ép buộc quá lớn. Giả định`k=3`Và`n=100`. Bắt đầu từ bất kỳ giá trị đầu tiên nào`x`, dãy lớn nhất có thể là xấp xỉ$$x,\ 2x,\ 4x$$với tổng số`7x`. Hạn chế tăng trưởng giới hạn tổng số tiền có thể được đóng gói vào chuỗi. Một công trình phải tôn trọng cả giới hạn tăng trưởng dưới và trên. 

Coi như`n=5, k=3`. 

Số tiền tối thiểu có thể là$$1+2+3=6$$vậy câu trả lời đúng là`NO`. Việc triển khai bất cẩn chỉ kiểm tra mức độ tích cực có thể tạo ra kết quả không chính xác`1 1 3`, nhưng dãy không tăng một cách chặt chẽ. 

Coi như`n=8, k=3`. 

Một câu trả lời hợp lệ là$$1\ 2\ 5$$Không, nó không hợp lệ vì`5 > 2·2`. Hạn chế nhân đôi rất dễ bị lãng quên khi tham lam trút bỏ những vấn đề còn sót lại vào ngày cuối cùng. 

Coi như`n=26, k=6`. 

Trình tự tối thiểu đã được sử dụng$$1+2+3+4+5+6 = 21$$Chỉ có 5 vấn đề bổ sung để phân phối. Việc phân phối phải duy trì cả tính đơn điệu và ràng buộc nhân đôi. Chỉ cần thêm tất cả các tính năng bổ sung vào phần tử cuối cùng sẽ tạo ra$$1\ 2\ 3\ 4\ 5\ 11$$vi phạm`11 ≤ 10`. 

Những ví dụ này cho thấy khó khăn là cân bằng số tiền tăng thêm trong khi vẫn duy trì tất cả các ràng buộc cục bộ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng tạo ra các chuỗi tăng dần có tổng là`n`và kiểm tra điều kiện nhân đôi. Ngay cả đối với các giá trị vừa phải, điều này trở nên rất lớn. Số dãy số nguyên tăng dần với tổng cố định tăng theo tổ hợp và`n`có thể đạt được`10^9`. Tìm kiếm toàn diện là hoàn toàn không khả thi. 

Quan sát quan trọng là mọi chuỗi hợp lệ đều có thể được xem như một chuỗi cơ sở$$x,\ x+1,\ x+2,\ \ldots,\ x+k-1$$cộng với một số khoản tăng thêm được phân bổ trong các ngày. 

Nếu giá trị đầu tiên được cố định thành`x`, số tiền tối thiểu có thể đạt được là$$kx + \frac{k(k-1)}2.$$Điều này ngay lập tức gợi ý việc tìm giá trị bắt đầu lớn nhất có thể`x`có tổng tối thiểu không vượt quá`n`. 

Tại sao giá trị ban đầu lớn lại hữu ích? Bởi vì tăng mỗi phần tử lên một sẽ tiêu tốn`k`đơn vị của tổng số tiền trong khi tự động duy trì tất cả các ràng buộc. Chúng tôi muốn hấp thụ càng nhiều`n`theo cách này càng tốt trước khi xử lý số tiền nhỏ hơn còn lại. 

Sau khi sửa`x`, vẫn còn một số tiền dư$$r = n - \left(kx + \frac{k(k-1)}2\right).$$Trình tự bắt đầu như$$x,\ x+1,\ \ldots,\ x+k-1.$$Bây giờ chúng tôi phân phối`r`các đơn vị bổ sung. Ràng buộc nhân đôi xác định mỗi vị trí có thể tăng bao nhiêu. Xử lý từ trái sang phải, mỗi phần tử có thể phát triển cho đến khi đạt được một trong hai$$2a_{i-1}$$hoặc bất cứ số lượng nào vẫn cần thiết. Sự phân bổ tham lam này trước tiên tối đa hóa các phần tử trước đó, tạo ra càng nhiều chỗ càng tốt cho các phần tử sau. 

Nếu sau khi xử lý tất cả các vị trí mà phần còn lại bằng 0 thì chúng ta có một chuỗi hợp lệ. Nếu không thì không thể đạt được tổng mục tiêu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số tiền tối thiểu có thể:$$S_{\min}=\frac{k(k+1)}2$$Nếu như`n < Smin`, in`NO`. 
2. Tìm số nguyên lớn nhất`x`như vậy$$kx+\frac{k(k-1)}2 \le n.$$Đây là$$x=\left\lfloor\frac{n-\frac{k(k-1)}2}{k}\right\rfloor.$$Việc chọn giá trị ban đầu lớn nhất có thể sẽ hấp thụ phần lớn tổng số tiền trong khi tự động đáp ứng tất cả các ràng buộc. 
3. Xây dựng trình tự cơ sở$$a_i = x+i$$sử dụng lập chỉ mục dựa trên số không. 
4. Tính số tiền còn lại$$r = n-\sum a_i.$$5. Xử lý các vị trí từ đầu đến cuối. 

Đối với vị trí`i`, xác định mức tăng lớn nhất giữ nguyên$$a_i \le 2a_{i-1}.$$Công suất sẵn có là$$2a_{i-1}-a_i.$$6. Thêm$$\min(r,\text{capacity})$$ĐẾN`a_i`, sau đó trừ đi số tiền tương tự từ`r`. 
7. Tiếp tục cho đến khi tất cả các vị trí đã được xử lý. 
8. Nếu`r`vẫn dương, in`NO`. Nếu không thì in`YES`và trình tự. 

### Tại sao nó hoạt động 

Chuỗi cơ sở là chuỗi hợp lệ nhỏ nhất có giá trị đầu tiên`x`. Bất kỳ chuỗi hợp lệ lớn hơn nào có cùng giá trị đầu tiên chỉ có thể thu được bằng cách thêm số gia không âm. 

Khi phân phối số tiền còn lại, việc tăng vị trí sau không bao giờ ảnh hưởng đến các ràng buộc liên quan đến các vị trí trước đó. Cung cấp cho mỗi vị trí càng nhiều càng tốt sẽ tối đa hóa năng lực trong tương lai của chuỗi. Nếu quá trình tham lam này không thể hấp thụ tất cả các đơn vị còn lại thì không có sự phân phối nào khác có thể làm được, bởi vì mọi vị trí đều đã đạt đến giá trị pháp lý tối đa. Do đó thuật toán thành công chính xác khi tồn tại một chuỗi hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())

    if n < k * (k + 1) // 2:
        print("NO")
        return

    x = (n - k * (k - 1) // 2) // k

    a = [x + i for i in range(k)]

    cur_sum = sum(a)
    rem = n - cur_sum

    for i in range(k - 1, 0, -1):
        cap = 2 * a[i - 1] - a[i]
        add = min(rem, cap)
        a[i] += add
        rem -= add

    if rem != 0:
        print("NO")
        return

    print("YES")
    print(*a)

solve()
```Kiểm tra đầu tiên xử lý điều kiện cơ bản không thể thực hiện được. Nếu ngay cả dãy tăng dần nghiêm ngặt`1,2,...,k`vượt quá`n`, không có công trình nào tồn tại. 

giá trị`x`được chọn là giá trị ban đầu khả thi lớn nhất. Đây là ý tưởng quan trọng giúp giảm mức điều chỉnh còn lại xuống ít hơn`k`. 

Mảng ban đầu chứa chuỗi hợp lệ tối thiểu bắt đầu từ`x`. Số tiền của nó được đảm bảo không vượt quá`n`. 

Phần còn lại được phân phối từ phải sang trái. Đối với mỗi vị trí, chúng tôi tính toán chính xác lượng chỗ còn lại trước khi giới hạn nhân đôi trở nên chặt chẽ. Việc thêm nhiều hơn số tiền này sẽ vi phạm các điều kiện của vấn đề. 

Việc kiểm tra phần còn lại cuối cùng là cần thiết. Một số giá trị của`n`không thể được đại diện mặc dù`n`vượt quá số tiền tối thiểu. Trong những trường hợp đó, việc lấp đầy tham lam đạt đến mọi khả năng trong khi một số phần còn lại vẫn tồn tại, chứng tỏ là không thể. 

Tất cả số học đều phù hợp thoải mái với số nguyên 64 bit. Số nguyên Python tự động xử lý việc này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
26 6
```Tính toán đầu tiên$$x=\left\lfloor\frac{26-15}{6}\right\rfloor=1$$Trình tự cơ sở:$$[1,2,3,4,5,6]$$Tổng = 21, số dư = 5. 

| Vị trí | Giá trị trước | Công suất | Đã thêm | Giá trị sau | Còn lại | 
| --- | --- | --- | --- | --- | --- | 
| 5 | 6 | 4 | 4 | 10 | 1 | 
| 4 | 5 | 3 | 1 | 6 | 0 | 

Trình tự cuối cùng:```
1 2 3 4 6 10
```Tổng là 26, chuỗi tăng nghiêm ngặt và mọi giá trị tối đa gấp đôi giá trị trước đó. 

### Ví dụ 2 

đầu vào:```
10 4
```Số tiền tối thiểu có thể:$$1+2+3+4=10$$Vì thế`x=1`. 

| Vị trí | Giá trị trước | Công suất | Đã thêm | Giá trị sau | Còn lại | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | - | - | - | [1,2,3,4] | 0 | 

Trình tự cuối cùng:```
1 2 3 4
```Ví dụ này cho thấy ranh giới không cần phân phối thêm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k) | Một lượt để xây dựng trình tự và một lượt để phân phối phần còn lại | 
| Không gian | O(k) | Lưu trữ câu trả lời được xây dựng | 

Với`k ≤ 100000`, quét tuyến tính dễ dàng đủ nhanh. Việc sử dụng bộ nhớ cũng khiêm tốn vì chỉ lưu trữ chuỗi cuối cùng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n, k = map(int, input().split())

    if n < k * (k + 1) // 2:
        return "NO\n"

    x = (n - k * (k - 1) // 2) // k

    a = [x + i for i in range(k)]
    rem = n - sum(a)

    for i in range(k - 1, 0, -1):
        cap = 2 * a[i - 1] - a[i]
        add = min(rem, cap)
        a[i] += add
        rem -= add

    if rem:
        return "NO\n"

    return "YES\n" + " ".join(map(str, a)) + "\n"

assert run("26 6\n").startswith("YES"), "sample"

assert run("1 1\n") == "YES\n1\n", "minimum input"

assert run("5 3\n") == "NO\n", "below minimum sum"

assert run("10 4\n") == "YES\n1 2 3 4\n", "exact minimum"

assert run("1000000000 1\n") == "YES\n1000000000\n", "largest n with k=1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`YES 1`| Trường hợp pháp lý nhỏ nhất | 
|`5 3`|`NO`| Tổng dưới mức tối thiểu có thể | 
|`10 4`|`1 2 3 4`| Chính xác ranh giới dưới | 
|`1000000000 1`| Giá trị đơn | Xử lý số lượng lớn | 

## Vỏ cạnh 

Hãy xem xét:```
5 3
```Tổng hợp lệ tối thiểu là$$1+2+3=6$$Từ`5 < 6`, thuật toán in ngay lập tức`NO`. Điều này ngăn cản việc xây dựng đáp ứng tổng nhưng vi phạm mức tăng nghiêm ngặt. 

Coi như:```
8 3
```Thuật toán xây dựng`[1,2,3]`với phần còn lại`2`. 

Vị trí cuối cùng có năng lực$$2\cdot2-3=1$$vì vậy nó trở thành`4`, để lại phần dư`1`. 

Vị trí ở giữa không thể tăng lên vì nó sẽ phá vỡ trật tự nghiêm ngặt với phần tử đầu tiên. Phần còn lại tồn tại nên thuật toán đưa ra`NO`. 

Điều này từ chối chính xác các chuỗi như`1 2 5`, thỏa mãn tổng nhưng vi phạm giới hạn nhân đôi. 

Coi như:```
100 3
```Thuật toán bắt đầu với một số lượng lớn`x`, sau đó cố gắng phân phối số tiền còn lại. Mọi vị trí cuối cùng đều đạt đến giá trị pháp lý tối đa. Nếu số tiền bổ sung vẫn còn thì thuật toán sẽ báo cáo là không thể thực hiện được. Điều này nắm bắt được hạn chế tăng trưởng cao hơn mà nhiều giải pháp tham lam ngây thơ bỏ qua.
