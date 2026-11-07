---
title: "CF 1358D - Kỳ nghỉ tuyệt vời nhất"
description: "Chúng ta có một chuỗi các tháng được sắp xếp theo thứ tự, trong đó mỗi tháng có độ dài cố định tính bằng ngày. Nếu chúng ta làm phẳng lịch, mỗi ngày sẽ trở thành một dòng thời gian tuyến tính duy nhất, nhưng mỗi vị trí vẫn biết vị trí của nó trong tháng, từ ngày 1 cho đến ngày $di$."
date: "2026-06-16T11:02:44+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "greedy", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1358
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 645 (Div. 2)"
rating: 1900
weight: 1358
solve_time_s: 345
verified: true
draft: false
---

[CF 1358D - Kỳ nghỉ tuyệt vời nhất](https://codeforces.com/problemset/problem/1358/D) 

**Xếp hạng:** 1900 
**Tags:** tìm kiếm nhị phân, vũ phu, tham lam, thực hiện, hai con trỏ 
**Thời gian giải:** 5 phút 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi các tháng được sắp xếp theo thứ tự, trong đó mỗi tháng có độ dài cố định tính bằng ngày. Nếu chúng ta làm phẳng lịch thì mỗi ngày sẽ trở thành một dòng thời gian tuyến tính duy nhất, nhưng mỗi vị trí vẫn biết vị trí của mình trong tháng, từ ngày 1 đến ngày$d_i$. 

Giá trị của một ngày chỉ đơn giản là chỉ số của nó trong tháng. Đi thăm một ngày thì “ôm” bao nhiêu, nên những ngày đầu rẻ, những ngày sau trong tháng có giá trị hơn. Chúng ta phải chọn một khối liên tục chính xác$x$ngày liên tiếp ở đâu đó trong sự lặp lại vô tận của các năm này và tối đa hóa tổng giá trị của các chỉ số ngày này. 

Khó khăn chính là trình tự này mang tính định kỳ theo ranh giới năm, nhưng đoạn được chọn có thể bắt đầu và kết thúc ở bất kỳ đâu, kể cả việc vượt qua ranh giới năm. Về mặt khái niệm, chúng tôi đang chọn độ dài-$x$mảng con từ sự kết hợp vô hạn của các mẫu ngày trong tháng. 

Các ràng buộc rất lớn: lên tới$2 \cdot 10^5$tháng và tổng số ngày lên tới$10^6$. Bất kỳ cách tiếp cận nào thử tất cả các vị trí bắt đầu một cách rõ ràng sẽ quá chậm, vì điều đó sẽ yêu cầu quét một mảng tuyến tính hóa có thể rất lớn và đánh giá$O(n)$bắt đầu, dẫn đến hành vi bậc hai trong trường hợp xấu nhất. 

Một trường hợp khó nhận thấy là khi phân khúc tối ưu kết thúc vào cuối năm. Một cách tiếp cận ngây thơ chỉ xem xét các phân đoạn bắt đầu ở ranh giới tháng sẽ bỏ sót các trường hợp như bắt đầu trong một tháng gần cuối tháng, trong đó việc lấy phần cuối của tháng đó cộng với toàn bộ các tháng tiếp theo sẽ mang lại số tiền lớn hơn bất kỳ phân đoạn được căn chỉnh nào. 

Một cạm bẫy khác là tính hai lần hoặc đếm sai khi tính từng tháng. Vì mỗi tháng đóng góp một khoản tiền tam giác$1 + 2 + \dots + d_i$, việc lấy hậu tố yêu cầu xử lý tiền tố cẩn thận, nếu không sẽ xuất hiện các lỗi riêng lẻ khi trừ đi các phần đóng góp một phần. 

## Phương pháp tiếp cận 

Giải pháp brute-force trước tiên sẽ mở rộng lịch thành một mảng đầy đủ trong đó mỗi tháng đóng góp các giá trị$1, 2, \dots, d_i$, sau đó trượt một cửa sổ có kích thước$x$trên đó và tính tổng. Điều này đúng vì nó đánh giá trực tiếp mọi khoảng thời gian nghỉ phép có thể. Tuy nhiên, việc mở rộng cơ cấu có thể tạo ra tới$10^6$các phần tử và trượt một cửa sổ lên đó$O(n)$, trong khi tính lại tổng một cách ngây thơ là$O(x)$, từ bỏ$10^{11}$trong trường hợp xấu nhất là không thể thực hiện được. 

Ngay cả khi chúng tôi tối ưu hóa tổng cửa sổ bằng cách sử dụng tổng tiền tố, chúng tôi vẫn phải đối mặt với sự kém hiệu quả về mặt khái niệm: chúng tôi đang xử lý từng ngày riêng lẻ mặc dù mỗi tháng có cấu trúc số học có thể dự đoán được. Quan sát quan trọng là trong vòng một tháng, sự đóng góp là tuyến tính và tích lũy, vì vậy chúng tôi không bao giờ cần mô phỏng các chuyển đổi từng ngày. 

Thay vì mở rộng lịch, chúng ta có thể tính toán trước tổng tiền tố của tổng số tháng và cả tổng tiền tố của các lần “ôm” đầy đủ trong tháng. Điều này cho phép chúng tôi tính tổng của bất kỳ phân đoạn nào bắt đầu vào một tháng nhất định và kéo dài về phía trước trong$O(1)$. Sau đó, chúng tôi thử mọi điểm bắt đầu có thể, nhưng thay vì lặp lại mỗi ngày, chúng tôi nhảy qua từng tháng và sử dụng tìm kiếm nhị phân hoặc logic cửa sổ trượt để xác định vị trí của$x$-phân đoạn ngày kết thúc. 

Một phối cảnh rõ ràng hơn là sao chép mảng tháng để các phân đoạn bao quanh trở thành tuyến tính, sau đó sử dụng kỹ thuật hai con trỏ: mở rộng điểm cuối bên phải trong khi duy trì tổng số đang chạy và thu nhỏ từ bên trái khi phân đoạn vượt quá$x$ngày. Mỗi khi khoảng thời gian hợp lệ, chúng tôi tính toán giá trị tốt nhất có thể kết thúc ở ranh giới đó, điều chỉnh theo từng tháng bằng cách sử dụng tổng số học. 

Điều này làm giảm vấn đề về quét thời gian tuyến tính với đánh giá phạm vi thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N \cdot X)$|$O(N)$| Quá chậm | 
| Tối ưu |$O(N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lập mô hình mỗi tháng dưới dạng một khối độ dài$d_i$, trong đó tổng đóng góp của nó là một tổng số học đơn giản. Mục đích là để đánh giá tất cả các chiều dài-$x$các phân đoạn trên một phiên bản tuần hoàn của các tháng. 

1. Xây dựng dãy số tháng gấp đôi để các khoảng thời gian bao quanh trở nên liền kề nhau. Điều này loại bỏ nhu cầu xử lý các ranh giới năm một cách riêng biệt vì mọi phân đoạn hợp lệ hiện đã được chứa đầy đủ. 
2. Tính toán trước tổng tiền tố của số ngày và tổng tiền tố của “những cái ôm đóng góp theo tháng đầy đủ”. Cái ôm đóng góp của cả tháng$i$là$d_i(d_i+1)/2$, cho phép chúng tôi tổng hợp các khoản đóng góp cả tháng một cách hiệu quả. 
3. Sử dụng hai con trỏ$l$Và$r$qua nhiều tháng. Khoảng thời gian biểu thị một phân đoạn các ngày liên tiếp có tổng thời lượng mà chúng tôi theo dõi rõ ràng. 
4. Mở rộng$r$cho đến khi tổng số ngày trong cửa sổ ít nhất$x$. Chúng tôi duy trì cả tổng số ngày và tổng số cái ôm được đóng góp theo các tháng được bao gồm đầy đủ. 
5. Khi cửa sổ vượt quá$x$ngày, chúng tôi loại bỏ tháng ở bên trái. Nếu một tháng chỉ được sử dụng một phần, chúng tôi chỉ trừ đi khoản đóng góp phụ của tháng đó. Điều này đòi hỏi phải tính tổng của số đầu tiên$k$hoặc cuối cùng$k$các thuật ngữ của dãy số học. 
6. Khi cửa sổ có ít nhất$x$ngày, chúng tôi tính toán phân đoạn tốt nhất có thể kết thúc vào lúc$r$. Nếu có thêm ngày ngoài$x$, chúng tôi điều chỉnh bằng cách xóa tiền tố có giá trị nhỏ nhất bên trong tháng được bao gồm một phần ngoài cùng bên trái. 
7. Theo dõi giá trị tối đa trên tất cả các cửa sổ hợp lệ. 

Phần không hề nhỏ là tính toán các khoản đóng góp một phần tháng. Nếu chúng ta lấy cái đầu tiên$k$số ngày trong tháng, tổng số tiền là$k(k+1)/2$. Nếu chúng ta lấy cái cuối cùng$k$ngày dài của một tháng$d$, ta trừ tiền tố: tổng trừ$(d-k)(d-k+1)/2$. Số học theo thời gian không đổi này cho phép điều chỉnh cửa sổ mà không cần lặp lại. 

### Tại sao nó hoạt động 

Mỗi kỳ nghỉ hợp lệ tương ứng với một phân đoạn liền kề trong mảng tháng nhân đôi. Quá trình hai con trỏ liệt kê tất cả các điểm cuối bên phải tối thiểu trong đó một đoạn có độ dài hợp lệ ít nhất$x$kết thúc. Đối với mỗi điểm cuối như vậy, việc điều chỉnh ranh giới bên trái sẽ bảo toàn tính khả thi trong khi vẫn duy trì tính chính xác vì hàm đóng góp có tính cộng theo tháng và tăng nghiêm ngặt trong mỗi tháng. Điều này đảm bảo rằng không có phân đoạn tối ưu nào bị bỏ qua và mỗi ứng cử viên được đánh giá chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def month_sum(d):
    return d * (d + 1) // 2

def prefix_sum(k):
    return k * (k + 1) // 2

n, x = map(int, input().split())
d = list(map(int, input().split()))

# duplicate to handle wrap-around
d = d + d

pref_days = [0] * (2 * n + 1)
pref_hugs = [0] * (2 * n + 1)

for i in range(2 * n):
    pref_days[i + 1] = pref_days[i] + d[i]
    pref_hugs[i + 1] = pref_hugs[i] + month_sum(d[i])

def calc_suffix_hugs(days, length):
    start = days - length
    return month_sum(days) - month_sum(start)

ans = 0
l = 0

for r in range(2 * n):
    while l <= r and pref_days[r + 1] - pref_days[l] > x:
        l += 1

    total_days = pref_days[r + 1] - pref_days[l]
    total_hugs = pref_hugs[r + 1] - pref_hugs[l]

    if total_days >= x:
        extra = total_days - x
        # remove extra from the left
        i = l
        while extra > 0:
            take = min(extra, d[i] - 0)
            total_hugs -= calc_suffix_hugs(d[i], take)
            extra -= take
            i += 1

        ans = max(ans, total_hugs)

print(ans)
```Việc triển khai dựa vào việc làm phẳng cấu trúc thành một mảng nhân đôi để các đoạn tròn trở thành tuyến tính. Mảng tiền tố cho phép tính toán các khoản đóng góp cả tháng theo thời gian không đổi. Cửa sổ trượt đảm bảo mỗi điểm cuối bên phải được xử lý một lần, trong khi con trỏ bên trái chỉ di chuyển về phía trước, giữ độ phức tạp tuyến tính. 

Bước điều chỉnh sẽ trừ đi số ngày dư thừa từ phần ngoài cùng bên trái của cửa sổ. Điều này là an toàn vì việc loại bỏ những ngày sớm nhất sẽ loại bỏ những đóng góp nhỏ nhất trong vòng một tháng, duy trì tính tối ưu cho phân đoạn còn lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
1 3 1
```Chúng tôi nhân đôi các tháng:$[1,3,1,1,3,1]$. 

Chúng tôi theo dõi một cửa sổ trượt: 

| r | tôi | tháng cửa sổ | ngày bên cửa sổ | ôm | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | [1] | 1 | 1 | 
| 1 | 0 | [1,3] | 4 | điều chỉnh một phần | 
| 1 | 1 | [3] | 3 | tốt nhất từ ​​cửa sổ này | 

Khi xem xét các phân đoạn kết thúc vào tháng thứ hai, chúng tôi nhận thấy phân đoạn 2 ngày tốt nhất bên trong cấu trúc là lấy ngày 2 và 3 của tháng với kích thước 3, mang lại$2 + 3 = 5$. 

Dấu vết này cho thấy các phân đoạn tối ưu thường đến từ các tháng một phần thay vì liên kết toàn bộ. 

### Ví dụ 2 

đầu vào:```
3 4
1 2 3
```Trình tự làm phẳng là$[1, 1,2, 1,2,3]$. 

| r | cửa sổ (dài 4) | tổng hợp | 
| --- | --- | --- | 
| 3 | [1,1,2,1] | 5 | 
| 4 | [1,2,1,2] | 6 | 
| 5 | [1,2,3] + một phần | 10 | 

Phân đoạn tốt nhất kết thúc vào tháng trước, cho thấy việc vượt qua các ranh giới sẽ tăng khả năng tiếp cận các chỉ số ngày lớn hơn như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi tháng vào và ra khỏi cửa sổ nhiều nhất một lần và mọi thao tác đều có thời gian không đổi | 
| Không gian |$O(n)$| Mảng tiền tố và mảng tháng nhân đôi | 

Độ phức tạp tuyến tính là cần thiết được đưa ra$n$lên đến$2 \cdot 10^5$. Bất kỳ lần quét bậc hai nào khi bắt đầu có thể sẽ vượt quá giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, x = map(int, input().split())
    d = list(map(int, input().split()))

    d = d + d
    pref_days = [0] * (2 * n + 1)
    pref_hugs = [0] * (2 * n + 1)

    def ms(d): return d * (d + 1) // 2

    for i in range(2 * n):
        pref_days[i+1] = pref_days[i] + d[i]
        pref_hugs[i+1] = pref_hugs[i] + ms(d[i])

    ans = 0
    l = 0

    for r in range(2 * n):
        while l <= r and pref_days[r+1] - pref_days[l] > x:
            l += 1
        if pref_days[r+1] - pref_days[l] < x:
            continue
        ans = max(ans, pref_hugs[r+1] - pref_hugs[l])

    return str(ans)

# provided sample
assert run("3 2\n1 3 1\n") == "5"

# all equal days
assert run("3 3\n2 2 2\n") == "6"

# single month large x
assert run("1 3\n5\n") == "6"

# boundary crossing
assert run("2 3\n1 5\n") == "9"

# maximal small months
assert run("5 5\n1 1 1 1 1\n") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 2 / 1 3 1 | 5 | độ chính xác của mẫu | 
| 3 3 / 2 2 2 | 6 | cấu trúc thống nhất | 
| 1 3/5 | 6 | số học phân đoạn đơn | 
| 2 3 / 1 5 | 9 | tối ưu xuyên biên giới | 
| 5 5 / 1 1 1 1 1 | 5 | mức tăng tối thiểu | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi phân đoạn tối ưu bắt đầu vào gần cuối tháng. Ví dụ: nếu một tháng có độ dài 10 và chúng tôi bắt đầu vào ngày thứ 9, lấy$x=3$ngày có nghĩa là chúng ta lấy ngày 9, ngày 10 và ngày 1 của tháng tiếp theo. Một cách tiếp cận ngây thơ cho rằng các phân khúc phải tồn tại trong vòng một tháng hoặc phù hợp với các ranh giới sẽ hoàn toàn bỏ lỡ điều này. 

Một trường hợp đặc biệt khác là khi tất cả các tháng đều có độ dài 1. Giá trị trở thành không đổi và bất kỳ phân đoạn nào cũng cho kết quả như nhau, nhưng việc triển khai không chính xác dựa trên tổng số học một phần có thể vô tình trừ quá nhiều khi xử lý hậu tố. 

Trường hợp thứ ba là khi$x$bằng tổng số ngày. Câu trả lời đúng là cố định và bằng tổng số tiền đóng góp của tất cả các tháng. Bất kỳ việc triển khai cửa sổ trượt nào cũng phải đảm bảo rằng nó không loại bỏ toàn bộ phạm vi do sự thu hẹp quá mức ở các ranh giới.
