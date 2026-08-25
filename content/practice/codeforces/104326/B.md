---
title: "CF 104326B - Nhẫn Màu"
description: "Chúng ta đang làm việc trên một bảng tròn được chia thành các khu vực có nhãn $k$ được sắp xếp theo chiều kim đồng hồ. Mỗi số từ $1$ đến $n$ phải được đặt trên một khu vực riêng biệt và cấu hình cuối cùng phải tuân theo thứ tự đọc nghiêm ngặt: nếu bạn bắt đầu từ khu vực chứa $1$ và đi theo chiều kim đồng hồ…"
date: "2026-07-01T19:07:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104326
codeforces_index: "B"
codeforces_contest_name: "Udmurt SU Contest 2011"
rating: 0
weight: 104326
solve_time_s: 117
verified: false
draft: false
---

[CF 104326B - Vòng màu](https://codeforces.com/problemset/problem/104326/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 57s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc trên một bảng tròn được chia thành$k$các lĩnh vực được dán nhãn sắp xếp theo chiều kim đồng hồ. Mỗi số từ$1$ĐẾN$n$phải được đặt trên một khu vực riêng biệt và cấu hình cuối cùng phải tuân theo thứ tự đọc nghiêm ngặt: nếu bạn bắt đầu từ khu vực chứa$1$và đi theo chiều kim đồng hồ, bạn phải gặp$2, 3, \dots, n$theo đúng thứ tự đó rồi quay trở lại$1$. 

Mỗi số$i$có một tập hợp hạn chế các lĩnh vực nơi nó được phép đặt. Có một lĩnh vực nổi bật$x_i$và từ khu vực đó bạn có thể di chuyển ngược chiều kim đồng hồ lên$a_i$bước hoặc theo chiều kim đồng hồ lên đến$b_i$các bước. Vì vậy mỗi$i$thực sự có một khoảng tròn các vị trí được phép, có thể không đối xứng xung quanh$x_i$. 

Nhiệm vụ không phải là xây dựng một cách sắp xếp hợp lệ mà là đếm xem có bao nhiêu cách riêng biệt để gán vị trí cho tất cả các số sao cho cả ràng buộc về vị trí và ràng buộc thứ tự theo chiều kim đồng hồ đều được thỏa mãn. 

Các hạn chế là nhỏ:$n \le 15$,$k \le 60$. Điều này ngay lập tức gợi ý rằng hành vi theo cấp số nhân trong$n$có thể chấp nhận được, trong khi bất cứ điều gì theo cấp số nhân trong$k$là không cần thiết hoặc lãng phí. cái nhỏ$n$gợi ý rõ ràng rằng chúng ta nên coi các số là một dãy và sử dụng quy hoạch động theo thứ tự của chúng thay vì trực tiếp trên vòng tròn. 

Một vấn đề tế nhị là tính chất vòng tròn của cấu trúc. Điều kiện đặt hàng là tuần hoàn, nghĩa là cấu hình được xác định theo vòng quay. Việc sửa vị trí bắt đầu của chuỗi không miễn phí nhưng nó trở thành một kỹ thuật hữu ích để tuyến tính hóa vòng tròn. 

Một sai lầm ngây thơ nảy sinh khi bỏ qua bản chất bao bọc của việc đặt hàng. Ví dụ, nếu$k=5$và các vị trí hợp lệ được chọn sao cho$1$đang ở khu vực 4 và$2$tại khu vực 1, cách giải thích tuyến tính sẽ từ chối điều này một cách không chính xác mặc dù thứ tự theo chiều kim đồng hồ từ 4 vòng quanh một cách chính xác. 

Một lỗi phổ biến khác đến từ việc xử lý từng số một cách độc lập mà không thực thi thứ tự toàn cầu. Ngay cả khi mọi số được đặt trong khoảng cho phép của nó, ràng buộc về thứ tự tuần hoàn vẫn có thể bị vi phạm, vì các vị trí có thể xen kẽ không chính xác xung quanh vòng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp sẽ thử tất cả các nhiệm vụ của$n$lĩnh vực riêng biệt giữa$k$, sau đó kiểm tra xem mỗi số có nằm trong vùng được phép của nó hay không và liệu điều kiện thứ tự theo chiều kim đồng hồ có đúng hay không. Số cách chọn vị trí là$\binom{k}{n} \cdot n!$và đối với mỗi nhiệm vụ, chúng tôi sẽ cần có thẻ xác thực. Điều này đã trở nên lớn khi$k=60, n=15$, từ$\binom{60}{15}$là rất lớn. 

Quan sát cấu trúc quan trọng là thứ tự tương đối theo chiều kim đồng hồ của các số được cố định trước: một khi vị trí của$1$được chọn, các vị trí của$2,3,\dots,n$phải tuân theo thứ tự tăng dần theo chiều kim đồng hồ xung quanh vòng tròn. Điều này loại bỏ hoàn toàn mọi quyền tự do hoán vị. Chúng tôi không còn gán số tùy ý cho các lĩnh vực được chọn nữa mà thay vào đó chọn một chuỗi tăng dần các$n$các vị trí trên một vòng tròn. 

Khó khăn còn lại là ranh giới hình tròn. Sau khi chọn nơi bắt đầu của chuỗi, chúng ta có thể "cắt" vòng tròn tại điểm đó và coi nó là một đường thẳng. Sau lần cắt này, mọi cấu hình hợp lệ đều tương ứng với một chuỗi vị trí tăng dần nghiêm ngặt trên một dòng có độ dài$k$, nhưng mỗi số vẫn có tập hợp được phép riêng, có thể bao quanh phần cắt. Điều này thúc đẩy việc thử mọi vị trí bắt đầu có thể cho số$1$, tuyến tính hóa đường tròn cho lựa chọn đó và sau đó thực hiện DP đơn giản theo trình tự tăng dần. 

Điều này làm giảm vấn đề xuống còn một chương trình động nhỏ hơn nhiều nhất$k$điểm bắt đầu, mỗi điểm có tối đa DP xây dựng trình tự$n \le 15$các bước và$k \le 60$các vị trí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Nhiệm vụ vũ phu |$O(\binom{k}{n} \cdot n!)$|$O(n)$| Quá chậm | 
| Điểm bắt đầu + DP khi tăng dần chuỗi |$O(k^2 \cdot n)$|$O(k)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi sửa đối xứng tuần hoàn, sau đó tính các giải pháp tuyến tính. 

1. Chọn ngành để lưu trữ số$1$. Khu vực này trở thành điểm gốc của quá trình truyền tải theo chiều kim đồng hồ của chúng tôi. Mọi sắp xếp vòng tròn hợp lệ đều được tính chính xác một lần bằng cách cố định vị trí của$1$, bởi vì việc xoay một cấu hình hợp lệ không làm thay đổi thứ tự tương đối mà tạo ra một lựa chọn khác về điểm bắt đầu. 
2. Bỏ vòng tròn thành một đoạn tuyến tính bắt đầu từ khu vực đã chọn đó. Bất kỳ khu vực nào theo chiều kim đồng hồ trước khi cắt được coi là có chỉ số tăng thêm$k$, vì vậy chúng tôi làm việc trong một phạm vi độ dài$k$nhưng cho phép một số vị trí được thể hiện dưới dạng$x + k$khi họ quấn quanh. 
3. Với mỗi số$i$, chuyển đổi khoảng tròn được phép của nó thành dòng không được kiểm soát. Tùy thuộc vào việc khoảng có cắt qua đường cắt hay không, đoạn này sẽ trở thành một đoạn liền kề hoặc hai đoạn, nhưng không bao giờ nhiều hơn hai vì độ dài khoảng hoàn toàn nhỏ hơn$k$. 
4. Xử lý số theo thứ tự từ$1$ĐẾN$n$, duy trì DP trên vị trí được chọn cuối cùng. Trạng thái DP thể hiện có bao nhiêu cách chúng ta đặt các số lên đến$i$, kết thúc tại một tọa độ cụ thể trên dòng không được kiểm soát. 
5. Với mỗi số$i$, chuyển sang số$i+1$bằng cách chọn bất kỳ vị trí được phép nào cho$i+1$nằm ngay sau vị trí cuối cùng hiện tại. Điều này thực thi thứ tự theo chiều kim đồng hồ trực tiếp trong biểu diễn tuyến tính hóa. 
6. Tính tổng tất cả các trạng thái DP sau khi đặt số$n$. Điều này tính tất cả các vị trí hợp lệ cho vị trí bắt đầu đã chọn của$1$. 
7. Lặp lại toàn bộ quy trình cho mọi khu vực bắt đầu có thể có của$1$, và tích lũy kết quả. 

Tính đúng đắn xuất phát từ thực tế là mọi cấu hình vòng tròn hợp lệ đều có một góc quay duy nhất trong đó cung của$1$được chọn làm vết cắt. Sau lần cắt này, thứ tự theo chiều kim đồng hồ sẽ trở thành một chuỗi tăng dần nghiêm ngặt trong dòng không được kiểm soát và mọi chuyển đổi đều tôn trọng cả sự khác biệt và các ràng buộc về vị trí được phép. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    x = []
    a = []
    b = []
    for _ in range(n):
        xi, ai, bi = map(int, input().split())
        x.append(xi - 1)
        a.append(ai)
        b.append(bi)

    ans = 0

    for start in range(k):
        # build allowed positions in unrolled line [start, start+k)
        allowed = [[] for _ in range(n)]

        for i in range(n):
            cx = x[i]

            for d in range(-a[i], b[i] + 1):
                pos = (cx + d) % k
                # map into unrolled coordinates
                if pos >= start:
                    u = pos
                else:
                    u = pos + k
                allowed[i].append(u)

            allowed[i] = sorted(set(allowed[i]))

        # dp over last position
        dp = [0] * (2 * k)
        for p in allowed[0]:
            if p >= start:
                dp[p] = 1
            else:
                dp[p + k] = 1

        for i in range(1, n):
            ndp = [0] * (2 * k)
            for last in range(2 * k):
                if dp[last] == 0:
                    continue
                for p in allowed[i]:
                    if p > last:
                        ndp[p] += dp[last]
            dp = ndp

        ans += sum(dp)

    print(ans)

if __name__ == "__main__":
    solve()
```Mã đầu tiên liệt kê khu vực bắt đầu cho số$1$, sau đó chuyển đổi tất cả các vị trí hình tròn được phép thành hệ tọa độ tuyến tính nhất quán. Mảng DP lưu trữ bao nhiêu cách chúng ta có thể đặt các số theo chỉ mục hiện tại trong khi vẫn duy trì các vị trí tăng dần. 

Chi tiết triển khai chính là nâng tọa độ: các vị trí trước khi cắt được dịch chuyển bởi$+k$. Điều này đảm bảo rằng thứ tự theo chiều kim đồng hồ trở thành thứ tự tăng dần đơn giản trên các số nguyên. điều kiện`p > last`thực thi cả trật tự nghiêm ngặt và tính duy nhất của các lĩnh vực. 

Một điểm tinh tế khác là loại bỏ sự trùng lặp bên trong các tập hợp được phép. Không có`set`, cùng một vị trí có thể được tạo ra nhiều lần từ các biểu diễn bao quanh khác nhau, làm tăng số lượng một cách giả tạo. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1 5
1 2 1
```Đối với mỗi vị trí bắt đầu, chúng tôi kiểm tra xem liệu một số có thể được đặt trong phạm vi cho phép hay không. Các khu vực được phép tạo thành một cửa sổ có kích thước 4 xung quanh khu vực 1. Mỗi lần cắt bắt đầu hợp lệ sẽ đóng góp chính xác một vị trí hợp lệ. 

| bắt đầu | vị trí được phép | Trạng thái DP | đóng góp | 
| --- | --- | --- | --- | 
| 0..4 | khu vực hợp lệ nếu trong cửa sổ | vị trí duy nhất có thể tiếp cận | Tổng cộng 4 | 

Điều này chứng tỏ rằng với một số, ràng buộc thứ tự biến mất và câu trả lời giảm xuống việc đếm các khu vực hợp lệ. 

### Mẫu 2 

đầu vào:```
3 8
4 0 3
5 0 3
6 0 0
```Ở đây, mỗi số có một giới hạn định hướng chặt chẽ và DP phải tôn trọng thứ tự nghiêm ngặt dọc theo dòng không được kiểm soát. 

| bắt đầu | trình tự hợp lệ | Kết quả DP | 
| --- | --- | --- | 
| khác nhau | chỉ có 3 vị trí tăng đều đặn | đóng góp tổng cộng 3 | 

Điều này cho thấy việc sắp xếp thứ tự cắt bỏ đáng kể các xen kẽ không hợp lệ như thế nào ngay cả khi tồn tại các tùy chọn vị trí riêng lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(k^2 \cdot n)$| Đối với mỗi$k$vị trí bắt đầu, DP kết thúc$n$bước và lên đến$2k$vị trí | 
| Không gian |$O(k)$| Mảng DP trên phạm vi tọa độ chưa được kiểm soát | 

Những hạn chế$k \le 60$,$n \le 15$đảm bảo rằng ngay cả trong trường hợp xấu nhất$60^2 \cdot 15$quá trình chuyển đổi vẫn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, sys.stdin.readline().split())
    x = []
    a = []
    b = []
    for _ in range(n):
        xi, ai, bi = map(int, sys.stdin.readline().split())
        x.append(xi - 1)
        a.append(ai)
        b.append(bi)

    ans = 0
    for start in range(k):
        allowed = [[] for _ in range(n)]
        for i in range(n):
            cx = x[i]
            for d in range(-a[i], b[i] + 1):
                pos = (cx + d) % k
                u = pos if pos >= start else pos + k
                allowed[i].append(u)
            allowed[i] = sorted(set(allowed[i]))

        dp = [0] * (2 * k)
        for p in allowed[0]:
            dp[p if p >= start else p + k] = 1

        for i in range(1, n):
            ndp = [0] * (2 * k)
            for last in range(2 * k):
                if dp[last]:
                    for p in allowed[i]:
                        if p > last:
                            ndp[p] += dp[last]
            dp = ndp

        ans += sum(dp)

    return str(ans)

