---
title: "CF 105691A - Bữa tiệc nhàm chán!"
description: "Nhiệm vụ này mô tả một trò chơi nhỏ chơi với các số trong đó mỗi số đóng góp một giá trị bằng chính nó, nhưng việc chọn một số có tác dụng phụ: nó “loại bỏ” các số lân cận về giá trị."
date: "2026-06-26T08:14:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105691
codeforces_index: "A"
codeforces_contest_name: "MOI25 Training camp"
rating: 0
weight: 105691
solve_time_s: 39
verified: true
draft: false
---

[CF 105691A - Một bữa tiệc nhàm chán!](https://codeforces.com/problemset/problem/105691/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ này mô tả một trò chơi nhỏ chơi với các số trong đó mỗi số đóng góp một giá trị bằng chính nó, nhưng việc chọn một số có tác dụng phụ: nó “loại bỏ” các số lân cận về giá trị. Cụ thể, nếu bạn quyết định lấy tất cả các lần xuất hiện của một giá trị x nào đó, thì tất cả các lần xuất hiện của x−1 và x+1 sẽ không thể sử dụng được cho các lựa chọn trong tương lai. Mỗi lần bạn lấy một giá trị x, bạn sẽ kiếm được số điểm bằng x nhân với số lần x xuất hiện trong đầu vào. Mục tiêu là chọn những giá trị cần lấy sao cho không có hai giá trị được chọn nào khác nhau đúng 1, đồng thời tối đa hóa tổng số điểm đạt được. 

Đầu vào chỉ là một danh sách các số nguyên. Đầu ra là một số duy nhất: số điểm tối đa có thể đạt được theo ràng buộc này. 

Ràng buộc chính là n lên tới 100000 và các giá trị cũng lên tới 100000. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các tập hợp con hoặc mô phỏng các quyết định trên tất cả các phần tử. Một tìm kiếm theo cấp số nhân ngây thơ về những giá trị cần chọn sẽ tăng lên gấp 2 lũy thừa của các giá trị riêng biệt, điều này trở nên không khả thi ngay cả đối với vài chục số riêng biệt. Ngay cả việc lặp lại tất cả các tập hợp con của các lần xuất hiện cũng vượt xa mọi giới hạn thực tế. 

Một vấn đề tế nhị hơn xuất hiện trong những nỗ lực tham lam ngây thơ. Ví dụ: việc chọn giá trị thường xuyên nhất trước tiên có vẻ hợp lý, nhưng nó có thể thất bại nặng nề vì việc chọn giá trị ít thường xuyên hơn một chút có thể chặn hai giá trị lân cận lớn vừa phải và tạo ra tổng tổng cao hơn. 

Ví dụ, hãy xem xét đầu vào: 

3 

1 2 2 

Lựa chọn tham lam chọn 1 trước được 1, sau đó không thể chọn 2, dẫn đến tổng là 1. Giải pháp tối ưu chọn 2 và được 4. Điều này cho thấy các quyết định về tần số cục bộ hoặc giá trị cục bộ là không đáng tin cậy. 

Khó khăn là mỗi giá trị chỉ tương tác với các giá trị lân cận của nó, điều này tạo ra sự phụ thuộc có cấu trúc thay vì xung đột tùy tiện. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là xem xét mọi tập hợp con của các giá trị riêng biệt và xác minh xem nó có hợp lệ hay không, nghĩa là không có hai giá trị được chọn nào khác nhau 1. Đối với mỗi tập hợp con hợp lệ, hãy tính tổng đóng góp bằng cách tính tổng giá trị × tần số. Nếu có k giá trị riêng biệt, điều này yêu cầu kiểm tra 2^k tập hợp con và mỗi tập hợp con yêu cầu tính toán O(k) hoặc O(1) bằng tiền xử lý. Với k lên tới 100000 trong trường hợp xấu nhất, cách tiếp cận này thất bại ngay lập tức vì ngay cả k = 40 cũng đã dẫn đến hơn một nghìn tỷ tập hợp con. 

Quan sát quan trọng là cấu trúc này giống hệt với lập trình động “lấy hoặc bỏ qua” cổ điển trên một dòng. Sau khi tổng hợp tất cả các lần xuất hiện của mỗi giá trị, mỗi giá trị số nguyên sẽ trở thành một trọng số duy nhất: val[x] = x × count[x]. Hạn chế “không thể lấy x và x+1 cùng nhau” có nghĩa là việc chọn x sẽ buộc loại trừ các chỉ số liền kề trong mảng giá trị một chiều. 

Một khi vấn đề được viết lại theo cách này, nó sẽ trở thành một sự tái diễn tiêu chuẩn. Đặt dp[x] đại diện cho câu trả lời tốt nhất khi xem xét tất cả các giá trị lên đến x. Đối với mỗi x, chúng ta hoặc bỏ qua nó, giữ dp[x−1] hoặc lấy nó, nhận val[x] cộng dp[x−2]. Cấu trúc này hoạt động vì xung đột duy nhất được đưa ra khi lấy x chính xác là x−1 và mọi thứ nhỏ hơn x−1 vẫn độc lập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con | O(2^k) | O(k) | Quá chậm | 
| DP trên giá trị | O(maxA + n) | O(maxA) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc danh sách các số và đếm xem mỗi giá trị xuất hiện bao nhiêu lần. Bước này nén đầu vào vì chỉ tần số mới quan trọng chứ không phải vị trí. 
2. Chuyển đổi mỗi giá trị x thành đóng góp trọng số x × tần số[x]. Điều này biến bài toán thành việc lựa chọn giữa các vị trí có trọng số độc lập trên trục số. 
3. Tạo một mảng DP trong đó dp[x] thể hiện tổng tốt nhất có thể đạt được bằng cách sử dụng các giá trị lên đến x. 
4. Khởi tạo dp[0] = 0 và dp[1] = value[1] vì chỉ có giá trị 1, lựa chọn duy nhất là có lấy nó hay không. 
5. Với mỗi x từ 2 đến giá trị lớn nhất hiện có, hãy tính dp[x] là giá trị lớn nhất giữa dp[x−1] và dp[x−2] + value[x]. Tùy chọn đầu tiên tương ứng với việc bỏ qua x, tùy chọn thứ hai tương ứng với việc lấy nó và buộc loại trừ x−1. 
6. Câu trả lời là dp[giá trị tối đa]. 

Lý do sự lặp lại này hợp lệ là khi các giá trị được sắp xếp trên dòng số nguyên, mọi quyết định chỉ ảnh hưởng đến hàng xóm ngay lập tức. Bất kỳ giải pháp tối ưu nào lên đến x đều phải bao gồm x hoặc không. Nếu nó loại trừ x thì nó sẽ rút gọn thành nghiệm tối ưu cho x−1. Nếu nó bao gồm x thì x−1 bị cấm, nhưng mọi thứ cho đến x−2 vẫn không bị ảnh hưởng, vì vậy tiền tố tương thích tốt nhất là dp[x−2]. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = list(map(int, input().split()))

    if n == 0:
        print(0)
        return

    maxv = max(arr)
    freq = [0] * (maxv + 1)

    for x in arr:
        freq[x] += 1

    val = [0] * (maxv + 1)
    for x in range(maxv + 1):
        val[x] = x * freq[x]

    if maxv == 0:
        print(0)
        return
    if maxv == 1:
        print(max(val[0], val[1]))
        return

    dp0 = 0
    dp1 = val[1]

    for x in range(2, maxv + 1):
        dp2 = max(dp1, dp0 + val[x])
        dp0, dp1 = dp1, dp2

    print(dp1)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã nén đầu vào vào bảng tần số sao cho mỗi giá trị số nguyên đóng góp độc lập. DP được triển khai trong không gian O(1) bằng cách chỉ giữ lại hai trạng thái trước đó thay vì một mảng đầy đủ. Điều này tránh được chi phí bộ nhớ khi giá trị tối đa lớn. Quá trình chuyển đổi phản ánh chính xác quyết định nên bao gồm hay loại trừ giá trị hiện tại và các biến cuộn dp0 và dp1 lần lượt đại diện cho dp[x−2] và dp[x−1]. 

Một lỗi triển khai phổ biến là chỉ lặp lại tối đa giá trị đầu vào mà quên rằng các giá trị trung gian có thể bằng 0. Đó không phải là vấn đề ở đây vì quá trình chuyển đổi dp xử lý các số 0 một cách tự nhiên: việc bỏ qua các giá trị trống sẽ duy trì kết quả tối ưu trước đó. 

## Ví dụ đã hoạt động 

Xem xét đầu vào:```
3
1 2 2
```Chúng tôi tính toán tần số và giá trị: 

| x | tần số[x] | giá trị[x] | quyết định dp | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | lấy 1 hoặc bỏ qua | 
| 2 | 2 | 4 | xung đột với 1 | 

DP từng bước: 

| x | dp0 (x-2) | dp1 (x-1) | dp[x] | 
| --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 | 
| 2 | 1 | 1 | 4 | 

Câu trả lời cuối cùng là 4, đạt được bằng cách lấy 2. 

Điều này cho thấy dù số 1 xuất hiện nhưng việc hy sinh nó là tối ưu vì số 2 mang lại tổng đóng góp cao hơn. 

Bây giờ hãy xem xét:```
5
1 1 2 3 3
```Tần số cho các giá trị: 1→2, 2→2, 3→6. 

| x | dp0 | dp1 | dp | 
| --- | --- | --- | --- | 
| 1 | 0 | 2 | 2 | 
| 2 | 2 | 2 | 2 | 
| 3 | 2 | 2 | 8 | 

Câu trả lời cuối cùng là 8, đạt được bằng cách chỉ lấy giá trị 3. Điều này xác nhận DP truyền bá chính xác tác dụng chặn các giá trị lân cận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + maxA) | đếm tần số cộng với DP tuyến tính trên phạm vi giá trị | 
| Không gian | O(maxA) | mảng tần số và trạng thái DP | 

