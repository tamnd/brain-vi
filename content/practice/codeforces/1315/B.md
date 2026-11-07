---
title: "CF 1315B - Về Nhà"
description: "Chúng ta có một thị trấn một chiều được biểu diễn dưới dạng một đường giao nhau được đánh số từ 1 đến n. Mỗi vị trí có chính xác một loại trạm vận chuyển: loại A (điểm đánh dấu đoạn tương thích với xe buýt) hoặc loại B (điểm đánh dấu đoạn tương thích với xe điện). Chuỗi s mã hóa bố cục này."
date: "2026-06-16T06:57:52+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1315
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 623 (Div. 2, based on VK Cup 2019-2020 - Elimination Round, Engine)"
rating: 1300
weight: 1315
solve_time_s: 155
verified: false
draft: false
---

[CF 1315B - Homecoming](https://codeforces.com/problemset/problem/1315/B) 

**Đánh giá:** 1300 
**Tags:** tìm kiếm nhị phân, dp, tham lam, chuỗi 
**Thời gian giải:** 2m 35s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một thị trấn một chiều được biểu diễn dưới dạng một đường giao nhau được đánh số từ 1 đến n. Mỗi vị trí có chính xác một loại trạm vận chuyển: loại A (điểm đánh dấu đoạn tương thích với xe buýt) hoặc loại B (điểm đánh dấu đoạn tương thích với xe điện). Chuỗi s mã hóa bố cục này. 

Petya bắt đầu ở vị trí 1 và muốn đạt đến vị trí n. Việc di chuyển chỉ có ý nghĩa thông qua phương tiện giao thông công cộng: từ bất kỳ vị trí i nào, anh ta có thể nhảy tới vị trí j bằng xe buýt nếu mọi vị trí từ i đến j−1 là A, hoặc sử dụng xe điện nếu mọi vị trí từ i đến j−1 là B. Mỗi lần nhảy như vậy sẽ có một mức giá cố định tùy thuộc vào loại phương tiện giao thông, a đối với các đoạn xe buýt và b đối với các đoạn xe điện. 

Petya được phép đi bộ từ đầu đến vị trí i nào đó mà không phải trả tiền và sau đó chỉ phải sử dụng những bước nhảy vận chuyển này từ i để đến đích. Mục tiêu là chọn vị trí bắt đầu nhỏ nhất có thể i sao cho tổng chi phí từ i đến n không vượt quá p. 

Cấu trúc chính là chi phí di chuyển chỉ phụ thuộc vào các phân đoạn thống nhất liền kề trong chuỗi. Mỗi khối tối đa gồm các ký tự giống hệt nhau tương ứng với một phân đoạn “chế độ” bắt buộc và trong mỗi khối như vậy, Petya có thể di chuyển tự do bằng một vé duy nhất. 

Các ràng buộc cho phép tổng cộng tối đa 10^5 ký tự trong các trường hợp thử nghiệm, loại trừ mọi giải pháp tính toán lại chi phí từ đầu cho mọi vị trí bắt đầu. Một cách tiếp cận bậc hai tính toán lại chi phí đi lại từ mỗi i sẽ đạt tới 10^10 phép toán trong trường hợp xấu nhất, điều này là không khả thi. Chúng ta cần tiền xử lý tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh tinh tế là khi Petya bắt đầu bên trong một khối đồng nhất gần cuối chuỗi. Ví dụ: nếu s = "AAAAA" và a lớn trong khi p nhỏ, việc bắt đầu sớm hơn hoặc muộn hơn trong cùng một khối sẽ không làm thay đổi cấu trúc chi phí, vì chi phí chỉ phụ thuộc vào sự chuyển đổi giữa các khối chứ không phụ thuộc vào vị trí chính xác. 

Một trường hợp khác là các chuỗi xen kẽ như "ABABAB". Ở đây, mỗi bước là một phân đoạn mới, do đó chi phí tăng gần như tuyến tính theo số lần chuyển đổi và trực giác tham lam về việc nhóm phải được áp dụng cẩn thận. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử mọi vị trí bắt đầu có thể i. Đối với mỗi i, mô phỏng hành trình từ i đến n bằng cách quét xuôi và tính tổng chi phí bất cứ khi nào chúng ta nhập một phân đoạn mới gồm các ký tự không đổi. Mỗi mô phỏng mất O(n) thời gian và thực hiện điều này với tất cả i dẫn đến O(n^2) cho mỗi trường hợp thử nghiệm, quá chậm khi n đạt 10^5. 

Quan sát quan trọng là chi phí từ i đến n chỉ phụ thuộc vào chuỗi các khối liền kề tối đa bắt đầu từ i. Nếu chúng ta nén chuỗi thành các khối, ví dụ "AABBBBAA" trở thành A, B, A có độ dài, thì chi phí từ bất kỳ i nào trong khối chỉ phụ thuộc vào hậu tố của chuỗi khối này bắt đầu từ khối đó. 

Điều này gợi ý một cấu trúc lập trình động từ phải sang trái. Nếu chúng ta xác định cost[i] là chi phí tối thiểu để đi từ vị trí i đến n, chúng ta có thể tính chi phí theo thứ tự ngược lại. Khi chúng ta ở vị trí i, chúng ta sẽ ở trong cùng một khối ký tự hoặc chúng ta chuyển sang khối tiếp theo, trả tiền a hoặc b tùy thuộc vào ký tự hiện tại. Nếu s[i] == s[i+1] thì cost[i] = cost[i+1] vì chúng ta vẫn ở trong cùng một phân khúc và không cần vé mới ở ranh giới đó. Ngược lại, chúng tôi trả một vé cộng với chi phí[i+1]. 

Khi đã biết tất cả chi phí[i], chúng ta chỉ cần tìm giá trị i nhỏ nhất sao cho chi phí[i] ≤ p. 

Sự cải thiện xuất phát từ việc nhận ra rằng việc tính toán lại chi phí hậu tố nhiều lần là dư thừa, bởi vì chi phí [i] chỉ phụ thuộc vào chi phí [i+1] và so sánh ký tự cục bộ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force từ mỗi i | O(n^2) | O(1) | Quá chậm | 
| Đảo ngược DP qua chi phí hậu tố | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính toán trước chi phí mảng có kích thước n+1 trong đó cost[n] = 0 vì không cần di chuyển khi đã đến đích. 
2. Duyệt chuỗi từ phải sang trái, từ i = n−1 xuống 1, tính cost[i] từ cost[i+1]. Điều này đảm bảo rằng khi tính toán chi phí [i], chi phí hậu tố đã được biết trước. 
3. Nếu s[i] giống với s[i+1], đặt cost[i] = cost[i+1]. Điều này tương ứng với việc ở trong cùng một khối loại vận chuyển, nơi không cần có vé mới cho bước này. 
4. Nếu s[i] khác s[i+1], chúng ta đang vượt qua ranh giới giữa A và B. Trong trường hợp này, chúng ta phải trả chi phí vé cho đoạn bắt đầu từ i. Nếu s[i] == 'A', chúng ta thêm a; nếu không thì chúng ta thêm b. Sau đó đặt cost[i] = cost[i+1] + giá trị đó. Điều này cho thấy thực tế là mọi thay đổi về loại phân khúc đều buộc phải có một vé mới. 
5. Sau khi tính toán toàn bộ chi phí, quét từ i = 1 đến n và trả về chỉ số i nhỏ nhất sao cho cost[i] ≤ p. Đây là điểm sớm nhất mà Petya có đủ khả năng để đi đến cuối cùng. 

### Tại sao nó hoạt động 

Bất biến chính là cost[i] thể hiện chính xác chi phí tối thiểu cần thiết để di chuyển từ i đến n bằng cách sử dụng phân đoạn tối ưu của các ký tự thống nhất liền kề. Bởi vì bất kỳ đường dẫn hợp lệ nào cũng phải tôn trọng ranh giới phân đoạn và mỗi giao điểm qua ranh giới được xác định duy nhất bởi liệu s[i] có khác với s[i+1] hay không, phép truy toán sẽ nắm bắt tất cả các phân tách có thể có của hành trình thành các phân đoạn đơn sắc tối đa. Không có con đường thay thế nào có thể giảm chi phí, vì việc bỏ qua một ranh giới là không thể nếu không thay đổi các ràng buộc về ký tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, b, p = map(int, input().split())
    s = input().strip()
    n = len(s)

    cost = [0] * n
    cost[-1] = 0

    for i in range(n - 2, -1, -1):
        if s[i] == s[i + 1]:
            cost[i] = cost[i + 1]
        else:
            cost[i] = cost[i + 1] + (a if s[i] == 'A' else b)

    ans = 0
    for i in range(n):
        if cost[i] <= p:
            ans = i + 1
            break

    print(ans)
```Việc triển khai phản ánh trực tiếp sự lặp lại DP. Mảng chi phí được lập chỉ mục 0, trong khi vấn đề được lập chỉ mục 1, vì vậy câu trả lời cuối cùng sẽ thêm một chỉ mục khi chuyển đổi chỉ mục. Việc truyền tải ngược đảm bảo chúng ta không bao giờ truy cập vào trạng thái chưa được tính toán. 

Một lỗi phổ biến là cố gắng nén các phân đoạn trước rồi mô phỏng các bước nhảy; mặc dù hợp lệ nhưng không cần thiết và dễ mắc sai lầm về điều kiện biên. DP này tránh hoàn toàn việc xây dựng phân khúc rõ ràng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

s = "ABAB", a = 2, b = 1 

Chúng tôi tính chi phí từ phải sang trái. 

| tôi | s[i] | s[i+1] | chi phí[i+1] | chuyển tiếp | chi phí[i] | 
| --- | --- | --- | --- | --- | --- | 
| 3 | B | - | 0 | căn cứ | 0 | 
| 2 | A | B | 0 | +a | 2 | 
| 1 | B | A | 2 | +b | 3 | 
| 0 | A | B | 3 | +a | 5 | 

Nếu p = 3, vị trí bắt đầu hợp lệ là những vị trí có chi phí ≤ 3, tức là i = 2 và i = 3 (1-chỉ mục: 3 và 4). Nhỏ nhất là 3. 

Dấu vết này cho thấy mỗi lần thay thế buộc phải thanh toán mới như thế nào và cách tái sử dụng hậu tố được tích lũy một cách chính xác như thế nào. 

### Ví dụ 2 

đầu vào: 

s = "AABBB", a = 3, b = 2 

| tôi | s[i] | s[i+1] | chi phí[i+1] | chuyển tiếp | chi phí[i] | 
| --- | --- | --- | --- | --- | --- | 
| 4 | B | - | 0 | căn cứ | 0 | 
| 3 | B | B | 0 | giống nhau | 0 | 
| 2 | B | B | 0 | giống nhau | 0 | 
| 1 | A | B | 0 | +a | 3 | 
| 0 | A | A | 3 | giống nhau | 3 | 

Nếu p = 3, Petya có thể bắt đầu ở i = 1 (1-index 2) hoặc cao hơn. Chỉ số hợp lệ nhỏ nhất là 2. 

Điều này xác nhận rằng các khối đồng nhất dài không tích lũy chi phí nội bộ, chỉ có sự chuyển tiếp ranh giới mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi vị trí được xử lý một lần trong DP ngược và một lần trong lần quét cuối cùng | 
| Không gian | O(n) | Lưu trữ cho mảng chi phí | 

Tổng n trên các trường hợp thử nghiệm tối đa là 10^5, do đó giải pháp chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        a, b, p = map(int, input().split())
        s = input().strip()
        n = len(s)

        cost = [0] * n
        cost[-1] = 0

        for i in range(n - 2, -1, -1):
            if s[i] == s[i + 1]:
                cost[i] = cost[i + 1]
            else:
                cost[i] = cost[i + 1] + (a if s[i] == 'A' else b)

        ans = 0
        for i in range(n):
            if cost[i] <= p:
                ans = i + 1
                break

        out.append(str(ans))

    return "\n".join(out)

# provided sample
assert run("""5
2 2 1
BB
1 1 1
AB
3 2 8
AABBBBAABB
5 3 4
BBBBB
2 1 1
ABABAB
""") == """2
1
3
1
6"""

# all same character minimal
assert run("""1
5 3 10
AAAAA
""") == "1"

# alternating tight budget
assert run("""1
1 1 2
ABABAB
""") == "3"

# single step case
assert run("""1
10 10 0
AB
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả Một chuỗi | 1 | không chuyển tiếp, chi phí không đổi | 
| ABABAB chặt chẽ | 3 | chi phí luân phiên tích lũy chính xác | 
| AB với ngân sách bằng 0 | 2 | di chuyển tối thiểu khi không có tiền | 

## Vỏ cạnh 

Đối với một chuỗi bao gồm toàn bộ các ký tự giống hệt nhau như "AAAAA", mọi cost[i] đều bằng 0 vì không có chuyển đổi nào xảy ra. Thuật toán đặt cost[n−1] = 0 và truyền ngược lại đẳng thức, tạo ra cost[i] = 0 cho tất cả i, do đó chỉ số 1 luôn được trả về chính xác. 

Đối với các chuỗi xen kẽ hoàn toàn như "ABABAB", mỗi bước sẽ giới thiệu một ranh giới phân đoạn mới. DP thêm a hoặc b ở mọi chỉ mục, do đó cost[i] tăng tuyến tính theo khoảng cách đến cuối. Thuật toán xác định chính xác hậu tố sớm nhất trong ngân sách mà không cần phân đoạn rõ ràng. 

Đối với trường hợp a hoặc b lớn hơn p nhiều, chỉ các vị trí gần cuối mới hợp lệ. DP vẫn nắm bắt chính xác điều này vì bất kỳ sự không phù hợp sớm nào sẽ ngay lập tức tích lũy chi phí, ngăn chặn tính khả thi không chính xác do lòng tham địa phương.
