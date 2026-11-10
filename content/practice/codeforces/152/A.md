---
title: "CF 152A - Nhãn hiệu"
description: "Chúng ta được yêu cầu tìm số học sinh trong lớp giỏi nhất ít nhất một môn học. Mỗi học sinh có điểm cho nhiều môn học, với mỗi điểm là một số có một chữ số từ 1 đến 9."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 152
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 108 (Div. 2)"
rating: 900
weight: 152
solve_time_s: 68
verified: true
draft: false
---

[CF 152A - Dấu](https://codeforces.com/problemset/problem/152/A) 

**Xếp hạng:** 900 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tìm số học sinh trong lớp giỏi nhất ít nhất một môn học. Mỗi học sinh có điểm cho nhiều môn học, với mỗi điểm là một số có một chữ số từ 1 đến 9. Dữ liệu đầu vào biểu thị điểm dưới dạng lưới có kích thước _n_ x _m_, trong đó mỗi hàng tương ứng với một học sinh và mỗi cột tương ứng với một môn học. Một học sinh được coi là thành công nếu, trong ít nhất một cột, điểm của họ khớp với điểm tối đa của tất cả học sinh cho môn học đó. 

Các ràng buộc rất khiêm tốn: cả số lượng học sinh và số lượng đối tượng tối đa là 100. Điều này có nghĩa là một giải pháp có vòng lặp kép đối với học sinh và đối tượng, tối đa là 10.000 thao tác, sẽ chạy thoải mái trong giới hạn thời gian 1 giây. Các trường hợp đặc biệt mà chúng tôi phải theo dõi bao gồm các tình huống trong đó tất cả học sinh có điểm giống nhau, chỉ có một học sinh đứng đầu ở mọi môn học hoặc điểm tối đa trong một môn học xuất hiện nhiều lần. Một cách tiếp cận bất cẩn có thể chỉ tính một học sinh nếu họ là học sinh tối đa duy nhất trong một môn học, điều này sẽ dẫn đến câu trả lời sai. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ đơn giản là kiểm tra từng học sinh ở mọi môn học, theo dõi điểm tối đa của môn học đó. Khi chúng tôi đạt được mức tối đa, chúng tôi đánh dấu tất cả những học sinh đã đạt được mức đó. Điều này bao gồm việc lặp lại từng cột để tìm điểm tối đa, sau đó lặp lại từng học sinh để kiểm tra xem chúng có khớp với điểm tối đa đó hay không. Tính chính xác được đảm bảo vì chúng tôi so sánh rõ ràng từng học sinh ở mọi môn học. 

Lực lượng vũ phu này đã tối ưu cho những hạn chế này. Thông tin chi tiết quan trọng là chúng ta không cần sắp xếp hoặc sử dụng các cấu trúc dữ liệu bổ sung. Chúng ta có thể chỉ cần duy trì một tập hợp các chỉ số về những học sinh đạt thành tích, cập nhật nó bất cứ khi nào một học sinh đạt điểm tối đa cho một môn học. Cấu trúc bài toán làm cho điều này trở nên khả thi vì lưới nhỏ và phép tính tối đa trên mỗi cột rẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n * m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các giá trị đầu vào cho số học sinh _n_ và số môn học _m_, sau đó đọc _n_ dòng tiếp theo dưới dạng chuỗi biểu thị điểm học sinh. Mỗi dòng được chuyển đổi thành một danh sách để lập chỉ mục dễ dàng hơn. 
2. Khởi tạo một bộ để theo dõi những học sinh thành công. Việc sử dụng một bộ sẽ tránh được việc đếm hai lần và cho phép chèn hiệu quả. 
3. Lặp lại từng chỉ mục chủ đề từ 0 đến _m-1_. Đối với mỗi môn học, hãy tìm điểm cao nhất của tất cả học sinh bằng cách lặp lại các hàng của cột đó. 
4. Sau khi xác định được điểm tối đa cho môn học hiện tại, hãy lặp lại trên tất cả học sinh và thêm vào tập hợp thành công bất kỳ học sinh nào có điểm khớp với điểm tối đa của môn học đó. 
5. Sau khi xử lý tất cả các môn học, kích thước của tập hợp thành công thể hiện tổng số học sinh thành công. In giá trị này. 

Tại sao nó hoạt động: Bằng cách lặp lại từng cột, chúng tôi đảm bảo rằng mọi chủ đề đều được kiểm tra để tìm ra (những) chủ đề có hiệu suất cao nhất. Bộ này đảm bảo rằng mỗi học sinh chỉ được tính một lần bất kể học sinh đó đứng đầu bao nhiêu môn. Thuật toán không thể bỏ sót bất kỳ học sinh nào đạt thành tích cao vì mọi so sánh điểm số của học sinh đều diễn ra một cách rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
grades = [list(input().strip()) for _ in range(n)]
successful = set()

for col in range(m):
    max_grade = max(grades[row][col] for row in range(n))
    for row in range(n):
        if grades[row][col] == max_grade:
            successful.add(row)

print(len(successful))
```Mã đầu tiên đọc kích thước và lưới lớp. Điểm của mỗi học sinh được lưu trữ dưới dạng danh sách các ký tự. Vòng lặp chính lặp qua từng chủ đề (cột). Đối với mỗi cột, điểm tối đa được tính bằng cách sử dụng biểu thức tạo, sau đó một vòng lặp khác đánh dấu tất cả học sinh đã đạt được điểm đó. Cuối cùng, độ dài của bộ này cho biết số học sinh thành công. Sự tinh tế là việc xử lý các điểm dưới dạng chuỗi vì chúng là các ký tự một chữ số; điều này hiệu quả vì so sánh ký tự cho các chữ số cho cùng thứ tự như so sánh số nguyên. Sử dụng một bộ sẽ tránh được sự trùng lặp mà không cần ghi sổ bổ sung. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
3 3
223
232
112
```Bảng theo dõi: 

| col | max_grade | đặt thành công | 
| --- | --- | --- | 
| 0 | 2 | {0, 1} | 
| 1 | 3 | {0, 1} | 
| 2 | 3 | {0, 1} | 

Giải thích: Học sinh 0 đạt điểm cao nhất môn 0, học sinh 1 đạt điểm cao nhất môn 1, cả hai đều được cộng vào tập thành công. Học sinh 2 không bao giờ đạt mức tối đa. 

**Mẫu 2** 

đầu vào:```
2 2
99
99
```Bảng theo dõi: 

| col | max_grade | đặt thành công | 
| --- | --- | --- | 
| 0 | 9 | {0, 1} | 
| 1 | 9 | {0, 1} | 

Cả hai học sinh đều thành công ở cả hai môn học. Thuật toán chỉ đếm chính xác mỗi lần một lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * m) | Mỗi lần lặp lại môn học yêu cầu quét tất cả học sinh để tìm tối đa và tìm lại để đánh dấu những học sinh thành công | 
| Không gian | O(n) | Bộ lưu trữ chỉ số của tối đa n sinh viên | 

