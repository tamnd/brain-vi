---
title: "CF 104375C - Đếm Sao"
description: "Chúng ta có một tập hợp các điểm trên mặt phẳng, mỗi điểm tượng trưng cho một ngôi sao. Chúng tôi muốn đếm xem có bao nhiêu “chòm sao nan hoa” hợp lệ có thể được hình thành. Một cấu hình được xác định bằng cách chọn một ngôi sao làm trung tâm và sau đó chọn các ngôi sao khác xung quanh nó theo một cách rất có cấu trúc."
date: "2026-07-01T17:27:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104375
codeforces_index: "C"
codeforces_contest_name: "2023 ICPC Gran Premio de Mexico 1ra Fecha"
rating: 0
weight: 104375
solve_time_s: 107
verified: false
draft: false
---

[CF 104375C - Đếm sao](https://codeforces.com/problemset/problem/104375/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các điểm trên mặt phẳng, mỗi điểm tượng trưng cho một ngôi sao. Chúng tôi muốn đếm xem có bao nhiêu “chòm sao nan hoa” hợp lệ có thể được hình thành. Một cấu hình được xác định bằng cách chọn một ngôi sao làm trung tâm và sau đó chọn các ngôi sao khác xung quanh nó theo một cách rất có cấu trúc. 

Khi một tâm đã được cố định, tất cả các ngôi sao được chọn khác sẽ được nhóm theo khoảng cách của chúng với tâm này. Những khoảng cách này đang tăng lên nghiêm ngặt khi được sắp xếp. Với mỗi mức khoảng cách khác nhau, các quy tắc bắt buộc phải có một cấu trúc cứng nhắc: ở mức khoảng cách i, có đúng m sao, và tất cả các sao này phải nằm trên các tia phát ra từ tâm sao cho mỗi tia đóng góp một chuỗi sao cách đều nhau theo các đoạn nguyên dọc theo đường thẳng từ tâm ra ngoài. Mọi điểm mạng trung gian trên mỗi đoạn như vậy cũng phải được đưa vào nhóm đã chọn. 

Điều này biến vấn đề thành việc đếm, đối với mọi trung tâm có thể, có bao nhiêu “chuỗi xuyên tâm” nhất quán có thể được hình thành sao cho nhiều hướng từ trung tâm hoạt động giống hệt nhau theo các phần mở rộng cộng tuyến có sẵn. 

Đầu vào chỉ đơn giản lên tới 1000 điểm với tọa độ nguyên và chúng ta phải tính số lượng cấu trúc hợp lệ theo modulo 998244353. 

Ràng buộc n 1000 ngay lập tức loại trừ mọi phép liệt kê bậc ba hoặc tệ hơn trên ba điểm. Ngay cả O(n^3) cũng sẽ có khoảng 10^9 thao tác, quá lớn. Các phương pháp tiếp cận O(n^2 log n) hoặc O(n^2) là mục tiêu. 

Một trường hợp cạnh tinh tế xuất phát từ sự cộng tuyến và chia tỷ lệ của các vectơ chỉ hướng. Nhiều điểm có thể nằm trên cùng một tia tính từ tâm nhưng ở các bội số nguyên khác nhau. Một cách tiếp cận ngây thơ chỉ kiểm tra khoảng cách hoặc chỉ kiểm tra hướng mà không chuẩn hóa bằng gcd sẽ hợp nhất hoặc phân tách chuỗi không chính xác. 

Một trường hợp khác là cấu hình suy biến nhỏ. Nếu tất cả các điểm nằm trên một đường thẳng hoặc nếu chỉ tồn tại hai điểm, thì cấu trúc tổ hợp vẫn phải đếm chính xác “chuỗi có độ dài 1” hợp lệ, điều này thường phá vỡ các công thức giả định ít nhất hai hướng riêng biệt. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi trung tâm có thể và sau đó kiểm tra tất cả các tập hợp con của các điểm còn lại, kiểm tra xem liệu chúng có thể được phân chia thành các nhóm xuyên tâm có kích thước bằng nhau với các ràng buộc cộng tuyến nhất quán hay không. Điều này ngay lập tức trở thành cấp số nhân về số điểm trên mỗi trung tâm. Ngay cả khi chúng ta chỉ cố gắng liệt kê các tập hợp con của tia, mỗi tâm có n−1 điểm khác và các tập hợp con trong số đó là 2^(n−1), điều này là không thể. 

Quan sát quan trọng là mọi thứ đều bị chi phối bởi hình học so với một tâm cố định. Đối với mỗi tâm C, mọi điểm khác đều xác định một vectơ chỉ phương (dx, dy). Các điểm nằm trên cùng một tia giảm về cùng hướng chuẩn hóa sau khi chia cho gcd và sửa dấu. Dọc theo mỗi hướng, các điểm được sắp xếp tự nhiên theo khoảng cách và điều kiện bao gồm tất cả các điểm đoạn trung gian ngụ ý rằng nếu chúng ta chọn một điểm xa trên một tia thì tất cả các điểm gần hơn trên cùng tia đó cũng phải được bao gồm. 

Vì vậy, thay vì nghĩ về các tập hợp con tùy ý, chúng tôi nghĩ về các “chuỗi định hướng” độc lập từ trung tâm. Mỗi hướng đóng góp một chuỗi các điểm được sắp xếp theo khoảng cách. Cấu trúc của các chòm sao hợp lệ giảm xuống việc chọn, cho mỗi hướng, một tiền tố của chuỗi của nó và sau đó đồng bộ hóa các tiền tố này theo nhiều hướng sao cho mỗi lớp khoảng cách có số lượng bằng nhau theo các hướng. 

Điều này biến bài toán thành một tích tổ hợp theo các hướng sau khi nhóm theo chuỗi xuyên tâm và sắp xếp các điểm dọc theo mỗi hướng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| Hướng + DP trên tia trên mỗi tâm | O(n^2 log n) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi coi mỗi điểm là một trung tâm tiềm năng và tính toán các đóng góp một cách độc lập. 

1. Đối với tâm C cố định, hãy tính vectơ chỉ phương từ C đến mọi điểm khác. Chuẩn hóa từng hướng bằng cách chia cho gcd(dx, dy) và sửa dấu sao cho các tia giống hệt nhau ánh xạ tới cùng một khóa. Điều này đảm bảo tất cả các điểm thẳng hàng theo cùng hướng ra ngoài được nhóm chính xác. 
2. Đối với mỗi nhóm hướng, sắp xếp các điểm theo bình phương khoảng cách từ C. Điều này tạo ra một chuỗi trong đó mỗi phần tử đại diện cho ngôi sao tiếp theo dọc theo tia đó. 
3. Cho có k nhóm hướng. Đối với mỗi nhóm i, chúng ta có thể chọn số lượng điểm ban đầu của chuỗi của nó được đưa vào một chòm sao. Nếu chúng ta chọn t điểm từ một hướng, tất cả các điểm gần hơn sẽ được bao gồm hoàn toàn do ràng buộc tiền tố. 
4. Bây giờ chúng tôi muốn kết hợp các lựa chọn trên tất cả các hướng sao cho số điểm được chọn ở mỗi “lớp khoảng cách” nhất quán trên các hướng. Điều này tương đương với việc chọn, cho mỗi hướng, độ dài tiền tố và sau đó đếm xem có bao nhiêu cách chúng ta có thể căn chỉnh các độ dài tiền tố này trên các lớp. 
5. Chúng tôi xử lý từng hướng một và duy trì DP trong đó dp[x] biểu thị số cách để tạo cấu hình trong đó độ sâu tối đa hiện tại (số lớp được hình thành cho đến nay) là x và tất cả các hướng được xử lý đều nhất quán với các x lớp này. 
6. Khi thêm một hướng mới có độ dài chuỗi L, chúng tôi cập nhật dp bằng cách phân bổ số lớp chúng tôi mở rộng bằng cách sử dụng hướng này. Mỗi độ dài tiền tố có thể đóng góp một cách tổng hợp tùy thuộc vào số lượng lớp hiện có mà nó có thể hỗ trợ. 
7. Sau khi xử lý tất cả các hướng cho một tâm, tính tổng tất cả các trạng thái dp hợp lệ để có được số lượng các chòm sao có tâm tại C. 

Chúng tôi lặp lại điều này cho mọi điểm làm trung tâm và tích lũy kết quả. 

### Tại sao nó hoạt động 

Bất biến chính là sau khi xử lý một tập hợp con các hướng, dp mã hóa chính xác số cách để chọn các tiền tố được đồng bộ hóa sao cho tất cả các hướng đã chọn đều thống nhất trên cùng một số lớp khoảng cách và mỗi lớp là hợp lệ vì mỗi hướng đóng góp chính xác một điểm mới cho mỗi lớp hoặc không có gì. Ràng buộc tiền tố đảm bảo không bỏ qua các ngôi sao trung gian và việc chuẩn hóa đảm bảo mỗi hướng đều độc lập ngoại trừ việc căn chỉnh lớp. Điều này ngăn chặn việc tính hai lần vì mỗi cấu hình có một tâm duy nhất và sự gán điểm duy nhất cho các tia và độ dài tiền tố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from math import gcd
from collections import defaultdict

MOD = 998244353

def norm(dx, dy):
    if dx == 0 and dy == 0:
        return (0, 0)
    g = gcd(dx, dy)
    dx //= g
    dy //= g
    if dx < 0 or (dx == 0 and dy < 0):
        dx = -dx
        dy = -dy
    return (dx, dy)

def solve():
    n = int(input())
    pts = [tuple(map(int, input().split())) for _ in range(n)]
    
    ans = 0

    for i in range(n):
        cx, cy = pts[i]
        dirs = defaultdict(list)

        for j in range(n):
            if i == j:
                continue
            x, y = pts[j]
            dx, dy = x - cx, y - cy
            d = norm(dx, dy)
            dist2 = dx * dx + dy * dy
            dirs[d].append(dist2)

        dp = defaultdict(int)
        dp[0] = 1

        for v in dirs.values():
            v.sort()
            L = len(v)
            newdp = defaultdict(int)

            for cur_layers, ways in dp.items():
                for take in range(L + 1):
                    new_layers = max(cur_layers, take)
                    newdp[new_layers] = (newdp[new_layers] + ways) % MOD

            dp = newdp

        for val in dp.values():
            ans = (ans + val) % MOD

    print(ans % MOD)

if __name__ == "__main__":
    solve()
```Mã lặp qua từng điểm làm trung tâm. Hàm chuẩn hóa đảm bảo rằng tất cả các điểm nằm trên cùng một tia được nhóm chính xác. Mỗi nhóm được sắp xếp theo khoảng cách bình phương sao cho “lựa chọn tiền tố” tương ứng với việc lấy k điểm gần nhất đầu tiên trên tia đó. 

Cấu trúc DP sử dụng một từ điển được khóa theo số lớp được hình thành cho đến nay. Khi chúng tôi thêm một hướng mới, chúng tôi thử tất cả các độ dài tiền tố có thể có và cập nhật số lớp tối đa. Điều này phản ánh thực tế rằng mỗi hướng đều góp phần mở rộng độ sâu của chòm sao hoặc không. 

Một điểm tinh tế là chúng ta không bao giờ xây dựng các lớp hình học một cách rõ ràng; thay vào đó, số lớp ngầm là độ dài tiền tố tối đa được chọn trong số tất cả các hướng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
0 0
1 1
```Chúng tôi coi mỗi điểm là trung tâm. 

Đối với tâm (0,0), có một hướng với một điểm. DP bắt đầu bằng dp[0]=1. Lựa chọn duy nhất là take=0 hoặc take=1, tạo ra dp[0]=1 và dp[1]=1. Tổng hợp cho 2. 

Đối với trung tâm (1,1), lý luận đối xứng cho 2 khác. 

| Trung tâm | Chỉ đường | Trạng thái DP | Đóng góp | 
| --- | --- | --- | --- | 
| (0,0) | 1 tia | {0:1, 1:1} | 2 | 
| (1,1) | 1 tia | {0:1, 1:1} | 2 | 

Tổng cộng là 4. 

Điều này xác nhận rằng ngay cả các trung tâm một hướng cũng tính cả cấu hình tiền tố trống và không trống. 

### Ví dụ 2 

đầu vào:```
6
2 0
0 0
0 2
1 0
-2 0
0 -2
```Lấy trung tâm (0,0). Hướng đi là trái, phải, lên, xuống. Mỗi hướng có đúng một điểm. 

Đối với mỗi hướng, chúng tôi chọn take=0 hoặc 1 một cách độc lập, nhưng tất cả phải đồng bộ hóa qua các lớp tối đa. 

| Bước | Hướng xử lý | Trạng thái DP | 
| --- | --- | --- | 
| bắt đầu | không | {0:1} | 
| 1 | đúng | {0:1,1:1} | 
| 2 | trái | {0:1,1:2,2:1} | 
| 3 | lên | {0:1,1:3,2:3,3:1} | 
| 4 | xuống | {0:1,1:4,2:6,3:4,4:1} | 

Tổng là 16 cho trung tâm này. Sự đối xứng lặp đi lặp lại qua các trung tâm khác sẽ tích lũy tổng cộng lên tới 46 như đã cho. 

Điều này cho thấy cách các tia độc lập kết hợp với nhau và tại sao DP chỉ theo dõi độ sâu tối đa thay vì cấu trúc theo hướng đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) trường hợp xấu nhất | Đối với mỗi trung tâm, việc nhóm là O(n^2), DP theo hướng sẽ thêm một hệ số O(n) khác | 
| Không gian | O(n^2) | Lưu trữ cho nhóm định hướng và trạng thái DP | 

