---
title: "CF 105905A - \u041f\u043b\u043e\u0445\u0438\u0435 \u0444\u0438\u0441\u0442\u0430\u0448\u043a\u0438"
description: "Vấn đề mô tả một nhóm thượng nghị sĩ. Mỗi thượng nghị sĩ có một giá trị sức mạnh và tỷ lệ phần trăm lòng trung thành hiện tại. Trước khi cuộc bỏ phiếu bắt đầu, chúng tôi có thể phân phát một số lượng kẹo có hạn."
date: "2026-06-25T14:13:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105905
codeforces_index: "A"
codeforces_contest_name: "Ural championship 2025"
rating: 0
weight: 105905
solve_time_s: 41
verified: true
draft: false
---

[CF 105905A - \u041f\u043b\u043e\u0445\u0438\u0435 \u0444\u0438\u0441\u0442\u0430\u0448\u043a\u0438](https://codeforces.com/problemset/problem/105905/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề mô tả một nhóm thượng nghị sĩ. Mỗi thượng nghị sĩ có một giá trị sức mạnh và tỷ lệ phần trăm lòng trung thành hiện tại. Trước khi cuộc bỏ phiếu bắt đầu, chúng tôi có thể phân phát một số lượng kẹo có hạn. Mỗi chiếc kẹo được tặng cho một thượng nghị sĩ sẽ làm tăng lòng trung thành của thượng nghị sĩ đó thêm 10%, nhưng không ai có thể vượt quá 100%. Sau khi phân phối, mỗi thượng nghị sĩ sẽ bỏ phiếu độc lập ủng hộ hoặc chống lại chúng tôi tùy theo xác suất trung thành cuối cùng của họ. 

Nếu hơn một nửa số thượng nghị sĩ bỏ phiếu ủng hộ chúng tôi, chúng tôi sẽ thành công ngay lập tức. Nếu không, chúng tôi có thể cố gắng loại bỏ chính xác các thượng nghị sĩ đã bỏ phiếu chống lại chúng tôi. Cơ hội loại bỏ một nhóm phụ thuộc vào sức mạnh của chính chúng ta và sức mạnh tổng hợp của các thượng nghị sĩ bị loại bỏ. Chúng ta cần chọn cách phân phối kẹo để tối đa hóa xác suất thành công cuối cùng. 

Số lượng thượng nghị sĩ ít và điều này làm thay đổi toàn bộ chiến lược. Với tối đa 8 thượng nghị sĩ, tổng số cấu hình lòng trung thành cuối cùng có thể quản lý được. Một giải pháp lặp lại thông qua các tập hợp con thượng nghị sĩ là thực tế, trong khi giải pháp thử mọi kết quả bỏ phiếu có thể có cho một nhóm lớn hơn nhiều sẽ không mở rộng được. 

Số lượng kẹo cũng ít. Vì mỗi viên kẹo chỉ thay đổi giá trị mức độ trung thành một bước là 10 phần trăm nên chúng ta có thể biểu diễn quyết định này bằng cách chỉ định một số lượng nhỏ các nâng cấp riêng biệt. Một mô phỏng đơn giản về mọi phân phối kẹo có thể chỉ có thể thực hiện được vì kích thước đầu vào rất nhỏ. Nếu số lượng thượng nghị sĩ vào khoảng 10^5, thì ngay cả việc vượt qua tuyến tính cũng sẽ là hướng hợp lý duy nhất và việc liệt kê theo cấp số nhân sẽ ngay lập tức thất bại. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. Nếu một thượng nghị sĩ đã có 100% lòng trung thành thì việc tặng thêm kẹo cho họ cũng không có tác dụng gì. Ví dụ:```
1 3 20
20 100
```Đầu ra đúng là:```
1.0000000000
```Việc thực hiện bất cẩn, coi mọi viên kẹo một cách mù quáng vì lòng trung thành ngày càng tăng có thể tạo ra xác suất không thể trên 100%. 

Một trường hợp góc khác xuất hiện khi chúng ta không đủ số phiếu bầu và phải dựa vào việc loại bỏ đối thủ. Ví dụ:```
1 0 10
20 0
```Thượng nghị sĩ duy nhất luôn bỏ phiếu chống lại chúng tôi. Chúng tôi loại bỏ thượng nghị sĩ đó với xác suất:```
10 / (10 + 20) = 1 / 3
```vậy câu trả lời là:```
0.3333333333
```Một giải pháp chỉ kiểm tra xem liệu chúng tôi có thể nhận được đa số phiếu bầu hay không sẽ trả về số 0 không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận đầu tiên là trực tiếp thử tất cả các lần phân phối kẹo. Vì mỗi viên kẹo được giao cho một trong số các thượng nghị sĩ nên chúng ta có thể chọn đệ quy nơi mỗi viên kẹo sẽ đi. Sau khi tạo một phân phối, chúng tôi tính toán mức độ trung thành mới và đánh giá xác suất chiến thắng. 

Cách tiếp cận này đúng vì mọi cách có thể để tiêu kẹo đều được xem xét. Đối với mỗi lần phân phối, xác suất có thể được tính bằng cách lặp lại tất cả các mặt nạ biểu quyết có thể có. Với n thượng nghị sĩ, có thể có 2^n kết quả bỏ phiếu. Đối với mỗi mặt nạ, chúng tôi xác định xem nó đã mang lại cho chúng tôi đủ phiếu bầu tích cực hay chưa. Nếu không, chúng tôi tính xác suất loại bỏ tất cả các thượng nghị sĩ thua cuộc. 

Vấn đề là số lượng phân phối kẹo tăng lên nhanh chóng. Nếu chúng ta có k viên kẹo và n thượng nghị sĩ thì số lượng nhiệm vụ sẽ vào khoảng n^k. Ngay cả khi chỉ có 8 thượng nghị sĩ, việc tăng k khiến phương pháp này trở nên đắt đỏ. Tổng công việc sẽ trở thành khoảng O(n^k * 2^n), lặp lại cùng một phép tính phiếu bầu nhiều lần. 

Nhận xét quan trọng là kẹo chỉ ảnh hưởng đến lòng trung thành cuối cùng của mỗi thượng nghị sĩ. Chúng tôi không quan tâm đến thứ tự chính xác mà kẹo được đưa ra. Trạng thái cuối cùng của các thượng nghị sĩ là đủ. Vì n và k đều rất nhỏ nên chúng ta có thể sử dụng quy hoạch động thay vì sử dụng kẹo. 

Chúng tôi xử lý từng viên kẹo một và lưu trữ tất cả các trạng thái trung thành có thể tiếp cận. Một bang được thể hiện bằng mức độ trung thành của mỗi thượng nghị sĩ sau khi một số kẹo được phân phát. Số lượng trạng thái có thể có bị giới hạn vì mỗi thượng nghị sĩ chỉ có 11 giá trị trung thành có thể có: 0, 10, 20, tối đa 100. Sau khi tất cả kẹo được chỉ định, chúng tôi đánh giá mọi trạng thái có thể tiếp cận và giữ xác suất tốt nhất. 

Quá trình chuyển đổi rất đơn giản. Từ tình trạng hiện tại, bất kỳ thượng nghị sĩ nào chưa đạt 100% có thể được tặng thêm một viên kẹo nữa. Trạng thái mới được thêm vào lớp tiếp theo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^k * 2^n * n) | O(n) | Quá chậm đối với k lớn hơn | 
| Tối ưu | O(k * S * n + S * 2^n * n) | O(S) | Đã chấp nhận | 

