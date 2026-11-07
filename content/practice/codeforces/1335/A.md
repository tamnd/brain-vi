---
problem: 1335A
contest_id: 1335
problem_index: A
name: "Candies and Two Sisters"
contest_name: "Codeforces Round 634 (Div. 3)"
rating: 800
tags: ["math"]
answer: passed_samples
verified: true
solve_time_s: 284
date: 2026-06-14
model: gpt-5-5
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2e0db5-5080-83ec-a350-4bcdb1d55f19
---

#CF 1335A - Kẹo Và Hai Chị Em 

**Đánh giá:** 800 
**Thẻ:** toán 
**Mẫu:** gpt-5-5 
**Thời gian giải:** 4 phút 44 giây 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2e0db5-5080-83ec-a350-4bcdb1d55f19 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta đang chia một đống kẹo giống hệt nhau thành hai phần dương. Mỗi trường hợp thử nghiệm đưa ra một số`n`, và chúng ta muốn đếm xem có bao nhiêu cặp có thứ tự khác nhau`(a, b)`tồn tại sao cho cả hai`a`Và`b`là các số nguyên dương, chúng có tổng bằng`n`, và phần thứ nhất lớn hơn phần thứ hai. 

Mặc dù các loại kẹo không thể phân biệt được nhưng thứ tự vẫn quan trọng vì Alice và Betty là những người khác nhau và sự ràng buộc`a > b`tự động loại bỏ các bản sao đối xứng. 

Từ góc độ ràng buộc, số lượng ca kiểm thử có thể lên tới 10.000 và mỗi ca`n`có thể lên tới 2×10^9. Điều đó ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng liệt kê tất cả các cặp`(a, b)`lên tới`n`, vì điều đó sẽ tuyến tính cho mỗi trường hợp thử nghiệm và quá chậm. Giải pháp dự định phải tính toán câu trả lời trong thời gian không đổi cho mỗi trường hợp thử nghiệm. 

Một trường hợp thất bại tinh vi đối với lý luận ngây thơ là do việc quên mất bất đẳng thức nghiêm ngặt. Ví dụ, khi`n = 4`, các phép chia hợp lệ là`(3,1)`chỉ một. Cặp đôi`(2,2)`không hợp lệ ngay cả khi nó tính tổng chính xác và việc thiếu điều kiện đó sẽ bị tính quá mức. Một sai lầm phổ biến khác là điều trị`(a, b)`Và`(b, a)`là hai lựa chọn hợp lệ, sẽ được tính gấp đôi và phá vỡ hoàn toàn điều kiện đặt hàng. 

## Phương pháp tiếp cận 

Một chiến lược bạo lực sẽ thử mọi giá trị có thể có của`a`từ`1`ĐẾN`n - 1`, tính toán`b = n - a`, và đếm nó nếu`a > b`. Điều này đúng, nhưng nó kiểm tra đại khái`n`các ứng cử viên cho mỗi trường hợp thử nghiệm, trong trường hợp xấu nhất sẽ trở thành khoảng 2×10^9 thao tác cho một đầu vào và tối đa 10^13 thao tác tổng thể. Điều đó hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là một lần`a + b = n`, đang chọn`a`tự động sửa lỗi`b`, do đó bài toán rút gọn thành việc đếm xem có bao nhiêu số nguyên`a`thỏa mãn cả hai`a > n - a`Và`a > 0`. Sự bất đẳng thức đơn giản hóa để`2a > n`, Vì thế`a > n/2`. 

Đồng thời, chúng ta cũng phải đảm bảo`b = n - a > 0`, ngụ ý`a < n`. Vì vậy các giá trị hợp lệ của`a`là các số nguyên lớn hơn`n/2`và đúng là ít hơn`n`. 

Điều này biến bài toán thành việc đếm các số nguyên trong một khoảng liền kề, có thể được tính trực tiếp bằng số học thay vì phép lặp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quan sát rằng mọi phân bố hợp lệ đều được xác định đầy đủ bằng cách chọn`a`, từ`b = n - a`. Điều này làm giảm vấn đề đếm các giá trị hợp lệ của`a`. 
2. Dịch ràng buộc`a > b`thành bất đẳng thức bằng phép thay thế:`a > n - a`. 
3. Sắp xếp lại bất đẳng thức để cô lập`a`, cho`2a > n`, ngụ ý`a > n/2`. 
4. Kết hợp điều này với những ràng buộc tích cực. Từ`b > 0`, chúng ta phải có`a < n`, và kể từ đó`a > 0`đã được đảm bảo bởi`a > n/2`vì`n ≥ 1`, phạm vi hiệu quả là`a ∈ (n/2, n)`. 
5. Đếm các số nguyên trong phạm vi này. Số nguyên hợp lệ nhỏ nhất là`floor(n/2) + 1`, và lớn nhất là`n - 1`. Do đó, số lượng là`(n - 1) - (floor(n/2) + 1) + 1`, điều này đơn giản hóa thành`n//2 - 1 + 1 = (n - 1) // 2`. 
6. Trả về giá trị này cho từng test case. 

