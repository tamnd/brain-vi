---
title: "CF 104287J - Hai và Ba"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm có một mảng các số nguyên dương. Hai người chơi luân phiên nhau, bắt đầu với Nino."
date: "2026-07-01T20:49:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104287
codeforces_index: "J"
codeforces_contest_name: "Teamscode Spring 2023 Contest"
rating: 0
weight: 104287
solve_time_s: 73
verified: true
draft: false
---

[CF 104287J - Hai và Ba](https://codeforces.com/problemset/problem/104287/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm có một mảng các số nguyên dương. Hai người chơi luân phiên nhau, bắt đầu với Nino. Trong một lượt, người chơi chọn bất kỳ phần tử đơn lẻ nào và thay thế nó bằng ⌊x/2⌋ hoặc ⌊x/3⌋, nhưng chỉ khi kết quả vẫn dương. Nếu người chơi không có nước đi hợp lệ trong lượt của mình, họ sẽ thua. Câu hỏi đặt ra là ai thắng nếu cả hai người chơi đều chơi tối ưu. 

Khía cạnh quan trọng là trạng thái trò chơi không chỉ là các giá trị mảng mà còn có thể giảm bao nhiêu bước từ mỗi giá trị. Mỗi số tiến hóa độc lập ngoại trừ thứ tự lần lượt, vì chỉ có một phần tử được sửa đổi cho mỗi lần di chuyển. 

Các ràng buộc rất lớn: lên tới 100000 trường hợp thử nghiệm và tổng kích thước mảng lên tới 100000. Điều này loại trừ mọi mô phỏng mỗi bước di chuyển của cây trò chơi. Ngay cả việc tính toán biểu đồ trò chơi đầy đủ cho mỗi số cũng sẽ quá chậm, vì một giá trị duy nhất lên tới 10^9 có thể có nhiều đường rút gọn nếu khám phá một cách ngây thơ. 

Mối nguy hiểm tự nhiên đang cố gắng mô phỏng các bước di chuyển một cách trực tiếp. Ví dụ: đối với một giá trị duy nhất như 10^9, việc chia liên tục cho 2 hoặc 3 sẽ tạo ra một cây trạng thái phân nhánh lớn và việc kết hợp nhiều giá trị như vậy sẽ tạo ra không gian trạng thái theo cấp số nhân. Bất kỳ cách tiếp cận nào xử lý rõ ràng từng bước đi sẽ thất bại. 

Vấn đề tế nhị thứ hai là giả định rằng chỉ có tính chẵn lẻ của các giá trị mới quan trọng. Điều đó là sai vì chia cho 3 tạo ra quỹ đạo giảm khác với chia cho 2 và chúng không tương đương về mặt trò chơi. 

## Phương pháp tiếp cận 

Thoạt nhìn, đây là một trò chơi tổ hợp khách quan bình thường. Mỗi phần tử mảng hoạt động giống như một đống nơi bạn có thể giảm kích thước của nó theo các bước riêng biệt. Điều này gợi ý tính toán số Grundy cho mỗi phần tử và XOR chúng. 

Nếu chúng ta ép buộc một giá trị x duy nhất, chúng ta có thể xác định tất cả các trạng thái có thể truy cập bằng cách áp dụng liên tục x → ⌊x/2⌋ hoặc x → ⌊x/3⌋ trong khi kết quả vẫn dương. Điều đó xây dựng một biểu đồ tuần hoàn có hướng của các trạng thái. Từ biểu đồ đó, chúng tôi tính toán số Grundy. Điều này đúng vì mỗi lần di chuyển đều làm giảm giá trị một cách nghiêm ngặt, do đó không tồn tại chu kỳ. 

Tuy nhiên, việc tạo ra tất cả các trạng thái cho mỗi x là rất tốn kém. Trong trường hợp xấu nhất, mỗi số có thể tạo ra độ sâu O(log x) và việc phân nhánh theo 2 thao tác dẫn đến nhiều trạng thái lặp lại trong các trường hợp thử nghiệm khác nhau. Trên 10^5 giá trị, tốc độ này trở nên quá chậm. 

Quan sát quan trọng là biểu đồ trạng thái của một số có cấu trúc cực kỳ chặt chẽ. Mọi giá trị cuối cùng sẽ ánh xạ xuống một tập hợp nhỏ các trạng thái cuối và hầu hết các đường dẫn đều nhanh chóng hội tụ. Quan trọng hơn, giá trị Grundy chỉ phụ thuộc vào số lần chúng ta có thể chia cho 2 và 3 theo các chuỗi khác nhau, điều này làm giảm việc đếm các trạng thái trong một tập hợp nhỏ có thể truy cập thay vì khám phá các đường dẫn đầy đủ. 

Một cách xem hiệu quả hơn là tính toán giá trị Grundy cho tất cả các số lên đến mức tối đa có thể truy cập thông qua chuỗi chia, nhưng vì mỗi bước làm giảm giá trị ít nhất theo hệ số 2 hoặc 3, nên tổng giá trị có thể truy cập riêng biệt trên tất cả các nút là đủ nhỏ để tính toán trước một cách linh hoạt cho mỗi trường hợp thử nghiệm mà không bị trùng lặp. Điều này dẫn đến việc ghi nhớ các trạng thái có sự chuyển đổi trực tiếp sang ⌊x/2⌋ và ⌊x/3⌋. 

Sau khi chúng tôi tính toán các giá trị Grundy cho từng phần tử, trò chơi tổng thể là tổng của các trò chơi công bằng độc lập, do đó XOR xác định người chiến thắng: XOR khác 0 có nghĩa là người chơi đầu tiên thắng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Khám phá trạng thái vũ lực cho mỗi trường hợp thử nghiệm | Hàm mũ trong trường hợp xấu nhất | O(n) | Quá chậm | 
| DP được ghi nhớ qua các lần chuyển đổi x→x/2, x/3 | O(n log A) được khấu hao | O(n log A) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi giá trị là một trạng thái trò chơi có số Grundy phụ thuộc vào hai nước đi có thể có của nó.

1. Đối với mỗi trường hợp thử nghiệm, chúng tôi xử lý mọi phần tử một cách độc lập nhưng sử dụng lại bản đồ ghi nhớ chung để các giá trị lặp lại không được tính toán lại. Điều này hợp lệ vì luật chơi chỉ phụ thuộc vào giá trị chứ không phụ thuộc vào vị trí của nó. 
2. Xác định hàm grundy(x) trả về số Grundy của giá trị x. Nếu x bằng 1 thì không thể di chuyển được nên số Grundy của nó là 0. 
3. Đối với x lớn hơn 1, chúng tôi xem xét tất cả các trạng thái có thể đạt được: x chia cho 2 và x chia cho 3, nhưng chỉ khi kết quả vẫn dương. Đây là những động thái hợp pháp duy nhất nên chúng xác định đầy đủ cấu trúc chuyển đổi. 
4. Tính toán đệ quy grundy(x // 2) và grundy(x // 3). Thu thập kết quả của họ thành một bộ. 
5. Giá trị Grundy của x là mex (giá trị loại trừ tối thiểu) của tập hợp đó. Trong bài toán này, kích thước được đặt nhiều nhất là 2, do đó mex được tính bằng cách kiểm tra 0, 1, 2 theo thứ tự. 
6. XOR tất cả các lỗi(a_i) trên mảng. Nếu kết quả khác 0 thì Nino thắng; nếu không thì Miku thắng. 

### Tại sao nó hoạt động 

Mỗi nước đi sẽ làm giảm nghiêm trọng giá trị của phần tử được chọn, do đó đồ thị trò chơi có tính tuần hoàn và phù hợp với lý thuyết Sprague-Grundy tiêu chuẩn. Mỗi phần tử là một trò chơi con độc lập vì một nước đi chỉ ảnh hưởng đến một vị trí. Do đó, trò chơi đầy đủ là XOR của các giá trị Grundy độc lập. 

Việc ghi nhớ đảm bảo mỗi giá trị số nguyên được giải quyết một lần và vì mỗi lần chuyển đổi đều giảm cường độ ít nhất theo hệ số 2 hoặc 3, nên độ sâu đệ quy là logarit theo kích thước giá trị. Điều này đảm bảo rằng DP hội tụ nhanh chóng và không cần tính toán lại làm tăng độ phức tạp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

memo = {}

def grundy(x):
    if x == 1:
        return 0
    if x in memo:
        return memo[x]

    moves = set()

    nx = x // 2
    if nx > 0:
        moves.add(grundy(nx))

    ny = x // 3
    if ny > 0:
        moves.add(grundy(ny))

    g = 0
    while g in moves:
        g += 1

    memo[x] = g
    return g

t = int(input())
for _ in range(t):
    n = int(input())
    arr = list(map(int, input().split()))

    xor_sum = 0
    for v in arr:
        xor_sum ^= grundy(v)

    print("Nino" if xor_sum != 0 else "Miku")
```Giải pháp được xây dựng dựa trên tính toán Grundy đệ quy được ghi nhớ. Từ điển ghi nhớ đảm bảo mỗi giá trị được đánh giá một lần trên toàn cầu. Đệ quy xử lý trực tiếp hai chuyển đổi được phép. 

Việc tính toán mex được đơn giản hóa vì mỗi trạng thái có nhiều nhất hai cạnh đi ra, vì vậy chúng tôi chỉ kiểm tra các số nguyên nhỏ một cách tuần tự. 

Tập hợp XOR thực hiện định lý Sprague-Grundy cho các cọc rời rạc. 

Một chi tiết triển khai tinh tế là bản ghi nhớ chung: nếu không có nó, các giá trị lặp lại trong các trường hợp thử nghiệm sẽ tính toán lại toàn bộ chuỗi, làm tăng thời gian. Một điều tinh tế khác là độ sâu đệ quy; mặc dù các giá trị co lại nhanh chóng, nhưng giới hạn đệ quy của Python vẫn có thể bị ảnh hưởng đối với các chuỗi đối nghịch, do đó giới hạn đệ quy sẽ tăng lên. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trong số các trường hợp thử nghiệm mẫu. 

### Vật mẫu:`[1, 2, 5]`Chúng tôi tính toán các giá trị Grundy: 

| Giá trị | x//2 | x//3 | Di chuyển bộ Grundy | bẩn thỉu | 
| --- | --- | --- | --- | --- | 
| 1 | - | - | ∅ | 0 | 
| 2 | 1 | 0 không hợp lệ | {0} | 1 | 
| 5 | 2 | 1 | {1, 0} | 2 | 

Bây giờ XOR: 0 ⊕ 1 ⊕ 2 = 3 ≠ 0, vậy Nino thắng. 

Điều này chứng tỏ rằng ngay cả việc phân nhánh nhỏ từ 5 cũng tạo ra hai trạng thái Grundy riêng biệt có thể tiếp cận được, tạo ra một mex không tầm thường. 

### Vật mẫu:`[1, 2, 3, 4]`Chúng tôi tính toán: 

| Giá trị | x//2 | x//3 | Di chuyển bộ Grundy | bẩn thỉu | 
| --- | --- | --- | --- | --- | 
| 1 | - | - | ∅ | 0 | 
| 2 | 1 | 0 không hợp lệ | {0} | 1 | 
| 3 | 1 | 1 | {0} | 1 | 
| 4 | 2 | 1 | {1, 0} | 2 | 

XOR: 0 ⊕ 1 ⊕ 1 ⊕ 2 = 2 ≠ 0, vậy Nino thắng. 

Dấu vết này hiển thị hiệu ứng hủy trong XOR khi xuất hiện nhiều giá trị Grundy giống hệt nhau, trong khi vẫn để lại kết quả khác 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(∑ log a_i) khấu hao | Mỗi trạng thái số nguyên được tính một lần và mỗi phép tính chỉ lặp lại thành x/2 và x/3 | 
| Không gian | O(#giá trị có thể truy cập khác biệt) | Ghi nhớ lưu trữ một mục nhập cho mỗi giá trị gặp phải | 

Tổng số trạng thái được giới hạn bởi số lượng giá trị duy nhất có thể truy cập được thông qua phép chia lặp lại cho 2 và 3 từ tất cả các đầu vào. Vì mỗi bước sẽ giảm cường độ một cách nhanh chóng nên giá trị này vẫn nằm trong giới hạn đối với tổng kích thước đầu vào là 10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    sys.setrecursionlimit(10**7)
    memo = {}

    def grundy(x):
        if x == 1:
            return 0
        if x in memo:
            return memo[x]

        moves = set()

        nx = x // 2
        if nx > 0:
            moves.add(grundy(nx))

        ny = x // 3
        if ny > 0:
            moves.add(grundy(ny))

        g = 0
        while g in moves:
            g += 1

        memo[x] = g
        return g

    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        xor_sum = 0
        for v in arr:
            xor_sum ^= grundy(v)
        res.append("Nino" if xor_sum else "Miku")

    return "\n".join(res) + "\n"

# provided samples
assert run("""5
3
1 2 5
3
2 3 4
2
3366 3366
1
1000000000
7
1 2 3 4 5 6 7
""") == """Nino
Nino
Miku
Nino
Miku
"""

# custom cases
assert run("""1
1
1
""") == "Miku\n", "single losing state"

assert run("""1
1
2
""") == "Nino\n", "single winning state"

assert run("""1
3
1 1 1
""") == "Miku\n", "all neutral"

assert run("""1
4
2 2 2 2
""") == "Miku\n", "even XOR cancellation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn 1 | Miku | thiết bị đầu cuối mất vị trí | 
| đơn 2 | Nino | nước đi thắng đơn giản nhất | 
| tất cả những cái | Miku | tất cả đều bằng không Grundy | 
| bốn đôi | Miku | Hủy XOR | 

## Vỏ cạnh 

cho`a = [1]`, phần tử duy nhất đã không có nước đi hợp lệ nên người chơi bắt đầu sẽ thua ngay lập tức. Thuật toán trả về Grundy(1) = 0, XOR = 0 nên Miku thắng. 

Đối với các giá trị lớn như`10^9`, đệ quy liên tục áp dụng phép chia cho 2 và 3 cho đến khi đạt 1. Việc ghi nhớ đảm bảo rằng các bài toán con chồng chéo như 10^9 → 5×10^8 → 2,5×10^8 được tính toán một lần và được sử dụng lại trên toàn bộ bộ thử nghiệm. Điều này ngăn chặn sự bùng nổ theo cấp số nhân. 

Đối với các phần tử giống hệt nhau lặp đi lặp lại, chẳng hạn như`[2, 2, 2, 2]`, mỗi cái đóng góp Grundy(2) = 1 và XOR hủy theo cặp. Thuật toán tạo ra đúng số 0, nghĩa là người chơi thứ hai thắng, khớp với tính đối xứng của các cọc độc lập giống hệt nhau.
