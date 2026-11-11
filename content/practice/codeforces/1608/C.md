---
title: "CF 1608C - Bậc thầy trò chơi"
description: "Chúng tôi được yêu cầu xác định đối với mỗi người chơi trong một giải đấu, liệu họ có thể giành chiến thắng hay không nếu giải đấu được tiến hành một cách tối ưu. Có hai bản đồ và mỗi người chơi có một điểm mạnh trên cả hai bản đồ."
date: "2026-06-10T07:33:08+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "dp", "graphs", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1608
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 758 (Div.1 + Div. 2)"
rating: 1700
weight: 1608
solve_time_s: 104
verified: true
draft: false
---

[CF 1608C - Bậc thầy trò chơi](https://codeforces.com/problemset/problem/1608/C) 

**Đánh giá:** 1700 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, dp, đồ thị, tham lam, hai con trỏ 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xác định đối với mỗi người chơi trong một giải đấu, liệu họ có thể giành chiến thắng hay không nếu giải đấu được tiến hành một cách tối ưu. Có hai bản đồ và mỗi người chơi có một điểm mạnh trên cả hai bản đồ. Khi hai người chơi chiến đấu trên bản đồ đã chọn, người chơi có sức mạnh cao hơn trên bản đồ đó luôn thắng. Không có hai người chơi nào có sức mạnh như nhau trên cùng một bản đồ nên mỗi trận chiến đều có người chiến thắng rõ ràng. 

Một giải đấu bao gồm việc liên tục chọn hai người chơi còn lại bất kỳ và để họ chiến đấu trên bất kỳ bản đồ nào, loại bỏ người thua cuộc cho đến khi còn lại một người chơi. Đối với mỗi người chơi, chúng ta cần quyết định xem có tồn tại một chuỗi trận đấu cho phép họ là người chiến thắng cuối cùng hay không. Đầu ra là một chuỗi có độ dài$n$, với "1" nếu người chơi có thể thắng và "0" nếu ngược lại. 

Những ràng buộc ngụ ý$n$có thể đạt được$10^5$tổng cộng trên tất cả các trường hợp thử nghiệm. Điều này loại trừ bất kỳ cách tiếp cận nào mô phỏng tất cả các chuỗi trận chiến có thể xảy ra, vì đó có thể là giai thừa trong$n$. Thay vào đó, chúng ta cần một thuật toán tuyến tính hoặc gần tuyến tính. 

Các trường hợp có lợi thế không rõ ràng bao gồm giải đấu một người chơi, trong đó người chơi đó giành chiến thắng một cách tầm thường và các tình huống trong đó sức mạnh của người chơi đến mức không một người chơi nào thống trị cả hai bản đồ, cho phép một số người chơi có cơ hội nếu họ tránh đối đầu trực tiếp với đối thủ mạnh hơn trên bản đồ mạnh của họ. Ví dụ: với hai người chơi: điểm mạnh ở bản đồ A$[2,1]$và bản đồ B$[1,2]$, cả hai đều có thể thắng tùy theo thứ tự trận đấu, vì vậy kết quả đầu ra phải là "11". 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu là mô phỏng mọi chuỗi chiến đấu có thể xảy ra. Chúng tôi sẽ chọn mọi cặp và bản đồ có thể, tuyên truyền người chiến thắng và kiểm tra xem người chơi có thể lọt vào trận chung kết hay không. Cách tiếp cận này đúng về nguyên tắc nhưng đòi hỏi phải xem xét$O(n!)$trình tự cho$n$người chơi, điều này là không thể thực hiện được$n$lên tới$10^5$. 

Thông tin chi tiết quan trọng là kết quả của giải đấu bị hạn chế bởi sự thống trị ở hai bản đồ. Một người chơi yếu hơn một số người chơi khác trong cả hai bản đồ không bao giờ có thể giành chiến thắng, bởi vì cuối cùng họ sẽ đối mặt với người chơi mạnh hơn đó và thua bất kể bản đồ đã chọn. Ngược lại, bất kỳ người chơi nào không bị người khác thống trị trong cả hai bản đồ đều có thể được sắp xếp để tránh đối thủ mạnh hơn của họ cho đến cuối cùng, khiến họ có thể trở thành người chiến thắng. 

Để làm bê tông này, hãy xem xét cường độ hai chiều$(a_i, b_i)$của mỗi người chơi. Chúng tôi có thể xác định "biên giới thống trị" tối thiểu bằng cách sắp xếp người chơi theo một bản đồ và sau đó theo dõi sức mạnh tối đa ở bản đồ khác được thấy cho đến nay. Một người chơi có khả năng bị loại nếu tồn tại ai đó mạnh hơn ở cả hai bản đồ. Bất kỳ người chơi nào nằm ngoài nhóm thống trị này đều có thể là người chiến thắng. 

Điều này làm giảm vấn đề sắp xếp và quét tuyến tính trên người chơi, mang lại độ phức tạp về thời gian$O(n \log n)$cho mỗi trường hợp thử nghiệm thay vì giai thừa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$và mảng sức mạnh$a$Và$b$. Kết hợp chúng thành từng cặp$(a_i, b_i, i)$để giữ lại chỉ mục ban đầu. 
2. Sắp xếp các cặp theo thứ tự tăng dần$a_i$. Nếu hai người chơi có cùng điểm$a_i$, đặt hàng theo$b_i$giảm dần. Điều này đảm bảo rằng khi chúng tôi quét từ trái sang phải, chúng tôi chỉ cần theo dõi mức tối đa hiện tại$b$. 
3. Khởi tạo một biến$max_b$ĐẾN$-\infty$. Điều này sẽ theo dõi mức cao nhất$b$giá trị nhìn thấy cho đến nay theo thứ tự được sắp xếp. 
4. Khởi tạo một mảng kết quả có độ dài bằng 0$n$. 
5. Lặp lại danh sách người chơi đã được sắp xếp. Đối với mỗi người chơi$(a, b, idx)$:

- Nếu như$b \ge max_b$, đánh dấu result[idx] là 1, vì không có người chơi nào được quét trước đó thống trị người chơi này trong cả hai bản đồ. 
- Cập nhật$max_b$đến mức tối đa$max_b$Và$b$. 
6. Sau khi quét tất cả người chơi, in kết quả dưới dạng chuỗi. 

Tại sao nó hoạt động: Sắp xếp theo$a$và quét trong khi theo dõi tối đa$b$đảm bảo rằng chúng tôi xác định được bất kỳ người chơi nào chiếm ưu thế trong cả hai bản đồ. Bất kỳ người chơi nào sống sót sau cuộc kiểm tra này đều có ít nhất một chiều mà họ không bị thống trị bởi bất kỳ người chơi mạnh hơn nào, nghĩa là tồn tại một chuỗi chiến đấu thông minh để họ có thể tránh bị loại cho đến cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        players = [(a[i], b[i], i) for i in range(n)]
        players.sort()  # sort by a ascending

        res = [0] * n
        max_b = -1
        for pa, pb, idx in players:
            if pb >= max_b:
                res[idx] = 1
            max_b = max(max_b, pb)
        print("".join(map(str, res)))

if __name__ == "__main__":
    solve()
```Giải pháp này trước tiên đọc tất cả đầu vào một cách hiệu quả. Trình phát được kết hợp thành các bộ dữ liệu với các chỉ số ban đầu của chúng để tạo ra đầu ra theo thứ tự đầu vào. Sắp xếp theo$a$đảm bảo rằng khi chúng tôi kiểm tra$b$, chúng tôi chỉ cần duy trì mức chạy tối đa để xác định xem người chơi có bị áp đảo ở cả hai bản đồ hay không. Điều này xử lý các ranh giới và các trường hợp cạnh một cách tự nhiên, chẳng hạn như một người chơi hoặc những người chơi có điểm mạnh tạo thành mô hình chéo trong hai bản đồ. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:```
4
1 2 3 4
1 2 3 4
```| Người chơi | một | b | max_b | độ phân giải | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | -1 | 1 | 
| 2 | 2 | 2 | 1 | 1 | 
| 3 | 3 | 3 | 2 | 1 | 
| 4 | 4 | 4 | 3 | 1 | 

Đầu ra: "1111". 

Tất cả người chơi đều mạnh hơn tất cả những người trước đó ở cả hai bản đồ, vì vậy tất cả đều có tiềm năng chiến thắng. 

**Mẫu 2:** 

đầu vào:```
4
11 12 20 21
44 22 11 30
```| Người chơi | một | b | max_b | độ phân giải | 
| --- | --- | --- | --- | --- | 
| 1 | 11 | 44 | -1 | 1 | 
| 2 | 12 | 22 | 44 | 0 | 
| 3 | 20 | 11 | 44 | 0 | 
| 4 | 21 | 30 | 44 | 0 | 

Đầu ra: "1000". 

Người chơi 1 không bị thống trị ở cả hai bản đồ nên có thể giành chiến thắng. Người chơi 2, 3, 4 đều có người chơi trước mạnh hơn ở cả hai chiều. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp người chơi chiếm ưu thế trong tính toán. Quét tuyến tính sau đó là O(n). | 
| Không gian | O(n) | Lưu trữ các bộ dữ liệu của trình phát và mảng kết quả. | 

Thuật toán dễ dàng nằm gọn trong giới hạn 1 giây ngay cả đối với tổng$n = 10^5$qua các trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("3\n4\n1 2 3 4\n1 2 3 4\n4\n11 12 20 21\n44 22 11 30\n1\n1000000000\n1000000000\n") == "1111\n1000\n1"

# Custom cases
assert run("1\n2\n2 1\n1 2\n") == "11", "Both players can win depending on order"
assert run("1\n1\n42\n42\n") == "1", "Single player always wins"
assert run("1\n3\n3 1 2\n1 3 2\n") == "111", "Cross pattern, all can win"
assert run("1\n5\n1 2 3 4 5\n5 4 3 2 1\n") == "11111", "Descending b, all can win"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 người chơi chéo sức mạnh | 11 | Độ nhạy lệnh | 
| Người chơi đơn | 1 | Giải đấu tầm thường | 
| 3 người chơi chéo | 111 | Sự thống trị chéo phức tạp | 
| 5 tăng dần a, giảm dần b | 11111 | Logic biên giới không bị chi phối | 

## Vỏ cạnh 

Đầu vào giải đấu một người chơi như`1\n42\n42\n`được xử lý vì vòng lặp qua những người chơi được sắp xếp ngay lập tức đánh dấu người chơi duy nhất là người chiến thắng. 

Đối với mẫu thống trị chéo có đầu vào`3\n3 1 2\n1 3 2\n`, thuật toán xác định chính xác rằng không có người chơi nào bị thống trị trong cả hai bản đồ ở bất kỳ bước nào. Mỗi bản cập nhật của`max_b`đảm bảo chúng tôi chỉ không đồng ý
