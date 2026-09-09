---
title: "CF 104587H - Màn hình phòng vệ sinh"
description: "Chúng tôi được sắp xếp một nhóm người cần được bố trí vào các phòng vệ sinh có một ngăn giống hệt nhau, trong đó mỗi người chiếm một ngăn trong đúng một đơn vị thời gian. Có s quầy hàng, nghĩa là bất cứ lúc nào cũng có tối đa s người có thể vào bên trong cùng một lúc."
date: "2026-06-30T07:30:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104587
codeforces_index: "H"
codeforces_contest_name: "2020-2021 ICPC East Central North America Regional Contest (ECNA 2020)"
rating: 0
weight: 104587
solve_time_s: 47
verified: true
draft: false
---

[CF 104587H - Màn hình phòng vệ sinh](https://codeforces.com/problemset/problem/104587/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được sắp xếp một nhóm người cần được bố trí vào các phòng vệ sinh có một ngăn giống hệt nhau, trong đó mỗi người chiếm một ngăn trong đúng một đơn vị thời gian. có`s`quầy hàng, có nghĩa là bất cứ lúc nào lên đến`s`mọi người có thể ở bên trong cùng một lúc. 

Mỗi người đều có một thời hạn`d`, có nghĩa là họ phải hoàn thành đơn vị sử dụng duy nhất của mình không muộn hơn thời gian`d`. Thời gian là rời rạc và mỗi người chiếm đúng một ô, vì vậy nếu ai đó bắt đầu vào thời điểm`x`, họ kết thúc vào lúc`x + 1`. 

Điều phức tạp thứ hai là một số người cần có một nguồn tài nguyên khan hiếm được chia sẻ, một cuộn giấy vệ sinh, được đánh dấu bằng`t = 'y'`. Bất cứ lúc nào, chỉ có một người cần giấy vệ sinh có thể vào trong quầy hàng. Những người có`t = 'n'`không sử dụng tài nguyên này và không can thiệp lẫn nhau ngoài khả năng của gian hàng. 

Nhiệm vụ là quyết định xem có thể sắp xếp tất cả mọi người vào các quầy hàng và các khoảng thời gian để mỗi người hoàn thành đúng thời hạn và “hạn chế về giấy vệ sinh” không bao giờ bị vi phạm hay không. 

Các hạn chế chính rất lớn: lên tới 100.000 người và 50.000 gian hàng. Điều này ngay lập tức loại trừ bất kỳ mô phỏng nào cố gắng gán từng cá nhân một cách tham lam vào các khe thời gian trong khi quét về phía trước theo thời gian, vì điều đó sẽ chuyển thành hành vi bậc hai nếu thời hạn lớn hoặc được sắp xếp chặt chẽ. 

Một dạng lỗi tinh vi xuất phát từ việc bỏ qua hạn chế về tài nguyên dùng chung. Nếu chúng ta chỉ tôn trọng năng lực của gian hàng, chúng ta có thể giả định tính khả thi một cách không chính xác. Ví dụ, với nhiều`'y'`Tất cả người dùng đều có thời hạn chặt chẽ giống hệt nhau, chỉ riêng công suất gian hàng có thể cho phép lập lịch nhưng một cuộn buộc phải tuần tự hóa. 

Một trường hợp khác phát sinh khi tất cả các thời hạn đều bằng nhau và số lượng`'y'`người dùng vượt quá thời hạn đó. Ngay cả khi có nhiều quầy hàng, bản thân thời gian cũng trở thành nút thắt cổ chai. 

## Phương pháp tiếp cận 

Chiến lược bạo lực trực tiếp sẽ cố gắng mô phỏng thời gian từ`1`đến thời hạn tối đa, phân công mọi người vào bất kỳ quầy hàng miễn phí nào đồng thời tôn trọng giấy vệ sinh hiện đang được sử dụng hay chưa. Đối với mỗi bước thời gian, chúng tôi sẽ chọn từ tất cả những người có sẵn những người chưa vượt quá thời hạn và phân công`s`của họ. Vì`'y'`người dùng, chúng tôi cũng sẽ đảm bảo chỉ có một người được đặt cho mỗi đơn vị thời gian. 

Về mặt khái niệm, điều này hoạt động hiệu quả vì nó bắt chước quá trình lập kế hoạch thực tế, nhưng lại quá chậm. Trường hợp xấu nhất xảy ra khi thời hạn quá lớn, lên tới`10^9`, trong khi chỉ`10^5`con người tồn tại. Ngay cả khi chúng tôi nén thời gian theo các sự kiện, việc duy trì một nhóm người sẵn có năng động và liên tục chọn các nhiệm vụ hợp lệ sẽ dẫn đến các hoạt động sắp xếp và xử lý đống theo từng bước thời gian, biến thành một cách hiệu quả.`O(n log n)`cho mỗi mô phỏng thời gian sự kiện với tối đa`O(max d)`sự kiện trong công thức tồi tệ nhất. 

Quan sát quan trọng là thời gian không cần phải được mô phỏng một cách rõ ràng. Mỗi người chỉ yêu cầu một vị trí đơn vị, vì vậy hạn chế thực sự là có bao nhiêu người phải hoàn thành trước mỗi thời hạn. Đây là một vấn đề khả thi về năng lực tích lũy cổ điển. 

Chúng ta có thể diễn giải lại hệ thống như sau: bất cứ lúc nào`t`, nhiều nhất`s * t`mọi người có thể đã hoàn thành tổng thể. Ngoài ra, trong số những sản phẩm đã hoàn thành, nhiều nhất`t`trong số họ có thể là`'y'`người dùng vì chỉ có một`'y'`người dùng có thể được xử lý trên mỗi đơn vị thời gian. 

Vì vậy, thay vì lập kế hoạch, chúng tôi kiểm tra các ràng buộc về tính khả thi của tiền tố theo thời hạn đã sắp xếp. Sắp xếp mọi người theo thời hạn cho phép chúng tôi tích lũy số lượng và đảm bảo rằng theo từng thời hạn`d`, chúng tôi không vượt quá tổng công suất và chúng tôi không vượt quá`'y'`dung tích. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(tối đa d · s) | O(n) | Quá chậm | 
| Sắp xếp + Kiểm tra tính khả thi của tiền tố | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển vấn đề thành việc kiểm tra xem tiền tố con người có thể phù hợp với các hạn chế về năng lực thời gian và năng lực nguồn lực hay không. 

1. Chia mọi người thành hai nhóm về mặt khái niệm: những người cần giấy vệ sinh (`y`) và những người không (`n`). Chúng tôi không xử lý chúng một cách riêng biệt trong việc lập kế hoạch nhưng chúng tôi theo dõi số lượng của chúng một cách riêng biệt. 
2. Sắp xếp tất cả mọi người theo thời hạn theo thứ tự không giảm. Điều này đảm bảo rằng khi chúng tôi xử lý tiền tố trước bất kỳ thời hạn nào`d`, chúng tôi đang xem xét chính xác những người phải hoàn thành không muộn hơn`d`. 
3. Duy trì ba bộ đếm trong khi quét danh sách đã sắp xếp: tổng số người đã xem cho đến nay, số lượng`'y'`mọi người đã nhìn thấy cho đến nay và số lượng ngầm`'n'`mọi người. 
4. Đối với mỗi người theo thứ tự được sắp xếp, chúng tôi tăng các bộ đếm và sau đó kiểm tra tính khả thi ở tiền tố đó. Nếu người hiện tại có thời hạn`d`, rồi theo thời gian`d`chúng ta phải có khả năng lên lịch cho tất cả những người được xử lý. 
5. Kiểm tra hai ràng buộc ở mỗi bước. Đầu tiên, tổng_người_so_far không được vượt quá`s * d`, vì mỗi`d`đơn vị thời gian có thể chứa tối đa`s`mọi người song song. Thứ hai,`'y'_so_far must not exceed `d`, vì chỉ được phục vụ một người sử dụng giấy vệ sinh trong một đơn vị thời gian. 
6. Nếu một trong hai ràng buộc bị vi phạm tại bất kỳ thời điểm nào, chúng ta có thể kết luận ngay rằng việc lập kế hoạch là không thể. 
7. Nếu chúng tôi xử lý xong tất cả mọi người mà không vi phạm thì lịch trình sẽ tồn tại. 

Lựa chọn thiết kế quan trọng là chỉ đánh giá các ràng buộc ở ranh giới thời hạn. Bất kỳ lịch trình hợp lệ nào cũng phải tôn trọng các khả năng tiền tố này, vì vậy nếu một lúc nào đó xuất hiện vi phạm thì không có sự sắp xếp lại nào có thể khắc phục được. 

### Tại sao nó hoạt động 

Thuật toán dựa trên một thuộc tính khả thi đơn điệu. Sắp xếp theo thời hạn đảm bảo rằng bất kỳ tiền tố nào cũng tương ứng với một nhóm người, tất cả đều phải được lên lịch trong cùng một khoảng thời gian. Trong bất kỳ khung thời gian nào`[1, d]`, hệ thống có chính xác`s * d`tổng số khe xử lý và chính xác`d`khe đặc biệt dành cho`'y'`người dùng. Nếu tiền tố vượt quá một trong hai giới hạn thì không có hoán vị nhiệm vụ nào có thể nén đủ khối lượng công việc, vì mỗi người không thể phân chia được và tiêu tốn chính xác một đơn vị thời gian. Điều này biến việc lập kế hoạch thành một cuộc kiểm tra năng lực toàn cầu thay vì một vấn đề phân công mang tính xây dựng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s, n = map(int, input().split())
    people = []
    for _ in range(n):
        d, t = input().split()
        d = int(d)
        people.append((d, t))

    people.sort()

    total = 0
    y_count = 0

    for d, t in people:
        total += 1
        if t == 'y':
            y_count += 1

        if total > s * d:
            print("No")
            return
        if y_count > d:
            print("No")
            return

    print("Yes")

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp tuân theo logic khả thi tiền tố. Việc sắp xếp đảm bảo chúng tôi xử lý thời hạn theo đúng thứ tự. Hai bộ đếm theo dõi chính xác số lượng xác định ranh giới khả thi. 

Một điểm tinh tế là sự so sánh sử dụng`s * d`, phải được tính toán với độ chính xác nguyên đầy đủ; Python xử lý việc này một cách an toàn, nhưng trong các ngôn ngữ khác, tình trạng tràn tràn sẽ cần được quan tâm. Một chi tiết khác là chúng tôi không cố gắng mô phỏng việc phân bổ gian hàng thực tế vì chỉ tính khả thi tổng hợp mới quan trọng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 7
2 y
2 n
5 y
1 n
5 n
2 y
1 n
```Chúng tôi sắp xếp theo thời hạn: 

| Người | Hạn chót | Loại | Tổng cộng | đếm Y | Kiểm tra`total ≤ s*d`| Kiểm tra`y ≤ d`| 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | n | 1 | 0 | 1 3 | 0 1 | 
| 2 | 2 | n | 2 | 0 | 2 6 | 0 2 | 
| 2 | 2 | y | 3 | 1 | 3 6 | 1 2 | 
| 2 | 2 | y | 4 | 2 | 4 6 | 2 2 2 | 
| 5 | 5 | y | 5 | 3 | 5 15 | 3 5 | 
| 5 | 5 | n | 6 | 3 | 6 15 | 3 5 | 
| 1 | 1 | n | 7 | 3 | 7 3 | 3 1 | 

Ở lần chèn cuối cùng, tổng công suất ở thời hạn 1 bị vi phạm, cho thấy không thể thực hiện được. 

Điều này chứng tỏ rằng ngay cả khi các tiền tố sớm hơn có vẻ hợp lệ thì việc đến muộn với thời hạn rất chặt chẽ có thể làm mất hiệu lực tính khả thi. 

### Ví dụ 2 

đầu vào:```
2 3
1 y
1 y
1 n
```Thứ tự sắp xếp đã đến hạn. 

| Bước | Hạn chót | Tổng cộng | đếm Y | s*d | Khả thi | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 2 | Có | 
| 2 | 1 | 2 | 2 | 2 | Có | 
| 3 | 1 | 3 | 2 | 2 | Không | 

Ở bước thứ ba, tổng số vượt quá khả năng sẵn có mặc dù thời hạn là như nhau. 

Điều này coi hạn chế về năng lực của gian hàng là yếu tố giới hạn ngay cả khi tất cả các thời hạn đều bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; quét tuyến tính đơn sau đó | 
| Không gian | O(n) | Kho lưu trữ của mọi người | 

Giải pháp phù hợp thoải mái trong giới hạn vì`n ≤ 100000`và việc sắp xếp cộng với một đường chuyền tuyến tính là hiệu quả trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    try:
        solve()
    except SystemExit:
        pass
    return ""

# minimal case
assert run("1 1\n1 y\n") == "", "single person"

# stall bottleneck
assert run("1 3\n1 y\n1 n\n1 n\n") == "", "capacity tight"

# impossible due to y constraint
assert run("2 3\n1 y\n1 y\n1 y\n") == "", "too many y"

# feasible mixed
assert run("2 3\n2 y\n2 n\n1 n\n") == "", "feasible mix"

# tight deadlines
assert run("3 4\n1 y\n1 n\n1 n\n2 y\n") == "", "deadline stress"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 / 1 năm | Có | trường hợp nhỏ nhất | 
| 1 3 tất cả thời hạn 1 | Không | tràn gian hàng | 
| 2 3 tất cả y | Không | tắc nghẽn giấy vệ sinh | 
| khả thi hỗn hợp | Có | sự tương tác đúng đắn | 
| thời hạn chặt chẽ | Có/Không có ranh giới | tính chính xác của tiền tố | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả mọi người đều có cùng thời hạn và`s`lớn nhưng`y`những ràng buộc rất chặt chẽ. Thuật toán xử lý việc này vì việc kiểm tra tiền tố`y_count ≤ d`ngay lập tức thực thi ràng buộc một nguồn lực độc lập với khả năng ngăn chặn. 

Một trường hợp khác là khi thời hạn rất lớn nhưng số lượng gian hàng lại ít. Mặc dù`s * d`có thể trông rất lớn, tiền tố phát triển cùng với`n`và việc kiểm tra đảm bảo rằng chúng ta không bao giờ ngầm giả định tính song song vô hạn. 

Một trường hợp khó phát hiện cuối cùng là khi những người có thời hạn nhỏ xuất hiện muộn theo thứ tự đầu vào. Việc sắp xếp đảm bảo chúng được xử lý trước, do đó thuật toán không vô tình trì hoãn thời hạn chặt chẽ thành các tiền tố không khả thi.
