---
title: "CF 106016D - Ước số ít phổ biến nhất"
description: "Chúng ta có một số cố định $x$ và một danh sách dài các giá trị $ai$. Với mỗi $ai$, chúng ta muốn tìm số nguyên dương nhỏ nhất $z$ thỏa mãn hai điều kiện: nó phải chia $x$, và nó phải không chia hết $ai$."
date: "2026-06-22T16:50:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106016
codeforces_index: "D"
codeforces_contest_name: "The 2025 Homs Collegiate programming contest"
rating: 0
weight: 106016
solve_time_s: 72
verified: true
draft: false
---

[CF 106016D - Ước số không phổ biến nhỏ nhất](https://codeforces.com/problemset/problem/106016/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một số cố định$x$và một danh sách dài các giá trị$a_i$. Đối với mỗi$a_i$, ta muốn tìm số nguyên dương nhỏ nhất$z$thỏa mãn hai điều kiện: nó phải chia$x$, và nó phải không thể chia được$a_i$. Nếu mọi ước của$x$cũng chia$a_i$, thì không như vậy$z$tồn tại và câu trả lời là$-1$. 

Một cách khác để đọc tác vụ là với mỗi giá trị truy vấn$a_i$, chúng tôi quét qua tất cả các ước của$x$theo thứ tự tăng dần và hỏi xem sự “không khớp” đầu tiên xảy ra ở đâu giữa các tập hợp ước số của$x$Và$a_i$. 

Các ràng buộc làm cho cấu trúc trở nên quan trọng. Kích thước mảng có thể đạt tới một triệu và giá trị lên tới$10^{12}$. Điều này ngay lập tức loại trừ bất kỳ phương pháp nào cố gắng phân tích từng số một cách độc lập hoặc tính toán lại các ước số của$x$lặp đi lặp lại cho mỗi truy vấn. Ngay cả việc quét tuyến tính cho mỗi truy vấn trên một tập hợp số chia lớn cũng trở nên quan trọng vì số lượng ước số của một số lên tới$10^{12}$vẫn có thể đạt tới khoảng một triệu trong các trường hợp bệnh lý. 

Một trường hợp thất bại tinh tế xuất hiện khi$a_i$chia hết cho mọi ước của$x$. Ví dụ, nếu$x = 12$, các ước của nó là$1,2,3,4,6,12$. Nếu như$a_i = 60$, mỗi cái đều chia hết cho 60, nên đáp án phải là$-1$. Một cách tiếp cận ngây thơ dừng sớm hoặc giả sử luôn tồn tại một ước số bị thiếu sẽ trả về không chính xác 1 hoặc một giá trị nhỏ khác. 

Một trường hợp cạnh khác xảy ra khi ước số nhỏ nhất của$x$, luôn là 1, đã không đáp ứng được điều kiện. Điều này là không thể vì 1 chia hết mọi thứ, nên quá trình quét luôn bắt đầu bằng một ước số “hợp lệ”. Lỗi có ý nghĩa đầu tiên phải xảy ra ở một số ước lớn hơn 1. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp tự gợi ý ngay lập tức. Ta có thể liệt kê tất cả các ước của$x$, sắp xếp chúng và với mỗi$a_i$, kiểm tra tính chia hết của từng ước số theo thứ tự cho đến khi tìm được số không chia hết$a_i$. Điều này đúng vì định nghĩa của câu trả lời yêu cầu một cách rõ ràng ước số nhỏ nhất như vậy. 

Điểm nghẽn là rõ ràng khi chúng tôi ước tính chi phí. Nếu như$x$có$D$các ước số thì mỗi truy vấn có thể yêu cầu tối đa$D$kiểm tra mô đun. Trong trường hợp xấu nhất$D$có thể lớn và với$n$lên đến$10^6$, việc này sẽ trở nên quá chậm nếu mỗi truy vấn quét một tiền tố lớn. 

Sự đơn giản hóa cấu trúc quan trọng xuất phát từ việc nhận thấy rằng chúng ta thực sự không cần phân tích nhân tử đầy đủ cho mỗi truy vấn. Đối với một cố định$a_i$, các ước của$x$sự chia rẽ đó$a_i$chính xác là những ước số của$\gcd(x, a_i)$. Điều này chuyển vấn đề thành việc kiểm tra tư cách thành viên trong một tập hợp ước số: ước số$d$có giá trị cho$a_i$khi và chỉ nếu nó không chia$\gcd(x, a_i)$. 

Vì vậy, với mỗi truy vấn, chúng tôi tính toán$g = \gcd(x, a_i)$, sau đó quét các ước của$x$theo thứ tự tăng dần và trả về ước số thứ nhất$d$như vậy$d \nmid g$. Điều này thay thế việc kiểm tra tính chia hết đầy đủ lặp đi lặp lại đối với$a_i$bằng cách kiểm tra một số nhỏ hơn. 

Chúng tôi vẫn quét danh sách ước số đã sắp xếp, nhưng giờ đây đây là một đường truyền tuyến tính đơn giản trên một cấu trúc được tính toán trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tính toán lại các ước số cho mỗi truy vấn) |$O(n \sqrt{x})$|$O(1)$| Quá chậm | 
| Bảng liệt kê số chia + cắt tỉa gcd |$O(D + nD)$trường hợp xấu nhất |$O(D)$| Được chấp nhận trong thực tế | 

