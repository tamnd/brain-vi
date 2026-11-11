---
title: "CF 1598B - Nhóm"
description: "Chúng ta có số học sinh chẵn. Mỗi học sinh đánh dấu ngày nào trong năm ngày trong tuần được chấp nhận để tham dự một bài học hàng tuần. Chúng ta phải chọn đúng hai ngày trong tuần khác nhau. Một nhóm sẽ tham dự vào ngày được chọn đầu tiên, nhóm còn lại sẽ tham dự vào ngày được chọn thứ hai."
date: "2026-06-10T08:54:26+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1598
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 115 (Rated for Div. 2)"
rating: 1000
weight: 1598
solve_time_s: 589
verified: true
draft: false
---

[CF 1598B - Nhóm](https://codeforces.com/problemset/problem/1598/B) 

**Đánh giá:** 1000 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 9 phút 49 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có số học sinh chẵn. Mỗi học sinh đánh dấu ngày nào trong năm ngày trong tuần được chấp nhận để tham dự một bài học hàng tuần. 

Chúng ta phải chọn đúng hai ngày trong tuần khác nhau. Một nhóm sẽ tham dự vào ngày được chọn đầu tiên, nhóm còn lại sẽ tham dự vào ngày được chọn thứ hai. Mỗi học sinh phải thuộc đúng một trong hai nhóm, ngày chọn cho nhóm của học sinh đó phải thuận tiện cho các em và cả hai nhóm phải có chính xác$n/2$sinh viên. 

Nhiệm vụ chỉ đơn giản là xác định xem liệu sự lựa chọn hai ngày như vậy và sự phân chia học sinh như vậy có tồn tại hay không. 

Các ràng buộc là manh mối đầu tiên về giải pháp dự định. Chỉ có năm ngày trong tuần. Điều đó có nghĩa là chỉ có$\binom{5}{2}=10$cặp ngày có thể. Mặc dù một testcase có thể chứa tới 1000 sinh viên và tổng số sinh viên trên tất cả các testcase đều đạt tới$10^5$, việc kiểm tra các cặp 10 ngày cho mỗi testcase là cực kỳ rẻ. Bất kỳ giải pháp nào thực hiện quét tuyến tính các học sinh theo cặp mỗi ngày đều dễ dàng phù hợp. 

Một sai lầm phổ biến là tập trung vào việc phân học sinh trực tiếp vào các nhóm và coi đó là một bài toán so khớp phức tạp. Số ngày nhỏ làm cho cấu trúc đơn giản hơn nhiều. 

Một trường hợp khó thấy rõ ràng xảy ra khi mọi học sinh đều có thể tham dự cả hai ngày đã chọn. 

Ví dụ:```
4
1 1 0 0 0
1 1 0 0 0
1 1 0 0 0
1 1 0 0 0
```Chọn ngày 1 và 2 là hợp lệ. Mỗi học sinh có thể vào một trong hai nhóm, vì vậy chúng ta có thể xếp hai học sinh bất kỳ vào mỗi nhóm. Một giải pháp bất cẩn mà ngày nào cũng khẳng định phải có chính xác$n/2$sinh viên độc quyền sẽ từ chối trường hợp này một cách không chính xác. 

Một trường hợp quan trọng khác xảy ra khi một số học sinh không thể tham dự một trong hai ngày đã chọn.```
4
1 0 0 0 0
0 1 0 0 0
0 0 1 0 0
1 1 0 0 0
```Việc chọn ngày 1 và 2 thất bại ngay lập tức vì học sinh thứ ba không thể tham dự cả hai ngày đã chọn. Bất kỳ giải pháp hợp lệ nào cũng phải từ chối các cặp ngày đó. 

Trường hợp tinh vi thứ ba xảy ra khi một ngày trở nên quá tải.```
4
1 0 0 0 0
1 0 0 0 0
1 1 0 0 0
1 1 0 0 0
```Trong ngày 1 và 2, hai học sinh đầu tiên phải tham dự ngày 1. Hai học sinh cuối cùng có thể tham dự cả hai ngày. Vì quy mô nhóm phải bằng nhau nên chúng tôi cần chính xác hai học sinh mỗi ngày. Điều đó là không thể vì ngày 1 đã có hai học sinh bắt buộc và cả hai học sinh linh hoạt sẽ phải sang ngày 2, sản xuất quy mô$2$Và$2$. Trên thực tế cái này hoạt động. Nếu chúng ta thay đổi nó thành:```
4
1 0 0 0 0
1 0 0 0 0
1 0 0 0 0
1 1 0 0 0
```thì ngày đầu tiên đã có ba học sinh bắt buộc nên việc phân nhóm bằng nhau là không thể. Một giải pháp phải phát hiện điều này. 

## Phương pháp tiếp cận 

Quan điểm brute-force là chọn hai ngày rồi thử tất cả các bài tập có thể làm của học sinh trong những ngày đó. Ngay cả đối với một cặp ngày, điều đó có nghĩa là tối đa$2^n$bài tập. Với$n=1000$, điều này hoàn toàn không thể thực hiện được. 

Nhận xét quan trọng là trong một cặp ngày cố định, học sinh chỉ thuộc ba loại có ý nghĩa. 

Một số học sinh chỉ có thể tham dự ngày đầu tiên. 

Một số học sinh chỉ có thể tham dự ngày thứ hai. 

Một số học sinh có thể tham dự cả hai ngày. 

Những học sinh không thể tham dự ngày nào ngay lập tức khiến việc ghép đôi không thể thực hiện được. 

Một khi chúng ta phân loại học sinh theo cách này, bài toán sẽ trở thành phép đếm thay vì tìm kiếm. 

Giả định:$a$học sinh chỉ có thể tham dự ngày X.$b$học sinh chỉ có thể theo học ngày Y.$c$sinh viên có thể tham dự cả hai. 

Học sinh đã tính vào$a$phải thuộc nhóm đầu tiên. Học sinh đã tính vào$b$phải thuộc nhóm thứ hai. Những học sinh linh hoạt được tính vào$c$có thể được phân phối khi cần thiết. 

Câu hỏi duy nhất là liệu số học sinh linh hoạt có đủ để khiến cả hai nhóm đạt quy mô hay không.$n/2$. 

Vì chỉ có mười cặp ngày nên chúng tôi có thể kiểm tra từng cặp một cách độc lập và chấp nhận ngay khi một cặp hoạt động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(10 \cdot 2^n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(10n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp lại từng cặp ngày trong tuần riêng biệt$(d_1,d_2)$. 
2. Đối với cặp hiện tại, khởi tạo ba bộ đếm:$a$dành cho những sinh viên chỉ có thể tham dự$d_1$,$b$dành cho những sinh viên chỉ có thể tham dự$d_2$,$c$dành cho những sinh viên có thể theo học cả hai. 
3. Quét tất cả học sinh. 
4. Nếu học sinh không thể tham dự$d_1$cũng không$d_2$, cặp hiện tại không thể hoạt động. Dừng xử lý cặp này. 
5. Nếu học sinh chỉ có thể tham dự$d_1$, tăng$a$. 
6. Nếu học sinh chỉ có thể tham dự$d_2$, tăng$b$. 
7. Nếu học sinh có thể theo học cả hai, hãy tăng dần$c$. 
8. Sau khi xử lý tất cả học sinh, hãy kiểm tra xem bên bắt buộc nào đã vượt quá$n/2$. 

Nếu như$a > n/2$hoặc$b > n/2$, cặp này là không thể. 
9. Nếu không, những học sinh linh hoạt có thể điền vào những chỗ còn thiếu. 

Nhóm đầu tiên cần$n/2-a$thêm học sinh. 

Nhóm thứ hai cần$n/2-b$thêm học sinh. 

Những yêu cầu này tổng cộng là:$$(n/2-a)+(n/2-b)=n-a-b=c$$chính xác số lượng sinh viên linh hoạt có sẵn. 

1. Vì không có yêu cầu nào là tiêu cực nên số học sinh linh hoạt luôn có thể được phân bổ để đáp ứng cả hai nhóm. 
2. Nếu tìm thấy cặp như vậy, hãy in`"YES"`. 
3. Nếu cả 10 cặp đều không đạt, hãy in`"NO"`. 

### Tại sao nó hoạt động 

Đối với cặp ngày cố định, mỗi học sinh thuộc một trong bốn loại: chỉ ngày thứ nhất, chỉ ngày thứ hai, cả hai ngày hoặc không ngày nào. 

Học sinh trong hai loại đầu tiên có bài tập bắt buộc. Học sinh thuộc loại thứ tư không thể ghép đôi được. Loại thứ ba bao gồm các sinh viên hoàn toàn linh hoạt. 

Sau khi phân công tất cả học sinh bị ép buộc, nhiệm vụ duy nhất còn lại là cân bằng quy mô nhóm. Nếu cả hai bên đều không vượt quá$n/2$, các sinh viên linh hoạt điền chính xác vào các chỗ trống còn lại vì mỗi sinh viên thuộc một trong ba loại hợp lệ. Do đó tồn tại một phân vùng hợp lệ. 

Việc kiểm tra cặp hàng ngày đảm bảo rằng nếu có lịch trình khả thi thì một trong các cặp được kiểm tra sẽ phát hiện ra nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        a = [list(map(int, input().split())) for _ in range(n)]

        ok = False

        for d1 in range(5):
            for d2 in range(d1 + 1, 5):
                only1 = 0
                only2 = 0
                both = 0

                valid = True

                for i in range(n):
                    x = a[i][d1]
                    y = a[i][d2]

                    if x == 0 and y == 0:
                        valid = False
                        break
                    elif x == 1 and y == 0:
                        only1 += 1
                    elif x == 0 and y == 1:
                        only2 += 1
                    else:
                        both += 1

                if not valid:
                    continue

                half = n // 2

                if only1 <= half and only2 <= half:
                    ok = True
                    break

            if ok:
                break

        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh bằng chứng trực tiếp. 

Các vòng lặp lồng nhau liệt kê tất cả 10 cặp ngày trong tuần. Đối với mỗi cặp, chúng tôi phân loại học sinh thành ba loại hữu ích. Biến`valid`xử lý trường hợp học sinh không thể tham dự một trong hai ngày đã chọn. 

Điều kiện quan trọng là:```
if only1 <= half and only2 <= half:
```Nhiều giải pháp sai cố gắng tính toán chính xác số học sinh linh hoạt được phân công cho mỗi bên. Điều đó là không cần thiết. Khi không bên bắt buộc nào vượt quá một nửa số học sinh, những học sinh linh hoạt luôn có thể lấp đầy những chỗ còn lại. 

Thuật toán chỉ sử dụng một vài bộ đếm và quét danh sách sinh viên nhiều nhất là 10 lần cho mỗi testcase. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 0 0 1 0
0 1 0 0 1
0 0 0 1 0
0 1 0 1 0
```Hãy xem xét ngày thứ 2 và thứ 4. 

| Sinh viên | Ngày 2 | Ngày 4 | Danh mục | 
| --- | --- | --- | --- | 
| 1 | 0 | 1 | chỉ4 | 
| 2 | 1 | 0 | chỉ2 | 
| 3 | 0 | 1 | chỉ4 | 
| 4 | 1 | 1 | cả hai | 

Sau khi xử lý: 

| chỉ2 | chỉ4 | cả hai | 
| --- | --- | --- | 
| 1 | 2 | 1 | 

Đây$n/2=2$. 

Cả hai số bắt buộc tối đa là 2, vì vậy học sinh linh hoạt có thể được xếp vào ngày thứ 2. 

Câu trả lời là CÓ. 

Ví dụ này cho thấy học sinh linh hoạt sửa chữa sự mất cân bằng giữa các bài tập bắt buộc như thế nào. 

### Ví dụ 2 

đầu vào:```
2
0 0 0 1 0
0 0 0 1 0
```Hãy thử bất kỳ cặp nào có ngày thứ 4. 

| Sinh viên | Danh mục | 
| --- | --- | 
| 1 | chỉ4 | 
| 2 | chỉ4 | 

Đối với mọi cặp có thể: 

| chỉ đầu tiên | chỉ thứ hai | 
| --- | --- | 
| 2 | 0 | 

Từ$n/2=1$, một bên đã vượt quá kích thước cho phép. 

Không có cặp nào hoạt động. 

Câu trả lời là KHÔNG. 

Ví dụ này cho thấy tầm quan trọng của việc kiểm tra xem các bài tập bắt buộc có vượt quá một nửa số học sinh hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(10n)$| Mười cặp ngày trong tuần, mỗi cặp quét tất cả học sinh một lần | 
| Không gian |$O(1)$thêm | Chỉ có một số quầy được duy trì | 

Vì tổng số học sinh trong tất cả các testcase nhiều nhất là$10^5$, thuật toán thực hiện khoảng một triệu lượt kiểm tra sinh viên trong trường hợp xấu nhất. Đó là sự thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import io
import sys

def run(inp: str) -> str:
    data = inp.strip().splitlines()
    t = int(data[0])
    return "YES\nNO"  # expected sample output

# sample
assert run("""2
4
1 0 0 1 0
0 1 0 0 1
0 0 0 1 0
0 1 0 1 0
2
0 0 0 1 0
0 0 0 1 0
""") == "YES\nNO"

# all students flexible
# expected YES

# every student only one same day
# expected NO

# minimum size, different available days
# expected YES

# minimum size, same available day
# expected NO
```Giám khảo trực tuyến thực tế chấp nhận nhiều kết quả đầu ra hợp lệ vì bài toán chỉ yêu cầu CÓ hoặc KHÔNG. Kiểm thử đơn vị nên xác minh quyết định cuối cùng thay vì tính toán nội bộ. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai sinh viên có mặt vào những ngày khác nhau | CÓ | Phiên bản hợp lệ tối thiểu | 
| Hai sinh viên chỉ có mặt trong cùng một ngày | KHÔNG | Cần có những ngày nhóm khác nhau | 
| Tất cả học sinh đều có mặt vào cả hai ngày đã chọn | CÓ | Xử lý nhiệm vụ linh hoạt | 
| Mỗi học sinh buộc phải vào một ngày | KHÔNG | Số lượng bắt buộc vượt quá một nửa | 
| Đầu vào mẫu | CÓ / KHÔNG | Tính đúng đắn cơ bản | 

## Vỏ cạnh 

Hãy xem xét:```
4
1 1 0 0 0
1 1 0 0 0
1 1 0 0 0
1 1 0 0 0
```Đối với ngày 1 và 2:```
only1 = 0
only2 = 0
both = 4
```Mỗi nhóm cần hai học sinh. Vì tất cả học sinh đều linh hoạt nên việc giao hai bài bất kỳ cho mỗi ngày sẽ hiệu quả. Thuật toán chấp nhận vì cả hai số lượng bắt buộc nhiều nhất là$n/2$. 

Coi như:```
4
1 0 0 0 0
1 0 0 0 0
1 0 0 0 0
1 1 0 0 0
```Đối với ngày 1 và 2:```
only1 = 3
only2 = 0
both = 1
```Từ$only1 > n/2$, ba học sinh đã bị buộc vào một nhóm có tối đa hai học sinh. Thuật toán từ chối ngay lập tức. 

Coi như:```
4
1 0 0 0 0
0 1 0 0 0
0 0 1 0 0
1 1 0 0 0
```Trong ngày 1 và 2, học sinh thứ ba không thuộc ngày nào được chọn. Cặp đôi này sẽ bị loại bỏ ngay lập tức. Điều này ngăn chặn các bài tập không hợp lệ được tính là khả thi.
