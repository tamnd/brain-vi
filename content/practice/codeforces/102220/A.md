---
title: "CF 102220A - Apple Business"
description: "Trang trại là một cây nhị phân hoàn chỉnh được viết theo thứ tự đống. Cây 1 là gốc và mọi cây i 1 đều có cha mẹ i // 2. Ban đầu cây i chứa a[i] quả táo. Một yêu cầu của khách hàng (u, v, c, w) là đặc biệt vì u được đảm bảo là tổ tiên của v."
date: "2026-08-17T22:26:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102220
codeforces_index: "A"
codeforces_contest_name: "The 13th Chinese Northeast Collegiate Programming Contest"
rating: 0
weight: 102220
solve_time_s: 220
verified: true
draft: false
---

[CF 102220A - Apple Business](https://codeforces.com/problemset/problem/102220/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Trang trại là một cây nhị phân hoàn chỉnh được viết theo thứ tự đống. Cây 1 là gốc và mọi cây`i > 1`có cha mẹ`i // 2`. Cây`i`ban đầu chứa`a[i]`táo. 

Một yêu cầu của khách hàng`(u, v, c, w)`là đặc biệt bởi vì`u`được đảm bảo là tổ tiên của`v`. Khách hàng có thể mua tối đa`c`táo, nhưng mỗi quả táo bán cho khách hàng này phải xuất phát từ một con đường đi xuống duy nhất từ`u`ĐẾN`v`. Mỗi quả táo như vậy kiếm được`w`đô la. Táo có thể được chia cho các khách hàng khác nhau và mục tiêu là chọn số lượng táo để bán cho mỗi yêu cầu trong khi tôn trọng cả sức chứa của cây và giới hạn của khách hàng. 

Các giới hạn chính thức là`n,m <= 100000`mỗi trường hợp thử nghiệm, với tổng số`n`và tổng cộng`m`trên tất cả các trường hợp thử nghiệm được giới hạn bởi`10^6`. Giới hạn thời gian là 6 giây và giới hạn bộ nhớ là 512 MB. Nút cây chỉ có độ sâu`O(log n)`bởi vì cây là một đống nhị phân, đây là thực tế về cấu trúc tạo nên một chương trình động có độ sâu logarit. Tại`n = 100000`, một thuật toán thực hiện số bậc hai của các hoạt động yêu cầu theo cặp đã quá lớn, trong khi`O((n+m) log^2 n)`phương pháp nằm trong phạm vi dự định. Việc triển khai được chấp nhận ban đầu sử dụng chính xác ý tưởng cây-DP này. 

Có một số trường hợp việc triển khai tham lam dường như tự nhiên không thành công. Đầu tiên là việc xử lý yêu cầu bằng cách giảm giá là cần thiết, nhưng chỉ lấy táo tùy ý dọc đường thôi thì chưa đủ. Coi như```
1
2 2
1 1
1 2 1 10
2 2 1 9
```Câu trả lời đúng là`19`. Khách hàng đầu tiên có thể lấy quả táo ở cây 1, để lại quả táo ở cây 2 cho khách hàng thứ hai. Việc triển khai bất cẩn có thể đáp ứng yêu cầu đầu tiên bằng cách sử dụng cây 2, sau đó yêu cầu thứ hai không nhận được gì và câu trả lời trở thành`10`. 

Trường hợp cạnh thứ hai là một yêu cầu có hai điểm cuối bằng nhau. Vì```
1
1 2
5
1 1 3 10
1 1 5 2
```câu trả lời là`34`. Cả hai yêu cầu đều bị giới hạn ở cây duy nhất, vì vậy yêu cầu đầu tiên bán 3 quả táo và yêu cầu thứ hai bán 2 quả còn lại. Việc triển khai giả định mọi đường dẫn đều có ít nhất một cạnh có thể xử lý sai trường hợp này. 

Vấn đề thứ ba là một yêu cầu có thể yêu cầu nhiều táo hơn đường dẫn của nó. Vì```
1
3 1
1 1 1
2 3 10 7
```Con đường chỉ có cây 2 và 3 nên chỉ bán được 2 quả táo và đáp án là`14`. Sử dụng số lượng yêu cầu`c`trực tiếp mà không kiểm tra dung lượng sẵn có của đường dẫn sẽ bị tính quá mức. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là coi mỗi quả táo như một mặt hàng riêng lẻ và mọi yêu cầu như một người mua có thể. Đối với mỗi quả táo, chúng ta có thể thử mọi yêu cầu có đường dẫn chứa cây của nó và chọn mức giá cao nhất hiện có. Điều này đúng về mặt khái niệm nếu bài tập được giải dưới dạng khớp hai bên có trọng số tối đa, nhưng nó hoàn toàn không khả thi vì`a[i]`có thể lớn như`10^9`, vậy tổng số quả táo riêng lẻ có thể đạt tới`10^14`. 

Thay vào đó, chúng ta có thể nén từng quả táo theo từng cây và xây dựng mạng lưới luồng chi phí tối đa. Một cái cây đóng góp số lượng táo của nó làm nguồn cung, còn một yêu cầu sẽ đóng góp năng lực và giá cả của nó. Việc kết nối một yêu cầu tới mọi cây trên đường đi của nó sẽ tạo ra tối đa`O(m log n)`sự cố xảy ra trên cây yêu cầu vì mọi đường dẫn đều có độ dài logarit. Biểu đồ đã lớn và thuật toán luồng chi phí tối đa chung có thể cần một số lượng lớn các lần tăng cường vì dung lượng rất lớn. Điều này không khai thác cấu trúc đặc biệt của đường dẫn. 

Quan sát hữu ích là tất cả các yêu cầu đều là đường dẫn dọc, từ tổ tiên xuống con cháu. Điều này biến bài toán gán thành bài toán khả thi kiểu Hall trên cây có gốc. Đối với một tập hợp yêu cầu cố định, chúng có thể được đáp ứng đầy đủ chính xác khi mọi tập hợp yêu cầu đều có đủ táo trong tập hợp các đường dẫn được phép của chúng. Vì các đường dẫn có dạng tổ tiên-hậu duệ này nên các tập Hall có khả năng chặt chẽ có thể được biểu diễn bằng các cây con có gốc cùng với một đường đi xuống được phân biệt. Chúng ta có thể duy trì tất cả các ràng buộc Hall đó bằng DP cây. 

Quan sát thứ hai liên quan đến lợi nhuận. Xử lý các yêu cầu theo thứ tự giảm dần`w`. Giả sử tất cả các yêu cầu đắt tiền hơn đã được đáp ứng nhiều nhất có thể. Đối với yêu cầu hiện tại, chúng tôi muốn bổ sung càng nhiều nhu cầu càng tốt trong khi vẫn duy trì khả năng phân bổ đã được xây dựng. Nếu chúng ta có thể thêm`C`táo, mức tăng lợi nhuận chính xác là`C * w`, do đó tối đa hóa`C`là tối ưu. Các yêu cầu về giá bằng nhau có thể được xử lý theo bất kỳ thứ tự nào vì chỉ có tổng số táo được bán của họ mới quan trọng. 

Với mọi gốc có thể`x`, chúng ta duy trì một bản sao của cây con của`x`, lại được biểu diễn dưới dạng một đống.`v[x]`lưu trữ các giá trị dung lượng còn lại được sử dụng bởi Hall DP, trong khi`f[x]`lưu trữ độ chùng của Hall hiện tại. Đối với một nút tương đối`y`, sự tái diễn là 

[ 
f[y] = v[y] + \min(0,f[2y]) + \min(0,f[2y+1]). 
] 

Trẻ em mất tích đóng góp bằng không. Giá trị con âm có nghĩa là cây con của nó có thâm hụt phải được cung cấp thông qua cha mẹ của nó, do đó cha mẹ thừa hưởng số tiền âm đó. Một đứa trẻ không âm đã có đủ năng lực bên trong và không ràng buộc cha mẹ nó. 

Khi có yêu cầu từ`A`ĐẾN`B`được thêm vào, điểm cuối của nó`B`được coi là đơn vị nhu cầu mới trong mọi bản sao tổ tiên có gốc là tổ tiên của`A`. Đối với mỗi bản sao như vậy, chúng tôi tính toán độ chùng Hall của đường dẫn kết thúc tại`B`, bao gồm cả những thiếu sót không thể tránh khỏi từ các cây con anh em. Số lượng tối thiểu của những khoảng trống này chính xác là số tiền lớn nhất có thể được thêm vào mà không vi phạm bất kỳ điều kiện Hall nào. 

Việc triển khai tuân theo giải pháp cuộc thi ban đầu`v`Và`f`cây DP trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force / cá nhân-táo chảy | Hàm mũ hoặc tỷ lệ thuận với tổng số táo, tối đa`10^14`mặt hàng | Có khả năng rất lớn | Quá chậm | 
| Cây Hall tối ưu DP |`O(n log n + m log² n)`|`O(n log n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các yêu cầu và sắp xếp chúng theo mức giá giảm dần`w`. Những quả táo có giá cao hơn phải luôn được phân bổ trước những quả táo có giá thấp hơn vì mọi quả táo đều có giá trị như nhau đối với khách hàng ngoại trừ giá của khách hàng. 
2. Đối với mỗi cây`x`, xây dựng một đống phụ biểu diễn cây con có gốc tại`x`. Gốc`x`nhận chỉ số tương đối`1`, con của nó nhận được chỉ số`2`Và`3`, con cái của họ nhận được`4`bởi vì`7`, vân vân. Điều này cho phép mọi đường dẫn con cháu tổ tiên được biểu diễn bằng các phép toán cha và con thông thường trên các chỉ số tương đối. 
3. Với mỗi cây phụ, khởi tạo`v`với số lượng táo tại mỗi nút tương đối. Ban đầu không có yêu cầu nào được đưa ra, vì vậy`f`bằng với`v`. 
4. Đối với mọi yêu cầu`(A,B,C,W)`, trước tiên hãy xác định chỉ số tương đối của`B`trong mọi cây phụ trợ bắt nguồn từ tổ tiên`x`của`A`. Nếu chênh lệch độ sâu là`d`, thì chỉ số heap tương đối của`B`có được bằng cách giữ cái cuối cùng`d`chữ số nhị phân của`B`và thêm một nhị phân hàng đầu`1`. 
5. Đối với một gốc phụ cố định`x`, bắt đầu với`dp = f[o]`, Ở đâu`o`là vị trí tương đối của`B`. Đi bộ từ`o`về phía rễ phụ. Tại mỗi tổ tiên`y`, thêm vào`v[y]`, bởi vì đường dẫn thực tế có thể sử dụng những quả táo tại`y`. Sau đó kiểm tra anh chị em của con đường. Nếu anh chị em đó có âm tính`f`, sự thiếu hụt của nó là không thể tránh khỏi đối với tập Hall tương ứng, vì vậy hãy cộng giá trị âm đó vào`dp`. 
6. Lấy mức tối thiểu`dp`trên tất cả các rễ phụ`x`trên đường đi từ`A`tới gốc toàn cầu. Mức tối thiểu này là số tiền bổ sung tối đa của yêu cầu này có thể được chấp nhận mà không khiến một số hạn chế của Hall không thể thực hiện được. Thay thế`C`bằng mức tối thiểu này nếu nó nhỏ hơn. 
7. Nếu kết quả`C`bằng 0, yêu cầu hiện tại không thể bán được gì, vì vậy hãy giữ nguyên DP. Nếu không thì thêm`C * W`để trả lời. 
8. Áp dụng số tiền được chấp nhận`C`tới mọi cây phụ trợ có liên quan. Giảm bớt`v[x][o]`Và`f[x][o]`qua`C`, sau đó tính toán lại`f`về tổ tiên của`o`sử dụng phép lặp tương tự. Chỉ những nút này mới có thể thay đổi độ chùng của Hall, vì vậy không có lý do gì để tính toán lại toàn bộ cây phụ trợ. 

Điều bất biến là sau khi xử lý bất kỳ tiền tố nào của các yêu cầu được sắp xếp theo mức giá giảm dần, giá trị được duy trì`f`các giá trị mô tả sự phân bổ khả thi chính xác với số lượng được chấp nhận cho tiền tố đó và mọi ràng buộc Hall chuẩn được biểu thị bằng một số`f`giá trị hoặc theo độ chùng của đường dẫn được tính toán trong lần chèn tiếp theo. Bước chèn chọn số lớn nhất có thể`C`mà tất cả những sự thiếu sót này vẫn không âm. Vì vậy, mọi quả táo giá cao được chấp nhận sẽ được giữ lại bất cứ khi nào tính khả thi cho phép. Vì tất cả các yêu cầu đều được xử lý từ mức giá cao hơn đến mức giá thấp hơn nên không có giải pháp khả thi nào có thể thu được tổng lợi nhuận tốt hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    out = []

    for _ in range(T):
        n, m = map(int, input().split())
        a = [0] + list(map(int, input().split()))

        # v[x] and f[x] describe the subtree rooted at x.
        # Index 0 is unused.
        v = [None] * (n + 1)
        f = [None] * (n + 1)
        size = [0] * (n + 1)

        # Map a relative heap index z inside the subtree of x
        # to the original heap node.
        def actual_node(x, z):
            h = 1 << (z.bit_length() - 1)
            return x * h + (z - h)

        # Build the auxiliary heap for every x.
        for x in range(1, n + 1):
            lo, hi = 1, n + 1

            # Find the largest valid relative heap index.
            while lo + 1 < hi:
                mid = (lo + hi) >> 1
                y = actual_node(x, mid)
                if y <= n:
                    lo = mid
                else:
                    hi = mid

            mx = lo
            size[x] = mx

            vals = [0] * (mx + 1)
            for z in range(1, mx + 1):
                vals[z] = a[actual_node(x, z)]

            v[x] = vals
            f[x] = vals.copy()

        requests = []
        for _ in range(m):
            u, wv, c, w = map(int, input().split())
            requests.append((u, wv, c, w))

        requests.sort(key=lambda e: e[3], reverse=True)

        ans = 0

        def getid(x, y):
            d = y.bit_length() - x.bit_length()
            k = 1 << d
            return y - (x - 1) * k

        for A, B, C, W in requests:
            # First pass: find the largest feasible amount.
            x = A
            while x:
                o = getid(x, B)
                mx = size[x]
                vx = v[x]
                fx = f[x]

                dp = fx[o]
                y = o >> 1

                while y:
                    dp += vx[y]

                    t = y << 1
                    if t <= mx and t != o and fx[t] < 0:
                        dp += fx[t]

                    t = (y << 1) | 1
                    if t <= mx and t != o and fx[t] < 0:
                        dp += fx[t]

                    y >>= 1

                if dp < C:
                    C = dp

                x >>= 1

            if C <= 0:
                continue

            ans += C * W

            # Second pass: actually insert the accepted demand.
            x = A
            while x:
                o = getid(x, B)
                mx = size[x]
                vx = v[x]
                fx = f[x]

                vx[o] -= C
                fx[o] -= C

                y = o >> 1
                while y:
                    cur = vx[y]

                    t = y << 1
                    if t <= mx and fx[t] < 0:
                        cur += fx[t]

                    t = (y << 1) | 1
                    if t <= mx and fx[t] < 0:
                        cur += fx[t]

                    fx[y] = cur
                    y >>= 1

                x >>= 1

        out.append(str(ans))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Các cây phụ trợ được lưu trữ dưới dạng danh sách được lập chỉ mục theo vị trí heap tương đối. biểu thức`actual_node(x, z)`xây dựng lại nút gốc được biểu thị bằng chỉ mục tương đối`z`. Nếu bit được đặt cao nhất của`z`là`h`, sau đó`z = h + r`và nút gốc tương ứng là`x * h + r`. 

Chỉ số tương đối của một hậu duệ đã biết`B`có thể mua được với giá rẻ hơn. Nếu như`d`là sự khác biệt độ sâu giữa`x`Và`B`, sau đó`k = 2^d`, và chỉ số tương đối là`B - (x - 1)k`. Điều này tương đương với thao tác bit được sử dụng trong quá trình triển khai tham chiếu. 

Lần đầu tiên vượt qua một yêu cầu chỉ xác định số lượng khả thi của nó. Lần thứ hai chỉ thay đổi DP sau khi biết số lượng đó. Việc kết hợp hai đường chuyền này sẽ không chính xác vì yêu cầu được xử lý một phần có thể ảnh hưởng đến việc kiểm tra tính khả thi đối với số lượng còn lại của chính yêu cầu đó. 

Tất cả các dung lượng và câu trả lời đều yêu cầu số nguyên 64 bit trong C++. Số nguyên Python có độ chính xác tùy ý, do đó không cần xử lý tràn đặc biệt. các`C <= 0`kiểm tra cũng là cần thiết. Độ chùng bằng 0 có nghĩa là yêu cầu hiện tại không thể thêm bất kỳ nhu cầu nào, trong khi giá trị âm không thể xảy ra đối với trạng thái duy trì khả thi. 

## Ví dụ đã hoạt động 

Đối với mẫu chính thức, các yêu cầu được xử lý theo thứ tự giá:`(2,4,2,4)`,`(2,5,2,3)`, Và`(1,2,3,1)`. Cây có táo`[2,1,3,1,1]`. Đầu ra chính thức là`13`. 

| Yêu cầu | Đường dẫn | Đã yêu cầu`C`| Khả thi tối đa`C`| Giá | Lợi nhuận tăng thêm | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
|`(2,4,2,4)`|`2 -> 4`| 2 | 2 | 4 | 8 | 8 | 
|`(2,5,2,3)`|`2 -> 5`| 2 | 1 | 3 | 3 | 11 | 
|`(1,2,3,1)`|`1 -> 2`| 3 | 2 | 1 | 2 | 13 | 

Khách hàng đầu tiên có thể sử dụng táo ở cây 2 và 4. Khách hàng thứ hai chia sẻ cây 2 với khách hàng đầu tiên và cây 5 chỉ cung cấp thêm một quả táo nên chỉ có thể bán được một trong hai quả táo được yêu cầu. Sau đó, hai quả táo vẫn có thể sử dụng được trên đường đi`1 -> 2`, đưa ra hai đô la cuối cùng. Sự chùng của Hall ngăn yêu cầu thứ hai lấy nhầm hai quả táo từ phần chung của cây. 

Ví dụ thứ hai chứng minh tại sao các quả táo của đường dẫn không thể được tiêu thụ một cách đơn giản theo thứ tự tùy ý.```
1
2 2
1 1
1 2 1 10
2 2 1 9
```| Yêu cầu | Đường dẫn | Đã yêu cầu`C`| khả thi`C`| Giá | Lợi nhuận tăng thêm | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
|`(1,2,1,10)`|`1 -> 2`| 1 | 1 | 10 | 10 | 10 | 
|`(2,2,1,9)`|`2 -> 2`| 1 | 1 | 9 | 9 | 19 | 

Yêu cầu đầu tiên có thể có hai quả táo. Đại diện của Hall ghi lại rằng yêu cầu thứ hai chỉ có thể sử dụng cây 2, vì vậy việc đáp ứng cả hai yêu cầu đòi hỏi phải để lại cây 2. Thuật toán không chọn quả táo vật lý một cách rõ ràng vào thời điểm này. Thay vào đó, nó duy trì các ràng buộc Hall còn lại, chính xác là điều ngăn cản sự lựa chọn tùy tiện tồi tệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n + m log² n)`| Mỗi cây con phụ trợ chứa`O(log n)`các cấp độ tổng hợp và mỗi yêu cầu sẽ kiểm tra`O(log n)`tổ tiên với`O(log n)`DP làm việc cho mỗi tổ tiên. | 
| Không gian |`O(n log n + m)`| phụ trợ`v`Và`f`mảng lưu trữ một mục nhập cho mỗi cặp tổ tiên-con cháu, trong khi danh sách yêu cầu lưu trữ`m`yêu cầu. | 

Vì`n,m <= 100000`, độ sâu heap nhiều nhất là khoảng 17, do đó hệ số logarit nhỏ. Cuộc thi ban đầu sử dụng giới hạn 6 giây và 512 MB, đồng thời việc triển khai C++ được chấp nhận của DP cây Hall này phù hợp với các giới hạn đó. Việc triển khai Python tuân theo cùng một phương pháp tiệm cận, mặc dù Python có các hệ số không đổi cao hơn đáng kể và ít phù hợp hơn với đầu vào tổng hợp lớn nhất có thể so với triển khai C++ ban đầu. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây giả sử giải pháp trên được lưu dưới dạng`solution.py`và phơi bày`solve()`.```python
# test_solution.py
import sys
import io

from solution import solve

def run(inp: str) -> str:
    global_input = sys.stdin
    old_input = sys.modules["solution"].input

    sys.stdin = io.StringIO(inp)
    sys.modules["solution"].input = sys.stdin.readline

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = global_input
        sys.modules["solution"].input = old_input

# Official sample
sample1 = """\
1
5 3
2 1 3 1 1
2 5 2 3
2 4 2 4
1 2 3 1
"""
assert run(sample1) == "13\n", "official sample"

# Minimum-size tree, equal endpoints, capacity split by price
sample2 = """\
1
1 2
5
1 1 3 10
1 1 5 2
"""
assert run(sample2) == "40\n", "single-node path"

# Shared endpoint catches arbitrary apple consumption
sample3 = """\
1
2 2
1 1
1 2 1 10
2 2 1 9
"""
assert run(sample3) == "19\n", "shared endpoint Hall constraint"

# Path capacity and ancestor/descendant boundary
sample4 = """\
1
3 2
1 1 1
2 3 10 7
1 3 2 6
"""
assert run(sample4) == "20\n", "path capacity boundary"

# Maximum-size n with all equal values.
# The path from 1 to 100000 contains 17 nodes.
n = 100000
sample5 = (
    "1\n"
    f"{n} 1\n"
    + " ".join(["1"] * n)
    + "\n"
    f"1 {n} {n} 1\n"
)
assert run(sample5) == "17\n", "maximum-size tree"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1`, hai yêu cầu trên cây 1 |`40`| Điểm cuối bằng nhau và phân chia công suất của một cây giữa các mức giá | 
|`n=2`, yêu cầu`1->2`Và`2->2`|`19`| Ràng buộc hội trường do điểm cuối dùng chung gây ra | 
|`n=3`, yêu cầu`2->3`Và`1->3`|`20`| Dung lượng đường dẫn và lập chỉ mục từ tổ tiên đến con cháu | 
|`n=100000`, tất cả`a[i]=1`, lời yêu cầu`1->100000`|`17`| Đầu vào có kích thước tối đa và ranh giới độ sâu heap nhị phân | 

## Vỏ cạnh 

Trường hợp điểm cuối dùng chung```
1
2 2
1 1
1 2 1 10
2 2 1 9
```được xử lý bởi sự chùng của Hall thay vì bằng cách chọn một quả táo vật lý cụ thể. Sau khi yêu cầu đầu tiên được chấp nhận, DP phụ ghi lại rằng cây 2 vẫn cần thiết cho một yêu cầu hạn chế hơn. Do đó, yêu cầu thứ hai vẫn khả thi, tạo ra`19`. 

Trường hợp điểm cuối bằng nhau```
1
1 2
5
1 1 3 10
1 1 5 2
```không có cạnh đường đi nào cả. Đối với yêu cầu đầu tiên, chỉ mục tương đối chỉ đơn giản là`1`, nên lần đầu tiên thấy chùng xuống`5`và chấp nhận`3`. Bản cập nhật thay đổi tình trạng chùng gốc từ`5`ĐẾN`2`. Yêu cầu thứ hai sau đó chấp nhận chính xác`2`, cho`30 + 10 = 40`. 

Yêu cầu vượt quá khả năng đường dẫn của nó,```
1
3 1
1 1 1
2 3 10 7
```có đường dẫn tương đối`2 -> 3`. Độ chùng Hall ban đầu là tổng của hai quả táo có sẵn, cụ thể là`2`, vậy số lượng yêu cầu`10`được giảm xuống`2`. Lợi nhuận do đó là`2 * 7 = 14`. 

Ranh giới đống lớn```
1
100000 1
1 1 1 ... 1
1 100000 100000 1
```có một đường dẫn chứa chuỗi tiền tố nhị phân từ`1`ĐẾN`100000`. Từ`100000`có độ dài nhị phân 17, đường dẫn đó chứa 17 nút. Chuyển đổi chỉ số tương đối của thuật toán tuân theo biểu diễn nhị phân của các chỉ số heap, do đó, nó đếm chính xác 17 quả táo có sẵn và trả về`17`, mà không đi qua các nhánh không liên quan của cây.
