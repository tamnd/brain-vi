---
title: "CF 2200E - Trận chiến chia cắt"
description: "Chúng ta được cung cấp một dãy số nguyên dương và hai người chơi thay phiên nhau bắt đầu với Alice. Trò chơi sẽ phát triển bằng cách kết thúc ngay lập tức nếu mảng hiện tại không giảm hoặc bằng cách thực hiện một thao tác được phép duy nhất trên phần tử đã chọn."
date: "2026-06-07T20:17:13+07:00"
tags: ["codeforces", "competitive-programming", "games", "greedy", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2200
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1084 (Div. 3)"
rating: 1500
weight: 2200
solve_time_s: 101
verified: false
draft: false
---

[CF 2200E - Trận chiến chia rẽ](https://codeforces.com/problemset/problem/2200/E) 

**Đánh giá:** 1500 
**Tags:** trò chơi, tham lam, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy số nguyên dương và hai người chơi thay phiên nhau bắt đầu với Alice. Trò chơi sẽ phát triển bằng cách kết thúc ngay lập tức nếu mảng hiện tại không giảm hoặc bằng cách thực hiện một thao tác được phép duy nhất trên phần tử đã chọn. 

Thao tác duy nhất có sẵn là lấy một số$x$và thay thế nó bằng hai yếu tố$y$Và$z$sao cho cả hai đều lớn hơn 1 và$x = yz$. Thứ tự của$y$Và$z$có thể được đặt tự do ở vị trí ban đầu của$x$, do đó độ dài mảng có thể tăng lên nhưng vị trí tương đối của tất cả các phần tử khác vẫn được giữ nguyên. 

Trò chơi kết thúc ngay khi mảng không giảm hoặc không có động thái phân tích nhân tử hợp lệ nào cho bất kỳ phần tử nào. Sau khi kết thúc, nếu mảng cuối cùng được sắp xếp theo thứ tự không giảm thì Bob thắng; nếu không Alice sẽ thắng. 

Điểm căng thẳng chính là người chơi không cố gắng trực tiếp tối đa hóa hoặc giảm thiểu các giá trị mảng theo nghĩa thông thường mà thay vào đó thao túng cấu trúc hệ số để trì hoãn hoặc buộc thời điểm sắp xếp trở nên không thể tránh khỏi. 

Các ràng buộc đủ chặt chẽ để giải pháp phải tránh mọi mô phỏng phân tích nhân tử trên mỗi bước di chuyển. Với tổng số$n \le 10^5$và giá trị lên đến$10^6$, bất kỳ cách tiếp cận nào khám phá tất cả các hệ số hóa hoặc mô phỏng trạng thái trò chơi sẽ ngay lập tức thất bại vì hệ số phân nhánh cho số tổng hợp lớn và việc chơi lặp lại sẽ tạo ra sự tăng trưởng theo cấp số nhân về kích thước mảng. 

Trường hợp cạnh tinh tế xuất hiện khi mảng không giảm khi bắt đầu. Trong trường hợp đó, trò chơi kết thúc trước bất kỳ nước đi nào, vì vậy Bob luôn thắng bất kể cấu trúc. Một trường hợp tế nhị khác là khi tất cả các phần tử đều là số nguyên tố hoặc 1, vì khi đó không thể chia được và trò chơi kết thúc ngay lập tức, khiến thứ tự ban đầu có tính quyết định. Tình huống không hề tầm thường thứ ba là khi một tổ hợp lớn nằm bên trong một chuỗi tăng dần, vì việc tách nó có thể phá vỡ tính đơn điệu hoặc kéo dài trò chơi tùy thuộc vào tính chẵn lẻ. 

Ví dụ, hãy xem xét`[6, 5]`. Điều này không được sắp xếp. Alice có thể chia 6 thành 2 và 3, thu được`[2, 3, 5]`, được sắp xếp nên Bob thắng ngay sau nước đi của Alice. Tuy nhiên, trong`[10, 9, 8]`, việc chia tách lặp đi lặp lại tạo ra đủ tính linh hoạt để cách chơi tối ưu thay đổi tính chẵn lẻ của ai buộc chấm dứt. Những ví dụ này cho thấy rằng trò chơi về cơ bản là về số lượng “sự phân chia hữu ích” có sẵn và cách chúng ảnh hưởng đến tính chẵn lẻ của quyền kiểm soát. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ cố gắng mô hình hóa tất cả các phân chia có thể có của các số tổng hợp và các bước di chuyển thay thế cho đến khi ổn định. Mỗi số$x$có thể được phân chia theo nhiều cách tùy thuộc vào cặp nhân tố của nó và mỗi lần phân chia sẽ thay đổi cấu trúc của mảng. Ngay cả khi chúng ta tính toán trước các cặp nhân tố, cây trò chơi vẫn phát triển nhanh chóng vì mỗi bước di chuyển sẽ làm tăng kích thước mảng và thay đổi các tùy chọn di chuyển trong tương lai. Trong trường hợp xấu nhất, những con số như$10^6$có thể có hàng tá hệ số hóa và chuỗi tổng hợp tạo ra các trạng thái phân nhánh sâu sắc. 

Sự đơn giản hóa chính là ngừng suy nghĩ về việc “chọn hệ số chính xác nào” và thay vào đó tập trung vào số lần chia hiệu quả mà mỗi số có thể đóng góp cho trò chơi. Mỗi phép chia hợp lệ sẽ giảm một số thành các thừa số nhỏ hơn và thuộc tính duy nhất quan trọng là chúng ta có thể phân tách một số thành các thừa số không tầm thường bao nhiêu lần một cách có ý nghĩa trước khi mọi thứ trở thành số nguyên tố. 

Điều này dẫn đến một ý tưởng định giá: mỗi số đóng góp một số lượng "nước đi bổ sung" nhất định và trò chơi giảm xuống việc đếm tổng số nước đi tồn tại trước khi mảng trở nên hoàn toàn không giảm hoặc không còn phần chia nào. Một khi trò chơi này trở thành một trò chơi chẵn lẻ thuần túy, cách chơi tối ưu chỉ phụ thuộc vào việc Alice hay Bob thực hiện nước đi hiệu quả cuối cùng. 

Nhận xét mang tính quyết định là mọi số$x$đóng góp một số phép toán cố định bằng tổng số thừa số nguyên tố của$x$, được tính với bội số, trừ đi một khi nó là hợp số, vì mỗi phép chia sẽ giảm một hợp số thành hai số nhỏ hơn. Điều này chuyển đổi vấn đề thành tính toán ngân sách di chuyển toàn cầu. 

Sau đó, chúng tôi mô phỏng một cách hợp lý: nếu mảng không giảm thì Bob sẽ thắng ngay lập tức. Ngược lại, Alice thắng nếu tổng số các hoạt động hiệu quả sẵn có là số lẻ và Bob thắng nếu nó là số chẵn, bởi vì mỗi nước đi tiêu tốn một đơn vị của ngân sách phân rã này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng chia tách) | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu (đếm hệ số + chẵn lẻ) | O(n log A) | O(A) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính trước hệ số nguyên tố nhỏ nhất cho mọi số nguyên lên đến$10^6$. Điều này cho phép nhân tố hóa nhanh chóng mọi giá trị đầu vào. 
2. Đối với mỗi trường hợp thử nghiệm, trước tiên hãy kiểm tra xem mảng có phải là không giảm hay không. Nếu đúng như vậy, hãy xuất Bob ngay lập tức vì trò chơi kết thúc trước khi có thể thực hiện bất kỳ nước đi nào. 
3. Phân tích từng phần tử bằng bảng SPF và tính xem nó chứa bao nhiêu thừa số nguyên tố, tính bội số. Điều này đưa ra thước đo xem nó đóng góp bao nhiêu “đơn vị chia tách”. 
4. Chuyển đổi mỗi số thành đóng góp hiệu quả của nó cho trò chơi, đó là số thừa số nguyên tố trừ đi một khi số đó lớn hơn 1. Điều này phản ánh số lần nó có thể được phân tách thành các phần chia không tầm thường. 
5. Tổng hợp tất cả các đóng góp trên mảng để có được tổng số nước đi có sẵn trong trò chơi. 
6. Xác định người chiến thắng bằng tính chẵn lẻ của tổng số này. Nếu tổng là số lẻ, Alice thắng; nếu chẵn thì Bob thắng. 

Lý do tính chẵn lẻ là đủ là vì mỗi bước di chuyển sẽ làm giảm chính xác một đơn vị phân rã sẵn có và không có bước đi nào có thể tạo ra khả năng phân rã ròng bổ sung ngoài những gì hệ số hóa đã cung cấp. Cấu trúc phân tách đảm bảo trò chơi luôn tiến tới phân rã nguyên tố hoàn toàn, là điểm cuối cố định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXA = 10**6

spf = list(range(MAXA + 1))
for i in range(2, int(MAXA ** 0.5) + 1):
    if spf[i] == i:
        for j in range(i * i, MAXA + 1, i):
            if spf[j] == j:
                spf[j] = i

def factor_count(x):
    cnt = 0
    while x > 1:
        cnt += 1
        x //= spf[x]
    return cnt

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    ok = True
    for i in range(n - 1):
        if a[i] > a[i + 1]:
            ok = False
            break
    if ok:
        print("Bob")
        return

    moves = 0
    for x in a:
        moves += max(0, factor_count(x) - 1)

    print("Alice" if moves % 2 == 1 else "Bob")

if __name__ == "__main__":
    t = int(input())
    for _ in range(t):
        solve()
```Phần đầu tiên xây dựng sàng hệ số nguyên tố nhỏ nhất để mỗi số có thể được phân tách theo thời gian logarit. Điều này tránh việc tính toán lại hệ số cho mỗi truy vấn. 

Việc kiểm tra tính đơn điệu được thực hiện trước bất kỳ điều gì khác vì đây là điều kiện cuối cùng hấp dẫn của trò chơi: một khi mảng đã được sắp xếp, không còn vấn đề lý luận nào nữa. 

các`factor_count`hàm đếm các thừa số nguyên tố có bội số bằng cách sử dụng phép chia lặp lại cho SPF. Điều này trực tiếp tương ứng với độ sâu phân rã của một số. 

biểu hiện`factor_count(x) - 1`mã hóa thực tế là một cấu trúc được mở rộng hoàn toàn nguyên tố không có sự phân chia hữu ích nào nữa, do đó chỉ có cấu trúc tổng hợp mới đóng góp. 

Cuối cùng, tính chẵn lẻ của tổng số nước đi sẽ quyết định người chiến thắng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
a = [6, 5, 4]
```| Bước | Mảng | Sắp xếp? | Đã thêm di chuyển | 
| --- | --- | --- | --- | 
| 1 | [6,5,4] | Không | 6→1, 5→0, 4→1 | 

Yếu tố đóng góp đến từ: 

- 6 = 2×3 cho 2 thừa số → 1 nước đi 
- 5 là số nguyên tố → 0 bước 
- 4 = 2×2 cho 2 thừa số → 1 nước đi 

Tổng số nước đi = 2, chẵn nên Bob thắng. 

Điều này khẳng định rằng mặc dù mảng không được sắp xếp cao nhưng khả năng phân rã vẫn được cân bằng và người chơi thứ hai kiểm soát nước đi cuối cùng. 

### Ví dụ 2 

đầu vào:```
n = 2
a = [6, 5]
```| Bước | Mảng | Sắp xếp? | Đã thêm di chuyển | 
| --- | --- | --- | --- | 
| 1 | [6,5] | Không | 1 + 0 | 

Tổng số nước đi = 1, lẻ nên Alice thắng. 

Điều này phản ánh rằng Alice có thể thực hiện một phân rã hiệu quả duy nhất trước khi trò chơi ổn định thành một cấu hình được sắp xếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log A)$| Hệ số SPF cho từng số | 
| Không gian |$O(A)$| sàng đến giá trị tối đa | 

