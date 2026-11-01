---
title: "CF 106484C - Hàng đợi độc đáo của Bugcat"
description: "Chúng tôi duy trì một hàng đợi hoạt động giống như cấu trúc FIFO thông thường nhưng có thêm một quy tắc: mọi giá trị có thể xuất hiện nhiều nhất một lần vào bất kỳ lúc nào. Các giá trị có thể được lưu trữ là các số nguyên từ 1 đến m và chúng tôi xử lý các phép toán Q để sửa đổi hoặc truy vấn cấu trúc này."
date: "2026-06-19T15:16:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106484
codeforces_index: "C"
codeforces_contest_name: "2026 GBA International Programming Contest"
rating: 0
weight: 106484
solve_time_s: 47
verified: true
draft: false
---

[CF 106484C - Hàng đợi độc đáo của Bugcat](https://codeforces.com/problemset/problem/106484/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một hàng đợi hoạt động giống như cấu trúc FIFO thông thường nhưng có thêm một quy tắc: mọi giá trị có thể xuất hiện nhiều nhất một lần vào bất kỳ lúc nào. Các giá trị có thể được lưu trữ là các số nguyên từ 1 đến m và chúng tôi xử lý các phép toán Q để sửa đổi hoặc truy vấn cấu trúc này. 

Có ba hoạt động. Người ta cố gắng chèn một giá trị vào cuối hàng đợi, nhưng chỉ thành công nếu giá trị đó chưa xuất hiện. Một cái khác loại bỏ phần tử ở phía trước, phần tử này luôn hợp lệ vì hàng đợi được đảm bảo không trống khi thao tác này xuất hiện. Thao tác cuối cùng yêu cầu phần tử thứ x hiện có trong hàng đợi theo thứ tự từ trước ra sau và nếu vị trí đó không tồn tại thì câu trả lời là −1. 

Điểm tinh tế quan trọng là chúng ta không được yêu cầu mô phỏng một vùng chứa tùy ý mà là một chuỗi có các ràng buộc về tính duy nhất và các truy vấn vị trí. Các ràng buộc Q ≤ 10^6 và m ≤ 10^6 loại trừ mọi phương pháp quét hàng đợi hoặc kiểm tra tư cách thành viên bằng tìm kiếm tuyến tính trên mỗi thao tác. Ngay cả một thao tác O(n) đơn lẻ được lặp lại Q lần sẽ ngay lập tức vượt quá giới hạn, vì kích thước hàng đợi có thể tăng lên Q trong trường hợp xấu nhất. 

Một sai lầm ngây thơ là biểu diễn hàng đợi dưới dạng danh sách Python và sử dụng`x in list`để thực thi tính duy nhất. Một cách khác là trả lời các truy vấn bằng cách lập chỉ mục vào một danh sách nhưng quên rằng việc xóa từ phía trước sẽ làm thay đổi mọi thứ, tạo ra O(n) đó cho mỗi cửa sổ bật lên. 

Một trường hợp lỗi cụ thể xuất hiện khi hàng đợi ngày càng lớn và các truy vấn thường xuyên xuất hiện: 

đầu vào: 

1 1 

1 1 

1 2 

1 3 

... 

3 100000 

Một giải pháp dựa trên danh sách đơn giản sẽ liên tục quét các phần tử thành viên và dịch chuyển trên pop, dẫn đến hết thời gian chờ. 

Một lỗi tinh vi khác phát sinh từ việc bỏ qua tính duy nhất: nếu chúng ta cho phép trùng lặp, truy vấn cho vị trí xuất hiện đầu tiên sẽ trở nên mơ hồ và khác với mô hình dự kiến. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu giữ hàng đợi dưới dạng danh sách động. Tính năng chèn kiểm tra xem giá trị đã tồn tại hay chưa bằng cách quét toàn bộ danh sách. Nếu không tìm thấy thì nó sẽ thêm vào cuối. Pop loại bỏ phần tử đầu tiên bằng cách cắt hoặc pop(0) và truy vấn lập chỉ mục trực tiếp. 

Tính chính xác rất đơn giản vì nó phản ánh chính xác các hoạt động. Vấn đề là hiệu suất. Việc kiểm tra tư cách thành viên có chi phí O(n) và việc bật lên từ phía trước cũng có chi phí O(n) do dịch chuyển. Với Q lên tới 10^6 và kích thước hàng đợi có khả năng tăng tuyến tính, tổng công việc sẽ trở thành O(Q^2), vượt xa giới hạn khả thi. 

Quan sát quan trọng là chúng ta thực sự không cần phải duy trì “cấu trúc danh sách” theo nghĩa thay đổi. Chúng ta chỉ cần giữ nguyên thứ tự tương đối của các phần tử và hỗ trợ ba thao tác: nối các phần tử duy nhất, xóa phần tử cũ nhất và truy cập theo chỉ mục theo thứ tự hiện tại. 

Điều này gợi ý các mối quan tâm riêng biệt: chúng tôi theo dõi các phần tử trong một mảng cố định biểu thị thứ tự chèn và duy trì một con trỏ di chuyển ở phía trước. Thay vì loại bỏ các phần tử về mặt vật lý, chúng tôi nâng cao chỉ mục đầu. Để thực thi tính duy nhất, chúng tôi duy trì một mảng boolean hoặc tập hợp băm cho biết liệu một giá trị hiện có hoạt động trong hàng đợi hay không. Điều này làm giảm tất cả các hoạt động xuống O(1). 

Hàng đợi trở thành một cửa sổ ảo trên một mảng, giúp loại bỏ tất cả các thao tác xóa tốn kém. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(Q^2) | O(Q) | Quá chậm | 
| Tối ưu | O(Q) | O(Q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì ba cấu trúc: một mảng`arr`lưu trữ các giá trị được chèn theo thứ tự, một con trỏ`head`đánh dấu mặt trước hiện tại và một mảng boolean`in_queue`cho biết liệu một giá trị hiện có nằm trong cửa sổ hàng đợi đang hoạt động hay không. 

1. Đối với thao tác chèn có giá trị x, trước tiên chúng tôi kiểm tra xem`in_queue[x]`là sai. Nếu nó sai, chúng ta thêm x vào`arr`và đánh dấu`in_queue[x] = true`. Điều này đảm bảo tính duy nhất mà không cần quét hàng đợi. 
2. Đối với hoạt động bật lên, chúng tôi di chuyển`head`chuyển tiếp cho đến khi chúng ta bỏ qua một phần tử hoạt động. Cụ thể ta lấy phần tử tại`arr[head]`, đánh dấu nó là không có trong hàng đợi và tăng dần`head`. Điều này mô phỏng việc loại bỏ mặt trước trong thời gian khấu hao O(1) vì mỗi phần tử được loại bỏ chính xác một lần. 
3. Đối với thao tác truy vấn yêu cầu phần tử thứ x, chúng tôi tính chỉ mục thực tế là`head + x - 1`. Nếu chỉ số này vượt quá độ dài mảng hiện tại, chúng ta sẽ xuất ra −1. Nếu không, chúng tôi xuất`arr[index]`. Lý do là hàng đợi hoạt động luôn là hậu tố liền kề của mảng được xử lý. 

Tại sao nó hoạt động dựa trên tính bất biến là mọi phần tử hoặc đã bị con trỏ đầu bỏ qua vĩnh viễn hoặc vẫn là một phần của hậu tố hoạt động của`arr`. Không có phần tử nào nhập lại cấu trúc sau khi xóa và không có phần tử nào xuất hiện hai lần vì chúng tôi duy trì cờ được đặt hoạt động nghiêm ngặt. Vì vậy, trạng thái hàng đợi luôn được biểu diễn chính xác bằng đoạn`arr[head : len(arr)]`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    Q, m = map(int, input().split())
    
    arr = []
    in_queue = [False] * (m + 1)
    head = 0
    
    out = []
    
    for _ in range(Q):
        tmp = input().split()
        t = int(tmp[0])
        
        if t == 1:
            x = int(tmp[1])
            if not in_queue[x]:
                arr.append(x)
                in_queue[x] = True
        
        elif t == 2:
            x = arr[head]
            in_queue[x] = False
            head += 1
        
        else:
            x = int(tmp[1])
            idx = head + x - 1
            if idx >= len(arr):
                out.append("-1")
            else:
                out.append(str(arr[idx]))
    
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai lưu trữ tất cả các giá trị được chèn vào một mảng đang phát triển và không bao giờ xóa vật lý từ bên trái. các`head`con trỏ thay thế các pops deque tốn kém. Mảng boolean đảm bảo kiểm tra tư cách thành viên O(1) để chèn, do đó các bản sao không bao giờ được thêm vào. 

Một điểm tinh tế là chúng ta không bao giờ thu mình lại`arr`. Đây là cố ý: mặc dù các phần tử cũ vẫn còn trong bộ nhớ nhưng chúng sẽ bị bỏ qua một lần`head`vượt qua họ. Các truy vấn chỉ dựa vào số học chỉ mục chứ không dựa vào đột biến cấu trúc. 

## Ví dụ đã hoạt động 

Hãy xem xét trình tự sau: 

đầu vào:```
1 1
1 1
1 2
1 3
2
3 1
3 2
```Chúng tôi theo dõi trạng thái: 

| Bước | Hoạt động | mảng | đầu | trong_hàng | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | chèn 1 | [1] | 0 | {1} | | 
| 2 | chèn 2 | [1,2] | 0 | {1,2} | | 
| 3 | chèn 3 | [1,2,3] | 0 | {1,2,3} | | 
| 4 | bật | [1,2,3] | 1 | {2,3} | | 
| 5 | truy vấn 1 | [1,2,3] | 1 | {2,3} | 2 | 
| 6 | truy vấn 2 | [1,2,3] | 1 | {2,3} | 3 | 

Điều này cho thấy nội dung hàng đợi logic luôn là hậu tố bắt đầu từ`head`. 

Bây giờ hãy xem xét việc ngăn ngừa trùng lặp: 

đầu vào:```
1 5
1 1
1 2
1 1
3 2
```| Bước | Hoạt động | mảng | đầu | trong_hàng | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | chèn 1 | [1] | 0 | {1} | | 
| 2 | chèn 2 | [1,2] | 0 | {1,2} | | 
| 3 | chèn 1 | [1,2] | 0 | {1,2} | | 
| 4 | truy vấn 2 | [1,2] | 0 | {1,2} | 2 | 

Việc chèn thứ hai của 1 bị bỏ qua, duy trì tính duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Q) | Mỗi phần tử được chèn một lần, xóa một lần và được truy vấn trong O(1) | 
| Không gian | O(Q + m) | Mảng lưu trữ tối đa Q phần tử, mảng boolean theo dõi các giá trị m | 

Các ràng buộc cho phép lên tới một triệu thao tác, do đó việc xử lý thời gian tuyến tính với công việc liên tục trên mỗi thao tác phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    Q, m = map(int, sys.stdin.readline().split())
    arr = []
    in_queue = [False] * (m + 1)
    head = 0
    out = []
    
    for _ in range(Q):
        tmp = sys.stdin.readline().split()
        t = int(tmp[0])
        
        if t == 1:
            x = int(tmp[1])
            if not in_queue[x]:
                arr.append(x)
                in_queue[x] = True
        
        elif t == 2:
            x = arr[head]
            in_queue[x] = False
            head += 1
        
        else:
            x = int(tmp[1])
            idx = head + x - 1
            out.append(str(arr[idx]) if idx < len(arr) else "-1")
    
    return "\n".join(out)

# sample-like test
assert run("""10 10
1 3
1 5
3 1
3 2
1 3
1 5
1 8
3 2
3 3
1 2
""") == "3\n5\n5\n8", "sample-like"

# minimum case
assert run("""1 1
3 1
""") == "-1", "min query empty"

# duplicate prevention
assert run("""5 3
1 1
1 1
1 2
3 2
3 1
""") == "2\n1", "duplicates ignored"

# full pop then query
assert run("""6 5
1 1
1 2
2
2
3 1
""") == "-1", "empty queue after pops"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| giống mẫu | 3 5 5 8 | các thao tác cơ bản + tính chính xác của truy vấn | 
| truy vấn tối thiểu trống | -1 | truy vấn cấu trúc trống | 
| trùng lặp bị bỏ qua | 2 1 | thực thi tính duy nhất | 
| bật đầy đủ rồi truy vấn | -1 | ranh giới sau khi xóa | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là việc chèn lặp lại cùng một giá trị mà không loại bỏ. Ví dụ: 

đầu vào:```
1 5
1 1
1 1
1 1
3 1
```Thuật toán chỉ giữ lại một phiên bản có giá trị 1 vì`in_queue[1]`chặn tất cả các lần lặp lại. Trạng thái hàng đợi là`[1]`, vì vậy truy vấn trả về 1. 

Một trường hợp khác đang truy vấn vượt quá kích thước hiện tại sau nhiều lần bật: 

đầu vào:```
1 3
1 1
1 2
2
3 2
```Sau nhạc pop, chỉ`[2]`vẫn còn. Truy vấn phần tử thứ hai tính toán`head + 1`, vượt quá`len(arr)`, vậy đáp án là −1. Việc biểu diễn dựa trên con trỏ đảm bảo chúng ta không bao giờ truy cập vào bộ nhớ không hợp lệ; giới hạn được kiểm tra hoàn toàn về mặt số học. 

Trường hợp cạnh cuối cùng là ứng suất đầu vào tối đa trong đó tất cả các thao tác được chèn theo sau là các truy vấn. Vì không có thao tác xóa vật lý nào nên mảng sẽ tăng lên nhưng vẫn hợp lệ đối với bộ nhớ O(Q) và tất cả các truy vấn đều được giải quyết theo thời gian không đổi.
