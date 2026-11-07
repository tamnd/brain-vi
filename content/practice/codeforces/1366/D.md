---
title: "CF 1366D - Hai ước số"
description: "Mỗi giá trị đầu vào có thể được xem dưới dạng một hộp được gắn nhãn một số nguyên và với mỗi hộp chúng ta phải chọn hai ước số không tầm thường của số đó."
date: "2026-06-16T11:57:29+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1366
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 89 (Rated for Div. 2)"
rating: 2000
weight: 1366
solve_time_s: 359
verified: false
draft: false
---

[CF 1366D - Hai Ước số](https://codeforces.com/problemset/problem/1366/D) 

**Xếp hạng:** 2000 
**Tags:** xây dựng thuật toán, toán, lý thuyết số 
**Thời gian giải:** 5 phút 59 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi giá trị đầu vào có thể được xem dưới dạng một hộp được gắn nhãn một số nguyên và với mỗi hộp chúng ta phải chọn hai ước số không tầm thường của số đó. Các ước được chọn đều phải lớn hơn một, phải chia số một cách chính xác và tổng của chúng phải “không tương thích” với số ban đầu theo nghĩa là không có thừa số nguyên tố nào của số ban đầu được phép chia số đó. 

Đầu ra sẽ cung cấp một cặp ước số như vậy cho mọi số hoặc tuyên bố không thành công với -1 -1. Khó khăn chính là ràng buộc không nằm ở khả năng chia hết của các ước đã chọn mà là về mối quan hệ giữa tổng của chúng và cấu trúc nguyên tố đầy đủ của số. 

Các ràng buộc rất lớn: tối đa 5×10^5 số, mỗi số lớn bằng 10^7. Điều này loại trừ mọi hệ số hóa cho mỗi số bằng phép chia thử. Ngay cả cách tiếp cận O(n√a_i) cũng sẽ bao hàm khoảng 10^10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn thời gian. Cấu trúc gợi ý rõ ràng một bước tiền xử lý trên toàn bộ phạm vi giá trị, tự nhiên nhất là thứ gì đó giống như sàng cho phép truy vấn hệ số không đổi hoặc gần như không đổi trên mỗi số. 

Trường hợp cạnh tinh tế xuất hiện khi số đó là lũy thừa nguyên tố. Ví dụ, đối với 8, mọi ước số lớn hơn một vẫn chỉ bao gồm cùng một số nguyên tố, do đó, bất kỳ tổng nào của hai ước số đó vẫn chia hết cho số nguyên tố đó, điều này ngay lập tức vi phạm điều kiện gcd. Trong những trường hợp như vậy, không có cặp hợp lệ nào tồn tại. 

Một trường hợp khác là khi số có nhiều số nguyên tố riêng biệt nhưng một trong số chúng chỉ xuất hiện một lần trong một lựa chọn phân tích nhân tử cụ thể. Cách tiếp cận bất cẩn khi chọn các ước số tùy ý có thể thất bại vì tổng có thể vô tình kế thừa một thừa số nguyên tố chung với số ban đầu ngay cả khi cả hai ước số đều hợp lệ riêng lẻ. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ kiểm tra tất cả các cặp ước số cho mỗi số. Đối với một giá trị duy nhất a_i, chúng ta có thể liệt kê tất cả các ước số và kiểm tra tất cả các cặp, kiểm tra xem gcd(d1 + d2, a_i) có bằng 1 hay không. Trong trường hợp xấu nhất, các số như 10^7 có thể có hàng nghìn ước số và trên các truy vấn 5×10^5, điều này trở nên không khả thi về mặt tính toán. Ngay cả việc liệt kê các ước số cũng đòi hỏi phải phân tích nhân tử, điều này vốn đã chi phối độ phức tạp. 

Quan sát quan trọng là chúng ta không cần các ước số tùy ý. Chỉ cần xây dựng một cặp có cấu trúc rất cụ thể bắt nguồn từ hệ số nguyên tố của số là đủ. Viết a_i dưới dạng p^k · b trong đó p là thừa số nguyên tố nhỏ nhất của a_i và p không chia hết b. Nếu b bằng 1 thì số đó là lũy thừa nguyên tố thuần túy và không tồn tại nghiệm nào. Mặt khác, việc chọn d1 = p^k và d2 = b đảm bảo cả hai đều là ước số hợp lệ lớn hơn 1 và điều kiện gcd được giữ do đối số mâu thuẫn về cấu trúc trên các thừa số nguyên tố chung. 

Điều này làm giảm vấn đề đối với các truy vấn phân tích nhân tố nhanh, có thể được trả lời bằng cách sử dụng sàng hệ số nguyên tố nhỏ nhất được tính toán trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · d^2 + chi phí nhân tố hóa) | O(1) thêm | Quá chậm | 
| Tối ưu | O(N log log N + n log a_i) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Tiền xử lý và logic xây dựng

