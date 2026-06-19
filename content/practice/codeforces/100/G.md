---
title: "CF 100G - Đặt tên album"
description: "Aryo muốn chọn tiêu đề cho album mới từ danh sách tên ứng cử viên. Một số tên đã được sử dụng trong những năm trước. Quy tắc quyết định của ông có hai lớp. Nếu tên ứng cử viên chưa từng được sử dụng trước đây thì đó là lựa chọn tốt nhất có thể."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 100
codeforces_index: "G"
codeforces_contest_name: "Unknown Language Round 3"
rating: 1800
weight: 100
solve_time_s: 110
verified: true
draft: false
---

[CF 100G - Đặt tên cho album](https://codeforces.com/problemset/problem/100/G) 

**Đánh giá:** 1800 
**Thẻ:** *đặc biệt, cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Aryo muốn chọn tiêu đề cho album mới từ danh sách tên ứng cử viên. Một số tên đã được sử dụng trong những năm trước. Quy tắc quyết định của ông có hai lớp. 

Nếu tên ứng cử viên chưa từng được sử dụng trước đây thì đó là lựa chọn tốt nhất có thể. Trong số tất cả các ứng viên chưa được sử dụng, anh ta chọn ứng viên lớn nhất theo thứ tự bảng chữ cái. 

Nếu mọi ứng cử viên đã được sử dụng trước đó, anh ta sẽ chọn ứng viên có lần sử dụng gần đây nhất và cách đây lâu nhất. Nếu nhiều tên có cùng năm cũ nhất, anh ta lại chọn tên lớn nhất theo thứ tự bảng chữ cái. 

Đầu vào cung cấp lịch sử tên album cùng với năm xuất bản, sau đó là danh sách các tên mới có thể có. Chúng ta phải in tiêu đề đã chọn. 

Các ràng buộc ngay lập tức gợi ý rằng giải pháp phải gần tuyến tính. Có thể có tới$10^5$tên đã được sử dụng, do đó việc quét liên tục toàn bộ lịch sử để tìm mọi ứng viên sẽ trở nên tốn kém. Một giải pháp bậc hai có thể đạt tới khoảng$10^9$hoạt động, vượt xa những gì phù hợp với giới hạn 2 giây. Bản đồ băm là công cụ tự nhiên ở đây vì chúng ta chỉ cần tra cứu nhanh năm gần đây nhất được liên kết với mỗi tên. 

Một điểm tinh tế là một cái tên có thể xuất hiện nhiều lần trong lịch sử. Chúng tôi không quan tâm đến năm đầu tiên, chúng tôi quan tâm đến việc sử dụng gần đây nhất. Hãy xem xét đầu vào này:```
3
echo 1999
echo 2005
nova 2001
2
echo
nova
```Câu trả lời đúng là`nova`, bởi vì`nova`được sử dụng lần cuối vào năm 2001 trong khi`echo`được sử dụng lần cuối vào năm 2005. Việc triển khai bất cẩn lưu trữ lần xuất hiện đầu tiên thay vì lần xuất hiện mới nhất sẽ chọn sai`echo`. 

Một sai lầm dễ mắc phải khác là xử lý sai các mối quan hệ theo thứ tự bảng chữ cái. Giả sử chúng ta có:```
2
alpha 2000
beta 2000
2
alpha
beta
```Cả hai cái tên đều được sử dụng như nhau từ lâu nên câu trả lời phải là`beta`bởi vì nó được sắp xếp theo thứ tự bảng chữ cái muộn hơn. Nếu chúng tôi chỉ theo dõi năm tối thiểu và ngừng cập nhật các mối quan hệ, chúng tôi sẽ trả về kết quả sai. 

Những tên không được sử dụng cũng cần được xử lý cẩn thận. Chúng luôn được ưa thích hơn những cái tên đã sử dụng bất kể năm tháng. Ví dụ:```
1
dream 2010
2
dream
vision
```Câu trả lời đúng là`vision`, mặc dù`dream`có năm cũ hợp lệ. Coi những tên không được sử dụng là có năm`0`hoạt động về mặt khái niệm, nhưng chỉ khi việc triển khai ưu tiên chúng một cách rõ ràng hơn các tên được sử dụng. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ xử lý mọi tên ứng viên bằng cách quét toàn bộ lịch sử để xác định xem tên đó có xuất hiện trước đó hay không và năm gần nhất của tên đó là gì. Đối với mỗi ứng cử viên, chúng tôi so sánh với tất cả các tên đã sử dụng trước đó và giữ nguyên số năm tối đa được xem cho ứng cử viên đó. 

Điều này hoạt động hợp lý vì nó tính toán chính xác thông tin cần thiết cho quy tắc quyết định. Vấn đề là chi phí. Với$10^5$mục lịch sử và$10^4$ứng viên, trường hợp xấu nhất sẽ trở thành$10^9$so sánh. Python không thể xử lý lượng công việc đó trong thời gian giới hạn. 

Cấu trúc của vấn đề đưa ra hướng đi nhanh hơn nhiều. Mọi truy vấn đều hỏi cùng một điều: "năm gần đây nhất gắn liền với tên này là gì?" Việc tính toán lại nhiều lần là lãng phí. Thay vào đó, chúng tôi xử lý trước lịch sử một lần thành bản đồ băm từ tên đến năm gần nhất. 

Trong khi đọc lịch sử, chúng tôi cập nhật:```
latest[name] = max(latest[name], year)
```Sau khi tiền xử lý, mỗi ứng viên có thể được đánh giá trong thời gian dự kiến ​​không đổi. 

Bản thân quy tắc quyết định cũng có thể được đơn giản hóa. Một cái tên chưa được sử dụng luôn tốt hơn một cái tên đã được sử dụng. Trong số những tên không được sử dụng, hãy chọn tên lớn nhất theo thứ tự bảng chữ cái. Nếu không có tên nào chưa được sử dụng, hãy chọn tên đã sử dụng có năm gần nhất nhỏ nhất, phá vỡ mối quan hệ theo thứ tự bảng chữ cái lớn nhất. 

Điều này biến toàn bộ vấn đề thành một lần vượt qua các ứng viên bằng những so sánh đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot m)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n + m)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một từ điển có tên`latest`. 

