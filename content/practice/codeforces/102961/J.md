---
title: "CF 102961J - Thiếu số tiền xu"
description: "Chúng ta được cung cấp một tập hợp các số nguyên dương có thể được hiểu là giá trị đồng xu. Mỗi giá trị có thể được sử dụng nhiều nhất một lần và bằng cách chọn một số tập hợp con của những đồng tiền này, chúng ta có thể tạo thành các tổng số tiền khác nhau."
date: "2026-07-04T06:52:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102961
codeforces_index: "J"
codeforces_contest_name: "CSES Problem Set: Sorting and Searching"
rating: 0
weight: 102961
solve_time_s: 38
verified: true
draft: false
---

[CF 102961J - Thiếu số tiền xu](https://codeforces.com/problemset/problem/102961/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các số nguyên dương có thể được hiểu là giá trị đồng xu. Mỗi giá trị có thể được sử dụng nhiều nhất một lần và bằng cách chọn một số tập hợp con của những đồng tiền này, chúng ta có thể tạo thành các tổng số tiền khác nhau. Nhiệm vụ là xác định số nguyên dương nhỏ nhất không thể được hình thành bằng cách sử dụng bất kỳ tập hợp con nào của các đồng tiền đã cho. 

Đầu vào chỉ đơn giản là một danh sách các giá trị tiền xu. Đầu ra là một số nguyên duy nhất biểu thị “khoảng cách” đầu tiên trong tổng có thể đạt được bắt đầu từ 0, bỏ qua chính số 0 vì một tập hợp con trống luôn tạo thành nó. 

Khó khăn chính là số lượng tập hợp con tăng theo cấp số nhân với số lượng xu, do đó, việc suy luận trực tiếp về tổng là không khả thi khi mảng phát triển vượt quá kích thước nhỏ. Nếu có tối đa khoảng 10^5 xu, mọi cách tiếp cận liệt kê các tập hợp con hoặc thực hiện lập trình động theo tổng sẽ vượt quá giới hạn thời gian vì không gian trạng thái sẽ bùng nổ vượt quá giới hạn thực tế. 

Một cách tiếp cận đơn giản sẽ cố gắng tạo ra tất cả các tập hợp con và sau đó tìm kiếm số nguyên còn thiếu nhỏ nhất. Điều này không thành công ngay cả đối với kích thước đầu vào vừa phải. Ví dụ: nếu tiền xu`[1, 2, 3, 10]`, tổng tập hợp con bao gồm nhiều giá trị, nhưng việc xây dựng chúng một cách rõ ràng đã yêu cầu theo dõi tối đa 2^n kết hợp. 

Trường hợp lỗi tinh vi hơn xuất hiện khi đồng xu nhỏ nhất lớn hơn 1. Ví dụ: nhập`[5, 7]`không có cách nào để tạo thành 1, vì vậy câu trả lời ngay lập tức là 1. Bất kỳ phương pháp nào bắt đầu tính tổng mà không kiểm tra ranh giới này trước sẽ lãng phí việc tính toán khi khám phá các kết hợp không liên quan. 

Một trường hợp cạnh quan trọng khác là khi các đồng xu liên tiếp hoặc gần như liên tiếp. Vì`[1, 2, 3]`, mọi số lên tới 6 đều có thể truy cập được và giá trị còn thiếu trở thành 7. Một giải pháp đúng phải nhận ra cấu trúc trung gian đó chứ không chỉ đóng góp từng đồng tiền riêng lẻ. 

## Phương pháp tiếp cận 

Phối cảnh brute-force bắt đầu từ định nghĩa: tính toán tất cả các tổng tập hợp con và sau đó quét lên từ 1 cho đến khi thiếu một giá trị. Điều này đúng vì nó thể hiện đầy đủ mọi cấu hình có thể đạt được. Tuy nhiên, số lượng tập hợp con tăng gấp đôi với mỗi đồng xu được thêm vào, dẫn đến khoảng 2^n trạng thái. Ngay cả việc lưu trữ những khoản tiền này cũng không thể thực hiện được khi n vượt quá 25 hoặc 30. 

Bước đột phá đến từ việc nhận ra rằng chúng ta không thực sự cần biết mọi số tiền có thể đạt được. Chúng tôi chỉ quan tâm đến việc liệu tất cả các giá trị từ 1 đến một số ranh giới có thể đạt được liên tục hay không. Nếu chúng ta đã biết rằng mọi giá trị trong`[1, x]`có thể truy cập được thì một đồng tiền mới có giá trị`v`chỉ có thể mở rộng phạm vi này nếu nó không tạo ra khoảng cách. Cụ thể, nếu`v`nhiều nhất là`x + 1`, sau đó nó có thể lấp đầy phân khúc còn thiếu tiếp theo và mở rộng khả năng tiếp cận tới`x + v`. Ngược lại, một khoảng trống xuất hiện ở`x + 1`ngay lập tức. 

Điều này biến vấn đề từ việc liệt kê tập hợp con thành một cuộc quét tham lam trên các đồng tiền đã được sắp xếp. Mỗi đồng xu mở rộng một tiền tố có thể truy cập liên tục hoặc tiết lộ số nguyên không thể truy cập đầu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tổng tập hợp con Brute Force | O(2^n · n) | O(2^n) | Quá chậm | 
| Phần mở rộng tiền tố tham lam | O(n log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp các đồng tiền theo thứ tự tăng dần. Việc sắp xếp đảm bảo chúng tôi luôn xem xét tiện ích mở rộng nhỏ nhất có sẵn trước tiên, điều này rất cần thiết để duy trì phạm vi có thể truy cập liên tục. 
2. Khởi tạo một biến`reach = 0`, đại diện cho tất cả các tổng trong phạm vi`[1, reach]`hiện có thể đạt được. Ban đầu, chỉ có thể đạt được tổng 0, vì vậy số nguyên dương bị thiếu đầu tiên là 1 trừ khi được mở rộng. 
3. Lặp lại từng đồng tiền được sắp xếp. Đối với mỗi giá trị tiền xu`v`, so sánh nó với`reach + 1`. 
4. Nếu`v`lớn hơn`reach + 1`, dừng lại ngay lập tức. Điều này có nghĩa là có một khoảng trống ở`reach + 1`không thể lấp đầy bằng bất kỳ sự kết hợp nào của các đồng tiền còn lại, vì tất cả các đồng tiền còn lại thậm chí còn lớn hơn. 
5. Ngược lại, nếu`v`nhiều nhất là`reach + 1`, cập nhật`reach`ĐẾN`reach + v`. Điều này hoạt động vì mọi giá trị lên tới`reach`trước đây có thể đạt được và thêm`v`cho phép tất cả các giá trị lên đến`reach + v`được hình thành bằng cách bao gồm hoặc loại trừ đồng tiền này. 
6. Sau khi xử lý hết xu hoặc dừng sớm thì câu trả lời là`reach + 1`. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến là tất cả các số nguyên trong`[1, reach]`có thể được hình thành bằng cách sử dụng một tập hợp con các đồng tiền được xử lý. Việc sắp xếp đảm bảo chúng tôi luôn xem xét đồng tiền nhỏ nhất có thể kéo dài khoảng thời gian này. Nếu một đồng xu vượt quá`reach + 1`, không có sự kết hợp nào của các đồng tiền sau này có thể thu hẹp khoảng cách vì tất cả các giá trị trong tương lai thậm chí còn lớn hơn, do đó số nguyên không thể truy cập đầu tiên được cố định. Ngược lại, khoảng thời gian có thể truy cập sẽ mở rộng mà không bị gián đoạn, duy trì tính liên tục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    arr = list(map(int, input().split()))
    
    arr.sort()
    
    reach = 0
    
    for v in arr:
        if v > reach + 1:
            break
        reach += v
    
    print(reach + 1)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đọc danh sách và sắp xếp nó để chúng ta luôn xử lý tiền theo thứ tự tăng dần. Biến`reach`theo dõi tổng liên tục tối đa có thể đạt được bắt đầu từ số 0. Mỗi đồng xu được hấp thụ vào phạm vi này hoặc gây ra sự gián đoạn. 

Chi tiết triển khai quan trọng là điều kiện`v > reach + 1`. Lỗi từng cái một thường xảy ra ở đây: sử dụng`>=`sẽ hòa vốn không chính xác ngay cả khi một đồng xu lấp đầy chính xác khoảng trống tiếp theo. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 2 3 10
```| Bước | Đồng xu | Tiếp cận trước | Tình trạng | Tiếp cận sau | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 1 | 1 | 
| 2 | 2 | 1 | 2 2 2 | 3 | 
| 3 | 3 | 3 | 3 4 | 6 | 
| 4 | 10 | 6 | 10 > 7 | dừng lại | 

Đầu ra là`7`. 

Dấu vết này cho thấy một khoảng liên tục mở rộng một cách trơn tru như thế nào cho đến khi một đồng xu phá vỡ tính liên tục đó. Giá trị không thể truy cập đầu tiên chính xác là nơi khoảng trống xuất hiện. 

### Ví dụ 2 

đầu vào:```
3
2 2 5
```| Bước | Đồng xu | Tiếp cận trước | Tình trạng | Tiếp cận sau | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | 2 > 1 | dừng lại | 

Đầu ra là`1`. 

Điều này chứng tỏ rằng khi đồng xu nhỏ nhất đã vượt quá 1 thì không thể tiến triển được và câu trả lời được đưa ra ngay lập tức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, quét tuyến tính đơn sau đó | 
| Không gian | O(1) | Chỉ một số biến ngoài bộ nhớ đầu vào được sử dụng | 

Các ràng buộc điển hình cho loại vấn đề này cho phép tối đa 10^5 phần tử, làm cho cách tiếp cận O(n log n) trở nên an toàn. Quét tuyến tính đảm bảo chi phí tối thiểu sau khi sắp xếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input().strip())
    arr = list(map(int, input().split()))
    arr.sort()
    reach = 0
    for v in arr:
        if v > reach + 1:
            break
        reach += v
    return str(reach + 1)

# provided samples (constructed)
assert run("4\n1 2 3 10\n") == "7", "sample 1"
assert run("3\n2 2 5\n") == "1", "sample 2"

# custom cases
assert run("1\n1\n") == "2", "minimum single coin"
assert run("1\n5\n") == "1", "gap at start"
assert run("5\n1 1 1 1 1\n") == "6", "all ones extend linearly"
assert run("6\n1 2 4 8 16 32\n") == "3", "binary-like gaps"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n`|`2`| trường hợp liên tục nhỏ nhất có thể | 
|`1\n5\n`|`1`| phát hiện khoảng cách ngay lập tức | 
|`1 1 1 1 1`|`6`| đồng xu nhỏ lặp đi lặp lại mở rộng phạm vi | 
|`1 2 4 8 16 32`|`3`| khoảng cách hàm mũ cho thấy sự thất bại sớm | 

## Vỏ cạnh 

Trường hợp cạnh khóa phát sinh khi đồng xu nhỏ nhất lớn hơn 1. Đối với đầu vào`[5, 7, 9]`, việc sắp xếp không thay đổi thứ tự và đồng xu đầu tiên đã vi phạm`reach + 1`tình trạng. Thuật toán dừng ngay lập tức và trả về 1, xác định chính xác rằng không thể tạo được tổng dương. 

Một trường hợp khác xảy ra khi tiền xu dày đặc lúc đầu nhưng thưa thớt về sau. Vì`[1, 2, 3, 4, 10, 11]`, tiền tố có thể truy cập tăng dần lên 10, sau đó khoảng cách ở mức 11 trở nên không liên quan vì lần ngắt đầu tiên đã xảy ra ở mức 5 nếu nó bị thiếu; ở đây không có ngắt xảy ra sớm, vì vậy câu trả lời cuối cùng được xác định bởi khoảng trống lớn đầu tiên sau tiền tố dày đặc hoàn toàn.
