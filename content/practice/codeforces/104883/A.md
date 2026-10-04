---
title: "CF 104883A - rnm\uff0c\u9000\u94b1\uff01"
description: "Chúng tôi được cung cấp nhật ký theo trình tự thời gian về số dư tài khoản của người chơi trong hệ thống tiền ảo. Mỗi bản ghi thay đổi số dư của mình theo một trong ba cách: họ nhận tiền bằng cách trả tiền thật, tiêu tiền trong trò chơi hoặc làm điều gì đó không liên quan nhưng không ảnh hưởng đến…"
date: "2026-06-28T09:09:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104883
codeforces_index: "A"
codeforces_contest_name: "The 18-th Beihang University Collegiate Programming Contest (BCPC 2023) - Final"
rating: 0
weight: 104883
solve_time_s: 42
verified: true
draft: false
---

[CF 104883A - rnm\uff0c\u9000\u94b1\uff01](https://codeforces.com/problemset/problem/104883/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhật ký theo trình tự thời gian về số dư tài khoản của người chơi trong hệ thống tiền ảo. Mỗi bản ghi thay đổi số dư của mình theo một trong ba cách: họ nhận tiền bằng cách trả tiền thật, tiêu tiền trong trò chơi hoặc làm điều gì đó không liên quan nhưng không ảnh hưởng đến số dư của họ. 

Số lượng chính là số tiền ảo cuối cùng chưa được sử dụng sau khi xử lý tất cả các sự kiện theo thứ tự. Vì tỷ giá hối đoái giữa RMB và đơn vị tiền tệ trong trò chơi là 1-1 nên số tiền còn lại chưa chi tiêu có thể được hoàn trả trực tiếp dưới dạng RMB vào cuối. 

Đầu vào là một dãy số nguyên có dấu. Giá trị dương làm tăng số dư, giá trị âm làm giảm số dư và số 0 giữ nguyên số dư. Chi tiêu được đảm bảo không bao giờ vượt quá số dư hiện tại bất kỳ lúc nào, do đó số dư không bao giờ trở nên vô hiệu trong quá trình xử lý. 

Từ góc độ ràng buộc, số lượng sự kiện tối đa là 1000 và mỗi giá trị có thể lớn tới 10^9 về độ lớn. Điều này ngay lập tức loại trừ mọi nhu cầu về cấu trúc dữ liệu phức tạp hoặc tối ưu hóa ngoài việc tổng hợp một lượt. Ngay cả mô phỏng O(n^2) cũng có thể vượt qua một cách thoải mái, nhưng cấu trúc cho thấy rằng quét tuyến tính vừa đủ vừa tự nhiên. 

Một sai lầm phổ biến là suy nghĩ quá kỹ về điều kiện hoàn tiền và cố gắng tách biệt loại tiền "đã chi nhưng có thể hoàn lại" và loại tiền "đã sử dụng". Ví dụ: nếu nhật ký là`[+328, -328, +488]`, một cách giải thích ngây thơ có thể cho rằng khoản tiền gửi đầu tiên góp phần hoàn lại tiền một cách không chính xác vì nó đã từng là số dương, nhưng việc chi tiêu sẽ hủy bỏ nó hoàn toàn. Một trường hợp khó phát hiện khác là khi không có mục nào xuất hiện; chúng nên được bỏ qua hoàn toàn, ví dụ`[+100, 0, -50]`nên cư xử chính xác như`[+100, -50]`. Bất kỳ giải pháp nào coi số 0 là phân đoạn bị phá vỡ hoặc kích hoạt tính toán riêng biệt sẽ tính toán sai kết quả. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo sẽ mô phỏng một tài khoản có số dư đang hoạt động và đối với mỗi truy vấn hoàn tiền, hãy cố gắng xây dựng lại những khoản tiền gửi nào vẫn chưa được sử dụng. Người ta có thể tưởng tượng việc theo dõi từng khoản tiền gửi riêng biệt và đánh dấu các phần được tiêu thụ khi chi tiêu xảy ra. Điều này giống như việc duy trì nhiều khoản tiền khả dụng và liên tục khớp số tiền rút với số tiền gửi trước đó. Mặc dù đúng nhưng cách tiếp cận này trở nên nặng nề không cần thiết vì mỗi hoạt động chi tiêu có thể cần phải quét ngược để tìm các khoản tiền gửi có sẵn, dẫn đến hành vi bậc hai trong trường hợp xấu nhất khi luân phiên gửi tiền và rút tiền. 

Sự đơn giản hóa chính xuất phát từ việc nhận ra rằng vấn đề không yêu cầu phân bổ vốn mà chỉ yêu cầu tổng số tiền còn lại cuối cùng. Mỗi khoản tiền gửi sẽ tăng tổng giá trị hoàn lại, mỗi khoản chi tiêu sẽ giảm giá trị đó và cấu trúc trung gian không liên quan. Vì chi tiêu được đảm bảo hợp lệ ở mọi bước nên không có khoản tiền gửi nào cần theo dõi một phần ngoài tổng số tiền. 

Điều này thu gọn toàn bộ quá trình thành tính toán tổng tiền tố đang chạy trên tất cả các giá trị. Câu trả lời cuối cùng chỉ đơn giản là tổng của tất cả các mục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tiền gửi theo dõi Brute Force | O(n^2) | O(n) | Quá chậm | 
| Tổng hợp tiền tố | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`balance`về không. Biến này đại diện cho tổng số tiền chưa chi tiêu tại bất kỳ thời điểm nào. 
2. Lặp lại từng bản ghi theo thứ tự. Với mỗi giá trị`x`, cập nhật`balance`bằng cách thêm`x`trực tiếp đến nó. Giá trị dương làm tăng số tiền khả dụng, giá trị âm làm giảm số tiền đó và số 0 không thay đổi. 
3. Sau khi xử lý tất cả các bản ghi, xuất ra`balance`là số tiền hoàn lại cuối cùng. 

Lý do tích lũy trực tiếp này có giá trị là vì các hoạt động chi tiêu hoàn toàn phù hợp với các khoản tiền gửi trước đó. Vì mỗi lần rút tiền đều được đảm bảo được hỗ trợ bởi số dư hiện có nên không cần phải theo dõi số tiền gửi cụ thể đến từ đâu. Hệ thống hoạt động giống như một sự bảo toàn giá trị thuần túy mà không có ràng buộc ẩn giấu nào. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong chuỗi, tổng số tiền hiện có bằng chênh lệch ròng giữa tất cả số tiền được thêm vào và tất cả số tiền đã chi tiêu cho đến nay. Vì chi tiêu không bao giờ vượt quá số tiền sẵn có nên số tiền này luôn tương ứng chính xác với số tiền chưa chi tiêu còn lại. Không có sự phân bổ lại hoặc so khớp lịch sử nào có thể thay đổi giá trị ròng cuối cùng, vì mọi hoạt động đều tuyến tính và chỉ có thể đảo ngược ở dạng tổng hợp chứ không phải về cấu trúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
balance = 0

for _ in range(n):
    x = int(input().strip())
    balance += x

print(balance)
```Việc thực hiện duy trì một bộ tích lũy duy nhất. Mỗi dòng đầu vào được phân tích cú pháp và thêm trực tiếp vào bộ tích lũy này. Đầu vào nhanh được sử dụng vì mặc dù các ràng buộc nhỏ nhưng đây là phương pháp lập trình cạnh tranh tiêu chuẩn. 

Một điểm tinh tế là không có bộ lọc nào được áp dụng cho giá trị 0. Việc xử lý các số 0 một cách đặc biệt sẽ chỉ làm phức tạp logic mà không làm thay đổi kết quả. Một chi tiết quan trọng khác là kiểu số nguyên của Python xử lý các khoản tiền lớn một cách tự nhiên, do đó không có vấn đề tràn ngay cả khi tất cả các giá trị đều gần 10^9. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào trong đó người chơi gửi tiền, chi tiêu một phần trong số đó, sau đó nhận thêm: 

đầu vào:```
5
100
-30
0
50
-20
```| Bước | x | Số dư trước | Cân bằng sau | 
| --- | --- | --- | --- | 
| 1 | 100 | 0 | 100 | 
| 2 | -30 | 100 | 70 | 
| 3 | 0 | 70 | 70 | 
| 4 | 50 | 70 | 120 | 
| 5 | -20 | 120 | 100 | 

Đầu ra cuối cùng là 100. Điều này chứng tỏ rằng không có mục nào không có hiệu lực và việc chi tiêu đó chỉ làm giảm số tiền hoàn lại còn lại. 

Bây giờ hãy xem xét một mô hình xen kẽ: 

đầu vào:```
4
200
-100
-50
150
```| Bước | x | Số dư trước | Cân bằng sau | 
| --- | --- | --- | --- | 
| 1 | 200 | 0 | 200 | 
| 2 | -100 | 200 | 100 | 
| 3 | -50 | 100 | 50 | 
| 4 | 150 | 50 | 200 | 

Sản lượng cuối cùng là 200. Điều này cho thấy rằng chi tiêu trước đó có thể được “bổ sung” bằng các khoản tiền gửi sau và chỉ có số tiền ròng mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi bản ghi được xử lý chính xác một lần với số học thời gian không đổi | 
| Không gian | O(1) | Chỉ có một bộ tích lũy duy nhất được duy trì bất kể kích thước đầu vào | 

Các ràng buộc cho phép tối đa 1000 thao tác, do đó, việc truyền tuyến tính rất nhanh. Ngay cả đầu vào lớn hơn đáng kể cũng sẽ vẫn hiệu quả theo phương pháp này do chi phí xử lý mỗi phần tử không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    return str(solution())

def solution():
    n = int(input().strip())
    balance = 0
    for _ in range(n):
        balance += int(input().strip())
    return balance

# sample-like cases
assert solution.__code__  # placeholder to ensure function exists

# custom cases
sys.stdin = io.StringIO("1\n0\n")
assert solution() == 0, "minimum non-trivial zero effect"

sys.stdin = io.StringIO("3\n10\n-5\n-5\n")
assert solution() == 0, "exact cancellation"

sys.stdin = io.StringIO("5\n100\n100\n-50\n-50\n0\n")
assert solution() == 100, "mixed operations with zero"

sys.stdin = io.StringIO("4\n1000000000\n-1\n-999999999\n0\n")
assert solution() == 0, "boundary large values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, 0 | 0 | trường hợp cạnh chỉ bằng 0 | 
| 10, -5, -5 | 0 | hủy bỏ hoàn toàn | 
| trình tự hỗn hợp | 100 | tương tác của ops và số không | 
| giá trị lớn | 0 | an toàn số học biên | 

## Vỏ cạnh 

Một chuỗi chỉ có 0 như`n=3, [0, 0, 0]`tạo ra số dư bằng 0 vì không có thao tác nào thay đổi trạng thái. Thuật toán xử lý từng mục nhưng bộ tích lũy không thay đổi trong suốt. 

Một chuỗi hủy hoàn toàn như`[+50, -20, -30]`giảm số dư từng bước cho đến khi nó bằng không. Tổng số tiền hiện hành phản ánh sự hủy bỏ này một cách tự nhiên mà không cần phải theo dõi quyền sở hữu tiền trung gian. 

Một trường hợp hồi phục muộn như`[+100, -150, +200]`, cho thấy rằng thâm hụt tạm thời là không thể xảy ra do vấn đề bảo đảm và kết quả cuối cùng chỉ phụ thuộc vào số tiền ròng, ở đây là 150.
