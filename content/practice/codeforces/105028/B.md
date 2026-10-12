---
title: "CF 105028B - Sao chép trình tự"
description: "Chúng tôi được đưa ra hai chuỗi. Chuỗi đầu tiên a là mẫu chúng ta muốn khớp và chuỗi thứ hai b là chuỗi cơ sở được lặp lại vô tận nhưng chỉ có số lần hữu hạn. Nếu lặp lại b chính xác k lần, chúng ta sẽ có một chuỗi dài gồm k bản sao liên tiếp của b."
date: "2026-06-28T01:36:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105028
codeforces_index: "B"
codeforces_contest_name: "TheForces Round #28 (Epic-Forces)"
rating: 0
weight: 105028
solve_time_s: 79
verified: false
draft: false
---

[CF 105028B - Sao chép trình tự](https://codeforces.com/problemset/problem/105028/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra hai chuỗi. Trình tự đầu tiên`a`là mẫu mà chúng tôi muốn khớp và chuỗi thứ hai`b`là một chuỗi cơ sở được lặp lại vô tận nhưng chỉ có số lần hữu hạn. Nếu chúng ta lặp lại`b`chính xác`k`đôi khi, chúng ta nhận được một chuỗi dài được tạo thành từ`k`bản sao liên tiếp của`b`. 

Nhiệm vụ là tìm số lần lặp lại nhỏ nhất`k`như vậy`a`có thể được tìm thấy dưới dạng một dãy con bên trong dãy lặp lại này. Nếu thậm chí sự lặp lại vô hạn của`b`không thể hình thành`a`như một chuỗi con, chúng ta phải quay lại`-1`. 

Dãy con có nghĩa là chúng ta quét từ trái sang phải và chọn các phần tử mà không cần sắp xếp lại, có thể bỏ qua các phần tử. 

Khó khăn chính đó là`k`không được đưa ra. Chúng ta phải xác định có bao nhiêu lần vượt qua đầy đủ`b`được yêu cầu “tiêu thụ” tất cả các yếu tố của`a`theo thứ tự. 

Các ràng buộc cho phép tổng số lên tới 200.000 phần tử trong tất cả các trường hợp thử nghiệm. Điều đó ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng xây dựng một cách rõ ràng`dup(b, k)`hoặc mô phỏng việc so khớp với các bản sao ngày càng tăng của`b`cho nhiều giá trị ứng viên của`k`. Bất cứ điều gì tệ hơn tuyến tính hoặc gần tuyến tính cho mỗi lần kiểm tra sẽ thất bại. 

Trường hợp cạnh tinh tế xuất hiện khi một số phần tử của`a`không bao giờ xuất hiện ở`b`. Ví dụ,`a = [1]`,`b = [2, 3]`. Việc lặp đi lặp lại không giúp ích được gì và câu trả lời phải là`-1`. Một quá trình quét tham lam ngây thơ đặt lại không chính xác vẫn có thể lặp lại và cho rằng tiến trình có thể xảy ra một cách sai lầm trừ khi nó kiểm tra rõ ràng khả năng tiếp cận trong`b`. 

Một trường hợp phức tạp khác xảy ra khi khớp`a`yêu cầu khởi động lại ở giữa bản sao của`b`. Ví dụ,`b = [1, 2, 3]`,`a = [2, 3, 1, 2]`. Chúng tôi có thể hoàn thành một lượt đầy đủ`b`trước khi hoàn thành`a`, vì vậy chúng ta cần nhiều chu kỳ và chúng ta phải đếm xem chúng ta quấn quanh bao nhiêu lần`b`. 

## Phương pháp tiếp cận 

Một cách tiếp cận mô phỏng trực tiếp rất đơn giản. Chúng tôi cố gắng để phù hợp`a`chống lại`b`, di chuyển con trỏ vào`a`bất cứ khi nào chúng tôi tìm thấy phần tử phù hợp trong`b`. Một khi chúng ta đi đến cuối`b`, chúng tôi tăng bộ đếm số lượng bản sao đầy đủ mà chúng tôi đã sử dụng và bắt đầu quét lại`b`ngay từ đầu. Chúng tôi tiếp tục cho đến khi một trong hai`a`hoàn toàn trùng khớp hoặc chúng tôi phát hiện thấy rằng tiến trình đó là không thể. 

Điều này hiệu quả vì việc so khớp chuỗi con có tính chất tham lam: bất cứ khi nào chúng ta có thể so khớp phần tử tiếp theo của`a`, chúng ta nên khớp nó ở vị trí sớm nhất có thể trong`b`. 

Tuy nhiên, việc triển khai đơn giản sẽ trở nên kém hiệu quả nếu chúng ta khởi động lại quá trình quét`b`quá nhiều lần và liên tục quét lại từ đầu. Trong trường hợp xấu nhất, mỗi phần tử của`a`có thể yêu cầu duyệt qua đầy đủ`b`, dẫn đến hành vi O(nm), quá chậm khi cả hai mảng đều lớn. 

Quan sát quan trọng là hành vi bên trong mỗi lần lặp lại của`b`giống hệt nhau. Thay vì quét liên tục`b`, chúng ta có thể xử lý trước vị trí của từng giá trị trong`b`và “nhảy” tới lần xuất hiện hợp lệ tiếp theo bằng cách sử dụng tìm kiếm nhị phân. Điều này biến mỗi bước khớp`a`thành một phép toán logarit hoặc khấu hao không đổi. 

Chúng tôi duy trì một con trỏ vào một vô hạn ảo`b`. Đối với mỗi`a[i]`, chúng tôi tìm thấy sự xuất hiện tiếp theo trong`b`đó là đúng sau vị trí hiện tại của chúng tôi trong chu kỳ. Nếu không có sự xuất hiện nào như vậy, chúng ta chuyển sang chu kỳ tiếp theo và tiếp tục từ lần xuất hiện đầu tiên trong`b`, tăng dần`k`. 

Điều này làm giảm vấn đề tìm kiếm các vị trí tiếp theo trong một mảng tuần hoàn một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n · k · m) | O(1) | Quá chậm | 
| Vị trí được xử lý trước + Tìm kiếm nhị phân | O((n + m) log m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Chúng tôi xây dựng ánh xạ từ từng giá trị trong`b`vào danh sách các chỉ mục nơi nó xuất hiện. Điều này cho phép tra cứu nhanh chóng nơi chúng ta có thể đặt từng phần tử của`a`. Thứ tự của các chỉ số rất quan trọng vì chúng ta cần vị trí hợp lệ tiếp theo. 
2. Chúng ta khởi tạo hai biến:`pos`, theo dõi chỉ mục hiện tại của chúng tôi bên trong một bản sao của`b`(bắt đầu từ 0) và`k`, đếm xem có bao nhiêu bản sao đầy đủ của`b`chúng tôi đã sử dụng cho đến nay (bắt đầu từ 1 nếu có thể khớp). 
3. Chúng tôi lặp qua từng phần tử`x`TRONG`a`. Đối với mỗi`x`, chúng tôi tham khảo danh sách các vị trí của nó trong`b`. Nếu danh sách này trống, chúng ta kết luận ngay đáp án là`-1`bởi vì`x`không bao giờ xuất hiện ở`b`. 
4. Chúng tôi cố gắng tìm chỉ số nhỏ nhất trong danh sách lớn hơn`pos`. Đây là nơi tiếp theo chúng ta có thể kết hợp`x`mà không vi phạm trật tự bên trong bản sao hiện tại của`b`. Nếu vị trí đó tồn tại, chúng tôi sẽ cập nhật`pos`đến đó và tiếp tục. 
5. Nếu không có vị trí như vậy tồn tại, điều đó có nghĩa là chúng ta phải chuyển sang lần lặp lại tiếp theo của`b`. Chúng tôi tăng`k`, cài lại`pos`đến lần xuất hiện đầu tiên của`x`TRONG`b`, và tiếp tục. Điều này phản ánh việc gói gọn vào một bản sao mới của`b`. 
6. Sau khi xử lý xong tất cả các phần tử của`a`, giá trị cuối cùng của`k`là số lượng bản sao tối thiểu cần thiết. 

### Tại sao nó hoạt động 

Ở mỗi bước, chúng tôi luôn đặt từng phần tử của`a`ở vị trí hợp lệ sớm nhất có thể trong cấu trúc lặp lại. Vị trí tham lam này đảm bảo chúng tôi không bao giờ trì hoãn các trận đấu một cách giả tạo và do đó không bao giờ tăng số lần lặp lại cần thiết một cách không cần thiết. Bất kỳ vị trí thay thế nào làm trì hoãn trận đấu sẽ chỉ đẩy chúng tôi tiến xa hơn`b`và có khả năng yêu cầu một chu kỳ bổ sung sớm hơn, không bao giờ ít hơn. 

Điều bất biến là sau khi xử lý`a[i]`,`pos`là vị trí của`a[i]`trong bản sao sớm nhất có thể của`b`phù hợp với việc xây dựng chuỗi con hợp lệ và`k`đếm chính xác bao nhiêu lần chúng ta đã vượt qua ranh giới của`b`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from bisect import bisect_right

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        pos_map = {}
        for i, x in enumerate(b):
            if x not in pos_map:
                pos_map[x] = []
            pos_map[x].append(i)

        # feasibility check
        for x in a:
            if x not in pos_map:
                print(-1)
                break
        else:
            k = 1
            pos = -1  # current index in b
            for x in a:
                lst = pos_map[x]
                idx = bisect_right(lst, pos)
                if idx < len(lst):
                    pos = lst[idx]
                else:
                    k += 1
                    pos = lst[0]
            print(k)

if __name__ == "__main__":
    solve()
```Mã xây dựng đầu tiên`pos_map`, nhóm các chỉ số của từng giá trị trong`b`. Cấu trúc này cho phép chúng ta tránh việc quét`b`nhiều lần. 

các`pos`các bản nhạc có thể thay đổi nơi chúng tôi khớp lần cuối trong một bản sao của`b`. sử dụng`bisect_right`, chúng tôi tìm thấy lần xuất hiện hợp lệ tiếp theo một cách hiệu quả. Nếu không tồn tại, chúng tôi mô phỏng việc chuyển sang lần lặp lại tiếp theo bằng cách tăng dần`k`và khởi động lại từ lần xuất hiện đầu tiên của giá trị đó. 

Một chi tiết tinh tế là giá trị ban đầu`pos = -1`, điều này đảm bảo phần tử đầu tiên có thể khớp với bất kỳ vị trí nào trong bản sao đầu tiên của`b`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét`a = [1, 2, 3]`,`b = [1, 2]`. 

| Bước | x | tư thế trước | chỉ số đã chọn | k | tư thế sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | -1 | 0 | 1 | 0 | 
| 2 | 2 | 0 | 1 | 1 | 1 | 
| 3 | 3 | 1 | không | - | không thể | 

Chúng tôi thất bại ở`3`bởi vì nó không tồn tại trong`b`, vậy đầu ra là`-1`. 

Điều này xác nhận rằng các phần tử bị thiếu sẽ được phát hiện ngay lập tức và ngăn chặn việc tiếp tục không chính xác. 

### Ví dụ 2 

hãy để`a = [2, 3, 1, 2]`,`b = [1, 2, 3]`. 

| Bước | x | tư thế trước | chỉ số đã chọn | k | tư thế sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | -1 | 1 | 1 | 1 | 
| 2 | 3 | 1 | 2 | 1 | 2 | 
| 3 | 1 | 2 | 0 (chu kỳ tiếp theo) | 2 | 0 | 
| 4 | 2 | 0 | 1 | 2 | 1 | 

Chúng tôi cần 2 bản sao của`b`. 

Dấu vết này cho thấy tại sao cần phải bao bọc và cách thuật toán chỉ đếm chính xác các chu kỳ đầy đủ khi được yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log m) | Mỗi phần tử của`a`thực hiện tìm kiếm nhị phân trong danh sách của nó trong`b`| 
| Không gian | O(m) | Lưu trữ danh sách chỉ mục cho các phần tử của`b`| 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm được giới hạn bởi 200.000, do đó, giải pháp log-tuyến tính dễ dàng phù hợp trong giới hạn thời gian. Việc sử dụng bộ nhớ là tuyến tính trong`m`, cũng an toàn dưới các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io
from bisect import bisect_right

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    import sys
    input = sys.stdin.readline

    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        pos_map = {}
        for i, x in enumerate(b):
            pos_map.setdefault(x, []).append(i)

        for x in a:
            if x not in pos_map:
                print(-1)
                break
        else:
            k = 1
            pos = -1
            for x in a:
                lst = pos_map[x]
                idx = bisect_right(lst, pos)
                if idx < len(lst):
                    pos = lst[idx]
                else:
                    k += 1
                    pos = lst[0]
            print(k)

    return out.getvalue().strip()

# custom tests

# all elements exist, single cycle
assert run("1\n3 3\n1 2 3\n1 2 3\n") == "1"

# needs multiple cycles
assert run("1\n4 3\n2 3 1 2\n1 2 3\n") == "2"

# impossible case
assert run("1\n2 2\n1 2\n3 4\n") == "-1"

# repeated wrapping
assert run("1\n5 2\n1 2 1 2 1\n1 2\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 3 / 1 2 3`|`1`| một lần đủ | 
|`2 3 1 2 / 1 2 3`|`2`| đếm vòng quanh | 
|`1 2 / 3 4`|`-1`| phát hiện không thể | 
|`1 2 1 2 1 / 1 2`|`3`| xử lý chu kỳ lặp đi lặp lại | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi phần tử đầu tiên của`a`không có mặt ở`b`. Ví dụ,`a = [5]`,`b = [1, 2, 3]`. Thuật toán kiểm tra`pos_map`trước khi bất kỳ sự so khớp nào bắt đầu, vì vậy nó ngay lập tức trả về`-1`. Nếu không có kiểm tra này, cách tiếp cận con trỏ đơn giản có thể lặp lại vô thời hạn hoặc đặt lại chu kỳ không chính xác. 

Một trường hợp khác là khi`a`hoàn toàn được chứa trong một lần duyệt duy nhất của`b`, chẳng hạn như`a = [1, 3]`,`b = [1, 2, 3, 4]`. Đây`k`còn lại`1`bởi vì`pos`không bao giờ yêu cầu bọc. Thuật toán không bao giờ tăng`k`, vì mỗi lần xuất hiện tiếp theo đều được tìm thấy trong chu kỳ hiện tại. 

Trường hợp cuối cùng là sự lặp lại nhiều, chẳng hạn như`a = [1, 1, 1, 1]`,`b = [1]`. Mỗi phần tử buộc phải sử dụng lại cùng một chỉ mục hoặc tăng dần theo chu kỳ. Thuật toán tăng chính xác`k`mỗi lần không có sự xuất hiện tiếp theo nào tồn tại, tạo ra`k = 4`, phù hợp với số lần lặp lại cần thiết.
