---
title: "CF 102388E - Chuồng ngựa"
description: "Chúng tôi có một đồ thị vô hướng với nhiều nhất là 50 thành phố. Đường cho phép ngựa di chuyển giữa hai điểm cuối của nó trong một bước và đường cũng có thể là đường vòng. Đối với một thành phố cố định v, chúng ta cần quyết định xem có tồn tại một cuộc đi bộ bắt đầu tại v, sử dụng chính xác k đường và kết thúc tại v hay không."
date: "2026-08-16T08:50:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102388
codeforces_index: "E"
codeforces_contest_name: "SUFE ICPC Team Formation Test"
rating: 0
weight: 102388
solve_time_s: 360
verified: false
draft: false
---

[CF 102388E - Chuồng ngựa](https://codeforces.com/problemset/problem/102388/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một đồ thị vô hướng với nhiều nhất là 50 thành phố. Đường cho phép ngựa di chuyển giữa hai điểm cuối của nó trong một bước và đường cũng có thể là đường vòng. Đối với một thành phố cố định v, chúng ta cần quyết định xem có tồn tại một lối đi bắt đầu tại v, sử dụng chính xác k đường và kết thúc ở v hay không. Câu trả lời là số lượng thành phố tồn tại một lối đi khép kín như vậy. 

Đầu vào chứa tối đa 20 biểu đồ độc lập. Đồ thị có số đỉnh nhỏ, với n<50, nhưng k có thể lớn bằng 10 9. Sự kết hợp đó chính là khó khăn chính. Bất kỳ thuật toán nào thực hiện một thao tác mỗi ngày hoặc một lần duyệt đồ thị mỗi bước đều không thể tồn tại được sau một tỷ bước. Mặt khác, n=50 đủ nhỏ để chúng ta có thể thực hiện các thuật toán liên quan đến khoảng n 2 công việc trên mỗi bit của k. Vì 10 9 chỉ có khoảng 30 chữ số nhị phân nên phép lũy thừa logarit là mục tiêu tự nhiên. 

Có một số trường hợp nguy hiểm có thể dễ dàng phá vỡ quá trình triển khai. Khi k=0, mọi thành phố đều đủ điều kiện vì chặng đi bộ trống đã bắt đầu và kết thúc tại cùng một thành phố. Ví dụ,```
13 0 0
```có đầu ra```
3
```Giải pháp yêu cầu ít nhất một con đường sẽ trả về số 0 không chính xác. 

Vòng lặp tự quan trọng đặc biệt khi k = 1. Vì```
12 1 10 0
```câu trả lời là`1`, bởi vì thành phố 0 có thể tự lặp lại một lần và quay trở lại chính nó, trong khi thành phố 1 bị cô lập. Một giải pháp coi biểu đồ là một biểu đồ đơn giản mà không bảo toàn các mục đường chéo sẽ bỏ sót thành phố 0. 

Đường song song không cần xử lý đặc biệt. Nếu hai con đường nối cùng một cặp thành phố thì chúng không tạo thêm khả năng nào cho việc đi bộ. Chúng tôi chỉ quan tâm liệu có tồn tại ít nhất một quá trình chuyển đổi hay không. Ví dụ,```
12 3 20 10 10 1
```có đầu ra`2`. Cả hai thành phố đều có thể đi đến thành phố kia và quay trở lại ngay lập tức. 

Cuối cùng, tính chẵn lẻ có thể gây nhầm lẫn. Trong đồ thị hai bên, mỗi bước đi khép kín có độ dài chẵn, nhưng sự tồn tại của một chu trình lẻ sẽ làm thay đổi tình hình. Ví dụ,```
13 3 30 11 22 0
```có đầu ra`3`, vì mọi thành phố đều nằm trên hình tam giác. Cố gắng giải quyết vấn đề chỉ sử dụng tính chất lưỡng cực của đồ thị cũng sẽ bỏ lỡ các trường hợp đặc biệt, chẳng hạn như một đỉnh gắn liền với một chu trình lẻ, trong đó các bước đi khép kín lẻ đủ dài có thể tồn tại nhưng các bước đi ngắn thì có thể không. Việc xây dựng ma trận tránh phải mô tả thủ công tất cả các trường hợp này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng các vị trí có thể có sau mỗi bước. Sửa một thành phố bắt đầu s, giữ cho tập hợp các thành phố có thể truy cập được sau đúng t bước và liên tục mở rộng tập hợp đó thông qua biểu đồ. Sau k vòng, kiểm tra xem s có thể truy cập được không. Điều này đúng vì tập hợp sau vòng t biểu thị chính xác điểm cuối của các bước đi có độ dài t bắt đầu từ s. 

Vấn đề là K. Trong trường hợp xấu nhất, một vòng có thể kiểm tra mọi con đường, do đó việc xử lý một thành phố xuất phát mất O(km). Lặp lại điều này cho tất cả n thành phố bắt đầu sẽ cho O(knm). Ở những hạn chế tối đa, điều này gần như 

10 9 ⋅50⋅2500=1,25×10 14 

kỳ thi đường bộ, vượt xa thời hạn. 

Biểu đồ đủ nhỏ để thay thế mô phỏng từng bước bằng phép lũy thừa ma trận. Xác định ma trận kề Boolean A, trong đó A[i][j] đúng khi một con đường cho phép di chuyển từ i đến j. Trong phép nhân ma trận Boolean, mục (A t )[i][j] cho chúng ta biết liệu có tồn tại một bước đi chính xác t bước từ i đến j hay không. Do đó, thành phố i hợp lệ chính xác khi mục nhập đường chéo (A k )[i][i] là đúng. 

Lũy thừa nhị phân làm giảm số phép nhân ma trận từ k xuống O(logk). Phép nhân ma trận thông thường sẽ tốn O(n 3 ), điều này vốn đã hợp lý với n=50, nhưng Python thậm chí còn có thể làm tốt hơn ở đây bằng cách biểu diễn mỗi hàng ma trận dưới dạng một tập hợp số nguyên duy nhất. Sau đó, một hàng chứa tập hợp các đỉnh có thể tiếp cận dưới dạng bit và việc nhân hai ma trận Boolean sẽ trở thành một chuỗi các phép toán OR theo bit. 

Đối với hàng i của ma trận bên trái, mỗi tập bit j có nghĩa là i có thể đạt đến j. Hàng tương ứng B[j] của ma trận bên phải chứa tất cả các đỉnh có thể tới được từ j. Do đó, hàng kết quả chỉ đơn giản là OR của B[j] trên tất cả các bit được đặt j trong hàng i. Điều này làm giảm phép nhân thực tế đối với các phép toán hàng O(n 2 ), với mỗi phép toán sử dụng các số nguyên có độ chính xác tùy ý được tối ưu hóa cao của Python. 

Phương pháp brute-force hoạt động vì nó tuân theo rõ ràng các bước đi từng bước một, nhưng không thành công vì k rất lớn. Nhận xét rằng chỉ có biểu diễn nhị phân của k quan trọng mới cho phép chúng ta thực hiện nhiều bước theo cấp số nhân cùng một lúc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(knm) | O(n) | Quá chậm | 
| Hàm mũ ma trận Boolean với Bitset | Hoạt động bitset O (n 2 logk) | O(n) bit | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng mối quan hệ kề cận như một tập hợp nhỏ cho mọi thành phố. Bit j ở hàng i được thiết lập khi có một con đường nằm giữa i và j. Bởi vì đồ thị là vô hướng, nên cạnh đầu vào (x,y) đặt cả x→y và y→x. Đối với vòng lặp tự, điều này tự nhiên đặt bit đường chéo. 
2. Biểu diễn ma trận nhận dạng dưới dạng bitset. Hàng i của nó chỉ chứa bit i, bởi vì ma trận danh tính biểu thị một bước đi có độ dài bằng 0 ở cùng một thành phố. 
3. Duy trì hai ma trận Boolean,`result`Và`base`. Ban đầu,`result`là ma trận nhận dạng và`base`là ma trận kề. Tính bất biến đó là`result`đại diện cho tích các lũy thừa của ma trận kề ban đầu đã được chọn từ các bit được xử lý của k, trong khi`base`đại diện cho sức mạnh hiện tại A 2 p. 
4. Kiểm tra biểu diễn nhị phân của k từ bit có trọng số nhỏ nhất của nó. Nếu bit hiện tại là một, hãy nhân`result`qua`base`. Điều này kết hợp lũy thừa tương ứng A 2 p vào câu trả lời. 
5. Hình vuông`base`để có được sức mạnh tiếp theo của hai. Phép nhân ma trận Boolean được sử dụng ở đây vì chúng tôi quan tâm liệu có tồn tại ít nhất một bước đi chứ không phải có bao nhiêu bước tồn tại. 
6. Dịch k sang phải một bit và tiếp tục cho đến khi mọi bit được xử lý. Cần nhiều nhất 30 bit vì k 10 9. 
7. Sau khi tính lũy thừa, xét đường chéo của`result`. Nếu bit i được đặt ở hàng i thì sẽ phải đi bộ chính xác k bước từ thành phố i trở lại thành phố i. Đếm tất cả các thành phố như vậy. 

### Tại sao nó hoạt động 

Bất biến trung tâm là sau khi xử lý một số tiền tố của biểu diễn nhị phân của k,`result`bằng tích Boolean có lũy thừa chính xác tương ứng với các bit một được xử lý. Vì phép nhân ma trận Boolean tạo nên sự tồn tại của các bước đi, nên A t [i][j] đúng chính xác khi một số bước đi có độ dài t nối i với j. Phép lũy thừa nhị phân cuối cùng xây dựng nên A k, do đó đường chéo của nó chứa chính xác các thành phố thừa nhận một quãng đường khép kín có chiều dài k. Việc triển khai bitset chỉ thay đổi cách tính phép nhân Boolean chứ không thay đổi kết quả toán học mà nó biểu thị. 

## Giải pháp Python```python
Pythonimport sysinput = sys.stdin.readline

def multiply(A, B, n):    """    Boolean matrix multiplication.
    Each row is a bitset. For every set bit j in A[i],    row B[j] contributes all vertices reachable after the    second part of the walk.    """    C = [0] * n
    for i in range(n):        mask = A[i]        row = 0
        while mask:            bit = mask & -mask            j = bit.bit_length() - 1            row |= B[j]            mask ^= bit
        C[i] = row
    return C

def solve():    T = int(input())    answers = []
    for _ in range(T):        n, m, k = map(int, input().split())
        adj = [0] * n
        for _ in range(m):            x, y = map(int, input().split())            adj[x] |= 1 << y            adj[y] |= 1 << x
        # A^0 = I.        result = [1 << i for i in range(n)]
        # A^(2^p), starting with A^1.        base = adj
        while k:            if k & 1:                result = multiply(result, base, n)
            k >>= 1
            if k:                base = multiply(base, base, n)
        answer = 0        for i in range(n):            if result[i] & (1 << i):                answer += 1
        answers.append(str(answer))
    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":    solve()
```Việc xây dựng kề sử dụng một số nguyên cho mỗi thành phố. Chút`j`đại diện cho thành phố`j`, vì vậy việc thiết lập`1 << j`ghi lại sự tồn tại của quá trình chuyển đổi đến thành phố đó. Việc đặt cả hai hướng sẽ xử lý đường vô hướng và thực hiện cùng một thao tác hai lần cho một cạnh song song không có tác dụng, đó chính xác là những gì chúng ta muốn.`result = [1 << i for i in range(n)]`tạo ra ma trận nhận dạng. Điều này là cần thiết ngay cả khi k=0, vì A 0 =I và đường chéo của đồng dạng chứa mọi thành phố. các`while k`do đó vòng lặp xử lý k=0 mà không có bất kỳ nhánh đặc biệt nào. 

Thói quen nhân xứng đáng được chú ý nhất. Giả sử bit j được đặt trong`A[i]`. Điều đó có nghĩa là có một đoạn bước đầu tiên từ i đến j. Mỗi bit được đặt vào`B[j]`đại diện cho đoạn thứ hai từ j tới đích nào đó. Lấy OR trên tất cả như vậy`B[j]`do đó cung cấp chính xác các điểm đến có thể tiếp cận thông qua việc đi bộ được nối. 

biểu thức`mask & -mask`trích xuất bit được đặt thấp nhất.`bit.bit_length() - 1`chuyển đổi bit đó thành chỉ số đỉnh của nó. Loại bỏ nó với`mask ^= bit`đảm bảo rằng mọi đỉnh trung gian có thể tiếp cận đều được xử lý một lần. 

Không có vấn đề tràn số nguyên. Số nguyên Python tự động tăng lên và bitset lớn nhất chỉ có 50 bit có ý nghĩa. Giá trị của k cũng được xử lý trực tiếp dưới dạng số nguyên Python, do đó giới hạn 10 9 không yêu cầu số học đặc biệt. 

Thứ tự của các phép tính trong vòng lũy ​​thừa cũng được cân nhắc kỹ lưỡng. Nếu bit hiện tại của k là một thì công suất hiện tại phải được nhân thành`result`. Sau đó, công suất hiện tại được bình phương để chuẩn bị chữ số nhị phân tiếp theo. các`if k`bảo vệ tránh một bình phương cuối cùng không cần thiết. 

## Ví dụ đã hoạt động 

Trường hợp thử nghiệm mẫu đầu tiên là```
3 2 30 10 2
```Đồ thị là một đường đi có độ dài bằng 2, với thành phố 0 ở giữa. Chúng tôi muốn một cuộc đi bộ khép kín có chiều dài 3. 

Các hàng liền kề được biểu diễn bằng các tập hợp bit. Vị trí bit 0, 1 và 2 tương ứng với ba thành phố. 

| Sân khấu | k |`result`hàng |`base`đại diện cho | 
| --- | --- | --- | --- | 
| Ban đầu | 3 |`001`,`010`,`100`| A 1 | 
| Bit 0 = 1 | 3 | A | A 1 | 
| Thay đổi | 1 | A | A 2 | 
| Bit 1 = 1 | 1 | A 3 | A 2 | 
| Kết thúc | 0 | A 3 | A 2 | 

Không có chu trình lẻ và không có vòng tự lặp, nên đồ thị là hai phần và mọi bước đi khép kín đều có độ dài chẵn. Đường chéo của A 3 sai hoàn toàn, cho đáp án`0`. 

Trường hợp thử nghiệm mẫu thứ hai là```
3 2 40 10 2
```Đây là cùng một biểu đồ, nhưng bây giờ k=4. 

| Sân khấu | k |`result`|`base`| 
| --- | --- | --- | --- | 
| Ban đầu | 4 | Tôi | A | 
| Thay đổi | 2 | Tôi | A 2 | 
| Thay đổi | 1 | Tôi | A 4 | 
| Bit 2 = 1 | 1 | A 4 | A 4 | 
| Kết thúc | 0 | A 4 | A 4 | 

Mỗi thành phố đều có một lối đi dài 4 chiều. Ví dụ từ thành phố 1, chúng ta có thể sử dụng 

1→0→1→0→1. 

Công trình xây dựng tương tự cho thành phố 2, trong khi thành phố 0 có thể xen kẽ với một trong hai thành phố lân cận. Do đó mọi mục nhập chéo của A 4 đều đúng và câu trả lời là`3`. 

Hai dấu vết này cũng cho thấy lý do tại sao chỉ nhìn vào khả năng tiếp cận mà không theo dõi độ dài bước đi chính xác sẽ là không đủ. Biểu đồ được kết nối trong cả hai trường hợp, nhưng độ dài 3 tạo ra không có đường đi khép kín trong khi độ dài 4 tạo ra một đường đi ở mọi thành phố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Hoạt động bitset O (n 2 logk) | Có các sản phẩm ma trận O(logk) và mỗi sản phẩm xử lý tối đa n 2 tập bit | 
| Không gian | O(n) Số nguyên Python | Hai ma trận Boolean n hàng được lưu trữ, mỗi hàng chỉ chứa n bit liên quan | 

Với n 50 và k 10 9, có nhiều nhất 30 mức lũy thừa. Mỗi phép nhân ma trận Boolean xử lý tối đa 50 mối quan hệ hàng 2 = 2500 và mỗi mối quan hệ được xử lý thông qua các phép toán bit số nguyên gốc. Điều này thoải mái trong giới hạn thời gian 3 giây và thấp hơn nhiều so với giới hạn bộ nhớ 256 MB. 

Sự khác biệt giữa cách triển khai này và phép nhân ma trận O(n 3 logk) thông thường rất hữu ích trong Python. Biểu diễn bitset nén toàn bộ hàng Boolean thành một số nguyên, do đó phép toán bên trong tốn kém được thực hiện bằng số học số nguyên được tối ưu hóa thay vì vòng lặp cấp Python trên tất cả các đích có thể. 

## Trường hợp thử nghiệm```python
Pythonimport sysimport io

def solve_data(inp: str) -> str:    old_stdin = sys.stdin    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)    out = io.StringIO()    sys.stdout = out
    try:        T = int(sys.stdin.readline())        answers = []
        def multiply(A, B, n):            C = [0] * n
            for i in range(n):                mask = A[i]                row = 0
                while mask:                    bit = mask & -mask                    j = bit.bit_length() - 1                    row |= B[j]                    mask ^= bit
                C[i] = row
            return C
        for _ in range(T):            n, m, k = map(int, sys.stdin.readline().split())            adj = [0] * n
            for _ in range(m):                x, y = map(int, sys.stdin.readline().split())                adj[x] |= 1 << y                adj[y] |= 1 << x
            result = [1 << i for i in range(n)]            base = adj
            while k:                if k & 1:                    result = multiply(result, base, n)
                k >>= 1
                if k:                    base = multiply(base, base, n)
            answer = sum(                1 for i in range(n)                if result[i] & (1 << i)            )            answers.append(str(answer))
        sys.stdout.write("\n".join(answers))        return out.getvalue()
    finally:        sys.stdin = old_stdin        sys.stdout = old_stdout