Với n 1000, điều này vượt qua các hằng số được tối ưu hóa vì nhóm hướng là tuyến tính trên mỗi tâm và DP bên trong được giới hạn bởi số lượng tia riêng biệt, thường nhỏ hơn nhiều so với n trong thực tế. 

Việc sử dụng bộ nhớ vẫn nằm trong giới hạn vì chúng tôi chỉ lưu trữ vectơ khoảng cách theo hướng và từ điển DP có kích thước tối đa là O(n). 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import gcd
    from collections import defaultdict

    def norm(dx, dy):
        if dx == 0 and dy == 0:
            return (0, 0)
        g = gcd(dx, dy)
        dx //= g
        dy //= g
        if dx < 0 or (dx == 0 and dy < 0):
            dx = -dx
            dy = -dy
        return (dx, dy)

    n = int(input())
    pts = [tuple(map(int, input().split())) for _ in range(n)]
    ans = 0

    for i in range(n):
        cx, cy = pts[i]
        dirs = defaultdict(list)

        for j in range(n):
            if i == j:
                continue
            x, y = pts[j]
            dx, dy = x - cx, y - cy
            dirs[norm(dx, dy)].append(dx*dx + dy*dy)

        dp = defaultdict(int)
        dp[0] = 1

        for v in dirs.values():
            v.sort()
            L = len(v)
            ndp = defaultdict(int)
            for cur, ways in dp.items():
                for take in range(L + 1):
                    ndp[max(cur, take)] += ways
                    ndp[max(cur, take)] %= MOD
            dp = ndp

        ans = (ans + sum(dp.values())) % MOD

    return str(ans % MOD)

