---
title: "CF 106369E - Pez có giá trị nhất"
description: "Vấn đề mô hình một bộ sưu tập máy phân phối Pez. Mỗi bộ phân phối chứa chính xác 12 viên kẹo được sắp xếp từ trên xuống dưới và một bộ phân phối chỉ có thể được ăn từ trên xuống."
date: "2026-06-25T08:19:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106369
codeforces_index: "E"
codeforces_contest_name: "2023 UCF Local Programming Contest"
rating: 0
weight: 106369
solve_time_s: 38
verified: true
draft: false
---

[CF 106369E - Pez có giá trị nhất](https://codeforces.com/problemset/problem/106369/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề mô hình một bộ sưu tập máy phân phối Pez. Mỗi bộ phân phối chứa chính xác 12 viên kẹo được sắp xếp từ trên xuống dưới và một bộ phân phối chỉ có thể được ăn từ trên xuống. Nếu bạn quyết định lấy một số kẹo từ một bộ phân phối, bạn phải lấy tiền tố liền kề của bộ phân phối đó. Bạn được phép chia các lựa chọn của mình cho tất cả các máy phân phối và mục tiêu là ăn chính xác`k`kẹo đồng thời tối đa hóa tổng giá trị của số kẹo đã ăn. 

Đầu vào cung cấp số lượng bộ phân phối và số lượng kẹo phải ăn, theo sau là các giá trị bên trong mỗi bộ phân phối từ trên xuống dưới. Đầu ra là tổng giá trị lớn nhất có thể đạt được bằng cách chọn các tiền tố hợp lệ có tổng độ dài chính xác`k`. 

Hạn chế quan trọng là có thể có tới 1000 máy phân phối nhưng mỗi máy phân phối chỉ có 12 viên kẹo. Từ`k`có thể lớn như`12000`, các giải pháp thử mọi lựa chọn kẹo có thể sẽ bùng nổ. Một lực lượng vũ phu đối với tất cả các lựa chọn sẽ xem xét nhiều tổ hợp tiền tố và vượt xa giới hạn 2 giây cho phép. Chúng ta cần một cái gì đó gần gũi`n * k`hoặc`n * k * small_constant`. 

Các trường hợp phức tạp là do hạn chế tiền tố. Chiến lược tham lam liên tục lấy viên kẹo lớn nhất hiện tại sẽ thất bại vì lấy một viên kẹo nhỏ có thể làm lộ ra viên kẹo lớn hơn nhiều bên dưới. Ví dụ:```
Input
2 2
1 100 1 1 1 1 1 1 1 1 1 1
50 1 1 1 1 1 1 1 1 1 1 1
```Đầu ra đúng là:```
101
```Một cách tiếp cận tham lam bất cẩn cần`50`đầu tiên, sau đó`1`, tổng cộng là`51`. Lựa chọn tối ưu là lấy hai viên kẹo từ máy phân phối đầu tiên, chấp nhận giá trị`1`để mở khóa`100`. 

Một trường hợp nguy hiểm khác là khi tất cả các loại kẹo hữu ích đều nằm sâu trong bộ phân phối. Ví dụ:```
Input
1 3
1 1 100 1 1 1 1 1 1 1 1 1
```Đầu ra đúng là:```
102
```Chỉ lấy số kẹo tối đa có thể nhìn thấy không thành công vì`100`không thể được thu thập một mình. Cần có viên kẹo thứ ba và độ dài tiền tố rất quan trọng. 

Trường hợp ranh giới cuối cùng là khi`k`bằng tổng số kẹo:```
Input
1 12
5 4 3 2 1 6 7 8 9 10 11 12
```Câu trả lời là tổng của tất cả các giá trị:```
78
```Bất kỳ cách tiếp cận nào cho rằng nó có thể dừng sớm hoặc chỉ lấy được một số viên kẹo tốt nhất sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Cách trực tiếp để giải quyết vấn đề là liệt kê xem chúng ta lấy được bao nhiêu viên kẹo từ mỗi hộp phân phối. Đối với mỗi bộ phân phối, chúng tôi có 13 lựa chọn: lấy 0 viên kẹo, lấy một viên kẹo, v.v. cho đến hết 12 viên kẹo. Việc thử tất cả các kết hợp của những lựa chọn này là đúng vì mỗi lựa chọn cuối cùng hợp lệ đều tương ứng với chính xác một lựa chọn cho mỗi bộ phân phối. Tuy nhiên, số lượng khả năng là`13^n`, điều này là rất lớn ngay cả đối với vài chục máy phân phối và hoàn toàn không thể khi`n`đạt 1000. 

Cấu trúc của bài toán cho chúng ta một không gian trạng thái nhỏ hơn. Chúng tôi không quan tâm máy phân phối nào đã sản xuất ra số kẹo đã được ăn. Chúng tôi chỉ quan tâm đến số lượng kẹo đã được lấy cho đến nay và giá trị tối đa có thể đạt được với số lượng đó. Vì mỗi bộ phân phối chỉ đóng góp từ 0 đến 12 viên kẹo nên chúng tôi có thể xử lý từng bộ phân phối một lần và cập nhật những giá trị tốt nhất này. 

Đối với mỗi bộ phân phối, chúng tôi tính toán trước giá trị của mọi tiền tố có thể. Sau đó, quá trình chuyển đổi rất đơn giản: nếu hiện tại chúng tôi có giải pháp sử dụng`j`kẹo, chúng ta có thể thêm tiền tố có độ dài`x`từ bộ phân phối hiện tại và tiếp cận`j + x`kẹo. Số lượng lựa chọn trên mỗi bộ phân phối chỉ là 13, do đó lập trình động đủ nhanh. 

Lực lượng vũ phu hoạt động vì nó khám phá mọi cách phân chia kẹo có thể có giữa các máy phân phối, nhưng nó không thành công vì số lượng phân chia theo cấp số nhân. Nhận xét rằng chỉ số lượng kẹo đã được chọn mới là quan trọng cho phép chúng tôi nén tất cả các quyết định trước đó vào mảng DP một chiều. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(13^n) | O(n) | Quá chậm | 
| Tối ưu | O(n * k * 12) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng tiền tố cho mỗi bộ phân phối Pez. Giá trị tại vị trí`x`đại diện cho tổng giá trị thu được bằng cách lấy giá trị đầu tiên`x`kẹo từ máy phân phối đó. Vị trí số 0 có giá trị bằng 0 vì luôn không được phép lấy gì. 
2. Tạo một mảng lập trình động trong đó`dp[j]`lưu trữ giá trị tối đa có thể đạt được sau khi xử lý một số bộ phân phối và ăn chính xác`j`kẹo. Ban đầu chỉ`dp[0]`là hợp lệ vì chúng tôi chưa lấy bất cứ thứ gì. 
3. Xử lý từng bộ phân phối một cách độc lập. Đối với mọi số kẹo có thể có trước đó`j`, hãy thử mọi độ dài tiền tố`x`từ 0 đến 12. Nếu dùng`x`kẹo từ bộ phân phối hiện tại không vượt quá`k`, cập nhật trạng thái cho`j + x`. 
4. Sau khi tất cả các bộ phân phối được xử lý, câu trả lời là`dp[k]`bởi vì trạng thái đại diện cho giá trị tốt nhất cho chính xác`k`kẹo. 

Lý do điều này có hiệu quả là vì mỗi bộ phân phối chỉ ảnh hưởng đến câu trả lời thông qua số lượng kẹo mà nó đóng góp và giá trị của tiền tố đó. Sau khi bộ phân phối được xử lý, tất cả thông tin chi tiết về cách chọn những viên kẹo đó có thể bị lãng quên vì những bộ phân phối trong tương lai chỉ quan tâm đến số lượng kẹo còn lại có sẵn. 

Tại sao nó hoạt động: Bất biến DP là sau khi xử lý lần đầu tiên`i`máy rút,`dp[j]`là giá trị tốt nhất có thể bằng cách sử dụng chính xác`j`kẹo từ những máy pha chế đó. Khi xử lý bộ phân phối tiếp theo, mọi giải pháp hợp lệ sẽ lấy một số tiền tố từ bộ phân phối này hoặc không lấy gì cả. Quá trình chuyển đổi sẽ thử mọi khả năng đó, vì vậy mọi giải pháp hợp lệ vẫn có thể truy cập được và giá trị tối đa được giữ nguyên. Sau bộ phân phối cuối cùng, bất biến đưa ra câu trả lời tối ưu cho`k`kẹo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())

    dp = [-1] * (k + 1)
    dp[0] = 0

    for _ in range(n):
        values = list(map(int, input().split()))
        pref = [0] * 13

        for i, value in enumerate(values):
            pref[i + 1] = pref[i] + value

        ndp = [-1] * (k + 1)

        for used in range(k + 1):
            if dp[used] == -1:
                continue
            limit = min(12, k - used)
            for take in range(limit + 1):
                ndp[used + take] = max(
                    ndp[used + take],
                    dp[used] + pref[take]
                )

        dp = ndp

    print(dp[k])

