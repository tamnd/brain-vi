---
title: "CF 106077F - Sao Thổ"
description: "Chúng ta được cho một chuỗi mục tiêu t đại diện cho khẳng định mà Shani muốn chứng minh. Cô ấy có một chuỗi các chuỗi dữ liệu được ghi lại. Cô ấy có thể chọn bất kỳ tập hợp con nào của các chuỗi này, nhưng các chuỗi được chọn phải giữ nguyên thứ tự ban đầu khi nối."
date: "2026-06-25T12:11:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106077
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 9-17-25 Div. 2 (Beginner)"
rating: 0
weight: 106077
solve_time_s: 46
verified: true
draft: false
---

[CF 106077F - Sao Thổ](https://codeforces.com/problemset/problem/106077/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi mục tiêu`t`điều đó đại diện cho khẳng định mà Shani muốn chứng minh. Cô ấy có một chuỗi các chuỗi dữ liệu được ghi lại. Cô ấy có thể chọn bất kỳ tập hợp con nào của các chuỗi này, nhưng các chuỗi được chọn phải giữ nguyên thứ tự ban đầu khi nối. Ủy ban sau đó sẽ tìm kiếm mọi sự xuất hiện của`t`bên trong văn bản được nối cuối cùng, bao gồm cả những lần xuất hiện trùng nhau và Shani muốn tối đa hóa số lượng đó. 

Đầu vào chứa mẫu đích, theo sau là số chuỗi có sẵn và chính các chuỗi đó. Đầu ra là số lần lớn nhất có thể mà mẫu mục tiêu có thể xuất hiện sau khi chọn chuỗi con tốt nhất. 

Những hạn chế chính định hình giải pháp. Có tối đa 1000 chuỗi dữ liệu, mỗi chuỗi có độ dài tối đa là 100, trong khi độ dài mục tiêu tối đa là 20. Một giải pháp xung quanh`O(n^2)`có thể vượt qua số lượng chuỗi, nhưng thách thức thực sự là xử lý tác động của chuỗi nối. Chúng ta cần tránh lưu trữ toàn bộ chuỗi nối trước đó, vì nó có thể đạt tới độ dài 100000. Độ dài mục tiêu nhỏ cho thấy rằng chỉ thông tin liên quan đến mẫu mục tiêu mới có thể quan trọng. 

Các trường hợp đặc biệt xuất phát từ các kết quả trùng khớp vượt qua ranh giới của các chuỗi đã chọn. Giải pháp đếm các kết quả khớp bên trong mỗi chuỗi một cách độc lập sẽ không thành công. Ví dụ:```
Input
abc
2
a
bc
```Đầu ra đúng là:```
1
```Việc chọn cả hai chuỗi sẽ tạo ra`abc`, trong đó kết quả khớp sử dụng phần cuối của chuỗi đầu tiên và phần đầu của chuỗi thứ hai. Phương pháp chỉ quét từng chuỗi riêng biệt sẽ không xuất hiện. 

Một trường hợp phức tạp khác là các trận đấu trùng lặp. Coi như:```
Input
aa
1
aaa
```Đầu ra đúng là:```
2
```Các kết quả trùng khớp là các vị trí từ 1 đến 2 và 2 đến 3. Việc triển khai bất cẩn di chuyển qua văn bản trùng khớp thay vì cho phép chồng chéo sẽ chỉ được tính một. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là thử mọi tập hợp con có thể có của chuỗi, nối các chuỗi đã chọn và đếm số lần xuất hiện của mẫu. Điều này đúng vì mọi văn bản cuối cùng hợp lệ đều tương ứng với chính xác một tập hợp con. Tuy nhiên, có tới 1000 chuỗi nên có`2^1000`tập hợp con có thể. Ngay cả việc tạo ra các ứng cử viên là không thể. 

Quan sát tiếp theo là tương lai không phụ thuộc vào toàn bộ sự nối tiếp. Khi chúng ta nối thêm một chuỗi khác, các ký tự trước đó duy nhất có thể ảnh hưởng đến sự xuất hiện mới của chuỗi`t`là hậu tố của văn bản hiện tại có thể trở thành tiền tố của`t`. Đây chính xác là thông tin được duy trì bởi một máy tự động khớp chuỗi. 

Chúng ta có thể xây dựng một máy tự động KMP cho`t`. Trạng thái biểu thị độ dài của hậu tố dài nhất của văn bản được xử lý cũng là tiền tố của`t`. Bất cứ khi nào chúng tôi thêm một ký tự, máy tự động sẽ chuyển sang trạng thái mới và cho chúng tôi biết liệu kết quả khớp hoàn toàn có được tạo hay không. Bởi vì độ dài mẫu tối đa là 20 nên số lượng trạng thái rất nhỏ. 

Đối với mỗi chuỗi dữ liệu, chúng ta có thể tính toán trước tác động của nó trên mọi trạng thái tự động. Nếu chúng ta bao gồm chuỗi, chúng ta sẽ đạt được một số kết quả khớp và chuyển sang trạng thái khác. Nếu chúng ta bỏ qua nó, trạng thái và điểm số vẫn giữ nguyên. Điều này biến vấn đề thành một chương trình động trên các chuỗi và trạng thái tự động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * tổng chiều dài) | O(tổng chiều dài) | Quá chậm | 
| Tối ưu | O(n * | t | * tổng chiều dài chuỗi) | 

