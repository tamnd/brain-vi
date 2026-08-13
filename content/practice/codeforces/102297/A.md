---
title: "CF 102297A - Tìm cặp song sinh"
description: "Mỗi trường hợp thử nghiệm mô tả số áo của chính xác 10 cầu thủ bóng đá. Mack luôn mặc áo số 18, trong khi Zack luôn mặc áo số 17. Với mỗi bộ 10 số, chúng ta phải xác định xem bộ đó có chứa Mack, Zack, cả hai hay không."
date: "2026-08-13T08:22:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102297
codeforces_index: "A"
codeforces_contest_name: "UCF Locals 2015"
rating: 0
weight: 102297
solve_time_s: 135
verified: true
draft: false
---

[CF 102297A - Tìm cặp song sinh](https://codeforces.com/problemset/problem/102297/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm mô tả số áo của chính xác 10 cầu thủ bóng đá. Mack luôn mặc áo số 18, trong khi Zack luôn mặc áo số 17. Với mỗi bộ 10 số, chúng ta phải xác định xem bộ đó có chứa Mack, Zack, cả hai hay không. 

Đầu ra có hai phần cho mỗi trường hợp thử nghiệm. Đầu tiên, chúng tôi in mười số áo đấu chính xác như chúng được đưa ra. Sau đó chúng tôi in`mack`nếu chỉ có 18 xuất hiện,`zack`nếu chỉ có 17 xuất hiện,`both`nếu cả hai đều xuất hiện và`none`nếu không xuất hiện. Một dòng trống phân tách kết quả của từng trường hợp thử nghiệm. 

Đầu vào chứa một số dương`n`theo sau là`n`các tập hợp, mỗi tập hợp chứa đúng 10 số nguyên phân biệt. Mỗi số áo đấu nằm trong khoảng từ 11 đến 99. Kích thước cố định của mỗi bộ là hạn chế chính ở đây. Kể cả nếu`n`lớn như`10^5`, việc xử lý một bộ chỉ yêu cầu một lượng công việc không đổi, do đó, giải pháp O(n) thực hiện khoảng vài triệu thao tác đơn giản và dễ dàng phù hợp với giới hạn thời gian thi đấu thông thường. Không cần sắp xếp, băm hoặc bất kỳ cấu trúc dữ liệu phức tạp nào hơn. 

Có một số trường hợp logic bất cẩn có thể đưa ra sự phân loại sai. Nếu bộ chỉ chứa 17, chẳng hạn như`11 12 13 14 15 16 17 19 20 21`, câu trả lời là`zack`, không`both`, vì 18 vắng mặt. Nếu bộ chỉ chứa 18, chẳng hạn như`11 12 13 14 15 16 18 19 20 21`, câu trả lời là`mack`. Nếu cả hai giá trị xảy ra, chẳng hạn như`11 12 13 14 15 16 17 18 20 21`, câu trả lời là`both`. Cuối cùng, nếu không xảy ra, chẳng hạn như`11 12 13 14 15 16 19 20 21 22`, câu trả lời là`none`. 

Câu lệnh đảm bảo rằng mười số là khác nhau, do đó, đầu vào hoàn toàn bằng nhau sẽ không hợp lệ theo các ràng buộc chính thức. Việc triển khai mạnh mẽ vẫn có thể xử lý nó một cách tự nhiên. Ví dụ,`17 17 17 17 17 17 17 17 17 17`sẽ được phân loại là`zack`, vì chỉ có sự hiện diện của 17 và 18 mới quan trọng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là tìm kiếm trong mười số cho 17 và tìm kiếm lại chúng cho 18. Tìm kiếm đầu tiên trả lời liệu Zack có mặt hay không và tìm kiếm thứ hai trả lời liệu Mack có mặt hay không. Vì mỗi bộ chứa chính xác 10 số, trường hợp xấu nhất thực hiện 10 phép so sánh để tìm kiếm 17 và 10 phép so sánh khác để tìm kiếm 18, cho tối đa 20 phép so sánh thành viên cho mỗi trường hợp thử nghiệm. Với`n`trường hợp thử nghiệm, đó là nhiều nhất`20n`so sánh, đó là O (n). Nó đã đủ nhanh vì kích thước cài đặt không bao giờ tăng theo`n`. 

Chúng ta có thể làm cho quá trình quét sạch hơn một chút bằng cách chỉ xem mỗi số một lần và duy trì hai cờ Boolean. Bất cứ khi nào một số là 17, chúng tôi đánh dấu Zack là đã tìm thấy. Bất cứ khi nào một số là 18, chúng tôi đánh dấu Mack là đã tìm thấy. Sau khi đọc hết mười số, hai lá cờ hoàn toàn xác định được câu trả lời. 

Quan sát quan trọng là giá trị thực tế của tám số còn lại là không liên quan. Chúng ta không cần sắp xếp các con số hay đếm bất cứ thứ gì ngoài việc 17 và 18 có xuất hiện hay không. Một lần vượt qua sẽ cung cấp cho chúng tôi chính xác hai dữ kiện cần thiết cho việc phân loại cuối cùng. 

Không giống như nhiều vấn đề trong đó cách tiếp cận bạo lực trở nên quá chậm khi đầu vào tăng lên, không có khoảng cách tiệm cận có ý nghĩa nào ở đây vì mọi trường hợp thử nghiệm luôn chứa chính xác mười số. Phiên bản tìm kiếm riêng biệt đã được chấp nhận. Phiên bản một lần được ưa chuộng hơn vì nó thể hiện trực tiếp cấu trúc của vấn đề và sử dụng tối đa mười lần kiểm tra cho mỗi trường hợp kiểm thử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) phụ trợ | Đã chấp nhận | 
| Một Lần | O(n) | O(1) phụ trợ | Đã chấp nhận | 

