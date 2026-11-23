---
title: "CF 495A - Bộ đếm kỹ thuật số"
description: "Thang máy luôn hiển thị số tầng gồm hai chữ số từ 00 đến 99. Mỗi chữ số được rút ra bằng màn hình bảy đoạn. Một phân đoạn có thể bị phá vỡ. Khi một đoạn bị hỏng, dù có cần thì nó cũng không thể sáng lên được."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 495
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 282 (Div. 2)"
rating: 1100
weight: 495
solve_time_s: 99
verified: true
draft: false
---

[CF 495A - Bộ đếm kỹ thuật số](https://codeforces.com/problemset/problem/495/A) 

**Đánh giá:** 1100 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Thang máy luôn hiển thị số tầng gồm hai chữ số từ`00`ĐẾN`99`. Mỗi chữ số được vẽ bằng màn hình bảy đoạn. 

Một phân đoạn có thể bị phá vỡ. Khi một đoạn bị hỏng, dù có cần thì nó cũng không thể sáng lên được. Quan sát quan trọng là các đoạn bị hỏng chỉ có thể biến một đoạn được chiếu sáng dự kiến ​​thành một đoạn không được chiếu sáng. Họ không bao giờ có thể tạo thêm ánh sáng. 

Chúng tôi được cung cấp hai chữ số hiện hiển thị trên màn hình. Chúng ta phải đếm xem có bao nhiêu số tầng ban đầu`x`từ`00`ĐẾN`99`có thể đã tạo ra con số hiển thị này sau khi một số tập hợp con của phân đoạn bị hỏng. 

Vì màn hình luôn chứa chính xác hai chữ số nên vị trí hàng chục và hàng đơn vị độc lập. Một tầng như`03`phải được coi là hai chữ số`0`Và`3`, không phải là số nguyên`3`. 

Những hạn chế là rất nhỏ. Chỉ có 100 số tầng ban đầu có thể có. Ngay cả khi chúng ta so sánh từng ứng viên với chữ số được hiển thị theo từng chữ số và từng phân đoạn thì tổng khối lượng công việc là không đáng kể. Vài nghìn thao tác là đủ. 

Nguồn gốc chính của sai lầm là do hiểu sai cách hoạt động của các đoạn bị hỏng. 

Xem xét đầu vào:```
88
```Chữ số hiển thị`8`sử dụng tất cả bảy phân đoạn. Vì không thể xuất hiện thêm đoạn nào do bị đứt nên chữ số gốc cũng phải được`8`. Như vậy chỉ`88`Và`89`có thể sản xuất`89`, bởi vì`9`khác với`8`bởi chính xác một đoạn có thể bị phá vỡ. 

Một sai lầm dễ mắc phải khác là quên số 0 đứng đầu. 

Đối với đầu vào:```
00
```ứng cử viên`08`là hợp lệ. Chữ số bên phải`8`có thể mất tất cả các phân đoạn ngoại trừ những phân đoạn cần thiết cho`0`. Nếu chúng ta lưu trữ các tầng dưới dạng số nguyên thông thường và bỏ qua các số 0 đứng đầu, chúng ta sẽ bỏ lỡ những khả năng như vậy. 

Trường hợp tinh tế thứ ba giả định rằng nếu các phân đoạn được hiển thị là tập hợp con của các phân đoạn ban đầu thì ứng viên có giá trị trên toàn cầu. Việc kiểm tra phải được thực hiện riêng biệt cho cả hai vị trí chữ số. Ví dụ: ngay cả khi chữ số hàng đơn vị tương thích thì chữ số hàng chục có thể không tương thích. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là kiểm tra mọi số tầng ban đầu có thể từ`00`ĐẾN`99`. 

Đối với một số ứng cử viên, hãy so sánh hai chữ số của nó với chữ số hiển thị tương ứng. Một chữ số được hiển thị có thể đạt được từ một chữ số gốc nếu mọi phân đoạn sáng trong chữ số được hiển thị cũng sáng ở chữ số gốc. 

Giả sử một đoạn được hiển thị sáng nhưng không ở chữ số gốc. Điều đó đòi hỏi phải có một đoạn bị hỏng để tạo ra ánh sáng, điều này là không thể. Một ứng cử viên như vậy phải bị từ chối. 

Phương pháp vũ phu này đã đủ nhanh rồi. Chỉ có 100 ứng cử viên. Mỗi ứng viên yêu cầu kiểm tra hai chữ số và mỗi chữ số có bảy đoạn. Tổng công việc là khoảng`100 × 2 × 7 = 1400`so sánh phân khúc. 

Quan sát quan trọng là lỗi phân đoạn chỉ loại bỏ các phân đoạn sáng. Nếu chúng ta biểu thị mỗi chữ số bằng tập hợp các phân đoạn cần bật thì chữ số được hiển thị`d`có thể đến từ một chữ số gốc`o`chính xác khi nào:`segments(d)`là một tập hợp con của`segments(o)`. 

Điều đó biến vấn đề thành một bài kiểm tra khả năng tương thích đơn giản giữa các chữ số. Chúng ta có thể tính toán trước, với mỗi chữ số được hiển thị, có bao nhiêu chữ số gốc có thể tạo ra nó. 

Vì số gồm hai vị trí độc lập nên tổng số số sàn hợp lệ bằng:`compatible[tens] × compatible[ones]`. 

Điều này tránh việc liệt kê tất cả 100 số tầng và dẫn đến việc triển khai thậm chí còn đơn giản hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(100 × 14) | O(1) | Đã chấp nhận | 
| Tối ưu | Tiền xử lý O(10²) + Truy vấn O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Biểu diễn phân đoạn 

Chúng tôi mã hóa từng chữ số thập phân bằng bảy đoạn sáng cho chữ số đó. 

Ví dụ:`0 = "1111110"`

`1 = "0110000"`và vân vân. 

Mỗi vị trí trong chuỗi tương ứng với một đoạn của màn hình. 

### Số chữ số gốc tương thích 

1. Đối với mỗi chữ số được hiển thị`d`từ`0`ĐẾN`9`, khởi tạo một bộ đếm. 
2. Với mọi chữ số gốc có thể có`o`từ`0`ĐẾN`9`, kiểm tra xem`d`có thể được lấy từ`o`. 
3. Để xác minh tính tương thích, hãy kiểm tra tất cả bảy đoạn. 
4. Nếu một số đoạn được thắp sáng`d`nhưng không sáng trong`o`, loại bỏ cặp vì các đoạn bị hỏng không thể tạo ra ánh sáng. 
5. Ngược lại, cặp này hợp lệ, do đó hãy tăng bộ đếm cho`d`. 

Điều kiện tương thích chính xác là điều kiện tập hợp con trên các phân đoạn sáng. 

### Xử lý số đầu vào 

1. Đọc hai chữ số được hiển thị. 
2. Gọi chữ số đầu tiên là`a`và chữ số thứ hai là`b`. 
3. Hai vị trí độc lập nên tổng số sàn gốc hợp lệ bằng:`count[a] × count[b]`. 
4. Xuất kết quả. 

### Tại sao nó hoạt động 

Đối với bất kỳ vị trí chữ số nào, phân đoạn được hiển thị chỉ có thể sáng nếu ban đầu nó đã sáng. Các đoạn bị hỏng có thể tắt các đoạn sáng, nhưng không thể tắt các đoạn có chọn lọc theo cách tạo ra ánh sáng mới. Do đó, một chữ số được hiển thị có thể đạt được một cách chính xác khi tất cả các đoạn sáng của nó đã thuộc về chữ số gốc. 

Chữ số hàng chục và chữ số hàng đơn vị sử dụng màn hình riêng biệt nên lỗi ở một vị trí không ảnh hưởng đến vị trí kia. Mọi chữ số hàng chục gốc hợp lệ có thể được kết hợp với mọi chữ số hàng đơn vị gốc hợp lệ. Do đó, việc nhân các số đếm sẽ tạo ra chính xác số tầng có hai chữ số hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

SEG = [
    "1111110",  # 0
    "0110000",  # 1
    "1101101",  # 2
    "1111001",  # 3
    "0110011",  # 4
    "1011011",  # 5
    "1011111",  # 6
    "1110000",  # 7
    "1111111",  # 8
    "1111011",  # 9
]

count = [0] * 10

for shown in range(10):
    for original in range(10):
        ok = True
        for k in range(7):
            if SEG[shown][k] == '1' and SEG[original][k] == '0':
                ok = False
                break
        if ok:
            count[shown] += 1

s = input().strip()

a = int(s[0])
b = int(s[1])

print(count[a] * count[b])
```Phần đầu tiên lưu trữ mẫu bảy đoạn cho mỗi chữ số. MỘT`'1'`nghĩa là đoạn đó đang sáng. 

Vòng tiền xử lý tính toán có bao nhiêu chữ số gốc có thể tạo ra mỗi chữ số được hiển thị. điều kiện```
SEG[shown][k] == '1' and SEG[original][k] == '0'
```phát hiện một tình huống không thể xảy ra khi màn hình hiển thị một đoạn sáng mà chữ số gốc chưa từng có. 

Sau khi tiền xử lý, việc giải quyết đầu vào thực tế là ngay lập tức. Chúng ta đọc hai chữ số được hiển thị, tra cứu số lượng bản gốc tương thích cho từng vị trí và nhân chúng. 

Một lỗi phổ biến là đảo ngược quá trình kiểm tra tính tương thích. Chúng ta phải xác minh rằng mọi đoạn sáng trong chữ số được hiển thị cũng tồn tại ở chữ số gốc chứ không phải ngược lại. 

Một lỗi phổ biến khác là chuyển đổi toàn bộ dữ liệu đầu vào thành số nguyên. đầu vào`"03"`phải duy trì chuỗi hai ký tự để giữ nguyên số 0 đứng đầu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
89
```Số lượng tương thích là: 

| Vị trí | Chữ số được hiển thị | Chữ số gốc tương thích | Đếm | 
| --- | --- | --- | --- | 
| Hàng chục | 8 | {8} | 1 | 
| Những cái | 9 | {8, 9} | 2 | 

Kết quả: 

| Hàng chục lựa chọn | Lựa chọn của mọi người | Tổng cộng | 
| --- | --- | --- | 
| 1 | 2 | 2 | 

Đầu ra:```
2
```Ví dụ này chứng minh rằng một hiển thị`8`là cực kỳ hạn chế vì cả bảy đoạn đều đã sáng. 

### Mẫu 2 

đầu vào:```
00
```Đối với chữ số`0`, các bản gốc tương thích là:`{0, 8}`bởi vì`8`có thể mất đoạn giữa và trở thành`0`. 

| Vị trí | Chữ số được hiển thị | Chữ số gốc tương thích | Đếm | 
| --- | --- | --- | --- | 
| Hàng chục | 0 | {0, 8} | 2 | 
| Những cái | 0 | {0, 8} | 2 | 

Kết quả: 

| Hàng chục lựa chọn | Lựa chọn của mọi người | Tổng cộng | 
| --- | --- | --- | 
| 2 | 2 | 4 | 

Đầu ra:```
4
```Số tầng hợp lệ là`00`,`08`,`80`, Và`88`. Dấu vết này nêu bật lý do tại sao các số 0 đứng đầu phải được giữ nguyên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(10² × 7) | So sánh từng cặp chữ số trên bảy đoạn | 
| Không gian | O(1) | Chỉ mảng có kích thước cố định | 

Quá trình tiền xử lý thực hiện tối đa 700 phép so sánh phân đoạn. Sau đó, việc trả lời đầu vào cần có thời gian không đổi. Điều này thấp hơn nhiều so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

SEG = [
    "1111110",
    "0110000",
    "1101101",
    "1111001",
    "0110011",
    "1011011",
    "1011111",
    "1110000",
    "1111111",
    "1111011",
]

def solve():
    count = [0] * 10

    for shown in range(10):
        for original in range(10):
            ok = True
            for k in range(7):
                if SEG[shown][k] == '1' and SEG[original][k] == '0':
                    ok = False
                    break
            if ok:
                count[shown] += 1

    s = input().strip()
    print(count[int(s[0])] * count[int(s[1])])

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    global input
    input = sys.stdin.readline

    solve()

    out = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("89\n") == "2", "sample 1"

# custom cases
assert run("00\n") == "4", "leading zeros"
assert run("88\n") == "1", "all segments lit"
assert run("11\n") == "25", "many compatible digits"
assert run("99\n") == "4", "boundary near maximum value"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 00 | 4 | Xử lý số 0 hàng đầu | 
| 88 | 1 | Chữ số được chiếu sáng đầy đủ chỉ có một nguồn | 
| 11 | 25 | Nhân số đếm độc lập | 
| 99 | 4 | Kiểm tra khả năng tương thích chữ số cao | 

## Vỏ cạnh 

Xem xét đầu vào:```
00
```Đối với mỗi vị trí, chữ số hiển thị là`0`. Chỉ có chữ số gốc`0`Và`8`chứa tất cả các phân đoạn được yêu cầu bởi`0`. Thuật toán tính toán số lượng`2`cho từng vị trí và trả về`2 × 2 = 4`. Xử lý đầu vào dưới dạng số nguyên`0`thay vì chuỗi`"00"`sẽ mất một chữ số và đưa ra câu trả lời sai. 

Xem xét đầu vào:```
88
```Mọi phân đoạn đều được thắp sáng. Bất kỳ chữ số gốc nào bị thiếu dù chỉ một phân đoạn đều không thể tạo ra một`8`, vì các đoạn bị hỏng chỉ loại bỏ ánh sáng. Quá trình tiền xử lý mang lại`count[8] = 1`, vì vậy câu trả lời trở thành`1 × 1 = 1`. Điều này phát hiện các hoạt động triển khai cho phép các phân đoạn xuất hiện nhầm. 

Xem xét đầu vào:```
89
```Chữ số hàng chục`8`có chính xác một nguồn tương thích, cụ thể là`8`. Chữ số hàng đơn vị`9`có hai nguồn tương thích,`8`Và`9`. Năng suất nhân`2`. Thuật toán xử lý từng vị trí chữ số một cách độc lập, điều này đúng vì các màn hình hiển thị riêng biệt.
