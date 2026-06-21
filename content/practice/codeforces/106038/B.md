---
title: "CF 106038B - Astana"
description: "Chúng ta được cung cấp một danh sách các số nguyên riêng biệt được sắp xếp theo thứ tự thời gian, trong đó mỗi số nguyên biểu thị một ngày mà Bernardo đến phòng tập thể dục. Mục tiêu là xác định chuỗi ngày dương lịch liên tiếp dài nhất có trong danh sách này."
date: "2026-06-20T21:04:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106038
codeforces_index: "B"
codeforces_contest_name: "UNICAMP Selection Contest 2025"
rating: 0
weight: 106038
solve_time_s: 50
verified: true
draft: false
---

[CF 106038B - Astana](https://codeforces.com/problemset/problem/106038/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên riêng biệt được sắp xếp theo thứ tự thời gian, trong đó mỗi số nguyên biểu thị một ngày mà Bernardo đến phòng tập thể dục. Mục tiêu là xác định chuỗi ngày dương lịch liên tiếp dài nhất có trong danh sách này. Một vệt được hình thành khi các ngày cách nhau đúng một, chẳng hạn như 5, 6, 7 tạo thành một vệt có độ dài 3 và chúng tôi muốn độ dài tối đa trên tất cả các chuỗi liền kề như vậy. 

Kích thước đầu vào là tuyến tính theo số lượt truy cập được ghi lại, do đó, bất kỳ giải pháp nào quét mảng liên tục hoặc cố gắng mở rộng khoảng thời gian từ mọi điểm bắt đầu vẫn phải đủ hiệu quả để xử lý tối đa khoảng 200.000 phần tử trong vòng một giây. Điều này loại trừ mọi hành vi bậc hai, vì việc quét lồng nhau trên dữ liệu đã được sắp xếp sẽ dễ dàng vượt quá thứ tự 10^10 thao tác trong trường hợp xấu nhất. 

Một dạng lỗi phổ biến xuất phát từ việc xử lý vấn đề như thể chúng ta cần xem xét tất cả các cặp hoặc tính toán lại độ dài vệt một cách độc lập cho từng chỉ mục. Ví dụ, với đầu vào đã cho`1 2 3 4 10 11`, một cách tiếp cận đơn giản có thể bắt đầu từ mỗi chỉ mục và liên tục kiểm tra chuyển tiếp, tính toán lại các phân đoạn chồng chéo nhiều lần, điều này trở nên dư thừa và chậm chạp. 

Một trường hợp khó phát hiện khác là khi không có phần tử liên tiếp nào cả. Ví dụ, đầu vào`5 10 20`sẽ trả về 1, vì mỗi ngày đứng một mình như một chuỗi dài một. Bất kỳ giải pháp nào giả định tồn tại ít nhất một cặp liên tiếp và khởi tạo câu trả lời không chính xác đều có thể thất bại ở đây. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: đối với mỗi vị trí trong mảng, hãy coi nó là điểm bắt đầu của một chuỗi và kéo dài về phía trước trong khi giá trị tiếp theo chính xác là lớn hơn giá trị hiện tại một đơn vị. Điều này tính toán chính xác chuỗi dài nhất bắt đầu từ mỗi chỉ mục và lấy mức tối đa trên tất cả các lần bắt đầu mang lại câu trả lời đúng. 

Tuy nhiên, phương pháp này liên tục quét các phân đoạn chồng chéo. Trong trường hợp xấu nhất khi mảng đó là một chuỗi dài liên tiếp, chẳng hạn như`1 2 3 4 ... n`, chỉ mục đầu tiên quét n phần tử, chỉ mục thứ hai quét n-1, v.v., dẫn đến so sánh khoảng n2/2. Điều này trở nên quá chậm khi n lớn. 

Điều quan trọng là vì mảng đã được sắp xếp nên các chuỗi liên tiếp xuất hiện dưới dạng các khối liền kề nhau. Thay vì bắt đầu lại quá trình quét ở mọi chỉ mục, chúng ta chỉ cần theo dõi độ dài của vệt hiện tại khi quét một lần từ trái sang phải. Khi sự khác biệt giữa các phần tử liên tiếp chính xác là một, chúng tôi sẽ kéo dài vệt; nếu không, chúng tôi đặt lại nó. Điều này loại bỏ tất cả các tính toán lại dư thừa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng trong một lượt duy nhất trong khi vẫn duy trì độ dài của chuỗi liên tiếp hiện tại và câu trả lời hay nhất được thấy cho đến nay. 

1. Khởi tạo một biến`best`là 1, vì bất kỳ phần tử đơn lẻ nào cũng tạo thành một chuỗi có độ dài hợp lệ. Điều này xử lý trường hợp không có cặp liên tiếp. 
2. Khởi tạo`cur`là 1, biểu thị độ dài của chuỗi hiện tại kết thúc ở phần tử đầu tiên. 
3. Lặp lại từ phần tử thứ hai đến cuối mảng. Tại mỗi chỉ số`i`, so sánh`a[i]`với`a[i-1]`. 
4. Nếu`a[i] == a[i-1] + 1`, tăng`cur`bằng 1 vì chuỗi tiếp tục mà không bị gián đoạn. Điều này trực tiếp mã hóa định nghĩa của những ngày liên tiếp. 
5. Nếu không, hãy đặt lại`cur`thành 1 vì vệt bị đứt và vệt mới bắt đầu ở vị trí`i`. 
6. Sau khi cập nhật`cur`, cập nhật`best`BẰNG`max(best, cur)`để theo dõi vệt tối đa được nhìn thấy ở bất kỳ đâu trong mảng. 
7. Sau khi kết thúc vòng lặp, xuất ra`best`. 

Tính đúng đắn đến từ việc duy trì tính bất biến ở mọi chỉ số`i`,`cur`bằng độ dài của chuỗi liên tiếp dài nhất kết thúc chính xác tại`i`. Vì mỗi chuỗi phải kết thúc ở một vị trí nào đó nên việc theo dõi tất cả các điểm cuối như vậy đảm bảo chúng tôi nắm bắt được mức tối đa toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = list(map(int, input().split()))
    if not data:
        return

    n = data[0]
    arr = data[1:]

    # In some inputs n may not be needed explicitly if all values are present
    # but we trust format: first number is n, followed by n values.
    if len(arr) < n:
        arr += list(map(int, input().split()))

    if n == 0:
        print(0)
        return

    best = 1
    cur = 1

    for i in range(1, n):
        if arr[i] == arr[i - 1] + 1:
            cur += 1
        else:
            cur = 1
        if cur > best:
            best = cur

    print(best)

if __name__ == "__main__":
    solve()
```Mã thực hiện một lần quét tuyến tính duy nhất. Trạng thái duy nhất được giữ lại là độ dài chuỗi hiện tại và kết quả tốt nhất cho đến nay. Sự so sánh`arr[i] == arr[i-1] + 1`là toàn bộ cơ chế quyết định và mọi thứ khác đều được ghi sổ xung quanh nó. 

Một lỗi triển khai phổ biến là quên đặt lại chuỗi hiện tại khi chuỗi bị hỏng, điều này sẽ tích lũy giá trị không chính xác trên các khoảng trống. Một cái khác đang khởi tạo`best`thành 0, điều này sẽ không thành công khi mảng có ít nhất một phần tử nhưng không có cặp liên tiếp. 

## Ví dụ đã hoạt động 

Xem xét đầu vào`5 100 101 102 103 200`. 

| tôi | mảng[i] | mảng[i-1] | Liên tiếp? | cur | tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 100 | - | - | 1 | 1 | 
| 1 | 101 | 100 | vâng | 2 | 2 | 
| 2 | 102 | 101 | vâng | 3 | 3 | 
| 3 | 103 | 102 | vâng | 4 | 4 | 
| 4 | 200 | 103 | không | 1 | 4 | 

Điều này cho thấy một chuỗi dài được kéo dài như thế nào cho đến khi thời gian nghỉ đặt lại chuỗi đó và mức tối đa được giữ nguyên. 

Bây giờ hãy xem xét đầu vào`6 1 2 3 10 11 12`. 

| tôi | mảng[i] | mảng[i-1] | Liên tiếp? | cur | tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | - | - | 1 | 1 | 
| 1 | 2 | 1 | vâng | 2 | 2 | 
| 2 | 3 | 2 | vâng | 3 | 3 | 
| 3 | 10 | 3 | không | 1 | 3 | 
| 4 | 11 | 10 | vâng | 2 | 3 | 
| 5 | 12 | 11 | vâng | 3 | 3 | 

Điều này xác nhận thuật toán xử lý chính xác nhiều vệt riêng biệt và không hợp nhất chúng một cách không chính xác giữa các khoảng trống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần với công việc liên tục | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được duy trì | 

Quét tuyến tính là tối ưu vì mọi phần tử đầu vào phải được đọc ít nhất một lần và các thao tác trên mỗi phần tử là không đổi. Điều này phù hợp một cách thoải mái trong giới hạn thời gian đối với các ràng buộc điển hình của Codeforce lên tới 2×10^5 trở lên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples (interpreted)
assert run("5\n100 101 102 103 200\n") == "4"
assert run("4\n10 20 30 40\n") == "1"

# single element
assert run("1\n7\n") == "1", "single element"

# full consecutive
assert run("5\n1 2 3 4 5\n") == "5", "full streak"

# two streaks
assert run("6\n1 2 3 10 11 12\n") == "3", "two blocks"

# alternating gaps
assert run("5\n1 3 5 7 9\n") == "1", "no consecutive pairs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | ranh giới tối thiểu | 
| 1 2 3 4 5 | 5 | vệt mảng đầy đủ | 
| 1 2 3 10 11 12 | 3 | nhiều khối vệt | 
| 1 3 5 7 9 | 1 | không liền kề | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi mảng có kích thước bằng một. Thuật toán khởi tạo cả hai`cur`Và`best`thành 1, do đó nó trực tiếp xuất ra 1 mà không cần nhập bất kỳ lần lặp vòng lặp nào. Đối với đầu vào`7`, trạng thái không thay đổi và câu trả lời đúng là 1. 

Một trường hợp khác là khi toàn bộ dãy tăng nghiêm ngặt nhưng không liên tiếp, chẳng hạn như`1 3 4 6`. Thuật toán đặt lại`cur`tại mỗi lần nghỉ, đảm bảo không xảy ra sự hợp nhất không hợp lệ. trận chung kết`best`trở thành 2 do cặp`3 4`, đó là đoạn liên tiếp dài nhất chính xác. 

Trường hợp khó phát hiện cuối cùng là khi vệt dài nhất nằm ở đầu hoặc cuối. Từ`best`được cập nhật sau mỗi bước và không chỉ khi nghỉ giải lao, kết thúc như`1 2 3 4`hoặc`10 11 12 13`đều được ghi lại hoàn toàn mà không cần xử lý hậu kỳ đặc biệt.
