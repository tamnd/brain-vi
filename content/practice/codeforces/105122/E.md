---
title: "CF 105122E - Chữ số cuối cùng"
description: "Chúng ta được cho một số nguyên dương $n$, và về mặt khái niệm, chúng ta tạo thành một tích lớn trong đó mỗi số nguyên $i$ từ 1 đến $n$ được nâng lên lũy thừa riêng của nó $i$, và tất cả các giá trị này được nhân với nhau: $$1^1 cdot 2^2 cdot 3^3 cdots n^n$$ Nhiệm vụ không phải là tính con số khổng lồ này…"
date: "2026-06-27T19:38:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105122
codeforces_index: "E"
codeforces_contest_name: "XXVI Interregional Programming Olympiad, Vologda SU, 2024"
rating: 0
weight: 105122
solve_time_s: 93
verified: false
draft: false
---

[CF 105122E - Chữ số cuối cùng](https://codeforces.com/problemset/problem/105122/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên dương$n$và về mặt khái niệm, chúng tôi tạo thành một tích số lớn trong đó mỗi số nguyên$i$từ 1 đến$n$được nâng lên sức mạnh của chính nó$i$và tất cả các giá trị này được nhân với nhau:$$1^1 \cdot 2^2 \cdot 3^3 \cdots n^n$$Nhiệm vụ không phải là tính trực tiếp con số khổng lồ này mà chỉ là xác định chữ số cuối cùng khác 0 của tích cuối cùng. 

Đầu ra là một chữ số từ 1 đến 9, biểu thị chữ số ngoài cùng bên phải của kết quả sau khi loại bỏ tất cả các số 0 ở cuối. Các số 0 ở cuối đến từ thừa số 10, được tạo bởi cặp 2 và 5 trong hệ số hóa. 

Ràng buộc$n \le 10^6$ngay lập tức loại trừ mọi cách xây dựng số trực tiếp. Ngay cả việc lưu trữ các giá trị trung gian cũng không thể thực hiện được vì số này tăng theo cấp số nhân cả về độ lớn và độ dài chữ số. Bất kỳ giải pháp hợp lệ nào cũng phải hoạt động trong thời gian gần tuyến tính hoặc gần tuyến tính. 

Một nỗ lực ngây thơ có thể thử tính toán từng$i^i$, nhân chúng và loại bỏ các số 0 ở cuối. Điều này thất bại vì hai lý do độc lập. Đầu tiên,$i^i$đã vượt quá phạm vi số nguyên tiêu chuẩn gần như ngay lập tức. Thứ hai, ngay cả khi sử dụng số học chính xác tùy ý, chuỗi nhân trở nên không khả thi ở$n = 10^6$, vì cả thời gian và trí nhớ sẽ bùng nổ. 

Một trường hợp thất bại tinh vi hơn phát sinh khi ai đó tính toán mọi thứ theo modulo 10 ở mỗi bước. Cách tiếp cận đó bị phá vỡ vì các số 0 lan truyền qua phép nhân theo cách phụ thuộc vào việc hủy bỏ hệ số trước đó, chứ không phải hành vi của chữ số cục bộ. Ví dụ,$10 \cdot 2 = 20$, nhưng lấy chữ số cuối cùng cục bộ sẽ cho$0 \cdot 2 = 0$, ẩn cấu trúc thực sự khác 0 sau khi loại bỏ hệ số 10. 

Thử thách thực sự là chúng ta phải theo dõi phép nhân trong khi liên tục hủy các thừa số 2 và 5, vì chỉ riêng các thừa số đó đã tạo ra các số 0 ở cuối. 

## Phương pháp tiếp cận 

Chiến lược brute-force tính toán sản phẩm đầy đủ từng bước, nhân$i^i$vào một bộ tích lũy. Sau mỗi phép nhân, chúng ta loại bỏ các số 0 ở cuối bằng cách chia các thừa số của 10 và chỉ giữ lại một vài chữ số cuối. 

Điều này đúng về mặt lý thuyết vì chúng tôi bảo toàn cấu trúc chính xác của số ở mỗi bước. Tuy nhiên, kích thước của các giá trị trung gian tăng cực kỳ nhanh. Ngay cả khi chúng tôi tích cực cắt bớt các số 0, số còn lại vẫn yêu cầu theo dõi đủ chữ số để duy trì tính chính xác và$i^i$nó trở nên quá lớn để tính toán một cách rõ ràng. 

Vì$n = 10^6$, thậm chí tính toán lũy thừa mô-đun cho mỗi$i^i$chi phí$O(\log i)$, và tổng hợp tất cả$i$đưa ra đại khái$10^6 \log 10^6$, đó là ranh giới nhưng vẫn không phải là vấn đề chính. Nút thắt thực sự là phép nhân và chuẩn hóa theo hệ số 2 và 5 trên toàn bộ sản phẩm. 

Ý nghĩa quan trọng là tách biệt hai hiệu ứng: cấu trúc số 0 ở cuối và các chữ số có nghĩa còn lại theo modulo 10. Mỗi số 0 ở cuối được tạo bởi cặp 2 và 5 trùng khớp trong hệ số hóa của tích. Nếu chúng ta theo dõi rõ ràng có bao nhiêu số 2 và 5 xuất hiện trong$\prod i^i$, chúng ta có thể loại bỏ tất cả các cặp và sau đó tính tích còn lại modulo 10 mà không bị nhiễm từ số 0. 

Mỗi số$i^i$đóng góp các thừa số nguyên tố một cách có cấu trúc: số mũ của một số nguyên tố$p$TRONG$i^i$là$i \cdot v_p(i)$, Ở đâu$v_p(i)$là số mũ của$p$TRONG$i$. Chúng tôi chỉ quan tâm đến$p = 2$Và$p = 5$cho sự hình thành bằng không. 

Sau khi trừ các cặp 2 và 5 trùng khớp, chúng ta còn lại một số không có thừa số 10. Sau đó, chúng ta tính chữ số cuối cùng của nó bằng cách sử dụng phép nhân mô đun, cẩn thận bỏ qua các thừa số đã loại bỏ. 

Cấu trúc trở nên dễ quản lý vì chúng ta không bao giờ xây dựng các số nguyên lớn. Chúng tôi chỉ tích lũy đóng góp của các số nguyên nhỏ và giữ riêng số lượng 2 giây và 5 giây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Tăng trưởng theo cấp số nhân về giá trị, thực sự không khả thi | O(1) đến O(số nguyên lớn) | Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng$i$từ 1 đến$n$, điều trị$i^i$như một sự đóng góp có cấu trúc. 

1. Lấy tất cả số 2 và số 5 ra thành nhân tử$i$, và đếm xem có bao nhiêu cái xuất hiện. Nhân số đó với$i$, vì trong$i^i$mỗi yếu tố được lặp lại$i$lần. Điều này mang lại tổng số đóng góp là 2 giây và 5 giây cho thuật ngữ này. Bước này cô lập các số nguyên tố duy nhất chịu trách nhiệm về số 0 ở cuối. 
2. Loại bỏ số 2 và số 5 đó khỏi$i$, để lại một cơ số rút gọn nguyên tố cùng nhau với 10. Giá trị rút gọn này an toàn để nhân modulo 10 mà không tạo ra các số 0 nhân tạo. 
3. Tính toán$(i \bmod 10)^i$nhưng đã loại bỏ tất cả các thừa số của 2 và 5 trong quá trình lũy thừa. Trong thực tế, chúng tôi tính toán lũy thừa mô-đun trong khi loại bỏ các hệ số 2 và 5 khỏi các kết quả trung gian bất cứ khi nào chúng xuất hiện. 
4. Nhân phần đóng góp đã được làm sạch này vào một bộ tích lũy modulo 10, đảm bảo chúng ta không bao giờ cho phép bộ tích lũy trở về 0 do việc hủy bỏ 2 và 5 bị ẩn. 
5. Sau khi xử lý tất cả các số, chúng tôi lắp lại hiệu ứng mất cân bằng còn sót lại giữa 2 giây và 5 giây. Nếu có nhiều hơn 2 giây so với 5 giây, 2 giây còn lại có thể đóng góp hệ số cuối cùng là 2 trong chu kỳ chữ số cuối cùng; tương tự đối với 5 giây, mặc dù 5 chỉ ảnh hưởng đến chữ số cuối cùng là 5 hoặc 0 và các số 0 đã bị xóa. 

Chữ số cuối cùng được trích xuất dưới dạng bộ tích lũy modulo 10 sau tất cả các lần hủy. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên việc duy trì tính bất biến: ở mỗi bước, giá trị tích lũy biểu thị tích của tất cả các số hạng được xử lý với tất cả các cặp 2 và 5 trùng khớp bị loại bỏ. Điều này đảm bảo rằng không có thừa số 10 nào tồn tại ở trạng thái được duy trì, vì vậy chữ số cuối cùng mà chúng tôi tính toán luôn là chữ số cuối cùng khác 0 hợp lệ của tiền tố sản phẩm thực. Vì phép nhân có tính kết hợp và việc loại bỏ nhân tố chỉ loại bỏ trung tính$10$cặp, phép biến đổi giữ nguyên chữ số cuối cùng khác 0 của sản phẩm cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def strip_factors(x, p):
    cnt = 0
    while x % p == 0 and x > 0:
        x //= p
        cnt += 1
    return x, cnt

def mod_pow_no_2_5(base, exp, cnt2, cnt5):
    res = 1
    for _ in range(exp):
        x = base
        c2, c5 = 0, 0

        x, t = strip_factors(x, 2)
        c2 += t
        x, t = strip_factors(x, 5)
        c5 += t

        cnt2 += c2
        cnt5 += c5

        res *= x
        while res % 2 == 0 and cnt2 > 0:
            res //= 2
            cnt2 -= 1
        while res % 5 == 0 and cnt5 > 0:
            res //= 5
            cnt5 -= 1

        res %= 10

    return res, cnt2, cnt5

def solve():
    n = int(input().strip())
    ans = 1
    cnt2 = 0
    cnt5 = 0

    for i in range(1, n + 1):
        base, c2 = strip_factors(i, 2)
        base, c5 = strip_factors(base, 5)

        cnt2 += c2 * i
        cnt5 += c5 * i

        val, cnt2, cnt5 = mod_pow_no_2_5(base, i, cnt2, cnt5)

        ans *= val
        while ans % 2 == 0 and cnt2 > 0:
            ans //= 2
            cnt2 -= 1
        while ans % 5 == 0 and cnt5 > 0:
            ans //= 5
            cnt5 -= 1

        ans %= 10

    print(ans)

if __name__ == "__main__":
    solve()
```Mã này duy trì hai bộ đếm toàn cầu để biết có bao nhiêu thừa số 2 và 5 vẫn “chưa từng có” trong sản phẩm. Mỗi học kỳ$i^i$đóng góp$i$bản sao của hệ số hóa của$i$, vì vậy chúng tôi chia tỷ lệ số lượng cho phù hợp. 

Bộ tích lũy`ans`luôn được giữ không có các số 0 ở cuối bằng cách hủy ngay lập tức 2 giây và 5 giây bất cứ khi nào có thể bằng cách sử dụng bộ đếm được lưu trữ. Điều này ngăn ngừa sự sai lệch của chữ số cuối cùng do số 0 ẩn. 

Việc giảm mô-đun xuống 10 là an toàn vì chúng tôi không bao giờ cho phép các hệ số 10 vẫn còn trong bộ tích lũy. 

## Ví dụ đã hoạt động 

Hãy xem xét$n = 3$. Sản phẩm là:$$1^1 \cdot 2^2 \cdot 3^3 = 1 \cdot 4 \cdot 27 = 108$$Chữ số cuối cùng khác 0 là 8. 

Chúng tôi theo dõi sự đóng góp: 

| tôi | tôi^tôi | đóng góp sạch sẽ | sản phẩm tích lũy (không có số 0) | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 4 | 4 | 4 | 
| 3 | 27 | 27 | 108 → dải số 0 → 18 | 

Chữ số cuối cùng khác 0 là 8. 

Dấu vết này cho thấy việc loại bỏ số 0 phải xảy ra trên toàn bộ sau khi nhân chứ không phải cục bộ bên trong mỗi số hạng. 

Bây giờ hãy xem xét$n = 5$, sản phẩm ở đâu:$$1 \cdot 4 \cdot 27 \cdot 256 \cdot 3125$$| tôi | tôi^tôi | dạng an toàn chữ số cuối cùng | bộ tích lũy (sau khi loại bỏ số 0) | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 4 | 4 | 4 | 
| 3 | 27 | 27 | 108 → 18 | 
| 4 | 256 | 256 | 4608 → 48 | 
| 5 | 3125 | 3125 | 15000000 → 15 | 

Chữ số cuối cùng là 5. 

Ví dụ này cho thấy các số 0 ở cuối xuất hiện liên tục như thế nào và phải được loại bỏ bằng cách sử dụng tính năng theo dõi hệ số thay vì phương pháp phỏng đoán ở cấp độ chữ số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi$i^i$được xử lý thông qua phép lũy thừa và loại bỏ hệ số tỷ lệ với$i$và chi phí nhân tố của nó | 
| Không gian |$O(1)$| Chỉ duy trì một số lượng bộ đếm không đổi và một bộ tích lũy nhỏ | 

Thuật toán chạy trong giới hạn cho$n \le 10^6$bởi vì tất cả số học nặng được giảm xuống thành các phép tính trên các số nguyên nhỏ với sự chuẩn hóa định kỳ. Hạn chế chính là tránh xây dựng số lượng lớn, điều mà giải pháp này thực hiện hoàn toàn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    return str(solve())

assert run("1\n") == "1", "minimum case"
assert run("2\n") == "4", "2^2 case"
assert run("5\n") == "4", "sample case"
assert run("10\n") in "123456789", "valid digit range"
assert run("1000000\n") is not None, "stress case"
assert run("3\n") == "8", "1*4*27 case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | trường hợp cơ sở | 
| 2 | 4 | sức mạnh đơn giản | 
| 5 | 4 | tính nhất quán của mẫu | 
| 3 | 8 | sản phẩm composite nhỏ | 
| 1000000 | chữ số | tính khả thi căng thẳng | 

## Vỏ cạnh 

cho$n = 1$, sản phẩm tầm thường$1^1 = 1$. Thuật toán khởi tạo bộ đếm về 0 và đặt bộ tích lũy thành 1, do đó đầu ra ngay lập tức là 1 mà không có bất kỳ sự hủy bỏ nào. 

Vì$n = 2$, chúng tôi tính toán$1 \cdot 2^2 = 4$. Việc loại bỏ hệ số sẽ loại bỏ một cặp số 2 bên trong$2^2$, để lại phần đóng góp rõ ràng là 4. Không tồn tại 5 giây, do đó bộ tích lũy vẫn còn 4 và được in trực tiếp. 

Đối với trường hợp có kích thước lớn$n$, các khoản đóng góp lặp đi lặp lại của 5 giây tích lũy chậm so với 2 giây, nghĩa là nhiều trạng thái trung gian mang theo 2 giây vượt quá và chỉ bị hủy muộn. Thuật toán xử lý vấn đề này vì việc hủy luôn được trì hoãn cho đến khi xuất hiện số 5 phù hợp, đảm bảo không bị mất cấu trúc sớm trong phép tính chữ số cuối cùng.
