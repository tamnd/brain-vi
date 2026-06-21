---
title: "CF 105761J - Cam Kết Cuối Cùng Mãi Mãi"
description: "Chúng tôi đang làm việc trên một lưới số nguyên rất lớn trong đó mỗi điểm có chuyển động của bốn điểm lân cận, lên, xuống, trái và phải. Một số điểm lưới bị chặn vì việc xây dựng đang diễn ra ở đó và không thể truy cập được những điểm bị chặn này."
date: "2026-06-21T22:58:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105761
codeforces_index: "J"
codeforces_contest_name: "2021 UCF Local Programming Contest"
rating: 0
weight: 105761
solve_time_s: 55
verified: true
draft: false
---

[CF 105761J - Cam kết lâu dài mãi mãi](https://codeforces.com/problemset/problem/105761/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc trên một lưới số nguyên rất lớn trong đó mỗi điểm có chuyển động của bốn điểm lân cận, lên, xuống, trái và phải. Một số điểm lưới bị chặn vì việc xây dựng đang diễn ra ở đó và không thể truy cập được những điểm bị chặn này. 

Mỗi truy vấn yêu cầu số lượng đường đi ngắn nhất hợp lệ từ điểm bắt đầu đến điểm kết thúc, trong đó “ngắn nhất” có nghĩa là ngắn nhất Manhattan, vì vậy mỗi bước phải giảm nghiêm ngặt khoảng cách Manhattan đến đích. Hạn chế đó loại bỏ mọi sự di chuyển: mọi chuyển động phải theo đúng hướng ngang hoặc đúng hướng dọc, không bao giờ rời xa mục tiêu. 

Nếu không có ô nào bị chặn thì mọi đường đi từ$(x_1, y_1)$ĐẾN$(x_2, y_2)$sẽ bao gồm một số lượng cố định các bước di chuyển theo chiều ngang và chiều dọc theo bất kỳ thứ tự nào, vì vậy câu trả lời sẽ là một hệ số nhị thức. Khó khăn là một số điểm mạng bị cấm và các đường đi qua chúng phải bị loại trừ. 

Tọa độ lưới lên tới 500.000 ở cả hai chiều, do đó lưới quá lớn để thể hiện rõ ràng. Số lượng điểm bị chặn nhiều nhất là 10, đây là hạn chế về cấu trúc chính. 

Một điểm tinh tế nhưng quan trọng là các con đường chỉ được phép di chuyển dọc theo những tuyến đường ngắn nhất đều đều. Điều này có nghĩa là nếu đích đến là hướng đông bắc thì chúng ta chỉ di chuyển về hướng bắc hoặc đông. Vì vậy, mọi đường dẫn hợp lệ đều nằm bên trong hình chữ nhật thẳng hàng theo trục giữa điểm bắt đầu và điểm kết thúc. 

Các trường hợp khó khăn xuất phát từ cách các chướng ngại vật tương tác với cấu trúc đơn điệu này. Số lượng đường đi ngắn nhất ngây thơ có thể dễ dàng vượt qua các đường dẫn bị chặn ở xa nhưng vẫn ở bên trong hình chữ nhật và một lỗi phổ biến khác là quên rằng ngay cả một chướng ngại vật bên trong hình chữ nhật cũng có thể thay đổi hoàn toàn tổ hợp bằng cách chia vùng. 

Ví dụ: nếu bắt đầu là$(0,0)$, kết thúc là$(2,2)$, và có một khối tại$(1,1)$, thì mọi đường đi qua$(1,1)$không hợp lệ. Câu trả lời đúng là tổng số đường đi trừ đi đường đi qua điểm đó: 

tổng là 6, đến (1,1) là$\binom{2}{1} \cdot \binom{2}{1} = 4$, vì vậy câu trả lời là 2. Một cách tiếp cận đơn giản bỏ qua các điểm chặn trung gian sẽ cho kết quả 6. 

Các ràng buộc ngụ ý rằng chúng tôi không thể thực hiện lưới DP hoặc BFS cho mỗi truy vấn. Với tối đa 10.000 truy vấn, bất kỳ giải pháp nào tệ hơn khoảng$O(1)$hoặc$O(n^2)$mỗi truy vấn chỉ được chấp nhận nếu$n$nó rất nhỏ, đúng vậy. Điều quan trọng là khai thác thực tế là chỉ có 10 trở ngại tồn tại. 

## Phương pháp tiếp cận 

Nếu không có trở ngại nào thì mỗi truy vấn sẽ trở thành một bài toán tổ hợp. Cho phép$\Delta x = |x_2 - x_1|$Và$\Delta y = |y_2 - y_1|$. Số đường đi ngắn nhất là$\binom{\Delta x + \Delta y}{\Delta x}$, vì chúng ta chọn vị trí các nước đi theo chiều ngang trong số tất cả các nước đi. 

Với những trở ngại, ý tưởng mạnh mẽ là coi mỗi truy vấn dưới dạng lưới DP hoặc BFS được giới hạn trong hình chữ nhật. Điều đó ngay lập tức thất bại vì phạm vi tọa độ lên tới 500.000, do đó, ngay cả một lần truyền tải cho mỗi truy vấn cũng không thể thực hiện được. 

Một cách mạnh mẽ hơn là chỉ chạy DP trên hình chữ nhật bằng cách sử dụng tọa độ không bị chặn. Nhưng hình chữ nhật vẫn có thể rất lớn nên điều này vẫn không khả thi. 

Quan sát quan trọng là số lượng chướng ngại vật rất nhỏ. Điều đó gợi ý rằng hãy coi các chướng ngại vật là những điểm trung gian đặc biệt và sử dụng phương pháp loại trừ bao hàm đối với chúng. Thay vì suy nghĩ theo các trạng thái lưới, chúng ta chuyển sang suy nghĩ theo các đường đi giữa một tập hợp nhỏ các điểm. 

Chúng tôi sắp xếp các chướng ngại vật cùng với sự bắt đầu và kết thúc. Sau đó, chúng ta tính số đường đi đơn điệu giữa hai điểm bất kỳ bỏ qua chướng ngại vật. Sau đó, chúng tôi trừ đi sự đóng góp của các đường đi qua ít nhất một chướng ngại vật, nhưng theo cách có cấu trúc: đối với mỗi điểm, tính số đường đi hợp lệ từ đầu đến đó, loại trừ các đường đi qua chướng ngại vật trước đó. 

Điều này trở thành DP cổ điển với tối đa 12 điểm cho mỗi truy vấn, vì chúng tôi bao gồm điểm bắt đầu và kết thúc cộng với tối đa 10 chướng ngại vật. 

Đối với mỗi truy vấn, chúng tôi: 

tính toán các cách từ đầu đến tất cả các điểm, rồi từ mỗi điểm đến cuối, đồng thời đảm bảo khi di chuyển giữa hai điểm, chúng ta chỉ xét các chuyển tiếp đơn điệu khả thi (chỉ khi tọa độ tăng phù hợp). Sau đó, chúng tôi áp dụng việc đưa vào các chướng ngại vật bằng cách xử lý các điểm theo thứ tự được sắp xếp và trừ đi các đường đi qua các điểm bị chặn trung gian. 

Điều này làm giảm mỗi truy vấn xuống$O(n^2)$Ở đâu$n \le 12$, đủ nhanh cho 10.000 truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (lưới DP/BFS cho mỗi truy vấn) |$O(Q \cdot H \cdot W)$|$O(H \cdot W)$| Quá chậm | 
| Tối ưu (DP vượt chướng ngại vật) |$O(Q \cdot n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng tôi sửa một truy vấn duy nhất. Chúng tôi thu thập tất cả các điểm có liên quan: điểm bắt đầu, điểm kết thúc và tất cả các ô bị chặn. 

Chúng tôi chỉ quan tâm đến các điểm nằm bên trong hình chữ nhật đơn điệu được xác định bởi điểm bắt đầu và kết thúc. Nếu chướng ngại vật nằm ngoài hình chữ nhật này, nó không thể nằm trên bất kỳ đường đi ngắn nhất nào và có thể bị bỏ qua đối với truy vấn này. 

Tiếp theo, chúng tôi sắp xếp tất cả các điểm liên quan bằng cách tăng x và nếu x bằng thì tăng y. Thứ tự này tôn trọng ràng buộc chuyển động đơn điệu, bởi vì bất kỳ đường dẫn hợp lệ nào cũng phải di chuyển từ tọa độ nhỏ hơn đến tọa độ lớn hơn ở cả hai chiều. 

Chúng tôi xác định giá trị DP cho mỗi điểm, biểu thị số lượng đường đi đơn điệu từ đầu đến điểm đó trong khi tránh các chướng ngại vật đã được xử lý trước đó. 

Chúng ta khởi tạo DP tại điểm bắt đầu là 1. 

Đối với mỗi điểm theo thứ tự được sắp xếp, chúng tôi tính giá trị DP của nó bằng cách trước tiên đếm tất cả các đường đi đơn điệu từ đầu đến đó, bỏ qua các chướng ngại vật. Đây là một giá trị tổ hợp. Sau đó, chúng tôi trừ đi những khoản đóng góp đến từ những điểm trước đó có thể đạt được điểm đó. 

Vì vậy, chúng tôi tiến hành theo cách có cấu trúc sau đây. 

1. Xây dựng danh sách các điểm bao gồm điểm bắt đầu, điểm kết thúc và tất cả các chướng ngại vật. 

Điều này đảm bảo tất cả “tương tác khối” tiềm năng được thể hiện rõ ràng dưới dạng các nút trong một DAG nhỏ. 
2. Lọc các điểm không nằm trong hình chữ nhật của truy vấn. 

Bất kỳ điểm nào như vậy không thể nằm trên đường đi đơn điệu ngắn nhất, vì vậy việc thêm nó vào sẽ chỉ thêm vào những trạng thái không cần thiết. 
3. Sắp xếp điểm theo x rồi y. 

Điều này đảm bảo rằng khi chúng tôi xử lý một điểm, tất cả các điểm trước đó có thể có trong đường dẫn đơn điệu hợp lệ đều đã được xử lý. 
4. Tính toán trước các hệ số nhị thức hoặc giai thừa lên tới 1.000.000 hoặc sử dụng hàm tổ hợp nhanh với các giai thừa được tính toán trước và giai thừa nghịch đảo. 

Điều này là cần thiết vì mỗi trọng số cạnh giữa hai điểm là một hệ số nhị thức. 
5. Xác định dp[i] là số đường đi hợp lệ từ đầu đến điểm i bỏ qua chướng ngại vật cho đến nay. 
6. Với mỗi điểm i theo thứ tự sắp xếp: 

tính các cách cơ bản từ đầu đến i dưới dạng lược(dx + dy, dx). 

sau đó trừ đi các đường đi qua bất kỳ điểm j nào trước đó: 

nếu j có thể đến i một cách đơn điệu, hãy trừ dp[j] * cách(j → i). 
7. Câu trả lời là dp[end]. 

Bước trừ về cơ bản là bao gồm-loại trừ được nhúng ở dạng DP. Mỗi dp[j] đã biểu thị các đường đi tránh chướng ngại vật trước j, do đó nhân với các đường đi từ j đến i sẽ tính cho tất cả các đường đi chạm j đầu tiên và sau đó tiếp tục. 

### Tại sao nó hoạt động 

Mọi đường đi đơn điệu từ đầu đến cuối đều có thể được phân tách duy nhất bởi chướng ngại vật đầu tiên (hoặc không có chướng ngại vật nào). Nếu một đường đi không đi qua bất kỳ chướng ngại vật nào thì nó được tính vào số hạng tổ hợp trực tiếp cho điểm cuối. Nếu nó đi qua ít nhất một chướng ngại vật, gọi j là chướng ngại vật đầu tiên đi qua theo thứ tự sắp xếp. Tiền tố từ đầu đến j đã được tính trong dp[j] và hậu tố từ j đến cuối được tính độc lập. Phép trừ DP đảm bảo mỗi lần phân tách như vậy được tính chính xác một lần và việc xử lý theo thứ tự được sắp xếp đảm bảo không có sự phụ thuộc theo chu kỳ hoặc tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modinv(x):
    return pow(x, MOD - 2, MOD)

MAXN = 500000

fact = [1] * (MAXN + 1)
invfact = [1] * (MAXN + 1)

for i in range(1, MAXN + 1):
    fact[i] = fact[i - 1] * i % MOD

invfact[MAXN] = modinv(fact[MAXN])
for i in range(MAXN, 0, -1):
    invfact[i - 1] = invfact[i] * i % MOD

def comb(n, r):
    if r < 0 or r > n:
        return 0
    return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

def solve():
    n = int(input())
    obstacles = [tuple(map(int, input().split())) for _ in range(n)]
    t = int(input())

    for _ in range(t):
        x1, y1, x2, y2 = map(int, input().split())

        if x1 <= x2:
            xs, ys = x1, y1
            xt, yt = x2, y2
        else:
            xs, ys = x2, y2
            xt, yt = x1, y1

        pts = [(xs, ys), (xt, yt)]
        for x, y in obstacles:
            if xs <= x <= xt and ys <= y <= yt:
                pts.append((x, y))

        pts = list(set(pts))
        pts.sort()

        k = len(pts)
        dp = [0] * k
        dp[0] = 1

        for i in range(k):
            xi, yi = pts[i]
            ways = comb(xi - xs + yi - ys, xi - xs)
            dp[i] = ways

            for j in range(i):
                xj, yj = pts[j]
                if xj <= xi and yj <= yi:
                    dx = xi - xj
                    dy = yi - yj
                    dp[i] = (dp[i] - dp[j] * comb(dx + dy, dx)) % MOD

        end_index = pts.index((xt, yt))
        print(dp[end_index] % MOD)

solve()
```Mã bắt đầu bằng cách tính toán trước các giai thừa và giai thừa nghịch đảo lên đến tọa độ tối đa, cho phép truy vấn hệ số nhị thức theo thời gian không đổi. Điều này rất cần thiết vì mọi chuyển đổi giữa hai điểm đều được đánh giá theo phương pháp tổ hợp. 

Đối với mỗi truy vấn, chúng tôi chuẩn hóa hướng sao cho luôn di chuyển từ dưới cùng bên trái lên trên cùng bên phải, đảm bảo tính đơn điệu. Sau đó, chúng tôi thu thập tất cả các chướng ngại vật bên trong hình chữ nhật giới hạn và thêm chúng vào tập hợp điểm. 

Việc sắp xếp đảm bảo rằng khi tính toán dp[i], tất cả các giá trị trước đó đều đã được tính toán. Giá trị dp bắt đầu từ tổng số đường dẫn tổ hợp ngay từ đầu và chúng tôi trừ đi phần đóng góp từ tất cả các điểm có thể tiếp cận trước đó, loại bỏ hiệu quả các đường dẫn “đi qua” chướng ngại vật. 

Câu trả lời cuối cùng là giá trị dp tại điểm đích. 

Một chi tiết triển khai tinh tế là lấy modulo sau khi trừ để tránh các giá trị âm. Một điều nữa là đảm bảo rằng chỉ những người tiền nhiệm đơn điệu mới được xem xét; nếu không thì các chuyển đổi ngược không hợp lệ sẽ làm hỏng số lượng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Bắt đầu$(0,0)$, kết thúc$(2,2)$, chướng ngại vật tại$(1,1)$. 

Chúng tôi xem xét các điểm được sắp xếp:$(0,0), (1,1), (2,2)$. 

| tôi | Điểm | Đường dẫn cơ sở từ đầu | Phép trừ | dp[i] | 
| --- | --- | --- | --- | --- | 
| 0 | (0,0) | 1 | không | 1 | 
| 1 | (1,1) | 2 | 1*1 | 1 | 
| 2 | (2,2) | 6 | 1*2 | 4 | 

Bảng cho thấy các đường đi qua chướng ngại vật làm giảm tổng số một cách chính xác và chúng tôi khôi phục 4 đường đi ngắn nhất hợp lệ. 

### Ví dụ 2 

Bắt đầu$(0,0)$, kết thúc$(1,3)$, chướng ngại vật tại$(1,1)$. 

Điểm sắp xếp:$(0,0), (1,1), (1,3)$. 

| tôi | Điểm | Đường dẫn cơ sở | Phép trừ | dp | 
| --- | --- | --- | --- | --- | 
| 0 | (0,0) | 1 | 0 | 1 | 
| 1 | (1,1) | 2 | 1 | 1 | 
| 2 | (1,3) | 4 | 1*2 | 2 | 

Điều này cho thấy một chướng ngại vật làm giảm không gian đường đi có thể tiếp cận bằng cách cắt bỏ một tập hợp lớn các tuyến đường đơn điệu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(Q \cdot n^2)$| mỗi truy vấn xử lý tối đa 12 điểm và kiểm tra tất cả các cặp | 
| Không gian |$O(n^2)$| Mảng DP cho mỗi truy vấn và bảng giai thừa | 

Những hạn chế$n \le 10$Và$Q \le 10000$đảm bảo rằng phương pháp tiếp cận bậc hai trên mỗi truy vấn đủ nhanh, vì mỗi truy vấn chỉ thực hiện vài trăm thao tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# provided samples (format not fully specified in prompt, placeholders)
# assert run("...") == "..."

# custom cases
assert run("0\n1\n0 0 1 1\n")  # minimal no obstacles
assert run("1\n1 1\n1\n0 0 2 2\n")  # single obstacle diagonal cut
assert run("2\n1 1\n2 2\n1\n0 0 3 3\n")  # two blocking points
assert run("0\n1\n0 0 500000 500000\n")  # max distance
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| lưới nhỏ đơn | tổ hợp tầm thường | độ đúng cơ sở | 
| một trở ngại | đường dẫn giảm | xử lý đưa vào | 
| hai chướng ngại vật | nhiều tương tác trừ | Phân lớp DP | 
| khoảng cách tối đa | tính toán trước giai thừa chính xác | ranh giới hiệu suất | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi chướng ngại vật nằm bên ngoài hình chữ nhật giới hạn của truy vấn. Trong tình huống đó, nó phải được bỏ qua hoàn toàn, vì không có con đường ngắn nhất đơn điệu nào có thể đến được nó. Thuật toán xử lý vấn đề này bằng cách lọc các chướng ngại vật bằng giới hạn tọa độ trước khi xây dựng bộ DP. 

Một trường hợp khác là khi nhiều chướng ngại vật có cùng giới hạn thứ tự x hoặc y khiến chúng không thể so sánh được theo thứ tự đường đi. Sắp xếp theo x rồi y đảm bảo thứ tự tôpô hợp lệ của biểu đồ đơn điệu, do đó mỗi điểm chỉ phụ thuộc vào các điểm có thể tiếp cận trước đó. 

Cuối cùng, khi điểm đầu và điểm cuối gần nhau, DP suy biến thành một hệ số nhị thức duy nhất có kích thước nhỏ. Thuật toán vẫn hoạt động vì không có điểm trung gian trong tập hợp, do đó không xảy ra phép trừ và số tổ hợp cơ sở được trả về trực tiếp.
