---
title: "CF 102437F - \u0411\u044b\u0441\u0442\u0440\u044b\u0439 \u043f\u0435\u0440\u0435\u0432\u043e\u0434"
description: "Đây là một vấn đề tương tác. Có một số dư không âm n chưa biết, với n <= 10^18. Chúng ta không thể đọc n trực tiếp. Thay vào đó, chúng tôi có thể yêu cầu thiết bị đầu cuối rút một số tiền dương x."
date: "2026-08-09T12:57:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102437
codeforces_index: "F"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0427\u0435\u0442\u0432\u0451\u0440\u0442\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102437
solve_time_s: 998
verified: false
draft: false
---

[CF 102437F - \u0411\u044b\u0441\u0442\u0440\u044b\u0439 \u043f\u0435\u0440\u0435\u0432\u043e\u0434](https://codeforces.com/problemset/problem/102437/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 16 phút 38 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Đây là một vấn đề tương tác. Có một số dư không âm chưa biết`n`, với`n <= 10^18`. Chúng tôi không thể đọc`n`trực tiếp. Thay vào đó, chúng tôi có thể yêu cầu thiết bị đầu cuối rút một số tiền dương`x`. Nếu số dư hiện tại ít nhất là`x`, thiết bị đầu cuối trả lời`accepted`và trừ`x`từ số dư. Ngược lại nó trả lời`rejected`và số dư không đổi. 

Mục tiêu là làm cho số dư chính xác bằng 0 và sau đó in`finish`. Khó khăn là giới hạn truy vấn. Nếu như`q`là số nguyên nhỏ nhất thỏa mãn`n <= 2^q`, thì thiết bị đầu cuối cho phép nhiều nhất`q + 10`những nỗ lực rút tiền. Từ`10^18 < 2^60`, một giải pháp kiểm tra một cách mù quáng tất cả lũy thừa của hai từ`2^59`xuống tới`1`sử dụng 60 lần thử, quá nhiều đối với các giá trị nhỏ của`n`. 

Không có đầu vào thông thường có chứa`n`. Luồng đầu vào bao gồm các câu trả lời từ trình tương tác sau khi chương trình của chúng tôi in lệnh. Tương tự, đầu ra là một chuỗi các lệnh tương tác như`withdraw x`và cuối cùng`finish`. Mọi lệnh phải được xóa ngay lập tức để người tương tác có thể trả lời. 

Sự ràng buộc`10^18`rất hữu ích vì nó chỉ cung cấp 60 vị trí nhị phân có liên quan. Do đó, việc phân tách nhị phân đơn giản sẽ mất tối đa 60 lần rút tiền thành công hoặc bị từ chối. Thách thức không phải là độ phức tạp tính toán theo nghĩa thông thường mà là giảm số lượng tương tác xuống mức tối đa.`q + 10`, Ở đâu`q`phụ thuộc vào cái chưa biết`n`. 

Việc thực hiện bất cẩn có thể thất bại với số dư nhỏ. Ví dụ, nếu số dư là`0`, sự tương tác đúng chỉ đơn giản là`finish`, bởi vì`withdraw 1`sẽ bị từ chối và lãng phí một trong những nỗ lực có sẵn. Để cân bằng`1`,`withdraw 1`phải được chấp nhận và sau đó`finish`là đúng. Một chiến lược luôn thực hiện tất cả 60 truy vấn lũy thừa hai sẽ lãng phí nhiều hơn mức cho phép`q + 10 = 10`những nỗ lực. 

Một trường hợp tinh tế khác là một truy vấn được chấp nhận sẽ thay đổi số dư ẩn. Giả sử số dư là`10`và chúng tôi yêu cầu`8`. Câu trả lời là`accepted`, nhưng số dư bây giờ là`2`. Mọi lý luận sau này đều phải sử dụng số dư mới chứ không phải số dư ban đầu. Phương pháp tối ưu được thiết kế có chủ ý để chịu đựng giá trị thay đổi này. 

## Phương pháp tiếp cận 

Giải pháp cơ bản là xử lý trực tiếp biểu diễn nhị phân. Bắt đầu với`2^59`, hỏi xem số tiền đó có thể rút được không. Nếu có thể, hãy trừ nó ra khỏi số dư. Sau đó tiếp tục với`2^58`,`2^57`, v.v. cho đến`1`. Ở mỗi bước, nếu bit nhị phân tương ứng được đặt thì nó sẽ bị xóa. Sau khi tất cả 60 lũy thừa đã được xem xét, số dư bằng không. 

Điều này có tác dụng vì mọi số lên tới`10^18`nhỏ hơn`2^60`, do đó biểu diễn nhị phân của nó chỉ chứa các lũy thừa từ`2^59`bởi vì`2^0`. Vấn đề là số lượng tương tác. Đối với một số dư nhỏ như`n = 1`, phương pháp này vẫn thực hiện 60 lần thử, trong khi thiết bị đầu cuối chỉ cho phép 10 lần. Hướng dẫn chính thức của cuộc thi mô tả chính xác đây là đường cơ sở và sau đó giảm số lượng truy vấn bằng tìm kiếm nhị phân. 

Quan sát quan trọng là chúng ta thực sự không cần biết chính xác lũy thừa lớn nhất của hai hiện có thể rút ra được. Chúng ta chỉ cần một lũy thừa đủ lớn để giới hạn số dư còn lại, bởi vì một khi đã có giới hạn như vậy thì việc xử lý tất cả các lũy thừa nhỏ hơn là đủ. 

Chúng ta có thể tìm thấy số mũ bắt đầu hữu ích bằng cách tìm kiếm nhị phân trên 60 lũy thừa có thể. Đối với một truy vấn`2^m`, MỘT`accepted`câu trả lời chứng minh rằng số dư hiện tại ít nhất là`2^m`, Vì thế`m`là số mũ hợp lệ và chúng tôi di chuyển ranh giới bên trái sang`m`. MỘT`rejected`câu trả lời chứng minh rằng số dư hiện tại thấp hơn`2^m`, Vì thế`m`không thể được sử dụng và chúng tôi di chuyển ranh giới bên phải tới`m`. 

Điều phức tạp là các truy vấn được chấp nhận sẽ trừ tiền, do đó số dư sẽ thay đổi trong quá trình tìm kiếm nhị phân này. May mắn thay, điều này không phá vỡ phương pháp. Mỗi số mũ từng được ghi là được chấp nhận đều tương ứng với một lũy thừa thực sự hiện diện trong số dư tại thời điểm đó. Vì số dư chỉ có thể giảm đi nên sức mạnh đó cũng không lớn hơn số dư ban đầu. Như vậy trận chung kết`l`không bao giờ có thể vượt quá logarit nhị phân của bản gốc`n`. 

Khi tìm kiếm nhị phân kết thúc với`r = l + 1`, ranh giới bị từ chối cuối cùng cho chúng ta biết rằng số dư hiện tại nhỏ hơn`2^r = 2^(l+1)`. Bây giờ chúng ta có thể đơn giản thử`2^l`,`2^(l-1)`, ...,`1`. Đây là tất cả các lũy thừa có thể xảy ra trong biểu diễn nhị phân của một số nhỏ hơn`2^(l+1)`, vì vậy chúng đủ để tiêu hao hoàn toàn tài khoản. 

Có nhiều nhất 6 truy vấn tìm kiếm nhị phân vì chỉ có 60 số mũ có thể và`2^6 = 64 > 60`. Việc dọn dẹp sử dụng nhiều nhất`l + 1`truy vấn và`l <= q`, Ở đâu`q`là logarit của số dư ban đầu. Như vậy tổng số nhiều nhất là`q + 7`, thoải mái dưới mức cho phép`q + 10`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Tương tác O(60) | O(1) | Quá nhiều truy vấn cho nhỏ`n`| 
| Tối ưu | Tương tác O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đặt`l = 0`Và`r = 60`. Khoảng đại diện cho tất cả số mũ nhị phân có thể có, từ`0`bởi vì`59`, Và`2^60`đã được biết là vượt quá mọi số dư ban đầu có thể có. 
2. Trong khi`r - l > 1`, chọn`m = (l + r) // 2`và yêu cầu rút tiền`2^m`. Đây là bước tìm kiếm nhị phân. Một phản hồi được chấp nhận chứng tỏ rằng số mũ`m`có thể đạt được, vì vậy hãy thiết lập`l = m`. Một phản hồi bị từ chối chứng tỏ rằng`2^m`quá lớn so với số dư hiện tại, vì vậy hãy đặt`r = m`. 
3. Nếu người tương tác trả lời`fail`, chấm dứt ngay lập tức. Tiếp tục sau`fail`không thể đưa ra một bản án có giá trị. 
4. Sau khi tìm kiếm nhị phân, xử lý số mũ từ`l`xuống tới`0`. Đối với mỗi số mũ`i`, yêu cầu rút tiền`2^i`. Nếu câu trả lời là`accepted`, sức mạnh đó sẽ bị loại bỏ khỏi số dư còn lại. Nếu nó là`rejected`, bit tương ứng với lũy thừa đó không có nên không cần thay đổi gì. 
5. In`finish`sau tất cả quyền lực thông qua`2^0`đã được xem xét. Tại thời điểm này số dư phải bằng không. 

### Tại sao nó hoạt động 

Khi kết thúc tìm kiếm nhị phân,`r = l + 1`và ranh giới bên phải tương ứng với một lũy thừa bị bác bỏ tại một điểm nào đó. Số dư chỉ có thể giảm sau lần từ chối đó nên số dư hiện tại chắc chắn nhỏ hơn`2^r = 2^(l+1)`. Mọi số nguyên không âm nhỏ hơn`2^(l+1)`chỉ có thể được đại diện bằng cách sử dụng quyền hạn`2^l, 2^(l-1), ..., 1`. Giai đoạn dọn dẹp sẽ kiểm tra chính xác những nguồn năng lượng đó và rút mọi nguồn năng lượng hiện có, do đó số dư còn lại sẽ bằng không. 

Giới hạn truy vấn tuân theo bất biến thứ hai. Bất cứ khi nào bộ tìm kiếm nhị phân`l = m`, sự rút lui của`2^m`đã được chấp nhận, có nghĩa là số dư ban đầu ít nhất là`2^m`. Kể từ đây`m <= q`. Do đó`l <= q`. Tìm kiếm nhị phân cần tối đa 6 truy vấn và việc dọn dẹp cần nhiều nhất`l + 1 <= q + 1`tổng số truy vấn tối đa là`q + 7`, bên dưới`q + 10`. 

## Giải pháp Python 

Vấn đề là ở tính tương tác, vì vậy chương trình này nhằm mục đích giao tiếp với người tương tác chính thức thay vì đọc trường hợp thử nghiệm thông thường.```python
import sys
input = sys.stdin.readline

def ask(x):
    print(f"withdraw {x}", flush=True)
    response = input().strip()

    if response == "accepted":
        return True
    if response == "rejected":
        return False

    # The only other possible response is "fail".
    sys.exit(0)

def main():
    l = 0
    r = 60

    # Find an exponent l such that the remaining balance is below 2^(l+1).
    while r - l > 1:
        m = (l + r) // 2

        if ask(1 << m):
            l = m
        else:
            r = m

    # The remaining balance is smaller than 2^(l+1).
    # Its binary representation therefore uses only these powers.
    for i in range(l, -1, -1):
        ask(1 << i)

    print("finish", flush=True)

if __name__ == "__main__":
    main()
```các`ask`chức năng thực hiện chính xác một lần rút tiền tương tác. sử dụng`flush=True`là cần thiết vì người tương tác không thể phản hồi cho đến khi nhận được lệnh. Hàm trả về một giá trị Boolean cho hai phản hồi thông thường và kết thúc ngay lập tức đối với`fail`. 

Khoảng tìm kiếm nhị phân sử dụng số mũ thay vì số tiền.`r = 60`là an toàn vì mọi số dư ban đầu có thể có đều nhỏ hơn`2^60`. Điều kiện vòng lặp`r - l > 1`để lại các ranh giới liên tiếp, vì vậy sau vòng lặp`r = l + 1`. 

biểu hiện`1 << m`tính toán`2^m`trực tiếp. Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn. Giá trị lớn nhất được tạo ra là`2^59`, cũng an toàn trong số tiền rút được phép là`10^18`. 

Vòng lặp dọn dẹp có chủ ý đi xuống. Nếu chúng ta xử lý lũy thừa nhỏ hơn trước, thì lũy thừa lớn hơn được chấp nhận sau này vẫn có thể để lại số dư chứa các bit nhỏ hơn, khiến cho việc suy luận trở nên ít trực tiếp hơn. Quyền hạn giảm dần phản ánh sự phân rã nhị phân thông thường và đảm bảo rằng mọi bit còn lại được xử lý chính xác một lần. 

Không có nỗ lực bỏ qua truy vấn dọn dẹp bị từ chối. Truy vấn bị từ chối vẫn được tính là tương tác, nhưng điều này là cần thiết vì chúng ta cần xác định xem bit nhị phân đó có hiện diện hay không. Số lượng truy vấn như vậy đã được bao phủ bởi`q + 7`ràng buộc. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Sự tương tác thể hiện trong mẫu phù hợp với sự cân bằng ban đầu của`1`. 

| Bước | Hành động | Phản hồi | Số dư còn lại |`l`|`r`| 
| --- | --- | --- | --- | --- | --- | 
| 1 |`withdraw 42`|`rejected`| 1 | 0 | 42 | 
| 2 |`withdraw 1`|`accepted`| 0 | 0 | 42 | 
| 3 |`withdraw 1`|`rejected`| 0 | 0 | 42 | 
| 4 |`finish`| được thẩm phán chấp nhận | 0 | 0 | 42 | 

Bản thân mẫu không được tạo ra bằng thuật toán tối ưu, do đó, chuỗi lệnh của nó không được mong đợi khớp với chương trình trên. Nó thể hiện giao thức: việc rút tiền bị từ chối không thay đổi gì, việc rút tiền được chấp nhận sẽ trừ đi số tiền được yêu cầu và`finish`chỉ thành công khi số dư bằng không. 

Vì`n = 1`, chương trình tối ưu trước tiên thực hiện tìm kiếm nhị phân của nó. Tất cả các quyền hạn được kiểm tra lớn hơn`1`bị từ chối, rời đi`l = 0`. Việc dọn dẹp sau đó yêu cầu`1`, nhận`accepted`, và kết thúc. Tổng số lần rút tiền tối đa là 7. 

### Mẫu 2 

Mẫu này phù hợp với số dư ban đầu của`0`. 

| Bước | Hành động | Phản hồi | Số dư còn lại |`l`|`r`| 
| --- | --- | --- | --- | --- | --- | 
| 1 |`withdraw 1`|`rejected`| 0 | 0 | 1 | 
| 2 |`finish`| được thẩm phán chấp nhận | 0 | 0 | 1 | 

Một lần nữa, mẫu đang minh họa giao thức chứ không phải là trình tự truy vấn tối ưu chính xác. Trường hợp quan trọng là việc rút tiền bị từ chối không chứng minh được số dư là âm. Điều đó chỉ có nghĩa là số tiền được yêu cầu lớn hơn số dư hiện tại, do đó số dư bằng 0 là có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Tương tác O(log n) | Tối đa 6 truy vấn tìm kiếm nhị phân và nhiều nhất`q + 1`truy vấn dọn dẹp | 
| Không gian | O(1) | Chỉ một số lượng biến số nguyên không đổi được lưu trữ | 

Từ`n <= 10^18 < 2^60`, giai đoạn tìm kiếm nhị phân luôn cần tối đa 6 tương tác. Việc dọn dẹp cần nhiều nhất`q + 1`, Ở đâu`q`thỏa mãn`n <= 2^q`. Như vậy tổng số nhiều nhất là`q + 7`, để lại ba truy vấn về an toàn theo yêu cầu`q + 10`giới hạn. Việc sử dụng bộ nhớ là không đổi. 

## Trường hợp thử nghiệm 

Bởi vì đây là một vấn đề tương tác, thông thường`run(input_string)`các bài kiểm tra không thể cung cấp số dư ẩn thông qua stdin. Thay vào đó, một khai thác thử nghiệm ngoại tuyến hữu ích sẽ mô phỏng trình tương tác. Đoạn mã sau sử dụng cùng một thuật toán đối với thiết bị đầu cuối giả, kiểm tra xem số dư cuối cùng có bằng 0 không và xác minh giới hạn truy vấn.```python
import io
import sys

def run_simulation(initial_balance):
    balance = initial_balance
    attempts = 0
    commands = []

    def ask(x):
        nonlocal balance, attempts
        attempts += 1
        commands.append(f"withdraw {x}")

        if balance >= x:
            balance -= x
            return True
        return False

    l = 0
    r = 60

    while r - l > 1:
        m = (l + r) // 2

        if ask(1 << m):
            l = m
        else:
            r = m

    for i in range(l, -1, -1):
        ask(1 << i)

    commands.append("finish")

    q = 0
    while (1 << q) < initial_balance:
        q += 1

    assert balance == 0
    assert attempts <= q + 10

    return "\n".join(commands)

# Provided sample 1 corresponds to an initial balance of 1.
sample1 = run_simulation(1)
assert sample1.endswith("finish"), "sample 1"

# Provided sample 2 corresponds to an initial balance of 0.
sample2 = run_simulation(0)
assert sample2.endswith("finish"), "sample 2"

# Minimum-size balance.
assert run_simulation(0).endswith("finish"), "zero balance"

# Small boundary values around powers of two.
for n in [1, 2, 3, 4, 7, 8, 9, 15, 16, 17]:
    run_simulation(n)

# A large value close to 2^60.
assert run_simulation(10**18).endswith("finish"), "maximum allowed balance"

# Exact powers of two exercise the boundary between q values.
for n in [2**10, 2**20, 2**30, 2**40, 2**50, 2**59]:
    run_simulation(n)

# Values immediately below and above powers of two catch off-by-one errors.
for n in [2**10 - 1, 2**10, 2**10 + 1]:
    run_simulation(n)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`|`finish`| Tài khoản trống và số dư nhỏ nhất có thể | 
|`1`|`finish`sau khi rút tiền thành công | Số dư dương nhỏ nhất | 
|`2^k - 1`|`finish`| Giới hạn dưới của khoảng nhị phân | 
|`2^k`|`finish`| Ranh giới lũy thừa chính xác của hai | 
|`2^k + 1`|`finish`| Phía trên ranh giới | 
|`10^18`|`finish`| Số dư tối đa được phép | 
|`2^59`|`finish`| Công suất nhị phân lớn nhất có liên quan | 

Trình mô phỏng cố tình không so sánh trình tự lệnh chính xác với bản ghi dự kiến ​​cố định. Các giải pháp tương tác có thể thực hiện các truy vấn hợp lệ khác nhau một cách hợp pháp và các điều kiện chính xác là số dư cuối cùng bằng 0 và giới hạn truy vấn được tôn trọng. 

## Vỏ cạnh 

### Số dư bằng không 

cho`n = 0`, tình huống đầu vào chính xác được thể hiện bằng một trình tương tác từ chối mọi lần rút tiền tích cực. Tìm kiếm nhị phân cuối cùng rời đi`l = 0`và việc dọn dẹp yêu cầu`1`, bị từ chối. Số dư vẫn bằng 0, vì vậy`finish`là đúng. Thuật toán không bao giờ giả định rằng một truy vấn được chấp nhận phải xảy ra. 

### Số dư bằng một 

cho`n = 1`, mọi lũy thừa lớn hơn một đều bị bác bỏ trong quá trình tìm kiếm nhị phân. Kết quả`l`bằng 0, do đó quá trình dọn dẹp yêu cầu`1`. Truy vấn đó được chấp nhận và để lại số không. Trường hợp này đặc biệt hữu ích vì`q = 0`, chỉ thực hiện số lần rút tiền tối đa được phép`10`. 

### Sức mạnh chính xác của hai 

Giả sử`n = 16`. Trong quá trình tìm kiếm nhị phân, truy vấn cho`16`có thể được chấp nhận, ngay lập tức giảm số dư về 0. Số mũ`l = 4`ghi lại rằng`2^4`đã có sẵn. Truy vấn dọn dẹp sau này cho`16`,`8`,`4`,`2`, Và`1`đều vô hại vì số dư đã bằng 0 và tất cả đều bị từ chối. trận chung kết`finish`là hợp lệ. Điều này chứng tỏ tại sao một truy vấn tìm kiếm nhị phân được chấp nhận thay đổi số dư không làm mất hiệu lực thuật toán. 

### Ngay dưới lũy thừa hai 

Giả sử`n = 15`. Một truy vấn cho`16`bị bác bỏ, do đó ranh giới bên phải trở thành số mũ tương ứng với`16`. Cuối cùng, tìm kiếm nhị phân sẽ thiết lập số mũ thấp hơn có lũy thừa tiếp theo lớn hơn số dư hiện tại. Kiểm tra dọn dẹp`8`,`4`,`2`, Và`1`và tất cả bốn bit đều được chấp nhận. Tổng của họ là`15`, do đó số dư đạt đến số không. 

### Ngay trên lũy thừa hai 

Giả sử`n = 17`. Một truy vấn cho`16`có thể được chấp nhận, để lại số dư`1`. Tìm kiếm nhị phân ghi lại số mũ`4`và việc dọn dẹp sau đó sẽ kiểm tra nguồn điện từ`16`. Vì số dư còn lại chỉ`1`, quyền lực lớn hơn bị từ chối và`1`được chấp nhận. Số dư cuối cùng bằng không. Trường hợp này chứng minh tại sao việc dọn dẹp phải tiếp tục cho đến hết`2^0`, ngay cả sau khi một quyền lực lớn đã bị rút đi. 

### Số dư tối đa 

cho`n = 10^18`, chúng tôi có`q = 59`bởi vì`2^59 <= 10^18 < 2^60`. Tìm kiếm nhị phân sử dụng tối đa 6 truy vấn và`l`nhiều nhất là 59, do đó quá trình dọn dẹp sử dụng tối đa 60 truy vấn nữa. Tổng số tối đa là 66, trong khi thiết bị đầu cuối cho phép`q + 10 = 69`những nỗ lực. Do đó, thuật toán vẫn nằm trong giới hạn ngay cả ở số dư lớn nhất có thể.