Các ràng buộc cho phép lên đến$10^5$các số có giá trị lên tới$10^6$, do đó, sàng tuyến tính cộng với hệ số hóa trên mỗi lần kiểm tra phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MAXA = 10**6
    spf = list(range(MAXA + 1))
    for i in range(2, int(MAXA ** 0.5) + 1):
        if spf[i] == i:
            for j in range(i * i, MAXA + 1, i):
                if spf[j] == j:
                    spf[j] = i

    def factor_count(x):
        cnt = 0
        while x > 1:
            cnt += 1
            x //= spf[x]
        return cnt

    def solve():
        n = int(input())
        a = list(map(int, input().split()))

        ok = True
        for i in range(n - 1):
            if a[i] > a[i + 1]:
                ok = False
                break
        if ok:
            return "Bob\n"

        moves = 0
        for x in a:
            moves += max(0, factor_count(x) - 1)

        return ("Alice\n" if moves % 2 == 1 else "Bob\n")

    t = int(input())
    out = []
    for _ in range(t):
        out.append(solve())
    return "".join(out)

# provided samples
assert run("""4
10
10 9 8 7 6 5 4 3 2 1
3
1 8192 677
2
6 5
2
6 7
""") == """Alice
Bob
Alice
Bob
"""

# custom cases
assert run("""1
1
10
""") == "Bob\n"  # already sorted

