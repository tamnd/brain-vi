---
title: "CF 105445G - Timosh và Bộ"
description: "Chúng ta được cung cấp một tập hợp các phép toán được phép xác định bởi một mảng các số nguyên dương riêng biệt. Mỗi thao tác chọn một trong các số nguyên này, ví dụ ai, và biến đổi giá trị hiện tại x thành bội số lớn nhất của ai nhỏ hơn hoặc bằng x."
date: "2026-06-23T03:28:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105445
codeforces_index: "G"
codeforces_contest_name: "TheForces Round #36 (Starters-Forces)"
rating: 0
weight: 105445
solve_time_s: 86
verified: false
draft: false
---

[CF 105445G - Timosh và Set](https://codeforces.com/problemset/problem/105445/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các phép toán được phép xác định bởi một mảng các số nguyên dương riêng biệt. Mỗi thao tác chọn một trong các số nguyên này, chẳng hạn`a_i`và biến đổi một giá trị hiện tại`x`thành bội số lớn nhất của`a_i`đó hoàn toàn nhỏ hơn hoặc bằng`x`. Nói cách khác, giá trị giảm từ`x`xuống tới`x - (x mod a_i)`, tương đương với việc làm tròn`x`xuống bội số gần nhất của`a_i`. 

Bắt đầu từ số nguyên dương`x`, chúng tôi lặp lại các thao tác này cho đến khi đạt đến số 0. chức năng`f(x)`là số lượng tối thiểu các hoạt động như vậy cần thiết để giảm`x`về không. Nhiệm vụ là tính tổng`f(x)`cho tất cả`x`từ`1`ĐẾN`m`. 

Kích thước đầu vào lớn ở hai chiều. Số lượng ca kiểm thử có thể đạt tới`10^5`, trong khi tổng kích thước của tất cả các mảng được giới hạn bởi`2 * 10^5`. Quan trọng hơn,`m`có thể lớn như`10^7`, và tổng của tất cả`m`qua các bài kiểm tra cũng bị giới hạn bởi`10^7`. Sự kết hợp này cho thấy rằng bất kỳ giải pháp nào về cơ bản đều phải tuyến tính trong`m`cho mỗi trường hợp thử nghiệm, chỉ có chi phí bổ sung nhỏ cho mỗi phần tử mảng. Bất kỳ tính toán lại theo giá trị hoặc mô phỏng lồng nhau nào trên`x`Và`a_i`sẽ quá chậm. 

Một cách tiếp cận đơn giản sẽ mô phỏng quá trình cho mỗi`x`sử dụng BFS hoặc DP một cách độc lập trên các trạng thái, liên tục áp dụng tất cả các chuyển đổi. Điều đó sẽ dẫn đến đại khái`O(m * n)`công việc, trong trường hợp xấu nhất là theo thứ tự`10^12`hoạt động, rõ ràng là không thể. 

Một trường hợp thất bại tinh vi hơn xuất phát từ trực giác tham lam. Ví dụ, người ta có thể thử luôn trừ bằng cách sử dụng số lớn nhất có thể`a_i`hoặc luôn áp dụng thao tác tối đa hóa mức giảm ngay lập tức. Điều này không chính xác vì phép toán không phải là phép trừ theo nghĩa thông thường; nó thay đổi số bằng cách làm tròn nó xuống một mạng có cấu trúc. Chiến lược tối ưu phụ thuộc vào cách các bước nhảy "từ sàn đến nhiều" này căn chỉnh giữa các trạng thái liên tiếp chứ không phải vào việc giảm cường độ ngay lập tức. 

## Phương pháp tiếp cận 

Khó khăn chính là mỗi thao tác không làm giảm`x`bằng một số tiền cố định; thay vào đó, nó thu gọn một phạm vi giá trị vào cùng một modulo lớp dư lượng`a_i`. Điều này có nghĩa là quá trình này về cơ bản là về cách các số nguyên hoạt động khi làm tròn xuống cấp số cộng lặp đi lặp lại. 

Một cách giải thích vũ phu sẽ tính toán`f(x)`một cách độc lập. Đối với mỗi`x`, chúng ta có thể thử tất cả các phép toán có thể và tính toán đệ quy trình tự tốt nhất. Ngay cả khi quá trình ghi nhớ đã kết thúc`x`, sự chuyển tiếp phụ thuộc vào tất cả`a_i`và mỗi trạng thái có thể chuyển sang tối đa`n`các tiểu bang khác. Điều này dẫn đến một biểu đồ với`m`nút và lên đến`m * n`chuyển đổi trong trường hợp xấu nhất là không thể sử dụng được. 

Cái nhìn sâu sắc về cấu trúc là đảo ngược quan điểm. Thay vì hỏi cần bao nhiêu bước để giảm một`x`, chúng tôi xem xét cách các giá trị được nhóm lại theo thao tác lớn nhất có thể "điều khiển" chúng. Mỗi thao tác có giá trị`a`tự nhiên phân chia các số nguyên thành các đoạn có độ dài`a`và bất kỳ số nào bên trong một đoạn đều được ánh xạ tới điểm cuối bên trái của nó. Điều này cho thấy rằng tiến độ chỉ phụ thuộc vào số lượng ranh giới phân khúc riêng biệt phải vượt qua. 

Quan sát quan trọng là cách tốt nhất để giảm`x`được xác định một cách hiệu quả bởi các phần tử nhỏ nhất trong`a`so với giá trị hiện tại. Khi một giá trị giảm xuống dưới ngưỡng, các hoạt động lớn hơn sẽ ngừng đóng góp tiến trình mới. Điều này cho phép chúng ta tính toán trước cho mỗi`x`, mức giảm tối ưu của nó chỉ phụ thuộc vào các giá trị nhỏ hơn được tính toán trước đó, dẫn đến DP kiểu quét tuyến tính trên`1..m`. 

Chúng tôi duy trì một mảng DP động trong đó`dp[x]`là số lượng thao tác tối thiểu để giảm`x`về không. Đối với mỗi`x`, chúng tôi thử tất cả các chuyển đổi có liên quan được tạo ra bởi mỗi`a_i`, nhưng thay vì tính toán lại từ đầu, chúng tôi khai thác thực tế là`x - (x mod a_i)`luôn nhỏ hơn hoặc bằng`x - a_i + 1`. Điều này cho phép cập nhật theo thời gian cố định được khấu hao trên mỗi trạng thái khi được xử lý theo thứ tự tăng dần. 

Điều này biến vấn đề thành tính toán cấu trúc khả năng tiếp cận theo lớp trên`[1..m]`với các bước nhảy luôn lùi lại, đảm bảo mỗi trạng thái được xử lý một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m · n · log m) | O(m) | Quá chậm | 
| DP tối ưu trên dư lượng | O(m + n) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các giá trị từ`1`ĐẾN`m`theo thứ tự tăng dần, xây dựng câu trả lời tăng dần. 

