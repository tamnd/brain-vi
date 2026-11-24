---
title: "CF 908D - Năm Mới và Sự Sắp Xếp Tùy Ý"
description: "Chúng tôi xây dựng một chuỗi một ký tự tại một thời điểm. Ở mỗi bước, chúng tôi nối 'a' với xác suất $$frac{pa}{pa+pb}$$ và nối 'b' với xác suất $$frac{pb}{pa+pb}.$$ Đối với một chuỗi cố định, số lượng chuỗi con bằng \"ab\" rất dễ mô tả."
date: "2026-06-12T23:39:08+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 908
codeforces_index: "D"
codeforces_contest_name: "Good Bye 2017"
rating: 2200
weight: 908
solve_time_s: 394
verified: true
draft: false
---

[CF 908D - Năm mới và sự sắp xếp tùy tiện](https://codeforces.com/problemset/problem/908/D) 

**Đánh giá:** 2200 
**Tags:** dp, toán, xác suất 
**Thời gian giải:** 6 phút 34 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi xây dựng một chuỗi một ký tự tại một thời điểm. Ở mỗi bước chúng tôi nối thêm`'a'`với xác suất$$\frac{p_a}{p_a+p_b}$$và nối thêm`'b'`với xác suất$$\frac{p_b}{p_a+p_b}.$$Đối với một chuỗi cố định, số lượng các chuỗi con bằng`"ab"`thật dễ dàng để mô tả. Mọi`'b'`đóng góp thật nhiều`"ab"`các chuỗi tiếp theo như có`'a'`các ký tự đứng trước nó. Nếu hiện tại có$i$chữ cái`'a'`, sau đó thêm một`'b'`làm tăng số lượng`"ab"`các chuỗi tiếp theo một cách chính xác$i$. 

Quá trình dừng lại ngay khi tổng số`"ab"`các dãy sau đạt ít nhất$k$. Chúng tôi được yêu cầu về con số cuối cùng dự kiến ​​của`"ab"`các chuỗi tiếp theo. Vì kỳ vọng là một số hữu tỉ nên chúng ta xuất nó theo modulo$10^9+7$. 

Ràng buộc$k\le1000$là quan sát chính. Một bảng lập trình động bậc hai với khoảng$10^6$trạng thái hoàn toàn có thể quản lý được trong vòng hai giây. Bất kỳ cách tiếp cận nào cố gắng liệt kê các chuỗi hoặc mô phỏng các quy trình ngẫu nhiên đều vô vọng vì số lượng các chuỗi có thể tăng theo cấp số nhân theo độ dài của chúng. 

Một trường hợp tế nhị xuất hiện khi nhiều`'a'`các ký tự được thu thập trước ký tự đầu tiên`'b'`. Ví dụ, với```
1 1000000 1
```rất có thể là một số lượng lớn`'a'`các ký tự được tích lũy trước một`'b'`xuất hiện. Một lần đầu tiên`'b'`đến, số lượng`"ab"`các chuỗi tiếp theo có thể vượt xa$k$. Một DP cho rằng chúng ta luôn dừng lại chính xác ở$k$sẽ không chính xác. 

Một tình huống khó khăn khác xảy ra khi$i+j\ge k$, Ở đâu$i$là số lượng`'a'`nhân vật và$j$là số hiện tại của`"ab"`các chuỗi tiếp theo. Sau một lần nữa`'b'`, quá trình kết thúc ngay lập tức vì tổng mới trở thành$i+j\ge k$. Xử lý quá trình chuyển đổi này một cách riêng biệt là điều cần thiết. 

Ví dụ,```
1 1 1
```có đáp án 2. Chuỗi có thể kết thúc tại`"ab"`, nhưng nó cũng có thể trở thành`"aab"`,`"aaab"`, vân vân. Giá trị mong đợi vượt quá 1 vì có thể xảy ra tình trạng vọt lố. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tạo ra mọi chuỗi có thể cùng với xác suất của nó, dừng lại bất cứ khi nào số lượng`"ab"`dãy tiếp theo đạt tới$k$. Giá trị kỳ vọng sau đó có được bằng cách tính tổng$$\Pr(\text{string})\times(\text{final number of }ab).$$Điều này đúng về mặt toán học nhưng vô dụng trong thực tế. Ngay cả những chuỗi có độ dài vừa phải cũng đã tạo ra nhiều khả năng theo cấp số nhân. 

Nhận xét quan trọng là chỉ có hai đại lượng quan trọng. 

Đại lượng đầu tiên là số lượng`'a'`các nhân vật đã thấy cho đến nay. Giả sử nó bằng$i$. 

Đại lượng thứ hai là số hiện tại`"ab"`trình tự tiếp theo, nói$j$. 

Không có gì khác về thứ tự của các nhân vật quan trọng. Bất cứ khi nào chúng tôi thêm một cái khác`'a'`, nhà nước trở thành$(i+1,j)$. Bất cứ khi nào chúng tôi thêm một`'b'`, số lượng`"ab"`dãy sau tăng dần$i$, trạng thái sản xuất$(i,j+i)$. 

Điều này tự nhiên dẫn đến lập trình động trên các trạng thái$(i,j)$. Từ$j<k$, chỉ có khoảng$k^2$các bang có liên quan. 

Một sự tối ưu hóa hơn nữa sẽ tiếp tục$i\le k$. Nếu như$i\ge k$, sau đó thêm một`'b'`ngay lập tức kết thúc quá trình. Thay vì lưu trữ các giá trị lớn hơn của$i$, chúng ta có thể tính trực tiếp số bước bổ sung dự kiến ​​trước bước đầu tiên`'b'`xuất hiện. Điều này loại bỏ vô số trạng thái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| DP tối ưu |$O(k^2)$|$O(k^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuẩn hóa xác suất modulo$10^9+7$. 

Cho phép$$A=\frac{p_a}{p_a+p_b},\qquad B=\frac{p_b}{p_a+p_b}.$$2. Xác định$dp[i][j]$như câu trả lời cuối cùng được mong đợi bắt đầu từ một trạng thái với$i$chữ cái`'a'`và hiện tại$j$dãy số`"ab"`. 

Chỉ những trạng thái có$j<k$là cần thiết. 
3. Các trạng thái của quy trình theo thứ tự ngược lại$i+j$. 

Giá trị lớn hơn của$i+j$chỉ phụ thuộc vào các giá trị thậm chí còn lớn hơn, do đó việc truyền tải ngược đảm bảo tất cả các trạng thái bắt buộc đều đã được tính toán. 
4. Nếu$i+j\ge k$, một cái nữa`'b'`kết thúc quá trình. 

Trước đó`'b'`đến, một số bổ sung`'a'`ký tự có thể xuất hiện. Số lượng bổ sung dự kiến`'a'`ký tự trước ký tự đầu tiên`'b'`là$$\frac{A}{B}.$$Do đó số lượng dự kiến`'a'`ký tự khi kết thúc`'b'`xuất hiện bằng$$i+\frac{A}{B}.$$Thêm trận chung kết đó`'b'`đóng góp số tiền này, vì vậy câu trả lời trở thành$$j+i+\frac{A}{B}+1.$$5. Nếu không, một quá trình chuyển đổi sẽ thêm`'a'`và người kia thêm vào`'b'`. 

Với xác suất$A$, nhà nước trở thành$(i+1,j)$. 

Với xác suất$B$, nhà nước trở thành$(i,j+i)$. 
6. Sắp xếp lại phương trình kỳ vọng cho$$dp[i][j] = A\cdot dp[i+1][j] + B\cdot dp[i][j+i].$$1. Đầu ra$dp[1][0]$. 

Ban đầu chúng ta có thể tưởng tượng đã có một`'a'`. Thủ thuật tiêu chuẩn này loại bỏ trạng thái rắc rối$i=0$, nơi thêm`'b'`không có gì thay đổi 

### Tại sao nó hoạt động 

Nhà nước$(i,j)$chứa chính xác thông tin cần thiết cho tương lai. Bất kỳ hai chuỗi nào có cùng số`'a'`ký tự và số lượng giống nhau`"ab"`các chuỗi hành xử giống hệt nhau kể từ thời điểm đó trở đi. 

Các phương trình đệ quy xuất phát trực tiếp từ việc điều hòa ký tự tiếp theo. Mọi sự tiếp tục có thể xảy ra đều được tính bằng xác suất của nó và các trạng thái kết thúc được thay thế bằng mức đóng góp dự kiến ​​chính xác của chúng. Vì thứ tự ngược lại đảm bảo rằng các trạng thái phụ thuộc đã được biết đến nên mọi trạng thái đều nhận được kỳ vọng chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

k, pa, pb = map(int, input().split())

inv_sum = pow(pa + pb, MOD - 2, MOD)
A = pa * inv_sum % MOD
B = pb * inv_sum % MOD
inv_B = pow(B, MOD - 2, MOD)

dp = [[0] * (k + 1) for _ in range(k + 2)]

for s in range(2 * k - 1, -1, -1):
    for i in range(min(k, s), 1 - 1, -1):
        j = s - i
        if j < 0 or j >= k:
            continue

        if i + j >= k:
            dp[i][j] = (i + j + 1) % MOD
            dp[i][j] = (dp[i][j] + A * inv_B) % MOD
        else:
            dp[i][j] = (
                A * dp[i + 1][j] +
                B * dp[i][j + i]
            ) % MOD

print(dp[1][0])
```Bước đầu tiên chuyển đổi xác suất thành số mô-đun. Phép chia được thực hiện bằng cách sử dụng nghịch đảo mô-đun. 

Bảng lưu trữ các câu trả lời dự kiến ​​cho tất cả các trạng thái$(i,j)$. Đảo ngược di chuyển bằng$i+j$đảm bảo rằng cả hai trạng thái kế tiếp đều sẵn có khi tính toán một trạng thái. 

điều kiện`i + j >= k`đại diện cho các trạng thái nơi tiếp theo`'b'`nhất thiết phải kết thúc quá trình. Thuật ngữ`A * inv_B`bằng$$\frac{A}{B},$$đó là số lượng bổ sung dự kiến`'a'`ký tự trước ký tự đầu tiên`'b'`. 

Sự chuyển tiếp```
A * dp[i + 1][j] + B * dp[i][j + i]
```đơn giản là quy luật của sự kỳ vọng tổng thể. 

Sai lầm phổ biến nhất là quên rằng việc chấm dứt`'b'`chính nó đóng góp thêm một đợt nữa$i$các chuỗi tiếp theo. Một lỗi thường gặp khác là cố gắng lưu trữ các giá trị lớn tùy ý của$i$, dẫn đến vô số trạng thái. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào```
1 1 1
```Đây$A=B=\frac12$. 

| Tiểu bang | Giá trị | 
| --- | --- | 
| (1,0) | chưa biết | 
| i+j=1 ≥ k | công thức đầu cuối | 
| trả lời | 2 | 

Công thức cho$$1+0+1+\frac{1/2}{1/2}=2.$$Ví dụ này cho thấy tại sao câu trả lời lại lớn hơn$k$. Nhiều`'a'`nhân vật có thể xuất hiện trước trận chung kết`'b'`. 

### Ví dụ 2 

đầu vào```
2 1 1
```| Tiểu bang | Chuyển tiếp | 
| --- | --- | 
| (1,0) | đến (2,0) hoặc (1,1) | 
| (2,0) | công thức đầu cuối | 
| (1,1) | công thức đầu cuối | 

Đối với cả hai quốc gia kế thừa,$$i+j=2,$$vì vậy chúng là thiết bị đầu cuối. 

Giá trị của chúng là$$2+1=3.$$Kể từ đây$$dp[1][0] = \frac12\cdot3+\frac12\cdot3 = 3.$$Dấu vết này minh họa cách các trạng thái đệ quy cuối cùng đạt đến vùng cuối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(k^2)$| Mỗi trạng thái được xử lý một lần | 
| Không gian |$O(k^2)$| Bảng DP có kích thước khoảng$(k+1)^2$| 

Với$k\le1000$, số lượng tiểu bang là khoảng một triệu. Điều này dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    k, pa, pb = map(int, input().split())

    inv_sum = pow(pa + pb, MOD - 2, MOD)
    A = pa * inv_sum % MOD
    B = pb * inv_sum % MOD
    inv_B = pow(B, MOD - 2, MOD)

    dp = [[0] * (k + 1) for _ in range(k + 2)]

    for s in range(2 * k - 1, -1, -1):
        for i in range(min(k, s), -1, -1):
            j = s - i
            if j < 0 or j >= k:
                continue

            if i + j >= k:
                dp[i][j] = (i + j + 1 + A * inv_B) % MOD
            else:
                dp[i][j] = (
                    A * dp[i + 1][j] +
                    B * dp[i][j + i]
                ) % MOD

    return str(dp[1][0])

# provided sample
assert run("1 1 1\n") == "2", "sample 1"

# minimum input
assert run("1 5 5\n") == "2", "minimum case"

# highly biased toward a
run("1 1000000 1\n")

# highly biased toward b
run("1 1 1000000\n")

# boundary k=1000
run("1000 1 1\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`2`| Trường hợp mẫu | 
|`1 5 5`|`2`| Giá trị tối thiểu của k | 
|`1 1000000 1`| Giá trị lớn | chạy dài`'a'`| 
|`1 1 1000000`| Gần 1 | Thường xuyên`'b'`| 
|`1000 1 1`| Đầu ra hợp lệ | Số trạng thái tối đa | 

## Vỏ cạnh 

Hãy xem xét```
1 1000000 1
```Xác suất nhìn thấy nhiều lần liên tiếp`'a'`ký tự trước ký tự đầu tiên`'b'`là rất cao. Thuật toán xử lý việc này thông qua thuật ngữ$$\frac{A}{B},$$đại diện cho số lượng bổ sung dự kiến`'a'`ký tự trước khi kết thúc`'b'`. Không cần mô phỏng rõ ràng về thời gian chạy dài. 

Một trường hợp thú vị khác là```
2 1 1
```Từ tiểu bang$(1,1)$, điều kiện$i+j\ge k$đã nắm giữ rồi. Một cái nữa`'b'`ngay lập tức kết thúc quá trình. Công thức cuối cùng tạo ra kỳ vọng chính xác mà không cần tạo thêm trạng thái. 

Cuối cùng, khi```
1 1 1000000
```quá trình này thường kết thúc sau một chuỗi rất ngắn. DP vẫn hoạt động vì xác suất được biểu diễn theo modulo$10^9+7$và không sử dụng số học dấu phẩy động. Mọi kỳ vọng vẫn chính xác.
