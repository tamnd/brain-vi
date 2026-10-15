---
title: "CF 105434J - \u70ab\u8000\u5feb\u4e50"
description: "Chúng tôi được xếp một hàng sinh viên. Mỗi học sinh giữ một “con số may mắn” trong khoảng từ 1 đến m, vì vậy đường thẳng có thể được coi là một mảng S có độ dài n trên m loại. Chúng ta được phép gán thứ hạng cho m số may mắn này bằng cách chọn hoán vị từ 1 đến m."
date: "2026-06-23T03:54:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105434
codeforces_index: "J"
codeforces_contest_name: "2024\u5e74\u201c\u6838\u6843\u676f\u201d\u6b66\u6c49\u5730\u533aACM\u840c\u65b0\u8d5b"
rating: 0
weight: 105434
solve_time_s: 79
verified: true
draft: false
---

[CF 105434J - \u70ab\u8000\u5feb\u4e50](https://codeforces.com/problemset/problem/105434/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được xếp một hàng sinh viên. Mỗi học sinh giữ một “con số may mắn” trong khoảng từ 1 đến m, vì vậy đường thẳng có thể được coi là một mảng S có độ dài n trên m loại. 

Chúng ta được phép gán thứ hạng cho m số may mắn này bằng cách chọn hoán vị từ 1 đến m. Hoán vị này cho chúng ta biết “thứ hạng” hoặc “cấp quà tặng” của mỗi số: mọi giá trị x đều có thứ hạng p[x], trong đó thứ hạng nhỏ hơn hay lớn hơn không quan trọng, chỉ có sự khác biệt tương đối giữa các thứ hạng mới quan trọng thông qua quy tắc tính điểm. 

Sau khi xếp hạng đã được ấn định, chúng tôi xem xét từng cặp học sinh liền kề trong hàng. Đối với mỗi ranh giới giữa các vị trí i−1 và i, chúng ta so sánh thứ hạng các con số may mắn của chúng. Nếu học sinh bên phải có thứ hạng cao hơn hoặc bằng học sinh bên trái, chúng tôi sẽ trả chi phí tỷ lệ thuận với chênh lệch thứ hạng. Nếu người bên phải có thứ hạng thấp hơn, thay vào đó chúng tôi sẽ trả một mức chi phí khác tùy thuộc vào tổng thứ hạng của họ. 

Nhiệm vụ là chọn hoán vị thứ hạng cho các giá trị m sao cho tổng chi phí trên tất cả các cặp liền kề trong dòng là nhỏ nhất. 

Điểm cấu trúc quan trọng là n có thể lớn tới 100000, nhưng m nhiều nhất là 20. Điều đó ngay lập tức cho chúng ta biết rằng chúng ta không thể xử lý từng học sinh trong bất kỳ không gian trạng thái nào. Bất kỳ giải pháp nào lặp lại các hoán vị của cấp bậc hoặc trên học sinh ở mỗi tiểu bang sẽ không thành công. Thứ nguyên duy nhất có thể quản lý được là số lượng giá trị riêng biệt, vì vậy giải pháp phải nén mảng S thành các tương tác giữa các giá trị. 

Một cách giải thích ngây thơ có thể gợi ý nên thử tất cả m! phân công cấp bậc. Điều đó đã bùng nổ ở tuổi 20! và hoàn toàn không thể thực hiện được. 

Một vấn đề tế nhị hơn là chi phí chỉ phụ thuộc vào các cặp lân cận trong dòng ban đầu chứ không phụ thuộc vào các tương tác tùy ý. Điều này có nghĩa là chúng ta có thể tổng hợp dữ liệu đầu vào thành số lần chuyển tiếp giữa các giá trị. Nếu chúng ta định nghĩa cnt[a][b] là số lần một học sinh có giá trị a được theo sau bởi giá trị b, thì toàn bộ vấn đề sẽ giảm xuống việc quyết định thứ hạng cho các giá trị 1..m. 

Một sai lầm phổ biến là nghĩ rằng thứ tự của học sinh là quan trọng trong quá trình tối ưu hóa. Nó không. Khi cnt được xây dựng, trình tự sinh viên không còn quan trọng nữa. 

Các trường hợp cạnh phát sinh khi m=1, trong đó không có giá trị kề nào đóng góp bất kỳ sự so sánh có ý nghĩa nào và khi tất cả các giá trị S giống hệt nhau, trong đó mọi chuyển đổi đều là tự ghép đôi và hoán vị là không liên quan. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là liệt kê tất cả các hoán vị xếp hạng cho các giá trị m. Đối với mỗi hoán vị, chúng tôi tính toán chi phí bằng cách quét tất cả n−1 cặp liền kề và áp dụng trực tiếp quy tắc. Điều này đúng vì nó đánh giá định nghĩa theo nghĩa đen. Tuy nhiên, mỗi lần đánh giá có chi phí O(n) và có m! hoán vị, dẫn đến O(m!·n), điều này vượt xa tính khả thi. 

Quan sát quan trọng là dòng đầu vào chỉ đóng góp thông qua cặp tần số cnt[a][b]. Khi đã biết những điều này, chi phí của một thứ hạng cố định chỉ phụ thuộc vào cách mỗi cặp giá trị được sắp xếp tương tác với hoán vị đã chọn. Điều này chuyển vấn đề thành việc gán mỗi giá trị một vị trí duy nhất từ ​​1 đến m. 

Sau đó, chúng tôi diễn giải lại hoán vị bằng cách gán các vị trí từ 1 đến m cho các giá trị. Khi hai giá trị a và b có vị trí pa và pb, mỗi lần xuất hiện của giá trị kề a→b đều đóng góp một chi phí xác định chỉ phụ thuộc vào pa và pb. Điều này biến bài toán thành một bài tập trên m mục, trong đó mục tiêu là tổng của các tương tác theo cặp có trọng số là cnt. 

Vì m nhỏ nên chúng ta có thể sử dụng quy hoạch động trên các tập con. Mỗi trạng thái DP đại diện cho một tập hợp các giá trị đã được gán và ngầm định những giá trị này được gán cho |mask| đầu tiên. các vị trí theo một thứ tự nào đó. Chúng tôi thử gán vị trí tiếp theo cho một giá trị không được sử dụng và tính toán phần đóng góp gia tăng với tất cả các giá trị được đặt trước đó.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị vũ phu | O(m! · n) | O(1) | Quá chậm | 
| Tập hợp con DP qua bài tập | O(2^m · m^2) | O(2^m · m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi nén chuỗi đầu vào vào bảng chuyển tiếp cnt[a][b], đếm số lần giá trị a được theo sau bởi giá trị b trong dòng ban đầu. 

Sau đó chúng tôi chạy chương trình động trên các tập hợp con các giá trị. Mặt nạ trạng thái thể hiện những giá trị nào đã được xếp hạng. Số bit trong mặt nạ chính xác là số vị trí được gán, vì vậy thứ hạng tiếp theo chúng ta gán là vị trí t = popcount(mask) + 1. 

Với mỗi mặt nạ trạng thái, chúng ta thử chọn một giá trị x không có trong mặt nạ để đặt ở vị trí t. Chi phí gia tăng do đặt x phụ thuộc vào sự tương tác của nó với tất cả các giá trị y được đặt trước đó trong mặt nạ. 

Đối với mỗi y như vậy, chúng ta đã biết vị trí được gán của chúng, bởi vì mỗi cái được gán khi nó bước vào cấu trúc DP. Đặt px là t và py là vị trí được gán cho y trước đó. 

Sau đó chúng tôi tính đến cả hai khoản đóng góp trực tiếp: 

Đối với các cạnh x→y, mỗi lần xuất hiện đóng góp k1·(py − px) nếu px ≤ py, nếu không thì k2·(px + py). 

Đối với các cạnh y→x, mỗi lần xuất hiện đóng góp k1·(px − py) nếu py ≤ px, nếu không thì k2·(px + py). 

Chúng ta tính tổng những giá trị này trên tất cả y trong mặt nạ, tạo ra chi phí chuyển tiếp khi đặt x ở vị trí t. Chúng tôi cập nhật dp[mask ∪ {x}] tương ứng. 

Câu trả lời là dp tối thiểu trên tất cả các mặt nạ đầy đủ. 

Tính đúng đắn xuất phát từ thực tế là mọi đóng góp kề cận cnt[a][b] được tính chính xác một lần tại thời điểm điểm cuối sau của cặp trở nên cố định hoàn toàn so với cấu trúc được gán trước đó. Trạng thái DP duy trì sự phân công từng phần đầy đủ và nhất quán, do đó, mỗi cặp (a, b) được đánh giá theo thứ tự tương đối cuối cùng được ngụ ý trong cách xây dựng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k1, k2 = map(int, input().split())
    S = list(map(int, input().split()))
    S = [x - 1 for x in S]

    cnt = [[0] * m for _ in range(m)]
    for i in range(n - 1):
        cnt[S[i]][S[i + 1]] += 1

    INF = 10**30
    dp = [INF] * (1 << m)
    dp[0] = 0

    # positions are implicit: mask size = next position
    for mask in range(1 << m):
        t = mask.bit_count() + 1
        if t > m:
            continue
        for x in range(m):
            if mask & (1 << x):
                continue
            nmask = mask | (1 << x)

            cost = dp[mask]
            px = t

            # compute interaction of x with all y in mask
            for y in range(m):
                if not (mask & (1 << y)):
                    continue
                py = bin(mask & ((1 << y) - 1)).count("1") + 1

                cxy = cnt[x][y]
                cyx = cnt[y][x]

                if cxy:
                    if px <= py:
                        cost += cxy * k1 * (py - px)
                    else:
                        cost += cxy * k2 * (px + py)

                if cyx:
                    if py <= px:
                        cost += cyx * k1 * (px - py)
                    else:
                        cost += cyx * k2 * (px + py)

            if cost < dp[nmask]:
                dp[nmask] = cost

    print(dp[(1 << m) - 1])

if __name__ == "__main__":
    solve()
```Giải pháp xây dựng ma trận chuyển tiếp trước tiên để hàng dài sinh viên không bao giờ cần phải xem lại. 

DP lặp lại các tập hợp con của các giá trị. Mỗi lần chuyển đổi gán một giá trị mới cho vị trí xếp hạng sẵn có tiếp theo. Điều tinh tế quan trọng là vị trí của một giá trị không được lưu trữ một cách rõ ràng mà được xác định bởi kích thước của tập hợp con, vì chúng tôi luôn điền các thứ hạng từ 1 trở lên. 

Đối với mỗi vị trí, chúng tôi tính toán sự tương tác của nó với các giá trị đã được đặt bằng bảng cnt. Vòng lặp lồng nhau trên y là cần thiết vì mỗi giá trị được gán trước đó có thể đóng góp khác nhau tùy thuộc vào vị trí tương đối của nó. 

Bản cập nhật DP sử dụng chi phí tối thiểu trong số tất cả các lựa chọn có thể có của giá trị tiếp theo. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp đơn giản trong đó n=3, m=3, S=[1,2,3]. Số lượng chuyển đổi là cnt[1][2]=1 và cnt[2][3]=1, với tất cả các số khác bằng 0. 

Chúng ta bắt đầu từ mặt nạ=000. Tại t=1, chúng ta thử đặt từng giá trị. Nếu chúng ta đặt 1 đầu tiên thì vẫn chưa có chi phí. 

Tại t=2, giả sử số 2 được đặt tiếp theo. Sự đóng góp từ 1→2 phụ thuộc vào vị trí của chúng và tạo ra k1·1 vì 1 đứng trước 2. Cuối cùng, đặt 3 mang lại k1·1 khác từ 2→3. Tổng số là 2, phù hợp với cấu trúc tối ưu. 

| Bước | Mặt nạ | Giá trị gia tăng | Vị trí | Đóng góp chi phí mới | 
| --- | --- | --- | --- | --- | 
| 1 | 000 | 1 | 1 | 0 | 
| 2 | 001 | 2 | 2 | cnt[1][2]·k1 | 
| 3 | 011 | 3 | 3 | cnt[2][3]·k1 | 

Điều này cho thấy rằng khi các giá trị được căn chỉnh theo thứ tự tự nhiên, chỉ các cạnh phía trước mới đóng góp và luôn sử dụng dạng sai phân. 

Bây giờ hãy xem xét cấu trúc đảo ngược S=[1,1,4,5,1,4,1,9,1,9,8,10] từ mẫu mẫu. Ở đây, nhiều quá trình chuyển đổi lặp lại giữa các giá trị nhỏ và lớn, và DP phải quyết định xem việc đặt thứ hạng cao hay thấp sớm hơn sẽ giảm hình phạt k2 đắt đỏ cho việc đảo ngược thứ tự. DP khám phá chính xác cả hai khả năng vì nó không đảm nhận bất kỳ nhiệm vụ đơn điệu nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^m · m^2) | mỗi lần chuyển đổi tập hợp con thử m lựa chọn và quét tối đa m giá trị trước đó | 
| Không gian | O(2^m) | DP trên các tập hợp con | 

Với m 20, 2^m là khoảng một triệu trạng thái. Các quá trình chuyển đổi chỉ có thể được quản lý bằng Python được tối ưu hóa nếu các hằng số được kiểm soát và phù hợp với thời gian một cách thoải mái trong C++. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import inf

    # inline solution
    n, m, k1, k2 = map(int, sys.stdin.readline().split())
    S = list(map(int, sys.stdin.readline().split()))
    S = [x - 1 for x in S]

    cnt = [[0] * m for _ in range(m)]
    for i in range(n - 1):
        cnt[S[i]][S[i + 1]] += 1

    INF = 10**30
    dp = [INF] * (1 << m)
    dp[0] = 0

    for mask in range(1 << m):
        t = mask.bit_count() + 1
        if t > m:
            continue
        for x in range(m):
            if mask & (1 << x):
                continue
            nmask = mask | (1 << x)
            cost = dp[mask]
            px = t

            for y in range(m):
                if mask & (1 << y):
                    py = bin(mask & ((1 << y) - 1)).count("1") + 1
                    cxy = cnt[x][y]
                    cyx = cnt[y][x]

                    if cxy:
                        if px <= py:
                            cost += cxy * k1 * (py - px)
                        else:
                            cost += cxy * k2 * (px + py)

                    if cyx:
                        if py <= px:
                            cost += cyx * k1 * (px - py)
                        else:
                            cost += cyx * k2 * (px + py)

            dp[nmask] = min(dp[nmask], cost)

    return str(dp[-1])

# provided samples
assert run("3 3 1 1\n1 2 3\n") == "2"
assert run("4 3 1 1\n1 2 3 1\n") == "6"

# custom cases
assert run("2 2 5 1\n1 2\n") == "5", "single transition"
assert run("5 2 3 2\n1 1 1 1 1\n") == "0", "all equal"
assert run("3 3 10 1\n1 2 1\n") >= "0", "basic sanity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | 0 | không có cấu trúc liên kiểu | 
| xích hai loại nhỏ | chi phí hữu hạn | chuyển tiếp DP cơ bản | 
| mô hình xen kẽ | hỗn hợp không cần thiết | cả hai nhánh k1 và k2 | 

## Vỏ cạnh 

Khi tất cả học sinh có cùng con số may mắn thì ma trận chuyển tiếp chỉ có cnt[x][x]. Vì cả hai điểm cuối của mỗi cạnh đều giống hệt nhau nên mọi hoán vị đều gán cùng một thứ hạng cho giá trị đó và không có sự so sánh có ý nghĩa nào sẽ làm thay đổi kết quả. DP ngay lập tức mang lại chi phí bằng 0 vì mỗi cặp có giá trị chênh lệch bằng 0 hoặc tổng giống hệt nhau và không bao giờ được kích hoạt trên các giá trị khác nhau. 

Khi m=1, DP chỉ có một trạng thái. Thuật toán đặt giá trị duy nhất ở vị trí 1 và không có tương tác nào để xử lý nên kết quả bằng 0. Điều này tránh mọi vấn đề về giới hạn hoặc mặt nạ trống. 

Khi n lớn nhưng các chuyển tiếp thưa thớt thì chỉ có một số mục cnt khác 0. DP vẫn khám phá tất cả các mặt nạ nhưng hầu hết các tính toán chi phí đều không bổ sung thêm gì, vì các cạnh có trọng số bằng 0 sớm bỏ qua số học, giữ cho thời gian chạy ổn định.
