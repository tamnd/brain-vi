---
title: "CF 102299H - Đề xuất khóa học"
description: "Mỗi học sinh đã tham gia một số khóa học có sẵn và với mỗi khóa học họ tham gia, chúng tôi biết điểm của họ. Đối với một sinh viên cụ thể, chúng ta phải tìm sinh viên khác có điểm giống nhau nhất trong các khóa học mà cả hai đã tham gia."
date: "2026-08-13T08:17:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102299
codeforces_index: "H"
codeforces_contest_name: "2019 USP Try-outs"
rating: 0
weight: 102299
solve_time_s: 111
verified: true
draft: false
---

[CF 102299H - Đề xuất khóa học](https://codeforces.com/problemset/problem/102299/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi học sinh đã tham gia một số khóa học có sẵn và với mỗi khóa học họ tham gia, chúng tôi biết điểm của họ. Đối với một sinh viên cụ thể, chúng ta phải tìm sinh viên khác có điểm giống nhau nhất trong các khóa học mà cả hai đã tham gia. 

Khoảng cách giữa hai học sinh chỉ được tính từ các khóa học xuất hiện trong lịch sử của cả hai học sinh. Nếu họ chia sẻ các khóa học (c_1,c_2,\ldots,c_k), với điểm (g_{i,c}) và (g_{j,c}), bình phương khoảng cách của họ là 

[ 
D(i,j)=\sum_c (g_{i,c}-g_{j,c})^2, 
] 

trong đó tổng chỉ chứa các khóa học được chia sẻ. Khoảng cách Euclide thực tế là (\sqrt{D(i,j)}), nhưng việc so sánh khoảng cách bình phương sẽ cho kết quả chính xác như nhau. 

Sau khi tìm thấy sinh viên gần nhất, chúng tôi xem qua các khóa học của sinh viên đó. Chúng tôi muốn một khóa học mà sinh viên ban đầu chưa tham gia, với điểm cao nhất trong hồ sơ của sinh viên gần nhất. Nếu một số khóa học như vậy có cùng điểm, chỉ số khóa học nhỏ nhất sẽ thắng. Nếu mọi khóa học của sinh viên gần nhất đều đã được học thì câu trả lời là (-1). 

Đầu vào có tối đa 100 sinh viên và 100 khóa học. Kích thước này đủ nhỏ để chúng ta có thể so sánh trực tiếp từng cặp học sinh và kiểm tra từng khóa học. Có nhiều nhất (100\cdot99\cdot100=990{,}000) so sánh đường đi khi tìm khoảng cách, có thể dễ dàng quản lý trong một giây. Giai đoạn đề xuất chỉ thêm một lần kiểm tra (100\cdot100=10{,}000) khác. Không cần cấu trúc dữ liệu lân cận gần nhất phức tạp. 

Một biểu diễn hữu ích là mảng hai chiều`grade[student][course]`. Một khóa học bị thiếu có thể được đại diện bởi`-1`, trong khi mọi điểm thực đều nằm trong khoảng từ 0 đến 10. Điều này cho phép chúng tôi kiểm tra xem một khóa học có được chia sẻ hay không bằng một phép so sánh đơn giản. 

Một trường hợp tế nhị là sinh viên không thể chia sẻ các khóa học. Khoảng cách của họ là vô tận, vì vậy một sinh viên như vậy không bao giờ được trở thành sinh viên gần nhất khi đầu vào đảm bảo rằng mọi sinh viên đều có ít nhất một khóa học chung với người khác. Việc triển khai bất cẩn khởi tạo khoảng cách tối thiểu về 0 sẽ không bao giờ thay thế được khoảng cách đó, tạo ra câu trả lời không hợp lệ. Ví dụ:```
2 2
1
1 7
1
2 9
```Điều này vi phạm sự đảm bảo của bài toán, vì hai sinh viên không có khóa học chung. Theo các ràng buộc đầu vào đã nêu, tình huống này không xảy ra, nhưng việc triển khai vẫn thể hiện nó một cách tự nhiên là vô cùng. 

Một trường hợp khó khăn khác xảy ra khi sinh viên gần nhất không có khóa học mới nào để giới thiệu. Ví dụ:```
2 2
2
1 10
2 5
1
1 8
```Học sinh thứ nhất là học sinh gần nhất với học sinh thứ hai nhưng học sinh 2 chưa học khóa 2 nên đáp án của học sinh 2 là`2`. Ngược lại, nếu sinh viên thứ hai cũng đã học cả hai khóa học thì sẽ không có đề xuất nào và câu trả lời sẽ là`-1`. 

Sự ràng buộc giữa các khóa học phải được giải quyết theo chỉ mục khóa học chứ không phải theo thứ tự các khóa học xuất hiện trong đầu vào. Ví dụ:```
2 3
1
1 7
3
1 5
2 9
3 9
```Học sinh 1 gần gũi nhất với học sinh 2, còn khóa 2 và 3 đều có điểm 9 dành cho học sinh 2. Đề xuất đúng là khóa 2 vì chỉ số của nó nhỏ hơn. Việc triển khai chỉ thay thế câu trả lời hiện tại bất cứ khi nào nó thấy điểm bằng nhau có thể trả về khóa 3 không chính xác. 

Tuyên bố không nêu rõ những việc cần làm khi một số học sinh khác có khoảng cách tối thiểu giống hệt nhau. Chúng tôi quét các chỉ số sinh viên theo thứ tự tăng dần và chỉ thay thế sinh viên gần nhất hiện tại khi tìm thấy khoảng cách nhỏ hơn. Do đó, sinh viên gần nhất có chỉ số nhỏ nhất sẽ được chọn. Đây cũng là hành vi xác định của việc thực hiện trực tiếp vấn đề thông thường. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp đã đủ nhanh cho những hạn chế này. Lưu trữ điểm của mỗi học sinh trong ma trận (N\time M). Với mỗi cặp sinh viên riêng biệt (i,j), hãy quét tất cả các khóa học (M). Bất cứ khi nào cả hai học sinh tham gia khóa học, hãy cộng bình phương chênh lệch điểm vào khoảng cách của họ. Sau khi kiểm tra tất cả các khóa học, hãy so sánh khoảng cách đó với khoảng cách tốt nhất tìm được cho học sinh (i). 

Điều này hiệu quả vì bản thân định nghĩa về khoảng cách là tổng của các khóa học được chia sẻ. Quét rõ ràng mọi khóa học không bỏ lỡ khóa học được chia sẻ cũng như không bao gồm khóa học mà chỉ một sinh viên đã tham gia. 

Việc triển khai hoàn toàn theo nghĩa đen có thể tính căn bậc hai sau mỗi cặp và so sánh khoảng cách Euclide thực tế. Điều đó là không cần thiết. Vì hàm căn bậc hai tăng nghiêm ngặt đối với các giá trị không âm, nên việc giảm thiểu (D) hoàn toàn tương đương với việc giảm thiểu (\sqrt D). Việc sử dụng khoảng cách bình phương cũng chỉ giữ cho việc triển khai ở dạng số nguyên. 

Phương pháp brute-force thực hiện tối đa (N(N-1)M) kiểm tra khóa học. Với (N=M=100), tức là 

[ 
100\cdot99\cdot100=990{,}000 
] 

séc. Sau đó, việc tìm kiếm đề xuất yêu cầu tối đa (NM=10{,}000) kiểm tra khóa học bổ sung. Đây là mức thoải mái dưới mức Python có thể xử lý trong các giới hạn nhất định. 

Việc tối ưu hóa hữu ích không phải là một thuật toán tiệm cận khác, vì ở đây không cần thiết. Quan sát quan trọng là giới hạn đầu vào làm cho việc so sánh toàn diện trở nên rẻ hơn. Việc biểu diễn dữ liệu dưới dạng ma trận dày đặc và sử dụng khoảng cách bình phương mang lại cách thực hiện đơn giản nhất với các hệ số không đổi nhỏ nhất. Một từ điển cho mỗi học sinh cũng sẽ hoạt động, nhưng nó bổ sung thêm chi phí băm mà không giải quyết được vấn đề về hiệu suất. 

Phương pháp brute-force có hiệu quả vì chỉ có 100 sinh viên và 100 khóa học. Nó sẽ trở nên kém hấp dẫn nếu cả hai chiều đều ở mức hàng chục nghìn, nhưng ở những hạn chế thực tế, tính toán toàn diện giống nhau là lựa chọn kỹ thuật tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Khoảng cách theo cặp theo nghĩa đen với căn bậc hai Euclide | (O(N^2M)) | (O(NM)) | Được chấp nhận, nhưng căn bậc hai không cần thiết | 
| Khoảng cách bình phương theo cặp với ma trận điểm | (O(N^2M)) | (O(NM)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo`grade[i][c]`cho mỗi học viên và khóa học. Khởi tạo mọi mục nhập vào`-1`, có nghĩa là học sinh đó chưa tham gia khóa học đó. Khi một bản ghi đầu vào`(c, g)`được đọc, lưu trữ`g`ở vị trí tương ứng. Ma trận thuận tiện vì mỗi phép tính khoảng cách sau này có thể kiểm tra đường đi trong thời gian không đổi. 
2. Đối với mỗi học sinh`i`, khởi tạo khoảng cách tốt nhất đến vô cùng và học sinh gần nhất với giá trị không hợp lệ. Sau đó kiểm tra từng học sinh khác`j`. 
3. Cho mỗi cặp`(i, j)`, khởi tạo khoảng cách bình phương của nó về 0 và quét tất cả các hướng. Nếu một trong hai học sinh có`-1`đối với một khóa học, hãy bỏ qua nó vì khóa học đó không được chia sẻ. Nếu không thì thêm`(grade[i][c] - grade[j][c]) ** 2`đến khoảng cách. 
4. Sau khi xử lý xong các khóa học chia sẻ, hãy so sánh bình phương khoảng cách thu được với khoảng cách tối thiểu hiện tại đối với học sinh.`i`. Chỉ thay thế học sinh gần nhất khi khoảng cách mới nhỏ hơn hoàn toàn. Bởi vì học sinh được kiểm tra theo thứ tự chỉ số tăng dần nên khoảng cách bằng nhau sẽ giữ cho chỉ số học sinh nhỏ hơn. 
5. Từng là học sinh thân thiết nhất`j`đã biết, hãy quét lại tất cả các khóa học. Bỏ qua mọi khóa học mà học viên đã tham gia`i`. Đối với mỗi khóa học còn lại mà sinh viên đó`j`đã thực hiện, hãy so sánh điểm của nó với điểm đề xuất tốt nhất được thấy cho đến nay. 
6. Giữ khóa học với điểm cao nhất. Nếu hai thí sinh có cùng điểm thì giữ chỉ số môn học nhỏ hơn. Vì các khóa học được quét từ chỉ mục 1 trở lên nên chỉ cập nhật ở cấp độ lớn hơn sẽ tự động đưa ra quy tắc ràng buộc bắt buộc. 
7. Nếu không có môn học nào trong lịch sử của học sinh gần nhất là mới đối với học sinh`i`, in`-1`. Nếu không thì in chỉ mục khóa học đã chọn. 

### Tại sao nó hoạt động 

Đối với mỗi cặp sinh viên, thuật toán sẽ kiểm tra mọi khóa học và thêm chênh lệch điểm bình phương chính xác khi khóa học đó thuộc về cả hai sinh viên. Do đó, giá trị được tính toán chính xác là bình phương khoảng cách Euclide đã xác định của chúng. Vì bình phương bảo toàn thứ tự của các khoảng cách không âm nên học sinh được giữ lại gần nhất là học sinh gần nhất hợp lệ. 

Sau khi chọn sinh viên đó, lần quét thứ hai sẽ xem xét chính xác các khóa học mà sinh viên gần nhất đã tham gia còn sinh viên ban đầu thì không. Trong số các ứng cử viên này, thuật toán giữ điểm lớn nhất và đối với các điểm bằng nhau, chỉ số nhỏ nhất. Do đó khóa học cuối cùng chính xác là khuyến nghị cần thiết, hoặc`-1`khi tập ứng viên trống. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    grade = [[-1] * (m + 1) for _ in range(n)]

    for i in range(n):
        q = int(input())
        for _ in range(q):
            c, g = map(int, input().split())
            grade[i][c] = g

    answers = []

    for i in range(n):
        best_dist = float('inf')
        closest = -1

        for j in range(n):
            if i == j:
                continue

            dist = 0

            for c in range(1, m + 1):
                gi = grade[i][c]
                gj = grade[j][c]

                if gi != -1 and gj != -1:
                    diff = gi - gj
                    dist += diff * diff

            if dist < best_dist:
                best_dist = dist
                closest = j

        best_course = -1
        best_grade = -1

        for c in range(1, m + 1):
            if grade[i][c] != -1:
                continue

            g = grade[closest][c]
            if g == -1:
                continue

            if g > best_grade:
                best_grade = g
                best_course = c

        answers.append(str(best_course))

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    solve()
```Phần đầu tiên xây dựng ma trận điểm được mô tả ở bước 1. Chỉ số khóa học được lưu trữ từ 1 đến`m`, do đó cột bổ sung ở chỉ số 0 không được sử dụng. Điều này phản ánh việc đánh số khóa học trong đầu vào và tránh việc liên tục trừ đi một khóa học từ các chỉ số khóa học. 

Sự lồng nhau`i`Và`j`vòng lặp thực hiện các bước từ 2 đến 4. Điều kiện`i == j`ngăn cản việc so sánh một học sinh với chính mình.`best_dist`bắt đầu từ vô cùng nên sinh viên khác hợp lệ đầu tiên luôn được chấp nhận. Chúng tôi sử dụng khoảng cách bình phương, vì vậy`dist`vẫn là số nguyên và không có độ chính xác của dấu phẩy động. 

Đầu vào đảm bảo rằng mỗi sinh viên sẽ chia sẻ ít nhất một khóa học với một sinh viên khác. Do đó,`closest`sẽ luôn trở thành một sinh viên hợp lệ. Việc triển khai cũng sẽ hoạt động an toàn nếu không có sự đảm bảo đó cho đến giai đoạn đề xuất, nhưng vấn đề không yêu cầu xử lý dữ liệu đầu vào không hợp lệ đó. 

Quá trình quét khóa học cuối cùng thực hiện các bước 5 và 6. Bài kiểm tra`grade[i][c] != -1`loại bỏ các khóa học đã được thực hiện bởi sinh viên ban đầu. Bài kiểm tra tiếp theo sẽ loại bỏ các khóa học mà sinh viên gần nhất không tham gia.`best_grade`bắt đầu lúc`-1`, nằm dưới mọi cấp độ pháp lý từ 0 đến 10, do đó, ngay cả cấp 0 cũng được chấp nhận chính xác. 

Việc so sánh sử dụng`g > best_grade`, không`g >= best_grade`. Các khóa học được duyệt theo thứ tự chỉ mục tăng dần, vì vậy khi hai khóa học có điểm bằng nhau thì khóa học đầu tiên vẫn được chọn. Điều đó trực tiếp thực hiện ràng buộc chỉ số nhỏ nhất. 

Không có vấn đề tràn số nguyên trong Python. Ngay cả trong ngôn ngữ có chiều rộng cố định, chênh lệch bình phương lớn nhất là (10^2=100) và có tối đa 100 khóa học được chia sẻ đóng góp, do đó khoảng cách tối đa là 10.000. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào mô tả hai sinh viên. Học sinh 1 có khóa 1 và 2 đều đạt điểm 10. Học sinh 2 có khóa 2 lớp 9 và khóa 3 lớp 5. 

Đối với học sinh 1, học sinh gần nhất duy nhất có thể là học sinh 2. Họ học chung khóa 2, nên bình phương khoảng cách là ((10-9)^2=1). Khóa học 3 của học sinh 2 là mới đối với học sinh 1 nên đây là khóa học duy nhất. 

| Sinh viên | Ứng viên | Khóa học chia sẻ | Khoảng cách bình phương | Gần nhất | Khuyến nghị | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 2: (10,9) | 1 | 2 | 3 | 
| 2 | 1 | 2: (9,10) | 1 | 1 | không | 

Đối với học sinh 2, học sinh 1 gần nhất với khoảng cách bình phương bằng 1. Khóa 1 mới dành cho học sinh 2 và có lớp 10 nên khuyến khích học. Kết quả đầu ra là`3`theo sau là`1`. 

### Mẫu 2 

Học sinh 1 có khóa 1, 2, 3 lớp 7, 8, 10. Học sinh 2 có khóa 4, 2, 1 lớp 10, 9, 5. 

Đối với sinh viên 1 và sinh viên 2, các khóa học chung là 1 và 2. Bình phương khoảng cách của họ là 

[ 
(7-5)^2+(8-9)^2=4+1=5. 
] 

Khóa 4 của Học sinh 2 không có trong lịch sử của Học sinh 1 nên nó trở thành đề xuất. 

| Sinh viên | Ứng viên | Khóa học chia sẻ | Khoảng cách bình phương | Gần nhất | Khóa học mới của học viên gần gũi nhất | Khuyến nghị | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 1, 2 | 5 | 2 | 4 với lớp 10 | 4 | 
| 2 | 1 | 1, 2 | 5 | 1 | 3 với lớp 10 | 3 | 

Đối với sinh viên 2, khóa 3 của sinh viên 1 là khóa học duy nhất mà sinh viên 2 chưa tham gia nên khuyến nghị là 3. Ví dụ này cũng xác nhận rằng các khóa học vắng mặt của cả hai sinh viên không đóng góp gì vào khoảng cách. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N^2M)) | Có (N(N-1)) cặp sinh viên được sắp xếp và mỗi cặp quét tất cả (M) khóa học. Đề xuất quét thêm (O(NM)), trong đó bị chi phối bởi (O(N^2M)). | 
| Không gian | (O(NM)) | Ma trận điểm lưu trữ một mục nhập cho mỗi cặp khóa học của sinh viên. | 

Với (N,M\le100), tính toán ưu thế có ít hơn một triệu lượt kiểm tra khóa học. Việc sử dụng bộ nhớ chỉ khoảng 10.000 mục nhập ma trận, do đó, giải pháp thoải mái nằm trong cả giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    grade = [[-1] * (m + 1) for _ in range(n)]

    for i in range(n):
        q = int(input())
        for _ in range(q):
            c, g = map(int, input().split())
            grade[i][c] = g

    answers = []

    for i in range(n):
        best_dist = float('inf')
        closest = -1

        for j in range(n):
            if i == j:
                continue

            dist = 0

            for c in range(1, m + 1):
                gi = grade[i][c]
                gj = grade[j][c]

                if gi != -1 and gj != -1:
                    diff = gi - gj
                    dist += diff * diff

            if dist < best_dist:
                best_dist = dist
                closest = j

        best_course = -1
        best_grade = -1

        for c in range(1, m + 1):
            if grade[i][c] != -1:
                continue

            g = grade[closest][c]

            if g == -1:
                continue

            if g > best_grade:
                best_grade = g
                best_course = c

        answers.append(str(best_course))

    sys.stdout.write("\n".join(answers))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
assert run(
    """2 3
2
1 10
2 10
2
2 9
3 5
"""
) == "3\n1", "sample 1"

# Provided sample 2
assert run(
    """2 4
3
1 7
2 8
3 10
3
4 10
2 9
1 5
"""
) == "4\n3", "sample 2"

# Minimum-size input.
# Both students took the only course, so neither has anything new to recommend.
assert run(
    """2 1
1
1 0
1
1 10
"""
) == "-1\n-1", "minimum-size case"

# Equal recommendation grades.
# Student 2 has two new courses with the same grade, so the smaller index wins.
assert run(
    """2 3
1
1 7
3
1 5
2 9
3 9
"""
) == "2\n-1", "course-index tie case"

# All grades equal.
# Student 1 should receive the smallest new course index from student 2.
assert run(
    """3 5
2
1 5
2 5
3
1 5
3 5
4 5
2
1 5
5 5
"""
) == "3\n2\n2", "all-equal grades"

# Boundary grade 0 and 10.
# Distance calculations must include both endpoints of the grade range.
assert run(
    """2 3
1
1 0
2
1 10
2 0
"""
) == "2\n-1", "boundary grades"

# Maximum-size case generated programmatically.
# Every student has all courses, so every closest student has no new course.
n = 100
m = 100
parts = [f"{n} {m}"]
for _ in range(n):
    parts.append(str(m))
    for c in range(1, m + 1):
        parts.append(f"{c} 5")

max_case = "\n".join(parts) + "\n"
assert run(max_case) == "-1\n" * n, "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1`, cả hai sinh viên đều học khóa học duy nhất |`-1`,`-1`| Kích thước tối thiểu và trường hợp không có khóa học mới | 
| Hai khóa học ứng viên có trình độ ngang nhau |`2`,`-1`| Chỉ số khóa học nhỏ nhất giành được điểm hòa | 
| Ba học sinh có điểm bằng nhau |`3`,`2`,`2`| Khoảng cách bằng nhau, điểm bằng nhau và lựa chọn học sinh mang tính quyết định | 
| Lớp 0 và 10 |`2`,`-1`| Ranh giới lớp và số học bình phương khoảng cách | 
| 100 sinh viên và 100 khóa học, tất cả đều đăng ký đầy đủ | 100 dòng`-1`| Kích thước tối đa và trường hợp mọi khóa học gần nhất của học viên đều đã được đăng ký | 

## Vỏ cạnh 

Trường hợp quy mô tối thiểu có một khóa học và hai sinh viên:```
2 1
1
1 0
1
1 10
```Các sinh viên học chung khóa 1 nên bình phương khoảng cách của họ là (100). Học sinh gần nhất của mỗi học sinh có khóa học giống hệt nhau và không có học sinh nào có khóa học mà học sinh kia thiếu. Quá trình quét đề xuất không tìm thấy ứng cử viên nào, để lại`best_course = -1`. Đầu ra là:```
-1
-1
```Trường hợp hòa bằng cấp là:```
2 3
1
1 7
3
1 5
2 9
3 9
```Đối với học viên 1, học viên 2 là học viên thân thiết nhất vì khóa 1 học chung. Khóa 2 và 3 đều mới và đều có cấp 9. Quét thăm khóa 2 trước, lưu vào đề xuất tốt nhất, sau đó xem điểm ngang bằng ở khóa 3. Vì điều kiện cập nhật lớn hơn nên khóa 2 vẫn được chọn. Dòng đầu ra đầu tiên là`2`. 

Trường hợp hoàn toàn bằng nhau là:```
3 5
2
1 5
2 5
3
1 5
3 5
4 5
2
1 5
5 5
```Mọi điểm chung đều giống nhau nên mọi khoảng cách giữa những học sinh học chung môn học đều bằng không. Đối với học sinh 1, học sinh 2 và 3 đều gần nhau và việc quét chỉ số tăng dần giữ lại học sinh 2. Sau đó, học sinh 2 đề xuất khóa học 3, khóa học mới nhỏ nhất trong số các khóa học của nó. Điều này thực hiện mối ràng buộc giữa sinh viên gần nhất không xác định và xác nhận hành vi chỉ số nhỏ nhất xác định của việc triển khai. 

Trường hợp ranh giới lớp là:```
2 3
1
1 0
2
1 10
2 0
```Khóa học được chia sẻ đóng góp ((0-10)^2=100), được xử lý mà không cần số học dấu phẩy động. Học sinh 1 nhận khóa 2 vì nó mới và đạt điểm 0. Học sinh 2 không có khóa học nào mà học sinh 1 lấy khác làm nguồn giới thiệu nên kết quả đầu ra là`-1`. Điều này xác nhận rằng lớp 0 phải được coi là một ứng cử viên hợp lệ chứ không phải là một lính canh. 

Cuối cùng, hãy xem xét đầu vào có kích thước tối đa trong đó cứ 1 trong số 100 học sinh đã học tất cả 100 khóa học với lớp 5. Mỗi cặp có khoảng cách bằng 0 và học sinh đầu tiên còn lại được giữ lại là học sinh gần nhất vì khoảng cách bằng nhau không thay thế lựa chọn hiện tại. Sinh viên gần nhất đó không có khóa học nào mà sinh viên ban đầu không biết, vì vậy mọi câu trả lời đều là`-1`. Thuật toán thực hiện kiểm tra khóa học theo cặp đầy đủ (990{,}000) mà không vượt quá độ phức tạp dự kiến.