## Hướng dẫn thuật toán 

1. Xây dựng hàm tiền tố KMP cho chuỗi đích. Hàm tiền tố cho phép chúng ta nhanh chóng tìm thấy trạng thái tự động tiếp theo sau khi đọc một ký tự. 

Trạng thái chỉ lưu trữ bao nhiêu mục tiêu đã được khớp dưới dạng hậu tố. Điều này là đủ vì bất kỳ sự kiện nào xảy ra trong tương lai chỉ có thể phụ thuộc vào thông tin hậu tố này. 
2. Tạo bảng chuyển tiếp cho mọi chuỗi dữ liệu. Đối với mọi trạng thái KMP bắt đầu có thể xảy ra, hãy mô phỏng việc đọc toàn bộ chuỗi và ghi lại trạng thái kết thúc cũng như số lần mục tiêu xuất hiện trong quá trình mô phỏng. 

Việc làm này một lần sẽ ngăn chúng ta quét liên tục cùng một chuỗi trong quá trình lập trình động. 
3. Khởi tạo mảng lập trình động.`dp[state]`lưu trữ số lượng kết quả khớp tối đa có thể đạt được sau khi xử lý một số tiền tố của danh sách dữ liệu và kết thúc ở trạng thái tự động hóa đó. 

Trước khi chọn bất kỳ chuỗi nào, máy tự động ở trạng thái 0 với điểm 0. 
4. Xử lý từng chuỗi dữ liệu một. Đối với mỗi chuỗi, hãy sao chép các giá trị DP hiện tại vì việc bỏ qua chuỗi luôn được cho phép. Sau đó thử lấy chuỗi từ mọi trạng thái trước đó, áp dụng quá trình chuyển đổi được tính toán trước và thêm các kết quả khớp thu được. 

Bước này xem xét chính xác hai lựa chọn cho mỗi chuỗi: bỏ qua hoặc nối thêm nó. 
5. Câu trả lời là giá trị lớn nhất trong số tất cả các trạng thái cuối cùng. Trạng thái tự động kết thúc không thành vấn đề vì không có thêm ký tự nào được thêm vào. 

Điều bất biến là sau khi xử lý lần đầu tiên`i`dây, mỗi`dp[state]`đại diện cho số điểm tốt nhất có thể có trong số tất cả các lựa chọn từ các chuỗi khiến máy tự động ở trạng thái đó. Việc bỏ qua sẽ duy trì các khả năng hợp lệ hiện có, trong khi việc lấy một chuỗi sẽ xem xét mọi trạng thái có thể có trước đó và áp dụng hiệu quả chính xác của việc nối thêm nó. Vì đây là hai hành động duy nhất có thể thực hiện được nên tính bất biến được giữ cho mọi chuỗi được xử lý. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = input().strip()
    n = int(input())
    s = [input().strip() for _ in range(n)]

    m = len(t)

    pi = [0] * m
    for i in range(1, m):
        j = pi[i - 1]
        while j > 0 and t[i] != t[j]:
            j = pi[j - 1]
        if t[i] == t[j]:
            j += 1
        pi[i] = j

    def advance(state, ch):
        while state > 0 and t[state] != ch:
            state = pi[state - 1]
        if t[state] == ch:
            state += 1
        if state == m:
            return pi[m - 1], 1
        return state, 0

    trans = []
    for word in s:
        cur = []
        for start in range(m):
            state = start
            gain = 0
            for ch in word:
                state, add = advance(state, ch)
                gain += add
            cur.append((state, gain))
        trans.append(cur)

    dp = [-10**9] * m
    dp[0] = 0

    for word_id in range(n):
        ndp = dp[:]
        for state in range(m):
            if dp[state] < 0:
                continue
            nxt, gain = trans[word_id][state]
            if dp[state] + gain > ndp[nxt]:
                ndp[nxt] = dp[state] + gain
        dp = ndp

    print(max(dp))

if __name__ == "__main__":
    solve()
```Việc xây dựng hàm tiền tố là quá trình tiền xử lý KMP thông thường. Nó cho phép máy tự động khôi phục từ những chỗ không khớp mà không cần quét lại văn bản đã được xử lý. 

các`advance`hàm xử lý một ký tự. Nếu hoàn thành việc khớp hoàn toàn, máy tự động sẽ quay trở lại đường viền dài nhất của mẫu thay vì trạng thái 0. Đây là điều cho phép tính chính xác các kết quả trùng lặp. 

Bảng chuyển tiếp lưu trữ hiệu ứng của mọi chuỗi từ mọi trạng thái có thể. Số lượng trạng thái nhiều nhất là 20, vì vậy việc tính toán trước sẽ rẻ hơn so với việc quét liên tục các chuỗi bên trong DP. 

Bản cập nhật DP sử dụng một mảng được sao chép vì mọi chuỗi đều có thể bị bỏ qua. Lấy một chuỗi đọc từ mảng cũ và ghi vào mảng mới, tránh việc vô tình sử dụng lại cùng một chuỗi nhiều lần. Mức tối đa cuối cùng được sử dụng vì mọi trạng thái hậu tố đều được chấp nhận. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
t = lol

strings:
olo
lol
olo
```Dấu vết là: 

