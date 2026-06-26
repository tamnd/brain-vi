---
title: "CF 105316K - Dấu hiệu"
description: "Mỗi trường hợp thử nghiệm mô tả một ảnh chụp nhanh lớp học. Đối với mỗi trường hợp kiểm tra, chúng tôi có một số học sinh và với mỗi học sinh, chúng tôi nhận được chính xác tám số nguyên biểu thị điểm số của họ trong tám môn học khác nhau."
date: "2026-06-23T06:12:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105316
codeforces_index: "K"
codeforces_contest_name: "2024 Aleppo Collegiate Programming Contest"
rating: 0
weight: 105316
solve_time_s: 47
verified: true
draft: false
---

[CF 105316K - Dấu](https://codeforces.com/problemset/problem/105316/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm mô tả một ảnh chụp nhanh lớp học. Đối với mỗi trường hợp kiểm tra, chúng tôi có một số học sinh và với mỗi học sinh, chúng tôi nhận được chính xác tám số nguyên biểu thị điểm số của họ trong tám môn học khác nhau. Nhiệm vụ là nén hồ sơ của mỗi học sinh thành một số duy nhất bằng cách cộng tám điểm môn học lại với nhau, sau đó xuất ra tổng điểm của mỗi học sinh. 

Cấu trúc có thứ bậc: đầu tiên chúng tôi đọc có bao nhiêu trường hợp kiểm tra, sau đó với mỗi trường hợp kiểm tra, chúng tôi đọc có bao nhiêu học sinh thuộc trường hợp đó, sau đó chúng tôi xử lý từng học sinh một cách độc lập. Không có sự tương tác nào tồn tại giữa các học sinh hoặc giữa các trường hợp kiểm tra, vì vậy mỗi tổng có thể được tính ngay lập tức sau khi đọc tám số. 

Các ràng buộc đủ nhỏ để bất kỳ chiến lược xử lý đơn giản nào cũng đủ. Số lượng bộ test nhiều nhất là 100, mỗi bộ test có tối đa 100 học sinh. Mỗi học sinh đóng góp chính xác 8 số nguyên, do đó tổng số số nguyên được xử lý trên toàn bộ đầu vào tối đa là 100 × 100 × 8 = 80000. Điều này hoàn toàn nằm trong giới hạn cho một lần quét tuyến tính đơn giản. 

Một cạm bẫy triển khai phổ biến xuất phát từ việc xử lý cấu trúc đầu vào không chính xác. Một sai lầm là đọc tất cả các số theo một chuỗi phẳng mà không tôn trọng ranh giới của các trường hợp kiểm thử, điều này vẫn có thể tạo ra tổng đúng nhưng lại sai nhóm. Một vấn đề khác là quên đặt lại vòng lặp cho mỗi trường hợp kiểm tra, dẫn đến việc in kết quả tích lũy trên các trường hợp kiểm tra thay vì trên mỗi học sinh. Ví dụ: nếu một lập trình viên tích lũy tổng theo một biến duy nhất giữa các học sinh, thì kết quả đầu ra có thể tăng không chính xác trên các dòng thay vì đặt lại cho mỗi học sinh. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là đọc tám điểm của mỗi học sinh và tính tổng của chúng ngay lập tức. Điều này đúng vì kết quả cuối cùng của mỗi học sinh chỉ phụ thuộc vào tám giá trị của chính họ mà không phụ thuộc vào các học sinh hoặc ca kiểm tra khác. Cách giải thích brute-force về cơ bản giống như giải pháp tối ưu: tính tổng rõ ràng tất cả tám số nguyên cho mỗi học sinh. 

Nếu chúng ta mô tả nó như một chiến lược đơn giản, người ta có thể tưởng tượng việc lưu trữ tất cả điểm của học sinh trước tiên trong một cấu trúc và sau đó lặp lại để tính tổng. Điều đó vẫn đúng nhưng gây ra chi phí không cần thiết về cả độ phức tạp của bộ nhớ và thời gian do có thêm dung lượng lưu trữ và lượt thứ hai. Bài làm thực tế của mỗi học sinh luôn không đổi vì nó chỉ là phép cộng tám. 

Quan sát quan trọng là vấn đề không có sự kết hợp giữa các phần tử. Mỗi học sinh tạo thành một khối độc lập gồm tám giá trị, do đó việc xử lý có thể được thực hiện theo kiểu truyền phát. Điều này loại bỏ mọi nhu cầu về mảng hoặc tiền xử lý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (lưu trữ rồi tính tổng) | O(n) | O(n) | Đã chấp nhận | 
| Tối ưu (tổng phát trực tuyến) | O(n) | O(1) | Đã chấp nhận | 

Ở đây n đề cập đến tổng số số nguyên được xử lý, hay tương đương là tổng số học sinh nhân với 8. 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp kiểm tra đầu vào và trong mỗi trường hợp kiểm tra, chúng tôi xử lý từng học sinh một cách độc lập. 

1. Đọc số lượng test case. Điều này xác định số lượng nhóm dữ liệu độc lập mà chúng tôi sẽ xử lý. 
2. Với mỗi đề thi, hãy đọc số học sinh. Điều này kiểm soát số dòng dữ liệu học sinh theo sau. 
3. Với mỗi học sinh, đọc chính xác tám số nguyên từ dòng đầu vào. Chúng đại diện cho các vectơ đặc trưng có chiều rộng cố định. 
4. Tính tổng của tám số nguyên này ngay khi đọc. Điều này tránh việc lưu trữ dữ liệu trung gian không cần thiết. 
5. Xuất kết quả tính được của học sinh đó trước khi chuyển sang học sinh tiếp theo. 
6. Lặp lại cho đến khi tất cả học sinh trong ca kiểm thử hiện tại được xử lý xong, sau đó chuyển sang ca kiểm thử tiếp theo.

Lý do chúng tôi có thể xuất dữ liệu ngay lập tức một cách an toàn cho mỗi học sinh là vì không có sự phụ thuộc trong tương lai. Khi tám giá trị được đọc, kết quả được xác định đầy đủ và không thể thay đổi dựa trên bất kỳ đầu vào nào khác. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thực tế là việc ánh xạ từ đầu vào đến đầu ra có thể tách rời giữa các học sinh. Mỗi giá trị đầu ra là một hàm xác định của chính xác tám số nguyên đầu vào. Vì không có phép biến đổi nào sử dụng trạng thái chung giữa các học sinh hoặc trường hợp kiểm thử nên việc tính toán từng tổng riêng biệt sẽ tạo ra kết quả chính xác trên toàn cầu. Thuật toán đang đánh giá một cách hiệu quả một tập hợp các biểu thức số học độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        for _ in range(n):
            nums = list(map(int, input().split()))
            out.append(str(sum(nums)))
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp đọc đầu vào bằng I/O nhanh vì số dòng có thể lên tới 10.000 trong trường hợp xấu nhất. Đối với mỗi dòng sinh viên, nó chuyển đổi tám số nguyên thành một danh sách và tính tổng của chúng một cách trực tiếp. 

Một chi tiết triển khai tinh tế là đầu ra đệm. Thay vì in từng dòng, kết quả được tập hợp thành danh sách và in một lần ở cuối. Điều này tránh được chi phí I/O lặp lại, điều này có thể quan trọng trong Python ngay cả đối với các ràng buộc nhỏ. 

Một chi tiết quan trọng khác là đảm bảo rằng mỗi dòng học sinh được xử lý độc lập. Cấu trúc vòng lặp đảm bảo rằng không có trạng thái nào được truyền giữa các học sinh hoặc giữa các ca kiểm thử. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
2
1 2 3 4 5 6 7 8
10 3 4 5 6 7 8 2
```| Bước | Sinh viên | Giá trị | Tổng hợp | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 2 3 4 5 6 7 8 | 36 | 
| 2 | 2 | 10 3 4 5 6 7 8 2 | 45 | 

Đầu ra:```
36
45
```Dấu vết này cho thấy mỗi học sinh được xử lý độc lập và kết quả chỉ phụ thuộc vào tám con số của chính họ. 

### Ví dụ 2 

đầu vào:```
1
3
5 5 5 5 5 5 5 5
1 1 1 1 1 1 1 1
100 0 0 0 0 0 0 0
```| Bước | Sinh viên | Giá trị | Tổng hợp | 
| --- | --- | --- | --- | 
| 1 | 1 | 5 5 5 5 5 5 5 | 40 | 
| 2 | 2 | 1 1 1 1 1 1 1 1 | 8 | 
| 3 | 3 | 100 0 0 0 0 0 0 0 | 100 | 

Đầu ra:```
40
8
100
```Ví dụ này nhấn mạnh rằng các giá trị có thể đồng nhất hoặc lệch, nhưng logic tính tổng vẫn không thay đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t · n) | Mỗi học sinh yêu cầu tính tổng chính xác 8 số nguyên, đây là công việc không đổi của mỗi học sinh | 
| Không gian | O(1) phụ trợ | Không cần lưu trữ tỷ lệ với kích thước đầu vào ngoài dòng hiện tại | 

Tổng số thao tác nhỏ ngay cả trong trường hợp tối đa, do đó giải pháp chạy dễ dàng trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("""1
2
1 2 3 4 5 6 7 8
10 3 4 5 6 7 8 2
""") == "36\n45"

# single student minimal case
assert run("""1
1
1 1 1 1 1 1 1 1
""") == "8"

# multiple test cases
assert run("""2
1
10 10 10 10 10 10 10 10
2
1 2 3 4 5 6 7 8
8 7 6 5 4 3 2 1
""") == "80\n36\n36"

# all zeros except one dominant value
assert run("""1
2
100 0 0 0 0 0 0 0
0 0 0 0 0 0 0 1
""") == "100\n1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| sinh viên độc thân | 8 | trường hợp ranh giới tối thiểu | 
| nhiều trường hợp thử nghiệm | tổng hỗn hợp | tính chính xác trên đầu vào được nhóm | 
| giá trị vượt trội | 100, 1 | xử lý phân phối thưa thớt | 

## Vỏ cạnh 

Một trường hợp tối thiểu với một sinh viên và tám giá trị giống nhau xác nhận rằng thuật toán không phụ thuộc vào bất kỳ cấu trúc nào ngoài phép tính tổng. Đối với đầu vào`1 1 1 1 1 1 1 1`, vòng lặp đọc dòng một lần, tính tổng là 8 và xuất ra ngay lập tức, khớp với hành vi dự kiến. 

Một trường hợp có nhiều trường hợp kiểm thử đảm bảo rằng không có rò rỉ trạng thái dư giữa chúng. Vì thuật toán không bao giờ tích lũy qua các trường hợp thử nghiệm nên mỗi nhóm được xử lý độc lập và đầu ra sẽ được đặt lại một cách tự nhiên ở mỗi ranh giới.
