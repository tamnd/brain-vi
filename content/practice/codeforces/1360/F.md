---
title: "CF 1360F - Chuỗi gián điệp"
description: "Chúng ta có một số chuỗi có độ dài bằng nhau và chúng ta muốn tạo một chuỗi mới có cùng độ dài. Yêu cầu là chuỗi được xây dựng này phải cực kỳ gần với mọi chuỗi đã cho: đối với mỗi chuỗi đầu vào, nó được phép khác với chuỗi được xây dựng của chúng tôi tại…"
date: "2026-06-18T18:19:22+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "constructive-algorithms", "dp", "hashing", "strings"]
categories: ["algorithms"]
codeforces_contest: 1360
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 644 (Div. 3)"
rating: 1700
weight: 1360
solve_time_s: 73
verified: false
draft: false
---

[CF 1360F - Chuỗi gián điệp](https://codeforces.com/problemset/problem/1360/F) 

**Đánh giá:** 1700 
**Tags:** bitmasks, brute Force, thuật toán xây dựng, dp, băm, chuỗi 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số chuỗi có độ dài bằng nhau và chúng ta muốn tạo một chuỗi mới có cùng độ dài. Yêu cầu là chuỗi được xây dựng này phải cực kỳ gần với mọi chuỗi đã cho: đối với mỗi chuỗi đầu vào, nó được phép khác với chuỗi được xây dựng của chúng tôi ở tối đa một vị trí. 

Một cách khác để thấy điều đó là mỗi chuỗi đầu vào chỉ có thể “không đồng ý” với câu trả lời của chúng ta một lần. Trên tất cả các vị trí, mỗi chuỗi cho phép nhiều nhất một lỗi không khớp. 

Nhiệm vụ không phải là tối ưu hóa bất kỳ thứ gì như thứ tự từ điển hoặc giảm thiểu các thay đổi. Chúng ta chỉ cần quyết định xem một cấu trúc hợp lệ có tồn tại hay không và xuất ra bất kỳ một chuỗi hợp lệ nào. 

Các ràng buộc nhỏ theo một cách rất cụ thể: cả số lượng chuỗi và độ dài của chúng tối đa là 10. Điều đó ngay lập tức loại trừ mọi nhu cầu về thủ thuật tối ưu hóa tiệm cận. Thậm chí việc kiểm tra tất cả các khả năng về ký tự trên mỗi vị trí cũng có thể thực hiện được, vì không gian tìm kiếm tối đa là$26^{10}$, nhưng quan trọng hơn, cấu trúc của ràng buộc khiến cho việc sử dụng vũ lực trở nên không cần thiết nếu chúng ta suy nghĩ cục bộ trên mỗi vị trí. 

Một ý tưởng ngây thơ nhưng tự nhiên là thử xây dựng câu trả lời theo từng ký tự. Tuy nhiên, một dạng thất bại tinh tế sẽ xuất hiện nếu chúng ta chỉ xem xét thỏa thuận cục bộ ở từng vị trí một cách độc lập. Ràng buộc là toàn cục trên mỗi chuỗi, không phải trên mỗi vị trí. 

Ví dụ: giả sử chúng ta cố gắng chọn ký tự đa số ở mỗi vị trí. Điều này có thể thất bại vì một chuỗi có thể khác nhau ở hai vị trí khác nhau, ngay cả khi mỗi vị trí riêng lẻ có vẻ an toàn. 

Coi như:```
n = 2, m = 3
a1 = "aba"
a2 = "bab"
```Nếu chúng ta chọn`"aaa"`theo lý luận đa số, cả hai chuỗi khác nhau hai lần, điều này không hợp lệ. Vì vậy, sự tham lam về mặt vị trí là không an toàn. 

Khó khăn chính là mỗi chuỗi có ngân sách chính xác là một điểm không khớp và chúng tôi phải phân phối những điểm không khớp này một cách nhất quán trên tất cả các vị trí. 

## Phương pháp tiếp cận 

Phối cảnh bạo lực bắt đầu từ việc quan sát rằng chuỗi câu trả lời cũng có độ dài$m \le 10$, vì vậy chúng ta có thể thử tất cả các chuỗi có thể có trên 26 chữ cái. Đối với mỗi chuỗi ứng cử viên, chúng tôi kiểm tra mọi chuỗi đầu vào và đếm các chuỗi không khớp. Nếu mỗi chuỗi có nhiều nhất một chuỗi không khớp thì chúng tôi chấp nhận. 

Điều này đúng nhưng đắt tiền: có$26^m$ứng viên, và mỗi lần kiểm tra chi phí$O(nm)$. Với trường hợp xấu nhất$m = 10$, điều này trở thành$26^{10} \approx 1.4 \cdot 10^{14}$, điều này vượt xa khả năng thực hiện được. 

Cấu trúc của điều kiện cho phép một cái nhìn sắc nét hơn. Vì mỗi chuỗi có thể khác với câu trả lời ở nhiều nhất một vị trí, nên chúng ta có thể nghĩ ngược lại: nếu chúng ta đoán một “chuỗi cơ sở” gần với tất cả các chuỗi đầu vào, thì mọi chuỗi đầu vào đều bằng chuỗi đó hoặc khác nhau chính xác ở một vị trí. Điều đó gợi ý rằng câu trả lời phải cực kỳ gần với ít nhất một trong các chuỗi đầu vào, bởi vì mỗi chuỗi chỉ có thể chấp nhận một sai lệch duy nhất so với câu trả lời. 

Cái nhìn sâu sắc quan trọng là neo cấu trúc trên một trong các chuỗi nhất định. Giả sử chúng ta thử từng chuỗi đầu vào$a_i$làm mẫu ứng viên. Nếu chúng ta bắt đầu từ$a_i$, câu trả lời cuối cùng phải khác nó ở nhiều nhất một vị trí, bởi vì$a_i$bản thân nó phải khác với câu trả lời ở nhiều nhất một vị trí. Vì vậy, câu trả lời có thể thu được bằng cách giữ$a_i$không thay đổi hoặc sửa đổi đúng một vị trí của nó. 

Điều này làm giảm đáng kể không gian tìm kiếm: với mỗi$a_i$, chúng ta chỉ cần thử thay đổi 0 hoặc một ký tự, sau đó xác minh tính khả thi đối với tất cả các chuỗi. 

Từ$n, m \le 10$, điều này trở nên rất nhỏ: nhiều nhất$10 \times (1 + 10 \times 25)$ứng viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các dây |$O(26^m \cdot nm)$|$O(1)$| Quá chậm | 
| Hãy thử từng chuỗi cơ sở, sửa đổi vị trí 1 |$O(n^2 m \cdot 26)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp lại từng chuỗi$a_i$và coi nó như một chuỗi cơ sở tiềm năng. 

Lý do là câu trả lời cuối cùng phải khác với mọi chuỗi đầu vào ở nhiều nhất một vị trí, do đó không thể cách quá xa bất kỳ chuỗi nào trong số đó. 
2. Đầu tiên kiểm tra ứng viên tại nơi chúng tôi đặt$s = a_i$không có sửa đổi. 

Chúng tôi tính toán cho mỗi chuỗi$a_j$, có bao nhiêu vị trí khác nhau giữa$s$Và$a_j$và xác minh rằng tất cả đều lớn nhất là 1. 
3. Nếu cơ sở chưa sửa đổi hoạt động, chúng tôi có thể xuất nó ngay lập tức. 

Điều này tương ứng với trường hợp một chuỗi đầu vào đã thỏa mãn ràng buộc chung. 
4. Nếu không, hãy thử sửa đổi chính xác một vị trí của$a_i$. Đối với mỗi vị trí$p$từ 0 đến$m-1$, và với mỗi ký tự$c$từ 'a' đến 'z', tạo chuỗi ứng cử viên bằng cách thay thế$a_i[p]$với$c$. 
5. Đối với mỗi ứng cử viên như vậy, hãy xác minh nó dựa trên tất cả các chuỗi đầu vào theo cùng một cách, đếm các giá trị không khớp trên mỗi chuỗi. 

Chúng tôi loại bỏ các ứng cử viên có bất kỳ chuỗi nào khác nhau ở nhiều vị trí. 
6. Nếu có thí sinh nào đạt thì xuất ngay. 
7. Nếu không có chuỗi và không có sửa đổi vị trí đơn nào hoạt động, xuất -1. 

Tính chính xác phụ thuộc vào thực tế là mọi câu trả lời hợp lệ đều phải nằm trong khoảng cách Hamming tối đa là 1 tính từ mọi chuỗi đầu vào và do đó phải nằm trong khoảng cách tối đa là 1 tính từ bất kỳ ứng cử viên cơ sở hợp lệ nào được chọn trong số đó. 

### Tại sao nó hoạt động 

Giả sử một câu trả lời hợp lệ$s$tồn tại. Chọn bất kỳ chuỗi đầu vào$a_i$. Từ$a_i$khác với$s$ở nhiều nhất một vị trí,$a_i$Và$s$khác nhau ở nhiều nhất một chỉ số. Điều đó có nghĩa$s$có thể được lấy từ$a_i$bằng cách thay đổi nhiều nhất một ký tự. 

Vì vậy, nếu một giải pháp tồn tại, nó phải xuất hiện trong số các bảng liệt kê được tạo ra từ một số$a_i$: hoặc$a_i$chính nó hoặc một sửa đổi một ký tự. Vì chúng ta đã thử tất cả$a_i$, chúng ta phải gặp phải$s$. Bước xác minh đảm bảo chúng tôi chỉ chấp nhận các công trình thực sự hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ok(s, arr, n, m):
    for a in arr:
        diff = 0
        for i in range(m):
            if s[i] != a[i]:
                diff += 1
                if diff > 1:
                    return False
        if diff > 1:
            return False
    return True

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    arr = [input().strip() for _ in range(n)]

    ans = None

    for base in arr:
        if ok(base, arr, n, m):
            ans = base
            break

        base = list(base)
        found = False

        for i in range(m):
            original = base[i]
            for c in "abcdefghijklmnopqrstuvwxyz":
                base[i] = c
                candidate = "".join(base)
                if ok(candidate, arr, n, m):
                    ans = candidate
                    found = True
                    break
            base[i] = original
            if found:
                break

        if ans is not None:
            break

    if ans is None:
        print(-1)
    else:
        print(ans)
```Mã trực tiếp tuân theo ý tưởng mang tính xây dựng về việc neo trên mỗi chuỗi đầu vào. Chức năng trợ giúp`ok`tính toán xem một chuỗi ứng cử viên có thỏa mãn ràng buộc đối với tất cả các đầu vào hay không bằng cách đếm rõ ràng các giá trị không khớp và dừng sớm nếu một cặp vượt quá một chênh lệch. 

Vòng lặp xây dựng bên trong khôi phục cẩn thận các ký tự sau khi sửa đổi tạm thời, đảm bảo chúng tôi chỉ kiểm tra các biến thể vị trí đơn hợp lệ của từng chuỗi cơ sở. 

Cấu trúc ngắt sớm rất quan trọng vì một khi đã tìm thấy câu trả lời hợp lệ thì không cần tìm kiếm thêm nữa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 4
abac
zbab
```Chúng tôi thử cơ sở`"abac"`. 

| Ứng viên | Kiểm tra so với "abac" | Kiểm tra so với "zbab" | hợp lệ | 
| --- | --- | --- | --- | 
| bàn tính | 0 | 2 | Không | 
| abbc | 1 | 1 | Có | 

Chúng tôi sửa đổi vị trí 2 thành 'b', tạo ra`"abbc"`. Điều này thỏa mãn cả hai chuỗi trong một chuỗi không khớp. 

Dấu vết cho thấy rằng giải pháp đúng không nhất thiết phải bằng bất kỳ chuỗi đầu vào nào, nhưng rất gần với một trong số chúng. 

### Ví dụ 2 

đầu vào:```
2 4
aaaa
bbbb
```Thử`"aaaa"`: 

| Ứng viên | Kiểm tra so với "aaaa" | Kiểm tra so với "bbbb" | hợp lệ | 
| --- | --- | --- | --- | 
| aaa | 0 | 4 | Không | 
| aba | 1 | 3 | Không | 
| ... | ... | ... | Không | 

Không có sửa đổi nào có tác dụng cả, vì vậy câu trả lời là`-1`. 

Điều này cho thấy rằng mặc dù mỗi chuỗi đều đơn giản nhưng khoảng cách tương hỗ của chúng có thể khiến cho việc có một trung tâm nhất quán toàn cầu là không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2 \cdot m \cdot 26)$| Đối với mỗi$n$chuỗi cơ sở, chúng tôi cố gắng lên tới$m \cdot 26$sửa đổi và mỗi lần kiểm tra sẽ quét tất cả$n$chuỗi có độ dài$m$. | 
| Không gian |$O(1)$| Chỉ các chuỗi tạm thời được tạo trong quá trình xây dựng và xác minh. | 

