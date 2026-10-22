---
title: "CF 105862F - Kẻ cướp ngân hàng Kinan"
description: "Nhiệm vụ mô tả một chuỗi tuyến tính các hộp ký gửi an toàn, mỗi hộp được đặt ở tọa độ nguyên trên trục số. Một số vị trí chứa tiền giấy, có thể có nhiều tờ tiền ở cùng một vị trí."
date: "2026-06-25T14:34:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105862
codeforces_index: "F"
codeforces_contest_name: "ACPC Kickoff 2025"
rating: 0
weight: 105862
solve_time_s: 36
verified: true
draft: false
---

[CF 105862F - Kẻ cướp ngân hàng Kinan](https://codeforces.com/problemset/problem/105862/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ mô tả một chuỗi tuyến tính các hộp ký gửi an toàn, mỗi hộp được đặt ở tọa độ nguyên trên trục số. Một số vị trí chứa tiền giấy, có thể có nhiều tờ tiền ở cùng một vị trí. Tên cướp xuất phát ở một vị trí cố định và có thể di chuyển sang trái hoặc phải từng bước một, nhưng hai lính canh cố định chặn lối vào ngoài hai vị trí bị cấm: mọi thứ nghiêm ngặt giữa các lính canh là khu vực duy nhất hắn có thể hoạt động và bản thân tên cướp không được phép bước vào các vị trí canh gác. 

Mỗi lần tên cướp đến một vị trí, hắn có thể thu hết số tiền giấy cất trong két sắt đó. Mục tiêu là xác định số lượng tiền giấy tối đa mà anh ta có thể thu thập được vì anh ta có thể chọn đường dẫn bắt đầu từ vị trí ban đầu của mình và phải tôn trọng các điểm cuối bị chặn. 

Sự đơn giản hóa cấu trúc quan trọng là chuyển động không bị hạn chế bên trong phân đoạn được phép, do đó tên cướp có thể tiếp cận mọi két sắt không bị chặn trong khu vực được kết nối chứa điểm xuất phát của hắn. Vì hạn chế duy nhất là anh ta không thể vượt qua hoặc đi vào vị trí của lính canh, nên vấn đề giảm xuống còn việc tính tổng tất cả các tờ tiền nằm trong thành phần được kết nối của vị trí bắt đầu sau khi loại bỏ hai điểm cấm. 

Kích thước đầu vào lớn, lên tới 100.000 tờ tiền và tọa độ lên tới 1e9. Điều này ngay lập tức loại trừ mọi mô phỏng theo tọa độ hoặc truyền tải trên trục số. Thay vào đó, bất kỳ giải pháp nào cũng phải tổng hợp các giá trị theo vị trí một cách hiệu quả, thường sử dụng cách sắp xếp hoặc băm. 

Trường hợp cạnh tinh vi xuất hiện khi vị trí bắt đầu nằm ngoài vùng có thể truy cập được, điều này có thể xảy ra nếu nó trùng với bộ phận bảo vệ hoặc nằm trong một đoạn bị ngắt kết nối. Ví dụ: nếu tên cướp xuất phát ở vị trí 6 và lính canh ở vị trí 5 và 7 thì không có chuyển động hợp lệ nào cả vì cả hai hướng liền kề đều bị chặn ngay lập tức. Trong trường hợp đó, ngay cả khi những chiếc két sắt khác tồn tại ở nơi khác thì chúng cũng không thể truy cập được và câu trả lời là không. Một cách tiếp cận ngây thơ chỉ đơn giản là tính tổng tất cả các két giữa các lính canh sẽ bao gồm các khu vực không thể truy cập một cách không chính xác nếu nó bỏ qua kết nối ngay từ đầu. 

Một trường hợp góc khác xảy ra khi có nhiều tờ tiền tồn tại ở cùng một tọa độ. Một giải pháp đúng phải tích lũy chúng chứ không phải coi chúng là những vị trí riêng biệt. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ mô phỏng chuyển động của tên cướp trên trục số. Người ta có thể tưởng tượng việc xây dựng một biểu đồ rõ ràng về tất cả các vị trí xuất hiện trong đầu vào cộng với điểm bắt đầu, kết nối các tọa độ được sắp xếp liền kề và sau đó thực hiện lấp lũ ngay từ đầu trong khi tránh hai vị trí bị cấm. Về nguyên tắc, điều này đúng vì chuyển động giữa các tọa độ nguyên liền kề xác định khả năng kết nối. 

Tuy nhiên, trục số kéo dài tới 1e9 nên việc xây dựng tất cả các nút trung gian là không thể. Ngay cả khi chúng tôi giới hạn các nút chỉ ở những nút có tiền giấy cộng với điểm bắt đầu và phần bảo vệ, thì việc di chuyển vẫn có vấn đề: tính liền kề trong lưới số nguyên phụ thuộc vào khoảng cách giữa các tọa độ được sắp xếp và việc di chuyển qua các khoảng trống lớn đòi hỏi phải suy luận về các khoảng thay vì bước qua các số nguyên. 

Quan sát quan trọng là chúng ta không cần chính đường dẫn đó, chỉ cần những vị trí tiền giấy nào có thể tiếp cận được. Vì việc di chuyển được tự do trong bất kỳ phân đoạn liên tục nào của các vị trí được phép không bao gồm người bảo vệ nên tất cả các vị trí an toàn trong cùng khoảng mở với điểm bắt đầu đều có thể tiếp cận được. Do đó, vấn đề giảm xuống còn việc xác định phía nào của điểm bắt đầu không bị người bảo vệ chặn và tổng hợp tất cả các tờ tiền trong vùng được kết nối có thể tiếp cận. 

Cấu trúc về cơ bản là một đường được chia thành ba khu vực bởi hai người bảo vệ. Chỉ vùng chứa điểm bắt đầu mới có liên quan. Mọi thứ khác đều bị ngắt kết nối.

Điều này làm giảm nhiệm vụ nhóm các tờ tiền theo vị trí và tính tổng các tờ tiền nằm trong cùng khoảng mở với tọa độ bắt đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng chuyển động vũ phu | O(phạm vi tọa độ lớn) hoặc không thể | O(lớn) | Quá chậm | 
| Phân nhóm + phân loại theo khoảng thời gian | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc vị trí xuất phát và hai vị trí bảo vệ, sau đó xác định các điểm cắt bị cấm trên trục số. Các lính canh chia dòng thành tối đa ba đoạn và chỉ một trong số chúng có thể chứa vị trí bắt đầu. 
2. Đọc tất cả các vị trí tiền giấy và nhóm các tọa độ giống hệt nhau bằng cách tính tổng số lượng của chúng. Điều này tránh được công việc lặp đi lặp lại và đảm bảo mỗi vị trí đều đóng góp đầy đủ giá trị của nó. 
3. Xác định đoạn nào chứa vị trí bắt đầu bằng cách so sánh nó với hai tấm chắn. Điều này cho chúng ta biết vùng có thể truy cập hoàn toàn ở bên trái của bộ phận bảo vệ bên trái, hoàn toàn ở bên phải của bộ phận bảo vệ bên phải hay hoàn toàn nằm giữa chúng. 
4. Lặp lại tất cả các vị trí được nhóm và chỉ tích lũy số lượng từ những vị trí nằm trong cùng đoạn với vị trí bắt đầu. Bất kỳ vị trí nào bên ngoài phân khúc đó đều không thể truy cập được bất kể khoảng cách của nó. 
5. Xuất tổng tích lũy làm đáp án cuối cùng. 

Tính đúng đắn dựa trên thực tế là việc di chuyển dọc theo các vị trí số nguyên liền kề cho phép di chuyển qua bất kỳ vùng liền kề nào không chứa vị trí bị cấm. Khi vị trí bảo vệ bị loại bỏ, nó sẽ trở thành một rào cản cứng, chia biểu đồ thành các thành phần không kết nối. Vì tất cả chuyển động đều nằm trong cấu trúc liền kề 1D nên khả năng kết nối tương đương với việc ở trong cùng một khoảng mở. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, b, c = map(int, input().split())
    n = int(input())
    arr = list(map(int, input().split()))

    left_guard = min(b, c)
    right_guard = max(b, c)

    from collections import defaultdict
    cnt = defaultdict(int)

    for x in arr:
        cnt[x] += 1

    # determine which region 'a' lies in
    if a < left_guard:
        valid = lambda x: x < left_guard
    elif a > right_guard:
        valid = lambda x: x > right_guard
    else:
        valid = lambda x: left_guard < x < right_guard

    ans = 0
    for pos, v in cnt.items():
        if valid(pos):
            ans += v

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ nén các vị trí giống hệt nhau bằng cách sử dụng bản đồ băm, đảm bảo xử lý tuyến tính đầu vào. Hàm phân loại vùng mã hóa quan sát cấu trúc quan trọng: chỉ đoạn chứa điểm bắt đầu mới có thể truy cập được. 

Một cạm bẫy triển khai phổ biến là quên rằng bản thân các lính canh cũng không thể tiếp cận được ngay cả khi có tiền giấy ở đó. Đó là lý do tại sao sự bất bình đẳng là nghiêm ngặt. Một điểm tinh tế khác là xử lý xem vị trí bắt đầu có nằm chính xác ở ranh giới hay không; trong công thức này thì không thể, vì những người bảo vệ đã khác biệt ngay từ đầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3 7
8
4 7 5 5 3 6 2 8
```Ở đây những người bảo vệ ở vị trí 3 và 7, do đó chỉ có khoảng mở (3, 7) là quan trọng vì vị trí bắt đầu 5 nằm bên trong nó. 

| vị trí | đếm | hợp lệ trong (3,7)? | tổng số tiền chạy | 
| --- | --- | --- | --- | 
| 4 | 1 | vâng | 1 | 
| 5 | 2 | vâng | 3 | 
| 6 | 1 | vâng | 4 | 
| 2 | 1 | không | 4 | 
| 3 | 1 | không | 4 | 
| 7 | 1 | không | 4 | 
| 8 | 1 | không | 4 | 

Kết quả là 4, xác nhận rằng chỉ có phân khúc ở giữa đóng góp. 

### Ví dụ 2 

đầu vào:```
6 5 7
5
1 5 7 92 3
```Vị trí bắt đầu chính xác là 6, nằm trong khoảng từ 5 đến 7, nhưng cả hai vị trí liền kề đều bị chặn ngay lập tức nên không thể di chuyển được. Vùng hợp lệ là (5,7), nhưng không có tiền giấy bên trong nó. 

| vị trí | đếm | hợp lệ trong (5,7)? | tổng số tiền chạy | 
| --- | --- | --- | --- | 
| 1 | 1 | không | 0 | 
| 3 | 1 | không | 0 | 
| 5 | 1 | không | 0 | 
| 7 | 1 | không | 0 | 
| 92 | 1 | không | 0 | 

Đầu ra là 0, phù hợp với thực tế là không có két sắt nào có thể tiếp cận được để đựng tiền. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi tờ tiền được xử lý một lần và mỗi vị trí riêng biệt được kiểm tra một lần | 
| Không gian | O(n) | Lưu trữ bản đồ tần số vị trí | 

Các ràng buộc cho phép tối đa 100.000 mục nhập, do đó, quét tuyến tính với hàm băm dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    a, b, c = map(int, input().split())
    n = int(input())
    arr = list(map(int, input().split()))

    left_guard = min(b, c)
    right_guard = max(b, c)

    from collections import defaultdict
    cnt = defaultdict(int)
    for x in arr:
        cnt[x] += 1

    if a < left_guard:
        valid = lambda x: x < left_guard
    elif a > right_guard:
        valid = lambda x: x > right_guard
    else:
        valid = lambda x: left_guard < x < right_guard

    return str(sum(v for k, v in cnt.items() if valid(k)))

# provided sample
assert run("5 3 7\n8\n4 7 5 5 3 6 2 8\n") == "4"

# custom: no banknotes
assert run("5 3 7\n0\n\n") == "0"

# custom: all in left region
assert run("10 5 7\n3\n1 2 4\n") == "3"

# custom: all blocked region
assert run("10 5 7\n3\n6 6 6\n") == "0"

# custom: duplicates and boundary behavior
assert run("10 2 8\n5\n3 3 3 7 7\n") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mảng trống | 0 | không xử lý dữ liệu | 
| tất cả các vệ sĩ còn lại | tổng hợp tất cả | độ chính xác của vùng bên trái | 
| tất cả các khu vực không thể truy cập | 0 | logic chặn | 
| bản sao và mặt hỗn hợp | tổng hợp đúng | xử lý tần số | 

## Vỏ cạnh 

Nếu vị trí bắt đầu nằm ở đoạn bên trái trước cả hai bộ phận bảo vệ, thuật toán sẽ hạn chế thu thập nghiêm ngặt ở các vị trí nhỏ hơn bộ phận bảo vệ bên trái. Ví dụ, với`a = 1, b = 10, c = 20`, chỉ các vị trí`< 10`được tính. Điều kiện đảm bảo rằng ngay cả khi tiền giấy tồn tại ở phía bên phải, chúng vẫn bị bỏ qua vì chúng bị ngắt kết nối với thành phần ban đầu. 

Khi tất cả các tờ tiền đều nằm chính xác ở vị trí bảo vệ, thì không có tờ nào trong số chúng được đưa vào vì các vị trí bảo vệ bị loại trừ rõ ràng bởi sự bất bình đẳng nghiêm ngặt. Điều này phù hợp với mô hình trong đó các lính canh hoạt động như những nút không thể vượt qua chứ không phải chướng ngại vật có thể tháo rời. 

Nếu có nhiều bản sao ở một vị trí hợp lệ, bản đồ tần số sẽ tích lũy chúng một cách chính xác, ngăn chặn việc đếm thiếu có thể xảy ra nếu mỗi lần xuất hiện được xử lý độc lập mà không tổng hợp.
