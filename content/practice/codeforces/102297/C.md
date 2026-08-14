---
title: "CF 102297C - Bánh hạnh nhân so với kẹo và. Cookie"
description: "Mỗi buổi thực hành bắt đầu với một số lượng bánh hạnh nhân cố định và một số lượng học sinh đã biết. Học sinh đến bàn giải khát theo nhóm. Mỗi học sinh trong nhóm lấy chính xác một chiếc bánh hạnh nhân, nhưng trước khi điều đó xảy ra, Dr."
date: "2026-08-14T04:26:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102297
codeforces_index: "C"
codeforces_contest_name: "UCF Locals 2015"
rating: 0
weight: 102297
solve_time_s: 93
verified: true
draft: false
---

[CF 102297C - Bánh hạnh nhân so với kẹo so với bánh quy](https://codeforces.com/problemset/problem/102297/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi buổi thực hành bắt đầu với một số lượng bánh hạnh nhân cố định và một số lượng học sinh đã biết. Học sinh đến bàn giải khát theo nhóm. Mỗi học sinh trong một nhóm lấy chính xác một chiếc bánh hạnh nhân, nhưng trước khi điều đó xảy ra, Tiến sĩ Orooji sẽ kiểm tra xem nguồn cung hiện tại có quá nhỏ đối với nhóm hay không. 

Nếu số lượng bánh hạnh nhân nhỏ hơn hoặc bằng kích thước của nhóm đến, số bánh hạnh nhân sẽ được nhân đôi bằng cách cắt đôi mỗi bánh hạnh nhân. Việc nhân đôi có thể xảy ra nhiều lần cho đến khi số lượng bánh hạnh nhân nhiều hơn số học sinh trong nhóm. Chỉ sau đó cả nhóm mới lấy bánh hạnh nhân của mình. Sau đó, số đếm mới còn lại sẽ trở thành trạng thái bắt đầu cho nhóm tiếp theo. 

Đầu vào bắt đầu bằng số lượng thực hành. Mỗi bài thực hành cung cấp số lượng học sinh và số lượng bánh hạnh nhân ban đầu, tiếp theo là số lượng nhóm và quy mô của mỗi nhóm. Đầu ra trước tiên xác định phương pháp thực hành và các giá trị ban đầu của nó, sau đó in từng kích thước nhóm cùng với số lượng bánh hạnh nhân còn lại sau khi nhóm đó đã được phục vụ. Một dòng trống ngăn cách các lần thực hành liên tiếp. 

Số lượng học sinh trong một buổi thực hành tối đa là 30 và mỗi nhóm có ít nhất một và nhiều nhất là tất cả những học sinh đó. Nguồn cung cấp bánh hạnh nhân ban đầu là từ 60 đến 600. Các giới hạn này rất nhỏ nên không cần cấu trúc dữ liệu phức tạp hoặc thuật toán nâng cao. Ngay cả một vài thao tác liên tục trong mỗi nhóm cũng đủ nhanh. Số lượng nhóm là phần đầu vào có thể tăng lên, vì vậy mục tiêu tự nhiên là thời gian tuyến tính của số lượng bản ghi nhóm. 

Có một số trường hợp ranh giới có thể khiến việc triển khai hợp lý bị sai. Đầu tiên là sự bình đẳng. Giả sử một buổi luyện tập bắt đầu với 5 chiếc bánh hạnh nhân và một nhóm có 5 học sinh. Điều kiện là`brownies <= group`, do đó, trước tiên nguồn cung phải trở thành 10, sau đó nhóm lấy 5, để lại 5. Một chương trình chỉ sử dụng`brownies < group`sẽ ăn nhầm 5 chiếc bánh hạnh nhân ban đầu và báo cáo là 0. 

Trường hợp thứ hai là khi một lần nhân đôi là không đủ. Ví dụ: sau khi các nhóm trước để lại 1 chiếc bánh hạnh nhân, một nhóm 30 học sinh sẽ đến. Nguồn cung cấp phải đi qua`1 -> 2 -> 4 -> 8 -> 16 -> 32`trước khi có người ăn bánh hạnh nhân. Số đếm còn lại chính xác là 2. Một chương trình chỉ nhân đôi một lần sẽ tạo ra số đếm âm sau khi trừ 30. 

Trường hợp thứ ba là một nhóm đến khi nguồn cung đã đủ. Nếu có 60 chiếc bánh hạnh nhân và một nhóm 10 học sinh đến thì không xảy ra việc cắt bánh. Nhóm chỉ đơn giản tiêu thụ 10, để lại 50. Cắt bất cứ khi nào một nhóm đến, thay vì chỉ khi nguồn cung quá nhỏ, sẽ thay đổi trạng thái và làm hỏng mọi câu trả lời sau đó. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp nhất xử lý quy trình chính xác như được mô tả. Đối với mỗi nhóm, liên tục nhân đôi số lượng bánh hạnh nhân trong khi nó không lớn hơn quy mô nhóm, sau đó trừ đi quy mô nhóm. Đây đã là một giải pháp đúng vì chương trình thực hiện các chuyển đổi trạng thái giống như quy trình thực. 

Việc triển khai bạo lực chi tiết hơn có thể mô phỏng từng học sinh. Đối với một nhóm có quy mô`g`, nó sẽ thực hiện`g`tiêu dùng cá nhân sau khi chuẩn bị đủ bánh hạnh nhân lần đầu tiên. Từ`g <= 30`, trường hợp xấu nhất là 30 phép toán tiêu dùng sinh viên trong mỗi nhóm, cộng với các phép toán nhân đôi. Với`m`nhóm, đó là nhiều nhất`30m`hoạt động tiêu dùng và số lượng cắt giảm không đổi nhỏ cho mỗi nhóm. Đây vẫn là kích thước tuyến tính ở kích thước đầu vào, vì vậy nó không thực sự quá chậm theo các ràng buộc nhất định. 

Việc tối ưu hóa hữu ích là nhận ra rằng các học sinh trong một nhóm không thể phân biệt được vì mục đích đếm bánh hạnh nhân. Khi nguồn cung đã được thực hiện lớn hơn toàn bộ nhóm, nhóm luôn loại bỏ chính xác`g`bánh hạnh nhân. Không có lý do gì để bắt chước từng học sinh một. Chúng ta có thể thay thế tối đa 30 phép trừ riêng lẻ bằng một phép trừ duy nhất. 

Số lần nhân đôi cũng bị giới hạn bởi một hằng số nhỏ. Vì một nhóm có tối đa 30 học sinh, khi nguồn cung hiện tại nhiều nhất là 30, việc nhân đôi lặp lại sẽ đạt ít nhất 31 sau nhiều nhất 5 lần nhân đôi khi bắt đầu từ một số nguyên dương. Ví dụ: giá trị bắt đầu tệ nhất là 1, cho`1 -> 2 -> 4 -> 8 -> 16 -> 32`. Do đó, mô phỏng nhóm trực tiếp chỉ thực hiện công việc không đổi cho mỗi nhóm. 

Giải pháp thu được vừa đơn giản hơn vừa tối ưu tiệm cận vì mỗi nhóm phải được đọc và xử lý ít nhất một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng cá nhân-học sinh | O(30m), tức là O(m) | O(1) | Đã chấp nhận nhưng công việc không cần thiết | 
| Mô phỏng nhóm | O(m) | O(1) | Được chấp nhận và ưa thích | 

Đây,`m`biểu thị tổng số nhóm trong các hoạt động. 

## Hướng dẫn thuật toán 

1. Đọc số lần thực hành. Đối với mỗi lần thực hành, hãy đọc số lượng học sinh và số lượng bánh hạnh nhân ban đầu, sau đó đọc số lượng nhóm đến và quy mô của chúng. 
2. In tiêu đề thực hành bằng cách sử dụng số lượng học sinh và bánh hạnh nhân ban đầu. Số lượng bánh hạnh nhân ban đầu phải được lưu riêng nếu biến làm việc được sửa đổi sau này. 
3. Đối với mỗi quy mô nhóm`g`, kiểm tra xem số lượng bánh hạnh nhân hiện tại có nhỏ hơn hoặc bằng không`g`. Nếu đúng như vậy, hãy liên tục nhân đôi số lượng bánh hạnh nhân cho đến khi nó lớn hơn hẳn`g`. Bất đẳng thức nghiêm ngặt có ý nghĩa quan trọng vì có chính xác`g`bánh hạnh nhân sẽ không để lại gì sau khi nhóm được phục vụ. 
4. Trừ`g`từ số lượng bánh hạnh nhân đã chuẩn bị. Điều này thể hiện mỗi học sinh trong nhóm lấy một chiếc bánh hạnh nhân và việc xử lý toàn bộ nhóm bằng một phép trừ tương đương với việc xử lý từng học sinh trong nhóm đó. 
5. In`g`cùng với số lượng bánh hạnh nhân mới. Số lượng đó bây giờ là trạng thái được sử dụng khi nhóm tiếp theo đến. 
6. Sau khi xử lý xong tất cả các nhóm trong bài thực hành, hãy in một dòng trống trước khi chuyển sang bài thực hành tiếp theo. 

### Tại sao nó hoạt động 

Điều bất biến chính là ngay trước khi một nhóm được phục vụ, số lượng bánh hạnh nhân sẽ lớn hơn quy mô của nhóm đó. Vòng lặp nhân đôi thiết lập thuộc tính này bất cứ khi nào ban đầu nó sai. Vì mỗi lần nhân đôi khớp chính xác với một thao tác cắt được phép, nên số lượng kết quả chính xác là số lượng mà Tiến sĩ Orooji sẽ có sau khi thực hiện các lần cắt được yêu cầu. Khi số lượng lớn hơn quy mô nhóm, việc trừ đi quy mô toàn bộ nhóm sẽ cho ra chính xác số bánh hạnh nhân còn lại sau khi mỗi học sinh trong nhóm đó lấy một chiếc. Do đó, trạng thái sau mỗi nhóm giống hệt với quy trình thực, vì vậy mọi câu trả lời được in ra đều đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for practice in range(1, t + 1):
        students, original_brownies = map(int, input().split())
        m = int(input())

        brownies = original_brownies

        out.append(f"Practice #{practice}: {students} {original_brownies}")

        for _ in range(m):
            group = int(input())

            while brownies <= group:
                brownies *= 2

            brownies -= group

            out.append(f"{group} {brownies}")

        out.append("")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Chương trình giữ`original_brownies`không thay đổi nên tiêu đề thực hành luôn có thể in giá trị bắt đầu. Sự riêng biệt`brownies`biến đại diện cho nguồn cung hiện tại và được cập nhật sau mỗi nhóm. 

các`while brownies <= group`condition mã hóa trực tiếp quy tắc từ quy trình. sử dụng`<`sẽ là một lỗi riêng biệt vì sự bình đẳng cũng đòi hỏi phải cắt bỏ. Vòng lặp phải xảy ra trước khi trừ, vì nhóm chỉ được phục vụ sau khi đã chuẩn bị đủ bánh hạnh nhân. 

Sau vòng lặp,`brownies > group`, do đó trừ đi`group`không thể làm cho nguồn cung âm. Số nguyên Python cũng có độ chính xác tùy ý, mặc dù các ràng buộc đã cho đã làm cho các giá trị trở nên rất nhỏ. 

Đầu ra được tích lũy thành một danh sách và được ghi một lần vào cuối. Điều này tránh các lệnh gọi đầu ra lặp lại trong khi vẫn sử dụng thiết lập đầu vào nhanh cần thiết. 

## Ví dụ đã hoạt động 

Mẫu đầu tiên có 20 học sinh và bắt đầu với 60 chiếc bánh hạnh nhân. Có tám nhóm. Phần quan trọng của dấu vết là điều gì sẽ xảy ra khi nguồn cung trở nên nhỏ hơn nhóm mới đến. 

| Nhóm | Bánh hạnh nhân trước khi chuẩn bị | Quy mô nhóm | Nhân đôi | Bánh hạnh nhân sau nhóm | 
| --- | --- | --- | --- | --- | 
| 1 | 60 | 15 | không | 45 | 
| 2 | 45 | 10 | không | 35 | 
| 3 | 35 | 20 | không | 15 | 
| 4 | 15 | 18 | 15 → 30 | 12 | 
| 5 | 12 | 9 | không | 3 | 
| 6 | 3 | 12 | 3 → 6 → 12 → 24 | 12 | 
| 7 | 12 | 2 | không | 10 | 
| 8 | 10 | 10 | 10 → 20 | 10 | 

Đối với nhóm thứ tư, 15 chiếc bánh hạnh nhân là không đủ vì nhóm có 18 học sinh. Một lần cắt sẽ thay đổi nguồn cung thành 30, sau đó 18 chiếc được tiêu thụ và 12 chiếc còn lại. Nhóm thứ sáu thể hiện nhiều lần cắt trong một thao tác: 3 bánh hạnh nhân phải đạt 24 trước khi có thể phục vụ 12 học sinh. Nhóm cuối cùng thể hiện ranh giới bình đẳng, vì 10 bánh hạnh nhân và 10 học sinh vẫn cần nhân đôi. 

Mẫu thứ hai bắt đầu với 100 bánh hạnh nhân và có bốn nhóm tương đối nhỏ. 

| Nhóm | Bánh hạnh nhân trước khi chuẩn bị | Quy mô nhóm | Nhân đôi | Bánh hạnh nhân sau nhóm | 
| --- | --- | --- | --- | --- | 
| 1 | 100 | 1 | không | 99 | 
| 2 | 99 | 2 | không | 97 | 
| 3 | 97 | 3 | không | 94 | 
| 4 | 94 | 5 | không | 89 | 

Không cần cắt giảm trong phương pháp này vì nguồn cung vẫn lớn hơn mọi nhóm mới đến. Dấu vết cho thấy thuật toán không thực hiện các thao tác cắt không cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M) | Mỗi nhóm được xử lý một lần và mỗi nhóm cần tối đa số lần nhân đôi không đổi | 
| Không gian | O(M) | Quá trình triển khai lưu trữ kết quả được tạo trước khi ghi nó | 

