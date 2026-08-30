---
title: "CF 104385L - Kim ren Zhang Fei - Dày và mịn"
description: "Chúng ta được cho một số nguyên $N$, biểu thị số lượng binh lính trong quân đội của Tào Tháo đóng quân trước cầu Trường Bản. Truyện mô tả tiếng gầm của Trương Phi gây hoảng loạn và khiến quân lính bỏ chạy."
date: "2026-07-01T02:55:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104385
codeforces_index: "L"
codeforces_contest_name: "2023 (ICPC) Jiangxi Provincial Contest -- Official Contest"
rating: 0
weight: 104385
solve_time_s: 37
verified: true
draft: false
---

[CF 104385L - Kim luồn chỉ Zhang Fei - Dày và mịn](https://codeforces.com/problemset/problem/104385/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên duy nhất$N$, tượng trưng cho số lượng binh sĩ trong quân đội của Tào Tháo đóng quân trước cầu Trường Bản. Truyện mô tả tiếng gầm của Trương Phi gây hoảng loạn và khiến quân lính bỏ chạy. Tuy nhiên, có một ngoại lệ cụ thể: Tướng Xia Houjie bị ảnh hưởng theo cách khác và được tuyên bố rõ ràng là không tính vào số người bị “sợ hãi”. 

Vì vậy, nhiệm vụ giảm xuống còn việc xác định có bao nhiêu binh sĩ thực sự bỏ chạy sau khi loại trừ một người không được tính là một phần của nhóm sợ hãi bỏ chạy. 

Mặc dù câu chuyện được cách điệu hóa nhiều nhưng cấu trúc cơ bản vẫn thuần túy là số học. Kích thước đầu vào tăng lên$10^6$, điều này ngay lập tức loại trừ bất kỳ điều gì liên quan đến việc mô phỏng đối với từng binh sĩ có hành vi phức tạp, nhưng ở đây, điều đó thậm chí không cần thiết vì chúng tôi không theo dõi các tương tác hoặc trình tự. Một tính toán thời gian không đổi là đủ. 

Trường hợp cạnh tinh tế xuất hiện khi$N = 1$. Trong tình huống đó, nếu người duy nhất có mặt là người bị loại khỏi cuộc đếm, số binh sĩ sợ hãi sẽ trở thành con số không. Bất kỳ phép trừ ngây thơ nào cũng phải xử lý việc này một cách rõ ràng mà không tạo ra các giá trị âm. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ mô phỏng rõ ràng từng người lính, đánh dấu xem họ có bỏ chạy do tiếng gầm hay không, sau đó điều chỉnh số đếm bằng cách loại trừ Xia Houjie. Điều này về mặt khái niệm sẽ liên quan đến việc lặp lại trên tất cả$N$các thực thể và duy trì cờ trạng thái cho mỗi người lính. Mặc dù điều này đã là tuyến tính và sẽ dễ dàng vượt qua đối với$N \le 10^6$, điều đó là không cần thiết vì không có sự chuyển đổi trạng thái riêng lẻ nào thực sự phụ thuộc vào vị trí hoặc tương tác. 

Nhận xét quan trọng là câu chuyện xác định một cá nhân đặc biệt duy nhất không nên được đưa vào danh sách cuối cùng. Điều đó có nghĩa là câu trả lời chỉ đơn giản là tổng số binh sĩ trừ đi một người bị loại. Không có cấu trúc, thứ tự hoặc điều kiện nào khác ảnh hưởng đến kết quả. 

Do đó, bài toán được chuyển thành một phép tính số học theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(N) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Số học tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$N$, đại diện cho tổng số binh sĩ ở phía trước cầu. Đây là toàn bộ dân số trước khi áp dụng bất kỳ loại trừ nào. 
2. Xác định chính xác một cá nhân không được đưa vào số liệu cuối cùng “sợ hãi”. Tuyên bố xác định rõ ràng ngoại lệ này, vì vậy chúng tôi không tính người đó vào kết quả. 
3. Tính kết quả như sau$N - 1$. Điều này trực tiếp loại bỏ cá nhân bị loại khỏi tổng số. 
4. Nếu$N = 1$, công thức tương tự vẫn được áp dụng và đương nhiên mang lại kết quả 0, thể hiện chính xác rằng không có người lính nào bị tính là sợ hãi bỏ chạy. 

### Tại sao nó hoạt động 

Bất biến quan trọng là tất cả binh lính ngoại trừ một người đều đóng góp vào số lượng cuối cùng những người chạy trốn. Vì vấn đề đảm bảo chính xác một loại trừ và không có điều kiện lọc nào khác nên câu trả lời cuối cùng luôn là tổng kích thước trừ đi thực thể bị loại trừ duy nhất đó. Không có sự phụ thuộc giữa các binh sĩ, không có hành vi có điều kiện và không có sự thay đổi năng động trong dân số, do đó phép trừ vẫn có hiệu lực đối với tất cả các đầu vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input().strip())
    print(max(0, n - 1))

