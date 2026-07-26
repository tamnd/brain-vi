---
title: "CF 102864J - \u5c60\u9f99\u52c7\u8005ErvinXie"
description: "Dòng sông là một hàng dài các vị trí và mỗi vị trí đều chứa chính xác một loại vật liệu giả kim. ErvinXie có thể chọn nơi bắt đầu và sau đó tiến về phía trước, thu thập mọi tài liệu anh ấy đi qua."
date: "2026-07-25T13:48:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102864
codeforces_index: "J"
codeforces_contest_name: "The 15-th BIT Campus Programming Contest - Online Round"
rating: 0
weight: 102864
solve_time_s: 45
verified: true
draft: false
---

[CF 102864J - \u5c60\u9f99\u52c7\u8005ErvinXie](https://codeforces.com/problemset/problem/102864/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Dòng sông là một hàng dài các vị trí và mỗi vị trí đều chứa chính xác một loại vật liệu giả kim. ErvinXie có thể chọn nơi bắt đầu và sau đó tiến về phía trước, thu thập mọi tài liệu anh ấy đi qua. Các tài liệu được thu thập không cần phải được sắp xếp theo thứ tự như khi chúng được thu thập. Yêu cầu duy nhất là bộ sưu tập cuối cùng phải chứa đủ bản sao của mọi loại vật liệu theo yêu cầu của pháp trận. 

Sự hình thành được mô tả bởi một mảng`a`. Một tài liệu xuất hiện nhiều lần trong`a`cũng phải được thu thập nhiều lần. Con sông được mô tả bằng một mảng khác`b`, trong đó mỗi phần tử là vật liệu được tìm thấy ở vị trí đó. Nhiệm vụ là tìm phần liên tục ngắn nhất của dòng sông có số lượng vật chất bao gồm tất cả số lượng cần thiết từ hệ tầng. Câu trả lời là độ dài của phần đó. Nếu không có phần đó tồn tại, câu trả lời là`DragonXie`. 

Kích thước đầu vào buộc một giải pháp gần tuyến tính. Chiều dài hình thành có thể đạt tới một triệu và chiều dài sông có thể đạt tới hai triệu. Một giải pháp kiểm tra mọi vị trí bắt đầu và mở rộng một phân khúc sẽ cần khoảng`s * len`các hoạt động trong trường hợp xấu nhất là khoảng hai nghìn tỷ lần kiểm tra và không thể hoàn thành. Ngay cả những phương pháp đếm vật liệu liên tục trong phạm vi cũng quá chậm. Chúng ta chỉ cần xử lý mỗi vị trí sông một số lần không đổi. 

Số lượng các loại vật liệu khác nhau chỉ`5000`, có nghĩa là chúng ta có thể lưu trữ thông tin tần số trong mảng thay vì sử dụng các cấu trúc chậm hơn. Kích thước bảng chữ cái nhỏ sẽ giúp ích, nhưng độ dài chuỗi rất lớn có nghĩa là chúng ta vẫn cần một`O(len)`thuật toán. 

Một số trường hợp đặc biệt có thể phá vỡ việc triển khai đơn giản hơn. Nếu đội hình yêu cầu vật liệu lặp đi lặp lại, việc chỉ kiểm tra xem mọi loại có xuất hiện hay không là không chính xác. Ví dụ:```
1
3
1 1 2
4
1 2 1 2
```Đầu ra đúng là`3`bởi vì phân khúc`[1, 2, 1]`chứa hai bản sao của tài liệu`1`và một bản sao của tài liệu`2`. Một giải pháp bất cẩn chỉ ghi lại liệu một loại có xuất hiện hay không sẽ chấp nhận một phân đoạn không hợp lệ ngắn hơn. 

Một trường hợp khác là khi cả sông không đủ:```
2
2
1 2
1
1
```Đầu ra đúng là`DragonXie`. Một giải pháp dừng lại sau khi tìm thấy một tài liệu cần thiết mà không kiểm tra tất cả các yêu cầu sẽ tạo ra câu trả lời sai. 

Đoạn ngắn nhất cũng có thể là toàn bộ dòng sông:```
1
4
1 1 1 1
4
1 1 1 1
```Câu trả lời là`4`. Việc triển khai chỉ cập nhật câu trả lời khi việc thu hẹp xảy ra có thể bỏ lỡ trường hợp này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi vị trí xuất phát có thể có trên sông. Đối với mỗi lần bắt đầu, chúng tôi mở rộng phân khúc sang bên phải, đếm số lượng vật liệu đã thu thập và dừng lại khi đã đáp ứng đủ số lượng yêu cầu. Điều này đúng vì mọi điểm bắt đầu của đoạn đường có thể đều được xem xét và điểm cuối hợp lệ đầu tiên cho mỗi lần bắt đầu sẽ cho ra đoạn đường ngắn nhất bắt đầu từ đó. 

Vấn đề là nhiều phân khúc chia sẻ thông tin gần như giống nhau. Trong trường hợp xấu nhất, sự hình thành có thể cần một loại vật liệu không bao giờ xuất hiện ở gần đầu một con sông lớn. Quét vũ lực có thể kiểm tra hầu hết mọi cặp ranh giới trái và phải có thể có. Với`len = 2 * 10^6`, số lượng hoạt động có thể đạt tới`4 * 10^12`, vượt xa giới hạn. 

Quan sát chính là điều kiện bắt buộc chỉ phụ thuộc vào số lượng bên trong phân đoạn hiện tại. Khi một phân đoạn đã có đủ mọi loại vật liệu, việc di chuyển ranh giới bên trái của nó sang bên phải chỉ có thể làm cho nó ngắn hơn. Khi một phân khúc bị thiếu thứ gì đó, việc mở rộng ranh giới bên phải là cách duy nhất để có được nhiều nguyên liệu hơn. Hành vi đơn điệu này cho phép một cửa sổ trượt. 

Cửa sổ giữ hai mảng tần số: một mảng cho sự hình thành cần thiết và một mảng cho đoạn sông hiện tại. Thay vì so sánh tất cả các loại vật liệu sau mỗi thay đổi, chúng tôi duy trì một giá trị duy nhất biểu thị số lần xuất hiện vật liệu bắt buộc vẫn còn thiếu. Việc thêm vật liệu chỉ làm giảm giá trị này khi sự xuất hiện đó trước đó bị thiếu. Việc loại bỏ một vật liệu chỉ làm tăng nó khi việc loại bỏ đó làm cho đoạn đó không còn đủ nữa. 

Lực lượng vũ phu hoạt động vì nó kiểm tra tất cả các phân đoạn một cách độc lập, nhưng nó lặp lại công việc đếm gần như giống hệt nhau. Cửa sổ trượt giữ nguyên độ chính xác trong khi sử dụng lại thông tin từ các phân đoạn lân cận, giảm vấn đề xuống còn một lần vượt sông. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(len²) | O(k) | Quá chậm | 
| Tối ưu | O(len) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số lần mỗi vật liệu xuất hiện trong đội hình. Cho phép`missing`ban đầu bằng chiều dài hình thành vì chưa có vật liệu cần thiết nào được thu thập. 
2. Di chuyển con trỏ phải từ đầu sông đến cuối. Thêm từng vật liệu mới vào số lượng của cửa sổ hiện tại. Nếu vẫn cần vật liệu này trước khi thêm vào, hãy giảm`missing`. 

Giá trị của`missing`thể hiện chính xác số lượng bản sao tài liệu cần thiết mà cửa sổ hiện tại vẫn còn thiếu. 
3. Bất cứ khi nào`missing`trở thành 0, cửa sổ hiện tại có thể xây dựng đội hình. Di chuyển con trỏ trái về phía trước trong khi cửa sổ vẫn hợp lệ, cập nhật câu trả lời với mỗi độ dài hợp lệ ngắn hơn được tìm thấy. 

Loại bỏ những vật liệu không cần thiết luôn an toàn vì mục tiêu là đoạn hợp lệ ngắn nhất. Thời điểm loại bỏ thêm một vật liệu sẽ phá vỡ tính hợp lệ, ranh giới bên trái hiện tại là nhỏ nhất có thể đối với ranh giới bên phải này. 
4. Sau khi xử lý toàn bộ dòng sông, xuất ra chiều dài nhỏ nhất được ghi lại. Nếu không tìm thấy cửa sổ hợp lệ nào, hãy xuất`DragonXie`. 

Tại sao nó hoạt động: 

Bất biến được duy trì bởi cửa sổ trượt là`missing`luôn bằng số lượng bản sao tài liệu cần thiết không có trong cửa sổ hiện tại. Con trỏ bên phải chỉ di chuyển về phía trước và mỗi khi cửa sổ trở nên hợp lệ, con trỏ bên trái sẽ xóa tất cả các vật liệu có thể tháo rời mà không làm mất hiệu lực. Mọi đoạn ngắn nhất có thể đều có một thời điểm đạt đến ranh giới bên phải của nó và quá trình thu nhỏ sẽ tìm thấy ranh giới bên trái tối thiểu của nó. Vì mọi cửa sổ ngắn nhất của ứng cử viên đều được kiểm tra nên mức tối thiểu cuối cùng là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    k_line = input().strip()
    if not k_line:
        return
    k = int(k_line)

    s = int(input())
    need = [0] * (k + 1)

    arr = list(map(int, input().split()))
    for x in arr:
        need[x] += 1

    length = int(input())
    river = list(map(int, input().split()))

    have = [0] * (k + 1)
    missing = s
    left = 0
    ans = length + 1

    for right, x in enumerate(river):
        have[x] += 1
        if have[x] <= need[x]:
            missing -= 1

        while missing == 0:
            cur = right - left + 1
            if cur < ans:
                ans = cur

            y = river[left]
            if have[y] <= need[y]:
                missing += 1
            have[y] -= 1
            left += 1

    if ans == length + 1:
        print("DragonXie")
    else:
        print(ans)

if __name__ == "__main__":
    solve()
```Chương trình đầu tiên xây dựng bảng tần số của hệ tầng. Kích thước mảng chỉ phụ thuộc vào số lượng loại vật liệu, vì vậy nó vẫn nhỏ ngay cả khi các chuỗi lớn. 

Biến`missing`được khởi tạo thành`s`, tổng số bản sao tài liệu cần thiết. Khi một tài liệu đi vào cửa sổ, điều đó chỉ hữu ích nếu cửa sổ trước đó có ít bản sao hơn mức cần thiết. Các bản sao bổ sung không đáp ứng được điều gì mới nên không thay đổi`missing`. 

Vòng lặp thu hẹp chỉ chạy khi cửa sổ hợp lệ. Trước khi loại bỏ vật liệu ngoài cùng bên trái, mã sẽ kiểm tra xem vật liệu đó hiện có góp phần đáp ứng yêu cầu hay không. Nếu số lượng vẫn nằm trong số lượng yêu cầu, việc xóa nó sẽ tạo ra một bản sao bị thiếu. Thứ tự kiểm tra và giảm dần rất quan trọng vì nó quyết định liệu bản sao bị xóa có hữu ích hay không. 

Hai con trỏ di chuyển từ trái sang phải nhiều nhất một lần. Không cần quét sông nhiều lần nên việc thực hiện vẫn nằm trong giới hạn yêu cầu. 

## Ví dụ đã hoạt động 

Mẫu 1:```
1
2
1 2 3 2
5
1 2 1 3 2
```Số lượng yêu cầu là một trong số`1`,`2`, Và`3`, với một bản sao khác của`2`. Cửa sổ trượt hoạt động như sau. 

| Đúng vị trí | Đã thêm tài liệu | Vị trí bên trái | Thiếu | Cửa sổ hiện tại | Câu trả lời hay nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 3 | [1] | không | 
| 1 | 2 | 0 | 2 | [1,2] | không | 
| 2 | 1 | 0 | 2 | [1,2,1] | không | 
| 3 | 3 | 0 | 1 | [1,2,1,3] | không | 
| 4 | 2 | 0 | 0 | [1,2,1,3,2] | 5 | 
| 4 | xóa 1 | 1 | 0 | [2,1,3,2] | 4 | 
| 4 | loại bỏ 2 | 2 | 1 | không hợp lệ | 4 | 

Câu trả lời trở thành`4`, phù hợp với bộ sưu tập ngắn nhất được mô tả. Dấu vết cho thấy các bản sao bổ sung có thể được xóa khỏi bên trái mà không làm mất hiệu lực. 

Mẫu 2:```
5
4
1 2 3 4
5
1 2 3 4 5
```| Đúng vị trí | Đã thêm tài liệu | Vị trí bên trái | Thiếu | Cửa sổ hiện tại | Câu trả lời hay nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 3 | [1] | không | 
| 1 | 2 | 0 | 2 | [1,2] | không | 
| 2 | 3 | 0 | 1 | [1,2,3] | không | 
| 3 | 4 | 0 | 0 | [1,2,3,4] | 4 | 
| 3 | xóa 1 | 1 | 1 | không hợp lệ | 4 | 

Toàn bộ bộ yêu cầu đã có sẵn khi vị trí`3`đã đạt được. Loại bỏ bất kỳ vật liệu đầu tiên nào sẽ làm cho cửa sổ không đầy đủ, vì vậy câu trả lời vẫn là`4`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(len) | Mỗi vị trí sông vào cửa sổ một lần và rời khỏi cửa sổ một lần. | 
| Không gian | O(k) | Chỉ các mảng tần số cho loại vật liệu được lưu trữ. | 

Chiều dài con sông lớn nhất là hai triệu, do đó việc quét tuyến tính là cần thiết. Mảng tần số chứa tối đa`5001`các mục nhập, làm cho việc sử dụng bộ nhớ nhỏ so với kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_case(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result.strip()

# provided sample
assert solve_case(
"""3
4
1 2 3 2
5
1 2 1 3 2
"""
) == "4", "sample"

assert solve_case(
"""5
4
1 2 3 4
5
1 2 3 4 5
"""
) == "4", "sample"

# impossible case
assert solve_case(
"""2
2
1 2
1
1
"""
) == "DragonXie", "missing material"

# all equal values
assert solve_case(
"""1
5
1 1 1 1 1
6
1 1 1 1 1 1
"""
) == "5", "all equal"

# minimum size case
assert solve_case(
"""1
1
3
1
3
"""
) == "1", "single element"

# boundary where the answer is at the end
assert solve_case(
"""3
3
1 2 3
5
2 2 1 2 3
"""
) == "3", "late valid window"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| sự hình thành`[1,2,3,2]`, dòng sông`[1,2,1,3,2]`|`4`| Hành vi thu nhỏ mẫu | 
| Đội hình không thể hoàn thành |`DragonXie`| Phát hiện không thể | 
| Tất cả các vật liệu đều giống hệt nhau |`5`| Xử lý yêu cầu trùng lặp | 
| Một vật phẩm bắt buộc trong một ô sông |`1`| Cửa sổ nhỏ nhất có thể | 
| Phân đoạn hợp lệ xuất hiện ở cuối |`3`| Xử lý ranh giới bên phải | 

## Vỏ cạnh 

Đối với các yêu cầu vật liệu lặp đi lặp lại, thuật toán theo dõi số lượng thay vì chỉ tồn tại. Trong đầu vào```
1
3
1 1 2
4
1 2 1 2
```cửa sổ mở rộng cho đến khi nó chứa hai bản sao của`1`và một bản sao của`2`. Khi cửa sổ`[1,2,1]`đã đạt được,`missing`trở thành số 0 và câu trả lời trở thành`3`. Một giải pháp dựa trên loại sẽ suy nghĩ không chính xác`[1,2]`là đủ. 

Đối với một đội hình bất khả thi, con trỏ bên phải cuối cùng sẽ đến cuối sông trong khi`missing`không bao giờ đạt tới số không. TRONG```
2
2
1 2
1
1
```cửa sổ chứa một vật liệu cần thiết nhưng vẫn thiếu vật liệu`2`. Thuật toán không bao giờ ghi lại câu trả lời và xuất ra`DragonXie`. 

Đối với trường hợp toàn bộ dòng sông là câu trả lời,```
1
4
1 1 1 1
4
1 1 1 1
```con trỏ bên phải làm cho cửa sổ chỉ hợp lệ sau khi thu thập vị trí cuối cùng. Giai đoạn thu gọn không thể loại bỏ bất kỳ mục nào vì phải sao chép từng mục nên độ dài được ghi vẫn được giữ nguyên`4`. Điều này tránh được sai lầm phổ biến là chỉ chấp nhận các câu trả lời được tìm thấy sau khi thu hẹp thành công.
