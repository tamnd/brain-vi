---
title: "CF 102620G - Cúi đầu theo nhạc"
description: "Đoạn nhạc được thể hiện bằng một chuỗi các nốt nhạc. Mỗi nốt có thể đã có sẵn hướng cung bắt buộc, cung xuống (D) hoặc cung lên (U) hoặc có thể không được đánh dấu (B). Chúng ta phải quyết định hướng đi của tất cả các ghi chú chưa được đánh dấu."
date: "2026-07-31T03:29:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102620
codeforces_index: "G"
codeforces_contest_name: "mBIT Standard June 2020"
rating: 0
weight: 102620
solve_time_s: 140
verified: true
draft: false
---

[CF 102620G - Cú cúi đầu theo âm nhạc](https://codeforces.com/problemset/problem/102620/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đoạn nhạc được thể hiện bằng một chuỗi các nốt nhạc. Mỗi nốt có thể đã có hướng cung bắt buộc, hoặc là cúi xuống (`D`) hoặc cung lên (`U`) hoặc có thể không được đánh dấu (`B`). Chúng ta phải quyết định hướng đi của tất cả các ghi chú chưa được đánh dấu. Chuỗi cuối cùng không thể chứa bất kỳ`B`, và các dấu hiệu ban đầu không thể thay đổi được. 

Cúi chào móc nối xảy ra khi hai nốt lân cận sử dụng cùng một hướng. Mục đích không phải là tránh tất cả các động tác cúi chào móc câu, bởi vì điều đó có thể là không thể khi một số nốt cố định buộc chúng phải thực hiện. Thay vào đó, chúng ta cần chọn hướng cho các nốt không đánh dấu sao cho tổng số cặp liền kề bằng nhau càng nhỏ càng tốt. 

Kích thước đầu vào cho phép lên tới 1000 ghi chú. Một giải pháp thử mọi khả năng gán các ghi chú trống sẽ cần được xem xét tới$2^{1000}$khả năng, vượt xa những gì có thể chạy. Chúng ta cần một cách tiếp cận xử lý trình tự một cách trực tiếp, sử dụng thực tế là quyết định cho nốt tiếp theo chỉ phụ thuộc vào hướng đã chọn cho nốt trước đó. 

Những trường hợp phức tạp là những nơi mà sự lựa chọn tối ưu không hiển nhiên rõ ràng ở địa phương. Ví dụ: nếu đầu vào là:```
3
B D B
```câu trả lời đúng có thể là:```
U D U
```với hai sự thay đổi hướng và các cung không có móc. Một chiến lược tham lam luôn chọn một hướng đi khác với nốt trước đó có thể thất bại sau này vì một nốt cố định đi trước một số vị trí có thể buộc phải đưa ra lựa chọn sớm hơn tốt hơn. 

Một trường hợp khó khăn khác là khi mọi nốt nhạc đều đã được sửa:```
4
D D U U
```Đầu ra hợp lệ duy nhất là:```
D D U U
```với hai chiếc nơ có móc. Việc triển khai cố gắng sửa đổi các ghi chú cố định trong khi tìm kiếm mức tối thiểu sẽ tạo ra câu trả lời không hợp lệ. 

Trường hợp ranh giới cuối cùng là một ghi chú duy nhất:```
1
B
```Câu trả lời có thể là`U`hoặc`D`. Không có nốt liền kề nên số lần cúi móc tối thiểu là bằng không. Mã giả định mọi ghi chú đều có hàng xóm bên trái có thể bị lỗi ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là thay thế mọi`B`với một trong hai`U`hoặc`D`, đánh giá trình tự kết quả và giữ bài tập với ít lần cúi chào nhất. Điều này đúng vì nó kiểm tra mọi khả năng hoàn thành hợp lệ của bản nhạc. Tuy nhiên, nếu tất cả 1000 ghi chú đều trống thì số lượng bài tập là$2^{1000}$và mỗi nhiệm vụ yêu cầu quét toàn bộ chuỗi. Số lượng hoạt động là khoảng$1000 \times 2^{1000}$, điều đó là không thể. 

Cấu trúc của bài toán cho chúng ta một không gian trạng thái nhỏ hơn. Khi chọn hướng của nốt hiện tại, thông tin duy nhất từ ​​các ghi chú trước đó quan trọng là hướng của nốt ngay trước đó. Việc mười nốt đầu tiên chứa nhiều cung móc hay ít cung móc không thành vấn đề sau khi chúng ta biết giá tốt nhất kết thúc bằng`U`và chi phí tốt nhất kết thúc bằng`D`. 

Điều này dẫn đến lập trình động một cách tự nhiên. Đối với mọi vị trí, chúng tôi lưu trữ số lần cúi chào tối thiểu trong số tất cả các bài tập hợp lệ cho đến thời điểm đó, được phân tách bằng hướng của nốt cuối cùng. Khi xử lý nốt tiếp theo, chúng tôi thử cả hai hướng có thể nếu nó trống hoặc hướng duy nhất được phép nếu nó cố định. Quá trình chuyển đổi thêm một bất cứ khi nào hướng mới bằng hướng trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N × 2^N) | O(N) | Quá chậm | 
| Tối ưu | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo hai trạng thái lập trình động cho từng vị trí. Một tiểu bang lưu trữ số lần cúi chào tối thiểu trong số các bài tập kết thúc bằng`U`và cái còn lại lưu trữ cùng một giá trị cho các bài tập kết thúc bằng`D`. 

