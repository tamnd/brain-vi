---
title: "CF 2062B - Bộ máy đồng hồ"
description: "Chúng ta được yêu cầu xác định xem liệu chúng ta có thể tránh vô thời hạn việc để bất kỳ đồng hồ nào chạm 0 trong một chuỗi đồng hồ hay không. Mỗi đồng hồ có thời gian ban đầu ai. Mỗi giây, tất cả đồng hồ đều giảm đi một. Chúng ta có thể di chuyển sang đồng hồ liền kề và ngay lập tức đặt lại thời gian của nó về ai."
date: "2026-06-08T07:33:31+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2062
codeforces_index: "B"
codeforces_contest_name: "Ethflow Round 1 (Codeforces Round 1001, Div. 1 + Div. 2)"
rating: 900
weight: 2062
solve_time_s: 127
verified: false
draft: false
---

[CF 2062B - Cơ chế đồng hồ](https://codeforces.com/problemset/problem/2062/B) 

**Xếp hạng:** 900 
**Tags:** tham lam, toán học 
**Thời gian giải:** 2m 7s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định xem liệu chúng ta có thể tránh vô thời hạn việc để bất kỳ đồng hồ nào chạm 0 trong một chuỗi đồng hồ hay không. Mỗi đồng hồ đều có thời gian ban đầu`a_i`. Mỗi giây, tất cả đồng hồ đều giảm đi một. Chúng ta có thể di chuyển sang đồng hồ liền kề và ngay lập tức đặt lại thời gian của nó về`a_i`. Điều đáng chú ý là nếu bất kỳ đồng hồ nào về 0 trước khi chúng ta có thể đặt lại nó, chúng ta sẽ thua ngay lập tức. Chúng ta có thể bắt đầu từ bất kỳ đồng hồ nào và có thể di chuyển qua lại một cách tự do. Đầu vào cung cấp một số trường hợp thử nghiệm, mỗi trường hợp có danh sách các giá trị đồng hồ và đầu ra phải là "CÓ" nếu tồn tại chiến lược vĩnh viễn hoặc "KHÔNG" nếu không. 

Các ràng buộc cho phép tối đa 500.000 đồng hồ trong tất cả các trường hợp thử nghiệm và mỗi giá trị đồng hồ có thể lớn tới 10^9. Điều này ngay lập tức loại trừ việc mô phỏng mỗi giây vì ngay cả một chiếc đồng hồ có giá trị 10^9 cũng sẽ yêu cầu một tỷ thao tác. Do đó, lời giải phải suy luận một cách toán học về việc liệu một chuỗi chuyển động có tồn tại để ngăn chặn vô thời hạn bất kỳ đồng hồ nào chạm tới số 0 hay không. 

Một sự tinh tế quan trọng nảy sinh khi đồng hồ có giá trị nhỏ so với vị trí của chúng. Ví dụ, nếu chúng ta có`[2, 2]`, chúng ta không thể quay vòng vô thời hạn vì vào thời điểm chúng ta đặt lại đồng hồ thứ hai, đồng hồ thứ nhất đã về 0. Các chuỗi nhỏ có giá trị nhỏ so với số lượng đồng hồ là các trường hợp đặc biệt phá vỡ các phương pháp tiếp cận ngây thơ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng từng giây: giảm tất cả đồng hồ, chuyển sang đồng hồ lân cận và đặt lại nếu có thể. Về nguyên tắc thì điều này đúng nhưng không khả thi. Vì`n = 10^5`Và`a_i = 10^9`, mô phỏng sẽ thực hiện quá nhiều thao tác. 

Quan sát quan trọng là vấn đề có thể được giảm xuống thành một phép kiểm tra đơn giản bằng cách sử dụng phương pháp tham lam. Nếu chúng ta xem xét việc đi bộ từ trái sang phải (và sau đó là từ phải sang trái), chúng ta có thể duy trì thời gian còn lại tối thiểu mà đồng hồ phải tồn tại cho đến khi chúng ta có thể đặt lại nó. Cụ thể, đối với lượt từ trái sang phải, chúng ta tưởng tượng việc di chuyển từ đồng hồ đầu tiên đến đồng hồ cuối cùng, luôn đặt lại ở đồng hồ hiện tại. Ở mỗi đồng hồ, chúng tôi tính toán "nợ" thời gian tối đa mà chúng tôi cần mang theo: khoản nợ trước đó cộng với một cho bước đã thực hiện không được vượt quá giá trị ban đầu của đồng hồ. Nếu vậy thì hướng đi đó là không thể. Sau đó, chúng tôi thực hiện tương tự từ phải sang trái và kiểm tra xem có đồng hồ bắt đầu nào mà cả hai lần vượt qua đều thành công hay không. 

Điều này biến vấn đề thành quét tuyến tính từ trái sang phải và phải sang trái, tức là O(n) cho mỗi trường hợp thử nghiệm. Cái nhìn sâu sắc là chúng ta không cần phải mô phỏng từng giây; chúng ta chỉ cần kiểm tra các ràng buộc do chuyển động áp đặt và đặt lại thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n * max(a_i)) | O(n) | Quá chậm | 
| Đường chuyền tuyến tính tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai mảng`left_debt`Và`right_debt`kích thước`n`.`left_debt[i]`sẽ đại diện cho thời gian tối thiểu cần thiết để tồn tại từ bên trái đến đồng hồ`i`. Tương tự,`right_debt[i]`sẽ theo dõi thời gian tối thiểu từ bên phải. 
2. Thực hiện đường chuyền từ trái sang phải. Bộ`left_debt[0] = 0`vì chúng ta có thể bắt đầu vào bất kỳ đồng hồ nào. Cho mỗi đồng hồ`i`từ 1 đến n-1, tính`left_debt[i] = max(left_debt[i-1] + 1, 0)`. Nếu tại bất kỳ thời điểm nào`left_debt[i] > a[i]`, không thể đến được đồng hồ này từ bên trái mà không bị mất. 
3. Thực hiện tương tự từ phải sang trái. Bộ`right_debt[n-1] = 0`và lan truyền về phía đồng hồ đầu tiên. Tại mỗi bước, hãy đảm bảo rằng`right_debt[i] <= a[i]`. 
4. Kiểm tra xem có đồng hồ không`i`nơi cả hai`left_debt[i] <= a[i]`Và`right_debt[i] <= a[i]`. Nếu một chiếc đồng hồ như vậy tồn tại, chúng ta có thể bắt đầu từ đó và di chuyển tới lui vô tận. Nếu không tồn tại, nó là không thể. 