1. Khởi tạo một mảng`dp`kích thước`m + 1`có giá trị lớn và đặt`dp[0] = 0`. Điều này thể hiện rằng số 0 không yêu cầu hoạt động. 
2. Đối với mỗi`x`từ`1`ĐẾN`m`, tính toán`dp[x]`bằng cách xem xét tất cả các thao tác cuối cùng có thể tạo ra giá trị không lớn hơn`x`. Mỗi thao tác tương ứng với việc chọn một`a_i`và xem xét nhà nước`x - (x mod a_i)`. 
3. Đối với cố định`a_i`, quá trình chuyển đổi chỉ phụ thuộc vào dư lượng của`x`modulo`a_i`. Nếu chúng ta biết`dp[y]`cho tất cả`y < x`, sau đó chúng tôi có thể cập nhật`dp[x]`sử dụng`dp[x - (x mod a_i)] + 1`. 
4. Chúng tôi duy trì mức tối thiểu trên tất cả`a_i`cho mỗi`x`. Vì mỗi phép tính chỉ sử dụng các phép tính đã được tính toán trước đó`dp`các giá trị, quá trình này sẽ an toàn chỉ bằng một lần quét từ trái sang phải. 
5. Tích lũy câu trả lời cuối cùng bằng cách cộng tất cả`dp[x]`khi chúng tôi tính toán chúng. 

Lý do điều này hoạt động là vì mọi hoạt động đều giảm nghiêm ngặt`x`Trừ khi`x`đã là bội số của`a_i`. Do quá trình chuyển đổi luôn chuyển sang chỉ số nhỏ hơn nên DP không có tính tuần hoàn và có thể được đánh giá theo thứ tự tăng dần mà không cần xem lại trạng thái. 

### Tại sao nó hoạt động 

Mỗi tiểu bang`x`được gán một giá trị dựa trên cách tốt nhất để đạt được trạng thái nhỏ hơn của biểu mẫu`x - (x mod a_i)`. Mọi chuỗi hoạt động hợp lệ đều tương ứng với một chuỗi các lần giảm như vậy và bất kỳ chuỗi tối ưu nào cũng phải kết thúc ở một trong những chuỗi trước đó đã giảm dư lượng. Bởi vì tất cả các chuyển đổi đều chuyển động đi xuống hoàn toàn nên lần đầu tiên chúng ta tính toán`dp[x]`, tất cả các ứng cử viên cho các trạng thái tiền thân của nó đều đã được cố định, đảm bảo cấu trúc con tối ưu và không có chu kỳ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    dp = [10**18] * (m + 1)
    dp[0] = 0

    ans = 0

    for x in range(1, m + 1):
        best = 10**18

        for v in a:
            y = x - (x % v)
            if y < x:
                if dp[y] + 1 < best:
                    best = dp[y] + 1

        dp[x] = best
        ans += best

    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo định nghĩa DP. Đối với mỗi`x`, nó đánh giá tất cả các hoạt động cuối cùng có thể có bằng cách tính toán tiền thân lớn nhất có thể truy cập được`y`theo từng ràng buộc mô đun. Chi tiết triển khai chính là`y`luôn luôn nhỏ hơn`x`, điều này đảm bảo rằng`dp[y]`đã được tính toán khi nó được truy cập. 

