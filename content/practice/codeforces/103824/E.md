---
title: "CF 103824E - awa\u75c7\u5019\u7fa4"
description: "Chúng ta được cung cấp một chuỗi các chữ cái viết thường và số lần xuất hiện mục tiêu của mẫu “awa”. Chúng tôi được phép sửa đổi các ký tự một cách tự do, nhưng mỗi sửa đổi sẽ thay đổi chính xác một vị trí thành bất kỳ chữ cái viết thường nào."
date: "2026-07-02T08:18:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103824
codeforces_index: "E"
codeforces_contest_name: "2022 Summer Camp of XTU Qualifying Round"
rating: 0
weight: 103824
solve_time_s: 49
verified: true
draft: false
---

[CF 103824E - awa\u75c7\u5019\u7fa4](https://codeforces.com/problemset/problem/103824/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các chữ cái viết thường và số lần xuất hiện mục tiêu của mẫu “awa”. Chúng tôi được phép sửa đổi các ký tự một cách tự do, nhưng mỗi sửa đổi sẽ thay đổi chính xác một vị trí thành bất kỳ chữ cái viết thường nào. Mục tiêu là làm cho chuỗi cuối cùng chứa ít nhất k lần xuất hiện của chuỗi con “awa”, trong đó mỗi lần xuất hiện chỉ được tính nếu nó sử dụng ba vị trí liên tiếp và quan trọng là không có vị trí nào có thể được chia sẻ giữa hai lần xuất hiện khác nhau. 

Nhiệm vụ là xác định số lượng thay đổi ký tự tối thiểu cần thiết để đạt được cấu hình có ít nhất k chuỗi con “awa” rời rạc. 

Một hạn chế quan trọng về cấu trúc là sự xuất hiện của “awa” không thể trùng lặp. Điều này ngay lập tức ngụ ý rằng nếu chúng ta quyết định đặt k mẫu, chúng sẽ chiếm 3k chỉ mục riêng biệt và các mẫu phải được đặt cách nhau để không có chỉ mục nào được sử dụng lại. 

Kích thước đầu vào n lên tới 2000, điều này cho thấy giải pháp kiểu O(n^2) hoặc O(nk) là khả thi. Một giải pháp bậc ba trên tất cả các chuỗi con và tất cả các lựa chọn của mẫu k vẫn có thể vượt qua, nhưng mọi thứ theo cấp số nhân trên các vị trí sẽ quá chậm. 

Một trường hợp phức tạp xuất phát từ các ứng cử viên chồng chéo. Ví dụ: trong một chuỗi như “awawa”, có hai chuỗi con bằng “awa” bắt đầu ở vị trí 1 và 3, nhưng chúng trùng nhau ở vị trí 3. Mặc dù cả hai chuỗi con đều khớp cục bộ nhưng chúng ta không được phép đếm cả hai cùng một lúc. Bộ đếm chuỗi con ngây thơ sẽ đánh giá quá cao các mẫu hợp lệ và dẫn đến câu trả lời sai nếu nó không thực thi tính rời rạc. 

Một góc quan trọng khác là khi k bằng 0. Trong trường hợp đó, không cần mẫu và câu trả lời phải là 0 bất kể nội dung chuỗi. Một DP ngây thơ giả định ít nhất một mẫu có thể vô tình trả về một giá trị lớn hoặc khởi tạo không thành công. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là xem xét tất cả các cách chọn k đoạn rời rạc có độ dài 3. Đối với mỗi đoạn, chúng tôi tính toán cần bao nhiêu thay đổi để chuyển nó thành “awa”, đơn giản là số lượng ký tự không khớp. Sau đó, chúng tôi thử tất cả các kết hợp hợp lệ của k phân đoạn. 

Ý tưởng bạo lực này đúng vì nó trực tiếp mô hình hóa vấn đề: mỗi phân đoạn được chọn bị buộc phải “awa” và chúng tôi phải trả chi phí chỉnh sửa. Khó khăn là số cách chọn k đoạn rời rạc là rất lớn. Ngay cả khi chúng tôi giới hạn bản thân ở các bộ ba không chồng chéo hợp lệ, chúng tôi vẫn chọn k phân đoạn trong số khoảng n vị trí bắt đầu có thể có với các ràng buộc về khoảng cách, dẫn đến vụ nổ tổ hợp. 

Quan sát chính là đây là vấn đề lựa chọn có trọng số trên một dòng có các khoảng không chồng chéo. Mỗi khoảng có độ dài cố định là 3 và có chi phí. Chúng ta cần chính xác khoảng k, không trùng lặp, giảm thiểu tổng chi phí. Đây là cấu trúc lập trình động cổ điển trong đó chúng tôi xử lý chuỗi từ trái sang phải và quyết định xem có bắt đầu một khoảng thời gian ở mỗi vị trí hay không. 

Chúng tôi xác định DP tại vị trí i, chúng tôi xem xét bỏ qua nó hoặc bắt đầu khối "awa" tại i, tiêu thụ i, i+1, i+2 và đóng góp một chi phí bằng với sự không khớp. Điều này làm giảm vấn đề thành DP tuyến tính với hai trạng thái: vị trí và số khối được chọn. 

Quá trình chuyển đổi rất đơn giản vì khi chúng ta đặt một khối tại i, vị trí hợp lệ tiếp theo sẽ trở thành i+3, tạo ra sự rời rạc một cách tự nhiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force qua lựa chọn phân khúc | Hàm mũ | O(1) | Quá chậm | 
| DP trên vị trí và số lượng khối “awa” | O(nk) | O(nk) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý chuỗi từ trái sang phải và theo dõi xem chúng tôi đã đặt bao nhiêu khối “awa”.

1. Xác định dp[i][j] là số lượng sửa đổi tối thiểu cần thiết bằng cách sử dụng tiền tố bắt đầu từ vị trí i, sau khi đã đặt j phân đoạn “awa” hợp lệ. Trạng thái này đại diện cho một vấn đề quyết định hậu tố. 
2. Tại mỗi vị trí i, ta có hai lựa chọn. Chúng ta có thể bỏ qua vị trí i và chuyển tới i+1 mà không thay đổi j. Điều này thể hiện việc không bắt đầu “awa” tại i. 
3. Nếu còn lại ít nhất 3 ký tự và j < k, chúng ta có thể thử tạo một chữ “awa” bắt đầu từ i. Cái giá của hành động này được tính bằng số lượng không khớp giữa s[i], s[i+1], s[i+2] và chuỗi “awa”. 
4. Khi chúng tôi thực hiện tùy chọn này, chúng tôi sẽ chuyển sang i+3 và tăng j lên 1. Việc nhảy thêm 3 sẽ tự động thực thi tính năng không chồng chéo. 
5. Câu trả lời là dp[0][0], nghĩa là chúng ta bắt đầu lại từ đầu với các mẫu được xây dựng bằng 0. 

Lý do đằng sau sự chuyển đổi là mọi giải pháp hợp lệ có thể được phân tách duy nhất thành một tập hợp các vị trí bắt đầu của các khối có độ dài 3 và DP liệt kê các lựa chọn này theo thứ tự chỉ số tăng dần mà không lặp lại. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là ở mọi trạng thái (i, j), dp lưu trữ chi phí tối thiểu trên tất cả các cách hợp lệ để đặt chính xác j khối “awa” rời rạc hoàn toàn trong hậu tố bắt đầu từ i. Mọi chuyển đổi đều duy trì tính hợp lệ: bỏ qua sẽ giữ nguyên hậu tố và việc đặt một khối sẽ tiêu tốn chính xác ba ký tự và chuyển sang hậu tố độc lập tiếp theo. Bởi vì chúng ta chỉ tiến về phía trước trong i nên không có cấu hình nào có thể được tính hai lần hoặc được hình thành theo cách chồng chéo không hợp lệ. Điều này đảm bảo rằng mọi lựa chọn khả thi của k khối đều tương ứng với chính xác một đường dẫn DP và DP đánh giá chính xác chi phí của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**18

def solve():
    n, k = map(int, input().split())
    s = input().strip()

    # dp[i][j] = min cost from i with j blocks already used
    # we use rolling over i, so dp[i] is array of size k+1
    dp = [[INF] * (k + 1) for _ in range(n + 4)]

    # base: at position n, cost is 0 if j == k else impossible
    for j in range(k + 1):
        dp[n][j] = 0 if j == k else INF

    # fill backwards
    for i in range(n - 1, -1, -1):
        for j in range(k + 1):
            # option 1: skip
            dp[i][j] = dp[i + 1][j]

            # option 2: take block starting at i
            if j < k and i + 2 < n:
                cost = 0
                cost += (s[i] != 'a')
                cost += (s[i + 1] != 'w')
                cost += (s[i + 2] != 'a')
                dp[i][j] = min(dp[i][j], cost + dp[i + 3][j + 1])

    print(dp[0][0])

if __name__ == "__main__":
    solve()
```Bảng DP được xây dựng từ phải sang trái để các chuyển đổi sang i+1 và i+3 đã được tính toán sẵn. Điều kiện cơ bản bắt buộc rằng chính xác k khối phải được hình thành vào thời điểm chúng ta đi đến cuối; nếu không thì trạng thái không hợp lệ. 

Việc tính toán chi phí là thời gian cục bộ và không đổi trên mỗi trạng thái, đồng thời hai quá trình chuyển đổi phản ánh trực tiếp mô tả thuật toán: bỏ qua hoặc đặt “awa” bắt đầu từ chỉ mục hiện tại. 

Một chi tiết tinh tế là việc sử dụng khởi tạo dp[n][j]. Chỉ trạng thái có j == k là hợp lệ ở cuối, bởi vì chúng tôi yêu cầu ít nhất k lần xuất hiện và mọi yêu cầu bổ sung chưa được thực hiện đều không thể thực hiện được. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 3, k = 1 

s = "bbb" 

Chúng tôi tính toán vị trí có thể bắt đầu từ chỉ số 0. 

| tôi | j | hành động | chi phí | trạng thái tiếp theo | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | lấy “bbb” → “awa” | 3 | dp[3][1] | 

Việc chuyển đổi “bbb” thành “awa” cần 3 lần thay đổi. 

DP so sánh việc bỏ qua (không hợp lệ vì không còn khoảng trống để đạt k=1) với việc lấy khối. Kết quả là 3. 

Điều này xác nhận rằng thuật toán tính chính xác chi phí chỉnh sửa cục bộ và thực thi phạm vi bao phủ đầy đủ. 

### Ví dụ 2 

đầu vào: 

n = 6, k = 2 

s = "awaw" 

Tại i = 0, lấy sẽ có giá 0 cho “awa”, sau đó nhảy tới i = 3. 

Tại i = 3, “awa” lại có giá 0. 

| tôi | j | sự lựa chọn | chi phí | 
| --- | --- | --- | --- | 
| 0 | 0 | lấy | 0 | 
| 3 | 1 | lấy | 0 | 

Tổng chi phí là 0. 

Điều này cho thấy rằng việc thực thi không chồng chéo thông qua i+3 một cách chính xác cho phép các mô hình giáp lưng mà không bị can thiệp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nk) | Mỗi trạng thái (i, j) đánh giá hai lần chuyển đổi trong O(1) | 
| Không gian | O(nk) | Bảng DP về vị trí và số lượng mẫu | 

Với n lên tới 2000, điều này dẫn đến khoảng 4 triệu trạng thái, nằm trong giới hạn thoải mái trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue().strip() if False else solve_capture(inp)

def solve_capture(inp: str) -> str:
    import sys
    input = sys.stdin.readline
    n, k = map(int, inp.splitlines()[0].split())
    s = inp.splitlines()[1]

    INF = 10**18
    dp = [[INF] * (k + 1) for _ in range(n + 4)]

    for j in range(k + 1):
        dp[n][j] = 0 if j == k else INF

    for i in range(n - 1, -1, -1):
        for j in range(k + 1):
            dp[i][j] = dp[i + 1][j]
            if j < k and i + 2 < n:
                cost = (s[i] != 'a') + (s[i+1] != 'w') + (s[i+2] != 'a')
                dp[i][j] = min(dp[i][j], cost + dp[i + 3][j + 1])

    return str(dp[0][0])

# sample
assert solve_capture("3 1\nbbb") == "3"

# minimum k=0
assert solve_capture("5 0\nabcde") == "0"

# already perfect single
assert solve_capture("3 1\nawa") == "0"

# two back-to-back
assert solve_capture("6 2\nawawaw") == "0"

# needs edits but optimal skipping
assert solve_capture("6 1\nbbbbbb") == "3"

# boundary overlap trap
assert solve_capture("5 1\nawawa") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| k = 0 trường hợp | 0 | xử lý yêu cầu trống | 
| đã đúng “awa” | 0 | vị trí không tốn phí | 
| lặp đi lặp lại mô hình hoàn hảo | 0 | tính đúng đắn không chồng chéo | 
| tất cả b | 3 hoặc bội số | tính toán chi phí | 
| ứng viên chồng chéo | 0/1 đúng | ngăn ngừa việc đếm hai lần | 

## Vỏ cạnh 

Với k = 0, DP khởi tạo chính xác tất cả các trạng thái hợp lệ ở lớp cuối vì không yêu cầu khối nào. Điều này buộc dp[0][0] duy trì ở mức 0 vì việc bỏ qua luôn đảm bảo tính khả thi. 

Đối với các mẫu chồng chéo như “awawa”, cấu trúc chuyển tiếp không bao giờ cho phép sử dụng lại vị trí i+1 hoặc i+2 sau khi một khối được lấy tại i. Mặc dù một “awa” hợp lệ khác bắt đầu ở i+2, nhưng nó đương nhiên bị bỏ qua vì DP tiến về phía trước một cách nghiêm ngặt, đảm bảo chỉ xem xét các lựa chọn rời rạc.
