---
title: "CF 102591C - \u041f\u0440\u043e\u0441\u043f\u0435\u043a\u0442 \u0441\u043e \u0441\u0432\u0435\u0442\u043e\u0444\u043e\u0440\u0430\u043c\u0438"
description: "Có (N) đèn giao thông được đặt dọc theo một đại lộ thẳng. Một số đèn bị hỏng, được biểu thị bằng 0 trong chuỗi, trong khi những đèn đang hoạt động được biểu thị bằng 1. Mỗi đội sửa chữa có thể sửa mọi đèn bên trong một đoạn liên tục."
date: "2026-08-02T06:33:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102591
codeforces_index: "C"
codeforces_contest_name: "\u041e\u0442\u043a\u0440\u044b\u0442\u0430\u044f \u043f\u0440\u0435\u0434\u043c\u0435\u0442\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u041c\u0423\u0418\u0422 \u043f\u043e \u0441\u043f\u043e\u0440\u0442\u0438\u0432\u043d\u043e\u043c\u0443 \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2020. \u0424\u0438\u043d\u0430\u043b\u044c\u043d\u044b\u0439 \u0442\u0443\u0440."
rating: 0
weight: 102591
solve_time_s: 366
verified: false
draft: false
---

[CF 102591C - \u041f\u0440\u043e\u0441\u043f\u0435\u043a\u0442 \u0441\u043e \u0441\u0432\u0435\u0442\u043e\u0444\u043e\u0440\u0430\u043c\u0438](https://codeforces.com/problemset/problem/102591/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 6s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

có\(N\)đèn giao thông đặt dọc theo một đại lộ thẳng. Một số trong số chúng bị hỏng, đại diện bởi`0`trong chuỗi, trong khi những chuỗi đang hoạt động được biểu thị bằng`1`. Mỗi đội sửa chữa có thể sửa chữa mọi đèn bên trong một đoạn liên tục. Một đội có thể được chọn hoặc bỏ qua và được phép chọn các đội bổ sung ngay cả khi công việc của họ trùng lặp với các đèn đã được sửa chữa. Nhiệm vụ là đếm xem có bao nhiêu nhóm con khác nhau sửa chữa mỗi đèn bị hỏng ít nhất một lần. 

Câu trả lời là không yêu cầu số lượng đội tối thiểu. Mọi tập hợp con có thể có đều quan trọng, bao gồm cả các tập hợp con chứa các nhóm không cần thiết. Hai tập hợp con khác nhau nếu chúng chứa các chỉ số nhóm khác nhau. 

Cả hai\(N\)Và\(M\)tối đa là 5000. Điều này loại trừ bất cứ điều gì thử tất cả các tập hợp con của các đội, bởi vì có thể có\(2^{5000}\)của họ. Một giải pháp xung quanh \(O(NM)\) có thể chấp nhận được vì\(25\)hàng triệu thao tác nằm trong tầm tay của Python, trong khi các cách tiếp cận như \(O(N^2M)\) sẽ quá gần với giới hạn. 

Nguồn gốc của sai lầm chính là quên rằng một đội có thể xuất phát chính xác ở vị trí hiện tại và vẫn sửa chữa được vị trí đó. Ví dụ:```
1
0
1
1 1
```Câu trả lời là`1`. Quá trình quét kiểm tra xem vị trí 1 có được đảm bảo hay không trước khi khoảng thời gian xử lý bắt đầu từ 1 sẽ từ chối nhóm hợp lệ duy nhất một cách không chính xác. 

Một trường hợp phức tạp khác là đèn làm việc không cần che phủ. Ví dụ:```
3
101
1
1 3
```Câu trả lời là`1`, vì đèn hỏng duy nhất ở vị trí 2. Nhóm che nó lại, mặc dù 2 vị trí còn lại đã ổn. Một giải pháp yêu cầu mọi vị trí phải được bảo vệ sẽ từ chối điều này một cách không chính xác. 

Trường hợp cuối cùng là việc chọn không có đội nào có thể hợp lệ khi không có đèn hỏng:```
3
111
2
1 1
2 2
```Câu trả lời là`4`, bởi vì mọi tập hợp con của hai đội đều hoạt động, kể cả tập hợp con trống. Bất kỳ cách tiếp cận nào giả định rằng phải chọn ít nhất một đội sẽ bỏ lỡ trường hợp này. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ liệt kê mọi tập hợp con của\(M\)các đội. Đối với mỗi tập hợp con, chúng ta có thể đánh dấu các đèn được bao phủ bởi tất cả các phân đoạn đã chọn và kiểm tra xem liệu mọi đèn bị hỏng có đạt được hay không. Điều này đúng vì mọi câu trả lời có thể đều được kiểm tra chính xác một lần. Tuy nhiên, số lượng tập hợp con là\(2^M\), điều này đã là không thể đối với\(M=5000\). 

Quan sát hữu ích là các khoảng trên một dòng có phần tóm tắt đơn giản. Khi quét con đường từ trái sang phải, thông tin duy nhất về các đội đã được chọn có ảnh hưởng đến tương lai là vị trí xa nhất mà họ hiện đạt được. Chúng tôi không cần biết đội nào đã tạo ra tin tức đó. 

Cho phép`dp[x]`thể hiện số cách chọn đội trong số những đội đã được xử lý sao cho điểm cuối bên phải tối đa trong số các đội được chọn là chính xác`x`. Trong khi xử lý các đội xuất phát ở vị trí`i`, việc chọn một nhóm như vậy sẽ giữ nguyên điểm cuối bên phải tối đa tương tự hoặc tăng nó lên điểm cuối bên phải tối đa của đội. Sau khi tất cả các đội xuất phát lúc`i`được xem xét, nếu ánh sáng ở`i`bị hỏng, tất cả các tiểu bang có phạm vi phủ sóng tối đa kết thúc trước`i`không hợp lệ và bị loại bỏ. 

Phương pháp brute-force hoạt động vì nó theo dõi mọi tập hợp con một cách rõ ràng. Nó thất bại vì có quá nhiều tập hợp con. Quan sát cho thấy chỉ vị trí được bảo vệ xa nhất hiện tại mới quan trọng sẽ nén tất cả các tập hợp con có cùng hành vi trong tương lai vào một trạng thái lập trình động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
|---|---|---|---| 
| Lực lượng vũ phu | \(O(2^M \cdot (N+M))\) | \(O(N)\) | Quá chậm | 
| Tối ưu | \(O(NM)\) | \(O(N)\) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ tất cả các đội được nhóm theo điểm cuối bên trái của họ. Quá trình quét xử lý các vị trí theo thứ tự tăng dần nên khi chúng ta đến vị trí`i`chúng ta chỉ cần xem xét các đội bắt đầu từ đó. 

2. Khởi tạo`dp[0] = 1`. Điều này thể hiện việc chưa chọn đội nào, nghĩa là vị trí được đảm bảo tối đa hiện tại là 0. 

3. Đối với mỗi đội`[i, r]`bắt đầu từ vị trí hiện tại, cập nhật trạng thái lập trình động như thể đội này không được chọn hoặc được chọn. Nếu phạm vi phủ sóng tối đa hiện tại nhỏ hơn`r`, việc chọn nhóm này sẽ chuyển trạng thái sang`r`. Nếu mức tối đa hiện tại đã ít nhất là`r`, việc chọn đội không làm thay đổi trạng thái, do đó trạng thái đó chỉ đơn giản là tăng gấp đôi. 

4. Sau khi tất cả các đội xuất phát tại`i`được xử lý, hãy kiểm tra xem đèn giao thông ở`i`bị hỏng. Nếu nó bị hỏng, chỉ những trạng thái có mức độ bao phủ tối đa ít nhất`i`là hợp lệ. Tất cả các trạng thái nhỏ hơn đều bị loại bỏ. 

5. Sau khi xử lý từng vị trí, tính tổng tất cả các trạng thái còn lại. Mỗi trạng thái còn lại tương ứng với một tập hợp con các đội đã che phủ mọi ánh sáng bị hỏng. 

Điều bất biến là sau khi kết thúc vị trí`i`, mọi trạng thái khác 0 trong`dp`thể hiện chính xác số lượng tập hợp con của nhóm đã xử lý chính xác tất cả các đèn bị hỏng cho đến`i`, được nhóm theo vị trí xa nhất mà các đội đó đảm nhiệm. Khi gặp đèn bị hỏng, việc loại bỏ các trạng thái không đạt tới nó sẽ loại bỏ chính xác các tập hợp con không hợp lệ và giữ lại mọi tập hợp con hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10 ** 9 + 7

def solve():
    n = int(input())
    s = input().strip()
    m = int(input())

    by_left = [[] for _ in range(n + 1)]
    for _ in range(m):
        l, r = map(int, input().split())
        by_left[l].append(r)

    dp = [0] * (n + 1)
    dp[0] = 1

    for i in range(1, n + 1):
        for r in by_left[i]:
            add = 0
            for j in range(r):
                add += dp[j]
            add %= MOD

            for j in range(r + 1, n + 1):
                dp[j] = (dp[j] * 2) % MOD

            dp[r] = (dp[r] * 2 + add) % MOD

        if s[i - 1] == '0':
            for j in range(i):
                dp[j] = 0

    print(sum(dp) % MOD)

if __name__ == "__main__":
    solve()
```Mảng`by_left`tránh việc liên tục tìm kiếm các đội xuất phát ở vị trí hiện tại. Mảng lập trình động có các chỉ số từ`0`ĐẾN`N`, trong đó chỉ số`x`có nghĩa là các đội được chọn hiện tại đạt được vị trí chính xác`x`. 

Bản cập nhật cho một đội kết thúc vào`r`là chi tiết triển khai chính. Các tiểu bang dưới đây`r`tất cả chuyển sang trạng thái`r`nếu đội được chọn. ít nhất là Hoa Kỳ`r`vẫn giữ nguyên chỉ số hiện tại vì đội này không cải thiện được vị trí được bảo vệ tối đa. Việc nhân với hai đối với các trạng thái lớn hơn sẽ dẫn đến việc lựa chọn tham gia hoặc bỏ qua đội. 

Mã thực hiện cập nhật theo cách không cần mảng thứ hai vì mọi trạng thái được thay đổi đều được xử lý theo giá trị cũ của nó. Vị trí lớn hơn`r`được nhân đôi, trong khi giá trị tại`r`được tính từ giá trị cũ của tất cả các vị trí nhỏ hơn cộng với giá trị cũ tại`r`. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
4
0000
2
1 4
2 3
```Các trạng thái quan trọng là: 

| Vị trí | Đội xử lý | Trạng thái dp khác 0 | 
|---|---|---| 
| Bắt đầu | không | dp[0] = 1 | 
| 1 | [1,4] | dp[4] = 1 | 
| 2 | [2,3] | dp[3] = 1, dp[4] = 2 | 
| 3 | không | dp[3] = 1, dp[4] = 2 | 
| 4 | không | dp[3] = 1, dp[4] = 2 | 

Tổng cuối cùng là`3`? Trên thực tế nhà nước`dp[3]`không hợp lệ sau vị trí 4 vì nó không che được đèn hỏng ở vị trí 4. Bước lọc sẽ loại bỏ nó, chỉ để lại hai tập hợp con hợp lệ. Những tập hợp con đó chỉ được chọn`[1,4]`và chọn cả hai đội. 

Một ví dụ thứ hai:```
3
101
1
1 3
```| Vị trí | Đội xử lý | Trạng thái dp khác 0 | 
|---|---|---| 
| Bắt đầu | không | dp[0] = 1 | 
| 1 | [1,3] | dp[3] = 1 | 
| 2 | không | dp[3] = 1 | 
| 3 | không | dp[3] = 1 | 

Đèn hỏng duy nhất là vị trí số 2 và đội được chọn sẽ đến được vị trí đó. Câu trả lời là`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | \(O(NM)\) | Mỗi bản cập nhật nhóm được quét nhiều nhất\(N\)tiểu bang. | 
| Không gian | \(O(N)\) | Chỉ có mảng lập trình động và các khoảng được nhóm lại được lưu trữ. | 

Với\(N,M \leq 5000\), trường hợp xấu nhất có khoảng 25 triệu thao tác trạng thái, phù hợp với các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    data = sys.stdin.read().split()
    ptr = 0

    n = int(data[ptr])
    ptr += 1
    s = data[ptr]
    ptr += 1
    m = int(data[ptr])
    ptr += 1

    by_left = [[] for _ in range(n + 1)]
    for _ in range(m):
        l = int(data[ptr])
        r = int(data[ptr + 1])
        ptr += 2
        by_left[l].append(r)

    MOD = 10 ** 9 + 7
    dp = [0] * (n + 1)
    dp[0] = 1

    for i in range(1, n + 1):
        for r in by_left[i]:
            add = sum(dp[:r]) % MOD
            for j in range(r + 1, n + 1):
                dp[j] = dp[j] * 2 % MOD
            dp[r] = (dp[r] * 2 + add) % MOD

        if s[i - 1] == '0':
            for j in range(i):
                dp[j] = 0

    ans = str(sum(dp) % MOD)
    sys.stdin = old_stdin
    return ans

assert run("""4
0000
2
1 4
2 3
""") == "2"

assert run("""1
0
1
1 1
""") == "1"

assert run("""3
101
1
1 3
""") == "1"

assert run("""3
111
2
1 1
2 2
""") == "4"

assert run("""2
00
1
1 1
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| Đầu vào mẫu | 2 | Khoảng chồng chéo cơ bản | 
| Một đèn hỏng với một khoảng thời gian chính xác | 1 | Ranh giới ở vị trí đầu tiên | 
| Đèn bị hỏng giữa đèn làm việc | 1 | Chỉ có vị trí bị hỏng mới quan trọng | 
| Không có đèn bị hỏng | 4 | Tập hợp con trống là hợp lệ | 
| Hai đèn hỏng không che phủ được | 0 | Thiếu vị trí yêu cầu bị từ chối | 

## Vỏ cạnh 

Đối với trường hợp biên thứ nhất:```
1
0
1
1 1
```Khoảng thời gian được xử lý trước khi kiểm tra vị trí 1. Trạng thái thay đổi từ`dp[0]=1`ĐẾN`dp[1]=1`, và đèn hỏng giữ nguyên trạng thái đó vì nó đạt đến vị trí 1. Câu trả lời vẫn là 1. 

Đối với trường hợp đèn làm việc xuất hiện giữa các đèn bị hỏng:```
3
101
1
1 3
```Quá trình quét không bao giờ kiểm tra vị trí 1 và 3 vì chúng đã được sửa chữa. Chỉ vị trí 2 loại bỏ các trạng thái không hợp lệ. Khoảng thời gian đạt đến vị trí thứ 2, vì vậy đội được chọn duy nhất vẫn hợp lệ. 

Đối với trường hợp tập hợp trống:```
3
111
2
1 1
2 2
```Không có vị trí nào loại bỏ một trạng thái. Mọi lựa chọn của hai đội đều tồn tại, vì vậy trạng thái cuối cùng sẽ tính cả bốn tập hợp con. Lập trình động đương nhiên bao gồm tập con trống vì nó bắt đầu bằng`dp[0]=1`.
