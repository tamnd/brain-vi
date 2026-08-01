---
title: "CF 102617I - Kem"
description: "Có một dãy túp lều, mỗi túp lều chứa một số người. Các túp lều cách đều nhau và thông tin đầu vào là dân số của mỗi túp lều. Các cửa hàng kem hiện tại cũng được đặt trên cùng một đường ở tọa độ nhất định. Chúng ta cần chọn tọa độ của một cửa hàng mới."
date: "2026-08-01T07:14:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102617
codeforces_index: "I"
codeforces_contest_name: "mBIT Rookie November 2019"
rating: 0
weight: 102617
solve_time_s: 61
verified: true
draft: false
---

[CF 102617I - Kem](https://codeforces.com/problemset/problem/102617/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Có một dãy túp lều, mỗi túp lều chứa một số người. Các túp lều cách đều nhau và thông tin đầu vào là dân số của mỗi túp lều. Các cửa hàng kem hiện tại cũng được đặt trên cùng một đường ở tọa độ nhất định. 

Chúng ta cần chọn tọa độ của một cửa hàng mới. Một người chỉ mua hàng từ cửa hàng mới nếu cửa hàng mới ở gần túp lều của họ hơn mọi cửa hàng hiện có. Mục tiêu là tìm ra tổng số túp lều tối đa có thể chiếm được bằng cách chọn vị trí tốt nhất có thể cho cửa hàng mới. 

Số chòi và cửa hàng hiện có đều có thể lên tới 200.000. Điều đó ngay lập tức loại trừ việc kiểm tra mọi vị trí có thể và so sánh nó với tất cả các túp lều, bởi vì có vô số tọa độ thực có thể có cho cửa hàng mới. Ngay cả việc kiểm tra từng cặp lều và cửa hàng cũng sẽ quá chậm. Chúng ta cần một giải pháp gần với tuyến tính hoặc dựa trên sắp xếp, xung quanh$O(n \log n)$. 

Khó khăn chính là từ "nghiêm ngặt". Nếu cửa hàng mới cách túp lều bằng khoảng cách với cửa hàng hiện có thì người đó không được tính. Một giải pháp coi các khoảng là các khoảng đóng sẽ đánh giá quá cao câu trả lời. 

Ví dụ:```
Input
2 1
5 7
100

Output
7
```Túp lều ở vị trí 0 và 100, còn cửa hàng hiện tại ở vị trí 100. Túp lều thứ hai có khoảng cách 0 đến cửa hàng hiện có nên không bao giờ có thể thắng được. Chỉ có thể chiếm được túp lều đầu tiên. Việc thực hiện bất cẩn để cho phép buộc có thể tính không chính xác cả hai túp lều. 

Một trường hợp khác là khi nhiều túp lều có cùng khu vực tốt nhất có thể.```
Input
3 1
2 5 6
169

Output
7
```Túp lều thứ nhất và túp lều thứ hai đều có thể chiếm được bằng cách đặt cửa hàng mới gần phía bên trái. Túp lều thứ ba không thể. Giải pháp chỉ kiểm tra các vị trí bên cạnh các cửa hàng hiện tại có thể bỏ lỡ điểm tốt nhất vì vị trí tối ưu có thể ở bất kỳ đâu trong một khoảng thời gian liên tục. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi vị trí có ý nghĩa cho cửa hàng mới. Giữa hai cửa hàng hiện có liên tiếp, nhóm người thích cửa hàng mới không thay đổi liên tục trừ khi đi qua điểm mà một số túp lều bị trói. Chúng tôi có thể tạo ra nhiều điểm ứng viên, kiểm tra từng ứng viên với từng túp lều và giữ mức tối đa. 

Điều này hoạt động về mặt khái niệm vì câu trả lời phải đến từ một trong các vùng được tạo ra bằng cách so sánh khoảng cách. Tuy nhiên, số lượng ứng viên có thể lớn và việc kiểm tra từng ứng viên với tất cả các túp lều dẫn đến nhiều thao tác hơn mức cho phép. Trong trường hợp xấu nhất, điều này trở thành bậc hai hoặc tệ hơn. 

Quan sát hữu ích là đảo ngược quan điểm. Thay vì hỏi vị trí cửa hàng đã chọn có thể chiếm được túp lều nào, hãy hỏi vị trí túp lều đã chọn cho phép đặt cửa hàng mới ở đâu. 

Giả sử một túp lều ở tọa độ$c_i$. Cho phép$d_i$là khoảng cách từ túp lều này đến cửa hàng hiện có gần nhất của nó. Quán mới thắng túp lều này chính xác khi:$$|x-c_i| < d_i$$có nghĩa là:$$c_i-d_i < x < c_i+d_i$$Vì vậy, mỗi túp lều đóng góp một khoảng vị trí mở mà cửa hàng mới sẽ chiếm được túp lều đó. Vấn đề trở thành việc tìm tổng trọng lượng tối đa của các khoảng mở chồng chéo. 

Đây là một vấn đề đường quét có trọng số. Chúng tôi tạo một sự kiện khi một khoảng thời gian bắt đầu và một sự kiện khác khi nó kết thúc. Sự tinh tế duy nhất là xử lý các khoảng thời gian mở một cách chính xác. Ở cùng một tọa độ, các khoảng kết thúc ở đó biến mất trước khi các khoảng bắt đầu ở đó trở nên hoạt động, vì không bao gồm chính ranh giới đó. Sau khi xử lý tất cả các sự kiện tại một tọa độ, tổng hiện tại thể hiện mức độ bao phủ ngay sau tọa độ đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$hoặc tệ hơn |$O(n)$| Quá chậm | 
| Tối ưu |$O((n+m)\log m)$|$O(n+m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tọa độ của tất cả các cửa hàng kem hiện có. Đối với mỗi túp lều, chúng ta cần biết cửa hàng gần nhất và trong danh sách được sắp xếp, bạn có thể tìm thấy cửa hàng này bằng tìm kiếm nhị phân. 
2. Đối với mỗi túp lều, hãy tìm khoảng cách đến cửa hàng hiện có gần nhất. Nếu tọa độ túp lều là$c$và khoảng cách là$d$, cửa hàng mới chỉ có thể chiếm được túp lều này trong khoảng thời gian mở cửa$(c-d,c+d)$. 
3. Bỏ qua túp lều ở đâu$d=0$. Khoảng cách của chúng có độ dài bằng 0 vì cửa hàng hiện tại đã nằm chính xác ở vị trí túp lều. 
4. Thêm sự kiện bắt đầu tại$c-d$và sự kiện kết thúc vào lúc$c+d$. Sức nặng của sự kiện chính là số người trong túp lều đó. 
5. Sắp xếp tất cả tọa độ sự kiện. Đối với mỗi tọa độ, hãy xóa các khoảng kết thúc ở đó trước, sau đó thêm các khoảng bắt đầu từ đó. Sau khi tất cả các sự kiện tại tọa độ đó được xử lý, trọng số hiện tại là số người được ghi lại trong phân đoạn mở tiếp theo. 
6. Theo dõi trọng lượng hiện tại tối đa nhìn thấy trong quá trình quét. 

Tại sao nó hoạt động: 

Mỗi túp lều đóng góp chính xác tập hợp tất cả các vị trí mà một cửa hàng mới sẽ đánh bại mọi cửa hàng hiện có cho túp lều đó. Tổng số người bị bắt bởi một vị trí đã chọn chính xác là tổng trọng số của tất cả các khoảng chứa vị trí đó. Đường quét duy trì tổng này cho mọi vùng có thể có giữa các tọa độ sự kiện liên tiếp. Vì giá trị chỉ có thể thay đổi tại tọa độ sự kiện nên việc kiểm tra từng khu vực như vậy sẽ tìm ra vị trí tối ưu. 

## Giải pháp Python```python
import sys
import bisect

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    people = list(map(int, input().split()))
    shops = list(map(int, input().split()))
    shops.sort()

    events = []

    for i in range(n):
        pos = i * 100
        idx = bisect.bisect_left(shops, pos)

        best = 10**18
        if idx < m:
            best = min(best, abs(shops[idx] - pos))
        if idx > 0:
            best = min(best, abs(shops[idx - 1] - pos))

        if best > 0:
            events.append((pos - best, 1, people[i]))
            events.append((pos + best, -1, people[i]))

    events.sort()

    ans = 0
    cur = 0
    i = 0
    while i < len(events):
        x = events[i]

        while i < len(events) and events[i][0] == x[0] and events[i][1] == -1:
            cur -= events[i][2]
            i += 1

        j = i
        while j < len(events) and events[j][0] == x[0]:
            j += 1

        while i < j:
            cur += events[i][2]
            i += 1

        ans = max(ans, cur)

    print(ans)

if __name__ == "__main__":
    solve()
```Tọa độ cửa hàng được sắp xếp trước vì các truy vấn lân cận gần nhất trên một dòng có thể được trả lời bằng tìm kiếm nhị phân. Tọa độ của túp lều không cần phải được lưu trữ riêng vì túp lều$i$luôn ở tọa độ$100i$khi sử dụng lập chỉ mục dựa trên số không. 

Việc tạo khoảng thời gian là sự chuyển đổi cốt lõi. Khoảng cách đến cửa hàng hiện tại gần nhất xác định chính xác khoảng cách mà cửa hàng mới có thể di chuyển đi mà vẫn được ưu tiên. Vì khoảng mở nên khoảng cách bằng 0 không tạo ra vị trí hợp lệ và bị bỏ qua. 

Thứ tự quét là phần có nhiều khả năng gây ra sai sót nhất. Sự kiện kết thúc phải được áp dụng trước sự kiện bắt đầu ở cùng tọa độ. Nếu quá trình bắt đầu được xử lý trước, hai khoảng thời gian chạm nhau chẳng hạn như$(-1,0)$Và$(0,1)$sẽ xuất hiện chồng chéo không chính xác ở tọa độ 0. 

Các số nguyên Python có độ chính xác tùy ý, do đó tổng dân số lớn có thể không yêu cầu bất kỳ xử lý đặc biệt nào. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3 1
2 5 6
169
```Các túp lều nằm ở tọa độ 0, 100 và 200. Khoảng cách cửa hàng gần nhất là 169, 69 và 31. 

| Phối hợp sự kiện | Đã loại bỏ trọng lượng | Đã thêm trọng lượng | Người hiện tại | 
| --- | --- | --- | --- | 
| -169 | 0 | 2 | 2 | 
| 31 | 5 | 0 | 2 | 
| 31 | 0 | 5 | 7 | 
| 131 | 5 | 0 | 2 | 
| 169 | 2 | 0 | 0 | 

Giá trị tối đa là 7, nghĩa là cửa hàng mới có thể chiếm được 2 túp lều đầu tiên. 

Đối với mẫu thứ hai:```
4 2
1 2 7 8
35 157
```Các cửa hàng gần nhất xác định các khoảng thời gian sau. 

| Túp lều | Tọa độ | Khoảng cách đến cửa hàng gần nhất | Khoảng thời gian | Cân nặng | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 35 | (-35,35) | 1 | 
| 2 | 100 | 57 | (43,157) | 2 | 
| 3 | 200 | 43 | (157,243) | 7 | 
| 4 | 300 | 143 | (157,443) | 8 | 

Sự chồng chéo tối đa xảy ra ngay sau tọa độ 157, nơi có sẵn hai túp lều cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n+m)\log m)$| Sắp xếp cửa hàng và sắp xếp các sự kiện theo khoảng thời gian thống trị thời gian chạy | 
| Không gian |$O(n+m)$| Lưu trữ các sự kiện được tạo và sắp xếp vị trí cửa hàng | 

