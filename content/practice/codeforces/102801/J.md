---
title: "CF 102801J - Tô màu các khối"
description: "Lưới là một bảng (N lần N). Mỗi ô có thể được sơn màu đen hoặc trắng. Một số cặp ô được kết nối bởi một hạn chế: hai ô trong một cặp như vậy không được phép có cùng màu."
date: "2026-07-28T23:00:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102801
codeforces_index: "J"
codeforces_contest_name: "The 14th Chinese Northeast Collegiate Programming Contest"
rating: 0
weight: 102801
solve_time_s: 73
verified: true
draft: false
---

[CF 102801J - Tô màu các khối](https://codeforces.com/problemset/problem/102801/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Lưới là một bảng (N \times N). Mỗi ô có thể được sơn màu đen hoặc trắng. Một số cặp ô được kết nối bởi một hạn chế: hai ô trong một cặp như vậy không được phép có cùng màu. Nhiệm vụ là đếm xem có bao nhiêu màu hoàn chỉnh trên bảng thỏa mãn mọi hạn chế. 

Mẫu hạn chế kết nối một ô với các ô cách nhau ba vị trí theo chiều ngang và với hai ô trong hàng phía trên nó hai bước, dịch chuyển một vị trí sang trái hoặc phải. Vì mọi hạn chế chỉ nói rằng hai ô phải có các màu khác nhau, nên vấn đề thực sự là yêu cầu số lần tô màu hợp lệ của biểu đồ được tạo bởi các ô và cạnh này. 

Đối với bất kỳ thành phần nào được kết nối của biểu đồ này, khi chúng ta chọn màu của một ô, mọi ô khác trong thành phần đó đều có màu bắt buộc. Quyền tự do duy nhất là chọn màu bắt đầu của thành phần, vì vậy mọi thành phần được kết nối đều đóng góp hệ số hai. Toàn bộ nhiệm vụ trở thành việc tìm số lượng thành phần được kết nối. 

Đầu vào chứa tối đa (10^5) trường hợp thử nghiệm và (N) có thể lớn bằng (10^9). Điều này ngay lập tức loại trừ mọi mô phỏng trên lưới vì ngay cả việc lưu trữ bảng cũng sẽ yêu cầu bộ nhớ (O(N^2)). Giải pháp chỉ phải phụ thuộc vào cấu trúc lặp lại nhỏ của các kết nối chứ không phụ thuộc vào số lượng ô thực tế. 

Những trường hợp khó khăn là những bảng rất nhỏ mà mô hình chung chưa hình thành. Ví dụ: khi (N=2), có bốn ô và không có cặp ô nào được kết nối vì tất cả các lần nhảy có thể đều rời khỏi bảng. Câu trả lời là (2^4=16), trong khi giải pháp giả sử mẫu bảng lớn sẽ cho kết quả sai. 

Đối với (N=3), hai hàng có cùng tính chẵn lẻ được kết nối với nhau, nhưng hàng đơn còn lại không có đối tác chẵn lẻ theo chiều dọc. Hàng đó chứa ba chuỗi ngang riêng biệt. Đồ thị có bốn thành phần nên đáp án lại là (16). Việc triển khai bất cẩn chỉ tính số chẵn lẻ của hàng sẽ bỏ lỡ các thành phần bổ sung này. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ tạo tất cả (N^2) ô, thêm một cạnh cho mọi hạn chế và chạy duyệt đồ thị để đếm các thành phần được kết nối. Điều này phù hợp với các bo mạch nhỏ vì mọi thành phần đều có thể được phát hiện bằng BFS hoặc DFS. Tuy nhiên, với (N=10^9), số lượng ô là (10^{18}), do đó, ngay cả việc tạo bước đầu tiên của phương pháp này cũng không thể thực hiện được. 

Quan sát hữu ích đến từ việc xem xét chuyển động bên trong một thành phần hoạt động như thế nào. Mỗi lần di chuyển sẽ thay đổi số hàng bằng 0 hoặc hai. Kết quả là tính chẵn lẻ của hàng không bao giờ thay đổi. Các ô từ hàng chẵn không bao giờ có thể đến hàng lẻ và lưới sẽ chia thành hai nửa độc lập. 

Bên trong một hàng, kết nối ngang di chuyển ba cột cùng một lúc, do đó các cột có cùng phần dư ba được kết nối. Đối với một hàng riêng biệt có chiều rộng đủ lớn, điều này sẽ tạo ra ba nhóm riêng biệt. 

Các kết nối chéo giữa các hàng có cùng tính chẵn lẻ là thứ hợp nhất các nhóm này. Nếu có ít nhất hai hàng có cùng tính chẵn lẻ và (N \geq 3), thì việc dịch chuyển theo một cột sẽ kết nối cả ba nhóm modulo-ba với nhau. Sau khi điều đó xảy ra, toàn bộ nửa chẵn lẻ là một thành phần được kết nối. 

Điều này chỉ để lại một số trường hợp nhỏ cần xử lý riêng. Đối với (N \geq 4), cả hai số chẵn lẻ của hàng đều chứa nhiều hàng, do đó có chính xác hai thành phần. Với (N=3), một hàng chẵn lẻ có hai hàng và hàng kia có một hàng cách ly, tạo ra bốn thành phần. Với (N=2), mọi ô đều bị cô lập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N^2)) | (O(N^2)) | Quá chậm | 
| Công thức thành phần | (O(1)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc (N). Chỉ riêng kích thước bảng xác định cấu trúc thành phần, do đó không cần xây dựng lưới. 
2. Tay cầm (N=1). Có một ô và không có hạn chế nên có thể tô màu theo hai cách. 
3. Tay cầm (N=2). Mỗi ô đều được cô lập, đưa ra bốn lựa chọn độc lập. 
4. Tay cầm (N=3). Đồ thị có bốn thành phần liên thông nên đáp án là (2^4). 
5. Với mọi (N \geq 4), đầu ra (4). Các hàng chẵn tạo thành một thành phần và các hàng lẻ tạo thành một thành phần khác. 

Tại sao nó hoạt động: mọi cạnh hạn chế buộc các màu đối lập nhau bên trong một thành phần được kết nối. Một thành phần lưỡng cực được kết nối có chính xác hai phép gán màu hợp lệ, vì việc chọn màu của một ô sẽ xác định màu của tất cả các ô khác. Biểu đồ phân chia chính xác theo tính chẵn lẻ của hàng và các trường hợp nhỏ là trường hợp duy nhất một nhóm chẵn lẻ không thể hợp nhất thành một thành phần. Do đó số lượng màu là (2^{\text{số thành phần}}). 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())

        if n == 1:
            ans.append("2")
        elif n == 2 or n == 3:
            ans.append("16")
        else:
            ans.append("4")

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Chương trình chỉ kiểm tra kích thước của bo mạch vì kiểu kết nối ổn định sau các trường hợp nhỏ. Các giá trị đặc biệt được xử lý trước trường hợp chung để tránh áp dụng sai lý luận lưới lớn khi thiếu hàng. 

