---
title: "CF 102756D - Điều Tuyệt vời nhất Kể từ Bánh mì Cắt lát"
description: "Bài toán yêu cầu chúng ta xem xét một tập hợp các phát minh lịch sử. Mỗi phát minh đều có tên, ngày phát minh và điểm số thể hiện mức độ tuyệt vời của nó khi so sánh với bánh mì cắt lát."
date: "2026-07-29T00:30:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102756
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 10-09-20 Div. 1"
rating: 0
weight: 102756
solve_time_s: 74
verified: true
draft: false
---

[CF 102756D - Điều tuyệt vời nhất kể từ khi cắt bánh mì](https://codeforces.com/problemset/problem/102756/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta xem xét một tập hợp các phát minh lịch sử. Mỗi phát minh đều có tên, ngày phát minh và điểm số thể hiện mức độ tuyệt vời của nó khi so sánh với bánh mì cắt lát. Chúng ta cần tìm ra phát minh vĩ đại nhất trong số những phát minh duy nhất được tạo ra sau bánh mì cắt lát, được coi là phát minh ra vào ngày 7 tháng 7 năm 1928. Câu trả lời là tên của phát minh có số điểm cao nhất trong số những phát minh đủ điều kiện đó. 

Đầu vào chứa tới 10.000 mô tả phát minh. Mỗi mô tả chiếm ba dòng: tiêu đề phát minh, ngày ở định dạng tháng mà con người có thể đọc được và điểm thập phân. Vì số lượng phát minh chỉ có 10.000 nên việc quét tuyến tính dễ dàng đủ nhanh. Bất kỳ cách tiếp cận nào so sánh từng cặp phát minh sẽ thực hiện khoảng 100 triệu so sánh trong trường hợp xấu nhất, điều này là không cần thiết đối với một bài toán lựa chọn đơn giản như vậy. 

Khó khăn chính không phải là kích thước của dữ liệu mà là việc xử lý ngày tháng và lọc chính xác. Một sai lầm phổ biến là chọn ngay điểm lớn nhất trong khi đọc mà không kiểm tra xem phát minh có xảy ra sau khi cắt lát bánh mì hay không. Một sai lầm khác là so sánh trực tiếp chuỗi ngày tháng. Ví dụ: ngày "ngày 1 tháng 12 năm 1930" và "ngày 1 tháng 1 năm 1931" không sắp xếp chính xác dưới dạng chuỗi vì tên tháng không được sắp xếp theo thứ tự thời gian. 

Trường hợp đặc biệt xảy ra khi điểm cao nhất thuộc về một phát minh trước bánh mì cắt lát. Hãy xem xét đầu vào này:```
3
Ancient Fire
Apr 20, 960
0.99999
Radio
Dec 26, 1933
0.65
Hovercraft
Dec 12, 1955
0.896
```Đầu ra đúng là:```
Hovercraft
```Việc thực hiện bất cẩn chỉ theo dõi điểm tối đa sẽ in sai "Lửa cổ". Bộ lọc ngày phải diễn ra trước khi so sánh điểm số. 

Một trường hợp khác là một phát minh được tạo ra chính xác vào ngày 7 tháng 7 năm 1928. Phát minh đó không được coi là kể từ khi bánh mì cắt lát, bởi vì chỉ những ngày sau ngày đó mới đủ điều kiện. Ví dụ:```
2
Exact Date
Jul 7, 1928
1.0
Future Item
Jan 1, 1929
0.5
```Đầu ra phải là:```
Future Item
```Việc so sánh điểm chỉ được thực hiện sau khi xác nhận được sáng chế hoàn toàn muộn hơn ngày tham chiếu. 

## Phương pháp tiếp cận 

Giải pháp đơn giản là kiểm tra mọi phát minh và giữ lại phát minh đủ điều kiện tốt nhất được tìm thấy cho đến nay. Điều này đúng vì câu trả lời chỉ đơn giản là số điểm tối đa trong một tập hợp được lọc. Đối với mỗi phát minh, trước tiên chúng tôi kiểm tra xem ngày của phát minh đó có sau ngày 7 tháng 7 năm 1928 hay không. Nếu đúng như vậy, chúng tôi so sánh điểm của phát minh đó với điểm tốt nhất hiện tại và thay thế câu trả lời khi cần thiết. 

Cách tiếp cận bạo lực có thể so sánh mọi phát minh với mọi phát minh đủ điều kiện khác để xác định phát minh nào có điểm cao nhất. Điều này vẫn đúng, nhưng với 10.000 phát minh, nó có thể cần khoảng 100 triệu điểm so sánh. Nó cũng bỏ qua thực tế là việc tìm mức tối đa không đòi hỏi phải biết mối quan hệ giữa mọi cặp phần tử. 

Nhận xét quan trọng là vấn đề không có sự tương tác giữa các phát minh. Việc lựa chọn sáng chế tốt nhất chỉ phụ thuộc vào tính đủ điều kiện và điểm số của từng sáng chế. Điều này cho phép chúng tôi duy trì mức chạy tối đa trong khi đọc dữ liệu đầu vào. Tính năng phân tích ngày tháng sẽ chuyển đổi từng ngày thành dạng biểu diễn số, khiến cho việc so sánh theo trình tự thời gian trở nên đáng tin cậy. 

Phương pháp vũ phu hoạt động vì cuối cùng nó phát hiện ra mức tối đa, nhưng thất bại vì lặp lại những so sánh không cần thiết. Việc quan sát thấy mức tối đa có thể được duy trì tăng dần sẽ giảm toàn bộ nhiệm vụ xuống còn một lần chuyển qua đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(1) | Quá chậm đối với trường hợp chung | 
| Tối ưu | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số phát minh và khởi tạo các biến để lưu điểm cao nhất và danh hiệu tương ứng. 
2. Xử lý từng phát minh một. Đọc tiêu đề, ngày tháng và điểm số của nó vì việc lưu trữ tất cả các phát minh là không cần thiết. 
3. Chuyển đổi ngày thành dạng số có thể so sánh được. Tên tháng được chuyển đổi thành số tháng và ngày trở thành một bộ chứa năm, tháng và ngày. Sự trình bày này tuân theo trình tự thời gian một cách tự nhiên. 
4. Kiểm tra xem ngày phát minh có lớn hơn ngày 7 tháng 7 năm 1928 hay không. Nếu không, hãy bỏ qua phát minh này vì nó không đủ tiêu chuẩn. 
5. Đối với mỗi phát minh đủ điều kiện, hãy so sánh điểm của nó với điểm cao nhất hiện tại. Nếu điểm lớn hơn, hãy cập nhật câu trả lời được lưu trữ. 
6. Sau khi tất cả các phát minh đã được xử lý, hãy in tiêu đề đã lưu. 

Lý do chỉ cần một câu trả lời được lưu trữ là đủ vì sau khi xử lý bất kỳ tiền tố nào của dữ liệu đầu vào, phát minh được lưu trữ luôn là phát minh có giá trị tốt nhất được thấy trong tiền tố đó. Khi một phát minh khác được xử lý, nó sẽ tệ hơn và câu trả lời hiện tại vẫn đúng hoặc nó tốt hơn và việc thay thế câu trả lời sẽ khôi phục lại đặc tính tương tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    
    months = {
        "Jan": 1,
        "Feb": 2,
        "Mar": 3,
        "Apr": 4,
        "May": 5,
        "Jun": 6,
        "Jul": 7,
        "Aug": 8,
        "Sep": 9,
        "Oct": 10,
        "Nov": 11,
        "Dec": 12,
    }

    sliced_bread_date = (1928, 7, 7)

    best_score = -1.0
    best_title = ""

    for _ in range(n):
        title = input().rstrip("\n")
        date_line = input().strip()
        score = float(input().strip())

        month, day_year = date_line.split(" ", 1)
        day, year = day_year.replace(",", "").split()

        date = (int(year), months[month], int(day))

        if date > sliced_bread_date and score > best_score:
            best_score = score
            best_title = title

    print(best_title)

if __name__ == "__main__":
    solve()
```Từ điển tháng chuyển các tháng dạng văn bản thành số nguyên, làm cho việc so sánh ngày không phụ thuộc vào thứ tự chuỗi. Định dạng bộ dữ liệu`(year, month, day)`hoạt động vì Python so sánh các phần tử bộ từ trái sang phải, khớp với thứ tự được sử dụng trong so sánh lịch thông thường. 

Điểm được lưu dưới dạng số dấu phẩy động vì đầu vào đưa ra xếp hạng thập phân. Vì không có mối ràng buộc nào cho phát minh có giá trị lớn nhất nên chỉ cần so sánh nghiêm ngặt và đơn giản là đủ. Điểm số ban đầu của`-1.0`đảm bảo rằng phát minh hợp lệ đầu tiên sẽ thay thế phần giữ chỗ. 

Điều kiện ngày sử dụng`>`còn hơn là`>=`. Điều này xử lý đúng quy tắc ranh giới vì phát minh vào ngày 7 tháng 7 năm 1928 không được coi là mới hơn bánh mì cắt lát. 

Thuật toán không bao giờ lưu trữ danh sách đầy đủ các phát minh. Nó chỉ giữ lại tiêu đề và điểm số tốt nhất hiện tại, giúp duy trì mức sử dụng bộ nhớ không đổi. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu:```
5
FM Radio
Dec 26, 1933
0.65
Ballpoint Pen
Oct 30, 1888
0.7442
Penicillin
Sep 28, 1928
0.0036
The Hovercraft
Dec 12, 1955
0.896
Fireworks
Apr 20, 960
0.99999
```dấu vết là: 

| Tiêu đề | Ngày | Đạt chuẩn? | Điểm | Tốt nhất hiện tại | 
| --- | --- | --- | --- | --- | 
| Đài FM | 26/12/1933 | Có | 0,65 | Đài FM | 
| Bút bi | 30 tháng 10 năm 1888 | Không | 0,7442 | Đài FM | 
| Penicillin | 28/09/1928 | Có | 0,0036 | Đài FM | 
| Thủy phi cơ | 12/12/1955 | Có | 0,896 | Thủy phi cơ | 
| Pháo hoa | 20 tháng 4 năm 960 | Không | 0,99999 | Thủy phi cơ | 

Dấu vết này chứng tỏ tại sao việc lọc phải diễn ra trước khi so sánh điểm số. Pháo hoa có số điểm lớn nhất nhưng không thể giành chiến thắng vì có trước bánh mì cắt lát. 

Một ví dụ thứ hai:```
4
Old Device
Jul 6, 1928
0.99
Boundary Device
Jul 7, 1928
0.98
New Device
Jul 8, 1928
0.5
Later Device
Jan 1, 1930
0.8
```Dấu vết là: 

| Tiêu đề | Ngày | Đạt chuẩn? | Điểm | Tốt nhất hiện tại | 
| --- | --- | --- | --- | --- | 
| Thiết Bị Cũ | Ngày 6 tháng 7 năm 1928 | Không | 0,99 | Không có | 
| Thiết bị ranh giới | Ngày 7 tháng 7 năm 1928 | Không | 0,98 | Không có | 
| Thiết bị mới | 8 tháng 7 năm 1928 | Có | 0,5 | Thiết bị mới | 
| Thiết bị sau này | Ngày 1 tháng 1 năm 1930 | Có | 0,8 | Thiết bị sau này | 

Dấu vết này xác nhận sự so sánh nghiêm ngặt về ngày tháng và cho thấy rằng một phát minh đủ điều kiện có thể bắt đầu câu trả lời ngay cả khi những phát minh trước đó có điểm số cao hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi phát minh được đọc và xử lý một lần | 
| Không gian | O(1) | Chỉ có dữ liệu trợ giúp và phát minh tốt nhất hiện tại mới được lưu trữ | 

Với tối đa 10.000 phát minh, giải pháp tuyến tính sử dụng ít thao tác hơn nhiều so với giới hạn thời gian sẵn có yêu cầu. Việc sử dụng bộ nhớ không đổi bất kể số lượng phát minh. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(data):
    input = io.StringIO(data).readline

    n = int(input())

    months = {
        "Jan": 1, "Feb": 2, "Mar": 3, "Apr": 4,
        "May": 5, "Jun": 6, "Jul": 7, "Aug": 8,
        "Sep": 9, "Oct": 10, "Nov": 11, "Dec": 12
    }

    sliced_bread_date = (1928, 7, 7)
    best_score = -1.0
    best_title = ""

    for _ in range(n):
        title = input().rstrip("\n")
        date_line = input().strip()
        score = float(input().strip())

        month, rest = date_line.split(" ", 1)
        day, year = rest.replace(",", "").split()
        date = (int(year), months[month], int(day))

        if date > sliced_bread_date and score > best_score:
            best_score = score
            best_title = title

    return best_title + "\n"

assert solve("""5
FM Radio
Dec 26, 1933
0.65
Ballpoint Pen
Oct 30, 1888
0.7442
Penicillin
Sep 28, 1928
0.0036
The Hovercraft
Dec 12, 1955
0.896
Fireworks
Apr 20, 960
0.99999
""") == "The Hovercraft\n"

assert solve("""1
Modern Thing
Jan 1, 2020
0.1
""") == "Modern Thing\n"

assert solve("""3
Old
Jan 1, 1900
0.9
Equal Boundary
Jul 7, 1928
1.0
New
Jul 8, 1928
0.2
""") == "New\n"

assert solve("""3
A
Jan 1, 2000
0.5
B
Jan 1, 2001
0.5
C
Jan 1, 2002
0.7
""") == "C\n"

assert solve("""2
First
Jul 8, 1928
0.01
Second
Dec 31, 1928
0.02
""") == "Second\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu gốc | Thủy phi cơ | Lọc cơ bản và lựa chọn tối đa | 
| Phát minh hiện đại duy nhất | Điều hiện đại | Kích thước đầu vào tối thiểu | 
| Phát minh ngày 7/7/1928 | Mới | Xử lý ngày ranh giới | 
| Một số điểm gần | C | Logic cập nhật tối đa | 
| Nhiều phát minh sau khi cắt lát bánh mì | Thứ hai | Thứ tự thay thế đúng | 

## Vỏ cạnh 

Trường hợp khó khăn đầu tiên là khi một phát minh cũ có điểm rất cao. Đối với đầu vào:```
3
Ancient Fire
Apr 20, 960
0.99999
Radio
Dec 26, 1933
0.65
Hovercraft
Dec 12, 1955
0.896
```thuật toán đầu tiên từ chối Ancient Fire vì bộ dữ liệu ngày của nó là`(960, 4, 20)`, nhỏ hơn`(1928, 7, 7)`. Sau đó, nó so sánh hai phát minh hợp lệ và giữ lại Thủy phi cơ vì`0.896`lớn hơn`0.65`. 

Trường hợp thứ hai là ngày cắt lát bánh mì chính xác:```
2
Exact Date
Jul 7, 1928
1.0
Future Item
Jan 1, 1929
0.5
```Phát minh đầu tiên tạo ra bộ dữ liệu`(1928, 7, 7)`, bằng với ngày tham chiếu, do đó phép so sánh chặt chẽ sẽ loại bỏ nó. Phát minh thứ hai tạo ra`(1929, 1, 1)`và trở thành câu trả lời. Điều này ngăn không cho điều kiện biên được xử lý không chính xác.
