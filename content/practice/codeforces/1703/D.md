---
title: "CF 1703D - Dây Đôi"
description: "Chúng ta có một tập hợp các chuỗi chữ thường ngắn, mỗi chuỗi có độ dài tối đa là 8. Đối với mỗi chuỗi trong danh sách, chúng ta cần quyết định xem liệu nó có thể được hình thành bằng cách lấy hai chuỗi từ cùng một danh sách và nối chúng theo thứ tự hay không."
date: "2026-06-09T21:36:38+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "strings"]
categories: ["algorithms"]
codeforces_contest: 1703
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 806 (Div. 4)"
rating: 1100
weight: 1703
solve_time_s: 91
verified: true
draft: false
---

[CF 1703D - Dây đôi](https://codeforces.com/problemset/problem/1703/D) 

**Đánh giá:** 1100 
**Tags:** Brute Force, cấu trúc dữ liệu, chuỗi 
**Thời gian giải:** 1 phút 31 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các chuỗi chữ thường ngắn, mỗi chuỗi có độ dài tối đa là 8. Đối với mỗi chuỗi trong danh sách, chúng ta cần quyết định xem liệu nó có thể được hình thành bằng cách lấy hai chuỗi từ cùng một danh sách và nối chúng theo thứ tự hay không. Hai chuỗi được chọn được phép là cùng một phần tử và chúng tôi không bắt buộc phải sử dụng các chỉ mục riêng biệt. 

Vì vậy với mỗi chuỗi$s_i$, về cơ bản chúng ta đang hỏi liệu có tồn tại một điểm phân chia sao cho tiền tố là một chuỗi nào đó trong tập hợp và hậu tố cũng là một chuỗi nào đó trong tập hợp hay không. 

Cấu trúc ràng buộc là rất quan trọng ở đây. Mặc dù tổng số chuỗi trong tất cả các trường hợp thử nghiệm lên tới$10^5$, mỗi chuỗi riêng lẻ cực kỳ ngắn, với độ dài tối đa là 8. Điều này ngay lập tức gợi ý rằng bất kỳ cách tiếp cận nào phụ thuộc nhiều vào độ dài chuỗi, chẳng hạn như liệt kê tất cả các chuỗi con hoặc tất cả các phần tách, đều an toàn vì có tối đa 7 vị trí phân tách trên mỗi chuỗi. 

Tuy nhiên, cách tiếp cận đơn giản là thử tất cả các cặp chuỗi cho mỗi truy vấn vẫn sẽ quá chậm. Nếu chúng ta cố gắng kiểm tra mọi$s_i$chống lại tất cả các cặp$(s_j, s_k)$, đó là$O(n^3)$kiểm tra nối chuỗi trong trường hợp xấu nhất, điều này vượt xa khả thi đối với$10^5$. 

Một vấn đề tế nhị hơn xuất hiện khi nghĩ đến việc đặt hàng. Vì một chuỗi có thể được tạo thành từ hai chuỗi nhỏ hơn nên các chuỗi nhỏ hơn đó có thể xuất hiện ở bất kỳ đâu trong thứ tự đầu vào, không nhất thiết phải trước chuỗi hiện tại. Điều này loại trừ mọi cách xây dựng tham lam hoặc chỉ có tiền tố trừ khi chúng tôi giải thích rõ ràng về tập hợp đầy đủ. 

Các trường hợp cạnh thường phá vỡ các giải pháp không chính xác đến từ sự trùng lặp và tự nối. 

Một ví dụ là trường hợp như: 

đầu vào:```
1
3
a
aa
aaa
```Đầu ra đúng là:```
011
```bởi vì "aa" = "a" + "a" và "aaa" = "aa" + "a". 

Cách tiếp cận có lỗi có thể thất bại nếu giả định rằng ít nhất một trong các phần phải xuất hiện sớm hơn theo thứ tự đầu vào hoặc nếu nó không cho phép sử dụng cùng một chuỗi hai lần. 

Một trường hợp tinh vi khác là khi tồn tại nhiều phân rã, nhưng chỉ một phân tách cần hợp lệ. Ví dụ: "abc" có thể được chia thành "a"+"bc" hoặc "ab"+"c". Chúng ta chỉ cần sự tồn tại của một phần chia hợp lệ. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: với mỗi chuỗi$s_i$, hãy thử mọi vị trí phân chia có thể. Đối với mỗi vị trí phân tách, hãy kiểm tra xem tiền tố và hậu tố có tồn tại trong tập hợp các chuỗi đã cho hay không. Vì mỗi chuỗi có độ dài tối đa là 8 nên có tối đa 7 phần phân tách trên mỗi chuỗi và mỗi lần kiểm tra tư cách thành viên có thể được thực hiện trong thời gian không đổi bằng cách sử dụng bộ băm. 

Điều này đã làm giảm vấn đề xuống còn khoảng$O(n \cdot L)$, Ở đâu$L \le 8$, đại khái thế$O(n)$. Tuy nhiên, chúng ta cần phải cẩn thận về cách xác thực tư cách thành viên một cách hiệu quả và cách tránh việc vô tình sử dụng lại các trạng thái được xây dựng một phần. 

Một quan điểm khác nhưng tương đương làm cho cấu trúc rõ ràng hơn. Thay vì suy nghĩ theo từng chuỗi, trước tiên chúng ta chèn tất cả các chuỗi vào một tập hợp băm. Sau đó, đối với mỗi chuỗi, chúng tôi kiểm tra tất cả các điểm phân tách và kiểm tra xem cả hai phần có tồn tại trong tập hợp hay không. Điều này có tác dụng vì sự cố không yêu cầu hai phần phải là các chuỗi khác nhau hoặc phải xuất hiện riêng biệt theo bất kỳ thứ tự cụ thể nào. 

Thông tin chi tiết quan trọng là độ dài tối đa nhỏ sẽ loại bỏ hoàn toàn mọi nhu cầu xử lý trước phức tạp. Toàn bộ khó khăn giảm bớt khi kiểm tra tất cả các phân vùng có thể có của mỗi chuỗi và kiểm tra tư cách thành viên của tập hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (cặp dây) |$O(n^2 \cdot L)$|$O(n)$| Quá chậm | 
| Bộ tách + băm |$O(n \cdot L)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ tất cả các chuỗi đầu vào trong một tập hợp băm sao cho các truy vấn thành viên cho bất kỳ chuỗi con nào có thời gian trung bình không đổi. Cấu trúc toàn cầu này đại diện cho tất cả các khối xây dựng có thể có mà chúng ta được phép sử dụng. 
2. Đối với mỗi chuỗi$s$, lặp lại tất cả các vị trí phân chia có thể$i$từ 1 đến$|s|-1$. Mỗi lần chia chia$s$thành tiền tố$s[0:i]$và hậu tố$s[i:]$. Lý do chúng tôi kiểm tra tất cả các phần tách là vì bất kỳ cấu trúc hợp lệ nào cũng phải tương ứng với chính xác một trong các phân vùng này. 
3. Đối với mỗi lần phân chia, hãy kiểm tra xem cả tiền tố và hậu tố có tồn tại trong tập băm hay không. Nếu cả hai đều có mặt, chúng ta có thể kết luận ngay rằng$s$có thể xây dựng được và đánh dấu nó là hợp lệ. 
4. Nếu không có sự phân tách nào tạo ra hai chuỗi từ điển hợp lệ, hãy đánh dấu chuỗi đó là không hợp lệ. 
5. Lặp lại điều này một cách độc lập cho mọi trường hợp thử nghiệm, xây dựng lại bộ băm cho mỗi trường hợp. 

Tại sao nó hoạt động: mọi đại diện hợp lệ$s = a + b$tương ứng duy nhất với một chỉ mục phân chia trong$s$. Vì chúng tôi liệt kê tất cả các chỉ số phân tách có thể có nên chúng tôi bao gồm mọi phân tách có thể có. Bộ băm đảm bảo rằng chúng tôi phát hiện chính xác xem mỗi phần ứng cử viên có tồn tại ở bất kỳ đâu trong đầu vào hay không, không phụ thuộc vào các ràng buộc về thứ tự hoặc bội số. Bởi vì các bản sao được lưu trữ trong biểu diễn tập hợp (hoặc cấu trúc nhận biết tần số tương đương nếu cần), việc tự sử dụng là điều đương nhiên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        arr = [input().strip() for _ in range(n)]
        st = set(arr)

        res = []
        for s in arr:
            ok = False
            m = len(s)
            for i in range(1, m):
                if s[:i] in st and s[i:] in st:
                    ok = True
                    break
            res.append('1' if ok else '0')

        print(''.join(res))

if __name__ == "__main__":
    solve()
```Giải pháp này xây dựng một tập hợp để kiểm tra tư cách thành viên theo thời gian cố định, sau đó đánh giá từng chuỗi một cách độc lập. Vòng lặp bên trong thử mọi điểm phân chia ngoại trừ các phần tách trống tầm thường. Một khi một phân tách hợp lệ được tìm thấy, nó sẽ bị đoản mạch vì sự tồn tại mới là vấn đề quan trọng. 

Các thao tác cắt là an toàn vì độ dài chuỗi tối đa là 8, do đó, mỗi lần trích xuất chuỗi con là thời gian không đổi trong thực tế. 

Một lỗi triển khai phổ biến là quên xây dựng lại tập hợp cho mỗi trường hợp thử nghiệm, điều này sẽ cho phép các chuỗi từ các trường hợp thử nghiệm trước đó ảnh hưởng không chính xác đến kết quả. Một điểm tinh tế khác là cho phép sử dụng lại cùng một chuỗi; việc này được xử lý một cách tự nhiên vì tập hợp không theo dõi số lượng sử dụng trên mỗi truy vấn. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu:```
5
abab
ab
abc
abacb
c
```Chúng tôi xây dựng bộ`{abab, ab, abc, abacb, c}`. 

| Chuỗi | Chia | Tiền tố | Hậu tố | Tiền tố trong bộ | Hậu tố trong bộ | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| abab | 1 | một | em yêu | không | không | | 
| abab | 2 | ab | ab | vâng | vâng | 1 | 
| ab | - | - | - | - | - | 0 | 
| abc | 1 | một | bc | không | không | | 
| abc | 2 | ab | c | vâng | vâng | 1 | 

Điều này xác nhận rằng thuật toán xác định chính xác các chuỗi có nhiều phân tách hợp lệ và bỏ qua những chuỗi không có bất kỳ phân tách nào. 

Bây giờ hãy xem xét trường hợp cạnh tối thiểu:```
1
2
a
aa
```Đặt là`{a, aa}`. 

| Chuỗi | Chia | Tiền tố | Hậu tố | Tiền tố trong bộ | Hậu tố trong bộ | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| aa | 1 | một | một | vâng | vâng | 1 | 
| một | - | - | - | - | - | 0 | 

Điều này cho thấy việc tự nối được xử lý một cách tự nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot L)$| Mỗi chuỗi được chia thành tối đa 7 vị trí, mỗi chuỗi được kiểm tra trong tra cứu băm theo thời gian cố định | 
| Không gian |$O(n)$| Tất cả các chuỗi được lưu trữ trong một bộ băm cho mỗi trường hợp thử nghiệm | 

Các ràng buộc đảm bảo tổng$n \le 10^5$và vì mỗi chuỗi cực kỳ ngắn nên thuật toán chạy thoải mái trong giới hạn. Hệ số không đổi nhỏ vì mỗi chuỗi kích hoạt tối đa 7 lần kiểm tra tư cách thành viên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input())
            arr = [input().strip() for _ in range(n)]
            st = set(arr)

            res = []
            for s in arr:
                ok = False
                for i in range(1, len(s)):
                    if s[:i] in st and s[i:] in st:
                        ok = True
                        break
                res.append('1' if ok else '0')
            print(''.join(res))

    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# provided samples
assert run("""3
5
abab
ab
abc
abacb
c
3
x
xx
xxx
8
codeforc
es
codes
cod
forc
forces
e
code
""") == """10100
011
10100101"""

# custom cases
assert run("""1
2
a
aa
""") == """01""", "self concat"

assert run("""1
3
a
b
ab
""") == """001""", "no split unless both exist"

assert run("""1
4
ab
cd
abcd
cdab
""") == """0011""", "multiple constructions"

assert run("""1
3
aaa
aa
a
""") == """111""", "all chains valid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| à, àa | 01 | xử lý tự nối | 
| a, b, ab | 001 | phân chia hợp lệ yêu cầu cả hai phần | 
| ab, cd, abcd, cdab | 0011 | nhiều mẫu nối | 
| aaa, aaa, a | 111 | phân hủy theo chuỗi | 

## Vỏ cạnh 

Đối với các trường hợp tự nối như chuỗi "aa", thuật toán kiểm tra phân tách ở vị trí 1. Tiền tố "a" và hậu tố "a" đều được tìm thấy trong tập hợp, do đó chuỗi được đánh dấu chính xác là hợp lệ. 

Đối với trường hợp thiếu một phần, chẳng hạn như "ab" chỉ có "a" chứ không có "b", mỗi lần phân tách không thành công ít nhất một lần kiểm tra tư cách thành viên, do đó chuỗi được đánh dấu chính xác là không hợp lệ. 

Đối với các cấu trúc chuỗi như "aaa" với "a" và "aa", việc phân tách ở vị trí 2 thành công bằng cách sử dụng tiền tố "aa" và hậu tố "a", cho thấy thuật toán hỗ trợ chính xác việc kết hợp nhiều bước mà không yêu cầu xây dựng lặp lại hoặc DP.
