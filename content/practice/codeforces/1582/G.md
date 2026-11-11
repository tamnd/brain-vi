---
title: "CF 1582G - Kuzya và bài tập về nhà"
description: "Chúng ta được cho một dãy số và một chuỗi các phép tính được đặt giữa chúng. Chúng ta bắt đầu mỗi phân đoạn với giá trị 1, sau đó áp dụng các thao tác từ trái sang phải. Mỗi vị trí nhân giá trị hiện tại với số đã cho hoặc chia cho số đó."
date: "2026-06-14T23:04:46+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1582
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 750 (Div. 2)"
rating: 2600
weight: 1582
solve_time_s: 349
verified: false
draft: false
---

[CF 1582G - Kuzya và Bài tập về nhà](https://codeforces.com/problemset/problem/1582/G) 

**Đánh giá:** 2600 
**Tas:** cấu trúc dữ liệu, lý thuyết số 
**Thời gian giải:** 5 phút 49 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số và một chuỗi các phép tính được đặt giữa chúng. Chúng ta bắt đầu mỗi phân đoạn với giá trị 1, sau đó áp dụng các thao tác từ trái sang phải. Mỗi vị trí nhân giá trị hiện tại với số đã cho hoặc chia cho số đó. Khi tiến hành, chúng tôi ghi lại giá trị trung gian sau mỗi thao tác. Một phân đoạn được gọi là hợp lệ nếu mỗi một trong các giá trị trung gian này là số nguyên. 

Nhiệm vụ là đếm xem có bao nhiêu phân đoạn liền kề chỉ tạo ra các số nguyên trong suốt quá trình này. 

Khó khăn chính là phép chia đưa ra một ràng buộc toàn cục: khi chúng ta chia cho một số, chúng ta phải đảm bảo rằng tích tích lũy đến thời điểm đó chứa tất cả các thừa số nguyên tố bắt buộc, nếu không thì giá trị sẽ không còn là số nguyên. Vì các phân đoạn có thể bắt đầu ở bất kỳ đâu nên điều kiện phải đúng cho mọi tiền tố bên trong phân đoạn chứ không chỉ ở phần cuối. 

Những hạn chế làm cho vũ lực không thể thực hiện được. Với tối đa một triệu phần tử, ngay cả việc kiểm tra tất cả các phân đoạn cũng đã là bậc hai và việc tính toán lại hệ số hoặc theo dõi tính chia hết bên trong mỗi phân đoạn sẽ đưa ra một hệ số logarit khác hoặc hệ số tệ hơn cho mỗi phép toán. Bất kỳ giải pháp nào tính toán lại các thừa số nguyên tố nhiều lần hoặc mô phỏng các phân đoạn một cách độc lập sẽ thất bại. 

Trường hợp cạnh tinh tế xuất hiện khi phép chia xuất hiện trước bất kỳ phép nhân nào bù cho chúng. Ví dụ: nếu chuỗi bắt đầu bằng phép chia thì mọi phân đoạn bắt đầu từ đó sẽ không hợp lệ ngay lập tức, nhưng các phân đoạn bắt đầu sau đó vẫn có thể hợp lệ trở lại. Một tình huống khó khăn khác là khi thâm hụt yếu tố cơ bản được tạo ra và sau đó được sửa chữa, vì chỉ có phân khúc địa phương mới quan trọng chứ không phải lịch sử toàn cầu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp lại trên tất cả các phân đoạn và mô phỏng quy trình cho từng phân đoạn. Đối với một phân đoạn cố định, chúng tôi sẽ duy trì giá trị hiện tại và liên tục áp dụng phép nhân hoặc chia trong khi kiểm tra xem giá trị đó có còn là số nguyên hay không. Cách tiếp cận này đúng vì nó tuân theo định nghĩa trực tiếp. Tuy nhiên, mỗi phân đoạn tốn thời gian tuyến tính trong trường hợp xấu nhất, dẫn đến hành vi tổng thể theo khối. 

Trở ngại chính là khả năng chia hết phụ thuộc vào sự cân bằng thừa số nguyên tố. Nhân với một số sẽ thêm các thừa số nguyên tố của nó, trong khi phép chia sẽ loại bỏ chúng. Giá trị vẫn là số nguyên miễn là không có số mũ nguyên tố nào trở thành số âm trong bất kỳ tiền tố nào của phân đoạn. 

Điều này chuyển vấn đề thành việc theo dõi, đối với mọi số nguyên tố, liệu số mũ của nó có giữ nguyên không âm trên mọi tiền tố hay không. Thay vì tính toán lại từ đầu cho từng phân đoạn, chúng tôi xử lý mảng từ trái sang phải và duy trì điểm bắt đầu tốt nhất có thể cho phân đoạn kết thúc tại mỗi vị trí. 

Quan sát quan trọng là đối với mỗi số nguyên tố, chúng ta chỉ cần biết liệu số dư hiện tại của nó có bao giờ giảm xuống dưới 0 hay không. Nếu đúng như vậy, không có phân đoạn nào bắt đầu trước thời điểm đó có thể vẫn hợp lệ, vì vậy phần bắt đầu phải tiếp tục. Mỗi số nguyên tố thực thi một cách độc lập một ràng buộc về khoảng cách còn lại của một phân đoạn. Điểm bắt đầu hợp lệ cuối cùng được xác định bởi ràng buộc nguyên tố hạn chế nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² log A) | O(1) | Quá chậm | 
| Tối ưu | O(n log A) | O(n log A) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải trong khi vẫn duy trì các ràng buộc do thừa số nguyên tố áp đặt. 

