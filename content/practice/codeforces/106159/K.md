---
title: "CF 106159K - Kronos"
description: "Chúng ta được cung cấp một tập hợp các dấu thời gian biểu thị thời điểm tin nhắn đến từ tương lai. Mỗi dấu thời gian chỉ là một số nguyên duy nhất trên một dòng số rất lớn."
date: "2026-06-20T02:28:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106159
codeforces_index: "K"
codeforces_contest_name: "XIII UnB Contest Mirror"
rating: 0
weight: 106159
solve_time_s: 50
verified: true
draft: false
---

[CF 106159K - Kronos](https://codeforces.com/problemset/problem/106159/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các dấu thời gian biểu thị thời điểm tin nhắn đến từ tương lai. Mỗi dấu thời gian chỉ là một số nguyên duy nhất trên một dòng số rất lớn. Sau khi đọc tất cả tin nhắn, chúng tôi được yêu cầu trả lời nhiều truy vấn, trong đó mỗi truy vấn đưa ra một khoảng thời gian đóng$[L, R]$và chúng ta phải đếm xem có bao nhiêu dấu thời gian được lưu trữ nằm trong khoảng thời gian đó. 

Do đó, đầu vào là tĩnh: đầu tiên là một danh sách giá trị cố định, sau đó là một loạt truy vấn đếm phạm vi trên cùng danh sách đó. Không có cập nhật nào xảy ra và mọi truy vấn đều độc lập. Mỗi truy vấn sẽ hỏi một câu hỏi tần số trên một phạm vi con của dòng số nguyên. 

Các ràng buộc đủ lớn để bất kỳ phương pháp nào quét toàn bộ danh sách cho mỗi truy vấn đều ngay lập tức quá chậm. Với$N, Q \le 2 \cdot 10^5$, ngây thơ$O(NQ)$giải pháp sẽ thực hiện lên đến$4 \cdot 10^{10}$so sánh trong trường hợp xấu nhất, vượt xa những gì có thể chạy trong 4 giây bằng Python hoặc thậm chí C++ trong giới hạn thông thường. Điều này thúc đẩy chúng ta tiến tới xử lý tiền xử lý và truy vấn logarit. 

Một điểm tinh tế là dấu thời gian là tùy ý trong$[1, 10^9]$, vì vậy chúng tôi không thể dựa vào chỉ mục trực tiếp hoặc mảng tần số. Miền quá lớn để xây dựng biểu đồ trực tiếp. 

Các trường hợp cạnh chủ yếu xoay quanh việc xử lý ranh giới. Ví dụ: nếu tất cả các dấu thời gian giống hệt nhau và một truy vấn khớp chính xác hoặc thiếu giá trị đó thì độ chính xác phụ thuộc hoàn toàn vào logic khoảng bao gồm. 

Hãy xem xét: 

đầu vào:```
3 1
5 5 5
5 5
```Đầu ra phải là:```
3
```Một sai lầm ngây thơ là coi các khoảng là nửa mở hoặc xử lý sai đẳng thức, sẽ trả về 0 hoặc 1 không chính xác. 

Một trường hợp cạnh khác:```
4 1
1 3 5 7
2 4
```Đầu ra đúng là 1. Bất kỳ sai lệch nào trong so sánh ranh giới hoặc chiến lược đếm không chính xác sẽ bị hỏng ở đây. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi truy vấn$[L, R]$, chúng tôi lặp lại tất cả$N$dấu thời gian và đếm xem có bao nhiêu nằm trong phạm vi. Điều này đúng vì nó khớp trực tiếp với định nghĩa của truy vấn. Tuy nhiên, nó lặp lại quá trình quét toàn bộ tương tự cho mọi truy vấn. Với$Q$truy vấn, điều này dẫn đến$N \cdot Q$so sánh. 

Khi cả hai$N$Và$Q$lớn, điều này trở thành nút cổ chai. Ngay cả khi mỗi lần so sánh có thời gian không đổi thì tổng công việc vẫn quá lớn. 

Quan sát quan trọng là chúng ta không cần giữ nguyên thứ tự ban đầu của dấu thời gian. Truy vấn chỉ phụ thuộc vào tư cách thành viên trong phạm vi giá trị chứ không phụ thuộc vào vị trí. Khi chúng tôi sắp xếp dấu thời gian, mọi truy vấn sẽ giảm xuống việc tìm xem có bao nhiêu phần tử nằm giữa hai vị trí trong một mảng được sắp xếp. 

Sau khi sắp xếp, tất cả các phần tử$\le R$tạo thành một tiền tố và tất cả các phần tử$< L$tạo thành một tiền tố khác. Câu trả lời trở thành sự khác biệt giữa hai số tiền tố. Đây chính xác là những gì tìm kiếm nhị phân cung cấp: chúng ta có thể xác định ranh giới của$R$và ranh giới của$L$theo thời gian logarit. 

Vì vậy, thay vì quét mảng, chúng tôi sắp xếp trước một lần, sau đó trả lời từng truy vấn bằng hai tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(NQ)$|$O(1)$| Quá chậm | 
| Sắp xếp + Tìm kiếm nhị phân |$O(N \log N + Q \log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi vấn đề thành vấn đề thứ tự được sắp xếp trong đó việc tính phạm vi trở thành tính ranh giới tiền tố. 

1. Đọc tất cả dấu thời gian vào một mảng. Chúng tôi lưu trữ chúng vì chúng tôi sẽ cần sắp xếp chúng để tổ chức cấu trúc dữ liệu. Việc sắp xếp là cần thiết vì nó biến các giá trị tùy ý thành một chuỗi đơn điệu khi áp dụng tìm kiếm nhị phân. 
2. Sắp xếp mảng theo thứ tự không giảm. Bước này là bước chuyển đổi quan trọng: sau khi được sắp xếp, tất cả các giá trị nhỏ hơn hoặc bằng ngưỡng sẽ tạo thành một tiền tố liền kề, cho phép chúng ta thay thế việc đếm bằng việc tìm ranh giới. 
3. Đối với mỗi truy vấn$[L, R]$, tính hai vị trí trong mảng được sắp xếp: chỉ mục đầu tiên có giá trị lớn hơn hoặc bằng$L$và chỉ mục đầu tiên có giá trị lớn hơn$R$. Hai ranh giới này cách ly chính xác phân khúc hợp lệ. 
4. Câu trả lời cho câu hỏi là sự khác biệt giữa hai vị trí này. Điều này hiệu quả vì mọi thứ trước ranh giới bên trái quá nhỏ, mọi thứ sau ranh giới bên phải quá lớn và mọi thứ ở giữa đều hợp lệ. 
5. Xuất ngay mỗi sai số tính toán hoặc lưu trữ kết quả cho đầu ra cuối cùng. 

### Tại sao nó hoạt động 

Việc sắp xếp chuyển đổi vấn đề từ việc đếm thành viên tùy ý sang trích xuất độ dài khoảng trên một chuỗi đơn điệu. Mỗi truy vấn đều yêu cầu kích thước của giao điểm giữa mảng được sắp xếp và một khoảng một cách hiệu quả. Ranh giới tìm kiếm nhị phân là các điểm cắt chính xác để phân chia mảng thành ba vùng riêng biệt: các giá trị bên dưới$L$, giá trị bên trong$[L, R]$và các giá trị trên$R$. Bởi vì các vùng này tiếp giáp nhau theo thứ tự được sắp xếp nên kích thước của chúng có thể được tính toán mà không cần quét các phần tử và không có phần tử nào bị tính hai lần hoặc bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from bisect import bisect_left, bisect_right

def main():
    n, q = map(int, input().split())
    arr = list(map(int, input().split()))
    arr.sort()

    out = []
    for _ in range(q):
        l, r = map(int, input().split())
        left = bisect_left(arr, l)
        right = bisect_right(arr, r)
        out.append(str(right - left))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Giải pháp bắt đầu bằng cách đọc và sắp xếp dấu thời gian. Việc sắp xếp được thực hiện một lần, điều này đảm bảo rằng tất cả các hoạt động trong tương lai có thể dựa vào cấu trúc tìm kiếm nhị phân. 

Mỗi truy vấn sử dụng hai tìm kiếm nhị phân tiêu chuẩn.`bisect_left`tìm thấy vị trí đầu tiên ở đó`l`có thể được chèn vào mà không phá vỡ trật tự, đếm hiệu quả số lượng phần tử nhỏ hơn`l`.`bisect_right`tìm thấy vị trí đầu tiên lớn hơn`r`, đếm xem có bao nhiêu phần tử nhỏ hơn hoặc bằng`r`. Sự khác biệt của chúng cho biết chính xác số phần tử trong khoảng. 

Một cạm bẫy phổ biến là hoán đổi hai chức năng này hoặc chỉ sử dụng một trong số chúng. Một vấn đề tế nhị khác là quên rằng khoảng này bao gồm cả hai đầu, đó là lý do tại sao`bisect_right`là cần thiết hơn là cái khác`bisect_left`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3
10 1 10 7 5
1 5
6 10
2 4
```Mảng được sắp xếp là: 

[1, 5, 7, 10, 10] 

| Truy vấn | L | R | trái (bisect_left) | đúng (bisect_right) | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 5 | 0 | 2 | 2 | 
| 2 | 6 | 10 | 2 | 5 | 3 | 
| 3 | 2 | 4 | 1 | 1 | 0 | 

Đầu ra:```
2
3
0
```Dấu vết này cho thấy cách xử lý các bản sao một cách tự nhiên: cả hai lần xuất hiện 10 đều được tính trong truy vấn thứ hai vì chúng nằm trong ranh giới được xác định bởi`bisect_right`. 

### Ví dụ 2 

đầu vào:```
3 3
10 15 20
1 5
10 10
15 25
```Mảng được sắp xếp là: 

[10, 15, 20] 

| Truy vấn | L | R | trái | đúng | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 5 | 0 | 0 | 0 | 
| 2 | 10 | 10 | 0 | 1 | 1 | 
| 3 | 15 | 25 | 1 | 3 | 2 | 

Ví dụ này nêu bật độ chính xác của ranh giới. Truy vấn đối sánh chính xác`[10,10]`chỉ cô lập chính xác phần tử đầu tiên bằng 10. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N + Q \log N)$| việc sắp xếp chiếm ưu thế ban đầu, mỗi truy vấn sử dụng hai tìm kiếm nhị phân | 
| Không gian |$O(N)$| lưu trữ dấu thời gian | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các phần tử và truy vấn, do đó việc xử lý truy vấn logarit giữ cho tổng số hoạt động luôn nằm trong giới hạn. Việc sử dụng bộ nhớ là tuyến tính ở kích thước đầu vào và vừa vặn ở mức 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import bisect

    n, q = map(int, input().split())
    arr = list(map(int, input().split()))
    arr.sort()

    out = []
    for _ in range(q):
        l, r = map(int, input().split())
        from bisect import bisect_left, bisect_right
        out.append(str(bisect_right(arr, r) - bisect_left(arr, l)))

    return "\n".join(out) + "\n"

# provided samples
assert run("""5 3
10 1 10 7 5
1 5
6 10
2 4
""") == "2\n3\n0\n"

assert run("""3 3
10 15 20
1 5
10 10
15 25
""") == "0\n1\n2\n"

# custom cases
assert run("""1 3
5
5 5
4 4
6 6
""") == "1\n0\n0\n"

assert run("""4 2
1 1 1 1
1 1
2 2
""") == "4\n0\n"

assert run("""6 2
100 200 300 400 500 600
150 450
1 1000
""") == "2\n6\n"

assert run("""5 2
5 4 3 2 1
3 3
1 5
""") == "1\n5\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn lặp lại | 1,0,0 | độ chính xác ranh giới và xử lý phần tử đơn | 
| tất cả các giá trị bằng nhau | 4,0 | bảo hiểm đầy đủ và loại trừ | 
| truy vấn phạm vi giá trị cách nhau | 2,6 | độ chính xác tầm trung | 
| nhập thứ tự ngược lại | 1,5 | phân loại độc lập | 

## Vỏ cạnh 

Đối với một giá trị lặp lại, mảng được sắp xếp sẽ thu gọn tất cả cấu trúc thành các phần tử giống hệt nhau. Thuật toán vẫn hoạt động vì`bisect_left`Và`bisect_right`khung chính xác toàn bộ khối trùng lặp. Ví dụ, với đầu vào`[5,5,5]`và truy vấn`[5,5]`, cả hai ranh giới đều cô lập các chỉ số`[0,3)`, thu được 3. 

Đối với các truy vấn hoàn toàn nằm ngoài phạm vi, chẳng hạn như`[4,4]`TRÊN`[1,2,3,4,5]`, cả hai tìm kiếm nhị phân đều trả về cùng một chỉ mục, tạo ra số 0. Điều này xác nhận rằng các khoảng trống được xử lý tự nhiên mà không cần vỏ đặc biệt. 

Đối với các truy vấn đầy đủ như`[1,10^9]`,`bisect_left`trả về 0 và`bisect_right`trả lại$N$, đưa ra số đếm đầy đủ. Điều này cho thấy thuật toán xử lý chính xác các ranh giới cực đoan mà không bị tràn hoặc thiếu phần tử.
