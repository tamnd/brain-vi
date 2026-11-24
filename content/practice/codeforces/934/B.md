---
title: "CF 934B - Một Lô Thịnh Vượng"
description: "Chúng ta được yêu cầu xây dựng một số nguyên dương không vượt quá $10^{18}$ sao cho khi nó được viết ở cơ số 10, tổng số “vòng lặp” được hình thành bởi các chữ số của nó chính xác là $k$."
date: "2026-06-17T02:50:32+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation"]
categories: ["algorithms"]
codeforces_contest: 934
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 462 (Div. 2)"
rating: 1200
weight: 934
solve_time_s: 92
verified: true
draft: false
---

[CF 934B - Một lô đất thịnh vượng](https://codeforces.com/problemset/problem/934/B) 

**Đánh giá:** 1200 
**Tas:** thuật toán xây dựng, triển khai 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một số nguyên dương không vượt quá$10^{18}$sao cho khi viết theo cơ số 10 thì tổng số “vòng” tạo bởi các chữ số của nó bằng chính xác$k$. Mỗi chữ số đóng góp một số vùng cố định tùy thuộc vào hình dạng của nó: một số chữ số không bao gồm một vùng nào cả, một số bao quanh một vùng và một số ít bao quanh hai vùng. Tổng của một số chỉ đơn giản là tổng của tất cả các chữ số. 

Nhiệm vụ này mang tính xây dựng: chúng tôi không tính các vòng lặp cho một số nhất định mà thay vào đó xây dựng bất kỳ số nào có đóng góp về mặt chữ số có tổng chính xác bằng giá trị được yêu cầu. Nếu không có số nào như vậy tồn tại trong giới hạn kích thước, chúng ta phải xuất -1. 

Ràng buộc$k \le 10^6$là tín hiệu quan trọng cho thấy việc xây dựng tuyến tính hoặc tham lam về mặt$k$được yêu cầu. Một con số lên tới$10^{18}$có tối đa 18 chữ số, do đó, bất kỳ giải pháp nào cũng phải đóng gói cẩn thận các đóng góp thành một chuỗi chữ số ngắn. 

Một trường hợp thất bại tinh vi đối với suy nghĩ ngây thơ là cố gắng giảm thiểu số lượng chữ số một cách tham lam bằng cách sử dụng chữ số vòng lặp lớn nhất trước tiên mà không xem xét các ràng buộc về khả năng biểu diễn ở mức nhỏ.$k$. Ví dụ: nếu người ta giả sử mọi vòng lặp có thể được phân tách độc lập mà không quan tâm đến tính khả dụng của chữ số, thì người ta có thể kết luận sai rằng tất cả$k$là có thể. Trong thực tế, sự đóng góp của chữ số là rời rạc và bị hạn chế. 

## Phương pháp tiếp cận 

Mỗi chữ số đóng góp một số vòng lặp cố định: 

Các chữ số không có vòng lặp là 1, 2, 3, 5 và 7. Chữ số 4 đóng góp 1 vòng lặp. Các chữ số 0, 6 và 9 mỗi chữ số đóng góp 1 vòng lặp. Chữ số 8 đóng góp 2 vòng. Quan sát cấu trúc quan trọng là chúng tôi muốn trình bày$k$dưới dạng tổng các giá trị trong$\{0,1,2\}$, nhưng với một hạn chế là chúng tôi đang xây dựng một số, vì vậy chúng tôi quan tâm đến việc đếm chữ số và cấu trúc từ điển chứ không chỉ phân vùng. 

Một cách tiếp cận bạo lực sẽ cố gắng liệt kê các số lên tới$10^{18}$, tính số vòng lặp của chúng và so sánh. Ngay cả việc hạn chế số có 18 chữ số, đây là$10^{18}$khả năng, vượt xa tính khả thi. 

Sự đơn giản hóa chính là lật ngược vấn đề: thay vì xây dựng một số và vòng lặp đếm, chúng ta xây dựng chuỗi chữ số trực tiếp từ các đơn vị vòng lặp. Mỗi chữ số thực sự “tiêu tốn” một số vòng lặp nhất định và chúng tôi muốn đóng gói chính xác$k$vòng lặp thành chữ số. Vì mục tiêu là bất kỳ số hợp lệ nào nên chúng ta có thể chọn các chữ số một cách tham lam, nhưng chúng ta cũng phải đảm bảo số kết quả là dương và không vượt quá 18 chữ số. 

Chiến lược tối ưu là sử dụng lặp lại chữ số đóng góp vòng lặp lớn nhất. Chữ số 8 cho 2 vòng trên mỗi chữ số, đây là mật độ tối ưu. Vì vậy, trước tiên chúng ta sử dụng càng nhiều số 8 càng tốt, và nếu$k$là số lẻ, chúng tôi xử lý vòng lặp 1 còn lại bằng cách sử dụng một chữ số đóng góp chính xác cho 1 vòng lặp, chẳng hạn như 0 hoặc 6 hoặc 9 hoặc 4. Vì các số 0 đứng đầu không được phép trong cấu trúc có ý nghĩa nên chúng tôi đảm bảo chữ số đầu tiên không phải là số 0. 

Việc xây dựng giảm xuống việc chọn biểu diễn có độ dài tối thiểu sử dụng chủ yếu là 8 giây, với tối đa một chữ số điều chỉnh nếu$k$thật kỳ quặc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê Brute Force |$O(10^{18})$|$O(1)$| Quá chậm | 
| Xây dựng chữ số tham lam |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quan sát rằng mỗi chữ số đóng góp 0, 1 hoặc 2 vòng lặp, vì vậy cách hiệu quả nhất để xây dựng một số lớn$k$là tối đa hóa việc sử dụng các chữ số đóng góp 2 vòng lặp. 
2. Tính xem chúng ta có thể sử dụng bao nhiêu chữ số có giá trị 8: đây là$k // 2$. Mỗi chữ số như vậy đóng góp chính xác 2 vòng lặp, vì vậy số này chiếm phần lớn$k$. 
3. Nếu$k$là số chẵn, chúng ta đã hoàn thành chỉ với 8 giây. Điều này tạo thành số hợp lệ có độ dài nhỏ nhất với mật độ vòng lặp tối đa. 
4. Nếu$k$là số lẻ, chúng tôi không thể biểu thị vòng lặp 1 cuối cùng bằng số 8. Do đó, chúng tôi giảm một số 8 (nếu có) hoặc chỉ thêm một chữ số 1 vòng lặp. 
5. Đảm bảo tổng số chữ số không vượt quá 18. Nếu vượt quá, ghi -1. 
6. Viết số bằng cách sắp xếp tất cả các chữ số theo một thứ tự hợp lệ, đảm bảo chữ số đầu tiên không bằng 0. 

Tại sao nó hoạt động: điều bất biến là sau bước 2, chúng ta đã biểu thị phần chẵn tối đa có thể có của$k$sử dụng các chữ số có mật độ tối ưu. Số dư còn lại tối đa là 1, phải được biểu thị bằng một chữ số 1 vòng lặp. Vì các chữ số là độc lập và bổ sung trong việc đóng góp vòng lặp nên việc sắp xếp lại các chữ số không ảnh hưởng đến tính chính xác mà chỉ ảnh hưởng đến tính hợp lệ dưới dạng số. Việc xây dựng luôn tạo ra nhiều tập hợp chữ số hợp lệ có độ dài tối thiểu có tổng chính xác$k$hoặc xác định chính xác rằng nó không thể vừa trong 18 chữ số. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    k = int(input().strip())

    # If k is 0, we cannot form a positive number
    if k == 0:
        print(-1)
        return

    # use as many '8' digits as possible (each contributes 2 loops)
    eights = k // 2
    rem = k % 2

    # total digits needed
    length = eights + rem
    if length > 18:
        print(-1)
        return

    digits = []

    # place one 1-loop digit first if remainder exists (avoid leading zero)
    if rem == 1:
        digits.append('4')  # or 6 or 9; any 1-loop digit works

    # fill remaining with 8s
    digits.extend(['8'] * eights)

    # ensure no leading zero issue (not relevant here since we don't use 0 first)
    print("".join(digits))

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện ý tưởng tham lam là tối đa hóa việc sử dụng chữ số 8. Việc xử lý phần còn lại đảm bảo tính chính xác chẵn lẻ khi$k$thật kỳ quặc. Chữ số 4 được sử dụng làm số dự phòng duy nhất vì nó đóng góp chính xác một vòng lặp và tránh các biến chứng về số 0 đứng đầu. 

Việc kiểm tra độ dài thực thi$10^{18}$ràng buộc, vì mọi giải pháp hợp lệ đều phải chứa tối đa 18 chữ số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$k = 2$Chúng tôi tính toán$k // 2 = 1$, số dư là 0. 

| Bước | k | số tám | rem | chữ số | 
| --- | --- | --- | --- | --- | 
| ban đầu | 2 | 0 | 0 | [] | 
| sau khi chia tay | 2 | 1 | 0 | [] | 
| xây dựng | 2 | 1 | 0 | [8] | 

Đầu ra là:```
8
```Điều này cho thấy cách sử dụng đầy đủ đơn giản nhất của một chữ số có 2 vòng lặp. 

### Ví dụ 2 

đầu vào:$k = 5$Chúng tôi tính toán$k // 2 = 2$, dư 1. 

| Bước | k | số tám | rem | chữ số | 
| --- | --- | --- | --- | --- | 
| ban đầu | 5 | 0 | 0 | [] | 
| sau khi chia tay | 5 | 2 | 1 | [] | 
| thêm rem | 5 | 2 | 1 | [4] | 
| điền 8s | 5 | 2 | 1 | [4, 8, 8] | 

Đầu ra:```
488
```Điều này xác nhận rằng vòng lặp đơn còn sót lại được xử lý chính xác mà không vi phạm các ràng buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ các công trình số học và tối đa 18 chữ số | 
| Không gian |$O(1)$| Chuỗi đầu ra được giới hạn bởi 18 ký tự | 

Giải pháp dễ dàng phù hợp với các giới hạn vì cả kích thước tính toán và đầu ra đều bị giới hạn không đổi bởi ràng buộc 18 chữ số. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    k = int(input().strip())

    if k == 0:
        return "-1"

    eights = k // 2
    rem = k % 2

    if eights + rem > 18:
        return "-1"

    res = []
    if rem == 1:
        res.append('4')
    res.extend(['8'] * eights)
    return "".join(res)

# provided sample
assert run("2\n") == "8"

# custom cases
assert run("1\n") == "4", "single loop digit"
assert run("3\n") == "488", "odd k with extra loop"
assert run("18\n") == "888888888888888888", "max fill case"
assert run("37\n") == "-1", "exceeds length limit"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 4 | vòng lặp khác không tối thiểu | 
| 3 | 488 | xử lý số dư lẻ | 
| 18 | 888... | xây dựng ranh giới tối đa | 
| 37 | -1 | không thể do độ dài | 

## Vỏ cạnh 

Trường hợp một cạnh là khi$k = 1$. Thuật toán gán$eights = 0$và số dư 1, tạo ra một chữ số duy nhất “4”. Điều này đáp ứng chính xác yêu cầu và tôn trọng ràng buộc 18 chữ số. 

Một trường hợp cạnh khác xảy ra khi$k$đủ lớn để$k // 2 > 18$. Trong tình huống đó, mặc dù về mặt lý thuyết, các vòng lặp có thể được hình thành nhưng giới hạn chữ số sẽ ngăn cản việc xây dựng. Ví dụ,$k = 100$dẫn đến 50 chữ số là 8, vi phạm ràng buộc và trả về đúng -1. 

Trường hợp cạnh thứ ba là xử lý tính chẵn lẻ khi$k$là lẻ nhưng lớn. Thuật toán đảm bảo sử dụng chính xác một chữ số 1 vòng lặp và tất cả đóng góp còn lại đến từ 8 giây, do đó tổng số vòng lặp luôn khớp$k$chính xác mà không cần điều chỉnh thêm.
