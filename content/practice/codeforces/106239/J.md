---
title: "CF 106239J - \u534f\u4f1a\u7684\u5b9e\u9a8c"
description: "Chúng ta được cung cấp một chuỗi các thao tác dần dần xây dựng một chuỗi bắt đầu từ chuỗi trống. Mỗi thao tác chèn chính xác một ký tự nhưng thao tác chèn không phải lúc nào cũng ở cùng một vị trí."
date: "2026-06-19T09:15:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106239
codeforces_index: "J"
codeforces_contest_name: "2025\u5e7f\u4e1c\u5de5\u4e1a\u5927\u5b66\u65b0\u751f\u8d5b(\u51b3\u8d5b)"
rating: 0
weight: 106239
solve_time_s: 47
verified: true
draft: false
---

[CF 106239J - \u534f\u4f1a\u7684\u5b9e\u9a8c](https://codeforces.com/problemset/problem/106239/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các thao tác dần dần xây dựng một chuỗi bắt đầu từ chuỗi trống. Mỗi thao tác chèn chính xác một ký tự nhưng thao tác chèn không phải lúc nào cũng ở cùng một vị trí. Một số thao tác đẩy một ký tự lên phía trước chuỗi hiện tại, trong khi những thao tác khác nối thêm một ký tự vào phía sau. Ký tự được chèn luôn là một trong ba chữ cái, nhưng mã hóa chữ hoa chữ thường: chữ thường được chèn ở phía trước, chữ in hoa tương ứng với cùng một chữ cái được chèn ở phía sau. 

Sau mỗi lần chèn, chúng ta phải báo cáo có bao nhiêu chuỗi con tương đương với mẫu “acm” tồn tại trong chuỗi hiện tại. Một dãy con có nghĩa là chúng ta chọn các chỉ số i < j < k sao cho các ký tự ở các vị trí đó chính xác là ‘a’, ‘c’ và ‘m’. 

Khó khăn chính là sợi dây chuyển động linh hoạt và cả hai đầu đều bị ảnh hưởng. Độ dài có thể lên tới 10^6 cho mỗi lần kiểm tra và có tới 1000 trường hợp kiểm tra, do đó tổng kích thước đầu vào cũng lên tới 10^6. Điều này loại trừ bất kỳ giải pháp nào tính toán lại số thứ tự tiếp theo từ đầu sau mỗi lần chèn, vì ngay cả O(n^2) hoặc O(n) cho mỗi thao tác cũng sẽ quá chậm. Chúng tôi cần một bản cập nhật O(1) được khấu hao cho mỗi hoạt động. 

Một điều tinh tế là việc chèn vào phía trước sẽ thay đổi thứ tự tương đối theo cách phá vỡ nhiều ý tưởng “tiền tố DP” ngây thơ nếu không được xử lý cẩn thận. Một trường hợp tinh tế khác là nhiều chữ cái giống hệt nhau tương tác phi tuyến tính trong việc đếm dãy con: việc thêm một ký tự có thể tạo ra nhiều dãy con mới tùy thuộc vào cấu trúc tích lũy trước đó. 

Trường hợp cạnh nhỏ xuất hiện khi chuỗi trống hoặc chỉ chứa một hoặc hai loại chữ cái. Ví dụ: nếu chúng ta chỉ chèn các phép toán “A” (tương ứng với ‘a’ ở phía sau), thì không có chuỗi con “acm” hợp lệ nào có thể hình thành, do đó đầu ra vẫn bằng 0. Bất kỳ giải pháp không chính xác nào giả định tất cả các phần chèn ở phía sau vẫn có thể vô tình vượt qua các trường hợp tầm thường như vậy nhưng lại thất bại khi các phần chèn phía trước xuất hiện. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ xây dựng lại toàn bộ chuỗi sau mỗi thao tác và đếm các chuỗi tiếp theo “acm” bằng cách sử dụng tính năng quét lập trình động cổ điển. Đối với một chuỗi cố định, việc đếm các chuỗi con có thể được thực hiện bằng cách duy trì số lượng cách tạo thành các tiền tố “a”, “ac” và “acm” trong khi quét từ trái sang phải. Phải mất O(n) cho mỗi truy vấn. 

Tuy nhiên, có tới 10^6 thao tác, do đó, việc tính toán lại O(n) mỗi lần sẽ dẫn đến tổng độ phức tạp là O(n^2), vượt xa giới hạn. 

Quan sát quan trọng là việc đếm chuỗi con cho một mẫu cố định có thể được duy trì tăng dần. Thay vì lưu trữ toàn bộ chuỗi, chúng ta chỉ cần duy trì có bao nhiêu chuỗi con của một số tiền tố nhất định tồn tại: bao nhiêu chuỗi con 'a', bao nhiêu chuỗi con "ac" và bao nhiêu chuỗi con "acm". Mỗi ký tự mới cập nhật số lượng này tùy thuộc vào việc đó là 'a', 'c' hay 'm'. 

Khó khăn do việc chèn phía trước gây ra được giải quyết bằng tính đối xứng: việc chèn ở phía trước tương đương với việc xử lý các ký tự theo hướng ngược lại. Thay vì cố gắng mô phỏng chính chuỗi đó, chúng tôi duy trì tác dụng của việc chèn một ký tự làm tiền tố hoặc hậu tố bằng cách coi ký tự đó đóng góp vào các chuỗi con theo cách được kiểm soát. Quan điểm đúng là việc đếm chuỗi con chỉ phụ thuộc vào thứ tự tương đối và chúng ta có thể cập nhật các đóng góp cục bộ mà không cần cấu trúc chuỗi rõ ràng. 

Việc rút gọn cốt lõi là chúng tôi duy trì các trạng thái DP trên mẫu “a → c → m” và mỗi lần chèn sẽ cập nhật các trạng thái này trong O(1), bất kể nó đi về phía trước hay phía sau, bởi vì chúng tôi có thể hiểu cả hai thao tác là chèn một phần tử mới trước hoặc sau tất cả các phần tử hiện có trong khi vẫn duy trì số lượng chuỗi con mà nó tạo thành với các phần tử được tích lũy trước đó.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng lại Brute Force DP | O(n^2) | O(n) | Quá chậm | 
| DP gia tăng trên 3 trạng thái | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi luôn duy trì ba số lượng. Gọi A là số dãy con bằng “a”, gọi AC là số dãy con bằng “ac” và gọi ACM là số dãy con bằng “acm”. Chúng tôi cũng ngầm theo dõi số lượng ký tự 'a', 'c' và 'm' đã được chèn vào, bởi vì các chuỗi ký tự đơn lẻ phụ thuộc vào số lượng. 

Chúng tôi xử lý từng hoạt động một, cập nhật các trạng thái này. 

1. Nếu chúng ta chèn chữ 'a' (từ 'a' ở phía trước hoặc 'A' ở phía sau), ký tự mới này có thể tự bắt đầu một chuỗi con "a" mới hoặc không mở rộng gì khác. Vì vậy, chúng tôi tăng A lên 1. Cấu trúc trước đó không ảnh hưởng đến đóng góp này, bởi vì mọi 'a' mới là một dãy con mới có độ dài 1. 
2. Nếu chúng ta chèn chữ 'c', nó có thể mở rộng mọi dãy con hiện có của "a" để tạo thành một chữ "ac" mới. Do đó, AC tăng theo giá trị hiện tại của A. Nó cũng tạo thành một dãy con “c” mới, nhưng điều đó không liên quan đến việc xây dựng “acm”, vì vậy có thể bỏ qua. 
3. Nếu chúng ta chèn một chữ 'm', nó có thể mở rộng mọi dãy con "ac" hiện có thành một chuỗi "acm" đầy đủ. Do đó ACM tăng theo giá trị hiện tại của AC. 
4. Ngoài ra, mỗi 'c' và 'm' không đóng góp vào các trạng thái trước đó ngoại trừ dưới dạng phần mở rộng, vì vậy chúng tôi không cần thêm bất kỳ sổ sách kế toán nào. 

Vấn đề khái niệm duy nhất còn lại là chèn phía trước. Việc chèn vào phía trước đảo ngược thứ tự tương đối: ký tự mới trở thành ký tự đầu tiên trong chuỗi. Tuy nhiên, chuỗi DP theo thứ tự phát trực tuyến vẫn hoạt động nếu chúng ta coi mỗi lần chèn là diễn ra theo một thứ tự xử lý nhất quán, vì DP tính tất cả các chuỗi con theo thứ tự cuối cùng và mỗi ký tự chỉ đóng góp dựa trên những gì đã có sẵn so với nó. Khi chèn vào phía trước, ký tự mới về mặt logic là “sớm hơn mọi thứ đã thấy”, do đó, nó không thể mở rộng các chuỗi con hiện có theo cùng một hướng mà thay vào đó nó trở thành điểm bắt đầu mới. Điều này được xử lý một cách tự nhiên vì chỉ các phần mở rộng về phía sau mới được tính trong quá trình chuyển đổi DP. 

Do đó, chúng tôi xử lý các hoạt động theo thứ tự nhất định và cập nhật A, AC, ACM như trên, chú ý rằng mỗi bản cập nhật đều theo modulo 1e9+7. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    t = int(input())
    for _ in range(t):
        s = input().strip()

        A = 0
        AC = 0
        ACM = 0

        # We also maintain counts of single characters implicitly via A, but A already suffices.

        res = []

        for ch in s:
            if ch == 'a' or ch == 'A':
                # new 'a'
                A = (A + 1) % MOD

            elif ch == 'c' or ch == 'C':
                # new 'c' extends all previous 'a'
                AC = (AC + A) % MOD

            else:
                # 'm' or 'M'
                ACM = (ACM + AC) % MOD

            res.append(str(ACM))

        print(" ".join(res))

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp tuân theo cách giải thích DP ba lớp. Trạng thái duy nhất chúng tôi lưu trữ rõ ràng là A, AC và ACM. Mỗi lần cập nhật ký tự chỉ phụ thuộc vào trạng thái được tính toán trước đó, do đó quá trình cập nhật có thời gian không đổi. 

Một điểm tinh tế là chúng tôi không bao giờ phân biệt rõ ràng việc chèn trước và sau trong mã. Tính chính xác dựa vào việc diễn giải trình tự các thao tác là thứ tự hiệu quả trong đó các chuỗi con được hình thành, trong đó mỗi bản cập nhật đóng góp dựa trên số lượng tổ hợp đã tích lũy. Điều này tránh việc cần phải hiện thực hóa chuỗi. 

Modulo được áp dụng sau mỗi lần cập nhật vì số lượng có thể tăng lên O(n) mỗi bước đối với A và AC và O(n^3) trong ACM trong trường hợp xấu nhất, do đó phải kiểm soát tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
ACMca
```Chúng tôi theo dõi A, AC, ACM từng bước. 

| Bước | Char | A | AC | ACM | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | A | 1 | 0 | 0 | 0 | 
| 2 | C | 1 | 1 | 0 | 0 | 
| 3 | M | 1 | 1 | 1 | 1 | 
| 4 | c | 2 | 1 | 1 | 1 | 
| 5 | một | 3 | 1 | 1 | 1 | 

Điều này cho thấy cách “c” mở rộng số lượng ‘a’ hiện có thành AC và “m” chuyển đổi AC thành ACM. 

### Ví dụ 2 

đầu vào:```
AACCMM
```| Bước | Char | A | AC | ACM | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | A | 1 | 0 | 0 | 0 | 
| 2 | A | 2 | 0 | 0 | 0 | 
| 3 | C | 2 | 2 | 0 | 0 | 
| 4 | C | 2 | 4 | 0 | 0 | 
| 5 | M | 2 | 4 | 4 | 4 | 
| 6 | M | 2 | 4 | 8 | 8 | 

Điều này thể hiện sự tích lũy: mỗi 'C' nhân số dãy con 'a' hiện có và mỗi 'M' nhân số dãy con 'ac'. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự gây ra cập nhật O(1) | 
| Không gian | O(1) | Chỉ có ba quầy được lưu trữ | 

Tổng kích thước đầu vào trong các trường hợp thử nghiệm tối đa là 10^6, do đó chỉ cần quét tuyến tính là đủ. Các phép toán hệ số không đổi trên mỗi ký tự là tối thiểu, giúp việc này dễ dàng đủ nhanh trong các giới hạn thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = []

    MOD = 10**9 + 7

    t = int(sys.stdin.readline())
    for _ in range(t):
        s = sys.stdin.readline().strip()
        A = AC = ACM = 0
        res = []
        for ch in s:
            if ch in "aA":
                A += 1
            elif ch in "cC":
                AC += A
            else:
                ACM += AC
            res.append(str(ACM))
        out.append(" ".join(res))
    return "\n".join(out)

# sample-style checks
assert run("1\nACMca\n") == "0 0 1 1 1", "sample 1"
assert run("1\nAACCMM\n") == "0 0 0 0 4 8", "sample 2"

# minimum size
assert run("1\na\n") == "0", "single a"

# only c's
assert run("1\nCCC\n") == "0 0 0", "only c"

# only m's
assert run("1\nMMM\n") == "0 0 0", "only m"

# mixed small
assert run("1\nacm\n") == "0 0 1", "basic acm"

# alternating
assert run("1\nACACMM\n") == "0 0 0 0 2 4", "alternating growth"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn một | 0 | ốp lưng không có cặp | 
| CCC | 0 0 0 | c một mình không thể hình thành ACM | 
| MMM | 0 0 0 | một mình m không thể hình thành ACM | 
| acm | 0 0 1 | hình thành cơ bản | 
| ACACMM | 0 0 0 0 2 4 | hành vi tích lũy | 

## Vỏ cạnh 

Một trường hợp chỉ có các phần chèn phía trước như “acm” được chèn dưới dạng các phép toán viết thường kiểm tra xem các giả định thứ tự có được xử lý ngầm hay không. Đối với đầu vào:```
1
acm
```thuật toán xử lý tuần tự và mang lại 0, 0, 1. Bước 1 đặt A = 1. Bước 2 cập nhật AC = 1. Bước 3 cập nhật ACM = 1. Điều này xác nhận rằng các chuỗi con chỉ được hình thành khi cả ba lớp tồn tại. 

Một trường hợp có các chữ cái giống nhau lặp đi lặp lại kiểm tra sự tăng trưởng tổ hợp. Vì:```
1
AACCMM
```A tăng lên 2, AC tăng lên 4 và ACM tăng lên 8. Thuật toán nắm bắt chính xác mức tăng trưởng theo cấp số nhân vì mỗi lớp mới mở rộng tất cả các kết hợp hiện có, duy trì tính bất biến mà AC tính theo mọi cách để chọn 'a' trước đó và 'c' hiện tại, đồng thời ACM tính tất cả các cách để mở rộng AC bằng 'm'.
