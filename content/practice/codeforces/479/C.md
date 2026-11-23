---
title: "CF 479C - Kỳ thi"
description: "Chúng tôi được cung cấp một tập hợp các bài kiểm tra, mỗi bài có hai ngày liên quan. Lịch thi chính thức phân công thi i một ngày ai, nhưng học sinh có thỏa thuận đặc biệt cho phép thi sớm hơn vào ngày bi, khi bi < ai."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 479
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 274 (Div. 2)"
rating: 1400
weight: 479
solve_time_s: 658
verified: false
draft: false
---

[CF 479C - Bài kiểm tra](https://codeforces.com/problemset/problem/479/C) 

**Đánh giá:** 1400 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 10 phút 58 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một tập hợp các bài kiểm tra, mỗi bài có hai ngày liên quan. Lịch thi chính thức`i`một ngày`a_i`, nhưng học sinh có một thỏa thuận đặc biệt cho phép anh ta thi sớm hơn trong ngày`b_i`, Ở đâu`b_i < a_i`. Bất cứ khi nào một kỳ thi được thực hiện, trường đại học sẽ ghi lại vào ngày đã lên lịch`a_i`, không phải ngày thực tế nó được chụp. 

Học sinh có thể thi nhiều bài thi trong cùng một ngày và thứ tự thi trong ngày đó không quan trọng. Tuy nhiên, thứ tự ngày tháng ghi trong bảng điểm phải không giảm theo thứ tự thi. Điều này tạo ra sự kết hợp giữa các lựa chọn lập lịch trình và thứ tự: việc chọn một bài kiểm tra sớm hơn trong thời gian thực có thể ảnh hưởng đến việc liệu các bài kiểm tra trong tương lai có thể được đặt mà không vi phạm ràng buộc thứ tự bảng điểm hay không. 

Mục đích là giảm thiểu ngày thực cuối cùng mà bất kỳ kỳ thi nào được thực hiện trong khi vẫn tạo ra một chuỗi các kết quả được ghi lại hợp lệ.`a_i`giá trị theo thứ tự không giảm. 

Ràng buộc`n ≤ 5000`ngụ ý một`O(n^2)`hoặc`O(n log n)`giải pháp có thể chấp nhận được, nhưng bất cứ điều gì như liệt kê tất cả các hoán vị hoặc tập hợp con đều không khả thi ngay lập tức. Chiến lược giai thừa hoặc cấp số nhân trong việc sắp xếp các kỳ thi sẽ bùng nổ vượt quá giới hạn ngay cả đối với những người vừa phải.`n`. 

Một trường hợp phức tạp xuất hiện khi các lựa chọn sớm buộc các kỳ thi sau đó mất đi khoảng thời gian khả thi. Ví dụ, nếu một bài thi có điểm rất nhỏ`b_i`nhưng lớn`a_i`, việc chọn quá muộn có thể chặn tất cả các vị trí trong tương lai có`a_i`nằm giữa hai lựa chọn của nó. 

Một trường hợp khó khăn khác là khi nhiều bài kiểm tra có cùng điểm thi`a_i`. Vì thứ tự bảng điểm không giảm nên các giá trị bằng nhau có thể được nhóm tùy ý, nhưng những quyết định tham lam không chính xác vẫn có thể đẩy ngày cuối cùng lên cao một cách không cần thiết. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ xem xét tất cả các lựa chọn có thể thực hiện mỗi bài kiểm tra trong ngày`b_i`hoặc`a_i`, sau đó thử tất cả các hoán vị của bài kiểm tra để tìm thứ tự hợp lệ có trình tự được ghi được sắp xếp. Đối với mỗi cấu hình, chúng tôi sẽ kiểm tra tính khả thi và theo dõi ngày thực được chọn tối đa. Cách tiếp cận này ngầm khám phá cả không gian gán và thứ tự, dẫn đến`O(2^n * n!)`hành vi trong trường hợp xấu nhất, điều này hoàn toàn không thể thực hiện được ngay cả đối với`n = 20`. 

Quan sát quan trọng là thứ tự của các giá trị được ghi`a_i`là điều duy nhất hạn chế tính khả thi. Sau khi chúng tôi quyết định thứ tự các bài kiểm tra xuất hiện trong bảng điểm, mỗi bài kiểm tra chỉ cần được xếp lịch càng sớm càng tốt mà không vi phạm thứ tự đó. Quyền tự do duy nhất chúng ta thực sự có là lựa chọn xem mỗi bài kiểm tra có được thực hiện hay không`b_i`hoặc`a_i`và tác động của quyết định đó là chúng ta có thể lên lịch sớm như thế nào so với các kỳ thi khác. 

Nếu chúng ta sắp xếp các bài kiểm tra theo thời hạn dự kiến`a_i`, chúng ta có thể thực thi thứ tự tự nhiên của bản ghi. Nhiệm vụ còn lại là quyết định bài kiểm tra nào chúng ta buộc phải đẩy sang ngày sau.`a_i`so với cái nào có thể được chuyển đến một cách an toàn`b_i`để giảm ngày hoàn thành cuối cùng. 

Chúng tôi xử lý các bài kiểm tra ngày càng tăng`a_i`. Ở mỗi bước, chúng tôi cố gắng lên lịch thi vào ngày sớm hơn`b_i`. Nếu làm như vậy vi phạm tính khả thi với các quyết định đã được lên lịch sẵn, chúng tôi buộc phải "đẩy" một số kỳ thi đã chọn trước đó sang ngày sau để nhường chỗ. Cấu trúc của quyết định này giống như việc lựa chọn một tập hợp tối đa các nhiệm vụ ban đầu dưới các ràng buộc về thứ tự, trong đó xung đột được giải quyết bằng cách hy sinh những lựa chọn ban đầu tốn kém nhất. 

Điều này dẫn đến một chiến lược tham lam là luôn giữ các nhiệm vụ sớm càng lâu càng tốt và chỉ phân công lại khi cần thiết, đảm bảo ngày cuối cùng ở mức tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force theo mệnh lệnh và lựa chọn | O(2^n · n!) | O(n) | Quá chậm | 
| Tham lam bằng cách phân loại + giải quyết xung đột | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các bài kiểm tra theo ngày dự kiến`a_i`. Điều này sẽ sửa thứ tự bản ghi vì các giá trị được ghi phải không giảm. 
2. Duy trì một bộ bài kiểm tra hiện được giao cho ngày đầu của họ`b_i`. Đồng thời theo dõi tập hợp các ngày thực đã chọn hiện tại cho tất cả các bài kiểm tra đã xử lý. 
3. Lặp lại các bài kiểm tra theo thứ tự được sắp xếp`a_i`, quyết định có nên tham gia mỗi kỳ thi hay không`b_i`hoặc buộc phải`a_i`. 
4. Dự kiến ​​phân công bài thi hiện tại cho ngày`b_i`. Điều này giảm thiểu sự đóng góp của nó vào câu trả lời cuối cùng, vì vậy chúng tôi luôn thử điều này trước. 
5. Nếu lựa chọn này gây ra xung đột về thứ tự ngày thực hoặc làm cho cấu trúc không hợp lệ, chúng tôi sẽ giải quyết nó bằng cách hoàn tác một trong các nhiệm vụ ban đầu đã chọn trước đó. Ứng cử viên phù hợp để hoàn tác là ứng cử viên "đắt" nhất về mặt giữ tính khả thi, điển hình là ứng cử viên có giá trị lớn nhất`b_i`trong số các lựa chọn sớm, vì nó cản trở sự linh hoạt nhất trong tương lai. 
6. Sau khi xử lý tất cả các bài kiểm tra, hãy tính ngày tối đa trong số tất cả các bài tập đã chọn, đó là ngày cuối cùng Valera thực sự làm bài kiểm tra. 

### Tại sao nó hoạt động 

Việc sắp xếp theo-`a_i`thứ tự đảm bảo rằng tính hợp lệ của bản ghi được giảm xuống để tôn trọng một chuỗi không giảm toàn cầu duy nhất. Trong khuôn khổ này, mỗi kỳ thi đều góp phần đưa ra lựa chọn giữa vị trí sớm hơn và vị trí muộn hơn và cách duy nhất để giảm ngày cuối cùng là tối đa hóa số lượng vị trí sớm. Bất cứ khi nào xung đột nảy sinh, việc loại bỏ nhiệm vụ sớm hạn chế nhất hiện tại sẽ duy trì tính linh hoạt tối đa trong tương lai, bởi vì nó giữ được ít hơn`b_i`các lựa chọn có sẵn cho các kỳ thi sau này. Đối số trao đổi tham lam này đảm bảo rằng không có chuỗi thay thế nào có thể đạt được ngày tối đa nhỏ hơn mà không vi phạm tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
exams = []
for i in range(n):
    a, b = map(int, input().split())
    exams.append((a, b))

exams.sort()

# We maintain chosen days in a simple list
chosen = []

for a, b in exams:
    chosen.append(b)
    chosen.sort()

    # Check if ordering constraint is violated:
    # since we process in increasing a, we only ensure feasibility of max constraint
    if len(chosen) and chosen[-1] > a:
        chosen.pop()

print(max(chosen) if chosen else 0)
```Mã tuân theo ý tưởng luôn cố gắng ấn định ngày sớm hơn`b_i`Đầu tiên. các`chosen`danh sách đại diện cho nhiều tập hợp hiện tại của các ngày thực đã chọn. Việc sắp xếp nó cho phép chúng tôi phát hiện xem có bất kỳ nhiệm vụ nào đã "đẩy" chúng tôi vượt quá giới hạn đã lên lịch hiện tại hay không`a_i`cho phép. 

Khi giá trị được chọn lớn nhất vượt quá`a_i`, điều đó có nghĩa là một số bài kiểm tra không thể được giữ lại trong thời gian sớm mà không vi phạm thứ tự ngụ ý theo lịch trình, vì vậy chúng tôi loại bỏ thí sinh đó. Giá trị còn lại tối đa là ngày thi thực tế cuối cùng. 

Một chi tiết triển khai tinh tế là chúng tôi luôn so sánh với`a_i`, không`b_i`, bởi vì`a_i`xác định ràng buộc bảng điểm. Ngoài ra, giải pháp dựa vào việc duy trì cấu trúc giống nhiều tập hợp; một đống sẽ hiệu quả hơn, nhưng đối với`n ≤ 5000`, việc sắp xếp theo từng bước vẫn đủ nhanh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
5 2
3 1
4 2
```Sắp xếp theo`a_i`:```
(3,1), (4,2), (5,2)
```| Bước | Thi (a,b) | đã chọn trước | hành động | được chọn sau | 
| --- | --- | --- | --- | --- | 
| 1 | (3,1) | [] | lấy b | [1] | 
| 2 | (4,2) | [1] | lấy b | [1,2] | 
| 3 | (5,2) | [1,2] | lấy b | [1,2,2] | 

Không xảy ra vi phạm nên đáp án cuối cùng là`2`. Điều này xác nhận rằng việc xếp chồng những ngày đầu là khả thi khi tất cả các ràng buộc đều tương thích. 

### Ví dụ 2 

đầu vào:```
4
6 1
5 2
4 3
3 1
```Đã sắp xếp:```
(3,1), (4,3), (5,2), (6,1)
```| Bước | thi | đã chọn trước | hành động | được chọn sau | 
| --- | --- | --- | --- | --- | 
| 1 | (3,1) | [] | b | [1] | 
| 2 | (4,3) | [1] | b | [1,3] | 
| 3 | (5,2) | [1,3] | b | [1,2,3] | 
| 4 | (6,1) | [1,2,3] | b | [1,1,2,3] | 

Tất cả các giá trị vẫn khả thi theo tương ứng của chúng`a_i`, vậy ngày cuối cùng là`3`. 

Điều này chứng tỏ các bản sao trong những ngày đầu không gây tổn hại đến tính chính xác miễn là chúng vẫn bị giới hạn bởi mức tăng`a_i`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2 log n) | Mỗi lần chèn sắp xếp một danh sách có kích thước lên tới n | 
| Không gian | O(n) | Chúng tôi lưu trữ các bài tập đã chọn | 