Sở dĩ chúng ta chỉ giữ hướng cuối cùng là vì nốt tiếp theo chỉ có thể tạo ra một cách cúi chào mới với nốt trước đó. 
2. Khởi tạo ghi chú đầu tiên. Nếu nó được cố định thành`U`, chỉ có`U`trạng thái là có thể. Nếu nó được cố định thành`D`, chỉ có`D`trạng thái là có thể. Nếu nó trống, cả hai trạng thái đều có thể thực hiện được với chi phí bằng 0. 
3. Xử lý các nốt còn lại từ trái sang phải. Đối với mỗi hướng có thể có của nốt hiện tại, hãy xem xét cả hai hướng có thể có của nốt trước đó. Thêm một vào chi phí khi hai hướng bằng nhau, vì điều đó tạo ra một đường cong móc câu. 
4. Lưu giá trị nhỏ hơn và ghi nhớ hướng trước đó đã tạo ra giá trị đó. Thông tin gốc được lưu trữ cho phép chúng ta xây dựng lại chuỗi thực tế sau khi tìm ra chi phí tối thiểu. 
5. Sau khi xử lý toàn bộ phần, chọn phần nhỏ hơn của phần cuối cùng`U`Và`D`tiểu bang. Làm theo hướng dẫn ngược lại của phụ huynh đã lưu để khôi phục câu trả lời hoàn chỉnh. 

Tại sao nó hoạt động: 

Bất biến quy hoạch động là sau khi xử lý vị trí`i`, mỗi trạng thái chứa số lượng móc nối tối thiểu có thể có trong số tất cả các phép gán hợp lệ của trạng thái đầu tiên`i + 1`ghi chú với hướng cuối cùng được chỉ định. Quá trình chuyển đổi xem xét mọi hướng có thể có của nốt hiện tại và mọi hướng kết thúc tối ưu có thể có của tiền tố trước đó. Vì mọi bài tập hoàn chỉnh phải kết thúc bằng một trong hai`U`hoặc`D`, lấy trạng thái cuối cùng tốt hơn sẽ mang lại giải pháp tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(s):
    n = len(s)

    inf = 10**9
    dp = [[inf, inf] for _ in range(n)]
    parent = [[-1, -1] for _ in range(n)]

    choices = []
    for c in s:
        if c == 'U':
            choices.append([0])
        elif c == 'D':
            choices.append([1])
        else:
            choices.append([0, 1])

    for c in choices[0]:
        dp[0][c] = 0

    for i in range(1, n):
        for cur in choices[i]:
            for prev in (0, 1):
                cost = dp[i - 1][prev] + (1 if cur == prev else 0)
                if cost < dp[i][cur]:
                    dp[i][cur] = cost
                    parent[i][cur] = prev

    last = 0 if dp[-1][0] <= dp[-1][1] else 1

    ans = [0] * n
    ans[-1] = last
    for i in range(n - 1, 0, -1):
        ans[i - 1] = parent[i][ans[i]]

    return ' '.join('U' if x == 0 else 'D' for x in ans)