if __name__ == "__main__":
    solve()
```Mã chỉ giữ lại một lớp DP vì bộ phân phối hiện tại phải được sử dụng hoặc không được sử dụng chính xác một lần. Cần có mảng thứ hai để tránh kết hợp cùng một bộ phân phối với chính nó trong quá trình cập nhật. 

Mảng tiền tố có độ dài 13 vì bộ phân phối có chính xác 12 viên kẹo.`pref[take]`là sự góp phần của việc lựa chọn được nhiều kẹo như vậy từ các máy phân phối hiện tại. 

Quá trình chuyển đổi bỏ qua các trạng thái không thể xảy ra khi`dp[used]`vẫn còn`-1`. Giới hạn trên của`take`được giảm bớt số lượng kẹo còn lại cần thiết, điều này giúp tránh những công việc không cần thiết và tránh các trạng thái lập chỉ mục vượt quá`k`. 

Không có mối lo ngại về tràn trong Python. Trong các ngôn ngữ có kích thước số nguyên cố định, câu trả lời tối đa vẫn đủ nhỏ cho số nguyên 32 bit tiêu chuẩn, nhưng việc sử dụng loại lớn hơn là vô hại. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 7
3 6 8 2 2 5 6 1 2 3 1 4
4 1 1 1 1 70 4 1 5 3 2 3
5 3 2 6 6 2 3 1 6 2 3 2
1 4 3 8 4 9 2 1 6 5 4 1
```Các trạng thái DP quan trọng sau khi xử lý từng bộ phân phối là: 

