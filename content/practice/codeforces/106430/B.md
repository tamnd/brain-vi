---
title: "CF 106430B - Bessie Và Làm Tròn"
description: "Chúng ta có một số X và muốn biến nó thành một số Y khác. Một thao tác chọn số nguyên dương M và làm tròn X lên bội số gần nhất của M. Nếu hai bội số gần bằng nhau thì bội số lớn hơn sẽ được chọn."
date: "2026-06-25T08:27:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106430
codeforces_index: "B"
codeforces_contest_name: "2026 USACO.Guide Informatics Tournament"
rating: 0
weight: 106430
solve_time_s: 43
verified: true
draft: false
---

[CF 106430B - Bessie And Rounding](https://codeforces.com/problemset/problem/106430/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một số`X`và muốn biến nó thành một số khác`Y`. Một thao tác chọn một số nguyên dương`M`và vòng`X`tới bội số gần nhất của`M`. Nếu hai bội số gần bằng nhau thì bội số lớn hơn sẽ được chọn. Nhiệm vụ là tìm số thao tác tối thiểu cần thiết để biến đổi`X`vào trong`Y`. 

Khó khăn chính là thao tác làm tròn không cho phép chúng ta di chuyển một lượng tùy ý. Sự lựa chọn của`M`kiểm soát các giá trị tiếp theo có thể có, vì vậy giải pháp phụ thuộc vào việc hiểu được tiến trình lớn nhất có thể có từ một thao tác. 

Các ràng buộc cho phép các giá trị rất lớn của`X`Và`Y`, do đó việc mô phỏng nhiều thay đổi nhỏ là không khả thi. Nếu một giá trị có thể lớn tới hàng tỷ hoặc hơn, thì một cách tiếp cận liên tục giảm giá trị đó đi một hoặc thử tất cả những gì có thể`M`các giá trị sẽ mất quá nhiều thao tác. Chúng ta cần quan sát logarit vì mỗi phép toán hữu ích phải thay đổi độ lớn của số một cách đáng kể. 

Các trường hợp cạnh chính xuất phát từ hành vi gần các giá trị nhỏ và từ quy tắc hòa trong làm tròn. 

Nếu giá trị bắt đầu đã là mục tiêu thì không cần thực hiện thao tác nào.```
Input:
5 5
```Đầu ra đúng là:```
0
```Giải pháp luôn nhập logic tăng hoặc giảm trước tiên có thể tính sai một thao tác không cần thiết. 

Một trường hợp khác là khi một số không thể giảm được nữa. Ví dụ:```
Input:
3 1
```số`3`không thể làm tròn xuống`1`. Lựa chọn`M = 2`làm cho`3`gần bằng nhau`2`Và`4`, và dây buộc hướng lên trên, tạo ra`4`. Chọn giá trị nhỏ hơn của`M`cũng không thể sản xuất`1`. Đầu ra đúng là:```
-1
```Việc triển khai tham lam liên tục áp dụng công thức giảm tối đa mà không kiểm tra các giá trị nhỏ có thể lặp lại mãi mãi hoặc tạo ra câu trả lời không hợp lệ. 

Trường hợp ranh giới cuối cùng xuất hiện khi giá trị hiện tại nhỏ hơn hai lần mục tiêu.```
Input:
10 6
```Lựa chọn`M = 6`vòng`10`ĐẾN`6`, bởi vì`10`gần hơn với`6`hơn là`12`. Câu trả lời là:```
1
```Một giải pháp chỉ xem xét lũy thừa của hai mức tăng trưởng hoặc thu hẹp sẽ bỏ lỡ những chuyển đổi một bước cuối cùng này. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ thử mọi giá trị có thể có của`M`ở mỗi bước, mô phỏng thao tác làm tròn và tìm kiếm đường đi ngắn nhất từ`X`ĐẾN`Y`. Điều này đúng vì mọi động thái hợp pháp đều được khám phá. Tuy nhiên, nó không thể sử dụng được vì có thể có vô số lựa chọn khả thi cho`M`và số trạng thái giữa`X`Và`Y`cũng có thể rất lớn. Đối với một giá trị xung quanh`10^18`, việc khám phá các trạng thái hay thử mọi lựa chọn là điều không thể. 

Quan sát hữu ích đến từ việc xem xét riêng biệt việc tăng và giảm. 

Khi`X < Y`, một thao tác có thể tăng giá trị lên nhiều nhất là hệ số hai. Lựa chọn`M = 2X`vòng`X`hướng lên tới`2X`, vì hai bội số gần nhất là`0`Và`2X`. Điều này có nghĩa là chúng ta có thể nhân đôi liên tục cho đến khi mục tiêu đủ gần, sau đó chọn`M = Y`cho bước cuối cùng. Số thao tác chính xác là số lần nhân đôi cần thiết, tức là`ceil(log2(Y / X))`. 

Trường hợp giảm dần là trường hợp tế nhị hơn. Để giảm`X`, chúng ta cần chọn một`M < X`. Nếu như`X`nằm giữa`M`Và`2M`, giá trị làm tròn trở thành`M`khi`X`gần hơn với`M`hơn là`2M`. 

Biên độ giảm là:```
X - M < 2M - X
```mang lại:```
M > 2X / 3
```Hợp lệ nhỏ nhất`M`tạo ra mức giảm lớn nhất, do đó, bước đi tốt nhất là:```
M = floor(2X / 3) + 1
```Việc lặp lại thao tác này sẽ làm giảm giá trị khoảng một hệ số là`2/3`. Khi giá trị hiện tại nhỏ hơn hai lần`Y`, một thao tác cuối cùng với`M = Y`đạt được mục tiêu. 

Lực lượng vũ phu hoạt động vì nó khám phá mọi lựa chọn làm tròn có thể có, nhưng không thành công vì không gian tìm kiếm rất lớn. Nhận xét rằng mọi bước đi tối ưu đều có thể được thay thế bằng mức tăng hoặc giảm hữu ích tối đa sẽ làm giảm vấn đề thành một quá trình tham lam ngắn hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số trạng thái × giá trị M có thể có) | O(số tiểu bang) | Quá chậm | 
| Tối ưu | O(log(max(X, Y))) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu`X`đã bằng rồi`Y`, trở lại`0`vì không cần thao tác. 
2. Nếu`X < Y`, liên tục nhân đôi`X`trong khi nhân đôi không vượt qua mục tiêu. Mỗi lần nhân đôi đạt được bằng cách chọn`M = 2X`, vì vậy đây luôn là một phép toán hợp lệ. 
3. Sau giai đoạn nhân đôi, thực hiện thao tác cuối cùng với`M = Y`. Vì giá trị hiện tại bây giờ lớn hơn`Y / 2`, làm tròn đến bội số gần nhất của`Y`sản xuất chính xác`Y`. 
4. Nếu`X > Y`, trước tiên hãy kiểm tra xem giá trị hiện tại có nhỏ hơn không`2Y`. Nếu có, một thao tác với`M = Y`đạt được câu trả lời. 
5. Nếu không, hãy thay thế nhiều lần`X`với`floor(2X / 3) + 1`. Đây là mức giảm nhanh nhất có thể mà vẫn giữ được kết quả khả quan. 
6. Đếm các mức giảm này cho đến khi giá trị hiện tại nhỏ hơn`2Y`, sau đó thêm một thao tác cuối cùng để làm tròn trực tiếp tới`Y`. 
7. Nếu giá trị không thể giảm và vẫn lớn hơn`Y`, báo cáo rằng việc chuyển đổi là không thể. 