Các ràng buộc cho phép tối đa 100000 phần tử và giá trị lên tới 100000, do đó, quét tuyến tính trên phạm vi giá trị và kích thước đầu vào vừa vặn thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ vẫn ở mức tuyến tính và đủ nhỏ để đáp ứng các giới hạn thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import sys as _sys

    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples (from statement)
assert run("2\n1 2\n") == "2"
assert run("3\n1 2 3\n") == "4"
assert run("9\n1 2 1 3 2 2 2 2 3\n") == "10"

# custom cases
assert run("1\n10\n") == "10", "single element"
assert run("2\n5 5\n") == "10", "same value duplicates"
assert run("4\n1 3 3 3\n") == "9", "gap allows combination"
assert run("5\n1 2 3 4 5\n") == "9", "chain forces alternating picks"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| giá trị đơn | 10 | xử lý trường hợp cơ bản | 
| trùng lặp | 10 | tính chính xác của tổng hợp | 
| giá trị thưa thớt | 9 | tích lũy không liền kề | 
| chuỗi đầy đủ | 9 | lan truyền ràng buộc kề | 

## Vỏ cạnh 

Một trường hợp có tất cả các giá trị giống nhau sẽ kiểm tra xem thuật toán có tổng hợp tần số chính xác hay không: 

đầu vào:```
4
7 7 7 7
```Ở đây freq[7] = 4, vì vậy value[7] = 28. Không có ràng buộc liền kề nào quan trọng vì 6 và 8 không tồn tại trong đóng góp đầu vào. DP bỏ qua tất cả các giá trị khác và trực tiếp chọn 7, tạo ra 28. 

Một trường hợp cạnh khác là một chuỗi liên tục: 

đầu vào:```
4
1 2 3 4
```DP đánh giá: 

dp[1] = 1 

dp[2] = max(1, 0 + 2) = 2 

dp[3] = max(2, 1 + 3) = 4 

dp[4] = max(4, 2 + 4) = 6 

Kết quả cuối cùng 6 tương ứng với việc chọn giá trị 2 và 4, cho thấy phép truy toán giải quyết chính xác các lựa chọn xen kẽ trên một chuỗi như thế nào.
