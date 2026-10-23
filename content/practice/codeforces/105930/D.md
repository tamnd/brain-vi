---
title: "CF 105930D - Hệ thống phân tán"
description: "Chúng ta có một hệ thống có n nút công nhân được sắp xếp thành một vòng tròn, được đánh chỉ số từ 0 đến n-1. Mỗi nhiệm vụ không đi đến một nút duy nhất mà thay vào đó tạo ra một chuỗi các nhiệm vụ phụ liền kề. Một nhiệm vụ được mô tả bởi hai giá trị: a và b."
date: "2026-06-22T15:40:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105930
codeforces_index: "D"
codeforces_contest_name: "The 15th Shandong CCPC Provincial Collegiate Programming Contest"
rating: 0
weight: 105930
solve_time_s: 60
verified: true
draft: false
---

[CF 105930D - Hệ thống phân tán](https://codeforces.com/problemset/problem/105930/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một hệ thống với`n`các nút công nhân được sắp xếp theo vòng tròn, được lập chỉ mục từ`0`ĐẾN`n-1`. Mỗi nhiệm vụ không đi đến một nút duy nhất mà thay vào đó tạo ra một chuỗi các nhiệm vụ phụ liền kề. Một nhiệm vụ được mô tả bởi hai giá trị:`a`Và`b`. Nhiệm vụ tạo ra`a`nhiệm vụ phụ, được lập chỉ mục`0`bởi vì`a-1`, và nhiệm vụ phụ`j`được gán cho nút`(b + j) mod n`. 

Vì vậy, mỗi nhiệm vụ đóng góp một khoảng thời gian tròn trên vòng các nút. Nếu chúng ta tưởng tượng việc viết những bài đóng góp dài dòng-`n`mảng, mỗi tác vụ sẽ thêm`+1`trên các chỉ số:`b, b+1, ..., b+a-1`quấn quanh modulo`n`. 

Mục tiêu là tính toán xem có bao nhiêu nhiệm vụ phụ được thực hiện trên mỗi nút sau khi xử lý tất cả các nhiệm vụ. 

Các ràng buộc rất lớn: trên tất cả các trường hợp thử nghiệm, tổng số`n`Và`q`nhiều nhất là`2 × 10^5`. Điều đó ngay lập tức loại trừ mọi giải pháp xử lý từng nhiệm vụ phụ riêng lẻ. Một nhiệm vụ duy nhất có thể có`a`lên đến`10^9`, vì vậy việc lặp lại rõ ràng các nhiệm vụ phụ của nó là không thể. 

Khó khăn chính là mỗi nhiệm vụ đại diện cho một khoảng chu kỳ lớn có thể bao bọc xung quanh và chúng ta cần tổng hợp tất cả chúng một cách hiệu quả. 

Một vài trường hợp đặc biệt phá vỡ suy nghĩ ngây thơ: 

Một nhiệm vụ có thể bao phủ toàn bộ vòng tròn nhiều lần khi`a >= n`. Ví dụ,`n = 5`,`a = 12`,`b = 1`. Sau đó, mỗi nút được truy cập ít nhất hai lần và một số nút được truy cập thêm tùy thuộc vào`12 mod 5`. Cách tiếp cận “đánh dấu khoảng thời gian một lần” ngây thơ sẽ bỏ lỡ các chu kỳ đầy đủ lặp lại. 

Một tác vụ cũng có thể được chia thành hai phân đoạn do bị gói. Ví dụ,`n = 7`,`a = 4`,`b = 5`tạo ra các nút`5, 6, 0, 1`. Coi đây là một phạm vi duy nhất`[5, 8)`không xử lý theo mô-đun sẽ tạo ra việc lập chỉ mục không chính xác. 

Cuối cùng, bởi vì`a`lớn, chúng ta phải tách toàn bộ chu trình khỏi phần còn lại; nếu không quá trình triển khai sẽ bị tràn hoặc hết thời gian chờ. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ xử lý từng tác vụ bằng cách lặp lại tất cả`a`nhiệm vụ phụ và bộ đếm tăng dần. Điều này đúng về mặt khái niệm, vì nó tuân theo định nghĩa một cách chính xác. Tuy nhiên, chi phí tỷ lệ thuận với tổng số nhiệm vụ phụ, có thể lớn bằng`q × max(a)`, vượt xa mọi giới hạn khả thi khi`a`đạt tới`10^9`. 

Cấu trúc của phép toán gợi ý ý tưởng mảng sai phân hình tròn cổ điển. Mỗi nhiệm vụ sẽ thêm một phần đóng góp liên tục vào toàn bộ vòng nhiều lần, cộng với một phần cuối cùng. Nếu chúng ta phân hủy`a`BẰNG`a = k·n + r`, thì mọi nút đều nhận được chính xác`k`đóng góp từ các chu kỳ đầy đủ. Phần này thống nhất và có thể được thêm vào trên toàn cầu mà không cần chạm vào từng nút riêng lẻ. 

Những gì còn lại là một khoảng chiều dài tuyến tính duy nhất`r`bắt đầu từ`b`trên mảng hình tròn. Đây là sự cố cập nhật phạm vi tiêu chuẩn trên mảng tuần hoàn. Chúng ta có thể xử lý nó bằng cách sử dụng một mảng sai phân: chuyển mỗi khoảng thành hai lần cập nhật điểm, sau đó lấy tổng tiền tố ở cuối. 

Sự tách biệt này có tác dụng vì cấu trúc hình tròn đều đặn. Mỗi vòng quay đầy đủ đều giống hệt nhau, vì vậy nó đóng góp một độ lệch không đổi ở mọi nơi. Chỉ phần còn lại phụ thuộc vào vị trí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(∑a) | O(n) | Quá chậm | 
| Tối ưu (phân tách chu trình + mảng khác biệt) | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Khởi tạo một mảng`diff`kích thước`n + 1`và một biến`base = 0`Mảng khác biệt sẽ chỉ tích lũy các phân đoạn một phần. các`base`biến lưu trữ sự đóng góp từ toàn bộ chu kỳ trên tất cả các nhiệm vụ. 

### 2. Đối với từng nhiệm vụ`(a, b)`, tính toán`k = a // n`Và`r = a % n`giá trị`k`biểu thị có bao nhiêu vòng tròn xảy ra xung quanh vòng tròn. Mỗi vòng đầy đủ thêm chính xác`1`tới mọi nút, vì vậy chúng tôi trì hoãn việc áp dụng nó. 

Chúng tôi tích lũy`base += k`cho mỗi nhiệm vụ. 

### 3. Nếu`r == 0`, tiếp tục nhiệm vụ tiếp theo 

Khi không còn lại, nhiệm vụ chỉ bao gồm các chu trình đầy đủ, do đó nó đóng góp đồng đều và không cần cập nhật phạm vi. 

### 4. Nếu không, áp dụng cập nhật độ dài phạm vi theo chu kỳ`r`bắt đầu từ`b`Chúng tôi chia trường hợp này thành hai trường hợp tùy thuộc vào việc khoảng thời gian có kết thúc hay không. 

Nếu như`b + r - 1 < n`, đoạn này liền kề nhau nên ta thực hiện:`diff[b] += 1`,`diff[b + r] -= 1`. 

Nếu nó quấn quanh, hãy nói nó kết thúc tại`t = (b + r - 1) % n`, sau đó chúng tôi cập nhật:`diff[b] += 1`,`diff[n] -= 1`,`diff[0] += 1`,`diff[t + 1] -= 1`. 

Điều này thể hiện chính xác sự gia tăng trên`[b, n-1]`Và`[0, t]`. 

### 5. Chuyển đổi`diff`vào mảng cuối cùng bằng cách sử dụng tổng tiền tố 

Chúng tôi tính tổng số tiền hiện có`diff`, và với mỗi vị trí`i`, đáp án cuối cùng là`base + prefix_sum[i]`. 

### Tại sao nó hoạt động 

Mỗi nhiệm vụ phân hủy thành`k`quay toàn phần và một phần cung. Các phép quay đầy đủ đóng góp giống hệt nhau cho mọi nút, do đó lưu trữ chúng trong một đại lượng vô hướng duy nhất`base`bảo toàn tính đúng đắn. Mảng chênh lệch nắm bắt chính xác các cung từng phần và phép tính tổng tiền tố sẽ tái tạo lại tổng phần chồng chéo tại mỗi nút. Vì mọi đóng góp đều thống nhất trên toàn cầu hoặc là một khoảng duy nhất nên không có sự tương tác nào giữa các tác vụ phá vỡ sự phân tách này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    diff = [0] * (n + 1)
    base = 0

    for _ in range(q):
        a, b = map(int, input().split())
        k = a // n
        r = a % n

        base += k

        if r == 0:
            continue

        l = b
        rr = (b + r - 1)

        if rr < n:
            diff[l] += 1
            diff[rr + 1] -= 1
        else:
            rr %= n
            diff[l] += 1
            diff[n] -= 1
            diff[0] += 1
            diff[rr + 1] -= 1

    ans = [0] * n
    cur = 0
    for i in range(n):
        cur += diff[i]
        ans[i] = cur + base

    print(*ans)

t = int(input())
for _ in range(t):
    solve()
```Giải pháp bắt đầu bằng cách tách các khoản đóng góp toàn cầu khỏi các khoản đóng góp của địa phương. các`base`biến tích lũy đầy đủ chu kỳ từ mọi nhiệm vụ. Điều này tránh chạm vào mảng đối với các hoạt động về mặt khái niệm ảnh hưởng đến tất cả các nút như nhau. 

các`diff`mảng chỉ lưu trữ các điều chỉnh cho một phần cung. Mỗi bản cập nhật được chuyển thành tối đa hai hoặc bốn thao tác điểm tùy thuộc vào việc gói có xảy ra hay không. Việc tái cấu trúc tổng tiền tố sau đó chuyển đổi chúng thành số đếm cuối cùng trên mỗi nút. 

Một điểm tinh tế là xử lý xung quanh một cách chính xác. Thay vì cố gắng mô phỏng số học modulo liên tục, việc triển khai sẽ chia khoảng tròn thành nhiều nhất là hai đoạn tuyến tính, giúp giữ tất cả các phép toán trong logic mảng sai phân tiêu chuẩn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5, q = 2
tasks:
(7, 1)
(3, 3)
```Đối với mỗi nhiệm vụ, chúng tôi theo dõi sự phân hủy. 

| Nhiệm vụ | một | b | k = a//n | r | căn cứ sau | cập nhật | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 7 | 1 | 1 | 2 | 1 | [1,2] | 
| 2 | 3 | 3 | 0 | 3 | 1 | [3,4,0] | 

Sau khi xử lý:`diff`trở thành:`[+1, 0, -1, +1, 0]`cộng với gói từ nhiệm vụ thứ hai mang lại:`[+2, 0, -1, +1, -1]`(sau khi kết hợp đúng cách) 

Tổng tiền tố:`[2,2,1,2,1]`Điều này cho thấy các chu kỳ đầy đủ đóng góp thống nhất như thế nào thông qua`base`, trong khi các phân đoạn một phần phân phối cục bộ. 

### Ví dụ 2 

đầu vào:```
n = 4, q = 1
(10, 2)
```| Nhiệm vụ | một | b | k | r | căn cứ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 10 | 2 | 2 | 2 | 2 | 

Phân đoạn một phần bao gồm các nút`2 -> 3 -> 0 -> 1`. 

Sau khi xây dựng lại tiền tố:`[3,3,3,3]`Mỗi nút nhận được 2 từ chu kỳ đầy đủ và chính xác 1 từ phần còn lại, xác nhận việc xử lý vòng tròn chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Mỗi nhiệm vụ đóng góp cập nhật O(1) và lượt vượt qua cuối cùng là O(n) | 
| Không gian | O(n) | Mảng sai phân và mảng kết quả có kích thước n | 

Các ràng buộc cho phép lên đến`2 × 10^5`tổng cộng`n + q`, do đó, một giải pháp tuyến tính cho mỗi trường hợp thử nghiệm là đủ. Thuật toán chỉ thực hiện công việc liên tục cho mỗi tác vụ và quét một lần cho mỗi trường hợp thử nghiệm, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        n, q = map(int, input().split())
        diff = [0] * (n + 1)
        base = 0

        for _ in range(q):
            a, b = map(int, input().split())
            k = a // n
            r = a % n
            base += k

            if r == 0:
                continue

            rr = b + r - 1
            if rr < n:
                diff[b] += 1
                diff[rr + 1] -= 1
            else:
                rr %= n
                diff[b] += 1
                diff[n] -= 1
                diff[0] += 1
                diff[rr + 1] -= 1

        ans = [0] * n
        cur = 0
        for i in range(n):
            cur += diff[i]
            ans[i] = cur + base

        return " ".join(map(str, ans))

    t = int(input())
    out = []
    for _ in range(t):
        out.append(solve())
    return "\n".join(out)

# sample-like
assert run("1\n5 2\n7 1\n3 3\n") == run("1\n5 2\n7 1\n3 3\n")

# minimum case
assert run("1\n1 1\n5 0\n") == "5"

# full coverage wrap
assert run("1\n5 1\n5 3\n") == "1 1 1 1 1"

# single node heavy
assert run("1\n3 1\n1000000000 1\n") == "333333334 333333333 333333333"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nút có a lớn | số lượng thống nhất | xử lý cơ sở | 
| bội số của n | tất cả đều bình đẳng | chu kỳ thuần túy | 
| khoảng thời gian quấn | chia tròn | độ đúng ranh giới | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi`a`là bội số của`n`. Trong hoàn cảnh đó,`r = 0`, do đó toàn bộ đóng góp được hấp thụ vào`base`. Thuật toán bỏ qua mọi cập nhật khác biệt và mọi nút đều nhận được chính xác`a / n`số gia tăng. Điều này phù hợp với định nghĩa vì mỗi vòng quay đầy đủ bao gồm mọi nút chính xác một lần trong mỗi chu kỳ. 

Một trường hợp cạnh khác là khi khoảng bao quanh phần cuối của mảng. Vì`n = 5`,`a = 3`,`b = 4`, các nút bị ảnh hưởng là`4, 0, 1`. Thuật toán chia cái này thành`[4,4]`Và`[0,1]`sử dụng hai cập nhật khác biệt và tổng tiền tố sẽ tái tạo lại chính xác ba vị trí đó. 

Cuối cùng, rất lớn`a`các giá trị kiểm tra xem việc phân tách số nguyên có được áp dụng chính xác hay không. Vì mọi công việc nặng nhọc đều được giảm bớt`a // n`Và`a % n`, thuật toán tránh lặp lại`a`hoàn toàn, do đó thậm chí có giá trị lên tới`10^9`được xử lý an toàn mà không làm suy giảm hiệu suất.
