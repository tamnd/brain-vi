---
title: "CF 102375E - \u0414\u0443\u043c\u0441\u043a\u0438\u0439 \u0440\u0435\u0433\u043b\u0430\u043c\u0435\u043d\u0442"
description: "Chúng tôi được cung cấp nhật ký theo trình tự thời gian của một phiên họp quốc hội. Mỗi sự kiện Thêm x có nghĩa là bên x giới thiệu một hóa đơn mới. Dự luật mới được đưa ra ngay lập tức trở thành dự luật đang được thảo luận, do đó dự luật đang được thảo luận trước đó bị đình chỉ."
date: "2026-08-15T17:54:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102375
codeforces_index: "E"
codeforces_contest_name: "\u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434 \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442\u0430 \u0421\u0435\u0432\u0435\u0440\u043e-\u0417\u0430\u043f\u0430\u0434\u0430 \u0420\u043e\u0441\u0441\u0438\u0438 \u0438 \u041c\u043e\u0441\u043a\u0432\u044b ICPC 2019"
rating: 0
weight: 102375
solve_time_s: 145
verified: false
draft: false
---

[CF 102375E - \u0414\u0443\u043c\u0441\u043a\u0438\u0439 \u0440\u0435\u0433\u043b\u0430\u043c\u0435\u043d\u0442](https://codeforces.com/problemset/problem/102375/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 25s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhật ký theo trình tự thời gian của một phiên họp quốc hội. Mọi`Add x`sự kiện có nghĩa là bữa tiệc đó`x`giới thiệu một dự luật mới. Dự luật mới được đưa ra ngay lập tức trở thành dự luật đang được thảo luận, do đó dự luật đang được thảo luận trước đó bị đình chỉ. Mọi`Vote x`sự kiện này có nghĩa là dự luật đang được thảo luận đã được bỏ phiếu và bên được ghi lại phải là bên đưa ra dự luật đó. Sau cuộc bỏ phiếu, cuộc thảo luận quay trở lại dự luật đã bị đình chỉ bên dưới nó. 

Đây chính xác là một kỷ luật ngăn xếp. MỘT`Add`đặt một hóa đơn mới lên trên tất cả các hóa đơn chưa hoàn thành. MỘT`Vote`phải bỏ hóa đơn hiện ở trên cùng. Bên viết trong sự kiện phải đồng ý với hóa đơn hàng đầu đó. Khi nhật ký kết thúc, không được có hóa đơn nào chưa hoàn thành nên ngăn xếp phải trống. 

Đầu vào chứa tối đa 1000 sự kiện. Nó đủ nhỏ để ngay cả một thuật toán bậc hai cũng có thể hoàn thành dễ dàng, nhưng cấu trúc của bài toán cho chúng ta một nghiệm tuyến tính chỉ với một ngăn xếp. Không cần thuật toán đồ thị, lập trình động hoặc bất kỳ tìm kiếm nào trên các đơn hàng có thể. Trong thực tế, thứ tự sự kiện hoàn toàn xác định trạng thái ngăn xếp. 

Trường hợp cạnh tranh đầu tiên là một cuộc bỏ phiếu trước khi bất kỳ dự luật nào được đưa ra. Ví dụ,```
1
Vote z
```phải sản xuất`No`. Không có dự luật hiện tại để bỏ phiếu, vì vậy việc thực hiện bất cẩn chỉ kiểm tra xem một số sự kiện trước đó có được sử dụng hay không`z`có thể chấp nhận một phiên không thể. 

Trường hợp thứ hai là bỏ phiếu cho dự luật bị đình chỉ thay vì dự luật hiện tại. Ví dụ,```
4
Add a
Add b
Vote a
Vote b
```sản xuất`No`. Sau đó`Add b`, hóa đơn`b`đang được thảo luận và lập hóa đơn`a`bị đình chỉ. Cuộc bỏ phiếu tiếp theo phải dành cho`b`, không`a`. Việc triển khai chỉ đơn thuần là kiểm tra xem`a`nằm trong số những hóa đơn chưa hoàn thành sẽ chấp nhận sai trình tự. 

Trường hợp cạnh thứ ba là việc sử dụng lặp đi lặp lại của cùng một bên. Ví dụ,```
6
Add a
Add a
Vote a
Vote a
Add b
Vote b
```sản xuất`Yes`. hai`a`các hóa đơn là các hóa đơn khác nhau mặc dù chúng có cùng nhãn đảng. Một ngăn xếp xử lý việc này một cách tự nhiên bởi vì mỗi`Add a`tạo ra một mục ngăn xếp riêng biệt khác. 

Trường hợp cạnh cuối cùng là một tờ tiền chưa hoàn thiện ở cuối. Ví dụ,```
1
Add a
```sản xuất`No`. Dự luật đã được đưa ra nhưng chưa bao giờ được biểu quyết nên phiên họp không thể kết thúc một cách hợp pháp. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu trực tiếp có thể liên tục xây dựng lại tập hợp các dự luật chưa hoàn thành trước mỗi cuộc bỏ phiếu. Bắt đầu từ đầu nhật ký, chúng tôi xử lý tất cả sớm hơn`Add`Và`Vote`sự kiện và xây dựng lại ngăn xếp hiện tại, sau đó kiểm tra đỉnh của nó để tìm phiếu bầu hiện tại. Điều này đúng vì ngăn xếp sau bất kỳ tiền tố nào hoàn toàn được xác định bởi tiền tố đó. 

Vấn đề với phiên bản này là công việc lặp đi lặp lại. Nếu có (K) sự kiện và chúng tôi xây dựng lại ngăn xếp cho mọi sự kiện thì sự kiện đầu tiên có thể yêu cầu một thao tác, hai thao tác thứ hai, v.v. Trong trường hợp xấu nhất việc này mất khoảng 

[ 
1 + 2 + \dots + K = O(K^2) 
] 

những hoạt động không cần thiết. 

Quan sát quan trọng là trạng thái sau tiền tố không cần phải được xây dựng lại. Chúng ta chỉ cần trạng thái được tạo ra bởi sự kiện ngay trước đó. MỘT`Add x`đẩy`x`, và một`Vote x`kiểm tra đỉnh hiện tại và sau đó bật nó lên. Bản thân ngăn xếp chính xác là mô hình toán học của quy tắc nghị viện. 

Brute-force hoạt động vì mọi tiền tố có thể được mô phỏng từ lịch sử sự kiện nhưng không thành công do tính toán lại cùng một lịch sử nhiều lần. Việc quan sát thấy rằng mỗi sự kiện chỉ thay đổi ngăn xếp theo một cách cục bộ cho phép chúng tôi duy trì trạng thái tăng dần trong một lần thực hiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(K2) | O(K) | Được chấp nhận với K ≤ 1000, nhưng chậm không cần thiết | 
| Tối ưu | O(K) | O(K) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một ngăn xếp trống. Nó đại diện cho tất cả các dự luật đã được đưa ra nhưng chưa nhận được phiếu bầu cuối cùng, với dự luật hiện đang được thảo luận ở trên cùng. 
2. Đọc các sự kiện từ trái sang phải. Thứ tự không thể sắp xếp lại vì nó là bản ghi theo trình tự thời gian của phiên họp. 
3. Đối với một`Add x`sự kiện, đẩy`x`lên ngăn xếp. Dự luật mới ngay lập tức làm gián đoạn cuộc thảo luận trước đó nên nó phải trở thành đỉnh cao mới. 
4. Đối với một`Vote x`sự kiện, trước tiên hãy kiểm tra xem ngăn xếp có trống không. Nếu nó trống, nghĩa là hiện tại không có dự luật nào đang được thảo luận nên không thể ghi nhật ký. 
5. Nếu ngăn xếp không trống, so sánh phần tử trên cùng của nó với`x`. Nếu chúng khác nhau, sự kiện sẽ không thể diễn ra vì chỉ có dự luật đang được thảo luận mới có thể được bỏ phiếu. Trở lại`No`. 
6. Nếu đỉnh bằng`x`, bật nó lên. Dự luật đó hiện đã kết thúc và dự luật bên dưới nó, nếu có, sẽ lại trở thành cuộc thảo luận tích cực. 
7. Sau khi tất cả các sự kiện đã được xử lý, hãy kiểm tra xem ngăn xếp có trống không. Một ngăn xếp trống có nghĩa là mọi dự luật được đưa ra đều nhận được phiếu bầu của nó. Một ngăn xếp không trống có nghĩa là ít nhất một hóa đơn còn lại chưa hoàn thành, vì vậy hãy quay lại`No`. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý bất kỳ tiền tố hợp lệ nào, ngăn xếp chứa chính xác các hóa đơn chưa hoàn thành theo thứ tự gián đoạn của chúng, với hóa đơn hiện đang được thảo luận ở trên cùng. MỘT`Add`duy trì sự bất biến này bằng cách đặt hóa đơn mới hoạt động lên trên. hợp lệ`Vote`bảo tồn nó bằng cách loại bỏ chính xác hóa đơn đang hoạt động và hiển thị hóa đơn bị đình chỉ trước đó. Nếu một cuộc bỏ phiếu đề cập đến bất kỳ điều gì khác ngoài phần trên cùng thì không có phiên hợp lệ nào có thể tạo ra sự kiện đó. Nếu ngăn xếp không trống ở cuối thì có nghĩa là một số cuộc thảo luận chưa kết thúc. Do đó, thuật toán chấp nhận chính xác các chuỗi sự kiện hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    k = int(input())
    stack = []

    for _ in range(k):
        event, party = input().split()

        if event == "Add":
            stack.append(party)
        else:
            if not stack or stack[-1] != party:
                print("No")
                return
            stack.pop()

    print("Yes" if not stack else "No")

if __name__ == "__main__":
    solve()
```các`stack`danh sách các cửa hàng thư của đảng cho tất cả các hóa đơn chưa hoàn thành. Mỗi lần xuất hiện của`Add`tạo một mục nhập ngăn xếp mới, ngay cả khi thư của bên đó đã có sẵn, vì hai hóa đơn do cùng một bên đưa ra vẫn là các hóa đơn riêng biệt. 

Vì`Vote`, việc kiểm tra trống phải diễn ra trước khi truy cập`stack[-1]`. Nếu không, việc bỏ phiếu làm sự kiện đầu tiên sẽ gây ra hoạt động lập chỉ mục không hợp lệ thay vì tạo ra`No`. 

Sự so sánh với`stack[-1]`xảy ra trước khi`pop`. Nếu các nhãn khác nhau, chúng tôi sẽ từ chối toàn bộ nhật ký ngay lập tức vì việc xóa bất kỳ phần tử nào thấp hơn sẽ vi phạm quy tắc gián đoạn. 

Cuối cùng, kiểm tra`not stack`xử lý yêu cầu rằng mọi cuộc thảo luận cuối cùng phải kết thúc. Số nguyên Python không xuất hiện trong thuật toán nên không có vấn đề tràn. Mỗi sự kiện được xử lý chính xác một lần. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là:```
4
Add a
Add b
Vote a
Vote b
```Những thay đổi trạng thái là: 

| Sự kiện | Xếp chồng trước | Hành động | Xếp chồng sau | 
| --- | --- | --- | --- | 
|`Add a`|`[]`| xô`a`|`[a]`| 
|`Add b`|`[a]`| xô`b`|`[a, b]`| 
|`Vote a`|`[a, b]`| hàng đầu là`b`, không khớp | từ chối | 
|`Vote b`| chưa đạt | chưa đạt | chưa đạt | 

Sau đó`Add b`, hóa đơn`b`là hóa đơn đang hoạt động. Cuộc bỏ phiếu cho`a`cố gắng hoàn thành dự luật bị đình chỉ trong khi dự luật mới hơn vẫn đang được thảo luận, vì vậy trình tự này là không thể. 

Thuật toán từ chối ngay lập tức và in`No`. 

### Mẫu 2 

Đầu vào là:```
8
Add z
Vote z
Add x
Add y
Add x
Vote x
Vote y
Vote x
```Dấu vết là: 

| Sự kiện | Xếp chồng trước | Hành động | Xếp chồng sau | 
| --- | --- | --- | --- | 
|`Add z`|`[]`| xô`z`|`[z]`| 
|`Vote z`|`[z]`| nhạc pop`z`|`[]`| 
|`Add x`|`[]`| xô`x`|`[x]`| 
|`Add y`|`[x]`| xô`y`|`[x, y]`| 
|`Add x`|`[x, y]`| xô`x`|`[x, y, x]`| 
|`Vote x`|`[x, y, x]`| nhạc pop`x`|`[x, y]`| 
|`Vote y`|`[x, y]`| nhạc pop`y`|`[x]`| 
|`Vote x`|`[x]`| nhạc pop`x`|`[]`| 

Mọi phiếu bầu đều khớp với đỉnh hiện tại và ngăn xếp trống ở cuối. Việc lặp đi lặp lại`x`nhãn không gây ra sự mơ hồ vì mỗi nhãn`Add x`đóng góp mục nhập ngăn xếp của riêng nó. 

Thuật toán in`Yes`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(K) | Mỗi sự kiện thực hiện một lượng công việc ngăn xếp không đổi | 
| Không gian | O(K) | Trong trường hợp xấu nhất tất cả K sự kiện có thể`Add`sự kiện trước khi bỏ phiếu | 

Với (K \le 1000), thuật toán tuyến tính nằm trong giới hạn. Ngay cả cách tiếp cận tái thiết bậc hai cũng đủ nhỏ cho giới hạn cụ thể này, nhưng mô phỏng ngăn xếp đơn giản hơn, nhanh hơn và thể hiện trực tiếp quy tắc đang được kiểm tra. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    k = int(input())
    stack = []

    for _ in range(k):
        event, party = input().split()

        if event == "Add":
            stack.append(party)
        else:
            if not stack or stack[-1] != party:
                print("No")
                return
            stack.pop()

    print("Yes" if not stack else "No")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue().strip()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

# Provided samples
assert run("""4
Add a
Add b
Vote a
Vote b
""") == "No", "sample 1"

assert run("""8
Add z
Vote z
Add x
Add y
Add x
Vote x
Vote y
Vote x
""") == "Yes", "sample 2"

assert run("""1
Vote z
""") == "No", "sample 3"

# Minimum-size input
assert run("""1
Add a
""") == "No", "unfinished single bill"

# A single complete bill
assert run("""2
Add z
Vote z
""") == "Yes", "single completed bill"

# All events use the same party
assert run("""6
Add a
Add a
Add a
Vote a
Vote a
Vote a
""") == "Yes", "nested bills from one party"

# Wrong nesting order
assert run("""6
Add a
Add b
Add c
Vote b
Vote c
Vote a
""") == "No", "vote must match the top"

# Maximum-size valid input
assert run("1000\n" + "\n".join(["Add a"] * 500 + ["Vote a"] * 500) + "\n") == "Yes", \
    "maximum-size valid sequence"

# Maximum-size invalid input
assert run("1000\n" + "\n".join(["Add a"] * 500 + ["Vote b"] + ["Vote a"] * 499) + "\n") == "No", \
    "maximum-size invalid sequence"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / Add a`|`No`| Kích thước tối thiểu và ngăn xếp cuối cùng chưa hoàn thành | 
|`2 / Add z, Vote z`|`Yes`| Phiên hợp lệ nhỏ nhất có thể | 
| Ba`Add a`theo sau là ba`Vote a`|`Yes`| Nhiều hóa đơn riêng biệt của cùng một bên | 
|`Add a, Add b, Add c, Vote b, ...`|`No`| Một dự luật bị đình chỉ không thể được bỏ phiếu trước dự luật đang có hiệu lực | 
| 500 lượt thêm theo sau là 500 phiếu bầu phù hợp |`Yes`| Kích thước đầu vào tối đa và ngăn xếp sâu | 
| 500`a`thêm theo sau là`Vote b`|`No`| Kích thước đầu vào tối đa và phát hiện sự không khớp ngay lập tức | 

## Vỏ cạnh 

### Bình chọn trước khi thêm bất kỳ 

cho```
1
Vote z
```ngăn xếp bắt đầu trống. Sự kiện này là một`Vote`, do đó thuật toán sẽ kiểm tra ngăn xếp trước khi nhìn vào đỉnh của nó. Vì không có hóa đơn hiện hành nên nó sẽ in`No`ngay lập tức. Điều này tránh được việc chấp nhận một sự kiện không thể xảy ra và cố gắng truy cập vào ngăn xếp trống. 

### Bỏ phiếu cho dự luật bị đình chỉ 

cho```
4
Add a
Add b
Vote a
Vote b
```ngăn xếp trở thành`[a, b]`sau sự kiện thứ hai. Sự kiện tiếp theo yêu cầu bỏ phiếu cho`a`, Nhưng`stack[-1]`là`b`. Thuật toán từ chối nhật ký mà không xuất hiện bất kỳ thứ gì. Điều này nắm bắt được quy tắc lồng ghép trung tâm: hóa đơn mới hơn phải hoàn thành trước khi hóa đơn bị gián đoạn có thể tiếp tục. 

### Nhãn đảng lặp đi lặp lại 

cho```
6
Add a
Add a
Vote a
Vote a
Add b
Vote b
```ngăn xếp phát triển như`[a]`,`[a, a]`,`[a]`,`[]`,`[b]`,`[]`. hai`a`các mục được coi là các hóa đơn riêng biệt mặc dù nhãn của chúng giống hệt nhau. Thuật toán không bao giờ cố gắng xác định một tờ tiền trên toàn cầu vì chỉ có vị trí hàng đầu mới quan trọng. 

###Cuối cùng thảo luận còn dang dở 

cho```
3
Add a
Add b
Vote b
```ngăn xếp cuối cùng là`[a]`. Cuộc bỏ phiếu cho`b`đã hợp lệ vì`b`là hóa đơn đang hoạt động, nhưng hóa đơn cũ hơn`a`cuộc thảo luận vẫn bị đình chỉ và chưa kết thúc. Do đó, việc kiểm tra độ trống cuối cùng sẽ in`No`. 

### Làm trống ngăn xếp sau mỗi hóa đơn hoàn thành 

cho```
6
Add a
Vote a
Add b
Vote b
Add c
Vote c
```ngăn xếp trở về trống sau mỗi cặp. Mỗi dự luật mới bắt đầu một cuộc thảo luận mới và không bao giờ có bất kỳ dự luật nào bị đình chỉ. Thuật toán chấp nhận vì mọi phiếu bầu đều khớp với đỉnh và ngăn xếp cuối cùng trống. 

### Cùng một bên có thể giới thiệu các hóa đơn lồng nhau 

cho```
4
Add x
Add x
Vote x
Vote x
```cả hai phiếu đều hợp lệ. đầu tiên`Vote x`bỏ tờ tiền bên trong ra, để lộ tờ tiền cũ hơn`x`dự luật, và lần bỏ phiếu thứ hai sẽ loại bỏ dự luật đó. Giải pháp chỉ lưu trữ một tập hợp các bên đang hoạt động sẽ mất đi sự khác biệt này, trong khi một ngăn xếp sẽ bảo toàn số lượng và thứ tự lồng của các hóa đơn chưa hoàn thành một cách tự nhiên.
