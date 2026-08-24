---
title: "CF 102191E - Động tác rắn"
description: "Chuỗi di chuyển mô tả một bước đi trên lưới số nguyên vô hạn. Chúng ta bắt đầu từ một ô và mỗi ký tự di chuyển vị trí hiện tại một ô theo một trong bốn hướng chính."
date: "2026-08-23T23:34:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102191
codeforces_index: "E"
codeforces_contest_name: "PSUT Coding Marathon 2019"
rating: 0
weight: 102191
solve_time_s: 1739
verified: true
draft: false
---

[CF 102191E - Động tác của rắn](https://codeforces.com/problemset/problem/102191/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 28 phút 59 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chuỗi di chuyển mô tả một bước đi trên lưới số nguyên vô hạn. Chúng ta bắt đầu từ một ô và mỗi ký tự di chuyển vị trí hiện tại một ô theo một trong bốn hướng chính. Đối với một chuỗi con đã chọn, con rắn bắt đầu từ một ô bắt đầu mới và chỉ đi theo những bước di chuyển đó. Chuỗi con hợp lệ chính xác khi mọi ô được truy cập trong lần đi bộ đó khác với mọi ô trước đó. Chúng ta cần độ dài tối đa có thể có của chuỗi con như vậy. Bài toán chính thức sử dụng công thức tương tự và cho (n) tối đa (10^6). 

Một cách hữu ích để thể hiện bước đi là sử dụng các vị trí tiền tố. Đặt (P_i) là ô lưới đạt được sau lần di chuyển (i) đầu tiên của toàn bộ chuỗi, với (P_0) là ô bắt đầu. Một chuỗi con từ nước đi (l+1) đến nước đi (r) tương ứng với chuỗi các vị trí (P_l,P_{l+1},\ldots,P_r). Nó hợp lệ chính xác khi các vị trí tiền tố này đều khác biệt. Do đó, câu trả lời là giá trị lớn nhất của (r-l) mà khoảng vị trí tiền tố đó không chứa tọa độ trùng lặp. 

Với (n) lớn bằng (10^6), thuật toán có hành vi bậc hai hoặc bậc ba không thể phù hợp với giới hạn thời gian khoảng hai giây. Chẵn (O(n^2)) có nghĩa là khoảng (5\cdot10^{11}) lần lặp trong trường hợp xấu nhất, vượt xa những gì có thể được xử lý kịp thời. Về cơ bản, chúng ta cần kiểm tra chuỗi một số lần không đổi, điều này hướng tới nghiệm (O(n)). 

Có một số trường hợp đặc biệt có thể khiến việc triển khai bất cẩn trở nên sai lầm. Đầu tiên, một lần di chuyển luôn hợp lệ vì nó truy cập vào hai ô khác nhau. Đối với đầu vào`1`theo sau là`R`, câu trả lời là`1`. Việc triển khai chỉ kiểm tra xem vị trí hiện tại đã xuất hiện sau khi thực hiện một bước di chuyển có thể vô tình trả về 0 hay không. 

Trường hợp thứ hai là quay trở lại ô bắt đầu ngay lập tức. Vì`RL`, bước đi sẽ đi từ ô bắt đầu sang bên phải rồi quay lại ô bắt đầu, vì vậy câu trả lời là`1`, không`2`. Vị trí bắt đầu phải được coi là một ô đã được truy cập. 

Trường hợp thứ ba xảy ra khi vị trí lặp lại thuộc về phần cũ hơn của bước đi không còn nằm trong chuỗi con ứng viên hiện tại. Vì`RRLL`, các vị trí tiền tố là (0,1,2,1,0). Sau khi thấy vị trí lặp lại (1), cửa sổ hợp lệ sẽ bắt đầu sau lần xuất hiện trước đó. Sau đó, vị trí (0) được lặp lại, nhưng lần xuất hiện trước đó của nó thậm chí còn xa hơn về bên trái và nằm ngoài cửa sổ hiện tại. Việc triển khai cửa sổ trượt di chuyển ranh giới bên trái của nó về phía sau một cách mù quáng có thể tạo ra câu trả lời sai. Câu trả lời đúng là`2`, đến từ trận chung kết`LL`. 

Cuối cùng, các hướng lặp lại không hàm ý các ô lặp lại. Vì`RRRRR`, mỗi lần di chuyển đều đến một ô mới, vì vậy câu trả lời là`5`. Giải pháp theo dõi chỉ đường thay vì tọa độ thực tế sẽ từ chối trường hợp này một cách không chính xác. 

## Phương pháp tiếp cận 

Lực lượng vũ phu trực tiếp là khái niệm đơn giản. Chọn mọi vị trí bắt đầu có thể, sau đó mở rộng chuỗi con từng lần một trong khi vẫn duy trì một tập hợp các ô đã truy cập. Ngay khi một ô được truy cập hai lần, hãy dừng vị trí bắt đầu đó và tiếp tục với ô tiếp theo. Điều này đúng vì tập hợp này thể hiện rõ ràng mọi ô được chuỗi con ứng cử viên truy cập. 

Vấn đề là số lượng công việc lặp đi lặp lại. Với một mô phỏng mới cho mỗi chuỗi con, tổng số lần kiểm tra di chuyển trên một chuỗi không xảy ra xung đột là 

[ 
1+2+\cdots+n+\text{tất cả độ dài chuỗi con ngắn hơn} 
=\frac{n(n+1)(n+2)}{6}, 
] 

đó là các phép toán (\Theta(n^3)), khoảng (1.67\cdot10^{17}) khi (n=10^6). Ngay cả việc sử dụng lại tập hợp đã truy cập cho từng vị trí bắt đầu cũng chỉ cải thiện điều này thành (\Theta(n^2)), với (n(n+1)/2), khoảng (5\cdot10^{11}), các lần lặp trong một chuỗi hoàn toàn bên phải. Cả hai cách tiếp cận đều quá chậm. 

Quan sát quan trọng là bước đi của mỗi chuỗi con đã được mã hóa theo một chuỗi vị trí tiền tố duy nhất (P_0,P_1,\ldots,P_n). Một chuỗi con hợp lệ chính xác khi các vị trí tiền tố liên tiếp tương ứng khác nhau. Do đó, chúng tôi đã giảm vấn đề xuống còn tìm khoảng thời gian liền kề dài nhất của chuỗi này không chứa giá trị trùng lặp. 

Đó chính xác là cấu trúc được xử lý bởi một cửa sổ trượt. Duy trì ranh giới bên trái sao cho mọi vị trí tiền tố bên trong cửa sổ hiện tại là duy nhất. Khi vị trí mới đã xuất hiện trước đó bên trong cửa sổ, hãy di chuyển ranh giới bên trái qua vị trí trước đó của nó. Mỗi vị trí tiền tố vào cửa sổ một lần và rời khỏi cửa sổ nhiều nhất một lần, vì vậy toàn bộ quá trình là tuyến tính. 

Brute Force hoạt động vì nó kiểm tra xem mỗi bước đi ứng viên có chứa một ô lặp lại hay không nhưng không thành công vì nó liên tục tái tạo lại thông tin mà các chuỗi con lân cận chia sẻ. Biểu diễn vị trí tiền tố hiển thị thông tin được chia sẻ đó và cửa sổ trượt cho phép chúng tôi duy trì khoảng thời gian không trùng lặp dài nhất tăng dần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^3)) hoặc (O(n^2)) với việc sử dụng lại | (O(n)) | Quá chậm | 
| Tối ưu | (O(n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Coi ô ban đầu là vị trí tiền tố (P_0=(0,0)) và duy trì tọa độ hiện tại ((x,y)). Chúng ta sẽ xử lý các bước di chuyển từ trái sang phải và xây dựng (P_1,P_2,\ldots,P_n). 
2. Duy trì một cuốn từ điển`last`ánh xạ từng ô lưới đã truy cập vào chỉ mục xuất hiện gần đây nhất của nó trong chuỗi vị trí tiền tố. Ban đầu,`(0,0)`xảy ra tại chỉ mục`0`. 
3. Duy trì`left`, chỉ số tiền tố nhỏ nhất hiện được phép trong cửa sổ trượt. Tại mọi điểm, tất cả các vị trí (P_{\text{left}},\ldots,P_i) phải khác biệt. 
4. Đối với mỗi lần di chuyển, hãy cập nhật ((x,y)) vào ô lưới mới và tăng chỉ số tiền tố (i). Mã hóa tọa độ thành một số nguyên và tra cứu nó trong`last`. 
5. Nếu tọa độ đã xuất hiện trước đó tại chỉ mục`p`, kiểm tra xem`p >= left`. Nếu vậy, vị trí mới sẽ nhân đôi một ô hiện tại bên trong cửa sổ, vì vậy cách duy nhất để khôi phục tính duy nhất là đặt`left = p + 1`. Nếu như`p < left`, lần xuất hiện trước đó đã nằm ngoài cửa sổ hiện tại nên nó không được ảnh hưởng`left`. 
6. Lưu trữ chỉ mục hiện tại là lần xuất hiện mới nhất của tọa độ này. Việc cập nhật chỉ mục được lưu trữ là cần thiết vì bản sao trong tương lai sẽ được so sánh với lần xuất hiện gần đây nhất. 
7. Vị trí tiền tố từ`left`bởi vì`i`bao gồm`i - left + 1`các tế bào tương ứng với`i - left`di chuyển. Cập nhật câu trả lời với`i - left`. 
8. Sau khi xử lý từng nước đi, in ra giá trị lớn nhất ghi được trong đáp án. 

Điều bất biến là ngay sau mỗi lần lặp, chuỗi tiền tố-vị trí từ`left`thông qua chỉ mục hiện tại không chứa tọa độ trùng lặp và`left`là ranh giới nhỏ nhất khiến điều này trở thành sự thật. Khi một bản sao xuất hiện, mọi cửa sổ kết thúc ở chỉ mục hiện tại và bắt đầu tại hoặc trước lần xuất hiện trước đó của nó đều không hợp lệ, trong khi cửa sổ bắt đầu ngay sau lần xuất hiện đó là hợp lệ đối với bản sao mới được giới thiệu. Do đó di chuyển`left`ĐẾN`p+1`loại bỏ chính xác phần cần thiết của cửa sổ. Vì mỗi chuỗi con hợp lệ tương ứng với một khoảng vị trí tiền tố không trùng lặp, nên việc ghi lại độ dài cửa sổ lớn nhất sẽ tìm thấy câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def longest_valid(s):
    n = len(s)

    # Every coordinate lies in [-n, n].
    # Encode (x, y) into one integer to avoid the memory cost
    # of storing a tuple for every visited cell.
    base = 2 * n + 1

    x = 0
    y = 0
    key = n * base + n

    last = {key: 0}
    left = 0
    ans = 0

    for i, c in enumerate(s, 1):
        if c == 'R':
            x += 1
        elif c == 'L':
            x -= 1
        elif c == 'U':
            y += 1
        else:
            y -= 1

        key = (x + n) * base + (y + n)

        previous = last.get(key, -1)
        if previous >= left:
            left = previous + 1

        last[key] = i

        length = i - left
        if length > ans:
            ans = length

    return ans

def solve():
    n = int(input())
    s = input().strip()
    print(longest_valid(s))

if __name__ == "__main__":
    solve()
```các`longest_valid`chức năng là cửa sổ trượt hoàn chỉnh được mô tả ở trên. Từ điển bắt đầu với điểm gốc ở chỉ số tiền tố 0 vì con rắn không được phép quay lại ô bắt đầu trong chuỗi con đã chọn. 

Việc cập nhật tọa độ xảy ra trước khi tra cứu từ điển vì`last`lưu trữ các vị trí tiền tố và vị trí tiền tố (P_i) là ô đạt được sau khi thực hiện di chuyển (i). Việc liệt kê bắt đầu từ một, vì vậy chỉ mục của nó tự nhiên khớp với chỉ mục tiền tố. 

biểu thức`previous >= left`là điều kiện biên tới hạn. Lần xuất hiện trước đó`left`thuộc về một phần của chuỗi tiền tố đã bị loại khỏi chuỗi con hiện tại. Di chuyển`left`vì sự xuất hiện như vậy sẽ loại bỏ các nước đi hợp lệ một cách không chính xác. 

Câu trả lời là`i - left`, còn hơn là`i - left + 1`, vì cửa sổ chứa các vị trí tiền tố, trong khi câu trả lời được yêu cầu sẽ tính các bước di chuyển. Ví dụ: vị trí tiền tố (P_2,P_3,P_4) chứa ba ô nhưng chỉ biểu thị hai bước di chuyển giữa chúng. 

Mã hóa tọa độ sử dụng thực tế là sau nhiều nhất (n) lần di chuyển, cả hai tọa độ đều nằm trong khoảng (-n) và (n). yếu tố`base = 2*n + 1`làm cho mỗi cặp ánh xạ tới một số nguyên duy nhất. Sử dụng một số nguyên làm khóa từ điển về cơ bản sẽ tiết kiệm bộ nhớ hơn so với việc lưu trữ một bộ dữ liệu Python`(x, y)`cho tối đa một triệu vị trí, điều này quan trọng trong giới hạn bộ nhớ 256 MB. 

Số nguyên Python không bị chặn nên tọa độ được mã hóa không thể tràn. Giá trị được mã hóa lớn nhất chỉ theo thứ tự (n^2), có thể dễ dàng xử lý bằng cách biểu diễn số nguyên của Python. 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho`RULD`, các vị trí tiền tố có được bằng cách bắt đầu từ`(0, 0)`và áp dụng từng động tác một. 

| chỉ số tiền tố`i`| Di chuyển | Vị trí | Chỉ mục trước |`left`sau khi cập nhật | Độ dài hiện tại | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | bắt đầu |`(0,0)`| | 0 | 0 | 0 | 
| 1 | R |`(1,0)`| không | 0 | 1 | 1 | 
| 2 | Bạn |`(1,1)`| không | 0 | 2 | 2 | 
| 3 | L |`(0,1)`| không | 0 | 3 | 3 | 
| 4 | D |`(0,0)`| 0 | 1 | 3 | 3 | 

Ở tiền tố thứ 4, con rắn quay về nguồn gốc. Lần xuất hiện trước đó là ở chỉ mục 0, vì vậy cửa sổ phải bắt đầu ở chỉ mục một. Cửa sổ kết quả chứa ba bước di chuyển, tương ứng với chuỗi con hợp lệ`ULD`. Chuỗi bốn nước đi hoàn chỉnh không hợp lệ vì nó quay lại ô bắt đầu. 

### Mẫu 2 

cho`RRDDLLUUURDDR`, tọa độ tiến triển như sau. 

| chỉ số tiền tố`i`| Di chuyển | Vị trí | Chỉ mục trước |`left`sau khi cập nhật | Độ dài hiện tại | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | bắt đầu |`(0,0)`| | 0 | 0 | 0 | 
| 1 | R |`(1,0)`| không | 0 | 1 | 1 | 
| 2 | R |`(2,0)`| không | 0 | 2 | 2 | 
| 3 | D |`(2,1)`| không | 0 | 3 | 3 | 
| 4 | D |`(2,2)`| không | 0 | 4 | 4 | 
| 5 | L |`(1,2)`| không | 0 | 5 | 5 | 
| 6 | L |`(0,2)`| không | 0 | 6 | 6 | 
| 7 | Bạn |`(0,1)`| không | 0 | 7 | 7 | 
| 8 | Bạn |`(0,0)`| 0 | 1 | 7 | 7 | 
| 9 | Bạn |`(0,-1)`| không | 1 | 8 | 8 | 
| 10 | R |`(1,-1)`| không | 1 | 9 | 9 | 
| 11 | D |`(1,0)`| 1 | 2 | 9 | 9 | 
| 12 | D |`(1,1)`| không | 2 | 10 | 10 | 
| 13 | R |`(2,1)`| 3 | 4 | 9 | 10 | 

Mức tối đa xảy ra ở tiền tố chỉ số mười hai. Cửa sổ từ (P_2) đến (P_{12}), chứa 11 ô riêng biệt và do đó biểu thị 10 nước đi. Ở chỉ số thứ mười ba,`(2,1)`lặp lại vị trí từ chỉ số ba. Vì chỉ số 3 nằm trong cửa sổ hiện tại nên`left`nước đi còn bốn nước, thu hẹp ứng cử viên xuống còn chín nước đi. Câu trả lời vẫn là mười. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Mỗi bước di chuyển thực hiện một số lượng cập nhật tọa độ và thao tác từ điển không đổi. | 
| Không gian | (O(n)) | Nhiều nhất một mục từ điển được lưu trữ cho mỗi ô lưới được truy cập riêng biệt. | 

Đầu vào có thể chứa một triệu bước di chuyển, do đó việc xử lý tuyến tính là phù hợp. Thuật toán thực hiện một lần chuyển qua chuỗi và lưu trữ tối đa một triệu vị trí tiền tố. Mã hóa tọa độ số nguyên giữ cho biểu diễn từ điển nhỏ hơn đáng kể so với từ điển được khóa bằng hai bộ phần tử, làm cho phương pháp này phù hợp với giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def longest_valid(s):
    n = len(s)
    base = 2 * n + 1

    x = 0
    y = 0
    key = n * base + n

    last = {key: 0}
    left = 0
    ans = 0

    for i, c in enumerate(s, 1):
        if c == 'R':
            x += 1
        elif c == 'L':
            x -= 1
        elif c == 'U':
            y += 1
        else:
            y -= 1

        key = (x + n) * base + (y + n)

        previous = last.get(key, -1)
        if previous >= left:
            left = previous + 1

        last[key] = i
        ans = max(ans, i - left)

    return ans

def run(inp: str) -> str:
    data = inp.split()
    n = int(data[0])
    s = data[1]
    assert len(s) == n
    return str(longest_valid(s)) + "\n"

assert run("4\nRULD\n") == "3\n", "sample 1"
assert run("13\nRRDDLLUUURDDR\n") == "10\n", "sample 2"
assert run("3\nRRU\n") == "3\n", "sample 3"

assert run("1\nR\n") == "1\n", "minimum-size input"
assert run("2\nRL\n") == "1\n", "immediate return to the origin"
assert run("4\nRRRR\n") == "4\n", "all equal moves"
assert run("4\nRRLL\n") == "2\n", "duplicate outside the current window"
assert run("1000000\n" + "R" * 1000000 + "\n") == "1000000\n", "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / R`|`1`| Kích thước đầu vào tối thiểu và thực tế là một lần di chuyển luôn hợp lệ | 
|`2 / RL`|`1`| Quay lại ô bắt đầu phải bị từ chối | 
|`4 / RRRR`|`4`| Chỉ đường lặp đi lặp lại vẫn có thể truy cập các ô hoàn toàn khác nhau | 
|`4 / RRLL`|`2`| Vị trí lặp lại bên ngoài cửa sổ đang hoạt động không được di chuyển`left`lạc hậu | 
|`1000000 / RRR...R`|`1000000`| Kích thước đầu vào tối đa và hiệu suất tuyến tính | 

## Vỏ cạnh 

Đối với trường hợp kích thước tối thiểu`1 / R`, thuật toán bắt đầu bằng`last[(0,0)] = 0`. Sau đó`R`, vị trí là`(1,0)`, chưa bao giờ xuất hiện, vì vậy`left`vẫn bằng 0 và độ dài hiện tại là`1 - 0 = 1`. Đầu ra là`1`. 

Đối với trường hợp trả lại ngay`2 / RL`, các vị trí tiền tố là`(0,0)`,`(1,0)`,`(0,0)`. Bước đi thứ hai tìm thấy`(0,0)`tại chỉ số tiền tố bằng 0. Vì số 0 nằm trong cửa sổ hiện tại,`left`trở thành một. Độ dài kết quả là`2 - 1 = 1`, vì vậy đầu ra là`1`. Điều này giải thích rõ ràng rằng ô bắt đầu là một phần của các ô được truy cập. 

Vì`4 / RRLL`, các vị trí tiền tố là`(0,0)`,`(1,0)`,`(2,0)`,`(1,0)`,`(0,0)`. Tại tiền tố chỉ số ba,`(1,0)`được nhìn thấy lần cuối ở chỉ mục một, vì vậy`left`trở thành hai. Cửa sổ hợp lệ hiện tại thể hiện một bước di chuyển. Tại tiền tố chỉ số bốn,`(0,0)`được nhìn thấy lần cuối ở chỉ số 0, nhỏ hơn`left = 2`. Sự xuất hiện cũ đó không còn là một phần của chuỗi con ứng cử viên nữa, vì vậy`left`vẫn còn hai. Cửa sổ hiện tại thể hiện hai bước di chuyển, cụ thể là`LL`, và đầu ra là`2`. 

Đối với đầu vào hoàn toàn phù hợp`5 / RRRRR`, các vị trí là`(0,0)`,`(1,0)`,`(2,0)`,`(3,0)`,`(4,0)`,`(5,0)`. Mỗi tọa độ đều mới, vì vậy`left`không bao giờ thay đổi. Ở vị trí cuối cùng, độ dài cửa sổ là`5 - 0 = 5`, cho đầu ra`5`. Ví dụ này giải thích tại sao thuật toán theo dõi các ô thay vì di chuyển các ký tự. 

Đối với đầu vào tối đa chứa một triệu`R`di chuyển, mỗi tọa độ tiền tố là khác nhau, do đó từ điển nhận được một triệu mục nhập và câu trả lời đạt tới một triệu. Không có vòng lặp lồng nhau và mỗi lần di chuyển sẽ thực hiện công việc từ điển theo thời gian không đổi, do đó thời gian chạy vẫn giữ nguyên (O(n)) ngay cả trong trường hợp xấu nhất này.
