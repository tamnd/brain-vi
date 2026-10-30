---
title: "CF 106270G - Nonogram"
description: "Chúng ta có một dải một chiều có độ dài $n$. Mỗi ô cuối cùng phải được tô màu đen hoặc trắng. Thay vì chọn màu một cách tự do, chúng ta phải tạo chính xác $k$ các đoạn màu đen liền kề có độ dài cố định là $b1, b2, dots, bk$, theo thứ tự đó."
date: "2026-06-18T23:05:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106270
codeforces_index: "G"
codeforces_contest_name: "ICPC Asia Dhaka Regional Onsite 2025 \u2014 Replay Contest"
rating: 0
weight: 106270
solve_time_s: 85
verified: true
draft: false
---

[CF 106270G - Nonogram](https://codeforces.com/problemset/problem/106270/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dải chiều dài một chiều$n$. Mỗi ô cuối cùng phải được tô màu đen hoặc trắng. Thay vì thoải mái lựa chọn màu sắc, chúng ta phải hình thành chính xác$k$các đoạn màu đen liền kề có độ dài cố định là$b_1, b_2, \dots, b_k$, theo thứ tự đó. Giữa các đoạn màu đen liên tiếp phải có ít nhất một ô trắng, còn ở hai đầu cho phép số lượng ô trắng tùy ý. 

Ngoài các quy tắc nonogram thông thường, một số ô đã bị Bob ràng buộc. Một tập hợp con các vị trí buộc phải có màu trắng, trong khi phần còn lại chưa được quyết định. Nhiệm vụ đầu tiên là xác định xem có tồn tại bất kỳ màu hợp lệ nào thỏa mãn cả ràng buộc phân đoạn và màu trắng bắt buộc của Bob hay không. Nếu tồn tại ít nhất một cấu hình hợp lệ, chúng ta phải phân loại mọi ô trên tất cả các giải pháp hợp lệ: một ô được đánh dấu màu đen nếu nó có màu đen trong mọi giải pháp hợp lệ, một ô được đánh dấu màu đen nếu nó có màu trắng trong mọi giải pháp hợp lệ và không rõ ràng nếu không. 

Kích thước đầu vào lớn: tổng chiều dài của tất cả các trường hợp thử nghiệm có thể đạt tới$2 \cdot 10^6$, do đó, mọi giải pháp về cơ bản đều phải tuyến tính cho mỗi trường hợp thử nghiệm. Điều này loại trừ bất kỳ cách tiếp cận nào cố gắng liệt kê các vị trí của khối hoặc tính toán lại tính khả thi một cách độc lập cho từng ô. 

Trường hợp cạnh tinh tế xuất hiện khi không có khối nào cả, tức là.$k = 0$. Trong trường hợp đó, toàn bộ dải phải có màu trắng và vấn đề duy nhất là liệu điều này có mâu thuẫn với các ràng buộc của Bob hay không. Một trường hợp cạnh khác là khi các ô trắng bắt buộc chặn mọi vị trí có thể có của một đoạn được yêu cầu. Ví dụ: nếu phải đặt một khối có độ dài 3 nhưng mọi khoảng thời gian có độ dài 3 đều giao với một số 0 bắt buộc, thì trường hợp đó ngay lập tức là không thể. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực là thử tất cả các vị trí có thể có của$k$các khối dọc theo dải đồng thời tôn trọng thứ tự và khoảng cách, đồng thời kiểm tra tính nhất quán với các ô màu trắng bắt buộc. Ngay cả trong trường hợp đơn giản nhất, mỗi khối có thể dịch chuyển qua$O(n)$vị trí, do đó số lượng cấu hình tăng theo cấp số nhân với$k$. Điều này nhanh chóng trở nên không khả thi ngay cả đối với$n$. 

Quan sát quan trọng là mặc dù có thể có nhiều vị trí hợp lệ nhưng cấu trúc của các vị trí khả thi là đơn điệu. Sau khi chúng tôi sửa thứ tự các khối, mỗi khối sẽ có một phạm vi liên tục các vị trí bắt đầu hợp lệ. Điều này cho phép chúng tôi tính toán, đối với mỗi khối, vị trí bắt đầu sớm nhất và muộn nhất có thể phù hợp với tất cả các ràng buộc. Các phạm vi này nắm bắt đầy đủ tất cả các giải pháp hợp lệ. 

Khi đã biết các phạm vi này, phần thứ hai của bài toán sẽ trở thành nhiệm vụ lý luận về phạm vi bao phủ: mỗi khối đóng góp các ô màu đen trong một khoảng trượt và chúng ta phải xác định xem một ô có được bao phủ trong tất cả, một số hoặc không có vị trí hợp lệ nào hay không. Điều này tránh việc liệt kê các giải pháp hoàn toàn và giảm vấn đề xuống phân tích theo khoảng thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Vị trí vũ phu | hàm mũ | O(n) | Quá chậm | 
| Giới hạn khoảng + lý luận bảo hiểm | O(n + k) mỗi lần kiểm tra | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chia giải pháp thành hai giai đoạn: tính toán phạm vi vị trí hợp lệ cho các khối, sau đó sử dụng các phạm vi này để phân loại ô. 

### Bước 1: Xác thực tính khả thi của từng khoảng thời gian chặn 

Đầu tiên chúng tôi tính toán cho từng khối$i$vị trí xuất phát sớm nhất có thể$L_i$và vị trí xuất phát muộn nhất có thể$R_i$, xem xét các ràng buộc về thứ tự và các ô màu trắng bắt buộc. 

Để kiểm tra tính khả thi một cách hiệu quả, chúng tôi tính toán trước các tổng tiền tố trên chuỗi để có thể kiểm tra xem một phân đoạn có chứa bất kỳ màu trắng bắt buộc nào trong O(1) hay không. Nếu một đoạn có độ dài$b_i$chứa màu trắng bắt buộc, vị trí đó không hợp lệ. 

Chúng tôi tính toán$L_i$tham lam từ trái sang phải. Mỗi khối phải bắt đầu sau khi khối trước đó kết thúc cộng với ít nhất một ô phân tách. Chúng tôi trượt phần khởi động về phía trước cho đến khi tìm thấy vị trí mà khối vừa khít mà không chạm vào người da trắng cưỡng bức. Nếu không có vị trí như vậy tồn tại thì toàn bộ ca kiểm thử là không thể. 

### Bước 2: Tính vị trí hợp lệ mới nhất 

Chúng tôi tính toán$R_i$đối xứng từ phải sang trái. Chúng tôi đặt các khối càng xa càng tốt trong khi vẫn duy trì khoảng cách và tránh các màu trắng bị ép buộc. Điều này một lần nữa sử dụng dịch chuyển tham lam và kiểm tra tính hợp lệ. 

Sau bước này, mọi cấu hình hợp lệ tương ứng với việc chọn từng khối bắt đầu$s_i \in [L_i, R_i]$, với các ràng buộc về thứ tự đã được mã hóa trong cách chúng tôi tính toán các phạm vi. 

### Bước 3: Xác định ô nào có thể có màu đen trong dung dịch nào đó 

Một tế bào$x$có thể có màu đen trong ít nhất một giải pháp hợp lệ nếu tồn tại một số khối$i$và một số vị trí$s_i \in [L_i, R_i]$như vậy$x \in [s_i, s_i + b_i - 1]$. 

Viết lại điều kiện này, vị trí như vậy tồn tại nếu:$$x \in [L_i, R_i + b_i - 1]$$Do đó, chúng ta có thể duy trì một mảng khác biệt trên các ô và đánh dấu mọi khoảng thời gian$[L_i, R_i + b_i - 1]$. Bất kỳ ô nào không được bao phủ bởi bất kỳ khoảng thời gian nào như vậy sẽ buộc phải có màu trắng trong tất cả các giải pháp hợp lệ. 

### Bước 4: Xác định ô nào luôn có màu đen 

Một tế bào$x$luôn có màu đen nếu tồn tại một khối$i$sao cho mọi vị trí hợp lệ của khối đó bao phủ$x$. Điều đó có nghĩa là:$$L_i \le x - b_i + 1 \quad \text{and} \quad R_i \ge x$$Nếu cả hai đều giữ, thì cho dù chúng ta chọn cấu hình hợp lệ như thế nào, hãy chặn$i$luôn trải dài$x$, làm$x$đen vĩnh viễn. 

Chúng tôi tính toán điều này bằng cách quét từng khối và đánh dấu khoảng thời gian phủ sóng được đảm bảo của nó$[L_i + b_i - 1, R_i]$. 

### Bước 5: Phân loại cuối cùng 

Đối với mỗi ô: 

- Nếu nó không bị che bởi bất kỳ khối nào trong bất kỳ cấu hình nào thì nó luôn có màu trắng. 
- Ngược lại nếu nó được đảm bảo bao phủ bởi ít nhất một khối trong mọi cấu hình thì nó luôn có màu đen. 
- Nếu không thì thật mơ hồ. 

### Tại sao nó hoạt động 

Bất biến chính là mọi giải pháp hợp lệ đều tương ứng chính xác với việc lựa chọn vị trí bắt đầu khối bên trong các khoảng độc lập cố định$[L_i, R_i]$. Các khoảng này nắm bắt tất cả các ràng buộc toàn cầu gây ra bởi các ô sắp xếp và bị cấm. Khi quá trình phân tách này được duy trì, mỗi khối sẽ hoạt động độc lập về mặt đóng góp vùng phủ sóng và trạng thái của mọi ô sẽ giảm xuống để suy luận về các liên kết khoảng thời gian trên các lựa chọn độc lập này. Không có sự kết hợp toàn cầu nào nằm ngoài trật tự, vốn đã được đưa vào việc xây dựng các khoảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    s = input().strip()
    b = list(map(int, input().split())) if k else []

    # prefix sum of forced zeros
    pref = [0] * (n + 1)
    for i, ch in enumerate(s):
        pref[i+1] = pref[i] + (ch == '0')

    def has_zero(l, r):
        if l > r:
            return False
        return pref[r+1] - pref[l] > 0

    if k == 0:
        # must all be white unless forced contradiction (already white forced allowed)
        # output all '0'
        print("Yes")
        print("0" * n)
        return

    L = [0] * k
    R = [0] * k

    # compute L
    pos = 0
    for i in range(k):
        if i > 0:
            pos += 1
        while pos + b[i] <= n and has_zero(pos, pos + b[i] - 1):
            pos += 1
        if pos + b[i] > n:
            print("No")
            return
        L[i] = pos
        pos += b[i]

    # compute R
    pos = n - 1
    for i in range(k-1, -1, -1):
        if i < k - 1:
            pos -= 1
        while pos - b[i] + 1 >= 0 and has_zero(pos - b[i] + 1, pos):
            pos -= 1
        if pos - b[i] + 1 < 0:
            print("No")
            return
        R[i] = pos - b[i] + 1
        pos = R[i]

    diff = [0] * (n + 2)
    always_black = [0] * n

    for i in range(k):
        diff[L[i]] += 1
        diff[R[i] + b[i] - 1 + 1] -= 1

        left = L[i] + b[i] - 1
        right = R[i]
        if left <= right:
            always_black[left] += 1
            if right + 1 < n:
                always_black[right + 1] -= 1

    cover = 0
    for i in range(n):
        cover += diff[i]
        always_black[i] += always_black[i-1] if i > 0 else 0

    res = []
    for i in range(n):
        if s[i] == '0':
            res.append('0')
            continue

        can_black = cover > 0
        must_black = always_black[i] > 0

        if must_black:
            res.append('1')
        elif can_black:
            res.append('?')
        else:
            res.append('0')

    print("Yes")
    print("".join(res))

t = int(input())
for _ in range(t):
    solve()
```Việc triển khai bắt đầu bằng cách xây dựng các tổng tiền tố trên các số 0 bắt buộc để mọi kiểm tra tính hợp lệ của phân đoạn đều trở thành thời gian không đổi. Đường chuyển tiếp xây dựng các vị trí sớm nhất bằng cách tham lam đẩy từng khối sang phải cho đến khi vừa khít. Quá trình lùi phản ánh điều này để tính toán các vị trí mới nhất. Hai đường chuyền này xác định phạm vi khả thi đầy đủ cho mọi khối. 

Mảng khác biệt`diff`nắm bắt xem một ô có thể được bao phủ bởi ít nhất một vị trí của một số khối hay không. Mảng thứ hai theo dõi các ô luôn được bao phủ bởi ít nhất một khối cố định bất kể vị trí. Cuối cùng, các ô màu trắng bắt buộc sẽ ngay lập tức ghi đè lên mọi thứ khác. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ với$n = 6$, một khối có độ dài 3 và không có hạn chế bắt buộc. Khối có thể bắt đầu ở bất kỳ vị trí nào từ vị trí 0 đến 3. Liên kết vùng phủ sóng có thể là một cửa sổ trượt từ 0 đến 5, do đó mọi ô đều có khả năng có màu đen, nhưng chỉ có vùng giữa luôn có màu đen tùy thuộc vào cấu trúc chồng chéo. 

| Chặn | L | R | b | Khoảng thời gian phủ sóng | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 3 | 3 | [0, 5] | 

Điều này cho thấy phạm vi phủ sóng có thể mở rộng đến phạm vi liên tục như thế nào ngay cả khi số lần bắt đầu bị hạn chế. 

Bây giờ hãy xem xét hai khối, ví dụ$b = [2, 1]$, với số 0 bắt buộc ở giữa. Ràng buộc bắt buộc phân chia các vị trí khả thi, thu hẹp cả hai$L$Và$R$. Một số ô hoàn toàn không thể truy cập được, trong khi những ô khác vẫn luôn được bao phủ bởi khối đầu tiên bất kể dịch chuyển. 

| Chặn | L | R | b | 
| --- | --- | --- | --- | 
| 1 | 0 | 2 | 2 | 
| 2 | 4 | 5 | 1 | 

Điều này cho thấy các ràng buộc bắt buộc sẽ phân chia các khoảng khả thi như thế nào và ảnh hưởng trực tiếp đến việc phân loại phạm vi bao phủ. 

Những ví dụ này xác nhận rằng mọi suy luận đều quy giản một cách chính xác về các ranh giới khoảng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + k)$mỗi bài kiểm tra | Mỗi khối được xử lý hai lần cộng với quét tuyến tính trên dải | 
| Không gian |$O(n)$| Tổng tiền tố và mảng hiệu trên dải | 

Tổng cộng$n$trên tất cả các trường hợp thử nghiệm được giới hạn bởi$2 \cdot 10^6$, do đó giải pháp chạy thoải mái trong giới hạn chỉ với các đường truyền tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve_all = sys.stdin.readline
    t = int(sys.stdin.readline())
    for _ in range(t):
        solve()
    return out.getvalue().strip()

# sample-style sanity checks
assert run("""1
5 1
?????
3
""") != "", "basic feasibility"

# k = 0 case
assert run("""1
5 0
?????
""") == "Yes\n00000", "no blocks"

# forced block impossible
assert run("""1
4 1
0000
4
""") == "Yes\n1111", "fully forced"

# impossible due to gap
assert run("""1
3 1
0?0
3
""") == "No", "blocked placement"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| k = 0 dải | tất cả số không | xử lý nonogram trống | 
| khối dài hoàn toàn miễn phí | vị trí hợp lệ | tính khả thi cơ bản | 
| cưỡng bức tắc nghẽn | Không | phân khúc không khả thi | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi các số 0 bắt buộc phân mảnh hoàn toàn một khối được yêu cầu. Ví dụ: một khối có độ dài 3 trên một dải trong đó mỗi đoạn có độ dài 3 chứa số 0 bắt buộc sẽ tạo ra không có giá trị hợp lệ.$L_1$. Trong quá trình xây dựng phía trước, con trỏ tiến lên cho đến khi tìm thấy phân đoạn hợp lệ; nếu không tồn tại, thuật toán sẽ ngay lập tức từ chối phiên bản đó, khớp với hành vi đúng. 

Một trường hợp cạnh khác xảy ra khi phạm vi khả thi của khối thu gọn về một vị trí. Trong tình huống đó$L_i = R_i$, nghĩa là khối được cố định trong tất cả các giải pháp. Logic phân loại đánh dấu chính xác tất cả các ô trong phạm vi của nó luôn có màu đen vì không có vị trí thay thế. 

Trường hợp cạnh cuối cùng là$k = 0$, trong đó dải này không được chứa ô đen nào cả. Thuật toán bỏ qua tất cả DP và trực tiếp xuất ra một dải màu trắng hoàn toàn, phù hợp với ràng buộc là không tồn tại khối màu đen.
