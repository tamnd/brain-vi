---
title: "CF 105025C - \u0421\u043e\u043b\u043d\u0435\u0447\u043d\u0430\u044f \u043f\u0430\u043d\u0435\u043b\u044c"
description: "Chúng ta được cho một dãy các chiều cao đại diện cho các phần liên tiếp của mái nhà. Mỗi đoạn có một chiều cao cố định và chúng tôi muốn đặt một tấm pin mặt trời trên một khoảng liền kề của các đoạn này."
date: "2026-06-28T01:39:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105025
codeforces_index: "C"
codeforces_contest_name: "\u041e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u043e\u0439 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b \u00ab\u041c\u0430\u0448\u0438\u043d\u0430 \u0422\u044c\u044e\u0440\u0438\u043d\u0433\u0430\u00bb \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 105025
solve_time_s: 50
verified: true
draft: false
---

[CF 105025C - \u0421\u043e\u043b\u043d\u0435\u0447\u043d\u0430\u044f \u043f\u0430\u043d\u0435\u043b\u044c](https://codeforces.com/problemset/problem/105025/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy các chiều cao đại diện cho các phần liên tiếp của mái nhà. Mỗi đoạn có một chiều cao cố định và chúng tôi muốn đặt một tấm pin mặt trời trên một khoảng liền kề của các đoạn này. Hạn chế duy nhất là trong khoảng đã chọn, độ cao phải đơn điệu, hoàn toàn không giảm hoặc hoàn toàn không tăng. 

Nhiệm vụ là tìm khoảng thời gian liền kề dài nhất và đưa ra điểm cuối của nó. 

Kích thước đầu vào có thể lớn tới một triệu phần tử. Điều đó ngay lập tức loại trừ mọi cách tiếp cận kiểm tra tất cả các mảng con một cách rõ ràng. Quét bậc hai trên tất cả các cặp điểm cuối sẽ yêu cầu theo thứ tự 10^12 thao tác, vượt xa giới hạn thời gian. Ngay cả các phương pháp quét khối hoặc lồng nhau rõ ràng là không thể thực hiện được. 

Cần phải quét tuyến tính hoặc gần tuyến tính, nghĩa là chúng ta nên mong đợi một giải pháp xử lý mảng trong một lần duy nhất trong khi vẫn duy trì một số cấu trúc đơn điệu cục bộ. 

Trường hợp cạnh tinh tế phát sinh khi đoạn tốt nhất trải dài với các giá trị bằng nhau. Ví dụ: một mảng như 5 5 5 5 5 hợp lệ cả ở dạng không giảm và không tăng, do đó toàn bộ mảng phải được trả về. Bất kỳ giải pháp nào xử lý các phần tử bằng nhau một cách không nhất quán trong độ dốc sẽ thay đổi đều có nguy cơ phá vỡ trường hợp này. 

Một trường hợp cạnh khác là khi mảng được xen kẽ nghiêm ngặt, chẳng hạn như 1 3 2 4 3 5. Đoạn đơn điệu dài nhất khi đó có thể chỉ có độ dài 2 và giải pháp phải đặt lại chính xác ở mỗi lần thay đổi hướng. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử mọi cặp điểm cuối l và r, kiểm tra xem đoạn đó không giảm hay không tăng và theo dõi độ dài tối đa. Đối với mỗi phân đoạn, việc xác minh tính đơn điệu mất O(n) thời gian và có O(n^2) phân đoạn, cho kết quả tổng thể là O(n^3). Ngay cả khi chúng tôi tối ưu hóa việc kiểm tra bằng cách sử dụng so sánh tiền tố, chúng tôi vẫn kết thúc ở O(n^2), tốc độ này quá chậm đối với n lên tới 10^6. 

Nhận xét quan trọng là tính đơn điệu là một tính chất cục bộ. Khi một hướng được thiết lập, nó sẽ tiếp tục cho đến khi bị phá vỡ. Điều này có nghĩa là mảng có thể được phân tách thành các lần chạy đơn điệu tối đa và câu trả lời phải là một trong những lần chạy này hoặc sự kết hợp của hai lần chạy có chung một bước ngoặt. 

Chính xác hơn, nếu chúng ta xem mỗi vị trí như một “bước ngoặt” tiềm năng, chúng ta có thể mở rộng sang trái và phải trong khi vẫn duy trì trạng thái không giảm hoặc không tăng. Thay vì tính toán lại từ đầu cho mỗi l, chúng ta có thể sử dụng lại thông tin từ các hàng xóm và duy trì các phân đoạn đang chạy. 

Điều này dẫn đến một giải pháp quét tuyến tính trong đó chúng tôi theo dõi động hai loại phân đoạn đơn điệu: tăng và giảm. Chúng tôi gia hạn chúng càng lâu càng tốt và đặt lại khi điều kiện bị phá vỡ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) đến O(n^3) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai độ dài chạy khi duyệt mảng: độ dài hiện tại của đoạn không giảm kết thúc ở vị trí i và độ dài hiện tại của đoạn không tăng kết thúc ở vị trí i. Chúng tôi cũng theo dõi phân khúc tốt nhất được thấy cho đến nay.

