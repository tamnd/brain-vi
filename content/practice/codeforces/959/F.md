---
problem: 959F
contest_id: 959
problem_index: F
name: "Mahmoud and Ehab and yet another xor task"
contest_name: "Codeforces Round 473 (Div. 2)"
rating: 2400
tags: ["bitmasks", "dp", "math", "matrices"]
answer: passed_samples
verified: true
solve_time_s: 71
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a339f5b-7da0-83ec-9af1-078177c4b240
---

# CF 959F - Mahmoud và Ehab và một nhiệm vụ xor khác 

**Đánh giá:** 2400 
**Tags:** bitmasks, dp, toán học, ma trận 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 11s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a339f5b-7da0-83ec-9af1-078177c4b240 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên trong đó mỗi giá trị được coi là số 20 bit. Đối với mọi truy vấn, chúng tôi chỉ xem xét tiền tố của mảng, cụ thể là tiền tố đầu tiên$l$các phần tử và chúng ta phải đếm xem có bao nhiêu chuỗi con của tiền tố đó có XOR bằng giá trị đích$x$. Dãy con ở đây có nghĩa là chúng ta có thể lấy hoặc bỏ qua từng phần tử một cách độc lập, do đó mọi tập hợp con chỉ mục trong tiền tố đều hợp lệ. 

Đầu ra khóa không phải là một chuỗi con duy nhất mà là số lượng các chuỗi con khác nhau có XOR của các phần tử được chọn bằng giá trị truy vấn, được lấy theo modulo$10^9 + 7$. 

Các ràng buộc ngay lập tức loại trừ việc liệt kê các chuỗi con. Mảng và số lượng truy vấn đều lên tới$10^5$và mỗi phần tử đóng góp vào một không gian trạng thái có kích thước$2^{20}$bởi vì giá trị XOR nằm trong$[0, 2^{20})$. Bất kỳ giải pháp nào xử lý từng truy vấn bằng cách tính toán lại tập hợp con XOR hoặc thậm chí duy trì DP rõ ràng cho mỗi tiền tố một cách độc lập sẽ vượt quá giới hạn thời gian theo nhiều bậc độ lớn. 

Trường hợp cạnh tinh tế là dãy con trống. XOR của nó được định nghĩa là 0, vì vậy bất kỳ truy vấn nào yêu cầu$x = 0$phải bao gồm nó nếu chúng ta xem xét tiền tố, ngay cả khi không có phần tử nào được chọn. 

Một trường hợp góc quan trọng khác là các giá trị lặp lại hoặc số không. Ví dụ: nếu mảng chứa nhiều số 0, mỗi số 0 sẽ nhân đôi số lượng chuỗi con cho bất kỳ mục tiêu XOR cố định nào, nhưng chỉ theo cách có cấu trúc phải được truyền bá nhất quán thông qua chuyển tiếp DP. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ liệt kê tất cả các dãy con của dãy đầu tiên$l$các phần tử cho mỗi truy vấn. Đối với tiền tố có độ dài cố định$l$, đây là$2^l$và trên các truy vấn, điều này trở nên không thể thực hiện được vì$l$có thể$10^5$. Ngay cả việc tính toán DP độc lập cho mỗi truy vấn cũng sẽ tốn kém$O(q \cdot l \cdot 2^{20})$, điều này vượt xa khả năng thực hiện được. 

Thông tin chi tiết tiêu chuẩn cho các bài toán đếm tập hợp con XOR là tập hợp con XOR hoạt động giống như các tổ hợp tuyến tính trên một không gian vectơ trên GF(2). Mỗi phần tử mới không làm thay đổi không gian của các XOR có thể truy cập hoặc tăng kích thước lên một nếu nó độc lập tuyến tính với cơ sở hiện tại. 

Thay vì theo dõi trực tiếp tất cả số lượng tập hợp con, chúng tôi duy trì cơ sở tuyến tính cho các giá trị XOR cùng với hệ số nhân tổng thể biểu thị số lượng tập hợp con khác nhau thu gọn thành cùng một giá trị XOR do các phần tử phụ thuộc. 

Thực tế cấu trúc quan trọng là nếu cơ sở hiện tại có kích thước$k$, thì trong số những người đầu tiên$i$chính xác là các phần tử$2^{i-k}$các tập hợp con tương ứng với từng giá trị XOR có thể truy cập được trong phạm vi cơ sở. Tính đồng nhất này giúp cho việc đếm có thể thực hiện được: mọi giá trị XOR trong khoảng đều có cùng số lượng tập hợp con được tạo ra. 

Đối với các truy vấn, chúng tôi giảm XOR mục tiêu$x$sử dụng cơ sở. Nếu nó không thể biểu diễn được thì câu trả lời là không. Nếu nó có thể biểu diễn được thì câu trả lời là chính xác$2^{l-k}$, Ở đâu$k$là số vectơ cơ sở trong số các vectơ đầu tiên$l$các phần tử. 