1. Xây dựng một mảng các thừa số nguyên tố nhỏ nhất cho mọi số nguyên lên tới 10^7. Điều này cho phép chúng ta phân tích bất kỳ số nào một cách nhanh chóng bằng cách chia liên tục cho số nguyên tố nhỏ nhất của nó. 
2. Với mỗi số a_i, tìm ra thừa số nguyên tố p nhỏ nhất của nó. Điều này đảm bảo rằng p là số nguyên tố đầu tiên trong hệ số hóa của nó và có thể được tách biệt một cách rõ ràng. 
3. Trích xuất toàn bộ lũy thừa của p từ a_i bằng cách chia liên tục khi chia hết. Điều này tạo ra p^k và giá trị còn lại b không chia hết cho p. 
4. Nếu b bằng một thì số đó là lũy thừa nguyên tố và không tồn tại phân tách hợp lệ, do đó xuất ra -1 -1. 
5. Ngược lại, đặt d1 = p^k và d2 = b. Cả hai đều được đảm bảo lớn hơn một và đều chia a_i. 
6. Xuất ra cặp đã xây dựng. 

### Tại sao nó hoạt động 

Đặt a_i = p^k · b trong đó p không chia hết b. Giả sử một số nguyên tố q chia cả d1 + d2 và a_i. Khi đó q phải chia p hoặc b vì đây là nguồn số nguyên tố duy nhất trong phân tích nhân tử. Nếu q = p thì p chia hết p^k + b, ngụ ý p chia hết b, mâu thuẫn với cách xây dựng. Nếu q chia hết cho b thì q chia hết cho cả b và p^k + b, ngụ ý q chia hết p^k, điều này là không thể vì p không chia hết cho q. Điều này loại bỏ tất cả các số nguyên tố chung có thể có, vì vậy gcd phải là một. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXA = 10**7

spf = list(range(MAXA + 1))
for i in range(2, int(MAXA ** 0.5) + 1):
    if spf[i] == i:
        step = i
        start = i * i
        for j in range(start, MAXA + 1, step):
            if spf[j] == j:
                spf[j] = i

def solve():
    n = int(input())
    arr = list(map(int, input().split()))

    out1 = []
    out2 = []

    for x in arr:
        p = spf[x]

        d1 = 1
        while x % p == 0:
            x //= p
            d1 *= p

        d2 = x

        if d2 == 1:
            out1.append("-1")
            out2.append("-1")
        else:
            out1.append(str(d1))
            out2.append(str(d2))

    print(" ".join(out1))
    print(" ".join(out2))

if __name__ == "__main__":
    solve()
