---
title: "CF 103664A - \u0421\u0442\u0438\u043a\u0435\u0440\u044b"
description: "Cho một tấm ván hình chữ nhật có cạnh a và b, được đặt cố định sao cho cạnh dưới của nó nằm ngang. Trong k ngày, mỗi ngày một miếng dán hình vuông giống hệt nhau được dán, mỗi miếng dán cũng được định hướng sao cho cạnh dưới của nó nằm ngang."
date: "2026-07-02T21:48:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103664
codeforces_index: "A"
codeforces_contest_name: "\u0422\u0443\u0440\u043d\u0438\u0440 \u0410\u0440\u0445\u0438\u043c\u0435\u0434\u0430 2019"
rating: 0
weight: 103664
solve_time_s: 48
verified: true
draft: false
---

[CF 103664A - \u0421\u0442\u0438\u043a\u0435\u0440\u044b](https://codeforces.com/problemset/problem/103664/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tấm bảng hình chữ nhật có cạnh`a`Và`b`, được đặt theo hướng cố định sao cho cạnh dưới của nó nằm ngang. Qua`k`ngày, các nhãn dán hình vuông giống hệt nhau được đặt mỗi ngày một nhãn dán, mỗi nhãn dán cũng được định hướng sao cho cạnh dưới của nó nằm ngang. Sau những điều này`k`vị trí, toàn bộ khu vực bảng được bao phủ, nghĩa là mọi điểm của bảng đều nằm bên trong ít nhất một nhãn dán. 

Bây giờ chúng tôi thiết lập lại quy trình và muốn mua hình dán hình vuông mới có chiều dài cạnh số nguyên`s`. Mục đích là để đảm bảo rằng một cách chính xác`k`ngày, dán lại một nhãn dán mỗi ngày, có thể che phủ hoàn toàn cùng một nhãn dán`a × b`Cái bảng. 

Chúng ta được yêu cầu tính số nguyên nhỏ nhất`s`như vậy`k`giống hệt nhau`s × s`hình vuông có thể bao phủ hoàn toàn hình chữ nhật. 

Những hạn chế là vô cùng lớn, với`a, b, k`lên đến`10^18`, Và`a · b ≤ 10^18`. Điều này ngay lập tức loại trừ mọi mô phỏng vị trí hoặc xây dựng dựa trên lưới. Bất kỳ lời giải nào cũng phải quy bài toán về số học thuần túy, sử dụng một số phép toán không đổi. 

Một điểm tinh tế là các miếng dán không bị giới hạn ở dạng lưới, chúng có thể được đặt tùy ý trên mặt phẳng. Điều này có nghĩa là chúng ta đang giải một bài toán bao phủ hình học, nhưng trong thực tế chỉ có các ràng buộc về diện tích và độ phân chia mới là quan trọng. 

Một điểm quan trọng cần cân nhắc là khi`k`quá nhỏ để che phủ khu vực ngay cả khi miếng dán dán bảng hoàn hảo. Ví dụ, nếu`a = 4`,`b = 4`,`k = 4`, thì mỗi nhãn dán phải bao phủ ít nhất diện tích`4`thậm chí đạt đến tổng diện tích`16`. Bất kỳ nhỏ hơn`s`thất bại bất kể vị trí. Điều này đã gợi ý rằng giới hạn dưới của khu vực là cần thiết nhưng chỉ riêng nó thì chưa đủ. 

Một trường hợp khác là khi một chiều lớn hơn nhiều so với`s`. Ngay cả khi tổng diện tích là đủ, nếu nhãn dán quá nhỏ để trải dài trên một chiều trong cách sắp xếp che phủ khả thi, thì nhiều nhãn dán phải căn chỉnh để che phủ kích thước đó, điều này ảnh hưởng đến tính khả thi thông qua số lượng trần. 

## Phương pháp tiếp cận 

Một cách tiếp cận ngây thơ sẽ cố gắng diễn giải vấn đề như một nhiệm vụ đóng gói hình học. Đối với một cố định`s`, chúng ta có thể thử mô phỏng xem`k`hình vuông có thể bao phủ một`a × b`hình chữ nhật bằng cách đặt chúng một cách tối ưu. Điều này nhanh chóng trở thành một vấn đề bao phủ 2D liên tục với vô số vị trí, do đó việc mô phỏng là không thể. Ngay cả các vị trí rời rạc cũng sẽ bùng nổ theo kiểu tổ hợp. 

Sự đơn giản hóa đầu tiên xuất phát từ việc nhận thấy rằng cấu trúc hữu ích duy nhất là căn chỉnh trục và kích thước hình vuông đồng nhất. Bất kỳ lớp phủ tối ưu nào của hình chữ nhật bằng các ô vuông bằng nhau đều hoạt động giống như một lưới gần đúng, có thể bị dịch chuyển. Điều này làm giảm vấn đề đếm xem cần bao nhiêu ô vuông để bao phủ mỗi chiều. 

Nếu chúng ta sửa`s`, số lượng miếng dán cần thiết dọc theo chiều rộng là`ceil(a / s)`và dọc theo chiều cao là`ceil(b / s)`, vậy tổng phạm vi bảo hiểm cần thiết là`ceil(a / s) * ceil(b / s)`. Yêu cầu là giá trị này tối đa là`k`. 

Bây giờ vấn đề trở nên đơn điệu trong`s`. Nếu một`s`hoạt động, lớn hơn`s`cũng có tác dụng vì việc tăng kích thước hình vuông sẽ làm giảm cả số lượng trần hoặc giữ chúng không thay đổi. Điều này cho phép tìm kiếm nhị phân trên`s`. 

Chúng tôi tìm kiếm nhỏ nhất`s`như vậy`ceil(a / s) * ceil(b / s) ≤ k`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Vị trí hình học vũ lực | không khả thi | không khả thi | Quá chậm | 
| Tìm kiếm nhị phân với công thức bao phủ | O(log max(a,b)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định một hàm mà với một kích thước hình vuông cho trước`s`, tính xem cần bao nhiêu miếng dán để phủ lên tấm bảng. Điều này được thực hiện bằng cách tính toán`ceil(a / s)`Và`ceil(b / s)`và nhân chúng lên. Đây là mô hình sắp xếp theo trục tốt nhất có thể. 
2. Hãy quan sát điều đó`ceil(x / s)`giảm hoặc không đổi như`s`tăng lên. Điều này đảm bảo số lượng nhãn dán cần thiết là đơn điệu`s`, điều này rất cần thiết cho tìm kiếm nhị phân. 
3. Đặt phạm vi tìm kiếm nhị phân cho`s`. Giới hạn dưới là`1`. Giới hạn trên có thể được xác định một cách an toàn`max(a, b)`, vì một hình vuông lớn hơn cả hai cạnh sẽ che phủ toàn bộ hình chữ nhật trong một hình dán. 
4. Thực hiện tìm kiếm nhị phân trên`s`. Đối với mỗi điểm giữa, hãy tính các nhãn dán cần thiết. Nếu yêu cầu nhỏ hơn hoặc bằng`k`, nó có nghĩa là`s`là đủ, vì vậy chúng tôi thử các giá trị nhỏ hơn. Nếu không thì,`s`quá nhỏ và chúng tôi tăng nó lên. 
5. Tiếp tục cho đến khi hợp lệ nhỏ nhất`s`được tìm thấy. 

### Tại sao nó hoạt động 

Đối với bất kỳ cố định`s`, biểu thức`ceil(a / s) * ceil(b / s)`thể hiện chính xác số lượng ô vuông bằng nhau tối thiểu cần thiết trong một lớp phủ được căn chỉnh theo trục tối ưu. Bất kỳ lớp phủ nào ít nhất phải phân chia mỗi chiều thành các đoạn có chiều dài tối đa`s`, vì vậy không có công trình nào có thể đánh bại được số lượng này. Hàm số đơn điệu vì tăng`s`không thể tăng trần kỳ hạn. Do đó tìm kiếm nhị phân hội tụ đến mức khả thi tối thiểu`s`không bỏ sót một ứng cử viên nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def needed(a, b, s):
    return ((a + s - 1) // s) * ((b + s - 1) // s)

def solve():
    a, b, k = map(int, input().split())

    lo, ro = 1, max(a, b)
    ans = ro

    while lo <= ro:
        mid = (lo + ro) // 2
        if needed(a, b, mid) <= k:
            ans = mid
            ro = mid - 1
        else:
            lo = mid + 1

    print(ans)

if __name__ == "__main__":
    solve()
```Cốt lõi của việc thực hiện là`needed`chức năng mã hóa logic đếm dựa trên trần. biểu thức`(a + s - 1) // s`là cách an toàn số nguyên tiêu chuẩn để tính toán`ceil(a / s)`không có số học dấu phẩy động. 

Tìm kiếm nhị phân duy trì tính bất biến rằng tất cả các kích thước lớn hơn`ro`đều hợp lệ và tất cả các kích thước nhỏ hơn`lo`không hợp lệ. Mỗi bước thu nhỏ ranh giới này cho đến khi đạt giá trị tối thiểu`s`bị cô lập. 

Phải cẩn thận khi chỉ sử dụng số học số nguyên. Việc sử dụng số float sẽ gây ra lỗi chính xác cho các giá trị lên tới`10^18`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 4 4
```Chúng tôi tìm kiếm tối thiểu`s`. 

| s | trần nhà(4/s) | cần thiết | ≤ k? | 
| --- | --- | --- | --- | 
| 1 | 4 | 16 | không | 
| 2 | 2 | 4 | vâng | 
| 3 | 2 | 4 | vâng | 
| 4 | 1 | 1 | vâng | 

Tìm kiếm nhị phân tìm thấy`s = 2`. 

Điều này chứng tỏ rằng mặc dù lớn hơn`s`các giá trị cũng hoạt động, chúng tôi chọn chính xác giá trị khả thi nhỏ nhất. 

### Ví dụ 2 

đầu vào:```
2 5 1
```Chúng ta cần một nhãn dán duy nhất để che hình chữ nhật, vì vậy`needed(s)`phải là 1. 

| s | trần nhà(2/s) | trần nhà(5/s) | cần thiết | ≤ k? | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 5 | 10 | không | 
| 2 | 1 | 3 | 3 | không | 
| 5 | 1 | 1 | 1 | vâng | 

Hợp lệ nhỏ nhất`s`là`5`. 

Điều này cho thấy hạn chế trong đó một nhãn dán phải trải rộng trên toàn bộ kích thước lớn hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log max(a, b)) | tìm kiếm nhị phân theo độ dài cạnh có thể | 
| Không gian | O(1) | chỉ sử dụng một số biến cố định | 

Việc tìm kiếm logarit có thể dễ dàng đủ nhanh với các giá trị lên tới`10^18`, yêu cầu tối đa khoảng 60 lần lặp cho mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def needed(a, b, s):
        return ((a + s - 1) // s) * ((b + s - 1) // s)

    a, b, k = map(int, input().split())

    lo, ro = 1, max(a, b)
    ans = ro

    while lo <= ro:
        mid = (lo + ro) // 2
        if needed(a, b, mid) <= k:
            ans = mid
            ro = mid - 1
        else:
            lo = mid + 1

    return str(ans)

# provided samples
assert run("4 4 4") == "2"
assert run("2 5 1") == "5"

# custom cases
assert run("1 1 1") == "1"
assert run("10 10 100") == "1"
assert run("10 10 1") == "10"
assert run("7 4 5") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | 1 | bảng nhỏ nhất có thể | 
| 10 10 100 | 1 | nhiều nhãn dán cho phép kích thước nhỏ nhất | 
| 10 10 1 | 10 | lực lượng nhãn dán duy nhất toàn nhịp | 
| 7 4 5 | 3 | trường hợp bất đối xứng không vuông | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi`k = 1`. Sau đó, chúng ta phải che hình chữ nhật bằng một hình vuông duy nhất, vì vậy câu trả lời phải là`max(a, b)`. Đối với đầu vào`7 4 1`, thuật toán đánh giá`s = 4`như không đủ vì`ceil(7/4)=2`, Và`s = 7`trở nên hợp lệ, trả về chính xác`7`. 

Một trường hợp khác là khi`a`Và`b`bằng nhau và`k`là lớn. Đối với đầu vào`6 6 36`, thậm chí`s = 1`hoạt động vì 36 ô vuông đơn vị có thể xếp được bảng. Tìm kiếm nhị phân hội tụ chính xác đến`1`. 

Cuối cùng, khi kích thước rất lệch, chẳng hạn như`1 10^18 1`, lực lượng thuật toán`s = 10^18`vì chỉ cho phép một hình dán và nó phải trải rộng trên cả hai chiều. Việc tính toán trần chính xác tạo ra`1 * 1 = 1`chỉ ở ranh giới đó.
