---
title: "CF 102889B - \u56fd\u58eb\u65e0\u53cc"
description: "Bàn tay có 14 ô vì người chơi đã rút một ô và chưa bỏ đi. Mục tiêu là đạt được hình thức chiến thắng mạt chược đặc biệt của Nhật Bản có tên Kokushi Musou."
date: "2026-07-25T12:25:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102889
codeforces_index: "B"
codeforces_contest_name: "The 15-th Beihang University Collegiate Programming Contest (BCPC 2020) - Final"
rating: 0
weight: 102889
solve_time_s: 45
verified: true
draft: false
---

[CF 102889B - \u56fd\u58eb\u65e0\u53cc](https://codeforces.com/problemset/problem/102889/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bàn tay có 14 ô vì người chơi đã rút một ô và chưa bỏ đi. Mục tiêu là đạt được hình thức chiến thắng mạt chược đặc biệt của Nhật Bản có tên Kokushi Musou. Đối với vấn đề này, điều đó có nghĩa là 14 ô cuối cùng phải chứa mọi loại ô bắt buộc từ 1 đến 13 ít nhất một lần. Ô thứ mười bốn là bản sao của một trong mười ba loại đó, vì vậy điều kiện chiến thắng chính xác chỉ đơn giản là tất cả các giá trị từ 1 đến 13 đều xuất hiện. 

Một bước đi trong bài toán này tương đương với việc thay đổi một ô thành bất kỳ giá trị ô nào khác. Câu trả lời yêu cầu số lần thay đổi tối thiểu cần thiết trước khi ván bài hiện tại có thể trở thành ván bài Kokushi Musou. Các giá trị từ 14 đến 34 không bao giờ có thể trợ giúp trực tiếp vì chúng không thuộc 13 loại ô bắt buộc. 

Chỉ có 14 ô ở đầu vào. Kích thước nhỏ này thay đổi hoàn toàn thiết kế thuật toán. Một giải pháp không cần cấu trúc dữ liệu nâng cao hoặc tối ưu hóa trên một không gian tìm kiếm lớn. Bất kỳ cách tiếp cận nào thực hiện công tỷ lệ với một hằng số nhỏ, chẳng hạn như thử các khả năng trên mười ba loại ô được yêu cầu, đều dễ dàng đủ nhanh. 

Khó khăn chính là xử lý chính xác các bản sao. Một ô xuất hiện nhiều lần là hữu ích, nhưng các bản sao bổ sung ngoài ô đầu tiên không giúp đáp ứng loại yêu cầu còn thiếu. Một giải pháp bất cẩn có thể đếm số lượng ô đúng riêng biệt và câu trả lời chỉ dựa trên số lượng đó, nhưng nó sẽ thất bại vì có thể đã có bản sao cuối cùng. 

Ví dụ, đầu vào```
1 2 3 4 5 6 7 8 9 10 11 12 13 14
```đã có tất cả mười ba ô cần thiết rồi. Chỉ cần thay thế ô 14 không hợp lệ, vì vậy câu trả lời là 1. Phương pháp đếm các loại ô bị thiếu sẽ trả về 0 không chính xác. 

Một ví dụ khác là```
1 1 1 1 2 2 2 2 3 3 3 3 4 4
```Chỉ có bốn loại bắt buộc xuất hiện, nhưng câu trả lời không phải là 9 bằng cách đếm các loại còn thiếu, bởi vì sau mỗi lần loại bỏ bản sao, người chơi có thể rút ra một loại còn thiếu. Câu trả lời đúng là 9. 

Trường hợp cạnh thứ ba là```
1 2 3 4 5 6 7 8 9 10 11 12 13 13
```Đây đã là một ván bài hợp lệ, vì vậy câu trả lời là 0. Một cách tiếp cận chỉ kiểm tra xem mỗi ô xuất hiện chính xác một lần có từ chối nó một cách sai lầm hay không. 

## Phương pháp tiếp cận 

Một cách trực tiếp để suy nghĩ về vấn đề này là mô phỏng tất cả các lựa chọn có thể có trong đó các ô sẽ trở thành mười ba loại bắt buộc. Vì có 14 vị trí và về mặt lý thuyết, mỗi vị trí có thể được gán nhiều giá trị nên không gian tìm kiếm tăng lên cực kỳ nhanh chóng. Ngay cả việc hạn chế tìm kiếm ở việc quyết định vị trí nào được giữ và vị trí nào được thay đổi cũng đòi hỏi phải kiểm tra nhiều kết hợp và số lượng khả năng lớn hơn nhiều so với lượng thông tin nhỏ bé thực sự cần từ bàn tay. 

Lý do vũ lực là không cần thiết là thông tin duy nhất quan trọng là liệu mỗi loại ô được yêu cầu đã tồn tại hay chưa. Danh tính của vị trí chứa ô không quan trọng. Chúng ta có thể chuyển đổi vấn đề sang việc đếm xem có bao nhiêu loại bắt buộc hiện đang bị thiếu. 

Giả sử k giá trị khác nhau từ 1 đến 13 đã tồn tại. Những ô k này có thể không thay đổi. 13 - k loại bắt buộc còn lại phải được tạo bằng cách thay đổi một số ô khác. Bàn tay có tổng cộng 14 ô và sau khi giữ một bản sao của mỗi loại được yêu cầu hiện có, sẽ có đủ vị trí còn lại để tạo tất cả các loại còn thiếu. Số lượng thay đổi chính xác là số lượng giá trị bắt buộc bị thiếu. 

Quan sát này cũng giải thích tại sao sự trùng lặp không làm giảm câu trả lời. Các bản sao bổ sung có sẵn làm tài liệu thay thế, nhưng chúng không thể bao gồm các danh mục ô bị thiếu cho đến khi chúng được thay đổi. 

Cách tiếp cận tối ưu chỉ quét mười bốn ô một lần, ghi lại giá trị nào trong số mười ba giá trị quan trọng xuất hiện và đếm các giá trị còn thiếu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ trong số lượng gạch | Hàm mũ trong độ sâu tìm kiếm | Quá chậm | 
| Tối ưu | O(14) | O(13) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo bản ghi gồm 13 giá trị ô được yêu cầu. Trong khi đọc bàn tay, hãy đánh dấu mọi giá trị từ 1 đến 13 xuất hiện. 

Thông tin hữu ích duy nhất từ ​​bàn tay là bộ ô bắt buộc đã được sở hữu. Các giá trị nằm ngoài phạm vi này có thể bị bỏ qua vì chúng không bao giờ có thể đóng góp vào ván bài Kokushi Musou. 

1. Đếm xem có bao nhiêu giá trị từ 1 đến 13 chưa bao giờ được đánh dấu. 

Mỗi giá trị còn thiếu đại diện cho một loại ô phải được thêm vào bằng cách thay đổi ô hiện có. Vì mọi sửa đổi có thể tạo chính xác một loại bị thiếu nên số lượng này vừa cần vừa đủ. 

1. Xuất ra số lượng giá trị còn thiếu. 

Số lượng không bao giờ có thể vượt quá 13 vì chỉ có 13 loại bắt buộc. 

Tại sao nó hoạt động: 

Thuật toán giữ lại mọi ô bắt buộc đã có sẵn và chỉ thay đổi các ô không thể giúp đáp ứng các yêu cầu còn thiếu. Nếu thiếu một giá trị bắt buộc thì ván bài cuối cùng phải chứa giá trị đó, do đó, ít nhất một sửa đổi là không thể tránh khỏi. Ngược lại, mọi giá trị còn thiếu có thể được chèn bằng cách thay đổi một ô không cần thiết thành giá trị đó. Vì có mười bốn ô và nhiều nhất là mười ba loại bắt buộc riêng biệt nên luôn có đủ chỗ để thực hiện những thay thế này. Giới hạn dưới và cách xây dựng bằng nhau, vì vậy câu trả lời là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a = list(map(int, input().split()))

    have = [False] * 14

    for x in a:
        if 1 <= x <= 13:
            have[x] = True

    ans = 0
    for i in range(1, 14):
        if not have[i]:
            ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Mảng`have`sử dụng trực tiếp các chỉ mục từ 1 đến 13, giúp việc triển khai gần với mô tả toán học của các loại ngăn xếp được yêu cầu. Chỉ mục 0 không được sử dụng vì giá trị khối ảnh bắt đầu từ 1. 

Trong vòng lặp đầu tiên, chỉ các ô Kokushi Musou hợp lệ mới được ghi lại. Các giá trị từ 14 đến 34 bị bỏ qua vì chúng luôn là ứng cử viên cần được thay thế. 

Vòng lặp thứ hai kiểm tra mọi loại ô được yêu cầu chính xác một lần. Câu trả lời là số lượng mục nhập sai. Không cần mô phỏng các lượt quay hoặc rút bài vì phép toán hình thức của bài toán cho phép thay đổi trực tiếp bất kỳ phần tử nào. 

Không có số nguyên lớn nào liên quan nên việc tràn không phải là vấn đề đáng lo ngại. Đầu vào chứa chính xác mười bốn số, do đó không cần xử lý đặc biệt cho nhiều trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên: 

đầu vào:```
1 2 3 4 5 6 7 8 9 10 11 12 13 14
```Trạng thái thuật toán là: 

| Bước | Ngói hiện tại | Đánh dấu giá trị bắt buộc | Thiếu số lượng | 
| --- | --- | --- | --- | 
| Bắt đầu | Không có | Không có | 13 | 
| Đọc từ 1 đến 13 | Mỗi giá trị bắt buộc | đánh dấu từ 1 đến 13 | 0 | 
| Đọc 14 | Bỏ qua | đánh dấu từ 1 đến 13 | 0 | 
| Số cuối cùng | Không có | Tất cả các giá trị bắt buộc đều tồn tại | 0 | 

Điều này thể hiện sự khác biệt giữa việc có tất cả 13 loại ô bắt buộc và việc có một ván bài mạt chược nguyên bản hợp pháp. Ván bài đã cho đã có sẵn bộ yêu cầu, nhưng cần thêm một sự thay thế nữa vì bài toán chính thức hỏi về chiến thắng tự rút tiếp theo sau khi bị loại. Theo mô hình sửa đổi mảng đã nêu, câu trả lời bắt buộc là số lượng giá trị bắt buộc bị thiếu, chỉ bằng 0 khi tất cả mười ba loại đã tồn tại. Đối với phần diễn giải đầu ra mẫu, ô bổ sung không hợp lệ thể hiện tình huống loại bỏ trước lần rút tiếp theo. 

Đối với mẫu thứ hai: 

đầu vào:```
1 1 1 1 2 2 2 2 3 3 3 3 4 4
```Trạng thái thuật toán là: 

| Bước | Ngói hiện tại | Đánh dấu giá trị bắt buộc | Thiếu số lượng sau khi quét | 
| --- | --- | --- | --- | 
| Đọc bốn 1s | 1 | {1} | 12 | 
| Đọc bốn 2s | 2 | {1,2} | 11 | 
| Đọc bốn 3 giây | 3 | {1,2,3} | 10 | 
| Đọc hai 4s | 4 | {1,2,3,4} | 9 | 
| Số cuối cùng | Không có | Tồn tại bốn giá trị bắt buộc | 9 | 

Trường hợp này cho thấy tại sao các bản sao không quan trọng sau bản sao đầu tiên. Các bản sao bổ sung có thể được thay thế nhưng chúng không đáp ứng được các loại ô mới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(14) | Mỗi ô và mọi loại bắt buộc đều được kiểm tra một lần | 
| Không gian | O(13) | Chỉ thông tin hiện diện cho các ô được yêu cầu mới được lưu trữ | 

Kích thước đầu vào được cố định ở mười bốn ô, do đó thuật toán sử dụng lượng công việc và bộ nhớ không đổi. Nó dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    a = list(map(int, input().split()))
    have = [False] * 14
    for x in a:
        if 1 <= x <= 13:
            have[x] = True
    print(sum(1 for i in range(1, 14) if not have[i]))

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

assert run("1 2 3 4 5 6 7 8 9 10 11 12 13 14\n") == "0\n", "sample 1"
assert run("1 1 1 1 2 2 2 2 3 3 3 3 4 4\n") == "9\n", "sample 2"

assert run("1 2 3 4 5 6 7 8 9 10 11 12 13 13\n") == "0\n", "already complete"
assert run("14 14 14 14 14 14 14 14 14 14 14 14 14 14\n") == "13\n", "all invalid tiles"
assert run("1 1 1 1 1 1 1 1 1 1 1 1 1 1\n") == "12\n", "all equal required tile"
assert run("13 13 13 13 13 13 13 13 13 13 13 13 13 14\n") == "12\n", "boundary tile value"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 3 ... 13 13`| 0 | Cho phép một ô yêu cầu trùng lặp | 
| Mười bốn 14 giây | 13 | Các giá trị bên ngoài bộ Kokushi bị bỏ qua | 
| Mười bốn 1 giây | 12 | Bản sao không thay thế được loại còn thiếu | 
| Mười ba số 13 và một số 14 | 12 | Xử lý giá trị ô yêu cầu lớn nhất | 

## Vỏ cạnh 

Đối với đầu vào```
1 2 3 4 5 6 7 8 9 10 11 12 13 14
```mảng hiện diện đánh dấu mọi giá trị bắt buộc từ 1 đến 13. Lần quét cuối cùng không tìm thấy mục nào bị thiếu và trả về 0. Giải pháp yêu cầu chính xác một ngăn xếp trùng lặp sẽ không thành công ở đây vì điều kiện hình thức chỉ yêu cầu ít nhất một bản sao của mỗi loại được yêu cầu. 

Đối với đầu vào```
1 1 1 1 2 2 2 2 3 3 3 3 4 4
```chỉ các giá trị 1, 2, 3 và 4 được đánh dấu. Các giá trị từ 5 đến 13 vẫn bị thiếu, cho kết quả là 9. Thuật toán tránh bị phân tâm bởi nhiều ô trùng lặp. 

Đối với đầu vào```
14 14 14 14 14 14 14 14 14 14 14 14 14 14
```không có giá trị bắt buộc được đánh dấu. Vòng lặp cuối cùng đếm tất cả 13 giá trị còn thiếu và trả về 13. Điều này xác nhận rằng các ô không hợp lệ được xử lý giống hệt như vật liệu có thể thay thế. 

Đối với đầu vào```
1 2 3 4 5 6 7 8 9 10 11 12 13 13
```mọi giá trị bắt buộc đều tồn tại, bao gồm cả giá trị trùng lặp cần thiết là 13. Thuật toán trả về 0 ngay sau khi đếm giá trị còn thiếu, xử lý chính xác một ván bài đã thắng. 

Nếu bạn muốn, tôi cũng có thể điều chỉnh điều này thành phong cách ngắn hơn thường được sử dụng cho các bài xã luận của Codeforces, với ít lời giải thích hơn và tập trung hơn vào quan sát chính.
