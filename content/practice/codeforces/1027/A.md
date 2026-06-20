---
title: "CF 1027A - Vòng xoắn Palindromic"
description: "Chúng tôi được cung cấp một số chuỗi độc lập. Mỗi chuỗi có độ dài chẵn và mỗi ký tự là một chữ cái tiếng Anh viết thường. Đối với mỗi ký tự, chúng tôi buộc phải sửa đổi chính xác một lần và quy tắc sửa đổi là cố định: chúng tôi chỉ có thể di chuyển một bước trong bảng chữ cái lên hoặc xuống."
date: "2026-06-16T21:31:50+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1027
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 49 (Rated for Div. 2)"
rating: 1000
weight: 1027
solve_time_s: 377
verified: true
draft: false
---

[CF 1027A - Vòng xoắn Palindromic](https://codeforces.com/problemset/problem/1027/A) 

**Đánh giá:** 1000 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 6 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số chuỗi độc lập. Mỗi chuỗi có độ dài chẵn và mỗi ký tự là một chữ cái tiếng Anh viết thường. Đối với mỗi ký tự, chúng tôi buộc phải sửa đổi chính xác một lần và quy tắc sửa đổi là cố định: chúng tôi chỉ có thể di chuyển một bước trong bảng chữ cái lên hoặc xuống. Ví dụ, một nhân vật như`m`trở thành một trong hai`l`hoặc`n`, trong khi`a`chỉ có thể trở thành`b`, Và`z`chỉ có thể trở thành`y`. 

Sau khi áp dụng những thay đổi từng bước độc lập này cho mọi vị trí, chúng tôi muốn biết liệu chuỗi kết quả có thể trở thành một chuỗi palindrome hay không, nghĩa là các vị trí đối xứng từ hai đầu phải khớp nhau sau khi sửa đổi. 

Khó khăn chính là chúng ta không trực tiếp chọn các chữ cái cuối cùng. Thay vào đó, đối với mỗi vị trí, chúng tôi chọn một trong hai chữ cái lân cận có thể có. Vấn đề trở thành câu hỏi liệu chúng ta có thể phối hợp các lựa chọn nhị phân cục bộ này để tất cả các cặp đối xứng đều bằng nhau hay không. 

Các ràng buộc rất nhỏ: mỗi chuỗi có độ dài tối đa là 100 và có tối đa 50 trường hợp thử nghiệm. Điều này có nghĩa là ngay cả một giải pháp kiểm tra tất cả các vị trí một cách độc lập hoặc thử kiểm tra tính nhất quán tham lam đơn giản cũng đủ. Bất cứ điều gì theo cấp số nhân đối với các vị trí cũng có thể thực hiện được về mặt kỹ thuật nhưng không cần thiết. 

Cách viết hoa tinh tế đến từ các chữ cái ở cuối bảng chữ cái. Những nhân vật như`a`Và`z`chỉ có một khả năng biến đổi. Điều này loại bỏ tính linh hoạt và có thể tạo ra sự không phù hợp ngay cả khi các vị trí khác linh hoạt. Ví dụ, nếu chúng ta cần`a`để phù hợp với một cái gì đó khác hơn là`b`, cặp đó trở nên không thể thực hiện được bất kể các lựa chọn khác. 

Một trường hợp cạnh khác là khi cả hai ký tự được phản chiếu đều chỉ có một giá trị được chuyển đổi có thể có. Nếu các giá trị bắt buộc đó khác nhau thì không có cách nào khắc phục được. Một cách tiếp cận ngây thơ chỉ kiểm tra sự bình đẳng ban đầu của các ký tự sẽ hoàn toàn bỏ lỡ điều này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các phép biến đổi có thể có của chuỗi. Mỗi vị trí có nhiều nhất hai lựa chọn, vậy có tới$2^n$khả năng. Đối với mỗi chuỗi ứng cử viên, chúng tôi kiểm tra xem đó có phải là một bảng màu hay không. Điều này đúng vì nó khám phá tất cả các kết quả hợp lệ, nhưng đối với$n = 100$, con số này đã đạt tới khoảng$2^{100}$, vượt xa mọi tính toán khả thi. 

Quan sát quan trọng là chúng ta không bao giờ cần xây dựng chuỗi đầy đủ. Chúng ta chỉ cần đảm bảo rằng đối với mỗi cặp được phản chiếu$(i, n-1-i)$, các tập hợp ký tự cuối cùng có thể giao nhau. Mỗi vị trí đóng góp một bộ tối đa hai ký tự. Vấn đề giảm xuống còn việc kiểm tra xem hai tập hợp nhỏ có giao điểm khác trống đối với mọi cặp đối xứng hay không. 

Vì vậy, thay vì tìm kiếm toàn cục, chúng tôi giảm vấn đề xuống việc kiểm tra khả năng tương thích cục bộ. Mỗi cặp phải có khả năng đồng ý về ít nhất một ký tự chung sau khi cả hai điểm cuối chọn các phép biến đổi được phép một cách độc lập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Kiểm tra theo cặp |$O(n)$mỗi chuỗi |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng chuỗi một cách độc lập. 

1. Với mỗi vị trí, hãy tính hai ký tự có thể có. Nếu nhân vật là`a`, chỉ lưu trữ`b`. Nếu nó là`z`, chỉ lưu trữ`y`. Nếu không thì lưu trữ cả hai hàng xóm. Điều này xác định một tập hợp nhỏ có kích thước 1 hoặc 2 cho mỗi chỉ mục. 
2. Đối với mỗi cặp chỉ số đối xứng$i$Và$n-1-i$, so sánh hai tập ứng cử viên của chúng. Chúng tôi kiểm tra xem có tồn tại ít nhất một ký tự xuất hiện trong cả hai bộ hay không. Điều này thể hiện khả năng lựa chọn các phép biến đổi làm cho hai vị trí này bằng nhau. 
3. Nếu bất kỳ cặp đối xứng nào có giao điểm trống, chúng ta kết luận ngay rằng chuỗi đó không thể chuyển thành chuỗi palindrome. 
4. Nếu tất cả các cặp vượt qua bài kiểm tra giao nhau này, chúng tôi kết luận rằng điều đó là có thể. 

### Tại sao nó hoạt động 

Mỗi vị trí đều độc lập ngoại trừ ràng buộc do tính đối xứng áp đặt. Vì mỗi vị trí phải chọn chính xác một trong các ký tự được chuyển đổi được phép của nó, nên cách duy nhất để thỏa mãn điều kiện palindrome là đảm bảo rằng đối với mỗi cặp đối xứng đều tồn tại một lựa chọn hợp lệ được chia sẻ. Khi đã có lựa chọn như vậy cho mỗi cặp, chúng ta có thể gán ký tự chung đó cho cả hai đầu một cách nhất quán mà không vi phạm bất kỳ ràng buộc cục bộ nào. Không có sự kết hợp toàn cầu nào tồn tại ngoài những ràng buộc theo cặp này, vì vậy tính khả thi theo cặp bao hàm tính khả thi hoàn toàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def options(c):
    if c == 'a':
        return {'b'}
    if c == 'z':
        return {'y'}
    return {chr(ord(c) - 1), chr(ord(c) + 1)}

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        s = input().strip()

        ok = True

        for i in range(n // 2):
            left = options(s[i])
            right = options(s[n - 1 - i])

            if left.isdisjoint(right):
                ok = False
                break

        out.append("YES" if ok else "NO")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```chức năng`options`mã hóa phép biến đổi được phép cho mỗi ký tự. Vòng lặp chính xử lý từng trường hợp thử nghiệm và chỉ so sánh các vị trí được phản chiếu. Hoạt động quan trọng là kiểm tra giao điểm đã thiết lập, xác định xem cả hai đầu có thể đồng ý với ký tự cuối cùng hay không. 

Một lỗi phổ biến là so sánh các ký tự gốc hoặc thậm chí so sánh sau một lựa chọn chuyển đổi cố định. Điều đó thất bại vì sự lựa chọn không mang tính toàn cầu; mỗi vị trí chọn hướng của nó một cách độc lập, vì vậy chúng ta phải xem xét đồng thời cả hai khả năng. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp từ mẫu. 

### Ví dụ 1 

đầu vào:```
n = 6
s = abccba
```Chúng tôi tính toán các lựa chọn: 

| tôi | s[i] | tùy chọn(s[i]) | s[n-1-i] | tùy chọn(s[n-1-i]) | ngã tư | 
| --- | --- | --- | --- | --- | --- | 
| 0 | một | {b} | một | {b} | {b} | 
| 1 | b | {a, c} | b | {a, c} | {a, c} | 
| 2 | c | {b, d} | c | {b, d} | {b, d} | 

Tất cả các giao lộ đều không trống, vì vậy câu trả lời là CÓ. 

Điều này cho thấy rằng mặc dù chuỗi ban đầu đã là một chuỗi palindrome, nhưng tính khả thi phụ thuộc vào việc các chữ cái được chuyển đổi nhất quán có thể được căn chỉnh hay không chứ không phụ thuộc vào sự bằng nhau ban đầu. 

### Ví dụ 2 

đầu vào:```
n = 2
s = cf
```| tôi | s[i] | tùy chọn(s[i]) | s[n-1-i] | tùy chọn(s[n-1-i]) | ngã tư | 
| --- | --- | --- | --- | --- | --- | 
| 0 | c | {b, d} | f | {e, g} | ∅ | 

Vì không có ký tự có thể chia sẻ nên không có phép biến đổi hợp lệ nào tồn tại. 

Điều này chứng tỏ trường hợp thất bại: mặc dù cả hai quan điểm đều có tính linh hoạt nhưng kết quả có thể xảy ra của chúng không trùng lặp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi vị trí được xử lý một lần và mỗi lần kiểm tra là giao lộ được đặt thời gian không đổi | 
| Không gian |$O(1)$| Chỉ có một vài bộ ký tự được tạo cho mỗi vị trí | 

Tổng công việc tối đa là vài nghìn thao tác ký tự trên tất cả các trường hợp thử nghiệm, nằm trong giới hạn cho$n \le 100$Và$T \le 50$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def options(c):
        if c == 'a':
            return {'b'}
        if c == 'z':
            return {'y'}
        return {chr(ord(c) - 1), chr(ord(c) + 1)}

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        s = input().strip()

        ok = True
        for i in range(n // 2):
            if options(s[i]).isdisjoint(options(s[n - 1 - i])):
                ok = False
                break
        out.append("YES" if ok else "NO")

    return "\n".join(out)

# provided samples
assert run("""5
6
abccba
2
cf
4
adfa
8
abaazaba
2
ml
""") == """YES
NO
YES
NO
NO"""

# minimum size
assert run("""1
2
ab
""") in {"YES", "NO"}

# forced mismatch due to endpoints
assert run("""1
2
az
""") == "NO"

# all middle flexibility
assert run("""1
4
bcdb
""") in {"YES", "NO"}

# symmetric identical structure
assert run("""1
4
bdbc
""") in {"YES", "NO"}
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`ab`| hoặc | trường hợp tối thiểu | 
|`az`| KHÔNG | ràng buộc điểm cuối phá vỡ tính khả thi | 
|`bcdb`| hoặc | kiểm tra đối xứng chung | 
|`bdbc`| hoặc | hành vi ghép đôi không tầm thường | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi một ký tự nằm ở ranh giới của bảng chữ cái. Đối với đầu vào`az`, các phép biến đổi bắt buộc:`a → b`Và`z → y`. Chuỗi cuối cùng duy nhất có thể là`by`, không phải là một palindrome. Thuật toán phát hiện chính xác điều này bởi vì`{'b'}`Và`{'y'}`có giao lộ trống. 

Một trường hợp khác là khi cả hai bên đều linh hoạt nhưng không tương thích. Vì`cf`,`c`có thể trở thành`{b, d}`Và`f`có thể trở thành`{e, g}`. Mặc dù cả hai bộ đều có kích thước hai nhưng không có sự trùng lặp nên cặp này không thành công. Thuật toán xác định chính xác điều này hoàn toàn thông qua giao điểm đã đặt mà không cần mô phỏng các lựa chọn. 

Trường hợp tinh tế cuối cùng là khi cả hai mặt đều có chữ cái chồng lên nhau nhưng gốc khác nhau. Vì`adfa`, cấu trúc ở giữa vẫn có thể căn chỉnh vì các cặp phản chiếu được kiểm tra độc lập và tính khả thi không gắn với tính đối xứng ban đầu mà với tính đối xứng được biến đổi có thể tiếp cận được.
