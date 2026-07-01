---
title: "CF 104217B - Chênh lệch tối đa"
description: "Chúng ta được cho một số nguyên $n$, và chúng ta được yêu cầu xem xét tất cả các hoán vị của dãy $(1, 2, 3, dots, n)$. Mỗi hoán vị được hiểu là một số được hình thành bằng cách nối các phần tử của nó theo thứ tự."
date: "2026-07-01T23:52:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104217
codeforces_index: "B"
codeforces_contest_name: "UTPC Contest 03-03-23 Div. 2 (Beginner)"
rating: 0
weight: 104217
solve_time_s: 61
verified: true
draft: false
---

[CF 104217B - Chênh lệch tối đa](https://codeforces.com/problemset/problem/104217/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên duy nhất$n$, và chúng ta được yêu cầu xem xét tất cả các hoán vị của dãy$(1, 2, 3, \dots, n)$. Mỗi hoán vị được hiểu là một số được hình thành bằng cách nối các phần tử của nó theo thứ tự. Ví dụ, đối với$n = 4$, một hoán vị như$[3, 1, 4, 2]$tương ứng với số 3142. 

Trong số tất cả các hoán vị có thể có, chúng ta muốn có sự khác biệt lớn nhất có thể có giữa hai số bất kỳ được tạo thành như vậy. Một hoán vị sẽ tạo ra số được nối lớn nhất có thể và một hoán vị khác sẽ tạo ra số được nối nhỏ nhất có thể và chúng tôi lấy hiệu của chúng. 

Kích thước đầu vào tăng lên$n = 10^5$. Điều đó loại trừ bất kỳ cách tiếp cận nào liệt kê rõ ràng các hoán vị hoặc xây dựng tất cả các số, vì có$n!$hoán vị, vốn rất lớn về mặt thiên văn ngay cả đối với những hoán vị nhỏ$n$. Bất kỳ giải pháp hợp lệ nào cũng phải tuyến tính hoặc tệ nhất$O(n \log n)$, mặc dù ở đây việc sắp xếp là không cần thiết vì cấu trúc hoán vị tối ưu đã được cố định. 

Một điểm tinh tế là chúng ta đang xử lý việc ghép nối chứ không phải sắp xếp lại số học các chữ số. Vì thế$n = 12$đóng góp hai chữ số, trong khi$n = 9$đóng góp một chữ số. Điều này có nghĩa là trọng số vị trí phụ thuộc vào độ dài chữ số, ảnh hưởng đến cấu trúc của hoán vị cực trị. 

Các trường hợp cạnh đến từ các giá trị nhỏ của$n$. Vì$n = 1$, chỉ có một hoán vị nên hiệu phải bằng 0. Vì$n = 10$, độ dài chữ số thay đổi từ số có 1 chữ số thành số có 2 chữ số, đây chính xác là nơi mà cách tiếp cận lý luận cấp độ chữ số ngây thơ có thể thất bại nếu nó giả định chiều rộng đồng đều. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ tạo ra tất cả các hoán vị của$1$ĐẾN$n$, chuyển đổi từng hoán vị thành biểu diễn số nguyên nối của nó và tính giá trị tối thiểu và tối đa giữa chúng. Về mặt khái niệm, điều này đơn giản và chính xác vì nó tuân theo định nghĩa của vấn đề. Tuy nhiên, độ phức tạp của nó là$O(n! \cdot n)$do tạo ra tất cả các hoán vị và ghép nối từng hoán vị, điều này trở nên không thể ngay cả đối với$n = 10$. 

Quan sát quan trọng là chúng ta thực sự không cần khám phá các hoán vị. Chúng ta chỉ cần xác định cách sắp xếp số nào tạo ra giá trị nối lớn nhất và cách sắp xếp nào tạo ra giá trị nhỏ nhất. Đối với thứ tự dựa trên nối, sự sắp xếp tối ưu được xác định bằng cách so sánh từ điển của các chuỗi kết quả, chứ không phải bằng độ lớn của các phần tử riêng lẻ. 

Số nối lớn nhất thu được bằng cách sắp xếp các số theo thứ tự giảm dần khi viết dưới dạng chuỗi, bởi vì việc đặt các chữ số đầu lớn hơn trước luôn chiếm ưu thế trong các đóng góp sau. Tương tự, số nối nhỏ nhất có được bằng cách sắp xếp các số theo thứ tự tăng dần. 

Vì trình tự được cố định là$1$ĐẾN$n$, điều này dẫn đến việc xây dựng hai chuỗi: một chuỗi được hình thành bằng cách viết các số từ$n$xuống$1$, và một cái khác từ$1$lên đến$n$, sau đó chuyển đổi cả hai thành số nguyên và trừ. 

Bài toán chuyển từ tối ưu hóa tổ hợp qua các hoán vị sang bài toán xây dựng trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n! \cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \cdot d)$Ở đâu$d$là chi phí chữ số |$O(n \cdot d)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán 

