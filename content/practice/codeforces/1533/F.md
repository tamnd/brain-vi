---
title: "CF 1533F - Phân vùng chuỗi nhị phân"
description: "Chúng tôi được cung cấp một chuỗi nhị phân. Đối với một giá trị cố định của $k$, một chuỗi con được coi là hợp lệ nếu ít nhất một trong hai số lượng ký tự nhỏ: hoặc nó chứa nhiều nhất $k$ số 0 hoặc nhiều nhất là $k$ số một."
date: "2026-06-10T16:23:48+07:00"
tags: ["codeforces", "competitive-programming", "*special", "binary-search", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1533
codeforces_index: "F"
codeforces_contest_name: "Kotlin Heroes: Episode 7"
rating: 0
weight: 1533
solve_time_s: 243
verified: true
draft: false
---

[CF 1533F - Phân vùng chuỗi nhị phân](https://codeforces.com/problemset/problem/1533/F) 

**Đánh giá:** - 
**Tags:** *đặc biệt, tìm kiếm nhị phân, tham lam 
**Thời gian giải:** 4m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi nhị phân. Đối với một giá trị cố định của$k$, một chuỗi con được coi là hợp lệ nếu ít nhất một trong hai số ký tự nhỏ: hoặc nó chứa nhiều nhất$k$số không hoặc nhiều nhất$k$những cái đó. 

Nhiệm vụ là chia chuỗi gốc thành số phần hợp lệ liền kề tối thiểu. Chúng ta phải tính toán mức tối thiểu đó cho mọi$k$từ$1$ĐẾN$n$, Ở đâu$n$là độ dài của chuỗi. 

Chiều dài có thể đạt tới$2 \cdot 10^5$. Bất kỳ giải pháp nào xử lý mọi$k$độc lập trong$O(n)$thời gian đã tốn rồi$O(n^2)$, vượt xa giới hạn. Tổng công việc phải ở gần$O(n \log n)$. 

Một điểm tinh tế là giá trị không dựa trên độ dài. Một đoạn dài vẫn có thể hợp lệ nếu một ký tự đủ hiếm. Ví dụ, với$k=1$, đoạn`0000001`hợp lệ vì nó chỉ chứa một`1`. 

Một sai lầm dễ mắc phải khác là cắt ngay khi vượt quá một số đếm$k$. Phân đoạn chỉ trở nên không hợp lệ khi cả hai số lượng vượt quá$k$. Vì$k=2$, chuỗi`00011`vẫn còn giá trị vì nó chỉ có hai cái. 

Bẫy thứ ba xuất hiện khi tính toán phần mở rộng hợp lệ tối đa. Giả định$k=1$và phân đoạn hiện tại bắt đầu ở đầu`00100010`. Tiền tố hợp lệ dài nhất không được xác định bởi ký hiệu đầu tiên vượt quá giới hạn. Một loại ký tự có thể đã vượt quá$k$, trong khi phần còn lại vẫn nằm trong giới hạn, cho phép phân đoạn tiếp tục. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ khắc phục giá trị của$k$, tính toán tất cả các ranh giới phân đoạn hợp lệ và chạy lập trình động hoặc phân vùng tham lam. Ngay cả khi chúng tôi quản lý việc này trong$O(n)$thời gian cho một$k$, lặp lại nó cho tất cả$n$giá trị mang lại$O(n^2)$, đại khái$4 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất. 

Quan sát quan trọng là đối với vị trí bắt đầu cố định$i$, đoạn hợp lệ dài nhất có thể được mô tả bằng cách sử dụng vị trí xuất hiện của số 0 và số 1. 

Cho phép:

-$p_0$là vị trí của$(k+1)$-st 0 xảy ra tại hoặc sau$i$. 
-$p_1$là vị trí của$(k+1)$-lần thứ nhất xảy ra vào lúc hoặc sau$i$. 

Một phân đoạn vẫn hợp lệ miễn là vẫn còn nhiều nhất một số đếm$k$. 

Nếu chúng ta dừng lại trước$p_0$, thì đoạn đó chứa nhiều nhất$k$số không. 

Nếu chúng ta dừng lại trước$p_1$, thì đoạn đó chứa nhiều nhất$k$những cái đó. 

Vì vậy, phân đoạn hợp lệ dài nhất sẽ kết thúc ngay trước khi cả hai điều kiện đều thất bại. Vị trí bắt đầu tiếp theo của nó là$$\text{next}(i)=\max(p_0,p_1).$$Khi chức năng nhảy này được biết đến, phân vùng tối ưu là tham lam: luôn lấy phân đoạn hợp lệ dài nhất. Bất kỳ phân đoạn đầu tiên ngắn hơn nào cũng chỉ có thể làm giảm các lựa chọn trong tương lai. 

Thử thách còn lại là tính toán câu trả lời cho tất cả$k$. 

Đối với nhỏ$k$, chỉ có khoảng$\sqrt n$những giá trị khác nhau. Chúng ta có thể xây dựng rõ ràng hàm nhảy cho mọi vị trí và tính toán câu trả lời bằng lập trình động. 

Đối với lớn$k$, mỗi đoạn dài nên câu trả lời có vài đoạn. Chúng ta có thể mô phỏng trực tiếp quá trình tham lam. Tổng chi phí trên tất cả lớn$k$trở thành một chuỗi điều hòa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n\sqrt n + n\log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ vị trí của tất cả các số 0 và tất cả các số 1. 
2. Đối với mọi chỉ số$i$, tính: 

- có bao nhiêu số 0 xuất hiện trước$i$, 
- có bao nhiêu cái xuất hiện trước đó$i$. 

Những cấp bậc này cho phép chúng ta nhảy thẳng tới$(k+1)$-lần xuất hiện thứ nhất của một trong hai ký tự sau$i$. 
3. Chọn ngưỡng$B \approx \sqrt n$. 
4. Đối với mọi$k \le B$: 

Tính toán$$\text{next}[i] = \max( \text{pos0}[r_0(i)+k], \text{pos1}[r_1(i)+k] ),$$trong đó những lần xuất hiện không tồn tại được coi là vị trí$n+1$. 
5. Tương tự$k$, tính toán$$dp[i]=1+dp[\text{next}[i]]$$từ phải sang trái. 

Đây$dp[i]$là số lượng phân đoạn tối thiểu cần thiết để bao gồm hậu tố bắt đầu từ$i$. 
6. Cửa hàng$dp[1]$như câu trả lời cho điều này$k$. 
7. Đối với mọi$k>B$, mô phỏng trực tiếp phân vùng tham lam. 

Bắt đầu ở vị trí$1$. 

Liên tục nhảy tới$$\max( \text{pos0}[r_0(i)+k], \text{pos1}[r_1(i)+k] )$$cho đến khi vị trí trở thành$n+1$. 
8. Đếm số lần nhảy đã được thực hiện. Số đó là câu trả lời cho việc này$k$. 

### Tại sao nó hoạt động 

Đối với một đoạn bắt đầu lúc$i$, cách duy nhất nó trở nên không hợp lệ là khi nó chứa nhiều hơn$k$số không và hơn thế nữa$k$những cái cùng một lúc. 

Khoảnh khắc đầu tiên khi số 0 vượt quá$k$là$(k+1)$-st 0 sau$i$. Khoảnh khắc đầu tiên khi số lượng vượt quá$k$là$(k+1)$-thứ nhất sau$i$. 

Trước khi đạt đến cả hai vị trí, số lượng ký tự ít nhất vẫn nằm trong giới hạn. Sau khi đạt đến vị trí lớn hơn trong hai vị trí, cả hai giới hạn đều đã bị vượt quá. Điều này xác định duy nhất phân đoạn hợp lệ dài nhất. 

Lấy phân đoạn hợp lệ dài nhất một cách tham lam là tối ưu vì mọi phân vùng hợp lệ phải kết thúc phân đoạn đầu tiên của nó không muộn hơn thời điểm này. Việc mở rộng đoạn đầu tiên càng xa càng tốt sẽ không bao giờ làm tăng số lượng các phần trong tương lai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    n = len(s)

    pos0 = [0]
    pos1 = [0]

    for i, c in enumerate(s, 1):
        if c == '0':
            pos0.append(i)
        else:
            pos1.append(i)

    zcnt = len(pos0) - 1
    ocnt = len(pos1) - 1

    pos0.extend([n + 1] * (n + 2))
    pos1.extend([n + 1] * (n + 2))

    r0 = [0] * (n + 2)
    r1 = [0] * (n + 2)

    z = o = 0
    for i in range(1, n + 1):
        r0[i] = z
        r1[i] = o
        if s[i - 1] == '0':
            z += 1
        else:
            o += 1

    r0[n + 1] = z
    r1[n + 1] = o

    B = int(n ** 0.5) + 1
    ans = [0] * (n + 1)

    nxt = [0] * (n + 2)
    dp = [0] * (n + 2)

    for k in range(1, min(B, n) + 1):
        for i in range(1, n + 1):
            p0 = pos0[r0[i] + k + 1]
            p1 = pos1[r1[i] + k + 1]
            nxt[i] = max(p0, p1)

        dp[n + 1] = 0
        for i in range(n, 0, -1):
            dp[i] = 1 + dp[nxt[i]]

        ans[k] = dp[1]

    for k in range(B + 1, n + 1):
        cur = 1
        cnt = 0

        while cur <= n:
            p0 = pos0[r0[cur] + k + 1]
            p1 = pos1[r1[cur] + k + 1]
            cur = max(p0, p1)
            cnt += 1

        ans[k] = cnt

    print(*ans[1:])

solve()
```Các mảng`r0`Và`r1`thứ hạng xuất hiện của cửa hàng. Nếu vị trí`i`là điểm bắt đầu của một đoạn, sau đó`r0[i] + k + 1`trỏ tới số 0 đầu tiên sẽ làm cho số 0 vượt quá`k`. Ý tưởng tương tự áp dụng cho những người. 

Đối với các giá trị nhỏ của`k`, chúng tôi xây dựng biểu đồ bước nhảy một cách rõ ràng và tính toán số lượng phân đoạn tối thiểu bằng cách sử dụng DP một chiều. Vì mỗi bước nhảy đều di chuyển về bên phải nên DP được đánh giá từ phải sang trái. 

Đối với các giá trị lớn của`k`, câu trả lời là nhỏ. Việc mô phỏng các bước nhảy tham lam trở nên rẻ hơn so với việc xây dựng một bảng DP đầy đủ. 

Lỗi thường gặp nhất là tra cứu sự xuất hiện. Chúng tôi cần$(k+1)$-lần xuất hiện thứ nhất sau vị trí bắt đầu, tương ứng với`rank + k + 1`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
00100010
```Vì$k=1$: 

| Bước | Bắt đầu hiện tại | Bắt đầu tiếp theo | 
| --- | --- | --- | 
| 1 | 1 | 7 | 
| 2 | 7 | 9 | 

Trả lời = 2. 

cho$k=2$: 

| Bước | Bắt đầu hiện tại | Bắt đầu tiếp theo | 
| --- | --- | --- | 
| 1 | 1 | 9 | 

Trả lời = 1. 

Điều này cho thấy ngày càng tăng$k$mở rộng từng phân đoạn hợp lệ và giảm số lượng các phần cần thiết. 

### Ví dụ 2 

đầu vào:```
1001011100
```Vì$k=1$: 

| Bước | Bắt đầu hiện tại | Bắt đầu tiếp theo | 
| --- | --- | --- | 
| 1 | 1 | 4 | 
| 2 | 4 | 8 | 
| 3 | 8 | 11 | 

Đáp án = 3. 

cho$k=2$: 

| Bước | Bắt đầu hiện tại | Bắt đầu tiếp theo | 
| --- | --- | --- | 
| 1 | 1 | 6 | 
| 2 | 6 | 11 | 

Trả lời = 2. 

Ví dụ này chứng minh rằng quy tắc phân đoạn dài nhất tham lam sẽ tự động tạo ra phân vùng tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n\sqrt n + n\log n)$| Bé nhỏ$k$được xử lý bởi DP, lớn$k$xử lý bằng mô phỏng chuỗi hài | 
| Không gian |$O(n)$| Mảng vị trí, cấp bậc, DP và mảng nhảy | 

Với$n \le 2 \cdot 10^5$, điều này thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    s = input().strip()
    n = len(s)

    pos0 = [0]
    pos1 = [0]

    for i, c in enumerate(s, 1):
        if c == '0':
            pos0.append(i)
        else:
            pos1.append(i)

    pos0.extend([n + 1] * (n + 2))
    pos1.extend([n + 1] * (n + 2))

    r0 = [0] * (n + 2)
    r1 = [0] * (n + 2)

    z = o = 0
    for i in range(1, n + 1):
        r0[i] = z
        r1[i] = o
        if s[i - 1] == '0':
            z += 1
        else:
            o += 1

    B = int(n ** 0.5) + 1
    ans = [0] * (n + 1)

    nxt = [0] * (n + 2)
    dp = [0] * (n + 2)

    for k in range(1, min(B, n) + 1):
        for i in range(1, n + 1):
            p0 = pos0[r0[i] + k + 1]
            p1 = pos1[r1[i] + k + 1]
            nxt[i] = max(p0, p1)

        dp[n + 1] = 0
        for i in range(n, 0, -1):
            dp[i] = 1 + dp[nxt[i]]

        ans[k] = dp[1]

    for k in range(B + 1, n + 1):
        cur = 1
        cnt = 0
        while cur <= n:
            p0 = pos0[r0[cur] + k + 1]
            p1 = pos1[r1[cur] + k + 1]
            cur = max(p0, p1)
            cnt += 1
        ans[k] = cnt

    return " ".join(map(str, ans[1:]))

assert run("00100010\n") == "2 1 1 1 1 1 1 1"
assert run("1001011100\n") == "3 2 2 2 1 1 1 1 1 1"

assert run("0\n") == "1"
assert run("11111\n") == "1 1 1 1 1"
assert run("01\n") == "1 1"
assert run("0101\n") == "2 1 1 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`|`1`| Chiều dài tối thiểu | 
|`11111`|`1 1 1 1 1`| Tất cả các ký tự đều bằng nhau | 
|`01`|`1 1`| Trường hợp ranh giới nhỏ | 
|`0101`|`2 1 1 1`| Mẫu xen kẽ và kiểm tra từng cái một | 

## Vỏ cạnh 

Hãy xem xét`0101`với$k=1$. Toàn bộ chuỗi không hợp lệ vì nó chứa hai số 0 và hai số 1. Việc thực hiện bất cẩn có thể cho phép nó một cách không chính xác. Thuật toán tìm số 0 thứ hai và số 0 thứ hai, chiếm vị trí lớn hơn và cắt trước khi cả hai số đếm vượt quá giới hạn. Câu trả lời trở thành`2`. 

Coi như`11111`. Mọi chuỗi con đều chứa các số 0 nên mọi chuỗi con đều hợp lệ bất kể$k$.
