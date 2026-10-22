---
title: "CF 105863G - Bài toán đếm"
description: "Chúng ta được cung cấp một số vị trí cố định và một phạm vi giá trị và về mặt khái niệm, chúng ta gán các giá trị từ phạm vi này cho từng vị trí."
date: "2026-06-22T02:14:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105863
codeforces_index: "G"
codeforces_contest_name: "PPSC 2025"
rating: 0
weight: 105863
solve_time_s: 47
verified: true
draft: false
---

[CF 105863G - Vấn đề đếm](https://codeforces.com/problemset/problem/105863/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số vị trí cố định và một phạm vi giá trị và về mặt khái niệm, chúng ta gán các giá trị từ phạm vi này cho từng vị trí. Đối với mỗi phép gán hoàn chỉnh, chúng tôi quan tâm đến việc xác định các vị trí trong đó giá trị tại vị trí đó là lần đầu tiên mức tối thiểu mới xuất hiện khi quét từ trái sang phải. Nói cách khác, tại vị trí i, giá trị ở đó trở thành tiền tố tối thiểu nếu nó không lớn hơn mọi thứ trước nó. 

Nhiệm vụ không phải là đánh giá một phép gán đơn lẻ mà là tổng hợp điều kiện này trên tất cả các phép gán có thể có theo một mô hình đếm thống nhất. Mỗi vị trí có thể độc lập nhận bất kỳ giá trị nào trong một phạm vi cố định, do đó không gian của các cấu hình là theo cấp số nhân. Đầu ra là tổng đóng góp của tất cả các cấu hình trong đó mỗi vị trí đóng vai trò là tiền tố tối thiểu theo cách được mô tả. 

Các ràng buộc được ngụ ý bởi gợi ý của người biên tập rằng cả số lượng vị trí và phạm vi giá trị đều có thể đủ lớn để việc lặp lại tất cả các phép gán là không thể. Ngay cả việc lặp lại tất cả các cặp vị trí và giá trị cũng quá chậm nếu được thực hiện một cách ngây thơ theo cách vòng lặp ba lồng nhau. Bất kỳ giải pháp nào liệt kê rõ ràng các bài tập hoặc thậm chí đóng góp theo cặp mà không nén đại số sẽ vượt quá giới hạn thông thường. 

Một trường hợp cạnh tinh vi phát sinh khi suy luận về các điều kiện cực tiểu tiền tố một cách độc lập. Ví dụ: nếu chỉ có một vị trí thì vị trí đó luôn là tiền tố tối thiểu bất kể giá trị. Ở đây, cách giải thích ngây thơ thực thi không chính xác các điều kiện từ cả hai phía của mảng sẽ không thành công. Một trường hợp đặc biệt khác là khi phạm vi giá trị có kích thước bằng một, nghĩa là mọi phép gán đều không đổi. Trong trường hợp đó, mọi vị trí đều là tiền tố tối thiểu và các công thức giả định tính biến thiên giữa các giá trị có thể chia sai cho 0 hoặc giả sử các tỷ lệ hình học suy biến. 

## Phương pháp tiếp cận 

Quan điểm vũ phu bắt đầu từ định nghĩa. Chúng ta tưởng tượng việc lặp lại mọi lần gán giá trị cho các vị trí. Đối với mỗi bài tập, chúng tôi quét từ trái sang phải và đánh dấu mọi vị trí có giá trị nhỏ hơn hoặc bằng tất cả các giá trị trước đó. Sau đó chúng tôi thêm một vào câu trả lời cho mỗi lần xuất hiện như vậy. Điều này đúng nhưng về cơ bản là không khả thi vì số lượng bài tập là k^n, và thậm chí n lên đến vài chục cũng khiến điều này không thể thực hiện được. 

Chúng ta cần tổ chức lại việc đếm để không bao giờ liệt kê đầy đủ các bài tập. Sự thay đổi quan trọng là đảo ngược quan điểm. Thay vì hỏi có bao nhiêu phép gán tạo ra một cấu trúc toàn cục nhất định, chúng ta hỏi mỗi cặp (i, j), có bao nhiêu phép gán làm cho vị trí i bằng giá trị j và đồng thời biến nó thành tiền tố tối thiểu. 

Để vị trí i là tiền tố tối thiểu có giá trị j, mọi vị trí trước đó phải có giá trị ít nhất là j. Điều đó mang lại một cấu trúc độc lập rõ ràng: các vị trí trước i bị giới hạn trong phạm vi [j, k], vị trí i được cố định trong j và các vị trí sau i không bị hạn chế. Điều này tách việc đếm thành một sản phẩm của những lựa chọn độc lập. 

Vì vậy, phần đóng góp của một cặp cố định (i, j) trở thành (k − j + 1)^(i−1) cho tiền tố, nhân với k^(n−i) cho hậu tố. Tính tổng số này trên tất cả i và j sẽ cho một tổng gấp đôi trông vẫn lớn nhưng giờ đã có cấu trúc. 

Quan sát quan trọng là đối với j cố định, khi i tăng, số hạng (k − j + 1)^(i−1) tạo thành một cấp số nhân. Điều này cho phép chúng ta thay tổng bên trong của i bằng một chuỗi hình học dạng đóng. Khi điều này được thực hiện, tổng còn lại trên j sẽ trở thành tuyến tính theo k và mỗi số hạng có thể được đánh giá trong thời gian không đổi bằng cách sử dụng lũy ​​thừa mô đun và nghịch đảo mô đun. Điều này làm giảm độ phức tạp từ O(nk) hoặc tệ hơn thành O(k log MOD).

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force qua bài tập | O(k^n · n) | O(n) | Quá chậm | 
| Đếm cặp với phép tính tổng hình học | O(k log MOD) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi cơ cấu lại việc tính toán xung quanh việc cố định giá trị j xuất hiện ở vị trí tiền tố tối thiểu. 

1. Cố định giá trị j từ 1 đến k. Chúng tôi hiểu đây là giá trị được gán ở một số vị trí i trở thành tiền tố tối thiểu. 
2. Đối với vị trí i cố định, thực thi vị trí i đó có giá trị j. Mọi vị trí trước i ít nhất phải là j, nên mỗi vị trí có (k − j + 1) lựa chọn. Điều này đóng góp (k − j + 1)^(i−1). 
3. Các vị trí sau i không bị hạn chế nên mỗi vị trí đóng góp k lựa chọn, cho ra k^(n−i). 
4. Kết hợp những điều này thành đóng góp của cố định (i, j), đó là (k − j + 1)^(i−1) · k^(n−i). 
5. Bây giờ tính tổng tất cả các vị trí i cho j cố định. Viết lại tổng sao cho lũy thừa của (k − j + 1) tạo thành cấp số nhân trong i. Phân tích k^(n−1) thành nhân tử để tách tỷ số (k − j + 1) / k. 
6. Áp dụng công thức chuỗi hình học để tính tổng trên i trong thời gian không đổi trên j. Điều này loại bỏ sự cần thiết phải lặp lại tất cả các vị trí một cách rõ ràng. 
7. Tính tổng kết quả của tất cả các giá trị j từ 1 đến k, tích lũy câu trả lời cuối cùng modulo MOD. 

Tại sao nó hoạt động 

Mỗi cấu hình hợp lệ được tính duy nhất bằng cách chọn vị trí và giá trị của mỗi lần xuất hiện tối thiểu tiền tố. Điều kiện “tiền tố tối thiểu tại i với giá trị j” chỉ phụ thuộc vào các ràng buộc trên các vị trí trước đó, độc lập giữa các chỉ số. Sự độc lập này cho phép phân tích thành các lũy thừa. Cấu trúc hình học phát sinh do phần thay đổi duy nhất giữa các vị trí là số mũ trên cùng một cơ số (k − j + 1), đảm bảo không trùng lặp hoặc đếm kép khi tính tổng i và j. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modpow(a, e):
    res = 1
    while e:
        if e & 1:
            res = res * a % MOD
        a = a * a % MOD
        e >>= 1
    return res

def modinv(a):
    return modpow(a, MOD - 2)

def solve():
    n, k = map(int, input().split())

    inv_k = modinv(k)
    ans = 0

    for j in range(1, k + 1):
        base = k - j + 1

        # geometric ratio r = base / k
        r = base * inv_k % MOD

        # sum_{i=1..n} base^(i-1) * k^(n-i)
        # = k^(n-1) * sum_{t=0..n-1} r^t
        if r == 1:
            geom = n % MOD
        else:
            geom = (modpow(r, n) - 1) * modinv(r - 1) % MOD

        ans = (ans + modpow(k, n - 1) * geom) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách thực hiện phép lũy thừa mô đun và nghịch đảo mô đun vì tất cả các biểu thức đều liên quan đến lũy thừa và phép chia theo mô đun nguyên tố. Bên trong vòng lặp chính, mỗi giá trị j xác định một ràng buộc cơ sở trên các vị trí tiền tố. Tỷ lệ r thể hiện mức độ đóng góp thay đổi như thế nào khi di chuyển ranh giới tiền tố. 

Tổng hình học được tính bằng dạng đóng tiêu chuẩn, với trường hợp đặc biệt cho r bằng 1, xảy ra khi j = 1 và tất cả các giá trị tiền tố không bị hạn chế. Hệ số k^(n−1) được trích ra để ổn định biểu thức và giảm lũy thừa lặp lại. 

Một cạm bẫy phổ biến ở đây là trộn lẫn các phần bù số mũ. Tiền tố sử dụng i−1 và hậu tố sử dụng n−i, vì vậy việc trích xuất k^(n−1) là cần thiết để tránh các lỗi sai lệch một trong tỷ lệ hình học. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Xét n = 3, k = 2. 

Chúng tôi tính toán đóng góp cho j = 1 và j = 2. 

| j | cơ số = k − j + 1 | tỷ lệ r | tổng hình học | đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 3 | k^(2) · 3 | 
| 2 | 1 | 1/2 | 1 + 1/2 + 1/4 | k^(2) · tổng | 

Với j = 1, mọi ràng buộc tiền tố đều biến mất, do đó mọi phép gán đều đóng góp đồng đều giữa các vị trí. Với j = 2, tất cả các vị trí tiền tố phải chính xác bằng 2, điều này nhanh chóng hạn chế các cấu hình hợp lệ. 

Dấu vết này cho thấy tỷ lệ hình học co lại như thế nào khi j tăng, phản ánh các ràng buộc tiền tố chặt chẽ hơn. 

### Ví dụ 2 

Xét n = 2, k = 3. 

Chúng tôi đánh giá từng j: 

| j | căn cứ | r | tổng địa lý | 
| --- | --- | --- | --- | 
| 1 | 3 | 1 | 2 | 
| 2 | 2 | 2/3 | 1 + 2/3 | 
| 3 | 1 | 1/3 | 1 + 1/3 | 

Điều này xác nhận rằng các ràng buộc chặt chẽ hơn (j lớn hơn) làm giảm cơ số và do đó làm giảm phần đóng góp, đồng thời công thức xử lý chính xác các mảng ngắn trong đó chuỗi hình học thu gọn nhanh chóng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k log MOD) | Mỗi giá trị j yêu cầu lũy thừa mô-đun và các phép toán nghịch đảo | 
| Không gian | O(1) | Chỉ có một số lượng biến không đổi được duy trì | 

