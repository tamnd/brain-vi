---
title: "CF 106015H - Lời thì thầm của ánh sáng nơi chưa biết"
description: "Chúng ta được tặng một bộ sưu tập N mảnh rêu ma thuật, trong đó mỗi mảnh đóng góp một số giờ ánh sáng cố định sau khi được sử dụng trong đèn lồng."
date: "2026-06-22T16:46:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106015
codeforces_index: "H"
codeforces_contest_name: "Game of Coders 4 - Over the Garden Wall"
rating: 0
weight: 106015
solve_time_s: 46
verified: true
draft: false
---

[CF 106015H - Lời thì thầm của ánh sáng nơi không xác định](https://codeforces.com/problemset/problem/106015/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được tặng một bộ sưu tập N mảnh rêu ma thuật, trong đó mỗi mảnh đóng góp một số giờ ánh sáng cố định sau khi được sử dụng trong đèn lồng. Đèn lồng được sử dụng theo cách tuyến tính đơn giản: từng mảnh rêu lần lượt được tiêu thụ hết và tổng thời gian đèn lồng vẫn sáng chính xác là tổng của tất cả thời lượng đốt riêng lẻ. 

Dữ liệu đầu vào bao gồm một số nguyên N, theo sau là N số nguyên, trong đó mỗi số nguyên biểu thị thời gian một mảnh rêu cụ thể có thể giữ cho đèn cháy sáng. Nhiệm vụ là tính tổng thời gian ghi kết hợp nếu tất cả các mảnh được sử dụng. 

Từ góc độ ràng buộc, N tối đa là 1000 và mỗi thời gian ghi có thể lớn tới 10^9. Điều này ngay lập tức ngụ ý rằng việc truyền tuyến tính đơn giản qua mảng là đủ. Ngay cả trong giới hạn nghiêm ngặt 1 giây, việc thực hiện 1000 phép cộng là không đáng kể, do đó, bất kỳ giải pháp nào quét danh sách một lần và tích lũy tổng đều là tối ưu. 

Vấn đề tế nhị duy nhất là số tiền có thể tăng lên lớn. Với 1000 giá trị, mỗi giá trị lên tới 10^9, tổng số có thể đạt tới 10^12, vượt quá giới hạn số nguyên 32 bit. Bất kỳ triển khai nào lưu trữ kết quả ở loại 32 bit sẽ âm thầm tràn vào các ngôn ngữ quan trọng. Trong Python đây không phải là vấn đề, nhưng trong các ngôn ngữ khác, đó là một cạm bẫy tiềm ẩn kinh điển. 

Các trường hợp cạnh là tối thiểu nhưng vẫn có giá trị nêu cụ thể. Nếu N = 1, câu trả lời chỉ là giá trị duy nhất đó. Nếu tất cả Hi là 10^9 thì tổng là 10^12 và vẫn phải được in chính xác. Nếu tất cả các giá trị đều giống nhau thì kết quả sẽ phản ánh phép nhân với N, chứ không phải bất kỳ hình thức cài đặt lại lặp đi lặp lại hoặc lỗi tích lũy một phần nào. 

## Phương pháp tiếp cận 

Cách giải thích trực tiếp của vấn đề là mô phỏng quá trình sử dụng đèn lồng bằng cách lặp lại tất cả các mảnh rêu và tích lũy thời gian cháy của chúng. Cách tiếp cận bạo lực này đã phù hợp với quy trình được mô tả: lấy từng phần theo thứ tự, cộng thời lượng của nó vào tổng thời gian đang chạy và tiếp tục cho đến khi tiêu thụ hết tất cả các phần. 

Điều này hoạt động chính xác vì hoạt động của đèn lồng không có sự tương tác giữa các mảnh rêu. Không có sự chồng chéo, không phân rã và không có logic có điều kiện. Mỗi phần đóng góp độc lập vào tổng số cuối cùng. 

Chế độ xem bạo lực vẫn đúng ngay cả khi được triển khai theo đúng nghĩa đen dưới dạng phép cộng lặp lại trong một vòng lặp. Tổng số thao tác chính xác là N phép cộng, đối với N lên tới 1000 là không đáng kể. Không cần tối ưu hóa ngoài một lần vượt qua. 

Quan sát chính là quy trình được mô tả không phải là vấn đề mô phỏng chuyển đổi trạng thái mà là vấn đề tổng hợp thuần túy. Khi chúng tôi nhận ra rằng trạng thái của đèn lồng chỉ là tổng chạy, vấn đề sẽ giảm xuống việc tính tổng của một mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(N) | O(1) | Đã chấp nhận | 
| Tổng trực tiếp | O(N) | O(1) | Đã chấp nhận | 

Ở đây, cả hai cách tiếp cận đều giống nhau về mặt hiệu quả, nhưng cách đóng khung “tổng hợp trực tiếp” sẽ tránh được mọi chi phí diễn giải không cần thiết. 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một bộ tích lũy duy nhất thể hiện tổng thời gian ghi được tích lũy cho đến nay. Chúng tôi xử lý từng mảnh rêu chính xác một lần và thêm giá trị của nó vào bộ tích lũy này. 

### bước 

1. Đọc số nguyên N cho biết có bao nhiêu mảnh rêu. Điều này xác định có bao nhiêu giá trị chúng tôi sẽ xử lý. 
2. Khởi tạo biến tổng bằng 0. Biến này biểu thị thời gian đốt tích lũy của tất cả các mảnh rêu được xử lý cho đến nay. 
3. Lặp lại N số nguyên biểu thị thời gian ghi. Đối với mỗi giá trị Xin chào, hãy cộng trực tiếp vào tổng số. Điều này phản ánh chiếc đèn lồng đã tiêu thụ hoàn toàn mảnh rêu đó trước khi chuyển sang chiếc đèn tiếp theo. 
4. Sau khi xử lý tất cả các giá trị, kết quả tổng là kết quả cuối cùng. 

### Tại sao nó hoạt động

Tại mỗi thời điểm trong quá trình lặp lại, tổng bằng tổng của tất cả các mảnh rêu được xử lý cho đến nay. Vì mỗi mảnh rêu được xử lý chính xác một lần và đóng góp chính xác toàn bộ giá trị của nó nên tổng cuối cùng là tổng của tất cả Hi. Không có sự phụ thuộc giữa các phần tử nên việc sắp xếp lại hoặc nhóm lại không làm thay đổi kết quả. Do đó, bộ tích lũy khớp với định nghĩa chính xác về tổng thời gian cháy của đèn lồng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input().strip())
    arr = list(map(int, input().split()))
    print(sum(arr))

