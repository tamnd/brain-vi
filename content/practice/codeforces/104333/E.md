---
title: "CF 104333E - Kẻ lữ hành ngẫu nhiên"
description: "Chúng tôi liên tục chọn một thành phố một cách thống nhất một cách ngẫu nhiên từ tập hợp các thành phố $n$ và mỗi lần chúng tôi chọn một thành phố, chúng tôi phải trả chi phí liên quan. Quá trình chỉ dừng lại khi mỗi thành phố đã được nhìn thấy ít nhất một lần."
date: "2026-07-01T18:55:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104333
codeforces_index: "E"
codeforces_contest_name: "Replay of BU - PSTU Programming club collaborative contest"
rating: 0
weight: 104333
solve_time_s: 65
verified: true
draft: false
---

[CF 104333E - Người du hành ngẫu nhiên](https://codeforces.com/problemset/problem/104333/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta liên tục chọn một thành phố thống nhất một cách ngẫu nhiên từ tập hợp các$n$các thành phố và mỗi lần chúng tôi chọn một thành phố, chúng tôi sẽ thanh toán chi phí liên quan. Quá trình chỉ dừng lại khi mỗi thành phố đã được nhìn thấy ít nhất một lần. Câu hỏi không phải là về số bước mà là về tổng chi phí dự kiến ​​được tích lũy cho đến khi tất cả các thành phố riêng biệt xuất hiện. 

Mỗi lần chúng tôi “vẽ” một thành phố, chúng tôi sẽ trả chi phí của nó ngay lập tức, ngay cả khi chúng tôi đã nhìn thấy nó trước đó. Điều này có nghĩa là tổng chi phí là tổng chi phí của tất cả các mẫu trong một quy trình ngẫu nhiên tiếp tục cho đến khi bao gồm toàn bộ chỉ số. 

Khó khăn là thời gian dừng phụ thuộc vào tính ngẫu nhiên và chi phí cho mỗi lần rút thăm phụ thuộc vào thành phố được chọn. Điều này làm cho giá trị kỳ vọng trở nên không cần thiết, bởi vì số lần mỗi thành phố được ghé thăm trước khi kết thúc là không độc lập. 

Những ràng buộc cho phép$n$lên tới$10^5$, do đó, bất kỳ giải pháp nào mô phỏng quy trình hoặc theo dõi các tập hợp con một cách rõ ràng đều không thể thực hiện được. Thậm chí duy trì các tập hợp con$2^n$trạng thái hoặc thực hiện DP trên các bộ đã truy cập sẽ bị loại trừ. Một giải pháp hợp lệ phải giảm vấn đề về biểu thức dạng đóng hoặc tính toán thời gian tuyến tính trên mảng. 

Một vấn đề tế nhị xuất hiện khi mọi chi phí đều bằng nhau. Một cách tiếp cận ngây thơ có thể cố gắng tính số lần rút dự kiến ​​nhân với chi phí trung bình, nhưng điều đó không thành công vì thời gian dừng và chi phí tích lũy không thể tách rời một cách ngây thơ mà không có sự biện minh. 

Một trường hợp góc khác là$n = 1$, trong đó quá trình kết thúc ngay sau một lần rút thăm, vì vậy câu trả lời đơn giản là$a_1$. Bất kỳ công thức dẫn xuất nào cũng phải thu gọn rõ ràng về trường hợp cơ bản này. 

## Phương pháp tiếp cận 

Mô hình tinh thần brute-force là mô phỏng quá trình ngẫu nhiên: bắt đầu với một tập hợp trống các thành phố đã ghé thăm, liên tục chọn một chỉ số ngẫu nhiên, tích lũy chi phí của nó và đánh dấu nó là đã thấy. Lặp lại cho đến khi tất cả các thành phố được truy cập. Người ta có thể thử mô phỏng Monte Carlo hoặc liệt kê đầy đủ các trạng thái, nhưng cả hai đều không khả thi. Ngay cả một mô phỏng duy nhất cũng có độ dài dự kiến$n H_n$và tính trung bình nhiều lần chạy sẽ vượt xa giới hạn. 

Một cách mạnh mẽ hơn có cấu trúc chặt chẽ hơn là mô hình hóa quy trình như một chuỗi Markov trên các tập hợp con của các thành phố được ghé thăm. Từ một tiểu bang$S$, quá trình chuyển đổi tiếp theo sẽ thêm một thành phố ngẫu nhiên và mỗi quá trình chuyển đổi sẽ đóng góp chi phí dự kiến ​​tùy thuộc vào xác suất lựa chọn. Điều này dẫn đến$O(2^n)$trạng thái, điều này là không thể ngay lập tức. 

Quan sát quan trọng là tách biệt “thời điểm quá trình kết thúc” khỏi “tần suất mỗi thành phố được thanh toán”. Thay vì lý luận về các tập hợp con, chúng tôi phân tích các khoản đóng góp theo từng thành phố. Mỗi thành phố đóng góp chi phí mỗi lần rút thăm và chúng tôi chỉ cần số lần dự kiến ​​mỗi thành phố xuất hiện trước khi quá trình dừng lại. 

Điều này gợi ý nên tập trung vào số lượt truy cập dự kiến. Một thủ thuật tiêu chuẩn cho các điều kiện dừng giống như người thu phiếu giảm giá là đặt điều kiện vào thành phố mới được phát hiện cuối cùng. Thay vì theo dõi toàn bộ lịch sử, chúng tôi xem xét thời điểm thành phố cuối cùng chưa được nhìn thấy được phát hiện. Cấu trúc xác suất của sự kiện đó cho phép chúng ta biểu thị số lượng dự kiến ​​bằng cách sử dụng tính đối xứng trên các hoán vị của thứ tự khám phá. 

Khi chúng ta chuyển quan điểm sang thứ tự, mỗi hoán vị của các thành phố có thể được coi là thứ tự các thành phố được khám phá lần đầu tiên. Thời gian dừng tương ứng với phần tử cuối cùng trong hoán vị này được phát hiện. Điều này chuyển vấn đề thành lý luận về cấp bậc trong các hoán vị ngẫu nhiên và thời gian chờ đợi hình học giữa các khám phá. 

Sự chuyển đổi cuối cùng mang lại một biểu thức tuyến tính trong chi phí, trong đó mỗi$a_i$được nhân với số lần dự kiến ​​của thành phố$i$được rút ra trước khi nó trở nên không liên quan trong quá trình che phủ. Kỳ vọng đó chỉ phụ thuộc vào vị trí của nó trong số các tập hợp con của các phần tử không nhìn thấy được, dẫn đến một cấu trúc hài hòa. 

Điều này làm giảm vấn đề tính toán một tổng có trọng số hài hòa duy nhất trên tất cả các thành phố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng / Markov DP | Hàm mũ | Hàm mũ | Quá chậm | 
| Đóng góp + phân tích hài hòa |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quan sát rằng quá trình này tiếp tục cho đến khi tất cả các thành phố được nhìn thấy ít nhất một lần, vậy là chúng ta đang xử lý một sự kiện được đưa tin đầy đủ qua việc lấy mẫu ngẫu nhiên thống nhất. 
2. Thay vì theo dõi các tập hợp con của các thành phố đã ghé thăm, hãy tập trung vào thời điểm mỗi thành phố được khám phá lần đầu tiên. Thứ tự xuất hiện lần đầu tạo thành một hoán vị ngẫu nhiên của tất cả các thành phố, được phân bố đồng đều trên tất cả các thành phố.$n!$khả năng. 
3. Đối với thành phố cố định$i$, hãy xem xét sự đóng góp của nó vào tổng chi phí. Mỗi lần nó được rút ra trước khi quá trình kết thúc, nó sẽ thêm$a_i$đến tổng số. Vì vậy, chúng tôi muốn số lần dự kiến ​​của thành phố$i$được lấy mẫu trước khi thành phố vô hình cuối cùng được phát hiện. 
4. Điều kiện về sự kiện thành phố đó$i$là$k$-Thành phố mới thứ 2 xuất hiện theo thứ tự khám phá. Trước khi điều này xảy ra, có$k-1$các thành phố đã được khám phá và$n-k+1$các thành phố chưa được nhìn thấy bao gồm$i$. Quá trình tiếp tục vẽ cho đến khi$i$xuất hiện lần đầu tiên. 
5. Trong khi$i$là không thể nhìn thấy, mỗi lần rút thăm đều có xác suất trúng một thành phố không thể nhìn thấy$\frac{n-k+1}{n}$. Thời gian chờ đợi cho đến khi khám phá được một thành phố mới trong tập hợp chưa được nhìn thấy đóng góp vào kỳ vọng hình học của$\frac{n}{n-k+1}$. Điều này quyết định bao lâu$i$vẫn không được nhìn thấy và do đó có tổng cộng bao nhiêu trận hòa xảy ra trước khi nó xuất hiện lần đầu. 
6. Tổng hợp tất cả các cấp bậc có thể$k$, có trọng số đồng nhất vì tất cả các hoán vị đều có khả năng như nhau, mang lại kết quả là mỗi thành phố đóng góp một hệ số kỳ vọng bằng$n$-số hài hòa thứ$H_n$. 
7. Do đó tổng chi phí dự kiến ​​sẽ trở thành$H_n \cdot \sum a_i$. 
8. Tính modulo số hài$10^9+7$BẰNG$H_n = \sum_{i=1}^n i^{-1}$và nhân với tổng chi phí. 

### Tại sao nó hoạt động 

Quá trình này diễn ra đối xứng trên tất cả các thành phố và cấu trúc duy nhất quan trọng là còn lại bao nhiêu thành phố chưa được nhìn thấy khi lễ bốc thăm diễn ra. Mỗi thành phố hoạt động giống hệt nhau khi được dán nhãn lại, vì vậy số lần xuất hiện dự kiến ​​​​của nó trước khi được phủ sóng đầy đủ chỉ phụ thuộc vào kích thước nhóm giảm dần của các yếu tố không nhìn thấy. Điều này làm giảm vấn đề đối với cấu trúc thu thập phiếu giảm giá trong đó mỗi giai đoạn đóng góp một giá trị dự kiến$\frac{n}{k}$các bước cho$k$những yếu tố chưa được nhìn thấy còn lại. Tính tuyến tính của kỳ vọng cho phép tính tổng các khoản đóng góp một cách độc lập, đảm bảo tổng kỳ vọng được phân tích rõ ràng thành tích của tổng chi phí và số hài. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modinv(x):
    return pow(x, MOD - 2, MOD)

n = int(input())
a = list(map(int, input().split()))

S = sum(a) % MOD

H = 0
for i in range(1, n + 1):
    H = (H + modinv(i)) % MOD

print((S * H) % MOD)
```Việc thực hiện trực tiếp theo công thức dẫn xuất. Phần không cần thiết duy nhất là tính toán nghịch đảo mô-đun cho tất cả các số nguyên lên đến$n$, vì số hài được định nghĩa là tổng của các nghịch đảo theo số học mô đun. 

Tổng chi phí được tính một lần. Sau đó chúng ta tích lũy$H_n$sử dụng nghịch đảo mô-đun. Cuối cùng, chúng tôi nhân cả hai giá trị. 

Một lỗi phổ biến là cố gắng mô phỏng quá trình ngẫu nhiên, quá trình này sẽ không bao giờ kết thúc trong giới hạn. Một sai lầm khác là quên rằng phép chia phải được xử lý thông qua phép chia nghịch đảo mô-đun thay vì phép chia số nguyên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
```Chúng tôi tính toán$\sum a_i = 6$. 

Chúng tôi tính toán:$H_3 = 1 + 1/2 + 1/3 = 1 + 500000004 + 333333336 = 833333341$| Bước | Giá trị | 
| --- | --- | 
| tổng(a) | 6 | 
| H_1 | 1 | 
| H_2 | 500000004 | 
| H_3 | 833333341 | 
| kết quả | 6 × 833333341 | 

Câu trả lời cuối cùng là$11$. 

Điều này thể hiện sự hủy bỏ số học mô-đun và xác nhận sự tích lũy hài hòa khớp với dạng đóng dự kiến. 

### Ví dụ 2 

đầu vào:```
1
5
```Chúng tôi chỉ có một thành phố, vì vậy:$H_1 = 1$| Bước | Giá trị | 
| --- | --- | 
| tổng(a) | 5 | 
| H_1 | 1 | 
| kết quả | 5 | 

Quá trình này luôn dừng sau một lần rút tiền, do đó chi phí chính xác là một giá trị duy nhất, khớp với công thức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một lần tính tổng chi phí và một lần tính tổng hài hòa | 
| Không gian |$O(1)$| Chỉ có một số ắc quy được sử dụng | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì$n = 10^5$cho phép lặp tuyến tính một cách thoải mái và lũy thừa mô-đun là thời gian không đổi trên mỗi giá trị trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    S = sum(a) % MOD
    def inv(x): return pow(x, MOD-2, MOD)
    H = 0
    for i in range(1, n+1):
        H = (H + inv(i)) % MOD
    return str((S * H) % MOD)

# provided samples
assert run("3\n1 2 3\n") == "11", "sample 1"
assert run("1\n5\n") == "5", "sample 2"

# custom cases
assert run("2\n1 1\n") == "3", "minimum nontrivial case"
assert run("4\n1 2 3 4\n") == str((10 * sum(pow(i, MOD-2, MOD) for i in range(1,5))) % MOD), "manual harmonic check"
assert run("5\n5 5 5 5 5\n") == str((25 * sum(pow(i, MOD-2, MOD) for i in range(1,6))) % MOD), "uniform costs"
assert run("1\n100000\n") == "100000", "single element large value"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`| 3 | trường hợp nhiều thành phố nhỏ nhất | 
|`1 2 3 4`| dựa trên sóng hài | tính đúng đắn của phép tính tổng | 
|`5 × identical`| điều hòa tỷ lệ | chia tỷ lệ chi phí thống nhất | 
|`n=1 large`| chi phí trực tiếp | trường hợp chấm dứt cơ sở | 

## Vỏ cạnh 

Khi nào$n = 1$, quá trình này không bao giờ lặp lại. Số hài hòa$H_1 = 1$, do đó công thức giảm trực tiếp thành$a_1$. Thuật toán tính tổng$= a_1$và nhân với$1$, tạo ra đầu ra chính xác mà không cần vỏ đặc biệt. 

Khi tất cả$a_i$đều bình đẳng, nói rằng tất cả đều$c$, giá trị kỳ vọng trở thành$c \cdot n \cdot H_n$. Việc triển khai xử lý việc này một cách chính xác vì tổng chi phí trở thành$n \cdot c$, và hệ số hài không đổi. 

Khi$n$lớn, tổng hài vẫn được tính theo thời gian tuyến tính và nghịch đảo mô đun là an toàn vì mô đun là số nguyên tố và tất cả các mẫu số đều khả nghịch.
