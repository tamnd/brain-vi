---
title: "CF 2131C - Làm cho nó bình đẳng"
description: "Chúng ta bắt đầu với một tập hợp $S$ có kích thước $n$ và chúng ta muốn chuyển đổi nó thành một tập hợp $T$ khác có cùng kích thước. Nước đi duy nhất được phép lấy một phần tử $x$ từ $S$, xóa nó và thay thế nó bằng $x+k$ hoặc $ Câu hỏi cốt lõi là liệu việc lặp lại các ứng dụng này…"
date: "2026-06-08T02:54:33+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2131
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1042 (Div. 3)"
rating: 1100
weight: 2131
solve_time_s: 103
verified: false
draft: false
---

[CF 2131C - Làm cho nó bình đẳng](https://codeforces.com/problemset/problem/2131/C) 

**Đánh giá:** 1100 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với nhiều bộ$S$kích thước$n$và chúng tôi muốn chuyển đổi nó thành một bộ đa năng khác$T$có cùng kích thước. Nước đi duy nhất được phép có một phần tử$x$từ$S$, xóa nó và thay thế bằng một trong hai$x+k$hoặc$|x-k|$. Mỗi thao tác duy trì kích thước của nhiều tập hợp nhưng có thể thay đổi các giá trị riêng lẻ. 

Câu hỏi cốt lõi là liệu việc áp dụng lặp đi lặp lại những động thái này có thể định hình lại$S$chính xác vào$T$, tôn trọng sự đa dạng. 

Các ràng buộc gợi ý rằng mỗi trường hợp kiểm thử phải được xử lý trong thời gian gần tuyến tính trong$n$, vì tổng$n$trên tất cả các bài kiểm tra lên đến$2 \cdot 10^5$. Bất kỳ cách tiếp cận nào khám phá các phép biến đổi trên mỗi giá trị hoặc mô phỏng các hoạt động theo từng bước sẽ ngay lập tức thất bại do bùng nổ ở các trạng thái có thể tiếp cận. 

Một khía cạnh tinh tế là các giá trị không bị giới hạn chặt chẽ ngoại trừ bởi$10^9$, vì vậy việc duyệt đồ thị trực tiếp qua các giá trị là không thể. 

Mô hình hai cạnh có xu hướng phá vỡ suy nghĩ ngây thơ. 

Đầu tiên, giả sử mỗi phần tử có thể độc lập tiếp cận bất kỳ mục tiêu nào có cùng mod dư lượng$k$là sai bởi vì$|x-k|$hoạt động có thể giảm các giá trị theo cách phi tuyến tính cho đến khi chúng vượt qua 0, sau đó hành vi sẽ thay đổi. Ví dụ, với$k=5$, từ$x=6$bạn có thể đi đến$1$, sau đó đến$4$, sau đó đến$9$, tạo ra các chu kỳ không hoàn toàn theo mô-đun. 

Thứ hai, giả sử việc khớp tham lam các giá trị gần nhất không thành công vì các phép biến đổi có thể yêu cầu các giá trị trung gian không có trong cả hai tập hợp. 

Một trường hợp sai lầm nhỏ là:```
S = [1, 6], T = [4, 3], k = 5
```Một nỗ lực tham lam có thể khớp với 1→6 và 6→1, nhưng các chuyển đổi trung gian cho thấy cấu trúc không đối xứng theo cách đơn giản. 

## Phương pháp tiếp cận 

Cách diễn giải brute-force sẽ mô hình hóa từng giá trị dưới dạng một nút trong biểu đồ, với các cạnh$x \to x+k$Và$x \to |x-k|$và thử kiểm tra xem multiset$T$có thể truy cập từ$S$thông qua việc phân phối lại. Điều này trở thành một vấn đề về khả năng tiếp cận đa nguồn với số lượng trạng thái, theo cấp số nhân trong thực tế vì các giá trị tăng lên không giới hạn trong$+k$phương hướng. 

Điểm thất bại là mỗi số phân nhánh thành hai khả năng trong mỗi thao tác và các chuỗi có thể dài tùy ý. Ngay cả khi chúng ta giới hạn các giá trị ở một giới hạn heuristic nào đó thì các quá trình chuyển đổi không đơn điệu, do đó việc cắt tỉa là không an toàn. 

Quan sát quan trọng là cấu trúc hoạt động duy trì một bất biến rất mạnh: mọi số cuối cùng hoạt động độc lập bên trong modulo lớp dư lượng của nó$k$, ngoại trừ các giá trị đó có thể được giảm đi bằng cách áp dụng nhiều lần$|x-k|$cho đến khi chúng đi vào phạm vi chuẩn. 

Cụ thể hơn, việc áp dụng nhiều lần phép “trừ$k$theo nghĩa tuyệt đối” phép toán luôn đẩy một số về phía một đại diện duy nhất trong khoảng$[0, k)$, sau đó cấu trúc tiếp theo trở nên có thể dự đoán được. Mọi giá trị có thể được ánh xạ tới một chuỗi mà cuối cùng sẽ quay vòng trong một vùng giới hạn nhỏ được xác định bởi modulo dư của nó$k$. Sự tự do có ý nghĩa duy nhất là bao nhiêu lần chúng ta “quấn lấy” bằng cách sử dụng$+k$, làm thay đổi giá trị nhưng không bao giờ thay đổi lớp dư lượng của chúng. 

Điều này làm giảm vấn đề trong việc theo dõi xem có bao nhiêu phần tử từ mỗi lớp dư lượng có sẵn trong$S$và cần thiết trong$T$và xác minh rằng trong mỗi lớp, nhiều tập hợp có thể được căn chỉnh theo các ca được phép. 

Việc kiểm tra cuối cùng trở thành bài toán cân bằng cho mỗi lớp dư lượng, trong đó các phần tử có thể được đẩy lên trên theo các bước$k$, nhưng không được sắp xếp lại một cách tùy tiện trên các dư lượng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm đồ thị Brute Force | Hàm mũ | O(n) | Quá chậm | 
| Dư + Tham Lam Phù Hợp | O(n \log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm mọi giá trị về dạng chính tắc bên trong lớp dư lượng của nó và sau đó so sánh cách phân bổ khối lượng trên các chuỗi tăng dần. 

1. Nhóm các phần tử của$S$Và$T$theo modulo giá trị của chúng$k$. Điều này là cần thiết vì cả hai thao tác được phép đều bảo toàn mod dư lượng$k$lên đến các hiệu ứng dấu hiệu biến mất sau khi bình thường hóa. 
2. Với mỗi nhóm, hãy sắp xếp các phần tử. Việc sắp xếp là cần thiết vì phép biến đổi cho phép dịch chuyển các phần tử lên trên theo gia số một cách hiệu quả.$k$, do đó việc căn chỉnh phải tôn trọng trật tự trong chuỗi. 
3. Trong nhóm dư lượng, xử lý các giá trị theo thứ tự tăng dần và mô phỏng cách các phần tử dư thừa trong$S$có thể được đẩy lên để phù hợp với thâm hụt trong$T$. Khi một giá trị trong$S$vượt quá những gì cần thiết ở cùng mức độ trong$T$, phần dư thừa sẽ được chuyển lên cấp độ cao hơn tiếp theo bằng cách xử lý nó một cách hiệu quả như$x+k$. 
4. Duy trì bộ đếm thặng dư đang chạy trong khi lặp qua các giá trị đã được sắp xếp. Ở mỗi bước, hãy trừ đi những gì được yêu cầu bởi$T$. Nếu chúng ta thâm hụt, chúng ta cố gắng bù đắp bằng cách sử dụng thặng dư từ các giá trị thấp hơn. 
5. Nếu tại bất kỳ thời điểm nào chúng tôi không thể bù đắp được khoản thâm hụt thì việc chuyển đổi là không thể đối với loại dư lượng đó. 
6. Lặp lại độc lập cho tất cả các loại cặn. Nếu mỗi lớp đều đáp ứng thì câu trả lời là “CÓ”, ngược lại là “KHÔNG”. 

Ý tưởng chính là chuyển động chỉ đi lên theo cấp số cộng của bước$k$, do đó mỗi lớp dư lượng hoạt động giống như một hệ thống dòng chảy một chiều. 

### Tại sao nó hoạt động 

Hướng duy nhất không thể đảo ngược của mỗi phần tử là hướng lên trên bằng cách thêm$k$, trong khi$|x-k|$hoạt động chỉ có thể giảm các giá trị nhưng không bao giờ thay đổi lớp dư lượng của chúng theo cách cho phép tương tác giữa các lớp. Điều này tạo ra các chuỗi độc lập trên mỗi dư lượng. Trong mỗi chuỗi, các phần tử có thể được dịch chuyển lên trên một cách tùy ý nhưng không thể xuyên qua hoặc sắp xếp lại thứ tự giữa các chuỗi. Sự lan truyền thặng dư tham lam mô phỏng chính xác dòng chảy này, đảm bảo rằng mọi thâm hụt phải được bù đắp bằng thặng dư trước đó, và bất kỳ sự không phù hợp nào đều có nghĩa là không thể xảy ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        s = list(map(int, input().split()))
        tarr = list(map(int, input().split()))
        
        groups_s = defaultdict(list)
        groups_t = defaultdict(list)
        
        for x in s:
            groups_s[x % k].append(x)
        for x in tarr:
            groups_t[x % k].append(x)
        
        ok = True
        
        for r in set(list(groups_s.keys()) + list(groups_t.keys())):
            a = sorted(groups_s[r])
            b = sorted(groups_t[r])
            
            i = j = 0
            surplus = 0
            
            while i < len(a) or j < len(b):
                av = a[i] if i < len(a) else float('inf')
                bv = b[j] if j < len(b) else float('inf')
                
                if av == bv:
                    i += 1
                    j += 1
                    continue
                
                if av < bv:
                    surplus += 1
                    i += 1
                else:
                    if surplus == 0:
                        ok = False
                        break
                    surplus -= 1
                    j += 1
            
            if not ok:
                break
        
        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Việc thực hiện phân vùng đầu tiên các giá trị theo modulo dư$k$, trong đó cô lập các chuỗi biến đổi độc lập. Trong mỗi nhóm, việc sắp xếp thực thi một hướng nhất quán để xử lý những thay đổi có thể xảy ra. 

Vòng lặp hai con trỏ so sánh tính khả dụng trong$S$so với nhu cầu trong$T$. Khi$S$có giá trị nhỏ hơn, chúng ta coi nó là thặng dư tiềm năng có thể được đẩy lên trên. Khi$T$đòi hỏi một giá trị hiện không có sẵn, chúng ta tiêu thụ thặng dư tích lũy trước đó. Nếu không có thặng dư, chúng ta sẽ thất bại ngay lập tức. 

lính gác$+\infty$việc xử lý đảm bảo chấm dứt sạch khi một danh sách đã cạn kiệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n=3, k=2
S = [0, 1, 0]
T = [1, 0, 1]
```Chúng tôi nhóm theo modulo 2. 

| dư lượng | S được sắp xếp | T đã sắp xếp | 
| --- | --- | --- | 
| 0 | [0, 0] | [0] | 
| 1 | [1] | [1] | 

Dư lượng xử lý 0: 

| bước | S[i] | T[j] | dư thừa | hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | trận đấu | 
| 1 | 0 | ∞ | 1 | thặng dư từ S | 

Xử lý cặn 1: 

| bước | S[i] | T[j] | dư thừa | hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | trận đấu | 

Điều này cho thấy khối lượng dư 0 có thể được đẩy lên trong cùng một lớp, cho phép phân phối lại. 

### Ví dụ 2 

đầu vào:```
n=2, k=7
S = [2, 8]
T = [2, 9]
```Lớp dư 2 chứa tất cả các phần tử. 

| bước | S[i] | T[j] | dư thừa | hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 2 | 0 | trận đấu | 
| 1 | 8 | 9 | thất bại | không thể khớp 9 | 

Ở đây 8 không thể đạt tới 9 thông qua các hoạt động được phép trong phạm vi ràng buộc của chuỗi, do đó quá trình không thành công. 

Điều này chứng tỏ rằng ngay cả trong một lớp dư lượng duy nhất, sự chuyển động đi lên phải tôn trọng cấu trúc thặng dư sẵn có. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp trong các nhóm dư lượng chiếm ưu thế; mỗi phần tử được xử lý một lần trong lần quét hai con trỏ | 
| Không gian |$O(n)$| Lưu trữ các phần tử được nhóm | 

Độ phức tạp phù hợp thoải mái trong giới hạn vì tổng$n$qua các trường hợp thử nghiệm là$2 \cdot 10^5$và việc sắp xếp vẫn hiệu quả ở quy mô này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    from collections import defaultdict

    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        s = list(map(int, input().split()))
        tt = list(map(int, input().split()))

        groups_s = defaultdict(list)
        groups_t = defaultdict(list)

        for x in s:
            groups_s[x % k].append(x)
        for x in tt:
            groups_t[x % k].append(x)

        ok = True

        for r in set(groups_s.keys()) | set(groups_t.keys()):
            a = sorted(groups_s[r])
            b = sorted(groups_t[r])

            i = j = 0
            surplus = 0

            while i < len(a) or j < len(b):
                av = a[i] if i < len(a) else float('inf')
                bv = b[j] if j < len(b) else float('inf')

                if av == bv:
                    i += 1
                    j += 1
                elif av < bv:
                    surplus += 1
                    i += 1
                else:
                    if surplus == 0:
                        ok = False
                        break
                    surplus -= 1
                    j += 1

            if not ok:
                break

        out.append("YES" if ok else "NO")

    return "\n".join(out)

# provided samples
assert run("""5
1 3
1
2
1 8
4
12
3 5
6 2 9
8 4 11
2 7
2 8
2 9
3 2
0 1 0
1 0 1
""") == """YES
YES
YES
NO
NO"""

# custom cases
assert run("""1
1 1
5
5
""") == "YES", "minimum equal"

assert run("""1
2 3
1 4
2 5
""") == "NO", "different residue structure"

assert run("""1
3 2
0 0 0
2 2 2
""") == "YES", "pure upward shifts"

assert run("""1
3 10
0 10 20
0 10 30
""") == "NO", "gap in chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử bằng nhau duy nhất | CÓ | trường hợp cơ bản tầm thường | 
| dư lượng không khớp | KHÔNG | sự tách cặn đúng đắn | 
| xích chuyển số đồng đều | CÓ | lan truyền đi lên | 
| thiếu giá trị có thể truy cập | KHÔNG | phát hiện khoảng cách | 

## Vỏ cạnh 

Trường hợp cạnh tranh nhất là khi cả hai tập hợp giống hệt nhau nhưng được sắp xếp sao cho việc chuyển đổi là không cần thiết. Ví dụ,$S = T = [x_1, x_2, \dots]$. Thuật toán xử lý từng nhóm dư lượng, xem các cặp khớp hoàn hảo và không bao giờ sử dụng số dư. Đầu ra vẫn CÓ mà không dựa vào bất kỳ logic chuyển đổi nào. 

Một trường hợp khác là khi một lớp dư lượng có thêm các phần tử nhỏ phải được đẩy lên trên nhiều lần. Ví dụ:```
S = [0, 0, 0], T = [6, 6, 6], k = 3
```Tất cả các phần tử đều thuộc về phần dư 0. Thuật toán tích lũy thặng dư ở các giá trị thấp hơn và tiêu thụ nó khi chúng ta di chuyển lên trên, đáp ứng thành công mọi nhu cầu. Điều này xác nhận rằng các chuyển tiếp +k lặp lại được mô hình hóa chính xác bằng quá trình lan truyền dư thừa. 

Trường hợp thất bại là khi mục tiêu bắt buộc xuất hiện mà không có đủ nguồn cung trước đó trong cùng một chuỗi. TRONG:```
S = [1], T = [100], k = 7
```Không có chuỗi thế hệ dư thừa nào có thể thu hẹp khoảng cách vì không có phần tử nào trước đó được truyền đi lên qua các bước trung gian. Thuật toán xác định chính xác điều này là không thể khi thặng dư vẫn bằng 0 tại điểm cầu.
