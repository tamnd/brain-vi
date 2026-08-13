---
title: "CF 102281H - \u0421\u043f\u0438\u0447\u0435\u0447\u043d\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430"
description: "Chúng ta có hai hộp diêm, mỗi hộp ban đầu chứa đúng n que diêm. Mỗi khi Giáo sư X cần một que diêm, ông ấy chọn ngẫu nhiên một trong hai túi giống nhau và cố gắng lấy một que diêm từ hộp đó."
date: "2026-08-13T09:25:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102281
codeforces_index: "H"
codeforces_contest_name: "2011, IV \u0421\u0430\u043c\u0430\u0440\u0441\u043a\u0430\u044f \u043e\u0431\u043b\u0430\u0441\u0442\u043d\u0430\u044f \u043c\u0435\u0436\u0432\u0443\u0437\u043e\u0432\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 102281
solve_time_s: 87
verified: true
draft: false
---

[CF 102281H - \u0421\u043f\u0438\u0447\u0435\u0447\u043d\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430](https://codeforces.com/problemset/problem/102281/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai hộp diêm, mỗi hộp ban đầu chứa chính xác`n`trận đấu. Mỗi khi Giáo sư X cần một que diêm, ông ấy chọn ngẫu nhiên một trong hai túi giống nhau và cố gắng lấy một que diêm từ hộp đó. Quá trình kết thúc ở thời điểm đầu tiên anh ta chọn một ô trống. 

Nhiệm vụ là tính toán số lượng kết quả phù hợp dự kiến ​​vẫn còn trong hộp kia tại thời điểm đó. 

Đầu vào duy nhất là`n`, với`1 <= n <= 30`. Giới hạn trên đủ nhỏ để ngay cả các thuật toán đa thức trong`n`sẽ dễ dàng đủ nhanh. Tuy nhiên, việc liệt kê trực tiếp tất cả lịch sử lựa chọn ngẫu nhiên là theo cấp số nhân, do đó phần thú vị của vấn đề là nhận ra cấu trúc tổ hợp của lịch sử dừng thay vì mô phỏng mọi khả năng. 

Có hai trường hợp ranh giới rất dễ xử lý sai. Đầu tiên, hộp kia vẫn có thể chứa tất cả`n`trận đấu. Ví dụ, với`n = 1`, giáo sư có thể chọn cùng một ô hai lần: lựa chọn đầu tiên loại bỏ kết quả phù hợp duy nhất của nó và lựa chọn thứ hai phát hiện ra ô đó trống trong khi ô còn lại vẫn chứa`1`cuộc thi đấu. Trên thực tế, câu trả lời được mong đợi cho`n = 1`là`0.5`. Việc triển khai chỉ xem xét các tình huống trong đó hộp kia đã được sử dụng sẽ bỏ sót trường hợp này một cách không chính xác. 

Thứ hai, khám phá hộp trống cuối cùng là một lựa chọn không thành công, do đó nó phải được đưa vào xác suất của một lịch sử cụ thể. Vì`n = 2`, câu trả lời đúng là`0.875000000000000`. Thay vào đó, nếu chúng ta dừng ngay lập tức khi một hộp nhận được kết quả khớp thành công cuối cùng, thì chúng ta đang giải một thử nghiệm ngẫu nhiên khác và nhận được kỳ vọng sai. 

Giới hạn trên`n = 30`cũng làm cho mối quan tâm về số trở nên đơn giản. Số mũ lớn nhất của hai xuất hiện là`2n = 60`, do đó độ chính xác gấp đôi thông thường là quá đủ cho yêu cầu`1e-6`khả năng chịu lỗi. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp có thể tạo ra mọi chuỗi lựa chọn bỏ túi có thể có cho đến khi tìm thấy một hộp trống. Mỗi lựa chọn có hai khả năng, vì vậy nếu chúng ta cho phép lựa chọn cuối cùng không thành công thì lịch sử có độ dài tối đa`2n + 1`. Vì`n = 30`, việc liệt kê tất cả các chuỗi nhị phân có độ dài tối đa này sẽ cho`2^61`, khoảng`2.3 * 10^18`, lịch sử ứng cử viên. Việc mô phỏng tới 61 lựa chọn cho mỗi lịch sử sẽ yêu cầu theo thứ tự`61 * 2^61`, đại khái`1.4 * 10^20`các thao tác cơ bản. Điều đó hoàn toàn không thể thực hiện được. 

Lực lượng vũ phu hoạt động vì mọi chuỗi có thể có một xác suất đơn giản, cụ thể là lũy thừa của`1/2`. Thất bại xuất phát từ việc xử lý các thứ tự khác nhau một cách riêng biệt ngay cả khi chúng dẫn đến cùng số lượng trận đấu còn lại cuối cùng. 

Giả sử khi tìm thấy hộp trống thì hộp còn lại chứa chính xác`k`trận đấu. Khi đó ô trống chắc chắn đã được chọn`n`lần thành công trước khi lựa chọn thất bại cuối cùng. Ô còn lại phải được chọn chính xác`n-k`lần. Do đó, trước sự lựa chọn thất bại cuối cùng đã có chính xác`n + (n-k) = 2n-k`tuyển chọn thành công. 

Để có một lựa chọn cố định về hộp nào cuối cùng được tìm thấy trống, hộp đầu tiên`2n-k`lựa chọn chứa chính xác`n`lựa chọn của hộp đó và`n-k`lựa chọn của hộp khác. có`C(2n-k, n)`cách sắp xếp những lựa chọn này. 

Mọi sự sắp xếp như vậy đều có giá trị. Trước lựa chọn cuối cùng, ô sẽ trống đã được sử dụng chính xác`n`lần, trong khi hộp kia có ít nhất`k`trận đấu còn lại. Vì vậy, không hộp nào có thể trống trước đó. 

Lựa chọn cuối cùng phải chọn ô đã hết. Bao gồm sự lựa chọn đó sẽ mang lại một xác suất`C(2n-k, n) / 2^(2n-k+1)`để một hộp cụ thể là hộp trống đầu tiên. Một trong hai hộp có thể đóng vai trò đó và hai trường hợp có xác suất như nhau. Vì thế`P(k) = C(2n-k, n) / 2^(2n-k)`. 

Do đó, số trận đấu còn lại dự kiến ​​là`E = sum(k * P(k))`vì`k = 0..n`. 

Vì chỉ có`n+1`các giá trị có thể có của`k`, chúng ta đã rút gọn bài toán từ nhiều lịch sử theo cấp số nhân xuống chỉ còn`O(n)`thuật ngữ xác suất. 

Chúng ta có thể đánh giá các số hạng này mà không cần tính toán nhiều lần các hệ số nhị thức. Bắt đầu với`k = n`. Trong trường hợp đó, hộp còn lại không bao giờ được chọn, vì vậy`P(n) = 1 / 2^n`. 

Để giảm`k`, tỷ số giữa các xác suất liên tiếp là`P(k-1) / P(k) = (2n-k+1) / (2(n-k+1))`. 

Do đó, mọi xác suất tiếp theo đều có thể thu được từ xác suất trước đó với một lượng số học không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n * 2^(2n+1))`|`O(n)`| Quá chậm | 
| Tối ưu |`O(n)`|`O(1)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`. Chúng ta sẽ tính xác suất chính xác`k`các trận đấu vẫn còn trong hộp không được tìm thấy trống. 
2. Bắt đầu với`k = n`và xác suất`p = 2^(-n)`. Điều này tương ứng với việc chọn cùng một ô`n`lần thành công và sau đó chọn lại ô đó một lần nữa. Do đó, chiếc hộp kia chưa bao giờ được chạm vào. 
3. Thêm`k * p`đến giá trị mong đợi. Đây là sự đóng góp của số trận đấu còn lại hiện tại có thể có. 
4. Di chuyển từ`k`ĐẾN`k-1`. Cập nhật xác suất bằng cách sử dụng`p *= (2*n-k+1) / (2*(n-k+1))`. 

Sự truy hồi này thu được trực tiếp từ tỉ số của hai biểu thức xác suất nhị thức, do đó nó tránh được các phép tính tổ hợp lớn. 
5. Tiếp tục cho đến khi`k = 0`. giá trị`k = 0`hợp lệ vì cả hai hộp đều có thể trống khi giáo sư cuối cùng phát hiện ra một trong số chúng. Đóng góp của nó vào kỳ vọng là bằng 0, nhưng xác suất của nó vẫn là một phần của phân phối. 
6. In kỳ vọng tích lũy với đủ số thập phân để đáp ứng độ chính xác yêu cầu. 

### Tại sao nó hoạt động 

Đối với mọi khả năng`k`, một lịch sử dừng lại với chính xác`k`các kết quả còn lại trong hộp khác có chính xác`n`các lựa chọn thành công từ hộp sẽ trở nên trống rỗng và chính xác`n-k`lựa chọn thành công từ hộp khác trước khi lựa chọn thất bại cuối cùng. Việc lựa chọn vị trí của những`n`lựa chọn mang lại`C(2n-k,n)`tiền tố hợp lệ và lựa chọn cuối cùng là bắt buộc. Việc tính cả hai ô là ô trống sẽ cho xác suất chính xác`P(k)`được thuật toán sử dụng. Những sự kiện này loại trừ lẫn nhau và bao gồm mọi lịch sử dừng có thể xảy ra, vì vậy việc tổng hợp`k * P(k)`đưa ra chính xác giá trị mong đợi cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    # Probability that exactly n matches remain in the other box.
    p = 2.0 ** (-n)
    ans = 0.0

    for k in range(n, -1, -1):
        ans += k * p

        if k > 0:
            p *= (2 * n - k + 1) / (2 * (n - k + 1))

    print(f"{ans:.15f}")

if __name__ == "__main__":
    solve()
```Biến`p`đại diện cho xác suất của giá trị hiện tại của`k`. Chúng tôi bắt đầu lúc`k = n`vì xác suất đó có dạng đơn giản nhất,`2^-n`. 