Từ điển này sẽ lưu trữ năm gần đây nhất mà mỗi tên album được sử dụng. 
2. Đọc từng tên album lịch sử. 

Đối với mỗi`(name, year)`cặp, cập nhật:```
latest[name] = max(existing_year, year)
```Chúng tôi giữ năm tối đa vì vấn đề quan tâm đến lần sử dụng mới nhất chứ không phải lần sử dụng đầu tiên. 
3. Khởi tạo hai biến theo dõi. 

Một biến lưu trữ ứng viên chưa được sử dụng tốt nhất được tìm thấy cho đến nay. 

Một cửa hàng khác lưu trữ ứng viên được sử dụng tốt nhất cùng với năm sử dụng gần nhất của nó. 
4. Xử lý từng tên ứng viên. 

Nếu tên không tồn tại trong`latest`, nó không được sử dụng. So sánh nó với ứng cử viên chưa được sử dụng tốt nhất hiện tại và giữ cái lớn hơn theo thứ tự bảng chữ cái. 
5. Nếu ứng viên đã được sử dụng trước đó, hãy lấy năm gần đây nhất của nó từ từ điển. 

So sánh nó với ứng cử viên được sử dụng tốt nhất hiện tại. 

Một ứng cử viên sẽ tốt hơn nếu: 

- năm gần đây nhất của nó nhỏ hơn, có nghĩa là nó đã được sử dụng lâu hơn 
- hoặc các năm bằng nhau và tên lớn hơn theo thứ tự bảng chữ cái 
6. Sau khi tất cả các ứng cử viên được xử lý, hãy kiểm tra xem có tồn tại một ứng cử viên chưa được sử dụng hay không. 

Nếu có, hãy in ra ứng viên chưa được sử dụng tốt nhất vì những tên chưa được sử dụng luôn được ưu tiên. 

Nếu không thì in ra ứng cử viên được sử dụng tốt nhất. 

### Tại sao nó hoạt động 

Tính bất biến của từ điển là sau khi tiền xử lý,`latest[name]`bằng năm gần đây nhất cái tên đó xuất hiện trong lịch sử. Mọi bản cập nhật đều duy trì điều này vì chúng tôi luôn giữ số năm tối đa được thấy cho đến nay. 

Trong quá trình xử lý ứng viên, trình theo dõi không được sử dụng luôn lưu trữ tên chưa được sử dụng lớn nhất theo thứ tự bảng chữ cái cho đến nay. Trình theo dõi đã sử dụng luôn lưu trữ ứng viên tốt nhất theo thứ tự: 

