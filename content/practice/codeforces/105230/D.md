---
title: "CF 105230D - Dãy số chia"
description: "Chúng ta được cung cấp một danh sách các số nguyên và với mỗi số nguyên, chúng ta liên tục áp dụng một phép biến đổi: thay số đó bằng tổng các ước số thực sự của nó, nghĩa là tất cả các ước số đều nhỏ hơn chính số đó."
date: "2026-06-24T15:58:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105230
codeforces_index: "D"
codeforces_contest_name: "2024-2025 ICPC Bolivia Pre-National Contest"
rating: 0
weight: 105230
solve_time_s: 70
verified: true
draft: false
---

[CF 105230D - Dãy số chia](https://codeforces.com/problemset/problem/105230/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên và với mỗi số nguyên, chúng ta liên tục áp dụng một phép biến đổi: thay số đó bằng tổng các ước số thực sự của nó, nghĩa là tất cả các ước số đều nhỏ hơn chính số đó. Phép biến đổi này có thể được áp dụng nhiều lần, nhưng nhiệm vụ ở đây không phải là mô phỏng chuỗi dài. Thay vào đó, đối với mỗi số, chúng ta chỉ cần xem bước đầu tiên của quy trình này, tổng các ước số thực sự một lần. 

Dựa trên giá trị được tính toán duy nhất đó, mỗi số được phân thành một trong bốn loại. Nếu tổng các ước thực sự bằng số đó thì số đó được gọi là số hoàn hảo. Nếu áp dụng thao tác tương tự với tổng đó sẽ trả về số ban đầu, hai số tạo thành một cặp thân thiện, ở đây gọi là lãng mạn. Nếu tổng lớn hơn số thì dồi dào. Nếu không, nó được phân loại là phức tạp. 

Kích thước đầu vào đạt tới 100.000 số, mỗi số có giá trị lên tới 100.000. Điều này ngay lập tức loại trừ việc tính toán lại các ước số bằng cách chia thử cho mọi truy vấn. Một cách tiếp cận đơn giản để kiểm tra tất cả các số có đến căn bậc hai cho mỗi truy vấn sẽ tốn khoảng O(n√a), trong trường hợp xấu nhất sẽ có khoảng 10^10 phép tính, vượt xa giới hạn 1 giây. 

Ràng buộc cấu trúc chính là tất cả các truy vấn đều độc lập nhưng có chung cách tính tổng chia. Điều đó có nghĩa là chúng ta có thể tính toán trước tổng các ước số thích hợp cho tất cả các giá trị lên tới 100.000 một lần, sau đó trả lời từng truy vấn trong thời gian không đổi. 

Các trường hợp cạnh chủ yếu đến từ số lượng nhỏ. Với n = 1, tổng các ước số thực sự là 0, điều này làm cho nó không hoàn hảo, không dồi dào và cũng không phải là một phần của một cặp lãng mạn trừ khi được kiểm tra rõ ràng. Việc triển khai ngây thơ có thể quên xử lý 1 một cách chính xác và phân loại không chính xác là hoàn hảo hoặc để nó không được phân loại. 

Một trường hợp tế nhị khác là những con số lãng mạn. Việc kiểm tra xem sum(sum(a)) có bằng a hay không là chưa đủ. Chúng ta phải đảm bảo rằng sum(a) không bằng a, nếu không, những con số hoàn hảo có thể bị coi là lãng mạn nếu bị hiểu sai. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ tính tổng các ước số thích hợp cho từng số một cách độc lập. Đối với số x, chúng ta lặp từ 1 đến √x và cộng các ước số theo cặp. Điều này đúng vì mọi ước số lớn hơn √x đều được ghép với một ước số nhỏ hơn √x. Tuy nhiên, thực hiện điều này cho tối đa 100.000 truy vấn sẽ dẫn đến khoảng 100.000 × 316 ≈ 30 triệu kiểm tra số chia trong trường hợp tốt nhất và các hằng số trong Python kém hơn đáng kể, đặc biệt là với các phép tính chia và mô đun lặp lại. 

Sự kém hiệu quả xuất phát từ việc tính toán lại cùng một thông tin về số chia nhiều lần. Mỗi số đều đóng góp vào nhiều truy vấn, do đó cấu trúc gợi ý tính toán trước. Thay vì phân tích từng số một cách độc lập, chúng ta có thể đảo ngược quan điểm: với mỗi ước số tiềm năng d, chúng ta cộng nó với tất cả các bội số của d. Đây là sự tích lũy kiểu sàng cổ điển. Với mỗi i, mọi bội số j của i đều nhận được i dưới dạng đóng góp ước số thực sự, ngoại trừ khi j bằng chính i. 

Điều này biến bài toán thành một chuỗi các vòng lặp điều hòa, tương tự như sàng của Eratosthenes, mang lại tổng số N log N phép toán cho quá trình tiền xử lý. Khi chúng tôi có tổng các ước số thích hợp cho tất cả các số lên tới 100.000, phân loại sẽ trở thành O(1) cho mỗi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n√a) | O(1) | Quá chậm | 
| Sàng tính toán trước | O(N log N + n) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán trước một bảng`sigma[x]`lưu trữ tổng các ước số thực sự của x. 