Đây`n`là số lượng ca kiểm thử. Hằng số ẩn khác nhau giữa hai cách tiếp cận, nhưng cả hai đều tuyến tính vì mỗi trường hợp thử nghiệm đều chứa chính xác 10 giá trị. 

## Hướng dẫn thuật toán 

1. Đọc số`n`của các trường hợp thử nghiệm. Chúng tôi cần giá trị này để xử lý chính xác số lượng bộ mười số cần thiết. 
2. Với mỗi test, đọc 10 số jersey của nó vào một danh sách. Việc giữ lại danh sách cho phép chúng tôi in bộ gốc sau khi xử lý nó. 
3. Khởi tạo hai biến Boolean,`has_mack`Và`has_zack`, ĐẾN`False`. Tại thời điểm này, chúng tôi đã kiểm tra không có số áo thi đấu nên không tìm thấy cặp song sinh nào. 
4. Quét tất cả mười số một lần. Nếu số hiện tại là 18, hãy đặt`has_mack`ĐẾN`True`. Nếu là 17, đặt`has_zack`ĐẾN`True`. Các giá trị khác không cần thực hiện hành động nào vì chúng không thể thay đổi câu trả lời. 
5. In mười số theo thứ tự ban đầu. Vấn đề rõ ràng yêu cầu chính bộ đầu vào xuất hiện trong đầu ra, do đó việc sắp xếp hoặc sửa đổi danh sách sẽ không cần thiết và có thể không chính xác. 
6. Sử dụng hai lá cờ để chọn kết quả. Nếu cả hai đều đúng, hãy in`both`. Nếu chỉ`has_mack`là đúng, in`mack`. Nếu chỉ`has_zack`là đúng, in`zack`. Nếu cả hai đều sai, hãy in`none`. 
7. In một dòng trống sau kết quả của test case. Điều này phù hợp với định dạng đầu ra được yêu cầu. 

### Tại sao nó hoạt động 

