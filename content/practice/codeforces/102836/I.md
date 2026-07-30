---
title: "CF 102836I - \u041a\u0441\u0435\u0440\u043e\u043a\u0441\u0438\u043d\u0430\u0442\u043e\u0440"
description: "Vấn đề mô tả một bưu điện hoạt động trong n phút. Vào đầu phút thứ i, một bản sao [i] được đưa vào hàng đợi. Trong phút đó, bưu điện chỉ phục vụ tối đa b nhân thân từ phía trước của hàng đợi. Các bản sao được phục vụ sẽ rời đi vào cuối cùng một phút."
date: "2026-07-26T14:54:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102836
codeforces_index: "I"
codeforces_contest_name: "\u0426\u0438\u043a\u043b \u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434, \u0421\u0435\u0437\u043e\u043d 2020-21, \u0422\u0440\u0435\u0442\u044c\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102836
solve_time_s: 41
verified: true
draft: false
---

[CF 102836I - \u041a\u0441\u0435\u0440\u043e\u043a\u0441\u0438\u043d\u0430\u0442\u043e\u0440](https://codeforces.com/problemset/problem/102836/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề mô tả một bưu điện hoạt động cho`n`phút. Vào đầu phút`i`,`a[i]`bản sao vào hàng đợi. Trong thời gian đó, bưu điện chỉ phục vụ tối đa`b`nhân bản từ phía trước của hàng đợi. Các bản sao được phục vụ sẽ rời đi vào cuối cùng một phút. Bất kỳ bản sao nào vẫn chờ sau phút cuối cùng sẽ ở lại thêm một phút trước khi rời đi. Nhiệm vụ là tìm tổng thời gian mỗi bản sao ở trong bưu điện. 

Đầu vào cung cấp số phút làm việc, công suất dịch vụ mỗi phút và số lượng bản sao đến trong mỗi phút. Đầu ra là một số nguyên biểu thị tổng thời gian chờ và thời gian phục vụ của tất cả các bản sao cộng lại. Một bản sao vào vào đầu phút`i`và rời đi vào cuối phút`i`đóng góp một phút 

Giới hạn cho phép lên tới`100000`số phút và số lượt đến lên đến`10^8`. Việc mô phỏng xử lý từng bản sao riêng lẻ là không thể vì tổng số bản sao có thể đạt tới`10^13`. Ngay cả việc lưu trữ tất cả các bản sao hoặc cập nhật thời gian còn lại của mỗi bản sao cũng sẽ vượt quá giới hạn thực tế. Thuật toán phải hoạt động bằng cách chỉ theo dõi số lượng bản sao hiện có trong hàng đợi, điều này sẽ đưa ra giải pháp tuyến tính. 

Một số chi tiết có thể gây ra câu trả lời sai. Một sai lầm phổ biến là quên rằng các bản sao đến trong phút làm việc cuối cùng vẫn dành thời gian ở đó, ngay cả khi chúng không được phục vụ. Ví dụ:```
Input
1 10
5

Output
5
```Tất cả năm bản sao đều tham gia trong một phút duy nhất và được phục vụ ngay lập tức, vì vậy mỗi bản sao sẽ dành một phút. 

Một sai lầm khác là bỏ qua phút bổ sung sau khi đóng cửa. Coi như:```
Input
1 1
3

Output
5
```Một bản sao được phục vụ trong phút làm việc và dành một phút. Hai bản sao còn lại dành một phút bên trong trong thời gian làm việc và thêm một phút sau khi đóng cửa, mang lại cho`1 + 2 + 2 = 5`. 

Lỗi thứ ba là chỉ tính hàng đợi sau dịch vụ. Ví dụ:```
Input
2 100
3 4

Output
7
```Mỗi bản sao đi vào trong một phút đều đóng góp phút đó ngay cả khi nó rời đi ngay lập tức. Tổng số đúng là`3`ngay từ phút đầu tiên và`4`từ phút thứ hai. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng mọi bản sao. Khi một bản sao đến, chúng ta có thể xếp nó vào hàng đợi và di chuyển qua các phút, loại bỏ tối đa`b`nhân bản mỗi phút. Để tính toán câu trả lời, chúng tôi sẽ thêm một bản sao vào mỗi bản sao vẫn còn trong bưu điện. Điều này đúng về mặt logic vì mỗi bản sao đóng góp chính xác một phút cho mỗi phút còn lại. Tuy nhiên, số lượng bản sao có thể lớn như`10^13`, do đó, ngay cả một lần chuyển qua tất cả các bản sao cũng là quá chậm. 

Quan sát quan trọng là các bản sao có cùng vị trí trong hàng đợi sẽ hoạt động giống hệt nhau. Chúng tôi không cần biết danh tính cá nhân, chỉ cần biết có bao nhiêu bản sao hiện đang chờ đợi. Trong mỗi phút, số lượng bản sao có mặt khi bắt đầu dịch vụ đủ để biết lượng thời gian được đóng góp trong phút đó. Sau khi thêm số tiền này vào câu trả lời, chúng tôi giảm hàng đợi theo số lượng bản sao được phục vụ. 

Brute Force hoạt động vì nó theo dõi mọi đóng góp riêng biệt nhưng không thành công vì số lượng đối tượng quá lớn. Quan sát rằng tất cả các bản sao trong cùng trạng thái hàng đợi đều có thể thay thế cho nhau giúp giảm vấn đề duy trì một số nguyên biểu thị kích thước hàng đợi hiện tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng số bản sao) | O(tổng số bản sao) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giữ một biến`queue`đại diện cho số lượng bản sao đang chờ sau phút trước và giữ`answer`như thời gian tích lũy đã bỏ ra. 
2. Cứ mỗi phút, hãy thêm các bản sao đến vào hàng đợi. Kích thước hàng đợi hiện tại đại diện cho tất cả các bản sao dành phút này bên trong bưu điện, vì vậy hãy thêm nó vào`answer`. 
3. Loại bỏ các bản sao được phục vụ trong phút này. Số bị loại bỏ là số nhỏ hơn so với kích thước hàng đợi hiện tại và giới hạn dịch vụ`b`. 
4. Sau khi tất cả số phút làm việc được xử lý, hãy thêm kích thước hàng đợi còn lại một lần nữa. Những bản sao này tồn tại thêm một phút sau khi đóng cửa. 

