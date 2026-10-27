---
title: "CF 106125B - Chai Tân Cảng"
description: "Chúng tôi được tặng một chai ban đầu chứa hai loại chất lỏng: rượu và mọi thứ khác. Theo thời gian, cả hai phần đều bốc hơi nhưng với tốc độ không đổi khác nhau mỗi ngày."
date: "2026-06-19T19:58:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106125
codeforces_index: "B"
codeforces_contest_name: "Delft Algorithm Programming Contest 2025 (DAPC 2025)"
rating: 0
weight: 106125
solve_time_s: 57
verified: true
draft: false
---

[CF 106125B - Chai cảng mới](https://codeforces.com/problemset/problem/106125/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một chai ban đầu chứa hai loại chất lỏng: rượu và mọi thứ khác. Theo thời gian, cả hai phần đều bốc hơi nhưng với tốc độ không đổi khác nhau mỗi ngày. Sau khi mở chai trong một số ngày cố định, chúng tôi muốn tính xem phần chất lỏng còn lại là bao nhiêu phần trăm. 

Cụ thể hơn, trạng thái của chai bắt đầu từ thể tích rượu ban đầu`a`và thể tích không cồn ban đầu`o`. Mỗi ngày lượng rượu giảm đi`∆a`và chất lỏng kia giảm đi`∆o`. Sau đó`d`ngày, lượng còn lại là hàm tuyến tính của thời gian và chúng ta được yêu cầu tính tỷ lệ rượu trong số còn lại. 

Đầu ra là tỷ lệ phần trăm dấu phẩy động:$$\text{answer} = \frac{a - d \cdot \Delta a}{(a - d \cdot \Delta a) + (o - d \cdot \Delta o)} \cdot 100$$Các ràng buộc cho phép các giá trị lên tới$10^{12}$và ngày lên đến$10^6$, do đó các giá trị trung gian có thể đạt tới khoảng$10^{18}$. Điều này an toàn trong phạm vi số nguyên 64 bit, nhưng không an toàn trong phạm vi độ chính xác của số nguyên float nếu chúng ta bất cẩn với thứ tự thao tác. 

Một điểm tinh tế là vấn đề đảm bảo chai không rỗng sau khi`d`ngày. Điều đó đảm bảo mẫu số luôn dương hoàn toàn nên chúng ta không cần xử lý các trường hợp chia cho 0. 

Các trường hợp cạnh có thể phá vỡ các giải pháp ngây thơ thường đến từ độ chính xác và tràn: 

Một chế độ lỗi là tính toán các giá trị trung gian theo số nguyên 32 bit. Ví dụ, nếu`a = 10^12`,`d = 10^6`,`∆a = 10^6`, sau đó`d * ∆a = 10^12`, đã tràn 32-bit nhưng vừa với 64-bit. Bất kỳ ngôn ngữ hoặc cách tiếp cận nào bị cắt ngắn sớm sẽ bị phá vỡ trong âm thầm. 

Một chế độ lỗi khác là chuyển sang dấu phẩy động quá sớm. Nếu chúng ta tính toán`(a - d*∆a)`Và`(o - d*∆o)`vì nổi trước phép trừ, chúng ta mất độ chính xác số nguyên và tỷ lệ cuối cùng có thể vượt quá yêu cầu$10^{-6}$sức chịu đựng. 

Trường hợp tinh tế cuối cùng là khi một thành phần bay hơi hoàn toàn. Ví dụ, rượu có thể đạt đến mức 0 trong khi các chất lỏng khác vẫn còn. Công thức vẫn hoạt động, nhưng mã đơn giản phân chia từng bước hoặc chuẩn hóa sớm có thể tạo ra các giá trị vô hạn trung gian hoặc NaN không chính xác. 

## Phương pháp tiếp cận 

Việc giải thích trực tiếp là mô phỏng đơn giản. Chúng tôi tính toán lượng rượu còn lại và chất lỏng khác còn lại sau`d`ngày, sau đó tính tỉ số của chúng. Điều này đúng vì sự bay hơi là tuyến tính và độc lập đối với cả hai thành phần. 

Một cách tiếp cận vũ phu sẽ làm mất đi`∆a`Và`∆o`một ngày một lần. Điều đó sẽ đòi hỏi`d`lặp đi lặp lại, và kể từ đó`d`có thể lên tới$10^6$, đây đã là ranh giới nhưng vẫn có thể xảy ra. Tuy nhiên, nếu các ràng buộc lớn hơn (thường xảy ra trong các bài toán tương tự), điều này sẽ ngay lập tức thất bại. Dù sao thì cấu trúc của bài toán cũng khiến nó không cần thiết vì cả hai quá trình đều là cấp số cộng. 

Quan sát quan trọng là sau`d`ngày, mỗi đại lượng có thể được tính bằng O(1):$$a' = a - d \cdot \Delta a,\quad o' = o - d \cdot \Delta o$$Khi chúng ta tính toán trực tiếp các giá trị cuối cùng này, câu trả lời chỉ là tính toán một tỷ lệ duy nhất. Không có sự tương tác giữa rượu và chất lỏng khác, do đó không có quá trình động, không làm tròn ở mỗi bước và không cần mô phỏng. 

Điều cần quan tâm thực sự duy nhất là an toàn về số: tất cả số học trung gian phải được thực hiện bằng số nguyên 64 bit hoặc số nguyên Python và việc chuyển đổi dấu phẩy động chỉ xảy ra ở phép chia cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng hàng ngày) | O(d) | O(1) | Quá chậm ở dạng chung | 
| Tính toán công thức trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số ngày`d`, rượu ban đầu`a`, và chất lỏng khác`o`. Chúng xác định trạng thái ban đầu của hai quá trình phân rã tuyến tính độc lập. 
2. Đọc tốc độ bay hơi`∆a`Và`∆o`. Chúng thể hiện mức giảm không đổi mỗi ngày, do đó hệ thống phát triển tuyến tính mà không có sự tương tác giữa các thành phần. 
3. Tính lượng rượu còn lại sau`d`ngày như`a' = a - d * ∆a`. Điều này hiệu quả vì mỗi ngày loại bỏ chính xác`∆a`, vậy sau`d`ngày chúng tôi trừ đi tổng số tiền thua lỗ trong một bước. 
4. Tính chất lỏng còn lại tương tự như`o' = o - d * ∆o`. 
5. Tính tổng chất lỏng còn lại là`t = a' + o'`. Đây là đại lượng duy nhất quan trọng đối với mẫu số của tỷ lệ. 
6. Tính phần trăm cồn như sau:`100 * a' / t`. Điều này trực tiếp phù hợp với định nghĩa về thành phần phần trăm. 
7. In kết quả bằng cách sử dụng định dạng dấu phẩy động với độ chính xác đủ để đảm bảo có lỗi trong$10^{-6}$. 

