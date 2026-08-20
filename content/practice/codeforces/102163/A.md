---
title: "CF 102163A - Hasan thẩm phán lười biếng"
description: "Mỗi đoạn ngang được mô tả bởi hai tọa độ x điểm cuối và tọa độ y cố định của nó. Mỗi đoạn thẳng đứng được mô tả bởi hai điểm cuối tọa độ y và tọa độ x cố định của nó."
date: "2026-08-19T07:41:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102163
codeforces_index: "A"
codeforces_contest_name: "NCD 2019"
rating: 0
weight: 102163
solve_time_s: 344
verified: false
draft: false
---

[CF 102163A - Hasan thẩm phán lười biếng](https://codeforces.com/problemset/problem/102163/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 44s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mỗi đoạn ngang được mô tả bởi hai tọa độ x điểm cuối và tọa độ y cố định của nó. Mỗi đoạn thẳng đứng được mô tả bởi hai điểm cuối tọa độ y và tọa độ x cố định của nó. Việc chọn một đoạn ngang và một đoạn dọc sẽ cho dấu cộng chính xác khi hai đoạn thẳng cắt nhau. 

Giả sử giao điểm của chúng là C=(x,y). Bốn nhánh của dấu cộng có độ dài 

x−X 1 ​ ,X 2 ​ −x,y−Y 1 ​ ,Y 2 ​ −y. 

Giá trị của dấu cộng này là nhỏ nhất trong bốn độ dài này. Chúng ta cần giá trị tối đa có thể có trên mỗi cặp ngang và dọc giao nhau. 

Đầu vào có thể chứa 10 5 đoạn ngang và 10 5 đoạn dọc. Một thuật toán bậc hai sẽ phải kiểm tra tối đa 10 10 cặp trong một trường hợp thử nghiệm, vượt xa giới hạn 1 giây có thể hỗ trợ. Các tọa độ cũng được giới hạn bởi 10 5, điều này làm cho việc lập chỉ mục cây Fenwick trở nên đặc biệt thuận tiện. 

Có một số trường hợp ranh giới có thể khiến việc triển khai âm thầm thất bại. Đầu tiên, giao lộ ở điểm cuối vẫn là giao lộ. Ví dụ,```
11 11 3 22 4 1
```có câu trả lời`1`, vì các đoạn gặp nhau tại (1,2), nhưng đoạn ngang không có khoảng cách dương tới điểm cuối bên trái của nó tại điểm đó. Trong thực tế, giao điểm chính xác là (1,2), cho độ dài nhánh ngang là 0 và 2, vì vậy câu trả lời thực sự là`0`. Một giải pháp coi các khoảng là mở có thể báo cáo không chính xác không có giao lộ nào cả, trong khi giải pháp chỉ kiểm tra độ dài đoạn cũng có thể nhầm lẫn giữa sự tồn tại của giao lộ với sự tồn tại của một điểm cộng có độ dài dương. 

Thứ hai, một đoạn có độ dài chính xác là 2d là hợp lệ cho câu trả lời ứng cử viên d. Ví dụ,```
11 11 5 31 5 3
```có câu trả lời`2`. Giao điểm ở (3,3) và cả bốn nhánh đều có chiều dài 2. Sử dụng`length > 2*d`thay vì`length >= 2*d`sẽ từ chối nó một cách không chính xác. 

Thứ ba, nhiều phân đoạn có thể có cùng tọa độ y. Ví dụ,```
12 11 5 32 6 31 5 3
```có câu trả lời`2`. Cả hai phân đoạn ngang đều đóng góp vào cùng một tọa độ y, do đó cấu trúc dữ liệu phải hỗ trợ nhiều phân đoạn hoạt động ở cùng một tọa độ. Việc triển khai loại bỏ bất cẩn coi giá trị Fenwick chỉ là một boolean có thể loại bỏ tọa độ trong khi một phân đoạn khác có cùng y vẫn hoạt động. 

Cuối cùng, hai điểm cuối không cần thiết phải được giả định là đến theo thứ tự tăng dần trừ khi điều đó được thẩm phán đảm bảo. Bình thường hóa từng phân đoạn với`min`Và`max`làm cho việc triển khai trở nên mạnh mẽ. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp xem xét mọi phân đoạn ngang và mọi phân đoạn dọc. Đối với mỗi cặp, trước tiên chúng tôi kiểm tra xem phạm vi tọa độ của chúng có chứa điểm giao nhau hay không, sau đó tính bốn khoảng cách từ điểm đó đến điểm cuối. Điều này đúng vì mọi dấu cộng có thể đều tương ứng với chính xác một cặp như vậy. 

Vấn đề là số lượng cặp. Với N=M=10 5, có thể có N⋅M=10 10 cặp. Ngay cả một lượng công việc không đổi rất nhỏ trên mỗi cặp cũng là quá nhiều, vì vậy phương pháp bạo lực bị loại trừ. 

Quan sát hữu ích là hãy ngừng cố gắng tối đa hóa câu trả lời một cách trực tiếp. Thay vào đó, hãy đặt câu hỏi có hoặc không: liệu chúng ta có thể xây dựng một dấu cộng có độ dài ít nhất là d không? 

Đối với đoạn nằm ngang [X 1 ​ ,X 2 ​ ], điểm giao nhau phải cách cả hai điểm cuối ít nhất d. Do đó tọa độ x của nó phải thỏa mãn 

X 1 ​ +d<x<X 2 ​ −d. 

Tương tự, đối với đoạn thẳng đứng [Y 1 ​ ,Y 2 ​ ] thì giao điểm y phải thỏa mãn 

Y 1 ​ +d<y<Y 2 ​ −d. 

Vì vậy, đối với một d cố định, mọi đoạn ngang trở thành khoảng x được phép và mọi đoạn dọc trở thành khoảng y được phép. Chúng ta chỉ cần xác định xem một số đường thẳng đứng có đi qua phạm vi x được phép của một số đoạn ngang hay không trong khi phạm vi y của nó chứa tọa độ y của đoạn ngang đó. 

Điều kiện khả thi này là đơn điệu. Nếu tồn tại dấu cộng có độ dài d thì dấu cộng có độ dài nhỏ hơn cũng tồn tại. Điều đó cho chúng ta tìm kiếm nhị phân trên câu trả lời. 

Đối với một d cố định, quét các đoạn thẳng đứng từ trái sang phải. Một đoạn ngang sẽ có thể sử dụng được khi tọa độ x hiện tại đạt đến X 1 ​ +d và không còn sử dụng được sau X 2 ​ −d. Trong khi quét, hãy duy trì tọa độ y của tất cả các đoạn ngang hiện có thể sử dụng được trong cây Fenwick. Khi xử lý một đoạn thẳng đứng tại x, chúng tôi hỏi liệu có bất kỳ y ngang đang hoạt động nào nằm trong khoảng thu hẹp của nó [Y 1 ​ +d,Y 2 ​ −d] hay không. 

Các sự kiện kích hoạt và hủy kích hoạt theo chiều ngang đã được sắp xếp nếu chúng ta sắp xếp các phân đoạn ngang theo X 1 ​ và X 2 ​ ban đầu của chúng. Vì việc cộng hoặc trừ cùng một d sẽ giữ nguyên thứ tự, nên các mảng được sắp xếp này có thể được sử dụng lại cho mỗi lần lặp tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NM) | O(N+M) | Quá chậm | 
| Tối ưu | O((N+M)logClogC) | O(N+M+C) | Đã chấp nhận | 