Không sử dụng mảng, đệ quy hoặc cấu trúc đồ thị. Điều này là cần thiết vì lưới lý thuyết có thể chứa tối đa (10^{18}) ô. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên: 

đầu vào:```
1
1
```Sự phát triển của trạng thái là: 

| Bước | N | Linh kiện | Trả lời | 
| --- | --- | --- | --- | 
| Bắt đầu | 1 | 1 | 2 | 

Khối duy nhất có hai màu có thể, vì vậy câu trả lời là (2). 

Đối với mẫu thứ hai: 

đầu vào:```
1
6
```Sự phát triển của trạng thái là: 

| Bước | N | Thành phần chẵn lẻ hàng | Tổng số thành phần | Trả lời | 
| --- | --- | --- | --- | --- | 
| Chia các hàng theo số chẵn lẻ | 6 | 2 | 2 | 4 | 

Các hàng chẵn kết nối thành một thành phần và các hàng lẻ kết nối với một thành phần khác. Mỗi thành phần có hai lựa chọn màu sắc, tạo ra (2^2=4). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(1)) cho mỗi trường hợp thử nghiệm | Chỉ có một vài so sánh với (N) được thực hiện. | 
| Không gian | (O(1)) | Không có cấu trúc dữ liệu phụ thuộc vào (N) được lưu trữ. | 

Giải pháp xử lý (10^5) trường hợp kiểm thử một cách dễ dàng vì mọi trường hợp đều được xử lý độc lập với công việc liên tục. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return out

assert run("2\n1\n6\n") == "2\n4\n", "samples"

assert run("4\n1\n2\n3\n4\n") == "2\n16\n16\n4\n", "small boundaries"

assert run("3\n10\n1000000000\n999999999\n") == "4\n4\n4\n", "large values"

assert run("2\n2\n3\n") == "16\n16\n", "small isolated structures"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| (N=1) | 2 | Vỏ đơn bào | 
| (N=2,3) | 16 | Các trường hợp nhỏ trước mẫu lặp lại | 
| Giá trị lớn (N) | 4 | Xử lý liên tục các bảng lớn | 
| Thùng nhỏ hỗn hợp | 16,16 | Chuyển tiếp ranh giới | 

## Vỏ cạnh 

Khi (N=1), không có hạn chế nào cả. Thuật toán trả về (2), khớp với hai lựa chọn cho một ô. 

Khi (N=2), các bước nhảy có thể có độ dài ba cột hoặc hai hàng, vì vậy mọi đích đến có thể đều nằm ngoài bảng. Tất cả bốn ô đều độc lập, cho (2^4=16). 

Khi (N=3), các hàng có cùng tính chẵn lẻ có thể kết nối thông qua các bước di chuyển theo đường chéo. Hàng giữa còn lại không thể kết nối theo chiều dọc và giữ ba nhóm cột ba modulo của nó tách biệt. Điều này tạo ra bốn thành phần và thuật toán trả về (2^4=16). 

Khi (N\geq4), mỗi hàng chẵn lẻ chứa đủ hàng để di chuyển theo đường chéo để hợp nhất ba lớp cột. Biểu đồ có chính xác hai thành phần, một thành phần tương đương với mỗi hàng, vì vậy câu trả lời luôn là (4). 

Tôi cũng có thể điều chỉnh nó thành định dạng biên tập ngắn hơn theo phong cách Codeforces nếu bạn muốn một phiên bản gần giống với những gì sẽ xuất hiện trên blog cuộc thi.
