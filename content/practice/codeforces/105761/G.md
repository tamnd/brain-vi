---
title: "CF 105761G - Đi xe trượt băng"
description: "Chúng ta được cung cấp một đường thẳng từ vị trí 0 đến vị trí L. Dọc theo đường này có các điểm đặc biệt gọi là trạm tăng tốc."
date: "2026-06-21T22:55:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105761
codeforces_index: "G"
codeforces_contest_name: "2021 UCF Local Programming Contest"
rating: 0
weight: 105761
solve_time_s: 48
verified: true
draft: false
---

[CF 105761G - Đi xe trượt băng](https://codeforces.com/problemset/problem/105761/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đường thẳng từ vị trí 0 đến vị trí L. Dọc theo đường này có các điểm đặc biệt gọi là trạm tăng tốc. Bất cứ khi nào chúng ta đến một trong những trạm này, vận tốc của chúng ta ngay lập tức tăng lên theo một giá trị không đổi cố định c, giá trị này chúng ta chọn một lần khi bắt đầu chuyến đi và sau đó phải sử dụng ở mọi nơi. 

Giữa các trạm tăng tốc, chuyển động bị chi phối bởi một phân rã tuyến tính đơn giản: nếu chúng ta đi vào một đoạn có vận tốc v thì sau t giây vận tốc sẽ trở thành v − t và quãng đường di chuyển trong đoạn đó là tích phân của vận tốc theo thời gian. Nếu hết tốc độ trước trạm tiếp theo, chúng ta sẽ dừng lại và chuyến đi sẽ thất bại. 

Mục tiêu là chọn giá trị tăng c nhỏ nhất có thể sao cho bắt đầu từ vị trí 0 với vận tốc ban đầu c (vì lần tăng đầu tiên là 0), chúng ta có thể đến vị trí cuối cùng L, đi qua tất cả các trạm tăng theo thứ tự và về đích trong tổng thời gian tối đa là T. 

Các ràng buộc được cấu trúc sao cho n, số lượng trạm tăng tốc, tối đa là 100 trong khi L và T có thể lên tới 10^9. Điều này ngay lập tức loại trừ mọi mô phỏng theo các bước thời gian hoặc sự rời rạc hóa chi tiết. Bất kỳ giải pháp nào cũng phải xử lý từng phân đoạn một cách độc lập trong thời gian không đổi, vì vậy O(n log câu trả lời) hoặc O(n) cho mỗi lần kiểm tra là hướng khả thi duy nhất. 

Một vấn đề tế nhị phát sinh từ độ chính xác của dấu phẩy động. Công thức khoảng cách bao gồm các biểu thức bậc hai theo thời gian, do đó việc so sánh bất cẩn có thể gây ra lỗi. Một trường hợp cạnh quan trọng khác là khi vận tốc cần thiết vừa đủ để đến được trạm chính xác khi vận tốc chạm tới 0. Trong trường hợp đó, phân đoạn kết thúc chính xác ở ranh giới khả thi và cách suy luận theo kiểu khác nhau trong thời gian liên tục sẽ dẫn đến các quyết định tìm kiếm nhị phân sai nếu không được xử lý nhất quán. 

Ví dụ: nếu hai trạm rất gần nhau và c bị đánh giá thấp hơn một chút, thì thời gian di chuyển tính toán có thể vẫn đủ do lỗi nổi, nhưng về mặt vật lý, vận tốc sẽ về 0 quá sớm và chúng ta sẽ không bao giờ đạt được lần tăng tốc tiếp theo. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử giá trị ứng cử viên là c và mô phỏng toàn bộ chuyến đi. Đối với mỗi phân đoạn, chúng tôi giải phương trình bậc hai để xác định xem phải mất bao lâu để vượt qua khoảng cách đến lần tăng tiếp theo. Chúng tôi tích lũy thời gian và kiểm tra xem liệu chúng tôi có thể về đích trong phạm vi T hay không. Mô phỏng này đúng vì mỗi đoạn độc lập với vận tốc đi vào và công thức chuyển động xác định chính xác khoảng cách di chuyển. 

Tuy nhiên, việc thử các giá trị của c một cách tuần tự là không thể vì c có giá trị thực và có khả năng không bị giới hạn. Ngay cả khi chúng tôi rời rạc hóa c theo các bước nhỏ, độ chính xác cần thiết là khoảng 1e-6, nghĩa là cần khoảng 10^7 đến 10^8 ứng viên trong phạm vi tìm kiếm hợp lý và mỗi ứng viên có giá O(n). Điều đó trở nên quá chậm. 

Quan sát chính là tính đơn điệu. Nếu một giá trị tăng cường c nhất định cho phép chúng ta hoàn thành trong thời gian T thì bất kỳ giá trị c lớn hơn nào cũng có tác dụng. Việc tăng c sẽ làm tăng đáng kể vận tốc ở mỗi ga, điều này chỉ làm giảm thời gian di chuyển cần thiết ở mỗi đoạn. Hành vi đơn điệu này cho phép tìm kiếm nhị phân trên c. 

Mỗi lần kiểm tra tính khả thi là một mô phỏng chuyển tiếp trên tất cả các phân đoạn. Thách thức kỹ thuật chính là tính toán, đối với một đoạn có chiều dài d với vận tốc ban đầu v, phải mất bao lâu để bao phủ nó dưới sự phân rã tuyến tính. Giải v t − t^2/2 = d sẽ thu được phương trình bậc hai và chúng ta lấy căn dương nhỏ hơn. 

Điều này biến toàn bộ vấn đề thành một “tìm kiếm nhị phân trên câu trả lời với kiểm tra O(n)” tiêu chuẩn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu trên c rời rạc | O(K · n) | O(1) | Quá chậm | 
| Tìm kiếm nhị phân trên c với mô phỏng O(n) | O(n độ chính xác của nhật ký) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tìm kiếm nhị phân giá trị tăng c nhỏ nhất để làm cho chuyến đi khả thi.

1. Chúng tôi xác định chức năng kiểm tra(c) xác định xem giá trị tăng cường cố định có cho phép hoàn thành trong thời gian T hay không. Chức năng này mô phỏng từng đoạn chuyến đi. 
2. Khi bắt đầu, vận tốc là c vì chúng ta ngay lập tức nhận được mức tăng ở vị trí 0. Chúng ta cũng khởi tạo tổng thời gian là 0. 
3. Với mỗi đoạn giữa các vị trí boost liên tiếp, ta biết khoảng cách yêu cầu d. Chúng ta cũng biết vận tốc hiện tại v khi đi vào đoạn thẳng. 
4. Đối với mỗi đoạn, chúng tôi tính thời gian tối thiểu t cần thiết để đi được quãng đường d khi chuyển động giảm tốc. Chúng ta giải phương trình v t − t^2 / 2 = d. Căn nguyên hợp lệ là t = v − sqrt(v^2 − 2d), giả sử v^2 ≥ 2d. 
5. Nếu v^2 < 2d, đoạn đường này không thể thực hiện được vì vận tốc đạt đến 0 trước khi đến trạm tiếp theo. Trong trường hợp đó, check(c) ngay lập tức thất bại. 
6. Mặt khác, chúng ta cộng t vào tổng thời gian và cập nhật vận tốc ở cuối đoạn dưới dạng v − t, bằng sqrt(v^2 − 2d). 
7. Khi đến trạm tăng tốc, chúng ta tăng vận tốc lên c, do đó v trở thành v + c và tiếp tục. 
8. Sau khi xử lý tất cả các phân đoạn, chúng tôi kiểm tra xem tổng thời gian có ≤ T hay không. Nếu có, c là khả thi. 

Sau đó, chúng tôi tìm kiếm nhị phân c trong một phạm vi đủ lớn để bao gồm câu trả lời, thường là [0, 1e9] hoặc cao hơn và trả về giá trị khả thi nhỏ nhất. 

### Tại sao nó hoạt động 

Mô phỏng duy trì tính bất biến vật lý nghiêm ngặt: khi bắt đầu mỗi đoạn, vận tốc nắm bắt đầy đủ tất cả các hiệu ứng trong quá khứ và mối quan hệ khoảng cách bậc hai xác định duy nhất thời gian di chuyển. Bởi vì vận tốc chỉ tăng khi c lớn hơn và thời gian di chuyển đoạn là hàm số giảm của vận tốc đi vào nên tính khả thi là đơn điệu trong c. Điều này đảm bảo tính chính xác của tìm kiếm nhị phân, vì kiểm tra vị từ (c) chuyển từ sai sang đúng đúng một lần dọc theo dòng thực. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def check(c, L, n, tlim, b):
    v = c
    time = 0.0

    for i in range(1, n):
        d = b[i] - b[i - 1]

        if v * v < 2 * d:
            return False

        nv = math.sqrt(v * v - 2 * d)
        dt = v - nv

        time += dt
        v = nv + c

        if time > tlim:
            return False

    d = L - b[-1]
    if v * v < 2 * d:
        return False

    nv = math.sqrt(v * v - 2 * d)
    dt = v - nv

    time += dt

    return time <= tlim

def solve():
    L, n, tlim = map(int, input().split())
    b = list(map(int, input().split()))

    lo, hi = 0.0, 1.0
    while not check(hi, L, n, tlim, b):
        hi *= 2

    for _ in range(80):
        mid = (lo + hi) / 2
        if check(mid, L, n, tlim, b):
            hi = mid
        else:
            lo = mid

    print(hi)

if __name__ == "__main__":
    solve()
```Cốt lõi của việc triển khai là chức năng kiểm tra, tính toán tính khả thi trong O(n). Mỗi phân đoạn sử dụng nghiệm dạng đóng của phương trình chuyển động bậc hai thay vì mô phỏng sự suy giảm vận tốc từng giây. 

Biểu thức sqrt(v^2 − 2d) là vận tốc còn lại sau khi đi hết quãng đường d và v − sqrt(v^2 − 2d) là thời gian dành cho đoạn đường đó. Điều này tránh bất kỳ sự tích hợp số nào. 

Tìm kiếm nhị phân mở rộng giới hạn trên theo cấp số nhân cho đến khi tìm thấy giải pháp khả thi, đảm bảo tính chính xác mà không cần phải đoán giới hạn. 

Vòng lặp cuối cùng sử dụng số lần lặp cố định (80) vì độ chính xác của dấu phẩy động gấp đôi là đủ cho độ chính xác 1e-6. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi mẫu: 

đầu vào: 

L = 238, b = [0, 32, 110], T = 18 

Chúng tôi kiểm tra một ứng cử viên c = 10. 

Lúc đầu v = 10. 

| Phân đoạn | d | v trước | v^2 ≥ 2d | thêm thời gian | v sau đoạn | v sau khi tăng tốc | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 → 32 | 32 | 10 | vâng | 4 | 6 | 16 | 
| 32 → 110 | 78 | 16 | vâng | 6 | 10 | 20 | 
| 110 → 238 | 128 | 20 | vâng | 8 | 12 | - | 

Tổng thời gian trở thành 18. 

Điều này khẳng định tính khả thi chính xác ở mức c = 10. 

Bây giờ hãy xem xét một c = 9,9 nhỏ hơn một chút. Trong phân đoạn đầu tiên, v^2 hầu như không cao hơn 2d, nhưng thời gian tăng nhẹ, khiến thời gian tích lũy cuối cùng vượt quá 18. Điều này cho thấy hành vi ngưỡng đơn điệu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log R) | Mỗi lần kiểm tra tính khả thi sẽ quét tất cả các phân đoạn một lần và tìm kiếm nhị phân chạy cho các lần lặp có độ chính xác không đổi | 
| Không gian | O(1) | Chỉ lưu trữ các vị trí và một số đại lượng vô hướng | 

Với n ≤ 100, thậm chí ~80 lần kiểm tra tính khả thi cũng không đáng kể. Giải pháp dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import sqrt

    L, n, tlim = map(int, input().split())
    b = list(map(int, input().split()))

    def check(c):
        v = c
        time = 0.0
        for i in range(1, n):
            d = b[i] - b[i - 1]
            if v * v < 2 * d:
                return False
            nv = math.sqrt(v * v - 2 * d)
            time += v - nv
            v = nv + c
            if time > tlim:
                return False

        d = L - b[-1]
        if v * v < 2 * d:
            return False
        nv = math.sqrt(v * v - 2 * d)
        time += v - nv
        return time <= tlim

    lo, hi = 0.0, 1.0
    while not check(hi):
        hi *= 2
    for _ in range(60):
        mid = (lo + hi) / 2
        if check(mid):
            hi = mid
        else:
            lo = mid
    return str(hi)

# provided samples
assert abs(float(run("238 3 18\n0 32 110\n")) - 10.0) < 1e-6
assert abs(float(run("1000 5 20\n0 200 315 816 900\n")) - 27.829407683424986) < 1e-6

# custom cases
assert float(run("10 2 5\n0 10\n")) > 0, "minimum non-trivial"
assert abs(float(run("1 2 100\n0 0\n")) - 0.0) < 1e-6, "zero distance edge"
assert float(run("100 3 1\n0 50 90\n")) > 0, "tight time constraint"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10 2 5 / 0 10 | >0 | ngưỡng khả thi tối thiểu | 
| 1 2 100 / 0 0 | 0 | đoạn thoái hóa có khoảng cách bằng 0 | 
| 100 3 1 / 0 50 90 | tích cực | thời gian chặt chẽ lực lượng c lớn | 

## Vỏ cạnh 

Trường hợp biên quan trọng là khi hai trạm tăng tốc trùng nhau hoặc rất gần nhau. Trong tình huống đó, khoảng cách cần thiết là gần bằng 0, do đó đoạn đường đó hầu như không đóng góp thời gian và vận tốc về cơ bản chỉ tăng c. Thuật toán xử lý điều này một cách chính xác vì d = 0 cho v^2 ≥ 0 luôn đúng, nv = v và dt = 0, do đó chỉ còn lại bản cập nhật tăng cường. 

Một trường hợp khác là khi vận tốc vừa đủ, nghĩa là v^2 = 2d. Khi đó nv trở thành 0 và dt bằng v. Mô phỏng vẫn hoạt động vì nó hạ cánh chính xác với vận tốc bằng 0 tại trạm và ngay lập tức áp dụng mức tăng, duy trì tính khả thi. 

Một trường hợp tế nhị hơn là độ chính xác nổi gần ranh giới khả thi. Vì việc kiểm tra sử dụng căn bậc hai và phép so sánh nên các lỗi số nhỏ có thể làm mất tính khả thi. Tìm kiếm nhị phân giảm thiểu điều này bằng cách chỉ yêu cầu tính nhất quán trong phạm vi 1e-6 và sử dụng số lần lặp cố định đủ lớn để đảm bảo sự hội tụ ổn định.