Ở đây C<10 5 là giới hạn tọa độ. Logarit đầu tiên đến từ các phép toán trên cây Fenwick và logarit thứ hai đến từ tìm kiếm nhị phân trên câu trả lời. 

## Hướng dẫn thuật toán

1. Chuẩn hóa mọi phân đoạn sao cho điểm cuối đầu tiên của nó nhỏ hơn điểm cuối thứ hai. Đối với cửa hàng phân khúc ngang (X 1​ ,X 2​ ,Y) và đối với cửa hàng phân khúc dọc (Y 1 ​ ,Y 2​ ,X). Tính giới hạn trên cho câu trả lời từ nửa độ dài lớn nhất có thể có của các đoạn. 
2. Sắp xếp các đoạn ngang một lần theo X 1 ​ và riêng biệt theo X 2 ​. Sắp xếp các đoạn dọc theo tọa độ x của chúng. Các thứ tự này không phụ thuộc vào độ dài ứng cử viên d, bởi vì việc thu nhỏ một khoảng theo d chỉ đơn giản là thêm cùng một giá trị vào điểm cuối bên trái của nó và trừ cùng một giá trị từ điểm cuối bên phải của nó. 
3. Tìm kiếm nhị phân với độ dài khả thi lớn nhất d. Đối với ứng cử viên d, loại bỏ ngay lập tức mọi đoạn ngang có X 2 ​ −X 1 ​ <2d, vì nó không thể để lại ít nhất d đơn vị ở cả hai phía giao điểm của nó. Thực hiện kiểm tra tương tự cho các đoạn thẳng đứng. 
4. Đối với mọi đoạn ngang còn lại, hãy xác định phạm vi x hợp lệ của nó là [X 1 ​ +d,X 2 ​ −d]. Trong quá trình quét qua x, hãy chèn tọa độ y của nó khi phương thẳng đứng x hiện tại đạt đến X 1 ​ +d. 
5. Xóa một đoạn ngang khi phạm vi hợp lệ của nó đã kết thúc. Điều kiện loại bỏ đúng là X 2 ​ −d<x, vì x=X 2 ​ −d vẫn là vị trí giao nhau hợp lệ. Cây Fenwick lưu trữ số lượng phân đoạn ngang đang hoạt động tồn tại ở mỗi tọa độ y, thay vì chỉ lưu trữ liệu có tồn tại hay không, bởi vì một số phân đoạn có thể chia sẻ cùng một y. 
6. Xử lý các đoạn thẳng đứng theo tọa độ x tăng dần. Trước khi xử lý một đoạn dọc tại x, hãy kích hoạt tất cả các đoạn ngang có điểm cuối bên trái hợp lệ tối đa là x, sau đó xóa tất cả các đoạn có điểm cuối bên phải hợp lệ hoàn toàn nhỏ hơn x. 
7. Đoạn thẳng đứng có phạm vi y hợp lệ [Y 1 ​ +d,Y 2 ​ −d]. Truy vấn cây Fenwick để biết số đoạn ngang đang hoạt động có tọa độ y nằm trong khoảng bao gồm đó. Nếu số đếm dương thì ứng cử viên d là khả thi và tìm kiếm nhị phân sẽ di chuyển lên trên. 
8. Nếu không có phân đoạn dọc nào tìm thấy phân đoạn ngang đang hoạt động thì d là không thể và tìm kiếm nhị phân sẽ di chuyển xuống dưới. 

