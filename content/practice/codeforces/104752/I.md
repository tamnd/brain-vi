---
title: "CF 104752I - Kiểm tra điểm số"
description: "Mỗi trò chơi bao gồm một chuỗi các trận đấu $N$. Điểm bắt đầu từ 1 và mỗi trận đấu sẽ nhân số điểm hiện tại với $A$, nhân với $B$ hoặc giữ nguyên nếu không ai giải quyết được vấn đề. Điểm mấu chốt là chúng tôi không chọn một chuỗi duy nhất."
date: "2026-06-29T01:25:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104752
codeforces_index: "I"
codeforces_contest_name: "Concurso de programaci\u00f3n ANIEI 2023"
rating: 0
weight: 104752
solve_time_s: 69
verified: false
draft: false
---

[CF 104752I - Kiểm tra điểm số](https://codeforces.com/problemset/problem/104752/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mỗi trò chơi bao gồm một chuỗi các$N$trận đấu. Tỉ số bắt đầu từ 1 và mỗi trận đấu sẽ nhân số điểm hiện tại với$A$, nhân nó với$B$, hoặc giữ nguyên nếu không có ai giải quyết được vấn đề. 

Điểm mấu chốt là chúng tôi không chọn một chuỗi duy nhất. Chúng ta phải xem xét mọi cách có thể để các trận đấu có thể được giải quyết một cách độc lập: mỗi trận đấu có ba khả năng xảy ra, nhưng hai trong số đó ảnh hưởng đến tỷ số. Hai trò chơi được coi là khác nhau nếu có ít nhất một trận đấu được giải quyết bởi một người khác. 

Đối với mỗi chuỗi kết quả đầy đủ như vậy, chúng tôi tính điểm cuối cùng và sau đó tính tổng các điểm này trên tất cả các chuỗi có thể có. 

Đầu vào đưa ra nhiều trường hợp kiểm thử, mỗi trường hợp có$N$,$A$, Và$B$. Chúng ta phải xuất tổng điểm trên tất cả các chuỗi kết quả hợp lệ theo modulo$10^9 + 7$. 

Các ràng buộc cho phép lên đến$10^5$trường hợp thử nghiệm và$N$lên đến$1000$. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng liệt kê các kết quả, vì ngay cả đối với một trường hợp thử nghiệm cũng có$3^N$khả năng. Thay vào đó, giải pháp phải tổng hợp các khoản đóng góp cho mỗi vị trí hoặc sử dụng cấu trúc tổ hợp dạng đóng. 

Trường hợp cạnh tinh tế xuất hiện khi$A = B = 1$. Trong trường hợp đó, mỗi chuỗi đều cho điểm 1, vì vậy câu trả lời chính xác là số chuỗi, tức là$3^N$. Bất kỳ đạo hàm nào chia cho$A - B$hoặc giả định tính khả nghịch mà không kiểm tra sẽ thất bại ở đây. Một trường hợp cạnh khác là$A = B \neq 1$, trong đó tính đối xứng làm sụp đổ cấu trúc và các công thức ngây thơ có tính hủy bỏ lại bị phá vỡ. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo coi mỗi trận đấu là sự lựa chọn độc lập giữa ba kết quả: Franco giải quyết, Rafa giải quyết hoặc không ai giải quyết được. Đối với một chuỗi cố định, điểm số là tích của những đóng góp dọc theo chuỗi đó. Điều này dẫn trực tiếp tới$3^N$trình tự, mỗi trình tự yêu cầu$O(N)$phép nhân để đánh giá, điều này vượt xa khả năng ngay cả đối với$N = 30$. 

Quan sát quan trọng là phép nhân phân phối trên các lựa chọn độc lập. Thay vì nghĩ về các chuỗi đầy đủ, chúng ta có thể nghĩ về mỗi trận đấu đóng góp một yếu tố độc lập trên tất cả các chuỗi. Đối với mỗi trận đấu, chúng tôi đang tổng hợp các khoản đóng góp một cách hiệu quả qua ba lựa chọn: nhân với$A$, nhân với$B$, hoặc nhân với 1. 

Điều này có nghĩa là khi mở rộng tất cả các chuỗi, tổng sẽ được tính thành thừa số. Mỗi vị trí đóng góp một hệ số nhân bằng tổng đóng góp cục bộ của nó trên tất cả các lựa chọn và vì các lựa chọn độc lập giữa các vị trí nên câu trả lời chung sẽ trở thành lũy thừa của tổng cục bộ đó. 

Ở mỗi trận đấu, số tiền có thể đóng góp vào tổng số tiền là: 

- 1 nếu không ai giải được, 
-$A$nếu Franco giải quyết được, 
-$B$nếu Rafa giải quyết được. 

Vì vậy mỗi vị trí đều đóng góp một yếu tố$1 + A + B$. Vì tất cả các vị trí đều độc lập nên tổng của tất cả các chuỗi là:$$(1 + A + B)^N.$$Điều này làm giảm toàn bộ vấn đề thành lũy thừa mô-đun nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(3^N \cdot N)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(T \log N)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Tính toán tối ưu thông qua lũy thừa mô-đun 

1. Với mỗi test case, hãy đọc$N$,$A$, Và$B$. Toàn bộ cấu trúc của bài toán chỉ phụ thuộc vào các giá trị này thông qua biểu thức$1 + A + B$. 
2. Tính giá trị cơ sở$x = (1 + A + B) \bmod (10^9 + 7)$. Điều này thể hiện tổng đóng góp của một trận đấu sau khi tổng hợp tất cả các kết quả có thể xảy ra. 
3. Tính toán$x^N \bmod (10^9 + 7)$sử dụng lũy ​​thừa nhị phân. Bước này tổng hợp những đóng góp độc lập của tất cả các trận đấu. 
4. Xuất kết quả. 

Phép lũy thừa nhị phân là cần thiết vì phép nhân trực tiếp$N$thời gian quá chậm đối với kích thước lớn$N$và lặp đi lặp lại trên$10^5$trường hợp thử nghiệm. 

### Tại sao nó hoạt động 

Mỗi trận đấu đóng góp độc lập cho mọi cấu hình trò chơi đầy đủ. Khi tổng hợp tất cả các cấu hình, chúng tôi đang mở rộng một cách hiệu quả một sản phẩm có số tiền giống nhau:$$\prod_{i=1}^{N} (1 + A + B).$$Vì không có sự tương tác giữa các vị trí nên không có nhiệm kỳ nào phụ thuộc vào quyết định của các vị trí khác. Tính độc lập này đảm bảo rằng phép nhân giữa các vị trí sẽ xây dựng lại chính xác việc khai triển tổ hợp đầy đủ mà không bị đếm quá mức hoặc thiếu bất kỳ chuỗi nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modexp(a, e):
    res = 1
    while e:
        if e & 1:
            res = (res * a) % MOD
        a = (a * a) % MOD
        e >>= 1
    return res

t = int(input())
for _ in range(t):
    n, a, b = map(int, input().split())
    base = (1 + a + b) % MOD
    print(modexp(base, n))
```Ý tưởng cốt lõi là giảm toàn bộ quá trình tổ hợp thành một phép tính công suất duy nhất. chức năng`modexp`thực hiện lũy thừa nhị phân, đảm bảo thời gian logarit cho mỗi trường hợp thử nghiệm. 

Một cạm bẫy phổ biến là quên đi kết quả “không giải quyết” trung lập, góp phần vào$1$trong căn cứ. Thiếu nó dẫn đến tính toán$(A + B)^N$, tính thiếu tất cả các chuỗi trong đó một kết quả khớp bị bỏ qua. 

## Ví dụ đã hoạt động 

### Mẫu 1:$N = 1, A = 6, B = 9$| Bước | Căn cứ$1 + A + B$| Kết quả | 
| --- | --- | --- | 
| Cơ sở tính toán |$1 + 6 + 9 = 16$| 16 | 
| Quyền lực |$16^1$| 16 | 

Điều này xác nhận rằng đối với một trận đấu, cả ba kết quả đều được tính tổng trực tiếp. 

### Mẫu 2:$N = 2, A = 8, B = 4$| Bước | Căn cứ | Tính toán điện năng | 
| --- | --- | --- | 
| Cơ sở tính toán |$1 + 8 + 4 = 13$| 13 | 
| Mở rộng đầu tiên |$13^2 = 169$| 169 | 

Điều này cho thấy hai kết quả phù hợp độc lập sẽ nhân lên cấu trúc đóng góp giống hệt nhau của chúng như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T \log N)$| Mỗi trường hợp thử nghiệm yêu cầu lũy thừa nhị phân trên$N$| 
| Không gian |$O(1)$| Chỉ một vài biến được sử dụng cho mỗi trường hợp thử nghiệm | 

Các ràng buộc cho phép lên đến$10^5$trường hợp thử nghiệm và$N \le 1000$, Vì thế$\log N$tối đa là khoảng 10. Điều này đảm bảo giải pháp dễ dàng chạy trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    MOD = 10**9 + 7

    def modexp(a, e):
        res = 1
        while e:
            if e & 1:
                res = (res * a) % MOD
            a = (a * a) % MOD
            e >>= 1
        return res

    t = int(sys.stdin.readline())
    for _ in range(t):
        n, a, b = map(int, sys.stdin.readline().split())
        base = (1 + a + b) % MOD
        output.append(str(modexp(base, n)))

    return "\n".join(output)

# provided samples
assert run("1\n1 6 9\n") == "16", "sample 1"
assert run("1\n2 8 4\n") == "169", "sample 2"

# custom cases
assert run("1\n1 0 0\n") == "1", "only neutral outcome"
assert run("1\n3 1 1\n") == "27", "symmetric small case"
assert run("2\n1 2 3\n1 5 7\n") == "6\n13", "multiple independent cases"
assert run("1\n1000 1 1\n") == str(pow(3, 1000, 10**9+7)), "large exponent check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$1\ 0\ 0$| 1 | Chỉ có kết quả không hoạt động | 
|$3\ 1\ 1$| 27 | Phân nhánh thống nhất | 
| hai trường hợp | 13/6 | Xử lý nhiều thử nghiệm | 
|$1000\ 1\ 1$|$3^{1000}$| Độ chính xác số mũ lớn | 

## Vỏ cạnh 

Khi nào$A = 0$Và$B = 0$, chỉ có kết quả “không giải quyết được” mới đóng góp. Cơ số trở thành 1 nên đáp án là$1^N = 1$. Thuật toán xử lý việc này trực tiếp thông qua tính toán cơ sở. 

Khi$A = B = 1$, mọi trận đấu đều đóng góp như nhau và cơ số trở thành 3. Câu trả lời là$3^N$, khớp với số lượng tất cả các chuỗi kết quả có thể xảy ra. Bất kỳ cách tiếp cận nào đơn giản hóa tính đối xứng không chính xác có thể vô tình thu gọn giá trị này thành 1 hoặc$2^N$, nhưng công thức lũy thừa vẫn đảm bảo tính đúng đắn. 

Khi$A$Và$B$lớn, việc rút gọn mô-đun chỉ xảy ra ở phép tính cơ sở và trong các bước lũy thừa. Vì tất cả số học đều mang tính mô-đun nên tránh được tình trạng tràn và các giá trị trung gian vẫn bị giới hạn.