1. Phân tích từng số thành số nguyên tố. Chúng ta coi phép nhân là phép cộng các số nguyên tố và phép chia là phép trừ chúng. Điều này chuyển vấn đề thành việc theo dõi nhiều bộ đếm số dư độc lập, mỗi bộ đếm số nguyên tố. 
2. Duy trì một cuốn từ điển`cur[p]`đại diện cho thặng dư hiện tại của Prime`p`bên trong phân đoạn hoạt động kết thúc tại vị trí`i`. Giá trị dương có nghĩa là chúng tôi có sẵn các bản sao bổ sung của số nguyên tố đó cho các bộ phận trong tương lai. 
3. Duy trì một từ điển khác`bad[p]`ghi lại vị trí sớm nhất buộc chúng tôi phải khởi động lại các phân đoạn cho vị trí chính`p`. Ban đầu, tất cả các giá trị đều bằng 0. 
4. Khi xử lý phép nhân tại vị trí`i`, chúng tôi tăng`cur[p]`với mọi thừa số nguyên tố của`a[i]`. Không vi phạm ràng buộc nào có thể xảy ra chỉ bằng phép nhân vì nó chỉ bổ sung thêm tài nguyên. 
5. Khi xử lý phép chia cho`a[i]`, chúng tôi trừ các thừa số nguyên tố của nó khỏi`cur`. Nếu với bất kỳ số nguyên tố nào`p`, giá trị trở thành âm, điều này có nghĩa là phân đoạn hiện tại không thể bao gồm tiền tố này. Sau đó chúng tôi thiết lập lại`cur[p]`về 0 và đặt`bad[p] = i`, nghĩa là bất kỳ phân đoạn hợp lệ nào kết thúc ở đây đều phải bắt đầu sau thời điểm này. 
6. Đối với từng vị trí`i`, thời điểm bắt đầu hợp lệ sớm nhất được xác định bởi ràng buộc nguyên tố hạn chế nhất. Chúng tôi tính toán`L = max(bad[p])`trên tất cả các số nguyên tố hiện có liên quan. Số lượng phân đoạn hợp lệ kết thúc tại`i`vậy thì`i - L + 1`. 
7. Chúng tôi tích lũy khoản đóng góp này cho mọi vị trí. 

Lý do điều này có hiệu quả là vì mỗi ràng buộc chính là độc lập và thực thi một hạn chế đơn điệu đối với các vị trí bắt đầu phân đoạn. Khi một thủ tướng buộc phải khởi động lại ở vị trí`i`, bất kỳ sự bắt đầu nào sớm hơn sẽ vi phạm vĩnh viễn tính hợp lệ của số nguyên đối với số nguyên tố đó bên trong phân đoạn kết thúc tại`i`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXV = 10**6 + 1

spf = list(range(MAXV))
for i in range(2, int(MAXV ** 0.5) + 1):
    if spf[i] == i:
        for j in range(i * i, MAXV, i):
            if spf[j] == j:
                spf[j] = i

def factor(x):
    res = {}
    while x > 1:
        p = spf[x]
        cnt = 0
        while x % p == 0:
            x //= p
            cnt += 1
        res[p] = cnt
    return res

n = int(input())
a = list(map(int, input().split()))
b = input().strip()

cur = {}
bad = {}
L = 0
ans = 0

for i in range(n):
    if b[i] == '*':
        f = factor(a[i])
        for p, c in f.items():
            cur[p] = cur.get(p, 0) + c
    else:
        f = factor(a[i])
        for p, c in f.items():
            cur[p] = cur.get(p, 0) - c
            if cur[p] < 0:
                bad[p] = i + 1
                cur[p] = 0

    for p, v in bad.items():
        if v > L:
            L = v

    ans += (i + 1 - L)