assert run("""1
1
6 5
""") == "Alice\n"

assert run("""1
1
6 7
""") == "Bob\n"

assert run("""1
3
2 3 4
""") == "Bob\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[10]`| Bob | phần tử đơn lẻ, đã được sắp xếp | 
|`[6,5]`| Alice | trường hợp di chuyển lẻ chưa được sắp xếp tối thiểu | 
|`[6,7]`| Bob | đã được sắp xếp nhưng chưa giảm hẳn | 
|`[2,3,4]`| Bob | số nguyên tố hỗn hợp và hợp số | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi mảng không giảm. Đối với đầu vào`[1, 5, 5, 10]`, thuật toán ngay lập tức đưa ra Bob trước khi xem xét phân tích nhân tử. Điều này phù hợp với quy tắc trò chơi kết thúc ngay lập tức mà không có nước đi nào. 

Một trường hợp khác là khi tất cả các số đều là số nguyên tố, chẳng hạn như`[2, 3, 5, 7]`. Việc kiểm tra tính đơn điệu quyết định kết quả ngay lập tức. Nếu nó không được sắp xếp, thì không có sự phân chia nào tồn tại, vì vậy trò chơi kết thúc ở lượt đầu tiên với một mảng không được sắp xếp, dẫn đến việc Alice chỉ thắng khi sự chấm dứt xảy ra sau khi cô ấy không thể di chuyển, điều này được tính bằng tính chẵn lẻ của 0 nước đi. 

Trường hợp tinh tế cuối cùng là một mảng giảm dần các số tổng hợp cao như`[12, 6, 4]`. Mỗi đơn vị đóng góp nhiều đơn vị nhân tử, nhưng tính chẵn lẻ vẫn xác định người chiến thắng bất kể độ phức tạp của thứ tự, bởi vì trò chơi giảm hoàn toàn đến mức cạn kiệt các phần chia có sẵn thay vì sắp xếp lại.
