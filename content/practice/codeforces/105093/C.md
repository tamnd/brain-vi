---
title: "CF 105093C - Bộ ngắt giao dịch"
description: "Chúng ta được cung cấp một tập hợp cố định tối đa 20 sai sót có thể xảy ra. Mỗi người nộp đơn chọn một số tập hợp con của những sai sót này và được cho điểm mong muốn."
date: "2026-06-27T20:49:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105093
codeforces_index: "C"
codeforces_contest_name: "2024 UP ACM Algolympics Final Round"
rating: 0
weight: 105093
solve_time_s: 53
verified: true
draft: false
---

[CF 105093C - Công cụ phá vỡ giao dịch](https://codeforces.com/problemset/problem/105093/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp cố định tối đa 20 sai sót có thể xảy ra. Mỗi người nộp đơn chọn một số tập hợp con của những sai sót này và được cho điểm mong muốn. Sau đó, người tìm kiếm sẽ cung cấp một danh sách các sai sót “phá vỡ thỏa thuận” và chúng ta phải tìm ra ứng viên mong muốn nhất không mắc bất kỳ sai sót nào bị cấm. 

Được diễn đạt lại theo cách thuật toán hơn, mỗi người nộp đơn là một tập hợp con của một tập hợp có kích thước f, được ghép nối với một giá trị. Mỗi truy vấn cung cấp một tập hợp con khác và chúng ta phải tìm giá trị tối đa trong số tất cả các tập hợp con tách rời khỏi tập hợp con truy vấn. 

Những hạn chế là điều khiến điều này trở nên thú vị. Với tối đa 200.000 người đăng ký và 200.000 truy vấn, việc quét tất cả người đăng ký theo mỗi truy vấn là không thể ngay lập tức. Ngay cả O(n) cho mỗi truy vấn cũng dẫn đến 40 tỷ lượt kiểm tra trong trường hợp xấu nhất. Hạn chế chính là f 20, ngụ ý rằng tất cả các tập hợp con có thể được biểu diễn dưới dạng mặt nạ bit từ 0 đến 2^f − 1, một khoảng trống đủ nhỏ để tính toán trước. 

Một cạm bẫy tinh vi đến từ cách diễn đạt các truy vấn. Danh sách "người phá vỡ thỏa thuận" là một điều kiện HOẶC: bất kỳ người nộp đơn nào có dù chỉ một trong những sai sót đó đều không hợp lệ. Vì vậy, chúng tôi không khớp các tập hợp chính xác mà loại trừ tất cả các mặt nạ giao nhau với mặt nạ truy vấn. 

Một sai lầm ngây thơ là hiểu điều này là tập hợp con khớp sai hướng, ví dụ như kiểm tra xem mặt nạ của người nộp đơn có phải là tập hợp con của mặt nạ truy vấn hay không. Điều đó sẽ đảo ngược logic và tạo ra những câu trả lời hoàn toàn sai. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp lưu trữ từng ứng viên dưới dạng một mặt nạ bit và đối với mỗi truy vấn, kiểm tra từng ứng viên để xem liệu mặt nạ của họ có giao nhau với mặt nạ truy vấn hay không. Điều này đúng nhưng chi phí O(nq), vượt xa khả năng thực hiện. 

Cấu trúc trở nên đơn giản hơn nhiều khi chúng ta lật ngược góc nhìn. Thay vì suy nghĩ theo khía cạnh “người nộp đơn này có xung đột với truy vấn hay không”, chúng tôi nghĩ theo khía cạnh “những mặt nạ nào được truy vấn cho phép hoàn toàn”. Nếu mặt nạ truy vấn là q thì những người đăng ký hợp lệ chính xác là những mặt nạ m sao cho m & q = 0. Điều này tương đương với việc nói m là mặt nạ con của phần bù của q trong f bit. 

Điều này biến mỗi truy vấn thành một vấn đề tổng hợp mặt nạ con cổ điển: đưa ra một mảng các giá trị tốt nhất được tính toán trước cho mỗi mặt nạ, chúng tôi muốn giá trị tối đa trên tất cả các mặt nạ con của một mặt nạ nhất định. Điều này có thể được giải quyết một cách hiệu quả bằng cách sử dụng kỹ thuật lập trình động tập hợp con tiêu chuẩn (SOS DP trên các mặt nạ con). 

Trước tiên, chúng tôi nén từng ứng viên vào một bitmask và lưu trữ mức độ mong muốn tối đa cho từng mặt nạ chính xác. Sau đó, chúng tôi truyền bá các giá trị này để mỗi mặt nạ lưu trữ giá trị tối đa trên tất cả các mặt nạ con của nó. Sau quá trình tiền xử lý này, mỗi truy vấn sẽ trở thành một tra cứu duy nhất trên mặt nạ bổ sung. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Mặt nạ bit + Mặt nạ con DP | O((n + 2^f)·f + q) | O(2^f) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm mọi người nộp đơn và truy vấn thành dạng bitmask trên f bit. 

1. Gán cho mỗi lỗ hổng một chỉ số từ 0 đến f − 1 và chuyển danh sách lỗ hổng của mỗi người nộp đơn thành một mặt nạ bit. Chúng tôi lưu trữ mức độ mong muốn tối đa cho từng mặt nạ chính xác trong một mảng. Bước này nén tất cả đầu vào có cấu trúc vào một không gian trạng thái có kích thước cố định. 
2. Xây dựng một mảng`best[mask]`có kích thước 2^f trong đó mỗi mục lưu trữ mức độ mong muốn tối đa trong số những người nộp đơn có bộ khuyết điểm chính xác là chiếc mặt nạ đó. 
3. Chuyển đổi nó thành DP “tối đa mặt nạ con”. Đối với mỗi mặt nạ, chúng tôi muốn`dp[mask] = max(best[s]) for all s ⊆ mask`. Chúng tôi tính toán điều này bằng cách sử dụng bit DP tiêu chuẩn trên các tập hợp con: đối với mỗi bit, chúng tôi truyền cực đại từ các mặt nạ con nhỏ hơn đến các mặt nạ con lớn hơn. 
4. Đối với mỗi truy vấn, hãy xây dựng bitmask q của nó. Tính toán mặt nạ bổ sung trong phạm vi f bit,`comp = (~q) & ((1 << f) - 1)`. 
5. Câu trả lời cho câu hỏi này chỉ đơn giản là`dp[comp]`, vì comp chứa chính xác các sai sót được phép và dp[comp] tổng hợp tất cả các mặt nạ ứng viên hợp lệ. 
6. Nếu dp[comp] trống (không có ứng viên nào đóng góp), hãy xuất chuỗi lỗi. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là sau khi tiền xử lý,`dp[x]`lưu trữ mức độ mong muốn tối đa trong số tất cả các ứng viên có tập hợp lỗ hổng là tập con của x. Bất kỳ ứng viên nào hợp lệ cho truy vấn q phải không trùng lặp với q, nghĩa là mặt nạ của nó là tập con của phần bù của q. Vì vậy, tất cả những người nộp đơn hợp lệ chính xác là những người được tính vào`dp[comp]`và không có ứng viên không hợp lệ nào có thể xuất hiện ở đó bởi vì bất kỳ ứng viên không hợp lệ nào cũng sẽ chứa một chút bên ngoài comp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    f = int(input().strip())
    flaws = input().split()
    idx = {flaws[i]: i for i in range(f)}

    n = int(input().strip())

    size = 1 << f
    best = [-1] * size

    for _ in range(n):
        a = int(input().strip())
        tokens = input().split()

        mask = 0
        i = 0
        # tokens: im flaw (and/sry chaining)
        while i < len(tokens):
            if tokens[i] == "im":
                i += 1
                continue
            if tokens[i] == "sry":
                break
            flaw = tokens[i]
            i += 1
            if i < len(tokens) and tokens[i] in ("and", "sry"):
                mask |= (1 << idx[flaw])
                if tokens[i] == "sry":
                    break
                i += 1
            else:
                mask |= (1 << idx[flaw])

        best[mask] = max(best[mask], a)

    dp = best[:]

    for i in range(f):
        for mask in range(size):
            if mask & (1 << i):
                dp[mask] = max(dp[mask], dp[mask ^ (1 << i)])

    q = int(input().strip())
    full = (1 << f) - 1

    out = []
    for _ in range(q):
        tokens = input().split()

        qmask = 0
        i = 0
        while i < len(tokens):
            if tokens[i] == "no":
                i += 1
                continue
            if tokens[i] == "pls":
                break
            flaw = tokens[i]
            i += 1
            if i < len(tokens) and tokens[i] in ("or", "pls"):
                qmask |= (1 << idx[flaw])
                if tokens[i] == "pls":
                    break
                i += 1
            else:
                qmask |= (1 << idx[flaw])

        comp = full ^ qmask
        ans = dp[comp]
        if ans == -1:
            out.append("LOWER YOUR STANDARDS")
        else:
            out.append(str(ans))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giai đoạn đầu tiên nén từng ứng viên vào một mặt nạ bit và chỉ lưu trữ giá trị tốt nhất cho mỗi cấu hình chính xác. Các va chạm được xử lý bằng cách lấy giá trị cực đại vì nhiều ứng viên có thể chia sẻ các tập sai sót giống hệt nhau. 

Giai đoạn thứ hai là mặt nạ con DP. Mỗi lần lặp qua các bit cho phép thông tin từ một mặt nạ thiếu một bit cụ thể được truyền vào các mặt nạ bao gồm nó, tạo nên bảng “tối đa tất cả các mặt nạ con” đầy đủ. 

Logic truy vấn dựa vào việc bổ sung tập hợp bị cấm và xử lý vấn đề như một truy vấn tập hợp con thuần túy. 

## Ví dụ đã hoạt động 

Chúng tôi minh họa bằng một ví dụ tùy chỉnh nhỏ. 

Giả sử f = 3 có các khuyết tật a, b, c. 

Ứng viên: 

mặt nạ 001 → 10 

mặt nạ 010 → 5 

mặt nạ 011 → 7 

Sau khi tiền xử lý: 

tốt nhất[001]=10, tốt nhất[010]=5, tốt nhất[011]=7 

Sau mặt nạ con DP: 

dp[000]=0 (không có người nộp đơn) 

dp[001]=10 

dp[010]=5 

dp[011]=10 

dp[111]=10 

Bây giờ truy vấn: “no a hoặc c” → qmask = 101 

comp = 010 

câu trả lời = dp[010] = 5 

| Sân khấu | Giá trị | 
| --- | --- | 
| qmask | 101 | 
| máy tính | 010 | 
| dp[comp] | 5 | 

Điều này cho thấy rằng chỉ những ứng viên có đầy đủ các tính năng được phép mới được xem xét. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + 2^f) · f + q) | mặt nạ xây dựng, SOS DP qua f bit, truy vấn liên tục | 
| Không gian | O(2^f) | mảng trên tất cả các tập hợp con | 

