---
title: "CF 1082C - Cuộc thi đa chủ đề"
description: "Mỗi ứng viên thuộc về một chủ đề duy nhất và có đóng góp bằng số, có thể tích cực hoặc tiêu cực."
date: "2026-06-15T05:57:55+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1082
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 55 (Rated for Div. 2)"
rating: 1600
weight: 1082
solve_time_s: 132
verified: true
draft: false
---

[CF 1082C - Cuộc thi đa chủ đề](https://codeforces.com/problemset/problem/1082/C) 

**Đánh giá:** 1600 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi ứng viên thuộc về một chủ đề duy nhất và có đóng góp bằng số, có thể tích cực hoặc tiêu cực. Chúng ta được yêu cầu chọn một nhóm ứng viên theo một ràng buộc về cấu trúc: sau khi lựa chọn, chúng ta cũng chọn một số nhóm đối tượng và đối với mỗi đối tượng được chọn, số lượng ứng viên được chọn trong chủ đề đó phải giống hệt nhau. 

Ràng buộc này buộc giải pháp phải cân bằng số lượng giữa các đối tượng. Nếu chúng ta quyết định rằng mỗi chủ đề được chọn phải đóng góp chính xác$k$mọi người thì mỗi chủ đề được lựa chọn đều phải đóng góp phần lớn nhất của mình$k$thí sinh từ môn đó, vì việc thêm một thí sinh yếu hơn chỉ có thể làm giảm tổng số và không giúp ích gì cho tính khả thi. 

Kết quả đầu ra là tổng tổng tốt nhất có thể có trên tất cả các lựa chọn hợp lệ hoặc bằng 0 nếu mọi cấu trúc hợp lệ không trống đều có tổng âm. 

Các ràng buộc đi lên đến$10^5$ứng viên và đối tượng. Một giải pháp thử tất cả các tập hợp con của chủ đề hoặc tất cả những gì có thể$k$giá trị cho mỗi chủ đề một cách độc lập sẽ ngay lập tức thất bại. Bất cứ điều gì bậc hai trong$n$hoặc thậm chí$n \log^2 n$với hằng số lớn có nguy cơ hết thời gian. 

Trường hợp nhạy cảm nhất là khi tất cả các ứng cử viên đều phủ định hoặc khi chỉ một số đối tượng có tiền tố dương. Ví dụ: nếu mọi giá trị đều âm thì mọi lựa chọn không trống đều tạo ra tổng âm, do đó câu trả lời đúng là bằng 0. Một trường hợp tinh vi khác là khi trộn các đối tượng sẽ cải thiện kết quả ngay cả khi các đối tượng riêng lẻ có số lượng tối ưu khác nhau. Một kẻ tham lam ngây thơ chọn lựa tốt nhất từng chủ đề một cách độc lập có thể thất bại vì sự chia sẻ$k$yêu cầu cặp đôi tất cả các môn học. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là quyết định một con số$k$, sau đó đối với mỗi môn học hãy chọn câu đầu tiên một cách độc lập$k$ứng cử viên nếu nó có ít nhất$k$mọi người. Sau đó, chúng tôi tổng hợp những đóng góp về tất cả các chủ đề và chọn ra ý kiến ​​hay nhất$k$. Điều này đúng vì trong một khoảng thời gian cố định$k$, mức độ tối ưu cho mỗi đối tượng là độc lập. 

Tuy nhiên, chi phí nằm ở việc đánh giá từng$k$. Nếu chúng ta thử tất cả$k$từ$1$ĐẾN$n$, và với mỗi$k$tính toán lại tổng tiền tố cho mỗi chủ đề, chúng tôi sẽ chi tiêu$O(n)$mỗi$k$, dẫn đến$O(n^2)$hoạt động trong trường hợp xấu nhất. Với$10^5$, điều này là không thể. 

Quan sát quan trọng là mỗi chủ đề có thể được xử lý trước thành một danh sách các kỹ năng được sắp xếp và tổng tiền tố của nó được tính một lần. Sau đó để cố định$k$, tổng số tiền đóng góp là tổng của các chủ thể trong tiền tố của chúng tại chỉ mục$k$, nếu nó tồn tại. Thay vì tính toán lại mỗi$k$, chúng ta đảo ngược quan điểm: mỗi tổng tiền tố đóng góp cho tất cả$k$theo chiều dài của nó, vì vậy chúng ta có thể tích lũy các đóng góp bằng cách lặp lại các chủ đề và phân phối tổng tiền tố của chúng thành một mảng toàn cục được lập chỉ mục bởi$k$. 

Điều này biến vấn đề thành tổng hợp các đóng góp trên tất cả các chủ đề theo thời gian tuyến tính sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force trên k và đối tượng |$O(n^2)$|$O(n)$| Quá chậm | 
| Sắp xếp + tiền tố + tích lũy |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nhóm ứng viên theo chủ đề của họ. Điều này là cần thiết vì ràng buộc áp dụng độc lập trong từng đối tượng. 
2. Đối với mỗi môn học, hãy sắp xếp các giá trị kỹ năng theo thứ tự giảm dần. Điều này đảm bảo rằng việc lấy cái đầu tiên$k$yếu tố tối ưu cho chủ đề đó. 
3. Xây dựng tổng tiền tố cho từng môn học. Tiền tố ở vị trí$i$đại diện cho số tiền tốt nhất có thể nếu chúng ta lấy chính xác$i+1$ứng viên của môn học đó. 
4. Đối với mỗi chủ đề, lặp lại các tổng tiền tố của nó. Đối với$i$- tổng tiền tố thứ, thêm nó vào bộ tích lũy chung cho kích thước nhóm$k = i+1$. Điều này thể hiện việc lựa chọn chủ đề này trong số những chủ đề góp phần vào việc tăng quy mô$k$. 
5. Duy trì một mảng toàn cầu`best[k]`đại diện cho tổng số đóng góp nếu mỗi chủ đề được chọn đóng góp chính xác$k$người tham gia. 
6. Sau khi xử lý tất cả các chủ đề, quét tất cả$k$và lấy giá trị dương lớn nhất. 

Lý do bước 4 có hiệu quả là vì mỗi môn học đóng góp độc lập vào mọi quy mô nhóm thống nhất có thể.$k$và chúng tôi đang tổng hợp những đóng góp tương thích giữa các chủ đề. 

### Tại sao nó hoạt động 

Khắc phục giải pháp hợp lệ với quy mô nhóm$k$. Trong giải pháp đó, mỗi chủ đề được chọn đều đóng góp chính xác ưu điểm của nó$k$các ứng cử viên, nếu không thì việc hoán đổi một ứng cử viên tốt hơn sẽ cải thiện tổng mà không vi phạm các ràng buộc. Do đó, mọi giải pháp hợp lệ đều tương ứng chính xác với việc chọn một số tập hợp con của các chủ đề và tính tổng của chúng.$k$-giá trị tiền tố. Thuật toán liệt kê ngầm tất cả những đóng góp như vậy và đánh giá mọi khả năng có thể.$k$, do đó không có cấu hình hợp lệ nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict

n, m = map(int, input().split())

subjects = defaultdict(list)

for _ in range(n):
    s, r = map(int, input().split())
    subjects[s].append(r)

best = [0] * (n + 1)

for vals in subjects.values():
    vals.sort(reverse=True)
    pref = 0
    for i, v in enumerate(vals):
        pref += v
        k = i + 1
        if pref > 0:
            best[k] += pref

print(max(0, max(best)))
```Mã trước tiên sẽ nhóm các ứng cử viên theo chủ đề, sau đó sắp xếp từng nhóm sao cho tổng tiền tố thể hiện các lựa chọn tối ưu cho mỗi quy mô nhóm cố định. các`best`mảng tích lũy các đóng góp được lập chỉ mục theo số lượng người được lấy từ mỗi chủ đề. 

Séc`if pref > 0`đảm bảo chúng tôi chỉ bao gồm các tiền tố giúp cải thiện kết quả; tiền tố phủ định sẽ chỉ làm xấu đi bất kỳ cấu hình nào sử dụng tiền tố đó$k$. Điều này phù hợp với bước tối đa hóa cuối cùng mà chúng tôi cũng giữ ở mức 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 3
2 6
3 6
2 5
3 5
1 9
3 1
```Sau khi phân nhóm: 

| Chủ đề | Giá trị được sắp xếp | Tổng tiền tố | 
| --- | --- | --- | 
| 1 | [9] | [9] | 
| 2 | [6, 5] | [6, 11] | 
| 3 | [6, 5, 1] ​​| [6, 11, 12] | 

Bây giờ chúng tôi phân phối: 

| k | đóng góp | 
| --- | --- | 
| 1 | 9 + 6 + 6 = 21 | 
| 2 | 11 + 11 = 22 | 
| 3 | 12 | 

Tối đa là 22. 

Điều này khẳng định rằng giải pháp tối ưu sử dụng$k=2$, nghĩa là hai môn có hai học sinh đứng đầu mỗi môn. 

### Ví dụ 2 

đầu vào:```
5 3
1 6
2 6
3 11
1 -5
2 0
```Nhóm và sắp xếp: 

| Chủ đề | Đã sắp xếp | Tổng tiền tố | 
| --- | --- | --- | 
| 1 | [6, -5] | [6, 1] | 
| 2 | [6, 0] | [6, 6] | 
| 3 | [11] | [11] | 

tích lũy: 

| k | giá trị | 
| --- | --- | 
| 1 | 6 + 6 + 11 = 23 | 
| 2 | 1 + 6 = 7 | 

Tốt nhất là 23. 

Điều này cho thấy thậm chí các yếu tố tiêu cực có thể được đưa vào như thế nào nếu chúng được bù đắp bởi các yếu tố tích cực khác cùng một lúc.$k$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| sắp xếp tất cả các danh sách chủ đề chiếm ưu thế | 
| Không gian |$O(n)$| lưu trữ các giá trị được nhóm và mảng tiền tố | 

Bước sắp xếp có thể chấp nhận được đối với$10^5$các phần tử. Tất cả các hoạt động khác là truyền tuyến tính trên dữ liệu, giúp duy trì thời gian chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    n, m = map(int, input().split())
    subjects = defaultdict(list)

    for _ in range(n):
        s, r = map(int, input().split())
        subjects[s].append(r)

    best = [0] * (n + 1)

    for vals in subjects.values():
        vals.sort(reverse=True)
        pref = 0
        for i, v in enumerate(vals):
            pref += v
            k = i + 1
            if pref > 0:
                best[k] += pref

    return str(max(0, max(best)))

# provided sample
assert run("""6 3
2 6
3 6
2 5
3 5
1 9
3 1
""") == "22"

# all negative
assert run("""3 2
1 -1
1 -2
2 -3
""") == "0"

# single subject
assert run("""4 2
1 5
1 4
1 -10
1 3
""") == "12"

# mixed subjects optimal k=1
assert run("""3 3
1 10
2 9
3 8
""") == "27"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều tiêu cực | 0 | xử lý đúng tập tối ưu trống | 
| chủ đề duy nhất | 12 | lựa chọn tiền tố trong một nhóm | 
| hỗn hợp k=1 | 27 | tổng hợp toàn cầu giữa các chủ đề | 

## Vỏ cạnh 

Một trường hợp hoàn toàn tiêu cực thể hiện hành vi dự phòng. Đối với đầu vào như`[-1, -2, -3]`giữa các chủ đề, mọi tổng tiền tố đều âm, vì vậy không$k$tích lũy giá trị dương và thuật toán trả về 0 một cách chính xác vì`max(best)`là âm hoặc bằng không. 

Đầu vào một chủ đề kiểm tra rằng thuật toán không yêu cầu nhiều chủ đề để tạo thành một giải pháp hợp lệ. Tổng tiền tố tốt nhất của chủ đề trực tiếp trở thành câu trả lời và cơ chế tích lũy vẫn hoạt động vì chỉ có một chủ đề đóng góp. 

Phân phối thưa thớt trong đó mỗi chủ đề chỉ có một ứng cử viên đảm bảo thuật toán xử lý chính xác tất cả các trường hợp có thể$k=1$đóng góp. Mỗi chủ đề đóng góp đúng một lần vào`best[1]`, và không cao hơn$k$được điền, làm cho giá trị tối đa phản ánh chính xác tổng của tất cả các mục dương đơn lẻ.