if __name__ == "__main__":
    main()
```Việc triển khai dựa trực tiếp vào tổng tích hợp sẵn của Python, thực hiện một lần truyền tuyến tính duy nhất qua danh sách đầu vào. Điều này tương ứng chính xác với việc duy trì bộ tích lũy đang chạy như được mô tả trong thuật toán. Đầu vào đọc sử dụng I/O nhanh để tránh chi phí không cần thiết, mặc dù hiệu suất không quá quan trọng đối với N lên tới 1000. 

Một lỗi phổ biến ở các ngôn ngữ khác là sử dụng loại số nguyên không thể chứa giá trị lên tới 10^12. Trong Python, việc này được xử lý tự động với các số nguyên chính xác tùy ý, do đó không cần đặc biệt chú ý gì ngoài việc sử dụng một biến duy nhất. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào đơn giản trong đó ba mảnh rêu có thời gian cháy là 1, 2 và 3. 

### Ví dụ 1 

đầu vào:```
3
1 2 3
```| Bước | Giá trị hiện tại | Tổng cộng | 
| --- | --- | --- | 
| Bắt đầu | - | 0 | 
| 1 | 1 | 1 | 
| 2 | 2 | 3 | 
| 3 | 3 | 6 | 

Tổng số tiền đang tăng lên đều đặn khi mỗi mảnh rêu được thêm vào. Đầu ra cuối cùng là 6, là tổng thời gian ghi. 

Bây giờ hãy xem xét một trường hợp có một giá trị lớn. 

### Ví dụ 2 

đầu vào:```
1
1000000000
```| Bước | Giá trị hiện tại | Tổng cộng | 
| --- | --- | --- | 
| Bắt đầu | - | 0 | 
| 1 | 1000000000 | 1000000000 | 

Điều này xác nhận rằng thuật toán xử lý chính xác trường hợp tối thiểu và giá trị lớn mà không bị tràn trong Python. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi mảnh rêu được xử lý chính xác một lần trong một lần | 
| Không gian | O(1) | Chỉ cần một bộ tích lũy duy nhất ngoài bộ nhớ đầu vào | 

Giới hạn giới hạn N ở mức 1000, do đó việc quét tuyến tính nằm trong giới hạn một cách thoải mái. Ngay cả khi N lớn hơn nhiều, cấu trúc tương tự sẽ vẫn tối ưu vì mọi phần tử phải được đọc ít nhất một lần. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import builtins

    output = io.StringIO()
    with redirect_stdout(output):
        import sys
        input = sys.stdin.readline

        n = int(sys.stdin.readline().strip())
        arr = list(map(int, sys.stdin.readline().split()))
        print(sum(arr))

    return output.getvalue().strip()

# provided sample
assert run("3\n1 2 3\n") == "6"

# single element
assert run("1\n5\n") == "5"

# all equal values
assert run("4\n2 2 2 2\n") == "8"

# large values
assert run("2\n1000000000 1000000000\n") == "2000000000"

# alternating values
assert run("5\n1 2 3 4 5\n") == "15"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 giá trị duy nhất | 5 | Xử lý đầu vào tối thiểu | 
| 4 giá trị bằng nhau | 8 | Tính chính xác tích lũy lặp đi lặp lại | 
| cặp lớn | 2000000000 | Độ chính xác số nguyên lớn | 
| Trình tự 1..5 | 15 | Hành vi tổng hợp chung | 

## Vỏ cạnh 

Với N = 1, thuật toán đọc một giá trị duy nhất và xuất ra ngay lập tức. Ví dụ, đầu vào`1 10`khởi tạo tổng số thành 0, cộng 10 và in 10, khớp với định nghĩa về thời gian đốt đèn lồng. 

Đối với các giá trị tối đa như`1000`bản sao của`10^9`, vòng lặp sẽ thêm từng giá trị một cách tuần tự. Bộ tích lũy tăng từ 0 lên 10^12 mà không bị cắt bớt trung gian trong Python, tạo ra kết quả cuối cùng chính xác. Trong các ngôn ngữ có số nguyên có chiều rộng cố định, đây là nơi thường xảy ra tràn, nhưng bản thân logic vẫn đúng. 

Nếu tất cả các giá trị giống hệt nhau, hãy nói`3 7 7 7`, thuật toán thực hiện phép cộng lặp lại cùng một số. Cấu trúc của vòng lặp không giả định tính duy nhất, do đó tổng cuối cùng tự nhiên trở thành 21, khớp với hành vi tổng hợp dự kiến.
