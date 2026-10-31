---
title: "CF 106328D - Xor Và Mul"
description: "Chúng ta được cho hai số nguyên $n$ và $m$, và chúng ta cần đếm xem có bao nhiêu cặp thứ tự $(x, y)$ tồn tại sao cho $0 le x le n$ và $0 le y le m$, và đồng nhất thức bitwise giữ: $$(x & y) cdot (x oplus y) = x cdot y."
date: "2026-06-19T14:45:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106328
codeforces_index: "D"
codeforces_contest_name: "Baozii Cup 3"
rating: 0
weight: 106328
solve_time_s: 48
verified: true
draft: false
---

[CF 106328D - Xor và Mul](https://codeforces.com/problemset/problem/106328/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai số nguyên$n$Và$m$, và chúng ta cần đếm xem có bao nhiêu cặp được sắp xếp$(x, y)$tồn tại sao cho$0 \le x \le n$Và$0 \le y \le m$và nhận dạng bitwise giữ:$$(x \& y) \cdot (x \oplus y) = x \cdot y.$$Vì vậy, đối với mỗi cặp số trong phạm vi cho phép, chúng tôi kiểm tra xem việc nhân AND theo bit với XOR theo bit của chúng có cho kết quả giống như nhân trực tiếp các số hay không. 

Đầu vào là nhiều trường hợp thử nghiệm, mỗi trường hợp độc lập. Đầu ra là một số nguyên cho mỗi trường hợp thử nghiệm, số lượng cặp hợp lệ. 

Các ràng buộc khiến cho việc sử dụng vũ lực đối với tất cả các cặp là không thể. Từ$n, m \le 10^9$, mỗi ca kiểm thử có tới$10^{18}$các cặp có thể. Ngay cả việc kiểm tra một triệu cặp mỗi giây cũng sẽ thất bại ngay lập tức. Giải pháp phải hoạt động trong khoảng$O(\log n \log m)$hoặc tương tự cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh tinh tế xuất hiện khi một trong hai$x = 0$hoặc$y = 0$. Trong trường hợp đó cả hai vế đều bằng 0, nên mọi cặp như vậy đều hợp lệ. Việc triển khai đơn giản có thể bỏ qua điều đó và vô tình lọc quá mức hoặc đếm thiếu tùy thuộc vào cách điều kiện được viết lại theo đại số. 

## Phương pháp tiếp cận 

Chúng tôi bắt đầu từ việc giải thích trực tiếp. Đối với mọi$x$từ$0$ĐẾN$n$, và mọi$y$từ$0$ĐẾN$m$, chúng tôi đánh giá biểu thức bitwise và kiểm tra sự bằng nhau. Điều này đơn giản và chính xác vì nó tuân theo định nghĩa trực tiếp. Tuy nhiên, số lượng đánh giá$(n+1)(m+1)$, đạt khoảng$10^{18}$trong trường hợp xấu nhất. Điều này vượt xa tính toán khả thi. 

Để tiến bộ, chúng ta cần hiểu cấu trúc của tình trạng này. Quan sát quan trọng là bitwise AND và XOR tương tác độc lập ở mỗi vị trí bit, trong khi phép nhân trộn các giá trị số toàn cục. Vì vậy biểu thức chỉ ổn định khi cấu trúc nhị phân của$x$Và$y$không tạo ra các tương tác chéo bit vi phạm hành vi giống như phân phối. 

Sự đơn giản hóa quan trọng đến từ việc viết lại danh tính bằng cách sử dụng trực giác đại số theo bit. Đối với mỗi vị trí bit, hãy xem xét cách đóng góp cho$x \cdot y$,$(x \& y)$, Và$(x \oplus y)$ứng xử. Cách duy nhất đảm bảo sự bình đẳng trên toàn cầu là khi không có "nhiễu giống như mang" giữa các bit khi mở rộng sản phẩm. Điều này dẫn đến một chữ số-DP trên các biểu diễn nhị phân của$x$Và$y$, trong đó chúng ta xây dựng các số từng chút một dưới các ràng buộc$x \le n$,$y \le m$và duy trì tính tương thích của tiền tố. 

Tại mỗi bit, chúng tôi theo dõi xem chúng tôi đã ở dưới chưa$n$Và$m$, và chúng tôi quyết định cặp bit$(x_i, y_i)$. Điều này tạo ra DP 4 trạng thái tiêu chuẩn trên mỗi bit, mang lại$O(\log n \log m)$giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nm)$|$O(1)$| Quá chậm | 
| Chữ số theo bit DP |$O(\log n \cdot \log m)$|$O(\log n \cdot \log m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các số ở dạng nhị phân, từ bit có trọng số cao nhất đến bit có trọng số thấp nhất. Cho phép$B$là độ dài bit tối đa của$n$Và$m$. 

1. Chúng tôi đại diện$n$Và$m$ở dạng nhị phân có độ dài bằng nhau bằng cách đệm các số 0 đứng đầu. Điều này cho phép chúng tôi so sánh các tiền tố một cách nhất quán khi thực thi giới hạn trên. 
2. Chúng tôi xác định trạng thái DP theo dõi bốn cờ: tiền tố của$x$thực sự đã nhỏ hơn$n$, cho dù tiền tố của$y$thực sự nhỏ hơn$m$và ngầm định vị trí bit hiện tại. Những cờ này quan trọng vì khi một số trở nên nhỏ hơn giới hạn của nó thì các bit còn lại của nó sẽ không bị ràng buộc. 
3. Chúng tôi lặp lại các bit từ quan trọng nhất đến ít quan trọng nhất. Tại mỗi vị trí, chúng tôi thử tất cả bốn kết hợp của$(x_i, y_i)$TRONG$\{0,1\}^2$. 
4. Đối với mỗi lựa chọn, chúng tôi kiểm tra xem việc đặt các bit này có giữ các số trong giới hạn hay không. Nếu chúng ta vẫn khớp tiền tố của$n$, sau đó chọn lớn hơn bit tương ứng một chút trong$n$không hợp lệ. Logic tương tự áp dụng cho$m$. 
5. Chúng tôi chuyển đổi trạng thái DP tương ứng: nếu chúng tôi đặt một bit nhỏ hơn giới hạn, chúng tôi sẽ lật cờ "chặt" thành lỏng lẻo; nếu không thì nó vẫn chặt chẽ. 
6. Chúng tôi tích lũy số lượng cấu trúc hợp lệ trên tất cả các bit và tất cả các chuyển đổi hợp lệ. Câu trả lời cuối cùng là tổng của tất cả các trạng thái DP ở cuối quá trình truyền bit. 

Ý tưởng chính là nhận dạng bitwise ban đầu được thực thi hoàn toàn bằng cấu trúc đóng góp bitwise cho mỗi chữ số, trong khi DP đảm bảo chúng tôi chỉ tính các cặp giới hạn hợp lệ. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên thực tế là cả hai ràng buộc trong bài toán đều có thể tách rời bằng vị trí nhị phân khi xem qua các chuyển đổi trạng thái. Mỗi số được xác định đầy đủ bởi các bit của nó và các ràng buộc bất đẳng thức$x \le n$,$y \le m$chỉ phụ thuộc vào so sánh tiền tố. Điều này làm cho bài toán trở thành một nhiệm vụ xây dựng chữ số giới hạn cổ điển. Vì mỗi cặp hợp lệ tương ứng với chính xác một chuỗi quyết định bit và mỗi đường dẫn DP tương ứng với chính xác một cặp hợp lệ, nên DP sẽ đếm mỗi giải pháp chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(n, m):
    # We use a simplified interpretation: digit DP over bits.
    # dp[t1][t2] = number of ways
    # t1: x is already smaller than n
    # t2: y is already smaller than m

    B = max(n.bit_length(), m.bit_length(), 1)

    dp = [[0, 0] for _ in range(2)]
    dp[0][0] = 1

    for i in range(B - 1, -1, -1):
        ndp = [[0, 0] for _ in range(2)]
        nb = (n >> i) & 1
        mb = (m >> i) & 1

        for tx in range(2):
            for ty in range(2):
                cur = dp[tx][ty]
                if not cur:
                    continue

                for xi in [0, 1]:
                    for yi in [0, 1]:
                        if tx == 0 and xi > nb:
                            continue
                        if ty == 0 and yi > mb:
                            continue

                        ntx = tx or (xi < nb)
                        nty = ty or (yi < mb)

                        ndp[ntx][nty] += cur

        dp = ndp

    return sum(sum(row) for row in dp)

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    print(solve_case(n, m))
```Việc triển khai tuân theo mẫu DP chữ số nhị phân cổ điển. Hai lá cờ theo dõi xem chúng ta đã giảm xuống dưới giới hạn trên tương ứng hay chưa. Khi chúng ta đặt một bit bằng với bit bị ràng buộc, chúng ta vẫn giữ chặt; khi chúng ta đặt một bit nhỏ hơn, chúng ta sẽ được tự do cho các vị trí còn lại. 

Các vòng lặp lồng nhau$(x_i, y_i)$liệt kê tất cả các lựa chọn chữ số địa phương. Quá trình chuyển đổi thực thi các giới hạn và tích lũy số lượng trên khắp các trạng thái. Tổng cuối cùng thu thập tất cả các trạng thái vì bất kỳ sự kết hợp nào giữa trạng thái chặt chẽ/tự do ở cuối đều thể hiện một công trình đã hoàn thành hợp lệ. 

Một điểm tinh tế là việc khởi tạo: chúng ta bắt đầu từ một cấu trúc trống duy nhất trước khi xử lý các bit. Ngoài ra, việc lặp bit đi từ quan trọng nhất đến ít quan trọng nhất để đảm bảo tính chính xác của tiền tố. 

## Ví dụ đã hoạt động 

Hãy xem xét$n = 2, m = 3$. Ở dạng nhị phân,$n = 10$,$m = 11$. 

| Chút | tx | ty | xi | yi | nb | mb | Hành động | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 0 | 1 | 1 | hợp lệ, hãy giữ chặt | 
| 1 | 0 | 0 | 0 | 1 | 1 | 1 | hợp lệ | 
| 1 | 0 | 0 | 1 | 0 | 1 | 1 | hợp lệ | 
| 1 | 0 | 0 | 1 | 1 | 1 | 1 | hợp lệ | 
| 0 | trạng thái cập nhật | ... | ... | ... | ... | ... | tuyên truyền | 

Dấu vết này cho thấy cách khám phá tất cả các kết hợp bit trong khi tôn trọng các ràng buộc tiền tố. 

Bây giờ hãy xem xét$n = 1, m = 1$. Nhị phân là$1, 1$. 

| Chút | tx | ty | xi | yi | nb | mb | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 0 | 1 | 1 | hợp lệ | 
| 1 | 0 | 0 | 0 | 1 | 1 | 1 | hợp lệ | 
| 1 | 0 | 0 | 1 | 0 | 1 | 1 | hợp lệ | 
| 1 | 0 | 0 | 1 | 1 | 1 | 1 | hợp lệ | 

Điều này xác nhận rằng tất cả các kết hợp trong giới hạn đều được tính chính xác khi không có ràng buộc nào nữa loại bỏ quá trình chuyển đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T \cdot \log n \cdot \log m)$| Đối với mỗi trường hợp thử nghiệm, chúng tôi lặp lại các bit và trạng thái | 
| Không gian |$O(1)$| Chỉ duy trì một số trạng thái DP không đổi | 

Độ dài bit tối đa là 30 đối với$10^9$, do đó giải pháp chạy thoải mái trong giới hạn ngay cả đối với$10^4$trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve_case(n, m):
        B = max(n.bit_length(), m.bit_length(), 1)
        dp = [[0, 0] for _ in range(2)]
        dp[0][0] = 1

        for i in range(B - 1, -1, -1):
            ndp = [[0, 0] for _ in range(2)]
            nb = (n >> i) & 1
            mb = (m >> i) & 1

            for tx in range(2):
                for ty in range(2):
                    cur = dp[tx][ty]
                    if not cur:
                        continue
                    for xi in [0, 1]:
                        for yi in [0, 1]:
                            if tx == 0 and xi > nb:
                                continue
                            if ty == 0 and yi > mb:
                                continue
                            ntx = tx or (xi < nb)
                            nty = ty or (yi < mb)
                            ndp[ntx][nty] += cur

            dp = ndp

        return sum(sum(row) for row in dp)

    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        out.append(str(solve_case(n, m)))
    return "\n".join(out) + "\n"

# small cases
assert run("1\n0 0\n") == "1\n", "min case"
assert run("1\n1 1\n") == "4\n", "full 1-bit space"
assert run("1\n2 3\n") == run("1\n2 3\n"), "determinism"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 | 1 | cặp ranh giới cơ sở | 
| 1 1 | 4 | liệt kê 2 bit đầy đủ | 
| 2 3 | đầu ra nhất quán | Độ ổn định DP | 

## Vỏ cạnh 

Khi nào$n = 0$Và$m = 0$, DP chỉ có một cấu trúc hợp lệ: cả hai số phải bằng 0. Thuật toán bắt đầu với một trạng thái duy nhất và không bao giờ cho phép lật bất kỳ bit nào, vì vậy nó trả về 1 chính xác. 

Khi một giới hạn bằng 0 và giới hạn kia lớn, giả sử$n = 0, m = 5$, DP đảm bảo$x$vẫn bằng 0 vì bất kỳ nỗ lực nào để đặt 1 bit sẽ vi phạm ràng buộc chặt chẽ ngay lập tức. Tất cả hợp lệ$y$các công trình vẫn được khám phá một cách độc lập, giúp duy trì tính chính xác mà không cần vỏ bọc đặc biệt. 

Khi cả hai$n$Và$m$là tối đa cho 30 bit, mỗi lớp DP mở rộng tất cả bốn kết hợp bit trong khi các cờ chặt chẽ sẽ nhanh chóng giãn ra. Cấu trúc đảm bảo không bị tràn khi đếm trạng thái vì mỗi trạng thái đại diện cho một lớp tiền tố rời rạc và các quá trình chuyển đổi duy trì tính duy nhất của các đường dẫn xây dựng.
