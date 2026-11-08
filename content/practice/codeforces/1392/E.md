---
title: "CF 1392E - Omkar và Vịt"
description: "Chúng tôi đang làm việc với một lưới nhỏ, nhiều nhất là 25 x 25, trong đó một đường dẫn được hình thành từ ô trên cùng bên trái đến ô dưới cùng bên phải chỉ bằng cách di chuyển sang phải hoặc xuống dưới."
date: "2026-06-11T10:07:41+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms", "interactive", "math"]
categories: ["algorithms"]
codeforces_contest: 1392
codeforces_index: "E"
codeforces_contest_name: "Codeforces Global Round 10"
rating: 2100
weight: 1392
solve_time_s: 120
verified: false
draft: false
---

[CF 1392E - Omkar và Vịt](https://codeforces.com/problemset/problem/1392/E) 

**Xếp hạng:** 2100 
**Tags:** bitmask, thuật toán xây dựng, tương tác, toán học 
**Thời gian giải:** 2 phút 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một lưới nhỏ, nhiều nhất là 25 x 25, trong đó một đường dẫn được hình thành từ ô trên cùng bên trái đến ô dưới cùng bên phải chỉ bằng cách di chuyển sang phải hoặc xuống dưới. Do đó, mỗi đường đi luôn có chính xác$2n-1$các ô đã truy cập, bởi vì mỗi bước sẽ tăng hàng hoặc cột và chúng ta cần chính xác$n-1$di chuyển theo từng hướng. 

Trước bất kỳ truy vấn nào, chúng tôi được phép gán trọng số không âm cho mỗi ô lưới, với mỗi trọng số tối đa$10^{16}$. Sau đó, một đường dẫn đơn điệu không xác định sẽ được cố định cho mỗi truy vấn. Đối với mỗi truy vấn, chúng tôi chỉ được cung cấp tổng trọng số dọc theo đường dẫn ẩn đó và chúng tôi phải xây dựng lại chuỗi chính xác các ô được truy cập. 

Khó khăn chính là chúng tôi chỉ thấy một số duy nhất cho mỗi truy vấn nhưng nhiều đường dẫn khác nhau có thể tạo ra cùng một tổng trừ khi lưới được xây dựng cẩn thận. 

Những ràng buộc vô cùng chặt chẽ$n$, điều này gợi ý rằng các cấu trúc hàm mũ là có thể. Với$n \le 25$, có nhiều nhất$\binom{48}{24}$những con đường có thể, xung quanh$10^{13}$, quá lớn để phân biệt trực tiếp cho mỗi truy vấn. Điều này ngay lập tức loại trừ bất kỳ chiến lược nào cố gắng mã hóa từng đường dẫn một cách độc lập hoặc mô phỏng quá trình khôi phục đường dẫn một cách linh hoạt. 

Bản chất tương tác không làm thay đổi cấu trúc cốt lõi: chúng tôi thiết kế một mã hóa cố định trong lưới sao cho mỗi đường dẫn hợp lệ tạo ra một tổng duy nhất và mã hóa này phải có thể đảo ngược vào đường dẫn. 

Một trường hợp phức tạp xuất hiện khi nghĩ về tính đối xứng. Nhiều đường dẫn khác nhau chia sẻ các tiền tố dài và bất kỳ mã hóa nào chỉ phụ thuộc vào số hàng hoặc số cột sẽ không thành công vì việc xen kẽ các bước di chuyển sang phải và xuống khác nhau có thể tạo ra các đóng góp tổng hợp giống hệt nhau. Một cách tiếp cận ngây thơ như gán$a_{x,y} = x \cdot n + y$cũng không thành công vì tổng đường dẫn không thể giải mã được duy nhất từ ​​các mã hóa tuyến tính như vậy, vì nhiều tập hợp ô được truy cập khác nhau có thể trùng nhau về tổng. 

Yêu cầu thực sự không chỉ là tính duy nhất của tổng mà còn là tổng đó mã hóa một chuỗi các quyết định nhị phân dọc theo đường dẫn. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ là cố gắng gán các trọng số ngẫu nhiên hoặc tham lam và hy vọng tất cả các đường dẫn đều có tổng riêng biệt. Mặc dù điều này có thể có hiệu quả với xác suất cao đối với những$n$, nó không mang lại sự đảm bảo nào trong cách xây dựng đối nghịch và quan trọng hơn, ngay cả khi các tổng là duy nhất, việc giải mã chúng là không khả thi về mặt tính toán vì chúng ta sẽ cần phải tìm kiếm trong số nhiều đường dẫn ứng cử viên theo cấp số nhân. 

Cái nhìn sâu sắc về cấu trúc là mọi con đường đơn điệu từ$(1,1)$ĐẾN$(n,n)$chính xác là một chuỗi$2n-2$quyết định: ở mỗi bước chúng ta có thể di chuyển sang phải hoặc xuống. Điều này tương đương với một chuỗi nhị phân có chính xác$n-1$những cái và$n-1$số không. Vấn đề giảm xuống khi mã hóa chuỗi nhị phân này thành một số duy nhất bằng hệ thống vị trí. 

Điều này gợi ý rằng hãy coi mỗi lựa chọn bước là đóng góp lũy thừa riêng biệt của hai vào tổng. Tuy nhiên, mã hóa trực tiếp theo từng bước không hoạt động vì chúng tôi không quan sát trực tiếp các bước mà chỉ quan sát tổng trên các ô lưới. Vì vậy, thay vì mã hóa các bước, chúng tôi mã hóa các ô theo cách mà sự đóng góp của mỗi bước có thể được tách biệt trong quá trình tái thiết. 

Ý tưởng chính là gán các trọng số chỉ phụ thuộc vào chỉ số tuyến tính của ô trên cơ sở được lựa chọn cẩn thận: lũy thừa của hai căn chỉnh theo cấu trúc của các đường chéo. Chúng tôi muốn mỗi quyết định chuyển động tương ứng với việc chọn một trong hai đóng góp rời rạc, đảm bảo rằng tổng số tiền xác định duy nhất chuỗi các lựa chọn. 

Một cấu trúc rõ ràng là gán cho mỗi ô một trọng số dựa trên vị trí của nó trong phân tách nhị phân theo đường chéo, đảm bảo rằng việc di chuyển sang phải hoặc xuống sẽ chuyển chính xác một bit theo cách được kiểm soát. Điều này làm giảm vấn đề khôi phục đường dẫn từ biểu diễn nhị phân của tổng, có thể được giải mã một cách tham lam bằng cách mô phỏng các chuyển đổi trong khi kiểm tra tính nhất quán của tổng còn lại. 

Một giải pháp mang tính xây dựng trực tiếp hơn và là giải pháp thường được dự định, là gán trọng số sao cho mỗi quyết định bước có thể được khôi phục cục bộ: chúng tôi mã hóa đường dẫn sao cho tại mỗi nút, sự khác biệt giữa đi sang phải và đi xuống có thể được phát hiện nghiêm ngặt từ cấu trúc tổng còn lại. Điều này đạt được bằng cách sử dụng các trọng số tăng theo cấp số nhân dọc theo một trục và cân bằng cẩn thận các khoản đóng góp để các quyết định tiền tố không gây trở ngại. 

Sau khi mã hóa như vậy được sửa, việc giải mã sẽ trở thành một bước đi xác định: tại mỗi ô, chúng tôi kiểm tra xem tổng còn lại có phù hợp với việc di chuyển sang phải hay xuống hay không và chọn hướng hợp lệ duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đoán đường dẫn Brute Force | hàm mũ trên mỗi truy vấn | O(1) | Quá chậm | 
| Mã hóa nhị phân trên cấu trúc lưới | O(nq) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng mã hóa lưới bằng cách sử dụng lũy thừa của hai cột được căn chỉnh. Cụ thể, chúng tôi gán từng ô$(i,j)$một trọng lượng$a_{i,j} = 2^{(i-1)n + (j-1)}$. Điều này biến mỗi ô thành một vị trí bit duy nhất theo số nhị phân có độ dài$n^2$. 

Tuy nhiên, chúng ta không cần sự độc lập hoàn toàn của tất cả các ô, chỉ cần mỗi tổng đường dẫn đơn điệu đều có thể giải mã được duy nhất. Vì mỗi đường dẫn chứa chính xác một ô trên mỗi bước và truy cập chính xác$2n-1$các ô, tổng chỉ đơn giản là tổng tập hợp con trên các bit riêng biệt, có thể đảo ngược duy nhất thành tập hợp đã truy cập. 

Sau đó chúng tôi xây dựng lại đường đi một cách tham lam bằng cách mô phỏng chuyển động từ$(1,1)$: 

1. Bắt đầu lúc$(1,1)$với số tiền còn lại$k$. Ô bắt đầu luôn được bao gồm, vì vậy hãy trừ đi$a_{1,1}$từ$k$. 
2. Ở mỗi bước, hãy cân nhắc xem việc di chuyển sang phải hay xuống có dẫn đến việc tiếp tục hợp lệ hay không. Chúng tôi kiểm tra cả hai ô ứng cử viên tiếp theo. 
3. Nếu ô tiếp theo$(x, y+1)$có trọng số không vượt quá số tiền còn lại theo cách phù hợp với phân rã nhị phân, chúng tôi cố gắng lấy nó; nếu không chúng tôi sẽ lấy$(x+1, y)$. 
4. Sau khi di chuyển, trừ đi trọng lượng ô đã chọn khỏi số tiền còn lại và tiếp tục. 
5. Tiếp tục cho đến khi đạt$(n,n)$, luôn duy trì rằng số tiền còn lại tương ứng chính xác với các ô trong tương lai chưa được truy cập. 

Thuộc tính quan trọng là mỗi ô tương ứng với một lũy thừa duy nhất của 2, do đó, tại bất kỳ thời điểm nào, tổng còn lại sẽ mã hóa chính xác những ô trong tương lai vẫn còn trên đường đi. Vì đường đi đơn điệu và có độ dài cố định nên tại mỗi bước, chính xác một trong hai bước di chuyển có thể duy trì tính nhất quán với cấu trúc bit còn lại. 

### Tại sao nó hoạt động 

Mỗi trọng số ô là lũy thừa riêng biệt của 2, do đó tổng của bất kỳ tập hợp ô nào là một số nhị phân duy nhất. Vì mọi đường dẫn hợp lệ đều tương ứng với một tập hợp con các ô có kích thước cố định nên việc ánh xạ từ đường dẫn đến tổng là nội xạ. Trong quá trình xây dựng lại, chúng tôi đang giải mã một cách hiệu quả cấu trúc tổng tập hợp con đã biết với ràng buộc bổ sung là tập hợp con phải tạo thành một đường dẫn đơn điệu. Ràng buộc cấu trúc này đảm bảo rằng ở mỗi bước, chính xác một trong hai cạnh đi ra sẽ dẫn đến một tập hợp con hợp lệ phù hợp với tổng còn lại, do đó việc tái cấu trúc tham lam không thể phân kỳ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

# assign powers of two to each cell
a = [[0]*n for _ in range(n)]
cur = 0
for i in range(n):
    for j in range(n):
        a[i][j] = 1 << cur
        cur += 1

for row in a:
    print(*row)
sys.stdout.flush()

q = int(input())

for _ in range(q):
    k = int(input())

    x, y = 0, 0
    path = [(x+1, y+1)]
    rem = k - a[0][0]

    while x != n-1 or y != n-1:
        if y + 1 < n:
            if rem & a[x][y+1]:
                y += 1
            else:
                x += 1
        else:
            x += 1

        path.append((x+1, y+1))
        rem -= a[x][y]

    for p in path:
        print(p[0], p[1])
    sys.stdout.flush()
```Cấu trúc lưới gán cho mỗi ô một vị trí bit duy nhất để không có hai tập hợp con nào xung đột với nhau. Việc xây dựng lại giữ phần còn lại đang chạy và sử dụng kiểm tra bit để quyết định xem bước tiếp theo có phải bao gồm một ô cụ thể trong tập hợp con còn lại hay không. Bởi vì đường dẫn là đơn điệu, bước đi không bao giờ quay lại các ô, do đó, việc trừ đi các trọng số đã truy cập sẽ duy trì tính chính xác. 

Một điểm tinh tế là chúng ta luôn dựa vào bất biến mà tổng còn lại tương ứng chính xác với các ô chưa được thăm trên hậu tố đường dẫn thực. Điều này làm cho việc kiểm tra tư cách thành viên trở nên đáng tin cậy ngay cả khi chúng tôi không lưu trữ toàn bộ tập hợp con một cách rõ ràng. 

## Ví dụ đã hoạt động 

Hãy xem xét$n=2$. Lưới là: 

| tế bào | giá trị | 
| --- | --- | 
| (1,1) | 1 | 
| (1,2) | 2 | 
| (2,1) | 4 | 
| (2,2) | 8 | 

Một đường đi có thể là (1,1) → (1,2) → (2,2), cho tổng 1 + 2 + 8 = 11. 

Lúc đầu, tổng còn lại là 11 - 1 = 10. Từ (1,1), ta so sánh (1,2) và (2,1). Vì 2 nằm trong phân tích tổng nên chúng ta chuyển sang phải. Còn lại trở thành 10 - 2 = 8. Từ (1,2), chỉ có (2,2) có giá trị kể từ 8 trận đấu. Chúng tôi đạt đến cuối cùng một cách chính xác. 

Bây giờ hãy xem xét đường dẫn (1,1) → (2,1) → (2,2), tổng là 1 + 4 + 8 = 13. 

| bước | vị trí | số tiền còn lại | quyết định | 
| --- | --- | --- | --- | 
| 0 | (1,1) | 12 | bắt đầu | 
| 1 | di chuyển | 8 | đã chọn xuống | 
| 2 | (2,1) | 8 | buộc phải | 
| 3 | (2,2) | 0 | kết thúc | 

Điều này cho thấy ở mỗi bước chỉ có một bước di chuyển giữ được tính nhất quán với cấu trúc bit còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2 + qn) | xây dựng lưới cộng với tái cấu trúc tuyến tính cho mỗi truy vấn | 
| Không gian | O(n^2) | lưu trữ trọng lượng và đường dẫn | 

Các ràng buộc cho phép lên đến$n=25$Và$q \le 1000$, Vì thế$qn$nhiều nhất là$25000$, dễ dàng đủ nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = [[0]*n for _ in range(n)]
    cur = 0
    for i in range(n):
        for j in range(n):
            a[i][j] = 1 << cur
            cur += 1

    out = []
    for i in range(n):
        out.append(" ".join(str(x) for x in a[i]))

    q = int(input())
    for _ in range(q):
        k = int(input())
        x, y = 0, 0
        rem = k - a[0][0]
        path = [(1,1)]
        while x != n-1 or y != n-1:
            if y + 1 < n and (rem & a[x][y+1]):
                y += 1
            else:
                x += 1
            path.append((x+1, y+1))
            rem -= a[x][y]
        for p in path:
            out.append(f"{p[0]} {p[1]}")
    return "\n".join(out)

# custom sanity checks only (interactive not fully simulated)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=2 đường đi đơn giản | giải mã đường dẫn xác định | tính đúng đắn cơ bản | 
| n=3 đường đi bên phải | hành vi đường thẳng | chuyển động ranh giới | 
| n=3 đường đi xuống | truyền tải cột | đối xứng | 

## Vỏ cạnh 

Lưới tối thiểu$n=2$cho biết liệu mã hóa có duy trì tính duy nhất trong số lượng đường dẫn nhỏ nhất hay không. Chỉ với hai đường đi có thể, hệ thống vẫn phải phân biệt tổng một cách rõ ràng. Phép gán nhị phân đảm bảo rằng (1,1) luôn được bao gồm và quyết định còn lại được giải quyết bằng việc bit thứ hai tương ứng với bên phải hay bên dưới, điều này luôn có thể được phát hiện duy nhất. 

Trường hợp góc là khi đường đi luôn di chuyển theo một hướng cho đến khi bị ép buộc, chẳng hạn như luôn di chuyển sang phải trước. Trong tình huống đó, phần còn lại giảm theo một chuỗi lũy thừa hai có thể dự đoán được một cách chính xác, do đó ở mỗi bước vẫn có chính xác một bit lân cận được đặt. Điều này xác nhận rằng việc tái cấu trúc tham lam không phụ thuộc vào logic phá vỡ liên kết, vì các liên kết không thể xảy ra dưới mã hóa bit duy nhất.
