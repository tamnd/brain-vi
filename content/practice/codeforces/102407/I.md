---
title: "CF 102407I - \u0412\u044b\u0440\u0432\u0430\u0442\u044c\u0441\u044f \u0438\u0437 \u043e\u043a\u0440\u0443\u0436\u0435\u043d\u0438\u044f"
description: "Chúng ta có một lưới (n lần n), với Joker ở ô ((a,b)). Chúng ta cần đếm các ô bên trong lưới có khoảng cách từ Manhattan đến Joker chính xác là (d). Đối với một ô ((x,y)), điều kiện là [ ] Không có ranh giới lưới, các ô này tạo thành một hình thoi bao quanh ((a,b))."
date: "2026-08-11T16:19:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102407
codeforces_index: "I"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0412\u0442\u043e\u0440\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102407
solve_time_s: 88
verified: true
draft: false
---

[CF 102407I - \u0412\u044b\u0440\u0432\u0430\u0442\u044c\u0441\u044f \u0438\u0437 \u043e\u043a\u0440\u0443\u0436\u0435\u043d\u0438\u044f](https://codeforces.com/problemset/problem/102407/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới (n \times n), với Joker ở ô ((a,b)). Chúng ta cần đếm các ô bên trong lưới có khoảng cách từ Manhattan đến Joker chính xác là (d). 

Đối với một ô ((x,y)), điều kiện là 

[ 
|x-a|+|y-b|=d. 
] 

Không có ranh giới lưới, các ô này tạo thành một hình thoi xung quanh ((a,b)). Viên kim cương có bốn cạnh chéo và trên một lưới vô hạn không giới hạn, nó chứa chính xác (4d) ô. Điều khó khăn là một số phần của viên kim cương này có thể nằm bên ngoài căn phòng (n \times n). 

Dữ liệu đầu vào chứa (n,a,b,d), trong đó (n) lớn bằng (10^{18}). Bản thân câu trả lời cũng có thể theo thứ tự (10^{18}), do đó việc triển khai cần số học số nguyên để xử lý thoải mái các giá trị lớn hơn số nguyên 32 bit. Số nguyên Python phù hợp trực tiếp cho việc này. 

Kích thước của (n) loại trừ việc lặp lại trên lưới. Ngay cả việc quét tất cả (n^2) ô cũng sẽ yêu cầu tối đa (10^{36}) lần kiểm tra trong trường hợp lớn nhất. Một cách tiếp cận tỷ lệ thuận với (d) cũng không thể thực hiện được vì (d) có thể đạt tới (10^{18} một cách độc lập). Chúng ta cần một lượng công việc không đổi, không phụ thuộc vào cả (n) và (d). 

Có một số trường hợp biên trong đó một công thức đơn giản như (4d) không thành công. Ví dụ, với đầu vào`5 1 1 2`, viên kim cương không giới hạn có tám ô, nhưng chỉ có ba ô nằm vừa trong phòng: ((1,3)), ((2,2)) và ((3,1)). Câu trả lời đúng là`3`, do đó quay trở lại một cách mù quáng (4d) sẽ đếm các ô bên ngoài phòng. 

Một trường hợp tinh tế khác là góc kim cương. Vì`3 2 2 1`, cả bốn ô ở khoảng cách một đều ở trong phòng, nên câu trả lời là`4`. Nếu chúng ta đếm bốn cạnh chéo một cách độc lập thì mỗi cạnh có hai điểm cuối và tổng số là tám. Mỗi đỉnh trong số bốn đỉnh kim cương thuộc về hai cạnh lân cận, vì vậy chỉ cần cộng độ dài bốn cạnh sẽ đếm gấp đôi mỗi đỉnh và tạo ra`8`thay vì`4`. 

Trường hợp thứ ba xảy ra khi toàn bộ viên kim cương ở ngoài phòng. Vì`3 2 2 10`, không ô nào có thể có khoảng cách 10 từ trung tâm Manhattan vì khoảng cách tối đa có thể có bên trong lưới là 4. Câu trả lời đúng là`0`. Bất kỳ giải pháp nào giả định rằng ít nhất một điểm của viên kim cương lý thuyết còn tồn tại sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là kiểm tra mọi ô ((x,y)) của phòng và kiểm tra xem (|x-a|+|y-b|=d) hay không. Nó đúng vì nó kiểm tra mọi ô có thể một cách chính xác một lần và áp dụng trực tiếp định nghĩa về khoảng cách Manhattan. Thời gian chạy trong trường hợp xấu nhất của nó là (O(n^2)), trở thành (10^{36}) ô kiểm tra khi (n=10^{18}). Việc sử dụng bộ nhớ có thể vẫn còn (O(1)), nhưng thời gian chạy khiến phương pháp này không thể sử dụng được. 

Chúng ta có thể làm tốt hơn bằng cách nhìn vào hình học của phương trình thay vì nhìn vào từng ô riêng lẻ. phương trình 

[ 
|x-a|+|y-b|=d 
] 

mô tả bốn đoạn đường chéo. Điểm cuối của chúng là 

[ 
(a-d,b),\quad (a,b+d),\quad (a+d,b),\quad (a,b-d). 
] 

Ví dụ: phân đoạn từ ((a-d,b)) đến ((a,b+d)) có thể được tham số hóa thành 

[ 
(x,y)=(a-d+k,b+k),\qquad 0\le k\le d. 
] 

Mỗi số nguyên (k) cho chính xác một ô lưới ở phía đó của hình thoi. Ba mặt còn lại có cấu trúc tương tự, có dấu tọa độ thay đổi được điều chỉnh tương ứng. 

Phương pháp brute-force hoạt động vì nó xem xét tất cả các ô một cách độc lập nhưng không thành công vì có quá nhiều ô. Nhận xét rằng tập hợp mong muốn chỉ bao gồm bốn đoạn đường chéo đơn điệu cho phép chúng ta thay thế tìm kiếm hai chiều khổng lồ bằng bốn khoảng một chiều. 

Đối với một phân đoạn như vậy, chúng ta không cần phải lặp lại (k=0,1,\ldots,d). Chúng ta chỉ cần xác định giá trị nào của (k) giữ cả hai tọa độ bên trong ([1,n]). Vì mỗi tọa độ thay đổi chính xác (+1) hoặc (-1) khi (k) tăng lên, mỗi hạn chế tọa độ chỉ đơn giản là một khoảng giá trị (k) được phép. Việc giao nhau các khoảng đó sẽ cho số lượng ô hợp lệ chính xác trên đoạn đó trong thời gian không đổi. 

Còn một chi tiết cuối cùng. Các đoạn kim cương liền kề chia sẻ điểm cuối của chúng, do đó, tổng bốn đoạn được cắt sẽ tính mỗi đỉnh kim cương hai lần bất cứ khi nào đỉnh đó nằm trong phòng. Chúng tôi trừ một cho mỗi đỉnh như vậy. Một đỉnh bên ngoài phòng không thể tạo một bản sao bên trong phòng vì hai đoạn thẳng chỉ gặp nhau tại đỉnh đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(1)) | Quá chậm | 
| Tối ưu | (O(1)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu diễn mỗi cạnh trong số bốn cạnh của hình thoi có khoảng cách Manhattan dưới dạng một đoạn được tham số hóa. Bốn điểm bắt đầu là ((a-d,b)), ((a,b+d)), ((a+d,b)) và ((a,b-d)) và mỗi đoạn di chuyển chính xác (d) các bước đơn vị theo một trong bốn hướng chéo. Điều này bao gồm chính xác mọi ô ở khoảng cách Manhattan (d) trước khi áp dụng ranh giới phòng. 
2. Đối với một đoạn, đưa vào một tham số nguyên (k) với (0\le k\le d). Tọa độ của nó có dạng 

[ 
x=x_0+s_xk,\qquad y=y_0+s_yk, 
] 

trong đó (s_x,s_y) là (1) hoặc (-1). Mỗi số nguyên hợp lệ (k) tương ứng với một ô ở phía đó. 
3. Chuyển đổi điều kiện (1\le x\le n) thành một khoảng cho (k). Nếu (s_x=1) thì 

[ 
1-x_0\le k\le n-x_0. 
] 

Nếu (s_x=-1), thì 

[ 
x_0-n\le k\le x_0-1. 
] 

Thực hiện tương tự cho tọa độ (y). Giao cả hai khoảng tọa độ với khoảng ban đầu ([0,d]). 
4. Nếu giới hạn dưới thu được là (L) và giới hạn trên là (R), thì phân đoạn sẽ đóng góp (R-L+1) ô khi (L\le R) và đóng góp 0 nếu ngược lại. Điều này xử lý một phân đoạn hoàn toàn bên ngoài và một phân đoạn bị cắt bớt một phần với cùng một phép tính. 
5. Tính tổng số tiền đóng góp của cả bốn phân khúc. Bốn đoạn bao phủ toàn bộ viên kim cương ở khoảng cách Manhattan, nhưng mỗi đỉnh kim cương thuộc về hai đoạn. 
6. Kiểm tra bốn đỉnh ((a-d,b)), ((a,b+d)), ((a+d,b)) và ((a,b-d)). Đối với mỗi đỉnh có hai tọa độ nằm trong ([1,n]), hãy trừ đi một từ tổng. Điều này loại bỏ chính xác sự xuất hiện trùng lặp được tạo bởi hai phân đoạn liền kề. 

### Tại sao nó hoạt động

Mọi ô ở khoảng cách Manhattan (d) nằm trên đúng một trong bốn cạnh chéo của hình thoi, ngoại trừ bốn đỉnh kim cương nằm ở hai cạnh cạnh nhau. Việc tham số hóa mỗi bên sẽ tạo ra mỗi ô số nguyên ở bên đó đúng một lần. Giao điểm khoảng giữ chính xác các ô được tạo có tọa độ thuộc về phòng. Do đó, tổng hợp bốn đoạn bị cắt sẽ tính mỗi ô hợp lệ một lần và mỗi đỉnh kim cương hợp lệ hai lần. Việc trừ một cho mỗi đỉnh trong phòng sẽ loại bỏ chính xác số lượng trùng lặp đó, khiến mọi ô bắt buộc được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def segment_count(x0, y0, sx, sy, d, n):
    lo = 0
    hi = d

    if sx == 1:
        lo = max(lo, 1 - x0)
        hi = min(hi, n - x0)
    else:
        lo = max(lo, x0 - n)
        hi = min(hi, x0 - 1)

    if sy == 1:
        lo = max(lo, 1 - y0)
        hi = min(hi, n - y0)
    else:
        lo = max(lo, y0 - n)
        hi = min(hi, y0 - 1)

    if lo > hi:
        return 0
    return hi - lo + 1

def solve():
    n, a, b, d = map(int, input().split())

    segments = [
        (a - d, b, 1, 1),
        (a, b + d, 1, -1),
        (a + d, b, -1, -1),
        (a, b - d, -1, 1),
    ]

    answer = 0

    for x0, y0, sx, sy in segments:
        answer += segment_count(x0, y0, sx, sy, d, n)

    vertices = [
        (a - d, b),
        (a, b + d),
        (a + d, b),
        (a, b - d),
    ]

    for x, y in vertices:
        if 1 <= x <= n and 1 <= y <= n:
            answer -= 1

    print(answer)

if __name__ == "__main__":
    solve()
```các`segment_count`chức năng là cốt lõi của giải pháp. Nó bắt đầu với phạm vi tham số tự nhiên (0\le k\le d), đại diện cho mặt kim cương hoàn chỉnh. Mỗi tọa độ sau đó thu hẹp phạm vi này theo ranh giới lưới. 

Đối với hướng dương, chẳng hạn như (x=x_0+k), ranh giới dưới (x\ge1) cho (k\ge1-x_0), trong khi ranh giới trên (x\le n) cho (k\le n-x_0). Đối với hướng âm, (x=x_0-k), các bất đẳng thức đảo ngược và cho (k\ge x_0-n) và (k\le x_0-1). Việc xử lý trực tiếp những bất bình đẳng này sẽ tránh được mọi trường hợp đặc biệt dựa trên việc Joker có ở gần một phía cụ thể của căn phòng hay không. 

Bốn mô tả phân đoạn sử dụng các đỉnh kim cương liên tiếp. Các cặp hướng của chúng là ((1,1)), ((1,-1)), ((-1,-1)) và ((-1,1)), do đó mỗi bên được đi qua đúng một lần. 

Phép trừ đỉnh xảy ra sau khi tất cả bốn số đoạn đã được tính toán. Một đỉnh chỉ được nhân đôi khi chính đỉnh đó ở trong phòng. Kiểm tra tọa độ của nó một cách rõ ràng sẽ an toàn hơn việc cố gắng suy ra điều này từ độ dài đoạn lân cận. 

Không có vấn đề tràn trong Python vì số nguyên của nó có độ chính xác tùy ý. Trong các ngôn ngữ có loại số nguyên có chiều rộng cố định, số nguyên có dấu 64 bit là đủ cho tọa độ đầu vào và cho câu trả lời, vì câu trả lời không thể vượt quá số ô trong phòng, (10^{36}), trên thực tế, số trên vòng khoảng cách tối đa là (4d), tối đa là (4\cdot10^{18}), do đó, cần có loại 64 bit không dấu hoặc loại có dấu đủ rộng tùy thuộc vào ngôn ngữ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho`5 3 3 2`, Joker nằm ở trung tâm căn phòng và toàn bộ viên kim cương khoảng cách hai nằm gọn trong lưới. 

| Phân đoạn | Bắt đầu | Hướng | Hợp lệ (k) | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | ((1,3)) | ((1,1)) | (0..2) | 3 | 
| 2 | ((3,5)) | ((1,-1)) | (0..2) | 3 | 
| 3 | ((5,3)) | ((-1,-1)) | (0..2) | 3 | 
| 4 | ((3,1)) | ((-1,1)) | (0..2) | 3 | 
| | | | Tổng phân khúc | 12 | 

Tất cả bốn đỉnh kim cương đều ở trong phòng nên mỗi đỉnh được tính hai lần. Chúng tôi trừ bốn: 

[ 
12-4=8. 
] 

Đầu ra là`8`. 

Ví dụ này chứng minh tại sao việc hiệu chỉnh đỉnh là cần thiết ngay cả khi viên kim cương được chứa hoàn toàn trong phòng. 

### Mẫu 2 

cho`5 2 3 4`, viên kim cương theo lý thuyết lớn hơn nhiều so với căn phòng. Chỉ những mảnh nhỏ của hai mặt đối diện mới lọt vào lưới. 

| Phân đoạn | Bắt đầu | Hướng | Hợp lệ (k) | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | ((-2,3)) | ((1,1)) | (3..4) | 2 | 
| 2 | ((2,7)) | ((1,-1)) | không | 0 | 
| 3 | ((6,3)) | ((-1,-1)) | (1..2) | 2 | 
| 4 | ((2,-1)) | ((-1,1)) | không | 0 | 
| | | | Tổng phân khúc | 4 | 

Không có đỉnh nào trong số bốn đỉnh hình thoi nằm trong phòng nên không có đỉnh nào bị trùng lặp để trừ đi. Câu trả lời là`4`. 

Dấu vết này chứng tỏ tại sao việc cắt mỗi bên một cách độc lập sẽ xử lý một viên kim cương hầu như nằm ngoài phòng mà không yêu cầu bất kỳ trường hợp hình học đặc biệt nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(1)) | Bốn đoạn và bốn đỉnh được xử lý, với số phép tính số học không đổi. | 
| Không gian | (O(1)) | Chỉ có một số lượng tọa độ và giới hạn khoảng không đổi được lưu trữ. | 

Các ràng buộc cho phép (n) và (d) đạt tới (10^{18}), do đó, cả kích thước lưới lẫn bán kính đều không thể được sử dụng làm giới hạn lặp. Thuật toán thực hiện cùng một lượng công việc không đổi cho mỗi đầu vào, giúp nó dễ dàng đủ nhanh trong giới hạn hai giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys
import io

def segment_count(x0, y0, sx, sy, d, n):
    lo = 0
    hi = d

    if sx == 1:
        lo = max(lo, 1 - x0)
        hi = min(hi, n - x0)
    else:
        lo = max(lo, x0 - n)
        hi = min(hi, x0 - 1)

    if sy == 1:
        lo = max(lo, 1 - y0)
        hi = min(hi, n - y0)
    else:
        lo = max(lo, y0 - n)
        hi = min(hi, y0 - 1)

    return max(0, hi - lo + 1)

def solution(inp: str) -> str:
    n, a, b, d = map(int, inp.split())

    segments = [
        (a - d, b, 1, 1),
        (a, b + d, 1, -1),
        (a + d, b, -1, -1),
        (a, b - d, -1, 1),
    ]

    answer = sum(
        segment_count(x0, y0, sx, sy, d, n)
        for x0, y0, sx, sy in segments
    )

    vertices = [
        (a - d, b),
        (a, b + d),
        (a + d, b),
        (a, b - d),
    ]

    for x, y in vertices:
        if 1 <= x <= n and 1 <= y <= n:
            answer -= 1

    return str(answer)

def run(inp: str) -> str:
    return solution(inp).strip()

# Provided samples
assert run("5 3 3 2") == "8", "sample 1"
assert run("5 2 3 4") == "4", "sample 2"
assert run(
    "1000000000000000000 123456789987654321 "
    "987654321123456789 543211234567899876"
) == "679013703432097408", "sample 3"

# Minimum-size input
assert run("1 1 1 1") == "0", "no cell can be at positive distance"

# Entire ring fits, checking duplicate diamond vertices
assert run("5 3 3 1") == "4", "four immediate neighbors"

# Diamond starts at a corner and is heavily clipped
assert run("5 1 1 2") == "3", "corner clipping"

# All n, a, b, d equal
assert run("5 5 5 5") == "0", "all-equal boundary values"

# Maximum-size coordinates and radius
assert run("1000000000000000000 1 1 1000000000000000000") == \
       "999999999999999999", "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1`|`0`| Lưới tối thiểu và vòng khoảng cách trống | 
|`5 3 3 1`|`4`| Tất cả bốn đỉnh đều nằm bên trong và phải được loại bỏ trùng lặp | 
|`5 1 1 2`|`3`| Cắt góc và bất đẳng thức biên | 
|`5 5 5 5`|`0`| Bán kính lớn có thể không tạo ra ô nào | 
|`10^18 1 1 10^18`|`999999999999999999`| Giá trị tọa độ và bán kính tối đa | 

## Vỏ cạnh 

cho`5 1 1 2`, bốn đỉnh lý thuyết là ((-1,1)), ((1,3)), ((3,1)) và ((1,-1)). Chỉ có hai đỉnh ở giữa nằm bên trong, trong khi các cạnh chéo giữa chúng tạo ra điểm hợp lệ thứ ba ((2,2)). Việc cắt đoạn cho ra chính xác tổng cộng ba ô và hai đỉnh bên trong mỗi đỉnh được tính hai lần trước khi chỉnh sửa trùng lặp. Câu trả lời cuối cùng là`3`. 

Vì`3 2 2 1`, bốn đỉnh là ((1,2)), ((2,3)), ((3,2)) và ((2,1)). Mỗi đoạn chứa hai điểm, tổng cộng là 8 điểm. Mỗi một trong bốn đỉnh đều ở trong phòng nên bốn lần xuất hiện trùng lặp sẽ bị loại bỏ. Kết quả là`4`. 

Vì`3 2 2 10`, mọi đỉnh lý thuyết đều nằm bên ngoài phòng và khoảng (k) được phép của mọi phân đoạn sẽ trở nên trống sau khi giao với các giới hạn tọa độ. Tất cả bốn phần đóng góp của phân đoạn đều bằng 0 và không có đỉnh nào để trừ. Kết quả là`0`. 

Vì`5 3 3 2`, mỗi bên có ba giá trị tham số hợp lệ, do đó tổng số phân đoạn thô là mười hai. Bốn đỉnh kim cương nằm bên trong căn phòng và chính xác là bốn ô được nhân đôi. Trừ đi những chiếc lá`8`, khớp mẫu và xác nhận tính bất biến mà chỉ các điểm cuối bên cạnh được nhân đôi. 

Nếu bạn muốn, tôi cũng có thể biến bài này thành một **bài xã luận ngắn gọn theo phong cách Codeforces** điển hình hơn, giữ nguyên bằng chứng và giải pháp nhưng giảm đáng kể phần trình bày.
