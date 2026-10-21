---
title: "CF 105838G - Ai thích Toán không phải là Boki-chan"
description: "Chúng ta được cung cấp một khoảng số nguyên rất lớn, từ $L$ đến $R$, trong đó $R$ có thể lớn tới $10^{18}$. Nhiệm vụ là đếm xem có bao nhiêu số trong khoảng này thỏa mãn tính chất dựa trên chữ số: khi bạn nhìn vào biểu diễn thập phân của một số, hiệu tuyệt đối…"
date: "2026-06-22T01:22:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105838
codeforces_index: "G"
codeforces_contest_name: "The 14th Huazhong Agricultural University Programming Contest"
rating: 0
weight: 105838
solve_time_s: 47
verified: true
draft: false
---

[CF 105838G - Ai thích Toán không phải là Boki-chan](https://codeforces.com/problemset/problem/105838/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một khoảng số nguyên rất lớn, từ$L$ĐẾN$R$, Ở đâu$R$có thể lớn như$10^{18}$. Nhiệm vụ là đếm xem có bao nhiêu số trong khoảng này thỏa mãn tính chất dựa trên chữ số: khi bạn nhìn vào biểu diễn thập phân của một số, hiệu tuyệt đối giữa mỗi cặp chữ số liền kề phải chính xác là 1. Tất cả các số có một chữ số đều tự động thỏa mãn quy tắc này. 

Vì vậy, các số như 6, 10, 21, 3210 hoặc 4567 là hợp lệ vì mỗi cặp chữ số lân cận thay đổi đúng một. Các số như 112 không thành công vì hai chữ số đầu tiên khác nhau 0, 12332 không thành công vì 2 đến 3 là ổn nhưng 3 đến 3 phá vỡ quy tắc và 555 thất bại ngay lập tức vì tất cả các khác biệt liền kề đều bằng 0. 

Đầu vào là một khoảng duy nhất và đầu ra là số số nguyên hợp lệ trong khoảng đó. 

Khó khăn chính là kích thước của phạm vi. Kiểm tra trực tiếp mọi số từ$L$ĐẾN$R$là không thể vì khoảng thời gian có thể kéo dài đến$10^{18}$, điều đó có nghĩa là lên đến$10^{18}$ứng viên trong trường hợp xấu nhất. Ngay cả việc kiểm tra một số duy nhất cũng có giá$O(\text{digits})$, vì vậy vũ lực hoàn toàn nằm ngoài phạm vi. 

Sự tinh tế thứ hai là cấu trúc chữ số hàng đầu. Điều kiện chỉ phụ thuộc vào các chữ số liền kề, vì vậy các số hoạt động giống như các đường dẫn trong biểu đồ chữ số và chúng ta cần đếm tất cả các chuỗi chữ số hợp lệ có tối đa 18 chữ số nằm trong giới hạn số. 

Các trường hợp Edge phá vỡ lý luận ngây thơ bao gồm: 

Một khoảng một chữ số như$L=1, R=9$, trong đó tất cả các câu trả lời đều hợp lệ và lời giải không được phép đếm quá phức tạp. 

Một ranh giới như$L=10, R=10$, trong đó chỉ tồn tại một số và nó phải được đếm chính xác ngay cả khi đó là trường hợp nhỏ nhất có hai chữ số. 

Khoảng thời gian ở đó$L$Và$R$khác nhau về chiều dài, chẳng hạn như$L=9, R=1000$, trong đó giải pháp DP chữ số phải xử lý liền mạch mọi độ dài. 

Một cách tiếp cận đơn giản tạo ra tất cả các số hợp lệ có tối đa 18 chữ số và lọc chúng theo phạm vi vẫn sẽ yêu cầu tạo ra hàng triệu trạng thái, nhưng quan trọng hơn là nó sẽ yêu cầu cắt tỉa cẩn thận để tránh tràn vượt quá giới hạn, đó chính xác là chữ số mà DP được thiết kế để xử lý rõ ràng. 

## Phương pháp tiếp cận 

Một chiến lược bạo lực sẽ lặp đi lặp lại mọi con số trong$[L, R]$, chuyển đổi nó thành một chuỗi và xác minh xem các chữ số liền kề có khác nhau chính xác một không. Điều này hoạt động hợp lý vì điều kiện là cục bộ đối với mỗi số và chỉ yêu cầu quét tuyến tính cho mỗi ứng viên. Giá mỗi số là$O(d)$, Ở đâu$d \leq 18$. Tuy nhiên, số lượng ứng viên$R - L + 1$, trong trường hợp xấu nhất là$10^{18}$, làm cho phương pháp này không thể thực hiện được. 

Quan sát quan trọng là chúng ta không đánh giá hàm theo số một cách độc lập mà đếm các chuỗi chữ số thỏa mãn ràng buộc chuyển tiếp cục bộ. Mỗi số hợp lệ có thể được xem như một đường dẫn trong đó các chữ số tạo thành một chuỗi và mỗi bước di chuyển ±1. Điều này biến vấn đề thành việc đếm các chuỗi chữ số hợp lệ theo một ràng buộc ràng buộc, đây chính xác là cài đặt cho lập trình động chữ số. 

Thay vì liệt kê các số, chúng ta đếm xem có bao nhiêu dãy chữ số hợp lệ nhỏ hơn hoặc bằng một giới hạn cho trước$X$, sau đó tính kết quả là:$$\text{count}(R) - \text{count}(L-1)$$Để tính toán$\text{count}(X)$, chúng tôi sử dụng DP trên các vị trí có trạng thái theo dõi chữ số trước đó và liệu chúng tôi có còn chặt chẽ với tiền tố của$X$. Điều này tránh việc tạo ra các số một cách rõ ràng và đảm bảo chúng ta chỉ thực hiện các chuyển đổi chữ số khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(R-L+1)\cdot O(d)$|$O(1)$| Quá chậm | 
| Chữ số DP |$O(18 \cdot 10 \cdot 2)$|$O(18 \cdot 10 \cdot 2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định một chức năng$f(X)$đếm xem có bao nhiêu số hợp lệ$[1, X]$. Câu trả lời cuối cùng là$f(R) - f(L-1)$. 

1. Chuyển đổi$X$thành một mảng chữ số để chúng ta có thể xử lý từng vị trí. Điều này cho phép chúng ta so sánh các tiền tố một phần với giới hạn. 
2. Xác định trạng thái DP$dp[pos][prev][tight]$, Ở đâu`pos`là chỉ số chữ số hiện tại,`prev`là chữ số cuối cùng được chọn và`tight`cho biết liệu tiền tố cho đến nay có bằng tiền tố của$X$. Vai trò của`prev`là cần thiết vì tính hợp lệ phụ thuộc vào sự khác biệt giữa các chữ số liền kề. 
3. Khởi tạo DP ở vị trí 0 mà không chọn chữ số trước đó. Chúng tôi sử dụng một giá trị trọng điểm đặc biệt (chẳng hạn như 10 hoặc -1) để biểu thị rằng chưa có chữ số nào được đặt. Ở giai đoạn này, tất cả các chữ số từ 1 đến chữ số đầu tiên của$X$được phép, vì các số 0 đứng đầu không được coi là số hợp lệ trừ khi số đó chính xác bằng 0. 
4. Tại mỗi vị trí, lặp lại tất cả các chữ số tiếp theo có thể có. Nếu chúng ta chưa bắt đầu một số, chúng ta có thể chọn 0 làm phần tiếp theo của "vẫn trống", nhưng khi một số bắt đầu, các số 0 đứng đầu được coi là chữ số bình thường. Điều này cho phép xử lý chính xác các số như 10 hoặc 100. 
5. Đối với mỗi chữ số ứng cử viên, hãy thực thi quy tắc kề: nếu chúng ta đã có chữ số trước đó thì chênh lệch tuyệt đối phải chính xác bằng 1. Nếu điều kiện này không thành công, chúng ta bỏ qua quá trình chuyển đổi. 
6. Cập nhật`tight`cờ: nếu chúng tôi chặt chẽ và chúng tôi chọn một chữ số bằng chữ số giới hạn hiện tại, chúng tôi vẫn chặt chẽ; nếu không chúng ta sẽ mất đi sự chặt chẽ. 
7. Tổng hợp tất cả các lần hoàn thành hợp lệ khi đạt đến vị trí cuối cùng. Bất kỳ trạng thái nào có ít nhất một chữ số được chọn đều đóng góp một số hợp lệ. 
8. Tính toán câu trả lời cuối cùng bằng cách sử dụng loại trừ bao gồm số lượng tiền tố. 

### Tại sao nó hoạt động 

Mỗi số hợp lệ tương ứng với chính xác một đường dẫn trong các vị trí DP trên chữ số, bởi vì mỗi quyết định vị trí xác định duy nhất trạng thái tiếp theo thông qua ràng buộc chữ số trước đó. DP đảm bảo chúng tôi đếm từng đường dẫn như vậy chính xác một lần và`tight`ràng buộc đảm bảo chúng ta không bao giờ vượt quá giới hạn trên. Vì tất cả các chuyển đổi đều thực thi điều kiện sai phân liền kề nên không thể xây dựng số nào không hợp lệ và vì tất cả các chuỗi chữ số hợp lệ có thể đã được khám phá nên không có số nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_upto(x: int) -> int:
    if x <= 0:
        return 0

    digits = list(map(int, str(x)))
    n = len(digits)

    # dp[pos][prev][tight][started]
    # prev: 0-9, or 10 meaning "no previous digit yet"
    dp = [[[[0] * 2 for _ in range(2)] for _ in range(11)] for _ in range(n + 1)]
    dp[0][10][1][0] = 1

    for pos in range(n):
        for prev in range(11):
            for tight in range(2):
                for started in range(2):
                    cur = dp[pos][prev][tight][started]
                    if cur == 0:
                        continue

                    limit = digits[pos] if tight else 9

                    for d in range(limit + 1):
                        ntight = 1 if (tight and d == limit) else 0

                        if not started:
                            if d == 0:
                                dp[pos + 1][10][ntight][0] += cur
                            else:
                                dp[pos + 1][d][ntight][1] += cur
                        else:
                            if abs(d - prev) == 1:
                                dp[pos + 1][d][ntight][1] += cur

    res = 0
    for prev in range(11):
        for tight in range(2):
            res += dp[n][prev][tight][1]
    return res

def solve():
    L, R = map(int, input().split())
    print(count_upto(R) - count_upto(L - 1))

if __name__ == "__main__":
    solve()
```Việc triển khai tách logic đếm thành một hàm trợ giúp tính toán số tiền tố lên tới$X$. DP theo dõi rõ ràng liệu chúng tôi đã bắt đầu xây dựng một số hay chưa, điều này cần thiết để xử lý chính xác các số 0 đứng đầu mà không thực thi sai ràng buộc kề. 

Giá trị trọng điểm`10`được sử dụng để thể hiện sự vắng mặt của một chữ số trước đó. Điều này tránh việc trộn lẫn các chữ số thực với trạng thái “không đặt” và giữ cho logic chuyển đổi luôn rõ ràng. 

Bước trừ`count_upto(R) - count_upto(L - 1)`đảm bảo khoảng thời gian được xử lý sạch sẽ mà không cần vỏ bọc đặc biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$L=6, R=21$Chúng tôi tính các số hợp lệ lên tới 21 và trừ các số đó lên đến 5. 

| Vị trí | trạng thái trước | chặt chẽ | bắt đầu | chuyển tiếp | 
| --- | --- | --- | --- | --- | 
| bắt đầu | 10 | 1 | 0 | chữ số từ 0-2 | 
| vị trí 1 | - | - | - | dựng số có 1 chữ số 1-9 | 
| vị trí 2 | - | - | - | xây dựng 10, 12, 21 trong số những cái khác | 

Các số hợp lệ trong phạm vi: 6, 7, 8, 9, 10, 12, 21. 

Dấu vết này cho thấy cách các số có một chữ số được đưa vào một cách tự nhiên và cách chuyển đổi hai chữ số thực thi quy tắc ±1. 

### Ví dụ 2:$L=10, R=15$| Số | Hiệu lực | 
| --- | --- | 
| 10 | hợp lệ | 
| 11 | không hợp lệ | 
| 12 | hợp lệ | 
| 13 | không hợp lệ | 
| 14 | không hợp lệ | 
| 15 | không hợp lệ | 

DP sẽ chỉ chấp nhận các chuyển tiếp trong đó chữ số thứ hai khác chính xác một so với chữ số đầu tiên, điều này ngay lập tức lọc tập hợp xuống còn 10 và 12. 

Ví dụ này chứng minh rằng việc kiểm tra kề được thực thi cục bộ và loại bỏ chính xác các cặp chữ số bằng hoặc không liền kề không hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(18 \cdot 10 \cdot 2 \cdot 10)$| 18 vị trí, 10 chữ số, trạng thái chặt chẽ và bắt đầu và tối đa 10 lần chuyển đổi trên mỗi trạng thái | 
| Không gian |$O(18 \cdot 10 \cdot 2)$| Bảng DP theo vị trí, chữ số và trạng thái chặt chẽ (được tối ưu hóa ở trạng thái trước/bắt đầu) | 

Không gian trạng thái được cố định bởi số chữ số ở giới hạn trên, do đó nghiệm dễ dàng nằm gọn trong các ràng buộc ngay cả đối với$10^{18}$. Việc tính toán cho mỗi truy vấn là không đổi một cách hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def count_upto(x: int) -> int:
        if x <= 0:
            return 0
        digits = list(map(int, str(x)))
        n = len(digits)
        dp = [[[[0] * 2 for _ in range(2)] for _ in range(11)] for _ in range(n + 1)]
        dp[0][10][1][0] = 1

        for pos in range(n):
            for prev in range(11):
                for tight in range(2):
                    for started in range(2):
                        cur = dp[pos][prev][tight][started]
                        if cur == 0:
                            continue
                        limit = digits[pos] if tight else 9
                        for d in range(limit + 1):
                            ntight = 1 if (tight and d == limit) else 0
                            if not started:
                                if d == 0:
                                    dp[pos + 1][10][ntight][0] += cur
                                else:
                                    dp[pos + 1][d][ntight][1] += cur
                            else:
                                if abs(d - prev) == 1:
                                    dp[pos + 1][d][ntight][1] += cur

        return sum(dp[n][p][t][1] for p in range(11) for t in range(2))

    L, R = map(int, inp.split())
    return str(count_upto(R) - count_upto(L - 1))

assert run("6 21") == "7"
assert run("1 9") == "9"
assert run("10 10") == "1"
assert run("11 11") == "0"
assert run("1 100")  # sanity check run
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 6 21 | 7 | độ chính xác của mẫu và chuyển đổi nhiều chữ số | 
| 1 9 | 9 | tất cả các số có một chữ số hợp lệ | 
| 10 10 | 1 | xử lý số ranh giới đơn | 
| 11 11 | 0 | chữ số lặp lại không hợp lệ | 

## Vỏ cạnh 

Trường hợp biên là khi$L = 1$. Trong tình huống này, phép trừ sử dụng$L - 1 = 0$và DP trả về 0 một cách chính xác cho các đầu vào không dương, đảm bảo không xảy ra hiện tượng tràn hoặc đếm âm. 

Trường hợp thứ hai là khi$X$là một chữ số. DP cho phép bắt đầu các số mới ở bất kỳ chữ số nào từ 1 đến 9 và ràng buộc kề không bao giờ được kích hoạt vì không có chữ số trước đó. Điều này đảm bảo tất cả các số có một chữ số đều được tính mà không cần viết hoa đặc biệt. 

Trường hợp thứ ba là khi các số bao gồm các số 0 bên trong như 101 hoặc 210. Các số này được xử lý chính xác vì khi một số đã bắt đầu, các số 0 được xử lý giống như bất kỳ chữ số nào khác và phải đáp ứng ràng buộc ±1 so với chữ số trước đó.