| Máy phân phối | Chuyển tiếp chính | Giá trị tốt nhất cho 7 viên kẹo | 
| --- | --- | --- | 
| 1 | Xây dựng tất cả các tiền tố từ ngăn xếp đầu tiên | Không thể | 
| 2 | Kết hợp các tiền tố từ hai ngăn xếp đầu tiên | 50 | 
| 3 | Xét giá trị lớn 70 sau khi lấy 5 viên kẹo | 83 | 
| 4 | Kiểm tra sự kết hợp cuối cùng | 83 | 

Dấu vết cho thấy tại sao DP lại cần thiết. Câu trả lời hay nhất yêu cầu phải lấy vài viên kẹo có giá trị thấp trước để mở khóa`70`. 

### Mẫu 2 

đầu vào:```
1 6
20 30 40 15 1 14 2 2 2 2 2 2
```| Máy phân phối | Độ dài tiền tố được xem xét | Giá trị | 
| --- | --- | --- | 
| 1 | 0 | 0 | 
| 1 | 1 | 20 | 
| 1 | 2 | 50 | 
| 1 | 3 | 90 | 
| 1 | 4 | 105 | 
| 1 | 5 | 106 | 
| 1 | 6 | 120 | 

Câu trả lời là tiền tố có độ dài 6 vì chỉ có một bộ phân phối và số lượng kẹo cần thiết phải đến từ chuỗi đơn đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * k * 12) | Mỗi bộ phân phối cập nhật mọi số lượng kẹo bằng cách sử dụng tối đa 13 lựa chọn tiền tố | 
| Không gian | O(k) | Chỉ cần thông tin lớp DP hiện tại và trước đó | 

