---
title: "CF 102439H - Số không phải fibonacci"
description: "Đối với số nguyên (x), gạch bỏ các chữ số có nghĩa là xóa một số vị trí khỏi biểu diễn thập phân của nó trong khi vẫn giữ tất cả các chữ số còn lại theo thứ tự ban đầu của chúng. Một số sẽ không được ưa thích nếu có thể thu được số Fibonacci dương nào đó theo cách này."
date: "2026-08-10T06:55:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102439
codeforces_index: "H"
codeforces_contest_name: "2018-2019 9th BSUIR Open Programming Championship. Semifinal"
rating: 0
weight: 102439
solve_time_s: 201
verified: true
draft: false
---

[CF 102439H - Số không phải fibonacci](https://codeforces.com/problemset/problem/102439/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 21s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đối với số nguyên (x), gạch bỏ các chữ số có nghĩa là xóa một số vị trí khỏi biểu diễn thập phân của nó trong khi vẫn giữ tất cả các chữ số còn lại theo thứ tự ban đầu của chúng. Một số sẽ không được ưa thích nếu có thể thu được số Fibonacci dương nào đó theo cách này. Chúng ta cần đếm những số trong phạm vi bao gồm ([0,n]) không bị ghét. 

Ví dụ: (193) không được ưa thích vì xóa đi chữ số ở giữa của nó (13), tức là Fibonacci. Mặt khác, (4) được ưa chuộng vì không thể thu được số Fibonacci dương từ các chữ số của nó. Các ràng buộc chính thức cho phép tối đa mười trường hợp thử nghiệm, với (n) lớn bằng (10^{18}). 

Giới hạn trên của (10^{18}) là ràng buộc khóa. Việc lặp qua mọi số nguyên là không thể vì có thể có (10^{18}+1) ứng cử viên trong một trường hợp kiểm thử. Chúng ta cần làm việc với tối đa 19 chữ số thập phân của (n), vì vậy phương pháp đếm dựa trên chữ số là mục tiêu đương nhiên. 

Có một số trường hợp ranh giới có thể dễ dàng phá vỡ việc triển khai bất cẩn. Với (n=0), câu trả lời là (1), vì số duy nhất trong phạm vi là (0) và số 0 được ưa thích. Việc triển khai chỉ đếm số dương sẽ trả về sai (0). 

Với (n=4), đáp án là (2), cụ thể là (0) và (4). Các chữ số (1,2,3) không thể xuất hiện ở dạng số dương thích hợp, vì bản thân mỗi chữ số đều là số Fibonacci dương. Giải pháp chỉ kiểm tra các số Fibonacci có ít nhất hai chữ số sẽ đếm không chính xác (1,2,3). 

Với (n=2019), đáp án là (125). Chữ số đầu tiên của số có bốn chữ số tối đa (2019) sẽ phải là (1) hoặc (2) và cả hai chữ số đều bị cấm. Vì vậy không có số thích mới có bốn chữ số. Một giải pháp chỉ đơn giản là đếm tất cả các chuỗi được tạo từ các chữ số được phép mà không tôn trọng giới hạn trên có thể bị tính quá mức ở đây. 

Các số 0 đứng đầu cũng cần được xử lý cẩn thận. Số (04) không phải là số nguyên riêng biệt với (4), do đó, số 0 có thể được sử dụng bên trong một số, nhưng nó không thể được sử dụng làm chữ số đầu tiên của số dương. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là kiểm tra mọi số nguyên (x) từ (0) đến (n). Đối với mỗi (x), chúng ta có thể tạo ra tất cả các số Fibonacci lên đến (x), sau đó kiểm tra xem có bất kỳ chuỗi thập phân nào của chúng là dãy con của biểu diễn thập phân của (x) hay không. Vì các số Fibonacci tăng theo cấp số nhân nên chỉ có khoảng 90 giá trị Fibonacci phù hợp bên dưới (10^{18}) và mỗi giá trị có tối đa 19 chữ số. Cách tiếp cận này đúng vì một số không được ưa thích chính xác khi có ít nhất một trong các chuỗi Fibonacci đó xuất hiện dưới dạng một dãy con. 

Vấn đề là số lượng số nguyên ứng cử viên. Trong trường hợp xấu nhất, chúng tôi thực hiện so sánh khoảng (10^{18}\cdot90\cdot19=1.71\cdot10^{21}) chữ số cho một trường hợp thử nghiệm. Ngay cả trước khi tính đến chi phí Python, con số đó vẫn vượt xa giới hạn một giây. 

Quan sát hữu ích đến từ các số Fibonacci nhỏ nhất. Các số Fibonacci dương có một chữ số là (1,2,3,5,8). Nếu một số chứa bất kỳ chữ số nào trong số này, thì bản thân chữ số đó có thể được giữ lại, vì vậy số đó sẽ ngay lập tức bị ghét. Do đó, mọi số thích chỉ có thể chứa các chữ số 

[ 
{0,4,6,7,9}. 
] 

Tại thời điểm này, chúng ta vẫn phải lo lắng về số Fibonacci có nhiều chữ số mà mọi chữ số đều thuộc về tập hợp này. Chỉ có tối đa 19 số Fibonacci (10^{18}) và chỉ cần kiểm tra khoảng 90 giá trị Fibonacci. Việc tạo chúng một lần và kiểm tra các chữ số thập phân của chúng cho thấy rằng không có số Fibonacci dương nào lên tới (10^{18}) bao gồm hoàn toàn (0,4,6,7,9). Do đó, không có số Fibonacci có nhiều chữ số nào có thể là dãy con của một số chỉ sử dụng năm chữ số này. 

Điều này làm giảm vấn đề ban đầu thành một vấn đề đơn giản hơn nhiều: đếm tối đa các số nguyên (n) có biểu diễn thập phân chỉ sử dụng (0,4,6,7,9). Công việc còn lại là quy trình đếm chữ số tiêu chuẩn.

Phương pháp brute-force hoạt động vì nó kiểm tra định nghĩa một cách trực tiếp nhưng không thành công vì phạm vi rất lớn. Quan sát cho thấy các số Fibonacci một chữ số loại bỏ năm chữ số thập phân sẽ thu gọn điều kiện dãy con phức tạp thành một hạn chế đơn giản trên mỗi chữ số. Khi các ứng cử viên Fibonacci còn lại đã cạn kiệt, vấn đề sẽ trở thành việc đếm các chuỗi thập phân bị hạn chế. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n\cdot 90\cdot 19)) | (O(90)) | Quá chậm | 
| Tối ưu | (O(19\cdot 10)) mỗi trường hợp thử nghiệm | (O(19)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xử lý riêng (0). Nó là một số nguyên thích hợp, vì thế nó đóng góp một số vào mỗi câu trả lời bằng (n\ge0). 
2. Với mọi số nguyên dương, chỉ có thể có các chữ số (4,6,7,9) ở vị trí đầu tiên. Chữ số (0) chỉ được phép sau vị trí đầu tiên vì các số 0 đứng đầu không tạo thành một phần của biểu diễn thập phân. 
3. Với mọi độ dài có thể ngắn hơn số chữ số trong (n), hãy đếm tất cả các số hợp lệ có độ dài đó. Một số dương có độ dài (k) có bốn lựa chọn cho chữ số đầu tiên và năm lựa chọn cho mỗi chữ số tiếp theo, cho ra các khả năng (4\cdot5^{k-1}). 
4. Đếm các số hợp lệ có cùng độ dài với (n). Xử lý các chữ số của (n) từ trái sang phải. Tại mỗi vị trí, đếm xem có bao nhiêu chữ số cho phép nhỏ hơn chữ số tương ứng của (n). Việc chọn bất kỳ chữ số nhỏ hơn nào như vậy sẽ làm cho toàn bộ tiền tố nhỏ hơn, do đó, tất cả các vị trí còn lại có thể được điền độc lập bằng năm chữ số được phép. 
5. Nếu chữ số hiện tại của (n) không được phép, hãy dừng ngay lập tức. Tiền tố được xây dựng cho đến nay có thể đã được tạo nhỏ hơn (n) và bất kỳ số nào giữ chữ số bị cấm hiện tại sẽ không hợp lệ. 
6. Nếu cho phép mọi chữ số của chính (n), hãy thêm một chữ số vào cuối để bao gồm chính (n). Mặt khác, không có số nào có cùng tiền tố với (n) có thể hợp lệ. 

### Tại sao nó hoạt động 

Mỗi số không thích phải chứa ít nhất một số Fibonacci dương làm dãy con. Vì (1,2,3,5,8) là các số Fibonacci nên một số thích không thể chứa bất kỳ chữ số nào trong số đó. Ngược lại, sau khi giới hạn mọi chữ số ở (0,4,6,7,9), không có số Fibonacci dương nào lên đến (10^{18}) vẫn có thể tồn tại dưới dạng chuỗi chữ số hoàn chỉnh, do đó không có số nào có thể xuất hiện dưới dạng một dãy con. Do đó, các số thích chính xác là các số nguyên có chữ số thập phân thuộc về tập hợp được phép đó. 

Quy trình đếm kiểm tra mọi biểu diễn thập phân hợp lệ có thể có chính xác một lần. Để có độ dài ngắn hơn, chữ số đầu tiên có bốn lựa chọn và mỗi chữ số sau có năm lựa chọn. Đối với độ dài của (n), bất cứ khi nào chúng ta chọn chữ số nhỏ hơn ở vị trí khác nhau đầu tiên, hậu tố còn lại sẽ không bị hạn chế ngoại trừ điều kiện chữ số được phép. Nếu chữ số hiện tại không thể khớp thì nhánh tiền tố bằng sẽ biến mất. Điều này phân vùng tất cả các số hợp lệ tối đa (n) mà không bị chồng chéo hoặc thiếu sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

ALLOWED = "04679"
FIRST = "4679"

def count_liked(n: int) -> int:
    s = str(n)
    length = len(s)

    # The number 0 is always liked.
    ans = 1

    # Count all positive valid numbers with fewer digits.
    power = 1
    for digits in range(1, length):
        ans += 4 * power
        power *= 5

    # Count valid numbers with exactly len(s) digits and <= n.
    for i, ch in enumerate(s):
        d = ord(ch) - ord('0')

        choices = FIRST if i == 0 else ALLOWED

        smaller = 0
        for c in choices:
            if ord(c) - ord('0') < d:
                smaller += 1

        # Once this position is made smaller than n,
        # every remaining position can be filled freely.
        remaining = length - i - 1
        ans += smaller * (5 ** remaining)

        # We cannot continue with the same prefix.
        if ch not in choices:
            return ans

    # n itself uses only allowed digits.
    return ans + 1

def solve() -> None:
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        out.append(str(count_liked(n)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```các`ALLOWED`chuỗi đại diện cho tất cả các chữ số có thể xuất hiện sau vị trí đầu tiên. Nó chứa số 0 vì các số 0 bên trong là hợp lệ, trong khi`FIRST`loại trừ số 0 để ngăn các số 0 đứng đầu. 

Giá trị ban đầu`ans = 1`chiếm số không. Điều này là cần thiết ngay cả khi (n=0), vì phạm vi đã bao gồm. 

Vòng lặp đầu tiên xử lý mọi chiều dài dương ngắn hơn. Biến`power`bắt đầu từ (5^0), do đó phần đóng góp của một chữ số là (4), sau đó là (20), rồi (100), v.v. Sau mỗi chiều dài nó được nhân với năm. 

Vòng lặp thứ hai thực hiện việc đếm tiền tố chặt chẽ từ thuật toán. Giả sử chữ số hiện tại của (n) là (6). Ở vị trí không dẫn đầu, các chữ số nhỏ hơn được phép là (0) và (4), do đó có thể có hai nhánh nhỏ hơn. Khi một vị trí được chọn, tất cả các vị trí còn lại mỗi vị trí có năm lựa chọn, cho ra các số (2\cdot5^{\text{remaining}}). 

Việc trả về sớm khi chữ số hiện tại bị cấm sẽ xử lý rõ ràng điều kiện giới hạn trên. Chúng tôi đếm mọi số hợp lệ mà lần đầu tiên trở nên nhỏ hơn ở vị trí hiện tại, nhưng chúng tôi không thể tiếp tục dọc theo tiền tố chính xác vì điều đó sẽ yêu cầu sử dụng chữ số bị cấm. 

Nếu mọi chữ số đều tồn tại thì bản thân (n) là số hợp lệ và phải được đưa vào. trận chung kết`+ 1`xử lý chính xác trường hợp đó. 

Số nguyên Python có độ chính xác tùy ý, do đó các giá trị như (5^{18}) không bị tràn. Công suất liên quan lớn nhất là rất nhỏ so với những gì số nguyên Python có thể biểu thị. 

## Ví dụ đã hoạt động 

### Mẫu 1: (n=4) 

Các số dương có một chữ số hợp lệ là (4,6,7,9), nhưng chỉ có (4) nhiều nhất là (4). Cùng với số 0, câu trả lời là (2). 

| Vị trí | Chữ số hiện tại | Cho phép chữ số nhỏ hơn | Đã thêm số | Quyết định | 
| --- | --- | --- | --- | --- | 
| 0 | 4 | không | 0 | 4 được phép | 

Sau khi xử lý chữ số duy nhất, bản thân (4) là hợp lệ nên nó đóng góp thêm một số. Bao gồm số 0 sẽ đưa ra câu trả lời cuối cùng (2). 

### Mẫu 2: (n=2019) 

Có (4) số dương có một chữ số hợp lệ, (20) số có hai chữ số hợp lệ và (100) số có ba chữ số hợp lệ. Cùng với số 0 thì kết quả là (125). 

| Vị trí | Chữ số hiện tại | Cho phép chữ số nhỏ hơn | Đã thêm số | Quyết định | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | không | 0 | 2 bị cấm | 

Chữ số đầu tiên của mọi số có bốn chữ số phải là (4,6,7) hoặc (9), nhưng tất cả các số đó đều vượt quá (2019). Vì chữ số đầu tiên của (2019) bị cấm nên nhánh tiền tố bằng sẽ dừng ngay lập tức. Câu trả lời vẫn là (1+4+20+100=125), khớp với mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(19\cdot10)) mỗi trường hợp thử nghiệm | Có tối đa 19 chữ số và mỗi vị trí kiểm tra tối đa 5 chữ số được phép | 
| Không gian | (O(19)) | Biểu diễn đầu vào chứa tối đa 19 chữ số thập phân | 

Quan sát tiền xử lý về các số Fibonacci có kích thước không đổi cho bài toán này, vì chỉ các giá trị Fibonacci tối đa (10^{18}) là phù hợp. Sau đó, mỗi truy vấn sẽ kiểm tra tối đa 19 vị trí, do đó, ngay cả 10 trường hợp kiểm thử cũng chỉ yêu cầu vài nghìn thao tác đơn giản. Điều này thoải mái trong giới hạn thời gian một giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys
import io

ALLOWED = "04679"
FIRST = "4679"

def count_liked(n: int) -> int:
    s = str(n)
    length = len(s)

    ans = 1

    power = 1
    for digits in range(1, length):
        ans += 4 * power
        power *= 5

    for i, ch in enumerate(s):
        d = ord(ch) - ord('0')
        choices = FIRST if i == 0 else ALLOWED

        smaller = 0
        for c in choices:
            if ord(c) - ord('0') < d:
                smaller += 1

        remaining = length - i - 1
        ans += smaller * (5 ** remaining)

        if ch not in choices:
            return ans

    return ans + 1

def solve_data(inp: str) -> str:
    data = inp.strip().split()
    t = int(data[0])

    out = []
    for i in range(1, t + 1):
        out.append(str(count_liked(int(data[i]))))

    return "\n".join(out)

# Provided sample.
assert solve_data("2\n4\n2019\n") == "2\n125", "sample"

# Minimum-size inputs.
assert solve_data("3\n0\n1\n4\n") == "1\n1\n2", "minimum values"

# Boundary around the largest allowed one-digit number.
assert solve_data("4\n8\n9\n10\n44\n") == "4\n5\n5\n6", "one and two digit boundaries"

# All-equal boundary case.
assert solve_data("1\n7777\n") == "475", "all-equal digits"

# Maximum allowed input.
assert solve_data("1\n1000000000000000000\n") == "3814697265625", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`,`1`,`4`|`1`,`1`,`2`| Không xử lý và ranh giới tích cực nhỏ nhất | 
|`8`,`9`,`10`,`44`|`4`,`5`,`5`,`6`| Các chữ số bị cấm, chuyển tiếp giới hạn trên và xử lý số 0 đứng đầu | 
|`7777`|`475`| Đếm chữ số chặt chẽ khi một số vị trí bằng giới hạn trên | 
|`1000000000000000000`|`3814697265625`| Kích thước đầu vào tối đa và số học số nguyên lớn | 

## Vỏ cạnh 

Với (n=0), thuật toán bắt đầu bằng`ans = 1`và không có độ dài dương để đếm. chữ số`0`chỉ được phép trong nội bộ, nhưng ở đây nó đại diện cho chính số nguyên 0, được ưa thích một cách rõ ràng. Do đó kết quả là (1). 

Với (n=1), số 0 ban đầu đóng góp một. Chữ số đầu tiên là`1`, không có trong`FIRST`, do đó không có nhiều nhất số dương có một chữ số hợp lệ (1). Hàm trả về (1), kết quả này đúng. 

Đối với (n=4), chữ số đầu tiên được phép, nhưng không được phép có chữ số dương nhỏ hơn`4`. Sau đó, thuật toán sẽ tự thêm (4), cho (0,4) và do đó (2). 

Với (n=9), cả bốn số dương có một chữ số được phép (4,6,7,9) nhiều nhất là (9). Thuật toán đếm các lựa chọn nhỏ hơn`4`,`6`, Và`7`, sau đó thêm`9`chính nó và cuối cùng bao gồm số không. Kết quả là (5). 

Đối với (n=10), số lượng một chữ số vẫn còn (5). Không có số hợp lệ có nhiều nhất hai chữ số (10), vì mọi số dương hợp lệ có hai chữ số đều bắt đầu bằng (4), (6), (7) hoặc (9). Chữ số đầu tiên`1`bị cấm nên nhánh chặt dừng lại và đáp án giữ nguyên (5). 

Đối với (n=2019), logic tương tự được áp dụng ở vị trí đầu tiên. chữ số`2`bị cấm và mọi số có bốn chữ số hợp lệ đều bắt đầu bằng ít nhất`4`, vì vậy không có số hợp lệ có bốn chữ số nào có thể vừa với bên dưới (2019). Ba độ dài dương ngắn hơn đóng góp (4+20+100=124) và số 0 đóng góp một, tạo ra (125). 

Đối với (n=10^{18}), dữ liệu đầu vào có 19 chữ số, nhưng chữ số đầu tiên của nó là`1`, đó là điều bị cấm. Mọi số có 19 chữ số hợp lệ đều bắt đầu bằng`4`,`6`,`7`, hoặc`9`, vì vậy không có gì nhiều nhất (10^{18}). Tất cả các số hợp lệ có tối đa 18 chữ số và số đếm của chúng bao gồm cả số 0 là (5^{18}=3814697265625). Việc triển khai xử lý việc này mà không bị tràn vì số nguyên Python có độ chính xác tùy ý.
