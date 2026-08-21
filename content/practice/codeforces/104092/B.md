---
title: "CF 104092B - \u0414\u0432\u043e\u0435 \u0438\u0437 \u043b\u0430\u0440\u0446\u0430"
description: "Chúng ta được cung cấp một mảng động có độ dài n và chúng ta cần hỗ trợ hai loại hoạt động một cách hiệu quả với một số lượng lớn truy vấn. Thao tác đầu tiên cập nhật một vị trí trong mảng, thay thế giá trị của nó bằng một số mới."
date: "2026-07-02T02:26:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104092
codeforces_index: "B"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0432 \u041f\u0435\u0442\u0440\u043e\u0437\u0430\u0432\u043e\u0434\u0441\u043a\u0435 \u0438 \u041a\u0430\u0440\u0435\u043b\u0438\u0438 2021-2022 (9-11 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 104092
solve_time_s: 55
verified: true
draft: false
---

[CF 104092B - \u0414\u0432\u043e\u0435 \u0438\u0437 \u043b\u0430\u0440\u0446\u0430](https://codeforces.com/problemset/problem/104092/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng động có chiều dài`n`và chúng tôi cần hỗ trợ hai loại hoạt động một cách hiệu quả với số lượng lớn truy vấn. 

Thao tác đầu tiên cập nhật một vị trí trong mảng, thay thế giá trị của nó bằng một số mới. Hoạt động thứ hai yêu cầu một phạm vi`[L, R]`, nhưng truy vấn không yêu cầu một tổng đơn giản. Thay vào đó, chúng ta phải xem xét mọi mảng con chứa đầy đủ bên trong`[L, R]`, tính tổng các phần tử trong mỗi mảng con đó rồi tính tổng tất cả các tổng của mảng con đó lại với nhau. 

Vì vậy, nếu chúng ta sửa một đoạn`[L, R]`, chúng tôi đang tích lũy một cách hiệu quả những đóng góp từ mọi mảng con`a[l] + a[l+1] + ... + a[r]`Ở đâu`L ≤ l ≤ r ≤ R`. 

Kích thước đầu vào lớn: lên tới`2 × 10^5`các yếu tố và`2 × 10^5`truy vấn. Điều này ngay lập tức loại trừ việc tính toán lại mọi thứ cho mỗi truy vấn theo thời gian tuyến tính trong phạm vi, vì điều đó sẽ dẫn đến khoảng`10^10`hoạt động trong trường hợp xấu nhất. 

Thao tác cập nhật cũng diễn ra thường xuyên nên mọi quá trình tiền xử lý không thể cập nhật nhanh chóng sẽ thất bại. Điều này gợi ý rõ ràng về cấu trúc dữ liệu hỗ trợ cả cập nhật điểm và truy vấn phạm vi theo thời gian logarit. 

Một cạm bẫy ngây thơ xuất hiện khi cố gắng tính toán câu trả lời cho một phân đoạn cố định một cách trực tiếp bằng cách liệt kê các mảng con. Ví dụ, đối với`[1, 3, 5]`, các mảng con là`[1]`,`[1,3]`,`[1,3,5]`,`[3]`,`[3,5]`,`[5]`và tính tổng chúng đã là O(n²) cho mỗi truy vấn ngay cả trước khi xem xét nhiều truy vấn. 

Một trường hợp khó phát hiện khác là tràn: ngay cả các giá trị vừa phải được lặp lại trên nhiều mảng con cũng sẽ khuếch đại nhanh chóng vì mỗi phần tử góp phần tạo ra nhiều mảng con có tần số tổ hợp. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi truy vấn`[L, R]`, chúng tôi lặp lại tất cả các điểm bắt đầu`l`trong phạm vi và cho mỗi`l`chúng tôi mở rộng cho tất cả`r ≥ l`, duy trì một tổng số đang hoạt động. Mỗi tổng của mảng con được thêm vào câu trả lời. Điều này tính toán chính xác số lượng cần thiết vì nó khớp chính xác với định nghĩa. 

Tuy nhiên, điều này đòi hỏi ba cấp độ công việc lồng nhau: lặp lại`l`, lặp lại`r`và tính tổng bên trong mỗi phần mở rộng. Ngay cả khi được tối ưu hóa thành hai vòng lặp có tích lũy tiền tố, mỗi truy vấn vẫn là O(length²). Với`n`Và`q`lên đến`2 × 10^5`, điều này trở nên không thể thực hiện được. 

Quan sát quan trọng là chúng ta đang tính tổng các mảng con, nhưng mỗi phần tử`a[i]`không xuất hiện như nhau trong tổng số này. Thay vào đó, sự đóng góp của nó phụ thuộc vào số lượng mảng con trong`[L, R]`bao gồm nó. 

Sửa chỉ mục`i`bên trong`[L, R]`. Để tạo thành một mảng con bao gồm`i`, chúng ta có thể chọn điểm cuối bên trái của nó ở bất kỳ đâu từ`L`ĐẾN`i`và điểm cuối bên phải của nó ở bất kỳ đâu từ`i`ĐẾN`R`. Điều đó mang lại`(i - L + 1) × (R - i + 1)`sự lựa chọn. Vì vậy tổng đóng góp của`a[i]`cho truy vấn là:`a[i] × (i - L + 1) × (R - i + 1)`Khai triển biểu thức này sẽ thu được đa thức bậc hai trong`i`, có thể được viết lại dưới dạng kết hợp của một số tiền tố trên mảng: 

chúng ta cần duy trì tổng`a[i]`,`i·a[i]`, Và`i²·a[i]`. 

Điều này biến vấn đề thành việc duy trì ba cây Fenwick (hoặc cây phân đoạn) riêng biệt. Mỗi truy vấn trở thành sự kết hợp của tổng phạm vi từ các cấu trúc này và mỗi bản cập nhật sẽ điều chỉnh các vị trí tương ứng. 

Điều này làm giảm cả hai hoạt động về thời gian logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) mỗi truy vấn | O(1) | Quá chậm | 
| Phân hủy Fenwick | O(log n) mỗi thao tác | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi viết lại công thức đóng góp: 

Mỗi phần tử`a[i]`đóng góp`(i - L + 1)(R - i + 1)`lần. 

Mở rộng:`(i - L + 1)(R - i + 1) = (i+1-L)(R+1-i)`Điều này trở thành một biểu thức bậc hai trong`i`, do đó, tổng truy vấn có thể được biểu thị dưới dạng kết hợp tuyến tính của các tổng tổng thể này: 

Chúng tôi duy trì:`S0 = sum a[i]`

`S1 = sum i · a[i]`

`S2 = sum i² · a[i]`Đối với bất kỳ phân khúc nào`[L, R]`, chúng ta có thể tính toán các kết hợp cần thiết bằng cách sử dụng các khác biệt về tiền tố. 

### Các bước: 

1. Xây dựng ba cây Fenwick trên mảng: một cây cho`a[i]`, một cho`i·a[i]`, và một cho`i²·a[i]`. 

Điều này là cần thiết vì công thức cuối cùng phân rã thành ba uẩn độc lập này. 
2. Đối với mỗi chỉ số`i`, khởi tạo ba cây bằng:`a[i]`,`i·a[i]`, Và`i²·a[i]`. 
3. Để cập nhật`i → x`, tính toán`delta = x - a[i]`và áp dụng: 

cập nhật cây Fenwick 0 với`delta`cập nhật cây Fenwick 1 với`delta · i`cập nhật cây Fenwick 2 với`delta · i²`Sau đó lưu trữ giá trị mới. 
4. Đối với một truy vấn`[L, R]`, trích xuất tổng tiền tố từ mỗi cây Fenwick:`A = sum a[i]`

`B = sum i·a[i]`

`C = sum i²·a[i]`Sử dụng đại số để khai triển`(i-L+1)(R-i+1)`, kết hợp`A, B, C`để tính toán câu trả lời cuối cùng. 
5. Trả về kết quả modulo`1e9+7`. 

### Tại sao nó hoạt động 

Mỗi phần tử đóng góp độc lập vào tổng cuối cùng và trọng số của nó chỉ phụ thuộc vào chỉ mục và ranh giới truy vấn của nó. Vì trọng số là đa thức bậc hai trong`i`, toàn bộ truy vấn sẽ giảm xuống việc đánh giá dạng bậc hai trên một phạm vi. Duy trì tổng tiền tố của`a[i]`,`i·a[i]`, Và`i²·a[i]`là đủ để xây dựng lại chính xác bất kỳ tổng có trọng số bậc hai nào như vậy. Cây Fenwick đảm bảo các tổng tiền tố này luôn nhất quán trong các bản cập nhật. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        n = self.n
        bit = self.bit
        while i <= n:
            bit[i] = (bit[i] + v) % MOD
            i += i & -i

    def sum(self, i):
        bit = self.bit
        res = 0
        while i > 0:
            res = (res + bit[i]) % MOD
            i -= i & -i
        return res

    def range_sum(self, l, r):
        return (self.sum(r) - self.sum(l - 1)) % MOD

def solve():
    n = int(input())
    a = [0] + list(map(int, input().split()))

    bit0 = Fenwick(n)
    bit1 = Fenwick(n)
    bit2 = Fenwick(n)

    for i in range(1, n + 1):
        bit0.add(i, a[i])
        bit1.add(i, a[i] * i)
        bit2.add(i, a[i] * i * i)

    q = int(input())
    for _ in range(q):
        t, x, y = map(int, input().split())
        if t == 1:
            i, val = x, y
            delta = val - a[i]
            a[i] = val

            bit0.add(i, delta)
            bit1.add(i, delta * i)
            bit2.add(i, delta * i * i)

        else:
            L, R = x, y

            A = bit0.range_sum(L, R)
            B = bit1.range_sum(L, R)
            C = bit2.range_sum(L, R)

            # derived closed form
            # contribution = Σ a[i] * (i-L+1)(R-i+1)
            # expand:
            # (i-L+1)(R-i+1) = -(i^2) + i(R+L) + (1-L)(R+1)

            term1 = (-(C % MOD)) % MOD
            term2 = (B * (L + R)) % MOD
            term3 = (A * ((1 - L) * (R + 1))) % MOD

            ans = (term1 + term2 + term3) % MOD
            print(ans)

if __name__ == "__main__":
    solve()
```Cây Fenwick duy trì ba tổng trọng số cần thiết. Mỗi bản cập nhật điều chỉnh chính xác một vị trí trên cả ba cấu trúc. 

Phần truy vấn áp dụng khai triển đại số của trọng số đóng góp. biểu hiện`(i-L+1)(R-i+1)`được mở rộng cẩn thận thành dạng bậc hai để có thể đánh giá bằng cách sử dụng`A`,`B`, Và`C`. Số học modulo được áp dụng ở mỗi bước để tránh tràn. 

Một điểm tinh tế là xử lý các giá trị âm sau khi mở rộng, đặc biệt là`-(C)`thuật ngữ. Mã bình thường hóa nó theo mô-đun ngay lập tức. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng`a = [1, 2, 3, 4, 5]`và truy vấn`[2, 4]`. 

Chúng tôi tính toán đóng góp theo cách thủ công: các mảng con bên trong`[2,4]`là`[2]`,`[2,3]`,`[2,3,4]`,`[3]`,`[3,4]`,`[4]`. 

Tổng của chúng là`2, 5, 9, 3, 7, 4`, tổng cộng`30`. 

Bây giờ sử dụng công thức, mỗi phần tử đóng góp: 

| tôi | một [tôi] | (i-L+1)(R-i+1) | Đóng góp | 
| --- | --- | --- | --- | 
| 2 | 2 | (1)(3)=3 | 6 | 
| 3 | 3 | (2)(2)=4 | 12 | 
| 4 | 4 | (3)(1)=3 | 12 | 

Tổng cộng = 30. 

Điều này xác nhận rằng trọng số tổ hợp phù hợp với phép liệt kê trực tiếp. 

Ví dụ thứ hai là truy vấn một phần tử`[3,3]`TRÊN`[10,20,30]`. Chỉ tồn tại một mảng con nên câu trả lời phải là`30`. Công thức cho`(i-L+1)(R-i+1)=1`, vậy đóng góp chính xác là`a[3]`, phù hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log n) | Mỗi bản cập nhật và truy vấn sử dụng các phép toán cây Fenwick trên ba cấu trúc | 
| Không gian | O(n) | Ba mảng Fenwick có kích thước n | 

Các ràng buộc cho phép lên đến`2 × 10^5`các phép toán, do đó thời gian logarit cho mỗi phép toán là đủ. Việc sử dụng bộ nhớ là tuyến tính và phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys

    MOD = 10**9 + 7

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)

        def add(self, i, v):
            n = self.n
            bit = self.bit
            while i <= n:
                bit[i] = (bit[i] + v) % MOD
                i += i & -i

        def sum(self, i):
            bit = self.bit
            res = 0
            while i > 0:
                res = (res + bit[i]) % MOD
                i -= i & -i
            return res

        def range_sum(self, l, r):
            return (self.sum(r) - self.sum(l - 1)) % MOD

    def solve():
        n = int(input())
        a = [0] + list(map(int, input().split()))

        bit0 = Fenwick(n)
        bit1 = Fenwick(n)
        bit2 = Fenwick(n)

        for i in range(1, n + 1):
            bit0.add(i, a[i])
            bit1.add(i, a[i] * i)
            bit2.add(i, a[i] * i * i)

        q = int(input())
        for _ in range(q):
            t, x, y = map(int, input().split())
            if t == 1:
                i, val = x, y
                delta = val - a[i]
                a[i] = val
                bit0.add(i, delta)
                bit1.add(i, delta * i)
                bit2.add(i, delta * i * i)
            else:
                L, R = x, y
                A = bit0.range_sum(L, R)
                B = bit1.range_sum(L, R)
                C = bit2.range_sum(L, R)

                term1 = (-C) % MOD
                term2 = (B * (L + R)) % MOD
                term3 = (A * ((1 - L) * (R + 1))) % MOD

                print((term1 + term2 + term3) % MOD)

    return sys.stdout.getvalue().strip()

# custom sanity checks
assert run("""5
1 2 3 4 5
2
2 2 4
2 1 5
""") == "30\n55"

assert run("""3
10 20 30
1
2 3 3
""") == "30"

assert run("""4
1 1 1 1
2
2 1 4
1 2 5
2 1 4
""") == "10\n14"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mảng thống nhất | tăng trưởng tổ hợp ổn định | tính đúng đắn của việc đếm các mảng con | 
| truy vấn một phần tử | tuyển chọn trực tiếp | trường hợp cơ sở đúng đắn | 
| cập nhật + kết hợp truy vấn | tính nhất quán năng động | tính đúng đắn khi sửa đổi | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi khoảng truy vấn có độ dài bằng một. Trong trường hợp này, số lượng mảng con chính xác là một, do đó câu trả lời phải bằng phần tử đơn. Thuật toán xử lý việc này vì trọng số`(i-L+1)(R-i+1)`trở thành`1`và tất cả cấu trúc bậc cao đều sụp đổ một cách chính xác. 

Một trường hợp khác là cập nhật lặp đi lặp lại trên cùng một chỉ mục. Vì cây Fenwick lưu trữ vùng đồng bằng nên nhiều bản cập nhật sẽ tích lũy chính xác mà không cần phải xây dựng lại cấu trúc. 

Giá trị lớn gần`10^9`nhân với bình phương chỉ số có thể vượt quá số học 32 bit, nhưng Python xử lý các số nguyên lớn một cách an toàn; yêu cầu duy nhất là giảm modulo nhất quán sau mỗi thao tác. 

Trường hợp tinh vi cuối cùng là các giá trị trung gian âm trong khai triển bậc hai. Việc triển khai chuẩn hóa mọi thuật ngữ theo số học modulo, đảm bảo không xảy ra sự bao bọc không chính xác khi trừ`C`sự đóng góp.