### Tại sao nó hoạt động 

Đối với một d cố định, một đoạn ngang hoạt động tại x chính xác khi giao điểm tại x đó sẽ để lại ít nhất d đơn vị ở cả hai phía ngang. Tương tự như vậy, một đoạn thẳng đứng chấp nhận chính xác tọa độ y để lại ít nhất d đơn vị trên cả hai cạnh thẳng đứng. 

Tại thời điểm một đoạn thẳng đứng tại x được xử lý, cây Fenwick chứa chính xác tọa độ y của các đoạn ngang có phạm vi x hợp lệ chứa x. Sau đó, truy vấn phạm vi Fenwick sẽ kiểm tra chính xác xem một trong các tọa độ y đó có thuộc phạm vi y hợp lệ của phân đoạn dọc hay không. Như vậy`check(d)`trả về true chính xác khi tồn tại dấu cộng có độ dài ít nhất d. 

Vị từ là đơn điệu vì việc thu nhỏ chiều dài cánh tay cần thiết chỉ có thể phóng to tập hợp các giao điểm hợp lệ. Do đó, tìm kiếm nhị phân tìm thấy độ dài khả thi tối đa. 

## Giải pháp Python```python
Pythonimport sysinput = sys.stdin.readline

def solve():    t = int(input())    out = []
    for _ in range(t):        n, m = map(int, input().split())
        horizontal = []        vertical = []
        max_coord = 0        max_half_h = 0        max_half_v = 0
        for _ in range(n):            x1, x2, y = map(int, input().split())            if x1 > x2:                x1, x2 = x2, x1
            horizontal.append((x1, x2, y))            max_coord = max(max_coord, x2, y)            max_half_h = max(max_half_h, (x2 - x1) // 2)
        for _ in range(m):            y1, y2, x = map(int, input().split())            if y1 > y2:                y1, y2 = y2, y1
            vertical.append((y1, y2, x))            max_coord = max(max_coord, y2, x)            max_half_v = max(max_half_v, (y2 - y1) // 2)
        horizontal_by_left = sorted(horizontal, key=lambda s: s[0])        horizontal_by_right = sorted(horizontal, key=lambda s: s[1])        vertical.sort(key=lambda s: s[2])
        bit_size = max_coord + 2
        def check(d):            bit = [0] * bit_size
            def add(pos, delta):                pos += 1                while pos < bit_size:                    bit[pos] += delta                    pos += pos & -pos
            def prefix(pos):                if pos < 0:                    return 0                pos += 1                res = 0                while pos:                    res += bit[pos]                    pos -= pos & -pos                return res
            left_ptr = 0            right_ptr = 0
            for y1, y2, x in vertical:                if y2 - y1 < 2 * d:                    continue
                while left_ptr < n:                    x1, x2, y = horizontal_by_left[left_ptr]
                    if x1 + d > x:                        break
                    if x2 - x1 >= 2 * d:                        add(y, 1)
                    left_ptr += 1
                while right_ptr < n:                    x1, x2, y = horizontal_by_right[right_ptr]
                    if x2 - d >= x:                        break
                    if x2 - x1 >= 2 * d:                        add(y, -1)
                    right_ptr += 1
                low_y = y1 + d                high_y = y2 - d
                if prefix(high_y) - prefix(low_y - 1) > 0:                    return True
            return False
        high = min(max_half_h, max_half_v)        low = 0        answer = 0
        while low <= high:            mid = (low + high) // 2
            if check(mid):                answer = mid                low = mid + 1            else:                high = mid - 1
        out.append(str(answer))
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":    solve()
```Đầu vào được đọc với`sys.stdin.readline`, điều này tránh được chi phí phân tích cú pháp đầu vào chung lặp đi lặp lại. Mỗi phân đoạn được chuẩn hóa ngay lập tức, do đó, tất cả các phép tính khoảng thời gian sau này có thể giả định điểm cuối tăng dần một cách an toàn. 

Hai mảng ngang được sắp xếp là bước tiền xử lý chính.`horizontal_by_left`kiểm soát thời điểm một phân đoạn đi vào tập hoạt động, trong khi`horizontal_by_right`kiểm soát khi nó rời đi. Con trỏ chỉ di chuyển về phía trước nên mỗi đoạn ngang được coi là số lần không đổi cho mỗi lần kiểm tra tính khả thi. 

Cây Fenwick được lập chỉ mục theo tọa độ y. Vì tọa độ tối đa là 10 5 nên việc lập chỉ mục trực tiếp đơn giản hơn nén tọa độ. Cây lưu trữ số lượng chứ không phải boolean, do đó hai đoạn ngang đang hoạt động có cùng tọa độ y được biểu diễn chính xác. 

Điều kiện kích hoạt sử dụng`x1 + d <= x`. Điều kiện loại bỏ sử dụng`x2 - d < x`. Sự bất đẳng thức chặt chẽ đó là cần thiết vì điểm cuối bên phải của khoảng thu gọn vẫn có hiệu lực. Truy vấn dọc cũng bao gồm cả hai đầu, được triển khai dưới dạng```
Pythonprefix(high_y) - prefix(low_y - 1)
```Một đoạn có độ dài chính xác là 2d được chấp nhận vì khoảng thu gọn của nó bao gồm một tọa độ. Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn số nguyên, mặc dù tất cả các giá trị trong bài toán này đều đủ nhỏ cho số học 32 bit thông thường. 

Giới hạn trên của tìm kiếm nhị phân sử dụng nửa chiều dài ngang lớn nhất và nửa chiều dọc lớn nhất nhỏ hơn. Một điểm cộng hợp lệ của độ dài d cần cả đoạn ngang và đoạn dọc có độ dài ít nhất là 2d, do đó không có câu trả lời nào có thể vượt quá giới hạn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
13 21 5 32 4 26 12 61 5 36 9 2
```Câu trả lời ứng viên có thể có nhiều nhất là 2. Tìm kiếm nhị phân sẽ kiểm tra ứng viên 1 và 2. 

Đối với d=1, phạm vi x hợp lệ theo chiều ngang là [2,4], [3,3] và [7,11]. Các đoạn dọc có phạm vi y hợp lệ [2,4] và [7,8]. 

| d | Dọc x | Hoạt động ngang y | Dọc hợp lệ y | Khả thi | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | không | [2, 4] | không | 
| 1 | 3 | 3, 2 | [2, 4] | vâng | 

Khi x đạt 3, hai đoạn ngang đầu tiên sẽ hoạt động. Đoạn thẳng đứng tại x=3 bao gồm y từ 2 đến 4, do đó, đoạn ngang nào cũng có độ dài cộng ít nhất là 1. 

Đối với d=2, đoạn ngang đầu tiên có phạm vi x hợp lệ [3,3], trong khi phân đoạn thứ hai không còn chiều rộng và phân đoạn thứ ba có phạm vi x hợp lệ [8,10]. Đoạn dọc đầu tiên tại x=3 có phạm vi y hợp lệ [3,3]. 

| d | Dọc x | Hoạt động ngang y | Dọc hợp lệ y | Khả thi | 
| --- | --- | --- | --- | --- | 
| 2 | 2 | không | [3, 3] | không | 
| 2 | 3 | 3 | [3, 3] | vâng | 

Giao điểm tại (3,3) để lại đúng 2 đơn vị theo cả 4 hướng cho đoạn ngang thứ nhất và đoạn dọc thứ nhất nên đáp án là`2`. 

### Ví dụ tùy chỉnh 2 

Hãy xem xét```
12 21 9 52 6 35 7 51 9 4
```Đoạn ngang thứ nhất có độ dài 8, đoạn dọc thứ nhất có độ dài 2. Do đó, đáp án không thể vượt quá 1. Đoạn dọc thứ hai có độ dài 8. 

Đối với d=1, phạm vi x hợp lệ của chiều ngang thứ nhất là [2,8] và phạm vi x hợp lệ của chiều ngang thứ hai là [3,5]. Đoạn dọc tại x=5 có phạm vi y hợp lệ [6,6], do đó, nó không thể đáp ứng theo chiều ngang tại y hợp lệ. Đoạn dọc tại x=4 có phạm vi y hợp lệ [2,8] và cả hai giá trị y ngang 5 và 3 đều nằm trong phân đoạn đó. 

| d | Dọc x | Hoạt động ngang y | Dọc hợp lệ y | Khả thi | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 5, 3 | [2, 8] | vâng | 
| 1 | 5 | 5, 3 | [6, 6] | không | 

Sự tồn tại của giao điểm x=4 chứng tỏ d=1 là khả thi nên đáp án cuối cùng là`1`. Dấu vết này cũng giải thích tại sao quá trình quét phải xử lý tất cả các phân đoạn ngang có điểm cuối bên trái được thu nhỏ tối đa là x hiện tại trước khi thực hiện truy vấn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N+M)logClogC) | Tìm kiếm nhị phân thực hiện kiểm tra O(logC), mỗi kiểm tra sử dụng các phép toán con trỏ O(N+M) và các phép toán Fenwick O(N+M), mỗi lần có giá trị O(logC). | 
| Không gian | O(N+M+C) | Hai mảng ngang được sắp xếp, mảng dọc và một cây Fenwick được lưu trữ. | 

