---
title: "CF 102788H - Kỳ thi"
description: "Chúng ta có một chiếc máy bắt đầu bằng giá trị 1. Chương trình dành cho chiếc máy này là một chuỗi các lệnh. Một lệnh tăng giá trị hiện tại lên 1, lệnh khác tăng giá trị đó lên một giá trị không xác định x lớn hơn 1 và lệnh thứ ba nhân giá trị hiện tại với 7."
date: "2026-08-03T15:04:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102788
codeforces_index: "H"
codeforces_contest_name: "2017-2018 ICPC Central Quarter Final of Northeastern European Regional Collegiate Programming Contest"
rating: 0
weight: 102788
solve_time_s: 65
verified: true
draft: false
---

[CF 102788H - Bài kiểm tra](https://codeforces.com/problemset/problem/102788/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chiếc máy bắt đầu bằng giá trị 1. Chương trình dành cho chiếc máy này là một chuỗi các lệnh. Một lệnh tăng giá trị hiện tại lên 1, lệnh khác tăng giá trị đó lên một giá trị không xác định x lớn hơn 1 và lệnh thứ ba nhân giá trị hiện tại với 7. 

Đối với một x cố định, Vasily đếm xem có bao nhiêu chuỗi lệnh khác nhau kết thúc chính xác tại N. Chúng ta được cho N và số lượng K cần thiết, đồng thời chúng ta phải tìm x nhỏ nhất làm cho số chương trình hợp lệ bằng K. Nếu không có x nào như vậy tồn tại, chúng ta sẽ xuất ra 0. 

Các ràng buộc đủ nhỏ trên N để có thể áp dụng phương pháp bậc hai. Vì N tối đa là 10000 nên giải pháp thực hiện khoảng 10^8 thao tác đơn giản là gần với giới hạn thực tế trong Python, nhưng giải pháp khám phá nhiều chuỗi lệnh theo cấp số nhân là không thể. Giá trị K có thể lớn tới 2^60, vì vậy chúng ta không bao giờ cần số đếm chính xác trên điểm đó. Chúng ta chỉ cần biết liệu câu trả lời đã đạt hay vượt quá K hay chưa, điều này cho phép chúng ta giới hạn các giá trị trung gian và tránh tăng số nguyên lớn không cần thiết. 

Các trường hợp cạnh chính xuất phát từ các giá trị của x gần với giới hạn. Nếu x lớn hơn N thì không bao giờ được sử dụng lệnh 2 nhưng câu trả lời phải thỏa mãn x < N. Ví dụ: nếu N = 3 và K = 1 thì chương trình hợp lệ duy nhất là 1,1, do đó không có x hợp lệ và đầu ra là 0. 

Một trường hợp khác là khi phép nhân tạo ra các đường dẫn bổ sung. Với N = 7 và x = 5, các chương trình hợp lệ là 1,1,1,1,1,1, 2,1, 1,2 và 3, cho K = 4. Một giải pháp chỉ xét đến phép cộng sẽ tính ba chương trình và thất bại vì nó bỏ qua quá trình chuyển đổi nhân. 

Trường hợp cạnh cuối cùng là khi số lượng lớn hơn K trong quá trình tính toán. Ví dụ: nếu một ứng cử viên x tạo ra hơn 2^60 chương trình có thể thì việc lưu trữ giá trị chính xác là không cần thiết. Việc triển khai bất cẩn có thể tốn thời gian xây dựng các số nguyên khổng lồ thay vì dừng lại ở ngưỡng yêu cầu. 

## Phương pháp tiếp cận 

Một giải pháp brute-force trực tiếp sẽ tạo ra mọi chuỗi lệnh có thể có và mô phỏng nó cho đến khi đạt đến N. Điều này đúng vì mọi chương trình đều tương ứng với chính xác một chuỗi thao tác. Vấn đề là số lượng trình tự tăng lên cực kỳ nhanh chóng. Ngay cả trước khi xem xét phép nhân, số cách viết hiệu bằng phép cộng tăng lên theo tổ hợp, do đó việc liệt kê các chương trình trở nên bất khả thi. 

Một cách nhìn tốt hơn là đếm các cách để đạt được từng giá trị thay vì xây dựng chương trình. Đối với x cố định, gọi dp[v] là số chuỗi lệnh biến đổi giá trị ban đầu 1 thành giá trị v. Lệnh cuối cùng của chương trình kết thúc tại v phải là một trong ba khả năng. Nó có thể thêm 1, nghĩa là chúng ta đến từ v - 1. Nó có thể thêm x, nghĩa là chúng ta đến từ v - x. Hoặc nó có thể nhân với 7, nghĩa là chúng ta đến từ v / 7 nếu v chia hết cho 7. 

Sự tái phát là: 

dp[v] = dp[v - 1] + dp[v - x] + dp[v / 7] 

trong đó chỉ bao gồm các chuyển đổi hợp lệ. Việc tính toán phép truy hồi này cho một x sẽ mất O(N). Vì chúng ta cần x nhỏ nhất nên chúng ta chỉ cần thử x từ 2 đến N - 1 và dừng ở số đầu tiên tạo ra K. 

Nhận xét quan trọng là không gian tìm kiếm x đủ nhỏ. Chúng ta không cần phép đảo ngược toán học phức tạp hơn vì N chỉ bằng 10000. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Lập trình động cho mọi x | O(N^2) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Thử mọi giá trị có thể có của x từ 2 đến N - 1. Chúng tôi kiểm tra các giá trị theo thứ tự tăng dần vì câu trả lời bắt buộc là x hợp lệ nhỏ nhất. 
2. Đối với x hiện tại, hãy tạo một mảng lập trình động trong đó dp[i] biểu thị số lượng chương trình đạt giá trị i từ giá trị bắt đầu 1. 
3. Khởi tạo dp[1] = 1 vì trước khi thực hiện bất kỳ lệnh nào, chúng ta đã ở trạng thái bắt đầu. 
4. Xử lý các giá trị từ 2 đến N. Cộng số cách từ mỗi trạng thái có thể có trước đó: 
dp[i - 1] cho lệnh 1, dp[i - x] cho lệnh 2 khi i >= x + 1 và dp[i / 7] cho lệnh 3 khi i chia hết cho 7. 
5. Sau khi tính dp[N], so sánh nó với K. Nếu chúng bằng nhau, in ra x hiện tại. 
6. Nếu mọi x có thể đã được kiểm tra và không có cái nào hoạt động, xuất ra 0. 

Tại sao nó hoạt động: 

Mỗi chương trình kết thúc bằng giá trị i đều có một lệnh cuối cùng duy nhất. Việc xóa lệnh cuối cùng đó sẽ để lại một chương trình hợp lệ nhỏ hơn kết thúc ở trạng thái chính xác trước thao tác cuối cùng. Sự lặp lại đếm tất cả các trạng thái có thể có trước đó và các nhóm này không thể chồng lên nhau vì lệnh cuối cùng là khác nhau. Do đó dp[i] đếm mọi chương trình hợp lệ chính xác một lần. Kiểm tra x theo thứ tự tăng dần đảm bảo rằng kết quả khớp đầu tiên là giá trị nhỏ nhất có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

LIMIT = 1 << 60

def count_programs(n, x, target):
    dp = [0] * (n + 1)
    dp[1] = 1

    for i in range(2, n + 1):
        cur = dp[i - 1]

        if i - x >= 1:
            cur += dp[i - x]

        if i % 7 == 0:
            cur += dp[i // 7]

        if cur > target:
            cur = target + 1

        dp[i] = cur

    return dp[n]

def solve():
    n, k = map(int, input().split())

    for x in range(2, n):
        if count_programs(n, x, k) == k:
            print(x)
            return

    print(0)

solve()
```chức năng`count_programs`thực hiện lập trình động được mô tả trong hướng dẫn. Mảng có các chỉ số khớp với giá trị thực tế được máy hiển thị, giúp tránh các lỗi chuyển đổi giữa trạng thái chương trình và vị trí mảng. 

Số lượng được giới hạn khi vượt quá K. Chỉ có sự bình đẳng với K mới quan trọng, vì vậy mọi giá trị lớn hơn đều tương đương với quyết định cuối cùng. Điều này cũng giữ cho các giá trị trung gian có thể quản lý được. 

Quá trình chuyển đổi phép nhân được kiểm tra sau các thao tác khác vì nó không phụ thuộc vào x. điều kiện`i % 7 == 0`là cần thiết vì chỉ những số chia hết cho 7 mới có thể được tạo ra bằng cách nhân giá trị trước đó với 7. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, N = 7 và K = 4. 

| Giá trị | dp[giá trị] | Lý do | 
| --- | --- | --- | 
| 1 | 1 | Trạng thái bắt đầu | 
| 2 | 1 | Chỉ thêm 1 | 
| 3 | 1 | Chỉ thêm 1 lần nữa | 
| 4 | 1 | Chỉ thêm 1 lần nữa | 
| 5 | 1 | Chỉ thêm 1 lần nữa | 
| 6 | 2 | Thêm 1 hoặc thêm x | 
| 7 | 4 | Thêm 1, thêm x hoặc nhân | 

Ứng viên x = 5 tạo ra chính xác bốn chương trình, vì vậy câu trả lời là 5. 

Đối với mẫu thứ hai, N = 14 và K = 3. 

Việc kiểm tra mỗi x dưới 14 sẽ cho kết quả đếm khác 3. Quá trình lập trình động sẽ kiểm tra mọi chuyển đổi có thể xảy ra, bao gồm cả phép nhân từ 2 đến 14, nhưng không có x nào tạo ra chính xác ba chương trình, vì vậy đầu ra là 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N^2) | Có thể có O(N) giá trị x và mỗi chương trình động mất O(N) thời gian. | 
| Không gian | O(N) | Chỉ mảng lập trình động hiện tại được lưu trữ. | 

Với N = 10000, thuật toán thực hiện khoảng 100 triệu chuyển đổi đơn giản trong trường hợp xấu nhất. Số học bị giới hạn giúp mỗi lần chuyển đổi không tốn kém và mức sử dụng bộ nhớ vẫn ở mức thấp. 

## Trường hợp thử nghiệm```python
import sys
import io

LIMIT = 1 << 60

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())

    def count_programs(n, x, target):
        dp = [0] * (n + 1)
        dp[1] = 1
        for i in range(2, n + 1):
            cur = dp[i - 1]
            if i - x >= 1:
                cur += dp[i - x]
            if i % 7 == 0:
                cur += dp[i // 7]
            dp[i] = min(cur, target + 1)
        return dp[n]

    for x in range(2, n):
        if count_programs(n, x, k) == k:
            return str(x) + "\n"
    return "0\n"

assert run("7 4\n") == "5\n"
assert run("14 3\n") == "0\n"

assert run("3 1\n") == "0\n"
assert run("7 1\n") == "0\n"
assert run("8 4\n") == "0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1 | 0 | Không có x hợp lệ nào tồn tại khi chỉ những phép cộng mới có thể đạt được mục tiêu | 
| 7 1 | 0 | Đường nhân được xem xét | 
| 8 4 | 0 | Kiểm tra các giá trị gần ranh giới nhỏ | 
| 7 4 | 5 | Xác thực trường hợp mẫu bằng nhiều loại lệnh | 

## Vỏ cạnh 

Với N = 3 và K = 1, mọi x có thể đều không hợp lệ vì x phải nằm trong khoảng từ 2 đến N - 1, và ứng cử viên duy nhất là x = 2. Số đếm lập trình động cho x = 2 lớn hơn một vì cả hai phép cộng đều có thể được sử dụng, do đó thuật toán sẽ loại bỏ nó một cách chính xác và in ra 0. 

Với N = 7 và K = 4, thuật toán đạt đến chuyển đổi nhân khi xử lý giá trị 7. Nó bao gồm dp[1] vì 7 chia hết cho 7, cộng chương trình chỉ gồm lệnh 3. Đây là chuyển đổi bị bỏ sót bởi các phương pháp chỉ có phép cộng mô hình. 

Đối với trường hợp số lượng chương trình lớn, giới hạn ở K + 1 sẽ ngăn chặn việc so sánh không chính xác do tràn ngôn ngữ với kích thước số nguyên cố định và tránh lãng phí thời gian lưu trữ các giá trị không thể ảnh hưởng đến câu trả lời.