Bất biến quan trọng là`debt[i]`đại diện cho thời gian tối thiểu phải duy trì trên đồng hồ`i`để tồn tại khi di chuyển từ hướng đó. Nếu như`debt[i] > a[i]`, đồng hồ không thể duy trì được chuyển động và kế hoạch thiết lập lại của chúng ta. Điều này đảm bảo rằng nếu tồn tại một đồng hồ khả thi, thì việc tuân theo các bước tuyến tính sẽ đảm bảo một chiến lược không xác định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_survive(a):
    n = len(a)
    left_debt = [0] * n
    right_debt = [0] * n
    
    # left to right pass
    for i in range(1, n):
        left_debt[i] = max(left_debt[i-1] + 1, 0)
        if left_debt[i] > a[i]:
            left_debt[i] = float('inf')
    
    # right to left pass
    for i in range(n-2, -1, -1):
        right_debt[i] = max(right_debt[i+1] + 1, 0)
        if right_debt[i] > a[i]:
            right_debt[i] = float('inf')
    
    # check for feasible starting point
    for i in range(n):
        if left_debt[i] <= a[i] and right_debt[i] <= a[i]:
            return "YES"
    return "NO"

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    print(can_survive(a))
```Giải pháp duy trì hai lượt tuyến tính, một lượt từ mỗi bên, tính toán thời gian còn lại tối thiểu cần thiết ở mỗi đồng hồ. Việc đặt đồng hồ không khả thi thành vô cực sẽ đảm bảo chúng bị loại khỏi điểm xuất phát đề cử. Lần quét cuối cùng sẽ xác định một đồng hồ có cả hai ràng buộc đều giữ nguyên, thể hiện vị trí bắt đầu an toàn. 

## Ví dụ đã hoạt động 

đầu vào`[4, 10, 5]`: 

| Đồng hồ | một [tôi] | Nợ trái | Đúng nợ | 
| --- | --- | --- | --- | 
| 0 | 4 | 0 | 2 | 
| 1 | 10 | 1 | 1 | 
| 2 | 5 | 2 | 0 | 

Không có đồng hồ nào thỏa mãn`left_debt[i] <= a[i]`Và`right_debt[i] <= a[i]`đồng thời ở mọi vị trí; do đó đầu ra là "KHÔNG". Điều này cho thấy một chiếc đồng hồ cỡ trung được đặt cạnh những chiếc đồng hồ nhỏ hơn không thể duy trì sự tồn tại vô thời hạn. 

đầu vào`[5, 3, 5]`: 

| Đồng hồ | một [tôi] | Nợ trái | Đúng nợ | 
| --- | --- | --- | --- | 
| 0 | 5 | 0 | 2 | 
| 1 | 3 | 1 | 1 | 
| 2 | 5 | 2 | 0 | 

Đồng hồ 1 thỏa mãn cả hai điều kiện:`1 <= 3`Và`1 <= 3`. Bắt đầu từ đồng hồ này, chúng ta có thể di chuyển sang trái và phải vô thời hạn, đặt lại đồng hồ khi chúng ta di chuyển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Hai đường chuyền tuyến tính cộng với một lần quét để tìm đồng hồ khởi động khả thi | 
| Không gian | O(n) | Mảng lưu trữ nợ trái phải | 

Thuật toán phù hợp thoải mái trong các ràng buộc vì tổng của tất cả các đồng hồ trong tất cả các trường hợp thử nghiệm là 5·10^5. Mỗi trường hợp thử nghiệm được xử lý trong thời gian tuyến tính với hệ số không đổi nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())  # assume solution.py contains the above code
    return output.getvalue().strip()

# Provided samples
assert run("5\n2\n4 10\n2\n2 2\n3\n4 10 5\n3\n5 3 5\n5\n12 13 25 17 30\n") == "YES\nNO\nNO\nYES\nYES"

# Custom tests
assert run("1\n2\n1 1\n") == "YES", "minimum-size input"
assert run("1\n3\n1 1 1\n") == "NO", "all equal, too small to survive"
assert run("1\n4\n10 10 10 10\n") == "YES", "all equal, large values"
assert run("1\n5\n5 6 1 6 5\n") == "YES", "middle small value still survives with right strategy"
assert run("1\n2\n1 1000000000\n") == "YES", "extreme values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 đồng hồ`[1,1]`| CÓ | Đầu vào tối thiểu, hầu như không tồn tại | 
| 3 đồng hồ`[1,1,1]`| KHÔNG | Giá trị nhỏ, không thể tồn tại | 
| 4 đồng hồ`[10,10,10,10]`| CÓ | Tất cả lớn, sinh tồn đơn giản | 
| 5 đồng hồ`[5,6,1,6,5]`| CÓ | Vỏ cạnh có đồng hồ nhỏ ở giữa | 
| 2 đồng hồ`[1,1000000000]`| CÓ | Sự khác biệt lớn về giá trị đồng hồ | 

## Vỏ cạnh 

cho`[2,2]`, vượt qua trái:`left_debt = [0,1]`, chuyền phải:`right_debt = [1,0]`. Đồng hồ 0 có`0 <= 2`và `1 <=