Với N,M<10 5 và C<10 5, tìm kiếm nhị phân cần ít hơn 17 lần lặp. Giải pháp tránh việc liệt kê 10 10 cặp lực lượng vũ phu và giữ mọi kiểm tra tính khả thi trong thời gian gần tuyến tính. Giới hạn tọa độ cũng giữ cho cây Fenwick đủ nhỏ để đạt giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
Pythonimport sysimport io

def solve_io(inp: str) -> str:    old_stdin = sys.stdin    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)    sys.stdout = io.StringIO()
    solve()
    result = sys.stdout.getvalue()
    sys.stdin = old_stdin    sys.stdout = old_stdout
    return result

def solve():    t = int(input())    out = []
    for _ in range(t):        n, m = map(int, input().split())
        horizontal = []        vertical = []
        max_coord = 0        max_half_h = 0        max_half_v = 0
        for _ in range(n):            x1, x2, y = map(int, input().split())            if x1 > x2:                x1, x2 = x2, x1            horizontal.append((x1, x2, y))            max_coord = max(max_coord, x2, y)            max_half_h = max(max_half_h, (x2 - x1) // 2)
        for _ in range(m):            y1, y2, x = map(int, input().split())            if y1 > y2:                y1, y2 = y2, y1            vertical.append((y1, y2, x))            max_coord = max(max_coord, y2, x)            max_half_v = max(max_half_v, (y2 - y1) // 2)
        horizontal_by_left = sorted(horizontal, key=lambda s: s[0])        horizontal_by_right = sorted(horizontal, key=lambda s: s[1])        vertical.sort(key=lambda s: s[2])
        bit_size = max_coord + 2
        def check(d):            bit = [0] * bit_size
            def add(pos, delta):                pos += 1                while pos < bit_size:                    bit[pos] += delta                    pos += pos & -pos
            def prefix(pos):                if pos < 0:                    return 0                pos += 1                res = 0                while pos:                    res += bit[pos]                    pos -= pos & -pos                return res
            left_ptr = 0            right_ptr = 0
            for y1, y2, x in vertical:                if y2 - y1 < 2 * d:                    continue
                while left_ptr < n:                    x1, x2, y = horizontal_by_left[left_ptr]                    if x1 + d > x:                        break                    if x2 - x1 >= 2 * d:                        add(y, 1)                    left_ptr += 1
                while right_ptr < n:                    x1, x2, y = horizontal_by_right[right_ptr]                    if x2 - d >= x:                        break                    if x2 - x1 >= 2 * d:                        add(y, -1)                    right_ptr += 1
                low_y = y1 + d                high_y = y2 - d
                if prefix(high_y) - prefix(low_y - 1):                    return True
            return False
        low = 0        high = min(max_half_h, max_half_v)        answer = 0
        while low <= high:            mid = (low + high) // 2            if check(mid):                answer = mid                low = mid + 1            else:                high = mid - 1
        out.append(str(answer))
    sys.stdout.write("\n".join(out))

input = sys.stdin.readline
# Provided sampleassert solve_io("""\13 21 5 32 4 26 12 61 5 36 9 2""") == "2\n", "sample 1"
# Minimum-size segments, with a genuine intersectionassert solve_io("""\11 11 1 11 1 1""") == "0\n", "minimum-size case"
# Exactly 2*d on both segmentsassert solve_io("""\11 11 5 31 5 3""") == "2\n", "exact boundary length"
# No intersection at allassert solve_io("""\11 11 3 11 3 5""") == "0\n", "no intersection"
# Same y-coordinate on multiple horizontal segmentsassert solve_io("""\13 11 9 52 8 53 7 51 9 5""") == "4\n", "duplicate active y"
# Reversed endpointsassert solve_io("""\11 19 1 57 1 5""") == "4\n", "reversed endpoints"
# Boundary-coordinate maximum-size constructionn = 100000h_lines = "\n".join(["1 100000 50000"] * n)v_lines = "\n".join(["1 100000 50000"] * n)max_input = f"1\n{n} {n}\n{h_lines}\n{v_lines}\n"assert solve_io(max_input) == "49999\n", "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 1 / 1 1 1 / 1 1 1`|`0`| Các đoạn có kích thước tối thiểu và các nhánh có độ dài bằng 0 | 
|`1 / 1 1 / 1 5 3 / 1 5 3`|`2`| Ranh giới 2d chính xác | 
|`1 / 1 1 / 1 3 1 / 1 3 5`|`0`| Các đoạn không bao giờ giao nhau | 
| Ba đoạn ngang có cùng y |`4`| Tọa độ y hoạt động trùng lặp | 
| Điểm cuối đảo ngược |`4`| Chuẩn hóa điểm cuối | 
| 10 5 đoạn ngang và dọc giống hệt nhau |`49999`| Kích thước đầu vào tối đa và ranh giới tọa độ | 

## Vỏ cạnh 

Một đoạn có thể giao nhau một cách chính xác tại một điểm cuối. Coi như```
11 11 3 22 4 1
```Giao lộ duy nhất là (1,2). Các cánh tay ngang có độ dài 0 và 2, do đó độ dài cộng là`0`. Trong lúc`check(0)`, phạm vi x hợp lệ theo chiều ngang là`[1,3]`và phạm vi y hợp lệ theo chiều dọc là`[2,4]`. Tại x=1 chiều ngang được kích hoạt vì điều kiện là`left <= x`. Truy vấn Fenwick tìm thấy y=2, do đó giao điểm được nhận dạng. Vì`d=1`, phạm vi x hợp lệ theo chiều ngang trở thành`[2,2]`, trong khi chiều dọc vẫn có giá trị trong y`[3,3]`, do đó không tồn tại cộng của độ dài 1. 

Một đoạn có độ dài chính xác là 2d phải hợp lệ. Vì```
11 11 5 31 5 3
```câu trả lời là`2`. Vì`check(2)`, cả hai khoảng thu nhỏ trở thành điểm duy nhất 3. Chiều ngang được kích hoạt ở x=3, chiều dọc chấp nhận y=3 và truy vấn Fenwick thành công. Sử dụng các bất đẳng thức nghiêm ngặt trong kiểm tra độ dài hoặc ranh giới quét sẽ làm mất nghiệm này một cách không chính xác. 

Tọa độ y trùng lặp yêu cầu đếm. TRONG```
12 11 9 52 6 51 9 5
```cả hai phân đoạn ngang đều hoạt động ở mức y=5 khi phân đoạn dọc được xử lý. Giá trị Fenwick tại y=5 trở thành 2. Nếu một đoạn ngang bị xóa trong khi đoạn ngang kia vẫn hoạt động, thì giá trị sẽ trở thành 1, do đó tọa độ y vẫn được biểu thị. Cấu trúc boolean không có số lượng tham chiếu có thể xóa nó một cách không chính xác. 

Cuối cùng, thứ tự điểm cuối phải được chuẩn hóa trước bất kỳ phép tính nào. Vì```
11 19 1 57 1 5
```đoạn ngang thực sự là [1,9] và đoạn dọc thực sự là [1,7]. Giao điểm của chúng là (5,5), cho bốn cạnh có chiều dài 4, nên đáp án là`4`. Hoán đổi điểm cuối trước khi tính toán độ dài và khoảng thu hẹp làm cho thuật toán tương tự hoạt động mà không có trường hợp đặc biệt.
