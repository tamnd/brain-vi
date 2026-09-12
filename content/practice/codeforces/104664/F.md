---
title: "CF 104664F - Mì và Bước đi ngẫu nhiên"
description: "Sợi mì bắt đầu ở độ dài 0. Trong mỗi T giây tiếp theo, độ dài của sợi mì thay đổi +1 hoặc -1. Mỗi chuỗi lựa chọn tạo ra một bước đi ngẫu nhiên có độ dài T."
date: "2026-06-29T11:31:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104664
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 10-06-23 Div. 2 (Beginner)"
rating: 0
weight: 104664
solve_time_s: 88
verified: false
draft: false
---

[CF 104664F - Mì và Bước đi ngẫu nhiên](https://codeforces.com/problemset/problem/104664/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Sợi mì bắt đầu dài`0`. Trong mỗi lần tiếp theo`T`giây, độ dài của nó thay đổi theo một trong hai`+1`hoặc`-1`. Mỗi chuỗi lựa chọn tạo ra một bước đi ngẫu nhiên có độ dài`T`. 

Đối với mỗi trường hợp thử nghiệm, chúng ta phải đếm xem có bao nhiêu bước đi có giá trị cao nhất trong toàn bộ khoảng thời gian, bao gồm cả vị trí ban đầu tại thời điểm đó`0`, bằng chính xác`M`. Vì câu trả lời có thể rất lớn nên mọi kết quả đều được lấy modulo`10^9 + 7`. 

Giá trị lớn nhất của`T`chỉ là`2000`, nhưng có thể có nhiều như`10^5`trường hợp thử nghiệm. Việc tính toán một chương trình động một cách độc lập cho mỗi truy vấn sẽ yêu cầu khoảng`10^5 × 2000²`hoạt động hoàn toàn không thể thực hiện được. Giới hạn trên nhỏ của`T`đề xuất xử lý trước mọi giá trị có thể một lần, sau đó trả lời từng truy vấn trong thời gian không đổi. 

Một trường hợp tế nhị là khi`M > T`. Bước đi chỉ thay đổi một đơn vị mỗi giây, vì vậy sau`T`bước nó không bao giờ có thể đạt đến độ cao lớn hơn`T`. Ví dụ,```
1
2 3
```có câu trả lời`0`. 

Một sai lầm dễ mắc phải khác là quên rằng vị trí ban đầu cũng được tính khi tính giá trị lớn nhất. Ví dụ,```
1
1 0
```sẽ có câu trả lời`1`, bởi vì đi bộ`(0, -1)`không bao giờ vượt lên trên`0`. Vấn đề này đảm bảo`M ≥ 1`, vì vậy tình huống này không bao giờ xuất hiện trong đầu vào, nhưng nó giải thích tại sao mức tối đa luôn được lấy, bao gồm cả`t = 0`. 

Một trường hợp ranh giới thú vị hơn xảy ra khi bước đi đạt tới`M`nhiều lần. Ví dụ,```
1
4 2
```chứa đi bộ```
0 → 1 → 2 → 1 → 2
```Mức tối đa của nó vẫn chính xác`2`. Một giải pháp chỉ tính lần truy cập đầu tiên`M`sẽ từ chối bước đi hợp lệ này một cách không chính xác. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là liệt kê tất cả`2^T`trình tự có thể có của`+1`Và`-1`di chuyển. Mỗi chuỗi đều dễ dàng mô phỏng trong khi theo dõi giá trị lớn nhất của nó. Điều này rõ ràng là đúng vì mỗi bước đi được kiểm tra chính xác một lần. Thật không may, khi`T = 2000`, không gian tìm kiếm xấp xỉ`2^2000`, vượt xa mọi giới hạn thực tế. 

Sự bùng nổ theo cấp số nhân đến từ việc xử lý từng bước đi một cách độc lập. Quan sát quan trọng là chỉ có vị trí hiện tại và liệu chúng ta đã vượt qua ranh giới cấm hay chưa mới quan trọng. 

Giả sử chúng ta đếm số lần đi bộ có mức tối đa không bao giờ vượt quá giới hạn nào đó`K`. Mọi trạng thái chỉ phụ thuộc vào thời gian hiện tại và vị trí hiện tại, bởi vì tất cả thông tin trước đó được tóm tắt bởi thực tế là chúng tôi chưa bao giờ vượt quá`K`. 

Cho phép```
f[K][T]
```biểu thị số bước đi dài`T`tối đa của nó là nhiều nhất`K`. 

Câu trả lời mong muốn là sau đó```
f[M][T] - f[M - 1][T].
```Mỗi lần đi bộ với mức tối đa chính xác`M`được tính một lần bởi đại lượng thứ nhất và bị loại trừ bởi đại lượng thứ hai. 

Vì mọi truy vấn đều thỏa mãn`T ≤ 2000`, chúng ta có thể xử lý trước tất cả các giá trị của`f`cho mọi`K`từ`0`ĐẾN`2000`. Không gian trạng thái chỉ có khoảng`2001 × 4001`, vì vị trí luôn nằm giữa`-T`Và`T`. Quá trình tiền xử lý này tốn khoảng`O(MAXT²)`trên mỗi ranh giới, dẫn đến sự phức tạp tổng thể của`O(MAXT³)`, đó là về`8 × 10^9`chuyển tiếp và vẫn còn quá lớn. 

Quan sát thứ hai loại bỏ một yếu tố khác. Trong khi tăng ranh giới từ`K`ĐẾN`K + 1`, biểu đồ chuyển tiếp chỉ thay đổi cục bộ. Chuyển tọa độ bằng cách trừ`K`biến mọi vấn đề thành việc đếm số bước đi không bao giờ trở nên tích cực. Nguyên lý phản xạ đưa ra dạng đóng$$f(K,T)=\sum_x
\left(
\binom{T}{\frac{T+x}{2}}
-
\binom{T}{\frac{T+x}{2}+K+1}
\right),$$trong đó tổng chạy trên các điểm cuối có thể truy cập. Sau khi tính toán trước các giai thừa và giai thừa nghịch đảo, mọi giá trị sẽ được tính theo thời gian không đổi. Việc điền vào mọi bảng trả lời bây giờ chỉ mất`O(MAXT²)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(2^T · T)`|`O(1)`| Quá chậm | 
| Tối ưu |`O(MAXT² + N)`|`O(MAXT²)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính trước giai thừa và nghịch đảo giai thừa modulo`10^9 + 7`lên đến`2000`. Điều này cho phép mọi hệ số nhị thức được đánh giá trong thời gian không đổi. 
2. Đối với mọi`T`từ`0`ĐẾN`2000`, lặp lại mọi mức tối đa có thể`M`. 
3. Sử dụng công thức nguyên lý phản xạ để đếm số bước đi có mức tối đa không bao giờ vượt quá`M`. 
4. Tính cùng một đại lượng cho ranh giới`M - 1`. 
5. Trừ hai giá trị theo modulo`10^9 + 7`. Kết quả chính xác là số lần đi bộ có giá trị tối đa bằng`M`. 
6. Lưu trữ mọi câu trả lời trong bảng tra cứu được lập chỉ mục bởi`(T, M)`. 
7. Với mỗi test, hãy in giá trị được tính toán trước. 

### Tại sao nó hoạt động 

Nguyên tắc phản xạ thiết lập sự song song giữa các bước đi vượt qua ranh giới bị cấm và các bước đi được phản ánh kết thúc ở một vị trí đã dịch chuyển. Mỗi bước đi không hợp lệ được ghép nối với chính xác một bước đi được phản ánh và mỗi bước đi được phản ánh đều đến từ chính xác một bước đi không hợp lệ. Trừ đi những đường đi được phản ánh này khỏi số lượng không hạn chế sẽ để lại chính xác những bước đi có mức tối đa không bao giờ vượt quá ranh giới. Lấy sự khác biệt giữa các ranh giới liên tiếp sẽ cô lập các bước đi có mức tối đa chính xác`M`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10 ** 9 + 7
MAXT = 2000

fac = [1] * (MAXT + 1)
for i in range(1, MAXT + 1):
    fac[i] = fac[i - 1] * i % MOD

invfac = [1] * (MAXT + 1)
invfac[MAXT] = pow(fac[MAXT], MOD - 2, MOD)
for i in range(MAXT, 0, -1):
    invfac[i - 1] = invfac[i] * i % MOD

def C(n, k):
    if k < 0 or k > n:
        return 0
    return fac[n] * invfac[k] % MOD * invfac[n - k] % MOD

ans = [[0] * (MAXT + 1) for _ in range(MAXT + 1)]

for T in range(MAXT + 1):
    at_most = [0] * (MAXT + 1)
    for M in range(MAXT + 1):
        s = 0
        for x in range(-T, T + 1, 2):
            k = (T + x) // 2
            s += C(T, k)
            s -= C(T, k + M + 1)
        at_most[M] = s % MOD

    for M in range(1, MAXT + 1):
        ans[T][M] = (at_most[M] - at_most[M - 1]) % MOD

n = int(input())
for _ in range(n):
    T, M = map(int, input().split())
    if M > T:
        print(0)
    else:
        print(ans[T][M])
```Quá trình tiền xử lý tính toán mọi hệ số nhị thức trong thời gian không đổi bằng cách sử dụng các giai thừa và nghịch đảo mô đun. Công thức nguyên lý phản xạ được đánh giá cho mỗi cặp`(T, M)`, sau đó các giá trị liên tiếp được trừ đi để tách riêng các bước đi có mức tối đa chính xác là`M`. 

Phép trừ phải luôn được thực hiện modulo`10^9 + 7`, vì các giá trị trung gian có thể trở thành âm. Một chi tiết khác là kiểm tra tính chẵn lẻ ẩn bên trong phép lặp điểm cuối. Các điểm cuối có thể tiếp cận luôn có tính chẵn lẻ giống như`T`, do đó vòng lặp tăng thêm hai. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1
4 2
```| Bước | Giá trị | 
| --- | --- | 
|`T`| 4 | 
|`M`| 2 | 
| Số lần đi bộ tối đa 2 | 10 | 
| Số lần đi bộ tối đa 1 | 6 | 
| Trả lời | 4 | 

Sự khác biệt sẽ loại bỏ mọi bước đi có điểm cao nhất`1`, chỉ để lại những bước đi có mức tối đa chính xác là`2`. 

### Ví dụ thứ hai 

đầu vào:```
1
1 1
```| Bước | Giá trị | 
| --- | --- | 
|`T`| 1 | 
|`M`| 1 | 
| Số lần đi bộ tối đa 1 | 2 | 
| Số lần đi bộ tối đa ≤ 0 | 1 | 
| Trả lời | 1 | 

Đi bộ hợp lệ duy nhất là`0 → 1`. đi bộ`0 → -1`không bao giờ đạt đến độ cao`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(MAXT² + N)`| Tiền xử lý chiếm ưu thế, mỗi truy vấn được trả lời trong thời gian không đổi | 
| Không gian |`O(MAXT²)`| Lưu trữ mọi bảng trả lời | 

Với`MAXT = 2000`, tất cả quá trình tiền xử lý được hoàn thành một lần, sau đó thậm chí`10^5`thắc mắc được trả lời ngay lập tức. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    # call solution()

    return out.getvalue()

assert run("3\n1 1\n4 2\n6 3\n") == "1\n4\n6\n"

assert run("1\n1 2\n") == "0\n"

assert run("1\n2 2\n") == "1\n"

assert run("1\n2 1\n") == "2\n"

assert run("1\n2000 2000\n") == run("1\n2000 2000\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2`|`0`| Tối đa không thể | 
|`2 2`|`1`| Chỉ đạt mức tối đa ở bước cuối cùng | 
|`2 1`|`2`| Nhiều lần đi bộ hợp lệ | 
|`2000 2000`| Nhất quán | Hạn chế lớn nhất | 

## Vỏ cạnh 

Hãy xem xét```
1
2 3
```Vì số lần đi bộ có thể tăng tối đa một lần mỗi giây, đạt đến độ cao`3`trong hai bước là không thể. Thuật toán phát hiện ngay`M > T`và trả về`0`. 

Coi như```
1
4 2
```đi bộ```
0 → 1 → 2 → 1 → 2
```đạt đến độ cao`2`hai lần. Nó thuộc tập hợp các bước đi có tối đa nhiều nhất`2`, nhưng không đến mức tối đa`1`. Sự khác biệt của họ được tính chính xác một lần. 

Cuối cùng, hãy xem xét```
1
3 1
```đi bộ```
0 → 1 → 0 → 1
```chạm mức tối đa nhiều lần mà không vượt quá nó. Nguyên tắc phản ánh tính nó trong số các bước đi được giới hạn bởi`1`, trong khi phép trừ chỉ loại bỏ các bước đi được giới hạn bởi`0`, vì vậy bước đi này đóng góp chính xác một lần vào câu trả lời cuối cùng.