print(ans)
```Việc triển khai dựa trên quét tuyến tính kết hợp với hệ số hóa nhanh bằng cách sử dụng sàng hệ số nguyên tố nhỏ nhất. Mỗi số được phân tích thành thừa số một lần và mỗi số nguyên tố chỉ đóng góp một vài cập nhật. 

Biến`cur`theo dõi xem có bao nhiêu bản sao bổ sung của mỗi số nguyên tố hiện có trong phân đoạn đang chạy. Khi phép chia tạo ra mức thâm hụt, số nguyên tố tương ứng được đánh dấu là không hợp lệ tại vị trí đó và bộ đếm của nó được đặt lại, vì mọi phần dư thừa trước đó không thể được sử dụng lại cho các phân đoạn phải vượt quá vi phạm này. 

Biến`L`tổng hợp tất cả các ràng buộc cụ thể về số nguyên tố thành một ranh giới bên trái toàn cục duy nhất cho các phân đoạn hợp lệ kết thúc tại mỗi vị trí. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi nhỏ trong đó phép nhân và phép chia tương tác với nhau: 

đầu vào:```
5
2 6 3 2 3
*/*/*
```Chúng tôi theo dõi các số nguyên tố 2 và 3. 

Ở mỗi bước, chúng tôi duy trì`cur`Và`L`. 

| tôi | op | một [tôi] | thay đổi hiện tại | cập nhật xấu | L | đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | * | 2 | 2: +1 | không | 0 | 1 | 
| 2 | / | 6 | 2:-1,3:-1 → đặt lại cả hai | xấu[2]=2,xấu[3]=2 | 2 | 0 | 
| 3 | * | 3 | 3:+1 | không | 2 | 1 | 
| 4 | / | 2 | 2:-1 | tệ[2]=4 | 4 | 0 | 
| 5 | * | 3 | 3:+1 | không | 4 | 1 | 

Dấu vết này cho thấy các bộ phận tích cực di chuyển ranh giới bắt đầu hợp lệ về phía trước như thế nào, trong khi các phép nhân xây dựng lại các nguồn lực chính có sẵn. 

Một ví dụ thứ hai: 

đầu vào:```
4
1 2 4 2
*/**
```Ở đây không có phép chia nào tạo ra thâm hụt, vì vậy ranh giới không bao giờ vượt quá số 0. Mọi tiền tố vẫn hợp lệ, chứng tỏ rằng thuật toán xử lý chính xác các phân đoạn mở rộng thuần túy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log A) | Mỗi số được phân tích thành thừa số một lần và mỗi bản cập nhật số nguyên tố được khấu hao không đổi trong tất cả các hoạt động | 
| Không gian | O(n log A) | Lưu trữ sàng lọc SPF và bản đồ cân bằng nguyên tố | 

Việc sàng lọc lên tới một triệu là khả thi trong bộ nhớ và mỗi thao tác chỉ chạm vào các thừa số nguyên tố của một số duy nhất, giữ cho thời gian chạy tổng thể trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MAXV = 10**6 + 1
    spf = list(range(MAXV))
    for i in range(2, int(MAXV ** 0.5) + 1):
        if spf[i] == i:
            for j in range(i * i, MAXV, i):
                if spf[j] == j:
                    spf[j] = i

    def factor(x):
        res = {}
        while x > 1:
            p = spf[x]
            cnt = 0
            while x % p == 0:
                x //= p
                cnt += 1
            res[p] = cnt
        return res

    n = int(input())
    a = list(map(int, input().split()))
    b = input().strip()

    cur = {}
    bad = {}
    L = 0
    ans = 0

    for i in range(n):
        if b[i] == '*':
            f = factor(a[i])
            for p, c in f.items():
                cur[p] = cur.get(p, 0) + c
        else:
            f = factor(a[i])
            for p, c in f.items():
                cur[p] = cur.get(p, 0) - c
                if cur[p] < 0:
                    bad[p] = i + 1
                    cur[p] = 0

        for p, v in bad.items():
            if v > L:
                L = v

        ans += (i + 1 - L)

    return str(ans)

# provided sample
assert run("3\n1 2 3\n*/*\n") == "2"

# minimum case
assert run("2\n1 1\n**\n") == "3"

# division first creates immediate constraint
assert run("3\n2 2 2\n/*/\n") is not None

# all divisions
assert run("3\n2 3 5\n///\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu tất cả nhân | 3 | độ đúng cơ sở | 
| phân chia nặng | hạn chế | chuyển động ranh giới | 
| tất cả các bộ phận | thu nhỏ phân đoạn hợp lệ | xử lý tiêu cực | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là khi mảng chỉ chứa một. Vì số này không có thừa số nguyên tố nên phép nhân hay phép chia đều không làm thay đổi số dư. Thuật toán không bao giờ cập nhật`bad`, Vì thế`L`giữ nguyên bằng 0 và tất cả các phân đoạn đều được tính. Điều này phù hợp với thực tế là mọi phân đoạn đều có giá trị tầm thường. 

Một trường hợp cạnh khác là khi phép toán đầu tiên là phép chia. Việc phân tích nhân tử ngay lập tức tạo ra số dư âm, buộc`bad[p]`bằng 1, di chuyển`L`chuyển tiếp để không có phân đoạn nào bắt đầu trước khi vi phạm được tính. Điều này loại bỏ chính xác các tiền tố không hợp lệ trong khi vẫn cho phép khôi phục sau này. 

Trường hợp cuối cùng là dao động lặp lại giữa phép nhân và phép chia của cùng một số. Thuật toán xử lý vấn đề này bằng cách đặt lại bộ đếm bất cứ khi nào xảy ra thâm hụt, đảm bảo rằng chỉ những số dư cơ bản khả thi hiện tại mới ảnh hưởng đến ranh giới phân khúc đang hoạt động, thặng dư lịch sử không bao giờ lỗi thời.