Với`n = 1000`Và`k = 12000`, số lần chuyển đổi trong trường hợp xấu nhất là khoảng 156 triệu. Hệ số không đổi nhỏ vì mỗi bộ phân phối chỉ có 12 độ dài tiền tố có thể có, do đó lập trình động phù hợp với giới hạn đã định. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    
    n, k = map(int, input().split())
    dp = [-1] * (k + 1)
    dp[0] = 0

    for _ in range(n):
        a = list(map(int, input().split()))
        pref = [0] * 13
        for i, x in enumerate(a):
            pref[i + 1] = pref[i] + x

        ndp = [-1] * (k + 1)
        for used in range(k + 1):
            if dp[used] == -1:
                continue
            for take in range(min(12, k - used) + 1):
                ndp[used + take] = max(ndp[used + take], dp[used] + pref[take])
        dp = ndp

    sys.stdin = old
    return str(dp[k])

assert run("""4 7
3 6 8 2 2 5 6 1 2 3 1 4
4 1 1 1 1 70 4 1 5 3 2 3
5 3 2 6 6 2 3 1 6 2 3 2
1 4 3 8 4 9 2 1 6 5 4 1
""") == "83"

assert run("""1 6
20 30 40 15 1 14 2 2 2 2 2 2
""") == "120"

assert run("""1 1
300 1 1 1 1 1 1 1 1 1 1 1
""") == "300"

assert run("""2 2
1 100 1 1 1 1 1 1 1 1 1 1
50 1 1 1 1 1 1 1 1 1 1 1
""") == "101"

assert run("""1 12
5 4 3 2 1 6 7 8 9 10 11 12
""") == "78"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cần kẹo đơn | 300 | tối thiểu`k`và lựa chọn tiền tố trực tiếp | 
| Giá trị lớn ẩn | 101 | Ngăn chặn các giải pháp tham lam được thông qua | 
| Lấy mọi thứ | 78 | Xử lý tiền tố có độ dài đầy đủ | 
| Mẫu | 83 và 120 | Xác nhận các trường hợp tiêu chuẩn | 

## Vỏ cạnh 

Đối với trường hợp giá trị ẩn:```
2 2
1 100 1 1 1 1 1 1 1 1 1 1
50 1 1 1 1 1 1 1 1 1 1 1
```DP trước tiên lưu trữ các tùy chọn từ bộ phân phối đầu tiên. Trạng thái lấy hai viên kẹo trở thành`101`, bởi vì cách duy nhất để truy cập`100`kẹo là để lấy`1`ở trên nó. Khi bộ phân phối thứ hai được xem xét, trạng thái`2`vẫn ở`101`, chứng tỏ thuật toán không bị mắc kẹt bởi những viên kẹo hấp dẫn cục bộ. 

Đối với trường hợp tiền tố sâu:```
1 3
1 1 100 1 1 1 1 1 1 1 1 1
```Bộ phân phối duy nhất được xử lý với tất cả độ dài tiền tố. Tiền tố có độ dài ba có giá trị`102`, trong khi tiền tố có độ dài một và hai không thể đạt tới`100`. Trạng thái DP cuối cùng của ba viên kẹo chính xác là`102`. 

Đối với trường hợp bộ phân phối đầy đủ:```
1 12
5 4 3 2 1 6 7 8 9 10 11 12
```Quá trình chuyển đổi cho phép lấy tất cả 12 viên kẹo vì danh sách tiền tố bao gồm độ dài 12. DP không bao giờ giả định bộ phân phối phải đóng góp ít kẹo hơn, do đó trạng thái cuối cùng chứa chính xác tổng của toàn bộ bộ phân phối.
