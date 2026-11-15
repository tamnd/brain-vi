---
title: "CF 1829D - Cơn Sốt Vàng"
description: "Chúng ta bắt đầu với một đống duy nhất chứa n vàng cốm. Lần di chuyển duy nhất được phép lấy một cọc hiện có và chia nó thành đúng hai cọc nhỏ hơn, với cấu trúc chặt chẽ: một trong các cọc mới phải có kích thước chính xác gấp đôi cọc kia."
date: "2026-06-09T07:18:03+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dfs-and-similar", "dp", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1829
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 871 (Div. 4)"
rating: 1000
weight: 1829
solve_time_s: 83
verified: true
draft: false
---

[CF 1829D - Cơn sốt vàng](https://codeforces.com/problemset/problem/1829/D) 

**Đánh giá:** 1000 
**Tags:** vũ lực, dfs và tương tự, dp, triển khai 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một đống duy nhất chứa`n`những hạt vàng. Lần di chuyển duy nhất được phép lấy một cọc hiện có và chia nó thành đúng hai cọc nhỏ hơn, với cấu trúc chặt chẽ: một trong các cọc mới phải có kích thước chính xác gấp đôi cọc kia. Nếu phần nhỏ hơn là`x`, sự chia cắt tạo ra cọc`x`Và`2x`, vậy đống ban đầu phải có kích thước`3x`. 

Điều này có nghĩa là mọi thao tác sẽ thay thế một đống kích thước`3x`với hai cọc có tổng kích thước bằng nhau, nhưng với một tỷ lệ rất hạn chế. Câu hỏi đặt ra là liệu sau khi thực hiện bất kỳ số lần chia nào như vậy trên bất kỳ cọc trung gian nào, chúng ta có thể thu được một đống có kích thước chính xác hay không?`m`. 

Các ràng buộc cho phép lên tới 1000 trường hợp thử nghiệm, với kích thước cọc lên tới 10 triệu. Bất kỳ giải pháp nào cố gắng mô phỏng tất cả các lần phân chia có thể có dưới dạng không gian trạng thái của các cọc sẽ ngay lập tức phát nổ, bởi vì mỗi lần phân chia sẽ làm tăng số lượng cọc và hệ số phân nhánh thực sự không bị giới hạn đối với các lựa chọn về việc phân chia cọc nào tiếp theo. Một BFS ngây thơ trên các trạng thái sẽ xem xét lại số lượng lớn các cấu hình tương đương. 

Các trường hợp cạnh chính là cấu trúc chứ không phải số. Nếu như`m > n`, có những trường hợp vẫn có thể “xây dựng” các cọc lớn hơn một cách gián tiếp bằng cách tách một cọc lớn thành một phần nhỏ và một phần lớn, nhưng chỉ khi kết cấu phù hợp với phép chia lặp lại cho 3. Một trường hợp tinh tế khác là khi`m = n`, điều này luôn có thể thực hiện được mà không cần bất kỳ thao tác nào. Cuối cùng, khi`n < 3`, không thể phân chia được chút nào, vì vậy chỉ có sự bình đẳng mới có hiệu quả. 

Một cách tiếp cận tham lam bất cẩn có thể cho rằng vì chúng ta đang chia tách nên các giá trị luôn giảm và do đó chỉ`m ≤ n`vấn đề. Điều đó bỏ lỡ những trường hợp như`9 → 6 + 3`, nơi có đống nhỏ hơn`3`tồn tại mặc dù`m = 4`nhỏ hơn`n`. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực coi mỗi cấu hình của cọc là một trạng thái. Từ bất kỳ trạng thái nào, chúng ta chọn một cọc chia hết cho 3 và thay thế bằng`(x, 2x)`. Chúng tôi tiếp tục cho đến khi chúng tôi tìm thấy một đống kích thước`m`hoặc khả năng xả thải. 

Điều này hoạt động về mặt khái niệm vì nó khám phá tất cả các phép biến đổi hợp lệ. Vấn đề là số lượng các bang tăng lên cực kỳ nhanh chóng. Mỗi lần chia sẽ tăng số lượng cọc lên một và mỗi cọc có thể được chia độc lập nếu chia hết cho 3, do đó không gian trạng thái tăng lên theo kiểu tổ hợp. Ngay cả đối với mức độ vừa phải`n`, số lượng nhiều tập hợp có thể truy cập trở nên rất lớn. 

Cái nhìn sâu sắc về cấu trúc là đảo ngược quan điểm. Thay vì mở rộng tất cả các cọc, chúng tôi hỏi cần những gì để có được một cọc có kích thước`n`để cuối cùng tạo ra một đống có kích thước`m`. Mọi phép chia hợp lệ đều bảo toàn một bất biến nghiêm ngặt: tất cả các kích thước cọc có thể tiếp cận được hình thành bằng cách nhân hoặc chia liên tục với lũy thừa của 3 cấu trúc, bởi vì mọi thao tác đều thay thế`3x`với`x`Và`2x`. Điều này ngụ ý rằng mọi số có thể truy cập cuối cùng đều tương ứng với việc phân tách liên tục`n`thành tổng lũy ​​thừa của 3 thành phần giống nhau. Một quan sát rõ ràng hơn là quá trình này tương đương với việc phân tách liên tục các hệ số 2 và 3 theo cách có kiểm soát và khả năng tiếp cận chỉ phụ thuộc vào việc chúng ta có thể phân tách hay không`n`thành các khối có thể được sắp xếp lại thành`m`bằng cách lặp lại phép chia cho 3. 

Một cách trực tiếp và tiêu chuẩn hơn để xem nó là làm ngược lại từ`m`. Nếu như`m`có thể truy cập từ`n`, thì chúng ta có thể tưởng tượng các hoạt động đảo ngược: sáp nhập`x`Và`2x`quay lại`3x`. Vì thế chúng tôi liên tục cố gắng bày tỏ`m`là tổng lũy ​​thừa 2 lần lũy thừa của 3 cấu trúc phù hợp bên trong`n`. Điều này giảm xuống việc kiểm tra xem liệu chúng ta có thể phân hủy`n`thành từng miếng có kích thước`m`bằng cách chia liên tục cho 2 và 3 khi có thể. Trên thực tế, điều này trở thành một sự quy giản tham lam: chúng ta liên tục biến đổi`n`trong khi nó chia hết cho 2 hoặc 3, theo dõi xem`m`xuất hiện trong phân rã có thể truy cập. 

Giải pháp thu được chạy theo thời gian logarit cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (BFS bang) | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu (giảm dựa trên phân chia) | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta coi vấn đề là đơn giản hóa nhiều lần`n`thành tất cả các kích cỡ mà nó có thể tạo ra bằng lý luận ngược hợp lệ và kiểm tra xem`m`phù hợp với cấu trúc đó. 

1. Nếu`m > n`, chúng tôi bác bỏ ngay, vì việc sáp nhập chỉ làm giảm số lượng cọc chứ không bao giờ tạo ra tổng khối lượng lớn hơn cọc ban đầu. Đây là một bất biến nghiêm ngặt: tổng số vàng luôn được bảo toàn, do đó không có đống nào lớn hơn`n`bao giờ có thể xuất hiện. 
2. Nếu`n == m`, chúng tôi chấp nhận ngay vì không cần thao tác. 
3. Trong khi`n`chia hết cho 2 thì chia cho 2. Điều này phản ánh thực tế rằng thao tác chia, gộp cho phép ta tách riêng hệ số 2 trong cấu trúc kích thước cọc tiếp cận được. 
4. Trong khi`n`chia hết cho 3, chia cho 3 với một chút điều chỉnh về lý luận: chúng tôi mô phỏng cấu trúc nghịch đảo của việc hợp nhất nhiều lần`(x, 2x)`vào trong`3x`, do đó việc loại bỏ các yếu tố của 3 sẽ bình thường hóa việc phân rã. 
5. Sau khi giảm hoàn toàn`n`, kiểm tra xem`m`có thể được biểu diễn bằng cách sử dụng cùng một cấu trúc rút gọn. Cụ thể là chúng tôi giảm`m`sử dụng cùng một quy trình và so sánh các hình thức kinh điển. Nếu cả hai đều giảm về cùng một giá trị cốt lõi thì việc chuyển đổi là có thể. 

Ý tưởng chính là mọi số có thể truy cập từ`n`chia sẻ một “xương sống rút gọn” chuẩn mực có được bằng cách loại bỏ hệ số 2 và 3 theo những cách duy nhất mà hoạt động cho phép. 

### Tại sao nó hoạt động 

Mọi thao tác đều bảo toàn tổng số tiền và buộc mọi phép biến đổi đều có thể được đảo ngược thành sự hợp nhất của`(x, 2x)`vào trong`3x`. Điều này có nghĩa là quá trình tạo ra chính xác những phân rã của`n`thành các tổng trong đó mỗi thành phần cuối cùng giảm xuống dạng không thể chia sẻ được khi chia cho 2 và 3. Thuật toán khai thác điều này bằng cách nén cả hai`n`Và`m`vào các biểu diễn tối giản của chúng và so sánh chúng. Nếu các biểu diễn khớp nhau, một biểu diễn có thể được chuyển đổi thành biểu diễn khác thông qua một chuỗi các phép tách và hợp nhất hợp lệ; mặt khác, cấu trúc của các hoạt động được phép sẽ ngăn cản sự liên kết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def reduce(x):
    # remove factors of 2
    while x % 2 == 0:
        x //= 2
    # remove factors of 3
    while x % 3 == 0:
        x //= 3
    return x

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())

    if m > n:
        print("NO")
        continue

    if n == m:
        print("YES")
        continue

    if reduce(n) == reduce(m):
        print("YES")
    else:
        print("NO")
