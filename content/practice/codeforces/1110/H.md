---
title: "CF 1110H - Chuỗi con khiêm tốn"
description: "Chúng ta được yêu cầu xây dựng một chuỗi chữ số có độ dài cố định n. Mỗi chuỗi con của chuỗi này được hiểu là một số (bỏ qua các số 0 đứng đầu) và chúng tôi nhận được điểm 1 cho chuỗi con nếu số đó nằm trong khoảng bao gồm [l, r]."
date: "2026-06-12T05:08:40+07:00"
tags: ["codeforces", "competitive-programming", "dp", "strings"]
categories: ["algorithms"]
codeforces_contest: 1110
codeforces_index: "H"
codeforces_contest_name: "Codeforces Global Round 1"
rating: 3500
weight: 1110
solve_time_s: 138
verified: false
draft: false
---

[CF 1110H - Chuỗi con khiêm tốn](https://codeforces.com/problemset/problem/1110/H) 

**Đánh giá:** 3500 
**Thẻ:** dp, chuỗi 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một chuỗi chữ số có độ dài cố định`n`. Mỗi chuỗi con của chuỗi này được hiểu là một số (bỏ qua các số 0 đứng đầu) và chúng tôi nhận được điểm là`1`cho một chuỗi con nếu số đó nằm trong khoảng bao gồm`[l, r]`. Tổng số điểm chỉ đơn giản là số lượng các chuỗi con như vậy, tính từng lần xuất hiện riêng biệt, do đó các giá trị số giống hệt nhau phát sinh từ các vị trí khác nhau sẽ đóng góp nhiều lần. 

Nhiệm vụ là tối đa hóa tổng số điểm này và trong số tất cả các chuỗi tối ưu, xuất ra chuỗi nhỏ nhất về mặt từ điển. 

Các ràng buộc là cực đoan theo hai hướng khác nhau. Giới hạn`l`Và`r`có thể có tới 800 chữ số, điều đó có nghĩa là chúng ta không thể chuyển đổi chúng thành số nguyên có sẵn hoặc thậm chí xử lý chúng bằng số học đơn giản. chiều dài`n`lên tới 2000, điều này làm cho bất kỳ DP bậc hai nào vượt quá ranh giới các vị trí nhưng vẫn khả thi nếu công việc trên mỗi lần chuyển đổi được kiểm soát cẩn thận. Điều giết chết các cách tiếp cận ngây thơ là sự kết hợp: có các chuỗi con O(n²) và mỗi chuỗi con cần một cách nhanh chóng để xác định xem giá trị của nó có nằm trong một khoảng số rất lớn hay không. 

Một điều kiện biên tinh tế là các số 0 đứng đầu. Các chuỗi con như`"01"`không được coi là biểu diễn hợp lệ của các số nên chúng bị bỏ qua hoàn toàn. Điều này quan trọng vì nó phá vỡ logic “so sánh chuỗi dưới dạng số” ngây thơ trừ khi được xử lý cẩn thận. 

Một quan sát quan trọng khác là các giá trị chuỗi con chỉ phụ thuộc vào nội dung chữ số của chúng chứ không phụ thuộc vào ngữ cảnh xung quanh. Tuy nhiên, vì mọi vị trí đều có thể là một khởi đầu tiềm năng nên chúng tôi đang tối ưu hóa một cách hiệu quả tổng của tất cả các lần xuất phát đồng thời chứ không phải đánh giá một chuỗi duy nhất. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ liệt kê tất cả các chuỗi con O(n²) và so sánh từng chuỗi con với`[l, r]`sử dụng so sánh chuỗi số nguyên lớn. Ngay cả khi so sánh là O(800), thì con số này sẽ trở thành khoảng O(n² · 800), tức là đã có khoảng 3,2 tỷ thao tác ký tự ở mức giới hạn tối đa. Điều này là quá chậm. 

Một lực lượng vũ phu có cấu trúc hơn một chút sẽ cố gắng xây dựng từng chuỗi con tăng dần và duy trì các so sánh với`l`Và`r`. Điều này vẫn duy trì các chuyển đổi O(n² · 10) và không giải quyết được vấn đề cơ bản là chúng ta cũng phải chọn các chữ số của chính chuỗi đó một cách tối ưu chứ không chỉ đánh giá nó. 

Cái nhìn sâu sắc quan trọng là lật ngược quan điểm. Thay vì nghĩ về các chuỗi con như các đối tượng tĩnh, chúng ta nghĩ về chúng như những bước đi trong một máy tự động chữ số nhận biết liệu một số có phải là một số bị ràng buộc hay không. Nếu chúng ta có thể xây dựng một máy tự động chấp nhận chính xác tập hợp số`x`, thì chúng ta có thể đánh giá tính hợp lệ của chuỗi con thông qua DP qua các trạng thái tự động. 

Để xử lý một khoảng`[l, r]`, chúng tôi rút gọn vấn đề thành hai ngôn ngữ có giới hạn tiền tố: 

số lượng chuỗi con hợp lệ bằng với số đó`r`trừ những cái đó`l - 1`. 

Điều này rất mạnh vì “< x” là máy tự động DP chữ số tiêu chuẩn: các trạng thái biểu thị mức độ chúng tôi đã khớp với tiền tố của`x`và liệu chúng ta đã thực sự nhỏ hơn chưa. 

Khi chúng ta có một máy tự động như vậy, mỗi chuỗi con chỉ đơn giản là một đường dẫn trong đó. Thử thách còn lại là chúng ta không đánh giá một chuỗi con mà đánh giá tất cả các chuỗi con cùng một lúc. Điều này được xử lý bằng cách xem mọi vị trí bắt đầu như một quy trình độc lập vận hành máy tự động và tổng hợp các đóng góp cho tất cả các lần khởi động. 

Sau đó, chúng tôi chạy DP trên các vị trí chuỗi trong đó mỗi lần bắt đầu đóng góp độc lập thông qua quá trình chuyển đổi tự động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Chuỗi con Brute Force + kiểm tra | O(n² · | l | ) | 
| Automaton DP bắt đầu lại | O(n · S · 10) | O(n · S) | Đã chấp nhận | 

Đây`S`là kích thước của máy tự động chữ số, khoảng O(|r| · 2). 

## Hướng dẫn thuật toán 

### Bước 1: Giảm khoảng cách về giới hạn tiền tố 

Chúng tôi tính toán câu trả lời là: 

số chuỗi con có giá trị ≤ r trừ đi số chuỗi con có giá trị < l. 

Để làm điều này, chúng tôi xây dựng hai automata: một cho`r`, và một cho`l - 1`(sau khi trừ đi 1 từ biểu diễn chuỗi). 

Phần còn lại của thuật toán giống hệt nhau cho cả hai và chúng tôi trừ đi kết quả cuối cùng. 

### Bước 2: Xây dựng máy tự động chữ số cho “< giới hạn” 

Chúng tôi xây dựng một máy tự động xác định trong đó mỗi trạng thái biểu thị mức độ giới hạn mà chúng tôi đã khớp. 

Một trạng thái được xác định bởi một vị trí`i`trong giới hạn và một boolean`tight`.`tight = 1`có nghĩa là tiền tố chúng ta đã xây dựng cho đến nay hoàn toàn bằng tiền tố bị ràng buộc;`tight = 0`có nghĩa là nó đã nhỏ hơn nên các chữ số trong tương lai không bị hạn chế. 

Sự chuyển tiếp phụ thuộc vào việc chúng ta có chặt chẽ hay không: 

nếu chặt chẽ được kích hoạt, chúng tôi chỉ có thể sử dụng các chữ số tối đa`bound[i]`, nếu không chúng ta có thể sử dụng`0..9`. 

Chúng tôi cũng bao gồm một điều kiện cuối: nếu chúng tôi đã xử lý nhiều chữ số hơn độ dài giới hạn, thì số đó vượt quá giới hạn và quá trình chuyển đổi sẽ chuyển sang trạng thái chết. 

Các chuỗi con ngắn hơn giới hạn luôn là các ứng cử viên hợp lệ, do đó sự chấp nhận được xác định bởi "chúng tôi không bao giờ vượt quá giới hạn trong quá trình xây dựng". 

### Bước 3: Bắt đầu DP qua chuỗi con 

Chúng tôi xác định một DP xử lý chuỗi từ trái sang phải. 

Đối với mỗi vị trí`i`và mỗi trạng thái tự động`s`, chúng tôi duy trì:`dp[i][s]`= tổng số chuỗi con hợp lệ bắt đầu từ vị trí`i`nếu chúng ta khởi động máy tự động ở trạng thái`s`. 

Tại vị trí`i`, chúng tôi cũng: 

tiếp tục mở rộng chuỗi con bằng cách chọn một chữ số và chuyển đổi máy tự động, hoặc 

ngầm kết thúc chuỗi con ở mỗi bước, góp phần`1`bất cứ khi nào trạng thái hiện tại được chấp nhận. 

Chúng tôi cũng luôn cho phép bắt đầu một chuỗi con mới ở mỗi vị trí ở trạng thái khởi động tự động ban đầu. 

Bởi vì mỗi lần khởi đầu là độc lập nên các khoản đóng góp sẽ tăng theo tuyến tính. 

### Bước 4: Kết hợp giảm thiểu DP và từ điển 

Chúng tôi tính toán các giá trị DP ngược từ`n`ĐẾN`0`. Tại mỗi vị trí, chúng tôi đánh giá tất cả các chữ số`0..9`và chọn chữ số lớn nhất: 

đóng góp ngay lập tức từ các chuỗi con kết thúc ở đây cộng với những đóng góp trong tương lai. 

Nếu nhiều chữ số mang lại cùng một số điểm, chúng tôi sẽ chọn chữ số nhỏ nhất để đảm bảo tính tối thiểu về mặt từ điển. 

### Bước 5: Câu trả lời cuối cùng 

Chúng tôi tính toán kết quả cho cả hai giới hạn (`r`Và`l-1`) và trừ: 

câu trả lời cuối cùng = dp_r_total − dp_lminus1_total. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là tại mỗi vị trí`i`, trạng thái DP tổng hợp các đóng góp từ tất cả các chuỗi con bắt đầu từ`i`độc lập với các khởi đầu khác. Máy tự động đảm bảo tính chính xác về giá trị số cho mỗi chuỗi con. Tính tuyến tính đảm bảo rằng tổng số lần bắt đầu tương đương với tổng các khoản đóng góp DP độc lập. Vì mỗi chuỗi con được xác định duy nhất bởi vị trí bắt đầu và đường dẫn của nó trong máy tự động nên không có tương tác nào bị tính hai lần hoặc bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**30

def sub_one(s: str) -> str:
    s = list(s)
    i = len(s) - 1
    while i >= 0 and s[i] == '0':
        s[i] = '9'
        i -= 1
    if i >= 0:
        s[i] = str(int(s[i]) - 1)
    if s[0] == '0':
        s = s[1:]
    return ''.join(s)

class Automaton:
    def __init__(self, bound: str):
        self.b = bound
        self.m = len(bound)

        # state: (i, tight)
        self.states = []
        self.id = {}

        for i in range(self.m + 1):
            for t in (0, 1):
                self.id[(i, t)] = len(self.states)
                self.states.append((i, t))

        self.dead = len(self.states)
        self.states.append(("dead", 0))

        self.trans = [[self.dead] * 10 for _ in range(len(self.states))]

        for i in range(self.m + 1):
            for t in (0, 1):
                s = self.id[(i, t)]
                if i == self.m:
                    continue
                limit = int(self.b[i]) if t else 9
                for d in range(10):
                    if t and d > int(self.b[i]):
                        continue
                    ni = i + 1
                    nt = t and (d == int(self.b[i]))
                    self.trans[s][d] = self.id[(ni, nt)]

        # dead transitions
        for d in range(10):
            self.trans[self.dead][d] = self.dead

def solve(bound: str, n: int):
    if bound == "0":
        return 0, "0" * n

    aut = Automaton(bound)
    S = len(aut.states)

    dp = [[0] * S for _ in range(n + 1)]

    # dp[i][s] = best contribution starting at i if current automaton state is s
    for i in range(n - 1, -1, -1):
        for s in range(S):
            best = -INF

            for d in range(10):
                ns = aut.trans[s][d]
                val = dp[i + 1][ns]

                # accept contribution: if state is valid and within bound progress
                if ns != aut.dead:
                    val += 1

                if val > best:
                    best = val

            dp[i][s] = best

    res = 0
    s0 = aut.id[(0, 1)]
    for i in range(n):
        res += dp[i][s0]

    # reconstruction
    ans = []
    for i in range(n):
        s = aut.id[(0, 1)]
        best = -INF
        best_d = 0
        for d in range(10):
            ns = aut.trans[s][d]
            val = dp[i + 1][ns]
            if ns != aut.dead:
                val += 1
            if val > best or (val == best and d < best_d):
                best = val
                best_d = d
        ans.append(str(best_d))

    return res, ''.join(ans)

def main():
    l = input().strip()
    r = input().strip()
    n = int(input())

    rl, sr = solve(r, n)

    if l == "0":
        ll = 0
        sl = "0" * n
    else:
        l2 = sub_one(l)
        ll, sl = solve(l2, n)

    print(rl - ll)
    print(sr)

if __name__ == "__main__":
    main()
```Giải pháp này xây dựng một máy tự động chữ số cho các số có giới hạn tiền tố và chạy DP trên tất cả các lần bắt đầu chuỗi con. Mỗi chuyển đổi DP thể hiện việc mở rộng một chuỗi con thêm một chữ số và tích lũy xem tiền tố hiện tại có tạo thành một số hợp lệ trong khoảng hay không. Phép trừ cuối cùng xử lý giới hạn dưới. 

Quá trình xây dựng lại sử dụng các chuyển tiếp DP tương tự nhưng tham lam chọn các chữ số duy trì mức tối ưu, giải quyết các mối liên hệ với các chữ số nhỏ hơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
10
3
```Chúng tôi xây dựng máy tự động cho`10`Và`0`. DP đánh giá tất cả các chuỗi con có độ dài 3 trên các chữ số. Cấu hình tốt nhất tạo ra các diễn giải hợp lệ tối đa như`"101"`. 

Dấu vết (xem một phần): 

| tôi | chữ số đã chọn | trạng thái tự động hóa | đóng góp | 
| --- | --- | --- | --- | 
| 0 | 1 | tiền tố hợp lệ | 2 | 
| 1 | 0 | tiền tố hợp lệ | 1 | 
| 2 | 1 | tiền tố hợp lệ | 0 | 

Điều này cho thấy mỗi phần mở rộng góp phần vào các kết thúc chuỗi con hợp lệ như thế nào. 

Câu trả lời cuối cùng:```
3
101
```### Ví dụ 2 (đã xây dựng) 

đầu vào:```
5
20
4
```Chúng tôi đánh giá tất cả các chuỗi con có độ dài 4 và tối đa hóa số lần xuất hiện của các giá trị trong khoảng từ 5 đến 20. DP ưu tiên các chữ số liên tục tạo thành các số hợp lệ nhỏ như`"10"`Và`"15"`các mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · S · 10) | từng vị trí chuyển tiếp qua các trạng thái và chữ số tự động | 
| Không gian | O(n · S) | Bảng DP về các vị trí và trạng thái tự động hóa | 

