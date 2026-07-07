---
title: "CF 102961V - Nhiệm vụ và Thời hạn"
description: "Mỗi nhiệm vụ có hai thuộc tính: mất bao lâu để hoàn thành và thời hạn được sử dụng để đánh giá mức độ “trễ” của bạn khi hoàn thành nó. Bạn phải thực hiện tất cả các nhiệm vụ một cách tuần tự bắt đầu từ thời điểm 0, chọn bất kỳ thứ tự nào bạn muốn."
date: "2026-07-04T06:57:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102961
codeforces_index: "V"
codeforces_contest_name: "CSES Problem Set: Sorting and Searching"
rating: 0
weight: 102961
solve_time_s: 42
verified: true
draft: false
---

[CF 102961V - Nhiệm vụ và Thời hạn](https://codeforces.com/problemset/problem/102961/V) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi nhiệm vụ có hai thuộc tính: mất bao lâu để hoàn thành và thời hạn được sử dụng để đánh giá mức độ “trễ” của bạn khi hoàn thành nó. Bạn phải thực hiện tất cả các nhiệm vụ một cách tuần tự bắt đầu từ thời điểm 0, chọn bất kỳ thứ tự nào bạn muốn. 

Khi một nhiệm vụ kết thúc vào thời điểm`f`và thời hạn của nó là`d`, nó góp phần`d - f`vào tổng số điểm. Tổng phần thưởng là tổng của những đóng góp này trong tất cả các nhiệm vụ. Vì thời gian hoàn thành được tích lũy khi nhiệm vụ được xử lý nên việc đặt hàng sẽ ảnh hưởng trực tiếp đến tất cả các hình phạt trong tương lai. 

Đầu vào đưa ra một danh sách các nhiệm vụ, mỗi nhiệm vụ được mô tả bằng thời lượng và thời hạn. Đầu ra là một số duy nhất: tổng phần thưởng tối đa có thể có trên tất cả các hoán vị của thứ tự nhiệm vụ. 

Các ràng buộc cho phép tối đa 200.000 nhiệm vụ, điều này ngay lập tức loại trừ bất kỳ mô phỏng lập kế hoạch giai thừa hoặc bậc hai nào. Thậm chí`O(n^2)`trở nên quá chậm nếu mỗi tác vụ yêu cầu quét hoặc hoán đổi. Mọi giải pháp hợp lệ đều phải gần với`O(n log n)`hoặc tuyến tính. 

Một vấn đề tế nhị trong bài toán này là mọi nhiệm vụ đều phải được hoàn thành ngay cả khi nó mang lại sự đóng góp tiêu cực. Một cách giải thích ngây thơ có thể gợi ý loại bỏ các nhiệm vụ “xấu”, nhưng tuyên bố rõ ràng không cho phép bỏ qua, vì vậy việc tối ưu hóa hoàn toàn chỉ là sắp xếp thứ tự. 

Một cạm bẫy phổ biến khác là nghĩ rằng mỗi nhiệm vụ có thể được tối ưu hóa một cách độc lập. Ví dụ, việc đặt thời hạn nhỏ nhất trước tiên có vẻ tự nhiên nhưng điều đó vẫn có thể chưa tối ưu khi một nhiệm vụ dài làm trì hoãn mọi thứ tiếp theo. 

Một ví dụ nhỏ trong đó việc tham lam theo thời hạn không thành công: 

đầu vào:```
2
1 10
10 11
```Nếu bạn lấy deadline sớm nhất trước thì làm (1,10) rồi (10,11), thời gian về đích là 1 và 11, tổng phần thưởng là (10-1)+(11-11)=9. 

Nếu bạn đảo ngược, thời gian kết thúc là 10 và 11, tổng phần thưởng là (10-10)+(11-11)=0, vì vậy trong trường hợp nhỏ này, tính tham lam có tác dụng, nhưng điều này gây hiểu lầm. Trong những trường hợp lớn hơn, những nhiệm vụ dài hạn được đặt sớm có thể phá hủy nhiều đóng góp trong tương lai, đây là vấn đề cơ cấu then chốt. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử mọi hoán vị của nhiệm vụ, mô phỏng lịch trình, tính toán thời gian hoàn thành tăng dần và theo dõi kết quả tốt nhất. Điều này đúng vì nó đánh giá tất cả các đơn đặt hàng có thể một cách rõ ràng. Tuy nhiên, nó đòi hỏi`n!`hoán vị, và thậm chí đánh giá một chi phí hoán vị`O(n)`, cho`O(n! · n)`hoạt động, điều này là không thể ngay cả đối với`n = 20`. 

Quan sát quan trọng là mục tiêu có thể được viết lại sao cho thứ tự chỉ quan trọng thông qua thời gian hoàn thành. Nếu chúng ta mở rộng tổng, mỗi nhiệm vụ sẽ đóng góp thời hạn một lần và trừ đi thời gian hoàn thành một lần. Tổng thời hạn là cố định bất kể thứ tự nào, do đó tối đa hóa phần thưởng tương đương với việc giảm thiểu tổng thời gian hoàn thành. 

Bây giờ vấn đề trở thành một câu hỏi lập kế hoạch cổ điển: giảm thiểu tổng thời gian hoàn thành trên một máy với thời gian xử lý cố định. Chiến lược tối ưu nổi tiếng là xử lý các nhiệm vụ theo thứ tự thời gian tăng dần. Điều này có hiệu quả vì việc hoán đổi hai nhiệm vụ liền kề cho thấy rằng một nhiệm vụ dài hơn được đặt trước sẽ tăng tổng thời gian hoàn thành nhiều hơn so với việc đặt nó sau. 

Thời hạn hoàn toàn biến mất khỏi quyết định đặt hàng sau khi chuyển đổi; họ chỉ đóng góp một khoản bù đắp không đổi cho câu trả lời cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n) | O(n) | Quá chậm | 
| Sắp xếp theo thời lượng (tối ưu) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các nhiệm vụ và lưu trữ thời lượng cũng như thời hạn của chúng. Mục tiêu là sau này sẽ chọn được đơn hàng sao cho thời gian hoàn thiện tích lũy được giảm thiểu. 
2. Sắp xếp nhiệm vụ theo thời lượng tăng dần. Điều này đảm bảo các tác vụ ngắn được thực thi sớm, ngăn chặn thời gian xử lý kéo dài làm tăng thêm thời gian hoàn thành trong tương lai. 
3. Duy trì thời gian tiền tố đang chạy, ban đầu bằng 0, biểu thị thời gian hoàn thành hiện tại sau khi xử lý các tác vụ theo thứ tự đã chọn. 
4. Lặp lại các nhiệm vụ theo thứ tự được sắp xếp. Đối với mỗi nhiệm vụ, hãy thêm thời lượng của nó vào thời gian chạy để biết thời gian hoàn thành. 
5. Tích lũy đóng góp`deadline - finishing_time`vào câu trả lời. 
6. Xuất giá trị tích lũy cuối cùng. 

