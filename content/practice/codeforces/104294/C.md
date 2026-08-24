---
title: "CF 104294C - Tấn công người khổng lồ"
description: "Chúng ta có ba chuỗi riêng biệt, mỗi chuỗi đại diện cho chiều cao của phần tường. Mỗi bức tường đều có số phần như nhau nhưng thứ tự không liên quan đến nhiệm vụ. Điều quan trọng chỉ là độ cao nào xuất hiện trên mỗi bức tường ít nhất một lần."
date: "2026-07-01T20:23:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104294
codeforces_index: "C"
codeforces_contest_name: "UTPC Spring 2023 Open Contest"
rating: 0
weight: 104294
solve_time_s: 70
verified: true
draft: false
---

[CF 104294C - Tấn công người khổng lồ](https://codeforces.com/problemset/problem/104294/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có ba chuỗi riêng biệt, mỗi chuỗi đại diện cho chiều cao của phần tường. Mỗi bức tường đều có số phần như nhau nhưng thứ tự không liên quan đến nhiệm vụ. Điều quan trọng chỉ là độ cao nào xuất hiện trên mỗi bức tường ít nhất một lần. 

Mục tiêu là tìm giá trị chiều cao lớn nhất xuất hiện đồng thời ở cả ba bức tường. Nói cách khác, chúng ta đang tìm kiếm một giá trị tồn tại ở đâu đó trong mảng đầu tiên, ở đâu đó trong mảng thứ hai và ở đâu đó trong mảng thứ ba, và trong số tất cả các giá trị như vậy, chúng ta muốn giá trị lớn nhất. Nếu không có giá trị nào tồn tại thì câu trả lời được xác định là -1. 

Các ràng buộc cho phép tối đa 100000 phần tử trên mỗi bức tường và mỗi chiều cao tối đa là 100000. Điều này ngay lập tức loại trừ mọi cách tiếp cận so sánh trực tiếp từng cặp hoặc bộ ba phần tử. Trong trường hợp xấu nhất, một chiến lược bậc ba hoặc thậm chí bậc hai sẽ bao gồm tối đa 10^10 thao tác, vượt xa những gì phù hợp trong 2 giây trong Python. Ngay cả việc sắp xếp từng mảng cũng được, nhưng việc quét lặp đi lặp lại hoặc kiểm tra tư cách thành viên lồng nhau đối với danh sách Python sẽ trở nên quá chậm nếu được thực hiện nhiều lần. 

Sự tinh tế chính là các bản sao không quan trọng ngoài sự tồn tại. Nếu chiều cao xuất hiện nhiều lần trên tường, nó vẫn chỉ đóng góp một sự hiện diện hợp lý. Một trường hợp cạnh khác là khi giao lộ trống. Ví dụ: nếu ba mảng rời rạc như [1], [2], [3], thì kết quả đầu ra đúng là -1 và bất kỳ cách triển khai nào khởi tạo câu trả lời ứng viên thành 0 sẽ trả về 0 không chính xác nếu không cẩn thận. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là coi đây là vấn đề giao nhau giữa các thành viên. Đối với mọi giá trị ở bức tường đầu tiên, chúng ta có thể kiểm tra xem nó có xuất hiện ở bức tường thứ hai và thứ ba hay không. Điều này đúng vì nó kiểm tra rõ ràng điều kiện tồn tại trong cả ba bộ. Tuy nhiên, việc kiểm tra tư cách thành viên đơn giản trong danh sách có chi phí O(n), do đó, đối với mỗi phần tử trong số n phần tử trong mảng đầu tiên, chúng tôi có thể quét hai mảng bổ sung. Điều này dẫn đến hành vi O(n^2) cho mỗi trường hợp thử nghiệm, trong trường hợp xấu nhất sẽ có khoảng 10^10 thao tác, quá chậm. 

Quan sát quan trọng là chúng ta không cần thông tin vị trí hoặc bội số, chỉ cần liệu giá trị có tồn tại hay không. Điều này cho phép chúng ta chuyển đổi từng mảng thành một tập hợp các giá trị riêng biệt. Khi chúng tôi thực hiện điều đó, việc kiểm tra tư cách thành viên sẽ trở thành thời gian trung bình O(1). Sau đó, bài toán giảm xuống việc tính giao của ba tập hợp và chọn phần tử lớn nhất từ ​​kết quả. Vì các giá trị được giới hạn bởi 100000 nên chúng ta cũng có thể đánh dấu trực tiếp sự hiện diện bằng cách sử dụng mảng boolean, nhưng việc đặt giao điểm đơn giản hơn và đủ nhanh. 

Cách tiếp cận bạo lực hoạt động về mặt khái niệm vì nó trực tiếp kiểm tra tất cả các ứng cử viên, nhưng không thành công trong điều kiện hạn chế về thời gian do quét tuyến tính lặp đi lặp lại. Cách tiếp cận dựa trên tập hợp làm giảm mỗi lần kiểm tra thành viên về thời gian không đổi, biến vấn đề thành quét tuyến tính trên tối đa n giá trị trên mỗi tập hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Sử dụng bộ | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc ba mảng tượng trưng cho các phần tường. Thứ tự của các giá trị không liên quan nên chúng tôi chỉ tập trung vào sự hiện diện. 
2. Chuyển từng mảng thành một tập hợp. Điều này loại bỏ các bản sao và chuẩn bị kiểm tra tư cách thành viên nhanh chóng. Bước này đảm bảo rằng các lần tra cứu sự tồn tại trong một bức tường trong tương lai ở mức trung bình là O(1). 
3. Lặp lại phần nhỏ nhất trong ba tập hợp, vì bất kỳ chiều cao chung hợp lệ nào cũng phải xuất hiện ở đó. Đối với mỗi giá trị ứng cử viên, hãy kiểm tra xem nó có tồn tại trong hai bộ còn lại hay không. 
4. Duy trì một biến theo dõi chiều cao hợp lệ tối đa được tìm thấy cho đến nay. Mỗi lần chúng tôi tìm thấy một giá trị có trong cả ba bộ, chúng tôi sẽ so sánh nó với giá trị tối đa hiện tại và cập nhật nó nếu nó lớn hơn. 
5. Sau khi kiểm tra tất cả các ứng viên, in ra giá trị lớn nhất tìm được. Nếu không có giá trị nào hợp lệ, xuất -1. 

Tại sao nó hoạt động: thuật toán dựa trên thực tế là mọi câu trả lời hợp lệ phải thuộc về giao điểm của ba bộ. Bằng cách giới hạn không gian tìm kiếm trong một bộ và xác minh tư cách thành viên trong các bộ khác, chúng tôi đảm bảo tính đầy đủ mà không bỏ sót bất kỳ ứng cử viên nào. Điều bất biến được duy trì là tại bất kỳ thời điểm nào, giá trị tốt nhất hiện tại là giá trị tối đa trong số tất cả các phần tử giao nhau được thấy cho đến nay. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
m = list(map(int, input().split()))
r = list(map(int, input().split()))
s = list(map(int, input().split()))

set_m = set(m)
set_r = set(r)
set_s = set(s)

ans = -1

for x in set_m:
    if x in set_r and x in set_s:
        if x > ans:
            ans = x

print(ans)
```Giải pháp đọc ba mảng và ngay lập tức chuyển đổi chúng thành các bộ, vì chỉ sự hiện diện mới quan trọng. Vòng lặp kết thúc`set_m`đảm bảo chúng tôi chỉ xem xét các chiều cao ứng cử viên riêng biệt một lần. Mỗi thành viên kiểm tra chống lại`set_r`Và`set_s`chạy trong thời gian không đổi trung bình, giúp cho lời giải tổng thể tuyến tính. Biến trả lời bắt đầu từ -1 để xử lý chính xác trường hợp không tồn tại giao điểm. 

Một lỗi phổ biến là lặp lại các mảng ban đầu thay vì các tập hợp, điều này có thể dẫn đến việc kiểm tra dư thừa. Một vấn đề tế nhị khác là khởi tạo`ans`đến 0, điều này sẽ không chính xác nếu tất cả các giá trị đều âm trong một bài toán biến thể, nhưng ở đây nó chỉ an toàn vì bài toán đảm bảo độ cao dương; Tuy nhiên, -1 vẫn là trọng điểm chính xác cho "không có giao lộ". 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
m = [1, 2, 3, 8, 5]
r = [5, 6, 7, 8, 9]
s = [8, 12, 14, 19, 12]
```Sau khi chuyển đổi thành bộ:```
set_m = {1, 2, 3, 5, 8}
set_r = {5, 6, 7, 8, 9}
set_s = {8, 12, 14, 19}
```Chúng tôi lặp đi lặp lại`set_m`: 

| x | trong set_r | trong set_s | ứng cử viên | 
| --- | --- | --- | --- | 
| 1 | không | không | bỏ qua | 
| 2 | không | không | bỏ qua | 
| 3 | không | không | bỏ qua | 
| 5 | vâng | không | bỏ qua | 
| 8 | vâng | vâng | 8 | 

Câu trả lời cuối cùng là 8. 

Điều này xác nhận rằng thuật toán chỉ xác định chính xác các giá trị có trong cả ba bộ và chọn mức tối đa trong số chúng. 

### Ví dụ 2 

đầu vào:```
m = [4, 4, 2]
r = [1, 2, 3]
s = [2, 9, 10]
```Bộ:```
set_m = {2, 4}
set_r = {1, 2, 3}
set_s = {2, 9, 10}
```| x | trong set_r | trong set_s | ứng cử viên | 
| --- | --- | --- | --- | 
| 2 | vâng | vâng | 2 | 
| 4 | không | không | bỏ qua | 

Câu trả lời là 2. 

Điều này chứng tỏ rằng các bản sao trong mảng đầu vào không ảnh hưởng đến tính chính xác, vì việc chuyển đổi tập hợp sẽ tự động loại bỏ sự lặp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi mảng được quét một lần để xây dựng các tập hợp, sau đó chúng tôi lặp lại một tập hợp với kiểm tra tư cách thành viên O(1) | 
| Không gian | O(n) | chúng tôi lưu trữ tối đa n giá trị riêng biệt trên ba bộ | 

Các ràng buộc cho phép tối đa 100000 phần tử trên mỗi mảng, do đó thời gian tuyến tính và bộ nhớ tuyến tính vừa vặn thoải mái trong giới hạn trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    m = list(map(int, input().split()))
    r = list(map(int, input().split()))
    s = list(map(int, input().split()))

    set_m = set(m)
    set_r = set(r)
    set_s = set(s)

    ans = -1
    for x in set_m:
        if x in set_r and x in set_s:
            ans = max(ans, x)

    return str(ans)

# provided sample
assert run("""5
1 2 3 8 5
5 6 7 8 9
8 12 14 19 12
""") == "8"

# all disjoint
assert run("""3
1 2 3
4 5 6
7 8 9
""") == "-1"

# single common value
assert run("""4
10 20 30 40
5 10 6 7
10 8 9 1
""") == "10"

# duplicates everywhere
assert run("""5
1 1 2 2 3
3 3 2 2 1
2 2 2 2 2
""") == "2"

# all same
assert run("""3
7 7 7
7 7 7
7 7 7
""") == "7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bộ rời rạc | -1 | không có trường hợp giao nhau | 
| Chồng chéo hỗn hợp | 10 | giao lộ tối đa chính xác | 
| Trùng lặp nặng | 2 | tính đúng đắn của sự trùng lặp | 
| Giá trị thống nhất | 7 | trường hợp giao nhau đầy đủ | 

## Vỏ cạnh 

Trường hợp một cạnh là khi không có giá trị chung nào cả. Ví dụ:```
3
1 2 3
4 5 6
7 8 9
```Sau khi chuyển đổi thành tập hợp, không có phần tử nào của`set_m`xuất hiện ở cả hai`set_r`Và`set_s`. Vòng lặp không bao giờ cập nhật`ans`, vì vậy nó vẫn là -1, điều này đúng. 

Một trường hợp khác là khi tất cả các mảng đều có cùng giá trị lặp lại:```
3
5 5 5
5 5 5
5 5 5
```Tất cả các bộ trở thành`{5}`. Vòng lặp kiểm tra 5, xác nhận nó tồn tại trong cả hai bộ khác và cập nhật`ans`thành 5. Không có giá trị nào khác tồn tại nên đầu ra vẫn là 5. 

Trường hợp tinh vi cuối cùng là khi các bản sao được trộn lẫn với một giá trị giao nhau hợp lệ duy nhất, chẳng hạn như:```
5
1 1 1 2 2
3 2 2 2 4
2 5 6 2 7
```Ở đây chỉ có 2 là phổ biến. Mặc dù nó xuất hiện nhiều lần, chuyển đổi tập hợp đảm bảo nó được đánh giá một lần và thuật toán trả về chính xác 2.
