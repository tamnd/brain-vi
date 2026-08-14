---
title: "CF 102437F - \u0411\u044b\u0441\u0442\u0440\u044b\u0439 \u043f\u0435\u0440\u0435\u0432\u043e\u0434"
description: "Đây là một vấn đề tương tác. Có một số dư không âm ẩn (n), với (n le 10^{18}) và chương trình không nhận (n) như đầu vào thông thường. Thay vào đó, nó có thể hỏi thiết bị đầu cuối xem có còn lại ít nhất (x) đô la hay không bằng cách rút tiền x."
date: "2026-08-14T15:41:08+07:00"
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

Đây là một vấn đề tương tác. Có một số dư không âm ẩn (n), với (n \le 10^{18}) và chương trình không nhận (n) như đầu vào thông thường. Thay vào đó, nó có thể hỏi thiết bị đầu cuối xem có còn lại ít nhất (x) đô la bằng cách phát hành`withdraw x`. MỘT`accepted`câu trả lời có nghĩa là (x) đô la được loại bỏ khỏi số dư hiện tại, trong khi`rejected`nghĩa là số dư nhỏ hơn (x) và không thay đổi. Khi chương trình tin rằng số dư bằng 0, nó sẽ in`finish`. 

Giới hạn truy vấn phụ thuộc vào ẩn số (n). Gọi (q) là số nguyên nhỏ nhất thỏa mãn (n \le 2^q). Thiết bị đầu cuối cho phép tối đa (q+10) lần rút tiền. Điều này làm cho số lượng truy vấn trở thành thước đo độ phức tạp thực sự. Vì (10^{18<2^{60}), một chiến lược cố định sử dụng tất cả 60 lũy thừa của 2 là đúng, nhưng nó có thể thực hiện 60 truy vấn ngay cả khi (n) rất nhỏ. Ví dụ: khi (n=0), giới hạn chỉ là 10 truy vấn, do đó việc quét 60 truy vấn vô điều kiện sẽ không hợp lệ. 

Giải pháp đơn giản cũng nhạy cảm với sự khác biệt giữa một truy vấn được chấp nhận và biết rằng số dư bằng không. Nếu (n=5), truy vấn`withdraw 4`trả lại`accepted`, nhưng số dư còn lại là 1. Đang in`finish`ngay lập tức sẽ sai. Việc rút tiền thành công chỉ cho chúng tôi biết rằng số tiền được yêu cầu đã có sẵn. 

Số dư bằng 0 là một trường hợp ranh giới khác. Nếu (n=0),`withdraw 1`phải quay lại`rejected`, sau đó`finish`là đúng. Nếu (n=1), truy vấn tương tự trả về`accepted`, Và`finish`chỉ trở nên chính xác sau lần rút tiền đó. 

Quyền hạn chính xác của hai cũng rất hữu ích cho việc kiểm tra từng lỗi một. Với (n=2^k), truy vấn`withdraw 2^k`thành công và để lại kết quả chính xác bằng 0. Thuật toán vẫn phải có khả năng tiếp tục một cách an toàn vì nó thường không thể cho rằng một truy vấn được chấp nhận đã làm cạn kiệt tài khoản. Giá trị lớn nhất có thể (10^{18}) nằm dưới (2^{60}) nhưng cao hơn (2^{59}), vì vậy số mũ 59 là lũy thừa lớn nhất của 2 có thể được yêu cầu. Giá trị (2^{60}) sẽ vượt quá số tiền rút được phép và không cần thiết. 

Còn có một sự tinh tế nữa. Trong giai đoạn tối ưu hóa, số dư thay đổi sau mỗi truy vấn được chấp nhận, do đó các câu trả lời không tạo thành một vị từ đơn điệu thông thường về (n) ban đầu. Ví dụ: với (n=100), truy vấn cho 8 có thể được chấp nhận và giảm số dư xuống 92, sau đó truy vấn cho 64 có thể bị từ chối ngay cả khi số dư ban đầu là 100. Do đó, tìm kiếm nhị phân cần một đối số chính xác khác với tìm kiếm nhị phân thông thường. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thử mọi lũy thừa của hai từ (2^{59}) xuống (2^0). Bất cứ khi nào việc rút tiền được chấp nhận, bit đó sẽ bị xóa khỏi số dư hiện tại. Điều này hiệu quả vì mọi số nguyên không âm đều có một biểu diễn nhị phân duy nhất. Cuối cùng, mọi bit có thể đều đã được thử nên không còn lại gì. 

Vấn đề là số lần thử. Có chính xác 60 lũy thừa của 2 từ (2^0) đến (2^{59}), vì vậy trường hợp xấu nhất là 60 truy vấn. Tuy nhiên, đối với (n=0), (q=0) và thiết bị đầu cuối chỉ cho phép 10 lần thử. Thuật toán brute-force có thể đã thất bại ở số dư nhỏ nhất có thể. 

Quan sát quan trọng là chúng ta thực sự không cần xác định chính xác bit được đặt cao nhất. Chúng ta chỉ cần tìm số mũ (l) đủ nhỏ để sau một thời gian tìm kiếm ngắn, số dư còn lại nhỏ hơn (2^{l+1}). Khi đó việc phân rã nhị phân giảm dần thông thường có thể bắt đầu ở (l) thay vì 59. 

Chúng ta có thể thu được (l) như vậy bằng cách tìm kiếm nhị phân trên 60 số mũ có thể có. Đối với điểm giữa (m), chúng tôi cố gắng rút (2^m). Nếu thành công, chúng ta đặt (l=m). Nếu thất bại, chúng tôi đặt (r=m). Số dư có thể thay đổi trong quá trình này, vì vậy (l) không nhất thiết phải là số cao nhất của số dư ban đầu hoặc số dư hiện tại. Điều quan trọng là khi tìm kiếm kết thúc với (r=l+1), truy vấn bị từ chối cuối cùng sẽ đưa ra giới hạn về số dư hiện tại. Nếu truy vấn được chấp nhận hữu ích cuối cùng được thiết lập (l), thì mọi số mũ sau đó lớn hơn (l) đều bị từ chối và đặc biệt là số mũ biên (l+1) quá lớn so với số dư hiện tại. Do đó số dư hiện tại ở mức dưới (2^{l+1}). 

Việc tìm kiếm có nhiều nhất sáu truy vấn vì chỉ có 60 số mũ ứng cử viên và (60<2^6). Sau đó, tối đa (l+1) cần thêm truy vấn và (l) không bao giờ vượt quá thang logarit của số dư ban đầu. Do đó tổng số nhiều nhất là (q+7), thấp hơn mức cho phép (q+10). Đây là sự tối ưu hóa dự định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(60)) truy vấn | (O(1)) | Quá nhiều truy vấn cho nhỏ (n) | 
| Tối ưu | (O(q)) truy vấn, nhiều nhất là (q+7) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với khoảng số mũ ([l,r)=[0,60)). Chúng tôi sử dụng 60 vì mọi số dư được phép đều nhỏ hơn (2^{60}), nên mọi lũy thừa liên quan đều là (2^0,\ldots,2^{59}). 
2. Trong khi (r-l>1), chọn (m=(l+r)//2) và phát hành`withdraw ⟦PROTECT_2⟧`. Nếu thiết bị đầu cuối trả lời`accepted`, đặt (l=m). Việc rút tiền thực sự đã làm giảm số dư, nhưng (l) vẫn ghi lại một số mũ hữu ích có giá cả phải chăng tại thời điểm đó. Nếu câu trả lời là`rejected`, được đặt (r=m), vì số dư hiện tại chắc chắn thấp hơn (2^m). 
3. Sau khi tìm kiếm nhị phân, xử lý số mũ từ (l) xuống 0. Đối với mọi (i), vấn đề`withdraw ⟦PROTECT_3⟧`. Một phản hồi được chấp nhận sẽ loại bỏ bit nhị phân đó khỏi số dư còn lại. Một phản hồi bị từ chối đơn giản có nghĩa là bit đó bị thiếu. 
4. Sau khi thử tất cả các số mũ từ (l) đến 0, hãy in`finish`. Tìm kiếm nhị phân đã đảm bảo rằng số dư còn lại ở dưới (2^{l+1}), vì vậy mọi bit còn lại có thể có hiện đã được xem xét. 

Bất biến trung tâm là số dư không bao giờ tăng và mọi truy vấn được chấp nhận sẽ loại bỏ chính xác số tiền được yêu cầu. Khi kết thúc tìm kiếm nhị phân, tìm kiếm đạt đến (l=59) sau khi rút thành công (2^{59}), trong trường hợp đó số dư còn lại ở dưới (2^{59}) hoặc tìm kiếm có ranh giới (r=l+1) được tạo bởi truy vấn bị từ chối cho (2^r). Trong trường hợp sau, số dư hiện tại ở mức dưới (2^{l+1}). Do đó, lần quét giảm dần cuối cùng từ (l) xuống 0 là đủ để loại bỏ từng đô la còn lại. 

Truy vấn bị ràng buộc tuân theo cùng một cấu trúc. Tìm kiếm nhị phân sử dụng tối đa sáu lần thử. Lần quét cuối cùng sử dụng tối đa (l+1) lần thử. Vì các khoản rút tiền được chấp nhận chỉ làm giảm số dư nên (l) không thể vượt quá thang logarit của số dư ban đầu. Do đó, tổng số tối đa là (q+7), không sử dụng ba lần thử với mức cho phép bắt buộc (q+10). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    def ask(x):
        print(f"withdraw {x}", flush=True)
        response = input().strip()
        if response == "fail":
            sys.exit(0)
        return response

    l, r = 0, 60

    while r - l > 1:
        m = (l + r) // 2
        response = ask(1 << m)

        if response == "accepted":
            l = m
        else:
            r = m

    for i in range(l, -1, -1):
        ask(1 << i)

    print("finish", flush=True)

if __name__ == "__main__":
    solve()
```các`ask`chức năng là nơi duy nhất diễn ra giao tiếp với người tương tác. Nó in lệnh rút tiền, xóa ngay lập tức theo yêu cầu của giao thức và đọc phản hồi. các`fail`phản hồi gây ra sự chấm dứt ngay lập tức vì việc tiếp tục sau khi thiết bị đầu cuối đã bị khóa bị cấm rõ ràng. 

Tìm kiếm nhị phân sử dụng số mũ thay vì bản thân giá trị tiền tệ. Khoảng chứa các số nguyên từ 0 đến 59, vì vậy mỗi số tiền được yêu cầu tối đa là (2^{59<10^{18}). Số nguyên Python không bị tràn, nhưng cách triển khai tương tự trong C++ cũng sẽ phù hợp thoải mái với số nguyên 64 bit đã ký cho mọi truy vấn thực tế. 

Vòng lặp cuối cùng có chủ ý bắt đầu tại`l`, không`l + 1`hoặc 59. Một số quyền hạn có thể đã bị rút lại trong quá trình tìm kiếm nhị phân, vì vậy chúng có thể bị từ chối khi được truy vấn lại. Điều đó là vô hại. Vì số dư chỉ giảm đi nên lần rút tiền thành công trước đó không bao giờ có thể thành công lần thứ hai. 

Chương trình không cố gắng phát hiện số 0 từ một`accepted`phản ứng. Không có cách nào để phân biệt giữa tài khoản chứa chính xác (x) và tài khoản chứa nhiều hơn (x) sau khi được chấp nhận.`withdraw x`. Tiếp tục với chiến lược định trước sẽ tránh được sự mơ hồ đó. 

## Ví dụ đã hoạt động 

Các mẫu tương tác là bản ghi chứ không phải là các tệp đầu vào thông thường. Mẫu 1 phù hợp với số dư ban đầu là 1:`withdraw 42`bị từ chối,`withdraw 1`được chấp nhận và điều thứ hai`withdraw 1`bị từ chối vì số dư đã bằng 0. Mẫu 2 phù hợp với số dư ban đầu là 0. 

Đối với Mẫu 1, thuật toán tối ưu không phải tái tạo bản ghi mẫu. Sáu truy vấn của nó cho (n=1) được hiển thị bên dưới. 

| Bước | Số mũ | Rút tiền | Phản hồi | Số dư còn lại | 
| --- | --- | --- | --- | --- | 
| 1 | 30 | (2^{30}) | bị từ chối | 1 | 
| 2 | 15 | (2^{15}) | bị từ chối | 1 | 
| 3 | 7 | (2^7) | bị từ chối | 1 | 
| 4 | 3 | (2^3) | bị từ chối | 1 | 
| 5 | 1 | (2^1) | bị từ chối | 1 | 
| 6 | 0 | (2^0) | được chấp nhận | 0 | 

Việc tìm kiếm nhị phân kết thúc bằng (l=0) và lần quét cuối cùng sẽ loại bỏ một đô la. Thuật toán sau đó in`finish`. Bản ghi ngắn hơn của mẫu chỉ đơn giản là một tương tác hợp lệ khác cho cùng số dư ẩn. 

Đối với Mẫu 2, số dư ban đầu bằng 0. 

| Bước | Số mũ | Rút tiền | Phản hồi | Số dư còn lại | 
| --- | --- | --- | --- | --- | 
| 1 | 30 | (2^{30}) | bị từ chối | 0 | 
| 2 | 15 | (2^{15}) | bị từ chối | 0 | 
| 3 | 7 | (2^7) | bị từ chối | 0 | 
| 4 | 3 | (2^3) | bị từ chối | 0 | 
| 5 | 1 | (2^1) | bị từ chối | 0 | 
| 6 | 0 | (2^0) | bị từ chối | 0 | 

Việc tìm kiếm lại kết thúc với (l=0). Truy vấn cuối cùng xác nhận rằng không thể rút được đô la nào và`finish`là đúng. Chỉ có sáu lần thử được thực hiện, dưới giới hạn (q+10=10). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(q)) truy vấn tương tác | Tối đa 6 truy vấn tìm kiếm nhị phân cộng với tối đa (q+1) truy vấn cuối cùng | 
| Không gian | (O(1)) | Chỉ một số lượng biến số nguyên không đổi được lưu trữ | 

Vì (n\le10^{18<2^{60}) nên ta luôn có (q\le60). Do đó, thuật toán sử dụng tối đa 67 lần rút tiền, trong khi giao thức cho phép (q+10), tối thiểu là 10 và đạt tới 70 cho phạm vi logarit lớn nhất có thể. Việc thực hiện sử dụng bộ nhớ không đổi. 

## Trường hợp thử nghiệm 

Vì đây là tính tương tác nên đầu vào Codeforces thông thường không thể được sao chép bằng ngoại tuyến thông thường.`run(input_string)`người giúp đỡ. Cách hữu ích để kiểm tra logic cục bộ là thay thế bộ tương tác bằng một trình mô phỏng chứa số dư ẩn. Giống nhau`strategy`Sau đó, chức năng này được sử dụng bởi cả giải pháp thực và khai thác thử nghiệm.```python
import sys

def strategy(ask, finish):
    l, r = 0, 60

    while r - l > 1:
        m = (l + r) // 2
        response = ask(1 << m)

        if response == "accepted":
            l = m
        elif response == "rejected":
            r = m
        else:
            raise RuntimeError("unexpected interactor response")

    for i in range(l, -1, -1):
        ask(1 << i)

    finish()

def run_hidden(n):
    balance = n
    commands = []

    def ask(x):
        nonlocal balance
        assert 1 <= x <= 10**18

        commands.append(f"withdraw {x}")

        if balance >= x:
            balance -= x
            return "accepted"
        return "rejected"

    def finish():
        commands.append("finish")
        assert balance == 0

    strategy(ask, finish)
    return commands, balance

def check_sample_transcript(n, commands, replies):
    balance = n

    assert len(commands) == len(replies)

    for command, reply in zip(commands, replies):
        parts = command.split()

        if parts[0] == "withdraw":
            x = int(parts[1])
            expected = "accepted" if balance >= x else "rejected"

            assert reply == expected

            if expected == "accepted":
                balance -= x

        elif command == "finish":
            assert balance == 0
        else:
            raise AssertionError("invalid command")

    assert balance == 0

# Provided Sample 1.
sample1_commands = [
    "withdraw 42",
    "withdraw 1",
    "withdraw 1",
]
sample1_replies = [
    "rejected",
    "accepted",
    "rejected",
]
check_sample_transcript(1, sample1_commands, sample1_replies)

# Provided Sample 2.
sample2_commands = [
    "withdraw 1",
]
sample2_replies = [
    "rejected",
]
check_sample_transcript(0, sample2_commands, sample2_replies)

# Minimum balance.
commands, balance = run_hidden(0)
assert balance == 0
assert commands[-1] == "finish"
assert len(commands) <= 10

# Small boundary values.
commands, balance = run_hidden(1)
assert balance == 0
assert commands[-1] == "finish"

commands, balance = run_hidden(2)
assert balance == 0
assert commands[-1] == "finish"

commands, balance = run_hidden(3)
assert balance == 0
assert commands[-1] == "finish"

# Exact power of two near the upper range.
commands, balance = run_hidden(1 << 59)
assert balance == 0
assert commands[-1] == "finish"
assert len(commands) <= 59 + 10

# Maximum allowed balance.
commands, balance = run_hidden(10**18)
assert balance == 0
assert commands[-1] == "finish"
assert len(commands) <= 60 + 10

# Repeated equal hidden balances catch accidental state leakage.
results = [run_hidden(42) for _ in range(3)]
assert all(balance == 0 for _, balance in results)
assert results[0][0] == results[1][0] == results[2][0]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bảng điểm mẫu 1, ẩn (n=1) |`finish`, số dư 0 | Rút tiền thành công theo sau là một truy vấn bị từ chối dư thừa | 
| Bảng điểm mẫu 2, ẩn (n=0) |`finish`, số dư 0 | Số dư tối thiểu và trạng thái 0 ngay lập tức | 
| (n=0) |`finish`, số dư 0 | Ngân sách truy vấn nghiêm ngặt (q) nhỏ nhất có thể và | 
| (n=1,2,3) |`finish`, số dư 0 | Số dư khác 0 thấp nhất và hành vi ranh giới bit | 
| (n=2^{59}) |`finish`, số dư 0 | Sức mạnh liên quan cao nhất của hai | 
| (n=10^{18}) |`finish`, số dư 0 | Số dư tối đa được phép và (q=60) | 
| (n=42) lặp lại ba lần |`finish`, số dư 0 mỗi lần | Sự cô lập trạng thái và tương tác xác định | 

## Vỏ cạnh 

Số dư bằng 0 được xử lý bằng cách tìm kiếm một cách tự nhiên. Vì`withdraw 1`với (n=0), câu trả lời là`rejected`, và mọi lần rút tiền sau đó cũng bị từ chối. Tìm kiếm nhị phân cuối cùng đạt đến (l=0), lần quét cuối cùng thực hiện thêm một lần nữa`withdraw 1`, Và`finish`là hợp lệ. Sự tương tác hoàn chỉnh chỉ sử dụng sáu lần thử, trong khi giao thức cho phép mười lần thử. 

Với (n=1), tìm kiếm nhị phân sẽ loại bỏ mọi lũy thừa được kiểm tra trên một. Lần quét cuối cùng bắt đầu ở số mũ 0, vì vậy`withdraw 1`thành công và để lại số không. Thuật toán sau đó kết thúc. Ranh giới quan trọng là số mũ 0 được đưa vào vòng lặp cuối cùng. Bắt đầu từ một sẽ bỏ lỡ đồng đô la duy nhất. 

Đối với lũy thừa chính xác như (n=2^5=32), truy vấn tìm kiếm nhị phân có thể rút thành công 32 và để lại 0. Các truy vấn sau đó đều bị từ chối. Lần quét cuối cùng vẫn an toàn vì việc rút tiền bị từ chối không có tác dụng gì. Điều này chứng tỏ tại sao lời giải không được giả định rằng một`accepted`phản ứng có nghĩa là số dư bây giờ bằng không. 

Đối với số dư tối đa (n=10^{18}), chúng ta có (2^{59<10^{18<2^{60}), do đó (q=60). Mọi truy vấn đều sử dụng số mũ dưới 60 và tìm kiếm nhị phân cần tối đa sáu lần thử. Ngay cả khi (l=59), lần quét cuối cùng chỉ cần thêm 60 lần thử nữa, tổng cộng tối đa là 66 hoặc 67 tùy thuộc vào đường dẫn tìm kiếm chính xác, dưới mức 70 cho phép. 

Trường hợp tinh vi nhất là khi các truy vấn được chấp nhận làm thay đổi số dư trong quá trình tìm kiếm nhị phân. Giả sử (n=100). Truy vấn về 8 có thể thành công, giảm số dư xuống 92. Truy vấn sau đó cho 32 cũng có thể thành công, giảm xuống còn 60, trong khi truy vấn về 64 sau đó bị từ chối. Các phản hồi không thể được hiểu là so sánh với 100 ban đầu. Điều còn hiệu lực là câu lệnh yếu hơn mà thuật toán cần: ranh giới bị bác bỏ cuối cùng chứng tỏ rằng số dư hiện tại nhỏ hơn lũy thừa tiếp theo ở trên (l). Lần quét giảm dần cuối cùng sẽ loại bỏ biểu diễn nhị phân còn lại mà không cần dựa vào số dư ban đầu không thay đổi.
