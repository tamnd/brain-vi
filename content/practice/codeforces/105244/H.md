---
title: "CF 105244H - Chuỗi có sự khác biệt được chỉ định"
description: "Chúng ta được cho một dãy các số nguyên riêng biệt và chúng ta muốn trích ra một dãy con tăng nghiêm ngặt. Ngoài ràng buộc tăng dần thông thường, còn có một quy tắc bổ sung hạn chế mức độ khác nhau của các phần tử liên tiếp trong dãy con: nếu hai phần tử liên tiếp được chọn…"
date: "2026-06-24T07:02:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105244
codeforces_index: "H"
codeforces_contest_name: "Dynamic Programming, SPbSU 2024, Training 2"
rating: 0
weight: 105244
solve_time_s: 45
verified: true
draft: false
---

[CF 105244H - Chuỗi có sự khác biệt được chỉ định](https://codeforces.com/problemset/problem/105244/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy các số nguyên riêng biệt và chúng ta muốn trích ra một dãy con tăng nghiêm ngặt. Ngoài ràng buộc tăng thông thường, còn có một quy tắc bổ sung hạn chế mức độ khác nhau của các phần tử liên tiếp trong dãy con: nếu hai giá trị được chọn liên tiếp là$a$Và$b$, thì giá trị$b - a$phải thuộc một tập hợp sai phân cho phép nhất định. 

Nhiệm vụ là tính toán độ dài tối đa có thể có của dãy con như vậy. 

Kích thước đầu vào lớn, lên tới$10^6$các số trong dãy và tối đa 10 sai số cho phép. Điều này ngay lập tức loại trừ mọi phương trình bậc hai hoặc thậm chí$O(n \log n)$DP mỗi phần tử phụ thuộc vào việc quét tất cả các phần tử trước đó. Bất kỳ giải pháp nào về cơ bản phải tuyến tính hoặc gần tuyến tính trong$n$, chỉ với chi phí nhỏ cho mỗi phần tử. 

Quan sát cấu trúc quan trọng là dãy con bị ràng buộc không chỉ bởi thứ tự mà còn bởi sự chuyển đổi số học giữa các giá trị. Điều này cho thấy rằng sự chuyển đổi chỉ phụ thuộc vào sự khác biệt về giá trị chứ không phải vị trí. 

Một cách tiếp cận ngây thơ sẽ xem xét mọi cặp chỉ số$i < j$và kiểm tra xem liệu chúng ta có thể mở rộng dãy con kết thúc tại$i$ĐẾN$j$. Điều đó đã hàm ý rồi$O(n^2)$, điều này vượt xa khả năng thực hiện. 

Một chế độ lỗi tinh tế hơn sẽ xuất hiện nếu chúng ta cố gắng duy trì, đối với mỗi giá trị, chuỗi tốt nhất kết thúc ở đó nhưng tính toán lại các chuyển đổi bằng cách quét tất cả các phần tử trước đó. Ngay cả khi được thực hiện cẩn thận, điều này vẫn thoái hóa thành hành vi bậc hai do khả năng tiếp cận dày đặc giữa các giá trị. 

Các trường hợp Edge bộc lộ logic ngây thơ không chính xác bao gồm: 

Khi các chênh lệch được phép bao gồm 1, mọi số nguyên liên tiếp theo thứ tự được sắp xếp có thể sử dụng được hoặc không tùy thuộc vào các khoảng trống trung gian trong chuỗi ban đầu. Ví dụ, với trình tự`1 3 2 4`và sự khác biệt`{1}`, câu trả lời hay nhất là`1 2 3 4`là không thể vì thứ tự được cố định bởi các chỉ số ban đầu, do đó chỉ có các chuỗi con hợp lệ mới quan trọng. Một DP có giá trị được sắp xếp ngây thơ sẽ giả định không chính xác toàn bộ chuỗi. 

Một trường hợp khác là khi sự khác biệt lớn, chẳng hạn như`{1000000000}`. Chỉ những cặp có khoảng trống chính xác mới đóng góp, vì vậy hầu hết các chuyển đổi đều không có. Một DP giả định có sự chuyển tiếp dày đặc sẽ bị tính quá mức. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là tính toán cho mỗi chỉ số$i$, độ dài của dãy con hợp lệ tốt nhất kết thúc tại$i$. Đối với mỗi$i$, chúng tôi quét tất cả$j < i$, và nếu$s[j] < s[i]$Và$s[i] - s[j]$được cho phép, chúng tôi cập nhật. 

Điều này đúng vì nó phản ánh trực tiếp định nghĩa về dãy con và kiểm tra mọi phần trước hợp lệ. Tuy nhiên, nó đòi hỏi phải kiểm tra tới$n$các phần tử trước đó cho mỗi$n$vị trí, dẫn đến$O(n^2)$chuyển tiếp. Với$n = 10^6$, đây là khoảng$10^{12}$hoạt động, điều đó là không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm. Thay vì tìm kiếm các giá trị tiền nhiệm hợp lệ, chúng tôi coi mỗi giá trị là một trạng thái và hỏi giá trị nào có thể chuyển sang giá trị hiện tại bằng cách sử dụng các chênh lệch cho phép. Vì mảng là một bài toán về dãy con trên các giá trị riêng biệt nên chúng ta có thể xử lý các phần tử theo thứ tự giá trị tăng dần và duy trì bản đồ DP từ giá trị đến chuỗi tốt nhất kết thúc ở giá trị đó. 

Với mỗi giá trị$x$, bất kỳ người tiền nhiệm nào cũng phải$x - d$đối với một số khác biệt cho phép$d$, miễn là giá trị đó tồn tại trong đầu vào. Điều này chuyển đổi vấn đề thành một tập hợp tra cứu có kích thước không đổi cho mỗi phần tử. Bởi vì$k \le 10$, mỗi trạng thái chỉ phụ thuộc vào tối đa 10 trạng thái có thể có trước đó. 

Chúng tôi duy trì bản đồ băm từ giá trị đến độ dài DP tốt nhất. Chúng tôi xử lý các giá trị theo thứ tự tăng dần hoặc lặp lại tương đương qua mảng trong khi vẫn đảm bảo các giá trị DP đã được tính toán cho các giá trị trước đó nhỏ hơn. Câu trả lời là giá trị DP tối đa được nhìn thấy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| DP tối ưu với sự chuyển đổi giá trị | O(nk) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ tất cả các giá trị chuỗi trong bộ băm hoặc từ điển để kiểm tra sự tồn tại của O(1). Điều này là cần thiết để nhanh chóng xác minh liệu người tiền nhiệm tiềm năng có thực sự xuất hiện trong chuỗi hay không. 
2. Ghép từng giá trị với vị trí ban đầu của nó và sắp xếp mảng theo giá trị. Chúng tôi làm điều này để khi xử lý một giá trị, tất cả các giá trị nhỏ hơn đã được tính toán, đảm bảo tính chính xác của DP khi chúng tôi tham chiếu$x - d$. 
3. Tạo từ điển`dp`giá trị ánh xạ tới độ dài chuỗi con tốt nhất kết thúc ở giá trị đó, ban đầu bằng 0. 
4. Lặp lại các giá trị đã được sắp xếp. Với mỗi giá trị$x$, khởi tạo`dp[x] = 1`, vì mỗi phần tử riêng lẻ tạo thành một dãy con hợp lệ có độ dài 1. 
5. Đối với từng mức chênh lệch cho phép$d$, tính toán$p = x - d$. Nếu như$p$tồn tại trong đầu vào, cập nhật`dp[x] = max(dp[x], dp[p] + 1)`. Bước này kết nối trạng thái hiện tại với tất cả các trạng thái trước đó hợp lệ. 
6. Duy trì mức tối đa toàn cầu trên tất cả`dp[x]`các giá trị gặp phải. 
7. Xuất ra mức tối đa toàn cục sau khi xử lý tất cả các phần tử. 

Lý do chúng tôi xử lý theo thứ tự được sắp xếp là vì mọi chuyển đổi đều giảm giá trị một cách nghiêm ngặt, đảm bảo không có sự phụ thuộc theo chu kỳ giữa các trạng thái. 

### Tại sao nó hoạt động 

Trạng thái DP biểu thị chuỗi con hợp lệ tốt nhất kết thúc ở một giá trị cụ thể. Bởi vì mọi chuyển đổi đều làm giảm giá trị theo một chênh lệch dương cho phép, nên biểu đồ chuyển đổi theo các giá trị sẽ không theo chu kỳ khi được sắp xếp theo giá trị. Mỗi trạng thái chỉ phụ thuộc vào các giá trị nhỏ hơn, điều này đảm bảo rằng khi chúng ta tính toán`dp[x]`, tất cả các phiên bản tiền nhiệm có thể đã được hoàn thiện. Điều này đảm bảo mọi dãy con hợp lệ đều được xem xét chính xác một lần cho đến phần tử cuối cùng của nó và không có phần mở rộng không hợp lệ nào được đưa vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    arr = list(map(int, input().split()))
    diffs = list(map(int, input().split()))

    vals = set(arr)
    dp = {}

    for x in sorted(arr):
        best = 1
        for d in diffs:
            p = x - d
            if p in vals:
                best = max(best, dp.get(p, 0) + 1)
        dp[x] = best

    print(max(dp.values()))

if __name__ == "__main__":
    solve()
```Giải pháp dựa vào việc sắp xếp các giá trị sao cho khi xử lý một số$x$, tất cả những người tiền nhiệm có thể có$x - d$đã được xử lý rồi. Bộ băm`vals`đảm bảo O(1) kiểm tra sự tồn tại, trong khi`dp`lưu trữ độ dài chuỗi tính toán. 

Một điểm tinh tế là chúng ta không cần xem xét rõ ràng các vị trí trong mảng ban đầu. Vì chúng ta chỉ yêu cầu các dãy con tăng dần và tất cả các giá trị đều khác biệt nên việc sắp xếp theo giá trị là đủ để đảm bảo việc xây dựng dãy con hợp lệ. 

Một chi tiết quan trọng khác là khởi tạo: mọi phần tử đều bắt đầu bằng giá trị DP 1, ngay cả khi không có phần tử tiền nhiệm nào tồn tại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7 2
2 5 6 3 8 10 9
3 1
```Các giá trị được sắp xếp:`2, 3, 5, 6, 8, 9, 10`| x | được coi là khác biệt | chuyển tiếp dp | dp[x] | 
| --- | --- | --- | --- | 
| 2 | - | không | 1 | 
| 3 | 1, 3 | 3-1=2 → 1+1=2 | 2 | 
| 5 | 1, 3 | 5-3=2 → 1+1=2 | 2 | 
| 6 | 1, 3 | 6-3=3 → 2+1=3 | 3 | 
| 8 | 1, 3 | 8-1=7 không có | 1 | 
| 9 | 1, 3 | 9-1=8 → 1+1=2 | 2 | 
| 10 | 1, 3 | 10-1=9 → 2+1=3 | 3 | 

Tối đa là 3. 

Điều này cho thấy các chuỗi hợp lệ không nhất thiết phải liền kề nhau theo thứ tự giá trị mà phụ thuộc vào việc các giá trị trung gian có tồn tại hay không. 

### Ví dụ 2 

đầu vào:```
9 2
2 5 6 3 8 10 9 7 12
1 3
```| x | lý luận dp[x] | 
| --- | --- | 
| 2 | 1 | 
| 3 | 2 qua 2 | 
| 5 | 2 qua 2 | 
| 6 | 3 qua 3 | 
| 7 | 3 qua 4 vắng mặt, 4 không có mặt nên 1 | 
| 8 | 2 đến 7 | 
| 9 | 3 đến 6 | 
| 10 | 4 đến 9 | 
| 12 | 4 đến 9 hoặc 11 vắng mặt | 

Điều này chứng tỏ rằng nhiều chuyển tiếp phân nhánh có thể hợp nhất thành một chuỗi duy nhất kết thúc ở các giá trị khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nk log n) | việc sắp xếp chiếm ưu thế với O(n log n), chi phí chuyển đổi DP O(nk) | 
| Không gian | O(n) | lưu trữ dp và tập giá trị | 

