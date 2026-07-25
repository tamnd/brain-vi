---
title: "CF 102861G - Game Show!"
description: "Trò chơi bắt đầu với việc Ricardo có số dư 100 sbec. Các hộp được sắp xếp theo một thứ tự cố định và anh ta có thể mở một số tiền tố của chúng. Sau khi mở hộp, giá trị ẩn của nó sẽ được cộng vào số dư hiện tại."
date: "2026-07-25T14:04:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102861
codeforces_index: "G"
codeforces_contest_name: "2020-2021 ACM-ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 102861
solve_time_s: 35
verified: true
draft: false
---

[CF 102861G - Game Show!](https://codeforces.com/problemset/problem/102861/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi bắt đầu với việc Ricardo có số dư 100 sbec. Các hộp được sắp xếp theo một thứ tự cố định và anh ta có thể mở một số tiền tố của chúng. Sau khi mở hộp, giá trị ẩn của nó sẽ được cộng vào số dư hiện tại. Bất cứ lúc nào anh ta có thể dừng lại và lấy số tiền hiện tại, kể cả trước khi mở bất kỳ hộp nào. 

Nhiệm vụ là tìm ra điểm cân bằng tối đa có thể đạt được bằng cách chọn điểm dừng tốt nhất. Vì việc mở chính xác k hộp đầu tiên sẽ có số dư là 100 cộng với tổng của k giá trị đó, nên vấn đề tương đương với việc tìm tổng tiền tố tốt nhất, đồng thời cho phép tiền tố trống. 

Đầu vào chứa số lượng hộp theo sau là giá trị bên trong mỗi hộp theo thứ tự. Sản lượng là số dư cuối cùng lớn nhất mà Ricardo có thể đạt được. 

Số lượng hộp nhiều nhất là 100 và mọi giá trị nằm trong khoảng từ -1000 đến 1000. Các giới hạn này đủ nhỏ để ngay cả việc xử lý tuyến tính đơn giản cũng là quá đủ. Cách tiếp cận bậc hai vẫn có thể đạt được đối với giới hạn cụ thể này, nhưng cấu trúc của bài toán đưa ra lời giải trực tiếp một lần. Không cần cấu trúc dữ liệu phức tạp vì mỗi ô chỉ ảnh hưởng đến các lựa chọn trong tương lai thông qua số dư tích lũy hiện tại. 

Trường hợp cạnh chính là một chuỗi trong đó mọi hộp đều âm. Việc thực hiện bất cẩn có thể cho rằng Ricardo phải mở ít nhất một hộp và trả lại tổn thất ít gây thiệt hại nhất. Ví dụ:```
3
-5
-2
-8
```Đầu ra đúng là`100`, vì Ricardo có thể nghỉ việc ngay lập tức. Cách tiếp cận chỉ kiểm tra các tiền tố không trống sẽ trả về không chính xác`95`. 

Một trường hợp đặc biệt khác là khi câu trả lời đúng nhất xuất hiện sau một vài ô phủ định. Ví dụ:```
4
-10
-10
30
-100
```Đầu ra đúng là`110`, bởi vì việc mở ba hộp đầu tiên sẽ mang lại mức tăng ròng là 10. Phương pháp dừng bất cứ khi nào tổng hiện tại trở thành âm sẽ không thành công vì nó sẽ loại bỏ tiền tố mà sau này có lãi. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ mô phỏng mọi quyết định có thể xảy ra. Vì Ricardo có thể dừng lại sau bất kỳ số hộp nào đã mở nên chúng ta có thể thử mọi vị trí dừng có thể, tính toán số dư tại điểm đó và giữ mức tối đa. Có C+1 vị trí có thể dừng, kể cả dừng ngay. Nếu mỗi người tính lại tổng tiền tố từ đầu thì tổng công là O(C2). Với C = 100, điều này có thể chấp nhận được, nhưng nó bỏ qua thực tế là mọi vị trí dừng liên tiếp đều chia sẻ gần như toàn bộ thông tin của nó với vị trí trước đó. 

Quan sát hữu ích là giá trị thay đổi duy nhất khi di chuyển qua các hộp là số dư hiện tại. Sau khi mở hộp tiếp theo, số dư mới chỉ đơn giản là số dư cũ cộng với giá trị hộp đó. Chúng ta có thể quét các hộp một lần, cập nhật số dư hiện tại và ghi nhớ giá trị lớn nhất được thấy cho đến nay. Điều này có tác dụng vì mọi trạng thái cuối cùng có thể đều tương ứng chính xác với một điểm trong quá trình quét này. 

Phương pháp brute-force hoạt động vì mọi lựa chọn dừng có thể đều được kiểm tra, nhưng nó lặp lại các phép tính tiền tố giống nhau nhiều lần. Nhận xét rằng mỗi lựa chọn tiếp theo chỉ khác nhau khi thêm một giá trị mới sẽ giảm vấn đề xuống còn một lần vượt qua. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(C2) | O(1) | Được chấp nhận cho các giới hạn nhất định, nhưng không cần thiết | 
| Tối ưu | O(C) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với số dư hiện tại bằng 100 và câu trả lời bằng 100. Câu trả lời ban đầu thể hiện lựa chọn thoát trước khi mở bất kỳ ô nào. 
2. Đọc từng giá trị trong ô theo thứ tự và cộng vào số dư hiện tại. Số dư hiện tại thể hiện số tiền Ricardo sẽ nhận được nếu anh ấy dừng lại sau khi mở hộp này. 
3. So sánh số dư hiện tại với câu trả lời đúng nhất tìm được cho đến nay và giữ giá trị lớn hơn. Mỗi vị trí dừng được xem xét chính xác một lần trong quá trình quét. 
4. Sau khi tất cả các hộp đã được xử lý, xuất số dư tối đa được lưu trữ. 

Tại sao nó hoạt động: 

Sau khi xử lý hộp i đầu tiên, số dư hiện tại chính xác là số tiền mà Ricardo sẽ có sau khi mở các hộp i đó. Thuật toán lưu trữ số dư tối đa trong số tất cả các điểm dừng từ 0 đến i. Khi hộp tiếp theo được xử lý, điểm dừng mới sẽ được thêm vào nhóm khả năng này và mức tối đa sẽ được cập nhật nếu cần. Vào thời điểm tất cả các ô được xử lý, mọi chiến lược khả thi đều đã được xem xét, vì vậy câu trả lời được lưu trữ là số dư cuối cùng tối ưu. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

def solve():
    c = int(input())
    balance = 100
    answer = 100

    for _ in range(c):
        balance += int(input())
        if balance > answer:
            answer = balance

    print(answer)

if __name__ == "__main__":
    solve()
```Biến`balance`lưu trữ số tiền mà Ricardo sẽ có nếu anh ta tiếp tục mở các hộp đến vị trí hiện tại. Nó bắt đầu từ 100 vì chọn ô số 0 là một chiến lược hợp lệ. 

Biến`answer`lưu trữ sự cân bằng tốt nhất trong số tất cả các điểm dừng được thấy cho đến nay. Việc khởi tạo nó thành 100 sẽ xử lý trường hợp mọi hộp đều có hại và Ricardo không bao giờ nên mở một hộp. 

Vòng lặp xử lý mỗi hộp chính xác một lần. Việc cập nhật diễn ra trước khi so sánh với`answer`, vì việc mở hộp hiện tại sẽ tạo ra một điểm dừng mới có thể có. Số nguyên Python không vượt quá giới hạn đã cho, do đó không cần xử lý đặc biệt. 

Không có lập chỉ mục liên quan, điều này tránh được những sai sót nhỏ. Thuật toán xử lý ô cuối cùng một cách tự nhiên vì nó kiểm tra số dư ngay sau mỗi lần thêm. 

## Ví dụ đã hoạt động 

Đối với ví dụ đầu tiên, sử dụng các giá trị:```
4
-1
-2
-3
-4
```dấu vết là: 

| Bước | Giá trị hộp | Số dư hiện tại | Câu trả lời hay nhất | 
| --- | --- | --- | --- | 
| Bắt đầu | không | 100 | 100 | 
| 1 | -1 | 99 | 100 | 
| 2 | -2 | 97 | 100 | 
| 3 | -3 | 94 | 100 | 
| 4 | -4 | 90 | 100 | 

Dấu vết cho thấy tại sao tiền tố trống lại quan trọng. Mỗi hộp mở ra sẽ làm giảm số dư nên lựa chọn tối ưu là dừng ngay lập tức. 

Đối với ví dụ thứ hai:```
3
-10
-30
-50
```dấu vết là: 

| Bước | Giá trị hộp | Số dư hiện tại | Câu trả lời hay nhất | 
| --- | --- | --- | --- | 
| Bắt đầu | không | 100 | 100 | 
| 1 | -10 | 90 | 100 | 
| 2 | -30 | 60 | 100 | 
| 3 | -50 | 10 | 100 | 

Điều này khẳng định rằng một chuỗi các giá trị âm không buộc Ricardo phải mất tiền vì anh luôn có thể chọn không bắt đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(C) | Mỗi hộp được đọc và xử lý một lần | 
| Không gian | O(1) | Chỉ số dư hiện tại và câu trả lời được lưu trữ | 

Số lượng hộp tối đa chỉ là 100 nên giải pháp tuyến tính này dễ dàng nằm trong giới hạn yêu cầu. Việc sử dụng bộ nhớ liên tục của nó cũng làm cho nó phù hợp với các phiên bản lớn hơn nhiều của cùng một vấn đề. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

# provided samples
assert run("4\n-1\n-2\n-3\n-4\n") == "100\n", "sample 1"
assert run("3\n-10\n-30\n-50\n") == "100\n", "sample 2"

# custom cases
assert run("1\n0\n") == "100\n", "single zero value"
assert run("5\n5\n-20\n30\n-10\n-100\n") == "115\n", "best prefix in the middle"
assert run("3\n1000\n1000\n1000\n") == "3100\n", "maximum positive growth"
assert run("100\n-1000\n" * 100) == "100\n", "maximum size all negative input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một hộp số không | 100 | Thuật toán giữ số dư ban đầu khi không có gì cải thiện | 
| Giá trị dương và âm hỗn hợp | 115 | Điểm dừng tốt nhất có thể xảy ra trước khi kết thúc | 
| Tất cả các giá trị dương | 3100 | Thuật toán tiếp tục qua mọi hộp có lãi | 
| 100 hộp âm bản | 100 | Giải pháp xử lý trường hợp đầu vào lớn nhất và trường hợp tiền tố trống | 

## Vỏ cạnh 

Đối với trường hợp toàn âm:```
3
-5
-2
-8
```thuật toán bắt đầu bằng`balance = 100`Và`answer = 100`. Sau mỗi lần cộng, số dư sẽ là 95, 93 và 85. Không có giá trị nào trong số này cải thiện câu trả lời nên kết quả cuối cùng vẫn là 100. Điều này phù hợp với chiến lược bỏ trước khi mở bất kỳ ô nào. 

Đối với trường hợp tiền tố xấu tạm thời trở thành lựa chọn tốt nhất:```
4
-10
-10
30
-100
```số dư trong quá trình quét là 90, 80, 110 và 10. Thuật toán giữ giá trị nhìn thấy tối đa, do đó, khi nó đạt đến 110 sau hộp thứ ba, giá trị đó sẽ trở thành câu trả lời. Ô phủ định cuối cùng không thể loại bỏ sự thật rằng việc dừng lại sớm hơn đã là một lựa chọn hợp lệ. 

Đối với đầu vào kích thước tối thiểu:```
1
-50
```thuật toán kiểm tra kết quả duy nhất có thể có của hộp mở, 50, so với tùy chọn ban đầu là 100. Câu trả lời cuối cùng là 100, xác nhận rằng không cần phải mở một hộp độc hại. 

Tôi cũng có thể điều chỉnh định dạng này thành định dạng biên tập ngắn hơn theo phong cách Codeforces nếu bạn muốn nội dung nào đó gần giống với nội dung xuất hiện trên trang cuộc thi.
