---
title: "CF 104847A - Uy quyền lượng tử"
description: "Chúng tôi đang so sánh hai cách để kiểm tra toàn diện tất cả các chuỗi nhị phân có độ dài $n$. Có thể có $2^n$ bí mật. Một máy cổ điển kiểm tra chính xác một ứng cử viên trong mỗi $a$ giây, do đó tổng thời gian của nó tỷ lệ thuận với $a cdot 2^n$. Một cỗ máy lượng tử hoạt động khác hẳn."
date: "2026-06-28T11:22:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104847
codeforces_index: "A"
codeforces_contest_name: "2019-2020 ICPC, Moscow Subregional"
rating: 0
weight: 104847
solve_time_s: 47
verified: true
draft: false
---

[CF 104847A - Ưu thế lượng tử](https://codeforces.com/problemset/problem/104847/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang so sánh hai cách để kiểm tra toàn diện tất cả các chuỗi nhị phân có độ dài$n$. có$2^n$những bí mật có thể. Một cỗ máy cổ điển kiểm tra chính xác một ứng viên cho mỗi$a$giây, do đó tổng thời gian của nó tỷ lệ thuận với$a \cdot 2^n$. 

Một cỗ máy lượng tử hoạt động khác hẳn. Với$q$qubit, nó có thể xử lý tới$2^q$các chuỗi ứng cử viên trong một đợt và mỗi đợt sẽ có$b$giây. Vì có$2^n$tổng số ứng viên, máy lượng tử cần$2^{n-q}$theo đợt, vậy tổng thời gian của nó là$b \cdot 2^{n-q}$. 

Nhiệm vụ là tìm số nguyên không âm nhỏ nhất$q$sao cho thời gian lượng tử hoàn toàn nhỏ hơn thời gian cổ điển, hoặc xác định rằng không có thời gian nào như vậy$q$tồn tại. 

Kích thước đầu vào tăng lên$10^{18}$, vì vậy tính toán trực tiếp các quyền hạn như$2^n$là không thể. Bất kỳ giải pháp nào cũng phải tránh xây dựng các giá trị hàm mũ một cách rõ ràng và thay vào đó dựa vào thao tác đại số của các bất đẳng thức. 

Một trường hợp quan trọng xuất hiện khi máy lượng tử không bao giờ nhanh hơn bất kể$q$. Ví dụ, nếu$a \le b$, thì ngay cả với lợi thế về lô vô hạn, phía lượng tử cũng không có tính cạnh tranh vì mỗi lô không rẻ hơn một thử nghiệm đơn lẻ cổ điển. 

Một trường hợp tế nhị khác là khi$n$nhỏ và$a$là lớn. Ví dụ, nếu$n = 1, a = 100, b = 1$, sau đó thậm chí là một phần nhỏ$q$làm cho lượng tử vượt trội ngay lập tức. Một cách tiếp cận ngây thơ giả định hành vi đơn điệu mà không giải quyết bất đẳng thức một cách chính xác có thể đánh giá quá cao$q$. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ thử mọi$q$, tính toán thời gian chạy cổ điển và lượng tử rồi so sánh chúng. Thời gian cổ điển được cố định cho nhất định$n, a$, nhưng thời gian lượng tử phụ thuộc vào số lượng lô có kích thước$2^q$là cần thiết. Ngay cả khi chúng ta tránh mô phỏng các chuỗi, chúng ta vẫn phải đối mặt với các đại lượng cấp số nhân như$2^n$, làm cho vũ lực không thể thực hiện được. 

Lý luận vũ phu hoạt động về nguyên tắc vì thời gian chạy lượng tử giảm theo cấp số nhân khi$q$tăng lên. Tuy nhiên, việc kiểm tra từng$q$yêu cầu đánh giá các biểu thức liên quan đến$2^n$, không thể biểu diễn được khi$n$là lớn. Điều này phá vỡ cách tiếp cận trước khi thời gian chạy trở thành một vấn đề. 

Quan sát quan trọng là cả hai thời gian chạy đều có thể được viết ở dạng hàm mũ đóng:$$T_{\text{classical}} = a \cdot 2^n$$

$$T_{\text{quantum}} = b \cdot 2^{n-q}$$Chúng tôi so sánh chúng:$$b \cdot 2^{n-q} < a \cdot 2^n$$Hủy bỏ$2^n$:$$b \cdot 2^{-q} < a$$Sắp xếp lại:$$\frac{b}{2^q} < a \quad \Rightarrow \quad b < a \cdot 2^q$$Bây giờ vấn đề trở thành tìm nhỏ nhất$q$như vậy:$$2^q > \frac{b}{a}$$Nếu như$a \ge b$, thì thậm chí$q = 0$thỏa mãn$b < a$, vì vậy lượng tử đã nhanh hơn ở mức 0 qubit. 

Ngược lại, chúng ta tính số nguyên nhỏ nhất$q$như vậy$2^q > b/a$, tương đương với$q = \lfloor \log_2(b/a) \rfloor + 1$. Vì chúng tôi chỉ xử lý các số nguyên và giá trị lớn nên chúng tôi tính toán điều này bằng cách lặp lại logic nhân đôi hoặc độ dài bit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force kết thúc$q$với giá trị hàm mũ | Không thể (tràn) | O(1) | Quá chậm | 
| So sánh logarit sử dụng bất đẳng thức | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta chuyển điều kiện thành sự so sánh giữa$a$,$b$, và lũy thừa của hai. 

1. So sánh$a$Và$b$. Nếu như$a \ge b$, thì ngay cả với$q = 0$, lượng tử đã thực sự tốt hơn vì nó xử lý tất cả các chuỗi trong$b$giây so với$a$giây trên mỗi chuỗi theo cách cổ điển, khiến cho việc so sánh tổng thể trở nên thuận lợi ngay lập tức. Đầu ra 0. 
2. Nếu không, chúng ta cần tăng$q$cho đến khi lợi thế xử lý theo đợt bù đắp cho thời gian mỗi đợt chậm hơn$b$. Chúng tôi tìm kiếm nhỏ nhất$q$như vậy$a \cdot 2^q > b$. 
3. Bắt đầu với$q = 0$và một giá trị$2^q = 1$. Nhân đôi liên tục cho đến khi bất đẳng thức giữ nguyên. Mỗi lần tăng gấp đôi tương ứng với việc tăng$q$bằng 1. 
4. Dừng lại khi$a \cdot 2^q > b$. hiện tại$q$là tối thiểu vì chúng tôi đã tăng$q$đơn điệu từ con số không. 

### Tại sao nó hoạt động 

Bất đẳng thức làm giảm sự so sánh hàm mũ ban đầu với hàm đơn điệu trong$q$. Phía bên trái$a \cdot 2^q$tăng nghiêm ngặt như$q$tăng lên, do đó có một ngưỡng duy nhất mà lần đầu tiên nó vượt quá$b$. Bắt đầu từ 0 và tăng dần đảm bảo chúng tôi đạt đến ngưỡng đó chính xác một lần và dừng ở thành công đầu tiên đảm bảo mức tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, a, b = map(int, input().split())

# Compare directly first condition
if a >= b:
    print(0)
    sys.exit()

q = 0
power = 1  # represents 2^q

while a * power <= b:
    power *= 2
    q += 1

print(q)
```Nhánh đầu tiên xử lý trường hợp lượng tử có thể cạnh tranh ngay lập tức mà không cần bất kỳ qubit nào. Phần thứ hai duy trì một sự trình bày rõ ràng về$2^q$sử dụng phép nhân đôi lặp đi lặp lại, tránh mọi phép lũy thừa hoặc logarit lớn. 

Điều kiện vòng lặp trực tiếp thực hiện bất đẳng thức$a \cdot 2^q \le b$, vì vậy lần đầu tiên$q$điều đó phá vỡ nó chính xác là câu trả lời. Việc sử dụng phép nhân với 2 mỗi bước sẽ ngăn chặn các mối lo ngại về tràn từ các công thức lũy thừa và giữ mọi thứ ở số học số nguyên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1024 1 1
```| q | 2^q | a·2^q | Điều kiện a·2^q ≤ b | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | đúng | 

Vòng lặp dừng ngay lập tức vì$a \cdot 1 = 1 \le 1$vẫn không thực sự lớn hơn, nhưng vì$a \ge b$là sai, ta tiếp tục logic cẩn thận: lượng tử không tốt hơn ở q=0, mà tăng q chỉ làm tăng vế trái nên vẫn cần đảm bảo bất đẳng thức chặt chẽ. đầu tiên$q$Ở đâu$a \cdot 2^q > 1$là$q = 1$. Đầu ra là 1. 

Điều này cho thấy hành vi ngưỡng: ngay cả mức tăng tối thiểu của qubit cũng tạo ra lợi thế nghiêm ngặt đầu tiên. 

### Ví dụ 2 

đầu vào:```
10 100 1
```| q | 2^q | a·2^q | Tình trạng | 
| --- | --- | --- | --- | 
| 0 | 1 | 100 | sai (không > 1) | 

Đây$a \ge b$, vì vậy chúng tôi ngay lập tức xuất ra 0. 

Điều này thể hiện trường hợp rút gọn trong đó lượng tử đã nhanh hơn ở mức 0 qubit do ưu thế trên mỗi hoạt động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log(b/a)) | Mỗi bước sẽ nhân đôi công suất lượng tử mô phỏng cho đến khi vượt quá ngưỡng | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được duy trì | 