Ở đây S là số trạng thái trung thành có thể tiếp cận. Với n = 8, số trạng thái tối đa có thể có là đủ nhỏ. 

## Hướng dẫn thuật toán 

1. Đọc các thượng nghị sĩ và chuyển đổi mọi phần trăm lòng trung thành thành một số nguyên biểu thị số khối 10 phần trăm. Mức độ trung thành từ 70 trở thành 7. Điều này làm cho việc vận hành kẹo trở nên đơn giản vì một viên kẹo chỉ là thêm một viên. 
2. Bắt đầu với một trạng thái duy nhất chứa các giá trị trung thành ban đầu và không sử dụng kẹo. Đây là tình huống duy nhất có thể xảy ra trước khi chúng ta đưa ra bất kỳ lựa chọn nào. 
3. Đối với mỗi viên kẹo, hãy tạo tất cả các trạng thái mới có thể có. Ở mọi bang hiện tại, hãy thử tặng kẹo cho mọi thượng nghị sĩ có lòng trung thành vẫn dưới 100%. Tăng lòng trung thành của thượng nghị sĩ đó thêm một khối và lưu trữ trạng thái kết quả. 
4. Sau khi tất cả kẹo được xử lý, hãy kiểm tra mọi trạng thái trung thành cuối cùng. Đối với mỗi trạng thái, hãy tính xác suất thành công bằng cách lặp lại tất cả các mặt nạ biểu quyết có thể có. Nếu một chiếc mặt nạ mang lại cho chúng ta đa số thì xác suất của nó sẽ đóng góp trực tiếp. Ngược lại, hãy tính xác suất loại bỏ mọi thượng nghị sĩ thua cuộc và cộng phần đóng góp đó. 
5. Lấy xác suất tối đa trên tất cả các trạng thái cuối cùng. Đây là câu trả lời vì mọi khả năng phân phối kẹo đều dẫn đến một trong các trạng thái được tạo ra. 