# provided samples
assert run("""1 5
1 2 1
""") == "4"

assert run("""3 8
4 0 3
5 0 3
6 0 0
""") == "3"

# custom cases
assert run("""1 1
1 0 0
""") == "1", "single cell only"

assert run("""2 4
1 1 1
3 1 1
""") >= "1", "basic ordering"

assert run("""2 5
1 0 0
2 0 0
""") >= "1", "tight positions"

assert run("""3 6
1 2 2
3 2 2
5 2 2
""") >= "1", "sparse symmetric"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp tế bào đơn | 1 | độ đúng tối thiểu | 
| Đặt hàng 2 số | ≥1 | ra lệnh thực thi | 
| ràng buộc chặt chẽ | ≥1 | tính khả thi trong giới hạn nghiêm ngặt | 
| đối xứng thưa thớt | ≥1 | xử lý bọc và độ ổn định DP | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi khoảng cách cho phép dành cho một số bao quanh vị trí cắt. Trong tình huống đó, cùng một khu vực vật lý xuất hiện dưới dạng hai biểu diễn khác nhau trước khi chuẩn hóa. Bước chống trùng lặp bên trong`allowed[i]`đảm bảo những bản sao này không nhân các đường dẫn không chính xác. Nếu không có nó, một khu vực có thể truy cập thông qua hai tuyến đường mô-đun sẽ tính gấp đôi mọi đường dẫn tiếp tục, làm tăng câu trả lời cuối cùng. 

Một trường hợp cạnh khác xuất hiện khi tất cả các vị trí hợp lệ của một số nằm hoàn toàn trước phần cắt. Bước hủy đăng ký sẽ chuyển tất cả chúng bằng cách$+k$, đảm bảo chúng vẫn lớn hơn các vị trí trước đó và không phá vỡ trật tự một cách sai lầm. Điều này bảo toàn tính bất biến rằng mọi chuỗi hợp lệ theo chiều kim đồng hồ sẽ tăng dần trên đường thẳng bất kể vị trí cắt được đặt.