Với n và m lên tới 100, tổng số thao tác tối đa là 10.000, dễ dàng nằm gọn trong giới hạn 1 giây. Việc sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    grades = [list(input().strip()) for _ in range(n)]
    successful = set()
    for col in range(m):
        max_grade = max(grades[row][col] for row in range(n))
        for row in range(n):
            if grades[row][col] == max_grade:
                successful.add(row)
    return str(len(successful))

# Provided samples
assert run("3 3\n223\n232\n112\n") == "2", "sample 1"
assert run("2 2\n99\n99\n") == "2", "sample 2"

# Custom cases
assert run("1 1\n5\n") == "1", "single student and subject"
assert run("3 3\n111\n111\n111\n") == "3", "all equal values"
assert run("2 5\n12345\n54321\n") == "2", "top in multiple subjects"
assert run("4 4\n9123\n1294\n2912\n3219\n") == "4", "multiple winners"
assert run("100 1\n" + "\n".join("9" for _ in range(100)) + "\n") == "100", "maximum n edge case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1\n5\n | 1 | Đầu vào kích thước tối thiểu | 
| 3 3\n111\n111\n111\n | 3 | Giá trị hoàn toàn bằng nhau | 
| 2 5\n12345\n54321\n | 2 | Nhiều môn học với những người chiến thắng khác nhau | 
| 4 4\n9123\n1294\n2912\n3219\n | 4 | Sự kết hợp phức tạp của người chiến thắng | 
| 100 1\n9 lặp lại | 100 | Số lượng sinh viên tối đa | 

## Vỏ cạnh 

Khi tất cả học sinh có điểm giống nhau, mọi học sinh đều được coi là thành công. Đối với đầu vào:```
3 3
111
111
111
```Điểm tối đa trong mỗi môn học là 1. Lặp lại từng cột, mỗi học sinh khớp với điểm tối đa và được thêm vào tập hợp. Số cuối cùng là 3, như mong đợi. Điều này xác nhận thuật toán xử lý các trường hợp ràng buộc một cách chính xác. 

Đối với một học sinh học nhiều môn:```
1 5
12345
```Học sinh này khá thành công trong tất cả các môn học. Bộ này đảm bảo học sinh được tính một lần, cho kết quả đầu ra là 1. Điều này xác nhận thuật toán xử lý chính xác các trường hợp cạnh có kích thước tối thiểu.
