---
title: "CF 992A - Nastya và một mảng"
description: "Chúng ta được cung cấp một danh sách các số nguyên và chúng ta được phép áp dụng nhiều lần một thao tác thay đổi mảng theo một cách rất cụ thể: trong một lần di chuyển, chúng ta chọn một giá trị số nguyên và thêm nó vào mọi phần tử hiện khác 0. Số không không bị ảnh hưởng trong quá trình hoạt động đó."
date: "2026-06-17T00:25:35+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 992
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 489 (Div. 2)"
rating: 800
weight: 992
solve_time_s: 68
verified: true
draft: false
---

[CF 992A - Nastya và một mảng](https://codeforces.com/problemset/problem/992/A) 

**Đánh giá:** 800 
**Tags:** triển khai, sắp xếp 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên và chúng ta được phép áp dụng nhiều lần một thao tác thay đổi mảng theo một cách rất cụ thể: trong một lần di chuyển, chúng ta chọn một giá trị số nguyên và thêm nó vào mọi phần tử hiện khác 0. Số không không bị ảnh hưởng trong quá trình hoạt động đó. Quá trình kết thúc khi tất cả các phần tử đồng thời trở về 0 và nhiệm vụ là giảm thiểu số lượng thao tác cần thiết để đạt được trạng thái đó. 

Điểm mấu chốt là mỗi thao tác hoạt động thống nhất trên tất cả các phần tử khác 0, vì vậy chúng tôi không chỉnh sửa các phần tử một cách độc lập. Thay vào đó, cấu trúc của mảng phát triển hàng loạt và các giá trị 0 hoạt động giống như “các điểm cố định” ngừng tham gia vào các bản cập nhật trong tương lai. 

Ràng buộc n lên tới 10^5 buộc mọi giải pháp phải chạy trong thời gian tuyến tính hoặc gần tuyến tính. Bất kỳ cách tiếp cận nào mô phỏng mảng qua nhiều thao tác, đặc biệt nếu nó liên tục quét hoặc cập nhật các phần tử, sẽ có nguy cơ xảy ra hành vi O(n^2) và sẽ không vượt qua. 

Trường hợp cạnh tinh vi phát sinh khi mảng chứa nhiều số 0 trộn lẫn với các số khác không. Ví dụ: trong một mảng như`[0, 5, 0, -2]`, các số 0 đóng vai trò là các dải phân cách vĩnh viễn và chỉ các phân đoạn khác 0 mới có thể bị ảnh hưởng. Một ý tưởng ngây thơ có thể cố gắng bình thường hóa tất cả các giá trị trong một lần, nhưng vì các số 0 bị loại khỏi các bản cập nhật nên chúng ngăn cản sự đồng bộ hóa toàn cầu và buộc phải lý luận riêng cho mỗi phân đoạn. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực cố gắng mô phỏng quá trình một cách trực tiếp. Trong mỗi giây, chúng tôi chọn một giá trị và áp dụng nó cho tất cả các phần tử khác 0, sau đó kiểm tra xem mọi thứ có trở thành 0 hay không. Nếu không, chúng tôi lặp lại. Mặc dù điều này rất đơn giản nhưng mỗi thao tác vẫn yêu cầu quét mảng để xác định các phần tử khác 0 và cập nhật chúng. Trong trường hợp xấu nhất, chúng ta có thể cần một số thao tác tỷ lệ thuận với số lượng “pha” riêng biệt cần thiết để loại bỏ tất cả các giá trị và mỗi pha có giá O(n). Điều này dẫn đến hành vi O(n^2) trong các trường hợp đối nghịch, quá chậm đối với n = 10^5. 

Quan sát quan trọng là các số 0 phân chia mảng thành các phân đoạn độc lập về mức độ ảnh hưởng. Khi một phần tử trở thành 0, nó sẽ ngừng thay đổi mãi mãi. Điều này có nghĩa là sự phát triển của mảng được thúc đẩy hoàn toàn bởi số lần chúng ta cần "hoàn thành" các khối liền kề riêng biệt có giá trị khác 0 được phân tách bằng số 0. 

Mỗi lần chúng tôi áp dụng một thao tác, chúng tôi sẽ giảm cùng nhau tất cả các giá trị đang hoạt động (khác 0) một cách hiệu quả và bất cứ khi nào một phân đoạn chứa các dấu hỗn hợp hoặc độ lớn khác nhau, thì cần phải giảm nhiều lần trước khi phân đoạn đó thu gọn hoàn toàn. Tuy nhiên, vì chúng ta có thể chọn bất kỳ số nguyên nào mỗi lần, chiến lược tối ưu là loại bỏ các đóng góp theo cách thu gọn từng phân đoạn khác 0 liền kề tối đa một cách độc lập và các phân đoạn này có thể được xử lý song song trên các hoạt động. 

Điều này làm giảm vấn đề về số lượng cấu trúc: mọi phân đoạn liền kề tối đa của các phần tử khác 0 đóng góp chính xác một đơn vị thời gian cho câu trả lời. Theo trực giác, mỗi phân đoạn yêu cầu ít nhất một “giai đoạn loại bỏ” riêng biệt không thể chia sẻ giữa các phân đoạn riêng biệt vì các số 0 chặn tương tác giữa chúng. 

Vì vậy, câu trả lời chỉ đơn giản là số khối liền kề của các phần tử khác 0 trong mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Đếm các phân đoạn khác 0 | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi quét mảng từ trái sang phải và đếm số lần chúng tôi nhập một đoạn khác 0. 

1. Khởi tạo bộ đếm về 0. Điều này sẽ lưu trữ số khối khác 0 liền kề. 
2. Duyệt mảng từ chỉ số 0 đến n − 1. 
3. Khi chúng tôi tìm thấy một phần tử khác 0 và nó là phần tử đầu tiên hoặc phần tử trước đó bằng 0, chúng tôi sẽ tăng bộ đếm. Điều này xác định sự bắt đầu của một phân khúc mới. 
4. Tiếp tục quét mà không tăng dần cho đến khi chúng ta rời khỏi đoạn đó (tức là gặp số 0 hoặc đến cuối). 
5. Sau khi kết thúc quá trình duyệt, trả lại bộ đếm. 

Lý do đằng sau bước 3 là chỉ có sự chuyển đổi từ 0 sang khác 0 mới đánh dấu sự khởi đầu của một nhóm độc lập mới. Các phần tử bên trong của một phân đoạn không quan trọng vì chúng đều được xử lý cùng nhau theo cùng một chuỗi hoạt động. 

### Tại sao nó hoạt động 

Hoạt động này chỉ có thể ảnh hưởng đồng thời đến các phần tử khác 0 và các số 0 sẽ chia mảng vĩnh viễn thành các vùng bị cô lập. Trong một vùng liền kề có các giá trị khác 0, tất cả các phần tử luôn được cập nhật cùng nhau cho đến khi toàn bộ vùng bị thu gọn. Không có thao tác nào có thể hợp nhất hai vùng riêng biệt vì số 0 không bao giờ thay đổi. Do đó, mỗi phân đoạn khác 0 liền kề hoạt động giống như một đơn vị độc lập yêu cầu chính xác một “giai đoạn sụp đổ” hiệu quả và số lượng các giai đoạn đó chính xác là số lượng phân đoạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    ans = 0
    for i in range(n):
        if a[i] != 0 and (i == 0 or a[i-1] == 0):
            ans += 1
    
    print(ans)

if __name__ == "__main__":
    solve()
```Mã này tuân theo logic truyền tải trực tiếp. điều kiện`a[i] != 0`đảm bảo chúng tôi chỉ xem xét các yếu tố hoạt động, trong khi`(i == 0 or a[i-1] == 0)`đảm bảo chúng tôi chỉ đếm phần tử đầu tiên của mỗi khối khác 0. Điều này tránh việc tính hai lần trong một phân khúc và đảm bảo mỗi phân khúc đóng góp chính xác một lần. 

Giải pháp này hoàn toàn có tính chất phát trực tuyến, chỉ cần một lần truyền và bộ nhớ bổ sung liên tục. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 1 1 1 1
```Chúng tôi theo dõi việc bắt đầu phân đoạn: 

| tôi | một [tôi] | a[i-1] | Phân khúc mới? | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | - | vâng | 1 | 
| 1 | 1 | 1 | không | 1 | 
| 2 | 1 | 1 | không | 1 | 
| 3 | 1 | 1 | không | 1 | 
| 4 | 1 | 1 | không | 1 | 

Đầu ra là 1, vì toàn bộ mảng là một khối khác 0 liên tục. 

Điều này xác nhận rằng một mảng dày đặc chỉ cần một giai đoạn thu gọn. 

### Ví dụ 2 

đầu vào:```
5
1 0 2 3 0
```| tôi | một [tôi] | a[i-1] | Phân khúc mới? | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | - | vâng | 1 | 
| 1 | 0 | 1 | không | 1 | 
| 2 | 2 | 0 | vâng | 2 | 
| 3 | 3 | 2 | không | 2 | 
| 4 | 0 | 3 | không | 2 | 

Đầu ra là 2. 

Điều này chứng tỏ rằng các số 0 chia mảng thành các vùng độc lập và mỗi vùng đóng góp riêng cho câu trả lời cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Truyền một lần qua mảng, công việc không đổi trên mỗi phần tử | 
| Không gian | O(1) | Chỉ có một bộ đếm được lưu trữ bên cạnh đầu vào | 

Quét tuyến tính đủ cho n lên đến 10^5, dễ dàng phù hợp với giới hạn thời gian thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as _io

    out = _io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("5\n1 1 1 1 1\n") == "1"

# single element zero
assert run("1\n0\n") == "0"

# single element non-zero
assert run("1\n7\n") == "1"

# alternating zeros
assert run("5\n1 0 2 0 3\n") == "3"

# all zeros
assert run("4\n0 0 0 0\n") == "0"

# one big block
assert run("6\n-1 -2 -3 -4 -5 -6\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n0\n`| 0 | trường hợp cạnh tối thiểu | 
|`1\n7\n`| 1 | phần tử đơn khác 0 | 
|`1 0 2 0 3`| 3 | nhiều phân đoạn riêng biệt | 
|`0 0 0 0`| 0 | trường hợp tất cả số không | 
|`-1 -2 -3 -4 -5 -6`| 1 | ký không liên quan | 

## Vỏ cạnh 

Đối với một đầu vào như`1 0 2 0 3`, quá trình quét sẽ phát hiện một phân đoạn mới ở mỗi phân đoạn khác 0 sau số 0. Phần tử đầu tiên bắt đầu một phân đoạn, chỉ mục 2 bắt đầu một phân đoạn khác sau số 0 và chỉ mục 4 bắt đầu phân đoạn thứ ba. Mỗi cái được tính chính xác một lần vì điều kiện chuyển tiếp chỉ kích hoạt ở các ranh giới từ 0 đến khác 0. 

Đối với một đầu vào như`0 0 0`, không có phần tử nào thỏa mãn`a[i] != 0`, do đó bộ đếm vẫn bằng 0 trong suốt quá trình. Điều này phù hợp với thực tế là mảng đã ở trạng thái cuối. 

Đối với một mảng âm hoàn toàn như`-5 -4 -3`, dấu hiệu không liên quan vì thuật toán chỉ theo dõi phân đoạn cấu trúc chứ không theo dõi giá trị. Toàn bộ mảng là một khối khác 0 liền kề, vì vậy câu trả lời là 1.
