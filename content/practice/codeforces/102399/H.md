---
title: "CF 102399H - \u0424\u043e\u043a\u0443\u0441 \u0441 \u0434\u0435\u043b\u0435\u043d\u0438\u0435\u043c \u0438 \u0443\u043c\u043d\u043e\u0436\u0435\u043d\u0438\u0435\u043c"
description: "Chúng ta có (n) số nguyên dương phân biệt (a1,ldots,an). Một thẻ chứa một số nguyên dương (x). Lấy tấm thẻ đó cho phép chúng ta thực hiện đúng một thao tác trên đúng một phần tử mảng: nhân nó với (x) hoặc chia cho (x) khi phép chia chính xác."
date: "2026-08-11T15:56:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102399
codeforces_index: "H"
codeforces_contest_name: "2019 \u041c\u043e\u0441\u043a\u043e\u0432\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432, \u043b\u0438\u0433\u0430 A"
rating: 0
weight: 102399
solve_time_s: 248
verified: true
draft: false
---

[CF 102399H - \u0424\u043e\u043a\u0443\u0441 \u0441 \u0434\u0435\u043b\u0435\u043d\u0438\u0435\u043c \u0438 \u0443\u043c\u043d\u043e\u0436\u0435\u043d\u0438\u0435\u043c](https://codeforces.com/problemset/problem/102399/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có (n) số nguyên dương phân biệt (a_1,\ldots,a_n). Một thẻ chứa một số nguyên dương (x). Lấy tấm thẻ đó cho phép chúng ta thực hiện đúng một thao tác trên đúng một phần tử mảng: nhân nó với (x) hoặc chia cho (x) khi phép chia chính xác. Sau đó thẻ biến mất nên giá trị tương tự (x) không thể được sử dụng lại. 

Mục tiêu là làm cho tất cả các phần tử mảng bằng nhau trong khi lấy càng ít thẻ càng tốt. Đầu vào cung cấp số phần tử và các giá trị mảng riêng biệt theo thứ tự tăng dần. Đầu ra là số lượng thẻ tối thiểu phải được tiêu thụ. 

Tuyên bố cuộc thi ban đầu có giới hạn thời gian là 1 giây và giới hạn bộ nhớ là 512 MB. Với (n) lên tới (200.000), giải pháp (O(n^2)) sẽ thực hiện khoảng (4\cdot10^{10}) kiểm tra cặp trong trường hợp xấu nhất, vượt xa những gì phù hợp với giới hạn cuộc thi một giây. Chúng ta cần một thuật toán cơ bản tuyến tính hoặc gần tuyến tính. 

Có một số chi tiết dễ bỏ sót. Đầu tiên, thẻ là duy nhất trên toàn cầu. Nếu 2 phần tử đều cần thẻ (2) thì chúng ta không thể thực hiện cả 2 thao tác với thẻ đó. Ví dụ,```
5
2 3 6 12 18
```có câu trả lời (5), không phải (4). Việc chọn giá trị cuối cùng (6) có vẻ đầy hứa hẹn vì mọi phần tử đều có thể so sánh được với (6): giá trị thẻ bắt buộc là (3,2,2,3). Việc lặp lại (2) và lặp lại (3) khiến kế hoạch đó không thể thực hiện được. Một giải pháp bất cẩn chỉ kiểm tra tính chia hết và đếm một thao tác cho mỗi phần tử sẽ trả về sai (4). 

Thứ hai, giá trị cuối cùng không có trong mảng ban đầu không thể đưa ra nghiệm (n-1). Với (n-1) thao tác, ít nhất một phần tử không nhận được thao tác nào vì mọi thao tác chỉ ảnh hưởng đến một phần tử. Phần tử không bị ảnh hưởng đó vẫn bằng giá trị ban đầu của nó, vì vậy giá trị cuối cùng chung phải là một trong các phần tử mảng ban đầu. Ví dụ,```
3
6 10 15
```không thể giải quyết bằng hai lá bài. Giá trị (30) cho phép mọi phần tử tiếp cận mục tiêu bằng một thẻ, sử dụng các thẻ (5,3,2), nhưng (30) không có trong mảng nên cả ba phần tử phải được thay đổi. Câu trả lời đúng là (3). 

Thứ ba, (n=1) không cần thẻ nào vì phần tử đơn lẻ đã bằng chính nó. Ví dụ,```
1
42
```có câu trả lời (0). Đây cũng là cách có ý nghĩa duy nhất để kiểm tra trường hợp "tất cả các phần tử bằng nhau", vì đầu vào đảm bảo rằng các phần tử mảng là khác biệt. 

Cuối cùng, số học liên quan đến (10^{18}) cần được quan tâm trong các ngôn ngữ có số nguyên có chiều rộng cố định. Ngay cả LCM trung gian cũng có thể lớn hơn nhiều so với (10^{18}), mặc dù bản thân các số nguyên Python không bị tràn. Việc triển khai vẫn giới hạn LCM vì một khi vượt quá (10^{18}), nó không bao giờ có thể chia một phần tử mảng. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp bắt đầu bằng việc nhận thấy rằng câu trả lời cuối cùng bị hạn chế một cách đáng ngạc nhiên. 

Giả sử chúng ta muốn làm cho mọi phần tử đều bằng nhau bằng cách sử dụng thẻ (k). Mỗi thẻ chỉ thay đổi một phần tử, vì vậy nếu (k<n-1), ít nhất hai phần tử mảng không bao giờ được chạm vào. Ban đầu chúng khác biệt và vẫn còn khác biệt, vì vậy sự bình đẳng là không thể. Như vậy mọi giải pháp đều cần ít nhất (n-1) thẻ. 

Giải pháp thẻ (n) luôn tồn tại. Gọi (g) là GCD của tất cả các phần tử mảng. Chia mọi (a_i) cho (a_i/g), gửi mọi phần tử tới (g). Tất cả các giá trị thẻ bắt buộc đều khác biệt vì (a_i) ban đầu là khác biệt. Như vậy (n) thẻ luôn là đủ. 

Do đó, câu trả lời là (n-1) hoặc (n). 

Bây giờ hãy xem xét khi nào có thể có (n-1) thẻ. Chính xác một phần tử mảng có thể không bị ảnh hưởng. Đặt giá trị của nó là (y). Vì giá trị cuối cùng là (y), nên mọi phần tử khác (a_i) phải đạt tới (y) trong đúng một thao tác. 

Một thao tác có thể thay đổi (a_i) thành (y) chỉ trong một trong hai trường hợp. Nếu (a_i<y) thì (a_i) phải chia (y), và quân bài cần lấy là (y/a_i). Nếu (a_i>y) thì (y) phải chia (a_i), và lá bài cần lấy là (a_i/y). 

Thẻ yêu cầu được xác định hoàn toàn bởi (a_i) và (y). Vì thẻ không thể được sử dụng lại nên tất cả các tỷ lệ này phải khác nhau. 

Vì vậy, một phần tử (y=a_i) là mục tiêu (n-1) hợp lệ chính xác khi mọi phần tử mảng khác có thể so sánh với (y) dưới mức chia hết và tất cả các tỷ lệ kết quả đều khác biệt. 

Vấn đề còn lại là tìm tất cả các ứng viên có thể mà không kiểm tra tất cả (n) phần tử cho mọi vị trí mảng. 

Đối với một ứng cử viên (a_i), mọi phần tử nhỏ hơn phải chia (a_i). Tương tự, LCM của tất cả các phần tử trước (i) phải chia (a_i). Chúng ta có thể duy trì tiền tố LCM này trong khi quét mảng. LCM có thể được giới hạn ở (10^{18}+1), vì mỗi phần tử mảng có nhiều nhất là (10^{18}), do đó LCM lớn hơn không bao giờ có thể chia được một ứng cử viên. 

Tương tự, mọi phần tử lớn hơn phải chia hết cho (a_i). Tương tự, (a_i) phải chia GCD của tất cả các phần tử sau (i). Mảng hậu tố GCD đưa ra điều kiện này trong thời gian không đổi. 

Không thể có nhiều ứng cử viên. Nếu (a_i) và (a_j), với (i<j), đều là ứng cử viên, thì điều kiện cho (a_i) cho biết rằng (a_i\mid a_j). Vì các giá trị là khác nhau nên (a_j\ge 2a_i). Bắt đầu từ giá trị dương và không bao giờ vượt quá (10^{18}), điều này có thể xảy ra tối đa khoảng 60 lần. Điều đó cho phép chúng tôi kiểm tra mọi ứng cử viên dựa trên toàn bộ mảng. 

Đối với mỗi ứng cử viên, chúng tôi quét mảng, tính tỷ lệ thẻ cần thiết cho mọi phần tử khác và chèn tỷ lệ vào một bộ. Nếu một phần tử không chia hết cho ứng viên theo một trong hai hướng thì ứng viên đó sẽ thất bại. Nếu một tỷ lệ xuất hiện hai lần, thí sinh cũng thất bại vì sẽ yêu cầu cùng một thẻ hai lần. 

Phương pháp brute-force kiểm tra tất cả (n) mục tiêu có thể và quét tất cả (n) phần tử cho từng mục tiêu. Trường hợp xấu nhất là (O(n^2)), khoảng (4\cdot10^{10}) kiểm tra phần tử đích để tìm (n=200.000). Phương pháp tối ưu trước tiên lọc các ứng viên bằng cách sử dụng tiền tố LCM và hậu tố GCD và chỉ có (O(\log 10^{18})) trong số đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n\log 10^{18})) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc mảng đã sắp xếp và xây dựng mảng GCD hậu tố. Đối với mọi vị trí (i), mảng này lưu trữ GCD của tất cả các giá trị ở bên phải của (i). Đối với vị trí cuối cùng, hậu tố GCD là (0), điều này thuận tiện vì mọi số nguyên dương đều chia hết cho (0). 
2. Quét mảng từ trái sang phải trong khi vẫn duy trì LCM của tất cả các giá trị trước vị trí hiện tại. Nếu LCM này vượt quá (10^{18}), hãy thay thế nó bằng giá trị đặc biệt lớn hơn (10^{18}). LCM như vậy không bao giờ có thể chia bất kỳ phần tử mảng ứng cử viên nào. 
3. Chỉ đánh dấu (a_i) là mục tiêu có thể khi tiền tố LCM chia (a_i) và (a_i) chia hậu tố GCD. Điều kiện đầu tiên cho biết mọi phần tử nhỏ hơn đều có thể đạt tới (a_i) chỉ bằng một thao tác. Phần thứ hai cho biết mọi phần tử lớn hơn đều có thể đạt tới (a_i) chỉ bằng một thao tác. 
4. Đối với mọi mục tiêu có thể có (y), hãy quét tất cả các phần tử mảng ngoại trừ (y). Nếu (x<y), hãy tính lá bài cần lấy là (y/x). Nếu (x>y), hãy tính nó là (x/y). Thí sinh đảm bảo rằng các phép chia này là chính xác, do đó không cần kiểm tra tính chia hết bổ sung ở đây. 
5. Chèn mọi tỷ lệ cần thiết vào một bộ. Nếu tỷ lệ tương tự xuất hiện hai lần, hãy từ chối mục tiêu này vì lá bài tương ứng chỉ tồn tại một lần trên bàn. Nếu mọi tỷ lệ đều khác biệt thì mục tiêu sẽ đưa ra cấu trúc thẻ (n-1) hợp lệ, do đó xuất ra ngay lập tức (n-1). 
6. Nếu không có ứng viên nào làm việc thì ghi (n). Cấu trúc GCD được mô tả trước đó đảm bảo rằng (n) thẻ luôn đủ. 

