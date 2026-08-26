---
title: "CF 104344A - Tài liệu phân phối"
description: "Chúng tôi được hỏi liệu có thể phân phối chính xác số kẹo $K$ cho trẻ em $N$ với hai ràng buộc hay không. Mỗi đứa trẻ phải nhận được ít nhất số kẹo $L$ và không đứa trẻ nào được nhận nhiều hơn số kẹo $R$."
date: "2026-07-01T18:27:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104344
codeforces_index: "A"
codeforces_contest_name: "Maratona dos Bixes 2023 - UNICAMP"
rating: 0
weight: 104344
solve_time_s: 72
verified: true
draft: false
---

[CF 104344A - Tài liệu phân phối](https://codeforces.com/problemset/problem/104344/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được hỏi liệu có thể phân phối chính xác$K$kẹo trong số$N$trẻ em dưới hai ràng buộc. Mỗi đứa trẻ phải nhận được ít nhất$L$kẹo, và không đứa trẻ nào có thể nhận được nhiều hơn$R$kẹo. Chúng ta phải quyết định xem có tồn tại bất kỳ phép gán số nguyên nào thỏa mãn tất cả trẻ em cùng một lúc khi sử dụng tất cả các loại kẹo hay không. 

Về cơ bản, đây là một vấn đề khả thi đối với việc phân bổ có giới hạn. Mỗi đứa trẻ đóng góp một biến$x_i$như vậy$L \le x_i \le R$, và tổng của tất cả$x_i$phải bằng$K$. Câu hỏi giảm xuống còn liệu$K$nằm trong phạm vi tổng có thể đạt được được hình thành bởi các biến giới hạn này. 

Các ràng buộc đủ nhỏ để bất kỳ$O(N)$hoặc$O(1)$tính toán là chuyện nhỏ. Các giá trị$N \le 10^3$,$K \le 10^6$, Và$R \le 10^5$có nghĩa là chúng tôi không cần phải mô phỏng các phân phối hoặc tìm kiếm trên các bài tập. Thay vào đó, chúng ta nên tập trung vào việc tìm ra các giới hạn trên và dưới chặt chẽ cho tổng số tiền. 

Trường hợp cạnh tinh tế xuất hiện khi tổng số tối thiểu có thể đã vượt quá$K$hoặc khi tổng tối đa có thể vẫn nhỏ hơn$K$. Một trường hợp cạnh khác là khi$L = R$, điều này buộc mọi đứa trẻ phải nhận được số kẹo chính xác như nhau, biến bài toán thành một phép kiểm tra tính chia hết nghiêm ngặt. Ví dụ, nếu$N = 3$,$L = R = 3$, thì tổng số duy nhất có thể là$9$. Bất kì$K \neq 9$phải thất bại ngay lập tức. 

Một trường hợp góc khác là$L = 0$, điều này cho phép một số trẻ không nhận được gì, nhưng giới hạn trên vẫn hạn chế tổng số tiền. Trong trường hợp đó, tính khả thi phụ thuộc hoàn toàn vào việc liệu$K \le N \cdot R$. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ cố gắng chia kẹo cho từng đứa trẻ. Với mỗi đứa trẻ, chúng ta có thể thử mọi giá trị giữa$L$Và$R$, phân phối đệ quy số kẹo còn lại cho đến khi tất cả$N$trẻ em được gán giá trị. Điều này tạo ra hệ số phân nhánh khoảng$R - L + 1$mỗi đứa trẻ, dẫn đến$(R-L+1)^N$các cấu hình có thể. Ngay cả với giới hạn nhỏ, điều này là không thể thực hiện được vì$N$có thể đạt được$10^3$, làm cho việc liệt kê trở nên lớn lao về mặt thiên văn. 

Quan sát quan trọng là chúng ta không quan tâm đến các phân phối riêng lẻ mà chỉ quan tâm đến việc liệu tổng có tồn tại hay không. Mỗi đứa trẻ đóng góp độc lập trong một khoảng thời gian cố định, do đó tập hợp tất cả các khoản tiền có thể tạo thành một phạm vi liên tục từ số tiền tối thiểu có thể đạt được đến số tiền tối đa có thể đạt được. 

Số tiền tối thiểu xảy ra khi mọi đứa trẻ đều nhận được$L$, cho$N \cdot L$. Số tiền tối đa xảy ra khi mọi đứa trẻ đều nhận được$R$, cho$N \cdot R$. Vì chúng ta có thể điều chỉnh từng phần tử con một cách độc lập theo các bước số nguyên nên mọi giá trị số nguyên giữa hai cực trị này đều có thể đạt được. Do đó, vấn đề giảm xuống còn việc kiểm tra xem$K$nằm trong khoảng$[N \cdot L, N \cdot R]$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | Hàm mũ | Ngăn xếp đệ quy O(N) | Quá chậm | 
| Kiểm tra định kỳ | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số kẹo tối thiểu là$N \cdot L$. Điều này thể hiện trường hợp mọi đứa trẻ đều nhận được số tiền được phép ít nhất và không có phân phối hợp lệ nào có thể xuống dưới giá trị này. 
2. Tính tổng số kẹo tối đa là$N \cdot R$. Điều này thể hiện trường hợp mọi đứa trẻ đều nhận được số tiền tối đa được phép và không có phân phối hợp lệ nào có thể vượt quá giá trị này. 
3. Kiểm tra xem$K$nằm trong khoảng này. Nếu như$N \cdot L \le K \le N \cdot R$, thì tồn tại một phân phối hợp lệ; nếu không thì không. 
4. Đầu ra`'S'`nếu khả thi, nếu không thì xuất ra`'N'`. 

Lý do đằng sau bước 3 là vì khoản đóng góp của mỗi đứa trẻ có thể tăng hoặc giảm một cách độc lập trong cùng một giới hạn nên tổng số tiền không bị chia thành các khoảng trống. Thay vào đó, nó tạo thành một phạm vi liền kề của các số nguyên có thể đạt được. 

### Tại sao nó hoạt động 

Bất biến chính là sau khi xử lý bất kỳ tập con con nào, tổng có thể đạt được sẽ tạo thành một khoảng liên tục. Ban đầu, không có con nào, số tiền duy nhất có thể đạt được là$0$. Việc thêm một con sẽ mở rộng mọi số tiền có thể đạt được bằng cách thêm một giá trị vào$[L, R]$, dịch chuyển và kết hợp các khoảng nhưng không bao giờ tạo ra khoảng trống. Lặp đi lặp lại điều này$N$thời gian bảo toàn tính liên tục, do đó tập tổng cuối cùng chính xác là tất cả các số nguyên nằm giữa$N \cdot L$Và$N \cdot R$. Vì vậy, việc kiểm tra tư cách thành viên trong khoảng thời gian này vừa cần thiết vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, K, L, R = map(int, input().split())
    
    min_sum = N * L
    max_sum = N * R
    
    if min_sum <= K <= max_sum:
        print('S')
    else:
        print('N')

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp tuân theo các giới hạn dẫn xuất. Tính toán duy nhất cần thiết là hai phép nhân và so sánh. Việc sử dụng số nguyên chính xác tùy ý của Python sẽ tránh mọi lo ngại về tràn ngay cả khi đạt đến giá trị trung gian$10^8$hoặc cao hơn. 

Logic quyết định được đặt ở cuối để phản ánh rõ ràng điều kiện toán học. Không có cấu trúc dữ liệu hoặc vòng lặp bổ sung được yêu cầu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1 2 2 4
```Đây$N = 1$, như vậy trẻ phải nhận được từ 2 đến 4 viên kẹo. Tổng số có thể có chính xác là {2, 3, 4}. mục tiêu$K = 2$nằm bên trong bộ này. 

| Bước | tổng tối thiểu | max_sum | K | Quyết định | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 2 | 4 | 2 | Kiểm tra phạm vi | 
| Kiểm tra | 2 2 2 4 | đúng | S | | 

Điều này xác nhận trường hợp đơn giản nhất trong đó một biến duy nhất bị chặn trực tiếp. 

### Mẫu 2 

đầu vào:```
2 1 6 10
```Mỗi đứa trẻ phải nhận được ít nhất 6 viên kẹo, vì vậy tổng số kẹo tối thiểu là 12. Tối đa là 20. Mục tiêu là$K = 1$, thấp hơn nhiều so với phạm vi có thể đạt được. 

| Bước | tổng tối thiểu | max_sum | K | Quyết định | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 12 | 20 | 1 | Kiểm tra phạm vi | 
| Kiểm tra | 12  1  20 | sai | N | | 

Điều này cho thấy trường hợp tràn rõ ràng trong đó ngay cả phép gán hợp lệ nhỏ nhất cũng đã vượt quá tổng số yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ các phép tính và so sánh số học không đổi mới được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp này phù hợp một cách tầm thường với tất cả các ràng buộc vì nó không thực hiện lặp lại$N$, mặc dù$N$có thể lớn như$10^3$. Việc tính toán hoàn toàn là số học và không phụ thuộc vào kích thước đầu vào ngoài việc đọc các giá trị. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    N, K, L, R = map(int, input().split())
    min_sum = N * L
    max_sum = N * R
    return 'S' if min_sum <= K <= max_sum else 'N'

# provided samples
assert run("1 2 2 4") == "S", "sample 1"
assert run("2 1 6 10") == "N", "sample 2"
assert run("3 9 3 3") == "S", "sample 3"

# custom cases
assert run("5 0 0 0") == "S", "all zero distribution"
assert run("4 10 3 3") == "N", "fixed sum mismatch"
assert run("10 50 1 10") == "S", "wide feasible range"
assert run("10 9 1 1") == "N", "below minimum total"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 0 0 0 | S | trường hợp cạnh không giới hạn | 
| 4 10 3 3 | N | phân bổ cố định không khả thi | 
| 10 50 1 10 | S | khoảng khả thi rộng | 
| 10 9 1 1 | N | dưới ranh giới tối thiểu | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi$L = R$, điều này buộc tất cả trẻ em phải nhận được số kẹo giống nhau. Đối với đầu vào:```
3 9 3 3
```thuật toán tính toán$min\_sum = 9$Và$max\_sum = 9$. Từ$K = 9$, điều kiện đúng và kết quả là`'S'`. Bất kỳ sai lệch nào so với 9 sẽ ngay lập tức thất bại vì khoảng thời gian thu gọn lại thành một điểm duy nhất. 

Một trường hợp cạnh khác là khi$L = 0$, cho phép phân phối linh hoạt:```
5 7 0 3
```Đây$min\_sum = 0$,$max\_sum = 15$. Vì 7 nằm trong khoảng này nên thuật toán sẽ đưa ra kết quả chính xác`'S'`. Điểm mấu chốt là mặc dù từng đứa trẻ có thể nhận được 0, nhưng giới hạn trên vẫn chỉ hạn chế tính khả thi thông qua tổng mức tối đa. 

Trường hợp cạnh thứ ba là khi$K$là cực kỳ lớn nhưng vẫn nằm trong giới hạn của số học số nguyên:```
1000 1000000 0 1000
```Số tiền tối đa có thể chính xác là$1{,}000{,}000$, vậy câu trả lời là`'S'`. Thuật toán xử lý việc này mà không bị tràn hoặc viết hoa đặc biệt vì số nguyên Python chia tỷ lệ một cách tự nhiên.