Để hỗ trợ các truy vấn tiền tố một cách hiệu quả, chúng tôi xây dựng cơ sở tăng dần và lưu trữ ảnh chụp nhanh về kích thước và cấu trúc của nó tại mỗi vị trí tiền tố. Mỗi truy vấn sau đó được trả lời trong$O(20)$bằng cách giảm$x$dựa trên cơ sở được lưu trữ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(q \cdot 2^l)$|$O(1)$| Quá chậm | 
| Tối ưu |$O((n+q) \cdot 20)$|$O(n \cdot 20)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải trong khi vẫn duy trì cơ sở XOR tuyến tính. 

1. Khởi tạo một mảng có kích thước 20 biểu thị các vectơ cơ sở, tất cả được đặt bằng 0. Cũng tính toán trước lũy thừa của hai modulo$10^9+7$. 
2. Đối với mỗi phần tử$a_i$, chèn nó vào cơ sở XOR bằng cách sử dụng loại bỏ bit cao nhất tiêu chuẩn. Nếu nó trở về 0 sau khi giảm, nó phụ thuộc và không tăng kích thước cơ sở. 
3. Duy trì số lượng xếp hạng cơ bản$k_i$sau khi xử lý từng tiền tố$i$. Đồng thời lưu trữ ảnh chụp nhanh cơ sở hoặc ít nhất là đủ cấu trúc để trả lời các mức giảm cho tiền tố đó. 
4. Đối với một truy vấn$(l, x)$, lấy cơ sở tương ứng với tiền tố$l$và cố gắng giảm$x$bằng cách loại bỏ các bit cao nhất bằng cách sử dụng các vectơ cơ sở được lưu trữ. 
5. Nếu sau khi giảm$x$trở thành số 0, câu trả lời là$2^{l - k_l}$. Nếu không, câu trả lời là số không. 

Lý do việc rút gọn có tác dụng là vì cơ sở biểu thị một tập hợp bao trùm tất cả các giá trị XOR có thể đạt được. Nếu một giá trị không thể giảm xuống 0, thì nó nằm ngoài khoảng và không có chuỗi con nào tạo ra nó. 

### Tại sao nó hoạt động 

Tập hợp tất cả các tập con XOR tạo thành một không gian vectơ trên GF(2). Cơ sở chúng tôi duy trì là cơ sở của không gian này. Mỗi tập hợp con tương ứng với một tổ hợp tuyến tính của các vectơ cơ sở cộng với các lựa chọn của các phần tử phụ thuộc, chúng chỉ nhân số lượng mà không thay đổi các giá trị XOR có thể biểu diễn. Bởi vì mỗi phần tử độc lập nhân đôi số lượng tập hợp con trong khi chỉ tăng thứ nguyên khi độc lập, nên số lượng tập hợp con trên mỗi XOR có thể biểu diễn sẽ trở nên đồng nhất và bằng nhau$2^{\text{free dimensions}}$. Bất biến này giữ ở mọi tiền tố, đảm bảo câu trả lời chính xác cho tất cả các truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7
MAXB = 20

def build_powers(n):
    pw = [1] * (n + 1)
    for i in range(1, n + 1):
        pw[i] = (pw[i - 1] * 2) % MOD
    return pw

def insert_basis(basis, x):
    for b in reversed(range(MAXB)):
        if not (x >> b) & 1:
            continue
        if basis[b]:
            x ^= basis[b]
        else:
            basis[b] = x
            return True
    return False

def reduce_x(basis, x):
    for b in reversed(range(MAXB)):
        if (x >> b) & 1:
            if basis[b]:
                x ^= basis[b]
    return x == 0

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    basis = [0] * MAXB
    prefix_basis = [None] * (n + 1)
    rank = 0

    prefix_basis[0] = ([0] * MAXB, 0)

    for i in range(1, n + 1):
        new_basis = basis[:]
        before_rank = sum(1 for x in new_basis if x)
        if insert_basis(new_basis, a[i - 1]):
            rank = before_rank + 1
        else:
            rank = before_rank
        basis = new_basis
        prefix_basis[i] = (basis[:], rank)

    pw = build_powers(n)

    out = []
    for _ in range(q):
        l, x = map(int, input().split())
        b, r = prefix_basis[l]
        if reduce_x(b, x):
            out.append(str(pw[l - r]))
        else:
            out.append("0")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng một cơ sở tuyến tính tiền tố, lưu trữ cho mỗi tiền tố cả vectơ cơ sở và thứ hạng của nó. Mỗi truy vấn truy xuất trạng thái được lưu trữ và giảm XOR đích dựa trên cơ sở đó. Nếu việc rút gọn thành công, câu trả lời là lũy thừa của hai được xác định bởi số lượng phần tử phụ thuộc tồn tại trong tiền tố. 

Một chi tiết triển khai tinh tế là sao chép cơ sở cho mỗi tiền tố. Điều này đảm bảo các truy vấn không can thiệp vào các bản cập nhật sau này. Bước giảm phải tiến hành từ bit cao đến bit thấp để phù hợp với cấu trúc cơ sở chuẩn. 