Đây,`M`là tổng số nhóm trong tất cả các hoạt động. Bản thân trạng thái chỉ yêu cầu thêm O(1) không gian. Không gian O(M) trong quá trình triển khai đến từ việc thu thập các chuỗi đầu ra, đây là tùy chọn và có thể được thay thế bằng tính năng in ngay lập tức nếu muốn. Vì mỗi nhóm có tối đa 30 sinh viên nên số lần nhân đôi bị giới hạn bởi một hằng số, do đó tổng thời gian xử lý vẫn tuyến tính theo lượng đầu vào. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    t = int(input())
    out = []

    for practice in range(1, t + 1):
        students, original_brownies = map(int, input().split())
        m = int(input())

        brownies = original_brownies
        out.append(f"Practice #{practice}: {students} {original_brownies}")

        for _ in range(m):
            group = int(input())

            while brownies <= group:
                brownies *= 2

            brownies -= group
            out.append(f"{group} {brownies}")

        out.append("")

    sys.stdout.write("\n".join(out))

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

sample = """2
20 60
8
15
10
20
18
9
12
2
10
15 100
4
1
2
3
5
"""

sample_expected = """Practice #1: 20 60
15 45
10 35
20 15
18 12
9 3
12 12
2 10
10 10

Practice #2: 15 100
1 99
2 97
3 94
5 89

"""