### Tại sao nó hoạt động 

Mỗi phép chia hợp lệ tương ứng với chính xác một số nguyên`a`trong phạm vi`(n/2, n)`và mọi số nguyên như vậy tạo ra một giá trị hợp lệ`b = n - a`đó là số dương và hoàn toàn nhỏ hơn`a`. Ánh xạ giữa các phần tách hợp lệ và hợp lệ`a`các giá trị là một-một, vì vậy việc đếm hợp lệ`a`các giá trị tương đương với việc đếm các phân phối hợp lệ. Không có cặp không hợp lệ nào được đưa vào vì sự bất đẳng thức đảm bảo cả tính tích cực và thứ tự nghiêm ngặt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    print((n - 1) // 2)
```Giải pháp đọc từng trường hợp thử nghiệm và áp dụng trực tiếp công thức dẫn xuất. biểu hiện`(n - 1) // 2`nắm bắt chính xác số lượng số nguyên lớn hơn`n/2`và ít hơn`n`. Phép chia số nguyên đảm bảo hoạt động sàn chính xác mà không cần thao tác dấu phẩy động. 

Một cạm bẫy phổ biến sẽ là sử dụng`n // 2`thay vào đó, nó bị tắt đi một đối với các giá trị chẵn của`n`, vì nó bao gồm không chính xác trường hợp trung điểm trong đó`a = b`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 7
```Có hiệu lực`a`các giá trị hoàn toàn lớn hơn 3,5 và nhỏ hơn 7, vì vậy`a ∈ {4, 5, 6}`. 

| Bước | Biểu hiện | Giá trị | 
| --- | --- | --- | 
| n | đầu vào | 7 | 
| giới hạn dưới | tầng(n/2)+1 | 4 | 
| giới hạn trên | n-1 | 6 | 
| đếm | 6 - 4 + 1 | 3 | 

Điều này xác nhận 3 phần chia hợp lệ:`(6,1), (5,2), (4,3)`. 

### Ví dụ 2 

đầu vào:```
n = 6
```Có hiệu lực`a`giá trị thỏa mãn`a > 3`Và`a < 6`, Vì thế`a ∈ {4, 5}`. 

| Bước | Biểu hiện | Giá trị | 
| --- | --- | --- | 
| n | đầu vào | 6 | 
| giới hạn dưới | tầng(n/2)+1 | 4 | 
| giới hạn trên | n-1 | 5 | 
| đếm | 5 - 4 + 1 | 2 | 

Điều này khớp với hai phần chia hợp lệ`(5,1)`Và`(4,2)`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm được xử lý trong thời gian không đổi bằng công thức dạng đóng | 
| Không gian | O(1) | Không sử dụng cấu trúc phụ trợ | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì ngay cả đối với 10.000 trường hợp thử nghiệm, công việc chỉ là các phép tính số học đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    t = int(input())
    for _ in range(t):
        n = int(input())
        print((n - 1) // 2)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.strip()

# provided samples
assert run("6\n7\n1\n2\n3\n2000000000\n763243547\n") == "3\n0\n0\n1\n999999999\n381621773"

# custom cases
assert run("1\n1\n") == "0", "minimum edge"
assert run("1\n2\n") == "0", "smallest even"
assert run("1\n4\n") == "1", "first non-trivial even"
assert run("1\n5\n") == "2", "odd case check"
assert run("1\n8\n") == "3", "larger even case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | trường hợp không hợp lệ nhỏ nhất | 
| 2 | 0 | cạnh chẵn nhỏ nhất | 
| 4 | 1 | sự phân chia hợp lệ đầu tiên xuất hiện | 
| 5 | 2 | hành vi kỳ quặc đúng đắn | 
| 8 | 3 | tính đúng đắn của mô hình tăng trưởng | 

## Vỏ cạnh 

cho`n = 1`, không có cách nào để chia thành hai phần dương, vì tổng nhỏ nhất của hai số nguyên dương là 2. Công thức cho`(1 - 1) // 2 = 0`, khớp với tập hợp trống các cặp hợp lệ. 

Vì`n = 2`, cách phân chia duy nhất có thể là`(1,1)`, nhưng nó vi phạm`a > b`, do đó câu trả lời đúng là 0. Công thức cho`(2 - 1) // 2 = 0`, loại trừ chính xác trường hợp trung điểm đối xứng. 

Thậm chí`n`, chẳng hạn như`n = 10`, cặp`(5,5)`không được phép, do đó số đếm chỉ bao gồm các giá trị trên một nửa. Công thức đương nhiên loại trừ điểm giữa vì sự bất đẳng thức nghiêm ngặt được nhúng trong đạo hàm. 

Đối với lớn`n`giống`2 × 10^9`, không có vấn đề tràn nào phát sinh trong Python và việc tính toán vẫn là một phép toán số nguyên duy nhất cho mỗi trường hợp kiểm thử, duy trì tính chính xác và hiệu quả.