1. Khởi tạo cả độ dài tăng và giảm thành 1 ở phần tử đầu tiên. Tại thời điểm này, bất kỳ yếu tố đơn lẻ nào đều có giá trị tầm thường. 
2. Lặp lại mảng từ trái sang phải bắt đầu từ chỉ số 1. 
3. Nếu chiều cao hiện tại lớn hơn hoặc bằng chiều cao trước đó, chúng ta có thể mở rộng đoạn không giảm. Ngược lại, chúng ta đặt lại nó về độ dài 1 vì tính đơn điệu bị phá vỡ. 
4. Nếu chiều cao hiện tại nhỏ hơn hoặc bằng chiều cao trước đó, chúng ta có thể mở rộng đoạn không tăng. Nếu không, chúng tôi đặt lại nó về độ dài 1. 
5. Sau khi cập nhật cả hai trạng thái, hãy so sánh kết thúc tốt nhất hiện tại tại i với câu trả lời đúng nhất toàn cầu. Nếu một trong hai hướng đơn điệu tạo ra một đoạn dài hơn, hãy cập nhật các điểm cuối tốt nhất được lưu trữ tương ứng. 
6. Tiếp tục cho đến hết mảng, luôn duy trì ranh giới phân đoạn chính xác bằng cách ghi nhớ mỗi lần chạy bắt đầu từ đâu. 

Chi tiết quan trọng là chúng ta phải theo dõi không chỉ độ dài mà còn cả chỉ số bắt đầu của mỗi lần chạy hiện tại, bởi vì câu trả lời yêu cầu điểm cuối chứ không chỉ độ dài. 

### Tại sao nó hoạt động 

Tại mọi vị trí i, bất kỳ mảng con đơn điệu hợp lệ nào kết thúc tại i đều phải là một phần của chuỗi không giảm tối đa kết thúc tại i hoặc chuỗi không tăng tối đa kết thúc tại i. Những lần chạy này được xác định duy nhất bằng so sánh cục bộ và không thể mở rộng hơn nữa mà không vi phạm tính đơn điệu. Vì mọi giải pháp hợp lệ đều được chứa trong một trong các lần chạy tối đa này, nên việc kiểm tra tất cả các lần chạy ngầm bao gồm tất cả các ứng cử viên. Thuật toán không bao giờ bỏ lỡ một ứng cử viên vì mỗi khoảng hợp lệ chính xác là hậu tố của một trong các phân đoạn đơn điệu được duy trì này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n_and_rest = input().split()
    if not n_and_rest:
        return
    n = int(n_and_rest[0])
    h = list(map(int, input().split()))
    
    if n == 1:
        print(1, 1)
        return

    best_len = 1
    best_l = 1
    best_r = 1

    inc_start = 0
    dec_start = 0
    inc_len = 1
    dec_len = 1

    for i in range(1, n):
        if h[i] >= h[i - 1]:
            inc_len += 1
        else:
            inc_len = 1
            inc_start = i

        if h[i] <= h[i - 1]:
            dec_len += 1
        else:
            dec_len = 1
            dec_start = i

        if inc_len > best_len:
            best_len = inc_len
            best_l = inc_start + 1
            best_r = i + 1

        if dec_len > best_len:
            best_len = dec_len
            best_l = dec_start + 1
            best_r = i + 1

    print(best_l, best_r)

if __name__ == "__main__":
    solve()
```Việc thực hiện giữ hai cửa sổ trượt độc lập. Cửa sổ tăng dần sẽ mở rộng khi phần tử tiếp theo không nhỏ hơn, nếu không nó sẽ đặt lại và bắt đầu ở chỉ mục hiện tại. Cửa sổ giảm hoạt động đối xứng. 

Chi tiết triển khai quan trọng là khi quá trình đặt lại xảy ra, chỉ mục bắt đầu được đặt ở vị trí hiện tại chứ không phải vị trí trước đó. Điều này đảm bảo tính chính xác cho các đảo ngược nghiêm ngặt như 1 3 2, trong đó đoạn này phải khởi động lại lần lượt ở 3 và 2. 

Chúng tôi cũng duy trì các chỉ mục tốt nhất ở dạng dựa trên 1, chỉ chuyển đổi từ lập chỉ mục dựa trên 0 tại thời điểm đầu ra. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
2 9 4 3 5
```Chúng tôi theo dõi cả hai hướng đơn điệu. 

