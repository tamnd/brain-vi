---
title: "CF 102535K - Kim Able và gia đình Mooks"
description: "Dòng kẻ thù có thể được xem như một mảng có độ dài n. Mỗi vị trí là kẻ thù đang hoạt động, được viết là MOOK hoặc kẻ thù không hoạt động, được viết là MEEK. Kim luôn bắt đầu từ đầu bên trái và đi bộ cho đến khi tiếp cận kẻ thù hoạt động đầu tiên."
date: "2026-08-05T15:23:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102535
codeforces_index: "K"
codeforces_contest_name: "2020 UP ACM Algolympics Elimination Round"
rating: 0
weight: 102535
solve_time_s: 71
verified: true
draft: false
---

[CF 102535K - Kim Able và Mooks](https://codeforces.com/problemset/problem/102535/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Dòng kẻ thù có thể được xem như một mảng có độ dài`n`. Mỗi vị trí là một kẻ thù tích cực, được viết là`MOOK`, hoặc kẻ thù không hoạt động, được viết là`MEEK`. Kim luôn bắt đầu từ đầu bên trái và đi bộ cho đến khi tiếp cận kẻ thù hoạt động đầu tiên. Đánh bại kẻ thù đó tốn một phút, sau đó vị trí đó sẽ không hoạt động và mọi vị trí không hoạt động trước khi nó hoạt động trở lại. 

Nhiệm vụ là tính tổng số phút cho đến khi mọi vị trí trở thành`MEEK`. 

Những hạn chế là nhỏ về mặt`n`, với`n`nhiều nhất là 50, nhưng có thể có nhiều trường hợp thử nghiệm, lên tới 10.000. Một mô phỏng mất thời gian tỷ lệ thuận với số lượng hoạt động vẫn có thể nguy hiểm vì số lượng hoạt động có thể tăng theo cấp số nhân với`n`. Một dòng vị trí 50 có thể biểu thị các giá trị xung quanh`2^50`, vì vậy bất kỳ cách tiếp cận nào thực hiện mọi cuộc chiến theo đúng nghĩa đen đều không thể kết thúc. Lời giải phải tìm ra một mô hình toán học thay vì mô phỏng các trận chiến. 

Những trường hợp rắc rối xuất phát từ việc phòng tuyến không chỉ mất đi một kẻ địch sau mỗi trận chiến. Ví dụ: một vị thế đã không hoạt động có thể hoạt động trở lại. 

Coi như:```
1
MOOK
```Câu trả lời là`1`. Một giải pháp chỉ đếm số lượng ban đầu của`MOOK`các vị trí sẽ hoạt động ở đây, nhưng nó không thành công trong các trường hợp lớn hơn vì kẻ thù có thể quay trở lại. 

Một ví dụ khác là:```
3
MOOK
MEEK
MEEK
```Câu trả lời cũng là`1`. Sau khi đánh bại vị trí đầu tiên, không còn kẻ thù nào còn hoạt động. Một mô phỏng bất cẩn mong đợi mọi bản gốc`MEEK`yêu cầu một số công việc sẽ quá đáng. 

Một ví dụ rõ ràng hơn là:```
3
MEEK
MOOK
MEEK
```Câu trả lời là`2`. Trận chiến đầu tiên thay đổi dòng thành:```
MOOK
MEEK
MEEK
```Cuộc chiến thứ hai kết thúc nó. Kẻ thù ở giữa khiến kẻ thù ở phía trước xuất hiện trở lại, đó là hành vi trung tâm của vấn đề. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ giữ mảng hiện tại, tìm mảng đầu tiên`MOOK`, thay đổi nó thành một`MEEK`, lật tất cả trước đó`MEEK`vị trí vào`MOOK`và lặp lại cho đến khi mảng chỉ chứa`MEEK`. Điều này tuân theo đúng quy trình nên nó chính xác. 

Vấn đề là số lần lặp lại. Quá trình này thực sự đang đếm ngược thông qua một số nhị phân, do đó số lượng trận chiến có thể lớn bằng`2^n - 1`. Với`n = 50`, trường hợp xấu nhất sẽ yêu cầu nhiều hơn một triệu triệu phép tính. Ngay cả một mô phỏng rất hiệu quả cũng không thể xử lý được điều đó. 

Quan sát hữu ích là mỗi vị trí hoạt động chính xác như một chữ số nhị phân. Cho phép`MOOK`đại diện`1`Và`MEEK`đại diện`0`. Vị trí đầu tiên là bit ít quan trọng nhất. Khi Kim đánh bại người đầu tiên`MOOK`, cô ấy tìm thấy cái đầu tiên`1`bit và thay đổi nó thành`0`, trong khi tất cả trước đó`0`bit trở thành`1`. Đây chính xác là cách hoạt động của phép trừ một số khỏi số nhị phân. 

Ví dụ, nhà nước```
MOOK
MEEK
MOOK
```đại diện cho chữ số nhị phân`101`khi đọc với phía bên trái là bit có ý nghĩa nhỏ nhất. Giá trị của nó là:```
1 * 2^0 + 0 * 2^1 + 1 * 2^2 = 5
```Quá trình thực hiện năm trận chiến trước khi đạt đến con số 0. 

Toàn bộ vấn đề giảm xuống việc chuyển đổi cách sắp xếp ban đầu thành số nhị phân trong đó vị trí ngoài cùng bên trái có trọng số`2^0`, sau đó xuất ra giá trị đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc dòng từ trái sang phải và xử lý mọi`MOOK`dưới dạng chữ số nhị phân`1`và mọi`MEEK`dưới dạng chữ số nhị phân`0`. 

Kẻ địch ngoài cùng bên trái là kẻ ít đáng chú ý nhất vì đây là vị trí đầu tiên Kim có thể đánh bại. 
2. Duy trì giá trị nhị phân hiện tại trong khi quét dòng. Đối với vị trí tại chỉ mục`i`, thêm vào`2^i`nếu nó chứa một`MOOK`. 

Mỗi kẻ thù đang hoạt động đóng góp chính xác số lượng trận chiến được biểu thị bằng trọng số nhị phân của nó. 
3. In giá trị tích lũy. 

Giá trị có thể đạt`2^50 - 1`, vừa vặn thoải mái bên trong kiểu số nguyên của Python. 

Tại sao nó hoạt động: 

Điều bất biến là hàng kẻ thù hiện tại thể hiện số trận chiến vẫn cần thiết trước khi quá trình kết thúc. Một cuộc chiến duy nhất thực hiện phép biến đổi chính xác bằng cách trừ đi một phần tử khỏi biểu diễn nhị phân, trong đó vị trí ngoài cùng bên trái là bit ít quan trọng nhất. Vì quá trình dừng khi số nhị phân đạt đến 0 nên giá trị nhị phân bắt đầu chính xác là số lần chiến đấu cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        value = 0

        for i in range(n):
            s = input().strip()
            if s == "MOOK":
                value += 1 << i

        ans.append(str(value))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Chương trình xử lý từng trường hợp thử nghiệm một cách độc lập. Biến`value`lưu trữ số nhị phân được biểu thị bằng dòng. 

Hoạt động chuyển đổi`1 << i`tạo ra sức nặng của vị trí. Vì dòng đầu vào đầu tiên có chỉ mục`0`trong mã, kẻ thù đầu tiên đóng góp`2^0`, phù hợp với quá trình mà kẻ thù đầu tiên là kẻ đầu tiên mà Kim có thể tiếp cận. 

Giải pháp này không bao giờ mô phỏng các cuộc chiến, vì vậy nó tránh được số lần thay đổi trạng thái theo cấp số nhân. Số nguyên Python cũng tránh được sự cố tràn vì giá trị lớn nhất có thể nằm dưới`2^50`. 

## Ví dụ đã hoạt động 

Đối với trường hợp mẫu:```
MOOK
MEEK
MEEK
```giá trị nhị phân được tính như sau: 

| Chỉ mục | Tiểu bang | Đóng góp | Giá trị hiện tại | 
| --- | --- | --- | --- | 
| 0 | MOOK | 2^0 = 1 | 1 | 
| 1 | MEEK | 0 | 1 | 
| 2 | MEEK | 0 | 1 | 

Câu trả lời là`1`. Điều này xác nhận trường hợp kẻ thù đầu tiên đã là cuộc chiến bắt buộc duy nhất. 

Vì:```
MOOK
MEEK
MEEK
MOOK
MEEK
MOOK
MEEK
```những đóng góp là: 

| Chỉ mục | Tiểu bang | Đóng góp | Giá trị hiện tại | 
| --- | --- | --- | --- | 
| 0 | MOOK | 1 | 1 | 
| 1 | MEEK | 0 | 1 | 
| 2 | MEEK | 0 | 1 | 
| 3 | MOOK | 8 | 9 | 
| 4 | MEEK | 0 | 9 | 
| 5 | MOOK | 32 | 41 | 
| 6 | MEEK | 0 | 41 | 

Đầu ra là`41`. Dấu vết cho thấy các nhóm kẻ thù riêng biệt không độc lập vì mỗi vị trí đóng góp một trọng số nhị phân. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi vị trí của kẻ thù được đọc một lần | 
| Không gian | O(1) | Chỉ có câu trả lời tích lũy được lưu trữ | 

Tổng số công việc tối đa là 500.000 lượt đọc vị trí trên tất cả các trường hợp thử nghiệm, dễ dàng đáp ứng các giới hạn. 

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
    input = sys.stdin.readline
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        value = 0
        for i in range(n):
            if input().strip() == "MOOK":
                value += 1 << i
        ans.append(str(value))

    print("\n".join(ans))

assert run("""3
1
MOOK
3
MOOK
MEEK
MEEK
7
MOOK
MEEK
MEEK
MOOK
MEEK
MOOK
MEEK
""") == "1\n1\n41\n"

assert run("""1
1
MEEK
""") == "0\n"

assert run("""1
4
MOOK
MOOK
MOOK
MOOK
""") == "15\n"

assert run("""1
3
MEEK
MOOK
MEEK
""") == "2\n"

assert run("""1
50
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
MOOK
""") == str((1 << 50) - 1) + "\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đơn`MEEK`|`0`| Trạng thái đã hoàn thành | 
| bốn`MOOK`giá trị |`15`| Tất cả các bit được thiết lập và chuyển đổi nhị phân | 
|`MEEK, MOOK, MEEK`|`2`| Một bit ở giữa khác không | 
| Năm mươi`MOOK`giá trị |`2^50 - 1`| Kích thước tối đa và xử lý số nguyên lớn | 

## Vỏ cạnh 

Đối với đầu vào:```
1
MOOK
```thuật toán chỉ định vị trí duy nhất có trọng số là`2^0`, cho`1`. Điều này phù hợp với cuộc chiến duy nhất cần thiết. 

Đối với đầu vào:```
3
MOOK
MEEK
MEEK
```thuật toán bỏ qua các vị trí không hoạt động và trả về`1`. Quá trình kết thúc ngay sau thất bại đầu tiên vì không có bit nhị phân cao hơn nào được đặt. 

Đối với đầu vào:```
3
MEEK
MOOK
MEEK
```vị trí thứ hai góp phần`2^1`, cho`2`. Trận chiến đầu tiên kích hoạt vị trí đầu tiên và trận chiến thứ hai sẽ loại bỏ nó. Điều này xác nhận rằng các vị trí không hoạt động ban đầu không chỉ đơn giản là khoảng trống, chúng là các chữ số nhị phân có giá trị bằng 0. 

Đối với trường hợp tối đa trong đó tất cả 50 vị trí đều`MOOK`, câu trả lời là:```
1 + 2 + 4 + ... + 2^49 = 2^50 - 1
```Thuật toán xử lý việc này một cách trực tiếp mà không thực hiện bất kỳ trận chiến riêng lẻ nào.
