---
title: "CF 102281L - \u041d\u0435\u043e\u0431\u044b\u0447\u043d\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430"
description: "Hàm foo(a, b) liên tục trừ a từ b cho đến khi giá trị hiện tại trở thành không dương. Giá trị cuối cùng bằng 0 chính xác khi a chia cho b. Sau đó, hàm này thay thế đệ quy a bằng 2a và 2a+1, do đó, bắt đầu từ a=1, cuối cùng nó có thể đạt đến mọi số nguyên dương."
date: "2026-08-13T09:34:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102281
codeforces_index: "L"
codeforces_contest_name: "2011, IV \u0421\u0430\u043c\u0430\u0440\u0441\u043a\u0430\u044f \u043e\u0431\u043b\u0430\u0441\u0442\u043d\u0430\u044f \u043c\u0435\u0436\u0432\u0443\u0437\u043e\u0432\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 102281
solve_time_s: 110
verified: true
draft: false
---

[CF 102281L - \u041d\u0435\u043e\u0431\u044b\u0447\u043d\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430](https://codeforces.com/problemset/problem/102281/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

chức năng`foo(a, b)`trừ đi nhiều lần`a`từ`b`cho đến khi giá trị hiện tại trở thành không dương. Giá trị cuối cùng bằng 0 chính xác khi`a`chia rẽ`b`. Hàm sau đó thay thế đệ quy`a`qua`2a`Và`2a+1`, vậy bắt đầu từ`a=1`cuối cùng nó có thể đạt đến mọi số nguyên dương. 

Hằng số thực tế trong câu lệnh ban đầu lớn hơn nhiều so với dòng cuối cùng được hiển thị trong lời nhắc. Đó là sự kết hợp của ba phần`33931086844518982011982560935885732032396635556994207701963662088123265`,`31417633033625453597120718116969886858499194160778011107392823626119960`,
Và`4691797570505851011072000000000000000000000000000`. 

Hãy để hằng số này là`C`. Với mọi số nguyên`k`trong khoảng thời gian được yêu cầu, chúng ta cần đánh giá`foo(1, C+k)`và in`TRUE`hoặc`FALSE`. 

Hạn chế quan trọng đó là`k`chỉ nằm trong khoảng từ 0 đến 100, trong khi`C`có hàng trăm chữ số thập phân. Điều này làm cho bất kỳ thuật toán nào cũng tỷ lệ thuận với giá trị của`C`không thể nào. Thậm chí lặp qua các ước số lên đến`sqrt(C)`sẽ đòi hỏi một số lượng lớn các hoạt động. Việc rút gọn dự định là một câu hỏi nguyên tố, sau đó phép lũy thừa mô-đun đưa ra một giải pháp thực tế. 

Có một số trường hợp dễ xảy ra khi việc triển khai bất cẩn có thể dẫn đến sai sót. Đối với đầu vào`0 0`, câu trả lời là`TRUE`, bởi vì`C`kết thúc bằng nhiều số 0 và chắc chắn là hợp số. Một chương trình chỉ kiểm tra xem số đó có phải là số chẵn hay không sẽ hoạt động ở đây nhưng sẽ không hoạt động đối với một tổng số lẻ chẳng hạn như giá trị tương ứng với`k=99`, kết quả đúng của nó cũng là`TRUE`. Đối với đầu vào`0 1`, đầu ra là`TRUE`Và`FALSE`, như trong mẫu. Số thứ hai là số nguyên tố đặc biệt trong cặp này, vì vậy việc coi mọi số đủ lớn là hợp số sẽ không thành công. Cuối cùng, đối với đầu vào`100 100`, câu trả lời là`TRUE`, bởi vì`C+100`là chẵn. Một vòng lặp được viết với giới hạn trên dành riêng cho`k`có thể âm thầm bỏ qua trường hợp này. 

## Phương pháp tiếp cận 

Việc triển khai trực tiếp tuân theo chương trình cũ theo đúng nghĩa đen. Đối với mỗi cuộc gọi đệ quy`(a,b)`, chúng tôi trừ`a`từ`b`cho đến khi số dư không dương thì truy cập đệ quy`2a`Và`2a+1`bất cứ khi nào các điều kiện trước đó chưa quyết định kết quả. 

Việc triển khai trực tiếp này đúng về mặt logic vì phép đệ quy mô tả chính xác hàm ban đầu. Bắt đầu lúc`a=1`, các cặp đối số đệ quy liệt kê tất cả các số nguyên dương`a`dưới`b`: mọi số nguyên xuất hiện một lần dưới dạng nút của cây nhị phân có con là`2a`Và`2a+1`. Nếu như`b`là số nguyên tố, không có giá trị nào trong số đó chia hết`b`, vì vậy đệ quy khám phá cơ bản mọi`a`từ 1 đến`b-1`. 

Chi phí của các vòng trừ khi đó là khoảng 

[ 
\sum_{a=1}^{b-1}\left\lfloor\frac ba\right\rfloor, 
] 

đó là`Theta(b log b)`. Đây`b`có hàng trăm chữ số thập phân nên số phép trừ riêng lẻ theo thứ tự`10^187`. Việc giải thích bằng vũ lực không chỉ là quá chậm mà về cơ bản là không thể thực hiện được. 

Quan sát giúp giải quyết được vấn đề là phép đệ quy không thực sự tính toán một hàm Boolean tùy ý. Vì`a=1`Và`b>1`, điều kiện đầu tiên là sai và điều kiện`a<b`là đúng. Từ`a==1`, nhánh chia hết bị vô hiệu hóa, chỉ để lại các lệnh gọi đệ quy. Do đó,`foo(1,b)`đúng chính xác khi một trong các số nguyên có thể truy cập`a`với`1<a<b`chia rẽ`b`. 

Mọi số nguyên từ 2 đến`b-1`có thể truy cập được, vì vậy một`a`tồn tại chính xác khi`b`có một ước số thích hợp. Vì`b>1`, điều đó tương đương với`b`là tổng hợp. 

Do đó, toàn bộ vấn đề được rút gọn thành việc kiểm tra xem mỗi trong số tối đa 101 số nguyên khổng lồ có`C+k`là tổng hợp. Kiểm tra tính nguyên tố Miller-Rabin là một sự phù hợp tự nhiên vì nó hoạt động với các số nguyên lớn tùy ý bằng cách sử dụng phép nhân và lũy thừa mô-đun mà không bao giờ lặp lại chính số đó. 

Đối với các số có kích thước này, một tập hợp cố định của nhiều cơ sở Miller-Rabin làm cho xác suất chấp nhận một hợp số là số nguyên tố không đáng kể. Các số nguyên có độ chính xác tùy ý của Python cũng loại bỏ vấn đề tràn được đề cập rõ ràng trong câu lệnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`Theta(B log B)`mỗi giá trị |`O(log B)`độ sâu đệ quy | Quá chậm | 
| Miller-Rabin |`O(R log B)`hoạt động mô-đun trên mỗi giá trị |`O(1)`bên cạnh số nguyên lớn | Đã chấp nhận | 

Đây`B`biểu thị độ lớn của`C+k`, Và`R`là số căn cứ Miller-Rabin cố định. 

## Hướng dẫn thuật toán 

1. Đọc`kmin`Và`kmax`và xây dựng hằng số đầy đủ`C`dưới dạng số nguyên. Ba phần này được viết riêng biệt trong mã nguồn để chữ dài vẫn dễ dàng xác minh so với câu lệnh gốc. 
2. Đối với mọi`k`từ`kmin`bởi vì`kmax`, bộ`n = C + k`. Điểm cuối trên được bao gồm vì đầu ra phải chứa một dòng cho mỗi số nguyên trong khoảng đóng. 
3. Xử lý các vụ án tầm thường trước Miller-Rabin. Nếu như`n < 2`, nó không liên quan đến các ràng buộc chính thức, nhưng việc coi nó là không nguyên tố làm cho quy trình tính nguyên tố trở nên hoàn chỉnh. Nếu như`n`chia hết cho một số nguyên tố nhỏ như 2, 3, 5 hoặc một số nguyên tố nhỏ khác thì nó là hợp số ngay lập tức. 
4. Tiếp tục chạy Miller-Rabin`n`. Viết`FALSE`nếu bài kiểm tra xem xét`n`số nguyên tố và`TRUE`nếu không thì. Việc đảo ngược là có chủ ý: hàm đệ quy ban đầu trả về giá trị true cho hợp số, không phải cho số nguyên tố. 
5. In tất cả các câu trả lời theo cùng thứ tự với các giá trị tương ứng của`k`. Điều này duy trì sự tương ứng một-một cần thiết giữa các giá trị đầu vào và dòng đầu ra. 

### Tại sao nó hoạt động 

cho`b>1`,`foo(1,b)`chỉ có thể trở thành đúng thông qua một cuộc gọi đệ quy. Các chuyển đổi đệ quy`a -> 2a`Và`a -> 2a+1`tạo ra mọi số nguyên dương chính xác một lần, vì vậy trước khi đạt được`a>=b`hàm này kiểm tra mọi ước số thực sự có thể`a`của`b`. Vòng trừ làm cho phép thử tính chia hết chính xác, vì giá trị cuối cùng của nó bằng 0 chính xác khi`a`chia rẽ`b`. Như vậy`foo(1,b)`là đúng khi`b`có một ước số thích hợp, mà đối với`b>1`có nghĩa là chính xác khi nào`b`là tổng hợp. Thuật toán kiểm tra cùng thuộc tính đó thông qua kiểm tra tính nguyên tố, do đó đầu ra là giá trị Boolean được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

C = int(
    "33931086844518982011982560935885732032396635556994207701963662088123265"
    "31417633033625453597120718116969886858499194160778011107392823626119960"
    "4691797570505851011072000000000000000000000000000"
)

# A sufficiently large fixed set of bases makes the probability of a
# composite number passing all tests negligible for this problem.
BASES = [
    2, 3, 5, 7, 11, 13, 17, 19, 23, 29,
    31, 37, 41, 43, 47, 53, 59, 61, 67, 71,
    73, 79, 83, 89, 97, 101, 103, 107, 109, 113,
    127, 131, 137, 139, 149, 151, 157, 163, 167, 173,
    179, 181, 191, 193, 197, 199, 211, 223, 227, 229,
    233, 239, 241, 251, 257, 263, 269, 271, 277, 281,
    283, 293, 307, 311, 313, 317, 331, 337, 347, 349,
    353, 359, 367, 373, 379, 383, 389, 397, 401, 409,
    419, 421, 431, 433, 439, 443, 449, 457, 461, 463,
    467, 479, 487, 491, 499
]

def is_prime(n: int) -> bool:
    if n < 2:
        return False

    small_primes = (2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37)

    for p in small_primes:
        if n == p:
            return True
        if n % p == 0:
            return False

    # n - 1 = d * 2^s, with d odd.
    d = n - 1
    s = 0
    while d % 2 == 0:
        d //= 2
        s += 1

    for a in BASES:
        if a >= n:
            continue

        x = pow(a, d, n)

        if x == 1 or x == n - 1:
            continue

        for _ in range(s - 1):
            x = x * x % n
            if x == n - 1:
                break
        else:
            return False

    return True

def solve() -> None:
    kmin, kmax = map(int, input().split())

    ans = []

    for k in range(kmin, kmax + 1):
        n = C + k
        ans.append("FALSE" if is_prime(n) else "TRUE")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Hằng số được chuyển đổi từ một chuỗi thay vì được viết dưới dạng một mã thông báo số nguyên khổng lồ. Kiểu số nguyên có độ chính xác tùy ý của Python có thể biểu thị trực tiếp nên không cần xử lý tràn. 

các`is_prime`hàm đầu tiên loại bỏ các thừa số nguyên tố nhỏ. Điều này hữu ích cho nhiều giá trị chẵn trong phạm vi và tránh sử dụng hàng tá lũy thừa mô-đun cho một số tổng hợp rõ ràng. 

Miller-Rabin viết`n-1`ở dạng`d * 2^s`, với`d`số lẻ. Đối với một cơ sở được lựa chọn`a`, nó tính toán`a^d mod n`sử dụng phép lũy thừa mô-đun tích hợp sẵn của Python. Nếu chuỗi kết quả không bao giờ đạt tới`n-1`, số đó chắc chắn là hợp số. 

Điều kiện đầu ra là đảo ngược của kết quả nguyên tố. Một số nguyên tố`C+k`tương ứng với`FALSE`, trong khi tổng hợp`C+k`tương ứng với`TRUE`. 

Vòng lặp sử dụng`range(kmin, kmax + 1)`, không`range(kmin, kmax)`, bởi vì`kmax`thuộc về khoảng được yêu cầu. Đây là một trong những nơi dễ dàng nhất để đưa ra lỗi từng cái một. 

Tuyên bố chính thức đảm bảo rõ ràng rằng tràn số nguyên không tồn tại trong ngôn ngữ gốc. Python có cùng thuộc tính hữu ích cho nhiệm vụ này vì số nguyên của nó tăng lên khi cần thiết. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, hai giá trị là`C`Và`C+1`. 

|`k`|`C+k`tài sản |`is_prime`| Kết quả hàm | 
| --- | --- | --- | --- | 
| 0 | Tổng hợp |`False`|`TRUE`| 
| 1 | Thủ tướng |`True`|`FALSE`| 

Số đầu tiên là hợp số vì nó kết thúc bằng một dãy số 0 dài. Số thứ hai là số nguyên tố nên không có ước số thích hợp để hàm đệ quy khám phá. Kết quả đầu ra chính xác là đầu ra mẫu. 

Đối với mẫu thứ hai, các giá trị là`C+99`Và`C+100`. 

|`k`|`C+k`tài sản |`is_prime`| Kết quả hàm | 
| --- | --- | --- | --- | 
| 99 | Tổng hợp |`False`|`TRUE`| 
| 100 | Chẵn, do đó tổng hợp |`False`|`TRUE`| 

Giá trị thứ hai là một trường hợp phức hợp đặc biệt đơn giản vì việc cộng 100 sẽ bảo toàn sự đồng đều. Giá trị đầu tiên chứng minh tại sao chỉ kiểm tra chữ số cuối cùng là không đủ: nó là số lẻ nhưng vẫn là hợp số. Kiểm tra tính nguyên thủy xử lý cả hai trường hợp một cách thống nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O((kmax-kmin+1) R log C)`hoạt động mô-đun | Tối đa 101 số được kiểm tra, mỗi số có một số cố định`R`của cơ sở lũy thừa môđun | 
| Không gian |`O(1)`không gian phụ trợ | Chỉ giữ lại một số lượng số nguyên lớn và biến Miller-Rabin | 

Sự khác biệt quan trọng so với đệ quy nghĩa đen là thời gian chạy phụ thuộc vào số bit trong`C`, không phụ thuộc vào giá trị số của`C`. Chỉ với 101 ứng cử viên và một tập cơ sở Miller-Rabin cố định, lời giải dễ dàng tránh được các vấn đề thiên văn.`Theta(C log C)`công việc mô phỏng trực tiếp. 

## Trường hợp thử nghiệm```python
# helper: run the core solver on an input string
import sys
import io

C = int(
    "33931086844518982011982560935885732032396635556994207701963662088123265"
    "31417633033625453597120718116969886858499194160778011107392823626119960"
    "4691797570505851011072000000000000000000000000000"
)

BASES = [
    2, 3, 5, 7, 11, 13, 17, 19, 23, 29,
    31, 37, 41, 43, 47, 53, 59, 61, 67, 71,
    73, 79, 83, 89, 97, 101, 103, 107, 109, 113,
    127, 131, 137, 139, 149, 151, 157, 163, 167, 173,
    179, 181, 191, 193, 197, 199, 211, 223, 227, 229,
    233, 239, 241, 251, 257, 263, 269, 271, 277, 281,
    283, 293, 307, 311, 313, 317, 331, 337, 347, 349,
    353, 359, 367, 373, 379, 383, 389, 397, 401, 409,
    419, 421, 431, 433, 439, 443, 449, 457, 461, 463,
    467, 479, 487, 491, 499
]

def is_prime(n):
    if n < 2:
        return False

    for p in (2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37):
        if n == p:
            return True
        if n % p == 0:
            return False

    d = n - 1
    s = 0
    while d % 2 == 0:
        d //= 2
        s += 1

    for a in BASES:
        if a >= n:
            continue

        x = pow(a, d, n)

        if x == 1 or x == n - 1:
            continue

        for _ in range(s - 1):
            x = x * x % n
            if x == n - 1:
                break
        else:
            return False

    return True

def run(inp: str) -> str:
    kmin, kmax = map(int, inp.split())
    out = []

    for k in range(kmin, kmax + 1):
        n = C + k
        out.append("FALSE" if is_prime(n) else "TRUE")

    return "\n".join(out) + "\n"

# Provided sample 1.
assert run("0 1") == "TRUE\nFALSE\n", "sample 1"

# Provided sample 2.
assert run("99 100") == "TRUE\nTRUE\n", "sample 2"

# Minimum-size interval and the first value.
assert run("0 0") == "TRUE\n", "minimum interval"

# All-equal bounds. C + 50 is even.
assert run("50 50") == "TRUE\n", "equal bounds"

# Boundary at k = 100. C + 100 is even.
assert run("100 100") == "TRUE\n", "upper boundary"

# Consecutive values crossing the upper endpoint.
assert run("98 100") == "TRUE\nTRUE\nTRUE\n", "inclusive upper bound"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0`|`TRUE`| Khoảng tối thiểu và giá trị cơ sở tổng hợp | 
|`50 50`|`TRUE`| Bình đẳng`kmin`Và`kmax`, với một ứng cử viên đồng đều | 
|`100 100`|`TRUE`| Xử lý đúng mức tối đa cho phép`k`| 
|`98 100`|`TRUE`,`TRUE`,`TRUE`| Bao gồm giới hạn trên và xử lý liên tục | 

## Vỏ cạnh 

cho`0 0`, thuật toán xây dựng chính xác một ứng cử viên,`C`. Biểu diễn thập phân của nó kết thúc bằng 0, do đó bộ lọc số nguyên tố nhỏ ngay lập tức tìm thấy khả năng chia hết cho 2.`is_prime(C)`trả lại`False`, được đảo ngược thành`TRUE`. Không cần mô phỏng đệ quy. 

Vì`0 1`, ứng cử viên đầu tiên một lần nữa được công nhận ngay lập tức là hợp số. Ứng cử viên thứ hai vượt qua tất cả các cuộc kiểm tra số nguyên tố nhỏ và các bài kiểm tra Miller-Rabin, vì vậy nó được phân loại là số nguyên tố. Vì hàm đệ quy tương đương với một bài kiểm tra tổng hợp nên hai dòng đầu ra là`TRUE`Và`FALSE`. 

Vì`99 100`, vòng lặp sẽ thực hiện hai lần vì điểm cuối trên được bao gồm. Giá trị cho`k=99`được phân loại là composite, sản xuất`TRUE`. Giá trị cho`k=100`chẵn và bị từ chối bởi séc số nguyên tố nhỏ đầu tiên, cũng tạo ra`TRUE`. Trường hợp này đồng thời kiểm tra một hợp số lẻ và biên trên. 

Vì`50 50`, cả hai giới hạn đều giống nhau nên vòng lặp phải thực hiện chính xác một lần. Từ`C`là số chẵn và 50 là số chẵn,`C+50`thậm chí là tốt. Kết quả là một đơn`TRUE`dòng, xác nhận rằng số lượng đầu ra là một chứ không phải 0 hoặc 2. 

Bất biến chính đằng sau tất cả các trường hợp này là không thay đổi: phép đệ quy ban đầu hỏi liệu ứng cử viên lớn có ước số thích hợp hay không, trong khi thuật toán tối ưu hóa sẽ hỏi liệu ứng cử viên có phải là hợp số hay không. Hai vị từ này giống hệt nhau đối với mọi ứng viên trong bài toán này.
