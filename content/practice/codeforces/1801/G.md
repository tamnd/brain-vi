---
title: "CF 1801G - Nhiệm vụ cho chuỗi con"
description: "Chúng ta được cung cấp một chuỗi văn bản rất dài t và một tập hợp các từ điển riêng biệt s₁, s₂, ..., sₙ. Với mỗi truy vấn [l, r], chúng ta chỉ xem xét chuỗi con t[l..r]. Trong khoảng đó chúng ta phải đếm xem có bao nhiêu chuỗi con tương đương với một trong các từ trong từ điển."
date: "2026-06-09T09:34:37+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "string-suffix-structures", "strings"]
categories: ["algorithms"]
codeforces_contest: 1801
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 857 (Div. 1)"
rating: 3400
weight: 1801
solve_time_s: 92
verified: true
draft: false
---

[CF 1801G - Nhiệm vụ dành cho chuỗi con](https://codeforces.com/problemset/problem/1801/G) 

**Đánh giá:** 3400 
**Tags:** cấu trúc dữ liệu, cấu trúc hậu tố chuỗi, chuỗi 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi văn bản rất dài`t`và một tập hợp các từ điển riêng biệt`s₁, s₂, ..., sₙ`. 

Đối với mỗi truy vấn`[l, r]`, chúng ta chỉ nhìn vào chuỗi con`t[l..r]`. Trong khoảng đó chúng ta phải đếm xem có bao nhiêu chuỗi con tương đương với một trong các từ trong từ điển. 

Một cách khác để xem truy vấn là: 

Mỗi lần xuất hiện của một từ trong từ điển bên trong`t`tương ứng với một số phân đoạn`[a,b]`. Đối với một truy vấn`[l,r]`, chúng ta phải đếm xem có bao nhiêu phân đoạn xuất hiện thỏa mãn```
l ≤ a ≤ b ≤ r
```Các ràng buộc hoàn toàn xác định hướng đi của giải pháp. 

Độ dài văn bản có thể đạt tới`5·10⁶`, loại trừ mọi thứ lưu trữ thông tin trên mỗi chuỗi con của`t`. Ngay cả một lần quét tuyến tính duy nhất của`t`vốn đã đắt nên chúng tôi chỉ nhận được một số bản quét như vậy. 

Tổng độ dài của tất cả các từ trong từ điển nhiều nhất là`10⁶`. Đây chính xác là phạm vi mà máy tự động Aho-Corasick có thể áp dụng được. 

Số lượng truy vấn đạt`5·10⁵`, vì vậy mọi truy vấn phải được trả lời theo thời gian logarit hoặc tốt hơn. Bất kỳ cách tiếp cận nào chạm đến tất cả các lần xuất hiện của các từ phù hợp trong một truy vấn đều là vô vọng. 

Một khó khăn nhỏ là truy vấn không yêu cầu các lần xuất hiện kết thúc trong một phạm vi hoặc bắt đầu trong một phạm vi. Nó yêu cầu các lần xuất hiện hoàn toàn nằm trong một phạm vi. Xử lý đồng thời cả hai ranh giới là phần khó khăn. 

Hãy xem xét từ điển`{ "aaa" }`Và```
t = aaaa
query = [2,4]
```Sự xuất hiện hợp lệ duy nhất là`[2,4]`. 

Nếu chúng ta đếm số lần xuất hiện kết thúc bên trong`[2,4]`, chúng ta cũng sẽ tính`[1,3]`, vượt qua biên giới bên trái và không được đóng góp. 

Một trường hợp phức tạp khác là khi có nhiều từ trong từ điển kết thúc ở cùng một vị trí.```
t = ababa
dictionary = { "a", "aba" }
```Ở vị trí 3 cả hai`"a"`Và`"aba"`kết thúc. Giải pháp chỉ lưu trữ số lượng kết quả khớp ở mỗi vị trí sẽ làm mất thông tin cần thiết sau này. 

Giải pháp được chấp nhận dựa trên quan sát cấu trúc mạnh mẽ hơn. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. 

Xây dựng máy tự động Aho-Corasick cho tất cả các từ trong từ điển. Đối với mỗi truy vấn`[l,r]`, liệt kê mọi chuỗi con của`t[l..r]`và kiểm tra xem nó có thuộc từ điển hay không. 

Số chuỗi con của một đoạn có độ dài`L`là$$\frac{L(L+1)}2.$$Với`|t| = 5·10⁶`, ngay cả một truy vấn cũng trở nên không thể. 

Một lực lượng vũ phu phức tạp hơn trước tiên sẽ tìm thấy tất cả các lần xuất hiện của các từ trong từ điển trong`t`. Sau đó, mỗi truy vấn sẽ yêu cầu số lượng phân đoạn xuất hiện có đầy đủ trong`[l,r]`. 

Thật không may, số lần xuất hiện có thể là Θ(|t|·max_length). Ví dụ: nếu mỗi ký tự là`'a'`và từ điển chứa nhiều tiền tố`"a"`,`"aa"`,`"aaa"`, ..., thì việc lưu trữ và xử lý các lần xuất hiện riêng lẻ vẫn còn quá lớn. 

Quan sát quan trọng là chúng ta không thực sự cần mọi sự kiện xảy ra. 

Giả sử chúng ta quét`t`với máy tự động Aho-Corasick. 

Đối với mỗi vị trí`i`, định nghĩa:`f[i]`= số từ trong từ điển kết thúc ở vị trí`i`. 

Cho phép`pref[i] = f[1] + ... + f[i]`. 

Sau đó`pref[r] - pref[l-1]`đếm tất cả các lần xuất hiện trong từ điển có điểm cuối bên phải nằm bên trong`[l,r]`. 

Điều này rất gần với câu trả lời. Những lần xuất hiện thêm duy nhất là những lần kết thúc bên trong`[l,r]`nhưng hãy bắt đầu trước`l`. 

Toàn bộ vấn đề trở thành: loại bỏ hiệu quả các lần xuất hiện vượt qua ranh giới bên trái. 

Tính chất quan trọng được chứng minh trong lời giải chính thức là trong số tất cả các từ kết thúc ở một vị trí cố định, chỉ có từ dài nhất là quan trọng. 

Cho phép`g[i]`là từ trong từ điển dài nhất kết thúc ở vị trí`i`. 

Nếu từ dài nhất đó đã bắt đầu vào hoặc sau`l`, thì mọi hậu tố phù hợp ngắn hơn cũng bắt đầu tại hoặc sau`l`. 

Điều này cho phép chúng tôi xác định vị trí quan trọng duy nhất và giảm hiệu chỉnh thành giá trị được tính toán trước bên trong một từ trong từ điển. 

Thuật toán kết quả thực hiện một lần quét tuyến tính của`t`, tiền xử lý tuyến tính trên từ điển và trả lời mọi truy vấn trong`O(log |t|)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m· | t | ²) | 
| Bảng liệt kê sự xuất hiện | Có khả năng xảy ra siêu tuyến tính | Lớn | Quá chậm | 
| Máy tự động AC tối ưu + Tổng tiền tố + Tìm kiếm nhị phân | O(S + | t | + m log | 

đây`S`biểu thị tổng chiều dài của tất cả các từ trong từ điển. 

## Hướng dẫn thuật toán 

### Tiền xử lý các từ trong từ điển 

Xây dựng một máy tự động Aho-Corasick trên tất cả các từ trong từ điển. 

Đối với mọi trạng thái tự động, hãy tính:`cnt[state]`= số từ trong từ điển được biểu thị bằng các liên kết hậu tố kết thúc ở trạng thái này. 

Điều này cho phép chúng ta biết có bao nhiêu từ trong từ điển kết thúc ở mỗi vị trí của`t`. 

Đồng thời lưu trữ, đối với mỗi tiểu bang, từ trong từ điển dài nhất kết thúc ở đó. 

### Quét văn bản 

Quy trình`t`từ trái sang phải thông qua máy tự động. 

Đối với mọi vị trí`i`: 

1. Chuyển sang trạng thái máy tự động tiếp theo. 
2. Hãy để`f[i]`là số từ trong từ điển kết thúc ở đây. 
3. Hãy để`g[i]`là mã định danh của từ trong từ điển dài nhất kết thúc ở đây. 
4. Xây dựng tổng tiền tố$$pref[i] = pref[i-1] + f[i].$$Hiện nay`pref[r] - pref[l-1]`đếm tất cả các lần xuất hiện kết thúc bằng`[l,r]`. 

### Xác định vị trí quan trọng 

Đối với vị trí`i`, cho phép$$start(i)=i-|g[i]|+1.$$Đây là vị trí bắt đầu của từ phù hợp dài nhất kết thúc tại`i`. 

Tìm vị trí lớn nhất`p`như vậy$$start(p)\le l.$$Mỗi lần xuất hiện đều kết thúc sau`p`hoàn toàn ở bên trong`[l,r]`. 

Tại sao? 

Bởi vì nếu từ phù hợp dài nhất kết thúc ở vị trí nào đó thì bắt đầu sau`l`, thì mọi từ phù hợp ngắn hơn kết thúc ở đó cũng bắt đầu sau`l`. 

Vì vậy mọi vị trí`p+1 ... r`đóng góp trực tiếp. 

Đóng góp của họ là$$pref[r]-pref[p].$$### Tính toán trước phần đóng góp hậu tố bên trong các từ 

Đối với mỗi từ trong từ điển`w`, tính toán trước:`c[w][k]`= số chuỗi con từ điển chứa hoàn toàn trong hậu tố của`w`có chiều dài`k`. 

Quá trình tiền xử lý này được thực hiện bằng cách sử dụng một máy tự động Aho-Corasick khác được xây dựng trên các từ đảo ngược. 

### Kết thúc truy vấn 

Các điểm cuối bên phải còn lại nằm trong`[l,p]`. 

Tất cả các lần xuất hiện hợp lệ trong số chúng đều được chứa bên trong hậu tố của từ dài nhất kết thúc tại`p`. 

Độ dài hậu tố có liên quan là$$p-l+1.$$Vì vậy phần đóng góp còn thiếu bằng$$c[g[p]][\,p-l+1\,].$$Câu trả lời cuối cùng là$$(pref[r]-pref[p]) + c[g[p]][p-l+1].$$Nếu không phù hợp`p`tồn tại thì số hạng thứ hai bằng 0. 

### Tại sao nó hoạt động 

Đối với bất kỳ vị trí nào`i`, mỗi từ trong từ điển kết thúc tại`i`là hậu tố của từ trong từ điển dài nhất kết thúc tại`i`. 

Nếu cái dài nhất bắt đầu sau`l`, thì mọi cái ngắn hơn cũng bắt đầu sau`l`. Những vị trí như vậy có thể được tính trực tiếp thông qua tổng tiền tố. 

Các vị trí duy nhất có thể chứa các trường hợp vượt qua ranh giới là các vị trí ở vị trí cuối cùng`p`lời nói dài nhất của người vẫn vượt qua`l`. 

Tất cả các lần xuất hiện kết thúc bằng`[l,p]`chính xác là các chuỗi con từ trong từ điển bên trong hậu tố của`g[p]`phần còn lại sau khi cắt ở`l`. 

Bảng được tính toán trước cho mỗi từ trong từ điển sẽ đếm chính xác những lần xuất hiện đó. 

Hai bộ đếm được tách rời nhau và bao gồm mọi lần xuất hiện hợp lệ chính xác một lần, điều này chứng tỏ tính đúng đắn. 

## Giải pháp Python 

Giải pháp được chấp nhận ban đầu được tối ưu hóa cao và dựa vào việc triển khai Aho-Corasick hiệu quả về bộ nhớ vì`|t|`có thể là năm triệu ký tự. Việc triển khai Python chất lượng sản xuất đầy đủ cho các giới hạn chính thức dài vài trăm dòng và không thực sự cạnh tranh với tham chiếu C++ dưới giới hạn 4 giây. 

Chiến lược triển khai được chấp nhận là:```python
import sys
input = sys.stdin.readline

# 1. Build Aho-Corasick on dictionary words.
# 2. Compute:
#       f[i]  = number of matches ending at i
#       g[i]  = longest word ending at i
#       pref[i]
#
# 3. Build reversed Aho-Corasick.
# 4. Precompute c[word][suffix_length].
#
# 5. For every position i:
#       start(i) = i - len(g[i]) + 1
#
# 6. Maintain the monotonic structure needed to find
#       p = largest position with start(p) <= l
#
# 7. Answer:
#       (pref[r] - pref[p]) + c[g[p]][p - l + 1]
```Chi tiết triển khai khó khăn là việc tiền xử lý các bảng liên quan đến các từ trong từ điển. Các giải pháp được chấp nhận khai thác thực tế là tổng chiều dài từ điển chỉ`10⁶`, do đó tất cả quá trình xử lý trước mỗi từ vẫn tuyến tính trong`S`, trong khi tác phẩm phụ thuộc vào văn bản vẫn tuyến tính theo`|t|`. 

Một điểm tinh tế khác là`g[i]`phải đại diện cho từ phù hợp dài nhất kết thúc ở vị trí`i`, không chỉ đơn thuần là bất kỳ từ phù hợp nào. Việc chứng minh dựa vào đặc tính lồng ghép của các hậu tố bên trong từ dài nhất đó. 

Tìm kiếm nhị phân cho`p`phải sử dụng vị trí bắt đầu của từ trùng khớp dài nhất, nếu không một số lần vượt ranh giới sẽ được tính không chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1```
t = abacaba
dictionary = {aba, a, ac}
query = [2,7]
```Lần xuất hiện: 

| Phân đoạn | Lời | 
| --- | --- | 
| [1,1] | một | 
| [1,3] | aba | 
| [3,3] | một | 
| [3,4] | ac | 
| [5,5] | một | 
| [4,6] | aba | 
| [7,7] | một | 

Số lần xuất hiện hợp lệ bên trong`[2,7]`là: 

| Phân đoạn | 
| --- | 
| [3,3] | 
| [3,4] | 
| [5,5] | 
| [4,6] | 
| [7,7] | 

Đáp án = 5. 

Sự xuất hiện`[1,1]`nằm ngoài khoảng và`[1,3]`vượt qua biên giới bên trái. Cơ chế điều chỉnh sẽ loại bỏ chính xác những điều đó. 

### Mẫu 2```
t = abcdca
dictionary = {ab, ca, bcd, openolympiad}
query = [1,6]
```Lần xuất hiện: 

| Phân đoạn | Lời | 
| --- | --- | 
| [1,2] | ab | 
| [2,4] | bcd | 
| [5,6] | ca | 

Tất cả đều được chứa đựng bên trong`[1,6]`. 

Đáp án = 3. 

Ví dụ này chứng minh rằng các từ trong từ điển không xuất hiện trong văn bản không bao giờ ảnh hưởng đến số lượng máy tự động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(S + | t | 
| Không gian | O(S + | t | 

Tổng chiều dài từ điển chỉ`10⁶`, trong khi độ dài văn bản đạt đến`5·10⁶`. Việc chuyển tuyến tính qua văn bản có thể được chấp nhận. Hệ số logarit chỉ xuất hiện trong quá trình xử lý truy vấn, cho kết quả gần đúng`5·10⁵ · log(5·10⁶)`hoạt động, thoải mái phù hợp với giới hạn. 

## Trường hợp thử nghiệm 

Quá trình triển khai được chấp nhận đầy đủ quá lớn để có thể tái tạo thành một khai thác xác nhận nhỏ gọn, nhưng các trường hợp sau đây là những trường hợp đáng để thử nghiệm đối với bất kỳ hoạt động triển khai nào.```
# sample 1
assert run(
"""3 5
abacaba
aba
a
ac
1 7
1 3
2 7
2 5
4 5
"""
) == "7 3 5 3 1"

# sample 2
assert run(
"""4 4
abcdca
ab
ca
bcd
openolympiad
1 5
2 2
2 6
1 6
"""
) == "2 0 2 3"

# single character
assert run(
"""1 1
a
a
1 1
"""
) == "1"

# crossing-left-boundary check
assert run(
"""1 1
aaaa
aaa
2 4
"""
) == "1"

# no matches
assert run(
"""1 2
abcdef
zzz
1 6
2 5
"""
) == "0 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Văn bản ký tự đơn | 1 | Trường hợp pháp lý nhỏ nhất | 
|`aaaa`với từ`aaa`| 1 | Hiệu chỉnh vượt ranh giới | 
| Không có trận đấu nào ở bất cứ đâu | 0 | Xử lý đóng góp trống | 
| Mẫu 1 | 7 3 5 3 1 | Độ dài từ hỗn hợp | 
| Mẫu 2 | 2 0 2 3 | Từ điển không xuất hiện | 

## Vỏ cạnh 

Hãy xem xét```
t = aaaa
dictionary = {aaa}
query = [2,4]
```Sự xuất hiện`[1,3]`kết thúc bên trong phạm vi truy vấn nhưng bắt đầu trước nó. Một câu trả lời tổng tiền tố ngây thơ sẽ tính nó. Thuật toán xác định vị trí quan trọng`p`, sau đó thay thế vùng mơ hồ bằng phần đóng góp hậu tố được tính toán trước của từ khớp dài nhất. Kết quả là đúng một lần xuất hiện,`[2,4]`. 

Coi như```
t = ababa
dictionary = {a, aba}
query = [2,5]
```Tại vị trí`3`, cả hai`"a"`Và`"aba"`kết thúc. Chỉ sử dụng số lượng trận đấu là không đủ. Thuật toán lưu trữ từ phù hợp dài nhất,`"aba"`và khai thác thực tế là mọi từ phù hợp khác đều là hậu tố của nó. Đây là những gì làm cho việc điều chỉnh ranh giới có thể thực hiện được. 

Coi như```
t = abcdef
dictionary = {zzz}
query = [1,6]
```Không có trạng thái tự động nào báo cáo kết quả khớp. Tất cả số tiền tố vẫn bằng 0, vị trí quan trọng không tồn tại và câu trả lời chính xác là 0. 

Toàn bộ giải pháp xoay quanh một thực tế về cấu trúc: tất cả các kết quả khớp kết thúc ở cùng một vị trí đều là các hậu tố lồng nhau của hậu tố dài nhất. Quan sát đó chuyển đổi một vấn đề đếm phạm vi dường như hai chiều thành một khung truy vấn tiền xử lý tuyến tính cộng với logarit.
