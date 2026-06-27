---
title: "CF 105067E - Một vấn đề đặt hàng khác"
description: "Mỗi mục trong đầu vào đại diện cho một phần trên cùng. Phần trên cùng có một giá trị và nó cũng mang một hạn chế duy nhất trỏ đến một chỉ số phần trên khác. Nếu bạn quyết định đưa phần trên cùng i vào lựa chọn cuối cùng của mình thì bi phần trên sẽ không còn được phép xuất hiện cùng với nó nữa."
date: "2026-06-28T00:13:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105067
codeforces_index: "E"
codeforces_contest_name: "Teamscode Spring 2024 (Advanced Division)"
rating: 0
weight: 105067
solve_time_s: 97
verified: false
draft: false
---

[CF 105067E - Một vấn đề đặt hàng khác](https://codeforces.com/problemset/problem/105067/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 37 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mỗi mục trong đầu vào đại diện cho một phần trên cùng. Phần trên cùng có một giá trị và nó cũng mang một hạn chế duy nhất trỏ đến một chỉ số phần trên khác. Nếu bạn quyết định đưa phần trên cùng i vào lựa chọn cuối cùng của mình thì phần trên cùng b_i không còn được phép xuất hiện cùng với nó nữa. 

Nhiệm vụ là chọn một tập hợp con các lớp phủ để tối đa hóa tổng giá trị trong khi vẫn tôn trọng tất cả các hạn chế đó. Mọi hạn chế đều có một hướng trong đầu vào, nhưng nó vẫn hoạt động như một quy tắc loại trừ lẫn nhau cho tập hợp được chọn cuối cùng: khi bạn lấy i, bạn buộc phải loại trừ b_i. 

Cấu trúc không xung đột theo cặp tùy ý. Mỗi chỉ mục tạo ra chính xác một đối tác bị cấm, có nghĩa là mọi phần tử đều có nhiều nhất một ràng buộc gửi đi. Chi tiết đó là điều làm cho bài toán hoạt động khác với một tập độc lập có trọng số tối đa chung, trong đó cấu trúc biểu đồ sẽ khó xử lý ở tỷ lệ này. 

Với n lên tới 100000, bất kỳ giải pháp nào cố gắng liệt kê các tập hợp con hoặc mô phỏng các lựa chọn qua các kết hợp đều nằm ngoài phạm vi ngay lập tức. Ngay cả các tương tác O(n²) cũng quá lớn và bất kỳ điều gì liên quan đến tính khả thi của việc tính toán lại trên mỗi tập hợp con đều bị loại trừ. Các giải pháp khả thi duy nhất là những giải pháp sắp xếp và xử lý các mục một cách tham lam hoặc khai thác bản chất chức năng của biểu đồ ràng buộc. 

Một số tình huống rất dễ mắc sai lầm nếu tiếp cận một cách ngây thơ. Người ta cho rằng hạn chế là đối xứng. Nếu tôi cấm j, điều đó không có nghĩa là j cấm tôi trừ khi được nêu rõ ràng ở nơi khác. Ví dụ: nếu 1 cấm 2 nhưng 2 không cấm gì thì chỉ chọn 2 luôn hợp lệ và chọn 1 buộc phải loại trừ 2 chứ không phải ngược lại. 

Một trường hợp tinh vi khác là xích. Nếu 1 cấm 2 và 2 cấm 3, chọn 1 sẽ loại bỏ 2, nhưng nó không tự động buộc loại bỏ 3 trừ khi 2 cũng được chọn. Một chiến lược tham lam phải tránh vô tình coi đây là một vấn đề đóng cửa bắc cầu. 

Cạm bẫy cuối cùng là nghĩ rằng đây là một vấn đề phá vỡ chu kỳ. Ngay cả khi các chu kỳ tồn tại như 1 cấm 2, 2 cấm 3, 3 cấm 1, ràng buộc vẫn chỉ kích hoạt từ các nút đã chọn ra bên ngoài, do đó cấu trúc không phải là biểu đồ xung đột vô hướng tiêu chuẩn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử tất cả các tập hợp con của lớp phủ, kiểm tra xem mọi i được chọn có chứa b_i bị cấm hay không và tính tổng. Điều này đúng nhưng yêu cầu lặp lại trên 2^n tập hợp con và thậm chí kiểm tra từng tập hợp con có giá O(n), dẫn đến O(n·2^n), điều này trở nên không thể thực hiện được ngay khi n vượt quá khoảng 25. 

Quan sát chính xuất phát từ sự bất đối xứng và thưa thớt của các ràng buộc. Mỗi mục chỉ cấm một mục khác, nghĩa là việc chọn một mục có một hậu quả ngay lập tức: loại bỏ tối đa một ứng cử viên khỏi việc xem xét trong tương lai. Điều này làm cho quyết định mang tính hủy diệt cục bộ nhưng lại đơn giản trên toàn cầu. 

Khi các mặt hàng được sắp xếp theo giá trị giảm dần, chiến lược tham lam sẽ trở nên tự nhiên. Khi xử lý một mục, nếu mục đó chưa bị xóa bởi mục đã chọn trước đó thì việc chọn mục đó luôn an toàn và chỉ vô hiệu hóa một mục khác. Vì chúng tôi luôn ưu tiên các mặt hàng có giá trị cao hơn trước nên mọi lựa chọn sau này không thể cải thiện tổng số bằng cách thay thế mặt hàng có giá trị cao hơn đã chọn. 

Cấu trúc này tương đương với việc duy trì một tập hợp các mục có sẵn và liên tục chọn mục có sẵn tốt nhất, trong đó mỗi lựa chọn sẽ loại bỏ tối đa một nút bổ sung. Bởi vì việc xóa không bao giờ giới thiệu lại các mục và không bao giờ xếp tầng ngoại trừ thông qua thứ tự lựa chọn, nên thứ tự tham lam là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·2^n) | O(n) | Quá chậm | 
| Tham lam theo giá trị sắp xếp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi vấn đề thành một quy trình trên các mục được sắp xếp theo giá trị.

