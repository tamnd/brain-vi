---
title: "CF 102617A - Hệ thống xếp hạng"
description: "Bài toán mô phỏng một trận đấu một đấu một trong một trò chơi. Chloe có xếp hạng hiện tại và cô ấy đấu với một người chơi khác có xếp hạng nhất định. Người chiến thắng nhận được 10% đánh giá của người thua cuộc dưới dạng điểm đạt được, trong khi người thua cuộc mất 10% đánh giá của chính họ."
date: "2026-07-31T03:57:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102617
codeforces_index: "A"
codeforces_contest_name: "mBIT Rookie November 2019"
rating: 0
weight: 102617
solve_time_s: 71
verified: true
draft: false
---

[CF 102617A - Hệ thống xếp hạng](https://codeforces.com/problemset/problem/102617/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán mô phỏng một trận đấu một đấu một trong một trò chơi. Chloe có xếp hạng hiện tại và cô ấy đấu với một người chơi khác có xếp hạng nhất định. Người chiến thắng nhận được 10% đánh giá của người thua cuộc dưới dạng điểm đạt được, trong khi người thua cuộc mất 10% đánh giá của chính họ. Dựa trên cả xếp hạng và kết quả trận đấu, chúng ta cần tính xếp hạng của Chloe sau trận đấu. 

Dữ liệu đầu vào chứa xếp hạng hiện tại của Chloe, xếp hạng hiện tại của đối thủ và một ký tự mô tả Chloe thắng hay thua. Đầu ra là xếp hạng mới mà Chloe có sau khi áp dụng thay đổi xếp hạng. 

Xếp hạng có thể lớn như$10^9$. Vì chỉ có một vài phép tính số học nên kích thước của các con số là điều duy nhất quan trọng. Cách tiếp cận sử dụng mô phỏng, tìm kiếm hoặc tính toán lặp đi lặp lại sẽ tạo thêm công việc không cần thiết. Cần có giải pháp thời gian không đổi vì kích thước đầu vào không chứa chuỗi lớn để xử lý. 

Một sai lầm phổ biến là luôn trừ đi 10% đánh giá của Chloe. Điều đó chỉ đúng khi Chloe thua cuộc. Khi Chloe thắng, số tiền thu được sẽ đến từ xếp hạng của đối thủ. 

Ví dụ: nếu đầu vào là:```
1000
1200
w
```đầu ra đúng là:```
1120
```Một giải pháp bất cẩn có thể tính toán$1000 + 100$, bởi vì nó sử dụng đánh giá của chính Chloe làm nguồn điểm. Mức tăng chính xác là 10% đánh giá của đối thủ, là 120. 

Một trường hợp khác là thua trước một đối thủ yếu hơn nhiều. Vì:```
100
900
l
```đầu ra là:```
90
```Việc thua dựa vào đánh giá của chính Chloe chứ không phải đánh giá của đối thủ. Sử dụng đánh giá của đối thủ sẽ bị trừ 90 điểm thay vì 10 một cách sai lầm. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ là cố gắng lập mô hình toàn bộ hệ thống xếp hạng hoặc tìm kiếm thông qua các thay đổi xếp hạng có thể có cho đến khi đạt được giá trị cuối cùng. Điều này là không cần thiết vì trận đấu chỉ có duy nhất một trạng thái chuyển tiếp. Hành động duy nhất có thể làm là thêm 10% xếp hạng của đối thủ hoặc xóa 10% xếp hạng hiện tại của Chloe. Bất kỳ cách tiếp cận nào thực hiện nhiều hơn một vài phép tính số học đều giải được một bài toán khó hơn bài đã cho. 

Quan sát quan trọng là quy tắc cập nhật xếp hạng có tính chất quyết định. Phần thắng của người thắng và phần thua của người thua đã được mô tả bằng toán học nên câu trả lời có thể được tính toán trực tiếp. Cấu trúc bài toán không yêu cầu cấu trúc dữ liệu, phép lặp hoặc mô phỏng. Chúng ta chỉ cần chọn công thức đúng dựa trên kết quả trận đấu. 

Lực lượng vũ phu hoạt động vì chỉ có một kết quả phù hợp có thể xảy ra để đánh giá, nhưng nó trở thành chi phí không cần thiết khi so sánh với một công thức trực tiếp. Quan sát cho thấy sự thay đổi xếp hạng chỉ phụ thuộc vào hai xếp hạng hiện tại và đặc tính thắng hoặc thua làm giảm toàn bộ vấn đề về thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k) trong đó k là công việc mô phỏng không cần thiết | O(1) | Về nguyên tắc quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc xếp hạng của Chloe, xếp hạng của đối thủ và nhân vật kết quả. Ba giá trị này chứa tất cả thông tin cần thiết để xác định xếp hạng tiếp theo. 
2. Nếu Chloe thắng, hãy tính 10% đánh giá của đối thủ và cộng vào đánh giá của Chloe. Số điểm Chloe nhận được đều được lấy từ đối thủ nên đánh giá của đối thủ chính là giá trị dùng để tăng điểm. 
3. Nếu Chloe thua, hãy tính 10 phần trăm xếp hạng hiện tại của Chloe và trừ nó khỏi xếp hạng của cô ấy. Người chơi thua sẽ mất đi 10% số điểm của mình. 
4. In kết quả xếp hạng. 

Tại sao nó hoạt động: 

