---
title: "CF 898A - Làm tròn"
description: "Chúng ta được cho một số nguyên không âm và được yêu cầu chuyển đổi nó thành một số gần đó có chữ số cuối cùng bằng 0."
date: "2026-06-17T03:31:30+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 898
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 451 (Div. 2)"
rating: 800
weight: 898
solve_time_s: 63
verified: true
draft: false
---

[CF 898A - Làm tròn](https://codeforces.com/problemset/problem/898/A) 

**Đánh giá:** 800 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên không âm và được yêu cầu chuyển đổi nó thành một số gần đó có chữ số cuối cùng bằng 0. Quy tắc là Vasya muốn "làm tròn" số sao cho nó chia hết cho 10, và trong số tất cả những ứng cử viên như vậy, anh ấy chọn người gần nhất về độ chênh lệch tuyệt đối. Nếu có hai ứng cử viên gần bằng nhau thì kết quả nào cũng được chấp nhận. 

Trong thực tế, mọi số nguyên nằm giữa hai bội số của 10: một số có được bằng cách giảm chữ số cuối cùng xuống 0 và một số có được bằng cách tăng số đó lên bội số tiếp theo của 10. Nhiệm vụ là quyết định xem số nào trong hai số này gần hơn hoặc liệu số đó đã là bội số của 10 hay chưa. 

Ràng buộc đầu vào cực kỳ nhỏ về mặt thuật toán: số lượng tối đa là 10^9. Điều này có nghĩa là bất kỳ phép toán số học theo thời gian không đổi nào cũng đủ và thậm chí quét tuyến tính cũng sẽ là quá mức cần thiết. Cấu trúc của bài toán không phụ thuộc vào cấu trúc dữ liệu phức tạp hoặc phép lặp; it is purely about the last digit.

 Một sai lầm ngây thơ thường xuất hiện ở đây là cố gắng “mô phỏng làm tròn” từng chữ số hoặc lặp đi lặp lại lên hoặc xuống cho đến khi đạt bội số hợp lệ của 10. Điều đó là không cần thiết nhưng vẫn an toàn trong các ràng buộc; rủi ro thực sự là việc xử lý không đúng các trường hợp hòa. 

Ví dụ: nếu n = 5 thì cả 0 và 10 đều là câu trả lời hợp lệ vì chúng cách xa nhau như nhau. Việc triển khai đơn giản luôn ở mức sàn (đến 0) hoặc luôn ở mức trần (đến 10) mà không kiểm tra khoảng cách vẫn có thể vượt qua, nhưng việc so sánh khoảng cách hỗn hợp hoặc không chính xác có thể thất bại nếu được triển khai không nhất quán. 

Một trường hợp cạnh khác là khi n đã kết thúc bằng 0, chẳng hạn như 4720. Mọi giải pháp đúng sẽ trả về số không thay đổi. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ là: bắt đầu từ n và di chuyển xuống dưới cho đến khi chạm bội số của 10, và di chuyển riêng biệt lên trên cho đến khi chạm bội số của 10, sau đó so sánh cái nào gần hơn. Mỗi bước sẽ giảm hoặc tăng thêm 1, vì vậy trong trường hợp xấu nhất (các số kết thúc bằng 1 hoặc 9), việc này cần tới 9 bước cho mỗi hướng. Đây vẫn là thời gian không đổi, nhưng ý tưởng không mở rộng về mặt khái niệm và che khuất cấu trúc thực sự của vấn đề. 

Quan sát quan trọng là chữ số cuối cùng quyết định hoàn toàn mọi thứ. Bội số thấp nhất gần nhất của 10 có được bằng cách trừ chữ số cuối cùng và bội số cao hơn gần nhất có được bằng cách cộng (10 − chữ số cuối cùng), trừ khi chữ số đó đã bằng 0. 

Vì vậy, thay vì mô phỏng chuyển động, chúng tôi tính toán trực tiếp hai ứng cử viên: 

một là n − (n mod 10), cái kia là n + (10 − n mod 10), và chúng ta so sánh khoảng cách của chúng với n. Khoảng cách nhỏ hơn sẽ thắng và trong trường hợp bằng nhau thì có thể in được. 

Điều này làm giảm vấn đề thành một phép toán modulo duy nhất và một vài phép toán số học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) amortized, conceptually iterative | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Read the integer n from input. Toàn bộ quyết định chỉ phụ thuộc vào chữ số cuối cùng của nó. 
2. Tính d = n mod 10. Điều này giúp tách biệt khoảng cách giữa n với bội số dưới gần nhất của 10. 
3. If d equals 0, return n immediately. Con số này đã là mục tiêu hợp lệ nên không cần điều chỉnh. 
4. Tính ứng viên thấp hơn là n − d. Đây là bội số gần nhất của 10 và không vượt quá n. 
5. Tính ứng viên cao hơn là n + (10 − d). Đây là bội số gần nhất của 10 lớn hơn n. 
6. Compare distances from n to both candidates. Nếu (n − dưới) nhỏ hơn hoặc bằng (trên – n), hãy chọn ứng viên thấp hơn; nếu không thì chọn ứng viên cao hơn. 