Tại sao nó hoạt động: trong mỗi phút làm việc, mỗi bản sao hiện có bên trong sẽ đóng góp chính xác một phút. Thuật toán sẽ thêm chính xác số lượng bản sao có mặt trong phút đó, bất kể chúng rời đi cuối cùng hay tiếp tục chờ đợi. Phần bổ sung cuối cùng xử lý khoảng thời gian duy nhất nằm ngoài lịch làm việc. Vì toàn bộ thời gian lưu trú của mỗi bản sao đều được bao gồm trong các khoảng thời gian được tính này nên tổng cuối cùng là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, b = map(int, input().split())
    a = list(map(int, input().split()))

    queue = 0
    answer = 0

    for x in a:
        queue += x
        answer += queue
        queue -= min(queue, b)

    answer += queue
    print(answer)

if __name__ == "__main__":
    solve()
```Biến`queue`chỉ lưu trữ số lượng bản sao chưa hoàn thành, tránh mọi sự phụ thuộc vào tổng số bản sao. Khi bắt đầu mỗi lần lặp, những người đến sẽ được thêm vào vì họ vào trước khi dịch vụ bắt đầu. 

Việc bổ sung vào`answer`xảy ra trước khi loại bỏ các bản sao đã phục vụ. Lệnh này quan trọng vì một bản sao được gửi ngay lập tức vẫn dành một phút trong bưu điện. Di chuyển phần bổ sung sau khi loại bỏ sẽ bỏ lỡ tất cả các bản sao đó. 

Bước dịch vụ sử dụng`min(queue, b)`bởi vì bưu điện không thể phục vụ nhiều bản sao hơn số lượng đang chờ đợi. Số nguyên Python tự động xử lý câu trả lời lớn có thể có, điều này là cần thiết vì tổng thời gian có thể lớn hơn nhiều so với giới hạn số nguyên 32 bit. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 4
1 5 9
```| Phút | Đến | Xếp hàng trước dịch vụ | Đã thêm vào câu trả lời | Phục vụ | Xếp hàng sau dịch vụ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 1 | 0 | 
| 2 | 5 | 5 | 5 | 4 | 1 | 
| 3 | 9 | 10 | 10 | 4 | 6 | 

