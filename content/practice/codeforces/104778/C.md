---
title: "CF 104778C - \u0414\u0432\u0435 \u043f\u043e\u0441\u043b\u0435\u0434\u043e\u0432\u0430\u0442\u0435\u043b\u044c\u043d\u043e\u0441\u0442\u0438"
description: "Chúng ta được cho hai mảng số nguyên có độ dài bằng nhau. Mỗi vị trí i xác định một “khoảng ràng buộc”, nhưng khoảng đó không có thứ tự: phạm vi hợp lệ cho số nguyên ứng cử viên x tại chỉ số i chỉ đơn giản là đoạn giữa ai và bi, bất kể số nào lớn hơn."
date: "2026-06-28T15:05:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104778
codeforces_index: "C"
codeforces_contest_name: "2023-2024 \u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e, \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 (\u0412\u041a\u041e\u0428\u041f 23, \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u0438\u0439 \u043e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u044d\u0442\u0430\u043f)"
rating: 0
weight: 104778
solve_time_s: 43
verified: true
draft: false
---

[CF 104778C - \u0414\u0432\u0435 \u043f\u043e\u0441\u043b\u0435\u0434\u043e\u0432\u0430\u0442\u0435\u043b\u044c\u 043d\u043e\u0441\u0442\u0438](https://codeforces.com/problemset/problem/104778/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai mảng số nguyên có độ dài bằng nhau. Mỗi vị trí i xác định một “khoảng ràng buộc”, nhưng khoảng đó không có thứ tự: phạm vi hợp lệ cho số nguyên ứng cử viên x tại chỉ số i chỉ đơn giản là đoạn giữa ai và bi, bất kể số nào lớn hơn. Vì vậy, mỗi cặp đóng góp một khoảng đóng trên trục số. 

Nhiệm vụ là đếm xem có bao nhiêu số nguyên x thỏa mãn tất cả các ràng buộc cùng một lúc, nghĩa là x phải nằm bên trong mỗi một trong n khoảng này. Nói cách khác, chúng ta đang tìm số điểm nguyên trong giao điểm của tất cả các đoạn được xác định bởi (ai, bi). 

Các ràng buộc lớn, với n lên tới 200000 và giá trị lên tới 1e9. Điều này loại trừ mọi cách tiếp cận thử mọi x có thể hoặc xây dựng phạm vi rõ ràng. Không thể quét toàn bộ miền giá trị vì kích thước phạm vi quá lớn. Hướng khả thi duy nhất là nén vấn đề xuống một lượng thông tin không đổi trong mỗi khoảng thời gian. 

Một sai lầm ngây thơ là cố gắng xây dựng một tập hợp toàn cầu các giá trị x hợp lệ bằng cách kiểm tra từng khoảng một và lọc các ứng cử viên. Ngay cả khi bắt đầu từ một khoảng đơn và các tập hợp giao nhau lặp đi lặp lại sẽ bùng nổ, vì giao điểm của nhiều khoảng nguyên lớn không thể được biểu diễn rõ ràng dưới dạng tập hợp các điểm riêng lẻ khi phạm vi lớn. 

Cạm bẫy thứ hai là quên rằng mỗi cặp không xác định hướng, vì vậy (ai, bi) và (bi, ai) là các ràng buộc giống hệt nhau. Việc coi chúng như các khoảng định hướng sẽ dẫn đến giới hạn giao nhau không chính xác. 

## Phương pháp tiếp cận 

Nếu thử dùng vũ lực, chúng ta có thể tưởng tượng việc lặp qua mọi số nguyên x từ 1 đến 1e9 và kiểm tra xem nó có thỏa mãn tất cả n khoảng hay không. Mỗi lần kiểm tra có chi phí O(n), vì vậy trường hợp xấu nhất là O(1e9 * n), điều này hoàn toàn không khả thi. 

Quan sát quan trọng là mỗi ràng buộc là một khoảng đóng tiêu chuẩn. Tập hợp tất cả x hợp lệ chính xác là giao điểm của các khoảng này. Giao điểm của nhiều khoảng trên một đường cũng là một khoảng duy nhất, có thể trống. Điều đó có nghĩa là chúng tôi không cần theo dõi nhiều ứng viên, chúng tôi chỉ cần sự chồng chéo toàn cầu. 

Mỗi khoảng [min(ai, bi), max(ai, bi)] đóng góp một giới hạn dưới và một giới hạn trên. Giao điểm của tất cả các khoảng có được bằng cách lấy mức tối đa của tất cả các giới hạn dưới và mức tối thiểu của tất cả các giới hạn trên. Nếu điểm cuối bên trái thu được lớn hơn điểm cuối bên phải thì giao điểm trống. Mặt khác, mọi số nguyên giữa chúng đều hợp lệ và chúng tôi đếm chúng trực tiếp. 

Điều này làm giảm vấn đề từ việc quản lý n khoảng thời gian đến việc duy trì hai giá trị đang chạy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · 1e9) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì giao điểm của tất cả các khoảng trong khi quét đầu vào một lần.