Hệ số mũ là an toàn vì f 20, tạo ra 2^f khoảng một triệu, có thể quản lý được cả về bộ nhớ và thời gian với DP bit tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    try:
        solve()
    except Exception as e:
        return str(e)

# Minimal case
assert run("""1
a
1
5
im a sry
1
no a pls
""").strip() == "LOWER YOUR STANDARDS"

# Simple valid selection
assert run("""2
a b
2
10
im a sry
5
im b sry
1
no a pls
""").strip() == "5"

# All compatible
assert run("""2
a b
2
10
im a sry
7
im b sry
1
no c pls
""").strip() == "10"

# No valid applicants
assert run("""2
a b
1
5
im a and b sry
1
no a or b pls
""").strip() == "LOWER YOUR STANDARDS"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| xung đột lỗ hổng duy nhất | HẠT TIÊU CHUẨN CỦA BẠN | trường hợp bổ sung trống | 
| lựa chọn đơn giản | 5 | truy xuất tối đa chính xác | 
| truy vấn không liên quan | 10 | chấp nhận hoàn toàn | 
| từ chối hoàn toàn | HẠT TIÊU CHUẨN CỦA BẠN | trường hợp chặn công đoàn | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một truy vấn cấm tất cả các sai sót. Trong trường hợp đó, mặt nạ bổ sung bằng 0 và chỉ những ứng viên không có sai sót mới hợp lệ. Nếu không tồn tại, dp[0] vẫn không hợp lệ và đầu ra phải là chuỗi lỗi. DP xử lý chính xác việc này vì dp[0] chỉ tổng hợp các mặt nạ trống chính xác. 

Một trường hợp đặc biệt khác là nhiều ứng viên có chung bộ lỗ hổng giống hệt nhau. Bước tiền xử lý chỉ lưu trữ mức mong muốn tối đa trên mỗi mặt nạ, do đó các bản sao không làm sai lệch kết quả và DP vẫn truyền mức tối đa chính xác lên trên thông qua các mối quan hệ mặt nạ con.