Với$n, m \le 10$, đây thực sự là thời gian không đổi cho mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        arr = [input().strip() for _ in range(n)]

        def ok(s):
            for a in arr:
                diff = 0
                for i in range(m):
                    if s[i] != a[i]:
                        diff += 1
                        if diff > 1:
                            return False
            return True

        ans = None
        for base in arr:
            if ok(base):
                ans = base
                break
            base = list(base)
            for i in range(m):
                orig = base[i]
                for c in "abcdefghijklmnopqrstuvwxyz":
                    base[i] = c
                    if ok("".join(base)):
                        ans = "".join(base)
                        break
                base[i] = orig
                if ans:
                    break
            if ans:
                break

        out.append(ans if ans else "-1")

    return "\n".join(out)

# provided samples
assert run("""5
2 4
abac
zbab
2 4
aaaa
bbbb
3 3
baa
aaa
aab
2 2
ab
bb
3 1
a
b
c
""") == """abab
-1
aaa
ab
z"""

# custom cases
assert run("""1
1 5
abcde
""") == "abcde", "single string always valid"

assert run("""1
2 2
aa
cc
""") in {"ac", "ca"}, "two strings with symmetric solution"

assert run("""1
3 3
abc
def
ghi
""") == "-1", "completely incompatible strings"