Bất biến của thuật toán là giá trị xếp hạng được lưu trữ luôn là xếp hạng của Chloe sau khi áp dụng chính xác quy tắc so khớp được mô tả bởi bài toán. Một chiến thắng sẽ thay đổi xếp hạng của cô ấy theo sự đóng góp của đối thủ và một trận thua sẽ thay đổi xếp hạng của cô ấy bằng sự đóng góp của chính cô ấy. Vì đây là hai lần chuyển đổi duy nhất có thể xảy ra nên thuật toán bao gồm mọi trường hợp hợp lệ và áp dụng bản cập nhật được yêu cầu chính xác. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

def solve():
    c = int(input())
    o = int(input())
    result = input().strip()

    if result == "w":
        c += o // 10
    else:
        c -= c // 10

    print(c)

if __name__ == "__main__":
    solve()
```Hai dòng đầu tiên đọc hai xếp hạng dưới dạng số nguyên. Số nguyên Python xử lý khả năng$10^9$giá trị một cách an toàn mà không có mối lo ngại tràn. 

Điều kiện kiểm tra kết quả trận đấu. Để thắng, số tiền cộng thêm sẽ được tính từ xếp hạng của đối thủ. Vì tất cả các xếp hạng đều chia hết cho 10 nên việc chia số nguyên cho 10 sẽ cho ra số điểm chính xác đạt được. Đối với trường hợp thua, phép trừ được tính dựa trên xếp hạng hiện tại của Chloe. 

Thứ tự thực hiện các thao tác có ý nghĩa quan trọng trong trường hợp thua cuộc. Số tiền bị loại bỏ phải được tính từ xếp hạng của Chloe trước khi cập nhật. Viết công thức dưới dạng`c -= c // 10`duy trì hành vi đó vì phía bên phải được đánh giá trước khi chuyển nhượng. 

## Ví dụ đã hoạt động 

Ví dụ 1: 

đầu vào:```
1000
1200
w
```| Bước | Đánh giá Chloe | Đánh giá đối thủ | Kết quả | Thay đổi | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 1000 | 1200 | w | không | 
| Áp dụng quy tắc thắng | 1120 | 1200 | w | +120 | 
| Đầu ra | 1120 | 1200 | w | xong | 

Ví dụ này chứng tỏ rằng lợi ích đó đến từ sự đánh giá của đối thủ. Thuật toán giữ nguyên xếp hạng ban đầu của Chloe cho đến khi áp dụng bản cập nhật win. 

Ví dụ 2: 

đầu vào:```
1500
800
l
```| Bước | Đánh giá Chloe | Đánh giá đối thủ | Kết quả | Thay đổi | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 1500 | 800 | tôi | không | 
| Áp dụng quy luật mất mát | 1350 | 800 | tôi | -150 | 
| Đầu ra | 1350 | 800 | tôi | xong | 

Ví dụ này cho thấy hướng ngược lại. Chloe mất điểm dựa trên đánh giá của chính mình, mặc dù đánh giá của đối thủ khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Thuật toán chỉ thực hiện phân tích cú pháp đầu vào và một số phép toán số học cố định. | 
| Không gian | O(1) | Chỉ có ba biến số nguyên hoặc chuỗi được lưu trữ. | 

Giải pháp sử dụng tài nguyên không đổi nên dễ dàng nằm trong giới hạn ngay cả khi xếp hạng đạt giá trị tối đa. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    c = int(sys.stdin.readline())
    o = int(sys.stdin.readline())
    result = sys.stdin.readline().strip()

    if result == "w":
        c += o // 10
    else:
        c -= c // 10

    sys.stdin = old_stdin
    return str(c)

# provided sample
assert solve("""1000
1200
w
""") == "1120", "sample 1"

# custom cases
assert solve("""100
900
l
""") == "90", "minimum values and loss calculation"

assert solve("""1000000000
1000000000
w
""") == "1100000000", "large values"

assert solve("""5000
7000
w
""") == "5700", "win uses opponent rating"

assert solve("""5000
7000
l
""") == "4500", "loss uses own rating"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`100, 900, l`|`90`| Kiểm tra giới hạn dưới và công thức thua | 
|`1000000000, 1000000000, w`|`1100000000`| Kiểm tra việc xử lý số nguyên lớn | 
|`5000, 7000, w`|`5700`| Khẳng định chiến thắng sử dụng đánh giá của đối thủ | 
|`5000, 7000, l`|`4500`| Xác nhận thua lỗ sử dụng đánh giá của Chloe | 

## Vỏ cạnh 

Đối với trường hợp Chloe thắng đối thủ mạnh hơn, chẳng hạn như:```
1000
1200
w
```thuật toán đi vào nhánh win và thêm`1200 // 10`, tạo ra 1120. Giải pháp sử dụng đánh giá của Chloe cho cả hai kết quả sẽ thất bại ở đây vì nó chỉ cộng 100. 

Đối với trường hợp Chloe thua đối thủ có chỉ số rất khác:```
100
900
l
```thuật toán đi vào nhánh mất và trừ`100 // 10`. Xếp hạng của đối thủ không liên quan đến phép tính này nên kết quả là 90. Điều này phát hiện các triển khai sử dụng sai xếp hạng của đối thủ khi tính toán mọi thay đổi. 

Để xếp hạng bằng nhau:```
5000
5000
w
```thuật toán thêm 500 điểm và trả về 5500. Số học tương tự hoạt động vì giá trị người thắng và người thua bằng nhau, xác nhận rằng không cần xử lý đặc biệt cho tình huống này.
