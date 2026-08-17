---
title: "CF 102437F - \u0411\u044b\u0441\u0442\u0440\u044b\u0439 \u043f\u0435\u0440\u0435\u0432\u043e\u0434"
description: "Đây là một vấn đề tương tác. Không có đầu vào thông thường nào chứa số dư tài khoản. Người tương tác bí mật chọn số dư ban đầu (n), với (0 len n le 10^{18}) và chương trình của chúng tôi phải khám phá đủ thông tin về số dư đó để chuyển toàn bộ số dư đi."
date: "2026-08-16T09:33:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102437
codeforces_index: "F"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0427\u0435\u0442\u0432\u0451\u0440\u0442\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102437
solve_time_s: 247
verified: false
draft: false
---

[CF 102437F - \u0411\u044b\u0441\u0442\u0440\u044b\u0439 \u043f\u0435\u0440\u0435\u0432\u043e\u0434](https://codeforces.com/problemset/problem/102437/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 7s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Đây là một vấn đề tương tác. Không có đầu vào thông thường nào chứa số dư tài khoản. Người tương tác bí mật chọn số dư ban đầu (n), với (0 \le n \le 10^{18}) và chương trình của chúng tôi phải khám phá đủ thông tin về số dư đó để chuyển toàn bộ số dư đi. Truy vấn duy nhất là`withdraw x`. Nếu số dư hiện tại ít nhất là (x), người tương tác sẽ trả lời`accepted`và loại bỏ (x). Ngược lại nó trả lời`rejected`và giữ nguyên số dư. Chúng tôi có thể kết thúc bằng cách in`finish`, nhưng điều đó chỉ được chấp nhận khi số dư ẩn thực tế bằng 0. Tuyên bố chính thức xác nhận giao thức tương tác này và giới hạn truy vấn (q+10), trong đó (q) là số nguyên nhỏ nhất thỏa mãn (n\le2^q). 

Thử thách không chỉ đơn thuần là tìm ra (n), mà là thực hiện nó với rất ít sự so sánh mang tính phá hoại. Vì (n) có thể lớn bằng (10^{18}), chiến lược thực hiện một lần rút mỗi đô la có thể yêu cầu (10^{18}) truy vấn. Ngay cả một tìm kiếm nhị phân thông thường trong toàn bộ khoảng ([0,10^{18}]) cũng sẽ sử dụng khoảng 60 truy vấn, con số này đã là quá nhiều khi (n) nhỏ. Ví dụ: nếu (n=1), thì (q=0), do đó, tối đa 10 lần thử được cho phép. 

Có hai trường hợp đáng được quan tâm đặc biệt. Nếu (n=0), tương tác đúng có thể chỉ là`withdraw 1`, nhận`rejected`, theo sau là`finish`. Một chiến lược bắt đầu thử nghiệm một cách mù quáng các lũy thừa lớn của 2 sẽ lãng phí nhiều truy vấn và vượt quá giới hạn cho (q=0). Nếu (n=1) thì đầu tiên`withdraw 1`được chấp nhận, nhưng điều đó không tự nó chứng tỏ rằng tài khoản trống. Một giây`withdraw 1`cần thiết để phân biệt (n=1) với (n\ge2). Ví dụ, sự tương tác mẫu```
withdraw 42
withdraw 1
withdraw 1
finish
```với câu trả lời```
rejected
accepted
rejected
```chứng minh rằng số dư ẩn chính xác là 1. Lần từ chối đầu tiên cho ra (n<42), lần rút tiền được chấp nhận đầu tiên cho ra (n\ge1) và lần từ chối thứ hai chứng tỏ rằng sau khi loại bỏ một đô la, không còn lại gì. Các mẫu chính thức chứa chính xác sự tương tác này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử nhiều lần`withdraw 1`. Mỗi truy vấn thành công sẽ loại bỏ chính xác một đô la, do đó rõ ràng là nó đúng và cuối cùng tài khoản sẽ trống rỗng. Vấn đề là số lượng hoạt động. Đối với (n=10^{18}), điều này yêu cầu chính xác (10^{18}) lần thử, trong khi trình tương tác chỉ cho phép (q+10), với (q=60). Cách tiếp cận này không khả thi từ xa. 

Một ý tưởng hứa hẹn hơn là sử dụng lũy ​​thừa của hai. Nếu bằng cách nào đó chúng tôi biết rằng số dư nằm giữa (2^k) và (2^{k+1}-1), thì việc rút (2^{k-1},2^{k-2},\ldots,1) sẽ trích xuất biểu diễn nhị phân còn lại của nó trong tối đa (k) truy vấn. Phần còn thiếu là làm thế nào để khám phá (k) mà không tốn thêm (k) truy vấn chỉ để kiểm tra lũy thừa của hai. 

Quan sát quan trọng là việc rút tiền thành công có thể được coi là so sánh với số dư ban đầu. Giả sử chúng ta đã rút chính xác (s) đô la, do đó tài khoản hiện tại chứa (n-s). Để hỏi liệu số dư ban đầu có phải là mục tiêu (T) hay không, chúng ta có thể yêu cầu`withdraw T-s`. Nếu được chấp nhận thì (n-s\ge T-s), tương đương với (n\ge T). Sau lần truy vấn thành công đó, tổng số tiền rút sẽ trở thành chính xác (T). Nếu nó bị từ chối, tổng số tiền rút vẫn còn (s) và chúng tôi đã học được (n<T). 

Điều này cho phép chúng tôi thực hiện tìm kiếm nhị phân theo số mũ của lũy thừa lớn nhất của hai không vượt quá (n), trong khi mọi so sánh thành công chỉ đơn giản là di chuyển số tiền đã rút về lũy thừa được kiểm tra. Chỉ cần khoảng sáu truy vấn để xác định số mũ đó vì chỉ có 60 số mũ có thể có. Sau đó, số dư còn lại nhỏ hơn lũy thừa lớn nhất đã biết của 2, do đó biểu diễn nhị phân của nó có thể được trích xuất trực tiếp. 

Phương pháp brute-force sử dụng một truy vấn cho mỗi đô la, trong khi phương pháp tối ưu sử dụng một số lượng truy vấn không đổi để xác định độ lớn và sau đó một truy vấn cho mỗi chữ số nhị phân. Sự khác biệt là rất quan trọng vì bản thân giới hạn truy vấn là logarit theo (n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n)) truy vấn | (O(1)) | Quá chậm | 
| Tối ưu | (O(\log n)) truy vấn | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xin một đô la. Nếu người tương tác từ chối nó, số dư ẩn bằng 0, vì vậy hãy in`finish`. Điều này xử lý trường hợp (n=0) trong một truy vấn. 
2. Nếu đồng đô la đầu tiên được chấp nhận, hãy yêu cầu thêm một đô la nữa. Nếu nó bị từ chối, số dư ban đầu chính xác là một, vì vậy hãy in`finish`. Sau hai lần rút một đô la được chấp nhận, chúng ta biết rằng (n\ge2) và chính xác hai đô la đã bị xóa. 
3. Duy trì`paid`, tổng số tiền đã rút. Ban đầu`paid = 2`. Bây giờ chúng ta tìm số mũ lớn nhất (f) sao cho (2^f\le n). Vì (n\le10^{18<2^{60}), chỉ cần tìm kiếm số mũ từ 1 đến 59 là đủ. 
4. Tìm kiếm nhị phân số mũ. Đối với số mũ ứng viên (m), hãy`target = 2^m`. Nếu như`target <= paid`, thì (n\ge pay\ge target) đã được biết nên không cần truy vấn. Nếu không thì yêu cầu`withdraw target - paid`. Một phản hồi được chấp nhận chứng minh (n\ge target) và chúng tôi cập nhật`paid`ĐẾN`target`. Một phản hồi bị từ chối chứng tỏ (n<mục tiêu), do đó số mũ ứng cử viên quá lớn. 
5. Sau khi tìm kiếm số mũ,`paid = 2^f`và (2^f\le n<2^{f+1}). Do đó, số dư còn lại nhỏ hơn (2^f). Kiểm tra lũy thừa (2^{f-1},2^{f-2},\ldots,1) theo thứ tự giảm dần. Bất cứ khi nào một truy vấn được chấp nhận, chữ số nhị phân đó sẽ xuất hiện và bị xóa khỏi tài khoản. Từ chối có nghĩa là chữ số đó không có. 
6. Khi tất cả các lũy thừa này đã được kiểm tra, mọi chữ số nhị phân có thể có bên dưới (2^f) đều bị xóa. Tài khoản trống nên hãy in`finish`. 

### Tại sao nó hoạt động 

Bất biến trung tâm là`paid`luôn chính xác bằng tổng số tiền được rút khỏi tài khoản ban đầu. Do đó số dư hiện tại là (n-\text{đã thanh toán}). Bất cứ khi nào chúng tôi muốn kiểm tra xem (n\ge T) và (T>\text{trả phí}), truy vấn`withdraw T-paid`được chấp nhận chính xác khi (n-\text{trả tiền}\ge T-\text{trả tiền}), chính xác là (n\ge T). Một truy vấn thành công cũng thay đổi`paid`đến (T), bảo toàn bất biến. 

Do đó, việc tìm kiếm số mũ tìm thấy lũy thừa lớn nhất của hai không vượt quá (n). Một khi sức mạnh đó đã bị rút đi, số tiền còn lại hoàn toàn nhỏ hơn nó. Việc kiểm tra tất cả các lũy thừa nhỏ hơn theo thứ tự giảm dần chính xác là cách xây dựng tham lam của biểu diễn nhị phân, do đó mỗi đô la còn lại cuối cùng sẽ được chuyển đi. Không có truy vấn nào có thể để lại số dư khác 0 sau khi lũy thừa cuối cùng của một truy vấn đã được kiểm tra. 

Số lần thử cũng nằm trong giới hạn tương tác đặc biệt. Có nhiều nhất hai truy vấn ban đầu, nhiều nhất là sáu truy vấn tìm kiếm theo số mũ và nhiều nhất là 59 truy vấn có chữ số nhị phân. Vậy có nhiều nhất là 67 lần thử. Đối với số dư lớn nhất có thể, (q=60), do đó giới hạn là 70. Đối với số dư nhỏ hơn, tìm kiếm theo số mũ vẫn chỉ tốn một số truy vấn không đổi, trong khi trích xuất nhị phân cuối cùng tốn nhiều nhất (q) truy vấn, để lại khoảng trống bắt buộc là 10 truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ask(x):
    print(f"withdraw {x}", flush=True)
    response = input().strip()

    if response == "fail":
        sys.exit(0)

    return response == "accepted"

def finish():
    print("finish", flush=True)

# First distinguish n = 0 and n = 1.
if not ask(1):
    finish()
    sys.exit(0)

if not ask(1):
    finish()
    sys.exit(0)

# Two dollars have already been withdrawn.
paid = 2

# Find the largest f such that 2^f <= n.
lo = 1
hi = 59

while lo < hi:
    mid = (lo + hi + 1) // 2
    target = 1 << mid

    if target <= paid:
        lo = mid
    else:
        if ask(target - paid):
            paid = target
            lo = mid
        else:
            hi = mid - 1

f = lo

# Extract the remaining balance bit by bit.
power = 1 << (f - 1)

while power >= 1:
    if ask(power):
        pass
    power >>= 1

finish()
```các`ask`chức năng là nơi duy nhất giao tiếp với người tương tác. Nó in lệnh, xóa ngay lập tức và đọc câu trả lời. MỘT`fail`phản hồi phải chấm dứt chương trình ngay lập tức vì việc tiếp tục sẽ vi phạm giao thức. 

Hai cuộc gọi đầu tiên đến`ask(1)`là đặc biệt. Đầu tiên phân biệt số 0 với số dư dương. Cái thứ hai phân biệt một với ít nhất hai. Sau khi cả hai cuộc gọi thành công,`paid`chính xác là 2, cho chúng ta số tiền đã biết đã bị xóa khỏi số dư ban đầu. 

Trong quá trình tìm kiếm số mũ, biểu thức`target - paid`luôn dương vì truy vấn chỉ được thực hiện khi`target > paid`. Nó cũng nhiều nhất là (2^{59}), thấp hơn số lượng truy vấn tối đa được phép là (10^{18}). Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn. 

Vòng cuối cùng không cần duy trì một biến riêng cho số dư tài khoản vãng lai. Mỗi lần rút điện thành công chỉ cần loại bỏ chữ số nhị phân đó. Bởi vì công suất được kiểm tra từ lớn nhất đến nhỏ nhất nên tại mọi thời điểm, công suất được kiểm tra không lớn hơn phạm vi cân bằng có thể còn lại. 

Chương trình không có đầu vào thông thường để phân tích vì đây là một tác vụ tương tác. Yêu cầu`input = sys.stdin.readline`khai báo vẫn được sử dụng để đọc phản hồi của người tương tác, theo yêu cầu của quy ước triển khai Python. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Tương tác mẫu tương ứng với số dư ban đầu chính xác là 1. Bảng điểm của nó là:```
withdraw 42
rejected
withdraw 1
accepted
withdraw 1
rejected
finish
```Việc triển khai của chúng tôi đạt được kết luận tương tự thông qua một bản ghi hơi khác vì nó bắt đầu bằng việc kiểm tra một đô la. 

| Bước | Truy vấn | Phản hồi cho (n=1) |`paid`sau bước | Ý nghĩa | 
| --- | --- | --- | --- | --- | 
| 1 |`withdraw 1`|`accepted`| 1 | (n\ge1) | 
| 2 |`withdraw 1`|`rejected`| 1 | (n<2), do đó (n=1) | 
| 3 |`finish`|`OK`| 1 | Tài khoản trống | 

Dấu vết cho thấy tại sao truy vấn một đô la thứ hai là cần thiết. Một lần rút tiền được chấp nhận không thể phân biệt (n=1) với (n=2) hoặc bất kỳ số dư dương nào lớn hơn. 

### Mẫu 2 

Mẫu thứ hai tương ứng với số dư ban đầu bằng 0:```
withdraw 1
rejected
finish
```| Bước | Truy vấn | Phản hồi cho (n=0) |`paid`sau bước | Ý nghĩa | 
| --- | --- | --- | --- | --- | 
| 1 |`withdraw 1`|`rejected`| 0 | (n<1), do đó (n=0) | 
| 2 |`finish`|`OK`| 0 | Tài khoản trống | 

Đây là trường hợp giá trị nhỏ quan trọng. Chiến lược luôn thực hiện tìm kiếm lũy thừa dài của hai sẽ vượt quá giới hạn truy vấn (q+10=10) ở đây, trong khi thuật toán được đề xuất sẽ dừng sau một lần rút tiền. 

### Một ví dụ lớn hơn 

Hãy xem xét (n=13). Hai lần rút 1 đô la đầu tiên để lại 11 đô la và đặt`paid=2`. Trong quá trình tìm kiếm số mũ, thuật toán cuối cùng chứng minh rằng (2^3=8\le13) nhưng (2^4=16>13). Việc so sánh thành công với 8 sẽ rút 6 đô la còn lại cần thiết để thực hiện`paid=8`. Tài khoản hiện có 5 đô la. 

Phép trích xuất nhị phân cuối cùng kiểm tra 4, 2 và 1. Truy vấn cho 4 thành công, để lại 1 đô la; truy vấn cho 2 bị từ chối; truy vấn cho 1 thành công. Tổng số tiền rút là (8+4+1=13), do đó`finish`là an toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(\log n)) truy vấn tương tác | Tìm kiếm số mũ có kích thước không đổi được theo sau bởi một truy vấn cho mỗi chữ số nhị phân | 
| Không gian | (O(1)) | Chỉ một số biến số nguyên và phản hồi tương tác hiện tại được lưu trữ | 

Số dư được giới hạn bởi (10^{18}), do đó có tối đa 60 vị trí nhị phân liên quan. Số lần rút tiền trong trường hợp xấu nhất là nhiều nhất là 67, dưới mức 70 lần cho phép khi (n) gần (10^{18}). Đối với (n nhỏ), số lượng truy vấn nhị phân cuối cùng giảm theo (q), trong khi tìm kiếm số mũ vẫn bị giới hạn bởi sáu truy vấn, do đó giới hạn (q+10) được thỏa mãn trong toàn bộ phạm vi. 

## Trường hợp thử nghiệm 

Vì tác vụ ban đầu có tính tương tác nên các mẫu của nó không phải là các trường hợp kiểm thử stdin/stdout thông thường. Khai thác thử nghiệm ngoại tuyến hữu ích phải mô phỏng số dư ẩn và xác minh rằng mọi khoản rút tiền được tạo là hợp pháp, số dư cuối cùng bằng 0 và số lần thử không vượt quá (q+10). Các thử nghiệm sau đây phản ánh thuật toán đã gửi.```python
import sys
import io

def offline_commands(n):
    balance = n
    commands = []

    def ask(x):
        nonlocal balance

        assert 1 <= x <= 10**18
        commands.append(("withdraw", x))

        if balance >= x:
            balance -= x
            return True
        return False

    if not ask(1):
        commands.append(("finish",))
        return commands, balance

    if not ask(1):
        commands.append(("finish",))
        return commands, balance

    paid = 2

    lo = 1
    hi = 59

    while lo < hi:
        mid = (lo + hi + 1) // 2
        target = 1 << mid

        if target <= paid:
            lo = mid
        else:
            if ask(target - paid):
                paid = target
                lo = mid
            else:
                hi = mid - 1

    f = lo
    power = 1 << (f - 1)

    while power >= 1:
        ask(power)
        power >>= 1

    commands.append(("finish",))
    return commands, balance

def run(n):
    commands, balance = offline_commands(n)

    q = 0 if n == 0 else (n - 1).bit_length()
    attempts = sum(1 for command in commands if command[0] == "withdraw")

    assert balance == 0
    assert commands[-1] == ("finish",)
    assert attempts <= q + 10

    return commands

def check_sample_1():
    balance = 1
    transcript = [
        ("withdraw", 42, False),
        ("withdraw", 1, True),
        ("withdraw", 1, False),
    ]

    for _, x, accepted in transcript:
        actual = balance >= x
        assert actual == accepted

        if actual:
            balance -= x

    assert balance == 0

def check_sample_2():
    balance = 0
    transcript = [
        ("withdraw", 1, False),
    ]

    for _, x, accepted in transcript:
        actual = balance >= x
        assert actual == accepted

        if actual:
            balance -= x

    assert balance == 0

check_sample_1()
check_sample_2()

# Minimum-size cases.
assert run(0)[-1] == ("finish",), "zero balance"
assert run(1)[-1] == ("finish",), "one dollar"

# Boundary between q = 1 and q = 2.
assert run(2)[-1] == ("finish",), "exact power of two"
assert run(3)[-1] == ("finish",), "just above a power of two"

# Large power of two, where the exponent reaches 59.
assert run(1 << 59)[-1] == ("finish",), "2^59"

# Maximum allowed initial balance.
assert run(10**18)[-1] == ("finish",), "maximum balance"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| (n=0) |`finish`sau một lần rút tiền bị từ chối | Số dư tối thiểu và giới hạn truy vấn (q=0) | 
| (n=1) |`finish`sau khi phân biệt được lần rút tiền thứ hai | Số dư dương nhỏ nhất và ranh giới chênh lệch | 
| (n=2) |`finish`với sức mạnh chính xác của hai người đã rút hoàn toàn | Xử lý sức mạnh chính xác của hai | 
| (n=3) |`finish`sau khi trích xuất biểu diễn nhị phân (11_2) | Giá trị ngay trên lũy thừa hai | 
| (n=2^{59}) |`finish`| Số mũ nhị phân có liên quan cao nhất | 
| (n=10^{18}) |`finish`| Số dư tối đa được phép và giới hạn số lượng truy vấn | 

## Vỏ cạnh 

Đối với (n=0), trạng thái đầu vào chính xác là số dư ẩn bằng 0, vì vậy lệnh đầu tiên là`withdraw 1`. Bộ tương tác từ chối nó vì (0<1) và chương trình sẽ in ngay lập tức`finish`. Chỉ có một lần thử được thực hiện, trong khi (q=0) cho phép thực hiện mười lần. 

Với (n=1), sự tương tác bắt đầu bằng`withdraw 1`, được chấp nhận và để lại số 0. Chương trình không thể đơn giản kết thúc vào thời điểm này vì phản hồi tương tự cũng sẽ xảy ra với mọi (n\ge1). Nó gửi`withdraw 1`một lần nữa, nhận được`rejected`, và bây giờ biết rằng số dư ban đầu nhỏ hơn hai. Kết hợp với lần rút tiền được chấp nhận đầu tiên, điều này chứng tỏ (n=1). Chương trình sẽ kết thúc sau hai lần thử, thấp hơn nhiều (q+10=10). 

Đối với (n=2), cả hai truy vấn một đô la ban đầu đều được chấp nhận, vì vậy`paid=2`và tài khoản thật trống rỗng. Việc tìm kiếm số mũ biết rằng số mũ 1 đã hợp lệ vì`paid`chính nó bằng (2^1). Nó không đưa ra một truy vấn có giá trị bằng 0. Các cuộc kiểm tra sức mạnh còn lại đều hết tiền và tất cả đều bị từ chối, sau đó`finish`là đúng. Điều này tránh được một lỗi ranh giới phổ biến khi việc triển khai vô tình cố gắng`withdraw 0`. 

Với (n=3), hai lần rút tiền đầu tiên lại được thiết lập`paid=2`. Tìm kiếm số mũ tìm thấy (f=1), vì (2\le3<4). Số dư còn lại là một nên cuối cùng`withdraw 1`thành công và loại bỏ nó. Việc trích xuất nhị phân đã biểu thị giá trị ban đầu là (2+1), chính xác theo yêu cầu. 

Đối với (n=2^{59}), tìm kiếm số mũ đạt lũy thừa lớn nhất cho phép (2^{59}). Sau lần so sánh thành công đó,`paid`bằng toàn bộ số dư. Các thử nghiệm cuối cùng sử dụng lũy ​​thừa từ (2^{58}) xuống 1, tất cả đều bị từ chối. Trường hợp này thực hiện giới hạn số mũ trên mà không cần truy vấn (2^{60}), vượt quá số tiền rút được phép là (10^{18}). 

Với (n=10^{18}), số dư ban đầu lớn nhất có thể có (q=60). Đầu tiên, thuật toán rút hai đô la, sử dụng tối đa sáu truy vấn bổ sung để xác định mức độ phù hợp cao nhất và sau đó sử dụng tối đa 59 truy vấn có chữ số nhị phân. Ngay cả trong trường hợp tương tác tồi tệ nhất, con số này tối đa là 67 lần rút tiền, dưới mức cho phép (q+10=70). Việc triển khai cũng nằm trong mức tối đa được phép (10^{18}) cho mỗi lần rút tiền riêng lẻ.