1. Với mỗi cặp (ai, bi), hãy chuyển đổi nó thành một khoảng thích hợp [l, r] trong đó l = min(ai, bi) và r = max(ai, bi). Bước chuẩn hóa này là cần thiết vì các điểm cuối không có thứ tự. 
2. Khởi tạo giao điểm tổng thể dưới dạng một khoảng chứa tất cả các số nguyên: giới hạn trái là rất nhỏ, giới hạn phải là rất lớn. Điều này đảm bảo khoảng thời gian đầu tiên xác định đầy đủ giới hạn ban đầu. 
3. Đối với mỗi khoảng [l, r], hãy cập nhật giao điểm bằng cách đặt giới hạn trái mới thành max(current_left, l). Điều này chỉ giữ các giá trị hợp lệ trong mọi khoảng thời gian được thấy cho đến nay. 
4. Tương tự, cập nhật giới hạn bên phải thành min(current_right, r). Điều này đảm bảo chúng tôi loại bỏ các giá trị vượt quá mọi ràng buộc. 
5. Sau khi xử lý tất cả các khoảng, hãy kiểm tra xem giới hạn bên trái thu được có lớn hơn giới hạn bên phải hay không. Nếu vậy thì không có số nguyên nào thỏa mãn mọi ràng buộc. 
6. Ngược lại, câu trả lời là phải − left + 1, vì mọi số nguyên trong khoảng đóng này đều hợp lệ. 

### Tại sao nó hoạt động 

Mỗi khoảng xác định một tập hợp các số nguyên được phép. Giao của các tập hợp được xác định bởi các khoảng trên một đường thẳng chính là một khoảng. Ở mỗi bước, phạm vi được duy trì chính xác bằng giao điểm của tất cả các khoảng được xử lý cho đến nay. Các bản cập nhật bảo toàn thuộc tính này vì bất kỳ số nguyên nào nằm ngoài khoảng mới đều phải vi phạm ít nhất một ràng buộc, trong khi bất kỳ số nguyên nào bên trong cả giao điểm trước đó và khoảng mới đều thỏa mãn tất cả các ràng buộc đã thấy cho đến nay. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    left = -10**18
    right = 10**18
    
    for i in range(n):
        l = min(a[i], b[i])
        r = max(a[i], b[i])
        left = max(left, l)
        right = min(right, r)
    
    if left > right:
        print(0)
    else:
        print(right - left + 1)

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp thuật toán. Sự lựa chọn thiết kế tinh tế duy nhất là sử dụng các điểm canh gác đủ lớn cho các ranh giới giao lộ ban đầu. Vì giá trị đầu vào lên tới 1e9 nên việc sử dụng ±1e18 một cách an toàn sẽ tránh được việc vô tình cắt bớt trước lần cập nhật đầu tiên. Phần còn lại của logic là một giao lộ lăn đơn giản. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 4 

