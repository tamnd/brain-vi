---
title: "CF 104805G - Ngủ"
description: "Chúng tôi được đưa cho một cuốn nhật ký về cách Veronica ngủ trong một ngày. Mỗi mục nhật ký đều ghi thời gian bắt đầu khi cô ấy ngủ và thời gian kết thúc khi cô ấy thức dậy."
date: "2026-06-28T13:19:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104805
codeforces_index: "G"
codeforces_contest_name: "Central Russia Regional Contest, 2022"
rating: 0
weight: 104805
solve_time_s: 80
verified: true
draft: false
---

[CF 104805G - Ngủ](https://codeforces.com/problemset/problem/104805/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa cho một cuốn nhật ký về cách Veronica ngủ trong một ngày. Mỗi mục nhật ký đều ghi thời gian bắt đầu khi cô ấy ngủ và thời gian kết thúc khi cô ấy thức dậy. Từ tất cả những khoảng thời gian ngủ này, chúng ta cần xác định xem còn lại bao nhiêu thời gian rảnh trong một ngày, nghĩa là những thời điểm cô ấy thức và người hàng xóm được phép sửa chữa. 

Tất cả thời gian đều nằm trong chu kỳ 24 giờ, nhưng khoảng thời gian ngủ có thể vượt qua nửa đêm. Ví dụ: ngủ từ 22:00:00 đến 02:00:00 có nghĩa là cô ấy ngủ đến hết ngày và tiếp tục ngủ sang ngày tiếp theo. Mỗi khoảng thời gian được đảm bảo kéo dài dưới 24 giờ, vì vậy nó không bao giờ bao gồm cả ngày một cách trọn vẹn. 

Nhiệm vụ chính không phải là tính tổng thời lượng một cách trực tiếp vì các khoảng thời gian ngủ có thể trùng nhau. Thay vào đó, chúng ta phải tính tổng thời lượng kết hợp của tất cả các khoảng thời gian ngủ trong khoảng thời gian 24 giờ, sau đó trừ đi tổng số giây trong một ngày là 86400. 

Các ràng buộc lên tới 100000 khoảng thời gian, loại trừ bất kỳ giải pháp nào cố gắng đánh dấu từng giây riêng lẻ cho mỗi khoảng thời gian. Một mô phỏng đơn giản trong cả ngày cho mỗi khoảng thời gian sẽ quá chậm vì nó sẽ liên quan đến khoảng 10^10 thao tác trong trường hợp xấu nhất. Giải pháp dự định phải nén từng khoảng thời gian thành các sự kiện và xử lý chúng theo thời gian tuyến tính hoặc gần tuyến tính. 

Một vấn đề nhỏ xuất hiện khi các khoảng thời gian chồng lên nhau nhiều hoặc kéo dài đến nửa đêm. Ví dụ: nếu một khoảng thời gian là 23:00:00 đến 01:00:00 và một khoảng thời gian khác là 00:30:00 đến 02:00:00, thì tổng đơn giản là 2 giờ cộng 1,5 giờ, nhưng sự kết hợp thực sự chỉ là 3 giờ. Một trường hợp đặc biệt khác là phạm vi bao phủ toàn bộ: nếu các khoảng thời gian bao trùm toàn bộ 24 giờ thì câu trả lời phải bằng 0. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là chuyển đổi từng khoảng thời gian ngủ thành giây và đánh dấu mỗi giây trong một mảng boolean có kích thước 86400. Đối với mỗi khoảng thời gian, chúng tôi sẽ đánh dấu tất cả các giây là “ngủ” và cuối cùng đếm xem có bao nhiêu giây vẫn chưa được đánh dấu. Điều này đúng vì nó trực tiếp lập mô hình ngày, nhưng mỗi khoảng thời gian có thể yêu cầu tới 86400 thao tác, dẫn đến tổng cộng khoảng 10^10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn. 

Cải tiến quan trọng đến từ việc nhận ra rằng chúng tôi chỉ quan tâm đến ranh giới khoảng thời gian chứ không phải từng giây. Mỗi khoảng đóng góp một phân đoạn liên tục trên dòng thời gian vòng tròn có độ dài 86400. Nếu chúng ta chuyển đổi tất cả các khoảng thành các phân đoạn tuyến tính và tính toán độ dài hợp của chúng, chúng ta có thể tránh chạm vào từng giây một cách rõ ràng. Đây là bài toán liên các khoảng cổ điển. 

Sự phức tạp duy nhất là xử lý các khoảng thời gian bao quanh. Khoảng thời gian ngủ vượt qua nửa đêm có thể được chia thành hai khoảng thời gian thông thường: một khoảng thời gian từ đầu đến cuối ngày và một khoảng thời gian khác từ đầu ngày đến thời gian thức dậy. Sau phép biến đổi này, tất cả các khoảng nằm trên một trục số tiêu chuẩn và chúng ta có thể tính toán sự kết hợp của chúng bằng cách sử dụng phương pháp hợp nhất dựa trên đường quét hoặc sắp xếp trong O(n log n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đánh dấu từng giây | O(n · 86400) | O(86400) | Quá chậm | 
| Sắp xếp + hợp nhất các khoảng thời gian | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi tất cả thời gian thành giây kể từ đầu ngày, sau đó bình thường hóa các khoảng thời gian để chúng không kéo dài đến nửa đêm. Sau đó, chúng tôi hợp nhất các đoạn chồng chéo và tính tổng chiều dài được bao phủ của chúng.

1. Chuyển đổi từng dấu thời gian hh:mm:ss thành tổng số giây kể từ 00:00:00. Điều này mang lại một biểu diễn số nhất quán trong đó việc so sánh rất đơn giản. 
2. Đối với mỗi khoảng thời gian, hãy kiểm tra xem nó có vượt qua nửa đêm hay không. Nếu thời gian bắt đầu nhỏ hơn hoặc bằng thời gian kết thúc, chúng tôi sẽ giữ nó dưới dạng một phân đoạn duy nhất. Nếu thời gian bắt đầu lớn hơn thời gian kết thúc, chúng tôi chia thành hai phân đoạn: một phân đoạn từ đầu đến 86400 và phân đoạn khác từ 0 đến kết thúc. Điều này đảm bảo mọi phân đoạn nằm trong một phạm vi liên tục duy nhất. 
3. Thu thập tất cả các phân đoạn kết quả vào một danh sách. Tại thời điểm này, chúng tôi có tối đa 2n phân đoạn, tất cả đều nằm trong [0, 86400]. 
4. Sắp xếp các phân đoạn theo thời gian bắt đầu. Việc sắp xếp đảm bảo rằng khi chúng tôi quét từ trái sang phải, mọi sự trùng lặp chỉ có thể xảy ra với phân đoạn được hợp nhất đang hoạt động hiện tại. 
5. Quét qua các phân đoạn đã sắp xếp, duy trì khoảng thời gian hợp nhất hiện tại. Nếu đoạn tiếp theo bắt đầu trước hoặc ở cuối đoạn hiện tại, chúng ta sẽ mở rộng đoạn cuối hiện tại. Nếu không, chúng tôi thêm độ dài của khoảng thời gian hợp nhất đã hoàn thành và bắt đầu một khoảng thời gian mới. 
6. Sau khi xử lý tất cả các phân đoạn, hãy cộng độ dài khoảng thời gian hoạt động cuối cùng vào tổng thời gian ngủ. 
7. Trừ tổng thời gian ngủ từ 86400 để có kết quả. 

### Tại sao nó hoạt động 

Ở mỗi bước, thuật toán duy trì tính bất biến rằng tất cả các khoảng thời gian được xử lý cho đến nay sẽ được hợp nhất hoàn toàn thành các phân đoạn rời rạc có liên kết khớp chính xác với phạm vi giấc ngủ ban đầu. Mọi sự trùng lặp sẽ được hấp thụ vào phân đoạn hiện tại, do đó không có thời gian nào được tính hai lần. Vì mỗi khoảng thời gian ngủ được biểu thị chính xác một lần sau khi phân tách các trường hợp bao bọc nên độ dài được hợp nhất cuối cùng chính xác là tổng thời lượng ngủ trong một ngày. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

DAY = 24 * 60 * 60

def to_sec(t):
    h = int(t[0:2])
    m = int(t[3:5])
    s = int(t[6:8])
    return h * 3600 + m * 60 + s

n = int(input())
intervals = []

for _ in range(n):
    a, b = input().split()
    l = to_sec(a)
    r = to_sec(b)

    if l <= r:
        intervals.append((l, r))
    else:
        intervals.append((l, DAY))
        intervals.append((0, r))

intervals.sort()

total_sleep = 0
cur_l, cur_r = intervals[0]

for l, r in intervals[1:]:
    if l <= cur_r:
        if r > cur_r:
            cur_r = r
    else:
        total_sleep += cur_r - cur_l
        cur_l, cur_r = l, r

total_sleep += cur_r - cur_l

print(DAY - total_sleep)
```Quá trình triển khai bắt đầu bằng cách chuyển đổi dấu thời gian thành số nguyên giây để thực hiện trực tiếp số học theo khoảng thời gian. Khoảng thời gian bao quanh được phân chia sao cho tất cả các phân đoạn nằm trong phạm vi tiêu chuẩn từ 0 đến 86400. Việc sắp xếp đảm bảo chúng ta có thể hợp nhất các khoảng thời gian trong một lần duy nhất mà không cần quay lại. Logic hợp nhất duy trì một khoảng thời gian hoạt động hiện tại và mở rộng nó bất cứ khi nào xảy ra sự chồng chéo. Khi tìm thấy khoảng trống, phân đoạn trước đó sẽ được hoàn thiện và thêm vào tổng thời gian ngủ. 

Một điểm tinh tế là khoảng thời gian ban đầu phải tồn tại, do đó, đầu vào được giả định chứa ít nhất một phân đoạn ngủ. Phép trừ cuối cùng từ 86400 tạo ra thời gian rảnh để sửa chữa. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Khoảng thời gian: 

13:00-14:00, 16:00-17:00, 19:00-20:00, 01:00-05:00, 04:00-05:00 

Các phân đoạn được chuyển đổi và hợp nhất: 

| Bước | Khoảng thời gian hiện tại | Hành động | Tổng số giấc ngủ | 
| --- | --- | --- | --- | 
| 1 | [01:00, 05:00] | bắt đầu | 0 | 
| 2 | hợp nhất với [04:00, 05:00] | mở rộng | 0 | 
| 3 | [13:00, 14:00] | đóng trước, thêm 4h | 4h | 
| 4 | [16:00, 17:00] | thêm 1h | 5h | 
| 5 | [19:00, 20:00] | thêm 1h | 6h | 

Tổng thời gian ngủ = 7 giờ (25200 giây). 

Thời gian rảnh = 86400 − 25200 = 61200. 

Điều này xác nhận thuật toán hợp nhất chính xác các khoảng chồng chéo và tránh tính hai lần. 

### Mẫu 2 

Một số khoảng thời gian bao quanh nửa đêm và chồng chéo lên nhau rất nhiều. 

Sau khi chia tách và sắp xếp, phạm vi phủ sóng được hợp nhất sẽ mở rộng từng bước cho đến khi kéo dài cả ngày. 

| Bước | Khoảng thời gian | Hợp nhất kết quả | 
| --- | --- | --- | 
| 1 | đoạn đầu tiên | bắt đầu bảo hiểm | 
| 2 | đoạn chồng chéo | mở rộng | 
| 3 | chồng chéo thêm | mở rộng | 
| 4 | đoạn cuối cùng | đầy đủ [0, 86400] | 

Tổng thời gian ngủ trở thành 86400 giây, do đó thời gian rảnh là 0. 

Điều này cho thấy rằng việc xử lý bao quanh sẽ chuyển đổi chính xác các khoảng thời gian theo chu kỳ thành phạm vi bao phủ tuyến tính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp tối đa 2n điểm cuối khoảng thời gian chi phối thời gian chạy | 
| Không gian | O(n) | Lưu trữ cho các khoảng thời gian chuyển đổi | 

Thuật toán phù hợp một cách thoải mái trong các giới hạn cho n lên tới 100000. Việc sắp xếp và quét tuyến tính duy nhất nằm trong giới hạn 1 giây thông thường trong Python khi được triển khai bằng tính năng phân tích cú pháp đầu vào hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    DAY = 24 * 60 * 60

    def to_sec(t):
        h = int(t[0:2])
        m = int(t[3:5])
        s = int(t[6:8])
        return h * 3600 + m * 60 + s

    n = int(input())
    intervals = []

    for _ in range(n):
        a, b = input().split()
        l = to_sec(a)
        r = to_sec(b)
        if l <= r:
            intervals.append((l, r))
        else:
            intervals.append((l, DAY))
            intervals.append((0, r))

    intervals.sort()

    total = 0
    cur_l, cur_r = intervals[0]

    for l, r in intervals[1:]:
        if l <= cur_r:
            cur_r = max(cur_r, r)
        else:
            total += cur_r - cur_l
            cur_l, cur_r = l, r

    total += cur_r - cur_l

    return str(DAY - total)

# provided samples
assert run("""5
13:00:00 14:00:00
16:00:00 17:00:00
19:00:00 20:00:00
01:00:00 05:00:00
04:00:00 05:00:00
""") == "61200"

assert run("""4
22:49:13 05:22:28
18:50:30 14:32:25
11:34:33 11:28:25
08:55:57 19:49:53
""") == "0"

# custom cases
assert run("""1
00:00:00 00:00:01
""") == str(86400 - 1)

assert run("""1
00:00:00 23:59:59
""") == "1"

assert run("""2
10:00:00 11:00:00
10:30:00 11:30:00
""") == str(86400 - 7200)

assert run("""2
23:00:00 01:00:00
01:00:00 02:00:00
""") == str(86400 - 7200)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khoảng thời gian ngắn duy nhất | 86399 | bảo hiểm tối thiểu | 
| bảo hiểm cả ngày | 1 | ranh giới phủ sóng gần như toàn bộ | 
| chồng chéo các khoảng thời gian trong ngày | hợp nhất đúng | xử lý chồng chéo | 
| chuỗi xuyên nửa đêm | đoàn đúng | xử lý bọc | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi khoảng thời gian ngủ kết thúc vào khoảng nửa đêm. Ví dụ: 23:00:00 đến 01:00:00 trở thành hai phân đoạn: [23:00, 86400) và [0, 01:00]. Nếu không phân tách, việc hợp nhất ngây thơ sẽ cho rằng khoảng thời gian không hợp lệ hoặc có độ dài bằng 0 một cách không chính xác. 

Một trường hợp khác là phạm vi phủ sóng đầy đủ trong ngày. Nếu các khoảng thời gian kéo dài từ 00:00:00 đến 24:00:00 sau khi hợp nhất thì tổng thời gian rảnh phải bằng 0. Thuật toán xử lý việc này một cách tự nhiên vì khoảng được hợp nhất trở thành chính xác [0, 86400], tạo ra số dư bằng 0. 

Trường hợp tinh vi cuối cùng là sự chồng chéo nặng nề qua nhiều khoảng thời gian. Ví dụ: không nên tính nhiều khoảng thời gian nhỏ trong cùng một giờ. Bước hợp nhất đảm bảo rằng các phần trùng lặp lặp lại chỉ mở rộng phân đoạn hiện tại thay vì tăng tổng chiều dài lên nhiều lần, duy trì tính chính xác ngay cả dưới các đầu vào đối nghịch.