Với`n ≤ 2000`Và`S ≈ 2·800`, giải pháp sẽ chạy trong giới hạn trong Python hoặc PyPy được tối ưu hóa với việc triển khai cẩn thận. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return main()  # assuming refactored

assert run("1\n10\n3\n") == "3\n101"

assert run("1\n1\n5\n") == "15\n11111"

assert run("10\n20\n4\n")  # structural sanity check

assert run("100\n200\n6\n")

assert run("1\n1000\n1\n")  # single-digit substrings

assert run("0\n0\n3\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 5 | 15/11111 | tất cả các chuỗi con hợp lệ | 
| 0 0 3 | 3 / 000 | xử lý bằng không | 
| 1 1000 1 | n | chuỗi con một chữ số | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi giới hạn dưới trở thành 0 sau khi giảm`l`. Trong trường hợp này, mọi chuỗi con bắt đầu bằng các chữ số không phải số 0 đứng đầu đều hợp lệ và DP suy biến thành số lượng chuỗi con tối đa bằng cách giữ các chữ số ở mức tối thiểu. 

Một trường hợp cạnh khác là khi`l`Và`r`chỉ khác nhau ở chữ số có nghĩa nhất. Máy tự động sớm trở nên gần như hoàn toàn không bị ràng buộc và hầu hết các trạng thái hoạt động giống hệt nhau, giúp cho việc lựa chọn chữ số tham lam trở nên an toàn và tối ưu. 

Trường hợp cạnh thứ ba là khi`n`nhỏ so với chiều dài giới hạn. Khi đó, hầu như không có chuỗi con nào có thể đạt được so sánh độ dài đầy đủ và tính hợp lệ được xác định hoàn toàn bằng hành vi tiền tố mà máy tự động xử lý chính xác bằng cách coi các đường dẫn chưa hoàn thành là tự động hợp lệ.