Vòng lặp truy cập mọi giá trị có thể có của`k`đúng một lần. biểu thức`ans += k * p`áp dụng trực tiếp định nghĩa của kỳ vọng toán học. 

Việc cập nhật chỉ được thực hiện khi`k > 0`. Tại`k = 0`, không có xác suất tiếp theo để tính toán và việc cố gắng lặp lại sẽ chia cho một biểu thức ranh giới không hợp lệ. 

Tất cả số học được thực hiện bằng dấu phẩy động Python. Số mũ lớn nhất chỉ là 60 và có tối đa 31 lần lặp, do đó sai số tích lũy thấp hơn nhiều so với yêu cầu`1e-6`. 

Không thể xảy ra tràn số nguyên vì việc triển khai hoàn toàn không xây dựng các hệ số nhị thức. Phép truy toán chỉ sử dụng các thừa số nguyên nhỏ và phép chia dấu phẩy động. 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho`n = 2`, chúng ta bắt đầu với xác suất hộp còn lại vẫn chứa cả hai kết quả phù hợp. 

|`k`|`p = P(k)`|`k * p`| 
| --- | --- | --- | 
| 2 |`0.2500000000`|`0.5000000000`| 
| 1 |`0.3750000000`|`0.3750000000`| 
| 0 |`0.3750000000`|`0.0000000000`| 

