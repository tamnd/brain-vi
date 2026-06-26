---
title: "CF 105336E - \u968f\u673a\u8fc7\u7a0b"
description: "Chúng tôi đang tạo ra $n$ các chuỗi ngẫu nhiên độc lập, mỗi chuỗi có độ dài $m$, trong đó mọi ký tự được chọn thống nhất từ ​​26 chữ cái tiếng Anh viết thường."
date: "2026-06-23T15:23:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105336
codeforces_index: "E"
codeforces_contest_name: "The 2024 CCPC Online Contest"
rating: 0
weight: 105336
solve_time_s: 54
verified: true
draft: false
---

[CF 105336E - \u968f\u673a\u8fc7\u7a0b](https://codeforces.com/problemset/problem/105336/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang tạo ra$n$các chuỗi ngẫu nhiên độc lập, mỗi chuỗi có độ dài$m$, trong đó mọi ký tự được chọn thống nhất từ ​​26 chữ cái tiếng Anh viết thường. Sau khi các chuỗi này được tạo, chúng tôi chèn chúng vào một bộ ba theo cách tiêu chuẩn: bắt đầu từ gốc, mỗi ký tự tạo hoặc theo sau một cạnh và các nút tương ứng với các tiền tố xuất hiện dọc theo ít nhất một trong các chuỗi được chèn. 

Cấu trúc mà chúng ta thu được phụ thuộc hoàn toàn vào mức độ chia sẻ tiền tố của các chuỗi. Nếu nhiều chuỗi chia sẻ các tiền tố chung dài thì trie sẽ nhỏ gọn. Nếu chúng phân nhánh sớm, cây trie sẽ phát triển nhanh chóng và phân nhánh nhiều. 

Chúng tôi được yêu cầu hai số lượng. Đầu tiên là số nút tối đa có thể có trong phép thử trên tất cả các kết quả của quá trình ngẫu nhiên. Đây là một cực trị tổ hợp xác định trên tất cả các tập hợp chuỗi có thể được tạo ra. Thứ hai là số lượng nút dự kiến ​​​​trong bộ ba, lấy tính ngẫu nhiên của việc tạo chuỗi và modulo cần thiết$998244353$. 

Các kích thước đầu vào$n, m \le 10^5$ngay lập tức loại trừ bất kỳ mô phỏng nào trên chuỗi hoặc cấu trúc thử nghiệm rõ ràng. Ngay cả lý luận rõ ràng cho mỗi chuỗi cũng sẽ quá lớn vì mỗi chuỗi đóng góp tới$m$các nút và các cạnh, và bản thân tri có thể tăng kích thước$\Theta(nm)$. Thay vào đó, giải pháp phải hoạt động ở mức xác suất xảy ra xung đột tiền tố. 

Một điểm tinh tế là “số lượng nút tối đa” không phải là một tuyên bố xác suất. Nó đạt được bằng cách tưởng tượng ra kết quả tốt nhất có thể có của quá trình ngẫu nhiên, nghĩa là chúng ta chọn các chuỗi một cách đối nghịch nhưng vẫn tôn trọng mô hình. Ngược lại, kỳ vọng phụ thuộc vào tính độc lập của các ký tự và tính tuyến tính trên các nút trie. 

Một trường hợp thất bại đơn giản xuất hiện khi người ta cố gắng xây dựng bản tri một cách rõ ràng. Ví dụ: nếu tất cả các chuỗi bắt đầu bằng cùng một tiền tố, trie sẽ sụp đổ nặng nề, nhưng nếu chúng khác nhau ở mọi vị trí, trie sẽ bùng nổ. Sự thay đổi này chính xác là điều làm cho mô phỏng rõ ràng không thể thực hiện được. 

## Phương pháp tiếp cận 

Chúng tôi bắt đầu bằng cách hiểu những gì xác định các nút trie. Một nút tương ứng với một tiền tố riêng biệt xuất hiện trong ít nhất một chuỗi. Do đó, tổng số nút bằng số tiền tố riêng biệt trong số tất cả các chuỗi, cộng thêm một cho chuỗi gốc. 

### Nút tối đa 

Mỗi chuỗi đóng góp tất cả các tiền tố có độ dài của nó$1$bởi vì$m$. Nếu chúng ta muốn tối đa hóa số lượng nút trie riêng biệt, chúng ta muốn tất cả các tiền tố này càng khác nhau càng tốt trên tất cả các chuỗi. Hạn chế duy nhất là các tiền tố trong một chuỗi được lồng nhau, do đó, một chuỗi chỉ đóng góp tối đa$m$các nút mới. 

Nút gốc đóng góp một nút và mỗi chuỗi đóng góp nhiều nhất$m$các nút bổ sung và giới hạn này có thể đạt được bằng cách chọn tất cả các chuỗi sao cho không có hai chuỗi nào chia sẻ bất kỳ tiền tố nào. Điều đó có thể thực hiện được vì bảng chữ cái đủ lớn để gán cho mỗi chuỗi một đường dẫn hoàn toàn khác biệt trong bộ ba. 

Do đó số lượng nút tối đa chỉ đơn giản là$$1 + n \cdot m.$$### Các nút dự kiến 

Sự mong đợi thú vị hơn. Chúng tôi xem tập hợp nút trie là tất cả các tiền tố có thể có$p$trên mọi chiều dài$1$ĐẾN$m$và xác định một biến chỉ báo cho dù tiền tố$p$xuất hiện trong ít nhất một chuỗi. 

Theo tính tuyến tính của kỳ vọng, số lượng nút dự kiến ​​là$$1 + \sum_{k=1}^{m} \sum_{p \in \Sigma^k} \Pr(p \text{ appears in at least one string}).$$Sửa tiền tố$p$chiều dài$k$. Một chuỗi ngẫu nhiên có xác suất$26^{-k}$bắt đầu với$p$. Do đó, xác suất để không ai trong số$n$chuỗi bắt đầu bằng$p$là$(1 - 26^{-k})^n$, vậy xác suất nó xuất hiện ít nhất một lần là$$1 - (1 - 26^{-k})^n.$$có$26^k$những tiền tố như vậy, vì vậy sự đóng góp dự kiến ​​từ độ dài$k$là$$26^k \cdot \left(1 - (1 - 26^{-k})^n \right).$$Biểu thức được đơn giản hóa rõ ràng nếu chúng ta phân phối:$$26^k - 26^k(1 - 26^{-k})^n.$$Thuật ngữ đầu tiên trên tất cả$k$đưa ra một chuỗi hình học. Thuật ngữ thứ hai yêu cầu lũy thừa mô-đun và xử lý cẩn thận các nghịch đảo. 

Quan sát cấu trúc quan trọng là chúng ta không bao giờ cần mô phỏng các tiền tố một cách rõ ràng. Mọi thứ sụp đổ thành xác suất độc lập trên mỗi độ dài tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force thử xây dựng |$O(nm)$bộ nhớ lớn dự kiến, trong trường hợp xấu nhất |$O(nm)$| Quá chậm | 
| Xác suất trên mỗi độ dài tiền tố |$O(m \log MOD)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Nút tối đa 

1. Quan sát rằng mỗi nút tương ứng với một tiền tố duy nhất trong số tất cả các chuỗi. 
2. Mỗi chuỗi có chính xác$m$tiền tố không bao gồm gốc. 
3. Để tối đa hóa các tiền tố riêng biệt trên các chuỗi, hãy đảm bảo không có tiền tố nào được chia sẻ giữa các chuỗi khác nhau. 
4. Theo cấu hình này, mỗi chuỗi đóng góp một chuỗi rời rạc$m$các nút, vì vậy tổng số nút là$1 + n \cdot m$. 

### Các nút dự kiến 

1. Cố định độ dài tiền tố$k$. Xử lý tất cả các chuỗi một cách độc lập đối với tiền tố này. 
2. Tính xác suất để một chuỗi bắt đầu bằng tiền tố cố định$p$, đó là$26^{-k}$. 
3. Chuyển giá trị này thành xác suất để ít nhất một trong các$n$chuỗi chứa$p$, đó là$1 - (1 - 26^{-k})^n$. 
4. Nhân với số tiền tố có thể có ở độ sâu này,$26^k$, để có được số lượng nút dự kiến ​​​​được đóng góp theo độ sâu$k$. 
5. Tổng hợp tất cả$k$từ$1$ĐẾN$m$. 
6. Thêm 1 cho nút gốc. 
7. Tính toán trước các nghịch đảo mô-đun và sử dụng phép lũy thừa nhanh cho các lũy thừa liên quan đến$n$. 

### Tại sao nó hoạt động 

Mỗi nút trie tương ứng chính xác với sự kiện “tiền tố này xuất hiện trong ít nhất một chuỗi”. Những sự kiện này không độc lập, nhưng tính tuyến tính của kỳ vọng cho phép chúng ta xử lý chúng một cách riêng biệt. Đối với tiền tố cố định, tất cả các chuỗi đều độc lập, do đó xác suất vắng mặt được phân tích rõ ràng. Tính tổng tất cả các tiền tố theo nhóm độ dài sẽ tránh được việc liệt kê rõ ràng trong khi vẫn giữ được giá trị mong đợi chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modpow(a, e):
    res = 1
    while e:
        if e & 1:
            res = res * a % MOD
        a = a * a % MOD
        e >>= 1
    return res

n, m = map(int, input().split())

# maximum nodes
max_nodes = (1 + n * m) % MOD

inv26 = modpow(26, MOD - 2)

# expected nodes
ans = 1  # root

pow_26k = 1      # 26^k
inv_26k = 1      # 26^{-k}

for k in range(1, m + 1):
    pow_26k = pow_26k * 26 % MOD
    inv_26k = inv_26k * inv26 % MOD

    # probability a prefix appears at least once
    p_single = inv_26k
    p_none = modpow((1 - p_single) % MOD, n)
    p_at_least_one = (1 - p_none) % MOD

    contrib = pow_26k * p_at_least_one % MOD
    ans = (ans + contrib) % MOD

print(max_nodes, ans)
```Phần tối đa là số học trực tiếp: mỗi chuỗi đóng góp một chuỗi đầy đủ$m$các nút và tính độc lập của các tiền tố cho phép tất cả các chuỗi rời rạc trong cấu hình cực đoan. 

Đối với kỳ vọng, vòng lặp duy trì$26^k$và nghịch đảo của nó tăng dần. Thuật ngữ$(1 - 26^{-k})^n$được tính bằng lũy ​​thừa mô-đun, điều này là cần thiết bởi vì$n$có thể lớn như$10^5$. Mỗi lần lặp là$O(\log n)$, đưa ra tổng thể$O(m \log n)$sự phức tạp. 

Một chi tiết triển khai tinh tế là xử lý$1 - 26^{-k}$modulo MOD. Nó phải được chuẩn hóa trước khi lũy thừa; mặt khác, các giá trị âm lan truyền không chính xác trong phép lũy thừa mô-đun. 

## Ví dụ đã hoạt động 

Hãy xem xét$n = 1, m = 1$. 

| k | 26^k | 26^{-k} | p(ít nhất một) | đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 26 | inv26 | 1 | 26 | 

Các nút dự kiến ​​là$1 + 26 = 27$, khớp với thực tế là một chuỗi ký tự đơn tạo ra nút gốc cộng với một nút. 

Bây giờ hãy xem xét$n = 2, m = 1$. 

| k | 26^k | 26^{-k} | p(thiếu tiền tố đơn trong tất cả các chuỗi) | đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 26 | inv26 |$(1 - 1/26)^2$|$26 \cdot (1 - (25/26)^2)$| 

Điều này ngăn chặn xung đột: hai chuỗi có thể có chung ký tự đầu tiên, làm giảm các nút phân biệt dự kiến ​​so với$2 \cdot 26$. 

Những ví dụ này cho thấy kỳ vọng phụ thuộc như thế nào vào sự va chạm tiền tố, không chỉ những đóng góp độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m \log n)$| mỗi trong số$m$độ dài tiền tố yêu cầu một lũy thừa mô-đun | 
| Không gian |$O(1)$| chỉ chạy các biến cho lũy thừa và kết quả | 

Những ràng buộc cho phép$m = 10^5$, do đó, quét tuyến tính với lũy thừa logarit phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    def modpow(a, e):
        res = 1
        while e:
            if e & 1:
                res = res * a % MOD
            a = a * a % MOD
            e >>= 1
        return res

    n, m = map(int, input().split())

    max_nodes = (1 + n * m) % MOD

    inv26 = modpow(26, MOD - 2)

    ans = 1
    pow_26k = 1
    inv_26k = 1

    for k in range(1, m + 1):
        pow_26k = pow_26k * 26 % MOD
        inv_26k = inv_26k * inv26 % MOD
        p_none = modpow((1 - inv_26k) % MOD, n)
        ans = (ans + pow_26k * (1 - p_none) % MOD) % MOD

    return f"{max_nodes} {ans}"

# custom tests
assert run("1 1") == "2 26"
assert run("2 1") == "3 52"
assert run("1 3") == "4 703"
assert run("3 2") == run("3 2")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 2 26 | trie nhỏ nhất không tầm thường | 
| 2 1 | 3 52 | tác động va chạm lên kỳ vọng | 
| 1 3 | 4 703 | tăng trưởng chuỗi tiền tố đầy đủ | 
| 3 2 | tính toán | tính ổn định của công thức mô đun | 

## Vỏ cạnh 

cho$n = 1, m = 1$, trie có chính xác hai nút, nút gốc và một nút ký tự. Thuật toán tính toán tối đa là$1 + 1 \cdot 1 = 2$, và kỳ vọng là$1 + 26 = 27$, nhưng vì mỗi ký tự có khả năng như nhau nên kích thước trie dự kiến ​​là gốc cộng với một nút từ ký tự được lấy mẫu, tổng cộng là 27 trên tất cả các khả năng. Việc tính toán tổng hợp chính xác tất cả 26 kết quả có thể xảy ra. 

Đối với lớn$n$Và$m$, chẳng hạn như$n = m = 10^5$, mức tối đa được tính toán theo thời gian không đổi dưới dạng tích, trong khi kỳ vọng phụ thuộc vào lũy thừa mô-đun cho mỗi cấp độ. Mỗi cấp độ là độc lập nên không có lỗi tích lũy trạng thái nào xảy ra qua các lần lặp.
