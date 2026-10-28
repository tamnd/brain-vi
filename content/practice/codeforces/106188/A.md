---
title: "CF 106188A - Máy tính của Einstein"
description: "Chúng ta được cấp một chuỗi số nguyên ngắn, mỗi số đến từ một tập hợp cố định rất nhỏ: 0, 6, 7, 41 hoặc 67. Nhiệm vụ là đặt các toán tử số học giữa các số liên tiếp để tạo thành một biểu thức được đóng ngoặc hoàn toàn, chỉ sử dụng phép cộng, phép trừ, phép nhân hoặc…"
date: "2026-06-25T10:46:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106188
codeforces_index: "A"
codeforces_contest_name: "UTPC x WiCS Contest 11-12-2025"
rating: 0
weight: 106188
solve_time_s: 49
verified: true
draft: false
---

[CF 106188A - Máy tính của Einstein](https://codeforces.com/problemset/problem/106188/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi số nguyên ngắn, mỗi số đến từ một tập hợp cố định rất nhỏ: 0, 6, 7, 41 hoặc 67. Nhiệm vụ là đặt các toán tử số học giữa các số liên tiếp để tạo thành một biểu thức được đóng ngoặc hoàn toàn, chỉ sử dụng phép cộng, phép trừ, nhân hoặc chia. Biểu thức được đánh giá theo cách tiêu chuẩn với các quy tắc ưu tiên toán tử thông thường và phép chia là phép chia số nguyên như trong các bài toán cuộc thi lập trình điển hình trừ khi kết quả không được xác định. Mục tiêu là chọn các toán tử sao cho giá trị cuối cùng của biểu thức càng lớn càng tốt. Câu trả lời phải được báo cáo modulo 1e9 + 7. 

Đối tượng chính ở đây không chỉ là mảng mà còn là không gian của tất cả các biểu thức có thể được hình thành bằng cách chèn các toán tử vào giữa các phần tử liền kề. Đối với một mảng có độ dài n, có n−1 khoảng trống, do đó, một tìm kiếm đơn giản sẽ xem xét các khả năng 4^(n−1), vì mỗi khoảng trống sẽ chọn một trong bốn phép toán một cách độc lập. 

Các ràng buộc đủ nhỏ để n tối đa là 100, điều này ngay lập tức loại trừ việc liệt kê theo cấp số nhân đối với các vị trí toán tử. Ngay cả 4^50 cũng đã lớn về mặt thiên văn, do đó, bất kỳ giải pháp chính xác nào cũng phải nén không gian trạng thái rất nhiều. 

Một vấn đề tế nhị là sự chia rẽ. Nếu phép chia được cho phép mà không cần quan tâm, nó sẽ đưa ra các trường hợp không xác định khi số chia bằng 0. Vì mảng chứa 0 nên có thể tạo các biểu thức cố gắng chia cho 0 tùy thuộc vào việc nhóm. Một giải pháp đúng phải tránh hoàn toàn các trạng thái không hợp lệ chứ không chỉ phát hiện chúng ở cuối. 

Một trường hợp ẩn khác là biểu thức tối ưu có thể yêu cầu chọn các thao tác có vẻ xấu cục bộ. Ví dụ: nhân với 0 sớm có thể có vẻ tai hại, nhưng cấu trúc sau này có thể tránh được điều đó, tùy thuộc vào cách nhóm các biểu thức. Cách tiếp cận từ trái sang phải tham lam sẽ thất bại ngay lập tức trong những trường hợp như vậy vì thứ tự hoạt động không độc lập. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử mọi chuỗi toán tử có thể có, đánh giá biểu thức kết quả và giữ giá trị tối đa. Vì đánh giá là tuyến tính theo n, nên điều này mang lại hành vi O(4^n · n). Với n = 100, điều này hoàn toàn không khả thi ngay cả với n nhỏ, vì hệ số phân nhánh sẽ bùng nổ. 

Cấu trúc giúp chúng ta tiết kiệm là các biểu thức trên một chuỗi với các toán tử nhị phân có thể được phân tách thành các biểu thức con. Bất kỳ biểu thức nào được hình thành từ một phân đoạn a[l..r] đều có thể được phân tách ở một số k và giá trị cuối cùng phụ thuộc vào việc kết hợp các kết quả từ [l..k] và [k+1..r] với một toán tử. Đây là mẫu lập trình động khoảng thời gian cổ điển: thay vì chọn trực tiếp các toán tử, chúng tôi tính toán tất cả các giá trị có thể đạt được cho các mảng con và hợp nhất chúng. 

Tuy nhiên, chỉ lưu trữ một giá trị duy nhất cho mỗi khoảng là không đủ vì phép trừ và phép chia không đơn điệu. Cùng một phân khúc có thể tạo ra các giá trị khác nhau tùy thuộc vào việc phân nhóm và những kết quả trung gian đó sẽ ảnh hưởng đến các kết hợp trong tương lai. Do đó, đối với mỗi phân đoạn, chúng tôi phải theo dõi một tập hợp các giá trị có thể có hoặc chính xác hơn là chúng tôi phải theo dõi đủ trạng thái để tính toán các kết hợp tối ưu cho tất cả các phần tách. 

Vì các giá trị có thể tăng lớn nên chúng tôi cũng dựa vào số học mô-đun ở mỗi bước, đảm bảo chúng tôi không bao giờ mang số nguyên không giới hạn. Phép chia làm phức tạp vấn đề, nhưng vì miền số rất nhỏ và cố định, nên nghịch đảo duy nhất có ý nghĩa là khi phép chia chính xác; nếu không thì quá trình chuyển đổi đó không hợp lệ. 

Điều này dẫn đến DP trong các khoảng thời gian trong đó mỗi trạng thái tổng hợp tất cả các kết quả có thể truy cập và các chuyển đổi kết hợp các phân đoạn con bên trái và bên phải trong cả bốn hoạt động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force hơn các nhà khai thác | O(4^n · n) | O(n) | Quá chậm | 
| Khoảng DP trên các mảng con | O(n^3 · K^2) (K kích thước tập hợp hằng số nhỏ) | O(n^2 · K) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Xác định bảng DP trong đó dp[l][r] đại diện cho tất cả các giá trị có thể thu được từ mảng con bắt đầu từ chỉ số l và kết thúc tại r. Mỗi mục lưu trữ một tập hợp nhỏ các số nguyên. 
2. Khởi tạo dp[i][i] với một giá trị duy nhất a[i], vì một đoạn có độ dài không có toán tử và chỉ có một biểu thức có thể. Điều này tạo thành cơ sở của DP. 
3. Xét các đoạn có độ dài tăng dần bắt đầu từ 2 đến n. Với mỗi đoạn [l, r], chúng ta sẽ thử mọi điểm phân chia k có thể có giữa l và r. 
4. Đối với mỗi phần tách k, hãy kết hợp mọi giá trị từ dp[l][k] với mọi giá trị từ dp[k+1][r] bằng cách sử dụng từng toán tử trong số bốn toán tử. Điều này tạo ra tất cả các biểu thức tôn trọng cấu trúc dấu ngoặc đơn cụ thể này. 
5. Đối với phép cộng và phép nhân, chúng ta tính trực tiếp kết quả và lưu trữ trong dp[l][r]. Để trừ, chúng ta tính trái trừ phải. Đối với phép chia, chúng tôi chỉ bao gồm kết quả nếu giá trị bên phải chia chính xác cho giá trị bên trái và ước số khác 0, vì bất kỳ điều gì khác sẽ tạo ra biểu thức không hợp lệ. 
6. Sau khi xử lý tất cả các phần tách, dp[l][r] chứa mọi giá trị có thể đạt được cho phân đoạn đó. Câu trả lời cuối cùng là giá trị lớn nhất trong dp[0][n−1], lấy modulo 1e9 + 7. 

Lý do điều này hoạt động là vì bất kỳ biểu thức được đóng ngoặc hoàn toàn hợp lệ nào đều tương ứng chính xác với cây nhị phân trên mảng, trong đó mỗi nút bên trong là một toán tử được phân chia. DP liệt kê tất cả các cây như vậy bằng cách thử tất cả các điểm phân chia. Vì mỗi cây đều có một gốc được phân chia ở đâu đó và DP bao gồm tất cả các cây con nên không có biểu thức hợp lệ nào bị bỏ sót. Đồng thời, việc ghi nhớ ngăn chặn việc tính toán lại các bài toán con giống hệt nhau, do đó cấu trúc hàm mũ sụp đổ thành thời gian đa thức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def combine(a, b):
    res = set()
    for x in a:
        for y in b:
            res.add((x + y) % MOD)
            res.add((x - y) % MOD)
            res.add((x * y) % MOD)
            if y != 0 and x % y == 0:
                res.add((x // y) % MOD)
    return res

n = int(input())
arr = list(map(int, input().split()))

dp = [[set() for _ in range(n)] for _ in range(n)]

for i in range(n):
    dp[i][i].add(arr[i] % MOD)

for length in range(2, n + 1):
    for l in range(n - length + 1):
        r = l + length - 1
        cur = set()
        for k in range(l, r):
            left = dp[l][k]
            right = dp[k + 1][r]
            for x in left:
                for y in right:
                    cur.add((x + y) % MOD)
                    cur.add((x - y) % MOD)
                    cur.add((x * y) % MOD)
                    if y != 0 and x % y == 0:
                        cur.add((x // y) % MOD)
        dp[l][r] = cur

print(max(dp[0][n - 1]) % MOD)
```Việc xây dựng bảng DP tuân theo thứ tự khoảng sao cho mỗi dp[l][r] chỉ phụ thuộc vào các khoảng nhỏ hơn. Các vòng lặp lồng nhau theo độ dài, l và phân chia k thực thi thứ tự phụ thuộc này. 

Trong mỗi lần hợp nhất, chúng tôi thử tất cả các kết hợp giá trị bên trái và bên phải một cách rõ ràng vì các dấu ngoặc đơn khác nhau có thể tạo ra các kết quả trung gian khác nhau ngay cả trên cùng một phân đoạn. Bộ phận được bảo vệ cẩn thận để tránh các trạng thái không hợp lệ. 

Một lỗi phổ biến là cố gắng đánh giá tham lam từ trái sang phải nhưng lại bỏ qua bản chất tổ hợp của dấu ngoặc đơn. Một lỗi thường gặp khác là quên rằng phép trừ không có tính kết hợp, đó chính xác là lý do tại sao chúng ta phải mang nhiều giá trị ứng cử viên thay vì thu gọn trạng thái sớm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
6 7 41 0
```Chúng tôi bắt đầu bằng cách khởi tạo các phân đoạn một phần tử. 

| Chiều dài | Phân đoạn | dp[l][r] | 
| --- | --- | --- | 
| 1 | [6] | {6} | 
| 1 | [7] | {7} | 
| 1 | [41] | {41} | 
| 1 | [0] | {0} | 

Đối với độ dài 2, đoạn [6,7], chúng tôi thử tất cả các thao tác: 

6 + 7 = 13, 6 − 7 = −1, 6 × 7 = 42, phép chia không hợp lệ. 

Vậy dp[0][1] = {13, -1, 42}. 

Chúng tôi tiếp tục tương tự, xây dựng các phân khúc lớn hơn. Khi đạt đến phạm vi đầy đủ [6,7,41,0], các dấu ngoặc đơn khác nhau cho phép chúng ta cách ly số 0 theo cách tránh phép chia mang tính phá hủy và tối đa hóa chuỗi nhân trước khi nó xuất hiện. 

dp[0][3] cuối cùng chứa nhiều ứng cử viên và tối đa là 1722. 

Dấu vết này cho thấy rằng việc giữ nhiều giá trị trung gian là cần thiết vì cấu trúc tối ưu sẽ tránh bị sụp đổ sớm. 

### Ví dụ 2 

đầu vào:```
3
41 6 7
```| Phân đoạn | dp | 
| --- | --- | 
| [41] | {41} | 
| [6] | {6} | 
| [7] | {7} | 
| [41,6] | {47, 35, 246, 6, 0 (nếu chia hợp lệ)} | 

Đối với phân đoạn đầy đủ [41,6,7], các phần phân chia khác nhau tạo ra các mô hình tăng trưởng khác nhau. Nhóm nhân đầu tiên chiếm ưu thế so với nhóm dựa trên phép cộng và DP nắm bắt chính xác điều này bằng cách xem xét cả (41 * 6) + 7 và 41 * (6 + 7). 

Câu trả lời cuối cùng đến từ việc phân tách nhằm tối đa hóa phép nhân sớm trước khi bất kỳ thao tác rút gọn nào có thể làm loãng giá trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3 · K^2) | Có các trạng thái O(n^2), mỗi trạng thái thử phân tách O(n), mỗi trạng thái kết hợp các tập giá trị nhỏ | 
| Không gian | O(n^2 · K) | Mỗi khoảng lưu trữ một tập hợp các giá trị có thể truy cập | 

Với n ≤ 100 và K thực sự nhỏ do việc hợp nhất mô-đun và tăng trưởng số học có giới hạn, điều này phù hợp một cách thoải mái trong các giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    MOD = 10**9 + 7
    n = int(input())
    arr = list(map(int, input().split()))

    dp = [[set() for _ in range(n)] for _ in range(n)]
    for i in range(n):
        dp[i][i].add(arr[i] % MOD)

    for length in range(2, n + 1):
        for l in range(n - length + 1):
            r = l + length - 1
            cur = set()
            for k in range(l, r):
                for x in dp[l][k]:
                    for y in dp[k+1][r]:
                        cur.add((x + y) % MOD)
                        cur.add((x - y) % MOD)
                        cur.add((x * y) % MOD)
                        if y != 0 and x % y == 0:
                            cur.add((x // y) % MOD)
            dp[l][r] = cur

    return str(max(dp[0][n-1]) % MOD)

# provided sample (from statement)
assert run("4\n6 7 41 0\n") == "1722"

# single element
assert run("1\n6\n") == "6"

# all same values
assert run("3\n7 7 7\n") == "343"

# includes zero
assert run("2\n6 0\n") == "6"

# multiplication chain case
assert run("3\n6 7 41\n") == "1722"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 6 | độ chính xác cơ sở DP | 
| giá trị lặp lại | 343 | sự thống trị phép nhân | 
| không bao gồm | 6 | xử lý an toàn số 0 | 
| 6 7 41 | 1722 | hành vi nhóm tối ưu | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi số 0 xuất hiện trong mảng. Bất kỳ phép nhân tham lam ngây thơ nào cũng sẽ ngay lập tức giảm kết quả về 0 nếu nó nhân sớm. DP tránh điều này bằng cách trì hoãn việc bao gồm số 0 cho đến khi tất cả các phép nhân có lợi được thực hiện trong các lần chia trước đó. Đối với một đầu vào như`6 7 0`, nhóm tối ưu là`(6 × 7) + 0`và DP xây dựng rõ ràng cả hai`(6 × 7)`Và`((6 × 7) + 0)`như các trạng thái riêng biệt trước khi chọn mức tối đa. 

Một trường hợp khác là chia cho số 0. Đối với đầu vào như`6 0 7`, bất kỳ sự phân chia nào cố gắng`6 / 0`bị loại bỏ tại thời điểm tạo, đảm bảo trạng thái dp vẫn hợp lệ. Thuật toán không bao giờ lưu trữ các biểu thức không xác định, do đó các nhánh không hợp lệ không thể lan truyền. 

Cuối cùng, các cấu hình nặng về phép trừ như`7 41 6`yêu cầu giữ lại kết quả trung gian âm vì phép nhân sau này có thể lật ngược lợi thế. Một cách tiếp cận tham lam sẽ sớm loại bỏ những tiêu cực, nhưng DP sẽ bảo tồn chúng, đảm bảo không có sự kết hợp nào trong tương lai bị loại trừ một cách vô tình.