1. Năm gần đây nhỏ hơn thì tốt hơn. 
2. Nếu số năm bằng nhau, theo thứ tự bảng chữ cái lớn hơn thì tốt hơn. 

Vì mọi ứng cử viên đều được so sánh với mức tối ưu hiện tại theo chính xác các quy tắc này nên câu trả lời được lưu trữ cuối cùng là tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    latest = {}

    for _ in range(n):
        name, year = input().split()
        year = int(year)

        if name not in latest or year > latest[name]:
            latest[name] = year

    m = int(input())

    best_unused = None

    best_used_name = None
    best_used_year = float('inf')

    for _ in range(m):
        name = input().strip()

        if name not in latest:
            if best_unused is None or name > best_unused:
                best_unused = name
        else:
            year = latest[name]

            if (year < best_used_year or
               (year == best_used_year and name > best_used_name)):
                best_used_year = year
                best_used_name = name

    if best_unused is not None:
        print(best_unused)
    else:
        print(best_used_name)

if __name__ == "__main__":
    solve()
```Phần đầu tiên xây dựng`latest`từ điển. Chi tiết quan trọng là sử dụng năm tối đa cho các tên lặp lại. Việc quên điều này sẽ làm thay đổi hoàn toàn ý nghĩa của vấn đề vì chúng ta quan tâm đến lần sử dụng gần đây nhất. 

Phần thứ hai quét danh sách ứng cử viên một lần. Mã phân tách các ứng cử viên không được sử dụng và đã sử dụng vì tên không được sử dụng luôn chiếm ưu thế so với các tên được sử dụng. Điều này tránh các giá trị nhân tạo khó xử như gán tên không sử dụng năm`-1`. 

Logic phá vỡ ràng buộc được viết rõ ràng:```
year < best_used_year
```có nghĩa là tên đã được sử dụng lâu hơn, điều đó tốt hơn.```
year == best_used_year and name > best_used_name
```thực hiện tie-break theo thứ tự bảng chữ cái. sử dụng`>`hoạt động vì Python so sánh các chuỗi theo từ điển. 

Việc khởi tạo với`float('inf')`đảm bảo rằng ứng cử viên được sử dụng đầu tiên sẽ luôn thay thế phần giữ chỗ ban đầu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
eyesonme 2008
anewdayhascome 2002
oneheart 2003
2
oneheart
bienbien
```Lịch sử xử lý: 

| Tên | Năm | mới nhất sau khi cập nhật | 
| --- | --- | --- | 
| mắt tôi | 2008 | {eyesonme: 2008} | 
| một ngày mới đã đến | 2002 | {eyesonme: 2008, anewdayhascome: 2002} | 
| một lòng | 2003 | {eyesonme: 2008, anewdayhascome: 2002, oneheart: 2003} | 

Xử lý ứng viên: 

| Ứng viên | Đã sử dụng trước đây | Hành động | Tốt nhất hiện tại | 
| --- | --- | --- | --- | 
| một lòng | Có | sử dụng tốt nhất = một lòng | đã sử dụng: một trái tim | 
| biên giới | Không | tốt nhất chưa sử dụng = bienbien | chưa sử dụng: bienbien | 

Thuật toán in`bienbien`bởi vì bất kỳ tên nào không được sử dụng đều xếp hạng cao hơn tất cả các tên được sử dụng. 

### Ví dụ tùy chỉnh 

đầu vào:```
4
alpha 2005
beta 2000
gamma 2000
alpha 2010
3
alpha
beta
gamma
```Lịch sử xử lý: 

| Tên | Năm | mới nhất sau khi cập nhật | 
| --- | --- | --- | 
| alpha | 2005 | {alpha: 2005} | 
| phiên bản beta | 2000 | {alpha: 2005, beta: 2000} | 
| gamma | 2000 | {alpha: 2005, beta: 2000, gamma: 2000} | 
| alpha | 2010 | {alpha: 2010, beta: 2000, gamma: 2000} | 

Xử lý ứng viên: 

| Ứng viên | Năm Gần Nhất | Kết quả so sánh | Được sử dụng tốt nhất hiện nay | 
| --- | --- | --- | --- | 
| alpha | 2010 | ứng cử viên đầu tiên | alpha | 
| phiên bản beta | 2000 | cũ hơn 2010 | phiên bản beta | 
| gamma | 2000 | cùng năm, lớn hơn theo thứ tự bảng chữ cái | gamma | 

