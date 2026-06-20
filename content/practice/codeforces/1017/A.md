---
title: "CF 1017A - Thứ hạng"
description: "Chúng ta có một lớp học sinh, mỗi học sinh được xác định bằng một id số nguyên duy nhất bắt đầu từ 1. Mỗi học sinh có bốn điểm thi tương ứng với các môn học khác nhau. Nhiệm vụ là xếp hạng tất cả học sinh theo tổng số điểm của họ trong bốn môn học."
date: "2026-06-16T22:09:13+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1017
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 502 (in memory of Leopoldo Taravilse, Div. 1 + Div. 2)"
rating: 800
weight: 1017
solve_time_s: 90
verified: true
draft: false
---

[CF 1017A - Thứ hạng](https://codeforces.com/problemset/problem/1017/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 30 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lớp học sinh, mỗi học sinh được xác định bằng một id số nguyên duy nhất bắt đầu từ 1. Mỗi học sinh có bốn điểm thi tương ứng với các môn học khác nhau. Nhiệm vụ là xếp hạng tất cả học sinh theo tổng số điểm của họ trong bốn môn học. Tổng số điểm cao hơn có nghĩa là thứ hạng tốt hơn. Khi hai học sinh có tổng điểm bằng nhau, tỉ số hòa sẽ bị phá vỡ bằng cách cho học sinh có id nhỏ hơn xếp hạng cao hơn. 

Đầu ra chúng ta cần là thứ hạng của học sinh có id 1, Thomas. Thứ hạng được xác định là vị trí trong thứ tự sắp xếp này bắt đầu từ 1. 

Các hạn chế là nhỏ, tối đa là 1000 sinh viên. Điều này ngay lập tức cho chúng ta biết rằng cách tiếp cận O(n²) là hoàn toàn ổn và ngay cả giải pháp O(n log n) dựa trên sắp xếp đơn giản cũng là quá mức cần thiết nhưng rõ ràng và an toàn. Không cần tối ưu hóa ngoài việc tổng hợp và đặt hàng đơn giản. 

Sự tinh tế chính trong vấn đề này là quy tắc ràng buộc. Nếu chúng ta quên rằng id nhỏ hơn sẽ thắng khi tổng bằng nhau, chúng ta có thể xếp Thomas sau người có cùng số điểm một cách không chính xác. Một điểm tinh tế khác là thứ hạng phụ thuộc vào vị trí trong danh sách được sắp xếp cuối cùng, không tính số lượng người có điểm cao hơn chỉ sau khi sắp xếp không chính xác. Việc triển khai ngây thơ chỉ tính tổng số cao hơn mà không xử lý chính xác các mối quan hệ sẽ thất bại. 

Một trường hợp thất bại cụ thể phát sinh khi Thomas có cùng điểm với nhiều học sinh. Giả sử Thomas và học sinh 5 đều có tổng số 400 và học sinh 2 có 401. Xếp hạng chính xác xếp học sinh 2 lên trước, sau đó là Thomas trước học sinh 5 vì id nhỏ hơn. Nếu chúng ta bỏ qua việc phá vỡ id, thứ hạng của Thomas có thể bị tính sai thành 3 thay vì 2. 

## Phương pháp tiếp cận 

Cách đơn giản nhất để suy nghĩ về vấn đề này là tính tổng điểm của mỗi học sinh và sau đó sắp xếp tất cả học sinh theo các quy tắc được mô tả. Sau khi phân loại, chúng tôi xác định vị trí của Thomas và đọc vị trí của anh ấy. 

Một cách tiếp cận mạnh mẽ sẽ là quét liên tục tất cả học sinh và chọn ra ứng viên tốt nhất tiếp theo chưa được chọn. Điều này bắt chước hành vi sắp xếp lựa chọn. Mỗi bước lựa chọn yêu cầu quét O(n) và chúng tôi thực hiện n lần, dẫn đến các hoạt động O(n2). Với n lên tới 1000, đây là khoảng một triệu so sánh, vẫn có thể chấp nhận được nhưng không cần thiết. 

Một cách tiếp cận trực tiếp hơn là tính toán tất cả các tổng, lưu trữ chúng bằng id và sắp xếp bằng cách sử dụng tính năng sắp xếp tích hợp của Python. Thông tin chi tiết quan trọng là thứ tự xếp hạng được xác định hoàn toàn bằng một khóa duy nhất: tổng điểm âm (đối với thứ tự giảm dần) và sau đó là id (đối với thứ tự tăng dần). Điều này chuyển đổi vấn đề thành một nhiệm vụ sắp xếp tiêu chuẩn. 

Sau khi được sắp xếp, thứ hạng của Thomas chỉ đơn giản là chỉ số của anh ấy trong danh sách được sắp xếp cộng thêm một. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lựa chọn vũ phu | O(n²) | O(n) | Đã chấp nhận | 
| Sắp xếp theo khóa | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng học sinh và lưu trữ điểm của họ. 
2. Với mỗi học sinh, hãy tính tổng điểm của bốn môn học. Điều này làm giảm mỗi học sinh xuống một giá trị có thể so sánh được. 
3. Lưu trữ mỗi học sinh thành một cặp gồm tổng âm và id. Số âm được sử dụng để sắp xếp theo thứ tự tăng dần sẽ tạo ra thứ tự điểm giảm dần. 
4. Sắp xếp danh sách học sinh. Việc sắp xếp tự động áp dụng quy tắc phụ vì id được bao gồm trong bộ dữ liệu. 
5. Quét danh sách đã sắp xếp và tìm mục tương ứng với id 1. 
6. Xuất vị trí dựa trên số 1 của nó làm thứ hạng. 

Tại sao nó hoạt động dựa trên thực tế là việc sắp xếp theo bộ sẽ thực thi thứ tự từ điển. Bộ dữ liệu (-sum, id) đảm bảo rằng số tiền cao hơn sẽ được xếp trước và trong số các số tiền bằng nhau, các id nhỏ hơn sẽ được xếp trước. Vì mỗi học sinh được đại diện chính xác một lần và thứ tự tổng thể và nhất quán nên danh sách được sắp xếp chính xác là thứ hạng được yêu cầu. Vì vậy, vị trí của Thomas trong danh sách này là thứ hạng chính xác của anh ấy.

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    students = []

    for i in range(1, n + 1):
        a, b, c, d = map(int, input().split())
        total = a + b + c + d
        students.append((-total, i))

    students.sort()

    for idx, (_, sid) in enumerate(students, start=1):
        if sid == 1:
            print(idx)
            return

if __name__ == "__main__":
    main()
```Giải pháp xây dựng danh sách các bộ dữ liệu trong đó mỗi bộ dữ liệu mã hóa cả hai tiêu chí xếp hạng. Bước sắp xếp xử lý tất cả logic sắp xếp, giúp tránh các lỗi so sánh thủ công. Vòng lặp cuối cùng chỉ tìm kiếm id 1, an toàn với các ràng buộc nhỏ. 

Một lỗi phổ biến là quên đưa id vào khóa sắp xếp. Nếu không có nó, các điểm bằng nhau có thể xuất hiện theo thứ tự tùy ý, điều này sẽ phá vỡ định nghĩa xếp hạng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
100 98 100 100
100 100 100 100
100 100 99 99
90 99 90 100
100 98 60 99
```Tổng số được tính toán: 

| id | tổng hợp | 
| --- | --- | 
| 1 | 398 | 
| 2 | 400 | 
| 3 | 398 | 
| 4 | 379 | 
| 5 | 357 | 

Thứ tự sắp xếp theo (-sum, id): 

| vị trí | id | tổng hợp | 
| --- | --- | --- | 
| 1 | 2 | 400 | 
| 2 | 1 | 398 | 
| 3 | 3 | 398 | 
| 4 | 4 | 379 | 
| 5 | 5 | 357 | 

Thomas (id 1) xuất hiện ở vị trí 2 nên đầu ra là 2. 

Dấu vết này xác nhận rằng việc phá vỡ mối ràng buộc theo id là điều cần thiết, vì id 1 và 3 có cùng số điểm nhưng được sắp xếp chính xác. 

### Mẫu 2 

đầu vào:```
6
100 100 100 69
60 60 60 60
80 80 80 70
75 75 75 75
75 75 75 75
0 0 0 0
```Tổng số: 

| id | tổng hợp | 
| --- | --- | 
| 1 | 369 | 
| 2 | 240 | 
| 3 | 310 | 
| 4 | 300 | 
| 5 | 300 | 
| 6 | 0 | 

Thứ tự sắp xếp: 

| vị trí | id | tổng hợp | 
| --- | --- | --- | 
| 1 | 1 | 369 | 
| 2 | 3 | 310 | 
| 3 | 4 | 300 | 
| 4 | 5 | 300 | 
| 5 | 2 | 240 | 
| 6 | 6 | 0 | 

Thomas đứng đầu nên thứ hạng là 1. Dấu vết cho thấy ngay cả khi có nhiều mối quan hệ tồn tại (id 4 và 5), thứ tự vẫn ổn định do phá vỡ mối liên kết dựa trên id. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế sau khi tính tổng | 
| Không gian | O(n) | Mỗi học sinh được lưu trữ một lần dưới dạng một bộ | 

Các ràng buộc cho phép tối đa 1000 sinh viên, vì vậy việc sắp xếp tối đa 1000 phần tử là chuyện nhỏ. Việc sử dụng bộ nhớ có quy mô không đổi và nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    return sys.stdout.getvalue() if (main() or True) else ""

# sample tests
assert run("""5
100 98 100 100
100 100 100 100
100 100 99 99
90 99 90 100
100 98 60 99
""").strip() == "2"

assert run("""6
100 100 100 69
60 60 60 60
80 80 80 70
75 75 75 75
75 75 75 75
0 0 0 0
""").strip() == "1"

# custom: minimum case
assert run("""1
10 10 10 10
""").strip() == "1"

# custom: all equal scores
assert run("""3
10 10 10 10
10 10 10 10
10 10 10 10
""").strip() == "1"

# custom: Thomas is worst
assert run("""3
0 0 0 0
10 0 0 0
0 0 0 1
""").strip() == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| sinh viên độc thân | 1 | ranh giới tối thiểu | 
| tất cả đều bình đẳng | 1 | hòa theo id | 
| Thomas tệ nhất | 3 | đúng thứ tự xếp hạng | 

## Vỏ cạnh 

Khi tất cả học sinh có tổng điểm giống nhau thì việc xếp hạng phụ thuộc hoàn toàn vào thứ tự id. Ví dụ: với ba học sinh đều đạt điểm 40, thứ tự sắp xếp phải là id 1, 2, 3. Thomas luôn đứng đầu trong số những học sinh bằng nhau, vì vậy thứ hạng của anh ấy là 1. Thuật toán xử lý điều này một cách chính xác vì id là khóa phụ trong bộ dữ liệu. 

Khi Thomas có điểm thấp nhất, anh ấy sẽ xuất hiện ở cuối danh sách đã sắp xếp. Ví dụ: nếu Thomas có tổng số 10 và những người khác có 20 và 30, thứ tự sắp xếp sẽ trở thành 30, 20, 10, xếp anh ta ở hạng 3. Phím sắp xếp đảm bảo điều này một cách tự nhiên mà không cần xử lý đặc biệt, vì -10 lớn hơn -30 và -20, đẩy anh ta về thứ tự muộn hơn.