Việc tính toán thứ hạng phải phản ánh số lần chèn độc lập, không chỉ đơn giản là số lượng vectơ khác 0 trong mảng cơ sở, vì việc duy trì cơ sở có thể dịch chuyển vectơ giữa các vị trí. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng dấu vết đơn giản hóa với một mảng nhỏ. 

đầu vào:```
n = 4, q = 2
a = [1, 2, 3, 0]
queries:
(3, 3)
(4, 1)
```Chúng tôi theo dõi việc chèn cơ sở: 

| tôi | một [tôi] | Cơ sở sau khi chèn | xếp hạng | 
| --- | --- | --- | --- | 
| 1 | 1 | {1} | 1 | 
| 2 | 2 | {1,2} | 2 | 
| 3 | 3 | {1,2} (người phụ thuộc) | 2 | 
| 4 | 0 | {1,2} | 2 | 

Truy vấn (3,3): rút gọn 3 sử dụng cơ sở {1,2}. 3 = 1 xor 2, có thể rút gọn về 0, nên đáp án là$2^{3-2} = 2$. 

Truy vấn (4.1): 1 có thể biểu diễn nên câu trả lời là$2^{4-2} = 4$. 

Dấu vết này cho thấy các phần tử phụ thuộc không tăng thứ hạng nhưng vẫn tăng gấp đôi số lượng tập hợp con. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n+q)\cdot 20)$| Mỗi lần chèn và giảm XOR truy vấn chạm tối đa 20 bit | 
| Không gian |$O(n \cdot 20)$| Lưu trữ tiền tố của căn cứ | 

Sự phức tạp phù hợp thoải mái trong giới hạn vì$2 \cdot 10^5 \cdot 20$hoạt động nằm trong giới hạn điển hình. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    MAXB = 20
    def insert(basis, x):
        for b in reversed(range(MAXB)):
            if (x >> b) & 1:
                if basis[b]:
                    x ^= basis[b]
                else:
                    basis[b] = x
                    return True
        return False

    def reduce_x(basis, x):
        for b in reversed(range(MAXB)):
            if (x >> b) & 1:
                if basis[b]:
                    x ^= basis[b]
        return x == 0

    basis = [0] * MAXB
    pref = []
    rank = 0

    pref.append((basis[:], rank))
    for i in range(n):
        nb = basis[:]
        before = sum(1 for v in nb if v)
        if insert(nb, a[i]):
            rank = before + 1
        else:
            rank = before
        basis = nb
        pref.append((basis[:], rank))

    out = []
    for _ in range(q):
        l, x = map(int, input().split())
        b, r = pref[l]
        if reduce_x(b[:], x):
            out.append(str(pow(2, l - r, MOD)))
        else:
            out.append("0")

    return "\n".join(out)

# provided samples
assert run("""5 5
0 1 2 3 4
4 3
2 0
3 7
5 7
5 8
""") == """4
2
0
4
0"""

# custom cases
assert run("""1 3
0
1 0
1 1
1 0
""") == """2
0
2""", "single zero element"

assert run("""3 2
1 2 3
3 0
3 3
""") == """2
2""", "full basis small"

assert run("""4 2
0 0 0 0
4 0
4 1
""") == """16
0""", "all zeros"

assert run("""5 2
1 1 1 1 1
5 1
5 0
""") == """16
16""", "duplicates collapse"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử số 0 đơn | 2/0/2 | xử lý chuỗi con trống | 
| đầy đủ cơ sở nhỏ | 2/2 | truy vấn XOR có thể biểu diễn | 
| tất cả số không | 16/0 | tăng trưởng theo cấp số nhân với sự phụ thuộc | 
| trùng lặp sụp đổ | 16/16 | xếp hạng sự ổn định khi lặp lại | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi mảng chỉ chứa các số 0. Cơ sở vẫn trống đối với tất cả các tiền tố, vì vậy mọi tiền tố có độ dài$l$có$2^l$các chuỗi con có XOR bằng 0 và tất cả các truy vấn khác 0 đều trả về 0. Thuật toán xử lý việc này vì việc giảm không thành công đối với các giá trị khác 0 và thứ hạng vẫn bằng 0. 

Một trường hợp khác là khi tất cả các phần tử đều có giá trị khác 0 giống hệt nhau. Sau lần chèn đầu tiên, mọi phần tử tiếp theo đều phụ thuộc. Thứ hạng vẫn ở mức 1 và mọi tiền tố hoạt động như một hệ thống hai trạng thái: XOR bằng 0 hoặc giá trị đó, với số lượng chia tỷ lệ thành lũy thừa của hai dựa trên các phép chèn phụ thuộc. 

Trường hợp tinh tế cuối cùng là các truy vấn trong đó$x = 0$. Ngay cả khi không có vectơ cơ sở nào tồn tại, việc quy giản thành công ngay lập tức và câu trả lời trở thành$2^l$, đếm chính xác tất cả các tập con kể cả tập trống.