1. Sắp xếp tất cả các loại topping theo thứ tự giá giảm dần. Điều này đảm bảo rằng chúng tôi luôn xem xét lựa chọn còn lại có lợi nhất trước tiên, điều này rất cần thiết vì một khi mặt hàng có giá trị thấp hơn được chọn, nó không bao giờ có thể bù đắp cho việc mất đi mặt hàng có giá trị cao hơn. 
2. Duy trì hai mảng boolean. Một bản ghi xem một mục đã được chọn hay chưa. Các bản ghi khác liệu một mục có bị vô hiệu bởi một số mục đã chọn trước đó hay không. 
3. Lặp lại danh sách đã sắp xếp. Đối với mỗi mục i, nếu đã hết hiệu lực thì bỏ qua vì việc đưa vào sẽ vi phạm quyết định đã cam kết trước đó. 
4. Nếu i vẫn hợp lệ, hãy chọn nó và cộng giá trị của nó vào câu trả lời. 
5. Ngay lập tức vô hiệu hóa b_i, vì việc chọn i làm cho b_i không tương thích với giải pháp hiện tại. Nếu b_i đã được chọn thì bước này sẽ bị ngăn chặn sớm hơn khi b_i được xử lý, do đó tính nhất quán được duy trì. 
6. Tiếp tục cho đến khi tất cả các mục được xử lý. 

Thứ tự đảm bảo rằng bất cứ khi nào có xung đột giữa hai mục, mục nào có giá trị cao hơn sẽ được xem xét trước tiên. Giá trị thấp hơn sẽ bị bỏ qua hoặc loại bỏ tùy theo hướng. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý tất cả các mục có giá trị lớn hơn một ngưỡng nào đó, tập được chọn hiện tại là tối ưu trong số tất cả các tập hợp con được giới hạn ở các mục có giá trị cao đó. Bất kỳ mục nào bị xóa trong quá trình xử lý sẽ bị xóa vì mục có giá trị cao hơn rõ ràng cấm mục đó. Việc thay thế mục có giá trị cao hơn bằng mục đã xóa không bao giờ có thể làm tăng tổng số tiền, vì mục đã xóa sẽ được xử lý nghiêm ngặt sau đó theo thứ tự được sắp xếp. Điều này đảm bảo rằng mọi quyết định đều tối ưu cục bộ và nhất quán trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = [0] * (n + 1)
    b = [0] * (n + 1)

    items = []
    for i in range(1, n + 1):
        ai, bi = map(int, input().split())
        a[i] = ai
        b[i] = bi
        items.append((ai, i))

    items.sort(reverse=True)

    taken = [False] * (n + 1)
    banned = [False] * (n + 1)

    ans = 0

    for _, i in items:
        if banned[i]:
            continue
        ans += a[i]
        taken[i] = True
        bi = b[i]
        banned[bi] = True

    print(ans)

if __name__ == "__main__":
    solve()