def main():
    n = int(input())
    s = input().split()
    print(solve_case(s))

if __name__ == "__main__":
    main()
```các`choices`mảng chuyển đổi từng nốt thành tập hợp các hướng mà nó cho phép. Các nốt cố định có một hướng khả thi, trong khi các nốt trống có cả hai hướng. 

các`dp`bảng lưu trữ số lượng móc nối tốt nhất cho mọi vị trí. chỉ mục`0`đại diện cho`U`và chỉ số`1`đại diện cho`D`. Trong quá trình chuyển đổi, biểu thức`(1 if cur == prev else 0)`hoàn toàn phù hợp với định nghĩa của một cái cúi đầu móc. 

các`parent`bảng là cần thiết vì chỉ chi phí tối thiểu là không đủ. Sau khi kết thúc, chúng ta cần xuất ra một chuỗi thực tế. Bằng cách lưu trữ hướng trước đó tạo ra mọi trạng thái tối ưu, chúng ta có thể xây dựng lại câu trả lời mà không cần lưu trữ mọi chuỗi ứng cử viên. 

Không có mối quan tâm lớn về mặt số học vì số lượng móc nối tối đa chỉ là`N - 1`, vì vậy số nguyên Python bình thường là quá đủ. Nốt đầu tiên được khởi tạo riêng biệt vì nó không có nốt trước đó và không thể tạo ra một nốt móc nối. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu:```
7
B D B B B U U
```Một dấu vết lập trình động có thể là: 

| Vị trí | Lưu ý | Kết thúc hay nhất U | Kết thúc hay nhất D | Lựa chọn hướng đi cuối cùng | 
| --- | --- | --- | --- | --- | 
| 0 | B | 0 | 0 | | 
| 1 | D | INF | 0 | | 
| 2 | B | 1 | 1 | | 
| 3 | B | 1 | 2 | | 
| 4 | B | 2 | 2 | | 
| 5 | Bạn | 2 | INF | | 
| 6 | Bạn | 2 | INF | | 

Câu trả lời cuối cùng được xây dựng lại từ cha mẹ được lưu trữ là một bài tập tối ưu như:```
U D D U D U U
```Dấu vết cho thấy thuật toán không cực tiểu hóa từng cặp cục bộ một cách độc lập. Nó giữ cả hai kết thúc có thể xảy ra vì quyết định tốt hơn trong tương lai phụ thuộc vào hướng đi trước đó. 

Ví dụ thứ hai:```
4
D D U U
```dấu vết trở thành: 

| Vị trí | Lưu ý | Kết thúc hay nhất U | Kết thúc hay nhất D | Lựa chọn hướng đi cuối cùng | 
| --- | --- | --- | --- | --- | 
| 0 | D | INF | 0 | | 
| 1 | D | INF | 1 | | 
| 2 | Bạn | 1 | INF | | 
| 3 | Bạn | 1 | INF | | 

Đầu ra hợp lệ duy nhất là:```
D D U U
```Hai cặp liền kề bằng nhau là điều khó tránh khỏi. Trạng thái lập trình động bảo toàn chính xác các dấu cố định thay vì cố gắng loại bỏ các móc đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi ghi chú chỉ xem xét hai trạng thái trước đó và hai trạng thái hiện tại. | 
| Không gian | O(N) | Bảng cha lưu trữ một hướng trước đó cho mỗi trạng thái trong quá trình xây dựng lại. | 

Ràng buộc 1000 nốt dễ dàng phù hợp với giải pháp tuyến tính này. Ngay cả với dữ liệu tái tạo gốc, mức sử dụng bộ nhớ chỉ là vài nghìn số nguyên. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_case(s):
    n = len(s)
    inf = 10**9
    dp = [[inf, inf] for _ in range(n)]
    parent = [[-1, -1] for _ in range(n)]

    choices = []
    for c in s:
        if c == 'U':
            choices.append([0])
        elif c == 'D':
            choices.append([1])
        else:
            choices.append([0, 1])

    for c in choices[0]:
        dp[0][c] = 0

    for i in range(1, n):
        for cur in choices[i]:
            for prev in (0, 1):
                cost = dp[i - 1][prev] + (cur == prev)
                if cost < dp[i][cur]:
                    dp[i][cur] = cost
                    parent[i][cur] = prev

    last = 0 if dp[-1][0] <= dp[-1][1] else 1
    ans = [0] * n
    ans[-1] = last
    for i in range(n - 1, 0, -1):
        ans[i - 1] = parent[i][ans[i]]

    return ' '.join('U' if x == 0 else 'D' for x in ans)

def run(inp: str) -> str:
    data = inp.strip().split()
    n = int(data[0])
    return solve_case(data[1:1+n])

def hooked_count(out):
    a = out.split()
    return sum(a[i] == a[i+1] for i in range(len(a)-1))

assert len(run("1\nB\n").split()) == 1, "minimum size"

assert hooked_count(run("4\nD D U U\n")) == 2, "fixed notes"

assert hooked_count(run("3\nB D B\n")) == 0, "blank around fixed note"

assert hooked_count(run("5\nB B B B B\n")) == 0, "all equal freedom"

assert hooked_count(run("6\nU U U U U U\n")) == 5, "all fixed same direction"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / B`| Bất cứ hướng nào | Xử lý chính xác vị trí đầu tiên | 
|`D D U U`| Trình tự cố định tương tự | Giữ nguyên các dấu hiệu bắt buộc | 
|`B D B`| Không có móc nào có thể | Tránh những sai lầm tham lam của địa phương | 
| Năm nốt trống | Hướng thay thế | Sử dụng quyền tự do ở các vị trí trống | 
| Sáu`U`ghi chú | Sáu`U`ghi chú | Xử lý các móc không thể tránh khỏi | 

## Vỏ cạnh 

Đối với một ghi chú trống:```
1
B
```thuật toán khởi tạo cả hai trạng thái với chi phí bằng 0. Vì không có quá trình chuyển đổi nên một trong hai hướng đều tối ưu và việc xây dựng lại sẽ trả về một câu trả lời hợp lệ. 

Đối với các nốt cố định liên tiếp:```
4
D D U U
```thuật toán không bao giờ chèn các hướng thay thế vì danh sách lựa chọn cho mỗi vị trí chỉ chứa giá trị cố định. Quá trình chuyển đổi đếm hai cặp bằng nhau không thể tránh khỏi, tạo ra sự hoàn thành hợp lệ duy nhất. 

Đối với các khoảng trống xung quanh các nốt cố định:```
3
B D B
```chỗ trống đầu tiên có thể được chỉ định`U`và chỗ trống cuối cùng cũng có thể được chỉ định`U`, sản xuất:```
U D U
```Cả hai quá trình chuyển đổi đều diễn ra giữa các hướng khác nhau, vì vậy câu trả lời không có điểm nối nào. Các trạng thái lập trình động giữ cả hai khả năng ở mỗi bước, cho phép tìm ra lựa chọn tổng thể này.
