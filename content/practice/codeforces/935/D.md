---
title: "CF 935D - Fafa và bảng chữ cái cổ"
description: "Chúng ta có hai từ có độ dài bằng nhau trên một bảng chữ cái gồm các ký hiệu 1...m. Một số vị trí đã được biết, trong khi những vị trí khác đã bị xóa và được biểu thị bằng 0. Mỗi vị trí bị xóa được điền độc lập và thống nhất bằng một trong các ký hiệu trong bảng chữ cái m."
date: "2026-06-13T03:22:46+07:00"
tags: ["codeforces", "competitive-programming", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 935
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 465 (Div. 2)"
rating: 1900
weight: 935
solve_time_s: 264
verified: true
draft: false
---

[CF 935D - Fafa và bảng chữ cái cổ](https://codeforces.com/problemset/problem/935/D) 

**Xếp hạng:** 1900 
**Tags:** toán, xác suất 
**Thời gian giải:** 4 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai từ có độ dài bằng nhau trên một bảng chữ cái bao gồm các ký hiệu`1...m`. Một số vị trí đã được biết, trong khi những vị trí khác đã bị xóa và được biểu thị bằng`0`. 

Mỗi vị trí bị xóa sẽ được điền độc lập và thống nhất bằng một trong các vị trí`m`ký hiệu bảng chữ cái. Sau khi thực hiện tất cả các thay thế, chúng ta thu được hai từ hoàn chỉnh. Chúng ta phải tính xác suất để từ đầu tiên có từ vựng lớn hơn từ thứ hai về mặt từ điển. 

Câu trả lời không bắt buộc phải ở dạng phân số. Nếu xác suất là$$\frac{P}{Q},$$chúng ta phải in$$P \cdot Q^{-1} \pmod{10^9+7}.$$Từ$m \le 10^5$, mẫu số luôn nguyên tố cùng với môđun$10^9+7$, do đó tồn tại nghịch đảo mô đun. 

Độ dài của từ cũng có thể đạt tới$10^5$. Bất kỳ thuật toán nào liệt kê rõ ràng việc gán các ký hiệu bị xóa đều không thể thực hiện được. Ngay cả một vị trí chưa biết cũng đã giới thiệu$m$khả năng, và với nhiều vị trí chưa biết, số lần hoàn thành sẽ trở thành cấp số nhân. 

Giới hạn thời gian gợi ý một giải pháp tuyến tính hoặc gần tuyến tính. Việc xử lý từng vị trí một lần là hoàn toàn an toàn, trong khi mọi thứ bậc hai ở$n$bị loại trừ. 

Một số tình huống rất dễ xử lý sai. 

Coi như```
n = 1, m = 2
S1 = [0]
S2 = [1]
```Biểu tượng chưa biết trong`S1`trở thành một trong hai`1`hoặc`2`. Chỉ một trong hai khả năng tạo ra một từ lớn hơn, vì vậy câu trả lời là$1/2$. Một giải pháp xử lý các vị trí không xác định một cách tham lam thay vì theo xác suất sẽ thất bại. 

Coi như```
n = 2, m = 3
S1 = [0, 3]
S2 = [0, 1]
```Vị trí đầu tiên không xác định ngay sự so sánh vì cả hai ký hiệu đều không xác định. Chúng ta phải tính đến xác suất để chúng bằng nhau và tiếp tục ở vị trí thứ hai. Bỏ qua trạng thái "vẫn bằng" sẽ tạo ra kết quả không chính xác. 

Coi như```
n = 2, m = 5
S1 = [2, 1]
S2 = [2, 4]
```Vị trí đầu tiên là bằng nhau và cố định. Vị trí thứ hai quyết định sự so sánh ngay lập tức và câu trả lời chính xác là 0. Quá trình chuyển đổi xác suất sẽ dừng ngay khi gặp phải một cặp không bằng nhau cố định. 

Một trường hợp tế nhị khác là```
n = 1, m = 5
S1 = [0]
S2 = [0]
```Cả hai biểu tượng đều không rõ. Trong số$25$chính xác là nhiệm vụ$10$thỏa mãn`S1 > S2`. Xác suất là$$\frac{10}{25}=\frac{2}{5}.$$Công thức đếm cho hai ký hiệu chưa biết phải được suy ra cẩn thận. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ tạo ra mọi khả năng hoàn thành của cả hai từ, so sánh chuỗi kết quả, đếm kết quả thành công và chia cho tổng số kết quả. 

Giả sử có$k$tổng số vị trí bị xóa. Số lần hoàn thành là$m^k$. Với$m=10^5$Và$k$có khả năng gần$2\cdot10^5$, cái này lớn về mặt thiên văn. Ngay cả đối với các giá trị rất nhỏ, nó trở nên không khả thi. 

Quan sát chính xuất phát từ cách hoạt động của so sánh từ điển. 

Khi so sánh hai từ, chỉ vị trí đầu tiên nơi chúng khác nhau mới quan trọng. Mọi thứ sau vị trí đó đều trở nên không liên quan. Điều này có nghĩa là khi quét từ trái sang phải, chúng ta chỉ cần biết một thông tin: 

Xác suất để tất cả các vị trí trước đó bằng nhau là bao nhiêu? 

Đặt xác suất này là`same`. 

Tại một vị trí, một số phần của bài tập sẽ làm cho từ đầu tiên ngay lập tức lớn hơn. Những kết quả đó góp phần trực tiếp vào câu trả lời cuối cùng. Một phân số khác giữ cho hai tiền tố bằng nhau, cho phép tiếp tục so sánh. 

Điều này biến bài toán thành DP xác suất từ ​​trái sang phải đơn giản chỉ có một trạng thái. 

Với mỗi vị trí ta tính: 

1. Xác suất mà vị trí này tạo ra`S1 > S2`, giả sử tất cả các vị trí trước đó đều bằng nhau. 
2. Xác suất để vị trí này giữ cho chúng bằng nhau. 

Số lượng đầu tiên góp phần trả lời. Cập nhật số lượng thứ hai`same`. 

Mỗi vị trí có thể được xử lý độc lập trong thời gian không đổi, tạo ra$O(n)$giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(m^k)$|$O(k)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

hãy để`MOD = 10^9 + 7`. 

Cho phép`same`biểu thị xác suất mà tất cả các vị trí được xử lý cho đến nay đều bằng nhau. 

Ban đầu,$$same = 1.$$Cho phép`ans`lưu trữ xác suất mà từ đầu tiên đã được biết là lớn hơn về mặt từ điển. 

### 1. Tính toán trước nghịch đảo mô-đun 

Chúng tôi liên tục chia cho`m`Và`m²`, vậy hãy tính$$inv_m = m^{-1} \pmod{MOD}.$$### 2. Quét vị trí từ trái sang phải 

Đối với mỗi vị trí, hãy kiểm tra cặp`(a[i], b[i])`. 

Sự đóng góp phụ thuộc vào bốn trường hợp. 

### 3. Cả hai ký hiệu đều được biết 

Nếu cả hai giá trị đều cố định: 

Nếu`a[i] > b[i]`, thì mọi phép gán được đại diện bởi`same`trở nên thành công. 

Thêm vào`same`đến câu trả lời và dừng lại. 

Nếu như`a[i] < b[i]`, thành công trở nên không thể và chúng ta dừng lại. 

Nếu chúng bằng nhau thì tiếp tục. 

### 4. Ký hiệu đầu tiên chưa biết, ký hiệu thứ hai đã biết 

Giả sử`a[i]=0`Và`b[i]=x`. 

Trong số`m`các giá trị có thể có của`a[i]`:`m-x`giá trị lớn hơn`x`. 

Vì vậy, xác suất mà vị trí này ngay lập tức đưa ra`S1>S2`là$$\frac{m-x}{m}.$$Thêm vào$$same \cdot \frac{m-x}{m}$$để trả lời. 

Chính xác một giá trị giữ sự bình đẳng, cụ thể là`a[i]=x`. 

Cập nhật$$same \leftarrow same \cdot \frac1m.$$### 5. Biết lần đầu, chưa biết lần thứ hai 

Giả sử`a[i]=x`Và`b[i]=0`. 

Trong số`m`sự lựa chọn cho`b[i]`:`x-1`các giá trị nhỏ hơn`x`. 

Do đó xác suất mà vị trí này tạo ra`S1>S2`bằng$$\frac{x-1}{m}.$$Thêm vào$$same \cdot \frac{x-1}{m}.$$Chỉ có một giá trị duy trì sự bình đẳng, vì vậy một lần nữa$$same \leftarrow same \cdot \frac1m.$$### 6. Cả hai ký hiệu đều không rõ 

có$m^2$nhiệm vụ có khả năng như nhau. 

Số cặp có ký hiệu thứ nhất lớn hơn ký hiệu thứ hai là$$\frac{m(m-1)}2.$$Do đó xác suất mà vị trí này ngay lập tức tạo ra`S1>S2`là$$\frac{m(m-1)/2}{m^2} = \frac{m-1}{2m}.$$Thêm vào$$same \cdot \frac{m(m-1)/2}{m^2}.$$Sự bình đẳng xảy ra chính xác`m`bài tập, vậy$$same \leftarrow same \cdot \frac1m.$$### 7. Kết thúc quá trình quét 

Nếu tất cả các vị trí vẫn bằng nhau thì các từ sẽ bằng nhau và không đóng góp vào xác suất. 

đầu ra`ans`. 

### Tại sao nó hoạt động 

Ở mỗi bước,`same`bằng xác suất mà tất cả các vị trí được xử lý trước đó đều bằng nhau sau khi thay thế ngẫu nhiên. 

So sánh từ điển học chỉ phụ thuộc vào vị trí đầu tiên nơi các từ khác nhau. Khi xử lý một vị thế, mọi kết quả đều thuộc đúng một trong ba loại: 

1. Từ đầu tiên trở nên lớn hơn. 
2. Từ thứ hai trở nên lớn hơn. 
3. Vị trí vẫn bình đẳng. 

Loại đầu tiên đóng góp vĩnh viễn cho câu trả lời cuối cùng. Loại thứ hai không bao giờ có thể đóng góp sau này. Loại thứ ba là loại duy nhất cho phép các vị trí trong tương lai có ý nghĩa quan trọng, do đó xác suất của nó trở thành giá trị mới của`same`. 

Vì các danh mục này phân chia tất cả các phép gán có thể có và mọi vị trí được xử lý trong điều kiện tất cả các vị trí trước đó đều bằng nhau, nên xác suất tích lũy chính xác là xác suất mà từ đầu tiên lớn hơn về mặt từ điển. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000007

def main():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    inv_m = pow(m, MOD - 2, MOD)
    inv_m2 = inv_m * inv_m % MOD
    inv2 = (MOD + 1) // 2

    same = 1
    ans = 0

    for x, y in zip(a, b):
        if x == 0 and y == 0:
            greater_pairs = m * (m - 1) // 2
            ans = (ans + same * (greater_pairs % MOD) % MOD * inv_m2) % MOD
            same = same * inv_m % MOD

        elif x == 0:
            ans = (ans + same * (m - y) % MOD * inv_m) % MOD
            same = same * inv_m % MOD

        elif y == 0:
            ans = (ans + same * (x - 1) % MOD * inv_m) % MOD
            same = same * inv_m % MOD

        else:
            if x > y:
                ans = (ans + same) % MOD
                break
            elif x < y:
                break

    print(ans)

if __name__ == "__main__":
    main()
```Biến`same`là trạng thái trung tâm của thuật toán. Nó luôn thể hiện xác suất mà mọi vị trí được xử lý đều bằng nhau. 

Ba trường hợp xác suất tương ứng trực tiếp với các đối số đếm từ hướng dẫn. Vì tất cả số học được thực hiện modulo`MOD`, phép chia được thay thế bằng phép nhân với nghịch đảo mô-đun. 

Trường hợp có hai biểu tượng không xác định đáng được quan tâm đặc biệt. Số cặp đặt hàng`(u,v)`thỏa mãn`u>v`không phải`m²/2`. Các cặp bằng nhau phải được loại trừ trước tiên. Số đếm đúng là$$1+2+\cdots+(m-1)=\frac{m(m-1)}2.$$Một chi tiết tinh tế khác là dừng lại khi hai ký hiệu cố định khác nhau. Một khi vị trí khác nhau đầu tiên đã được xác định thì các vị trí sau đó không bao giờ có thể ảnh hưởng đến kết quả từ điển. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào```
1 2
0
1
```| Vị trí | một [tôi] | b[i] | trước đây cũng vậy | Đóng góp | tương tự sau | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 | (2-1)/2 = 1/2 | 1/2 | 1/2 | 

Biểu tượng chưa biết duy nhất có thể là`1`hoặc`2`. Chính xác một lựa chọn sẽ tạo ra một từ lớn hơn, vì vậy xác suất là$1/2$. 

### Ví dụ 2 

đầu vào```
2 3
0 3
0 1
```| Vị trí | một [tôi] | b[i] | trước đây cũng vậy | Đóng góp | tương tự sau | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 1 | 3/9 = 1/3 | 1/3 | 1/3 | 
| 2 | 3 | 1 | 1/3 | 1/3 | dừng lại | 2/3 | 

Ở vị trí đầu tiên, một phần ba tổng số nhiệm vụ đã đáp ứng`S1>S2`. Một phần ba khác làm cho các ký hiệu đầu tiên bằng nhau, để vị trí thứ hai quyết định. Từ`3>1`, tất cả xác suất còn lại đó được cộng lại, cho$2/3$. 

Dấu vết này thể hiện sự bất biến chính:`same`luôn đại diện cho khối lượng xác suất tồn tại ở các vị trí trong tương lai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi vị trí được xử lý một lần | 
| Không gian |$O(1)$| Chỉ có một số biến được duy trì | 

Với$n \le 10^5$, quét tuyến tính dễ dàng đủ nhanh. Việc sử dụng bộ nhớ không đổi bất kể kích thước đầu vào. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

MOD = 1000000007

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    inv_m = pow(m, MOD - 2, MOD)
    inv_m2 = inv_m * inv_m % MOD

    same = 1
    ans = 0

    for x, y in zip(a, b):
        if x == 0 and y == 0:
            greater_pairs = m * (m - 1) // 2
            ans = (ans + same * (greater_pairs % MOD) % inv_m2) % MOD
            same = same * inv_m % MOD

        elif x == 0:
            ans = (ans + same * (m - y) % MOD * inv_m) % MOD
            same = same * inv_m % MOD

        elif y == 0:
            ans = (ans + same * (x - 1) % MOD * inv_m) % MOD
            same = same * inv_m % MOD

        else:
            if x > y:
                ans = (ans + same) % MOD
                break
            elif x < y:
                break

    return str(ans)

# provided sample
assert run("1 2\n0\n1\n") == "500000004"

# minimum size, fixed equal symbols
assert run("1 5\n3\n3\n") == "0"

# minimum size, fixed greater
assert run("1 5\n4\n2\n") == "1"

# both unknown, m=2 => probability 1/4
assert run("1 2\n0\n0\n") == "250000002"

# fixed equal prefix, later position decides
assert run("2 5\n2 5\n2 1\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 5 / 3 / 3`|`0`| Những từ bằng nhau không bao giờ được tính | 
|`1 5 / 4 / 2`|`1`| Chiến thắng cố định ngay lập tức | 
|`1 2 / 0 / 0`|`250000002`| Đếm đúng hai ký hiệu chưa biết | 
|`2 5 / 2 5 / 2 1`|`1`| Tiền tố bằng nhau theo sau là vị trí quyết định cố định | 

## Vỏ cạnh 

Hãy xem xét```
1 2
0
0
```Có bốn nhiệm vụ có khả năng xảy ra như nhau:```
(1,1)
(1,2)
(2,1)
(2,2)
```Chỉ một`(2,1)`thỏa mãn`S1>S2`, vậy xác suất là`1/4`. Thuật toán tính toán$$\frac{2(2-1)/2}{2^2} = \frac14,$$phù hợp với số lượng chính xác. 

Coi như```
2 5
2 1
2 4
```Các ký hiệu đầu tiên bằng nhau nên quá trình xử lý vẫn tiếp tục. Ở vị trí thứ hai,`1<4`, nghĩa là vị trí khác nhau đầu tiên đã thiên về từ thứ hai. Thuật toán ngay lập tức dừng lại và trả về số 0. 

Coi như```
2 3
0 3
0 1
```Vị trí đầu tiên đóng góp khối lượng xác suất cho ba nhóm: lớn hơn, nhỏ hơn và bằng nhau. Chỉ có nhóm bằng nhau sống sót ở vị trí thứ hai. bản cập nhật$$same \leftarrow same \cdot \frac13$$nắm bắt chính xác xác suất sống sót đó. Khi vị trí thứ hai được xử lý, mọi nhiệm vụ còn sót lại đều thắng vì`3>1`. 

Những ví dụ này minh họa bất biến cơ bản:`same`luôn bằng xác suất mà tất cả các vị trí trước đó vẫn bằng nhau, đó chính xác là điều kiện cần thiết để các vị trí sau ảnh hưởng đến việc so sánh từ điển.