Với 200.000 túp lều và cửa hàng, việc sắp xếp khoảng 400.000 sự kiện có thể dễ dàng nằm trong giới hạn cuộc thi thông thường. 

## Trường hợp thử nghiệm```python
import sys, io, bisect

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)

    def solve():
        import sys, bisect
        input = sys.stdin.readline
        n, m = map(int, input().split())
        people = list(map(int, input().split()))
        shops = sorted(map(int, input().split()))

        events = []
        for i, p in enumerate(people):
            pos = i * 100
            k = bisect.bisect_left(shops, pos)
            d = 10**18
            if k < m:
                d = min(d, abs(shops[k] - pos))
            if k:
                d = min(d, abs(shops[k-1] - pos))
            if d:
                events.append((pos-d, 1, p))
                events.append((pos+d, -1, p))

        events.sort()
        cur = ans = 0
        i = 0
        while i < len(events):
            x = events[i][0]
            while i < len(events) and events[i][0] == x and events[i][1] == -1:
                cur -= events[i][2]
                i += 1
            while i < len(events) and events[i][0] == x:
                cur += events[i][2]
                i += 1
            ans = max(ans, cur)

        return str(ans) + "\n"

    out = solve()
    sys.stdin = old
    return out

assert run("""3 1
2 5 6
169
""") == "7\n", "sample 1"

