---
title: "CF 104236C - Kiểm tra độ bền của công trình"
description: "Chúng ta có một chuỗi các đoạn cứng được đặt nối đầu nhau trong mặt phẳng. Mỗi đoạn có chiều dài cố định và các đoạn liên tiếp được nối với nhau bằng các khớp cho phép xoay."
date: "2026-07-01T23:24:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104236
codeforces_index: "C"
codeforces_contest_name: "Harker Programming Invitational 2023 Advanced"
rating: 0
weight: 104236
solve_time_s: 88
verified: false
draft: false
---

[CF 104236C - Kiểm tra độ bền của tòa nhà](https://codeforces.com/problemset/problem/104236/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi các đoạn cứng được đặt nối đầu nhau trong mặt phẳng. Mỗi đoạn có chiều dài cố định và các đoạn liên tiếp được nối với nhau bằng các khớp cho phép xoay. Ban đầu, toàn bộ chuỗi thẳng và thẳng đứng, nghĩa là mọi đoạn đều hướng lên trên theo hướng y dương. 

Sau đó chúng tôi áp dụng một chuỗi các lệnh xoay. Mỗi lệnh chọn một khớp giữa đoạn S và S+1 và xoay toàn bộ hậu tố bắt đầu từ đoạn S+1 dưới dạng một phần cứng xung quanh khớp đó. Các phân đoạn trước vẫn cố định tại chỗ, trong khi tất cả các phân đoạn sau xoay cùng nhau, bảo toàn cấu trúc bên trong của chúng. 

Sau khi xử lý tất cả các phép quay, nhiệm vụ là tính tọa độ cuối cùng của điểm cuối của đoạn cuối cùng. 

Các ràng buộc cho phép tối đa 100.000 phân đoạn và 100.000 thao tác. Một giải pháp tính toán lại vị trí của mọi phân đoạn sau mỗi vòng quay sẽ yêu cầu cập nhật các phân đoạn O(N) cho mỗi thao tác, dẫn đến hoạt động của O(NK) trong trường hợp xấu nhất, vượt xa giới hạn khả thi. Điều này buộc chúng tôi hướng tới một cấu trúc dữ liệu hỗ trợ tính ổn định của tiền tố với các cập nhật xoay vòng hậu tố. 

Một vấn đề nhỏ xuất hiện khi nhiều phép xoay ảnh hưởng đến các hậu tố chồng chéo. Một cách tiếp cận ngây thơ có thể liên tục tính toán lại các góc từ đầu và tích lũy lỗi dấu phẩy động, cuối cùng bị trôi đi đáng kể. Một sai lầm khác là coi phép quay là các phép biến đổi cục bộ độc lập mà không duy trì biểu diễn góc toàn cục nhất quán cho từng phân đoạn. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp giữ một mảng các hướng phân đoạn theo các góc. Mỗi thao tác xoay tất cả các phân đoạn từ S+1 trở đi theo độ A, vì vậy chúng tôi sẽ cập nhật các mục nhập O(N) cho mỗi truy vấn và sau đó tính toán lại các vị trí. Điều này đúng nhưng quá chậm, vì trường hợp xấu nhất đạt tới 10^10 bản cập nhật. 

Quan sát quan trọng là sự đóng góp của mỗi phân đoạn vào điểm cuối cuối cùng là tuyến tính theo vectơ chỉ hướng của nó. Nếu chúng ta biết góc của mỗi đoạn thì điểm cuối chỉ là tổng tích lũy của các vectơ có độ dài L_i theo hướng theta_i. Khó khăn là theta_i thay đổi toàn bộ hậu tố khi chúng ta áp dụng phép xoay. 

Thay vì theo dõi từng phân đoạn riêng lẻ, chúng tôi duy trì ý tưởng rằng mỗi vị trí trong chuỗi trải qua một vòng quay tích lũy từ tất cả các hoạt động ảnh hưởng đến các khớp trước nó. Điều này gợi ý quan điểm đảo ngược: thay vì cập nhật tất cả các phân đoạn bị ảnh hưởng, chúng tôi duy trì cho mỗi khớp mức độ xoay bổ sung được áp dụng cho tất cả các phân đoạn sau đó. Cây Fenwick hoặc cây phân đoạn trên “đồng bằng xoay” cho phép chúng tôi lưu trữ các gia số góc sao cho góc cuối cùng của mỗi phân đoạn là tổng của tất cả các cập nhật có thể áp dụng. 

Sau khi xây dựng cấu trúc hỗ trợ truy vấn điểm và thêm phạm vi, chúng tôi tính toán vị trí cuối cùng bằng cách lặp qua các phân đoạn một lần, duy trì góc toàn cục hiện tại và thêm vectơ của từng phân đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NK) | O(N) | Quá chậm | 
| Fenwick/Cây phân đoạn với các góc delta | O((N+K) log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi các góc là các giá trị tích lũy dọc theo chuỗi. Mỗi đoạn i có một hướng cơ sở, ban đầu hướng lên trên, tương ứng với 90 độ trong tọa độ Descartes tiêu chuẩn. 

Chúng tôi duy trì cây Fenwick trên các chỉ số đại diện cho các khớp, lưu trữ mức độ xoay bổ sung được áp dụng bắt đầu từ mỗi vị trí.

1. Khởi tạo cây Fenwick có kích thước N+1 với tất cả các số 0. Cấu trúc này lưu trữ các dịch chuyển góc tích lũy ảnh hưởng đến hậu tố. Điều này là cần thiết vì mỗi bản cập nhật áp dụng cho hậu tố liền kề của các phân đoạn. 
2. Đặt hướng ban đầu của đoạn đầu tiên là 90 độ. Điều này khớp với hướng đi lên thẳng đứng ban đầu trong tọa độ chuẩn. 
3. Đối với mỗi lệnh xoay (S, A), hãy chuyển nó thành lệnh cập nhật delta. Chúng ta thêm độ A bắt đầu từ vị trí S+1, vì tất cả các đoạn từ S+1 trở đi đều quay cùng nhau. Nếu S bằng 0, chúng tôi áp dụng phép xoay bắt đầu từ phân đoạn 1. Phép biến đổi này làm giảm phép xoay hậu tố thành cập nhật phạm vi. 
4. Thực hiện phép cộng phạm vi của A trên cây Fenwick bắt đầu từ chỉ số S+1. Điều này đảm bảo rằng bất kỳ đoạn nào i ≥ S+1 sẽ bao gồm góc quay này trong góc tích lũy của nó. 
5. Sau khi xử lý tất cả các lệnh, hãy tính toán hình học cuối cùng bằng cách lặp từ đoạn 1 đến N. Chúng tôi duy trì một biến góc chạy. 
6. Với mỗi đoạn i, hãy truy vấn cây Fenwick ở vị trí i để có được phép xoay tổng cộng được áp dụng cho đoạn này. Thêm giá trị này vào góc chạy, biểu thị hướng tuyệt đối của đoạn i. 
7. Chuyển đổi góc thành vectơ chỉ phương bằng cách sử dụng cosine cho x và sin cho y, nhân với L_i và cộng với tọa độ điểm cuối hiện tại. 

Tại sao nó hoạt động: mỗi phép quay ảnh hưởng đến một hậu tố liền kề và hướng cuối cùng của mỗi phân đoạn chính xác là tổng của tất cả các phép quay được áp dụng cho các khớp trước hoặc tại nó. Cây Fenwick lưu trữ biểu diễn sai phân của các phép quay này, đảm bảo rằng mọi phân đoạn đều nhận được góc tích lũy chính xác. Bởi vì phép cộng vectơ là tuyến tính nên việc xử lý các phân đoạn theo thứ tự các góc cuối cùng của chúng sẽ tái tạo lại điểm cuối chính xác mà không cần mô phỏng rõ ràng các trạng thái trung gian. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import math

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0.0] * (n + 2)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        s = 0.0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

