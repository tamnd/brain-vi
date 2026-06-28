---
title: "CF 105129M - Trình xác thực sự cố"
description: "Hệ thống mà chúng tôi đang xác thực cực kỳ đơn giản: đối với mỗi trường hợp thử nghiệm, chúng tôi được cho biết có bao nhiêu thử nghiệm mà một chương trình phải chạy và bao nhiêu thử nghiệm trong số đó thực sự đã vượt qua. Từ đó, chúng tôi quyết định liệu chương trình có thành công hoàn toàn hay không."
date: "2026-06-27T19:23:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105129
codeforces_index: "M"
codeforces_contest_name: "Shorouk Academy 2024 Collegiate Programming Contest"
rating: 0
weight: 105129
solve_time_s: 43
verified: true
draft: false
---

[CF 105129M - Trình xác thực sự cố](https://codeforces.com/problemset/problem/105129/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hệ thống mà chúng tôi đang xác thực cực kỳ đơn giản: đối với mỗi trường hợp thử nghiệm, chúng tôi được cho biết có bao nhiêu thử nghiệm mà một chương trình phải chạy và bao nhiêu thử nghiệm trong số đó thực sự đã vượt qua. Từ đó, chúng tôi quyết định liệu chương trình có thành công hoàn toàn hay không. 

Mỗi trường hợp kiểm thử đầu vào mô tả một lần gửi. Số đầu tiên là tổng số trường hợp thử nghiệm tồn tại cho lần gửi đó. Số thứ hai là số lượng trường hợp kiểm thử được gửi thành công. Quyết định cần thiết là nhị phân. Nếu mọi trường hợp thử nghiệm đơn lẻ đều được thông qua, chúng tôi tuyên bố việc gửi hoàn toàn chính xác và xuất ra AC. Nếu thậm chí một trường hợp thử nghiệm không được thông qua, chúng tôi sẽ xuất WA. 

Các ràng buộc là nhỏ, với số lượng trường hợp kiểm thử trên mỗi truy vấn bị giới hạn tối đa là 100. Điều này có nghĩa là ngay cả việc xử lý mỗi trường hợp kiểm thử đơn giản nhất cũng không đáng kể xét về mặt chi phí tính toán. Không cần tính toán trước, cấu trúc dữ liệu hoặc thủ thuật tối ưu hóa vì kích thước đầu vào cho mỗi lần kiểm tra bị giới hạn không đổi và thao tác trên mỗi lần kiểm tra là so sánh theo thời gian không đổi. 

Các trường hợp cạnh chủ yếu là về sự bình đẳng ranh giới. Tình huống quan trọng nhất là khi số lượng bài kiểm tra đã vượt qua bằng tổng số bài kiểm tra, bao gồm cả trường hợp nhỏ nhất có thể xảy ra khi cả hai đều bằng 1. Ví dụ: một đầu vào như`1 1`phải xuất ra AC. Một đầu vào hơi khác như`1 0`phải xuất WA vì ít nhất một lần kiểm tra không thành công. Một lỗi phổ biến trong việc triển khai đơn giản là vô tình kiểm tra xem`m > 0`thay vì`m == n`, sẽ chấp nhận không chính xác các trường hợp như`3 2`. 

Một vấn đề tế nhị khác là quên rằng việc vượt qua tất cả các bài kiểm tra có nghĩa là sự bình đẳng chính xác chứ không phải “gần đạt mức bao phủ đầy đủ”. Đầu vào như`100 99`vẫn nên tạo WA mặc dù tỷ lệ thành công cao. 

## Phương pháp tiếp cận 

Việc giải thích vấn đề một cách thô bạo sẽ mô phỏng việc kiểm tra từng trường hợp thử nghiệm riêng lẻ. Người ta có thể tưởng tượng việc lặp lại tất cả n bài kiểm tra và xác minh xem mỗi bài kiểm tra có được đánh dấu là đạt hay không. Tuy nhiên, điều này là không cần thiết vì đầu vào đã cung cấp thông tin tổng hợp: số lần thử nghiệm thành công m. 

Trong mô phỏng trực tiếp, về mặt khái niệm, chúng tôi sẽ xây dựng lại một mảng có kích thước n và đánh dấu m trong số chúng là đã đạt, sau đó xác minh xem có tồn tại lỗi nào không. Điều đó sẽ tốn O(n) cho mỗi trường hợp thử nghiệm. Trong khi n tối đa là 100, cách tiếp cận này dư thừa về mặt logic vì chúng ta không bao giờ cần kiểm tra từng kết quả kiểm tra riêng lẻ. 

Quan sát quan trọng là toàn bộ vấn đề được rút gọn thành một kiểm tra đẳng thức duy nhất. Nếu số lần thử nghiệm thành công bằng tổng số lần thử nghiệm thì không có lần thử nghiệm thất bại nào. Ngược lại, có ít nhất một lỗi tồn tại. Điều này thu gọn vấn đề từ việc lặp qua một cấu trúc thành một so sánh duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(T · n) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu | O(T) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng nội dung gửi một cách độc lập và đưa ra phán quyết bằng cách so sánh trực tiếp. 

1. Đọc số lượng ca kiểm thử T. Điều này xác định số lượng bài gửi mà chúng tôi sẽ đánh giá. 
2. Mỗi lần nộp bài đọc hai số nguyên n và m. Đầu tiên là tổng số bài kiểm tra và thứ hai là số bài kiểm tra đã vượt qua. Hai giá trị này xác định đầy đủ tính chính xác nên không cần thêm dữ liệu. 
3. So sánh m với n. Nếu chúng bằng nhau thì có nghĩa là mọi thử nghiệm đều thành công. Ngược lại, ít nhất một thử nghiệm đã thất bại. 
4. Xuất ra "AC" nếu m bằng n, nếu không thì xuất ra "WA". 

Logic được cố ý tối thiểu vì đầu vào đã mã hóa kết quả đánh giá đầy đủ. 

### Tại sao nó hoạt động 

Đối với mỗi lần gửi, tập hợp các bài kiểm tra thất bại có kích thước n − m. Việc nộp chỉ được chấp nhận khi số lượng này bằng không. Vì n − m = 0 tương đương với m = n nên đẳng thức thể hiện đầy đủ tính đúng đắn. Không có cấu trúc ẩn hoặc sự phụ thuộc giữa các trường hợp thử nghiệm, vì vậy điều kiện này vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        if n == m:
            out.append("AC")
        else:
            out.append("WA")
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp đọc tất cả các trường hợp thử nghiệm một lần và lưu trữ kết quả vào danh sách để tránh lặp lại chi phí I/O. Quyết định cốt lõi là kiểm tra sự bình đẳng`n == m`, mã hóa trực tiếp xem có trường hợp thử nghiệm nào thất bại hay không. Không có điều kiện biên nào ngoài việc đảm bảo phân tích cú pháp chính xác các dòng đầu vào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
3 3
3 2
1 1
```| Bước | n | m | Điều kiện (n == m) | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | Đúng | AC | 
| 2 | 3 | 2 | Sai | WA | 
| 3 | 1 | 1 | Đúng | AC | 

Bài gửi thứ nhất và thứ ba vượt qua tất cả các bài kiểm tra một cách chính xác, trong khi bài gửi thứ hai thiếu một bài kiểm tra, ngay lập tức kích hoạt WA. 

### Ví dụ 2 

đầu vào:```
4
2 1
5 5
10 9
1 0
```| Bước | n | m | Điều kiện (n == m) | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | Sai | WA | 
| 2 | 5 | 5 | Đúng | AC | 
| 3 | 10 | 9 | Sai | WA | 
| 4 | 1 | 0 | Sai | WA | 

Ví dụ này nhấn mạnh rằng ngay cả một bài kiểm tra bị thiếu trong số nhiều bài kiểm tra cũng đủ để từ chối bài nộp và trường hợp lỗi nhỏ nhất khác 0 sẽ hoạt động giống hệt với trường hợp lỗi lớn hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm được xử lý bằng một phép so sánh số nguyên duy nhất | 
| Không gian | O(1) | Chỉ lưu trữ liên tục ngoài bộ đệm đầu ra | 

Các ràng buộc đảm bảo rằng T đủ nhỏ để một lần truyền tuyến tính duy nhất diễn ra tức thời. Mỗi phép toán là số học và so sánh theo thời gian không đổi, vì vậy giải pháp phù hợp cả về giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        out.append("AC" if n == m else "WA")
    return "\n".join(out)

# provided sample
assert run("3\n3 3\n3 2\n1 1\n") == "AC\nWA\nAC"

# minimum size, failure
assert run("1\n1 0\n") == "WA"

# minimum size, success
assert run("1\n1 1\n") == "AC"

# partial success
assert run("1\n100 99\n") == "WA"

# all equal large
assert run("1\n100 100\n") == "AC"

# mixed cases
assert run("3\n2 2\n2 1\n3 0\n") == "AC\nWA\nWA"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 | WA | trường hợp thất bại tối thiểu | 
| 1 1 | AC | trường hợp đậu tối thiểu | 
| 100 99 | WA | thất bại gần như hoàn toàn | 
| 100 100 | AC | thành công hoàn toàn ở giới hạn trên | 
| bộ nhỏ hỗn hợp | AC/WA/WA | tính nhất quán trong nhiều trường hợp | 

## Vỏ cạnh 

Trường hợp có ý nghĩa nhỏ nhất là`n = 1`. Đối với đầu vào`1 1`, thuật toán so sánh đẳng thức và đưa ra AC ngay lập tức, vì m bằng n. Vì`1 0`, điều kiện không thành công và WA được tạo ra. Điều này xác nhận việc xử lý chính xác các giá trị biên mà không cần bất kỳ cách viết hoa đặc biệt nào. 

Đối với các trường hợp gần ranh giới như`100 99`, so sánh vẫn xác định chính xác rằng ít nhất một thử nghiệm đã thất bại. Việc tính toán không phụ thuộc vào độ lớn mà chỉ phụ thuộc vào sự bình đẳng, vì vậy các giá trị lớn hoạt động giống hệt với các giá trị nhỏ.
