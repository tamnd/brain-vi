---
title: "CF 102219K - Giúp Cô Hỗ Trợ"
description: "Mỗi yêu cầu của khách hàng có thời gian xử lý là t. Nếu Nina bắt đầu từ thời điểm 0 thì yêu cầu đó phải được xử lý hoàn toàn trước thời gian 2t thì khách hàng mới được hài lòng."
date: "2026-08-17T23:06:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102219
codeforces_index: "K"
codeforces_contest_name: "2019 ICPC Malaysia National"
rating: 0
weight: 102219
solve_time_s: 177
verified: false
draft: false
---

[CF 102219K - Giúp đỡ Cô hỗ trợ](https://codeforces.com/problemset/problem/102219/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 57s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mỗi yêu cầu của khách hàng đều có thời gian xử lý`t`. Nếu Nina bắt đầu từ thời gian`0`, yêu cầu đó phải được xử lý hoàn toàn theo thời gian`2t`để khách hàng hài lòng. Nina có thể chọn thứ tự xử lý yêu cầu và cô ấy muốn tối đa hóa số lượng yêu cầu đáp ứng đúng thời hạn. 

Đầu vào chứa tối đa 20 trường hợp thử nghiệm độc lập. Đối với mỗi trường hợp,`n`có thể lớn như`10^5`, trong khi mỗi thời gian xử lý có thể lớn bằng`10^9`. Câu trả lời là số lượng yêu cầu tối đa có thể được hoàn thành trước thời hạn riêng của chúng. Từ`n`đạt tới`10^5`, việc liệt kê các tập hợp con hoặc hoán vị là không thể. Chúng ta cần thứ gì đó xung quanh`O(n log n)`hoặc tốt hơn cho mỗi trường hợp. Giới hạn thời gian một giây cũng làm cho phương pháp bậc hai không phù hợp với các trường hợp lớn nhất. 

Quan sát cấu trúc đầu tiên là một yêu cầu có thời gian xử lý`t`có thời hạn`2t`. Do đó cả thời gian xử lý và thời hạn của nó đều được xác định bởi cùng một giá trị. Nếu hai yêu cầu có thời gian xử lý`a <= b`, thì thời hạn của họ cũng thỏa mãn`2a <= 2b`. Do đó, việc sắp xếp các yêu cầu theo thời gian xử lý cũng sắp xếp thời hạn của chúng. 

Ví dụ, với đầu vào`1 / 1`, yêu cầu duy nhất luôn được đáp ứng vì nó kết thúc đúng lúc`1`, nhiều nhất là thời hạn của nó`2`. Việc thực hiện bất cẩn để kiểm tra xem`t <= 0`hoặc coi thời hạn là`t`sẽ từ chối nó một cách không chính xác. 

Coi như`1 / 3 / 1 1 1`. Câu trả lời đúng là`2`. Sau khi hoàn thành hai yêu cầu đầu tiên, thời gian hiện tại là`2`, đó chính xác là thời hạn của yêu cầu thứ hai. Yêu cầu thứ ba sẽ kết thúc vào thời điểm`3`, sau thời hạn của nó`2`. Một triển khai sử dụng`<`thay vì`<=`sẽ từ chối yêu cầu thứ hai một cách không chính xác và trả lại`1`. 

Một trường hợp thú vị hơn là`1 / 4 / 1 1 1 3`. Sau hai yêu cầu đầu tiên, thời gian tích lũy là`2`. Yêu cầu thứ ba không thể được hoàn thành trước thời hạn vì`2 + 1 > 2`, nên phải bỏ qua. Yêu cầu độ dài`3`sau đó có thể được hoàn thành vào thời gian`5`, chính xác trước thời hạn của nó`6`. Câu trả lời đúng là`3`. Một sai lầm phổ biến là dừng lại sau yêu cầu thất bại đầu tiên thay vì tiếp tục kiểm tra những thời hạn lớn hơn sau đó. 

Cuối cùng, các giá trị lớn yêu cầu thời gian xử lý tích lũy phải được lưu trữ ở dạng số nguyên đủ rộng. Với`10^5`yêu cầu mỗi lần lấy`10^9`, tổng có thể đạt tới`10^14`. Số nguyên Python tự động xử lý việc này, nhưng việc triển khai có chiều rộng cố định bằng ngôn ngữ khác sẽ cần số nguyên 64 bit. 

## Phương pháp tiếp cận 

Giải pháp cưỡng bức trực tiếp có thể xem xét mọi tập hợp con yêu cầu, sắp xếp tập hợp con đó theo thời gian xử lý và kiểm tra xem mọi yêu cầu đã chọn có kết thúc trước thời hạn hay không. Điều này đúng vì mọi lựa chọn có thể có của những khách hàng hài lòng đều được biểu thị bằng một tập hợp con và việc sắp xếp tập hợp con đã chọn theo thời hạn tăng dần sẽ đưa ra thứ tự khả thi chính xác. có`2^n`tập hợp con và việc kiểm tra một tập hợp con có thể mất`O(n)`thời gian, cho`Theta(n 2^n)`làm việc theo cách thực hiện đơn giản. Ngay cả số lượng tập hợp con cũng đã là không thể đối với`n = 10^5`. 

Cấu trúc hữu ích xuất phát từ thực tế là thời hạn chính xác gấp đôi thời gian xử lý. Sắp xếp tất cả các yêu cầu theo`t`. Giả sử các yêu cầu hiện được chọn có tổng thời gian xử lý`S`và yêu cầu tiếp theo có thời gian xử lý`t`. Nếu chúng ta thêm nó vào thì thời gian hoàn thành của nó là`S + t`. Hạn chót của nó là`2t`, do đó điều kiện để chấp nhận nó trở thành`S + t <= 2t`điều đó đơn giản hóa thành`S <= t`. 

Điều này thuận tiện một cách lạ thường. Sau khi sắp xếp, mọi yêu cầu trong tương lai đều có thời gian xử lý ít nhất bằng yêu cầu hiện tại. Nếu yêu cầu hiện tại không thể đáp ứng được thì việc giữ nguyên yêu cầu đó không thể giúp chúng tôi đạt được số lượng yêu cầu được thỏa mãn lớn hơn. Đây là yêu cầu lớn nhất được xem xét cho đến nay, vì vậy việc thay thế bất kỳ yêu cầu nào đã được chọn bằng yêu cầu đó sẽ chỉ làm tăng tổng thời gian xử lý. 

Giải pháp brute-force hoạt động vì nó xem xét rõ ràng mọi tập hợp con có thể, nhưng không thành công vì có nhiều tập hợp con theo cấp số nhân. Quan sát cho thấy các yêu cầu có thể được xử lý theo thứ tự tăng dần và một yêu cầu phù hợp chính xác khi thời gian tích lũy hiện tại tối đa bằng thời gian xử lý của chính nó cho phép chúng ta đưa ra quyết định tham lam cho mọi yêu cầu một cách độc lập. 

Quy tắc tham lam rất đơn giản: sắp xếp các giá trị, quét chúng từ nhỏ nhất đến lớn nhất và thêm yêu cầu chính xác khi thời gian tích lũy hiện tại cao nhất là giá trị của yêu cầu đó. Nếu không thì bỏ qua nó và tiếp tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n 2^n)`|`O(n)`| Quá chậm | 
| Tối ưu |`O(n log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả thời gian xử lý yêu cầu theo thứ tự không giảm. Điều này cũng sắp xếp thời hạn của họ, bởi vì một yêu cầu có giá trị`t`có thời hạn`2t`. Việc xử lý các yêu cầu đã chọn theo thứ tự này là đủ để kiểm tra tính khả thi. 
2. Đặt`total = 0`Và`answer = 0`. Đây`total`thể hiện lượng thời gian đã dành cho các yêu cầu mà chúng tôi quyết định đáp ứng. 
3. Đối với mỗi yêu cầu được sắp xếp có thời gian xử lý`t`, kiểm tra xem`total <= t`. Đây chính xác là điều kiện về thời hạn vì việc chấp nhận yêu cầu sẽ khiến thời gian hoàn thành của nó`total + t`, trong khi thời hạn của nó là`2t`. 
4. Nếu`total <= t`, chấp nhận yêu cầu. Tăng`total`qua`t`và tăng`answer`bởi một. Yêu cầu hiện được đảm bảo hoàn thành không muộn hơn`2t`. 
5. Nếu`total > t`, bỏ qua yêu cầu. Vì tất cả các yêu cầu được xem xét trước đó đều không lớn hơn`t`, yêu cầu này là yêu cầu lớn nhất cho đến nay. Không thể thay thế yêu cầu đã chọn để nhận được cùng số lượng yêu cầu với tổng thời gian xử lý nhỏ hơn. 
6. Sau khi xử lý mọi yêu cầu, hãy in`answer`cho trường hợp thử nghiệm hiện tại. 

### Tại sao nó hoạt động 

Duy trì bất biến sau khi xử lý bất kỳ tiền tố nào của mảng được sắp xếp,`answer`là số lượng yêu cầu có thể thỏa mãn tối đa từ tiền tố đó và trong số tất cả các lựa chọn khả thi có kích thước đó,`total`càng nhỏ càng tốt. 

Khi yêu cầu hiện tại`t`thỏa mãn`total <= t`, thêm nó mang lại`total + t <= 2t`, vì vậy chúng ta có một lựa chọn khả thi chứa thêm một yêu cầu nữa. Vì tiền tố trước có thể chứa tối đa`answer`theo yêu cầu, lựa chọn mới này có lượng số tối đa có thể và việc chọn tổng số nhỏ nhất có thể trước đó sẽ giữ cho tổng số mới ở mức tối thiểu. 

Khi`total > t`, việc thêm yêu cầu hiện tại sẽ yêu cầu nhiều hơn`2t`thời gian. Bất kỳ lựa chọn nào có thêm một yêu cầu sẽ phải thay thế một số yêu cầu đã chọn trước đó hoặc bao gồm yêu cầu hiện tại. Vì yêu cầu hiện tại ít nhất phải lớn bằng mọi yêu cầu trước đó nên việc thay thế yêu cầu đã chọn trước đó bằng yêu cầu đó không thể tạo ra tổng số nhỏ hơn. Người bất biến nói`total`đã là tổng số tối thiểu cho số lượng yêu cầu được chọn hiện tại, do đó không có lựa chọn khả thi nào với một yêu cầu bổ sung. Bỏ qua`t`do đó là tối ưu. 

Sắp xếp cũng đủ cho thứ tự lập kế hoạch. Vì thời hạn tỷ lệ thuận với thời gian xử lý nên thời gian xử lý càng nhỏ thì thời hạn càng nhỏ. Lên lịch các yêu cầu được chọn ngày càng tăng`t`là thứ tự có thời hạn sớm nhất và việc đáp ứng mọi thời hạn tiền tố là đủ để làm cho toàn bộ tập hợp đã chọn trở nên khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    m = int(input())
    out = []

    for case in range(1, m + 1):
        n = int(input())
        a = list(map(int, input().split()))

        a.sort()

        total = 0
        answer = 0

        for t in a:
            if total <= t:
                total += t
                answer += 1

        out.append(f"Case #{case}: {answer}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Đầu tiên, chương trình sẽ đọc số lượng ca kiểm thử và xử lý từng trường hợp một cách độc lập. Mảng chứa chính xác thời gian xử lý, do đó không cần mảng thời hạn riêng biệt. Thời hạn của một giá trị`t`luôn có thể được suy ra là`2t`. 

Sau khi sắp xếp,`total`chứa thời gian hoàn thành ngay trước khi yêu cầu hiện tại bắt đầu. biểu thức`total <= t`có thể trông khác với điều kiện thời hạn ban đầu, nhưng chúng giống hệt nhau về mặt đại số:`total + t <= 2t`tương đương với`total <= t`. 

Việc sử dụng điều kiện đơn giản sẽ tránh được phép nhân không cần thiết và làm cho quy tắc tham lam trở nên rõ ràng hơn. 

Trường hợp bình đẳng phải được chấp nhận. Nếu như`total == t`, yêu cầu kết thúc vào đúng lúc`2t`, vẫn còn đúng giờ. Đây là lý do tại sao điều kiện là`<=`, không`<`. 

Kiểu số nguyên của Python có thể biểu thị thời gian tích lũy mà không bị tràn. Mặc dù mỗi giá trị riêng lẻ tối đa là`10^9`, tổng số có thể vào khoảng`10^14`. 

Sản lượng được tích lũy trong`out`và viết một lần ở cuối. Điều này tránh các hoạt động đầu ra lặp đi lặp lại khi có nhiều trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

Đối với mẫu được cung cấp,```
1
5
15 2 1 5 3
```thời gian xử lý được sắp xếp là`1, 2, 3, 5, 15`. 

| Lời yêu cầu`t`|`total`trước | Tình trạng`total <= t`| Hành động |`total`sau |`answer`| 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | đúng | Chấp nhận | 1 | 1 | 
| 2 | 1 | đúng | Chấp nhận | 3 | 2 | 
| 3 | 3 | đúng | Chấp nhận | 6 | 3 | 
| 5 | 6 | sai | Bỏ qua | 6 | 3 | 
| 15 | 6 | đúng | Chấp nhận | 21 | 4 | 

Yêu cầu độ dài`5`sẽ kết thúc vào lúc đó`11`, nhưng thời hạn của nó chỉ là`10`, vì vậy nó không thể được đưa vào sau ba yêu cầu đầu tiên. Yêu cầu độ dài`15`có thời hạn`30`, vì vậy nó có thể được thêm vào sau yêu cầu bị bỏ qua. Câu trả lời cuối cùng là`4`. 

Đối với ví dụ thứ hai,```
1
4
1 2 4 8
```thứ tự sắp xếp đã có rồi`1, 2, 4, 8`. 

| Lời yêu cầu`t`|`total`trước | Tình trạng`total <= t`| Hành động |`total`sau |`answer`| 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | đúng | Chấp nhận | 1 | 1 | 
| 2 | 1 | đúng | Chấp nhận | 3 | 2 | 
| 4 | 3 | đúng | Chấp nhận | 7 | 3 | 
| 8 | 7 | đúng | Chấp nhận | 15 | 4 | 

Mọi yêu cầu đều được chấp nhận. Thời gian hoàn thành của họ là`1, 3, 7, 15`, trong khi thời hạn của họ là`2, 4, 8, 16`. Câu trả lời là`4`. 

Những ví dụ này thể hiện tính bất biến trung tâm: sau mỗi yêu cầu được chấp nhận, thời gian tích lũy là tổng nhỏ nhất có thể cho số lượng yêu cầu tối đa được chọn cho đến nay. Ví dụ đầu tiên cũng giải thích tại sao một yêu cầu không thành công phải được bỏ qua thay vì kết thúc quá trình quét. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n)`| Việc sắp xếp chiếm ưu thế trong quá trình quét tuyến tính | 
| Không gian |`O(n)`| Mảng thời gian yêu cầu yêu cầu`O(n)`bộ nhớ | 

Vì`n = 10^5`, việc sắp xếp thực hiện một số phép so sánh có thể quản lý được và lần quét tiếp theo là tuyến tính. Việc sử dụng bộ nhớ cũng thoải mái trong vòng 256 MB. Với tối đa 20 trường hợp, giới hạn tương tự sẽ được áp dụng cho từng trường hợp, với tổng công việc tỷ lệ thuận với tổng số yêu cầu đầu vào. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    m = int(input())
    out = []

    for case in range(1, m + 1):
        n = int(input())
        a = list(map(int, input().split()))

        a.sort()

        total = 0
        answer = 0

        for t in a:
            if total <= t:
                total += t
                answer += 1

        out.append(f"Case #{case}: {answer}")

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

assert run("""\
1
5
15 2 1 5 3
""") == "Case #1: 4", "sample 1"

assert run("""\
1
4
1 2 4 8
""") == "Case #1: 4", "all requests fit"

assert run("""\
1
1
1000000000
""") == "Case #1: 1", "minimum size and maximum value"

assert run("""\
1
3
1 1 1
""") == "Case #1: 2", "equality boundary"

assert run("""\
1
3
1 1 2
""") == "Case #1: 3", "exact deadline equality"

assert run("""\
1
4
1 1 1 3
""") == "Case #1: 3", "skip one request and continue"

assert run("1\n100000\n" + " ".join(["1"] * 100000) + "\n") == \
       "Case #1: 2", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 1000000000`|`Case #1: 1`| tối thiểu`n`và giá trị thời gian xử lý tối đa | 
|`1 / 3 / 1 1 1`|`Case #1: 2`| Ranh giới thời hạn chính xác và lỗi từng cái một | 
|`1 / 3 / 1 1 2`|`Case #1: 3`| Bình đẳng phải được chấp nhận | 
|`1 / 4 / 1 1 1 3`|`Case #1: 3`| Yêu cầu không thành công phải được bỏ qua trong khi các yêu cầu sau đó vẫn được xem xét | 
|`1 / 100000 / 1 ... 1`|`Case #1: 2`| Kích thước đầu vào tối đa và các giá trị hoàn toàn bằng nhau | 

## Vỏ cạnh 

Trường hợp yêu cầu duy nhất rất đơn giản nhưng hữu ích để kiểm tra quá trình khởi tạo. Đối với đầu vào```
1
1
1000000000
```

`total`bắt đầu lúc`0`, Vì thế`0 <= 1000000000`là đúng. Yêu cầu được chấp nhận, đưa ra`Case #1: 1`. Thuật toán không bao giờ cần trường hợp đặc biệt cho`n = 1`. 

Các giá trị bằng nhau thể hiện điều kiện biên. Vì```
1
3
1 1 1
```yêu cầu đầu tiên thay đổi`total`từ`0`ĐẾN`1`và cái thứ hai thay đổi nó từ`1`ĐẾN`2`. Đối với yêu cầu thứ ba,`total = 2`trong khi`t = 1`, nên nó bị bỏ qua. Câu trả lời là`2`. Yêu cầu thứ hai kết thúc đúng lúc`2`, thời hạn của nó, vì vậy sử dụng`<`thay vì`<=`sẽ tạo ra kết quả sai. 

Trường hợp yêu cầu bị bỏ qua nhưng yêu cầu sau đó được chấp nhận là```
1
4
1 1 1 3
```Sau hai yêu cầu đầu tiên,`total = 2`. Tiếp theo`1`thất bại vì`2 > 1`, nên nó bị bỏ qua. Yêu cầu cuối cùng có`t = 3`, Và`2 <= 3`, nên nó được chấp nhận. Câu trả lời cuối cùng là`3`. Điều này cho thấy lý do tại sao quá trình quét phải tiếp tục sau khi yêu cầu không thành công. 

Ranh giới bình đẳng cũng có thể xảy ra sau một số yêu cầu. Vì```
1
3
1 1 2
```hai yêu cầu đầu tiên tạo ra`total = 2`. Vì`t = 2`, điều kiện chính xác là`2 <= 2`, vậy là yêu cầu được chấp nhận. Thời gian hoàn thành của nó là`4`, chính xác là thời hạn của nó`2 * 2`. Câu trả lời là`3`. 

Cuối cùng, trường hợp kích thước tối đa```
1
100000
1 1 1 ... 1
```chứa 100.000 yêu cầu giống hệt nhau. Hai điều đầu tiên có thể được thỏa mãn vì thời gian hoàn thành của chúng là`1`Và`2`, cả vào hoặc trước thời hạn`2`. Mọi yêu cầu tiếp theo sẽ kết thúc sau`2`, vì vậy tất cả các yêu cầu còn lại đều bị bỏ qua. Câu trả lời là`2`. Thuật toán vẫn chỉ thực hiện một lần sắp xếp và một lần quét tuyến tính, do đó đầu vào lớn không làm thay đổi hành vi tiệm cận.
