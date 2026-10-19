---
title: "CF 105637A - Giá cuối cùng"
description: "Chúng ta được cung cấp một mô hình đơn giản về cách giá biến động theo thời gian. Có một giá trị ban đầu biểu thị giá vào ngày đầu tiên và sau đó vào mỗi ngày tiếp theo, chúng ta sẽ biết giá thay đổi bao nhiêu so với ngày trước đó."
date: "2026-06-26T13:50:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105637
codeforces_index: "A"
codeforces_contest_name: "The 2022 ICPC Asia Tehran Regional Contest"
rating: 0
weight: 105637
solve_time_s: 34
verified: true
draft: false
---

[CF 105637A - Giá cuối cùng](https://codeforces.com/problemset/problem/105637/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mô hình đơn giản về cách giá biến động theo thời gian. Có một giá trị ban đầu biểu thị giá vào ngày đầu tiên và sau đó vào mỗi ngày tiếp theo, chúng ta sẽ biết giá thay đổi bao nhiêu so với ngày trước đó. Mỗi thay đổi được áp dụng tích lũy, do đó giá trị của ngày thứ i là giá trị của ngày i−1 cộng với thay đổi thứ i−1. 

Nhiệm vụ là tính giá trị cuối cùng sau khi tất cả các thay đổi hàng ngày đã được áp dụng, tức là giá trị vào ngày cuối cùng. 

Mặc dù tuyên bố này nghe giống như một “quá trình thời gian”, nhưng cấu trúc này hoàn toàn là sự tích lũy tuyến tính. Mọi cập nhật đều mang tính bổ sung và độc lập, vì vậy chúng tôi thực sự chỉ đang xây dựng lại tổng tiền tố trên mảng thay đổi. 

Kích thước đầu vào nhỏ, tối đa là 1000 ngày. Điều đó ngay lập tức cho chúng ta biết rằng ngay cả việc quét O(n) đơn giản cũng là quá đủ, vì 10^3 thao tác là không đáng kể trong các giới hạn thông thường. Bất kỳ giải pháp nào ngoài thời gian tuyến tính sẽ là chi phí không cần thiết. 

Một số trường hợp cần lưu ý. 

Nếu chỉ có một ngày thì không có thay đổi nào được áp dụng nên đáp án chỉ là giá trị ban đầu. Việc triển khai ngây thơ luôn cố đọc ít nhất một thay đổi sẽ thất bại ở đây. 

Nếu tất cả các thay đổi đều âm, giá cuối cùng có thể vẫn dương theo sự đảm bảo của vấn đề, nhưng việc triển khai trung gian vô tình kẹp các giá trị về 0 (một lỗi phổ biến trong tư duy kiểu mô phỏng) sẽ tạo ra kết quả không chính xác. 

Nếu các thay đổi bao gồm số 0 thì không có gì đặc biệt xảy ra về mặt toán học, nhưng việc triển khai bỏ qua các mục 0 theo giả định rằng chúng không liên quan sẽ phá vỡ tính chính xác. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ mô phỏng quá trình này từng ngày. Chúng tôi bắt đầu từ giá ban đầu và lặp lại qua từng thay đổi, cập nhật giá trị hiện tại mỗi lần. Đây đã là thời gian O(n), vì mỗi ngày đóng góp một bản cập nhật liên tục. Không có trạng thái ẩn, không phân nhánh và không có vụ nổ tổ hợp. 

Cố gắng “tối ưu hóa” hơn nữa sẽ bỏ lỡ mục đích của cấu trúc. Quan sát quan trọng là giá trị cuối cùng chỉ phụ thuộc vào tổng của tất cả các thay đổi, không phụ thuộc vào thứ tự hoặc trạng thái trung gian của chúng. Các cập nhật lặp đi lặp lại sẽ thu gọn thành một biểu thức số học duy nhất: giá trị ban đầu cộng với tổng của tất cả các chênh lệch hàng ngày. 

Vì vậy, cách tiếp cận tối ưu không phải là một kỹ thuật thuật toán khác, mà là sự thừa nhận rằng mô phỏng và tính tổng trực tiếp ở đây giống hệt nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng từng bước | O(n) | O(1) | Đã chấp nhận | 
| Tổng số thay đổi trực tiếp | O(n) | O(1) | Đã chấp nhận | 

Cả hai đều có hiệu quả thực hiện giống nhau trong thực tế. 

## Hướng dẫn thuật toán 

1. Đọc số ngày n. Điều này xác định có bao nhiêu bản cập nhật sẽ được áp dụng. 
2. Đọc giá ban đầu. Đây là trạng thái bắt đầu trước khi có bất kỳ thay đổi nào. 
3. Khởi tạo một dòng điện thay đổi với mức giá ban đầu này. 
4. Đối với mỗi dòng trong số n-1 dòng tiếp theo, hãy đọc sự thay đổi hàng ngày và thêm nó vào dòng hiện tại. Mỗi lần bổ sung tương ứng chính xác với việc di chuyển về phía trước một ngày. 
5. Sau khi xử lý tất cả các thay đổi, dòng điện đầu ra là giá cuối cùng. 

Quyết định quan trọng là chúng ta không bao giờ cần lưu trữ toàn bộ chuỗi giá trị. Mỗi mức giá trung gian chỉ tồn tại dưới dạng trạng thái tạm thời và vì mọi chuyển đổi đều có tính chất cộng thêm nên chỉ giữ giá trị hiện tại sẽ bảo toàn tất cả thông tin cần thiết. 

### Tại sao nó hoạt động 

Quá trình xác định sự lặp lại của dạng value[i] = value[i−1] + delta[i]. Việc mở rộng phép lặp này nhiều lần cho thấy value[n] bằng giá trị ban đầu cộng với tổng của tất cả các delta. Vì phép cộng có tính chất kết hợp nên việc nhóm các phép toán không thành vấn đề và không có bước trung gian nào ảnh hưởng đến kết quả cuối cùng ngoài việc đóng góp giá trị của nó một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    cur = int(input())
    
    for _ in range(n - 1):
        cur += int(input())
    
    print(cur)

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện phép lặp được mô tả trong hướng dẫn. Biến`cur`đại diện cho giá đang phát triển. Mỗi dòng đầu vào được áp dụng ngay lập tức, tránh việc lưu trữ toàn bộ mảng không cần thiết. 

Một lỗi triển khai phổ biến là cố gắng lưu trữ tất cả các giá trị trong danh sách và tính toán lại sau đó. Điều đó là không cần thiết nhưng vẫn an toàn ở đây. Một sai lầm nghiêm trọng hơn là khởi tạo bộ tích lũy không chính xác, chẳng hạn như bắt đầu từ số 0 thay vì giá ban đầu đã cho, điều này làm thay đổi toàn bộ kết quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
11
68
```| Bước | Giá hiện tại | Hoạt động | 
| --- | --- | --- | 
| Bắt đầu | 11 | giá trị ban đầu | 
| Ngày 2 | 79 | 11 + 68 | 

Đầu ra là 79. 

Điều này xác nhận trường hợp đơn giản nhất khi áp dụng một bản cập nhật duy nhất. 

### Ví dụ 2 

đầu vào:```
4
110
-5
0
27
```| Bước | Giá hiện tại | Hoạt động | 
| --- | --- | --- | 
| Bắt đầu | 110 | giá trị ban đầu | 
| Ngày 2 | 105 | 110 - 5 | 
| Ngày 3 | 105 | 105 + 0 | 
| Ngày 4 | 132 | 105 + 27 | 

Đầu ra là 132. 

Dấu vết này cho thấy các thay đổi âm và 0 hoạt động thống nhất theo cùng một quy tắc cộng mà không cần xử lý đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi thay đổi hàng ngày được xử lý một lần | 
| Không gian | O(1) | chỉ có một biến tích lũy được duy trì | 

Các ràng buộc cho phép tối đa 1000 ngày, do đó, một lượt tuyến tính duy nhất cũng nằm trong giới hạn. Ngay cả khi giới hạn lớn hơn nhiều, cách tiếp cận tương tự vẫn sẽ mở rộng quy mô một cách hiệu quả vì nó tránh được mọi tính toán lồng nhau. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n = int(input())
    cur = int(input())
    for _ in range(n - 1):
        cur += int(input())
    return str(cur)

# provided samples
assert run("2\n11\n68\n") == "79"
assert run("4\n110\n-5\n0\n27\n") == "132"

# custom cases
assert run("1\n5\n") == "5", "single day no changes"
assert run("3\n10\n-3\n-7\n") == "0", "all negative changes"
assert run("5\n1\n1\n1\n1\n1\n") == "5", "uniform increments"
assert run("2\n1000000\n-1\n") == "999999", "boundary decrement"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chỉ 1 ngày | giá trị ban đầu | không xử lý thay đổi | 
| tất cả đều tiêu cực | 0 | độ đúng theo giảm | 
| lặp lại +1 | tích lũy tuyến tính | bổ sung lặp đi lặp lại | 
| giá trị lớn | 999999 | độ đúng số học biên | 

## Vỏ cạnh 

Đối với đầu vào một ngày, thuật toán đọc giá trị ban đầu và không bao giờ đi vào vòng lặp, do đó đầu ra chính xác là giá trị đó. Không có nguy cơ cập nhật chưa được khởi tạo vì bộ tích lũy được đặt ngay lập tức từ đầu vào. 

Đối với các chuỗi chỉ âm, mỗi phép trừ được áp dụng trực tiếp vào tổng số đang chạy. Thuật toán không giả định giá trị dương tại bất kỳ điểm nào, do đó, nó theo dõi mức giảm một cách tự nhiên mà không gặp vấn đề về kẹp hoặc tràn. 

Đối với những thay đổi bằng 0, mỗi lần lặp sẽ thực hiện`cur += 0`, giữ nguyên trạng thái. Vì không có điều kiện nào lọc ra các giá trị 0 nên chúng được đưa vào quy trình tích lũy thống nhất một cách chính xác.