### Tại sao nó hoạt động 

Giới hạn dưới (n-1) tuân theo vì ít thao tác hơn sẽ khiến ít nhất hai phần tử gốc riêng biệt không bị ảnh hưởng. Giải pháp phép toán (n-1) phải chạm vào mọi phần tử ngoại trừ một phần tử, vì vậy giá trị cuối cùng của nó phải bằng phần tử không được chạm tới đó. Sau đó, mọi phần tử đã thay đổi phải có thể chuyển đổi thành mục tiêu đó trong chính xác một thao tác, điều này có thể thực hiện được chính xác khi hai giá trị có thể chia hết theo cách này hay cách khác. Thẻ được yêu cầu là thương số của chúng, vì vậy tất cả thương số phải khác biệt. 

Điều kiện tiền tố LCM hoàn toàn tương đương với việc nói rằng mọi phần tử mảng nhỏ hơn đều chia ứng viên. Điều kiện hậu tố GCD hoàn toàn tương đương với việc nói rằng ứng viên chia hết mọi phần tử lớn hơn. Khi các điều kiện đó được giữ, quá trình quét tỷ lệ sẽ kiểm tra hạn chế duy nhất còn lại, đó là không yêu cầu giá trị thẻ hai lần. Do đó, thuật toán tìm ra nghiệm (n-1) chính xác khi có nghiệm đó. Nếu không tìm thấy, giới hạn dưới sẽ loại trừ mọi câu trả lời nhỏ hơn và cấu trúc GCD cung cấp giải pháp thẻ (n), vì vậy (n) là tối ưu. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