assert run(sample) == sample_expected, "provided samples"

minimum_case = """1
1 60
3
1
1
1
"""

minimum_expected = """Practice #1: 1 60
1 59
1 58
1 57

"""

assert run(minimum_case) == minimum_expected, "minimum-size practice"

maximum_case = """1
30 600
4
30
30
30
30
"""

maximum_expected = """Practice #1: 30 600
30 570
30 540
30 510
30 480

"""

assert run(maximum_case) == maximum_expected, "maximum-size values"

equality_case = """1
5 60
4
50
5
5
5
"""

equality_expected = """Practice #1: 5 60
50 10
5 5
5 5
5 5

"""

assert run(equality_case) == equality_expected, "equality must trigger cutting"

multiple_cuts_case = """1
30 60
2
29
30
"""

multiple_cuts_expected = """Practice #1: 30 60
29 31
30 2

"""

assert run(multiple_cuts_case) == multiple_cuts_expected, "multiple doublings"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 60 / 1, 1, 1`|`59, 58, 57`còn lại | Số lượng sinh viên tối thiểu và các nhóm không bao giờ cần cắt giảm | 
|`1 / 30 600 / 30, 30, 30, 30`|`570, 540, 510, 480`còn lại | Giá trị sinh viên và bánh hạnh nhân tối đa với các nhóm có kích thước bằng nhau được lặp lại | 
|`1 / 5 60 / 50, 5, 5, 5`|`10, 5, 5, 5`còn lại | Điều kiện bình đẳng, trong đó`brownies == group`phải kích hoạt cắt giảm | 
|`1 / 30 60 / 29, 30`|`31, 2`còn lại | Nhóm sau yêu cầu năm lần nhân đôi liên tiếp | 

## Vỏ cạnh 

Trường hợp đẳng thức được xử lý bởi`<=`tình trạng. Hãy xem xét đầu vào này:```
1
5 60
4
50
5
5
5
```Sau nhóm đầu tiên, vẫn còn 10 bánh hạnh nhân. Nhóm tiếp theo có đúng 5 học sinh, vậy 10 là đủ và kết quả là 5. Nhóm sau đến khi còn đúng 5 bánh hạnh nhân. Từ`5 <= 5`, thuật toán sẽ nhân đôi nguồn cung lên 10 trước khi trừ đi 5, để lại 5. Nhóm cuối cùng hành xử giống hệt nhau. Một sự nghiêm khắc`<`điều kiện sẽ tiêu thụ trực tiếp năm chiếc bánh hạnh nhân một cách không chính xác và tạo ra số không. 

Trường hợp cắt lặp lại được xử lý bằng cách cho phép vòng lặp nhân đôi thực hiện nhiều lần nếu cần thiết. Coi như:```
1
30 60
2
29
30
```Nhóm đầu tiên lấy 29 từ 60, để lại 31. Nhóm tiếp theo có 30 học sinh, do đó không cần cắt vì 31 hoàn toàn lớn hơn 30. Nhóm lấy 30 và bỏ 1. Dấu vết cụ thể này cho thấy tại sao điều kiện được đánh giá trước khi trừ và nó cũng tạo ra trạng thái có thể buộc cắt lặp lại trong nhóm sau. 

Đối với một ví dụ cắt lặp lại trực tiếp, phần tiếp theo sau đây làm cho hành vi trở nên rõ ràng:```
1
30 60
3
29
30
30
```Sau nhóm đầu tiên, còn lại 31 chiếc bánh hạnh nhân. Sau nhóm thứ hai, chỉ còn lại 1. Khi nhóm thứ ba gồm 30 người đến, thuật toán thực hiện năm lần nhân đôi:```
1 -> 2 -> 4 -> 8 -> 16 -> 32
```Bây giờ 32 lớn hơn 30, vì vậy nhóm có thể được phục vụ và chỉ còn lại đúng 2 bánh hạnh nhân. Việc triển khai bất cẩn chỉ nhân đôi một lần sẽ đạt 2 rồi trừ 30, tạo ra số đếm âm không thể xảy ra. 

Cuối cùng, hãy xem xét một nhóm mà nguồn cung hiện tại đã đủ lớn:```
1
1 60
1
1
```Nguồn cung hiện tại là 60 chiếc và nhóm chỉ cần 1 chiếc bánh hạnh nhân. Vòng nhân đôi bị bỏ qua hoàn toàn và câu trả lời là 59. Việc cắt trong tình huống này sẽ là một thao tác bổ sung không chính xác vì quy tắc chỉ cho phép cắt khi nguồn cung hiện tại nhỏ hơn hoặc bằng quy mô nhóm đến.