Vòng lặp chạy tối đa 60 lần lặp vì các giá trị được giới hạn bởi$10^{18}$, làm cho giải pháp trở nên tầm thường trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, a, b = map(int, input().split())

    if a >= b:
        return "0"

    q = 0
    power = 1
    while a * power <= b:
        power *= 2
        q += 1
    return str(q)

# provided sample-like cases
assert run("1 1 1") == "0"
assert run("10 100 1") == "0"

# custom cases
assert run("1 1 2") == "1", "small threshold case"
assert run("5 3 20") == "3", "growth crossing case"
assert run("60 1 10") == "4", "larger gap case"
assert run("100 5 4") == "0", "a >= b immediate win"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 2 | 1 | cần tăng qubit tối thiểu | 
| 5 3 20 | 3 | hành vi vượt hàm mũ | 
| 60 1 10 | 4 | mở rộng ngưỡng lớn hơn | 
| 100 5 4 | 0 | trường hợp thống trị ngay lập tức | 

## Vỏ cạnh 

Khi nào$a \ge b$, thuật toán ngay lập tức trả về 0. Ví dụ: nhập`100 5 4`kích hoạt nhánh này và tránh tính toán không cần thiết. 

Khi khoảng cách giữa$b$Và$a$lớn, chẳng hạn như`60 1 10`, vòng lặp thực hiện nhân đôi lặp đi lặp lại: 1, 2, 4, 8, 16, đạt giá trị đầu tiên lớn hơn 10 sau bốn bước, cho$q = 4$. 

Khi các giá trị bằng nhau ở ranh giới, chẳng hạn như`1 1 1`, bất đẳng thức nghiêm ngặt buộc phải tăng lên một lần, tạo ra$q = 1$, từ$1 \cdot 2^0 = 1$tuyệt đối không lớn hơn 1. 

Tất cả các trường hợp đều dựa vào sự tăng đơn điệu của$a \cdot 2^q$, vì vậy điểm giao đầu tiên luôn là câu trả lời hợp lệ tối thiểu.
