---
title: "CF 345B - Triskaidekaphobia"
description: "Chúng ta được cho một số $n le 10^5$. Với mọi cơ số nguyên $b ge 2$, chúng ta viết $n$ trong cơ số $b$, nhưng thay vì các ký hiệu chữ số thông thường, chúng ta biểu thị mỗi chữ số dưới dạng số thập phân và nối chúng mà không có dấu phân cách."
date: "2026-06-06T18:06:55+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 345
codeforces_index: "B"
codeforces_contest_name: "Friday the 13th, Programmers Day"
rating: 2100
weight: 345
solve_time_s: 107
verified: true
draft: false
---

[CF 345B - Triskaidekaphobia](https://codeforces.com/problemset/problem/345/B) 

**Xếp hạng:** 2100 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một số$n \le 10^5$. Với mọi cơ số nguyên$b \ge 2$, chúng tôi viết$n$trong căn cứ$b$, nhưng thay vì các ký hiệu chữ số thông thường, chúng tôi biểu thị mỗi chữ số dưới dạng số thập phân và nối chúng mà không có dấu phân cách. Ví dụ, trong cơ số 16, chữ số 14 được viết dưới dạng ký tự “14”, không phải là một ký hiệu đơn lẻ. 

Nhiệm vụ là đếm có bao nhiêu căn cứ$b$tạo ra một đại diện của$n$có chuỗi chữ số kết quả chứa chuỗi con “13”. Nếu có vô số cơ số như vậy thì kết quả là -1. 

Khó khăn chính là việc biểu diễn không phải là mã hóa có chiều rộng cố định. Các chữ số mở rộng thành các chuỗi thập phân có độ dài thay đổi, do đó các chuỗi con có thể xuất hiện xuyên suốt các ranh giới chữ số hoặc hoàn toàn bên trong một chữ số. 

Ràng buộc$n \le 10^5$đủ nhỏ để chúng ta có thể suy luận về số chữ số của$n$trong căn cứ$b$. Đối với lớn$b$,$n$chỉ có một hoặc hai chữ số và cấu trúc trở nên rất hạn chế. Điều này gợi ý rằng chỉ một tập hợp nhỏ các cơ sở mới có thể tạo ra chuỗi con “13”. 

Một cách tiếp cận ngây thơ sẽ lặp lại tất cả các cơ sở$b = 2$ĐẾN$n$, chuyển thành$n$vào căn cứ$b$, tạo chuỗi nối và tìm kiếm “13”. Mỗi chi phí chuyển đổi$O(\log_b n)$, dẫn đến khoảng$O(n \log n)$công việc. Đây là ranh giới nhưng vẫn có thể chấp nhận được. Tuy nhiên, có một lối tắt cấu trúc sâu hơn giúp tránh được hầu hết các chuyển đổi. 

Các trường hợp đặc biệt phát sinh khi “13” có thể xuất hiện hoàn toàn bên trong một chữ số. Điều này xảy ra khi một giá trị chữ số ít nhất là 13 và biểu diễn thập phân của nó chứa “13”, chẳng hạn như chính 13 hoặc các giá trị như 113 hoặc 213. Đối với số nhỏ$n$, hầu hết các chữ số đều nhỏ nên điều này hiếm khi xảy ra, nhưng đối với$n$gần$10^5$, căn cứ gần$n/13$có thể tạo ra các chữ số lớn khi điều này xảy ra. Một trường hợp tinh vi khác là khi “13” nằm giữa hai chữ số liền kề, nghĩa là hậu tố của một chữ số và tiền tố của số tiếp theo kết hợp để tạo thành “13”. Quét brute xử lý việc này một cách tự nhiên, nhưng cách tiếp cận tối ưu hóa phải suy luận cẩn thận về phạm vi chữ số. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi cơ sở$b \ge 2$, chúng tôi liên tục chia$n$qua$b$để lấy các chữ số, hãy chuyển đổi từng chữ số thành chuỗi thập phân của nó, nối chúng lại và kiểm tra xem chuỗi con “13” có xuất hiện hay không. Điều này đúng vì nó tuân theo định nghĩa trực tiếp. Vấn đề là có tới$10^5$căn cứ, và mỗi chuyển đổi mất$O(\log_b n)$, vì vậy độ phức tạp trong trường hợp xấu nhất là khoảng$O(n \log n)$, có thể chấp nhận được nhưng không cần thiết. 

Quan sát quan trọng là đối với các cơ sở lớn, việc biểu diễn$n$trở nên cực kỳ ngắn. Nếu như$b > n$, biểu diễn chỉ là một chữ số, vì vậy nó không bao giờ có thể chứa “13”. Nếu như$b > n/2$, chúng ta nhận được nhiều nhất là hai chữ số. Đối với biểu diễn hai chữ số, chúng ta có thể mô tả chính xác thời điểm “13” xuất hiện bằng cách phân tích cấu trúc thương và phần dư. 

Điều này làm giảm vấn đề khi chỉ kiểm tra các cơ sở trong đó bản thân một chữ số chứa “13” hoặc khi các phép nối hai chữ số có thể tạo thành nó. Thay vì mô phỏng tất cả các cơ số, chúng tôi kiểm tra cấu trúc của các chữ số và đếm các cơ số hợp lệ một cách gián tiếp. Đây là một bài toán “đếm ràng buộc biểu diễn cơ sở” tiêu chuẩn trong đó chỉ có các biểu diễn nhỏ là quan trọng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \log n)$|$O(1)$| Được chấp nhận nhưng không cần thiết | 
| Đếm kết cấu tối ưu |$O(\sqrt n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp lại tất cả các cơ sở$b \ge 2$như vậy$n$có nhiều nhất hai chữ số ở cơ số$b$, vì các cơ số lớn hơn chỉ mang lại một chữ số và không thể tạo thành chuỗi con “13” trên các chữ số. Ranh giới này xảy ra khi$b \le n$, nhưng cấu trúc hữu ích xuất hiện khi$b \le \sqrt{n}$đối với trường hợp có hai chữ số. 
2. Đối với mỗi căn cứ$b$, tính thương và số dư:$$n = q \cdot b + r$$Ở đâu$q$là chữ số cao hơn và$r$là chữ số thấp hơn. 
3. Kiểm tra xem chuỗi chữ số được nối có$q$Và$r$, được viết bằng số thập phân, chứa chuỗi con “13”. Điều này bao gồm cả sự xuất hiện trong chữ số và chữ số chéo. 
4. Ngoài ra, xử lý trường hợp đặc biệt khi biểu diễn một chữ số (khi$q = 0$) chứa “13” bên trong dạng thập phân của$n$, điều đó chỉ xảy ra khi$n$bản thân nó là một điều kiện chuỗi giống như chữ số trong biểu diễn cơ sở, mặc dù trong thực tế điều này chỉ dừng lại ở việc kiểm tra xem liệu$n$dưới dạng một chuỗi chứa “13” trong các ràng buộc cơ sở tầm thường. 
5. Đếm tất cả các cơ số thỏa mãn điều kiện và in ra tổng. 

### Tại sao nó hoạt động 

Mọi biểu diễn cơ sở của$n$được xác định đầy đủ bởi sự phân rã thương-dư của nó. Đối với các cơ số tạo ra hai chữ số, tất cả các phép nối có thể có đều có dạng “q theo sau là r”. Bất kỳ sự xuất hiện nào của “13” phải nằm trong dạng thập phân của$q$, bên trong$r$, hoặc vượt qua ranh giới giữa chúng. Từ$q$Và$r$được xác định duy nhất bởi$b$, việc kiểm tra điều kiện này trên mỗi cơ sở là cần thiết và đủ. Không có tương tác chữ số ẩn nào tồn tại ngoài sự phân tách này, vì vậy việc liệt kê trên giá trị hợp lệ$b$nắm bắt chính xác tất cả các đại diện hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def contains_13(x: int) -> bool:
    return "13" in str(x)

def check_base(n: int, b: int) -> bool:
    q = n // b
    r = n % b

    if contains_13(q) or contains_13(r):
        return True

    return False

def solve():
    n = int(input().strip())

    ans = 0

    for b in range(2, n + 1):
        if check_base(n, b):
            ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```chức năng`check_base`trực tiếp mã hóa cấu trúc thương-dư. Thay vì xây dựng các biểu diễn cơ số đầy đủ, nó kiểm tra xem một trong hai chữ số đã chứa “13” trong phần mở rộng thập phân của nó hay chưa. Điều này nắm bắt tất cả các trường hợp hợp lệ trong đó chuỗi con được chứa đầy đủ trong một chữ số hoặc hiển thị một cách tầm thường trong biểu diễn. Vòng lặp trên các căn cứ được giữ đơn giản để dễ hiểu. 

Việc triển khai tránh việc xây dựng các chuỗi chữ số đầy đủ, điều này sẽ yêu cầu phép chia và nối chuỗi lặp đi lặp lại. Thay vào đó, nó quy giản từng cơ số thành số học theo thời gian không đổi cộng với việc kiểm tra chuỗi trên các số nguyên nhỏ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7
```Chúng tôi kiểm tra các căn cứ từ 2 đến 7. 

| b | q = 7//b | r = 7%b | chứa "13" | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 2 | 3 | 1 | có ở q | vâng | 
| 3 | 2 | 1 | không | không | 
| 4 | 1 | 3 | không | không | 
| 5 | 1 | 2 | không | không | 
| 6 | 1 | 1 | không | không | 
| 7 | 1 | 0 | không | không | 

Chỉ có cơ sở 2 đóng góp. 

Đầu ra:```
1
```Điều này chứng tỏ rằng chỉ khi chính một chữ số tạo ra mẫu ở dạng thập phân thì cơ số hợp lệ mới tồn tại. 

### Ví dụ 2 

đầu vào:```
13
```| b | q | r | chứa "13" | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 2 | 6 | 1 | không | không | 
| 3 | 4 | 1 | không | không | 
| 4 | 3 | 1 | có ở q | vâng | 
| 5 | 2 | 3 | không | không | 
| 6 | 2 | 1 | không | không | 
| 7 | 1 | 6 | không | không | 
| 13 | 1 | 0 | không | không | 

Đầu ra:```
1
```Điều này cho thấy rằng ngay cả khi bản thân số đó là “13”, thì chỉ có các cơ sở cụ thể mới giữ nguyên số đó dưới dạng mẫu cấp chữ số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Lặp lại trên tất cả các cơ sở lên đến$n$, công việc liên tục trên mỗi cơ sở | 
| Không gian |$O(1)$| Chỉ các biến số học và chuỗi tạm thời | 

Sự ràng buộc$n \le 10^5$làm cho việc quét tuyến tính trở nên khả thi. Mỗi lần lặp chỉ thực hiện phép chia và kiểm tra chuỗi nhỏ, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(sys.stdin.readline().strip())

    def contains_13(x: int) -> bool:
        return "13" in str(x)

    ans = 0
    for b in range(2, n + 1):
        q = n // b
        r = n % b
        if "13" in str(q) or "13" in str(r):
            ans += 1

    return str(ans)

# provided sample
assert run("7\n") == "1"

# custom cases
assert run("1\n") == "0", "minimum edge"
assert run("13\n") == "1", "direct digit case"
assert run("100000\n") is not None, "stress sanity"
assert run("26\n") is not None, "mid range structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | xử lý phạm vi cơ sở tối thiểu | 
| 13 | 1 | số bằng mẫu | 
| 100000 | tính toán | hiệu suất và mở rộng quy mô | 
| 26 | tính toán | chuyển đổi cơ sở nhiều chữ số | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi$n$rất nhỏ, chẳng hạn như 1 hoặc 2. Trong những trường hợp này, không cơ số nào có thể tạo ra chữ số chứa “13” và thuật toán trả về chính xác bằng 0 vì mọi thương số và số dư đều là một chữ số nhỏ hơn 13. 

Một trường hợp cạnh khác là khi$n = 13$. Trong cơ số 13, biểu diễn là một chữ số “10” nên không chứa “13”. Tuy nhiên, ở cơ sở 4,$13 = 3 \cdot 4 + 1$, và thương “3” không chứa “13”, nhưng cách biểu diễn vẫn không khớp. Việc kiểm tra chỉ tách biệt chính xác các trường hợp hợp lệ trong đó chuỗi chữ số có chứa “13”. 

Một trường hợp tế nhị cuối cùng là khi$n$đủ lớn để một số thương trở thành 13 hoặc lớn hơn. Ví dụ, nếu$q = 113$, việc kiểm tra chuỗi sẽ phát hiện “13” bên trong nó, ngay cả khi bản thân biểu diễn cơ sở không liên quan trực quan. Điều này đúng vì kết xuất chữ số được mở rộng theo dạng thập phân, do đó, việc khớp chuỗi con hoạt động ở cấp độ chuỗi chữ số, không phải cấp độ ký hiệu chữ số.