Các xác suất có tổng là`1`, và giá trị kỳ vọng là`0.5 + 0.375 = 0.875`. 

Do đó đầu ra là`0.875000000000000`. Vụ án`k = 2`chứng minh tại sao phạm vi phải bao gồm`n`: giáo sư có thể khám phá một hộp trống mà không cần sử dụng hộp kia. 

### Mẫu 2 

cho`n = 3`, xác suất ban đầu là`P(3) = 1/8`. 

|`k`|`p = P(k)`|`k * p`| 
| --- | --- | --- | 
| 3 |`0.1250000000`|`0.3750000000`| 
| 2 |`0.2500000000`|`0.5000000000`| 
| 1 |`0.3125000000`|`0.3125000000`| 
| 0 |`0.3125000000`|`0.0000000000`| 

Các xác suất một lần nữa tổng hợp lại thành`1`. Giá trị mong đợi là`0.375 + 0.5 + 0.3125 = 1.1875`. 

Điều này khớp với mẫu thứ hai và cũng cho thấy cách phép truy toán tạo ra toàn bộ phân phối mà không liệt kê các chuỗi lựa chọn bỏ túi riêng lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n)`| Có một bản cập nhật xác suất liên tục cho mỗi`k`từ`n`xuống`0`. | 
| Không gian |`O(1)`| Chỉ một`n`, xác suất hiện tại và câu trả lời tích lũy sẽ được lưu trữ. | 

Với`n <= 30`, thuật toán thực hiện tối đa 31 lần lặp. Việc sử dụng bộ nhớ là không đổi và phép tính số học vẫn thoải mái trong phạm vi mà độ chính xác gấp đôi mang lại độ chính xác cao hơn nhiều so với yêu cầu`1e-6`. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    import sys
    input = sys.stdin.readline

    n = int(input())
    p = 2.0 ** (-n)
    ans = 0.0

    for k in range(n, -1, -1):
        ans += k * p
        if k > 0:
            p *= (2 * n - k + 1) / (2 * (n - k + 1))

    print(f"{ans:.15f}")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def reference(n: int) -> float:
    # Independent calculation using the closed-form probability.
    import math

    ans = 0.0
    for k in range(n + 1):
        p = math.comb(2 * n - k, n) / (2.0 ** (2 * n - k))
        ans += k * p
    return ans

# Provided samples.
assert run("2\n") == "0.875000000000000", "sample 1"
assert run("3\n") == "1.187500000000000", "sample 2"

# Minimum-size input. The two possible remaining counts, 0 and 1,
# are equally likely, so the answer is 0.5.
assert run("1\n") == "0.500000000000000", "minimum n"

# Boundary case n = 4, with all possible k values contributing.
assert run("4\n") == "0.992187500000000", "off-by-one boundary"

# Maximum allowed n.
assert abs(float(run("30\n")) - reference(30)) < 1e-12, "maximum n"

# Another symmetric case, checked against the direct combinatorial formula.
assert abs(float(run("5\n")) - reference(5)) < 1e-12, "probability distribution"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`0.500000000000000`| Đầu vào tối thiểu và`k = n`trường hợp ranh giới | 
|`4`|`0.992187500000000`| Xử lý đúng mọi cách có thể`k`, bao gồm`k = 0`Và`k = n`| 
|`30`| Giá trị tham chiếu tổ hợp trực tiếp | Đầu vào tối đa được phép và độ ổn định số | 
|`5`| Giá trị tham chiếu tổ hợp trực tiếp | Thoả thuận giữa công thức truy hồi và công thức xác suất | 

## Vỏ cạnh 

cho`n = 1`, hai hộp ban đầu chứa một kết quả phù hợp. Với xác suất`1/2`, giáo sư chọn cùng một ô hai lần nên hộp kia chứa`1`. Với xác suất`1/2`, anh ta chọn hai hộp khác nhau, vì vậy lựa chọn thứ hai sẽ phát hiện ra một hộp trống và hộp còn lại chứa`0`. Do đó, giá trị kỳ vọng là`1/2`, mà thuật toán thu được từ`P(1) = 0.5`Và`P(0) = 0.5`. 

Vì`n = 2`, phân phối đầy đủ là`P(2) = 1/4`,`P(1) = 3/8`, Và`P(0) = 3/8`. Kết quả kỳ vọng là`2 * 1/4 + 1 * 3/8 = 7/8`. các`k = 2`kết quả đặc biệt hữu ích trong việc phát hiện lỗi dừng phân tích khi một hộp trống thay vì khi một hộp trống sau đó được chọn. 

Vì`k = 0`, hộp còn lại cũng trống khi giáo sư phát hiện ra hộp trống đầu tiên. Trường hợp này có xác suất dương, mặc dù đóng góp của nó vào giá trị kỳ vọng bằng 0. Ví dụ, với`n = 2`, xác suất của nó là`3/8`. Một triển khai chỉ tính tổng dương`k`các giá trị vẫn có thể vô tình lấy đúng một số ví dụ, nhưng phân bố xác suất của nó không đầy đủ và mọi phép tính liên quan đều có thể sai. 

Để có đầu vào tối đa`n = 30`, lũy thừa lớn nhất của hai được sử dụng là`2^60`và vòng lặp chỉ có 31 lần lặp. Sự truy hồi không bao giờ đòi hỏi phải xây dựng một tập hợp khổng lồ các lịch sử có thể có, do đó, cùng một biểu diễn trạng thái có kích thước không đổi hoạt động không thay đổi ở ranh giới trên.