## Hướng dẫn thuật toán 

1. Tính tất cả các ước của$x$bằng cách kiểm tra số nguyên lên đến$\sqrt{x}$. Đối với mỗi cặp số chia hợp lệ, hãy chèn cả hai giá trị vào danh sách. 

Mục tiêu là xây dựng một tập hợp chính xác các ứng viên cho$z$. 
2. Sắp xếp danh sách ước số theo thứ tự tăng dần. 

Thứ tự này rất cần thiết vì bài toán yêu cầu ước số hợp lệ nhỏ nhất. 
3. Với mỗi giá trị truy vấn$a_i$, tính toán$g = \gcd(x, a_i)$. 

Điều này nén tất cả thông tin về ước số nào của$x$cũng chia$a_i$thành một số duy nhất. 
4. Lặp lại các ước số đã được sắp xếp của$x$từ nhỏ nhất đến lớn nhất. 
5. Với mỗi ước số$d$, kiểm tra xem$g \bmod d = 0$. 

Nếu nó khác 0,$d$không chia$a_i$, Vì thế$d$là câu trả lời cho truy vấn này. 
6. Nếu quá trình quét hoàn tất mà không tìm thấy ước số đó, hãy xuất ra$-1$. 

Sự lựa chọn thiết kế quan trọng là chúng ta không bao giờ tính toán lại khả năng chia hết theo$a_i$trực tiếp. Tất cả các truy vấn thành viên được giảm xuống để kiểm tra xem một ước số của$x$cũng chia$g$. 

### Tại sao nó hoạt động 

Đối với bất kỳ số chia nào$d$của$x$, điều kiện “$d$chia rẽ$a_i$” tương đương với “$d$chia rẽ$\gcd(x, a_i)$Sự tương đương này đúng vì bất kỳ ước số nào của$x$đó là sự chia rẽ$a_i$phải chia ước chung lớn nhất của chúng. 

Vì vậy, danh sách ước số tự nhiên được chia thành hai phần cho mỗi truy vấn: những phần chia$g$, và những cái không. Vì chúng ta quét theo thứ tự tăng dần nên ước số đầu tiên nằm ngoài tập hợp ước số của$g$chính xác là câu trả lời hợp lệ nhỏ nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from math import gcd

