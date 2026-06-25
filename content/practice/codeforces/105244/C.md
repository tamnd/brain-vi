---
title: "CF 105244C - Thám hiểm không gian"
description: "Một nhà thám hiểm không gian ghé thăm một chuỗi cố định các thiên thể. Mỗi đồ vật đều mang lại một số giá trị khoa học nếu được nghiên cứu nhưng cũng tiêu tốn hai nguồn tài nguyên có hạn: năng lượng và thời gian."
date: "2026-06-24T07:31:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105244
codeforces_index: "C"
codeforces_contest_name: "Dynamic Programming, SPbSU 2024, Training 2"
rating: 0
weight: 105244
solve_time_s: 52
verified: true
draft: false
---

[CF 105244C - Thám hiểm không gian](https://codeforces.com/problemset/problem/105244/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Một nhà thám hiểm không gian ghé thăm một chuỗi cố định các thiên thể. Mỗi đồ vật đều mang lại một số giá trị khoa học nếu được nghiên cứu nhưng cũng tiêu tốn hai nguồn tài nguyên có hạn: năng lượng và thời gian. Người khám phá phải xử lý các đối tượng theo thứ tự nhất định và quyết định xem nên nghiên cứu hay bỏ qua đối tượng đó. Sau khi được chọn, một đối tượng sẽ tiêu thụ hết năng lượng và thời gian cần thiết. 

Mục tiêu là chọn một dãy con của các đối tượng này, tôn trọng thứ tự cố định, sao cho tổng năng lượng sử dụng không vượt quá K và tổng thời gian không vượt quá M, đồng thời tối đa hóa tổng giá trị khoa học. 

Đầu ra là gấp đôi. Đầu tiên, chúng tôi báo cáo giá trị tối đa có thể đạt được. Thứ hai, chúng tôi đưa ra các chỉ số của các đối tượng được chọn theo thứ tự truy cập tăng dần. 

Các ràng buộc đủ chặt chẽ để đề xuất một phương pháp lập trình động. Với N lên tới 100 và cả hai giới hạn tài nguyên cũng tối đa là 100, không gian trạng thái theo dõi lượng năng lượng và thời gian đã được sử dụng cho mỗi tiền tố của đối tượng vẫn còn nhỏ. Một phép liệt kê tập hợp con đơn giản sẽ yêu cầu kiểm tra tất cả các tập hợp con 2^N, điều này đã trở nên không khả thi ở khoảng N=40 và ở đây N đạt tới 100, do đó việc tìm kiếm theo cấp số nhân ngay lập tức bị loại trừ. 

Một vấn đề tế nhị phát sinh từ ràng buộc đặt hàng. Đây không phải là một chiếc ba lô tiêu chuẩn nơi các vật phẩm có thể được sắp xếp lại một cách tự do, nhưng quyết định vẫn chỉ phụ thuộc vào việc lấy hay bỏ qua từng vật phẩm theo trình tự, điều này vẫn giữ nguyên cấu trúc DP cổ điển. 

Các trường hợp cạnh xuất hiện khi không thể lấy đối tượng nào trong giới hạn. Ví dụ: nếu K=5 và M=5 và mọi đối tượng cần ít nhất 6 năng lượng hoặc 6 thời gian thì không có lựa chọn nào hợp lệ. Trong trường hợp đó, đầu ra đúng là một số 0, không phải là một chuỗi trống hoặc giá trị 0 theo sau là một dòng trống. Cách tiếp cận tái thiết đơn giản luôn in tiêu đề trình tự có thể dễ dàng định dạng sai trong trường hợp này. 

Một trường hợp góc khác là khi tồn tại nhiều giải pháp tối ưu. Vì bất kỳ trình tự hợp lệ nào cũng có thể được chấp nhận nên DP không cần thực thi thứ tự từ điển nhưng việc tái cấu trúc vẫn phải nhất quán với các quyết định được lưu trữ. 

## Phương pháp tiếp cận 

Chiến lược brute-force là thử mọi tập hợp con của các đối tượng trong khi vẫn duy trì trật tự một cách ngầm định bằng cách kiểm tra các mặt nạ bao hàm. Đối với mỗi tập hợp con, chúng tôi tính tổng năng lượng, thời gian và giá trị, sau đó xác minh tính khả thi. Điều này hoạt động về mặt khái niệm vì nó khớp trực tiếp với định nghĩa vấn đề, nhưng nó yêu cầu đánh giá 2^N tập hợp con. Với N=100, điều này có nghĩa là có khoảng 10^30 ứng viên, vượt xa mọi giới hạn tính toán. 

Cấu trúc của vấn đề cho thấy rằng sự tương tác duy nhất giữa các quyết định là thông qua việc sử dụng tài nguyên tích lũy. Mỗi đối tượng đóng góp độc lập về giá trị, năng lượng và thời gian và sau khi được xử lý, nó không ảnh hưởng đến quá trình chuyển đổi trong tương lai ngoại trừ thông qua dung lượng còn lại. Tính độc lập này gợi ý một công thức lập trình động dựa trên các tiền tố và ngân sách tài nguyên. 

Tại mỗi đối tượng, chúng ta quyết định bỏ qua hay lấy nó, và nếu lấy đi, chúng ta sẽ giảm cả hai năng lực còn lại. Điều này đương nhiên dẫn đến DP ba chiều về chỉ số, năng lượng sử dụng và thời gian sử dụng. Vì N, K và M đều có nhiều nhất là 100 nên tổng số trạng thái là khoảng một triệu và mỗi lần chuyển đổi trong thời gian không đổi, điều này rất hiệu quả. 

Để xây dựng lại trình tự đã chọn, chúng tôi lưu trữ các quyết định trong quá trình chuyển đổi DP, ghi nhớ liệu trạng thái đến từ việc lấy hay bỏ qua mục hiện tại. Quay lui từ trạng thái cuối cùng với giá trị tối đa mang lại chuỗi yêu cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^N · N) | O(N) | Quá chậm | 
| DP tối ưu | O(N · K · M) | O(N · K · M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định dp[i][j][k] là giá trị tối đa có thể đạt được bằng cách sử dụng i mục đầu tiên trong khi tiêu tốn chính xác j năng lượng và k thời gian hoặc coi các trạng thái không thể truy cập là vô cực âm.

1. Khởi tạo tất cả các trạng thái dp là không thể truy cập được ngoại trừ dp[0][0][0] bằng 0. Điều này thể hiện việc bắt đầu mà không có mục nào và không sử dụng tài nguyên. 
2. Lặp lại các mục từ 1 đến N. Đối với mỗi mục, chúng tôi truyền bá các chuyển đổi từ tất cả các trạng thái có thể truy cập của lớp trước đó. 
3. Đối với mỗi trạng thái dp[i−1][j][k], trước tiên chúng ta xem xét bỏ qua mục hiện tại. Chúng tôi cập nhật dp[i][j][k] với cùng giá trị nếu nó cải thiện kết quả. Điều này bảo tồn tính khả thi mà không tiêu tốn tài nguyên. 
4. Chúng tôi cũng xem xét lấy hạng mục hiện tại, với điều kiện j + energy[i] ≤ K và k + time[i] ≤ M. Chúng tôi cập nhật dp[i][j + F][k + T] với dp[i−1][j][k] + V[i]. Điều này mã hóa quyết định bao gồm mục. 
5. Bên cạnh các bản cập nhật dp, chúng tôi lưu trữ một con trỏ gốc ghi lại trạng thái đến từ việc bỏ qua hay lấy và trạng thái đó bắt nguồn từ (j, k) trước đó. 
6. Sau khi xử lý tất cả các mục, chúng tôi quét tất cả dp[N][j][k] để tìm giá trị lớn nhất và trạng thái tương ứng của nó. 
7. Chúng tôi xây dựng lại đường dẫn bằng cách di chuyển lùi qua các con trỏ cha được lưu trữ cho đến khi đạt i = 0, thu thập tất cả các chỉ số trong đó quyết định lấy mục. 

Tại sao nó hoạt động xuất phát từ thực tế là mọi trạng thái (i, j, k) tóm tắt tất cả các cách có thể để đạt được chính xác mức sử dụng tài nguyên đó bằng cách sử dụng i mục đầu tiên. Bất kỳ giải pháp tối ưu nào cũng phải tương ứng với một số trạng thái hợp lệ trong không gian này và các chuyển đổi liệt kê tất cả các quyết định pháp lý mà không bỏ sót hoặc trùng lặp. Vì mỗi lần chuyển đổi duy trì tính chính xác của giá trị tích lũy và tôn trọng các ràng buộc, nên mức tối đa trên lớp cuối cùng phải tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, K, M = map(int, input().split())
    items = []
    for _ in range(N):
        v, f, t = map(int, input().split())
        items.append((v, f, t))

    NEG = -10**18
    dp = [[[NEG] * (M + 1) for _ in range(K + 1)] for _ in range(N + 1)]
    parent = [[[-1] * (M + 1) for _ in range(K + 1)] for _ in range(N + 1)]
    take = [[[-1] * (M + 1) for _ in range(K + 1)] for _ in range(N + 1)]

    dp[0][0][0] = 0

    for i in range(1, N + 1):
        v, f, t = items[i - 1]
        for j in range(K + 1):
            for k in range(M + 1):
                if dp[i - 1][j][k] == NEG:
                    continue

                if dp[i][j][k] < dp[i - 1][j][k]:
                    dp[i][j][k] = dp[i - 1][j][k]
                    parent[i][j][k] = (j, k)
                    take[i][j][k] = 0

                nj, nk = j + f, k + t
                if nj <= K and nk <= M:
                    if dp[i][nj][nk] < dp[i - 1][j][k] + v:
                        dp[i][nj][nk] = dp[i - 1][j][k] + v
                        parent[i][nj][nk] = (j, k)
                        take[i][nj][nk] = 1

    best_val = 0
    bj = bk = 0
    for j in range(K + 1):
        for k in range(M + 1):
            if dp[N][j][k] > best_val:
                best_val = dp[N][j][k]
                bj, bk = j, k

    if best_val == 0:
        print(0)
        return

    res = []
    i, j, k = N, bj, bk
    while i > 0:
        pj, pk = parent[i][j][k]
        if take[i][j][k] == 1:
            res.append(i)
        j, k = pj, pk
        i -= 1

    res.reverse()
    print(best_val)
    print(*res)

if __name__ == "__main__":
    solve()
```Bảng DP được xây dựng từng lớp trên các mục, đảm bảo rằng mỗi trạng thái chỉ phụ thuộc vào chỉ mục mục trước đó. Quá trình chuyển đổi bỏ qua sao chép các giá trị trước đó về phía trước, trong khi quá trình chuyển đổi lấy sẽ thêm giá trị và tăng cả hai bộ đếm tài nguyên. 

Mảng cha và mảng lấy lưu trữ đủ thông tin để xây dựng lại chuỗi con đã chọn. Quá trình xây dựng lại đi lùi từ trạng thái cuối cùng tốt nhất cho đến khi đạt đến tiền tố trống, thu thập các chỉ mục nơi cờ lấy được đặt. 

Một điểm tinh tế là chúng tôi chỉ khởi tạo dp[0] [0] [0] và coi tất cả những thứ khác là không thể truy cập được, điều này ngăn các trạng thái một phần không hợp lệ ảnh hưởng đến quá trình chuyển đổi. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ với ba đối tượng và các ràng buộc vừa phải. Giả sử giải pháp tối ưu là lấy đối tượng thứ nhất và thứ ba nhưng bỏ qua đối tượng thứ hai do áp lực về tài nguyên. 

Chúng tôi theo dõi trạng thái dp về mặt khái niệm cho một đường dẫn hợp lệ: 

| Bước | Mục | Trạng thái (năng lượng, thời gian) | Giá trị | Hành động | 
| --- | --- | --- | --- | --- | 
| 0 | - | (0, 0) | 0 | bắt đầu | 
| 1 | 1 | (20, 3) | 100 | lấy | 
| 2 | 2 | (20, 3) | 100 | bỏ qua | 
| 3 | 3 | (30, 7) | 160 | lấy | 

Dấu vết này cho thấy cách bỏ qua để duy trì trạng thái, cho phép lấy các mục sau khi các mục trước đó có dung lượng bị chặn. 

Bây giờ hãy xem xét trường hợp tất cả các mục vượt quá ràng buộc. 

đầu vào:```
2 5 5
10 6 1
20 1 6
```Ở đây không có mục nào phù hợp một mình. DP không bao giờ cải thiện từ dp[0] [0] [0], vì vậy giá trị tốt nhất vẫn bằng 0 và không thể tái thiết được. 

| Bước | Mục | Tiểu bang | Giá Trị Tốt Nhất | 
| --- | --- | --- | --- | 
| 0 | - | (0,0) | 0 | 
| 1 | 1 | không hợp lệ | 0 | 
| 2 | 2 | không hợp lệ | 0 | 

Điều này xác nhận rằng thuật toán tránh được việc chọn các mục không khả thi một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N · K · M) | Mỗi mục trong số N mục xử lý tất cả trạng thái K·M một lần với các chuyển đổi liên tục | 
| Không gian | O(N · K · M) | Bảng DP đầy đủ và bảng tái cấu trúc lưu trữ trạng thái cho mỗi tiền tố | 

Tổng số thao tác là khoảng một triệu, vừa vặn thoải mái trong giới hạn cho N, K, M lên tới 100. Việc sử dụng bộ nhớ cũng an toàn vì vài triệu số nguyên nằm trong giới hạn 512 MiB nhất định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return capture(solve)

def capture(func):
    import sys, io
    old = sys.stdout
    sys.stdout = io.StringIO()
    func()
    out = sys.stdout.getvalue()
    sys.stdout = old
    return out.strip()

# minimum case
assert run("""1 10 10
5 3 3
""") == "5\n1"

# no valid items
assert run("""2 5 5
10 6 1
20 1 6
""") == "0"

# choose all items
assert run("""2 10 10
3 4 4
2 3 3
""") == "5\n1 2"

# must skip due to constraints
assert run("""3 10 10
8 6 6
7 6 6
10 4 4
""") == "10\n3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mục duy nhất phù hợp | giá trị + chỉ số | trường hợp cơ sở đúng đắn | 
| không có mục nào khả thi | 0 | xử lý giải pháp trống | 
| tất cả các mục phù hợp | trình tự đầy đủ | tích lũy đúng đắn | 
| bỏ qua có chọn lọc | trình tự một phần | Logic quyết định DP | 

## Vỏ cạnh 

Khi mọi vật phẩm đều quá đắt về năng lượng hoặc thời gian, DP sẽ không bao giờ vượt quá trạng thái ban đầu. Thuật toán phát hiện chính xác điều này vì giá trị tốt nhất vẫn bằng 0 và không tồn tại con trỏ gốc cho các mục đã lấy. Đầu ra trở thành một số 0 duy nhất, phù hợp với định dạng được yêu cầu. 

Khi tồn tại nhiều đường dẫn tối ưu, ví dụ như hai tập hợp con khác nhau mang lại cùng một giá trị tối đa dưới các ràng buộc giống hệt nhau, việc xây dựng lại sẽ tuân theo bất kỳ chuyển đổi nào được ghi lần cuối vào bảng DP. Vì bài toán cho phép bất kỳ câu trả lời hợp lệ nào, nên tính không tất định này có thể chấp nhận được và vẫn tạo ra một dãy con đúng. 

Khi K hoặc M bằng 0, chỉ những mục có mức tiêu thụ tài nguyên bằng 0 mới có thể được chọn. Vì các ràng buộc đảm bảo F ≥ 1 và T ≥ 1, nên kết quả duy nhất có thể xảy ra là giá trị 0, giá trị này thuật toán xử lý một cách tự nhiên thông qua trạng thái khởi tạo.