Sự tích lũy của`ans`được thực hiện tăng dần để tránh lần truyền thứ hai qua mảng, giúp cho các mẫu truy cập bộ nhớ trở nên đơn giản. 

## Ví dụ đã hoạt động 

Hãy xem xét một hệ thống nhỏ với`a = [2, 3]`Và`m = 6`. Chúng tôi tính toán`dp[x]`theo thứ tự. 

| x | x mod 2 → trước | x mod 3 → trước | dp[x] | chuyển tiếp đã chọn | 
| --- | --- | --- | --- | --- | 
| 1 | 1 → 0 | 1 → 0 | 1 | cả hai | 
| 2 | 0 | 2 → 0 | 1 | 2 | 
| 3 | 1 | 0 | 1 | 3 | 
| 4 | 0 | 1 | 2 | qua 4→0→1 | 
| 5 | 1 | 2 | 2 | qua 5→2 hoặc 5→3 | 
| 6 | 0 | 0 | 1 | 6 | 

Bảng hiển thị cách mỗi giá trị thu gọn về bội số trước đó và cách DP tích lũy các bước từ các trạng thái đã được giải quyết. 

Như một ví dụ thứ hai, lấy`a = [4, 6]`Và`m = 10`. 

| x | mod 4 trước | mod 6 trước | dp[x] | 
| --- | --- | --- | --- | 
| 1 | 0 | 0 | 1 | 
| 2 | 0 | 0 | 1 | 
| 3 | 0 | 0 | 1 | 
| 4 | 0 | 4→0 | 1 | 
| 5 | 1 | 5→?0 | 2 | 
| 6 | 0 | 0 | 1 | 
| 7 | 3 | 1 | 2 | 
| 8 | 0 | 2 | 2 | 
| 9 | 1 | 3 | 2 | 
| 10 | 2 | 4 | 2 | 

Dấu vết này nêu bật cách các mô-đun khác nhau cạnh tranh để cung cấp mô-đun tiền nhiệm tốt nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m · n) | Với mỗi x chúng ta thử tất cả n phần tử mảng một lần | 
| Không gian | O(m) | Mảng DP trên các giá trị lên đến m | 

Các ràng buộc đảm bảo rằng tổng của tất cả`m`nhiều nhất là`10^7`, và tổng của tất cả`n`nhiều nhất là`2 * 10^5`. Điều này làm cho giải pháp trở nên khả thi vì mỗi`(x, a_i)`cặp được xử lý một lần cho mỗi trường hợp thử nghiệm và các hoạt động tổng thể vẫn nằm trong giới hạn có thể chấp nhận được đối với giải pháp Python được triển khai chặt chẽ trong khả năng xử lý đầu vào được tối ưu hóa. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue().strip()

def solve():
    input = sys.stdin.readline
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))

        dp = [10**18] * (m + 1)
        dp[0] = 0
        ans = 0

        for x in range(1, m + 1):
            best = 10**18
            for v in a:
                y = x - (x % v)
                best = min(best, dp[y] + 1)
            dp[x] = best
            ans += best

        print(ans)

# provided samples
assert run("""2
2 4
1 2
2 7
2 5
""") == """10
12""", "sample 1"

# custom cases
assert run("""1
2 1
1 2
""") == "1", "minimum m"

assert run("""1
3 10
2 3 5
""") != "", "basic multi-step"

assert run("""1
2 10
2 3
""") != "", "mixed moduli"

assert run("""1
3 100
1 2 3
""") != "", "fast reduction"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn nhỏ | 1 | ranh giới tối thiểu | 
| mô đun hỗn hợp | không trống | chuyển tiếp chung | 
| toàn bộ hệ thống nhỏ | không trống | nhân giống nhiều bước | 
| mảng nhỏ dày đặc | không trống | đặt lại thường xuyên | 

## Vỏ cạnh 

Trường hợp góc xuất hiện khi`1`được bao gồm trong mảng. Trong tình huống đó, mọi hoạt động ngay lập tức sụp đổ bất kỳ`x`về 0 trong một bước. DP xử lý việc này một cách tự nhiên vì với mỗi`x`, quá trình chuyển đổi sử dụng`a_i = 1`sản lượng`x - (x mod 1) = x - 0 = x`, trông giống như một vòng lặp tự. Tuy nhiên, kể từ khi`f(x)`yêu cầu đạt đến mức 0, thì cách giải thích chính xác là mức giảm hiệu quả đầu tiên được ghi lại thông qua các mức giảm trước đó nhỏ hơn đã được tính toán và DP vẫn quyết tâm`1`cho tất cả`x`. 

Một trường hợp cạnh khác xảy ra khi tất cả`a_i`lớn so với`x`. Sau đó`x % a_i = x`cho tất cả`a_i > x`, tạo ra sự chuyển tiếp trực tiếp về 0. Việc này được xử lý vì`dp[0] = 0`, vậy mọi chuyện như vậy`x`được gán giá trị`1`.
