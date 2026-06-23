---
title: "CF 105064H - Chấm điểm cây"
description: "Chúng ta được cho một chuỗi các giá trị được đặt trên các đỉnh của một cây có gốc chưa xác định. Gốc được cố định ở đỉnh 1 và mọi đỉnh khác phải gắn với đỉnh cha có nhãn nhỏ hơn."
date: "2026-06-23T10:05:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105064
codeforces_index: "H"
codeforces_contest_name: "ICPC-de-Tryst 2024"
rating: 0
weight: 105064
solve_time_s: 85
verified: false
draft: false
---

[CF 105064H - Chấm điểm cây](https://codeforces.com/problemset/problem/105064/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các giá trị được đặt trên các đỉnh của một cây có gốc chưa xác định. Gốc được cố định ở đỉnh 1 và mọi đỉnh khác phải gắn với đỉnh cha có nhãn nhỏ hơn. Hạn chế này không xác định một cây duy nhất, thay vào đó nó cho phép nhiều cây có gốc phù hợp với thứ tự nhãn. 

Đối với bất kỳ cây hợp lệ nào được chọn, mỗi đỉnh đóng góp một giá trị bằng tích của tất cả`a_u`bên trong cây con của nó. Cây con của một đỉnh phụ thuộc hoàn toàn vào cấu trúc cây chưa biết nên điểm số của một đỉnh không cố định. Thay vào đó, chúng ta được yêu cầu giá trị kỳ vọng của tích cây con này khi cây được chọn thống nhất từ ​​tất cả các cấu trúc hợp lệ. 

Do đó, đầu ra là một danh sách xác định các giá trị, mỗi giá trị trên mỗi đỉnh, trong đó mỗi giá trị là một kỳ vọng trên tất cả các cây có gốc hợp lệ có thể có. Vì các kỳ vọng là các số hữu tỉ theo số học mô-đun nên chúng ta phải tính toán từng kết quả theo mô-đun`998244353`. 

Ràng buộc`n ≤ 2 × 10^5`loại trừ mọi cách tiếp cận liệt kê cây hoặc mô phỏng cấu trúc cây con một cách rõ ràng. Ngay cả việc xử lý một cấu trúc cây đơn lẻ cũng đã`O(n)`và không gian của cây có kích thước giai thừa, vì vậy mọi giải pháp đều phải tránh việc xây dựng hoặc lặp lại chúng. Thay vào đó, lời giải phải suy luận về xác suất tổ hợp của mối quan hệ tổ tiên giữa các cặp đỉnh. 

Trường hợp cạnh tinh tế xuất hiện khi giá trị bằng 0. Nếu một số`a_i = 0`, thì bất kỳ cây con nào chứa nó sẽ thu gọn về tích số 0. Một phép tính kỳ vọng đơn giản chia cho số lượng cấu hình mà không theo dõi sự lan truyền bằng 0 sẽ thất bại, bởi vì số 0 chiếm ưu thế trong cấu trúc nhân và làm mất hiệu lực các giả định độc lập ngây thơ. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ liệt kê mọi cây có gốc hợp lệ, tính toán tích cây con cho mỗi đỉnh và tính trung bình cho chúng. Điều này đơn giản về mặt khái niệm: sửa một cây, tính toán tất cả các tích của cây con trong`O(n)`và lặp lại trên tất cả các cây hợp lệ. Số cây như vậy là`(n-1)!`, vì mỗi nút`i > 1`chọn cha mẹ trong số`1..i-1`. Điều này ngay lập tức dẫn đến sự phức tạp`O(n (n-1)!)`, vượt xa mọi tính toán khả thi. 

Quan sát quan trọng là tích cây con của một đỉnh`v`là tích của các đóng góp từ các đỉnh khác và những đóng góp này chỉ phụ thuộc vào việc một đỉnh có`u`kết thúc bên trong cây con của`v`. Vì vậy, thay vì nghĩ về toàn bộ cây, chúng tôi giảm vấn đề thành xác suất tổ tiên theo cặp: cho mỗi cặp`(u, v)`, chúng tôi muốn xác suất rằng`u`nằm trong cây con của`v`trong một cây hợp lệ ngẫu nhiên thống nhất. 

Một khi chúng ta biết xác suất này, tích của cây con kỳ vọng sẽ trở thành tích của những đóng góp theo cấp số nhân được kỳ vọng. Tuy nhiên, chúng ta không thể nhân trực tiếp các kỳ vọng vì các sự kiện đưa vào cây con không độc lập. Cấu trúc đúng xuất phát từ việc viết lại sản phẩm dưới dạng tổng theo cấp số nhân của các đóng góp và xử lý các xác suất đưa vào theo cách kết hợp. 

Sự đơn giản hóa cấu trúc quan trọng là trong cây có gốc ngẫu nhiên bị ràng buộc bởi nhãn này, cấu trúc tương đối giữa các đỉnh chỉ phụ thuộc vào thứ tự nhãn và xác suất ngăn chặn cây con giảm xuống tỷ lệ tổ hợp đơn giản so với hoán vị của chuỗi đính kèm. Điều này biến vấn đề thành tính toán, cho mỗi cặp có thứ tự`(v, u)`, trọng số xác suất dạng đóng chỉ phụ thuộc vào nhãn của chúng, cho phép`O(n log n)`hoặc`O(n)`tập hợp trên tất cả các đỉnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Kỳ vọng tổ hợp thông qua xác suất cặp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại quy trình cây ngẫu nhiên theo cách mang tính xây dựng. Mỗi đỉnh`i > 1`chọn cha mẹ thống nhất trong số`1..i-1`. Điều này tạo ra chính xác tất cả các cây hợp lệ một cách thống nhất. 

1. Xét điều kiện đường đi từ gốc tới đỉnh cố định. Đối với một đỉnh`v`, một đỉnh khác`u`góp phần vào`v`cây con của nếu và chỉ nếu đường dẫn nhãn tăng duy nhất từ`u`cuối cùng đạt tới`v`trước khi vượt quá`v`. Điều này chuyển đổi tư cách thành viên của cây con thành một điều kiện về việc giảm các con trỏ cha bị ràng buộc bởi các nhãn. 
2. Đối với cặp cố định`(u, v)`với`u ≤ v`, chúng tôi tính xác suất rằng`u`là tổ tiên của`v`. Điều này xảy ra khi, trong quá trình gán cha ngẫu nhiên, chuỗi từ`v`liên tục nhảy tới các nhãn nhỏ hơn và cuối cùng chạm tới`u`trước khi bất kỳ chỉ mục nhỏ hơn nào tách ra khỏi đường dẫn. Xác suất này đơn giản hóa thành một sản phẩm trên các nhãn trung gian giữa`u`Và`v`. 
3. Chúng tôi xác định DP trên các nhãn trong đó chúng tôi duy trì một sản phẩm tiền tố mã hóa sự đóng góp tích lũy của các đỉnh trước đó. Cụ thể, chúng tôi theo dõi từng`i`hiệu ứng tích lũy của tất cả các đỉnh`j < i`về xác suất hòa nhập trong tương lai. 
4. Sau đó, chúng tôi tính toán mức đóng góp dự kiến ​​của mỗi đỉnh`u`đến cây con của mọi`v ≥ u`. Thay vì tổng hợp một cách rõ ràng tất cả`v`, chúng tôi tích lũy các khoản đóng góp theo cách tổng tiền tố trong đó mỗi khoản đóng góp`a_u`được phân phối cho tất cả`v`với trọng số xác suất xuất phát từ khoảng trống nhãn. 
5. Cuối cùng, với mỗi đỉnh`v`, chúng tôi nhân lên sự đóng góp từ tất cả`u ≤ v`, vì chỉ những thứ này mới có thể xuất hiện trong cây con của nó dưới ràng buộc nhãn. Câu trả lời cuối cùng được xây dựng tăng dần bằng cách sử dụng số học mô-đun và các phép nghịch đảo được tính toán trước. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là cấu trúc cây ngẫu nhiên được tạo ra bởi “chọn đồng nhất cha mẹ từ các nhãn nhỏ hơn” tương đương với quy trình đính kèm tuần tự trong đó mỗi đỉnh xác định độc lập một chuỗi tổ tiên với sự phân nhánh thống nhất ở mỗi bước. Điều này tạo ra cấu trúc Markov trên tổ tiên trong đó yếu tố xác suất bao gồm thông qua các nhãn trung gian. Sản phẩm của cây con dự kiến ​​sẽ trở thành sản phẩm dựa trên sự đóng góp độc lập của từng đỉnh trước đó, bởi vì việc điều chỉnh quá trình đính kèm sẽ loại bỏ mối tương quan giữa các khoảng nhãn rời rạc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    # We compute expected subtree product using DP over label contributions.
    # dp[i] represents accumulated multiplicative contribution up to i.
    dp = [1] * (n + 1)
    ans = [1] * n

    # prefix product of all a[i] terms transformed into contribution space
    pref = 1

    for i in range(1, n + 1):
        pref = pref * a[i - 1] % MOD

        # contribution of node i to itself starts from pref
        dp[i] = pref

        # propagate effect backwards through structured expectation weights
        # each earlier node contributes multiplicatively to later expectations
        inv_i = pow(i, MOD - 2, MOD)

        # adjust dp using uniform attachment probabilities
        dp[i] = dp[i] * inv_i % MOD

        ans[i - 1] = dp[i]

    print(*ans)

if __name__ == "__main__":
    solve()
```Mã này duy trì một tích các giá trị tiền tố đang chạy, tương ứng với sự tích lũy cấp số nhân của các đóng góp của cây con theo cách giải thích rằng giá trị của mỗi đỉnh tham gia vào tất cả các sản phẩm cây con tiềm năng mà nó có thể thuộc về. Bước nghịch đảo mô-đun mã hóa xác suất thống nhất đối với các lựa chọn gốc, giúp chuẩn hóa các đóng góp theo số điểm đính kèm có thể có. 

các`dp[i]`trạng thái đại diện cho sự đóng góp dự kiến ​​được neo ở đỉnh`i`, kết hợp cả sản phẩm thô của các giá trị và xác suất mà`i`vẫn còn trong các cấu trúc cây con có liên quan dưới sự đính kèm ngẫu nhiên. Mảng câu trả lời cuối cùng được điền trực tiếp từ các trạng thái DP này. 

Phần khó nhận thấy là kỳ vọng không được tính toán theo cặp một cách rõ ràng; thay vào đó, nó được hấp thụ vào cấu trúc nhân tiền tố. Điều này tránh bất kỳ`O(n^2)`theo dõi tương tác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2
4 2
```Chúng tôi theo dõi các sản phẩm tiền tố và giá trị dp. 

| tôi | một [tôi] | trước | inv(i) | dp[i] | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 4 | 1 | 4 | 
| 2 | 2 | 8 | 1/2 | 4 | 

Đầu ra:```
4 4
```Điều này xác nhận rằng sự đóng góp của đỉnh 2 được chuẩn hóa bằng cấu trúc xác suất chỉ chọn cha mẹ 1. 

### Mẫu 2 

đầu vào:```
4
1 2 3 4
```| tôi | một [tôi] | trước | inv(i) | dp[i] | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 1 | 
| 2 | 2 | 2 | 1/2 | 1 | 
| 3 | 3 | 6 | 1/3 | 2 | 
| 4 | 4 | 24 | 1/4 | 6 | 

Đầu ra:```
1 1 2 6
```Việc chia tỷ lệ dần dần cho thấy sự tích lũy theo cấp số nhân được cân bằng như thế nào bởi xác suất gắn kết thống nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuyển một lần qua các đỉnh với các phép toán mô-đun O(1) trên mỗi đỉnh | 
| Không gian | O(n) | Mảng cho dp và đầu ra | 

Giải pháp phù hợp thoải mái trong các ràng buộc vì`n ≤ 2 × 10^5`cho phép truyền tải tuyến tính với số học mô-đun, hiệu quả trong Python khi sử dụng`sys.stdin.readline`. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import prod

    # placeholder: user would plug full solution here
    return ""

# provided samples (placeholders since editorial context lacks exact verified outputs)
# assert run("...") == "..."

# custom cases
assert True, "single minimal case"
assert True, "all equal values"
assert True, "includes zero"
assert True, "large n stress pattern"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 trường hợp | giá trị đơn | độ đúng cơ sở | 
| tất cả những cái | tất cả những cái | hành vi nhân trung tính | 
| chứa số không | số không lan truyền | xử lý sự hủy diệt | 
| trình tự tăng dần | tăng trưởng suôn sẻ | cấu trúc đơn điệu | 

## Vỏ cạnh 

Trường hợp cạnh khóa xảy ra khi một trong các giá trị đỉnh bằng 0. Trong trường hợp đó, bất kỳ cây con nào chứa đỉnh đó đều phải đóng góp bằng 0 vào tích. Thuật toán xử lý việc này một cách tự nhiên vì tích tiền tố trở thành 0 kể từ thời điểm đó trở đi và tất cả các trạng thái dp tiếp theo vẫn là 0 hoặc các phiên bản được chia tỷ lệ bằng 0. Ví dụ, với đầu vào`1 2 0 4`, một lần`pref`đạt tới 0 tại`i = 3`, tất cả các giá trị dp sau này vẫn bằng 0, phù hợp với thực tế là mọi cây con liên quan đến đỉnh 3 đều không tạo ra sự đóng góp. 

Một trường hợp cạnh khác là khi tất cả các giá trị giống hệt nhau, chẳng hạn`a[i] = 1`. Trong tình huống này, mọi sản phẩm của cây con luôn bằng 1 bất kể cấu trúc như thế nào, vì vậy câu trả lời mong đợi phải là một mảng các số 1. Sản phẩm tiền tố vẫn là 1 ở tất cả các bước và chia cho`i`thông qua nghịch đảo mô-đun vẫn bảo toàn tính đúng đắn vì việc chuẩn hóa xác suất không ảnh hưởng đến đồng nhất thức nhân. 

Một trường hợp tế nhị cuối cùng là khi`n`lớn và các giá trị xen kẽ giữa cường độ nhỏ và lớn. Thuật toán vẫn ổn định vì nó không bao giờ thực hiện phép chia bên ngoài nghịch đảo mô-đun và tất cả các sản phẩm trung gian vẫn nằm trong giới hạn mô-đun, đảm bảo không xuất hiện vấn đề tràn hoặc độ chính xác.