Sau khi xử lý bất kỳ tiền tố nào của mười số,`has_mack`đúng chính xác khi 18 xuất hiện trong tiền tố đó và`has_zack`đúng khi số 17 xuất hiện. Thuộc tính này được giữ nguyên bất cứ khi nào một số khác được xử lý vì chỉ 17 có thể thay đổi cờ Zack và chỉ 18 có thể thay đổi cờ Mack. Sau khi tất cả mười số đã được kiểm tra, hai lá cờ mô tả chính xác cặp song sinh nào có mặt. Bốn sự kết hợp có thể có của các cờ tương ứng một-một với`none`,`mack`,`zack`, Và`both`, nên kết quả báo cáo không thể sai được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    for _ in range(n):
        nums = list(map(int, input().split()))

        has_mack = False
        has_zack = False

        for x in nums:
            if x == 18:
                has_mack = True
            elif x == 17:
                has_zack = True

        print(*nums)

        if has_mack and has_zack:
            print("both")
        elif has_mack:
            print("mack")
        elif has_zack:
            print("zack")
        else:
            print("none")

        print()

if __name__ == "__main__":
    solve()
```Dòng đầu tiên của`solve`đọc số lượng tập dữ liệu. Sau đó, vòng lặp sẽ xử lý một bộ hoàn chỉnh tại một thời điểm, khớp với cấu trúc của đầu vào.`nums`lưu trữ chính xác mười giá trị cần thiết cho cả việc phát hiện và tái tạo tập hợp đầu vào ở đầu ra.`print(*nums)`in chúng cách nhau bằng dấu cách mà không thay đổi thứ tự của chúng. 

Hai lá cờ bắt đầu như`False`. Trong quá trình quét gặp 18 bộ`has_mack`, khi gặp 17 bộ`has_zack`. các`elif`là an toàn vì một số không thể đồng thời bằng 17 và 18. Các cờ không bao giờ được đặt lại trong quá trình quét, vì vậy khi tìm thấy một cặp song sinh, thông tin đó sẽ được giữ lại. 

Điều kiện cuối cùng kiểm tra sự kết hợp của các cờ từ cụ thể nhất đến ít cụ thể nhất. Cả hai đều đúng trước tiên phải được kiểm tra, nếu không trường hợp có cả hai cặp song sinh sẽ bị phân loại sai thành chỉ Mack hoặc chỉ Zack. Ba trường hợp còn lại tiếp theo trực tiếp. 

Không có ranh giới lập chỉ mục để quản lý vì mã tự lặp lại các giá trị. Không thể tràn số nguyên vì mỗi số đầu vào nằm trong khoảng từ 11 đến 99. Kết quả cuối cùng`print()`tạo ra dòng trống cần thiết sau mỗi tập dữ liệu. 

Mẫu chính thức chứa bốn bộ dữ liệu, với dòng đầu tiên`4`chỉ định số lượng của họ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Tập dữ liệu đầu tiên là:`11 99 88 17 19 20 12 13 33 44`Thuật toán bắt đầu với cả hai cờ sai. Khi đạt tới 17, nó đánh dấu Zack có mặt. Không có giá trị nào còn lại là 18. 

| Vị trí | Số áo | has_mack | has_zack | 
| --- | --- | --- | --- | 
| 1 | 11 | Sai | Sai | 
| 2 | 99 | Sai | Sai | 
| 3 | 88 | Sai | Sai | 
| 4 | 17 | Sai | Đúng | 
| 5 | 19 | Sai | Đúng | 
| 6 | 20 | Sai | Đúng | 
| 7 | 12 | Sai | Đúng | 
| 8 | 13 | Sai | Đúng | 
| 9 | 33 | Sai | Đúng | 
| 10 | 44 | Sai | Đúng | 

Cuối cùng,`has_mack`là sai và`has_zack`là đúng, vậy câu trả lời là`zack`. 

### Mẫu 2 

Tập dữ liệu thứ hai là:`11 12 13 14 15 16 66 88 19 20`Cả 17 và 18 đều không xảy ra. Cả hai cờ vẫn sai trong suốt quá trình quét. 

| Vị trí | Số áo | has_mack | has_zack | 
| --- | --- | --- | --- | 
| 1 | 11 | Sai | Sai | 
| 2 | 12 | Sai | Sai | 
| 3 | 13 | Sai | Sai | 
| 4 | 14 | Sai | Sai | 
| 5 | 15 | Sai | Sai | 
| 6 | 16 | Sai | Sai | 
| 7 | 66 | Sai | Sai | 
| 8 | 88 | Sai | Sai | 
| 9 | 19 | Sai | Sai | 
| 10 | 20 | Sai | Sai | 

Cả hai cờ đều sai nên thuật toán in ra`none`. Dấu vết này chứng tỏ rằng các giá trị gần với số mục tiêu, chẳng hạn như 16, 19 và 20, không được tính. Sự so sánh phải chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi trường hợp thử nghiệm chứa chính xác 10 số, do đó chi phí xử lý O(10) = O(1) cho mỗi trường hợp. | 
| Không gian | O(1) phụ trợ | Chỉ cần hai cờ Boolean ngoài bộ đầu vào mười số. | 

Công việc thực tế cho mỗi trường hợp kiểm thử là rất nhỏ vì quá trình quét luôn xử lý chính xác mười số nguyên. Ngay cả đối với một số lượng rất lớn các tập dữ liệu, tổng công việc vẫn tăng tuyến tính với`n`. Bộ nhớ được sử dụng cho thuật toán không tăng theo số lượng trường hợp thử nghiệm. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây sử dụng một lớp bao bọc nhỏ xung quanh cùng một`solve`logic. Trường hợp hoàn toàn bằng nhau nằm ngoài ràng buộc số phân biệt chính thức, nhưng nó rất hữu ích như một bài kiểm tra độ tin cậy. Trường hợp lớn kiểm tra xem việc xử lý nhiều tập dữ liệu có còn tuyến tính hay không.```python
import sys
import io