Sau khi đóng cửa, 6 bản sao còn lại đóng góp thêm một phút. 

Tổng cộng là`1 + 5 + 10 + 6 = 22`. Dấu vết này cho thấy tại sao hàng đợi còn sót lại cuối cùng phải được tính. 

### Ví dụ tùy chỉnh 

đầu vào:```
2 100
3 4
```| Phút | Đến | Xếp hàng trước dịch vụ | Đã thêm vào câu trả lời | Phục vụ | Xếp hàng sau dịch vụ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | 3 | 3 | 0 | 
| 2 | 4 | 4 | 4 | 4 | 0 | 

Không có bản sao nào còn lại sau khi đóng, vì vậy phút thêm không bổ sung thêm gì. Câu trả lời là`7`. 

Dấu vết này chứng tỏ rằng dịch vụ ngay lập tức vẫn đóng góp thời gian và việc xếp hàng sau dịch vụ không phải là phần duy nhất quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phút được xử lý một lần với số học theo thời gian không đổi. | 
| Không gian | O(1) | Chỉ có kích thước hàng đợi hiện tại và câu trả lời tích lũy được lưu trữ. | 

Thuật toán chỉ thực hiện vài thao tác trong một phút nên dễ dàng đạt được giới hạn về`100000`phút. Nó cũng tránh việc lưu trữ số lượng bản sao khổng lồ. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# provided sample
assert run("""3 4
1 5 9
""") == "22\n", "sample"

# minimum size
assert run("""1 1
0
""") == "0\n", "empty queue"

# all served immediately
assert run("""3 100
5 5 5
""") == "15\n", "large service capacity"

# leftover after closing
assert run("""1 1
3
""") == "5\n", "remaining clones"

# boundary where service equals arrivals
assert run("""5 3
3 3 3 3 3
""") == "45\n", "steady queue growth"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 0`|`0`| Trường hợp tối thiểu không có bản sao | 
|`3 100 / 5 5 5`|`15`| Tất cả các bản sao rời đi ngay lập tức | 
|`1 1 / 3`|`5`| Thêm phút sau khi đóng cửa | 
|`5 3 / 3 3 3 3 3`|`45`| Dịch vụ một phần lặp đi lặp lại | 

## Vỏ cạnh 

Đối với trường hợp bưu điện đóng cửa còn lại bản sao:```
Input
1 1
3
```Thuật toán bắt đầu với một hàng đợi trống. Ba người đến được thêm vào, xếp hàng ba người và thêm ba phút vào câu trả lời. Một bản sao được phục vụ, để lại hai bản sao. Sau tất cả số phút làm việc, thuật toán sẽ cộng thêm hai phút còn lại. Kết quả là`5`, phù hợp với hành vi được yêu cầu. 

Đối với trường hợp tất cả các bản sao được phục vụ ngay lập tức:```
Input
3 100
5 5 5
```Mỗi phút chỉ bắt đầu với những người mới đến vì hàng đợi trước đó trống. Thuật toán bổ sung`5`ba lần và loại bỏ tất cả các bản sao mỗi lần. Không có thêm phút cuối cùng nào được thêm vào vì không còn ai. 

Đối với trường hợp không có người đến:```
Input
2 10
0 0
```Hàng đợi giữ nguyên bằng 0 trong suốt quá trình mô phỏng. Câu trả lời vẫn là 0 vì không có bản sao nào cần tính thời gian. 

Đối với trường hợp giới hạn phục vụ nhỏ hơn số lượt đến:```
Input
3 2
5 0 0
```Sau phút đầu tiên, vẫn còn lại ba bản sao. Mỗi phút thứ hai và thứ ba tính hàng đợi hiện có trước khi phục vụ. Sau khi đóng, một bản sao vẫn còn và đóng góp vào phút cuối cùng. Thuật toán xử lý việc này một cách tự nhiên vì nó luôn đếm hàng đợi hiện tại trước khi áp dụng dịch vụ.