| Chuỗi đã xử lý | Trạng thái tốt nhất hiện nay | Điểm tối đa | 
| --- | --- | --- | 
| không | trạng thái 0: 0 | 0 | 
| ôi | trạng thái 3: 1 | 1 | 
| ôi, cười lớn | trạng thái 3: 2 | 2 | 
| cả ba | trạng thái 3: 3 | 3 | 

Máy tự động giữ đủ thông tin hậu tố để nhận ra`lol`hình thành khi các dây chạm vào nhau. 

Đối với mẫu thứ hai:```
t = ababac

strings:
abab
aba
abac
```Dấu vết là: 

| Chuỗi đã xử lý | Hành động | Điểm | 
| --- | --- | --- | 
| không | văn bản trống | 0 | 
| abab | lấy chuỗi đầu tiên | 0 | 
| abab + aba | lấy chuỗi thứ hai | 0 | 
| abab + aba + abac | lấy chuỗi thứ ba | 1 | 

Sự lựa chọn tốt nhất là nối chuỗi thứ nhất và thứ ba. Bảng cho thấy DP không tham lam lấy các chuỗi mà giữ tất cả các trạng thái tự động có thể có. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * m * L) | Mỗi chuỗi được mô phỏng từ mọi trạng thái KMP, trong đó`m`là chiều dài mẫu và`L`là độ dài chuỗi tối đa. | 
| Không gian | O(m^2 + m) | Bảng chuyển tiếp có`n * m`các mục trong quá trình triển khai và bản thân DP chỉ cần`m`tiểu bang. | 

Với`m <= 20`, máy tự động rất nhỏ. Mặc dù số lượng chuỗi là 1000, nhưng mỗi mô phỏng chuyển tiếp chỉ chạm vào khoảng 2000 ký tự, dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve_case(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = input().strip()
    n = int(input())
    s = [input().strip() for _ in range(n)]

    m = len(t)

    pi = [0] * m
    for i in range(1, m):
        j = pi[i - 1]
        while j > 0 and t[i] != t[j]:
            j = pi[j - 1]
        if t[i] == t[j]:
            j += 1
        pi[i] = j

    def advance(state, ch):
        while state > 0 and t[state] != ch:
            state = pi[state - 1]
        if t[state] == ch:
            state += 1
        if state == m:
            return pi[m - 1], 1
        return state, 0

    trans = []
    for word in s:
        cur = []
        for start in range(m):
            state = start
            gain = 0
            for ch in word:
                state, add = advance(state, ch)
                gain += add
            cur.append((state, gain))
        trans.append(cur)

    dp = [-10**9] * m
    dp[0] = 0

    for i in range(n):
        ndp = dp[:]
        for st in range(m):
            nxt, gain = trans[i][st]
            ndp[nxt] = max(ndp[nxt], dp[st] + gain)
        dp = ndp

    return str(max(dp)) + "\n"

assert solve_case("""lol
3
olo
lol
olo
""") == "3\n"

assert solve_case("""ababac
3
abab
aba
abac
""") == "1\n"

assert solve_case("""abc
2
a
bc
""") == "1\n"

assert solve_case("""aa
1
aaa
""") == "2\n"

assert solve_case("""x
4
x
x
a
x
""") == "3\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`abc`với`a`,`bc`|`1`| Trận đấu vượt qua ranh giới chuỗi | 
|`aa`với`aaa`|`2`| Trận đấu chồng chéo | 
|`x`với nhiều chuỗi |`3`| Lặp đi lặp lại một ký tự phù hợp | 
| Trường hợp mẫu | Kết quả đầu ra mẫu | Độ chính xác chung của DP | 

## Vỏ cạnh 

Trường hợp vượt biên:```
abc
2
a
bc
```DP xử lý đầu tiên`a`, để máy tự động ở trạng thái đại diện cho tiền tố phù hợp`a`. Khi`bc`được lấy, máy tự động tiếp tục từ trạng thái đó và hoàn thành`abc`, thêm một trận đấu. 

Đối với các kết quả trùng lặp:```
aa
1
aaa
```Sau lần đầu tiên`a`, máy tự động ghi nhớ một ký tự trùng khớp. thứ hai`a`hoàn thành một trận đấu, nhưng thay vì đặt lại hoàn toàn, nó lại rơi về đường viền của`aa`, cái nào khác`a`. thứ ba`a`tạo ra trận đấu thứ hai. 

Đối với các chuỗi giống hệt nhau lặp đi lặp lại:```
x
4
x
x
a
x
```DP có thể chọn chuỗi thứ nhất, thứ hai và thứ tư. Mỗi chuỗi được chọn sẽ thêm một lần xuất hiện. Chuỗi bị bỏ qua không ảnh hưởng đến trạng thái máy tự động, vì vậy câu trả lời cuối cùng là ba.