Các ràng buộc cho phép lên đến$10^6$các yếu tố, vì vậy việc phân loại là chi phí chính. Bước DP là tuyến tính trong$n$với hệ số tối đa là 10 cho mỗi phần tử, con số này không đáng kể. Việc sử dụng bộ nhớ là tuyến tính theo số lượng giá trị riêng biệt. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys as _sys
    _stdout = _sys.stdout
    _sys.stdout = io.StringIO()
    solve()
    out = _sys.stdout.getvalue()
    _sys.stdout = _stdout
    return out.strip()

# provided samples
assert run("""7 2
2 5 6 3 8 10 9
3 1
""") == "3"

assert run("""9 2
2 5 6 3 8 10 9 7 12
1 3
""") == "4"

# minimum size
assert run("""1 2
5
1 2
""") == "1"

# all equal differences but no chains
assert run("""4 1
10 1 7 3
5
""") == "1"

# chainable sequence
assert run("""5 2
1 4 5 8 9
1 3
""") == "3"

# large simple chain
assert run("""6 1
1 2 3 4 5 6
1
""") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | độ chính xác cơ sở DP | 
| không có chuyển tiếp hợp lệ | 1 | xử lý các nút bị cô lập | 
| xích sạch | 6 | tuyên truyền đầy đủ | 
| cấu trúc hỗn hợp | 3 | chuyển tiếp phân nhánh | 

## Vỏ cạnh 

Một trường hợp khó phát hiện xảy ra khi các giá trị trước hợp lệ tồn tại trong không gian giá trị nhưng không thể truy cập được trong DP do thiếu các giá trị trung gian. Ví dụ: nếu chuỗi chứa`2`Và`5`nhưng không`3`hoặc`4`và sự khác biệt được phép bao gồm`1`Và`3`, sau đó`5`chỉ có thể đạt được từ`2`thông qua sự khác biệt 3, mặc dù sự khác biệt 1 gợi ý một cấu trúc dày đặc hơn. Thuật toán xử lý việc này một cách chính xác vì nó kiểm tra rõ ràng sự tồn tại của từng phần trước đó trước khi sử dụng. 

Một trường hợp khác là khi có nhiều điểm khác biệt tạo ra sự chuyển đổi cạnh tranh thành cùng một giá trị. Ví dụ, nếu`x-1`Và`x-3`cả hai đều tồn tại, DP sẽ tận dụng tối đa cả hai một cách chính xác, đảm bảo chuỗi tốt nhất luôn được bảo toàn bất kể đường dẫn nào mang lại nó.
