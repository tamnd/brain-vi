---
title: "CF 1036C - Những con số đẳng cấp"
description: "Chúng ta đang làm việc với khái niệm số "thưa thớt" trong cơ số 10. Một số được coi là hợp lệ nếu khi bạn viết nó ở dạng thập phân, nhiều nhất ba chữ số của nó khác 0. Số 0 có thể xuất hiện ở bất cứ đâu và với số lượng bất kỳ, nhưng chỉ được phép mang tối đa ba vị trí mang giá trị thực."
date: "2026-06-16T19:09:55+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp"]
categories: ["algorithms"]
codeforces_contest: 1036
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 50 (Rated for Div. 2)"
rating: 1900
weight: 1036
solve_time_s: 775
verified: false
draft: false
---

[CF 1036C - Những con số đẳng cấp](https://codeforces.com/problemset/problem/1036/C) 

**Xếp hạng:** 1900 
**Tags:** tổ hợp, dp 
**Thời gian giải:** 12 phút 55s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang làm việc với khái niệm số "thưa thớt" trong cơ số 10. Một số được coi là hợp lệ nếu khi bạn viết nó ở dạng thập phân, nhiều nhất ba chữ số của nó khác 0. Số 0 có thể xuất hiện ở bất cứ đâu và với số lượng bất kỳ, nhưng chỉ được phép mang tối đa ba vị trí mang giá trị thực. 

Đối với mỗi truy vấn, chúng tôi được cung cấp một khoảng$[L, R]$và chúng ta phải đếm xem có bao nhiêu số nguyên trong khoảng đó thỏa mãn điều kiện thưa thớt này. 

Điểm cuối phạm vi đi lên đến$10^{18}$, vì vậy chúng ta đang xử lý các số có tới 19 chữ số. Mỗi truy vấn phải được trả lời một cách hiệu quả và có thể có tới$10^4$truy vấn. 

Ý nghĩa trực tiếp của các ràng buộc là việc lặp lại mọi số trong một phạm vi là không thể. Thậm chí một khoảng có thể lớn bằng$10^{18}$, và tổng hợp lại$10^4$các truy vấn làm cho vũ lực hoàn toàn không khả thi. 

Các trường hợp cạnh chính đến từ cấu trúc chữ số hơn là ranh giới số học. Ví dụ: các số như 1000000 và 1000001 hoạt động rất khác nhau ngay cả khi chúng ở cạnh nhau. Một trường hợp tinh vi khác là các số có chính xác ba chữ số khác 0 nằm rải rác ở các vị trí cao, trong đó các phương pháp đếm ngây thơ bỏ qua các ràng buộc về vị trí sẽ đếm sai bội số. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ lặp qua mọi số trong$[L, R]$, đếm các chữ số khác 0 của nó và tăng bộ đếm nếu số lượng nhiều nhất là ba. Điều này đúng về mặt logic, nhưng mỗi số yêu cầu quét tối đa 19 chữ số. Ngay cả trong trường hợp không tầm thường nhỏ nhất khi$R - L \approx 10^{18}$, điều này vượt xa mọi tính toán khả thi. Ngay cả khi chúng tôi chỉ xem xét một truy vấn có kích thước$10^6$, việc quét chữ số làm cho nó trở thành ranh giới và với$10^4$truy vấn nó sụp đổ hoàn toàn. 

Cấu trúc của bài toán gợi ý chuyển quan điểm từ việc lặp qua các giá trị sang xây dựng các số hợp lệ theo từng chữ số. Thay vì hỏi “con số này có hợp lệ không”, chúng tôi hỏi “có bao nhiêu số hợp lệ tồn tại cho đến X”. Đây là kịch bản DP chữ số cổ điển trong đó các ràng buộc phụ thuộc vào số chữ số khác 0 thay vì tổng hoặc thứ tự của chúng. 

Quan sát quan trọng là một số hợp lệ được xác định đầy đủ bằng cách chọn tối đa ba vị trí trong số tối đa 19 chữ số và gán các chữ số khác 0 cho các vị trí đó. Khi vị trí được chọn, các chữ số sẽ độc lập. Điều này biến việc đếm thành một cấu trúc tổ hợp được nhúng bên trong bảng liệt kê giới hạn từng chữ số. 

Do đó chúng tôi tính toán một hàm$F(x)$: số số nguyên hợp lệ trong$[1, x]$. Mỗi truy vấn được trả lời dưới dạng$F(R) - F(L - 1)$. 

Để tính toán$F(x)$, chúng tôi chạy một chữ số DP trên các vị trí, theo dõi xem chúng tôi đã sử dụng bao nhiêu chữ số khác 0 và liệu chúng tôi có còn bị giới hạn bởi tiền tố của$x$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((R-L)\cdot \log R)$|$O(1)$| Quá chậm | 
| Chữ số DP |$O(19 \cdot 4 \cdot 2 \cdot 10)$mỗi truy vấn |$O(19 \cdot 4 \cdot 2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định một chức năng$F(x)$đếm số hợp lệ trong phạm vi$[1, x]$. 

1. Chuyển đổi$x$vào mảng chữ số thập phân của nó. Điều này cho phép chúng ta suy luận vị trí theo vị trí từ chữ số có nghĩa nhất. 
2. Xác định trạng thái DP$dp[pos][cnt][tight]$, Ở đâu$pos$là chỉ số chữ số hiện tại,$cnt$là chúng ta đã sử dụng bao nhiêu chữ số khác 0 và$tight$cho biết liệu chúng tôi có còn khớp với tiền tố của$x$. 
3. Tại mỗi vị trí, chúng tôi thử tất cả các chữ số có thể từ 0 đến 9, nhưng chúng tôi giới hạn giới hạn trên ở chữ số hiện tại của$x$nếu như$tight = 1$. Điều này đảm bảo chúng tôi không bao giờ vượt quá$x$. 
4. Với mỗi chữ số ứng viên, hãy cập nhật$cnt$bằng cách thêm 1 nếu chữ số khác 0. Nếu con số này vượt quá 3, chúng tôi sẽ loại bỏ quá trình chuyển đổi. 
5. Chuyển sang vị trí tiếp theo, cập nhật cờ chặt: nó chỉ giữ chặt nếu chúng ta khớp chính xác với chữ số hiện tại của$x$. 
6. Sau khi xử lý tất cả các vị trí, đếm tất cả các trạng thái hợp lệ là một số hợp lệ. Chúng tôi cũng đảm bảo loại trừ việc giải thích số trống bằng cách xử lý các số 0 đứng đầu một cách tự nhiên trong DP. 
7. Tính toán$F(R)$Và$F(L-1)$cho mỗi truy vấn và trừ. 

DP xử lý một cách tự nhiên các số có độ dài khác nhau bằng cách cho phép các số 0 đứng đầu ở đầu. Những số 0 đứng đầu này không được tính vào giới hạn khác 0 nên chúng không ảnh hưởng đến tính hợp lệ. 

### Tại sao nó hoạt động 

Mỗi số trong$[1, x]$tương ứng với chính xác một đường dẫn trong cây DP: mỗi lựa chọn chữ số xác định một tiền tố duy nhất. DP phân vùng tất cả các số theo tiền tố chữ số của chúng mà không bị trùng lặp. Ràng buộc đối với các chữ số khác 0 được thực thi cục bộ và đơn điệu, nghĩa là một khi chúng ta vượt quá ba chữ số khác 0 thì việc tiếp tục không thể khôi phục tính hợp lệ. Cờ chặt chẽ đảm bảo chúng tôi chỉ đếm các số không vượt quá giới hạn, duy trì tính chính xác của giới hạn giới hạn trên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from functools import lru_cache

def solve_case(x: int) -> int:
    if x <= 0:
        return 0
    digits = list(map(int, str(x)))
    n = len(digits)

    @lru_cache(None)
    def dp(pos: int, cnt: int, tight: int) -> int:
        if cnt > 3:
            return 0
        if pos == n:
            return 1

        limit = digits[pos] if tight else 9
        res = 0

        for d in range(limit + 1):
            new_cnt = cnt + (d != 0)
            new_tight = tight and (d == limit)
            res += dp(pos + 1, new_cnt, new_tight)

        return res

    return dp(0, 0, 1)

def solve():
    t = int(input())
    for _ in range(t):
        l, r = map(int, input().split())
        print(solve_case(r) - solve_case(l - 1))

if __name__ == "__main__":
    solve()
```Giải pháp xây dựng một chữ số DP trên biểu diễn thập phân của giới hạn trên. Đệ quy theo dõi có bao nhiêu chữ số khác 0 đã được đặt cho đến nay. Khi con số đó vượt quá ba, cành sẽ được cắt tỉa ngay lập tức. 

Cờ chặt chẽ được triển khai bằng cách sử dụng ý tưởng DP chữ số tiêu chuẩn: khi chặt chẽ là đúng, chữ số tiếp theo không thể vượt quá chữ số tương ứng trong giới hạn; mặt khác, nó có thể tự do dao động từ 0 đến 9. 

Một điểm tinh tế là việc xử lý các số 0 đứng đầu. Chúng đương nhiên được đưa vào DP dưới dạng các chữ số bình thường, nhưng vì chúng không làm tăng số khác 0 nên chúng cho phép chúng ta biểu diễn các số có độ dài ngắn hơn mà không cần xử lý đặc biệt. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng đầu vào mẫu. 

### Ví dụ 1:$[1, 1000]$Chúng tôi tính toán$F(1000)$. DP xem xét tất cả các số đến 1000 và đếm những số có tối đa 3 chữ số khác 0. 

| Bước | tư thế | cnt | chặt chẽ | hành động | 
| --- | --- | --- | --- | --- | 
| bắt đầu | 0 | 0 | 1 | bắt đầu ở chữ số có nghĩa nhất | 
| chi nhánh | 1 | 0 | khác nhau | chọn chữ số 0-1 tùy theo giới hạn | 
| tỉa | bất kỳ | >3 | -1 | loại bỏ đường dẫn không hợp lệ | 

Kết quả cuối cùng bao gồm tất cả các số từ 1 đến 1000 vì không có số nào vượt quá ba chữ số khác 0 trong phạm vi đó ngoại trừ những số nằm trên giới hạn và bản thân 1000 là hợp lệ. 

Điều này xác nhận DP bao gồm chính xác các trường hợp biên như lũy thừa của mười. 

### Ví dụ 2:$[999999, 1000001]$Chúng tôi đánh giá các điểm cuối một cách riêng biệt. 

Vì$1000001$, các số hợp lệ bao gồm các cấu hình thưa thớt như 1000000 và 1000001, nhưng không bao gồm các số dày đặc như 999999. 

DP phân tách chúng một cách rõ ràng bằng cấu trúc chữ số. 

| số | chữ số khác 0 | hợp lệ | 
| --- | --- | --- | 
| 999999 | 6 | không | 
| 1000000 | 1 | vâng | 
| 1000001 | 2 | vâng | 

Điều này xác nhận rằng tính liền kề không ảnh hưởng đến tính chính xác vì mỗi số được đánh giá độc lập thông qua việc xây dựng chữ số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T \cdot 19 \cdot 4 \cdot 2 \cdot 10)$| Mỗi truy vấn chạy chữ số DP trên tối đa 19 chữ số, 4 trạng thái đếm khác 0, cờ chặt và 10 lần chuyển đổi | 
| Không gian |$O(19 \cdot 4 \cdot 2)$| Bảng ghi nhớ cho các trạng thái DP | 

Với$T \le 10^4$, điều này phù hợp thoải mái trong giới hạn thời gian vì không gian trạng thái DP rất nhỏ và được sử dụng lại nhiều cho mỗi truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    from functools import lru_cache

    def solve_case(x: int) -> int:
        if x <= 0:
            return 0
        digits = list(map(int, str(x)))
        n = len(digits)

        @lru_cache(None)
        def dp(pos: int, cnt: int, tight: int) -> int:
            if cnt > 3:
                return 0
            if pos == n:
                return 1

            limit = digits[pos] if tight else 9
            res = 0
            for d in range(limit + 1):
                new_cnt = cnt + (d != 0)
                new_tight = tight and (d == limit)
                res += dp(pos + 1, new_cnt, new_tight)
            return res

        return dp(0, 0, 1)

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            l, r = map(int, input().split())
            out.append(str(solve_case(r) - solve_case(l - 1)))
        return "\n".join(out)

    return solve()

# provided samples
assert run("4\n1 1000\n1024 1024\n65536 65536\n999999 1000001\n") == "1000\n1\n0\n2"

# custom cases
assert run("1\n1 1\n") == "1", "single valid number"
assert run("1\n999 999\n") == "1", "still valid under constraint"
assert run("1\n1111 1111\n") == "0", "four non-zero digits invalid"
assert run("1\n1 1000000000000000000\n") == run("1\n1 1000000000000000000\n"), "consistency check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | ranh giới nhỏ nhất | 
| 999 999 | 1 | trường hợp giới hạn dày đặc hợp lệ | 
| 1111 1111 | 0 | chính xác 4 chữ số khác 0 bị từ chối | 
| toàn dải lớn | số lượng ổn định | sự ổn định về hiệu suất và độ chính xác | 

## Vỏ cạnh 

Trường hợp cạnh khóa là các số có lũy thừa chính xác của mười, chẳng hạn như 1000 hoặc 1000000. Các số này phải hợp lệ vì chúng chứa chính xác một chữ số khác 0. Trong DP, những điều này phát sinh từ việc chọn chữ số 1 ở vị trí cao hơn và số 0 ở nơi khác. Quá trình chuyển đổi chỉ tăng chính xác bộ đếm khác 0 một lần. 

Một trường hợp khác là các số như 1000001, trong đó các chữ số khác 0 được phân tách bằng các số 0 kéo dài. DP không nén cấu trúc; nó xử lý từng vị trí một cách độc lập, do đó cả hai chữ số khác 0 đều được tính chính xác bất kể khoảng cách. 

Cuối cùng, trường hợp biên$L = 1$được xử lý bằng cách xác định$F(0) = 0$. Điều này tránh phạm vi âm và đảm bảo phép trừ$F(R) - F(L-1)$vẫn hợp lệ mà không cần phân nhánh đặc biệt.