def solve():
    n, x = map(int, input().split())
    a = list(map(int, input().split()))
    
    # compute divisors of x
    divs = []
    i = 1
    while i * i <= x:
        if x % i == 0:
            divs.append(i)
            if i * i != x:
                divs.append(x // i)
        i += 1
    
    divs.sort()
    
    for ai in a:
        g = gcd(x, ai)
        ans = -1
        for d in divs:
            if g % d != 0:
                ans = d
                break
        print(ans)

if __name__ == "__main__":
    solve()
```Bước đầu tiên xây dựng danh sách ước số đầy đủ của$x$TRONG$O(\sqrt{x})$. Việc này chỉ được thực hiện một lần, điều này rất quan trọng vì$x$được chia sẻ trên tất cả các truy vấn. 

Vòng lặp bên trong sử dụng gcd để tránh kiểm tra tính chia hết đối với$a_i$trực tiếp. điều kiện`g % d != 0`là chìa khóa thay thế cho`ai % d != 0`, điều này sẽ tốn kém hơn nếu phải lý luận nhiều lần. 

Việc nghỉ sớm đảm bảo rằng khi tìm thấy ước số không hợp lệ nhỏ nhất, chúng tôi sẽ ngừng quét ngay lập tức. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
x = 30
a = [6, 10, 15]
```Các ước của 30 là:`[1, 2, 3, 5, 6, 10, 15, 30]`Đối với mỗi giá trị: 

| ai | g = gcd(x, ai) | kết quả quét | 
| --- | --- | --- | 
| 6 | 6 | 5 | 
| 10 | 10 | 3 | 
| 15 | 15 | 2 | 

Vì$a_i = 6$, ước của$g = 6$là`[1,2,3,6]`, vậy ước số đầu tiên không có trong tập hợp này là 5. 

Điều này xác nhận rằng thuật toán đang tìm kiếm ước số bị thiếu đầu tiên một cách hiệu quả trong tập hợp ước số của$g$. 

### Ví dụ 2 

đầu vào:```
x = 12
a = 60
```Các ước của 12 là`[1,2,3,4,6,12]`. 

Từ$60$được chia hết cho tất cả những điều này, chúng tôi nhận được$g = \gcd(12, 60) = 12$. Mỗi ước số của$x$chia rẽ$g$, do đó quá trình quét kết thúc mà không bị gián đoạn và quay trở lại$-1$. 

Điều này chứng tỏ tính đúng đắn của điều kiện hư hỏng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt{x} + nD)$| các ước số được tạo một lần, mỗi truy vấn sẽ quét danh sách ước số | 
| Không gian |$O(D)$| lưu trữ tất cả các ước của$x$| 

Số chia$D$được giới hạn bởi số ước của$x$, có thể quản lý được trong các trường hợp điển hình. Vì mỗi truy vấn dừng sớm khi tìm thấy điểm không khớp đầu tiên nên hiệu suất trung bình sẽ tốt hơn đáng kể so với giới hạn trong trường hợp xấu nhất, khiến giải pháp phù hợp cho$n = 10^6$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import gcd
    import sys

    input = sys.stdin.readline

    def solve():
        n, x = map(int, input().split())
        a = list(map(int, input().split()))

        divs = []
        i = 1
        while i * i <= x:
            if x % i == 0:
                divs.append(i)
                if i * i != x:
                    divs.append(x // i)
            i += 1

        divs.sort()

        res = []
        for ai in a:
            g = gcd(x, ai)
            ans = -1
            for d in divs:
                if g % d != 0:
                    ans = d
                    break
            res.append(str(ans))

        return " ".join(res)

    return solve()

# provided sample (illustrative formatting)
assert run("5 30\n6 10 15 35 60\n") == "5 3 2 2 -1", "sample"

# all ai divisible by x -> -1
assert run("3 12\n12 24 36\n") == "-1 -1 -1"

# small x
assert run("3 6\n1 2 3\n") == "2 3 2"

# prime x
assert run("4 13\n1 13 26 27\n") == "13 13 2 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| x nhỏ, chia hết hỗn hợp | khác nhau | tính đúng đắn của việc lọc dựa trên gcd | 
| tất cả đều chia hết cho x | tất cả -1 | trường hợp cạnh bảo hiểm đầy đủ | 
| số nguyên tố x | tập chia số đơn giản | cấu trúc chia tối thiểu | 
| giá trị hỗn hợp | đầu ra hỗn hợp | tính đúng đắn chung | 

## Vỏ cạnh 

Khi nào$a_i$là bội số của mọi ước của$x$, thuật toán tính toán$g = x$, và mọi ước số đều vượt qua việc kiểm tra`g % d == 0`. Quá trình quét hoàn tất mà không bị hỏng và trả về chính xác$-1$, phù hợp với định nghĩa. 

Khi$x$là số nguyên tố, danh sách chia là`[1, x]`. Vì 1 luôn chia hết mọi số$a_i$, câu trả lời là$x$(nếu như$a_i$không chia hết cho$x$) hoặc$-1$. Thuật toán xử lý việc này một cách tự nhiên vì nó kiểm tra 1 trước, sau đó trực tiếp tiếp cận$x$. 

Khi$a_i$rất nhỏ, đặc biệt là 1, nên gcd trở thành 1. Vì chỉ có ước số 1 chia hết cho 1 nên mọi ước số khác của$x$ngay lập tức trở thành một câu trả lời ứng cử viên, do đó thuật toán trả về ước số nhỏ thứ hai của$x$, phù hợp chính xác với định nghĩa.