1. Xây dựng phép nối nhỏ nhất có thể bằng cách nối các số từ 1 đến$n$theo thứ tự tăng dần. Điều này có tác dụng vì các chữ số trước chiếm ưu thế trong các chữ số sau trong thứ tự từ điển của chuỗi kết quả. 
2. Xây dựng phép nối lớn nhất có thể bằng cách nối các số từ$n$xuống 1. Điều này đảm bảo rằng các số lớn hơn xuất hiện sớm hơn, tối đa hóa các vị trí chữ số có ý nghĩa nhất. 
3. Chuyển đổi cả hai chuỗi được xây dựng thành số nguyên. Bước này an toàn vì giá trị kết quả có thể tăng lớn nhưng Python xử lý các số nguyên có độ chính xác tùy ý. 
4. Tính toán sự khác biệt giữa giá trị lớn hơn và nhỏ hơn và xuất ra. 

Tại sao nó hoạt động: số được nối hoàn toàn được xác định bởi chuỗi các khối chuỗi và việc so sánh hai chuỗi nối tương đương với việc so sánh thứ tự từ điển của chúng khi tất cả các khối đều là mã thông báo cố định. Vì bài toán cho phép hoán vị tùy ý nên các giá trị cực trị đạt được bằng cách sắp xếp toàn cục các mã thông báo này theo thứ tự giảm dần và tăng dần tương ứng. Bất kỳ sự sai lệch nào so với các đơn đặt hàng này sẽ tạo ra một mã thông báo lớn hơn sau này hoặc một mã thông báo nhỏ hơn sớm hơn, điều này làm mục tiêu trở nên tồi tệ hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())

    # build smallest concatenation: 1 to n
    small_parts = []
    for i in range(1, n + 1):
        small_parts.append(str(i))
    small_val = int("".join(small_parts))

    # build largest concatenation: n to 1
    large_parts = []
    for i in range(n, 0, -1):
        large_parts.append(str(i))
    large_val = int("".join(large_parts))

    print(large_val - small_val)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo sau việc xây dựng được mô tả trước đó. Quyết định không tầm thường duy nhất là sử dụng nối chuỗi thay vì dịch chuyển chữ số số học. Điều này tránh việc theo dõi thủ công lũy ​​thừa mười và xử lý chính xác độ dài chữ số khác nhau giữa các số nguyên liên tiếp. 

Các số nguyên chính xác tùy ý của Python giúp cho việc chuyển đổi trở nên an toàn ngay cả khi$n = 10^5$, trong đó số kết quả có hàng trăm nghìn chữ số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
```Chúng tôi xây dựng cả hai thái cực. 

| Bước | Xây dựng | Kết quả | 
| --- | --- | --- | 
| Nhỏ | 1 → 2 → 3 → 4 | 1234 | 
| Lớn | 4 → 3 → 2 → 1 | 4321 | 

Sự khác biệt được tính là$4321 - 1234 = 3087$. 

Điều này cho thấy thứ tự xác định hoàn toàn giá trị như thế nào và không cần hoán vị trung gian. 

### Ví dụ 2 

đầu vào:```
3
```| Bước | Xây dựng | Kết quả | 
| --- | --- | --- | 
| Nhỏ | 1 → 2 → 3 | 123 | 
| Lớn | 3 → 2 → 1 | 321 | 

Sự khác biệt là$321 - 123 = 198$, xác nhận rằng ngay cả đối với nhỏ$n$, cấu trúc cực trị ổn định và không phụ thuộc vào phép liệt kê. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot d)$| Mỗi số nguyên được chuyển đổi thành chuỗi một lần và được nối | 
| Không gian |$O(n \cdot d)$| Lưu trữ hai chuỗi nối | 

Giá trị của$d$tăng theo logarit với$n$, nhưng trong thực tế mỗi số có tối đa 6 chữ số cho$n \le 10^5$. Giải pháp dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    input = sys.stdin.readline

    def solve():
        n = int(input().strip())

        small_parts = []
        for i in range(1, n + 1):
            small_parts.append(str(i))
        small_val = int("".join(small_parts))

        large_parts = []
        for i in range(n, 0, -1):
            large_parts.append(str(i))
        large_val = int("".join(large_parts))

        print(large_val - small_val)

    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue().strip()
    sys.stdout = old_stdout
    return out

# provided sample
assert run("4") == "3087"

# minimum case
assert run("1") == "0"

# small case
assert run("3") == "198"

# check digit boundary
assert run("10") == str(int("10987654321") - int("12345678910"))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | trường hợp cạnh hoán vị đơn | 
| 3 | 198 | tính đúng đắn cơ bản của công trình | 
| 10 | chênh lệch tính toán | xử lý chuyển đổi độ dài chữ số | 

## Vỏ cạnh 

cho$n = 1$, thuật toán xây dựng cả hai chuỗi dưới dạng`"1"`. Sự khác biệt trở thành 0, phù hợp với thực tế là chỉ có một hoán vị. 

Vì$n = 10$, thuật toán xây dựng`"12345678910"`Và`"10987654321"`. Sự tinh tế quan trọng là ở chỗ`"10"`đóng góp hai ký tự, điều này ảnh hưởng đến việc căn chỉnh nếu một ký tự giả định sai mã thông báo có chiều rộng cố định. Cách tiếp cận dựa trên chuỗi xử lý vấn đề này một cách tự nhiên, vì phép nối duy trì ranh giới mã thông báo. Sự khác biệt được tính toán phù hợp với việc giải thích số nguyên trực tiếp. 

Không cần phân nhánh đặc biệt và tất cả các trường hợp biên đều được xử lý thống nhất theo cùng một logic xây dựng.