Đầu ra của thuật toán`gamma`. 

Dấu vết này thể hiện cả hai chi tiết quan trọng: tên lặp lại phải giữ năm mới nhất và các năm bằng nhau yêu cầu so sánh theo thứ tự bảng chữ cái. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$| Mỗi mục nhập lịch sử và mỗi ứng cử viên được xử lý một lần | 
| Không gian |$O(n)$| Từ điển lưu trữ tối đa một mục nhập cho mỗi tên được sử dụng riêng biệt | 

Kích thước đầu vào lớn nhất vừa vặn thoải mái trong giới hạn này. Xung quanh$10^5$Các thao tác từ điển không quan trọng đối với Python và mức sử dụng bộ nhớ vẫn thấp hơn giới hạn 64 MB vì ​​tên ngắn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())

    latest = {}

    for _ in range(n):
        name, year = input().split()
        year = int(year)

        if name not in latest or year > latest[name]:
            latest[name] = year

    m = int(input())

    best_unused = None

    best_used_name = None
    best_used_year = float('inf')

    for _ in range(m):
        name = input().strip()

        if name not in latest:
            if best_unused is None or name > best_unused:
                best_unused = name
        else:
            year = latest[name]

            if (year < best_used_year or
               (year == best_used_year and name > best_used_name)):
                best_used_year = year
                best_used_name = name

    if best_unused is not None:
        print(best_unused)
    else:
        print(best_used_name)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run(
"""3
eyesonme 2008
anewdayhascome 2002
oneheart 2003
2
oneheart
bienbien
"""
) == "bienbien", "sample 1"

# minimum input
assert run(
"""0
1
solo
"""
) == "solo", "single unused name"

# repeated historical names
assert run(
"""3
echo 1999
echo 2005
nova 2001
2
echo
nova
"""
) == "nova", "must keep latest year"

# tie on year, alphabetical rule
assert run(
"""2
alpha 2000
beta 2000
2
alpha
beta
"""
) == "beta", "alphabetical tie break"

# multiple unused names
assert run(
"""1
dream 2010
3
vision
future
galaxy
"""
) == "vision", "largest alphabetical unused name"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Không có tên lịch sử |`solo`| Xử lý đúng khi mọi ứng viên đều không được sử dụng | 
| Tên lịch sử lặp đi lặp lại |`nova`| Năm gần nhất phải ghi đè lên những năm trước | 
| Năm bằng nhau |`beta`| Sự ràng buộc theo thứ tự bảng chữ cái giữa các tên được sử dụng | 
| Nhiều tên không được sử dụng |`vision`| Ứng cử viên chưa được sử dụng lớn nhất theo thứ tự bảng chữ cái sẽ thắng | 

## Vỏ cạnh 

Hãy xem xét tên album lặp đi lặp lại trong lịch sử:```
3
echo 1999
echo 2005
nova 2001
2
echo
nova
```Từ điển phát triển như sau:```
echo -> 1999
echo -> 2005
nova -> 2001
```Khi các ứng viên được kiểm tra,`echo`có năm mới nhất`2005`trong khi`nova`có`2001`. Từ`2001`lớn hơn, câu trả lời trở thành`nova`. Thuật toán xử lý việc này một cách chính xác vì nó luôn lưu trữ năm tối đa cho mỗi tên. 

Bây giờ hãy xem xét những năm bằng nhau:```
2
alpha 2000
beta 2000
2
alpha
beta
```Cả hai ứng cử viên đều có cùng năm sử dụng gần nhất. Thuật toán đạt đến điều kiện ràng buộc:```
year == best_used_year and name > best_used_name
```Từ`"beta" > "alpha"`về mặt từ điển, câu trả lời trở thành`beta`. 

Cuối cùng, hãy xem xét sự tương tác giữa tên được sử dụng và tên không được sử dụng:```
1
dream 2010
2
dream
vision
```

`dream`trở thành ứng cử viên được sử dụng tốt nhất hiện nay. Sau đó`vision`được xác định là chưa sử dụng nên nó được lưu trữ riêng trong`best_unused`. Cuối cùng, thuật toán luôn in ra một ứng cử viên chưa được sử dụng nếu có, do đó kết quả đầu ra là`vision`.