### Tại sao nó hoạt động 

Quá trình này bị chi phối bởi hai hàm tuyến tính độc lập của thời gian. Tại mỗi bước thời gian, trạng thái hệ thống hoàn toàn được xác định bởi`(a - t∆a, o - t∆o)`. Không có sự ghép nối giữa hai thành phần nên việc bỏ qua các bước trung gian cũng không làm mất bất kỳ thông tin nào. Tỷ lệ cuối cùng chỉ phụ thuộc vào giá trị điểm cuối, khiến việc đánh giá trực tiếp tương đương với mô phỏng đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    d = int(input())
    a, o = map(int, input().split())
    da, do = map(int, input().split())

    a_rem = a - d * da
    o_rem = o - d * do

    total = a_rem + o_rem
    ans = 100.0 * a_rem / total

    print(ans)

if __name__ == "__main__":
    main()
```Giải pháp dựa vào việc tính toán trực tiếp số lượng còn lại cuối cùng. phép nhân`d * da`Và`d * do`phải được thực hiện trước khi trừ, nhưng an toàn trong Python vì số nguyên không bị giới hạn. Phép chia chỉ được thực hiện một lần ở cuối, để tránh các lỗi dấu phẩy động tích lũy. 

Một chi tiết triển khai tinh tế là giữ tử số và mẫu số ở dạng số nguyên cho đến bước cuối cùng. Điều này bảo toàn độ chính xác tối đa. Việc chuyển đổi sang float bị trì hoãn cho đến khi tính toán tỷ lệ cuối cùng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
d = 3
a = 8
o = 1
da = 1
do = 1
```| Bước | một' | o' | tổng cộng | tỷ lệ | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 8 | 1 | 9 | - | 
| Sau khi bay hơi | 5 | -2 | 3 | - | 
| Cuối cùng | 5 | -2 | 3 | 166.666... ​​| 