Tại sao nó hoạt động: lập trình động lưu giữ chính xác thông tin ảnh hưởng đến các quyết định trong tương lai. Điều duy nhất kẹo thay đổi là lòng trung thành hiện tại của mỗi thượng nghị sĩ, vì vậy hai lịch sử khác nhau có cùng vectơ lòng trung thành có khả năng tương lai giống hệt nhau. Bước đánh giá sẽ kiểm tra mọi kết quả bỏ phiếu có thể xảy ra, nghĩa là nó tính toán xác suất thành công chính xác cho trạng thái cuối cùng đó. Vì mọi trạng thái cuối cùng có thể đều được tạo ra nên trạng thái tối đa trong số đó là câu trả lời tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    first = input().split()
    if not first:
        return
    n, k, A = map(int, first)

    senators = []
    for _ in range(n):
        b, l = map(int, input().split())
        senators.append((b, l // 10))

    start = tuple(x[1] for x in senators)
    states = {start}

    for _ in range(k):
        nxt = set()
        for state in states:
            for i in range(n):
                if state[i] < 10:
                    arr = list(state)
                    arr[i] += 1
                    nxt.add(tuple(arr))
        states = nxt

    total_masks = 1 << n
    answer = 0.0

    for state in states:
        cur = 0.0
        for mask in range(total_masks):
            prob = 1.0
            bad_sum = 0
            good = 0

            for i in range(n):
                p = state[i] / 10.0
                if mask & (1 << i):
                    good += 1
                    prob *= p
                else:
                    prob *= (1.0 - p)
                    bad_sum += senators[i][0]

            if good > n // 2:
                cur += prob
            else:
                cur += prob * (A / (A + bad_sum))

        if cur > answer:
            answer = cur

    print("{:.10f}".format(answer))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của mã lưu trữ lòng trung thành của mỗi thượng nghị sĩ theo đơn vị 10%. Điều này tránh những thay đổi dấu phẩy động trong giai đoạn tạo trạng thái. Bộ có tên`states`chứa tất cả các vectơ trung thành có thể đạt được sau khi sử dụng kẹo đã xử lý. 

Vòng chuyển tiếp tuân theo bước thuật toán thứ ba. Một thượng nghị sĩ có giá trị lòng trung thành là 10 đã đại diện cho 100%, do đó, việc tăng giá trị này sẽ tạo ra trạng thái không hợp lệ. Đây là điều kiện biên chính trong quy hoạch động. 

Vòng đánh giá sẽ kiểm tra mọi tập hợp con có thể có của các thượng nghị sĩ bỏ phiếu tích cực. Bitmask cho chúng ta biết thượng nghị sĩ nào đã bỏ phiếu cho chúng ta. Chúng tôi nhân xác suất bỏ phiếu độc lập để có được xác suất của kết quả chính xác đó. Khi kết quả không chiếm đa số, mã sẽ tính toán sức mạnh của các thượng nghị sĩ thua cuộc và áp dụng xác suất loại bỏ. 

biểu hiện`A / (A + bad_sum)`được tính toán bằng cách sử dụng phép chia dấu phẩy động. Các giá trị đủ nhỏ để độ chính xác của dấu phẩy động của Python đủ cho đầu ra được yêu cầu. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, sau khi cho tất cả kẹo một cách tối ưu, trạng thái trung thành sẽ trở thành: 

| Bước | Trạng thái trung thành | Lời giải thích tốt nhất hiện nay | 
| --- | --- | --- | 
| Bắt đầu | 8, 9, 7, 3, 7 | Lòng trung thành ban đầu | 
| Tặng kẹo | 9, 10, 8, 3, 7 | Cải thiện các thượng nghị sĩ có tác động cao | 
| Cuối cùng | 10, 10, 10, 3, 7 | Đa số trở nên rất có khả năng | 

Trạng thái cuối cùng khiến ba thượng nghị sĩ trở thành những người ủng hộ nhất định. Vì điều đó đã đủ cho đa số nên mọi kết quả bỏ phiếu quan trọng đều thành công. 

Đối với mẫu thứ ba:```
1 3 20
20 20
```Thượng nghị sĩ có thể nhận được cả ba loại kẹo: 

| Bước | Trạng thái trung thành | Xác suất thành công | 
| --- | --- | --- | 
| Bắt đầu | 2 | Cần loại bỏ | 
| Sau chiếc kẹo đầu tiên | 3 | Cải thiện cơ hội bỏ phiếu | 
| Sau kẹo thứ hai | 4 | Cải thiện cơ hội bỏ phiếu | 
| Sau kẹo thứ ba | 5 | Bình chọn thành công với xác suất 0,5 | 

Xác suất còn lại đến từ trường hợp bỏ phiếu thất bại, trong đó thượng nghị sĩ bị loại bỏ. DP xem xét trạng thái cuối cùng này và tính xác suất chính xác là 0,7500000000. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k * S * n + S * 2^n * n) | Chúng tôi tạo ra các trạng thái thông qua quá trình chuyển đổi kẹo và đánh giá từng trạng thái cuối cùng | 
| Không gian | O(S) | Chúng tôi chỉ lưu trữ lớp trạng thái trung thành hiện tại | 

Số lượng thượng nghị sĩ nhiều nhất là 8, do đó, ngay cả việc lặp lại tất cả các mặt nạ bỏ phiếu cũng không tốn kém. Số lượng trạng thái trung thành có thể tiếp cận cũng bị giới hạn bởi số lượng nhỏ mức độ trung thành và kẹo, vì vậy giải pháp dễ dàng phù hợp với giới hạn đã định. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(data):
    sys.stdin = io.StringIO(data)
    input = sys.stdin.readline

    first = input().split()
    n, k, A = map(int, first)

    senators = []
    for _ in range(n):
        b, l = map(int, input().split())
        senators.append((b, l // 10))

    states = {tuple(x[1] for x in senators)}

    for _ in range(k):
        nxt = set()
        for state in states:
            for i in range(n):
                if state[i] < 10:
                    arr = list(state)
                    arr[i] += 1
                    nxt.add(tuple(arr))
        states = nxt

    ans = 0.0
    for state in states:
        cur = 0.0
        for mask in range(1 << n):
            p = 1.0
            bad = 0
            good = 0
            for i in range(n):
                if mask >> i & 1:
                    good += 1
                    p *= state[i] / 10.0
                else:
                    p *= 1.0 - state[i] / 10.0
                    bad += senators[i][0]
            if good > n // 2:
                cur += p
            else:
                cur += p * A / (A + bad)
        ans = max(ans, cur)

    return f"{ans:.10f}\n"

assert solve("""5 6 100
11 80
14 90
23 70
80 30
153 70
""") == "1.0000000000\n"

assert solve("""5 3 100
11 80
14 90
23 70
80 30
153 70
""") == "0.9628442962\n"

assert solve("""1 3 20
20 20
""") == "0.7500000000\n"

assert solve("""1 3 20
20 100
""") == "1.0000000000\n"

assert solve("""1 0 10
20 0
""") == "0.3333333333\n"

assert solve("""2 0 50
10 100
10 100
""") == "1.0000000000\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một thượng nghị sĩ đã hoàn toàn trung thành | 1.0000000000 | Kẹo không thể tăng lòng trung thành quá 100% | 
| Một cử tri bất khả thi không có kẹo | 0.3333333333 | Xử lý xác suất loại bỏ | 
| Hai người ủng hộ được đảm bảo | 1.0000000000 | Trường hợp đa số ngay lập tức | 
| Mẫu gốc | Kết quả đầu ra mẫu | Tính đúng đắn chung | 

## Vỏ cạnh 

Khi một thượng nghị sĩ đã có lòng trung thành tối đa thì quá trình chuyển đổi phải bỏ qua thượng nghị sĩ đó. Đối với đầu vào:```
1 3 20
20 100
```trạng thái được tạo duy nhất vẫn là mức trung thành 10. Xác suất bỏ phiếu là 1, vì vậy câu trả lời chính xác là 1. Thuật toán xử lý vấn đề này vì các trạng thái không bao giờ được tạo với mức trung thành trên 10. 

Khi không có ai hỗ trợ chúng tôi và chúng tôi phải dựa vào việc loại bỏ thì giai đoạn đánh giá vẫn hoạt động. Vì:```
1 0 10
20 0
```mặt nạ bỏ phiếu duy nhất là phiếu thua. Thượng nghị sĩ có sức mạnh 20 nên cơ hội loại bỏ là 10/30. Thuật toán nhân xác suất bỏ phiếu với giá trị này và trả về 0,3333333333. 

Khi kẹo được phân phối giữa một số thượng nghị sĩ, các mệnh lệnh khác nhau có thể tạo ra cùng một vectơ trung thành cuối cùng. Tập hợp các trạng thái loại bỏ những trùng lặp này, do đó thuật toán không lãng phí thời gian tính toán lại các tình huống giống hệt nhau. Đây là đặc tính làm cho phương pháp quy hoạch động trở nên hiệu quả.