if __name__ == "__main__":
    main()
```Việc thực hiện là cố ý tối thiểu. Phép trừ$n - 1$mã hóa loại trừ được mô tả trong câu lệnh. các`max(0, ...)`bảo vệ đảm bảo tính chính xác khi$n = 1$, ngăn chặn mọi đầu ra âm trong trường hợp suy biến. 

Giải pháp sử dụng quá trình xử lý đầu vào theo thời gian không đổi và tránh lưu trữ bất kỳ thứ gì vượt quá số nguyên đơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
```Chúng tôi bắt đầu với$n = 5$. 

| Bước | Giá trị của n | Tính toán | Kết quả | 
| --- | --- | --- | --- | 
| Ban đầu | 5 | - | - | 
| Áp dụng quy tắc | 5 | 5 - 1 | 4 | 

Đầu ra:```
4
```Điều này xác nhận rằng một cá nhân bị loại khỏi số lượng cuối cùng. 

### Ví dụ 2 

đầu vào:```
1
```| Bước | Giá trị của n | Tính toán | Kết quả | 
| --- | --- | --- | --- | 
| Ban đầu | 1 | - | - | 
| Áp dụng quy tắc | 1 | 1 - 1 | 0 | 

Đầu ra:```
0
```Trường hợp này cho thấy điều kiện biên trong đó người lính duy nhất là người bị loại trừ, dẫn đến không có kẻ chạy trốn nào được tính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một phép tính số học duy nhất được thực hiện bất kể kích thước đầu vào | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Các ràng buộc cho phép lên tới một triệu binh sĩ, nhưng việc tính toán không phụ thuộc vào việc lặp lại chúng. Giải pháp thoải mái phù hợp trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    n = int(input().strip())
    return str(max(0, n - 1))

# provided sample-like cases
assert run("5\n") == "4"
assert run("1\n") == "0"

# custom cases
assert run("2\n") == "1", "minimum non-trivial case"
assert run("1000000\n") == "999999", "maximum boundary case"
assert run("3\n") == "2", "small mid-range sanity check"
assert run("10\n") == "9", "typical linear check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | 1 | trường hợp nhỏ nhất trong đó phép trừ khác 0 | 
| 1 | 0 | trường hợp loại trừ ranh giới | 
| 1000000 | 999999 | hành vi ràng buộc tối đa | 
| 3 | 2 | tính đúng đắn chung | 

## Vỏ cạnh 

### Trường hợp$N = 1$đầu vào:```
1
```Thuật toán tính toán$1 - 1 = 0$. Điều này phù hợp với cách giải thích rằng cá nhân duy nhất là người bị loại trừ, không để lại binh lính nào được tính. 

### Trường hợp$N = 2$đầu vào:```
2
```Việc thực hiện tiến hành như$2 - 1 = 1$. Một người lính còn lại sau khi loại trừ cá nhân đặc biệt. Không có điều kiện ẩn nào làm thay đổi kết quả này nên tính toán ổn định. 

### Trường hợp đầu vào lớn 

đầu vào:```
1000000
```Kết quả phép trừ$999999$trực tiếp. Vì hoạt động diễn ra trong thời gian không đổi nên không có nguy cơ suy giảm hiệu suất hoặc tràn số học số nguyên của Python.
