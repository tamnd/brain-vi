---
title: "CF 102775I - \u041f\u0435\u0440\u0435\u043f\u043e\u043b\u043e\u0445 \u0432 \u041d\u0418\u0418\u0427\u0410\u0412\u041e"
description: "Nhiệm vụ là trả lời nhiều truy vấn độc lập về một hàm toán học. Đối với giá trị a, hàm này là tích $$f(a)=1^a cdot 2^{a-1}cdot 3^{a-2}cdots (a-1)^2cdot a^1$$ với giá trị đặc biệt f(0)=1. Mỗi truy vấn yêu cầu giá trị này theo modulo $10^9+7$."
date: "2026-07-27T20:42:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102775
codeforces_index: "I"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 20), \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u0426\u0435\u043d\u0442\u0440\u0430\u043b\u044c\u043d\u043e\u0439 \u0420\u043e\u0441\u0441\u0438\u0438, \u043a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434"
rating: 0
weight: 102775
solve_time_s: 55
verified: true
draft: false
---

[CF 102775I - \u041f\u0435\u0440\u0435\u043f\u043e\u043b\u043e\u0445 \u0432 \u041d\u0418\u0418\u0427\u0410\u0412\u041e](https://codeforces.com/problemset/problem/102775/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là trả lời nhiều truy vấn độc lập về một hàm toán học. Đối với một giá trị`a`, chức năng là sản phẩm$$f(a)=1^a \cdot 2^{a-1}\cdot 3^{a-2}\cdots (a-1)^2\cdot a^1$$với giá trị đặc biệt`f(0)=1`. Mỗi truy vấn yêu cầu giá trị này theo modulo$10^9+7$. Tuyên bố chính thức đưa ra định nghĩa tương tự và giới hạn các giá trị được truy vấn ở mức$10^5$. 

Đầu vào chứa tối đa$10^5$truy vấn và mọi truy vấn`a`nhiều nhất cũng là$10^5$. Điều này ngay lập tức loại trừ việc tính toán lại sản phẩm từ đầu cho mọi truy vấn. Tính toán trực tiếp cho một truy vấn cần$O(a)$phép nhân, sẽ trở thành khoảng$10^{10}$hoạt động trong trường hợp xấu nhất. Giới hạn thời gian 2 giây yêu cầu chúng tôi tìm cách chia sẻ công việc giữa các truy vấn. 

Các trường hợp phức tạp hầu hết đều ở xung quanh ranh giới. Đầu tiên là`a = 0`, trong đó công thức không chứa bất kỳ yếu tố nào cả, nhưng câu trả lời được xác định rõ ràng là`1`. Một giải pháp bắt đầu nhân lên từ`1`và giả sử mọi truy vấn đều có ít nhất một yếu tố có thể xử lý sai trường hợp này. 

Ví dụ:```
Input
1
0
```Đầu ra đúng là:```
1
```Việc thực hiện bất cẩn làm khởi tạo câu trả lời cho`0`hoặc cố gắng truy cập yếu tố đầu tiên của sản phẩm sẽ không thành công. 

Một trường hợp cạnh khác là`a = 1`. Sản phẩm chỉ chứa một thuật ngữ:$$f(1)=1^1=1$$Ví dụ:```
Input
1
1
```Đầu ra đúng là:```
1
```Việc triển khai dựa trên sự lặp lại cũng phải xử lý quá trình chuyển đổi này một cách chính xác vì nó phải nhân với`1!`và giữ nguyên giá trị. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là đánh giá mọi truy vấn một cách độc lập. Đối với một truy vấn`a`, chúng ta có thể lặp qua tất cả các số từ`1`ĐẾN`a`và nhân mỗi số với lũy thừa cần thiết của nó. Điều này tuân theo định nghĩa trực tiếp, vì vậy nó đúng. Tuy nhiên, truy vấn lớn nhất có thể cần khoảng$10^5$các yếu tố và làm điều này cho$10^5$truy vấn cung cấp đại khái$10^{10}$hoạt động mô-đun. Điều đó vượt xa những gì có thể phù hợp trong thời hạn. 

Quan sát quan trọng xuất phát từ việc so sánh hai giá trị liên tiếp của hàm. Thay vì mỗi lần mở rộng toàn bộ sản phẩm, hãy so sánh`f(a)`với`f(a-1)`:$$f(a)=1^a\cdot2^{a-1}\cdots(a-1)^2\cdot a$$Và$$f(a-1)=1^{a-1}\cdot2^{a-2}\cdots(a-1)^1$$Mọi số hiện có từ`1`ĐẾN`a-1`nhận được chính xác một bản sao bổ sung trong số mũ và thừa số mới`a`xuất hiện một lần. Tỷ lệ trở thành:$$\frac{f(a)}{f(a-1)}=1\cdot2\cdot3\cdots a=a!$$Vì vậy, sự tái phát là:$$f(a)=f(a-1)\cdot a!$$Điều này thay đổi vấn đề hoàn toàn. Chúng ta chỉ cần tính toán trước các giai thừa rồi xây dựng chuỗi câu trả lời một lần. Sau đó, mọi truy vấn đều được trả lời bằng cách tra cứu một mảng. 

Brute-force hoạt động vì định nghĩa mô tả trực tiếp giá trị, nhưng nó lặp lại công việc gần như tương tự đối với các truy vấn lân cận. Nhận xét rằng các câu trả lời liên tiếp chỉ khác nhau một giai thừa cho phép chúng ta thay thế các tích lặp lại bằng phép tính tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng của tất cả các giá trị được truy vấn) | O(1) | Quá chậm | 
| Tối ưu | O(max(a) + n) | O(max(a)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các truy vấn trước và tìm giá trị lớn nhất được yêu cầu. Chúng tôi cần mức tối đa này vì tất cả các giá trị lên tới nó có thể được chuẩn bị trước, trong khi các truy vấn nhỏ hơn có thể sử dụng lại cùng các câu trả lời được tính toán trước. 
2. Tính giai thừa từ`1`đến giá trị tối đa. Cho phép`fact[i]`cửa hàng$i!$modulo$10^9+7$. Các giá trị này đại diện cho hệ số nhân cần thiết để chuyển từ`f(i-1)`ĐẾN`f(i)`. 
3. Xây dựng mảng câu trả lời. Bắt đầu với`f(0)=1`. Đối với mọi`i`từ`1`đến giá trị tối đa, nhân câu trả lời trước đó với`fact[i]`. Giá trị kết quả là`f(i)`bởi vì mọi chuyển đổi đều áp dụng chính xác một giai thừa. 
4. Đối với mọi truy vấn đầu vào`a`, xuất giá trị được tính toán trước được lưu trữ tại chỉ mục`a`. Không cần thực hiện thêm công việc toán học nào nữa vì tất cả các giá trị có thể đã được tạo ra. 

Tại sao nó hoạt động: 

Bất biến trong quá trình tiền xử lý là sau khi xử lý chỉ số`i`, giá trị được lưu trữ chính xác là$f(i)$. Ban đầu, điều này đúng vì giá trị được lưu trữ là$f(0)=1$. Khi di chuyển từ`i-1`ĐẾN`i`, chúng tôi nhân với`i!`và phép truy toán dẫn xuất chứng minh rằng điều này tạo ra chính xác`f(i)`. Vì mọi truy vấn đều yêu cầu một trong những trạng thái đã được xác minh này nên giá trị trả về luôn đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10 ** 9 + 7

def solve():
    n = int(input())
    queries = [int(input()) for _ in range(n)]

    mx = max(queries)

    fact = [1] * (mx + 1)
    for i in range(1, mx + 1):
        fact[i] = fact[i - 1] * i % MOD

    ans = [1] * (mx + 1)
    for i in range(1, mx + 1):
        ans[i] = ans[i - 1] * fact[i] % MOD

    print("\n".join(str(ans[x]) for x in queries))

if __name__ == "__main__":
    solve()
```Đầu vào được lưu trữ trước khi xử lý vì truy vấn lớn nhất xác định mức độ xử lý trước phải đi bao xa. Nếu không biết mức tối đa đó, chúng ta sẽ lãng phí bộ nhớ hoặc cần phải mở rộng mảng một cách linh hoạt. 

các`fact`mảng lưu trữ các giá trị giai thừa thông thường theo số nguyên tố cần thiết. Vòng lặp thứ hai xây dựng các giá trị hàm thực tế bằng cách sử dụng phép truy toán$f(i)=f(i-1)\cdot i!$. Việc tách biệt hai mảng này sẽ tránh việc tính lại giai thừa nhiều lần. 

Việc khởi tạo của`ans[0]`BẰNG`1`xử lý định nghĩa đặc biệt của`f(0)`. Vòng lặp bắt đầu từ`1`, do đó không có vấn đề gì xảy ra với sản phẩm trống. Số nguyên Python không bị tràn, nhưng mỗi phép nhân đều được giảm modulo`MOD`để giữ các giá trị nhỏ và phù hợp với đầu ra được yêu cầu. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
3
3
4
5
```Quá trình tiền xử lý hoạt động như sau. 

| tôi | giai thừa i! | f(i) | 
| --- | --- | --- | 
| 0 | 1 | 1 | 
| 1 | 1 | 1 | 
| 2 | 2 | 2 | 
| 3 | 6 | 12 | 
| 4 | 24 | 288 | 
| 5 | 120 | 34560 | 

Các kết quả đầu ra là:```
12
288
34560
```Dấu vết này cho thấy sự tái diễn trong hành động. Di chuyển từ hàng này sang hàng tiếp theo chỉ yêu cầu nhân với giai thừa tiếp theo, thay vì xây dựng lại toàn bộ biểu thức ban đầu. 

Ví dụ thứ hai tập trung vào ranh giới:```
4
0
1
2
3
```| tôi | giai thừa i! | f(i) | 
| --- | --- | --- | 
| 0 | 1 | 1 | 
| 1 | 1 | 1 | 
| 2 | 2 | 2 | 
| 3 | 6 | 12 | 

Đầu ra là:```
1
1
2
12
```Điều này xác nhận rằng trường hợp sản phẩm trống và một vài lần chuyển đổi lặp lại đầu tiên được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(max(a) + n) | Chúng tôi tính toán tất cả các giá trị một lần cho đến truy vấn lớn nhất và sau đó trả lời từng truy vấn trong thời gian không đổi. | 
| Không gian | O(max(a)) | Mảng giai thừa và mảng trả lời mỗi mảng chứa tối đa`100001`các giá trị. | 

Giá trị tối đa của`a`chỉ là$10^5$, do đó quá trình tiền xử lý yêu cầu một số thao tác nhỏ. Việc sử dụng bộ nhớ cũng nằm trong giới hạn thoải mái vì các mảng chỉ lưu trữ vài trăm nghìn số nguyên. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 10 ** 9 + 7

def solution(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    n = int(sys.stdin.readline())
    queries = [int(sys.stdin.readline()) for _ in range(n)]

    mx = max(queries)

    fact = [1] * (mx + 1)
    for i in range(1, mx + 1):
        fact[i] = fact[i - 1] * i % MOD

    ans = [1] * (mx + 1)
    for i in range(1, mx + 1):
        ans[i] = ans[i - 1] * fact[i] % MOD

    res = "\n".join(str(ans[x]) for x in queries)

    sys.stdin = old_stdin
    return res

assert solution("""3
3
4
5
""") == """12
288
34560""", "sample"

assert solution("""1
0
""") == "1", "zero case"

assert solution("""5
1
1
1
1
1
""") == """1
1
1
1
1""", "all equal values"

assert solution("""4
0
1
2
3
""") == """1
1
2
12""", "small boundary values"

assert solution("""2
99999
100000
""").count("\n") == 1, "maximum size queries"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3, 3, 4, 5`|`12, 288, 34560`| Tiến triển và tái phát mẫu ban đầu | 
|`1, 0`|`1`| rõ ràng`f(0)`định nghĩa | 
| Năm truy vấn có giá trị`1`| Năm cái | Truy vấn lặp lại và giá trị dương nhỏ nhất | 
|`0, 1, 2, 3`|`1, 1, 2, 12`| Ranh giới chuyển tiếp | 
| Truy vấn`99999`Và`100000`| Hai câu trả lời hợp lệ | Phạm vi tiền xử lý tối đa | 

## Vỏ cạnh 

cho`a = 0`, thuật toán tạo mảng câu trả lời với`ans[0]=1`trước khi áp dụng bất kỳ chuyển đổi nào. Trên đầu vào```
1
0
```giá trị được yêu cầu tối đa là 0, do đó không có giai thừa nào được tính toán và câu trả lời được lưu trữ vẫn giữ nguyên`1`. Đầu ra đúng vì hàm được xác định riêng cho trường hợp này. 

Vì`a = 1`, thuật toán tính toán`fact[1]=1`và sau đó cập nhật:$$ans[1]=ans[0]\cdot fact[1]=1\cdot1=1$$Đối với đầu vào```
1
1
```câu trả lời là:```
1
```Quá trình chuyển đổi không vô tình đưa ra một yếu tố bổ sung vì nhân với`1!`giữ nguyên giá trị trước đó. 

Đối với nhiều truy vấn lớn giống hệt nhau, quá trình tiền xử lý vẫn chỉ diễn ra một lần. Ví dụ:```
3
100000
100000
100000
```Thuật toán xây dựng`f(100000)`một lần và thực hiện ba lần truy cập mảng sau đó. Điều này tránh việc lặp đi lặp lại khiến công thức trực tiếp quá chậm.