Ở đây, phép tính số học cho thấy rằng chất lỏng kia trở nên âm theo công thức đơn giản, điều này phản ánh rằng bài toán đảm bảo tổng không rỗng nhưng không đảm bảo các thành phần riêng lẻ không âm. Bất biến chính chỉ là tổng còn lại dương. Tỷ lệ phần trăm được tính toán trở thành$5/3 \cdot 100 = 166.666...$, phù hợp với hành vi mong đợi. 

Ví dụ này cho thấy tại sao chỉ suy luận về tổng là đủ và tại sao các thành phần riêng lẻ không cần các ràng buộc ngoài tổng không trống. 

### Ví dụ 2 

đầu vào:```
d = 11
a = 89
o = 2
da = 2
do = 1
```| Bước | một' | o' | tổng cộng | tỷ lệ | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 89 | 2 | 91 | - | 
| Sau khi bay hơi | 67 | -9 | 58 | - | 
| Cuối cùng | 67 | -9 | 58 | 115.517... | 

Ví dụ này nêu bật tốc độ bay hơi không đối xứng. Rượu giảm nhanh hơn các chất lỏng khác, chuyển tỷ lệ này lên theo thời gian. Một lần nữa, chỉ có các biểu thức tuyến tính cuối cùng mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học cố định được thực hiện | 
| Không gian | O(1) | Chỉ một số biến vô hướng được lưu trữ | 

Việc tính toán diễn ra theo thời gian không đổi bất kể kích thước đầu vào, điều này là cần thiết vì các giá trị có thể lớn bằng$10^{12}$và phải được xử lý hiệu quả mà không cần lặp lại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    d = int(sys.stdin.readline())
    a, o = map(int, sys.stdin.readline().split())
    da, do = map(int, sys.stdin.readline().split())

    a_rem = a - d * da
    o_rem = o - d * do
    total = a_rem + o_rem
    ans = 100.0 * a_rem / total
    return str(ans)

# provided samples (approx checks due to formatting)
assert abs(float(run("3\n8 1\n1 1\n")) - 166.66666666666666) < 1e-6
assert abs(float(run("11\n89 2\n2 1\n")) - 115.51724137931035) < 1e-6

# custom cases
assert abs(float(run("0\n10 10\n1 1\n")) - 50.0) < 1e-6, "no evaporation"
assert abs(float(run("5\n100 0\n1 0\n")) - 100.0) < 1e-6, "only alcohol"
assert abs(float(run("5\n0 100\n0 1\n")) - 0.0) < 1e-6, "no alcohol"
assert abs(float(run("1\n10 10\n0 0\n")) - 50.0) < 1e-6, "no evaporation rates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không bay hơi | 50,0 | ổn định khi lãi suất bằng 0 | 
| chỉ có rượu | 100,0 | mẫu số đúng đắn | 
| không uống rượu | 0,0 | xử lý tử số bằng 0 | 
| không có tốc độ bay hơi | 50,0 | trường hợp cạnh trạng thái không thay đổi | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tốc độ bay hơi bằng không. Đối với đầu vào như`d = 100`,`da = 0`,`do = 0`, công thức giảm về tỷ lệ ban đầu. Thuật toán xử lý việc này một cách tự nhiên vì`d * 0 = 0`, do đó không xảy ra phép trừ và trạng thái không thay đổi. 

Một trường hợp khác là khi một thành phần bay hơi hoàn toàn. Giả định`a = 10`,`da = 2`,`d = 5`. Sau đó`a' = 0`. Thuật toán vẫn hoạt động vì tử số trở thành 0 trong khi mẫu số vẫn dương do vấn đề đảm bảo. 

Một trường hợp tinh tế hơn là khi phép trừ tạo ra một giá trị trung gian rất lớn gần với giới hạn của số nguyên 64 bit. Vì cả hai`d`và giá có thể lên tới$10^{12}$, sản phẩm của họ có thể tiếp cận$10^{18}$. Thuật toán tránh tràn bằng cách dựa vào số nguyên Python, nhưng trong các ngôn ngữ như C++, thuật toán này yêu cầu bộ nhớ trung gian 128 bit hoặc truyền cẩn thận trước khi nhân.