LIMIT = 10**18
INF = LIMIT + 1

def solve_case(a):
    n = len(a)

    # suffix_gcd[i] = gcd(a[i], ..., a[n-1])
    # We need the gcd strictly after i, so suffix_gcd[i + 1].
    suffix_gcd = [0] * (n + 1)
    for i in range(n - 1, -1, -1):
        suffix_gcd[i] = math.gcd(a[i], suffix_gcd[i + 1])

    candidates = []

    # pref_lcm is the LCM of elements strictly before i.
    pref_lcm = 1

    for i, x in enumerate(a):
        if pref_lcm <= LIMIT and x % pref_lcm == 0:
            if suffix_gcd[i + 1] % x == 0:
                candidates.append(i)

        if pref_lcm <= LIMIT:
            g = math.gcd(pref_lcm, x)
            multiplier = x // g

            if pref_lcm > LIMIT // multiplier:
                pref_lcm = INF
            else:
                pref_lcm *= multiplier

    # Try every possible untouched element.
    for idx in candidates:
        target = a[idx]
        used = set()

        for x in a:
            if x == target:
                continue

            if x < target:
                card = target // x
            else:
                card = x // target

            if card in used:
                break

            used.add(card)
        else:
            return n - 1

    return n

def main():
    n = int(input())
    a = list(map(int, input().split()))
    print(solve_case(a))