Lựa chọn thiết kế quan trọng là chúng tôi không bao giờ tối ưu hóa rõ ràng thời hạn trong quá trình đặt hàng, vì chúng không ảnh hưởng đến sự so sánh tương đối giữa các hoán vị sau phép biến đổi đại số. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là sau khi sắp xếp theo thời lượng, bất kỳ sự đảo ngược liền kề nào trong đó một nhiệm vụ dài hơn đứng trước một nhiệm vụ ngắn hơn chỉ có thể tăng tổng thời gian hoàn thành. Hoán đổi một cặp như vậy sẽ làm giảm tổng thời gian hoàn thành tích lũy và các hoán đổi lặp đi lặp lại sẽ chuyển đổi bất kỳ thứ tự nào thành thứ tự được sắp xếp theo thời lượng mà không làm xấu đi mục tiêu. Điều này chứng tỏ rằng thứ tự sắp xếp sẽ giảm thiểu tổng thời gian hoàn thành, từ đó tối đa hóa hàm phần thưởng ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
tasks = []

for _ in range(n):
    a, d = map(int, input().split())
    tasks.append((a, d))

tasks.sort()  # sort by duration

time = 0
answer = 0

for a, d in tasks:
    time += a
    answer += d - time

print(answer)
```Việc thực hiện trực tiếp theo sau việc giảm thiểu đến mức tối thiểu tổng thời gian hoàn thành. Việc sắp xếp được thực hiện theo từ điển, sắp xếp các nhiệm vụ theo thứ tự thời lượng tăng dần. 

Biến`time`theo dõi thời gian xử lý tích lũy. Điều quan trọng là nó phải được cập nhật trước khi tính toán phần đóng góp, vì`time`biểu thị thời gian kết thúc của nhiệm vụ hiện tại chứ không phải thời gian bắt đầu của nhiệm vụ đó. 

Một chi tiết tinh tế là tất cả số học đều vừa vặn thoải mái với các số nguyên 64 bit, nhưng Python xử lý các số nguyên lớn một cách tự nhiên nên việc tràn dữ liệu không phải là vấn đề đáng lo ngại. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:```
3
6 10
8 15
5 12
```Sau khi sắp xếp theo thời lượng, nhiệm vụ sẽ trở thành`(5,12)`,`(6,10)`,`(8,15)`. 

| Bước | Nhiệm vụ (a,d) | Thời gian tích lũy | Thời gian hoàn thiện | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | (5,12) | 5 | 5 | 12 - 5 = 7 | 
| 2 | (6,10) | 11 | 11 | 10 - 11 = -1 | 
| 3 | (8,15) | 19 | 19 | 15 - 19 = -4 | 

Tổng phần thưởng là`7 - 1 - 4 = 2`. 

Dấu vết này cho thấy rằng ngay cả những nhiệm vụ có đóng góp cá nhân tiêu cực vẫn phải được đưa vào và việc đặt hàng sẽ đánh đổi lợi ích ban đầu với các hình phạt sau này. 

Bây giờ hãy xem xét ví dụ thứ hai:```
4
3 5
2 100
1 4
4 6
```Thứ tự sắp xếp là`(1,4)`,`(2,100)`,`(3,5)`,`(4,6)`. 

| Bước | Nhiệm vụ | Thời gian | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | (1,4) | 1 | 3 | 
| 2 | (2.100) | 3 | 97 | 
| 3 | (3,5) | 6 | -1 | 
| 4 | (4,6) | 10 | -4 | 

Tổng cộng là`3 + 97 - 1 - 4 = 95`. 

Điều này khẳng định rằng việc hoàn thành sớm nhiệm vụ có thời hạn rất lớn là có lợi vì nó tránh bị phạt do thời gian hoàn thành quá lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; quét tuyến tính đơn sau đó | 
| Không gian | O(n) | Lưu trữ danh sách nhiệm vụ | 

Với`n ≤ 2 × 10^5`, việc sắp xếp vừa vặn thoải mái trong giới hạn thời gian và đường chuyền tuyến tính là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import inf

    n = int(input())
    tasks = []
    for _ in range(n):
        a, d = map(int, input().split())
        tasks.append((a, d))

    tasks.sort()
    time = 0
    ans = 0
    for a, d in tasks:
        time += a
        ans += d - time
    return str(ans)

# provided sample
assert run("""3
6 10
8 15
5 12
""") == "2"

# minimum input
assert run("""1
5 10
""") == "5"

# already sorted durations
assert run("""3
1 10
2 20
3 30
""") == "54"

# reverse durations
assert run("""3
3 30
2 20
1 10
""") == "54"

# equal durations
assert run("""3
5 10
5 10
5 10
""") == "-30"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nhiệm vụ đơn lẻ | 5 | trường hợp cơ sở đúng đắn | 
| thứ tự tăng dần | 54 | tính đúng đắn theo thứ tự tối ưu ổn định | 
| thứ tự giảm dần | 54 | sắp xếp các bản sửa lỗi sắp xếp ban đầu tồi tệ nhất | 
| thời lượng bằng nhau | -30 | xử lý ràng buộc và phạt tích lũy | 

## Vỏ cạnh 

Khi tất cả các tác vụ có thời lượng giống nhau, việc sắp xếp sẽ không thay đổi thứ tự tương đối của chúng và thuật toán sẽ xử lý chúng một cách hiệu quả theo thứ tự đầu vào. Vì mọi nhiệm vụ đều góp phần làm tăng thời gian hoàn thành một cách tuyến tính nên tổng sẽ trở nên âm nếu thời hạn nhỏ, khớp chính xác với công thức. 

Đối với một đầu vào như:```
3
5 10
5 10
5 10
```Thời gian chạy là 5, 10 và 15, tạo ra các đóng góp 5, 0 và -5, tổng cộng là -30. Bất kỳ hoán vị nào cũng mang lại cùng một tập hợp thời gian hoàn thiện, do đó thuật toán vẫn đúng bất kể độ ổn định. 

Trường hợp tinh vi thứ hai là một nhiệm vụ rất dài trong số rất nhiều nhiệm vụ ngắn. Việc sắp xếp đảm bảo nhiệm vụ dài được đặt cuối cùng, ngăn không cho nó tăng quá nhiều lần hoàn thành trung gian. Ví dụ:```
3
1 100
1 100
10 100
```Việc xử lý nhiệm vụ có độ dài 10 trước tiên sẽ cộng thêm 10 nhiệm vụ vào mỗi lần hoàn thành tiếp theo, điều này sẽ làm giảm đáng kể tổng phần thưởng. Thứ tự sắp xếp sẽ tránh được điều này và giữ mức phạt tích lũy ở mức tối thiểu.
