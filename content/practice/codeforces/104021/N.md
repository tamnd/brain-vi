---
title: "CF 104021N - Chuỗi Fibonacci"
description: "Nhiệm vụ cực kỳ trực tiếp: chúng tôi được yêu cầu tạo phần đầu của một chuỗi số nguyên nổi tiếng được xác định hoàn toàn bằng phép lặp. Chuỗi bắt đầu bằng hai hạt giống cố định, cả hai đều bằng một và mọi giá trị sau đó thu được bằng cách tính tổng hai giá trị trước đó."
date: "2026-07-02T04:38:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104021
codeforces_index: "N"
codeforces_contest_name: "The 2019 ICPC Asia Yinchuan Regional Contest"
rating: 0
weight: 104021
solve_time_s: 33
verified: true
draft: false
---

[CF 104021N - Chuỗi Fibonacci](https://codeforces.com/problemset/problem/104021/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ cực kỳ trực tiếp: chúng tôi được yêu cầu tạo phần đầu của một chuỗi số nguyên nổi tiếng được xác định hoàn toàn bằng phép lặp. Chuỗi bắt đầu bằng hai hạt giống cố định, cả hai đều bằng một và mọi giá trị sau đó thu được bằng cách tính tổng hai giá trị trước đó. Yêu cầu đầu ra không phải là tham số, không có đầu vào để xử lý và chúng tôi luôn tạo ra chính xác năm số hạng đầu tiên. 

Mặc dù các ràng buộc không được nêu rõ ràng, cấu trúc của vấn đề cho thấy rõ rằng hiệu suất là không liên quan. Chúng tôi chỉ thực hiện một lượng số học không đổi, do đó, mọi triển khai hợp lý đều chạy trong thời gian không đổi và bộ nhớ không đổi. Ngay cả một cách tiếp cận cực kỳ kém hiệu quả vẫn có thể chấp nhận được vì độ dài chuỗi là cố định. 

Không có trường hợp biên nào có ý nghĩa theo nghĩa lập trình cạnh tranh thông thường, nhưng có một ràng buộc định dạng tinh vi có thể phá vỡ giải pháp: đầu ra phải chứa chính xác năm số nguyên cách nhau bởi một khoảng trắng, không có dấu cách hoặc ký hiệu phụ. Việc triển khai bất cẩn làm in một khoảng trắng sau mỗi số, kể cả số cuối cùng, có thể tạo ra kết quả sai mặc dù tính toán các giá trị đúng. Ví dụ như in`"1 1 2 3 5 "`có dấu cách ở cuối là không hợp lệ, mặc dù bản thân các số đó vẫn đúng. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ sẽ là mô phỏng định nghĩa từng bước bằng cách sử dụng một mảng và tính toán liên tục từng giá trị tiếp theo từ hai giá trị trước đó. Cách tiếp cận này phản ánh chính xác định nghĩa toán học và đúng về mặt xây dựng. Nó sẽ xây dựng một danh sách`F`Ở đâu`F[0] = 1`,`F[1] = 1`và với mỗi chỉ mục tiếp theo, nó sẽ thêm vào`F[i-1] + F[i-2]`. 

Ngay cả việc mô phỏng trực tiếp này cũng quá mức cần thiết vì chúng ta chỉ cần năm giá trị. Tổng số phép tính số học là không đổi, do đó thời gian chạy không phụ thuộc vào bất kỳ kích thước đầu vào nào. Không có lợi ích gì trong việc tối ưu hóa thêm vì chúng tôi đã ở mức độ phức tạp tối thiểu có thể. 

Quan sát quan trọng là định nghĩa Fibonacci vốn có tính lặp lại và cục bộ. Mỗi thuật ngữ chỉ phụ thuộc vào hai thuật ngữ trước đó, vì vậy chúng ta không bao giờ cần lưu trữ nhiều hơn hai giá trị đó bất kỳ lúc nào. Điều này cho phép chúng tôi tính toán trình tự theo kiểu phát trực tuyến mà không cần phân bổ mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (xây dựng mảng) | O(5) | O(5) | Đã chấp nhận | 
| Tối ưu (biến cuộn) | O(5) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán chuỗi lặp đi lặp lại trong khi chỉ giữ lại hai giá trị cuối cùng. 

1. Khởi tạo hai biến đại diện cho hai số Fibonacci đầu tiên, cả hai đều được đặt thành 1. Những biến này đóng vai trò là trường hợp cơ sở của phép truy toán. 
2. Xuất giá trị đầu tiên ngay lập tức vì nó được cố định theo định nghĩa. 
3. Xuất ra giá trị thứ hai giống với giá trị đầu tiên. 
4. Lặp lại tính toán số hạng tiếp theo bằng tổng của hai giá trị được lưu trữ trước đó. Sau khi tính toán một giá trị mới, hãy dịch chuyển cặp đã lưu về phía trước để chúng luôn biểu thị hai số hạng cuối cùng trong chuỗi. 
5. Tiếp tục quá trình này cho đến khi tạo được tổng cộng năm giá trị. 

Ý tưởng chính trong bước 4 và 5 là chúng ta không bao giờ tính toán lại từ đầu. Mỗi giá trị mới sẽ mở rộng chuỗi thêm một bước chỉ sử dụng các kết quả đã được tính toán, phản ánh trực tiếp phép truy toán toán học. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong quá trình, hai biến được lưu trữ biểu thị các số Fibonacci liên tiếp. Khi tính tổng của chúng, chúng ta thu được số Fibonacci tiếp theo theo định nghĩa. Cập nhật cặp bằng cách dịch chuyển về phía trước sẽ bảo toàn tính bất biến mà chúng luôn tương ứng với hai số hạng mới nhất trong dãy. Vì phép truy hồi xác định duy nhất mỗi số hạng từ hai số hạng trước đó nên việc duy trì tính bất biến này đảm bảo mọi giá trị được tạo ra đều chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# first five Fibonacci numbers
a, b = 1, 1
out = [a, b]

for _ in range(3):
    a, b = b, a + b
    out.append(b)

sys.stdout.write(" ".join(map(str, out)))
```Giải pháp khởi tạo chuỗi với hai giá trị cơ bản rồi lặp lại tạo ba giá trị tiếp theo. Vòng lặp chạy đúng ba lần vì chúng ta đã có hai số và cần tổng cộng là năm số. 

Bước cập nhật`a, b = b, a + b`là cốt lõi của việc thực hiện. Nó đồng thời dịch chuyển cửa sổ về phía trước và tính số Fibonacci tiếp theo mà không cần các biến tạm thời. Thứ tự này rất quan trọng vì`b`phải được cập nhật dựa trên cái cũ`a`Và`b`trước khi ghi đè một trong hai giá trị. 

Đầu ra cuối cùng sử dụng`" ".join(...)`để đảm bảo khoảng cách chính xác không có khoảng trắng ở cuối, đây là lỗi định dạng phổ biến nhất trong các vấn đề như vậy. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi tính toán bắt đầu từ trạng thái ban đầu. 

### Dấu vết ví dụ 

Trạng thái ban đầu: 

| Bước | một | b | Đầu ra | 
| --- | --- | --- | --- | 
| Ban đầu | 1 | 1 | 1, 1 | 

Sau lần lặp đầu tiên: 

| Bước | một | b | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | 1 | 2 | 1, 1, 2 | 

Sau lần lặp thứ hai: 

| Bước | một | b | Đầu ra | 
| --- | --- | --- | --- | 
| 2 | 2 | 3 | 1, 1, 2, 3 | 

Sau lần lặp thứ ba: 

| Bước | một | b | Đầu ra | 
| --- | --- | --- | --- | 
| 3 | 3 | 5 | 1, 1, 2, 3, 5 | 

Điều này xác nhận rằng mỗi lần lặp lại sẽ nâng cao phép truy toán Fibonacci lên một bước một cách chính xác trong khi vẫn duy trì trạng thái cuộn. 

### Ví dụ thứ hai (biến thể khái niệm) 

Ngay cả khi chúng ta bắt đầu từ cùng một hạt giống, cấu trúc vẫn đảm bảo tính quyết định: 

| Bước | một | b | Đầu ra | 
| --- | --- | --- | --- | 
| Ban đầu | 1 | 1 | 1, 1 | 
| 1 | 1 | 2 | 1, 1, 2 | 
| 2 | 2 | 3 | 1, 1, 2, 3 | 
| 3 | 3 | 5 | 1, 1, 2, 3, 5 | 

Điều này củng cố rằng không có sự phụ thuộc vào phân nhánh hoặc đầu vào; trình tự được xác định đầy đủ bởi định nghĩa của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chúng tôi thực hiện một số phép tính số học cố định độc lập với đầu vào | 
| Không gian | O(1) | Chỉ một số lượng biến không đổi được lưu trữ | 

Kích thước vấn đề được cố định ở năm đầu ra, do đó, giải pháp là không đáng kể trong bất kỳ giới hạn thời gian và bộ nhớ nào có thể hình dung được. Ngay cả trong môi trường nghiêm ngặt, việc tính toán này diễn ra tức thời một cách hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    a, b = 1, 1
    out = [a, b]
    for _ in range(3):
        a, b = b, a + b
        out.append(b)
    return " ".join(map(str, out))

# provided sample
assert run("") == "1 1 2 3 5"

# minimal case (same problem, no input influence)
assert run("") == "1 1 2 3 5", "deterministic output"

# repeated check consistency
assert run("") == "1 1 2 3 5", "idempotent behavior"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trống | 1 1 2 3 5 | độ đúng cơ sở | 
| trống | 1 1 2 3 5 | thuyết định mệnh | 
| trống | 1 1 2 3 5 | không phụ thuộc vào nhà nước | 

## Vỏ cạnh 

Trường hợp cạnh có ý nghĩa duy nhất là định dạng đầu ra, vì bản thân chuỗi đã được cố định. Mọi tính toán đúng luôn mang lại kết quả`1 1 2 3 5`, do đó lỗi đến từ việc in ấn chứ không phải do logic. 

Ví dụ: việc triển khai có lỗi có thể gây ra: 

đầu vào:```
(no input)
```Đầu ra không chính xác:```
1 1 2 3 5
```Đầu ra đúng:```
1 1 2 3 5
```Bản thân thuật toán không bao giờ tạo ra dấu cách; cấu trúc đầu ra dựa trên phép nối đảm bảo rằng khoảng cách chỉ được chèn giữa các phần tử. Điều này làm cho ràng buộc định dạng được tự động thỏa mãn mà không cần xử lý trường hợp đặc biệt cho phần tử cuối cùng.
