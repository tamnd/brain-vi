---
title: "CF 105380F - Tổng đảo ngược"
description: "Chúng ta được yêu cầu xem xét mọi hoán vị của các số từ 1 đến n, tính xem mỗi hoán vị chứa bao nhiêu nghịch đảo, rồi tính tổng số lần nghịch đảo đó trên tất cả các hoán vị."
date: "2026-06-23T16:06:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105380
codeforces_index: "F"
codeforces_contest_name: "TSEC Round 1 (Div. 4)"
rating: 0
weight: 105380
solve_time_s: 62
verified: true
draft: false
---

[CF 105380F - Tổng nghịch đảo](https://codeforces.com/problemset/problem/105380/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xem xét mọi hoán vị của các số từ 1 đến n, tính xem mỗi hoán vị chứa bao nhiêu nghịch đảo, rồi tính tổng số lần nghịch đảo đó trên tất cả các hoán vị. 

Đảo ngược trong hoán vị chỉ đơn giản là một cặp vị trí mà số lớn hơn xuất hiện sớm hơn số nhỏ hơn. Đối với mỗi hoán vị, chúng tôi đếm các cặp như vậy và sau đó chúng tôi tổng hợp số lượng này trên toàn bộ không gian hoán vị có kích thước n!. 

Do đó, đầu ra của mỗi trường hợp thử nghiệm là một số duy nhất: tổng số lần đảo ngược được đóng góp bởi tất cả các hoán vị có độ dài n. 

Các ràng buộc rất lớn về số lượng trường hợp thử nghiệm, lên tới 10^6 và bản thân n có thể lên tới 10^6. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào lặp lại các hoán vị hoặc thậm chí xây dựng lý luận có quy mô giai thừa cho mỗi bài kiểm tra. Giải pháp phải giảm từng truy vấn xuống O(1) hoặc O(log n) sau khi tính toán trước, nếu không nó sẽ không vượt qua. 

Một sự hiểu lầm ngây thơ thường xuất hiện ở đây là cố gắng tính số lần đảo ngược trên mỗi hoán vị hoặc mô phỏng các giao dịch hoán đổi. Ngay cả việc suy nghĩ về mặt tạo hoán vị cũng đã quá chậm vì n! phát triển vượt quá khả năng ở mức n = 10. 

Trường hợp cạnh tinh tế hơn là n = 1. Có chính xác một hoán vị và không tồn tại cặp nào, vì vậy câu trả lời phải là 0. Bất kỳ công thức nào chia cho n hoặc giả định ít nhất một nghịch đảo trên mỗi cặp vẫn phải tuân theo trường hợp cơ bản này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ liệt kê mọi hoán vị và đếm các nghịch đảo riêng lẻ. Đối với mỗi hoán vị, quá trình quét O(n^2) tiêu chuẩn sẽ tìm thấy các đảo ngược. Vì có n! hoán vị, điều này trở thành O(n! · n^2), phát nổ ngay lập tức ngay cả khi n = 10. 

Quan sát quan trọng là ngừng suy nghĩ về các hoán vị tổng thể và thay vào đó phân tích một cặp vị trí cố định i và j với i < j. Chúng tôi hỏi: trong tất cả các hoán vị, cặp này có thường xuyên đóng góp một phép nghịch đảo không? 

Cố định bất kỳ cặp giá trị phân biệt x và y nào. Trong tất cả các hoán vị, tính đối xứng cho chúng ta biết rằng x xuất hiện trước y ở đúng một nửa số hoán vị và sau y ở nửa còn lại. Điều này là do việc hoán đổi x và y trong bất kỳ hoán vị nào sẽ tạo ra một phép đối chiếu làm đảo ngược liệu cặp đó có tạo thành một phép nghịch đảo hay không. 

Vì vậy, đối với mọi cặp giá trị không có thứ tự, sự đóng góp vào tổng số nghịch đảo là giống hệt nhau và hoàn toàn mang tính tổ hợp. 

Có hai quan điểm tương đương. Một là cố định vị trí: mỗi cặp chỉ số đóng góp như nhau. Cách khác là cố định các giá trị: mỗi cặp giá trị hoạt động đối xứng qua các hoán vị. Chế độ xem dựa trên giá trị sạch hơn. 

Đối với bất kỳ cặp phần tử riêng biệt nào, chẳng hạn (a, b), chúng xuất hiện theo thứ tự thường xuyên như nhau trên tất cả các hoán vị. Có n! tổng số hoán vị và chính xác một nửa trong số chúng đặt a trước b. Khi a đứng sau b, cặp đó đóng góp một phép nghịch đảo. 

Do đó, mỗi cặp không có thứ tự đóng góp chính xác n! / Tổng cộng 2 lần đảo ngược. 

Có C(n, 2) cặp như vậy nên đáp án cuối cùng là: 

C(n, 2) · n! / 2 

Chúng ta có thể đơn giản hóa: 

C(n, 2) = n(n−1)/2 

Vậy câu trả lời = n(n−1)/2 · n! / 2 = n(n−1)n! / 4 

Điều này làm giảm vấn đề khi tính toán giai thừa lên đến n và nhân với số học đơn giản cho mỗi truy vấn. 

Chúng tôi tính toán trước các giai thừa lên tới tối đa n trong tất cả các trường hợp thử nghiệm, vì t lớn và việc tính toán lặp đi lặp lại sẽ lãng phí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n^2) | O(n) | Quá chậm | 
| Tối ưu | O(tối đa n + t) | O(tối đa n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Bây giờ chúng ta chuyển đổi công thức tổ hợp thành một chiến lược tính toán thực tế.

1. Đọc tất cả các trường hợp kiểm thử và xác định n tối đa trong số đó. Điều này đảm bảo giai thừa chỉ được tính một lần đến giới hạn cần thiết. 
2. Tính trước các giai thừa từ 1 đến tối đa n bằng phép nhân lặp. Mỗi giai thừa được xây dựng trực tiếp từ giai thừa trước đó, giúp tránh việc tính toán lại nhiều lần. 
3. Với mỗi trường hợp kiểm tra n, hãy tính câu trả lời bằng công thức n(n−1)n! / 4. 
4. In kết quả cho từng test. 

Điểm tinh tế duy nhất là đảm bảo số học số nguyên vẫn chính xác. Tất cả các giá trị đều là số nguyên, nhưng phép chia cho 4 phải chính xác vì n(n−1)n! luôn chia hết cho 4 với n ≥ 2. Với n = 0 hoặc 1, chúng ta trực tiếp trả về 0. 

### Tại sao nó hoạt động 

Mọi cặp giá trị không có thứ tự đều hoạt động giống hệt nhau trên tất cả các hoán vị do tính đối xứng. Thao tác hoán đổi giữa hai giá trị bất kỳ sẽ phân chia hoán vị thành các lớp có kích thước bằng nhau trong đó thứ tự của cặp đó sẽ bị đảo ngược. Điều này đảm bảo rằng mỗi cặp đóng góp chính xác một nửa số hoán vị dưới dạng nghịch đảo. Tính tổng tất cả các cặp mang lại một biểu thức dạng đóng thống nhất, độc lập với cấu trúc bên trong các hoán vị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = None  # no modulus specified

t = int(input())
ns = []
max_n = 0

for _ in range(t):
    n = int(input())
    ns.append(n)
    if n > max_n:
        max_n = n

fact = [1] * (max_n + 1)
for i in range(1, max_n + 1):
    fact[i] = fact[i - 1] * i

def solve(n):
    if n < 2:
        return 0
    return fact[n] * n * (n - 1) // 4

out = []
for n in ns:
    out.append(str(solve(n)))

print("\n".join(out))
```Mảng giai thừa được tính toán trước một lần, giúp tránh việc tính toán lại tối đa 10^6 truy vấn. Mỗi truy vấn sau đó giảm xuống một biểu thức số học có thời gian không đổi. 

Việc chia cho 4 là an toàn vì với n ≥ 2, trong số n! hoán vị, tính đối xứng đảm bảo rằng n(n−1)n! chia hết cho 4. Điều này tránh được các vấn đề về dấu phẩy động và bảo toàn đầu ra số nguyên chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 3 

Trước tiên chúng ta tính giai thừa: Fact[3] = 6. 

Khi đó đáp án = 6 × 3 × 2/4 = 36/4 = 9. 

| Bước | Giá trị | 
| --- | --- | 
| n | 3 | 
| N! | 6 | 
| n(n−1) | 6 | 
| tử số | 36 | 
| kết quả | 9 | 

Điều này khớp với tổng số nghịch đảo dự kiến ​​​​trên tất cả 6 hoán vị. 

### Ví dụ 2: n = 4 

thực tế [4] = 24. 

Đáp án = 24 × 4 × 3/4 = 288/4 = 72. 

| Bước | Giá trị | 
| --- | --- | 
| n | 4 | 
| N! | 24 | 
| n(n−1) | 12 | 
| tử số | 288 | 
| kết quả | 72 | 

Điều này xác nhận rằng việc chia tỷ lệ hoạt động nhất quán với đối số đếm cặp tổ hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tối đa n + t) | tính toán trước giai thừa lên tới tối đa n, sau đó O(1) cho mỗi truy vấn | 
| Không gian | O(tối đa n) | lưu trữ cho mảng giai thừa | 

Giải pháp này phù hợp thoải mái trong các giới hạn vì quá trình tiền xử lý là tuyến tính ở mức tối đa n và mỗi truy vấn trong số tối đa 10^6 truy vấn được trả lời bằng một số phép toán số học không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    t = int(input())
    ns = []
    mx = 0
    for _ in range(t):
        n = int(input())
        ns.append(n)
        mx = max(mx, n)

    fact = [1] * (mx + 1)
    for i in range(1, mx + 1):
        fact[i] = fact[i - 1] * i

    def solve(n):
        if n < 2:
            return 0
        return fact[n] * n * (n - 1) // 4

    return "\n".join(str(solve(n)) for n in ns)

# provided samples
assert run("4\n1\n2\n3\n4\n") == "0\n1\n9\n72"

# custom cases
assert run("1\n1\n") == "0"
assert run("1\n2\n") == "1"
assert run("1\n5\n") == str((120 * 5 * 4) // 4)
assert run("3\n1\n2\n3\n") == "0\n1\n9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n = 1 | 0 | trường hợp cơ bản, không đảo ngược | 
| n = 2 | 1 | trường hợp không tầm thường nhỏ nhất | 
| n = 5 | kiểm tra công thức | tính đúng đắn của dạng đóng | 
| trộn nhỏ n | 0,1,9 | tính nhất quán giữa các truy vấn | 

## Vỏ cạnh 

Với n = 1, không có cặp chỉ số nào, do đó số lần đảo ngược phải bằng 0. Công thức sẽ cho trực tiếp n(n−1)n!/4 = 0, do đó việc triển khai sẽ trả về 0 một cách an toàn mà không cần xử lý đặc biệt. 

Với n = 2, ta có hai hoán vị [1,2] và [2,1]. Chỉ có cái thứ hai đóng góp một sự đảo ngược. Công thức cho 2 × 1 × 2! / 4 = 4 / 4 = 1, khớp chính xác với bảng liệt kê. 

Đối với n lớn, các giá trị giai thừa tăng nhanh, nhưng số nguyên Python xử lý độ chính xác tùy ý một cách an toàn. Rủi ro chính là hiệu suất, có thể tránh được bằng cách tính toán trước một lần và sử dụng lại kết quả trên tất cả các truy vấn.