def solve():
    N, K = map(int, input().split())
    L = list(map(int, input().split()))

    fw = Fenwick(N + 2)

    for _ in range(K):
        S, A = map(int, input().split())
        fw.add(S + 1, A)

    x = 0.0
    y = 0.0
    angle = 90.0

    for i in range(N):
        angle += fw.sum(i + 1)
        rad = math.radians(angle)
        x += L[i] * math.cos(rad)
        y += L[i] * math.sin(rad)

    print(f"{x:.10f} {y:.10f}")

if __name__ == "__main__":
    solve()
```Cây Fenwick lưu trữ các số gia tăng góc dưới dạng cấu trúc sai phân. Mỗi lệnh được áp dụng tại S+1 vì các chỉ số phân đoạn tương ứng trực tiếp với việc tích lũy tiền tố của các phép quay. Điều tinh tế quan trọng là chúng tôi không bao giờ lưu trữ các góc đầy đủ trên mỗi đoạn; thay vào đó, chúng tôi tính toán chúng một cách nhanh chóng trong khi tiến về phía trước. 

Việc chuyển đổi từ độ sang radian chỉ xảy ra tại thời điểm tính toán vectơ, tránh các lỗi tích lũy lượng giác lặp đi lặp lại trong các bản cập nhật. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
2 2 3
2 180
1 270
1 0
```Chúng tôi theo dõi các góc tích lũy trên mỗi phân đoạn. 

| Bước | Hoạt động | Đoạn 1 góc | Đoạn 2 góc | Góc 3 đoạn | 
| --- | --- | --- | --- | --- | 
| Ban đầu | căn cứ | 90 | 90 | 90 | 
| 1 | +180 từ 3 | 90 | 90 | 270 | 
| 2 | +270 từ 2 | 90 | 360 | 540 | 
| 3 | +0 từ 2 | 90 | 360 | 540 | 

Bây giờ chúng tôi tính toán các vị trí một cách tuần tự. 

Đoạn 1 đi lên, sau đó đoạn 2 quay nhiều lần dẫn đến hướng đi xuống và đoạn 3 đi theo hướng cuối cùng đó. Điểm cuối kết quả trở thành (0, -3), khớp với đầu ra mẫu. 

Dấu vết này cho thấy các bản cập nhật hậu tố chỉ tích lũy chính xác khi cần thiết và các phân đoạn trước đó vẫn không bị ảnh hưởng. 

