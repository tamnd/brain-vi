---
title: "CF 102740C - Trang trí lại của Isabelle"
description: "Căn phòng là một hình chữ nhật bao gồm các hình vuông có kích thước N x M. Isabelle muốn bao phủ mọi hình vuông bằng những viên gạch hình chữ L giống hệt nhau."
date: "2026-07-29T00:57:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102740
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 9-25-20 Div. 2"
rating: 0
weight: 102740
solve_time_s: 48
verified: true
draft: false
---

[CF 102740C - Trang trí lại của Isabelle](https://codeforces.com/problemset/problem/102740/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Căn phòng là một hình chữ nhật gồm các hình vuông có kích thước`N`qua`M`. Isabelle muốn phủ kín mọi ô vuông bằng những viên gạch hình chữ L giống hệt nhau. Ngói là một tetromino được tạo thành từ bốn hình vuông: ba hình vuông liên tiếp trên một đường thẳng và một hình vuông bổ sung được gắn vào một trong các đầu, giống như một mảnh Tetris hình chữ L. Ngói có thể được xoay và phản ánh. 

Nhiệm vụ chỉ là quyết định xem có tồn tại một tấm lát hoàn chỉnh hay không. Đầu ra là`YES`nếu mỗi ô vuông ở tầng có thể được che phủ chính xác một lần, và`NO`nếu không thì. 

Kích thước có thể lớn tới 1000, do đó, giải pháp cố gắng xây dựng bảng hoặc tìm kiếm qua các vị trí là không cần thiết và sẽ là hướng đi sai lầm. Kích thước đầu vào gợi ý rằng câu trả lời phải đến từ một thuộc tính toán học của hình chữ nhật chứ không phải từ mô phỏng. 

Một lỗi phổ biến là chỉ kiểm tra xem diện tích có chia hết cho kích thước ô hay không. Mỗi ô có bốn hình vuông nên diện tích phải là bội số của bốn, nhưng điều kiện đó chưa đủ. Ví dụ, một`2 x 2`phòng có diện tích`4`, tuy nhiên một tetromino hình chữ L không thể lấp đầy nó vì viên gạch luôn bị thiếu một góc. 

Một trường hợp cạnh khác là phòng một hàng hoặc một cột. Ví dụ:```
Input:
1 8

Output:
NO
```Diện tích chia hết cho bốn, nhưng mỗi ô cần hai hàng hoặc hai cột sau khi xoay nên không thể che được một dải thẳng. 

Trường hợp quan trọng thứ hai là khi cả hai chiều đều lớn hơn một nhưng diện tích không chia hết cho 8:```
Input:
3 4

Output:
NO
```Diện tích là`12`, chia hết cho 4, nhưng đối số tô màu L-tetromino cho thấy số ô phải là số chẵn, do đó tổng diện tích thực tế phải chia hết cho 8. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi vị trí có thể có của mọi ô hình chữ L và sử dụng phương pháp quay lui để xác định xem hình chữ nhật có thể được che phủ hoàn toàn hay không. Điều này đúng vì nó khám phá mọi cách xếp gạch có thể, nhưng số lượng vị trí tăng lên nhanh chóng. Ngay cả một hình chữ nhật nhỏ cũng có thể có nhiều hướng và vị trí ô xếp, và không gian tìm kiếm sẽ trở nên theo cấp số nhân. Với kích thước lên tới 1000, cách tiếp cận này là không thể. 

Quan sát hữu ích là kích thước hình chữ nhật chứa tất cả thông tin chúng ta cần. Mỗi L-tetromino bao gồm bốn ô vuông, nhưng nó cũng có sự mất cân bằng về màu sắc khi bảng được tô màu theo các sọc ngang xen kẽ. Nếu chúng ta tô màu mỗi hàng khác là màu đen và các hàng còn lại là màu trắng thì L-tetromino luôn bao phủ ba hình vuông màu đen và một hình vuông màu trắng hoặc ba hình vuông màu trắng và một hình vuông màu đen. Để che toàn bộ hình chữ nhật, số lượng ô có mỗi ô không cân bằng phải bằng nhau, nghĩa là số ô phải là số chẵn. Vì mỗi ô có bốn hình vuông nên diện tích hình chữ nhật phải chia hết cho 8. 

Điều kiện này cũng đủ. Bất kỳ hình chữ nhật nào có cả hai cạnh lớn hơn một và diện tích chia hết cho 8 đều có thể được chia thành các hình chữ nhật nhỏ hơn có thể được lát bằng L-tetromino. Các khối xây dựng hữu ích nhỏ nhất là các hình chữ nhật như`2 x 4`Và`3 x 8`và các hình chữ nhật hợp lệ lớn hơn có thể được phân tách thành các mẫu này. 

Brute-force hoạt động vì nó trực tiếp tìm kiếm cách sắp xếp vị trí, nhưng không thành công vì số lượng sắp xếp quá lớn. Bất biến màu sắc làm giảm toàn bộ vấn đề để kiểm tra một vài điều kiện số học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Điều kiện toán học | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai chiều của căn phòng. 
2. Kiểm tra xem một trong hai chiều có`1`. Một hàng hoặc một cột không thể chứa tetromino hình chữ L nên đáp án là ngay lập tức`NO`. 
3. Tính diện tích`N * M`. Nếu nó không chia hết cho`8`, hình chữ nhật không thể xếp được vì số ô phải là số chẵn. 
4. Nếu cả hai điều kiện đều đạt, xuất ra`YES`. 

Tại sao nó hoạt động: thuật toán dựa trên tính bất biến mà mỗi ô hợp lệ đều sử dụng số L-tetromino chẵn. Vì mỗi ô bao gồm bốn ô nên mỗi hình chữ nhật hợp lệ phải có diện tích chia hết cho 8. Điều kiện kích thước còn lại sẽ loại bỏ các hình chữ nhật mỏng không thể thực hiện được trong đó ô không thể vừa khít về mặt vật lý. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    if n == 1 or m == 1:
        print("NO")
    elif (n * m) % 8 != 0:
        print("NO")
    else:
        print("YES")

if __name__ == "__main__":
    solve()
```Đầu tiên chương trình xử lý trường hợp biên hình học trong đó một cạnh là một. Việc kiểm tra này là riêng biệt vì chỉ riêng điều kiện khu vực sẽ chấp nhận sai các ví dụ như`1 x 8`. 

phép nhân`n * m`an toàn trong Python vì số nguyên có độ chính xác tùy ý. Điều kiện cuối cùng kiểm tra yêu cầu chia hết xuất phát từ đối số tô màu. 

Không có vòng lặp hoặc cấu trúc được lưu trữ vì câu trả lời chỉ phụ thuộc vào hai chiều đầu vào. 

## Ví dụ đã hoạt động 

Đối với ví dụ đầu tiên:```
Input:
6 4
```| Bước | n | m | Tình trạng | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 6 | 4 | Cả hai chiều đều lớn hơn 1 | Tiếp tục | 
| 2 | 6 | 4 | Diện tích = 24, chia hết cho 8 | CÓ | 

Hình chữ nhật có đủ không gian cho mẫu L-tetromino cần thiết và diện tích thỏa mãn bất biến cần thiết. 

Đối với ví dụ thứ hai:```
Input:
9 6
```| Bước | n | m | Tình trạng | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 9 | 6 | Cả hai chiều đều lớn hơn 1 | Tiếp tục | 
| 2 | 9 | 6 | Diện tích = 54, không chia hết cho 8 | KHÔNG | 

Mặc dù căn phòng đủ rộng về mặt vật lý nhưng tính bất biến về màu sắc sẽ ngăn cản việc lát gạch hoàn chỉnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một vài kiểm tra số học được thực hiện. | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung được sử dụng. | 

Các ràng buộc cho phép kích thước lên tới 1000 và giải pháp thời gian không đổi này dễ dàng phù hợp với các giới hạn. 

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

def solve():
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())

    if n == 1 or m == 1:
        print("NO")
    elif (n * m) % 8 != 0:
        print("NO")
    else:
        print("YES")

assert run("6 4\n") == "YES\n", "sample 1"
assert run("9 6\n") == "NO\n", "sample 2"

assert run("1 8\n") == "NO\n", "single row cannot fit"
assert run("2 4\n") == "YES\n", "smallest valid rectangle"
assert run("3 4\n") == "NO\n", "area divisible by 4 but not 8"
assert run("1000 1000\n") == "YES\n", "large valid rectangle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 8`|`NO`| Kiểm tra trường hợp ranh giới một chiều. | 
|`2 4`|`YES`| Kiểm tra khối ốp lát cơ bản nhỏ nhất. | 
|`3 4`|`NO`| Nắm bắt các giải pháp chỉ kiểm tra khả năng chia hết cho bốn. | 
|`1000 1000`|`YES`| Xác nhận giải pháp số học xử lý các kích thước tối đa. | 

## Vỏ cạnh 

Đối với một`1 x 8`room, trước tiên thuật toán sẽ kiểm tra các kích thước và trả về`NO`. Mặc dù tám ô có thể được chia thành hai nhóm bốn ô, nhưng không thể đặt ô này vì nó cần hàng thứ hai sau bất kỳ lần xoay nào. 

Đối với một`3 x 4`phòng, thuật toán sẽ tính diện tích`12`. Diện tích đủ lớn cho ba ô theo số lượng, nhưng ba ô sẽ vi phạm bất biến số ô chẵn. Kiểm tra chia hết cho tám sẽ nắm bắt được điều này và trả về`NO`. 

Đối với một`2 x 4`phòng, diện tích là`8`và cả hai chiều đều lớn hơn một. Thuật toán trả về`YES`, phù hợp với thực tế là hình chữ nhật này có thể được chia thành hai vị trí tetromino hình chữ L. 

Đối với một`1000 x 1000`phòng, kích thước hợp lệ nhưng diện tích là`1,000,000`, không chia hết cho 8. Thuật toán vẫn kết thúc ngay và trả về chính xác`NO`.