| tôi | h[i] | inc_len | inc_start | dec_len | tháng mười hai | tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 1 | 0 | 1 | 0 | (1,1) | 
| 1 | 9 | 2 | 0 | 1 | 1 | (1,2) | 
| 2 | 4 | 1 | 2 | 2 | 1 | (2,3) | 
| 3 | 3 | 1 | 3 | 3 | 1 | (2,4) | 
| 4 | 5 | 2 | 3 | 1 | 4 | (2,4) | 

Phân đoạn giảm dần từ chỉ số 2 xuống 4 là phân đoạn hợp lệ dài nhất, cho phép lập chỉ mục 4 2 trong 1. 

Dấu vết này cho thấy cách đặt lại các phân đoạn khởi động lại một cách chính xác và mức tối đa toàn cầu có thể đến từ bất kỳ hướng nào. 

### Ví dụ 2 

đầu vào:```
4
1 2 3 4
```| tôi | h[i] | inc_len | inc_start | dec_len | tháng mười hai | tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | 1 | 0 | (1,1) | 
| 1 | 2 | 2 | 0 | 1 | 1 | (1,2) | 
| 2 | 3 | 3 | 0 | 1 | 2 | (1,3) | 
| 3 | 4 | 4 | 0 | 1 | 3 | (1,4) | 

Phân khúc tăng dần bao gồm toàn bộ mảng. 

Điều này xác nhận rằng thuật toán xử lý chính xác các chuỗi đơn điệu hoàn toàn mà không cần thiết lập lại không cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần với công việc liên tục | 
| Không gian | O(1) | Chỉ có một số bộ đếm và chỉ số được duy trì | 

Quét tuyến tính là đủ cho n tối đa 10^6 vì mỗi lần lặp chỉ thực hiện một số phép so sánh và phép gán không đổi, trong giới hạn thời gian thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()  # placeholder if solve prints directly

# Since solve() prints, redefine properly
def run(inp: str) -> str:
    import sys, io
    backup = sys.stdin
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    sys.stdin = backup
    return out.getvalue().strip()

# provided sample
assert run("5\n2 9 4 3 5\n") == "2 4"

# all increasing
assert run("4\n1 2 3 4\n") == "1 4"

# all decreasing
assert run("4\n4 3 2 1\n") == "1 4"

# all equal
assert run("5\n7 7 7 7 7\n") == "1 5"

# single element
assert run("1\n10\n") == "1 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 2 9 4 3 5 | 2 4 | chạy đơn điệu hỗn hợp | 
| 1 2 3 4 | 1 4 | trường hợp tăng đầy đủ | 
| 4 4 3 2 1 | 1 4 | trường hợp giảm đầy đủ | 
| 5 7 7 7 7 7 | 1 5 | xử lý các phần tử bằng nhau | 
| 1 10 | 1 1 | kích thước tối thiểu | 

## Vỏ cạnh 

Đối với trường hợp mảng không đổi như 7 7 7 7 7, cả hai điều kiện đơn điệu đều luôn đúng. Thuật toán tiếp tục mở rộng cả inc_len và dec_len liên tục, không bao giờ kích hoạt thiết lập lại. Phân đoạn tốt nhất được cập nhật ở chỉ mục cuối cùng để bao trùm toàn bộ phạm vi, tạo ra 1 5 chính xác. 

Đối với một đường ngoằn ngoèo sắc nét chẳng hạn như 1 3 2 4, đoạn tăng sẽ đặt lại ở mức 2 và 4, trong khi đoạn giảm đặt lại ở mức 3. Lần chạy hợp lệ dài nhất là 1 3 hoặc 3 4 tùy theo hướng và thuật toán xác định chính xác độ dài 2 đoạn là tối ưu. 

Đối với đầu vào giảm đơn điệu nghiêm ngặt như 5 4 3 2, bộ đếm giảm tăng dần ở mỗi bước mà không bị gián đoạn. Bộ đếm tăng sẽ đặt lại ở mỗi bước nhưng không bao giờ ảnh hưởng đến câu trả lời cuối cùng, vì lượt giảm chiếm ưu thế.