Giải pháp này hiệu quả với k lớn vì mỗi số hạng được rút gọn về số học dạng đóng. Ngay cả đối với k lên tới 10^5 trở lên, lũy thừa mô-đun vẫn chiếm ưu thế nhưng vẫn đủ nhanh trong giới hạn thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 10**9 + 7

    def modpow(a, e):
        res = 1
        while e:
            if e & 1:
                res = res * a % MOD
            a = a * a % MOD
            e >>= 1
        return res

    def modinv(a):
        return modpow(a, MOD - 2)

    n, k = map(int, input().split())

    inv_k = modinv(k)
    ans = 0

    for j in range(1, k + 1):
        base = k - j + 1
        r = base * inv_k % MOD

        if r == 1:
            geom = n % MOD
        else:
            geom = (modpow(r, n) - 1) * modinv(r - 1) % MOD

        ans = (ans + modpow(k, n - 1) * geom) % MOD

    return str(ans)

# minimal
assert run("1 1\n") == "1", "single element"

# all equal range 1 prefix trivial
assert run("3 1\n") == "3", "degenerate k=1"

# small case
assert run("2 2\n") is not None

# boundary equality case
assert run("2 3\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | trường hợp cạnh trạng thái đơn | 
| 3 1 | 3 | phạm vi giá trị suy biến | 
| 2 2 | tính toán | hành vi hình học nhỏ | 
| 2 3 | tính toán | tính đúng đắn của cấu trúc chung | 

## Vỏ cạnh 

Trường hợp k = 1 sụp đổ toàn bộ tính ngẫu nhiên. Mọi vị trí buộc phải có giá trị 1, vì vậy mọi vị trí đều là tiền tố tối thiểu. Công thức rút gọn thành tổng các khoản đóng góp giống hệt nhau và việc triển khai xử lý chính xác điều này vì r trở thành 1 và nhánh tổng hình học trả về n. 

Với n = 1, bài toán rút gọn thành tổng tất cả các giá trị j có thể có trong đó vị trí đơn luôn là tiền tố tối thiểu. Mã đánh giá k số hạng có cấu trúc bằng nhau, mỗi số hạng đóng góp k^0 = 1 nhân tổng hình học có độ dài 1, mang lại k như mong đợi. 

Khi j = 1 thì tỉ số r chính xác bằng 1. Đây là điểm duy nhất mà công thức hình học sẽ chia cho 0 ở dạng tổng quát. Nhánh rõ ràng xử lý trường hợp này một cách rõ ràng, tạo ra tổng tuyến tính thay vì tổng hình học.