```Sàng tính toán trước các thừa số nguyên tố nhỏ nhất để mỗi số có thể được phân tách theo thời gian logarit tương ứng với kích thước của nó. Vòng lặp bên trong trích xuất toàn bộ lũy thừa của số nguyên tố nhỏ nhất, đảm bảo d1 là ước số công suất nguyên tố rõ ràng và d2 chứa cấu trúc còn lại. 

Chi tiết triển khai chính là x được giảm triệt để trong khi xây dựng d1. Điều này đảm bảo rằng d2 chính xác là thừa số bù mà không cần nhân thêm hoặc theo dõi thừa số. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:```
10
2 3 4 5 6 7 8 9 10 24
```Chúng tôi theo dõi một vài giá trị đại diện. 

| a_i | p = spf | xây dựng d1 | còn lại d2 | kết quả | 
| --- | --- | --- | --- | --- | 
| 4 | 2 | 2^2 = 4 | 1 | -1 -1 | 
| 6 | 2 | 2 | 3 | (2, 3) | 
| 8 | 2 | 2^3 = 8 | 1 | -1 -1 | 
| 24 | 2 | 2^3 = 8 | 3 | (8, 3) | 

Đối với 6, việc chia thành 2 và 3 có tác dụng vì chúng bắt nguồn từ các thừa số nguyên tố riêng biệt. Đối với 24, việc tách toàn bộ lũy thừa của 2 thành 3 và tổng của chúng tránh được bất kỳ thừa số nguyên tố chung nào với 24. 

Điều này chứng tỏ rằng việc xây dựng luôn tách biệt phần đóng góp của số nguyên tố nhỏ nhất khỏi phần còn lại của hệ số hóa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(MAXA log log MAXA + n log a_i) | sàng xây dựng các thừa số nguyên tố nhỏ nhất, mỗi số được phân rã bằng phép chia lặp lại | 
| Không gian | O(MAXA) | lưu trữ cho mảng SPF lên tới 10^7 | 

Quá trình tiền xử lý chiếm ưu thế một lần và mỗi truy vấn được xử lý theo thời gian tỷ lệ thuận với số lượng thừa số nguyên tố được trích xuất, con số này rất nhỏ trong thực tế. Điều này phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    MAXA = 10**7

    spf = list(range(MAXA + 1))
    for i in range(2, int(MAXA ** 0.5) + 1):
        if spf[i] == i:
            for j in range(i * i, MAXA + 1, i):
                if spf[j] == j:
                    spf[j] = i

    n = int(input())
    arr = list(map(int, input().split()))

    res1 = []
    res2 = []

    for x in arr:
        p = spf[x]
        d1 = 1
        while x % p == 0:
            x //= p
            d1 *= p
        d2 = x
        if d2 == 1:
            res1.append("-1")
            res2.append("-1")
        else:
            res1.append(str(d1))
            res2.append(str(d2))

    print(" ".join(res1))
    print(" ".join(res2))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

assert run("1\n2\n") == "-1\n-1"
assert run("3\n4 6 8\n") in ["-1 -1\n2 3\n-1 -1", "-1 -1\n3 2\n-1 -1"]
assert run("1\n6\n") in ["2\n3", "3\n2"]
assert run("1\n24\n") in ["8\n3", "3\n8"]
assert run("2\n6 10\n") in ["2 2\n3 5", "2 2\n5 3"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 | -1 -1 | xử lý thủ tướng | 
| 3 4 6 8 | hỗn hợp | sức mạnh tổng hợp và sức mạnh chính | 
| 1 6 | 2 3 | chia nhân tố cơ bản | 
| 1 24 | 8 3 | quyền hạn cao hơn | 
| 2 6 10 | chia tách hợp lệ | tính nhất quán của nhiều truy vấn | 

## Vỏ cạnh 

Đối với lũy thừa nguyên tố như 16, thuật toán liên tục trích xuất p = 2 cho đến khi d1 trở thành 16 và d2 trở thành 1. Điều này gây ra tình trạng lỗi ngay lập tức, loại bỏ chính xác đầu vào vì tất cả các ước số được giới hạn trong một số nguyên tố duy nhất. 

Đối với một số như 10, phép phân tách mang lại p = 2, d1 = 2, d2 = 5. Tổng là 7, không chia thừa số nguyên tố nào với 10, xác nhận tính đúng đắn mặc dù cả hai ước riêng lẻ đều là số nguyên tố. 

Đối với một số có nhiều thừa số chẳng hạn như 12, việc trích toàn bộ lũy thừa của 2 sẽ tạo ra d1 = 4 và d2 = 3. Mặc dù tồn tại các cách phân tách hợp lệ khác, cách xây dựng này luôn tránh các số nguyên tố chung trong tổng bằng cách tách thành phần nguyên tố nhỏ nhất khỏi phần còn lại của hệ số hóa.
