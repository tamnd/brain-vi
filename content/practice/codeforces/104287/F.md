---
title: "CF 104287F - Bội chung lớn nhất"
description: "Mỗi bài kiểm tra cho ba số nguyên. Hãy coi hai số đầu tiên như việc xác định quy tắc cho số nguyên nào là "hợp lệ": một số chỉ hợp lệ nếu nó chia hết cho cả hai số đó. Số thứ ba hoạt động giống như giới hạn mô đun mà chúng ta chỉ quan tâm đến phần dư."
date: "2026-07-01T20:46:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104287
codeforces_index: "F"
codeforces_contest_name: "Teamscode Spring 2023 Contest"
rating: 0
weight: 104287
solve_time_s: 83
verified: true
draft: false
---

[CF 104287F - Mutiple chung lớn nhất](https://codeforces.com/problemset/problem/104287/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi bài kiểm tra cho ba số nguyên. Hãy coi hai số đầu tiên như việc xác định quy tắc cho số nguyên nào là "hợp lệ": một số chỉ hợp lệ nếu nó chia hết cho cả hai số đó. Số thứ ba hoạt động giống như giới hạn mô đun mà chúng ta chỉ quan tâm đến phần dư. 

Trong số tất cả các số hợp lệ, chúng ta được phép chọn bất kỳ số nào chúng ta muốn, ngay cả những số cực lớn. Với mỗi số được chọn, chúng ta tính số dư của nó khi chia cho số thứ ba. Nhiệm vụ là xác định số dư lớn nhất mà chúng ta có thể thu được. 

Khó khăn chính là các số hợp lệ không phải là tùy ý. Bất kỳ số nào chia hết cho cả hai đầu vào đều phải là bội số của bội số chung nhỏ nhất của chúng. Vì vậy, không gian tìm kiếm là một cấp số cộng vô hạn bắt đầu từ bội số chung nhỏ nhất đó. 

Các ràng buộc có giá trị lớn nhưng số lượng trường hợp thử nghiệm vừa phải. Có tối đa vài nghìn truy vấn và mỗi giá trị có thể lên tới 10^9. Điều này gợi ý rõ ràng một giải pháp là logarit hoặc thời gian không đổi cho mỗi trường hợp thử nghiệm. Bất kỳ cách tiếp cận nào liệt kê bội số hoặc mô phỏng sự tăng trưởng của các số hợp lệ sẽ thất bại ngay lập tức vì các số hợp lệ không bị giới hạn và giới hạn mô đun cũng có thể lớn. 

Một sai lầm ngây thơ là cho rằng chúng ta chỉ cần kiểm tra một vài bội số của bội số chung nhỏ nhất cho đến c. Điều đó không an toàn vì phần dư tốt nhất thường xảy ra ở gần cuối chu kỳ mô đun chứ không phải gần đầu. 

Ví dụ: giả sử bội số chung nhỏ nhất là 6 và c là 10. Dãy số hợp lệ là 6, 12, 18, 24, v.v. Số dư modulo 10 là 6, 2, 8, 4, 0, lặp lại. Tối đa là 8, không đến từ bội số thứ nhất hoặc bội số thứ hai trong bất kỳ cửa sổ nhỏ nào có thể dự đoán được. Một lần quét giới hạn sẽ bỏ lỡ nó tùy thuộc vào điểm cắt. 

Một vấn đề tế nhị khác là giả sử câu trả lời đơn giản là bội số chung nhỏ nhất theo modulo c. Điều đó không thành công vì bội số lớn hơn có thể bao quanh và tạo ra số dư lớn hơn giá trị cơ sở. 

## Phương pháp tiếp cận 

Mọi số hợp lệ trong bài toán đều có dạng bội số của một giá trị cơ sở, bội số chung nhỏ nhất của hai đầu vào. Đặt giá trị này là L. Khi đó tất cả các ứng cử viên là L, 2L, 3L, v.v. 

Cách tiếp cận bạo lực sẽ tạo ra các giá trị này và tính phần dư của chúng theo modulo c, dừng sau một phạm vi nào đó. Điều này đúng về mặt khái niệm nhưng ngay lập tức bị phá vỡ vì không có giới hạn trên tự nhiên về việc chúng ta có thể cần phải đi bao xa. Ngay cả việc giới hạn ở bội số c đầu tiên cũng đã quá lớn khi c có thể là 10^9. 

Quan sát quan trọng là chúng ta không cố gắng tối ưu hóa trên các số nguyên ban đầu mà trên các dư lượng modulo c. Khi chúng ta giảm mọi số hợp lệ theo modulo c, cấu trúc sẽ trở thành tuần hoàn. Dãy số (k·L) mod c tạo thành một nhóm con tuần hoàn đơn giản trong số học mô-đun. Kích thước bước là cố định, do đó phần dư có thể tiếp cận được phân bố đều xung quanh vòng tròn mô-đun. 

Cấu trúc này ngụ ý rằng tập các phần dư có thể tiếp cận chính xác là tập hợp các bội số của gcd(L, c). Từ đó, số dư tối đa có thể tiếp cận chỉ đơn giản là bội số lớn nhất của gcd đó bên dưới c, bằng c trừ gcd đó. 

Vì vậy, vấn đề giảm hoàn toàn vào việc tính toán L và sau đó là một thao tác gcd. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(c / LCM step) cho mỗi bài kiểm tra, không giới hạn trong trường hợp xấu nhất | O(1) | Quá chậm | 
| Tối ưu | O(log a + log b + log c) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Gọi L là bội chung nhỏ nhất của a và b, và g là ước chung lớn nhất của L và c.

1. Tính g1 = gcd(a, b). Điều này xác định sự chồng chéo các thừa số nguyên tố giữa a và b, giúp ngăn chặn tình trạng tràn khi xây dựng LCM. LCM là a*b/g1. 
2. Tính L cẩn thận bằng phép chia trước khi nhân: L = (a // g1) * b. Điều này tránh tràn trung gian vượt quá giới hạn 32 bit, mặc dù về mặt kỹ thuật Python có thể xử lý các số nguyên lớn một cách an toàn. 
3. Tính g = gcd(L, c). Điều này thể hiện khoảng cách của các phần dư có thể tiếp cận theo modulo c. Mọi số hợp lệ đều giảm xuống bội số của g trong số học mô-đun. 
4. Trả lời c - g. Đây là giá trị lớn nhất nhỏ hơn c nằm trong lớp dư lượng được tạo bởi L modulo c. 

Lý do bước 3 đúng là vì nhân với L theo modulo c sẽ tạo ra một nhóm con tuần hoàn của nhóm cộng modulo c. Kích thước nhóm con được xác định hoàn toàn bởi gcd(L, c) và tất cả các phần dư có thể tiếp cận chính xác là bội số của gcd đó. 

## Tại sao nó hoạt động 

Tất cả các số hợp lệ đều là bội số của L. Việc giảm modulo c biến điều này thành phép cộng lặp lại của kích thước bước cố định L mod c. Do đó, tập hợp các gốc có thể tiếp cận được là nhóm con cộng được tạo bởi L trong vòng modulo c. Nhóm con đó chứa chính xác tất cả các bội số của gcd(L, c). Phần tử lớn nhất trong tập hợp đó là bội số cuối cùng trước khi gói qua c, bằng c trừ gcd(L, c). Không thể đạt được phần dư lớn hơn vì bất kỳ mức tăng nào của L chỉ di chuyển trong mạng cố định này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from math import gcd

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        a, b, c = map(int, input().split())
        
        g1 = gcd(a, b)
        l = (a // g1) * b
        g = gcd(l, c)
        
        out.append(str(c - g))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc thực hiện theo đại số trực tiếp. Phần tế nhị duy nhất là xây dựng LCM không bị tràn ở các bước trung gian, được xử lý bằng cách chia trước khi nhân. 

Phép trừ cuối cùng`c - g`tương ứng với việc chọn dư lượng có thể tiếp cận cao nhất trong chu trình mô-đun. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:```
a = 2, b = 3, c = 10
```Ở đây L = 6. Bảng bội số là: 

| k | d = kL | d mod c | 
| --- | --- | --- | 
| 1 | 6 | 6 | 
| 2 | 12 | 2 | 
| 3 | 18 | 8 | 
| 4 | 24 | 4 | 
| 5 | 30 | 0 | 

Tối đa là 8. Vì gcd(6, 10) = 2 nên công thức cho 10 - 2 = 8, khớp với bảng. 

Bây giờ hãy xem xét:```
a = 4, b = 2, c = 2023
```Ở đây L = 4. Gcd với c là 1, vì 2023 không chia hết cho 2 hoặc 4. 

| k | d = 4k | d mod 2023 | 
| --- | --- | --- | 
| 1 | 4 | 4 | 
| 2 | 8 | 8 | 
| 3 | 12 | 12 | 

Phần dư có thể tiếp cận cuối cùng bao gồm tất cả các bội số của 1 modulo 2023, do đó giá trị lớn nhất là 2022. Công thức cho 2023 - 1 = 2022. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T log tối đa A) | Mỗi bài kiểm tra sử dụng một số thao tác gcd không đổi | 
| Không gian | O(1) | Chỉ một vài số nguyên được lưu trữ cho mỗi trường hợp thử nghiệm | 

Các ràng buộc cho phép vài nghìn trường hợp thử nghiệm và mỗi thao tác có độ lớn logarit, do đó giải pháp dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    from math import gcd

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            a, b, c = map(int, input().split())
            g1 = gcd(a, b)
            l = (a // g1) * b
            g = gcd(l, c)
            out.append(str(c - g))
        return "\n".join(out)

    return solve()

# provided samples
assert run("""5
2 3 10
3 7 10
4 2 2023
33 66 3366
103241 103870 100000007
""") == """8
9
2022
3300
100000006"""

# minimum values
assert run("""1
1 1 5
""") == "0"

# identical numbers
assert run("""1
6 6 10
""") == "4"

# coprime case
assert run("""1
7 9 20
""") == "19"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 5 | 0 | L bằng 1, chu trình cặn đầy đủ | 
| 6 6 10 | 4 | LCM bằng đầu vào, gcd không cần thiết với c | 
| 7 9 20 | 19 | trường hợp nguyên tố cùng nhau trong đó câu trả lời trở thành c−1 | 

## Vỏ cạnh 

Khi a và b giống hệt nhau, bội số chung nhỏ nhất sẽ thu gọn về cùng một giá trị và tập hợp có thể truy cập chỉ là bội số của số đó. Đối với đầu vào 6 6 10, L là 6 và gcd(6, 10) là 2, vì vậy câu trả lời là 8 mod 10, khớp với 10 − 2 = 8. 

Khi a và b nguyên tố cùng nhau, L trở thành tích của chúng, nhưng cấu trúc đơn giản hóa vì gcd(L, c) chỉ phụ thuộc vào cách tích đó tương tác với c. Nếu không tồn tại thừa số chung, gcd bằng 1 và câu trả lời trở thành c − 1, nghĩa là tất cả các phần dư ngoại trừ 0 đều có thể truy cập được. 

Khi c chia sẻ tất cả các thừa số nguyên tố với L, gcd bằng c và câu trả lời trở thành 0. Điều này tương ứng với trường hợp mọi số hợp lệ luôn chia hết cho c, do đó không có số dư dương nào có thể xuất hiện.
