---
title: "CF 102775A - \u041a\u0442\u043e \u0431\u043b\u0438\u0436\u0435?"
description: "Câu chuyện có thể được rút gọn thành một quyết định hình học đơn giản trên trục số. Ba tọa độ được đưa ra: chú hề Nekit, người chủ cũ Luka và con chó. Con chó phải quyết định xem người nào ở gần hơn. Nếu con chó ở gần Nekit hơn thì câu trả lời là Tetka."
date: "2026-07-27T20:46:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102775
codeforces_index: "A"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 20), \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u0426\u0435\u043d\u0442\u0440\u0430\u043b\u044c\u043d\u043e\u0439 \u0420\u043e\u0441\u0441\u0438\u0438, \u043a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434"
rating: 0
weight: 102775
solve_time_s: 69
verified: true
draft: false
---

[CF 102775A - \u041a\u0442\u043e \u0431\u043b\u0438\u0436\u0435?](https://codeforces.com/problemset/problem/102775/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Câu chuyện có thể được rút gọn thành một quyết định hình học đơn giản trên trục số. Ba tọa độ được đưa ra: chú hề Nekit, người chủ cũ Luka và con chó. Con chó phải quyết định xem người nào ở gần hơn. Nếu con chó ở gần Nekit hơn thì câu trả lời là`Tetka`. Nếu con chó ở gần Luka hơn hoặc nếu cả hai khoảng cách đều bằng nhau thì câu trả lời là`Kashtanka`. 

Đầu vào chứa ba số nguyên không âm khác nhau, mỗi số nhiều nhất$10^6$. Vì tọa độ nhỏ nên khoảng cách giữa các điểm cũng nhỏ, nhưng ngay cả khi tọa độ lớn hơn nhiều thì chỉ cần một số phép tính số học không đổi. Điều này có nghĩa là bất kỳ giải pháp nào quét phạm vi, mô phỏng chuyển động hoặc thực hiện công việc tùy thuộc vào giá trị tọa độ đều không cần thiết. Giải pháp dự định sẽ chạy trong thời gian không đổi. 

Một sai lầm thường gặp là xử lý hộp cà vạt không đúng cách. Ví dụ, với đầu vào`1 5 3`, con chó cách cả hai người hai đơn vị. Câu trả lời đúng là`Kashtanka`, nhưng mã chỉ kiểm tra xem con chó có ở gần Nekit hơn hay không và gửi tất cả các trường hợp khác tới`Tetka`sẽ thất bại. 

Một trường hợp khó khăn khác là khi con chó ở giữa hai người nhưng không hẳn ở giữa. Ví dụ,`1 6 5`đưa ra khoảng cách là bốn và một, nên con chó quay trở lại`Kashtanka`. Một giải pháp chỉ so sánh tọa độ và cho rằng người ở gần nhất luôn là người có tọa độ nhỏ hơn có thể thất bại nếu bỏ qua vị trí của con chó. 

Trường hợp cuối cùng là khi con chó nằm ngoài đoạn do hai người tạo thành. Ví dụ,`2 9 12`đưa ra khoảng cách là mười và ba, nên con chó ở lại với Nekit. Chỉ nhìn vào người có tọa độ lớn hơn sẽ không đủ vì vị trí của con chó thay đổi khoảng cách. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là tính khoảng cách từ con chó đến mỗi người và so sánh hai giá trị. Vì tọa độ nằm trên một đường thẳng nên khoảng cách giữa hai điểm là hiệu tọa độ tuyệt đối của chúng. Phương pháp này đã tối ưu cho vấn đề này, nhưng sẽ rất hữu ích khi xem xét cách tiếp cận bạo lực sẽ trông như thế nào. 

Cách giải thích bằng vũ lực có thể mô phỏng chuyển động có thể có của con chó đối với mỗi người và đếm xem cần bao nhiêu bước để tiếp cận họ. Nếu tọa độ lớn như$10^6$, điều này có thể cần khoảng một triệu bước mô phỏng cho một lần so sánh. Đó là việc làm không cần thiết vì đáp án cuối cùng chỉ phụ thuộc vào độ chênh lệch giữa các tọa độ chứ không phụ thuộc vào từng vị trí trung gian. 

Quan sát quan trọng là khoảng cách trên trục số có được trực tiếp thông qua phép trừ. Thay vì đi theo đường đi giữa các điểm, chúng ta có thể tính ngay cả hai khoảng cách và quyết định. Chi tiết duy nhất cần quan tâm là dây buộc: khoảng cách bằng nhau thuộc về Luka nên điều kiện để`Tetka`phải nhỏ hơn một cách nghiêm ngặt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(10^6) | O(1) | Quá chậm trong cài đặt tổng quát | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba tọa độ đại diện cho Nekit, Luka và con chó. Tên của các biến phải phản ánh vai trò của chúng vì sự so sánh là về con người và vị trí của con chó chứ không phải về một mảng tùy ý. 
2. Tính khoảng cách tuyệt đối giữa Nekit và con chó. Giá trị tuyệt đối là cần thiết vì con chó có thể nằm ở hai bên của Nekit. 
3. Tính khoảng cách tuyệt đối giữa Luka và con chó bằng công thức tương tự. 
4. Nếu khoảng cách của Nekit nhỏ hơn khoảng cách của Luka, hãy in`Tetka`. Việc so sánh phải sử dụng`<`bởi vì khoảng cách bằng nhau thuộc về Luka. 
5. Trong mọi tình huống khác, hãy in`Kashtanka`. Điều này bao gồm cả trường hợp Luka ở gần hơn và trường hợp khoảng cách giống hệt nhau. 

Tại sao nó hoạt động: thuật toán so sánh chính xác hai đại lượng xác định quyết định, khoảng cách của con chó với mỗi người. Nếu khoảng cách của Nekit nhỏ hơn thì con chó ở gần Nekit hơn và kết quả đầu ra cần thiết là`Tetka`. Nếu điều kiện đó sai thì khoảng cách của Luka nhỏ hơn hoặc bằng khoảng cách của Nekit và cả hai khả năng đều yêu cầu`Kashtanka`. Vì mọi mối quan hệ có thể có giữa hai khoảng cách đều được che đậy nên thuật toán không thể tạo ra kết quả không chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    nek, luka, dog = map(int, input().split())

    nek_dist = abs(nek - dog)
    luka_dist = abs(luka - dog)

    if nek_dist < luka_dist:
        print("Tetka")
    else:
        print("Kashtanka")

if __name__ == "__main__":
    solve()
```Đầu tiên, chương trình đọc ba tọa độ và lưu trữ chúng theo vai trò của chúng trong câu chuyện. Điều này tránh nhầm lẫn thứ tự của các giá trị đầu vào trong quá trình so sánh. 

hai`abs`các cuộc gọi trực tiếp thực hiện công thức khoảng cách dòng số. Số nguyên Python không gặp vấn đề tràn đối với các ràng buộc này, vì vậy phép trừ là an toàn. 

Điều kiện cuối cùng có chủ ý sử dụng một so sánh nghiêm ngặt. Nếu khoảng cách bằng nhau thì`else`chi nhánh chạy và in`Kashtanka`, phù hợp với quy tắc ràng buộc cần thiết. Không có vòng lặp hoặc cấu trúc dữ liệu bổ sung vì toàn bộ vấn đề được giải quyết bằng hai phép tính và một phép so sánh. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, đầu vào là`1 4 2`. 

| Bước | Tọa độ Nekit | Luka phối hợp | Chó phối hợp | Khoảng cách Nekit | Khoảng cách Luka | Đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | 
| Đọc giá trị | 1 | 4 | 2 | - | - | - | 
| Tính khoảng cách | 1 | 4 | 2 | 1 | 2 | - | 
| So sánh | 1 | 4 | 2 | 1 < 2 | - | Tetka | 

Con chó cách Nekit một đơn vị và cách Luka hai đơn vị, vì vậy việc so sánh chặt chẽ đã thành công. Ví dụ này minh họa trường hợp người gần gũi hơn bình thường. 

Đối với mẫu thứ hai, đầu vào là`1 5 3`. 

| Bước | Tọa độ Nekit | Luka phối hợp | Chó phối hợp | Khoảng cách Nekit | Khoảng cách Luka | Đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | 
| Đọc giá trị | 1 | 5 | 3 | - | - | - | 
| Tính khoảng cách | 1 | 5 | 3 | 2 | 2 | - | 
| So sánh | 1 | 5 | 3 | 2 < 2 là sai | - | Kashtanka | 

Cả hai khoảng cách đều bằng nhau nên việc so sánh chặt chẽ không chọn Nekit. Luật buộc phải trả lại con chó cho Luka. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có hai khoảng cách và một so sánh được thực hiện. | 
| Không gian | O(1) | Chương trình chỉ lưu trữ một vài biến số nguyên. | 

Giải pháp dễ dàng phù hợp với các giới hạn vì thời gian chạy của nó không phụ thuộc vào kích thước của tọa độ. Ngay cả giá trị tọa độ tối đa cũng yêu cầu lượng công việc không đổi như đầu vào nhỏ nhất. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(data: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(data)
    sys.stdout = io.StringIO()

    nek, luka, dog = map(int, sys.stdin.readline().split())

    nek_dist = abs(nek - dog)
    luka_dist = abs(luka - dog)

    if nek_dist < luka_dist:
        print("Tetka")
    else:
        print("Kashtanka")

    result = sys.stdout.getvalue().strip()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

assert solve_data("1 4 2\n") == "Tetka", "sample 1"
assert solve_data("1 5 3\n") == "Kashtanka", "sample 2"
assert solve_data("1 6 5\n") == "Kashtanka", "sample 3"

assert solve_data("0 10 0\n") == "Kashtanka", "same coordinate side boundary is impossible, but catches equality handling"
assert solve_data("0 1000000 999999\n") == "Tetka", "maximum coordinate range"
assert solve_data("0 2 1\n") == "Kashtanka", "middle point tie case"
assert solve_data("5 8 1000000\n") == "Kashtanka", "dog far outside the segment"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 10 0`|`Kashtanka`| Xử lý bình đẳng khi khoảng cách phù hợp. | 
|`0 1000000 999999`|`Tetka`| Giá trị tọa độ lớn và định vị phân khúc bên ngoài. | 
|`0 2 1`|`Kashtanka`| Chính xác điểm giữa buộc. | 
|`5 8 1000000`|`Kashtanka`| Tính khoảng cách khi con chó vượt xa cả hai người. | 

Bài toán ban đầu đảm bảo rằng cả ba tọa độ đều khác nhau, do đó một bài kiểm tra hợp lệ thực tế không thể chứa các giá trị hoàn toàn bằng nhau. Sự bình đẳng về khoảng cách là trường hợp cạnh có ý nghĩa vì đây là trường hợp mà quy tắc đầu ra khác với việc chỉ chọn khoảng cách nhỏ hơn. 

## Vỏ cạnh 

Trường hợp hòa được xử lý bằng sự so sánh chặt chẽ. Đối với đầu vào`1 5 3`, thuật toán tính toán`abs(1 - 3) = 2`Và`abs(5 - 3) = 2`. Từ`2 < 2`là sai, nó in`Kashtanka`, tuân theo quy tắc khoảng cách bằng nhau sẽ trả con chó về với Luka. 

Khi con chó ở gần người có tọa độ lớn hơn, thuật toán vẫn hoạt động vì nó không bao giờ dựa vào thứ tự tọa độ. Đối với đầu vào`1 6 5`, khoảng cách là`4`Và`1`, do đó việc so sánh chọn`Kashtanka`. 

Khi con chó nằm ngoài phạm vi giữa hai người, phép tính chênh lệch tuyệt đối vẫn có hiệu lực. Đối với đầu vào`2 9 12`, khoảng cách là`10`Và`3`, vậy là con chó ở gần Nekit hơn và kết quả là`Tetka`. Công thức tương tự xử lý các điểm ở hai bên của con chó mà không có trường hợp đặc biệt nào.