a = [5, 5, 5, 5] 

b = [5, 5, 5, 5] 

Mỗi khoảng là [5, 5], do đó giao điểm vẫn giữ nguyên [5, 5] xuyên suốt. 

| Bước | Khoảng thời gian | Trái | Đúng | 
| --- | --- | --- | --- | 
| 1 | [5, 5] | 5 | 5 | 
| 2 | [5, 5] | 5 | 5 | 
| 3 | [5, 5] | 5 | 5 | 
| 4 | [5, 5] | 5 | 5 | 

Câu trả lời là 1. Điều này xác nhận thuật toán xử lý chính xác các khoảng suy biến. 

### Ví dụ 2 

đầu vào: 

n = 3 

a = [3, 7, 10] 

b = [6, 8, 12] 

Các khoảng là [3,6], [7,8], [10,12]. Những điều này không chồng chéo lên nhau. 

| Bước | Khoảng thời gian | Trái | Đúng | 
| --- | --- | --- | --- | 
| 1 | [3,6] | 3 | 6 | 
| 2 | [7,8] | 7 | 6 | 
| 3 | [10,12] | 10 | 6 | 

Sau bước 2, trái > phải, do đó giao điểm trở nên trống và vẫn trống. 

Câu trả lời là 0. Điều này chứng tỏ tính khả thi đã sớm sụp đổ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | một lần vượt qua tất cả các khoảng thời gian với các cập nhật liên tục | 
| Không gian | O(1) | chỉ có hai giới hạn đang chạy được lưu trữ | 

Quét tuyến tính trong khoảng thời gian lên tới 200000 dễ dàng nằm trong giới hạn và không cần cấu trúc bổ sung, giúp giải pháp hiệu quả cả về bộ nhớ và thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    left = -10**18
    right = 10**18
    
    for i in range(n):
        l = min(a[i], b[i])
        r = max(a[i], b[i])
        left = max(left, l)
        right = min(right, r)
    
    if left > right:
        return "0\n"
    return str(right - left + 1) + "\n"

# provided samples
assert run("4\n5 5 5 5\n5 5 5 5\n") == "1\n"
assert run("3\n3 7 10\n6 8 12\n") == "0\n"

# custom cases
assert run("1\n10\n10\n") == "1\n"  # single point
assert run("2\n1 100\n50 60\n") == "11\n"  # overlapping interval
assert run("3\n1 2 3\n10 20 30\n") == "0\n"  # disjoint everywhere
assert run("2\n5 1\n1 5\n") == "5\n"  # reversed intervals
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bình đẳng 1 điểm | 1 | khoảng thời gian hợp lệ tối thiểu | 
| chồng chéo tầm trung | 11 | kích thước giao lộ chính xác | 
| hoàn toàn rời rạc | 0 | xử lý ngã tư trống | 
| điểm cuối đảo ngược | 5 | tính đúng theo hoán đổi ai, bi | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các khoảng đều là các điểm đơn giống hệt nhau. Đối với đầu vào n = 3 với (ai, bi) = (5, 5) được lặp lại, thuật toán đặt left = 5 và right = 5 sau lần lặp đầu tiên và không thay đổi. Đầu ra trở thành 1, đếm chính xác rằng chỉ x = 5 thỏa mãn mọi ràng buộc. 

Một trường hợp đặc biệt khác là khi các khoảng thu gọn ngay sau vài bước đầu tiên. Ví dụ: 

đầu vào: 

n = 2 

a = [1, 100] 

b = [2, 90] 

Sau khi chuẩn hóa, chúng tôi nhận được [1,2] và [90,100]. Sau khi xử lý khoảng đầu tiên, left = 1, right = 2. Sau khoảng thứ hai, left trở thành 90 và right trở thành 2, tạo ra left > right và đầu ra 0. Thuật toán phát hiện chính xác những điều không thể xảy ra ngay khi giao lộ trở nên trống trải, mặc dù các bản cập nhật sau đó vẫn tiếp tục nhất quán.