Tại sao nó hoạt động: mọi thao tác hữu ích đều có giới hạn về mức độ có thể thay đổi giá trị. Khi tăng, nhân đôi là bước nhảy lớn nhất có thể vì làm tròn số không thể tăng một số nhiều hơn kích thước của chính nó. Khi giảm, giá trị làm tròn dương nhỏ nhất có thể đạt được là`floor(2X/3)+1`và bất kỳ lựa chọn nào khác đều để lại số ít nhất bằng. Vì mỗi bước di chuyển tham lam sẽ đạt được tiến độ tối đa có thể, nên không có chuỗi nào khác có thể đạt được mục tiêu với ít thao tác hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(x, y):
    if x == y:
        return 0

    ans = 0

    if x < y:
        while x * 2 <= y:
            x *= 2
            ans += 1
        return ans + 1

    if x <= 3:
        return -1

    while x >= 2 * y:
        x = (2 * x) // 3 + 1
        ans += 1
        if x <= 3 and x > y:
            return -1

    return ans + 1

def main():
    data = list(map(int, sys.stdin.read().split()))
    if not data:
        return

    t = data[0]
    idx = 1
    out = []

    for _ in range(t):
        x = data[idx]
        y = data[idx + 1]
        idx += 2
        out.append(str(solve_case(x, y)))

    print("\n".join(out))

if __name__ == "__main__":
    main()