assert run("""4 2
1 2 7 8
35 157
""") == "15\n", "sample 2"

assert run("""2 1
5 7
100
""") == "5\n", "existing shop on hut"

assert run("""3 2
1 1 1
0 200
""") == "1\n", "two boundary shops"

assert run("""5 1
10 10 10 10 10
1000
""") == "50\n", "all huts captured"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cửa hàng hiện có trên túp lều | 5 | Xử lý các khoảng có độ dài bằng 0 | 
| Hai cửa hàng ranh giới | 1 | Kiểm tra so sánh chặt chẽ và khoảng thời gian chạm | 
| Tất cả các quần thể bình đẳng | 50 | Kiểm tra số tiền lớn và khoảng thời gian chồng chéo | 

## Vỏ cạnh 

Khi cửa hàng hiện tại nằm chính xác trong một túp lều, khoảng cách đến cửa hàng gần nhất bằng không. Khoảng này trở nên trống và thuật toán sẽ bỏ qua nó. Ví dụ:```
2 1
5 7
100
```Túp lều thứ hai không thể chiếm được vì khoảng cách của nó với cửa hàng hiện tại đã bằng 0. Cuộc quét chỉ nhận được khoảng thời gian từ túp lều đầu tiên và trả về 5. 

Khi hai khoảng chạm vào một ranh giới, thuật toán không được tính cả hai cùng một lúc. Ví dụ, một túp lều có thể cho phép các vị trí trong$(-10,0)$trong khi một cái khác cho phép các vị trí trong$(0,10)$. Bản thân tọa độ 0 không thỏa mãn khoảng nào, vì vậy câu trả lời không thể bao gồm cả hai trọng số. Việc xử lý các sự kiện kết thúc trước sự kiện bắt đầu sẽ duy trì quy tắc này. 

Khi nhiều túp lều có thể bị chiếm giữ bởi cùng một khu vực, việc quét sẽ tích lũy trọng lượng của chúng một cách tự nhiên. Trong mẫu đầu tiên, khoảng cách của hai túp lều đầu tiên trùng nhau, do đó số lượng của chúng kết hợp lại thành câu trả lời cuối cùng là 7.
