---
title: "CF 106239H - \u80fd\u91cf\u6c47\u805a"
description: "Chúng ta đang đứng trên một hàng các vị trí được đánh số từ 0 đến n. Vị trí 0 là điểm bắt đầu và không có phần thưởng, trong khi mọi vị trí từ 1 đến n đều đóng góp một giá trị năng lượng cố định nếu chúng ta đặt chân vào vị trí đó. Chúng tôi bắt đầu ở vị trí 0 và muốn đạt đến vị trí n. Chuyển động bị hạn chế theo hai cách."
date: "2026-06-19T14:09:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106239
codeforces_index: "H"
codeforces_contest_name: "2025\u5e7f\u4e1c\u5de5\u4e1a\u5927\u5b66\u65b0\u751f\u8d5b(\u51b3\u8d5b)"
rating: 0
weight: 106239
solve_time_s: 47
verified: true
draft: false
---

[CF 106239H - \u80fd\u91cf\u6c47\u805a](https://codeforces.com/problemset/problem/106239/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang đứng trên một hàng các vị trí được đánh số từ 0 đến n. Vị trí 0 là điểm bắt đầu và không có phần thưởng, trong khi mọi vị trí từ 1 đến n đều đóng góp một giá trị năng lượng cố định nếu chúng ta đặt chân vào vị trí đó. Chúng tôi bắt đầu ở vị trí 0 và muốn đạt đến vị trí n. 

Chuyển động bị hạn chế theo hai cách. Mỗi bước di chuyển là một bước có độ dài 1 hoặc 2 về phía trước. Tuy nhiên, chúng ta không thể thực hiện hai nước đi 1 bước liên tiếp. Điều này tạo ra sự phụ thuộc giữa nước đi cuối cùng và nước đi được phép tiếp theo. Mỗi khi chúng ta hạ cánh ở vị trí i, chúng ta sẽ thu thập ngay Ei. 

Nhiệm vụ là chọn một chuỗi các bước nhảy hợp lệ từ 0 đến n tuân theo quy tắc “không có hai bước 1 bước liên tiếp” và tối đa hóa tổng năng lượng thu được. 

Ràng buộc n 100000 ngụ ý rằng chúng ta cần ít nhất thời gian tuyến tính hoặc gần tuyến tính. Bất kỳ giải pháp nào liệt kê các đường dẫn đều theo cấp số nhân vì tại mỗi vị trí, chúng tôi phân nhánh thành hai loại di chuyển với sự phụ thuộc toàn cục và số lượng trình tự hợp lệ tăng lên giống như một phép lặp kiểu Fibonacci nhưng có trạng thái bổ sung, do đó, lực lượng vũ phu là không thể. Ngay cả một chương trình động đơn giản thử tất cả các chuỗi mà không nén trạng thái cũng sẽ lặp lại các bài toán con giống hệt nhau. 

Trường hợp phức tạp xuất phát từ việc hạn chế 1 bước liên tiếp. Nếu một giải pháp quên sự phụ thuộc này, giải pháp đó sẽ xử lý vấn đề không chính xác như một DP “leo lên 1 hoặc 2 bước” tiêu chuẩn và tính quá mức các chuyển đổi không hợp lệ. Một trường hợp khác là khi n nhỏ, đặc biệt là n = 1 hoặc n = 2, trong đó quy tắc “không có 1 bước liên tiếp” không được kích hoạt đầy đủ và các chuyển đổi bất cẩn có thể lập chỉ mục ra ngoài giới hạn hoặc bỏ lỡ đường dẫn hợp lệ duy nhất. 

Ví dụ: nếu n = 2 và E = [100, 1], đường đi tối ưu là 0 → 2 nếu được phép, nhưng tùy theo cách giải thích, chỉ những nước đi hợp lệ mới phải được kiểm tra cẩn thận vì 0 → 1 → 2 bị cấm do có 1 bước liên tiếp, vì vậy câu trả lời là E2 = 1. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi chuỗi bước nhảy có thể bắt đầu từ 0, theo dõi vị trí hiện tại và liệu bước di chuyển cuối cùng là 1 bước hay 2 bước. Ở mỗi trạng thái, chúng ta phân nhánh thành các bước di chuyển hợp lệ và tích lũy năng lượng. Điều này đúng vì nó khám phá tất cả các con đường hợp pháp và tính tổng phần thưởng một cách chính xác theo vị trí hạ cánh. Vấn đề là số lượng đường đi tăng theo cấp số nhân với n. Ngay cả khi cắt bớt các chuyển đổi 1 bước liên tiếp không hợp lệ, không gian trạng thái vẫn bùng nổ vì mỗi vị trí có thể đạt được theo nhiều cách với lịch sử khác nhau, dẫn đến việc tính toán lại nhiều lần. 

Quan sát quan trọng là thông tin lịch sử duy nhất quan trọng là liệu động thái trước đó là 1 bước hay 2 bước. Toàn bộ bài toán được chuyển thành một chương trình động trên các vị trí có hai trạng thái. Khi chúng tôi mã hóa trạng thái này, các quá trình chuyển đổi sẽ trở thành cục bộ và độc lập với toàn bộ lịch sử. Điều này biến bài toán thành một DP tuyến tính trong đó mỗi vị trí được tính một lần cho mỗi trạng thái. 

Chúng tôi xác định dp[i][0] là năng lượng tối đa khi tiếp cận i với bước cuối cùng là 1 bước và dp[i][1] là năng lượng tối đa khi tiếp cận i với bước cuối cùng là 2 bước. Ràng buộc cấm chuyển đổi từ dp[i][0] sang i+1 bằng 1 bước khác, điều này thực thi quy tắc một cách tự nhiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | Độ sâu đệ quy O(n) | Quá chậm | 
| DP tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một bảng DP được lập chỉ mục theo vị trí và loại nước đi cuối cùng.

1. Khởi tạo tất cả các trạng thái dưới dạng âm vô cực ngoại trừ trạng thái bắt đầu. Ở vị trí 0, về mặt khái niệm, chúng tôi bắt đầu với trạng thái “không có nước đi trước”, cho phép thực hiện hành động 1 bước hoặc 2 bước làm hành động đầu tiên. Điều này tránh việc hạn chế sai lần nhảy đầu tiên. 
2. Từ mỗi vị trí i, hãy xem xét các chuyển tiếp tùy thuộc vào cách chúng ta đến đó. Nếu chúng ta đến bằng bước 1 thì bước tiếp theo phải là bước 2, vì vậy chúng ta chỉ chuyển sang i+2. Nếu chúng ta đến bằng 2 bước, chúng ta có thể đi đến i+1 hoặc i+2. Điều này trực tiếp mã hóa quy tắc và ngăn chặn các chuỗi 1 bước liên tiếp không hợp lệ. 
3. Khi chuyển từ i sang j, chúng tôi thêm Ej vào giá trị DP vì chúng tôi thu thập năng lượng khi hạ cánh. Điều này đảm bảo mọi đóng góp hạ cánh được tính chính xác một lần. 
4. Chúng tôi xử lý các vị trí theo thứ tự tăng dần từ 0 đến n, bởi vì tất cả các quá trình chuyển đổi đều được tiến hành nên các trạng thái trước đó được hoàn thiện đầy đủ trước khi được sử dụng. 
5. Câu trả lời là mức tối đa của dp[n][0] và dp[n][1], vì việc đạt đến n có thể xảy ra với một trong hai loại nước đi cuối cùng. 

Tại sao nó hoạt động được gắn với một đối số nén trạng thái. Bất kỳ đường đi hợp lệ nào kết thúc tại i đều được đặc trưng đầy đủ bởi điểm tốt nhất và loại nước đi cuối cùng. Không có chi tiết lịch sử nào khác ảnh hưởng đến tính hợp pháp hoặc phần thưởng trong tương lai, vì vậy chỉ lưu trữ hai trạng thái này sẽ bảo tồn cấu trúc cơ sở tối ưu. Mọi quá trình chuyển đổi đều đảm bảo tính khả thi và mọi đường dẫn khả thi đều được thể hiện trong DP, do đó, không có giải pháp ứng cử viên nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    E = [0] + list(map(int, input().split()))

    NEG = -10**18

    dp = [[NEG, NEG] for _ in range(n + 1)]

    dp[0][0] = dp[0][1] = 0

    for i in range(n + 1):
        for last in (0, 1):
            if dp[i][last] == NEG:
                continue

            if i + 1 <= n:
                if last == 1:
                    dp[i + 1][0] = max(dp[i + 1][0], dp[i][last] + E[i + 1])
            if i + 2 <= n:
                dp[i + 2][1] = max(dp[i + 2][1], dp[i][last] + E[i + 2])

    print(max(dp[n][0], dp[n][1]))

if __name__ == "__main__":
    solve()
```Bảng DP phân biệt rõ ràng hai trạng thái “nước đi cuối cùng” được phép. Logic chuyển đổi thực thi quy tắc: sau 1 bước (cuối = 0), chỉ cho phép 2 bước, vì vậy chúng tôi chỉ cập nhật dp[i+2][1]. Sau 2 bước (cuối = 1), cả hai lần chuyển đổi đều hợp lệ. Năng lượng được bổ sung tại thời điểm đến, đảm bảo mỗi vị trí đóng góp chính xác một lần cho mỗi đường dẫn. 

Việc khởi tạo dp[0][0] = dp[0][1] = 0 là một thủ thuật mô hình hóa cho phép nước đi đầu tiên hoạt động thống nhất mà không cần vỏ đặc biệt. Vì vị trí 0 không có năng lượng nên nó không làm biến dạng tổng. 

## Ví dụ đã hoạt động 

Xét đầu vào mẫu n = 5, E = [1, 10, 10, 10, 1]. 

Chúng tôi chỉ theo dõi các trạng thái có thể truy cập. 

| tôi | dp[i][last=1] (2 bước cuối cùng) | dp[i][last=0] (1 bước cuối cùng) | 
| --- | --- | --- | 
| 0 | 0 | 0 | 
| 2 | 10 | - | 
| 4 | 20 | - | 
| 5 | - | 21 | 

Quá trình chuyển đổi là 0 → 2 → 4 → 5. Mỗi lần tiếp đất, chúng ta sẽ thêm năng lượng: 10 + 10 + 1 = 21. Bảng cho thấy chỉ có chuyển đổi 2 bước chiếm ưu thế vì quy tắc hạn chế rất nhiều 1 bước liên tiếp. 

Bây giờ hãy xem xét trường hợp nhỏ hơn n = 4, E = [5, 100, 5, 100]. 

| tôi | dp[i][last=1] | dp[i][last=0] | 
| --- | --- | --- | 
| 0 | 0 | 0 | 
| 2 | 100 | - | 
| 3 | - | 105 | 
| 4 | 200 | - | 

Ở đây, đường dẫn tối ưu là 0 → 2 → 4, thu thập 100 + 100 = 200. Mọi nỗ lực chèn sớm 1 bước sẽ chặn 1 bước khác và làm giảm tính linh hoạt mà DP nắm bắt chính xác. 

Dấu vết xác nhận rằng việc phân chia trạng thái thể hiện chính xác cách các tùy chọn di chuyển trong tương lai chỉ phụ thuộc vào bước cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vị trí xử lý các chuyển đổi liên tục cho hai trạng thái | 
| Không gian | O(n) | Bảng DP lưu trữ hai trạng thái cho mỗi vị trí | 

Thuật toán chạy trong thời gian tuyến tính trên n lên tới 100000, phù hợp thoải mái trong các ràng buộc điển hình. Việc sử dụng bộ nhớ cũng tuyến tính và đủ nhỏ để đạt đến giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(sys.stdin.readline())
    E = [0] + list(map(int, sys.stdin.readline().split()))

    NEG = -10**18
    dp = [[NEG, NEG] for _ in range(n + 1)]
    dp[0][0] = dp[0][1] = 0

    for i in range(n + 1):
        for last in (0, 1):
            if dp[i][last] == NEG:
                continue
            if i + 1 <= n and last == 1:
                dp[i + 1][0] = max(dp[i + 1][0], dp[i][last] + E[i + 1])
            if i + 2 <= n:
                dp[i + 2][1] = max(dp[i + 2][1], dp[i][last] + E[i + 2])

    return str(max(dp[n][0], dp[n][1]))

# provided sample
assert run("5\n1 10 10 10 1\n") == "21"

# minimum n
assert run("1\n5\n") == "5"

# n = 2, must avoid invalid 1-1
assert run("2\n100 1\n") == "1"

# all equal values
assert run("4\n10 10 10 10\n") == "30"

# alternating high values
assert run("5\n1 100 1 100 1\n") == "201"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 giá trị đơn | 5 | tính đúng đắn của trường hợp cơ sở | 
| n=2 [100,1] | 1 | cấm đường 1+1 | 
| mảng thống nhất | 30 | tính nhất quán của tích lũy DP | 
| đỉnh xen kẽ | 201 | ưu tiên chính xác cho cấu trúc 2 bước | 

## Vỏ cạnh 

Đối với n = 1, bước di chuyển hợp lệ duy nhất là trực tiếp đến vị trí 1. DP khởi tạo dp[0][*] = 0 và cho phép thực hiện 2 bước, nhưng i+2 không hợp lệ, vì vậy chỉ chuyển đổi i+1 được sử dụng nếu trạng thái cuối cùng cho phép. Thuật toán tạo ra chính xác E1. 

Với n = 2 với E = [100, 1], đường đi tốt nhất tránh 0 → 1 → 2 vì sau 1 bước, 1 bước khác bị cấm. DP chỉ cho phép 0 → 2 thông qua 2 bước, mang lại giá trị 1, phù hợp với giải pháp nhận biết ràng buộc tối ưu. 

Đối với trường hợp tất cả các giá trị giống hệt nhau, DP đương nhiên thích nhảy 2 bước hơn vì chúng không làm giảm tính di động trong tương lai. Ví dụ: n = 4, tất cả E = 10 mang lại 0 → 2 → 4 cho tổng số 20, trong khi các chuỗi không hợp lệ không bao giờ được xây dựng do hạn chế trạng thái và DP tránh được việc đếm quá mức một cách chính xác. 

Những trường hợp này xác nhận rằng mã hóa trạng thái nắm bắt đầy đủ hạn chế di chuyển mà không cần lịch sử sâu hơn.