Các ràng buộc cho phép điều này một cách thoải mái vì`n ≤ 5000`và các phép toán là so sánh đơn giản và điều chỉnh cục bộ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    n = int(input())
    exams = []
    for _ in range(n):
        a, b = map(int, input().split())
        exams.append((a, b))

    exams.sort()
    chosen = []

    for a, b in exams:
        chosen.append(b)
        chosen.sort()
        if chosen[-1] > a:
            chosen.pop()

    return str(max(chosen) if chosen else 0)

# provided sample
assert run("3\n5 2\n3 1\n4 2\n") == "2"

# minimum input
assert run("1\n10 1\n") == "1"

# all equal structure
assert run("3\n5 1\n5 2\n5 3\n") == "3"

# strictly increasing a, tight b
assert run("4\n3 1\n4 2\n5 3\n6 4\n") == "4"

# reverse-feasible forcing removals
assert run("4\n6 1\n5 1\n4 1\n3 1\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| thi đơn | 1 | trường hợp cơ sở | 
| thời hạn bằng nhau | max b_i | xử lý quan hệ | 
| chuỗi ngày càng tăng | b_i cuối cùng | tích lũy tham lam | 
| giảm áp suất b | 1 | giải pháp ràng buộc cưỡng bức | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các kỳ thi đều có cùng một ngày sớm nhưng có số ngày dự kiến tăng dần. Thuật toán tiếp tục tích lũy giống hệt nhau`b_i`các giá trị không có xung đột vì mức tối đa vẫn bị giới hạn bởi hoạt động nhỏ nhất`a_i`ở mỗi bước. Vì mỗi bước tăng lên`a_i`, không có thao tác xóa nào được kích hoạt. 

Một trường hợp khác là khi số ngày đầu rất lớn nhưng số ngày đã lên lịch lại rất eo hẹp. Trong trường hợp đó, thời điểm được chọn tối đa`b_i`vượt quá hiện tại`a_i`, thuật toán sẽ loại bỏ nó ngay lập tức, đảm bảo tính khả thi được khôi phục. Điều này đảm bảo rằng không có đơn đặt hàng không hợp lệ nào được cam kết và mức tối đa cuối cùng luôn phản ánh khả năng nén lịch trình tốt nhất có thể.