if __name__ == "__main__":
    main()
```Mảng hậu tố GCD được xây dựng từ phải sang trái.`suffix_gcd[i + 1]`chứa chính xác các phần tử sau vị trí`i`, vì vậy hãy kiểm tra`suffix_gcd[i + 1] % x == 0`xử lý phía bên phải của một ứng cử viên. 

Tiền tố LCM chỉ được cập nhật sau khi kiểm tra phần tử hiện tại, vì bản thân ứng cử viên không được là một phần của tiền tố. LCM được tính là (L/\gcd(L,x)\cdot x), điều này tránh được phép nhân không cần thiết với một thừa số chung. 

Giới hạn ở (10^{18}+1) là thước đo hiệu suất và độ rõ ràng. Khi tiền tố LCM lớn hơn (10^{18}), không có giá trị mảng nào trong tương lai có thể chia hết cho nó, vì tất cả các giá trị mảng tối đa là (10^{18}). Chúng ta không bao giờ cần biết chính xác LCM lớn hơn. 

Trong quá trình xác thực ứng viên, các điều kiện chia hết đã được thiết lập. Điều đó cho phép mã sử dụng phép chia số nguyên trực tiếp thay vì thực hiện một phép toán modulo khác cho mọi phần tử. 

Bộ này chứa các giá trị thẻ, không phải các giá trị mảng được chuyển đổi. Hai phần tử có thể cần cùng một thương số mặc dù chúng bắt đầu từ các giá trị khác nhau và đó chính xác là tình huống mà ứng viên phải bị từ chối. 

Python có các số nguyên có độ chính xác tùy ý, do đó không có hiện tượng tràn khi tính toán LCM. Giới hạn rõ ràng vẫn ngăn LCM phát triển lớn không cần thiết và giữ cho hoạt động bị giới hạn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
2
1 2
```Các điều kiện tiền tố LCM và hậu tố GCD để lại cả hai vị trí là ứng cử viên có thể, nhưng ứng cử viên đầu tiên đã thành công. 

| Ứng viên | Tiền tố LCM | Hậu tố GCD | Thẻ bắt buộc | Kết quả | 
| --- | --- | --- | --- | --- | 
| (1) | (1) | (2) | (2) | hợp lệ | 

