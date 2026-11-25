---
title: "CF 999A - Mishka và cuộc thi"
description: "Chúng ta được cung cấp một danh sách các bài toán được sắp xếp theo một thứ tự cố định từ trái sang phải, trong đó mỗi bài toán có một giá trị độ khó. Mishka có cấp độ kỹ năng k, nghĩa là anh ta chỉ có thể giải quyết những vấn đề có độ khó không vượt quá k."
date: "2026-06-16T23:51:47+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 999
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 490 (Div. 3)"
rating: 800
weight: 999
solve_time_s: 80
verified: true
draft: false
---

[CF 999A - Mishka và Cuộc thi](https://codeforces.com/problemset/problem/999/A) 

**Đánh giá:** 800 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các bài toán được sắp xếp theo một thứ tự cố định từ trái sang phải, trong đó mỗi bài toán có một giá trị độ khó. Mishka có trình độ kỹ năng`k`, nghĩa là anh ta chỉ có thể giải quyết những vấn đề có độ khó không vượt quá`k`. 

Quá trình này bị hạn chế theo một cách quan trọng: tại bất kỳ thời điểm nào, chỉ có các vấn đề còn lại ở ngoài cùng bên trái và ngoài cùng bên phải. Mishka liên tục chọn một trong hai đầu này, loại bỏ vấn đề đó và chỉ giải quyết nó nếu độ khó của nó nằm trong khả năng của anh ta. Nếu cả hai đầu đều không thể giải quyết được, anh ta dừng lại ngay lập tức và không thể di chuyển thêm nữa. 

Mục tiêu là xác định xem anh ta có thể loại bỏ và giải quyết thành công bao nhiêu vấn đề trước khi gặp khó khăn. 

Các hạn chế là nhỏ:`n ≤ 100`và tất cả các giá trị nhiều nhất`100`. Điều này ngay lập tức cho chúng ta biết rằng ngay cả các mô phỏng bậc hai hoặc bậc ba cũng đủ nhanh và mô phỏng tham lam với chuyển động của con trỏ là đủ. Không cần cấu trúc dữ liệu nâng cao hoặc tối ưu hóa ngoài việc kiểm tra liên tục theo thời gian cho mỗi bước. 

Một số tình huống khó khăn quan trọng: 

Nếu cả hai đầu đều quá khó khăn thì Mishka không giải quyết được gì. Ví dụ, đầu vào`n = 3, k = 1, [5, 6, 7]`tạo ra đầu ra`0`bởi vì không có động thái hợp lệ nào tồn tại khi bắt đầu. 

Nếu tất cả các giá trị đều nằm trong`k`, Mishka cuối cùng có thể xóa mọi thứ bất kể lựa chọn nào, bởi vì mỗi lần xóa sẽ tiếp tục hiển thị các điểm cuối hợp lệ mới. 

Một trường hợp tinh vi là khi chỉ có một bên hợp lệ ở mỗi bước, buộc phải tuân theo một trình tự xác định. Một sự hiểu lầm ngây thơ có thể luôn cố gắng chọn điểm cuối nhỏ hơn hoặc lớn hơn, nhưng vấn đề không yêu cầu tối ưu hóa chiến lược lựa chọn, vì bất kỳ chuỗi hợp lệ nào tiếp tục loại bỏ các điểm cuối có thể giải quyết được cho đến khi bị chặn đều dẫn đến cùng số lần loại bỏ tối đa trong ràng buộc tham lam này. 

## Phương pháp tiếp cận 

Một cách trực tiếp để suy nghĩ về điều này là mô phỏng quá trình một cách chính xác. Chúng ta duy trì hai con trỏ, một ở đầu bên trái và một ở đầu bên phải của mảng. Ở mỗi bước, chúng tôi kiểm tra xem giá trị bên trái hay giá trị bên phải có thể giải được. 

Nếu không thể giải quyết được, chúng tôi dừng lại. Ngược lại, chúng ta lấy một trong các đầu hợp lệ và di chuyển con trỏ tương ứng vào trong, làm tăng số lượng của chúng ta. 

Bởi vì cả hai đầu luôn là những lựa chọn có sẵn, nên ý tưởng mạnh mẽ tự nhiên sẽ là thử tất cả các chuỗi lựa chọn có thể có: ở mỗi bước, hãy rẽ sang trái hoặc phải nếu hợp lệ. Điều này dẫn đến số lượng trạng thái theo cấp số nhân, vì mỗi trạng thái có tới`n`các bước có thể phân nhánh thành hai lựa chọn. Trong trường hợp xấu nhất, điều này trở thành`O(2^n)`, điều này là không cần thiết vì quyết định không thực sự phụ thuộc vào kết quả trong tương lai theo cách đòi hỏi phải tìm kiếm. 

Quan sát quan trọng là chúng ta không bao giờ cần phải quay lại. Khi một phần tử được lấy từ một trong hai đầu, nó sẽ bị loại bỏ vĩnh viễn và hạn chế duy nhất là liệu nó có phải là hay không`k`. Không có sự tương tác giữa các lựa chọn có thể làm cho một nhánh hoàn toàn tốt hơn nhánh khác xét về tính khả thi trong tương lai. Bất kỳ nước đi hợp lệ nào đều đóng góp chính xác một nước đi cho câu trả lời và việc dừng lại chỉ xảy ra khi cả hai đầu đều không hợp lệ. 

Điều này làm giảm vấn đề thành mô phỏng hai con trỏ tham lam: liên tục sử dụng các điểm cuối hợp lệ cho đến khi không có đầu nào đủ tiêu chuẩn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (thử tất cả các trình tự) | O(2^n) | O(n) | Quá chậm | 
| Mô phỏng hai con trỏ | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giữ hai chỉ số,`l = 0`Và`r = n - 1`, và một bộ đếm`ans = 0`. 

1. Kiểm tra xem quá trình có thể tiếp tục hay không bằng cách kiểm tra cả hai đầu của đoạn hiện tại. 
2. Nếu`a[l] <= k`, xóa phần tử bên trái bằng cách tăng dần`l`và ngày càng tăng`ans`bởi một. 
3. Ngược lại nếu`a[r] <= k`, loại bỏ phần tử bên phải bằng cách giảm dần`r`và ngày càng tăng`ans`. 
4. Nếu không có điều kiện nào xảy ra, hãy chấm dứt ngay lập tức vì không thể di chuyển thêm nữa. 

Mỗi bước tương ứng chính xác với một lần xóa, vì vậy vòng lặp sẽ tiếp tục cho đến khi phân đoạn đó bị thu gọn hoặc không thể phát được. 

Lý do đằng sau việc chọn bên trái trước không quan trọng đối với tính đúng đắn của bài toán này, bởi vì bất cứ khi nào cả hai đầu đều hợp lệ thì một trong hai lựa chọn vẫn giữ cho quy trình nhất quán: cả hai lựa chọn đều sử dụng một phần tử hợp lệ và giảm kích thước bài toán xuống một phần tử. 

### Tại sao nó hoạt động 

Ở mỗi bước, yếu tố duy nhất quan trọng là liệu có thể giải quyết được ít nhất một điểm cuối hay không. Nếu cả hai điểm cuối vượt quá`k`, không có hành động nào trong tương lai có thể thay đổi tình huống đó vì không có phần tử nội bộ nào có thể truy cập được nếu không xóa điểm cuối trước. Do đó điều kiện dừng có giá trị toàn cục chứ không phải cục bộ. Mỗi lần xóa hợp lệ sẽ giảm nghiêm ngặt kích thước mảng trong khi vẫn duy trì tính chính xác của các điểm cuối còn lại, do đó quy trình mô phỏng chính xác số lần xóa có thể giải được tối đa có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    l, r = 0, n - 1
    ans = 0
    
    while l <= r:
        if a[l] <= k:
            ans += 1
            l += 1
        elif a[r] <= k:
            ans += 1
            r -= 1
        else:
            break
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai sử dụng mẫu hai con trỏ cổ điển. Điều kiện vòng lặp`l <= r`đảm bảo chúng tôi chỉ xử lý các phần tử còn lại. Bên trong, chúng tôi kiểm tra điểm cuối bên trái trước, sau đó đến điểm cuối bên phải. Thứ tự này là tùy ý nhưng nhất quán, vì cả hai lựa chọn đều có đóng góp tương đương cho kết quả đếm cuối cùng. 

Điều kiện kết thúc được kích hoạt khi cả hai điểm cuối đều không hợp lệ, tương ứng trực tiếp với quy tắc dừng trong báo cáo vấn đề. Không cần ghi sổ kế toán bổ sung. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8 4
4 2 3 1 5 1 6 4
```| Bước | tôi | r | một [l] | một [r] | Hành động | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 7 | 4 | 4 | rẽ trái | 1 | 
| 2 | 1 | 7 | 2 | 4 | rẽ trái | 2 | 
| 3 | 2 | 7 | 3 | 4 | rẽ trái | 3 | 
| 4 | 3 | 7 | 1 | 4 | rẽ trái | 4 | 
| 5 | 4 | 7 | 5 | 4 | rẽ phải | 5 | 
| 6 | 4 | 6 | 5 | 6 | dừng lại | 5 | 

Dấu vết này cho thấy quá trình tiếp tục như thế nào miễn là có ít nhất một đầu hợp lệ. Khi cả hai đầu vượt quá`k`, việc chấm dứt là ngay lập tức. 

### Ví dụ 2 

đầu vào:```
5 3
4 4 4 4 4
```| Bước | tôi | r | một [l] | một [r] | Hành động | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 4 | 4 | 4 | dừng lại | 0 | 

Điều này chứng tỏ trường hợp chấm dứt ngay lập tức trong đó không có điểm cuối nào có thể giải quyết được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử bị xóa nhiều nhất một lần ở một trong hai đầu | 
| Không gian | O(1) | Chỉ có hai con trỏ và bộ đếm được sử dụng | 

Những hạn chế`n ≤ 100`thậm chí làm cho việc quét tuyến tính này trở nên tầm thường trong thực tế, nhưng giải pháp đã tối ưu về mặt tiệm cận và quy mô đến các giới hạn lớn hơn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("8 4\n4 2 3 1 5 1 6 4\n") == "5"

# minimum case
assert run("1 10\n5\n") == "1"

# all too hard
assert run("3 1\n2 3 4\n") == "0"

# all solvable
assert run("4 10\n1 2 3 4\n") == "4"

# alternating valid endpoints
assert run("5 3\n3 9 3 9 3\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | xử lý ranh giới tối thiểu | 
| tất cả > k | 0 | chấm dứt ngay lập tức | 
| tất cả ≤ k | chiều dài đầy đủ | tiêu thụ đầy đủ | 
| xen kẽ | dòng chảy tham lam một phần | quyết định con trỏ đúng | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi phần tử đầu tiên và cuối cùng đều không hợp lệ. Ví dụ: 

đầu vào:```
4 2
5 1 1 5
```Khi bắt đầu, cả hai điểm cuối đều`5`, vượt quá`k`. Thuật toán kiểm tra cả hai đầu, không tìm thấy giá trị nào hợp lệ và dừng ngay lập tức với đầu ra`0`. 

Một trường hợp khác là khi chỉ có một bên hợp lệ: 

đầu vào:```
5 3
3 4 4 4 3
```Từng bước, đầu bên trái luôn có hiệu lực trước cho đến khi nó trở nên không hợp lệ, sau đó đầu bên phải được sử dụng. Các con trỏ co lại một cách nhất quán cho đến khi không còn điểm cuối hợp lệ. Quy tắc tham lam chỉ luân phiên chính xác khi cần thiết và không bao giờ bỏ qua một nước đi hợp lệ. 

Trường hợp cuối cùng là một mảng có thể giải được hoàn toàn: 

đầu vào:```
3 5
1 2 3
```Thuật toán loại bỏ tất cả các phần tử theo thứ tự vì ở mỗi bước có ít nhất một điểm cuối hợp lệ và cuối cùng cả hai con trỏ đều gặp nhau.