def solve():
    input = sys.stdin.readline
    n = int(input())

    for _ in range(n):
        nums = list(map(int, input().split()))

        has_mack = False
        has_zack = False

        for x in nums:
            if x == 18:
                has_mack = True
            elif x == 17:
                has_zack = True

        print(*nums)

        if has_mack and has_zack:
            print("both")
        elif has_mack:
            print("mack")
        elif has_zack:
            print("zack")
        else:
            print("none")

        print()

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

sample_input = """4
11 99 88 17 19 20 12 13 33 44
11 12 13 14 15 16 66 88 19 20
20 18 55 66 77 88 17 33 44 11
12 23 34 45 56 67 78 89 91 18
"""

sample_output = """11 99 88 17 19 20 12 13 33 44
zack

11 12 13 14 15 16 66 88 19 20
none

20 18 55 66 77 88 17 33 44 11
both

12 23 34 45 56 67 78 89 91 18
mack

"""

assert run(sample_input) == sample_output, "official sample"

assert run(
    """1
11 12 13 14 15 16 17 19 20 21
"""
) == """11 12 13 14 15 16 17 19 20 21
zack

""", "zack only"

assert run(
    """1
11 12 13 14 15 16 18 19 20 21
"""
) == """11 12 13 14 15 16 18 19 20 21
mack

""", "mack only"

assert run(
    """1
11 12 13 14 15 16 17 18 20 21
"""
) == """11 12 13 14 15 16 17 18 20 21
both

""", "both twins"

assert run(
    """1
11 12 13 14 15 16 19 20 21 22
"""
) == """11 12 13 14 15 16 19 20 21 22
none

""", "neither twin"

assert run(
    """1
17 17 17 17 17 17 17 17 17 17
"""
) == """17 17 17 17 17 17 17 17 17 17
zack

""", "all equal values"

assert run(
    """1
11 12 13 14 15 16 17 18 98 99
"""
) == """11 12 13 14 15 16 17 18 98 99
both

""", "boundary values and both twins"

