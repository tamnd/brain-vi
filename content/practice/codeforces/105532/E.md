---
title: "CF 105532E - Droid Foundry A (Phiên bản dễ dàng)"
description: "Nhiệm vụ xoay quanh việc so sánh một chuỗi tham chiếu cố định với nhiều chuỗi ứng cử viên và kiểm tra xem liệu có thể lấy được từng chuỗi ứng cử viên hay không bằng cách xóa một số ký tự khỏi tham chiếu mà không sắp xếp lại các ký tự còn lại."
date: "2026-06-27T01:03:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105532
codeforces_index: "E"
codeforces_contest_name: "Aggie Competitive Programming Contest (ACPC) 2024"
rating: 0
weight: 105532
solve_time_s: 49
verified: true
draft: false
---

[CF 105532E - Droid Foundry A (Phiên bản dễ dàng)](https://codeforces.com/problemset/problem/105532/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ xoay quanh việc so sánh một chuỗi tham chiếu cố định với nhiều chuỗi ứng cử viên và kiểm tra xem liệu có thể lấy được từng chuỗi ứng cử viên hay không bằng cách xóa một số ký tự khỏi tham chiếu mà không sắp xếp lại các ký tự còn lại. Nói cách khác, đối với mỗi chuỗi truy vấn, chúng ta cần xác định xem nó có xuất hiện dưới dạng một chuỗi con của chuỗi cơ sở hay không. 

Chuỗi cơ sở đại diện cho một chuỗi ký tự do hệ thống tạo ra, trong khi mỗi chuỗi thiết kế đại diện cho một cấu hình mong muốn. Chúng tôi được phép xóa các ký tự khỏi đường cơ sở, nhưng chúng tôi không thể sắp xếp lại chúng hoặc chèn ký tự mới. Đầu ra của mỗi thiết kế là một quyết định đơn giản có hoặc không tùy thuộc vào việc liệu thiết kế có thể được nhúng vào đường cơ sở theo kiểu duy trì trật tự hay không. 

Gọi độ dài đường cơ sở là$n$và giả sử có$q$các chuỗi thiết kế, mỗi chuỗi có tổng chiều dài tối đa$n$. Quét hai con trỏ trực tiếp cho mỗi truy vấn tốn thời gian tuyến tính trong đường cơ sở, vì vậy giải pháp đơn giản là gần như$O(q \cdot n)$. Nếu cả hai$q$Và$n$lớn, điều này nhanh chóng trở nên quá chậm, đặc biệt khi cả hai có thể tiếp cận$10^5$, vì trường hợp xấu nhất sẽ cần khoảng$10^{10}$so sánh nhân vật. 

Ràng buộc mà mỗi thiết kế chỉ dài bằng đường cơ sở sẽ loại trừ các trường hợp chúng tôi cố gắng ghép các chuỗi dài hơn thành các chuỗi ngắn hơn, nhưng nó không ngăn ngừa các trường hợp bệnh lý trong đó mọi thiết kế đều dài gần bằng đường cơ sở. Trong những trường hợp như vậy, việc quét toàn bộ đường cơ sở lặp đi lặp lại sẽ trở thành nút thắt cổ chai. 

Trường hợp cạnh tinh tế xuất hiện khi đường cơ sở chứa các ký tự lặp lại và thiết kế cũng chứa các ký tự lặp lại. Ví dụ, nếu đường cơ sở là`aaaaabaaaaa`và thiết kế là`aaaaaa`, một trình so khớp tham lam bất cẩn có thể quét liên tục từ đầu cho mỗi kết quả khớp và vô tình chuyển sang hành vi bậc hai ngay cả khi logic khớp riêng lẻ là chính xác. 

Một vấn đề khác phát sinh nếu người ta cố gắng tính toán trước chỉ vị trí của các ký tự mà không sắp xếp cẩn thận. Chẳng hạn, việc chỉ lưu trữ tần số ký tự sẽ thất bại ngay lập tức: đường cơ sở`abc`và thiết kế`cba`có tần số giống nhau nhưng thiết kế không phải là một dãy con hợp lệ. 

## Phương pháp tiếp cận 

Phương pháp Brute Force kiểm tra từng thiết kế một cách độc lập bằng cách dùng con trỏ duyệt qua đường cơ sở và tiến lên bất cứ khi nào các ký tự khớp với nhau. Điều này đúng vì nó trực tiếp áp dụng định nghĩa dãy con. Tuy nhiên, việc lặp lại quá trình quét này cho mọi thiết kế sẽ dẫn đến việc duyệt lặp lại cùng một chuỗi cơ sở, điều này sẽ chi phối thời gian chạy khi số lượng truy vấn lớn. 

Quan sát quan trọng là chúng ta liên tục giải quyết cùng một vấn đề về cấu trúc: khớp một chuỗi ngắn với một chuỗi dài cố định. Thay vì tính toán lại các kết quả khớp từ đầu, chúng tôi có thể xử lý trước đường cơ sở để có thể nhanh chóng chuyển sang lần xuất hiện tiếp theo của mỗi ký tự. Khi chúng tôi biết, đối với mọi vị trí và ký tự, nơi xuất hiện tiếp theo, chúng tôi có thể chuyển qua từng thiết kế theo thời gian logarit hoặc không đổi trên mỗi bước tùy thuộc vào việc triển khai. 

Điều này chuyển đổi từng bước so khớp từ quét tuyến tính thành bước nhảy trực tiếp, loại bỏ công việc dư thừa đối với các truy vấn lặp lại. Cấu trúc của bài toán không yêu cầu sự tương tác giữa các truy vấn khác nhau, do đó việc xử lý trước là đủ để tách chúng ra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (hai con trỏ cho mỗi truy vấn) |$O(q \cdot n)$|$O(1)$| Quá chậm | 
| Tiền xử lý lần xuất hiện tiếp theo | (O(n \cdot \sigma + \sum | d_i | )) | 

Đây$\sigma$là kích thước bảng chữ cái. 

## Hướng dẫn thuật toán 

Chúng tôi xử lý trước đường cơ sở để có thể trả lời ngay lập tức “tôi sẽ đi đâu tiếp theo nếu tôi cần ký tự c từ vị trí i”. 

1. Chúng tôi xây dựng một bảng vị trí tiếp theo sao cho đối với mọi chỉ mục trong đường cơ sở và mọi ký tự, chúng tôi lưu trữ lần xuất hiện tiếp theo của ký tự đó tại hoặc sau chỉ mục đó. Điều này được xây dựng bằng cách quét chuỗi từ phải sang trái trong khi vẫn duy trì vị trí nhìn thấy mới nhất của mỗi ký tự. Hướng lùi này đảm bảo rằng khi chúng ta ở vị trí thứ i, chúng ta đã biết câu trả lời cho tất cả các vị trí sau đó. 
2. Đối với mỗi chuỗi thiết kế, chúng tôi mô phỏng việc so khớp với đường cơ sở bằng cách sử dụng một con trỏ được đặt ban đầu trước ký tự đầu tiên của đường cơ sở. 
3. Đối với mỗi ký tự trong thiết kế, chúng tôi chuyển con trỏ bằng bảng được tính toán trước đến vị trí hợp lệ tiếp theo nơi ký tự đó xuất hiện. Nếu không có vị trí như vậy tồn tại, chúng tôi kết luận ngay rằng thiết kế không thể hình thành được. 
4. Nếu chúng tôi xử lý thành công tất cả các ký tự của thiết kế, nó được xác nhận là một chuỗi con hợp lệ. 

Lý do sử dụng bảng tiếp theo thay vì quét tiếp là vì nó giúp loại bỏ việc duyệt lặp lại cùng một hậu tố của đường cơ sở. Mỗi bước trở thành một tra cứu trực tiếp. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, con trỏ luôn biểu thị vị trí sớm nhất có thể có trong đường cơ sở nơi tiền tố của thiết kế được khớp. Bảng lần xuất hiện tiếp theo đảm bảo rằng nếu tồn tại bất kỳ sự tiếp tục hợp lệ nào của trận đấu, chúng ta sẽ đạt được nó vì chúng ta luôn nhảy đến vị trí khả thi sớm nhất cho ký tự tiếp theo. Điều này bảo tồn bất biến tham lam không bao giờ bỏ qua phần nhúng hợp lệ có thể có trong khi đảm bảo không có ký tự nào được kiểm tra nhiều lần cho mỗi truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_next(s):
    n = len(s)
    nxt = [[n] * 26 for _ in range(n + 1)]
    
    last = [n] * 26
    for i in range(n - 1, -1, -1):
        last[ord(s[i]) - 97] = i
        for c in range(26):
            nxt[i][c] = last[c]
    return nxt

def solve():
    b = input().strip()
    n = len(b)
    nxt = build_next(b)
    
    q = int(input())
    out = []
    
    for _ in range(q):
        d = input().strip()
        pos = 0
        ok = True
        
        for ch in d:
            c = ord(ch) - 97
            if pos >= n or nxt[pos][c] == n:
                ok = False
                break
            pos = nxt[pos][c] + 1
        
        out.append("YES" if ok else "NO")
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Cốt lõi của việc thực hiện là`build_next`chức năng. Nó xây dựng một bảng trong đó`nxt[i][c]`đưa ra chỉ số sớm nhất vào hoặc sau`i`nhân vật ở đâu`c`xuất hiện. Đây là điều cho phép nhảy liên tục trong quá trình khớp. 

Trong quá trình xử lý truy vấn,`pos`theo dõi xem chúng tôi đã khớp đến mức nào trong đường cơ sở. Với mỗi ký tự trong thiết kế, chúng ta di chuyển`pos`đến lần xuất hiện hợp lệ tiếp theo cộng với một, vì lần tìm kiếm tiếp theo phải bắt đầu đúng sau vị trí phù hợp. Khi thiếu một ký tự bắt buộc trong hậu tố còn lại, chúng tôi sẽ kết thúc sớm. 

Một cạm bẫy phổ biến là quên di chuyển`pos`chuyển tiếp sau khi khớp một ký tự. Nếu không có`+1`, thuật toán có thể liên tục khớp với cùng một vị trí, chấp nhận sai các trường hợp không hợp lệ hoặc nhập các vòng lặp không chính xác. 

## Ví dụ đã hoạt động 

Xem xét một đường cơ sở`abac`với hai thiết kế`ac`Và`ca`. 

Đối với thiết kế đầu tiên`ac`: 

| Bước | Nhân vật | tư thế trước | tra cứu nxt | tư thế sau | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | một | 0 | 0 | 1 | khớp | 
| 2 | c | 1 | 3 | 4 | khớp | 

Thiết kế được chấp nhận vì cả hai ký tự đều có thể được tìm thấy theo thứ tự. 

Đối với thiết kế thứ hai`ca`: 

| Bước | Nhân vật | tư thế trước | tra cứu nxt | tư thế sau | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | c | 0 | 3 | 4 | khớp | 
| 2 | một | 4 | không | - | thất bại | 

Bước thứ hai thất bại vì sau khi sử dụng`c`, không có`a`xuất hiện muộn hơn trong đường cơ sở. 

Những dấu vết này cho thấy con trỏ chỉ di chuyển về phía trước và không bao giờ quay lại các vị trí trước đó, điều này đảm bảo thứ tự chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n \cdot \sigma + \sum | d_i | 
| Không gian |$O(n \cdot \sigma)$| lưu trữ lần xuất hiện tiếp theo cho từng vị trí và ký tự | 

Chi phí tiền xử lý là tuyến tính theo kích thước cơ sở nhân với kích thước bảng chữ cái, có thể chấp nhận được trong các ràng buộc điển hình khi bảng chữ cái được cố định (ví dụ: chữ cái tiếng Anh viết thường). Sau đó, mỗi truy vấn sẽ chạy theo thời gian chỉ tỷ lệ thuận với độ dài của chính nó, giúp giải pháp có thể mở rộng ngay cả đối với các lô thiết kế lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()  # placeholder, replace with solve() in real use

# These are structural tests, assuming solve() is properly wired.

def solve_wrapper(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from io import StringIO
    _out = StringIO()
    _sys.stdout = _out
    solve()
    _sys.stdout = sys.__stdout__
    return _out.getvalue().strip()

# sample-like test
assert solve_wrapper("abac\n2\nac\nca\n") == "YES\nNO"

# single character baseline
assert solve_wrapper("a\n3\na\nb\naa\n") == "YES\nNO\nNO"

# repeated characters
assert solve_wrapper("aaaa\n2\naa\naaa\n") == "YES\nYES"

# alternating pattern
assert solve_wrapper("ababab\n3\nbbb\naaa\nabab\n") == "NO\nNO\nYES"

# edge: empty design always valid
assert solve_wrapper("abc\n1\n\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| abac, ac/ca | CÓ / KHÔNG | tính đúng đắn của dãy con cơ bản | 
| a, a/b/aa | CÓ / KHÔNG / KHÔNG | xử lý cạnh một ký tự | 
| aaa, aaa/aaa | CÓ / CÓ | khớp ký tự lặp đi lặp lại | 
| ababab, bbb/aaa/abab | KHÔNG/KHÔNG/CÓ | hạn chế đặt hàng | 
| thiết kế trống | CÓ | trường hợp dãy số tầm thường | 

## Vỏ cạnh 

Trường hợp một cạnh là khi thiết kế trống. Vì một chuỗi trống luôn là một chuỗi con nên thuật toán sẽ thành công ngay lập tức mà không cần phải duyệt qua vì không có ký tự nào để xử lý. 

Một trường hợp khác là khi đường cơ sở rất ngắn nhưng thiết kế lại dài. Ví dụ, đường cơ sở`ab`và thiết kế`aba`. Trong quá trình tra cứu ký tự cuối cùng, bảng lần xuất hiện tiếp theo trả về giá trị trọng điểm cho biết không tồn tại vị trí hợp lệ nào và thuật toán sẽ từ chối thiết kế một cách chính xác mà không cần quét thêm. 

Trường hợp thứ ba là khi các ký tự lặp lại nhiều ở đường cơ sở. Đối với đường cơ sở`aaaaaa`và thiết kế`aaaaa`, con trỏ tiến lên một cách xác định thông qua các vị trí liên tiếp 0, 1, 2, 3, 4, 5, đảm bảo không quay lui hoặc mơ hồ.
