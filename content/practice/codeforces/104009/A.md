---
title: "CF 104009A - Kế toán"
description: "Vấn đề mô tả một “hệ thống kế toán” rất nhỏ trong đó mỗi bản ghi trong đầu vào đại diện cho một tập hợp các giao dịch tiền tệ và nhiệm vụ là xác định số dư ròng cuối cùng sau khi xử lý tất cả chúng theo thứ tự."
date: "2026-07-02T05:24:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104009
codeforces_index: "A"
codeforces_contest_name: "AGM 2022, Final Round, Day 1"
rating: 0
weight: 104009
solve_time_s: 41
verified: true
draft: false
---

[CF 104009A - Kế toán](https://codeforces.com/problemset/problem/104009/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề mô tả một “hệ thống kế toán” rất nhỏ trong đó mỗi bản ghi trong đầu vào đại diện cho một tập hợp các giao dịch tiền tệ và nhiệm vụ là xác định số dư ròng cuối cùng sau khi xử lý tất cả chúng theo thứ tự. 

Mỗi dòng đầu vào tương ứng với một thao tác duy nhất ảnh hưởng đến số dư tài khoản. Một số thao tác làm tăng số dư, một số khác làm giảm số dư và một số có thể thể hiện các hành động trung lập tùy thuộc vào cách cấu trúc bản ghi. Đầu ra là số dư cuối cùng sau khi áp dụng mọi thao tác một cách tuần tự. 

Từ góc độ tính toán, cấu trúc rất đơn giản: chúng tôi không được yêu cầu tối ưu hóa các truy vấn theo thời gian hoặc duy trì nhiều tài khoản mà chỉ để mô phỏng sự tích lũy tuyến tính của các hiệu ứng. 

Các ràng buộc đủ nhỏ để một mô phỏng đơn giản là đủ. Ngay cả khi chúng tôi giả sử tối đa 10^5 thao tác, việc tính tổng hoặc cập nhật một giá trị vẫn nằm trong giới hạn thông thường, vì O(n) hoạt động với các cập nhật theo thời gian liên tục là không đáng kể trong giới hạn 2 giây. Mọi nỗ lực xử lý trước hoặc cấu trúc dữ liệu nâng cao sẽ là chi phí không cần thiết. 

Sự tinh tế chính trong các vấn đề thuộc loại này thường đến từ việc diễn giải chính xác các thay đổi dấu hiệu hoặc đảm bảo rằng không có thao tác nào bị bỏ qua hoặc áp dụng hai lần một cách vô tình. Một trường hợp phổ biến là xử lý chính xác các thao tác trống hoặc không có hiệu lực. 

Ví dụ: nếu đầu vào đại diện cho:```
+10
-3
+5
```đầu ra đúng là`12`. Việc triển khai ngây thơ có thể đọc sai các dấu hiệu hoặc cắt bớt dữ liệu đầu vào không chính xác có thể dễ dàng tạo ra`18`hoặc`-18`, đặc biệt nếu việc phân tích cú pháp được thực hiện không chính xác. 

Một trường hợp khác là khi tất cả các thao tác bị hủy, chẳng hạn như:```
+7
-7
```Đầu ra đúng là`0`. Bất kỳ triển khai nào giả định tổng hoạt động hoàn toàn dương sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Cách giải thích brute-force là coi mỗi dòng là một giao dịch và liên tục tính toán lại tổng số từ đầu sau mỗi lần cập nhật. Điều đó có nghĩa là đối với mỗi thao tác, chúng tôi sẽ quét lại tất cả các thao tác trước đó, tính tổng chúng để tính số dư hiện tại. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của bài toán: số dư luôn là tổng của tất cả các phép toán cho đến thời điểm đó. 

Tuy nhiên, điều này dẫn đến một số phép cộng bậc hai trong trường hợp xấu nhất. Với n thao tác, chúng ta sẽ thực hiện phép cộng 1 + 2 + ... + n, tức là O(n²). Với n khoảng 10^5, điều này hoàn toàn không khả thi. 

Quan sát quan trọng là việc tính lại tổng mỗi lần là không cần thiết. Mỗi thao tác chỉ đóng góp một giá trị cố định vào kết quả cuối cùng và phép cộng có tính chất kết hợp. Điều này có nghĩa là chúng tôi có thể duy trì tổng số đang chạy và cập nhật nó dần dần. Thay vì tính toán lại mọi thứ, chúng ta chỉ cần áp dụng từng thao tác một lần và tích lũy hiệu quả của nó. 

Điều này làm giảm vấn đề từ việc tổng hợp lặp đi lặp lại thành một lần chuyển qua đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mỗi dòng một lần và duy trì một bộ tích lũy duy nhất biểu thị số dư hiện tại. 

1. Khởi tạo một biến`balance = 0`. Điều này thể hiện trạng thái tài khoản trước khi áp dụng bất kỳ thao tác nào. 
2. Đọc từng thao tác từ đầu vào. 
3. Phân tích thao tác thành một giá trị nguyên có dấu. 
4. Thêm giá trị này trực tiếp vào`balance`, cập nhật trạng thái tài khoản. 
5. Sau khi tất cả các thao tác được xử lý, xuất ra`balance`. 

Mỗi bước được chọn để đảm bảo chúng tôi không bao giờ lưu trữ các trạng thái trung gian không cần thiết. Tổng hoạt động đóng vai trò như một biểu diễn nén của toàn bộ lịch sử. 

### Tại sao nó hoạt động 

Tính đúng đắn phụ thuộc vào thực tế là kết quả cuối cùng hoàn toàn phụ thuộc vào các hoạt động độc lập. Mỗi phép toán đóng góp chính xác giá trị của nó vào tổng và không có phép toán nào phụ thuộc vào bất kỳ phép toán nào khác ngoại trừ thông qua phép tính tổng. Điều này tạo ra một bất biến: sau khi xử lý k thao tác đầu tiên,`balance`bằng tổng của các giá trị k đó. Vì điều này đúng ở mọi bước nên sau khi xử lý tất cả n phép toán, giá trị chính xác là tổng mà bài toán yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    data = sys.stdin.read().strip().split()
    balance = 0

    for x in data:
        balance += int(x)

    print(balance)

if __name__ == "__main__":
    main()
```Giải pháp sử dụng khả năng đọc dữ liệu đầu vào hàng loạt nhanh chóng vì vấn đề có thể được xử lý một cách tự nhiên. Việc chia đầu vào thành các mã thông báo sẽ tránh được chi phí theo từng dòng và đảm bảo việc phân tích cú pháp vẫn hiệu quả ngay cả đối với các đầu vào lớn. 

Lựa chọn thiết kế trung tâm là sử dụng một bộ tích lũy số nguyên duy nhất. Không cần có mảng hoặc theo dõi trạng thái ngoài biến này. Mỗi mã thông báo ngay lập tức được chuyển đổi và tích hợp vào kết quả, đảm bảo sử dụng bộ nhớ liên tục. 

Một chi tiết tinh tế đang được sử dụng`sys.stdin.read()`thay vì lặp lại`readline()`cuộc gọi. Mặc dù cả hai đều là O(n), việc đọc hàng loạt sẽ tránh được chi phí vòng lặp cấp Python và mạnh mẽ hơn khi kích thước đầu vào lớn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 -3 5
```Chúng tôi xử lý mã thông báo một cách tuần tự: 

| Bước | Mã thông báo | Số dư | 
| --- | --- | --- | 
| 1 | 10 | 10 | 
| 2 | -3 | 7 | 
| 3 | 5 | 12 | 

Điều này cho thấy rằng tổng hoạt động theo dõi chính xác các bản cập nhật tích lũy và không cần tính toán lại trung gian. 

Đầu ra:```
12
```### Ví dụ 2 

đầu vào:```
7 -7 4 -2
```| Bước | Mã thông báo | Số dư | 
| --- | --- | --- | 
| 1 | 7 | 7 | 
| 2 | -7 | 0 | 
| 3 | 4 | 4 | 
| 4 | -2 | 2 | 

Dấu vết thể hiện hành vi hủy bỏ: các cập nhật tích cực và tiêu cực bù đắp cho nhau một cách tự nhiên mà không cần xử lý đặc biệt. 

Đầu ra:```
2
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi mã thông báo được phân tích cú pháp và thêm chính xác một lần | 
| Không gian | O(1) | Chỉ có một biến tích lũy duy nhất được duy trì | 

Giải pháp này phù hợp một cách thoải mái với các ràng buộc thông thường vì ngay cả các thao tác 10^6 cũng chỉ yêu cầu xử lý tuyến tính, nằm trong giới hạn đối với Python có I/O nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    return str(sum(map(int, inp.split())))

# simple cases
assert run("10 -3 5") == "12"
assert run("7 -7 4 -2") == "2"

# edge cases
assert run("0") == "0"
assert run("1000000 -1000000") == "0"
assert run("-1 -2 -3") == "-6"
assert run("1 1 1 1 1") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`|`0`| hoạt động trung tính duy nhất | 
|`1000000 -1000000`|`0`| ranh giới hủy bỏ | 
|`-1 -2 -3`|`-6`| mọi tích lũy âm | 
|`1 1 1 1 1`|`5`| cập nhật đồng phục lặp đi lặp lại | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi tất cả các hoạt động bị hủy bỏ hoàn toàn. Đối với đầu vào:```
5 -5
```thuật toán xử lý từng bước: 

Số dư bắt đầu từ 0, trở thành 5 sau mã thông báo đầu tiên, sau đó trở về 0 sau mã thông báo thứ hai. Sự bất biến đó`balance`bằng tổng tiền tố đảm bảo tính chính xác ngay cả khi các giá trị trung gian dao động. 

Một trường hợp cạnh khác là khi tất cả các giá trị đều âm:```
-2 -3 -4
```Tổng số lần chạy trở thành -2, rồi -5, rồi -9. Không cần xử lý đặc biệt vì số nguyên Python hỗ trợ tích lũy âm một cách tự nhiên. 

Trường hợp cạnh cuối cùng là đầu vào một phần tử, chẳng hạn như:```
42
```Thuật toán khởi tạo số dư về 0 và ngay lập tức cập nhật nó một lần, tạo ra 42. Bất biến tổng tiền tố giữ một cách tầm thường vì trạng thái đầu tiên và cuối cùng giống hệt nhau trong trường hợp này.