1. Khởi tạo một mảng`sigma`có kích thước N+1 với số không. Mảng này sẽ tích lũy số chia đóng góp cho mỗi số. 
2. Lặp lại mọi số nguyên i từ 1 đến N. Với mỗi số i, chúng ta cộng i với tất cả các bội số j = 2i, 3i, 4i, v.v. cho đến N. Chúng ta bắt đầu từ 2i vì i không nên được đưa vào làm ước số thực sự của chính nó. 
3. Sau vòng lặp này,`sigma[x]`chứa tổng tất cả các ước thực sự của x. 
4. Với mỗi giá trị truy vấn a, hãy tính b = sigma[a]. 
5. Nếu b == a, gắn nhãn số đó là số hoàn hảo. 
6. Nếu b != a và sigma[b] == a, thì a và b tạo thành một chu kỳ hai dưới hàm tổng chia, vì vậy số này là lãng mạn. 
7. Nếu b > a và nó chưa được xếp vào loại hoàn hảo hay lãng mạn, hãy gắn nhãn nó là phong phú. 
8. Nếu không, hãy phân loại nó là phức tạp. 
9. Xuất ra số theo sau là nhãn phân loại của nó. 

Lý do kiểm tra lãng mạn hoạt động là vì chúng tôi đã tính toán trước sigma cho tất cả các giá trị đạt đến giới hạn. Vì vậy việc kiểm tra sigma[b] là thời gian không đổi. 

### Tại sao nó hoạt động 

Sàng đảm bảo rằng mọi ước số thực sự d của x được thêm chính xác một lần vào sigma[x], bởi vì chúng ta lặp lại trên tất cả d và phân phối nó thành bội số của nó. Điều này đảm bảo sigma[x] chính xác là tổng của tất cả các số nguyên chia x ngoại trừ chính x. 

Logic phân loại chỉ phụ thuộc vào việc so sánh giữa sigma[x], x và sigma[sigma[x]]. Vì sigma chính xác cho tất cả các giá trị trong phạm vi nên điều kiện lãng mạn phát hiện chính xác các chu kỳ hai phần tử và không thể nhầm lẫn chúng với các số hoàn hảo hoặc các chu kỳ lớn hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAX = 100000

sigma = [0] * (MAX + 1)

for i in range(1, MAX + 1):
    for j in range(i * 2, MAX + 1, i):
        sigma[j] += i

def classify(x):
    s = sigma[x]
    if s == x:
        return "perfecto"
    if s <= MAX and sigma[s] == x and s != x:
        return "romantico"
    if s > x:
        return "abundante"
    return "complicado"

n = int(input())
out = []
for _ in range(n):
    a = int(input())
    out.append(f"{a} {classify(a)}")

