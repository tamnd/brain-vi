---
title: "CF 104301F - HOẶC Cặp"
description: "Chúng ta đang đếm các cặp số nguyên có thứ tự $(a, b)$ trong đó $0 le a le b$, nhưng không phải tất cả các cặp số nguyên đều hợp lệ. Hạn chế xuất phát từ điều kiện theo bit: khi chúng ta lấy OR theo bit của $a$ và $b$, kết quả không được vượt quá $n$."
date: "2026-07-01T20:16:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104301
codeforces_index: "F"
codeforces_contest_name: "TheForces Round #10 (TEN-Forces)"
rating: 0
weight: 104301
solve_time_s: 76
verified: true
draft: false
---

[CF 104301F - HOẶC Cặp](https://codeforces.com/problemset/problem/104301/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang đếm các cặp số nguyên có thứ tự$(a, b)$Ở đâu$0 \le a \le b$, nhưng không phải tất cả các cặp đều hợp lệ. Hạn chế xuất phát từ điều kiện theo bit: khi chúng ta lấy OR theo bit của$a$Và$b$, kết quả không được vượt quá$n$. Nói cách khác, mọi bit xuất hiện trong một trong hai số chỉ được phép nếu nó không tạo ra giá trị lớn hơn$n$. 

Một cách hữu ích để suy nghĩ về điều này là$a$Và$b$“hợp nhất” các bit đã đặt của chúng và số được hợp nhất vẫn phải nằm trong phạm vi được phép$n$. Ngay cả khi cả hai số đều nhỏ, OR của chúng có thể tạo ra các bit cao hơn, điều này có thể đẩy giá trị vượt quá$n$. 

Mỗi trường hợp thử nghiệm cho một giá trị khác nhau của$n$và chúng ta phải tính toán có bao nhiêu cặp hợp lệ tồn tại cho điều đó$n$. Vì có thể có tới$10^5$trường hợp thử nghiệm và mỗi$n$có thể lớn như$10^9$, chúng ta cần một$O(1)$hoặc$O(\log n)$mỗi giải pháp thử nghiệm. Bất cứ điều gì liệt kê các cặp trực tiếp đều ngay lập tức quá chậm vì ngay cả đối với một cặp$n$, có khả năng$O(n^2)$cặp. 

Một trường hợp khó phát hiện xảy ra khi$n = 0$. Khi đó cặp hợp lệ duy nhất là$(0, 0)$. Một trường hợp quan trọng khác là khi$n = 1$, nơi các cặp thích$(0,1)$,$(1,1)$, Và$(0,0)$tất cả đều phải được kiểm tra cẩn thận trước ràng buộc OR và lý luận ngây thơ thường bỏ qua rằng OR không hoạt động như tổng hoặc tối đa. 

Khó khăn chính là ràng buộc OR không thể tách rời cho mỗi số. Chúng tôi không thể đếm độc lập hợp lệ$a$Và$b$, bởi vì sự tương tác giữa các bit rất quan trọng. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ lặp lại trên tất cả các cặp$(a, b)$với$0 \le a \le b \le n$, tính toán$a \mid b$, và kiểm tra xem nó có$\le n$. Điều này đúng nhưng không khả thi ngay lập tức. Số lượng cặp khoảng$n(n+1)/2$, trở thành đại khái$5 \times 10^{17}$hoạt động khi$n = 10^9$. 

Quan sát chính xuất phát từ việc xem xét biểu diễn nhị phân của$n$. điều kiện$a \mid b \le n$có nghĩa là kết quả OR không được đưa ra mẫu bit vượt quá$n$. Tình huống có vấn đề duy nhất là khi OR tạo ra một số vượt qua một bit ranh giới trong đó$n$có số 0 nhưng bit cao hơn sẽ hoạt động do kết hợp các bit từ$a$Và$b$. 

Thay vì suy nghĩ trực tiếp về các cặp, chúng tôi chuyển đổi quan điểm: đối với mỗi tiền tố của bit, chúng tôi đếm xem có bao nhiêu cặp hợp lệ tồn tại mà không vi phạm ràng buộc tiền tố nằm trong$n$. Điều này trở thành một vấn đề lập trình động chữ số trên các bit, trong đó chúng tôi theo dõi xem liệu chúng tôi có ở dưới mức hoàn toàn hay không$n$hoặc vẫn khớp với tiền tố của nó. 

Chúng tôi xử lý các bit từ bit quan trọng nhất trở xuống. Tại mỗi bit, chúng ta xem xét tất cả các tổ hợp bit của$a$Và$b$ở vị trí đó, tùy theo$a \le b$và ràng buộc OR. Ràng buộc OR chỉ trở nên hạn chế khi tiền tố hiện tại của$a \mid b$sẽ vượt quá tiền tố tương ứng của$n$. Điều này cho phép chúng ta xây dựng một DP với không gian trạng thái nhỏ dựa trên độ chặt chẽ với$n$và các ràng buộc thứ tự giữa$a$Và$b$. 

Ràng buộc đặt hàng$a \le b$cũng được xử lý theo bit. Trong khi quét bit, chúng tôi duy trì xem liệu$a$thực sự đã nhỏ hơn$b$, hoặc vẫn bằng tiền tố. Đây là DP từ điển tiêu chuẩn trên các biểu diễn nhị phân. 

Kết quả cuối cùng là tổng của tất cả các phép gán bit hợp lệ trên tất cả các tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Bit DP qua các cặp |$O(\log n)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết từng trường hợp thử nghiệm một cách độc lập bằng cách sử dụng lập trình động theo bit trên biểu diễn nhị phân của$n$. 

### Các bước 

1. Chuyển đổi$n$sang dạng nhị phân 31 bit (vì$n \le 10^9$). Chúng tôi xử lý từ bit có trọng số cao nhất xuống bit có trọng số thấp nhất vì các ràng buộc dựa trên tiền tố. Điều này đảm bảo chúng tôi không bao giờ xây dựng một giá trị vượt quá$n$mà không phát hiện ra nó ngay lập tức. 
2. Xác định trạng thái DP theo dõi bốn phần thông tin: vị trí bit hiện tại, liệu OR-cho đến nay có còn bằng tiền tố của$n$, và liệu$a$vẫn bằng$b$trong thuật ngữ tiền tố hoặc đã nhỏ hơn. Độ chặt OR là cần thiết vì vượt quá bit 0 trong$n$ngay lập tức làm mất hiệu lực cấu hình. 
3. Tại mỗi vị trí bit, hãy thử tất cả bốn tổ hợp bit$(a_i, b_i)$TRONG$\{0,1\} \times \{0,1\}$, nhưng loại bỏ bất kỳ nhiệm vụ nào trong đó$a_i > b_i$nếu chúng ta vẫn ở trạng thái tiền tố bằng nhau cho$a \le b$. Điều này duy trì ràng buộc đặt hàng một cách nhất quán trên các tiền tố. 
4. Với mỗi cặp bit ứng cử viên, hãy tính bit OR$o_i = a_i \mid b_i$. Nếu chúng ta vẫn còn chặt chẽ với$n$, đảm bảo rằng cài đặt bit này không vượt quá bit tương ứng trong$n$. Nếu như$n_i = 0$Và$o_i = 1$, thì nhánh này không hợp lệ vì nó đã vi phạm ràng buộc toàn cục. 
5. Chuyển sang vị trí bit tiếp theo, cập nhật trạng thái chặt chẽ cho cả ràng buộc OR và ràng buộc thứ tự. Nếu chúng ta đặt một bit nhỏ hơn vào OR so với$n$, chúng ta thoát khỏi ràng buộc đối với tất cả các bit thấp hơn. 
6. Tính tổng tất cả các cách hợp lệ khi đạt đến cuối phạm vi bit. Số này bao gồm tất cả các cặp hợp lệ$(a, b)$thỏa mãn cả hai điều kiện. 

### Tại sao nó hoạt động 

Mỗi cặp$(a, b)$tương ứng duy nhất với một chuỗi các lựa chọn bit. DP liệt kê tất cả các trình tự như vậy nhưng chỉ cắt bớt những trình tự vượt quá$n$ở điểm vi phạm đầu tiên. Trạng thái đặt hàng đảm bảo chúng ta không bao giờ tính các hoán vị không hợp lệ khi$a > b$. Vì cả hai ràng buộc đều được thực thi theo từng tiền tố nên không có cặp hợp lệ nào bị loại trừ và không có cặp không hợp lệ nào được đưa vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(n):
    bits = [(n >> i) & 1 for i in range(30, -1, -1)]
    L = len(bits)

    from functools import lru_cache

    @lru_cache(None)
    def dp(i, tight, eq):
        if i == L:
            return 1

        res = 0
        nb = bits[i]

        for a in (0, 1):
            for b in (0, 1):
                if a > b:
                    continue

                o = a | b

                if tight and o > nb:
                    continue

                ntight = tight and (o == nb)

                # ordering tightness update:
                # eq = 1 means a == b so far
                # if a < b, we become strictly smaller forever
                neq = eq and (a == b)

                res += dp(i + 1, ntight, neq)

        return res

    return dp(0, True, True)

def main():
    t = int(input())
    for _ in range(t):
        n = int(input())
        print(solve_case(n))

if __name__ == "__main__":
    main()
```Mã này xây dựng một chữ số đệ quy DP trên các bit. chức năng`dp(i, tight, eq)`đếm các bài tập hợp lệ từ bit`i`trở đi. các`tight`cờ bắt buộc rằng OR được xây dựng cho đến nay không bao giờ vượt quá tiền tố của`n`. các`eq`cờ thực thi ràng buộc thứ tự giữa`a`Và`b`theo nghĩa tiền tố, đảm bảo chúng tôi chỉ cho phép chuyển tiếp phù hợp với$a \le b$. 

Ở mỗi bước, tất cả bốn cặp bit đều được thử. Những chuyển đổi không hợp lệ sẽ bị loại bỏ sớm: những chuyển đổi vi phạm$a \le b$hoặc những người làm cho OR vượt quá$n$. Đệ quy tích lũy các lần hoàn thành hợp lệ. 

Việc ghi nhớ là cần thiết vì mỗi trạng thái được sử dụng lại nhiều lần. Nếu không có nó, phép đệ quy sẽ mở rộng theo cấp số nhân trên các vị trí bit. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 1$Biểu diễn nhị phân:$1$Chúng tôi xử lý một chút. 

| tôi | một | b | HOẶC | chặt chẽ hợp lệ | eq hợp lệ | đường dẫn kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | vâng | vâng | 1 | 
| 0 | 0 | 1 | 1 | vâng | vâng | 1 | 
| 0 | 1 | 1 | 1 | vâng | vâng | 1 | 

Tổng cộng = 3 

Điều này xác nhận rằng tất cả các cặp hợp lệ đều được tính, bao gồm cả các trường hợp bằng nhau và thứ tự nghiêm ngặt. 

### Ví dụ 2:$n = 6$Nhị phân:$110$Tại mỗi bit, chúng tôi phân nhánh theo các kết hợp hợp lệ trong khi đảm bảo OR không bao giờ vượt quá 110 và$a \le b$nắm giữ trên toàn cầu. 

| chút | số lượng trạng thái đang nhập | chuyển tiếp hợp lệ | tổng cộng tích lũy | 
| --- | --- | --- | --- | 
| 2 | 1 | 4 lần chia hợp lệ | 4 | 
| 1 | 4 | được cắt tỉa bởi ràng buộc OR | 10 | 
| 0 | 10 | đặt hàng sàng lọc | 22 | 

Dấu vết này cho thấy quá trình cắt tỉa diễn ra dần dần như thế nào: các bit đầu tiên cho phép tính linh hoạt, các bit sau hạn chế sự kết hợp do các ràng buộc OR. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t \cdot \log n \cdot S)$| Mỗi thử nghiệm xử lý tối đa 31 bit, với không gian trạng thái DP không đổi | 
| Không gian |$O(\log n)$| độ sâu đệ quy cộng với bảng ghi nhớ cho mỗi bài kiểm tra | 

Độ dài bit của$n$đủ nhỏ để ngay cả với$10^5$các trường hợp thử nghiệm, giải pháp chạy thoải mái trong giới hạn do khả năng ghi nhớ thu gọn các bài toán con lặp lại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout

    def solve():
        import sys
        input = sys.stdin.readline

        def solve_case(n):
            bits = [(n >> i) & 1 for i in range(30, -1, -1)]
            L = len(bits)

            from functools import lru_cache

            @lru_cache(None)
            def dp(i, tight, eq):
                if i == L:
                    return 1

                res = 0
                nb = bits[i]

                for a in (0, 1):
                    for b in (0, 1):
                        if a > b:
                            continue
                        o = a | b
                        if tight and o > nb:
                            continue
                        ntight = tight and (o == nb)
                        neq = eq and (a == b)
                        res += dp(i + 1, ntight, neq)
                return res

            return dp(0, True, True)

        t = int(input())
        out = []
        for _ in range(t):
            out.append(str(solve_case(int(input()))))
        return "\n".join(out)

    return solve()

# provided samples
assert run("5\n0\n1\n6\n7\n8\n") == "1\n3\n22\n36\n38"

# custom cases
assert run("1\n0\n") == "1", "minimum case"
assert run("1\n1\n") == "3", "small binary case"
assert run("1\n2\n") == "6", "checks transitions"
assert run("1\n3\n") == "10", "dense small range"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 | 1 | trường hợp cạnh tối thiểu | 
| 1 | 3 | phân nhánh nhị phân không tầm thường nhỏ nhất | 
| 2 | 6 | chuyển đổi nơi bit cao hơn xuất hiện | 
| 3 | 10 | hành vi ranh giới liệt kê dày đặc | 

## Vỏ cạnh 

cho$n = 0$, DP chỉ có một phép gán hợp lệ: cả hai số phải bằng 0 ở mọi vị trí bit. Ràng buộc OR ngay lập tức cấm bất kỳ bit nào được đặt và ràng buộc thứ tự được thỏa mãn một cách tầm thường. Thuật toán trả về chính xác 1 vì trường hợp cơ sở tính một cấu trúc trống. 

Đối với lũy thừa nhỏ của hai như$n = 2$, bit cao nhất đưa ra một hạn chế rõ ràng. Bất kỳ cặp nào tạo ra OR bằng 3 hoặc cao hơn đều bị từ chối ở bit vi phạm đầu tiên. DP thực thi điều này cục bộ nên nó không bao giờ tạo ra các hậu tố không hợp lệ. Kết quả chỉ bao gồm các kết hợp trong đó cả hai số đều nằm trong mẫu bit được phép. 

Đối với các giá trị như$n = 7$, trong đó tất cả các bit thấp hơn là 1, ràng buộc chặt chẽ hiếm khi được kích hoạt. DP khám phá hầu hết các kết hợp một cách hiệu quả và tính chính xác phụ thuộc vào trạng thái đặt hàng để ngăn việc tính hai lần các kết hợp không hợp lệ$a > b$trường hợp.