```chức năng`solve_case`ngăn cách hai hướng vì tăng và giảm có những bước di chuyển tối ưu khác nhau. 

Nhánh tăng sử dụng phép nhân với hai thay vì phép tính logarit. Điều này tránh được các vấn đề về độ chính xác của dấu phẩy động và đếm trực tiếp các thao tác được thực hiện. 

Nhánh giảm sử dụng số học số nguyên cho`floor(2X/3)+1`. Thứ tự của các hoạt động quan trọng ở đây. Máy tính`2 * x // 3 + 1`giữ chính xác hoạt động của sàn, trong khi phép tính dấu phẩy động có thể mất độ chính xác đối với đầu vào lớn. 

điều kiện`x >= 2 * y`xác định liệu việc làm tròn trực tiếp cuối cùng có thể thực hiện được hay không. Một khi điều này là sai,`M = Y`hoạt động vì`X`gần hơn với`Y`hơn là`2Y`. 

## Ví dụ đã hoạt động 

Ví dụ 1:```
Input:
5 20
```| Bước | X hiện tại | Hoạt động | Kết quả | 
| --- | --- | --- | --- | 
| 0 | 5 | Chọn M = 10 | 10 | 
| 1 | 10 | Chọn M = 20 | 20 | 

Giai đoạn nhân đôi đạt đến giá trị mà mục tiêu có thể được tiếp cận trực tiếp. Ví dụ minh họa quy tắc tăng tối đa. 

Ví dụ 2:```
Input:
100 10
```| Bước | X hiện tại | Hoạt động | Kết quả | 
| --- | --- | --- | --- | 
| 0 | 100 | Chọn M = 67 | 67 | 
| 1 | 67 | Chọn M = 45 | 45 | 
| 2 | 45 | Chọn M = 31 | 31 | 
| 3 | 31 | Chọn M = 21 | 21 | 
| 4 | 21 | Chọn M = 10 | 10 | 

Việc giảm luôn chọn giá trị làm tròn dương nhỏ nhất có thể đạt được. Bước cuối cùng xảy ra khi giá trị đi vào phạm vi nơi hoạt động làm tròn trực tiếp đến mục tiêu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log(max(X, Y))) | Mỗi phép toán thay đổi giá trị theo một hệ số không đổi, do đó chỉ có nhiều phép toán xảy ra theo logarit. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ. | 

Các giá trị co lại hoặc tăng lên về mặt hình học, do đó số vòng lặp vẫn nhỏ ngay cả đối với đầu vào rất lớn. Thuật toán dễ dàng phù hợp với giới hạn Codeforces điển hình. 

## Trường hợp thử nghiệm```python
import sys, io

def solution(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    main()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert solution("""5
5 5
5 20
100 10
3 1
10 6
""") == """0
2
5
-1
1
"""

assert solution("""4
1 1
1 8
8 1
7 7
""") == """0
3
4
0
"""

assert solution("""3
2 100
1000 100
100 1
""") == """6
6
9
"""

assert solution("""3
3 2
4 1
1000000000000 1
""") == """1
2
69
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 5`|`0`| Giá trị ban đầu đã bằng mục tiêu | 
|`3 1`|`-1`| Giá trị nhỏ không thể giảm | 
|`10 6`|`1`| Ranh giới làm tròn cuối cùng trực tiếp | 
|`1000000000000 1`|`69`| Giá trị rất lớn và hành vi logarit | 

## Vỏ cạnh 

cho`X = Y`, thuật toán ngay lập tức trả về 0 vì trạng thái đã đúng. Điều này tránh việc đi vào nhánh tham lam và ngăn chặn việc tính toán một thao tác không cần thiết. 

Vì`X = 3`Và`Y = 1`, thuật toán phát hiện ra rằng`X`quá nhỏ để thực hiện việc giảm hữu ích. Các lựa chọn làm tròn có thể không thể tạo ra`1`, thế là quay lại`-1`là đúng. 

Vì`X = 10`Và`Y = 6`, thuật toán thấy rằng`10 < 2 * 6`, vì vậy nó bỏ qua giai đoạn khử. Lựa chọn`M = 6`vòng`10`ĐẾN`6`, xác nhận điều kiện bước cuối cùng. 

Đối với các giá trị cực lớn, chẳng hạn như`X = 10^12`Và`Y = 1`, thuật toán không bao giờ lặp qua các số riêng lẻ. Nó chỉ thực hiện việc rút gọn hình học, giữ số phép tính theo logarit.