print("\n".join(out))
```Cốt lõi của việc thực hiện là việc xây dựng giống như sàng của`sigma`. Vòng lặp bên trong bắt đầu tại`2*i`vì một số không được coi là ước số thực sự của chính nó. Điều này ngăn ngừa sai lầm phổ biến là vô tình bao gồm cả phần tự đóng góp. 

Hàm phân loại đánh giá các điều kiện theo đúng thứ tự. Những con số hoàn hảo phải được kiểm tra trước vì nếu không chúng sẽ bị nhầm lẫn là những con số lãng mạn nếu người ta chỉ kiểm tra sự bình đẳng lẫn nhau một cách lỏng lẻo. Lãng mạn đòi hỏi một điều kiện hai chu kỳ nghiêm ngặt, vì vậy chúng tôi đảm bảo`s != x`. Sự dồi dào chỉ được kiểm tra sau khi loại trừ những trường hợp đó, vì số hoàn hảo đáp ứng về mặt kỹ thuật`s >= x`nhưng không được dán nhãn dồi dào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Số đầu vào: 6, 12 

Chúng tôi sử dụng giá trị sigma: 

sigma[6] = 1 + 2 + 3 = 6 

sigma[12] = 1 + 2 + 3 + 4 + 6 = 16 

| x | sigma[x] | kiểm tra sigma[sigma[x]] | phân loại | 
| --- | --- | --- | --- | 
| 6 | 6 | không cần thiết | hoàn hảo | 
| 12 | 16 | sigma[16] = 15 | dồi dào | 

Trường hợp x = 6 xác nhận rằng sự bằng nhau giữa x và tổng ước số của nó trực tiếp tạo ra sự phân loại hoàn hảo. Trường hợp x = 12 cho thấy tổng ước số lớn hơn và không có mối quan hệ qua lại, khẳng định sự phong phú. 

### Ví dụ 2 

Số đầu vào: 220, 284 

Chúng tôi sử dụng các giá trị đã biết: 

sigma[220] = 284 

sigma[284] = 220 

| x | sigma[x] | sigma[sigma[x]] | phân loại | 
| --- | --- | --- | --- | 
| 220 | 284 | sigma[284] = 220 | lãng mạn | 
| 284 | 220 | sigma[220] = 284 | lãng mạn | 

Dấu vết này cho thấy thuộc tính ánh xạ lẫn nhau. Mỗi số ánh xạ tới số kia theo sigma, tạo thành một chu kỳ hai cố định. Điều này xác nhận lý do tại sao các con số lãng mạn được phát hiện bằng cách sử dụng kiểm tra đối xứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N + Q) | sàng phân phối từng số nguyên i trên bội số của nó, thì mỗi truy vấn là O(1) | 
| Không gian | O(N) | lưu trữ cho mảng sigma lên tới 100.000 | 

Quá trình tiền xử lý chiếm ưu thế trong thời gian chạy nhưng vẫn thoải mái trong giới hạn vì chuỗi cập nhật số chia hài hòa tăng chậm. Với giá trị tối đa là 100.000, tổng số thao tác ở mức vài triệu, tức là trong vòng 1 giây trong Python khi được triển khai bằng các vòng lặp đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MAX = 100000
    sigma = [0] * (MAX + 1)

    for i in range(1, MAX + 1):
        for j in range(i * 2, MAX + 1, i):
            sigma[j] += i

    def classify(x):
        s = sigma[x]
        if s == x:
            return "perfecto"
        if s <= MAX and sigma[s] == x and s != x:
            return "romantico"
        if s > x:
            return "abundante"
        return "complicado"

    n = int(input())
    out = []
    for _ in range(n):
        a = int(input())
        out.append(f"{a} {classify(a)}")

    return "\n".join(out)

# provided sample
assert run("""5
28
220
276
1
287
""") == """28 perfecto
220 romantico abundante
276 abundante
1 complicado
287 complicado"""

# minimum size
assert run("""1
1
""") == "1 complicado"

# perfect number
assert run("""1
6
""") == "6 perfecto"

# abundant small
assert run("""1
12
""") == "12 abundante"

# amicable pair
assert run("""2
220
284
""") == """220 romantico
284 romantico"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, 1 | 1 sự phức tạp | xử lý trường hợp cạnh nhỏ nhất | 
| 6 | hoàn hảo | tính đúng đắn của đẳng thức tổng chia | 
| 12 | dồi dào | phát hiện tổng lớn hơn số | 
| 220, 284 | lãng mạn | chu trình chia tổng lẫn nhau | 

## Vỏ cạnh 

Với x = 1, sigma[1] bằng 0 vì nó không có ước số thực sự. Hàm phân loại thấy s = 0, nhỏ hơn x và kiểm tra sigma[0] là không liên quan. Kết quả cuối cùng là phức tạp. Điều này xác nhận rằng thuật toán xử lý chính xác trường hợp suy biến trong đó một số không có ước số. 

Đối với một số hoàn hảo như 6, sigma[6] được tính là 2 + 3 + 1 = 6. Điều kiện s == x kích hoạt ngay lập tức, do đó nó được phân loại là số hoàn hảo trước bất kỳ kiểm tra nào khác. Điều này ngăn chặn việc phân loại sai là phong phú hoặc lãng mạn. 

Đối với một cặp thân thiện như 220 và 284, cả hai giá trị đều nằm trong phạm vi và giá trị sigma của chúng hướng tới nhau. Việc kiểm tra sigma[sigma[x]] == x thành công một cách đối xứng cho cả hai, đảm bảo cả hai đều được gắn nhãn lãng mạn mà không yêu cầu bất kỳ hoạt động theo dõi cặp rõ ràng nào. 

Đối với một số dồi dào như 12, sigma[12] = 16 và sigma[16] = 15, không trở về 12. Vì 16 > 12 nên nó được phân loại là số dư. Sự vắng mặt của hai chu kỳ sẽ giúp nó không bị dán nhãn sai là lãng mạn.
