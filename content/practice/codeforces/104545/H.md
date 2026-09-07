---
title: "CF 104545H - Anh hùng Morethor"
description: "Chúng tôi đang mô phỏng một quá trình chiến đấu tuần tự liên quan đến một anh hùng và một danh sách quái vật. Anh hùng bắt đầu với giá trị sức mạnh ban đầu, sau đó chạm trán từng quái vật theo một thứ tự cố định. Mỗi quái vật có một giá trị sức mạnh."
date: "2026-06-30T08:58:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104545
codeforces_index: "H"
codeforces_contest_name: "VIII MaratonUSP Freshman Contest"
rating: 0
weight: 104545
solve_time_s: 57
verified: true
draft: false
---

[CF 104545H - Anh hùng Morethor](https://codeforces.com/problemset/problem/104545/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một quá trình chiến đấu tuần tự liên quan đến một anh hùng và một danh sách quái vật. Anh hùng bắt đầu với giá trị sức mạnh ban đầu, sau đó chạm trán từng quái vật theo một thứ tự cố định. Mỗi quái vật có một giá trị sức mạnh. Khi anh hùng gặp quái vật, anh ta chỉ có thể đánh bại nó nếu sức mạnh hiện tại của anh ta ít nhất bằng sức mạnh của quái vật. Nếu anh ta thắng, sức mạnh của anh ta sẽ tăng lên đúng bằng sức mạnh của con quái vật đó. Nếu anh ta gặp một con quái vật mà anh ta không thể đánh bại, quá trình sẽ dừng lại ngay lập tức. 

Nhiệm vụ là xác định xem liệu anh hùng có thể đánh bại thành công mọi quái vật theo trình tự nhất định hay không, bắt đầu từ sức mạnh ban đầu của anh ta. 

Kích thước đầu vào cho phép lên tới 100.000 quái vật. Điều này ngay lập tức cho thấy rằng bất kỳ cách tiếp cận nào liên quan đến các vòng lặp lồng nhau hoặc việc quét danh sách lặp đi lặp lại nhiều lần sẽ là quá chậm. Cần phải vượt qua tuyến tính một lần qua quái vật vì các hoạt động O(N2) sẽ vượt xa giới hạn chấp nhận được. 

Một trường hợp phức tạp phát sinh khi không có con quái vật nào cả. Trong trường hợp đó, người anh hùng sẽ thành công một cách tầm thường bất kể sức mạnh ban đầu của anh ta là bao nhiêu. Một trường hợp góc khác xuất hiện khi sức mạnh ban đầu của anh hùng bằng 0. Nếu quái vật đầu tiên có sức mạnh dương, anh hùng hoàn toàn không thể tiếp tục và câu trả lời sẽ trở thành tiêu cực trừ khi quái vật đầu tiên cũng có sức mạnh bằng 0. Sự tương tác không cường độ này có ý nghĩa quan trọng vì nó không phá vỡ hành vi tích lũy đơn điệu, nhưng nó có thể ảnh hưởng đến các quyết định chấm dứt sớm. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực đã rất gần với mô phỏng thực tế. Chúng tôi lặp lại các quái vật theo thứ tự, kiểm tra từng bước xem sức mạnh anh hùng hiện tại có đủ hay không. Nếu đúng như vậy, chúng ta cộng sức mạnh của quái vật vào sức mạnh của anh hùng và tiếp tục. Nếu không, chúng ta kết luận ngay là thất bại. 

Mô phỏng trực tiếp này là chính xác vì các quy tắc xác định rõ ràng một quy trình xác định không có lựa chọn hoặc chiến lược thay thế nào. Không có cách nào để sắp xếp lại các trận chiến hoặc bỏ qua quái vật, vì vậy quá trình tiến hóa trạng thái được cố định. 

Mối quan tâm ngây thơ sẽ là liệu chúng ta có cần xem xét lại các quyết định trước đó hay cố gắng quay lại nếu sau đó người anh hùng thất bại. Trực giác đó không áp dụng ở đây vì một khi anh hùng đánh bại quái vật, sức mạnh của anh ta chỉ tăng lên mà thôi. Không có cơ chế nào làm giảm sức mạnh hoặc đưa ra các trạng thái phân nhánh. Kết quả là mô phỏng đã tối ưu. 

Thông tin chi tiết tối ưu hóa có ý nghĩa duy nhất là nhận ra rằng không cần sắp xếp, sắp xếp lại tham lam hoặc xử lý trước. Thứ tự đầu vào là cố định và phải được tuân thủ chính xác, vì vậy giải pháp hoàn toàn là tích lũy tuyến tính với kiểm tra có điều kiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(N) | O(1) | Đã chấp nhận | 
| Quét tuyến tính tối ưu | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng quái vật N và sức mạnh anh hùng ban đầu H. Điều này xác định trạng thái bắt đầu của mô phỏng trước khi bất kỳ tương tác nào xảy ra. 
2. Lặp lại sức mạnh của từng quái vật theo thứ tự nhất định. Thứ tự là cần thiết vì anh hùng không thể chọn quái vật nào để chiến đấu trước. 
3. Đối với mỗi quái vật, hãy so sánh sức mạnh của nó với sức mạnh anh hùng hiện tại. Nếu sức mạnh của anh hùng hoàn toàn thấp hơn sức mạnh của quái vật, hãy chấm dứt ngay lập tức và đầu ra thất bại. 
4. Nếu anh hùng có thể đánh bại quái vật, hãy tăng sức mạnh của anh ta bằng cách tăng thêm sức mạnh cho quái vật. Điều này mô hình hóa quy tắc đánh bại quái vật sẽ tăng sức mạnh vĩnh viễn cho anh hùng. 
5. Tiếp tục quá trình này cho đến khi tất cả quái vật được xử lý hoặc xảy ra lỗi. 
6. Nếu vòng lặp hoàn thành mà không bị lỗi thì kết quả xuất ra là thành công. 

### Tại sao nó hoạt động

Điều bất biến chính là sau khi xử lý thành công quái vật thứ i, sức mạnh của anh hùng chính xác bằng sức mạnh ban đầu cộng với tổng sức mạnh của tất cả quái vật bị đánh bại tính đến thời điểm đó. Vì mỗi cuộc chiến thành công đều bảo toàn nghiêm ngặt tất cả sức mạnh đã đạt được trước đó và chỉ cộng thêm những giá trị tích cực, nên sức mạnh của anh hùng đơn điệu không giảm. Điều này đảm bảo rằng một khi một con quái vật có thể bị đánh bại ở lượt của nó, sẽ không có điều kiện ẩn nào trước đó có thể làm mất hiệu lực các so sánh trong tương lai. Do đó, mô phỏng phản ánh chính xác định nghĩa của vấn đề và bất kỳ lỗi nào gặp phải đều là điểm lỗi sớm nhất có thể xảy ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, h = map(int, input().split())
    if n == 0:
        print("SIM")
        return

    monsters = list(map(int, input().split()))

    for f in monsters:
        if h < f:
            print("NAO")
            return
        h += f

    print("SIM")

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp mã hóa mô phỏng từng bước. Séc`if n == 0`xử lý chuỗi trống một cách rõ ràng, vì không cần so sánh và anh hùng thành công một cách tầm thường. 

Bên trong vòng lặp, sự so sánh`h < f`nắm bắt được tình trạng thất bại. Nếu nó kích hoạt, chúng tôi sẽ ngừng xử lý ngay lập tức vì những quái vật sau này sẽ không còn liên quan nữa khi xảy ra lỗi. Nếu không, chúng ta tích lũy sức mạnh của quái vật vào`h`, mô hình mức tăng sức mạnh vĩnh viễn. 

Không cần cấu trúc dữ liệu bổ sung vì trạng thái chỉ phụ thuộc vào giá trị công suất hiện tại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 10
1 2 3 4 5
```| Bước | Sức mạnh anh hùng (trước) | Quái vật | Có Thể Đánh Bại | Sức Mạnh Anh Hùng (sau) | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | 1 | Có | 11 | 
| 2 | 11 | 2 | Có | 13 | 
| 3 | 13 | 3 | Có | 16 | 
| 4 | 16 | 4 | Có | 20 | 
| 5 | 20 | 5 | Có | 25 | 

Người anh hùng luôn duy trì đủ sức mạnh vì mỗi quái vật đều yếu hơn hoặc bằng mức tăng trưởng tích lũy từ các trận chiến trước. Điều này khẳng định tính chất tăng trưởng đơn điệu. 

Đầu ra:```
SIM
```### Ví dụ 2 

đầu vào:```
4 7
1 1 1 11
```| Bước | Sức mạnh anh hùng (trước) | Quái vật | Có Thể Đánh Bại | Sức Mạnh Anh Hùng (sau) | 
| --- | --- | --- | --- | --- | 
| 1 | 7 | 1 | Có | 8 | 
| 2 | 8 | 1 | Có | 9 | 
| 3 | 9 | 1 | Có | 10 | 
| 4 | 10 | 11 | Không | dừng lại | 

Thất bại xảy ra chính xác ở lần chạm trán không thể đầu tiên. Vì quá trình này dừng lại ngay lập tức nên những con quái vật sau này không liên quan. 

Đầu ra:```
NAO
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi quái vật được xử lý chính xác một lần với thời gian làm việc không đổi | 
| Không gian | O(1) | Chỉ một biến chạy duy nhất sẽ lưu trữ sức mạnh của anh hùng | 

Các ràng buộc cho phép tối đa 100.000 quái vật và một lần quét tuyến tính dễ dàng phù hợp với giới hạn thời gian. Việc sử dụng bộ nhớ là không đổi và không phụ thuộc vào kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return capture(solve)

def capture(func):
    import sys
    from io import StringIO
    backup = sys.stdout
    sys.stdout = StringIO()
    func()
    out = sys.stdout.getvalue()
    sys.stdout = backup
    return out.strip()

# provided samples
assert run("5 10\n1 2 3 4 5\n") == "SIM"
assert run("4 7\n1 1 1 11\n") == "NAO"

# edge: no monsters
assert run("0 5\n") == "SIM"

# edge: immediate failure
assert run("3 1\n2 1 1\n") == "NAO"

# edge: all zeros
assert run("5 0\n0 0 0 0 0\n") == "SIM"

# edge: large increasing
assert run("4 1\n1 2 4 8\n") == "SIM"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 5`| SIM | xử lý chuỗi trống | 
|`3 1 / 2 1 1`| NAO | thất bại ngay lập tức ở con quái vật đầu tiên | 
|`5 0 / all zeros`| SIM | ổn định tích lũy cường độ bằng không | 
|`4 1 / 1 2 4 8`| SIM | tính đúng đắn của chuỗi ngày càng tăng | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi không có quái vật. Đầu vào là`0 H`. Thuật toán đọc`n = 0`và ngay lập tức đưa ra thành công mà không cần cố gắng đọc hoặc xử lý dòng thứ hai. Điều này ngăn chặn việc phân tích cú pháp đầu vào không cần thiết và tránh các lỗi lập chỉ mục cạnh. 

Một trường hợp khác là khi anh hùng bắt đầu với sức mạnh bằng không. Đối với đầu vào:```
3 0
0 0 1
```quá trình mô phỏng tiến hành như sau. Quái vật đầu tiên là 0, vì vậy`h < f`là sai và sức mạnh vẫn là 0. Thứ hai cũng là 0, vẫn ổn. Ở con quái vật thứ ba,`0 < 1`gây ra sự thất bại. Thuật toán xác định chính xác trận chiến bất khả thi đầu tiên mà không bỏ qua hoặc sắp xếp lại. 

Trường hợp thứ ba là khi tất cả quái vật đều bằng không. Ví dụ:```
4 0
0 0 0 0
```mọi sự so sánh đều trôi qua kể từ đó`h >= f`luôn luôn giữ. Sức mạnh của người anh hùng không thay đổi nhưng quá trình đó không bao giờ thất bại, tạo nên thành công.
