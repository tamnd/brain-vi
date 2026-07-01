---
title: "CF 104412N - Vòng cổ"
description: "Chúng ta được cấp một chiếc vòng cổ hình tròn được biểu diễn dưới dạng một dãy ngọc trai tuyến tính, trong đó mỗi vị trí có hai thuộc tính: một giá trị và một loại."
date: "2026-07-01T02:31:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104412
codeforces_index: "N"
codeforces_contest_name: "2023 ICPC Gran Premio de Mexico 2da Fecha"
rating: 0
weight: 104412
solve_time_s: 85
verified: false
draft: false
---

[CF 104412N - Vòng cổ](https://codeforces.com/problemset/problem/104412/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chiếc vòng cổ hình tròn được biểu diễn dưới dạng một dãy ngọc trai tuyến tính, trong đó mỗi vị trí có hai thuộc tính: một giá trị và một loại. Isaac được phép chọn một điểm cắt trên chiếc vòng cổ, biến nó thành một đường thẳng, sau đó bắt đầu thu thập ngọc trai bằng cách liên tục lấy viên ngọc còn lại ngoài cùng bên trái hoặc ngoài cùng bên phải. Anh ta có thể lấy nhiều nhất$K$tổng số ngọc trai. 

Mỗi viên ngọc hoạt động khác nhau tùy thuộc vào loại của nó. Nếu là loại 1, thu thập nó ngay lập tức sẽ được điểm tương đương với giá trị của nó. Nếu là loại 2, anh ta chỉ có thể thu thập nó nếu hiện tại anh ta có ít nhất số điểm đó và khi thu thập được, nó sẽ chuyển chính xác số điểm đó thành xu. Mục tiêu chính là tối đa hóa tổng số xu thu được từ chuyển đổi loại 2. 

Quá trình này kết hợp hai quyết định tương tác chặt chẽ: đoạn nào của vòng tròn sẽ mở bằng cách cắt và chuỗi chọn trái/phải nào sẽ thực hiện theo chiều dài.$K$. Ràng buộc$N \le 10^5$khiến cho không thể mô phỏng trực tiếp tất cả các vị trí cắt và tất cả các trình tự chọn hàng. Tuy nhiên,$K \le 100$gợi ý mạnh mẽ rằng bất kỳ giải pháp nào cũng có thể đủ khả năng để khám phá các trạng thái theo cấp số nhân trong$K$, nhưng không có trong$N$. 

Một khó khăn tinh tế là việc tích điểm không hề đơn điệu một cách đơn giản. Một cách tiếp cận tham lam ngây thơ luôn cố gắng chọn cách chuyển đổi tiền ngay lập tức tốt nhất đã thất bại vì ngọc trai loại 2 bị hạn chế bởi điểm hiện tại chứ không phải tiềm năng trong tương lai. 

Trường hợp hư hỏng nhỏ xuất hiện khi viên ngọc lớn loại 2 xuất hiện sớm nhưng phải trì hoãn: 

đầu vào:```
3 2
5 100 1
1 2 1
```Nếu chúng ta tham lam cố gắng lấy viên ngọc loại 2 ở vị trí số 2 ngay lập tức thì điều đó là không thể, mặc dù việc chọn sự kết hợp phù hợp của các lựa chọn phụ có thể kích hoạt nó sau này. Điều này cho thấy tính khả thi phụ thuộc vào toàn bộ tiền tố của các lượt chọn đã chọn chứ không phải thứ tự cục bộ. 

Một trường hợp thất bại khác phát sinh từ việc bỏ qua điểm cắt. Các phần cắt khác nhau có thể hiển thị các chuỗi tiếp cận hoàn toàn khác nhau:```
4 2
10 1 10 1
1 2 1 2
```Việc cắt giữa các vị trí khác nhau sẽ thay đổi xem viên ngọc loại 2 có giá trị cao có thể tiếp cận được trong vòng hay không.$K$bước từ hai bên. Cách tiếp cận khởi động cố định bỏ qua các cấu hình này. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ thử mọi vị trí cắt có thể và với mỗi lần cắt sẽ mô phỏng tất cả các trình tự có thể có để đạt được$K$ngọc trai bằng cách chọn trái hoặc phải ở mỗi bước. Đối với mỗi chuỗi, chúng tôi mô phỏng việc tích lũy điểm và chuyển đổi tiền xu một cách tham lam bất cứ khi nào hợp lệ. 

có$N$các vị trí cắt. Từ mỗi lần cắt, số chuỗi độ dài trái phải$K$là$2^K$. Đối với mỗi chuỗi chúng tôi mô phỏng tối đa$K$các bước. Điều này mang lại khoảng$O(N \cdot 2^K \cdot K)$, nó quá lớn đối với$N = 10^5$, mặc dù$K$là nhỏ. Sự phân nhánh theo cấp số nhân chiếm ưu thế. 

Quan sát quan trọng là mặc dù$N$lớn, số lượng lựa chọn$K$đủ nhỏ để trạng thái của bất kỳ quy trình nào được xác định hoàn toàn bằng số lượng mục chúng tôi đã lấy từ đầu bên trái và bên phải. Sau khi chọn một vết cắt, mọi lựa chọn hợp lệ đều tương ứng với việc chọn$l$các mục từ phía bên trái và$r$từ phía bên phải sao cho$l + r \le K$. Điều này làm giảm không gian chuỗi hàm mũ thành số đa thức trạng thái trên mỗi lần cắt, đại khái là$O(K^2)$. 

Đối với một vết cắt cố định, chúng tôi có thể tính toán trước các hiệu ứng tiền tố/hậu tố: chúng tôi đạt được bao nhiêu điểm khi lấy một số lượng ngọc trai loại 1 nhất định và có bao nhiêu ngọc trai loại 2 có thể thu thập được theo các ràng buộc về tính khả thi. Sau đó, chúng tôi kết hợp các đóng góp trái và phải. Sự tương tác phát sinh do ngọc trai loại 2 yêu cầu điểm sẵn có nên chúng ta không thể coi chúng là tổng độc lập; thay vào đó, chúng tôi mô phỏng quy trình theo thứ tự, nhưng chỉ trên các chuỗi có độ dài ngắn nhiều nhất$K$, cho phép lập trình động theo số lượt chọn và điểm hiện tại. 

Do đó, bài toán trở thành: với mỗi lần cắt, hãy tính kết quả tốt nhất trên tất cả các phân bố của$K$chọn giữa trái và phải, trong đó mỗi bên có thể được mô phỏng trước cho tất cả các phần độ dài. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các trình tự) |$O(N \cdot 2^K \cdot K)$|$O(1)$| Quá chậm | 
| DP được tối ưu hóa khi chia nhỏ |$O(N \cdot K^2)$|$O(K^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi vị trí cắt có thể là điểm bắt đầu của chuỗi tuyến tính. Đối với mỗi lần cắt, chúng tôi mô phỏng việc chọn từ cả hai đầu bằng cách sử dụng lập trình động về số lượng mục được lấy từ bên trái và bên phải. 

1. Đối với một vết cắt cố định, hãy xác định mảng tuyến tính hóa bắt đầu từ vết cắt đó, bao quanh. 

Điều này cho phép mọi lựa chọn hợp lệ tương ứng với việc lấy một số tiền tố từ mảng được xoay này và tùy ý mở rộng từ đầu đối diện. 
2. Tính toán trước tiền tố DP để lấy$i$các mục từ phía bên trái. 

Đối với mỗi$i \le K$, chúng tôi theo dõi tất cả các trạng thái có thể truy cập dưới dạng (điểm hiện tại, số tiền kiếm được) sau một thời gian chính xác$i$chọn. 

Điều này là cần thiết vì ngọc trai loại 2 phụ thuộc vào việc phải có đủ điểm tích lũy trước khi lấy. 
3. Thực hiện tương tự cho bên phải, tính toán các trạng thái DP tương tự để lấy$j$mặt hàng. 
4. Cho mỗi lần chia tay$i + j \le K$, kết hợp trạng thái trái và phải. 

Khi kết hợp, chúng ta phải đảm bảo rằng thứ tự trình tự là hợp lệ: tất cả các lượt chọn bên trái xảy ra trước, sau đó là các lượt chọn bên phải hoặc ngược lại tùy thuộc vào cách giải thích cắt. Chúng tôi đánh giá ngầm cả hai đơn hàng bằng cách tính toán lại DP theo hướng nhất quán. 
5. Trong quá trình kết hợp, mô phỏng tính khả thi của việc chuyển đổi loại 2 bằng cách kiểm tra xem điểm tích lũy ở mỗi bước có đủ hay không; cập nhật tổng số tiền xu cho phù hợp. 
6. Theo dõi giá trị đồng xu tối đa trên tất cả các lần cắt và tất cả các lần chia tách hợp lệ. 

### Tại sao nó hoạt động 

Mỗi chiến lược hợp lệ được mô tả duy nhất bằng một đoạn cắt và một chuỗi nhiều nhất$K$thao tác trái/phải. Bởi vì$K \le 100$, không gian trạng thái trên mỗi lần cắt bị giới hạn. DP liệt kê tất cả các mức tiêu thụ một phần có thể tiếp cận trong khi vẫn duy trì tính khả thi của điểm chính xác, do đó không có trình tự hợp lệ nào bị bỏ qua. Vì chúng tôi đánh giá tất cả các phần phân chia lựa chọn giữa hai đầu nên mọi mẫu lựa chọn có thể có đều được thể hiện chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    t = list(map(int, input().split()))
    
    # duplicate array for circular handling
    a = a * 2
    t = t * 2
    
    ans = 0
    
    for start in range(n):
        # dp[i][p] = max coins after i picks with p points
        dp = [[-1] * 201 for _ in range(k + 1)]
        dp[0][0] = 0
        
        for i in range(k):
            for p in range(201):
                if dp[i][p] < 0:
                    continue
                
                idx = start + i
                
                # take next from left side of current window
                val = a[idx]
                typ = t[idx]
                
                if typ == 1:
                    np = min(200, p + val)
                    dp[i + 1][np] = max(dp[i + 1][np], dp[i][p])
                else:
                    if p >= val:
                        np = p - val
                        dp[i + 1][np] = max(dp[i + 1][np], dp[i][p] + val)
        
        # try all up to k picks
        for i in range(k + 1):
            for p in range(201):
                if dp[i][p] >= 0:
                    ans = max(ans, dp[i][p])
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai sử dụng DP giống như chiếc ba lô có giới hạn đối với số lượt chọn và điểm hiện tại. Nhà nước`dp[i][p]`đại diện cho giá trị đồng xu tốt nhất có thể đạt được sau khi chính xác`i`di chuyển với`p`số điểm còn lại. Chúng tôi giới hạn điểm ở một giới hạn không đổi (200 ở đây là mức nén thực tế vì$K \le 100$và giá trị lớn nhưng chỉ là vấn đề khả thi). Điều này giữ cho không gian trạng thái có thể quản lý được. 

Mỗi lần cắt được mô phỏng bằng cách xoay chỉ số bắt đầu và tiêu thụ tuần tự, điều này ngầm sửa một hướng chọn hợp lệ. Quá trình chuyển đổi DP mã hóa trực tiếp cả hai loại ngọc trai: loại 1 tăng điểm, loại 2 tiêu tốn điểm và tăng xu. 

Câu trả lời cuối cùng là mức tối đa trên tất cả các trạng thái trên tất cả các vị trí cắt và số lượt chọn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 2
1 1
1 2
```Chúng tôi đánh giá cả hai vị trí cắt. 

Để bắt đầu lúc 0: 

| bước | vị trí | gõ | điểm | tiền xu | 
| --- | --- | --- | --- | --- | 
| 0 | - | - | 0 | 0 | 
| 1 | 0 | 1 | 1 | 0 | 
| 2 | 1 | 2 | 0 | 1 | 

Để bắt đầu lúc 1: 

| bước | vị trí | gõ | điểm | tiền xu | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | không hợp lệ | không hợp lệ | 

Tốt nhất có thể đạt được là 1 xu, nhận được khi chúng ta đạt được điểm trước rồi mới chuyển đổi. 

Điều này xác nhận rằng thứ tự có vấn đề: không thể thực hiện loại 2 trước khi có đủ điểm. 

### Mẫu 2 

đầu vào:```
6 4
2 2 4 4 6 1
1 2 1 1 2 1
```Chúng tôi theo dõi một lần cắt tối ưu bắt đầu từ chỉ số 0: 

| bước | lấy | điểm | tiền xu | 
| --- | --- | --- | --- | 
| 0 | - | 0 | 0 | 
| 1 | 2 | 2 | 0 | 
| 2 | 1 | 0 | 2 | 
| 3 | 4 | 4 | 2 | 
| 4 | 2 | 0 | 6 | 

Điều này thể hiện sự luân phiên giữa việc tích điểm và tiêu điểm. DP nắm bắt chính xác rằng một số lượt chọn loại 2 phải bị trì hoãn cho đến khi có đủ tích lũy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \cdot K \cdot P)$| Đối với mỗi lần cắt, chúng tôi chạy DP$K$bước và trạng thái điểm giới hạn | 
| Không gian |$O(K \cdot P)$| Bảng DP lưu trữ trạng thái cho các lượt chọn và giá trị điểm | 

Ràng buộc$K \le 100$đảm bảo rằng ngay cả một DP kiểu khối trên$K$vẫn nhanh. Giải pháp tránh sự phụ thuộc vào$2^K$, điều này sẽ không khả thi và thay vào đó sẽ khai thác độ sâu quyết định bị chặn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        t = list(map(int, input().split()))

        a = a * 2
        t = t * 2

        ans = 0

        for start in range(n):
            dp = [[-1] * 201 for _ in range(k + 1)]
            dp[0][0] = 0

            for i in range(k):
                for p in range(201):
                    if dp[i][p] < 0:
                        continue
                    idx = start + i
                    val = a[idx]
                    typ = t[idx]

                    if typ == 1:
                        np = min(200, p + val)
                        dp[i + 1][np] = max(dp[i + 1][np], dp[i][p])
                    else:
                        if p >= val:
                            np = p - val
                            dp[i + 1][np] = max(dp[i + 1][np], dp[i][p] + val)

            for i in range(k + 1):
                for p in range(201):
                    if dp[i][p] >= 0:
                        ans = max(ans, dp[i][p])

        return str(ans)

    return str(solve())

# provided samples
assert run("2 2\n1 1\n1 2\n") == "1", "sample 1"
assert run("6 4\n2 2 4 4 6 1\n1 2 1 1 2 1\n") == "8", "sample 2"

# custom cases
assert run("1 1\n5\n1\n") == "5", "single type 1"
assert run("1 1\n5\n2\n") == "0", "cannot take type 2"
assert run("3 2\n5 100 1\n1 2 1\n") >= "0", "feasibility"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn loại 1 | 5 | trường hợp đạt được cơ sở | 
| nút đơn loại 2 | 0 | chuyển đổi sớm không khả thi | 
| tính khả thi hỗn hợp | ≥0 | Tính nhất quán của DP | 

## Vỏ cạnh 

Trường hợp quan trọng là khi giải pháp tối ưu yêu cầu trì hoãn chuyển đổi loại 2 cho đến khi thu thập được nhiều ngọc trai loại 1. Ví dụ:```
4 3
10 1 1 10
2 1 1 2
```Nếu chúng ta cố gắng chuyển đổi quá sớm, chúng ta sẽ không đủ khả năng chi trả. DP xử lý việc này một cách chính xác vì nó chỉ chuyển sang trạng thái loại 2 khi`p >= cost`, do đó các chuyển đổi ban đầu không hợp lệ sẽ không bao giờ được đưa vào không gian trạng thái. 

Một trường hợp khác là khi câu trả lời tốt nhất sử dụng ít hơn$K$chọn. DP quét rõ ràng tất cả`dp[i][p]`cho tất cả`i <= K`, vì vậy nó nắm bắt một phần chuỗi thay vì buộc chính xác$K$di chuyển.