# Provided sampleassert solve_data("""\33 2 30 10 23 2 40 10 25 5 50 11 22 03 44 0""") == "0\n3\n4", "provided sample"

# Minimum-size graph, k = 0.# The empty walk is valid at the only city.assert solve_data("""\11 0 0""") == "1", "k = 0"

# One vertex with a self-loop.# The loop can be traversed any positive number of times.assert solve_data("""\11 1 10 0""") == "1", "self-loop and k = 1"

# Two isolated vertices, k > 0.# There is no road at all, so no positive-length walk exists.assert solve_data("""\12 0 7""") == "0", "isolated vertices"

# Parallel edges and an even walk.# Multiplicity does not matter because we only ask whether a walk exists.assert solve_data("""\12 3 20 10 10 1""") == "2", "parallel edges"

# A triangle, k = 3.# Every vertex can traverse the triangle once and return.assert solve_data("""\13 3 30 11 22 0""") == "3", "odd cycle"

# Maximum-size vertex count and a huge k.# Complete graph has a closed walk of every positive length at every vertex.edges = []n = 50for i in range(n):    for j in range(i + 1, n):        edges.append(f"{i} {j}")
max_case = "1\n50 1225 1000000000\n" + "\n".join(edges) + "\n"assert solve_data(max_case) == "50", "maximum n and huge k"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 0`|`1`| Đồ thị tối thiểu và ranh giới k=0 | 
| Một đỉnh có một vòng lặp, k=1 |`1`| Tự lặp và quay lại chính xác một bước | 
| Hai đỉnh cô lập, k=7 |`0`| Không đi bộ dài tích cực | 
| Ba cạnh song song giữa hai đỉnh, k=2 |`2`| Các cạnh song song không ảnh hưởng đến sự tồn tại | 
| Tam giác, k=3 |`3`| Đi dạo khép kín | 
| Đồ thị hoàn chỉnh trên 50 đỉnh, k=10 9 |`50`| N tối đa, k lớn và lũy thừa nhị phân | 

## Vỏ cạnh 

### Không bước 

Hãy xem xét```
13 0 0
```Thuật toán khởi tạo`result`vào ma trận nhận dạng và không bao giờ đi vào vòng lũy ​​thừa vì`k`là số không. Ma trận đồng nhất có mọi tập hợp mục nhập đường chéo, vì vậy cả ba thành phố đều được tính. Điều này phù hợp với định nghĩa về chiều dài đi bộ bằng không. 

### Tự lặp với một bước 

Hãy xem xét```
12 1 10 0
```Hàng kề của thành phố 0 chứa bit 0, trong khi thành phố 1 có hàng trống. Vì k=1 nên`result`trở thành ma trận kề. Đường chéo của nó chỉ chứa giá trị thực tại thành phố 0, vì vậy câu trả lời là`1`. 

Trường hợp này phát hiện các triển khai vô tình bỏ qua các vòng tự lặp hoặc chỉ chèn một cạnh khi điểm cuối của nó khác nhau. 

### Đỉnh cô lập 

Hãy xem xét```
12 0 7
```Ma trận kề đều bằng 0. Mọi lũy thừa dương của ma trận Boolean bằng 0 vẫn bằng 0, do đó không có mục nhập đường chéo nào được đặt. Câu trả lời là`0`. Ma trận nhận dạng không gây ra kết quả dương tính giả vì nó chỉ được sử dụng cho số mũ bằng 0 và ở đây số mũ là số dương. 

### Đường song song 

Hãy xem xét```
12 3 20 10 10 1
```Ba đường đầu vào đều đặt hai bit kề nhau giống nhau. Sau khi xây dựng, ma trận chính xác là ma trận kề của một cạnh vô hướng. Bình phương nó sẽ cho một đường chéo đúng ở cả hai đỉnh, tương ứng với các bước đi 0→1→0 và 1→0→1. Câu trả lời là`2`. 

Việc xử lý đầu vào dưới dạng đa đồ thị với số lượng sẽ không cần thiết vì bài toán yêu cầu sự tồn tại thay vì số lần đi bộ có thể. 

### Chu kỳ lẻ 

Hãy xem xét```
13 3 30 11 22 0
```Hàm mũ thứ nhất cho phép một cạnh và lập phương ma trận kề Boolean sẽ phát hiện đường đi của tam giác từ mọi đỉnh trở về chính nó. Đường chéo của A 3 hoàn toàn đúng nên đáp án là`3`. 

Đây cũng là lý do tại sao giải pháp chỉ dựa trên k chẵn hoặc lẻ là không đủ. Cấu trúc biểu đồ xác định độ dài chính xác nào có thể và lũy thừa ma trận Boolean biểu thị trực tiếp cấu trúc đó.
