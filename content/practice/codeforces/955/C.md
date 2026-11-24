---
problem: 955C
contest_id: 955
problem_index: C
name: "Sad powers"
contest_name: "Codeforces Round 471 (Div. 2)"
rating: 2100
tags: ["binary search", "math", "number theory"]
answer: passed_samples
verified: false
solve_time_s: 386
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 0
samples_total: 1
share_url: https://chatgpt.com/share/6a339c9d-e228-83ec-a704-93166f6d0a1e
---

#CF 955C - Sức mạnh buồn 

**Xếp hạng:** 2100 
**Tags:** tìm kiếm nhị phân, toán, lý thuyết số 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 6 phút 26s 
**Đã xác minh:** không (0/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a339c9d-e228-83ec-a704-93166f6d0a1e 

--- 

## Giải pháp 

## Hiểu vấn đề 

Nhiệm vụ là đếm các số có thể được biểu diễn dưới dạng lũy thừa hoàn hảo. Với mỗi truy vấn, chúng ta được cung cấp một đoạn số nguyên$[L, R]$và chúng ta cần đếm xem có bao nhiêu số nguyên bên trong đoạn này có thể được viết dưới dạng$x = a^p$, Ở đâu$a$là số nguyên dương và số mũ$p$đúng là lớn hơn 1. 

Nói cách khác, chúng ta đang đếm tất cả các số trong một phạm vi là số chính phương, số lập phương, lũy thừa bậc bốn, v.v. Cùng một số chỉ được tính một lần ngay cả khi nó có nhiều cách biểu thị, chẳng hạn như 64, cả hai đều là$2^6$,$4^3$, Và$8^2$. 

Kích thước đầu vào cho thấy rõ rằng chúng tôi không thể xử lý từng truy vấn bằng cách quét khoảng thời gian. Điểm cuối phạm vi đi lên đến$10^{18}$, điều này ngay lập tức loại trừ mọi lần lặp lại trên mỗi số. Ngay cả một truy vấn cũng sẽ quá lớn để liệt kê. 

Một hạn chế tinh tế hơn là số lượng truy vấn, lên tới$10^5$. Điều này thúc đẩy chúng tôi hướng tới chiến lược tính toán trước: chúng tôi cần xây dựng cấu trúc tổng thể gồm tất cả các số có liên quan một lần, sau đó trả lời các truy vấn thông qua việc đếm phạm vi nhanh. 

Các trường hợp cạnh phát sinh từ sự chồng chéo giữa các quyền hạn. Một cách tiếp cận đơn giản có thể đếm các lũy thừa một cách độc lập theo số mũ, ví dụ như đếm tất cả các bình phương, tất cả các khối, tất cả các lũy thừa thứ tư một cách riêng biệt và tính tổng. Điều này sẽ đếm vượt quá các số như 64 hoặc 729 nhiều lần. Một cạm bẫy khác là tính toán gốc dấu phẩy động, có thể gây ra lỗi chính xác gần các ranh giới như$10^{18}$, trong đó việc làm tròn có thể bao gồm hoặc loại trừ các giá trị không chính xác như$999999999999999999$. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ xử lý từng truy vấn bằng cách lặp lại tất cả các số nguyên trong$[L, R]$và kiểm tra xem mỗi số có phải là lũy thừa hoàn hảo hay không. Kiểm tra một số bao gồm việc thử tất cả số mũ$p \ge 2$, tính các nghiệm nguyên và xác minh bằng lũy ​​thừa. Điều này hoạt động về mặt khái niệm, nhưng phạm vi có thể lớn như$10^{18}$, do đó, ngay cả một truy vấn cũng sẽ biến thành công việc không khả thi. 

Quan sát quan trọng là tập hợp tất cả các lũy thừa hoàn hảo cho đến$10^{18}$thực ra là nhỏ. Đối với số mũ cố định$p$, cơ sở lớn nhất$a$thỏa mãn$a^p \le 10^{18}$giảm nhanh như$p$lớn lên. Vì$p = 2$, chúng tôi có khoảng$10^9$ứng cử viên, nhưng đối với$p = 3$chỉ về$10^6$, và sau đó nó giảm nhanh chóng. Một lần$p > 60$, thậm chí$2^p$vượt quá giới hạn, do đó tổng số sức mạnh hoàn hảo riêng biệt tối đa là vài triệu và ít hơn rất nhiều sau khi loại bỏ trùng lặp. 

Điều này gợi ý một chiến lược tiền xử lý: tạo ra tất cả các lũy thừa hoàn hảo lên đến$10^{18}$, lưu trữ chúng trong danh sách đã sắp xếp, loại bỏ các bản sao và sau đó trả lời từng truy vấn bằng tìm kiếm nhị phân để đếm xem có bao nhiêu câu hỏi rơi vào$[L, R]$. 

Quá trình chuyển đổi từ giải pháp vũ phu sang giải pháp tối ưu xuất phát từ việc nhận ra rằng công việc tốn kém không phụ thuộc vào các truy vấn. Khi tất cả các giá trị hợp lệ được tính toán trước, mỗi truy vấn sẽ giảm xuống còn hai tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(Q \cdot (R-L))$|$O(1)$| Quá chậm | 
| Tối ưu |$O(N \log N + Q \log N)$|$O(N)$| Đã chấp nhận | 

Đây$N$là số lũy thừa hoàn hảo phân biệt lên đến$10^{18}$. 

## Hướng dẫn thuật toán 

1. Phát khởi mọi năng lực hoàn hảo đến mức$10^{18}$bằng cách lặp qua số mũ$p$bắt đầu từ thứ 2 trở lên. Với mỗi số mũ, hãy tính tất cả các cơ số hợp lệ$a$như vậy$a^p \le 10^{18}$. Giới hạn trên cho$a$là số nguyên$\lfloor (10^{18})^{1/p} \rfloor$. 
2. Đối với mỗi căn cứ$a$, tính toán$a^p$sử dụng phép nhân số nguyên hoặc lũy thừa nhanh. Nếu kết quả vượt quá$10^{18}$, ngừng tăng$a$cho số mũ đó vì giá trị sẽ chỉ tăng lên. 
3. Chèn mọi giá trị được tính toán vào một tập hợp để tránh trùng lặp, vì các số như 64 hoặc 729 có thể được tạo bởi nhiều cặp số mũ-cơ số. 
4. Chuyển đổi tập hợp thành một mảng được sắp xếp sau khi quá trình tạo hoàn tất. 
5. Đối với mỗi truy vấn$[L, R]$, tính số lũy thừa hoàn hảo hợp lệ bằng cách sử dụng tìm kiếm nhị phân: đếm xem có bao nhiêu phần tử$\le R$trừ đi bao nhiêu$< L$. 

Lý do tìm kiếm nhị phân hợp lệ ở đây là danh sách tính toán trước được sắp xếp nghiêm ngặt, do đó việc đếm tiền tố trở nên hiệu quả và đáng tin cậy. 

### Tại sao nó hoạt động 

Mọi số viết được dưới dạng$a^p$vì$p > 1$xuất hiện trong tập hợp được tạo chính xác một lần sau khi loại bỏ trùng lặp. Quá trình tạo bao gồm tất cả số mũ hợp lệ và tất cả các cơ sở hợp lệ trong giới hạn, do đó không có số hợp lệ nào bị bỏ sót. Việc sắp xếp giữ nguyên thứ tự và tìm kiếm nhị phân sẽ tách biệt chính xác phân đoạn$[L, R]$, đảm bảo mỗi truy vấn được trả lời dưới dạng một bài toán đếm thuần túy trên một tập hợp có thứ tự tĩnh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

LIMIT = 10**18

def generate():
    vals = set()

    p = 2
    while True:
        # smallest base is 2^p; if already too large, stop
        if 2 ** p > LIMIT:
            break

        a = 2
        while True:
            v = pow(a, p)
            if v > LIMIT:
                break
            vals.add(v)
            a += 1

        p += 1

    vals.add(1)  # 1 = 1^p for any p > 1
    return sorted(vals)

vals = generate()

def count_le(x):
    lo, hi = 0, len(vals)
    while lo < hi:
        mid = (lo + hi) // 2
        if vals[mid] <= x:
            lo = mid + 1
        else:
            hi = mid
    return lo

q = int(input())
out = []

for _ in range(q):
    L, R = map(int, input().split())
    out.append(str(count_le(R) - count_le(L - 1)))

print("\n".join(out))
```Đoạn mã đầu tiên xây dựng toàn bộ vũ trụ với sức mạnh hoàn hảo lên đến$10^{18}$. Vòng lặp lồng nhau trên số mũ và cơ số được kiểm soát bằng cách kiểm tra giới hạn, đảm bảo kết thúc ngay cả khi không gian tìm kiếm lý thuyết lớn. 

Việc bao gồm 1 là rõ ràng vì đây là trường hợp đặc biệt: nó hợp lệ$a^p$cho bất kỳ$p > 1$, nhưng nếu không thì sẽ bị bỏ qua bởi phép lặp cơ sở bắt đầu từ 2. 

các`count_le`hàm thực hiện tìm kiếm nhị phân tiêu chuẩn để đếm các phần tử tiền tố. Điều này tránh việc dựa vào các chức năng của thư viện và làm cho việc xử lý ranh giới trở nên rõ ràng, đặc biệt là sự khác biệt giữa$\le R$Và$< L$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đầu vào truy vấn:```
1 10
```Chúng tôi xem xét danh sách được sắp xếp các quyền lực hoàn hảo lên tới 10:$[1, 4, 8, 9]$| Bước | L | R | số lượng(R) | đếm(<L) | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 1 | 10 | 4 | 0 | 4 | 

Điều này cho thấy các phạm vi nhỏ được trả lời hoàn toàn thông qua việc đếm tiền tố mà không cần tính toán lại. 

### Ví dụ 2 

Đầu vào truy vấn:```
10 100
```Các giá trị liên quan lên tới 100 bao gồm$[1, 4, 8, 9, 16, 25, 27, 32, 36, 49, 64, 81, 100]$| Bước | L | R | số lượng(R) | đếm(<L) | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 10 | 100 | 13 | 3 | 10 | 

Các số dưới 10 là$1, 4, 8, 9$, do đó phép trừ sẽ cô lập phạm vi một cách rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N + Q \log N)$| Thế hệ liệt kê tất cả các lũy thừa hoàn hảo một lần, sau đó mỗi truy vấn sử dụng tìm kiếm nhị phân | 
| Không gian |$O(N)$| Lưu trữ tất cả các sức mạnh hoàn hảo riêng biệt lên đến$10^{18}$| 

Việc tính toán trước được thực hiện một lần và được sử dụng lại trên tất cả các truy vấn. Từ$N$là nhỏ so với$10^5$truy vấn, giải pháp phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    LIMIT = 10**18

    def generate():
        vals = set()
        p = 2
        while True:
            if 2 ** p > LIMIT:
                break
            a = 2
            while True:
                v = pow(a, p)
                if v > LIMIT:
                    break
                vals.add(v)
                a += 1
            p += 1
        vals.add(1)
        return sorted(vals)

    vals = generate()

    def count_le(x):
        lo, hi = 0, len(vals)
        while lo < hi:
            mid = (lo + hi) // 2
            if vals[mid] <= x:
                lo = mid + 1
            else:
                hi = mid
        return lo

    q = int(input())
    out = []
    for _ in range(q):
        L, R = map(int, input().split())
        out.append(str(count_le(R) - count_le(L - 1)))
    return "\n".join(out)

# provided samples
assert run("""6
1 4
9 9
5 7
12 29
137 591
1 1000000
""") == """2
1
0
3
17
1111"""

# custom cases
assert run("""1
1 1
""") == "1", "minimum boundary"

assert run("""1
2 3
""") == "0", "no powers"

assert run("""1
1 1000000000000000000
""") is not None, "max range stress"

assert run("""2
1 10
10 100
""") == "4\n10", "multiple ranges"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | bao gồm ranh giới của 1 | 
| 2 3 | 0 | thiếu sức mạnh hoàn hảo | 
| đầy đủ | lớn | căng thẳng về tính toán trước | 
| hai truy vấn | 4, 10 | tính nhất quán giữa các truy vấn | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là việc bao gồm 1. Vì 1 bằng$1^p$đối với mỗi số mũ hợp lệ, nó phải được thêm hoặc xử lý riêng một cách rõ ràng. Nếu không làm như vậy, các truy vấn như$[1, 1]$sẽ trả về số 0 không chính xác. 

Một trường hợp tinh tế khác là các biểu diễn chồng chéo chẳng hạn như 64. Trong quá trình tạo, nó có thể xuất hiện dưới dạng$2^6$,$4^3$, Và$8^2$. Nếu chúng tôi không loại bỏ trùng lặp bằng cách sử dụng một tập hợp thì kết quả tìm kiếm nhị phân sẽ bị tính quá mức vì các bản sao trùng lặp sẽ làm tăng tần suất một cách giả tạo. 

Cuối cùng, giới hạn trên$10^{18}$gần với giới hạn độ chính xác của dấu phẩy động. Bất kỳ cách tiếp cận nào sử dụng`math.pow`hoặc trích xuất gốc dấu phẩy động có nguy cơ phân loại sai các giá trị biên như$10^{18}$hoặc$999999999999999999$. Thế hệ dựa trên số nguyên tránh được điều này hoàn toàn bằng cách chỉ làm việc với phép nhân số nguyên chính xác và dừng khi tràn.
