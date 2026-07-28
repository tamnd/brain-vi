---
title: "CF 102791D - Thùng"
description: "Chúng tôi có một hàng thùng và mỗi thùng bắt đầu bằng một lượng nước. Một nước đi bao gồm việc chọn một thùng không rỗng và đổ bất kỳ lượng nước nào của thùng đó vào một thùng khác. Chúng ta có thể thực hiện tối đa k nước đi."
date: "2026-07-27T18:09:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102791
codeforces_index: "D"
codeforces_contest_name: "ICPC 2020-2021 NERC (NEERC), Southern and Volga Russia Qualifier"
rating: 0
weight: 102791
solve_time_s: 62
verified: true
draft: false
---

[CF 102791D - Thùng](https://codeforces.com/problemset/problem/102791/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một hàng thùng và mỗi thùng bắt đầu bằng một lượng nước. Một nước đi bao gồm việc chọn một thùng không rỗng và đổ bất kỳ lượng nước nào của thùng đó vào một thùng khác. Chúng tôi có thể biểu diễn nhiều nhất`k`di chuyển. Mục đích là tạo ra sự khác biệt giữa thùng đầy nhất và thùng trống nhất càng lớn càng tốt sau mỗi lần di chuyển. 

Thao tác cực kỳ linh hoạt vì chúng ta có thể di chuyển toàn bộ nội dung của thùng chỉ trong một bước. Điều quan trọng duy nhất là chúng ta có thể thu được bao nhiêu nước vào một thùng và liệu chúng ta có thể tạo ra một thùng rỗng cùng lúc hay không. 

Các ràng buộc cho phép lên đến`2 * 10^5`thùng. Với giới hạn thời gian của cuộc thi điển hình, một cách tiếp cận thử nhiều chuỗi nước đi có thể xảy ra là không thể vì số lượng trạng thái tăng quá nhanh. Ngay cả việc kiểm tra nhiều tổ hợp thùng cũng sẽ vượt quá thời gian sẵn có. Chúng ta cần một giải pháp gần`O(n log n)`hoặc tốt hơn. 

Những trường hợp khó khăn đến từ sự tương tác giữa thùng rỗng và số lần di chuyển được phép. 

Hãy xem xét đầu vào này:```
3 1
0 5 0
```Câu trả lời là`5`. Một giải pháp bất cẩn có thể cho rằng một hành động là vô ích vì hai thùng đã rỗng, nhưng chúng ta có thể đổ đầy`5`lít vào thùng rỗng và vẫn để lại thùng rỗng phía sau, tạo ra sự khác biệt về`5`. 

Một trường hợp cạnh khác là:```
4 2
7 0 0 0
```Câu trả lời là`7`. Chúng ta có thể di chuyển nước giữa các thùng hai lần và kết thúc bằng một thùng chứa tất cả`7`lít và ít nhất một thùng rỗng. Một giải pháp chỉ xem xét việc chuyển trực tiếp từ thùng đầy ban đầu sang thùng cuối cùng có thể bỏ sót trường hợp này. 

Trường hợp quan trọng cuối cùng là khi mọi thùng đều trống:```
3 2
0 0 0
```Câu trả lời là`0`. Công thức tính lượng nước thu được tối đa vẫn cho`0`, và không hoạt động nào có thể tạo ra nước không có ở đó. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ cố gắng quyết định thùng nào trở thành nhà tài trợ và thùng nào nhận nước. Đối với mỗi thùng tiếp nhận đã chọn, chúng tôi có thể mô phỏng việc di chuyển nước từ các thùng đã chọn vào đó. Điều này đúng vì trạng thái cuối cùng tốt nhất phải có thùng nào đó chứa lượng nước tối đa, và những nước đi làm tăng giá trị này chính là những nước chuyển nước vào thùng đó. 

Tuy nhiên, việc lựa chọn nhóm nhà tài trợ tốt nhất mới là vấn đề. có`n`thùng và chúng ta cần chọn`k`nhà tài trợ cộng với một thùng nhận. Việc thử tất cả các lựa chọn đòi hỏi phải có sự kết hợp, vượt xa những gì có thể thực hiện được.`n = 2 * 10^5`. 

Quan sát quan trọng là giá trị tối thiểu sau các phép toán luôn có thể bằng 0. Từ`k < n`, chúng ta có ít nhất một thùng không cần phải là người nhận cuối cùng. Bằng cách làm trống thùng thông qua các bước di chuyển của mình, chúng ta có thể để lại một thùng trống phía sau. 

Khi mức tối thiểu được cố định ở mức 0, việc tối đa hóa câu trả lời cũng giống như tối đa hóa thùng lớn nhất. Một động tác có thể chuyển toàn bộ nước từ thùng này sang thùng khác, vì vậy sau`k`di chuyển chúng ta có thể kết hợp nội dung của chính xác`k`thùng tài trợ vào một thùng đích. Thùng đích cũng đóng góp nước ban đầu của nó. Do đó, thùng lớn nhất cuối cùng có thể chứa tổng số lượng lớn nhất`k + 1`số tiền ban đầu. 

Phương pháp brute-force hoạt động vì nó mô hình chính xác tất cả các lần chuyển tiền có thể, nhưng không thành công vì tìm kiếm quá nhiều lựa chọn. Nhận xét rằng chỉ những thùng tích lũy lớn nhất mới có thể làm giảm vấn đề phân loại và tính tổng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong`n`| O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(1) thêm không gian | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lượng nước trong tất cả các thùng và sắp xếp theo thứ tự giảm dần. 

Việc sắp xếp cho phép chúng tôi xác định các thùng đóng góp nhiều nước nhất đến mức tối đa cuối cùng. Bất kỳ thùng nhỏ hơn nào được chọn thay vì thùng lớn hơn chỉ có thể làm giảm kết quả. 
2. Thêm cái đầu tiên`k + 1`các giá trị từ mảng đã được sắp xếp. 

Một thùng sẽ chứa số lượng tối đa cuối cùng. Cái khác`k`thùng chính xác là thùng có nước được chuyển vào đó. Thùng đích đã chứa nước ban đầu, vì vậy chúng ta cần`k + 1`tổng cộng là thùng. 
3. Xuất số tiền này. 

Số tiền tối thiểu có thể bằng 0, vì vậy số tiền tối đa này trực tiếp là chênh lệch bắt buộc. 

Tại sao nó hoạt động: 

Sự khác biệt cuối cùng là`maximum - minimum`. Bởi vì ít nhất một thùng có thể vẫn trống sau nhiều nhất`k`di chuyển, mức tối thiểu luôn có thể bằng không. Nhiệm vụ duy nhất còn lại là tối đa hóa một thùng. Mỗi thao tác rót có thể chuyển hoàn toàn lượng chứa trong một thùng, vì vậy`k`hoạt động cho phép chính xác`k`các thùng khác để đóng góp nước của họ đến một địa điểm đã chọn. Điểm đến và nhà tài trợ tốt nhất có thể là`k + 1`thùng có số lượng ban đầu lớn nhất. Không có lựa chọn nào khác có thể tạo ra tổng số lớn hơn vì việc thay thế bất kỳ thùng đã chọn nào bằng thùng nhỏ hơn không bao giờ làm tăng tổng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    a.sort(reverse=True)
    
    print(sum(a[:k + 1]))

if __name__ == "__main__":
    solve()
```Chương trình trước tiên sẽ sắp xếp số lượng thùng từ lớn nhất đến nhỏ nhất. đầu tiên`k + 1`vị trí là những giá trị duy nhất có thể đóng góp vào thùng đầy nhất cuối cùng, do đó không cần xử lý các thùng còn lại. 

lát cắt`a[:k + 1]`là an toàn bởi vì`k < n`, có nghĩa là ít nhất`k + 1`thùng luôn tồn tại. Số nguyên Python tự động xử lý kích thước tổng có thể có, do đó không cần xử lý tràn đặc biệt. 

Việc triển khai chỉ sử dụng bộ nhớ mảng cộng với bộ nhớ sắp xếp được sử dụng nội bộ. Bước sắp xếp chiếm ưu thế trong thời gian chạy. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
4 1
5 5 5 5
```Mảng được sắp xếp là`[5, 5, 5, 5]`. 

| Bước | Giá trị được sắp xếp | k + 1 giá trị được chọn | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| Ban đầu | 5, 5, 5, 5 | 5, 5 | 10 | 

Thùng đầu tiên nhận nước giữ nguyên nguyên bản`5`và một nước đi sẽ chuyển một nước đi khác`5`vào đó. Vẫn còn một thùng rỗng, nên sự khác biệt là`10`. 

Đối với đầu vào:```
5 2
1 8 3 10 5
```Mảng được sắp xếp là`[10, 8, 5, 3, 1]`. 

| Bước | Giá trị được sắp xếp | k + 1 giá trị được chọn | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| Ban đầu | 10, 8, 5, 3, 1 | 10, 8, 5 | 23 | 

Hai động tác cho phép chúng ta chuyển`8`Và`5`vào thùng chứa`10`. Mức tối đa cuối cùng là`23`và một cái thùng chưa chạm tới có thể trống rỗng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp số lượng thùng chiếm ưu thế trong công việc | 
| Không gian | O(1) thêm không gian | Ngoài mảng đầu vào và chi phí sắp xếp | 

Với`n`lên đến`2 * 10^5`, sắp xếp dễ dàng phù hợp trong giới hạn. Giải pháp thực hiện một lần sắp xếp duy nhất và một lần truyền tuyến tính duy nhất thông qua các giá trị đã chọn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    data = list(map(int, inp.split()))
    n, k = data[0], data[1]
    a = data[2:]
    a.sort(reverse=True)
    return str(sum(a[:k + 1])) + "\n"

assert solve("""4 1
5 5 5 5
""") == "10\n", "sample 1"

assert solve("""3 2
0 0 0
""") == "0\n", "sample 2"

assert solve("""2 1
1000000000 999999999
""") == "1999999999\n", "large values"

assert solve("""5 1
0 0 0 0 7
""") == "7\n", "single non-zero barrel"

assert solve("""6 3
4 1 9 2 8 7
""") == "28\n", "choose top k+1 values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4 1 / 5 5 5 5`|`10`| Giá trị bằng nhau và kết hợp cơ bản | 
|`3 2 / 0 0 0`|`0`| Tất cả thùng trống | 
|`2 1 / 1000000000 999999999`|`1999999999`| Xử lý số tiền lớn và số nguyên | 
|`5 1 / 0 0 0 0 7`|`7`| Thùng rỗng đã có sẵn | 
|`6 3 / 4 1 9 2 8 7`|`28`| Lựa chọn chính xác lớn nhất`k + 1`thùng | 

## Vỏ cạnh 

Dành cho:```
3 1
0 5 0
```Mảng được sắp xếp là`[5, 0, 0]`. Chúng tôi lấy lớn nhất`k + 1 = 2`giá trị, trao`5 + 0 = 5`. Một nước đi có thể đổ nước vào thùng khác trong khi để lại thùng trống, vì vậy sự khác biệt là`5`. 

Vì:```
4 2
7 0 0 0
```Mảng được sắp xếp là`[7, 0, 0, 0]`. Chúng tôi lấy ba giá trị lớn nhất, đưa ra`7`. Mặc dù hai trong số các thùng đóng góp không chứa nước nhưng mức tối đa cuối cùng không thể vượt quá tổng lượng nước trong hệ thống. 

Vì:```
3 2
0 0 0
```Các giá trị được chọn đều bằng 0, vì vậy câu trả lời là 0. Không có nước để di chuyển và mọi trạng thái cuối cùng có thể có đều có lượng tối đa và tối thiểu như nhau. 

Vì:```
5 2
1 8 3 10 5
```Thuật toán chọn`10`,`8`, Và`5`. Đây là những giá trị duy nhất quan trọng vì hai bước di chuyển được phép có thể chuyển nội dung của`8`Và`5`thùng vào thùng chứa`10`. Bất kỳ sự lựa chọn nào liên quan đến`3`hoặc`1`sẽ giảm mức tối đa cuối cùng. 

Tôi cũng có thể điều chỉnh điều này thành định dạng biên tập theo phong cách Codeforces ngắn hơn hoặc giải thích theo phong cách hướng dẫn hơn nếu cần.