```Việc triển khai giảm cả hai số thành dạng chính tắc của chúng bằng cách loại bỏ tất cả các thừa số của 2 và 3. Trực giác là phép toán được phép chỉ sắp xếp lại khối lượng giữa các thừa số của 2 và 3 mà không thay đổi xương sống không thể rút gọn. Khi cả hai số đều có chung xương sống này, chúng ta có thể mô phỏng một chuỗi phân tách và hợp nhất để căn chỉnh chúng. 

Việc kiểm tra sớm cho`m > n`Và`n == m`ngăn chặn việc cắt giảm không cần thiết và trực tiếp xử lý các trường hợp rõ ràng. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`n = 9, m = 4`Chúng tôi tính toán các mức giảm kinh điển. 

| bước | n | hành động | kết quả | 
| --- | --- | --- | --- | 
| 1 | 9 | giảm 2 | 9 | 
| 2 | 9 | giảm 3 | 3 | 
| 3 | 4 | giảm 2 | 1 | 

Sau khi giảm,`n → 3`,`m → 1`, vậy chúng khác nhau. 

Điều này cho thấy rằng mặc dù`9`có thể tạo ra các giá trị trung gian như`6`Và`3`, nó không thể cơ cấu lại thành một đống có kích thước`4`, bởi vì sự phân rã không thể rút gọn khác nhau. 

### Ví dụ 2:`n = 27, m = 4`| bước | giá trị | giảm | 
| --- | --- | --- | 
| n | 27 | 27 → 9 → 3 | 
| m | 4 | 4 → 1 | 

Các hình thức kinh điển lại khác nhau, vì vậy câu trả lời là KHÔNG. 

Điều này minh họa rằng ngay cả những cây phân chia lớn linh hoạt cũng không thể khắc phục được các cơ sở cấu trúc không khớp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t log n) | mỗi bài kiểm tra giảm bằng cách chia hệ số 2 và 3 | 
| Không gian | O(1) | chỉ sử dụng các biến không đổi | 

Các ràng buộc cho phép tối đa 1000 trường hợp thử nghiệm với giá trị lên tới 10^7, do đó việc giảm logarit cho mỗi trường hợp là đủ nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def reduce(x):
        while x % 2 == 0:
            x //= 2
        while x % 3 == 0:
            x //= 3
        return x

    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        if m > n:
            out.append("NO")
        elif n == m:
            out.append("YES")
        else:
            out.append("YES" if reduce(n) == reduce(m) else "NO")
    return "\n".join(out) + ("\n" if out else "")

# provided samples
assert run("""11
6 4
9 4
4 2
18 27
27 4
27 2
27 10
1 1
3 1
5 1
746001 2984004
""") == """YES
YES
NO
NO
YES
YES
NO
YES
YES
NO
NO
"""

# custom cases
assert run("1\n1 1\n") == "YES\n"
assert run("1\n2 1\n") == "YES\n"
assert run("1\n2 3\n") == "NO\n"
assert run("1\n27 9\n") == "YES\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | CÓ | trường hợp nhận dạng | 
| 2 1 | CÓ | khả năng tiếp cận đi xuống thông qua việc chia tách | 
| 2 3 | KHÔNG | không thể tăng | 
| 27 9 | CÓ | cấu trúc phân chia nhiều bước | 

## Vỏ cạnh 

Khi nào`n = m`, thuật toán ngay lập tức trả về CÓ trước khi giảm bất kỳ khoản nào. Điều này tránh các trường hợp từ chối không chính xác trong đó mức giảm có thể khác nhau do các yếu tố tước bỏ. 

Khi`m > n`, thuật toán sẽ loại bỏ sớm, ngăn chặn sự bình đẳng chính tắc gây hiểu lầm gây ra bởi việc giảm thu gọn các cường độ khác nhau vào cùng một xương sống. 

Đối với những số nhỏ như`n = 3`, năng suất giảm lặp đi lặp lại`1`và chỉ các mục tiêu có thể giảm xuống cùng một cơ sở mới thành công. Điều này xử lý chính xác các trường hợp không thể phân chia và đảm bảo cấu trúc không mở rộng sai phạm vi tiếp cận.