Mục tiêu là (1). Phần tử (2) được chia cho thẻ (2), tạo ra (1). Chỉ cần một thẻ nên câu trả lời là (n-1=1). 

Điều này cũng xác nhận ranh giới nơi chính phần tử nhỏ nhất là mục tiêu. 

### Mẫu 2 

Đầu vào là```
5
2 3 6 12 18
```Việc lọc ứng viên tạo ra các kết quả sau. 

| Ứng viên | Tiền tố LCM | Hậu tố GCD | Điều kiện ứng viên | Thẻ bắt buộc | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| (2) | (1) | (3) | Thất bại | (3,\ldots) | Từ chối | 
| (3) | (2) | (6) | Thất bại | (2,\ldots) | Từ chối | 
| (6) | (6) | (6) | Vượt qua | (3,2,2,3) | Từ chối | 
| (12) | (6) | (18) | Thất bại | (6,4,2,2) | Từ chối | 
| (18) | (12) | (0) | Thất bại | (9,6,3,2) | Từ chối | 

Ứng cử viên thú vị là (6). Mọi giá trị mảng đều có thể so sánh với (6), nhưng chuỗi thẻ sẽ phải chứa cả (2) hai lần và (3) hai lần. Vì mỗi thẻ chỉ tồn tại một lần nên việc này không thể thực hiện được trong bốn thao tác. 

Không có mục tiêu nào có thể đưa ra giải pháp (n-1), vì vậy câu trả lời là (n=5). Cấu trúc năm lá bài tồn tại, ví dụ: bằng cách nhắm mục tiêu (72) và sử dụng thẻ (32,24,12,6,4). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log 10^{18})) | Có tối đa khoảng 60 mục tiêu ứng cử viên và mỗi ứng viên được đăng ký vào thời gian (O(n)) | 
| Không gian | (O(n)) | Mảng hậu tố GCD và danh sách ứng cử viên yêu cầu không gian tuyến tính | 

Hệ số logarit rất nhỏ vì (\log_2(10^{18})<60). Với (n=200.000), thuật toán thực hiện quá trình tiền xử lý tuyến tính và tối đa là khoảng 60 lần quét ứng viên. Giới hạn ứng viên mạnh hơn nhiều đối với các đầu vào thông thường và quá trình quét sẽ dừng ngay lập tức khi tìm thấy mục tiêu hợp lệ. Việc sử dụng bộ nhớ là tuyến tính và phù hợp thoải mái với giới hạn 512 MB. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

LIMIT = 10**18
INF = LIMIT + 1

def solution(inp: str) -> str:
    data = list(map(int, inp.split()))
    n = data[0]
    a = data[1:1 + n]

    suffix_gcd = [0] * (n + 1)
    for i in range(n - 1, -1, -1):
        suffix_gcd[i] = math.gcd(a[i], suffix_gcd[i + 1])

    candidates = []
    pref_lcm = 1

    for i, x in enumerate(a):
        if pref_lcm <= LIMIT and x % pref_lcm == 0:
            if suffix_gcd[i + 1] % x == 0:
                candidates.append(i)

        if pref_lcm <= LIMIT:
            g = math.gcd(pref_lcm, x)
            multiplier = x // g
            if pref_lcm > LIMIT // multiplier:
                pref_lcm = INF
            else:
                pref_lcm *= multiplier

    for idx in candidates:
        target = a[idx]
        used = set()

        for x in a:
            if x == target:
                continue

            if x < target:
                card = target // x
            else:
                card = x // target

            if card in used:
                break
            used.add(card)
        else:
            return str(n - 1)

    return str(n)

def run(inp: str) -> str:
    return solution(inp).strip()

# Provided samples
assert run("""2
1 2
""") == "1", "sample 1"

assert run("""5
2 3 6 12 18
""") == "5", "sample 2"

assert run("""3
239 717 1434
""") == "2", "sample 3"

# n = 1, already equal to itself
assert run("""1
42
""") == "0", "single element"

# A clean n-1 construction.
# Target 1 needs cards 2, 3, 4, 5, 6.
assert run("""6
1 2 3 4 5 6
""") == "5", "distinct card ratios"

# Target 6 works with cards 3 and 2.
assert run("""3
2 3 6
""") == "2", "interior target"