large_input = "100000\n" + (
    "11 12 13 14 15 16 19 20 21 99\n" * 100000
)
large_output = (
    "11 12 13 14 15 16 19 20 21 99\nnone\n\n" * 100000
)
assert run(large_input) == large_output, "large number of test cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`theo sau là một bộ chứa 17 nhưng không phải 18 |`zack`| Phân loại chỉ dành cho Zack | 
|`1`theo sau là một bộ chứa 18 nhưng không phải 17 |`mack`| Phân loại chỉ dành cho Mack | 
|`1`theo sau là một bộ chứa cả 17 và 18 |`both`| Cả hai lá cờ đều đúng | 
|`1`theo sau là một tập hợp không chứa |`none`| Cả hai cờ còn lại sai | 
| Mười bản 17 |`zack`| Mạnh mẽ ngay cả khi vi phạm đảm bảo về tính khác biệt | 
| Một bộ gồm 11, 99, 17 và 18 |`both`| Giá trị được phép thấp nhất và cao nhất cộng với cả hai giá trị mục tiêu | 
| 100000 bộ dữ liệu giống hệt nhau | 100000 kết quả đầu ra tương ứng | Hành vi tuyến tính trong số lượng trường hợp thử nghiệm | 

## Vỏ cạnh 

Một tập dữ liệu chỉ chứa Zack là`11 12 13 14 15 16 17 19 20 21`. Trong quá trình quét,`has_zack`trở thành đúng khi gặp 17, trong khi`has_mack`không bao giờ thay đổi. Trạng thái cuối cùng là`(False, True)`, ánh xạ tới`zack`. Việc triển khai bất cẩn chỉ kiểm tra xem một trong các số mục tiêu có tồn tại hay không và in ngay lập tức`both`sẽ thất bại ở đây. 

Một tập dữ liệu chỉ chứa Mack là`11 12 13 14 15 16 18 19 20 21`. Quá trình quét chỉ thay đổi`has_mack`, sản xuất`(True, False)`. Kết quả đúng là`mack`. Trường hợp này phát hiện mã vô tình hoán đổi ý nghĩa của 17 và 18, vì 17 thuộc về Zack và 18 thuộc về Mack. 

Một tập dữ liệu chứa cả hai cặp song sinh là`11 12 13 14 15 16 17 18 20 21`. Bộ quét đầu tiên`has_zack`khi nó nhìn thấy 17 và sau đó đặt`has_mack`khi nó nhìn thấy 18. Trạng thái cuối cùng là`(True, True)`, vậy kết quả là`both`. Việc kiểm tra trường hợp kết hợp trước các trường hợp riêng lẻ chỉ ngăn mã in sai`mack`hoặc chỉ`zack`. 

Một tập dữ liệu không chứa cặp song sinh nào là`11 12 13 14 15 16 19 20 21 22`. Không có sự so sánh nào thành công nên cả hai cờ vẫn sai và kết quả là`none`. Đây cũng là lý do tại sao các giá trị lân cận như 16 và 19 không thể được coi là kết quả khớp gần đúng. 

Giá trị biên 11 và 99 không có hành vi đặc biệt nào. Ví dụ,`11 12 13 14 15 16 17 18 98 99`chứa cả hai số mục tiêu, vì vậy nó tạo ra`both`. Thuật toán so sánh trực tiếp các giá trị và không bao giờ dựa vào việc chúng nằm trong phạm vi hẹp hơn. 

Cuối cùng, mười số được đảm bảo là khác biệt, nhưng ngay cả một tập dữ liệu hoàn toàn bằng nhau không hợp lệ cũng không phá vỡ thuật toán. Với`17 17 17 17 17 17 17 17 17 17`, mọi so sánh với 17 đều thành công và cờ Zack trở thành đúng, trong khi cờ Mack vẫn sai. Đầu ra vẫn được phân loại chính xác là`zack`.