assert run("""1
2 3
abc
abd
""") in {"abc", "abd"}, "already valid base exists"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi đơn | chính nó | tính đúng đắn của trường hợp cơ sở | 
| aa/cc | ac hoặc ca | giải pháp một thay đổi đối xứng | 
| ghi abc def | -1 | cấu hình không thể | 
| abc / abd | một trong hai chuỗi | xử lý đầu vào đã hợp lệ | 

## Vỏ cạnh 

Một trường hợp khó nhận thấy là khi nhiều chuỗi khác nhau ở nhiều hơn hai vị trí tổng thể, nhưng vẫn cho phép có một trung tâm chung. Ví dụ: nếu tất cả các chuỗi chỉ khác nhau trong một cột thì mọi lựa chọn nhất quán đều có tác dụng. Thuật toán xử lý việc này vì việc chọn bất kỳ chuỗi cơ sở nào sẽ ngay lập tức vượt qua quá trình xác minh. 

Một trường hợp đặc biệt khác là khi không có chuỗi cơ sở đơn nào hoạt động mà không sửa đổi, nhưng việc sửa đổi một ký tự sẽ khắc phục được tất cả các xung đột. Vòng lặp xây dựng khám phá rõ ràng mọi vị trí và mọi nhân vật, đảm bảo không bỏ sót trung tâm ẩn như vậy. 

Trường hợp cuối cùng là khi câu trả lời không tồn tại mặc dù khoảng cách giữa các cặp có vẻ nhỏ. Ví dụ`"aaaa"`Và`"bbbb"`chứng minh rằng mặc dù mỗi cặp cách xa nhau nhưng không có chuỗi nào có thể thỏa mãn ràng buộc một không khớp đồng thời cho cả hai. Bước xác minh trên tất cả các chuỗi sẽ loại bỏ chính xác mọi ứng cử viên trong tình huống đó.
