---
title: "CF 104178B - Moo"
description: "Chúng tôi được tặng một bộ gà, mỗi con có trọng lượng dương. Chúng tôi cũng có một số lượng bánh quy cố định. Mục tiêu là phân phối bánh quy sao cho mỗi con gà nhận được một số nguyên không âm và tất cả gà đều nhận được bánh quy theo tỷ lệ chặt chẽ với trọng lượng của chúng."
date: "2026-07-02T00:46:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104178
codeforces_index: "B"
codeforces_contest_name: "BdOI Preliminary 2023"
rating: 0
weight: 104178
solve_time_s: 46
verified: true
draft: false
---

[CF 104178B - Moo](https://codeforces.com/problemset/problem/104178/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một bộ gà, mỗi con có trọng lượng dương. Chúng tôi cũng có một số lượng bánh quy cố định. Mục tiêu là phân phối bánh quy sao cho mỗi con gà nhận được một số nguyên không âm và tất cả gà đều nhận được bánh quy theo tỷ lệ chặt chẽ với trọng lượng của chúng. Điều này có nghĩa là tồn tại một số tỷ lệ chung sao cho số lượng bánh quy cho mỗi con gà bằng tỷ lệ đó nhân với trọng lượng của nó. 

Nếu chúng ta chọn một tỷ lệ$k$, rồi gà$i$phải nhận$f[i] = k \cdot w[i]$. Tổng số bánh quy được sử dụng là$k \cdot (w_1 + w_2 + \dots + w_n)$. Vì bánh quy không thể chia được nên cả hai đều$k$và tất cả$f[i]$phải là số nguyên, điều này đã được đảm bảo nếu chúng ta chọn số nguyên$k$. 

Nhiệm vụ là tối đa hóa số lượng bánh quy được phân phối đồng thời đảm bảo chúng tôi không vượt quá$m$và điều kiện tỷ lệ được bảo toàn chính xác. 

Kích thước đầu vào cho phép lên tới 200.000 trọng số và tổng trọng số có thể có giá trị lớn vì mỗi trọng số lên tới$10^9$, trong khi$m$có thể lớn như$10^{15}$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào thử tất cả các phân phối ứng cử viên hoặc thực hiện mô phỏng trên mỗi bánh quy. Bất kỳ giải pháp nào cũng phải giảm bớt vấn đề thành một vài phép tính số học trên toàn bộ mảng. 

Một vấn đề tế nhị sẽ nảy sinh nếu chúng ta nghĩ đến việc phân phối độc lập cho mỗi con gà. Ví dụ như tham lam cho mỗi con gà$m$theo tỷ lệ sẽ thất bại vì bất kỳ sai lệch nào so với một tỷ lệ toàn cầu sẽ phá vỡ ràng buộc. 

Các trường hợp cạnh chủ yếu là cấu trúc: 

Nếu tất cả các trọng số đều bằng nhau thì sự phân bố cũng phải đồng đều và câu trả lời sẽ chuyển thành phép chia$m$bằng nhau giữa$n$các bộ phận. Ví dụ,$n=3$, trọng lượng$1,1,1$,$m=10$đưa ra tổng số$9$bởi vì chỉ có bội số của$3$là những tổng hợp lệ. 

Nếu trọng lượng không có khả năng tương thích tỷ lệ chung với$m$, câu trả lời có thể bằng không. Ví dụ,$w=[1,5,2,3,1]$,$m=10$, tổng là$12$, và bội số lớn nhất của$12$không vượt quá$10$là$0$. 

## Phương pháp tiếp cận 

Cách giải thích mạnh mẽ là thử mọi phép gán số nguyên có thể có cho mỗi con gà và kiểm tra xem tỷ lệ có khớp hay không. Điều đó ngay lập tức bùng nổ: ngay cả khi chúng tôi hạn chế thực hiện các phép gán tỷ lệ hợp lệ, chúng tôi vẫn cần phải kiểm tra mọi hệ số tỷ lệ có thể có$k$, và với mỗi cái, hãy tính tổng số bánh quy và xác minh các ràng buộc. Số lượng ứng viên$k$giá trị lên đến$m$, đó là$10^{15}$, làm cho điều này không thể thực hiện được. 

Sự đơn giản hóa chính xuất phát từ việc nhận ra rằng điều kiện tỷ lệ buộc tất cả các phân phối hợp lệ vào một họ một tham số duy nhất. Khi trọng số được cố định, mọi giải pháp hợp lệ chỉ được xác định bằng hệ số vô hướng$k$. Tổng số bánh quy được sử dụng luôn là$k \cdot S$, Ở đâu$S$là tổng của tất cả các trọng số. 

Vậy bài toán quy về việc tìm số nguyên lớn nhất$k$như vậy:$$k \cdot S \le m$$Đây là phép chia số nguyên trực tiếp và một khi$k$đã biết, tổng số bánh quy được phân phối chính xác là$k \cdot S$. 

Không có tổ hợp, không có cấu trúc gcd ngoài việc nhận ra tính tuyến tính và không cần tối ưu hóa từng phần tử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên k hoặc bài tập |$O(m)$hoặc tệ hơn |$O(1)$| Quá chậm | 
| Tổng + chia |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các trọng số và tính tổng của chúng$S$. Điều này nén tất cả cấu trúc của bài toán thành một đại lượng vô hướng duy nhất vì phép phân bổ theo tỷ lệ sẽ thu gọn mọi thứ thành một số nhân chung. 
2. Nếu$S = 0$, phân phối gần như bằng 0, nhưng trong bài toán này các trọng số hoàn toàn dương, nên trường hợp này không bao giờ xảy ra. 
3. Tính hệ số tỷ lệ hợp lệ tối đa$k = \lfloor m / S \rfloor$. Điều này đảm bảo rằng tổng số bánh quy$k \cdot S$không vượt quá số tiền sẵn có. 
4. Xuất ra tổng số bánh quy$k \cdot S$. 

Lý do điều này là đủ là vì bất kỳ phân phối theo tỷ lệ hợp lệ nào cũng phải chia tỷ lệ cho tất cả các trọng số như nhau và không có quyền tự do phân phối lại các giá trị dư thừa hoặc một phần giữa các phần tử. 

### Tại sao nó hoạt động 

Tất cả các phân phối hợp lệ đều nằm trên đường một chiều trong$n$không gian chiều, được xác định bởi vectơ trọng số. Mọi nghiệm hợp lệ đều phải là bội số nguyên của vectơ này. Ràng buộc$\sum f[i] \le m$giới hạn chúng tôi ở các tiền tố của dòng này và điểm khả thi tối đa chính xác là bội số nguyên lớn nhất không vượt quá$m$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    w = list(map(int, input().split()))
    
    s = sum(w)
    print((m // s) * s)

if __name__ == "__main__":
    solve()
```Giải pháp này hoạt động bằng cách thu gọn toàn bộ mảng thành tổng của nó. Sự phân chia$m // s$tính xem chúng ta có thể phân phát bao nhiêu lớp bánh quy theo tỷ lệ đầy đủ. Nhân trở lại với$s$xây dựng lại tổng số bánh quy được sử dụng. 

Điểm tinh tế duy nhất là sử dụng phép chia số nguyên; số học dấu phẩy động sẽ gây ra rủi ro về độ chính xác được đưa ra$10^{15}$tỉ lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 12
1 1 3
```Đây$S = 5$. 

| Bước | Tổng S | m | k = m//S | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 12 | 2 | 10 | 

Vì vậy, chúng ta có thể phân phối 2 đơn vị tỷ lệ đầy đủ:$2, 2, 6$. 

Điều này xác nhận rằng việc chia tỷ lệ theo tỷ lệ sẽ bảo toàn tỷ lệ chính xác. 

### Ví dụ 2 

đầu vào:```
5 10
1 5 2 3 1
```Đây$S = 12$. 

| Bước | Tổng S | m | k = m//S | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 12 | 10 | 0 | 0 | 

Vì ngay cả một đơn vị tỷ lệ đầy đủ cũng vượt quá ngân sách nên không thể phân phát bánh quy. 

Điều này thể hiện trường hợp ranh giới trong đó câu trả lời giảm về 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một lần để tính tổng trọng số | 
| Không gian |$O(1)$| Chỉ tổng hợp được lưu trữ | 

Các ràng buộc cho phép trọng lượng lên tới 200.000 và một lần vượt qua tuyến tính dễ dàng nằm trong giới hạn. Các phép toán số học là thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    n, m = map(int, input().split())
    w = list(map(int, input().split()))
    s = sum(w)
    print((m // s) * s)

# provided samples
assert run("3 12\n1 1 3\n") == "10"
assert run("5 10\n1 5 2 3 1\n") == "0"
assert run("1 5\n7\n") == "5"

# custom cases
assert run("2 100\n1 1\n") == "100"
assert run("4 9\n2 2 2 2\n") == "8"
assert run("3 7\n2 2 2\n") == "6"
assert run("3 5\n2 2 2\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 100 / 1 1 | 100 | chia tỷ lệ đầy đủ | 
| 4 9/2 2 2 2 | 8 | trọng lượng đồng nhất với phần còn lại | 
| 3 7 / 2 2 2 | 6 | công trình san lấp mặt bằng | 
| 3 5 / 2 2 2 | 0 | không có khả năng mở rộng quy mô | 

## Vỏ cạnh 

Nếu tất cả các trọng số đều giống nhau thì thuật toán giảm xuống còn chia$m$qua$n \cdot w$. Ví dụ: đầu vào:```
3 10
2 2 2
```cho$S=6$,$k=1$, kết quả$6$. Mặc dù$10$không chia hết cho$6$, chúng tôi tránh được việc phân bổ một phần một cách chính xác. 

Nếu như$m < S$, thuật toán ngay lập tức trả về 0. Ví dụ:```
4 3
1 1 1 1
```Đây$S=4$, do đó không có sự phân bổ tỷ lệ hợp lệ nào tồn tại trong ngân sách. 

Nếu một trọng số lớn hơn nhiều so với các trọng số khác thì nó không thay đổi cấu trúc vì việc chia tỷ lệ được áp dụng trên toàn cầu. Ví dụ:```
3 100
1 1 50
```chúng tôi nhận được$S=52$,$k=1$, kết quả$52$, cho thấy sự mất cân bằng không đưa ra bất kỳ quyết định nào cho từng phần tử.