# No array element can be an n-1 target.
# Target 30 would use cards 5, 3, 2, so n cards are enough.
assert run("""3
6 10 15
""") == "3", "non-array target"

# Values close to the 1e18 boundary.
# Neither divides the other, so two cards are necessary.
assert run("""2
999999999999999999 1000000000000000000
""") == "2", "1e18 boundary"

# Maximum-size test.
# The target 1 works, requiring every card 2..200000 exactly once.
max_n = 200000
max_input = str(max_n) + "\n" + " ".join(map(str, range(1, max_n + 1))) + "\n"
assert run(max_input) == str(max_n - 1), "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 42`|`0`| Kích thước tối thiểu và các phần tử trống bằng nhau | 
|`1 2 3 4 5 6`|`5`| Mục tiêu hợp lệ ở giá trị nhỏ nhất với tất cả các thẻ riêng biệt | 
|`2 3 6`|`2`| Mục tiêu nội bộ và tỷ lệ thẻ riêng biệt | 
|`6 10 15`|`3`| Mục tiêu không phải mảng có thể sử dụng một thẻ cho mỗi phần tử nhưng không thể tiếp cận (n-1) | 
|`999999999999999999 1000000000000000000`|`2`| Giá trị tại ranh giới (10^{18}) | 
|`1..200000`|`199999`| Tối đa (n), tiền xử lý tuyến tính và cấu trúc (n-1) hợp lệ | 

## Vỏ cạnh 

Đối với một phần tử duy nhất,```
1
42
```ứng cử viên là (42), tiền tố LCM là (1) và hậu tố GCD là (0). Ứng viên hợp lệ, bộ tỷ lệ trống và thuật toán trả về (1-1=0). Không cần thao tác. 

Đối với các yêu cầu thẻ lặp đi lặp lại,```
5
2 3 6 12 18
```ứng cử viên (6) vượt qua các điều kiện chia hết. Tỷ lệ yêu cầu của nó là (3,2,2,3). Tập hợp này phát hiện ngay lần xuất hiện thứ hai của (2) nên (6) không thể đưa ra nghiệm (n-1). Các ứng viên có thể còn lại không đạt được điều kiện chia hết, đưa ra câu trả lời đúng (5). 

Đối với một mục tiêu bên ngoài mảng, hãy xem xét```
3
6 10 15
```Giá trị (30) chia hết cho cả ba phần tử và tỷ lệ (5,3,2) là khác nhau. Điều đó mang lại cấu trúc ba lá bài hợp lệ, nhưng nó không thể đưa ra cấu trúc hai lá bài vì không có phần tử ban đầu nào là (30). Với hai thao tác, một phần tử sẽ không bị ảnh hưởng và buộc giá trị cuối cùng phải là một trong (6,10,15). Thuật toán trả về chính xác (3). 

Đối với các giá trị gần giới hạn trên,```
2
999999999999999999 1000000000000000000
```không có số nào chia hết cho số kia. Không có giá trị ban đầu nào có thể là mục tiêu một lá bài, vì vậy câu trả lời không thể là (1). Hai thẻ là đủ bằng cách chia mỗi số cho chính nó và đạt (1), do đó câu trả lời là (2). Việc triển khai cũng không bao giờ dựa vào phép nhân tạo ra giá trị trên (10^{18}). 

Đối với LCM tiền tố lớn, giới hạn rất quan trọng. Giả sử tiền tố chứa các giá trị có LCM đã vượt quá (10^{18}). Không có giá trị mảng trong tương lai nào có thể chia hết cho tiền tố LCM đó, vì mọi giá trị trong tương lai tối đa là (10^{18}). Việc triển khai thay thế LCM bằng (10^{18}+1), vì vậy tất cả các ứng cử viên sau này đều bị từ chối mà không tạo được số nguyên khổng lồ. 

Đối với kích thước đầu vào tối đa, mảng```
1 2 3 ... 200000
```có (200.000) giá trị riêng biệt. Mục tiêu (1) yêu cầu các thẻ (2,3,\ldots,200000), tất cả đều khác biệt nên câu trả lời là (199999). Ứng viên được tìm thấy theo điều kiện tiền tố LCM ngay lập tức và quá trình quét tỷ lệ xác nhận rằng mọi thẻ được yêu cầu là duy nhất.