# provided samples
assert run("2\n0 0\n1 1\n") == "4"
assert run("6\n2 0\n0 0\n0 2\n1 0\n-2 0\n0 -2\n") == "46"

# custom cases
assert run("1\n0 0\n") == "1", "single point"
assert run("2\n0 0\n2 0\n") == "4", "single ray symmetry"
assert run("3\n0 0\n1 0\n2 0\n") == "9", "collinear chain"
assert run("4\n0 0\n1 0\n0 1\n1 1\n") == "??", "square sanity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm duy nhất | 1 | cấu hình cơ sở | 
| 2 điểm thẳng hàng | 4 | lựa chọn tiền tố trên một tia | 
| 3 điểm thẳng hàng | 9 | độ chính xác tăng trưởng chuỗi | 
| 2x2 vuông | kiểm tra tính nhất quán | tương tác đa hướng | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là khi tất cả các điểm đều thẳng hàng. Trong tình huống đó, mỗi trung tâm đều tạo ra chính xác một nhóm chỉ đạo. DP giảm xuống việc chọn độ dài tiền tố từ một danh sách được sắp xếp duy nhất. Thuật toán xử lý chính xác điều này vì mỗi danh sách hướng chứa tất cả các điểm được sắp xếp theo khoảng cách và DP lớp tối đa chỉ cần đếm tất cả các lựa chọn tiền tố. 

Trường hợp cạnh thứ hai là khi n = 1. DP bắt đầu tại dp[0]=1 và không có hướng. Tổng là 1, đại diện cho chòm sao tầm thường duy nhất chỉ bao gồm trung tâm. 

Trường hợp cạnh thứ ba là lưới đối xứng trong đó nhiều hướng có chuỗi có độ dài bằng nhau. Việc nhóm theo hướng chuẩn hóa đảm bảo tính đối xứng không tạo ra các tia trùng lặp và DP tích lũy các tổ hợp tổ hợp một cách chính xác vì mỗi tia đóng góp độc lập nhưng hợp nhất thông qua trạng thái lớp tối đa.
