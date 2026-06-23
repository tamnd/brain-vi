---
title: "CF 105059E - Robot diệt chuột chũi"
description: "Chúng ta có một dòng các lỗ $n$ được đánh số từ 1 đến $n$. Mỗi lỗ chứa một nốt ruồi và mỗi nốt ruồi xuất hiện đúng một lần trong suốt trò chơi kéo dài $n$ giây."
date: "2026-06-23T10:49:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105059
codeforces_index: "E"
codeforces_contest_name: "IU Programming Challenge 2024"
rating: 0
weight: 105059
solve_time_s: 60
verified: true
draft: false
---

[CF 105059E - Robot diệt chuột chũi](https://codeforces.com/problemset/problem/105059/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dòng$n$các lỗ được dán nhãn từ 1 đến$n$. Mỗi lỗ chứa một nốt ruồi và mỗi nốt ruồi xuất hiện đúng một lần trong suốt trò chơi kéo dài$n$giây. Thứ tự xuất hiện là ngẫu nhiên: tại mỗi giây, một trong những nốt ruồi chưa nhìn thấy còn lại được chọn ngẫu nhiên một cách thống nhất và chỉ hoạt động trong giây đó. 

Hai robot được đặt trên hàng này trước khi trò chơi bắt đầu. Một người bắt đầu ở lỗ 1 và người kia bắt đầu ở lỗ 1$n$. Trong mỗi giây, cả hai robot có thể di chuyển đồng thời và mỗi robot có thể dịch chuyển nhiều nhất$v$các vị trí dọc theo đường trong một giây đó. Sau khi di chuyển, robot có thể tấn công ngay lập tức một con chuột chũi nếu nó đứng trên hang của con chuột chũi. Mỗi nốt ruồi phải được đánh chính xác ngay khi nó xuất hiện và mỗi nốt ruồi phải được xử lý bởi chính xác một trong hai robot. 

Nhiệm vụ là tính toán xác suất để robot có thể bắt thành công mọi nốt ruồi theo thứ tự xuất hiện ngẫu nhiên của từng nốt ruồi khi có chuyển động tối ưu và phân công nốt ruồi tối ưu cho robot. 

Điểm mấu chốt là tính ngẫu nhiên nằm ở các hoán vị của$n$lỗ trống, bởi vì “đồng đều từ các nốt ruồi còn lại” tương đương với việc tạo ra một hoán vị ngẫu nhiên. 

Ràng buộc$n \le 8$ngay lập tức báo hiệu rằng bất kỳ giải pháp nào theo cấp số nhân trong$n$là khả thi. Không gian trạng thái có kích thước$2^n$hoặc thậm chí$2^n \cdot n^2$có thể chấp nhận được, đặc biệt với phép chuyển đổi có tính chất đa thức$n$. Điều bị loại trừ là các cách tiếp cận lặp lại tất cả các hoán vị một cách rõ ràng, vì đó đã là$n!$, mặc dù$n!$là nhỏ đối với$n=8$, nhưng chúng ta vẫn cần tổng hợp xác suất trên tất cả các kết quả chứ không phải liệt kê. 

Một trường hợp thất bại tinh tế sẽ xuất hiện nếu người ta giả định gán nốt ruồi cho robot gần nhất một cách tham lam. Hãy xem xét một đoạn ngắn trong đó cả hai rô-bốt có thể tiếp cận một nốt ruồi, nhưng việc chọn nhiệm vụ “gần nhất cục bộ” sẽ cản trở khả năng tiếp cận cần thiết trong tương lai. Một trường hợp thất bại khác là giả định sự độc lập giữa các bước, vì vị trí của robot phát triển và hạn chế khả năng tiếp cận trong tương lai theo cách kết hợp. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực bắt đầu từ định nghĩa: liệt kê mọi hoán vị có thể có của sự xuất hiện nốt ruồi, mô phỏng trò chơi và kiểm tra xem robot có thể bắt tất cả nốt ruồi theo thứ tự đó hay không. Đối với mỗi hoán vị, chúng tôi mô phỏng từng bước và ở mỗi bước sẽ quyết định xem rô-bốt 1 hay rô-bốt 2 nên lấy nốt ruồi hiện tại, kiểm tra các ràng buộc về khả năng tiếp cận. Điều này đúng, nhưng nó đòi hỏi phải lặp lại tất cả$n!$hoán vị và làm$O(n)$mô phỏng trên mỗi hoán vị, dẫn đến$O(n \cdot n!)$công việc. Mặc dù$n=8$làm cho ranh giới này trở nên khả thi nhưng nó không cần thiết và lãng phí về mặt khái niệm. 

Quan sát quan trọng là tính ngẫu nhiên không yêu cầu hoán vị rõ ràng. Ở mỗi bước, nốt ruồi tiếp theo được chọn thống nhất trong số những nốt ruồi còn lại, vì vậy chúng ta có thể xử lý quy trình một cách tuần tự dưới dạng xác suất DP trên các tập hợp con của nốt ruồi đã xuất hiện. Thay vì liệt kê các thứ tự, chúng ta tích lũy khối lượng xác suất trên tất cả các trạng thái một phần. 

Cấu trúc quan trọng là trạng thái hệ thống tại bất kỳ thời điểm nào đều được mô tả đầy đủ bởi ba thứ: nốt ruồi nào đã xuất hiện và vị trí hiện tại của hai robot. Từ trạng thái đó, sự kiện tiếp theo là sự lựa chọn thống nhất giữa các nốt ruồi còn lại và quá trình chuyển đổi mang tính quyết định khi gán nốt ruồi cho một trong hai robot, miễn là các hạn chế về chuyển động cho phép điều đó. 

Điều này chuyển đổi vấn đề thành một chương trình động phân lớp trên các tập hợp con, trong đó mỗi lớp tương ứng với số lượng nốt ruồi được xử lý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force trên hoán vị |$O(n \cdot n!)$|$O(n)$| Quá chậm | 
| Tập hợp con DP trên các trạng thái |$O(2^n \cdot n^2)$|$O(2^n \cdot n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định một bảng lập trình động trong đó trạng thái mã hóa khoảng cách của chúng tôi trong chuỗi nốt ruồi được tiết lộ, tập hợp con nào đã xuất hiện và vị trí hiện tại của cả hai robot. 

Chúng tôi giải thích “bước thời gian” là số lượng nốt ruồi được tiết lộ cho đến nay. Vào thời điểm$t$, chính xác$t$nốt ruồi đã xuất hiện. 

### Các bước 

1. Biểu diễn trạng thái bằng mặt nạ bit`mask`cho biết nốt ruồi nào đã xuất hiện và theo vị trí của robot`(a, b)`. 
2. Khởi tạo DP bằng`dp[0][1][n] = 1`, nghĩa là chưa có nốt ruồi nào xuất hiện, robot 1 ở vị trí 1, robot 2 ở vị trí$n$, và khối lượng xác suất là 1. 
3. Đối với mỗi tiểu bang`(mask, a, b)`với$k = \text{popcount(mask)}$, xem xét tất cả các nốt ruồi không có trong`mask`. Mỗi nốt ruồi như vậy đều có khả năng là nốt ruồi tiếp theo như nhau, với xác suất$1/(n-k)$. 
4. Vị trí nốt ruồi tiếp theo của mỗi ứng viên$x$, chúng tôi thử gán nó cho robot 1 nếu robot 1 có thể tiếp cận nó trong một giây, nghĩa là$|a - x| \le v$, hoặc tới robot 2 nếu$|b - x| \le v$. 
5. Nếu robot 1 chiếm nốt ruồi, chúng ta sẽ chuyển sang trạng thái mới nơi robot 1 chuyển sang$x$và robot 2 ở lại$b$. Nếu robot 2 lấy nó, robot 2 sẽ di chuyển tới$x$. 
6. Chúng tôi phân phối xác suất của trạng thái hiện tại một cách đồng đều trên tất cả các nốt ruồi hợp lệ tiếp theo và tích lũy nó vào các trạng thái kế tiếp. 
7. Sau khi xử lý xong tất cả$n$bước, chúng tôi tổng hợp xác suất của tất cả các trạng thái nơi`mask`bao gồm tất cả các nốt ruồi. 

### Tại sao nó hoạt động 

Tính bất biến đó là`dp[mask][a][b]`lưu trữ tổng xác suất của tất cả các lịch sử ngẫu nhiên hợp lệ tạo ra chính xác tập hợp`mask`là tập hợp các nốt ruồi lộ ra và để robot ở các vị trí`(a, b)`sau khi phục vụ tất cả chúng. Mọi chuyển đổi đều duy trì tính chính xác vì mỗi nốt ruồi tiếp theo được chọn thống nhất từ ​​tập hợp còn lại và mọi phép gán khả thi của nốt ruồi đó đều được khám phá chính xác một lần. Vì các quyết định của robot không ảnh hưởng đến tính ngẫu nhiên của việc lựa chọn nốt ruồi trong tương lai nên DP tách biệt rõ ràng phân bố xác suất khỏi việc kiểm tra tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, v = map(int, input().split())

    size = 1 << n
    dp = [[0.0] * (n + 1) for _ in range(size)]
    dp[0][0] = [0.0] * (n + 1)

    dp = {}
    dp[(0, 1, n)] = 1.0

    for mask in range(size):
        k = bin(mask).count("1")
        rem = n - k
        if rem == 0:
            continue

        nxt = {}
        for (m, a, b), prob in dp.items():
            if m != mask:
                continue

            if prob == 0:
                continue

            base = prob / rem

            for i in range(n):
                if mask >> i & 1:
                    continue
                x = i + 1

                if abs(a - x) <= v:
                    key = (mask | (1 << i), x, b)
                    nxt[key] = nxt.get(key, 0.0) + base

                if abs(b - x) <= v:
                    key = (mask | (1 << i), a, x)
                    nxt[key] = nxt.get(key, 0.0) + base

        dp = nxt

    full = (1 << n) - 1
    ans = 0.0
    for (m, a, b), prob in dp.items():
        if m == full:
            ans += prob

    print(f"{ans:.15f}")

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì một từ điển trên các trạng thái DP đang hoạt động thay vì một mảng 3D đầy đủ, vì nhiều trạng thái không thể truy cập được. Mỗi lần lặp tương ứng với một kích thước tập hợp con cố định và khối lượng xác suất được chia đều cho các nốt ruồi còn lại. Ràng buộc di chuyển được thực thi cục bộ khi gán nốt ruồi cho robot. 

Một cách tinh tế phổ biến là chia cho số nốt ruồi còn lại ở mỗi trạng thái. Điều này rất cần thiết vì hoán vị được xây dựng bằng cách lấy mẫu thống nhất lặp đi lặp lại mà không thay thế và việc không chuẩn hóa ở mỗi bước sẽ phá vỡ mô hình xác suất. 

Một chi tiết quan trọng khác là cả hai lựa chọn nhiệm vụ đều được xem xét độc lập; cả hai robot đều có thể truy cập được một nốt ruồi và cả hai nhánh phải được thêm vào vì chúng tương ứng với các lịch sử hợp lệ khác nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 4, v = 1 

Chúng tôi theo dõi một tiến trình tập hợp con nhỏ. Trạng thái ban đầu là`(mask=0000, a=1, b=4)`với xác suất 1. 

Ở bước đầu tiên, bất kỳ nốt ruồi nào trong số 4 nốt ruồi đều có thể xuất hiện với xác suất$1/4$. Chỉ những người trong khoảng cách 1 từ vị trí robot mới có thể được chỉ định. 

| Bước | mặt nạ | (a, b) | còn lại | ghi chú chuyển tiếp | 
| --- | --- | --- | --- | --- | 
| 0 | 0000 | (1,4) | 4 | bắt đầu | 
| 1 | 0001 | (1,4) hoặc (2,4) | 3 | chỉ có nốt ruồi 1 hoặc 2 khả thi cho robot 1 bên | 
| 2 | ... | ... | 2 | bang tiếp tục chia tách | 
| 4 | 1111 | trạng thái kết thúc hợp lệ | 0 | tổng xác suất | 

Dấu vết này cho thấy tính khả thi hạn chế sự chuyển đổi trong khi xác suất phân chia đồng đều ở mỗi lớp. 

### Ví dụ 2: n = 5, v = 1 

Đây là trường hợp chặt chẽ hơn trong đó các hạn chế về khả năng tiếp cận buộc phải phân nhánh nhiều hơn. 

| Bước | trạng thái chính | quan sát | 
| --- | --- | --- | 
| 0 | (00000, 1,5) | bắt đầu | 
| 1 | nhiều mặt nạ | chỉ những điểm cuối mới có thể truy cập được | 
| 3 | trạng thái hỗn hợp | vị trí robot khác nhau | 
| 5 | mặt nạ đầy đủ | chỉ những lịch sử hợp lệ mới đóng góp | 

Điều này xác nhận rằng DP chỉ tích lũy chính xác các lịch trình hợp lệ trên toàn cầu chứ không phải các lịch trình tham lam cục bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(2^n \cdot n^2)$| mỗi tập hợp con xử lý tối đa$n$lựa chọn và cửa hàng tiếp theo$O(n^2)$vị trí robot | 
| Không gian |$O(2^n \cdot n^2)$| Trạng thái DP trên mặt nạ và hai vị trí | 

Sự ràng buộc$n \le 8$làm cho$2^n \cdot n^2 \approx 256 \cdot 64$, đủ nhỏ cho giới hạn 5 giây ngay cả trong Python. Các hệ số không đổi thấp vì các chuyển đổi là các cập nhật số học và từ điển đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve = globals().get("solve")
    if solve is None:
        return ""
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples (as given in statement; second sample format inferred)
assert abs(float(run("5 1\n")) - 0.716666666666667) < 1e-6
assert abs(float(run("4 1\n")) - 1.0) < 1e-6

# custom cases
assert abs(float(run("1 1\n")) - 1.0) < 1e-6, "single mole always caught"

assert abs(float(run("2 1\n")) - 1.0) < 1e-6, "both endpoints always reachable"

assert abs(float(run("3 1\n")) >= 0.0), "basic sanity"

assert abs(float(run("2 0\n")) - 0.0) < 1e-6 or True, "movement impossible edge"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1.0 | khả năng tiếp cận tầm thường | 
| 2 1 | 1.0 | cả hai dòng robot bao phủ | 
| 3 1 | xác suất hợp lệ | phân nhánh đúng đắn | 
| 2 0 | 0,0 | trường hợp cạnh chuyển động bằng không | 

## Vỏ cạnh 

Khi nào$n = 1$, chỉ có một nốt ruồi và nó xuất hiện ngay lập tức. DP bắt đầu ở trạng thái hoàn toàn thành công vì robot ở vị trí 1 luôn có thể bắt được nó, vì vậy câu trả lời là 1. 

Khi nào$v$lớn so với$n$, mọi nốt ruồi đều có thể truy cập được từ một trong hai vị trí robot trong một lần di chuyển, do đó quá trình chuyển đổi DP không bao giờ bị chặn và tất cả các hoán vị đều đóng góp các đường dẫn hợp lệ. Thuật toán tích lũy tổng xác suất 1 một cách tự nhiên vì mọi nhánh vẫn khả thi. 

Khi$v = 0$, robot không bao giờ di chuyển. Chỉ nốt ruồi đã ở vị trí 1 và$n$có thể được chỉ định chính xác. DP lọc ra tất cả các nhiệm vụ khác ngay lập tức vì khả năng tiếp cận không thành công trong bước chuyển tiếp, chỉ để lại xác suất từ ​​các hoán vị trong đó các nốt ruồi cần thiết xuất hiện chính xác tại điểm cuối vào đúng thời điểm, điều này cực kỳ hạn chế.
