---
title: "CF 105999B - Big Mod"
description: "Chúng ta được cung cấp nhiều truy vấn, mỗi truy vấn mô tả một số cơ sở, một số mũ có thể cực lớn (lớn đến mức không thể vừa với các kiểu số nguyên tiêu chuẩn) và một mô đun."
date: "2026-06-25T13:21:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105999
codeforces_index: "B"
codeforces_contest_name: "La Salle-Pui Ching Programming Challenge \u57f9\u6b63\u5587\u6c99\u7de8\u7a0b\u6311\u6230\u8cfd 2024"
rating: 0
weight: 105999
solve_time_s: 50
verified: true
draft: false
---

[CF 105999B - Big Mod](https://codeforces.com/problemset/problem/105999/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp nhiều truy vấn, mỗi truy vấn mô tả một số cơ sở, một số mũ có thể cực lớn (lớn đến mức không thể vừa với các kiểu số nguyên tiêu chuẩn) và một mô đun. Đối với mỗi truy vấn, chúng ta cần tính giá trị của cơ số được nâng lên theo số mũ đó, lấy modulo của mô đun đã cho. 

Khó khăn chính không phải là bản thân số học mô-đun mà là cách biểu diễn số mũ. Thay vì là một số nguyên thông thường, nó được cung cấp dưới dạng một chuỗi các chữ số thập phân, do đó nó có thể có hàng nghìn hoặc thậm chí hàng chục nghìn chữ số. Điều đó ngay lập tức loại trừ việc phân tích cú pháp thành bất kỳ kiểu số tích hợp nào. 

Từ góc độ ràng buộc, cơ sở và mô đun nằm trong giới hạn số nguyên điển hình, do đó số học 32 bit hoặc 64 bit tiêu chuẩn là đủ khi chúng ta đang làm việc theo mô đun m. Độ dài số mũ chi phối độ phức tạp. Bất kỳ giải pháp nào cố gắng mở rộng số mũ hoặc mô phỏng trực tiếp phép nhân lặp lại sẽ thất bại vì ngay cả một số mũ cũng có thể biểu thị một số lượng lớn các phép toán. 

Vỏ có cạnh tinh tế đến từ các mô-đun nhỏ. Nếu mô đun là 1 thì mọi kết quả phải bằng 0 bất kể cơ số và số mũ, đồng thời việc triển khai bất cẩn làm bỏ qua việc giảm mô đun ở các bước trung gian vẫn có thể tạo ra các phép tính trông đúng nhưng không cần thiết. Một trường hợp cạnh khác là khi số mũ bằng 0, bao gồm cả chuỗi "0". Trong trường hợp đó, kết quả phải là 1 modulo m, ngay cả khi cơ số bằng 0. Việc triển khai đơn giản diễn giải số mũ dưới dạng số nguyên và cách viết hoa đặc biệt không nhất quán có thể trả về 0 không chính xác. 

Ví dụ: nếu đầu vào là cơ số 5, số mũ "0", mô đun 7, thì câu trả lời đúng là 1. Cách tiếp cận nhân dựa trên vòng lặp đơn giản sẽ không bao giờ vào vòng lặp và có thể trả về 0 không chính xác nếu khởi tạo kém. 

Tương tự, nếu cơ số là 0 và số mũ là "0", định nghĩa thông thường về mặt toán học trong các bài toán lập trình là 0^0 = 1, do đó đầu ra vẫn phải là 1 modulo m. Thay vào đó, quy trình lũy thừa bất cẩn có thể trả về 0 hoặc bị lỗi tùy thuộc vào quá trình khởi tạo. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: hiểu số mũ là một số nguyên và nhân cơ số với chính nó nhiều lần đồng thời lấy mô đun ở mỗi bước. Về nguyên tắc, điều này đúng vì số học mô-đun bảo toàn cấu trúc phép nhân, do đó việc rút gọn ở mỗi bước không làm thay đổi kết quả cuối cùng. 

Sự cố xuất hiện khi số mũ không thể biểu diễn dưới dạng số nguyên. Nếu số mũ có k chữ số thì giá trị của nó có thể lớn tới 10^k. Ngay cả với k = 100000, số phép nhân cần thiết sẽ hoàn toàn không thể thực hiện được. 

Một kỹ thuật lũy thừa nhanh tiêu chuẩn giải quyết vấn đề này khi số mũ nhỏ: lũy thừa nhị phân làm giảm số phép nhân xuống O(log b). Tuy nhiên, phép lũy thừa nhị phân vẫn giả định rằng chúng ta có thể đọc b dưới dạng số nguyên máy, điều mà chúng ta không thể làm được ở đây. 

Quan sát quan trọng là chúng ta thực sự không cần giá trị số của số mũ tại bất kỳ điểm nào. Chúng ta chỉ cần xử lý chữ số hiệu ứng của nó theo chữ số trong cơ số 10. Nếu chúng ta đã biết a^x mod m cho một số tiền tố của số mũ, thì việc thêm một chữ số d sẽ biến đổi số mũ từ x thành 10x + d. Điều này mang lại sự tái diễn trực tiếp: 

a^(10x + d) = (a^x)^10 * a^d 

Nhận dạng này cho phép chúng tôi xử lý chuỗi số mũ từ trái sang phải trong khi duy trì giá trị công suất hiện tại theo mô đun. Mỗi chữ số mở rộng số mũ trong cơ số 10 và chúng tôi cập nhật kết quả bằng cách sử dụng lũy ​​thừa mô-đun chỉ trên các số mũ nhỏ (các chữ số từ 0 đến 9 và lũy thừa cố định 10). 

Điều này loại bỏ hoàn toàn sự phụ thuộc vào độ lớn của số mũ và giảm vấn đề thành các phép toán O (số chữ số × log m). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(b) | O(1) | Quá chậm | 
| Tối ưu | O(k log m) | O(1) | Đã chấp nhận |

Ở đây k là số chữ số trong chuỗi số mũ. 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đọc cơ số a, chuỗi số mũ b và mô đun m. Chúng ta xử lý ngay trường hợp đặc biệt trong đó m bằng 1, vì mọi số modulo 1 luôn bằng 0. 
2. Khởi tạo kết quả là 1. Giá trị này biểu thị a^0 khi bắt đầu xử lý chuỗi số mũ. 
3. Lặp qua từng ký tự trong chuỗi số mũ từ trái sang phải. Mỗi ký tự là một chữ số thập phân d. 
4. Trước khi kết hợp chữ số mới, hãy nâng kết quả hiện tại lên modulo lũy thừa 10 m. Điều này tương ứng với việc dịch số mũ một chữ số thập phân sang trái trong cơ số 10. 
5. Nhân giá trị này với a^d modulo m, trong đó d là một chữ số. Vì d nhiều nhất là 9 nên giá trị này có thể được tính nhanh bằng cách sử dụng thủ tục lũy thừa nhanh nhỏ. 
6. Sau khi xử lý tất cả các chữ số, kết quả biểu thị một số mũ được nâng lên theo modulo số mũ đầy đủ m. 

Lý do điều này có hiệu quả là ở mỗi bước chúng tôi duy trì giá trị a^x mod m, trong đó x chính xác là giá trị số của tiền tố của chuỗi số mũ được xử lý cho đến nay. Việc thêm một chữ số d sẽ biến x thành 10x + d và quy tắc cập nhật phản ánh chính xác sự đồng nhất này theo lũy thừa mô-đun. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def mod_pow(a, e, mod):
    res = 1
    a %= mod
    while e > 0:
        if e & 1:
            res = (res * a) % mod
        a = (a * a) % mod
        e >>= 1
    return res

t = int(input())
for _ in range(t):
    a, b, m = input().split()
    a = int(a)
    m = int(m)

    if m == 1:
        print(0)
        continue

    result = 1

    for ch in b:
        digit = ord(ch) - 48
        result = mod_pow(result, 10, m)
        if digit:
            result = (result * mod_pow(a, digit, m)) % m

    print(result)
```Hàm trợ giúp thực hiện phép lũy thừa nhị phân tiêu chuẩn cho số mũ nhỏ, vốn chỉ được gọi với số mũ 10 hoặc nhiều nhất là 9, do đó giá trị của nó không đổi trên mỗi chữ số trong thực tế. 

Vòng lặp chính duy trì giá trị tiến hóa của lũy thừa tiền tố. Sự biến đổi đầu tiên`result = result^10 mod m`là rất quan trọng vì nó làm dịch chuyển cơ số mũ và thiếu bước này là lỗi triển khai phổ biến nhất. Phép nhân với`a^digit`sau đó chèn phần đóng góp của chữ số hiện tại. 

Việc kiểm tra mô đun ngay từ đầu sẽ tránh được hành vi không xác định đối với mô đun 1, nếu không sẽ gây ra phép nhân 0 lặp lại và tính toán không cần thiết. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào trong đó a = 2, số mũ = "13", m = 5. 

Chúng tôi theo dõi cách số mũ được xây dựng theo từng chữ số. 

| Bước | Chữ số | Kết quả trước ca | Sau lũy thừa 10 | Sau khi nhân | Ý nghĩa | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | - | 1 | - | 1 | 2^0 | 
| 1 | 1 | 1 | 1^10 = 1 | 2 | 2^1 | 
| 2 | 3 | 2 | 2^10 = 1024 mod 5 = 4 | 4 * (2^3=8 mod 5=3) = 2 | 2^13 | 

Kết quả cuối cùng là 2. 

Bây giờ hãy xem xét trường hợp có số mũ bằng 0 đứng đầu, a = 3, số mũ = "05", m = 7. 

| Bước | Chữ số | Kết quả trước ca | Sau lũy thừa 10 | Sau khi nhân | Ý nghĩa | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | - | 1 | - | 1 | 3^0 | 
| 0 | 0 | 1 | 1 | 1 | 3^0 | 
| 5 | 5 | 1 | 1^10 = 1 | 3^5 = 5 | 3^5 | 

Dấu vết cho thấy các số 0 đứng đầu không ảnh hưởng đến độ chính xác, vì nhân với a^0 sẽ khiến trạng thái không thay đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k log m) | Mỗi k chữ số kích hoạt một số lũy thừa mô-đun không đổi, mỗi số được thực hiện trong O(log m) | 
| Không gian | O(1) | Chỉ duy trì một số biến cố định | 

Độ dài số mũ chiếm ưu thế, nhưng các phép tính trên mỗi chữ số vẫn mang tính logarit theo kích thước mô đun. Với những ràng buộc điển hình, điều này thoải mái phù hợp trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    
    t = int(sys.stdin.readline())
    
    def mod_pow(a, e, mod):
        res = 1
        a %= mod
        while e > 0:
            if e & 1:
                res = (res * a) % mod
            a = (a * a) % mod
            e >>= 1
        return res

    for _ in range(t):
        a, b, m = sys.stdin.readline().split()
        a = int(a)
        m = int(m)

        if m == 1:
            output.append("0")
            continue

        result = 1
        for ch in b:
            digit = ord(ch) - 48
            result = mod_pow(result, 10, m)
            if digit:
                result = (result * mod_pow(a, digit, m)) % m

        output.append(str(result))

    return "\n".join(output)

# provided samples (illustrative, since actual samples not given)
assert run("1\n2 3 5\n") == "3"

# custom cases
assert run("1\n5 0 7\n") == "1", "0 exponent"
assert run("1\n0 0 7\n") == "1", "0^0 convention"
assert run("1\n10 10 1\n") == "0", "mod 1"
assert run("1\n2 10 1000\n") == str(pow(2,10,1000)), "small validation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 0 7 | 1 | xử lý số mũ bằng 0 | 
| 0 0 7 | 1 | quy ước 0^0 | 
| 10 10 1 | 0 | trường hợp cạnh mô đun | 
| 2 10 1000 | 1024 mod 1000 | tính đúng đắn đối với nguồn điện tích hợp | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là số mũ là "0". Thuật toán khởi tạo kết quả là 1, sau đó xử lý một chữ số 0. Bước cập nhật tính toán kết quả^10 giữ nguyên bằng 1 và nhân với a^0 không thay đổi, do đó đầu ra là 1 bất kể cơ số và mô đun. 

Một trường hợp cạnh khác là cả cơ số và số mũ đều bằng 0. Quá trình tính toán hoạt động tương tự như trên và kết quả cuối cùng vẫn là 1, phù hợp với định nghĩa lập trình thông thường. 

Trường hợp cạnh thứ ba có mô đun bằng 1. Thuật toán ngay lập tức trả về 0 trước khi xử lý các chữ số. Nếu không có sự bảo vệ này, phép lũy thừa mô-đun lặp đi lặp lại sẽ luôn giảm về 0, nhưng chỉ sau khi tính toán không cần thiết. 

Trường hợp cạnh cuối cùng liên quan đến chuỗi số mũ rất dài. Vì mỗi chữ số được xử lý độc lập nên thuật toán không bao giờ cố gắng chuyển đổi số mũ thành loại số, do đó không có nguy cơ tràn hoặc lỗi phân tích cú pháp và thời gian chạy tỷ lệ tuyến tính với số lượng chữ số.