Quyết định hoàn toàn dựa trên sự gần gũi và các mối quan hệ được giải quyết một cách tùy tiện có lợi cho bên dưới, điều này là hợp lệ. 

### Tại sao nó hoạt động

Every integer can be uniquely written as a multiple of 10 plus a remainder between 0 and 9. The two closest multiples of 10 are exactly the ones obtained by removing that remainder or completing it to 10. Any other multiple of 10 is at least 10 units away. Since both candidates are within at most 9 units, they are guaranteed to be the nearest possible choices. The algorithm therefore examines all possible optimal answers and selects one of them.

 ## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    
    d = n % 10
    
    if d == 0:
        print(n)
        return
    
    lower = n - d
    upper = n + (10 - d)
    
    if n - lower <= upper - n:
        print(lower)
    else:
        print(upper)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã này trích xuất chữ số cuối cùng bằng cách sử dụng số học modulo, mã hóa trực tiếp cấu trúc làm tròn của bài toán. Việc trả về sớm xử lý trường hợp tầm thường khi không cần làm tròn số. Sau đó, hai giá trị ứng cử viên được xây dựng một cách rõ ràng, tránh mọi tìm kiếm lặp lại. Việc so sánh sử dụng tính toán khoảng cách trực tiếp, đảm bảo tính chính xác ngay cả trong trường hợp hòa. 

Một điểm tinh tế là ứng cử viên cao hơn luôn được xác định rõ ngay cả khi d bằng 0, nhưng chúng tôi tránh tính toán nó trong trường hợp đó để làm rõ và duy trì hành vi nhận dạng. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 5 

Chúng tôi tính toán chữ số cuối cùng và các ứng cử viên từng bước. 

| Bước | n | d = n mod 10 | Ứng viên cấp dưới | Ứng cử viên cấp trên | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 5 | 0 | 10 | so sánh | 

Khoảng cách tới phía dưới là 5, khoảng cách đến phía trên cũng là 5, do đó cả hai đều hợp lệ. Thuật toán chọn ứng viên thấp hơn, 0. 

Điều này thể hiện hành vi hòa giải trong đó cả hai bên đều có giá trị như nhau. 

### Ví dụ 2: n = 4722 

| Bước | n | d | Hạ | Thượng | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4722 | 2 | 4720 | 4730 | so sánh | 

Khoảng cách tới phần dưới là 2, khoảng cách đến phần trên là 8. Ứng viên ở phần dưới gần hơn một cách nghiêm ngặt nên kết quả đầu ra là 4720. 

Điều này cho thấy sự bất đối xứng khi số này gần với bội số trước của 10. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học được thực hiện bất kể kích thước đầu vào | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Ràng buộc n 10^9 đảm bảo rằng số học số nguyên vẫn ở mức tầm thường và phù hợp thoải mái với các loại số nguyên 32 bit hoặc 64 bit tiêu chuẩn, do đó, giải pháp sẽ chạy ngay lập tức. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input().strip())
    d = n % 10
    if d == 0:
        return str(n)
    lower = n - d
    upper = n + (10 - d)
    if n - lower <= upper - n:
        return str(lower)
    else:
        return str(upper)

# provided samples
assert run("5\n") in ["0", "10"]

# custom cases
assert run("0\n") == "0", "minimum edge"
assert run("10\n") == "10", "already multiple of 10"
assert run("1\n") == "0", "closer to lower boundary"
assert run("9\n") == "10", "closer to upper boundary"
assert run("4722\n") == "4720", "typical middle case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 | 0 | đầu vào hợp lệ nhỏ nhất | 
| 10 | 10 | số đã được làm tròn | 
| 1 | 0 | ưu tiên giới hạn dưới | 
| 9 | 10 | độ đúng giới hạn trên | 
| 4722 | 4720 | trường hợp nội thất chung | 

## Vỏ cạnh 

Với n = 0, phần còn lại là 0, do đó thuật toán ngay lập tức trả về 0. Không cần xây dựng ứng viên và trường hợp nhận dạng được xử lý trực tiếp. 

Đối với n = 10, d = 0 cũng vậy, do đó, việc thoát sớm tương tự sẽ kích hoạt và trả về 10, duy trì tính chính xác cho các giá trị đã được làm tròn. 

Với n = 1, d = 1 thì dưới = 0 và trên = 10. Thuật toán so sánh khoảng cách 1 và 9 và chọn đúng 0. 

Với n = 9, d = 9, do đó dưới = 0 và trên = 10. Khoảng cách lại là 9 và 1, do đó thuật toán chọn đúng 10. 

Với n = 5, cả hai ứng cử viên đều có khoảng cách như nhau và thuật toán luôn chọn giá trị 0 thấp hơn do`<=`điều kiện có thể chấp nhận được theo quy tắc ràng buộc của bài toán.
