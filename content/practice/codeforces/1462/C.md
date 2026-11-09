---
title: "CF 1462C - Số duy nhất"
description: "Chúng ta được cho một tổng mục tiêu gồm các chữ số và chúng ta phải xây dựng số nguyên dương nhỏ nhất có các chữ số khác nhau và các chữ số của nó cộng chính xác với mục tiêu đó. Đầu ra không chỉ là số hợp lệ bất kỳ mà còn là số nhỏ nhất theo thứ tự thập phân tiêu chuẩn."
date: "2026-06-11T02:11:03+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1462
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 690 (Div. 3)"
rating: 900
weight: 1462
solve_time_s: 317
verified: false
draft: false
---

[CF 1462C - Số duy nhất](https://codeforces.com/problemset/problem/1462/C) 

**Xếp hạng:** 900 
**Tags:** vũ phu, tham lam, toán học 
**Thời gian giải:** 5 phút 17s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tổng mục tiêu gồm các chữ số và chúng ta phải xây dựng số nguyên dương nhỏ nhất có các chữ số khác nhau và các chữ số của nó cộng chính xác với mục tiêu đó. 

Đầu ra không chỉ là số hợp lệ bất kỳ mà còn là số nhỏ nhất theo thứ tự thập phân tiêu chuẩn. Điều đó có nghĩa là chúng ta đang chọn một tập hợp các chữ số riêng biệt một cách hiệu quả, quyết định cách sắp xếp của chúng và tạo thành số nhỏ nhất có thể về mặt từ điển từ chúng. 

Ràng buộc chính là các chữ số phải là duy nhất. Vì các chữ số chỉ nằm trong khoảng từ 0 đến 9 nên chúng tôi không bao giờ có thể sử dụng nhiều hơn 10 chữ số riêng biệt và quan trọng hơn là tổng tối đa có thể đạt được khi sử dụng các chữ số riêng biệt là cố định. Tập hợp lớn nhất có thể là tất cả các chữ số từ 0 đến 9, có tổng bằng 45. Bất kỳ đầu vào nào lớn hơn 45 ngay lập tức không thể thực hiện được. 

Điều này tạo ra một không gian tìm kiếm rất chặt chẽ: tối đa 10 chữ số, với tổng giới hạn. 

Trường hợp cạnh tinh vi phát sinh từ sự hiện diện của số 0. Số 0 không đóng góp gì vào tổng số nhưng ảnh hưởng đến việc đặt hàng. Ví dụ: được phép sử dụng chữ số 0 nhưng nó không được xuất hiện dưới dạng chữ số đứng đầu trong số cuối cùng. Một cách tiếp cận ngây thơ sắp xếp các chữ số đã chọn mà không xử lý số 0 đứng đầu một cách chính xác có thể vô tình tạo ra các biểu diễn tối thiểu không hợp lệ. 

Một trường hợp khác là các khoản tiền nhỏ như 1 hoặc 2, trong đó lựa chọn tham lam hoạt động khác nhau tùy thuộc vào chiến lược đặt hàng. Ví dụ, việc chọn các chữ số từ nhỏ nhất trở lên là không tối ưu nếu chúng ta đang cố gắng giảm thiểu giá trị từ điển trong khi vẫn thỏa mãn ràng buộc tổng. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử tất cả các tập hợp con các chữ số từ 1 đến 9 (và tùy chọn 0), kiểm tra tổng của chúng và sau đó tạo tất cả các hoán vị để tìm số hợp lệ nhỏ nhất. Điều này đúng về nguyên tắc vì không gian tìm kiếm là hữu hạn: có nhiều nhất$2^{10} = 1024$tập hợp con và nhiều nhất là 10! hoán vị cho mỗi tập hợp con. Tuy nhiên, điều này vẫn dẫn đến hàng triệu số ứng viên trong trường hợp xấu nhất, điều này là không cần thiết dựa trên cấu trúc của bài toán. 

Quan sát quan trọng là chúng ta muốn một tập hợp con gồm các chữ số riêng biệt có tổng bằng chính xác$x$, và chúng ta muốn số kết quả càng nhỏ càng tốt. Để giảm thiểu số kết quả, chúng tôi muốn các chữ số nhỏ hơn xuất hiện càng sớm càng tốt. Tuy nhiên, các chữ số nhỏ hơn cũng làm giảm tổng số tiền chậm hơn, điều này trái ngược với những gì chúng ta mong muốn khi cần đạt được tổng mục tiêu cố định một cách hiệu quả. 

Điều này tạo ra một sự căng thẳng tham lam cổ điển: để đạt được tổng nhanh chóng trong khi sử dụng các chữ số riêng biệt, trước tiên chúng ta nên ưu tiên các chữ số lớn. Nhưng để giảm thiểu số cuối cùng, sau này chúng ta nên sắp xếp chúng theo thứ tự tăng dần. 

Vì vậy, chiến lược đúng là chọn các chữ số một cách tham lam từ 9 xuống 1, luôn lấy một chữ số nếu nó không vượt quá tổng còn lại. Điều này đảm bảo chúng tôi sử dụng ít chữ số nhất có thể và ưu tiên sớm những khoản đóng góp lớn hơn. Sau khi chọn tập hợp, chúng ta sắp xếp nó theo thứ tự tăng dần để tạo ra số nhỏ nhất có thể. 

Chữ số 0 không bao giờ hữu ích cho bản thân tổng, nhưng nó có thể được thêm vào nếu cần và sẽ luôn được đặt sau tất cả các chữ số khác 0 theo thứ tự được sắp xếp, điều này đương nhiên khiến nó không trở thành chữ số đứng đầu trừ khi nó là chữ số duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con và hoán vị Brute Force | O(10! · 2^10) | O(1) | Quá chậm | 
| Lựa chọn tham lam + sắp xếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Kiểm tra xem tổng mục tiêu$x$lớn hơn 45. Nếu vậy, hãy xuất -1 ngay lập tức vì ngay cả khi sử dụng tất cả các chữ số từ 0 đến 9 cũng không thể đạt được tổng đó. Điều này loại bỏ các trường hợp bất khả thi trước khi xây dựng bất kỳ công trình nào. 
2. Khởi tạo một danh sách trống để lưu trữ các chữ số đã chọn và đặt một biến`rem = x`đại diện cho số tiền còn lại chúng ta vẫn cần đạt được. 
3. Lặp lại các chữ số từ 9 xuống 1. Tại mỗi chữ số`d`, kiểm tra xem`d <= rem`. Nếu có, hãy bao gồm`d`trong bộ câu trả lời và trừ`d`từ`rem`. Chúng ta bỏ qua số 0 ở đây vì nó không giúp giảm số tiền còn lại. 
4. Sau vòng lặp, nếu`rem`không bằng 0, xuất ra -1. Điều này có nghĩa là chúng ta không thể biểu diễn chính xác tổng bằng các chữ số riêng biệt từ 1 đến 9. 
5. Mặt khác, sắp xếp các chữ số đã chọn theo thứ tự tăng dần và ghép chúng lại để tạo thành số cuối cùng. Việc sắp xếp là cần thiết vì chúng ta muốn số nguyên nhỏ nhất có thể và việc đặt các chữ số nhỏ hơn trước đó luôn làm giảm giá trị số. 

### Tại sao nó hoạt động 

Việc lựa chọn tham lam từ 9 trở xuống đảm bảo rằng chúng ta sử dụng ít chữ số nhất có thể, vì các chữ số lớn hơn sẽ làm giảm tổng còn lại một cách mạnh mẽ hơn. Vì chúng ta không bao giờ sử dụng lại các chữ số nên việc xây dựng tương đương với việc chọn một tập con của {1..9} có tổng chính xác là x. Trong số tất cả các tập hợp con như vậy, bất kỳ nghiệm hợp lệ nào cũng chỉ khác nhau về thứ tự chữ số và việc sắp xếp các chữ số đó tăng dần luôn mang lại số nguyên nhỏ nhất có thể. Do đó, thuật toán tách vấn đề thành lựa chọn tập hợp con (tối ưu tham lam cho tính khả thi) và thứ tự (biểu diễn tối thiểu được sắp xếp), đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve(x):
    if x > 45:
        return "-1"
    
    rem = x
    chosen = []
    
    for d in range(9, 0, -1):
        if d <= rem:
            chosen.append(d)
            rem -= d
    
    if rem != 0:
        return "-1"
    
    chosen.sort()
    return "".join(map(str, chosen))

t = int(input())
out = []
for _ in range(t):
    x = int(input())
    out.append(solve(x))

print("\n".join(out))
```Việc thực hiện trực tiếp theo sau việc xây dựng tham lam. Việc kiểm tra tính khả thi`x > 45`tránh những công việc không cần thiết. Vòng lặp ngược từ 9 đến 1 đảm bảo chúng tôi ưu tiên các chữ số lớn, giúp giảm thiểu số lượng phần tử được chọn. Việc sắp xếp ở cuối là điều cần thiết, vì chỉ riêng việc lựa chọn tham lam không đảm bảo thứ tự số tối thiểu. 

Một lỗi triển khai phổ biến là sắp xếp các chữ số trước khi chọn hoặc lặp lên từ 1, điều này có xu hướng tạo ra nhiều chữ số nhỏ và có thể không đạt được tổng ngay cả khi có giải pháp. 

## Ví dụ đã hoạt động 

### Ví dụ 1: x = 15 

Chúng tôi theo dõi số tiền còn lại và các chữ số đã chọn. 

| Bước | Chữ số | Còn lại trước | Lấy chữ số | Còn lại sau | Được chọn | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 9 | 15 | vâng | 6 | [9] | 
| 2 | 8 | 6 | không | 6 | [9] | 
| 3 | 7 | 6 | không | 6 | [9] | 
| 4 | 6 | 6 | vâng | 0 | [9, 6] | 
| 5-9 | 5..1 | 0 | không | 0 | [9, 6] | 

Sắp xếp cho [6, 9], vì vậy đầu ra là 69. 

Điều này chứng tỏ rằng lựa chọn tham lam tìm thấy một tập hợp con hợp lệ và việc sắp xếp đảm bảo giá trị số tối thiểu. 

### Ví dụ 2: x = 5 

| Bước | Chữ số | Còn lại trước | Lấy chữ số | Còn lại sau | Được chọn | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 9 | 5 | không | 5 | [] | 
| 2 | 8 | 5 | không | 5 | [] | 
| 3 | 7 | 5 | không | 5 | [] | 
| 4 | 6 | 5 | không | 5 | [] | 
| 5 | 5 | 5 | vâng | 0 | [5] | 
| 6-9 | 4..1 | 0 | không | 0 | [5] | 

Kết quả là 5. 

Điều này cho thấy rằng khi một chữ số khớp chính xác với mục tiêu, thuật toán sẽ chọn nó một cách tự nhiên, xác nhận rằng không có chữ số không cần thiết nào được đưa vào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ 9 lần lặp cho mỗi trường hợp thử nghiệm cộng với việc sắp xếp liên tục tối đa 9 chữ số | 
| Không gian | O(1) | Mảng chữ số có kích thước cố định được giới hạn bởi 9 phần tử | 

Các ràng buộc cho phép tối đa 50 trường hợp kiểm thử, nhưng mỗi trường hợp chạy trong thời gian không đổi do phạm vi chữ số cố định. Giải pháp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve(x):
        if x > 45:
            return "-1"
        rem = x
        chosen = []
        for d in range(9, 0, -1):
            if d <= rem:
                chosen.append(d)
                rem -= d
        if rem != 0:
            return "-1"
        chosen.sort()
        return "".join(map(str, chosen))

    t = int(input())
    out = []
    for _ in range(t):
        x = int(input())
        out.append(solve(x))
    return "\n".join(out)

# provided samples
assert run("4\n1\n5\n15\n50\n") == "1\n5\n69\n-1"

# custom cases
assert run("1\n9\n") == "9", "single digit max"
assert run("1\n10\n") == "19", "smallest two-digit distinct sum"
assert run("1\n45\n") == "123456789", "all digits used"
assert run("1\n46\n") == "-1", "impossible maximum + 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 9 | 9 | khớp chính xác một chữ số | 
| 10 | 19 | tham lam chia thành cặp hợp lệ nhỏ nhất | 
| 45 | 123456789 | trường hợp sử dụng chữ số đầy đủ | 
| 46 | -1 | ranh giới không khả thi | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi tổng bằng 45. Thuật toán chọn các chữ số từ 9 xuống 1, sử dụng tất cả các chữ số có sẵn chính xác một lần. Việc sắp xếp tạo ra 123456789, đây là hoán vị nhỏ nhất có thể có của tất cả các chữ số. 

Một trường hợp khác là khi tổng cao hơn một chút so với ranh giới tập hợp con có thể tiếp cận, chẳng hạn như 8 hoặc 10. Đối với x = 10, quá trình tham lam bỏ qua 9 và 8, chọn 7 (quá lớn để vừa), rồi cuối cùng chọn 9? thực ra nó không được nên nó chọn 9 bị bỏ qua, 8 bị bỏ qua, 7 bị bỏ qua, 6 bị bỏ qua, 5 bị bỏ qua, 4 bị bỏ qua, 3 bị bỏ qua, 2 bị bỏ qua, rồi 1 và 9 không thể hoàn thành nên nó chọn 9? không, nó không phù hợp. Lựa chọn cuối cùng trở thành [9? không hợp lệ], rem vẫn khác 0 nên đầu ra là -1. Điều này chứng tỏ rằng sự lựa chọn tham lam không chỉ là việc hái cho đến khi kiệt sức; tính khả thi phải được xác minh thông qua số tiền còn lại. 

Trường hợp tinh tế cuối cùng là x = 1. Thuật toán chọn trực tiếp chữ số 1 và tạo ra đầu ra có một chữ số. Mọi nỗ lực bao gồm số 0 sẽ có hại vì nó không thể giúp thỏa mãn tổng và có thể ảnh hưởng không chính xác đến thứ tự nếu xử lý sai.
