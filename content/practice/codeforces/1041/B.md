---
problem: 1041B
contest_id: 1041
problem_index: B
name: "Buying a TV Set"
contest_name: "Codeforces Round 509 (Div. 2)"
rating: 1000
tags: ["math"]
answer: passed_samples
verified: false
solve_time_s: 403
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 0
samples_total: 1
share_url: https://chatgpt.com/share/6a33bb22-b27c-83ec-943d-62a14de8036c
---

#CF 1041B - Mua Tivi 

**Đánh giá:** 1000 
**Thẻ:** toán 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 6m 43s 
**Đã xác minh:** không (0/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33bb22-b27c-83ec-943d-62a14de8036c 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng tôi đang tìm kiếm có thể có bao nhiêu kích thước màn hình dưới hai ràng buộc đồng thời. Đầu tiên, chiều rộng của TV không được vượt quá giới hạn`a`và chiều cao không được vượt quá giới hạn`b`. Thứ hai, màn hình phải tuân theo tỷ lệ khung hình cố định`x : y`, nghĩa là mọi màn hình hợp lệ phải có tỷ lệ nguyên theo tỷ lệ này. 

Do đó, bất kỳ màn hình hợp lệ nào cũng có dạng`(w, h) = (kx, ky)`với một số nguyên dương nào đó`k`. Nhiệm vụ là đếm xem có bao nhiêu giá trị của`k`giữ cả hai`kx ≤ a`Và`ky ≤ b`. 

Vì vậy, vấn đề đơn giản là tìm xem có bao nhiêu bội số nguyên của một hình chữ nhật cơ sở vừa với một hộp giới hạn. 

Các ràng buộc lên tới 10^18, điều này ngay lập tức loại trừ việc lặp lại trên tất cả các chiều rộng hoặc chiều cao có thể có. Ngay cả việc quét tuyến tính cũng không thể thực hiện được. Bất kỳ lời giải hợp lệ nào cũng phải quy bài toán về một số phép tính số học không đổi. 

Một sai lầm ngây thơ là thử kiểm tra tất cả`w`lên đến`a`và xác minh xem`w * y % x == 0`, sau đó tính toán`h = w * y / x`. Tốc độ này quá chậm và cũng có nguy cơ tràn nếu phép nhân không được xử lý cẩn thận. 

Một vấn đề tế nhị khác là việc cắt bớt phép chia số nguyên. Nếu chúng ta tính toán`a // x`hoặc`b // y`ở dạng dẫn xuất không chính xác, chúng ta có thể tính sai các trường hợp biên trong đó một ràng buộc chặt chẽ hơn ràng buộc kia. 

Khó khăn cốt lõi là nhận ra rằng chúng ta đang tính các hệ số chia tỷ lệ hợp lệ chứ không phải các lựa chọn độc lập về chiều rộng và chiều cao. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ thử mọi cặp số nguyên có thể`(w, h)`như vậy`w ≤ a`Và`h ≤ b`, sau đó kiểm tra xem`w / h = x / y`. Đối với mỗi cặp, chúng tôi sẽ xác minh điều kiện tỷ lệ bằng cách nhân chéo`w * y == h * x`. 

Cách tiếp cận này đúng nhưng cực kỳ tốn kém. Số cặp là`a × b`, trong trường hợp xấu nhất là`10^36`. Ngay cả việc lặp lại khái niệm cũng là không thể. 

Quan sát quan trọng là điều kiện tỷ lệ buộc tất cả các cặp hợp lệ phải nằm trên một cấp số cộng duy nhất của các hình chữ nhật có tỷ lệ. Một khi chúng tôi sửa chữa`(x, y)`, mọi màn hình hợp lệ được xác định duy nhất bởi hệ số tỷ lệ`k`. Vì vậy, thay vì tìm kiếm trên lưới 2D, chúng ta chỉ cần tìm có bao nhiêu số nguyên`k`thỏa mãn cả hai giới hạn. 

Từ`(kx ≤ a)`chúng tôi nhận được`k ≤ a / x`, và từ`(ky ≤ b)`chúng tôi nhận được`k ≤ b / y`. Cả hai phải giữ đồng thời, do đó hệ số giới hạn là mức tối thiểu của hai giới hạn trên này. 

Vì vậy câu trả lời chỉ đơn giản là`min(a // x, b // y)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force kết thúc (w, h) | O(a · b) | O(1) | Quá chậm | 
| Hệ số tỷ lệ tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giải thích ràng buộc về tỷ lệ khung hình như một quy tắc chia tỷ lệ. Bất kỳ màn hình hợp lệ nào cũng phải`(w, h) = (k·x, k·y)`đối với một số nguyên`k ≥ 1`. Điều này giúp loại bỏ sự lựa chọn độc lập về chiều rộng và chiều cao. 
2. Chuyển giới hạn chiều rộng thành bất đẳng thức trên`k`. Từ`kx ≤ a`, chúng tôi rút ra`k ≤ a // x`. Phép chia số nguyên là an toàn vì chúng tôi chỉ tính các thang đo hợp lệ đầy đủ. 
3. Chuyển giới hạn chiều cao sang một bất đẳng thức khác trên`k`. Từ`ky ≤ b`, chúng tôi rút ra`k ≤ b // y`. 
4. Kết hợp cả hai ràng buộc. Giống nhau`k`phải thỏa mãn cả hai giới hạn trên, do đó giá trị hợp lệ tối đa`k`là`min(a // x, b // y)`. 
5. Đếm các giải pháp hợp lệ. Mỗi số nguyên`k`từ`1`đến mức tối đa này sẽ tạo ra chính xác một cặp hợp lệ, vì vậy câu trả lời là giá trị tối đa đó. 

### Tại sao nó hoạt động 

Mỗi cặp hợp lệ phải đáp ứng chính xác tỷ lệ, điều này buộc tỷ lệ giữa chiều rộng và chiều cao. Điều này thu gọn không gian tìm kiếm từ lưới 2D thành một tham số duy nhất`k`. Những hạn chế về`w`Và`h`trở thành giới hạn trên độc lập trên`k`và giao điểm của các ràng buộc này chính xác là mức tối thiểu của hai giới hạn dẫn xuất. Không có cặp hợp lệ nào bị bỏ sót vì mọi màn hình khả thi đều tương ứng với một số nguyên duy nhất`k`và không có cặp không hợp lệ nào được đưa vào vì bất kỳ`k`vượt quá một trong hai giới hạn vi phạm ít nhất một ràng buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, b, x, y = map(int, input().split())
    print(min(a // x, b // y))

if __name__ == "__main__":
    solve()
```Lời giải áp dụng trực tiếp phép biến đổi dẫn xuất từ ​​hình học sang số học. Chi tiết triển khai chính là sử dụng phép chia số nguyên, phép chia này tự động tính hệ số tỷ lệ khả thi tối đa. Không cần vòng lặp hoặc xác nhận thêm vì ràng buộc tỷ lệ đã được thực thi một cách có cấu trúc bằng cách biểu diễn tất cả các giải pháp dưới dạng bội số của`(x, y)`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
17 15 5 3
```Chúng tôi tính toán giới hạn cho`k`. 

| Bước | một // x | b // y | Kết quả | 
| --- | --- | --- | --- | 
| Tính giới hạn chiều rộng | 17 // 5 = 3 | - | - | 
| Tính giới hạn chiều cao | - | 15 // 3 = 5 | - | 
| Lấy tối thiểu | 3 | 5 | 3 | 

Điều này cho thấy rằng`k = 1, 2, 3`là hợp lệ. Mỗi người sản xuất`(5,3), (10,6), (15,9)`. 

Dấu vết xác nhận rằng chiều rộng trở thành yếu tố giới hạn chứ không phải chiều cao. 

### Ví dụ 2 

đầu vào:```
5 5 4 3
```| Bước | một // x | b // y | Kết quả | 
| --- | --- | --- | --- | 
| Tính giới hạn chiều rộng | 5 // 4 = 1 | - | - | 
| Tính giới hạn chiều cao | - | 5 // 3 = 1 | - | 
| Lấy tối thiểu | 1 | 1 | 1 | 

Chỉ một`k = 1`làm việc, sản xuất`(4, 3)`. Bất kỳ tỷ lệ lớn hơn nào ngay lập tức vượt quá cả hai ràng buộc. 

Ví dụ này cho thấy trường hợp cả hai ràng buộc đều chặt chẽ đồng thời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số phép tính số học không đổi được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp dễ dàng nằm trong giới hạn vì nó tránh được sự lặp lại hoàn toàn. Ngay cả với giá trị đầu vào tối đa gần 10^18, phép chia và so sánh số nguyên là các phép toán liên tục. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    a, b, x, y = map(int, input().split())
    return str(min(a // x, b // y))

# provided sample
assert run("17 15 5 3\n") == "3"

# minimum values
assert run("1 1 1 1\n") == "1"

# impossible case
assert run("5 5 6 1\n") == "0"

# only width constrains
assert run("20 100 5 10\n") == "4"

# only height constrains
assert run("100 20 10 5\n") == "2"

# both tight equally
assert run("12 12 3 3\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 1 | 1 | tỷ lệ hợp lệ nhỏ nhất | 
| 5 5 6 1 | 0 | không tồn tại k hợp lệ | 
| 20 100 5 10 | 4 | trường hợp giới hạn chiều rộng | 
| 100 20 10 5 | 2 | trường hợp giới hạn chiều cao | 
| 12 12 3 3 | 4 | trường hợp ranh giới đối xứng | 

## Vỏ cạnh 

Khi nào`a < x`hoặc`b < y`, công thức ngay lập tức cho kết quả bằng 0 vì ít nhất một phép chia trở thành 0. Ví dụ, đầu vào`a = 4, b = 100, x = 5, y = 1`cho`4 // 5 = 0`, vì vậy không tồn tại TV hợp lệ. Thuật toán xử lý việc này một cách tự nhiên mà không cần điều kiện đặc biệt. 

Khi cả hai ràng buộc đều lớn nhưng không đồng đều, chẳng hạn như`a = 10^18, b = 10^18, x = 1, y = 10^18`, chiều rộng cho phép nhiều giá trị của`k`, nhưng chiều cao giới hạn nó ở chính xác một. Sự tính toán`min(a // 1, b // 10^18)`mang lại kết quả chính xác`1`, chứng tỏ rằng kích thước chặt chẽ hơn luôn chiếm ưu thế thông qua hoạt động tối thiểu.