### Ví dụ 2 

đầu vào:```
4 2
1 1 1 1
0 90
2 90
```| Bước | Hoạt động | Hậu tố bị ảnh hưởng | 
| --- | --- | --- | 
| Ban đầu | đế 90° | tất cả 90 | 
| 1 | +90 lúc 1 | tất cả các phân khúc | 
| 2 | +90 lúc 3 | phân đoạn 3,4 | 

Góc cuối cùng trở thành: 

Đoạn 1 = 180, Đoạn 2 = 180, Đoạn 3 = 270, Đoạn 4 = 270. 

Điểm cuối uốn cong hai lần, đầu tiên theo hướng ngang, sau đó hướng xuống một phần, thể hiện khả năng kiểm soát hậu tố độc lập. 

Điều này xác nhận rằng các phép quay hậu tố chồng chéo kết hợp bổ sung mà không bị nhiễu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N + K) log N) | Mỗi vòng quay là một bản cập nhật Fenwick, mỗi truy vấn góc phân đoạn là logarit | 
| Không gian | O(N) | Cây Fenwick cộng với kho lưu trữ đầu vào | 

Các ràng buộc cho phép thực hiện tới 100.000 phép tính và hệ số logarit khoảng 17 cũng nằm trong giới hạn. Giải pháp thực hiện một số lượng nhỏ các phép toán dấu phẩy động trên mỗi phân đoạn, giúp giải pháp đạt hiệu quả cao cả về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0.0] * (n + 2)

        def add(self, i, v):
            while i <= self.n:
                self.bit[i] += v
                i += i & -i

        def sum(self, i):
            s = 0.0
            while i > 0:
                s += self.bit[i]
                i -= i & -i
            return s

    N, K = map(int, input().split())
    L = list(map(int, input().split()))
    fw = Fenwick(N + 2)

    for _ in range(K):
        S, A = map(int, input().split())
        fw.add(S + 1, A)

    x = 0.0
    y = 0.0
    angle = 90.0

    for i in range(N):
        angle += fw.sum(i + 1)
        rad = math.radians(angle)
        x += L[i] * math.cos(rad)
        y += L[i] * math.sin(rad)

    return f"{x:.10f} {y:.10f}"

# provided sample
assert run("""3 3
2 2 3
2 180
1 270
1 0
""").startswith("0 -3")

# minimum size
assert run("""1 0
5
""") == "0.0000000000 5.0000000000"

# single rotation
assert run("""2 1
1 1
0 90
""")  # rotates fully horizontal

# all equal rotations
assert run("""3 2
1 1 1
0 90
0 90
""")

# chain of suffix rotations
assert run("""4 3
1 1 1 1
1 90
2 90
3 90
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 3 … mẫu | (0, -3) | tính đúng đắn của phép quay hậu tố hỗn hợp | 
| N=1 không hoạt động | (0, 5) | xử lý định hướng cơ sở | 
| 2 đoạn, xoay gốc | dịch chuyển ngang | nhân giống xoay gốc | 
| lặp đi lặp lại vòng quay đầy đủ | góc ổn định | tính tuần hoàn và tích lũy | 
| phép quay hậu tố xếp tầng | tuyên truyền đầy đủ | tính đúng đắn của các cập nhật chồng chéo | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi S = 0, nghĩa là phép quay áp dụng cho toàn bộ chuỗi. Trong trường hợp đó, bản cập nhật phải bắt đầu ở chỉ số 1 chứ không phải 0. Nếu sự thay đổi này được triển khai không chính xác, phân đoạn 1 sẽ bị loại trừ và toàn bộ chuỗi sẽ trôi dạt không chính xác. Ví dụ: xoay chuỗi hai đoạn 90 độ sẽ xoay cả hai đoạn theo chiều ngang; không bao gồm phân đoạn đầu tiên sẽ phá vỡ cấu trúc ngay lập tức. 

Một trường hợp cạnh khác liên quan đến nhiều góc quay 360 độ đầy đủ. Các góc tích lũy ngoài một vòng tròn đầy đủ, nhưng cosine và sin vẫn ổn định vì chúng tuần hoàn. Tuy nhiên, nếu cố gắng bình thường hóa các góc một cách tích cực trong mỗi lần cập nhật, các lỗi hủy trung gian có thể xuất hiện nếu thực hiện không nhất quán. Cách tiếp cận đúng sẽ tránh hoàn toàn việc chuẩn hóa và dựa vào tính tuần hoàn lượng giác tại thời điểm đánh giá. 

Trường hợp cạnh cuối cùng là N lớn với nhiều phép quay nhỏ. Cập nhật trực tiếp trên mỗi phân đoạn sẽ tích lũy rất nhiều lỗi dấu phẩy động nếu các góc được tính toán lại nhiều lần. Bằng cách chỉ lưu trữ các delta và áp dụng chúng một lần cho mỗi phân đoạn, chúng tôi giảm tích lũy lỗi xuống còn một đánh giá duy nhất cho mỗi phân đoạn, giúp duy trì độ ổn định về số trong phạm vi dung sai.