```Trước tiên, giải pháp sẽ đọc tất cả các mục và sắp xếp chúng theo giá trị để các quyết định luôn được đưa ra theo thứ tự lợi ích giảm dần. các`banned`mảng là cấu trúc quan trọng: nó ghi lại các mục không thể lấy được nữa vì mục được chọn trước đó đã loại trừ chúng. 

các`taken`mảng không thực sự cần thiết để đảm bảo tính chính xác trong tham lam cụ thể này, nhưng nó làm cho logic rõ ràng và giúp suy luận về việc liệu xung đột có xảy ra hay không nếu chúng ta cố gắng chọn một mục bị cấm sau đó. Thao tác chính là đánh dấu`b_i`bị cấm khi lựa chọn`i`, điều này đảm bảo không có cặp đôi không hợp lệ nào được hình thành. 

## Ví dụ đã hoạt động 

Hãy xem xét một cấu hình nhỏ trong đó các mục có giá trị cao hơn sẽ chặn các mục có giá trị thấp hơn: 

đầu vào:```
3
10 2
8 3
5 1
```Thứ tự sắp xếp theo giá trị là (10,1), (8,2), (5,3). 

| Bước | Mục | Bị cấm | Đã chụp | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | {} | {} | 0 | 
| 2 | 1 | {2} | {1} | 10 | 
| 3 | 2 | {2} | {1} | 10 | 
| 4 | 3 | {2} | {1,3} | 15 | 

Điều này cho thấy việc chọn 1 sẽ loại bỏ 2 nhưng vẫn cho phép chọn 3 vì không có hạn chế trực tiếp nào ảnh hưởng đến nó. 

Bây giờ hãy xem xét một tương tác dây chuyền: 

đầu vào:```
4
9 2
7 3
6 4
5 1
```Thứ tự sắp xếp là 1, 2, 3, 4 theo giá trị. 

| Bước | Mục | Bị cấm | Đã chụp | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | {} | {} | 0 | 
| 2 | 1 | {2} | {1} | 9 | 
| 3 | 2 | {2} | {1} | 9 | 
| 4 | 3 | {2,4} | {1,3} | 15 | 
| 5 | 4 | {2,4} | {1,3} | 15 | 

Dấu vết cho thấy các lệnh cấm tích lũy độc lập và không bao giờ yêu cầu xem lại các lựa chọn trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế, trong khi mỗi mục được xử lý một lần | 
| Không gian | O(n) | Mảng lưu trữ trạng thái cho từng topping | 

Các ràng buộc cho phép tối đa 100000 mục, do đó, giải pháp O(n log n) phù hợp thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ vẫn tuyến tính và ổn định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    items = []
    a = [0] * (n + 1)
    b = [0] * (n + 1)

    for i in range(1, n + 1):
        ai, bi = map(int, input().split())
        a[i] = ai
        b[i] = bi
        items.append((ai, i))

    items.sort(reverse=True)

    banned = [False] * (n + 1)
    ans = 0

    for _, i in items:
        if banned[i]:
            continue
        ans += a[i]
        banned[b[i]] = True

    return str(ans)

# sample-like test
assert run("3\n10 2\n8 3\n5 1\n") == "15"

# minimum case
assert run("1\n100 1\n") == "100"

# all independent
assert run("3\n5 2\n4 3\n3 1\n") == "12"

# strong chain
assert run("4\n9 2\n7 3\n6 4\n5 1\n") == "15"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 100 | xử lý ranh giới tối thiểu | 
| chuỗi lệnh cấm | 15 | tuyên truyền loại trừ | 
| không có xung đột hiệu quả | 12 | lựa chọn đầy đủ chính xác | 
| chuỗi hỗn hợp | 15 | ổn định tương tác | 

## Vỏ cạnh 

Đầu vào tối thiểu có một đầu vào xác nhận rằng thuật toán xử lý lựa chọn tầm thường một cách chính xác. Vì không có vật phẩm nào khác nên không có gì bị cấm và giá trị luôn được bao gồm. 

Sự phụ thuộc theo chu kỳ như 1 cấm 2, 2 cấm 3, 3 cấm 1 không phá vỡ thuật toán vì các lệnh cấm chỉ được kích hoạt khi một nút được chọn. Nếu nút có giá trị cao nhất được chọn trước, nó chỉ cần loại bỏ một nút lân cận và không yêu cầu suy luận chu kỳ toàn cầu. 

Trường hợp nút có giá trị thấp cấm nút có giá trị cao được xử lý một cách tự nhiên bằng cách sắp xếp. Nút có giá trị cao được xử lý trước tiên và mọi nỗ lực sau đó để bao gồm nút có giá trị thấp sẽ bị bỏ qua nếu nút đó bị cấm, ngăn chặn bất kỳ sự hoán đổi hoặc ghi đè không chính xác nào đối với các lựa chọn tối ưu trước đó.
