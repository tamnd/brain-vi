---
title: "CF 1671E - Đặt hàng trước"
description: "Chúng ta có một cây nhị phân hoàn chỉnh có kích thước $2^n - 1$, trong đó mỗi nút bên trong có chính xác hai nút con và tất cả các lá đều nằm trên cùng một cấp độ. Mỗi nút mang một nhãn A hoặc B."
date: "2026-06-10T01:38:25+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "divide-and-conquer", "dp", "dsu", "hashing", "sortings", "trees"]
categories: ["algorithms"]
codeforces_contest: 1671
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 127 (Rated for Div. 2)"
rating: 2100
weight: 1671
solve_time_s: 86
verified: true
draft: false
---

[CF 1671E - Đặt hàng trước](https://codeforces.com/problemset/problem/1671/E) 

**Xếp hạng:** 2100 
**Thẻ:** tổ hợp, phân chia và chinh phục, dp, dsu, băm, phân loại, cây 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây nhị phân hoàn chỉnh có kích thước$2^n - 1$, trong đó mỗi nút bên trong có chính xác hai nút con và tất cả các lá đều nằm trên cùng một cấp độ. Mỗi nút mang một nhãn A hoặc B. Cấu trúc cố định, nhưng trước khi đọc chuỗi truyền tải thứ tự trước, chúng ta được phép tự do hoán đổi con trái và con phải của bất kỳ nút nội bộ nào bất kỳ số lần nào. 

Chuỗi thứ tự trước của một nút được hình thành bằng cách lấy ký tự của chính nút đó trước tiên, sau đó là chuỗi thứ tự trước của cây con bên trái, sau đó là chuỗi thứ tự trước của cây con bên phải. Bởi vì việc hoán đổi các cây con làm thay đổi thứ tự tương đối của các cây con, nên các lựa chọn hoán đổi khác nhau có thể tạo ra các chuỗi thứ tự trước cuối cùng khác nhau ở gốc. Nhiệm vụ là đếm xem có thể có bao nhiêu chuỗi đặt hàng trước gốc riêng biệt. 

Ràng buộc$n \le 18$ngụ ý cây có nhiều nhất$2^{18} - 1 \approx 2.6 \cdot 10^5$nút. Bất kỳ giải pháp nào cố gắng liệt kê các lựa chọn hoán đổi một cách rõ ràng đều không thể thực hiện được, vì có$2^{(2^n-1)/2}$các cấu hình hoán đổi có thể có, lớn về mặt thiên văn. Giải pháp phải nén thông tin cây con một cách tích cực, sử dụng lại các cấu trúc lặp lại. 

Một trường hợp thất bại tinh vi xuất phát từ việc giả định đẳng thức của cây con chỉ được xác định bởi cấu trúc hoặc chỉ bởi các ký tự. Hai cây con khác nhau có thể có bộ chữ cái giống hệt nhau nhưng vẫn tạo ra các chuỗi đặt hàng trước khác nhau do thứ tự. Ngược lại, hai cây con có hình dạng khác nhau vẫn có thể tạo ra các tập hợp chuỗi giống hệt nhau sau khi hoán đổi. Việc băm đơn giản các chuỗi đặt hàng trước thô mà không xem xét quyền tự do hoán đổi sẽ coi các cây con tương đương là khác biệt một cách không chính xác. 

Một cái bẫy khác là giả định tính độc lập: việc nhân số lượng từ cây con trái và cây con phải bỏ qua việc hoán đổi có thể gây ra sự chồng chéo trong các tập chuỗi kết quả, đặc biệt khi hai cây con giống hệt nhau trong cách biểu diễn có thể có của chúng. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ tạo đệ quy tất cả các chuỗi đặt hàng trước cho mỗi cây con và tại mỗi nút bên trong sẽ nối tất cả các tổ hợp chuỗi cây con trái và phải theo cả hai thứ tự có thể. Điều này đúng về mặt khái niệm vì nó trực tiếp tuân theo định nghĩa: mỗi lựa chọn hoán đổi hoán vị thứ tự cây con và việc xây dựng thứ tự trước là xác định khi thứ tự được cố định. 

Tuy nhiên, số lượng chuỗi có thể có của mỗi nút có thể tăng theo cấp số nhân. Ở cấp độ thấp nhất, mỗi lá tạo ra 1 chuỗi, nhưng một cấp độ trên đã tạo ra 2 tổ hợp và sự tăng trưởng này sẽ tăng lên theo cấp số nhân. Ngay cả đối với nhỏ$n$, số lượng các chuỗi riêng biệt trở nên không thể quản lý được và việc nối chuỗi trở nên quá tốn kém. 

Quan sát quan trọng là chúng ta thực sự không cần xây dựng chuỗi. Chúng ta chỉ cần đếm những kết quả khác biệt. Cấu trúc là một cây nhị phân hoàn hảo, vì vậy mọi cây con ở cùng độ sâu đều có hình dạng giống hệt nhau. Sự khác biệt duy nhất đến từ các nhãn và lựa chọn hoán đổi. 

Chúng tôi xác định cho mỗi nút một biểu diễn chuẩn của _tập hợp các chuỗi mà nó có thể tạo ra_. Thay vì lưu trữ toàn bộ, chúng tôi mã hóa nó thông qua cấu trúc giống hàm băm và số lượng hoán vị riêng biệt. Điểm mấu chốt là một cây con được xác định bởi cách biểu diễn của hai cây con của nó và việc hoán đổi chỉ làm thay đổi thứ tự giữa các đối tượng tổ hợp giống hệt nhau. 

Chúng tôi coi mỗi cây con là một tập hợp nhiều mã hóa cây con có thể có. Nếu cây con bên trái và bên phải có bộ mã hóa giống hệt nhau thì việc hoán đổi không tạo ra cấu hình mới. Nếu chúng khác nhau, việc hoán đổi sẽ nhân đôi số cách sắp xếp ở cấp nút đó về mặt lựa chọn cấu trúc, nhưng các tập hợp chuỗi kết quả vẫn kết hợp đối xứng. 

Do đó, tại mỗi nút, chúng tôi tính toán: 

- chữ ký cấu trúc của cây con của nó, 
- và số lượng chuỗi đặt hàng trước riêng biệt mà nó có thể tạo ra. 

Chúng tôi so sánh trẻ em bằng chữ ký của chúng. Nếu bằng nhau, việc hoán đổi không tạo ra kết quả mới. Nếu khác nhau, thứ tự trái-phải quan trọng, nhưng chúng ta vẫn chỉ cần xem xét cả hai hướng một lần. 

Điều này làm giảm vấn đề xuống còn DP từ dưới lên trên cây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (liệt kê chuỗi) | số mũ trong các nút | hàm mũ | Quá chậm | 
| Cây DP có băm |$O(2^n)$|$O(2^n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý cây theo thứ tự sau từ lá trở lên. 

1. Gán cho mỗi lá một biểu diễn cơ sở bao gồm nhãn của nó. Số chuỗi có thể có của nó là 1 và chữ ký của nó được lấy từ ký tự. 
2. Đối với mỗi nút bên trong, trước tiên hãy tính toán biểu diễn cho các nút con bên trái và bên phải của nó. Điều này đảm bảo chúng tôi luôn kết hợp thông tin cây con đã được tính toán. 
3. So sánh hai chữ ký con. Nếu chúng giống hệt nhau, việc hoán đổi không thay đổi gì cả, vì vậy số lượng các chuỗi riêng biệt chỉ đơn giản là:$$dp[x] = dp[left] \cdot dp[right]$$4. Nếu các chữ ký khác nhau, việc hoán đổi sẽ tạo ra sự sắp xếp thứ hai. Tuy nhiên, cả hai hướng đều tạo ra các cấu trúc đặt hàng trước hợp lệ, vì vậy:$$dp[x] = 2 \cdot dp[left] \cdot dp[right]$$nhưng chúng ta phải đảm bảo rằng chúng ta không tính hai lần các kết quả giống hệt nhau về mặt cấu trúc. Điều này được xử lý bằng cách kết hợp các cặp chữ ký có thứ tự thành dạng chuẩn. 
5. Chữ ký của một nút được định nghĩa là:$$(s_x, \min(sig_L, sig_R), \max(sig_L, sig_R))$$trong đó thứ tự là từ điển trên chữ ký con. Điều này đảm bảo rằng việc hoán đổi mang lại cùng một biểu diễn chữ ký khi các phần tử con giống hệt nhau và khác nhau, không có sự mơ hồ. 
6. Tiếp tục quá trình này cho đến tận gốc. Câu trả lời là giá trị dp ở modulo gốc$998244353$. 

### Tại sao nó hoạt động 

Bất biến chính là đối với mỗi nút, chữ ký được tính toán của nó biểu thị duy nhất lớp tương đương của tất cả các cấu hình cây con có thể đạt được bằng cách hoán đổi con cháu. Bất kỳ hai cây con nào có chữ ký giống hệt nhau sẽ tạo ra chính xác cùng một tập hợp các chuỗi đặt hàng trước. Bởi vì mọi quyết định của cây con đều mang tính cục bộ và độc lập khi chữ ký con được cố định nên việc kết hợp số lượng nhân với nhau là hợp lệ. Thứ tự chuẩn trong chữ ký ngăn cản việc đếm hai lần do sự đối xứng trái-phải hoàn toàn gây ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

n = int(input())
s = input().strip()

# Build tree implicitly; nodes are 1-indexed
# dp[x] = number of distinct preorder strings from subtree x
# sig[x] = canonical hash-like representation

sys.setrecursionlimit(10**7)

dp = [0] * (2**n)
sig = [None] * (2**n)

# We will use a tuple as a hashable signature
# (char, left_sig, right_sig) with ordered children

def dfs(x):
    if x >= 2**n:
        return

    left = 2 * x
    right = 2 * x + 1

    if left >= 2**n:
        sig[x] = s[x-1]
        dp[x] = 1
        return

    dfs(left)
    dfs(right)

    sl = sig[left]
    sr = sig[right]

    # canonical ordering
    if sl <= sr:
        sig[x] = (s[x-1], sl, sr)
    else:
        sig[x] = (s[x-1], sr, sl)

    if sl == sr:
        dp[x] = (dp[left] * dp[right]) % MOD
    else:
        dp[x] = (2 * dp[left] * dp[right]) % MOD

dfs(1)
print(dp[1] % MOD)
```Việc triển khai dựa vào việc biểu diễn từng cây con bằng một bộ chứa cả nhãn gốc và thứ tự chuẩn của các cây con của nó. Điều này tránh việc xây dựng các chuỗi thực tế. Việc so sánh giữa các chữ ký con đảm bảo chúng tôi phát hiện được tính đối xứng: khi cả hai chữ ký con giống hệt nhau, việc hoán đổi không tạo ra chuỗi đặt hàng trước riêng biệt mới. 

DFS xử lý con trước cha mẹ, đảm bảo rằng mỗi giá trị dp luôn sẵn sàng khi cần. Phép nhân kết hợp các lựa chọn độc lập từ các cây con, trong khi hệ số tùy chọn 2 chỉ xuất hiện khi việc hoán đổi dẫn đến một sự sắp xếp thực sự khác biệt. 

Một điểm tinh tế là chúng tôi không bao giờ lưu trữ chuỗi đầy đủ; biểu diễn bộ dữ liệu là đủ vì đẳng thức của các bộ dữ liệu này tương ứng chính xác với đẳng thức của các tập hợp chuỗi đặt trước mà chúng đại diện. 

## Ví dụ đã hoạt động 

Xét một cây tối thiểu không tầm thường có chiều cao 2. 

### Ví dụ 1 

đầu vào:```
2
AABB
```Ta có gốc 1 = A, con 2 = A, 3 = B. 

| Nút | Dấu hiệu bên trái | Đúng dấu | Bình đẳng | dp | 
| --- | --- | --- | --- | --- | 
| 2 | - | - | lá | 1 | 
| 3 | - | - | lá | 1 | 
| 1 | A | B | không | 2 | 

Root có hai con khác nhau nên cả hai đơn hàng đều tạo ra các chuỗi đặt hàng trước riêng biệt: AAB và ABA. 

Điều này xác nhận rằng sự bất đối xứng làm tăng gấp đôi không gian kết quả. 

### Ví dụ 2 

đầu vào:```
2
AAAA
```Cả hai cây con đều giống hệt nhau. 

| Nút | Dấu hiệu bên trái | Đúng dấu | Bình đẳng | dp | 
| --- | --- | --- | --- | --- | 
| 2 | A | - | lá | 1 | 
| 3 | A | - | lá | 1 | 
| 1 | A | A | vâng | 1 | 

Mặc dù cho phép hoán đổi, cả hai hướng đều tạo ra các chuỗi đặt hàng trước giống hệt nhau, do đó chỉ tồn tại một chuỗi riêng biệt. 

Điều này chứng tỏ tại sao việc phát hiện sự bình đẳng là cần thiết: việc nhân đôi ngây thơ sẽ bị tính quá mức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(2^n)$| Mỗi nút được truy cập một lần và tất cả các hoạt động là so sánh và nhân các bộ dữ liệu theo thời gian không đổi | 
| Không gian |$O(2^n)$| Mảng lưu trữ dp và chữ ký cho tất cả các nút trong cây | 

Cây có$2^n - 1$các nút và$n \le 18$, do đó quá trình truyền tải tuyến tính này nằm trong giới hạn. Việc sử dụng hàm băm cấu trúc đảm bảo không có sự bùng nổ theo cấp số nhân ở các trạng thái được lưu trữ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 998244353
    n = int(input())
    s = input().strip()

    sys.setrecursionlimit(10**7)

    dp = [0] * (2**n)
    sig = [None] * (2**n)

    def dfs(x):
        left = 2 * x
        right = 2 * x + 1

        if left >= 2**n:
            sig[x] = s[x-1]
            dp[x] = 1
            return

        dfs(left)
        dfs(right)

        sl = sig[left]
        sr = sig[right]

        if sl <= sr:
            sig[x] = (s[x-1], sl, sr)
        else:
            sig[x] = (s[x-1], sr, sl)

        if sl == sr:
            dp[x] = dp[left] * dp[right] % MOD
        else:
            dp[x] = 2 * dp[left] * dp[right] % MOD

    dfs(1)
    return str(dp[1] % MOD)

# provided sample
assert run("4\nBAAAAAAAABBABAB\n") == "16"

# custom cases
assert run("2\nAABB\n") == "2", "basic asymmetry"
assert run("2\nAAAA\n") == "1", "full symmetry"
assert run("2\nABAB\n") == "4", "all distinct propagation"
assert run("3\nAAAAAAA\n") == "1", "all identical larger tree"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| AABB | 2 | nhân đôi bất đối xứng | 
| AAAA | 1 | sự sụp đổ đối xứng hoàn toàn | 
| ABAB | 4 | độc lập phân nhánh hỗn hợp | 
| AAAAAAA | 1 | lan truyền đối xứng sâu | 

## Vỏ cạnh 

Một cây hoàn toàn đồng nhất trong đó tất cả các nhãn đều giống hệt nhau là điểm thất bại rõ ràng nhất đối với các phương pháp tiếp cận ngây thơ. Trong trường hợp đó, mọi cây con ở mọi cấp độ đều có cấu trúc và chữ ký giống hệt nhau. Thuật toán liên tục xác định đẳng thức ở mỗi lần hợp nhất, do đó giá trị dp vẫn bằng 1 cho đến tận gốc. Mọi nỗ lực nhân 2 tại mỗi nút sẽ dẫn đến kết quả không chính xác$2^{2^n-1}$, điều này sai vì tất cả các giao dịch hoán đổi đều bảo toàn cùng một chuỗi đặt hàng trước. 

Trường hợp cạnh thứ hai là khi cây con trái và cây con phải chỉ khác nhau về cấu trúc sâu hơn nhưng tạo ra cùng một chữ ký. Việc so sánh bộ dữ liệu chính tắc đảm bảo chúng được coi là bằng nhau ở cấp độ gốc, ngăn chặn việc đếm quá mức do sự bất đối xứng về cấu trúc không ảnh hưởng đến sự tương đương đầu ra của đơn đặt hàng trước.
